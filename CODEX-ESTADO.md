# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 18:15** · publicado em **01/11/2026 18:19** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 18:15 · sessao 3fb825a7930a · O261-E-F2 · verificador · origem=Code

Análise estática; resultados de execução e medições mencionadas nos logs: **[nao verificado]**. Não executei a suíte: o quarto teste cria e escreve arquivo temporário (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:259`, `:261`, `:262`).

Referências abreviadas, todas relativas à cópia:

- **T** = `app/chamados/tests/test_o261e_f2_ramo_e_i20.py`
- **E** = `app/chamados/services/disputa_emissao.py`
- **M** = `app/chamados/models.py`
- **J** = `app/chamados/juizes.py`
- **V** = `app/chamados/services/vigia_fio.py`
- **A** = `app/chamados/management/commands/auditar_invariantes_chamados.py`
- **P** = `app/ponto/portas/celula.py`

1. **Os seis procedentes foram tratados no código; a confirmação por execução permanece [nao verificado].**

   | Achado da triagem | Avaliação do delta |
   |---|---|
   | **1 — causa versus detalhe** | Resolvido: compara conjuntamente rota, causa e detalhe com `['sem_porta', 'e', CAUSA_DO_RAMO]`; a produção escreve exatamente nessa disposição (`T:183`; `V:589`). |
   | **2 — retorno não prova o ramo** | Resolvido: `_fechar` deixa de verificar o retorno; a prova passa pelo estado persistido da pergunta e pelo vigia (`T:159`, `T:175`, `T:176`, `T:193`). |
   | **3 — MORDE abortava antes do vigia** | Resolvido: os helpers são separados; o MORDE verifica explicitamente a causa `c` e depois exige que o selo do vigia rejeite esse resultado (`T:171`, `T:179`, `T:212`, `T:214`). |
   | **4 — `ok` inválido e célula sobrescrita depois** | Resolvido: usa `concorde` pela porta, cuja validação recusa vocabulário inválido; depois do fecho apenas relê e compara a célula, sem restaurá-la (`T:151`, `T:234`, `T:159`, `T:160`, `T:161`; `P:659`). |
   | **5 — falta de controles negativos do I20** | Resolvido no desenho: cria texto com instante e texto vazio sem instante; ambos coexistem com o caso positivo, cujo total esperado continua exatamente um (`T:226`, `T:230`, `T:244`, `T:220`). |
   | **6 — PK por substring** | Resolvido: interpreta a amostra com `literal_eval` e exige igualdade com `[p.pk]` (`T:79`, `T:220`). |

2. **Descartado 7: sustenta-se para estes cenários; “mutante equivalente” globalmente não se sustenta.** A guarda consulta a pergunta **antes** do carimbo; a porta aplica carimbo e eventualmente `respondida_em` antes de consultar novamente o C6. É o mesmo juiz com entradas diferentes (`E:1626`; `M:2555`, `M:2558`, `M:2565`).

   Há um contraexemplo estático: pergunta sem resposta, motivo `saida_sem_entrada`, célula `furo` por outro marco, mas E/hi acesa. Sem carimbo, o veredito acusador a mantém viva; com carimbo, a lâmpada própria a mata antes dessa decisão. Retirar somente a guarda permite então gravar o carimbo (`J:845`, `J:893`, `J:1077`, `J:1079`, `J:1083`, `J:1084`; `M:2576`). Execução desse exemplo e ocorrência em produção: **[nao verificado]**.

   **Descartado 8: sustenta-se como entrada adversarial do detector.** O teste declara e injeta o meio-estado; o escritor normal atribui texto e instante e salva ambos juntos; o I20 consulta precisamente texto preenchido com instante nulo (`T:147`, `T:149`; `E:1247`, `E:1248`, `E:1266`; `A:1102`, `A:1103`). Isso prova detecção/preservação da entrada injetada, sem provar sua origem em produção; “B=0 na frota” permanece **[nao verificado]**. [nao verificado]

3. **Origem das escritas e leituras.**

   Nos quatro cenários, o próprio teste escreve `resposta_colab='07:55'`; os instantes nulos e a via vazia vêm da criação com os defaults do modelo. O veredito e as lâmpadas também são entradas escolhidas pelo teste, gravadas pela porta real (`T:145`, `T:149`, `T:151`; `M:2212`, `M:2299`, `M:2756`; `P:768`, `P:770`, `P:771`, `P:775`). [nao verificado]

   A produção escreve carimbo/via em `_escrever_carimbo`, acrescenta `respondida_em` quando aplicável e só persiste após aceitação pelo C6. Na recusa, restaura a instância em memória (`M:2529`, `M:2531`, `M:2558`, `M:2569`, `M:2576`, `M:2582`, `M:2583`).

   | Teste | Campos da pergunta efetivamente verificados | Célula, censo e I20 | [nao verificado]
   |---|---|---|
   | **Ramo normal** | Relê o banco: `materializada_em=None`, via vazia e pergunta viva. Não compara diretamente texto nem `respondida_em` (`T:174`, `T:175`, `T:176`, `T:177`). | Confere preservação da célula e linhas/contadores do censo real; não roda I20 (`T:191`, `T:193`, `T:161`, `T:183`, `T:185`). |
   | **MORDE do ramo** | A produção sob mock escreve o carimbo; o teste relê e exige carimbo existente. O helper rejeita já na primeira asserção: sua comparação de via não é alcançada nessa chamada (`T:201`, `T:205`, `T:206`, `T:208`, `T:175`). | Confere célula preservada, linha real com causa `c` e rejeição pelo selo do vigia; não roda I20 (`T:202`, `T:161`, `T:209`, `T:212`, `T:214`). | [nao verificado]
   | **Caso 2 normal** | Relê e compara texto, `respondida_em` e `materializada_em`; não compara via (`T:237`, `T:239`). | Confere célula, ausência do dia no censo e contadores; o auditor real produz I20, exigido como `VIOL,1,[pk]` (`T:236`, `T:161`, `T:242`, `T:243`, `T:245`, `T:220`). |
   | **MORDE do I20** | Relê a pergunta, mas não compara diretamente aqueles quatro campos (`T:253`, `T:254`). | Confere célula; não chama o censo. O teste escreve a cópia alterada do auditor; essa cópia produz `OK,0,[]`, e o auditor original deve produzir `VIOL,1,[pk]` (`T:252`, `T:161`, `T:258`, `T:262`, `T:266`, `T:268`, `T:272`). | [nao verificado]

   As **linhas do censo** são construídas pela produção e apenas filtradas/lidas pelo teste. A **linha I20** vem da consulta, contagem/amostra e formatação do auditor, capturada em `StringIO` (`V:589`, `V:654`; `T:166`, `T:167`; `A:1102`, `A:564`, `A:565`, `A:1835`, `A:1837`; `T:86`, `T:97`). [nao verificado]

   Portanto, há valores escritos pelo teste e depois comparados — sobretudo resposta, veredito e lâmpadas — como **preservação de premissas**. Eles não demonstram que o sistema criou essas premissas; o código agora explicita essa distinção e não reescreve a célula depois do fecho (`T:147`, `T:156`, `T:159`, `T:161`, `T:238`, `T:239`). [nao verificado]

4. **Sim, existem outros sobreviventes por inspeção estática; sobrevivência medida: [nao verificado].**

   - **Eliminar `d['alcance_' + causa] += 1` em `V:651`.** Universo, total de fio morto e linhas permanecem iguais; são os resultados comparados pelos testes. `alcance_c`/`alcance_e` ficariam errados sem uma asserção correspondente (`V:553`, `V:650`, `V:654`; `T:183`, `T:185`, `T:212`, `T:243`). [nao verificado]
   - **Eliminar somente `_desfazer_em_memoria(antes)` em `M:2570`.** A porta continua retornando `False` antes do `save`, e o fecho continua sem persistir aquela instância. Os testes relêem o banco, deixando invisível o objeto interno sujo em memória; isso viola a restauração prometida pela porta (`M:2571`, `M:2576`, `M:2579`; `E:1638`, `E:1639`; `T:174`, `T:205`, `T:237`, `T:253`). [nao verificado]

5. **Não identifiquei relógio solto, vazamento do mock ou dependência explícita de ordem no desenho destes quatro testes.** A classe congela o tempo, o censo recebe `HOJE` explicitamente e o controle usa instante literal; os `now()` desses caminhos de produção ficam sob o congelamento declarado (`T:100`, `T:166`, `T:226`; `E:1623`; `M:2550`; `P:772`). [nao verificado]

   O mock termina antes das testemunhas, e sua restauração é verificada por identidade. Cada método cria seu cenário, com dados comuns declarados em `setUpTestData`, numa classe `TestCase` (`T:201`, `T:204`, `T:101`, `T:103`, `T:189`, `T:200`, `T:234`, `T:251`). Execução em ordens diferentes e confirmação prática do isolamento: **[nao verificado]**.

## 01/11 18:14 · sessao 43184c58c13e · - · - · origem=Ronald (tmux)

Mutantes sobreviventes por inspeção estática; execução **[não verificado]**:

1. **Excluir o rollback da recusa** em `app/chamados/models.py:2570`. A porta continua retornando `False` antes do `save` (`models.py:2571`, `models.py:2576`), e o fecho continua imediatamente (`app/chamados/services/disputa_emissao.py:1638`). O objeto interno fica alterado em memória, mas os testes observam instâncias relidas do banco (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:174`, `:205`, `:237`, `:253`). Viola o contrato de restituição integral da instância (`app/chamados/models.py:2579`).

2. **Excluir a atualização do contador por causa** em `app/chamados/services/vigia_fio.py:651`. Preserva universo (`:553`), total de fio morto (`:650`) e linhas (`:654`); são precisamente os resultados assertados (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:181`, `:185`, `:212`, `:242`, `:243`). Os testes não assertam `alcance_c`/`alcance_e`, embora a produção declare que as causas devem somar o total (`app/chamados/services/vigia_fio.py:499`). Excluir a atualização de `alcance_e_detalhe` também passa por essa lacuna (`:652`).

O descarte do mutante da linha 1626 exige uma ressalva: **“mesmo juiz” não demonstra equivalência universal**, pois a porta consulta o C6 após mudar o estado (`app/chamados/models.py:2555`, `:2558`, `:2565`). Contraexemplo estático: pergunta sem resposta com motivo `saida_sem_entrada`, célula `furo` por outro marco, mas E/hi acesa (`app/chamados/juizes.py:844`). Sem carimbo, o veredito acusador a mantém viva (`:1083`); com carimbo, a lâmpada própria a mata antes dessa decisão (`:1077`). Retirar a guarda permitiria gravar (`app/chamados/models.py:2576`). Os fixtures atuais usam `batida_ausente`, sem esse marco próprio (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:145`; mapa em `app/chamados/juizes.py:844`). Ocorrência desse contraexemplo em produção: **[não verificado]**.

## 01/11 18:14 · sessao 2bb9051113f1 · - · - · origem=Ronald (tmux)

Os dois descartes têm alcances diferentes:

- **#7: sustentado para o cenário dos quatro testes, mas “equivalente” globalmente é forte demais.** A guarda lê a pergunta antes do carimbo (`app/chamados/services/disputa_emissao.py:1626`); a porta escreve carimbo e eventualmente resposta em memória antes de consultar novamente o juiz (`app/chamados/models.py:2555`, `app/chamados/models.py:2558`, `app/chamados/models.py:2565`). Portanto, mesmo juiz não significa mesma entrada.

  Contraexemplo estático: pergunta `saida_sem_entrada`, sem resposta, célula `furo`, E acesa e outro marco apagado. Sem carimbo, C6 chega ao veredito acusador e retorna viva (`app/chamados/juizes.py:1081`). Com carimbo, C6 verifica antes a lâmpada do marco e retorna morta (`app/chamados/juizes.py:1077`); o mapeamento desse motivo para E está em `app/chamados/juizes.py:845`, e a consulta da lâmpada em `app/chamados/juizes.py:863`. Retirar só a guarda permite gravar nesse caso (`app/chamados/models.py:2576`). Contraexemplo inferido do código; execução [não verificado].

- **#8: sustentado como entrada adversarial.** O teste declara e injeta texto sem `respondida_em` (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:147`). O escritor real de resposta atribui ambos e salva juntos (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1266`). O I20 consulta justamente texto preenchido com instante nulo (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101`). Isso verifica detecção/preservação da entrada injetada, sem comprovar sua origem em produção; B=0/frota/sonda [não verificado].

Quanto a tempo e isolamento: classe congelada em `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:100`; vigia recebe `HOJE` explicitamente na linha 166; controle usa instante literal na linha 226. O mock está limitado ao contexto das linhas 201–202, com identidade restaurada assertada na linha 204. As quatro rotinas criam cenário próprio nas linhas 189, 200, 234 e 251; a classe usa `TestCase` na linha 101. Ausência de dependência de ordem em execução [não verificado].

