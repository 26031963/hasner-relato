# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 13:46** · publicado em **15/11/2026 13:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 13:46 · sessao 8d34801ce890 · E3-RESTO-R3 · construtor · origem=Code

Base: `6fb74c6d285a3f280800a5965c41102ec7121244`. SHA256 do código: `6644768e0fea2a64ee4700a54bd0e01cf44b813decbd474b99c0294a13852a49`. [Manifesto](chamados/tests/e3_r3_c11_auditoria.txt:1)

[Entrega completa — 29 linhas](chamados/tests/e3_r3_c11_entrega.txt:1), com `arquivo::função`, comandos e logs por item:

| Item | RED antes da cura | GREEN |
|---|---|---|
| 1 — LU-01 | [FAILED](chamados/tests/e3_r3_c11_red1.txt:26) | [32 OK](chamados/tests/e3_r3_c11_green1.txt:41) |
| 2 — dia no href e marca | [FAILED](chamados/tests/e3_r3_c11_red2.txt:25) | [4 OK](chamados/tests/e3_r3_c11_green2.txt:10) |
| 3 — recorte das pautas | [FAILED](chamados/tests/e3_r3_c11_red3.txt:51) | [6 OK](chamados/tests/e3_r3_c11_green3.txt:15) |
| 4 — relógio | [FAILED](chamados/tests/e3_r3_c11_red4.txt:69) | [3 OK](chamados/tests/e3_r3_c11_green4.txt:9) |
| 5 — alarme passivo | [FAILED](chamados/tests/e3_r3_c11_red5.txt:16) | [2 OK](chamados/tests/e3_r3_c11_green5.txt:8) |
| 6 — cor existente | [FAILED](chamados/tests/e3_r3_c11_red6.txt:29) | [2 OK](chamados/tests/e3_r3_c11_green6.txt:9) |
| 7 — docstrings e drawer | [FAILED](chamados/tests/e3_r3_c11_red7.txt:36) | [3 OK](chamados/tests/e3_r3_c11_green7.txt:9) |

Regressão local: **137 OK**; mutantes c11: **23/23 mortos por asserção**, com controles verdes. [Regressão](chamados/tests/e3_r3_c11_regressao.txt:168), [mutantes](chamados/tests/e3_r3_c11_mutantes.txt:550).

Censo dos cinco leitores e escritores/leitores dos campos constam na [entrega](chamados/tests/e3_r3_c11_entrega.txt:14).

LEI-AKITA: `padrao=Single Source of Truth / Aggregate Root; Clock Injection; CQRS com projecoes reconstruiveis`. [Referências](chamados/tests/e3_r3_c11_entrega.txt:29)

Sem commit: o `git-dir` fica fora de `app`, único diretório autorizado para escrita. [Evidência](chamados/tests/e3_r3_c11_auditoria.txt:2). ORM, produção, renderização Django e certificação final: [nao verificado].

## 15/11 13:29 · sessao 63adf9493c6c · - · - · origem=Ronald (tmux)

Censo do residual `decidir → admin`:

- **Folha:** dia muda `chamado→admin`; pessoa isolada `C→A`, misto com `colab` pode mudar `C→B`; outro dia `chamado` conserva C (`folha/fora_da_folha.py:89`, `:103`, `:214`, `:231`). Balde visual Admin permanece Admin (`folha/retrato_folha.py:193`).
- **Vigia:** `alcance_por_rota_papel` muda `porta_fila|chamado→porta_fila|admin`; `alcance_porta_exige_o_admin` aumenta. Rota e `fio_vivo` permanecem (`chamados/services/vigia_fio.py:578`, `:601`, `:369`).
- **Furo:** nenhum balde muda; lê rota (`chamados/services/furo_sem_canal.py:98`, `:104`).
- **Cadastro:** nenhum balde muda; lê presença do envelope (`escala/services/cadastro_realidade.py:472`).
- **Sombra:** projeções de fio e `exige_admin`, além da assinatura, mudam; `sem_porta`/`sem_fio` permanecem (`chamados/management/commands/diff_e3_sombra.py:57`, `:62`, `:63`, `:67`, `:75`). C17 e candidata usam o mesmo juiz; diferença nova entre lados é [nao verificado] (`:144`, `:145`).

Atenção: fio admin residual atualmente publica `acao='validar'` pela expressão existente, antes `None` (`chamados/services/tarefa_do_dia.py:693`). Comentário contraditório em `:669` precisa acompanhar a cura.

`LEI-AKITA: padrao=Single Source of Truth / Aggregate Root; Clock Injection; CQRS com projecoes reconstruiveis` — vocabulário em `docs/GLOSSARIO.md:11`, `:19`, `:24`; exigência em `docs/PLANO-PADROES.md:53`.

## 15/11 13:28 · sessao 071afeef32a4 · E4C-F1V3 · construtor · origem=Code

Aplicada a tipagem explícita, sem alterar os predicados: [juizes.py:1652](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:1652), [juizes.py:1674](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:1674). [nao verificado]

- Censo completo: **2 Coalesce, 4 Subquery, 5 Value, 0 Case**, em 89 arquivos Python examinados; todas as 11 expressões receberam tipo. [resultados:30](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:30)
- Tipos: `ato_texto → TextField`; `ato → CharField(max_length=16)`; PK → `BigAutoField`. [models.py:4015](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/models.py:4015), [apps.py:5](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/apps.py:5)
- Doubles adaptados para aceitar `output_field`, sem mudar asserções. [teste:460](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1_juizes_le_o_ato.py:460), [teste SQL:88](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1v4_sql.py:88) [nao verificado]

O `test_o261` tem causa independente, por leitura: a fixture omite `respondida_em`; a conservação gera somente validação, que não fornece texto de resposta. [fixture:357](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_o261_vigia_fio.py:357), [models.py:4074](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/models.py:4074), [leitura:50](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/services/atos_pergunta.py:50)
O `except` existente engole o erro de parsing do texto vazio e devolve slot ausente: sem plantio resulta `slot_nao_derivavel`; com plantio ligado resulta `com_registro`. Esse defeito permanece nesta volta. [juizes.py:459](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:459), [juizes.py:862](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:862), [classificador:106](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/services/vigia_carimbo.py:106)

- RED estrutural: `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v5_mutantes --red-base` → 11 falhas, rc=1. [resultados:46](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:46)
- GREEN/regressão: 49 testes, rc=0, **2 skips ORM**; comando completo registrado. [resultados:157](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:157) [nao verificado]
- Mutantes: `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v5_mutantes --todos` → 22 detectados; comandos individuais registrados. [resultados:166](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:166)
- Ruff com `ruff.toml` e `py_compile` em memória: rc=0; comandos registrados. [resultados:249](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:249)
- **FieldError real ainda não reproduzido aqui:** Django ORM indisponível. Os testes de `str(qs.query)`, com conexão/cursor proibidos, ficaram para Code/suite. [teste:110](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1v5_tipos_sql.py:110), [resultados:169](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:169) [nao verificado]

LEI-AKITA: origem=`chamados/juizes.py:1636` (face SQL da conservação); testemunha=`DecisaoPergunta` via `atos_pergunta.leitura:22`; RED=`test_e4c_f1v5_tipos_sql.py:42`; quem-mais-lê=`raiox.py:135`, `fio.py:428`, `juizes.py:1708`, `cadastro_realidade.py:460`; juízes novos=0; padrão=Parallel Change / Expand-Contract (`juizes.py:318`). Certificação permanece com Code/suite e opus final.

## 15/11 13:21 · sessao 22609382723a · E3-RESTO-R3 · verificador · origem=Code

Os casos abaixo são demonstráveis pelo código; sua ocorrência e quantidade em produção são **[nao verificado]**. Atribuição histórica além do diff fornecido também é **[nao verificado]**.

