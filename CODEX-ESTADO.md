# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 07:53** · publicado em **30/10/2026 07:58** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 07:53 · sessao e13c20d27801 · O261-V · sonda · origem=Code

**1. Batida → pergunta de origem → texto gravado.**

As linhas abaixo transcrevem a sonda fornecida, sem arquivo:linha disponível nem consulta independente a produção: **[não verificado]**. O código grava o vínculo em `pergunta_origem` em [materializacao.py:891](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:891).

| Batida | Timestamp mostrado | `pergunta_origem` | Campo `pergunta` e dia mostrado |
|---|---|---|---|
| B140648 | 20/10 07:00 | P46023 | Texto e dia da pergunta ausentes da sonda. **[não verificado]** |
| B140649 | 22/10 07:00 | P46704 | “Confirme o horário da saída no dia **21/10/2026**”. **[não verificado]** |
| B140650 | 29/10 11:33:16 | `None` | Origem `app`; não é `disputa_s84_retro`. **[não verificado]** |
| B140651 | 20/09 19:00 | P34113 | Texto e dia da pergunta ausentes da sonda. **[não verificado]** |
| B140652 | 24/10 07:00 | P49423 | “Confirme o horário da saída no dia **23/10/2026**”. **[não verificado]** |
| B140653 | 26/10 07:00 | P50180 | “Confirme o horário da saída no dia **25/10/2026**”. **[não verificado]** |
| B140654 | 28/10 07:00 | P51025 | “Confirme o horário da saída no dia **27/10/2026**”. **[não verificado]** |

Não é possível preencher os textos de P46023/P34113 pelo timestamp da batida ou pelo template: o valor efetivamente gravado nessas perguntas não foi fornecido. **[não verificado]**

**2. Escritor de `fantasma`, predicado e batidas que teriam casado.**

O escritor da cópia é `PerguntaDisputa.encerrar_fantasma(autor)`: atribui `via_resolucao=VIA_FANTASMA`, `materializada_em=now()` e `materializada_por=autor`; salva somente esses três campos, sem exigir nem escrever resposta. [models.py:2162](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2162), [models.py:2170](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2170).

O chamador atual é `encerrar_orfas_pela_lampada`, que obtém o usuário de sistema e chama essa porta. O helper busca `username='sistema'`; sua correspondência com o ID 666 é **[não verificado]**. [reconciliador.py:1086](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1086), [core/utils.py:18](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/core/utils.py:18).

O predicado atual funciona assim:

- `saida_sem_entrada` cobra `('E', 'hi')`; `orfao_14h` cobra `('S', 'hf')`. [juizes.py:823](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:823).
- Lê a hora em `p.celula.dna['marcos']` e procura a lâmpada da ATA pela chave **`(hora, tipo)`**, verificando `acesa`. Esse trecho não consulta uma `Batida`, não retorna seu ID e não lê `dna['lampadas'][].instante`. [juizes.py:845](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:845), [juizes.py:863](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:863).
- Exige resposta vazia, marco comprovadamente aceso e pergunta viva. Depois exige dia anterior a hoje e encerramento desse dia. **A linha que escolhe o dia para consultar o encerramento é `dia_encerrado_motivo(..., p.data_evento, ...)`.** [reconciliador.py:1030](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1030), [reconciliador.py:1038](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1038).

Portanto, **a associação local usa hora+tipo dentro da célula vinculada**; o dia participa pela escolha dessa célula e pelo encerramento em `p.data_evento`. Não existe nesse predicado uma comparação direta de timestamp de batida por “dia+hora” ou “só hora”. A porta de abertura escolhe a célula por `data=data_evento`. [models.py:2060](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2060), [juizes.py:866](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:866).

Há uma limitação temporal: o comando atual `reconciliar_perguntas_orfas` é vigia. Seu comentário registra a remoção da regra antiga `_eh_fantasma`/`_bem_formado`/`_turnos_na_janela`, que julgava turno completo e última saída até 180 minutos do fim previsto. Esse comentário não fornece o corpo antigo nem prova a execução que produziu cada carimbo de 22–28/10. **Predicado histórico exato desses carimbos: [não verificado].** [reconciliar_perguntas_orfas.py:1](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/reconciliar_perguntas_orfas.py:1), [reconciliar_perguntas_orfas.py:10](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/reconciliar_perguntas_orfas.py:10).

Aplicação ao material fornecido:

| Pergunta | Marco indicado no pedido | Batida que casou |
|---|---|---|
| P46703 | E 21/10 19:00 | ID não demonstrado. **[não verificado]** |
| P49422 | E 23/10 19:00 | ID não demonstrado. **[não verificado]** |
| P50179 | E 25/10 19:00 | ID não demonstrado. **[não verificado]** |
| P50883 | E 27/10 19:00 | ID não demonstrado. **[não verificado]** |
| P50169 | E 25/10 21:30 | ID não demonstrado. **[não verificado]** |

A sonda não fornece DNA/ATA dessas células nem as batidas que acenderam suas lâmpadas. Entre B140648–140654, a única entrada mostrada é B140651, de **20/09 19:00**, criada em **29/10**; as demais são saídas. Nenhuma identifica a entrada responsável pelos carimbos apresentados. **[não verificado]**

O trecho também não exibe `respondida_em`; a desigualdade `materializada_em < respondida_em` declarada no pedido não pode ser conferida nele. **[não verificado]**

**3. Leitores/escritores que usam o dia da célula e tratamento do instante noturno.**

O produtor do DNA grava o instante próprio do marco: escolhe **D+1 quando o turno cruza a meia-noite e `hora < hi`**, e escreve esse dia em `instante`. [gerar_celulas.py:67](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/gerar_celulas.py:67).

| Caminho | Uso do dia / instante |
|---|---|
| **Grade consumida pelas perguntas** | Indexa por `c.data`, devolve esse dia e projeta os marcos somente como `tipo/hora`, sem `instante`. [leitor_celula.py:325](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/services/leitor_celula.py:325), [linha 337](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/services/leitor_celula.py:337), [linha 120](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/services/leitor_celula.py:120). |
| **Marcos faltantes** | Retorna `(data_dia, posição, tipo, hora)` usando o dia da grade, inclusive para a saída da véspera; não transporta o instante próprio. [declaracao_texto.py:88](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:88), [linha 116](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:116). |
| **Texto da pergunta complementar** | Formata `{data}` com o `dia` retornado acima e usa esse mesmo dia na criação da pergunta. [disputa_emissao.py:806](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/disputa_emissao.py:806). |
| **Texto nas portas de declaração** | Colaborador e aplicação pelo admin usam o dia do marco selecionado tanto no texto quanto em `data_evento`. [declaracao_texto.py:321](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:321), [linha 610](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/declaracao_texto.py:610). |
| **Carimbo fantasma atual** | Consulta a célula vinculada e sua lâmpada por `hora/tipo`; usa `data_evento` para o encerramento do dia. Não consulta o instante do DNA. [juizes.py:845](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:845), [linha 866](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:866), [reconciliador.py:1038](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:1038). |
| **Plantio de resposta `hora`** | Começa com `data_evento + hora`, mas chama o ajuste overnight antes de devolver o timestamp. O ajuste procura D ou D+1 dentro do envelope da cadeia de marcos, com margem de 90 minutos. **Não permanece obrigatoriamente no dia da célula.** [materializacao.py:398](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:398), [linha 404](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/materializacao.py:404), [juizes.py:301](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/juizes.py:301). |
| **Classificação da validação em lote** | Na busca de conflito, combina diretamente `perg.data_evento + hora respondida`; esse trecho não passa pelo ajuste overnight. [validacao.py:817](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/services/validacao.py:817). |
| **Âncora de cobrança de furo parcial** | Projeta marcos em `tipo/hora/motivo`, ordena por hora e combina a primeira com `data`; essa âncora é enviada à abertura do chamado. [cartorio.py:1467](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/cartorio.py:1467), [linha 1470](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/cartorio.py:1470), [linha 1395](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/cartorio.py:1395). |
| **Reemissão C2** | Busca a célula pelo dia e grava seus marcos apagados no contexto. A projeção contém somente `hora/tipo/motivo`, sem instante. [reconciliador.py:661](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:661), [linha 674](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/reconciliador.py:674), [lastro.py:128](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/lastro.py:128). |

## 30/10 07:52 · sessao 03ab7c3f332f · - · - · origem=Ronald (tmux)

Na cópia, os caminhos relevantes são:

| Caminho | Dia utilizado / tratamento |
|---|---|
| DNA | `montar_lampadas` escolhe D+1 quando o turno cruza e a hora é anterior a `hi`; grava esse dia em `instante`. `app/ponto/management/commands/gerar_celulas.py:67-69`. |
| Grade consumida pelas perguntas | `grade_da_celula` indexa células por `c.data` e devolve esse dia em `base['data']`; `_celulas_da_ata` projeta marcos contendo somente `tipo` e `hora`, sem `instante`. `app/escala/services/leitor_celula.py:120`, `:325`, `:337`, `:354-365`. |
| Marcos faltantes | `_marcos_faltantes` percorre o dia da grade (`data_dia`), extrai somente `tipo/hora` e retorna `(data_dia, pos, tipo, hora)`. `app/chamados/services/declaracao_texto.py:88`, `:109`, `:120`. |
| Texto da pergunta complementar | A fábrica usa o `dia` retornado acima no texto e em `data_evento`; não lê `lampadas[].instante`. `app/chamados/services/disputa_emissao.py:806-813`. |
| Texto nas portas de declaração | A declaração do colaborador usa o `dia` casado ao marco para texto e `data_evento`; a aplicação pelo admin faz o mesmo com `p['dia']`. `app/chamados/services/declaracao_texto.py:296`, `:321-323`, `:576`, `:610-612`. |
| Vínculo pergunta → célula | `PerguntaDisputa.abrir` busca a célula em `data=data_evento`; reaponta a existente ou grava a nova pergunta com essa célula. `app/chamados/models.py:2058-2060`, `:2071-2073`, `:2085-2086`. |
| Carimbo fantasma atual | `fato_do_marco_no_espelho` lê `p.celula`, escolhe a hora em `dna.marcos` e consulta `ata.lampadas` por `(hora,tipo)`, retornando `acesa`. Não casa uma `Batida` e não lê `lampadas[].instante`. `app/chamados/juizes.py:845-849`, `:863-872`. O escritor exige resposta vazia, consulta esse fato e usa `p.data_evento` no teto/encerramento antes de chamar `encerrar_fantasma`. `app/chamados/reconciliador.py:1030-1038`, `:1092`. |
| Plantio de resposta `hora` | Começa com `data_evento + hora`, porém chama ajuste overnight antes de devolver o timestamp. O ajuste reconstrói os marcos, procura D ou D+1 dentro do envelope de 90 minutos e devolve o candidato. Portanto não cabe descrevê-lo como “sempre planta no dia da célula”. `app/chamados/services/materializacao.py:328-329`, `:398`, `:404`; `app/chamados/juizes.py:301-312`. A cadeia reconstrói datas crescentes em `app/escala/servico_jornada.py:81-84`. |
| Classificação de validação em lote | Para procurar conflito, combina diretamente `perg.data_evento + hora respondida` e consulta ±60 minutos, sem passar pelo ajuste overnight nesse trecho. `app/chamados/services/validacao.py:747`, `:817-822`. |
| Âncora de cobrança de furo parcial | Projeta os apagados somente em `tipo/hora/motivo`, ordena por hora, escolhe a primeira e combina essa hora com `data`. Essa âncora é usada na abertura do chamado. `app/ponto/services/cartorio.py:1463-1474`, `:1394-1396`. |
| Reemissão C2 / nomeação de marcos | C2 busca a célula pelo dia e grava `marcos_apagados(cel)` no contexto. Esse leitor projeta somente `hora/tipo/motivo`. `app/chamados/reconciliador.py:661`, `:674-677`; `app/ponto/services/lastro.py:128-135`. O comando `nomear_marco_por_lampada` também usa essa projeção e a grava no contexto. `app/chamados/management/commands/nomear_marco_por_lampada.py:59-61`, `:109`. |

Distinções adicionais para evitar generalização:

- O detector dedicado de intervalo recompõe a cadeia desde `cel.data`, mas grava em cada marco a **data do datetime encadeado**, permitindo D+1. `app/ponto/management/commands/detectar_intervalo_ausente.py:73-80`.
- O veredito humano de dia também recompõe a cadeia e avança o dia quando o marco seguinte não é posterior ao anterior. `app/ponto/services/veredito_celula.py:63-70`, `:229`.
- `juiz_batida` chama o produtor `montar_lampadas(celula.data, ...)` e depois lê o `instante` reconstruído; não mantém todos os marcos em D. `app/ponto/juiz_batida.py:174-176`, `:236`.

## 30/10 07:51 · sessao 2084a93f251f · - · - · origem=Ronald (tmux)

O único escritor ativo encontrado de `via_resolucao='fantasma'` é `PerguntaDisputa.encerrar_fantasma(autor)`: grava via, `timezone.now()` e autor, salvando somente esses três campos; não exige nem grava resposta. [app/chamados/models.py:2162–2174]

O chamador atual é `encerrar_orfas_pela_lampada`, que obtém `get_user_sistema()` e chama a porta em `:1092`. O ID desse usuário não é fixado no código: o helper procura/cria `username='sistema'`; que corresponda ao usuário 666 é **[não verificado]**. [app/chamados/reconciliador.py:1086–1092; app/core/utils.py:6–18]

O predicado atual não busca uma batida por timestamp nem devolve seu ID. Para `saida_sem_entrada`, escolhe `('E','hi')`; para `orfao_14h`, `('S','hf')`. Lê `cel.dna['marcos']`, cruza `cel.ata['lampadas']` por `(hora,tipo)` e retorna se todas as lâmpadas correspondentes estão acesas. Não lê `dna['lampadas'][].instante`. [app/chamados/juizes.py:823–828,842–872]

Antes de carimbar, exige resposta vazia, data presente, fato do marco `True`, pergunta viva, `data_evento < hoje` e encerramento do dia do colaborador. A linha que escolhe o dia para esse encerramento é `dia_encerrado_motivo(p.disputa.colaborador, p.data_evento, agora=agora)`. [app/chamados/reconciliador.py:1030–1040]

O comando `reconciliar_perguntas_orfas` da cópia atual é somente vigia. Seu comentário registra que a regra antiga `_eh_fantasma` / `_bem_formado` / `_turnos_na_janela` foi removida em 29/10; diz que julgava completude do turno e última saída até 180 minutos do fim previsto. O corpo antigo e a execução que produziu carimbos de 22–28/10 não estão demonstrados pelo código atual. [app/ponto/management/commands/reconciliar_perguntas_orfas.py:1–15] **Escritor/predicado histórico exato: [não verificado].**