Não executar o conjunto sob esta autorização: o MORDE do I20 cria e escreve arquivo temporário (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:259`, `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:262`).

## 01/11 17:56 · sessao 90d1cb24554b · E4-CALENDARIO-L1 · leitura · origem=Code

1) **O problema de desenho, em seis linhas.**

- “Status do dia” reúne natureza do dia, andamento do turno, julgamento e disponibilidade de dados no mesmo vocabulário. (`app/ponto/status_dia.py:28`)
- Há duas regras: o calendário deriva `status`; o D4 combina relógio e coluna persistida. Compartilhar o vocabulário não unifica essas regras. (`app/colaboradores/services/calendario.py:801`; `app/ponto/status_dia.py:196`)
- A própria palavra `falta` tem contratos diferentes: o calendário pode produzi-la por ausência de batida; L-039 exige falta decidida. (`app/colaboradores/services/calendario.py:133`; `app/docs/LEIS.md:87`)
- A célula é referência viva em qualquer competência, mas o lote de status congela justamente as entregues. (`app/docs/ADR/0003-celula-tres-naturezas.md:25`; `app/ponto/management/commands/lavrar_status_competencia.py:40`)
- “Lavrado” identifica a origem da resposta, sem certificar sua atualidade: o D4 lê o status sem conferir a impressão dos insumos. (`app/ponto/status_dia.py:203`)
- Igualdade entre dois status não certifica frase, ação disponível ou cobrança do marco; esses campos têm outros predicados. (`app/colaboradores/services/calendario.py:886`; `app/colaboradores/services/calendario.py:189`; `app/chamados/juizes.py:1073`)

2) **Quais colunas não encontram explicação suficiente na regra escrita?**

Os quantitativos abaixo são os anexos fornecidos, sem reprodução independente [nao verificado]. A comparação mede impacto; nenhum dos caminhos vira gabarito pelo número de coincidências, conforme L-110. (`app/docs/LEIS.md:136`)

| Coluna ou classe da saída | O que é explicável; o que permanece sem explicação |
|---|---|
| `tempo` | Passado não implica encerrado: é necessário ultrapassar o fim previsto e não haver turno vivo. Portanto `tempo` sozinho não explica nem condena `aberto`, `previsto` ou `falta`. (`app/ponto/turnos.py:2113`) O instante e o motivo individual de encerramento não aparecem nos anexos [nao verificado]. |
| `tela_hoje=falta` × `d4=irregular/ok` | A diferença entre receitas é explícita: a tela admite falta sem decisão; a lavra trata `nunca_bateu` como irregular e admite isento sem batida como `ok`. (`app/colaboradores/services/calendario.py:133`; `app/ponto/status_dia.py:55`; `app/ponto/status_dia.py:108`) **A acepção automática de falta não se sustenta em L-039.** (`app/docs/LEIS.md:87`) Qual dessas condições ocorreu em cada amostra [nao verificado]. |
| `folga/ausencia` × `previsto`, hoje ou futuro | O D4 consulta o relógio antes da coluna; a tela pode devolver ausência ou feriado antes do ramo temporal. Isso explica mecanicamente a diferença, sem provar erro individual. (`app/ponto/status_dia.py:197`; `app/colaboradores/services/calendario.py:109`) O ADR D4 manda devolver `aberto/previsto` antes do encerramento. (`app/docs/ADR/0002-celula-e-o-dia.md:21`) |
| Demais diferenças com `fonte=lavrado` | Não há no anexo veredito/via, ata, cobertura parcial, isenção ou impressão por dia; escolher o resultado devido para cada par fica [nao verificado]. Esses insumos mudam a resposta da lavra. (`app/ponto/status_dia.py:96`; `app/ponto/portas/celula.py:694`) |
| `nulo`, `completaveis`, `fila_cartorio`, `entregue` | O segundo anexo relata **35.432 completáveis, fila zero; 29.950 em entregues** [nao verificado]. Nulo completável é distinto de carimbo antigo no código. (`app/ponto/services/cartorio.py:758`) O bloqueio das entregues tem respaldo no ADR 0002, mas contraria a natureza viva declarada no ADR 0003: **há conflito escrito, não uma explicação normativa única.** (`app/docs/ADR/0002-celula-e-o-dia.md:68`; `app/docs/ADR/0003-celula-tres-naturezas.md:25`) |
| `sem_dados/sem_celula` | D4 retorna isso antes de consultar natureza ou relógio. (`app/ponto/status_dia.py:191`) Dos 1.230 dias do segundo anexo, 120 estão classificados como vínculo existente e 1.110 como sem vínculo [nao verificado]. Os primeiros são candidatos à quebra de cobertura; os demais não são defeito demonstrado apenas pela situação atual da pessoa. A obrigação escrita é por **dia de vínculo vigente**. (`app/ponto/portas/celula.py:561`) |
| `rotulo_perdido={'outro':…,'em_curso':…}` | A tela tem frases específicas para andamento e retenção; a lavra também guarda frase própria do isento. (`app/colaboradores/services/calendario.py:886`; `app/ponto/status_dia.py:48`) O significado operacional de `outro` e quais frases ele agrega ficam [nao verificado]. |
| `resolver_dia={'nasce':27,'some':250}` | O predicado depende de status, trabalho, data e último recurso; este último também depende da pergunta viva respondida. (`app/colaboradores/services/calendario.py:189`; `app/colaboradores/services/calendario.py:571`; `app/colaboradores/services/calendario.py:901`) A transformação usada pela sonda e a legitimidade de cada nascimento/desaparecimento ficam [nao verificado]. |
| `iguais`, `diferentes`, `ERROS 0`, `SONDA_OK`, `janela_x_dia_dia_a_dia {}` | Os anexos relatam resultados de comparação e execução [nao verificado]. Eles não demonstram conformidade com a regra nem cobertura adversarial; igualdade com o código anterior é impacto, não certificação. (`app/docs/LEIS.md:136`) |

3) **Que perguntas faltaram, e quem escreve/lê os campos?**

A afirmação histórica de que “ninguém perguntou” seria [nao verificado]. O confronto exige estas perguntas:

- **Estamos comparando a mesma pergunta?** Natureza vencedora, cobrança do dia encerrado, turno vivo e pendência de um marco têm contratos distintos. (`app/ponto/precedencia.py:46`; `app/ponto/status_dia.py:158`; `app/ponto/turnos.py:2105`; `app/chamados/juizes.py:1073`)
- **Qual é o universo obrigatório?** Pessoa atualmente viva, vínculo cobrindo aquela data e célula existente não são equivalentes; o gerador consulta vínculos ativos, enquanto a escolha diária também admite histórico encerrado. (`app/ponto/portas/celula.py:568`; `app/escala/alimentacao.py:177`)
- **Conhecer um fato humano exige célula?** A precedência pode reconhecer ausência/feriado sem célula, enquanto D4 encerra a leitura em `sem_celula`. (`app/ponto/precedencia.py:375`; `app/ponto/status_dia.py:192`)
- **“Lavrado” significa atual, completo ou apenas existente?** D4 não confere impressão; D5 considera já lavrado qualquer status não nulo, sem examinar o rótulo. (`app/ponto/status_dia.py:203`; `app/ponto/services/cartorio.py:758`)
- **Todo insumo capaz de mudar a resposta invalida o julgamento?** O ADR reconhece isenção fora da impressão; a função de impressão não recebe esse campo. (`app/docs/ADR/0002-celula-e-o-dia.md:59`; `app/ponto/services/cartorio.py:356`)
- **O passado consultado é referência atual ou fotografia entregue?** O ADR 0003 reserva a fotografia ao gravado e declara a célula viva. (`app/docs/ADR/0003-celula-tres-naturezas.md:25`)
- **A comparação conserva frase, palavra, ação e contagem?** O calendário publica esses componentes separadamente; seu resumo não possui parcelas para `sem_certidao` e `sem_dados`, embora ambos pertençam ao vocabulário. (`app/colaboradores/services/calendario.py:883`; `app/colaboradores/services/calendario.py:935`; `app/ponto/status_dia.py:42`)

Esta é a cadeia de escritores e leitores constatados; completude de um censo global além dos arquivos abertos [nao verificado].

| Campos envolvidos | Quem escreve ou deriva | Quem lê |
|---|---|---|
| `trabalha`, `dna`, `escala_geradora` | Geração, regeneração e reversão, declaradas no inventário do ADR; criação concreta pela porta. (`app/docs/ADR/0003-celula-tres-naturezas.md:41`; `app/ponto/portas/celula.py:593`) | Precedência, vínculo diário e encerramento. (`app/ponto/precedencia.py:349`; `app/escala/alimentacao.py:237`; `app/ponto/turnos.py:2164`) |
| Vigência `data_inicio/data_fim/ativa` | Fechamento por `fechar_vigencia`; demais escritores de início não recenseados aqui [nao verificado]. (`app/colaboradores/services/vinculo.py:561`) | Cobertura diária, geração e seleção do vínculo. (`app/escala/alimentacao.py:182`; `app/ponto/portas/celula.py:588`; `app/escala/alimentacao.py:250`) |
| Batidas: `timestamp`, `tipo`, origem/retratação | Registro por `registrar_batida`; operações de alteração/retratação no mesmo módulo. (`app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:235`; `app/ponto/registro_batida.py:267`) | Calendário pelos turnos; impressão dos insumos. (`app/colaboradores/services/calendario.py:301`; `app/ponto/services/cartorio.py:366`) |
| Ausência, estado, efeito e cobertura | Criação pelo serviço de ausência; decisão do dia pode criar ausência aprovada. (`app/ponto/services/ausencia.py:130`; `app/ponto/services/veredito_celula.py:315`) | Calendário consulta cobertura para cobrança e folha separadamente; palavra lê cobertura/decisão. (`app/colaboradores/services/calendario.py:582`; `app/colaboradores/services/calendario.py:626`; `app/ponto/services/dia_decidido.py:363`) |
| `FolgaDia` e origem de calendário/manual | `gerar_foto_folgas` e `lancar_folga`. (`app/escala/folgas.py:150`; `app/escala/folgas.py:228`) | Precedência seleciona a folga válida para o vínculo. (`app/ponto/precedencia.py:358`) |
| `ata`, `veredito`, `veredito_via`, `protestos`, `veredito_em` | `lavrar_veredito`; preserva a decisão administrativa durante lavra do cartório. (`app/ponto/portas/celula.py:709`; `app/ponto/portas/celula.py:737`) | Calendário, regra do status e C6. (`app/colaboradores/services/calendario.py:510`; `app/ponto/status_dia.py:101`; `app/chamados/juizes.py:1081`) |
| `impressao`, `julgada_em`, `ausencia`, `tem_entrada/tem_saida`, `insumos_em` | `carimbar`, com fontes/naturezas inventariadas. (`app/ponto/portas/celula.py:787`; `app/docs/ADR/0003-celula-tres-naturezas.md:49`; `app/docs/ADR/0003-celula-tres-naturezas.md:58`) | D5 recebe a distinção entre fatos atuais e fila antiga. (`app/ponto/services/cartorio.py:752`) |
| `status_dia`, `status_rotulo` persistidos | `_aplicar_status`, dentro de `lavrar_veredito`, pela regra da lavra ou decisão administrativa. (`app/ponto/portas/celula.py:694`; `app/ponto/portas/celula.py:705`) | D4 lê o par; D5 lê a nulidade do status. (`app/ponto/status_dia.py:203`; `app/ponto/services/cartorio.py:758`) |
| `status`, `status_rotulo`, `resumo` da tela | `_classificar` e montagem do calendário. (`app/colaboradores/services/calendario.py:801`; `app/colaboradores/services/calendario.py:886`; `app/colaboradores/services/calendario.py:935`) | Grade e rodapé; portas curtas de status e contagem. (`app/templates/colaboradores/partials/_calendario_grade.html:69`; `app/templates/colaboradores/partials/_calendario_grade.html:142`; `app/colaboradores/services/calendario.py:204`; `app/colaboradores/services/calendario.py:216`) |
| Encerramento; retorno D4 `status/rotulo/fonte` | Encerramento derivado por `dias_encerrados`; resposta derivada pelo D4, sem persistir o relógio. (`app/ponto/turnos.py:2125`; `app/ponto/status_dia.py:196`) | Calendário lê motivos; D4 lê célula e motivos. (`app/colaboradores/services/calendario.py:699`; `app/ponto/status_dia.py:191`) |
| Pergunta: resposta, validação, materialização e via | Resposta em `disputa_emissao`; validação em `validacao`; materializador e portas do modelo também carimbam estados. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/validacao.py:127`; `app/chamados/services/materializacao.py:509`; `app/chamados/models.py:2485`) | C6 lê esses estados, veredito, fato do marco e pendências; há classificação residual pela jornada. (`app/chamados/juizes.py:1032`; `app/chamados/juizes.py:1094`; `app/chamados/juizes.py:1099`) |
| `pode_ultimo_recurso`, `resolver_dia_oferecido` | Calendário combina competência, pergunta viva e predicado da ação. (`app/colaboradores/services/calendario.py:901`; `app/colaboradores/services/calendario.py:932`) | Template condiciona o controle pela permissão do usuário. (`app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| `DiaPago`/rubricas e palavra visual | Dinheiro por `dia_pago.lavrar`, chamado pelo fechamento; palavra por `dia_decidido.do_dia`. (`app/ponto/services/dia_pago.py:135`; `app/ponto/services/fechamento.py:1168`; `app/ponto/services/dia_decidido.py:399`) | Calendário lê dinheiro e publica palavra/cor separadamente do status. (`app/colaboradores/services/calendario.py:370`; `app/colaboradores/services/calendario.py:874`; `app/colaboradores/services/calendario.py:883`) |
| Contadores/rótulos exclusivos das sondas | Escritores e critérios de `rotulo_perdido`, `nasce/some`, `iguais/diferentes` não apresentados [nao verificado]. | Leitura pelos anexos fornecidos [nao verificado]. |

4) **Propriedades verificáveis por máquina, sem LLM.**

São critérios de detecção derivados dos contratos citados; sua execução nesta cópia e seus resultados ficam [nao verificado].

| Classe de divergência | Propriedade exigível |
|---|---|
| **Falta sem decisão** | `status=falta ⇒ decisão descontável válida para o dia`. Casos sintéticos distinguem furo, falta aprovada e abono, sem tomar o caminho antigo como oráculo. (`app/docs/LEIS.md:87`; `app/docs/LEIS.md:136`) |
| **Precedência/realização** | Casos pela regra cobrem ausência integral/parcial, folga humana, feriado, trabalho em feriado, batida e decisão administrativa; o resultado deve respeitar cada condição, inclusive a exceção parcial. (`app/ponto/precedencia.py:11`; `app/ponto/status_dia.py:96`; `app/ponto/portas/celula.py:694`) |
| **Relógio/teto** | Com instante fixo, nenhum encerramento antes do fim previsto ou com turno vivo; sem previsto, motivo `sem_previsto`. Testar fronteiras e turnos atravessando meia-noite. (`app/ponto/turnos.py:2113`; `app/docs/LEIS.md:100`) |
| **Dependência da janela** | Para o mesmo instante e dados, `dias_encerrados(c,I,F)[d] == dia_encerrado(c,d)`; também igualdade entre consulta unitária e lote D4. (`app/ponto/turnos.py:2134`; `app/ponto/status_dia.py:169`) |
| **Buraco de materialização** | Anti-join: todo `(pessoa,data)` com vínculo diário válido no universo obrigatório deve possuir célula. Segregar “sem vínculo” evita contar ausência legítima de domínio como falha. (`app/ponto/portas/celula.py:561`; `app/escala/alimentacao.py:182`) |
| **Nulo/completável/fila** | No mesmo universo elegível, `total = já_lavrado + completável + fila`; aplicação repetida é idempotente e completar status preserva veredito, ata e data do veredito. (`app/ponto/services/cartorio.py:752`; `app/ponto/services/cartorio.py:758`; `app/ponto/portas/celula.py:753`) |
| **Congelamento indevido da referência** | Mantidos insumos e instante, marcar a competência como entregue não pode alterar o resultado da referência viva; a fotografia histórica é outra entidade. Essa propriedade expõe o conflito do lote atual. (`app/docs/ADR/0003-celula-tres-naturezas.md:25`; `app/ponto/management/commands/lavrar_status_competencia.py:40`) |
| **Lavratura desatualizada** | Após consumir os eventos, impressão persistida deve corresponder aos insumos atuais. Teste metamórfico: alteração que muda julgamento precisa mudar a chave de atualidade ou provocar invalidação explícita; variar isenção detecta a lacuna já declarada. (`app/docs/ADR/0002-celula-e-o-dia.md:16`; `app/docs/ADR/0002-celula-e-o-dia.md:59`) |
| **Rótulo perdido** | Comparar o par `(status,rótulo)`, incluindo isenção, andamento e retenção; igualdade apenas do status não satisfaz esse contrato. (`app/ponto/status_dia.py:48`; `app/ponto/status_dia.py:205`; `app/colaboradores/services/calendario.py:886`) |
| **Ação nasce/some** | Verificar `resolver == último_recurso ∧ trabalho ∧ passado ∧ status_pede_decisão`; na migração, comparar a projeção completa sob os mesmos insumos. Para C6, marco satisfeito não pode permanecer cobrado apenas porque outro marco acusa o dia. (`app/colaboradores/services/calendario.py:189`; `app/chamados/juizes.py:1073`) |
| **Rodapé incompleto** | Histograma exclusivo dos status deve somar `len(dias)` e coincidir entre tela, contagem e resumo; HE fica fora dessa soma porque é selo sobreposto. (`app/colaboradores/services/calendario.py:217`; `app/colaboradores/services/calendario.py:935`; `app/colaboradores/services/calendario.py:940`) |
| **Dois leitores concordam no erro** | Casos e propriedades independentes pela regra continuam obrigatórios mesmo com divergência zero; comparação entre implementações só mede coerência/impacto. (`app/docs/LEIS.md:136`) |

## 01/11 17:53 · sessao 81f096571943 · - · - · origem=Ronald (tmux)

Leitura concluída, sem escrita. Os anexos não trazem perguntas, respostas, atas ou marcos; qualquer explicação individual por C6 continua [nao verificado].

**Fato de desenho útil à consolidação**

`_pergunta_viva_calc` não consome `status_dia`/`status_rotulo`: decide por assinatura, tranca, resposta, veredito/via, via/materialização, fato do marco e provisoriedade da ata; ainda chama `classificar_falta` quando não resolveu pelos degraus anteriores. Portanto “status da tela”, “status lavrado” e “pergunta viva” respondem perguntas distintas, com uma recomputação residual no C6. (`app/chamados/juizes.py:1032–1118`; L-061 em `app/docs/LEIS.md:95`.)

C6 distingue explicitamente **marco perguntado** de **dia inteiro**: uma pergunta materializada morre se a lâmpada daquele marco acendeu, mesmo se outro marco mantém o dia acusando. (`app/chamados/juizes.py:1073–1084`.) Em célula não acusadora, `marcos_pendentes` impede absolvição provisória; o fato do próprio marco pode absolvê-la. (`app/chamados/juizes.py:1085–1096`.)

**Regras escritas relevantes**

- L-002: testemunhas leem a mesma autoridade; derivação própria é divergência estrutural. (`app/docs/LEIS.md:60`.)
- L-020: DNA do passado congelado, exceção formal pela porta com denúncia. (`app/docs/LEIS.md:71`.)
- L-039: “falta” significa ausência decidida cujo tipo desconta; não basta furo de batida. (`app/docs/LEIS.md:87`.)
- L-061: chamado, pergunta, tela e folha leem célula/ata. (`app/docs/LEIS.md:95`.)
- L-073: data passada sozinha não prova encerramento; comparar instante ou exigir fato encerrado. (`app/docs/LEIS.md:100`.)
- L-089: atestado só produz efeito em dia previsto para trabalho; o índice registra tensão com classificação em folga. (`app/docs/LEIS.md:121`, `:172`.)
- L-110: comparação velho × novo mede impacto; regra aplicada a cadastro/batidas e propriedades fixas certificam. (`app/docs/LEIS.md:136`.)

**Perguntas de desenho a explicitar — não é possível afirmar historicamente que “ninguém perguntou” [nao verificado]**

1. “Falta” da tela é a falta decidida de L-039, ou ausência de marcação? Sem cadastro da ausência e seu status no anexo não há gabarito individual. (`app/docs/LEIS.md:87`; `app/chamados/juizes.py:1097–1103`.)
2. Quando duas projeções concordam em `status`, concordam também em ação oferecida, marco cobrado, frase e informação provisória? O C6 decide por marco e teto, enquanto o mapa declara `resolver_dia_oferecido` como outro predicado. (`app/chamados/juizes.py:1073–1096`; `app/docs/MAPA/calendario-dia-da-tela.md:10–13`.)
3. A pergunta aponta para a mesma `(colaborador,data)` da célula? O próprio alimentador recusa FK divergente; ausência de célula confiável é `None`, diferente de mapa que afirma inexistência. (`app/chamados/juizes.py:586–606`.)
4. “Sem pergunta viva” significa fato resolvido ou apenas competência trancada? C6 cala a cobrança por leitura de tranca sem absolver o dia. (`app/chamados/juizes.py:1035–1043`.)
5. Qual instante e qual estado dos insumos sustentam a comparação? C6 usa janela viva e ata provisória; o anexo não fornece essa identidade completa. (`app/chamados/juizes.py:533–565`, `:1085–1096`; [nao verificado].)

**Inventário dos campos lidos pelo C6 e escritores efetivamente abertos**

| Campo | Escritor observado | Leitor observado |
|---|---|---|
| `resposta_colab`, `respondida_em` | resposta do colaborador; automática; carimbo pela trilha | C6, ramo resposta |
| | `app/chamados/services/disputa_emissao.py:1247–1266`, `:1381–1413` | `app/chamados/juizes.py:1047–1059` |
| `validada_em`/`validada_por` | `_carimbar_validacao`; resposta nova desfaz; materializador também assina irmãs com texto | C6 encerra antes dos demais degraus |
| | `app/chamados/services/validacao.py:127–133`; `app/chamados/services/disputa_emissao.py:1250–1266`; `app/chamados/services/materializacao.py:513–532` | `app/chamados/juizes.py:1033–1034` |
| `materializada_em`, `via_resolucao` | materializador; portas do modelo para admin, tranca e fato; retratação de duplicata | C6 encerra por identidade ou exige testemunha |
| | `app/chamados/services/materializacao.py:509–532`; `app/chamados/models.py:2485–2541`; `app/chamados/services/fio.py:878–897` | `app/chamados/juizes.py:1042`, `:1066–1079`, `:1107–1118` |
| `celula`, `data_evento`, `motivo`, `tipo_resposta` | modelo declara identidade/snapshot; censo completo de escritores não realizado [nao verificado] | alimentador, lâmpada do marco e C6 |
| | `app/chamados/models.py:2193–2207` | `app/chamados/juizes.py:598–606`, `:863–890`, `:1097–1103` |
| `CelulaDia.veredito`, `veredito_via`, `ata` | modelo delega à porta; ato admin chama `lavrar_veredito`; admin cria ausência aprovada ou materializa batida | C6 e prova do marco |
| | `app/escala/models.py:1787–1796`; `app/ponto/services/veredito_celula.py:299–330` | `app/chamados/juizes.py:1044–1046`, `:1081–1096`, `:887–893` |
| `status_dia`, `status_rotulo` | declaração de escritor na porta e nulidade “ainda não lavrado” | D4 declarado; C6 não os lê |
| | `app/escala/models.py:1775–1785` | `app/escala/models.py:1780`; corpo aberto em `app/chamados/juizes.py:1032–1118` |

**Propriedades mecânicas úteis**

- **Identidade:** toda pergunta usada como prova satisfaz `p.celula.colaborador_id == p.disputa.colaborador_id` e `p.celula.data == p.data_evento`; violações são identidade divergente, não diferença de classificação. (`app/chamados/juizes.py:589–606`.)
- **Falta sem decisão:** `status == falta ⇒ existe ausência decidida descontável para o dia`; distingue furo de falta sem usar o código antigo como oráculo. (`app/docs/LEIS.md:87`, `:136`.)
- **Prova do marco:** pergunta materializada, não respondida pelo ramo especial nem retida por assinatura/tranca, com `fato_do_marco_no_espelho(p) is True`, deve estar morta; dia acusador por outro marco não a revive. (`app/chamados/juizes.py:1033–1079`.)
- **Teto:** célula sem acusação mas com marco pendente não prova encerramento da pergunta sem a lâmpada do próprio marco; executar com relógio fixo permite testar fronteiras antes/depois do vencimento. (`app/chamados/juizes.py:1085–1096`; `app/docs/LEIS.md:100`.)
- **Par resposta:** `respondida_em != None ⇒ trim(resposta_colab) != ''`; o código já nomeia esse estado ilegal. (`app/chamados/juizes.py:1154–1157`.)
- **Idempotência:** repetir a mesma decisão não altera estado/carimbo; portas admin e duplicata têm guardas explícitas. (`app/chamados/models.py:2490–2502`; `app/chamados/services/fio.py:882–897`.)
- **Ação não reduzida a cor:** igualdade de status, isoladamente, não certifica igualdade de `resolver_dia_oferecido` ou de pergunta viva, pois seus predicados têm entradas diferentes. A propriedade verificável é comparar a projeção completa do contrato, sob os mesmos insumos/instante. (`app/docs/MAPA/calendario-dia-da-tela.md:10–13`; `app/chamados/juizes.py:1032–1118`; formulação de propriedade: [nao verificado].)

## 01/11 17:53 · sessao c8a34173d33c · - · - · origem=Ronald (tmux)

Fatos e pontos de desenho confirmados:

- ADR 0002 declara status/rotulo na célula, escritor único no mesmo ato do veredito e relógio derivado; leitores permanecem congelados até E4. `app/docs/ADR/0002-celula-e-o-dia.md:8`, `:12`, `:21`, `:73`.
- Há conflito declarado entre o lote que recusa competência entregue e a natureza viva da célula. A recusa está em `app/ponto/management/commands/lavrar_status_competencia.py:40`; o próprio ADR 0003 registra-a em `app/docs/ADR/0003-celula-tres-naturezas.md:106`, embora declare julgamento vivo em qualquer competência (`:25`) e fotografia somente no gravado (`:27`).
- A L-092 protege o GRAVADO (`app/docs/LEIS.md:125`); L-113 revoga recusa de recálculo em exportada (`:148`). Não cabe justificar congelamento do status vivo apenas pela L-092. O ADR 0002 ainda manda o lote não tocar entregue (`app/docs/ADR/0002-celula-e-o-dia.md:68`).
- L-039 define falta como decidida, ausência cujo tipo desconta (`app/docs/LEIS.md:87`). A regra da lavra trata `nunca_bateu/furo/cobrado/...` como irregular (`app/ponto/status_dia.py:54`, `:112`), enquanto falta requer o `tipo_dia='falta'` (`:96`) ou decisão admin `faltou` (`:52`). Para classificar cada par `falta/ok` do anexo seriam necessários decisão humana, isenção, grade e fatos do mesmo julgamento; estes não estão no dado [nao verificado].
- L-110 exige oráculo pela regra, não igualdade com código anterior; comparação velho/novo mede impacto (`app/docs/LEIS.md:136`). Assim `iguais` não certifica correção e `diferentes` não prova defeito.

D4, com ordem exata:

1. Sem célula → `sem_dados/sem_celula`, antes de relógio ou natureza humana: `app/ponto/status_dia.py:191`.
2. Motivo de não encerramento diferente de `sem_previsto` → `previsto/relogio` quando motivo é relógio e não há turno com aquela `data_turno`; demais casos `aberto/relogio`: `app/ponto/status_dia.py:196`.
3. Depois → coluna lavrada e rótulo persistido, com fallback para rótulo canônico: `app/ponto/status_dia.py:203`.
4. Célula sem status → `previsto/nao_lavrado` se data ≥ hoje, `sem_certidao/nao_lavrado` caso contrário: `app/ponto/status_dia.py:207`.

Logo, `tempo='passado'` não prova encerramento, e “sem janela” segue lavra; o contrato escrito confirma a distinção em `app/ponto/status_dia.py:160`. Os anexos não mostram motivos individuais de encerramento [nao verificado].

Escritores/leitores verificados:

| Campo | Escritor | Leitores verificados |
|---|---|---|
| `status_dia`, `status_rotulo` persistidos | `_aplicar_status`, chamado pela porta `lavrar_veredito`; par atribuído junto em `app/ponto/portas/celula.py:701`; save junto do veredito em `:773` | D4 em `app/ponto/status_dia.py:203`; D5 verifica apenas `status_dia` em `app/ponto/services/cartorio.py:758` |
| Status/rotulo provenientes de decisão humana | `_status_alvo` usa `status_do_admin` quando via/admin prevalece: `app/ponto/portas/celula.py:694`; tabela em `app/ponto/status_dia.py:52` | A própria porta preserva decisão admin durante lavra cartório, podendo atualizar ata/status: `app/ponto/portas/celula.py:737` |
| Status/rotulo provenientes do cartório | `status_da_lavra` sobre fatos e o mesmo veredito gravado, `app/ponto/portas/celula.py:698`; fatos extraídos em `app/ponto/status_dia.py:143` | D4 conforme acima |
| `fonte`, `status`, `rotulo` de retorno do D4 | D4, sem gravação: `app/ponto/status_dia.py:192`, `:199`, `:204`, `:208` | Painel `_d4_em_lote`, atribui resposta à linha após confirmar encerramento: `app/core/services/painel_op.py:334` |
| `impressao`, `julgada_em`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | `carimbar`, `app/ponto/portas/celula.py:800` | D5 compara julgamento/impressão/veredito em `app/ponto/services/cartorio.py:1032` e `:1190` |
| `status_completaveis`, `status_fila_cartorio`, `status_ja_lavrado`, `status_completados` | `_completar_status`, `app/ponto/services/cartorio.py:758` | Command imprime em `app/ponto/management/commands/lavrar_status_competencia.py:57` |

Inventário mais amplo, **declarado pelo ADR, não recenseado por mim**: natureza/fonte/escritor de todas as colunas está em `app/docs/ADR/0003-celula-tres-naturezas.md:35`; leitores são pisos de contagem (`:5`) e não lista completa. `status_dia/status_rotulo` estão nas linhas `:56` e `:57`.

Sobre D5:

- D5 considera já lavrado qualquer `status_dia is not None`, sem verificar rótulo ou atualidade: `app/ponto/services/cartorio.py:758`.
- Nulo com impressão atual é completável; nulo sem fatos atuais é fila cartório: `app/ponto/services/cartorio.py:761`.
- D5 não rejulga, não altera veredito/ata/data do veredito; passa fatos à mesma porta e só completa status: `app/ponto/services/cartorio.py:752`, `app/ponto/portas/celula.py:753`.
- DRY com `completaveis` no anexo2 prova contagem desse ramo na execução relatada, não execução futura do command público nem migração já realizada [nao verificado].

Perguntas de desenho e propriedades mecânicas:

- **Status mede qual pergunta?** Há teste que explicita “dia encerrado tem o que cobrar?” e rejeita usar status como natureza conhecida do dia (`app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:4`). Propriedade: leitores de natureza/precedência não podem importar D4/status nem filtrar por status; AST pode impor contrato por papel, não por igualdade textual de status.
- **Ausência de célula é ausência de vínculo, lacuna materializada ou período fora do universo?** D4 responde igual para todos (`app/ponto/status_dia.py:192`). Propriedade: anti-join entre universo de dias esperado definido pela regra de vigência e `CelulaDia`, segregando dias fora de vínculo; uma contagem de sem-célula sem domínio não distingue defeito.
- **“Lavrado” significa atual?** D4 lê coluna sem comparar impressão (`app/ponto/status_dia.py:203`). Propriedade: após processamento dos eventos, `impressao_gravada == impressao_insumos_atuais` e par status/rotulo corresponde ao julgamento daqueles insumos. Não se pode inferir isso de `fonte='lavrado'`.
- **Todo insumo que muda resultado muda impressão?** ADR 0002 admite isenção fora da impressão (`app/docs/ADR/0002-celula-e-o-dia.md:59`); função de impressão efetivamente recebe batidas/cob_status/chamados/DNA/veto/teto/regra/fio/cobparcial/enc, sem isento (`app/ponto/services/cartorio.py:356`). Propriedade metamórfica: duas entradas com saída de julgamento diferente exigem fingerprint diferente ou invalidação/evento explícito. Teste pode variar isenção, cobertura, feriado, vínculo etc. sem LLM.
- **Rótulo é dado semântico?** Modelo aceita ambos nulos independentemente (`app/escala/models.py:1784`); escritor os muda juntos (`app/ponto/portas/celula.py:705`); D4 reconstrói canônico se rótulo ausente (`app/ponto/status_dia.py:205`). Propriedade: par `(status,rotulo)` completo e preservado em leitura/render, incluindo `ok/isento de ponto` (`app/ponto/status_dia.py:48`). Comparar só status não captura perda de mensagem.
- **Entregue muda elegibilidade de referência viva?** Propriedade: mesmos insumos vivos + mesmo instante dão o mesmo par vivo independentemente do flag entregue; hash do gravado permanece intacto. Base declarada: `app/docs/ADR/0003-celula-tres-naturezas.md:25` e `:27`. O command atual contradiz a primeira metade em `app/ponto/management/commands/lavrar_status_competencia.py:40`.
- **Migração está completa?** Propriedade contábil por partição: `total = ja_lavrado + completaveis + fila_cartorio` sobre o mesmo universo elegível, com testes de DRY sem escrita, segunda aplicação sem mudança e preservação de veredito/ata/data. Ramos em `app/ponto/services/cartorio.py:758`; testes atuais de idempotência em `app/ponto/tests/test_e3_status_lavrado.py:271`. Não executei testes nem consultei banco [nao verificado].

## 01/11 17:53 · sessao a8a6c2bba5fc · - · - · origem=Ronald (tmux)

Achados com evidência direta:

- `passado` não significa encerrado: encerramento exige ausência de turno vivo e fim previsto ultrapassado; sem previsto, a resposta é `sem_previsto`, mesmo para data passada (`app/ponto/turnos.py:2034`, `app/ponto/turnos.py:2050`, `app/ponto/turnos.py:2113`). Logo a coluna `tempo` do anexo não decide legitimidade de `falta`, `aberto` ou `previsto`; sem instante, DNA, turnos e motivo por linha, isso permanece [nao verificado].
- Há dois eixos: jurisdição do vínculo e previsão congelada. `vinculo_do_dia` rejeita geradora fora da vigência e retorna `None`, sem substituir por outro vínculo (`app/escala/alimentacao.py:237`); a precedência ainda lê `celula.trabalha` diretamente quando não nulo (`app/ponto/precedencia.py:349`, `app/ponto/precedencia.py:359`). Pergunta necessária: o que uma célula com previsão, mas sem jurisdição válida, pode afirmar e cobrar?
- A regra explícita é “todo dia de vínculo vigente tem célula; afastamento/ausência cobrem a célula” (`app/ponto/portas/celula.py:561`). Porém o gerador procura apenas vínculos atualmente `ativa=True` (`app/ponto/portas/celula.py:568`), enquanto leitores admitem vínculos encerrados com `data_fim` (`app/escala/alimentacao.py:116`, `app/escala/alimentacao.py:173`). Pergunta necessária: qual universo histórico deve ser materializado?
- Portanto os 120 dias do anexo classificados como `vinculo_ja_existia` — 114 sem fato, quatro feriados, dois atestados — são candidatos à violação de cobertura, mas o predicado da sonda e a validade temporal daqueles vínculos não foram fornecidos [nao verificado]. Os outros 1.110 dias sem vínculo não provam defeito por haver situação atual viva [nao verificado].
- Ausência e feriado podem existir independentemente de vínculo/célula: a precedência carrega ausências por pessoa e intervalo e acrescenta feriado antes de formar o vencedor (`app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:375`). Logo `ausencia/folga` na tela versus `sem_dados/sem_celula` pode representar universos diferentes, sem permitir concluir qual resultado específico é devido [nao verificado].
- A precedência resolve “fato que vence”: férias > afastamento > atestado > feriado > folga > trabalho (`app/ponto/precedencia.py:46`); não resolve, sozinha, realização ou certificado. Folga humana e feriado têm predicado próprio (`app/ponto/precedencia.py:178`). Folga de calendário só vale no vínculo dela; folga manual pertence à pessoa (`app/ponto/precedencia.py:222`).
- O contrato de janela promete igualdade com chamada diária (`app/ponto/turnos.py:2134`), embora o pareamento dependa da janela e a alimentação deva ser da mesma janela (`app/ponto/turnos.py:2128`). O anexo informa `janela_x_dia_dia_a_dia {}`; não mostra violação desse contrato [nao verificado].

Escritores/leitores abertos:

| Campos | Escritor observado/declarado | Leitores abertos |
|---|---|---|
| `trabalha`, `dna`, `escala_geradora` | Geração (`app/ponto/portas/celula.py:593`), regeneração (`app/ponto/portas/celula.py:243`), reversão (`app/ponto/portas/celula.py:532`). Modelo admite edição humana, mas seu executor não foi localizado nesta leitura (`app/escala/models.py:1606`). | Precedência (`app/ponto/precedencia.py:349`); escolha do vínculo (`app/escala/alimentacao.py:237`); turnos (`app/ponto/turnos.py:1882`); encerramento (`app/ponto/turnos.py:2164`). |
| `origem`, trilha da regeneração | Geração marca `gerada` (`app/ponto/portas/celula.py:595`); regeneração guarda DNA anterior, instante e contador (`app/ponto/portas/celula.py:259`). | Gerador preserva `editada` e passado (`app/ponto/portas/celula.py:598`); regeneração restringe às geradas (`app/ponto/portas/celula.py:251`). |
| Vigência `data_inicio/data_fim/ativa` | `validar_vigencia/fechar_vigencia` são donos declarados na L-021; implementação não foi aberta (`app/docs/LEIS.md:72`). | Carregador de candidatos (`app/escala/alimentacao.py:116`), cobertura (`app/escala/alimentacao.py:173`), vínculo diário (`app/escala/alimentacao.py:247`), gerador (`app/ponto/portas/celula.py:588`). |
| `FolgaDia`/`calendario_origem` | Executor de escrita não aberto [nao verificado]. | Carga de todas as folgas da pessoa (`app/ponto/precedencia.py:194`), validade pelo vínculo ou origem manual (`app/ponto/precedencia.py:222`), precedência (`app/ponto/precedencia.py:358`). |
| `status_dia/status_rotulo` | Escritor declarado: porta `lavrar_veredito`, regra `status_da_lavra`; não o gerador (`app/escala/models.py:1775`). | Leitor D4 declarado `status_do_dia_lavrado`; migração das telas é E4 (`app/escala/models.py:1779`). |
| `veredito/ata` e `julgada_em/impressao` | Operações `lavrar_veredito` e `carimbar` da porta (`app/ponto/portas/celula.py:6`). `ata.precedencia` tem dono específico (`app/ponto/portas/celula.py:94`). | Os comentários do modelo declaram folha/exportação como leitores da ata (`app/escala/models.py:1742`); chamadas concretas da folha não abertas [nao verificado]. |
| Motivo de encerramento | Derivado puro, sem persistência: turno vivo, sem previsto ou relógio (`app/ponto/turnos.py:2105`). | `dia_encerrado` (`app/ponto/turnos.py:2075`), janela (`app/ponto/turnos.py:2164`), carga em lote (`app/ponto/turnos.py:3009`). |

Propriedades verificáveis por máquina, como especificações de detecção:

1. **Cobertura:** para cada pessoa/data cujo vínculo diário válido existe dentro do escopo obrigatório, existe exatamente uma célula. A obrigação está em `app/ponto/portas/celula.py:561`; unicidade existe em `app/escala/models.py:1801`. Separar contadores por vínculo histórico/atual e por fato humano permite detectar buracos sem confundir situação atual com vigência.
2. **Proveniência:** geradora pertence à pessoa e sua vigência contém a data; nenhuma escolha muda ao reordenar ou acrescentar candidatos irrelevantes. As regras de autoria/candidatura estão em `app/escala/alimentacao.py:96`, `app/escala/alimentacao.py:237`.
3. **Precedência:** `fatos_do_dia(c,d) == fatos_do_periodo(c,d,d)[d]`, já garantido por delegação (`app/ponto/precedencia.py:242`); fornecimento completo de insumos versus carregamento interno produz o mesmo resultado (`app/ponto/precedencia.py:263`). Folga de outro calendário não altera o vencedor; folga manual válida altera conforme a ordem (`app/ponto/precedencia.py:222`).
4. **Encerramento:** `dias_encerrados(c,I,F,t)[d] == dia_encerrado(c,d,t)` e `bool == (motivo is None)` (`app/ponto/turnos.py:2075`, `app/ponto/turnos.py:2134`). Há teste existente dessas propriedades (`app/ponto/tests/test_c1_dias_encerrados.py:26`, `app/ponto/tests/test_c1_dias_encerrados.py:65`); execução nesta cópia não verificada.
5. **Teto temporal:** nenhum dia é encerrado antes do fim previsto congelado; turno ainda vivo impede encerramento; sem previsto impede encerramento. Fórmula executável em `app/ponto/turnos.py:2113`. Permite distinguir divergência por relógio de divergência por fato/cadastro.
6. **Imutabilidade/idempotência:** repetir geração mantém célula editada e passado; repetir operação com mesmos insumos não altera conteúdo nem contador de regeneração (`app/ponto/portas/celula.py:19`, `app/ponto/portas/celula.py:598`). Após exportação, comparação não autoriza escrita: L-092 (`app/docs/LEIS.md:125`).
7. **Referência:** igualdade entre leitores detecta incoerência estrutural, mas não prova valor correto: a referência deve ser regra aplicada a cadastro e batidas, conforme L-110 (`app/docs/LEIS.md:136`). O anexo não contém esses insumos por dia; nenhum dos dois status pode ser eleito gabarito só por sua origem [nao verificado].

## 01/11 17:51 · sessao 06c9e96978f9 · O261-E-F2 · verificador · origem=Code

**A cobertura é parcial:** os quatro testes podem continuar verdes sem a guarda do fecho; o MORDE do ramo não chega ao vigia; e o caso 2 usa um veredito que a porta de produção rejeita. — `app/chamados/services/disputa_emissao.py:1626`; `app/chamados/models.py:2569`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:171`; `app/ponto/portas/celula.py:658`.

Esta é uma análise estática da cópia. Execução dos testes, resultados dos logs citados no cabeçalho, suíte completa e correspondência Git com `7af133ad`/`cd623758`: **[não verificado]**.

Para compactar as referências, os caminhos abaixo são relativos a `app/`, salvo `CLAUDE.md`.

**1. O que os quatro testes provam** [nao verificado]

- **T1 — `test_ramo_variante…`:** chama o C6 real, `resolver_perguntas_por_chamado_resolvido` real e `censo_alcance` real. Entretanto, `_fechar` sobrescreve novamente o estado da célula depois da função de produção. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:155`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:181`.
  
  As asserções medem: C6 vivo antes; retorno dos contadores igual a `{0,0}`; `materializada_em` nulo; `via_resolucao` vazia; C6 vivo depois; exatamente uma linha daquela célula; rota `sem_porta`; detalhe `pergunta_viva_sob_chamado_terminal`; universo e dias sem porta iguais a `(2,2)`. **O índice 6 verificado é o detalhe; a causa está no índice 5 e não é verificada.** — `chamados/tests/test_o261e_f2_ramo_e_i20.py:184`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:168`; `chamados/services/vigia_fio.py:585`.

  O contador `perguntas_resolvidas=0` não prova que o ramo sem credor deixou de carimbar: `n_pq` é incrementado no primeiro ramo, com `chamado_destino`; o ramo sem credor não o incrementa. A pergunta da fixture tem destino nulo. — `chamados/services/disputa_emissao.py:1537`, `chamados/services/disputa_emissao.py:1569`, `chamados/services/disputa_emissao.py:1638`, `chamados/services/disputa_emissao.py:1661`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:142`.

- **T2 — `test_MORDE_ramo…`:** chama o fecho real com `pergunta_viva` substituído por `False`; exige que tenha nascido materialização e que `_assert_ramo` lance `AssertionError`. Isso demonstra rejeição do carimbo pela primeira asserção do helper. **Não demonstra uma leitura adversarial do vigia**, porque o helper aborta antes dela. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:193`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:196`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`.

- **T3 — `test_caso2_dia_regularizado…`:** chama C6 real, fecho real, vigia real e auditor real por `call_command`. Verifica C6 morto inicialmente; manutenção de `('07:55', None, None)` em texto/respondida/materializada; PK fora da cicatriz; ausência da célula no alcance; totais `(1,1)`; I20 com `VIOL 1`; e presença da representação textual do PK na linha. Não verifica o retorno do fecho nem o código de saída do auditor. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:207`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:202`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:79`.

- **T4 — `test_MORDE_caso2…`:** chama o fecho real, executa uma cópia integral do auditor com o PK acrescentado à cicatriz e depois executa o comando real. Verifica âncora textual única; cópia com `OK 0`; rejeição dessa saída pelo helper que exige `VIOL 1`; e comando real contando novamente a pergunta. O `assertRaises` da cópia é satisfeito pela divergência de flag/total, antes da checagem do PK. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:222`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:204`.

Os helpers também exigem uma única atribuição AST de `_I20_CICATRIZ` e uma única linha I20 reconhecível na saída. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:65`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:72`.

**Mutantes concretos, sem aplicá-los**

| Mutante | Alteração de produção de 1–3 linhas |
|---|---|
| **F — desligar guarda do fecho** | Trocar `if pergunta_viva(q):` por `if False:`. — `chamados/services/disputa_emissao.py:1626` |
| **P — desligar recusa da porta** | Trocar `if viva:` por `if False:`. — `chamados/models.py:2569` |
| **Z — silenciar I20** | Substituir o queryset passado a `add('I20',…)` por `PD.objects.none()`. — `chamados/management/commands/auditar_invariantes_chamados.py:1101` |
| **C — remover exclusão da cicatriz** | Remover `.exclude(pk__in=_I20_CICATRIZ)`. — `chamados/management/commands/auditar_invariantes_chamados.py:1103` |
| **R — remover filtro do instante** | Trocar `PD.objects.filter(respondida_em__isnull=True)` por `PD.objects.all()`, preservando as exclusões. — `chamados/management/commands/auditar_invariantes_chamados.py:1102` |

Veredictos **previstos por leitura estática**, supondo a fixture isolada, PK fora da cicatriz e execução-base verde; execução efetiva: **[não verificado]**. Os pontos de observação são T1/T2/T3/T4 acima. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:181`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:188`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:207`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:222`.

| Mutante | T1 ramo | T2 MORDE ramo | T3 caso 2 | T4 MORDE I20 |
|---|---:|---:|---:|---:|
| F | Verde | Verde | Verde | Verde | [nao verificado]
| P | Verde | Verde | Vermelho | Vermelho | [nao verificado]
| F + P | Vermelho | Verde | Vermelho | Vermelho | [nao verificado]
| Z | Verde | Verde | Vermelho | Vermelho | [nao verificado]
| C | Verde | Verde | Verde | Vermelho | [nao verificado]
| R | Verde | Verde | Verde | Verde | [nao verificado]

A redundância explica F e P: sem F, a porta aplica provisoriamente o carimbo e `respondida_em`, consulta C6 e desfaz; sem P, T1 continua pulando antes da porta. T3 alcança a porta e denuncia a remoção de sua recusa. — `chamados/services/disputa_emissao.py:1626`; `chamados/models.py:2555`, `chamados/models.py:2565`, `chamados/models.py:2569`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:214`.

R sobrevive porque não existe controle negativo com texto **e** `respondida_em` preenchido; a fixture oferece apenas o caso positivo do meio-estado. Portanto os testes não prendem integralmente o predicado do I20. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:214`; `chamados/management/commands/auditar_invariantes_chamados.py:1102`.

**2. Quem escreve e quem lê**

| Campo | Escrita de produção | Leituras de produção relevantes |
|---|---|---|
| `PerguntaDisputa.resposta_colab` | Resposta pelo core, gravação automática e limpeza na reabertura. — `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1393`, `chamados/services/disputa_emissao.py:1358` | C6, respostas sem veredito, materializador e I20. — `chamados/juizes.py:1047`; `chamados/models.py:1767`; `chamados/services/materializacao.py:763`; `chamados/management/commands/auditar_invariantes_chamados.py:1101` |
| `PerguntaDisputa.respondida_em` | Escritores do par acima; recuperação pela trilha; `carimbar` quando solicitado e com texto; limpeza na ressuscitação. — `chamados/services/disputa_emissao.py:1408`; `chamados/models.py:2556`, `chamados/models.py:1488` | C6, `.estado`, respostas sem veredito e I20. — `chamados/juizes.py:1047`; `chamados/models.py:2477`, `chamados/models.py:1769`; `chamados/management/commands/auditar_invariantes_chamados.py:1102` |
| `PerguntaDisputa.materializada_em` | `_escrever_carimbo`, persistido pelas portas `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote`. — `chamados/models.py:2527`, `chamados/models.py:2533`, `chamados/models.py:2588`, `chamados/models.py:2609`, `chamados/models.py:2619` | C6, seleção do fecho, respostas sem veredito e `.estado`. — `chamados/juizes.py:1071`, `chamados/juizes.py:1077`; `chamados/services/disputa_emissao.py:1620`; `chamados/models.py:1768`, `chamados/models.py:2480` |
| `PerguntaDisputa.via_resolucao` | Portas do carimbo; `encerrar_por_fato_no_espelho` possui operação explícita que troca apenas a via. — `chamados/models.py:2527`, `chamados/models.py:2629` | C6 e seleção/trilha do fecho. — `chamados/juizes.py:1042`, `chamados/juizes.py:1071`; `chamados/services/disputa_emissao.py:1628` |
| `CelulaDia.veredito` | `CelulaDia.lavrar_veredito` delega à porta da célula. — `escala/models.py:1787`; `ponto/portas/celula.py:770` | C6 e retenção da folha, que fornece o universo do vigia. — `chamados/juizes.py:1081`; `folha/export.py:1215`; `chamados/services/vigia_fio.py:545` |
| `CelulaDia.veredito_via` | Mesma porta de lavra. — `ponto/portas/celula.py:771` | C6 distingue decisão do admin; a própria lavra preserva sua precedência. — `chamados/juizes.py:1045`; `ponto/portas/celula.py:738` |
| `CelulaDia.ata` | Porta de lavra; chave `precedencia` por `carimbar_precedencia`. — `ponto/portas/celula.py:768`, `ponto/portas/celula.py:811` | Fato do marco, concordância com a luz e marcos pendentes usados pelo C6. — `chamados/juizes.py:887`, `chamados/juizes.py:1008`, `chamados/juizes.py:1094` |
| `DisputaSupervisao.fechada_em` | `fechar`, `reabrir_admin`, `reabrir_sistema`. — `chamados/models.py:1845`, `chamados/models.py:2047`, `chamados/models.py:2071` | Seleção/fechamento do serviço, badge e signal de materialização. — `chamados/services/disputa_emissao.py:1608`, `chamados/services/disputa_emissao.py:1658`; `chamados/models.py:1721`; `chamados/signals.py:130` |
| `ChamadoColaborador.status_local` | Transições por `_gravar_estado`; nascimento por `abrir`/`criar`. — `chamados/models.py:195`, `chamados/models.py:222`, `chamados/models.py:897`, `chamados/models.py:1046` | Credores do fecho, vigia e signal de rejulgamento da célula. — `chamados/services/disputa_emissao.py:1613`, `chamados/services/disputa_emissao.py:1616`; `chamados/services/vigia_fio.py:569`, `chamados/services/vigia_fio.py:578`; `chamados/signals.py:169` |

Há três distinções importantes na montagem:

- **Meio-estado injetado:** o teste escreve só `resposta_colab` por `QuerySet.update`; o core normal grava texto e instante juntos, e a reabertura limpa ambos. Isso é uma entrada adversarial para o tripwire, mas não prova um caminho atual que a produza. O auditor documenta cicatrizes históricas; ocorrência atual em produção: **[não verificado]**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`; `chamados/services/disputa_emissao.py:1247`, `chamados/services/disputa_emissao.py:1358`; `chamados/management/commands/auditar_invariantes_chamados.py:1089`.

- **Estado exato inválido no caso 2:** `_cenario('ok', ACESA)` acaba gravando `veredito='ok', veredito_via='cartorio'` diretamente. `ok` não pertence a `VEREDITOS_CARTORIO`; a porta lança `ValueError`. A lavra sem códigos produz `concorde`. Portanto esse estado exato **não sai da porta canônica**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:209`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`; `escala/models.py:1719`, `escala/models.py:1596`; `ponto/portas/celula.py:658`.

- **Resultado da célula sobrescrito:** `_fechar` chama produção e depois restaura veredito/ata por `update`, sem passar pela lavra que coordena instante, protestos, status e evento. O vigia mede essa célula imposta pelo teste, sem comprovar o resultado integral deixado pela função de produção. O chamado também nasce já resolvido, sem testar sua transição; não há asserção direta posterior de `status_local` ou `fechada_em`. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:135`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:157`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:185`; `ponto/portas/celula.py:770`.

**3. Testemunha e universo do auditor**

O `mock.patch` cobre somente `_fechar`. Já terminou durante o `refresh_from_db`, a asserção de materialização e o `assertRaises`. Contudo, `_assert_ramo` encontra o carimbo e falha em sua primeira asserção, antes do C6 real e antes de `censo_alcance`. **Nenhuma leitura do vigia acontece nesse MORDE, sob ou depois do patch.** — `chamados/tests/test_o261e_f2_ramo_e_i20.py:193`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:195`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:174`.

A cópia textual executa a mesma implementação de `Command.handle`, com os mesmos defaults relevantes: `amostra=5`, `html=False`. O conjunto candidato do I20 é global: perguntas com instante nulo e texto diferente de `''`, sem filtro por empresa, data, disputa ou materialização. **O conjunto final difere intencionalmente pela exclusão adicional de um PK.** `add` conta tudo e limita apenas a amostra. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:90`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:228`; `chamados/management/commands/auditar_invariantes_chamados.py:549`, `chamados/management/commands/auditar_invariantes_chamados.py:559`, `chamados/management/commands/auditar_invariantes_chamados.py:1101`.

A cópia chama `handle` diretamente; o controle chama `call_command`. Logo, a consulta I20 é comparável, mas a cópia não exercita toda a infraestrutura de despacho do comando. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:85`.

**4. Relógio, empresas, PK e formato**

- **Relógio:** há datas literais, `hoje` explícito para o vigia e `freeze_time` na classe. Fecho e lavra leem `now`; I20 não tem predicado temporal. Não há evidência suficiente para acusar um vazamento concreto de relógio. Cobertura efetiva do `setUpTestData` e de todas as dependências pelo decorador: **[não verificado]**. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:43`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:96`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`; `chamados/services/disputa_emissao.py:1623`; `ponto/portas/celula.py:772`; `chamados/management/commands/auditar_invariantes_chamados.py:1101`.

- **`empresas_visiveis`: sim, depende.** Passar `[self.emp]` faz interseção com as visíveis; não substitui essa fonte. A empresa criada satisfaz o filtro atual de ativa e CNPJ fora do prefixo ocultado. O carregador da lista repete a interseção. — `chamados/services/vigia_fio.py:535`, `chamados/services/vigia_fio.py:210`; `colaboradores/queries.py:85`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:104`.

- **PK/sequência:** o PK é automático, sem reserva explícita fora da cicatriz. T3 detecta colisão por `assertNotIn`; T4 detecta ao exigir que o comando real conte no final. Assim, colisão pode causar vermelho sem regressão semântica; não produz o verde esperado silenciosamente. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:141`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:215`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:245`; `chamados/management/commands/auditar_invariantes_chamados.py:1098`.

- **Formato:** o offset fixo `68` corresponde ao formato atual. O comando trunca a descrição em 58 caracteres, não a linha nem a amostra de PKs. Linha ausente, duplicada ou `ERR` provoca falha; mudança de layout também pode quebrar o parser. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:69`; `chamados/management/commands/auditar_invariantes_chamados.py:1833`.

- **Identidade fraca:** `assertIn(str(p.pk), ln)` busca substring na linha inteira; não verifica o PK como elemento da amostra e aceita coincidências parciais. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:205`.

**5. Juiz, escritor ou derivação próprios; selos da casa**

Não há predicado novo de vitalidade ou retenção: o teste lê C6, chama o censo real e seleciona sua linha por célula. Os helpers AST e de saída interpretam configuração/protocolo. Há, porém, escrita própria da fixture sobre campos de estado e reposição da célula depois do fecho. — `chamados/tests/test_o261e_f2_ramo_e_i20.py:59`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:69`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:143`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:152`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:163`.

**Não cabe afirmar que os selos estáticos reprovariam esse arquivo:** os censos de escritores e os contratos de par resposta, porta da célula e carimbo excluem testes. As escritas seriam problemáticas como produção, mas essa exclusão impede atribuir-lhes um vermelho automático neste teste. — `core/censo_escritas.py:66`; `chamados/tests/test_contract_par_resposta.py:54`; `ponto/tests/test_selo_veredito_so_pela_porta.py:173`; `chamados/tests/test_o261e_f2_porta_carimbo.py:208`.

A insuficiência sustentada pelas regras da casa é precisa: a mordida denuncia um carimbo, mas não executa a leitura adversarial do vigia; e nenhuma das quatro provas prende isoladamente a guarda do fecho. Isso limita a alegação de proteção da costura, diante da exigência de função real e caso que morde. — `CLAUDE.md:318`; `chamados/tests/test_o261e_f2_ramo_e_i20.py:171`, `chamados/tests/test_o261e_f2_ramo_e_i20.py:198`; `chamados/services/disputa_emissao.py:1626`; `chamados/models.py:2569`.

## 01/11 17:49 · sessao bcdff4d29137 · - · - · origem=Ronald (tmux)

Q3 — **mock fora da testemunha, mas vigia não é alcançado no MORDE**. O patch cobre somente `_fechar` (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:193`); já terminou quando ocorre `refresh_from_db` e a asserção de materialização (`:195`, `:196`). `_assert_ramo` começa recarregando o objeto e exigindo `materializada_em is None` (`:170`, `:171`), condição incompatível com a asserção imediatamente anterior do MORDE (`:196`). Assim, o `assertRaises` é satisfeito em `:171`; não chega ao C6 de `:173`, ao censo de `:174`, nem às suas asserções `:175-179`. O MORDE demonstra que o selo rejeita o carimbo indevido, mas não executa o vigia nesse caminho (`:196-198`).

A cópia do auditor mede **o mesmo universo I20**, com apenas a cicatriz ampliada: copia o módulo inteiro, exige uma ocorrência da âncora e modifica essa ocorrência (`:228-239`); ambos usam `amostra=5`, `html=False` (`:86`, `:90`; `app/chamados/management/commands/auditar_invariantes_chamados.py:549-550`). O I20 real usa todas as `PerguntaDisputa` com `respondida_em` nulo e resposta diferente de `''`, excluindo a tupla; não filtra empresa, dia, estado de disputa ou materialização (`auditar_invariantes_chamados.py:1101-1103`). `add` conta o queryset inteiro e limita somente a amostra (`:559-566`). A via da cópia chama `handle` diretamente, portanto não testa a infraestrutura completa de `call_command`, mas a consulta I20 executada é a mesma (`teste:85-90`).

Q4 — O cenário usa datas fixas e passa `hoje` explicitamente ao censo (`teste:43-47`, `:163`); a classe é decorada com `freeze_time` (`:96`). Há leituras de relógio no fecho e carimbo (`app/chamados/services/disputa_emissao.py:1623`; `app/chamados/models.py:2550`), no auditor (`auditar_invariantes_chamados.py:635`, `:1589`) e na lavra feita por `setUpTestData` (`teste:119`, `:125`; `app/ponto/portas/celula.py:772`). A implementação de freezegun não está na cópia; a cobertura efetiva de `setUpClass/setUpTestData` pelo decorador não foi demonstrada nesta leitura **[não verificado]**. Não há evidência suficiente para acusar concretamente um relógio fora do freeze; o pin disponível é `freezegun==1.5.5` (`app/requirements.txt:43`).

Existe dependência de `empresas_visiveis` mesmo passando `empresas=[self.emp]`: o censo obtém as visíveis e depois faz interseção (`app/chamados/services/vigia_fio.py:535-538`); o carregador da lista repete essa política (`:210-215`). A fixture satisfaz o filtro atual, pois cria empresa ativa e CNPJ fora do prefixo ocultado (`teste:104-105`; `app/colaboradores/queries.py:85-87`).

O pk não é escolhido fora da cicatriz: nasce por `objects.create` (`teste:141-142`). O teste normal detecta colisão e falha explicitamente (`:215`); no MORDE, a chamada final ao comando real também exigirá contagem 1 e falhará se o caso já estiver na cicatriz (`:245`, `:204`). Portanto é dependência de sequência, protegida contra falso verde por falha explícita, mas sem isolamento da sequência no próprio arquivo (`:97-99`, `:141-142`, `:215`). [nao verificado]

O parser corresponde ao formato atual: exige exatamente uma linha I20 (`teste:71-72`); o offset `68` de `:75` coincide com flag de quatro caracteres + espaço, id de quatro + espaço e descrição de 58 (`auditar_invariantes_chamados.py:1835-1836`). A descrição é truncada a 58; a amostra de pks não é truncada nessa formatação (`:1835-1836`). Mudança no layout ou linha `ERR` torna o parser falho por exceção, sem falso verde (`teste:75`). A identificação do pk é apenas substring (`:205`), portanto é mais fraca que validar o elemento da amostra; pode coincidir com outro trecho ou pk maior (`:205`; formato em `auditor:1835-1836`). [nao verificado]

Q5 — Os helpers consultam o C6 real e o censo real; filtram o resultado por `cel.pk`, sem construir um predicado próprio de vitalidade ou retenção (`teste:163-164`, `:169-179`). `_cicatriz_i20` lê o AST e `_linha_i20` interpreta o protocolo de saída; nenhum deles decide o estado de produção (`:59-76`). A cópia mutante constitui a própria implementação do auditor com uma exclusão alterada, acompanhada pela execução real como controle (`:228-245`). [nao verificado]

Há **escritas diretas de fixture fora das portas** em resposta e estado da célula (`teste:143`, `:152-153`), inclusive reposição da ata após o fecho (`:157-158`); isso limita a prova de integração do efeito do fecho sobre a célula. Não equivale automaticamente a escritor novo de produção: o censo exclui `tests/` e arquivos `test_*.py` (`app/core/censo_escritas.py:66-82`), e o selo P13 faz o mesmo (`app/core/tests/test_contract_escritores_estado.py:189-192`). O gate de célula usa esse censo (`app/ponto/tests/test_chokepoint_celula_gate.py:49-53`). Não afirmar que esses selos reprovariam o arquivo por esses updates.

A crítica amparada pela casa é a limitação da mordida: exige-se função real e caso que denuncie a costura rompida (`CLAUDE.md:318-325`), e a testemunha deve ler a autoridade (`CLAUDE.md:20-22`, `:35-38`). A mordida do ramo efetivamente denuncia o carimbo, mas não chega a medir a costura do vigia (`teste:171`, `:174`, `:196-198`). Resultado de execução dos selos/suíte: **[não verificado]**. [nao verificado]

## 01/11 17:49 · sessao 60d6079d4737 · - · - · origem=Ronald (tmux)

Análise estática; resultados reais da suíte e de mutações executadas permanecem **[não verificado]**. [nao verificado]

**Chamadas e asserções**

- **T1, ramo**: chama `resolver_perguntas_por_chamado_resolvido` por `_fechar`; verifica C6 vivo antes, retorno `{0,0}`, ausência de materialização/via, C6 vivo depois, uma linha da célula, rota `sem_porta`, **detalhe** `pergunta_viva_sob_chamado_terminal` e totais `(2,2)`. O índice 6 é detalhe, não causa. Fontes: `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:155`, `:168`, `:181`; estrutura da linha em `app/chamados/services/vigia_fio.py:585`.
- **T2, MORDE ramo**: chama o mesmo fecho com C6 falso, exige materialização e exige que `_assert_ramo` falhe. **A falha ocorre na primeira asserção, `assertIsNone(materializada_em)`; portanto não executa C6 real nem `censo_alcance` nessa contraprova**. Fontes: teste `:168`, `:188`. [nao verificado]
- **T3, caso2**: chama C6 real, fecho real, vigia real e comando real por `call_command`. Verifica C6 morto inicialmente, tripla texto/timestamps intacta, PK fora da cicatriz, nenhuma linha do dia, totais `(1,1)`, I20 `VIOL 1` e substring do PK na linha. Não verifica retorno do fecho nem RC do auditor. Fontes: teste `:79`, `:202`, `:207`. [nao verificado]
- **T4, MORDE I20**: chama fecho real, executa cópia integral do comando com PK adicional na cicatriz e depois comando real. Verifica âncora textual única; cópia `OK 0`; rejeição pelo helper que exige `VIOL 1`; comando real `VIOL 1` e presença textual do PK. Fontes: teste `:222`, `:202`. [nao verificado]

**Matriz de mutantes de produção — inferência estática**

V = passaria; F = falharia, considerando o universo isolado esperado pela fixture. As referências dos efeitos são os testes acima, o fecho em `app/chamados/services/disputa_emissao.py:1624`, a porta em `app/chamados/models.py:2554` e I20 em `app/chamados/management/commands/auditar_invariantes_chamados.py:1098`.

| Mutante concreto | T1 | T2 | T3 | T4 |
|---|---:|---:|---:|---:|
| Fecho `:1626`: substituir `if pergunta_viva(q):` por `if False:` | V | V | V | V |
| Porta `models.py:2569`: substituir `if viva:` por `if False:` | V | V | F | F |
| Aplicar os dois acima | F | V | F | F |
| I20 `:1103`: remover `.exclude(pk__in=_I20_CICATRIZ)` | V | V | V | F |
| I20 `:1103`: acrescentar `.none()` ao queryset | V | V | F | F |
| I20 `:1102`: remover só `.filter(respondida_em__isnull=True)` | V | V | V | V |

A redundância explica a primeira linha: sem guarda do fecho, `carimbar` aplica `respondida_em`, consulta C6, desfaz e recusa; sem recusa da porta, T1 ainda pula antes. No caso2, aplicar `respondida_em` torna a resposta viva se não houver concordância verificável com a luz. Fontes: `app/chamados/models.py:2555`, `:2565`, `:2569`; `app/chamados/juizes.py:1047`, `:1059`; `app/chamados/services/disputa_emissao.py:1626`.

**Mock e universo**

- O patch termina em teste `:194`; nenhuma leitura posterior permanece mockada. Mas o MORDE ramo tampouco chega ao vigia depois: `_assert_ramo` aborta em `:171`, antes de `:173–174`. Fontes: teste `:193`, `:195`, `:197`. [nao verificado]
- A cópia I20 executa `Command.handle(amostra=5, html=False)`, usa os mesmos modelos/banco e reproduz todo o módulo; só muda a tupla. Seu universo candidato é o global de perguntas com `respondida_em NULL` e texto diferente de `''`, sem filtro por empresa/data/disputa/materialização. O universo **após a exclusão** muda intencionalmente por um PK. Fontes: teste `:88`, `:228`, `:231`, `:236`; auditor `:552`, `:570`, `:1101`. [nao verificado]

**Relógio, PK e parsing**

- Há freeze na classe e `hoje` explícito para o vigia; o fecho usa `timezone.now` dentro da chamada e I20 não filtra por relógio. Não identifiquei relógio externo determinando I20 nesses trechos; cobertura de toda dependência transitiva é **[não verificado]**. Fontes: teste `:47`, `:96`, `:163`; fecho `:1623`; auditor `:1101`. [nao verificado]
- Existe dependência de `empresas_visiveis`: `empresas=[self.emp]` é interseção, não substituição. A fixture satisfaz o filtro atual porque cria empresa ativa e CNPJ fora do prefixo de diagnóstico. Fontes: `app/chamados/services/vigia_fio.py:534`, `:210`; teste `:104`; `app/colaboradores/queries.py:78`.
- Não reserva PK nem reinicia sequência explicitamente; T3 detecta colisão com cicatriz por `assertNotIn`, T4 detecta só ao exigir contagem real final. Isso pode produzir vermelho por identidade histórica, sem regressão semântica. Fontes: teste `:97`, `:141`, `:215`, `:245`; auditor `:1098`. [nao verificado]
- O parser exige exatamente uma linha I20 e usa offset fixo 68; corresponde ao formato atual. O comando trunca **descrição** a 58, não linha/amostra. Alteração no formato pode quebrar o teste; linha ausente/ERR não passa silenciosamente. Fontes: teste `:56`, `:69`; auditor `:1833`. [nao verificado]
- A identificação do PK usa substring em toda a linha, não parsing da lista de amostra; pode coincidir parcialmente com outro número. Fontes: teste `:202`; auditor `:1835`. [nao verificado]

## 01/11 17:49 · sessao 61e68756283b · - · - · origem=Ronald (tmux)

Q2 — fontes e limites encontrados:

| Campo | Escritor de produção | Leitor de produção relevante |
|---|---|---|
| `PerguntaDisputa.resposta_colab` | `responder_pergunta_disputa_core`, que grava texto e instante juntos (`app/chamados/services/disputa_emissao.py:1247`); `responder_automaticamente`, também em par (`app/chamados/services/disputa_emissao.py:1393`); reabertura limpa ambos (`app/chamados/services/disputa_emissao.py:1358`). | C6 (`app/chamados/juizes.py:1047`), respostas sem veredito (`app/chamados/models.py:1767`), I20 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101`), materializador (`app/chamados/services/materializacao.py:763`). |
| `respondida_em` | Os escritores acima; cura pela trilha (`app/chamados/services/disputa_emissao.py:1408`); porta `carimbar`, quando há texto e `resposta` solicitada (`app/chamados/models.py:2556`); ressuscitação limpa por porta de lote, selecionando texto vazio (`app/chamados/models.py:1423`, `app/chamados/models.py:1488`). | C6 (`app/chamados/juizes.py:1047`), `.estado` (`app/chamados/models.py:2477`), `.respondida` (`app/chamados/models.py:2783`), respostas sem veredito (`app/chamados/models.py:1769`), I20 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1102`). |
| `materializada_em` | Atribuição central em `_escrever_carimbo` (`app/chamados/models.py:2527`), persistida por `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote` (`app/chamados/models.py:2533`, `app/chamados/models.py:2588`, `app/chamados/models.py:2609`, `app/chamados/models.py:2619`). | C6 (`app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1077`, `app/chamados/juizes.py:1107`), seleção do fecho (`app/chamados/services/disputa_emissao.py:1620`), respostas sem veredito (`app/chamados/models.py:1768`), I9 (`app/chamados/management/commands/auditar_invariantes_chamados.py:953`). |
| `via_resolucao` | As portas do carimbo acima; exceção explícita que apenas troca a via em `encerrar_por_fato_no_espelho` (`app/chamados/models.py:2629`). | C6 (`app/chamados/juizes.py:1042`, `app/chamados/juizes.py:1071`), PMF (`app/chamados/juizes.py:624`), I9 (`app/chamados/management/commands/auditar_invariantes_chamados.py:954`). |
| `CelulaDia.veredito`, `veredito_via` | `CelulaDia.lavrar_veredito` delega (`app/escala/models.py:1787`) à porta que escreve ambos e `veredito_em`, status, protestos e evento (`app/ponto/portas/celula.py:709`, `app/ponto/portas/celula.py:770`). | C6 (`app/chamados/juizes.py:1045`, `app/chamados/juizes.py:1081`), retenção da folha (`app/folha/export.py:1215`), vigia pelo universo de `fora_da_folha` (`app/chamados/services/vigia_fio.py:545`). |
| `CelulaDia.ata` | Mesma porta de lavra (`app/ponto/portas/celula.py:726`, `app/ponto/portas/celula.py:768`); chave `precedencia` pertence a `carimbar_precedencia` (`app/ponto/portas/celula.py:811`). | Luz lida pelo C6 (`app/chamados/juizes.py:1008`); tratamento de marcos pendentes pelo C6 (`app/chamados/juizes.py:1094`). |
| `DisputaSupervisao.fechada_em` | `fechar` (`app/chamados/models.py:1845`), `reabrir_admin` (`app/chamados/models.py:2047`), `reabrir_sistema` (`app/chamados/models.py:2071`). | Seleção e fechamento do serviço (`app/chamados/services/disputa_emissao.py:1608`, `app/chamados/services/disputa_emissao.py:1658`), badge (`app/chamados/models.py:1721`), signal (`app/chamados/signals.py:28`), API (`app/api/views.py:1008`). |
| `ChamadoColaborador.status_local` | Transições por `_gravar_estado` (`app/chamados/models.py:195`, `app/chamados/models.py:222`, `app/chamados/models.py:279`); nascimento pelas portas `abrir`/`criar` (`app/chamados/models.py:897`, `app/chamados/models.py:1046`). | Sem-credor (`app/chamados/services/disputa_emissao.py:1613`, `app/chamados/services/disputa_emissao.py:1616`), C6/PMF (`app/chamados/juizes.py:721`), vigia (`app/chamados/services/vigia_fio.py:569`, `app/chamados/services/vigia_fio.py:578`), signal do fecho (`app/chamados/signals.py:81`). |

A fixture introduz deliberadamente texto sem instante por `QuerySet.update(resposta_colab='07:55')`, em vez de usar os escritores do par. Isso não demonstra um caminho atual de produção gerando esse estado; o código do I20 documenta acervo histórico com `respondida_em` nulo, que também não deve ser convertido em fato observado nesta revisão (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:143`; `app/chamados/services/disputa_emissao.py:1247`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1089`).

Achado mais forte: o caso regularizado injeta `veredito='ok'`, `veredito_via='cartorio'` diretamente no banco; a porta real rejeita esse veredito, pois `ok` não pertence a `VEREDITOS_CARTORIO`. O veredito regular sem códigos é `concorde`. Logo, o estado exato da fixture não sai dessa porta de produção (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:208`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:152`; `app/ponto/portas/celula.py:658`; `app/escala/models.py:1719`; `app/escala/models.py:1596`).

`_fechar` chama produção e, depois, sobrescreve a célula novamente. A testemunha mede esse estado imposto após o serviço, sem exigir que sobrevivam o veredito, a ata ou os efeitos da relavra real; o update omite `veredito_em`, status, protestos e emissão do evento da porta (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:155`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:152`; `app/ponto/portas/celula.py:770`).

O teste também cria o chamado já resolvido e a disputa aberta por ORM, sem provar a travessia pelas portas de estado; tampouco possui assert direto sobre `status_local` ou `fechada_em`, apenas o retorno contado do fecho no teste do ramo (`app/chamados/tests/test_o261e_f2_ramo_e_i20.py:135`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:139`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:185`).

Não dizer que “os selos da casa reprovam este teste”: os selos AST do par resposta/carimbo, da porta de célula e do carimbo excluem diretórios `tests`. As mesmas escritas em um arquivo de produção seriam alcançadas por esses contratos (`app/chamados/tests/test_contract_par_resposta.py:54`; `app/chamados/tests/test_contract_par_resposta.py:81`; `app/ponto/tests/test_selo_veredito_so_pela_porta.py:173`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:208`).

