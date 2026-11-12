# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 14:05** · publicado em **12/11/2026 14:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 14:05 · sessao 85db53f6de83 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**O v3 ainda não fecha o contrato.** Corrige a distinção entre universo e andabilidade, mas mantém problemas no RED, nos selos, na validação e na semente. (`contrato_item3_v3.txt:1`, `:3`, `:7`, `:8`; `contrato_item3_v3_semente.txt:2`, `:21`)

Nas referências aos **anexos fornecidos**, contei cada linha física, começando pelo título — ou `[ORDEM]`, na semente — como linha 1. Pousos abaixo são declarações documentais; confirmação em produção: **[nao verificado]**.

**1. Os seis furos do v2**

| Furo | Veredito sobre o v3 |
|---|---|
| **Universo × andável / RED ca320b69f** | **Semântica corrigida; promessa do RED não fechada.** Pertencimento passa a incluir todos de `[ORDEM]`, inclusive travados; restantes andáveis são somente os posteriores ao primeiro sem `TRAVA`. Isso permite distinguir os dois casos exigidos. Não demonstra que a fixture antiga passe sem edição. (`contrato_item3_v3.txt:3`, `:8`; `bin/hook_stop_fila1.py:1243`, `:1245`) |
| **Leitores não migrados e selos** | **Parcial.** A cláusula “TODOS os leitores do censo v2” amplia a cobertura, mas a enumeração omite leitores indiretos e há propriedades antigas incompatíveis com a nova autoridade. Migrar apenas fixtures não resolve esses conflitos. (`contrato_item3_v3.txt:5`, `:8`; `bin/interrompidas_estado.py:93`; `bin/tests/test_hook_estado_e_a_coluna.sh:53`; `bin/tests/test_censo_antes_da_vaga.sh:544`) |
| **Janela/cauda do publicador** | **Fecha a obrigação de preservar; não fecha completamente a divisão entre preservar e regenerar.** A “cauda após OBRAS:FIM” contém também tabelas geradas. Preservá-la inteira e acrescentar novamente o corpo gerado duplicaria conteúdo. O contrato precisa delimitar a substituição dessa parte, além da preservação manual. (`contrato_item3_v3.txt:6`; `bin/gerar_backlog.py:100`, `:127`; `app/docs/BACKLOG.md:2042`, `:2044`, `:2369`, `:2371`) |
| **942 × len−1** | **Corrige a unidade da comparação**, adotando “andáveis além do em curso” nos dois lados. **O 941 não corresponde à cópia entregue:** recontagem em memória pelas funções atuais deu **945 totais e 944 além do topo**. O valor de um HEAD externo continua **[nao verificado]**. (`contrato_item3_v3.txt:8`; `bin/hook_stop_fila1.py:231`, `:270`; `app/docs/BACKLOG.md:712`, `:2042`) |
| **Semente ambígua** | **Não fechado.** Há identidades sem linha válida em OBRAS, exclusão indevida de delta e contradição de precedência. A seção 5 encaminha a conferência; não resolve os fatos apresentados. (`contrato_item3_v3.txt:9`; `contrato_item3_v3_semente.txt:2`, `:10`, `:11`, `:21`; `app/docs/BACKLOG.md:1916`, `:1969`, `:2404`, `:2417`) |
| **Vazio/repetição/gramática/falha aberta** | **Parcial.** Vazio válido, repetição intra/entre seções, seções e gramática inválidas passam a ter regra. Restam ilegibilidade/decodificação e falhas da leitura do BACKLOG sem conversão explicitada em `ErroFonte`; a lista de erros também exige linha em OBRAS somente para `[ORDEM]`, embora a identidade da seção 0 valha para todas as seções. (`contrato_item3_v3.txt:2`, `:7`, `:8`; `bin/hook_stop_fila1.py:1460`, `:1461`) |

Há mais dois pontos concretos nessa especificação:

- **Topo com TRAVA:** permitido pelo v3. Se `_ids_que_nao_andam()` virar mecanicamente “ids com TRAVA”, o veto atual rejeitará o PAREI do próprio topo antes de compará-lo com o em curso. A migração precisa mudar esse veto; o selo A/B/C proposto deixa A sem TRAVA e não cobre esse caso. (`contrato_item3_v3.txt:3`, `:5`, `:8`; `bin/hook_stop_fila1.py:1298`, `:1302`, `:1310`)
- **Gramática das travas:** a seção 0 nomeia `aval`, `!`, `lei`, `desenho`, `portao`, mas a semente usa `E3-#4-ATRAS-DA-QUARENTENA` e `PERGUNTA-DE-DESENHO-S1`. Falta dizer se aquela enumeração é fechada ou se admite motivo livre; na leitura fechada, a própria semente é inválida. (`contrato_item3_v3.txt:2`; `contrato_item3_v3_semente.txt:3`, `:5`)

**2. O RED fica verde sem edição?** [nao verificado]

**Não há suporte para essa promessa.** O arquivo `bin/tests/test_hook_andavel_e_a_ordem_viva.sh` não está disponível nesta cópia: seu conteúdo literal e as mudanças exatas nas linhas 51/73 são **[nao verificado]**. O RELATO registra ca320b69f na outra raia `wt-travas-105806`. (`app/docs/RELATO.md:37`)

Para a fixture descrita no anexo — que escreve marcador/prosa/coluna — a semântica nova exige estas mudanças:

| Cenário informado | Mudança necessária na fixture |
|---|---|
| **:51 — restantes travados** | Fornecer o `.ids` da árvore temporária, com o mesmo topo e os restantes em `[ORDEM]` com `TRAVA`; ruído aberto fica fora de `[ORDEM]`, POS-20 em sua seção. Manter a exigência de PAREI do topo liberar. (`contrato_item3_v3.txt:2`, `:3`, `:8`; conteúdo literal do RED: **[nao verificado]**) |
| **:73 — restante aberto** | Alterar o **`.ids`**, retirando a TRAVA do restante que deve andar; alterar somente a coluna não pode mudar a resposta. Manter a exigência de bloquear com um restante andável. (`contrato_item3_v3.txt:3`, `:4`, `:8`; conteúdo literal do RED: **[nao verificado]**) |

Também é necessário dirigir o leitor ao `.ids` da fixture, junto do BACKLOG correspondente; usar a fonte real não representaria os cenários temporários. O selo vizinho verificável hoje substitui apenas `BACKLOG`/`RELATO`, escreve a tabela e alterna a coluna. (`bin/tests/test_hook_parei_com_andavel.sh:22`, `:25`, `:41`, `:56`)

**Isso mantém a propriedade do RED, mas edita sua fixture.** Portanto é compatível com “o RED fica” entendido como conservar os casos e as asserções; é incompatível com **“SEM ser editado”**. O próprio v3 manda migrar fixtures na linha 8 e proíbe editar na linha 1. (`contrato_item3_v3.txt:1`, `:8`)

**3. Censo: omissões e perguntas diferentes**

A lista **nominal** da seção 1 deixa estes sítios de fora; a cláusula geral “TODOS” os abrange, mas precisa alcançar as interfaces e os textos indicados. Vários já delegam, portanto a omissão nominal não prova que sobreviverá um parser próprio. (`contrato_item3_v3.txt:5`; `bin/interrompidas_estado.py:93`; `bin/handoff_sessao.sh:64`)

| Omissão nominal | O que precisa acompanhar a troca |
|---|---|
| `interrompidas_estado.py` | Topo consultado ao hook; preservar separadamente a leitura documental de interrupção/fusão/fechamento. (`bin/interrompidas_estado.py:54`, `:93`, `:105`, `:125`) |
| Primeira parte do handoff | Topo, em curso e PAREI; também o rótulo “MARCADOR”. (`bin/handoff_sessao.sh:64`, `:66`, `:71`, `:120`, `:123`) |
| Wrapper e placar | Delegação do censo e do alarme. (`bin/codex_pergunta.sh:247`; `bin/placar_code.sh:368`) |
| Chamadores do fabricante | Delegação ao seletor. (`bin/fabricante.sh:129`; `bin/fabricante_alvo.py:315`, `:320`) |
| Publicação e entradas | Transporte dos documentos e chamadas dos instrumentos. (`bin/relato.sh:115`, `:116`; `.claude/settings.json:9`, `:20`, `:29`, `:40`, `:53`, `:68`) |

`gerar_backlog.py` está fora da seção 1, mas coberto pela seção 2. As declarações documentais da autoridade antiga continuam sem emenda explicitamente listada no v3. (`contrato_item3_v3.txt:5`, `:6`; `app/docs/LEIS.md:140`; `app/docs/CORTES.json:736`; `app/docs/BACKLOG.md:1576`)

Quanto a **OUTRA pergunta**:

| Leitor | Fronteira correta |
|---|---|
| Handoff: avais de hoje e raia20 | Pergunta fechamento documental, não pertencimento à ORDEM. (`bin/handoff_sessao.sh:171`, `:209`, `:216`) |
| Interrompidas | Pergunta interrupção, fusão, fechamento e retorno; somente o topo vem da ORDEM. (`bin/interrompidas_estado.py:105`, `:116`, `:121`, `:125`) |
| `gerar_backlog_topo.py` | Resume O-números abertos e ordena numericamente; não representa ORDEM VIVA. (`bin/gerar_backlog_topo.py:19`, `:26`, `:39`) |
| Fabricante | Pergunta elegibilidade para sorteio. A exceção “livres/O-F fora da fila 1” precisa excluir efetivamente os membros de `[ORDEM]`; hoje o ramo “livre” também aplica veto pela coluna. (`contrato_item3_v3.txt:5`; `bin/fabricante_alvo.py:256`, `:273`, `:275`, `:282`) |
| Testes de existência/padrão | Podem continuar documentais. (`app/core/tests/test_marca_cron15.py:89`; `app/chamados/tests/e4b_c4_verificar.py:209`; `bin/tests/test_prompt_virou_item.sh:25`) |

**Há conflitos que não se resolvem apenas mudando fixture:**

- `test_hook_estado_e_a_coluna.sh` exige que tachado, FECHADA e aguardando invalidem o topo. O v3 retira essa autoridade da coluna e admite topo com TRAVA. Pode conservar a propriedade documental de fechamento; precisa substituir as antigas propriedades sobre a escolha do topo. (`bin/tests/test_hook_estado_e_a_coluna.sh:53`, `:54`, `:55`, `:59`; `contrato_item3_v3.txt:3`, `:4`, `:8`)
- O censo exige **NAO SEI/nega** para `O9` contra `O9 (C)`, caixa trocada e obra qualificada contra célula nua. `pertence(id)` por igualdade exata pode devolver **False**, que dispensa censo. Logo a promessa de preservar todas essas propriedades conflita com a nova identidade; precisa ser resolvida expressamente. (`bin/tests/test_censo_antes_da_vaga.sh:544`, `:549`, `:575`, `:603`; `bin/disjuntor.py:1707`, `:1708`; `contrato_item3_v3.txt:5`, `:8`)

**4. Conferência da semente**

O bloco OBRAS desta cópia vai de **712 a 2042**. Linha posterior não satisfaz a identidade exigida pela seção 0. (`app/docs/BACKLOG.md:712`, `:2042`; `contrato_item3_v3.txt:2`)

| Seção / ID | Evidência e classificação documental |
|---|---|
| ORDEM — `SAIDA-NOTURNA-NO-DIA-CERTO` | Linha exata no BACKLOG: **[nao verificado]**. O RELATO registra aval/despacho, mas não fornece a linha exigida em OBRAS. (`app/docs/RELATO.md:29`) |
| ORDEM — `FIO-E0-V4-E3-LEITORES` | **Dentro**, fila 1, ABERTA, EM OBRA. (`app/docs/BACKLOG.md:1873`) |
| ORDEM — `QUARENTENA-DO-COLAB` | **Dentro**, REPROVADA, espera a vez. A linha não declara fila 1; esse pertencimento fica **[nao verificado]**. (`app/docs/BACKLOG.md:765`) |
| ORDEM — `LASTRO-FAIXA-SEM-DEFAULT` | **Dentro**, fila 1, ABERTA. A linha diz que o aval responde o DESENHO; não comprova a trava `PERGUNTA-DE-DESENHO-S1`. (`app/docs/BACKLOG.md:1813`; `contrato_item3_v3_semente.txt:5`) |
| ORDEM — `C20-O168` | **Dentro**, fila 1, espera vaga, ABERTA. (`app/docs/BACKLOG.md:2008`) |
| ORDEM — `RAIZ-DA-DISPUTA-F0-CASO-7279` | Linha exata **fora**, fila 1, sonda, frente da F1. (`app/docs/BACKLOG.md:2417`) |
| ORDEM — `RAIZ-DA-DISPUTA-F0F1` | **Dentro**, fila 1; F0/F1 prontos, F1 respondida, c22 aguarda vaga. Não declara pouso. (`app/docs/BACKLOG.md:1882`) |
| ORDEM — `AUDITORIA-DE-TRAVAS-CORTE` | Linha exata **fora**, instrumento EM CURSO; não declara literalmente fila 1. (`app/docs/BACKLOG.md:2404`) |
| ORDEM — `APP-MENSAGENS-MSG-0` | **Dentro**, fila 1/raia 2, ABERTA, nada construído. (`app/docs/BACKLOG.md:1916`) |
| ORDEM — `TRIAGEM-OFFLINE-SEM-CAMERA` | **Dentro**, REVERTIDA, retorno condicionado; sua inclusão na fila 1/raia 2 aparece na RAIA-2-BO-VISIVEL. (`app/docs/BACKLOG.md:1915`, `:1906`) |
| POS-20 — `BUG-CADASTRO-NAO-ALCANCA-O-FUTURO` | Linha exata **fora**, POS-20/frente da fila 2, próxima vaga de construção. (`app/docs/BACKLOG.md:2408`) |
| POS-20 — `UI-PAINEL-FILA-OPUS-V5` | Linha exata **fora**, POS-20. O pouso citado é da tela; esta linha registra os achados restantes. (`app/docs/BACKLOG.md:2407`) |
| POS-20 — `AUDITORIA-ITEM6-CONTADOR-124` | Linha exata no BACKLOG: **[nao verificado]**. O RELATO diz “NAO construido → fila”, sem classificá-lo como POS-20. (`app/docs/RELATO.md:9`) |
| POS-20 — `TRIAGEM-FILA-OPUS-V6` | **Dentro**, POS-20, ABERTA. (`app/docs/BACKLOG.md:1815`) |
| POS-20 — `E4B-FILA-OPUS-C14` | **Dentro**, POS-20. (`app/docs/BACKLOG.md:1896`) |
| POUSADAS — `FIO-E0-V4-E4B` | **Dentro**, POUSOU 12/11 08:46, commit 087fe758a. (`app/docs/BACKLOG.md:1894`) |
| POUSADAS — `FIO-E0-V4-E4A-2` | **Dentro**, NO AR, merge 613a51498. (`app/docs/BACKLOG.md:1893`) |
| POUSADAS — `O169-FECHADA` | **Dentro, mas a classificação não está provada:** é fila 1 REGISTRAR, delta do contador, B3/volta 5. O pouso citado pertence a **C20-O169**. (`app/docs/BACKLOG.md:1969`, `:1798`) |

Portanto, a ORDEM já contém **um id sem linha exata demonstrada e dois com linha somente fora do bloco**. POS-20 também contém duas linhas externas e um id sem linha exata demonstrada. A validação não pode aceitar esses casos como se satisfizessem a seção 0. (`contrato_item3_v3_semente.txt:2`, `:7`, `:9`, `:13`, `:14`, `:15`; `contrato_item3_v3.txt:2`, `:7`; `app/docs/BACKLOG.md:2404`, `:2407`, `:2408`, `:2417`; linhas ausentes: **[nao verificado]**)

**Contradição de precedência, sem escolher ordem:** a semente põe APP antes de TRIAGEM; a própria linha de APP determina **depois da TRIAGEM**. (`contrato_item3_v3_semente.txt:10`, `:11`; `app/docs/BACKLOG.md:1916`)

**Faltante explícito:** `BUG-ORFAO-COM-PAR` está declarado **ESTRUTURA fila 1**, com sonda/classe/cura pendentes, e não está na semente; sua linha também está fora de OBRAS. Há ainda `TRAVA-MENOS-FILA-COM-SELO`, literalmente fila 1 instrumento **ABERTA**. (`app/docs/BACKLOG.md:2418`, `:2396`; `contrato_item3_v3_semente.txt:1`–`:22`)

Outras linhas continuam declarando fila 1/ABERTA sem classificação na semente: `MEDE-CLEAR-X-COMPACT`, `CODEX-QUEIMA-B`, `C20-FIO-MUDO`, `E3-X1-FOLGA-NAO-E-SEM-COBERTURA`, `SONDA-SEM-GRANT` e `AUDITORIA-0911`. Isso demonstra reconciliação documental incompleta; não certifica que todas devam executar agora. (`app/docs/BACKLOG.md:1796`, `:1797`, `:1817`, `:1876`, `:1979`, `:1981`; `contrato_item3_v3_semente.txt:1`–`:22`)

As **DUVIDAS** do fim:

- **E4c/E4d:** são etapas de `FIO-E0-V4-E4-ESCRITORES`: E4c leitores e E4d parar projeção. Não há identidade própria demonstrada para inseri-las separadamente. Usar o id da obra agrega etapas; usar os aliases viola a identidade exata. O contrato precisa resolver essa representação. (`app/docs/BACKLOG.md:1885`; `contrato_item3_v3.txt:2`)
- **BO-0611-05:** a linha própria continua B3/PARADA; o complemento registra bateria e aval de pouso, sem demonstrar sua efetivação. Estado final: **[nao verificado]**. (`app/docs/BACKLOG.md:1897`, `:1901`)
- **BO-DISPUTA-SEM-PERGUNTA:** a linha própria registra construtor lançado; RAIZ-F0F1 diz expressamente **ESTACIONADA**, com o bug seguindo até F2. Não há pouso demonstrado nessas linhas. (`app/docs/BACKLOG.md:1898`, `:1882`)
- **BUG-CADASTRO:** a relação do item 4(b) com essa obra está documentada no RELATO. Existe discrepância entre o BACKLOG POS-20/frente da fila 2 e a construção já despachada/entregue parcialmente, com 4(b) respondido pelo aval. Nenhuma dessas evidências declara sua transferência para fila 1; não cabe inventá-la nem ignorar o aval. (`app/docs/BACKLOG.md:2408`; `app/docs/RELATO.md:39`, `:50`, `:92`; `aval_respostas_1211_1230.txt:2`)
- **DECLARADO-X-GRAVADO:** o RELATO registra despacho de medida/censo e “Nada construido”, mas não demonstra pertencimento à fila 1 nem linha exata em OBRAS. Portanto **entrada em ORDEM: [nao verificado]**; a identidade exigida impede incluí-lo como está. (`app/docs/RELATO.md:6`, `:7`; `contrato_item3_v3.txt:2`)

## 12/11 14:02 · sessao 183b52816031 · - · - · origem=Ronald (tmux)

A conferência exata da primeira coluna deu:

| Seção/ID da semente | Linha do BACKLOG e conclusão documental |
|---|---|
| ORDEM — SAIDA-NOTURNA-NO-DIA-CERTO | Não ocorre literalmente na cópia; nenhuma linha exata em OBRAS. Bloco delimitado por `app/docs/BACKLOG.md:712` e `:2042`; estado/pertencimento **[nao verificado]**. |
| ORDEM — FIO-E0-V4-E3-LEITORES | `app/docs/BACKLOG.md:1873`: fila 1, ABERTA, EM OBRA. |
| ORDEM — QUARENTENA-DO-COLAB | `app/docs/BACKLOG.md:765`: linha própria dentro; REPROVADA, espera a vez. A linha não declara literalmente fila 1; pertencimento à fila 1 **[nao verificado]**. |
| ORDEM — LASTRO-FAIXA-SEM-DEFAULT | `app/docs/BACKLOG.md:1813`: CONGELA-O-20, obra LASTRO, fila 1, ABERTA. |
| ORDEM — C20-O168 | `app/docs/BACKLOG.md:2008`: fila 1, espera vaga, ABERTA. |
| ORDEM — RAIZ-DA-DISPUTA-F0-CASO-7279 | `app/docs/BACKLOG.md:2417`: fila 1, frente da F1, sonda; linha exata **fora** de OBRAS, depois de `:2042`. |
| ORDEM — RAIZ-DA-DISPUTA-F0F1 | `app/docs/BACKLOG.md:1882`: fila 1, F0+F1 prontos e F1 respondida; c22 na vaga após E3 e E4B; não diz pousada. |
| ORDEM — AUDITORIA-DE-TRAVAS-CORTE | `app/docs/BACKLOG.md:2404`: instrumento EM CURSO; linha exata **fora** de OBRAS. Não declara literalmente fila 1. |
| ORDEM — APP-MENSAGENS-MSG-0 | `app/docs/BACKLOG.md:1916`: fila 1, raia 2, ABERTA, nada construído. |
| ORDEM — TRIAGEM-OFFLINE-SEM-CAMERA | `app/docs/BACKLOG.md:1915`: REVERTIDA, volta condicionada a classe nomeada/RED; fila BO. `:1906` a coloca na fila 1 raia 2. |
| POS-20 — BUG-CADASTRO-NAO-ALCANCA-O-FUTURO | `app/docs/BACKLOG.md:2408`: POS-20, frente da fila 2, construção na próxima vaga; linha exata **fora** de OBRAS. |
| POS-20 — UI-PAINEL-FILA-OPUS-V5 | `app/docs/BACKLOG.md:2407`: POS-20; linha exata **fora** de OBRAS. O “pousada df2be6b12” refere a tela, cujos nove achados são esta fila. |
| POS-20 — AUDITORIA-ITEM6-CONTADOR-124 | Não ocorre literalmente na cópia; nenhuma linha exata em OBRAS. Pertencimento/estado **[nao verificado]**. |
| POS-20 — TRIAGEM-FILA-OPUS-V6 | `app/docs/BACKLOG.md:1815`: POS-20, ABERTA; linha dentro de OBRAS. |
| POS-20 — E4B-FILA-OPUS-C14 | `app/docs/BACKLOG.md:1896`: POS-20; linha dentro de OBRAS. |
| POUSADAS — FIO-E0-V4-E4B | `app/docs/BACKLOG.md:1894`: POUSOU 12/11 08:46, commit 087fe758a; linha dentro. Pouso declarado documentalmente, não confirmado em produção. |
| POUSADAS — FIO-E0-V4-E4A-2 | `app/docs/BACKLOG.md:1893`: NO AR, merge 613a51498; linha dentro. Pouso declarado documentalmente. |
| POUSADAS — O169-FECHADA | `app/docs/BACKLOG.md:1969`: **não prova o pouso deste delta**. Diz fila 1 REGISTRAR, delta do contador, B3 desenho e volta 5. O pouso ali citado pertence a **C20-O169**, cuja linha é `:1798`. |

