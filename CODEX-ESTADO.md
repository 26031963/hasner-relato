# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 10:58** · publicado em **08/11/2026 10:58** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 10:58 · sessao 87740fa7e361 · - · - · origem=Ronald (tmux)

S1..S6 no código atual:

| Sítio | Resultado e prova |
|---|---|
| S1 | `classificar_dia` é pura: recebe `batidas_no_dia`, sem construir universo (`app/ponto/supra_juiz.py:204`, `:207`). Usa a quantidade como fallback quando `dia` não tem `orfas` (`:513`, `:521`). O produtor do command segue **cru**: query `app/ponto/management/commands/supra_juiz.py:113`, contagem `:222`, passagem `:229`. Também há o produtor cru em `app/ponto/management/commands/emitir_furo_retroativo.py:53`, contagem `:100`, passagem `:106`. Já o cartório calcula sobre células da grade e órfãs (`app/ponto/services/cartorio.py:1373`), passando o resultado em `:1419`. Portanto a localização antiga não representa o sítio de leitura atual. |
| S2 | **Cru-JULGAMENTO**: `veredito_celula` carrega `Batida.objects.filter` sem marca em `app/ponto/services/veredito_celula.py:215`; passa as reais para `grade_dia` em `:220`; o resultado decide “nada a corrigir” em `:223`. `grade_dia` apenas encaminha as batidas à grade (`app/ponto/services/simular_dia.py:5`); a grade monta `locs` sem excluir retratação (`app/escala/utils.py:1344`). |
| S3 | `_batida_existente_na_janela` é **crua**, documentada como idempotência (`app/chamados/juizes.py:227`, query `:235`). O chamador de julgamento já exige a marca nula **no objeto retornado** (`:774`, `:775`): cura explícita local. A razão de manter a função crua para não replantar retratadas está escrita em `:768`. Materialização reutiliza a existente em `app/chamados/services/materializacao.py:834`, `:840`; apresentação também decide `ja_satisfeita` lendo apenas `aprovada`, sem checar retratação, em `:446`, `:447`. Não é lookup por chave, e sim por janela; separar seu uso de idempotência do uso de satisfação. |
| S4 | O caminho `app/ponto/services/materializacao.py` não existe nesta cópia [não verificado por arquivo:linha]. O código correspondente está em `app/chamados/services/materializacao.py:85`, `_vizinha_mesmo_tipo`, com duas queries **cruas**, `:94` e `:100`. Decide bloqueio/recusa de materialização em `:451`, `:452`, `:850`, `:851`. |
| S5 | `pares_mesmo_tipo` continua **cru-JULGAMENTO**, query em `app/ponto/services/flip_auto.py:35`; essa sequência define os pares em `:41`–`:46` e alimenta a fila em `:146`. Há cura separada da contagem de dia: `classificar_pares` usa `batidas_do_dia_turno` em `:104`, autoridade que exclui retratação por `turnos_do_colab` (`app/ponto/turnos.py:3024`, `:3033`). Isso não cura a query de formação dos pares. |
| S6 | **Cru-JULGAMENTO** na própria função: `proxima_batida_de` lê a última batida sem marca em `app/ponto/utils.py:244` e decide o tipo em `:246`. Não encontrei chamada dessa função em produção por busca AST/textual [não verificado quanto a chamadas dinâmicas]. A autoridade registrada para a próxima batida é outra: `ponto/turnos.py::proximo_tipo_de`, em `app/core/juizes.py:197`. |

Não é possível provar commits pela cópia sem metadados Git [não verificado]. As curas acima têm prova nas linhas atuais, não em comentários históricos. [nao verificado]

Selos:

- **B5-j** vigia chamadas a `parear_turnos` e `_turnos_do_colab_puro`, incluindo aliases de importação (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63`, `:66`, `:118`). Busca apenas `Batida.objects` literal dentro do escopo que chama essas funções (`:89`, `:93`). Considera qualquer query cuja representação contenha `retratada_em` conforme/copiadora; aceita referência à porta ou lookup `pk/id` (`:125`–`:131`). Não resolve fluxo entre funções nem managers relacionados; os limites declarados estão em `:26`–`:34`. Executando apenas suas funções AST em memória, o resultado foi **0 crus e 6 copiadores** [não verificado por artefato persistido; execução somente leitura].
- **Bocais** limita-se a seis arquivos: `ponto/services/fechamento.py`, `folha/export.py`, `relatorios/views.py`, `relatorios/pdf_espelho.py`, `ponto/views.py`, `api/views.py` (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`). Conta texto que casa `(?:Batida|_Bat)\.objects\.filter`, sem AST nem inspeção do predicado (`:25`, `:29`, `:30`). Permite 3 ocorrências em `ponto/views.py` e 7 em `api/views.py` (`:18`, `:20`).
- **Todos os sítios S1..S6 e os dois commands produtores crus de S1 ficam fora dos dois selos**, pelas restrições acima; executei `universos` sobre esses oito arquivos e todos retornaram `([], [])` [não verificado por artefato persistido]. Nenhum desses arquivos está no `ALLOW` dos bocais (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`).

Juiz:

- **Não há autoridade registrada para “esta batida conta?”**: a família `batida` registra períodos/intervalo, horas do par, geofence e selfie (`app/core/juizes.py:1173`–`:1183`); `turno/marcos` registra outras perguntas, não o universo apurável (`:195`–`:237`).
- **Não precisa criar função/lei nova para o universo**: `batidas_apuraveis` já se declara autoridade única, exclui retratadas e preserva a distinção de exibição (`app/ponto/turnos.py:2844`–`:2850`, filtro `:2864`); a lei a nomeia em `CLAUDE.md:155`.
- Registrar agora essa autoridade no registro vivo exige observar a regra de nascimento de autoridade, que pede corte nominal (`CLAUDE.md:74`–`:76`). Isso é decisão de registro; não demonstra lacuna na lei que define o universo (`CLAUDE.md:155`).

Decisões técnicas extraídas:

1. Migrar os universos de S1 nos dois commands para a porta; `classificar_dia` recebe insumo e não deve ganhar query (`app/ponto/management/commands/supra_juiz.py:113`, `app/ponto/management/commands/emitir_furo_retroativo.py:53`, `app/ponto/supra_juiz.py:207`).
2. S2, S4 e a formação de pares de S5 devem consultar universo apurável, pois seus resultados escolhem materialização/paridade/flip (`app/ponto/services/veredito_celula.py:215`, `app/chamados/services/materializacao.py:94`, `app/ponto/services/flip_auto.py:35`).
3. Manter o uso cru de S3 para impedir repetição de escrita; separar o julgamento de satisfação, que precisa de prova viva (`app/chamados/juizes.py:768`–`:775`; apresentação ainda sem essa marca em `app/chamados/services/materializacao.py:447`).
4. Para S6, escolher remoção de código sem chamadas localizadas ou delegação à autoridade já registrada da próxima batida (`app/ponto/utils.py:240`, `app/core/juizes.py:197`); chamadas dinâmicas não comprovadas [não verificado].
5. Ampliar selo para as leituras de julgamento que não chamam pareador e para aliases/managers relacionados; hoje os limites são explícitos (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:26`, `:89`; bocais `app/ponto/tests/test_tripwire_bocais_apuracao.py:13`, `:25`).

Nenhuma dessas mudanças de universo exige lei nova: a distinção apuração/exibição já está escrita (`CLAUDE.md:155`, `app/ponto/turnos.py:2847`). A exceção de idempotência tem justificativa técnica expressa no código (`app/chamados/juizes.py:768`).

## 08/11 10:58 · sessao 5a08669ba673 · - · - · origem=Ronald (tmux)

Lei e semântica: `CLAUDE.md:155-164`, `app/ponto/models.py:64-67` e `app/ponto/turnos.py:2844-2871` concordam: `retratada_em != NULL` exclui geometria/apuração, preserva exibição; `aprovada` significa crivo do DP, e pendente do DP ainda conta.

**Escritores encontrados fora de tests/migrations:**

- Escritor físico genérico: `app/ponto/registro_batida.py:235`, `:252-254` (`anotar`, `setattr` e `save(update_fields)`); campo permitido em `:205`.
- Retratar: `app/ponto/registro_batida.py:267`, `:271-274`: idempotência, grava `aprovada=False` e `retratada_em=timezone.now()`. Agenda relavra financeira em `:281`.
- Desfazer: `app/ponto/services/retratar_humano.py:190`, `:197-209`: grava `aprovada=True` e `retratada_em=None` pela mesma `anotar`.
- Chamadores que mandam retratar: `app/chamados/services/acoes_chamado.py:269`; `app/ponto/management/commands/detectar_par_relampago.py:224`; `app/ponto/management/commands/diff_passivo_s84.py:82` (ensaio, conforme `:75-76`, `:91`); `app/ponto/management/commands/passivo_disputa_retro.py:164`; `app/ponto/services/retratar_humano.py:179`.
- `app/ponto/services/retratar_humano.py:9-12` é histórico e já não serve como lista completa: afirma dois chamadores, mas os cinco acima existem.
- `app/ponto/registro_batida.py:152-158` aceita `**campos` na criação; não há passagem literal atual de `retratada_em` identificada nessas chamadas. Conteúdo arbitrário recebido dinamicamente: [não verificado].

**Leitores executáveis de `retratada_em`, levantados por AST (linhas; funções):**

| Arquivo | Leitores |
|---|---|
| `app/api/views.py` | `705 api_bater_ponto` |
| `app/api/views_core.py` | `702 api_bater_ponto` |
| `app/chamados/juizes.py` | `720,775 _pergunta_materializada_de_fato` |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py` | `73 handle` |
| `app/chamados/reconciliador.py` | `216 _par_integro` |
| `app/chamados/services/adesao.py` | `60,145 sincronizar_adesao` |
| `app/chamados/services/desvio_escala.py` | `60 medir_desvio` |
| `app/chamados/services/fila_por_causa.py` | `191 particionar` |
| `app/chamados/services/limbo_folgas.py` | `37 sugerir_folga`; `156 semana_muda` |
| `app/chamados/services/materializacao.py` | `616 perguntas_absorvidas_por` |
| `app/chamados/services/vigia_carimbo.py` | `113 classificar` |
| `app/chamados/veredito_lavrado.py` | `140-141 marcos_do_lote` |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | `95 _rodar` |
| `app/colaboradores/services/calendario.py` | `240,621 contexto_calendario` |
| `app/colaboradores/services/ficha.py` | `145 ficha_do_colab` |
| `app/colaboradores/services/pos_vinculo.py` | `38 fechar_adesao_com_batida` |
| `app/colaboradores/services/senha_provisoria.py` | `25 senhas_provisorias` |
| `app/core/services/painel_op.py` | `93 montar_painel_operacional` |
| `app/core/views.py` | `173 dashboard`; `372 dashboard_supervisor`; `775 painel_tv`; `1177 dashboard_dados` |
| `app/escala/services/meio_periodo.py` | `160 observado_meio_periodo` |
| `app/escala/services/propositor.py` | `478 propostas` |
| `app/ferias/situacao.py` | `278 situacao_de_ferias` |
| `app/ponto/management/commands/diff_calculador.py` | `271 handle` |
| `app/ponto/management/commands/diff_janela_he.py` | `94 handle` |
| `app/ponto/management/commands/diff_passivo_s84.py` | `80 _mutacao` |
| `app/ponto/management/commands/e6_piso_autopsia.py` | `72 handle` |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py` | `68 handle` |
| `app/ponto/management/commands/passivo_disputa_retro.py` | `161 handle` |
| `app/ponto/motor_calculo_v2.py` | `671 _periodos_pelo_marco` |
| `app/ponto/registro_batida.py` | `109,141 registrar_batida`; `271 retratar_batida`; leitura genérica dos valores anteriores em `244` |
| `app/ponto/services/adesao.py` | `35 primeira_batida`; `58 ids_com_adesao` |
| `app/ponto/services/cartorio.py` | `353 houve_fato_novo`; `1155 _julgar_colab_corpo` |
| `app/ponto/services/deslogados.py` | `161,174 apurar` |
| `app/ponto/services/faltas_de_hoje.py` | `55 faltas_de_hoje_sem_cobranca` |
| `app/ponto/services/flip_auto.py` | `229 etiquetas_pela_celula`; `287,298 avaliar_fila` |
| `app/ponto/services/flip_batida.py` | `15 flip_tipo` |
| `app/ponto/services/lastro.py` | `262 julgar` |
| `app/ponto/services/passivo_s84.py` | `54 vivas` |
| `app/ponto/services/piso_intervalo.py` | `41 pares_abaixo_do_piso` |
| `app/ponto/services/retratar_humano.py` | `48,53 prazo_de_desfazer`; `67,75 prazos_de_desfazer`; `88 decisao_de`; `173 retratar`; `197 desfazer` |
| `app/ponto/turnos.py` | `1600,1612 papel_por_minuto_da_ata`; `1862 turnos_do_colab`; `1900 _ultima_bat`; `2219 _turno_aberto_calc`; `2351 turnos_abertos_de`; `2382 sequencia_relogio_x_jornada`; `2467 proximo_tipo_de`; `2864 batidas_apuraveis`; `2903 dia_das_batidas` |

Essas referências cobrem atributos, `getattr`, filtros ORM e campo temporal dinâmico; não incluem comentários/docstrings nem snapshots `antes` de auditoria como se fossem leitura da coluna. Exemplos de snapshots: `app/ponto/management/commands/diff_passivo_s84.py:84`, `app/ponto/management/commands/passivo_disputa_retro.py:146,170`.

**Interpretações especiais que preservam o significado:**

- Exibição busca deliberadamente somente retratadas em `app/colaboradores/services/calendario.py:239-244`, organiza o render separado da apuração em `:619-621`, e o template desenha riscado e informa “fora do cálculo” em `app/templates/colaboradores/partials/_calendario_grade.html:121`.
- Invalidação usa o instante da retratação como fato novo, sem contar a marca como trabalho: `app/chamados/veredito_lavrado.py:137-142`; `app/ponto/services/cartorio.py:329-353`.
- Prazo/desfazer e decisão humana leem data para ações e explicação: `app/ponto/services/retratar_humano.py:42-53,66-77,80-101,190-210`.
- `_par_integro` consulta cru em `app/chamados/reconciliador.py:206`, mas recusa se membro selecionado foi retratado em `:216-217`: o resultado final tem filtro explícito em memória.
- `_batida_existente_na_janela` é propositalmente compartilhado com idempotência de escrita; justificativa atual em `app/chamados/juizes.py:768-773`; o leitor que prova materialização exige `retratada_em is None` em `:774-775`. Mudar o helper inteiro sem separar as duas perguntas pode replantar aquilo que o cron retratou, conforme `:770-772`.

**Falha semântica adicional que exige atenção no censo principal:** `app/ponto/services/flip_auto.py:291-292` carrega `reais` cru; `:298` exclui retratadas apenas da lista de flips, mas `:303` passa a lista crua inteira para `grade_dia` no cálculo de `p0`. Portanto, a presença de um leitor correto de `retratada_em` nessa função não prova que todo o julgamento usa o universo correto.

Todos os **leitores diretos identificados** reconhecem retratação como exclusão, apresentação ou instante de mudança pelos exemplos citados; o sistema inteiro não aplica uniformemente a exclusão, como demonstra `flip_auto.py:291-303`. Garantia sobre estado efetivo em produção: [não verificado].

## 08/11 10:52 · sessao d2eb00cbbb9c · SELOS-DO-CANON-0811 · leitura · origem=Code

As referências abaixo são da cópia fornecida. Ela não contém `.git`; não consegui revalidar sua identidade como HEAD `80df6a5dc22bd4868696699ab8bdb51ce033198d`. [nao verificado]

**I-1 — CENSO-ANTES-DA-VAGA**