## 01/11 16:53 · sessao ee8a3724479c · O261-E-F2 · verificador · origem=Code

1. **Sim:** o resultado agregado coincide exatamente com `VIA_DO_CHAMADOR`: admin → `VIA_ADMIN_DECIDIU`; duplicata → `VIA_DUPLICATA`; paridade → `VIA_PARIDADE_RECUSADA`; recusa → `VIA_FATO_OCUPADO`. Os quatro sítios são [models.py:2498](app/chamados/models.py:2498), [fio.py:891](app/chamados/services/fio.py:891), [materializacao.py:858](app/chamados/services/materializacao.py:858) e [materializacao.py:1033](app/chamados/services/materializacao.py:1033); a comparação está em [test_o261e_f2_porta_carimbo.py:482](app/chamados/tests/test_o261e_f2_porta_carimbo.py:482).

   Nenhuma ocorrência nesses três arquivos acrescenta `<indireta>`. O `def` não é `ast.Name`; comentários não entram na AST; docstrings e strings maiores não satisfazem a igualdade exata. Uma string/docstring contendo **somente** `carimbar_identidade` contaria como `<indireta>`. O atributo da chamada direta fica excluído por `diretas`. [test_o261e_f2_porta_carimbo.py:414](app/chamados/tests/test_o261e_f2_porta_carimbo.py:414), [test_o261e_f2_porta_carimbo.py:422](app/chamados/tests/test_o261e_f2_porta_carimbo.py:422).

