# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 10:00** · publicado em **30/10/2026 10:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 10:00 · sessao bd9f616654d5 · O261-NOTURNO · censo · origem=Code

O campo recebe **três referências diferentes de dia**: dia da célula/entrada, dia do marco de saída e dia civil da tentativa. O helper persiste o valor recebido, sem converter entre essas referências. (`app/chamados/services/fabrica_da_celula.py:122`; `app/chamados/detector_anomalias.py:43-49`; `app/ponto/services/triagem_batida.py:625-656`; `app/chamados/services/disputa_emissao.py:491-498`)

**Escritores de produção de `contexto[motivo]['data']`:**

| Escritor / montagem | Qual dia grava | Arquivo:linha |
|---|---|---|
| `detector_anomalias.detectar_anomalias`, motivo `orfao_14h` | **MARCO: saída esperada** da entrada pendente selecionada. Grava ISO ou `None` quando não obtém a saída esperada. | `app/chamados/detector_anomalias.py:35-49` |
| `fabrica_da_celula.perguntar`, para cada motivo recebido | Literalmente `dia.isoformat()`. No julgamento, o argumento é **DIA DA CÉLULA**, `cel.data`. | `app/chamados/services/fabrica_da_celula.py:118-124,143-149` |
| Command `disparar_perguntas_competencia` → mesma fábrica | **DIA DA CÉLULA** nos dois caminhos: `c.data` nos alvos da célula; `ch.celula.data` no fio mudo. Ambos chegam a `perguntar(colab, dia, [motivo])`. | `app/chamados/management/commands/disparar_perguntas_competencia.py:111-123,165-168,225-226,244,276-289` |
| `disputa_emissao.perguntar_entradas_pendentes` | **DIA DA ENTRADA**: extrai `entradas_pendentes[*]['data']`, classifica esse dia e escreve em `contexto[_motivo]['data']`. A lista original usa a data local da entrada do turno aberto. Salva o contexto somente se `criadas` ficou não vazia. | `app/ponto/utils_turno_orfao.py:31-42`; `app/chamados/services/disputa_emissao.py:255-276` |
| `api/views.py::api_bater_ponto` | **DIA CIVIL LOCAL DA TENTATIVA/RECEPÇÃO**: `agora_local.date()`, derivado de `timezone.now()`. Escreve sob **`saida_sem_entrada`**, embora acrescente **`orfao_14h`** à lista de motivos. | `app/api/views.py:527-528,652-658,777-779` |
| `api/views_core.py::api_bater_ponto` | Mesma referência: **DIA CIVIL LOCAL DA TENTATIVA/RECEPÇÃO**, com a mesma diferença entre motivo acrescentado e chave escrita. | `app/api/views_core.py:519-520,649-655,777-779` |
| `triagem_batida.ramo_registrar_es` | **DIA CIVIL LOCAL DA TENTATIVA**, calculado de `timezone.now()`. Aqui motivo e chave são ambos `saida_sem_entrada`. | `app/ponto/services/triagem_batida.py:625-626,648-660` |

No detector, `escala/utils.py::data_saida_esperada` ancora os marcos no dia local da **entrada**, calcula o último marco encadeado e retorna `fim.date()`. O encadeamento avança dias enquanto o próximo marco for `<=` ao anterior; por isso a saída noturna pode ser **D+1**. (`app/escala/utils.py:891-909,920-931`)

`criar_ou_atualizar_disputa` é o **persistidor dos contextos recebidos**: passa o mesmo contexto para nascimento e merge. No nascimento, `_abrir_ou_agregar_disputa` grava `contexto_nascimento`; na disputa aberta, faz `merged.update(v)`, de modo que uma nova `data` substitui a anterior. Acrescenta `qtd` e `ultima_em`, mas não conserva as duas datas no campo `data`. (`app/chamados/services/disputa_emissao.py:484-501,523-527,598-603`)

Os caminhos citados abaixo **não persistem `contexto[motivo]['data']`**, mas participam da escolha da data da pergunta:

| Caminho | O que efetivamente faz / qual dia usa | Arquivo:linha |
|---|---|---|
| `_data_furo_da_grade` | Retorna o **dia local de `entrada_aberta_em`**, ou de `ultima_batida.timestamp`, depois de confirmar trabalho nesse mesmo dia na grade. Não calcula saída esperada. | `app/chamados/services/disputa_emissao.py:51-69` |
| Fallback da grade em `gerar_perguntas_disputa` | Escreve `data` somente na cópia local `_ctx`, usada no texto; coloca o dia em `_data_fallback`. Não atribui essa cópia a `disputa.contexto`. | `app/chamados/services/disputa_emissao.py:318-331` |
| `_complementar_marcos_faltantes` | Cria perguntas diretamente. Usa o **dia da célula** devolvido por `_marcos_faltantes`, exigindo `dia == data_evento` recebido. | `app/chamados/services/disputa_emissao.py:783-815`; `app/chamados/services/declaracao_texto.py:80-89,104-123` |
| `declaracao_texto`: declaração do colaborador e aplicação pelo admin | Cria/reutiliza pergunta com o **dia da célula do marco casado**. A véspera pode fornecer somente o marco de fim, conservando o dia da célula da véspera. A disputa nasce apenas com `origem`, sem merge de contexto. | `app/chamados/services/declaracao_texto.py:104-123,131-139,292-323,563-583,605-612`; `app/chamados/services/disputa_emissao.py:563-566` |

`criar_disputa_a_partir_de_chamados_orfaos` também não grava esse campo: o contexto de nascimento contém somente origem e IDs dos chamados. Seus caminhos escolhem `data_evento` diretamente: (`app/chamados/services/disputa_emissao.py:927-931`)

- **Intervalo:** usa literalmente `chamado.contexto_json['marcos_faltantes'][*]['data']`. (`app/chamados/services/disputa_emissao.py:947-958`)
- **Pela ata:** obtém o dia por `data_do_chamado` e chama o complemento, que aceita somente faltantes daquele mesmo dia. (`app/chamados/services/disputa_emissao.py:969-979,806-813`)
- **Com batida de origem:** classifica pelo dia local da entrada; depois usa `resolver_data_evento(...) or data_entrada`. Para `orfao_14h`, o resolvedor retorna o **MARCO de saída esperada**, podendo ser D+1. Aqui ele usa a implementação de `escala/servico_jornada.py`. (`app/chamados/services/disputa_emissao.py:1002-1018`; `app/escala/servico_jornada.py:199-208,482-490`)
- **Sem origem:** usa `data_evento_do_contexto`, cuja prioridade é `data_turno`, `data_batida`, `entrada_timestamp`, data BR em `pendentes`, depois `horario_previsto`. Portanto, esse ramo preserva o dia encontrado nessa hierarquia. (`app/chamados/services/disputa_emissao.py:1030-1047`; `app/chamados/data_evento.py:16-39`)
- **Complementação em disputa existente:** usa o dia do contexto do chamado ou, faltando esse, a `data_evento` da pergunta existente. (`app/chamados/services/disputa_emissao.py:874-885`)

Nos demais commands relacionados:

- `reconciliar_grade._disputa` passa somente `{'origem': 'grade'}`; `marcar_foto_ausente_retro` passa `batida_em` e outros metadados, sem chave `data`. (`app/ponto/management/commands/reconciliar_grade.py:415-419`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`)
- `perguntar_chamados_sem_pergunta`, `detectar_ausencias`, `processar_alertas_turno` e `processar_alertas_avancados` delegam à fábrica de órfãos descrita acima. (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20-29`; `app/chamados/services/pergunta_no_app.py:148-159`; `app/ponto/management/commands/detectar_ausencias.py:782-791`; `app/ponto/management/commands/processar_alertas_turno.py:375-384`; `app/ponto/management/commands/processar_alertas_avancados.py:149-158`)
- Há montagem adicional em **fixture de smoke**: data fixa `2026-05-15`, colaborador `_Smoke`, origem `smoke`. (`app/chamados/management/commands/smoke_chamados.py:77-79,90-101`)
- Os chamadores dos métodos genéricos de contexto da disputa escrevem `supervisao_omissa_escalado`, `incidente_operacional` e `ultimo_alerta_diario`. (`app/chamados/views.py:1343-1344`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159-162`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`)

**Quem lê para definir a pergunta e a célula:**

1. `texto_pergunta_com_data` lê `contexto[motivo]['data']` para interpolar o texto. Se falta data necessária, o gerador tenta `_data_furo_da_grade`. (`app/chamados/catalogo/query.py:58-66`; `app/chamados/services/disputa_emissao.py:317-329`)
2. Em `disputa_emissao.py:343-347`, se `_data_fallback` ainda está vazio, lê o campo e converte seus primeiros dez caracteres em `date`. Depois tenta o dia local de `ultima_batida.timestamp`. (`app/chamados/services/disputa_emissao.py:343-365`)
3. `_derivar_chamado_destino` recebe essa data com precedência sobre o contexto, compara-a por igualdade com `dia_do_fato(ch)` e devolve a data resolvida. A abertura recebe `_data_ev`, ou `_data_fallback` quando aquele é `None`. (`app/chamados/services/disputa_emissao.py:174-179,213-237,389-396`)
4. [`PerguntaDisputa.abrir`](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2028) recebe `data_evento`; não deriva essa data do contexto. Busca `CelulaDia` por **colaborador e `data=data_evento`**. Assim, D busca célula D; D+1 busca célula D+1, se existente. Sem data recebida, trata como conversa, usa `localdate()` e não executa esse ramo de célula/dedup. (`app/chamados/models.py:2054-2061,2085-2086`)

**Efeito dos dias diferentes no dedup:**

- O dedup local usa igualdade literal de data: intervalo consulta `(disputa, motivo, _data_fallback)`; motivos com destino consultam `(disputa, motivo, _data_ev)`. Se o derivador não acha destino, `_data_ev` é `None`, embora a abertura ainda possa receber `_data_fallback`. (`app/chamados/services/disputa_emissao.py:221-237,369-396`)
- No caminho de órfãos, `_ja_perguntou` também recorta motivo e data exatos dentro da disputa, depois consulta se há fio vivo. (`app/chamados/services/disputa_emissao.py:1202-1209`)
- O gate final amplia o dedup para **`(colaborador, motivo, data_evento)` entre disputas**, conservando a igualdade literal do dia. Para a mesma identidade, devolve a última pergunta existente e pode reapontar destino/célula. (`app/chamados/models.py:2022-2025,2058-2084`)
- **D e D+1 não são fundidos pelo dedup**, mesmo quando correspondem ao mesmo turno. Cada data constitui identidade distinta e busca sua própria célula; o nascimento continua sujeito às guardas de cada caminho. A data nova no contexto também não altera a `data_evento` da pergunta anterior. (`app/chamados/services/disputa_emissao.py:369-396,491-501`; `app/chamados/models.py:2022-2025,2058-2086`)
- A validação do destino pode aceitar D/D+1 como mesma jornada quando há cruzamento de meia-noite ou quando ele é indeterminado; essa validação não modifica as datas nem unifica as identidades do dedup. (`app/chamados/models.py:2044-2051`; `app/chamados/juizes.py:1721-1738`)

## 30/10 09:59 · sessao 017c27885e0f · FOLGA-TRABALHADA-RESOLVE · leitura · origem=Code

O caminho atual bloqueia folga em dois pontos: a oferta exige `eh_trabalho`, e o serviço recusa `tipo_dia != 'trabalho'`. Além disso, a exclusividade de `flip_tipo` não se confirma: a regularização externa também altera `Batida.tipo` diretamente. (`app/colaboradores/services/calendario.py:196–197`; `app/ponto/services/veredito_celula.py:221–222`; `app/chamados/services/regularizacao_ext.py:153–155`)

**1. Oferta do “Resolver dia” e chegada de `trabalhou`**

- `resolver_dia_oferecido` exige: `pode_ultimo_recurso`, `eh_trabalho`, dia passado e status em `falta`, `irregular`, `aberto`, `sem_certidao`. `pode_ultimo_recurso` exige dia anterior ao início da competência atual e nenhuma pergunta viva com resposta. (`app/colaboradores/services/calendario.py:188–197`, `569–579`, `874–875`)
- `contexto_calendario` chama esse juiz e grava `dia['resolver_dia_oferecido']`. A porta curta `resolver_dia_oferecido_na_data` monta o calendário de um dia e lê o mesmo campo. (`app/colaboradores/services/calendario.py:200–208`, `902–905`)
- A view `calendario_ponto` chama `contexto_calendario`, libera `pode_veredito` conforme a permissão `regularizar_batida` e renderiza `calendario_partial.html`; este inclui a grade. A grade inclui o popover somente com `pode_veredito and dia.resolver_dia_oferecido`. (`app/colaboradores/views.py:1935–1962`; `app/templates/colaboradores/calendario_partial.html:62`; `app/templates/colaboradores/partials/_calendario_grade.html:125–126`)
- O predicado decide a presença do botão; as opções são fixas no template. Hoje `trabalhou` aparece como **“Trabalhou no horário da escala”**. O formulário envia POST com `colab`, `data` ISO, `volta` e `veredito=trabalhou`. (`app/templates/ponto/partials/_veredito_dia.html:17–25`)
- Rota efetiva: `/ponto/celula/veredito/` → `veredito_celula_view`. A view exige POST e `regularizar_batida`; aceita `datas` ou `data`, valida as datas e o veredito, depois chama `veredito_celula(colaborador=colab, data=data, veredito=veredito, autor=request.user, tipo_ausencia=… or None, request=request)`. (`app/config/urls.py:29`; `app/ponto/urls.py:58`; `app/ponto/views.py:3302–3331`)

**2. Ramo `trabalhou`, janela, julgamento e final comum**