**(a) Quem chama hoje.** Existe o ponto de interceptação: `.claude/settings.json:62` configura `PreToolUse`, chamando `disjuntor.py pretool` em `.claude/settings.json:68`. Porém, o ramo `Agent` atual lê o prompt e aplica B3/B4; não abre brief nem valida CENSO. Portanto, **o hook geral existe; a guarda de abertura do construtor seria nova**. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`)

O protocolo também difere: o disjuntor comunica negação por JSON `permissionDecision=deny`, e seu `main_pretool` retorna zero. O contrato novo de validador com `rc 1` ainda não corresponde a esse protocolo existente. (`bin/disjuntor.py:1395`, `bin/disjuntor.py:1493`, `bin/disjuntor.py:1507`)

Os briefs aparecem como artefatos preparados e despachados pela sessão: E1 “brief pronto” e depois abertura do construtor; AKMED e PLANTIO também têm caminhos registrados. **Não há nomenclatura uniforme:** `logs/fio_e0_v4/e1_brief.md` não casa com `logs/<obra>/brief*.md`; `logs/v2bugs/i2/brief_v6.md` tem um nível adicional. (`app/docs/RELATO.md:95`, `app/docs/RELATO.md:91`, `app/docs/RELATO.md:80`, `app/docs/RELATO.md:75`, `app/docs/RELATO.md:205`)

No inventário desta cópia: **zero arquivos `logs/*/brief*.md`, zero obras observáveis por esse glob**; `logs/` não foi fornecido. Quantas obras os possuem em produção: **[nao verificado]**. O diretório está ignorado pelo Git, portanto sua ausência na exportação não prova ausência no vivo. (`.gitignore:53`)

**(b) Colisão com PAREI.** A linha contratada **não satisfaz nenhum dos dois caminhos atuais**:

- PAREI precisa conter `!`; pergunta de lei tampouco devolve turno por esse caminho. Além disso, deve travar o item em curso, sem outros itens andáveis. (`bin/hook_stop_fila1.py:1109`, `bin/hook_stop_fila1.py:1119`, `bin/hook_stop_fila1.py:1155`, `bin/hook_stop_fila1.py:1076`)
- Espera provada precisa começar literalmente por `ESPERANDO:` e declarar `pid=` ou `arquivo=`. `PAREI: censo-<obra> | espera Codex` não declara nenhum deles. (`bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:665`, `bin/hook_stop_fila1.py:673`)

Os sítios envolvidos na compatibilização são o reconhecedor de espera, sua memoização por Stop, o orçamento que fica pendente durante espera provada e a liberação final. Mudar apenas a decisão final deixaria o orçamento tratando o censo por outro caminho. (`bin/hook_stop_fila1.py:645`, `bin/hook_stop_fila1.py:688`, `bin/hook_stop_fila1.py:984`, `bin/hook_stop_fila1.py:1231`)

| Leitor/selo solicitado | O que faz hoje e a colisão |
|---|---|
| `test_hook_parei_so_lei.sh` | Exige `_SO_BANG`; rejeita decisão técnica e lei como portas; aceita `!`. Relaxar todo PAREI contrariaria esses casos. (`bin/tests/test_hook_parei_so_lei.sh:25`, `bin/tests/test_hook_parei_so_lei.sh:41`, `bin/tests/test_hook_parei_so_lei.sh:52`, `bin/tests/test_hook_parei_so_lei.sh:60`) |
| `test_hook_esperando_com_prova.sh` | Distingue produtor vivo, morto, outro programa e declaração sem prova; verifica que a liberação ocorreu especificamente por espera provada. (`bin/tests/test_hook_esperando_com_prova.sh:67`, `bin/tests/test_hook_esperando_com_prova.sh:93`) |
| `test_disjuntor.sh` | Exercita PAREI CLEAR, DIFF em curso, agente sem entrega e COTA com handoff; também impede usar arquivo escrito pelo próprio hook como prova. São interações a preservar, sem um caso censo demonstrado. (`bin/tests/test_disjuntor.sh:192`, `bin/tests/test_disjuntor.sh:200`, `bin/tests/test_disjuntor.sh:215`, `bin/tests/test_disjuntor.sh:477`, `bin/tests/test_disjuntor.sh:578`) |
| `handoff_sessao.sh` | Já exibiria a linha censo, mas diria “item em curso ANDA”/“não devolve turno”: consulta somente os predicados de PAREI genérico. (`bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`) |
| `alarme_sessao_ociosa.py` | Já desarma com qualquer substring `PAREI:` nas primeiras 40 linhas, sem prova. Pode silenciar enquanto o Stop continua mandando seguir. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`) |

A prova PID atual verifica processo vivo, identidade do executável/script e estabilidade do PID durante a leitura; **não consulta `CODEX_OBRA` nem `CODEX_PAPEL`**. A alternativa por arquivo exige avanço de mtime; a primeira observação não prova progresso. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:548`, `bin/hook_stop_fila1.py:640`)

O wrapper exige obra e papel, mas isso não estabelece vínculo entre o novo PAREI e um censo vivo daquela obra. Sua emissão explícita de `ESPERANDO` com PID/JSONL ocorre no ramo **construtor**, não no papel censo. (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:375`)

Assim, o lastro específico “produtor vivo do censo desta obra” permanece **[nao verificado]**. Aceitar somente o literal dispensaria o requisito de prova; aceitar qualquer Codex vivo não distinguiria obra nem papel, campos que o wrapper já separa. (`bin/hook_stop_fila1.py:673`, `bin/codex_pergunta.sh:81`)

**(f) Furos.** O ramo atual não fotografa versão/conteúdo do brief; portanto não estabelece identidade abertura→brief. Também depende da passagem pelo hook configurado, e a chave `DISJUNTOR_DESLIGADO` pula suas verificações; exceções fora do ramo B3 liberam a ferramenta. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1496`)

Cobertura de agente aberto por outra via, allowlist vazia efetivamente guardada e proteção contra brief alterado após abertura: **[nao verificado]**. Existe ainda outra abertura de construtor, pelo wrapper Codex, com vaga própria e execução própria; cobri-la não decorre automaticamente do ramo `Agent`. (`bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:394`)

**I-2 — UNIDADE-NO-AVAL**

**(a) Quem registra hoje.** A responsabilidade declarada é do **Code/sessão**: prompt recebido vira linha em `PROMPTS.md`, e pedido de obra vira BACKLOG no mesmo turno. Ronald aparece como autor dos avais registrados. Não localizei uma porta executável única responsável por registrar e despachar esses avais. (`CLAUDE.md:433`, `app/docs/PROMPTS.md:3`, `app/docs/PROMPTS.md:1931`; registrador único: **[nao verificado]**)

Os registros apontam arquivos privados como `logs/plantio_mais1/aval_i.txt`; quem efetivamente escreveu esses arquivos, e por qual ferramenta, é **[nao verificado]**. (`app/docs/PROMPTS.md:1931`, `.gitignore:53`)

`gerar_avais.py` não resolve essa identidade: ele transforma `PENDENTES_RONALD.json` em `AVAIS.md`, a lista de pedidos abertos para Ronald responder. Não é o escritor de `PROMPTS.md`/`aval_*.txt`. (`bin/gerar_avais.py:3`, `bin/gerar_avais.py:30`, `bin/gerar_avais.py:105`)

**(d) Marcadores detectáveis nos registros disponíveis.** Há exemplos textuais das seis classes; isso comprova vocabulário, não um classificador formal. (`app/docs/PROMPTS.md:1477`, `app/docs/PROMPTS.md:1931`, `app/docs/PROMPTS.md:1933`, `app/docs/PROMPTS.md:1966`, `app/docs/PROMPTS.md:1967`)

| Classe | Exemplo no registro |
|---|---|
| Backfill | “backfill de data_evento”. (`app/docs/PROMPTS.md:1931`) |
| Migração | “migracao e backfill chamam a MESMA porta”. (`app/docs/PROMPTS.md:1967`) |
| Dedup | “oraculo E6 perde o dedup < 10 min”. (`app/docs/PROMPTS.md:1477`) |
| Juiz | “juiz tarefa_do_dia nasce”. (`app/docs/PROMPTS.md:1933`) |
| Veto | “MESMO veto de merito”. (`app/docs/PROMPTS.md:1967`) |
| Catraca | Contagem de `select_for_update` sobre modelos nomeados. (`app/docs/PROMPTS.md:1966`) |

**Falso positivo concreto:** `dedup` aparece em instrumento de medição de consumo do Code; `catraca`/`juizes` aparecem numa ordem de gestão da noite. Casar essas palavras classificaria também esses textos como DADO. (`app/docs/PROMPTS.md:1872`, `app/docs/PROMPTS.md:1839`)

**Falso negativo no resumo disponível:** E1-FATO-E-O-TURNO muda o critério dos selos que recusam por `data_evento`, sem empregar nenhuma das seis palavras. O texto integral privado desse aval não foi fornecido. (`app/docs/PROMPTS.md:1977`; integral: **[nao verificado]**)

A isenção por título também falha: TOAST-E-NOME-CALENDARIO reúne veto de criação/salvamento e apresentação de toast no mesmo registro. (`app/docs/PROMPTS.md:1828`)

A memória citada tem exemplos explícitos: grep contou 13 arquivos contra nove por AST porque prosa citava a palavra; na O118, identidade por texto `HH:MM` produziu falsos positivos, corrigidos consultando o predicado de batidas apuráveis. (`bin/atomicidade_da_porta.py:41`, `app/docs/BACKLOG.md:1199`)

**(f) Furos.** Autoridade para classificar DADO versus gestão/instrumento/UI, tratamento de aval misto, campos vazios e caminho obrigatório antes do despacho não estão estabelecidos por um mecanismo localizado nesta cópia. **[nao verificado]**

Aval apenas no chat fica fora do alcance de um fiscal que só examina arquivo. O selo atual parte de promessas **já registradas** em PROMPTS e verifica IDs no BACKLOG; não comprova que todo aval recebido entrou no registro. (`bin/tests/test_prompt_virou_item.sh:21`, `bin/tests/test_prompt_virou_item.sh:30`)

**I-3 — ETAPAS-NO-BRIEF**

**(a/c) Abertura, opus final e B3.** A abertura compartilha o sítio do I-1, atualmente sem leitura do brief. O verificador recebe ID da obra, diff e log RED; seu checklist não exige brief, RELATO do pouso ou correspondência entre etapas e resultados. (`bin/disjuntor.py:1451`, `.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:8`)

O **B3 atual não faz essa conferência**. Ele identifica obra/leitura final pelo prompt e extrai `APROVADO`/`REPROVADO` da primeira linha do `SubagentHandback`, filtrando agentes do tipo `verificador`. (`bin/disjuntor.py:546`, `bin/disjuntor.py:561`, `bin/disjuntor.py:742`)

Ele lê o RELATO para fotografar duas linhas úteis do topo e reconhecer `DESENHO`/`ERRO-BOBO` que liberam a obra. Conta reprovações e voltas Codex procedentes, não etapas entregues. (`bin/disjuntor.py:662`, `bin/disjuntor.py:626`, `bin/disjuntor.py:910`)

Há uma lacuna temporal: **o opus final precede o pouso**, enquanto a linha de medida é exigida no RELATO **do pouso**. O artefato e o instante da conferência de etapas antes de autorizar esse pouso não estão definidos no código atual. (`CLAUDE.md:374`, `CLAUDE.md:379`, `.claude/agents/verificador.md:3`)

**Como contaria hoje:** não há contador demonstrado. Pela letra do anexo, seria necessário comparar as etapas declaradas no brief com as etapas que têm resultado medido no RELATO; contar ocorrências de “etapa K” não comprova essa relação. Identidade de versão, resultado antigo versus atual e resultado esperado versus medido permanecem sem reconhecedor verificado. **[nao verificado]**

O checador de prova existente também não supre isso: procura `PROVA:` próximo de certos atos em título/negrito, sem comparar etapa, comando ou valor medido. (`bin/relato_afirma_com_prova.py:36`, `bin/relato_afirma_com_prova.py:47`)

**(f) Furos.** Brief editado depois da abertura perde uma referência estável para o denominador das etapas; o ramo atual não guarda essa referência. Além disso, B3 só reconhece os tipos/prompts finais previstos, e o verificador manda retornar apenas problemas — até 15 linhas —, sem relatório positivo N/N. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:555`, `bin/disjuntor.py:742`, `.claude/agents/verificador.md:18`)

Proteção específica contra reescopo, uso de brief anterior ou aval de alteração mantido apenas no chat: **[nao verificado]**.

**(e) MEDIDA — comum aos três instrumentos**

`ESTADO.md` é gerado por `bin/gerar_estado.py`, chamado por `relato.sh` para `$TMP/ESTADO.md`, e publicado pela raspagem. O gerador grava o destino recebido; não depende de existir `app/docs/ESTADO.md`. (`bin/relato.sh:38`, `bin/gerar_estado.py:390`, `bin/relato.sh:105`)

**Já existe molde de janela móvel de 24 h:** `disjuntor.ler` usa `agora - 86400`, lê eventos datados e conta disparos, negações e liberações. O ESTADO consome esse medidor por `ler/linha`. (`bin/disjuntor.py:1546`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1570`, `bin/gerar_estado.py:277`)

Há também contagem de chamadas Codex por papel em 24 h, baseada em eventos `ini`; e média de espera CLEAR baseada em `espera=Ns`. **Essa média não mede horas de vaga ociosa esperando censo.** (`bin/codex_limite_estado.py:24`, `bin/codex_limite_estado.py:71`, `bin/disjuntor.py:1580`, `bin/disjuntor.py:1688`)

Os contadores atuais não demonstram as novas medidas de I-1/I-2/I-3; zero construtores sem censo, 100% de avais conformes e 100% de pousos N/N em produção permanecem **[nao verificado]**. Os campos hoje publicados são os explicitados em `bin/disjuntor.py:1615` e `bin/disjuntor.py:1691`.

## 08/11 10:50 · sessao 3977fc5450fa · - · - · origem=Ronald (tmux)

**I-2 — quem registra.** A obrigação atual é do Code/sessão: prompt recebido ganha linha em `PROMPTS.md` e obra ganha item no BACKLOG no mesmo turno (`CLAUDE.md:433-435`; `app/docs/PROMPTS.md:3-9`). Ronald aparece como autor do aval; os registros dizem “recebido” e “repassado ao construtor” (`app/docs/PROMPTS.md:1948-1950`). Não localizei registrador central executável, validador `UNIDADE=`, diretório `avais_recusados` ou selo I-2 na cópia **[não verificado]**. O executor físico das escritas privadas em `logs/*/aval_*.txt` é **[não verificado]**; `logs/` está ignorado (`.gitignore:53`).

`gerar_cortes.py` é outro instrumento: lê `CORTES.json` e gera `CORTES.md`/tabela do RELATO; não registra o recebimento do aval (`bin/gerar_cortes.py:3-10`, `:23-25`). Handoff e placar **leem** PROMPTS (`bin/handoff_sessao.sh:128-140`; `bin/placar_code.sh:395-396`).

| Classe | Marcador textual já encontrado em registro de aval |
|---|---|
| Backfill | “backfill de data_evento” — `app/docs/PROMPTS.md:1931` |
| Migração | “migracao e backfill chamam a MESMA porta” — `app/docs/PROMPTS.md:1967` |
| Dedup | “oraculo E6 perde o dedup < 10 min” — `app/docs/PROMPTS.md:1477` |
| Juiz | “juiz tarefa_do_dia nasce” — `app/docs/PROMPTS.md:1933` |
| Veto | “MESMO veto de merito” — `app/docs/PROMPTS.md:1967` |
| Catraca | “conta so select_for_update sobre” quatro modelos — `app/docs/PROMPTS.md:1966` |

**Palavra não comprova classe.** “Dedup” aparece em medição de consumo de agentes, instrumento isento pelo contrato fornecido; usá-la como classe DADO recusaria esse instrumento (`app/docs/PROMPTS.md:1872`). “Catraca” e “juízes” aparecem em aval de ordem/teto da noite (`app/docs/PROMPTS.md:1839`). Título UI também não determina isenção: TOAST-E-NOME-CALENDARIO contém uma parte de veto de criação/salvamento e outra de apresentação (`app/docs/PROMPTS.md:1828`).

**Falso negativo lexical demonstrável:** o resumo do aval E1-FATO-E-O-TURNO-NAO-A-DATA-GRAVADA trata da recusa por `data_evento` sem nenhuma das seis palavras; classificador dessas palavras não o selecionaria (`app/docs/PROMPTS.md:1977`). O texto integral desse aval privado permanece **[não verificado]**.

A memória tem fundamento concreto: o varredor de atomicidade documenta grep contando 13 arquivos contra 9 por AST porque prosa citava a palavra, e proximidade textual de decorador não provava cobertura (`bin/atomicidade_da_porta.py:8-12`, `:41-44`). A memória O118 apontada pelo pai está em `app/docs/BACKLOG.md:1199`.

**Furo de chat/arquivo.** O selo atual considera somente IDs no idioma literal `BACKLOG **Oxx/Fxx**` já escritos em PROMPTS e verifica sua existência no BACKLOG; não estabelece que todo aval entrou no registro (`bin/tests/test_prompt_virou_item.sh:21-35`). A lacuna está explicitamente documentada no histórico (`app/docs/RELATO-ARQUIVO.md:30931`). Portanto, fiscalização restrita ao registro não observa aval que ficou apenas no chat; a ocorrência atual disso é **[não verificado]**.

**Allowlist/classificação.** O anexo I-2 isenta gestão/instrumento/UI, mas autoridade executável que decide essa classe, tratamento de aval misto e caminho único obrigatório de despacho permanecem **[não verificado]**. Não há evidência para chamar essas seis palavras de marcador formal existente **[não verificado]**.

## 08/11 10:49 · sessao fa5c2257202c · - · - · origem=Ronald (tmux)

I-3 — pontos comprovados na cópia:

- O verificador recebe **id da obra, diff e log do RED**; lê `CLAUDE.md`/`LEIS.md`, verifica nove itens e responde com primeira linha `APROVADO` ou `REPROVADO`. O arquivo não determina leitura de brief nem do RELATO do pouso. `.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:8`, `.claude/agents/verificador.md:18`.
- O B3 lê os **transcripts dos verificadores**: identifica obra/leitura final no primeiro prompt e extrai o veredito da primeira linha de `SubagentHandback`; filtra `agentType=verificador`. Não extrai etapas ou resultados dessas respostas. `bin/disjuntor.py:546`, `bin/disjuntor.py:551`, `bin/disjuntor.py:561`, `bin/disjuntor.py:736`.
- O B3 lê o **RELATO**, mas o uso atual é fotografar duas linhas úteis do topo e reconhecer `DESENHO`/`ERRO-BOBO` para liberar a obra. Não é leitura do marco do pouso para conferir execução integral. `bin/disjuntor.py:626`, `bin/disjuntor.py:662`, `bin/disjuntor.py:686`, `bin/disjuntor.py:930`.
- O disparo atual conta reprovações do opus final e voltas Codex triadas procedentes; limiares são 1 e 3, respectivamente. `bin/disjuntor.py:76`, `bin/disjuntor.py:77`, `bin/disjuntor.py:896`, `bin/disjuntor.py:910`.
- O opus final ocorre **antes do pouso**, depois da suite verde; a linha de medida é exigida no RELATO **do pouso**. Portanto o I-3 deixa sem endereço/instante definido o relato que o opus precisaria conferir antes de autorizar esse pouso. `CLAUDE.md:374`, `CLAUDE.md:379`; definição desse endereço/instante no anexo: [não verificado]. [nao verificado]
- Contador existente de “etapa K citada com resultado”, frase `pela metade, faltam N`, parser `RESULTADO OBSERVAVEL` ou selo `test_etapas_no_brief.sh`: [não verificado]. O anexo não fixa vínculo entre obra/versão do brief e marco do RELATO, nem critério para distinguir resultado medido de resultado esperado, citado ou antigo. [não verificado]
- O checador de prova já existente só procura `PROVA:` nas seis linhas próximas de certos atos em título/negrito; sua implementação não compara comando, número, etapas ou medição. `bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:36`, `bin/relato_afirma_com_prova.py:47`.
- Furo de rebrief: o ramo atual de abertura recebe `prompt` do `Agent` e faz B3/B4; não lê nem fotografa brief. Nenhum vínculo imutável abertura→brief→etapas fica estabelecido nesse ramo. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`. Existência de outra proteção específica: [não verificado].
- Furo de identificação: B3 só captura como final prompts que correspondam ao reconhecedor `_eh_final` e verificadores com o tipo esperado; a existência de outros agentes/verificadores efetivamente lançados fora disso não pode ser inferida. `bin/disjuntor.py:499`, `bin/disjuntor.py:555`, `bin/disjuntor.py:742`; uso real fora desses filtros: [não verificado].

MEDIDA — gerador e moldes existentes:

- `ESTADO.md` é produzido por `gerar(raiz,dst)` e gravado em `dst`; `relato.sh` chama o gerador para `$TMP/ESTADO.md` e publica esse arquivo como `ESTADO.md`. `bin/gerar_estado.py:178`, `bin/gerar_estado.py:390`, `bin/relato.sh:38`, `bin/relato.sh:105`.
- O gerador **consome** medidores: importa `disjuntor.ler/linha` dentro de `try`; não reimplementa a contagem. `bin/gerar_estado.py:277`.
- Molde mais direto: `disjuntor.ler` usa `corte=agora-86400`, lê trilhas com carimbo, conta disparos/negações/liberações e acumula `espera=Ns` de eventos `ATENDIDO`. Trilhas ilegíveis tornam contadores `None`; a ausência de arquivo é tratada separadamente. `bin/disjuntor.py:1546`, `bin/disjuntor.py:1554`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1580`, `bin/disjuntor.py:1584`, `bin/disjuntor.py:1590`.
- Outro molde: `codex_limite_estado` conta recusas em 24 h e chamadas por papel, contando apenas eventos `ini`; o escritor declarado é `codex_pergunta.sh`. `bin/codex_limite_estado.py:4`, `bin/codex_limite_estado.py:24`, `bin/codex_limite_estado.py:27`, `bin/codex_limite_estado.py:71`.
- Esses moldes demonstram janela e integração ao painel; dados atuais para “construtor sem censo”, “horas de vaga esperando censo”, “aval recusado por UNIDADE” e “pouso N/N etapas” não aparecem nesses contadores. `bin/disjuntor.py:1615`, `bin/disjuntor.py:1691`, `bin/codex_limite_estado.py:45`; existência das novas trilhas e suas medidas em produção: [não verificado].

## 08/11 10:49 · sessao 2c9c1db42771 · - · - · origem=Ronald (tmux)

**I-1 / colisão e leitores**

- A linha proposta `PAREI: censo-<obra> | espera Codex` **não libera hoje por nenhum dos dois caminhos**: `_parei_do_em_curso` exige `PAREI:` e `!`; `_esperando_com_prova` exige prefixo literal `ESPERANDO:` e ao menos `pid=` ou `arquivo=`. (`bin/hook_stop_fila1.py:1104`, `bin/hook_stop_fila1.py:1119`, `bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:673`.)
- O PAREI genérico ainda exige que a linha nomeie o item em curso e que não haja outros andáveis; acrescentar apenas `!` tampouco equivale ao contrato do censo. (`bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1142`, `bin/hook_stop_fila1.py:1155`.)
- Os sítios dependentes da espera são o reconhecedor, a memoização por Stop, o orçamento que fica pendente durante espera provada e a decisão principal do Stop; mudar só o ramo final deixa o orçamento tratar a espera de censo diferentemente. (`bin/hook_stop_fila1.py:645`, `bin/hook_stop_fila1.py:688`, `bin/hook_stop_fila1.py:984`, `bin/hook_stop_fila1.py:1231`.)
- Antes desses caminhos, a checagem de afirmações do RELATO e o B4 têm precedência; a exceção do censo não pode ser presumida como passagem por eles. (`bin/hook_stop_fila1.py:1194`, `bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1215`.)
- O handoff já **exibiria** a linha censo, pois coleta `PAREI` até a primeira seção, mas continuaria imprimindo “item em curso ANDA” / “não devolve turno”, por consultar exclusivamente os predicados de PAREI genérico. (`bin/handoff_sessao.sh:99`, `bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`.)
- O alarme já aceita qualquer substring `PAREI:` nas primeiras 40 linhas e desarma, sem consultar prova ou `_SO_BANG`: com censo inexistente/morto ele já pode ficar silencioso enquanto o Stop manda seguir. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`.)

**Selos que precisam ser confrontados, sem execução**

- `test_hook_parei_so_lei.sh` preserva o filtro `_SO_BANG`, rejeita decisão técnica e pergunta de lei como passagem e aceita `!`; liberar todo PAREI violaria esses casos. Não contém caso censo. (`bin/tests/test_hook_parei_so_lei.sh:25`, `bin/tests/test_hook_parei_so_lei.sh:41`, `bin/tests/test_hook_parei_so_lei.sh:52`, `bin/tests/test_hook_parei_so_lei.sh:60`.)
- `test_hook_esperando_com_prova.sh` distingue vivo/morto/outro produtor/sem prova e exige trilha `ALLOW ESPERANDO com prova` ou `NOPROVA`; censo precisaria entrar com essa mesma distinção, sem transformar o literal em prova. (`bin/tests/test_hook_esperando_com_prova.sh:67`, `bin/tests/test_hook_esperando_com_prova.sh:79`, `bin/tests/test_hook_esperando_com_prova.sh:93`.)
- `test_disjuntor.sh` protege PAREI CLEAR com handoff, validade temporal, DIFF e agente sem entrega; B4 COTA também exige handoff; protege ainda contra `pisos.log` como prova escrita pelo próprio hook. São interações a preservar, não regras hoje alteradas pelo literal censo. (`bin/tests/test_disjuntor.sh:104`, `bin/tests/test_disjuntor.sh:192`, `bin/tests/test_disjuntor.sh:200`, `bin/tests/test_disjuntor.sh:215`, `bin/tests/test_disjuntor.sh:477`, `bin/tests/test_disjuntor.sh:578`.)

**Prova de vida e furo**

- A prova PID atual lê `/proc/<pid>/stat`, executable e cmdline, rejeita ancestrais, zumbi, troca de processo e produtor divergente; ela **não lê `CODEX_OBRA`**. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:544`, `bin/hook_stop_fila1.py:548`.)
- A prova por arquivo exige arquivo regular e avanço de mtime desde avaliação anterior, sob trava; primeira observação não prova e arquivos do próprio painel/hook são excluídos. (`bin/hook_stop_fila1.py:563`, `bin/hook_stop_fila1.py:583`, `bin/hook_stop_fila1.py:604`, `bin/hook_stop_fila1.py:640`.)
- O wrapper exige `CODEX_OBRA` e `CODEX_PAPEL`, e o ramo construtor emite uma linha ESPERANDO com PID do wrapper e JSONL da chamada. Isso oferece identidade declarada da chamada, mas não há vínculo dela com o novo literal censo no Stop. (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:85`, `bin/codex_pergunta.sh:374`.)
- Uma prova específica precisaria vincular processo vivo/chamada ao **mesmo censo e obra**, por exemplo identidade `CODEX_OBRA` conferível no produtor ou sinal próprio com progresso. A existência desse mecanismo é **[não verificado]**; aceitar somente “espera Codex” abre espera sem produtor, e aceitar qualquer processo Codex permite que outra obra justifique a parada. A insuficiência do código atual se apoia em `bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:673`, `bin/hook_stop_fila1.py:535`.
- Processo Codex real ativo, acesso a `/proc` em produção e resultado dos selos nesta auditoria: **[não verificado]**.

## 08/11 10:41 · sessao 16d8ac42b6cf · CENSO-ANTES-DA-VAGA-HOOK · leitura · origem=Code

**O contrato ainda não está implementado nesta árvore:** `ESPERANDO: censo <OBRA> | Codex` retorna **“produtor nao declarado em 1 palavra”**; o parser atual exige produtor único e campos `pid=`/`arquivo=`. (`bin/hook_stop_fila1.py:658–674`)

**(i) Identidade em `/proc`**

Depois da passagem pelo shebang, já executando o corpo do wrapper:

| Chamada | `/proc/<pid>/exe` | `argv[0]` | `argv[1]` |
|---|---|---|---|
| `bin/codex_pergunta.sh ...` | Executável do Bash | `bash` | `bin/codex_pergunta.sh` |
| `bash bin/codex_pergunta.sh ...` | Executável do Bash | `bash` | `bin/codex_pergunta.sh` |

A primeira forma usa `#!/usr/bin/env bash`; a segunda explicita esse interpretador. O caminho absoluto concreto de `exe` nesta execução é **[não verificado]**. (`bin/codex_pergunta.sh:1`)

Consequência para o hook: sendo `exe` um interpretador, `_processo_casa` examina **`argv[1]`**, não exige que `exe` seja o `.sh`. Produtor declarado com caminho absoluto compara o caminho resolvido usando o `cwd` do processo; produtor sem barra compara apenas o basename. (`bin/hook_stop_fila1.py:511–520`)

**(ii) `CODEX_OBRA`**

**É entrada do wrapper, não um export posterior:** o script lê `CODEX_OBRA` para `OBRA` e recusa valor vazio. O selo fornece `CODEX_OBRA` pelo `env` antes de executar o wrapper; nessa chamada, portanto, a variável já entra no ambiente inicial do processo. (`bin/codex_pergunta.sh:80–85`; `bin/tests/test_codex_pergunta.sh:111–113`)

O hook atual não lê esse ambiente: `_prova_pid` consulta `stat`, `exe` e `cmdline`, sem conferir `CODEX_OBRA`. (`bin/hook_stop_fila1.py:534–550`)

**(iii) Furos e limites**

- **Outro usuário:** a exigência de mesmo UID ainda não existe em `_prova_pid`. Falha de leitura bloqueia aquela prova, mas não há comparação explícita de UID. (`bin/hook_stop_fila1.py:523–550`)
- **Wrapper esperando vaga/flock:** processo vivo não prova Codex iniciado. O wrapper espera no gate, nos slots e na trava da resposta antes da chamada ao Codex. Já a vaga exclusiva do construtor usa `flock -n`: ocupada, recusa imediatamente. (`bin/codex_pergunta.sh:341–369`; `bin/codex_pergunta.sh:401–402`; `bin/codex_pergunta.sh:232–233`)
- **Wrapper de outra sessão:** excluir ancestrais não vincula o produtor à sessão principal. `_ancestrais` monta somente a cadeia de pais do hook; `_prova_pid` não compara sessão nem exige descendência do chamador. (`bin/hook_stop_fila1.py:474–491`; `bin/hook_stop_fila1.py:529–550`)
- **Obra com prefixo comum:** a igualdade exata exigida pelo contrato elimina esse casamento por prefixo; sua implementação é **[não verificado]**. Hoje não há comparação de obra nessa prova. (`bin/hook_stop_fila1.py:534–550`)
- **Papel e fase:** o wrapper admite cinco papéis; conferir somente wrapper e obra não distingue `censo` dos demais. Além disso, ele continua vivo fazendo pós-processamento depois que o Codex retorna. (`bin/codex_pergunta.sh:81–85`; `bin/codex_pergunta.sh:404–448`)
- **“Terminou → volta a bloquear” tem limite:** perder a prova apenas continua o fluxo normal do Stop; fila vazia, teto, B2 e exceção ainda possuem caminhos de liberação. (`bin/hook_stop_fila1.py:1231–1246`; `bin/hook_stop_fila1.py:1284–1306`)