1. **Campos, escritores, leitores e significado**

   **Alta — a porta de Resolver dia perde a data que justificou sua publicação.** A carga consulta datas específicas, mas publica a ação sem transportar o dia até o link. A Folha conserva apenas `acao/ausencia/rota`, e `portas_da_linha` monta a URL somente com o colaborador. Caso concreto: pendência de setembro consultada em novembro publica `admin/porta_acao`, mas o clique abre novembro, porque o calendário sem parâmetros usa o mês atual. A porta publicada não entrega diretamente o dia que precisa ser resolvido. Referências: `app/chamados/services/tarefa_do_dia.py:609`, `app/folha/fora_da_folha.py:210`, `app/folha/retrato_folha.py:1042`, `app/colaboradores/services/calendario.py:194`, `app/colaboradores/services/calendario.py:239`.

   **Média — `dias_sem_porta` é gravado e fica sem leitor de produção.** Caso concreto: pessoa com um dia `admin` e outro `nada` passa ao grupo A; o dia sem porta é preservado no JSON, mas o drawer mostra todas as datas com a mesma apresentação, sem identificar aquela pendência. A mudança de agrupamento retira essa pessoa do balde sem ação, e a marca necessária para distinguir seus dias não chega à tela. Referências: `app/folha/fora_da_folha.py:102`, `app/folha/fora_da_folha.py:207`, `app/folha/retrato_folha.py:1225`, `app/folha/retrato_folha.py:1098`, `app/templates/folha/_retrato_o_que_fazer.html:42`.

   Censo dos contratos afetados:

   | Campos/valores | Quem escreve ou fornece | Quem lê / significado |
   |---|---|---|
   | `CelulaDia.ata`, `lampadas`, `veredito`, `veredito_em`, `veredito_via`, `protestos` | Porta da célula: `app/ponto/portas/celula.py:1032`, `app/ponto/portas/celula.py:1091` | Leitor da ata: `app/escala/services/leitor_celula.py:114`; tarefa: `app/chamados/services/tarefa_do_dia.py:282`; retenção: `app/folha/export.py:1176`. |
   | Pergunta: `data_evento`, `celula`, `motivo`, `instancia_marco`, `conversa`, `via_resolucao` | Nascimento: `app/chamados/models.py:3100`; rechaveamento: `app/chamados/services/fio.py:1018` | Carga: `app/chamados/services/tarefa_do_dia.py:404`; identidade: `app/chamados/services/tarefa_do_dia.py:242`. `data_evento` significa dia do turno, não dia civil. |
   | `chamado_destino`, `disputa.chamado`, `status_local` | Costura: `app/chamados/services/fio.py:964`; estado do chamado: `app/chamados/models.py:228` | Âncora: `app/chamados/services/fio.py:116`; terminais na carga: `app/chamados/services/tarefa_do_dia.py:448`; alcance administrativo: `app/chamados/services/tarefa_do_dia.py:652`. |
   | Decisão: `ato`, `ato_em`, `ato_texto`, `absorcao_ausencia` | Porta append-only: `app/chamados/models.py:4086` | Carga ordena e corta por `ato_em <= agora`: `app/chamados/services/tarefa_do_dia.py:419`; julgamento: `app/chamados/services/tarefa_do_dia.py:156`. |
   | `resposta_colab`, `respondida_em`, `validada_em`, projeção sem linha | Resposta: `app/chamados/services/disputa_emissao.py:1439`; validação: `app/chamados/services/validacao.py:398` | Passivo E1: `app/chamados/services/tarefa_do_dia.py:428`; partição do app: `app/chamados/services/tarefa_do_dia.py:528`. |
   | `veredito_lavrado`, `julgado_em`, `janela_expira_em`; efeito com `pergunta_origem` | Lavra da pergunta: `app/chamados/models.py:3233` | Carga lê vigência e efeito no horizonte: `app/chamados/services/tarefa_do_dia.py:430`, `app/chamados/services/tarefa_do_dia.py:440`. |
   | Ausência: `pk`, `status`, cobertura | Porta de decisão: `app/ponto/services/ausencia.py:963` | Tarefa: `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:307`; link por ID da ausência: `app/folha/retrato_folha.py:1042`. |
   | `agora`, `hoje`, `mes/ano`, `[ini,fim]` | Commands capturam o instante: `app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/ponto/management/commands/pautas_discordantes.py:118` | Tarefa exige instante: `app/chamados/services/tarefa_do_dia.py:720`; janela inclusiva: `app/chamados/services/tarefa_do_dia.py:455`; competência pelo juiz: `app/ponto/services/pautas_dos_discordantes.py:77`. As exceções de relógio estão no item 4. |
   | `fila_ids`, `fila_cel`, `fila_dia`, `verbo`; `ancoras_terminais`, `_tarefas`, `_resolver_lidos` | Alimentação: `app/chamados/juizes.py:1562`; memos: `app/chamados/services/tarefa_do_dia.py:449`, `app/chamados/services/tarefa_do_dia.py:595` | Publicador da tarefa: `app/chamados/services/tarefa_do_dia.py:636`; os memos são por carga, sem persistência. |
   | `instancias`, `acoes`, `de`, `porta`, `razao`, `nao_sei` | Corpo da tarefa: `app/chamados/services/tarefa_do_dia.py:206`, `app/chamados/services/tarefa_do_dia.py:282` | Publicador: `app/chamados/services/tarefa_do_dia.py:635`; projeção: `app/chamados/services/tarefa_do_dia.py:708`. Desconhecimento pode coexistir com rota. |
   | `fio`, `rota`, `fila`, `verbos`, `acao`, `ausencia` | Publicador: `app/chamados/services/tarefa_do_dia.py:696` | Folha: `app/folha/fora_da_folha.py:203`; vigia: `app/chamados/services/vigia_fio.py:593`; furo: `app/chamados/services/furo_sem_canal.py:97`; C11: `app/escala/services/cadastro_realidade.py:472`; sombra: `app/chamados/management/commands/diff_e3_sombra.py:47`. |
   | `grupo`, `por_fio`, `datas`, `dias`, `acoes`, `dias_sem_leitura`, `dias_sem_porta` | Folha: `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:218` | Retrato copia: `app/folha/retrato_folha.py:1217`; persistência: `app/folha/management/commands/lavrar_retrato_folha.py:78`. `grupo` e baldes contam pessoas; listas identificam dias. |
   | `dono`, `tipo`, `saldo`, `informativo`; cores e rótulos | Montagem: `app/folha/retrato_folha.py:1228`; tabelas: `app/folha/retrato_folha.py:190`, `app/folha/retrato_folha.py:309` | Leitura da UI: `app/folha/retrato_folha.py:438`, `app/folha/retrato_folha.py:468`; drawer: `app/colaboradores/views.py:986`; template lê `it.numero`: `app/templates/core/_barra_gestao.html:67`. |
   | `sem_leitura`, `retidas_sem_leitura`, chaves/texto de Pauta | Coleta: `app/ponto/services/pautas_dos_discordantes.py:90`; sincronização: `app/ponto/services/pautas_dos_discordantes.py:191` | Command: `app/ponto/management/commands/pautas_discordantes.py:132`; retenção funciona por colab/competência, não por dia: `app/ponto/services/pautas_dos_discordantes.py:195`. |
   | `com_porta_*`, `nao_sei`, `total`, `universo`; `alcance_*`, linha de nove posições | Furo: `app/chamados/services/furo_sem_canal.py:107`; alcance: `app/chamados/services/vigia_fio.py:580` | Snapshot: `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`; impressão: `app/chamados/services/vigia_fio.py:664`; backfill lê índice 4: `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`. |
   | `calados`, `roteados`, `dias_sem_leitura_sistema`, `cego` | `app/folha/vigia_retido_calado.py:50` | Impressão: `app/folha/vigia_retido_calado.py:78`; alarme lê só parte deles: `app/folha/management/commands/vigiar_retido_calado.py:37`. |

2. **Juiz duplicado / regra paralela**

   **Alta — LU-01 não foi implementada.** O corte determina residual `decidir` com dono `admin`, e o registro da R3 inclui LU-01..07. Contudo, o mapa compartilhado só reconhece `cobrar/validar`; `decidir` cai em `chamado`, resultado incorporado pela tarefa. Caso concreto: nenhuma ação de instância e fila `{101: 'decidir'}` produz `fio='chamado'`, `rota='porta_fila'`, `acao=None`. Referências: `app/docs/CORTES.md:433`, `app/docs/BACKLOG.md:768`, `app/chamados/juizes.py:1653`, `app/chamados/juizes.py:1664`, `app/chamados/services/tarefa_do_dia.py:665`.

   Nenhuma lista literal nova de terminais encontrada na cura c7: ela importa `TERMINAIS` e lê `status_local`, como a costura. Referências: `app/chamados/services/tarefa_do_dia.py:447`, `app/chamados/services/fio.py:794`, `app/chamados/services/fio.py:809`.