Nesta revisão, `:220` calcula a grade; a recusa está em `:221–222`. A janela é **`[data 00:00 local, data+2 dias 00:00)`**, ordenada por `timestamp`. Essa consulta não filtra retratadas nem intocáveis. (`app/ponto/services/veredito_celula.py:212–222`)

Para dia de trabalho, o ramo segue assim:

1. Se já perfeito, registra a ação “o dia ja estava completo, nada a corrigir”. Caso contrário, exige marcos, monta a cadeia absoluta e calcula candidatos a flip por `decidir_tipo_estrito`, pulando origens intocáveis e aprovação humana. (`app/ponto/services/veredito_celula.py:223–245`)
2. Simula combinações de flips e preenchimentos por template ou vizinho. Só escolhe combinação aprovada por `dia_perfeito`; sem vencedora, recusa como dia ambíguo. (`app/ponto/services/veredito_celula.py:246–289`)
3. Executa os flips por `flip_tipo(... autor=autor, motivo='veredito_celula trabalhou', acao_log='veredito_flip')`; materializa faltantes por `registrar_batida`, origem `manual`, autoridade humana e aprovação do autor. (`app/ponto/services/veredito_celula.py:290–307`)
4. Chama `recompute_turnos(colaborador, data−1, data+1)`. (`app/ponto/services/veredito_celula.py:308`)

O **final comum existente** é:

- Erros sem nenhuma ação provocam rollback e retorno. Com ações e erros, o código ainda segue para fechamento/lavra e termina com `ok=False`. (`app/ponto/services/veredito_celula.py:321–337`)
- `fechar_o_fio_do_dia` busca perguntas pelo colaborador/data e `q_vivas()`, chamando `fechar_por_admin(autor)`. Essa porta grava via `ADMIN_DECIDIU`, `materializada_em` e `materializada_por`, sem plantar batida. (`app/ponto/services/veredito_celula.py:174–178`; `app/chamados/models.py:2117–2135`)
- Os chamados vivos são reunidos tanto por `contexto_json.data_turno` quanto pelo vínculo com a célula. São ordenados por `sobrevivente_por_ultimo` e passam individualmente por `reconciliar()`; somente quando este retorna verdadeiro recebem ocorrência `dia_decidido_pelo_admin`. Portanto, o fechamento não é incondicional. (`app/ponto/services/veredito_celula.py:148–157`, `181–187`; `app/chamados/models.py:434–445`, `474–525`)
- **Depois do fechamento do fio**, havendo célula, chama `cel.lavrar_veredito(via='admin', veredito=veredito, ator=autor)`. O método delega à porta da célula, que escreve veredito/via/instante, preserva protestos, salva com evento de outbox e grava trilha antes/depois. A mesma lavra repetida retorna sem salvar. (`app/ponto/services/veredito_celula.py:324–330`; `app/escala/models.py:1775–1783`; `app/ponto/portas/celula.py:634–642`, `682–702`)
- O despachante ainda chama `registrar_log` com veredito, ações e erros. Após sucesso, a view chama `reconciliar_apos_vinculo(... motivo='resolver_dia')`. (`app/ponto/services/veredito_celula.py:334–337`; `app/ponto/views.py:3339–3341`)

`grade_dia` delega a `grade_espelho_janela(... data, data, dias=1, agora=agora)`. Na autoridade da escala, célula existente devolve seu `trabalha`; no builder por turno, `False` impede a montagem dos marcos de trabalho, e o dia sem essa montagem sai como `tipo_dia='folga'`, previsto zero e células `None`. Batidas não consumidas entram como órfãs; o overlay de `FolgaDia` limpa células e órfãs. (`app/ponto/services/simular_dia.py:5–8`; `app/escala/models.py:1066–1073`; `app/escala/utils.py:1541–1549`, `1743–1744`, `1755–1776`, `1114–1120`)

**Não existe “folga perfeita” nesse predicado:** `dia_perfeito` retorna falso para qualquer `tipo_dia != 'trabalho'`, inclusive com E,S,E,S. Para trabalho, exige ausência de órfãs, de `missing` e de divergência entre `tipo_real` e tipo do marco. (`app/ponto/services/simular_dia.py:11–23`)

**3. Intocáveis e censo dos escritores de tipo**

No veredito, `ORIGENS_INTOCAVEIS = ('manual', 'disputa_s84_retro')`; também é pulada qualquer batida com `aprovada_por_id`. O automático inclui retratadas no tratamento de “intocável”. (`app/ponto/services/veredito_celula.py:19`, `239–245`; `app/ponto/services/flip_auto.py:287–299`)

Essa proteção pertence aos **chamadores**: `flip_tipo` não verifica origem nem `aprovada_por_id`. Suas recusas são retratação, ausência de autor/request, competência bloqueada e tipo diferente de E/S. `anotar` admite o campo `tipo` e o salva sem guarda de origem. (`app/ponto/services/flip_batida.py:15–33`; `app/ponto/registro_batida.py:203–205`, `235–254`)

| Escritor encontrado | Escrita |
|---|---|
| `registrar_batida` | Define `tipo` na criação. (`app/ponto/registro_batida.py:152–159`) |
| `flip_tipo` → `anotar` | Calcula o oposto e persiste por `setattr`/`save`. (`app/ponto/services/flip_batida.py:25–33`; `app/ponto/registro_batida.py:252–254`) |
| Regularização externa, ação `corrigir` | Escolhe E/S, atribui `alvo.tipo` e salva diretamente. `alvo` vem de `item.batida_alvo`, FK para `Batida`. (`app/chamados/services/regularizacao_ext.py:127`, `142–155`; `app/chamados/models.py:2730–2732`) |
| `semear`, banco de ensaio | Cria batidas sintéticas em lote, com guarda de banco teste/sombra. (`app/ponto/registro_batida.py:316–337`) |

Logo, **não é possível provar `flip_tipo` como único escritor de alteração de tipo neste código**: há o contraexemplo concreto da regularização externa. (`app/chamados/services/regularizacao_ext.py:153–155`)

Não foram encontrados outros `update`, `bulk_update` ou SQL de alteração de `Batida.tipo` na busca estática; exaustividade diante de execução dinâmica ou escritores externos: **[não verificado]**.

**4. Recompute, lote e idempotência**

Cada flip bem-sucedido faz um recompute explícito. Além disso, `anotar.save()` dispara `post_save`, cujo receiver faz outro recompute; os signals são importados em `PontoConfig.ready`. Para N flips, são **N chamadas explícitas + N pelo signal**, e o ramo `trabalhou` acrescenta seu recompute final: **2N+1 nesses sítios**, sem contar outros efeitos. (`app/ponto/services/flip_batida.py:30–39`; `app/ponto/registro_batida.py:254`; `app/ponto/signals.py:19–36`; `app/ponto/apps.py:7–8`; `app/ponto/services/veredito_celula.py:308`)

O trecho chamado `HX-FLIP-LOTE` expõe o conjunto de flips; o comando continua percorrendo batidas e chamando `flip_tipo` individualmente. `com_flips` apenas simula em memória com `SimpleNamespace`. Não localizei API persistente de flip em lote pelo mesmo escritor. (`app/ponto/services/flip_auto.py:315–320`; `app/ponto/management/commands/flip_automatico.py:62–68`; `app/ponto/services/simular_dia.py:26–32`; ausência global: **[não verificado]**)

`flip_tipo` é inversão, sem parâmetro de tipo desejado. Chamado em todas as batidas de E,S,E,S, produz S,E,S,E; repetido, volta a E,S,E,S, com nova escrita/trilha/recompute. `anotar` é idempotente para valores iguais, mas o flip sempre pede o oposto. Já o chamador atual só enfileira quando o tipo calculado difere do gravado. (`app/ponto/services/flip_batida.py:10`, `25–39`; `app/ponto/registro_batida.py:239–254`; `app/ponto/services/veredito_celula.py:243–245`)

**5. Corte de 26/10**

O nome `PAUSA-BATIDA-NA-FOLGA-SEM-TIPO` não foi localizado em `app/docs/CORTES.md` nem em `app/docs/LEIS.md`. **[não verificado nesses dois documentos]**

O registro encontrado está em `app/docs/PROMPTS.md:1483`:

> “26/10 11:0x | **aval: PAUSA-BATIDA-NA-FOLGA-SEM-TIPO = nem (a) nem (b)** -- sem marco o tipo e o gravado (`decidir_tipo_estrito`), E,E,S,S = dono BATIDA, lista do admin; os 92 pares em dia de trabalho viram linha da fila 1 com o censo, sem cura antes. Estado: BACKLOG `R1-FLIP-NAO-VIROU`.”

A reprodução em `app/docs/RELATO.md:577` diz:

> “**RESPONDIDA 26/10 11:0x: PAUSA-BATIDA-NA-FOLGA-SEM-TIPO = nem (a) nem (b)** -- folga sem marco = BATIDA, lista do admin; pousada.”

O decisor confirma a conservação do gravado quando não há marcos. (`app/ponto/turnos.py:2361–2368`)

**6. PRONTO: #35892, col502, 10/10**

Célula, batidas, vínculos do chamado e competência desse caso: **[não verificado]**.

As verificações sustentadas pelo código são:

- Célula `trabalha=False` caracteriza a folga na autoridade da escala; ausência de marcos deve ser conferida separadamente, pois `_marcos_do_dia` pode recorrer ao template vigente. (`app/escala/models.py:1066–1073`; `app/ponto/services/veredito_celula.py:29–44`)
- Elegibilidade: excluir retratadas e respeitar origens `manual`/`disputa_s84_retro` e aprovação humana. Cada flip efetivo exige tipo E/S e autor/request. (`app/ponto/services/flip_batida.py:15–28`; `app/ponto/services/veredito_celula.py:19`, `241–242`)
- Competência “aberta”, para o flip, significa que o fechamento da competência de cada batida, calculada pela data local e corte da empresa, não esteja `aprovado` nem `exportado`. (`app/ponto/janelas.py:75–89`)
- **PAR aceita / ÍMPAR recusa e alternância cronológica ainda não são regras desse ramo.** Hoje a folga é recusada antes de flip e lavra; ter quantidade par e competência aberta não basta para esse dia passar. (`app/ponto/services/veredito_celula.py:220–245`, `271–308`; `app/ponto/services/simular_dia.py:11–13`)

## 30/10 09:58 · sessao 0c23c8bc638a · - · - · origem=Ronald (tmux)

Censo dos caminhos fora do núcleo:

| Caminho | Escrita de `contexto[motivo]['data']` / dia usado |
|---|---|
| `api/views.py` | `contexto_anomalia['saida_sem_entrada']['data'] = agora_local.date().isoformat()`; `agora_local` vem do relógio atual. É **dia civil local da tentativa/recepção**, sem usar `data_turno` nem `data_saida_esperada`. O motivo adicionado à lista é `orfao_14h`, mas a chave escrita é `saida_sem_entrada`. (`app/api/views.py:527-528`, `:652-658`). |
| `api/views_core.py` | Mesma escrita e semântica, com a mesma diferença entre motivo `orfao_14h` e chave `saida_sem_entrada`. (`app/api/views_core.py:519-520`, `:649-655`). |
| Persistência dessas APIs | Ambas passam o contexto montado a `criar_ou_atualizar_disputa`. (`app/api/views.py:777-779`; `app/api/views_core.py:777-779`). |
| PWA `triagem_batida.ramo_registrar_es` | Motivo e chave são `saida_sem_entrada`; grava `agora_local.date()`, calculado de `timezone.now()`: **dia civil local da tentativa**, não uma data extraída da célula/turno. (`app/ponto/services/triagem_batida.py:625-626`, `:648-660`). |
| `disparar_perguntas_competencia` | Delega a `fabrica_da_celula.perguntar`; alvos normais usam `c.data` de `CelulaDia`, e fio mudo usa `ch.celula.data`: **dia da célula**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `:104-123`, `:155-167`, `:277-289`). |

`declaracao_texto.py` **não escreve esse campo**: os dois caminhos usam `disputa_para_declaracao`, cujo nascimento leva somente `{'origem': origem}` e cujo `merge_contexto=None`; passam o dia diretamente a `PerguntaDisputa.abrir(data_evento=...)`. (`app/chamados/services/declaracao_texto.py:307-323`, `:605-612`; `app/chamados/services/disputa_emissao.py:563-566`).

O dia da declaração é **o dia da célula lida**, inclusive quando a candidata é a célula da véspera: `_marcos_faltantes` percorre `(d, vespera)`, aceita somente posição `fim` da véspera e devolve `data_dia`; o casamento retorna essa tupla e o escritor usa seu `dia`. Não incrementa o dia para o marco final noturno. (`app/chamados/services/declaracao_texto.py:80-89`, `:104-123`, `:131-139`, `:292-323`). No caminho de texto aplicado, a proposta preserva o mesmo dia do casamento e o passa à abertura. (`app/chamados/services/declaracao_texto.py:563-583`, `:600-612`).

Caminhos citados que **não montam `motivo.data`**:

- API `entrada_sem_saida` monta somente `entrada_aberta_em` e `hora_tentativa`; API `foto_ausente` monta somente `tipo_batida`. (`app/api/views.py:483`, `:672-675`; `app/api/views_core.py:475`, `:669-672`).
- `marcar_foto_ausente_retro` monta `foto_ausente` com `tipo_batida`, `batida_id`, `batida_em` e `_por`, sem `data`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`).
- `reconciliar_grade._disputa` nasce com `{'origem': 'grade'}`; não passa `merge_contexto`. (`app/ponto/management/commands/reconciliar_grade.py:415-419`).
- `perguntar_chamados_sem_pergunta` delega ao serviço `pergunta_no_app.perguntar`, que chama `criar_disputa_a_partir_de_chamados_orfaos`; não monta datas próprias. (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20-29`; `app/chamados/services/pergunta_no_app.py:148-159`).
- Os comandos `detectar_ausencias`, `processar_alertas_turno` e `processar_alertas_avancados` também delegam à fábrica de órfãos. (`app/ponto/management/commands/detectar_ausencias.py:782-791`; `app/ponto/management/commands/processar_alertas_turno.py:375-384`; `app/ponto/management/commands/processar_alertas_avancados.py:149-158`).
- Chamadores de `DisputaSupervisao.anotar_contexto/renovar_contexto` escrevem `supervisao_omissa_escalado`, `incidente_operacional` e `ultimo_alerta_diario`, não `motivo.data`. (`app/chamados/views.py:1343-1344`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159-162`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`).

