# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 14:59** · publicado em **30/10/2026 14:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 14:59 · sessao 46c312b8da3f · - · - · origem=Ronald (tmux)

1. **Aval e MORDE: sem achado no código.** Sem veredito, antes do fim previsto a juíza retorna falso; o calendário marca `futuro=True` e classifica trabalho sem batida como `previsto`. Depois do fim, sem turno aberto, retorna verdadeiro e alcança `falta` nas mesmas premissas dos testes. Referências: `app/ponto/turnos.py:2097–2105`; `app/colaboradores/services/calendario.py:707,802,132–142`. Os testes existentes cobrem hoje às 13h antes do turno 18h–06h e o dia seguinte às 11h: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23–26,42–45,61–63`. Execução: **[nao verificado]**.

2. **Chaves incompatíveis: pendência herdada persiste.** Cartório fornece `{(pk,data): célula}` a `dias_encerrados`, que repassa sem adaptação para `turnos_do_colab`; este trabalha com `{data: célula}` e recompõe `(_cid,_d)`, produzindo `(pk,(pk,data))`. A consulta de geradora espera `(pk,data)` e perde a célula nesse trecho. Referências: `app/ponto/services/cartorio.py:853`; `app/ponto/turnos.py:2131,1852,1880–1882,1928–1932`; `app/escala/alimentacao.py:237–254`. Ocorrência/impacto em produção: **[nao verificado]**.

3. **Dependência da janela: pendência herdada persiste.** Mapa DNA vazio vira `None`; mapa não vazio sem véspera retorna `None` em vez dos marcos do template. Logo, o alinhamento local de janelas não cura a diferença de alimentação entre chamadas largas e unitárias. Referências: `app/ponto/turnos.py:1949,1297–1300,1973–1976`; calendário alinhado em `app/colaboradores/services/calendario.py:310,705–706`. Contraexemplo executado e incidência: **[nao verificado]**.

4. **Tooltip: permanece impreciso.** “Turno em curso, ainda sem batida” continua tanto antes do início quanto em `sem_previsto`; o delta só protege o futuro por data, já excluído de `_em_curso`. Referências: `app/colaboradores/services/calendario.py:62,707,878–879`; `app/ponto/turnos.py:2102–2105`; o teste exige essa frase às 13h para turno que começa às 18h em `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:42–45`.

## 30/10 14:57 · sessao e70fd78a26cb · BENCH-CORRECAO · censo · origem=Code

Itens: **1** custo no request; **2** dois juízes de âncora; **3** dias reais/ação sobre vínculo errado; **4** RED restrito à primeira versão; **5** censo LEI-AKITA incompleto. (`app/docs/RELATO.md:19`)

Considerei 1, 2, 3 e 5 existentes nas três voltas, pela integração descrita desde a primeira e pelas autoridades já presentes na cópia. A retroação da declaração documental do item 5 não tem comprovação independente por hash. (`app/escala/services/cadastro_realidade.py:48`, `app/escala/services/fase_12x36.py:45`, `app/escala/services/fase_na_tela.py:50`, `app/docs/RELATO.md:23`; cronologia exata: [não verificado])

Para **4**, adoto **n/a na primeira volta**: a insuficiência específica surge diante das versões ampliadas, descritas na segunda volta. Nas duas seguintes, considero existente conforme o gabarito final; a ausência de outro RED intermediário permanece [não verificado]. (`app/docs/RELATO.md:19`, `app/docs/RELATO.md:22`)

As referências nas tabelas são as citadas pelas respostas dos anexos; sua correspondência às árvores históricas está [não verificado].

**Volta d6adf8c0**

| Item | A | B | Trecho que sustenta o crédito |
|---|---|---|---|
| 1 | VIU | VIU | A: “cada chamada da tela/PDF e dos dois comandos agora executa o detector global”. B: “Cada HTML/PDF e os commands C7/C8 passam a pagar a leitura de toda a frota”. Ambos descrevem o defeito. (`app/escala/services/cadastro_realidade.py:336`) |
| 2 | NAO_VIU | NAO_VIU | Nenhuma resposta identifica os dois juízes de âncora. [não verificado] |
| 3 | NAO_VIU | EM_PARTE | B: “C10 pode acusar um vínculo sob o cabeçalho de outro” e “ação sem identificar qual corrigir”. Identifica divergência entre vínculos ativos, sem diagnosticar o vínculo histórico dos dias e a regeneração do passado. (`app/escala/services/fase_12x36.py:114`, `app/escala/services/cadastro_realidade.py:488`; distinção: `app/docs/RELATO.md:18`) |
| 4 | n/a | n/a | Aplicabilidade temporal ressalvada acima. (`app/docs/RELATO.md:19`, `app/docs/RELATO.md:22`; cronologia: [não verificado]) |
| 5 | EM_PARTE | EM_PARTE | A: “ambos continuam selecionando explicitamente C7/C8”. B: “Os commands continuam exigindo especificamente C7/C8 para fechar”. Conferem consumidores, mas não apontam sua ausência na declaração LEI-AKITA. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`, `app/docs/RELATO.md:23`) |

**Volta 519fa2c2**

| Item | A | B | Trecho que sustenta o crédito |
|---|---|---|---|
| 1 | NAO_VIU | NAO_VIU | Não apontam o custo da varredura global no request. [não verificado] |
| 2 | NAO_VIU | EM_PARTE | B: “não identifiquei segundo juiz”. Toca a unicidade, mas não compara os dois calculadores de âncora. (`app/escala/services/cadastro_realidade.py:330`; autoridades: `app/escala/services/fase_12x36.py:45`, `app/escala/services/fase_na_tela.py:50`) |
| 3 | NAO_VIU | EM_PARTE | B: “presença dos quatro dias reais do col451: **[nao verificado]**”. Reconhece a prova pendente, sem identificar a ação necessária sobre o vínculo do passado. [não verificado] |
| 4 | EM_PARTE | EM_PARTE | A: “execução verde e RED histórico não foram verificados”. B: “Execução verde, RED histórico [...] **[nao verificado]**”. Não afirmam que o RED cobre somente a primeira versão. [não verificado] | [nao verificado]
| 5 | NAO_VIU | NAO_VIU | Não apontam a omissão dos leitores de `lista()` na declaração LEI-AKITA. [não verificado] |

**Volta af2cf55f**

| Item | A | B | Trecho que sustenta o crédito |
|---|---|---|---|
| 1 | NAO_VIU | NAO_VIU | Não apontam o custo no request. [não verificado] |
| 2 | EM_PARTE | NAO_VIU | A: “sem cálculo próprio de fase”. Examina a autoridade do adaptador, mas não confronta a sugestão do leitor com a da porta. (`app/escala/services/cadastro_realidade.py:331`; autoridades: `app/escala/services/fase_12x36.py:45`, `app/escala/services/fase_na_tela.py:50`) |
| 3 | EM_PARTE | NAO_VIU | A: “resultado real do col451: **[não verificado]**”. B discute serialização e vínculo iterado, sem examinar os dias reais ou o vínculo histórico. [não verificado] |
| 4 | EM_PARTE | NAO_VIU | A: “Execução da suíte, RED histórico [...] **[não verificado]**”. B registra somente “Execução dos testes: [não verificado]”, sem tratar a cobertura histórica do RED. [não verificado] | [nao verificado]
| 5 | NAO_VIU | EM_PARTE | B: “Trocar o pareamento alcançaria pauta, fila por causa, propositor e mensageria”. Faz censo do leitor de fase, mas não alcança os consumidores omitidos de `lista()`. (`app/escala/services/fase_12x36.py:223`, `app/chamados/services/fila_por_causa.py:173`, `app/docs/RELATO.md:23`) |

Não conto como achado novo o candidato de A/d6 sobre falha silenciosa, pois A declara “**Não fechei como achado**”. O conflito de vínculos de B/d6 recebeu crédito parcial no item 3, sem duplicação abaixo. (`app/escala/services/cadastro_realidade.py:603`, `app/escala/services/fase_12x36.py:114`)

Os registros NOVO reproduzem alegações fora do gabarito, inclusive dívidas anteriores; procedência histórica independente: [não verificado].

NOTA|O261-V-FASE_d6adf8c0|1|existia|A=VIU|B=VIU
NOTA|O261-V-FASE_d6adf8c0|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_d6adf8c0|3|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_d6adf8c0|4|n/a|A=n/a|B=n/a
NOTA|O261-V-FASE_d6adf8c0|5|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_519fa2c2|2|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|3|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|4|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-V-FASE_519fa2c2|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|2|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|3|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|4|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-V-FASE_af2cf55f|5|existia|A=NAO_VIU|B=EM_PARTE
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/services/cadastro_realidade.py:599|Falha do leitor C10 apenas gera log, sem indicador que distinga leitura incompleta de ausência de achados.
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:111|Teste aceita datas ISO erradas ou repetidas porque verifica formato e comprimento sem expectativa independente. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:128|Teste do PDF verifica apenas o prefixo PDF e permite omitir C10 e suas datas. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/tests/test_o261v_fase_na_lista.py:170|Mock vazio continuaria verde sem a integração, contrariando a afirmação de que ambos os casos ficariam vermelhos. [nao verificado]
NOVO|O261-V-FASE_d6adf8c0|B|app/escala/services/fase_12x36.py:146|Dívida anterior: leitor pareia batidas cruas sem o papel da ata, em vez de consumir turnos_do_colab.
NOVO|O261-V-FASE_519fa2c2|A|app/escala/services/cadastro_realidade.py:112|C10 anuncia apenas 12x36, embora incorpore também achados 24x48.
NOVO|O261-V-FASE_519fa2c2|A|app/escala/services/fase_12x36.py:175|Dívida anterior: parear_turnos é chamado sem papel_por_minuto e não incorpora a autoridade da ata.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/cadastro_realidade.py:112|Descrição e contador C10 anunciam 12x36 para um universo que também inclui 24x48.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/cadastro_realidade.py:602|Permanece a falha silenciosa de C10, compartilhada com outros leitores, sem indicador de leitura incompleta.
NOVO|O261-V-FASE_519fa2c2|B|app/escala/services/fase_12x36.py:175|Dívida anterior: leitor consulta batidas e chama diretamente o pareador, em vez de turnos_do_colab.
NOVO|O261-V-FASE_af2cf55f|A|app/escala/services/fase_12x36.py:175|Dívida anterior da ata permanece no pareamento do leitor; o inventário aponta migração para turnos_do_colab.
NOVO|O261-V-FASE_af2cf55f|B|app/escala/services/fase_12x36.py:146|Dívida anterior: parear_turnos sem papel_por_minuto não recebe o papel da ata carregado por turnos_do_colab.

## 30/10 14:56 · sessao 4e3077f0a328 · BENCH-CORRECAO · censo · origem=Code

A cobertura textual pode ser corrigida; a anterioridade de todos os itens não pode ser fechada com o material disponível. A cópia ainda retorna `None` para vínculo ausente e tem `reabrir` sem `humano` (`app/ponto/services/furos_vetados.py:26`; `app/chamados/models.py:1023`). O conteúdo integral dos quatro hashes está [nao verificado].

Por isso, uso **[nao verificado]** onde falta comprovar existência naquela volta. Não substituo desconhecimento por `n/a`. Nessas linhas, A/B representa cobertura textual, condicionada à existência do item. Essa é a única extensão ao formato solicitado [nao verificado].

As classificações **EM_PARTE** são:

- **1c565c8f, item 2, B:** identifica “desfazer corroboracão filtra pelo vínculo”, mas não denuncia a linha LEI-AKITA desatualizada (`app/escala/management/commands/desfazer_corroboracao_regua.py:38`, referência citada pela resposta).
- **1c565c8f, item 3, B:** escreve “Acrescenta `veto:VETO_DEGENERADA` à impressão”, sem apontar falta de teste próprio ou mutante sobrevivente (`app/ponto/services/cartorio.py:1013`; `app/ponto/services/cartorio.py:1083`, referências citadas pela resposta).
- **1c565c8f, item 6, B:** declara “reversão prévia e hashes do gravado exportado: [nao verificado]” e menciona `furos_sem_cobranca.medir`, sem conferir o conjunto das condições, a divergência 63/1.576 ou a medição antes/depois [nao verificado].
- **3b68a95b, item 7, A:** examina “os três `humano=True`” e afirma que “a pergunta e a nota [...] já constam”, sem apontar ausência de exibição/gravação do veto ultrapassado (`app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/acoes_disputa.py:270`, referências citadas pela resposta).
- **3b68a95b, item 7, B:** examina autorização, notas e trilha, concluindo “sem achado [...] quanto a novo chamado, nota ou pergunta”; não identifica o defeito exigido pelo aval (`app/chamados/models.py:1143`; `app/chamados/services/acoes_disputa.py:270`, referências citadas pela resposta; exigência em `app/docs/BACKLOG.md:785`).