2. **Existe mutante de duas linhas:** substituir [materializacao.py:858](app/chamados/services/materializacao.py:858) por:

   ```python
   p.VIA_PARIDADE_RECUSADA = PerguntaDisputa.VIA_ADMIN_DECIDIU
   p.carimbar_identidade(p.VIA_PARIDADE_RECUSADA, request_user)
   ```

   O coletor continua enxergando `VIA_PARIDADE_RECUSADA`, pois verifica apenas `via.attr`; não resolve seu receptor nem seu valor. A referência à porta permanece na mesma função declarada, mantendo também o teste dos chamadores verde. [test_o261e_f2_porta_carimbo.py:420](app/chamados/tests/test_o261e_f2_porta_carimbo.py:420), [test_o261e_f2_porta_carimbo.py:443](app/chamados/tests/test_o261e_f2_porta_carimbo.py:443), [test_o261e_f2_porta_carimbo.py:473](app/chamados/tests/test_o261e_f2_porta_carimbo.py:473).

   Entretanto, o argumento passa a valer `'admin_decidiu'`, uma identidade aceita pela porta, que o encaminha à escrita e ao `save`. [models.py:2237](app/chamados/models.py:2237), [relavra_via.py:166](app/chamados/services/relavra_via.py:166), [models.py:2597](app/chamados/models.py:2597), [models.py:2602](app/chamados/models.py:2602), [models.py:2607](app/chamados/models.py:2607).

   Reexecutei os corpos dos dois testes em memória, usando `unittest.TestCase` e sobrepondo a leitura do arquivo pelo mutante: ambos passaram. Runner Django completo: [não verificado]. [test_o261e_f2_porta_carimbo.py:473](app/chamados/tests/test_o261e_f2_porta_carimbo.py:473), [test_o261e_f2_porta_carimbo.py:482](app/chamados/tests/test_o261e_f2_porta_carimbo.py:482).