3. **Escritores, portas, idempotência e dry**

   **Alta — `pautas_discordantes --apply` fecha pautas fora do recorte apurado.** A apuração recebe competência/empresa, mas a sincronização carrega todas as pautas vivas `[DISCORDANTE]` e fecha `vivas - alvo`. Caso concreto: apurar competência 10/empresa 1 fecha pauta da competência 9/empresa 2 sem consultá-la. O novo `sem_leitura` protege somente pares conhecidos pela rodada. É falha preexistente preservada, inclusive reconhecida na entrega c6. Referências: `app/ponto/management/commands/pautas_discordantes.py:49`, `app/ponto/services/pautas_dos_discordantes.py:186`, `app/ponto/services/pautas_dos_discordantes.py:194`, `app/ponto/services/pautas_dos_discordantes.py:208`, `app/chamados/tests/e3_r3_c6_entrega.txt:103`.

   | Command | Escrita / porta / repetição |
   |---|---|
   | `lavrar_furo_sem_canal` | Dry por default; apply usa `MetricaSnapshot.update_or_create` por chave/escopo/dia, sem duplicar snapshot: `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`. |
   | `reatar_beco_folga_ao_limbo` | Dry por default; apply exige reversão; costura por `reapontar_resolvedora`; já reatada não escreve novamente: `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:39`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`, `app/chamados/services/limbo_folgas.py:242`, `app/chamados/services/limbo_folgas.py:245`. |
   | `pautas_discordantes` | Dry por default; escreve/fecha pelas portas de Pauta. A comparação de chaves evita repetição sequencial, mas mantém o erro de recorte acima: `app/ponto/services/pautas_dos_discordantes.py:193`, `app/ponto/services/pautas_dos_discordantes.py:202`, `app/ponto/services/pautas_dos_discordantes.py:208`. |
   | `diff_e3_sombra` | Bloqueia SQL de escrita, inclusive dependências, e apenas lê o usuário-sistema: `app/chamados/management/commands/diff_e3_sombra.py:30`, `app/chamados/management/commands/diff_e3_sombra.py:113`. |
   | `vigiar_fio_morto` | Escreve carimbo em disco na rodada de frota, mesmo sem flag; não possui dry de filesystem: `app/chamados/management/commands/vigiar_fio_morto.py:234`, `app/chamados/management/commands/vigiar_fio_morto.py:240`, `app/chamados/management/commands/vigiar_fio_morto.py:140`. |
   | `vigiar_retido_calado` | O corpo imprime e retorna/alarma, sem escrita explícita: `app/folha/management/commands/vigiar_retido_calado.py:30`. |

   **Média — leitura pode criar `auth.User`.** Com usuário `sistema` ausente e competência trancada exigindo consulta de toque, a tarefa alcança `get_user_sistema()`, implementado com `get_or_create`. Isso contradiz “zero escrita no banco”, embora não constitua segundo escritor das cinco entidades nomeadas. Referências: `app/chamados/services/tarefa_do_dia.py:457`, `app/chamados/juizes.py:1228`, `app/core/utils.py:18`, `app/chamados/management/commands/vigiar_fio_morto.py:12`.

   Nenhum segundo escritor explícito de `Chamado`, `CelulaDia`, `DiaPago` ou `FechamentoMensal` foi identificado nos commands alterados; Pauta e costura usam as portas indicadas acima. Idempotência sob concorrência: **[nao verificado]**.

4. **Relógio fora da borda**

   **Média — ainda há relógio dentro de leitores.** `censo(hoje=<dia passado>)`, sem `agora`, mistura o dia informado com tarefas julgadas no instante atual. `contexto_calendario` também conserva `timezone.now()` como fallback. O command principal corrigiu sua chamada, mas os contratos dos leitores continuam permitindo essa mistura. Referências: `app/chamados/services/vigia_fio.py:279`, `app/chamados/services/vigia_fio.py:352`, `app/colaboradores/services/calendario.py:192`, `app/chamados/management/commands/vigiar_fio_morto.py:180`.

   **Média — reatador usa instantes diferentes para a mesma rodada.** Captura `agora`, mas omite-o nas duas consultas de fila. Caso concreto: `silenciado_ate` vence entre a seleção C11 e a consulta do pai; o alcance muda por outro relógio. Referências: `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`, `app/chamados/services/painel.py:190`.

   A Folha também continua lendo o relógio durante apresentação em `_hoje_iso`; `o_que_fazer(hoje=...)` não repassa esse parâmetro ao cálculo do LED. Referências: `app/folha/retrato_folha.py:465`, `app/folha/retrato_folha.py:1093`, `app/folha/retrato_folha.py:1097`.

5. **Crons e papel declarado**

   **Média — o contador novo de desconhecimento não participa de nenhum alarme.** Caso concreto: `cego=0`, `calados=0`, `dias_sem_leitura_sistema=3`, com `--alarme --alarme-passivo`, termina sem alarme. As listas contêm somente `cego` e `calados`; o comentário do cron promete ativação do próprio número pelo passivo, mas isso nunca alcança o contador novo. Referências: `app/folha/vigia_retido_calado.py:82`, `app/folha/management/commands/vigiar_retido_calado.py:16`, `app/folha/management/commands/vigiar_retido_calado.py:37`, `app/config/crons.py:1679`.

   Os papéis registrados são coerentes com a operação principal: vigias em `vigia`, snapshot em `lavra`. Nenhuma nova decisão própria de prazo identificada nessas entradas. Referências: `app/config/crons.py:2236`, `app/config/crons.py:2262`, `app/config/crons.py:2291`.

6. **Registro e frase no CORTES**

   **Alta — o anexo e o CORTES da cópia divergem.** As frases de nascimento de `tarefa_do_dia` e `dia_da_pergunta_trancado` existem no anexo fornecido, mas não foram encontradas no `app/docs/CORTES.md` local. Ambos estão registrados e ficam fora da base anterior à trava; a extração da trava exige a frase literal. Portanto, a árvore fornecida não satisfaz essa verificação documental. Referências: `app/core/juizes.py:299`, `app/core/juizes.py:300`, `bin/tests/test_juiz_novo_tem_corte.sh:28`, `bin/tests/test_juiz_novo_tem_corte.sh:37`, `bin/tests/test_juiz_novo_tem_corte.sh:44`.

   O delta fornecido de `core/juizes.py` não acrescenta autoridade: reancora uma impressão pendente. Atribuir a ausência das frases à c10 é **[nao verificado]**; não estou afirmando ausência de autorização, pois ela consta do anexo. Referência da alteração: `app/core/juizes.py:878`.

7. **Testes e mutantes c7–c10** [nao verificado]

   **Alta — há teste verde que fixa resultado contrário à LU-01.** `test_C6_sem_devida_residual_decidir` exige `chamado`, enquanto o corte exige `admin`. Esse selo conserva a cura omitida. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:129`, `app/docs/CORTES.md:433`.

   Não identifiquei enfraquecimento na correção textual c10 do E1: a primeira rodada continua exigindo `segue_sem_porta` e uma decisão; a segunda exige `sem_porta_de_sistema` e mantém exatamente uma linha. Isso corresponde ao prefixo emitido quando não há ato executável. Referências: `app/chamados/tests/test_e4_passivo.py:65`, `app/chamados/tests/test_e4_passivo.py:69`, `app/chamados/tests/test_e4_passivo.py:72`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:263`.

   Foram executados 81 testes sem banco e os nove mutantes c7; os testes passaram e os mutantes morreram por asserção **[nao verificado por artefato persistido]**. Os casos não terminal/terminal permanecem separados no teste. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:540`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:544`.

   **Prova incompleta:** morte do mutante ORM `item02_E1_terminal` após c10 e suite inteira do hash permanecem **[nao verificado]**. O mutante está definido em `app/chamados/tests/e3r3_c9_mutantes.py:27`.

8. **`_barra_gestao.html` e duas cascas**

   Nenhum derivador numérico no delta: o template exibe `it.numero`; a alteração acrescenta somente o ramo de cor `petroleo`. Referência: `app/templates/core/_barra_gestao.html:67`.

   **Média — exceção de cor nova sem corte específico demonstrado.** O código admite que `#0f766e` é cor nova e chama a exceção de decisão da triagem c6, apesar da proibição registrada no próprio bloco. A entrega reconhece a necessidade de corte se essa proibição prevalecer. Autorização da exceção: **[nao verificado]**. Referências: `app/folha/retrato_folha.py:300`, `app/folha/retrato_folha.py:303`, `app/chamados/tests/e3_r3_c6_entrega.txt:108`.

   Paridade visual das duas cascas por renderização/estilo calculado: **[nao verificado]**. O teste novo examina texto do template, não estilo calculado: `app/chamados/tests/test_e3_r3_c6_sem_banco.py:376`.

9. **Docstrings e comentários incorretos**

   - **Baixa:** `furo_sem_canal` promete porta para chamado vivo da fila mesmo sem ação; residual `cobrar` é excluído pela tarefa. Referências: `app/chamados/services/furo_sem_canal.py:19`, `app/chamados/services/tarefa_do_dia.py:663`.
   - **Baixa:** `lavrar_furo_sem_canal` se descreve como VIGIA com papel declarado, mas está registrado como `lavra` e grava snapshot. Referências: `app/chamados/management/commands/lavrar_furo_sem_canal.py:6`, `app/config/crons.py:2291`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`.
   - **Baixa:** a Folha ainda promete queries constantes em N, enquanto a tarefa agora declara consulta plural adicional por colaborador sem porta. Referências: `app/folha/fora_da_folha.py:32`, `app/chamados/services/tarefa_do_dia.py:22`.
   - **Média:** o novo caminho de ausência abre formulário cuja edição chama `salvarAusencia`, definida apenas na página Ausências. O drawer da Folha não carrega essa definição; aprovar/rejeitar continuam por POST nativo. Referências: `app/folha/retrato_folha.py:1014`, `app/templates/folha/_retrato_o_que_fazer.html:73`, `app/templates/ponto/partials/painel_ausencia.html:148`, `app/templates/ponto/ausencias.html:234`, `app/templates/ponto/partials/painel_ausencia.html:323`.

10. **Travas e pré-requisitos de pouso** [nao verificado]

   Nenhuma dependência técnica ausente de COMPETENCIA-PAGA, BUG-PAR ou E4c foi demonstrada pelo código examinado; comprovação de pouso dessas obras em produção é **[nao verificado]**. O registro ainda declara o apply do reatador em produção bloqueado até corte de desenho. Referência: `app/config/crons.py:820`.

   A entrega c6 exige medição anterior ao pouso para a ampliação M1 do C11. Essa medição no hash final não consta do dado fornecido: **[nao verificado]**. Referência: `app/chamados/tests/e3_r3_c6_entrega.txt:86`.

**VEREDITO: REPROVA.**

## 15/11 13:21 · sessao 157e344c5f6f · - · - · origem=Ronald (tmux)

Censo completo do diff, validado por AST em 89 arquivos Python alterados: 11 construtores, todos em `chamados/juizes.py`:

- `Subquery`: 4 — `:1652`, `:1657` (PK), `:1674` (`ato_texto`), `:1675` (`ato`).
- `Coalesce`: 2 — `:1674`, `:1675`.
- `Value`: 5 — `:1674`, `:1675` (vazios), `:1676` (`RESPOSTA`), `:1678`, `:1680` (regex).
- `Case`: 0 no diff examinado.

Tipos de origem: `ato_texto = TextField` em `chamados/models.py:4018`; `ato = CharField(max_length=16)` em `chamados/models.py:4015`; PK implícita `BigAutoField` por `chamados/apps.py:5` e `config/settings/base.py:104`.

Atenção aos doubles existentes: `Value`, `Subquery` e `Coalesce` não aceitam kwargs em `chamados/tests/test_e4c_f1v4_sql.py:88`; há equivalentes em `chamados/tests/test_e4c_f1_juizes_le_o_ato.py` [linha não verificada]. Adaptar assinaturas sem alterar asserções.

## 15/11 13:20 · sessao 03e711464610 · - · - · origem=Ronald (tmux)

Causa independente do `FieldError`, por leitura do código:

- `_porta` preenche texto e validação, mas omite `respondida_em`, cujo padrão é NULL (`chamados/tests/test_o261_vigia_fio.py:357`; `chamados/tests/test_o261_vigia_fio.py:364`; `chamados/models.py:2526`).
- A conservação só gera RESPOSTA com `respondida_em`; sem isso gera apenas VALIDAÇÃO (`chamados/models.py:4074`; `chamados/models.py:4078`).
- `leitura` inicia resposta vazia; VALIDAÇÃO não atribui texto (`chamados/services/atos_pergunta.py:33`; `chamados/services/atos_pergunta.py:50`).
- `_slot_esperado` passa a ler esse texto vazio; `date.fromisoformat('')` falha e o `except Exception` existente devolve `(None,None)` (`chamados/juizes.py:446`; `chamados/juizes.py:459`; `chamados/juizes.py:466`).
- Sem batida, a classe torna-se `slot_nao_derivavel` (`chamados/services/vigia_carimbo.py:110`).
- Com batida ligada, slot ausente produz absolvição por existência da batida (`chamados/juizes.py:862`), portanto `com_registro` (`chamados/services/vigia_carimbo.py:106`).

O caminho explica exatamente as duas classes informadas, sem depender da face SQL problemática. O `except` captura falha de parsing, não o `FieldError`, nesse percurso (`chamados/juizes.py:460`). Na base, o derivador lia diretamente `p.resposta_colab`; a troca para `atos.resposta` está no diff `77dfa94fa..97bbaa604`, hunk de `_slot_esperado` [linha histórica: `77dfa94fa:app/chamados/juizes.py:363`].

Nenhum arquivo ou asserção alterado. Reprodução executada dos subtestes com banco: [não verificado].

## 15/11 13:18 · sessao 3336f98fe395 · - · - · origem=Ronald (tmux)

Achados:

- **Alta — LU-01 ficou sem cura e o teste certifica comportamento contrário à lei.** O corte manda residual `decidir` ter dono `admin` (`app/docs/CORTES.md:433`), e R3 inclui LU-01..07 (`app/docs/BACKLOG.md:768`). Contudo, `fio_pelos_verbos` devolve `chamado` para `decidir` (`app/chamados/juizes.py:1653`, `:1664`); a tarefa conserva isso e escreve “LU-01 fora” (`app/chamados/services/tarefa_do_dia.py:665`, `:669`). Reproduzi com corpos reais por AST: fila `{101: 'decidir'}`, nenhuma ação → `fio='chamado'`, `rota='porta_fila'`, `acao=None`. O teste exige esse resultado errado (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:129`). Nenhum corte posterior de exclusão localizado nos documentos consultados [nao verificado como censo universal].