**(iv) Outros leitores do topo do RELATO**

Primeiro, **“linha 2” não é posição física para o hook**: ele pula título e brancos e usa a primeira linha útil. A segunda linha útil integra o bloco destinado ao B3. (`bin/hook_stop_fila1.py:400–408`; `bin/hook_stop_fila1.py:415–440`)

| Leitor | Comportamento |
|---|---|
| **Disjuntor B3** | Lê fotografia consistente do documento e usa o mesmo `_bloco_do_topo`, com duas linhas úteis. (`bin/disjuntor.py:662–689`) |
| **`handoff_sessao.sh`** | Percorre o topo até `##`, mas seleciona somente `PAREI`, `lei:` e `!:`; omite ESPERANDO e consulta apenas os vereditos PAREI. (`bin/handoff_sessao.sh:92–123`) |
| **`relato.sh`** | Submete o documento ao checador de afirmações e ao raspador/publicador; não julga prova de ESPERANDO nessas etapas. (`bin/relato.sh:54–61`; `bin/relato.sh:103–107`) |
| **Alarme de sessão ociosa** | Procura `PAREI:` nas primeiras 40 linhas; ESPERANDO não satisfaz esse reconhecimento. (`bin/alarme_sessao_ociosa.py:119–127`) |
| **Selo ESPERANDO** | Monta estado na linha física **3** e verifica ALLOW/NOPROVA; cobre PID vivo, morto, outro programa e ausência de prova. (`bin/tests/test_hook_esperando_com_prova.sh:62–105`) |
| **Selo de orçamento** | Exige que ESPERANDO provado suspenda o disparo do orçamento; morto o produtor, cobra novamente o orçamento. (`bin/tests/test_orcamento_contexto.sh:220–233`) |
| **Selo do disjuntor** | Exercita o recorte compartilhado e inclui mutante que conserva somente a primeira linha útil. (`bin/tests/test_disjuntor.sh:845–865`; `bin/tests/test_disjuntor.sh:1658`) |

## 08/11 10:40 · sessao 55f94ed382ed · - · - · origem=Ronald (tmux)

Adicionais:

- O selo espiona chamadas a `H._bloco_do_topo`, verificando leitura sob a trava: `bin/tests/test_disjuntor.sh:845-865`.
- O mutante que conserva só a primeira linha útil deve reprovar: `bin/tests/test_disjuntor.sh:1658`. Há mutantes de fotografia parcial: `bin/tests/test_disjuntor.sh:1659-1663`.
- `test_no_relato_tem_secao_pinada.py` conserva o topo inteiro por comparação `startswith(TOPO)`; não interpreta a linha 2: `app/core/tests/test_no_relato_tem_secao_pinada.py:31-35`, `app/core/tests/test_no_relato_tem_secao_pinada.py:51-60`. Sua leitura posicional verifica a última linha útil: `app/core/tests/test_no_relato_tem_secao_pinada.py:68-69`.

Nenhum leitor adicional da linha física 2 identificado na varredura, incluindo `.claude`; ausência absoluta: [não verificado].

## 08/11 10:39 · sessao 55f94ed382ed · - · - · origem=Ronald (tmux)

Leitores e efeitos confirmados:

- **O hook não lê “linha física 2”**: `_linha_do_topo()` usa a primeira linha útil; `_bloco_do_topo()` pula brancos e `# RELATO`, tira crases iniciais e retorna até duas linhas úteis. A segunda útil existe para B3. (`bin/hook_stop_fila1.py:400-408`, `bin/hook_stop_fila1.py:415-440`)

- **Disjuntor B3** lê o RELATO inteiro duas vezes, confere identidade/mtime/tamanho e bytes; recorta as duas linhas úteis pelo mesmo `H._bloco_do_topo()`. Portanto continua sendo consumidor do painel, sem uma regra paralela de recorte. (`bin/disjuntor.py:662-689`)

- **Handoff** lê o topo até a primeira seção `## `, mas seleciona somente `PAREI`, `lei:` e `!:`. `ESPERANDO: censo ... | Codex` fica fora da lista; os vereditos mostrados chamam `_parei_do_em_curso()` e `_relato_parou()`, não a prova ESPERANDO. (`bin/handoff_sessao.sh:92-123`)

- **`relato.sh`** entrega o documento ao checador de afirmações de ato e depois ao raspador/publicador; nesse fluxo não interpreta o estado ESPERANDO. O checador percorre linhas procurando títulos/negritos com verbos de ato e prova próxima. (`bin/relato.sh:54-61`, `bin/relato.sh:103-107`, `bin/relato_afirma_com_prova.py:30-50`)

- **Alarme de sessão ociosa** lê as primeiras 40 linhas e só procura a substring `PAREI:`. A linha ESPERANDO não desarma esse reconhecimento. (`bin/alarme_sessao_ociosa.py:119-127`)

Selos relevantes:

- `test_hook_esperando_com_prova.sh` constrói RELATO com cabeçalho na linha 1, branco na 2 e estado na 3; exige liberação pelo motivo `ALLOW ESPERANDO com prova`, ou bloqueio com `siga: ITEM-VIVO` e motivo `NOPROVA`. (`bin/tests/test_hook_esperando_com_prova.sh:62-89`)
- O selo atual cobre produtor vivo, PID morto, outro programa e ausência de prova, usando a sintaxe `produtor | pid=`. (`bin/tests/test_hook_esperando_com_prova.sh:93-105`)
- Também exige que prova de ato seja conferida antes de ESPERANDO e que matar o produtor revogue a liberação. (`bin/tests/test_hook_esperando_com_prova.sh:186-192`)
- `test_orcamento_contexto.sh` exige que ESPERANDO com produtor vivo suspenda o disparo de orçamento e libere pelo motivo correto. (`bin/tests/test_orcamento_contexto.sh:220-230`)
- `test_disjuntor.sh` testa diretamente os juízes compartilhados de leitura do topo, inclusive leitura parcial após a primeira linha útil. (`bin/tests/test_disjuntor.sh:748-765`)
- O selo do handoff exercita perguntas de lei depois de 14 linhas RESPONDIDA, protegendo o alcance até a seção, sem testar censo. (`bin/tests/test_handoff_sessao.sh:139-148`)

## 08/11 10:35 · sessao 950169722c2a · FIO-E0-V4-E3 · censo · origem=Code

**O aval não confere como lista completa.** Dois pontos objetivos: não encontrei chamadas de produção de `o_que_segura`; e `explicador` e `folha/previa` são respondedores, embora estejam entre os nove nomes tratados como simples consumidores de C15. (`app/folha/fora_da_folha.py:203`, `app/chamados/services/explicador.py:163`, `app/folha/previa.py:95`)

| Base da conferência | Limite |
|---|---|
| AST dos arquivos Python da cópia, fora de testes, com conferência dos corpos, imports e aliases; chamadas concretas discriminadas abaixo. | Uso dinâmico externo à cópia: **[nao verificado]**. | [nao verificado]
| Aval e e2.r comparados pelo conteúdo colado no pedido. | Arquivos locais dos anexos e correspondência desta cópia com HEAD `94d41ea`: **[nao verificado]**. |
| `JUIZES.md` é lookup; o próprio documento manda conferir o vivo e aponta `core/juizes.py` como registro. | `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:14`. |
| Resultados abaixo descrevem código existente. | Quantidades atuais, ocorrências individuais em produção e comportamento visual do APK: **[nao verificado]**. |

**1. Conferência do aval**

| Item do aval | Resultado contra o vivo | Chamadores, unidade e fonte |
|---|---|---|
| `vigia_fio.fio_do_dia:156` | **Confere.** | Chamado por `censo` em `app/chamados/services/vigia_fio.py:414` e `app/chamados/services/vigia_fio.py:417`. Devolve booleano sobre a coleção recebida; **lê C15**, sem derivar vitalidade própria. `app/chamados/services/vigia_fio.py:166`. |
| `vigia_fio.censo_alcance` | **Confere.** Definição em `app/chamados/services/vigia_fio.py:542`. | Chamado pelo command em `app/chamados/management/commands/vigiar_fio_morto.py:173`. Responde por **(colab,dia)** retido, com rota, papel e causa; lê retenção, C17 e dono fora do fio, depois deriva contadores/causas. `app/chamados/services/vigia_fio.py:585`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:699`. |
| C17 `porta_alcancavel_do_dia:1341` | **Confere.** | Três chamadores diretos: `_porta`, `censo_alcance`, `beco_em_lote`. Devolve `None` ou `{rota,fila,verbos,fio}` por **célula/dia**. Compõe fila, perguntas C6 vivas presentes no app e verbos do painel. `app/folha/fora_da_folha.py:67`, `app/chamados/services/vigia_fio.py:635`, `app/escala/services/cadastro_realidade.py:488`, `app/chamados/juizes.py:1386`. |
| `folha._fio_do_dia:70` | **Confere.** | Três chamadas: `fora_da_folha`, `o_que_segura`, `dias_sem_fio`. Devolve `admin/colab/chamado/nada` por célula; **lê `fio` de C17**. `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:178`, `app/folha/fora_da_folha.py:221`, `app/ponto/services/pautas_dos_discordantes.py:77`. |
| `o_que_segura:203`, “6 chamadores” | **Não confere quanto aos seis.** O AST não encontrou chamada de produção da função nesta cópia. | O corpo existe e responde colaborador/competência com dias retidos, fio, destrava e chamados; compõe export, retenção e C17. Definição e retorno: `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:229`. Chamador dinâmico externo: **[nao verificado]**. |
| `resolver_dia_oferecido:97` e `STATUS_QUE_PEDEM_DECISAO:94` | **Confere.** | Única chamada direta: `contexto_calendario`, em `app/colaboradores/services/calendario.py:769`. Booleano por dia; **deriva** último recurso + passado + status em `falta/irregular/aberto/sem_certidao`. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`. |
| `perguntas_do_app:42 ← api/views:2552` | **Confere, mas faltam quatro chamadores.** | Também leem: `_no_app`, ficha, `medir`, `do_dia`. Unidade **pergunta**; lê `q_vivas` e C6 exato. `app/chamados/juizes.py:1319`, `app/colaboradores/services/ficha.py:166`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:162`, `app/chamados/services/pergunta_no_app.py:53`. |
| `pendentes_do_colaborador:1729 ← dois kernels` | **Confere, mas falta dashboard web.** | Três chamadas: `app/api/views.py:325`, `app/api/views_core.py:314`, `app/core/views.py:253`. Retorna **disputas abertas distintas** com pergunta C6 viva sem resposta; deriva o recorte da disputa e lê C6. `app/chamados/models.py:1736`, `app/chamados/models.py:1740`. |
| C11 `cadastro_realidade.beco_em_lote:442` | **Confere.** A chamada C17 está em **488**, não em 460. | Chamado em `app/escala/services/cadastro_realidade.py:439` e `app/escala/services/cadastro_realidade.py:573`. Devolve assinatura C11 por **(colab,dia)**; compõe célula indefinida, perguntas vivas, ausência de porta, âncora terminal, cadastro e dono. `app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:515`, `app/escala/services/cadastro_realidade.py:530`. A linha 460 é docstring. |

**Censo complementar: respondedores que faltaram na lista**

Na coluna “natureza”, **lê** identifica delegação ao juiz; **compõe/deriva** identifica seleção, agregação ou resposta construída no próprio sítio. As fontes e chamadas estão discriminadas em cada linha.

| Sítio/função | Quem chama | Unidade e resposta | Natureza |
|---|---|---|---|
| `vigia_fio.censo` — `app/chamados/services/vigia_fio.py:318` | `vigiar_fio_morto:172`; `materializar_passivo_validado:203`, em `app/chamados/management/commands/` | Células/dias com fio vivo, morto ou dono fora do fio. | Compõe universo de chamados/células, lê C15 e dono; agrega. `app/chamados/services/vigia_fio.py:334`, `app/chamados/services/vigia_fio.py:417`, `app/chamados/services/vigia_fio.py:431`. |
| `dono_fora_do_fio` — `app/chamados/services/vigia_fio.py:274` | `censo:431`, `censo_alcance:654`, mesmo arquivo | **(colab,dia)** → decisão pendente, ausência aberta, escala não descreve dia, fio do admin ou `None`; opcional papel. | Deriva precedência entre oferta do calendário, ausência, estado da pergunta e lista administrativa. `app/chamados/services/vigia_fio.py:306`. |
| `ListaDoAdmin.dia_do_admin/dia_na_lista/papel_do_dia` — `app/chamados/services/vigia_fio.py:222`, `app/chamados/services/vigia_fio.py:230`, `app/chamados/services/vigia_fio.py:238` | Dono em `app/chamados/services/vigia_fio.py:312`; alcance em `app/chamados/services/vigia_fio.py:661` | Presença e papel por **(colab,dia)**. | Lê lista Cadastro×Realidade e reduz seus donos. `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:246`. |
| `resolver_dia_oferecido_nas_datas` — `app/colaboradores/services/calendario.py:117` | Vigia em `app/chamados/services/vigia_fio.py:271` | `{dia:bool}` para um colaborador. | Lê campo do calendário; dia ausente recebe falso. `app/colaboradores/services/calendario.py:132`, `app/colaboradores/services/calendario.py:135`. |
| `contexto_calendario` — `app/colaboradores/services/calendario.py:159` | `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; plural em `app/colaboradores/services/calendario.py:132` | Dias com estado, pendência e oferta de decisão. | Compõe status/último recurso, lê C6 e escreve `resolver_dia_oferecido`. `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:769`. |
| `cadastro_realidade.lista/dias_na_lista/um_dono_por_dia` — `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:1044`, `app/escala/services/cadastro_realidade.py:985` | Tela `app/escala/views.py:487`; vigia `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:217`; lista chama uniformização em `app/escala/services/cadastro_realidade.py:885` | Lista administrativa e dono por **(colab,dia)**. | Compõe assinaturas, incorpora C11 e lê `InsumosDoDono.dono`. `app/escala/services/cadastro_realidade.py:864`, `app/escala/services/cadastro_realidade.py:1009`, `app/escala/services/cadastro_realidade.py:1026`, `app/escala/services/cadastro_realidade.py:1064`. |
| `montar_chamados_fio` — `app/chamados/services/fio.py:160` | Colab/admin/modal em `app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`; parcial em `app/chamados/services/fio.py:442` | **Chamados**, com perguntas e indicador de ação pendente. | Lê janela, C6/`.viva` e prova de ação possível. **`tem_pergunta_viva` não é apenas C15:** exige `.viva` e ausência de clique morto. `app/chamados/services/fio.py:171`, `app/chamados/services/fio.py:234`, `app/chamados/services/fio.py:279`. |
| `chamados_visiveis_colab` — `app/chamados/services/janela_colab.py:49` | API `app/api/views.py:1016`; fio `app/chamados/services/fio.py:171`; arquivamento `app/chamados/views_arquivar.py:195` | QuerySet de **chamados** da lista do colaborador. | Deriva janela/destinos/silêncio/módulos; lê **pré-filtro SQL `q_vivas`**, sem C6 exato nesse corpo. `app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:67`. |
| `explicar_pergunta/por_que_nao_fecha` — `app/chamados/services/explicador.py:68`, `app/chamados/services/explicador.py:108` | Explicador em `app/chamados/services/explicador.py:133`; montador em `app/chamados/services/fio.py:308`; modal mostra em `app/templates/chamados/partials/modal_fio.html:432` | **Pergunta:** estado/dono/ação; **disputa:** `pode_fechar/resumo/itens`. | Lê `.estado`, materialização de fato e C15; deriva descrição e ações. `app/chamados/services/explicador.py:72`, `app/chamados/services/explicador.py:93`, `app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:163`. |
| `desenlace` — `app/chamados/services/desenlace.py:44` | Fio em `app/chamados/services/fio.py:399`; modal em `app/templates/chamados/partials/modal_fio.html:118` | **Chamado** → verbo, rótulo, estado, ação ou `None`. | Compõe natureza, explicação, filhos e porta própria. `app/chamados/services/desenlace.py:50`, `app/chamados/services/desenlace.py:57`, `app/chamados/services/desenlace.py:62`. |
| `montar_contexto_detalhe` — `app/chamados/services/acoes_chamado.py:327` | `app/chamados/views.py:551` | Detalhe do **chamado** e perguntas vivas. | Lê C6 para filtrar perguntas da disputa aberta. `app/chamados/services/acoes_chamado.py:335`, `app/chamados/services/acoes_chamado.py:348`. |
| `contexto_drawer` — `app/colaboradores/services/drawer.py:17` | `app/colaboradores/views.py:2943` | Banner de motivos pendentes; fallback “N chamados de furo aguardando colaborador”. | Primeiro lê perguntas vivas; fallback **deriva por `VIVOS` e quatro módulos**, sem conferir C6. `app/colaboradores/services/drawer.py:78`, `app/colaboradores/services/drawer.py:88`, `app/colaboradores/services/drawer.py:92`. |
| `relatorios_disputa.classificar/coletar` — `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:26` | Coleta em `app/chamados/relatorios_disputa.py:47`; tela `app/colaboradores/views.py:2997`; command `app/chamados/management/commands/relatorio_pendencias_disputa.py:15` | **Disputa** → sem pergunta, pronta fechar, aguarda colab ou supervisão. | Lê C6; deriva categoria pela existência de resposta. `app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`. |
| `perguntas_pendentes_do_colab/aviso_texto_livre` — `app/chamados/services/aviso_pendencia.py:7`, `app/chamados/services/aviso_pendencia.py:16` | Cobrança em `app/chamados/services/cobranca.py:48`; API `app/api/views.py:997`, `app/api/views.py:2220`; web `app/chamados/views.py:1120` | Lista de **perguntas** e aviso com datas que continuam aguardando resposta. | Lê `q_vivas` + C6 + ausência de `respondida_em`; deriva texto. `app/chamados/services/aviso_pendencia.py:13`, `app/chamados/services/aviso_pendencia.py:20`. |
| `checar_cobranca` — `app/chamados/services/cobranca.py:75` | `cobrar_colaborador`, em `app/chamados/services/cobranca.py:155`; referência de intake em `app/core/intake.py:68` | Quantidade de **perguntas**, datas, disputas, prazo e canal. | Lê aviso de pendência; deriva agregados/prazo. `app/chamados/services/cobranca.py:82`, `app/chamados/services/cobranca.py:87`. |
| `_calados/linhas_para_a_pauta` — `app/chamados/services/cobranca.py:221`, `app/chamados/services/cobranca.py:251` | Pauta em `app/escala/services/fase_12x36.py:272`; redução em `app/chamados/services/cobranca.py:257` | **Pessoas** cobradas/caladas e sem canal. | Deriva com `q_vivas`, disputa aberta e pergunta sem resposta; **não chama C6 exato** nesse corpo. `app/chamados/services/cobranca.py:229`, `app/chamados/services/cobranca.py:248`. |
| `pergunta_no_app.retencao/do_dia/medir` — `app/chamados/services/pergunta_no_app.py:65`, `app/chamados/services/pergunta_no_app.py:152`, `app/chamados/services/pergunta_no_app.py:119` | `dia_do_colab:109`; commands `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21` | Por **chamado/dia**: pergunta no app, causa e dono da retenção. | Lê lista do app, fábrica, ata, cobertura e lista admin; deriva taxonomia. `app/chamados/services/pergunta_no_app.py:77`, `app/chamados/services/pergunta_no_app.py:88`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:165`. |
| `dia_do_colab/_veredito_do_dia` — `app/ponto/services/dia_do_colab.py:6`, `app/ponto/services/dia_do_colab.py:112` | API `app/api/views_mensageria.py:1970`; ficha `app/colaboradores/services/ficha.py:280`; frase em `app/ponto/services/dia_do_colab.py:102` | **(colab,dia)**: marcos, perguntas e frases “Faltou…”/“esperando resposta ou decisão”. | Lê ata, C6 e pergunta no app; **deriva** frase pelos marcos apagados e perguntas vivas. `app/ponto/services/dia_do_colab.py:30`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:144`, `app/ponto/services/dia_do_colab.py:150`. |
| `ficha_do_colab` — `app/colaboradores/services/ficha.py:99` | Ficha lazy em `app/colaboradores/services/ficha.py:269`; copiloto em `app/colaboradores/services/ficha.py:275`, servido por `app/api/views_mensageria.py:1131` | **Chamados/perguntas** por pessoa, com dono e ações quantificadas. | Lê verbos, perguntas do app e respostas sem veredito; deriva seleção por texto e ações. `app/colaboradores/services/ficha.py:155`, `app/colaboradores/services/ficha.py:166`, `app/colaboradores/services/ficha.py:171`, `app/colaboradores/services/ficha.py:190`. |
| `fila_validar_classificada` — `app/chamados/services/validacao.py:865` | `app/chamados/views_cobrar.py:288`, `app/chamados/views_cobrar.py:349`, `app/chamados/views_cobrar.py:361` | **Perguntas** e **chamados distintos** por classe, idade e total. | Lê fila C6 de respondidas a validar; deriva classes/contagens. `app/chamados/services/validacao.py:874`, `app/chamados/services/validacao.py:882`, `app/chamados/services/validacao.py:884`. |
| `becos.classificar` — `app/chamados/services/becos.py:25` | Command `app/chamados/management/commands/fechar_becos_disputa.py:39`; evento em `app/chamados/services/becos.py:69` | **Disputa** → `None/fechavel/barrada`. | Deriva recorte de beco por abertura, resposta/destino e pai terminal; lê C7 para fechar. `app/chamados/services/becos.py:27`, `app/chamados/services/becos.py:33`. |
| `api_chamados_v2` — `app/api/views.py:975` | Rota `app/api/urls.py:73` | **Chamados** com perguntas da disputa, marcos e flag de regularização. | Monta perguntas a partir de **disputas abertas**, sem filtrar essa coleção por C6; lê projeção por marco separadamente. `app/api/views.py:1029`, `app/api/views.py:1043`, `app/api/views.py:1103`, `app/api/views.py:1158`. |
| `projecao_por_marco/dia_em_regularizacao/marco_aceito_unico` — `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:444`, `app/chamados/services/declaracao_texto.py:431` | Fio `app/chamados/services/fio.py:371`; API `app/api/views.py:1103`; wrapper em `app/chamados/services/declaracao_texto.py:501` | **Marco** → aceita/recusa; **dia** → todos recusados; marco único oferecido. | Lê faltantes da célula e identidade/guarda da porta; deriva projeção e reduções. `app/chamados/services/declaracao_texto.py:414`, `app/chamados/services/declaracao_texto.py:423`, `app/chamados/services/declaracao_texto.py:427`, `app/chamados/services/declaracao_texto.py:454`. |
| `montar_contexto_painel` — `app/chamados/services/painel.py:196` | `app/chamados/views.py:727`, `app/chamados/views.py:852`; lavrador `app/core/management/commands/lavrar_badge_navbar.py:39` | Lista/pílulas/contadores de **chamados**. | Lê `qs_na_fila` e partição de verbos; agrega por estado/verbo. `app/chamados/services/painel.py:210`, `app/chamados/services/painel.py:228`. |
| `qs_na_fila/particionar_verbos/creditos_perguntas` — `app/chamados/services/painel.py:158`, `app/chamados/services/verbos.py:30`, `app/chamados/services/raiox.py:49` | Painel acima; alimentação C17 em `app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1290`; consumidores abaixo | **Chamado** → pertença à fila/verbo; crédito de pergunta ao chamado. | Fila deriva `COBRANCA` e silêncio; verbo lê crédito/natureza; crédito usa `q_vivas` e `veredito_lavrado`, **não C6 exato nesse corpo**. `app/chamados/services/painel.py:187`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`, `app/chamados/services/raiox.py:133`. |
| `coletar_lente` — `app/chamados/services/verbos.py:164` | `app/chamados/views.py:1480` | **Chamados** agrupados da pílula ativa. | Lê partição de verbos e agrega. `app/chamados/services/verbos.py:171`, `app/chamados/services/verbos.py:185`. |
| Navbar `_badge_staff/_badge_registrados/_badge_colab` — `app/core/context_processors.py:59`, `app/core/context_processors.py:131`, `app/core/context_processors.py:185` | `navbar_context`, que publica as lazies em `app/core/context_processors.py:154`, `app/core/context_processors.py:189` | Staff: **chamados que exigem admin**; segundo número: **fila**; colab: **chamados na fila dele**. | Staff lê lavrado, fallback partição e fallback fila; colab lê fila. `app/core/context_processors.py:84`, `app/core/context_processors.py:90`, `app/core/context_processors.py:93`, `app/core/context_processors.py:150`, `app/core/context_processors.py:187`. |
| Dashboard web — `app/core/views.py:253` | Render em `app/core/views.py:271`; badge em `app/templates/core/dashboard_colaborador_v2.html:79` | **Disputas** pendentes do colaborador. | Lê `pendentes_do_colaborador`. `app/core/views.py:253`. |
| `montar_situacional/_led` e `montar_linha_colab` — `app/colaboradores/services/situacional.py:65`, `app/colaboradores/services/situacional.py:56`, `app/colaboradores/painel_cell.py:11` | Telas em `app/colaboradores/views.py:2664`, `app/colaboradores/views.py:2801`; live-cell em `app/colaboradores/painel_cell.py:81` | **Colaborador** → LED vermelho se existe disputa aberta. | **Deriva existência de disputa aberta**, sem exigir pergunta C6 viva. Lote: `app/colaboradores/services/situacional.py:117`; pontual: `app/colaboradores/painel_cell.py:31`; cor: `app/colaboradores/services/situacional.py:58`, `app/colaboradores/painel_cell.py:68`. |
| `montar_painel_operacional` — `app/core/services/painel_op.py:38` | `app/core/views.py:607`, `app/core/views.py:632` | Número de **chamados por colaborador**. | Lê `qs_na_fila` e agrega. `app/core/services/painel_op.py:119`. |
| `api_mensageria_pendencias/sinais` — `app/api/views_mensageria.py:390`, `app/api/views_mensageria.py:427` | Rotas `app/api/urls.py:10`, `app/api/urls.py:11` | **Chamados** com verbo; totais por verbo. | Leem partição e derivam serialização/agregados. `app/api/views_mensageria.py:395`, `app/api/views_mensageria.py:421`, `app/api/views_mensageria.py:453`, `app/api/views_mensageria.py:460`. |
| `fila.por_verbo/idade_e_vazao/gestao` — `app/chamados/services/fila.py:53`, `app/chamados/services/fila.py:88`, `app/chamados/services/fila.py:145` | Redução em `app/chamados/services/fila.py:141`; APIs em `app/api/views_mensageria.py:668`, `app/api

