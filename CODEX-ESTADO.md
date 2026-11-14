# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 18:36** · publicado em **14/11/2026 18:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 18:36 · sessao 15bbc60ce588 · - · - · origem=Ronald (tmux)

Autoridade recomendada: `fio.dia_do_turno_da_linha` em `chamados/services/fio.py:1654`, já usada pelo nascimento em `chamados/models.py:2936`. Ela delega episódio ao juiz do turno (`ponto/turnos.py:2927`) e marco ao dia da célula (`ponto/turnos.py:2939`).

A substituição pode montar a mesma instância não salva usada pelo nascimento, com `instancia_do_motivo` e `batida_origem_do_nascimento`, e chamar essa autoridade. A data explícita do contexto entra somente como chave `pedido`; retire a cascata grade → contexto → batida. Sem chave nem episódio, a autoridade devolve `None` (`chamados/services/fio.py:1687`). Motivo datado/hora deve retornar `InsumoAusente`, com trilha; não deve ganhar data civil por conversa (`chamados/services/disputa_emissao.py:505`). O chamador já publica causas de recusa (`chamados/services/disputa_emissao.py:661`).

Cuidados da eleição:

- `_marcos_de_intervalo` fornece os fatos explícitos do intervalo (`chamados/services/disputa_emissao.py:263`). `motivos_atendidos` **não inclui** `intervalo_ausente`; usar somente esse juiz perderia esses candidatos (`chamados/juizes.py:2147`).
- O S160 exclui chamado com pergunta não materializada (`chamados/services/disputa_emissao.py:224`); o eleitor de intervalo inclui chamados com pergunta existente (`chamados/services/fio.py:750`). A unificação precisa conservar essa inclusão para ser idempotente.
- O `None` do S160 permite HIST/c1/c2 no reconciliador (`chamados/reconciliador.py:1559`) e reatamento imediato de pergunta recém-criada (`chamados/services/disputa_emissao.py:541`). Com menor PK na ambiguidade, esses caminhos deixam de executar para múltiplos candidatos; continuam necessários quando não há candidato.
- As duas portas de declaração passam o chamado de origem para a emissão (`chamados/services/declaracao_texto.py:365`, `chamados/services/declaracao_texto.py:653`). A resposta deve ler o destino de `perg.chamado_destino`, sem nova eleição.

Casos úteis sem banco: autoridade devolver dia diferente da chave e texto/dedup usarem o dia devolvido; autoridade devolver `None` apesar de grade/contexto/batida disponíveis e nenhum fallback ser consultado; intervalo explícito vencer chamado genérico com PK menor; permutação de pedidos conservar destino. Execução desses testes: [nao verificado]. [nao verificado]

## 14/11 18:36 · sessao 96181d5931e8 · - · - · origem=Ronald (tmux)

Censo somente leitura, linhas da base antes da cura:

| Sítio | Predicado atual |
|---|---|
| `chamados/services/disputa_emissao.py:885` — `criar_ou_atualizar_disputa` | motivos novos; DS aberta relida; não exige pergunta viva pendente |
| `chamados/services/disputa_emissao.py:1283` — `notificar_complementos` | `notificar`, motivos, dona diferente da principal e DS aberta; sem C6 |
| `chamados/services/disputa_emissao.py:1555` — órfãos | `notificar`, motivos, qualquer pergunta existente, DS aberta; inclui respondida/morta |
| `chamados/services/disputa_emissao.py:1645` — âncora | após criar conversa/analisar; sem C6 |
| `chamados/services/disputa_emissao.py:1992` — recobrança | conta todas sem resposta, mas envia mesmo com zero |
| `chamados/services/disputa_emissao.py:2041` / `:2049` — driver | só ContextVar de silêncio; envia WebPush e FCM |
| `ponto/management/commands/alertar_disputas_pendentes.py:59` | DS aberta e `renovar_contexto` concedeu carimbo diário; sem pergunta |
| `chamados/services/acoes_disputa.py:109` — reabrir | existe pergunta corrigível e `_reabrir(qs)` rodou; sem C6 após ato |
| `chamados/services/acoes_disputa.py:288` — reabrir admin | admin autorizado, DS fechada reaberta e conversa criada; sem C6 |
| `chamados/views.py:1447` — reabrir questionário | `_reabrir2(qs)` rodou; sem C6 após ato |
| `chamados/services/validacao.py:640` — recusar resposta | motivo presente, pergunta existente, respondida e não validada; reabertura rodou; sem C6 |
| `chamados/services/cobranca.py:107` / `:112`, chamado em `:187` | `checar_cobranca` e `_pendentes` já usam `consulta_do_app`; prazo não correndo; envio fora do atomic local, mas pode herdar atomic externo |
| `chamados/views_cobrar.py:44` — individual | permissão, POST, área, throttle diário; nem vitalidade nem DS aberta |
| `chamados/views_cobrar.py:123` — massa | permissão, `particionar_verbos == cobrar`, vivos, área, folha alterável, throttle; sem consulta direta à lista do app |

Avisos de conclusão de disputa também encontrados: `chamados/services/acoes_disputa.py:150`, `:156` e `chamados/services/fio.py:681`, `:688`. São posteriores à materialização e anteriores até à verificação de resultado vazio; não exigem conclusão bem-sucedida nem C6. A regra literal “todo push ... por causa de disputa” os alcança; semanticamente são avisos de conclusão, não cobrança. [Interpretação; texto literal fornecido pelo usuário.]

A fonte adequada já existe: `consulta_do_app` em `chamados/services/pergunta_no_app.py:61`. Ela toma `perguntas_do_app` (`:45`), que chama o C6 `pergunta_viva` (`:58`), e admite pendentes somente com `respondida_em is None` e disputa aberta (`:71`). Para uma DS, filtrar essa lista por `p.disputa_id == disputa.pk` conserva a mesma decisão do app.

A cura deve envolver os envios de disputa num callback de `transaction.on_commit`, relendo `consulta_do_app` dentro do callback; guardar só um booleano antes do commit pode cobrar pergunta respondida/fechada no restante da transação. `push_colaborador` só decide existência de usuário (`chamados/push_utils.py:114`) e FCM só existência de token (`chamados/fcm_utils.py:101`): o gate pertence à porta da disputa.

Atenção ao driver compartilhado: `cobranca._tocar` também é usado para abertura genérica pelo DP em `chamados/services/abertura.py:204`; pôr gate de disputa diretamente nele perderia esse caso. O `cobrar_colaborador` é o chamador de disputa e já usa a lista correta (`chamados/services/cobranca.py:155`, `:163`, `:187`).

Exclusões identificadas pelo código: mensagens DP (`chamados/views.py:989`, `chamados/services/fio.py:726`, `chamados/services/acoes_chamado.py:310`), exceção autorizada (`chamados/services/acoes_chamado.py:72`), lote de regularização externa (`chamados/views_regularizacao.py:171`, `:178`), alertas de turno (`ponto/management/commands/processar_alertas_turno.py:267`), ausência/justificativa/cobertura (`ponto/services/ausencia.py:1088`, `ponto/services/justificativa.py:242`, `:310`, `ponto/services/cobertura.py:160`, `:171`). Não são envios causados por `DisputaSupervisao` nesses sítios. `push_supervisao`/`push_dp` têm outro destinatário, inclusive alerta agregado em `ponto/management/commands/alertar_disputas_pendentes.py:103`.

O texto falso de quantidade está em `chamados/services/acoes_disputa.py:232` (`len(orfaos)`, que conta chamados). Outro “Colaborador notificado” incondicional aparece em `:299`; ambos precisam depender do resultado real do envio, distinguindo agendamento de entrega.

## 14/11 18:21 · sessao ca67df423b4b · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita: após a união dos períodos, soma acima do previsto retira todos os marcos do dia, zera os minutos e registra `pago_mais_trabalhado_passa_do_previsto`. Igualdade continua pagando (`ponto/services/efeito_ausencia.py:141–150`).

**Q1.** O previsto lavrado vem da ata (`ponto/services/cartorio.py:739`). Extraí a leitura existente da grade para `minutos_previstos_da_celula`: ata primeiro, inclusive zero; fallback em `minutos_previstos_do_dia`, sem cálculo paralelo (`escala/services/leitor_celula.py:354–366`). Grade e guarda compartilham essa leitura (`escala/services/leitor_celula.py:431`; `ponto/services/efeito_ausencia.py:145`). Isso evita chamar a grade inteira, que já lê os efeitos da justificativa (`escala/services/leitor_celula.py:379–380`).

O trabalhado usado na soma é `realizado_do_dia`, sobre todos os turnos do dia; pares fechados, pausas e E solta seguem a autoridade existente (`ponto/turnos.py:707–795`). `trabalhado_do_turno` permanece na DOBRA 2 (`ponto/services/efeito_ausencia.py:129`).

**Q2.** A fila anterior não consumia cobertura nem `conserva`. Acrescentei `justificativas_para_revisao`, reunindo recusa por soma, cobertura vazia da DOBRA e motivos de conservação (`ponto/services/efeito_ausencia.py:154–178`). A fila agora oferece **Revisar cobertura** e mostra o motivo (`ponto/views.py:1738–1743`; `templates/ponto/fila_justificativas.html:60,117`).

**Q3.** Não existe tolerância cadastrada **com juiz aplicável** encontrada: os campos existem (`core/models.py:59–62`), mas estão declarados sem efeito no cálculo (`core/regua_cct.py:72–75`). A tolerância de HE é constante (`ponto/motor_calculo_v2.py:51`). **TOLERANCIA-NA-JUSTIFICATIVA: sem construção**; +1 minuto recusa (`ponto/tests/test_justificativa_soma_sem_banco.py:104`).

O diff e seus escritores/leitores ficaram assim:

| Arquivo::função / campo | Escrita ou mudança | Leitores |
|---|---|---|
| `efeito_ausencia.py::periodos_justificados` — `cobre`, `minutos` | União e recusa integral: `ponto/services/efeito_ausencia.py:138–150` | Efeito financeiro: `:46–47`; grade: `escala/utils.py:1251–1276` |
| Mesmo serviço — `conserva`, `motivo` | Conservação em `:134`; motivo da soma em `:150` | Revisão em `:172–176`; fila em `ponto/views.py:1740`; template em `templates/ponto/fila_justificativas.html:117` |
| `leitor_celula.py::minutos_previstos_da_celula` — previsto | Extração da leitura: `escala/services/leitor_celula.py:354–366`; ata escrita por `ponto/services/cartorio.py:739` | Grade em `:431`; guarda em `ponto/services/efeito_ausencia.py:145` |
| Trabalhado/realizado | Autoridade existente: `ponto/turnos.py:707–795`; transporte à ata: `ponto/services/cartorio.py:740` | Guarda em `ponto/services/efeito_ausencia.py:147–148`; grade em `escala/utils.py:2064` |
| Testes | S1–S6, propriedade, fontes e fila: `ponto/tests/test_justificativa_soma_sem_banco.py:83–272`; adaptação das bordas v2: `ponto/tests/test_justificativa_periodo_v2_sem_banco.py:135–183` | Runner de mutantes: `ponto/tests/mutantes_justificativa_soma.py:73` |

O crédito continua chegando ao motor pelo mapa existente (`ponto/services/fechamento.py:755`; `ponto/services/espelho.py:1134`; `relatorios/pdf_espelho.py:407`), lido em `ponto/motor_calculo_v2.py:2628–2634`.

Resultados locais, para revisão pelo Code/suite e opus final: [nao verificado]

- **RED antes da cura:** 14 testes, 10 falhas de asserção — [saída preservada](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_red.txt:127). [nao verificado]
- **GREEN:** 32 testes sem banco — [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_green.txt:35). [nao verificado]
- **Mutantes novos:** 16/16 mortos por asserção; o comando individual de cada um está na [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_mutantes.txt:1).
- **Regressão v2:** 21/21 mutantes mortos — [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_regressao_mutantes.txt:22).