Em **a3bbef39, item 4**, ambas descrevem o helper já alterado para SEMANAL 0..6; o defeito específico de usar 5x2 com folga no fim de semana recebe `n/a` (`app/chamados/tests/_vinculo_do_dia.py:18`, referência de ambas as respostas; árvore desse hash [nao verificado]).

O gabarito situa diretamente **s1 e s2 em 1c565c8f** e **s3 em 3b68a95b**; sua permanência ou correção nas demais voltas está [nao verificado]. Não encontrei identificação textual dos demais defeitos conhecidos; a cobertura indicada abaixo é [nao verificado] quanto às árvores históricas.

NOTA|O261-VETO-A2_673ecaf7|1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|3|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|4|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|5|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|6|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|7|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|s1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|s2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_673ecaf7|s3|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|3|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|4|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|5|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|6|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|7|[nao verificado]|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A2_3b68a95b|s1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|s2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_3b68a95b|s3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|3|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|4|n/a|A=n/a|B=n/a
NOTA|O261-VETO-A2_a3bbef39|5|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|6|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|7|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|s1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|s2|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_a3bbef39|s3|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|1|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|2|[nao verificado]|A=NAO_VIU|B=EM_PARTE
NOTA|O261-VETO-A2_1c565c8f|3|[nao verificado]|A=NAO_VIU|B=EM_PARTE
NOTA|O261-VETO-A2_1c565c8f|4|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|5|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|6|[nao verificado]|A=NAO_VIU|B=EM_PARTE
NOTA|O261-VETO-A2_1c565c8f|7|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|s1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|s2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A2_1c565c8f|s3|[nao verificado]|A=NAO_VIU|B=NAO_VIU
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/tests/test_o261_veto_na_porta.py:251|Selo reconhece somente a sintaxe .reabrir e agrega por função, ocultando chamadas adicionais em função já classificada.
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/services/acoes_chamado.py:313|Transição resolvido para em_analise permite reabertura sem passar por .reabrir nem pelo veto.
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/tests/test_o261_veto_na_porta.py:256|Chamadas por alias ou getattr escapam do detector AST.
NOVO|O261-VETO-A2_673ecaf7|A|app/chamados/tests/test_o261_veto_na_porta.py:168|Lambda imediata contendo reabrir escapa do censo.
NOVO|O261-VETO-A2_673ecaf7|A|app/ponto/services/fechamento.py:1415|Chamador por alias importado de função que reabre não aparece no censo.
NOVO|O261-VETO-A2_673ecaf7|B|app/chamados/models.py:923|Deduplicação histórica da trilha não registra nova recusa após correção e retorno do mesmo veto em outro ciclo.
NOVO|O261-VETO-A2_3b68a95b|B|app/chamados/models.py:938|Recusa via reabrir recebe descrição de nascimento recusado.
NOVO|O261-VETO-A2_1c565c8f|A|app/chamados/tests/test_o261_veto_na_porta.py:207|Selo não detecta a reabertura automática por _pai.reabrir no HX.
NOVO|O261-VETO-A2_1c565c8f|B|app/colaboradores/queries.py:64|ATIVO sem vínculo não alcança o escritor de cadastro pelos fluxos normais examinados.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/tests/test_emissor_nomeia_o_marco.py:88|Teste promete nascimento, mas verifica somente contexto retornado por _ctx_c2. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/tests/test_bug125_cobranca_cega.py:176|Teste promete nascimento, mas verifica somente contexto retornado por _ctx_c2. [nao verificado]
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/services/disputa_emissao.py:1606|HX reabre automaticamente o pai por reabrir sem consultar o novo veto.
NOVO|O261-VETO-A2_1c565c8f|B|app/ponto/management/commands/reconciliar_grade.py:522|Grade pode reabrir o existente encerrado devolvido após recusa do renascimento.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/tests/test_o261_veto_na_porta.py:207|Varredor e MORDE não cobrem vivificação por pai.reabrir.
NOVO|O261-VETO-A2_1c565c8f|B|app/ponto/services/cartorio.py:941|Ramo nunca_bateu calcula e compara impressão sem o novo veto, retornando antes do leitor do juiz.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/models.py:921|Deduplicação da trilha omite via e pode suprimir a recusa de outra operação.
NOVO|O261-VETO-A2_1c565c8f|B|app/chamados/models.py:921|Deduplicação consulta toda a história e pode suprimir recusa de novo ciclo após correção da régua.
NOVO|O261-VETO-A2_1c565c8f|B|bin/placar_code.sh:209|Placar captura apenas o contador antigo e não mostra chamados_vivos_em_dia_vetado.
NOVO|O261-VETO-A2_1c565c8f|B|app/ponto/management/commands/processar_cartorio.py:49|Comando não restringe relavratura à competência aberta e pode alcançar células históricas.

## 30/10 14:56 · sessao cd7b5c1f6500 · BENCH-CORRECAO · censo · origem=Code

Itens 1–6 seguem a ordem do gabarito (`app/docs/RELATO.md:13`). A existência foi reconstruída pelos anexos e pelo histórico descrito em `app/docs/RELATO.md:24`; a conferência integral das árvores de cada hash está [não verificado]. As marcações posteriores por continuidade são inferências, sobretudo para os itens 3 e 5 [não verificado].

O item 6 recebe **n/a na primeira volta**: ali o cabeçalho ainda usa `data_evento`, sem recalcular o dia pela célula; `dia_mostrado` aparece na segunda volta (`app/templates/chamados/partials/_card_atendimento.html:90`, referências das respectivas respostas).

Trechos que sustentam VIU ou EM_PARTE:

- **469d61ba, A, item 3 — EM_PARTE:** “simula aplicar” e “não comprova que a ação real […] corrigirá para D+1”; discute cobertura, sem identificar o TypeError do RED (`app/chamados/tests/test_corrigir_pergunta_cega.py:54`).
- **469d61ba, A, item 4 — EM_PARTE:** “duas queries novas por alvo, inclusive tentativa deduplicada”; identifica custo na emissão, sem alcançar o plantio dentro da classificação da fila (`app/chamados/catalogo/query.py:100`).
- **469d61ba, B, item 2 — VIU:** “O texto lê `lampadas[].instante`; o plantio continua recompondo marcos […] dentro de um envelope” (`app/chamados/catalogo/query.py:65`; `app/chamados/juizes.py:305`).
- **469d61ba, B, itens 3 e 5 — EM_PARTE:** “não exercitam intervalos, os seis emissores, telas, identidade persistida ou teto de consultas”; aponta lacunas, sem identificar o TypeError nem a mudança de classe do intervalo diurno (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65`).
- **469d61ba, B, item 4 — VIU:** “Antes de procurar qualquer conflito, `classe_do_lote` calcula o plantio” e “Esse trabalho acontece por pergunta da fila”; descrição inequívoca do custo específico (`app/chamados/services/validacao.py:830`; `app/chamados/services/materializacao.py:354`).
- **24f1cd69, A, item 6 — EM_PARTE:** “a tela pode exibir D+1 e pedir confirmação de D”; aponta divergência de apresentação, sem regeneração da célula (`app/templates/chamados/partials/_card_atendimento.html:164`).
- **31b8cec0, A, item 4 — EM_PARTE:** “Em cobrança/aviso, esse carregamento por pergunta já acontece no juiz”; trata de outro custo por pergunta (`app/chamados/services/aviso_pendencia.py:9`; `app/chamados/juizes.py:791`).
- **31b8cec0, A, item 6 — EM_PARTE:** “`dia_mostrado` ainda pode contradizer o texto gravado”; o exemplo depende de contexto ISO com hora, sem regeneração (`app/chamados/catalogo/query.py:99`; `app/chamados/models.py:2176`).
- **691508dd, A e B, item 6 — EM_PARTE:** A: “O ponto final quebra a igualdade […] e aciona o fallback”; B: “acrescentar apenas `.` faz retornar 21/10”. Identificam outra causa de divergência entre cabeçalho e texto (`app/chamados/models.py:2178`).

Os NOVO abaixo registram as alegações dos revisores com suas referências históricas; sua procedência integral nos respectivos hashes está [não verificado].

NOTA|O261-NOTURNO_469d61ba|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_469d61ba|2|existia|A=NAO_VIU|B=VIU
NOTA|O261-NOTURNO_469d61ba|3|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-NOTURNO_469d61ba|4|existia|A=EM_PARTE|B=VIU
NOTA|O261-NOTURNO_469d61ba|5|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-NOTURNO_469d61ba|6|n/a|A=n/a|B=n/a
NOTA|O261-NOTURNO_24f1cd69|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_24f1cd69|6|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|4|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_31b8cec0|6|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_691508dd|6|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-NOTURNO_e8c314e0|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_e8c314e0|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|4|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-NOTURNO_06d7ad06|6|existia|A=NAO_VIU|B=NAO_VIU
NOVO|O261-NOTURNO_469d61ba|A|app/templates/chamados/partials/_card_atendimento.html:90|Cabeçalho conserva D enquanto o texto novo da pergunta mostra D+1.
NOVO|O261-NOTURNO_469d61ba|A|app/templates/chamados/partials/modal_fio.html:157|Modal apresenta texto D+1 e referência D sem detectar a divergência textual.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/services/fabrica_da_celula.py:118|Duas gerações por alvo consultam a célula antes do dedup, inclusive na tentativa duplicada.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/management/commands/disparar_perguntas_competencia.py:104|Disparador descarta a célula já carregada e o produtor consulta a mesma célula novamente.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/management/commands/auditar_disputas.py:188|Auditoria não compara o dia textual com a identidade e o instante do marco.
NOVO|O261-NOTURNO_469d61ba|A|app/chamados/tests/test_corrigir_pergunta_cega.py:54|Teste de correção simula gravação sem exercitar a ação real nem o caso noturno. [nao verificado]
NOVO|O261-NOTURNO_469d61ba|B|app/templates/chamados/partials/_card_atendimento.html:90|Cabeçalho e confirmação anunciam D enquanto o texto da pergunta mostra D+1.
NOVO|O261-NOTURNO_469d61ba|B|app/templates/chamados/partials/modal_fio.html:180|Modal mantém referência D e seu detector ignora a divergência com o texto D+1.
NOVO|O261-NOTURNO_469d61ba|B|app/api/views.py:1168|API entrega rótulo D junto da pergunta com D+1.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/catalogo/query.py:95|Corte nos primeiros dez caracteres aceita hora ou sufixo e altera a normalização anterior.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/catalogo/query.py:100|Motivo desconhecido com padrão datado consulta célula apesar de não possuir marco.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/services/fabrica_da_celula.py:122|Duas chamadas de geração acrescentam consultas à célula por alvo datado antes da deduplicação.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/services/materializacao.py:404|DNA inválido pode propagar exceção e interromper a classificação do lote.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:128|Selo não cobre outras formas de interpolação nem produtores fora de chamados.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/management/commands/auditar_disputas.py:191|Auditoria verifica existência esperada sem certificar o dia do texto já gravado.
NOVO|O261-NOTURNO_469d61ba|B|app/chamados/services/fio.py:736|Ação enviada pelo botão de correção não possui tratamento e cai em Nada a fazer.
NOVO|O261-NOTURNO_24f1cd69|A|app/templates/chamados/partials/_card_atendimento.html:164|Escape confirma data_evento D apesar de o cabeçalho dia_mostrado apresentar D+1.
NOVO|O261-NOTURNO_24f1cd69|A|app/chamados/services/aviso_pendencia.py:20|Aviso de questionário continua usando data_evento sem considerar dia_mostrado.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154|Detector acusa prosa com marcador data em argumento nomeado de exceção.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:147|Detector deixa passar format com expansão de dicionário contendo a chave data.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:156|Detector deixa passar marcador data sob JoinedStr usado com operador percentual.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154|Detector acusa concatenação de prosa por aceitar qualquer BinOp com o marcador.
NOVO|O261-NOTURNO_24f1cd69|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:171|Varredura restrita a chamados não garante ausência global de produtores.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/catalogo/query.py:99|Contexto ISO com hora permanece cru no texto enquanto dia_mostrado pode retornar o dia seguinte do DNA.
NOVO|O261-NOTURNO_31b8cec0|A|app/api/views.py:1043|API carrega chamado_destino__celula, mas a propriedade acessa p.celula e pode acrescentar um SELECT por pergunta.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/models.py:2162|Propriedade sem cache recalcula dia_mostrado no cabeçalho e em cada opção do card.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:155|Detector ignora substituição por variável e literal usado como argumento pergunta.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:137|Exclusão de models.py inteiro e perímetro limitado a chamados deixam produtores sem fiscalização.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150|Detector acusa format com expansão sem data e format_map de outro campo.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:158|Detector acusa concatenação de prosa porque aceita qualquer BinOp com marcador data.
NOVO|O261-NOTURNO_31b8cec0|B|app/templates/chamados/meu_atendimento.html:19|Confirmação usa p e opt sem definição no laço reg, deixando o texto sem rótulo e dia.
NOVO|O261-NOTURNO_691508dd|A|app/chamados/models.py:2178|Texto com ponto final ou data ISO com hora pode acionar fallback para a identidade e contradizer o dia textual.
NOVO|O261-NOTURNO_691508dd|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168|Detector ignora format com desempacotamento interno ou chave dinâmica que pode fornecer data.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/models.py:2178|Texto com ponto final ou data ISO com hora pode acionar fallback para a identidade e contradizer o dia textual.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168|Detector ignora format com desempacotamento interno que pode fornecer data.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:156|Literal com marcador data dentro de outro dicionário passa, permitindo copiar o espelho fora da isenção nominal.
NOVO|O261-NOTURNO_e8c314e0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170|Detector acusa formatação de nome com expansão interna de dicionário mesmo sem campo data.
NOVO|O261-NOTURNO_e8c314e0|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:172|Detector acusa formatação de nome com expansão interna de dicionário mesmo sem campo data.
NOVO|O261-NOTURNO_06d7ad06|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166|Detector deixa passar campos data válidos com conversão ou especificador, como {data!s} e {data:}.
NOVO|O261-NOTURNO_06d7ad06|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166|Detector deixa passar campos data válidos com conversão ou especificador, como {data!s} e {data:%d/%m/%Y}.

## 30/10 14:55 · sessao 676405590b37 · BENCH-CORRECAO · censo · origem=Code

Numeração: **1** LEI-AKITA; **2** Pauta DP; **3** custo da cerca; **4** guarda morta; **5** R1 sem exigir renascimento; **6** composição duplicada; **7** ordem da docstring; **8** emissão apesar do veto da régua. (`app/docs/RELATO.md:83`; `app/docs/RELATO.md:85`; `app/docs/RELATO.md:87`)

Considerei os itens 1, 2, 3, 5 e 6 existentes nas três primeiras voltas por continuidade até a reprovação de `df863705`; confirmação individual das árvores históricas: **[nao verificado]**. Os seis estavam curados em `22c08b8e`. (`app/docs/RELATO.md:81`; `app/docs/RELATO.md:83`; `app/docs/RELATO.md:84`)

Não retroagi a guarda morta: nas respostas de `68a38548` e `0ed50cc2`, a segunda chamada ainda alcança a relavratura, indicando ausência da proteção posteriormente removida. Marquei o item 4 como `n/a` nas primeiras voltas por essa inferência; introdução exata: **[nao verificado]**. (Referências das respostas: `app/chamados/reconciliador.py:862`, `app/chamados/reconciliador.py:871`; `app/chamados/services/fio.py:797`.)

O item 7 pertence à alteração posterior de `3ab94852`. O item 8 já existia com a ampliação do c2 e foi corrigido em `13963efb`; vínculo ausente e corrida entre leituras são achados distintos. (`app/docs/RELATO.md:86`; `app/docs/RELATO.md:87`; `app/chamados/reconciliador.py:807`; `app/chamados/reconciliador.py:852`)

Créditos **VIU/EM_PARTE**, com trechos das respostas e suas referências históricas:

| Volta/revisor | Item/nota | Trecho |
|---|---|---|
| 4da2943d/A | 3 — VIU | “Custo novo por pergunta na pilha síncrona; consulta repetida”; “O C6 ainda foi antecipado para antes dos gates baratos”. (`app/chamados/reconciliador.py:1436`; `app/chamados/reconciliador.py:1438`) |
| 4da2943d/B | 3 — EM_PARTE | “esse cadastro não exercita a nova seleção HIST da órfã”. Aponta a cobertura insuficiente, sem identificar a cerca cara antes dos vetos do c2. (`app/ponto/tests/test_selo_chokepoint_escrita.py:226`; `app/ponto/tests/test_selo_chokepoint_escrita.py:258`) |
| 4da2943d/B | 5 — EM_PARTE | “R1 exige destino vivo ou recusa”; “Não identifiquei vacuidade equivalente”. Examina a asserção permissiva, sem denunciar que ela dispensa o renascimento. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117`) |
| 68a38548/A | 3 — EM_PARTE | “cenário FICAM cria chamados vivos, sem cobrir o ramo novo com encerrados”. Identifica limite de cobertura, sem apontar o custo da cerca no c2. (`app/ponto/tests/test_selo_chokepoint_escrita.py:164`; `app/ponto/tests/test_selo_chokepoint_escrita.py:364`) |