[truncado em 20000 de 48406 caracteres]

## 08/11 10:28 · sessao 02956b4fd4e0 · - · - · origem=Ronald (tmux)

| Sítio confirmado fora de `tests/` | Chamadores de produção confirmados | Unidade e resposta | Deriva ou lê |
|---|---|---|---|
| `app/chamados/services/vigia_fio.py:156` `fio_do_dia` | `censo`, `app/chamados/services/vigia_fio.py:414`, `:417` | Booleano sobre perguntas recebidas: dia tem fio vivo. | Só lê C15 `fio_vivo`, `app/chamados/services/vigia_fio.py:165`. |
| `app/chamados/services/vigia_fio.py:274` `dono_fora_do_fio` | `censo :431`; `censo_alcance :654`, mesmo arquivo | Por `(colab,data)`: decisão pendente, ausência aberta, escala não descreve dia, fio do admin ou `None`; opcional papel. | Compõe, nesta ordem, `oferecido`, cobertura de ausência/ABERTAS, `.estado` + aceite de folga, presença/dono na lista; `app/chamados/services/vigia_fio.py:306`. |
| `app/chamados/services/vigia_fio.py:318` `censo` | `app/chamados/management/commands/vigiar_fio_morto.py:172`; `app/chamados/management/commands/materializar_passivo_validado.py:203` | Contadores e listas de células/dias; universo começa em chamados vivos ligados à célula e módulos de furo, `app/chamados/services/vigia_fio.py:334`; exclui hoje/futuro do alarmado, `:420`. | Lê C15 via `fio_do_dia`, `:417`; lê `dono_fora_do_fio`, `:431`; deriva contadores. |
| `app/chamados/services/vigia_fio.py:542` `censo_alcance` | `app/chamados/management/commands/vigiar_fio_morto.py:173` | Por `(colab,data)` retido: rota, causa, detalhe e papel; `app/chamados/services/vigia_fio.py:625`, `:699`. | Lê `dias_que_seguram :585`, C17 `:635`, dono `:654`; deriva causas sem porta `:667`. |
| `app/chamados/services/vigia_fio.py:222` `ListaDoAdmin.dia_do_admin`; `:230` `dia_na_lista`; `:238` `papel_do_dia` | `dono_fora_do_fio :312`, `:313`; `censo_alcance :661` | Booleano presença com dono BATIDA/CADASTRO; presença+dono; papel do dia. | Lê `cadastro_realidade.lista` e `dias_na_lista`, `app/chamados/services/vigia_fio.py:216`; filtra/traduz `:228`, `:246`. |
| `app/colaboradores/services/calendario.py:97` `resolver_dia_oferecido` | `contexto_calendario`, mesmo arquivo `:769` | Booleano por dia: último recurso, passado, status em `STATUS_QUE_PEDEM_DECISAO`, `:94`, `:103`. | Deriva da projeção do calendário; não considera permissão do observador, `:101`. |
| `app/colaboradores/services/calendario.py:117` `resolver_dia_oferecido_nas_datas` | `app/chamados/services/vigia_fio.py:271` | `{date:bool}`; dia ausente recebe falso, `app/colaboradores/services/calendario.py:135`. | Lê os campos produzidos pelo calendário; `:132`. |
| `app/colaboradores/services/calendario.py:159` `contexto_calendario` | `app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; plural `app/colaboradores/services/calendario.py:132` | Lista de dias, incluindo `resolver_dia_oferecido`, `:769`. | Lê C6 para perguntas respondidas vivas, `:476`; compõe status e último recurso e escreve projeção do juiz `:769`. |
| `app/escala/services/cadastro_realidade.py:442` `beco_em_lote` | `dias_folga_nao_definida_sem_porta :439`; `_folga_nao_definida_sem_porta :573`, mesmo arquivo | Lista `(colab_id,assinatura C11)`, com data, dono, perguntas, pai limbo e detalhe; `:530`. | Compõe lâmpada indefinida/protesto `:477`, vivas/C17 `:487`, âncora terminal `:515`, cadastro `:518`, dono `:531`. |
| `app/escala/services/cadastro_realidade.py:787` `lista`; `:1044` `dias_na_lista`; `:985` `um_dono_por_dia` | Tela `app/escala/views.py:487`; vigia `app/chamados/services/vigia_fio.py:216`, `:217`; `lista → um_dono_por_dia :885` | Lista administrativa; mapa `(colab,data) → dono`; uniformização do dono do dia. | Lista incorpora C11 `:864`; uniformização reaproveita C11 `:1009` ou lê `InsumosDoDono.dono :1026`; mapa reduz donos `:1064`. |
| `app/chamados/services/fio.py:160` `montar_chamados_fio` | `app/chamados/views.py:569` colab, `:677` admin, `:889` modal; `contexto_parcial_fio`, `app/chamados/services/fio.py:442` | Lista de chamados, com perguntas e `tem_pergunta_viva`, `:249`, `:279`. | Colab: janela e disputas abertas `:169`, `:213`; admin inclui vivas em fechadas via C6 `:234`; flag combina `.viva` e `raiox.pergunta_sem_acao_possivel`, `:279`. |
| `app/chamados/services/janela_colab.py:49` `chamados_visiveis_colab` | `app/api/views.py:1016`; `app/chamados/services/fio.py:171`; `app/chamados/views_arquivar.py:195` | Queryset de chamados da lista de trabalho. | Deriva recorte janela OU destinos de `q_vivas`, silêncio nulo e módulos não internos; `app/chamados/services/janela_colab.py:61`, `:67`, `:70`. Usa pré-filtro SQL, não C6 exato nesse corpo, `:63`. |
| `app/chamados/services/explicador.py:68` `explicar_pergunta`; `:108` `por_que_nao_fecha` | `por_que_nao_fecha → explicar_pergunta :133`; montador `app/chamados/services/fio.py:308`; exibição `app/templates/chamados/partials/modal_fio.html:432` | Por pergunta: estado, dono e ação, `explicador.py:74`; por disputa: `pode_fechar`, resumo, itens, `:163`. | Lê `.estado :72`, materialização real `:93`; C15 decide `pode`, `:137`; deriva descrição e ações. **É respondedor administrativo**, comprovado pelo template `modal_fio.html:435`–`:437`. |
| `app/chamados/services/desenlace.py:44` `desenlace` | `app/chamados/services/fio.py:399`; exibição `app/templates/chamados/partials/modal_fio.html:118` | Por chamado: `{verbo,rotulo,estado,acao}` ou `None`, `desenlace.py:47`. | Compõe `catalogo.natureza.dono_do_chamado :50`, explicação `:62`, filhos `:57`, módulos/porta própria `:78`. |
| `app/chamados/services/acoes_chamado.py:327` `montar_contexto_detalhe` | `app/chamados/views.py:551` | Detalhe de chamado, perguntas vivas da disputa aberta e evidência, `acoes_chamado.py:335`, `:348`. | Lê C6 `:347` e filtra; não usa C17 nesse percurso. |
| `app/colaboradores/services/drawer.py:15` `contexto_drawer` — confirmar linha de definição antes de usar; trechos verificados `:69`, `:78`, `:88` | `app/colaboradores/views.py:2943` | Banner `falha_texto`: motivos das vivas na última disputa aberta; fallback número de chamados de furo “aguardando colaborador”, `drawer.py:82`, `:92`. | Primeiro lê `perguntas_vivas_de :78`; fallback **deriva independentemente** de `VIVOS` e quatro módulos, sem C6, `:88`–`:92`. |
| `app/chamados/relatorios_disputa.py:11` `classificar`; `:26` `coletar` | `coletar → classificar :47`; tela `app/colaboradores/views.py:2997`; command `app/chamados/management/commands/relatorio_pendencias_disputa.py:15` | Por disputa: SEM_PERGUNTA/PRONTA_FECHAR/AGUARDA_COLAB/AGUARDA_SUPERVISAO, `relatorios_disputa.py:13`; buckets de disputas, `:47`. | Lê C6 `:18`; deriva categoria pela existência de resposta `:21`. |
| `app/chamados/services/aviso_pendencia.py:7` `perguntas_pendentes_do_colab`; `:16` `aviso_texto_livre` | `_pendentes`, `app/chamados/services/cobranca.py:48`; aviso `app/api/views.py:997`, `:2220`; `app/chamados/views.py:1120` | Lista de perguntas vivas sem resposta; aviso por conjunto de datas, `aviso_pendencia.py:13`, `:20`. | Lê `q_vivas` e C6, `:11`, `:13`; deriva texto. |
| `app/chamados/services/cobranca.py:75` `checar_cobranca` | `cobrar`, mesmo arquivo `:155`; também referência dinâmica de intake `app/core/intake.py:68` | Pendentes em perguntas, datas distintas e disputas distintas, `cobranca.py:87`–`:94`. | Lê aviso via `_pendentes :82`; deriva prazo/canal/contagens. |
| `app/chamados/services/cobranca.py:221` `_calados`; `:251` `linhas_para_a_pauta` | `linhas_para_a_pauta → _calados :257`; pauta `app/escala/services/fase_12x36.py:272` | Pessoas cobradas com prazo vencido e pessoas sem canal, `cobranca.py:248`; frases de pendência `:261`. | **Deriva com `q_vivas` SQL**, sem C6 exato nesse corpo, exigindo disputa aberta e pergunta sem resposta `:229`; lê juiz prazo `:246`. |
| `app/ponto/services/dia_do_colab.py:6` `dia_do_colab`; `:112` `_veredito_do_dia` | API `app/api/views_mensageria.py:1970`; ficha `app/colaboradores/services/ficha.py:280`; corpo chama frase `dia_do_colab.py:102` | Um `(colab,data)`: marcos/batidas/perguntas e frase “Faltou…” `:144`, vivas esperando resposta/decisão `:150`. | Lê ata `:30`, C6 `:71`, pergunta_no_app `:109`; frase deriva contagens de marcos/perguntas `:139`–`:154`. |
| `app/chamados/services/pergunta_no_app.py:65` `retencao`; `:152` `do_dia`; `:119` `medir` | `medir → retencao :142`; `do_dia → retencao :164`; `app/ponto/services/dia_do_colab.py:109`; commands `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14`, `perguntar_chamados_sem_pergunta.py:21`, `:30` | Causa/quem reteve; por dia chamados vivos, perguntas no app, dono da retenção `pergunta_no_app.py:165`; censo por chamado `:145`. | Lê perguntas_do_app `:139`, `:162`, ata `:80`, cobertura `:90`, lista admin `:88`, C6 em `_marco_ja_perguntado :111`; deriva taxonomia de retenção. |
| `app/chamados/services/validacao.py:865` `fila_validar_classificada` | `app/chamados/views_cobrar.py:288`, `:349`, `:361` | Perguntas e chamados distintos por classe, idade, total e rótulo `validacao.py:884`–`:887`. | Lê `perguntas_respondidas_a_validar :874`; deriva classes e contadores. |
| `app/chamados/services/becos.py:25` `classificar` | command `app/chamados/management/commands/fechar_becos_disputa.py:39`; evento `becos.py:69` | Por disputa: `None`, fechável ou barrada, `becos.py:26`. | Deriva aberta, destino e pai terminal `:27`–`:32`; lê C7 `pode_fechar :33`. |

Juízes e leitores que o e2.r incluía, mas o aval não nomeou:

| Função | Chamadores | Resposta e natureza |
|---|---|---|
| `app/chamados/juizes.py:1886` `cobranca_viva_do_dia` | `app/chamados/models.py:373`, `:711`; `app/ponto/services/cartorio.py:1604`; `entrada_sincronizada.py:28`; `triagem_batida.py:589` | Chamado mais novo ou `None`; deriva `VIVOS`, módulo, dia forte e desempate, `juizes.py:1897`–`:1902`. Estes chamadores usam o resultado para planejar/fechar/reconciliar, não são cinco telas novas. |
| `app/chamados/juizes.py:822` `pergunta_viva`, corpo `:1135` | Censo direto extenso é igual ao colado no e2.r; as chamadas adicionais de apresentação estão nas tabelas acima, `explicador.py:72`, `fio.py:280`, `models.py:2502`, `:2809`. | Booleano por pergunta; juiz que deriva vitalidade. |
| `app/chamados/models.py:2492` `.estado`; `:2799` `.viva` | API `app/api/views.py:2569`; explicador `app/chamados/services/explicador.py:72`; vigia `vigia_fio.py:311`; fio `fio.py:280`; também templates `app/templates/chamados/partials/_card_atendimento.html:77`, `:183` | `.viva` delega totalmente a C6, `models.py:2809`; `.estado` lê C6 mas também traduz carimbos/resposta com fallback, `:2499`–`:2506`. |
| `app/chamados/services/fabrica_da_celula.py:74` `motivos_a_perguntar` | `perguntar_pela_celula :164`; `motivos_que_o_evento_deixou :199` | Motivos de perguntas ainda a emitir por célula; deriva deduplicação por motivo `:118`–`:126`, lendo dia_sem_trabalho `:91`, teto `:98`, parcial_so_lista `:102`. Não devolve tarefa completa do dia. |
| `app/chamados/services/fabrica_da_celula.py:191` `motivos_que_o_evento_deixou` | `app/chamados/management/commands/disparar_perguntas_competencia.py:112` | Leitor/vigia dos motivos que evento deixou; lê fábrica `:199`. |
| `app/chamados/models.py:1789` `DisputaSupervisao.pode_fechar` | `becos.py:33`, `:51`; reconciliador `:411`, `:601`, `:1159`; materializacao `:567`, `:1291`; competencia_trancada `:153`, `:202`; disputa_emissao `:1112`, `:1717`; entrada_sincronizada `:58` | Booleano por disputa; compõe C15 e `respostas_sem_veredito`, `models.py:1794`; regra adicional de respostas sem veredito `:1783`–`:1787`. |

Comparação com aval:

| Item do aval | Conferência |
|---|---|
| `vigia_fio.fio_do_dia:156` e `censo_alcance` | Confere; `censo_alcance` está em `app/chamados/services/vigia_fio.py:542`; faltou explicitar `censo:318`, dono `:274` e leitores da lista `:222`. |
| `resolver_dia_oferecido:97` + STATUS `:94` | Confere; único chamador direto é `contexto_calendario:769`; plural é leitura do campo, `calendario.py:135`. |
| C11 `beco_em_lote:442` | Confere função; chamada C17 real é `app/escala/services/cadastro_realidade.py:488`, **não `:460` como colado no e2.r**. A linha 460 é docstring. |
| Nove consumidores C15 “não são respondedores” | Não confere para **explicador**: produz `pode_fechar`, resumo e itens `explicador.py:163`, expostos ao admin `modal_fio.html:432`–`:437`. |
| “TODOS” os leitores na lista | Não confere: montador de fio, painel, relatório, aviso, drawer, pergunta_no_app.do_dia/retenção, dia_do_colab, desenlace, fila de validação e beco de disputa estão documentados acima com seus chamadores. |

O aval acrescenta explicitamente, frente à seção “leitores E3” do e2.r, `resolver_dia_oferecido` e C11, mas ambos já apareciam noutras seções do próprio e2.r colado: juiz “dono fora do fio” e chamadores C17. A identidade da versão da cópia com o HEAD `94d41ea` mencionado no aval é **[nao verificado]** nesta subanálise.

## 08/11 10:28 · sessao 9cde0294b438 · - · - · origem=Ronald (tmux)

| Achado | Evidência |
|---|---|
| **“`o_que_segura` (6 chamadores)” não confere nesta cópia.** O AST não encontrou chamadas de produção à função. A busca textual em Python também só encontra a definição, comentários e chamadas em `tests/`. Não há seis chamadores a enumerar. | Definição: `app/folha/fora_da_folha.py:203`; chamadas de teste: `app/folha/tests/test_haiku_fora_da_folha.py:66`, `:76`, `:145`; `app/chamados/tests/test_n1_alimentacao_unica.py:134`. |
| `_fio_do_dia` tem **três** chamadores de produção. | `fora_da_folha` em `app/folha/fora_da_folha.py:178`; `o_que_segura` em `app/folha/fora_da_folha.py:221`; `dias_sem_fio` em `app/ponto/services/pautas_dos_discordantes.py:77`. |
| `pendentes_do_colaborador` tem três chamadores: os dois kernels **e dashboard web**, ausente da lista do aval. | `app/api/views.py:325`; `app/api/views_core.py:314`; `app/core/views.py:253`. |
| `perguntas_do_app` tem cinco chamadores, incluindo **ficha do colaborador**, ausente da lista do aval. | `app/api/views.py:2552`; `app/chamados/juizes.py:1319`; `app/colaboradores/services/ficha.py:166`; `app/chamados/services/pergunta_no_app.py:139`, `:162`. |
| A ficha responde pendência ao consumidor: forma ações “validar as respostas” e “colaborador responder”, com dono e quantidade. | `app/colaboradores/services/ficha.py:169`, `:171`, `:190`, `:193`. |

A varredura foi feita com `ast.parse`, sem importar Django nem executar consultas, em todos os `.py` da árvore fora de diretórios `tests`, reconhecendo nomes importados e aliases; nomes de método também foram procurados por `ast.Attribute`. Ausência de chamadas obtida por essa varredura é resultado estático, não prova de uso dinâmico em produção. **Uso dinâmico externo à cópia: [nao verificado].**

| Respondedor | Unidade e retorno | Deriva ou lê juiz | Chamadores |
|---|---|---|---|
| `DisputaSupervisao.pendentes_do_colaborador` | QuerySet de **disputas abertas**, uma por PK, com ≥1 pergunta viva sem `respondida_em`. Não agrupa por dia. | Deriva o recorte da disputa; lê `q_vivas` e **C6 `pergunta_viva`** para as perguntas. | Definição/recorte: `app/chamados/models.py:1729`, `:1736`, `:1739`; chamadas `app/api/views.py:325`, `app/api/views_core.py:314`, `app/core/views.py:253`. |
| `perguntas_do_app` | Lista de **perguntas vivas**, ordenada por ID; sem exigir disputa aberta nem pergunta sem resposta. | Lê `q_vivas` e C6; aplica identity map. | `app/chamados/services/pergunta_no_app.py:42`, `:46`, `:53`; cinco chamadas na primeira tabela. |
| `api_disputa_perguntas` | Dois arrays de **perguntas**: `perguntas` e `respondidas`. | Lê `perguntas_do_app`; separa por `.estado == RESPONDIDA`. | `app/api/views.py:2534`, `:2552`, `:2568`, `:2570`; rota em `app/api/urls.py:92`. |
| `ficha_do_colab` | Chamados com verbo/dono; perguntas; ações e quantidade de perguntas a responder/validar, para um colaborador. | Lê `particionar_verbos`, `perguntas_do_app` e `respostas_sem_veredito`; deriva a seleção sem resposta por texto e a lista de ações. | `app/colaboradores/services/ficha.py:155`, `:158`, `:166`, `:169`, `:171`, `:190`, `:193`. |
| `folha._fio_do_dia` | `admin`, `colab`, `chamado` ou `nada`, por célula. | Lê **C17**; devolve `porta['fio']` ou `FIO_SEM_PORTA`. | `app/folha/fora_da_folha.py:70`, `:77`; chamadas `:178`, `:221`, `app/ponto/services/pautas_dos_discordantes.py:77`. |
| `folha.o_que_segura` | Um colaborador/competência, mais lista de **dias retidos**, fio, destrava e IDs de chamados por dia. | Compõe fontes: export, retenção da célula, C17; deriva grupo pelo pior fio. | `app/folha/fora_da_folha.py:203`, `:215`, `:217`, `:221`, `:225`, `:229`; zero chamadas de produção encontradas. |
| `folha.fora_da_folha` | Colaboradores retidos, grupo por pior dia, totais de dias por fio. | Lê `dias_que_seguram` e `_fio_do_dia`; agrega. | `app/folha/fora_da_folha.py:142`, `:161`, `:178`, `:181`, `:194`. |
| `dias_sem_fio` | Lista de **(colaborador, dia)** discordantes sem porta, com empresa/competência/célula. | Lê veredito da célula e `_fio_do_dia`/C17; filtra o universo para pauta. | `app/ponto/services/pautas_dos_discordantes.py:46`, `:69`, `:77`, `:79`. |

| Badge — **mesmo comportamento nos dois kernels** | Unidade | Evidência |
|---|---|---|
| Primeiro calcula `questionarios_pendentes = pendentes_do_colaborador(...).count()` | **Disputas distintas abertas** contendo pergunta C6 viva sem resposta carimbada; não perguntas, não `(colab,dia)`. | `app/api/views.py:325`; `app/api/views_core.py:314`; `app/chamados/models.py:1736`. |
| Se `questionarios_pendentes > 0`: `{count: questionarios_pendentes, cor: '#f59e0b', rotulo: 'Responder DP'}` | Disputas | `app/api/views.py:326`, `:327`; `app/api/views_core.py:315`, `:316`. |
| Senão, se há `chamados_nao_lidos`: `{count: chamados_nao_lidos, cor: '#ef4444', rotulo: 'Chamados'}` | **Chamados vivos** que têm followup `autor_tipo='dp_rh'` não lido pelo colaborador; não número de followups. | `app/api/views.py:318`, `:321`, `:328`, `:329`; `app/api/views_core.py:307`, `:310`, `:317`, `:318`. |
| Sem os dois: `None` | Nenhum objeto badge | `app/api/views.py:331`; `app/api/views_core.py:320`. |
| JSON expõe também os escalares `chamados_nao_lidos`, `questionarios_pendentes` | Chamados / disputas, respectivamente | `app/api/views.py:370`, `:371`, `:372`; gêmeo `app/api/views_core.py:359`, `:360`, `:361`. |

| Divergência concreta **permitida pelo código**, não ocorrência medida | Badge / listas | Evidência |
|---|---|---|
| Uma disputa aberta possui duas perguntas de motivos diferentes, ambas C6 vivas e sem resposta. | Badge **1**; `perguntas_do_app` **2**; array API `perguntas` **2**. Elas podem pertencer ao mesmo dia: a identidade do fato inclui motivo. | Distinct por disputa: `app/chamados/models.py:1736`; identidade por colab/motivo/data: `app/chamados/models.py:2392`; lista sem dedup por disputa/dia: `app/chamados/services/pergunta_no_app.py:46`, `:53`; array: `app/api/views.py:2570`. |
| Uma pergunta C6 viva está numa disputa fechada, sem outras pendências ou não lidos. | Lista **1**; contagem de questionários **0**; `chamados_badge=None`. | Badge exige `disputa__fechada_em IS NULL`: `app/chamados/models.py:1737`; lista não exige: `app/chamados/services/pergunta_no_app.py:46`; badge vazio: `app/api/views.py:331`, `app/api/views_core.py:320`. |
| Uma pergunta tem resposta com texto, continua C6 viva e não foi validada. | `perguntas_do_app` inclui; API coloca em `respondidas`; badge de disputas exclui essa pergunta pelo carimbo de resposta. | Vitalidade com resposta: `app/chamados/juizes.py:1150`, `:1162`; estado: `app/chamados/models.py:2502`; separação: `app/api/views.py:2570`; exclusão: `app/chamados/models.py:1738`. |
| Qual número o colaborador verá **depois de uma E3 futura** | **[nao verificado]**; esta cópia define os números acima. | **[nao verificado]**. |

| `o_que_segura`: origem | O que determina | Direção |
|---|---|---|
| `classificar_export(empresa,mes,ano)` | Item do colaborador; resposta externa `entra = item.status`, `motivo = item.motivo`. Não calcula valor de rubrica nesse corpo. | Chamado antes do fio em `app/folha/fora_da_folha.py:215`; lido no retorno em `:230`; responsabilidade expressa/corpo do export em `app/folha/export.py:757`, `:766`, `:829`. |
| `motivos_retencao_celula(..., com_dias=True)` | Motivos e lista de dias/células com data, veredito e motivo. | `_dias_que_retem`: `app/folha/fora_da_folha.py:95`, `:98`; chamada `:217`; retenção em `app/folha/export.py:1206`, `:1218`, `:1233`, `:1253`. |
| C17 via `_fio_do_dia` | Campo `fio`; `DESTRAVA[fio]`; grupo pelo pior fio. | `app/folha/fora_da_folha.py:77`, `:78`, `:221`, `:226`, `:231`; escada da porta em `app/chamados/juizes.py:1327`, `:1332`. |
| C17 via `_porta` | Campo `chamado`: lista da chave `fila`, não todas as âncoras. | `app/folha/fora_da_folha.py:67`, `:225`; saída C17 em `app/chamados/juizes.py:1385`, `:1386`. |
| Fio → `classificar_export` | **Não há dependência no percurso auditado**: o export decide seus gates e termina retornando `out`; não recebe fio/porta, e `o_que_segura` não escreve seu resultado de volta. | `app/folha/export.py:757`, `:858`, `:864`, `:868`, `:960`, `:966`, `:973`; composição local em `app/folha/fora_da_folha.py:215`, `:229`. |
| `classificar_export` → fio | Em `o_que_segura`, **não altera** a classificação do fio: `_dias_que_retem` é chamado independentemente do status/motivo export, e C17 recebe IDs das células desses dias. Em `dias_que_seguram`/`fora_da_folha`, export **seleciona o universo** de colaboradores `furo_espelho` cujos fios serão lidos; não muda a regra C17. | Independência local: `app/folha/fora_da_folha.py:215`, `:217`, `:218`, `:221`; seleção do universo: `:119`, `:123`, `:128`, `:137`, `:161`, `:178`. |
| Fontes compartilhadas | Ambos leem estados da célula. Isso permite correlação das saídas sem uma escrever na outra. `classificar_export` chama retenção do espelho, que delega à mesma retenção da célula. | `app/folha/export.py:960`, `:1267`; C6 lê veredito em `app/chamados/juizes.py:1181`; C17 alimenta perguntas via C6 em `app/chamados/juizes.py:1302`. |

| Campo por item API | Quem escreve o JSON | Fonte/autor do valor |
|---|---|---|
| `estado` | `api_disputa_perguntas`, `app/api/views.py:2571` | `.estado`, propriedade derivada, `app/chamados/models.py:2492`; validação/materialização primeiro, depois C6 e `respondida_em`: `:2499`, `:2502`, `:2503`. |
| `aguarda_admin` | `app/api/views.py:2572` | Comparação local com `PerguntaDisputa.RESPONDIDA`; não coluna. |
| `disputa_id` | `app/api/views.py:2573` | FK `p.disputa`; declarada em `app/chamados/models.py:2211`, gravada no nascimento em `:2476`. |
| `chamado_id` | `app/api/views.py:2574` | **`p.chamado_destino_id`**, FK declarada em `app/chamados/models.py:2819`; nascimento `:2477`; reapontamento no gate `:2450`; porta de reapontamento `app/chamados/services/fio.py:845`. |
| `id` | `app/api/views.py:2575` | PK da pergunta criada em `app/chamados/models.py:2476`. |
| `motivo` | `app/api/views.py:2576` | Coluna `app/chamados/models.py:2216`; gate `abrir` recebe e cria em `:2398`, `:2476`. |
| `pergunta` | `app/api/views.py:2577` | Coluna `app/chamados/models.py:2217`; emissor fornece texto em `app/chamados/services/disputa_emissao.py:392` e gate grava `**campos` em `app/chamados/models.py:2477`. |
| `tipo_resposta` | `app/api/views.py:2578` | Coluna `app/chamados/models.py:2222`; emissor fornece em `app/chamados/services/disputa_emissao.py:393`, `:774`, `:1199`. |
| `opcoes` | `app/api/views.py:2579` | Coluna `app/chamados/models.py:2223`; emissor fornece em `app/chamados/services/disputa_emissao.py:394`, `:775`; serializer aplica `p.opcoes or []` e remove `OUTRO_MOTIVO` quando `tipo_resposta=='hora'`: `app/api/views.py:2557`, `:2559`. |
| `resposta_colab` | `app/api/views.py:2580` | Coluna `app/chamados/models.py:2227`; POST chama core em `app/api/views.py:2643`; core escreve texto em `app/chamados/services/disputa_emissao.py:1282` e salva em `:1301`; resposta automática também escreve em `:1433`. |
| `resposta_display` — adicional ao recorte do aval | `app/api/views.py:2581` | Propriedade `app/chamados/models.py:2867`: nome de posto/escala, rótulo da opção ou resposta original (`:2874`, `:2882`, `:2889`, `:2891`). |
| `respondida_em` — adicional ao recorte do aval | `app/api/views.py:2582` | ISO ou `None`. Escritores: core `app/chamados/services/disputa_emissao.py:1283`; automático `:1434`; trilha `:1450`; carimbo com texto `app/chamados/models.py:2589`; reset em memória de ressuscitação `app/chamados/models.py:1457`. |

| Autores dos campos de nascimento `motivo`, `pergunta`, `tipo_resposta`, `opcoes`, `disputa`, destino | Chamada do gate |
|---|---|
| Emissão por catálogo: texto com data e opções estáticas/dinâmicas | `app/chamados/services/disputa_emissao.py:294`, `:302`, `:304`, `:310`, `:317`, `:389`. |
| Agrupador de perguntas de hora | `app/chamados/services/disputa_emissao.py:770`. |
| Pergunta de âncora ausente | `app/chamados/services/disputa_emissao.py:1195`. |
| Declaração do colaborador / aplicação de texto pelo admin | `app/chamados/services/declaracao_texto.py:325`, `:614`. |
| Reabertura administrativa | `app/chamados/services/acoes_disputa.py:259`. |
| Persistência central e reaproveitamento de identidade | `app/chamados/models.py:2431`, `:2450`, `:2476`. |

| Campos externos da resposta API | Escritor e conteúdo |
|---|---|
| Lista vazia | `em_disputa=False`, `perguntas=[]`, `respondidas=[]`, em `app/api/views.py:2562`. |
| `em_disputa=True` | Significa lista viva não vazia neste endpoint; não exige disputa aberta, em `app/api/views.py:2561`, `:2585`, e `app/chamados/services/pergunta_no_app.py:46`. |
| `disputa_id` externo | Disputa da **primeira** pergunta da lista, em `app/api/views.py:2563`, `:2586`. |
| `chamado_id` externo | Primeiro destino de pergunta não respondida; fallback para chamado da primeira disputa, em `app/api/views.py:2590`, `:2592`. |
| `perguntas` e `respondidas` | Separação por estado, em `app/api/views.py:2570`, `:2593`, `:2594`. |
| `chamados_badge.count`, `.cor`, `.rotulo` | Objetos construídos diretamente em `api_me` dos dois kernels: `app/api/views.py:327`, `:329`; `app/api/views_core.py:316`, `:318`. Não há coluna gravada para esses campos. |

| Divergência da prosa existente com o vivo | Código |
|---|---|
| Comentário de `pendentes_do_colaborador` afirma badge e lista “no MESMO conjunto”, mas badge adiciona disputa aberta e ausência de carimbo de resposta. | Comentário `app/chamados/models.py:1734`; filtros `:1737`, `:1738`; lista `app/chamados/services/pergunta_no_app.py:46`. |
| Docstring de `api_disputa_perguntas` promete perguntas não respondidas de disputa aberta; corpo retorna vivas inclusive respondidas e sem filtro de abertura. | Docstring `app/api/views.py:2536`; corpo `:2552`, `:2570`; fonte `app/chamados/services/pergunta_no_app.py:46`. |
| Comentário de `perguntas_do_app` enumera “quatro chamadores”; AST encontra também ficha, total cinco. | Comentário `app/chamados/services/pergunta_no_app.py:49`; chamada adicional `app/colaboradores/services/ficha.py:166`. |

Ocorrência/quantidade atual em produção, comportamento visual do APK/PWA e resultado de uma E3 futura: **[nao verificado]**.

## 08/11 10:27 · sessao 24e28cd92da8 · - · - · origem=Ronald (tmux)

AST de todos os `.py` fora de diretórios `tests/`, resolvendo `ImportFrom` e aliases locais (`fio_vivo as _fio_vivo`): **13 chamadas C15, em 12 funções; 3 chamadas C17**. Os pontos abaixo são a evidência concreta do conjunto; uso dinâmico não demonstrado: [nao verificado].

| Chamador direto C15 | Chamada | Papel efetivamente exercido |
|---|---|---|
| `vigia_fio.fio_do_dia` | `app/chamados/services/vigia_fio.py:166` | **Respondedor**: devolve booleano para perguntas recebidas; `censo` lê em `:414` e `:417` e incrementa `fio_vivo` em `:418`. |
| `DisputaSupervisao.pode_fechar` | `app/chamados/models.py:1794` | **Juiz de outra pergunta**: autorização de fechar disputa, combinando ausência de respostas sem veredito com ausência de fio vivo (`:1789–1794`). Também alimenta respondedor de beco: `app/chamados/services/becos.py:33`. |
| `disputa_viva_do_chamado` | `app/chamados/juizes.py:2084` | **Gate de outra ação**: retorna disputa aberta que trava encerramento do chamado; ausência de perguntas também trava (`:2080–2086`). Chamado por alias `_disp_viva(self)` em `app/chamados/models.py:148`, seguido de erro falante ao usuário em `:150–154`. |
| `supra_juiz.Command.handle` | `app/ponto/management/commands/supra_juiz.py:215` | **Insumo de classificação/auditoria**: conta chamados com fio e passa `cobrancas_vivas=len(_com_fio)` a `classificar_dia` (`:223–225`); agrega placar e exemplos (`:235–238`). |
| `reconciliar_grade.Command._relavrar_orfa` | `app/ponto/management/commands/reconciliar_grade.py:460` | **Gate de relavra**: pergunta respondida viva impede relavra, retornando `ORFA_FIO_PRESO` (`:460–462`); chamada em `:228` agrega resultado ao plano. |
| `emitir_furo_retroativo.Command.handle` | `app/ponto/management/commands/emitir_furo_retroativo.py:98` | **Insumo para emitir**: alimenta `classificar_dia(cobrancas_vivas=...)` (`:101–110`) e só prossegue em `FURO_SEM_COBRANCA` (`:111–113`). |
| `folha.previa._material` | `app/folha/previa.py:144` | **Respondedor da prévia**: devolve `resposta do colaborador`, `pergunta viva (sem resposta)` ou `nada` (`:139–146`); escreve campo `material` de cada item de dia/marco em `:95`. |
| `limbo_folgas.filhos_no_defeito` | `app/chamados/services/limbo_folgas.py:201` | **Gate de fechamento do pai**: seleciona filhos cujo dia ainda tem defeito de cadastro e fio vivo (`:191–205`); `detectar_ausencias` lê em `app/ponto/management/commands/detectar_ausencias.py:156`. |
| `cartorio._julgar_colab_corpo` | `app/ponto/services/cartorio.py:1339` | **Insumo da impressão/julgamento**: conta chamados com fio e escreve `fio=len(_com_fio)` em `impressao_insumos` (`:1348–1352`). |
| `cartorio._julgar_colab_corpo` | `app/ponto/services/cartorio.py:1595` | **Gate de reconciliação**: pula chamado sem perguntas ou com fio vivo; só continua tratando fio morto (`:1591–1601`). |
| `explicador.por_que_nao_fecha` | `app/chamados/services/explicador.py:137` | **Respondedor administrativo**: constrói `pode_fechar`, `resumo` e `itens` (`:137–163`); `montar_chamados_fio` chama em `app/chamados/services/fio.py:308` e anexa `ch.explicacao` em `:310`. |
| `encerrar_perguntas_com_fato.Command.handle` | `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83` | **Gate de fechamento**: fio vivo incrementa barrada e pula disputa; do contrário tenta `d.fechar` (`:83–90`). |
| `disputa_emissao._ja_perguntou` | `app/chamados/services/disputa_emissao.py:1244` | **Deduplicação de emissão** por `(disputa,motivo,data_evento)`, com guarda de colaborador ativo (`:1237–1244`); chamada em `:765` e pelo comando `disparar_perguntas_competencia.py:173`. |

**Fora dos nove nomes do aval existem três chamadores diretos:** `vigia_fio.fio_do_dia`, `DisputaSupervisao.pode_fechar` e `juizes.disputa_viva_do_chamado`, conforme chamadas `vigia_fio.py:166`, `models.py:1794`, `juizes.py:2084`.

**A frase “os nove nomeados não são respondedores” não confere:** `explicador` responde ao admin por disputa (`app/chamados/services/explicador.py:163` → `app/chamados/services/fio.py:310`), e `folha/previa` responde com campo `material` apresentado na prévia (`app/folha/previa.py:95`, `app/folha/previa.py:144`). Os demais têm o uso específico descrito na tabela, sem inferir intenção humana.

C15 é uma redução existencial: `any(pergunta_viva(p) for p in perguntas)`; o universo é entregue pelo chamador e não tem necessariamente unidade `(colab,dia)` (`app/chamados/juizes.py:1233`, `app/chamados/juizes.py:1242`).

| Chamador direto C17 | O que lê hoje | Efeito de retirar o símbolo público sem adaptar chamador |
|---|---|---|
| `fora_da_folha._porta` | Importa C17 em `app/folha/fora_da_folha.py:66` e chama em `:67`. `_fio_do_dia` lê `porta['fio']` ou `FIO_SEM_PORTA` em `:77–78`; `o_que_segura` também lê `fila` em `:225`. | A importação deixa de resolver; ficam afetados `fio`, frase `DESTRAVA`, chamados mostrados e grupo por fio (`app/folha/fora_da_folha.py:221–231`). Não equivale automaticamente a alterar `classificar_export`, chamado separadamente em `:215`. |
| `vigia_fio.censo_alcance` | Importa em `app/chamados/services/vigia_fio.py:556`, chama por chave `(cid,dt)` em `:635`. Lê `rota` e `verbos` em `:642–646`; só sem porta recorre a dono e causas em `:654–685`. | A importação deixa de resolver; perde-se a partição porta/sem porta usada nos contadores e nas linhas de alcance (`:635–646`). Chamado pelo comando em `app/chamados/management/commands/vigiar_fio_morto.py:173`. |
| `cadastro_realidade.beco_em_lote` C11 | Importa em `app/escala/services/cadastro_realidade.py:464`; **chamada viva em `:488`**, não `:460`. Filtra dias com perguntas vivas e C17 `None`; depois exige âncora terminal (`:515–517`), defeito folga (`:518`) e produz assinatura C11 (`:530–540`). | A importação deixa de resolver; a seleção “sem porta” do C11 precisa continuar disponível/adaptada para o lote produzir assinaturas. Entradas são a janela individual em `:439` e o lote das competências abertas em `:573`; efeitos adicionais de uma substituição ainda não implementada: [nao verificado]. |

C17 devolve `None` ou `{rota,fila,verbos,fio}`; compõe chamados da célula na fila com perguntas vivas no app, resposta não vazia e âncora na fila (`app/chamados/juizes.py:1373–1387`). “Resolver dia” fica explicitamente fora desse critério (`app/chamados/juizes.py:1367–1369`). A linha `cadastro_realidade.py:460` do anexo não é chamada no vivo: é docstring; a chamada AST está em `:488`.

Outra omissão relevante ao censo amplo: **`chamados/services/becos.py::classificar`** responde `None|FECHAVEL|BARRADA` por disputa e lê C7 em `:33`; combina disputa aberta, visibilidade por `respondida_em`/destino e pai terminal (`app/chamados/services/becos.py:25–33`). Não é chamador direto C15, mas é respondedor indireto de beco.

## 08/11 10:22 · sessao 3832ccb31504 · COBPARC-AUSENCIA-CALA-DOIS-JUIZES · censo · origem=Code

**Não há um “sim” uniforme.** Para a parcial não-falta **sem período**, (A) cala a geração da pergunta; (B) continua julgando célula/ata e considera o dia furado se encontrar lâmpada `acesa=False`. O cartório bloqueia o nascimento do chamado por outro predicado, `parcial_so_lista`. Isso não fecha universalmente chamados existentes. `app/chamados/services/materializacao.py:81`; `app/chamados/juizes.py:480`; `app/ponto/services/cartorio.py:398`; `app/ponto/services/cartorio.py:1777`.

“Parcial”, abaixo, significa o predicado do código: tipo que aceita horas, registro de um dia e `0 < minutos < 480`. A ausência de `hora_ini/hora_fim` não determina parcialidade. `app/ponto/turnos.py:2598`.

**1. Censo dos sítios executáveis**

Nas tabelas, **“não cala” significa que aquela guarda deixa o julgamento continuar**, sem afirmar que efetivamente nascerá cobrança; os próprios emissores possuem outras guardas. `app/chamados/services/disputa_emissao.py:763`; `app/chamados/services/disputa_emissao.py:765`; `app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/services/fabrica_da_celula.py:102`.

| Sítio | Parcial não-falta sem período | Efeito |
|---|---|---|
| `app/ponto/turnos.py:2598` — `cobre_parte_do_dia` | `True`; não lê as horas. | Define parcialidade. |
| `app/ponto/turnos.py:2632` — `periodo_da_parcial_no_dia` | Retorna `'sem_periodo'`. | Classifica período. |
| `app/ponto/turnos.py:2675` — `ausencia_explica_o_dia_inteiro` | `False`. | Não reconhece cobertura inteira. |
| `app/chamados/services/materializacao.py:81` — (A) | **Cala** pelo segundo termo, em `:82`. | Guarda da pergunta e da reemissão. |
| `app/chamados/juizes.py:449` — (B) | Não cala pela ausência; segue à ata. Lâmpada falsa prende em `:480`; sem célula/ata lavrada retorna `None` em `:464`/`:478`. | Premissa/S122/fechamento/reabertura. |
| `app/chamados/juizes.py:410` — `ausencia_parcial_cobra_o_resto` | `True` para registro `tipo_dia='ausencia'` com `cobertura_parcial`. Não lê período. | Predicado compartilhado dos leitores do resto. |
| `app/chamados/juizes.py:426` — `dia_sem_trabalho` | A parcial só responde “sem trabalho” se `trabalha is False`. | Não cala pelo mero rótulo ausência. |
| `app/escala/utils.py:1211` e `app/escala/utils.py:1255` — `_marcar_ausencias` | Marca parcial e `'sem_periodo'`; não dispensa missing, pela guarda em `:1265`. | Grade/folha/insumo da ata. |
| `app/ponto/services/cartorio.py:398` — `parcial_so_lista` | **Cala nascimento**, retornando `True` incondicionalmente. | Chamado/pergunta/push. |
| `app/ponto/services/cartorio.py:436` — `_cob_parcial_da_impressao` | Conserva `'sem_periodo'` na impressão dos insumos. | Controle de rejulgamento; não decide cobrança sozinho. |
| `app/ponto/services/cartorio.py:732` e `app/ponto/services/cartorio.py:736` — `ata_do_dia` | Transporta parcialidade e período. | Lavratura da ata. |
| `app/ponto/services/cartorio.py:766` — `dispensar_por_ausencia` | **Não dispensa** pela ausência parcial. | Mantém lâmpadas recebidas da grade. |
| `app/escala/services/leitor_celula.py:416` e `app/escala/services/leitor_celula.py:418` | Transporta os dois carimbos da ata. | Alimentação dos leitores; não recalcula cobertura. |
| `app/ponto/supra_juiz.py:381` e `app/ponto/supra_juiz.py:439` | Com fato/batida nas condições dos respectivos ramos, produz `PARCIAL_SEM_PERIODO`. | Classificação para cadastro. |
| `app/ponto/supra_juiz.py:530` | Sem período não constitui `_resto_cobrado`; aprovada + chamado vivo produz `COBRANCA_EM_DIA_COBERTO`, em `:532`. | Protesto sobre cobrança existente. |

O supra **não trata todo sem-período como `PARCIAL_SEM_PERIODO`**: o ramo sem batida admite a parcial como dia de cobrança e pode produzir `FURO_SEM_COBRANCA`; a emissão é barrada posteriormente pelo cartório. `app/ponto/supra_juiz.py:304`; `app/ponto/supra_juiz.py:313`; `app/ponto/supra_juiz.py:367`; `app/ponto/services/cartorio.py:1725`.

Todos os leitores executáveis de `ausencia_parcial_cobra_o_resto`, além de `dia_sem_trabalho`:

| Sítio | Resposta local para sem período | Efeito |
|---|---|---|
| `app/chamados/services/declaracao_texto.py:98` | Mantém a parcial; enumera missing em `:110`. | Marcos faltantes/texto da cobrança. |
| `app/escala/services/furos_diarios.py:49` | Mantém o dia e conta furos em `:54`. | `FuroDiario`/métricas. |
| `app/relatorios/furos.py:195` | Mantém a parcial e apresenta marcos apagados em `:202`. | Raio-X/relatório. |
| `app/inteligencia/calculadores.py:50` | Mantém a parcial e conta marcos apagados em `:64`. | `preenchimento_espelho`. |
| `app/inteligencia/calculadores.py:149` | Mantém a parcial e conta previstos/batidos em `:160`. | `preenchimento_marcos`. |

Leitores adicionais de `dia_sem_trabalho`, que recebem essa mesma exceção da parcial:

| Sítio | Resposta local para sem período | Efeito |
|---|---|---|
| `app/chamados/models.py:1245` | Parcial não regulariza apenas pelo rótulo ausência. | Coletor da premissa. |
| `app/chamados/services/fabrica_da_celula.py:91` | Essa guarda deixa passar; `parcial_so_lista` cala depois, em `:102`. | Pergunta/pai/push. |
| `app/chamados/services/materializacao.py:174` | Deixa a parcial participar da avaliação do plantio. | Verificação de preenchimento do furo/paridade. |
| `app/ponto/services/cluster_espurio.py:25` | Deixa participar da leitura das lâmpadas/órfãs. | Diagnóstico do cluster; nascimento recebe bloqueio separado. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:51` | Deixa participar; a cobrança exige condições das lâmpadas em `:69`. | Furo de intervalo. |
| `app/ponto/management/commands/reconciliar_grade.py:282` | Parcial com `trabalha!=False` não entra nos mortos por essa cláusula. | Retratação de cobrança. |
| `app/ponto/supra_juiz.py:71` e `app/ponto/supra_juiz.py:496` | Consulta o juiz no ramo sem trabalho; esse ramo exige `cobertura_status is None`, em `:495`. | `COBRANCA_EM_DIA_SEM_TRABALHO`. |