O command `smoke_chamados` tem uma montagem adicional, mas é **fixture de smoke**: fixa `DATA_OK = "2026-05-15"` e envia `{motivo: {"data": DATA_OK}}` quando o texto contém `{data}`, usando colaborador `_Smoke` e origem `smoke`. Não representa dia calculado de turno real. (`app/chamados/management/commands/smoke_chamados.py:77-79`, `:90-101`).

## 30/10 09:58 · sessao d9c50c254f0a · - · - · origem=Ronald (tmux)

Itens 3/4 consolidados:

- **Intocáveis:** `ORIGENS_INTOCAVEIS = ('manual', 'disputa_s84_retro')`; o ramo `trabalhou` também preserva qualquer batida com `aprovada_por_id`. A fila automática soma `retratada_em` ao filtro. (`app/ponto/services/veredito_celula.py:19`, `app/ponto/services/veredito_celula.py:241`, `app/ponto/services/flip_auto.py:287`)
- **Essa proteção pertence aos chamadores:** `flip_tipo` recusa retratada, falta de autor/request, competência fechada e tipo diferente de E/S, mas não verifica origem protegida nem aprovação humana. (`app/ponto/services/flip_batida.py:15`, `app/ponto/services/flip_batida.py:19`, `app/ponto/services/flip_batida.py:21`, `app/ponto/services/flip_batida.py:25`)

**Censo de escritores de `Batida.tipo`:**