A semente viola a identidade da seção 0 já na ORDEM: primeiro ID inexistente, RAIZ-CASO e AUDITORIA com linha somente fora do bloco. Também há POS-20 com linha fora ou inexistente; a seção 0 exige linha de OBRAS para todos, mas a lista de erros da seção 3 menciona explicitamente só IDs de ORDEM, deixando inconsistente a validação das exclusões. (`contrato_item3_v3.txt:2`, `:7`; `app/docs/BACKLOG.md:2042`, `:2404`, `:2407`, `:2408`, `:2417`)

Há contradição de ordem explícita sem precisar escolher uma: a semente põe APP antes de TRIAGEM, enquanto a própria linha de APP diz **depois da TRIAGEM**. (`contrato_item3_v3_semente.txt:10`, `:11`; `app/docs/BACKLOG.md:1916`)

Faltantes que o documento ainda declara fila 1 aberta incluem **BUG-ORFAO-COM-PAR** (`app/docs/BACKLOG.md:2418`, fora do bloco), **TRAVA-MENOS-FILA-COM-SELO** (`:2396`, fila 1 instrumento ABERTA, fora), **FIO-E0-V4-E4-ESCRITORES** (`:1885`, fila 1, contém E4c/d), **SONDA-SEM-GRANT** (`:1979`, ABERTA), **AUDITORIA-0911** (`:1981`, ABERTA), **MEDE-CLEAR-X-COMPACT** (`:1796`, ABERTA), **CODEX-QUEIMA-B** (`:1797`, ABERTA) e **C20-FIO-MUDO** (`:1817`, ABERTA). Isso não decide execução nem certifica estado atual em produção; demonstra que a semente não é reconciliação documental completa. (`contrato_item3_v3.txt:3`, `:9`)

DUVIDAS:

- **E4c/E4d:** não têm linha própria exata. São etapas da linha **FIO-E0-V4-E4-ESCRITORES**, que declara E4c leitores e E4d parar projeção; usar o ID da obra perde distinção das etapas, usar “E4c/E4d” viola identidade da seção 0. O desenho precisa resolver a identidade, não o parser. (`app/docs/BACKLOG.md:1885`; `contrato_item3_v3.txt:2`)
- **BO-0611-05:** sua linha permanece B3/PARADA; a linha de complemento registra aval de pouso, bateria e manutenção dos 403, mas não prova efetivação do pouso. Estado final **[nao verificado]**. (`app/docs/BACKLOG.md:1897`, `:1901`)
- **BO-DISPUTA-SEM-PERGUNTA:** linha própria registra construtor lançado; RAIZ-F0F1 registra explicitamente **ESTACIONADA**, bug segue até F2. Não há prova documental de pouso nessas linhas. (`app/docs/BACKLOG.md:1898`, `:1882`)
- **BUG-CADASTRO:** linha exata diz POS-20 e próxima vaga, fora de OBRAS; o aval fornecido aprova 4(b), mas não nomeia o ID completo nem declara formalmente sua migração entre seções. Não presumir identidade/pertencimento só pelo prefixo. A ligação entre “BUG-CADASTRO 4(b)” e esta obra específica é **[nao verificado]**. (`app/docs/BACKLOG.md:2408`; `aval_respostas_1211_1230.txt:2`; `contrato_item3_v3.txt:2`)
- **DECLARADO-X-GRAVADO:** o ID não aparece literalmente nem tem primeira coluna exata; entrada em ORDEM/POS-20/POUSADAS **[nao verificado]**. A regra de identidade impede incluí-lo como está. (`contrato_item3_v3.txt:2`; limites de OBRAS em `app/docs/BACKLOG.md:712`, `:2042`)

## 12/11 14:02 · sessao 8e13c5768979 · - · - · origem=Ronald (tmux)

**A lista nominal do §1 não reproduz todo o censo; a cláusula “TODOS os leitores do censo v2” cobre as omissões genericamente.** Faltam nominalmente `interrompidas_estado.py`, as primeiras leituras do handoff e os transportadores/entradas abaixo. Isso não demonstra segundo parser sobrevivente: vários já delegam ao hook e herdarão a mudança se as interfaces forem conservadas. (`contrato_item3_v3.txt:5`; `bin/interrompidas_estado.py:54`, `:93`; `bin/handoff_sessao.sh:64`, `:71`, `:120`)

| Fora da enumeração nominal do §1 | Situação |
|---|---|
| `interrompidas_estado.py` | Pergunta topo ao hook; lê INTERROMPIDA/FUNDIDA e fechamento documental por outra pergunta. Migrar o topo indiretamente, conservar o medidor documental. (`bin/interrompidas_estado.py:93`, `:105`, `:121`, `:125`) |
| Primeira parte de `handoff_sessao.sh` | Topo, em curso e PAREI já vêm do hook; falta atualizar “MARCADOR” e comentários antigos. (`bin/handoff_sessao.sh:58`, `:64`, `:66`, `:71`, `:120`, `:123`) |
| `codex_pergunta.sh` | Delega o censo ao disjuntor; linha atual é **247**, não 228. (`bin/codex_pergunta.sh:247`) |
| `placar_code.sh` | Delega o alarme. (`bin/placar_code.sh:368`) |
| `fabricante.sh`; `fabricante_alvo.py` chamadas finais | Delegam o seletor migrado. (`bin/fabricante.sh:129`; `bin/fabricante_alvo.py:315`, `:320`) |
| `relato.sh` publicação | Transporta os dois documentos, não decide pertencimento. (`bin/relato.sh:115`, `:116`) |
| `.claude/settings.json` | Entradas dos instrumentos e leitura do handoff; continuam apontando para `/home/ronald/saas-hasner`. Execução desta cópia nesses caminhos: [nao verificado]. (`.claude/settings.json:9`, `:20`, `:29`, `:40`, `:53`, `:68`) |
| `gerar_backlog.py` | Não está nominalmente no §1, mas está coberto pelo §2. (`contrato_item3_v3.txt:6`) |
| Selos | Não enumerados no v3; §4 remete genericamente ao censo e exige lista no RELATO. (`contrato_item3_v3.txt:8`) |

**As exceções por “OUTRA pergunta” fazem sentido, com estas fronteiras:**

- Avais de hoje ainda não cumpridos e linhas da raia20 perguntam fechamento documental, não pertencimento. (`bin/handoff_sessao.sh:139`, `:171`, `:209`, `:216`)
- Interrompidas pergunta estado, fusão, fechamento e retorno ao topo: só a parte do topo pertence à migração da ORDEM. (`bin/interrompidas_estado.py:105`, `:116`, `:121`, `:125`)
- `gerar_backlog_topo.py` resume O-números abertos e ordena por número; não é ORDEM VIVA. (`bin/gerar_backlog_topo.py:19`, `:26`, `:39`)
- Fabricante pergunta elegibilidade de sorteio: dinheiro, “só registrar”, espera por Ronald e níveis livre/escalonado. Não equivale simplesmente a “pertence à fila 1”. A exceção do v3 só funciona se o ramo preservado de livres/O-F for efetivamente limitado aos **fora de [ORDEM]**; do contrário, um integrante da fila pode continuar julgado pela coluna no ramo “livre”. (`contrato_item3_v3.txt:5`; `bin/fabricante_alvo.py:256`, `:259`, `:261`, `:273`, `:275`, `:282`)
- Existência do item e padrão declarado da obra são perguntas documentais. (`app/core/tests/test_marca_cron15.py:89`; `app/chamados/tests/e4b_c4_verificar.py:209`; `bin/tests/test_prompt_virou_item.sh:25`)

**Há dois conflitos substantivos na promessa de migrar fixtures preservando a propriedade de todos os selos:**

1. `pertence(id)` por igualdade exata altera a propriedade do censo: hoje `O9` contra `O9 (C)`, caixa trocada e obra qualificada contra célula nua devem resultar em **NAO SEI/nega**, não FORA. Também `O9` exato acompanhado de `O9 (C)` deve negar. Igualdade simples retorna False ou True, e False dispensa censo. Isso exige decisão explícita no contrato; trocar fixture não preserva essa propriedade. (`contrato_item3_v3.txt:2`, `:5`, `:8`; `bin/tests/test_censo_antes_da_vaga.sh:544`, `:549`, `:575`, `:603`; `bin/disjuntor.py:1707`, `:1708`)

2. `test_hook_estado_e_a_coluna.sh` exige que tachado, FECHADA e aguardando façam o topo ser “parado”. O v3 retira essa autoridade da coluna e declara topo o primeiro id com ou sem TRAVA. Conservar o teste documental de `_item_fechado` é possível; conservar as antigas asserções sobre o topo não é. O contrato precisa identificar propriedades revogadas e propriedades preservadas, em vez de prometer apenas migração de fixture. (`contrato_item3_v3.txt:3`, `:4`, `:8`; `bin/tests/test_hook_estado_e_a_coluna.sh:53`, `:54`, `:55`, `:59`)

As referências a `contrato_item3_v3.txt` acima numeram o anexo fornecido, com título na linha 1; as referências ao código usam as linhas desta cópia. As funções atuais do disjuntor estão em `:1623` e `:1672`, diferentes das referências históricas do v2/v3. (`bin/disjuntor.py:1623`, `:1672`)

## 12/11 14:01 · sessao 00795d4fdce6 · - · - · origem=Ronald (tmux)

**O v3 corrige a diferença universo/andabilidade no desenho, mas não sustenta “RED verde sem editar”.** [contrato_item3_v3.txt:1-3, anexo fornecido] [nao verificado]

- `[ORDEM]` inclui itens travados; os restantes só andam se não tiverem `TRAVA`. Portanto A/B/C com B e C travados admite PAREI de A; removendo a trava de C, deve bloquear. Isso coincide com a regra existente: `_relato_parou` exige PAREI do topo e nenhum restante andável. [contrato_item3_v3.txt:3, :8, anexo; bin/hook_stop_fila1.py:1232, :1243, :1245]
- O arquivo `bin/tests/test_hook_andavel_e_a_ordem_viva.sh` não existe nesta cópia; seu conteúdo literal e mudanças exatas nas linhas 51/73 são **[nao verificado]**. O RELATO registra o RED ca320b69f em outra worktree, `wt-travas-105806`. [app/docs/RELATO.md:37]
- Pela descrição fornecida do RED, precisa: criar `.ids` na fixture; direcionar o leitor compartilhado para esse arquivo; manter os mesmos IDs e linhas OBRAS; representar os restantes travados como `ID | TRAVA: !` no primeiro cenário; retirar a TRAVA do restante aberto no segundo; manter ruído/POS-20 fora de ORDEM e preservar as expectativas liberar/bloquear. O marcador/prosa pode permanecer como ruído, mas não pode alimentar a ordem. [item3_contrato_v2.r, seção “Os selos”, entrada do RED; contrato_item3_v3.txt:2-3, :8, anexos; conteúdo literal do RED: **[nao verificado]**]
- Isso preserva a **propriedade** do RED, mas edita sua fixture. A promessa “SEM ser editado” e a ordem de migrar fixtures são incompatíveis. [contrato_item3_v3.txt:1, :8, anexo]
- Há exemplo verificável dessa migração necessária no selo vizinho: ele só escreve BACKLOG e alterna coluna entre livre/travada, depois exige bloquear/liberar. Sem fornecer `.ids`, não descreve a fonte do v3. [bin/tests/test_hook_parei_com_andavel.sh:25, :27, :30, :41, :46, :56, :57]

**Cuidado adicional:** topo com TRAVA é permitido no v3, mas o veto atual de PAREI rejeita qualquer ID de `_ids_que_nao_andam()` antes de comparar com o topo. Migrar essa lista mecanicamente para “IDs com TRAVA” recusaria o PAREI do próprio topo. O contrato manda migrar ambas funções, portanto é implementável, mas precisa distinguir pertencimento, trava e topo; o novo cenário A/B/C mantém A sem TRAVA e não guarda esse caso. [contrato_item3_v3.txt:3, :5, :8, anexo; bin/hook_stop_fila1.py:1298, :1302, :1310, :1313]

**Falha residual:** o v3 não enumera ilegibilidade ou erro de decodificação como `ErroFonte`; sem conversão explícita, alcançam a falha aberta existente. [contrato_item3_v3.txt:7, anexo; bin/hook_stop_fila1.py:1460, :1461]

## 12/11 14:01 · sessao 2b6ccd8a2d08 · DECLARADO-X-GRAVADO · sonda · origem=Code

Referências `saída:N` abaixo numeram somente as linhas iniciadas por `S` do dado fornecido, começando em `S UNIVERSO 6729`; a última é `saída:107`. São referências à transcrição, não a um arquivo criado.

**Não há bloqueio por erro de execução:** `TOTAL_ERROS TOTAL = 0`; todas as etapas informadas também têm zero. Não há exemplo de erro apresentado. As contagens podem ser publicadas como resultados da sonda, preservando as limitações abaixo (`saída:95-107`). [nao verificado]

Item 2 — desfechos das perguntas respondidas:

| Desfecho | Quantidade | Fonte |
|---|---:|---|
| Batida nova | 886 | saída:2 |
| Reuso | 23 | saída:3 |
| Recusa de paridade | 230 | saída:4 |
| Pendente do admin | 121 | saída:5 |
| Não validada | 1210 | saída:6 |
| OUTRO | 4259 | saída:7 |
| **Soma dos desfechos** | **6729** | saída:8 |
| **Universo** | **6729** | saída:1 |
| Indicador soma igual ao universo | 1 | saída:9 |

`OUTRO` está integralmente explicado pela categoria `validada_sem_relacao_nem_recusa`: **4259**. Não cabe redistribuí-la entre os demais desfechos (`saída:10`).

Item 3 — diferença entre instante gravado e resposta com data, preservando as categorias emitidas:

| Medida | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| Novas por origem | 437 — saída:18 | 449 — saída:38 |
| Até 5 minutos | 260 — saída:19 | 283 — saída:39 |
| Mais de 5 até 30 minutos | 24 — saída:20 | 43 — saída:40 |
| Acima de 30 minutos | 137 — saída:21 | 108 — saída:41 |
| Delta não verificado (`delta_nv`) | 16 — saída:22 | 15 — saída:42 |

As faixas, incluindo `delta_nv`, fecham os respectivos totais de novas por origem (`saída:18-22`; `saída:38-42`).

| Datas e sinal do delta | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| `resposta_dmais1` | 44 — saída:23 | 92 — saída:43 |
| Gravada fora do dia do turno | 48 — saída:24 | 93 — saída:44 |
| Datas diferentes | 10 — saída:25 | 5 — saída:45 |
| Delta assinado negativo | 87 — saída:26 | 61 — saída:46 |
| Delta assinado positivo | 83 — saída:27 | 127 — saída:47 |
| Delta zero | 251 — saída:28 | 246 — saída:48 |

A convenção da subtração usada no delta assinado é **[nao verificado]**; os sinais acima não foram traduzidos como “adiantamento” ou “atraso”.

| Proximidade do marco oposto | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| Medidos | 420 — saída:29 | 431 — saída:49 |
| `oposto_ate5` | 35 — saída:30 | 52 — saída:50 |
| `oposto_ate30` | 55 — saída:31 | 86 — saída:51 |
| Não verificados (`oposto_nv`) | 17 — saída:32 | 18 — saída:52 |

A saída fornece proximidade por tolerância; quantidade de coincidências exatas com o marco oposto **[nao verificado]**. A vigência histórica do marco também exige a ressalva de `dna_vigencia_nao_confirmada` (`saída:30-32`; `saída:50-52`; `saída:91`).

Item 4 — **TRIAGEM** dos casos acima de 30 minutos:

| Resultado emitido | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| Resposta mais próxima | 49 — saída:33 | 26 — saída:53 |
| Gravada mais próxima | 9 — saída:34 | 2 — saída:54 |
| Empate | 0 — saída:35 | 0 — saída:55 |
| Sem indício | 79 — saída:36 | 80 — saída:56 |
| Não verificado (`triagem_nv`) | 0 — saída:37 | 0 — saída:57 |

A triagem fecha nos respectivos totais acima de 30 minutos; ela permanece indício, conforme o item 4 do aval, sem determinar qual horário foi o efetivamente trabalhado (`saída:21,33-37`; `saída:41,53-57`; `aval_declarado_x_gravado_v2.txt:7`, contando as linhas do anexo transcrito).

Item 5 — regeneração da célula depois da resposta:

| Categoria emitida | Quantidade | Fonte |
|---|---:|---|
| Depois da resposta | **403** | saída:11 |
| Não depois da resposta | 5984 | saída:12 |
| Sem data | 0 | saída:13 |
| Sem célula | 342 | saída:14 |
| Erro de célula | 0 | saída:15 |
| Erro | 0 | saída:16 |
| Não processadas | 0 | saída:17 |

Essas categorias fecham o universo. “Sem célula” permanece separado de “não depois da resposta” (`saída:1,11-17`).

Item 6 — carga semanal ISO, com os três campos numéricos na ordem exata da saída:

| Semana ISO | Primeiro campo | Segundo campo | Terceiro campo | Fonte |
|---|---:|---:|---:|---|
| 2026-W39 | 280 | 44 | 5 | saída:58 |
| 2026-W40 | 148 | 13 | 6 | saída:59 |
| 2026-W41 | 147 | 34 | 0 | saída:60 |
| 2026-W42 | 221 | 28 | 1 | saída:61 |
| 2026-W43 | 160 | 30 | 7 | saída:62 |
| 2026-W44 | 180 | 37 | 0 | saída:63 |
| 2026-W45 | 81 | 15 | 3 | saída:64 |
| 2026-W46 | 93 | 6 | 0 | saída:65 |

**Os cabeçalhos desses três campos são [nao verificado].** As linhas fornecidas não os identificam; portanto, a associação de cada coluna a “validações”, “acima de 30 minutos” ou outra medida não pode ser publicada como confirmada (`saída:58-65`). O recorte temporal efetivamente aplicado pela consulta é **[nao verificado]**. [nao verificado]

| Colaborador | Contagem emitida | Fonte |
|---|---:|---|
| col569 | 32 | saída:66 |
| col723 | 22 | saída:67 |
| col926 | 14 | saída:68 |
| col438 | 11 | saída:69 |
| col696 | 9 | saída:70 |
| col993 | 8 | saída:71 |
| col518 | 7 | saída:72 |
| col055 | 6 | saída:73 |
| col502 | 5 | saída:74 |
| col909 | 5 | saída:75 |

| Escala modelo | Contagem emitida | Fonte |
|---|---:|---|
| #431 | 32 | saída:76 |
| #276 | 22 | saída:77 |
| #178 | 15 | saída:78 |
| #496 | 14 | saída:79 |
| #197 | 11 | saída:80 |

| Vínculo | Contagem emitida | Fonte |
|---|---:|---|
| #484 | 32 | saída:81 |
| #924 | 22 | saída:82 |
| #1181 | 14 | saída:83 |
| #376 | 11 | saída:84 |
| #970 | 9 | saída:85 |

O filtro exato usado para selecionar os casos desses rankings é **[nao verificado]**; as tabelas preservam as contagens e identificadores emitidos (`saída:66-85`).

Limitações e sobreposições declaradas pela própria saída:

| Aviso | Quantidade e fonte | O que impede afirmar |
|---|---|---|
| `criacao_por_esta_pergunta_nv` | 886 — saída:86 | Que cada batida classificada como nova foi criada pela própria pergunta examinada. |
| `dna_anterior` | 24 — saída:87 | São casos sinalizados com DNA anterior; o rótulo sozinho não comprova sua vigência no instante do plantio. |
| `dna_atual` | 831 — saída:88 | São casos sinalizados com DNA atual; isso sozinho não comprova que ele era o vigente no plantio. |
| `dna_dna_sem_marcos` | 5 — saída:89 | Concluir a comparação com o marco nesses casos. |
| `dna_historico_multiplas_regeneracoes` | 13 — saída:90 | Reconstruir toda a sequência histórica apenas com o DNA anterior disponível. |
| `dna_vigencia_nao_confirmada` | 13 — saída:91 | Afirmar atendimento integral ao requisito de usar o marco vigente no plantio. |
| `origem_disputa_preexiste_resposta_atual` | 3 — saída:92 | Atribuir a criação dessa batida à resposta atual. |
| `pendencia_admin_com_batida` | 14 — saída:93 | Tratar pendência administrativa como sinônimo de ausência de batida. |
| `relacao_com_batida_retratada` | 5 — saída:94 | Tratar toda relação encontrada como batida atualmente apurável. |

O código confirma que `dna_anterior` guarda somente a versão imediatamente anterior, enquanto o histórico completo fica fora desse campo; `regenerada_em` registra a última regeneração. Portanto, o item de regeneração e a comparação histórica não demonstram, por si, todas as versões passadas ([models.py](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/models.py:1714), [models.py](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/models.py:1718)).

Esses avisos não constituem uma partição demonstrada: suas interseções são **[nao verificado]**. Zero erros de execução não resolve as limitações declaradas (`saída:86-107`).

## 12/11 14:01 · sessao e74da5cb4d3c · - · - · origem=Ronald (tmux)

Parte A — provas locais para consolidação (todos os caminhos abaixo relativos à raiz da cópia).

A lei vigente está em `app/docs/CORTES.md:348`, complementada por O44 em `app/docs/CORTES.md:349`; data da flag sem trilha e item sem dia estão decididos em `app/docs/CORTES.md:352`. A reprovação e as sete curas constam literalmente em `app/docs/BACKLOG.md:765`. As alternativas de fechar chamados / impedir nascimento foram substituídas: `app/docs/CORTES.md:345`, `app/docs/CORTES.md:346`, `app/docs/CORTES.md:347`; a substituição do item do emissor também consta em `app/docs/BACKLOG.md:766`.

A1 — quem decide hoje