Os registros `NOVO` transcrevem os achados adicionais apresentados pelos revisores; procedência definitiva e reprodução histórica: **[nao verificado]**.

NOTA|O261-C_4da2943d|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_4da2943d|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_4da2943d|3|existia|A=VIU|B=EM_PARTE
NOTA|O261-C_4da2943d|4|n/a|A=n/a|B=n/a
NOTA|O261-C_4da2943d|5|existia|A=NAO_VIU|B=EM_PARTE
NOTA|O261-C_4da2943d|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_4da2943d|7|n/a|A=n/a|B=n/a
NOTA|O261-C_4da2943d|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|3|existia|A=EM_PARTE|B=NAO_VIU
NOTA|O261-C_68a38548|4|n/a|A=n/a|B=n/a
NOTA|O261-C_68a38548|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_68a38548|7|n/a|A=n/a|B=n/a
NOTA|O261-C_68a38548|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|2|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|4|n/a|A=n/a|B=n/a
NOTA|O261-C_0ed50cc2|5|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|6|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_0ed50cc2|7|n/a|A=n/a|B=n/a
NOTA|O261-C_0ed50cc2|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_22c08b8e|1|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|2|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|3|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|4|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|5|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|6|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|7|n/a|A=n/a|B=n/a
NOTA|O261-C_22c08b8e|8|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-C_13963efb|1|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|2|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|3|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|4|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|5|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|6|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|7|n/a|A=n/a|B=n/a
NOTA|O261-C_13963efb|8|n/a|A=n/a|B=n/a
NOVO|O261-C_4da2943d|A|app/chamados/services/disputa_emissao.py:1634|HIST ampliado permite carimbar pergunta de dia trancado pelo ramo posterior do S128, que não repete a proteção da tranca.
NOVO|O261-C_4da2943d|A|app/chamados/services/disputa_emissao.py:1634|Ramo posterior do S128 carimba resposta MORTA sem repetir a proteção de resposta sem batida real, inclusive com pai ausente.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:92|Fixture correlaciona vitalidade com validada_em e não distingue substituir C6 por um juiz desse campo.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:95|Bateria não chama diretamente reemitir_chamado_do_dia e permite implementar a cura apenas no wrapper.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:66|Fixture grava ii/fi enquanto o juiz consome hii/hfi, sem exercitar a lâmpada específica do intervalo.
NOVO|O261-C_68a38548|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:235|Teste exige igualdade integral após segunda chamada direta, embora a relavratura renove julgado_em. [nao verificado]
NOVO|O261-C_0ed50cc2|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264|Teste de idempotência compara todas as colunas apesar da renovação de julgado_em na segunda chamada. [nao verificado]
NOVO|O261-C_0ed50cc2|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264|Comparação integral falha com relógio avançando e pode ocultar a relavratura com relógio congelado.
NOVO|O261-C_22c08b8e|A|app/chamados/reconciliador.py:830|Veto de célula chama logger antes da cerca, contrariando a justificativa de conjunções sem efeito colateral.
NOVO|O261-C_22c08b8e|B|app/chamados/reconciliador.py:830|Reordenamento permite chamada adicional ao logger antes da cerca, contrariando a justificativa de ausência de efeitos colaterais.
NOVO|O261-C_13963efb|A|app/ponto/services/furos_vetados.py:26|Vínculo ausente retorna ausência de veto no leitor usado pelo c2, enquanto a emissão impede esse caso.
NOVO|O261-C_13963efb|A|app/chamados/reconciliador.py:843|Veto acrescentado também muda motivos C1 fora do alcance novo; autorização específica dessa ampliação [nao verificado].
NOVO|O261-C_13963efb|B|app/chamados/reconciliador.py:798|Dispensa do veto usa leitura anterior à trava; abrir pode reler chamado já encerrado e renascê-lo sem consultar a régua.

## 30/10 14:55 · sessao cf5aace19adf · BENCH-CORRECAO · censo · origem=Code

A existência nas voltas antigas foi reconstruída pelos anexos: a confirmação por histórico Git é **[nao verificado]**. Para ESTADO e SELO nas duas primeiras voltas, adoto **existia** por continuidade do leitor e do teste preexistentes; a confirmação dessas versões é **[nao verificado]** (`bin/fio_estado.py:82`; `app/chamados/tests/test_o261_vigia_fio.py:755`).

**LAVRA-VELHA fica n/a nas três primeiras voltas:** o defeito específico é consumir o dono lavrado sem conferir sua idade (`app/docs/BACKLOG.md:779`). Os anexos anteriores descrevem consulta ao dono atual; a mudança para leitura do dono lavrado está registrada em `app/docs/RELATO.md:8` e implementada em `app/chamados/services/vigia_fio.py:266`.

As evidências para VIU e EM_PARTE são:

| Volta | Item/revisor | Classificação e trecho |
|---|---|---|
| d75bd454 | SELO / A | **EM_PARTE:** “possibilidade de datas incoerentes ao atravessar meia-noite”. Discute o relógio do comando, mas não aponta a falta do cenário no selo (`app/chamados/management/commands/vigiar_fio_morto.py:154`, conforme resposta). |
| 8110a931 | ESTADO / A | **VIU:** “o destino `fio_do_admin` fica fora da projeção e da frase do painel” (`bin/fio_estado.py:82`, `:101`). |
| 8110a931 | ESTADO / B | **VIU:** “Não mostra os três campos novos” (`bin/fio_estado.py:82`, `:101`). |
| 8110a931 | SELO / A | **VIU:** “não fabrica travessia da meia-noite” (`app/chamados/tests/test_o261_vigia_fio.py:801`, conforme resposta). |
| 8110a931 | SELO / B | **EM_PARTE:** “O teste do carimbo chama novamente o censo sem `agora`; não prova propagação do instante”. Aponta insuficiência temporal do teste, sem identificar expressamente o cenário da virada (`app/chamados/tests/test_o261_vigia_fio.py:787`, `:801`, conforme resposta). |
| c3b91265 | LAVRA-VELHA / A | **EM_PARTE:** “o vigia compara o valor lavrado”. Reconhece a fonte, mas não aponta falta de controle de idade (`app/chamados/services/vigia_fio.py:267`, conforme resposta; leitura na cópia em `:266`). |
| c3b91265 | ESTADO / B | **VIU:** “não aparecem na seleção do ESTADO” e “lacuna de visibilidade verificável” (`bin/fio_estado.py:82`, `:101`). |
| c3b91265 | SELO / B | **VIU:** “não força travessia da meia-noite” (`app/chamados/tests/test_o261_vigia_fio.py:793`). |

Nos demais pares classificados como NAO_VIU, não identifiquei passagem que apontasse o defeito específico **[nao verificado]**. Em particular, o achado de A em c3b91265 sobre relógios de C2/C4 trata de outro caminho, sem apontar o selo do carimbo (`app/escala/services/cadastro_realidade.py:186`, `:350`). Os registros NOVO abaixo reproduzem os achados atribuídos às respostas; preservam suas referências históricas, cuja correspondência com cada árvore é **[nao verificado]**.