| Categoria | Escrita comprovada |
|---|---|
| Criação normal | `registrar_batida`: `objects.create(... tipo=tipo ...)`, nos dois caminhos. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`) |
| Alteração pelo flip | `flip_tipo` chama `anotar(... {'tipo': novo})`; `anotar` admite `tipo`, aplica `setattr` e salva. (`app/ponto/services/flip_batida.py:30`, `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:252`) |
| **Outro escritor ativo** | `materializar_regularizacao_externa`, ramo `corrigir`: escolhe E/S, atribui `alvo.tipo` e faz `alvo.save(update_fields=['tipo','timestamp'])`. `alvo` vem de `item.batida_alvo`, FK para `ponto.Batida`. (`app/chamados/services/regularizacao_ext.py:127`, `app/chamados/services/regularizacao_ext.py:142`, `app/chamados/services/regularizacao_ext.py:153`, `app/chamados/models.py:2730`) |
| Ensaio | `semear`: `Batida.objects.bulk_create([Batida(**d)...])`, protegido por `exigir_banco_de_ensaio`. (`app/ponto/registro_batida.py:331`, `app/ponto/registro_batida.py:316`) |
| Legado desabilitado | AEJ antigo fazia `get_or_create(tipo=tipo_batida)`; a view atual retorna HTTP 410. (`app/ponto/legacy/aej_importer.py.disabled:140`, `app/ponto/views.py:54`, `app/ponto/views.py:65`) |
| Apenas memória | `com_flips` e candidatas do veredito usam `SimpleNamespace`. (`app/ponto/services/simular_dia.py:26`, `app/ponto/services/veredito_celula.py:269`) |
| Django admin | `BatidaAdmin` usa `SoLeitura`, que recusa inclusão, alteração e exclusão. (`app/ponto/admin.py:11`, `app/core/admin.py:59`) |

Portanto, **o snapshot não prova exclusividade de `flip_tipo`: contém outro escritor ativo de tipo**. (`app/chamados/services/regularizacao_ext.py:153`)

O censo AST existente não denuncia esse caso: sua saída atribui `alvo.save()` a entidade desconhecida, e o gate ignora entidade `None`; o próprio censo documenta esse balde. (`app/core/censo_escritas.py:43`, `app/ponto/tests/test_chokepoint_batida_gate.py:30`) A execução feita foi `python3 -B` com `runpy` e leitura AST; resultado: Batida atribuída somente aos sítios de criação/save/delete/semeadura da porta, enquanto `regularizacao_ext.py:155` aparece como desconhecida. [não verificado: saída de ferramenta sem artefato persistido]

Não encontrei `bulk_anotar`, outro `anotar` passando `tipo`, ou `.update`/`.bulk_update`/SQL ativo de `Batida.tipo`; exaustividade contra escrita dinâmica ou executores externos não é demonstrável por essas buscas. [não verificado]

- **Recomputes:** cada flip bem-sucedido chama um recompute explicitamente, e o `save` de `anotar` dispara outro pelo receiver `post_save`; `PontoConfig.ready` importa esses signals. No percurso normal, N flips produzem N chamadas explícitas + N pelo signal; `trabalhou` ainda chama um recompute final: **2N+1 nesses sítios**, sem contar outros efeitos disparados pelos signals. (`app/ponto/services/flip_batida.py:39`, `app/ponto/registro_batida.py:254`, `app/ponto/signals.py:26`, `app/ponto/signals.py:34`, `app/ponto/apps.py:7`, `app/ponto/services/veredito_celula.py:308`)
- **Lote:** `HX-FLIP-LOTE` expõe o conjunto de flips; a gravação percorre esse conjunto chamando `flip_tipo` por batida. Não constitui escritor com recompute agregado. (`app/ponto/services/flip_auto.py:315`, `app/ponto/management/commands/flip_automatico.py:62`)
- **Idempotência:** `flip_tipo` é inversão, não atribuição de tipo desejado: E,S,E,S → S,E,S,E; uma segunda passagem → E,S,E,S, com novas escritas/trilhas/recomputes. `anotar` evita escrita quando o valor pedido já existe, mas o flip sempre pede o oposto. (`app/ponto/services/flip_batida.py:25`, `app/ponto/services/flip_batida.py:30`, `app/ponto/registro_batida.py:239`, `app/ponto/registro_batida.py:248`)

## 30/10 09:58 · sessao a825f0b32663 · O261-NOTURNO · sonda · origem=Code

Transcrição do dado fornecido; os números não foram conferidos em produção. `[nao verificado]`

| Motivo | Campos após o motivo em `DEV` | Valor | Verificação |
|---|---|---:|---|
| intervalo_saida | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 510 | [nao verificado] |
| intervalo_saida | cel_trabalha=False\|marco_da_vespera_cai_hoje=True | 114 | [nao verificado] |
| intervalo_saida | cel_trabalha=False\|marco_da_vespera_cai_hoje=True\|viva | 1 | [nao verificado] |
| intervalo_saida | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 6 | [nao verificado] |
| intervalo_saida | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 6961 | [nao verificado] |
| intervalo_saida | cel_trabalha=True\|marco_da_vespera_cai_hoje=True | 142 | [nao verificado] |
| intervalo_saida | cel_trabalha=True\|marco_da_vespera_cai_hoje=True\|viva | 59 | [nao verificado] |
| intervalo_saida | sem_celula | 286 | [nao verificado] |
| intervalo_volta | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 506 | [nao verificado] |
| intervalo_volta | cel_trabalha=False\|marco_da_vespera_cai_hoje=True | 126 | [nao verificado] |
| intervalo_volta | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 5 | [nao verificado] |
| intervalo_volta | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 7398 | [nao verificado] |
| intervalo_volta | cel_trabalha=True\|marco_da_vespera_cai_hoje=True | 309 | [nao verificado] |
| intervalo_volta | cel_trabalha=True\|marco_da_vespera_cai_hoje=True\|viva | 67 | [nao verificado] |
| intervalo_volta | sem_celula | 279 | [nao verificado] |
| orfao_14h | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 1013 | [nao verificado] |
| orfao_14h | cel_trabalha=False\|marco_da_vespera_cai_hoje=True | 404 | [nao verificado] |
| orfao_14h | cel_trabalha=False\|marco_da_vespera_cai_hoje=True\|viva | 5 | [nao verificado] |
| orfao_14h | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 20 | [nao verificado] |
| orfao_14h | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 11917 | [nao verificado] |
| orfao_14h | cel_trabalha=True\|marco_da_vespera_cai_hoje=True | 483 | [nao verificado] |
| orfao_14h | cel_trabalha=True\|marco_da_vespera_cai_hoje=True\|viva | 89 | [nao verificado] |
| orfao_14h | sem_celula | 1167 | [nao verificado] |
| saida_sem_entrada | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 962 | [nao verificado] |
| saida_sem_entrada | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 5 | [nao verificado] |
| saida_sem_entrada | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 8816 | [nao verificado] |
| saida_sem_entrada | sem_celula | 639 | [nao verificado] |
| ERROS | — | 0 | [nao verificado] |

O mapeamento do motivo para o marco está confirmado na cópia:

| Motivo | Tipo | Marco | Fonte |
|---|---|---|---|
| intervalo_saida | S | hii | [juizes.py:826](app/chamados/juizes.py#L826) |
| intervalo_volta | E | hfi | [juizes.py:827](app/chamados/juizes.py#L827) |
| orfao_14h | S | hf | [juizes.py:825](app/chamados/juizes.py#L825) |
| saida_sem_entrada | E | hi | [juizes.py:824](app/chamados/juizes.py#L824) |

**O critério descrito é compatível com `montar_lampadas`:** tomando `D = data_evento` e a célula da véspera como ancorada em `D−1`, o marco dessa célula cai em `D` quando o turno cruza a meia-noite e a hora do marco é estritamente menor que `hi`. A função aplica exatamente esse avanço de um dia e grava o resultado em `instante`. [gerar_celulas.py:55](app/ponto/management/commands/gerar_celulas.py#L55), [gerar_celulas.py:67](app/ponto/management/commands/gerar_celulas.py#L67), [gerar_celulas.py:69](app/ponto/management/commands/gerar_celulas.py#L69)

Isso também significa que `hi` não avança por essa regra; se `hf == hi`, esse marco tampouco avança, pois a comparação é `<`. [gerar_celulas.py:67](app/ponto/management/commands/gerar_celulas.py#L67)

A implementação efetiva da sonda, a seleção da célula da véspera e a afirmação de que o detector gravou o dia da saída nesses registros permanecem **[nao verificado]**: o resultado raspado não mostra essas operações.

## 30/10 09:58 · sessao 19b8586268d3 · - · - · origem=Ronald (tmux)

Censo dos caminhos delegados, somente leitura:

- **`criar_ou_atualizar_disputa` não calcula o dia**: repassa `contexto` como contexto de nascimento e de merge. No nascimento, o núcleo grava o objeto recebido integralmente; na atualização, `merged.update(v)` troca o valor antigo de `data` pelo recebido, preservando outras chaves e atualizando contadores. Portanto, o dia depende do chamador. `app/chamados/services/disputa_emissao.py:598-603`, `:484-501`, `:523-526`.
- **`perguntar_entradas_pendentes` escreve `contexto[motivo]['data']`**: extrai cada `entradas_pendentes[*]['data']`, classifica esse dia e injeta exatamente `dia.isoformat()`. Atribui a `disputa.contexto`; salva somente se a lista `criadas` ficou não vazia. `app/chamados/services/disputa_emissao.py:255-276`.
- **`_data_furo_da_grade` não escreve o campo**: usa o dia local de `entrada_sem_saida.entrada_aberta_em`, ou de `ultima_batida.timestamp`, confirma que esse mesmo dia é trabalho na grade e retorna seu ISO. Não calcula a saída esperada nem avança para D+1. `app/chamados/services/disputa_emissao.py:51-69`.
- **Fallback de `gerar_perguntas_disputa` não persiste o campo**: escreve `data` somente na cópia local `_ctx`, usada para montar o texto; coloca o resultado em `_data_fallback`, mas não atribui `_ctx` a `disputa.contexto`. `app/chamados/services/disputa_emissao.py:318-331`.
- **`_complementar_marcos_faltantes` não escreve `contexto[motivo]['data']`**: recebe `data_evento`, lê os faltantes e só aceita tuplas com `dia == data_evento`; passa esse `dia` diretamente à abertura da pergunta. `app/chamados/services/disputa_emissao.py:783-815`, `:770-777`. Os dias das tuplas são as chaves dos dias da grade/célula, inclusive o dia da célula da véspera para seu marco de fim; não são calculados do horário do marco. `app/chamados/services/declaracao_texto.py:80-89`, `:104-123`.
- **`criar_disputa_a_partir_de_chamados_orfaos` também não grava esse campo**: seu contexto de nascimento contém somente origem e IDs. `app/chamados/services/disputa_emissao.py:927-931`. Cria perguntas diretamente com:
  - Intervalo: `marcos_faltantes[*]['data']` do contexto do chamado. `:947-958`.
  - Caminho da ata: dia retornado por `data_do_chamado`, passado ao complemento. `:969-979`.
  - Grupo com batida de origem: dia local da entrada para classificar; depois `resolver_data_evento(...) or data_entrada` para a pergunta. `:1002-1018`. Para `orfao_14h`, esse resolvedor retorna **dia do marco de saída esperada**, mediante `servico_jornada.data_saida_esperada`, que retorna `fim_turno(...).date()`. `app/escala/servico_jornada.py:482-490`, `:199-208`. O fim é o último marco encadeado do turno. `app/escala/servico_jornada.py:89-92`.
  - Sem origem: dia de `data_evento_do_contexto(ch.contexto_json)` diretamente. `app/chamados/services/disputa_emissao.py:1030-1049`. A hierarquia é `data_turno`, `data_batida`, `entrada_timestamp`, data BR em `pendentes`, `horario_previsto`. `app/chamados/data_evento.py:16-39`.
  - Repasse a disputa já existente: usa o dia do contexto do chamado, ou `PerguntaDisputa.data_evento` anterior. `app/chamados/services/disputa_emissao.py:874-885`.

Leitura e célula:

- `texto_pergunta_com_data` lê `contexto[motivo]['data']` para interpolar o texto. `app/chamados/catalogo/query.py:58-66`.
- `gerar_perguntas_disputa` prioriza `_data_fallback` obtida da grade; na ausência, lê literalmente `contexto[motivo]['data']` e converte os primeiros dez caracteres em `date`. Depois tenta o dia local de `ultima_batida.timestamp`. `app/chamados/services/disputa_emissao.py:322-329`, `:343-365`.
- `_derivar_chamado_destino` recebe essa data com precedência sobre o contexto; compara com `dia_do_fato(ch)` por igualdade e devolve a data da pergunta/contexto, ou a do chamado. `app/chamados/services/disputa_emissao.py:174-179`, `:213-237`. A abertura recebe `_data_ev`, ou `_data_fallback` quando aquele é `None`. `:389-396`.
- `PerguntaDisputa.abrir` não lê o contexto: recebe `data_evento` e procura `CelulaDia` por **colaborador e igualdade `data=data_evento`**. Portanto, D seleciona a célula D e D+1 seleciona a célula D+1, se existente. `app/chamados/models.py:2028`, `:2058-2061`, `:2085-2086`.

Dedup quando os escritores fornecem dias distintos:

- O dedup local usa igualdade literal `(disputa, motivo, data_evento)`: intervalo compara com `_data_fallback`; motivos com destino comparam com `_data_ev`. `app/chamados/services/disputa_emissao.py:369-388`. No caminho de órfãos, `_ja_perguntou` também filtra motivo/data exatos dentro da disputa, depois consulta a condição de fio vivo. `:1202-1209`.
- O gate final amplia a identidade para **`(colaborador, motivo, data_evento)` entre quaisquer disputas**, também por igualdade literal da data. Reaponta célula/destino e retorna a última pergunta existente desse mesmo dia. `app/chamados/models.py:2022-2025`, `:2058-2084`.
- Assim, **D e D+1 são identidades distintas para o dedup**: não há conversão para um dia comum do turno nesses filtros; se os caminhos alcançarem a criação e passarem pelas guardas, podem produzir perguntas distintas, cada qual ligada à célula de sua própria `data_evento`. `app/chamados/services/disputa_emissao.py:369-396`; `app/chamados/models.py:2022-2025`, `:2058-2086`.
- A validação de chamado-destino pode aceitar D versus D+1 como mesmo turno se o turno cruza a meia-noite, ou se essa condição é indeterminada; isso valida o elo, sem modificar a data nem a identidade do dedup. `app/chamados/models.py:2044-2051`; `app/chamados/juizes.py:1721-1738`.

## 30/10 09:58 · sessao 21bfa79ae7b2 · - · - · origem=Ronald (tmux)

| Escritor/caminho | Dia gravado em `contexto[motivo]['data']` | Evidência |
|---|---|---|
| `detectar_anomalias`: `orfao_14h` | **MARCO de saída esperada** da entrada pendente escolhida; pode ser D+1; grava `None` se não consegue obter saída. Seleciona primeira pendência com ≥14 h, senão primeira pendência. | `app/chamados/detector_anomalias.py:35-49` |
| `fabrica_da_celula.perguntar` | Grava literalmente o argumento `dia.isoformat()` para **cada motivo**. | `app/chamados/services/fabrica_da_celula.py:118-124` |
| Julgamento → `perguntar_pela_celula` → `perguntar` | **DIA DA CÉLULA**: fornece `cel.data`. | `app/chamados/services/fabrica_da_celula.py:143-149` |
| Command `disparar_perguntas_competencia`: universo da célula | **DIA DA CÉLULA**: monta `(c.colaborador, c.data, motivo)` e chama o mesmo `perguntar`. | `app/chamados/management/commands/disparar_perguntas_competencia.py:111-123`, `:276-289` |
| Mesmo command: universo “fio mudo” | **DIA DA CÉLULA DO CHAMADO**: `dia = ch.celula.data`, monta `(colab, dia, motivo)` e chama o mesmo `perguntar`. | `app/chamados/management/commands/disparar_perguntas_competencia.py:165-168`, `:225-226`, `:244`, `:276-289` |
| `criar_ou_atualizar_disputa` | **Não escolhe nem converte o dia**: entrega o contexto recebido ao nascimento e ao merge. | `app/chamados/services/disputa_emissao.py:598-603` |
| `_abrir_ou_agregar_disputa`: nascimento | Persiste integralmente `contexto_nascimento`. | `app/chamados/services/disputa_emissao.py:523-527` |
| `_abrir_ou_agregar_disputa`: disputa aberta | Para o mesmo motivo, copia contexto anterior e faz `merged.update(v)`: a `data` recebida **substitui** a anterior quando presente; agrega `qtd` e `ultima_em` e salva. | `app/chamados/services/disputa_emissao.py:479-501` |

O dia do detector é calculado assim: `data_saida_esperada` usa o dia local da **entrada**, consulta `ec.marcos_do_dia(d)`, calcula o fim encadeado e retorna **`fim.date()`**; se vínculo/template/marcos necessários faltarem, retorna `None`. O encadeamento acrescenta um dia enquanto o próximo marco for `<=` ao anterior — daí a saída noturna poder pertencer a D+1. (`app/escala/utils.py:920-931`, `:907-909`, `:891-903`.)

No detector, os demais contextos montados não incluem chave literal `data`: colisão escreve IDs/contagem; ordem inválida escreve ordens; posto desativado escreve ID/nome; sem posto escreve `{}`. O ramo férias passa `info` se for dict ou sua representação textual; a função atualmente chamada retorna `ChamadoColaborador`, não dict. (`app/chamados/detector_anomalias.py:63-66`, `:94-109`, `:113-117`; `app/ponto/utils.py:100-101`, `:103`, `:137`.)

`detectar_batida_fora_escala` produz `data_batida`, não a chave `data`; o contexto de desvio nem inclui campo de data. (`app/chamados/detector_anomalias.py:170-175`, `:215-222`, `:229-236`.)

`models.py` apenas reexporta `criar_ou_atualizar_disputa` e os outros emissores desse módulo; não há uma segunda implementação desse helper ali. (`app/chamados/models.py:2776-2778`.)

## 30/10 09:57 · sessao f5efc31e6b22 · - · - · origem=Ronald (tmux)

- **Janela atual do ramo `trabalhou`:** `j0` é `data` à meia-noite no fuso corrente; `j1=j0+2 dias`; lê `Batida` de `timestamp>=j0` e `<j1`, por `timestamp`. A consulta não exclui retratadas nem origens protegidas; o descarte das intocáveis só aparece na seleção posterior de candidatos a flip. (`app/ponto/services/veredito_celula.py:212-218`, `app/ponto/services/veredito_celula.py:239-245`)

- **Juiz compartilhado:** `grade_dia` chama `grade_espelho_janela(colaborador, batidas, data, data, dias=1, agora=agora)` e devolve os marcos e o primeiro dia. Essa função monta a grade e aplica o clip de piso/hoje; o serviço captura um único `timezone.now()` para o ato. (`app/ponto/services/simular_dia.py:5-8`, `app/escala/utils.py:2263-2279`, `app/ponto/services/veredito_celula.py:200`)

- **Folga e perfeição:** a recusa efetiva nesta revisão fica em **221–222**; 220 apenas calcula `marcos0,dd0`. Se `tipo_dia!='trabalho'`, devolve “a escala nao preve trabalho nesse dia (...) -- nada a materializar”. Independentemente disso, `dia_perfeito` retorna `False` para qualquer tipo diferente de `trabalho`, ou com órfãs; depois exige ausência de `missing` e compatibilidade de `tipo_real` com o marco. Portanto **nenhuma folga é perfeita segundo esse predicado atual**, mesmo E,S,E,S. (`app/ponto/services/veredito_celula.py:220-228`, `app/ponto/services/simular_dia.py:11-23`)

- **Como a grade preserva folga:** `CelulaDia.trabalha=False` significa folga; `EscalaColaborador.eh_dia_trabalho` devolve diretamente `cel.trabalha` quando há célula. No builder por turno, pula o dia quando essa autoridade devolve `False`; dia sem `cel_por_data` desce como `tipo_dia='folga'`, previsto 0 e células `None`. Batidas não consumidas entram como órfãs do dia; o overlay de `FolgaDia` mantém `tipo_dia='folga'` e limpa células/órfãs. (`app/escala/models.py:1654-1656`, `app/escala/models.py:1066-1073`, `app/escala/utils.py:1541-1549`, `app/escala/utils.py:1722-1745`, `app/escala/utils.py:1755-1777`, `app/escala/utils.py:1101-1120`)
  
  A escolha do builder depende de `GRADE_POR_TURNO`; o settings assume `True` por padrão, mas lê variável de ambiente. O valor efetivo de execução aqui é [não verificado]. (`app/escala/utils.py:2024-2030`, `app/config/settings/base.py:165`)

- **O final comum já existente:** após a lógica técnica, há `recompute_turnos(colaborador,data-1,data+1)`; erros sem ações causam rollback/retorno; passa por `fechar_o_fio_do_dia`; se existe célula, chama `cel.lavrar_veredito(via='admin',veredito=veredito,ator=autor)`; grava o `registrar_log` do despachante; retorna `ok=not erros`. (`app/ponto/services/veredito_celula.py:308`, `app/ponto/services/veredito_celula.py:321-337`)

  A lavra da célula preserva protestos no caminho admin; mesma entrada não salva nem emite evento; mudança escreve `veredito`, `veredito_via`, `veredito_em`, `protestos`, chama `salvar_e_emitir` e registra `evento('veredito_celula',...)` com ator e antes/depois. Ela não escreve `trabalha`. (`app/ponto/portas/celula.py:682-703`)

- **Competência aberta:** `flip_tipo` consulta `competencia_fechada` antes da inversão e recusa sua mensagem; o guard calcula a competência pela data local da **batida**, com o corte da empresa, e barra fechamento `aprovado` ou `exportado`. Ausência de fechamento ou outros status devolvem `''`; “aberta” tecnicamente significa não estar nos dois status bloqueadores. (`app/ponto/services/flip_batida.py:21-28`, `app/ponto/janelas.py:75-89`)

- **Caso #35892 / col502 / 10/10:** dados reais, quantidade/origens/tipos de batidas, célula e competência estão [não verificado]. No código atual, uma célula de folga ordinária que chega à grade como `folga` será recusada antes de flip/lavra; paridade par, por si, ainda não é um caminho implementado nesse ramo. (`app/ponto/services/veredito_celula.py:220-228`, `app/ponto/services/simular_dia.py:11-23`)

  Para a leitura de pré-requisitos do desenho pedido: comprovar a célula `trabalha=False`; comprovar as batidas elegíveis e sua quantidade/paridade [não verificado para esse dia]; para cada batida efetivamente flipada, o escritor exige não retratada, autor/request, tipo E/S e competência não bloqueada. A proteção de origem/humano pertence ao chamador do veredito, não a `flip_tipo`. (`app/escala/models.py:1654-1656`, `app/ponto/services/flip_batida.py:10-28`, `app/ponto/services/veredito_celula.py:239-245`)

## 30/10 09:57 · sessao 35757c0da578 · - · - · origem=Ronald (tmux)

1. **Oferta e caminho até o serviço**

- O juiz `resolver_dia_oferecido` exige `pode_ultimo_recurso`, `eh_trabalho`, `data < hoje` e status em `('falta', 'irregular', 'aberto', 'sem_certidao')`; portanto, nesse predicado atual, folga não oferece o botão. `app/colaboradores/services/calendario.py:188`, `app/colaboradores/services/calendario.py:191`, `app/colaboradores/services/calendario.py:196`.
- `pode_ultimo_recurso` exige data anterior ao início da competência atual e ausência de pergunta viva respondida; o início vem de `janela_atual`, e a vivacidade é consultada ao C6 `pergunta_viva`. `app/colaboradores/services/calendario.py:569`, `app/colaboradores/services/calendario.py:574`, `app/colaboradores/services/calendario.py:576`, `app/colaboradores/services/calendario.py:578`, `app/colaboradores/services/calendario.py:874`.
- `contexto_calendario` carimba o campo em cada dia chamando o juiz; a porta curta `resolver_dia_oferecido_na_data` monta o calendário de um dia e lê esse mesmo campo. `app/colaboradores/services/calendario.py:200`, `app/colaboradores/services/calendario.py:205`, `app/colaboradores/services/calendario.py:208`, `app/colaboradores/services/calendario.py:902`, `app/colaboradores/services/calendario.py:905`.
- A view `calendario_ponto` exige `ver_cadastro`, chama `contexto_calendario(colaborador, request.GET)`, acrescenta `pode_veredito=True` quando há `regularizar_batida`, fornece tipos de ausência, motivos e URL de retorno, e renderiza `calendario_partial.html`. `app/colaboradores/views.py:1935`, `app/colaboradores/views.py:1937`, `app/colaboradores/views.py:1945`, `app/colaboradores/views.py:1946`, `app/colaboradores/views.py:1948`, `app/colaboradores/views.py:1957`, `app/colaboradores/views.py:1962`.
- A rota do calendário é `<int:cid>/calendario/`, sob `/colaboradores/`; o partial inclui `_calendario_grade.html`. `app/colaboradores/urls.py:24`, `app/config/urls.py:26`, `app/templates/colaboradores/calendario_partial.html:62`.
- A grade só inclui o popover quando `pode_veredito and dia.resolver_dia_oferecido`; passa `colab_pk`, `data`, `volta`. `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/templates/colaboradores/partials/_calendario_grade.html:126`.
- **Oferta do botão** é decisão do juiz; **opções dentro do popover** são fixadas no template: trabalhou, faltou, justificado e “Cobrar este dia” (esta última tem outro `formaction`). O texto atual de trabalhou é “Trabalhou no horário da escala”. `app/templates/ponto/partials/_veredito_dia.html:21`, `app/templates/ponto/partials/_veredito_dia.html:22`, `app/templates/ponto/partials/_veredito_dia.html:23`, `app/templates/ponto/partials/_veredito_dia.html:25`.
- O formulário faz POST para `ponto:veredito_celula`; campos ocultos `colab`, `data` em ISO e `volta`; o clique envia `veredito=trabalhou`. `app/templates/ponto/partials/_veredito_dia.html:17`, `app/templates/ponto/partials/_veredito_dia.html:19`, `app/templates/ponto/partials/_veredito_dia.html:21`.
- A rota efetiva é `/ponto/celula/veredito/` → `views.veredito_celula_view`. `app/config/urls.py:29`, `app/ponto/urls.py:58`.
- A view exige POST e `regularizar_batida`; lê colaborador, `datas` ou `data`, valida ISO e veredito, percorre datas únicas ordenadas e chama `_vc(colaborador=colab, data=data, veredito=veredito, autor=request.user, tipo_ausencia=... or None, request=request)`. `app/ponto/views.py:3302`, `app/ponto/views.py:3305`, `app/ponto/views.py:3308`, `app/ponto/views.py:3309`, `app/ponto/views.py:3313`, `app/ponto/views.py:3318`, `app/ponto/views.py:3329`, `app/ponto/views.py:3330`.
- Se houver sucesso, a view ainda chama `reconciliar_apos_vinculo(... autor=request.user, motivo='resolver_dia')`; devolve resposta individual ou de lote, incluindo motivos das recusas. `app/ponto/views.py:3339`, `app/ponto/views.py:3341`, `app/ponto/views.py:3346`, `app/ponto/views.py:3353`, `app/ponto/views.py:3354`, `app/ponto/views.py:3360`.

5. **Corte de 26/10**

- Não localizei `PAUSA-BATIDA-NA-FOLGA-SEM-TIPO` em `app/docs/CORTES.md` nem `app/docs/LEIS.md`; não há linha desses dois arquivos que eu possa atribuir ao texto solicitado. **[não verificado nesses dois documentos]**
- O texto encontrado está em `app/docs/PROMPTS.md:1483`:

> `26/10 11:0x | **aval: PAUSA-BATIDA-NA-FOLGA-SEM-TIPO = nem (a) nem (b)** -- sem marco o tipo e o gravado (decidir_tipo_estrito), E,E,S,S = dono BATIDA, lista do admin; os 92 pares em dia de trabalho viram linha da fila 1 com o censo, sem cura antes. Estado: BACKLOG R1-FLIP-NAO-VIROU.`

- Há reprodução da resposta em `app/docs/RELATO.md:577`: “**RESPONDIDA 26/10 11:0x: PAUSA-BATIDA-NA-FOLGA-SEM-TIPO = nem (a) nem (b)** -- folga sem marco = BATIDA, lista do admin; pousada.” A mesma linha conserva riscada a pergunta anterior com o caso col369 12/10, batidas E,E,S,S. `app/docs/RELATO.md:577`.

## 30/10 09:53 · sessao 3c1b129f7b2c · O261-FIO-MORTO · sonda · origem=Code

Sim: `2026-10-25` aparece com código `C9`, dono `BATIDA`, dono_do_dia `BATIDA` e ação `Regularizar a saida do dia`, no dado fornecido. [nao verificado]

```text
CXR129 codigo=C2 data=None dono=cadastro (lido agora) dono_do_dia=None acao=Revisar a jornada e os marcos do template [nao verificado]
CXR129 codigo=C9 data=2026-08-23 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-09-06 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-09-07 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-09-20 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-10-11 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=C9 data=2026-10-25 dono=BATIDA dono_do_dia=BATIDA acao=Regularizar a saida do dia [nao verificado]
CXR129 codigo=A5 data=None dono=cadastro dono_do_dia=None acao=Revisar os horarios do template [nao verificado]
CXR129 codigo=A7 data=None dono=cadastro dono_do_dia=None acao=Revisar os horarios do template [nao verificado]
CXR129_total=9 [nao verificado]
```

## 30/10 09:16 · sessao 9c7726ff84d8 · O261-VETO-NA-PORTA · censo · origem=Code

**Dos 30 caminhos das três tabelas de nascimento/reabertura do anexo, 29 passam por pelo menos uma das três portas; 1 escreve por fora.** As escritas convergem nos quatro pontos abaixo. Contagem por caminho, detalhada na tabela; não é contagem de ocorrências em produção. (`app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`)

| Sigla usada nas tabelas | Linha da escrita final |
|---|---|
| **A** — `ChamadoColaborador.abrir` | `get_or_create`: `app/chamados/models.py:730` |
| **C** — `ChamadoColaborador.criar` | `objects.create`: `app/chamados/models.py:834` |
| **G** — `ChamadoColaborador._gravar_estado` | `save(update_fields=campos)`: `app/chamados/models.py:231` |
| **X** — por fora | `QuerySet.update(status_local='aberto')`: `app/chamados/migrations/0038_a3_registrado_backfill.py:15` |

Na coluna **A / C / G**, a ordem é `abrir / criar / _gravar_estado`. **Sim*** indica passagem pelo ramo de reabertura: `abrir → _renascer_por_premissa_viva → reabrir → _gravar_estado`. (`app/chamados/models.py:686`, `app/chamados/models.py:770`, `app/chamados/models.py:1089`)

A marca **“copia de veto”** identifica o sítio que mantém bloqueio próprio de régua/célula antes de pedir a escrita. As delegações que atravessam esse sítio estão identificadas como herança. (`app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:626`, `app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:494`, `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:854`)

| Nº | Caminho — arquivo::função | Chamada que conduz à escrita | Escrita final | A / C / G | Marca |
|---:|---|---|---|---|---|
| 1 | `app/chamados/models.py::ChamadoColaborador.abrir` — nascimento | `app/chamados/models.py:730` | **A** | sim / não / não | Veto dentro da própria porta: `app/chamados/models.py:726`. |
| 2 | `app/chamados/models.py::ChamadoColaborador.criar` | `app/chamados/models.py:834` | **C** | não / sim / não | Veto dentro da própria porta: `app/chamados/models.py:822`. |
| 3 | `app/chamados/models.py::ChamadoColaborador.abrir` — dedup encerrado | Renascimento em `app/chamados/models.py:686`; `reabrir` em `app/chamados/models.py:770` | **G** | sim / não / sim | Retorna antes do veto de nascimento: `app/chamados/models.py:688`, `app/chamados/models.py:726`. |
| 4 | `app/chamados/models.py::ChamadoColaborador.reabrir` | `_gravar_estado` em `app/chamados/models.py:1089` | **G** | não / não / sim | Consulta da própria porta: `app/chamados/models.py:1050`. |
| 5 | `app/chamados/models.py::ChamadoColaborador.transicionar` — destino vivo | `_aplicar` em `app/chamados/models.py:183` → `_gravar_estado` em `app/chamados/models.py:260` | **G** | não / não / sim | Guardas de premissa do corpo são para resolução: `app/chamados/models.py:165`. |
| 6 | `app/ponto/management/commands/detectar_ausencias.py::Command.handle → _abrir_chamado` | `app/ponto/management/commands/detectar_ausencias.py:439` → `abrir` em `app/ponto/management/commands/detectar_ausencias.py:688` | **A/G** | sim / não / sim* | **copia de veto**: régua em `app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/detectar_ausencias.py:604`; célula em `app/ponto/management/commands/detectar_ausencias.py:626`. |
| 7 | `app/ponto/management/commands/emitir_furo_retroativo.py::Command.handle` | Detector em `app/ponto/management/commands/emitir_furo_retroativo.py:142` → caminho 6 | **A/G** | sim / não / sim* | Herda a cópia do detector: `app/ponto/management/commands/emitir_furo_retroativo.py:142`. |
| 8 | `app/ponto/management/commands/reconciliar_grade.py::Command.handle → _emitir_furo` | `abrir` em `app/ponto/management/commands/reconciliar_grade.py:499`; `reabrir` adicional em `app/ponto/management/commands/reconciliar_grade.py:520` | **A/G** | sim / não / sim* | **copia de veto**: régua em `app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:491`; célula em `app/ponto/management/commands/reconciliar_grade.py:494`. |
| 9 | `app/ponto/services/cartorio.py::_julgar_colab_corpo` — integral/morta/limbo | Detector em `app/ponto/services/cartorio.py:1358` → caminho 6 | **A/G** | sim / não / sim* | Herda bloqueio do detector. Consulta própria para impressão: `app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`. |
| 10 | `app/ponto/services/cartorio.py::_julgar_colab_corpo` — parcial | Detector em `app/ponto/services/cartorio.py:1394` → caminho 6 | **A/G** | sim / não / sim* | Herda bloqueio do detector. Mesma consulta para impressão: `app/ponto/services/cartorio.py:1083`. |
| 11 | `app/chamados/reconciliador.py::reemitir_chamado_do_dia` — c2 | `abrir` em `app/chamados/reconciliador.py:862` | **A/G** | sim / não / sim* | **copia de veto**: célula em `app/chamados/reconciliador.py:838`; régua em `app/chamados/reconciliador.py:854`, condicionada por `app/chamados/reconciliador.py:852`. |
| 12 | `app/ponto/management/commands/processar_alertas_turno.py::Command.handle → _marco_critico → _abrir_chamado_critico` | `app/ponto/management/commands/processar_alertas_turno.py:141` → `app/ponto/management/commands/processar_alertas_turno.py:316` → `abrir` em `app/ponto/management/commands/processar_alertas_turno.py:335` | **A/G** | sim / não / sim* | Consulta compartilhada pela porta: `app/chamados/models.py:726`. |
| 13 | `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `criar` em `app/ponto/management/commands/processar_alertas_avancados.py:121` | **C** | não / sim / não | Consulta compartilhada pela porta: `app/chamados/models.py:822`. |
| 14 | `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `abrir` em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:146` | **A/G** | sim / não / sim* | Consulta antecipada somente no dry: `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:140`. |
| 15 | `app/ponto/views.py::bater_ponto` | `_Chamado.criar` em `app/ponto/views.py:291` | **C** | não / sim / não | Condição própria de entrada bloqueada e dedup: `app/ponto/views.py:275`, `app/ponto/views.py:281`. |
| 16 | `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | `app/chamados/views.py:197` → `criar` em `app/chamados/services/abertura.py:47` | **C** | não / sim / não | Consulta compartilhada pela porta: `app/chamados/models.py:822`. |
| 17 | `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | `app/chamados/views.py:1053` → renascimento em `app/chamados/services/resposta_tardia.py:45` | **G** | não / não / sim | Seleção própria por `VEREDITOS_ACUSAM`: `app/chamados/services/resposta_tardia.py:45`. |
| 18 | `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | `app/chamados/views.py:547` → `transicionar` em `app/chamados/services/acoes_chamado.py:315` | **G** | não / não / sim | Passa pelo escritor da transição: `app/chamados/models.py:260`. |
| 19 | `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | `app/chamados/views.py:674`, `app/chamados/views.py:954` → `transicionar` em `app/chamados/services/fio.py:715` | **G** | não / não / sim | Passa pelo escritor da transição: `app/chamados/models.py:260`. |
| 20 | `app/chamados/views_arquivar.py::desarquivar_chamado` | `reabrir` em `app/chamados/views_arquivar.py:68` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 21 | `app/ponto/services/fechamento.py::reabrir_periodo → app/chamados/services/competencia_trancada.py::reabrir` | `app/ponto/services/fechamento.py:1416` → `ch.reabrir` em `app/chamados/services/competencia_trancada.py:258` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 22 | `app/chamados/services/fio.py::processar_post_fio → app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `app/chamados/services/fio.py:577` → `ch.reabrir` em `app/chamados/services/acoes_disputa.py:266` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 23 | `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `_pai.reabrir` em `app/chamados/services/disputa_emissao.py:1606` | **G** | não / não / sim | Guarda compartilhada de reabertura: `app/chamados/models.py:1050`. |
| 24 | `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `pai.reabrir` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` | **G** | não / não / sim | Pergunta antecipadamente à mesma `recusa_da_reabertura`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/models.py:1050`. |
| 25 | `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` | `update` direto em `app/chamados/migrations/0038_a3_registrado_backfill.py:15` | **X** | não / não / não | **escritor a migrar**. |
| 26 | `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `abrir` em `app/chamados/services/disputa_emissao.py:504` | **A/G** | sim / não / sim* | Chamado-pai `disputa_supervisao`: `app/chamados/services/disputa_emissao.py:507`. |
| 27 | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `abrir` em `app/chamados/services/disputa_emissao.py:908` | **A/G** | sim / não / sim* | Chamado-pai de módulo manual: `app/chamados/services/disputa_emissao.py:911`. |
| 28 | `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `abrir` em `app/chamados/services/disputa_emissao.py:1128` | **A/G** | sim / não / sim* | Recusa própria de âncora encerrada: `app/chamados/services/disputa_emissao.py:1109`. |
| 29 | `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | `app/chamados/views.py:136` → `criar` em `app/chamados/services/abertura.py:172` | **C** | não / sim / não | Validação própria de tipo/dia: `app/chamados/services/abertura.py:149`, `app/chamados/services/abertura.py:152`. |
| 30 | `app/chamados/services/disputa_emissao.py::gerar_perguntas_disputa → app/chamados/reconciliador.py::reatar_pergunta_orfa → reemitir_chamado_do_dia` | `app/chamados/services/disputa_emissao.py:409` → `app/chamados/reconciliador.py:1518` → `abrir` em `app/chamados/reconciliador.py:862` | **A/G** | sim / não / sim* | Herda a cópia do c2: `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:854`. |