| Leitor/estado | Pergunta, universo e escritor |
|---|---|
| `isento_ponto` | Responde **flag atual**, não “isento nesta data”. Retorna bool de `Colaborador.isento_ponto`, sem cargo nem período: `app/core/constants.py:13-18`. Campo booleano sem data: `app/colaboradores/models.py:294`. Tela Passe Livre escreve pela porta `gravar(...,'cadastro',...)` em `app/colaboradores/views.py:173-176`, tenta trilha com antes/depois/motivo em `:183-186`; falha da trilha não desfaz alteração, `:187-202`. Semente histórica escreve flag sem trilha em `app/colaboradores/migrations/0043_isento_ponto.py:13-14`. |
| `silencio_isentos.alvos` | “Quais chamados vivos de flag isento ainda não têm **nenhum** `silenciado_ate`?” Filtra flag diretamente e todos VIVOS; não restringe módulo, dia ou janela: `app/chamados/services/silencio_isentos.py:20-31`. `silenciar` põe prazo padrão pela porta `arquivo.silenciar`: `:34-49`. Ignora resgate por política declarada: `:5-6`. |
| Gatilho da flag e do chamado | Todo save de colab com flag true agenda `silenciar_do_colab`, sem detectar transição false→true: `app/chamados/signals.py:269-281`. Chamado nascido/voltando a VIVOS ou com véu levantado agenda a mesma porta se `silenciado_ate is None`: `:285-308`. Portanto levantar véu de isento pode chamar o ressequenciamento que o põe de novo: `:290-301`. |
| Cron isentos | Modo `--alarme` é leitura e retorna 2 se houver alvos; `--apply` continua autorizado pelo próprio command a escrever véus: `app/chamados/management/commands/silenciar_chamados_isentos.py:25-49`. Seu universo atual é `alvos()`, sem janela: `:36`. |
| `silenciado_em` / `q_silenciado_em` | “O véu do **chamado** vale neste instante?” Ambos usam prazo futuro: `app/chamados/models.py:508-524`. O escritor é `arquivo.silenciar` (`app/chamados/services/arquivo.py:141-174`) ou `arquivo.levantar` (`:177-212`), não um estado de quarentena. |
| `qs_na_fila` | “Quais chamados cobram agora?” VIVOS → COBRANÇA → exclui silêncio futuro: `app/chamados/services/painel.py:188-190`; VIVOS definido pela consulta em `:153-155`. Não pergunta isenção nem período. |
| `chamados_visiveis_colab` | Lista de trabalho da pessoa: competência corrente por empresa (`app/chamados/services/janela_colab.py:40-46`), ampliada por perguntas `q_vivas` com destino (`:65-73`), exclui silêncio futuro e módulos internos (`:74-79`). Não filtra status vivo. `desligamento_revisao` já é interno à lista do colab: `:30-37`. |
| Contadores/pílulas | Ativos e abertos/em análise usam `qs_na_fila`: `app/chamados/services/painel.py:212-225`. Pílula calcula `particionar_verbos(qs_vivos(...))`: `:229-231`. **Divergência ainda presente**: qualquer `silenciado_ate` vencido vira HISTORICO, `app/chamados/services/verbos.py:48-51`; lista padrão exclui ARQUIVADO e HISTORICO, `app/chamados/services/painel.py:325-331`. Contraria retorno exigido em `app/docs/CORTES.md:335`. |
| Badge admin | Prefere `PlacarNavbar.badge_chamados`, depois `exige_o_admin(particionar_verbos(qs_vivos()))`, depois fila: `app/core/context_processors.py:80-93`. Badge “registrados” lê lavrado ou fila: `:138-150`. Badge colab lê fila: `:185-187`. Escritor do lavrado: `app/core/management/commands/lavrar_badge_navbar.py:91-93`; origem painel: `:39-42`, exigência admin: `:72-74`. Logo só migrar fila deixa caminho da pílula/badge admin descoberto. |
| Alerta SLA | Universo de cobrança lido via fila: `app/chamados/management/commands/alertar_chamados_sla.py:24-32`. |
| Alerta avançado | Isento atual é descartado pelo juiz `isento_ponto`, `app/ponto/management/commands/processar_alertas_avancados.py:29-33`; N5 também descarta chamado com silêncio vigente pelo método, `:110-113`. O primeiro é flag global, o segundo véu do chamado; nenhum pergunta período. |
| Disparo pela célula | `motivos_a_perguntar` usa dia sem trabalho, situação ativo, tempo, veredito e lista admin: `app/chamados/services/fabrica_da_celula.py:105-125`; `perguntar_pela_celula` chama `perguntar` com dia da célula, `:217-220`. `perguntar` chama `gerar_perguntas_disputa` em `:160-174`; não há consulta ao novo juiz. |
| Disparo por disputa/agrupador | `gerar_perguntas_disputa` (`app/chamados/services/disputa_emissao.py:300`) abre conversa ou pergunta datada em `:439-451`. Agrupador `_criar_pergunta_se_couber` guarda ausência e duplicata e abre em `:823-847`; âncora abre conversa em `:1371`. |
| `detectar_par_relampago` | Na cópia linhas antigas deslocaram: universo é escala ativa sem fim + colab ativo, `app/ponto/management/commands/detectar_par_relampago.py:336-338`; abre **fato chamado** em `:368-385`, sem consulta a isento/quarentena. Pela lei vigente emissão de fato continua; isolar cobrança/disparo, não reintroduzir a regra substituída de “não nasce”: `app/docs/BACKLOG.md:766`, `app/docs/CORTES.md:348`. |

A2 — matriz exigida de pré-desenho (não há implementação desta entidade/juiz/porta localizada; status de implementação das sete curas nesta cópia: **[nao verificado]**, a linha da obra é reprovada em `app/docs/BACKLOG.md:765`).

| Estado → juiz | Operação | Persistência/retorno a exigir | Prova do contrato |
|---|---|---|---|
| Sem janela → falso; dia coberto → verdadeiro | Abrir período manual | Entidade com colab/início/fim/motivo/quem/quando e uma porta com trilha; não fechar chamados nem pôr véu. Retorno real/API inexistente **[nao verificado]**. | `app/docs/CORTES.md:348` |
| Janela de isento aberta → verdadeira só no período | **Fechar**, não levantar, ao desligar flag | Preservar passado do período e registrar fim; distinguir do levantamento administrativo que devolve cobrança. A distinção é cura exigida, assinatura concreta e bordas inclusivas **[nao verificado]**. | `app/docs/BACKLOG.md:765`; flag atual só bool em `app/core/constants.py:18` |
| Quarentena vigente → verdadeiro | Levantar | Quem/quando/motivo + trilha; retorno automático dos itens, sem mutar chamados/cálculo. | `app/docs/CORTES.md:348` |
| Dia isolado → falso **só para aquele dia** | Desarquivar `(colab,dia)` por toque admin | Exceção persistida/trilhada para o par; restante da janela segue isolado. Não chamar `arquivo.levantar` de todos os chamados. | `app/docs/CORTES.md:349`; porta atual levanta um chamado em `app/chamados/services/arquivo.py:177-212` |
| Flag true → janela de isento | Porta separada da janela de isento | Efeito da troca de flag passa pela porta; impedir que alteração manual de período vire segundo escritor da semântica da flag. Detalhe técnico da porta **[nao verificado]**. | `app/docs/BACKLOG.md:765`; trocas com trilha/abre-fecha em `app/docs/CORTES.md:352`; escritor atual `app/colaboradores/views.py:176` |
| Histórico de flag → início da janela | Migration reversível e congelada; última ligação | Selecionar **última ligação** relevante do histórico, não primeira flag true; sem trilha usar admissão e rótulo explícito. Congelar interpretação para não importar juiz runtime mutável e guardar reversão que remova só o que migration criou. Implementação/migration específica **[nao verificado]**. | sete curas `app/docs/BACKLOG.md:765`; fallback autorizado `app/docs/CORTES.md:352`; seed histórica sem trilha `app/colaboradores/migrations/0043_isento_ponto.py:13-14` |
| Chamado de isento com dia coberto pelo juiz | Vigia só dentro da janela | Esperado 0 para cobrança que escapou **dentro** da janela; chamado anterior ao início não pode ser chamado vazamento por flag atual. Não escrever nem re-silenciar. | `app/docs/BACKLOG.md:765`; universo atual demasiado amplo `app/chamados/services/silencio_isentos.py:24-31` |
| Pergunta viva ou retida em item isolado | RED da retenção | Cobrir que retenção não devolve cobrança pela lateral, e que levantar/desarquivar restaura conforme lei. Qual `retencao` a reprovação nomeou especificamente é **[nao verificado]**: há `pergunta_no_app.retencao`, `app/chamados/services/pergunta_no_app.py:121-153`, e retenção de retratação, `app/chamados/reconciliador.py:438-455`. Não afirmar que uma delas é a nomeada sem anexo adicional. | `app/docs/BACKLOG.md:765` |
| Candidatos migration → antes/depois | DIFF pela **função real da migration** | Executar seleção/projeção pura da função congelada em modo leitura e comparar itens/colabs/perguntas; não reconstruir seleção paralela. Função concreta não localizada **[nao verificado]**. | `app/docs/BACKLOG.md:765`; grandezas do DIFF `app/docs/CORTES.md:348` |
| Item sem dia | Perguntar o mesmo juiz | Janela aberta cobre item sem dia; fechada não cobre. | `app/docs/CORTES.md:352` |
| `desligamento_revisao` | Fora da quarentena | Sem isolar ato de DP por esta obra; não criar juiz extra de dono. | `app/docs/CORTES.md:348` |

A3 — arquivos e sobreposição

Núcleo necessariamente toca `app/chamados/models.py` (entidade histórica; estado atual só véu em `:508-524`), `app/chamados/juizes.py` (corte autoriza **apenas** `colab_em_quarentena`, `app/docs/CORTES.md:348`), `app/core/juizes.py` (registro previsto na política local; nova entrada **[nao verificado]**), migration nova (nome **[nao verificado]**), porta única nova (nome **[nao verificado]**), `app/chamados/views_arquivar.py`/template (modo/lista/botão exigidos por `app/docs/CORTES.md:348`), `app/colaboradores/views.py:176` e `app/chamados/signals.py:269-308` (troca de flag e vassoura atual). Preservar espelho/folha/TXT e não trocar `isento_ponto` de cálculo por novo significado de cobrança: `app/docs/CORTES.md:348`.

Leitores diretos a migrar: fila `app/chamados/services/painel.py:188-190`; pílula `app/chamados/services/verbos.py:48-51`; lista do colab `app/chamados/services/janela_colab.py:74-79`; alerta avançado `app/ponto/management/commands/processar_alertas_avancados.py:110-113`; fábrica `app/chamados/services/fabrica_da_celula.py:92`, `:160`, `:178`, `:234`; gerador e agrupador `app/chamados/services/disputa_emissao.py:300`, `:823`, `:1302`; vigia e universo fio mudo `app/chamados/management/commands/disparar_perguntas_competencia.py:99-120`; vassoura `app/chamados/services/silencio_isentos.py:20-31` e seu command `app/chamados/management/commands/silenciar_chamados_isentos.py:31-49`. Caminhos indiretos/badge precisam validação após migração porque preferem valor lavrado: `app/core/context_processors.py:80-93`, `:138-150`; lavrador `app/core/management/commands/lavrar_badge_navbar.py:39-42`, `:91-93`.

Outros disparadores a conferir sem trancar declaração/ação voluntária: `app/chamados/services/acoes_disputa.py:263` abre conversa; `app/chamados/services/declaracao_texto.py:376`, `:666` abre pergunta no ato de declaração. A porta genérica `PerguntaDisputa.abrir` atende ambos, `app/chamados/models.py:2792-2807`; pôr veto universal nela arriscaria contrariar “nada tranca”, `app/docs/CORTES.md:349`. O censo deve distinguir disparo de cobrança e ato voluntário por chamador, sem juiz novo.

Crons chamadores do agrupador: `app/ponto/management/commands/detectar_ausencias.py:803`, `app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/processar_alertas_avancados.py:159`; célula/cartório chama fábrica em `app/ponto/services/cartorio.py:1045`. `detectar_par_relampago.py:368` continua emissão de fato, não chamar isso de disparador direto de PerguntaDisputa sem prova.

Sobreposição E3: usar o aval recebido como fonte do diff, pois comparação main não está disponível. Ele nomeia `chamados/juizes.py` alimentar_portas e `chamados/models.py` q_silenciado_em/respostas_sem_veredito; fonte da cópia confirma localizações atuais em `app/chamados/models.py:515`, `:1836` e `app/chamados/juizes.py:1541` em diante. Sobreposição SAÍDA-NOTURNA nos mesmos **arquivos**, outras regiões dadas pelo usuário: `juizes.py:679-705/:941-971`, `models.py:2511/:3179/:3738`; conteúdo do diff SAÍDA-NOTURNA **[nao verificado]**.

Observação para C2: `app/docs/CORTES.md:345` lista os oito históricos nominalizados por #pk: `3403,3057,11935,23036,11658,23049,23037,23030`. Esses não são lista completa dos 12 nem medição atual. Só o dado/snapshot sustenta “8+4” à época; resultado atual permanece **[nao verificado]**.

## 12/11 14:00 · sessao 667377a17498 · - · - · origem=Ronald (tmux)

B1 — Varri **3.566 arquivos `.py` por AST**, sem erro de parse; procurei Store de atributo, keyword `respondida_em`, dict/subscript, `save(update_fields)`, `bulk_update`, `F()` e `setattr`, e complementei com fluxo de `**campos`/`**outros` e o censo estrutural existente. O censo existente reconhece aliases/imports e gerentes reversos, mas declara limites de atribuição em `app/core/censo_escritas.py:43`, `:451`, `:469`. Resultado da execução só em memória: 10 sítios literais runtime, 317 sítios em testes (160 arquivos); o total bruto inclui atribuições em memória e descritores de campo, portanto **não equivale a 10 escritores persistentes**. A execução do censo não tem arquivo de resultado persistido: `[nao verificado]` por arquivo:linha.

| Escritor / fluxo | Pode persistir timestamp sem texto? | Ramo e prova |
|---|---|---|
| `responder_pergunta_disputa_core` | Não com entrada vazia no fluxo normal | `resposta` é normalizada e vazia retorna antes (`app/chamados/services/disputa_emissao.py:1432`); texto e timestamp atribuídos juntos `:1467`, `:1468`; campos `:1469`, save `:1508`; registra ato de resposta em seguida `:1511`. |
| `responder_automaticamente` | **A função aceita** `texto=''`/espaços; grava ambos vazios e ato vazio se `instante` não nulo | Guarda apenas texto **anterior** `app/chamados/services/disputa_emissao.py:1647`; grava `texto` recebido sem validar `:1652`, timestamp `:1653`, save `:1655`, `ato_texto=texto` `:1656`. Único chamador runtime encontrado passa constante não vazia: `app/api/foto_recebida.py:25`, `:48`. Escrita vazia efetiva em produção: `[nao verificado]`. |
| `carimbar(...,resposta='responder'/'validar')` | Pode escrever timestamp sem **reescrever** texto, mas só com texto preexistente não vazio | Guarda real `texto_vazio` em `app/chamados/models.py:3146`, timestamp só quando nulo `:3147`, `:3148`, save `:3168`; não escreve `ato_texto` aqui. Ausência semântica de texto não é permitida por esse ramo. |
| `carimbar_resposta_pela_trilha` | Pode gravar só o timestamp no modelo, mas exige texto preexistente e registra-o no ato | Recusa carimbo existente ou texto vazio em `app/chamados/services/disputa_emissao.py:1670`; atribui `:1674`; save somente `respondida_em` `:1676`; `ato_texto=p.resposta_colab` `:1678`. |
| `reabrir_para_correcao` | Só grava `None` no timestamp nos usos atuais | Dicionário com `respondida_em=None` e `resposta_colab=''` em `app/chamados/services/disputa_emissao.py:1593`; preserva foto anterior `:1615`; `descarimbar_em_lote(**campos)` `:1630` ou `qs.update(**campos)` `:1632`. |
| `ressuscitar_perguntas` | Só grava `None` nos usos atuais; não cria timestamp não nulo | Universo restrito a texto vazio/null em `app/chamados/models.py:1499`; chamada persistente com `respondida_em=None` em `:1579`. |
| `descarimbar_em_lote(qs,**outros)` | **Porta genericamente aceita** timestamp arbitrário sem texto | Só proíbe chaves de carimbo, não o par de resposta: `app/chamados/models.py:3271`; `update(...,**outros)` em `:3280`–`:3282`. Chamadores encontrados passam apenas `None`: `models.py:1579`, `disputa_emissao.py:1593`, `:1630`. Uso não nulo atual: `[nao verificado]`. |
| Nascimento `abrir` / `abrir_conversa` → `_sob_contrato` → `_nascer` | **Porta genericamente aceita** `respondida_em` em `**campos` sem texto | `app/chamados/models.py:2792`, `:2810`, `:2822`, `:2841`; constrói `cls(...,**campos)` `:3030`–`:3031`, insere `:3033`; `save` não valida o par `:3429`–`:3439`. A conservação da projeção gera ato até com texto vazio em `:3947`–`:3950`. Não localizei chamador runtime que passe explicitamente assinatura não nula no nascimento; origem real em produção `[nao verificado]`. |

Não são escritores persistentes:

- `_universo_da_ressuscitacao` atribui `None` numa **cópia** para consulta ao juiz: `app/chamados/models.py:1507`, `:1521`, `:1522`.
- `_desfazer_em_memoria` restaura o valor antigo sem save: `app/chamados/models.py:3175`, `:3179`; usado nos ramos de recusa/exceção `:3157`, `:3160`, `:3171`.
- `DecisaoPergunta.linhas_da_projecao_sem_linha` **lê** `respondida_em` e pode copiar legado sem texto para `ato_texto=''`, não escreve `PerguntaDisputa.respondida_em`: `app/chamados/models.py:3947`–`:3950`.
- O `bulk_update` de PerguntaDisputa encontrado grava apenas os três campos da lavra, não `respondida_em`: `app/chamados/veredito_lavrado.py:237`, `:250`.
- `setattr` genérico de reexibição só muta memória, sem save: `app/core/reexibir.py:21`, `:49`, `:50`.
- Migration só declara o campo, não contém backfill dele: `app/chamados/migrations/0014_perguntadisputa.py:25`.
- Não encontrei `F('respondida_em')`, `setattr(...,'respondida_em',...)`, `bulk_update(...,'respondida_em')` nem update runtime direto não nulo; a ausência resulta da varredura em memória e não tem artefato persistido `[nao verificado]`.

A suíte antiga não certifica a simetria pedida: o contrato declara literalmente o inverso “inofensivo”, só exige `respondida_em` quando encontra escrita de `resposta_colab`, e só inspeciona atribuição/update no corpo: `app/chamados/tests/test_contract_par_resposta.py:28`–`:37`, `:69`–`:83`; mutante absolve “só carimbo” em `:109`, `:113`. Isso conflita com o corte novo do anexo e não prova a saúde do sentido timestamp→texto.

B2 — Leitores diretos runtime de `respostas_sem_veredito`:

| Arquivo:linha | Uso |
|---|---|
| `app/chamados/models.py:1869` | Parte do juiz `pode_fechar`. |
| `app/chamados/models.py:1902` | Guarda de **todas** as portas de `fechar`, inclusive exceções. |
| `app/chamados/reconciliador.py:1164` | Segura disputa e relata perguntas no fecho das órfãs pela lâmpada. |
| `app/chamados/services/fila_por_causa.py:226` | Gaveta `respondido_a_decidir`. |
| `app/chamados/services/competencia_trancada.py:114` | `ignorar_tranca=True` para pauta do DP. |
| `app/colaboradores/services/ficha.py:169` | Conta respostas sem veredito restritas a perguntas vivas da ficha; saída `:246`. |
| `app/colaboradores/services/desligamento.py:141` | Pula fechamento automático da disputa na cascata de desligamento. |
| `app/chamados/management/commands/validar_classe_a.py:54` | Compõe conjunto para classificação/validação assinada. |

Corpo atual divergente: validação/materialização nulas e `(respondida_em não nulo OU resposta_colab.strip())`, excetuando tranca salvo parâmetro: `app/chamados/models.py:1857`–`:1861`. `pode_fechar` combina essa lista com `fio_vivo` real: `:1867`–`:1869`.

Leitores diretos runtime de `pode_fechar`:

- `app/chamados/reconciliador.py:412`, `:612`, `:1171`.
- `app/chamados/services/becos.py:33` (classificação), `:56` (fecho).
- `app/chamados/services/competencia_trancada.py:154` (medição), `:203` (aplicação).
- `app/chamados/services/explicador.py:139` (retorna booleano na explicação em `:165`).
- `app/chamados/services/disputa_emissao.py:1278`, `:2091`.
- `app/chamados/services/materializacao.py:591`, `:1322`.
- `app/ponto/services/entrada_sincronizada.py:59`.

Leitores indiretos da saída do explicador: `app/chamados/services/desenlace.py:63`; template `app/templates/chamados/partials/modal_fio.html:435`, `:446`. Registro de autoridade, não chamada: `app/core/juizes.py:283`.

Não há banco/log disponível nesta tarefa; nenhum escritor histórico responsável por perguntas reais pode ser identificado só pela possibilidade estática `[nao verificado]`.

## 12/11 14:00 · sessao d8d8ef0c0ef6 · - · - · origem=Ronald (tmux)

C2 pode embutir a b4 sem migration: `q_silenciado_em` apenas devolve `Q(silenciado_ate__gt=agora)`, e o leitor novo o usa no `exclude`; não introduz campo nem consulta de escrita. `app/chamados/models.py:515-524`; `app/chamados/services/janela_colab.py:74-79`.

A coorte histórica é de **12 vencidos em COBRANCA**, não simplesmente todo chamado com data antiga de silêncio. O corte está em `app/docs/CORTES.md:335`. A contagem real existente é `particionar_verbos`, cujo ramo `HISTORICO` lê silêncio vencido; o painel chama essa partição sobre `qs_vivos`. `app/chamados/services/verbos.py:30-62`; `app/chamados/services/painel.py:230`.

Use no script:

```python
part = particionar_verbos(qs_vivos(), agora=agora)
ids_historico = {
    pk for pk, verbo in part["mapa"].items()
    if verbo == HISTORICO
}
ids_coorte = set(
    qs_na_fila(agora=agora)
    .filter(pk__in=ids_historico)
    .values_list("pk", flat=True)
)
```