NOTA|FIO-MORTO-DONO-NA-LISTA_d75bd454|FIO-DO-ADMIN-NO-ESTADO|existia|A=NAO_VIU|B=NAO_VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_d75bd454|FIO-DO-ADMIN-LAVRA-VELHA|n/a|A=n/a|B=n/a
NOTA|FIO-MORTO-DONO-NA-LISTA_d75bd454|SELO-CARIMBO-VIRADA|existia|A=EM_PARTE|B=NAO_VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|FIO-DO-ADMIN-NO-ESTADO|existia|A=NAO_VIU|B=NAO_VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|FIO-DO-ADMIN-LAVRA-VELHA|n/a|A=n/a|B=n/a
NOTA|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|SELO-CARIMBO-VIRADA|existia|A=NAO_VIU|B=NAO_VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_8110a931|FIO-DO-ADMIN-NO-ESTADO|existia|A=VIU|B=VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_8110a931|FIO-DO-ADMIN-LAVRA-VELHA|n/a|A=n/a|B=n/a
NOTA|FIO-MORTO-DONO-NA-LISTA_8110a931|SELO-CARIMBO-VIRADA|existia|A=VIU|B=EM_PARTE
NOTA|FIO-MORTO-DONO-NA-LISTA_c3b91265|FIO-DO-ADMIN-NO-ESTADO|existia|A=NAO_VIU|B=VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_c3b91265|FIO-DO-ADMIN-LAVRA-VELHA|existia|A=EM_PARTE|B=NAO_VIU
NOTA|FIO-MORTO-DONO-NA-LISTA_c3b91265|SELO-CARIMBO-VIRADA|existia|A=NAO_VIU|B=VIU
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/services/vigia_fio.py:253|Uma candidata dispara lista inteira e possíveis cálculos do espelho, contrariando a descrição de nenhum recálculo.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/services/vigia_fio.py:260|Falhas não são memorizadas nos caches da lista e do carregador, repetindo trabalho e exceções por célula.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/escala/services/cadastro_realidade.py:565|Falha interna de C9 produz lista parcial sem incrementar fio_do_admin_sem_resposta.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/management/commands/vigiar_fio_morto.py:154|Comando, censo e C2 capturam relógios independentes, permitindo datas incoerentes na mesma rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|A|app/chamados/tests/test_o261_vigia_fio.py:548|Mocks ignoram janela e instante e não exercitam integração real, custo ou exceções internas da lista.
NOVO|FIO-MORTO-DONO-NA-LISTA_d75bd454|B|app/folha/fora_da_folha.py:204|Observação de apresentação: folha recomenda resolver o chamado sem apontar Cadastro x Realidade; quebra não comprovada.
NOVO|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|A|app/chamados/tests/test_o261_vigia_fio.py:606|Teste não inclui segunda empresa visível fora do recorte e continua passando sem a interseção. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_3e49c1bc|B|app/chamados/tests/test_o261_vigia_fio.py:611|Expectativas do teste também passam sem cruzar empresas visíveis com o recorte solicitado. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:667|Projeção reconhece somente data estruturada de C9 e omite dias apresentados no detalhe de C4/C5.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/services/vigia_fio.py:275|Vigia usa dono atual enquanto a lista mostra dono lavrado, admitindo transferência sem porta humana correspondente na C9.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:187|C2 fixa corte de competência em 21 em vez de consultar a autoridade do corte por empresa.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/services/vigia_fio.py:181|Defaults independentes do censo e novas leituras em C2, C4 e calendário impedem congelar integralmente a rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/escala/services/cadastro_realidade.py:565|Exceção interna de C9 e ausência de snapshot não chegam ao contador externo de sem resposta.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/tests/test_o261_vigia_fio.py:655|Teste da soma inclui fio_do_admin, mas sua fixture não produz parcela admin positiva. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/tests/test_o261_vigia_fio.py:560|Teste positivo substitui lista e carregador por mocks, sem demonstrar a costura real. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|A|app/chamados/services/vigia_fio.py:250|Primeira candidata aciona lista completa e carregadores podem executar espelhos com trabalho sobreposto entre competências.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/tests/test_selo_grade_pede_o_instante.py:154|Selo existente de relógio acusa os dois fallbacks do censo e exige resultado vazio.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/services/vigia_fio.py:307|Calendário recebe chamada sem instante e C2/C4 releem datas, deixando dependências fora do instante da rodada.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/services/vigia_fio.py:267|Lista publica dono lavrado, mas vigia consulta novamente o juiz, usando observações distintas do mesmo dia.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:667|Presença exige chave data e exclui dias textuais apresentados por C4/C5.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:187|Competência de C2 usa corte 21 fixo, contrariando o corte por empresa registrado.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/tests/test_o261_vigia_fio.py:657|Teste da conservação acrescenta fio_do_admin sem fabricar parcela admin positiva. [nao verificado]
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/tests/test_o261_vigia_fio.py:560|Matriz positiva usa lista e carregador substituídos, deixando integração real sem cobertura.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/escala/services/cadastro_realidade.py:565|Lista absorve falhas internas de C9 que não incrementam fio_do_admin_sem_resposta.
NOVO|FIO-MORTO-DONO-NA-LISTA_8110a931|B|app/chamados/services/vigia_fio.py:250|Uma candidata dispara lista completa e possíveis cálculos do motor, ampliando o trabalho além dos dias mortos.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:669|Mapa dos dias aceita datas estruturadas de C9 e omite datas mencionadas visualmente por C4/C5.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:565|Falha interna de C9 ou snapshot ausente pode deixar contador de sem resposta em zero.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:186|C2 e C4 consultam relógios próprios no caminho transitivo do censo, apesar do hoje recebido.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/escala/services/cadastro_realidade.py:187|C2 deriva competência com corte 21 fixo em vez da autoridade registrada por empresa.
NOVO|FIO-MORTO-DONO-NA-LISTA_c3b91265|A|app/chamados/services/vigia_fio.py:259|Primeira célula candidata executa lista inteira, incluindo consultas e percurso de células de C2.

## 30/10 14:55 · sessao f965818c06a5 · - · - · origem=Ronald (tmux)

Concordo: item 4 deve ser `n/a` nas três primeiras voltas, por inferência temporal. Em `68a38548` e `0ed50cc2`, a segunda chamada ainda relavra e renova `julgado_em`, indicando ausência da guarda posteriormente removida. (Referências históricas das respostas: `app/chamados/reconciliador.py:862`, `:871`; `app/chamados/services/fio.py:797–799`.)

`22c08b8e/B` associa a remoção à restrição dos chamadores a perguntas sem destino e ao teste de idempotência pelo cron. (Referências da resposta: `app/chamados/reconciliador.py:860–877`, `:1375–1398`; `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:296–308`.)

Grau: forte em `68a38548` e `0ed50cc2`; indireto em `4da2943d`. O commit exato de introdução da guarda permanece [nao verificado].

## 30/10 14:54 · sessao 293994c424b3 · - · - · origem=Ronald (tmux)

Matriz sugerida, com avaliação conservadora de “toca o tema”:

| Volta | Item | Aplicabilidade | A | B |
|---|---:|---|---|---|
| d6adf8c0 | 1 | existia | VIU | VIU |
| d6adf8c0 | 2 | existia | NAO_VIU | NAO_VIU |
| d6adf8c0 | 3 | existia | NAO_VIU | EM_PARTE |
| d6adf8c0 | 4 | n/a, por inferência temporal | n/a | n/a |
| d6adf8c0 | 5 | existia, por inferência | EM_PARTE | EM_PARTE |
| 519fa2c2 | 1 | existia | NAO_VIU | NAO_VIU |
| 519fa2c2 | 2 | existia | NAO_VIU | EM_PARTE |
| 519fa2c2 | 3 | existia | NAO_VIU | EM_PARTE |
| 519fa2c2 | 4 | existia, por inferência temporal | EM_PARTE | EM_PARTE |
| 519fa2c2 | 5 | existia, por inferência | NAO_VIU | NAO_VIU |
| af2cf55f | 1 | existia | NAO_VIU | NAO_VIU |
| af2cf55f | 2 | existia | EM_PARTE | NAO_VIU |
| af2cf55f | 3 | existia | EM_PARTE | NAO_VIU |
| af2cf55f | 4 | existia | EM_PARTE | NAO_VIU |
| af2cf55f | 5 | existia | NAO_VIU | EM_PARTE |

As classificações das respostas usam os anexos transcritos; a tabela não tem referência própria arquivo:linha. [não verificado]

Fundamentos relevantes:

- **Item 1/d6, A e B VIU:** A diz “cada chamada da tela/PDF e dos dois comandos agora executa o detector global”; B diz “cada HTML/PDF e os commands C7/C8 passam a pagar a leitura de toda a frota”. Ambos descrevem inequivocamente o defeito, mesmo sem concluir reprovação. As citações dadas são `app/escala/services/cadastro_realidade.py:336` e os comandos. A proibição já está na cópia em `app/escala/services/cadastro_realidade.py:48-51`.
- **Item 2/519 B EM_PARTE:** “não identifiquei segundo juiz”; **af A EM_PARTE:** “Juiz, escritor e relógio [...] sem cálculo próprio de fase”. Tocam a unicidade sem comparar `ancora_sugerida` com `fase_sugerida`. As autoridades distintas existem na cópia em `app/escala/services/fase_12x36.py:45` e `app/escala/services/fase_na_tela.py:50`.
- **Item 3/d6 B EM_PARTE:** “C10 pode acusar um vínculo sob o cabeçalho de outro” e “ação sem identificar qual corrigir”. Trata seleção divergente de vínculos ativos; não identifica o vínculo histórico dos dias passados nem a necessidade de regenerar suas células. A medição específica está em `app/docs/RELATO.md:18`.
- **Item 3/519 B e af A EM_PARTE:** ambos registram explicitamente que os quatro dias reais do col451 não foram verificados. Isso toca a prova faltante, sem diagnosticar a ação sobre o passado. O resultado da medição é `app/docs/RELATO.md:18`.
- **Item 4:** nas duas respostas de 519 e em af A, “RED histórico [...] não verificado” merece EM_PARTE, não VIU. Af B só diz “Execução dos testes: [não verificado]”, sem RED histórico. O gabarito limita o RED à primeira versão em `app/docs/RELATO.md:19`.
- **Temporalidade do item 4:** a inadequação “RED apenas da primeira versão” surge quando há versões ampliadas. Em d6, B ainda pede reforço de datas/PDF; em 519, ambos descrevem esses reforços. Assim, recomendo n/a em d6 e existia nas seguintes. A falta de novo RED nessas versões intermediárias é inferência; a cronologia exata não está demonstrada por Git ou logs disponíveis. [não verificado]
- **Item 5/d6 EM_PARTE:** ambos leem comandos consumidores de `lista()`, mas não apontam a ausência deles no censo LEI-AKITA. Os leitores estão em `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35` e `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; a declaração incompleta está em `app/docs/RELATO.md:23`.
- **Item 5/af B EM_PARTE é a decisão mais discutível:** enumera pauta/fila/propositor/mensageria, isto é, consumidores de `fora_de_fase`, e fala de “consumidores existentes”; não procura os leitores de `lista()`. Se a rubrica exigir menção à função correta, usar NAO_VIU. A distinção está em `app/docs/RELATO.md:23`, comparada aos comandos acima e `app/core/placar_estrutural.py:149`.

A existência dos itens 1–3 nas voltas anteriores é sustentada pelas chamadas e comportamentos descritos nas respostas, junto às autoridades anteriores da cópia; o momento exato em que a declaração incompleta do item 5 foi escrita nas versões intermediárias não está demonstrado. [não verificado]

## 30/10 14:54 · sessao 478682a2cef6 · BENCH-CORRECAO · censo · origem=Code

**Correção parcial:** faltam as descrições dos itens 2, 3, 5 e 7 de `gab_O261-VETO-A2.txt`. Sem elas, sua classificação e a distinção definitiva dos achados NOVOS ficam `[nao verificado]`. Ausência do anexo não significa `n/a`.

A cópia disponível ainda retorna `None` para `ec=None`; portanto, não comprova diretamente a implementação de `61ae629c` (`app/ponto/services/furos_vetados.py:26`). Para os quatro itens explícitos, considero a existência no hash conforme o gabarito fornecido, também documentada em `app/docs/RELATO.md:9`.

| Item | Correção e evidência |
|---|---|
| s1 | **A=NAO_VIU; B=NAO_VIU.** Nenhum identifica a quebra das fixtures sem vínculo; a regressão está registrada em `app/docs/RELATO.md:9`. |
| s2 | **A=EM_PARTE; B=EM_PARTE.** A diz “deixa de reabrir; passa ao ramo de nota/log”; B diz “Resposta que reabria pode virar nota tardia” — ambos citam `app/chamados/services/resposta_tardia.py:45`. Tocam a mudança funcional, mas não apontam as expectativas antigas dos testes (`app/chamados/tests/test_resposta_tardia.py:72`; `app/chamados/tests/test_c2_le_o_protesto_que_cobra.py:71`). |
| s3 | **A=NAO_VIU; B=NAO_VIU.** Nenhum aponta juiz declarado sem inscrição `@bolha`, defeito exigido pelo contrato em `app/core/tests/test_selo_mapa_contrato.py:41`. |
| m1 | **A=EM_PARTE; B=VIU.** A identifica somente o efeito no c2: “Nova recusa retorna `False`, não costura pergunta” (`app/chamados/reconciliador.py:885`, referência da resposta). B identifica o núcleo: “impressão passa a conter `veto:VETO_DEGENERADA` […] provoca rejulgamento” (`app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`) e o passivo que “sai do total” (`app/ponto/services/furos_sem_cobranca.py:96`; `app/ponto/services/furos_sem_cobranca.py:109`). |

Os seguintes achados são **candidatos a NOVO perante s1–s3/m1**; possível correspondência aos quatro itens ausentes do A2: `[nao verificado]`. As referências abaixo são as citadas pelas respostas, sem certificação de suas linhas no hash da volta `[nao verificado]`.

| Revisor | Referência citada | Achado |
|---|---|---|
| A/B | `app/ponto/management/commands/reconciliar_grade.py:522` | Grade reabre diretamente após recusa de renascimento pela porta. |
| A/B | `app/chamados/services/disputa_emissao.py:1606` | Callback de resolução reabre automaticamente o pai sem veto. |
| A/B | `app/ponto/views.py:291` | Chamado de entrada bloqueada nasce sem dia e escapa do veto. |
| A/B | `app/ponto/management/commands/processar_alertas_turno.py:317` | Marco crítico mantém cobrança após renascimento recusado. |
| A/B | `app/chamados/views_arquivar.py:75` | Exceção humana não apresenta nem registra o motivo do veto. |
| B | `app/ponto/services/furos_vetados.py:98` | Vigia conta reabertura humana permitida como violação. |
| B | `app/ponto/management/commands/furos_vetados_por_regua.py:32` | Agregação conta `None` como um vínculo. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:100` | Selo AST deixa passar alias transitivo de classe. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:92` | Selo AST deixa passar import relativo de módulo. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:170` | Selo AST deixa passar construtor seguido de `save`. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:168` | Selo AST deixa passar `bulk_create` por alias de manager. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:143` | Selo AST deixa passar `update(**valores)`. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:163` | Selo AST não reconhece pedido à porta por alias local. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:170` | Selo AST não reconhece reabridores automáticos por instância. |
| B | `app/chamados/tests/test_contract_status_nucleo.py:7` | Selo complementar também deixa passar construtor/`save` e atualização indireta. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:266` | Falta teste de integração da porta com vínculo ausente. |
| B | `app/chamados/tests/test_o261_veto_na_porta.py:269` | Testes não distinguem a exclusão de dois módulos do universo. |

NOTA|O261-VETO-A_61ae629c|s1|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A_61ae629c|s2|existia|A=EM_PARTE|B=EM_PARTE
NOTA|O261-VETO-A_61ae629c|s3|existia|A=NAO_VIU|B=NAO_VIU
NOTA|O261-VETO-A_61ae629c|m1|existia|A=EM_PARTE|B=VIU

## 30/10 14:54 · sessao 22a1fc43dee9 · - · - · origem=Ronald (tmux)

Cobertura sustentada pelos trechos fornecidos:

| Volta | Item | A | B |
|---|---|---|---|
| 1c565c8f | 3 | NAO_VIU | EM_PARTE |
| 1c565c8f | 6 | NAO_VIU | EM_PARTE |
| 3b68a95b | 7 | EM_PARTE | EM_PARTE |

No item 3, B descreve que o cartório “acrescenta `veto:VETO_DEGENERADA` à impressão”, mas não aponta ausência de teste próprio ou mutante sobrevivente (`app/ponto/services/cartorio.py:1013`, `:1083`, referências da resposta).

No item 6, B declara reversão e hashes não verificados e identifica `furos_sem_cobranca.medir`, sem conferir antes/depois nem a divergência 63/1.576 (`app/ponto/services/furos_sem_cobranca.py:30`, `:96`, referências da resposta; condições operacionais [nao verificado]).

No item 7, ambos examinam `humano=True`, notas, perguntas e trilha, mas não identificam a ausência de exibição/gravação do veto ultrapassado (`app/chamados/models.py:1143`, `:1145`, `:1146`; `app/chamados/services/acoes_disputa.py:266`, referências das respostas).

Para as demais células, não encontrei identificação inequívoca do defeito conhecido no anexo; classificação recomendada NAO_VIU quando o item for temporalmente elegível [nao verificado no hash].

O item 4 está corrigido em a3bbef39 pelo template semanal 0..6; cabe n/a (`app/chamados/tests/_vinculo_do_dia.py:18`, referência de ambas as respostas). A elegibilidade dos demais itens em todos os hashes não pode ser demonstrada apenas pela origem da descoberta; desconhecimento histórico não equivale a n/a [nao verificado].

## 30/10 14:54 · sessao b20ff5bcdc90 · - · - · origem=Ronald (tmux)

Mapeamento independente, sem aproveitar a classificação histórica de `app/docs/RELATO.md:13`:

| Volta | Item | A | B | Sustentação |
|---|---:|---|---|---|
|469d61ba|1|NAO_VIU|NAO_VIU|Nenhuma descreve contexto ora dia do turno, ora dia da saída, levando a D+2. [não verificado]|
|469d61ba|2|NAO_VIU|VIU|B: “O texto lê `lampadas[].instante`; o plantio continua recompondo marcos e escolhendo o primeiro candidato D/D+1 dentro de um envelope” (`app/chamados/catalogo/query.py:65`; `app/chamados/juizes.py:305`).|
|469d61ba|3|EM_PARTE|EM_PARTE|A aponta teste que “simula aplicar” e não testa rota/noturno (`app/chamados/tests/test_corrigir_pergunta_cega.py:54`). B: “não exercitam intervalos, os seis emissores, telas, identidade persistida ou teto de consultas”; RED histórico fica explicitamente não verificado (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65`). Nenhuma identifica TypeError do RED.|
|469d61ba|4|EM_PARTE|VIU|A vê custo na emissão, não o custo específico do lote (`app/chamados/catalogo/query.py:87`). B: “Antes de procurar qualquer conflito, `classe_do_lote` calcula o plantio” e “Esse trabalho acontece por pergunta da fila” (`app/chamados/services/validacao.py:830`; `app/chamados/services/materializacao.py:354`).|
|469d61ba|5|NAO_VIU|EM_PARTE|B registra ausência de exercícios de intervalo, sem apontar mudança de classe no intervalo diurno (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65`).|
|469d61ba|6|n/a provável|n/a provável|Os anexos desta volta mostram cabeçalho lendo somente `data_evento`, sem dependência da célula; `dia_mostrado` aparece na volta seguinte (`app/templates/chamados/partials/_card_atendimento.html:90`, referência da resposta). Ausência histórica absoluta [não verificado].|
|24f1cd69|1|NAO_VIU|NAO_VIU|Sem descrição do defeito. [não verificado]|
|24f1cd69|2|NAO_VIU|NAO_VIU|Sem descrição do defeito. [não verificado]|
|24f1cd69|3|NAO_VIU|NAO_VIU|B discute cobertura do detector AST, que é outro defeito (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154`).|
|24f1cd69|4|NAO_VIU|NAO_VIU|Sem descrição do defeito. [não verificado]|
|24f1cd69|5|NAO_VIU|NAO_VIU|Sem descrição do defeito. [não verificado]|
|24f1cd69|6|EM_PARTE|NAO_VIU|A descreve divergência de apresentação: “tela pode exibir D+1 e pedir confirmação de D”, porém não regeneração da célula (`app/templates/chamados/partials/_card_atendimento.html:164`).|

Para item3, uma leitura menos estrita poderia dar VIU a B/469 pela ausência de testes de telas; recomendo EM_PARTE porque o núcleo “RED parcial”/TypeError não foi identificado. Para item5, também é defensável NAO_VIU a B/469: ausência de testes de intervalo, isoladamente, não identifica mudança de classe. [não verificado] [nao verificado]

Novos da 469d61ba, A:

- `app/templates/chamados/partials/_card_atendimento.html:90` — Cabeçalho conserva D enquanto texto novo mostra D+1.
- `app/templates/chamados/partials/modal_fio.html:157` — Modal imprime texto D+1 e referência D, sem aviso de divergência textual.
- `app/chamados/services/fabrica_da_celula.py:118` — Duas gerações por alvo consultam célula antes do dedup, inclusive tentativa duplicada.
- `app/chamados/management/commands/disparar_perguntas_competencia.py:104` — Emissão descarta a célula já carregada e depois a consulta novamente.
- `app/chamados/management/commands/auditar_disputas.py:188` — Auditoria não compara dia textual com identidade/instante.
- `app/chamados/tests/test_corrigir_pergunta_cega.py:54` — Teste de correção simula gravação sem exercitar ação real nem noturno.

Novos da 469d61ba, B:

- `app/templates/chamados/partials/_card_atendimento.html:90` — Cabeçalho e confirmação anunciam D enquanto texto mostra D+1.
- `app/templates/chamados/partials/modal_fio.html:180` — Modal mantém referência D e detector ignora divergência com o texto.
- `app/api/views.py:1168` — API entrega rótulo D junto da pergunta D+1.
- `app/chamados/catalogo/query.py:95` — Corte nos primeiros dez caracteres aceita hora/sufixo e altera normalização anterior.
- `app/chamados/catalogo/query.py:100` — Motivo desconhecido com padrão datado consulta célula apesar de não possuir marco.
- `app/chamados/services/fabrica_da_celula.py:122` — Duas chamadas de geração consultam célula antes do dedup por alvo datado.
- `app/chamados/services/materializacao.py:404` — DNA inválido pode propagar exceção e interromper classificação do lote.
- `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:128` — Selo não cobre outras formas de interpolação nem produtores fora de chamados.
- `app/chamados/management/commands/auditar_disputas.py:191` — Auditoria verifica existência esperada, sem certificar dia do texto existente.
- `app/chamados/services/fio.py:736` — Ação enviada pelo botão de correção não possui tratamento e cai em “Nada a fazer”.

Novos da 24f1cd69, A:

- `app/templates/chamados/partials/_card_atendimento.html:164` — Escape confirma `data_evento` D apesar de cabeçalho `dia_mostrado` D+1.
- `app/chamados/services/aviso_pendencia.py:20` — Aviso de questionário continua usando `data_evento`, sem `dia_mostrado`.

Novos da 24f1cd69, B:

- `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154` — Detector acusa prosa com `{data}` em argumento nomeado de exceção.
- `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:147` — Detector não reconhece `.format(**{'data': x})`.
- `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:156` — Detector não reconhece literal sob `JoinedStr` usado com `%`.
- `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:154` — Detector acusa concatenação de prosa por aceitar qualquer `BinOp`.
- `app/chamados/tests/test_noturno_le_o_dia_do_instante.py:171` — Varredura restrita a chamados não garante ausência global de produtores.

## 30/10 14:54 · sessao f965818c06a5 · - · - · origem=Ronald (tmux)

Para as três primeiras voltas, a cópia registra a existência dos seis defeitos antes da cura `22c08b8e`, mas não disponibiliza suas árvores históricas; a atribuição individual depende dos anexos e do relato. (`app/docs/RELATO.md:81–84`; confirmação histórica por Git: [nao verificado])

| Volta | Item | A | B |
|---|---|---|---|
| 4da2943d | 1 LEI-AKITA | NAO_VIU | NAO_VIU |
| 4da2943d | 2 Pauta DP | NAO_VIU | NAO_VIU |
| 4da2943d | 3 Custo | VIU | EM_PARTE |
| 4da2943d | 4 Guarda morta | NAO_VIU | NAO_VIU |
| 4da2943d | 5 R1 | NAO_VIU | EM_PARTE |
| 4da2943d | 6 Composição duplicada | NAO_VIU | NAO_VIU |
| 68a38548 | 1–2, 4–6 | NAO_VIU | NAO_VIU |
| 68a38548 | 3 Custo | EM_PARTE | NAO_VIU |
| 0ed50cc2 | 1–6 | NAO_VIU | NAO_VIU |

A classificação de custo de `4da2943d/A` coincide com o registro explícito “3 o Codex viu na volta 1 (custo) e a cura só cobriu o HIST”. (`app/docs/RELATO.md:83`)

Trechos que sustentam os créditos:

- `4da2943d/A`, custo, **VIU**: “Custo novo por pergunta na pilha síncrona; consulta repetida”; “O C6 ainda foi antecipado para antes dos gates baratos do c1”. A resposta dá `app/chamados/reconciliador.py:1436`, `:1438`, `:749`, `:709`, `:703`.
- `4da2943d/B`, custo, **EM_PARTE**: “o selo do chokepoint mede queries e callbacks [...] esse cadastro não exercita a nova seleção HIST da órfã”. Detecta cobertura insuficiente, sem apontar a cerca cara antes dos vetos do c2. A resposta dá `app/ponto/tests/test_selo_chokepoint_escrita.py:207`, `:226`, `:239`, `:258`.
- `4da2943d/B`, R1, **EM_PARTE**: “R1 exige destino vivo ou recusa [...] Não identifiquei vacuidade equivalente nessas asserções contra a remoção integral da cura”. Toca especificamente a asserção permissiva, sem denunciar a falta de exigência de renascimento. A resposta dá `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117`. O relato confirma que o defeito conhecido era R1 sem mordida e que a cura passou a exigir renascimento. (`app/docs/RELATO.md:83–84`)
- `68a38548/A`, custo, **EM_PARTE**: “cenário FICAM cria chamados vivos, sem cobrir o ramo novo com encerrados. Limite de cobertura, sem nova regressão de custo demonstrada”. A resposta dá `app/ponto/tests/test_selo_chokepoint_escrita.py:164`, `:364`.

Não considero as negativas genéricas sobre “segundo juiz” crédito parcial do item 6: elas discutem delegação aos juízes ou mapas de motivos, sem comparar a composição do vigia com a do reconciliador. A duplicação específica e a divergência sem data estão descritas em `app/chamados/services/vigia_fio.py:455–465`.

Achados novos a consolidar, preservando referências históricas citadas pelas respostas, sem afirmar validade definitiva:

```text
NOVO|O261-C_4da2943d|A|app/chamados/services/disputa_emissao.py:1622|HIST ampliado permite que intervalo morto por tranca alcance ramo do S128 que carimba perguntas sem respeitar a proteção da tranca.
NOVO|O261-C_4da2943d|A|app/chamados/services/disputa_emissao.py:1634|Ramo posterior do S128 carimba resposta MORTA sem repetir a proteção de resposta sem batida real, inclusive com pai ausente.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:92|Fixture correlaciona vitalidade com validada_em e permite substituir C6 por um segundo juiz desse campo sem distinguir os cenários.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:95|Bateria não chama diretamente reemitir_chamado_do_dia e não sela que a cura esteja dentro do c2.
NOVO|O261-C_4da2943d|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:66|Fixture grava ii/fi, enquanto o juiz consome hii/hfi, deixando os intervalos sem vínculo com sua lâmpada específica.
NOVO|O261-C_68a38548|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:235|Igualdade integral na segunda chamada direta de reatar exige idempotência incompatível com a relavratura que renova julgado_em.
NOVO|O261-C_0ed50cc2|A|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264|Teste de idempotência compara todas as colunas embora a segunda chamada relavre e renove julgado_em. [nao verificado]
NOVO|O261-C_0ed50cc2|B|app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:264|Teste de idempotência falha com relógio avançando e pode mascarar a relavratura quando o relógio está congelado. [nao verificado]
```

`68a38548/B` não apresenta achado novo; os controles negativos R4/R6 de `4da2943d/B` são descritos pela própria resposta como compatíveis com seu papel, portanto não os contaria como defeitos adicionais. (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:141`, `:148`, `:169`, `:182`, referências da resposta)

Quanto aos itens posteriores: o defeito de veto já decorre do caminho que começou a aceitar intervalos, antes de sua descoberta pela suite; não deve receber `n/a` apenas porque foi descoberto em `7a8e4604`. A cópia o chama de “bug no caminho escondido enquanto o c2 recusava intervalo”. (`app/docs/RELATO.md:85–86`; `app/chamados/reconciliador.py:843–849`) A ordem da docstring requer cuidado: `3ab94852` é posterior a `13963efb` e alterou prosa, mas a cópia não prova, por si só, em qual volta a lista ficou fora da ordem. (`app/docs/RELATO.md:86–87`; histórico exato: [nao verificado])

## 30/10 14:54 · sessao 65188d236c8b · - · - · origem=Ronald (tmux)

Fonte cronológica central: `app/docs/RELATO.md:81–87`.

| Item | 4da2943d | 68a38548 | 0ed50cc2 | 22c08b8e | 13963efb |
|---|---|---|---|---|---|
| 1 LEI-AKITA fora dos commits | Presença histórica exata [nao verificado] | idem | idem | curado | curado | [nao verificado]
| 2 Pauta DP sem código/INCOMPLETO | provável existente; anterioridade exata [nao verificado] | idem | idem | curado | curado |
| 3 cerca cara antes dos gates | existia | existia | existia | n/a | n/a |
| 4 guarda morta no c2 | introdução posterior provável [nao verificado] | idem | idem | n/a | n/a |
| 5 R1 sem mordida no renascer | existia | existia por continuidade [nao verificado] | idem | n/a | n/a |
| 6 composição duplicada | existia | existia | existia | n/a | n/a |
| 7 ordem da lista na docstring | n/a | n/a | n/a | n/a | n/a |
| 8 emissão apesar do veto da régua | existia | existia | existia | existia | n/a |

Justificativas decisivas:

- Os seis achados estão documentados somente em `df863705`; a cura em `22c08b8e` é expressa, incluindo exclusão da guarda, fortalecimento de R1 e composição única. `app/docs/RELATO.md:83–84`.
- A guarda morta provavelmente nasceu para responder à idempotência detectada nas voltas 68/0ed: ambas as respostas mostram que o segundo reatar ainda chegava à relavratura; em 22 já se informa a remoção da guarda. Data exata de introdução: [nao verificado]. Não retroagir automaticamente o item 4.
- A alteração da docstring ocorre em `3ab94852`, posterior a `13963efb`; somente então a quarta leitura reprova a ordem. Marcar item 7 como `n/a` nas cinco voltas. `app/docs/RELATO.md:86–87`.
- A falta do veto já existia no caminho ampliado de intervalo; a regressão ficava escondida enquanto c2 recusava intervalo. `13963efb` acrescenta o veto e deixa verdes os cinco casos. `app/docs/RELATO.md:85–86`; `app/chamados/reconciliador.py:843–860`.
- Os resíduos de `13963efb` — vínculo `None`, concorrência entre leitura e trava, alcance aos motivos C1 — são candidatos a NOVO; não equivalem à ausência completa de veto do item 8. `app/chamados/reconciliador.py:807–808`, `:849–854`.

Limite: não localizei histórico Git nem snapshots dos hashes antigos; as conclusões anteriores à versão atual dependem dos trechos fornecidos e do registro cronológico, sem confirmação dos bytes históricos. [nao verificado]

## 30/10 14:54 · sessao a6ca0cde0a9f · - · - · origem=Ronald (tmux)

Evidência histórica na cópia:

- `app/docs/RELATO.md:24` relaciona a construção em `31b8cec0`, o recorte anterior em `469d61ba` e o recorte posterior em `691508dd`; descreve produtor lendo DNA, `dia_mostrado` e `classe_do_lote` verificando instante da célula e do plantio.
- `app/docs/RELATO.md:13` registra os seis defeitos em `44e6a8a1`, porém não demonstra sozinho em qual commit cada defeito surgiu. A triagem antiga existente nessa linha não substitui a avaliação cega das respostas anexadas.
- Os hashes `24f1cd69`, `e8c314e0` e `06d7ad06` não apareceram na busca textual realizada nas documentações; ancestralidade Git e diferenças completas entre esses hashes: [não verificado].
- Os mecanismos dos itens 1, 2 e 4 já aparecem na resposta B de `469d61ba`: produtor consulta célula pelo dia; texto lê DNA enquanto plantio usa envelope; `classe_do_lote` chama `_ts_tipo_do_plantio` por pergunta (`app/chamados/catalogo/query.py:65,100`; `app/chamados/juizes.py:305,308`; `app/chamados/services/validacao.py:830`, referências históricas citadas por B).
- Para o item 6 específico, `469d61ba` ainda mostra cabeçalho por `data_evento`, não por DNA recalculado (`app/templates/chamados/partials/_card_atendimento.html:90`, citado por A/B). O mecanismo de regeneração alterar o cabeçalho só pode ser imputado depois da propriedade `dia_mostrado`; ela já aparece em `24f1cd69` (`app/templates/chamados/partials/_card_atendimento.html:90`, citado por A). Implementação histórica completa dessa propriedade na segunda volta: [não verificado].
- Item 3 exato, incluindo o TypeError do RED, e item 5 exato, alteração de classe do intervalo diurno, não têm origem por hash comprovada além do relato final: [não verificado]. A primeira resposta B evidencia lacuna de testes de intervalos e telas, mas isso não prova o mesmo histórico de execução do RED (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:65,96,104,110`, citações de B).

Últimas duas voltas:

- `e8c314e0`: A e B não abordam nenhum dos seis itens; ambos apontam falso positivo do detector para `.format(**{**{'nome': 'Ana'}})` (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170`, A; `:172`, B). Trata-se de NOVO, não de RED parcial.
- `06d7ad06`: A e B não abordam nenhum dos seis itens; ambos apontam falso negativo do detector para campos `{data!s}` ou especificadores de formato (`app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166`, ambos). Trata-se de NOVO.

Linhas de novos para consolidar:

NOVO|O261-NOTURNO_e8c314e0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170|Detector acusa formatação de nome com expansão interna de dicionário, mesmo sem campo data.
NOVO|O261-NOTURNO_e8c314e0|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:172|Detector acusa formatação de nome com expansão interna de dicionário, mesmo sem campo data.
NOVO|O261-NOTURNO_06d7ad06|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166|Detector deixa passar campos data válidos com conversão ou especificador, como {data!s} e {data:}.
NOVO|O261-NOTURNO_06d7ad06|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166|Detector deixa passar campos data válidos com conversão ou especificador, como {data!s} e {data:%d/%m/%Y}.

## 30/10 14:54 · sessao 163a7870915f · - · - · origem=Ronald (tmux)

Mapeamento recomendado, julgando os defeitos específicos do gabarito:

| Volta | Item | A | B |
|---|---:|---|---|
| 31b8cec0 | 1 | NAO_VIU | NAO_VIU |
| 31b8cec0 | 2 | NAO_VIU | NAO_VIU |
| 31b8cec0 | 3 | NAO_VIU | NAO_VIU |
| 31b8cec0 | 4 | EM_PARTE | NAO_VIU |
| 31b8cec0 | 5 | NAO_VIU | NAO_VIU |
| 31b8cec0 | 6 | EM_PARTE | NAO_VIU |
| 691508dd | 1–5 | NAO_VIU | NAO_VIU |
| 691508dd | 6 | EM_PARTE | EM_PARTE |

Sustentação dos parciais:

- **31 A, item4:** “Em cobrança/aviso, esse carregamento por pergunta já acontece no juiz” (`aviso_pendencia.py:9`; `juizes.py:757`, `:791`). Discute custo por pergunta, mas o mecanismo é carregar `p.celula` em `pergunta_viva`, não executar `_ts_tipo_do_plantio` dentro de `classe_do_lote`.
- **31 A, item6:** “`dia_mostrado` ainda pode contradizer o texto gravado”; exemplo com contexto ISO contendo hora (`query.py:99`; `models.py:2176`). Não menciona regeneração da célula.
- **691 A, item6:** “O ponto final quebra a igualdade com o produtor e aciona o fallback” (`models.py:2178`).
- **691 B, item6:** “acrescentar apenas `.` faz retornar 21/10, embora o texto continue dizendo 22/10” (`models.py:2178`). Ambos apontam contradição de apresentação por texto não canônico, sem regeneração.

Novos achados candidatos, extraídos das respostas; conferência histórica independente não realizada [não verificado]:

NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/catalogo/query.py:99|Contexto ISO com hora é preservado cru no texto, mas dia_mostrado pode retornar o dia seguinte do DNA.
NOVO|O261-NOTURNO_31b8cec0|A|app/api/views.py:1043|API carrega chamado_destino__celula, mas dia_mostrado acessa p.celula e pode acrescentar um SELECT por pergunta.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/models.py:2162|Propriedade sem cache recalcula dia_mostrado no cabeçalho e em cada opção do card.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:155|Detector ignora substituição por variável e literal usado como argumento pergunta.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:137|Exclusão de models.py inteiro e varredura limitada a chamados deixam produtores fora da fiscalização.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:150|Detector acusa format com expansão sem data e format_map de outro campo.
NOVO|O261-NOTURNO_31b8cec0|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:158|Detector acusa concatenação de prosa porque aceita qualquer BinOp com literal {data}.
NOVO|O261-NOTURNO_31b8cec0|B|app/templates/chamados/meu_atendimento.html:19|Confirmação usa p e opt sem definição no laço reg, deixando o texto sem rótulo e dia.
NOVO|O261-NOTURNO_691508dd|A|app/chamados/models.py:2178|Texto com ponto final ou data ISO com hora pode acionar fallback para identidade e contradizer o dia textual.
NOVO|O261-NOTURNO_691508dd|A|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168|Detector ignora format com desempacotamento interno ou chave dinâmica que pode fornecer data.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/models.py:2178|Texto com ponto final ou data ISO com hora pode acionar fallback para identidade e contradizer o dia textual.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:168|Detector ignora format com desempacotamento interno que pode fornecer data.
NOVO|O261-NOTURNO_691508dd|B|app/chamados/tests/test_noturno_le_o_dia_do_instante.py:156|Literal dia {data} dentro de outro dicionário passa, permitindo copiar o espelho fora da isenção nominal.

A observação da API devolver identidade D e label D+1 não afirma uso incorreto pelo consumidor; ambas as respostas deixam esse consumo [não verificado]. Não a recomendaria como NOVO autônomo.

## 30/10 14:53 · sessao ddeaeb928a6b · - · - · origem=Ronald (tmux)

- Item 2: a cópia não contém a declaração nova de `motivo_do_veto` no registro, mas já contém o leitor `furos_vetados_por_regua` em `app/escala/management/commands/desfazer_corroboracao_regua.py:38`. A desatualização específica da linha LEI-AKITA nos hashes das voltas depende do anexo; anterioridade exata [nao verificado]. B/1c cita esse leitor, mas não denuncia a declaração desatualizada; isso sustenta, no máximo, EM_PARTE [nao verificado].
- Item 3: o cartório já consulta o juiz sem filtrar `ec=None` e inclui seu resultado na impressão (`app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`). O juiz da cópia ainda devolve `None` para ausência de vínculo (`app/ponto/services/furos_vetados.py:26`). O teste anterior cria vínculo e só compara impressão com `veto=None`/`VETO_REGUA` (`app/ponto/tests/test_veto_que_cai_rejulga.py:42`, `app/ponto/tests/test_veto_que_cai_rejulga.py:115`). B/1c explica o efeito na impressão, sem apontar falta de teste próprio ou mutante: EM_PARTE [nao verificado quanto à classificação textual].
- Item 5: a cópia ainda não possui `recusado_veto`; o retorno `_ch is None` incrementa `recusado_celula` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:169`). Portanto o defeito novo descrito no gabarito não pode ser retroprojetado somente desta cópia; a data de introdução por volta é [nao verificado].
- Item 7: a exigência de mostrar o veto ao humano e registrar motivo já está documentada em `app/docs/BACKLOG.md:785` e `app/docs/PROMPTS.md:1659`. As notas humanas antigas registram resgate, competência ou justificativa administrativa, sem o veto (`app/chamados/views_arquivar.py:79`, `app/chamados/services/competencia_trancada.py:259`, `app/chamados/services/acoes_disputa.py:270`). A e B/3b discutem autorização e notas/trilha, mas não apontam ausência de exibição/registro do veto ultrapassado: EM_PARTE é defensável pelo tema explícito [nao verificado quanto à classificação textual].
- Fonte documental útil para item 6: reversão antes e prova depois já constam em `app/docs/BACKLOG.md:785`; o estado-base já documenta impacto da impressão e de `furos_sem_cobranca.medir` em `app/docs/RELATO.md:9`.