Comandos do RED, GREEN e dos três mutantes exigidos, respectivamente (`ponto/tests/justificativa_soma_red.txt:127`; `ponto/tests/justificativa_soma_green.txt:35`; `ponto/tests/justificativa_soma_mutantes.txt:1–6`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco -v
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma sem_guarda
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma limite_exclusivo
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma so_turno_da_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 --todos
```

Três testes ORM foram escritos para a sessão executar; resultado **[nao verificado]** (`ponto/tests/test_justificativa_paga_o_periodo.py:325–369`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_justificativa_paga_o_periodo
```

Sem teto, desconto parcial ou tolerância nova na guarda (`ponto/services/efeito_ausencia.py:148–150`). Nenhum caminho proibido aparece entre os alterados; `git diff --check` passou (`ponto/tests/justificativa_soma_verificacao.txt:3,17`). Frota, DSR, TXT, banco produtivo e aplicação em competência: **[nao verificado]**.

`LEI-AKITA | origem=anexo justificativa_pela_soma.txt, Ronald 14/11 18:08 [nao verificado na copia] | testemunha=S1–S6 (ponto/tests/test_justificativa_soma_sem_banco.py:83–139) | RED=justificativa_soma_red.txt:129 | quem-mais-le=efeito/grade/fila (ponto/services/efeito_ausencia.py:46,172; escala/utils.py:1257; ponto/views.py:1740) | juizes novos=0 | padrao=Single Source of Truth / Aggregate Root (docs/GLOSSARIO.md:11; escala/services/leitor_celula.py:354–366)`

## 14/11 18:01 · sessao b161216b6cea · E3-V10-DEGRAU-DONO-E-PORTA · leitura · origem=Code

1. **No teste isolado de classe e, a leitura da v2 dá `fio='colab'` e `rota='porta_app'`.** A fixture cria pergunta sem resposta, sob chamado `resolvido`; sem decisões, a tarefa exige `responder/colaborador`. A porta do colaborador depende da pendência no app, sem exigir chamado na fila. ([test_e4_passivo.py:67](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:67), [tarefa_do_dia.py:142](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601), [tarefa_do_dia.py:625](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:625))

   Os relógios são diferentes:

   - A classe de testes congela **25/09/2026 às 12:00 UTC**, relógio sob o qual cria a disputa e a pergunta. `DisputaSupervisao.aberta_em` usa `auto_now_add`; `data_evento` da pergunta é explicitamente `DIA=01/09/2026`, não a data de criação. ([test_e4_passivo.py:26](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:26), [test_e4_passivo.py:34](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:34), [models.py:1803](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/models.py:1803), [test_e4_backfill.py:28](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:28))
   - `_rodar(aplicar=True)` congela **15/09/2026 às 12:00 UTC** durante o comando. O comando captura `timezone.localtime()` e, em apply, usa `agora.date()`, ignorando `--hoje`. Isso corresponde ao cabeçalho `15/09 09:00 −03` anexado. ([test_e4_backfill.py:98](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:98), [backfill_fio_morto_e4b.py:218](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:218))

   A pergunta criada sob o relógio de 25/09 continua carregada na consulta de 15/09: a carga seleciona perguntas por colaborador, `conversa=False` e `data_evento`, sem cortar pelo nascimento da disputa. O próprio contrato declara pergunta e identidade como estado atual. ([tarefa_do_dia.py:367](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:367), [tarefa_do_dia.py:392](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:392))

   **O predicado exato que o exclui do plano normal é `x[4] == 'sem_porta'`.** Com `porta_app`, o vigia contabiliza a porta e executa `continue`, antes de incrementar `fio_morto_alcance`; `_alcance` retém somente as linhas `sem_porta`. Portanto ele sai do subconjunto do backfill, não necessariamente de `alcance_universo`. O complemento de exportadas também não inclui competência cuja entrega seja `False`. ([vigia_fio.py:658](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:658), [vigia_fio.py:714](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:714), [backfill_fio_morto_e4b.py:76](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:76), [backfill_fio_morto_e4b.py:173](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:173))

   O mutante falha antes de testar a reabertura: ele espera erro contendo `ato=reabrir_credor`, mas a caracterização primeiro exige encontrar a linha do dia. ([test_e4_passivo.py:73](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:73), [test_e4_passivo.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:87))

2. **Os dois dias são 01/09 e 02/09/2026.** O primeiro é a célula-base, inicialmente sem pergunta, com entrada apagada e saída acesa. O segundo é `DIA+1`, com lâmpadas equivalentes e pergunta sem resposta sob chamado `resolvido`. ([test_e4_backfill.py:77](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:77), [test_e4_passivo.py:97](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:97))

   **Qual deles produziu `segue_sem_porta` na execução anexada: [não verificado].** O anexo fornece o agregado, sem as linhas `DIA` desse teste. O comando atribui `segue_sem_porta` quando o `cel_id` executado permanece no conjunto `sem_porta` do censo posterior; o agregado não identifica essa célula. ([backfill_fio_morto_e4b.py:368](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:368))

   Há uma incompatibilidade que a diferença de relógios, sozinha, **não explica**: pela leitura estática da fixture, a pergunta de 02/09 deve ter porta no app também em 25/09. C6 retorna viva pela célula `furo`; a partição do app considera pendente a pergunta sem resposta em disputa aberta; sem decisões, a tarefa exige responder e publica a porta. ([juizes.py:1448](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/juizes.py:1448), [pergunta_no_app.py:78](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/pergunta_no_app.py:78), [tarefa_do_dia.py:142](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601))

   Assim, não consigo derivar dessa cópia o contador inicial **2**, nem atribuir causalmente o residual a 02/09. O teste exige esse contador em 25/09 e depois compara o plano com ele; o estado adicional que faria essa sequência produzir o agregado anexado é **[não verificado]**. ([test_e4_passivo.py:110](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:110))

3. **Usam o mesmo juiz de porta, mas não a mesma carga nem sempre o mesmo instante.** O vigia abre uma carga em lote e lê `porta_da_tarefa`. O backfill obtém seu universo chamando esse vigia com o `agora` da borda; depois, para escolher atos, chama `tarefa_do_dia` sem fornecer carga, abrindo outra carga, individual. ([vigia_fio.py:647](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:647), [backfill_fio_morto_e4b.py:78](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:78), [backfill_fio_morto_e4b.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:87), [tarefa_do_dia.py:650](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:650))

   O censo posterior recebe **novo** `timezone.localtime()`, para enxergar os atos escritos. No teste misto, o censo externo usa 25/09, enquanto o comando roda em 15/09. ([backfill_fio_morto_e4b.py:350](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:350), [test_e4_passivo.py:110](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:110), [test_e4_backfill.py:103](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:103))

4. **A caracterização isolada “sem resposta sob terminal ⇒ precisa reabrir para ter alcance” perdeu essa premissa na v2:** quando a tarefa exige responder e a pergunta está pendente no app, já existe dono `colab` e porta `porta_app`. O vigia aceita essa porta antes de classificar qualquer causa e. ([tarefa_do_dia.py:577](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:577), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601), [vigia_fio.py:658](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:658))

   O ramo `e/pergunta_viva_sob_chamado_terminal` continua no código, mas só é alcançado depois de faltar porta e de não haver outro dono pelas leis consultadas. Sua presença no código não demonstra ocorrência no dado fornecido. ([vigia_fio.py:670](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:670), [vigia_fio.py:698](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:698))

   **Existe uma segunda avaliação no backfill para escolher reabertura.** Além de ler a tarefa, `ato_do_dia` percorre as perguntas e consulta `reabrir_credor_de_pergunta_viva`, independentemente das ações publicadas. Esse helper exige ausência de resposta, C6 vivo e âncora fora de `VIVOS`; não consulta `pendentes_app` e não recebe `agora`. Portanto são critérios diferentes: a tarefa pergunta por alcance atual; o helper pergunta pela elegibilidade de reabrir a âncora. Ambos usam C6, mas o helper o chama sem o horizonte temporal da tarefa. ([backfill_fio_morto_e4b.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:87), [disputa_emissao.py:1812](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/disputa_emissao.py:1812), [disputa_emissao.py:1828](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/disputa_emissao.py:1828))

Quanto aos campos, são resultados e variáveis locais nesses caminhos:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `fio`, `rota` | `_portas_da_tarefa`, no dicionário retornado. ([tarefa_do_dia.py:626](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:626)) | `porta_da_tarefa`; vigia conta rota e publica fio como papel. ([tarefa_do_dia.py:636](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:636), [vigia_fio.py:662](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:662)) |
| `fio_morto_alcance` | Vigia inicializa em zero e incrementa no caminho final sem porta. ([vigia_fio.py:578](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:578), [vigia_fio.py:714](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:714)) | Backfill lê antes/depois; teste compara ao tamanho do plano. ([backfill_fio_morto_e4b.py:239](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:239), [backfill_fio_morto_e4b.py:415](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:415), [test_e4_passivo.py:114](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:114)) |
| `plano` | Comando cria a lista, acrescenta os dias e atribui resultados após execução/medição. ([backfill_fio_morto_e4b.py:251](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:251), [backfill_fio_morto_e4b.py:371](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:371)) | O comando executa/lista os itens e imprime `len(plano)` no resumo. ([backfill_fio_morto_e4b.py:262](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:262), [backfill_fio_morto_e4b.py:409](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:409)) |
| `partes` | `_resumo` do teste interpreta o texto `RESUMO`, remove `plano` e converte contagens em inteiros. O comando produz esse texto contando `p['res']`. ([test_e4_passivo.py:43](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:43), [backfill_fio_morto_e4b.py:405](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:405)) | O teste verifica soma e distribuição esperada. ([test_e4_passivo.py:113](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:113)) |

## 14/11 18:00 · sessao 37c906b22221 · - · - · origem=Ronald (tmux)

O vigia lê a porta publicada pela tarefa, sem refazer o predicado: abre `carga_da_tarefa(..., agora)`, chama `.tarefa` e `porta_da_tarefa` em `app/chamados/services/vigia_fio.py:647`, `:648`, `:649`. A alimentação anterior `alimentar_portas(..., agora=agora)` serve às causas do sem porta; é outra carga, embora receba o mesmo instante (`vigia_fio.py:611`, `:615`, `:621`).

Com porta, o vigia incrementa `alcance_<rota>`, registra papel=`porta['fio']` e encerra o processamento daquele dia (`vigia_fio.py:658`, `:662`, `:664`, `:665`). Sem porta, só depois dos baldes de lei calcula a causa e incrementa `fio_morto_alcance` (`vigia_fio.py:670`, `:672`, `:679`, `:714`). O detalhe `pergunta_viva_sob_chamado_terminal` é atribuído apenas nesse ramo já sem porta (`vigia_fio.py:706`, `:707`).

Na v2, a tarefa escreve `fio` e `rota` em `_portas_da_tarefa`; pergunta pendente no app com ação `responder/colaborador` dá dono `colab`, independentemente de âncora e prazo (`app/chamados/services/tarefa_do_dia.py:601`, `:602`, `:607`, `:608`, `:613`). Sem chamado vivo com dono, publica `porta_app` (`tarefa_do_dia.py:619`, `:624`, `:625`); a escrita final dos dois campos está em `:626`. `porta_da_tarefa` apenas copia campos da tarefa, retornando `None` quando não há rota (`:629`, `:636`). A matriz tem um caso explícito de só app → `(colab, porta_app, (), {})` (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:71`, `:73`).

O backfill usa o próprio vigia e filtra literalmente `x[4] == 'sem_porta'` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:78`, `:79`). Assim, uma pergunta sem resposta sob terminal que publique `porta_app` deixa o conjunto de reparo, embora permaneça no universo de dias retidos (`vigia_fio.py:599`, `:658`).

Há outra seleção de elegibilidade no backfill: `ato_do_dia` lê `tarefa_do_dia(colab, dia, agora)` (`backfill_fio_morto_e4b.py:87`), mas também chama `reabrir_credor_de_pergunta_viva(p, hoje=hoje, executar=False)` para todas as perguntas do dia (`:89`, `:90`, `:96`). Esse helper não recebe `agora`, não consulta `porta_da_tarefa` nem pendentes do app: testa resposta crua, C6 sem instante, âncora e status fora de VIVOS (`app/chamados/services/disputa_emissao.py:1812`, `:1828`, `:1830`, `:1834`). Portanto há a mesma autoridade de **porta** no vigia/tarefa, e uma seleção adicional de **reabertura de credor** com horizonte diferente no backfill (mesmas linhas).

A hipótese de que simplesmente mudar 15/09 para 25/09 retire `porta_app` da mesma pergunta **não foi demonstrada**. Pelo código visível, pergunta sem resposta, disputa aberta e célula `furo` é viva em ambos os instantes (`app/chamados/juizes.py:1447`, `:1450`, `:1451`); a partição do app exige apenas `respondida_em is None` e `disputa.fechada_em is None` (`app/chamados/services/pergunta_no_app.py:78`, `:82`, `:83`); sem decisões E1 a tarefa exige responder (`tarefa_do_dia.py:142`, `:143`). A carga declara expressamente que lê a pergunta/identidade atual e não recorta sua criação (`tarefa_do_dia.py:367`, `:371`, `:372`). A explicação exata do `v0=2` naquele anexo depende de outro estado/efeito ainda não identificado. [não verificado]

Escritores e leitores:

- `fio`, `rota`: escritos no resultado transitório pela tarefa (`tarefa_do_dia.py:626`); copiados por `porta_da_tarefa` (`:636`); lidos pelo vigia (`vigia_fio.py:662`, `:663`, `:664`).
- `fio_morto_alcance`: inicializado e incrementado pelo vigia (`vigia_fio.py:578`, `:714`); lido pelo backfill (`backfill_fio_morto_e4b.py:239`) e pelo teste (`app/chamados/tests/test_e4_passivo.py:111`, `:114`).
- `plano`: criado/apendido pelo comando (`backfill_fio_morto_e4b.py:251`, `:257`); `RESUMO plano` imprime `len(plano)` (`:409`).
- `partes`: o comando escreve o agregado `conta` por prefixo de resultado (`backfill_fio_morto_e4b.py:405`, `:407`, `:408`, `:409`); o teste lê a linha RESUMO, retira `plano` e chama o restante de partes (`test_e4_passivo.py:43`, `:46`, `:47`, `:113`, `:116`). [nao verificado]

## 14/11 17:59 · sessao c82c8fd8b7e8 · - · - · origem=Ronald (tmux)

Achados de fixture, todos só por leitura:

- `DIA = 2026-09-01`; `HOJE = 2026-09-25`; `AGORA_E4B = 2026-09-25 12:00 UTC`. Base cria competência 09/2026 aberta e uma célula de trabalho em `DIA`, `furo`, E 07:00 apagada/S 16:00 acesa. `app/chamados/tests/test_e4_backfill.py:27`, `:28`, `:30`, `:63`, `:77`.
- `PassivoE4bTest` inteiro fica congelado em 25/09 12:00; classe e cria chamado `resolvido`, sem célula, e pergunta sem resposta, motivo `saida_sem_entrada`, data 01/09, vinculada à célula-base e ao chamado terminal. `app/chamados/tests/test_e4_passivo.py:26`, `:29`, `:34`, `:67`.
- Disputa ganha `aberta_em` automaticamente; pergunta não tem `criada_em`. O horizonte da carga declara expressamente que pergunta/identidade/ata/status/fila são estado atual, não reconstrução histórica. `app/chamados/models.py:1803`; `app/chamados/services/tarefa_do_dia.py:367`.
- `_rodar` coloca `hoje=25/09` por padrão, mas quando `aplicar=True` congela apenas a execução do comando em 15/09 12:00 UTC. O comando usa `timezone.localtime()` e, em aplicar, ignora `--hoje` para obter `hoje=agora.date()`. Portanto cabeçalho 15/09 09:00 −03 do anexo é compatível. `app/chamados/tests/test_e4_backfill.py:98`, `:100`, `:103`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:218`.
- A exceção de classe c ganha congelamento próprio em 15/09 para resposta e apply coincidirem. Classe e e classes universos não têm esse congelamento próprio. `app/chamados/tests/test_e4_passivo.py:49`, `:51`, `:67`, `:93`.

Dois dias em `test_classes_e_universos_nao_somem_para_zerar`:

1. **01/09:** célula-base de fábrica, sem pergunta inicial, E apagada/S acesa. `app/chamados/tests/test_e4_backfill.py:77`.
2. **02/09:** `DIA+1`, célula análoga, pergunta sem resposta sob chamado resolvido, criada na montagem do teste. `app/chamados/tests/test_e4_passivo.py:97`, `:99`, `:101`, `:105`, `:107`.

O teste chama `censo_alcance` explicitamente em 25/09 12:00 UTC, exige `fio_morto_alcance=2`, depois chama `_rodar(aplicar=True)` em 15/09 e exige plano igual ao contador, soma de partes igual ao plano, e `{'resolvido':2}`. `app/chamados/tests/test_e4_passivo.py:110`, `:111`, `:112`, `:114`, `:115`, `:116`.

**Limite material encontrado:** não consigo derivar da cópia o sucesso de `v0['fio_morto_alcance']==2` no instante 25/09. A pergunta de 02/09 aparenta obter `responder/colaborador` e `porta_app` tanto em 15/09 quanto em 25/09: sem decisões, `_pela_decisao` retorna responder; C6 retorna viva por célula `furo`; consulta do app inclui pergunta e a partição pendente pede apenas resposta ausente e disputa aberta; `_portas_da_tarefa` dá dono colab/porta_app sem prazo. `app/chamados/services/tarefa_do_dia.py:142`, `:601`, `:607`, `:625`; `app/chamados/juizes.py:1448`; `app/chamados/services/pergunta_no_app.py:54`, `:63`, `:82`.

Não existe filtro por `Disputa.aberta_em` na seleção da tarefa nem na consulta do app; portanto o nascimento da disputa em 25/09 não impede sua leitura no horizonte de 15/09. `app/chamados/services/tarefa_do_dia.py:392`; `app/chamados/services/pergunta_no_app.py:54`.

Assim, **identificar categoricamente qual dia produziu `segue_sem_porta` na execução anexada exige linhas `DIA` ou estado não fornecidos — [não verificado]**. O anexo só dá o agregado e não mostra essas linhas. A hipótese “02/09 ficou sem porta” não fica sustentada apenas pelo fluxo visível da cópia. O `segue_sem_porta` é calculado comparando `cel_id` ao conjunto de linhas sem porta do censo posterior. `app/chamados/management/commands/backfill_fio_morto_e4b.py:352`, `:368`, `:371`.

`partes` é variável só do teste: `_resumo` lê a linha textual `RESUMO`, separa `plano` e converte contagens para inteiros. O comando produz as contagens a partir de `p['res']`, normalizando sufixos, e imprime `len(plano)`. `app/chamados/tests/test_e4_passivo.py:43`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:405`.