Demais chamadas executáveis dos predicados pedidos:

| Sítio | Resposta para parcial sem período | Efeito |
|---|---|---|
| `app/escala/servico_jornada.py:344` | A parcial não cala essa guarda. O laço consulta **todas** as ausências, em `:342`, não só a vencedora. | `classificar_falta`. |
| `app/ponto/turnos.py:2690` | Exclui a parcial dos dias inteiros; exige também efeito `ABONA`. | Folha/pontualidade. |
| `app/ponto/services/fechamento.py:694` | Parcial paga não abona o dia inteiro; conta pelas batidas, em `:695`. | Folha. |
| `app/ponto/services/efeito_ausencia.py:48` | Soma minutos da parcial paga. | Crédito financeiro. |
| `app/ponto/services/efeito_ausencia.py:55` | Parcial paga não justifica/suprime o dia inteiro. | Folha/espelho. |
| `app/ponto/services/credito_parcial.py:27` | Credita minutos pagos no próprio dia. | Folha/espelho. |
| `app/ponto/services/dia_decidido.py:494` | Conserva minutos no rótulo da parcial abonada. | Tela. |
| `app/ponto/services/ausencia.py:883` e `app/ponto/services/ausencia.py:887` | Validador rejeita hora faltante e período em não-parcial. Só é chamado quando alguma hora foi fornecida, em `:906`. | Cadastro. |
| `app/ponto/services/vigia_ausencia_veredito.py:54` | Pula parcial no contador. | Vigia de discordância. |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` | Pula parcial no contador. | Diagnóstico. |
| `app/ponto/views.py:2084` | Entrega parcialidade ao painel. | Tela. |
| `app/ponto/views.py:2240` | Marca `a.cobre_parte=True`. | Lista de ausências. |
| `app/templates/ponto/ausencias.html:163` | Mostra minutos. | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:296` e `app/templates/ponto/partials/painel_ausencia.html:311` | Mostra edição/aprovação do período e os avisos sobre lista do DP, em `:299`/`:313`. | Tela; não publica uma linha administrativa. |