## 30/10 14:53 · sessao 946742f8d813 · - · - · origem=Ronald (tmux)

A cópia disponível diverge do estado descrito para `61ae629c`: `motivo_do_veto(..., ec=None, ...)` ainda retorna `None` em `app/ponto/services/furos_vetados.py:26–27`; o arquivo termina em `:58` e ainda usa `ec.pk` diretamente em `:56`. A identificação histórica desta árvore como base anterior é [nao verificado].

- **s1:** A e B não identificam a quebra das fixtures em 45 módulos [nao verificado quanto à contagem, fornecida somente no gabarito]. Ambos censam leitores de produção; nenhum descreve fixtures sem vínculo recebendo `None`. Sugestão: **NAO_VIU / NAO_VIU**.
- **s2:** Os testes citados existem. `app/chamados/tests/test_resposta_tardia.py:24–29` cria colaborador sem vínculo; `:61–75` exige renascimento e declaração. `app/chamados/tests/test_c2_le_o_protesto_que_cobra.py:27–34` também não cria vínculo; `:68–72` exige emissão e destino. A/B discutem efeitos nos dois serviços, mas não identificam esses testes incompatíveis. Sugestão: **EM_PARTE / EM_PARTE** se “toca o tema” abranger os efeitos funcionais; nenhum merece VIU.
- **s3:** A/B não apontam ausência de inscrição `@bolha`. O selo exige ausência de novas bolhas sem contrato em `app/core/tests/test_selo_mapa_contrato.py:41–49`. O registro de `furos_vetados` não está no `core/juizes.py` desta cópia; existência no hash da volta vem somente do anexo [nao verificado no código disponível]. Sugestão: **NAO_VIU / NAO_VIU**.
- **m1:** B aponta precisamente impressão, rejulgamento e mudança do passivo/c2: “impressão passa a conter `veto:VETO_DEGENERADA` [...] provoca rejulgamento”; “passa de canal de cobrança para `veto`; sai do total”. O mecanismo preexistente é confirmado por `app/ponto/services/cartorio.py:329–330`, `:1013`, `:1083`, `:1092`; `app/ponto/services/furos_sem_cobranca.py:96–109`, `:125–126`; `app/chamados/reconciliador.py:854–857`. **B=VIU**, sem exigir que tivesse os números da medida. A lista efeitos de emissão/c2, mas não impressão/cartório/passivo: **A=EM_PARTE**, se contar esses efeitos como contato com o tema; caso critério estrito sobre a regressão compartilhada, NAO_VIU.

Os números `2.736`, `234 × 1.964`, `31 failures`, `163 errors`, `201 labels` e `46 módulos` não são conferíveis no código lido; só constam do dado fornecido [nao verificado].

## 30/10 14:53 · sessao 68fb51d5b847 · - · - · origem=Ronald (tmux)

O anexo A2 ausente impede confirmar quais candidatos abaixo são novos em relação ao gabarito completo; a correspondência com A2 itens 2, 3, 5 e 7 é `[nao verificado]`.

Candidatos fora de s1/s2/s3/m1, usando as referências citadas nas respostas:

| Revisor | Achado reportado | Referência citada |
|---|---|---|
| A/B | Grade reabre chamado após recusa de renascimento pela porta | `app/ponto/management/commands/reconciliar_grade.py:522` |
| A/B | Callback de resolução reabre pai automaticamente sem veto | `app/chamados/services/disputa_emissao.py:1606` |
| A/B | `entrada_bloqueada_turno_aberto` nasce sem dia e escapa do veto | `app/ponto/views.py:291` |
| A/B | Marco crítico cobra após renascimento recusado, porque só testa `None` | `app/ponto/management/commands/processar_alertas_turno.py:317` |
| A/B | Reabertura humana não apresenta nem registra motivo do veto | `app/chamados/views_arquivar.py:75` |
| B | Vigia inclui reabertura humana permitida entre violações | `app/ponto/services/furos_vetados.py:98` |
| B | Contador publica um vínculo para achados cujo vínculo é `None` | `app/ponto/management/commands/furos_vetados_por_regua.py:32` |
| B | AST não reconhece alias transitivo de classe | `app/chamados/tests/test_o261_veto_na_porta.py:100` |
| B | AST não reconhece import relativo de módulo | `app/chamados/tests/test_o261_veto_na_porta.py:92` |
| B | AST não reconhece construtor seguido de `save` | `app/chamados/tests/test_o261_veto_na_porta.py:170` |
| B | AST não reconhece `bulk_create` por alias de manager | `app/chamados/tests/test_o261_veto_na_porta.py:168` |
| B | AST não reconhece `update(**valores)` | `app/chamados/tests/test_o261_veto_na_porta.py:143` |
| B | AST não reconhece pedido à porta por alias local | `app/chamados/tests/test_o261_veto_na_porta.py:163` |
| B | AST não reconhece reabridores automáticos por instância | `app/chamados/tests/test_o261_veto_na_porta.py:170` |
| B | Selo complementar também deixa passar construtor/`save` e `update(**valores)` | `app/chamados/tests/test_contract_status_nucleo.py:7` |
| B | Testes não exercitam integração da porta com vínculo ausente | `app/chamados/tests/test_o261_veto_na_porta.py:266` |
| B | Testes não distinguem exclusão de `batida_ausente` e `intervalo_pendente` do universo | `app/chamados/tests/test_o261_veto_na_porta.py:269` |

A cópia atual confirma os sítios de grade, callback, nascimento sem dia, marco crítico e trilha humana, mas não contém a implementação nova do veto: `motivo_do_veto` ainda retorna `None` para vínculo ausente (`app/ponto/services/furos_vetados.py:26`). Os trechos do AST, do novo vigia e dos novos testes não puderam ser conferidos nesta cópia `[nao verificado]`.

Na cópia atual, os sítios correspondentes são: grade `app/ponto/management/commands/reconciliar_grade.py:519`; booleano ignorado `app/chamados/models.py:686`; callback `app/chamados/services/disputa_emissao.py:1606`; contexto sem dia `app/ponto/views.py:280`; marco crítico `app/ponto/management/commands/processar_alertas_turno.py:317`; trilha humana `app/chamados/views_arquivar.py:75`, `app/chamados/services/acoes_disputa.py:270` e `app/chamados/services/competencia_trancada.py:261`.

## 30/10 14:51 · sessao 388a9c07ec1a · BENCH-O261-VETO-A2 · verificador · origem=Code

**1. Achados 1–5 do Opus**

| Achado | Veredito e mudança | Mutante plausível que permanece |
|---|---|---|
| **1 — RED contra a base** | **Cura não comprovada.** O diff fornecido não traz nova execução contra a base; RED histórico continua **[nao verificado]**. Os testes de nascimento e reabertura existem, mas sua existência não prova aquela execução (`app/chamados/tests/test_o261_veto_na_porta.py:418`, `app/chamados/tests/test_o261_veto_na_porta.py:522`). | Não há teste novo dirigido a esse achado no diff fornecido **[nao verificado quanto ao histórico]**. |
| **2 — declaração LEI-AKITA** | **Cura não comprovada.** A declaração corrigida não foi fornecida **[nao verificado]**. Os elementos que ela deveria nomear existem: `recusa_da_reabertura`, chamada por `reabrir`, e o consumidor `desfazer_corroboracao_regua` (`app/chamados/models.py:1071`, `app/chamados/models.py:1143`, `app/escala/management/commands/desfazer_corroboracao_regua.py:38`). | Não há teste novo dirigido à completude dessa declaração **[nao verificado]**. |
| **3 — impressão com `ec=None`** | **Curado no escopo apontado.** O novo teste observa a chamada real, exige ausência de veto com vínculo e `VETO_DEGENERADA` depois de encerrá-lo (`app/ponto/tests/test_veto_que_cai_rejulga.py:124`, `app/ponto/tests/test_veto_que_cai_rejulga.py:129`, `app/ponto/tests/test_veto_que_cai_rejulga.py:131`). | Fazer o hash **ignorar especificamente `VETO_DEGENERADA`**: o spy ainda recebe o argumento esperado, pois o teste só inspeciona `kwargs`. O teste vizinho de hash cobre `VETO_REGUA` (`app/ponto/services/cartorio.py:329`, `app/ponto/tests/test_veto_que_cai_rejulga.py:126`, `app/ponto/tests/test_veto_que_cai_rejulga.py:138`). |
| **4 — helper 5x2** | **Curado para o calendário do helper.** Agora declara trabalho nos sete dias; o cálculo semanal respeita essa lista (`app/chamados/tests/_vinculo_do_dia.py:18`, `app/escala/models.py:830`). Isso não certifica toda a suíte contra dependência de calendário **[nao verificado]**. | Não foi acrescentado teste próprio percorrendo os sete dias no diff fornecido **[nao verificado quanto à suíte]**. Retirar domingo da lista é um mutante plausível cuja detecção dependeria dos dias exercitados; sobrevivência **[nao verificado]**. |
| **5 — veto por eliminação** | **Curado.** O contador de veto exige resposta positiva de `veto_no_nascimento`; ausência das duas respostas vai para `recusado_sem_juiz` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:179`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`). | Inverter a prioridade quando **célula e veto recusam simultaneamente**: os testes cobrem motivos isolados. Também passa pelo novo MORDE retirar a mensagem `[RECUSADO sem juiz]`, preservando os contadores que ele verifica (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:113`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:124`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:142`). |

As sobrevivências acima são inferências das asserções citadas; execução dos mutantes e resultado da suíte completa: **[nao verificado]**. [nao verificado]

**2. Efeito do helper nas fixtures**

**Sim, muda comportamento de calendário.** Sábado e domingo deixam de cair no `weekday() < 5` do 5x2 e passam a trabalho no semanal 0–6. Criação ou mudança de chamado pode disparar julgamento do dia, portanto o efeito pode alcançar grade, ata e emissão; chamado novo ainda depende dos demais predicados do cartório (`app/escala/models.py:861`, `app/escala/models.py:830`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`, `app/ponto/services/cartorio.py:1339`).