As entradas convergentes do anexo ficam relacionadas abaixo aos caminhos numerados. Para a escrita delegada indicada, passam por **A: sim; C: não; G: sim***, com as linhas finais **A/G** definidas acima. (`app/chamados/models.py:730`, `app/chamados/models.py:686`, `app/chamados/models.py:770`, `app/chamados/models.py:1089`, `app/chamados/models.py:231`)

| Entrada — arquivo::função | Referência e convergência até os caminhos acima | Escrita final |
|---|---|---|
| `app/ponto/management/commands/processar_cartorio.py::Command.handle` | `app/ponto/management/commands/processar_cartorio.py:65` → corpo via `app/ponto/services/cartorio.py:744` → **9/10** | **A/G** |
| `app/ponto/services/cartorio.py::julgar_celula` | `julgar_colab` em `app/ponto/services/cartorio.py:1560`; emissor em `app/ponto/services/cartorio.py:1561` → **9/10** | **A/G** |
| `app/ponto/services/vigia_de_hora.py::rodar` | `julgar_celula` em `app/ponto/services/vigia_de_hora.py:130` → **9/10** | **A/G** |
| `app/chamados/views_cobrar.py::cobrar_dia_view → app/chamados/services/cobrar_dia.py::cobrar_dia` | `app/chamados/views_cobrar.py:325` → `julgar_celula` em `app/chamados/services/cobrar_dia.py:29` → **9/10** | **A/G** |
| `app/ponto/management/commands/cobrar_furos_sem_cobranca.py::Command.handle → app/ponto/services/furos_sem_cobranca.py::cobrar` | `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30` → `julgar_celula` em `app/ponto/services/furos_sem_cobranca.py:128` → **9/10** | **A/G** |
| `app/ponto/signals.py::_julgar_dias` | `julgar_celula` em `app/ponto/signals.py:93` → **9/10** | **A/G** |
| `app/chamados/signals.py::_cartorio_post_save_chamado` | `julgar_celula` em `app/chamados/signals.py:180` → **9/10**; receiver posterior ao save original: `app/chamados/signals.py:164` | **A/G** |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | `julgar_celula` em `app/ponto/portas/celula.py:372` → **9/10** | **A/G** |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `julgar_celula` em `app/colaboradores/services/pos_vinculo.py:72` → **9/10** | **A/G** |
| `app/escala/regua_defesa.py::rejulgar_furos_do_vinculo` | `julgar_colab` em `app/escala/regua_defesa.py:692` → **9/10** | **A/G** |
| Commands de rejulgamento | `app/ponto/management/commands/celula_veredito_velho.py::Command.handle`, chamada em `app/ponto/management/commands/celula_veredito_velho.py:144`; `app/ponto/management/commands/curar_residuo_bug115.py::Command._lavrar`, chamada em `app/ponto/management/commands/curar_residuo_bug115.py:143`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py::Command.handle`, chamada em `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:71` → **9/10** | **A/G** |
| Scripts fora de `tests/` | `bin/rejulgar_o118_col221.py::<módulo>`, chamada em `bin/rejulgar_o118_col221.py:24`; `bin/r5_idempotencia_frota.py::rodada`, chamada em `bin/r5_idempotencia_frota.py:109` → **9/10** | **A/G** |
| `app/chamados/reconciliador.py::_ctx_c2` | `julgar_celula` em `app/chamados/reconciliador.py:671` → **9/10** | **A/G** |
| Reconciliação por command/signal | `app/chamados/management/commands/reconciliar_chamados.py::Command.handle`, chamada em `app/chamados/management/commands/reconciliar_chamados.py:52`; `app/ponto/signals.py::_reconciliador_post_save_batida`, chamada em `app/ponto/signals.py:215` → cadeia em `app/chamados/reconciliador.py:1348`, `app/chamados/reconciliador.py:1423`, `app/chamados/reconciliador.py:1518` → **11** | **A/G** |
| `app/ponto/services/cartorio.py::julgar_colab → app/chamados/services/fabrica_da_celula.py::perguntar_pela_celula → perguntar` | `app/ponto/services/cartorio.py:770` → `app/chamados/services/fabrica_da_celula.py:149` → chamadas em `app/chamados/services/fabrica_da_celula.py:122`, `app/chamados/services/fabrica_da_celula.py:124` → **26/30** | **A/G** |
| `app/chamados/management/commands/disparar_perguntas_competencia.py::_rodar` | Fábrica em `app/chamados/management/commands/disparar_perguntas_competencia.py:288` → **26/30**, pelas chamadas em `app/chamados/services/fabrica_da_celula.py:122`, `app/chamados/services/fabrica_da_celula.py:124` | **A/G** |
| `app/api/views.py::api_bater_ponto` e `app/api/views_core.py::api_bater_ponto` | `app/api/views.py:778`, `app/api/views_core.py:778` → núcleo/perguntas em `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:606`, `app/chamados/services/disputa_emissao.py:618` → **26/30** | **A/G** |
| `app/ponto/services/triagem_batida.py::ramo_registrar_es` | `app/ponto/services/triagem_batida.py:651` → mesma cadeia de disputa/perguntas → **26/30**, via `app/chamados/services/disputa_emissao.py:598`, `app/chamados/services/disputa_emissao.py:606` | **A/G** |
| `app/chamados/services/declaracao_texto.py::declarar_hora_colab/aplicar_declaracao` | `app/chamados/services/declaracao_texto.py:307`, `app/chamados/services/declaracao_texto.py:605` → núcleo em `app/chamados/services/disputa_emissao.py:563` → **26** | **A/G** |
| Outros chamadores do agrupador de órfãos | `app/chamados/services/acoes_disputa.py::acao_iniciar_disputa_orfaos`, chamada em `app/chamados/services/acoes_disputa.py:221`; `app/chamados/services/pergunta_no_app.py::perguntar`, chamada em `app/chamados/services/pergunta_no_app.py:159`; `app/chamados/services/cobrar_dia.py::cobrar_dia`, chamada em `app/chamados/services/cobrar_dia.py:44`; `app/ponto/management/commands/processar_alertas_turno.py::_abrir_chamado_critico`, chamada em `app/ponto/management/commands/processar_alertas_turno.py:384`; `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle`, chamada em `app/ponto/management/commands/processar_alertas_avancados.py:158`; `app/ponto/management/commands/detectar_ausencias.py::_abrir_chamado`, chamada em `app/ponto/management/commands/detectar_ausencias.py:791` → **27** | **A/G** |
| Outros chamadores do núcleo | `app/ponto/management/commands/reconciliar_grade.py::_disputa`, chamada em `app/ponto/management/commands/reconciliar_grade.py:416`; `app/chamados/management/commands/smoke_chamados.py::_bloco_a_motivos`, chamada em `app/chamados/management/commands/smoke_chamados.py:99` → **26** | **A/G** |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py::Command.handle` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` → núcleo em `app/chamados/services/disputa_emissao.py:598` → **26** | **A/G** |

Para as marcas, há estas distinções verificadas:

| Consulta anterior | Classificação |
|---|---|
| Cartório: `motivo_do_veto` em `app/ponto/services/cartorio.py:1013` | Alimenta a impressão em `app/ponto/services/cartorio.py:1083`; sua função de detectar queda do veto está documentada em `app/ponto/services/furos_vetados.py:22`. Não marcada como cópia de bloqueio. |
| Passivo: régua/célula em `app/ponto/services/furos_sem_cobranca.py:96`, `app/ponto/services/furos_sem_cobranca.py:98` | Classifica a medição; a cobrança filtra essa classificação em `app/ponto/services/furos_sem_cobranca.py:125`. Que essa leitura se torne apagável pela mudança da porta: **[nao verificado]**. |
| Retroativo de turno: `celula_absolveria` em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:134` | Consulta dry que termina sem emissão em `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:140`; reutiliza o juiz da porta em `app/chamados/models.py:858`. |
| Órfãs enterradas: `recusa_da_reabertura` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:105` | Reutiliza a mesma função da porta, chamada em `app/chamados/models.py:1050`; não copia suas guardas, conforme `app/chamados/models.py:993`. |
| Resposta tardia: teste de `VEREDITOS_ACUSAM` em `app/chamados/services/resposta_tardia.py:45` | Seleção própria de premissa. Equivalência com o veto de régua/célula a centralizar: **[nao verificado]**. |

| Total — unidade explícita | Quantidade e referência |
|---|---|
| Caminhos das três tabelas principais do anexo | **30**, enumerados acima; entradas convergentes remetem a essas linhas. |
| Passam por pelo menos uma das três portas | **29** — linhas 1–24 e 26–30; escritas em `app/chamados/models.py:730`, `app/chamados/models.py:834`, `app/chamados/models.py:231`. |
| Escrevem por fora | **1** — linha 25, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`. |
| **copia de veto**, contando sítios próprios | **3** — detector, grade e c2: `app/ponto/management/commands/detectar_ausencias.py:600`, `app/ponto/management/commands/reconciliar_grade.py:487`, `app/chamados/reconciliador.py:854`. |
| Caminhos principais que atravessam essas cópias, incluindo delegações | **7** — linhas 6, 7, 8, 9, 10, 11 e 30; delegações em `app/ponto/management/commands/emitir_furo_retroativo.py:142`, `app/ponto/services/cartorio.py:1358`, `app/ponto/services/cartorio.py:1394`, `app/chamados/services/disputa_emissao.py:409`. |
| **escritor a migrar** | **1** — reverso da migration 0038, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`. |
| Pontos físicos de escrita neste recorte | **4** — três internos às portas e o `update` externo: `app/cha

