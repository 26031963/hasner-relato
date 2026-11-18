# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 14:26** · publicado em **18/11/2026 14:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 14:26 · sessao 5f2a628261a9 · PRE-MAPA-E4D · censo · origem=Code

**Censo parcial: não consegui comprovar todos os escritores nem fechar as contagens.** A busca encontrou escritores posteriores ao inventário do anexo, inclusive uma devolução de paridade e a ressuscitação em lote; portanto, seria incorreto apresentar a tabela abaixo como exaustiva. A identidade Git da cópia também está **[nao verificado]**: o diretório fornecido não contém metadados Git.

A unidade pedida é **campo × função**. Nas linhas abaixo, agrupo campos quando compartilham o mesmo sítio para facilitar a leitura; isso não constitui uma contagem de unidades.

### A. Portas verificadas

Caminhos abreviados: `models.py` = [app/chamados/models.py](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py); os serviços ficam em `app/chamados/services/`.

| Porta | Escrita de estado identificada nesta cópia |
|---|---|
| A01 | `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por` e urgência do chamado; a porta também relavra perguntas. A classificação do estado canônico do chamado como projeção a parar é **[nao verificado]**. ([models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [models.py:302](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:302)) |
| A02 | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; há carimbo delegado de pergunta. ([models.py:2082](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2082), [models.py:2095](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2095)) |
| A03 | Os quatro campos de fechamento acima; também grava `reaberturas_admin` e, condicionalmente, `motivos`. A classificação desses dois últimos como projeção a parar é **[nao verificado]**. ([models.py:2312](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2312)) |
| A04 | Os quatro campos de fechamento. ([models.py:2336](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2336)) |
| A05 | `materializada_em`, `materializada_por`, `via_resolucao`; condicionalmente `respondida_em`, `validada_em`, `validada_por` e os três campos de lavra `veredito_lavrado`, `julgado_em`, `janela_expira_em`. A lavra é cache declarado no anexo; sua inclusão entre os campos a parar é **[nao verificado]**. ([models.py:3293](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3293), [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [models.py:3345](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3345)) |
| A06 | `materializada_em`, `materializada_por`, `via_resolucao` e, se fornecido, `recusa_motivo`. ([models.py:3378](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3378)) |
| A07 | Limpa `materializada_em`, `materializada_por`, `via_resolucao`; com `salvar=False`, prepara a alteração em memória para o chamador. ([models.py:3399](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3399)) |
| A08 | Limpa os três campos de A07 e grava os campos recebidos por `**outros`; os chamadores comprovados incluem R/V. ([models.py:3448](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3448), [models.py:3485](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3485), [models.py:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702)) |
| A09 | `via_resolucao`. ([models.py:3496](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3496)) |
| A10 | `validada_em`, `validada_por`. ([validacao.py:168](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:168)) |
| A11 | Limpa `validada_em`, `validada_por`. ([validacao.py:273](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:273)) |
| A12 | `resposta_colab`, `respondida_em`; pode limpar `validada_em`, `validada_por`, preparar descarimbo e gravar `recusa_motivo`. ([disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1494](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1494), [disputa_emissao.py:1511](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1511)) |
| A13 | Delega ao lote a limpeza de `respondida_em`, `resposta_colab`, `validada_em`, `validada_por` e dos três campos de carimbo. ([disputa_emissao.py:1617](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1617), [models.py:3485](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3485)) |
| A14 | `resposta_colab`, `respondida_em`. ([disputa_emissao.py:1682](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1682)) |
| A15 | `respondida_em`. ([disputa_emissao.py:1705](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1705)) |
| A16 | `via_resolucao` e lavra. ([relavra_via.py:125](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:125)) |
| A17 | `pendencia_admin`, `recusa_motivo`; efeitos e carimbos delegados requerem discriminação adicional **[nao verificado]**. ([materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365)) |
| A18 | `recusa_motivo`; no ramo de colisão, delega o carimbo de identidade. ([materializacao.py:1693](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1693)) |
| A19 | `recusa_motivo` e limpeza delegada por A13. ([validacao.py:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916)) |
| A20 | `data_conferida_em`, `data_conferida_por`. ([validacao.py:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996)) |
| A21 | Carimbo de identidade delegado; outros campos de estado próprios da disputa exigem discriminação adicional **[nao verificado]**. ([models.py:2218](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2218)) |
| A22 | Constrói pergunta com `**campos` e aceita os defaults do modelo; a enumeração dos campos efetivamente fornecidos por todos os chamadores está **[nao verificado]**. ([models.py:3012](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3012), [models.py:3206](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3206)) |
| A23 | Grava `reaberturas_colab` e delega correção; a classificação do contador como projeção a parar é **[nao verificado]**. ([models.py:2401](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2401)) |
| A24 | Limpa `resolucao`. ([models.py:2428](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2428)) |
| Porta 27 | `disparar_perguntas_competencia`: o corpo examinado conta e relata os alvos; escrita direta de projeção nesse caminho **[nao verificado]**. ([RELATO.md:2240](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/RELATO.md:2240), [disparar_perguntas_competencia.py:162](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/disparar_perguntas_competencia.py:162)) |

### B–C. Escritores adicionais comprovados — tabela parcial

A coluna “ato” abaixo identifica apenas uma correspondência comprovada no sítio; **não determina** que o ato substitua integralmente todas as propriedades do campo. Onde isso não foi provado, permanece **[nao verificado]**.

| Campo | Escritor | Tipo | Ato da E4a que o substitui | Exceção FATO |
|---|---|---|---|---|
| `validada_em`, `validada_por` | [validacao.py:devolver_paridade_bug_par_103:349](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:349) | outro | A11 **[nao verificado]**; o próprio sítio registra `DEVOLUCAO` | [nao verificado] |
| `materializada_em`, `materializada_por`, `via_resolucao` | [validacao.py:devolver_paridade_bug_par_103:346](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:346), delegando A07 | outro | [nao verificado] | [nao verificado] |
| `respondida_em`, `resposta_colab`, `validada_em`, `validada_por`, `materializada_em`, `materializada_por`, `via_resolucao` | [models.py:ressuscitar_perguntas:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702), por A08 e `**outros` | outro | [nao verificado]; registra `REABERTURA` marcada ([models.py:1694](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1694)) | [nao verificado] |
| `respondida_em`, `resposta_colab`, `materializada_em`, `materializada_por`, `via_resolucao` | [validacao.py:devolver_instante_sem_texto_l130:440](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:440), por A08 e `**outros` | outro | nenhum ato de reabertura/devolução nesse fluxo, conforme o código ([validacao.py:414](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:414)); substituição **[nao verificado]** | [nao verificado] |
| `status_local` | [0038_a3_registrado_backfill.py:registrar:10](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0038_a3_registrado_backfill.py:10), também reversão em `:15` | migration | [nao verificado] | [nao verificado] |
| `status_local`, `via_encerramento` | [0039_a3_via_backfill.py:backfill:24](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0039_a3_via_backfill.py:24), também reversão em `:33`–`:34` | migration | [nao verificado] | [nao verificado] |

A varredura de **signals, comandos, outras migrations, registros de admin, aliases, `setattr`, `save()` e atualizações dinâmicas** não foi concluída; logo, “todo escritor fora das portas” permanece **[nao verificado]**. O arquivo [admin.py](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/admin.py) está vazio, o que por si só não prova ausência de registros ou formulários de admin em outros arquivos. **[nao verificado]**

**Contagens finais:** campos **[nao verificado]**; escritores **[nao verificado]**; escritores sem ato substituto **[nao verificado]**; destes, em exceção FATO **[nao verificado]**.

## 18/11 14:25 · sessao 6a8ed6edd114 · PRE-MAPA-E4D · verificador · origem=Code

**AVAL-DEVOLVIDO: itens 3 e 5.** Os demais itens conferem dentro dos limites abaixo. Os anexos foram fornecidos no enunciado sem numeração de linhas; por isso, suas referências de linha estão **[não verificado]**.

1. **CONFERE** — **Premissas:** E4d e os seis fatos conferem no [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089) e [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091); **ordem:** o censo não altera vagas nem abre Agent, conforme anexo PRE-MAPA, item 1 e proibição [linha não verificada]; **literal:** nenhum impedimento identificado; exaustividade dos escritores **[não verificado]**; **exportada/dinheiro/!:** o item pede leitura e registro, sem alteração declarada [anexo PRE-MAPA, item 1, linha não verificada].

2. **CONFERE** — **Premissas:** CLASSE-D está registrada e a RAIZ liga F5 à E4d ([BACKLOG.md:2025](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2025), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109)); **ordem:** perguntas sem construção não alteram as vagas nem abrem Agent [anexo PRE-MAPA, item 2, linha não verificada]; **literal:** pede perguntas sem respondê-las; **exportada/dinheiro/!:** consulta o destino da exportada, sem autorizar mudança no gravado ([CLAUDE.md:411](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/CLAUDE.md:411)).

3. **PROCEDE** — **Premissas:** B2b e as matrizes F2–F9 constam no [BACKLOG.md:826](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:826) e [BACKLOG.md:2117](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2117); OOM-0917 e o envelope concreto **[não verificado]**; **ordem e literal:** criar script reexecutável é construir instrumento, embora o cabeçalho diga “nenhuma vaga”, e a RAIA reserva instrumento à vaga 2 após esvaziar a fila; falta também explicitar o envio do contrato ao Codex antes da construção ([PISTA.md:170](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/PISTA.md:170); anexos PRE-MAPA item 3 e RAIA item 1, linhas **[não verificado]**); **exportada/dinheiro/!:** o censo AST declarado não prevê alteração desses dados [anexo PRE-MAPA, item 3, linha não verificada].

4. **CONFERE** — **Premissas:** a sequência E4a–E4d consta no [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089); **ordem:** a matriz depende dos itens 1 e 3 e não abre construtor ou Agent [anexo PRE-MAPA, item 4, linha não verificada]; **literal:** sua execução depende da resolução do item 3; **exportada/dinheiro/!:** descreve escritas atuais e futuras, sem autorizar executá-las [anexo PRE-MAPA, item 4, linha não verificada].

5. **PROCEDE** — **Premissas:** E3 consta “PARADA (aguarda lei)” na [GRADE-MESTRA.md:121](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/GRADE-MESTRA.md:121); a lei de 17/11 e seu literal de origem **[não verificado]**; **ordem:** registro não altera vagas nem abre Agent [anexo PRE-MAPA, item 5, linha não verificada]; **literal:** o próprio item determina AVAL-DEVOLVIDO se não encontrado o literal, e os `logs/AVAIS-*` não estão disponíveis nesta cópia **[não verificado quanto ao conteúdo]**; **exportada/dinheiro/!:** pede registro, sem alteração declarada desses dados [anexo PRE-MAPA, item 5, linha não verificada].

## 18/11 14:18 · sessao 459a4922e755 · DST-L2 · sonda · origem=Code

- **(b1)** A grade lê somente justificativas **aprovadas**; elas limpam os furos e marcam o dia como justificado, mas não mudam seu `tipo_dia`. A pendente não entra nesse filtro. Separadamente, uma Ausência pode mudar o tipo do dia. [app/escala/utils.py:1188](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1188), [app/escala/utils.py:1197](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1197), [app/escala/utils.py:1210](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1210), [app/escala/utils.py:1254](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1254).

- **(b2)** O efeito concreto da rejeição após o fecho — voltar a protestar e gerar cobrança por chamado reaberto ou novo — permanece **[não verificado]**: o dado não traz a prova em sombra pela porta real com `atomic+raise`. A porta aceita justificativa pendente, grava rejeitada e seu `save()` tenta acionar novo julgamento; esse julgamento pode não ocorrer. [app/ponto/services/justificativa.py:273](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:273), [app/ponto/signals.py:85](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85), [app/ponto/signals.py:161](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161). O candidato da S1 é **chamado 37191, col507, 2026-11-05**; sua justificativa já estar rejeitada não demonstra a transição pendente → rejeitada requerida para a prova. [DADO RASPADO: `candidato chamado 37191 col507 2026-11-05`; anexo `medida_b_leitura.r`, S1 e S2].

- **(b3)** Universo **35**, igual em quantidade aos **35** do anexo; diferença numérica **0**. Contagens: **pendente 0, aprovada 0, rejeitada 1, nenhuma 34**. Um chamado pode contar em mais de uma categoria de status. [DADO RASPADO: linhas `universo 35` a `nenhuma 34`; anexo `PERGUNTA-DE-DESENHO.md`, “Números”; anexo `medida_b_leitura.r`, S1]. A igualdade dos integrantes e a equivalência exata com a sonda original permanecem **[não verificado]**. A S1 lê a ata gravada pela implementação de `dia_sem_trabalho` disponível na árvore em que roda; já o cartório da cópia usa a ata do julgamento corrente. Uma diferença de código da árvore viva ou de ata poderia afetar a comparação, mas sua ocorrência aqui é **[não verificado]**. [anexo `medida_b_leitura.r`, S1; app/chamados/juizes.py:445](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:445), [app/ponto/services/cartorio.py:1457](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1457).

## 18/11 14:10 · sessao 3a71134a64be · DST-L2 · leitura · origem=Code

**A medida de produção ainda não está completa:** o dado raspado informa 35 chamados e zero justificativas pendentes pelas duas vias, mas não informa as quantidades de aprovadas, rejeitadas ou sem justificativa. [não verificado] A sonda original `sonda_impacto_dstl2.py` não está disponível nesta cópia; portanto, a equivalência exata de uma nova seleção com ela também está **[não verificado]**.

**(b1) Leitor e escritores.** O leitor da justificativa para a grade é `_marcar_ausencias`: ele filtra `Justificativa.status='aprovada'` em [app/escala/utils.py:1194](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1194). Uma justificativa aprovada marca `justificado`, limpa os furos e **não altera o `tipo_dia`**; pendente não entra nesse filtro. [app/escala/utils.py:1192](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1192), [app/escala/utils.py:1254](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1254)

Separadamente, a mesma grade consulta a cobertura de **Ausência** para folha e pode escrever `tipo_dia='ausencia'` ou `'falta'`; o filtro de status dessa via está em `_pelo_efeito`, que para folha aceita ausência aprovada. [app/escala/utils.py:1188](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1188), [app/escala/utils.py:1210](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1210), [app/ponto/turnos.py:2527](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/turnos.py:2527). A ata copia o `tipo_dia` da grade, e `_protesta` pergunta a `dia_sem_trabalho`, com exceção para cobertura parcial. [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:730), [app/ponto/services/cartorio.py:1462](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1462), [app/chamados/juizes.py:454](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:454)

O status da justificativa nasce com padrão `pendente`; entre seus escritores estão a aprovação e rejeição individuais, a aprovação e rejeição em lote e a rejeição pelo reconciliador. [app/ponto/models.py:159](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:159), [app/ponto/services/justificativa.py:205](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:205), [app/ponto/services/justificativa.py:283](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:283), [app/ponto/views.py:1799](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1799), [app/ponto/views.py:1878](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1878), [app/chamados/reconciliador.py:576](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/reconciliador.py:576). Outro leitor da aprovada é `efeitos_da_ausencia`, usado para compor os dias justificados. [app/ponto/services/efeito_ausencia.py:28](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/efeito_ausencia.py:28), [app/ponto/services/efeito_ausencia.py:40](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/efeito_ausencia.py:40)

**(b2) Rejeição depois do fecho.** A porta individual real é `rejeitar_justificativa_dp`, chamada pela view `rejeitar_justificativa`; ela só decide uma justificativa pendente, grava `rejeitada` e chama `save()`. [app/ponto/views.py:1925](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/views.py:1925), [app/ponto/services/justificativa.py:262](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:262), [app/ponto/services/justificativa.py:277](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:277)

Esse `save()` dispara o `post_save` da justificativa, que tenta rejulgar a célula; o handler pode parar se já estiver em julgamento ou se ocorrer erro engolido, e `julgar_celula` também retorna sem julgar em certas condições temporais ou sem célula. [app/ponto/signals.py:161](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:161), [app/ponto/signals.py:85](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/signals.py:85), [app/ponto/services/cartorio.py:2033](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:2033)

A passagem de **pendente para rejeitada**, por si, não retira uma justificativa aprovada do filtro da grade: nenhuma das duas passa nele. [app/escala/utils.py:1194](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/escala/utils.py:1194) Assim, se os demais fatos que produziram o dia sem trabalho continuarem iguais, essa troca não faz o dia voltar a protestar por essa via. [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1464), [app/chamados/juizes.py:454](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:454)

Se o novo julgamento encontrar furo e cumprir os portões de emissão, o cartório chama o emissor, que pode reabrir ou emitir conforme o caso; isso não está demonstrado para um dos 35. [app/ponto/services/cartorio.py:1712](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1712), [app/ponto/services/cartorio.py:1755](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1755), [app/ponto/services/cartorio.py:1773](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1773)

Depois do `save`, a porta individual **tenta criar um novo chamado interno de categoria `justificativa_rejeitada`** e criar follow-up; o gate de criação pode recusá-lo, e isso não prova a existência de cobrança do furo. [app/ponto/services/justificativa.py:289](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:289), [app/chamados/models.py:1101](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:1101), [app/ponto/services/justificativa.py:324](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:324). Pergunta nova e resultado concreto da cobrança: **[não verificado]**.

**(b3) S1 — leitura, texto não executado.** A seleção abaixo interpreta “só pela ata gravada” como `trabalha` diferente de `False`, com `dia_sem_trabalho(celula)` verdadeiro; ela usa o juiz real, inclusive a exceção de ausência parcial. [app/chamados/juizes.py:445](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/juizes.py:445) É uma reconstrução da descrição fornecida, não equivalência comprovada à sonda original **[não verificado]**. As vias são `Justificativa.chamado_origem` e `ChamadoColaborador.justificativa_relacionada`; um chamado com justificativas de status diferentes conta em mais de uma categoria. [app/ponto/models.py:173](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/models.py:173), [app/chamados/models.py:69](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/chamados/models.py:69)

```python
from collections import Counter
from django.db.models import Q
from chamados.catalogo.furo_modulos import FURO_MODULES
from chamados.catalogo.motor import VIVOS
from chamados.juizes import dia_sem_trabalho
from chamados.models import ChamadoColaborador
from ponto.models import Justificativa

totais = Counter()
candidato = None

qs = (
    ChamadoColaborador.objects
    .filter(modulo_origem__in=FURO_MODULES, status_local__in=VIVOS,
            celula__isnull=False)
    .select_related("celula")
    .order_by("pk")
)

for ch in qs.iterator():
    cel = ch.celula
    if cel.trabalha is False or not dia_sem_trabalho(cel):
        continue

    totais["universo"] += 1
    justs = list(
        Justificativa.objects.filter(
            Q(chamado_origem_id=ch.pk) |
            Q(pk=ch.justificativa_relacionada_id)
        ).values_list("pk", "status")
    )
    status = {s for _, s in justs}
    if not justs:
        totais["nenhuma"] += 1
    for s in ("pendente", "aprovada", "rejeitada"):
        if s in status:
            totais[s] += 1
    if candidato is None and justs:
        candidato = (ch.pk, ch.colaborador_id, cel.data)

for chave in ("universo", "pendente", "aprovada", "rejeitada", "nenhuma"):
    print(chave, totais[chave])

if candidato:
    pk, colab_id, dia = candidato
    print("candidato", "chamado", pk, "col" + str(colab_id), dia.isoformat())
else:
    print("candidato nenhum")
print("FIM_SONDA")
```

**S2 — não forneço como pronta para execução.** `_protesta` é uma função local de `julgar_colab`, não uma porta importável, e o fecho depende da ata gerada durante o julgamento. [app/ponto/services/cartorio.py:1435](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1435), [app/ponto/services/cartorio.py:1464](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/cartorio.py:1464) Além disso, a porta real tenta enviar push fora de seu `atomic` interno; um `raise` em transação externa desfaz escritas de banco, mas não desfaz uma notificação externa. [app/ponto/services/justificativa.py:273](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:273), [app/ponto/services/justificativa.py:308](/home/ronald/codex_ws/dfb866cf8afcca2c40590bbbb2a896592cf7f061/app/ponto/services/justificativa.py:308) Não verifiquei todos os efeitos externos possíveis do julgamento e do emissor, nem uma forma de capturar o resultado da função local sem alterar a observação; apresentar uma sonda como segura e fiel seria **[não verificado]**. A prova em sombra, portanto, permanece pendente.

## 18/11 14:09 · sessao 1b52d7327d1d · RAIA-ESPURIAS-1811-1410 · verificador · origem=Code

Os anexos foram fornecidos como texto, sem numeração de linhas; por isso, as referências a eles aparecem com **[não verificado]** quanto a arquivo:linha. A cópia disponível não contém metadados Git para conferir os hashes ou executar o merge-tree. [não verificado]

**Item 1 — CONFERE, condicionado às medições antes de cada abertura:** a ordem foi declarada e a exceção ao complemento é nominal; os cruzamentos informados ainda não foram verificados nesta cópia. [não verificado]

1. **Premissas:** as medições de DST-L2 e ESPURIA-TELA e o recorte de ESCALA não puderam ser conferidos por merge-tree. [não verificado]
2. **Aval vigente:** a nova ordem depois das 18h é uma exceção nominal ao item 1c do complemento; a restrição anterior a esse horário permanece. [não verificado]
3. **Literal:** a abertura depende de medir contra a ponta da espinha em voo; arquivo `.py` comum exige esperar o pouso ou entrar no mesmo comboio. A medição antiga não substitui essa condição. [não verificado] [nao verificado]
4. **Exportada, dinheiro ou `!`:** o item proíbe tocar exportada e declara o custo de parar instrumentos; efeito das obras sobre dinheiro ou outras matérias de `!` ainda depende dos diffs. [não verificado]

**Item 2 — CONFERE, condicionado ao pouso após a simulação e ao smoke:** o critério do “desde” e o marco de pouso repetem a resposta do aval vigente. [não verificado] [nao verificado]

1. **Premissas:** L-040 existe em [LEIS.md:88](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/LEIS.md:88). A referência do anexo a `esmeril_espelho.py:238` não corresponde ao cálculo do “desde” nesta cópia: ali está `p = r['proposta']`, em [esmeril_espelho.py:238](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/esmeril_espelho.py:238). O recorte da obra e seu censo ainda precisam ser verificados. [não verificado]
2. **Aval vigente:** a volta ao construtor, o menor “desde” ou INCOMPLETO e o pouso após a simulação com smoke constam da resposta anterior; a nova ordem é a exceção nominal ao complemento. [não verificado] [nao verificado]
3. **Literal:** comboio com a espinha, se pronta na mesma meia hora, continua sujeito ao pouso de ESCALA somente depois da simulação; a viabilidade temporal do comboio é [não verificado]. [nao verificado]
4. **Exportada, dinheiro ou `!`:** o aval proíbe tocar exportada; o diff ainda é necessário para verificar outros efeitos. Alterar dados de escala ou vínculo exige `!`, conforme [CLAUDE.md:265](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/CLAUDE.md:265). [não verificado]

**Item 3 — CONFERE para a decisão e a medição, sem considerar o pouso liberado:** a cópia confirma o precedente para `trabalha is False`; os resultados pedidos em b1–b3 ainda não estão completos. [cartorio.py:1451](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/cartorio.py:1451)

1. **Premissas:** `_protesta` exclui `trabalha is False` e o fechamento usa `reconciliar` em [cartorio.py:1458](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/cartorio.py:1458) e [cartorio.py:1533](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/cartorio.py:1533). O comportamento específico da cópia DST-L2, a reação à rejeição e a distribuição completa dos 35 são [não verificado]. O anexo informa zero pendentes por `chamado_origem` e por ponteiro, sem fornecer a contagem de aprovadas e nenhuma. [não verificado]
2. **Aval vigente:** não foi identificada colisão nos dois avais fornecidos; o item mantém a DST-L2 sem pouso até a medida. [não verificado] [nao verificado]
3. **Literal:** b2 exige prova pela porta real de rejeição em sombra com rollback; a existência da porta é visível em [justificativa.py:262](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/justificativa.py:262), mas seu resultado no caso pedido é [não verificado].
4. **Exportada, dinheiro ou `!`:** a medição pedida é em leitura e sombra; eventual cobrança ou cura após rejeição ainda não foi demonstrada. O tratamento de gravado exportado segue sujeito a `!` em [CLAUDE.md:265](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/CLAUDE.md:265). [não verificado]

**Item 4 — CONFERE para registrar o dry:** o apply permanece dependente do `!` declarado. [não verificado]

1. **Premissas:** o universo dos afetados, o número de órfãs e a situação da competência 11 não constam do dado fornecido. [não verificado]
2. **Aval vigente:** o dry foi marcado para depois da simulação, compatível com a proibição anterior de regenerar células antes dela. [não verificado]
3. **Literal:** reversão e contagem antes/depois são condições do dry; sua execução e seus resultados são [não verificado].
4. **Exportada, dinheiro ou `!`:** o registro não autoriza apply; o aval reserva o apply ao `!` com o número em mãos e proíbe tocar exportada. [não verificado]

## 18/11 13:53 · sessao 9e7ece3f7811 · LAUDO-[nome]-1811-1340 · sonda · origem=Code

O retrato de col830 foi calculado em **18/11/2026 às 04:37:31,642334, UTC−3**. A hora do pouso da S6 e a ordem entre os dois eventos estão **[não verificado]**. (DADO RASPADO, linha `col830 retrato_calculado_em`; hora da S6: [não verificado]) [nao verificado]

- **Retrato de 15/11:** `sem_decisao`, 713 minutos, com uma ponta às 06:50, `marco=None`, `sentido=dia` e 713 minutos. (DADO RASPADO, linha `col830 retrato_dia`)
- **DecisaoHE:** a consulta retornou lista vazia para o dia. (DADO RASPADO, linha `col830 decisoes`)
- **Batidas apuráveis na data local:** 06:50:03,757330 e 18:53:09,580000, ambas em UTC−3. (DADO RASPADO, linha `col830 batidas_apuraveis_data_local`)
- **Espelho:** `he_fora_da_janela=[]` e zero minutos. (DADO RASPADO, linha `col830 espelho_dia`)
- **Lavratura dry:** a sonda reproduziu o critério de inclusão e apontou `entra=False`, sem minutos; o estado do dry completo permanece **[não verificado]**. O comando sem `--apply` é dry, mas a sonda não executou o comando completo. (DADO RASPADO, linha `col830 dry_inclusao_dia`; anexo `laudo2_d_leitura.r`, seção 3; [lavrar_he_pendente.py:25](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/lavrar_he_pendente.py:25); [he_pendente_lavrado.py:132](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/he_pendente_lavrado.py:132))

Se a Gestão enviar os **713 minutos** do retrato e as validações passarem, a porta gravará `DecisaoHE` autorizada com `minutos_na_decisao=713` e origem `admin`: ela recebe os minutos do formulário e não os recalcula. A lista vazia observada é a situação no momento da sonda; uma decisão vigente igual no momento do ato faria a porta retornar sem alterar a foto. ([gestao_he.py:270](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/gestao_he.py:270); [gestao_he.html:256](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/ponto/gestao_he.html:256); [he.py:159](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:159); [he.py:195](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:195))

O motor carrega **data e estado**, não a foto de 713 minutos. A autorização pode alterar o cálculo quando as condições dos cortes ou do bloqueio de dia sem previsto se aplicam; **se ela mudaria o valor deste dia ou moveria dinheiro está [não verificado]** com o dado fornecido. ([motor_calculo_v2.py:1397](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1397); [motor_calculo_v2.py:1448](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1448); [motor_calculo_v2.py:1691](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1691); [motor_calculo_v2.py:1733](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1733))

**Dono L-099: [não verificado].** O material mostra a divergência entre retrato e espelho, mas não estabelece a hora da S6 nem a causa dessa divergência. (DADO RASPADO, linhas `col830 retrato_dia` e `col830 espelho_dia`; causa e hora da S6: [não verificado])

## 18/11 13:51 · sessao 266cc9c807c3 · LAUDO-[nome]-1811-1340 · sonda · origem=Code

A sonda terminou com os três marcadores esperados, `FIM_SONDA_ENVELOPE` e `rc_sombra=0`; não há traceback no dado fornecido. [dado raspado, linhas 11, 18, 25–27]

Para localizar as referências abaixo, conto as linhas a partir de `sonda_leitura` como linha 1, incluindo a linha em branco seguinte. [dado raspado, linhas 1–3]

A sonda recalculou o motor em leitura para a competência 11/2026 e agrupou os dias com minutos positivos de intrajornada e escala com `intervalo_indenizavel=False`, pela quantidade de batidas apuráveis. A flag `False`, por si só, não comprova que todos esses dias tenham intervalo **pre-assinalado**; a sonda não apresentou a modalidade da escala. [dado raspado, anexo `laudo2_c_leitura.r`: código de `linha_motor` e do censo; `pre-assinalado`: não verificado]

| Batidas apuráveis | Dias | Minutos do motor |
|---|---:|---:|
| 2 | 335 | 20.264,00 |
| 3 | 169 | 10.444,00 |
| 4 ou mais | 216 | 6.196,86 |
| **Subtotal pedido** | **720** | **36.904,86** |

Os valores da tabela são os emitidos pela sonda; o subtotal é a soma das três linhas. [dado raspado, linhas 6–8] A sonda encontrou ainda **1 dia com uma batida e 60,00 minutos**, nenhum com zero batidas, nenhum dia sem escala e zero erros de colaboradores; incluindo a classe de uma batida, são **721 dias e 36.964,86 minutos**. [dado raspado, linhas 4–10]

- **col868, 14/11:** confirmados 2 batidas apuráveis, 60 minutos no motor, flag `False` e 60 minutos em `DiaPago`. A pergunta consultada como 39550 retornou `mesma_pessoa=False`, com evento e célula em 03/10; portanto, não confirma associação dessa pergunta ao caso. [dado raspado, linhas 12–14; anexo `laudo2_c_leitura.r`, tupla de conferência]
- **col934, 15/11:** confirmados 2 batidas apuráveis, 60 minutos no motor, flag `False` e 60 minutos em `DiaPago`. A pergunta consultada como 39556 também retornou `mesma_pessoa=False`, com evento e célula em 03/10; portanto, não confirma associação dessa pergunta ao caso. [dado raspado, linhas 19–21; anexo `laudo2_c_leitura.r`, tupla de conferência]

O aval pede comparação com **151/152**, e a leitura anterior descreve 151 dias `False` em um recorte de 152 dias com marcação ímpar; o arquivo original `os152.r` e suas linhas não estão disponíveis para verificar esses números e filtros. [dado raspado, anexo `laudo_fernando_1811_1340.txt`, item 1(c); anexo `laudo2_c_leitura.r`, parágrafo sobre os152; não verificado] Sob essa descrição, o recorte ímpar deixava de fora os dias pares, incluindo a classe de **335 dias com duas batidas** medida agora; a classe `4+` mistura quantidades pares e ímpares, sem abertura suficiente para contar quantos ficaram fora. [dado raspado, linhas 6 e 8; não verificado: filtro original os152] Não é possível obter a interseção ou o acréscimo exato por subtração: a leitura anterior informa que os152 usava `DiaPago` gravado, enquanto esta sonda mede o motor recalculado. [dado raspado, anexo `laudo2_c_leitura.r`, parágrafo final; não verificado: execução e linhas originais de os152]

**Dono L-099: [não verificado].** O código declara que o motor não lê `intervalo_indenizavel` por decisão expressa; os resultados apresentados não demonstram, por si, uma divergência classificada pelo juiz de dono nem permitem atribuí-la a ESTRUTURA, CADASTRO ou BATIDA. [motor_calculo_v2.py:822](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:822), [CLAUDE.md:179](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/CLAUDE.md:179); [não verificado]

## 18/11 13:50 · sessao dc9a98c9df5d · LAUDO-[nome]-1811-1340 · leitura · origem=Code

**Resultado:** não encontrei gerador de AEJ nem de AFD identificado como tal na cópia. O registro do projeto também declara ausente o gerador de AEJ, e o glossário da interface declara pendente a exportação de AFD. A busca por nome não exclui, por si só, uma implementação sob outro nome. ([RELATO.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/RELATO.md:13); [BACKLOG.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/BACKLOG.md:2340); [glossario.html](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/core/glossario.html:154))

O requisito legal detalhado da Portaria 671 para este desenho ainda não foi fornecido no material da pergunta; portanto, **a lista completa de lacunas para conformidade, inclusive layout, conteúdo, integridade e assinatura, é [não verificado]**. O que se pode afirmar pela cópia é que falta um gerador de AEJ identificado e que os artefatos existentes abaixo não demonstram essa geração. ([BACKLOG.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/BACKLOG.md:2340))

**Inventário AEJ e AFD**

| Componente | O que faz e fonte | Chamador e exposição |
|---|---|---|
| Importação AEJ atual | Não gera nem importa arquivo: retorna uma página informando a descontinuação, com HTTP 410. Não lê batidas, célula ou motor. ([views.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/views.py:54)) | A rota `importar-aej/` chama `importar_aej`, que exige login. A página descontinuada é a resposta efetiva da rota. ([urls.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/urls.py:9); [views.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/views.py:56)) |
| Importador AEJ legado | `processar_aej`, em arquivo `.disabled`, lia TXT ou TXT dentro de ZIP do Nexti, identificava registros de empresa, colaborador, escala e batida e gravava batidas com origem `aej`; era importação, não geração. ([aej_importer.py.disabled](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/legacy/aej_importer.py.disabled:9); [mesmo arquivo](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/legacy/aej_importer.py.disabled:140)) | A view histórica no mesmo arquivo o chamava; a view ativa não o chama. O README registra a descontinuação e risco de associação por matrícula reutilizada. ([aej_importer.py.disabled](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/legacy/aej_importer.py.disabled:185); [README.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/legacy/README.md:8)) |
| Modelo de importação | `ImportacaoAEJ` guarda arquivo recebido, estado, contadores, log, operador e datas; não é um modelo de AEJ gerado. `Batida` admite origem `aej`. ([models.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/models.py:200); [models.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/models.py:8)) | O legado usava o modelo; há registro no admin em modo somente leitura. A rota ativa não executa a importação. ([aej_importer.py.disabled](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/legacy/aej_importer.py.disabled:173); [admin.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/admin.py:25); [views.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/views.py:57)) |
| Formulário e manual antigos | O template ainda descreve upload TXT/ZIP e histórico; o manual ainda anuncia a importação. São textos remanescentes, divergentes da resposta 410 da view ativa. ([importar_aej.html](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/ponto/importar_aej.html:8); [manual.html](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/core/manual.html:296); [views.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/views.py:64)) |
| AFD | O glossário afirma que a exportação automática está pendente. Não localizei modelo, view, rota, comando ou teste **identificado por AFD**; fonte, chamador e download de gerador AFD: [não verificado]. ([glossario.html](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/core/glossario.html:154)) | O glossário é informativo: tem rota, view e link no menu; não é uma tela de geração. A afirmação nele sobre suficiência de evidência alternativa é [não verificado]. ([urls.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/core/urls.py:26); [views.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/core/views.py:327); [base.html](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/base.html:274)) |
| Comandos e testes que mencionam AEJ | O comando de smoke cria batidas de teste com origem `aej`; outro comando contabiliza importações. Há teste estrutural da porta e do admin e um teste que usa essa origem. Eles não demonstram geração de AEJ. Não localizei teste de gerador identificado como AEJ. ([gerar_batidas_smoke_motor.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/gerar_batidas_smoke_motor.py:5); [zerar_dados_operacionais.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/zerar_dados_operacionais.py:50); [test_chokepoint_batida_gate.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/tests/test_chokepoint_batida_gate.py:73); [test_e3_status_lavrado.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/tests/test_e3_status_lavrado.py:186)) |

**Fontes e saídas existentes que não demonstram ser AEJ**

- `Batida` guarda, entre outros campos, colaborador, instante, tipo, origem e informações de retratação, dispositivo e localização. Isso identifica dados disponíveis no modelo; sua suficiência para AFD ou AEJ é **[não verificado]**. ([models.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/models.py:8); [models.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/models.py:58))

- O espelho usa `autoridade_do_periodo` para obter batidas e resultado, monta os dias e consulta `folha_manda` para os totais; é chamado pela tela de ponto e pelo cartão PDF. O PDF avulso está exposto por rota e view, mas gera PDF de espelho, não demonstra gerar AEJ. ([espelho.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/espelho.py:1225); [espelho.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/espelho.py:1262); [espelho.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/espelho.py:1283); [ponto/views.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/views.py:160); [pdf_espelho.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/relatorios/pdf_espelho.py:596); [relatorios/urls.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/relatorios/urls.py:28))

- O TXT da folha é outro fluxo: `gerar_dominio_txt` chama `montar_linhas_export`, cujo módulo descreve como entrada fechamento já lavrado e célula soberana. A rota o expõe na interface de folha; o código citado não o identifica como AEJ. ([folha/views.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/folha/views.py:264); [export.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/folha/export.py:4); [folha/urls.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/folha/urls.py:14))

**Juízes a consultar no desenho**, conforme as perguntas que o requisito vier a exigir — a obrigatoriedade de cada campo para AEJ permanece **[não verificado]**:

- Geometria e pareamento de turnos, marcos da célula e passagem pela meia-noite; grade e alinhamento dos marcos; núcleo e cálculo. ([JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:20))
- Dia de trabalho, precedência, ausência, feriado, minutos previstos e status lavrado; vínculo que vale no dia e sua cobertura. ([JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:123); [JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:142))
- Régua CCT e aplicação, se os campos pedirem resultados sujeitos a ela; janela de folha, se a pergunta exigir competência; fechamento e classificação do TXT somente se o requisito alcançar essas perguntas. ([JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:29); [JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:35); [JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:114))

O próprio índice manda conferir o registro vivo quando houver divergência. Há uma diferença relevante: a tabela diz que competência fechada está sem juiz, enquanto o registro vivo já declara `competencia_fechada`; essa pergunta deve ser conferida no registro vivo se entrar no desenho. ([JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:14); [JUIZES.md](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/docs/JUIZES.md:114); [juizes.py](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/core/juizes.py:311))

## 18/11 13:50 · sessao 81bf612d48ec · LAUDO-[nome]-1811-1340 · leitura · origem=Code

**Campos dos modelos**, antes do rastreamento das funções:

- `MetricaSnapshot`: `chave`, `escopo`, `escopo_id`, `data_ini`, `data_ref`, `numerador`, `denominador`, `valor`, `detalhe`, `calculado_em`. [app/inteligencia/models.py:81](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/inteligencia/models.py:81)
- `DecisaoHE`: `colaborador`, `data`, `estado`, `minutos_na_decisao`, `motivo`, `decidida_por`, `origem`, `decidida_em`, `atualizada_em`, `revogada_em`, `revogada_motivo`, `minutos_na_revogacao`. [app/ponto/models.py:1300](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/models.py:1300)
- `Batida`: `colaborador`, `timestamp`, `tipo`, `sequencia`, `is_intervalo`, `terminal`, `tipo_registro`, `escala`, `pergunta_origem`, `origem`, `canal`, `justificativa`, `latitude`, `longitude`, `gps_accuracy`, `aprovada`, `retratada_em`, `aprovada_por`, `aprovada_em`, `timestamp_dispositivo`, `timestamp_gap_ms`, `sincronizado_em`, `foto`, `criado_em`, `ordem_substituicao`, `posto_efetivo`. [app/ponto/models.py:22](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/models.py:22)
- `CelulaDia`: `colaborador`, `data`, `trabalha`, `origem`, `escala_geradora`, `dna`, `gerada_em`, `regenerada_em`, `regeneracoes`, `dna_anterior`, `editada_em`, `editada_por`, `julgada_em`, `impressao`, `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em`, `status_dia`, `status_rotulo`. [app/escala/models.py:1681](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/escala/models.py:1681) [app/escala/models.py:1770](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/escala/models.py:1770)

**1. Lavratura.** A função que grava é `ponto.services.he_pendente_lavrado.lavrar`: ela faz `update_or_create` de `MetricaSnapshot` e guarda o retrato em `detalhe`; `calculado_em`, atualizado automaticamente, marca a hora da gravação. [app/ponto/services/he_pendente_lavrado.py:177](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/he_pendente_lavrado.py:177) [app/inteligencia/models.py:95](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/inteligencia/models.py:95)

O comando é `lavrar_he_pendente`; no pipeline, `config/crons.py` o encadeia após `lavrar_deslogados` com `--apply`. [app/ponto/management/commands/lavrar_he_pendente.py:18](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/lavrar_he_pendente.py:18) [app/config/crons.py:1572](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/config/crons.py:1572)

Existe dry: **omitir `--apply`**; não há um flag `--dry` declarado. [app/ponto/management/commands/lavrar_he_pendente.py:21](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/lavrar_he_pendente.py:21) O dry não grava, mas pode chamar a porta de revogação, que executa `select_for_update` antes de retornar; por isso, não é executável integralmente sob a transação `READ ONLY` pedida, nem atende à restrição de não chamar porta de decisão. [app/ponto/management/commands/lavrar_he_pendente.py:56](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/lavrar_he_pendente.py:56) [app/ponto/portas/he.py:473](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:473)

**2. Autorização sobre retrato velho.** Na Gestão, os dias e minutos vêm do retrato, enquanto o estado da decisão é consultado vivo; o formulário envia seus minutos no campo oculto `minutos`. [app/ponto/services/gestao_he.py:270](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/gestao_he.py:270) [app/templates/ponto/gestao_he.html:256](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/ponto/gestao_he.html:256)

A view passa esse valor à porta, que **não consulta novamente o motor ou o espelho**: valida permissão, motivo, data e minutos positivos. [app/ponto/views.py:512](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/views.py:512) [app/ponto/portas/he.py:159](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:159) Se o formulário enviar 713, as validações passarem e não houver autorização vigente igual, cria ou atualiza a decisão com `minutos_na_decisao=713` e `origem=admin`; se já houver autorização vigente, retorna sem alterar a foto existente. [app/ponto/portas/he.py:186](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:186) [app/ponto/portas/he.py:65](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:65)

Uma decisão nova ou trocada solicita recálculo do fechamento para a data. [app/ponto/portas/he.py:205](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/portas/he.py:205) O motor lê **data e estado** das decisões vigentes, não os 713 minutos fotografados; autorização pode impedir o corte das pontas e também afastar o bloqueio de dia sem previsto quando suas condições se aplicam. [app/ponto/motor_calculo_v2.py:1397](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1397) [app/ponto/motor_calculo_v2.py:1691](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1691) [app/ponto/motor_calculo_v2.py:1733](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1733) [app/ponto/motor_calculo_v2.py:1448](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1448)

Portanto, **a autorização pode mudar o cálculo, mas o código e o dado fornecido não provam mudança de dinheiro neste caso**: é preciso comparar o cálculo antes e depois, sem efetuar a autorização. [app/ponto/motor_calculo_v2.py:1630](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:1630) [não verificado] A hora do retrato, a hora do pouso da S6 e a relação entre ambas também estão **[não verificado]**.

**3. Sonda para a sombra.** Ela consulta o mesmo espelho e aplica ao dia o critério de inclusão de `apurar`: colaborador ativo, dia na janela e lista `he_fora_da_janela` não vazia. [app/ponto/services/he_pendente_lavrado.py:89](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/he_pendente_lavrado.py:89) [app/ponto/services/he_pendente_lavrado.py:114](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/he_pendente_lavrado.py:114) Ela mostra a **inclusão e os minutos** que o dry produziria para o dia, sem executar o callback de revogação; assim, o estado que o dry completo atribuiria à linha fica **[não verificado]**. [app/ponto/management/commands/lavrar_he_pendente.py:48](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/management/commands/lavrar_he_pendente.py:48)

Cole no `manage.py tenant_command shell --schema=juliani`. A sonda não foi executada aqui. [não verificado]

```python
import datetime as dt
import logging
from django.db import connection, transaction
from django.utils import timezone
from colaboradores.models import Colaborador
from ponto import janelas
from ponto.models import DecisaoHE
from ponto.services.espelho import espelho_do_colab
from ponto.services.he_pendente_lavrado import (
    data_do_dia, ler, minutos_fora_do_dia,
)
from ponto.turnos import batidas_apuraveis

logging.disable(logging.CRITICAL)
DIA = dt.date(2026, 11, 15)

def saida(chave, valor):
    print("col830", chave, valor)

try:
    with transaction.atomic():
        try:
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION READ ONLY")

            c = Colaborador.objects.select_related("empresa").get(pk=830)
            empresa = c.empresa
            ini, fim = janelas.janela_fechamento(11, 2026, empresa)
            retrato, calculado_em = ler(empresa, 11, 2026)

            saida(
                "retrato_calculado_em",
                timezone.localtime(calculado_em).isoformat()
                if calculado_em else None,
            )

            linhas_retrato = [
                {"data": d.get("data"), "estado": d.get("estado"),
                 "minutos": d.get("minutos"), "pontas": d.get("pontas")}
                for col in (retrato or {}).get("colabs", [])
                if col.get("colab") == c.pk
                for d in col.get("dias", [])
                if data_do_dia(d) == DIA
            ]
            saida("retrato_dia", linhas_retrato)

            decisoes = list(
                DecisaoHE.objects.filter(colaborador=c, data=DIA)
                .values("estado", "origem", "minutos_na_decisao", "revogada_em")
            )
            for d in decisoes:
                if d["revogada_em"]:
                    d["revogada_em"] = timezone.localtime(
                        d["revogada_em"]
                    ).isoformat()
            saida("decisoes", decisoes)

            batidas = [
                timezone.localtime(b.timestamp).isoformat()
                for b in batidas_apuraveis(c).order_by("timestamp")
                if timezone.localtime(b.timestamp).date() == DIA
            ]
            saida("batidas_apuraveis_data_local", batidas)

            agora = timezone.now()
            esp = espelho_do_colab(c, fim, ini, fim, ano=2026, agora=agora)
            dias = [d for d in (esp.get("dias") or [])
                    if data_do_dia(d) == DIA]

            for d in dias:
                pontas = d.get("he_fora_da_janela") or []
                saida("espelho_dia", {
                    "data": str(DIA),
                    "he_fora_da_janela": pontas,
                    "minutos": minutos_fora_do_dia(d),
                })

            if not dias:
                saida("espelho_dia", "ausente")

            entra = (
                c.situacao == "ativo"
                and ini <= DIA <= fim
                and any(d.get("he_fora_da_janela") for d in dias)
            )
            saida("dry_inclusao_dia", {
                "entra": entra,
                "minutos": [
                    minutos_fora_do_dia(d) for d in dias
                    if d.get("he_fora_da_janela")
                ],
                "estado_dry_completo": "nao verificado",
            })
        finally:
            transaction.set_rollback(True)
except Exception as exc:
    # Não imprimir mensagem nem traceback: podem conter dados pessoais.
    saida("erro_tipo", type(exc).__name__)
finally:
    print("FIM_SONDA_D")
```

A sonda não identifica a hora do pouso da S6: esse dado não está no material fornecido. [não verificado] [nao verificado]

## 18/11 13:50 · sessao 1e00446590f1 · LAUDO-[nome]-1811-1340 · sonda · origem=Code

Para localizar o dado, conto as linhas a partir de `sonda_leitura` como linha 1, incluindo a linha em branco: as cinco seções têm `FIM_SONDA_`, não há traceback e `rc_sombra=0`. [Dado raspado, linhas 1–13]

Para **col882 em 21/10/2026**, a competência identificada é **11/2026**, de 21/10 a 20/11. [Dado raspado, linha 3]

- **Espelho:** `espelho_do_colab` chama `montar_dias`, que usa `marcar_pontas_fora`; essa função testa a ponta e arredonda os minutos para inteiro. A sonda encontrou **10 min antes**. [app/ponto/services/espelho.py:1194; app/ponto/services/espelho.py:721; app/ponto/janela_he.py:215; dado raspado, linha 5]
- **Motor:** o cálculo passa por `_janela_do_par`, que chama `_entrada_efetiva` e `_saida_efetiva`. A sonda encontrou **9,598966666666666 min fora da janela** em um período atribuído ao dia, sem períodos sem atribuição. [app/ponto/motor_calculo_v2.py:1752; dado raspado, linha 5]
- **Gestão:** `he_pendente_lavrado.ler` lê o retrato e `gestao_he.enriquecer` usa `estado_por_dia` para sobrepor a decisão vigente. O dia aparece com **10 min** e estado **`nao`**; o retrato foi calculado em **18/11/2026 às 07:37:31,642334 UTC**. [app/ponto/views.py:932; app/ponto/services/gestao_he.py:245; app/ponto/services/gestao_he.py:278; dado raspado, linha 7]
- **PDF:** `_coletar_dados_espelho_mes` chama o coletor, que usa `espelho_do_colab`; o PDF lê `he_fora_da_janela`. A sonda encontrou **10 min antes**. [app/relatorios/pdf_espelho.py:654; app/relatorios/pdf_espelho.py:596; app/relatorios/pdf_espelho.py:932; dado raspado, linha 11]

Há **uma DecisaoHE vigente**, com estado **`nao`**, origem **`sistema`** e **10 min na decisão**. `nao` representa decisão de não autorizar, ou ciência registrada; a origem informada é do sistema. [Dado raspado, linha 9; app/ponto/models.py:1283; app/ponto/models.py:1299]

A sonda encontrou tanto os **9,598966666666666 min** do motor quanto os **10 min** do aviso no mesmo recorte; ela não demonstra, por si só, que o aviso resultou do arredondamento desse valor específico ou que os dois cálculos usaram entradas idênticas. [Dado raspado, linha 5; não verificado]

**Dono L-099: ESTRUTURA**, para a divergência de estado exibido: o template do espelho imprime “sem decisão” fixamente, embora a consulta tenha retornado decisão vigente `nao` para o dia. [app/templates/ponto/espelho.html:189; dado raspado, linha 9; app/docs/LEIS.md:122]

## 18/11 13:49 · sessao 8801768e4dda · - · - · origem=Ronald (tmux)

Consulta concluída e evidências enviadas ao agente principal, incluindo a divergência entre o documento de consulta e o registro vivo, os juízes pertinentes por pergunta e a distinção entre batida, célula e apuração. Referências: `app/docs/JUIZES.md:7`, `app/core/juizes.py:191`, `app/ponto/models.py:8`, `app/escala/models.py:1635` e `app/ponto/juiz_batida.py:1`.

## 18/11 13:49 · sessao db0136e12d1d · - · - · origem=Ronald (tmux)

Investigação de AEJ concluída e evidências encaminhadas ao agente principal.

## 18/11 13:49 · sessao 4560eac91331 · - · - · origem=Ronald (tmux)

Investigação de AFD enviada ao agente principal, com referências para a página informativa, sua rota e os campos de `Batida` disponíveis. Não foi identificado gerador nomeado AFD na busca textual; a conformidade legal permanece [não verificado].

## 18/11 13:49 · sessao cb451f5814dc · LAUDO-[nome]-1811-1340 · sonda · origem=Code

**O dado não fecha uma causa única nem um dono L-099: [não verificado].** A ausência na Gestão medida pela sonda decorre de col868 não constar no retrato lido; a razão pela qual não consta nele não foi estabelecida. [dado raspado, linhas 12 e 15; código: app/ponto/services/gestao_he.py:236]

Para as referências ao dado raspado abaixo, conto `sonda_leitura` como linha 1.

- **Retrato:** foi calculado em 18/11/2026 às 07:41:00 UTC, ou 04:41:00 no fuso UTC−3; sua janela é 21/10 a 20/11 e inclui 14/11, mas col868 não consta no retrato. Portanto, o dia não consta sob col868 nesse retrato. A data da mudança não foi fornecida, então não se pode concluir se o retrato é anterior a ela. [dado raspado, linhas 8–12; data da mudança: não verificado]
- **Contas atuais:** o espelho devolveu o dia com lista de pontas vazia e total de **0 min**; o período do motor com entrada em 14/11 às 18:42 e saída em 15/11 às 07:03 também publicou **0,0 min** fora da janela. Esses resultados atuais não reproduzem os 18 e 3 minutos relatados no aval; o instante e as condições daquela observação não foram medidos. [dado raspado, linhas 18–21; anexo, item 1(a); condições anteriores: não verificado]
- **Diferença entre as contas:** no trecho indicado, o motor soma os minutos retirados da entrada e da saída; o montador do espelho marca pontas da grade e acrescenta pontas publicadas pelo motor. O resultado fornecido não identifica o ramo do motor usado para esse período, nem demonstra que uma divergência entre as contas causou a ausência observada. [app/ponto/motor_calculo_v2.py:2759](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:2759) [app/ponto/motor_calculo_v2.py:2787](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/motor_calculo_v2.py:2787) [app/ponto/services/espelho.py:721](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/ponto/services/espelho.py:721) [ramo e causa: não verificado]
- **Cadastro e filtros:** na leitura, col868 estava ativo, sem data de demissão, e o limite cadastrado era 15 min. Mesmo sem filtros, a Gestão não retornou col868; logo, filtro de tela não explica essa ausência na leitura medida. O estado cadastral e os filtros no momento da observação anterior são **[não verificado]**. [dado raspado, linhas 7 e 15]
- **Decisão:** a consulta não encontrou decisões para o dia e o leitor da Gestão retornou `sem_decisao`. O texto “sem decisão” do espelho, isoladamente, é fixo no template e não prova o estado. [dado raspado, linhas 13–14] [app/templates/ponto/espelho.html:189](/home/ronald/codex_ws/b9ba4228280aafe7b1358ac870ee8bec3a6cf4d7/app/templates/ponto/espelho.html:189)
- **Empresa:** `empresa_corresponde_jsp=False` significa apenas que a sonda não encontrou a sequência “jsp” na razão social nem no nome fantasia da empresa de col868; a identidade da empresa e a correspondência com a Gestão observada são **[não verificado]**. [dado raspado, linha 7; anexo, código da sonda `cadastro()`]

**Dono L-099: [não verificado].** O retrato não contém col868 e a leitura atual não publica pontas, mas faltam a data da mudança e a causa da diferença em relação à observação de 18 + 3 minutos para atribuir o caso a ESTRUTURA, CADASTRO ou BATIDA. [dado raspado, linhas 12 e 19–21; anexo, item 1(a); causa: não verificado]

## 18/11 13:42 · sessao 411db8141832 · LAUDO-[nome]-1811-1340 · leitura · origem=Code

Os dados fornecidos **não permitem informar o total de dias e minutos do universo pedido**, nem confirmar os dois casos nominais: o anexo os152 traz um subconjunto de marcações ímpares e não inclui as linhas completas dos casos. A sonda abaixo prepara essa medição; ela não foi executada aqui. [não verificado]

## Campos antes da leitura

Campos usados diretamente pela sonda e pelas funções chamadas, com suas definições:

- `Colaborador.empresa`: [colaboradores/models.py:321](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:321); a identificação `pk` é a chave primária implícita do modelo. [colaboradores/models.py:283](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:283)
- `Batida.colaborador`, `timestamp`, `tipo` e `retratada_em`: [ponto/models.py:22](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:22), [ponto/models.py:23](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:23), [ponto/models.py:24](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:24), [ponto/models.py:67](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:67).
- `DiaPago.colaborador`, `mes`, `ano`, `data`, `tipo`, `versao` e `horas_intra_indenizada`: [ponto/models.py:315](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:315), [ponto/models.py:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:317), [ponto/models.py:318](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:318), [ponto/models.py:319](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:319), [ponto/models.py:321](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:321), [ponto/models.py:322](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:322), [ponto/models.py:337](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:337).
- `TipoEscala.intervalo_modo`, `intervalo_indenizavel`, `intervalo_duracao_min` e horários de intervalo: [escala/models.py:103](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:103), [escala/models.py:108](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:108), [escala/models.py:110](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:110), [escala/models.py:52](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:52).
- `EscalaColaborador.colaborador`, `tipo_escala`, `data_inicio`, `data_fim`, `ativa`: [escala/models.py:872](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:872). `CelulaDia.colaborador`, `data` e `origem`: [escala/models.py:1681](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/models.py:1681).
- `PerguntaDisputa.celula`, `motivo`, `data_evento`, `colaborador`, `resposta_colab` e `respondida_em`: [chamados/models.py:2527](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2527), [chamados/models.py:2534](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2534), [chamados/models.py:2536](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2536), [chamados/models.py:2546](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2546), [chamados/models.py:2560](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2560).

## Funções reais e achado do código

- **Espelho:** a view chama `espelho_do_colab`; esta chama `autoridade_do_periodo`, que carrega `batidas_apuraveis` e, havendo escalas, chama `apurar_por_fatia`. [ponto/views.py:160](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:160), [ponto/services/espelho.py:1227](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1227), [ponto/services/espelho.py:1022](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1022), [ponto/services/espelho.py:1162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1162).
- **Gestão de HE:** a tela chama `he_pendente_lavrado.ler` e `gestao_he.enriquecer`; esta consulta `estado_por_dia`, que lê `DecisaoHE.objects.vigentes()`. Isso responde à pendência de HE e sua decisão, não é a função que decide a intrajornada. [ponto/views.py:932](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:932), [ponto/views.py:945](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:945), [ponto/services/gestao_he.py:63](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:63).
- **Motor:** não há uma única implementação de indenização para todos os regimes. `Motor12x36.calcular_periodo` escreve os minutos suprimidos; `MotorComercial.calcular_periodo` tem sua própria condição; `MotorTurnoPartido.calcular_mes` trata o intervalo entre períodos. O mínimo vem de `_intra_minima`, e o intervalo reconhecido vem do pareador. [ponto/motor_calculo_v2.py:2750](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2750), [ponto/motor_calculo_v2.py:3215](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:3215), [ponto/motor_calculo_v2.py:2964](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2964), [ponto/motor_calculo_v2.py:1805](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1805), [ponto/motor_calculo_v2.py:1892](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1892).
- **PDF:** `_coletar_dados_espelho` chama `apurar_por_fatia` para seu resultado e chama `espelho_do_colab` para as linhas diárias. [relatorios/pdf_espelho.py:417](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:417), [relatorios/pdf_espelho.py:596](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:596).

O motor declara que **não usa `TipoEscala.intervalo_indenizavel` para impedir a indenização**; portanto, `False` é um filtro do censo pedido, não uma condição aplicada pelo motor. [ponto/motor_calculo_v2.py:817](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:817)

O anexo informa **152 dias** no recorte os152, dos quais **151 com a flag `False` e um com `True`**. Ele seleciona número ímpar de batidas, exige `DiaPago` e `pago_do_dia`, e exige pago maior que o realizado do pareador em mais de um minuto. Assim, ficaram fora todos os dias pares — inclusive eventuais dias de duas ou quatro batidas — e também os ímpares que não satisfazem as demais condições. A quantidade e os minutos deixados de fora não aparecem no dado fornecido. [não verificado: anexo os152.r e os152_os24.py fornecidos no pedido, sem arquivo e linhas acessíveis]

Para **col868 em 14/11, pergunta 39550**, e **col934 em 15/11, pergunta 39556**, “duas batidas e 60 minutos” é a afirmação do aval; não há no recorte apresentado resultado que a confirme. [não verificado]

## Sonda para o envelope na sombra

A sonda mede o **motor recalculado em leitura**, para todos os colaboradores encontrados, usando a janela da competência de cada empresa. Mostra também a rubrica `DiaPago` gravada nos dois casos, pois ela pode representar outro momento de cálculo. Agrupa as batidas apuráveis pelo `dia_das_batidas`, a mesma autoridade usada pelo espelho, e informa também as classes 0 e 1 para não escondê-las. [ponto/turnos.py:2868](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:2868), [ponto/turnos.py:2898](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:2898), [ponto/services/dia_pago.py:109](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:109).

Execute o bloco como entrada do `manage.py tenant_command shell --schema=juliani`. Ele exige PostgreSQL para estabelecer a transação `READ ONLY`; se isso falhar, não prossegue com a leitura. Os tracebacks mostram quadros e tipo da exceção, sem mensagem de exceção ou conteúdo de variáveis, para reduzir a exposição de dados pessoais. [não verificado: execução no envelope]

```python
import sys
import traceback
import logging
from collections import Counter, defaultdict

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from chamados.models import PerguntaDisputa
from escala.alimentacao import escalas_do_periodo, vinculo_do_dia
from ponto.janelas import janela_fechamento
from ponto.models import DiaPago
from ponto.services.dia_pago import _dia_de
from ponto.services.espelho import autoridade_do_periodo, espelho_do_colab
from ponto.services import gestao_he, he_pendente_lavrado
from ponto.turnos import dia_das_batidas
from relatorios.pdf_espelho import _coletar_dados_espelho

logging.disable(logging.CRITICAL)

def erro(item):
    exc = sys.exc_info()[1]
    print("TRACEBACK", item)
    traceback.print_tb(exc.__traceback__, file=sys.stdout)
    print("TIPO_ERRO", type(exc).__name__)

def faixa(n):
    if n >= 4:
        return "4+"
    return str(n)

def linha_motor(col, ini, fim):
    aut = autoridade_do_periodo(col, ini, fim)
    mapa_batida = dia_das_batidas(col, ini, fim)
    contagem = Counter(
        mapa_batida[b.pk]
        for b in aut.batidas
        if b.pk in mapa_batida and ini <= mapa_batida[b.pk] <= fim
    )
    minutos = defaultdict(float)
    resultado = aut.resultado
    mapa_periodo = getattr(resultado, "dia_da_jornada", None)
    for periodo in list(resultado.periodos) + list(resultado.periodos_ft):
        dia = _dia_de(periodo, mapa_periodo)
        if dia is not None and ini <= dia <= fim:
            minutos[dia] += float(
                periodo.minutos_intrajornada_indenizada or 0
            )
    return contagem, minutos

try:
    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )

        agora = timezone.now()
        totais = defaultdict(lambda: [0, 0.0])
        sem_escala = [0, 0.0]
        erros = 0

        # Censo: todos os colaboradores cadastrados; a janela é da empresa.
        try:
            ids = list(
                Colaborador.objects.order_by("pk")
                .values_list("pk", flat=True)
            )
            for pk in ids:
                try:
                    # Savepoint permite continuar após erro de uma consulta.
                    with transaction.atomic():
                        col = Colaborador.objects.select_related(
                            "empresa"
                        ).get(pk=pk)
                        ini, fim = janela_fechamento(11, 2026, col.empresa)
                        contagem, minutos = linha_motor(col, ini, fim)
                        escalas = escalas_do_periodo(pk, ini, fim)

                        for dia, valor in minutos.items():
                            if valor <= 0:
                                continue
                            vinculo = vinculo_do_dia(pk, dia, escalas)
                            te = vinculo.tipo_escala if vinculo else None
                            if te is None:
                                sem_escala[0] += 1
                                sem_escala[1] += valor
                                continue
                            if te.intervalo_indenizavel is False:
                                grupo = faixa(contagem.get(dia, 0))
                                totais[grupo][0] += 1
                                totais[grupo][1] += valor
                except Exception:
                    erros += 1
                    print("ERRO_COL col%d" % pk)
                    erro("CENSO_COL")

            print("CENSO_COLABORADORES", len(ids))
            for grupo in ("0", "1", "2", "3", "4+"):
                dias, mins = totais[grupo]
                print(
                    "MOTOR_FALSE batidas=%s dias=%d minutos=%.2f"
                    % (grupo, dias, mins)
                )
            print(
                "SEM_ESCALA dias=%d minutos=%.2f"
                % (sem_escala[0], sem_escala[1])
            )
            print("ERROS_COLABORADORES", erros)
            if erros:
                print("RESULTADO_CENSO_INCOMPLETO")
        except Exception:
            erro("CENSO")
        finally:
            print("FIM_SONDA_CENSO")

        # Conferência dos dois dias e da identidade das perguntas.
        for pk, dia_iso, pergunta_pk in (
            (868, "2026-11-14", 39550),
            (934, "2026-11-15", 39556),
        ):
            item = "col%d" % pk
            try:
                with transaction.atomic():
                    from datetime import date

                    dia = date.fromisoformat(dia_iso)
                    col = Colaborador.objects.select_related(
                        "empresa"
                    ).get(pk=pk)
                    ini, fim = janela_fechamento(11, 2026, col.empresa)
                    contagem, minutos = linha_motor(col, ini, fim)
                    escalas = escalas_do_periodo(pk, ini, fim)
                    vinculo = vinculo_do_dia(pk, dia, escalas)
                    te = vinculo.tipo_escala if vinculo else None

                    print(
                        "CASO %s dia=%s batidas_apuraveis=%d "
                        "motor_minutos=%.2f intervalo_indenizavel=%s"
                        % (
                            item, dia_iso, contagem.get(dia, 0),
                            minutos.get(dia, 0.0),
                            te.intervalo_indenizavel if te else "SEM_ESCALA",
                        )
                    )

                    linhas = DiaPago.objects.filter(
                        colaborador=col, mes=11, ano=2026,
                        versao="motor", tipo=DiaPago.TIPO_DIA, data=dia,
                    ).values_list("horas_intra_indenizada", flat=True)
                    for horas in linhas:
                        print(
                            "DIA_PAGO %s dia=%s minutos=%.2f"
                            % (item, dia_iso, float(horas or 0) * 60)
                        )

                    p = PerguntaDisputa.objects.filter(
                        pk=pergunta_pk
                    ).values(
                        "colaborador_id", "motivo",
                        "data_evento", "celula__data"
                    ).first()
                    if p is None:
                        print("PERGUNTA %s AUSENTE" % item)
                    else:
                        print(
                            "PERGUNTA %s mesma_pessoa=%s motivo=%s "
                            "data_evento=%s data_celula=%s"
                            % (
                                item, p["colaborador_id"] == pk,
                                p["motivo"], p["data_evento"],
                                p["celula__data"],
                            )
                        )

                    # Leitores reais: apenas características não pessoais.
                    esp = espelho_do_colab(
                        col, fim, ini, fim, ano=2026, agora=agora
                    )
                    ed = next(
                        (d for d in esp["dias"] if d["data"] == dia), None
                    )
                    print(
                        "ESPELHO %s dia_presente=%s batidas=%s"
                        % (
                            item, ed is not None,
                            len(ed["batidas"]) if ed else "AUSENTE",
                        )
                    )

                    pdf = _coletar_dados_espelho(
                        col, ini, fim, agora=agora
                    )
                    pd = next(
                        (d for d in pdf["dias"] if d["data"] == dia), None
                    )
                    print(
                        "PDF %s dia_presente=%s batidas=%s"
                        % (
                            item, pd is not None,
                            len(pd["batidas"]) if pd else "AUSENTE",
                        )
                    )

                    retrato, calculado_em = he_pendente_lavrado.ler(
                        col.empresa, 11, 2026
                    )
                    vis = gestao_he.enriquecer(
                        retrato,
                        limite=int(
                            getattr(
                                col.empresa, "limite_decisao_he_min", 0
                            ) or 0
                        ),
                    )
                    presente = any(
                        c["colab"] == pk for c in vis["colabs"]
                    )
                    print(
                        "GESTAO_HE %s retrato_existe=%s "
                        "calculado_em=%s colab_presente=%s"
                        % (
                            item, retrato is not None,
                            calculado_em, presente,
                        )
                    )
            except Exception:
                erro(item)
            finally:
                print("FIM_SONDA_%s" % item.upper())

        # A transação é somente leitura; não há chamada de lavratura.
except Exception:
    erro("ENVELOPE")
finally:
    print("FIM_SONDA_ENVELOPE")
```

A comparação numérica com os **151** exige o resultado do censo e, para uma interseção exata, as linhas completas D2 ou a reprodução do filtro os152 no mesmo instante. A diferença entre os totais, por si só, não mede essa interseção: o os152 usa `DiaPago` gravado e a sonda acima usa o motor recalculado. [não verificado]

## 18/11 13:42 · sessao 05766d97b939 · LAUDO-[nome]-1811-1340 · leitura · origem=Code

Os dados raspados relatam que **col868, em 14/11, aparecia no espelho com 18 min antes da entrada e 3 min depois da saída, e não aparecia na Gestão de HE**. A causa, a hora do retrato, a presença do dia nele, a situação cadastral, os filtros efetivamente usados e o instante da mudança permanecem **[não verificado]**. O anexo é o dado fornecido na solicitação, sem arquivo:linha disponível **[não verificado]**.

A cópia disponível não tem metadados Git acessíveis no diretório examinado; portanto, não pude confirmar que corresponde ao hash informado **[não verificado]**.

**Campos relevantes antes da leitura**

- `MetricaSnapshot`: `chave`, `escopo`, `escopo_id`, `data_ini`, `data_ref`, `numerador`, `denominador`, `valor`, `detalhe` e `calculado_em`. O retrato é o JSON `detalhe`; a data e hora são `calculado_em`. [app/inteligencia/models.py:81](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/inteligencia/models.py:81)
- `Colaborador`: `empresa`, `posto`, `situacao`, `data_admissao` e `data_demissao`; o identificador interno é a chave primária. [app/colaboradores/models.py:283](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:283) [app/colaboradores/models.py:319](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:319)
- `Empresa`: `dia_inicio_competencia`, `janela_he_ativa`, `janela_he_piso_min`, `limite_decisao_he_min`, `janela_he_teto_min`, `janela_he_saida_ativa`, `janela_he_saida_teto_min` e `janela_he_desde`. [app/colaboradores/models.py:46](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:46) [app/colaboradores/models.py:58](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:58) [app/colaboradores/models.py:69](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:69) [app/colaboradores/models.py:82](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:82) [app/colaboradores/models.py:90](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:90) [app/colaboradores/models.py:101](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:101) [app/colaboradores/models.py:117](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/models.py:117)
- `DecisaoHE`: `colaborador`, `data`, `estado`, `minutos_na_decisao`, `motivo`, `decidida_por`, `origem`, `revogada_em`, `revogada_motivo` e `minutos_na_revogacao`. `vigentes()` exclui as revogadas. [app/ponto/models.py:1265](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:1265) [app/ponto/models.py:1300](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:1300) [app/ponto/models.py:1335](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:1335)
- `Batida`: `colaborador`, `timestamp`, `tipo`, `sequencia`, `is_intervalo`, `escala` e `origem`. O resultado do motor não é modelo: `PeriodoCalculo` contém `entrada`, `saida` e `minutos_fora_janela`. [app/ponto/models.py:22](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:22) [app/ponto/models.py:45](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:45) [app/ponto/motor_calculo_v2.py:208](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:208)

**Funções reais e efeito dos filtros**

- A tela do espelho chama `espelho_do_colab`; este chama `autoridade_do_periodo` e `montar_dias`. O montador chama `marcar_pontas_fora`, acrescenta pontas publicadas pelo motor e publica `he_fora_da_janela`. O texto “sem decisão” está fixo no template; ele não comprova ausência de decisão vigente. [app/ponto/views.py:156](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:156) [app/ponto/services/espelho.py:1225](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1225) [app/ponto/services/espelho.py:721](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:721) [app/templates/ponto/espelho.html:189](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/templates/ponto/espelho.html:189)
- A Gestão chama `he_pendente_lavrado.ler` e `gestao_he.enriquecer`; a decisão viva vem de `estado_por_dia` e `DecisaoHE.objects.vigentes()`. O PDF da Gestão chama os mesmos leitores. O cartão PDF chama `espelho_do_colab`. [app/ponto/views.py:932](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:932) [app/ponto/services/gestao_he.py:63](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:63) [app/ponto/views.py:804](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:804) [app/relatorios/pdf_espelho.py:596](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:596)
- O espelho obtém o resultado por `autoridade_do_periodo`, que usa `apurar_por_fatia` quando há escalas e, no outro ramo, `motor.calcular_mes`. Na implementação `Motor12x36.calcular_periodo`, a linha indicada soma os minutos retirados da entrada e da saída; não há evidência fornecida de que esse seja o ramo específico de col868. [app/ponto/services/espelho.py:1162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:1162) [app/ponto/motor_calculo_v2.py:2750](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2750) [app/ponto/motor_calculo_v2.py:2787](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2787) **[não verificado]**
- Na apuração do retrato, o código seleciona colaboradores com `situacao='ativo'` no momento da apuração e só inclui dias dentro da competência que tenham pontas publicadas pelo espelho. Na leitura da Gestão, o limite classifica a ponta por `acima_do_limite`; os filtros de estado e dono podem retirar dias e, se não sobrar nenhum, a linha. Busca, praça e posto também podem retirar a linha. Isso não identifica qual condição ocorreu no caso. [app/ponto/services/he_pendente_lavrado.py:89](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:89) [app/ponto/services/he_pendente_lavrado.py:114](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:114) [app/ponto/services/gestao_he.py:274](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:274) [app/ponto/services/gestao_he.py:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:317) **[não verificado]**

A sonda abaixo assume **2026** e interpreta `col868` como chave primária 868; essas correspondências precisam ser confirmadas no envelope **[não verificado]**. Ela imprime primeiro os nomes de todos os campos dos modelos diretamente consultados, usa as funções reais, abre uma transação PostgreSQL `READ ONLY` e força rollback. Os períodos do motor são impressos com suas datas de entrada e saída, sem atribuí-los por conta própria ao dia 14. O traceback preserva a pilha e a classe da exceção, omitindo a mensagem, que pode conter dado pessoal **[não verificado]**.

Execute o texto pelo envelope na sombra com `manage.py tenant_command shell --schema=juliani`. Não execute a geração do PDF: a view da Gestão chama o gerador auditável, enquanto a sonda chama os leitores que a precedem. [app/ponto/views.py:870](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:870)

```python
import datetime as dt
import sys
import traceback

ITEM = "A"
COL = 868
DIA = dt.date(2026, 11, 14)
ANO, MES = 2026, 11


def falha(etapa):
    print("col868 ERRO", etapa)
    traceback.print_tb(sys.exc_info()[2], file=sys.stdout)
    print("TIPO_EXCECAO", sys.exc_info()[0].__name__,
          "(mensagem omitida para proteção de dados)")


def passo(nome, funcao):
    try:
        # Um savepoint permite continuar após erro de consulta.
        with transaction.atomic():
            return funcao()
    except Exception:
        falha(nome)
        return None


try:
    from django.db import connection, transaction
    from django.utils import timezone
    from colaboradores.models import Colaborador, Empresa
    from inteligencia.models import MetricaSnapshot
    from ponto.models import Batida, DecisaoHE
    from ponto.janelas import janela_fechamento
    from ponto.services import gestao_he as G
    from ponto.services import he_pendente_lavrado as L
    from ponto.services.espelho import espelho_do_colab

    # Metadados locais dos modelos, antes de qualquer consulta ao banco.
    for modelo in (Colaborador, Empresa, MetricaSnapshot, Batida, DecisaoHE):
        print("col868 [nome]", modelo._meta.label,
              [campo.name for campo in modelo._meta.fields])

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION READ ONLY")

        def cadastro():
            c = Colaborador.objects.select_related("empresa").get(pk=COL)
            e = c.empresa
            print(
                "col868 CADASTRO",
                "situacao", c.situacao,
                "admissao", c.data_admissao,
                "demissao", c.data_demissao,
                "empresa_corresponde_jsp",
                "jsp" in (e.razao_social or "").lower()
                or "jsp" in (e.nome_fantasia or "").lower(),
                "corte", e.dia_inicio_competencia,
                "janela_ativa", e.janela_he_ativa,
                "janela_desde", e.janela_he_desde,
                "piso", e.janela_he_piso_min,
                "saida_ativa", e.janela_he_saida_ativa,
                "teto_entrada", e.janela_he_teto_min,
                "teto_saida", e.janela_he_saida_teto_min,
                "limite_decisao", e.limite_decisao_he_min,
            )
            return c

        c = passo("cadastro", cadastro)

        if c is not None:
            e = c.empresa
            janela = passo(
                "janela",
                lambda: janela_fechamento(MES, ANO, e),
            )
            if janela is not None:
                ini, fim = janela
                print("col868 JANELA", ini, fim,
                      "contem_dia", ini <= DIA <= fim)

                def retrato():
                    foto, calculado_em = L.ler(e, MES, ANO)
                    print("col868 RETRATO_EXISTE", foto is not None)
                    print("col868 RETRATO_CALCULADO_EM", calculado_em)
                    if foto is None:
                        return None
                    print("col868 RETRATO_JANELA",
                          foto.get("data_ini"), foto.get("data_ref"),
                          "erros_apuracao", foto.get("erros"))
                    linhas = [
                        x for x in (foto.get("colabs") or [])
                        if x.get("colab") == COL
                    ]
                    print("col868 RETRATO_COLAB_EXISTE", bool(linhas))
                    for linha in linhas:
                        dias = [
                            d for d in (linha.get("dias") or [])
                            if d.get("data") == str(DIA)
                        ]
                        print("col868 RETRATO_DIA_EXISTE", bool(dias))
                        for d in dias:
                            pontas = [
                                {k: p.get(k) for k in
                                 ("hora", "marco", "minutos", "sentido")}
                                for p in (d.get("pontas") or [])
                            ]
                            print("col868 RETRATO_DIA",
                                  "data", d.get("data"),
                                  "minutos", d.get("minutos"),
                                  "estado_na_foto", d.get("estado"),
                                  "pontas", pontas)
                    return foto

                foto = passo("retrato", retrato)

                def decisoes():
                    todos = list(
                        DecisaoHE.objects.filter(
                            colaborador_id=COL, data=DIA
                        ).values(
                            "estado", "origem", "minutos_na_decisao",
                            "revogada_em"
                        )
                    )
                    vivos = G.estado_por_dia([COL], DIA, DIA)
                    print("col868 DECISOES", todos)
                    print("col868 ESTADO_GESTAO",
                          vivos.get((COL, str(DIA)), "sem_decisao"))

                passo("decisoes", decisoes)

                def gestao():
                    vis = G.enriquecer(
                        foto,
                        limite=int(e.limite_decisao_he_min or 0),
                    )
                    linhas = [
                        x for x in vis["colabs"]
                        if x.get("colab") == COL
                    ]
                    print("col868 GESTAO_SEM_FILTROS_COLAB", bool(linhas))
                    for linha in linhas:
                        dias = [
                            d for d in (linha.get("dias") or [])
                            if d.get("data") == str(DIA)
                        ]
                        print("col868 GESTAO_SEM_FILTROS_DIA", bool(dias))
                        for d in dias:
                            print("col868 GESTAO_DIA",
                                  "minutos", d.get("minutos"),
                                  "estado", d.get("estado"),
                                  "acima_limite", d.get("acima"))

                    # Os valores efetivamente usados na tela observada não
                    # foram fornecidos. Estes dois recortes medem os filtros
                    # reais de dono sobre a foto atual.
                    for dono in ("admin", "sistema"):
                        filtrado = G.enriquecer(
                            foto,
                            limite=int(e.limite_decisao_he_min or 0),
                            dono=dono,
                        )
                        achou = any(
                            x.get("colab") == COL
                            and any(d.get("data") == str(DIA)
                                    for d in (x.get("dias") or []))
                            for x in filtrado["colabs"]
                        )
                        print("col868 GESTAO_FILTRO_DONO", dono, achou)

                passo("gestao", gestao)

                def espelho_e_motor():
                    agora = timezone.now()
                    hoje = timezone.localtime(agora).date()
                    esp = espelho_do_colab(
                        c, hoje, ini, fim,
                        ano=ANO, modo_gestor=True, agora=agora,
                    )
                    dias = [
                        d for d in (esp.get("dias") or [])
                        if d.get("data") == DIA
                    ]
                    print("col868 ESPELHO_DIA_EXISTE", bool(dias))
                    for d in dias:
                        pontas = [
                            {k: p.get(k) for k in
                             ("hora", "marco", "minutos", "sentido")}
                            for p in (d.get("he_fora_da_janela") or [])
                        ]
                        print("col868 ESPELHO_PONTAS", pontas)
                        print("col868 ESPELHO_TOTAL_REAL",
                              L.minutos_fora_do_dia(d))

                    resultado = esp["resultado"]
                    for grupo in ("periodos", "periodos_ft"):
                        for p in (getattr(resultado, grupo, None) or []):
                            entrada = getattr(p, "entrada", None)
                            saida = getattr(p, "saida", None)
                            if entrada is None:
                                continue
                            data_entrada = timezone.localtime(entrada).date()
                            if abs((data_entrada - DIA).days) <= 1:
                                print(
                                    "col868 MOTOR_PERIODO",
                                    "grupo", grupo,
                                    "entrada", entrada,
                                    "saida", saida,
                                    "minutos_fora_janela",
                                    getattr(p, "minutos_fora_janela", None),
                                )

                passo("espelho_e_motor", espelho_e_motor)

        # Também assegura rollback ao sair do bloco, inclusive se algum
        # leitor chamado vier a tentar mudar dados.
        transaction.set_rollback(True)

except Exception:
    falha("envelope")
finally:
    print("FIM_SONDA_" + ITEM)
```

A sonda mede o retrato atual e os valores atuais; **ela não estabelece, por si só, quando ocorreu a mudança nem quais filtros estavam na tela observada**. Sem esses instantes e filtros, não é possível atribuir um único dono causal ao caso **[não verificado]**.

## 18/11 13:41 · sessao ef03d825469f · LAUDO-[nome]-1811-1340 · leitura · origem=Code

**Resultado do item 1(b):** o material fornecido relata que col882 viu **10 min** no espelho e ciência na Gestão, mas não traz a saída das funções nem a linha de `DecisaoHE`. Portanto, os valores atuais das duas leituras, o estado, a origem e a vigência da decisão estão **[não verificado]**. Também não é possível confirmar pelo material que os “9,6 min” do RELATO se referem ao mesmo cálculo e às mesmas entradas do aviso de 10 min. **[não verificado]**

**Campos antes da leitura**

Os campos abaixo são os diretamente relevantes ao recorte e à sonda; as funções reais consultam também outros modelos e campos. A lista não pretende ser um inventário de todas as consultas internas. [app/ponto/services/espelho.py:971; app/relatorios/pdf_espelho.py:187]

- `Colaborador`: `empresa`, `posto`, `situacao`, além do identificador primário usado pela sonda; o modelo também contém `nome` e `cpf`, que não devem ser impressos. [app/colaboradores/models.py:319]
- `Empresa`: `janela_he_ativa`, `janela_he_piso_min`, `janela_he_teto_min`, `janela_he_saida_ativa`, `janela_he_saida_teto_min`, `janela_he_desde` e `limite_decisao_he_min`. [app/colaboradores/models.py:58; app/colaboradores/models.py:69; app/colaboradores/models.py:82; app/colaboradores/models.py:90; app/colaboradores/models.py:101; app/colaboradores/models.py:105; app/colaboradores/models.py:117]
- `Batida`: `colaborador`, `timestamp`, `tipo`, `is_intervalo`, `escala` e `origem`. [app/ponto/models.py:22; app/ponto/models.py:45]
- `EscalaColaborador`: `colaborador`, `posto`, `tipo_escala`, `data_inicio`, `data_fim`, `ativa`; `TipoEscala` inclui `intervalo_indenizavel`. A `CelulaDia` inclui `colaborador`, `data`, `trabalha`, `origem` e `escala_geradora`. [app/escala/models.py:871; app/escala/models.py:108; app/escala/models.py:1681]
- `MetricaSnapshot`: `chave`, `escopo`, `escopo_id`, `data_ini`, `data_ref`, `numerador`, `denominador`, `valor`, `detalhe`, `calculado_em`. A Gestão lê `detalhe` e `calculado_em` pela função `ler`. [app/inteligencia/models.py:81; app/ponto/services/he_pendente_lavrado.py:193]
- `DecisaoHE`: `colaborador`, `data`, `estado`, `minutos_na_decisao`, `motivo`, `decidida_por`, `origem`, `decidida_em`, `atualizada_em`, `revogada_em`, `revogada_motivo`, `minutos_na_revogacao`. `objects.vigentes()` filtra `revogada_em__isnull=True`. [app/ponto/models.py:1265; app/ponto/models.py:1300; app/ponto/models.py:1335]
- Saída do motor, em memória: `ResultadoMes.periodos` e `periodos_ft`; em cada `PeriodoCalculo`, `entrada`, `saida` e `minutos_fora_janela`. O motor pode publicar o mapa `dia_da_jornada` para relacionar período e dia. [app/ponto/motor_calculo_v2.py:207; app/ponto/motor_calculo_v2.py:234; app/ponto/motor_calculo_v2.py:268; app/ponto/motor_calculo_v2.py:1073]

**Funções reais e o que cada uma entrega**

- **Espelho:** a view chama `espelho_do_colab`, que obtém a autoridade do período e chama `montar_dias`. Este chama `marcar_pontas_fora`, acrescenta pontas publicadas pelo motor quando aplicável e entrega `he_fora_da_janela` ao template. A função marcadora testa a ponta contra o piso e arredonda os minutos para inteiro. O template imprime o valor e a expressão fixa “sem decisão”. [app/ponto/views.py:156; app/ponto/services/espelho.py:1225; app/ponto/services/espelho.py:1262; app/ponto/services/espelho.py:721; app/ponto/janela_he.py:215; app/templates/ponto/espelho.html:189]
- **Gestão:** chama `he_pendente_lavrado.ler` para obter a foto e `gestao_he.enriquecer`. A quantidade e os dias vêm da foto; `enriquecer` chama `estado_por_dia`, que consulta `DecisaoHE.objects.vigentes()`, e sobrepõe o estado vivo. `estado='nao'` é a decisão que representa ciência registrada. O valor da foto e o estado desta pessoa permanecem **[não verificado]**. [app/ponto/views.py:932; app/ponto/services/gestao_he.py:245; app/ponto/services/gestao_he.py:278; app/ponto/services/gestao_he.py:63; app/ponto/models.py:1283]
- **Motor:** a autoridade da tela chama `apurar_por_fatia` quando há escalas; no ramo sem escalas chama `motor.calcular_mes`. O cálculo de período passa por `_janela_do_par`, que chama `_entrada_efetiva` e `_saida_efetiva`; no ramo `Motor12x36`, as duas parcelas alimentam `minutos_fora_janela`. O valor do período de col882 é **[não verificado]**. [app/ponto/services/espelho.py:1162; app/ponto/motor_calculo_v2.py:1752; app/ponto/motor_calculo_v2.py:2750; app/ponto/motor_calculo_v2.py:2787]
- **PDF do espelho:** `_coletar_dados_espelho` chama `espelho_do_colab` para montar os dias; o desenho lê `dia['he_fora_da_janela']`. O coletor também apura o motor para seu próprio resultado. O valor do PDF para o caso é **[não verificado]**. [app/relatorios/pdf_espelho.py:187; app/relatorios/pdf_espelho.py:417; app/relatorios/pdf_espelho.py:596; app/relatorios/pdf_espelho.py:932]

A sonda abaixo assume **2026** e que `col882` representa a **PK 882**; a correspondência da etiqueta raspada com essa PK é **[não verificado]**. Ela usa a competência que `janela_atual` atribuir a 21/10 e informa seus limites. O espelho usa a data atual, como a view; isso não reconstrói necessariamente o que foi visto no momento do relato. [app/ponto/views.py:156; app/ponto/views.py:161; app/ponto/views.py:928]

Execute o bloco pelo envelope, no shell `manage.py tenant_command shell --schema=juliani`. Cada passo abre uma transação PostgreSQL somente de leitura; o traceback conserva arquivos, linhas e tipo da exceção, omitindo mensagem e texto-fonte para evitar dados pessoais. A sonda não foi executada aqui. **[não verificado]**

```python
import datetime as dt
import io
import traceback
from contextlib import redirect_stdout, redirect_stderr

from django.db import connection, transaction
from django.utils import timezone

DIA = dt.date(2026, 10, 21)
COLAB_PK = 882


def executar(item, funcao):
    try:
        # Cada passo tem sua própria transação; falha em um não impede os demais.
        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION READ ONLY")
            # As funções podem produzir mensagens não controladas: não publicá-las.
            with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
                resultado = funcao()
        print("col882", item, resultado)
    except Exception as exc:
        print("col882", item, "Traceback (most recent call last):")
        for frame in traceback.extract_tb(exc.__traceback__):
            print('  File "%s", line %s, in %s' %
                  (frame.filename, frame.lineno, frame.name))
        print("col882", item, "EXCECAO", type(exc).__name__,
              "(mensagem omitida)")
    finally:
        print("FIM_SONDA_" + item)


def contexto():
    from colaboradores.models import Colaborador
    from ponto.janelas import janela_atual

    colab = Colaborador.objects.select_related("empresa").get(pk=COLAB_PK)
    ini, fim = janela_atual(DIA, colab.empresa_id)
    return colab, ini, fim


def identificar():
    colab, ini, fim = contexto()
    return {
        "pk_encontrada": "col882",
        "dia": DIA.isoformat(),
        "competencia_inicio": ini.isoformat(),
        "competencia_fim": fim.isoformat(),
        "rotulo_da_competencia": "%02d/%04d" % (fim.month, fim.year),
    }


def espelho_e_motor():
    from ponto.services.espelho import espelho_do_colab

    colab, ini, fim = contexto()
    agora = timezone.now()
    esp = espelho_do_colab(
        colab, timezone.localdate(), ini, fim,
        ano=fim.year, agora=agora,
    )
    dias = [d for d in esp["dias"] if d.get("data") == DIA]
    resultado = esp["resultado"]
    mapa = getattr(resultado, "dia_da_jornada", None)
    periodos = list(resultado.periodos) + list(resultado.periodos_ft)

    # O mapa é a atribuição publicada pelo motor. Sem ele, não se infere [nao verificado]
    # o dia do turno a partir da data civil da entrada.
    if mapa is None:
        motor = "dia_da_jornada nao publicado; valor do dia nao verificado" [nao verificado]
    else:
        sem_atribuicao = sum(1 for p in periodos if id(p) not in mapa)
        motor = {
            "minutos_fora_por_periodo_atribuido_ao_dia": [
                p.minutos_fora_janela
                for p in periodos if mapa.get(id(p)) == DIA
            ],
            "periodos_sem_atribuicao": sem_atribuicao,
        }

    return {
        "linhas_do_dia_no_espelho": len(dias),
        "avisos_minutos_e_sentido": [
            {
                "minutos": ponta.get("minutos"),
                "sentido": ponta.get("sentido"),
            }
            for dia in dias
            for ponta in (dia.get("he_fora_da_janela") or [])
        ],
        "motor": motor,
    }


def gestao():
    from ponto.services.he_pendente_lavrado import ler
    from ponto.services.gestao_he import enriquecer, estado_por_dia

    colab, ini, fim = contexto()
    retrato, calculado_em = ler(colab.empresa, fim.month, fim.year)
    estado = estado_por_dia([COLAB_PK], DIA, DIA)

    if retrato is None:
        return {
            "retrato": "ausente",
            "estado_por_dia": estado.get((COLAB_PK, DIA.isoformat())),
        }

    vis = enriquecer(
        retrato,
        limite=int(colab.empresa.limite_decisao_he_min or 0),
    )
    linhas = [c for c in vis.get("colabs", []) if c.get("colab") == COLAB_PK]
    dias = [
        d for c in linhas for d in (c.get("dias") or [])
        if str(d.get("data")) == DIA.isoformat()
    ]
    return {
        "retrato_calculado_em": calculado_em.isoformat() if calculado_em else None,
        "colab_no_resultado": bool(linhas),
        "dia_na_foto_enriquecida": bool(dias),
        "dias": [
            {"minutos": d.get("minutos"), "estado": d.get("estado")}
            for d in dias
        ],
        "estado_por_dia": estado.get((COLAB_PK, DIA.isoformat())),
    }


def decisao_vigente():
    from ponto.models import DecisaoHE

    linhas = list(
        DecisaoHE.objects.vigentes()
        .filter(colaborador_id=COLAB_PK, data=DIA)
        .values("estado", "origem", "minutos_na_decisao", "revogada_em")
    )
    return {
        "quantidade_vigente": len(linhas),
        "decisoes": [
            {
                "estado": linha["estado"],
                "origem": linha["origem"],
                "minutos_na_decisao": linha["minutos_na_decisao"],
                "vigente_pela_porta": True,
            }
            for linha in linhas
        ],
    }


def pdf():
    from relatorios.pdf_espelho import _coletar_dados_espelho_mes

    colab, ini, fim = contexto()
    dados = _coletar_dados_espelho_mes(
        colab, fim.month, fim.year, agora=timezone.now()
    )
    dias = [d for d in dados["dias"] if d.get("data") == DIA]
    return {
        "linhas_do_dia": len(dias),
        "avisos_minutos_e_sentido": [
            {
                "minutos": ponta.get("minutos"),
                "sentido": ponta.get("sentido"),
            }
            for dia in dias
            for ponta in (dia.get("he_fora_da_janela") or [])
        ],
    }


executar("CONTEXTO", identificar)
executar("ESPELHO_MOTOR", espelho_e_motor)
executar("GESTAO", gestao)
executar("DECISAO_VIGENTE", decisao_vigente)
executar("PDF", pdf)
```

Se o mapa de atribuição do motor não estiver publicado ou deixar períodos sem atribuição, a sonda declara essa limitação: uma lista vazia, nesse caso, não prova zero para o dia. A consulta de decisão vigente também não identifica eventual linha revogada; uma lista vazia significa apenas que a porta `vigentes()` não retornou decisão para o dia. [app/ponto/motor_calculo_v2.py:1073; app/ponto/models.py:1265]

## 18/11 13:32 · sessao 269d7cb7f1fb · POUSOS-COMBOIO-1811-1332 · verificador · origem=Code

**Item 1 — PROCEDE: AVAL-DEVOLVIDO para a condição literal, enquanto ela não for comprovada no resultado.** Os scripts refeitos melhoram o recorte, mas o dado não permite concluir que o texto registrado para **cada** corte será o literal exigido. Os demais pontos abaixo não demonstram, por si, colisão com o complemento. [anexos `doc_comum.py`, `aplica_doc_p2.py`, `aplica_doc_f4.py` e aval principal: não verificado em arquivo]

1. **Premissas — parcialmente conferem; resultado ainda não verificado.** `RESPOSTAS-DEVOLVIDOS-1711` já consta no JSON da cópia em [CORTES.json:1910](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/CORTES.json:1910). O gerador lê o JSON e aponta `CORTES.md` como destino, em [gerar_cortes.py:22](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/gerar_cortes.py:22). Isso não comprova que a geração será executada no pouso nem que o Markdown resultante estará atualizado. [não verificado]

   O censo dos diffs, a identidade com `81f678c7e`, a aplicação dos scripts nos commits, os selos e os fatos de produção não são conferíveis pelos dados disponíveis. O complemento os coloca como condições ou premissas, mas sua satisfação permanece [não verificado].

2. **Colisão — não demonstrada.** Incluir os três cortes adicionais **não excede, por si só, o escopo**: a alínea **(b) do item 1** exige que cada corte citado nos `.py` e testes esteja no JSON, independentemente de ter sido nomeado no aval principal. A completude do censo e a fidelidade dos três registros às fontes permanecem [não verificado]. [nao verificado]

   A leitura de que a conferência pela sessão principal substitui o opus curto **procede no ponto de divergência**: o complemento atribui a conferência à sessão principal, sem Agent, e determina sua prevalência onde divergir do principal. Outras exigências vigentes de revisão não fornecidas são [não verificado].

   Quanto ao item 3 de `LIBERA-B3-F2-P2`, a omissão no complemento não demonstra revogação. A decisão informada de cumpri-lo é compatível com essa leitura; o texto integral e a vigência do aval anterior, bem como o cumprimento efetivo de drenagem, medições e parada, são [não verificado].

3. **Literal — condição ainda não fecha comprovadamente.** Recortar linhas pode preservar **literalmente os trechos escolhidos** sem reproduzir o arquivo inteiro. Isso não prova que a seleção seja suficiente para a exigência de “o texto de cada corte” nem que não omita contexto necessário; os avais de origem não foram fornecidos nesta volta para confronto. [não verificado]

   Há também uma limitação concreta no script fornecido: `literal()` usa `strip()` e une as linhas com ` | `, portanto o campo resultante não preserva exatamente a formatação nem a continuidade original. Além disso, `registra()` **não atualiza um ID já existente**: mesmo que leia o aval de origem, pode conservar no JSON uma frase anterior que não foi conferida como literal. Assim, a verificação de prefixo único não basta para garantir o conteúdo final de cada entrada. [anexo `doc_comum.py`: não verificado em arquivo]

   Para prefixo ausente ou repetido, o `assert` interrompe a avaliação antes de `registra()` receber a lista daquele script; isso sustenta a devolução **para essa falha específica**, mas não comprova atomicidade de toda a operação documental. [anexos `doc_comum.py`, `aplica_doc_p2.py` e `aplica_doc_f4.py`: não verificado em arquivo]

4. **Exportada, dinheiro ou `!` — toque fora do declarado não demonstrado.** O complemento proíbe tocar exportada; o deploy normal pode executar migrations antes do reload, conforme [deploy.sh:30](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:30) e [deploy.sh:316](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:316). Os diffs efetivos e efeitos das migrations e do pouso sobre exportada, dinheiro ou `!` são [não verificado].

## 18/11 13:31 · sessao 922407afe52f · E4C-F6F7 · construtor · origem=Code

As curas C1–C4 estão aplicadas. As verificações pontuais sem banco passaram; o fechamento dos 18 vermelhos da suíte permanece **[não verificado]**. A suíte não foi executada. [nao verificado]

- **C1:** escolhi o default `False` nas fixtures, igual ao campo real, mantendo as asserções existentes. Arquivos: [test_e3_c8b_sem_banco.py:68](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_c8b_sem_banco.py:68), [test_e3_pos_suite_sem_banco.py:27](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_pos_suite_sem_banco.py:27), [test_e3_portas_tarefa.py:725](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_portas_tarefa.py:725), [test_e3_r3_c7_sem_banco.py:247](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_r3_c7_sem_banco.py:247), [test_e3_tarefa_admin_sem_banco.py:123](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:123). A escolha acompanha o [default do modelo:4195](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4195); o mutante que retira o campo volta a provocar o erro no selo ([teste:74](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:74)). [nao verificado]

- **C2:** a fixture compila `_fatos_das_perguntas` no namespace da classe, usando o corpo do módulo ([test_e3_r3_c7_sem_banco.py:211](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e3_r3_c7_sem_banco.py:211)). As asserções de status e relações continuam no selo; retirar a função reproduz `NameError` ([mutante:82](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:82)).

- **C3:** os selos agora verificam por AST a chamada com `colab` como primeiro argumento e o uso de `pendentes`, aceitando argumentos nomeados ([test_pergunta_no_app.py:29](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:29), [test_contract_pergunta_viva.py:26](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:26)). Cada arquivo contém `test_a_costura_MORDE`, que troca a fonte ou remove a chamada e exige rejeição ([primeiro:189](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:189), [segundo:110](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:110)). A cadeia adicional até `q_vivas` e o juiz continua no segundo selo ([linha 94](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:94)).

- **C4:** o contador entrega o universo à autoridade e conta apenas as linhas de validação que ela devolve ([backfill_decisoes_pergunta.py:104](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:104), [autoridade:4238](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4238)). Escolhi essa opção porque a autoridade já aceita perguntas sem carimbo; não acrescentei exceção à allowlist. O teste sem banco exercita a autoridade real e distingue validação sem linha, validação correspondente e resposta ([teste:30](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:30)). A varredura agora percorre mais perguntas; o custo em produção é **[não verificado]**. [nao verificado]

Para G5, a seleção de células geradas futuras usa `data__gt=hoje` sem teto de competência ([cadastro_futuro.py:22](/home/ronald/wt-codex-e4c-f6f7-132608/app/ponto/services/cadastro_futuro.py:22)); o resultado dos dois testes de folga nesta árvore é **[não verificado]**. [nao verificado]

Os mutantes executados estão em [test_e4c_f6f7_c7_sem_banco.py](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:53). Cada comando abaixo passou, confirmando que o caso detecta o respectivo mutante: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.MutantesDasFixtures.test_sem_default_da_resposta_MORDE
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.MutantesDasFixtures.test_sem_funcao_no_namespace_MORDE
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.CosturasSemDjango.test_selos_e_mutantes_MORDEM
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.ContadorSemBanco.test_mutante_conta_tambem_respostas_MORDE
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.ContadorSemBanco.test_mutante_reintroduz_filtro_cru_MORDE
```

Também passaram 36 testes pontuais de C1/C2, os seis testes sem banco do arquivo novo e o selo C4 com `SimpleTestCase` substituído por `unittest.TestCase`. `py_compile` passou nos nove arquivos alterados, com a escrita de bytecode desativada. A matriz e os selos que importam Django não iniciaram no interpretador disponível porque `django.contrib` e `django.test` não estavam disponíveis; seu resultado permanece **[não verificado]**. [nao verificado]

LEI-AKITA: origem=[tarefa_do_dia.py:164](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/tarefa_do_dia.py:164) + [backfill_decisoes_pergunta.py:94](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:94), testemunha=[models.py:4195](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4195) + [models.py:4238](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4238) + [pergunta_no_app.py:68](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/pergunta_no_app.py:68), RED=18 vermelhos da suíte b66587c26 fornecidos no anexo **[não verificado]**, quem-mais-le=[aviso_pendencia.py:9](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/aviso_pendencia.py:9) + [cobranca.py:48](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/cobranca.py:48) + [tarefa_do_dia.py:436](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/services/tarefa_do_dia.py:436), juizes novos=0, padrao=Single Source of Truth / Aggregate Root ([GLOSSARIO.md:11](/home/ronald/wt-codex-e4c-f6f7-132608/app/docs/GLOSSARIO.md:11)).

## 18/11 13:30 · sessao 49422d7c0b03 · POUSOS-COMBOIO-1811-1332 · verificador · origem=Code

**Item 1 — PROCEDE: AVAL-DEVOLVIDO.** A condição de registrar o texto **literal** de cada corte não fecha com os scripts fornecidos. **Item 3 — inconclusivo:** o dado fornecido não permite comprovar uma colisão nem certificar todas as premissas necessárias para chamá-lo de CONFERE. [anexo `pousos_comboio_1811_1332.txt`, itens 1 e 3, dado raspado; arquivo:linha não conferível]

Os anexos abaixo estão no dado raspado, mas não foram encontrados como arquivos nesta cópia. Por isso, posso comparar seu conteúdo fornecido, sem atribuir-lhes números de linha. A cópia também não tem metadados Git acessíveis: os hashes, diffs e certificações informados são **não conferíveis** aqui. [não verificado]

**Item 1 — E4C-F2-P2 e F4**

1. **Premissas.** `LEIS.md:151` é de fato a L-130, e contém os dois trechos que o script P2 procura alterar. A L-106 exige docs no commit do marco. [app/docs/LEIS.md:151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:151), [app/docs/LEIS.md:132](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:132)

   Os cinco avais de origem constam do dado raspado, com texto identificável; sua existência nos caminhos `logs/AVAIS-*` não é conferível nesta cópia. O item 1 menciona, entre parênteses, RESPOSTAS-1711-1050 e LIBERA-B3-F2-P2 para a P2, enquanto o script insere CORTE-F2-RESSUSCITAR-GRAVA-ATO e RESPOSTAS-1711-1050: a correspondência pretendida precisa ser esclarecida. [anexos `pousos_comboio_1811_1332.txt`, `aplica_doc_p2.py` e cinco avais fornecidos; arquivo:linha não conferível]

2. **Colisão com aval vigente.** LIBERA-B3-F2-P2 item 3 exige drenar os crons antes do migrate e do reload; depois do reload, medir N1 e N2 com denominadores e a corrida de `respondida_em`; qualquer resultado maior que zero mantém os crons desligados e exige PAREI. O item 1 manda fazer deploy e smoke, mas não repete essas condições. A omissão, por si só, não as revoga; executar apenas a sequência resumida do item 1 sem cumpri-las colidiria com o aval anterior. [anexos `libera_b3_f2p2_1758.txt`, item 3, e `pousos_comboio_1811_1332.txt`, item 1; arquivo:linha não conferível] [nao verificado]

3. **Literal.** O aval exige texto literal e devolução se a origem não for achada no literal. As frases dos scripts P2 e F4 são resumos: por exemplo, a frase do COMPLEMENTO-1 reúne os três itens em uma redação condensada, em vez de reproduzir o texto fornecido; a frase RESPOSTAS-1711-1050 também seleciona e resume itens. Essa é uma incompatibilidade comprovável no próprio dado fornecido e sustenta a devolução. Os scripts fornecidos escrevem `CORTES.json` e, no caso P2, `LEIS.md`; não escrevem diretamente `CORTES.md`, embora o item o mencione. Outro mecanismo que atualize o `.md` é **não verificado**. [anexos `aplica_doc_p2.py`, `aplica_doc_f4.py`, `complemento1_corte_u163_no_k6_1611.txt` e `respostas_1711_1050.txt`; arquivo:linha não conferível]

   A instrução de que o opus confira apenas a entrada do doc está expressa no aval; uma obrigação vigente de releitura integral de código que a contradiga é **não verificada**. [anexo `pousos_comboio_1811_1332.txt`, item 1; arquivo:linha não conferível]

4. **Exportada, dinheiro ou `!`.** O deploy normal pode aplicar migrations pendentes antes do reload; portanto, o pouso não é uma operação exclusivamente documental. O conteúdo e efeito da migration 0056 da P2, ausente desta cópia, são **não conferíveis**. Não há dado suficiente para afirmar toque em exportada, dinheiro ou `!` fora do declarado. [bin/deploy.sh:30](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:30), [bin/deploy.sh:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/deploy.sh:317), [não verificado]

**Item 3 — Cirineu sem ESCALA**

1. **Premissas.** A certificação de `fa49abfb6`, a composição de `7bc85383d` e o diff entre esta e a suite A são **não conferíveis** sem os objetos Git ou logs. O aval fornecido reconhece que a árvore sem ESCALA não foi a certificada e exige suite inteira e opus aprovados antes do pouso. [anexo `pousos_comboio_1811_1332.txt`, item 3; arquivo:linha não conferível]

2. **Colisões.** O texto integral e a vigência de [nome]-12H e COMBOIO-DAS-SUITES, bem como a existência e o estado da suite em `4ec3e15d0`, são **não conferíveis**. A ordem de rodar outra suite em paralelo não demonstra, sozinha, que a suite em curso será interrompida ou que a Cirineu tomará a vez da espinha; essas colisões permanecem **não verificadas**. [anexo `pousos_comboio_1811_1332.txt`, item 3; arquivo:linha não conferível]

3. **Literal.** Commits adicionais **apenas de documentação** não contradizem, por si, a proibição de mudar `.py`. São duas verificações distintas: comparar os `.py` com a árvore certificada e demonstrar que a suite verde cobre a árvore efetivamente pousada segundo a regra aplicável. O conteúdo dos commits adicionais, a regra completa de certificação e o resultado dessas verificações são **não conferíveis**. O script de doc da Cirineu fornecido altera `CORTES.json`, sem escrever `.py`. [anexos `pousos_comboio_1811_1332.txt`, item 3, e `aplica_doc_cirineu.py`; arquivo:linha não conferível] [nao verificado]

4. **Exportada, dinheiro ou `!`.** O item proíbe tocar exportada e regerar célula antes da simulação. O efeito real do diff Cirineu e do pouso em exportada, dinheiro ou `!` é **não conferível** nesta cópia. [anexo `pousos_comboio_1811_1332.txt`, item 3; arquivo:linha não conferível] [nao verificado]

_6972 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