- **Alta — frases de nascimento presentes no anexo estão ausentes do CORTES da árvore.** `tarefa_do_dia` e `dia_da_pergunta_trancado` estão registrados (`app/core/juizes.py:299`, `:300`), mas suas frases não aparecem no arquivo local; o anexo fornecido afirma restauração. Reproduzi em memória a extração da trava: esses dois e outros oito registros ficam sem frase. Pela implementação, isso dá falha (`bin/tests/test_juiz_novo_tem_corte.sh:37`, `:44`). Não executei o script, que escreve temporário/base (`:52`, `:57`). Atribuição ao delta c10 [nao verificado].

- **Média — exceção de cor nova sem corte demonstrado.** A própria produção registra a proibição e admite ter criado exceção por “triagem c6” (`app/folha/retrato_folha.py:300`, `:303`, `:306`); a nova cor entra no partial (`app/templates/core/_barra_gestao.html:67`). A entrega admite que depende de corte se a proibição prevalecer (`app/chamados/tests/e3_r3_c6_entrega.txt:108`). Autorização específica [nao verificado].

- **Baixa — comentário promete teste inexistente.** A referência ao teste Django `folha/tests/test_e3_r3_c6_cor_do_balde.py` está em `app/chamados/tests/test_e3_r3_c6_sem_banco.py:377`; o arquivo não existe na cópia [nao verificado por arquivo:linha inexistente]. A entrega confirma que a renderização não foi feita (`app/chamados/tests/e3_r3_c6_entrega.txt:99`).

Verificação dos testes: [nao verificado]

- Executei **81 testes sem banco: OK** [nao verificado por artefato persistido; execução nesta sessão]. M2 positivo e M2b terminal permanecem distintos (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:540`, `:544`).
- Executei os **nove mutantes c7: todos mortos por asserção**, sem escrever fontes [nao verificado por artefato persistido; execução nesta sessão]. Os alvos e critérios estão em `app/chamados/tests/e3_r3_c7_mutantes.py:14`, `:60`.
- A troca c10 de texto da segunda rodada é coerente com o command: ato não executável publica o prefixo (`app/chamados/management/commands/backfill_fio_morto_e4b.py:263`), agora exigido pelo teste (`app/chamados/tests/test_e4_passivo.py:69`). Não identifiquei enfraquecimento desse caso.
- O mutante ORM c9 item02 está definido (`app/chamados/tests/e3r3_c9_mutantes.py:27`), mas sua morte após c10 permanece **[nao verificado]**, conforme o anexo.
- O partial lê `it.numero`, sem derivação (`app/templates/core/_barra_gestao.html:67`). Quebra das duas cascas por execução visual **[nao verificado]**; o ramo adicionado só seleciona `petroleo` nessa linha.

## 15/11 13:18 · sessao c0a4f2aee8c3 · - · - · origem=Ronald (tmux)

Achados da revisão de commands/crons, somente leitura:

1. **Média — contador novo não participa do alarme.** `dias_sem_leitura_sistema` é publicado com esperado 0 (`app/folha/vigia_retido_calado.py:82`), mas `ALARMA_SEMPRE` contém somente `cego` e `ALARMA_EM_0` somente `calados` (`app/folha/management/commands/vigiar_retido_calado.py:16`). Caso concreto: `cego=0`, `calados=0`, `dias_sem_leitura_sistema=3`, com ambas as flags, retorna normalmente porque só consulta aquelas listas (`app/folha/management/commands/vigiar_retido_calado.py:37`). Confirmei executando o `handle` extraído por AST, com dependências simuladas, sem banco/escrita. O comentário do cron promete ativação pelo `--alarme-passivo`, que nunca alcançará esse contador (`app/config/crons.py:1679`).

2. **Média — editar ausência quebra no novo caminho pela Folha.** A ação nova aponta ao partial de ausência (`app/folha/retrato_folha.py:1014`), aberto no drawer genérico (`app/templates/folha/_retrato_o_que_fazer.html:73`). O formulário chama `salvarAusencia` e não declara `method` (`app/templates/ponto/partials/painel_ausencia.html:148`); a única definição está na página Ausências (`app/templates/ponto/ausencias.html:234`). A edição só grava com POST (`app/ponto/views.py:2804`); portanto o novo caminho mostra um formulário cuja edição não funciona. Aprovar/rejeitar usam forms POST nativos e continuam alcançáveis (`app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344`).

3. **Alta, preexistente preservada — sincronização fecha pautas fora do recorte.** A apuração é limitada por competência/empresa (`app/ponto/management/commands/pautas_discordantes.py:49`, `app/ponto/services/pautas_dos_discordantes.py:77`), mas `sincronizar` lê todas as pautas vivas `[DISCORDANTE]` (`app/ponto/services/pautas_dos_discordantes.py:186`) e fecha as ausentes do alvo (`app/ponto/services/pautas_dos_discordantes.py:194`, `app/ponto/services/pautas_dos_discordantes.py:208`). Caso concreto: executar competência 10/empresa 1 fecha pauta viva da competência 9/empresa 2, sem apurá-la. O delta mantém a falha; não atribuir sua origem à c10.

4. **Baixa — papel documentado divergente.** O command se declara VIGIA com papel registrado (`app/chamados/management/commands/lavrar_furo_sem_canal.py:6`), mas o registro é `lavra` (`app/config/crons.py:2265`, `app/config/crons.py:2291`), coerente com sua escrita em `MetricaSnapshot` (`app/chamados/management/commands/lavrar_furo_sem_canal.py:50`).

5. **Baixa — promessa antiga contradiz a nova porta.** O serviço afirma que chamado vivo da fila conserva porta mesmo sem ação (`app/chamados/services/furo_sem_canal.py:19`), mas a tarefa exclui residual `cobrar` da atribuição de dono (`app/chamados/services/tarefa_do_dia.py:665`).

6. **Relógio interno permanece:** `censo` ainda aceita omissão de `agora` e lê `timezone.now()` dentro do leitor (`app/chamados/services/vigia_fio.py:279`). O command alterado passa o instante corretamente (`app/chamados/management/commands/vigiar_fio_morto.py:180`). O reatador captura um instante (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52`), mas suas duas consultas de fila omitem esse parâmetro (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`); cada uma lê outro relógio (`app/chamados/services/painel.py:190`). Caso: `silenciado_ate` vence entre seleção C11 e consulta da fila.

Portas e efeitos conferidos:

- `lavrar_furo_sem_canal`: dry por default; com `--apply`, atualiza/cria snapshot por chave+escopo+dia (`app/chamados/management/commands/lavrar_furo_sem_canal.py:42`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`).
- `reatar_beco_folga_ao_limbo`: dry por default; exige arquivo de reversão no apply; escreve pela porta `reapontar_resolvedora` (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:39`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`, `app/chamados/services/limbo_folgas.py:245`). Já reatada, não escreve novamente (`app/chamados/services/limbo_folgas.py:242`).
- `pautas_discordantes`: dry por default; escreve/fecha pelas portas de Pauta (`app/ponto/management/commands/pautas_discordantes.py:54`, `app/ponto/services/pautas_dos_discordantes.py:198`, `app/ponto/services/pautas_dos_discordantes.py:202`, `app/ponto/services/pautas_dos_discordantes.py:208`).
- `diff_e3_sombra`: bloqueio SQL de escrita e usuário-sistema somente lido (`app/chamados/management/commands/diff_e3_sombra.py:30`, `app/chamados/management/commands/diff_e3_sombra.py:113`).
- `vigiar_fio_morto`: sem flag escreve carimbo em disco na rodada de frota; portanto não tem dry de filesystem (`app/chamados/management/commands/vigiar_fio_morto.py:234`, `app/chamados/management/commands/vigiar_fio_morto.py:240`, `app/chamados/management/commands/vigiar_fio_morto.py:140`).
- Há escrita indireta potencial de `auth.User` nos leitores: tarefa consulta `dias_trancados` (`app/chamados/services/tarefa_do_dia.py:457`), que pode chamar `get_user_sistema` (`app/chamados/juizes.py:1228`), implementado com `get_or_create` (`app/core/utils.py:18`). Caso concreto: sistema ausente e competência trancada com toque a verificar. Contradiz “zero escrita no banco” do vigia (`app/chamados/management/commands/vigiar_fio_morto.py:12`); não é segundo escritor das cinco entidades nomeadas pelo usuário.
- Nenhuma dependência concreta ausente de BUG-PAR/COMPETENCIA-PAGA/E4c demonstrada nesta fatia [nao verificado]. O próprio registro mantém apply em produção do reatador bloqueado por desenho (`app/config/crons.py:823`).

## 15/11 13:17 · sessao 2f5bab648358 · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Alta — porta “Resolver dia” perde o dia que a originou.** A carga consulta a oferta para datas específicas (`app/chamados/services/tarefa_do_dia.py:609`), mas publica somente `acao/ausencia/rota` (`app/chamados/services/tarefa_do_dia.py:696`). O leitor monta URL com apenas o colaborador (`app/folha/retrato_folha.py:1045`). A view usa `request.GET` (`app/colaboradores/views.py:1949`), e sem parâmetros o calendário abre o mês corrente (`app/colaboradores/services/calendario.py:194`, `app/colaboradores/services/calendario.py:239`). Caso: pendência de setembro consultada em novembro recebe porta administrativa que abre novembro; o botão do dia pendente só aparece no respectivo quadrinho (`app/templates/colaboradores/partials/_calendario_grade.html:131`).

2. **Média — relógio ainda lido dentro de leitores alterados.** `contexto_calendario` usa `timezone.now()` quando omitido `agora` (`app/colaboradores/services/calendario.py:192`), e `censo` também (`app/chamados/services/vigia_fio.py:279`). Caso: `censo(hoje=<dia passado>)` sem `agora` usa o dia informado para contadores e o instante presente para julgar tarefas (`app/chamados/services/vigia_fio.py:280`, `app/chamados/services/vigia_fio.py:352`). O command principal agora repassa ambos coerentemente (`app/chamados/management/commands/vigiar_fio_morto.py:180`); a API do leitor continua permitindo a mistura (`app/chamados/services/vigia_fio.py:266`).

3. **Baixa — editar ausência no novo caminho da Folha depende de JS ausente.** A nova ação aponta ao painel da ausência (`app/folha/retrato_folha.py:1014`), carregado pelo drawer genérico com `innerHTML` (`app/templates/folha/_retrato_o_que_fazer.html:73`, `app/templates/core/_drawer_generico.html:26`). O formulário de edição chama `salvarAusencia` (`app/templates/ponto/partials/painel_ausencia.html:148`), cuja definição está na página de Ausências (`app/templates/ponto/ausencias.html:234`). Aprovar/rejeitar usam POST nativo e não dependem dessa função (`app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344`); portanto o defeito comprovado afeta a edição, não toda decisão.