[truncado em 20000 de 20420 caracteres]

## 30/10 09:15 · sessao 56fb2aebdc3a · O261-NOTURNO · verificador · origem=Code

- **[P2] Falso negativo introduzido** — [app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166](/home/ronald/codex_ws/06d7ad061f6eb40555d3dafb6785a8d6067ffb7a/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:166): o novo filtro ignora templates válidos como `'no dia {data!s}'.format(data=x)` e `'no dia {data:%d/%m/%Y}'.format(data=x)`, pois `_tem_data` exige a substring exata `{data}`. Reproduzido em memória: antes retornavam `[1]`; agora retornam `[]`, permitindo formatação de data fora do produtor.

## 30/10 09:15 · sessao 44dd85543b31 · O261-VETO-NA-PORTA · leitura · origem=Code

**São relógios diferentes:** as **24 h** são idade da entrada do turno aberto; as **72 h** mudam a urgência. Já o emissor `turno_aberto` nasce em relação ao **fim previsto**, e as **4 h** pertencem à expiração do “turno vivo”. ([turnos.py:3002](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3002), [turnos.py:3013](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3013), [turnos.py:3020](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3020), [processar_alertas_turno.py:140](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:140), [turnos.py:2011](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2011))

1. **`processar_alertas_turno.py` → `turno_aberto`**

   O nascimento passa por `_marco_critico` quando o lembrete é **`pos_20`**, chegando a `ChamadoColaborador.abrir(modulo_origem='turno_aberto')`. Portanto depende do **fim previsto + 20 minutos**, não de entrada aberta há 24 h. ([processar_alertas_turno.py:138](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:138), [processar_alertas_turno.py:316](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:316), [processar_alertas_turno.py:335](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:335))

   Os números vêm de `LEMBRETES_PRE_MIN = {-15,-5}`, `LEMBRETES_POS_MIN = {5,10,20}` e tolerância **240 segundos**. A decisão usa distância absoluta até cada marco: `pos_20` vale entre **16 e 24 minutos depois do fim**, inclusive; depois dessa janela, esse marco não dispara. ([turnos.py:3030](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3030), [turnos.py:3038](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3038))

   O fim vem primeiro de **`tm.dt_fim_previsto`**. Na materialização, esse campo é calculado com o `TipoEscala` vigente na data e `janela_turno_de(te, data_turno)`, **sem `ec`**; esse caminho lê os marcos do template, incluindo override do dia. Se o campo estiver vazio, o comando consulta `escala_vigente` e chama `janela_turno_de(..., ec=ec)`, que lê `ec.marcos_do_dia`; esse método lê DNA da célula e, faltando marcos, cai no template. ([processar_alertas_turno.py:100](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:100), [nucleo.py:31](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/nucleo.py:31), [selecao_periodo.py:136](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/selecao_periodo.py:136), [selecao_periodo.py:151](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/selecao_periodo.py:151), [models.py:1094](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/escala/models.py:1094))

   **`ec=None`: depende do caminho.** Com `dt_fim_previsto=None`, o fallback executa `if not ec: continue`: **cala**. Com fim materializado preenchido, essa consulta é pulada; a consulta posterior `_ec_phe=None` também **não veta** o nascimento, pois só pula quando existe vínculo com `permite_hora_extra`. Logo não há veto universal por falta de vínculo nesse emissor. ([processar_alertas_turno.py:101](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:101), [processar_alertas_turno.py:107](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:107), [processar_alertas_turno.py:134](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:134))

   As **36 h** são alcance da busca, não prazo de nascimento. O `prazo_sla` gravado também é outro relógio: `prazo_de_cobranca(agora)` acrescenta **20 minutos à emissão**. ([processar_alertas_turno.py:51](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:51), [processar_alertas_turno.py:347](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_turno.py:347), [motor.py:45](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/catalogo/motor.py:45))