3. **O teste novo lê somente fonte sintética:** constrói `src`, passa ao coletor AST e compara conjuntos; não consulta campos de banco. O teste da árvore também lê arquivos de fonte. [test_o261e_f2_porta_carimbo.py:499](app/chamados/tests/test_o261e_f2_porta_carimbo.py:499), [test_o261e_f2_porta_carimbo.py:485](app/chamados/tests/test_o261e_f2_porta_carimbo.py:485).

   No diff fornecido, nenhum leitor de produção é alterado: as mudanças ficam no coletor e no teste. O leitor C6 existente consome `via_resolucao` e `materializada_em`; a porta existente escreve esses campos com o autor. [test_o261e_f2_porta_carimbo.py:406](app/chamados/tests/test_o261e_f2_porta_carimbo.py:406), [test_o261e_f2_porta_carimbo.py:499](app/chamados/tests/test_o261e_f2_porta_carimbo.py:499), [juizes.py:1071](app/chamados/juizes.py:1071), [models.py:2527](app/chamados/models.py:2527). Efeito em registros reais: [não verificado].

## 01/11 16:49 · sessao 4b97a069739e · O261-E-F2 · verificador · origem=Code

1. **QUEM CHAMA**

As quatro chamadas encontradas fora de `tests/` e `migrations/` coincidem com `VIA_DO_CHAMADOR`, inclusive a via atribuída a cada função. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:398`)

| Local | Chamador | Primeiro argumento literal |
|---|---|---|
| `app/chamados/models.py:2498` | `PerguntaDisputa.fechar_por_admin` | `self.VIA_ADMIN_DECIDIU` |
| `app/chamados/services/fio.py:891` | `retratar_duplicata` | `PerguntaDisputa.VIA_DUPLICATA` |
| `app/chamados/services/materializacao.py:858` | `_materializar_resposta_hora` | `PerguntaDisputa.VIA_PARIDADE_RECUSADA` |
| `app/chamados/services/materializacao.py:1033` | `_carimbar_recusa` | `PerguntaDisputa.VIA_FATO_OCUPADO` |

2. **MORDE: existe mutante que passa nos dois testes.** [nao verificado]

Substitua a chamada de `app/chamados/services/materializacao.py:858` pelas três linhas abaixo, mantendo o retorno seguinte:

```python
p.carimbar_identidade(PerguntaDisputa.VIA_PARIDADE_RECUSADA, request_user)
gravar = p.carimbar_identidade
gravar(via=PerguntaDisputa.VIA_ADMIN_DECIDIU, autor=request_user)
```

A primeira chamada preserva a via esperada pelo teste. A segunda chamada usa `ast.Name`, enquanto o varredor de vias exige `ast.Attribute`; portanto, a gravação de `admin_decidiu` fica invisível para ele. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:415`)