## 14/11 17:59 · sessao ffca45bbba11 · - · - · origem=Ronald (tmux)

- O relógio do comando é `agora = timezone.localtime()` uma vez na borda; em `--aplicar`, `hoje=agora.date()` mesmo que venha `--hoje`. O `_rodar` de `BaseE4b` injeta freeze em **15/09/2026 12:00 UTC** para apply; fora dele, `PassivoE4bTest` fica congelado em **25/09/2026 12:00 UTC**, inclusive na criação de pergunta/disputa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218`; `:220`; `app/chamados/tests/test_e4_backfill.py:94`; `:103`; `app/chamados/tests/test_e4_passivo.py:26`; `:34`)

- `_alcance` chama `censo_alcance` passando o mesmo `agora`, depois seleciona **apenas** `x[4] == 'sem_porta'`; esse é o filtro que tira um dia com `porta_app` do plano normal. `handle` soma apenas o complemento entregue/indeterminado da classe e, filtra competência e então constrói o plano. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`; `:79`; `:237`; `:250`; `:257`)

- O complemento `_exportadas_classe_e` não resgata o dia de competência aberta: ele dá `continue` quando `competencia_do_dia_entregue(...) is False`. Só inclui entregues/indeterminadas entre hoje−45 e hoje cuja porta de reabertura planeje algo. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:166`; `:173`; `:175`; `:178`)

- O censo consulta a tarefa **em lote**, abrindo `carga_da_tarefa(..., agora)` e produzindo tarefas/portas antes do laço. Ao encontrar porta, soma sua rota e lista seu fio, sem chegar à classificação e nem a `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:647`; `:648`; `:649`; `:658`; `:662`; `:665`; `:714`)

- O backfill não reutiliza a carga do censo: `ato_do_dia` chama `T.tarefa_do_dia(colab,dia,agora)` sem `carga`, portanto a casca abre uma carga individual nova. Na execução, repete `ato_do_dia` sob a trava. O censo posterior ganha outro instante, explicitamente `timezone.localtime()`, para enxergar fatos escritos pelos atos. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`; `:309`; `:350`; `:352`; `app/chamados/services/tarefa_do_dia.py:650`; `:654`)

- Há uma consulta adicional de elegibilidade no backfill: ele percorre todas as perguntas atuais do colaborador/data e chama `reabrir_credor_de_pergunta_viva(p,hoje=hoje,executar=False)` **independentemente das ações retornadas por tarefa_do_dia**. O helper exige `respondida_em is None`, `pergunta_viva(p,hoje=hoje)` e âncora existente fora de `VIVOS`; não recebe `agora`. Assim, o ato “reabrir” não nasce exclusivamente da tarefa, embora o universo normal venha da porta dela. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:89`; `:90`; `:96`; `app/chamados/services/disputa_emissao.py:1828`; `:1830`; `:1834`)

- A reabertura efetiva passa por `credor.reabrir()` e anexa nota; recusa também anexa nota. (`app/chamados/services/disputa_emissao.py:1858`; `:1859`; `:1864`)

- O horizonte da tarefa é parcial: pergunta/identidade e âncora são estado atual, sem corte por nascimento; decisões são cortadas por `ato_em`, passivo pelo instante da projeção e efeitos por `Batida.criado_em`. Portanto pergunta criada no freeze de 25/09 pode existir na carga julgada em 15/09. (`app/chamados/services/tarefa_do_dia.py:367`; `:372`; `:392`; `:409`; `:416`; `:428`)

- App: `consultas_do_app` passa `agora` ao C6, mas `_particao` lê diretamente `respondida_em` e `disputa.fechada_em` atuais para pendentes/histórico. Não projeta esses dois campos para a partição. (`app/chamados/services/pergunta_no_app.py:63`; `:78`; `:82`; `:89`; `:96`; `:98`)

- Escritores/leitores:
  - `fio` e `rota`: escritos no dicionário por `_portas_da_tarefa`; `porta_da_tarefa` lê e devolve se há rota; censo lê para contagens/listagem. (`app/chamados/services/tarefa_do_dia.py:626`; `:636`; `app/chamados/services/vigia_fio.py:662`; `:664`)
  - `fio_morto_alcance`: inicializado em zero e incrementado somente ao fim do caminho sem porta/sem balde; backfill lê antes/depois. (`app/chamados/services/vigia_fio.py:578`; `:714`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:239`; `:415`)
  - `plano`: lista local criada e preenchida pelo comando; atos/pids são revalidados; resultado é `SEGUE` se o `cel_id` ainda está no sem-porta medido depois, senão `RESOLVIDO`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:251`; `:257`; `:310`; `:368`; `:371`)
  - `partes`: só existe como dicionário local do teste, derivado da linha `RESUMO`; o comando imprime essa linha contando `p['res']`. (`app/chamados/tests/test_e4_passivo.py:43`; `:46`; `:47`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:405`; `:407`; `:409`)

Não consegui fechar estaticamente qual dos dois dias do teste misto permaneceu em `segue_sem_porta`; a saída fornecida só traz as contagens, sem linhas `DIA`. [não verificado] [nao verificado]

## 14/11 17:46 · sessao 68b2829022d2 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

lei: falta regra escrita que determine o abono quando o período justificado 13h–18h30 se sobrepõe ao trabalho 08h–17h; L-088/L-089 não respondem essa conta, e o corte define o marco coberto, sem definir o pagamento dessa interseção (`docs/LEIS.md:120–121`; `docs/CORTES.md:342`).

**(a) Origem, escritor e intervalo**

O escritor do valor é `periodos_justificados`: consulta justificativas aprovadas por `data_turno`, reúne seus intervalos e mede a união. `efeitos_da_ausencia` monta `min_justificados` e remove o parcial quando existe Ausencia ABONA inteira (`ponto/services/efeito_ausencia.py:99–102`, `137–154`, `47`, `63`).

O intervalo depende do marco:

- `esqueci_saida`: de `max(ponta_real, hi)` até `hf`; a ponta considera a última batida do turno e eventual `saida_informada` (`ponto/turnos.py:2737–2738`; `ponto/services/efeito_ausencia.py:197–202`).
- `esqueci_entrada`: de `hi` até `min(primeira_batida, hf)` (`ponto/turnos.py:2739`; `ponto/services/efeito_ausencia.py:203–204`).

Desconta pausas previstas e une intervalos sobrepostos de justificativas. **Não subtrai o trabalho do período.** Antes da medição, um filtro retira o marco inteiro se seu trecho cruza algum par fechado identificado; portanto, rejeição de cobertura e desconto de minutos trabalhados são comportamentos distintos (`ponto/turnos.py:2751–2766`, `2792–2805`; `ponto/services/efeito_ausencia.py:205–206`, `144–154`).

A grade transporta o mapa para a linha; `por_dia_da_grade` soma esse valor sem outra medição (`escala/services/leitor_celula.py:365`, `435–436`; `ponto/services/dia_pago.py:398–399`).

Para **col82 em 31/10** e **col592 em 10/09**, os totais fornecidos não identificam início/fim do período nem por que o filtro anterior permitiu crédito. Reprodução individual: **[nao verificado]**. Para decidir, faltam, em cada dia: justificativas aprovadas, tipo, âncora, `saida_informada`, marcos/pausas do vínculo, turnos pareados com papéis e timestamps, além da ata que forneceu realizado/previsto (`ponto/services/efeito_ausencia.py:99–129`, `172–205`; `escala/services/leitor_celula.py:385`, `403–429`).

**(b) Motor versus gravado**

No ramo com dias previstos informados, o motor soma integralmente `_mj[d] / 60` para os dias obrigatórios justificados. Não desconta realizado nessa soma (`ponto/motor_calculo_v2.py:2628–2634`).

Assim, **com o mesmo mapa e o mesmo enquadramento do dia, motor e gravado recebem o mesmo abono parcial**, inclusive quando há trabalho: o motor soma `_mj[d]`; a grade soma `min_justificados`. A igualdade efetiva dos resultados completos nos 41 dias: **[nao verificado]** (`ponto/motor_calculo_v2.py:2633–2634`; `ponto/services/dia_pago.py:398–399`).

A obra manda ambos seguirem **`efeito_ausencia`**, não escolher um resultado divergente como autoridade. O fechamento injeta esse mapa no motor e grava o abono retornado pela grade (`docs/CORTES.md:375`; `ponto/services/fechamento.py:665–667`, `755`, `1066–1067`).

A divergência A2 do anexo é outro ramo: Ausencia inteira com trabalho. Ela não demonstra divergência nestes casos de crédito parcial (`ponto/services/dia_pago.py:396–399`; `ponto/motor_calculo_v2.py:2611–2614`).

**(c) Caso antes de qualquer código**

**CASO:** período justificado 13h–18h30; trabalho 08h–17h. **Resposta legal numérica: [nao verificado].** As regras encontradas não determinam se deve haver subtração da interseção ou outro tratamento (`docs/LEIS.md:120–121`; `docs/CORTES.md:342`, `352`, `375`).

Se esses intervalos forem apresentados diretamente ao filtro atual, ele rejeita o marco inteiro por cruzamento. Isso descreve o código existente; não fornece fundamento para instituir outra conta (`ponto/turnos.py:2762–2766`).

**(d) Classificação dos 41**

Pela função medida, os 41 pertencem ao ramo **trabalho com crédito justificado positivo**: no ramo `ausencia`, abono é `previsto − realizado_capado`, logo sua soma com realizado não produziria o excesso observado. A grade emite uma linha por data (`ponto/services/dia_pago.py:394–404`; `escala/services/leitor_celula.py:383`, `451–452`).

Os totais `pv/abono/real` **não distinguem a causa temporal dos 41**. Não permitem atribuí-los individualmente a pausa fora de ordem ou sobreposição com trabalho. A classificação causal permanece **[nao verificado]**.

O encadeamento realmente desloca para D+1 um marco que não sucede o anterior; isso comprova o mecanismo possível, não sua ocorrência nesses registros (`escala/utils.py:345–347`). A indicação de sobreposição para col82 veio da triagem fornecida; sua reconstrução pelos instantes permanece **[nao verificado]**.

**Censo de escritores e leitores**

| Papel | Arquivo:linha |
|---|---|
| Calcula período e escreve mapa | `ponto/services/efeito_ausencia.py:140`, `47`, `68` |
| Escreve `min_justificados` nas grades | `escala/utils.py:1276`; `escala/services/leitor_celula.py:436` |
| Lê crédito na grade | `ponto/services/dia_pago.py:398–399` |
| Alimenta motor | `ponto/services/fechamento.py:755`, `831`; `ponto/services/espelho.py:1134`; `relatorios/pdf_espelho.py:407` |
| Alimenta caminhos de comparação | `ponto/management/commands/diff_espelho_alimentacao.py:56`, `124`; `relatorios/management/commands/diff_extrato_parcial.py:62`; `ponto/services/esmeril_espelho.py:166` |
| Escreve `FechamentoMensal.minutos_abonados` | `ponto/services/fechamento.py:1067` |
| Escreve abono em DiaPago: lavratura/retrato | `ponto/services/dia_pago.py:288–292`, `479`, `484` |
| Lê abono: prontidão, conferência, cartão e contador | `folha/export.py:1078`, `1104`; `folha/porta_export.py:139`; `relatorios/cartao_pela_celula.py:118`, `186`; `ponto/services/dia_pago.py:702` |

Comando do censo executado:

```bash
git grep -n -E 'min_justificados|minutos_justificados|minutos_abonados|por_dia_da_grade' \
  -- ponto escala folha relatorios ':!**/models.py' ':!**/migrations/**'
```

Vizinhos identificados: transporte e cálculo parcial (`ponto/tests/test_justificativa_periodo_v2_sem_banco.py:196`, `285`, `307`), percurso ORM até fechamento/DiaPago (`ponto/tests/test_justificativa_paga_o_periodo.py:395–408`), realizado da grade (`escala/tests/test_montador_realizado_pela_autoridade.py:479`), conferência da folha (`folha/tests/test_gravado_discorda_da_propria_grade.py:50–58`) e leitores do cartão (`relatorios/cartao_pela_celula.py:118`, `186`).