2. **`processar_alertas_avancados.py` → `turno_aberto_24h`**

   Examina **ontem**, exige vínculo e dia de trabalho, recalcula a materialização e pergunta `turno_aberto_ha`. Esse juiz exige turno aberto com entrada e retorna literalmente **`(agora - entrada.timestamp) / 3600`**: não desconta jornada prevista nem conta desde a saída prevista. ([processar_alertas_avancados.py:22](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:22), [processar_alertas_avancados.py:49](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:49), [processar_alertas_avancados.py:57](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:57), [turnos.py:3000](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3000))

   Os três números são constantes do juiz: **8 h**, **24 h**, **72 h**. N3 exige fim previsto vencido e idade `<24`; sem fim previsto, substitui o vencimento por idade `>=8`. N5 exige **idade `>=24`, mesmo sem ter vencido o fim previsto**. As 72 h só escolhem `emergencia` em vez de `urgente`. ([turnos.py:2987](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2987), [turnos.py:3010](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3010), [turnos.py:3020](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:3020))

   O fim previsto passado ao N3 vem de `janela_turno_de(tipo_escala, ontem, ec=ec)`. N3 envia push/alerta; o nascimento do chamado fica no ramo N5, via `ChamadoColaborador.criar`. Se encontrar chamado anterior, atualiza o existente. ([processar_alertas_avancados.py:71](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:71), [processar_alertas_avancados.py:81](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:81), [processar_alertas_avancados.py:100](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:100), [processar_alertas_avancados.py:121](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:121))

   **`ec=None`: cala imediatamente**, antes de calcular idade ou acessar `ec.tipo_escala`; não erra por desreferenciar esse `None`. ([processar_alertas_avancados.py:37](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/processar_alertas_avancados.py:37))

3. **`emitir_turno_aberto_retroativo.py` → `turno_aberto_24h`**

   Usa **constante local `HORAS_MIN=24`**, sem chamar o juiz de idade: calcula horas desde a entrada e pula se `<24`. O fato vem de `turnos_do_colab`, exigindo turno aberto e entrada. ([emitir_turno_aberto_retroativo.py:48](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:48), [emitir_turno_aberto_retroativo.py:88](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88), [emitir_turno_aberto_retroativo.py:100](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:100))

   O fim previsto só é lido **depois dos filtros**, para compor a descrição; não decide prazo. A urgência nasce fixa em `urgente`, inclusive para entradas com mais de 72 h. **`ec=None`: incrementa `sem_vinculo` e cala.** ([emitir_turno_aberto_retroativo.py:104](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104), [emitir_turno_aberto_retroativo.py:141](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:141), [emitir_turno_aberto_retroativo.py:152](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:152), [emitir_turno_aberto_retroativo.py:162](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:162))

   Exclui hoje pelo recorte de `fim`; por padrão apenas simula, e o nascimento via `abrir` depende de `--apply`. ([emitir_turno_aberto_retroativo.py:60](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:60), [emitir_turno_aberto_retroativo.py:73](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:73), [emitir_turno_aberto_retroativo.py:131](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:131))

**As 4 h de `turno_aberto_de`:** são o default `margem_horas=4` de `turno_aberto_vivo`/`expiracao_da_aberta`. A aberta deixa de ser viva quando **`agora > fim_previsto + margem`**. A janela é consultada **sem `ec`**. Sem vínculo, `te=None`; sem tipo ou sem fim previsto, essa função não expira a aberta por esse relógio. A carga de `turno_aberto_de` continua limitada às batidas de hoje−2 até hoje. ([turnos.py:1981](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:1981), [turnos.py:1991](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:1991), [turnos.py:2005](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2005), [turnos.py:2182](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2182), [turnos.py:2205](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/turnos.py:2205))

**Outro emissor encontrado, de módulo irmão:** o POST de entrada pode criar `entrada_bloqueada_turno_aberto`, com categoria `turno_aberto`. Sua condição temporal é **14 ≤ horas < 96**, constantes locais, sobre idade da entrada arredondada para uma casa decimal. Sem vínculo, a coleta continua com marcos `None`; a ausência de vínculo não cala esse emissor. ([views.py:275](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/views.py:275), [views.py:291](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/views.py:291), [triagem_batida.py:17](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:17), [triagem_batida.py:28](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:28), [triagem_batida.py:149](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:149), [triagem_batida.py:178](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:178), [triagem_batida.py:188](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/ponto/services/triagem_batida.py:188))

**Passar do prazo ainda não garante nascimento:** as portas `abrir` e `criar` podem recusar chamado de turno quando a célula o absolve; retornam respectivamente `(None, False)` e `None`. Isso é veto posterior ao relógio, distinto de `ec=None`. ([models.py:725](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:725), [models.py:821](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:821), [models.py:848](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:848))

## 30/10 09:14 · sessao 1a4581e339fa · - · - · origem=Ronald (tmux)

- **Não encontrei outro nascimento dos módulos exatos** além dos três comandos: os outros locais consultam chamados existentes (`app/ponto/views.py:392`, `app/ponto/management/commands/marcar_abandono_provisorio.py:64`); o `modulo = 'turno_aberto'` dinâmico pertence ao fechador `_autoresolve_f2_f4`, consulta `cobranca_viva_do_dia` e reconcilia o existente (`app/ponto/services/triagem_batida.py:522`, `app/ponto/services/triagem_batida.py:543`, `app/ponto/services/triagem_batida.py:589`, `app/ponto/services/triagem_batida.py:597`). A busca não constitui prova formal de inexistência de construções dinâmicas adicionais. [nao verificado]

- **As 4 h são expiração de presença viva**, não limiar de nascimento de `turno_aberto_24h`: `turno_aberto_vivo(..., margem_horas=4)` retorna `None` apenas quando `agora > expiracao`; `expiracao_da_aberta` calcula `fim previsto + margem_horas` (`app/ponto/turnos.py:1981`, `app/ponto/turnos.py:1991`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2011`).

- Essa expiração usa `janela_turno_de(tipo_escala, turno.data_turno)` **sem `ec`**; portanto lê `tipo_escala.marcos_do_dia` ou os campos crus `hora_inicio`/`hora_fim`, inclusive seu fallback de exceção (`app/ponto/turnos.py:2008`, `app/ponto/selecao_periodo.py:151`, `app/ponto/selecao_periodo.py:154`, `app/ponto/selecao_periodo.py:156`, `app/ponto/selecao_periodo.py:159`).

- `turno_aberto_de` consulta o vínculo de **hoje** e batidas entre hoje−2 e hoje; sem vínculo, `te=None`, pula leitura de marcos e ainda chama o pareador e `turno_aberto_vivo`; `expiracao_da_aberta` retorna `None` sem tipo, logo não o expira por esse relógio (`app/ponto/turnos.py:2179`, `app/ponto/turnos.py:2182`, `app/ponto/turnos.py:2186`, `app/ponto/turnos.py:2205`, `app/ponto/turnos.py:2213`, `app/ponto/turnos.py:2242`, `app/ponto/turnos.py:2005`).

- **Módulo irmão `entrada_bloqueada_turno_aberto`:** o POST de entrada cria-o quando `tem_bloqueio_turno` e não há regularização pendente (`app/ponto/views.py:275`, `app/ponto/views.py:291`, `app/ponto/views.py:299`). O prazo vem de constantes **14 e 96 h**; a decisão é `14 <= horas < 96` (`app/ponto/services/triagem_batida.py:17`, `app/ponto/services/triagem_batida.py:18`, `app/ponto/services/triagem_batida.py:28`). As horas são `agora − entrada`, arredondadas para uma casa antes da comparação (`app/ponto/services/triagem_batida.py:181`, `app/ponto/services/triagem_batida.py:188`, `app/ponto/services/triagem_batida.py:206`).

- Nesse irmão, **`ec=None` não cala a coleta**: `_mi_tri`/`_mb_tri` permanecem `None` e o código continua pareando e preenchendo `tem_bloqueio_turno` (`app/ponto/services/triagem_batida.py:149`, `app/ponto/services/triagem_batida.py:150`, `app/ponto/services/triagem_batida.py:151`, `app/ponto/services/triagem_batida.py:178`, `app/ponto/services/triagem_batida.py:206`). O aviso de cadastro “Sem escala” também não retorna: gera sinal e continua (`app/ponto/services/triagem_batida.py:92`, `app/ponto/services/triagem_batida.py:123`). A consulta `turno_aberto_de` controla outro campo do contexto e não o predicado de bloqueio (`app/ponto/services/triagem_batida.py:199`, `app/ponto/services/triagem_batida.py:200`, `app/ponto/services/triagem_batida.py:206`).

## 30/10 09:14 · sessao 5f4f8da95cc5 · O261-NOTURNO · verificador · origem=Code

- **[P2] Falso positivo introduzido:** `'Ola {nome}'.format(**{**{'nome': 'Ana'}})` retorna `'Ola Ana'`, mas o detector atual acusa `[1]`; antes retornava `[]`. O novo `all(...)` rejeita o desempacotamento interno sem considerar que o receptor literal não usa `{data}`. [app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170](/home/ronald/codex_ws/e8c314e0e07e3af5e4fce78592a704117d2b85c0/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:170)

Varredura executada em memória: **nenhum apontamento nos 787 arquivos elegíveis**, conforme o escopo do teste. [app/chamados/tests/test_noturno_le_o_dia_do_instante.py:197](/home/ronald/codex_ws/e8c314e0e07e3af5e4fce78592a704117d2b85c0/app/chamados/tests/test_noturno_le_o_dia_do_instante.py:197)

## 30/10 09:13 · sessao 3a9739218e03 · - · - · origem=Ronald (tmux)

| Caminho | Chamada à porta | Veto próprio no trecho |
|---|---|---|
| `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:504` | Não há consulta própria de régua/célula no núcleo; delega a abertura. (`app/chamados/services/disputa_emissao.py:419`, `app/chamados/services/disputa_emissao.py:504`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:908` | Filtra chamados órfãos regularizáveis, sem perguntar o veto de régua do novo pai. (`app/chamados/services/disputa_emissao.py:892`, `app/chamados/services/disputa_emissao.py:908`) |
| `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado` | `ChamadoColaborador.abrir`, `app/chamados/services/disputa_emissao.py:1128` | Recusa âncora encerrada; não pergunta veto de régua/célula do novo pai. (`app/chamados/services/disputa_emissao.py:1107`, `app/chamados/services/disputa_emissao.py:1109`, `app/chamados/services/disputa_emissao.py:1128`) |
| `app/chamados/views.py::_abrir_para_colaborador → app/chamados/services/abertura.py::abrir_chamado_para_colaborador` | `ChamadoColaborador.criar`, `app/chamados/services/abertura.py:172` | Validação de tipo/dia e dedup; não consulta própria de veto de régua. (`app/chamados/services/abertura.py:149`, `app/chamados/services/abertura.py:152`, `app/chamados/services/abertura.py:165`, `app/chamados/services/abertura.py:172`) |
| `app/chamados/services/disputa_emissao.py::gerar_perguntas_disputa → reconciliador::reatar_pergunta_orfa → reemitir_chamado_do_dia` | `reatar_pergunta_orfa` em `app/chamados/services/disputa_emissao.py:409` → c2 em `app/chamados/reconciliador.py:1518` → `abrir` em `app/chamados/reconciliador.py:862` | A consulta própria está no c2: célula em `app/chamados/reconciliador.py:838`; `motivo_do_veto` em `app/chamados/reconciliador.py:854`, condicionado por `not _costura_no_vivo` em `app/chamados/reconciliador.py:852`. A função `gerar_perguntas_disputa` delega. |

A fábrica também delega: `perguntar_pela_celula → perguntar` em `app/chamados/services/fabrica_da_celula.py:149`; `perguntar → criar_ou_atualizar_disputa` em `app/chamados/services/fabrica_da_celula.py:122`; perguntas/c2 pela chamada em `app/chamados/services/fabrica_da_celula.py:124`.

O passivo tem consulta própria na **medição**: `motivo_do_veto` em `app/ponto/services/furos_sem_cobranca.py:96` e `celula_veta_cobranca` em `app/ponto/services/furos_sem_cobranca.py:98`. A cobrança filtra a classificação e delega ao cartório em `app/ponto/services/furos_sem_cobranca.py:125` e `app/ponto/services/furos_sem_cobranca.py:128`. Não há base para declarar automaticamente essa medição apagável quando o veto entrar na porta. [nao verificado]

A referência literal da L-111 está em `app/docs/LEIS.md:137`: trata da função que perdeu o último chamador de produção; não permite concluir que todo leitor diagnóstico perde sua função quando a porta passar a vetar. (`app/docs/LEIS.md:137`)

## 30/10 09:13 · sessao f4b9c20ce21e · - · - · origem=Ronald (tmux)

`emitir_turno_aberto_retroativo` usa **idade da entrada**, com **vínculo vigente como requisito separado**: `HORAS_MIN = 24` é constante local; calcula `(agora - localtime(ts)).total_seconds()/3600` e pula somente se `<24`, portanto inclui exatamente 24 h. Não calcula 24 h a partir de duração ou fim previsto. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:48`, `:94`, `:100`, `:101`)

- A fonte do fato é `turnos_do_colab(c, ini, fim, celulas=cels)`; exige `t.aberto` e timestamp de entrada. Não chama `turno_aberto_ha`, `turno_aberto_de` ou `margem_horas` nesse caminho. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:68`, `:88`, `:92`, `:94`, `:100`)
- **`ec=None` cala**: consulta `escala_vigente(c,data)` depois do limiar; incrementa `sem_vinculo` e executa `continue`. Não há acesso a `ec.tipo_escala` nesse ramo. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `:105`, `:106`, `:107`)
- O pareador pode retornar turnos de dia sem dono: segmentos admitem `_dono=None`; `_marcos_de(None)` retorna `(None,None)` e o segmento segue para `parear_turnos`. Portanto o silêncio por vínculo é explícito no emissor, não impossibilidade de formar o turno. (`app/ponto/turnos.py:1890`, `:1932`, `:1933`, `:1938`, `:1953`, `:1965`, `:1973`)
- A leitura de previsto `hora_fim=(ec.marcos_do_dia(data) or [None,None])[1]` ocorre **depois** de limiar, vínculo, ausência e dedupe; exceção é engolida e resultado serve somente à descrição. Não decide prazo ou nascimento. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:141`, `:143`, `:144`, `:162`)
- Recorta `fim` para ontem se `fim>=hoje`; isso é exclusão de hoje, independente de idade da entrada. O intervalo `ini/fim` vem dos argumentos obrigatórios. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:56`, `:57`, `:70`, `:73`, `:74`)
- Carrega células com constantes `ini-2 dias` e `fim+2 dias`; são alcance de alimentação, não prazo de nascimento. `turnos_do_colab` carrega batidas via `janela_com_pad`, cuja implementação retorna `ini-2 dias,fim+1 dia`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `:87`; `app/ponto/turnos.py:1845`, `:1857`, `:1858`)
- Outros silêncios antes da porta: ausência cobrindo cobrança; `turno_aberto` VIVO do mesmo dia ou `turno_aberto_24h` NÃO FECHADO do mesmo dia. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`, `:118`, `:125`)
- Por padrão é dry-run; com `--apply` chama `ChamadoColaborador.abrir` com módulo/categoria `turno_aberto_24h`, urgência fixa `urgente`. A porta ainda pode vetar por célula absolvida; o comando contabiliza `_ch is None` como `recusado_celula`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:60`, `:131`, `:146`, `:147`, `:150`, `:152`, `:169`; `app/chamados/models.py:726`, `:729`)
- `escala_vigente` delega a `vinculo_do_dia`: geradora da célula fora de `data_inicio..data_fim` retorna `None`; sem geradora escolhe vínculo que cobre a data ou `None`. (`app/escala/servico_jornada.py:55`, `:57`, `:58`; `app/escala/alimentacao.py:237`, `:250`, `:251`, `:253`, `:254`)