O varredor de referências reconhece a atribuição do alias, mas ela permanece no mesmo chamador autorizado. Como o resultado vira conjunto de `(arquivo, qualname)`, essa referência adicional não altera a comparação. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:457`)

Em execução, ambas as vias são aceitas e a última chamada sobrescreve a primeira, persistindo `admin_decidiu`. (`app/chamados/models.py:2597`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2607`)

Reexecutei os dois métodos de teste em memória, interceptando somente a leitura da fonte mutante: original e mutante passaram nas duas verificações. Usei `unittest.TestCase` no lugar de `SimpleTestCase`; execução integral pelo Django: **[não verificado]**. Métodos exercitados: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:463` e `app/chamados/tests/test_o261e_f2_porta_carimbo.py:472`.

Os casos pedidos ficam assim:

| Forma | Resultado |
|---|---|
| Chamada direta com via em variável local | Vira `<dinamica>`; reprova o teste de vias. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:415`) |
| Chamada direta com `via=outra_variavel` | Também vira `<dinamica>`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:414`) |
| Keyword diferente de `via` | Sem argumento posicional, vira `<dinamica>`; a assinatura também não aceita esse outro nome. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:414`, `app/chamados/models.py:2588`) |
| Duas chamadas diretas com vias distintas | Ambas entram no conjunto; reprova. Duas com a mesma via não são contadas separadamente. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:416`) |
| Chamada direta em função aninhada nomeada | Ganha outro `qualname`; reprova os mapas. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:411`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:430`) |

3. **QUEM ESCREVE E QUEM LÊ**

A cadeia de escrita é `carimbar_identidade` → `_escrever_carimbo` → atribuição de `via_resolucao`; quem persiste é `carimbar_identidade`, no mesmo `save` dos campos de materialização. A guarda aceita qualquer uma das quatro identidades, sem vincular a via ao chamador. (`app/chamados/models.py:2597`, `app/chamados/models.py:2600`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2607`)

Estes leitores **distinguem as quatro vias**:

| Leitor | Decisão que pode mudar |
|---|---|
| `estado_aplicacao_pergunta` | Para pergunta hora, com resposta e materializada: paridade → `recusada_paridade`; outras identidades → `aplicada`. É efeito direto do mutante acima. (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:423`, `app/chamados/services/materializacao.py:427`) |
| `fila_flip_recusas` | Só seleciona paridade; trocar para admin retira a pergunta dessa fonte de candidatos, satisfeitas as demais condições. (`app/ponto/services/flip_auto.py:128`, `app/ponto/services/flip_auto.py:132`, `app/ponto/services/flip_auto.py:140`) |
| Cartório | O ramo que tenta reconciliar exige que todas as resolvedoras sejam duplicatas, após as guardas anteriores. (`app/ponto/services/cartorio.py:1424`, `app/ponto/services/cartorio.py:1429`) |
| Fábrica da célula | Duplicata é excluída do conjunto “já perguntado”; as outras identidades permanecem. (`app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:108`, `app/chamados/services/fabrica_da_celula.py:111`) |
| Emissão por competência | Também exclui duplicata do conjunto “já perguntado”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:127`) |
| `fechar_por_admin` | A guarda idempotente só reconhece `admin_decidiu` materializada; outra identidade é sobrescrita. (`app/chamados/models.py:2489`, `app/chamados/models.py:2498`) |
| `retratar_duplicata` e comando de deduplicação | Pulam a pergunta já marcada como duplicata. (`app/chamados/services/fio.py:882`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`) |
| Apresentação e classificação | Mudam a frase do cartão, o motivo do explicador e o balde `via:<valor>`. (`app/templates/chamados/partials/_card_atendimento.html:186`, `app/chamados/services/explicador.py:95`, `app/chamados/services/passivo_validado.py:140`) |

O C6, `_pergunta_materializada_de_fato`, `q_vivas` e `q_carimbo_desfazivel` agrupam as quatro identidades; trocar somente entre elas não muda esses predicados. A guarda da resposta do colaborador precede o encerramento por identidade no C6. (`app/chamados/juizes.py:632`, `app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1066`, `app/chamados/models.py:2708`, `app/chamados/services/relavra_via.py:179`)

Há também leituras para memo, restauração e trilha, sem decisão específica entre as quatro: `app/chamados/juizes.py:790`, `app/chamados/models.py:2552`, `app/chamados/models.py:1838`, `app/chamados/reconciliador.py:1089`, `app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:1023`, `app/chamados/services/passivo_validado.py:515`.

A re-lavra restringe-se às vias derivadas; o comando de encerramento por fato restringe-se a `SEM_HISTORIA`; os leitores de fantasma/sem lastro/tranca guardam por outras vias. Nenhum desses filtros distingue as quatro identidades. (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:116`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `app/chamados/models.py:2645`, `app/chamados/models.py:2657`, `app/chamados/models.py:2674`, `app/chamados/models.py:2681`, `app/chamados/services/competencia_trancada.py:244`)

4. **TRIAGEM V4**

Não encontrei descarte demonstravelmente improcedente **no alcance específico declarado**. Os MORDE realmente usam fontes sintéticas, e o teste da árvore realmente compara os chamadores vivos; isso não garante cobertura de toda forma de chamada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:463`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:509`)

O mutante encontrado é alias local de método, não importação renomeada. Assim, aquele descarte não o resolve: a referência fica autorizada pelo conjunto de chamadores, enquanto a chamada indireta escapa ao mapa de vias. A correção v5 barra a troca direta citada na triagem, mas deixa essa lacuna. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:457`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:479`)

## 01/11 16:48 · sessao 8e6feb3eb2a7 · - · - · origem=Ronald (tmux)

Mutante de duas linhas, inserido após `app/chamados/services/materializacao.py:858` e antes do retorno em `:859`:

```python
carimbar = p.carimbar_identidade
carimbar(PerguntaDisputa.VIA_ADMIN_DECIDIU, request_user)
```

A chamada original de paridade permanece. O varredor de vias exige `ast.Call.func` do tipo `ast.Attribute`, portanto não vê a segunda chamada por `ast.Name`; a referência ao método continua na mesma função já declarada e o outro teste reduz as referências a um conjunto de `(arquivo, qualname)`. Assim, ambos passam, enquanto a segunda gravação substitui a via por `admin_decidiu`. Fontes: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `:416`, `:433`, `:457`, `:472`; `app/chamados/models.py:2597`, `:2602`, `:2607`.

Executei em memória as duas funções de teste reais, mapas e varredores extraídos por AST: original e mutante deram dois testes, zero falhas, zero erros. Usei `unittest.TestCase` no lugar de `SimpleTestCase`, interceptando somente a leitura da fonte mutada; execução integral pelo Django permanece [não verificado]. [nao verificado]

Quanto aos descartes da triagem, não identifiquei um factualmente improcedente **dentro do alcance específico declarado**:

- Os seis MORDE realmente verificam fontes sintéticas; o teste da árvore realmente compara chamadores reais. A detecção de M1 não implica que detecte o mutante acima. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:463`, `:491`, `:509`, `:514`, `:533`.
- A limitação para nome montado decorre de comparar atributo/nome/string exatos. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`.
- A exclusão de `carimbar` do selo de chamadas internas é coerente com consultar o C6; as voltas escrevem dentro das portas autorizadas. `app/chamados/models.py:2565`, `:2609`, `:2619`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`, `:51`.

É improcedente apenas extrapolar esses descartes para uma garantia geral de que qualquer chamada com via indevida seria barrada: o mutante acima refuta essa garantia. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:413`, `:457`.

## 01/11 16:47 · sessao b972a23c050a · - · - · origem=Ronald (tmux)

A escrita é `PerguntaDisputa.carimbar_identidade` → `_escrever_carimbo` → `self.via_resolucao = via`; a porta persiste os três campos em `save(update_fields=campos)`. A lista aceita as quatro vias de identidade. (`app/chamados/models.py:2597`, `app/chamados/models.py:2600`, `app/chamados/models.py:2602`, `app/chamados/models.py:2531`, `app/chamados/models.py:2603`, `app/chamados/models.py:2607`, `app/chamados/services/relavra_via.py:166`)

Leitores que **mudam comportamento entre as quatro vias**:

| Leitor | Diferença |
|---|---|
| `estado_aplicacao_pergunta` | Para pergunta tipo hora com resposta e materializada, `paridade_recusada` retorna `recusada_paridade`; as demais retornam `aplicada`. (`app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:423`, `app/chamados/services/materializacao.py:427`) |
| `fila_flip_recusas` | Só seleciona `paridade_recusada`, com validação recente; extrai a batida vizinha do motivo e a inclui na fila de flip, incorporada por `fila_flip`. Trocar por `admin_decidiu` remove essa fonte de candidatura, preservadas as demais condições. (`app/ponto/services/flip_auto.py:128`, `app/ponto/services/flip_auto.py:132`, `app/ponto/services/flip_auto.py:140`, `app/ponto/services/flip_auto.py:148`) |
| Cartório | Quando todas as resolvedoras são `duplicata`, entra no ramo que tenta reconciliar o chamado. Outra via não entra nesse ramo. (`app/ponto/services/cartorio.py:1424`, `app/ponto/services/cartorio.py:1429`, `app/ponto/services/cartorio.py:1430`) |
| Fábrica da célula | Exclui `duplicata` do universo que alimenta `ja`; outra identidade permanece e pode impedir nova pergunta daquele motivo. (`app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:104`, `app/chamados/services/fabrica_da_celula.py:108`, `app/chamados/services/fabrica_da_celula.py:111`) |
| Emissão por competência | Também exclui `duplicata` do conjunto já perguntado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:127`, `app/chamados/management/commands/disparar_perguntas_competencia.py:129`) |
| `fechar_por_admin` | A guarda idempotente só retorna `False` para `admin_decidiu` materializada; outra identidade é sobrescrita. (`app/chamados/models.py:2489`, `app/chamados/models.py:2497`, `app/chamados/models.py:2498`) |
| `retratar_duplicata` | Só retorna imediatamente para `duplicata`; outra identidade é substituída pela via de duplicata. (`app/chamados/services/fio.py:882`, `app/chamados/services/fio.py:883`, `app/chamados/services/fio.py:891`) |
| Comando de deduplicação | Remove as já `duplicata` da lista de excedentes a processar. (`app/chamados/management/commands/dedup_perguntas_fato.py:74`, `app/chamados/management/commands/dedup_perguntas_fato.py:76`) |

Leitores que alteram **classificação/apresentação**: `_balde_da_recusa` devolve o valor concreto em `via:<valor>`; o explicador mostra o display da via; o cartão distingue admin, duplicata e as duas recusas. (`app/chamados/services/passivo_validado.py:140`, `app/chamados/services/passivo_validado.py:142`, `app/chamados/services/explicador.py:95`, `app/templates/chamados/partials/_card_atendimento.html:186`)

O C6 **não distingue as quatro** no degrau por identidade; `_pergunta_materializada_de_fato` também as absolve igualmente, `q_vivas` as agrupa e `q_carimbo_desfazivel` exclui todas. A resposta do colaborador é julgada antes do degrau de identidade; logo, não afirmar “qualquer identidade sempre mata a pergunta”. (`app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1055`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1071`, `app/chamados/juizes.py:632`, `app/chamados/juizes.py:635`, `app/chamados/models.py:2708`, `app/chamados/models.py:2713`, `app/chamados/services/relavra_via.py:166`, `app/chamados/services/relavra_via.py:179`)

A re-lavra só aceita `declaracao`/`completude_turno`, portanto não troca nenhuma das quatro identidades; `encerrar_perguntas_com_fato` também não as inclui em `SEM_HISTORIA`. (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:123`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:42`)

Outros usos são memória/restauração, memo, trilha ou agrupamento por via: `app/chamados/models.py:2552`, `app/chamados/models.py:2613`, `app/chamados/models.py:2627`, `app/chamados/juizes.py:790`, `app/chamados/models.py:1838`, `app/chamados/reconciliador.py:1089`, `app/chamados/reconciliador.py:1100`, `app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/disputa_emissao.py:1643`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:1023`, `app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:515`, `app/chamados/management/commands/auditar_invariantes_chamados.py:194`, `app/chamados/management/commands/auditar_invariantes_chamados.py:205`, `app/chamados/management/commands/auditar_invariantes_chamados.py:432`, `app/chamados/management/commands/auditar_invariantes_chamados.py:541`.

Há ainda escritores/leitores de outras vias: `encerrar_por_fato_no_espelho` escreve `fato_materializado`; as portas de órfã/fantasma/sem lastro/tranca guardam por essas vias, e seus filtros não diferenciam as quatro identidades. (`app/chamados/models.py:2634`, `app/chamados/models.py:2636`, `app/chamados/models.py:2645`, `app/chamados/models.py:2657`, `app/chamados/models.py:2674`, `app/chamados/models.py:2681`, `app/chamados/services/competencia_trancada.py:244`, `app/chamados/services/materializacao.py:572`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`)

## 01/11 16:44 · sessao 2b54b3c2d6c8 · O261-E-F2 · verificador · origem=Code

O patch de reabertura **não entrega o resultado prometido pelo fixture**: a primeira guarda devolve `veto_do_dia`, por ausência de vínculo de escala. Além disso, os selos novos aceitam mutantes concretos que burlam a intenção da porta. (`app/chamados/models.py:1285`; `app/ponto/services/furos_vetados.py:40`; `app/chamados/tests/test_o261_resposta_vazia.py:36`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`)

As referências abaixo são relativas à raiz da cópia. Para os dois anexos sem arquivo na árvore, numerei o conteúdo fornecido desde a primeira linha: `diff --git` no patch e `# O261-E-F2` na sonda. Execução Django, resultados da sonda e estado de produção: **[nao verificado]**.

1. Quem chama as seis portas.

Estes são os sítios executáveis encontrados para os métodos de `PerguntaDisputa`; chamadas repetidas dentro da mesma função aparecem separadamente pelas linhas. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:394`)

| Método | Chamador e sítio |
|---|---|
| `_escrever_carimbo` | `PerguntaDisputa.carimbar` — `app/chamados/models.py:2555` |
| | `PerguntaDisputa._desfazer_em_memoria` — `app/chamados/models.py:2582` |
| | `PerguntaDisputa.carimbar_identidade` — `app/chamados/models.py:2600`, `app/chamados/models.py:2602` |
| | `PerguntaDisputa.descarimbar` — `app/chamados/models.py:2611` |
| `_desfazer_em_memoria` | `PerguntaDisputa.carimbar`, tanto na exceção quanto na recusa — `app/chamados/models.py:2567`, `app/chamados/models.py:2570` |
| `carimbar_identidade` | `PerguntaDisputa.fechar_por_admin` — `app/chamados/models.py:2498` |
| | `retratar_duplicata` — `app/chamados/services/fio.py:891` |
| | `_materializar_resposta_hora` — `app/chamados/services/materializacao.py:858` |
| | `_carimbar_recusa` — `app/chamados/services/materializacao.py:1033` |
| `carimbar` | `PerguntaDisputa.encerrar_fantasma` — `app/chamados/models.py:2648` |
| | `lavrar_sem_turno_aberto` — `app/chamados/reconciliador.py:405` |
| | `retratar_perguntas_absolvidas_por_celula` — `app/chamados/reconciliador.py:618` |
| | `_materializar_ausencia_de_resposta` — `app/chamados/services/materializacao.py:523` |
| | `_materializar_uma` — `app/chamados/services/materializacao.py:1166` |
| | `materializar_pendentes_aprovadas` — `app/chamados/services/materializacao.py:1313` |
| | `resolver_perguntas_por_chamado_resolvido`, dois ramos — `app/chamados/services/disputa_emissao.py:1566`, `app/chamados/services/disputa_emissao.py:1638` |
| `descarimbar` | `ChamadoColaborador._universo_da_ressuscitacao`, somente na cópia em memória — `app/chamados/models.py:1439` |
| | `PerguntaDisputa.reabrir_fantasma` — `app/chamados/models.py:2659` |
| | `PerguntaDisputa.reabrir_por_resposta` — `app/chamados/models.py:2676` |
| | `PerguntaDisputa.reabrir_por_competencia` — `app/chamados/models.py:2683` |
| | `desfazer_carimbo_sem_lastro` — `app/chamados/services/relavra_via.py:231` |
| `descarimbar_em_lote` | `ChamadoColaborador.ressuscitar_perguntas` — `app/chamados/models.py:1488` |
| | `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1377` |

As referências descritivas às portas, sem chamada executável, estão nos comentários de declaração do modelo, na descrição do uso nos serviços, no registro de portas e no dossiê. Essas referências não acrescentam chamadores ao conjunto AST porque são comentários ou strings maiores que o nome procurado. (`app/chamados/models.py:2506`; `app/chamados/models.py:2512`; `app/chamados/models.py:2514`; `app/chamados/models.py:2516`; `app/chamados/models.py:2580`; `app/chamados/models.py:2620`; `app/chamados/services/disputa_emissao.py:1562`; `app/chamados/services/materializacao.py:857`; `app/core/portas.py:246`; `app/docs/DOSSIES.md:46`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:405`)