A exclusão da parcial no helper de dias inteiros também chega aos consumidores em `app/ponto/services/fechamento.py:687`, `app/ponto/services/efeito_ausencia.py:65`, `app/ponto/turnos.py:2695`, `app/ponto/management/commands/diff_calculador.py:412`, `app/ponto/services/esmeril_espelho.py:164` e `app/relatorios/management/commands/diff_extrato_parcial.py:67`.

Há ainda o juiz registrado da cobrança viva: `reconciliar_por_ausencia` escolhe a vencedora, mas, para aprovada, exige `_premissa_morre` e sucesso de `ch.reconciliar()`. Não testa `'sem_periodo'` diretamente. `app/chamados/services/silencio_ausencia.py:65`; `app/chamados/services/silencio_ausencia.py:93`; `app/chamados/services/silencio_ausencia.py:101`.

**2. Grade, lâmpadas, emissão e lista administrativa**

A grade conserva os missing da parcial sem período: escreve `tipo_dia='ausencia'`, `cobertura_parcial=True` e `'sem_periodo'`; a transformação de missing em dispensada exige período diferente de `'sem_periodo'`. `app/escala/utils.py:1241`; `app/escala/utils.py:1251`; `app/escala/utils.py:1255`; `app/escala/utils.py:1265`.

Na ata, **os marcos missing conhecidos ficam `acesa=False`**. Isso não significa que todas as lâmpadas do dia apaguem: as células cumpridas continuam acesas; existe também resposta `None` para fato cego sem coluna atribuível. `app/ponto/services/cartorio.py:647`; `app/ponto/services/cartorio.py:677`; `app/ponto/services/cartorio.py:689`; `app/ponto/services/cartorio.py:641`.

`ata_do_dia` grava as lâmpadas, conta as falsas e transporta os carimbos da parcial. `dispensar_por_ausencia` não as acende pela ausência parcial; `dia['justificado']` constitui outra fonte explícita de dispensa. `app/ponto/services/cartorio.py:704`; `app/ponto/services/cartorio.py:712`; `app/ponto/services/cartorio.py:732`; `app/ponto/services/cartorio.py:736`; `app/ponto/services/cartorio.py:766`; `app/ponto/services/cartorio.py:768`.

**O cartório não cria o chamado de furo desse dia com esses carimbos:** calcula `parcial_so_lista` e o exige falso nos dois portões de emissão. Um chamado vivo pode permanecer e receber espelhamento dos marcos. `app/ponto/services/cartorio.py:1706`; `app/ponto/services/cartorio.py:1725`; `app/ponto/services/cartorio.py:1777`; `app/ponto/services/cartorio.py:1787`.

Outros consumidores do bloqueio:

| Sítio | Ação para sem período |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:102` | Retorna sem motivos/pergunta/pai. |
| `app/chamados/services/disputa_emissao.py:871` e `app/chamados/services/disputa_emissao.py:894` | Classifica pelo predicado e remove o chamado da fábrica de disputas/perguntas. |
| `app/chamados/services/pergunta_no_app.py:88` | Rotula retenção como `parcial_na_lista_do_admin`. |
| `app/chamados/reconciliador.py:872` | Bloqueia nascer/renascer fora da costura em chamado já vivo. |
| `app/ponto/services/cluster_espurio.py:103` | Remove o dia de `a_abrir`, em `:107`. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154` | Conta e pula nova pergunta. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121` | Conta e pula emissão retroativa. |

Quanto à **lista do admin**, há três coisas distintas no código:

- Classificação `PARCIAL_SEM_PERIODO`, derivando `indefinida`: `app/ponto/supra_juiz.py:388`; `app/ponto/supra_juiz.py:441`; `app/escala/models.py:1615`.
- Contagem `parcial_na_lista_sem_push`: `_contar_na_lista` apenas incrementa contador em memória e deduplica a chave. `app/ponto/services/cartorio.py:1829`; `app/ponto/services/cartorio.py:1832`.
- Texto da tela dizendo que o dia está na lista do DP: `app/templates/ponto/partials/painel_ausencia.html:299`.

A tela administrativa chama `cadastro_realidade.lista`; sua fonte C11 exige especificamente o protesto `PREVISTO_COM_DIA_INDEFINIDO`. Há também uma fonte genérica para ausência com pelo menos duas batidas. Essas condições não demonstram inclusão de **toda parcial sem período**. `app/escala/views.py:484`; `app/escala/services/cadastro_realidade.py:477`; `app/escala/services/cadastro_realidade.py:478`; `app/ponto/services/esmeril_espelho.py:133`.

**Publicação efetiva de um dia específico numa lista administrativa por ser “parcial sem período”: [não verificado].**

**3. Chamadores de (A), de (B) e ações**

Os chamadores executáveis diretos de **(A)** são:

| Chamador | Onde a resposta vira ação |
|---|---|
| `app/chamados/services/disputa_emissao.py:763` — `_criar_pergunta_se_couber` | `True` retorna antes de `PerguntaDisputa.abrir`, em `:770`. |
| `app/chamados/reconciliador.py:845` — `reemitir_chamado_do_dia` | `True` retorna antes de abrir/reutilizar o chamado, em `:900`. |
| `app/chamados/services/pergunta_no_app.py:90` — `retencao` | Retorna causa diagnóstica; não fecha chamado. A classificação administrativa vem antes, em `:88`. |

Os chamadores executáveis diretos de **(B)** são:

| Chamador | Efeito |
|---|---|
| `app/chamados/models.py:1260` — `premissa_regularizada` | Quando B responde, devolve `not _f`, em `:1262`. |
| `app/chamados/juizes.py:471` | Recursão para a véspera quando o dia é sem trabalho; conserva `para`. |

O coletor pode aceitar provas **antes de B**: justificativa aprovada, decisão administrativa, dia sem trabalho para chamado que não seja `turno_aberto`, marcos acesos ou inexistentes. `app/chamados/models.py:1227`; `app/chamados/models.py:1243`; `app/chamados/models.py:1245`; `app/chamados/models.py:1249`; `app/chamados/models.py:1258`.

As ações posteriores são:

- **S122:** usa `premissa_regularizada()` no efeito padrão cobrança; se não regularizado, recusa transição para `resolvido`. `app/chamados/models.py:175`; `app/chamados/models.py:177`; `app/chamados/models.py:185`.
- **Fechamento do sistema:** consulta a premissa com `para='folha'`; há ainda outras guardas antes de gravar o fechamento. `app/chamados/models.py:701`; `app/chamados/models.py:648`; `app/chamados/models.py:617`.
- **Reabertura:** premissa regularizada com `para='folha'` produz recusa; `reabrir` registra e retorna `False`. Premissa não regularizada apenas deixa essa guarda passar. `app/chamados/models.py:1309`; `app/chamados/models.py:1310`; `app/chamados/models.py:1361`; `app/chamados/models.py:1366`.

**4. Juiz registrado**

**Sim: C11**, pergunta **“a ausência do dia suspende, fecha ou levanta a cobrança viva?”**, aponta para `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia`. `app/core/juizes.py:117`; `app/core/juizes.py:271`.

Também existe **C18**, para a pergunta mais estreita sobre a cobrança calar porque a célula lavrada diz que o dia não é de trabalho, apontando para `dia_sem_trabalho`. `app/core/juizes.py:149`; `app/core/juizes.py:278`.

O documento registra explicitamente a exceção parcial que cobra o resto e a regra especial de (A) para `'sem_periodo'`; portanto não descreve uma resposta única compartilhada por A e B. `app/docs/JUIZES.md:62`; `app/docs/JUIZES.md:65`; `app/docs/JUIZES.md:66`.

**5. Contraexemplos e limites**

Os seguintes resultados decorrem das condições do código; ocorrência produtiva concreta: **[não verificado]**.

| Condição | (A) | (B) |
|---|---|---|
| **Parcial com período; todos os missing dispensados dentro dele; nenhuma órfã** | Retorna `False`: não cala pela ausência. Com horas declaradas, `periodo(a, ())` retorna `'fora'`. `app/chamados/services/materializacao.py:81`; `app/ponto/turnos.py:2641`. | Retorna `False`: sem furo, pois as dispensadas ficam acesas e não há órfãs. **Diferença semântica, apesar dos dois booleanos serem falsos.** `app/escala/utils.py:1265`; `app/ponto/services/cartorio.py:647`; `app/chamados/juizes.py:480`; `app/chamados/juizes.py:490`. |
| **Falta aprovada; ata `'falta'`; véspera responde `False` ou `None`** | Não cala: ambos os termos excluem falta. `app/chamados/services/materializacao.py:82`; `app/ponto/turnos.py:2675`. | Retorna `False` pela regra do dia sem trabalho/véspera. Para chamado não-turno, o coletor já regulariza antes de B. `app/chamados/juizes.py:428`; `app/chamados/juizes.py:471`; `app/chamados/juizes.py:472`; `app/chamados/models.py:1245`. |

**Tipo integral não-falta com efeito diferente de `ABONA` não é, sozinho, divergência A/B:** ambos calam sem testar efeito. A diferença está no helper financeiro, que exige `ABONA`. `app/chamados/services/materializacao.py:81`; `app/chamados/juizes.py:449`; `app/ponto/turnos.py:2690`.

**`para='folha'` também não diferencia uma única vencedora aprovada comum aos dois.** A fixa cobrança; B aceita o efeito recebido. Cobrança admite não rejeitadas, folha somente aprovadas: com lançamentos concorrentes, podem selecionar vencedoras diferentes pelo desempate `-data, -pk`. `app/chamados/services/materializacao.py:78`; `app/chamados/juizes.py:448`; `app/ponto/turnos.py:2514`; `app/ponto/turnos.py:2516`; `app/ponto/turnos.py:2557`.

## 08/11 10:20 · sessao 7681152d2c16 · - · - · origem=Ronald (tmux)

B não dá uma resposta única apenas pela presença da ausência aprovada: retorna `False` imediatamente só quando a ausência vencedora explica o dia inteiro; a parcial segue para célula, ata e lâmpadas. Uma lâmpada `acesa is False` devolve `True`; célula inexistente ou ata sem `lampadas` devolve `None`. Para parcial `tipo_dia='ausencia'`, `cobertura_parcial=True`, `dia_sem_trabalho` só cala por `trabalha is False`. Fontes: `app/chamados/juizes.py:448`, `app/chamados/juizes.py:449`, `app/chamados/juizes.py:460`, `app/chamados/juizes.py:477`, `app/chamados/juizes.py:480`, `app/chamados/juizes.py:426`.

Chamadores diretos executáveis de B:

| Chamador | Efeito |
|---|---|
| `app/chamados/models.py:1260` | `premissa_regularizada` devolve `not _f` quando B sabe responder (`app/chamados/models.py:1261`). |
| `app/chamados/juizes.py:471` | Recursão para véspera quando `dia_sem_trabalho(cel)`; preserva `para` e usa `False` se a véspera retorna `None` (`app/chamados/juizes.py:472`). |

O import em `app/chamados/models.py:3318` reexporta B. As ações que usam o coletor são:

- S122 humano: `transicionar(..., novo_status='resolvido')` chama `premissa_regularizada()` com padrão cobrança; ausência de regularização produz `ValueError`. `app/chamados/models.py:175`, `app/chamados/models.py:177`, `app/chamados/models.py:185`.
- Fechamento pelo sistema: `_gate_premissa_do_fechamento` usa `para='folha'`; premissa regularizada libera essa guarda. O fechamento passa também pela guarda da pergunta viva. `app/chamados/models.py:701`, `app/chamados/models.py:648`, `app/chamados/models.py:651`.
- `reconciliar` grava o estado decidido somente se `_decisao_do_fechamento` permite. `app/chamados/models.py:617`, `app/chamados/models.py:620`.
- Reabertura: `recusa_da_reabertura` usa `para='folha'`; premissa regularizada impede `reabrir`, registrando recusa e retornando `False`. `app/chamados/models.py:1309`, `app/chamados/models.py:1310`, `app/chamados/models.py:1361`, `app/chamados/models.py:1366`.
- Antes de chamar B, o coletor já aceita justificativa aprovada, decisão do admin, dia sem trabalho para chamados que não são `turno_aberto`, marcos acesos e marcos inexistentes. `app/chamados/models.py:1227`, `app/chamados/models.py:1243`, `app/chamados/models.py:1245`, `app/chamados/models.py:1249`, `app/chamados/models.py:1258`.

Censo de leitores executáveis de `ausencia_parcial_cobra_o_resto`:

| Sítio | Parcial sem período | Efeito |
|---|---|---|
| `app/chamados/juizes.py:426` | Cobra pela ata, salvo `trabalha=False`; não consulta período. | Juiz do dia sem trabalho; `app/chamados/juizes.py:427`. |
| `app/chamados/services/declaracao_texto.py:98` | Mantém a parcial e enumera seus `missing`. | Marcos faltantes da declaração; `app/chamados/services/declaracao_texto.py:110`, `app/chamados/services/declaracao_texto.py:123`. |
| `app/escala/services/furos_diarios.py:49` | Mantém o dia na apuração e conta os furos. | Alimenta FuroDiario; `app/escala/services/furos_diarios.py:54`, `app/escala/services/furos_diarios.py:57`. |
| `app/relatorios/furos.py:195` | Mantém a parcial e apresenta marcos apagados. | Raio-X da folha; `app/relatorios/furos.py:202`, `app/relatorios/furos.py:208`. |
| `app/inteligencia/calculadores.py:50` | Mantém parcial; conta marcos apagados. | `preenchimento_espelho`; `app/inteligencia/calculadores.py:57`, `app/inteligencia/calculadores.py:64`. |
| `app/inteligencia/calculadores.py:149` | Mantém parcial; conta previstos e batidos. | `preenchimento_marcos`; `app/inteligencia/calculadores.py:156`, `app/inteligencia/calculadores.py:160`. |

Esses leitores não testam `sem_periodo` nas condições citadas; preservam a leitura do resto da parcial, sujeita às demais guardas locais. `app/chamados/juizes.py:410`, `app/chamados/services/declaracao_texto.py:98`, `app/escala/services/furos_diarios.py:49`, `app/relatorios/furos.py:195`, `app/inteligencia/calculadores.py:50`, `app/inteligencia/calculadores.py:149`.

Leitores adicionais de `dia_sem_trabalho`:

| Sítio | Resposta local para parcial sem período |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:91` | Esta guarda deixa passar se `trabalha!=False`; a guarda seguinte `parcial_so_lista` cala pergunta, pai e push. `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:104`. |
| `app/chamados/services/materializacao.py:174` | Deixa a parcial participar da avaliação de plantio que preenche furo; pode absolver a guarda de paridade. `app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:181`. |
| `app/ponto/services/cluster_espurio.py:25` | Deixa a parcial participar; assinatura requer pelo menos uma lâmpada apagada e duas órfãs próximas. `app/ponto/services/cluster_espurio.py:32`, `app/ponto/services/cluster_espurio.py:33`, `app/ponto/services/cluster_espurio.py:35`. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:51` | Deixa a parcial participar; só lista pausas apagadas quando entrada e saída estão acesas. `app/ponto/management/commands/detectar_intervalo_ausente.py:69`, `app/ponto/management/commands/detectar_intervalo_ausente.py:71`. |
| `app/ponto/management/commands/reconciliar_grade.py:282` | Parcial com `trabalha!=False` não entra nos mortos por essa cláusula. `app/chamados/juizes.py:426`, `app/chamados/juizes.py:427`. |
| `app/ponto/supra_juiz.py:71` e `app/ponto/supra_juiz.py:496` | Usa o mesmo juiz no ramo `COBRANCA_EM_DIA_SEM_TRABALHO`, alcançado com `cobertura_status is None`. `app/ponto/supra_juiz.py:494`, `app/ponto/supra_juiz.py:497`. |
| `app/chamados/models.py:1245` | A parcial não regulariza pelo rótulo ausência; continuam as demais provas do coletor. `app/chamados/juizes.py:426`, `app/chamados/models.py:1247`. |

Contraexemplos de código, sem afirmar ocorrência produtiva:

1. **Parcial COM período, todos os marcos faltantes dispensados, sem órfãs:** A retorna `False` — não cala por ausência — porque não explica o dia inteiro e o período declarado não é `sem_periodo`. A grade pode dispensar todos os marcos missing dentro do período; estes viram lâmpadas acesas, e B retorna `False` — sem furo. `app/chamados/services/materializacao.py:81`, `app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2641`, `app/escala/utils.py:1265`, `app/escala/utils.py:1269`, `app/ponto/services/cartorio.py:647`, `app/ponto/services/cartorio.py:677`, `app/chamados/juizes.py:480`, `app/chamados/juizes.py:490`.

2. **Falta aprovada:** A sempre retorna `False`, porque os dois termos excluem falta. B lê a ata: `tipo_dia='falta'` aciona `dia_sem_trabalho`, consulta a véspera e retorna `False` quando a véspera responde `False` ou `None`. No coletor, chamado que não seja `turno_aberto` já fica regularizado por esse tipo de ata antes de B. `app/chamados/services/materializacao.py:81`, `app/chamados/services/materializacao.py:82`, `app/ponto/turnos.py:2675`, `app/chamados/juizes.py:428`, `app/chamados/juizes.py:471`, `app/chamados/juizes.py:472`, `app/chamados/models.py:1245`.

3. **Tipo não ABONA, integral e não falta:** não distingue A de B: ambos calam sem consultar `efeito`. Distingue essa cobrança do helper de folha, que exige `a.efeito == ABONA`. `app/chamados/services/materializacao.py:81`, `app/chamados/juizes.py:449`, `app/ponto/turnos.py:2675`, `app/ponto/turnos.py:2689`.

4. **`para='folha'`:** A fixa cobrança, B aceita o efeito. Cobrança inclui não rejeitadas; folha inclui apenas aprovadas. Portanto podem consultar vencedoras diferentes quando houver mais de um lançamento; desempate é `-data, -pk`. Com uma única ausência aprovada comum aos dois, o filtro de status não constitui diferença. `app/chamados/services/materializacao.py:78`, `app/chamados/juizes.py:448`, `app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2557`.

O registro tem a pergunta mais estreita `_C18`, “a cobrança do dia cala porque a célula lavrada diz que o dia não é de trabalho?”, apontando para `dia_sem_trabalho`; não é um registro literal de A/B como juiz único da composição ausência → silêncio. `app/core/juizes.py:149`, `app/core/juizes.py:278`.

## 08/11 10:20 · sessao d3bca936a7a8 · - · - · origem=Ronald (tmux)

Censo auxiliar, só leitura:

| Sítio executável | Resposta para parcial não-falta sem horas | Efeito |
|---|---|---|
| `app/ponto/turnos.py:2598` | `cobre_parte_do_dia=True` se tipo aceita horas, registro de um dia, `0<minutos<480`; horas não participam. | Predicado de parcial. |
| `app/ponto/turnos.py:2632` | Retorna `sem_periodo`. | Período declarado. |
| `app/ponto/turnos.py:2675` | `ausencia_explica_o_dia_inteiro=False`. | Cobertura inteira. |
| `app/ponto/turnos.py:2690` | Excluída de dias inteiros; além disso exige efeito `ABONA`. | Folha/calculador, zera pontualidade apenas para inteiro. |
| `app/escala/servico_jornada.py:344` | Parcial não cala esse guarda; continua classificação. Varre todas as ausências, não somente vencedora (`:342`). | `classificar_falta`, furo. |
| `app/ponto/services/fechamento.py:694` | Parcial de `TIPOS_PAGOS` pula abono inteiro; dia conta pelas batidas (`:695`). | Folha. |
| `app/ponto/services/efeito_ausencia.py:48` | Soma minutos pagos quando dentro do período. | Crédito financeiro. |
| `app/ponto/services/efeito_ausencia.py:55` | Parcial paga não justifica nem suprime dia inteiro (`:56`). | Folha/espelho. |
| `app/ponto/services/credito_parcial.py:27` | Minutos pagos viram crédito do próprio dia (`:28`). | Folha/espelho. |
| `app/ponto/services/dia_decidido.py:494` | Parcial abonada conserva minutos no rótulo (`:496`). | Tela. |
| `app/ponto/services/ausencia.py:887` | Permite período somente para parcial; quando este validador recebe hora faltante, rejeita (`:883-884`). | Cadastro. |
| `app/ponto/services/vigia_ausencia_veredito.py:54` | Pula parcial no contador de discordância. | Vigia. |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` | Pula parcial no contador. | Diagnóstico. |
| `app/ponto/views.py:2084` | Entrega `True` ao drawer. | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:296` | Mostra edição de período aprovado; sem início mostra “dia está na lista do DP” (`:299`). | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:311` | Mostra período na aprovação, com aviso “sem hora [...] lista do DP” (`:313`). | Tela. |
| `app/ponto/views.py:2240` | Marca `a.cobre_parte=True`. | Lista de ausências. |
| `app/templates/ponto/ausencias.html:163` | Exibe minutos da parcial. | Tela. |
| `app/escala/services/leitor_celula.py:416` | Transporta `cobertura_parcial`; transporta literalmente `periodo_parcial` da ata (`:418-419`). | Alimentação dos leitores de ata. |
| `app/chamados/services/declaracao_texto.py:98` | Parcial entra mesmo com `tipo_dia='ausencia'`; conserva marcos `missing` (`:110-111`). | Texto/declarativa de cobrança. |
| `app/escala/services/furos_diarios.py:49` | Parcial entra na conta, inclusive sem período. Conta `missing` (`:54`, `:21`). | `FuroDiario`, métricas/ranking. |
| `app/relatorios/furos.py:195` | Parcial entra; marcos apagados vão ao resultado (`:202-209`). | Raio-X da folha/tela. |
| `app/inteligencia/calculadores.py:50` | Parcial entra; marcos apagados classificam completo/parcial/furo (`:64-70`). | Métrica `preenchimento_espelho`. |
| `app/inteligencia/calculadores.py:149` | Parcial entra; conta previstos e batidos (`:160-161`). | Métrica de preenchimento de marcos. |