## 30/10 09:13 · sessao 89d6d93ce23b · - · - · origem=Ronald (tmux)

| Caminho conferido | Chamada à porta | Escrita final | Consulta própria |
|---|---|---|---|
| `app/ponto/management/commands/detectar_ausencias.py::Command.handle → _abrir_chamado` | `handle:439` → `abrir:688` | `app/chamados/models.py:730`; se dedup encerrado, `:686` → `:770` → `:1089` → `:231` | **copia de veto**: régua em `detectar_ausencias.py:600` e `:604`; célula em `:626`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py::Command.handle` | Alias detector `:38`, delegação `:142` → detector `:688` | Mesmas escritas acima (`app/chamados/models.py:730`, `:231`) | Herda detector; chamada delegada em `emitir_furo_retroativo.py:142`. Não há outro gate de régua/célula no corpo lido (`:30`, `:109`, `:127`, `:142`). |
| `app/ponto/management/commands/reconciliar_grade.py::Command._emitir_furo` | `abrir:499`; também `reabrir:520` | `app/chamados/models.py:730` ou `:231` | **copia de veto**: régua em `reconciliar_grade.py:487`, `:491`; célula em `:494`. |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — integral/morta/limbo | Detector `:1358` → `detectar_ausencias.py:688` | `app/chamados/models.py:730` ou `:231` | Consulta própria a `motivo_do_veto` em `cartorio.py:1013`, usada na impressão em `:1083`; gate efetivo da emissão herdado do detector (`detectar_ausencias.py:600`, `:604`, `:626`). |
| `app/ponto/services/cartorio.py::_julgar_colab_corpo` — parcial | Detector `:1394` → `detectar_ausencias.py:688` | `app/chamados/models.py:730` ou `:231` | Mesma consulta para impressão (`cartorio.py:1013`, `:1083`); gate herdado do detector. |
| `app/chamados/reconciliador.py::reemitir_chamado_do_dia` — c2 | `abrir:862` | `app/chamados/models.py:730` ou `:231` | **copia de veto**: célula em `reconciliador.py:838`; régua em `:854`, condicionada a `not _costura_no_vivo` em `:852`. |
| `app/ponto/management/commands/processar_alertas_turno.py::Command.handle → _marco_critico → _abrir_chamado_critico` | `:141` → `:316` → `abrir:335` | `app/chamados/models.py:730` ou `:231` | Herda porta; `_marco_critico` verifica retorno em `processar_alertas_turno.py:317`; ausência própria em `:97`, hora extra em `:135`. |
| `app/ponto/management/commands/processar_alertas_avancados.py::Command.handle` | `criar:121` | `app/chamados/models.py:834` | Herda porta; ausência própria em `processar_alertas_avancados.py:116`. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py::Command.handle` | `abrir:146` | `app/chamados/models.py:730` ou `:231` | `celula_absolveria` é consulta **só dry** (`emitir_turno_aberto_retroativo.py:131`, `:134`, `:140`), reutiliza o juiz da porta (`app/chamados/models.py:855`, `:858`). |
| `app/ponto/views.py::bater_ponto` | `_Chamado.criar:291` | `app/chamados/models.py:834` | Dedup em `app/ponto/views.py:281`; contexto apenas `pendentes/total` em `:280`; porta depende de célula/módulo em `app/chamados/models.py:849`. |
| `app/chamados/views.py::abrir_chamado → app/chamados/services/abertura.py::abrir_chamado_local` | View `:197` → serviço `criar:47` | `app/chamados/models.py:834` | Herda porta; contexto do serviço em `app/chamados/services/abertura.py:58` não fornece dia. |

**11 caminhos pelas portas, nenhum escritor por fora neste subconjunto**: chamadas documentadas nas linhas acima; escritores comuns em `app/chamados/models.py:730`, `:834`, `:231`.

**Três sítios próprios de bloqueio duplicado**: detector, grade, c2 (`detectar_ausencias.py:600`, `reconciliar_grade.py:487`, `reconciliador.py:854`). Se a contagem for por caminho herdado, explicitar a unidade evita contar quatro vezes o mesmo detector (`emitir_furo_retroativo.py:142`, `cartorio.py:1358`, `:1394`).

A leitura de veto do cartório serve à impressão e ao rejulgamento quando o veto cai (`app/ponto/services/furos_vetados.py:22`, `cartorio.py:1083`); classificá-la como candidato automático a apagar não está demonstrado. A consulta dry reutiliza o juiz existente (`app/chamados/models.py:855`, `:858`).

L-111 determina apagar função que perdeu seu último chamador de produção; o texto está em `CLAUDE.md:341`.

## 30/10 09:13 · sessao 7cb56ed90292 · - · - · origem=Ronald (tmux)

| Caminho | Chamada intermediária | Escrita final | `abrir` / `criar` / `_gravar_estado` |
|---|---|---|---|
| `app/chamados/views.py::responder_chamado_colaborador → app/chamados/services/resposta_tardia.py::receber` | `views.py:1053` → `resposta_tardia.py:45` → `_renascer_por_premissa_viva` → `reabrir` em `models.py:770` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/views.py::detalhe_chamado_local → app/chamados/services/acoes_chamado.py::acao_resposta_livre` | `views.py:547` → `acoes_chamado.py:315` → `transicionar` → `_aplicar` em `models.py:183` | `_gravar_estado` em `app/chamados/models.py:260` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/views.py::fio_colaborador/modal_fio_colaborador → app/chamados/services/fio.py::processar_post_fio` | `views.py:674` / `views.py:954` → `fio.py:715` → `transicionar` → `_aplicar` em `models.py:183` | `_gravar_estado` em `app/chamados/models.py:260` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/views_arquivar.py::desarquivar_chamado` | `reabrir` em `app/chamados/views_arquivar.py:68` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/ponto/services/fechamento.py::reabrir_periodo → app/chamados/services/competencia_trancada.py::reabrir` | `fechamento.py:1416` → `competencia_trancada.py:258` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/services/fio.py::processar_post_fio → app/chamados/services/acoes_disputa.py::acao_reabrir_disputa_admin` | `fio.py:577` → `acoes_disputa.py:266` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/services/disputa_emissao.py::resolver_perguntas_por_chamado_resolvido` | `reabrir` em `app/chamados/services/disputa_emissao.py:1606` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle` | `reabrir` em `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` | `_gravar_estado` em `app/chamados/models.py:1089` → `save` em `app/chamados/models.py:231` | não / não / sim |
| `app/chamados/migrations/0038_a3_registrado_backfill.py::desfazer` | Nenhuma das três portas | `update(status_local='aberto')` em `app/chamados/migrations/0038_a3_registrado_backfill.py:15` | não / não / não — **escritor a migrar** |

Sobre as marcas:

- `resposta_tardia.receber` pergunta diretamente se a célula existe e seu veredito está em `VEREDITOS_ACUSAM`, antes de renascer: é seleção própria de premissa/célula; não é consulta a `motivo_do_veto`. (`app/chamados/services/resposta_tardia.py:44`, `app/chamados/services/resposta_tardia.py:45`)
- `reabrir_orfas_enterradas` consulta antecipadamente `pai.recusa_da_reabertura()`. É reutilização da mesma função chamada pela porta, não cópia das guardas; os comentários documentam explicitamente essa distinção. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/models.py:993`, `app/chamados/models.py:1048`, `app/chamados/models.py:1050`)
- As transições humanas chamam `_aplicar` e `_gravar_estado`; as guardas de disputa e premissa do corpo de `transicionar` atingem destinos encerrados/resolvido. (`app/chamados/models.py:142`, `app/chamados/models.py:165`, `app/chamados/models.py:183`, `app/chamados/models.py:260`)
- O reverso da migration não filtra módulo; muda todos os `registrado` para `aberto`. (`app/chamados/migrations/0038_a3_registrado_backfill.py:15`)

## 30/10 09:13 · sessao c38edef84295 · - · - · origem=Ronald (tmux)

- **`ec=None` cala antes de qualquer cálculo de prazo**: o vínculo é obtido para **ontem**, e `if not ec: continue` interrompe esse colaborador. Portanto esse caminho não nasce nem erra por desreferenciar `ec`. (`app/ponto/management/commands/processar_alertas_avancados.py:36-41`)

- O comando só examina `data_turno=ontem`, exige dia de trabalho do vínculo, recalcula materialização e lê a primeira linha de `TurnoMaterializado` desse dia. (`app/ponto/management/commands/processar_alertas_avancados.py:22`, `:49-59`)

- **A idade usada para N5 é tempo desde a entrada real**, sem subtrair duração prevista ou fim previsto: `turno_aberto_ha` exige linha existente, `aberto=True`, entrada existente e retorna `(agora - turno.entrada.timestamp).total_seconds() / 3600`. O comando consome esse resultado. (`app/ponto/turnos.py:2992-3002`; `app/ponto/management/commands/processar_alertas_avancados.py:62-66`)

- **24 h** é constante `TURNO_ABERTO_N5_H = 24`; a decisão é `elif horas >= TURNO_ABERTO_N5_H: return 'N5'`. Assim, o nível N5 independe de ter vencido o fim previsto. (`app/ponto/turnos.py:2988`, `:3010-3014`)

- **8 h** é constante `TURNO_ABERTO_N3_SEM_FIM_H = 8`, usada somente como substituição do fim previsto ausente. Com fim previsto, N3 exige `agora > fim_previsto`; em ambos os casos exige menos de 24 h. N3 envia push e registra `pos_50`; não cria chamado neste ramo. (`app/ponto/turnos.py:2987`, `:3010-3014`; `app/ponto/management/commands/processar_alertas_avancados.py:73-89`)

- O fim previsto passado ao juiz vem de `janela_turno_de(tipo_escala, ontem, ec=ec)`, que consulta `ec.marcos_do_dia(data_ref)`; se essa consulta lançar exceção, usa `tipo_escala.hora_inicio/hora_fim`; sem um desses marcos retorna `(None,None)`. Compõe data/hora e leva o fim para o dia seguinte se cruza meia-noite. (`app/ponto/management/commands/processar_alertas_avancados.py:70-72`; `app/ponto/selecao_periodo.py:136-150`)

- **72 h** é constante `TURNO_ABERTO_EMERGENCIA_H = 72`, usada apenas em `urgencia_turno_aberto`: emergência a partir de 72 h, urgente antes. Não é um segundo limiar de nascimento. (`app/ponto/turnos.py:2989`, `:3018-3020`; `app/ponto/management/commands/processar_alertas_avancados.py:119-130`)

- No N5, primeiro procura `turno_aberto` vivo **de ontem** ou `turno_aberto_24h` não fechado **sem filtro de dia**; se encontra, escala o existente. Se não encontra, cria `turno_aberto_24h` com `data_turno=ontem` e timestamp da entrada. (`app/ponto/management/commands/processar_alertas_avancados.py:100-109`, `:120-140`, `:165-174`)

- Além do vínculo e dia de trabalho, há silêncio por isenção, ausência de turno aberto/entrada, chamado existente silenciado e ausência que cobre o dia quando não existe chamado. (`app/ponto/management/commands/processar_alertas_avancados.py:31-39`, `:49-50`, `:64-65`, `:112-117`)

- O nascimento passa por **`ChamadoColaborador.criar`**, não `abrir`; a porta resolve célula e pode retornar `None` se ela absolve o fato. O emissor verifica `_novo is None` e continua antes do push. (`app/ponto/management/commands/processar_alertas_avancados.py:121`, `:141-144`; `app/chamados/models.py:805-825`, `:848-851`)

- Não há uso de `turno_aberto_de` nem `margem_horas` no cálculo deste emissor: ele chama `turno_aberto_ha`, `nivel_turno_aberto` e `urgencia_turno_aberto`. (`app/ponto/management/commands/processar_alertas_avancados.py:62-72`, `:119`)

_432 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