Observação de custo: cada primeira consulta de colaborador agora materializa todos os dias da janela (`app/chamados/services/tarefa_do_dia.py:595`), enquanto o censo fornece a janela entre o menor e maior dia do universo escasso (`app/chamados/services/vigia_fio.py:352`). Amplificação de CPU/memória é demonstrável pelo laço; impacto real em produção [nao verificado].

Não encontrei leitura direta de relógio no juiz `tarefa_do_dia`; sua casca exige `agora` e rejeita carga com instante diferente (`app/chamados/services/tarefa_do_dia.py:720`, `app/chamados/services/tarefa_do_dia.py:724`).

## 15/11 13:14 · sessao e358877bc62b · E4C-F1V3 · construtor · origem=Code

HEAD/base: `02d9bef0062fb7c1d2e872688224790414c503ce` (chamados/tests/e4c_f1v4_mutantes.py:19). **Sem commit:** o índice Git fica fora de `app/`, onde o ambiente proíbe escrita.
`lei: 0` — reuso restaurado à regra da base `714a92a61` (chamados/juizes.py:1591).

- **1 — L2:** chamados/juizes.py:363→372 conserva carimbo+Log e mantém leitura por ato; retirada no Contract após escritor/backfill E4d. Motivo real de remoção permanece intacto (chamados/models.py:2123).
- **2 — Vias:** chamados/juizes.py:754→783 recupera carimbo para as seis vias automáticas, preservando a guarda da lei do dia.
- **3 — Projeção:** chamados/juizes.py:1456→1483 remove E antecipado; `[3]` volta a ser instante/None. E é avaliado dentro da guarda do C6 (:1576).
- **4 — Custo:** chamados/juizes.py:1612→1636 troca materialização da frota por `Exists/Subquery`; :1836→1856 restaura `q_vivas()` na fila.
- **5 — Reuso:** chamados/juizes.py:1566→1585 restaura `materializada_em is None or pergunta_viva(p)`.
- **6–8:** evidência nesta entrega; trilha C8b ligada ao leitor real e instrumentada em `exists()` (chamados/tests/e3_c8b_apoio.py:162/:177; chamados/tests/test_e3_c8b_sem_banco.py:438).

RED **antes da cura:** 7 testes, 14 FAIL. Reprodução final contra c3: 8 testes, 15 FAIL, zero erros externos: [nao verificado]
`PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v4_mutantes --red-base` (runner:114).
GREEN: 8 testes novos e **10/10 mutantes mordidos**, zero erros: [nao verificado]
`PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v4_mutantes --todos` (runner:87/:95).
Comando individual de cada mutante, expandido pelo laço abaixo (runner:24):
```bash
for m in L2_sem_conservacao vias_sem_carimbo projecao_booleano projecao_E_antecipado SQL_frota SQL_empate_perdido SQL_ignora_correspondencia fila_sem_prefiltro reuso_por_E leitor_retratacao_perde_conservacao; do PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v4_mutantes --mutante "$m"; done
```
Regressão: **82 testes GREEN**, executados com `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f1_juizes_le_o_ato chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_c8c_sem_banco chamados.tests.test_e3_pos_suite_sem_banco`. [nao verificado]
Os onze testes originais de pergunta viva passam no recorte sem banco; SQL comparado em 271 cenários × 3 faces (chamados/tests/test_e4c_f1v4_conservacao.py:61; chamados/tests/test_e4c_f1v4_sql.py:84).
`assertNumQueries(0/1)` para N=1/N=20 preparado; execução ORM e testes Django originais de retratação/bloqueantes: **[não verificado]** (chamados/tests/test_e4c_f1v4_custo.py:113/:120/:123).
Ruff com `ruff.toml`, `py_compile` em memória nos 11 arquivos e `git diff --check`: rc=0; sem bytecode escrito.

ESMERIL: unidade=pergunta; `ato_motivo` é texto, escrito via registrar/remover_motivo e lido por `leitura` (chamados/models.py:4086/:2123; chamados/services/atos_pergunta.py:54/:59).
`ato_em` é instante do ato, escrito por registrar, ordenado e recortado pela leitura; UTC fixo nos testes, armazenamento produtivo [não verificado] (chamados/models.py:4086; chamados/services/atos_pergunta.py:42; chamados/tests/test_e4c_f1_juizes_le_o_ato.py:17).
`materializada_em` é instante do carimbo: escritor `_escrever_carimbo`, leitores conservação, identidade e reuso; não vira ato fabricado (chamados/models.py:3180; chamados/juizes.py:300/:367/:1591).

Quem-mais-lê E: chamados/veredito_lavrado.py:262; chamados/services/explicador.py:69/:93, raiox.py:190, disputa_emissao.py:2001, materializacao.py:1239/:1433, vigia_carimbo.py:106; chamados/management/commands/backfill_veredito_lavrado.py:105, auditar_invariantes_chamados.py:538/:1170; alias chamados/models.py:4226.
Internos/projeção/reuso: chamados/juizes.py:902/:920/:923/:1494/:1576/:1849; escala/services/cadastro_realidade.py:502; chamados/services/declaracao_texto.py:240/:254.
`q_vivas`: chamados/services/fio.py:254, janela_colab.py:63, raiox.py:74, vigia_fio.py:706, cobranca.py:237, pergunta_no_app.py:53; ponto/services/veredito_celula.py:181; chamados/juizes.py:1858.
Filas: chamados/views.py:1420; chamados/services/acoes_disputa.py:97, validacao.py:956; chamados/templatetags/chamados_tags.py:202; chamados/models.py:2282; api/views_mensageria.py:540.

LEI-AKITA: origem=chamados/juizes.py (L1/L2 + vias + projeção + custo), testemunha=DecisaoPergunta via atos_pergunta.leitura, RED=test_e4c_f1v4_conservacao.py:32/:45/:61/:90/:98 + test_e4c_f1v4_custo.py:67/:84, quem-mais-le=censo acima, juizes novos=0, padrao=**Parallel Change / Expand-Contract** (docs/GLOSSARIO.md:18).
Certificação pelo Code e opus final: **[não verificado]**.

## 15/11 13:10 · sessao 93b182aaef10 · - · - · origem=Ronald (tmux)

Criado apenas `chamados/tests/test_e4c_f1v4_sql.py`.

- RED contra c3 em memória: FAIL `AssertionError: frota materializada antes do filtro SQL` (`:68`), rc=1.
- GREEN: `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f1v4_sql -v`, 1 teste OK. [nao verificado]
- Propriedade compara 271 cenários × 3 faces com a leitura Python e porta real de conservação (`:81–122`).
- Mutante sem correspondência R: 6 falhas de asserção, zero erros.
- Para mutante `gt→gte`, adicionar suporte explícito a `gte` na álgebra (`:39`) antes de contar a mordida.
- SQL real e quantidade de queries: [não verificado].

## 15/11 13:08 · sessao b739c6c69e07 · - · - · origem=Ronald (tmux)

[e4c_f1v4_mutantes.py](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v4_mutantes.py:1) criado com `--red-base`, `--todos` e `--mutante NOME`.

Comandos executados com `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v4_mutantes`:

- `--red-base`: rc=1, sete casos, 14 falhas de asserção, zero erros.
- `--todos`: rc=0, controle sete GREEN; oito mutantes mordidos, inclusive L2 e vias.

Contagem ORM N=1/N=20 permanece reservada ao Code, [não verificado].

## 15/11 13:03 · sessao b739c6c69e07 · - · - · origem=Ronald (tmux)

Criado [test_e4c_f1v4_custo.py](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1v4_custo.py:1).

RED contra c3: `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e4c_f1v4_custo` → rc=1, quatro falhas de asserção: três variantes de avaliação precoce da frota (:59) e ausência de pré-filtro (:76).

Casos ORM N=1/N=20 usam `assertNumQueries(0)` na composição e `assertNumQueries(1)` na execução (:94–115); execução ORM [não verificado]. Nenhum arquivo existente alterado.

## 15/11 13:02 · sessao 93b182aaef10 · - · - · origem=Ronald (tmux)

Item 5: restaurar regra da base `714a92a61`: `p.materializada_em is None or pergunta_viva(p)`. C3 usa `pergunta_viva(p) or not _pergunta_materializada_de_fato(p)` e altera reuso de pergunta morta carimbada sem E; L1/L2 fornecida não exige essa alteração. Leitores: `chamados/services/declaracao_texto.py:240/254`, `chamados/juizes.py:1830`; reexport `chamados/models.py:4226`. RED sugerido: morta+C+E=False → não reutilizável; sem C+E=True → reutilizável. [RED ainda não executado]

Censo produtivo:

- L2: `chamados/juizes.py:873/891/894`; reexport `chamados/models.py:4226`.
- E: `chamados/juizes.py:1456/1553/1566`; `chamados/veredito_lavrado.py:262/263`; `chamados/services/explicador.py:69/93`; `chamados/services/raiox.py:190/191`; `chamados/services/disputa_emissao.py:9/2001`; `chamados/services/materializacao.py:50/1239/1433`; `chamados/services/vigia_carimbo.py:95/106`; `chamados/management/commands/backfill_veredito_lavrado.py:88/105`; `chamados/management/commands/auditar_invariantes_chamados.py:507/538/1155/1170`; reexport `chamados/models.py:4226`.
- Projeção: `chamados/juizes.py:1469`; `escala/services/cadastro_realidade.py:439/502`.
- `q_vivas`: sete chamadas externas executáveis: `chamados/services/fio.py:254`, `janela_colab.py:63`, `raiox.py:74`, `vigia_fio.py:706`, `cobranca.py:237`, `pergunta_no_app.py:53`; `ponto/services/veredito_celula.py:181`. Definição `chamados/models.py:3425`.
- Corrigíveis: `chamados/views.py:1419/1420`, `chamados/services/acoes_disputa.py:96/97`, `chamados/templatetags/chamados_tags.py:198/202`, `chamados/models.py:2282`.
- Respondidas a validar: `api/views_mensageria.py:539/540`; `chamados/services/validacao.py:953/956`. Base já pré-filtrava por `PerguntaDisputa.q_vivas()` antes do juiz Python (`git show 714a92a61:app/chamados/juizes.py`, função `perguntas_respondidas_a_validar`).