Há homônimos que não são essas portas: `api.credencial.carimbar`, a porta de célula importada pelo cartório e a variável local `carimbar` de competência trancada. (`app/api/views.py:23`; `app/api/views_core.py:23`; `app/ponto/services/cartorio.py:30`; `app/chamados/services/competencia_trancada.py:131`)

**Comparação com `CHAMADORES`:** os quatro chamadores de `_escrever_carimbo`, o único de `_desfazer_em_memoria` e os quatro de `carimbar_identidade` coincidem com os declarados. O dict **não fiscaliza** quem chama `carimbar`, `descarimbar` ou `descarimbar_em_lote`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:373`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:380`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:385`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:434`)

| Forma | O selo pega ou perde? |
|---|---|
| `p.metodo(...)`, `PerguntaDisputa.metodo(p,...)`, alias obtido por `g=p.metodo` | **Pega** o atributo; não precisa resolver seu receptor. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| Import local do modelo com alias, seguido de `PD.metodo(...)` | **Pega** pelo atributo; o import local não cria ponto cego nessa forma. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| Import do próprio nome, renomeado: `from x import _escrever_carimbo as g; g(...)` | **Perde o import como referência**: `ast.alias` não é uma das três formas examinadas. A existência de um export desse método na cópia não foi demonstrada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; **[nao verificado]**) |
| Função aninhada | **Pega**, incluindo a função interna no qualname; portanto cria outra chave na whitelist. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:407`) |
| Classe aninhada | **Pega**, incluindo classe e método internos no qualname. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`) |
| Lambda com referência explícita | **Pega**, mas não acrescenta um componente “lambda” ao qualname. Uma chamada extra dentro de função permitida pode conservar a mesma chave. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:400`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:427`) |
| `super().metodo`, `type(self).metodo`, `self.__class__.metodo` | **Pega**, pois todos apresentam `ast.Attribute.attr` igual ao nome. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |
| `getattr(p,'_escrever_carimbo')`, `methodcaller('_escrever_carimbo',...)` | **Pega** a constante exata. Também pega `nome='_escrever_carimbo'; getattr(p,nome)`, no literal. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:405`) |
| `getattr(p,'_escrever_'+'carimbo')`, `join`, formatação ou nome vindo de fora | **Perde quando não existe atributo, nome ou constante exata**: não avalia a expressão nem acompanha seu valor. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`) |

O selo também descarta linhas e multiplicidade: fiscaliza um **conjunto de `(arquivo, qualname)`**. Não distingue uma chamada legítima de outra chamada indevida na mesma função, nem examina seus argumentos. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:427`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:437`)

Seu universo é menor que “toda árvore fora tests/migrations”: exclui outros diretórios, todos os arquivos não Python, `test_*.py` e fontes que falhem no parse/decodificação. Ausência universal de chamadores dinâmicos fora desse alcance: **[nao verificado]**. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:417`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:420`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:425`)

2. Cada selo morde?

Para testar **cada um dos sete testes novos**, apliquei virtualmente o mesmo mutante de produção M1, de três linhas, acrescentado a `app/chamados/reconciliador.py`; as verificações foram executadas apenas sobre AST e fontes em memória. O mutante usa a escrita interna e persiste sem consultar C6. (`app/chamados/models.py:2527`; `app/chamados/models.py:2565`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`)

```diff
+def carimbar_por_fora(p, u, via, agora):
+    p._escrever_carimbo(agora, u.pk, via)
+    p.save(update_fields=['materializada_em', 'materializada_por', 'via_resolucao'])
```

| Teste de `SeloQuemChamaAPorta` | Com M1 na produção | [nao verificado]
|---|---|
| `test_os_chamadores_de_cada_porta_interna_sao_exatamente_os_declarados` | **Vermelho**: encontra a nova chave fora de `CHAMADORES`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:437`) |
| `test_MORDE_chamada_por_atributo_por_classe_por_alias_getattr_e_methodcaller` | **Verde**: verifica somente sua string `src`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:445`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:452`) |
| `test_o_def_e_a_prosa_nao_sao_referencia` | **Verde**: verifica somente sua string `src`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:455`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:460`) |
| `test_MORDE_service_que_escreve_o_carimbo_sem_o_c6` | **Verde**: demonstra que a amostra seria classificada como proibida; não lê o service modificado. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:469`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:472`) |
| `test_MORDE_metodo_novo_no_modelo_tambem_fica_fora` | **Verde**: examina a classe escrita na amostra. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:477`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:481`) |
| `test_MORDE_identidade_chamada_de_modulo_que_nao_julga_identidade` | **Verde**: examina a amostra `fechar_em_massa`. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:488`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`) |
| `test_MORDE_o_chamador_declarado_em_outro_arquivo_nao_vale` | **Verde**: examina o homônimo artificial. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:496`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:497`) |

Esses seis testes são úteis contra regressões do **varredor**; quem fiscaliza uma mutação efetiva da produção é o primeiro teste da tabela. Não são seis provas independentes de rejeição do código mutado. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:462`)

Para `test_MORDE_a_arvore_real_ainda_atribui_o_update_pelo_gerente_reverso`, o mutante concreto em `app/chamados/models.py:1488` é:

```diff
-        n = PerguntaDisputa.descarimbar_em_lote(alvo, validada_em=None, validada_por=None, respondida_em=None)
+        n = alvo.update(materializada_em=None, validada_em=None, validada_por=None, respondida_em=None)
```

**Fica vermelho**, primeiro porque desaparece a âncora literal exigida pelo teste. A segunda metade do teste, na fonte original, também demonstra que o varredor atribui o `alvo.update()` artificial a `PerguntaDisputa` pelo mapa real de relações. Portanto há mordida, mas a rejeição dessa alteração concreta ocorre antes, pela âncora. (`app/core/tests/test_censo_escritas.py:234`; `app/core/tests/test_censo_escritas.py:235`; `app/core/tests/test_censo_escritas.py:240`; `app/core/tests/test_censo_escritas.py:244`)

**Sim, há mutante plausível que passa nos oito testes pedidos, sem montagem de nome.** Em `app/chamados/services/materializacao.py:858`:

```diff
-            p.carimbar_identidade(PerguntaDisputa.VIA_PARIDADE_RECUSADA, request_user)
+            p.carimbar_identidade(PerguntaDisputa.VIA_ADMIN_DECIDIU, request_user)
```

Os oito permaneceram verdes no ensaio em memória: arquivo, função e referência continuam iguais. O juiz de paridade passa a registrar uma decisão de admin; `carimbar_identidade` aceita essa via porque ela pertence à lista de identidade. O selo não verifica essa correspondência entre chamador e via. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:388`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; `app/chamados/models.py:2597`; `app/chamados/juizes.py:1066`)

Há ainda o bypass de M1 trocando sua segunda linha por:

```diff
+    getattr(p, '_escrever_' + 'carimbo')(agora, u.pk, via)
```

Com essa forma, os sete testes de chamadores permanecem verdes; o teste novo do censo também permanece verde porque sua fonte-alvo não mudou. O varredor de escritores não reconhece o `save(update_fields=...)` como atribuição dos valores do carimbo. Resultado da suíte Django completa para esses mutantes: **[nao verificado]**. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:403`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:183`; `app/core/tests/test_censo_escritas.py:233`)

3. Escritores e leitores dos campos.

| Campo | Escritores persistentes fora das seis portas |
|---|---|
| `materializada_em` | Nenhuma atribuição explícita adicional encontrada para `PerguntaDisputa`: atribuição em `_escrever_carimbo` e limpeza na porta de lote. (`app/chamados/models.py:2529`; `app/chamados/models.py:2626`) |
| `materializada_por` | Mesma concentração, usando `_id` na atribuição e FK na limpeza. Há também o efeito de exclusão de usuário, abaixo. (`app/chamados/models.py:2530`; `app/chamados/models.py:2627`) |
| `via_resolucao` | `encerrar_por_fato_no_espelho`, atribuição e save; `relavra_via.relavrar`, nova via e relavra no mesmo save. (`app/chamados/models.py:2636`; `app/chamados/models.py:2637`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/relavra_via.py:125`) |
| `respondida_em` | Resposta do colaborador; limpeza para correção; resposta automática; recuperação pela trilha. (`app/chamados/services/disputa_emissao.py:1248`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`; `app/chamados/services/disputa_emissao.py:1394`; `app/chamados/services/disputa_emissao.py:1395`; `app/chamados/services/disputa_emissao.py:1410`; `app/chamados/services/disputa_emissao.py:1411`) |
| `validada_em` | `_carimbar_validacao`; `devolver_validacao_recusada`; nova resposta que desfaz validação; limpeza para correção. (`app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:133`; `app/chamados/services/validacao.py:188`; `app/chamados/services/validacao.py:190`; `app/chamados/services/disputa_emissao.py:1260`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`) |
| `validada_por` | Os mesmos quatro atos: atribuição do autor na validação e `None` nos desfazimentos. (`app/chamados/services/validacao.py:132`; `app/chamados/services/validacao.py:189`; `app/chamados/services/disputa_emissao.py:1261`; `app/chamados/services/disputa_emissao.py:1358`) |
| `resposta_colab` | Resposta do colaborador; limpeza para `''` na correção; resposta automática. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/disputa_emissao.py:1266`; `app/chamados/services/disputa_emissao.py:1358`; `app/chamados/services/disputa_emissao.py:1378`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/services/disputa_emissao.py:1395`) |

`ressuscitar_perguntas` determina a limpeza de `respondida_em`, `validada_em` e `validada_por`, mas entrega a persistência à porta de lote. As atribuições desses instantes em `_universo_da_ressuscitacao` atingem somente uma cópia em memória, sem save. (`app/chamados/models.py:1488`; `app/chamados/models.py:1427`; `app/chamados/models.py:1439`; `app/chamados/models.py:1440`; `app/chamados/models.py:1441`)

Há **dois escritores adicionais do terceiro campo do carimbo**, `via_resolucao`. Ambos são exceções explícitas de `PORTA_VIA`; portanto sua existência não viola a whitelist atual, embora impeça dizer literalmente que somente as seis portas escrevem a tupla inteira. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:238`)

`materializada_por` e `validada_por` também podem ser zerados pelo mecanismo `on_delete=SET_NULL`. Esse efeito não passa pelas seis portas, e o selo declara expressamente que não o alcança. (`app/chamados/models.py:2742`; `app/chamados/models.py:2762`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`)

Existe ainda o canal genérico de nascimento `PerguntaDisputa.abrir(**campos)`, que repassa os argumentos a `objects.create`. Não foi localizado chamador atual fornecendo por ele esses campos; impossibilidade de fornecimento dinâmico: **[nao verificado]**. (`app/chamados/models.py:2374`; `app/chamados/models.py:2452`)

O materializador de regularização escreve campos homônimos de **`RegularizacaoExterna`**, não de `PerguntaDisputa`; essa distinção está registrada na allowlist. (`app/chamados/services/regularizacao_ext.py:198`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`)

As leituras que determinam o comportamento discutido são:

| Campo | Quem lê e para quê |
|---|---|
| `materializada_em` | C6: tranca/identidade, marco e lastro; `.estado`: materialização; materializador: seleção das ainda não materializadas; raiox: consumo; procedência: instante do ato. (`app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/juizes.py:1077`; `app/chamados/juizes.py:1107`; `app/chamados/models.py:2477`; `app/chamados/services/materializacao.py:1261`; `app/chamados/services/raiox.py:133`; `app/ponto/services/procedencia_batida.py:74`) |
| `materializada_por` | Porta: fotografia e restauração; materializador: fotografia anterior; procedência: autoria. (`app/chamados/models.py:2552`; `app/chamados/models.py:2582`; `app/chamados/services/materializacao.py:618`; `app/ponto/services/procedencia_batida.py:71`) |
| `via_resolucao` | C6: tranca/identidade; relavra: elegibilidade e mudança; materializador: paridade; apresentação: explicação ao colaborador. (`app/chamados/juizes.py:1042`; `app/chamados/juizes.py:1071`; `app/chamados/services/relavra_via.py:116`; `app/chamados/services/relavra_via.py:119`; `app/chamados/services/materializacao.py:423`; `app/templates/chamados/partials/_card_atendimento.html:186`) |
| `respondida_em` | C6: resposta exige data **e** texto; `.estado` e `.respondida`: rótulo; validação: pré-condição; raiox: partição; HX: respostas aguardando consumo; template: apresentação. (`app/chamados/juizes.py:1047`; `app/chamados/models.py:2479`; `app/chamados/models.py:2783`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:136`; `app/chamados/juizes.py:1634`; `app/templates/chamados/partials/modal_fio.html:212`) |
| `validada_em` | Primeiro degrau do C6; `.estado`; validação idempotente; seleção do materializador; seleção do fecho sem credor; resposta sem veredito do C7. (`app/chamados/juizes.py:1033`; `app/chamados/models.py:2475`; `app/chamados/services/validacao.py:366`; `app/chamados/services/materializacao.py:1260`; `app/chamados/services/disputa_emissao.py:1620`; `app/chamados/models.py:1768`) |
| `validada_por` | Fotografias de validação/reversão, atribuição em painel/verbos e procedência. (`app/chamados/services/validacao.py:139`; `app/chamados/services/materializacao.py:616`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`; `app/ponto/services/procedencia_batida.py:72`) |
| `resposta_colab` | C6: resposta e concordância com a luz; C7: resposta sem veredito mesmo sem timestamp; materializador: hora e guarda de texto; validação; apresentação; auditoria de meio-estado. (`app/chamados/juizes.py:1047`; `app/chamados/juizes.py:1010`; `app/chamados/models.py:1770`; `app/chamados/services/materializacao.py:264`; `app/chamados/services/materializacao.py:763`; `app/chamados/services/validacao.py:806`; `app/templates/chamados/partials/modal_fio.html:214`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`) |

O histórico também lê os instantes dinamicamente por `getattr`, e a fotografia de passivo lê os campos para reversão; essas leituras não são escritores adicionais. (`app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:83`)

4. Patch não commitado. [nao verificado]

**(a) O retorno

[truncado em 20000 de 32629 caracteres]

## 01/11 16:40 · sessao 30d159e9fd65 · - · - · origem=Ronald (tmux)

Auditoria concluída e enviada com todas as referências executáveis das seis portas e limites do AST. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:373]

Verificação em memória, sem Django ou alterações de arquivos:

- Mutante com chamada direta externa: teste da árvore vermelho; seis testes de fontes sintéticas verdes. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:433] [nao verificado]
- Mutante usando `getattr(p, '_escrever_' + 'carimbo')`: sete testes novos verdes; scanner de escritores também não o reconhece. [app/chamados/tests/test_o261e_f2_porta_carimbo.py:403] [nao verificado]
- Mutante sem ofuscação, trocando `VIA_PARIDADE_RECUSADA` por `VIA_ADMIN_DECIDIU`: **oito testes pedidos verdes**; a whitelist valida função e arquivo, sem validar a via usada. [app/chamados/services/materializacao.py:858; app/chamados/tests/test_o261e_f2_porta_carimbo.py:388]
- Mutante restaurando `alvo.update(...)` no originário: teste novo do censo vermelho pela âncora desaparecida. [app/chamados/models.py:1488; app/core/tests/test_censo_escritas.py:235]

Resultado da suíte Django completa: [nao verificado]. [nao verificado]

## 01/11 16:39 · sessao 7d4e42e59c6f · - · - · origem=Ronald (tmux)

Q3/Q4b — achados consolidados, somente leitura.

Os dois escritores adicionais de `via_resolucao` são deliberadamente permitidos pelo selo separado `PORTA_VIA`; portanto existem fora das seis portas perguntadas, mas não constituem fuga da allowlist atual. O contrato de exclusividade dos campos `materializada_em`/`materializada_por` é distinto do contrato da via. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`, `:44`, `:51`, `:238`)

| Campo | Escritores persistentes fora das seis portas |
|---|---|
| `materializada_em` | Nenhum escritor explícito de `PerguntaDisputa` encontrado nas formas varridas; a atribuição está em `_escrever_carimbo`, e o lote limpa pelo gerente do modelo. (`app/chamados/models.py:2529`, `:2626`) |
| `materializada_por` | Idem; atribuição de FK por `_id` em `_escrever_carimbo`, limpeza no lote. (`app/chamados/models.py:2530`, `:2627`) |
| `via_resolucao` | `PerguntaDisputa.encerrar_por_fato_no_espelho`: atribui e salva somente a via; `relavra_via.relavrar`: atribui a nova via, relavra veredito e salva. (`app/chamados/models.py:2636`, `:2637`; `app/chamados/services/relavra_via.py:123`, `:124`, `:125`) |
| `respondida_em` | `responder_pergunta_disputa_core` atribui `now()` e salva; `reabrir_para_correcao` limpa no `update`; `responder_automaticamente` atribui o instante recebido e salva; `carimbar_resposta_pela_trilha` atribui o instante testemunhado e salva. (`app/chamados/services/disputa_emissao.py:1248`, `:1266`, `:1358`, `:1378`, `:1394`, `:1395`, `:1410`, `:1411`) |
| `validada_em` | `_carimbar_validacao` atribui `now()` e salva; `devolver_validacao_recusada` limpa; resposta nova limpa validação anterior; reabertura para correção limpa em lote. (`app/chamados/services/validacao.py:131`, `:133`, `:188`, `:190`; `app/chamados/services/disputa_emissao.py:1260`, `:1266`, `:1358`, `:1378`) |
| `validada_por` | Os mesmos quatro atos de `validada_em`, com autor na validação e `None` nos desfazimentos. (`app/chamados/services/validacao.py:132`, `:133`, `:189`, `:190`; `app/chamados/services/disputa_emissao.py:1261`, `:1266`, `:1358`, `:1378`) |
| `resposta_colab` | `responder_pergunta_disputa_core` escreve a resposta; `reabrir_para_correcao` escreve `''`; `responder_automaticamente` escreve o texto recebido. (`app/chamados/services/disputa_emissao.py:1247`, `:1266`, `:1358`, `:1378`, `:1393`, `:1395`) |

`ChamadoColaborador.ressuscitar_perguntas` fornece `validada_em=None`, `validada_por=None`, `respondida_em=None` à porta de lote; não executa um segundo `update` próprio desses campos. (`app/chamados/models.py:1488`)

As atribuições `_q.validada_em=None` e `_q.respondida_em=None` são simulação em cópia de memória: o código copia a pergunta, copia `_state` e o cache das relações, aplica `descarimbar(salvar=False)` e consulta o C6. Não persiste essa `_q`. (`app/chamados/models.py:1427`, `:1430`, `:1431`, `:1439`, `:1440`, `:1441`, `:1442`)

Há também comportamento de exclusão de usuário: ambas as FKs de autoria usam `on_delete=SET_NULL`, o que pode zerar autoria fora dessas portas. O próprio varredor declara que não alcança esse mecanismo. (`app/chamados/models.py:2740`, `:2742`, `:2760`, `:2762`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`)