Isso chama a partição verdadeira e a fila verdadeira, sem reimplementar comparação de silêncio/status. `app/chamados/services/verbos.py:47-51`; `app/chamados/services/painel.py:188-190`. A igualdade entre a coorte atual e os 12 históricos continua `[nao verificado]`; imprimir contagem atual e divergência de 12.

Para simular a b4 fielmente no MAIN, embutir literalmente em string:

- `app/chamados/models.py:515-524`, removendo apenas a indentação de classe com `textwrap.dedent`, e instalar a função com `patch.object(ChamadoColaborador, "q_silenciado_em", staticmethod(...), create=True)`.
- `app/chamados/services/janela_colab.py:30-37`, o conjunto `MODULOS_INTERNOS`.
- `app/chamados/services/janela_colab.py:40-46`, `janela_do_colab`.
- `app/chamados/services/janela_colab.py:49-79`, **a função inteira**, inclusive docstring e comentários.

Executar as três últimas fatias em namespace isolado `{"timezone": timezone}`. Assim o ensaio não pressupõe igualdade entre helper/constante do MAIN e da cópia. Essa função retorna queryset e só lê o banco. `app/chamados/services/janela_colab.py:65-79`.

Antes de instalar o método novo, calcular a visibilidade com a função real importada do MAIN. Durante a chamada b4, instalar temporariamente o método e chamar a função compilada com `hoje=timezone.localdate(agora), agora=agora`. Fixar `django.utils.timezone.now` em `agora` também durante chamadas legadas sem argumento; nenhuma segunda chamada solta ao relógio é necessária. `app/chamados/services/janela_colab.py:42`; `app/chamados/services/janela_colab.py:78`; `app/chamados/services/verbos.py:35`.

Para cada chamado da coorte, imprimir apenas `colNNN/#pk`, visibilidade MAIN, visibilidade b4 e `isento_ponto(ch.colaborador)`. O juiz real de isenção é `app/core/constants.py:13-18`. Contar separadamente:

- total da coorte;
- visíveis no MAIN;
- visíveis na b4;
- novos visíveis `b4 - MAIN`;
- isentos da coorte;
- isentos entre visíveis b4;
- isentos entre novos visíveis.

A b4 chama deliberadamente `q_vivas()`, que é um **superconjunto** do juiz exato; substituir por `pergunta_viva` falsearia o comportamento sob medição. `app/chamados/services/janela_colab.py:65-69`; `app/chamados/models.py:3352-3359`.

Os testes existentes cobrem retorno às duas filas com silêncio vencido, exclusão com vigente, inclusão sem silêncio, respeito ao relógio recebido e igualdade entre método/filtro de silêncio. Não executei a suíte. `app/chamados/tests/test_e3_b4_silencio_vencido.py:41-65`; execução `[nao verificado]`.

Há fontes locais úteis para a matriz A2: o corte posterior manda **abrir/fechar** a janela quando muda a flag, estabelece admissão como início sem trilha e cobre item sem dia apenas em janela aberta. `app/docs/CORTES.md:352`. O toque administrativo desarquiva apenas `(colab,dia)` e não tranca nada. `app/docs/CORTES.md:349`.

## 12/11 13:35 · sessao 0746de58f99f · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Curas implementadas nos três sítios:

| Sítio | Alteração | RED → GREEN |
|---|---|---|
| S1 | Órfãs casadas com a data da tela — [PDF:478](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/pdf_espelho.py:478) | [rc=1 → rc=0](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c17_execucoes.r:41) |
| S2 | Anomalias filtradas por DD/MM HH:MM — [PDF:682](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/pdf_espelho.py:682) | [rc=1 → rc=0](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c17_execucoes.r:49) |
| S3 | Supressão pelo instante completo — [PDF:926](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/pdf_espelho.py:926) | [rc=1 → rc=0](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c17_execucoes.r:57) |

O censo AST passou de seis nós sem data para zero. Cada mutante S1/S2/S3 derrubou o teste correspondente e o selo; comandos e saídas estão no [relato:65](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c17_execucoes.r:65). [nao verificado]

`marca_espuria` voltou a exigir zero no PDF e na tela — [teste:162](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/tests/test_cartao_marca_espuria.py:162). A catraca registra `[1040]`, com motivo — [registro:321](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/core/tests/test_medida_turno_do_par.py:321). [nao verificado]

Passaram 7 testes novos, 12 de regressão, 35 da catraca e ruff; comandos no [relato:130](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c17_execucoes.r:130). [nao verificado]

Fixture ORM: PDF e tela **[nao verificado]**; não houve acesso a banco. Certificação Code/opus permanece pendente — [limites:118](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c17_execucoes.r:118). Sem commit, pela proibição explícita de escrita em `.git` — [relato:7](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c17_execucoes.r:7). [nao verificado]

## 12/11 13:34 · sessao 73e7fb13d78d · DECLARADO-X-GRAVADO · censo · origem=Code

**A — Escritores e transformações**

A S139 continua substituindo a resposta pelo marco para ambos os motivos de intervalo. O corpo não verifica “intervalo fixo”, distância da resposta ou pertencimento ao envelope antes da substituição. (`app/chamados/services/materializacao.py:409-426`)

| Sítio | Entrada → valor associado/gravado | Caso protegido e teste | Sustentação | [nao verificado]
|---|---|---|---|
| S139, `_ts_tipo_do_plantio` | `resposta_colab` → hora de `_marco_faltante`, quando encontrado; preserva o texto da resposta. (`app/chamados/services/materializacao.py:412-419`) | Volta declarada `02:00`, marco `03:00`: exige `E@03:00` e rejeita `E@02:00`. (`app/chamados/tests/test_s139.py:14-18`; `app/chamados/tests/test_s139.py:40-56`) | **SEM LEI específica localizada** nos três documentos pesquisados. [nao verificado] L-032 reconhece intervalo fora do horário exato dentro do envelope; não enuncia substituição da declaração. (`app/docs/LEIS.md:80`) |
| `_tipo_pelo_marco` | Tipo inicialmente inferido pelo motivo → tipo do marco cobrado; fallback para marco próximo da ATA, até 240 minutos. O snapshot cobrado prevalece na divergência. (`app/chamados/services/materializacao.py:246-300`; `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:390-407`) | `orfao_14h` com hora de entrada gerava saída; teste exige entrada e cobre o sentido inverso. (`app/chamados/tests/test_hx_tipo_pelo_marco.py:86-119`) | Corte de 31/08 declarado no código; registro específico em LEIS/CORTES/DOSSIES: **SEM LEI localizada**. (`app/chamados/services/materializacao.py:247-262`) [nao verificado] |
| Tipo sem marco identificável | Mantém o tipo inferido pelo motivo e segue o plantio com `pendencia_admin` e `recusa_motivo`. (`app/chamados/services/materializacao.py:392-407`; `app/chamados/services/materializacao.py:851-859`) | Sem régua ou marco próximo, planta pelo motivo e deixa pendência. (`app/chamados/tests/test_hx_tipo_pelo_marco.py:121-142`) | “Caminho C”, corte de 31/08 declarado no corpo; registro específico nos três documentos: **SEM LEI localizada**. (`app/chamados/services/materializacao.py:393-405`) [nao verificado] |
| Data e `_ajustar_saida_overnight` | Combina hora com `data_evento`, fallback contexto do motivo; depois escolhe D ou D+1 pelo envelope da cadeia prevista, ±90 minutos. Fora dele, ou sem marcos, permanece D. (`app/chamados/services/materializacao.py:345-353`; `app/chamados/services/materializacao.py:418-426`; `app/chamados/juizes.py:296-331`) | Saída do turno partido permanece D; volta noturna `00:00` vai a D+1; sem marcos não recua para D−1. (`app/chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:59-91`; `app/chamados/tests/test_plantio_noturno_mais1.py:78-117`) | L-085: dia do turno é o dia do início. O ajuste específico declara cortes de 15/09 no código. (`app/docs/LEIS.md:117`; `app/chamados/juizes.py:297-309`) |
| Reuso S137 | Instante derivado → primeira batida por timestamp do mesmo colaborador/tipo em ±20 minutos. Preserva os valores existentes e anota `pergunta_origem`. (`app/chamados/juizes.py:235-247`; `app/chamados/services/materializacao.py:865-876`; `app/chamados/services/materializacao.py:952-956`) | Evita duplicar saída noturna já existente; teste exige contagem inalterada. (`app/chamados/tests/test_materializar_overnight.py:48-71`) | Propósito declarado no juiz; tolerância de ±20 minutos: **SEM LEI específica localizada** nos três documentos. (`app/chamados/juizes.py:235-238`) [nao verificado] |
| Deduplicação da porta | Mesmo tipo vivo em ±60 segundos → existente; mesma chave colaborador/instante → existente, inclusive outro tipo ou retratada. Pode devolver valores diferentes do tipo/instante solicitado, sem alterá-los. (`app/ponto/registro_batida.py:106-122`) | Reenvio da mesma marca já reclassificada causava conflito de unicidade; caso documentado e testado. (`app/ponto/registro_batida.py:116-122`; `app/ponto/tests/test_bater_mesma_chave_idempotente.py:1-6`) | Corte de 14/09 declarado no corpo; registro específico nos três documentos: **SEM LEI localizada**. (`app/ponto/registro_batida.py:116-119`) [nao verificado] |
| Ramo `data_hora` | Data + `HH:MM[:SS]` → data e HH:MM, descartando segundos; sem componente de hora completa `00:00`; tipo solicitado sempre `E`. (`app/chamados/services/materializacao.py:1001-1014`) | Teste fundador da truncagem/default: [nao verificado]. O teste localizado protege colisão da âncora com marca oposta. (`app/chamados/tests/test_chokepoint_ancora.py:8-11`; `app/chamados/tests/test_chokepoint_ancora.py:24-44`) | Truncagem e preenchimento `00:00`: **SEM LEI específica localizada**. [nao verificado] |

No ramo `hora`, o colaborador não fornece um campo de tipo: o tipo vem das inferências acima. Assim, a divergência de tipo identificada é entre motivo e marco, enquanto a divergência de hora pode ser diretamente resposta versus plantio. (`app/chamados/models.py:2443-2449`; `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:390-419`)

A O306 mantém a substituição `13:05` → `13:00`; protege contra escolher o marco de outro motivo. (`app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:136-153`)

Validação individual, lote, regularização da disputa e reparo do passivo convergem para o materializador; os dois ramos criadores passam `timestamp` e `tipo` à porta, que os grava sem outra transformação no `create`. (`app/chamados/services/acoes_disputa.py:13-22`; `app/chamados/services/validacao.py:461-469`; `app/chamados/services/validacao.py:735-746`; `app/chamados/services/acoes_disputa.py:118-139`; `app/chamados/services/passivo_validado.py:514-516`; `app/ponto/registro_batida.py:152-159`)

A regularização externa é uma entrada adjacente: usa itens da supervisão confirmados pelo colaborador, sem ler `PerguntaDisputa.resposta_colab`. No intervalo, ordena as horas e atribui S à menor/E à maior; o teste protege campos preenchidos na direção contrária. Na correção, prioriza `hora_saida`, senão `hora_entrada`, combina com `item.data` e altera diretamente timestamp/tipo, sem ajuste overnight nesse ramo. Lei específica para essa ordenação/prioridade: **SEM LEI localizada**. (`app/chamados/services/regularizacao_ext.py:1-19`; `app/chamados/services/regularizacao_ext.py:143-179`; `app/chamados/tests/test_reg_intervalo.py:11-14`; `app/chamados/tests/test_reg_intervalo.py:28-45`) [nao verificado]

O corte “LEI-DO-MIOLO-CEDE-A-LAMPADA” determina que resposta validada vire registro do intervalo; não especifica a substituição da hora respondida pelo marco. (`app/docs/CORTES.md:273`)

**B — Telas e rotas**

“Previsto” na tabela abaixo não significa automaticamente “instante que será gravado”: o lote obtém o previsto por `delta_da_resposta`, enquanto a derivação do plantio é outra chamada. (`app/chamados/services/validacao.py:864-877`; `app/chamados/services/materializacao.py:409-426`)

| Superfície | View/rota e template | Hora respondida / futura; leitor |
|---|---|---|
| Modal do fio | Rota `modal/<id>/`; view `modal_fio_colaborador`. (`app/chamados/urls.py:39`; `app/chamados/views.py:869`; `app/chamados/views.py:927`) Template: `app/templates/chamados/partials/modal_fio.html:219`. | Mostra resposta e, no alerta, previsto. Calcula `estado_aplicacao_pergunta`, mas desenha apenas detalhes de paridade/reuso; não desenha `aplicavel`, que contém a hora futura. (`app/chamados/services/fio.py:335-338`; `app/templates/chamados/partials/modal_fio.html:230-246`; `app/templates/chamados/partials/modal_fio.html:258-284`; `app/chamados/services/materializacao.py:463-465`) |
| Detalhe local, cartão da pergunta | Rota `local/<id>/`; view e inclusão do cartão. (`app/chamados/urls.py:14`; `app/chamados/views.py:483-501`; `app/templates/chamados/detalhe_local.html:307-310`) | Mostra `resposta_display`, evidência e resposta versus previsto; não apresenta derivação do plantio. Lê `leitura_da_resposta` → `classificar_resposta_validacao`. O alerta afirma que “este horário” será gravado, sem consultar o derivador nessa apresentação. (`app/templates/chamados/partials/_pergunta_card.html:29-54`; `app/chamados/templatetags/chamados_tags.py:144-152`) |
| Central, ✓ inline | Rota `validar-inline/<id>/`; view `validar_inline`. (`app/chamados/urls.py:33`; `app/chamados/views_cobrar.py:145-169`) Template: `app/templates/chamados/partials/_lista_chamados.html:79-82`. | O controle não apresenta resposta nem hora futura. Seleciona `perguntas_vivas_de` e chama `acao_validar_pergunta`, sem leitura de `estado_aplicacao_pergunta` nesse caminho. (`app/chamados/views_cobrar.py:168-169`; `app/chamados/views_cobrar.py:201-208`; `app/templates/chamados/partials/_lista_chamados.html:79-82`) |
| Central, drawer de lote | Rotas prévia/tela e views. (`app/chamados/urls.py:36-37`; `app/chamados/views_cobrar.py:353-386`) Template: `app/templates/chamados/partials/_lote_preview.html:21-26`. | Mostra resposta, previsto e diferença; não a derivação futura. Usa `fila_validar_classificada`/`classe_do_lote`; `_slot_esperado` serve à verificação de conflito. (`app/templates/chamados/partials/_lote_preview.html:21-26`; `app/chamados/services/validacao.py:864-877`) |
| Copiloto, proposta de lote | Rotas de proposta/validação; views. (`app/chamados/urls.py:34`; `app/chamados/urls.py:38`; `app/chamados/views_cobrar.py:232-302`) Template: `app/templates/core/_copiloto.html:236-250`. | Mostra amostra de resposta e previsto, obtidos da classificação do lote; não consulta `estado_aplicacao_pergunta`. (`app/chamados/views_cobrar.py:287-293`; `app/templates/core/_copiloto.html:236-250`) |
| Modal, aplicar declaração em texto | Preparação e aplicação da proposta. (`app/chamados/services/fio.py:291-299`; `app/chamados/services/declaracao_texto.py:641-652`) Template: `app/templates/chamados/partials/modal_fio.html:126-143`. | Mostra hora extraída, tipo, dia e marco; depois grava a resposta e chama `validar_pergunta`. Não calcula ali a hora efetiva do materializador. (`app/templates/chamados/partials/modal_fio.html:129-143`; `app/chamados/services/declaracao_texto.py:687-696`) |
| Fio completo | Rota `colaborador/<id>/`; delega POST ao processador comum. (`app/chamados/urls.py:21`; `app/chamados/views.py:667-677`) | Aceita o processamento comum de validação, mas seu HTML apresenta descrição, followups e caixa de resposta, sem os cartões de validação do modal. (`app/chamados/services/fio.py:585-609`; `app/templates/chamados/fio_colaborador.html:229-260`) |

“Regularizar disputa” também está no detalhe e modal: aciona fechamento/materialização, sem apresentar uma previsão individual adicional de plantio. (`app/templates/chamados/detalhe_local.html:314-326`; `app/templates/chamados/partials/modal_fio.html:353-359`; `app/chamados/views.py:467-475`; `app/chamados/services/fio.py:646-656`)

**C — Trilha atual**

`_log_auditoria_materializacao` envia ação, modelo `Batida`, PK, descrição, `valor_depois` e ator para `evento`, em regime best effort. (`app/chamados/services/materializacao.py:731-737`)

| Registro | Campos |
|---|---|
| `materializar_batida` | `batida_pk`, `colaborador_id`, `timestamp`, `tipo`, `origem`, `disputa_pk`, `chamado_pai_pk`, `chamado_destino_pk`, `chamados_relacionados_ids`, `pergunta_pk`, `resposta_colab`. (`app/chamados/services/materializacao.py:966-978`) |
| `materializar_ancora` | `batida_pk`, `colaborador_id`, `timestamp`, `tipo`, `origem`, `disputa_pk`, `chamado_destino_pk`, `pergunta_pk`, `resposta_colab`. (`app/chamados/services/materializacao.py:1053-1063`) |
| `anotar`, vínculo da pergunta | Ação `vincular_pergunta_origem`, antigo/novo `pergunta_origem`, descrição com PKs e ator; se já coincide, não grava novamente. (`app/chamados/services/materializacao.py:952-956`; `app/ponto/registro_batida.py:235-263`) |
| Envelope da auditoria | `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `ip`, `ator`, `criado_em`. (`app/core/models.py:295-307`) |

**Não registra qual regra substituiu a hora nem qual marco foi usado.** Registra resposta e instante derivado. No reuso, o mesmo log usa `timestamp=ts` derivado e origem literal de disputa, embora esses valores possam diferir dos da Batida existente; portanto o payload não é prova do instante efetivamente persistido na batida reusada. (`app/chamados/services/materializacao.py:865-876`; `app/chamados/services/materializacao.py:957-978`)

**D — Script dos itens 2–6**

Há um limite de identificação: `pergunta_origem` pode ser reassociada, e o reuso aceita inclusive batida de disputa. A porta também devolve existente sem retornar `created`. Portanto **origem de disputa não prova criação por aquela pergunta**. (`app/chamados/services/materializacao.py:863-876`; `app/chamados/services/materializacao.py:952-956`; `app/ponto/registro_batida.py:94-122`)

O script usa o critério operacional por origem pedido: relação única com `origem='disputa_s84_retro'` → `batida_nova`; outra origem → `reuso`. A soma dessa partição é verificável; a separação **causal** nova/reuso de disputa permanece [nao verificado]. Relações múltiplas/inconsistentes ficam em `OUTRO`. (`app/ponto/models.py:40-45`; `app/chamados/services/materializacao.py:933-956`)

Os demais critérios são os campos reais: via de paridade ou `traduzir_recusa(...)=PARIDADE`; pendência ou recusa não terminal; ausência de `validada_em`. Plantio/reuso precedem pendência porque ela pode coexistir com batida. O universo usa `respondida_em`, pois `p.estado == RESPONDIDA` excluiria validadas. (`app/chamados/services/materializacao.py:853-857`; `app/chamados/services/materializacao.py:906-930`; `app/chamados/services/materializacao.py:1089-1109`; `app/chamados/catalogo/recusa.py:77-88`; `app/chamados/models.py:3042-3049`)

Para datar a resposta, combina sua HH:MM com o dia do turno e chama **somente** `_ajustar_saida_overnight`, alimentado com DNA histórico. Plantio é `Batida.criado_em`; instante marcado é `Batida.timestamp`. Se houve regeneração posterior, seleciona `CelulaDia.dna_anterior`; múltiplas regenerações ou edição posterior deixam a reconstrução não verificada, sem fallback para template atual. (`app/chamados/juizes.py:296-331`; `app/ponto/models.py:23`; `app/ponto/models.py:79`; `app/escala/models.py:1714-1732`; `app/escala/models.py:1124-1133`)

A TRIAGEM usa `turnos_do_colab` e `_carimbada_como_miolo`: `is_intervalo`, `_intra_dur` ou `_intra_ata`; filtra origem `app` e confirma o vínculo do dia. Compara a menor distância de cada instante ao conjunto dessas marcas. É leitura da geometria atual, não reconstrução histórica do pareamento. (`app/ponto/turnos.py:932-940`; `app/ponto/turnos.py:1850-1866`; `app/ponto/turnos.py:1915-1939`; `app/escala/servico_jornada.py:15-66`)

Carga semanal significa uma validação atual por pergunta, datada por `validada_em`; não todos os atos históricos de revalidação. Estes têm outra representação, `DecisaoPergunta`. Os tops distinguem PK do modelo de escala e PK do vínculo, ambos presentes no DNA. (`app/chamados/models.py:2450`; `app/chamados/models.py:3840-3850`; `app/chamados/models.py:3872-3876`; `app/ponto/management/commands/gerar_celulas.py:95-100`)

Execução Django, cobertura histórica e números dos itens 2–6: [nao verificado].

```python
from collections import Counter, defaultdict
from contextlib import redirect_stdout, redirect_stderr
from copy import deepcopy
from datetime import date, datetime, time, timedelta
from io import StringIO
from types import SimpleNamespace
import logging
import re

from django.db import connection
from django.db.models import Prefetch
from django.utils import timezone
from chamados.models import PerguntaDisputa
from chamados.catalogo.recusa import traduzir_recusa
from chamados.juizes import _ajustar_saida_overnight
from escala.models import CelulaDia, EscalaColaborador
from escala.alimentacao import vinculo_do_dia
from escala.servico_jornada import escala_vigente
from ponto.models import Batida
from ponto.turnos import turnos_do_colab, _carimbada_como_miolo

INICIO = date(2026, 9, 21)
MOTIVOS = ("intervalo_saida", "intervalo_volta")
DESFECHOS = (
    "batida_nova", "reuso", "recusa_paridade",
    "pendente_admin", "nao_validada", "OUTRO",
)
ETAPAS = (
    "consulta", "classificar", "data", "celula", "item5",
    "semana", "dna", "data_resposta", "oposto", "triagem",
    "item", "sql",
)
NV = object()
erros = Counter({k: 0 for k in ETAPAS})
desfechos = Counter({k: 0 for k in DESFECHOS})
outros, avisos, item5 = Counter(), Counter(), Counter()
medidas = defaultdict(Counter)
semanas = defaultdict(Counter)
top_col, top_escala, top_vinculo = Counter(), Counter(), Counter()
cache_cel, cache_ec, cache_reais = {}, {}, {}