Semântica:

- Escritor único DP: `chamados/models.py:4086/4098`; escritor RETRATAÇÃO atual: `:2121/2123`, motivo textual `evento: motivo X removido (via antes: Y)`, incompatível com igualdade `'obsoleta'` do leitor `chamados/services/atos_pergunta.py:52`.
- `ato_em`: instante do ato; leitura ordena `(ato_em,pk)` e corta futuro em `chamados/services/atos_pergunta.py:35/36`; resposta/validação em `:40/44`, retratação em `:52`.
- `ato_motivo`: texto do motivo; lido para recusa e retratação em `chamados/services/atos_pergunta.py:47/52`. Escritas atuais em `chamados/models.py:1990/2123/4077/4082`, `chamados/services/validacao.py:233`, `chamados/services/disputa_emissao.py:1634/1678/1699`, `chamados/management/commands/backfill_decisoes_pergunta.py:327`.
- `materializada_em`: instante do carimbo da pergunta; atribuição única `chamados/models.py:3180/3182`; porta `carimbar` em `:3186/3212`; materializador chama em `chamados/services/materializacao.py:1283/1437`; identidade `chamados/models.py:3266`. Leitura conservada `chamados/juizes.py:310`, identidade `:338`, reuso `:1566`, demais leitores por E acima. Configuração timezone-aware `config/settings/base.py:93/95`; armazenamento real UTC [não verificado].
- Termo exato: **`Parallel Change / Expand-Contract`**, `docs/GLOSSARIO.md:18`; conservar caminhos legado/DP realiza a fase de expansão. Nenhuma edição feita.

## 15/11 12:38 · sessao b5803b7c5092 · LOTE7-POSTMORTEM · censo · origem=Code

```text
1 | c | “Os testes novos [...] mordem” (lote7_codex_v1.q:19): pergunta estática, sem comprovação do RED executado | “Execução dos testes [...] [nao verificado]” (lote7_codex_v1.r:3) | “e do c1 e NAO contem os casos acrescentados em c3/c4” (lote7_entrega.md:31). Falta de RED demonstrado fica fora do alcance. [nao verificado]

2 | d | Auditoria dos selos por commit e da constância de padrao= ausente das perguntas (lote7_codex_v1.q:1–24; lote7_codex_v2.q:1–7; lote7_codex_v3.q:1–5) | Nenhum trecho identifica esses dois problemas (lote7_codex_v1.r:1–133; lote7_codex_v2.r:1–44; lote7_codex_v3.r:1–29) | Nenhuma identificação na triagem; a entrega registra apenas “padrao=CQRS com projecoes reconstruiveis” (lote7_entrega.md:48–69; lote7_entrega.md:83). MODO-COMUM conforme a definição deste pedido. [nao verificado]

3 | e | Faltou perguntar: “A entrega final corresponde aos vereditos disponíveis e às correções, inclusive sobre o cron?”; o escopo solicitado era “Leia o diff e os sitios que ele toca ou de que depende” (lote7_codex_v1.q:3) | “Reconstrução automática prometida sem mecanismo correspondente” (lote7_codex_v1.r:130) | “v1 rotulo falso "o cron reconstroi" [...] -> c3” foi PROCEDENTE, não descartado (lote7_entrega.md:50; lote7_entrega.md:52). A entrega conservou “ZERO testes executados” e “o recalculo do cron reconstroi a projecao” (lote7_entrega.md:3; lote7_entrega.md:80). [nao verificado]

4 | a | “Ausencia.status” e “QUEM ESCREVE e QUEM LE em toda a arvore” (lote7_codex_v1.q:9; lote7_codex_v1.q:11) | “Cobertura para folha exige `aprovada`; cobrança admite qualquer não rejeitada”; não apontou a invalidação desnecessária de não aprovadas nem seu efeito no contador (lote7_codex_v1.r:11; lote7_codex_v1.r:1–133) | “chamadores (todos passam a acordar)” e “fechamento le `status='aprovada'`”: fatos registrados, sem identificar o problema de desenho (lote7_entrega.md:24; lote7_entrega.md:26).

5 | b | “dia do turno x dia civil” e “Mudou o valor gravado de alguma competencia EXPORTADA” (lote7_codex_v1.q:11; lote7_codex_v1.q:21) | “o evento recalcula somente novembro, embora o turno seja atribuído a outubro”; também apontou recálculo persistente em aprovado/exportado (lote7_codex_v1.r:129; lote7_codex_v1.r:121) | “COMPETENCIA ERRADA NO DESFAZER (procedente como bug, FORA do escopo de arquivo)” e “FILA: obra propria” (lote7_entrega.md:58). Reversão/hash efetivos em produção: [nao verificado].

6 | b | “o que acontece [...] com o chamador quando a trilha nao grava (RuntimeError)?” (lote7_codex_v2.q:4) | “As três views capturam somente `AgendamentoNaoCancelavel`; o `RuntimeError` escapa desses tratamentos” (lote7_codex_v2.r:11) | “views fora da lista de arquivos; FILA (traduzir RuntimeError da trilha no lote)” (lote7_entrega.md:69). O descarte demonstrado é o das views; que o Codex também tenha identificado o alcance KERNEL: [nao verificado].

7 | b | “para cada caso, um mutante plausivel da cura que ele NAO pegaria?” (lote7_codex_v1.q:19–20) | “Invalidar somente criações aprovadas, deixando abertas descobertas”; “invalidar somente quando o estado anterior for rejeitada” (lote7_codex_v1.r:103; lote7_codex_v1.r:106) | “v1 mutantes nao mortos (FILA de reforco do RED, sem codigo de producao)” e “mas o teste nao os prende” (lote7_entrega.md:65). O descarte demonstrado é o dos mutantes; a alteração c5 posterior foi relatada pelo opus, sem diff correspondente fornecido (lote7_c5_opus.md:8). [nao verificado]

8 | b | “data_fim”, “minutos” e “Algum caso passa vazio (fixture que nao exercita o ramo)?” (lote7_codex_v1.q:9; lote7_codex_v1.q:20) | “o modelo calcula minutos antes de completar `data_fim`” e “o teste novo usa atestado e não detecta isso” (lote7_codex_v1.r:131) | “pre-existente em models.py (proibido); FILA” (lote7_entrega.md:63). [nao verificado]

TOTAL | a | 1 — achado 4 (lote7_codex_v1.q:9; lote7_entrega.md:26).
TOTAL | b | 4 — achados 5, 6, 7, 8 (lote7_entrega.md:58; lote7_entrega.md:69; lote7_entrega.md:65; lote7_entrega.md:63).
TOTAL | c | 1 — achado 1 (lote7_entrega.md:31).
TOTAL | e | 1 — achado 3 (lote7_codex_v1.q:3; lote7_entrega.md:52; lote7_entrega.md:80).
TOTAL | d | 1 — achado 2 (lote7_codex_v1.q:1–24; lote7_codex_v2.q:1–7; lote7_codex_v3.q:1–5; lote7_entrega.md:48–69).
```

## 15/11 12:34 · sessao 8ee20ef0a20d · - · - · origem=Ronald (tmux)

Classificação independente: **1 c; 2 d; 3 e; 4 a; 5 b; 6 b; 7 b; 8 b**.

- **2 d:** falta de selo nos commits e divergência entre padrões não aparecem nas perguntas, respostas ou triagem específica. Aplicar a definição de MODO-COMUM dada pelo usuário, que prevalece sobre CLAUDE.md:393. [nao verificado]
- **3 e:** faltou pedir conferência final da entrega contra a cura e os vereditos. O cron foi visto: “Reconstrução automática prometida sem mecanismo correspondente”; a triagem o **aceitou**, em “v1 rotulo falso ‘o cron reconstroi’ (...) -> c3”. Portanto não cabe b, pois não houve descarte desse achado. A falha remanescente é documental.
- **4 a:** v1.q pergunta quem lê/escreve `Ausencia.status` e se há escritores sem invalidação; não identifica o despertar desnecessário em não aprovada e seu efeito no contador.
- **5 b:** resposta identifica competência civil errada e recálculo de exportadas; triagem põe “COMPETENCIA ERRADA NO DESFAZER” na fila.
- **6 b:** resposta v2 identifica `RuntimeError` escapando das três views; triagem assume “trilha que falha agora vira 500 (...) FILA”. Isso sustenta b pelo componente explicitamente descartado; o componente KERNEL não deve ser apresentado como também visto.
- **7 b:** resposta lista mutantes sobreviventes; triagem “v1 mutantes nao mortos (FILA de reforco do RED...)”. A alteração c5 posterior à v3 não foi auditada naquela resposta.
- **8 b:** resposta “Derivação antiga de falta em ordem incorreta”; triagem “pre-existente em models.py (proibido); FILA”.

Totais: **a 1; b 4; c 1; d 1; e 1**.

## 15/11 12:31 · sessao f0c4e040e92a · REVISAO-REEMISSAO-1215 · verificador · origem=Code

**AVAL-DEVOLVIDO: REEMISSAO-1215 A | item 3.**  
**AVAL-DEVOLVIDO: REEMISSAO-1215 B | itens 3 e 4.**  
A devolução por parte está prevista em `reemissao_1215.txt:20`.

Nas referências aos anexos, conto as linhas dentro de cada bloco, incluindo vazias. **R** = `reemissao_1215.txt`; **P** = `portao_da_11.txt`; **Q** = `queima_ate_a_redefinicao.txt`; **E** = `entrega_relavra_item2.md`.

**Parte A**

1. **NAO PROCEDE — a mudança descrita é tecnicamente executável.**

   Hoje o ramo `r.ok` **não lê JSON**: chama diretamente `v2Sucesso`. A leitura de JSON aparece somente depois, no tratamento não 2xx. Isso confirma o defeito que o novo literal manda corrigir (`app/templates/ponto/bater_ponto_v2.html:327-328`; `R:4`).

   Os três status existem nos dois kernels, em respostas sem código HTTP alternativo:

   | Status | `views_core.py` | `views.py` |
   |---|---|---|
   | `ok` | `app/api/views_core.py:807-818` | `app/api/views.py:813-824` |
   | `ja_registrada` | `app/api/views_core.py:521-531` | `app/api/views.py:535-545` |
   | `ignorada_intervalo` | `app/api/views_core.py:673-683` | `app/api/views.py:682-692` |

   O quarto ramo é defensivo: pode ser exercitado com resposta 200 contendo status desconhecido, ausente ou JSON ilegível. O literal não exige alterar a API para produzi-lo; ocorrência real desse quarto caso em produção **[nao verificado]** (`R:4-6`).

   **O retorno em 3 segundos existe**, com destino `core:dashboard`. **O RED nas duas cascas é possível**: já existe composição desse template com `base.html` e `base_app.html` reais (`app/templates/ponto/bater_ponto_v2.html:157`; `app/ponto/tests/test_l030_templates_renderizados.py:15-30`). RED/verde novos executados **[nao verificado]**.