Os chamadores indiretos de `dias_com_ausencia_de_dia_inteiro` também excluem a parcial: `app/ponto/services/fechamento.py:687`, `app/ponto/services/efeito_ausencia.py:65`, `app/ponto/turnos.py:2695`, `app/ponto/management/commands/diff_calculador.py:412`, `app/ponto/services/esmeril_espelho.py:164`, `app/relatorios/management/commands/diff_extrato_parcial.py:67`.

`supra_juiz` tem três respostas distintas comprovadas:

- Com fato e turno encerrado, lê `periodo_parcial`, produz `PARCIAL_SEM_PERIODO`, cobra cadastro: `app/ponto/supra_juiz.py:381-389`.
- Com batida sem par, realizado zero, lê o mesmo campo e produz a mesma acusação: `app/ponto/supra_juiz.py:437-442`.
- Sem batida, pode produzir `FURO_SEM_COBRANCA`: a parcial é dia de cobrança (`:304`), a ausência de período não barra o portão (`:313-316`), e o furo é emitido no resultado (`:367-370`). O cartório barra depois o nascimento por `parcial_so_lista`, não pelo classificador (`app/ponto/services/cartorio.py:1706`, `:1723-1725`).
- Com chamado vivo, parcial sem período não satisfaz `_resto_cobrado`, portanto produz `COBRANCA_EM_DIA_COBERTO`: `app/ponto/supra_juiz.py:530-534`.
- `PARCIAL_SEM_PERIODO` deriva veredito `indefinida`: `app/escala/models.py:1615`.

Guardas adicionais da lista do administrador:

| Sítio | Efeito comprovado |
|---|---|
| `app/ponto/services/cartorio.py:398` | Sem período + cobertura parcial retorna `True` incondicionalmente (`:399`). |
| `app/ponto/services/cartorio.py:1723` | Conta na lista e bloqueia emissão do furo do dia (`:1725`). |
| `app/ponto/services/cartorio.py:1775` | Conta na lista e bloqueia nascimento do chamado de furo parcial (`:1777`). |
| `app/ponto/services/cartorio.py:1832` | Incrementa `parcial_na_lista_sem_push`, deduplicado por dia-colaborador (`:1829-1831`). |
| `app/chamados/services/fabrica_da_celula.py:102` | Retorna sem motivos/pergunta/pai/push (`:104`). |
| `app/chamados/services/disputa_emissao.py:871` | `dia_na_lista_do_admin` delega à ata e ao predicado do cartório. |
| `app/chamados/services/disputa_emissao.py:894` | Remove chamados desses dias antes da fábrica de disputas; lista vazia retorna `None` (`:895-896`). |
| `app/chamados/services/pergunta_no_app.py:88` | Retenção retorna `parcial_na_lista_do_admin` (`:89`), antes de consultar A (`:90`). |
| `app/chamados/reconciliador.py:872` | Fora de costura em chamado já vivo (`:865`), barra nascer/renascer (`:875`). |
| `app/ponto/services/cluster_espurio.py:103` | Põe dia em `na_lista`; remove-o de `a_abrir` (`:107`); fechamento continua calculado separadamente (`:108`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154` | Incrementa barra `parcial_lista`, não pergunta (`:155-156`). |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121` | Incrementa `na_lista`, não emite (`:122-123`). |

Registro de juízes:

- Há registro expresso para a pergunta **“a ausência do dia suspende, fecha ou levanta a cobrança viva?”**: `_C11`, `app/core/juizes.py:117`, autoridade `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia`, `app/core/juizes.py:271`.
- C11 não chama A nem B: seleciona ausência vencedora para cobrança (`app/chamados/services/silencio_ausencia.py:65`); aprovada só fecha se `_premissa_morre` (`:93-94`), que exige veredito fora de `VEREDITOS_ACUSAM` (`:40-41`), com exceção `justificativa` (`:34-35`); fechamento efetivo passa por `ch.reconciliar()` (`:101`).
- Há C18 para **“a cobrança do dia cala porque a célula lavrada diz que o dia não é de trabalho?”**, `app/core/juizes.py:149`, autoridade `dia_sem_trabalho`, `:278`.
- O registro de ausência contém a pergunta de cobertura de dias (`app/core/juizes.py:241`) e a pergunta de período (`:249`); não há ali registro de A ou B como composição única para toda cobrança.
- `app/docs/JUIZES.md:59-67` documenta a autoridade `dia_sem_trabalho`, a exceção parcial que cobra resto, e a regra especial de A para `sem_periodo`.

Exemplo adicional relevante: para efeito de folha, ausência inteira não `ABONA` é excluída da dispensa de pontualidade por `app/ponto/turnos.py:2690`, embora `ausencia_explica_o_dia_inteiro` não teste efeito (`:2675`). Isso é diferença entre domínios/composições, não prova isolada de divergência A/B.

## 08/11 10:20 · sessao 4d73f0df7961 · - · - · origem=Ronald (tmux)

Grade e ata:

- `app/escala/utils.py:1209` consulta cobertura `para='folha'`; `:1210-1212` calcula a parcialidade por `cobre_parte_do_dia`. Para não-falta marca `tipo_dia='ausencia'` (`:1241`) e `cobertura_parcial=True` (`:1251`); chama `periodo_da_parcial_no_dia` (`:1255`). Se `sem_periodo`, não entra no ramo de dispensa (`:1265`): os missing permanecem. A limpeza de todas as células só ocorre em `not _parcial` (`:1272-1274`).
- `app/ponto/services/cartorio.py:647` faz `_acesa = isinstance(x, dict) and not x.get('missing')`; grava em `:677-679`. Sem DNA correspondente, o fallback também grava `acesa = not missing` (`:687-690`). Portanto os marcos missing conhecidos permanecem `acesa=False`; não significa que todas as lâmpadas do dia ficam falsas, pois batidas presentes acendem as respectivas lâmpadas (`:618-620`, `:647`).
- Há exceção de lâmpada sem coluna atribuível com fato cego: `acesa=None` (`app/ponto/services/cartorio.py:641-644`). Não transformar a afirmação sobre missing conhecidos numa garantia universal sobre todas as lâmpadas.
- `ata_do_dia` transporta tipo do dia e lâmpadas (`app/ponto/services/cartorio.py:701-705`), conta false em `n_missing` (`:712-713`), escreve `cobertura_parcial=True` (`:732-733`) e `periodo_parcial='sem_periodo'` (`:736-737`).
- `dispensar_por_ausencia` só aceita ausência aprovada SEM `dia['cobertura_parcial']` (`app/ponto/services/cartorio.py:766-769`); parcial sem período sai intacta. Se também houver `dia['justificado']`, existe outra fonte de dispensa (`:768`), que acende e marca `dispensada` (`:770-776`).
- Uso da montagem/dispensa de ata em produção: `app/ponto/services/cartorio.py:1210`, `:1386`, `:1800-1802`; o último lavra via `cel.lavrar_veredito`.
- `app/escala/services/leitor_celula.py:416-419` só transporta os dois carimbos da ata para o dia; não altera sem_periodo nem julga novamente.

Portão comum de emissão:

- `app/ponto/services/cartorio.py:370` define `parcial_so_lista`; sem `cobertura_parcial`, False (`:396-397`); com `periodo_parcial == 'sem_periodo'`, True incondicional (`:398-399`). Para parcial com período, True depende de julgamento anterior e ausência de fato novo (`:400`).
- Cartório calcula uma vez em `app/ponto/services/cartorio.py:1706-1707`; furo sem batida só chama emissor quando `apply_ and not _parcial_so_lista` (`:1725`, chamada `:1740-1742`); furo parcial só emite quando `apply_ and not vivos and not _parcial_so_lista` (`:1777-1780`). Logo não cria chamado de furo para sem_período que tenha os carimbos.
- `app/ponto/services/cartorio.py:1723-1724` e `:1775-1776` chamam `_contar_na_lista`. Este apenas incrementa contador em memória `parcial_na_lista_sem_push` e guarda chave em conjunto (`:1827-1832`): não escreve linha de lista.
- Chamado vivo pode continuar e ter marcos espelhados: `app/ponto/services/cartorio.py:1781-1789`, `:1790-1798`. O bloqueio de nascimento não é um fecho universal dos vivos.

Todos chamadores executáveis de `parcial_so_lista`, fora tests:

| Sítio | Efeito para parcial sem período |
|---|---|
| `app/ponto/services/cartorio.py:1706` | Cala os dois portões de novo chamado, `:1725`, `:1777`. |
| `app/chamados/services/fabrica_da_celula.py:102-104` | `motivos_a_perguntar` retorna `[]`: sem pergunta/pai/push; há guardas anteriores por dia/estado em `:91-100`. |
| `app/chamados/services/disputa_emissao.py:871-873` | `dia_na_lista_do_admin` retorna o predicado; sem dia/célula retorna False (`:864-870`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154-156` | Conta `parcial_lista`, pula nova pergunta do chamado mudo. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121-123` | Conta `na_lista`, pula emissão retroativa. |
| `app/ponto/services/cluster_espurio.py:102-107` | Exclui dias da lista de `a_abrir`; `reconciliar_colab` pula `abrir` em `:127-130`; `a_fechar` é calculado separadamente (`:108`), não pelo predicado. |

Leitores de `dia_na_lista_do_admin`:

- `app/chamados/services/disputa_emissao.py:894-896`: filtra chamados órfãos antes de criar disputa/pergunta; chamado existente não é fechado pela filtragem.
- `app/chamados/services/pergunta_no_app.py:88-91`: retorna causa `parcial_na_lista_do_admin` ANTES da causa `coberto_por_ausencia`. O rótulo e dono `'admin'` ficam em `:26-27`. `medir` transforma retenções de chamados existentes em itens (`:126-144`) e contagens (`:145-149`). O command `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14-21` imprime as contagens.

Supra:

- `app/ponto/supra_juiz.py:381-389`: com fato no dia encerrado + aprovado + sem_periodo, adiciona `PARCIAL_SEM_PERIODO`, texto “cobrar o CADASTRO”.
- Segundo ramo de batida sem par usa o mesmo código em `app/ponto/supra_juiz.py:437-442`.
- O dia sem fato pode seguir sendo FURO: a parcial sem período não é dia inteiro (`app/ponto/supra_juiz.py:268-269`), não ganha dispensa que satisfaça `_parcial_sem_resto` (`:311-312`), e o ramo de furo o alcança em `:313-316`. O cartório posteriormente cala a emissão por `parcial_so_lista`.
- `app/ponto/supra_juiz.py:530-534`: `_resto_cobrado` exige período diferente de sem_periodo; aprovado + chamado vivo sem esse resto produz `COBRANCA_EM_DIA_COBERTO`.
- `app/escala/models.py:1615` mapeia `PARCIAL_SEM_PERIODO` para `indefinida`; a ordem do mapa dá precedência a furo, cobrança indevida e coberta (`:1612-1615`).

Destino “lista do admin”, distinção comprovada:

- Existe promessa literal nos comentários de `app/ponto/services/cartorio.py:379-382` e texto da tela `app/templates/ponto/partials/painel_ausencia.html:296-299`: se parcial aprovada e sem `hora_ini`, mostra “sem periodo: o dia esta na lista do DP”.
- A lista real é `app/escala/views.py:484-492`, chamando `cadastro_realidade.lista`.
- Ela agrega explicitamente fontes de C1–C12 (`app/escala/services/cadastro_realidade.py:800-879`) e as A lavradas (`:886-887`, apresentação `:936-945`). C11 seleciona `veredito='indefinida'` mas **exige** protesto `PREVISTO_COM_DIA_INDEFINIDO` (`:477-478`), não `PARCIAL_SEM_PERIODO`.
- Não localizei publicador específico que coloque o código/dia `PARCIAL_SEM_PERIODO` nessa lista real. Presença efetiva por essa condição: **[não verificado]**. Bloqueio da emissão e contagem `parcial_na_lista_sem_push` estão comprovados, mas não são publicação de linha. [nao verificado]
- Existe caminho genérico A9 para ausência + pelo menos 2 batidas (`app/ponto/services/esmeril_espelho.py:133-134`), sem distinguir sem_período; é rotulado “juiz” (`:23`). Não garante que cada parcial sem período apareça na lista.

## 08/11 09:49 · sessao a44dd9306e29 · ATA-X-TURNO-PAUSA · verificador · origem=Code

**O delta corrige os achados 1 e 2 no caminho novo, mas não sustenta equivalência universal entre `marcos_do_dia` e `pausas_do_dia`. Há consulta adicional no caminho do motor e lacunas nos testes novos.** A comparação contra o main abaixo usa o diff integral fornecido; checkout histórico, execução dos testes e ocorrência em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1705–1754`; `app/ponto/services/cxr_na_lista.py:32–49`; `app/escala/services/cadastro_realidade.py:397–400`)

**A. Correção e contraexemplos**

- **Achado 1: corrigido.** Cada pausa real precisa estar longe, nas duas pontas, de **todas** as declaradas. Basta uma declarada descrevê-la para não acusar. O relato escolhe a menor soma das distâncias absolutas; o corte permanece estritamente `>180`, nas duas pontas. (`app/ponto/motor_calculo_v2.py:1705–1710`, `:2027–2030`)
- **Achado 2: corrigido para uma nova lavra.** Os quatro campos entram na allowlist e C12 possui ramo próprio. A leitura não recupera campos descartados de snapshots anteriores: `ultimo_lavrado` apenas devolve o detalhe persistido. Nova lavra em produção: [nao verificado]. (`app/ponto/services/cxr_na_lista.py:32–34`, `:91`, `:147–166`; `app/escala/services/cadastro_realidade.py:397–400`)
- **Dono sem `pausas_do_dia`: agora silencia.** Não há fallback para `hii/hfi`. Existem entradas desse formato nos testes: `_TE`, passado como template, e `_Vinculo`, passado como `escala_colaborador`. Logo, existe mudança no contrato aceito pelo motor; dono real de produção sem a irmã: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1733–1735`; `app/ponto/tests/test_atraso_cross_midnight.py:8–26`, `:38–48`; `app/ponto/tests/test_motor_pareia_pelo_dna.py:48–60`, `:74–76`)
- **Template versus célula:** com célula existente e `dna.marcos` vazio/nulo, `marcos_do_dia` pode oferecer pausa do template, mas `pausas_do_dia` responde vazio. O delta deixa de acusar nesse estado; respeita a regra já existente de pausa declarada pela célula. (`app/escala/models.py:1124–1133`, `:1138–1158`, `:1189–1208`; `app/ponto/motor_calculo_v2.py:1742–1747`)
- **Noturno com duas pausas:** no exemplo construído `19–07`, pausas `22–23` e `02–03`, o helper ancora a primeira em D e a segunda em D+1, conservando `dia=D` no relato. Essa construção coincide com a do produtor de lâmpadas; o teste novo de duas pausas não exercita esse caso noturno. (`app/ponto/motor_calculo_v2.py:1749–1754`; `app/ponto/management/commands/gerar_celulas.py:55–69`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:56–88`)
- **Ordem do cadastro:** não altera a existência da acusação, pois o predicado consulta todas. Em empate da distância mínima, vence a primeira declarada encontrada; portanto pode alterar o par publicado. A fonte preserva ordem cadastral, sem ordenar por relógio. (`app/ponto/motor_calculo_v2.py:1708–1710`; `app/escala/models.py:290–307`)
- **Dedupe por dia:** conserva somente a primeira pausa acusável daquele dia. Duas pausas divergentes distintas não produzem duas linhas. Isso é explícito e reinicia a cada cálculo; a lavra não consegue recuperar a segunda. (`app/ponto/motor_calculo_v2.py:1695`, `:1711–1723`, `:2364–2365`)

**B. Autoridade dos métodos e DNA**

O seletor usado pela acusação escolhe `escala_colaborador.marcos_do_dia`; na ausência desse método, escolhe `tipo_escala` ou `self.escala`. Nas duas classes reais examinadas, ambos os métodos existem. (`app/ponto/motor_calculo_v2.py:2768–2772`; `app/escala/models.py:281`, `:326`, `:1098`, `:1166`)

| Dono/estado | Relação entre primeira pausa e marcos |
|---|---|
| `TipoEscala` | A primeira pausa vem literalmente de `marcos_do_dia`. Override impede acrescentar a segunda do modelo; modo duração remove pausa fixa. (`app/escala/models.py:304–308`, `:335–345`) |
| EC com DNA contendo primeiro par completo | Ambos leem `hii/hfi` do mesmo DNA congelado. (`app/escala/models.py:1124–1132`, `:1189–1201`) |
| EC sem célula | Ambos recorrem ao template. (`app/escala/models.py:1133`, `:1208`) |
| EC com célula e marcos vazios/nulos | **Divergem:** marcos cai no template; pausas retorna vazio. (`app/escala/models.py:1124–1133`, `:1202–1208`) |
| EC com primeiro par incompleto/ausente e segundo completo | Marcos devolve o primeiro par literal; pausas elimina pares incompletos e pode devolver o segundo como `pausas[0]`. O produtor normal não gera sozinho essa configuração; ocorrência em produção: [nao verificado]. (`app/escala/models.py:1131–1132`, `:1196–1201`; `app/ponto/management/commands/gerar_celulas.py:81–93`) |

Portanto, **“mesmo dono” não implica “mesma resposta no par 1 em todo estado”**. A divergência da célula sem marcos é anterior ao delta e está explicada no próprio leitor de intervalo. (`app/escala/models.py:1138–1164`)

| Campo do DNA | Quem escreve | Quem lê |
|---|---|---|
| `hii/hfi` | `montar_dna`, a partir de `te.marcos_do_dia`. (`app/ponto/management/commands/gerar_celulas.py:81–84`) | EC.marcos, EC.pausas e mapa de marcos do período. (`app/escala/models.py:1131–1132`, `:1197–1200`; `app/escala/utils.py:103–107`) |
| `hii2/hfi2` | `montar_dna`, a partir de `pausas[1:]`. (`app/ponto/management/commands/gerar_celulas.py:92–93`) | EC.pausas e mapas de pausas do período. (`app/escala/models.py:1197–1200`; `app/escala/utils.py:57–61`, `:141–145`) |
| Todos os pares | Vocabulário e ordem em `CelulaDia.PARES_PAUSA_DNA`/`chaves_do_turno`; lâmpadas derivadas pelo gerador. (`app/escala/models.py:1652–1668`; `app/ponto/management/commands/gerar_celulas.py:60–69`) | Juiz da batida, cartório e papel da ata. (`app/ponto/juiz_batida.py:166–176`; `app/ponto/services/cartorio.py:581–586`; `app/ponto/turnos.py:1586–1587`, `:1711–1712`) |

A persistência do blob passa pela porta: geração/regeneração, edição, enriquecimento/geração e reversão que restaura `dna_anterior`. (`app/ponto/portas/celula.py:244–276`, `:591–609`, `:783–794`, `:848–870`)

**C. Quatro campos, `_txt`, dedupe e leitores**

| Campo | Quem escreve | Quem lê diretamente |
|---|---|---|
| `pausa_real` | Acusador: lista com dois instantes ISO localizados. (`app/ponto/motor_calculo_v2.py:1721`) | Allowlist/`medir`, igualdade do item e C12. (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397–399`) |
| `pausa_cadastrada` | Acusador: lista ISO do par escolhido pela menor distância. (`app/ponto/motor_calculo_v2.py:1710`, `:1722`) | Mesmos leitores. (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397–399`) |
| `delta_ida_min` | `(real−prevista)/60`, arredondado a uma casa. (`app/ponto/motor_calculo_v2.py:1705`, `:1723`) | Allowlist/`medir`, igualdade e texto C12. (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:399`) |
| `delta_volta_min` | Mesmo sentido e arredondamento. (`app/ponto/motor_calculo_v2.py:1706`, `:1723`) | Mesmos leitores. (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:400`) |

`_txt` alcança **todos os 14 campos da allowlist**, não somente as pausas. Agora qualquer lista/tupla vira texto unido por ` -> `; antes virava `str(v)`. Números permanecem números, inclusive `bool`; `None` permanece `None`; os demais tipos viram texto. No fluxo normal, valores `None` já são excluídos antes da chamada. (`app/ponto/services/cxr_na_lista.py:32–49`, `:91`)

Os produtores atuais dos campos antigos escrevem strings/números; os dois pares novos são listas. Assim, a mudança concreta de representação está nos pares: **lista no resultado do motor, string no snapshot**. Os deltas conservam representação numérica. (`app/ponto/motor_calculo_v2.py:1634–1639`, `:1682–1684`, `:1721–1723`, `:2073`; `app/ponto/services/cxr_na_lista.py:46–49`)

O `it in _do_colab` compara o item completo **depois** da conversão e do enriquecimento. Os quatro campos passam a distinguir relatos de pausas diferentes que antes poderiam colapsar. Não é dedupe somente por dia/causa; o dedupe por dia ocorre antes, dentro do motor. (`app/ponto/services/cxr_na_lista.py:89–112`; `app/ponto/motor_calculo_v2.py:1714–1723`)

A cadeia persistente é `medir → lavrar → MetricaSnapshot.detalhe → ultimo_lavrado → C12 → detalhe na lista/HTML/PDF`. A tela e o PDF recebem o texto formatado, sem interpretar os quatro valores separadamente. (`app/ponto/services/cxr_na_lista.py:147–166`; `app/escala/services/cadastro_realidade.py:397–409`, `:875–876`, `:930–932`, `:1110`; `app/templates/escala/cadastro_x_realidade.html:28`)

Outros leitores usam o envelope: fechamento concatena a lista; oráculo lê `dia`; `cxr_lavrado` lê identificação/data; relato de janela e resumo do espelho filtram causas próprias; `diff_janela_he` exclui expressamente `l084_pausa`. (`app/ponto/services/fechamento.py:96–97`, `:294–296`; `app/ponto/management/commands/e6_oraculo.py:170–171`; `app/ponto/services/cxr_na_lista.py:179–189`; `app/ponto/janela_he.py:277–282`; `app/ponto/services/espelho.py:1259–1261`; `app/ponto/management/commands/diff_janela_he.py:72–73`)

**D. Consultas no fechamento/lote**

**Sim: cada nova chamada elegível a `_turnos_do_juiz` acrescenta um SELECT de vínculos.** Ela fornece células e batidas, mas omite `escalas=`. O papel então chama `_escalas_da_ata`, com `select_related('tipo_escala')`: uma consulta, mesmo que depois nenhuma ata produza papel. O comentário “ALIMENTACAO, NUNCA CONSULTA” não descreve esse caminho. (`app/ponto/motor_calculo_v2.py:884–893`; `app/ponto/turnos.py:1561–1564`, `:1595–1597`, `:1634`)

O sítio antigo também omite `escalas=` e já pagava uma consulta. Nesta cópia, o nome da função é `turnos_via_autoridade`; `_parear_motor` aparece no comentário. (`app/ponto/motor_calculo_v2.py:453`, `:494–502`, `:885`)

Contando **somente esses SELECTs de escala para o papel**, em cálculo normal com batidas, vínculo e células elegíveis:

| Motor | Main pelo diff fornecido | Obra | Consultas acrescentadas |
|---|---:|---:|---:|
| Base/12x36 | 1 | 2 | +1. (`app/ponto/motor_calculo_v2.py:502`, `:2428`) |
| TurnoPartido | 1 | 3 | +2: chamada no `super` e novamente no override. (`app/ponto/motor_calculo_v2.py:2995`, `:3008`) |
| Comercial | 1 | 3 | +2: chamada no `super` e novamente via `_dia_do_turno`. (`app/ponto/motor_calculo_v2.py:3401`, `:3412`, `:898–899`) |

Sem as guardas `cid`, mapa alimentado e células desse colaborador, o sítio novo acrescenta zero. Isso não representa o total de consultas de `calcular_mes`. (`app/ponto/motor_calculo_v2.py:888–893`)

Os caminhos atingidos são:

- **Fechamento por fatia:** alimentação em `:750`, apuração em `:754`, cálculo por fatia em `:286`. (`app/ponto/services/fechamento.py:750–755`, `:278–291`)
- **Passada órfã:** alimentação em `:825`, cálculo com `ec=None` e `colaborador_id` em `:827–829`. O sítio novo pode consultar; o antigo depende de `_ec.colaborador_id` e não entra nesse ramo. (`app/ponto/services/fechamento.py:825–829`; `app/ponto/motor_calculo_v2.py:469`, `:496`, `:876`)
- **Espelho:** alimentação, fatias ou cálculo direto; **PDF:** chama o espelho. (`app/ponto/services/espelho.py:1131`, `:1164`, `:1179`, `:1227`; `app/relatorios/pdf_espelho.py:596–598`)

Há ainda custo específico do **delta v2**: fora do contexto de alimentação, cada turno passa a consultar célula duas vezes — marcos, já existente na v1, e pausas, acrescentada agora. Dentro do contexto e da janela, ambos usam o mapa. Turnos do pad fora da janela podem consultar, pois o helper não filtra `data_turno` pelo período pedido. (`app/escala/models.py:1048–1067`, `:1117–1123`, `:1182–1188`; `app/ponto/services/fechamento.py:278–291`; `app/ponto/motor_calculo_v2.py:1738–1754`)

Contagem executada e impacto de latência no lote: [nao verificado].

**E. Universo deduplicado: anterior à obra**

**Sim.** O fluxo normal limpa as batidas antes de `montar_periodos`; as implementações chegam a `_periodos_via_autoridade`, que chama o sítio antigo com essas batidas. Portanto, o papel do pareamento monetário já recebia universo deduplicado. (`app/ponto/motor_calculo_v2.py:2433–2436`, `:2835`, `:2962`, `:3300`, `:3619`, `:530`, `:502`)

`turnos_do_colab` carrega batidas vivas cruas e entrega essa lista ao papel. O papel conta por minuto e exige exatamente uma batida em cada luz. (`app/ponto/turnos.py:1866–1869`, `:1918`, `:1942`, `:1615–1620`, `:1659`)

Contraexemplo estático: duas entradas consecutivas `08:00:01 E` e `08:00:20 E`, no minuto de uma luz. O motor elimina a segunda; o papel sobre o universo cru encontra duas e rejeita a emissão daquele dia. Ocorrência desse exemplo em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:736`, `:914–923`; `app/ponto/turnos.py:1659`)

Pelo diff integral fornecido, **essa divergência entre motor monetário e espelho é anterior**; a obra a estende ao novo consumo de papel em `_turnos_do_juiz`. (`app/ponto/motor_calculo_v2.py:502`, `:882–893`; `app/ponto/turnos.py:1942`)

**F. Checklist e força dos testes** [nao verificado]

| Classe | Resultado da leitura |
|---|---|
| Segundo juiz registrado | O par real continua vindo de `intervalos_do_turno`, e o corte reutiliza `_cadastro_nao_descreve`; são autoridades já registradas. O delta não repareia pausas reais. (`app/core/juizes.py:202`; `app/ponto/motor_calculo_v2.py:1708`, `:1753`; `app/ponto/turnos.py:540–544`) |
| Segundo escritor fora da porta | A acusação escreve memória; CXR continua persistindo pelo `lavrar` existente. A reconciliação da grade altera células/orfas em memória; o cartório entrega a ata à porta de lavra. (`app/ponto/motor_calculo_v2.py:1720–1723`; `app/ponto/services/cxr_na_lista.py:147–153`; `app/escala/utils.py:2131–2133`; `app/ponto/services/cartorio.py:1800–1802`) |
| Relógio fora da borda | O delta usa datas/instantes recebidos. A lavra continua recebendo `agora` do command, que lê o relógio na borda. (`app/ponto/motor_calculo_v2.py:1749–1754`; `app/ponto/services/cxr_na_lista.py:52–65`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30–40`) |
| Derivação paralela | Pausas declaradas vêm da irmã do dono escolhido. A conversão hora→instante permanece local e reproduz a regra do produtor; para duas pausas noturnas bem formadas, coincide. A equivalência universal das fontes, porém, é falsa pelos estados de B. (`app/ponto/motor_calculo_v2.py:1733–1752`; `app/ponto/management/commands/gerar_celulas.py:55–69`; `app/escala/models.py:1202–1208`) |
| Ordem de trava | Os trechos novos não introduzem transação nem aquisição explícita de trava; a ordem de publicação continua acusação → teto de pontualidade → resultado. Verificação concorrente em execução: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1720–1754`, `:2802–2806`; `app/ponto/services/cxr_na_lista.py:147–153`) |
| Campo com dois significados | `dias_cadastro_x_realidade` inclui pausa, que não significa dia retirado da janela. O instrumento foi ajustado para essa diferença; os pares também mudam de lista para string ao atravessar a lavra. (`app/ponto/management/commands/diff_janela_he.py:70–73`; `app/ponto/motor_calculo_v2.py:1721–1723`; `app/ponto/services/cxr_na_lista.py:46–49`) |

Os testes novos **mordem a volta do comparador contra apenas a primeira pausa**, a retirada dos campos da allowlist e a retirada do ramo C12. Entretanto, o caso de duas pausas usa dono fake e intervalos mockados; o teste C12 monta o item manualmente e mocka `ultimo_lavrado`. (`app/ponto/tests/test_ata_pausa_l084_cxr.py:56–88`, `:96–117`)

Eles **não mordem retirar o ramo lista/tupla de `_txt`**: `str(lista_de_ISO)` ainda contém todos os horários exigidos. Tampouco exigem valores/sinais dos deltas ou atravessam `medir → lavrar → snapshot`; não cobrem o helper com EC real, célula vazia, dono sem irmã ou duas pausas noturnas. (`app/ponto/tests/test_ata_pausa_l084_cxr.py:72–88`, `:106–117`)

O teste de reset morde o reaproveitamento do dedupe, mas o teste de dedupe repete a mesma pausa; não demonstra qual relato sobrevive quando há duas divergências distintas no mesmo dia. (`app/escala/tests/test_ata_x_turno_pausa.py:332–350`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:48–53`)

Finalmente, **“só acusa” vale para o acusador, não prova imobilidade financeira da obra inteira**: a nova alimentação de papel altera o caminho que produz o mapa do dia antes de `montar_periodos`. Ausência de diferença monetária nesta cópia: [nao verificado]. (`app/ponto/motor_calculo_v2.py:893–894`, `:2428–2436`, `:1720–1724`)

O restante do estado lido/escrito pelo delta fica assim:

| Valor/estado | Quem escreve | Quem lê |
|---|---|---|
| `_turnos_juiz_calc`, `entrada`, `data_turno` | Pareador produz turnos; `calcular_mes` guarda a resposta. (`app/ponto/turnos.py:321`; `app/ponto/motor_calculo_v2.py:2428`, `:2478`) | Helper filtra turnos e usa o dia explícito. (`app/ponto/motor_calculo_v2.py:1738–1754`) |
| `ida/volta` reais | `intervalos_do_turno`. (`app/ponto/turnos.py:543–544`) | Helper e acusador. (`app/ponto/motor_calculo_v2.py:1753–1754`, `:1705–1706`) |
| `hi/hf`, pausas declaradas | Métodos do dono descritos em B. (`app/escala/models.py:304–345`, `:1124–1133`, `:1189–1208`) | Helper ancora `previstas`; acusador compara e escolhe o par. (`app/ponto/motor_calculo_v2.py:1742–1752`, `:1705–1710`) |
| `dia`, `causa='l084_pausa'` | Acusador. (`app/ponto/motor_calculo_v2.py:1720`) | Lavra, C12, oráculo e filtro do instrumento. (`app/ponto/services/cxr_na_lista.py:91–107`; `app/escala/services/cadastro_realidade.py:399–409`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/management/commands/diff_janela_he.py:73`) |
| `_l084_pausa_acusada` | Reset no cálculo; inclusão no acusador. (`app/ponto/motor_calculo_v2.py:2365`, `:1716`) | Guarda por dia. (`app/ponto/motor_calculo_v2.py:1714`) |
| `_cadastro_x_realidade` | Reset e append do acusador. (`app/ponto/motor_calculo_v2.py:2364`, `:1720–1723`) | Publicação no resultado e leitores de C. (`app/ponto/motor_calculo_v2.py:2806`; `app/ponto/services/cxr_na_lista.py:84–91`) |
| Campos antigos atravessados por `_txt` | Produtores de janela e L-084 do dia. (`app/ponto/motor_calculo_v2.py:1634–1639`, `:1682–1684`, `:2073`) | Allowlist, conversão, igualdade e ramos antigos de C12. (`app/ponto/services/cxr_na_lista.py:32–49`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:394–404`) |

## 08/11 09:46 · sessao 64acf49b617b · - · - · origem=Ronald (tmux)

A/B — conclusão sustentada pelo código:

- O delta cura o falso positivo da segunda pausa: cada pausa real agora precisa divergir de **todas** as declaradas; uma declarada que descreva ambas as pontas impede a acusação. A linha escolhe menor soma das distâncias absolutas. `app/ponto/motor_calculo_v2.py:1705-1710`.
- Dia obrigatório elimina fallback pelo instante da saída; grava diretamente o dia do turno recebido. `app/ponto/motor_calculo_v2.py:1688`, `:1720`, `:1754`.
- EC, tipo_escala e self.escala são as três opções do seletor; a prioridade é EC, depois tipo_escala ou self.escala. `app/ponto/motor_calculo_v2.py:2768-2772`.
- Ambas classes reais têm os dois métodos. No template, a primeira pausa vem literalmente de `marcos_do_dia`; override impede soma da segunda pausa e modo duração impede pausa fixa. `app/escala/models.py:281-308`, `:326-345`.
- **A afirmação de mesma autoridade em todo estado é falsa:** EC.marcos cai no template se célula existe com DNA/marcos nulo/vazio; EC.pausas retorna `()` nessa célula. Trata-se de diferença intencional anterior, documentada em `intervalo_do_dia`, e o delta passa a silenciar a acusação nesse estado. `app/escala/models.py:1124-1133`, `:1189-1208`, `:1138-1158`; `app/ponto/motor_calculo_v2.py:1742-1747`.
- Outra diferença possível: DNA com primeiro par ausente/incompleto e segundo completo faz EC.marcos devolver `(None,None)` ou meio par no primeiro, mas EC.pausas comprime completos e devolve a segunda como `pausas[0]`. `app/escala/models.py:1131-1132`, `:1196-1201`. A ocorrência em produção é [nao verificado].
- O produtor normal não cria sozinho essa segunda sem primeira: segundo par é escrito a partir de `pausas[1:]`, e TipoEscala só estende pares extras se o primeiro existir. `app/ponto/management/commands/gerar_celulas.py:81-93`; `app/escala/models.py:304-307`.
- **Dono sem irmã existe na interface exercitada pelos testes:** `_TE` tem só marcos e é passado ao motor/calcular_mes, e `_Vinculo` também tem só marcos e é passado como escala_colaborador. Com o delta esses donos ficam silenciosos; a volta1 ainda compararia hii/hfi. `app/ponto/tests/test_atraso_cross_midnight.py:8-26`, `:38-48`; `app/ponto/tests/test_motor_pareia_pelo_dna.py:48-60`, `:74-76`; guarda nova `app/ponto/motor_calculo_v2.py:1733-1735`. Dono real de produção sem irmã é [nao verificado].
- Noturno com duas pausas 19–07 / 22–23 e 02–03 recebe corretamente D e D+1 pelo `< hi`, mesma construção existente do produtor de lâmpadas. Não achei contraprova para esse caso bem formado. `app/ponto/motor_calculo_v2.py:1749-1752`; `app/ponto/management/commands/gerar_celulas.py:55-69`.
- Ordem cadastral é preservada, não ordenada por relógio; isso mantém identidade do primeiro par. No acusador, ordem só interfere em empate da soma de distâncias, porque `min` conserva primeiro candidato; existência da acusação independe da ordem. `app/escala/models.py:259-260`, `:290-307`; `app/ponto/motor_calculo_v2.py:1708-1710`.
- Dedupe é por dia e guarda apenas a primeira pausa divergente desse dia; duas divergências distintas não viram duas linhas. Reinicia em cada cálculo. `app/ponto/motor_calculo_v2.py:1711-1723`, `:2364-2365`. É redução explícita, não prova de cobertura de todas as pausas divergentes. `app/ponto/motor_calculo_v2.py:1695`.

Custo adicional da autoridade:

- Dentro de `celulas_do_ato`, tanto marcos como pausas leem o mapa, zero queries de célula; contexto cobre os cálculos das fatias. `app/escala/alimentacao.py:24-51`; `app/ponto/services/fechamento.py:277-291`; `app/escala/models.py:1117-1123`, `:1182-1188`.
- Fora do contexto ou fora da janela alimentada, o helper faz duas consultas de célula por turno: uma marcos já existente na v1 e **uma adicional pausas** no delta. Não há memoização por dia. `app/escala/models.py:1048-1067`, `:1122-1123`, `:1187-1188`; `app/ponto/motor_calculo_v2.py:1738-1744`.
- Turnos do pad podem cair fora da janela: helper varre todos sem filtrar data_turno e identidade só cobre ini..fim. `app/ponto/motor_calculo_v2.py:1738-1754`; `app/escala/models.py:1066-1067`.

DNA — escritores/leitores:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| hii/hfi | `montar_dna`, via te.marcos: `app/ponto/management/commands/gerar_celulas.py:81-84` | EC.marcos `app/escala/models.py:1131-1132`; EC.pausas `:1196-1201`; mapas 4-tupla `app/escala/utils.py:103-107` |
| hii2/hfi2 | `montar_dna`, via pausas[1:]: `app/ponto/management/commands/gerar_celulas.py:92-93` | EC.pausas `app/escala/models.py:1196-1201`; mapas de pausas `app/escala/utils.py:57-61`, `:141-145` |
| Todos pares | Vocabulário central: `app/escala/models.py:1652-1668`; lâmpadas derivadas `app/ponto/management/commands/gerar_celulas.py:60-69` | juiz `app/ponto/juiz_batida.py:166-176`; cartório `app/ponto/services/cartorio.py:581-586`; veredito `app/ponto/services/veredito_celula.py:31-38`; papel `app/ponto/turnos.py:1586-1587`, `:1711-1712` |
| Persistência do DNA | Porta gera/regenera `app/ponto/portas/celula.py:244-276`; edição `:591-609`; reversão restaura blob anterior `:783-794`; geração `:852-870` | Métodos/fontes acima |

Cobertura:

- O novo teste de duas pausas morde a volta do comparador contra a primeira, mas usa dono fake e pausa do juiz mockada; não exercita EC/DNA, template real, noturno duplo, dono sem irmã ou célula sem marcos. `app/ponto/tests/test_ata_pausa_l084_cxr.py:56-88`.
- Há selo antigo do produtor para noite com dois pares, mas não é teste do novo helper. `app/escala/tests/test_celula_segundo_intervalo.py:159-170`.
- Não executei testes; aprovação/RED efetivo desta cópia é [nao verificado]. [nao verificado]

## 08/11 09:46 · sessao d2f0642fdd2e · - · - · origem=Ronald (tmux)

D: há consulta nova. Com `cid`, células alimentadas e ao menos uma célula desse colaborador, `_turnos_do_juiz` chama `papel_por_minuto_da_ata` sem `escalas=`; esta chama `_escalas_da_ata`, que executa **um SELECT** com `select_related('tipo_escala')`. O comentário “ALIMENTACAO, NUNCA CONSULTA” não corresponde ao caminho executado. `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:888`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1561`.

A consulta acontece mesmo sem batidas ou com ata ilegível, porque a escala é carregada antes da análise das células. Sem satisfazer as guardas do novo sítio, ele acrescenta zero consultas. `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1616`, `app/ponto/turnos.py:1634`, `app/ponto/motor_calculo_v2.py:888`.

| Caminho elegível | SELECTs novos por `calcular_mes` | Evidência |
|---|---:|---|
| Base/12x36/Plantão | +1 | `app/ponto/motor_calculo_v2.py:2428` |
| TurnoPartido | +2 | `app/ponto/motor_calculo_v2.py:2995`, `app/ponto/motor_calculo_v2.py:3008` |
| Comercial | +2 | `app/ponto/motor_calculo_v2.py:3401`, `app/ponto/motor_calculo_v2.py:3412`, `app/ponto/motor_calculo_v2.py:899` |

O sítio antigo também omite `escalas=` e já pagava um SELECT. O nome real da função nesta cópia é `turnos_via_autoridade`; `_parear_motor` aparece somente no comentário. Num cálculo normal elegível, contando apenas essas consultas de escala da ata, passa de 1 para 2 no Base e de 1 para 3 nos dois overrides acima. `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:502`, `app/ponto/motor_calculo_v2.py:885`.

Os caminhos atingidos são:

- Fechamento por fatia: alimenta células em `app/ponto/services/fechamento.py:750`, chama a apuração em `app/ponto/services/fechamento.py:754` e calcula cada fatia em `app/ponto/services/fechamento.py:286`.
- Passada órfã: alimenta em `app/ponto/services/fechamento.py:825`, calcula com `escala_colaborador=None` e `colaborador_id` em `app/ponto/services/fechamento.py:827`. Nesse ramo, o novo sítio pode consultar; o antigo depende de `_ec.colaborador_id` e fica desabilitado. `app/ponto/motor_calculo_v2.py:469`, `app/ponto/motor_calculo_v2.py:496`.
- Espelho: alimenta em `app/ponto/services/espelho.py:1131`, chama fatias em `app/ponto/services/espelho.py:1164` ou cálculo direto em `app/ponto/services/espelho.py:1179`; a tela pede essa autoridade em `app/ponto/services/espelho.py:1227`.
- PDF: chama o espelho em `app/relatorios/pdf_espelho.py:596`, `app/relatorios/pdf_espelho.py:598`.

`papel_por_minuto_da_ata` diretamente pode executar três consultas sem alimentação: células, escalas e batidas. Com células e batidas fornecidas, sem `escalas=`, executa uma; fornecendo também `escalas={}` ou mapa carregado, essas três consultas desaparecem. `app/ponto/turnos.py:1588`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1602`.

E: **sim, o sítio antigo recebe batidas deduplicadas** no fluxo normal de `calcular_mes`: limpa em `app/ponto/motor_calculo_v2.py:2433`, chama `montar_periodos(batidas_limpas)` em `app/ponto/motor_calculo_v2.py:2436`, as implementações delegam a `_periodos_via_autoridade`, que chama `turnos_via_autoridade`. `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:2962`, `app/ponto/motor_calculo_v2.py:3300`, `app/ponto/motor_calculo_v2.py:530`.

`turnos_do_colab` carrega batidas vivas cruas e passa a lista ao papel sem essa deduplicação. O papel conta batidas por minuto e rejeita luzes com contagem diferente de um; também rejeita minuto ambíguo dentro do vão. `app/ponto/turnos.py:1866`, `app/ponto/turnos.py:1918`, `app/ponto/turnos.py:1942`, `app/ponto/turnos.py:1615`, `app/ponto/turnos.py:1659`, `app/ponto/turnos.py:1741`.

Contraexemplo estático: duas entradas consecutivas `08:00:01 E` e `08:00:20 E`, no minuto de uma luz, viram uma entrada no motor, porque o dedupe elimina o mesmo tipo com diferença menor que três minutos; no universo cru, a luz tem duas batidas e impede emissão do papel daquele dia. Ocorrência desse caso em produção: [nao verificado]. `app/ponto/motor_calculo_v2.py:70`, `app/ponto/motor_calculo_v2.py:736`, `app/ponto/motor_calculo_v2.py:914`, `app/ponto/motor_calculo_v2.py:921`, `app/ponto/turnos.py:1659`.

Pelo diff integral fornecido, essa diferença de universo entre o pareamento monetário do motor e `turnos_do_colab` é anterior à obra; a obra a estende ao novo consumo de papel em `_turnos_do_juiz`. Comparação independente com checkout do main e contagem real de consultas/latência: [nao verificado]. `app/ponto/motor_calculo_v2.py:502`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1942`.

_2860 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