Executei **15 testes existentes, todos passando**, com: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_justificativa_periodo_v2_sem_banco -v
```

Esse conjunto declara que não prova ORM, frota, DSR produtivo nem hashes TXT (`ponto/tests/test_justificativa_periodo_v2_sem_banco.py:6–8`).

**Diff por arquivo::função: vazio.** `git diff --stat` e `git status --short` terminaram sem saída. Não construí cura, RED novo nem mutantes novos porque a condição (c) não foi satisfeita. Não executei ORM, banco ou nova medição de frota; resultados desses percursos permanecem **[nao verificado]**. A certificação pelo Code e pelo opus final permanece pendente, conforme `docs/LEIS.md:147`.

## 14/11 17:43 · sessao b1bc8dd9fd69 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura entregue na cópia. A lista dos oito arquivos tocados, motivo por item, censo e comandos está na [entrega c24](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:6).

- **Catraca:** o `IfExp` contaminava `_bo` e, transitivamente, `_linhas_do_fato`. A cura usa `if/else` somente no hunk P3, preservando `None` explícito e a entrega integral do universo à canônica. A catraca permanece intacta. [models.py:2923](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/models.py:2923), [explicação:26](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:26).
- **(a)** A mudança relevante da c22 consulta a ata **antes** do classificador silencioso no ramo `sem_origem`. Isso marca a abstenção e impede o autofecho. [disputa_emissao.py:1465](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1465).
- **(b)** A fixture original não lavra ata nem cria batida-origem; cria vínculo, chamado e disputa vazia. [test_b2d_resposta_visivel.py:29](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/tests/test_b2d_resposta_visivel.py:29).
- **(c)** Para essa fixture original, a resposta correta é **acender e esperar**. Os casos antigos ganharam ata pelas portas reais; o caso sem ata ganhou teste próprio, mantendo abertura, luz e silêncio em duas passadas. [regra:1532](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1532), [fixture e contraste:16](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/tests/test_hx_disputa_muda.py:16). [nao verificado]
- **(d)** Sem nenhuma pergunta, não pode notificar. O envio observado é da **principal**, não de `complementadas`: esse dicionário exige pergunta anterior e complemento admitido. A guarda nova exige pergunta na própria disputa. [inclusão:1295](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1295), [push:1552](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/chamados/services/disputa_emissao.py:1552). [nao verificado]

O [censo completo](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:90) registra todas as escritas e leituras das três variáveis. Os [seis chamadores produtivos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:112) passam a suprimir o push da principal vazia quando a notificação está habilitada; retorno e autofecho conservam seus caminhos. [nao verificado]

Resultados locais: cinco testes c24 passaram; a base reproduziu três falhas por asserção; os cinco mutantes novos morreram, sem errors. A catraca original executada em memória passou oito testes, e as regressões passaram 8 + 12 + 6. Cada comando está [registrado na entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-172807/app/docs/BO_DISPUTA_C24_ENTREGA.txt:130). [nao verificado]

Certificação pela suíte real, ORM e execução da fixture nova: **[nao verificado]**. [nao verificado]

## 14/11 17:38 · sessao 190a94933cb1 · - · - · origem=Ronald (tmux)

Não encontrei defeito concreto que exija ajuste na fixture atual; execução ORM continua [nao verificado].

- O isolamento intercepta o nome importado dinamicamente pelo cartório e permanece ativo durante o `post_save` do órfão (`chamados/tests/test_hx_disputa_muda.py:28`, `ponto/services/cartorio.py:1033`, `:1045`, `chamados/signals.py:190-218`).
- O piso impede que o julgamento de 20/05 espelhe marcos ou abra cobranças pela emissão automática (`ponto/services/cartorio.py:298`, `:340-354`, `:1727`, `:1813`). A entrada real mantém a premissa acusadora; a guarda de nascimento só absolve diante de veredito conhecido fora de `VEREDITOS_ACUSAM` (`chamados/reconciliador.py:1027-1031`).
- O preparo exige resposta real `lido`; o novo caso exige `sem_celula` e origem ausente antes do repasse, depois verifica duas passadas abertas, sem pergunta, com luz e sem push (`chamados/tests/test_hx_disputa_muda.py:34-35`, `:80-94`).

Precisão para a entrega: esses dois casos não variam apenas a ata. O preparo com ata também cria batida-origem; o caso sem ata mantém origem ausente (`chamados/tests/test_hx_disputa_muda.py:29-31`, `:81`). Assim exercitam os dois ramos silenciosos distintos (`chamados/services/disputa_emissao.py:1420-1429`, `:1459-1471`).

## 14/11 17:31 · sessao 190a94933cb1 · - · - · origem=Ronald (tmux)

Censo por leitura da cópia; execução ORM e efeitos reais de signals: [nao verificado].

| Variável | Escrita | Leitura |
|---|---|---|
| `_absteve_por_ata` | Inicializada em `chamados/services/disputa_emissao.py:1289`; marcada nos complementos `:1304`, caminho de ata `:1392`, batida com classificador silencioso `:1428`, batida classificada `:1438` e sem origem `:1467`. | Veto de auto-fecho em `:1540`; acende em `:1542`, preservando a disputa aberta. |
| `complementadas` | Criada em `:1277`; única inclusão em `:1310`, seguida de atualização de motivos em `:1311`. Exige pergunta anterior `:1295`, ata disponível `:1302` e complemento admitido `:1307`. | Percorrida em `notificar_complementos`, `:1280`; push em `:1283`. Chamadas em `:1320`, `:1527`, `:1531`; esta última exclui a principal pela condição `:1281`. | [nao verificado]
| `motivos_atualizados` | Inicializada pelos motivos da principal em `:1327-1328`; recebe resultados de pergunta/complemento em `:1373`, `:1394`, `:1440`, `:1446`, `:1472`, `:1477`; seed `orfao_14h` em `:1486`. | Criação da disputa `:1354`; agregação `:1360`, `:1380`, `:1487`; insumo das mesmas portas de pergunta/complemento; entradas pendentes `:1501`; condição do push `:1550` e argumentos em `:1552`. Persistência dos motivos pertence a `_agregar_motivos_na_dona`, `:587`, `:601`, `:607`. | [nao verificado]

A fixture HX **não entra em `complementadas`**: o preparo cria disputa vazia com `motivos=['orfao_14h']`, sem perguntas (`chamados/tests/test_b2d_resposta_visivel.py:44-50`); a inclusão no dicionário exige pergunta anterior e complemento admitido (`chamados/services/disputa_emissao.py:1295`, `:1307`, `:1310`). O push `nova=False` do anexo corresponde ao push principal em `:1552`, após selecionar a disputa aberta existente em `:1323-1325` e copiar seus motivos em `:1327-1328`.

Também não há batida-origem criada pelo preparo (`chamados/tests/test_b2d_resposta_visivel.py:29-50`). Sem candidatas, `_achar_batida_origem` devolve `None` (`chamados/services/materializacao.py:140`); portanto a reordenação da c22 que alcança esse cenário é a consulta à ata antes do classificador no ramo sem origem (`chamados/services/disputa_emissao.py:1465-1471`). O ramo silencioso de batida existe separadamente em `:1420-1429`.

Os seis chamadores produtivos encontrados são:

- `ponto/management/commands/processar_alertas_turno.py:385`.
- `ponto/management/commands/processar_alertas_avancados.py:159`.
- `ponto/management/commands/detectar_ausencias.py:803`.
- `chamados/services/acoes_disputa.py:222`.
- `chamados/services/cobrar_dia.py:49`.
- `chamados/services/pergunta_no_app.py:246`, cujo parâmetro padrão é `notificar=False` em `:231`.

Os quatro primeiros importam pela reexportação de `chamados/models.py:4151`; os dois últimos importam diretamente do emissor (`cobrar_dia.py:20`, `pergunta_no_app.py:238`).

Adicionar existência de pergunta apenas à condição do push principal suprime, para esses chamadores, a notificação da principal aberta e vazia que passa pela condição atual (`chamados/services/disputa_emissao.py:1550-1552`). O retorno e a decisão de auto-fecho ficam nas linhas anteriores (`:1539-1549`); complementos continuam pelo caminho próprio (`:1279-1283`).

Há dois contratos de teste anteriores incompatíveis com essa supressão: ambos exigem push após afirmar zero perguntas sem ata (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:191-199`, `chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:187-193`). A resposta textual administrativa também afirma “Colaborador notificado” incondicionalmente após retorno de disputa (`chamados/services/acoes_disputa.py:231-232`); o efeito real dessa mensagem na execução corrigida é [nao verificado].

## 14/11 17:19 · sessao 7db8be5ab487 · JUSTIFICATIVA-PAGA · verificador · origem=Code

**Há curas efetivas, mas a classe ainda reprova: restam abono acima do previsto, divergência motor × gravado, prontidão paralela, `dias_abono` com significados diferentes e selo que aceita alimentação vazia indevida.** (`app/ponto/services/dia_pago.py:399`; `app/ponto/motor_calculo_v2.py:2611`; `app/folha/export.py:1100`; `app/ponto/services/dia_pago.py:701`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`)

**1. Achados da v1.r: situação no hash**

| Achado da v1 | Resultado |
|---|---|
| Parcial não chegava ao gravado | **Curado para justificativa parcial em dia `trabalho`**: a grade transporta, `por_dia_da_grade` lê, fechamento soma e entrega o mesmo mapa ao DiaPago. A igualdade integral entre motor e gravado ainda falha com ABONA e trabalho, descrita em A2. (`app/escala/services/leitor_celula.py:436`; `app/ponto/services/dia_pago.py:398`; `app/ponto/services/fechamento.py:1067`; `app/ponto/services/fechamento.py:1217`) |
| Chamadores de sombra sem mapa | **Curados os três apontados**: diff espelho, extrato e esmeril recebem a fonte. A/B/C do diff continuam explicitamente sem crédito; D recebe a fonte completa. (`app/ponto/management/commands/diff_espelho_alimentacao.py:109`; `app/ponto/management/commands/diff_espelho_alimentacao.py:124`; `app/relatorios/management/commands/diff_extrato_parcial.py:62`; `app/ponto/services/esmeril_espelho.py:166`) |
| Segunda pausa perdida | **Curada para pausas regularmente ordenadas**, pela cadeia compartilhada e desconto de todos os pares. A classe semântica completa permanece aberta: pausas sobrepostas produzem virada indevida e abono excessivo, A1. (`app/ponto/services/efeito_ausencia.py:176`; `app/ponto/turnos.py:2792`; `app/escala/utils.py:346`) |
| Duas âncoras somavam a mesma cauda | **Curado por classe**: reúne intervalos por dia e mede sua união, incluindo sobreposição e trechos disjuntos. (`app/ponto/services/efeito_ausencia.py:137`; `app/ponto/services/efeito_ausencia.py:140`; `app/ponto/services/efeito_ausencia.py:150`) |
| ABONA inteira concorria com mapa parcial | **Curado o mapa**, retirando o parcial. **Não curada toda a conta**: com trabalho no dia, o motor não paga o restante inteiro, enquanto a grade paga. (`app/ponto/services/efeito_ausencia.py:63`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/services/dia_pago.py:397`) |
| Cobertura vazia após cruzamento | **Permanece**: o filtro posterior ao juiz pode remover o marco; o efeito exclui esse dia das justificadas. Vazio, sozinho, continua não demonstrando falta inteira. (`app/ponto/services/efeito_ausencia.py:205`; `app/ponto/turnos.py:2764`; `app/ponto/services/efeito_ausencia.py:46`; `app/ponto/motor_calculo_v2.py:2605`) |
| Falha nova de relógio | **Não demonstrada** no caminho novo: cadeia usa data explícita, batidas e saída informada usam timestamps explícitos. (`app/escala/utils.py:345`; `app/ponto/services/efeito_ausencia.py:198`; `app/ponto/services/efeito_ausencia.py:202`) |
| Testes sem percurso até o gravado | **Melhorou, mas não fecha**: há teste ORM dos escritores reais, não executado nesta verificação; os isolados executam funções e blocos selecionados. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:367`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:398`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:294`; execução ORM [nao verificado]) |
| Derivação paralela e campo com dois significados | **Parcialmente curado**: saiu a montagem manual de quatro marcos; continuam o fallback de prontidão e a divergência de `dias_abono`. (`app/ponto/services/efeito_ausencia.py:177`; `app/folha/export.py:1100`; `app/ponto/services/dia_pago.py:701`; `app/ponto/motor_calculo_v2.py:2630`) |
| Selo insuficiente | **Permanece**, embora agora detecte remoção da atribuição: atribuir `{}` indevidamente ainda passa, A5. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:90`) |

**2. Fonte única: censo de produção**

A busca literal encontrou os seguintes escritores, transportadores e leitores; os testes são examinados no item 8. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:57`)

| Campo/função | Escrita/composição | Leitura/transporte |
|---|---|---|
| `periodos_justificados` | Consulta e agregação em `app/ponto/services/efeito_ausencia.py:99`, `app/ponto/services/efeito_ausencia.py:127`, `app/ponto/services/efeito_ausencia.py:140` | Efeito em `app/ponto/services/efeito_ausencia.py:45`; grade em `app/escala/utils.py:1198`, `app/escala/utils.py:1251`, `app/escala/utils.py:1257`, `app/escala/utils.py:1276` |
| `intervalos_do_periodo_justificado` | Trechos e subtração de pausas em `app/ponto/turnos.py:2783`, `app/ponto/turnos.py:2792`, `app/ponto/turnos.py:2805` | Medidor em `app/ponto/turnos.py:2780`; fonte em `app/ponto/services/efeito_ausencia.py:129`, `app/ponto/services/efeito_ausencia.py:206` |
| Mapa filtrado `min_justificados` | Cria em `app/ponto/services/efeito_ausencia.py:47`; remove ABONA em `app/ponto/services/efeito_ausencia.py:63`; devolve em `app/ponto/services/efeito_ausencia.py:68` | Fechamento em `app/ponto/services/fechamento.py:667`; grade em `app/escala/services/leitor_celula.py:365`; demais alimentadores abaixo |
| `min_justificados` na linha | `app/escala/utils.py:1276`; `app/escala/services/leitor_celula.py:436` | `app/ponto/services/dia_pago.py:398`, `app/ponto/services/dia_pago.py:399` |
| `minutos_justificados` no motor | Inicializa em `app/ponto/motor_calculo_v2.py:813`; escreve em `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:831`, `app/ponto/services/espelho.py:1134`, `app/relatorios/pdf_espelho.py:407`, `app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `app/relatorios/management/commands/diff_extrato_parcial.py:62`, `app/ponto/services/esmeril_espelho.py:166` | Conta em `app/ponto/motor_calculo_v2.py:2628`, `app/ponto/motor_calculo_v2.py:2634`; contrato nominal em `app/ponto/calculador/nucleo.py:424` |
| `dias_com_abono` | Deriva datas pela grade e conta DiaPago em `app/ponto/services/dia_pago.py:700`, `app/ponto/services/dia_pago.py:701`, `app/ponto/services/dia_pago.py:702` | Cartão em `app/relatorios/cartao_pela_celula.py:188` |
| `minutos_abonados` materializado | Fechamento em `app/ponto/services/fechamento.py:1067`; lavratura em `app/ponto/services/dia_pago.py:288`, `app/ponto/services/dia_pago.py:292`; retrato em `app/ponto/services/dia_pago.py:467`, `app/ponto/services/dia_pago.py:479`, `app/ponto/services/dia_pago.py:484` | Cartão em `app/relatorios/cartao_pela_celula.py:118`, `app/relatorios/cartao_pela_celula.py:186`; prontidão em `app/folha/export.py:1078` |

Motor, fechamento, espelho, PDF, esmeril e os três caminhos laterais identificados recebem os minutos da fonte; a derivação paralela comprovada de prontidão continua fora dessa cadeia. (`app/ponto/services/fechamento.py:667`; `app/ponto/services/espelho.py:1134`; `app/relatorios/pdf_espelho.py:407`; `app/ponto/services/esmeril_espelho.py:166`; `app/folha/export.py:1100`)

**A3 — prontidão diverge conforme o ramo.** Com snapshot, soma realizado e `minutos_abonados`. Sem snapshot, soma como abono somente o previsto de `ausencia`, ignorando justificativa parcial em `trabalho`. No exemplo sintético previsto 660, realizado zero, parcial 630: snapshot **95%**, fallback **0%**. O fechamento grava 630. (`app/folha/export.py:1078`; `app/folha/export.py:1079`; `app/folha/export.py:1100`; `app/folha/export.py:1111`; `app/ponto/services/fechamento.py:1067`)

**8932 responde outra grandeza:** tipos do catálogo e datas de registros `Ausencia`; não lê minutos justificados nem lavrados. Uma justificativa parcial isolada não entra nessa contagem, embora gere minutos no fechamento. Isso é diferença de alcance e unidade, não prova de desconto no pagamento. Emissão efetiva da 8932 por essa função: **[nao verificado]**. (`app/folha/export.py:193`; `app/folha/export.py:198`; `app/folha/export.py:204`; `app/folha/export.py:160`; `app/ponto/services/dia_pago.py:399`)

**3. Semântica**

Não há significado uniforme em todos os campos e leitores: a fonte usa dia do turno; DSR Comercial usa data civil; o contador de dias usa classificação atual da grade; o motor conta dias justificados sem trabalho. (`app/ponto/services/efeito_ausencia.py:109`; `app/ponto/motor_calculo_v2.py:3477`; `app/ponto/services/dia_pago.py:701`; `app/ponto/motor_calculo_v2.py:2611`)

- **Dia e fuso:** a fonte usa `data_turno`, rejeita âncora atribuída a outro dia e encadeia marcos locais através da meia-noite. Converte timestamps por `localtime`; não encontrei nessa cadeia comparação intencional de hora local ingênua com UTC. (`app/ponto/services/efeito_ausencia.py:184`; `app/escala/utils.py:345`; `app/escala/utils.py:347`; `app/ponto/services/efeito_ausencia.py:198`)
- **Unidade:** fonte e lavratura usam minutos; motor e resumo convertem por `/60`. (`app/ponto/services/efeito_ausencia.py:154`; `app/ponto/services/dia_pago.py:288`; `app/ponto/motor_calculo_v2.py:2634`; `app/relatorios/cartao_pela_celula.py:118`)
- **Bordas:** seleção de datas inclui ambas; duração é diferença entre instantes, sem minuto adicional; contato entre extremidades não configura cruzamento. A classificação de `Ausencia` parcial, distintamente, inclui ambas as extremidades. (`app/ponto/services/efeito_ausencia.py:101`; `app/ponto/services/efeito_ausencia.py:154`; `app/ponto/turnos.py:2764`; `app/ponto/turnos.py:2657`)
- **Pausas:** a justificativa desconta todos os pares transportados pela cadeia; isso não garante coerência com o previsto para pausas sobrepostas ou fora de ordem, A1. (`app/ponto/services/efeito_ausencia.py:177`; `app/ponto/turnos.py:2792`; `app/escala/utils.py:2324`)