2. **NAO PROCEDE — os “senão” pedidos estão definidos.**

   O literal cobre os três status conhecidos e encaminha qualquer outro, ausência de status ou JSON ilegível à mensagem de confirmação impossível. Também encaminha sítios fora de `templates/` ao BACKLOG e retira o sincronizador offline do escopo (`R:3-5`). A exigência anterior de mostrar próximo tipo foi expressamente eliminada (`R:4`).

3. **PROCEDE — a abertura de construção encontra a proibição de construtor novo.**

   A manda **CONSTRUIR**, enquanto QUEIMA proíbe **“construtor novo”**. A lista de substituições não inclui QUEIMA; a exceção declarada permite esta revisão pelo Codex, sem liberar construtor (`R:1`, `R:3`, `R:20`; `Q:10`). Redefinição ocorrida, revogação dessa proibição ou construtor de A já em voo **[nao verificado]**. Portanto, o texto não sustenta abrir construtor novo hoje.

   A precedência sobre UI-CAMERA foi corrigida: agora é pouso próprio **depois** dela. Pouso efetivo da UI-CAMERA **[nao verificado]** (`R:3`; `avais_todos.txt`, bloco `ui_camera_alternativa.txt:1`). [nao verificado]

   Permanecem duas exigências que “tela” não dispensa:

   - A v2 é **classe A nominal**, com escada inteira (`app/docs/CORTES.md:409`, `:411`, `:413`).
   - iPhone e Android identificam plataformas; não comprovam, sozinhos, clique nas **duas cascas**. RED renderizado e smoke humano são provas distintas (`R:5`, `R:7`; `CLAUDE.md:202-203`).

   Não há colisão automática apenas pela expressão “smoke no pouso”: PRODUÇÃO-MÁXIMA admite smoke posterior para obra só de tela. Isso não declara os cliques cumpridos nem elimina as demais exigências (`avais_todos.txt`, bloco `producao_maxima.txt:1`, item 5). [nao verificado]

4. **NAO PROCEDE — não encontrei referência material falsa no novo literal de A.**

   Template, POST, três status, temporizador e possibilidade de renderização nas duas cascas conferem nas referências acima. O sincronizador também não anuncia resultado individual; atualiza o indicador de conexão (`app/templates/ponto/bater_ponto_v2.html:322-328`; `app/static/pwa.js:158-202`).

   Censo com N, suite própria e smoke efetivamente realizados continuam **[nao verificado]**; são requisitos futuros de `R:5-7`, não resultados demonstrados. [nao verificado]

**Parte B**

1. **NAO PROCEDE quanto à impossibilidade técnica do procedimento novo; sua entrega completa permanece [nao verificado].**

   Existem os mecanismos necessários para preparar a comparação:

   - **MAPA:** o motor oferece leitura sem gravação, retorno de objetos completos e coleta dos furos. A classificação de cada diferença ainda precisa de fundamento na regra; o retorno numérico não fornece automaticamente essa classificação (`app/ponto/services/fechamento.py:448-483`; `CLAUDE.md:45-51`).
   - **TXT sem escrita:** `montar_linhas_export` existe e declara que apenas devolve linhas e IDs (`app/folha/export.py:690-705`).
   - **Hash:** existe hash de FM por colaborador–competência, excluindo carimbos e outros campos expressamente enumerados. Ele não é, sozinho, hash integral das competências 09/10 (`app/ponto/portas/regen_exportada.py:35-38`, `:62-74`).

   O TXT filtra status e elegibilidade; colaboradores retidos podem ficar fora. Logo, TXT idêntico não substitui a comparação das diferenças desses colaboradores (`app/folha/export.py:716-727`; `CLAUDE.md:45-51`).

   **O MAPA integral dos 50, a lista resultante de B4 e os resultados de B5 não constam do dado fornecido [nao verificado].** “Diferença aplicada = MAPA” é uma comparação possível quando ambos existirem; essa igualdade concreta ainda não está demonstrada (`R:12-14`; `E:1-17`). [nao verificado]

   A foto atual do HEAD fotografa rubricas de FM, recalcula antes de gravar o arquivo e apenas avisa se a gravação falhar. Não satisfaz a foto anterior ao delete; B2 manda justamente reconstruir esse instrumento (`app/ponto/management/commands/recalcular_fechamento.py:57-71`, `:118-139`; `R:11`).

2. **NAO PROCEDE — os “senão” estão presentes.**

   Diferença fora da classe “regra” deixa o colaborador esperando a cura; lista vazia não aplica; comparação diferente determina PAREI com o número (`R:13-14`). Condição literal não satisfeita também encontra o PAROU geral (`CLAUDE.md:37-38`).

3. **PROCEDE na entrega descrita — permanece a questão do DELETE de FM fora de ensaio.**

   A entrega afirma apagar FMs inexistentes na foto e identifica expressamente a pergunta sobre o corte que restringe DELETE em massa a ensaio (`E:8`, `E:16`). No HEAD, a porta correspondente exige banco de ensaio, e o teste exige recusa fora dele (`app/ponto/services/fechamento.py:1764-1768`; `app/folha/tests/test_porta_folha.py:107-112`).

   B1 resolve carimbos; B2 exige reconstrução. Nenhum deles concede exceção explícita para esse DELETE em produção. Assim, **a entrega descrita não pode ser certificada mantendo esse mecanismo**. Solução na reconstrução **[nao verificado]** (`R:10-11`; `E:16`).

   As outras colisões precisam ser distinguidas:

   - **Carimbos:** B1 corrige a restauração byte a byte da entrega. No precedente real, `atualizado_em` recebe agora e `derivado_em` fica **None por padrão**, porque restaurar não é derivar. Não significa colocar todos os carimbos em “agora” (`R:10`; `E:8`, `E:15`; `app/ponto/services/fechamento.py:1383-1393`).
   - **DINHEIRO 1:** B5 acrescenta a prova de frota posterior, no mesmo marco. Continuam obrigatórios reversão antes do apply e hash antes/depois das exportadas; cumprimento **[nao verificado]** (`R:14`; `CLAUDE.md:406-410`).
   - **L-110:** MAPA classificado pela regra é compatível; igualdade de DIFF ou TXT não certifica correção. Fundamento das classificações concretas **[nao verificado]** (`R:12-14`; `app/docs/LEIS.md:136`; `CLAUDE.md:45-51`).
   - **COMPETENCIA-PAGA:** o item 2 preservado ainda exige empilhar depois do pouso quando há arquivo comum. Main/rebase não substitui essa espera. B2 pode ser cumprido respeitando-a; apply antecipado não está autorizado (`R:9`, `R:11`; `P:5`; `E:2`). Pouso efetivo da COMPETENCIA-PAGA **[nao verificado]**. [nao verificado]
   - **QUEIMA:** reconstrução não prova, por si, abertura de outro agente construtor. Continuidade do construtor existente **[nao verificado]**; abertura de novo continua proibida (`E:1`; `R:11`; `Q:10`).

4. **PROCEDE — há referência incorreta preservada e universos não demonstrados.**

   O item 2 original preservado cita `dia_pago.py:359`; o sítio efetivo é **`app/ponto/services/dia_pago.py:359-360`**, onde realmente ocorre delete seguido de recriação. A referência anterior `recalcular_fechamento.py:118` aponta à chamada do serviço; a consulta à guarda está em **`:123-126`** (`P:5`, `P:7`; `app/ponto/management/commands/recalcular_fechamento.py:118-126`).

   As referências e os resultados da entrega pertencem à cópia provisória declarada. Não demonstram reconstrução, suite ou pouso sobre main exigidos por B2; esses resultados são **[nao verificado]** (`E:1`, `E:4-6`, `E:14`; `R:11`). [nao verificado]

   Os **50** são o universo exigido, mas sua lista integral, MAPA por campo/classe, subconjunto B4, hashes 09/10 e igualdade aplicada não foram fornecidos **[nao verificado]** (`P:1`; `R:12-14`; `E:1-17`). Não cabe substituir o novo B4 pelos antigos “50 menos três”: o complemento que estabelecia essa exclusão foi substituído (`R:1`, `R:9`, `R:13`). [nao verificado]

## 15/11 12:30 · sessao 2f880eaea171 · - · - · origem=Ronald (tmux)

Parte B — achados para consolidação:

1. **Executabilidade técnica do MAPA/TXT/hash: não encontrei impossibilidade intrínseca.** O motor oferece leitura sem gravação, objetos completos e coleta de furos (`app/ponto/services/fechamento.py:448`, `:454`, `:471`). O TXT sem escrita existe (`app/folha/export.py:690`, `:697`); há hash por colaborador–competência (`app/ponto/portas/regen_exportada.py:62`). MAPA integral dos 50, associação das classes, lista B4 e resultados B5: **[nao verificado]**.

   A foto existente no HEAD não satisfaz a entrega futura: fotografa rubricas de FM (`app/ponto/management/commands/recalcular_fechamento.py:57`), recalcula primeiro (`:118`) e grava o arquivo depois, permitindo falha (`:127`, `:136`). B2 manda reconstruir; portanto ausência atual do instrumento não demonstra impossibilidade dessa construção (`reemissao_1215.txt:11`).

2. **“Senão”: NAO PROCEDE, no conjunto das cláusulas.** B4 encaminha diferenças não classificadas como regra para espera e lista vazia para não aplicar; B5 determina PAREI nas comparações diferentes (`reemissao_1215.txt:13`, `:14`). A regra geral determina PAROU quando condição literal não fecha (`CLAUDE.md:37`).