`regularizacao_ext.materializar_regularizacao_externa` escreve campos homônimos em `RegularizacaoExterna`, não em `PerguntaDisputa`; a exceção está explicitada na allowlist. (`app/chamados/services/regularizacao_ext.py:198`, `:199`, `:200`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`, `:49`)

`PerguntaDisputa.abrir` recebe `**campos` e os repassa a `objects.create`, portanto é um canal genérico de criação capaz de receber esses campos; nenhum caller atual fornecendo os campos examinados apareceu na varredura explícita. A impossibilidade de fazê-lo dinamicamente não está provada. (`app/chamados/models.py:2374`, `:2452`, `:2453`; [nao verificado] para ausência universal de argumentos dinâmicos)

Leitores que explicam o efeito dos campos:

| Campo | Leitores e consequência |
|---|---|
| `materializada_em` | C6 lê identidade/tranca, fato do marco e lastro; `.estado` distingue validada/materializada; materializador seleciona apenas nulo; raiox exclui materialização com veredito `VL_SIM`; procedência usa a data como alternativa à validação. (`app/chamados/juizes.py:1042`, `:1071`, `:1077`, `:1107`; `app/chamados/models.py:2475`, `:2480`; `app/chamados/services/materializacao.py:1261`; `app/chamados/services/raiox.py:133`; `app/ponto/services/procedencia_batida.py:74`) |
| `materializada_por` | Foto de reversão do ato, autoria da procedência e rollback em memória da porta. (`app/chamados/services/materializacao.py:618`; `app/ponto/services/procedencia_batida.py:71`, `:72`; `app/chamados/models.py:2552`, `:2581`, `:2582`) |
| `via_resolucao` | C6 julga identidade/tranca; relavra escolhe somente vias derivadas; materializador reconhece recusa de paridade; template escolhe explicação ao colaborador. (`app/chamados/juizes.py:1042`, `:1071`; `app/chamados/services/relavra_via.py:116`, `:119`; `app/chamados/services/materializacao.py:423`; `app/templates/chamados/partials/_card_atendimento.html:186`) |
| `respondida_em` | C6 só entra no degrau da resposta se data e texto existem; `.estado` distingue pendente/respondida; validação recusa sem data; fila global exige data; raiox separa respondidas/não respondidas pela coluna; template condiciona apresentação. (`app/chamados/juizes.py:1047`, `:1284`; `app/chamados/models.py:2479`, `:2482`; `app/chamados/services/validacao.py:363`; `app/chamados/services/raiox.py:136`, `:137`, `:138`; `app/templates/chamados/partials/modal_fio.html:212`) |
| `validada_em` | Primeiro degrau do C6 mata a pergunta; validação evita repetição; materializador exige não nulo; fecho sem credor seleciona só nulo. (`app/chamados/juizes.py:1033`; `app/chamados/services/validacao.py:366`; `app/chamados/services/materializacao.py:1260`; `app/chamados/services/disputa_emissao.py:1620`) |
| `validada_por` | Foto da validação e reversão, métricas de autoria e procedência da batida. (`app/chamados/services/validacao.py:139`; `app/chamados/services/materializacao.py:616`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`, `:244`; `app/ponto/services/procedencia_batida.py:72`) |
| `resposta_colab` | C6 exige texto com data e compara com a luz; materializador interpreta a hora e recusa texto vazio; validação classifica a resposta; telas exibem texto; auditoria detecta meio-estado. (`app/chamados/juizes.py:1047`, `:1010`; `app/chamados/services/materializacao.py:264`, `:393`, `:763`; `app/chamados/services/validacao.py:806`; `app/templates/chamados/partials/modal_fio.html:214`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`, `:1102`, `:1103`) |

O histórico lê dinamicamente os três instantes por `getattr(p, campo)` e usa o texto da resposta para compor os eventos; a foto de reversão de passivo também lê dinamicamente os campos, sem escrevê-los. (`app/chamados/services/historico.py:99`, `:101`, `:103`, `:105`, `:109`; `app/chamados/services/passivo_validado.py:64`, `:66`, `:83`)

Para Q4b, não encontrei escritor atual que grave `resposta_colab` **preenchida** deixando de atribuir também `respondida_em`: o core atribui o par e salva junto; automático também atribui e salva o par. A limpeza de correção deixa `resposta_colab=''` com data nula, que é outro estado. (`app/chamados/services/disputa_emissao.py:1247`, `:1248`, `:1249`, `:1266`, `:1393`, `:1394`, `:1395`, `:1358`, `:1378`)

A porta automática não valida `instante is not None`, mas o único caller de produção localizado passa `agora=timezone.now()`. Assim, a possibilidade da API interna produzir meio-estado existe se usada com `None`; sua ocorrência nos dados fornecidos e a origem histórica dos registros continuam [nao verificado]. (`app/chamados/services/disputa_emissao.py:1382`, `:1391`, `:1393`, `:1394`; `app/api/foto_recebida.py:42`, `:48`)

A frase da sonda “só dado legado” não é uma conclusão provada pelo anexo: ele não traz resultados, datas de escrita ou trilha dos registros. [nao verificado]

## 01/11 16:39 · sessao 8051cb744c18 · - · - · origem=Ronald (tmux)

4e — A sonda usa funções reais, mas não prova uma execução completa do patch: `carimbar` consulta C6 e salva somente quando morta (`app/chamados/models.py:2565`, `app/chamados/models.py:2576`); `reabrir` usa `recusa_da_reabertura` e executa efeitos reais (`app/chamados/models.py:1338`, `app/chamados/models.py:1380`). A execução/saída `SONDA_OK` não consta do dado apresentado. [nao verificado]

O `atomic` externo com exceção desfaz as escritas da conexão participante; isso é ensaio com rollback, não conexão configurada para somente leitura. A configuração sombra declara apenas `default`, herdado de saas (`app/config/settings/sombra.py:27`, `app/config/settings/saas.py:83`); trilhas usam ORM sem outra conexão (`app/core/observ.py:133`). Ausência absoluta de escritas por qualquer conexão ou dependência transitiva: [nao verificado].

Há escapes concretos do procedimento documentado:

- O comando indicado chama `lavra_de_prod` antes da sonda (`bin/sombra.sh:489`). Essa função cria diretório e arquivos (`bin/sombra.sh:137`, `bin/sombra.sh:144`, `bin/sombra.sh:151`) e lança outro processo que grava `MetricaSnapshot` por dois `update_or_create`, fora do `atomic` da sonda (`bin/sombra.sh:157`, `bin/sombra.sh:160`, `bin/sombra.sh:166`).
- O wrapper grava a saída no host usando `tee` (`bin/sombra.sh:492`). O app está montado somente para leitura, mas `/sombra` é gravável e logs/media são tmpfs (`bin/sombra.sh:189`).
- Logging não participa do rollback: exceções do cartório chegam ao logger `ponto` (`app/ponto/services/cartorio.py:1763`), configurado com arquivo (`app/config/settings/saas.py:162`, `app/config/settings/saas.py:181`). `registrar_engolido` também modifica contador em memória e emite logging (`app/core/observ.py:75`, `app/core/observ.py:79`).
- Até C6 pode engolir exceção e continuar, portanto “qualquer exceção sobe” não descreve todas as funções chamadas (`app/chamados/juizes.py:1104`, `app/chamados/juizes.py:1115`); o reconciliador também captura falhas por chamado (`app/chamados/reconciliador.py:1369`).

A medição de laço cobre `reconciliar_chamados_colab`, que chama os reconciliadores e percorre chamados vivos (`app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1356`). Não cobre a rodada do consumidor do outbox (`app/chamados/reconciliador.py:1255`) nem os efeitos pendentes em `on_commit` (`app/chamados/signals.py:121`, `app/chamados/signals.py:202`, `app/chamados/signals.py:251`). O outbox encontrado é tabela ORM, não fila externa (`app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:631`). Ausência de laço após commit e consumidores: [nao verificado].

Item 5 — O registro vivo aponta C6 para `chamados.juizes.pergunta_viva` (`app/core/juizes.py:266`), e a porta efetivamente o consulta (`app/chamados/models.py:2565`): nenhum segundo juiz é necessário para o patch proposto. A dependência problemática é a promessa de reabrir sem demonstrar que as guardas aceitam o fixture: a porta recusa antes de mudar estado (`app/chamados/models.py:1285`, `app/chamados/models.py:1338`).

Os novos testes de formas são testes sobre strings artificiais (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:445`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:469`); a fiscalização da produção é o teste que lê a árvore (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:433`). O censo novo lê fonte real, mas exige uma âncora textual específica e sua substituição (`app/core/tests/test_censo_escritas.py:233`, `app/core/tests/test_censo_escritas.py:235`, `app/core/tests/test_censo_escritas.py:244`). Não equivale a prova universal contra mutações de escrita.

## 01/11 16:18 · sessao 4f8555cb3c3e · O261-E-F2 · censo · origem=Code

Nos anexos transcritos, conto linhas desde a primeira linha de cada arquivo, incluindo linhas em branco e excluindo o marcador `== anexo ==`.

1. **(a) — Codex não viu, com o tema perguntado.**  
   **Pergunta:** “selo/RED sem mordida (o selo AST de test_o261e_f2_porta_carimbo.py morde mutante real?)” (`codex_v1.q:25`); reiterada para o diff total (`codex_v3.q:14`).  
   **Resposta/triagem sobre o bypass por chamadas permitidas:** **nenhum**. As respostas apontaram escapes nas formas de atribuição, outro defeito do selo (`codex_v1.r:150`; `codex_v1.r:152`). O selo permite `_escrever_carimbo`, cuja implementação escreve sem consultar C6; a consulta está em `carimbar` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`; `app/chamados/models.py:2527`; `app/chamados/models.py:2565`).

2. **(b) — Codex viu; a triagem reduziu a nota.**  
   **Pergunta:** comportamento observável de cada chamador “quando a porta RECUSA”, incluindo o que devolve ou emite (`codex_v1.q:15`; `codex_v1.q:16`; `codex_v1.q:17`).  
   **Resposta:** para `_materializar_ausencia_de_resposta`, “Ignora o booleano. A ausência já criada/reaberta continua” (`codex_v1.r:93`).  
   **Triagem/motivo:** “Nota. O Codex mesmo diz que o conjunto segue restrito ao efetivamente carimbado” (`triagem_v3.md:9`). O núcleo — recusa ignorada e redução do conjunto absorvido — foi tratado; a formulação específica “sem trilha/log” não aparece nesse tratamento (`codex_v1.r:93`; `triagem_v3.md:9`).

3. **(b) — Os dois componentes foram vistos e encaminhados à fila.**  
   **Pergunta:** possibilidade de gravar e reler VIVA; divergência entre memória e banco; ordem de trava (`codex_v1.q:18`; `codex_v1.q:20`; `codex_v1.q:26`).  
   **Resposta:** “sem reler ou travar a pergunta” e “exceção da lavra ou do `save`, depois da aprovação, fica fora do `try` de reversão” (`codex_v1.r:109`; `codex_v1.r:126`).  
   **Triagem/motivos:** concorrência anterior ao diff e alegada mudança da ordem de trava de sete chamadores (`triagem_v1.md:9`; `triagem_v2.md:10`). Para memória, a triagem corrigiu “o atomic cobre”, reconheceu que a memória permanece carimbada e encaminhou: “Não é regressão; vai a fila” (`triagem_v3.md:11`).

4. **(b) — Relógio visto e remetido a complemento.**  
   **Pergunta:** “relogio lido fora da borda” (`codex_v1.q:24`), repetida na volta final (`codex_v3.q:13`).  
   **Resposta:** “Os novos métodos têm `now()` de fallback”; a injeção integral permanece incompleta (`codex_v1.r:144`).  
   **Triagem/motivo:** “a leitura do relogio MUDOU DE SITIO, nao nasceu”, encaminhando ao complemento RELOGIO-INJETADO (`triagem_v2.md:9`; confirmado em `triagem_v3.md:12`). O detalhe de `encerrar_fantasma` chamar sem `quando` está no código, mas não foi explicitamente nomeado na resposta citada (`app/chamados/models.py:2648`; `codex_v1.r:144`).

5. **(b) — Visto; descarte agregado, sem motivo individual registrado.**  
   **Pergunta:** “derivacao paralela de fonte unica” (`codex_v1.q:24`; `codex_v1.q:25`).  
   **Resposta:** “Permanece a separação já existente entre identidade no C6, no queryset e na lista da relavra”, citando os dois sítios do opus (`codex_v1.r:145`).  
   **Triagem:** encerra agregadamente “2o juiz, derivacao paralela e 2o escritor explicito: nenhum” (`triagem_v2.md:12`). **Classifico em (b) por esse encerramento agregado; motivo específico da triagem para manter as listas 5×4: [nao verificado].** A preexistência foi registrada pela resposta, não como justificativa individual da triagem (`codex_v1.r:145`; `triagem_v2.md:12`).

6. **(b) — Os três restos foram vistos e deixados para outro pouso.**   [nao verificado]
   **Pergunta:** “Algum chamador de `fechar_por_competencia_trancada` [...] sobrou?”, além do mandato “L-111 apaga” (`codex_v1.q:27`; `codex_v1.q:7`).  
   **Resposta:** “Restam referências instrumentais/documentais obsoletas dizendo que a porta existe”, citando exatamente os três arquivos do opus (`codex_v1.r:160`).  
   **Triagem/motivo:** “INCOMPLETO declarado. E instrumento (`bin/`) e doc, com pouso proprio (RAIAS 5)” (`triagem_v1.md:15`). [nao verificado]

7. **(a) — Codex não viu, com o tema perguntado.**  
   **Pergunta:** “selo/RED sem mordida”, reiterada para o “diff total” (`codex_v1.q:25`; `codex_v3.q:14`).  
   **Resposta/triagem sobre a perda da testemunha da classe (f): nenhum.** O teste passa a exigir uma chamada AST a `descarimbar_em_lote` e um `update()` censado diretamente, sem `[relacionado]` (`app/core/tests/test_censo_escritas.py:210`; `app/core/tests/test_censo_escritas.py:214`). É uma lacuna estrutural inspecionável no teste; classificá-la não depende de recuperar uma execução histórica de RED (`app/core/tests/test_censo_escritas.py:202`).

8. **(e) — Não perguntado quanto ao custo da dupla execução.**  
   **Pergunta mais próxima:** menciona “memo `_pv_memo`”, mas pergunta se a porta grava e a releitura resulta VIVA; não pede avaliação de custo ou quantidade de consultas ao C6 (`codex_v1.q:18`; `codex_v1.q:19`).  
   **Resposta relacionada:** descreve que `lavrar_sem_turno_aberto` “antes salvava depois do pré-C6”, sem apontar o custo da segunda avaliação (`codex_v1.r:88`).  
   **Resposta/triagem do achado de custo: nenhum.** Os dois pontos de chamada e a alteração dos campos da chave estão no código (`app/chamados/reconciliador.py:398`; `app/chamados/reconciliador.py:405`; `app/chamados/models.py:2565`; `app/chamados/juizes.py:790`; `app/chamados/juizes.py:791`).

9. **(e) — Não perguntado quanto ao vocabulário do commit/comentário.**   [nao verificado]
   **Trechos delimitadores:** as perguntas pedem escritores, leitores e classes de defeitos; nenhuma pede auditoria lexical do commit ou da palavra “fallback” em comentários (`codex_v1.q:11`; `codex_v1.q:23`; `codex_v2.q:17`; `codex_v3.q:13`).   [nao verificado]
   **Resposta/triagem deste achado: nenhum.** O “fallback” mencionado na resposta refere-se ao relógio, outro tema (`codex_v1.r:144`). A ocorrência no comentário está confirmada (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:404`); a ocorrência no corpo original do commit, além do relato do opus, é **[nao verificado]**.

| achado | classe | volta | trecho |
|---|---|---|---|
| 1 — chamadores sem selo | **(a)** | v1/v3 | Perguntado “selo/RED sem mordida”; resposta específica: nenhum (`codex_v1.q:25`; `codex_v3.q:14`). |
| 2 — recusa silenciosa na ausência | **(b)** | v1 → triagem v3 | “Ignora o booleano”; triagem: conjunto restrito ao efetivamente carimbado (`codex_v1.r:93`; `triagem_v3.md:9`). |
| 3 — trava e memória após erro | **(b)** | v1–v3 | Janela anterior; memória: “Não é regressão; vai a fila” (`triagem_v1.md:9`; `triagem_v3.md:11`). |
| 4 — relógio nas portas | **(b)** | v1–v3 | “MUDOU DE SITIO, nao nasceu”; complemento RELOGIO-INJETADO (`triagem_v2.md:9`). |
| 5 — duas listas de identidade | **(b)** | v1 → triagem v2 | Separação registrada; encerramento agregado “nenhum”; motivo individual ausente (`codex_v1.r:145`; `triagem_v2.md:12`). |
| 6 — restos de L-111 | **(b)** | v1 | “INCOMPLETO declarado”; instrumento/doc com pouso próprio (`triagem_v1.md:15`). | [nao verificado]
| 7 — testemunha da classe (f) | **(a)** | v1/v3 | Perguntado “selo/RED sem mordida”; resposta específica: nenhum (`codex_v1.q:25`; `codex_v3.q:14`). |
| 8 — custo de dois C6 | **(e)** | v1, pergunta mais próxima | Memo perguntado quanto à vivacidade, sem pergunta de custo (`codex_v1.q:18`; `codex_v1.q:19`). |
| 9 — palavra “fallback” | **(e)** | v1–v3 | Perguntas de defeitos técnicos, sem auditoria lexical (`codex_v1.q:23`; `codex_v2.q:17`; `codex_v3.q:13`). |

_1446 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