**22–06 com “justificativa 23–02”:** `Justificativa` não representa um intervalo arbitrário com início/fim; representa tipo, âncora e eventualmente `saida_informada`. Com `esqueci_saida`, âncora E23 e saída informada D+1 02, paga a cauda **02–06 = 240 minutos**, sem pausas. Sem saída informada, paga **23–06 = 420**. O intervalo declarado 23–02 pertence ao modelo `Ausencia`, cujo classificador desloca o fim para o dia seguinte. (`app/ponto/models.py:144`; `app/ponto/models.py:149`; `app/ponto/turnos.py:2738`; `app/ponto/services/efeito_ausencia.py:202`; `app/ponto/models.py:768`; `app/ponto/turnos.py:2654`)

**07–19, pausa 10–11, E07 e saída informada 07:30:** paga **630 minutos**. Se houver uma batida S07:30 fechando a ponta, conserva inteiro em vez de aplicar esse parcial. (`app/ponto/services/efeito_ausencia.py:202`; `app/ponto/turnos.py:2792`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:172`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:321`)

**4. Conta, teto, duplicidade, falta e DSR**

**A1 — parcial pode superar o previsto.** Reprodução sintética em memória, usando fonte e função de previsto reais: jornada 07–19, pausas **10–12 e 11–13**, E07 ancorada:

| Resultado | Minutos |
|---|---:|
| Previsto real | 480 |
| Fonte de justificativa | 1.920 |
| `por_dia_da_grade` | 1.920 |

A sequência mantém a ordem cadastrada; como 11h sucede 12h na lista, a cadeia empurra a segunda pausa para D+1 e também desloca o fim do turno. O previsto desconta durações sem essa cadeia. Não há teto final no parcial. O cadastro transporta o segundo par; o validador mostrado examina somente o primeiro. Ocorrência produtiva **[nao verificado]**. (`app/escala/utils.py:77`; `app/escala/utils.py:346`; `app/escala/utils.py:2328`; `app/ponto/services/efeito_ausencia.py:154`; `app/ponto/services/dia_pago.py:399`; `app/escala/services/cadastro_tipo.py:232`; `app/escala/models.py:656`)

**A2 — ABONA inteira com trabalho continua divergindo.** A retirada do parcial evita somar inteiro e parcial no mapa. Entretanto, com previsto 660 e realizado 180, o gravado recebe **480**, enquanto o motor recebe **zero** de abono: o dia trabalhado não integra `_abono`, e o mapa parcial foi esvaziado. Reproduzido executando os blocos reais em memória. (`app/ponto/services/efeito_ausencia.py:63`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/services/dia_pago.py:397`)

Duas justificativas sobrepostas agora pagam a união. Duas **Ausencia parciais**, porém, continuam tendo seus `a.minutos` somados sem união no efeito e no crédito diário; o PDF acrescenta a soma parcial ao abono do motor. É resíduo preexistente, não curado pela união de justificativas. (`app/ponto/services/efeito_ausencia.py:140`; `app/ponto/services/efeito_ausencia.py:52`; `app/ponto/services/credito_parcial.py:28`; `app/relatorios/pdf_espelho.py:480`)

**Falta inteira:** cobertura vazia retira a justificativa do conjunto, mas o Base também considera períodos abertos como trabalho; portanto, não se pode concluir “vazio ⇒ falta inteira”. A falta exige ausência simultânea de trabalho e justificativa. (`app/ponto/services/efeito_ausencia.py:46`; `app/ponto/motor_calculo_v2.py:2499`; `app/ponto/motor_calculo_v2.py:2583`; `app/ponto/motor_calculo_v2.py:2605`)

**DSR Comercial:** continua vulnerável à alimentação que perde a justificativa: refaz trabalho pela data civil, depois cobra dias obrigatórios ausentes de trabalho e justificadas. Se a fonte esvaziar a cobertura de D e a entrada civil estiver em D+1, esse predicado pode perder a semana mesmo havendo jornada atribuída a D no Base. Reprodução integrada desse cenário pelo pareador e ocorrência produtiva **[nao verificado]**. (`app/ponto/services/efeito_ausencia.py:205`; `app/ponto/motor_calculo_v2.py:2583`; `app/ponto/motor_calculo_v2.py:3477`; `app/ponto/motor_calculo_v2.py:3490`)

**A condição literal 0 falta inteira / 0 DSR adicional no universo 1.891 não está demonstrada pelos dados fornecidos: [nao verificado].**

**5. Âncora inválida conserva?**

**Sim, no predicado compartilhado**, para sem âncora, papel errado e ponta fechada: retorna motivo; fonte converte para `cobre=None`; efeito mantém o dia justificado sem crédito parcial. A grade também lê `None` como justificativa inteira. Não há tratamento por nome nesses ramos. (`app/ponto/turnos.py:2694`; `app/ponto/turnos.py:2700`; `app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:131`; `app/ponto/services/efeito_ausencia.py:46`; `app/escala/utils.py:1251`)

**“Em todos os leitores” não está provado**: a garantia do predicado não elimina A2, A3 e A4. Reprodução individual dos três dias exigidos pelo aval e lista administrativa por motivo: **[nao verificado]**.

**6. Motor tocado?**

O **delta fornecido não altera** `motor_calculo_v2.py`. No hash existem a inicialização do memo e o bloco que substitui abono inteiro por minutos da fonte nos dias obrigatórios justificados. (`app/ponto/motor_calculo_v2.py:813`; `app/ponto/motor_calculo_v2.py:2628`; `app/ponto/motor_calculo_v2.py:2634`)

O anexo v1.r já identifica esses trechos como mudança da obra. O confronto independente de **todo** `14b61c124..29460c1d1` ficou **[nao verificado]**: a cópia não contém histórico Git; inclusive o carregamento histórico do teste depende de `git show`. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`)

**7. Idempotência e escritor**

**Estado integral não é idêntico:** fechamento renova carimbos e DiaPago é apagado/recriado, renovando identidade e `lavrado_em`. Igualdade dos valores financeiros em duas execuções completas, com entradas fixas: **[nao verificado]**; o teste novo executa o fechamento uma vez. (`app/ponto/services/fechamento.py:1091`; `app/ponto/services/fechamento.py:1148`; `app/ponto/services/dia_pago.py:359`; `app/ponto/models.py:380`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:398`)

Não encontrei escritor novo de FechamentoMensal fora da porta no delta; o censo existente também não atribuiu escrita externa. A porta declarada permanece `ponto/services/fechamento.py`; escritas que o censo não consegue atribuir continuam limitação da análise. (`app/folha/tests/test_chokepoint_folha_gate.py:58`; `app/folha/tests/test_chokepoint_folha_gate.py:72`; `app/core/censo_escritas.py:43`)

**8. Testes e mutantes** [nao verificado]

Na execução local sem escrita: **15/15 testes passam; 14/14 mutantes declarados morrem por asserção**. Há mordida além do golden: união sobreposta/disjunta, duas pausas, duas âncoras, precedência e transporte à grade. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:186`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:192`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:201`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:258`; `app/ponto/tests/mutantes_justificativa_periodo_v2.py:78`)

**A5 — mutante plausível sobrevivente comprovado:** trocar a atribuição do esmeril para `_m.minutos_justificados = {}`. Os 15 continuam verdes: o selo verifica que há uma escrita antes do uso, sem conferir sua origem ou valor. O mutante declarado remove a escrita, cobrindo uma classe menor. (`app/ponto/services/esmeril_espelho.py:166`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:90`; `app/ponto/tests/mutantes_justificativa_periodo_v2.py:42`)

Há testes de alcance limitado, que não devem ser apresentados como prova financeira integral: [nao verificado]

- O teste 06 usa motor falso que devolve o próprio atributo: prova argumento obrigatório e transporte, não cálculo. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:226`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:235`)
- O teste 07 usa contador falso baseado em `data__in`: não prova os filtros ORM reais. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:243`)
- O teste 09 extrai cinco instruções e fornece `_abono`, obrigatórios e previsto artificialmente: não testa a seleção que produz A2 nem DSR. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:294`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:297`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:300`)

Esses testes têm mordida localizada; não são todos vacuamente verdes. A suíte, porém, não certifica a classe completa. ORM e RED histórico nesta cópia: **[nao verificado]**. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:6`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`)

**9. Classes que já reprovaram obra**

| Classe | Resposta |
|---|---|
| Segundo juiz duplicando juiz registrado | **Não demonstrado especificamente.** Há decisão adicional de cobertura depois do juiz, pelo filtro de cruzamento; isso continua explícito. (`app/ponto/services/efeito_ausencia.py:195`; `app/ponto/services/efeito_ausencia.py:205`; `app/ponto/turnos.py:2751`) |
| Segundo escritor fora da porta | **Não encontrado no delta/censo atribuível.** (`app/folha/tests/test_chokepoint_folha_gate.py:58`; `app/folha/tests/test_chokepoint_folha_gate.py:72`) |
| Relógio lido fora da borda | **Não demonstrado como novidade desta volta.** A cadeia nova recebe instantes explícitos; fechamento mantém seu relógio da passada. (`app/ponto/services/efeito_ausencia.py:202`; `app/ponto/services/fechamento.py:616`) |
| Derivação paralela de fonte única | **Sim, A3**, fallback de prontidão. (`app/folha/export.py:1100`) |
| Selo/RED sem mordida suficiente | **Sim, A5**, atribuição vazia indevida sobrevive. RED histórico independente **[nao verificado]**. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`) |
| Ordem de trava | **Não demonstrada falha nova.** O delta não acrescenta travas; concorrência completa **[nao verificado]**. (`app/ponto/services/fechamento.py:1149`; `app/ponto/services/fechamento.py:1165`) |
| Pouso antes do pré-requisito | **[nao verificado]** quanto a deploy/apply. O código permite recalcular 10 exportada; a guarda específica só lista 09. (`app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:539`; `app/ponto

[truncado em 20000 de 21630 caracteres]

## 14/11 17:17 · sessao cc7cf9417762 · - · - · origem=Ronald (tmux)

1. **Teto ainda falha como classe — contracaso confirmado em memória.** Jornada 07–19, pausas 10–12 e 11–13, E07 ancorada: `minutos_previstos_do_dia` real devolve **480**; fonte real devolve **1.920**; `por_dia_da_grade` real entrega **1.920** de abono. A sequência conserva a ordem dos pares; ao encontrar 11h depois de 12h, empurra a segunda pausa e o fim para D+1 (`app/escala/utils.py:76`; `:345`; `:347`). O previsto desconta os dois comprimentos de pausa, sem encadear (`app/escala/utils.py:2291`; `:2324`; `:2332`). A fonte e os leitores não limitam o parcial ao previsto (`app/ponto/services/efeito_ausencia.py:154`; `app/ponto/services/dia_pago.py:398`; `app/ponto/motor_calculo_v2.py:2633`). A porta atribui o segundo par, enquanto o validador de intervalo dentro do turno só examina o primeiro (`app/escala/services/cadastro_tipo.py:232`; `app/escala/models.py:643`; `:656`). Ocorrência produtiva: [nao verificado].

2. **As duas pausas normais e a união de justificativas foram curadas por classe.** Fonte usa `pausas_do_dia` e `cadeia_do_dia`; medidor percorre todos os pares; união elimina sobreposições antes de somar (`app/ponto/services/efeito_ausencia.py:176`; `:177`; `:137`; `:144`; `:150`; `app/ponto/turnos.py:2792`).

3. **`dias_abono` continua com dois significados.** Novo contador anuncia “dias INTEIROS”, mas seleciona todo `tipo_dia == 'ausencia'` (`app/ponto/services/dia_pago.py:691`; `:701`). A grade também classifica ausência parcial como `ausencia`, mantendo `cobertura_parcial=True` (`app/escala/utils.py:1217`; `:1227`). O motor conta `_abono_inteiro` apenas entre dias sem trabalho (`app/ponto/motor_calculo_v2.py:2611`; `:2629`; `:2630`). Assim, não basta excluir justificativas parciais do contador para equiparar a semântica de todos os abonos.

4. **ABONA inteira precede o mapa parcial, mas sobra divergência entre motor e gravado com trabalho no dia.** Fonte remove o parcial (`app/ponto/services/efeito_ausencia.py:63`); motor não inclui dia trabalhado em `_abono`, portanto mapa vazio produz zero abono desse dia (`app/ponto/motor_calculo_v2.py:2611`; `:2628`; `:2634`). Grade classificada `ausencia` abona `previsto - realizado`, inclusive com trabalho (`app/ponto/services/dia_pago.py:394`; `:397`). Exemplo: previsto 660, realizado 180 → gravado 480; bloco do motor → zero. O teste isolado de precedência verifica mapa vazio e grade, sem exercitar o motor nessa composição (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:207`).

5. **Âncora inválida conserva pela fonte, sem testes por nome.** Predicado único cobre ausência de âncora, papel errado e turno fechado (`app/ponto/turnos.py:2694`; `:2700`; `:2703`); fonte converte motivo em `cobre=None` (`app/ponto/services/efeito_ausencia.py:192`; `:194`; `:131`). Outra data de turno torna a âncora inexistente (`:184`). Porém cobertura inicialmente válida ainda pode virar vazia pelo filtro de cruzamento (`:205`; `app/ponto/turnos.py:2764`). Nesse caso sai de `justificadas` e `min_justificados` (`app/ponto/services/efeito_ausencia.py:46`; `:47`). Isso conserva a distinção anterior registrada em v1, não demonstra 0/0 de frota.

6. **DSR Comercial permanece risco condicional comprovado nos predicados, sem prova do pareamento completo.** Executei fonte e atribuições reais do Comercial em memória: turno de D22–06; âncora E em D+1 00:10 atribuída a D; outro trecho fechado D+1 02–04. Fonte retorna cobertura vazia e conjuntos vazios pelo cruzamento (`app/ponto/services/efeito_ausencia.py:205`; `app/ponto/turnos.py:2764`). Comercial toma entrada civil D+1, encontra D obrigatório sem trabalho/justificativa e dá `dsr_ok=False` (`app/ponto/motor_calculo_v2.py:3477`; `:3478`; `:3489`; `:3490`; `:3491`). O juiz admite herança da entrada noturna para a véspera (`app/ponto/turnos.py:299`; `:308`), mas **o pareador integral e a ocorrência produtiva desse cenário permanecem [nao verificado]**. Base usa dia da jornada e inclui períodos abertos; vazio não demonstra automaticamente falta inteira (`app/ponto/motor_calculo_v2.py:2499`; `:2501`; `:2583`).

7. **Exemplo 22–06 / “justificativa 23–02” exige distinguir os modelos.** `Justificativa` tem `data_turno`, `tipo` e `saida_informada`; não tem início/fim de intervalo arbitrário (`app/ponto/models.py:144`; `:146`; `:149`). Juiz nomeia exclusivamente `hi` ou `hf` (`app/ponto/turnos.py:2668`). `esqueci_saida` ancorada E23 sem saída informada mede **23–06 = 420**; com saída informada D+1 02 mede **02–06 = 240**, sem pausas (`app/ponto/services/efeito_ausencia.py:197`; `:202`; `app/ponto/turnos.py:2738`). Quem tem período 23–02 é `Ausencia.hora_ini/hora_fim` (`app/ponto/models.py:768`; `:770`): fim recebe +1440 e a classificação de batidas inclui ambas as bordas (`app/ponto/turnos.py:2652`; `:2655`; `:2657`). Duração paga usa diferença entre instantes, sem minuto adicional; toque de extremidades não é cruzamento (`:2779`; `:2764`).

8. **Exemplo 07–19 / pausa 10–11 / saída informada 07:30:** fonte mede **630 minutos** após aceitar âncora; saída informada desloca a ponta por `max` e pausa é subtraída (`app/ponto/services/efeito_ausencia.py:192`; `:198`; `:202`; `app/ponto/turnos.py:2792`). Com batida real S07:30, o turno fechado conserva inteiro antes da medição (`app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:194`). Teste distingue essas duas situações (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:172`; `:321`).