Casos concretos que merecem distinção:

| Módulo | Dependência e efeito |
|---|---|
| `app/chamados/tests/test_agrupado_em.py:17` | Volta a **01/08/2026**, sábado. A fixture precisa de premissa viva para reabrir quando o pai fecha; há controle positivo e verificação de unicidade, portanto não ficou apenas num negativo vazio (`app/chamados/tests/test_agrupado_em.py:39`, `app/chamados/tests/test_agrupado_em.py:40`, `app/chamados/tests/test_agrupado_em.py:45`). |
| `app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:52` | Usa **05/09/2026**, sábado, e recebe o helper. Entretanto, `premissa_regularizada` está patchada para `False`: esse teste não prova a classificação real de folga/trabalho (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:42`, `app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:68`). |
| `app/ponto/tests/test_justificativa_encerra_origem.py:101` | O lote inclui **25/10/2026**, domingo, com `turno_aberto`; muda o calendário de apoio. A asserção exige resolução dos chamados após aprovação (`app/ponto/tests/test_justificativa_encerra_origem.py:93`, `app/ponto/tests/test_justificativa_encerra_origem.py:102`). |
| `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196` | **Folga explícita preservada:** impõe `trabalha=False` e ata de folga; também cobre folga com ata velha e o oposto, trabalho com furo. A célula tem prioridade sobre o template (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:199`, `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:214`, `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:219`, `app/escala/models.py:1071`). |

Módulos com **data do fato relativa**, sujeitos à troca do calendário de apoio quando ela cai no fim de semana:

| Caminho | Referências |
|---|---|
| `app/ponto/tests/` | `test_a2_marco_morto.py:31`; `test_bug115_lampada_do_marco.py:54`; `test_bug114_fechador_de_dois_dias.py:73` |
| `app/api/tests/` | `test_chamados_v2_destino.py:37` |
| `app/chamados/tests/` | `test_orfa_sem_candidato.py:37`; `test_3c_abrir_ausencia.py:36`; `test_e5b_cobrar_massa.py:24`; `test_f3_dedup_atraso.py:55`; `test_selo_b5h_followup_motivo.py:68`; `test_validar_classe_a.py:45`; `test_reconciliar_pergunta_orfa.py:27`; `test_fila_por_causa.py:111`; `test_b8_smoke_reabertura.py:36`; `test_selo_i6_ouve_a_lei.py:63`; `test_resposta_tardia.py:20`; `test_emissor_nomeia_o_marco.py:44`; `test_selo_f4_fatia4.py:65`; `test_selo_dedup_premissa_viva.py:90`; `test_bug125_cobranca_cega.py:56`; `test_selo_porta_recusa.py:63`; `test_regressao_etapa7.py:79` |

Nessa lista, alguns declaram trabalho diretamente na célula, reduzindo a dependência do template: `app/chamados/tests/test_3c_abrir_ausencia.py:43`, `app/chamados/tests/test_f3_dedup_atraso.py:44`, `app/chamados/tests/test_orfa_sem_candidato.py:41`, `app/chamados/tests/test_emissor_nomeia_o_marco.py:48`, `app/chamados/tests/test_resposta_tardia.py:34`.

O caso “sem escala” de `test_selo_f4_fatia4` também permanece separado: consulta diretamente o colaborador sem passar pelo método que aplica o helper (`app/chamados/tests/test_selo_f4_fatia4.py:67`, `app/chamados/tests/test_selo_f4_fatia4.py:131`, `app/chamados/tests/test_selo_f4_fatia4.py:139`).

**Sem achado demonstrado de folga intencional convertida em verde vazio.** Quantidades efetivamente criadas ou ocultadas de furos, chamados e previstos, antes/depois: **[nao verificado]**. A afirmação do anexo sobre “27 módulos com data relativa” não fica certificada por este inventário **[nao verificado]**. [nao verificado]

**3. O spy intercepta?**

**Sim; sem achado.** A chamada usa o nome global `impressao_insumos`, definido no próprio módulo. Esse nome é resolvido no dicionário de `cartorio`, justamente onde `patch.object` coloca o spy; não há uma referência local previamente capturada nessa chamada (`app/ponto/services/cartorio.py:314`, `app/ponto/services/cartorio.py:829`, `app/ponto/services/cartorio.py:1083`, `app/ponto/tests/test_veto_que_cai_rejulga.py:124`).

O import local de `julgar_colab` no teste não captura `impressao_insumos`: ele chama a função que despacha o corpo do julgamento (`app/ponto/tests/test_veto_que_cai_rejulga.py:83`, `app/ponto/services/cartorio.py:744`).

**4. Ordem e alcance do MORDE**

**Sem achado na ordem implementada:** no nascimento, `abrir` pergunta célula e depois veto; o command repete essa prioridade. `sem_juiz` é uma classificação posterior do command, não uma terceira guarda de `abrir` (`app/chamados/models.py:726`, `app/chamados/models.py:731`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`).

**O MORDE exercita o ramo certo do command.** A fixture tem vínculo e célula acusadora; somente `abrir` é substituído por `(None, False)`, permitindo alcançar “nenhum juiz assume” (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:50`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:73`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:140`).

Ele não exercita internamente `abrir`, nem demonstra a mudança concorrente do dia narrada na docstring; essa ocorrência real é **[nao verificado]** (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:136`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:140`).

**5. Consumidores da linha `fim:`**

**Sem achado de quebra nos consumidores identificados da cópia.** Os testes leem substrings e toleram o campo acrescentado (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:118`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:131`, `app/ponto/tests/test_passe_turno_aberto_retroativo.py:142`).

O command está declarado fora do pipeline, por ato, sem horário; o envelope de cron produz JSON próprio a partir de início/fim/exit, sem interpretar essa linha de stdout (`app/config/crons.py:743`, `app/config/crons.py:875`, `app/config/crons.py:884`, `bin/cron_run.sh:11`, `bin/cron_run.sh:13`). O placar lê esse JSON, e a regex de `placar_code.sh` procura `crons_quebrados=`, não `fim:` (`app/core/management/commands/pipeline_placar.py:65`, `app/core/services/crons_quebrados.py:35`, `bin/placar_code.sh:453`).

Cron instalado, scripts externos e regex fora da cópia: **[nao verificado]**.

## 30/10 14:50 · sessao 0628f7e0436d · - · - · origem=Ronald (tmux)

Q2 — mudança real na fixture, mas **sem achado de regressão concreta demonstrada**; execução da suíte e comparação de quantidades de furos/chamados/previstos: **[nao verificado]**. [nao verificado]

O helper declara SEMANAL 0..6 em `app/chamados/tests/_vinculo_do_dia.py:18`. Isso muda sábado/domingo de folga para trabalho no cálculo do template: SEMANAL consulta `weekday in dias` em `app/escala/models.py:830`, enquanto 5x2 sem configuração retorna `weekday < 5` em `app/escala/models.py:861`. Não é só “dar vínculo”: pode mudar a grade/ata quando há julgamento disparado pela criação/mudança de chamado (`app/chamados/signals.py:164`, `app/chamados/signals.py:180`). Não implica necessariamente furo/chamado novo: emissão do cartório tem outros predicados e exige dia de trabalho (`app/ponto/services/cartorio.py:1339`, `app/ponto/services/cartorio.py:1348`).

Casos fixos relevantes:

- `test_agrupado_em`: volta para 01/08/2026; exige que pai fechado libere reabertura, portanto quer premissa viva/trabalho e não folga. Controle não é só negativo: exige filho existente e único e reabertura positiva (`app/chamados/tests/test_agrupado_em.py:17`, `:39`, `:40`, `:45`).
- `test_selo_i4b_um_chamado_por_turno`: fixa 05/09/2026 e dá vínculo quando não há escala (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:52`, `:68`). Porém a premissa está patchada para `False`, então não prova classificação de folga/trabalho (`:42`). Seu teste do emissor usa vínculo próprio 12x36 e célula explícita, fora do helper (`:219`, `:224`).
- `test_justificativa_encerra_origem`: segundo caso do lote usa 25/10/2026 e cria chamado `turno_aberto`; muda o calendário de apoio, mas asserção principal exige ambos resolvidos após aprovar (`app/ponto/tests/test_justificativa_encerra_origem.py:101`, `:102`, `:93`).

**Folga explícita preservada:** `test_reconciliar_pergunta_a_premissa` impõe `trabalha=False`/ata folga e exige fechamento (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196`, `:199`, `:201`); testa também folga com ata velha (`:212`, `:214`, `:215`) e o oposto, trabalho com furo recusando (`:217`, `:219`). O leitor da escala prioriza `cel.trabalha` sobre template (`app/escala/models.py:1071`). Não identifiquei caso que esperasse folga apenas por cair no fim de semana e agora ficasse verde vacuamente **[nao verificado quanto à execução]**.

**Inventário dos 21 módulos com data do fato explicitamente relativa**, todos sujeitos à troca do calendário de apoio caso a data caia no fim de semana; isto não significa que cada um tenha resultado final alterado:

- `app/ponto/tests/test_a2_marco_morto.py:31`
- `app/ponto/tests/test_bug115_lampada_do_marco.py:54`
- `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:73`
- `app/api/tests/test_chamados_v2_destino.py:37`
- `app/chamados/tests/test_orfa_sem_candidato.py:37`
- `app/chamados/tests/test_3c_abrir_ausencia.py:36`
- `app/chamados/tests/test_e5b_cobrar_massa.py:24`
- `app/chamados/tests/test_f3_dedup_atraso.py:55`
- `app/chamados/tests/test_selo_b5h_followup_motivo.py:68`
- `app/chamados/tests/test_validar_classe_a.py:45`
- `app/chamados/tests/test_reconciliar_pergunta_orfa.py:27`
- `app/chamados/tests/test_fila_por_causa.py:111`
- `app/chamados/tests/test_b8_smoke_reabertura.py:36`
- `app/chamados/tests/test_selo_i6_ouve_a_lei.py:63`
- `app/chamados/tests/test_resposta_tardia.py:20`
- `app/chamados/tests/test_emissor_nomeia_o_marco.py:44` (também `:126`)
- `app/chamados/tests/test_selo_f4_fatia4.py:65`
- `app/chamados/tests/test_selo_dedup_premissa_viva.py:90`
- `app/chamados/tests/test_bug125_cobranca_cega.py:56`
- `app/chamados/tests/test_selo_porta_recusa.py:63`
- `app/chamados/tests/test_regressao_etapa7.py:79` (também `:107`).

Distinções que evitam chamar todo consumidor de regressão:

- Trabalho declarado por célula: `test_3c_abrir_ausencia.py:43`, `test_f3_dedup_atraso.py:44`, `test_orfa_sem_candidato.py:41`, `test_emissor_nomeia_o_marco.py:48`, `test_resposta_tardia.py:34` (todos sob `app/chamados/tests/`). Esses testes não dependem exclusivamente do template para declarar trabalho. [nao verificado]
- `test_a2_marco_morto` preserva indefinição explicitamente (`app/ponto/tests/test_a2_marco_morto.py:40`) e tem controle positivo de furo vivo (`:56`, `:68`).
- `test_selo_f4_fatia4` só aplica helper ao construir chamado e se não houver escala (`app/chamados/tests/test_selo_f4_fatia4.py:67`). O teste que declara “sem escala” e espera `procedencia_fina == ambiguo` não chama esse método: cria colaborador sem escala, registra batidas retratadas e consulta diretamente (`:131`, `:136`, `:139`). Portanto não foi ocultado por este helper.
- `test_selo_dedup_premissa_viva` exige chamada criada e estado vivo, não só ausência de erro (`app/chamados/tests/test_selo_dedup_premissa_viva.py:95`, `:98`, `:119`).
- `test_regressao_etapa7` exige exatamente um chamado e distintos para dias diferentes (`app/chamados/tests/test_regressao_etapa7.py:94`, `:115`).

Censo bruto por import encontrou 45 módulos; 27 contêm alguma referência a relógio (`timezone.now`, `localdate` ou `today`), mas só 21 acima têm data do fato relativa explicitamente identificada. Esse censo é resultado de busca, sem arquivo de relatório gravado: **[nao verificado por artefato citável]**. Evite repetir “27 módulos com data relativa a hoje” como comprovado pela cópia.

_593 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