def tentar(etapa, fn):
    antes = erros["sql"]
    try:
        valor = fn()
        if erros["sql"] != antes:
            erros[etapa] += 1
            return NV
        return valor
    except Exception:
        erros[etapa] += 1
        return NV

def apenas_select(execute, sql, params, many, context):
    s = str(sql).strip()
    corpo = s[:-1] if s.endswith(";") else s
    if (not re.match(r"(?is)^SELECT\b", s) or ";" in corpo
            or re.search(
                r"(?is)\bFOR\s+(UPDATE|SHARE)\b|\bINTO\b", corpo)):
        erros["sql"] += 1
        raise RuntimeError("SQL_NAO_SELECT")
    return execute(sql, params, many, context)

def dia_de(p):
    # app/chamados/services/materializacao.py:345-353
    if p.data_evento:
        return p.data_evento
    texto = ((p.disputa.contexto or {}).get(p.motivo) or {}).get("data")
    return date.fromisoformat(texto) if texto else None

def celula(cid, dia):
    k = (cid, dia)
    if k not in cache_cel:
        cache_cel[k] = CelulaDia.objects.filter(
            colaborador_id=cid, data=dia).first()
    return cache_cel[k]

def ec_por_pk(pk):
    if pk not in cache_ec:
        cache_ec[pk] = EscalaColaborador.objects.filter(
            pk=pk).select_related("tipo_escala").first()
    return cache_ec[pk]

def classificar(p):
    # app/ponto/models.py:40-45
    # app/chamados/services/materializacao.py:863-876,952-956
    bs = p._b_censo
    if len(bs) > 1:
        return "OUTRO", None, "multiplas_relacoes"
    if bs:
        b = bs[0]
        if b.colaborador_id != p.disputa.colaborador_id:
            return "OUTRO", None, "colaborador_divergente"
        # Criterio por ORIGEM; criacao causal por p NAO VERIFICADA.
        if b.origem == "disputa_s84_retro":
            return "batida_nova", b, ""
        return "reuso", b, ""
    if (p.via_resolucao == PerguntaDisputa.VIA_PARIDADE_RECUSADA
            or (not p.materializada_em
                and traduzir_recusa(p.recusa_motivo)[0] == "PARIDADE")):
        return "recusa_paridade", None, ""
    # app/chamados/services/materializacao.py:1089-1109
    if (p.pendencia_admin
            or (not p.materializada_em and p.recusa_motivo)):
        return "pendente_admin", None, ""
    if p.validada_em is None:
        return "nao_validada", None, ""
    return "OUTRO", None, "validada_sem_relacao_nem_recusa"

def historico(c, cid, dia, instante):
    # app/escala/models.py:1697-1732
    # app/ponto/portas/celula.py:263-277
    if c is None:
        return None, "sem_celula"
    anterior = bool(c.regenerada_em and c.regenerada_em > instante)
    dna = deepcopy(c.dna_anterior if anterior else c.dna)
    if c.gerada_em > instante:
        return None, "celula_posterior_ao_plantio"
    if c.editada_em and c.editada_em > instante:
        return None, "edicao_posterior_ao_plantio"
    if anterior and c.regeneracoes != 1:
        return None, "historico_multiplas_regeneracoes"
    if not isinstance(dna, dict) or not isinstance(dna.get("marcos"), dict):
        return None, "dna_sem_marcos"
    if not dna["marcos"].get("hi") or not dna["marcos"].get("hf"):
        return None, "dna_sem_bordas"
    # Sem fallback para marcos do template atual.
    ev = ec_por_pk(dna.get("vinculo_id"))
    if ev is None or ev.colaborador_id != cid or ev.tipo_escala is N

[truncado em 20000 de 29952 caracteres]

## 12/11 13:33 · sessao 3ddb216d4271 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**O contrato v2 está errado como está: troca a fonte do topo, mas não especifica a troca de todos os juízes da fila; além disso, sua fórmula de andáveis e seu selo contradizem o RED que o aval manda manter.** (`contrato_item3_v2.txt:3`, `:6`; `aval_respostas_1211_1230.txt:4`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:59`, `:72`)

Nas referências aos dois anexos, numerei as linhas do **texto fornecido**, começando pelo título como linha 1. Não são referências a arquivos localizados nesta cópia. A análise abaixo trata o BACKLOG como evidência documental; pousos e medidas de produção nele declarados não foram confirmados em banco ou log. [nao verificado]

**1. Censo**

Na coluna “efeito”, confronto cada leitor com a determinação de retirar marcador e coluna da condição de fonte. (`aval_respostas_1211_1230.txt:4`; `contrato_item3_v2.txt:2`)

| Sítio | Pergunta ou escrita atual | Efeito da fonte nova |
|---|---|---|
| `bin/gerar_backlog.py:110`, `:119`, `:127` | Lê e republica a janela antiga desde o marcador até `OBRAS:FIM`, incluindo a prosa intermediária. | Precisa distinguir conteúdo manual preservado de publicação derivada; copiar essa janela integralmente pode recolocar marcador/lista antigos. |
| `bin/gerar_backlog.py:131`, `:138` | Exige presença dos textos do marcador e de `OBRAS:INICIO`; escreve BACKLOG por `.novo` e `os.replace`. | A guarda de presença não certifica igualdade com a fonte estruturada. |
| `bin/hook_stop_fila1.py:149`, `:171` | `_item_fechado` decide pela coluna de estado ou tachado na descrição; `_item_parado` acrescenta congelamento, fila 2, espera, PAREI e portão. | Esses predicados precisam deixar de decidir pertencimento e andabilidade da ORDEM. |
| `bin/hook_stop_fila1.py:192`, `:209`, `:225` | `_item_da_ordem_viva`: qual é o topo? Extrai marcador, procura o id no bloco OBRAS e veta pela coluna. | Trocar apenas a extração do marcador deixa o veto da coluna funcionando como segunda fonte. |
| `bin/hook_stop_fila1.py:231`, `:250`, `:256` | `_itens_andaveis` enumera todos os não parados de OBRAS; `_ha_item_aberto` decide fila vazia por essa lista. | Ambos precisam migrar; mudar somente `_andaveis_fora_do_em_curso` não resolve o vazio. |
| `bin/hook_stop_fila1.py:270`, `:276` | Quais e quantos andam além do topo? O recado manda buscar o próximo no adendo. | A contagem muda com a exclusão do ruído; a orientação ainda levaria o humano à prosa se permanecer. |
| `bin/hook_stop_fila1.py:290`, `:312` | Diagnostica marcador quebrado e oferece o item em curso aos demais leitores. | Diagnóstico precisa passar a “fonte quebrada”; o topo só permanece igual se o primeiro id estruturado for o mesmo. |
| `bin/hook_stop_fila1.py:323`, `:344` | `_ids_que_nao_andam`: quais ids estão parados pela coluna? | Continua juiz ativo: essa lista veta o PAREI em `:1298`. |
| `bin/hook_stop_fila1.py:349`, `:378`, `:380` | `travas_da_fila`: quais obras não fechadas têm palavra de trava na coluna? | Não pode continuar sustentando a conclusão de “fila inteira travada” pela prosa. |
| `bin/hook_stop_fila1.py:1232`, `:1245` | `_relato_parou`: PAREI do em curso devolve turno somente sem outro andável. | A resposta muda quando se retiram linhas abertas fora da ORDEM. |
| `bin/hook_stop_fila1.py:1248`, `:1298`, `:1310` | `_parei_do_em_curso`: reconhece `!`, rejeita ids parados pela coluna e compara com o topo. | Também precisa migrar o veto por coluna, não apenas a obtenção do topo. |
| `bin/hook_stop_fila1.py:1391`, `:1400` | Sem topo válido, decide entre fila vazia e marcador quebrado consultando OBRAS. | Precisa distinguir fonte válida vazia de fonte inválida usando a nova fonte. |
| `bin/handoff_sessao.sh:54`, `:64`, `:71`, `:120` | Publica topo, em curso, trava do item e devolução do turno, perguntando ao hook. | Herda a migração do juiz; rótulos e respostas sobre PAREI também mudam. |
| `bin/handoff_sessao.sh:163`, `:171`, `:209`, `:216` | Perguntas distintas: avais de hoje não cumpridos e linhas abertas da raia20, pelo fechamento da coluna. | Podem continuar como informação documental, mas não como exclusão de pousadas ou definição da ORDEM. |
| `bin/handoff_sessao.sh:290`, `:294`, `:299`, `:314` | Sem próximo, mostra lista travada e afirma que o turno se devolve; descreve próximo como primeiro aberto de OBRAS ou manda conferir a ordem à mão. | Precisa mudar: topo inválido não significa fila inteiramente travada; os textos ainda encaminham à autoridade antiga. |
| `bin/disjuntor.py:1671`, `:1682` | `_fila1_viva`: quais obras exigem censo? Obtém todos os andáveis de OBRAS. | Universo muda para o estruturado; linhas abertas fora dele deixam de integrar essa lista. |
| `bin/disjuntor.py:1720`, `:1736` | `_pertence_fila1`: esta obra pertence à fila? Usa token e nomeação inteira, com recusa de ambiguidades. | Contrato precisa definir identidade exata dos ids, inclusive qualificações e aliases. |
| `bin/disjuntor.py:1746`, `:1755`, `:2355` | Aplica ou dispensa o censo conforme pertencimento; CLI usa a mesma pergunta. | Herda a mudança de universo. “Não muda de resposta” não vale para itens retirados da lista. |
| `bin/codex_pergunta.sh:228` | Chama o censo antes de obter a vaga. | Leitor indireto; deve continuar delegando ao disjuntor. |
| `bin/backlog_estado.py:56`, `:57`, `:59`, `:62` | Escreve estado e compara próximo/ids parados antes e depois. | Escrever coluna precisa deixar de mudar a ORDEM. Hoje o script **escreve e informa**, apesar da docstring dizer que para quando muda o veredito (`:8`, `:66`). |
| `bin/interrompidas_estado.py:89`, `:93`, `:105`, `:121`, `:125` | Lê INTERROMPIDA/FUNDIDA; pergunta topo ao hook; considera retorno por topo, fusão ou fechamento. | Topo migra indiretamente; fechamento documental continua pela coluna, se mantido. Não pode definir a exclusão estruturada de pousadas. |
| `bin/gerar_estado.py:236`, `:241` | Publica interrompidas e topo do medidor. | Leitor indireto; diagnóstico de marcador em `:243` precisa acompanhar a fonte nova. |
| `bin/alarme_sessao_ociosa.py:89`, `:96`, `:113` | **Parser próprio:** conta linhas de `## OBRAS`, excluindo ☑/FEITO/FECHAD na linha inteira. | Continua segundo juiz se não migrar: essa contagem decide fila vazia e disparo (`:40`, `:148`). |
| `bin/placar_code.sh:368` | Publica o alarme de sessão. | Herda o leitor anterior. |
| `bin/fabricante_alvo.py:238`, `:251`, `:273`, `:279` | **Parser próprio:** escolhe O/F por texto e coluna — dinheiro, só registrar, Ronald/aval/corte/smoke, livre e fechado. | Continua lendo prosa e escolhendo “item da ORDEM” se não migrar. O contrato precisa delimitar essa pergunta. |
| `bin/fabricante_alvo.py:315`, `:320`; `bin/fabricante.sh:129` | Tenta livres e depois ORDEM escalonada; fabricante chama o seletor. | Leitores indiretos do seletor anterior. |
| `bin/gerar_backlog_topo.py:16`, `:26`, `:30`, `:39` | Produz resumo de O-números abertos, por frases e ordenação numérica. | É pergunta diferente da ORDEM VIVA; não pode passar a ser consumido como fila 1. |
| `bin/relato.sh:35`, `:115`, `:116` | Gera o resumo e publica BACKLOG completo. | Transporta a publicação derivada. Seu comentário ainda declara que a ORDEM se lê na tabela completa (`:7`). |
| `.claude/settings.json:9`, `:20`, `:29`, `:40`, `:53`, `:68` | Stop chama hook; PreCompact chama handoff; SessionStart lê handoff; PreToolUse chama disjuntor. | São entradas/leitores indiretos. Apontam para `/home/ronald/saas-hasner`; execução desta cópia nesse caminho é [nao verificado]. |

Os selos também fazem parte da migração:

| Selo em `bin/tests/` | Pergunta/fixture atual e efeito |
|---|---|
| `test_hook_andavel_e_a_ordem_viva.sh:34`, `:51`, `:73` | Escreve marcador, ordem em prosa, estados, POS-20 e ruído; exige liberar com restante travado e bloquear com restante aberto. Precisa representar os dois casos na fonte estruturada. |
| `test_hook_parei_com_andavel.sh:25`, `:42`, `:56` | Alterna restante aberto/travado pela coluna para exigir bloquear/liberar. |
| `test_hook_parei_trava_o_item_em_curso.sh:20`, `:40`, `:53` | Distingue PAREI do topo de PAREI de outro item; fixtures precisam migrar. |
| `test_hook_parei_casa_id_inteiro.sh:21`, `:42`, `:57` | Escreve marcador e congeladas; preserva a pergunta de identidade inteira, mudando a fonte do congelamento. |
| `test_hook_le_o_marcador.sh:39`, `:56`, `:74`, `:92` | Exige autoridade/falhas do marcador e vazio pela tabela. Essas expectativas precisam virar autoridade/falhas/vazio da fonte estruturada. |
| `test_hook_estado_e_a_coluna.sh:41`, `:51`, `:59`, `:80` | Exige estado/tachado como veto do topo e examina BACKLOG real. Essa expectativa sobre a ORDEM conflita com o aval novo. |
| `test_hook_nao_cobra_congelado.sh:12`, `:111`, `:115`, `:135`, `:168` | Lê o predicado de coluna, valida marcador real e escreve fixtures. Exigir marcador como fonte deixa de valer. |
| `test_interrompidas_estado.sh:37`, `:60`, `:67`, `:82` | Testa topo/fusão/fechamento e ausência de escrita. Migrar topo, mantendo a pergunta específica de interrupção. |
| `test_censo_antes_da_vaga.sh:36`, `:112`, `:125`, `:568`, `:694` | Testa pertencimento pela tabela, fechada fora, fonte ausente e ambiguidades; inclui mutante dependente dessa leitura. Fixtures, expectativas e mutante precisam migrar. |
| `test_handoff_sessao.sh:124`, `:130`, `:151` | Proíbe parser próprio do marcador/OBRAS e usa fixture de tabela. A guarda precisa alcançar a fonte nova; a fixture precisa fornecê-la. |
| `test_fabricante_le_a_ordem.sh:25`, `:34`, `:65` | Recalcula quem espera Ronald pela coluna e confronta o fabricante; continua leitor da fonte antiga se mantido. |
| `test_fabricante_backlog.sh:22`, `:35`, `:39` | Exige livres/expansão escalonada e reconhece ORDEM pelo texto. Depende da delimitação do fabricante. |
| `test_alarme_vigia_a_sessao.sh:62` | Exige textualmente que a fila venha de BACKLOG; essa guarda precisa acompanhar a troca. |
| `test_orcamento_contexto.sh:40`, `:165`, `:451` | Cria marcador e altera coluna para simular fila vazia. Migrar a representação do vazio. |
| `test_disjuntor.sh:59` | Cria marcador/tabela para hook e disjuntor. Precisa da fonte estruturada na fixture. |
| `test_hook_nao_para_por_processo.sh:36`, `:65` | Simula fila viva/vazia com marcador e fechado; preservar respostas com fixtures novas. |
| `test_hook_parei_so_lei.sh:36`, `:48`, `:68` | Simula lei, técnico, `!` e só registro pela tabela; migrar a fonte dessas condições. |
| `test_hook_teto_nao_conta_espera.sh:40`, `:69` | Fixture de marcador para testar teto/espera. Muda a fixture, não a pergunta do teto. |
| `test_hook_esperando_com_prova.sh:40`, `:65`, `:79` | Fixture para prova de espera e `siga`; precisa da nova fonte. |
| `test_hook_stop_saida_valida.sh:20`, `:45` | Fixture para schema e silêncio; precisa da nova fonte. |
| `test_relato_prova_no_stop.sh:16`, `:23` | Fixture para chegar à guarda de prova; precisa da nova fonte. |

Em `app/`, os outros leitores encontrados de BACKLOG perguntam **existência do item** e **padrão declarado da obra**, não ordem/andabilidade: `app/core/tests/test_marca_cron15.py:89` e `app/chamados/tests/e4b_c4_verificar.py:209`. A mesma distinção vale para `bin/tests/test_prompt_virou_item.sh:25`.

Há declarações documentais da autoridade antiga que precisam receber a emenda do instrumento: marcador manual em `app/docs/LEIS.md:140`, `app/docs/CORTES.json:736` e `app/docs/BACKLOG.md:1574`. O handoff salvo ainda publica `MANHA-0511`, enquanto o marcador atual publica `ORDEM-POS-E4A2`; não serve como prova da ordem atual. (`app/docs/HANDOFF-SESSAO.md:19`; `app/docs/BACKLOG.md:8`)

**2. Cobertura literal do aval**

| Exigência | Veredito sobre v2 |
|---|---|
| Fonte única estruturada em arquivo próprio | **Cobre a forma:** três seções no mesmo arquivo, com um escritor declarado. (`contrato_item3_v2.txt:2`) |
| `gerar_backlog` publica; hook lê | **Cobre a intenção.** “Renderiza no BACKLOG, sem escrever nele” precisa identificar que o arquivo intocável é o `.ids`; literalmente, o antecedente fica ambíguo. (`contrato_item3_v2.txt:2`) |
| Marcador e coluna deixam de ser fonte | **Não fecha a implementação:** nomeia duas funções, mas deixa sem especificação os demais caminhos do censo acima. (`contrato_item3_v2.txt:3`; `bin/hook_stop_fila1.py:225`, `:267`, `:344`, `:380`, `:1298`) |
| Universo = fila 1 não pousada | **Não resolve a semântica:** transforma todos os restantes de ORDEM em andáveis. O RED exige distinguir restante travado de restante aberto. (`contrato_item3_v2.txt:3`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:51`, `:73`) |
| Exclusões POS-20 e pousadas, pelo id | **Representa as seções**, mas falta a semente com identidade resolvida e a regra operacional de exclusão. A interdição de repetição entre seções cobre conflito, não resolve aliases. (`contrato_item3_v2.txt:2`, `:5`; `app/docs/BACKLOG.md:2031`, `:1965`) |
| RED ca320b69f permanece | **Diz que permanece, mas contradiz seu comportamento.** (`contrato_item3_v2.txt:6`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:59`, `:72`) |

O defeito central é concreto: **ORDEM com três ids, dois restantes andáveis e PAREI do em curso liberando** não preserva a regra guardada. Com outro andável, o RED exige **não devolver o turno**. Não é questão de confirmar uma hipótese do Code: essa cláusula do contrato está errada. (`contrato_item3_v2.txt:6`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:75`; `bin/hook_stop_fila1.py:1245`)

Além disso, uma lista apenas de ids não registra a diferença entre os dois estados que o RED exercita. O contrato precisa explicar essa diferença dentro da fonte estruturada, sem reintroduzir coluna, marcador ou adendo como juiz. A representação dessa condição está ausente do v2. (`contrato_item3_v2.txt:2`, `:3`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:51`, `:73`)

Também faltam: vazio válido; repetição **dentro** da mesma seção; gramática/seções inválidas; identidade canônica; e separação entre falha esperada da fonte e exceção que alcança a falha aberta. O v2 enumera ausência/ilegibilidade/repetição entre seções/id sem linha, mas não esses casos. (`contrato_item3_v2.txt:5`; `bin/disjuntor.py:1736`; `bin/hook_stop_fila1.py:1460`)

A promessa de handoff/disjuntor “não mudarem de resposta” sobra como garantia geral: a correção pretende justamente mudar pertencimento/contagem e, por consequência, devolução do turno e exigência de censo. (`contrato_item3_v2.txt:4`; `bin/disjuntor.py:1682`, `:1755`; `bin/hook_stop_fila1.py:1245`)

**Sim, sobrariam leitores de prosa** se a migração seguir apenas os nomes explícitos do §1: veto de PAREI, lista travada, alarme e fabricante; os recados também continuariam mandando o humano ao adendo. (`bin/hook_stop_fila1.py:1298`, `:380`, `:285`; `bin/alarme_sessao_ociosa.py:113`; `bin/fabricante_alvo.py:273`; `bin/handoff_sessao.sh:314`)

**3. Riscos do publicador e ordem de migração** [nao verificado]

- **Reintroduzir saída velha:** a preservação começa no marcador antigo. Acrescentar publicação nova sem retirar a antiga dessa janela pode duplicar marcador/lista; o hook atual usa a primeira ocorrência encontrada. (`bin/gerar_backlog.py:119`, `:127`; `bin/hook_stop_fila1.py:209`)
- **Perder adendos:** preservar apenas `OBRAS:INICIO` em diante descartaria a prosa entre o marcador e o início do bloco, atualmente incluída na janela. (`app/docs/BACKLOG.md:8`, `:712`; `bin/gerar_backlog.py:119`)
- **Tripwire inadequado:** remover o marcador pode parar a geração; conservar qualquer ocorrência textual satisfaz uma guarda que não verifica valor nem derivação. (`bin/gerar_backlog.py:131`)
- **Perder a cauda manual atual:** OBRAS termina em `:2038`, mas META-18 e outras linhas estão em `:2392` em diante. O gerador preserva apenas a janela terminada em OBRAS:FIM e recompõe o restante. (`app/docs/BACKLOG.md:2038`, `:2392`, `:2401`; `bin/gerar_backlog.py:100`, `:119`, `:138`)
- **Declarar falsa fila vazia:** ausência/erro da fonte não pode virar lista vazia nem escapar pelo tratamento genérico que libera o hook. (`contrato_item3_v2.txt:5`; `bin/hook_stop_fila1.py:1400`, `:1460`)
- **Fixture antiga impedir ou falsear a migração:** os selos de marcador/coluna precisam mudar junto com os leitores, preservando as propriedades do RED. (`bin/tests/test_hook_le_o_marcador.sh:56`; `bin/tests/test_hook_estado_e_a_coluna.sh:51`; `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:59`)