9. **Duas ausências:** cobertura escolhe uma por dia, sobrescrevendo em ordem data/pk, portanto não soma abono inteiro dessas ausências (`app/ponto/turnos.py:2916`; `:2944`). Caminho distinto continua somando minutos de todas as ausências parciais para crédito e resumo PDF (`app/ponto/services/efeito_ausencia.py:49`; `:52`; `app/ponto/services/credito_parcial.py:26`; `:28`; `app/relatorios/pdf_espelho.py:480`). Impacto dessa soma no pagamento efetivo: [nao verificado].

## 14/11 17:16 · sessao 9408059f510d · - · - · origem=Ronald (tmux)

Achados concretos:

1. **Prontidão conserva derivação paralela.** O caminho com snapshot soma `FechamentoMensal.minutos_abonados`; o fallback soma somente o previsto dos dias `ausencia`, ignorando `min_justificados` de dias `trabalho`. Exemplo sintético: previsto 660, realizado 0, justificativa parcial 630 → snapshot 95%; fallback 0%. (`app/folha/export.py:1078`, `:1079`, `:1095`, `:1100`, `:1106`, `:1111`; gravação dos 630: `app/ponto/services/dia_pago.py:398`, `app/ponto/services/fechamento.py:1067`)

2. **`dias_abono` continua com significados diferentes fora do caso parcial isolado.** O motor conta dias justificados sem trabalho, excluindo o mapa parcial. A lavratura agora conta qualquer linha `DiaPago` cuja grade atual seja `ausencia`, sem exigir minutos abonados positivos nem excluir ausência parcial. Uma ausência com par trabalhado pode dar motor 0 e lavratura 1; mudança posterior da grade altera o contador sem relavrar. (`app/ponto/motor_calculo_v2.py:2611`, `:2629`, `:2630`; `app/ponto/services/dia_pago.py:698`, `:701`, `:702`; transporte para resumo: `app/relatorios/cartao_pela_celula.py:188`)

3. **8932 não mede justificativa parcial.** `tipos_abonados_8932` consulta tipos do catálogo; `datas_abonadas` expande registros `Ausencia`, sem ler `Justificativa`, seu período ou minutos lavrados. Uma justificativa parcial isolada não entra nessa resposta. A pesquisa integral encontrou apenas testes chamando `datas_abonadas`; emissão efetiva da 8932 por esse caminho: **[nao verificado]**. (`app/folha/export.py:193`, `:198`, `:201`, `:204`, `:205`, `:146`, `:160`)

4. **Ausência parcial continua com outra aritmética.** A soma de `a.minutos` percorre todos os registros, sem união de períodos; `creditos_por_dia` também soma todos por dia. O PDF acrescenta essa soma ao abono do motor. Assim, a cura da união de justificativas não cura sobreposição de duas `Ausencia` parciais. (`app/ponto/services/efeito_ausencia.py:49`, `:52`; `app/ponto/services/credito_parcial.py:26`, `:28`; `app/relatorios/pdf_espelho.py:336`, `:480`)

Censo completo dos sítios de produção encontrados pela busca literal:

| Campo/função | Escritor/composição | Leitores/alimentadores |
|---|---|---|
| `periodos_justificados` | `app/ponto/services/efeito_ausencia.py:72`, `:127`, `:140` | efeito `:45`; `app/escala/utils.py:1198`, `:1251`, `:1257`, `:1266`, `:1268`, `:1275`, `:1276` |
| `intervalos_do_periodo_justificado` | `app/ponto/turnos.py:2783` | wrapper `:2780`; fonte `app/ponto/services/efeito_ausencia.py:129`, `:207` |
| União de intervalos | `app/ponto/services/efeito_ausencia.py:144` | `:140` |
| Mapa filtrado `min_justificados` | `app/ponto/services/efeito_ausencia.py:47`; retirada por ABONA inteira `:63`; retorno `:68` | fechamento `app/ponto/services/fechamento.py:667`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407`; esmeril `app/ponto/services/esmeril_espelho.py:166`; extrato `app/relatorios/management/commands/diff_extrato_parcial.py:62`; diff espelho `app/ponto/management/commands/diff_espelho_alimentacao.py:124`; grade `app/escala/services/leitor_celula.py:365` |
| `min_justificados` na linha | `app/escala/utils.py:1276`; `app/escala/services/leitor_celula.py:436` | `app/ponto/services/dia_pago.py:398`, `:399` |
| `minutos_justificados` no motor | init `app/ponto/motor_calculo_v2.py:813`; fechamento normal/órfão `app/ponto/services/fechamento.py:755`, `:831`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407`; diff `app/ponto/management/commands/diff_espelho_alimentacao.py:56`; extrato `app/relatorios/management/commands/diff_extrato_parcial.py:62`; esmeril `app/ponto/services/esmeril_espelho.py:166` | motor `app/ponto/motor_calculo_v2.py:2628`, `:2634`; contrato nominal `app/ponto/calculador/nucleo.py:424` |
| `dias_com_abono` | cálculo `app/ponto/services/dia_pago.py:690`, `:701`, `:702` | único chamador de produção localizado: `app/relatorios/cartao_pela_celula.py:188` |
| `minutos_abonados` | fechamento `app/ponto/services/fechamento.py:1067`; lavratura `app/ponto/services/dia_pago.py:288`, `:292`; retrato `:467`, `:479`, `:484` | cartão `app/relatorios/cartao_pela_celula.py:118`, `:186`; prontidão `app/folha/export.py:1078`; conferência pela mesma função `app/folha/porta_export.py:139` |

Os três chamadores laterais migraram para a fonte: diff espelho exige mapa explícito e o recebe no degrau D; extrato e esmeril alimentam justificadas e minutos antes de calcular. A/B/C do diff mantêm mapas explicitamente vazios. (`app/ponto/management/commands/diff_espelho_alimentacao.py:42`, `:56`, `:109`, `:111`, `:113`, `:117`, `:124`; `app/relatorios/management/commands/diff_extrato_parcial.py:41`, `:57`, `:62`; `app/ponto/services/esmeril_espelho.py:161`, `:162`, `:166`)

A cadeia até o gravado está conectada no código: grade transporta o número; `por_dia_da_grade` o lê; fechamento soma o mapa e fornece o mesmo mapa à lavratura. Execução ORM e igualdade produtiva: **[nao verificado]**. (`app/escala/services/leitor_celula.py:435`, `:436`; `app/ponto/services/dia_pago.py:398`; `app/ponto/services/fechamento.py:1066`, `:1067`, `:1217`)

## 14/11 17:16 · sessao 9cc2d627f055 · - · - · origem=Ronald (tmux)

Resultados do explorer, somente leitura:

- Os 15 testes sem banco passam; os 14 mutantes declarados morrem por asserção. A suíte carrega funções reais por AST, substituindo as bordas ORM; o runner exige falha sem erro (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:38`; `app/ponto/tests/mutantes_justificativa_periodo_v2.py:76`). Execuções locais confirmadas, sem artefato escrito.
- Há mordida de classe na união de intervalos sobrepostos/disjuntos, duas pausas, duas âncoras, precedência ABONA e transporte à grade; não é apenas comparação com o golden (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:186`, `:192`, `:201`, `:207`, `:258`).
- **Mutante plausível sobrevivente comprovado em memória:** trocar `ef['min_justificados']` por `{}` em `app/ponto/services/esmeril_espelho.py:166`. Todos os 15 continuam verdes quando a alteração antecede as mutações internas do teste 12. O selo só verifica Store e ordem textual, sem verificar RHS nem percurso (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:83`, `:90`). O mutante declarado só apaga a atribuição, deixando essa classe descoberta (`app/ponto/tests/mutantes_justificativa_periodo_v2.py:42`).
- O teste 06 usa motor falso que devolve o próprio atributo; prova contrato de argumento/transporte, **não** cálculo do motor (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:226`, `:235`). O teste 07 usa contador falso limitado a `data__in`; não prova filtros reais de colaborador, versão, tipo ou positividade (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:243`). Esses testes têm mordida localizada, mas não sustentam certificação financeira integral.
- O teste 09 executa somente cinco instruções extraídas do bloco `_mj`, com previsto falso; não executa seleção dos dias obrigatórios, DSR ou motor completo (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:293`, `:297`, `:299`). A suíte explicita não provar ORM/frota/DSR/TXT (`:6`).
- O teste ORM novo confere fechamento e DiaPago reais, mas só roda fechamento **uma vez**; não prova idempotência (`app/ponto/tests/test_justificativa_paga_o_periodo.py:398`, `:406`, `:408`). Não foi executado: import local de `django.core` confirmou `ModuleNotFoundError`; resultado ORM [nao verificado].
- **Estado integral duas vezes não é idêntico:** renovam-se timestamps de fechamento e DiaPago é apagado/recriado, mudando identidade/carimbo (`app/ponto/services/fechamento.py:1091`, `:1148`, `:1166`, `:1194`; `app/ponto/services/dia_pago.py:359`; `app/ponto/models.py:380`). Igualdade dos números sob entradas fixas no fechamento completo: [nao verificado].
- Executei o censo puro existente: zero escrita atribuída a FechamentoMensal fora da porta; o contrato e a porta estão em `app/folha/tests/test_chokepoint_folha_gate.py:58`, `:72`. O censo tem limites explícitos para escritas não atribuíveis (`app/core/censo_escritas.py:43`). No delta fornecido, nenhum novo escritor persistente.
- Não há trava nova no delta. Fechamento não possui `select_for_update` ou `atomic` próprio no percurso principal; acusar mudança de ordem de trava nesta volta não está demonstrado (`app/ponto/services/fechamento.py:448`; gravações `:1149`, `:1165`). Operações concorrentes completas [nao verificado].
- A guarda existente protege **somente 09/2026** (`app/ponto/services/fechamento.py:36`). 10 exportada recebe aviso, mas pode ser regravada se o chamador executar (`:539`, `:542`, `:1142`, `:1165`). Apply/deploy efetivo [nao verificado].
- Sem `.git` na cópia: o RED histórico, que exige `git show`, não pode ser reproduzido aqui (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`). Delta anexado não toca o motor; presença no diff inteiro e inventário exato histórico precisam ser sustentados pelo anexo v1 ou marcados [nao verificado]. No hash há init do memo e leitura parcial (`app/ponto/motor_calculo_v2.py:813`, `:2628`, `:2633`).

## 14/11 17:11 · sessao 750d1e2f857d · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita na cópia. **Certificação pelo Code/opus permanece pendente.** O teste ORM está escrito, mas sua execução ficou bloqueada por `ModuleNotFoundError: No module named 'django.core'` em `manage.py:11`; resultado ORM [nao verificado]. [nao verificado]

O diff por arquivo::função:

| Arquivo::função | Mudança |
|---|---|
| `ponto/services/efeito_ausencia.py::periodos_justificados` | Une intervalos antes de medir (`:139`, `:144`). |
| `efeito_ausencia.py::_cobertura_de_uma` | Usa `cadeia_do_dia` com todas as pausas; considera a saída informada após aceitar a âncora (`:176`, `:201`). |
| `efeito_ausencia.py::efeitos_da_ausencia` | Ausência inteira ABONA retira o dia do mapa parcial (`:63`). |
| `ponto/turnos.py::intervalos_do_periodo_justificado` | Extrai os trechos sem pausas para a união na fonte (`:2783`). |
| `escala/services/leitor_celula.py::grade_da_celula` | Transporta o número da fonte até a grade usada pelo fechamento (`:365`, `:436`). |
| `ponto/services/dia_pago.py::por_dia_da_grade` | Abona também `trabalho`, lendo `min_justificados` da linha (`:397`). |
| `dia_pago.py::dias_com_abono` | Conta dias lavrados classificados como `ausencia` (`:701`). |
| `ponto/services/fechamento.py::recalcular_fechamento_mes` | Lê o mapa filtrado de `efeitos_da_ausencia`, eliminando a derivação própria (`:667`). |
| `diff_espelho_alimentacao.py::_motor/handle` | Exige mapa explícito e alimenta o degrau D pela fonte (`:42`, `:124`). |
| `diff_extrato_parcial.py::_antes` | Alimenta justificadas e mapa pela fonte (`:43`, `:62`). |
| `ponto/services/esmeril_espelho.py::assinaturas_do_colab` | Alimenta justificadas e mapa pela fonte (`:161`, `:166`). |
| `relatorios/cartao_pela_celula.py::totais_da_lavratura` | Lê a contagem compartilhada de dias inteiros (`:188`). |

Os casos e seus números estão registrados nos testes: [nao verificado]

| Caso | Resposta exigida |
|---|---|
| E07:00, saída **informada** 07:30, pausa 10–11 | **630 min** — `test_justificativa_periodo_v2_sem_banco.py:172` |
| 07–19, duas pausas de 85 min | **550**, contra 635 na base — `:186` |
| E07:30/S11/E12, duas âncoras | **420**, contra 840 na base — `:192` |
| ABONA inteira junto de parcial | Mapa parcial `{}`; abono **660−180=480** — `:207` |
| Inteiro junto de parcial | **720+630=1.350**, um dia inteiro — `:280` |
| Controle do motor completo | **720+690=1.410** — `test_justificativa_paga_o_periodo.py:268` |
| Fechamento real e DiaPago | Ambos devem gravar **630** — `test_justificativa_paga_o_periodo.py:367`; execução [nao verificado] |

O caso com **batida S07:30** continua conservando o inteiro, conforme o aval; ele é distinguido da saída apenas informada (`test_justificativa_periodo_v2_sem_banco.py:321`).

O selo mantém a distinção da casa: atributo ausente é cobrado por `exige_memos`; omissão do chamador é cobrada pela alimentação explícita antes do cálculo. `{}` informado continua legítimo (`ponto/calculador/nucleo.py:424`; `ponto/tests/test_s5b_porta_unica.py:985`; `test_justificativa_periodo_v2_sem_banco.py:220`).

Comandos executados e resultados locais:

```bash
# RED: lê os arquivos da base informada, sem restaurá-los no disco
PYTHONDONTWRITEBYTECODE=1 JPP_BASE=1 python3 -m unittest ponto.tests.test_justificativa_periodo_v2_sem_banco -q
# Ran 15 tests — FAILED (failures=15)

# GREEN isolado
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_periodo_v2_sem_banco -q
# Ran 15 tests — OK

# Mutantes em memória
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 --todos
# 14/14 mortos por assercao, sem escrever fontes
```

O carregamento da base está em `test_justificativa_periodo_v2_sem_banco.py:27`; os mutantes e seus testes estão em `mutantes_justificativa_periodo_v2.py:12`. Cada mutante também tem comando individual: [nao verificado]

```bash
for mutante in \
  ignora_saida_informada gravado_ignora_periodo \
  fonte_so_primeira_pausa medidor_so_primeira_pausa soma_ancoras \
  ausencia_nao_precede fechamento_rederiva_mapa \
  diff_sem_mapa extrato_sem_mapa esmeril_sem_mapa diff_aceita_omissao \
  contador_inclui_trabalho grade_perde_periodo motor_velho_em_memoria
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 "$mutante" || exit 1
done
```

O comando Django deixado para a sessão é:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_justificativa_paga_o_periodo \
  ponto.tests.test_justificativa_periodo_v2_sem_banco \
  ponto.tests.test_s5b_porta_unica \
  relatorios.tests.test_s3_cartao_le_dia_pago \
  escala.tests.test_grade_alimentada \
  escala.tests.test_cobparc7_leitor_transporta_a_lampada \
  --settings=config.settings_test --noinput
```

Depois da cura, o censo fica:

| Campo | Quem escreve/alimenta | Quem lê |
|---|---|---|
| `min_justificados` | Fonte: `efeito_ausencia.py:47`, `:63`; grades: `escala/utils.py:1276`, `leitor_celula.py:436` | `dia_pago.py:397`; `fechamento.py:667`; alimentadores do motor |
| `minutos_justificados` | Init: `motor_calculo_v2.py:813`; fechamento `:755`, `:831`; espelho `:1134`; PDF `:407`; laterais acima | `motor_calculo_v2.py:2628` |
| `minutos_abonados` | Fechamento: `fechamento.py:1067`; DiaPago: `dia_pago.py:288`; retrato: `:484` | `cartao_pela_celula.py:118`, `:186`; prontidão: `folha/export.py:1078` |
| `dias_abono` | Motor: `motor_calculo_v2.py:2630`; lavratura: `dia_pago.py:701` | Espelho `:1253`; PDF `:479`, `:787`; cartão `:188` |
| `horas_abono` | Motor: `motor_calculo_v2.py:2631`, `:2633`; resumo por minutos gravados: `cartao_pela_celula.py:118`, `:186` | PDF: `pdf_espelho.py:480` |

`folha/export.py` permanece com os leitores existentes: prontidão usa `minutos_abonados`; seu fallback ainda soma o previsto inteiro dos dias `ausencia` (`:1078`, `:1100`). A rubrica TXT 8932 continua derivando dos tipos de Ausência (`:193`, `:202`). Igualdade dos hashes exportados e pagamento efetivo [nao verificado].

O DSR Comercial continua usando data civil da entrada e `self.datas_justificadas`; a cura alcança sua alimentação, sem corrigir essa rederivação (`motor_calculo_v2.py:3477`, `:3478`, `:3490`).

Não houve implementação em motor, porta da célula, juízes, models/migrations, export, crons ou docs; o diff protegido foi conferido vazio. Não executei banco, Docker, rede, commit, merge, push, deploy ou apply. **Frota 0/0, hashes das competências exportadas, multiplicidade dos 45 e contagens do admin por motivo: [nao verificado].** [nao verificado]

## 14/11 17:00 · sessao 8fb89e816b92 · E3-V10-DEGRAU-PUB · censo · origem=Code

**Nenhum B identificado nos 40 testes.** [não verificado: conclusão da triagem estática, sem reexecução] [nao verificado]

Referências relativas a `app/`. Nomes dos testes sem o prefixo `test_`. [nao verificado]

Regras usadas: **1** = responder + pergunta pendente no app, independentemente da âncora/prazo (`chamados/services/tarefa_do_dia.py:43`); **2** = ação administrativa/sistema alcançável (`:45`); **3** = residual sem `cobrar` (`:46`). **4*** = publicar rota somente com dono (`chamados/services/tarefa_do_dia.py:612`); sua numeração literal como item 4 é **[não verificado]**, pois o topo enumera apenas 1–3.

| teste | arquivo:linha do assert/erro | CLASSE | o que mudar e onde | [nao verificado]
|---|---|---|---|
| MORDE_o_universo_e_o_mesmo_do_fora_da_folha | chamados/tests/test_n1_alimentacao_unica.py:127 | F | Helper `_conta_pda`, em `:90`, precisa receber/repassar `em_lote` e `agora`; assinatura exigida em `chamados/services/pergunta_no_app.py:74`. Preservar igualdade dos universos. |
| RED_a_porta_e_alimentada_uma_vez_por_rodada | chamados/tests/test_n1_alimentacao_unica.py:99 | F | Mesmo helper `:90`: repassar os argumentos novos. Preservar alimentação única em `:101`; consulta do app é memorizada separadamente por colab (`chamados/services/tarefa_do_dia.py:551`). |
| RED_o_que_esta_no_app_se_pergunta_uma_vez_por_colaborador | chamados/tests/test_n1_alimentacao_unica.py:106 | F | Corrigir helper `:90`; depois, regra **1**: trocar expectativa zero de `:107` por uma consulta por colab utilizado na carga, constante em dias (`chamados/services/tarefa_do_dia.py:551`). |
| RED_o_que_segura_semeia_o_mapa_com_o_colaborador_pedido | chamados/tests/test_n1_alimentacao_unica.py:147 | F | `_le`, em `:141`, deve repassar kwargs. Regra **1**: atualizar zero leituras em `:149`; preservar proibição de busca individual em `:153` (`chamados/services/pergunta_no_app.py:59`). |
| a_saida_do_dia_nao_muda | chamados/tests/test_n1_alimentacao_unica.py:111 | F | Corrigir `_conta_pda` em `:90`. Regra **1** conserva a porta existente neste cenário pendente; manter verificações de saída (`chamados/tests/test_n1_alimentacao_unica.py:113`). |
| v8_R10_ancora_fora_do_fato_sem_acao | chamados/tests/test_e3_portas_tarefa.py:601 → chamados/services/tarefa_do_dia.py:551 | F | Inicializar `_pendentes_app` no objeto criado por `__new__`, `chamados/tests/e3_v8_apoio.py:127`; fornecer pendentes vazios ou mock de consulta. Inicialização real em `chamados/services/tarefa_do_dia.py:388`. |
| lote_responde_o_mesmo_que_o_avulso_e_com_queries_constantes | chamados/tests/test_e2_tarefa_do_dia.py:1180 | F | Separar custo do lote e nova consulta por colab; preservar igualdade lote/avulso. Parcela nova constante com dias, crescente com colabs; não fixar simplesmente 26 (`chamados/services/tarefa_do_dia.py:551`). |
| lote_tem_queries_constantes | chamados/tests/test_e2_tarefa_do_dia.py:938 | F | Comparação altera simultaneamente dias e colabs. Medir dias com colabs fixos e contabilizar consulta por colab; não substituir por teto 45 (`chamados/tests/test_e2_tarefa_do_dia.py:931`; `chamados/services/tarefa_do_dia.py:551`). |
| RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal | chamados/tests/test_e3_bloqueantes.py:168 | L | **1**: novo `porta_app/colab`; atualizar também `:170` para excluir dos sem canal (`chamados/services/tarefa_do_dia.py:43`). |
| RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal | chamados/tests/test_e3_bloqueantes.py:161 | L | **1**: novo `porta_app/colab`; atualizar também `:163` para excluir dos sem canal (`chamados/services/tarefa_do_dia.py:43`). |
| classe_e_viva_sob_terminal_reabre_pela_porta_com_nota | chamados/tests/test_e4_passivo.py:73 → chamados/tests/test_e4_backfill.py:112 | L | **1**: pendente sob terminal já tem `porta_app/colab`; esperar plano desse dia zero, sem reabertura/nota. Atualizar caracterização em `:73` (`chamados/services/tarefa_do_dia.py:43`; `chamados/services/vigia_fio.py:658`). |
| mutante_sem_porta_morde_caracterizacao_classe_e | chamados/tests/test_e4_passivo.py:90 | L | **1**: neste cenário, reabrir credor deixou de ser necessário. Reenquadrar teste de `:87` para verificar ausência dessa chamada; trocar apenas regex esconderia a mudança (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_dry_nao_escreve | chamados/tests/test_f3_beco1_reata_limbo.py:72 | L | **1**: novo zero candidatos/pais C11; preservar ausência de escrita. Para exercitar DRY com candidatos, montar cenário realmente sem porta (`escala/services/cadastro_realidade.py:469`). |
| MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata | chamados/tests/test_f3_beco1_reata_limbo.py:159 | L | **1**: pergunta já tem porta do app; novo zero reatas. Refazer cenário sem porta para continuar cobrindo silêncio vencido (`escala/services/cadastro_realidade.py:469`). |
| MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce | chamados/tests/test_f3_beco1_reata_limbo.py:82 | L | **1**: novo sem linha `SEM PAI`, pois não entra em C11. Preservar teste de ausência de pai com cenário sem porta (`escala/services/cadastro_realidade.py:469`). |
| RED_apply_reata_ao_pai_e_o_dia_sai_do_beco | chamados/tests/test_f3_beco1_reata_limbo.py:53 | L | **1**: novo `_c11()==[]` antes do apply; zero reatas. Refazer cenário sem porta para testar costura (`escala/services/cadastro_realidade.py:469`). |
| RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata | chamados/tests/test_f3_beco1_reata_limbo.py:145 | L | **1**: cenário atual não seleciona C11, portanto não alcança a corrida. Criar candidato sem porta e preservar guarda contra reata após silêncio (`escala/services/cadastro_realidade.py:469`). |
| RED_pai_silenciado_fora_da_fila_nao_recebe_reata | chamados/tests/test_f3_beco1_reata_limbo.py:119 | L | **1**: novo sem linha `PAI FORA DA FILA`; há porta do app. Refazer cenário sem porta para conservar essa cobertura (`escala/services/cadastro_realidade.py:469`). |
| MORDE_F5_queries_nao_crescem_com_os_dias | escala/tests/test_f3_beco1_folga_na_lista.py:322 | L | **1**: novo `(d1,d5)=(0,0)`. Refazer cenário C11 sem porta antes de medir queries; consulta nova fica uma por colab, constante com dias (`escala/services/cadastro_realidade.py:469`; `chamados/services/tarefa_do_dia.py:551`). |
| MORDE_competencia_anterior_NAO_exportada_vai | escala/tests/test_f3_beco1_folga_na_lista.py:110 | L | **1**: novo C11 `[]`; competência anterior não elimina porta da pergunta pendente (`chamados/services/tarefa_do_dia.py:43`; `escala/services/cadastro_realidade.py:469`). |
| RED_F1_lista_le_o_instante_de_quem_chama | escala/tests/test_f3_beco1_folga_na_lista.py:219 | L | **1**: novo C11 `[]` nesse instante. Refazer cenário sem porta para preservar prova do relógio (`escala/tests/test_f3_beco1_folga_na_lista.py:215`; `escala/services/cadastro_realidade.py:469`). |
| RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa | escala/tests/test_f3_beco1_folga_na_lista.py:265 | L | **1** remove C11; novo conjunto `{('C9','BATIDA')}`, preservando dono lavrado. Refazer C11 sem porta para continuar testando reconciliação de donos (`escala/tests/test_f3_beco1_folga_na_lista.py:262`; `escala/services/cadastro_realidade.py:988`). |
| RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta | escala/tests/test_f3_beco1_folga_na_lista.py:180 | L | **1**: manter rota/fila/verbos; novo `fio='colab'` pela pergunta pendente sob outra âncora. C11 continua vazio (`escala/tests/test_f3_beco1_folga_na_lista.py:173`; `chamados/services/tarefa_do_dia.py:43`). |
| RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta | escala/tests/test_f3_beco1_folga_na_lista.py:204 | L | **1**: novo C11 `[]`; pendentes sob terminal/registrado têm porta do app (`escala/tests/test_f3_beco1_folga_na_lista.py:193`; `chamados/services/tarefa_do_dia.py:43`). |
| RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO | escala/tests/test_f3_beco1_folga_na_lista.py:88 | L | **1**: novo C11 `[]`, sem dono C11. Refazer cenário sem porta para testar destino CADASTRO (`escala/services/cadastro_realidade.py:469`). |
| MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta | chamados/tests/test_o261e_f5b_juiz_porta.py:272 | L | **1**: novo fio da folha `colab`, com `porta_app`; arquivamento da âncora não impede resposta pendente (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_a_porta_vem_da_TAREFA_e_nao_do_app | chamados/tests/test_o261e_f5_alcance.py:256 | L | **1, 4***: carga deve consultar app; mock vazio em `:253` implica `nada/None`. Atualizar também contadores em `:255`, além de zero→uma consulta (`chamados/services/tarefa_do_dia.py:551`; `:616`). |
| a_ancora_VIVA_fora_da_fila_tambem_e_veu | chamados/tests/test_o261e_f5_alcance.py:460 → helper :134 | L | **1**: novo `porta_app/colab`; retirar expectativa de causa sem porta `a/registrado` (`chamados/services/tarefa_do_dia.py:43`). |
| b_respondida_com_resposta_VAZIA_e_porta_do_COLABORADOR_pela_tarefa | chamados/tests/test_o261e_f5_alcance.py:196 | L | **1, 4***: carimbo respondido exclui dos pendentes; novo `nada/None`, não colab. A cópia já suprime a rota sem dono exibida no anexo (`chamados/services/pergunta_no_app.py:75`; `chamados/services/tarefa_do_dia.py:616`). |
| e_pergunta_viva_sob_chamado_TERMINAL_tem_nome | chamados/tests/test_o261e_f5_alcance.py:222 → helper :134 | L | **1**: novo `porta_app/colab`; não pertence à lista sem porta da causa `e` (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_a_frase_do_fio_nada_nao_afirma_o_que_o_juiz_nao_garante | folha/tests/test_haiku_fora_da_folha.py:173 | M | **4***: renovar substrings para texto `SEM PORTA`, preservando garantia de ação alcançável (`folha/fora_da_folha.py:54`; `chamados/services/tarefa_do_dia.py:612`). |
| o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA | folha/tests/test_haiku_fora_da_folha.py:81 | M | **4***: `SEM FIO`→`SEM PORTA`; preservar verificação `fio=nada` (`folha/fora_da_folha.py:54`; `chamados/services/tarefa_do_dia.py:612`). |
| endpoint_do_app_filtra_por_q_vivas | chamados/tests/test_contract_pergunta_viva.py:76 | M | Atualizar string para `perguntas_do_app(colab, em_lote=True, agora=agora)`; conservar lote/C6 e encaminhamento do relógio (`chamados/services/pergunta_no_app.py:74`). |
| todo_mutante_da_E3_tem_alvo_unico | chamados/tests/test_e3_mutantes_alvo.py:25 | M | Retargetar `consulta_vazia` e `consulta_sem_lote` para assinatura com `agora=agora`. **Já ajustados na cópia**; manter unicidade (`chamados/tests/e3_mutantes.py:34`; `:59`). |
| cobrar_precede_validar_e_decidir_sem_depender_dos_donos | chamados/tests/test_e3_portas_tarefa.py:478 | L | **2, 3**: novo `fio='admin'`; conservar fila/verbos. `cobrar` residual não dá dono colab (`chamados/services/tarefa_do_dia.py:45`; `:46`). |
| fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar | chamados/tests/test_e3_portas_tarefa.py:457 | L | **1, 4***: entrada não fornece pergunta pendente; novo `nada/None`. Esse `validar` está na porta da ação, não no residual (`chamados/services/tarefa_do_dia.py:584`; `:606`). |
| RED_A_na_fila_aparece_com_acao_sob_B | chamados/tests/test_e3_portas_tarefa.py:413 | L | **1, 3**: novo `fio='chamado'` pelo residual `decidir`; conservar rota/fila/verbos. Não há pendente fornecida (`chamados/tests/test_e3_portas_tarefa.py:411`; `chamados/services/tarefa_do_dia.py:46`). |
| v8_R1_colab_alcancavel_e_leitores | chamados/tests/test_e3_portas_tarefa.py:490 | L | **1, 4***: entrada atual dá `nada/None`. Para preservar cenário nominal colab, fornecer `pendentes_app={303}` em `:489`; então colab é justificado pela regra **1** (`chamados/services/tarefa_do_dia.py:43`). |
| v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia — ambos subtestes | chamados/tests/test_e3_portas_tarefa.py:514 | L | **1, 2**: entrada atual dá `porta_fila/admin`, mantendo verbos/ações. Para provar simultaneidade com colab, fornecer identidade da pergunta e pendentes em `:509`/`:513` (`chamados/services/tarefa_do_dia.py:43`; `:45`). |
| alvos | chamados/tests/test_e3_volta7_sem_banco.py:92 → chamados/tests/test_e3_mutantes_alvo.py:25 | M | Mesmo selo delegado: conservar execução; alvos com relógio já atualizados na cópia (`chamados/tests/e3_mutantes.py:34`; `:59`). |

**B: nenhum identificado. Contagem: L=27; F=8; M=5; B=0; ?=0 — 40 testes distintos.** [não verificado por execução; consolidação das linhas acima] [nao verificado]

## 14/11 16:59 · sessao 47b7900a56ce · - · - · origem=Ronald (tmux)

| Teste | Assert/erro | Classe | Mudança justificada | [nao verificado]
|---|---|---|---|
| `test_v8_R10_ancora_fora_do_fato_sem_acao` | `app/chamados/tests/test_e3_portas_tarefa.py:601`; erro `app/chamados/services/tarefa_do_dia.py:551` | F | Inicializar `_pendentes_app={1: frozenset()}` no helper que usa `__new__`, `app/chamados/tests/e3_v8_apoio.py:127`. A carga real inicializa o memo em `app/chamados/services/tarefa_do_dia.py:388`. Não introduz queries no helper; na carga real, consulta uma vez por colab, constante com os dias, crescente com colabs (`app/chamados/services/tarefa_do_dia.py:551`). |
| `test_RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal` | `app/chamados/tests/test_e3_bloqueantes.py:161` | L | Item 1: pergunta pendente com responder tem porta independente da âncora; novo `rota='porta_app'`, `fio='colab'` (`app/chamados/services/tarefa_do_dia.py:43`). Atualizar também a exclusão do furo: `assertNotIn` em `app/chamados/tests/test_e3_bloqueantes.py:163`; porta publicada sai dos sem canal (`app/chamados/services/furo_sem_canal.py:104`). |
| `test_RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal` | `app/chamados/tests/test_e3_bloqueantes.py:168` | L | Item 1: novo `porta_app/colab`, independentemente da âncora registrada (`app/chamados/services/tarefa_do_dia.py:43`). Atualizar também para `assertNotIn` em `app/chamados/tests/test_e3_bloqueantes.py:170`, conforme consumo da porta em `app/chamados/services/furo_sem_canal.py:98`. |
| `test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | `app/chamados/tests/test_e4_passivo.py:73` → helper `app/chamados/tests/test_e4_backfill.py:112` | L | Item 1: responder pendente abre `porta_app/colab` sem reabrir terminal (`app/chamados/services/tarefa_do_dia.py:43`). Novo cenário: plano desse dia `0`, sem reabertura/nota; adaptar verificações em `app/chamados/tests/test_e4_passivo.py:73`. O vigia trata porta antes da classe sem porta (`app/chamados/services/vigia_fio.py:658`). |
| `test_mutante_sem_porta_morde_caracterizacao_classe_e` | `app/chamados/tests/test_e4_passivo.py:90` | L | Mesma mudança do item 1: reabrir credor deixou de ser necessário para esta pendente (`app/chamados/services/tarefa_do_dia.py:43`). Reenquadrar o selo em `app/chamados/tests/test_e4_passivo.py:87` para verificar que a porta de reabertura não é chamada; não basta trocar o regex, pois o alvo comportamental mudou (`app/chamados/services/vigia_fio.py:658`). |
| `test_RED_A_na_fila_aparece_com_acao_sob_B` | `app/chamados/tests/test_e3_portas_tarefa.py:413` | L | Itens 1 e 3: sem pergunta pendente fornecida, responder não dá colab; residual A `decidir` dá `chamado`. Novo `('porta_fila',(101,),{101:'decidir',202:'cobrar'},'chamado')`; modificar expectativa em `app/chamados/tests/test_e3_portas_tarefa.py:414`, pelas regras `app/chamados/services/tarefa_do_dia.py:43` e `:46`. |
| `test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar` | `app/chamados/tests/test_e3_portas_tarefa.py:457` | L | Itens 1 e 4: insumo atual não informa pergunta pendente; novo `fio='nada'`, `rota=None` (`app/chamados/services/tarefa_do_dia.py:43`, `:612`). `validar` pertence à porta da ação, portanto não é residual separado (`app/chamados/services/tarefa_do_dia.py:584`, `:606`). Alterar expectativa em `app/chamados/tests/test_e3_portas_tarefa.py:457`. |
| `test_cobrar_precede_validar_e_decidir_sem_depender_dos_donos` | `app/chamados/tests/test_e3_portas_tarefa.py:478` | L | Itens 2 e 3: validar/admin alcançável permanece; cobrar residual não cria colab. Novo mesmo `fila/verbos`, `fio='admin'`; modificar `app/chamados/tests/test_e3_portas_tarefa.py:479`, conforme `app/chamados/services/tarefa_do_dia.py:45`. |
| `test_v8_R1_colab_alcancavel_e_leitores` | `app/chamados/tests/test_e3_portas_tarefa.py:490` | L | Itens 1 e 4: embora tenha `pergunta_id=303`, não fornece pendentes; novo `fio='nada'`, `rota=None`, `porta_da_tarefa=None`, mantendo verbos (`app/chamados/services/tarefa_do_dia.py:43`, `:612`). Atualizar assert e projeções em `app/chamados/tests/test_e3_portas_tarefa.py:490`. Para preservar o cenário nominal alcançável, fornecer explicitamente `pendentes_app={303}` na chamada de `:489`. |
| `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia` — ambos subtestes | `app/chamados/tests/test_e3_portas_tarefa.py:514` | L | Itens 1 e 2: fixture não fornece pergunta identificada nem pendentes; `dia_plano` usa `pergunta_id=None` (`app/chamados/tests/e3_portas_apoio.py:29`). Novo `('porta_fila','admin',verbos)` em ambos, mantendo ações; mudar `app/chamados/tests/test_e3_portas_tarefa.py:514`, conforme `app/chamados/services/tarefa_do_dia.py:43`. Para continuar provando simultaneidade com colab pendente, fornecer identidade e `pendentes_app` em `:509`/`:513`. |

Nestes **10 testes distintos: L=9, F=1, B=0**; R2 aparece duas vezes por subtestes (`app/chamados/tests/test_e3_portas_tarefa.py:507`). A cópia já impede publicar rota com `fio=nada`, embora o anexo anterior mostre `porta_pergunta/nada` (`app/chamados/services/tarefa_do_dia.py:616`).

O topo enumera somente itens 1–3 (`app/chamados/services/tarefa_do_dia.py:43`); usei “item 4” como a regra explícita do aval 16:16 sobre publicação somente com dono, registrada em `app/chamados/services/tarefa_do_dia.py:612`. A numeração literal 1–4 no corte 16:16 permanece [não verificado].

## 14/11 16:58 · sessao af390de1e0e6 · - · - · origem=Ronald (tmux)

Na cópia, os itens numerados são apenas 1–3; a regra de publicação às 16:16 aparece separadamente, sem número (`app/chamados/services/tarefa_do_dia.py:43`, `:45`, `:46`, `:612`). O item 4 literal em CORTES é [não verificado].

| Teste | Assert/erro na cópia | Classe | Mudança justificada | [nao verificado]
|---|---|---|---|
| `test_MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta` | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:272` | L | Item 1: pergunta pendente + responder dá `colab`, mesmo com âncora arquivada; sem fila, `porta_app` (`app/chamados/services/tarefa_do_dia.py:43`, `:621`). Atualizar expectativa da folha. |
| `test_MORDE_a_porta_vem_da_TAREFA_e_nao_do_app` | `app/chamados/tests/test_o261e_f5_alcance.py:256` | L | Item 1 torna a consulta necessária: carga consulta uma vez por colaborador (`app/chamados/services/tarefa_do_dia.py:551`). O mock deliberadamente vazio em `app/chamados/tests/test_o261e_f5_alcance.py:253` significa pergunta fora dos pendentes: esperar `nada`, sem porta, e não conservar `(fio_morto=0, porta_pergunta=1)` (`app/chamados/services/tarefa_do_dia.py:598`, `:616`). |
| `test_a_ancora_VIVA_fora_da_fila_tambem_e_veu` | `app/chamados/tests/test_o261e_f5_alcance.py:460`; helper `:134` | L | Item 1: trocar causa `a/registrado` por `porta_app/colab`; a fila da âncora não limita o colaborador (`app/chamados/services/tarefa_do_dia.py:43`). |
| `test_b_respondida_com_resposta_VAZIA_e_porta_do_COLABORADOR_pela_tarefa` | `app/chamados/tests/test_o261e_f5_alcance.py:196` | L | Item 1: carimbo de resposta vazia não integra pendentes (`app/chamados/tests/test_o261e_f5_alcance.py:109`; `app/chamados/services/pergunta_no_app.py:75`). Não esperar `colab`; sem outro dono, `fio=nada`, `rota=None` pela publicação (`app/chamados/services/tarefa_do_dia.py:612`). O anexo mostra `porta_pergunta/nada`, estado já eliminado na cópia por `:616`. |
| `test_e_pergunta_viva_sob_chamado_TERMINAL_tem_nome` | `app/chamados/tests/test_o261e_f5_alcance.py:222`; helper `:134` | L | Item 1: pendente a responder conserva `porta_app/colab` sob terminal; não é linha sem porta da causa `e` (`app/chamados/services/tarefa_do_dia.py:43`, `:621`). |
| `test_MORDE_a_frase_do_fio_nada_nao_afirma_o_que_o_juiz_nao_garante` | `app/folha/tests/test_haiku_fora_da_folha.py:173` | M | Renovar as três substrings antigas de `:172` para o contrato textual publicado, `SEM PORTA: a tarefa nao aponta acao alcancavel neste dia; alguem precisa olhar` (`app/folha/fora_da_folha.py:54`), coerente com publicação apenas com dono (`app/chamados/services/tarefa_do_dia.py:612`). |
| `test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | `app/folha/tests/test_haiku_fora_da_folha.py:81` | M | Selo textual mudou de `SEM FIO` para `SEM PORTA`; preservar assert `fio=nada` em `:80` e verificar o texto publicado (`app/folha/fora_da_folha.py:54`; `app/chamados/services/tarefa_do_dia.py:616`). |

Contagem deste subconjunto: **L=5, M=2, B=0** (classificações e fundamentos nas sete linhas acima).

## 14/11 16:58 · sessao 2b92adb14c68 · - · - · origem=Ronald (tmux)

Os **13 testes deste recorte são L**: `_beco` cria pergunta de marco sem resposta em disputa aberta (`app/escala/tests/test_f3_beco1_folga_na_lista.py:55-57`), pendente no app (`app/chamados/services/pergunta_no_app.py:74-76`). Pela lei **item 1**, responder+pendente dá `fio='colab'`, independentemente da âncora (`app/chamados/services/tarefa_do_dia.py:43-44`). C11 só admite tarefa sem porta (`app/escala/services/cadastro_realidade.py:469`).

| teste | assert/erro | CLASSE | mudança | [nao verificado]
|---|---|---|---|
| `test_MORDE_dry_nao_escreve` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:72` | L | Item 1: cenário atual tem `porta_app/colab`; esperar zero candidatos/zero pais no DRY. Refazer cenário sem porta para conservar cobertura do command (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:159` | L | Item 1: cenário atual tem porta do app; zero reatas. Refazer cenário sem porta para testar vencimento do silêncio (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:82` | L | Item 1: não entra em C11; sem linha `SEM PAI`, zero reatas. Refazer cenário sem porta para testar ausência de pai (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:53` | L | Item 1: `_c11()` novo é `[]` antes do apply; zero reatas. Refazer cenário sem porta para cobrir reapontamento (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:145` | L | Item 1: cenário atual nem seleciona C11; não alcança a disputa de concorrência. Refazer cenário sem porta, conservando a proibição de reatar após silêncio (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:119` | L | Item 1: cenário atual não entra em C11; não há linha `PAI FORA DA FILA`. Refazer cenário sem porta para testar a guarda (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_F5_queries_nao_crescem_com_os_dias` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:322` | L | Item 1: neste cenário `(d1,d5)` novo é `(0,0)`; para preservar o teste de custo, refazer cenário realmente sem porta e manter assert de queries constantes por dia (`app/escala/services/cadastro_realidade.py:429-432,469`). |
| `test_MORDE_competencia_anterior_NAO_exportada_vai` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:110` | L | Item 1: aberta/anterior não suprime a porta do app; C11 novo `[]` (`app/chamados/services/tarefa_do_dia.py:43-44`; `app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_F1_lista_le_o_instante_de_quem_chama` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:219` | L | Item 1: no instante passado fornecido a pergunta está pendente; C11 novo `[]`. Refazer cenário sem porta para conservar a prova do relógio (`app/escala/tests/test_f3_beco1_folga_na_lista.py:215-218`; `app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:265` | L | Item 1 remove C11; resta só C9 lavrado `BATIDA`, portanto novo conjunto `{('C9','BATIDA')}` e `dias[...]='BATIDA'`. Sem dois donos, não há reconciliação; refazer cenário C11 sem porta para continuar testando divergência (`app/escala/tests/test_f3_beco1_folga_na_lista.py:262`; `app/escala/services/cadastro_realidade.py:988-989`). |
| `test_RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:180` | L | Item 1: manter rota/fila/verbos, mudar `fio` para `colab` pela pergunta pendente sob a outra âncora; C11 continua `[]` (`app/chamados/services/tarefa_do_dia.py:43-44`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:173-175`). |
| `test_RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:204` | L | Item 1: perguntas pendentes têm porta do app mesmo sob terminal/registrado; C11 novo `[]` (`app/escala/tests/test_f3_beco1_folga_na_lista.py:193-198`; `app/chamados/services/tarefa_do_dia.py:43-44`). |
| `test_RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:88` | L | Item 1: dia com porta do app não é C11; novo `c11=[]`, sem dono C11 publicado. Refazer cenário sem porta para testar `CADASTRO` (`app/escala/services/cadastro_realidade.py:469,512`). |

**B: nenhum neste recorte. Contagem: L=13, F=0, M=0, B=0, ?=0** — enumeração das linhas da tabela acima. [não verificado]

_5207 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
