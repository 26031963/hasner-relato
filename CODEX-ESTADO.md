# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 14:32** · publicado em **30/10/2026 14:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 14:32 · sessao ae95044256e7 · - · - · origem=Ronald (tmux)

Confirmado estaticamente: **o teste novo de idempotência falha com relógio correndo**, na comparação de todas as colunas em `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264`.

A segunda chamada percorre:

- O derivador exclui o chamado pela própria pergunta pendente (`app/chamados/services/disputa_emissao.py:209`).
- c1 recusa intervalo (`app/chamados/reconciliador.py:363`); c2 encontra chamado vivo (`:793`) e reutiliza-o (`app/chamados/models.py:670`, `:688`).
- c2 reaponta novamente (`app/chamados/reconciliador.py:871`).
- A porta relavra e salva `julgado_em`, sem guarda de destino já igual (`app/chamados/services/fio.py:797`).
- A lavra atribui `timezone.now()` incondicionalmente (`app/chamados/veredito_lavrado.py:228`), alterando o registro comparado pelo teste.

**Ressalva relevante:** o runner pode congelar o relógio quando `CONGELAR_EM` está definido; nesse modo a comparação pode passar apesar da reescrita (`app/core/runner_quarentena.py:93`). O arquivo novo não congela nem avança o relógio (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:249`).

Classificação sugerida: teste novo incompatível com o código atual / prova de idempotência vulnerável ao relógio congelado. Execução efetiva e histórico de RED/pouso: [não verificado]. [nao verificado]

## 30/10 14:32 · sessao 0baf4c5a67c5 · - · - · origem=Ronald (tmux)

Leitura estática; testes e mutantes não executados. [não verificado] [nao verificado]

**Achados na classe selo/RED:**

1. **A bateria não distingue o C6 de um segundo juiz por `validada_em`.** A fábrica define todas as vivas com `validada_em=None` e todas as mortas com o campo preenchido; os únicos casos mortos são R3 e R6. Portanto, substituir as novas consultas ao C6 por `p.validada_em is None` preserva os resultados previstos destes cenários, por inspeção. Isso deixa sem proteção mortes por tranca, vias ou célula, embora o C6 possua esses degraus. Fontes: `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:92`, `:150`, `:159`; `app/chamados/reconciliador.py:712`, `:1438`; `app/chamados/juizes.py:1008`, `:1039`, `:1054`. Execução do mutante: [não verificado].

2. **A bateria não sela a localização da cura no c2.** O helper chama apenas `reatar_pergunta_orfa`; o outro ponto de entrada testado é o cron. Não existe chamada direta ao `reemitir_chamado_do_dia` no arquivo. Assim, uma implementação que preserve a antiga recusa na porta c2 e faça o alargamento apenas no wrapper pode satisfazer estes cenários sem cumprir o contrato da própria porta. Fontes: `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:95`, `:209`; contrato da porta: `app/chamados/reconciliador.py:715`, `:719`. Execução desse mutante: [não verificado].

3. **O cadastro dos intervalos usa chaves diferentes das consumidas pelo juiz.** A fixture grava `ii`/`fi`, enquanto o juiz do marco procura `hii`/`hfi`; sem essas horas, `fato_do_marco_no_espelho` devolve `None`. Os casos de intervalo provam o comportamento com veredito genérico `furo`, mas não o vínculo com a lâmpada específica do intervalo. Fontes: `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:66`, `:67`; `app/chamados/juizes.py:823`, `:849`, `:863`, `:1054`.

**Sobre RED com a cura removida:**

- R4 exportada, R4b, R5 e R6 conservam o comportamento antigo e passam por inspeção mesmo sem a cura; isso é compatível com seu papel de controles negativos. R6 “MORDE” protege contra alargamento indevido às mortas, mas não demonstra RED contra o ancestral. Fontes: `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:141`, `:148`, `:169`, `:182`. Comparação executada contra o ancestral: [não verificado].
- R1 exige destino vivo ou recusa; R2 exige criação e costura; R3 exige costura e materialização; o controle R4 exige emissão sem exportação. Não identifiquei vacuidade equivalente nessas asserções contra a remoção integral da cura. Fontes: `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117`, `:133`, `:161`, `:178`.

**Limites do selo existente:**

- O selo do chokepoint mede queries e callbacks; seu cenário explícito de três perguntas já nasce com `chamado_destino=dest`, portanto esse cadastro não exercita a nova seleção HIST da órfã. Fontes: `app/ponto/tests/test_selo_chokepoint_escrita.py:207`, `:226`, `:239`, `:258`. Uma ultrapassagem efetiva de teto por O261-C não foi demonstrada. [não verificado]
- A exclusividade da escrita tem selo estrutural próprio indicado pela porta; a bateria nova de resultado não substitui esse selo. A implementação atual continua chamando `reapontar_resolvedora` tanto no HIST quanto no c2. Fontes: `app/chamados/services/fio.py:743`, `:750`, `:785`; `app/chamados/reconciliador.py:856`, `:1487`.

**Na implementação examinada:** segundo juiz real: **nada** — os novos helpers chamam autoridades existentes; segundo escritor novo: **nada** — preservam as portas. Fontes: `app/chamados/reconciliador.py:700`, `:711`, `:816`, `:856`, `:1487`.

## 30/10 14:32 · sessao d41846d646d4 · - · - · origem=Ronald (tmux)

- **2º escritor fora da porta: nada.** O novo alcance segue pelo emissor existente, que chama `ChamadoColaborador.abrir`, e pela costura `reapontar_resolvedora`; o HIST também usa essa costura e o S128. `app/chamados/reconciliador.py:764`, `:831`, `:871`, `:1507`, `:1513`; escrita centralizada em `app/chamados/services/fio.py:797`.
- **Ordem de trava: nada atribuível ao diff.** A fábrica mantém Disputa antes de Colaborador, e o novo desvio HIST→c2 reutiliza `abrir`. `app/chamados/services/disputa_emissao.py:466`, `:467`; `app/chamados/models.py:666`, `:669`; `app/chamados/reconciliador.py:1460`, `:1487`.
- **Pouso antes do pré-requisito: nada demonstrado pelo código.** O c2 conserva os gates anteriores ao nascimento; o renascimento conserva as recusas de tranca/premissa antes da escrita. `app/chamados/reconciliador.py:795`, `:805`, `:807`, `:822`, `:824`, `:829`; `app/chamados/models.py:768`, `:770`, `:1050`, `:1089`. A ordem histórica B→C exigida no corte não foi verificável nesta cópia sem Git. `app/docs/CORTES.md:399`; [não verificado].

A suspeita de desviar motivos fora do grupo DIA não procede nos chamadores atuais: ambos restringem o universo ao grupo, e o mapa antigo também só casava `orfao_14h`/`saida_sem_entrada`. `app/chamados/reconciliador.py:1351`, `:1374`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/models.py:2466`, `:2478`.

## 30/10 14:32 · sessao 87185b895018 · - · - · origem=Ronald (tmux)

Achado confirmado por leitura: **o selo novo de idempotência forte encontra uma alteração real na segunda chamada**.

- O teste recarrega a pergunta e chama diretamente `reatar_pergunta_orfa` novamente; depois compara a linha inteira. `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:99`, `:234`, `:235`.
- O derivador exclui o chamado que já possui a própria pergunta pendente, voltando ao c2. `app/chamados/services/disputa_emissao.py:209`.
- c1 recusa intervalo; o c2 aceita o chamado vivo existente. `app/chamados/reconciliador.py:363`, `:784`; `app/chamados/models.py:671`, `:688`.
- A costura permite o mesmo dia e sempre relavra/salva; a lavra atribui novo `julgado_em`. Portanto a comparação integral diverge nesse campo. `app/chamados/juizes.py:1721`; `app/chamados/services/fio.py:797`; `app/chamados/veredito_lavrado.py:228`.
- O teste de cron tem outra semântica: passa pelo filtro de órfãs, podendo legitimamente retornar zero na segunda rodada. `app/chamados/reconciliador.py:1380`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:245`.

Demais pontos:

- **Selo/RED sem mordida:** nada demonstrado; R7 diferencia janela de mês civil e o novo IDEM efetivamente expõe a alteração acima. `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:173`, `:235`. Execução/histórico do RED: [não verificado].
- **Custo:** teto continua 194; cenário FICAM cria chamados vivos, sem cobrir o ramo novo com encerrados. Limite de cobertura, sem nova regressão de custo demonstrada. `app/ponto/tests/test_selo_chokepoint_escrita.py:164`, `:364`.
- **Relógio fora da borda:** nada novo identificado nos helpers; datas de cenário são explícitas. `app/chamados/reconciliador.py:685`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:47`.
- **Pouso antes do pré-requisito:** nada demonstrado; o RELATO registra B no ar e passivo aplicado antes do trabalho C. `app/docs/RELATO.md:3`, `:4`, `:7`; requisito em `app/docs/CORTES.md:399`. Execução histórica dos registros: [não verificado].

## 30/10 14:31 · sessao 8c1cbeb265c1 · - · - · origem=Ronald (tmux)

Chamadores e alcance confirmados:

- `reatar_pergunta_orfa` é chamado pelo laço de órfãs (`app/chamados/reconciliador.py:1377`) e pela emissão de disputa, dentro de savepoint (`app/chamados/services/disputa_emissao.py:404`, `:408`, `:409`).
- O laço também roda no evento de batida: `app/ponto/signals.py:214` → `app/chamados/reconciliador.py:1302`; o cron é `*/15` (`app/config/crons.py:376`) e inclui colaboradores com órfãs pela definição compartilhada (`app/chamados/management/commands/reconciliar_chamados.py:36`).
- `reemitir_chamado_do_dia` tem um chamador de produção encontrado: `app/chamados/reconciliador.py:1467`. A documentação da porta dizendo “HOJE SO O CRON A CHAMA” está desatualizada, mas é anterior ao diff (`app/chamados/reconciliador.py:1389`; evento real em `app/chamados/services/disputa_emissao.py:409`).

**Achado concreto de custo:** o intervalo vivo que chega ao c2 consulta a mesma competência duas vezes: uma em `app/chamados/reconciliador.py:1436`, outra via `:749` → `:709`. Cada chamada executa a consulta e carrega os registros de exportação, sem memo, em `app/folha/export.py:570`. O memo do C6 (`app/chamados/juizes.py:791`) não elimina essas duas leituras do K6. Além disso, C6 agora é executado antes das recusas baratas do c1 (`app/chamados/reconciliador.py:1438`), embora o próprio c1 documente que deixava essa consulta por último para proteger o custo da batida (`app/chamados/reconciliador.py:394`). Estouro efetivo do teto: **[não verificado]**.

**Lacuna de selo associada:** os testes novos chamam a porta diretamente, com célula carregada (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:98`); o cron carrega apenas disputa (`app/chamados/reconciliador.py:1374`). O cenário existente de chokepoint com três perguntas já lhes atribui destino (`app/ponto/tests/test_selo_chokepoint_escrita.py:239`), enquanto o seletor deste caminho exige destino nulo (`app/chamados/reconciliador.py:1355`). Portanto esse cenário não protege o custo novo de órfãs.

**Ordem de trava: nada novo comprovado.** O nascimento continua pela porta `abrir` (`app/chamados/reconciliador.py:816`), que trava o colaborador antes do dedup (`app/chamados/models.py:666`).

**Pouso antes do pré-requisito: nada demonstrável contra a cópia.** O corte exige Maria B antes de C (`app/docs/CORTES.md:399`); o relato registra B no ar e remedição pós-B (`app/docs/RELATO.md:3`, `:4`). Autenticidade de implantação, logs e ancestralidade dos hashes: **[não verificado]**.

**MORTA respondida:** o HIST pode costurá-la, mas S128 preserva resposta sem testemunha, recusando carimbo (`app/chamados/services/disputa_emissao.py:1558`). Portanto não procede alegar automaticamente que a mudança enterra toda resposta; falta um caso concreto que ultrapasse essa guarda. O HIST novo e sua chamada ao S128 estão em `app/chamados/reconciliador.py:1450`, `:1487`, `:1493`.

## 30/10 14:31 · sessao ad7179c46d50 · - · - · origem=Ronald (tmux)

Não encontrei falha nova nas classes abaixo; revisão estática, sem execução de testes. [não verificado] [nao verificado]

- **2º juiz:** nada. O alcance chama `janela_atual` + `competencia_entregue`; vitalidade chama C6; HIST chama `motivos_atendidos` e C7: `app/chamados/reconciliador.py:704`, `:717`, `:1451`, `:1463`. O juiz dos motivos devolve a constante compartilhada em `app/chamados/juizes.py:1592`.
- **2º escritor fora da porta:** nada. c2 continua por `ChamadoColaborador.abrir` e `reapontar_resolvedora`; HIST continua por essa mesma costura e S128: `app/chamados/reconciliador.py:822`, `:862`, `:1500`, `:1506`. A costura valida antes de gravar e lavra no mesmo save: `app/chamados/services/fio.py:792`.
- **Relógio fora da borda:** nada novo. O diff consulta janela do dia, sem introduzir leitura de hora atual: `app/chamados/reconciliador.py:706`; a data continua entrando na porta antes do juiz e da lavra: `app/chamados/services/fio.py:788`.
- **Derivação paralela de fonte única:** nada além da questão do memo, delegada ao outro explorador. O HIST aberto migrou para o mesmo `motivos_atendidos` usado pelo derivador vivo: `app/chamados/reconciliador.py:1463`, `app/chamados/services/disputa_emissao.py:194`. O mapa antigo ficou restrito ao caminho conservado: `app/chamados/reconciliador.py:1466`.
- **Ordem de trava:** nada novo. A chamada nova alcança a porta existente, cujo `atomic` trava Colaborador antes do dedup; o evento conserva seu savepoint: `app/chamados/models.py:666`, `app/chamados/services/disputa_emissao.py:408`. Não há trava adicionada no trecho novo: `app/chamados/reconciliador.py:685`, `:1443`.
- **Pouso antes do pré-requisito:** nada demonstrado. O requisito está em `app/docs/CORTES.md:399`; `app/docs/RELATO.md:3` declara Maria B no ar (`b09b2dab`), passivo aplicado e remedição de C pós-B. Confirmação operacional dos deploys e sondas: [não verificado].

Não ressuscitar a acusação “morta com resposta”: o primeiro laço do S128 consulta C6, tranca e testemunha; o fecho sem credor é comportamento preexistente explicitamente descrito pela triagem do usuário: `app/chamados/services/disputa_emissao.py:1547`, `:1551`, `:1558`, `:1641`.

## 30/10 14:31 · sessao 0a5d10e605c0 · - · - · origem=Ronald (tmux)

**Achado: o memo pode ficar velho entre perguntas do mesmo dia na mesma instância.** A chave é apenas `dia`; depois da primeira consulta, a função devolve o booleano guardado sem consultar K6 nem invalidar o valor. Isso amplia a vida da leitura além da “MESMA pergunta” descrita no comentário. (`app/chamados/reconciliador.py:700-708`)

- **Cron:** o comando carrega um `colab` por iteração e passa essa instância a `reconciliar_chamados_colab`; esta chama o reconciliador de órfãs, que percorre todas as perguntas usando o mesmo objeto. Portanto o memo dura todo o lote de perguntas daquele colaborador, não somente HIST+c2 de uma pergunta. (`app/chamados/management/commands/reconciliar_chamados.py:51-56`; `app/chamados/reconciliador.py:1307-1308`; `app/chamados/reconciliador.py:1380-1383`)
- **Evento:** `gerar_perguntas_disputa` percorre os motivos e passa reiteradamente `disputa.colaborador` à porta; o `atomic` está dentro do laço, por tentativa. O mesmo objeto `disputa` também é reaproveitado nas chamadas consecutivas de geração e entradas pendentes. (`app/chamados/services/disputa_emissao.py:292`; `app/chamados/services/disputa_emissao.py:404-409`; `app/chamados/services/disputa_emissao.py:618-621`; `app/chamados/services/disputa_emissao.py:270-274`)
- **Mudança da fonte:** exportar chama `ExportacaoDominio.registrar`, que cria o registro sem atualizar esse memo. Invalidar também altera o registro sem invalidar o memo. K6, ao contrário, consulta o banco e considera a invalidação do vigente. (`app/folha/views.py:297-300`; `app/folha/models.py:178-188`; `app/folha/models.py:158-162`; `app/folha/export.py:569-575`)
- **Consequência comprovável pelo fluxo:** depois de guardar `False`, uma entrega concluída entre duas consultas ao mesmo dia não impede que a segunda pergunta continue no alcance novo: o HIST usa o valor em `:1451`, e os intervalos entram no c2 por `:755` → `:715`. O c2 pode alcançar `ChamadoColaborador.abrir` sem nova leitura de K6. Isso contraria a cerca de competência exportada registrada no aval. (`app/chamados/reconciliador.py:702-708`; `app/chamados/reconciliador.py:1451-1454`; `app/chamados/reconciliador.py:754-756`; `app/chamados/reconciliador.py:715-718`; `app/chamados/reconciliador.py:822`; `app/docs/CORTES.md:399`)
- **Distinção:** não identifiquei uma exportação executada sequencialmente *pelo próprio* laço do cron ou de emissão; o caso de produção depende de exportação concorrente enquanto esse objeto vive. A existência de uma ocorrência real dessa intercalação não foi verificada. (`app/chamados/reconciliador.py:1380-1386`; `app/chamados/services/disputa_emissao.py:404-415`; ocorrência real: [não verificado])
- **Selo não cobre essa transição:** R4 exportada e o controle sem exportação montam cenários separados; idempotência reutiliza `c`, mas não muda a entrega entre consultas. Não há teste de `False → export → nova consulta` nem `True → invalidação → nova consulta` na mesma instância. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:198-209`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:222-247`)

A correção mínima compatível com a justificativa do custo seria limitar o compartilhamento de K6 à execução de uma pergunta, preservando o reaproveitamento HIST+c2; afirmar que isso resolve também a corrida dentro da própria operação exigiria sincronização adicional, que o memo atual não fornece. (`app/chamados/reconciliador.py:700-708`; `app/chamados/reconciliador.py:1451`; `app/chamados/reconciliador.py:1480`)

## 30/10 14:31 · sessao ae95044256e7 · - · - · origem=Ronald (tmux)

**Selo/RED sem mordida: nada demonstrável.** Os testes chamam a porta real (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:103`), exigem emissão/costura em R2 (`:141`), costura/materialização em R3 (`:169`) e incluem controle positivo do dia exportado sem exportação (`:234`). R9 verifica exclusão da pergunta sem data (`:200`); R10 distingue perguntas do mesmo colaborador (`:211`).

**Pouso antes do pré-requisito: nada demonstrável nessa leitura.** O corte exige B antes de C (`app/docs/CORTES.md:399`); o relato registra B no ar e C remedido após B (`app/docs/RELATO.md:3`). Histórico Git e execução histórica do RED: [não verificado].

Observação sem classificação como achado: o novo gate exige `dia` do tipo `date` (`app/chamados/reconciliador.py:704`), embora o corpo de c2 aceite texto ISO (`:766`, `:786`). Depende de identificar chamador efetivo passando texto para intervalo; ocorrência [não verificado].

## 30/10 14:31 · sessao daa17d8619f8 · - · - · origem=Ronald (tmux)

- **2º juiz que duplica juiz registrado: nada.** O alcance novo consulta K6, janela e C7; vitalidade consulta C6; correspondência histórica consulta `motivos_atendidos`. Fontes: `app/chamados/reconciliador.py:710`, `app/chamados/reconciliador.py:724`, `app/chamados/reconciliador.py:1460`, `app/chamados/reconciliador.py:1470`.
- **Derivação paralela de fonte única: nada.** O c2 importa `MOTIVOS_DO_DIA_SEM_FECHAR`, mesma constante devolvida pelo juiz; competência usa `janela_atual`. Fontes: `app/chamados/reconciliador.py:757`, `app/chamados/juizes.py:1594`, `app/chamados/juizes.py:1599`, `app/chamados/reconciliador.py:712`.
- **Relógio fora da borda: nada nos sítios novos examinados.** Os novos usos de `datetime` validam ou convertem o dia recebido; não consultam o instante corrente. Fontes: `app/chamados/reconciliador.py:703`, `app/chamados/reconciliador.py:1440`.

Ressalva investigada, **sem achado demonstrado**: `_o261c_fora` permanece na instância e poderia envelhecer se a mesma pergunta fosse reutilizada depois de exportar; não identifiquei essa trajetória nos chamadores atuais — cron carrega perguntas e evento chama uma vez por criação. Fontes: `app/chamados/reconciliador.py:707`, `app/chamados/reconciliador.py:1389`, `app/chamados/services/disputa_emissao.py:404`.

## 30/10 14:23 · sessao 3943ba67c44f · CALENDARIO-HOJE · verificador · origem=Code

**Parecer: reprova.** O teto HOJE/ONTEM foi removido, mas permanecem divergências por `sem_previsto`, incompatibilidade de chaves no cartório e reutilização de turnos de outra janela. O custo novo caiu para **+1 query por calendário elegível**, sem zerar. (`app/colaboradores/services/calendario.py:707-712`; `app/ponto/turnos.py:1852,1928,2130-2139`)

1. **Teto de data**

   **Antigo corte HOJE/ONTEM: sem achado.** Agora a pergunta cobre `data_ini` até `min(data_fim, hoje)`, incluindo datas anteriores a ontem. (`app/colaboradores/services/calendario.py:707-710`)

   **Ainda há “não encerrado” que o calendário desconsidera:** `sem_previsto` produz `False` na juíza, mas fica fora de `_em_curso`. Em trabalho sem batida, isso permite `falta`. Há também a divergência de janela descrita abaixo. (`app/ponto/turnos.py:2102-2105,2140`; `app/colaboradores/services/calendario.py:711-712,132-142,805`)

2. **Equivalência da alimentação: não vale universalmente**

   | Chamador | Alimentação efetiva |
   |---|---|
   | Cartório | Passa `celulas={(colaborador_id, data): célula}`; não fornece `turnos` nem `escalas`. (`app/ponto/services/cartorio.py:853`) |
   | `supra_juiz` | Não fornece alimentação; o lote carrega. (`app/ponto/management/commands/supra_juiz.py:108`) |
   | Emissor retroativo | Fornece apenas `agora`; o lote carrega. (`app/ponto/management/commands/emitir_furo_retroativo.py:62`) |
   | `par_completo_da_janela` | Não fornece alimentação e depois carrega turnos novamente. (`app/ponto/turnos.py:2151-2153`) |
   | Calendário | Fornece células com chave composta e turnos previamente carregados; omite `escalas`. (`app/escala/alimentacao.py:41-46`; `app/colaboradores/services/calendario.py:709-710`) |

   **Chaves incompatíveis no cartório — achado herdado.** O lote espera chave `(colaborador_id, data)`, mas repassa o mesmo argumento a `turnos_do_colab`, cujo contrato é `{data: célula}`. Este recompõe a chave como `(colaborador_id, chave_recebida)`, produzindo `(colaborador_id, (colaborador_id, data))`; `vinculo_do_dia` procura `(colaborador_id, data)` e perde a geradora. A passagem sem adaptação já constava da versão anterior mostrada no anexo. (`app/ponto/turnos.py:2114,2133,1852,1928`; `app/escala/alimentacao.py:237-254`; anexo `diff2.txt`, hunk de `dias_encerrados`)

   **Células com `dna=NULL`, vazio ou sem marcos: sem achado nessa regra.** Com mapa ou consultando sozinho, `marcos_do_dia` recorre ao template. O carregador normal traz o campo `dna`; campo diferido é outra situação, advertida pelo próprio contrato como possível N+1. (`app/escala/models.py:1089-1108`; `app/escala/alimentacao.py:41-46`)

   **Turnos de janela diferente — achado no reaproveitamento novo.** O calendário forma turnos para `[data_ini, data_fim]`, mas os entrega ao lote perguntado somente até hoje, contrariando a exigência documentada de “MESMA janela”. O lote aceita a lista sem reconstruí-la. (`app/colaboradores/services/calendario.py:310-311,707-710`; `app/ponto/turnos.py:2113-2114,2133`)

   Contraexemplo derivado do código: vínculo único 21h–05h, sem intervalos; única entrada em D às 01h; nenhuma célula em D−1/D; único DNA com marcos em D+2; agora D às 06h; calendário `[D−1,D+3]`. Na carga curta, o mapa DNA fica vazio e vira `None`: a entrada ancora em D−1, cuja aberta vive até D às 09h. Na carga larga, o DNA de D+2 torna o mapa não vazio; a falta de D−1 nesse mapa elimina as bordas da véspera, e a entrada ancora em D. Assim, D−1 fica sem aberta e passa a encerrado às 06h. **A resposta muda com a janela.** (`app/escala/utils.py:100-107`; `app/ponto/turnos.py:1845,1949,1297-1300,283-306,1495-1496,1992,2008-2011,2102-2105`)

   Portanto, nem os chamadores sem alimentação têm equivalência universal com a consulta unitária: o pareamento conserva essa dependência da janela. Execução do contraexemplo e ocorrência em produção: **[nao verificado]**. (`app/ponto/turnos.py:2079-2080,2133`; traço acima)

3. **Os três motivos e `sem_previsto`**

   **É regra própria do leitor.** A autoridade distingue quatro motivos de não encerramento; para ela, `sem_previsto` continua sendo não encerrado. O calendário seleciona três e deixa o quarto seguir pelo caminho dos encerrados. Isso não equivale a ler a resposta booleana da juíza e conflita com a LEI-AKITA 2. (`app/ponto/turnos.py:2073-2075,2102-2105,2140`; `app/colaboradores/services/calendario.py:704-712`; `CLAUDE.md:20-22`)

   A exceção está declarada no comentário local, mas o teste ainda promete genericamente “turno NAO encerrou → previsto”. Declaração local não modifica o julgamento da autoridade. (`app/colaboradores/services/calendario.py:702-703`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:1-5`)

4. **Custo adicional**

   **Conta estática da cadeia nova: +1 query por calendário/card** quando `data_ini <= min(data_fim, hoje)`; **zero** para janela inteiramente futura. Células e turnos são reaproveitados, mas `escalas=` não é passado: o lote consulta todos os vínculos com `select_related('tipo_escala')`. Não há consulta adicional por dia para ler o DNA nesse caminho. (`app/colaboradores/services/calendario.py:707-710`; `app/ponto/turnos.py:2129-2139`; `app/escala/models.py:1094-1108`)

   No mapa HTML, são **+N queries para N cards efetivamente montados**, nessa condição. No PDF, são os mesmos **+N**, pagos antes da bifurcação de renderização; a geração reutiliza os cards. (`app/relatorios/furos.py:164-188`; `app/relatorios/views.py:1541,1548-1551,1560-1566`)

   Esse é o incremento estático atribuível à obra identificável na cópia. Medição efetiva e comparação integral com a árvore de `d2c37edd`: **[nao verificado]**.

5. **Leitores e efeitos além de “falta → previsto”**

   A declaração local concentra-se em “previsto, não falta”, mas o classificador também permite `ok → previsto`, `escala_suspeita → previsto`, `escala_suspeita → sem_dados` no limbo e bloqueio da sobreposição de veredito. Para HOJE/ONTEM com `sem_previsto`, a exclusão nova permite os sentidos inversos em relação à volta anterior. (`app/colaboradores/services/calendario.py:132-152,177-178,697-703,805`; anexo `diff2.txt`, hunk de `_em_curso`)

   | Leitor | Efeito |
   |---|---|
   | Perfil e gaveta | Herdam status, ícone e tooltip. **Tooltip corrigido: sem achado.** (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/colaboradores/services/calendario.py:881-882`; `app/templates/colaboradores/partials/_calendario_grade.html:99-105`) |
   | Furos HTML/PDF | Pintura muda; seleção, `nfuro`, marcos faltantes e ordenação seguem a cadeia independente da grade. (`app/relatorios/furos.py:141-190`; `app/relatorios/views.py:1549-1551`; `app/relatorios/furos_pdf.py:56-69`) |
   | `resolver_dia_oferecido` | `previsto` não pede decisão: datas passadas podem perder a ação, conforme os demais filtros. (`app/colaboradores/services/calendario.py:190,198-199,895-896,925-926`) |
   | `vigia_fio` | Pode trocar `decisao_do_dia_pendente` por um dos ramos seguintes, inclusive `fio_morto`. Ocorrência real: **[nao verificado]**. (`app/chamados/services/vigia_fio.py:294-324`) |
   | Rodapé | Totais acompanham o status; `previstos` são contados, mas não exibidos. (`app/colaboradores/services/calendario.py:930-946`; `app/templates/colaboradores/partials/_calendario_grade.html:137-173`) |
   | `status_do_dia` e contagem | Herdam a alteração ao remontarem o calendário; a diferença de janela acima também impede garantir igualdade entre consulta unitária e grade larga. (`app/colaboradores/services/calendario.py:213-233`; `app/ponto/turnos.py:1297-1300`) |

Execução dos testes Django, contagem medida de queries e ocorrência desses cenários em produção: **[nao verificado]**. [nao verificado]

## 30/10 14:22 · sessao d3f052a57a8b · - · - · origem=Ronald (tmux)

**P2: não há equivalência universal.** O calendário reaproveita turnos de janela maior; o cartório continua passando chaves incompatíveis ao pareador. (`app/colaboradores/services/calendario.py:310,707-710`; `app/ponto/services/cartorio.py:853`; `app/ponto/turnos.py:1852,1928,2133`)

| Chamador | Alimentação e resultado da inspeção |
|---|---|
| Cartório | Passa `{(cid,data): célula}`; `dias_encerrados` repassa sem conversão a `turnos_do_colab`, que espera `{data:célula}` e transforma as chaves em `(cid,(cid,data))`; a geradora deixa de ser encontrada pelo pareador. É defeito herdado: o anexo mantém essa chamada antiga. (`app/ponto/services/cartorio.py:853`; `app/ponto/turnos.py:1852,1928-1932,2133`; `app/escala/alimentacao.py:237-247`) |
| Supra_juiz | Não fornece alimentação: lote carrega mapa composto para vínculo/fim e `turnos_do_colab` carrega separadamente seu mapa por data. Sem achado novo nesse contrato. (`app/ponto/management/commands/supra_juiz.py:108`; `app/ponto/turnos.py:2129-2139,1880-1882`) |
| Emissor retroativo | Mesmo caminho sem alimentação; sem achado novo nesse contrato. (`app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/turnos.py:2129-2139`) |
| `par_completo_da_janela` | Mesmo caminho sem alimentação; depois carrega turnos novamente. Sem achado novo nas chaves. (`app/ponto/turnos.py:2151-2154`) |
| Calendário | Mapa composto correto para fim/vínculo; evita repassar esse mapa ao pareador porque fornece `turnos=`. Porém os turnos vêm de `[data_ini,data_fim]`, enquanto o julgamento é solicitado até `min(data_fim,hoje)`. (`app/colaboradores/services/calendario.py:310,404,707-710`) |

**Contraexemplo lógico da janela diferente:** EC único noturno 21–05, sem intervalos; única batida E em D às 01h; agora D às 06h; nenhuma célula em D−1/D; uma célula com DNA em D+2; calendário `[D−1,D+3]`. São entradas hipotéticas, ocorrência em produção **[nao verificado]**.

- Carga curta `[D−1,D]`: seu pad termina D+1; não encontra DNA, então o mapa vira `None`. `_vespera` usa o template e ancora E01 em D−1. A aberta permanece viva até D09; juiz responde não encerrado. (`app/ponto/turnos.py:1845,1949,1297-1300,297-306,1992,2008-2011,2097-2100`)
- Carga larga do calendário: encontra DNA D+2, deixando o mapa não vazio; como D−1 não consta, `_vespera` devolve `None`, e E01 fica em D. D−1 fica sem aberta; às D06, seu fim D05 passou, e o lote alimentado responde encerrado. (`app/escala/utils.py:100-107`; `app/ponto/turnos.py:1297-1299,283-286,1495-1496,1973-1976,2102-2105`)

Isso demonstra logicamente que turnos carregados em outra janela podem alterar o encerramento do dia; execução do contraexemplo **[nao verificado]**. (`app/ponto/turnos.py:1297-1300,1949`)

**Célula sem DNA: sem achado.** Tanto alimentação quanto carga individual chegam à mesma queda para `TipoEscala.marcos_do_dia`; `None`, `{}` ou ausência de `marcos` não inventam fim. (`app/escala/models.py:1094-1108`) A carga comum traz todos os campos, evitando DNA diferido. (`app/escala/alimentacao.py:41-46`)

**`escalas=`: sem achado nos chamadores atuais**, que não o fornecem; o lote carrega todos os vínculos com tipo relacionado. O contrato exige lista completa, pois geradora ausente devolve `None`. (`app/ponto/turnos.py:2114,2130-2131`; `app/escala/alimentacao.py:232-250`; chamadas citadas acima)

O teste de propriedade atual só compara lote sem alimentação com consulta individual; não cobre `turnos=`, `escalas=`, `motivos=True`, chaves do cartório nem o contraexemplo de janela. (`app/ponto/tests/test_c1_dias_encerrados.py:26-51`)

## 30/10 14:21 · sessao 9e672f61d1e5 · - · - · origem=Ronald (tmux)

1. **Sem achado no antigo teto HOJE/ONTEM:** consulta `data_ini..min(data_fim, hoje)`, incluindo D−2; o teste novo contempla encadeamento até D+2. Existe limite de 366 dias para a janela customizada, separado do julgamento de encerramento. (`app/colaboradores/services/calendario.py:296-298,707-712`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:47-55`)

2. **Ainda há “não encerrado” ignorado:** `sem_previsto` significa que a juíza não afirma encerramento; `dias_encerrados` retorna `False` para esse motivo. O calendário exclui explicitamente esse motivo de `_em_curso`, podendo chegar a `falta` em trabalho sem batida. Não é equivalência com a resposta booleana da juíza. (`app/ponto/turnos.py:2047-2050,2102-2105,2140`; `app/colaboradores/services/calendario.py:702-712,132-142,805`)

3. **A seleção dos três motivos é regra própria do leitor:** o ramo `sem_previsto` não virou encerrado na autoridade; o calendário lhe aplica o mesmo caminho aplicado a encerrados. Isso conflita com a exigência de leitor sem regra própria de classe e com a promessa do teste de que trabalho cujo turno “NAO encerrou” recebe `previsto`. A exceção está declarada no comentário local, mas declaração não altera a autoridade. (`CLAUDE.md:20-22`; `app/ponto/turnos.py:2102-2105`; `app/colaboradores/services/calendario.py:702-712`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:1-5`)

4. **Efeitos nos leitores:**

| Leitor | Efeito |
|---|---|
| Perfil/gaveta | Herdam mudança de status, ícone e tooltip; o tooltip em curso foi corrigido. (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/colaboradores/services/calendario.py:881-882`; `app/templates/colaboradores/partials/_calendario_grade.html:99-105`) |
| Furos HTML/PDF | Calendário/pintura mudam; seleção dos cards, `nfuro`, lista de marcos faltantes e ordenação continuam independentes, lendo `grade_da_celula → estado_do_dia`. (`app/relatorios/furos.py:141-190`; `app/relatorios/views.py:1549-1551`; `app/relatorios/furos_pdf.py:56-69`) |
| Resolver dia/DP | `previsto` não pede decisão. Datas passadas em curso podem perder ação; `sem_previsto` hoje/ontem pode percorrer o sentido inverso em relação ao delta anterior, se os demais filtros permitirem. (`app/colaboradores/services/calendario.py:190,198-199,895-896,925-926`; `app/templates/colaboradores/partials/_calendario_grade.html:125-126`) |
| Vigia | Perder a ação faz deixar `decisao_do_dia_pendente`; pode terminar em `fio_morto` ou outro balde subsequente. Ocorrência real **[nao verificado]**. (`app/chamados/services/vigia_fio.py:294-324`) |
| Rodapé | Totais `ok`, `faltas`, `escala_suspeita`, `previstos` acompanham status; previstos são contados, mas não aparecem no rodapé. (`app/colaboradores/services/calendario.py:930-946`; `app/templates/colaboradores/partials/_calendario_grade.html:137-173`) |
| `status_do_dia` / `contar_por_status` | Herdam integralmente a alteração, pois remontam o calendário. (`app/colaboradores/services/calendario.py:213-233`) |

5. **Alcance além de falta:** para datas antigas agora reconhecidas em curso, pode ocorrer `ok → previsto`, `escala_suspeita → previsto`, `escala_suspeita → sem_dados` no limbo, bloqueio de veredito e retirada do rótulo de escala suspeita. Para HOJE/ONTEM com `sem_previsto`, a exclusão nova permite os sentidos inversos em relação à volta anterior. A exclusão está declarada localmente; os demais efeitos não estão descritos na declaração da obra, que promete “previsto, nao falta”. (`app/colaboradores/services/calendario.py:132-152,177-178,697-703,805,894`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:1-5`)

Execução dos testes e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

## 30/10 14:20 · sessao 77b51e13bbae · - · - · origem=Ronald (tmux)

Custo novo do julgamento no calendário, por inspeção estática:

- **+1 query por calendário/card** quando `data_ini <= min(data_fim, hoje)`; **zero** quando a janela é inteiramente futura. A chamada passa `celulas` e `turnos`, mas omite `escalas`; `dias_encerrados` executa uma consulta com `select_related('tipo_escala')` para todos os vínculos. (`app/colaboradores/services/calendario.py:707-710`; `app/ponto/turnos.py:2129-2133`)
- Esse `+1` poderia alimentar dados já obtidos no calendário: `_esc_cal` foi carregado por `escalas_do_periodo(..., mais=_ger_cal)`, mas não é encaminhado à nova chamada. O parâmetro novo exige todos os vínculos; equivalência de reutilizar a lista filtrada precisa avaliação própria. (`app/colaboradores/services/calendario.py:404-406,432-434,709-710`; `app/ponto/turnos.py:2113-2114`)
- **Não há query por dia de `fim_turno` no caminho alimentado do calendário**, inclusive célula cujo `dna` é nulo: o carregador de células não usa `.only()`, o mapa fornece a instância completa e o método lê `cel.dna`; sem marcos, consulta o `tipo_escala` já trazido pelo `select_related`, em memória. (`app/escala/alimentacao.py:41-46`; `app/escala/models.py:1094-1108`; `app/ponto/turnos.py:2131,2138-2139`; `app/escala/servico_jornada.py:68-71,89-93`)
- **HTML: +N queries para N cards efetivamente montados**, sob a condição de janela acima. O calendário é chamado somente depois de `nfuro or not so_com_furo`. (`app/relatorios/furos.py:164-188`; `app/colaboradores/services/calendario.py:707-710`; `app/ponto/turnos.py:2130-2131`)
- **PDF: os mesmos +N no request, sem segunda cobrança do julgamento na geração do PDF**: os cards são montados antes da bifurcação HTML/PDF; o PDF recebe os cards e lê suas semanas. (`app/relatorios/views.py:1541,1548-1551,1560-1566`; `app/relatorios/furos_pdf.py:47-56,73-84`)
- A conta anterior `6 + E + M + U + K` não descreve a chamada atual do calendário: seus dois carregamentos caros estão bypassados pelos argumentos fornecidos e o DNA chega até o predicado. (`app/colaboradores/services/calendario.py:709-710`; `app/ponto/turnos.py:2129-2139`)

**Medição efetiva e comparação integral com a árvore de `d2c37edd`: [nao verificado].** O número `+1` descreve a nova cadeia identificável na cópia; a base completa não está acessível nesta árvore nem nos anexos fornecidos. **[nao verificado]**

## 30/10 14:12 · sessao ef4ba8f661a1 · CODEX-ASTRA-AB · verificador · origem=Code

**Veredito da cópia: REPROVADO.** Há reabertura automática que contorna o veto, e o selo não detecta esse caminho; a garantia de abrir cadastro para ativo sem vínculo também não está cumprida (`app/chamados/services/disputa_emissao.py:1606`; `app/chamados/tests/test_o261_veto_na_porta.py:212`; `app/ponto/management/commands/detectar_ausencias.py:139`).

Identidade do commit, comparação Git com `d2c37edd`, suíte Django, ocorrência dos contraexemplos em produção e execução da relavratura: **[nao verificado]**. A análise abaixo é da cópia disponível e dos anexos.

**1. Quem escreve, quem lê e efeito de `ec=None`**

O registro vivo nomeia `motivo_do_veto` como juiz; o corte contém a autorização exigida. O próprio `JUIZES.md` determina que o registro vivo prevalece (`app/core/juizes.py:248`; `app/docs/CORTES.md:401`; `app/docs/JUIZES.md:10`).

Os leitores de produção encontrados para o resultado de `motivo_do_veto` são:

| Leitor | Efeito quando `ec=None` passa a retornar `VETO_DEGENERADA` |
|---|---|
| Porta `_veto_da_regua` | Recusa nascimento por `abrir`/`criar` e renascimento por `_renascer_por_premissa_viva`, havendo dia forte. Sem dia, continua permitindo (`app/chamados/models.py:731`, `:778`, `:838`, `:891`, `:896`). |
| Cartório `_veto_do_dia` | Acrescenta o veto à impressão; pode desfazer a igualdade que dispensava rejulgamento. O resultado não é passado diretamente como veredito ao classificador (`app/ponto/services/cartorio.py:1013`, `:1083`, `:1092`, `:1108`). |
| `furos_sem_cobranca.medir` | Classifica o item como `porta='veto'`, retirando-o do total cobrável, mas mantendo-o em `itens`/`por_porta`. As exclusões anteriores — chamado vivo, tranca e afastamento — continuam prevalecendo (`app/ponto/services/furos_sem_cobranca.py:30`, `:83`, `:96`, `:109`). |
| C2 `reemitir_chamado_do_dia` | Recusa emissão/renascimento. Quando já há chamado vivo, deliberadamente não consulta esse veto e pode costurar nele (`app/chamados/reconciliador.py:807`, `:852`, `:854`, `:855`). |
| `furos_vetados_por_regua` | Inclui furo sem vínculo, com `vinculo=None` e `corroborada_em=None`; continua excluindo desligado (`app/ponto/services/furos_vetados.py:61`, `:65`, `:69`). |
| `chamados_vivos_em_dia_vetado` | Conta chamado vivo do universo com dia forte e vínculo ausente; não fecha nem modifica o chamado. Sem dia vai para contador separado (`app/ponto/services/furos_vetados.py:95`, `:101`, `:106`, `:110`). |

As cadeias de produção e consumo dos demais valores afetados são:

| Campo/valor | Quem fornece/escreve → quem lê |
|---|---|
| `VETO_DEGENERADA`, `VETO_REGUA`, `None` | `motivo_do_veto` compõe `regua_degenerada` e o veto de suspeita; não grava banco → leitores da tabela anterior (`app/ponto/services/furos_vetados.py:33`). |
| Universo de módulos | `FURO_MODULES` e `MODULOS_DE_TURNO` → união em `modulos_do_veto` → porta, vigia e teste dos cinco módulos (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/reconciliador.py:336`; `app/ponto/services/furos_vetados.py:79`; `app/chamados/models.py:889`; `app/chamados/tests/test_o261_veto_na_porta.py:383`). |
| Dia, `contexto_json`, `celula` | Emissores fornecem contexto; porta resolve a célula; `dia_do_fato(fortes=True)` prioriza célula e chaves do catálogo/contexto → porta e vigia (`app/chamados/models.py:718`, `:725`, `:830`; `app/chamados/juizes.py:89`, `:104`; `app/ponto/services/furos_vetados.py:101`). |
| Vínculo do dia, `escala_geradora`, vigência | Porta de célula grava geradora; carregador busca célula/vínculos e delega a `alimentacao.vinculo_do_dia` → `vinculo_do_emissor` → juiz. Não encontrei nova escolha paralela de vínculo nessa cadeia (`app/ponto/portas/celula.py:248`, `:260`; `app/escala/servico_jornada.py:46`, `:57`; `app/escala/alimentacao.py:196`; `app/ponto/services/furos_vetados.py:23`). |
| Template e avaliação da régua | `TipoEscala.marcos_do_dia` alimenta a degeneração; avaliação/corrobor ação gravam estado da régua → veto de suspeita lê avaliação, faixa e avaliações consecutivas (`app/escala/models.py:326`; `app/escala/regua_defesa.py:23`, `:26`, `:560`, `:644`, `:666`, `:765`; `app/colaboradores/services/vinculo.py:889`). |
| `status_local`, retorno de nascimento | Nascimentos continuam nos dois gates; transições continuam em `_gravar_estado` → dedup, emissores, C2 e vigia leem estado/retorno (`app/chamados/models.py:192`, `:670`, `:735`, `:817`; `app/chamados/reconciliador.py:885`, `:888`; `app/ponto/services/furos_vetados.py:95`). |
| Trilha: ação, modelo, objeto, `modulo_origem`, `dia`, `veto`, `via`, colaborador | `_trilha_do_veto` monta e consulta a chave; `core.observ.evento` escreve `LogAuditoria` → deduplicação e teste leem a trilha (`app/chamados/models.py:915`, `:921`, `:927`; `app/core/observ.py:134`; `app/chamados/tests/test_o261_veto_na_porta.py:359`). |
| Impressão, ata e veredito | `impressao_insumos` incorpora veto; cartório classifica e pede lavra; porta de célula grava → próxima passagem compara impressão (`app/ponto/services/cartorio.py:329`, `:1092`, `:1409`, `:1424`; `app/ponto/portas/celula.py:647`, `:708`). |
| Achados e contadores de veto | Serviços produzem dicionários → comando imprime; leitor de desfazer corroboração filtra pelo vínculo específico, descartando naturalmente `None` (`app/ponto/services/furos_vetados.py:69`, `:108`; `app/ponto/management/commands/furos_vetados_por_regua.py:29`, `:34`, `:44`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`). |
| `VETO_NA_PORTA` | Detector/grade devolvem texto → detector conta por chave, grade conta no plano e cartório conta veto, sem anunciar emissão (`app/ponto/management/commands/detectar_ausencias.py:733`, `:440`; `app/ponto/management/commands/reconciliar_grade.py:520`, `:254`; `app/ponto/services/cartorio.py:1497`). |
| `recusado_veto` | Retroativo consulta `veto_no_nascimento` no dry; no apply distingue retorno recusado; imprime total local (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`, `:174`, `:186`). |
| Fixtures: tipo, posto e vínculo | `dar_vinculo` cria/reutiliza template e posto e cria `EscalaColaborador` ativa desde 2020 → resolução normal de vínculo e juiz (`app/chamados/tests/_vinculo_do_dia.py:13`, `:19`, `:23`; `app/escala/servico_jornada.py:58`). |

Detector e grade ainda consultam os predicados anteriores antes da porta; C2 ainda consulta o novo juiz por fora. Isso está explicitamente permitido pela allowlist do pouso A, portanto **não classifico essas três permanências, isoladamente, como achado contra esse pouso** (`app/chamados/tests/test_o261_veto_na_porta.py:46`).

Há três achados nessa cadeia:

- **Impressão incompleta no ramo “nunca bateu”.** Esse ramo chama `impressao_insumos` sem `veto`, compara o hash e retorna antes do leitor do juiz. **Contraexemplo:** ativo sem adesão, célula carimbada, `ec=None` e demais insumos iguais: continua sendo pulado; o novo veto não entra em sua impressão. Esse ramo não emite cobrança por desenho; a falha é de cobertura da impressão, não prova de cobrança indevida (`app/ponto/services/cartorio.py:903`, `:907`, `:941`, `:943`, `:1003`, `:1013`).

- **Ato humano passa sem registrar o veto dispensado.** Desarquivar registra resgate, mas não consulta nem registra `VETO_DEGENERADA`; a trilha nova registra recusas. **Contraexemplo:** desarquivar chamado encerrado pela competência, com dia conhecido sem vínculo: há trilha de resgate, mas falta a informação de que o humano atravessou um veto. O próprio vigia documenta essa pendência (`app/chamados/views_arquivar.py:68`, `:75`, `:79`; `app/chamados/models.py:915`; `app/ponto/services/furos_vetados.py:90`).

- **Deduplicação da trilha não distingue a operação prometida.** A documentação promete uma linha por operação e grava `via`, mas o filtro não consulta `via`. **Contraexemplo:** `abrir` recusado e depois `criar` recusado para o mesmo colaborador/célula, módulo, dia e veto: a segunda operação fica sem linha própria (`app/chamados/models.py:908`, `:921`, `:927`).

**2. Fixtures migradas**

**Intenção “sem vínculo/sem escala” mascarada pelo helper: sem achado.** O caso explicitamente sem escala permanece sem ela: não chama o helper de chamado agrupado que passou a conceder vínculo. “Tranca sem cadastro” trata avisos de cadastro fora da tranca, não colaborador sem vínculo (`app/chamados/tests/test_selo_f4_fatia4.py:45`, `:69`, `:131`; `app/chamados/tests/test_tranca_sem_cadastro.py:9`).

Há vacuidades em testes tocados, **já presentes no corpo anterior mostrado pelo diff**, sem evidência de que tenham sido introduzidas pelo helper: [nao verificado]

| Teste | O que não prova e contraexemplo | [nao verificado]
|---|---|
| `test_MORDE_desligado_segue_o_gate_do_c2` | O desligado recebe vínculo, mas não recebe a célula criada para o outro colaborador. Remover o gate de situação ainda permite que a ausência de protesto impeça emissão; as asserções continuam satisfeitas (`app/chamados/tests/test_emissao_reata_orfa.py:31`, `:67`, `:71`, `:73`; `app/chamados/reconciliador.py:789`, `:834`, `:836`). |
| `test_sem_celula_o_chamado_ainda_nasce` | Só chama `_ctx_c2` e examina o dicionário. Uma porta que recusasse todos os nascimentos não seria observada pelo teste (`app/chamados/tests/test_emissor_nomeia_o_marco.py:88`, `:92`; `app/chamados/reconciliador.py:862`). |
| `test_sem_ata_possivel_o_chamado_nasce_sem_marco_e_nao_deixa_de_nascer` | Também só verifica o contexto. Recusar a abertura depois da montagem desse contexto preservaria suas asserções (`app/chamados/tests/test_bug125_cobranca_cega.py:176`, `:182`; `app/chamados/reconciliador.py:862`). |

O teste novo sem vínculo efetivamente pede abertura, espera `(None, False)` e compara com nascimento com vínculo; os testes de deduplicação também exigem chamado efetivo. Nesses controles: **sem achado** (`app/chamados/tests/test_o261_veto_na_porta.py:395`, `:404`, `:406`; `app/chamados/tests/test_3c_abrir_ausencia.py:52`; `app/chamados/tests/test_f3_dedup_atraso.py:57`).

Estado verde da suíte: **[nao verificado]**. [nao verificado]

**3. Quem abre `regua_degenerada` para ativo sem vínculo**

O escritor existe: `vetar_cobranca_se_degenerada` transforma `ec=None` em template ausente e abre `regua_degenerada`, com código `SEM_ESCALA`. Os chamadores são detector e grade (`app/escala/regua_defesa.py:23`, `:45`, `:50`, `:51`; `app/ponto/management/commands/detectar_ausencias.py:599`; `app/ponto/management/commands/reconciliar_grade.py:486`).

**Achado: existir o escritor não garante que o ativo sem vínculo o alcance.**

- Detector pula vínculo ausente antes do emissor (`app/ponto/management/commands/detectar_ausencias.py:139`).
- Cartório integral exige vínculo; parcial retorna sem ele (`app/ponto/services/cartorio.py:1347`, `:1446`).
- Retroativo de furo pula vínculo ausente (`app/ponto/management/commands/emitir_furo_retroativo.py:118`).
- Grade exclui template ausente da fila de emissão (`app/ponto/management/commands/reconciliar_grade.py:157`; `app/ponto/marcos.py:107`).
- Emissores de turno também saem sem vínculo (`app/ponto/management/commands/processar_alertas_avancados.py:37`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`).
- Juiz e nova porta somente recusam; não produzem o cadastro (`app/ponto/services/furos_vetados.py:40`; `app/chamados/models.py:896`).

**Contraexemplo:** ativo em operação, sem qualquer vínculo nem geradora de célula, sem aviso de cadastro anterior. Os fluxos acima saltam a emissão; chamar a nova porta com dia conhecido apenas veta. Não nasce `regua_degenerada` por esses caminhos. A garantia do aval não está cumprida na cópia; não atribuo a origem dessa lacuna ao diff (`app/ponto/management/commands/detectar_ausencias.py:139`; `app/ponto/services/cartorio.py:1348`; `app/chamados/models.py:896`).

**4. Há nascimento/reabertura automática que escapa?**

**Sim: duas rotas de reabertura.**

**A. Resposta pendente reabre diretamente.** O resolver chama `_pai.reabrir()` ao encontrar resposta aguardando validação. Ele é acionado automaticamente pelo signal e pelo reconciliador; não passa por `_renascer_por_premissa_viva` (`app/chamados/services/disputa_emissao.py:1598`, `:1604`, `:1606`; `app/chamados/signals.py:92`; `app/chamados/reconciliador.py:538`).

**Contraexemplo:** pai histórico `entrada_bloqueada_turno_aberto`, resolvido, dia conhecido sem vínculo, sem agrupamento/irmão vivo/encerramento administrativo, categoria sem classificação de furo; disputa aberta com resposta não validada nem materializada creditada ao pai. O juiz de respostas aceita o caso; `reabrir` grava estado vivo sem perguntar o veto. O módulo pertence ao universo, embora possa ficar fora da guarda específica de premissa de furo (`app/chamados/juizes.py:1471`; `app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/models.py:984`, `:1078`, `:1158`).

**B. A grade pode desfazer a recusa da porta.** `abrir` devolve o existente encerrado mesmo quando `_renascer_por_premissa_viva` recusa. A grade só trata `None`; recebendo encerrado, chama `reabrir()` diretamente (`app/chamados/models.py:686`, `:688`; `app/ponto/management/commands/reconciliar_grade.py:519`, `:521`, `:522`).

**Contraexemplo:** `batida_ausente` resolvido com marco ainda faltante; template pré-carregado era 08–17 e muda para 08–08 antes da abertura. O veto externo lê o objeto antigo; a porta recarrega o vínculo e veta; a grade recebe o encerrado e o reabre sem veto. A trava de `abrir` é no colaborador, não no template (`app/ponto/management/commands/reconciliar_grade.py:189`, `:196`, `:487`; `app/chamados/models.py:669`, `:778`, `:896`).

**O selo fica verde apesar dessas fugas.** A varredura pura em memória encontrou exatamente as duas escritas e três cópias permitidas, mas a função real do caminho A não apareceu. O selo reconhece operações de gerente/construtor; não acompanha a reabertura pelo receptor `_pai`. Portanto a prova atual não sustenta “toda reabertura automática passa pelo veto” (`app/chamados/tests/test_o261_veto_na_porta.py:40`, `:46`, `:207`, `:212`, `:216`; `app/chamados/services/disputa_emissao.py:1606`).

Nos demais critérios solicitados:

| Classe | Resultado |
|---|---|
| Segundo juiz novo / derivação paralela nova de vínculo ou universo | **Sem achado** na composição nova; usa autoridades existentes e união declarada (`app/ponto/services/furos_vetados.py:23`, `:33`, `:79`). |
| Segundo escritor novo de estado | **Sem achado**; as fugas usam o escritor existente por uma entrada que contorna a guarda (`app/chamados/models.py:192`, `:1158`). |
| Ordem de trava | **Sem achado de inversão introduzida**; permanece a trava de colaborador antes da leitura/dedup. Ensaio concorrente: **[nao verificado]** (`app/chamados/models.py:666`, `:669`). |
| Relógio novo fora da borda | **Sem achado no novo juiz/gate**: recebem ou derivam o dia do fato, sem consultar “agora” (`app/ponto/services/furos_vetados.py:27`; `app/chamados/models.py:891`). |
| Custo de frota no request | **Sem achado dessa classe**; há leitura adicional por dia na porta, pelo carregador de vínculo. Custo medido/latência: **[nao verificado]** (`app/chamados/models.py:896`; `app/escala/servico_jornada.py:46`, `:58`). |
| Pouso antes dos pré-requisitos financeiros | O novo hash pode provocar relavra, e o cron seleciona células passadas sem recorte de competência nessa seleção. Reversão anterior, DIFF financeiro, preservação de exportadas por hash e encaminhamento à Pauta DP: **[nao verificado]** (`app/ponto/services/cartorio.py:1083`, `:1092`, `:1409`; `app/ponto/management/commands/processar_cartorio.py:49`, `:61`). |

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

_505 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