3. **Colisões concretas e limites:**

   - **B1 corrige o conflito de carimbos da entrega:** ela restaurava os carimbos antigos; o novo aval manda comparar dados (`entrega_relavra_item2.md:8`, `:15`; `reemissao_1215.txt:10`). No precedente real, `atualizado_em` recebe agora, enquanto `derivado_em` recebe **None por padrão**, pois restaurar não deriva (`app/ponto/services/fechamento.py:1383`, `:1391`, `:1393`). Não se pode afirmar que todos recebem agora; toque e derivação têm significados diferentes (`app/ponto/models.py:454`).
   - **DELETE de FM permanece pergunta não resolvida da entrega:** ela propõe excluir FMs inexistentes na foto e identifica a proibição (`entrega_relavra_item2.md:8`, `:16`). A porta atual de DELETE em massa só permite ensaio (`app/ponto/services/fechamento.py:1764`). B1 altera carimbos; não concede explicitamente exceção para esse DELETE (`reemissao_1215.txt:10`). Colisão de uma implementação que preserve esse mecanismo: **PROCEDE**.
   - A espera pós-COMPETENCIA-PAGA permanece no original preservado; B2 não precisa violá-la: pode reconstruir e empilhar respeitando a precedência (`portao_da_11.txt:5`; `reemissao_1215.txt:9`, `:11`). Apply antecipado não está autorizado; existência de pouso da competência: **[nao verificado]**. [nao verificado]
   - A entrega já existia; B2 não prova abertura de **novo** agente construtor. Colisão específica de B com QUEIMA item 6 depende desse fato: **[nao verificado]** (`entrega_relavra_item2.md:1`; `reemissao_1215.txt:11`; `queima_ate_a_redefinicao.txt:14`).
   - DINHEIRO 1 exige reversão antes, exportada intacta com hash e prova de frota depois no mesmo marco; B5 inclui a última exigência expressamente (`CLAUDE.md:406`; `reemissao_1215.txt:14`). Cumprimento efetivo: **[nao verificado]**.

4. **Referências/universos: parcialmente comprovados; resultados não comprovados.**

   - TXT montado sem escrita: confere; ele filtra elegibilidade e status, portanto pode omitir os retidos (`app/folha/export.py:697`, `:716`, `:725`). TXT igual sozinho não certifica diferenças desses colaboradores (`CLAUDE.md:45`).
   - Hash existente: por colaborador–competência, excluindo carimbos e outros campos; não é hash integral da tabela nem cobre DiaPago (`app/ponto/portas/regen_exportada.py:35`, `:56`, `:62`).
   - Dados de FM completos: usar o objeto retornado evita o recorte do dict; o código documenta campos omitidos (`app/ponto/services/fechamento.py:454`, `:457`).
   - `furo_apurado`/`detalhe` são DiaPago; `derivado_em` é FM (`app/ponto/models.py:369`, `:378`, `:464`).
   - “Diferença aplicada = MAPA” é conferível quando o artefato integral e as medições existirem; neste dado faltam ambos: **[nao verificado]** (`reemissao_1215.txt:12`, `:14`). [nao verificado]

## 15/11 12:30 · sessao 79bd3fc7c81f · - · - · origem=Ronald (tmux)

Achados de lei e precedência, somente leitura:

- **A e B: há impedimento para abrir construtor novo hoje.** A manda CONSTRUIR e B manda reconstruir, mas `queima_ate_a_redefinicao.txt:10` proíbe “construtor novo”. A reemissão substitui quatro avais nomeados e declara exceção apenas à distribuição de trabalho do QUEIMA-UTIL; não revoga esse bloqueio (`reemissao_1215.txt:1`, `:3`, `:11`, `:20`). Redefinição ocorrida e continuidade por construtor já vivo: **[nao verificado]**.

- **B2 versus PORTÃO: precedência continua preservada.** O item 2 original exige, havendo arquivo comum, empilhar depois do pouso da COMPETENCIA-PAGA (`portao_da_11.txt:5`). A entrega modifica `recalcular_fechamento.py` e `fechamento.py`, sobre base provisória (`entrega_relavra_item2.md:1-2`); B2 determina reconstruir sobre main e rebase se COMPETENCIA-PAGA pousar antes (`reemissao_1215.txt:9`, `:11`). Rebase/cópia não demonstram pouso prévio. O estado efetivo de COMPETENCIA-PAGA: **[nao verificado]**. [nao verificado]

- **B1 corrige a colisão dos carimbos.** O selo explica que carimbo parado após escrita afirma fato falso, e cobra carimbo em `update`/`bulk_update` (`app/ponto/tests/test_carimbo_acompanha_a_escrita.py:2`, `:11`, `:14-21`). O precedente real restaura com `atualizado_em=timezone.now()` e `derivado_em=None` por padrão (`app/ponto/services/fechamento.py:1383-1393`). Atenção: o precedente permite `derivado_em` explicitamente pedido; não é proibição absoluta de restaurar esse campo (`app/ponto/services/fechamento.py:1388-1391`).

- **DINHEIRO 1:** B5 agora exige prova de frota depois, no mesmo marco (`reemissao_1215.txt:14`), alinhada a `CLAUDE.md:406-410`. Permanecem necessários arquivo de reversão em `logs/` antes do apply e hash das exportadas antes/depois; prova na sombra não substitui prova posterior (`CLAUDE.md:407-410`). Cumprimento: **[nao verificado]**.

- **L-110:** igualdade de DIFF/TXT não certifica correção. Referência é regra aplicada ao cadastro/batidas, e DIFF mede impacto (`CLAUDE.md:45-51`; `app/docs/LEIS.md:136`). B3/B4 exigir classe “regra” é compatível em tese; MAPA, fundamento de cada classe e certificação correspondente: **[nao verificado]** (`reemissao_1215.txt:12-14`).

- **Front:** v2 é nominalmente classe A e pede escada inteira (`app/docs/CORTES.md:411`, `:413`). Renderizar duas cascas é obrigatório; clique iPhone/Android não prova sozinho clique nas duas cascas (`CLAUDE.md:202-203`; `reemissao_1215.txt:5`, `:7`). Não declarar colisão automática apenas por smoke no pouso: PRODUÇÃO-MÁXIMA autoriza smoke posterior para obra só de tela (`producao_maxima.txt:1`, item 5).

- **DELETE de FM permanece pergunta material da entrega.** Ela afirma apagar FM nascido depois da foto e pergunta sobre o corte (`entrega_relavra_item2.md:8`, `:16`). No HEAD, a porta de DELETE em massa é exclusiva de ensaio (`app/ponto/services/fechamento.py:1764-1768`), com teste que exige recusa fora dele (`app/folha/tests/test_porta_folha.py:107-112`). B1 responde carimbos; não responde explicitamente esse DELETE (`reemissao_1215.txt:10-14`). Isso impede certificar a entrega descrita; a implementação reconstruída e eventual solução sem essa colisão: **[nao verificado]**.

## 15/11 12:29 · sessao 83b17424df27 · - · - · origem=Ronald (tmux)

A — achados para consolidação:

1. **NAO PROCEDE quanto à executabilidade da mudança.** Hoje `r.ok` anuncia sucesso sem ler JSON; só o caminho não 2xx lê JSON (`app/templates/ponto/bater_ponto_v2.html:327-328`). O literal novo corrige isso, reconhece `'ok'`, elimina o próximo tipo e define saída para status desconhecido/ausente e JSON ilegível (`anexo avais_todos.txt, bloco reemissao_1215.txt:4`).

   - Os três status existem em `Response` sem código HTTP alternativo nos dois kernels: `app/api/views_core.py:521-531`, `:673-683`, `:807-818`; `app/api/views.py:535-545`, `:682-692`, `:813-824`. Testes existentes exigem HTTP 200 com `'ok'`/`'ja_registrada'` (`app/api/tests/test_api_bater_ponto.py:280-283`) e exercitam `'ignorada_intervalo'` (`:638-639`).
   - O quarto ramo é tratamento defensivo: resposta desse tipo efetivamente emitida pela API/prod **[nao verificado]**. Um RED com fixture HTTP 200 é compatível com a exigência literal (`anexo reemissao_1215.txt:4-5`); não é necessário que o servidor atual emita JSON inválido.
   - O retorno em 3 s existe e leva ao dashboard (`app/templates/ponto/bater_ponto_v2.html:157`; `app/core/urls.py:17`).
   - Renderizar o mesmo template nas duas bases reais já tem modelo implementado (`app/ponto/tests/test_l030_templates_renderizados.py:15-30`). RED/verde novos e cliques efetivos **[nao verificado]**.

2. **NAO PROCEDE quanto a “senão” ausente.** O novo literal define fallback para todos os demais status/ausência/JSON ilegível e destina sítios fora de `templates/` ao BACKLOG (`anexo reemissao_1215.txt:4-5`). Condição geral que não fecha também determina PAROU (`CLAUDE.md:37-38`). Não depende mais de próximo tipo nem da cura offline (`anexo reemissao_1215.txt:3-4`).

3. **PROCEDE pela proibição vigente de construtor novo.** A abre CONSTRUIR (`anexo reemissao_1215.txt:3`), enquanto QUEIMA proíbe “construtor novo” (`anexo queima_ate_a_redefinicao.txt:10`). A lista explícita de substituições não inclui QUEIMA; a exceção declarada ao QUEIMA-UTIL-TARDE é somente esta revisão pelo Codex (`anexo reemissao_1215.txt:1`, `:22`). Redefinição ocorrida ou outra revogação **[nao verificado]**.

   Ressalvas sem converter em colisão automática:
   - “Tela” não dispensa a escada inteira: `bater_ponto_v2.html` é exceção nominal classe A (`app/docs/CORTES.md:409`, `:411`, `:413`).
   - iPhone e Android identificam plataformas, não comprovam duas cascas; smoke humano nas duas cascas permanece exigido (`CLAUDE.md:202-203`; `app/docs/LEIS.md:99`). A não o revoga (`anexo reemissao_1215.txt:7`).
   - Pouso próprio depois da UI respeita a precedência declarada (`anexo reemissao_1215.txt:3`; `anexo ui_camera_alternativa.txt:1`). Estado atual/pouso/suite da UI **[nao verificado]**. A menção a `estado_uicam.txt` na revisão antiga não fornece esse arquivo no dado atual. [nao verificado]

4. **NAO PROCEDE quanto a referência material falsa demonstrada em A.** Template, chamada, três status, timer e possibilidade de renderização nas duas bases conferem nos sítios acima (`app/templates/ponto/bater_ponto_v2.html:150-157`, `:322-328`; `app/api/urls_core.py:14`; `app/config/urls_core.py:6`; `app/ponto/tests/test_l030_templates_renderizados.py:15-30`). O offline não anuncia resultado individual: apenas atualiza indicador de conexão (`app/static/pwa.js:182-202`). Censo com N, RED/verde, suite e smoke pedidos são entregas futuras **[nao verificado]**.

_5640 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