A sonda colada não contém `respondida_em`, DNA, ATA ou batidas E nos dias 21/23/25/27 às 19:00, nem E em 25/10 às 21:30. Portanto não permite identificar qual batida satisfez P46703/P49422/P50179/P50883/P50169; o predicado atual tampouco identifica a batida. **Casamentos e antecedência `materializada_em < respondida_em`: [não verificado]**.

Citações devem ser expandidas para links absolutos pelo consolidante, e as referências em intervalo podem ser convertidas em linhas individuais.

## 30/10 07:48 · sessao 0b5243c88ab6 · O261-VETO-NA-PORTA · censo · origem=Code

Não existe uma porta única de escrita para nascimento e reabertura: há `get_or_create` em `abrir`, `create` em `criar`, `save` em `_gravar_estado` e um `update` direto no reverso da migration 0038. (`app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`)

O universo estrito é `batida_ausente`, `turno_aberto`, `intervalo_pendente` e `entrada_bloqueada_turno_aberto`; incluí também `turno_aberto_24h`, os emissores de chamados-pais de disputa e as reaberturas genéricas sem filtro que exclua furo. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/catalogo/modulos.py:109`, `app/chamados/services/disputa_emissao.py:507`, `app/chamados/services/acoes_chamado.py:315`)

Para evitar confundir vetos diferentes, as tabelas usam estas referências:

| Referência | Veto efetivamente consultado |
|---|---|
| **Régua** | `motivo_do_veto` consulta `regua_degenerada` e `vetar_cobranca_se_regua_suspeita`. Com vínculo ausente, retorna `None`. (`app/ponto/services/furos_vetados.py:26`, `app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`) |
| **Célula** | `celula_veta_cobranca`: veta `trabalha=False` e os vereditos `nunca_bateu`, `trabalhou`, `justificado`, `coberta`, `cobranca_indevida`. Sem célula, não veta. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:156`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`) |
| **Nascimento** | `abrir` e `criar` consultam `_celula_que_absolve`; somente alcança `MODULOS_DE_TURNO` com célula vinculada: `turno_aberto`, `turno_aberto_24h`, `entrada_bloqueada_turno_aberto`. Portanto, esse veto compartilhado não alcança `batida_ausente` nem `intervalo_pendente`. (`app/chamados/models.py:726`, `app/chamados/models.py:822`, `app/chamados/models.py:849`, `app/chamados/reconciliador.py:336`) |
| **Nascimento — predicado** | `_celula_que_absolve → _absolvido_pela_celula`: recusa quando há veredito não vazio fora de `VEREDITOS_ACUSAM` e não há marcos pendentes. É veto pela célula, não consulta de régua. (`app/chamados/models.py:851`, `app/chamados/reconciliador.py:974`, `app/chamados/reconciliador.py:975`, `app/chamados/reconciliador.py:977`) |
| **Reabertura** | `reabrir → recusa_da_reabertura`: fechamento administrativo sem fato novo; premissa regularizada de furo; agrupamento; irmão vivo; motor. (`app/chamados/models.py:1050`, `app/chamados/models.py:1003`, `app/chamados/models.py:1009`, `app/chamados/models.py:1012`, `app/chamados/models.py:1015`, `app/chamados/models.py:1019`) |

A coluna “antes” considera a escrita indicada. A chamada ao cartório pelo `post_save` do chamado acontece depois dessa escrita e não constitui veto anterior a ela. (`app/chamados/signals.py:164`, `app/chamados/signals.py:180`)

| Caminho — arquivo::função | Onde nasce/reabre | Chama `motivo_do_veto` **antes**? | Outro veto anterior |
|---|---|---|---|
| `app/chamados/models.py::ChamadoColaborador.abrir` — nascimento | `get_or_create` em `app/chamados/models.py:730` | **Não**, no corpo da porta. (`app/chamados/models.py:666`) | **Nascimento**, chamado em `app/chamados/models.py:726`. |
| `app/chamados/models.py::ChamadoColaborador.criar` | `objects.create` em `app/chamados/models.py:834` | **Não**, no corpo da porta. (`app/chamados/models.py:805`) | **Nascimento**, chamado em `app/chamados/models.py:822`. |
| `app/chamados/models.py::ChamadoColaborador.abrir` — dedup encerrado | `_renascer_por_premissa_viva` em `app/chamados/models.py:686` → `reabrir` em `app/chamados/models.py:770` → escrita em `app/chamados/models.py:1089` / `app/chamados/models.py:231` | **Não**, nessa cadeia. (`app/chamados/models.py:742`, `app/chamados/models.py:1023`) | Ativo, não cascata, competência não trancada; depois **Reabertura**. O ramo retorna antes do veto **Nascimento**. (`app/chamados/models.py:764`, `app/chamados/models.py:766`, `app/chamados/models.py:768`, `app/chamados/models.py:688`) |
| `app/chamados/models.py::ChamadoColaborador.reabrir` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | **Não**. (`app/chamados/models.py:1023`) | **Reabertura**, em `app/chamados/models.py:1050`. |
| `app/chamados/models.py::ChamadoColaborador.transicionar` — destino vivo | `_aplicar` em `app/chamados/models.py:183` → `_gravar_estado` em `app/chamados/models.py:260` → `save` em `app/chamados/models.py:231` | **Não**. (`app/chamados/models.py:122`) | Matriz/perfil do motor. As guardas de justificativa, disputa e premissa são para fechamento/resolução; essa direção não passa por **Reabertura**. (`app/chamados/models.py:126`, `app/chamados/models.py:129`, `app/chamados/models.py:142`, `app/chamados/models.py:165`) |
| `app/ponto/management/commands/detectar_ausencias.py::Command.handle → _abrir_chamado` | `handle` chama em `app/ponto/management/commands/detectar_ausencias.py:439`; `_abrir_chamado` chama `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **Não** na cadeia própria do detector. (`app/ponto/management/commands/detectar_ausencias.py:570`) | Régua degenerada e suspeita diretamente; feriado; posto fechado; **Célula**; existência de célula, com exceção de limbo. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`, `app/ponto/management/commands/detectar_ausencias.py:616`, `app/ponto/management/commands/detectar_ausencias.py:623`, `app/ponto/management/commands/detectar_ausencias.py:626`, `app/ponto/management/commands/detectar_ausencias.py:654`) |
| `app/ponto/management/commands/emitir_furo_retroativo.py::Command.handle` | Alias `_Emissor` importado em `app/ponto/management/commands/emitir_furo_retroativo.py:38`; chama `_abrir_chamado` em `app/ponto/management/commands/emitir_furo_retroativo.py:142` → detector `:688` acima | **Não** nessa cadeia. (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | Herda os vetos do detector; antes exige classificação cobravel e dia de trabalho. (`app/ponto/management/commands/emitir_furo_retroativo.py:109`, `app/ponto/management/commands/emitir_furo_retroativo.py:127`) |
| `app/ponto/management/commands/reconciliar_grade.py::Command.handle → _emitir_furo` | Chamada em `app/ponto/management/commands/reconciliar_grade.py:254`; `abrir` em `app/ponto/management/commands/reconciliar_grade.py:499`; reabertura explícita adicional em `app/ponto/management/commands/reconciliar_grade.py:520` | **Não**. (`app/ponto/management/commands/reconciliar_grade.py:480`) | Régua degenerada e suspeita diretamente; **Célula**; eventual reabertura herda **Reabertura**. (`app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/ponto/management/commands/reconciliar_grade.py:520`) |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — integral, cobrança morta e limbo de hoje | Delega ao detector em `app/ponto/services/cartorio.py:1358` → `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **Sim:** impressão em `app/ponto/services/cartorio.py:1083` → `_veto_do_dia` → alias `_motivo_veto` em `app/ponto/services/cartorio.py:1013`, importado em `app/ponto/services/cartorio.py:1007`. | O resultado entra na impressão; o bloqueio de régua da emissão delegada ocorre no detector. Há também piso, vivo existente, desligamento e afastamento. (`app/ponto/services/cartorio.py:1319`, `app/ponto/services/cartorio.py:1327`, `app/ponto/services/cartorio.py:1339`, `app/ponto/services/cartorio.py:1344`, `app/ponto/management/commands/detectar_ausencias.py:600`) |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — furo parcial | Delega ao detector em `app/ponto/services/cartorio.py:1394` → `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **Sim**, pela mesma cadeia `app/ponto/services/cartorio.py:1083` → `app/ponto/services/cartorio.py:1013`. | Piso, desligamento, afastamento, ausência de chamado vivo; depois vetos do detector. (`app/ponto/services/cartorio.py:1381`, `app/ponto/services/cartorio.py:1386`, `app/ponto/services/cartorio.py:1393`) |
| `app/chamados/reconciliador.py::reemitir_chamado_do_dia` — c2 | `abrir` em `app/chamados/reconciliador.py:862` | **Sim, condicionado a `not _costura_no_vivo`:** alias `_mv_c2` importado em `app/chamados/reconciliador.py:853`, chamado em `app/chamados/reconciliador.py:854`; veto retorna antes de abrir em `app/chamados/reconciliador.py:855`. | Ativo, horizonte, isenção, ausência, protesto cobravel, **Célula** e motor. (`app/chamados/reconciliador.py:789`, `app/chamados/reconciliador.py:809`, `app/chamados/reconciliador.py:819`, `app/chamados/reconciliador.py:821`, `app/chamados/reconciliador.py:836`, `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:858`) |
| `app/ponto/management/commands/processar_alertas_turno.py::Command.handle → _marco_critico → _abrir_chamado_critico` | `app/ponto/management/commands/processar_alertas_turno.py:141` → `app/ponto/management/commands/processar_alertas_turno.py:316` → `abrir` em `app/ponto/management/commands/processar_alertas_turno.py:335` | **Não**. (`app/ponto/management/commands/processar_alertas_turno.py:324`) | Isenção, ausência, hora extra permitida; depois **Nascimento** ou **Reabertura**, conforme o ramo de `abrir`. (`app/ponto/management/commands/processar_alertas_turno.py:85`, `app/ponto/management/commands/processar_alertas_turno.py:97`, `app/ponto/management/commands/processar_alertas_turno.py:135`) |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `criar`, módulo `turno_aberto_24h`, em `app/ponto/management/commands/processar_alertas_avancados.py:121` | **Não**. (`app/ponto/management/commands/processar_alertas_avancados.py:121`) | Isenção, dia de trabalho, turno aberto, ausência e **Nascimento**. (`app/ponto/management/commands/processar_alertas_avancados.py:32`, `app/ponto/management/commands/processar_alertas_avancados.py:49`, `app/ponto/management/commands/processar_alertas_avancados.py:63`, `app/ponto/management/commands/processar_alertas_avancados.py:116`) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `abrir`, módulo `turno_aberto_24h`, em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146` | **Não**. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146`) | Turno aberto, tempo mínimo, vínculo, ausência e dedup; depois portas do modelo. O dry consulta `celula_absolveria`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:101`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:105`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`) |
| `app/ponto/views.py::bater_ponto` | Alias `_Chamado.criar` em `app/ponto/views.py:291`, módulo `entrada_bloqueada_turno_aberto` em `app/ponto/views.py:299` | **Não** na cadeia específica desse nascimento. (`app/ponto/views.py:275`) | Condição de bloqueio/regularização e dedup de vivos. O contexto passado contém apenas `pendentes` e `total`, sem dia/célula; **Nascimento** depende de célula vinculada. (`app/ponto/views.py:275`, `app/ponto/views.py:280`, `app/ponto/views.py:281`, `app/chamados/models.py:849`) |
| `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | View chama em `app/chamados/views.py:197`; serviço chama `criar` em `app/chamados/services/abertura.py:47` | **Não**. (`app/chamados/services/abertura.py:19`) | A view aceita módulo presente em `MOTIVOS`, incluindo os de furo. Herda **Nascimento**, mas o contexto desse serviço não fornece `data_turno`. (`app/chamados/views.py:170`, `app/chamados/views.py:174`, `app/chamados/catalogo/modulos.py:105`, `app/chamados/services/abertura.py:58`) |

Os dois emissores que consultam régua diretamente usam o mesmo predicado de degeneração e a mesma função de suspeita de `motivo_do_veto`. Há diferença com `ec=None`: `motivo_do_veto` não veta; `vetar_cobranca_se_degenerada` trata a escala ausente como degenerada. (`app/ponto/services/furos_vetados.py:26`, `app/ponto/services/furos_vetados.py:28`, `app/ponto/services/furos_vetados.py:30`, `app/escala/regua_defesa.py:23`, `app/escala/regua_defesa.py:45`, `app/escala/regua_defesa.py:46`)

As reaberturas adicionais são estas; as linhas genéricas abaixo operam sobre o chamado selecionado, sem restringir a operação aos módulos de disputa. (`app/chamados/services/acoes_chamado.py:315`, `app/chamados/services/fio.py:705`, `app/chamados/views_arquivar.py:58`, `app/chamados/services/competencia_trancada.py:252`)

| Caminho — arquivo::função | Onde reabre | `motivo_do_veto` antes? | Outro veto anterior |
|---|---|---|---|
| `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | Alias `_rt.receber` em `app/chamados/views.py:1053` → `_renascer_por_premissa_viva` em `app/chamados/services/resposta_tardia.py:45` | **Não**. (`app/chamados/services/resposta_tardia.py:31`) | Exige célula em `VEREDITOS_ACUSAM`; depois ativo/cascata/tranca e **Reabertura**. (`app/chamados/services/resposta_tardia.py:44`, `app/chamados/services/resposta_tardia.py:45`, `app/chamados/models.py:764`, `app/chamados/models.py:770`) |
| `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | Delegação em `app/chamados/views.py:547` → `transicionar` em `app/chamados/services/acoes_chamado.py:315` | **Não**. (`app/chamados/services/acoes_chamado.py:283`) | Gestor, status válido e motor; sem veto equivalente de premissa para destino vivo. (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/models.py:126`, `app/chamados/models.py:165`) |
| `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | Delegações em `app/chamados/views.py:674` e `app/chamados/views.py:954` → `transicionar` em `app/chamados/services/fio.py:715` | **Não**. (`app/chamados/services/fio.py:702`) | Autorização por área, status válido e motor; sem passar por **Reabertura**. (`app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:713`, `app/chamados/models.py:183`) |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `reabrir` em `app/chamados/views_arquivar.py:68` | **Não**. (`app/chamados/views_arquivar.py:52`) | Permissão/área, via de competência trancada e **Reabertura**. (`app/chamados/views_arquivar.py:54`, `app/chamados/views_arquivar.py:60`, `app/chamados/views_arquivar.py:64`) |
| `app/ponto/services/fechamento.py::reabrir_periodo → app/chamados/services/competencia_trancada.py::reabrir` | Alias `_reabrir_fila` em `app/ponto/services/fechamento.py:1415`, chamado em `app/ponto/services/fechamento.py:1416` → `ch.reabrir` em `app/chamados/services/competencia_trancada.py:258` | **Não**. (`app/chamados/services/competencia_trancada.py:228`) | Via de encerramento, dia dentro da competência e **Reabertura**. (`app/chamados/services/competencia_trancada.py:252`, `app/chamados/services/competencia_trancada.py:255`) |
| `app/chamados/services/fio.py::processar_post_fio → app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | Delegação em `app/chamados/services/fio.py:577` → `ch.reabrir` em `app/chamados/services/acoes_disputa.py:266` | **Não**. (`app/chamados/services/acoes_disputa.py:245`) | Motivo/justificativa, disputa fechada, chamado encerrado e **Reabertura**. (`app/chamados/services/acoes_disputa.py:248`, `app/chamados/services/acoes_disputa.py:250`, `app/chamados/services/acoes_disputa.py:252`, `app/chamados/services/acoes_disputa.py:265`) |
| `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `_pai.reabrir` em `app/chamados/services/disputa_emissao.py:1606` | **Não** nessa cadeia. (`app/chamados/services/disputa_emissao.py:1596`) | Disputa aberta, pai não vivo, irmãs respondidas sem consumo e **Reabertura**. (`app/chamados/services/disputa_emissao.py:1599`, `app/chamados/services/disputa_emissao.py:1602`, `app/chamados/services/disputa_emissao.py:1604`) |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` | **Não**. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`) | Consulta antecipada à própria recusa da reabertura e às outras perguntas que ressuscitariam; depois **Reabertura**. O alvo é o pai da disputa. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:96`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`) |
| `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` | `update(status_local='aberto')` em `app/chamados/migrations/0038_a3_registrado_backfill.py:15` | **Não**. (`app/chamados/migrations/0038_a3_registrado_backfill.py:13`) | Nenhum veto de régua/célula/premissa. Seleciona todos os `registrado`, sem filtro de módulo. É vivo → vivo, pois `registrado` integra `VIVOS`. (`app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/catalogo/motor.py:24`) |

A guarda **Reabertura** consulta premissa regularizada, mas não chama `motivo_do_veto`: lê justificativa aprovada, célula/veredito administrativo, marcos acesos ou inexistentes, `_celula_furada` e os fatos de fallback. (`app/chamados/models.py:937`, `app/chamados/models.py:951`, `app/chamados/models.py:959`, `app/chamados/models.py:968`, `app/chamados/models.py:970`, `app/chamados/models.py:975`, `app/chamados/models.py:985`)

Em `disputa_emissao`, há duas escritas distintas: o chamado-pai da disputa e, pelo c2, o chamado de furo do dia. O veto do segundo não antecede retroativamente a criação do primeiro. (`app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:606`, `app/chamados/services/disputa_emissao.py:409`, `app/chamados/reconciliador.py:862`)

| Caminho — arquivo::função | Nascimento | `motivo_do_veto` antes? | Outro veto anterior |
|---|---|---|---|
| `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `abrir` em `app/chamados/services/disputa_emissao.py:504`; módulo `disputa_supervisao` em `app/chamados/services/disputa_emissao.py:507` | **Não** no núcleo. (`app/chamados/services/disputa_emissao.py:419`) | Dedup/agregação; esse módulo não pertence a `MODULOS_DE_TURNO`, portanto **Nascimento** não o barra por célula. (`app/chamados/services/disputa_emissao.py:502`, `app/chamados/reconciliador.py:336`, `app/chamados/models.py:849`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `abrir` em `app/chamados/services/disputa_emissao.py:908`; módulo manual em `app/chamados/services/disputa_emissao.py:911` | **Não** no agrupador. (`app/chamados/services/disputa_emissao.py:847`) | Portas do modelo; módulo fora de `MODULOS_DE_TURNO`. (`app/chamados/services/disputa_emissao.py:911`, `app/chamados/models.py:849`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `abrir` em `app/chamados/services/disputa_emissao.py:1128`; módulo manual em `app/chamados/services/disputa_emissao.py:1131` | **Não**. (`app/chamados/services/disputa_emissao.py:1093`) | Recusa chamado-limbo encerrado; módulo fora do veto **Nascimento**. (`app/chamados/services/disputa_emissao.py:1109`, `app/chamados/models.py:849`) |
| `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | Serviço chamado em `app/chamados/views.py:136`; `criar` em `app/chamados/services/abertura.py:172` | **Não**. (`app/chamados/services/abertura.py:122`) | Tipos permitidos e exigência de dia. A porta permite `disput

[truncado em 20000 de 31032 caracteres]

## 30/10 07:45 · sessao 526619754147 · - · - · origem=Ronald (tmux)

Consolidado da varredura global, para unir às portas de `models.py` e aos emissores centrais.

`R` abaixo = `reabrir` → `recusa_da_reabertura` (`app/chamados/models.py:1050`) → vetos por fechamento humano sem fato novo (`:1003`), premissa regularizada de chamado de furo (`:1009`), agrupamento (`:1012`), irmão vivo (`:1015`) e transição do motor (`:1019`) → `_gravar_estado` (`:1089`) → `save` (`:231`). Essa cadeia não chama `motivo_do_veto`; a premissa consulta justificativa aprovada (`:937`), célula/veredito humano (`:951`), marcos acesos/inexistentes (`:959`, `:968`), `_celula_furada` (`:970`), feriado (`:976`) e premissa por fatos (`:985`). Não é o mesmo veto de régua de `furos_vetados`.

| Caminho | Nascimento/reabertura | `motivo_do_veto` antes? | Outros vetos comprovados |
|---|---|---|---|
| `app/ponto/views.py::bater_ponto` | `_Chamado.criar` em `:291`, módulo `entrada_bloqueada_turno_aberto` em `:299` | Não | Só condições de entrada, bloqueio por turno e ausência de regularização pendente (`:275`), dedup de vivos (`:281`). Contexto contém apenas `pendentes/total` (`:280`): não informa dia/célula à porta. |
| `app/ponto/management/commands/processar_alertas_turno.py::_abrir_chamado_critico` | `ChamadoColaborador.abrir` em `:335`, módulo `turno_aberto` (`:337`) | Não | No `handle`: isenção (`:85`), ausência (`:97`), vínculo quando necessário (`:106`), HE permitida (`:135`). Cadeia `handle :141` → `_marco_critico :316` → `_abrir_chamado_critico :335`; herda veto de célula do `abrir`. |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `.criar` em `:121`, módulo `turno_aberto_24h` (`:123`) | Não | Isenção (`:32`), vínculo (`:37`), dia de trabalho (`:49`), turno efetivamente aberto (`:63`), ausência (`:116`); herda veto de célula do `criar`. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `.abrir` em `:146`, módulo `turno_aberto_24h` (`:147`) | Não | Turno aberto (`:92`), 24h (`:101`), vínculo (`:104`), ausência (`:108`), dedup (`:118`); herda veto de célula do `abrir`. Dry pergunta explicitamente `celula_absolveria` em `:134`. |
| `app/chamados/services/resposta_tardia.py::receber` | `_renascer_por_premissa_viva()` em `:45` → `models.py:770` → R | Não | Exige célula com `VEREDITOS_ACUSAM` (`:44`, `:45`); renascer exige ativo (`models.py:764`), não cascade (`:766`), competência não trancada (`:768`), depois R. |
| `app/chamados/services/acoes_chamado.py::acao_resposta_livre` | `.transicionar(novo_status)` em `:315` → `models.py:183` → `_aplicar :260` → `_gravar_estado :231` | Não | Para destino vivo: matriz e perfil do motor (`models.py:126`; `catalogo/motor.py:139`). Não passa por R nem sua premissa. |
| `app/chamados/services/fio.py::processar_post_fio` | `.transicionar` em `:715` → mesma cadeia humana acima | Não | Autorização por área em `:708`; matriz/perfil do motor. Não passa por R nem sua premissa. |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `.reabrir()` em `:68` → R | Não | Permissão (`:54`), área (`:60`), somente via de competência trancada entra nessa reabertura (`:64`), depois R. |
| `app/chamados/services/competencia_trancada.py::reabrir` | `.reabrir()` em `:258` → R | Não | Seleção por via de encerramento (`:252`) e dia da competência (`:254`); depois R. |
| `app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `.reabrir()` em `:266` → R | Não | Admin (`:242`), motivo e justificativa (`:248`, `:250`), disputa fechada (`:252`), `reabrir_admin` (`:256`), chamado encerrado (`:265`), depois R. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir()` em `:161` → R | Não | Lâmpada apagada e pergunta viva (`:82`), janela (`:87`), competência não exportada (`:98`), pré-consulta de R (`:105`) e ausência de irmãs ressuscitáveis (`:106`); depois R. |

Entradas das reaberturas genéricas:

- Resposta livre: `app/chamados/views.py::detalhe_chamado_local` (`:359`) → `acao_resposta_livre` (`:547`) → transição (`app/chamados/services/acoes_chamado.py:315`).
- Fio: `app/chamados/views.py::fio_colaborador` (`:659`) → `processar_post_fio` (`:674`); `modal_fio_colaborador` (`:869`) → mesma função (`:954`) → transição (`app/chamados/services/fio.py:715`).
- Reabertura administrativa: esses dois fios → `app/chamados/services/fio.py:577` → `acao_reabrir_disputa_admin` → chamado (`app/chamados/services/acoes_disputa.py:266`).
- Resposta tardia: `app/chamados/views.py::responder_chamado_colaborador` (`:1041`) → alias `_rt.receber` (`:1053`) → renascimento (`app/chamados/services/resposta_tardia.py:45`).
- Competência: `app/ponto/views.py::reabrir_periodo` (`:1521`) → alias `_fech.reabrir_periodo` (`:1544`) → `app/ponto/services/fechamento.py:1416`, alias `_reabrir_fila` importado em `:1415` → `app/chamados/services/competencia_trancada.py:258`.

Atenção à migration: `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` escreve diretamente `registrado → aberto` em `:15`, **sem filtro de módulo**, sem `motivo_do_veto` e sem portas do modelo. O caminho de ida restringe `AUDITORIA` (`:3`, `:10`); o reverso não restringe (`:15`). Portanto não se pode excluir seu reverso apenas porque a ida é de auditoria.

Varredura AST, sem executar aplicação, de `app/`, `mensageria/`, `bin/`, excluindo `tests/` e arquivos `test*`: a única atribuição de atributo `status_local` encontrada foi `app/chamados/models.py:192`; chamadas ORM com esse keyword foram `models.py:834`, migration `0038:10/:15` e `0039:24/:33`. Não apareceu `bulk_update` com `status_local` nem `bulk_create` de ChamadoColaborador. A ausência global é resultado da varredura [não verificável por referência única de arquivo:linha].

Wrappers do cartório que podem chegar aos emissores, herdando suas guardas:

| Entrada | Chamada ao cartório |
|---|---|
| `app/ponto/management/commands/processar_cartorio.py::Command.handle` | `julgar_colab` em `:65` |
| `app/ponto/services/vigia_de_hora.py::rodar` | `julgar_celula` em `:130` |
| `app/chamados/services/cobrar_dia.py::cobrar_dia` | `julgar_celula(..., toque=True)` em `:29` |
| `app/ponto/services/furos_sem_cobranca.py::cobrar` | `julgar_celula` em `:128`; command `cobrar_furos_sem_cobranca.py:30` chama `cobrar` |
| `app/ponto/signals.py::_julgar_dias` | `cartorio.julgar_celula` em `:93`; recebe Batida save `:135`/delete `:140`, Ausencia save `:147`/delete `:152`, Justificativa save `:165` |
| `app/chamados/signals.py::_cartorio_post_save_chamado` | `cartorio.julgar_celula` em `:180` — **depois** do save de chamado (`:164`), portanto não conta como veto anterior desse nascimento/reabertura |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | `julgar_celula` em `:372` |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `julgar_celula` em `:72` |
| `app/escala/regua_defesa.py::rejulgar_furos_do_vinculo` | `julgar_colab` em `:692` |
| `app/ponto/management/commands/celula_veredito_velho.py::Command.handle` | `julgar_colab` em `:144` |
| `app/ponto/management/commands/curar_residuo_bug115.py::Command._lavrar` | `julgar_celula` em `:143` |
| `app/ponto/management/commands/rejulgar_ausencia_em_folga.py::Command.handle` | `julgar_celula` em `:71` |
| `bin/rejulgar_o118_col221.py::<módulo>` | `julgar_colab` em `:24` |
| `bin/r5_idempotencia_frota.py::rodada` | `call_command('processar_cartorio', ..., apply=True)` em `:109` |

Fora do universo solicitado: `adesao.py` reabre apenas pai `sem_previsao_escala` (`app/chamados/services/adesao.py:75`, `:92`); `reabrir_chamados_ausencia_aberta` seleciona `ausencia_colab` (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:29`, `:49`). `detectar_intervalo_ausente` atualmente só reconcilia e encerra o laço antes de qualquer emissão (`app/ponto/management/commands/detectar_intervalo_ausente.py:134`, `:152`); não há nascimento ali.

Módulo adjacente, caso o censo inclua também alarmes de batidas espúrias: `app/ponto/services/cluster_espurio.py::reconciliar_colab` chama `ChamadoColaborador.abrir` em `:84`, módulo `cluster_batida_espuria` (`:11`, `:86`), sem `motivo_do_veto`; exige ativo (`:73`), escala ativa (`:75`), célula trabalha e sem `sem_ancora` (`:24`), cluster e marco apagado (`:27`, `:30`). Esse módulo não é um dos quatro de `furo_modulos.py:15`, nem um `MODULOS_DE_TURNO`.

## 30/10 07:45 · sessao 0fb2da7ae025 · O261-V · leitura · origem=Code

**O código demonstra o escritor e a derivação do instante; o anexo não demonstra o vínculo criador de cada B# com uma P#.** Esse vínculo é gravado em `pergunta_origem`, campo ausente da sonda fornecida. ([materializacao.py:891](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:891))

**1. Escritor e escolha do dia**

O escritor da resposta de hora é `_materializar_resposta_hora`: deriva o instante em `materializacao.py:786` e chama `registrar_batida` com `origem='disputa_s84_retro'` em `:872–879`. O `INSERT` ocorre em `registro_batida.py:157–159`. ([materializacao.py:786](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:786), [materializacao.py:872](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:872), [registro_batida.py:157](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/registro_batida.py:157))

A derivação segue esta sequência:

- Usa `p.data_evento`; na ausência, `disputa.contexto[p.motivo]['data']`. ([materializacao.py:328](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:328))
- Extrai a hora de `resposta_colab`, combina inicialmente **data-base + 07:00** e chama `_ajustar_saida_overnight`. ([materializacao.py:391](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:391), [materializacao.py:398](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:398), [materializacao.py:404](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/materializacao.py:404))
- A cadeia `19:00 → 07:00` avança o segundo marco para D+1: **a virada está em `servico_jornada.py:82–83`**. ([servico_jornada.py:82](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/escala/servico_jornada.py:82))
- O ajuste monta o envelope **D 17:30 até D+1 08:30**. **A escolha final do dia está em `juizes.py:308–311`**: testa D e D+1, retornando o primeiro candidato dentro do envelope. D 07:00 fica fora; D+1 07:00 fica dentro. ([juizes.py:305](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/juizes.py:305), [juizes.py:308](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/juizes.py:308))

Os IDs e horários abaixo são os apresentados no anexo; a correspondência criadora permanece sem comprovação:

| Batida apresentada | Pergunta/resposta apresentada na célula | Vínculo criador |
|---|---|---|
| B#140648 — 20/10 07:00 | Pergunta correspondente não fornecida | [nao verificado] |
| B#140649 — 22/10 07:00 | P#46704 — `07:00`, célula 21/10 | [nao verificado] |
| B#140652 — 24/10 07:00 | P#49423 — `07:00`, célula 23/10 | [nao verificado] |
| B#140653 — 26/10 07:00 | P#50180 — `07:00`, célula 25/10 | [nao verificado] |
| B#140654 — 28/10 07:00 | P#51025 — `07:00`, célula 27/10 | [nao verificado] |

B#140650 e B#140651 não aparecem no dado fornecido. [nao verificado]

**2. Qual órfã a pergunta escolhe**

**Nesse caminho, a fábrica não escolhe uma batida órfã por B# ou instante.** Ela lê os marcos faltantes da ata e converte posição/tipo em motivo; `('fim', 'S')` vira `orfao_14h`. ([fabrica_da_celula.py:48](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:48), [fabrica_da_celula.py:59](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:59), [declaracao_texto.py:44](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/declaracao_texto.py:44))

A seleção percorre os slots da ata, exige `missing`, identifica o último como `fim` e devolve `(dia, posição, tipo, hora)`. Esse trecho não seleciona elementos de `orfas`. ([declaracao_texto.py:97](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/declaracao_texto.py:97), [declaracao_texto.py:106](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/declaracao_texto.py:106))

O texto canônico é **“Confirme o horário da saída no dia {data}”**. A fábrica fornece o dia da célula no contexto; o formatador interpola esse dia no texto. ([perguntas.py:49](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/catalogo/perguntas.py:49), [fabrica_da_celula.py:122](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:122), [query.py:58](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/catalogo/query.py:58))

Portanto, atribuir essas perguntas especificamente às batidas das **06:55:11, 06:52:47, 06:58:07 ou 06:46:01** não está demonstrado: faltam o texto gravado, `data_evento` e a ata das perguntas históricas. [nao verificado]

**3. O instante plantado corresponde ao descrito?**

**Há divergência entre o dia civil expresso pelo texto canônico e a interpretação noturna do plantio.** A fábrica escreve “saída no dia D”; a resposta `07:00`, interpretada nesse texto, descreve D 07:00. O ajuste planta D+1 07:00 para o turno D 19:00 → D+1 07:00. A mudança acontece em **`juizes.py:308–311`**; o dia D do texto vem de **`fabrica_da_celula.py:122`**. ([perguntas.py:49](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/catalogo/perguntas.py:49), [fabrica_da_celula.py:122](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:122), [juizes.py:308](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/juizes.py:308))

Que esse tenha sido o texto efetivamente mostrado nas quatro P# históricas: [nao verificado].

**4. Por que `21/10=[]` e os outros dias recebem uma hora**

`InsumosDoDono` lê a janela **[ini − 12h, fim + 1 dia + 14h]**, converte os timestamps para horário local e chama `minutos_do_oraculo(ts)` sem pares ou envelopes. `batidas_apuraveis` filtra por timestamp e retratação; não exclui `disputa_s84_retro` pela origem. ([e6_oraculo.py:124](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/e6_oraculo.py:124), [turnos.py:2838](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/turnos.py:2838))

Nesse ramo, o núcleo corta quando há contagem par e gap ≥8h, gap >14h ou vão total >14h. **Todas as horas do grupo são atribuídas à data da primeira batida**, em `nucleo.py:237–238`. ([nucleo.py:215](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:215), [nucleo.py:237](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:237))

Executei essa função da cópia sobre os timestamps fornecidos, com recorte inferior em **20/10 12:00**. O resultado reproduz a assimetria: [nao verificado]

| Trecho alimentado | Resultado da regra |
|---|---|
| 20/10 18:53 → 21/10 06:55 | Duas batidas no grupo iniciado em **20/10**; `21/10=[]` |
| 22/10 07:00 plantada → 22/10 19:01 | Fecha contagem par; o gap seguinte ≥8h separa 23/10 06:52 |
| 24/10 07:00 plantada → 24/10 19:11 | Mesmo corte; 25/10 recebe somente `06:58` |
| 26/10 07:00 plantada → 26/10 19:03 | Mesmo corte; 27/10 recebe somente `06:46` |

Essa reprodução decorre das condições de corte e da atribuição à primeira data. ([nucleo.py:216](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:216), [nucleo.py:237](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:237))

Incluindo também **B#140648 de 20/10 07:00**, a mesma função devolve `21/10=['06:55']`: o grupo anterior já contém duas batidas antes da manhã de 21/10, e ocorre o corte. Logo, a reprodução distingue precisamente o efeito da borda sobre a primeira plantada. ([nucleo.py:216](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/calculador/nucleo.py:216))

**Os parâmetros `ini/fim` efetivamente usados pela sonda não foram apresentados; afirmar que ela usou esse recorte exato permanece [nao verificado].**

Para o dono, uma hora satisfaz a condição de contagem ímpar; zero horas não satisfaz. O código também consulta batida fora de turno antes da paridade e, depois, CxR. ([e6_oraculo.py:96](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/e6_oraculo.py:96))

**5. Por que não aparecem em `dias_cadastro_x_realidade`**

A regra exige **duas distâncias existentes, ambas com valor absoluto >180 minutos**; o operador é `AND`. As distâncias comparam instantes completos, incluindo a data. A inclusão na lista acontece somente após esse predicado. ([motor_calculo_v2.py:1885](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1885), [motor_calculo_v2.py:1911](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1911), [motor_calculo_v2.py:1926](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1926))

**Os grupos cronológicos do oráculo não são os pares do motor.** O Motor12x36 delega o pareamento à autoridade; uma saída órfã sem entrada é descartada dos períodos pelo adaptador. Um período aberto retorna antes do julgamento de pontualidade. ([motor_calculo_v2.py:2686](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:2686), [adaptador_motor.py:30](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/adaptador_motor.py:30), [motor_calculo_v2.py:2690](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:2690))

A sonda informa ausência desses dias na lista, mas não fornece os períodos efetivamente julgados, os papéis da ata nem os dois deltas. **Qual condição impediu a inclusão de cada dia, individualmente: [nao verificado].** A presença de furo ou órfã, isoladamente, não satisfaz o predicado da L-084. ([motor_calculo_v2.py:1885](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/motor_calculo_v2.py:1885))

## 30/10 07:44 · sessao 089b01bfeed8 · - · - · origem=Ronald (tmux)

Portas de escrita verificadas:

| Porta | Escrita efetiva | `motivo_do_veto` antes? | Outro veto |
|---|---|---|---|
| `app/chamados/models.py::abrir` | `get_or_create`, `app/chamados/models.py:730` | Não. | `_celula_que_absolve`, `app/chamados/models.py:726`, chama `app/chamados/reconciliador.py::_absolvido_pela_celula`, `app/chamados/models.py:848`. Aplica somente a `MODULOS_DE_TURNO` com `celula_id`, `app/chamados/models.py:849`; rejeita nascimento sob veredito não vazio, fora de `VEREDITOS_ACUSAM`, sem marcos pendentes, `app/chamados/reconciliador.py:974`. |
| `app/chamados/models.py::criar` | `objects.create`, `app/chamados/models.py:834` | Não. | Mesma consulta `_celula_que_absolve`, `app/chamados/models.py:822`. |
| `app/chamados/models.py::abrir`, dedup encerrado | Chama `_renascer_por_premissa_viva`, `app/chamados/models.py:686`; chega a `_gravar_estado`, `app/chamados/models.py:1089`, e `save`, `app/chamados/models.py:231` | Não. | Colaborador ativo, cascata e competência trancada, `app/chamados/models.py:764`; depois as guardas de `reabrir`. |
| `app/chamados/models.py::reabrir` | `_gravar_estado`, `app/chamados/models.py:1089`; `save`, `app/chamados/models.py:231` | Não. | `recusa_da_reabertura`, `app/chamados/models.py:1050`: fato novo após fechamento humano, `app/chamados/models.py:1003`; `premissa_regularizada` de furo, `app/chamados/models.py:1009`; agrupado, irmão vivo e motor, `app/chamados/models.py:1012`. |
| `app/chamados/models.py::transicionar` com destino vivo | `_aplicar`, `app/chamados/models.py:183`; `_gravar_estado`, `app/chamados/models.py:260`; `save`, `app/chamados/models.py:231` | Não. | Sem veto de premissa de furo na direção viva: as guardas específicas são para destino `fechado` ou `resolvido`, `app/chamados/models.py:129`, `app/chamados/models.py:142`, `app/chamados/models.py:165`. O motor permite `resolvido → em_analise`, `app/chamados/catalogo/motor.py:102`, e exceção administrativa de transição, `app/chamados/catalogo/motor.py:150`. |

`premissa_regularizada` consulta justificativa aprovada, `app/chamados/models.py:937`; veredito administrativo, dia sem trabalho e marcos acesos/inexistentes, `app/chamados/models.py:950`; `_celula_furada`, `app/chamados/models.py:970`; feriado/folga, `app/chamados/models.py:975`; fatos da premissa, `app/chamados/models.py:978`.

Chamadores relevantes:

| Caminho | Ponto de abertura/reabertura | Veto anterior |
|---|---|---|
| `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | View chama serviço em `app/chamados/views.py:197`; serviço chama `criar` em `app/chamados/services/abertura.py:47`; criação física em `app/chamados/models.py:834`. | Não chama `motivo_do_veto`; herda veto de `criar`. A view aceita qualquer chave de `MOTIVOS` em `app/chamados/views.py:173`; módulo vem do POST em `app/chamados/views.py:170`. O contexto do serviço não fornece `data_turno`, `app/chamados/services/abertura.py:58`. |
| `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | Serviço chamado em `app/chamados/views.py:136`; `criar` em `app/chamados/services/abertura.py:172`. | Não chama `motivo_do_veto`; herda veto de `criar`. Tipos permitidos são validados em `app/chamados/services/abertura.py:148`. `disputa_supervisao_manual` é abrível nessa porta, `app/chamados/catalogo/modulos.py:146`; os módulos `batida_ausente`, `turno_aberto`, `turno_aberto_24h`, `intervalo_pendente` não têm declaração de abertura humana, `app/chamados/catalogo/modulos.py:105`. |
| `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:504`; módulo `disputa_supervisao`, `app/chamados/services/disputa_emissao.py:507`. | Não chama `motivo_do_veto`; herda `abrir`. |
| `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `abrir`, `app/chamados/services/disputa_emissao.py:908`; módulo `disputa_supervisao_manual`, `app/chamados/services/disputa_emissao.py:911`. | Não chama `motivo_do_veto`; herda `abrir`. |
| `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `abrir`, `app/chamados/services/disputa_emissao.py:1128`; módulo manual, `app/chamados/services/disputa_emissao.py:1131`. | Não chama `motivo_do_veto`; herda `abrir`; rejeita chamado-limbo encerrado em `app/chamados/services/disputa_emissao.py:1109`. |
| `app/chamados/services/disputa_emissao.py::gerar_perguntas_disputa` | Pergunta recém-criada sem destino chama `reatar_pergunta_orfa`, `app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:409`; a cadeia segue ao reconciliador. | O veto do ramo de emissão de chamado do dia deve ser atribuído à cadeia do reconciliador; os chamados-pais acima são escritas anteriores e distintas, `app/chamados/services/disputa_emissao.py:504`. |
| `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `_pai.reabrir`, `app/chamados/services/disputa_emissao.py:1606`. | Não chama `motivo_do_veto`; exige disputa aberta e resposta sem consumo creditada ao pai, `app/chamados/services/disputa_emissao.py:1599`, `app/chamados/services/disputa_emissao.py:1604`, além das guardas de `reabrir`. |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `ch.reabrir`, `app/chamados/views_arquivar.py:68`, quando via é competência trancada, `app/chamados/views_arquivar.py:64`. | Não chama `motivo_do_veto`; herda guardas de `reabrir`. |
| `app/chamados/services/competencia_trancada.py::reabrir` | `ch.reabrir`, `app/chamados/services/competencia_trancada.py:258`; seleciona chamados encerrados pela tranca, `app/chamados/services/competencia_trancada.py:252`. | Não chama `motivo_do_veto`; herda guardas de `reabrir`. É importado por apelido `_reabrir_fila` em `app/ponto/services/fechamento.py:1415`. |
| `app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `ch.reabrir`, `app/chamados/services/acoes_disputa.py:266`. | Não chama `motivo_do_veto`; herda guardas de `reabrir`. O fio delega à função em `app/chamados/services/fio.py:577`. |
| `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | View chama `receber`, `app/chamados/views.py:1053`; serviço chama `_renascer_por_premissa_viva`, `app/chamados/services/resposta_tardia.py:45`. | Não chama `motivo_do_veto`; exige célula com `VEREDITOS_ACUSAM`, `app/chamados/services/resposta_tardia.py:44`, e herda renascimento/reabertura. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`. | Não chama `motivo_do_veto`; consulta previamente a própria `recusa_da_reabertura` e recusa ressuscitação de outras perguntas, `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`. |
| `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | Delegação, `app/chamados/views.py:547`; transição humana, `app/chamados/services/acoes_chamado.py:315`. | Não chama `motivo_do_veto`; sem veto equivalente de premissa para destino vivo, conforme `transicionar` acima. |
| `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | Delegações em `app/chamados/views.py:674` e `app/chamados/views.py:954`; transição humana em `app/chamados/services/fio.py:715`. | Não chama `motivo_do_veto`; mesma porta humana acima. |

Para evitar inclusão indevida no universo estrito: `api_abrir_chamado` grava módulo fixo `app_colaborador`, `app/api/views.py:2237`, `app/api/views.py:2246`; `api_justificativas` abre wrapper `justificativa_colab`, `app/api/views.py:2033`, `app/api/views.py:2036`; os aliases `_Ch9.criar` dos dois kernels gravam `batida_fora_escala_leve`, `app/api/views.py:746`, `app/api/views.py:754`, `app/api/views_core.py:746`, `app/api/views_core.py:754`. `reabrir_chamados_ausencia_aberta` seleciona somente `ausencia_colab`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:29`.

Os três pontos físicos distintos são `get_or_create`, `app/chamados/models.py:730`; `objects.create`, `app/chamados/models.py:834`; e `save` de estado, `app/chamados/models.py:231`. As reaberturas pela porta `reabrir` compartilham `_gravar_estado`, `app/chamados/models.py:1089`; as humanas também chegam a `_gravar_estado`, por `app/chamados/models.py:260`, mas não atravessam `reabrir` nem suas guardas.

## 30/10 07:44 · sessao 3931d03e0f52 · - · - · origem=Ronald (tmux)

A regra é estrita: as duas distâncias precisam existir e ser maiores que 180 minutos em valor absoluto; o retorno usa `AND` (`app/ponto/motor_calculo_v2.py:1869`, `app/ponto/motor_calculo_v2.py:1885`, `app/ponto/motor_calculo_v2.py:1886`, `app/ponto/motor_calculo_v2.py:1887`). `aplicar_tolerancia` calcula as distâncias nas linhas 1911–1914 e só acrescenta a acusação à lista quando esse predicado retorna verdadeiro (`app/ponto/motor_calculo_v2.py:1911`, `app/ponto/motor_calculo_v2.py:1914`, `app/ponto/motor_calculo_v2.py:1926`, `app/ponto/motor_calculo_v2.py:1930`).

A lista exportada é a própria `_cadastro_x_realidade` (`app/ponto/motor_calculo_v2.py:2657`). O leitor do dono só conserva itens com chave `dia` não nula (`app/ponto/management/commands/e6_oraculo.py:142`, `app/ponto/management/commands/e6_oraculo.py:143`).

Não existe conversão automática de uma saída órfã das 07:00 em entrada de um período 07→19: o pareador escolhe papel da ata ou tipo gravado, uma `E` abre turno e uma `S` sem turno corrente gera turno sem entrada (`app/ponto/turnos.py:1380`, `app/ponto/turnos.py:1400`, `app/ponto/turnos.py:1417`, `app/ponto/turnos.py:1493`, `app/ponto/turnos.py:1494`). O adaptador exclui esse turno sem entrada dos períodos e registra somente anomalia (`app/ponto/adaptador_motor.py:30`, `app/ponto/adaptador_motor.py:33`, `app/ponto/adaptador_motor.py:36`). Um período aberto do motor 12x36 retorna antes de julgar pontualidade (`app/ponto/motor_calculo_v2.py:2690`, `app/ponto/motor_calculo_v2.py:2693`).

Os pares cronológicos noite→manhã visíveis, comparados aritmeticamente com 19→07, têm estas distâncias:

| Par visível | Entrada vs.19 | Saída vs.07 |
|---|---:|---:|
| 20/10 18:53:20 →21/10 06:55:11 | 6m40 | 4m49 |
| 22/10 19:01:30 →23/10 06:52:47 | 1m30 | 7m13 |
| 24/10 19:11:58 →25/10 06:58:07 | 11m58 | 1m53 |
| 26/10 19:03:42 →27/10 06:46:01 | 3m42 | 13m59 |

Nenhum satisfaz `AND >180`; porém os anexos não mostram que esses foram os pares efetivamente usados nem os marcos efetivamente lidos para os dias pares de entrada: **[não verificado]**. Os tipos das batidas app, os turnos/pares do motor e os papéis da ata estão ausentes no dado fornecido: **[não verificado]**.

Consequentemente, é possível explicar com prova a regra e a exclusão de uma `S` órfã, mas atribuir a ausência de cada dia na lista a um ramo específico do motor continua **[não verificado]**. O motor pode receber papéis da ata (`app/ponto/motor_calculo_v2.py:444`, `app/ponto/motor_calculo_v2.py:450`, `app/ponto/motor_calculo_v2.py:453`), e deixa o pareamento geométrico quando a ata está aberta/desalinhada (`app/ponto/motor_calculo_v2.py:582`, `app/ponto/motor_calculo_v2.py:585`); esses insumos não estão na sonda.

## 30/10 07:44 · sessao f2dc35aafb84 · - · - · origem=Ronald (tmux)

| Caminho | Ponto de nascimento/reabertura | `motivo_do_veto` antes? | Outros vetos anteriores |
|---|---|---|---|
| `app/ponto/management/commands/detectar_ausencias.py::Command.handle → _abrir_chamado` | `handle:439` → `_abrir_chamado:688` chama `ChamadoColaborador.abrir` com `MODULO_ORIGEM` | **Não** no caminho próprio do detector. `_abrir_chamado` consulta diretamente as funções de régua em `:600` e `:604`, sem chamar `motivo_do_veto`. | Régua degenerada `:600`, régua suspeita `:604`, feriado `:616`, posto fechado `:623`, `celula_veta_cobranca:626`, existência de célula `:654`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py::Command.handle` | alias `_Emissor` importado em `:38`, instanciado `:48`, `emissor._abrir_chamado:142` → detector `:688` | **Não** na cadeia do command/emissor. | Classificação do dia `:99`, exigência `FURO_SEM_COBRANCA:109`, dia de trabalho `:127`, além dos vetos do detector acima. |
| `app/ponto/management/commands/reconciliar_grade.py::Command.handle → _emitir_furo` | `handle:254` → `_emitir_furo:499` chama `abrir`; eventual `ch.reabrir:520` | **Não**. | `_emitir_furo`: régua degenerada `:487`, suspeita `:491`, célula `:494`. `handle`: cobertura `:216` → `_coberto:393`; isenção `:230`; classificação da falta `:243`. |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo`, furo integral/cobrança morta/limbo de hoje | `emissor._abrir_chamado:1358` → detector `:688` | **Sim:** `_julgar_colab_corpo` chama `_veto_do_dia` ao calcular a impressão `:1083`; `_veto_do_dia:1013` chama alias `_motivo_veto`, importado de `furos_vetados.motivo_do_veto:1007`. | O retorno do veto entra na impressão `:1083`, **não é a condição que barra a emissão**. Bloqueio efetivo de régua ocorre no detector delegado `:600/:604`. Portão do cartório também exige código cobravel, pessoa não desligada e ausência de vivo `:1339/:1340`, afasta pessoa afastada `:1344`, dia trabalho `:1348`. |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo`, furo parcial | `emissor._abrir_chamado:1394` → detector `:688` | **Sim**, pela mesma cadeia `:1083 → :1013`. | Portão parcial `:1381`, afastado `:1386`, sem chamados vivos `:1393`, depois vetos do detector. |
| `app/chamados/reconciliador.py::reemitir_chamado_do_dia` | `ChamadoColaborador.abrir:862` | **Sim para nascer/renascer:** `if not _costura_no_vivo:852`, alias importado `:853`, `_mv_c2(...):854`, retorno de veto `:855/:857`. Na costura de chamado já vivo essa consulta é pulada `:808/:852`. | Situação ativo `:789`, horizonte `:809`, isenção `:819`, cobertura `:821`, protesto cobravel `:836`, célula `:838`, motor `:858`, cerca de intervalo `:860`. |
| `app/ponto/management/commands/processar_alertas_turno.py::Command.handle → _marco_critico → _abrir_chamado_critico` | `handle:141` → `_marco_critico:316` → `_abrir_chamado_critico:335`, módulo `turno_aberto:337` | **Não**. | Isenção `:85`, ausência `:97`, permissão de hora extra `:135`; veto compartilhado da porta `abrir` identificado pelo agente principal. |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `ChamadoColaborador.criar:121`, módulo `turno_aberto_24h:123` | **Não**. | Isenção `:32`, dia trabalho `:49`, turno aberto `:63/:64`, ausência `:116`, veto compartilhado de `criar`. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `ChamadoColaborador.abrir:146`, módulo `turno_aberto_24h:147` | **Não**. | Turno aberto `:92`, tempo mínimo `:101`, vínculo `:105`, ausência `:108`, dedup vivo `:118`, veto compartilhado de `abrir`; no dry run consulta `celula_absolveria:134`. |

A tupla estrita de módulos é `('batida_ausente', 'turno_aberto', 'intervalo_pendente', 'entrada_bloqueada_turno_aberto')` em `app/chamados/catalogo/furo_modulos.py:15`; `turno_aberto_24h` aparece nos dois emissores acima como módulo afim em `processar_alertas_avancados.py:123` e `emitir_turno_aberto_retroativo.py:147`.

A equivalência dos vetos de régua pode ser demonstrada pelo corpo: `furos_vetados.motivo_do_veto` consulta `regua_degenerada:28` e `vetar_cobranca_se_regua_suspeita:30` em `app/ponto/services/furos_vetados.py`; o emissor consulta `vetar_cobranca_se_degenerada`, cujo predicado é `regua_degenerada:46` em `app/escala/regua_defesa.py`, e a mesma função de suspeita `detectar_ausencias.py:604`. Há diferença para `ec=None`: `motivo_do_veto` retorna `None` em `furos_vetados.py:26`, enquanto `vetar_cobranca_se_degenerada` transforma `ec=None` em `te=None:45` e `regua_degenerada(None)` retorna `True:23` em `escala/regua_defesa.py`.

`celula_veta_cobranca` veta `trabalha=False` em `app/ponto/marcos.py:159` e vereditos pertencentes a `VEREDITOS_QUE_VETAM` em `:161`; a tupla é definida em `:119`. Célula ausente retorna `None:156/:157`.

Entradas superiores do mesmo caminho do cartório:

- `app/ponto/management/commands/processar_cartorio.py::Command.handle:65 → cartorio.julgar_colab:744 → _julgar_colab_corpo`.
- `app/ponto/management/commands/celula_veredito_velho.py::Command.handle:144 → julgar_colab`.
- `app/ponto/management/commands/rejulgar_ausencia_em_folga.py::Command.handle:71 → julgar_celula`.
- `app/ponto/management/commands/curar_residuo_bug115.py::_curar:143 → julgar_celula` — nome da função que contém a linha `:143` não conferido novamente; `[não verificado]` para `_curar`.
- `app/ponto/services/cartorio.py::julgar_celula:1560 → julgar_colab`, usando emissor `_Emissor()` em `:1561`, importado de `detectar_ausencias:1535`.
- `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/portas/celula.py:372`, `app/chamados/services/cobrar_dia.py:29`, `app/colaboradores/services/pos_vinculo.py:72`, `app/chamados/signals.py:180`, `app/ponto/signals.py:93` chamam `julgar_celula`.
- `app/escala/regua_defesa.py:692` chama `julgar_colab`.
- `app/chamados/reconciliador.py::_ctx_c2:671` pode chamar `julgar_celula` ao produzir o contexto de nascimento passado em `reemitir_chamado_do_dia:872`.

Entradas superiores do c2: `reconciliar_chamados_colab:1348 → reconciliar_perguntas_orfas_colab:1423 → reatar_pergunta_orfa:1518 → reemitir_chamado_do_dia`, todas em `app/chamados/reconciliador.py`.

Exclusões verificadas: `detectar_ausencias` abre cadastro `sem_previsao_escala:190/:192`, `ancora_limbo:271/:273` e reabre esse cadastro `:301`; alias `_ChG.abrir:510` é `_chamado_dia_sem_grade`, fora de cobrança de furo. `reabrir_chamados_ausencia_aberta.py:49` atua somente `ausencia_colab`, filtrado em `:29`. `smoke_chamados.py:123` cria somente módulo `smoke:126`. `reabrir_orfas_enterradas.py:161` reabre `p.disputa.chamado`, lido em `:96/:157`, sem filtro de módulo; não é a reabertura de `p.chamado_destino`.

## 30/10 07:43 · sessao f1bdcad8701f · - · - · origem=Ronald (tmux)

A fábrica atual escolhe **marco apagado**, não uma batida órfã identificada por B# ou instante:

- `app/chamados/services/fabrica_da_celula.py:48-64`: chama `_ata_do_dia`, percorre os marcos faltantes e converte posição/tipo em motivo.
- `app/chamados/services/declaracao_texto.py:42-46`: `('fim', 'S') → 'orfao_14h'`.
- `app/chamados/services/declaracao_texto.py:97-120`: percorre os slots da ata; aceita `missing`, identifica o último como `fim`, devolve `(dia, posição, tipo, hora)`. Não consulta `orfas`.
- `app/ponto/services/lastro.py:128-134`: `marcos_apagados` também só percorre `ata['lampadas']`, pula acesa/pendente e retorna hora/tipo; não seleciona uma batida física.
- `app/ponto/services/cartorio.py:1463-1467`: o chamado parcial nomeia esses marcos apagados por `marco_para_motivo`.

O texto canônico é **“Confirme o horário da saída no dia {data}”** (`app/chamados/catalogo/perguntas.py:48-50`). A data vem de `contexto[motivo]['data']`, interpolada em `app/chamados/catalogo/query.py:58-64`. No caminho da célula, o contexto recebe o próprio dia da célula (`app/chamados/services/fabrica_da_celula.py:122`); o gerador grava `pergunta=texto` e a `data_evento` derivada em `app/chamados/services/disputa_emissao.py:389-396`. No complemento pela ata, a seleção `dia == data_evento` e o texto canônico estão em `app/chamados/services/disputa_emissao.py:806-813`.

Há caminho alternativo por episódio: `_achar_batida_origem` escolhe a entrada do turno aberto mais recente até `criado_em`, com janela de dois dias (`app/chamados/services/materializacao.py:95-125`). O agrupador usa essa entrada e resolve a data da saída esperada (`app/chamados/services/disputa_emissao.py:997-1007`; `app/escala/servico_jornada.py:487-490`). Para `batida_ausente` passado com marcos nomeados há caminho específico pela ata (`app/chamados/services/disputa_emissao.py:658-680`, `966-979`).

O anexo não fornece o texto gravado, `data_evento`, ata original nem batida de origem das P#46704/49423/50180/51025. Portanto, afirmar que cada pergunta concreta se referia a B#132829/B#134922/B#136732/B#138367 — ou indicar seu instante como alvo textual da pergunta — é **[não verificado]**. A regra de fábrica localizada pergunta pela saída faltante do turno; sua vinculação histórica a determinada órfã não está demonstrada. (`app/chamados/services/declaracao_texto.py:97-120`; `app/chamados/catalogo/perguntas.py:48-50`.)

## 30/10 07:43 · sessao f0c61612ed1a · - · - · origem=Ronald (tmux)

Escritor e derivação comprováveis na cópia:

- `_materializar_resposta_hora` calcula `(ts,tipo)` chamando `_ts_tipo_do_plantio`: `app/chamados/services/materializacao.py:786`. Se não reutilizou batida existente, chama `registrar_batida` com `origem='disputa_s84_retro'`: `app/chamados/services/materializacao.py:871-879`. O `INSERT` normal ocorre em `app/ponto/registro_batida.py:157-159`.
- A ligação persistida B→P é `pergunta_origem`, anotada em `app/chamados/services/materializacao.py:891-895`. O anexo não exibe esse campo: vínculo específico B#140649→P#46704, B#140652→P#49423, B#140653→P#50180 e B#140654→P#51025 é **[não verificado]**, embora datas e horários sejam compatíveis com o algoritmo abaixo. A pergunta criadora de B#140648 também é **[não verificado]**. B#140650 e B#140651 não aparecem no dado **[não verificado]**.
- Data-base: usa `p.data_evento`; só na ausência usa `disputa.contexto[p.motivo]['data']`: `app/chamados/services/materializacao.py:328-334`.
- Hora: usa `p.resposta_colab` em `app/chamados/services/materializacao.py:391`, faz `combine(data_evento, time(h,m))` em `:397-398` e chama ajuste de data em `:404`.
- Para DNA 19→07, `marcos_turno` encadeia E19 em D e S07 em D+1; a linha que avança a data é `app/escala/servico_jornada.py:82-83`. Lê os marcos de `ec.marcos_do_dia` em `:68-69`.
- O ajuste calcula envelope `[primeiro marco−90min, último marco+90min]`: `app/chamados/juizes.py:305-306`. **A escolha do dia está em `:308-311`: testa D e D+1 e retorna o primeiro candidato dentro do envelope.** Assim, com data-base D e DNA dado, D07 fica fora e D+1 07 fica dentro. Resultado: D21→22/10 07; D23→24/10 07; D25→26/10 07; D27→28/10 07. Os `data_evento` históricos das perguntas não foram fornecidos **[não verificado]**.

Semântica escrita e divergência:

- O catálogo diz literalmente **“Confirme o horário da saída no dia {data}”**, `app/chamados/catalogo/perguntas.py:48-50`.
- O dia mostrado vem de `contexto[motivo]['data']`, interpolado em `app/chamados/catalogo/query.py:58-64`.
- No caminho da fábrica da célula, esse contexto recebe `dia.isoformat()`: `app/chamados/services/fabrica_da_celula.py:122`; o gerador calcula o texto em `app/chamados/services/disputa_emissao.py:316-317` e grava texto/data em `:389-397`.
- Portanto, **se o texto mostrado contém D, “07:00” responde saída D07, enquanto o ajuste pode plantar D+1 07**; a mudança ocorre em `app/chamados/juizes.py:308-311`. O texto e a data efetivamente armazenados nas P dos anexos são **[não verificado]**.
- A regra alternativa escrita em `resolver_data_evento` declara que `orfao_14h` usa a data da saída esperada a partir da entrada-origem: `app/escala/servico_jornada.py:482-490`. Esse helper não é chamado pelo caminho direto `fabrica_da_celula.perguntar` mostrado em `app/chamados/services/fabrica_da_celula.py:112-125`.

## 30/10 07:39 · sessao af28e22338ae · O261-N · sonda · origem=Code

**1. Autoria e horários**

- **Quem criou P#51789–51792 e quando: [nao verificado].** A sonda informa abertura da **disputa** em 28/10 06:34:43 e julgamento das perguntas em 30/10 06:35:14; não informa emissor nem nascimento das perguntas [nao verificado]. `aberta_em` pertence à disputa (`app/chamados/models.py:1331`), uma disputa existente pode receber perguntas novas (`app/chamados/services/disputa_emissao.py:618`) e `julgado_em` recebe o instante da lavra do veredito (`app/chamados/veredito_lavrado.py:228`).
- O escritor de nascimento é `PerguntaDisputa.abrir`, chamado por `gerar_perguntas_disputa`; a criação efetiva ocorre em `app/chamados/models.py:2085`, acionada em `app/chamados/services/disputa_emissao.py:389`. Isso identifica a porta do código, **não o emissor efetivo dessas quatro perguntas [nao verificado]**.
- **#35679:** a transcrição registra criação em **29/10 09:00:12**, resolução em **30/10 06:35:14**, via **`sistema`**, `respondido_por=None` e `superado_por=None` [nao verificado — dados da transcrição, sem arquivo numerado]. No código, `reconciliar()` passa `EVENTO_VIA['RECONCILIAR']` ao núcleo (`app/chamados/models.py:444`), cujo valor é `sistema` (`app/chamados/catalogo/motor.py:95`). **O escritor de `via_encerramento` é `app/chamados/models.py:209`**, persistido em `:231`; `resolvido_em` é carimbado em `:208`.
- **Qual serviço/cron chamou esse encerramento e por qual motivo concreto: [nao verificado].** A via identifica o caminho de reconciliação, mas a transcrição não contém a chamada nem sua trilha. O censo seleciona chamados vivos (`app/chamados/services/vigia_fio.py:186`); atribuir a saída da célula a uma rotina específica continua [nao verificado].

**2. Emissão para `nunca_bateu`**

**O emissor efetivo e até o `modulo_origem` de #35679 não aparecem na sonda [nao verificado].** Para o módulo `batida_ausente`, o código contém estes caminhos:

| Caminho | Escritor/delegação e passagem pelo veto |
|---|---|
| `detectar_ausencias::_abrir_chamado` | Abre em `app/ponto/management/commands/detectar_ausencias.py:688`; consulta o veto em `:626` e retorna em `:628`. |
| Cartório | Delega ao funil acima em `app/ponto/services/cartorio.py:1358` e `:1394`; sem adesão, retorna antes, em `:1003`. |
| `emitir_furo_retroativo` | Delega ao mesmo funil em `app/ponto/management/commands/emitir_furo_retroativo.py:142`; passa pelo veto desse funil. |
| `vigia_de_hora` | Delega ao cartório por `julgar_celula`, em `app/ponto/services/vigia_de_hora.py:130`. |
| `reconciliar_grade::_emitir_furo` | Abre em `app/ponto/management/commands/reconciliar_grade.py:499`; consulta o veto em `:494` e retorna em `:496`. |
| Reconciliador c2, `reemitir_chamado_do_dia` | Abre em `app/chamados/reconciliador.py:862`; consulta o veto em `:838` e retorna em `:842`. |
| Abertura por formulário/request | Aceita módulo catalogado em `app/chamados/views.py:170`, `:174`; delega em `:197`; cria com o módulo recebido em `app/chamados/services/abertura.py:47`, `:56`. **Não passa pelos dois vetos citados** (`app/chamados/services/abertura.py:19`; `app/chamados/models.py:805`). |

`nunca_bateu` está literalmente em `VEREDITOS_QUE_VETAM` (`app/ponto/marcos.py:119`), aplicado pela leitura da célula em `:155` e pelo retorno em `:161`. Portanto, **os caminhos automáticos acima não podem emitir se essa consulta encontra `nunca_bateu`**; o cartório também interrompe seu corpo quando `adesao is None` (`app/ponto/services/cartorio.py:907`, `:1003`).

A request passa por outro gate de absolvição (`app/chamados/models.py:821`, `:839`), que não equivale àquele veto: ele não absolve vereditos pertencentes a `VEREDITOS_ACUSAM` (`app/chamados/reconciliador.py:975`), conjunto que inclui `nunca_bateu` (`app/escala/models.py:1735`). Isso não prova emissão da request para a célula 143031 [nao verificado].

**3. Efeito real de faltar `out['cods']`**

O ramo sem adesão lavra/carimba e retorna sem registrar a data (`app/ponto/services/cartorio.py:981`, `:999`, `:1003`). A gravação do ramo normal usa **somente a data** como chave, inclusive quando `cods` é lista vazia (`:1120`); a fábrica é chamada pelo teste `_cel.data in _julgadas` (`:759`, `:769`, `:770`).

| Chamador | Consequência |
|---|---|
| Lote `processar_cartorio` | **Não impede sempre.** Cria um único `out` antes do laço e o reutiliza entre colaboradores (`app/ponto/management/commands/processar_cartorio.py:43`, `:59`, `:65`). Se outro colaborador anterior já registrou a data, a célula sem adesão alcança `perguntar_pela_celula`; sem essa entrada, não alcança (`app/ponto/services/cartorio.py:769`, `:1120`). |
| `julgar_celula` | Cria `out` novo para uma célula (`app/ponto/services/cartorio.py:1559`, `:1560`). Nesse ramo, não surge entrada em `cods`, e a fábrica não é chamada (`:1003`, `:759`, `:769`). |
| `rejulgar_furos_do_vinculo` | Cria `out` novo por invocação; se alcançar o ramo sem adesão, ocorre o mesmo bloqueio (`app/escala/regua_defesa.py:691`, `:692`; `app/ponto/services/cartorio.py:1003`, `:769`). |
| `celula_veredito_velho` | Passa `contadores()` novo por colaborador; mesmo bloqueio nesse ramo (`app/ponto/management/commands/celula_veredito_velho.py:139`, `:144`, `:145`). |
| Script `rejulgar_o118_col221` | Usa `out` novo para um colaborador; mesmo efeito se alcançar esse ramo com aplicação habilitada (`bin/rejulgar_o118_col221.py:20`, `:23`, `:25`). |

Alcançar a fábrica não garante pergunta nova: ela ainda verifica trabalho previsto, situação, data, vencimentos e deduplicação (`app/chamados/services/fabrica_da_celula.py:78`, `:80`, `:85`, `:106`, `:109`). **Não é possível atribuir as perguntas existentes da célula 143031 à colisão de datas do lote [nao verificado].**

Os outros usos são:

- **Cluster:** o wrapper testa apenas se `_julgadas` está não vazio e chama `_cluster(c, hoje)`; não entrega os códigos ao serviço (`app/ponto/services/cartorio.py:763`, `:767`). Assim, entradas de colaborador anterior também satisfazem essa condição; o cluster lê a ATA do colaborador atual (`app/ponto/services/cluster_espurio.py:22`, `:23`).
- **Releitura:** o gatilho é o aumento de `emitidos`, não `cods` (`app/ponto/services/cartorio.py:743`, `:798`). Cria acumulador novo, executa o corpo e mescla seus códigos ao acumulado (`:810`, `:811`, `:812`), sem nova fábrica depois (`:823`). O ramo sem adesão retorna antes das emissões (`:1003`, `:1358`, `:1394`).

**4. FROTA**

- **Células `nunca_bateu` sem pergunta nenhuma: 0**, conforme `nb_sem_pergunta_nenhuma=0` [nao verificado — transcrição fornecida].
- **Chamados vivos em células `nunca_bateu`: 12**, nos módulos `ausencia_colab`, `batida_ausente`, `disputa_supervisao` e `sem_previsao_escala` [nao verificado — transcrição fornecida].
- **Quantidade de células distintas com chamado vivo: [nao verificado].** O campo fornecido conta chamados; não informa a contagem distinta de células [nao verificado].

## 30/10 07:39 · sessao 74c8c0ee326f · O261-V · sonda · origem=Code

**1. Célula 129745**

- **P46703, resposta 19:00:** materialização anterior por 666 e validação posterior por 657 são a assinatura compatível com `_carimbar_validacao`, que altera somente `validada_em/por`. Os outros escritores positivos também gravam materialização no ato. A execução histórica exclusiva permanece **[não verificado]**. (`app/chamados/services/validacao.py:127-133`; `app/chamados/services/materializacao.py:506-530`, `1148-1198`; `app/chamados/services/disputa_emissao.py:1561-1577`)
- **Lastro de P46703:** a lista fornecida não mostra batida em **21/10 19:00**; `materializada_em` sozinho não demonstra plantio. Ausência absoluta de lastro: **[não verificado]**, pois faltam os vínculos das batidas com a pergunta, usados pelo juiz. (`app/chamados/juizes.py:674-681`, `730-732`)
- **P46704, resposta 07:00:** existe correspondência horária em **B140649, 22/10 07:00, não retratada**, segundo a sonda **[não verificado — sem confirmação independente]**. Escritor exclusivo da validação: **[não verificado]**; validação seguida de materialização e carimbo de simetria produzem estados compatíveis. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH33286 cobra E19:00:** o lastro horário de S07:00 não satisfaz essa cobrança; o fechador exige os marcos cobrados acesos. Dono da contradição **fio morto: ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; `app/ponto/management/commands/e6_oraculo.py:90-100`; marco cobrado: **[não verificado — sonda]**)

**2. Célula 129747**

- **P49422, resposta 19:00:** mesma assinatura de validação posterior à materialização, compatível com `_carimbar_validacao`; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Lastro de P49422:** nenhuma batida em **23/10 19:00** aparece na lista fornecida. Plantio vinculado ou ausência absoluta de lastro: **[não verificado]**. (`app/chamados/juizes.py:674-681`, `730-732`; conteúdo da lista: **[não verificado — sonda]**)
- **P49423, resposta 07:00:** correspondência horária em **B140652, 24/10 07:00, não retratada** **[não verificado — sonda]**. Escritor exclusivo da validação **[não verificado]**, pelos mesmos caminhos concorrentes de validação e simetria. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH34254 cobra E19:00:** S07:00 não comprova atendimento desse marco. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; marco cobrado: **[não verificado — sonda]**)

**3. Célula 129749**

- **P50179, resposta 19:00:** assinatura compatível com `_carimbar_validacao` sobre materialização anterior; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Lastro de P50179:** nenhuma batida em **25/10 19:00** aparece na lista fornecida. Plantio vinculado ou ausência absoluta de lastro: **[não verificado]**. (`app/chamados/juizes.py:674-681`, `730-732`; conteúdo da lista: **[não verificado — sonda]**)
- **P50180, resposta 07:00:** correspondência horária em **B140653, 26/10 07:00, não retratada** **[não verificado — sonda]**. Escritor exclusivo da validação **[não verificado]**: a igualdade mostrada até segundos não distingue validação seguida de materialização dos carimbos de simetria. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH34850 cobra E19:00:** S07:00 não comprova atendimento desse marco. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; marco cobrado: **[não verificado — sonda]**)

**4. Célula 129751**

- **P50883, resposta 19:00:** assinatura compatível com `_carimbar_validacao` sobre materialização anterior; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Lastro de P50883:** nenhuma batida em **27/10 19:00** aparece na lista fornecida. Plantio vinculado ou ausência absoluta de lastro: **[não verificado]**. (`app/chamados/juizes.py:674-681`, `730-732`; conteúdo da lista: **[não verificado — sonda]**)
- **P51025, resposta 07:00:** correspondência horária em **B140654, 28/10 07:00, não retratada** **[não verificado — sonda]**. Escritor exclusivo da validação **[não verificado]**, pelos caminhos distintos já identificados. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH35331 cobra E19:00:** S07:00 não comprova atendimento desse marco. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; marco cobrado: **[não verificado — sonda]**)

**5. Célula 140114**

- **P50169, resposta 17:30:** assinatura compatível com `_carimbar_validacao` sobre materialização anterior; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Tem lastro horário no dado:** **B137026, 25/10 17:30:12, não retratada** **[não verificado — sonda]**. Isso não comprova atendimento de **E21:30**, cobrada por CH35101, nem o vínculo com a pergunta, ausente na sonda. A cobrança é julgada pelo próprio marco. (`app/ponto/services/lastro.py:170-181`; `app/chamados/juizes.py:674-681`; marco cobrado: **[não verificado — sonda]**)
- **P50170:** não há validação gravada no estado apresentado **[não verificado — sonda]**. A cláusula exata que produziu `viva=False` é **[não verificado]**: `fantasma` não pertence às vias que encerram por identidade. Antes do retorno obrigatório de pergunta viva para célula acusando, os encerramentos compatíveis com os campos mostrados são **tranca por leitura**, em `1010-1011`, ou **marco próprio aceso na ATA**, em `1050-1052`. (`app/chamados/juizes.py:1005-1057`)
- Para `orfao_14h`, o marco próprio é **S/hf**; sua prova lê `cel.ata.lampadas[].acesa`. A sonda apresenta `dna.lampadas`, sem essa ATA e sem a tranca: não permite escolher a cláusula. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/chamados/juizes.py:823-828`, `866-872`; `app/chamados/services/vigia_fio.py:9-14`)

**Classes e pontos de decisão**

**Validação sobre materialização anterior.** O caminho concreto da cópia carimba a validação em `validacao.py:389`, mas só chama a unidade de materialização se `materializada_em` estiver nulo, em `415-417`. O lote posterior também exige materialização nula, e a devolução da validação não atua quando já existe materialização. Esse caminho permite conservar validação sem reapurar a unidade; identificar sua execução histórica em cada pergunta é **[não verificado]**. (`app/chamados/services/validacao.py:182-183`, `389`, `415-417`; `app/chamados/services/materializacao.py:1286-1290`)

**Pergunta morta não equivale a cobrança satisfeita.** `pergunta_viva` retorna `False` imediatamente com `validada_em` preenchido. Já `lastro` mantém `furo_real` quando a lâmpada cobrada está apagada e só encaminha `com_lastro` ao `reconciliar()`. Portanto, as respostas 07:00/17:30 com correspondência horária não demonstram falha do fechador das entradas 19:00/21:30. (`app/chamados/juizes.py:1006-1007`; `app/ponto/services/lastro.py:265-272`, `306-317`)

**Rejulgamento e fechamento.** O evento de batida aciona o cartório; o cartório monta a ATA atual e tenta reconciliar quando todos os marcos da cobrança acenderam. O reconciliador por absolvição da célula retorna sem fechar quando ela ainda acusa. Qual desses caminhos efetivamente rodou, recusou ou falhou nos atos apresentados — a solicitada “linha em que não fez” — é **[não verificado]**. (`app/ponto/signals.py:108-135`; `app/ponto/services/cartorio.py:1148-1163`; `app/chamados/reconciliador.py:975-976`)

**Dono L-099.** O fio morto é declarado contradição estrutural; isso não autoriza classificar automaticamente a causa do furo. O juiz exige flags de cadastro, batida fora de turno, paridade das batidas **atribuídas ao dia** e pertencimento à lista `dias_cadastro_x_realidade`. Esses resultados não constam da sonda; contar as cinco/seis batidas da janela vizinha não reproduz esse julgamento. Assim, atribuir definitivamente os furos a CADASTRO ou BATIDA seria **[não verificado]**. (`CLAUDE.md:306-316`; `app/ponto/management/commands/e6_oraculo.py:90-100`; `app/chamados/services/vigia_fio.py:99-101`)

## 30/10 07:39 · sessao 56120f37885d · - · - · origem=Ronald (tmux)

Inventário de emissores de `batida_ausente`:

| Caminho | Escritor | Veto `nunca_bateu` |
|---|---|---|
| `detectar_ausencias::_abrir_chamado` | `app/ponto/management/commands/detectar_ausencias.py:688` (módulo definido em `:29`) | Chama `celula_veta_cobranca` e retorna em `:625-628`. |
| Cartório | Delega ao escritor acima em `app/ponto/services/cartorio.py:1358` e `:1394`. | Além do veto do escritor, `adesao is None` retorna em `:1003`, antes dessas emissões; consulta de adesão em `:899-907`. |
| `emitir_furo_retroativo` | Delega ao escritor acima em `app/ponto/management/commands/emitir_furo_retroativo.py:142`. | Passa pelo mesmo veto; também pula colaborador sem batidas na janela em `:52-55`. |
| `reconciliar_grade::_emitir_furo` | `app/ponto/management/commands/reconciliar_grade.py:499` (CANON em `:54`) | Chama o veto e retorna em `:494-496`. |
| Reconciliador c2 `reemitir_chamado_do_dia` | `app/chamados/reconciliador.py:862` (chave do módulo em `:804`) | Chama o veto e retorna em `:838-842`. |
| Abertura pela request | `app/chamados/views.py:170-175` aceita o módulo do catálogo; delega em `:196-207`; escritor `app/chamados/services/abertura.py:47-65`. | Não passa pelo veto `celula_veta_cobranca` nem pelo ramo de adesão; segue `ChamadoColaborador.criar`, em `app/chamados/models.py:805-834`. |

`vigia_de_hora` chega ao cartório por `julgar_celula` em `app/ponto/services/vigia_de_hora.py:130`.

O veto compartilhado contém literalmente `nunca_bateu` em `app/ponto/marcos.py:119` e o aplica em `:155-162`.

A abertura pela request passa por outro gate, `_celula_que_absolve` (`app/chamados/models.py:821-825`, implementação em `:839-851`). Esse gate não equivale a `VEREDITOS_QUE_VETAM`: `_absolvido_pela_celula` retorna `None` quando o veredito pertence a `VEREDITOS_ACUSAM` (`app/chamados/reconciliador.py:975`).

As chaves de contexto de #35679 coincidem com o formato produzido pelo funil `detectar_ausencias`: base em `app/ponto/management/commands/detectar_ausencias.py:700-706`; `folga_nao_declarada` e `nao_calado_porque` em `:753-756`; `agrupado_em` possui escritor em `app/chamados/services/limbo_folgas.py:219`. Porém a sonda não traz `modulo_origem`, autor, trilha nem emissor: atribuir exatamente #35679 a uma execução desse funil continua **[nao verificado]**.

## 30/10 07:38 · sessao ae6aaf07e1c7 · - · - · origem=Ronald (tmux)

Leitura somente; nenhum arquivo alterado. [não verificado]

- **Lastro da resposta não equivale a lastro da cobrança.** Os quatro chamados de col451 cobram **E 19:00**; as batidas retro **07:00 do dia seguinte** sustentam temporalmente as respostas P46704/P49423/P50180/P51025, mas não demonstram atendimento da entrada cobrada. A autoridade do fechamento exige todos os marcos nomeados acesos, por hora **e tipo**; lâmpada apagada mantém `furo_real`. (`app/ponto/services/lastro.py:170`, `app/ponto/services/lastro.py:175`, `app/ponto/services/lastro.py:179`, `app/ponto/services/lastro.py:265`, `app/ponto/services/lastro.py:271`; correspondência às batidas da sonda: [não verificado])

- **140114:** B137026, de 25/10 17:30:12 e não retratada, sustenta temporalmente a resposta `17:30` de P50169; o chamado CH35101 cobra **E 21:30**, portanto aquela presença não prova regularização do marco cobrado. (`app/ponto/services/lastro.py:175`, `app/ponto/services/lastro.py:179`; presença e estado da batida, extraídos da sonda sem arquivo:linha: [não verificado])

- **Limite da prova técnica de materialização:** a sonda não informa `tipo_resposta`, `data_evento`, `tipo` das batidas, `pergunta_origem` nem `ata`. O juiz usa esses campos: para hora/data_hora procura plantio vinculado e vivo; aceita plantio em data_evento −1/0/+1 com hora dentro de ±20 minutos; na prova externa exige tipo e instante do slot. Logo não é rigoroso afirmar ausência absoluta de lastro dos P19 apenas porque inexiste B19 no próprio dia — há B19 vizinhas, mas o vínculo necessário não foi mostrado. (`app/chamados/juizes.py:225`, `app/chamados/juizes.py:234`, `app/chamados/juizes.py:257`, `app/chamados/juizes.py:263`, `app/chamados/juizes.py:266`, `app/chamados/juizes.py:674`, `app/chamados/juizes.py:676`, `app/chamados/juizes.py:680`, `app/chamados/juizes.py:730`)

- **Quem rejulga/fecha:** `post_save(Batida)` chama cartório para dia civil da marca e véspera; cartório monta a ata atual e só tenta reconciliar a cobrança por marco quando `_acesos(...) is True`. A rede `lastro.fechar` só chama `reconciliar()` para `com_lastro`. A materialização tenta fechar destino/clones, mas passa pelo mesmo `reconciliar()` e pelo bloqueio de irmã sem consumo. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:132`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1148`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/lastro.py:306`, `app/ponto/services/lastro.py:317`, `app/chamados/services/materializacao.py:702`, `app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:919`)

- **Por que a cópia permite chamado vivo com perguntas validadas:** fechar a pergunta não é a condição de fechamento do chamado. `reconciliar()` consulta a premissa; o gate recusa furo ainda vivo para a folha, salvo regularização/agrupamento ou deduplicação sob outra cobrança viva. A regularização pode decorrer do marco cobrado aceso; não decorre simplesmente de `validada_em`. (`app/chamados/models.py:440`, `app/chamados/models.py:470`, `app/chamados/models.py:495`, `app/chamados/models.py:498`, `app/chamados/models.py:525`, `app/chamados/models.py:957`, `app/chamados/models.py:959`)

- **Reconciliador não absolve estas células pelo veredito fornecido:** `_absolvido_pela_celula` retorna `None` quando o veredito pertence a `VEREDITOS_ACUSAM`. Ele não transforma uma validação individual em absolvição do dia. (`app/chamados/reconciliador.py:948`, `app/chamados/reconciliador.py:975`, `app/chamados/reconciliador.py:976`)

- **“Em que linha não fez” como execução histórica:** [não verificado]. Sem log, a sonda não mostra se houve signal, tentativa, exceção, decisão anterior ou recusa. É possível apontar as condições acima; não é possível afirmar qual linha foi efetivamente percorrida e recusou em cada ato. A própria porta do signal contém retorno por julgamento em curso e tratamento de exceção. (`app/ponto/signals.py:88`, `app/ponto/signals.py:90`, `app/ponto/signals.py:91`)

- **Dono formal L-099:** não pode ser determinado exatamente por esta sonda. O juiz recebe flags de cadastro, batida fora do turno, horas atribuídas pelo oráculo e pertencimento à lista `dias_cadastro_x_realidade`; faltam esses insumos. O código dá precedência: cadastro incompleto/sem vínculo → CADASTRO; batida fora do turno → BATIDA; horas em contagem ímpar → BATIDA; depois lista CxR → CADASTRO; resíduo → ESTRUTURA. DNA e protesto sozinhos não substituem esses insumos. (`CLAUDE.md:311`, `CLAUDE.md:313`, `app/ponto/management/commands/e6_oraculo.py:90`, `app/ponto/management/commands/e6_oraculo.py:96`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:100`, `app/ponto/management/commands/e6_oraculo.py:126`, `app/ponto/management/commands/e6_oraculo.py:142`, `app/ponto/management/commands/e6_oraculo.py:149`, `app/ponto/management/commands/e6_oraculo.py:155`)

- **Não converter classe de fio morto em dono:** o próprio vigia declara que essa classificação pertence ao juiz L-099, e não a um mapa de classes da porta. (`app/chamados/services/vigia_fio.py:99`, `app/chamados/services/vigia_fio.py:100`, `app/chamados/services/vigia_fio.py:101`)

## 30/10 07:38 · sessao 376360c5b485 · - · - · origem=Ronald (tmux)

Atribuição estrita:

- **Emissor e instante de criação das P#51789–51792: [nao verificado].** A sonda mostra abertura da disputa, não nascimento das perguntas: `DisputaSupervisao.aberta_em` é `auto_now_add` em `app/chamados/models.py:1331`. Uma disputa existente recebe novas perguntas em `app/chamados/services/disputa_emissao.py:618`. `julgado_em` é escrito como instante da lavra do cache em `app/chamados/veredito_lavrado.py:228`; portanto 30/10 06:35:14 não prova criação.
- **Escritor genérico de perguntas:** `gerar_perguntas_disputa` chama `PerguntaDisputa.abrir` em `app/chamados/services/disputa_emissao.py:389`; a criação efetiva ocorre em `app/chamados/models.py:2085`, e seu log `pergunta_emitida` em `app/chamados/models.py:2092`. O gate também reaponta perguntas já existentes e retorna sem criá-las em `app/chamados/models.py:2058` e `app/chamados/models.py:2084`; logo a disputa atualmente apresentada não identifica emissor original.
- **#35679:** a sonda declara criado 29/10 09:00:12 e resolvido 30/10 06:35:14, via `sistema`, sem respondente e sem superador — dados fornecidos sem arquivo:linha, portanto **[nao verificado] documentalmente**. O caminho correspondente no código é `ChamadoColaborador.reconciliar`, que chama `_gravar_estado(..., via=EVENTO_VIA['RECONCILIAR'])` em `app/chamados/models.py:444`; `RECONCILIAR → sistema` está em `app/chamados/catalogo/motor.py:95`. O escritor de `via_encerramento` é `app/chamados/models.py:209`, com persistência em `app/chamados/models.py:231`; `resolvido_em` é carimbado em `app/chamados/models.py:208`.
- **Qual serviço/cron chamou esse encerramento e sua causa concreta: [nao verificado].** Existem múltiplos chamadores, por exemplo `app/chamados/reconciliador.py:525` e `app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, `:1266`; horário e via não distinguem esses chamadores.
- O fechamento pode relavrar perguntas apontadas para o chamado: `app/chamados/models.py:243`–`:249`. Isso explica a existência de um caminho de código entre encerramento e `julgado_em`, mas não prova que tenha sido esse o caminho das quatro perguntas da sonda **[nao verificado]**.
- `contexto_keys` sozinho não prova módulo nem emissor de #35679 **[nao verificado]**. Não atribuir ao detector apenas porque contém `horario_previsto`, `atraso_min`, `marcos_faltantes` ou ressalvas de folga.

## 30/10 07:38 · sessao c98b4e89e71a · - · - · origem=Ronald (tmux)

Levantamento somente leitura dos escritores:

| Perguntas da sonda | Atribuição sustentada pela assinatura |
|---|---|
| P46703, P49422, P50179, P50883, P50169 | `_carimbar_validacao` é o escritor positivo que grava somente `validada_em/por`, preservando materialização anterior: `app/chamados/services/validacao.py:127-133`. Os outros três escritores positivos gravam também materialização com o mesmo instante/autor da validação, incompatível com a assinatura atual `mat` anterior/666 e validação posterior/657 em uma única dessas escritas: `materializacao.py:506-530`, `materializacao.py:1148-1198`, `disputa_emissao.py:1561-1583`. Execução histórica exclusiva, sem trilha: [não verificado]. |
| P46704, P49423, P50180, P51025 | Compatíveis com validação seguida de materialização: `validacao.py:389`, `validacao.py:415-417`, `materializacao.py:1148-1198`. A sonda mostra segundos, portanto igualdade dos microssegundos [não verificado]. Também não exclui só pelo estado o carimbo de simetria de `_materializar_ausencia_de_resposta`, que permite `via=declaracao` quando a irmã declarou hora: `materializacao.py:506-530`. Autor histórico exclusivo de `validada_em`: [não verificado]. |
| P50170 | `validada_em/por` são nulos na sonda: ninguém identificado como escritor de validação; eventual validação passada posteriormente apagada [não verificado]. |

O caminho concreto que permite validar uma pergunta já materializada sem reapurar o lastro é:

- A validação carimba antes da tentativa de materializar: `app/chamados/services/validacao.py:388-389`.
- A unidade só roda se `materializada_em is None`: `app/chamados/services/validacao.py:415-417`.
- A devolução recusa atuar se materialização já existe: `app/chamados/services/validacao.py:180-185`.
- O lote posterior também só seleciona materialização nula: `app/chamados/services/materializacao.py:1286-1290`.

Portanto, o carimbo antigo `fantasma` explica pelo código a preservação da validação sem nova verificação da unidade; afirmar que isso foi precisamente o caminho histórico de cada registro exige trilha ausente: [não verificado].

A materialização automática por chamado resolvido não é uma atribuição equivalente para os cinco fantasmas: ela seleciona materialização nula e, quando carimba validação, também substitui materialização/autor pelo mesmo `agora/user`: `app/chamados/services/disputa_emissao.py:1536-1539`, `app/chamados/services/disputa_emissao.py:1561-1577`.

Cuidados para a consolidação:

- P50169 tem lastro horário visível na sonda: B137026, 25/10 17:30:12, não retratada, para resposta 17:30; tipo, aprovação e vínculo com a pergunta [não verificado].
- O juiz `_pergunta_materializada_de_fato` avalia `via` sem testemunha antes do ramo de slot e pode reprovar `fantasma` pela lei do dia: `app/chamados/juizes.py:603-623`; logo reprovação desse juiz não prova ausência física da batida respondida.
- As quatro respostas 07:00 têm batidas retroativas visíveis no horário do encerramento noturno na sonda; vínculo `pergunta_origem` [não verificado]. O ramo que cria `origem='disputa_s84_retro'` está em `app/chamados/services/materializacao.py:871-879`.
- A regra L-099 determina ESTRUTURA quando o sistema se contradiz dado o cadastro, CADASTRO quando DNA não descreve batidas, BATIDA quando falta/sobra batida: `CLAUDE.md:306-316`.

## 30/10 07:38 · sessao a46ba23aaa09 · - · - · origem=Ronald (tmux)

O ramo `adesao is None` não registra `out['cods']`: incrementa `nunca_bateu`, lavra/carimba e retorna antes da única gravação `out.setdefault('cods', {})[data] = list(cods)` do ramo normal. (`app/ponto/services/cartorio.py:907`, `:946`, `:981`, `:999`, `:1003`, `:1120`)

O efeito depende do chamador:

| Chamador | Efeito da ausência de `cods` |
|---|---|
| `processar_cartorio` | **Não impede sempre**. Um único `out` nasce antes do laço e é reutilizado entre colaboradores; a chave é somente `data`. Se colaborador anterior registrou aquela data, a célula `nunca_bateu` passa pelo teste e chama `perguntar_pela_celula`. Se a data ainda não existe no acumulado, não chama. (`app/ponto/management/commands/processar_cartorio.py:43`, `:59`, `:65`; `app/ponto/services/cartorio.py:759`, `:768`, `:769`, `:770`, `:1120`) |
| `julgar_celula` | Cria `out` novo e passa uma única célula: no ramo sem adesão, `_julgadas` permanece vazio e a fábrica não é chamada. (`app/ponto/services/cartorio.py:1559`, `:1560`, `:759`, `:769`) |
| `rejulgar_furos_do_vinculo` | Cria `out` novo por invocação: mesma obstrução, caso entre no ramo sem adesão. (`app/escala/regua_defesa.py:691`, `:692`; `app/ponto/services/cartorio.py:1003`, `:759`, `:769`) |
| `celula_veredito_velho` | Passa `contadores()` novo para cada colaborador: mesma obstrução. (`app/ponto/management/commands/celula_veredito_velho.py:139`, `:144`, `:145`) |
| `bin/rejulgar_o118_col221.py` | Usa `out` novo e apenas um colaborador: mesma obstrução se esse ramo for alcançado com `APLICAR=True`. (`bin/rejulgar_o118_col221.py:20`, `:23`, `:24`, `:25`) |

`contadores()` não inicializa `cods`; ela nasce sob demanda no ramo normal. (`app/ponto/services/cartorio.py:1477`, `:1478`, `:1490`, `:1120`)

`cluster_espurio` **não recebe nem interpreta os valores dos códigos**: o wrapper usa apenas a existência de qualquer entrada em `_julgadas` para chamar `_cluster(c, hoje)`. Assim, um acumulado anterior também permite essa chamada para colaborador que não registrou códigos próprios. O serviço lê células/ATA do colaborador atual. (`app/ponto/services/cartorio.py:759`, `:763`, `:765`, `:767`; `app/ponto/services/cluster_espurio.py:22`, `:23`, `:26`, `:27`, `:77`)

A releitura pós-emissão **não usa `cods` como gatilho**: dispara pelo aumento de `emitidos` na chamada corrente, cria `_conv` novo, executa novamente o corpo e mescla os códigos encontrados em `out['cods']`. Não há nova chamada à fábrica após essa releitura. No ramo sem adesão, não ocorre emissão no corpo e esse delta não nasce. (`app/ponto/services/cartorio.py:743`, `:798`, `:810`, `:811`, `:812`, `:823`, `:1003`)

Passar pelo teste de data apenas **alcança a fábrica**; não garante pergunta nova. A fábrica ainda verifica `trabalha`, situação ativa, futuro, vencimentos, veredito e deduplicação por motivo. `nunca_bateu` pertence a `VEREDITOS_ACUSAM`, portanto esse veredito, isoladamente, não a veta. (`app/chamados/services/fabrica_da_celula.py:78`, `:80`, `:85`, `:86`, `:101`, `:106`, `:109`; `app/escala/models.py:1735`, `:1736`)

## 30/10 07:38 · sessao e3bb63ac3aef · - · - · origem=Ronald (tmux)

Referências e limites para consolidação:

- **L-099:** ESTRUTURA significa contradição/cálculo errado dado o cadastro; CADASTRO significa DNA que não descreve batidas; BATIDA significa falta/sobra de batida (`CLAUDE.md:306-316`).
- O juiz efetivo é `app/ponto/management/commands/e6_oraculo.py:53`. A decisão prioriza cadastro sem folga/vínculo (`:90-91`), batida solta declarada pelo realizado (`:96-97`), paridade do conjunto atribuído ao dia (`:98-99`) e presença na lista do MOTOR `dias_cadastro_x_realidade` (`:100`). Os insumos são montados em `:118-155`; batida solta real é conferida em `:158-172`.
- **A sonda não fornece os insumos completos desse juiz:** sua lista de batidas inclui dias vizinhos, sem atribuição do oráculo ao dia; também faltam `dias_cadastro_x_realidade`, resultado do realizado e flags de cadastro. Portanto dono E6 conclusivo por célula é **[não verificado]**.
- Observação permitida pelo dado: nas quatro células de col451, DNA cobra entrada às 19h em dias ímpares e a janela mostra entradas próximas de 19h nos dias pares; na 140114, a resposta/batida 17:30 difere do marco 21:30 e as saídas mostradas próximas de 06:30 diferem do marco05:42. Atribuir isso definitivamente a CADASTRO, excluindo BATIDA, não está provado **[não verificado]**.
- **Dono da contradição “fio morto” é ESTRUTURA**, expressamente documentado em `app/chamados/services/vigia_fio.py:9-14`; isso não identifica automaticamente o dono do furo ou do descompasso de horário. O próprio vigia declara que não deriva o dono L-099 em `:99-101`.

**P50170: cláusula exata não demonstrável.**

`pergunta_viva` delega ao cálculo em `app/chamados/juizes.py:795`. Para P50170, sem validação, sem resposta, via fantasma, materializada e célula `furo`, os únicos retornos `False` anteriores ao ramo de célula acusando compatíveis com os campos fornecidos são:

1. Competência trancada por leitura, `app/chamados/juizes.py:1010-1011`, cujo julgamento exige tranca e ausência de toque humano posterior (`:875-907`).
2. Marco próprio comprovado aceso na **ATA**, apesar de outro marco acusar, `app/chamados/juizes.py:1050-1052`. `orfao_14h` cobra `S/hf` (`:823-828`), e a prova lê `cel.ata['lampadas'][...]['acesa']` (`:866-872`).

A sonda não inclui tranca/toques nem ATA/`acesa`; o bloco `dna.lampadas` é previsão e não satisfaz essa leitura (`app/chamados/juizes.py:849`, `:866-872`). Assim, escolher entre essas cláusulas é **[não verificado]**.

`fantasma` não pertence a `_ENCERRA_POR_IDENTIDADE` (`app/chamados/juizes.py:1039-1045`). Se nenhum dos dois degraus acima a encerrou, célula `furo` produz `True` imediatamente (`:1054-1057`); sequer chega ao teste final de materialização (`:1082-1083`). A disputa fechada não é consultada como causa de morte nesse corpo (`:1005-1087`).

## 30/10 07:33 · sessao b270d6c7c46f · O261-N · sonda · origem=Code

**A sonda não comprova a premissa de “chamado vivo e nenhuma pergunta”:** o CH#35679 aparece como `resolvido`, a célula apresenta `np=4` e o colaborador tem 12 perguntas; o significado de `np` e a vitalidade dessas perguntas não foram detalhados. [nao verificado] `resolvido` está fora de `VIVOS`, e o censo seleciona somente chamados em `VIVOS`. (`app/chamados/catalogo/motor.py:24–31`; `app/chamados/services/vigia_fio.py:186`.)

1. **Batida e ramo do julgamento.** Não tem batida não retratada segundo o anexo: `vida=0`, `nao_retratadas=0`, `ADESAO=None`. [nao verificado] O código procura a primeira batida da vida com `retratada_em IS NULL` e deriva dela a adesão; sem essa batida, entra em **`if adesao is None`**, não no ramo normal. (`app/ponto/services/cartorio.py:899–907`.)

   Esse ramo lavra `nunca_bateu` sem cobertura integral, carimba a célula e retorna antes do ramo normal. (`app/ponto/services/cartorio.py:971–1004`.) Portanto, esse é o ramo determinado pelos insumos fornecidos; a execução histórica não tem trilha anexada. [nao verificado]

2. **Emissor do chamado e autorização pela regra.** O módulo informado para CH#35679 é `batida_ausente`; seu emissor histórico exato não consta da sonda. [nao verificado] O emissor usado pelo cartório é `detectar_ausencias.Command`, cujo `_abrir_chamado` abre esse módulo em `app/ponto/management/commands/detectar_ausencias.py:688–697`; a ligação do cartório está em `app/ponto/services/cartorio.py:1535` e `:1560–1561`.

   Não se pode atribuir CH#35679 exclusivamente a esse método: existem outros escritores com o mesmo módulo/categoria/subcategoria em `app/ponto/management/commands/reconciliar_grade.py:499–510` e `app/chamados/reconciliador.py:862–872`.

   **Nesse ramo, a regra escrita manda não emitir cobrança de furo:** `HX-CARTORIO-NUNCA-BATEU` declara “Dívida de ADESÃO” e “carimba (registro) e não emite”. (`app/ponto/services/cartorio.py:902–906`.) Além disso, `nunca_bateu` pertence a `VEREDITOS_QUE_VETAM`; o gate devolve `NUNCA_BATEU`, e o emissor retorna antes de abrir. (`app/ponto/marcos.py:119`, `:155–162`; `app/ponto/management/commands/detectar_ausencias.py:625–628`.) O veredito vigente no instante da criação e o caminho efetivamente executado não vieram no anexo. [nao verificado]

3. **Pergunta prevista e impedimento.** A regra da fábrica **admite pergunta para `nunca_bateu`**: exige colaborador ativo, célula de trabalho, data não futura e veredito acusador; `nunca_bateu` integra `VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:78–87`; `app/escala/models.py:1735–1736`.) Os motivos concretos dependem da ata, dos marcos vencidos e dos motivos já perguntados. (`app/chamados/services/fabrica_da_celula.py:34–65`, `:101–109`.)

   O responsável seria `perguntar_pela_celula → perguntar → criar_ou_atualizar_disputa / gerar_perguntas_disputa`, com abertura efetiva em `PerguntaDisputa.abrir`. (`app/chamados/services/fabrica_da_celula.py:118–124`, `:143–149`; `app/chamados/services/disputa_emissao.py:389–397`.)

   **A falha de encaminhamento no ramo sem adesão:** ele carimba e retorna sem registrar a data em `out['cods']`; o chamador só chama a fábrica para datas desse mapa. O registro da data ocorre no ramo normal. (`app/ponto/services/cartorio.py:947–1003`, `:759–770`, `:1120`.) Com `out` novo, como em `julgar_celula`, esse ramo não aciona a fábrica. (`app/ponto/services/cartorio.py:1559–1561`.) No lote, porém, `out` é compartilhado entre colaboradores, portanto não cabe afirmar bloqueio absoluto sem conhecer o mapa acumulado. (`app/ponto/management/commands/processar_cartorio.py:43`, `:59–65`.)

   A sonda não traz ata, marcos, motivos, vias ou estados das perguntas; **não permite concluir que falta criar uma pergunta adicional nem identificar o impedimento efetivamente ocorrido nessa célula**. [nao verificado]

4. **Quantidade de colaboradores.** O anexo mostra **um colaborador ativo sem adesão, com chamado histórico `batida_ausente` resolvido**. Não fornece o total da frota nem comprova um chamado atualmente vivo nessa condição. [nao verificado]

   Falta medir os colaboradores ativos distintos sem nenhuma batida não retratada na vida e, dentro desse conjunto, aqueles com chamado de furo em `VIVOS` apontando para célula `nunca_bateu`; para contar emissão contrária à regra no instante em que ocorreu, falta também a trilha da emissão e do veredito naquele instante. [nao verificado] Os predicados correspondentes estão em `app/ponto/services/cartorio.py:899–907`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/furo_modulos.py:15–16` e `app/ponto/marcos.py:119`.

_374 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