A sequência recomendada, sem dois juízes ativos, é:

1. **Corrigir o contrato antes do código:** resolver universo versus andabilidade, cláusula do PAREI, identidade dos ids e preservação da cauda. São incompatibilidades já demonstradas, não decisões para o código adivinhar. (`contrato_item3_v2.txt:3`, `:6`; `app/docs/BACKLOG.md:2038`, `:2401`)
2. **Preparar uma única alteração completa:** fonte semeada, leitor/validador compartilhado, todos os consumidores do censo, publicador, fixtures e emenda documental. Nenhum consumidor deve consultar a fonte antiga como fallback. (`aval_respostas_1211_1230.txt:4`; `contrato_item3_v2.txt:3`, `:7`) [nao verificado]
3. **No publicador, separar explicitamente preservação e geração:** conservar os trechos manuais necessários; substituir a publicação antiga; se houver marcador, gerar exatamente um a partir da fonte. Verificar igualdade da publicação e estabilidade de duas gerações, além da preservação manual. (`bin/gerar_backlog.py:119`, `:131`; `contrato_item3_v2.txt:6`)
4. **Ativar o conjunto no mesmo corte**, sem leitores antigos em execução atravessando a troca; confirmar o caminho efetivamente chamado pela configuração. Um commit único não demonstra sozinho qual cópia está em execução. (`contrato_item3_v2.txt:3`; `.claude/settings.json:9`, `:68`)
5. **Reverter o conjunto junto**, sem deixar seletor de compatibilidade marcador/arquivo. (`contrato_item3_v2.txt:7`)

Recontando **em memória**, pela lógica desta cópia: são **942** itens em `_itens_andaveis` e **941 além do em curso**. Portanto o “antes 942 / depois len(ORDEM)−1” do contrato compara total com restantes; precisa usar a mesma medida nos dois lados. O número pós-migração ainda é [nao verificado], pois depende da semente. (`bin/hook_stop_fila1.py:231`, `:270`; `app/docs/BACKLOG.md:8`, `:1758`; `contrato_item3_v2.txt:6`)

**4. Lista inicial e ambiguidades**

**Não há uma lista linear única que possa ser transcrita como semente sem resolver contradições.** O marcador aponta uma ordem que recebeu substituições posteriores, inclusive duas raias. (`app/docs/BACKLOG.md:8`, `:1758`, `:1759`, `:1880`, `:2401`)

Estas são as sequências literais relevantes, na ordem escrita em cada fonte:

| Fonte | Sequência nomeada |
|---|---|
| Adendo acima do marcador, histórico | O-OUTBOX → CONTRATO-20-E2-REBAIXA → funções de lastro/órfãs → disparar_perguntas_competencia → PENDENCIA-SEM-JUIZ-3/SILENCIO-VENCIDO-VOLTA → JUSTIFICATIVA-PAGA-O-PERIODO. Mistura ids e funções. (`app/docs/BACKLOG.md:7`) |
| **ORDEM-POS-E4A2**, apontada pelo marcador | BO-0611-05 → BO-DISPUTA-SEM-PERGUNTA → E4b → E4c → E4d → BUG-PAR-FROTA-1110 → DISPUTA-ZUMBI-1110 → O169/O168/LASTRO. (`app/docs/BACKLOG.md:8`, `:1758`) |
| **PLANO-JANELA-POS-RESET** | AUDITORIA-DE-TRAVAS primeiro; depois O169-FECHADA/O168/LASTRO → RAIZ F0/F1, F2=BO-DISPUTA, F3/F4 absorvendo ZUMBI/PAR → E4b/E4c/E4d → MAPA-DA-FILA-BO. Substitui a ordem anterior do E4b em diante. (`app/docs/BACKLOG.md:1759`) |
| **ORDEM-ATE-O-ZERO** | c21 BO → E3 inteira → E4b inteira → E4c → E4d → O169/O168/LASTRO; AUDITORIA permanece antes. (`app/docs/BACKLOG.md:1880`) |
| **META-18-DUAS-RAIAS** | Série E + RAIZ F1–F7 + O169/O168/LASTRO; muda posição da RAIZ/item 5 e inicia raia 2 com O169-FECHADA v5. Não fornece intercalação linear completa. (`app/docs/BACKLOG.md:2401`) |
| **RAIA-2-BO-VISIVEL** | Após O169 v5: BO-ROTULO-ENCERRADO → TRIAGEM-OFFLINE-SEM-CAMERA → LOTE-VALIDAR-REGRESSAO-0311. (`app/docs/BACKLOG.md:1903`) |
| Inserção posterior na raia 2 | APP-MENSAGENS-MSG-0 depois de TRIAGEM-OFFLINE e antes de O168/LASTRO; posição relativa ao LOTE não explicitada. (`app/docs/BACKLOG.md

[truncado em 20000 de 23679 caracteres]

## 12/11 13:30 · sessao 7bbc60e1c1e5 · - · - · origem=Ronald (tmux)

Censo de `bin/tests/` e `app/`, só leitura; não executei selos, pois eles criam fixtures (`bin/tests/test_hook_andavel_e_a_ordem_viva.sh:20`, `:42`).

**Ponto que invalida o v2 como está:** o RED mantido exige duas respostas: `ITEM-A` na ordem, mas esperando `!`, não impede liberar o PAREI do em curso; o mesmo `ITEM-A` aberto impede. O contrato “andáveis = IDs de ORDEM menos em curso” perde essa distinção se ORDEM significa o universo “fila 1 não pousada”. O selo mostra literalmente os dois estados em `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:51`, `:60`, `:66`, `:73`, `:75`, `:78`. Contar 2 restantes e simultaneamente exigir que o PAREI do em curso libere também contradiz o comportamento guardado por `bin/tests/test_hook_parei_com_andavel.sh:45`, `:55`. A especificação precisa resolver a distinção pertença/andabilidade sem voltar a usar prosa/coluna como fonte; como fazê-lo não está definido pelo anexo [nao verificado].

| Sítio | O que pergunta / efeito da migração |
|---|---|
| `bin/tests/test_hook_andavel_e_a_ordem_viva.sh:34`, `:51`, `:73` | Escreve marcador, ordem em prosa, estados, POS-20 e 30 ruídos; pergunta ao hook sobre topo/andáveis/PAREI. Precisa fixture estruturada preservando ambos os lados do RED. |
| `bin/tests/test_hook_parei_com_andavel.sh:25`, `:42`, `:46`, `:56` | Escreve marcador/tabela; usa coluna para alternar restante livre/`!` e exigir bloquear/liberar. Muda se o v2 só contar IDs. |
| `bin/tests/test_hook_parei_trava_o_item_em_curso.sh:20`, `:40`, `:53`, `:61` | Marcador O261, outro item; PAREI de outro item não libera; PAREI do topo libera quando outro está em `!`. Fixture/alteração de coluna precisam migrar. |
| `bin/tests/test_hook_parei_casa_id_inteiro.sh:21`, `:42`, `:57`, `:64` | Escreve marcador; classifica O1/O14 congeladas e O142 vivo para testar ID inteiro e PAREI. Fixture/fonte de congelamento precisam migrar. |
| `bin/tests/test_hook_le_o_marcador.sh:39`, `:56`, `:64`, `:74`, `:81`, `:88` | Exige autoridade do marcador e suas falhas; fila vazia é decidida pela tabela. Precisa virar autoridade/falhas do `.ids`, incluindo vazio explícito. |
| `bin/tests/test_hook_estado_e_a_coluna.sh:41`, `:51`, `:59`, `:80`, `:94` | Exige que estado/tachado definam topo parado; varre BACKLOG real com padrões do hook. A expectativa sobre ordem conflita com coluna deixar de ser fonte. |
| `bin/tests/test_hook_nao_cobra_congelado.sh:12`, `:25`, `:32`, `:111`, `:115`, `:135`, `:168` | Raspa `_NAO_ANDA` do código, valida coluna; pergunta ao hook pelo marcador real e sintético; exige falha sem marcador. Precisa rever tanto autoridade quanto fixtures. |
| `bin/tests/test_interrompidas_estado.sh:37`, `:44`, `:60`, `:67`, `:82` | Escreve marcador e estado, testa topo/fundida/fechada e ausência de escrita. Deve migrar topo para `.ids`, preservar a pergunta específica de interrupção. |
| `bin/tests/test_censo_antes_da_vaga.sh:36`, `:112`, `:125`, `:131`, `:568`, `:579`, `:694` | Escreve marcador e estados; exige pertencer à fila por tabela, exclusão fechada, “não sei” sem BACKLOG/bloco, ambiguidade de IDs; mutante depende da leitura de BACKLOG. Migração alcança fixtures, asserts e mutante. |
| `bin/tests/test_handoff_sessao.sh:124`, `:125`, `:130`, `:151` | Guarda exige importar hook e proíbe parser próprio OBRAS/marcador. A guarda permanece útil, mas precisa cobrir eventual parser próprio `.ids`; a fixture só tabela não fornece a nova fonte. |
| `bin/tests/test_fabricante_le_a_ordem.sh:25`, `:30`, `:34`, `:65`, `:70` | Lê estados reais para calcular quem espera Ronald e confronta fabricante; escreve casos espera/livre. Continua leitor de coluna se não migrar. |
| `bin/tests/test_fabricante_backlog.sh:22`, `:29`, `:35`, `:39` | Consome fabricante, exige itens livres e expansão `escalonar=True`, reconhece ordem pelo texto “ORDEM”. Deve rever se pertence ao escopo fila 1; essa delimitação não aparece no contrato [nao verificado]. |
| `bin/tests/test_alarme_vigia_a_sessao.sh:62` | Guarda textual exige que alarme use `docs/BACKLOG.md`; pode exigir autoridade antiga depois da migração. |
| `bin/tests/test_orcamento_contexto.sh:40`, `:165`, `:451` | Escreve marcador/tabela e muda coluna para simular fila vazia; precisa representar vazio no `.ids`. |
| `bin/tests/test_disjuntor.sh:59` | Escreve marcador/tabela ITEM-VIVO para exercitar hook/disjuntor; precisa `.ids` na fixture. |
| `bin/tests/test_hook_nao_para_por_processo.sh:36`, `:47`, `:65`, `:74` | Escreve marcador para fila viva e só fechado para vazia; bloqueio/liberação devem preservar resposta com nova fixture. |
| `bin/tests/test_hook_parei_so_lei.sh:36`, `:40`, `:48`, `:59`, `:68` | Escreve marcador; testa PAREI técnico/lei/`!` e só-registro por coluna. Precisa preservar perguntas com fonte estruturada. |
| `bin/tests/test_hook_teto_nao_conta_espera.sh:40`, `:52`, `:69` | Escreve marcador/tabela para exercitar teto com/sem espera. Fonte muda; resposta pretendida não. |
| `bin/tests/test_hook_esperando_com_prova.sh:40`, `:65`, `:67`, `:79` | Escreve marcador/tabela e exige liberação com prova ou `siga: ITEM-VIVO` sem prova. Fonte muda; resposta pretendida não. |
| `bin/tests/test_hook_stop_saida_valida.sh:20`, `:27`, `:45` | Escreve marcador/tabela; testa schema de bloqueio e silêncio com PAREI. Fonte muda; resposta pretendida não. |
| `bin/tests/test_relato_prova_no_stop.sh:16`, `:23`, `:28` | Escreve marcador/tabela para chegar ao guard de prova. Fonte muda; checagem de prova não deveria mudar. |

Outros leitores reais de BACKLOG encontrados em `app/` e `bin/tests/` não respondem à pergunta auditada: existência do item (`app/core/tests/test_marca_cron15.py:89`, `bin/tests/test_prompt_virou_item.sh:25`) e texto do padrão da obra (`app/chamados/tests/e4b_c4_verificar.py:209`). Não classificá-los como leitores da ORDEM VIVA.

Documentos que ainda declaram a autoridade antiga e precisam reconciliar no marco: `app/docs/LEIS.md:140` diz marcador escrito à mão; `app/docs/CORTES.json:736` idem; `app/docs/BACKLOG.md:707` chama marcador de primeiro aberto único; `app/docs/BACKLOG.md:1574` diz que medidor lê marcador e ele continua manual. `app/docs/HANDOFF-SESSAO.md:19` publica topo “MANHA-0511”, enquanto o marcador real está em “ORDEM-POS-E4A2” (`app/docs/BACKLOG.md:8`); é saída envelhecida, não prova da ordem atual.

## 12/11 13:30 · sessao a3d3702630bf · - · - · origem=Ronald (tmux)

Censo `bin/` e `.claude/`, exceto testes: [nao verificado]

| Sítio | Pergunta/ato atual | Efeito da fonte nova |
|---|---|---|
| `bin/gerar_backlog.py:110-127` | Lê e recoloca janela manual começando no marcador, incluindo toda prosa entre marcador e OBRAS. | Tem de separar saída derivada da janela preservada; senão recoloca marcador/lista velhos e pode duplicar saída. |
| `bin/gerar_backlog.py:129-140` | Tripwire exige que marcador e OBRAS presentes antes continuem no resultado; escreve BACKLOG por `.novo` + replace. | Se remover marcador, tripwire impede geração; se conservar como derivado, presença sozinha não verifica valor/fonte nova. |
| `bin/hook_stop_fila1.py:149-180` | `_item_fechado`: FECHADO/NO AR na coluna estado ou tachado na descrição; `_item_parado`: fechado ou regex de CONGELADA/FILA 2/espera/PAREI/portão. | Precisam deixar de julgar pertinência/andabilidade por estado; podem permanecer para perguntas documentais distintas. |
| `bin/hook_stop_fila1.py:192-228` | `_item_da_ordem_viva`: extrai marcador, exige linha correspondente em OBRAS, veta `_item_parado`. | Nova origem do topo; manter veto de coluna contradiz “coluna deixa de ser fonte”. |
| `bin/hook_stop_fila1.py:231-267` | `_itens_andaveis`: todos os ids não parados de OBRAS, ordem física; `_ha_item_aberto`: bool dessa lista. | Devem passar à fonte estruturada. Contrato só mudar `_andaveis_fora...` não basta: vazio continua julgado pela tabela. |
| `bin/hook_stop_fila1.py:270-287` | Conta andáveis além do topo; recado manda humano pegar próximo na “ORDEM VIVA (adendo do BACKLOG)”. | Lista deve vir da fonte única; recado ainda encaminharia leitura humana da prosa se não mudar. |
| `bin/hook_stop_fila1.py:290-322` | Diagnóstico de marcador quebrado; `_proximo_da_fila` delega topo. | Diagnóstico muda para fonte quebrada; topo pode conservar resposta se primeiro id for o mesmo. |
| `bin/hook_stop_fila1.py:323-346` | `_ids_que_nao_andam`: lista por estado de OBRAS. | Continua segundo juiz se não migrar; é usada para rejeitar PAREI em `bin/hook_stop_fila1.py:1298-1302`. |
| `bin/hook_stop_fila1.py:349-383` | `travas_da_fila`: varre OBRAS, exclui fechadas, mostra palavra de trava encontrada na coluna. | Se usada como “fila inteira travada”, continua juiz por prosa; contrato não a menciona. |
| `bin/hook_stop_fila1.py:1232-1245` | `_relato_parou`: PAREI do topo + nenhum outro andável. | Resposta muda quando tabela tem ruído fora da ORDEM. |
| `bin/hook_stop_fila1.py:1248-1313` | `_parei_do_em_curso`: PAREI `!`, elimina parados por coluna, compara id com topo. | Precisa retirar veto antigo por coluna e usar a mesma identidade estruturada. |
| `bin/hook_stop_fila1.py:1391-1403` | Main escolhe topo; sem topo consulta tabela para liberar “fila vazia” ou reclamar marcador. | Deve validar fonte antes de distinguir fila vazia/quebrada; manter tabela permite resposta divergente. |
| `bin/handoff_sessao.sh:54-78` | Lê BACKLOG, pergunta topo e próximo ao hook, publica ambos. | Herda fonte nova se API migrar; rótulo “MARCADOR” fica velho. |
| `bin/handoff_sessao.sh:120-123` | Pergunta “trava item?” e “devolve turno?” ao hook. | Herda mudanças do censo de andáveis/PAREI. |
| `bin/handoff_sessao.sh:163-174` | Para avais de hoje, procura linha BACKLOG e chama `_item_fechado` para dizer não cumprido. | Pergunta diferente de ordem, mas continua lendo estado/prosa de fechamento. Não pode servir de exclusão de pousadas da ORDEM. |
| `bin/handoff_sessao.sh:209-219` | Lista linhas abertas da raia20 usando `_item_fechado`. | Pergunta documental distinta; continua lendo coluna, portanto declarar exceção de escopo. |
| `bin/handoff_sessao.sh:290-299` | Sem `_proximo_da_fila`, publica `travas_da_fila` e afirma “nenhum item livre: turno se devolve”. | Perigoso manter: topo inválido não implica fila vazia nem inteira travada. |
| `bin/handoff_sessao.sh:309-316` | Publica próximo e diz “1o ABERTO do bloco OBRAS”; na ausência manda conferir ORDEM VIVA à mão. | Texto precisa migrar; preservá-lo manda humano ler a prosa velha. |
| `bin/disjuntor.py:1671-1682` | `_fila1_viva`: lê BACKLOG, exige marcador OBRAS, retorna `_itens_andaveis`. | Tem de ler universo estruturado; tabela aberta inteira deixa de exigir censo. Promessa contratual “não muda resposta” é falsa para itens abertos fora da ORDEM. |
| `bin/disjuntor.py:1720-1743` | Pertinência por token da célula e coincidência inteira nos dois sentidos, com ambiguidades. | Deve definir identidade do novo arquivo: ids exatos ou token qualificado? Contrato não resolve. |
| `bin/disjuntor.py:1746-1759` | Censo exigido só para obra pertencente à fila viva. | Herda troca do universo. |
| `bin/disjuntor.py:2355-2364` | CLI publica FORA ou aplica mesmo censo. | Herda troca; fora da lista nova muda resposta. |
| `bin/codex_pergunta.sh:228-234` | Invoca `disjuntor.py --censo` antes da vaga, usando raiz viva. | Leitor indireto da pertinência; não duplicar parser. |
| `bin/backlog_estado.py:56-68` | Antes/depois de escrever estado, consulta próximo e ids que não andam; imprime veredito alterado. | Deve deixar de sugerir que escrever coluna muda a ORDEM; hoje o código escreve e só informa a mudança, apesar da docstring prometer “PARA” em `bin/backlog_estado.py:8-10`. |
| `bin/interrompidas_estado.py:89-125` | Lê coluna INTERROMPIDA/FUNDIDA e topo importado; diz “voltou” quando id/fusão coincide com topo ou `_item_fechado`. | Topo migra indiretamente; fechamento documental permanece por coluna. Não usar este fechamento como exclusão estruturada. |
| `bin/gerar_estado.py:236-250` | Publica medição de interrompidas e topo importado. | Leitor indireto; texto “sem marcador” em `:243` precisa mudar. |
| `bin/alarme_sessao_ociosa.py:89-116` | **Leitor próprio esquecido:** conta itens em `## OBRAS`, excluindo ☑/FEITO/FECHAD da linha inteira. | Precisa migrar. Hoje decide fila vazia/disparo em `:40-45`, chamado por `:148-150`; mantendo-o, haverá dois juízes. |
| `bin/placar_code.sh:368-369` | Publica saída do alarme. | Leitor indireto; herda mudança. |
| `bin/fabricante_alvo.py:238-283` | **Leitor próprio esquecido:** lê linhas O/F, usa texto/estado (dinheiro, só registrar, Ronald/aval/corte/smoke/suspensão, livre, no ar/feito/fechado), escolhe itens da “ORDEM escalonada”. | Continua lendo prosa se não migrar; definição do universo do fabricante precisa ficar explícita. |
| `bin/fabricante_alvo.py:315-320` | Tenta itens livres, depois ORDEM escalonada. | Herda seletor anterior. |
| `bin/fabricante.sh:129` | Invoca seletor de alvo. | Leitor indireto. |
| `bin/gerar_backlog_topo.py:16-40` | Lista O-números “abertos”, decide FEITO pela descrição/onde e estado por frases; ordena numericamente. | É resumo distinto, não fila1/ORDEM; se permanecer, rotulá-lo e não consumi-lo como fonte de fila. |
| `bin/relato.sh:35`, `bin/relato.sh:115-121` | Gera resumo e publica BACKLOG inteiro. | Transporta saída derivada; não decide topo. Comentário em `bin/relato.sh:7-8` ainda afirma que ORDEM só se lê na tabela completa. |
| `.claude/settings.json:9` | Stop chama hook. | Herda fonte nova; configuração aponta caminho absoluto `/home/ronald/saas-hasner`, portanto execução dessa cópia em produção é [nao verificado]. |
| `.claude/settings.json:20`, `:29`, `:40`, `:53`, `:68` | PreCompact chama handoff; SessionStart lê handoff; PreToolUse chama disjuntor. | Leitores indiretos dos juízes acima. |

Contradições contratuais centrais:

- Mudar só duas funções deixa cinco caminhos ativos lendo tabela/estado: `_itens_andaveis`, `_ha_item_aberto`, `_ids_que_nao_andam`, `travas_da_fila` e veto `_item_parado` dentro do topo; referências em `bin/hook_stop_fila1.py:225`, `:250`, `:267`, `:344`, `:378-382`, `:1298-1302`, `:1400`.
- “Handoff e disjuntor mudam só fonte, não resposta” não pode ser literal: o bug consiste justamente em contar abertos fora da ORDEM, e disjuntor usa essa lista em `bin/disjuntor.py:1682`; PAREI depende dela em `bin/hook_stop_fila1.py:1245`.
- Não basta parar o parser de marcador: orientação ao humano ainda manda ler adendo/prosa em `bin/hook_stop_fila1.py:285-287` e `bin/handoff_sessao.sh:314-316`.
- Tripwire atual só verifica presença; necessidade de verificar igualdade da publicação derivada ao `.ids` e impedir prosa velha duplicada decorre de `bin/gerar_backlog.py:119-137`.

Não executei scripts auditados: possuem escritas explícitas (`bin/gerar_backlog.py:138-140`, `bin/backlog_estado.py:59`, `bin/hook_stop_fila1.py:51-54`, `bin/relato.sh:121-131`).

## 12/11 13:30 · sessao e4ace4e8c489 · - · - · origem=Ronald (tmux)

Não há lista linear única que eu possa transcrever como “fila 1 não pousada atual” sem resolver contradições e aliases por conta própria. O marcador aponta `ORDEM-POS-E4A2`; a própria cópia registra várias substituições posteriores dessa ordem. (`app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1758`, `app/docs/BACKLOG.md:1759`, `app/docs/BACKLOG.md:1880`, `app/docs/BACKLOG.md:2401`)

**Listas literais por camada, preservando a ordem escrita:**

| Fonte | Sequência literal |
|---|---|
| Topo, ainda anterior ao marcador | `O-OUTBOX` → `CONTRATO-20-E2-REBAIXA` → `fechar_cobranca_com_lastro` e `reconciliar_perguntas_orfas` → `disparar_perguntas_competencia` → `PENDENCIA-SEM-JUIZ-3` e `SILENCIO-VENCIDO-VOLTA` → `JUSTIFICATIVA-PAGA-O-PERIODO`; mistura ids e nomes de funções. (`app/docs/BACKLOG.md:7`) |
| Item apontado pelo marcador | `BO-0611-05` → `BO-DISPUTA-SEM-PERGUNTA` → E4b BACKFILL → E4c LEITORES → E4d CONTRACT → `BUG-PAR-FROTA-1110` → `DISPUTA-ZUMBI-1110` → O169/O168/LASTRO. (`app/docs/BACKLOG.md:1758`) |
| Substituição do E4b em diante, vigente a partir de 11/11 18h segundo o texto | Primeiro `AUDITORIA-DE-TRAVAS`; depois (a) `O169-FECHADA`, O168, LASTRO; (b) `RAIZ-DA-DISPUTA` F0/F1, F2=BO-DISPUTA, F3/F4 absorvem DISPUTA-ZUMBI e BUG-PAR; (c) E4b/E4c/E4d; (d) itens do `MAPA-DA-FILA-BO`. (`app/docs/BACKLOG.md:1759`) |
| Substituição do item (3) anterior | c21 BO → E3 inteira → E4b inteira → E4c → E4d → O169/O168/LASTRO; AUDITORIA-DE-TRAVAS permanece antes. (`app/docs/BACKLOG.md:1880`) |
| Emenda posterior | Série E + RAIZ F1–F7 + O169/O168/LASTRO; duas raias, com raia 2 iniciando em `O169-FECHADA` v5. Diz substituir posição da RAIZ e item 5, mas não dá nessa linha intercalação linear de todas as etapas. (`app/docs/BACKLOG.md:2401`) |
| Raia 2 subsequente | Depois do pouso O169 v5: `BO-ROTULO-ENCERRADO` → `TRIAGEM-OFFLINE-SEM-CAMERA` → `LOTE-VALIDAR-REGRESSAO-0311`. (`app/docs/BACKLOG.md:1903`) |
| Inserção na raia 2 | `APP-MENSAGENS-MSG-0` depois de TRIAGEM-OFFLINE e antes de O168/LASTRO; posição relativa ao LOTE-VALIDAR não explicitada. (`app/docs/BACKLOG.md:1913`) |
| Inserção na RAIZ | `RAIZ-DA-DISPUTA-F0-CASO-7279` à frente da F1 e de HJ-067/HJ-073. (`app/docs/BACKLOG.md:2406`) |
| Lista fechada do 20 | O168, LASTRO, E3-LEITORES, E4B, E4C, E4D; a linha diz “lista fechada”, não declara que esta enumeração é ordem de execução. O169 é expressamente “pousada”. (`app/docs/BACKLOG.md:2031`) |

**Exclusões/pousos com prova textual, sem inferir por nome ou prefixo:**

- `FIO-E0-V4-E4B`: a própria linha diz **POUSOU 12/11 08:46**, commit `087fe758a`; permanece no rol fechado do 20 sob alias E4B. Portanto não pode ser carregada como “não pousada” só porque aparece nesse rol. (`app/docs/BACKLOG.md:1891`, `app/docs/BACKLOG.md:2031`)
- `C20-O169`: **POUSADA 05/11**, FECHADA. O169 é dita pousada no rol do 20. (`app/docs/BACKLOG.md:1796`, `app/docs/BACKLOG.md:2031`)
- **Ambiguidade O169:** `O169-FECHADA` diz “já POUSADO 05/11 pela C20-O169”, mas separa delta do contador e ainda descreve B3/volta 5; não autoriza excluir automaticamente esse delta pelo pouso de `C20-O169`. (`app/docs/BACKLOG.md:1965`)
- `FIO-E0-V4-E4A-2`: NO AR em 10/11, merge `613a51498`. (`app/docs/BACKLOG.md:1890`)
- `FIO-E0-V4-E2-FUNCAO`: NO AR em 09/11, comboio com “E3”; isso não prova o pouso da E3-LEITORES posterior, cuja linha continua ABERTA/em obra. (`app/docs/BACKLOG.md:1869`, `app/docs/BACKLOG.md:1870`)
- `O-OUTBOX` e `CONTRATO-20-LASTRO-E-ORFAS`, ainda citados na lista do topo, estão respectivamente NO AR e FECHADA em suas linhas. (`app/docs/BACKLOG.md:733`, `app/docs/BACKLOG.md:735`)

**Ids explicitamente classificados POS-20 nas suas linhas:**

| id | Linha |
|---|---:|
| TRIAGEM-FILA-OPUS-V6 | `app/docs/BACKLOG.md:1812` |
| E4B-FILA-OPUS-C14 | `app/docs/BACKLOG.md:1893` |
| CORTES-JSON-DEFASADO | `app/docs/BACKLOG.md:2030` |
| CICLO-C-CEL-TEMPLATE | `app/docs/BACKLOG.md:2032` |
| FAIXA-NO-CADASTRO | `app/docs/BACKLOG.md:2033` |
| EFEITO-SEM-FALLBACK-CATALOGO | `app/docs/BACKLOG.md:2034` |
| LASTRO-FILA-PREVERIF-C11 | `app/docs/BACKLOG.md:2035` |
| BO-ROTULO-REABRIR-SO-NO-DETALHE | `app/docs/BACKLOG.md:2036` |
| PROPORCAO-FILA-OPUS-V11 | `app/docs/BACKLOG.md:2037` |
| AUDITORIA-DE-TRAVAS: MAPA-20-DERIVADO | `app/docs/BACKLOG.md:2399` |
| UI-PAINEL-FILA-OPUS-V5 | `app/docs/BACKLOG.md:2402` |
| BUG-CADASTRO-NAO-ALCANCA-O-FUTURO | `app/docs/BACKLOG.md:2403` |
| UI-ESCALA-REQUADRO | `app/docs/BACKLOG.md:2404` |
| UI-ESCALA-REQUADRO-ESPELHO | `app/docs/BACKLOG.md:2405` |

Não classificar `PROPORCAO-POR-RISCO` como POS-20 por substring: sua linha diz FECHADA NO AR e destina **achado novo** à AUDITORIA-DE-TRAVAS POS-20. Tampouco classificar `CONGELA-O-20` por sua frase de regra para **linha nova**. (`app/docs/BACKLOG.md:1917`, `app/docs/BACKLOG.md:2031`)

**Problemas concretos para semear o `.ids`:**

- E4c/E4d são etapas escritas dentro de `FIO-E0-V4-E4-ESCRITORES`; a linha não cria ids próprios para cada etapa. Exigir “id de ORDEM com linha OBRAS” impede usar literalmente `E4C`/`E4D` sem esclarecer a identidade. (`app/docs/BACKLOG.md:1882`, `app/docs/BACKLOG.md:2031`; ausência de linha própria: [nao verificado] como prova negativa completa)
- O168 tem linha `O168` e outra `C20-O168`; LASTRO tem `LASTRO-FOLGA-FORA-DO-CATALOGO` e `LASTRO-FAIXA-SEM-DEFAULT`, esta explicitamente “obra LASTRO”. A migração não pode escolher esses aliases silenciosamente. (`app/docs/BACKLOG.md:1398`, `app/docs/BACKLOG.md:2004`, `app/docs/BACKLOG.md:1810`, `app/docs/BACKLOG.md:1811`)
- `BUG-PAR-FROTA-1110` e `DISPUTA-ZUMBI-1110` são complementos absorvidos por outras obras/etapas; incluí-los como três obras autônomas pode duplicar o mesmo trabalho. (`app/docs/BACKLOG.md:1900`, `app/docs/BACKLOG.md:1908`, `app/docs/BACKLOG.md:1759`)
- A prosa histórica afirma simultaneamente “marcador autoridade”, “ordem é a das linhas” e “hook lê este bloco”; esses registros pertencem a momentos diferentes e não entregam uma regra atual única para ordenar a semente. (`app/docs/BACKLOG.md:689`, `app/docs/BACKLOG.md:699`, `app/docs/BACKLOG.md:722`)

## 12/11 13:26 · sessao 0fb37705e777 · - · - · origem=Ronald (tmux)

Sugestão de selo AST [nao verificado: implementação]:

- Propagar dependências por `Assign`, `AnnAssign`, `For` e comprehensions, separando escopos de funções.
- Marcar `%H:%M`, `[:5]` e `[-5:]` como identidade horária; reconhecer `orfas_grade` como ponte externa.
- Inspecionar `Compare`, elementos de `Set`/`SetComp`, chaves de `Dict`/`DictComp` e argumentos de `set(...)`.
- Exigir data no próprio operando horário. A chave externa de um dict não data seu set interno: esse é precisamente o problema de `relatorios/pdf_espelho.py:475`.
- Propagar o elemento da compreensão, sem confundi-lo com suas condições. A condição `b.orfa` identifica contexto sensível, mas não acrescenta data.
- Exibição isolada passa: listas de marcações e composição de texto não são comparações ou chaves (`relatorios/pdf_espelho.py:840`, `relatorios/pdf_espelho.py:844`, `relatorios/pdf_espelho.py:866`).

Os três mutantes devem restaurar, separadamente, a seleção por hora em `relatorios/pdf_espelho.py:479`, o conjunto/filtro sem data em `relatorios/pdf_espelho.py:675` e a supressão por hora em `relatorios/pdf_espelho.py:919`.

Cuidado com `.date()` de batidas: o selo anterior o rejeita explicitamente em `relatorios/tests/test_pdf_sem_regra_propria.py:158`. Formatação contendo data e hora evita essa nova ocorrência. Para S2, o produtor já fornece `%d/%m %H:%M`; inferência de ano seria informação adicional (`ponto/adaptador_motor.py:35`, `ponto/adaptador_motor.py:42`).

## 12/11 13:25 · sessao d01f0456916e · DECLARADO-X-GRAVADO · leitura · origem=Code

**O aval procede, com limite: a hora aplicável já é calculada, mas só sai embutida em texto. O destaque automático da divergência não cabe integralmente na fila “templates/static/testes”, mantendo Python intocado.** (`app/chamados/services/materializacao.py:431-465`).

**Contrato e chamadas**

- Retorna `None` se `tipo_resposta != 'hora'` ou resposta vazia após `strip`; nos demais retornos, `dict` com exatamente duas chaves, ambas `str`: `estado` e `detalhe`. Estados: `aplicada`, `recusada_paridade`, `invalida`, `ja_satisfeita`, `bloqueada_paridade`, `aplicavel`. Não entrega hora, data, tipo ou divergência como campos próprios. (`app/chamados/services/materializacao.py:438-465`).
- `aplicavel.detalhe` contém “validar vai registrar batida E/S HH:MM”. Leitura e escrita usam `_ts_tipo_do_plantio`; nele, a S139 pode substituir a resposta pelo marco do intervalo. (`app/chamados/services/materializacao.py:450`, `:463-465`, `:852`, `:409-416`).
- O consumidor encontrado anexa `pergunta.estado_aplicacao` no montador do fio; exceção também vira `None`. Esse dado chega ao fio completo, modal e re-render parcial. `meu_atendimento`, do colaborador, também utiliza o montador. (`app/chamados/services/fio.py:335-338`; `app/chamados/views.py:677-703`, `:889-931`, `:559-569`; `app/chamados/services/fio.py:446-465`).

| Tela / view | Hoje | Proposta e alcance |
|---|---|---|
| **Modal do fio** — `app/chamados/views.py:869`, `:927-931` | Mostra resposta; desenha apenas paridade/reuso, omitindo `aplicavel`. O alerta ainda promete gravar “este horário”. (`app/templates/chamados/partials/modal_fio.html:219-246`, `:258-263`). | Exibir literalmente `estado_aplicacao.detalhe` já cabe em template. Para “Resposta X → será gravado Y”, com destaque condicional, falta projeção estruturada do mesmo juiz; isso exigiria Python em `app/chamados/services/materializacao.py:431-465`, fora da fila UI. |
| **Fio completo** — `app/chamados/views.py:659`, `:677-703` | Recebe o estado, mas o template não oferece cartão de validação de pergunta: apresenta thread e resposta livre/status. (`app/templates/chamados/fio_colaborador.html:234-275`). | Não contar como validação visível já existente. Acrescentar validação/prévia aqui seria ampliar a tela; o contexto já contém as perguntas preparadas. (`app/chamados/services/fio.py:253-258`, `:319-338`). |
| **Central** — `app/chamados/views.py:726-743` | Lista detalhada valida inline, com confirmação genérica; o endpoint percorre perguntas respondidas. Não há estado de aplicação na linha. O resumo padrão abre o fio. (`app/templates/chamados/partials/_lista_chamados.html:82`, `:143-167`; `app/chamados/views_cobrar.py:145-169`; `app/chamados/services/painel.py:385-405`; `app/templates/chamados/partials/_lista_resumo.html:53-60`). | Prévia por **cada pergunta**, antes de validar. Exige alimentar contexto em `app/chamados/services/painel.py` ou uma prévia em `app/chamados/views_cobrar.py`: ambos fora de templates/static. Alternativa só de navegação: encaminhar ao modal com a prévia. |
| **Detalhe local / cartão HTMX** — `app/chamados/views.py:549-551`, `:398-405` | Exibe resposta, evidência, previsto/alerta e botão Validar; promete gravar “este horário”, sem estado de aplicação. (`app/templates/chamados/detalhe_local.html:310`; `app/templates/chamados/partials/_pergunta_card.html:29-54`, `:68-97`). | Alimentar estado no GET **e** no fragmento após ação; desenhar a mesma projeção do modal e corrigir a promessa. Exige `app/chamados/services/acoes_chamado.py:334-419` e `app/chamados/views.py:398-405`, fora de templates/static. |
| **Lote / drawer** — `app/chamados/views_cobrar.py:353-386` | Tabela mostra Resposta / Previsto / Dif.; o produtor fornece classificação, sem estado de aplicação. (`app/templates/chamados/partials/_lote_preview.html:23-26`; `app/chamados/services/validacao.py:921-924`). | Coluna “Será gravado”, estado e destaque por pergunta. Exige alimentação em `app/chamados/views_cobrar.py:361-368`; alterar o produtor em `services/validacao.py` também tocaria serviço de núcleo. “Previsto” atual não deve ser reaproveitado como resultado do plantio. |
| **Lote / copiloto** — `app/chamados/views_cobrar.py:275-302` | Até 200 referências, mas apenas 10 exemplos; JS mostra resposta/previsto e valida todas as referências. (`app/chamados/views_cobrar.py:270-293`; `app/templates/core/_copiloto.html:236-250`). | Encaminhar à prévia completa ou fornecer projeção de todas as referências. Exige Python em `views_cobrar.py`; alterar `_copiloto.html` alcança include da base. (`app/templates/base.html:432`). |

Há ainda “Aplicar hora e validar” para declaração textual no modal; essa porta também chega a `validar_pergunta` e precisa da mesma honestidade na promessa de gravação. (`app/templates/chamados/partials/modal_fio.html:130-143`; `app/chamados/services/declaracao_texto.py:696`).

**Riscos da proposta**

- **Previsão parcial:** o estado exige batida aprovada para anunciar reuso; a escrita reusa qualquer existente. A escrita também tem bloqueio por data divergente que não aparece nesse estado. Portanto `aplicavel` não garante o desfecho final. (`app/chamados/services/materializacao.py:453-465`, `:865-877`, `:1197-1204`).
- **N+1/cache:** o fio consulta estado por pergunta; ele busca grade, batida existente e vizinhas. `estado_aplicacao_pergunta` não recebe `carga`; `CargaDaVida` não elimina automaticamente esse custo. Proposta: alimentar o mesmo juiz por lote, sem cache global entre requests. (`app/chamados/services/fio.py:319-338`; `app/chamados/services/materializacao.py:283-285`, `:431-459`, `:95-104`; `app/chamados/services/carga_pergunta_viva.py:152-158`; `app/chamados/juizes.py:879-889`).
- **Verificação de tela:** cobrir hora igual/divergente, reuso, bloqueio, `None`, re-render e múltiplas perguntas; bases/includes/JS exigem smoke nas duas cascas ou prova pelo grafo, conforme a regra local. (`CLAUDE.md:202-203`; `app/chamados/services/materializacao.py:438-465`).
- **Produção além do anexo, custos medidos nesta cópia e resultado de smoke/testes:** [nao verificado]. [nao verificado]

## 12/11 13:25 · sessao ec41dabd9e8a · - · - · origem=Ronald (tmux)

Censo A conferido, sem escrita.

| Sítio | Entrada → saída | Caso protegido/teste | Lei/corte | [nao verificado]
|---|---|---|---|
| `_ts_tipo_do_plantio`: hora S139 | `resposta_colab` → hora de `_marco_faltante` para **todo** motivo `intervalo_saida`/`intervalo_volta` quando há marco; o corpo não testa `intervalo_livre`, “fixo” ou distância. `app/chamados/services/materializacao.py:409-419` | Volta informada `02:00`, marco `03:00`: exige `E@03:00`, impede `E@02:00`. `app/chamados/tests/test_s139.py:14-18`; `app/chamados/tests/test_s139.py:40-56` | **SEM LEI específica localizada** nos três documentos pesquisados [nao verificado]. L-032 trata intervalo no envelope, não autoriza substituir declaração; L-099 exige coerência com cadastro. `app/docs/LEIS.md:80`; `app/docs/LEIS.md:122` |
| `_tipo_pelo_marco` | Tipo inicialmente pelo motivo (`E` para três motivos, `S` nos demais) → tipo do marco cobrado por motivo; fallback ATA, marco mais próximo em até 240min. Havendo divergência, prevalece snapshot cobrado. `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:246-300`; `app/chamados/services/materializacao.py:390-407` | `orfao_14h` + resposta `19:01` gerava `S`, apesar de marco de entrada; teste exige `E`; sentido inverso também testado. `app/chamados/tests/test_hx_tipo_pelo_marco.py:86-119` | Corte Ronald 31/08 declarado no código e teste; documento nos três índices: **SEM LEI específica localizada** [nao verificado]. `app/chamados/services/materializacao.py:247-262`; `app/chamados/tests/test_hx_tipo_pelo_marco.py:16-26` |
| Tipo sem marco | Ausência de marco identificável → mantém tipo pelo motivo e grava `pendencia_admin=tipo_nao_conferido`, junto de `recusa_motivo`. `app/chamados/services/materializacao.py:392-407`; `app/chamados/services/materializacao.py:851-857` | Sem régua ou marco distante: ainda planta pelo motivo, com pendência explícita. `app/chamados/tests/test_hx_tipo_pelo_marco.py:121-142` | Corte caminho C 31/08 declarado no corpo; documento nos três índices: **SEM LEI específica localizada** [nao verificado]. `app/chamados/services/materializacao.py:393-405` |
| Data do plantio + overnight | `data_evento`, fallback contexto do motivo, + hora fonte → instante local; pode mudar de D para D+1 quando cabe no envelope da cadeia de marcos do vínculo do dia, ±90min; sem marcos fica em D. `app/chamados/services/materializacao.py:345-350`; `app/chamados/services/materializacao.py:418-426`; `app/chamados/juizes.py:296-331` | Turno partido S12 fica D; intervalo noturno volta00 vai D+1; sem marcos entrada não recua D−1. `app/chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:59-91` | L-085 sustenta **dia do turno = início**; envelope ±90 e derivação específica têm cortes 15/09 documentados em código/teste, sem corte específico encontrado nos três documentos [nao verificado]. `app/docs/LEIS.md:117`; `app/chamados/juizes.py:297-309` |
| Reuso S137 | Procura o primeiro timestamp do mesmo colaborador/tipo dentro de ±20min do instante derivado; preserva timestamp/tipo da batida encontrada e escreve `pergunta_origem`. `app/chamados/juizes.py:235-247`; `app/chamados/services/materializacao.py:865-876`; `app/chamados/services/materializacao.py:952-956` | Evitar S falsa em D06:46 quando a saída real do turno já existe em D+1 06:46; teste exige contagem inalterada. `app/chamados/tests/test_materializar_overnight.py:1-4`; `app/chamados/tests/test_materializar_overnight.py:48-71` | **SEM LEI específica localizada** para ±20min [nao verificado]; propósito declarado no próprio juiz. `app/chamados/juizes.py:236-238` |
| Deduplicação final da porta | Mesmo tipo vivo até ±60s → retorna existente; mesma chave `(colaborador,timestamp)` → retorna existente inclusive outro tipo/retratada. Não altera seus valores. `app/ponto/registro_batida.py:106-122` | Reenvio da mesma saída já reclassificada como entrada causava constraint/500; caso registrado no comentário e teste. `app/ponto/registro_batida.py:116-122`; `app/ponto/tests/test_bater_mesma_chave_idempotente.py:1-6` | Corte 14/09 declarado no corpo; **SEM LEI específica localizada** nos três documentos [nao verificado]. `app/ponto/registro_batida.py:116-119` |
| Ramo `data_hora` | Aceita data + HH:MM[:SS], mas só utiliza HH e MM; segundos são descartados; sem componente de hora usa `00:00`; tipo sempre `E`. `app/chamados/services/materializacao.py:1001-1014` | Teste fundador da truncagem/default: [nao verificado]. O teste encontrado protege colisão da âncora E com S real próxima. `app/chamados/tests/test_chokepoint_ancora.py:8-11`; `app/chamados/tests/test_chokepoint_ancora.py:24-44` | **SEM LEI específica localizada** para truncar segundos ou completar `00:00` [nao verificado]. |

O teste O306 **mantém S139**: `13:05` vira `13:00` quando o marco pertence à própria pergunta; impede usar marco de motivo vizinho. `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:136-153`

Os sítios efetivos de criação a partir de `PerguntaDisputa` são os dois ramos do materializador, que chamam `registrar_batida`; o timestamp/tipo recebido é gravado pela porta sem outra transformação no create. `app/chamados/services/materializacao.py:933-940`; `app/chamados/services/materializacao.py:1013-1025`; `app/ponto/registro_batida.py:152-159`

Validação individual, lote de perguntas, regularização de disputa e reparo do passivo convergem para essa unidade, sem segundo derivador nesses caminhos. `app/chamados/services/acoes_disputa.py:13-22`; `app/chamados/services/validacao.py:461-469`; `app/chamados/services/validacao.py:735-746`; `app/chamados/services/acoes_disputa.py:118-139`; `app/chamados/services/passivo_validado.py:514-516`

`RegularizacaoExterna` é outra entrada: itens lançados pela supervisão e confirmados pelo colaborador, sem leitura de `PerguntaDisputa.resposta_colab`; para item `intervalo=True`, ordena as duas horas e atribui S à menor/E à maior. Este é sítio de alteração do **tipo associado aos campos**, que merece constar no limite do censo, mas não pertence ao universo das perguntas. `app/chamados/services/regularizacao_ext.py:1-19`; `app/chamados/services/regularizacao_ext.py:52-55`; `app/chamados/services/regularizacao_ext.py:168-179`; teste `app/chamados/tests/test_reg_intervalo.py:11-14`; `app/chamados/tests/test_reg_intervalo.py:28-45`

A correção externa escreve diretamente `alvo.tipo` e `alvo.timestamp`, a partir de `item.hora_saida` prioritária, senão `hora_entrada`, e `item.data`; não executa ajuste overnight nesse ramo. `app/chamados/services/regularizacao_ext.py:143-156`

Para TRIAGEM, existe regra real: `_carimbada_como_miolo` reconhece `is_intervalo`, `_intra_dur` ou `_intra_ata`; as duas últimas são marcas em memória do pareamento. `intervalos_do_turno` lê pares marcados e aplica piso de 15min. `app/ponto/turnos.py:932-940`; `app/ponto/turnos.py:502-546`

`Batida.is_intervalo` é campo persistido explícito de saída/volta do intervalo; `Batida.origem` é o campo do canal de origem, default `app`. `app/ponto/models.py:26`; `app/ponto/models.py:45`

## 12/11 13:25 · sessao 4a40a12f21d6 · - · - · origem=Ronald (tmux)

Contratos encontrados para D, sem execução em Django ou produção. [nao verificado]

- Universo: `respondida_em__isnull=False`, motivos pedidos e `tipo_resposta='hora'`. **Não usar `p.estado == RESPONDIDA`**, porque a propriedade devolve `VALIDADA`/`RESOLVIDA` quando há `validada_em`; isso excluiria os plantios do universo. (`app/chamados/models.py:2443-2450`; `app/chamados/models.py:3034-3049`)
- Relação real: `Batida.pergunta_origem`, reverso `PerguntaDisputa.batidas_plantadas`; `PerguntaDisputa.batida_origem` identifica outro tipo de episódio e não a batida produzida pela resposta. (`app/ponto/models.py:40-44`; `app/chamados/models.py:2437-2442`)
- Origem: campo `Batida.origem`; aparelho é `'app'`; plantio hora atual passa literal `'disputa_s84_retro'`. (`app/ponto/models.py:13-20`; `app/ponto/models.py:45`; `app/chamados/services/materializacao.py:933-939`)
- **Origem disputa + relação não prova que aquela pergunta criou a batida**: S137 procura/reusa qualquer existente antes da criação, inclusive de disputa; a ligação depois é sobrescrita para a pergunta atual. Não há campo persistido “criada nesta pergunta” ou “reuso”. Logo, classificação histórica exata nova/reuso exige deixar ambiguidades em `OUTRO`, ou declarar explicitamente que “nova” é o critério operacional por origem, com sua limitação. (`app/chamados/services/materializacao.py:863-877`; `app/chamados/services/materializacao.py:952-956`)
- Há segundo dedup na porta: mesmo tipo dentro de 60 segundos e mesma chave exata podem devolver existente, sem informar um booleano `created`. (`app/ponto/registro_batida.py:94-122`)
- A trilha `materializar_batida` roda tanto para nova quanto reuso; o JSON grava o `ts` derivado, não necessariamente `nova_batida.timestamp`, e fixa origem disputa mesmo quando reusou app. **Não usar esse JSON como fato da batida persistida nem indicador de criação.** (`app/chamados/services/materializacao.py:947-978`)
- `Batida.criado_em` existe e serve como instante de criação da nova batida; `timestamp` é instante marcado. (`app/ponto/models.py:23`; `app/ponto/models.py:79`)

**Desfechos e precedência**

- Recusa paridade terminal: `p.via_resolucao == PerguntaDisputa.VIA_PARIDADE_RECUSADA`; a própria leitura `estado_aplicacao_pergunta` usa isso. (`app/chamados/models.py:2475`; `app/chamados/services/materializacao.py:440-447`; `app/chamados/services/materializacao.py:930`)
- Existe recusa paridade sem essa via: quando marco está apagado, chama `_carimbar_recusa`, grava `recusa_motivo` e fica sem materializar. O catálogo real pode classificá-la com `traduzir_recusa(p.recusa_motivo)[0] == 'PARIDADE'`. (`app/chamados/services/materializacao.py:906-922`; `app/chamados/services/materializacao.py:1089-1109`; `app/chamados/catalogo/recusa.py:26`; `app/chamados/catalogo/recusa.py:77`)
- `pendencia_admin` não significa ausência de plantio: pode ser gravado por tipo não conferido e a execução continuar criando batida. Portanto, nova/reuso devem preceder essa categoria no censo exclusivo, e sobreposição merece contador separado. (`app/chamados/services/materializacao.py:390-405`; `app/chamados/services/materializacao.py:851-859`; `app/chamados/services/materializacao.py:933-939`)
- `_carimbar_recusa` normalmente grava só `recusa_motivo`, sem `pendencia_admin`; apenas código `'COLISAO'` carimba a via `fato_ocupado`. (`app/chamados/services/materializacao.py:1094-1109`)
- Leitor existente dos rótulos de administração: `explicar_pergunta`: `aguarda_validacao` quando estado RESPONDIDA, depois `pendencia_admin`, depois juiz factual; falha de plantio validada vira `validada_sem_gravar`. Pode usá-lo como definição explícita de pendente administrativo, lembrando que sua precedência não é a precedência solicitada pelo censo exclusivo. (`app/chamados/services/explicador.py:68-104`)
- `nao_validada`: campo `validada_em is None`; outras vias/carimbos sem batida e sem pendência devem ficar `OUTRO`, sem adivinhar. (`app/chamados/models.py:2450`; `app/chamados/models.py:2469-2543`)

**Data e DNA histórico**

- Data da pergunta: `p.data_evento`; fallback real é `date.fromisoformat((p.disputa.contexto or {})[p.motivo]['data'])`. (`app/chamados/services/materializacao.py:345-353`)
- A data não é a data civil da resposta: `_ajustar_saida_overnight` trata `data_evento` como **dia do turno**, lê cadeia prevista, tenta dia do turno e seguinte dentro do envelope de 90 minutos; fora dele permanece no dia do turno. (`app/chamados/juizes.py:296-331`)
- Encadeamento real: `marcos_turno` lê `ec.marcos_do_dia`, sequência hi→hii→hfi→hf, avançando um dia quando próximo marco `<=` anterior. (`app/escala/servico_jornada.py:69-94`)
- Para datar resposta sem passar por S139, pode montar `ts` da resposta em `data_evento`, chamar somente `_ajustar_saida_overnight` alimentado com `carga.vinculo(...)` e `carga.celulas` contendo cópia da célula com o DNA histórico; assim chama o juiz real com alimentação histórica. A função recebe essa alimentação sem precisar do resto de `CargaDaVida`. (`app/chamados/juizes.py:316-321`; `app/escala/servico_jornada.py:75-77`; `app/escala/models.py:1117-1132`)
- Campos exatos: `CelulaDia.regenerada_em`, `.regeneracoes`, `.dna_anterior`, `.dna`; DNA contém `vinculo_id`, `tipo_escala_id`, `marcos` e `lampadas`. (`app/escala/models.py:1692-1729`; `app/ponto/management/commands/gerar_celulas.py:95-104`)
- Seleção exigida pelo usuário é DNA anterior se `regenerada_em > b.criado_em`, DNA atual caso contrário. Mas **dna_anterior só guarda imediatamente anterior**: com várias regenerações, não garante versão vigente no plantio; imprimir contador de ambiguidade e não chamar essa versão histórica comprovada. (`app/escala/models.py:1714-1729`; `app/ponto/portas/celula.py:271-277`)
- `gerada_em` é `auto_now_add`, não acompanha regeneração; `regenerada_em` é última regeneração, não edição humana. (`app/escala/models.py:1702-1721`)
- Não cair no template atual quando DNA histórico/marcos estiver ausente: `ec.marcos_do_dia` tem esse fallback; ausência deve sair como não verificada na sonda histórica. (`app/escala/models.py:1098-1103`; `app/escala/models.py:1124-1133`)
- Marco oposto: saída→`hfi`, volta→`hii`, conforme pares e tipos da célula. (`app/escala/models.py:1652-1656`)

**Vínculo e TRIAGEM**

- `escala_vigente(colaborador, dia)` é carregador da autoridade pura `escala.alimentacao.vinculo_do_dia`, não outra regra de escolha. Célula nomeia geradora; vigência continua sendo conferida. (`app/escala/servico_jornada.py:15-66`; `app/escala/alimentacao.py:269-286`)
- Há regra existente para batida de intervalo: `_carimbada_como_miolo(b)` considera `is_intervalo`, `_intra_dur` e `_intra_ata`. **Não precisa dizer que inexiste e usar primeira/última.** (`app/ponto/turnos.py:932-940`)
- As marcas em memória vêm de `turnos_do_colab(colab,d,d)` e seu pareador; itere `t.batidas`, filtre `_carimbada_como_miolo`, `origem == 'app'`, e use dono de `t.data_turno` via `escala_vigente` para confirmar mesmo vínculo. A função já exclui retratadas e considera virada do dia. (`app/ponto/turnos.py:1850-1866`; `app/ponto/turnos.py:1915-1925`; `app/ponto/turnos.py:1935-1939`; `app/ponto/turnos.py:1960`; `app/ponto/turnos.py:1992-1995`)
- `intervalos_do_turno` é outra pergunta: retorna pares com piso de 15 minutos, enquanto `_carimbada_como_miolo` responde geometria. Para comparar proximidade a batidas de intervalo, a segunda é mais direta. (`app/ponto/turnos.py:502-546`; `app/ponto/turnos.py:932-940`)

**Carga semanal**

- Se contar perguntas validadas no estado atual, use `p.validada_em` e semana ISO local desde `2026-09-21`, explicitando uma validação atual por pergunta. (`app/chamados/models.py:2450`)
- Se “validações” significar todos os atos históricos, existe `DecisaoPergunta` com `ato='validacao'`, `ato_em`, `pergunta`; a pergunta guarda projeção, e linha é o fato. Misturar os dois universos muda a unidade. (`app/chamados/models.py:3840-3856`; `app/chamados/models.py:3858`; `app/chamados/models.py:3872-3876`)
- Top escala precisa dizer se é pk de **vínculo** (`dna['vinculo_id']`/EscalaColaborador.pk) ou **modelo** (`dna['tipo_escala_id']`/TipoEscala.pk); os dois existem no mesmo DNA. (`app/ponto/management/commands/gerar_celulas.py:97-98`)

## 12/11 13:25 · sessao f80726ddfd1e · - · - · origem=Ronald (tmux)

B — telas/rotas de validação:

| Superfície | View/rota e template | O que mostra | Fonte |
|---|---|---|---|
| Modal do fio | `app/chamados/urls.py:39`; `app/chamados/views.py:869`, `:927`, `:951-954`; `app/templates/chamados/partials/modal_fio.html:219`, `:258-284` | Resposta bruta e, no alerta, resposta versus previsto; não desenha o detalhe `aplicavel` que contém a hora futura. O detalhe exibido é só `bloqueada_paridade` ou `ja_satisfeita`. | `estado_aplicacao_pergunta` é calculado em `app/chamados/services/fio.py:335-338`; filtrado no template em `app/templates/chamados/partials/modal_fio.html:230-246`; hora futura existe em `app/chamados/services/materializacao.py:463-465`. |
| Detalhe local/cartão individual | `app/chamados/urls.py:14`; `app/chamados/views.py:483-501`, `:549-551`; `app/templates/chamados/detalhe_local.html:307-310`; `app/templates/chamados/partials/_pergunta_card.html:31`, `:43-54`, `:67-97` | `resposta_display`, resposta versus previsto no alerta, evidência física; não exibe uma previsão derivada do plantio. | Usa `leitura_da_resposta` → `classificar_resposta_validacao`/`frase_da_resposta`, em `app/chamados/templatetags/chamados_tags.py:144-152`; montagem da evidência em `app/chamados/services/acoes_chamado.py:361-386`. O cartão não referencia `estado_aplicacao`; seus campos de exibição/validação estão em `app/templates/chamados/partials/_pergunta_card.html:29-98`. |
| Central, botão inline ✓ | `app/chamados/urls.py:33`; `app/chamados/views_cobrar.py:145-169`, `:201-214`; `app/templates/chamados/partials/_lista_chamados.html:79-82` | O botão/linha não apresenta hora respondida nem hora futura antes de validar. | Seleciona perguntas por `perguntas_vivas_de`, chama `acao_validar_pergunta`; não consulta `estado_aplicacao_pergunta`. `app/chamados/views_cobrar.py:168-169`, `:201-208`. |
| Central, drawer “Validar em lote” | `app/templates/chamados/painel_gestao.html:14`; `app/chamados/urls.py:36-37`; `app/chamados/views_cobrar.py:353-368`, `:372-386`; `app/templates/chamados/partials/_lote_preview.html:21-26` | Resposta, “Previsto” e diferença; não a hora derivada que será gravada. | `fila_validar_classificada`/`classe_do_lote`; `esperada` vem de `delta_da_resposta`, enquanto `_slot_esperado` só serve à busca de conflito. `app/chamados/services/validacao.py:864-877`. |
| Copiloto, proposta/validação de lote | `app/chamados/urls.py:34`, `:38`; `app/chamados/views_cobrar.py:232-261`, `:275-302`; `app/templates/core/_copiloto.html:236-250` | Amostra de resposta e “previsto”; não previsão do plantio. | `fila_validar_classificada`, campos `resposta_colab`, `detalhe.esperada`, `delta_min`. `app/chamados/views_cobrar.py:287-293`. |
| Modal, “Aplicar HH:MM e validar” de declaração em texto | `app/templates/chamados/partials/modal_fio.html:126-143`; `app/chamados/services/declaracao_texto.py:641-648`, `:687-696` | Hora extraída, tipo, dia e marco previsto; não deriva a hora efetiva pelo materializador. | Proposta de `propostas_do_chamado`; grava resposta pela porta canônica e chama `validar_pergunta`. `app/chamados/services/declaracao_texto.py:649-652`, `:691-696`; propostas preparadas em `app/chamados/services/fio.py:291-299`. |

A rota do fio completo aceita POST pelo mesmo `processar_post_fio`, incluindo `validar_pergunta`; entretanto seu HTML apresenta descrição, followups e caixa de resposta, sem os cartões/botões de validação presentes no modal. A ausência foi conferida por busca no arquivo inteiro; os blocos renderizados são `app/templates/chamados/fio_colaborador.html:229-260`; delegação da rota em `app/chamados/views.py:667-677`; validação comum em `app/chamados/services/fio.py:585-609`.

“Regularizar disputa” também aparece no detalhe/modal, depois do conjunto de respostas, como operação de fechamento/materialização, sem uma nova previsão individual por pergunta: `app/templates/chamados/detalhe_local.html:314-326`; `app/templates/chamados/partials/modal_fio.html:353-359`; entradas em `app/chamados/views.py:467-475` e `app/chamados/services/fio.py:646-656`.

C — trilha:

- `_log_auditoria_materializacao` envia ação, modelo `Batida`, PK, descrição, JSON `depois` e ator a `core.observ.evento`; seu contrato é best effort. `app/chamados/services/materializacao.py:731-737`.
- Na resposta `hora`, o JSON contém `batida_pk`, `colaborador_id`, `timestamp`, `tipo`, `origem`, `disputa_pk`, `chamado_pai_pk`, `chamado_destino_pk`, `chamados_relacionados_ids`, `pergunta_pk`, `resposta_colab`. A descrição ainda contém nome do colaborador, timestamp e referências de disputa/chamados. `app/chamados/services/materializacao.py:957-978`.
- Na âncora `data_hora`, contém `batida_pk`, `colaborador_id`, `timestamp`, `tipo`, `origem`, `disputa_pk`, `chamado_destino_pk`, `pergunta_pk`, `resposta_colab`; descrição contém nome, timestamp e referências. `app/chamados/services/materializacao.py:1045-1063`.
- Esses payloads não registram código da regra aplicada, motivo da substituição nem marco usado; registram resposta e timestamp derivado. `app/chamados/services/materializacao.py:966-978`, `:1053-1063`.
- Cuidado: o log `materializar_batida` também roda no reuso, declara “criada”, fixa `origem='disputa_s84_retro'` e usa `timestamp=ts` derivado; não garante que esses sejam origem/timestamp da Batida reusada. O reuso não altera timestamp: apenas chega à anotação de `pergunta_origem`. `app/chamados/services/materializacao.py:865-877`, `:952-978`.
- `anotar` grava os campos pedidos e trilha antes/depois automática; no vínculo da pergunta são o antigo/novo `pergunta_origem`, ação `vincular_pergunta_origem`, descrição com PKs e ator. Não recebe regra/marco nesse chamado. É idempotente, sem nova linha se o campo já coincide. `app/ponto/registro_batida.py:235-263`; `app/chamados/services/materializacao.py:952-956`.
- O registro final inclui `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `ip` e eventual `ator` do processo. `app/core/observ.py:118-123`, `:133-137`; adaptação de `anotar` em `app/ponto/registro_batida.py:225-232` e `app/core/auditoria.py:17-24`.

## 12/11 13:24 · sessao 6a8cc614e150 · - · - · origem=Ronald (tmux)

Achados em leitura:

- **Aval confirmado com limite:** `aplicavel` já traz “validar vai registrar batida E/S HH:MM” em `app/chamados/services/materializacao.py:463-465`. O horário vem de `_ts_tipo_do_plantio` (`:450`), também usado pela escrita (`:852`); S139 substitui resposta pelo marco de intervalo (`:409-416`).
- **Contrato real:** `dict[str,str]` com exatamente `estado` e `detalhe`, ou `None` para tipo diferente de `hora` ou resposta vazia (`materializacao.py:438-465`). Estados: `recusada_paridade`, `aplicada`, `invalida`, `ja_satisfeita`, `bloqueada_paridade`, `aplicavel` (`:444-465`). Não fornece `hora`, `timestamp`, `data`, `tipo` ou `divergiu` estruturados (`:431-465`). [nao verificado]
- **Consequência da fila 2:** exibir `detalhe` de `aplicavel` cabe em template; destacar diferença sem interpretar texto/recalcular exige projeção Python adicional, portanto fora de templates/static, em `app/chamados/services/materializacao.py:431` e alimentação de contextos. O atual contrato não permite comparação segura da hora (`:463-465`).
- **Quem chama e entrega:** único consumidor de produção encontrado é `montar_chamados_fio`, anexando `p.estado_aplicacao` (`app/chamados/services/fio.py:335-338`). Fio chama e entrega `chamados` (`app/chamados/views.py:677,697-703`); modal chama e entrega (`:889,927-931`); re-render parcial também (`app/chamados/services/fio.py:437-446,461-465`). `meu_atendimento` também usa o montador, embora seja tela do colaborador (`views.py:559-569`).
- **Modal omite apenas o aplicável:** os dois ramos mostram `bloqueada_paridade` e `ja_satisfeita`, sem `aplicavel` (`app/templates/chamados/partials/modal_fio.html:230-246`).

Riscos comprováveis na cópia:

- **N+1:** montador chama estado por pergunta (`fio.py:319-338`); estado consulta batida existente e vizinhas (`materializacao.py:453-459`), vizinhas fazem até duas queries (`:95-104`), tipo consulta grade (`:283-285`), overnight consulta vínculo (`app/chamados/juizes.py:317-321`). `select_related` de perguntas não elimina essas consultas (`fio.py:225-232`).
- **CargaDaVida não alimenta esse estado:** função não aceita `carga` e chama auxiliares avulsos (`materializacao.py:431,450-459`). CargaDaVida pertence à consulta do C6 (`carga_pergunta_viva.py:69-83`), memoiza grade por colaborador/dia (`:152-158`) e tem batida na janela alimentada (`:188-190`); seu uso atual é `pergunta_viva(...carga=...)` (`tarefa_do_dia.py:488-491`). Não é cache pronto para colar no estado.
- **Cache:** memo C6 vive na instância/request; com carga não lê nem altera memo avulso (`juizes.py:872-889`). Evitar cache global/entre requests (`:879-880`).
- **Promessa ainda parcial:** leitura só chama “já satisfeita” para batida aprovada (`materializacao.py:453-454`), escrita reusa qualquer existente (`:865-877`); gate `DATA_EM_DISPUTA` existe na escrita (`:1197-1204`) e não no estado (`:431-465`). Portanto estado aplicável não garante desfecho final.
- **Smoke:** regra explícita para bases/includes/JS e clique nas duas cascas (`CLAUDE.md:202-203`); piso existente explica árvores separadas (`app/core/tests/test_smoke_chromium.py:18-21`). Selo pode provar exclusão de uma casca pelo grafo de includes (`CLAUDE.md:203`).
- **S139 protege caso inverso:** teste documenta volta 03:00 digitada 02:00, slot oposto (`app/chamados/tests/test_s139.py:14-18`) e verifica plantio 03:00 (`:49-56`). Não executei testes, pois produzem arquivos/banco e a autorização é só leitura.

_4270 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
