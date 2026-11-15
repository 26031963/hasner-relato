# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 01:32** · publicado em **15/11/2026 01:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 01:32 · sessao 9e8e05c97134 · - · - · origem=Ronald (tmux)

1. **Não há mudança na escolha entre batidas vivas da triagem.** Antes, `Meta.ordering=["colaborador","timestamp"]`; depois, `order_by("timestamp")`. Como a query restringe um colaborador e existe unicidade `(colaborador,timestamp)`, ambas devolvem o menor instante elegível; a diferença é remover retratadas (`app/ponto/models.py:105`, `app/ponto/models.py:106`, `app/ponto/services/triagem_batida.py:664`, `app/ponto/turnos.py:2883`). A afirmação de equivalência da entrega está correta para esse cenário; confirmação SQL executada [nao verificado].

2. **B5j não cai com esta cura.** Executei apenas suas funções AST em memória, simulando as três substituições em strings: copiadores `6 → 6`; os três módulos tocados contribuem `0 → 0`. O detector considera funções que chamam `parear_turnos`/`_turnos_do_colab_puro`, não qualquer função do módulo (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:123`). Os seis copiadores atuais são:
   - `app/ponto/turnos.py:1863`;
   - `app/ponto/turnos.py:1901`;
   - `app/ponto/services/lastro.py:261`;
   - `app/ponto/services/piso_intervalo.py:39`;
   - `app/escala/services/meio_periodo.py:158`;
   - `app/chamados/services/fila_por_causa.py:190`.

3. **B5j já tem folga anterior: teto 8 para seis atuais.** O comentário promete catraca fundada no número contado, mas a implementação exige somente `total <= 8`, permitindo dois copiadores adicionais (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:184`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:191`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:215`). Não atribuir essa folga à cura: ela já existe no HEAD; a lei apenas declara “8 copiadores ... não cresce” (`CLAUDE.md:163`).

4. **O selo de bocais deveria apertar sua contagem para guardar a remoção.** A regex encontra três filtros em `api/views.py`, nas linhas `296`, `529`, `674`; a substituição proposta leva a dois. Entretanto, o teto permanece sete e a asserção é somente `<=`, aumentando a folga de quatro para cinco (`app/ponto/tests/test_tripwire_bocais_apuracao.py:20`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:25`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`). Restaurar o filtro removido continuará passando nesse selo (`app/api/views.py:529`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`). `views_core` e `triagem_batida` não constam da allowlist (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`).

5. **Sem troca de instante por dia no predicado novo.** A autoridade aplica `timestamp__gte` e `timestamp__lte`, inclusive nas duas pontas; filtra somente retratação, contando pendentes de DP e sem restrição por origem/canal (`app/ponto/turnos.py:2859`, `app/ponto/turnos.py:2876`). Ela não lê relógio, não escreve e não trava (`app/ponto/turnos.py:2875`). As APIs fornecem `ts_efetivo` da autoridade de hora; a triagem usa seu `agora` capturado (`app/api/views.py:497`, `app/api/views.py:507`, `app/ponto/services/triagem_batida.py:626`).

6. **Retratação é carimbo do ato, não o instante da batida.** `retratar_batida` anota `aprovada=False` e `retratada_em=timezone.now()`; o gravador material é `anotar`/`save(update_fields=...)` (`app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:252`). O modelo declara `aprovada` como crivo do DP e retratação como exclusão da geometria/apuração (`app/ponto/models.py:63`, `app/ponto/models.py:64`).

7. **Fuso consistente para o caminho inspecionado.** A configuração declara `America/Sao_Paulo` e `USE_TZ=True`; a regra documental exige UTC no banco e apresentação UTC−3 (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `CLAUDE.md:146`). A autoridade da hora torna timestamps ingênuos aware; aceita o aparelho com atraso inclusivo de zero a 48 horas e retorna `agora` fora disso (`app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`). Persistência/fuso efetivos de produção [nao verificado].

## 15/11 01:32 · sessao cfe31aeba724 · - · - · origem=Ronald (tmux)

1. **Retratada dentro de 60s:** a porta exclui a anterior no filtro (`app/ponto/registro_batida.py:107-113`). Sem outra viva do mesmo tipo, chave exata ou oposta impeditiva, cria linha viva (`app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:145`, `app/ponto/registro_batida.py:157`). Não grava duplicata da chave; o banco exige unicidade por colaborador+timestamp (`app/ponto/models.py:106`).

2. **Chave exata:** devolve a linha anterior, inclusive retratada ou de outro tipo, sem revivê-la e sem criar nova (`app/ponto/registro_batida.py:116-122`). A cura portanto não garante nova batida apurável nesse caso (`app/ponto/turnos.py:2876`).

3. **Oposta na janela:** a janela é ±300s com escala/marcos e ±60s sem eles (`app/ponto/registro_batida.py:21-32`). Considera só opostas vivas e recusa o automático com `(None, conflito)`, salvo correspondência ao marco próprio (`app/ponto/registro_batida.py:139-151`). A tolerância do marco próprio é inclusiva, `<=5min` (`app/ponto/registro_batida.py:41`, `app/ponto/registro_batida.py:48`). As três cascas passam `autoridade_humana=False` (`app/api/views.py:656`, `app/api/views_core.py:647`, `app/ponto/services/triagem_batida.py:704`). Esse caminho continua barrando materialização (`app/ponto/registro_batida.py:151`).

4. **Resposta rica após retorno existente:** a porta não distingue “criada” de “reaproveitada” no contrato de retorno (`app/ponto/registro_batida.py:102`, `app/ponto/registro_batida.py:115`, `app/ponto/registro_batida.py:122`, `app/ponto/registro_batida.py:161`). As APIs devolvem `status=ok`, tipo e rótulo calculados da variável `tipo`, mas timestamp e sequência da linha retornada (`app/api/views.py:799-815`, `app/api/views_core.py:793-809`). Se a chave exata devolver retratada/outro tipo, o payload não informa a retratação e pode anunciar um tipo diferente do objeto (`app/ponto/registro_batida.py:116-122`, `app/api/views.py:812-815`). A sequência devolvida é a gravada na linha antiga, não `sequencia_jornada` recém-calculada (`app/api/views.py:555`, `app/api/views.py:659`, `app/api/views.py:814`). A linha reutilizada também recebe os efeitos posteriores, como foto e geofence (`app/api/views.py:769-781`).

5. **Triagem possui divergência adicional na resposta de oposta:** após `None`, procura oposta crua em ±60s, sem excluir retratada (`app/ponto/services/triagem_batida.py:715-725`). A porta pode ter recusado por uma viva a 3min, pois usa até ±300s (`app/ponto/registro_batida.py:21-32`, `app/ponto/registro_batida.py:139-151`). Logo a resposta pode trazer `id=None`, ou apontar uma retratada que não causou a recusa (`app/ponto/services/triagem_batida.py:716-725`). As APIs consultam a janela da porta e excluem retratada (`app/api/views.py:672-687`, `app/api/views_core.py:663-678`).

6. **Pendente DP/origem/canal:** as três queries antigas não filtram esses estados (`app/api/views.py:529-532`, `app/api/views_core.py:515-518`, `app/ponto/services/triagem_batida.py:664-669`). `batidas_apuraveis` também não os filtra; exclui somente retratada (`app/ponto/turnos.py:2876-2883`). A porta usa o mesmo critério no dedup e na colisão, e nem esse filtro na chave exata (`app/ponto/registro_batida.py:107-120`, `app/ponto/registro_batida.py:139-144`). Portanto `aprovada=False`, origem ou canal, isoladamente, não são divergências entre essas consultas (`app/ponto/turnos.py:2876-2883`, `app/ponto/registro_batida.py:107-120`).

7. **Divergências remanescentes:** API usa ±5min, triagem `[agora−5min,agora]`, porta ±60s para mesmo tipo (`app/api/views.py:527-532`, `app/ponto/services/triagem_batida.py:663-669`, `app/ponto/registro_batida.py:106-113`). Na API, dedup usa o tipo solicitado antes da correção pela autoridade; a porta recebe o tipo corrigido (`app/api/views.py:529-532`, `app/api/views.py:570-601`, `app/api/views.py:654-655`). Essas diferenças já existem no HEAD; a cura informada não as altera [nao verificado].

## 15/11 01:32 · sessao 747fd334a147 · - · - · origem=Ronald (tmux)

Censo independente por busca textual e AST dos `.py` da cópia, excluindo testes/migrations: encontrei os mesmos **três sítios de dedup de chão**, antes dos únicos três chamadores com origem `'app'`: `app/api/views.py:529` → `:654`; `app/api/views_core.py:515` → `:645`; `app/ponto/services/triagem_batida.py:664` → `:702`. Quarto sítio estático de chão não encontrado; cobertura de caminhos dinâmicos [nao verificado].

Outros controles antes de criar/reaproveitar batida, sem serem toque de chão:

- Materialização: `_batida_existente_na_janela`, mesmo tipo ±20 min, primeiro timestamp, sem filtro de retratação (`app/chamados/juizes.py:235`, `:243`); guarda de vizinha imediata anterior/posterior até 12h, também crua (`app/chamados/services/materializacao.py:86`, `:95`, `:101`).
- Regularização externa: chave exata colaborador+timestamp, pula antes da porta (`app/chamados/services/regularizacao_ext.py:71`, `:74`, `:78`).
- Veredito humano: carga crua, simulação e criação somente das novas propostas pela porta (`app/ponto/services/veredito_celula.py:237`, `:323`).
- Demais chamadores são autorização humana/justificativa: `app/chamados/services/acoes_chamado.py:44`; `app/ponto/views.py:1818`; `app/ponto/services/justificativa.py:188`, `:361`; materialização com `fato_de_chao=False`: `app/chamados/services/materializacao.py:933`, `:1013`. O comando de ensaio também chama a porta (`app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`).

O AST encontrou criação de Batida apenas dentro da porta: `app/ponto/registro_batida.py:152`, `:157`, `:335`. Escritas via reflexão/SQL dinâmico [nao verificado].

**ID e clientes:** APIs devolvem `dupla.pk` em `ja_registrada` (`app/api/views.py:538`; `app/api/views_core.py:524`), triagem devolve `dupla.id` (`app/ponto/services/triagem_batida.py:671`). No cliente v2 presente, todo HTTP 2xx vira sucesso sem ler corpo/ID (`app/templates/ponto/bater_ponto_v2.html:327`); sincronizador PWA lê JSON e descarta a fila em todo `resp.ok`, ignorando status/ID (`app/static/pwa.js:158`, `:162`). Leitor Android/APK desse ID [nao verificado]. Há consumidor servidor de `batida_id` no upload separado de foto (`app/api/views.py:836`, `:842`; `app/api/views_core.py:820`, `:826`), mas vincular esse consumidor ao ID retornado exige o cliente ausente [nao verificado]. O cliente web antigo envia formulário normal (`app/templates/ponto/bater_ponto.html:237`), e a view repassa a resposta JSON de triagem (`app/ponto/views.py:353`, `:356`).

**Ordem:** triagem no HEAD usa `Meta.ordering = ["colaborador", "timestamp"]` (`app/ponto/models.py:105`) com colaborador fixo (`app/ponto/services/triagem_batida.py:665`), portanto escolhe o menor timestamp. A autoridade também usa timestamp ascendente (`app/ponto/turnos.py:2883`): a troca proposta não muda a escolha entre vivas; elimina candidatas retratadas [nao verificado: diff proposto]. APIs mantêm a maior timestamp (`app/api/views.py:532`; `app/api/views_core.py:518`). A chave única impede empate de timestamp no mesmo colaborador (`app/ponto/models.py:106`).

**Import/uso:** trajeto de topo de `turnos.py` é copy/datetime/namedtuple/timezone/`core.dia_do_turno` (`app/ponto/turnos.py:36`–`:43`); `core.dia_do_turno` só importa datetime/typing e constrói NewType (`app/core/dia_do_turno.py:10`–`:13`). Não há import de retorno para as views/triagem nesse trajeto; execução real do novo import [nao verificado]. `Batida` continua necessário nas buscas de oposta posteriores à porta (`app/api/views.py:674`; `app/api/views_core.py:665`; `app/ponto/services/triagem_batida.py:716`) e na busca de entrada da triagem (`app/ponto/services/triagem_batida.py:257`).

## 15/11 01:28 · sessao d954e76d3e69 · BATIDA-DEDUP-RETRATADA · construtor · origem=Code

Cura nas três cascas e censo registrados na [entrega](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c1_entrega.txt:6).

RED anterior, 9 testes GREEN sem banco e 3 mutantes mortos, com comandos e saídas guardados na [evidência](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c1_entrega.txt:33). [nao verificado]

Certificação por Code/suite, opus e smoke: pendente [não verificado]. [nao verificado]

## 15/11 01:26 · sessao 4793ac876070 · SILENCIO-VENCIDO · censo · origem=Code

**Dono: não sei para cada um dos 59 pares e para a diferença FIM.** O anexo informa divergências mensais, mas não os dias responsáveis nem os insumos diários necessários ao juiz. Isso não permite afirmar que ESTRUTURA, CADASTRO ou BATIDA tenham contagem zero. [nao verificado — anexo colado]

O juiz encontrado na cópia está em `app/ponto/management/commands/e6_oraculo.py`. Sua entrada é diária; o campo mensal divergente não é argumento da função. [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:53)

| Ordem | Predicado que decide | Dono retornado | Fonte |
|---|---|---|---|
| 1 | `folga_nao_definida or sem_vinculo_no_dia or sem_vinculo_em_dia_de_trabalho` | CADASTRO | [e6_oraculo.py:98](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:98) |
| 2 | `batida_fora_de_turno` | BATIDA | [e6_oraculo.py:104](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:104) |
| 3 | `len(horas_do_dia or ()) % 2` | BATIDA | [e6_oraculo.py:106](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:106) |
| 4 | `fase_deslocada` | CADASTRO | [e6_oraculo.py:108](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:108) |
| 5 | `em_cadastro_x_realidade` | CADASTRO se verdadeiro; ESTRUTURA se falso | [e6_oraculo.py:110](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:110) |

O carregador existente reúne esses insumos e chama o juiz para uma `data`; não fornece uma redução de vários dias para um único dono mensal. [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:182)

Para compactar as tabelas: **R**=`minutos_realizados`, **A**=`minutos_abonados`, **P**=`minutos_previstos`, **T**=`horas_trabalhadas`. Esses campos pertencem a `CAMPOS_DIA`. [app/ponto/services/dia_pago.py:34](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/dia_pago.py:34)

Cada comparação abaixo está em **minutos, na ordem lavra × fechamento**, exatamente no escopo de `campos_divergentes` do anexo; Q1–Q4, adiante, são a coleta faltante comum aos 59 casos. [nao verificado — anexo colado]

| Empresa | Colab | Comp. | Fechamento | Campo: lavra × fechamento | Dono | Evidência |
|---|---|---|---|---|---|---|
| emp2 | col174 | 09/2026 | #4825 | R: 7355 × 7546 | não sei | [nao verificado] |
| emp2 | col191 | 09/2026 | #4518 | R: 8817 × 8489 | não sei | [nao verificado] |
| emp2 | col193 | 09/2026 | #4801 | R: 9825 × 5074 | não sei | [nao verificado] |
| emp2 | col206 | 09/2026 | #4418 | R: 7839 × 7479 | não sei | [nao verificado] |
| emp2 | col211 | 09/2026 | #4648 | R: 9000 × 8940 | não sei | [nao verificado] |
| emp2 | col214 | 09/2026 | #4668 | A: 6500 × 0 | não sei | [nao verificado] |
| emp2 | col231 | 09/2026 | #4699 | R: 9550 × 4996 | não sei | [nao verificado] |
| emp2 | col242 | 09/2026 | #4503 | T: 8869,20 × 8868,60; R: 5027 × 4797 | não sei | [nao verificado] |
| emp2 | col252 | 09/2026 | #4796 | R: 10260 × 9797 | não sei | [nao verificado] |
| emp2 | col269 | 09/2026 | #4569 | R: 9223 × 9226 | não sei | [nao verificado] |
| emp2 | col272 | 09/2026 | #4828 | A: 1980 × 0 | não sei | [nao verificado] |
| emp2 | col278 | 09/2026 | #4750 | T: 10659 × 10658,40; R: 4984 × 4939 | não sei | [nao verificado] |
| emp2 | col292 | 09/2026 | #4671 | A: 5280 × 4800; R: 5538 × 5964 | não sei | [nao verificado] |
| emp2 | col331 | 09/2026 | #4737 | R: 7562 × 6882 | não sei | [nao verificado] |
| emp2 | col349 | 09/2026 | #4666 | R: 9259 × 9178 | não sei | [nao verificado] |
| emp2 | col355 | 09/2026 | #4782 | A: 3960 × 3300 | não sei | [nao verificado] |
| emp2 | col371 | 09/2026 | #4564 | R: 6929 × 7169 | não sei | [nao verificado] |
| emp2 | col444 | 09/2026 | #4571 | R: 9877 × 9590 | não sei | [nao verificado] |
| emp2 | col489 | 09/2026 | #4552 | R: 8999 × 8759 | não sei | [nao verificado] |
| emp2 | col529 | 09/2026 | #4698 | R: 10989 × 10964 | não sei | [nao verificado] |
| emp2 | col532 | 09/2026 | #4508 | R: 10556 × 10376 | não sei | [nao verificado] |
| emp2 | col592 | 09/2026 | #4512 | R: 10205 × 10316 | não sei | [nao verificado] |
| emp2 | col616 | 09/2026 | #4703 | R: 9225 × 8042 | não sei | [nao verificado] |
| emp2 | col634 | 09/2026 | #4442 | R: 9373 × 9321 | não sei | [nao verificado] |
| emp2 | col735 | 09/2026 | #4496 | R: 6348 × 6226 | não sei | [nao verificado] |
| emp2 | col788 | 09/2026 | #4475 | R: 3092 × 2404 | não sei | [nao verificado] |
| emp2 | col789 | 09/2026 | #4551 | R: 10807 × 11187 | não sei | [nao verificado] |
| emp2 | col833 | 09/2026 | #4434 | R: 7162 × 7403 | não sei | [nao verificado] |
| emp2 | col849 | 09/2026 | #4640 | P: 11340 × 12360 | não sei | [nao verificado] |
| emp2 | col859 | 09/2026 | #4820 | A: 0 × 420 | não sei | [nao verificado] |
| emp2 | col865 | 09/2026 | #4694 | R: 8644 × 8914 | não sei | [nao verificado] |
| emp2 | col872 | 09/2026 | #4744 | R: 9759 × 9663 | não sei | [nao verificado] |
| emp2 | col885 | 09/2026 | #4702 | R: 9594 × 8987 | não sei | [nao verificado] |
| emp2 | col915 | 09/2026 | #4759 | R: 8548 × 8499 | não sei | [nao verificado] |
| emp2 | col923 | 09/2026 | #4642 | R: 5803 × 2414 | não sei | [nao verificado] |
| emp2 | col941 | 09/2026 | #4436 | R: 2310 × 1998 | não sei | [nao verificado] |
| emp2 | col945 | 09/2026 | #4608 | R: 1437 × 1766 | não sei | [nao verificado] |
| emp2 | col301 | 10/2026 | #5308 | T: 10326 × 10325,40 | não sei | [nao verificado] |
| emp2 | col899 | 10/2026 | #5220 | T: 10192,80 × 10192,20 | não sei | [nao verificado] |
| emp2 | col191 | 11/2026 | #6074 | T: 8223 × 8223,60 | não sei | [nao verificado] |
| emp2 | col369 | 11/2026 | #5970 | T: 8800,20 × 8800,80 | não sei | [nao verificado] |
| emp2 | col415 | 11/2026 | #6054 | T: 8303,40 × 8304 | não sei | [nao verificado] |
| emp2 | col476 | 11/2026 | #5855 | T: 8959,20 × 8959,80 | não sei | [nao verificado] |
| emp2 | col788 | 11/2026 | #6139 | T: 6606 × 6606,60 | não sei | [nao verificado] |
| emp2 | col899 | 11/2026 | #6004 | T: 7587,60 × 7588,20 | não sei | [nao verificado] |
| emp2 | col961 | 11/2026 | #6018 | T: 8848,80 × 8849,40 | não sei | [nao verificado] |
| emp3 | col82 | 09/2026 | #4856 | R: 10560 × 10517 | não sei | [nao verificado] |
| emp3 | col87 | 09/2026 | #4861 | R: 8858 × 8227 | não sei | [nao verificado] |
| emp3 | col114 | 09/2026 | #4892 | R: 3051 × 2694 | não sei | [nao verificado] |
| emp3 | col133 | 09/2026 | #4915 | A: 3840 × 0 | não sei | [nao verificado] |
| emp3 | col145 | 09/2026 | #4926 | R: 1320 × 1267 | não sei | [nao verificado] |
| emp3 | col165 | 09/2026 | #4946 | R: 2265 × 2597 | não sei | [nao verificado] |
| emp3 | col502 | 09/2026 | #4887 | R: 7883 × 7409 | não sei | [nao verificado] |
| emp3 | col638 | 09/2026 | #4839 | P: 10690 × 12040; R: 9632 × 9773 | não sei | [nao verificado] |
| emp3 | col749 | 09/2026 | #4910 | P: 11440 × 11880; R: 9324 × 9702 | não sei | [nao verificado] |
| emp3 | col868 | 09/2026 | #4905 | R: 8645 × 8389 | não sei | [nao verificado] |
| emp3 | col749 | 11/2026 | #5630 | T: 7419,60 × 7420,20 | não sei | [nao verificado] |
| emp3 | col911 | 11/2026 | #6041 | T: 8576,40 × 8577 | não sei | [nao verificado] |
| emp4 | col50 | 09/2026 | #4964 | A: 660 × 1320; R: 7799 × 7724 | não sei | [nao verificado] |

O agrupamento **por conjunto exato de campos** conta cada linha uma vez e soma 59. [nao verificado — contagem do anexo colado]

| Campos divergentes | emp2 | emp3 | emp4 | Total | Evidência |
|---|---:|---:|---:|---:|---|
| somente R | 29 | 7 | 0 | 36 | [nao verificado] |
| somente A | 4 | 1 | 0 | 5 | [nao verificado] |
| somente P | 1 | 0 | 0 | 1 | [nao verificado] |
| somente T | 9 | 2 | 0 | 11 | [nao verificado] |
| T + R | 2 | 0 | 0 | 2 | [nao verificado] |
| A + R | 1 | 0 | 1 | 2 | [nao verificado] |
| P + R | 0 | 2 | 0 | 2 | [nao verificado] |
| **Total de linhas; dono não sei** | **46** | **12** | **1** | **59** | [nao verificado] |

Contando cada campo individualmente, são R=42, A=7, P=3 e T=13: **65 ocorrências**, porque seis linhas têm dois campos. [nao verificado — contagem do anexo colado]

DSR não integra esse predicado de erro: `CAMPOS_DIA` contém os campos comparados, enquanto `horas_reflexo_dsr` pertence a `CAMPOS_AJUSTE`. A soma da competência também arredonda cada agregado a duas casas antes de devolvê-lo. [app/ponto/services/dia_pago.py:34](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/dia_pago.py:34), [app/ponto/services/dia_pago.py:563](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/dia_pago.py:563)

**FIM: diferença de 19 dias; dono não sei.** O campo confrontado é a cardinalidade de `universo`/`total.dias`/`soma_partes.dias` contra `esperado`, não uma rubrica de lavra contra fechamento. [nao verificado — FIM colado]

| Conferência | Resultado | Evidência |
|---|---:|---|
| Soma das partes | 12+5+546+599+776+87+124+151+6+8+9 = 2323 | [nao verificado] |
| Estados | 2307 lavrados + 16 sem lavratura = 2323 | [nao verificado] |
| Universo contra esperado | 2323−2304 = **19** | [nao verificado] |
| Universo contra TSV | 2303 coincidentes + 20 fora = 2323 | [nao verificado] |
| TSV contra universo | 2303 coincidentes + 78 fora = 2381 | [nao verificado] |

Os 20 fora do TSV não podem ser chamados de “19 excedentes do esperado”: faltam as chaves dos conjuntos e a origem do esperado 2304. Também não há vínculo demonstrado entre esses 19 e os 16 dias sem lavratura. [nao verificado — anexo colado]

**Dado faltante e SELECTs de coleta.** Para os 59, faltam: identificação dos dias implicados na diferença mensal; horas atribuídas pelo oráculo; resposta de `batida_fora_de_turno`; flags de cadastro; fase lavrada; publicação CXR com cobertura. Essas são as fontes usadas por `InsumosDoDono`; valores mensais de R/A/P/T não as substituem. [app/ponto/management/commands/e6_oraculo.py:128](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:128), [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:182)

Os SELECTs abaixo são propostas **não executadas**; tabelas físicas e resultados não foram conferidos em banco. `fechamentos` e `colabs` são arrays dos identificadores da tabela acima, sem os prefixos; `ini/fim` devem vir de `janela_fechamento`, respeitando o corte da empresa. [nao verificado], [app/ponto/janelas.py:54](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/janelas.py:54)

**Q1 — linhas diárias, valores e carimbos.** A consulta recuperaria a decomposição atual da lavra; o fechamento mostrado pelo modelo guarda esses valores como totais mensais. [app/ponto/models.py:315](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/models.py:315), [app/ponto/models.py:398](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/models.py:398)

```sql
SELECT 'emp' || c.empresa_id AS empresa,
       'col' || f.colaborador_id AS colab,
       '#' || f.id AS fechamento,
       f.mes, f.ano, f.derivado_em,
       '#' || d.id AS dia_pago,
       d.data, d.tipo, d.versao, d.lavrado_em,
       d.versao_motor, d.versao_dna, d.evento_origem,
       d.horas_trabalhadas * 60 AS lavra_trabalhadas_min,
       d.minutos_abonados AS lavra_abonados_min,
       d.minutos_previstos AS lavra_previstos_min,
       d.minutos_realizados AS lavra_realizados_min,
       f.horas_trabalhadas * 60 AS fechamento_trabalhadas_min,
       f.minutos_abonados AS fechamento_abonados_min,
       f.minutos_previstos AS fechamento_previstos_min,
       f.minutos_realizados AS fechamento_realizados_min
FROM ponto_fechamentomensal f
JOIN colaboradores_colaborador c ON c.id = f.colaborador_id
LEFT JOIN ponto_diapago d
       ON d.colaborador_id = f.colaborador_id
      AND d.mes = f.mes AND d.ano = f.ano
      AND d.versao = 'motor'
WHERE f.id = ANY(%(fechamentos)s)
ORDER BY c.empresa_id, f.colaborador_id, f.ano, f.mes, d.data
```

**Q2 — batidas para alimentar as autoridades existentes.** A seleção do universo apurável pertence a `batidas_apuraveis`; a atribuição e a paridade usadas pelo dono vêm do carregador, e não de contar batidas por data civil em SQL. [app/ponto/turnos.py:2875](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/turnos.py:2875), [app/ponto/management/commands/e6_oraculo.py:150](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:150)

```sql
SELECT 'col' || b.colaborador_id AS colab,
       '#' || b.id AS batida,
       b.timestamp, b.tipo, b.is_intervalo,
       b.retratada_em
FROM ponto_batida b
WHERE b.colaborador_id = ANY(%(colabs)s)
ORDER BY b.colaborador_id, b.timestamp, b.id
```

**Q3 — cadastro, geradora e folgas.** Os resultados devem voltar a `cadastro_do_dia`/`cadastro_do_dia_carregado`; os SELECTs não escolhem o vínculo nem derivam os flags. [app/ponto/supra_juiz.py:155](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/supra_juiz.py:155), [app/ponto/supra_juiz.py:179](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/supra_juiz.py:179), [app/escala/models.py:1241](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/escala/models.py:1241)

```sql
SELECT 'col' || c.id AS colab,
       'emp' || c.empresa_id AS empresa,
       c.data_demissao, e.dia_inicio_competencia,
       '#' || v.id AS vinculo,
       v.data_inicio, v.data_fim, v.ativa,
       '#' || v.tipo_escala_id AS template,
       t.tipo_ciclo, t.folga_dia_semana
FROM colaboradores_colaborador c
JOIN colaboradores_empresa e ON e.id = c.empresa_id
LEFT JOIN escala_escalacolaborador v ON v.colaborador_id = c.id
LEFT JOIN escala_tipoescala t ON t.id = v.tipo_escala_id
WHERE c.id = ANY(%(colabs)s)
ORDER BY c.id, v.id
```

```sql
SELECT 'col' || colaborador_id AS colab,
       '#' || id AS celula, data, trabalha,
       '#' || escala_geradora_id AS geradora,
       dna -> 'marcos' AS marcos,
       ata -> 'tipo_dia' AS tipo_dia,
       ata -> 'lampadas' AS lampadas,
       ata -> 'orfas' AS orfas
FROM escala_celuladia
WHERE colaborador_id = ANY(%(colabs)s)
  AND data BETWEEN %(ini)s AND %(fim)s
ORDER BY colaborador_id, data
```

```sql
SELECT 'col' || v.colaborador_id AS colab,
       '#' || f.id AS folga,
       '#' || f.escala_colaborador_id AS vinculo,
       f.data
FROM escala_folgadia f
JOIN escala_escalacolaborador v ON v.id = f.escala_colaborador_id
WHERE v.colaborador_id = ANY(%(colabs)s)
ORDER BY v.colaborador_id, f.data
```

**Q4 — lavras de CXR e fase, preservando cobertura.** CXR não coberto retorna `None`, não conjunto vazio; fase é lida pela lavra mais recente que cobriu cada dia. A decisão deve continuar passando por esses leitores e pelo juiz existente. [app/ponto/services/cxr_na_lista.py:165](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/services/cxr_na_lista.py:165), [app/escala/services/fase_12x36.py:117](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/escala/services/fase_12x36.py:117)

```sql
SELECT '#' || s.id AS snapshot,
       s.chave, s.escopo, s.escopo_id,
       s.data_ref, s.calculado_em,
       s.detalhe -> 'agora' AS agora,
       s.detalhe -> 'janelas' AS janelas,
       s.detalhe -> 'cobertos' AS cobertos,
       (
         SELECT jsonb_agg(jsonb_build_object(
                  'colab', 'col' || x.item ->> 'colab',
                  'empresa', 'emp' || x.item ->> 'empresa',
                  'data', x.item ->> 'data',
                  'competencia', x.item ->> 'competencia',
                  'dono', x.item ->> 'dono'
                ))
         FROM jsonb_array_elements(
           COALESCE(s.detalhe -> 'itens', '[]'::jsonb) ||
           COALESCE(s.detalhe -> 'itens_reabertos', '[]'::jsonb)
         ) AS x(item)
         WHERE (x.item ->> 'colab')::integer = ANY(%(colabs)s)
       ) AS itens_dos_colabs
FROM inteligencia_metricasnapshot s
WHERE s.data_ref <= %(hoje)s
  AND (
    (s.chave = 'cxr_do_motor' AND s.escopo = 'frota')
    OR
    (s.chave = 'desvio_de_cadastro_12x36'
     AND s.escopo = 'tenant' AND s.escopo_id = 0)
  )
ORDER BY s.data_ref DESC, s.calculado_em DESC
```

Mesmo com essa coleta, faltaria demonstrar **quais dias explicam a diferença mensal**; aplicar o dono a todos os dias e eleger um deles como dono do mês acrescentaria uma regra ausente da função. [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/880331a8c8d394724b78217edc84bede94b99cb7/app/ponto/management/commands/e6_oraculo.py:53)

Para o **FIM**, faltam as chaves completas do universo 2323, do TSV 2381 e, principalmente, do conjunto esperado 2304. A sonda colada contém somente a constante `ESPERADO = 2304`; não demonstra onde obter a composição desse conjunto por SELECT. [nao verificado — sonda/anexo colados]

Este SELECT confrontaria os conjuntos **depois de fornecidos**, sem reproduzir a seleção da sonda; não recupera a referência ausente. [nao verificado — proposta não executada]

```sql
SELECT 'emp' || COALESCE(u.emp, e.emp) AS empresa,
       'col' || COALESCE(u.colab, e.colab) AS colab,
       COALESCE(u.mes, e.mes) AS mes,
       COALESCE(u.ano, e.ano) AS ano,
       COALESCE(u.dia, e.dia) AS dia,
       u.dia IS NOT NULL AS no_universo,
       e.dia IS NOT NULL AS no_esperado
FROM jsonb_to_recordset(%(universo_json)s::jsonb)
     AS u(emp integer, colab integer, mes integer, ano integer, dia date)
FULL JOIN jsonb_to_recordset(%(esperado_json)s::jsonb)
     AS e(emp integer, colab integer, mes integer, ano integer, dia date)
USING (emp, colab, mes, ano, dia)
WHERE u.dia IS NULL OR e.dia IS NULL
ORDER BY empresa, colab, ano, mes, dia
```

## 15/11 01:26 · sessao 7e9c9062581d · BATIDA-DEDUP-RETRATADA · sonda · origem=Code

```python
# Django shell; tenant juliani previamente ativo no envelope.
# Execucao e resultados de producao: [nao verificado].
# Correspondencia da copia ao HEAD Git: [nao verificado].
#
# Campos Batida: app/ponto/models.py:22, app/ponto/models.py:67.
# Empresa: app/colaboradores/models.py:40; FK do colaborador:
# app/colaboradores/models.py:321.
# Disputa/contexto: app/chamados/models.py:2494,
# app/chamados/models.py:1783, app/chamados/models.py:1792.
# Envelope ultima_batida: app/api/views.py:758.
#
# Terminal usado: PerguntaDisputa.fora_do_fato(),
# app/chamados/models.py:3276; predicado em :3284.
# Vida exata: PerguntaDisputa.viva, app/chamados/models.py:3474,
# delegacao em :3484. q_vivas e apenas pre-filtro (:3425).
# Exportada: folha.export.competencia_entregue,
# app/folha/export.py:556; consulta/retorno em :588.
# Janela por empresa: ponto.janelas.janela_atual,
# app/ponto/janelas.py:60. Nenhuma dessas regras e replicada.
#
# Convencoes desta sonda:
# 3a: 60 datas locais, incluindo hoje ate o instante da leitura.
# Recorte pelo timestamp da retratada, nao por retratada_em.
# A outra pode estar fora do recorte; janela inclusiva de +/- 5 minutos.
# OUTRAS_* conta PKs distintos; PARES_* conta retratada -> outra.
# COLABORADORES_DISTINTOS conta colaboradores com retratada que tem outra.
# T4: todo o acervo candidato, sem recorte de 60 dias.
# EXPORTADA = entregue em qualquer dos dois dias; ABERTA = em nenhum.
# ABERTA aqui significa somente "nao exportada".
# Erros tornam os resultados parciais e FIM ok=0.
# Nenhuma mensagem inclui conteudo de excecao, nomes ou documentos.

erros = 0
nivel_logging_anterior = None


def erro(etapa, exc, pergunta_pk=None):
    global erros
    erros += 1
    print(
        "ERRO_ITEM etapa={} pergunta_pk={} classe={}".format(
            etapa, pergunta_pk if pergunta_pk is not None else "-",
            type(exc).__name__,
        ),
        flush=True,
    )


try:
    import logging
    import re
    from collections import Counter
    from contextlib import ExitStack
    from datetime import datetime, time, timedelta, timezone as dt_timezone
    from itertools import islice
    from unittest.mock import patch

    from django.db import connection, connections
    from django.utils import timezone
    from django.utils.dateparse import parse_datetime

    nivel_logging_anterior = logging.root.manager.disable
    logging.disable(logging.CRITICAL)

    alias_leitura = connection.alias

    def somente_select(execute, sql, params, many, context):
        if (
            many
            or context["connection"].alias != alias_leitura
            or not re.match(r"\s*SELECT\b", sql, re.I)
            or ";" in sql
            or re.search(
                r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
                sql, re.I,
            )
        ):
            raise RuntimeError("consulta_recusada")
        return execute(sql, params, many, context)

    def best_effort_falhou(logger_nome, operacao, exc, **contexto):
        # Captura sem escrever log; autoridade de observabilidade:
        # app/core/observ.py:73; excepts do juiz:
        # app/chamados/juizes.py:1473, app/chamados/juizes.py:1486.
        erro("best_effort", exc)

    local = dt_timezone(timedelta(hours=-3))

    with ExitStack() as pilha:
        for alias in connections:
            pilha.enter_context(
                connections[alias].execute_wrapper(somente_select)
            )
        pilha.enter_context(timezone.override(local))
        pilha.enter_context(
            patch("core.observ.registrar_engolido", new=best_effort_falhou)
        )

        from colaboradores.models import Colaborador, Empresa
        from ponto.models import Batida
        from chamados.models import PerguntaDisputa
        from chamados.juizes import tranca_da_consulta
        from folha.export import competencia_entregue
        from ponto.janelas import janela_atual

        # Nenhum SET, atomic, savepoint ou troca de tenant nesta sonda.
        with connection.cursor() as cursor:
            cursor.execute("SELECT current_schema()")
            if cursor.fetchone()[0] != "juliani":
                raise RuntimeError("tenant_incorreto")

        empresas = {
            e.pk: e
            for e in Empresa.objects.using(alias_leitura).only(
                "pk", "dia_inicio_competencia"
            )
        }

        agora = timezone.now().astimezone(local)
        inicio = datetime.combine(
            agora.date() - timedelta(days=59), time.min, tzinfo=local
        )
        print(
            "JANELA_3A inicio_inclusivo={} fim_exclusivo={} datas_locais=60".format(
                inicio.isoformat(), agora.isoformat()
            )
        )

        # 3a: LEFT JOIN preserva tambem retratadas sem outra.
        bt = connection.ops.quote_name(Batida._meta.db_table)
        ct = connection.ops.quote_name(Colaborador._meta.db_table)
        sql = f"""
            SELECT
                c.empresa_id,
                COUNT(DISTINCT r.id),
                COUNT(DISTINCT r.id) FILTER (WHERE o.id IS NOT NULL),
                COUNT(DISTINCT r.id) FILTER (
                    WHERE o.id IS NOT NULL AND o.retratada_em IS NULL
                ),
                COUNT(DISTINCT o.id),
                COUNT(DISTINCT o.id) FILTER (
                    WHERE o.retratada_em IS NULL
                ),
                COUNT(o.id),
                COUNT(o.id) FILTER (WHERE o.retratada_em IS NULL),
                COUNT(DISTINCT r.colaborador_id) FILTER (
                    WHERE o.id IS NOT NULL
                )
            FROM {bt} r
            JOIN {ct} c ON c.id = r.colaborador_id
            LEFT JOIN {bt} o
                ON o.colaborador_id = r.colaborador_id
                AND o.tipo = r.tipo
                AND o.id <> r.id
                AND o.timestamp BETWEEN r.timestamp - %s
                                    AND r.timestamp + %s
            WHERE r.retratada_em IS NOT NULL
                AND r.timestamp >= %s
                AND r.timestamp < %s
            GROUP BY c.empresa_id
            ORDER BY c.empresa_id
        """
        with connection.cursor() as cursor:
            cursor.execute(
                sql, [timedelta(minutes=5), timedelta(minutes=5), inicio, agora]
            )
            por_empresa = {
                row[0]: tuple(int(n) for n in row[1:])
                for row in cursor.fetchall()
            }

        metricas = (
            "RETRATADAS",
            "RETRATADAS_COM_OUTRA",
            "RETRATADAS_COM_OUTRA_VIVA",
            "OUTRAS_DISTINTAS",
            "OUTRAS_VIVAS_DISTINTAS",
            "PARES",
            "PARES_OUTRA_VIVA",
            "COLABORADORES_DISTINTOS",
        )
        total = [0] * len(metricas)
        for empresa_pk in sorted(set(empresas) | set(por_empresa)):
            valores = por_empresa.get(empresa_pk, (0,) * len(metricas))
            for i, (chave, valor) in enumerate(zip(metricas, valores)):
                print(f"3A_EMPRESA_{empresa_pk}_{chave} {valor}")
                total[i] += valor
        for chave, valor in zip(metricas, total):
            print(f"3A_TOTAL_{chave} {valor}")

        # T4: cache somente em memoria; a autoridade continua sendo chamada.
        cache_exportada = {}

        def exportada_no_dia(empresa, dia):
            _, fim = janela_atual(dia, empresa)
            chave = (empresa.pk, fim.month, fim.year)
            if chave not in cache_exportada:
                cache_exportada[chave] = competencia_entregue(*chave)
            return cache_exportada[chave]

        cont = Counter()
        exportadas_pks = []
        candidatas = (
            PerguntaDisputa.objects.using(alias_leitura)
            .filter(PerguntaDisputa.q_vivas())
            .select_related(
                "disputa__colaborador__empresa",
                "celula",
                "chamado_destino",
                "batida_origem",
            )
            .defer(
                "disputa__colaborador__nome",
                "disputa__colaborador__cpf",
            )
            .order_by("pk")
            .iterator(chunk_size=200)
        )

        while True:
            bloco = list(islice(candidatas, 200))
            if not bloco:
                break

            preparados = []
            for p in bloco:
                try:
                    if p.fora_do_fato():
                        continue
                    cont["CANDIDATAS_EXAMINADAS"] += 1
                    ctx = p.disputa.contexto
                    if ctx is None:
                        ctx = {}
                    if not isinstance(ctx, dict):
                        raise ValueError("contexto_invalido")
                    ub = ctx.get("ultima_batida")
                    if ub is None:
                        cont["CANDIDATAS_SEM_ULTIMA_BATIDA"] += 1
                        continue
                    if not isinstance(ub, dict):
                        raise ValueError("ultima_batida_invalida")
                    bruto_id = ub.get("id")
                    if (
                        isinstance(bruto_id, bool)
                        or not isinstance(bruto_id, (int, str))
                        or not re.fullmatch(r"[0-9]+", str(bruto_id))
                        or int(bruto_id) <= 0
                    ):
                        raise ValueError("batida_id_invalido")
                    bruto_ts = ub.get("timestamp")
                    if not isinstance(bruto_ts, str):
                        raise ValueError("timestamp_ausente_ou_invalido")
                    ts = parse_datetime(bruto_ts)
                    if ts is None or timezone.is_naive(ts):
                        raise ValueError("timestamp_invalido_ou_sem_fuso")
                    preparados.append(
                        (p, int(bruto_id), ts.astimezone(local).date())
                    )
                except Exception as exc:
                    erro("t4_contexto", exc, p.pk)

            batidas = {
                b["pk"]: b
                for b in Batida.objects.using(alias_leitura)
                .filter(pk__in={bid for _, bid, _ in preparados})
                .values("pk", "colaborador_id", "timestamp")
            }

            diferentes = []
            for p, bid, dia_contexto in preparados:
                try:
                    b = batidas.get(bid)
                    if b is None:
                        raise ValueError("batida_referenciada_ausente")
                    cid = p.disputa.colaborador_id
                    if b["colaborador_id"] != cid or p.colaborador_id != cid:
                        raise ValueError("colaborador_divergente")
                    if timezone.is_naive(b["timestamp"]):
                        raise ValueError("batida_timestamp_sem_fuso")
                    dia_batida = b["timestamp"].astimezone(local).date()
                    if dia_contexto != dia_batida:
                        diferentes.append((p, dia_contexto, dia_batida))
                except Exception as exc:
                    erro("t4_batida", exc, p.pk)

            # Autoridade de tranca em lote, sem reconstruir seu predicado:
            # app/chamados/juizes.py:1128.
            with tranca_da_consulta([p for p, _, _ in diferentes]):
                for p, dia_contexto, dia_batida in diferentes:
                    try:
                        if not p.viva:
                            continue
                        empresa = empresas[p.disputa.colaborador.empresa_id]
                        exp_ctx = exportada_no_dia(empresa, dia_contexto)
                        exp_bat = exportada_no_dia(empresa, dia_batida)

                        cont["VIVAS_DIAS_DIFERENTES"] += 1
                        if exp_ctx or exp_bat:
                            cont["EXPORTADA"] += 1
                            detalhe = (
                                "AMBAS" if exp_ctx and exp_bat
                                else "SO_CONTEXTO" if exp_ctx
                                else "SO_BATIDA"
                            )
                            cont["EXPORTADA_" + detalhe] += 1
                            if len(exportadas_pks) < 20:
                                exportadas_pks.append(p.pk)
                        else:
                            cont["ABERTA"] += 1
                    except Exception as exc:
                        erro("t4_vida_exportacao", exc, p.pk)

        for chave in (
            "CANDIDATAS_EXAMINADAS",
            "CANDIDATAS_SEM_ULTIMA_BATIDA",
            "VIVAS_DIAS_DIFERENTES",
            "EXPORTADA",
            "ABERTA",
            "EXPORTADA_AMBAS",
            "EXPORTADA_SO_CONTEXTO",
            "EXPORTADA_SO_BATIDA",
        ):
            print(f"T4_{chave} {cont[chave]}")
        print(
            "T4_EXPORTADA_PERGUNTA_PKS "
            + (",".join(map(str, exportadas_pks)) or "-")
        )
        print(f"T4_PAREI_EXPORTADA {int(cont['EXPORTADA'] > 0)}")

except BaseException as exc:
    erro("sonda", exc)
finally:
    if nivel_logging_anterior is not None:
        logging.disable(nivel_logging_anterior)
    print(f"ERRO {erros}", flush=True)
    print(f"FIM ok={int(erros == 0)}", flush=True)
```

## 15/11 01:06 · sessao a83435cd3ae0 · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Entrega em [cef_c1_entrega.txt](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c1_entrega.txt:1), com curas T1/T2/T3 e selo T5.

[Verificação local](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c1_verificacao.txt:1): oito mutantes mortos, controles verdes.

T4 ficou como PERGUNTA pelo significado declarado no [leitor](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/juizes.py:151). Suite com banco e certificação final: [nao verificado]. [nao verificado]

## 15/11 00:54 · sessao 7011158ac77e · - · - · origem=Ronald (tmux)

Censo estático da cópia; nenhum arquivo escrito. AGENTS.md aplicável não localizado; descoberta de instruções por mecanismos dinâmicos [nao verificado].

| Campo | Escritores atuais | Leitores atuais |
|---|---|---|
| `ultima_batida.id` | APIs: `api/views.py:758-761`, `api/views_core.py:752-755`; agregado substitui chave de topo em `chamados/services/disputa_emissao.py:548-562`; nascimento persiste em `:590` | Origem BATIDA: `chamados/models.py:3603-3610`; referência para grade: `chamados/services/disputa_emissao.py:72-76`; dia: `:387-392`; upload: `api/foto_recebida.py:67-68` |
| `ultima_batida.tipo` | `api/views.py:760`, `api/views_core.py:754` | Nenhum leitor semântico específico localizado; despacho dinâmico [nao verificado] |
| `ultima_batida.timestamp` | Recepção `agora`: `api/views.py:497`, `:761`; `api/views_core.py:483`, `:755` | Único leitor executável localizado: `chamados/juizes.py:159-166`, que extrai `.date()`; singular chega por `:138`, lote por `:226` |
| `foto_ausente.tipo_batida` | APIs: `api/views.py:453`, `api/views_core.py:439`; retro: `ponto/management/commands/marcar_foto_ausente_retro.py:99` | Nenhum leitor semântico específico localizado; dinâmico [nao verificado] |
| `foto_ausente.batida_id` | Retro: `ponto/management/commands/marcar_foto_ausente_retro.py:100` | Nenhum leitor semântico específico localizado; origem lê outro caminho em `chamados/models.py:3606` |
| `foto_ausente.batida_em` | Retro, instante local da batida: `ponto/management/commands/marcar_foto_ausente_retro.py:101` | Nenhum leitor semântico específico localizado; gerador usa episódio em `chamados/services/disputa_emissao.py:387-392` |
| `foto_ausente._por` | Retro: `ponto/management/commands/marcar_foto_ausente_retro.py:102` | Nenhum leitor semântico específico localizado; dinâmico [nao verificado] |
| `contexto[motivo].data` | Triagem: `ponto/services/triagem_batida.py:739-740`; fábrica: `chamados/services/fabrica_da_celula.py:172`; enriquecimento: `chamados/services/disputa_emissao.py:283` | Gerador lê antes de buscar episódio em `chamados/services/disputa_emissao.py:372-377` |
| `contexto[motivo].qtd` / `ultima_em` | Agregação: `chamados/services/disputa_emissao.py:552-558` | `qtd` anterior lido pelo próprio merge em `:552`; outro leitor semântico específico [nao verificado] |

**Ressalva T4:** o código do único leitor executável consome apenas o dia civil do ISO (`chamados/juizes.py:166`), mas seu docstring atribui explicitamente aos carimbos o significado “QUANDO o problema foi notado” (`:151-156`); `dia_do_fato` também chama batida/disputa/carimbos de “QUANDO foi notado” (`:93-95`). Portanto, a conclusão “nenhum consumidor exige recepção” é inferência sobre execução; conflito documental deve constar da entrega. A varredura não encontrou decisão, cálculo de latência ou seleção baseada na recepção desse envelope; completude para despacho dinâmico [nao verificado].

O contexto fraco vem do **chamado** (`chamados/juizes.py:100`, `:138`), cujo contexto nasce separadamente do da disputa (`chamados/services/disputa_emissao.py:580`, `:590`). Em disputa já aberta, o merge atualiza somente a disputa nesse ramo (`:543-566`). A fusão de recorrência do chamado preserva escalar já existente (`chamados/services/contexto.py:131-156`).

Os cinco chamadores diretos da porta são:

- APIs, potencialmente BATIDA: `api/views.py:764-766`, `api/views_core.py:758-760`.
- Retro, motivo literal BATIDA `foto_ausente`: `ponto/management/commands/marcar_foto_ausente_retro.py:96-104`.
- Triagem, motivo literal MARCO `saida_sem_entrada`: `ponto/services/triagem_batida.py:735-744`.
- Fábrica, motivo variável originado da ata: `chamados/services/fabrica_da_celula.py:139`, `:157`, `:160-174`.

Família BATIDA declarada pelo catálogo: `chamados/catalogo/motivos.py:16`, `:36-47`; `foto_ausente` em `:43`. O índice correspondente identifica `(batida_origem, motivo)` e tem condições próprias de horizonte, conversa, via e origem preenchida (`chamados/models.py:3631-3636`).

Autoridades e padrão para a linha LEI-AKITA:

- Casos pela regra antes do código: `docs/LEIS.md:136`.
- Dia do turno por PK: `ponto/turnos.py:2927-2936`; delega a `dia_das_batidas`, cuja batida não reclamada usa o dia local (`:2955`). Registro do juiz: `core/juizes.py:212`.
- Juiz da selfie: `core/juizes.py:1205`.
- Corte do upload por episódio e incluindo foto tardia: `docs/CORTES.md:441`.
- Validação pelo escritor único: `docs/JUIZES.md:38-39`, usada pelo upload em `api/foto_recebida.py:48-51`.
- `padrao=idempotencia pela chave do fato` consta de `docs/GLOSSARIO.md:29`; para o selo AST, `Architecture Conformance Test` consta de `:15`.

Homônimos `ultima_batida` da ficha/ranking não são este envelope: escritores em `colaboradores/services/ficha.py:147`, `escala/services/furos_diarios.py:281`; leitores em `colaboradores/services/ficha.py:215-216`, `relatorios/views.py:1675`. Templates também leem esses homônimos, não o JSON da disputa: `templates/colaboradores/partials/_ps_linha.html:20`, `templates/relatorios/ranking_furos.html:76`.

## 15/11 00:54 · sessao 95b31c10c245 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhum arquivo escrito.

- O juiz geral já existe: `DisputaSupervisao.pode_fechar()` consulta respostas sem veredito e `fio_vivo`; `fechar()` recusa perguntas vivas. (`chamados/models.py:1911`, `chamados/models.py:1950`, `chamados/models.py:1960`)
- Para preservar o motivo enquanto outra pergunta de foto continua viva, o upload pode consultar `fio_vivo(d.perguntas.filter(motivo='foto_ausente'))`: esse redutor delega a `pergunta_viva`, sem novo predicado. (`chamados/juizes.py:1504`)
- `aparar_motivo_respondido()` preserva perguntas, remove motivo/contexto e não fecha a disputa; deve vir somente depois da consulta das irmãs. (`chamados/models.py:2016`)
- A validação já pode fechar a disputa: `validar_pergunta()` chama o materializador, cuja cauda consulta `pode_fechar()` e executa `fechar()`. Recarregar `d` depois de `_validar_pelo_dono` evita agir sobre estado anterior. (`chamados/services/validacao.py:494`, `chamados/services/materializacao.py:1384`, `chamados/services/materializacao.py:1322`)
- Há infraestrutura pronta para teste sem banco: `funcao()` compila a função real por AST; `fonte_real()` admite mutações somente em memória. (`chamados/tests/e4_apoio.py:8`, `chamados/tests/e4_catracas.py:5`)
- Há precedente de `unittest` puro com infraestrutura instrumentada por `patch.dict(sys.modules)`. (`chamados/tests/test_e3_tarefa_admin_sem_banco.py:19`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:82`)
- O teste atual do upload só confere gravação de `Batida.foto` e `FotoBatida`; não cobre fechamento por episódio. (`chamados/tests/test_b8_smoke_portas.py:60`)
- O teste existente de duas batidas na mesma disputa cobre episódios distintos e repetição, mas usa envelope sintético e somente ordem cronológica. (`chamados/tests/test_e1_v6_horizonte.py:257`, `chamados/tests/test_e1_v6_horizonte.py:262`)
- Os testes de atos existentes prendem a chamada `aparar_motivo_respondido` no upload e sua idempotência; mantê-la evita romper esses selos. (`chamados/tests/test_porta_chamado_e3.py:56`, `chamados/tests/test_porta_chamado_e3.py:110`)

## 15/11 00:52 · sessao 8db5c2184c1a · MADRUGADA-PREPARA · sonda · origem=Code

O previsto será calculado por `minutos_previstos_do_dia`; o pagamento diário será lido de `DiaPago`, conferido contra o `FechamentoMensal` gravado que o exportador lê. As rubricas permanecem separadas: somar trabalho, extras e seus subconjuntos produziria dupla contagem. (`app/escala/utils.py:2230`; `app/ponto/services/dia_pago.py:833`; `app/folha/export.py:719`; `app/ponto/services/dia_pago.py:904`)

A remede colada parte de `UNIV` literal; sua reconstrução original não está demonstrada por ela. [nao verificado] A sonda abaixo usa `F.dias_que_seguram`, que delega à retenção real, e depois aplica a seleção da remede com `F._carga`, `porta_da_tarefa` e o prazo do **pai da disputa**. (`app/folha/fora_da_folha.py:109`; `app/folha/fora_da_folha.py:105`; `app/folha/fora_da_folha.py:58`; `app/chamados/services/tarefa_do_dia.py:638`; seleção colada: [nao verificado])

Não identifiquei função real que receba o cenário completo “tornar este dia ausência descontante/abonado/manter” e devolva os dois deltas solicitados; existência de outra API permanece [nao verificado]. As funções próximas leem ausências existentes ou grade já julgada. Portanto, os três cenários imprimem `sem funcao real`; a leitura atual continua disponível em cada dia. (`app/ponto/services/efeito_ausencia.py:28`; `app/folha/export.py:125`; `app/ponto/services/dia_pago.py:364`)

DSR perdido é decidido por semana; reflexo de DSR é lavrado em ajuste mensal sem data. A sonda mostra esse contexto uma vez por colaborador/competência, sem rateá-lo pelos dias. (`app/ponto/motor_calculo_v2.py:2600`; `app/ponto/services/dia_pago.py:348`)

| Campos/saídas consumidos | Quem escreve/produz | Quem lê |
|---|---|---|
| `Empresa.pk`, `dia_inicio_competencia` | `criar_empresa`, `atualizar_empresa`. (`app/colaboradores/services/estrutura.py:13`; `app/colaboradores/services/estrutura.py:18`) | Janela da competência. (`app/ponto/janelas.py:36`; `app/ponto/janelas.py:54`) |
| `Colaborador.pk`, `empresa`, admissão/demissão, cadastro delegado | Portas de criação e gravação do colaborador. (`app/colaboradores/services/colaborador.py:32`; `app/colaboradores/services/colaborador.py:55`; `app/colaboradores/services/colaborador.py:91`) | Universo e retenção. (`app/folha/fora_da_folha.py:138`; `app/folha/export.py:1239`) |
| Dias retornados: `data`; célula: `veredito`, `protestos`, `trabalha` | Retorno produzido pela retenção; veredito/protestos lavrados pela porta da célula. (`app/folha/export.py:1259`; `app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1092`) | `dias_que_seguram` e retenção. (`app/folha/fora_da_folha.py:144`; `app/folha/export.py:1273`) |
| Tarefa: `instancias`, `pergunta_id`, ações `acao/de/porta`, `celula_id`, `colaborador_id`, `dia_do_turno`, `fio` | Juiz da tarefa, em memória. (`app/chamados/services/tarefa_do_dia.py:127`; `app/chamados/services/tarefa_do_dia.py:197`; `app/chamados/services/tarefa_do_dia.py:557`; `app/chamados/services/tarefa_do_dia.py:635`) | Projeção da porta e folha. (`app/chamados/services/tarefa_do_dia.py:638`; `app/folha/fora_da_folha.py:70`) |
| Alimentação: `fila_ids`, `fila_cel`, `fila_dia`; perguntas por colaborador/dia | Carregadores reais. (`app/chamados/juizes.py:1559`; `app/chamados/juizes.py:1589`; `app/chamados/services/tarefa_do_dia.py:395`) | Tarefa. (`app/chamados/services/tarefa_do_dia.py:596`; `app/chamados/services/tarefa_do_dia.py:602`) |
| Pergunta: `pk`, `disputa`, destino; disputa: `chamado`; pai: `prazo_sla` | Nascimento da pergunta/disputa; prazo inicial e renovação pelas portas existentes. (`app/chamados/models.py:2854`; `app/chamados/services/disputa_emissao.py:587`; `app/chamados/services/disputa_emissao.py:581`; `app/chamados/models.py:574`) | Âncora e relógio reais. (`app/chamados/services/fio.py:97`; `app/chamados/catalogo/motor.py:54`) |
| Previsto diário | `minutos_previstos_do_dia`, a partir de vínculo, trabalho, marcos e pausas. (`app/escala/utils.py:2230`; `app/escala/utils.py:2260`; `app/escala/utils.py:2269`; `app/escala/utils.py:2291`) | Motor e fechamento. (`app/escala/utils.py:2239`) |
| `DiaPago`: chaves e **todos os campos de `CAMPOS_DIA`**, `detalhe.folga_feriado_min` | `lavrar`, incluindo mapas da grade; retrato da exportada também atualiza os três campos de minutos. (`app/ponto/services/dia_pago.py:135`; `app/ponto/services/dia_pago.py:257`; `app/ponto/services/dia_pago.py:288`; `app/ponto/services/dia_pago.py:345`; `app/ponto/services/dia_pago.py:481`) | `linhas_do_periodo`, `pago_do_dia`, soma canônica. (`app/ponto/services/dia_pago.py:833`; `app/ponto/services/dia_pago.py:867`; `app/ponto/services/dia_pago.py:550`) |
| `FechamentoMensal`: existência, chaves, campos equivalentes a `CAMPOS_DIA` | Recalculo, persistido por `save/update`. (`app/ponto/services/fechamento.py:898`; `app/ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1160`) | Autoridade mensal do TXT. (`app/folha/export.py:719`; `app/folha/export.py:730`) |
| Semanas DSR; reflexo e banco no ajuste | Fechamento e lavratura do ajuste. (`app/ponto/services/fechamento.py:1101`; `app/ponto/services/fechamento.py:1104`; `app/ponto/services/dia_pago.py:350`) | Soma da competência; o exportador não emite reflexo de DSR como evento. (`app/ponto/services/dia_pago.py:562`; `app/folha/export.py:453`) |
| Competência entregue | Registro/invalidação da exportação. (`app/folha/models.py:174`; `app/folha/models.py:143`) | `competencia_entregue`. (`app/folha/export.py:556`) |
| PK do usuário reservado de sistema | Escritor desse cadastro: [nao verificado] | Preparo de leitura para evitar `get_or_create` nas dependências. (`app/chamados/management/commands/diff_e3_sombra.py:96`; `app/chamados/management/commands/diff_e3_sombra.py:100`) |

Fechamento de **todos** os escritores transitivos das funções delegadas: [nao verificado].

A entrada é `sonda(tsv_texto)`, com o conteúdo integral do TSV recebido; ele só participa da conferência. Essa entrada explícita é necessária porque o envelope monta a árvore e a sonda, sem montar automaticamente os anexos. (`bin/sonda_leitura.sh:217`; `bin/sonda_leitura.sh:238`) Sintaxe analisada em memória, sem executar Django; execução e resultados: [nao verificado]. O relógio reproduz o instante informado, mas a própria tarefa declara que parte do estado consultado continua atual. (`app/chamados/services/tarefa_do_dia.py:374`)

```python
import csv
import datetime as dt
import io
import json
import logging
import re
import sys
from collections import Counter, defaultdict
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from decimal import Decimal, localcontext
from unittest.mock import patch

# Valores do anexo colado: [nao verificado] por arquivo local.
AGORA = dt.datetime.fromisoformat("2026-11-15T02:20:00+00:00")
COMPETENCIAS = ((9, 2026), (10, 2026), (11, 2026))
ESPERADO = 2304
SEM_FUNCAO = "sem funcao real"


class EscritaProibida(BaseException):
    pass


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


def bloquear(*args, **kwargs):
    raise EscritaProibida()


def so_select(execute, sql, params, many, context):
    # Molde: app/chamados/management/commands/diff_e3_sombra.py:30.
    # SET também é recusado; tenant já escolhido pelo envelope.
    s = str(sql).strip()
    if (
        many
        or context["connection"].alias != "default"
        or not re.match(r"^SELECT\b", s, re.I)
        or ";" in s
        or "--" in s
        or "/*" in s
        or re.search(
            r"\b(INSERT|UPDATE|DELETE|MERGE|INTO|CREATE|ALTER|DROP|"
            r"TRUNCATE|COPY|CALL|DO|SET|RESET|GRANT|REVOKE|"
            r"NEXTVAL|SETVAL|PG_ADVISORY_LOCK|PG_TRY_ADVISORY_LOCK|"
            r"PG_ADVISORY_XACT_LOCK|PG_TRY_ADVISORY_XACT_LOCK|"
            r"LO_IMPORT|LO_EXPORT|PG_TERMINATE_BACKEND|PG_CANCEL_BACKEND|"
            r"PG_RELOAD_CONF|SET_CONFIG)\b|\bFOR\s+(SHARE|KEY)\b",
            s, re.I,
        )
    ):
        raise EscritaProibida()
    return execute(sql, params, many, context)


def sonda(tsv_texto):
    """Entrada de conferência: conteúdo INTEGRAL do TSV fornecido."""
    saida = sys.stdout
    erros = Counter()
    candidatos = set()
    universo = {}
    linhas = []
    referencia = set()
    partes = []
    tsv_linhas = 0
    completo = False
    campos = ()

    def emitir(tipo, **dados):
        saida.write(tipo + "|" + json.dumps(
            dados, ensure_ascii=False, sort_keys=True,
            default=str, allow_nan=False,
        ) + "\n")

    def erro(etapa, emp=None, cid=None, dia=None, comp=None):
        erros[etapa] += 1
        emitir(
            "ERRO", etapa=etapa,
            empresa=None if emp is None else f"emp{emp}",
            colab=None if cid is None else f"col{cid}",
            dia=None if dia is None else dia.isoformat(),
            comp=comp,
        )

    def dec(valor):
        if valor is None:
            return None
        n = Decimal(str(valor))
        if not n.is_finite():
            raise ValueError()
        return n

    def somar(rs):
        out = {
            "dias": len(rs),
            "colabs_distintos": len({r["cid"] for r in rs}),
            "estados": dict(sorted(
                Counter(r["estado"] for r in rs).items()
            )),
        }
        for nome in ("previsto_min", "pago_trabalho_min", *campos):
            valores = [
                r.get(nome) for r in rs if r.get(nome) is not None
            ]
            out[nome] = {
                "minutos": (
                    sum(valores, Decimal(0)) if valores else None
                ),
                "dias_com_numero": len(valores),
                "dias_sem_numero": len(rs) - len(valores),
            }
        return out

    nivel = logging.root.manager.disable
    try:
        logging.disable(logging.CRITICAL)
        with ExitStack() as stack, localcontext() as ctx:
            ctx.prec = 50
            stack.enter_context(redirect_stdout(Descartar()))
            stack.enter_context(redirect_stderr(Descartar()))

            from freezegun import freeze_time
            from django.db import connection, connections
            from django.db.models import Model
            from django.db.models.query import QuerySet
            from django.contrib.auth import get_user_model
            from django.utils import timezone
            from chamados.juizes import tranca_em_lote
            from chamados.models import PerguntaDisputa
            from chamados.catalogo.motor import prazo_estourou
            from chamados.services.fio import chamado_que_lista
            from chamados.services.tarefa_do_dia import porta_da_tarefa
            from colaboradores.models import Empresa
            from escala.utils import minutos_previstos_do_dia
            from folha import fora_da_folha as F
            from folha.export import competencia_entregue
            from ponto.janelas import janela_fechamento
            from ponto.models import DiaPago, FechamentoMensal
            from ponto.services.dia_pago import (
                CAMPOS_DIA, linhas_do_periodo, pago_do_dia,
                soma_da_competencia,
            )

            campos = tuple(CAMPOS_DIA)

            # bin/sonda_leitura.sh:237 e :239.
            if connection.settings_dict["NAME"] != "sombra":
                raise ValueError()
            if connection.schema_name != "juliani":
                raise ValueError()

            for alias in connections:
                stack.enter_context(
                    connections[alias].execute_wrapper(so_select)
                )
            for classe, metodos in (
                (Model, ("save", "delete")),
                (QuerySet, (
                    "create", "update", "delete", "_raw_delete",
                    "bulk_create", "bulk_update", "get_or_create",
                    "update_or_create",
                )),
            ):
                for metodo in metodos:
                    stack.enter_context(
                        patch.object(classe, metodo, bloquear)
                    )

            stack.enter_context(freeze_time(AGORA, tick=False))
            stack.enter_context(
                timezone.override("America/Sao_Paulo")
            )

            # TSV somente para conferência, nunca para seleção.
            leitor = csv.DictReader(
                io.StringIO(tsv_texto), delimiter="\t"
            )
            if not {"emp", "comp", "colab", "dia"} <= set(
                leitor.fieldnames or ()
            ):
                raise ValueError()

            for item in leitor:
                tsv_linhas += 1
                e = re.fullmatch(
                    r"(?:emp)?([0-9]+)", item["emp"].strip()
                )
                c = re.fullmatch(
                    r"(?:col)?([0-9]+)", item["colab"].strip()
                )
                m, a = map(int, item["comp"].split("/"))
                if not e or not c or (m, a) not in COMPETENCIAS:
                    raise ValueError()
                referencia.add((
                    int(e[1]), m, a, int(c[1]),
                    dt.date.fromisoformat(item["dia"]),
                ))

            # Preparo real: diff_e3_sombra.py:96.
            sistema_pk = (
                get_user_model().objects.filter(username="sistema")
                .values_list("pk", flat=True).first()
            )
            if sistema_pk is None:
                raise ValueError()
            stack.enter_context(
                tranca_em_lote(sistema_pk=sistema_pk)
            )

            # Universo: fora_da_folha.py:109/:58.
            for emp in Empresa.objects.order_by("pk"):
                for mes, ano in COMPETENCIAS:
                    comp = f"{mes:02d}/{ano}"
                    try:
                        u = F.dias_que_seguram(emp, mes, ano)
                        cs, por = u["colabs"], u["dias"]
                        ini, fim = janela_fechamento(
                            mes, ano, emp
                        )
                        retidos = [
                            cs[c] for c in por if por[c][1]
                        ]
                        if not retidos:
                            continue
                        carga = F._carga(
                            retidos, ini, fim, AGORA
                        )
                    except EscritaProibida:
                        raise
                    except Exception:
                        erro(
                            "carga_universo",
                            emp=emp.pk, comp=comp,
                        )
                        continue

                    for cid, (_motivos, dias) in sorted(por.items()):
                        for registro in dias:
                            dia = registro["data"]
                            chave = (emp.pk, mes, ano, cid, dia)
                            if chave in candidatos:
                                erro(
                                    "dia_duplicado",
                                    emp.pk, cid, dia, comp,
                                )
                                continue
                            candidatos.add(chave)

                            try:
                                colab = cs[cid]
                                t = carga.tarefa(colab, dia)
                                porta = porta_da_tarefa(t)
                                if (
                                    porta is None
                                    or str(porta["fio"]) != "colab"
                                ):
                                    continue

                                alim = carga._portas
                                perg = {
                                    p.pk: p for p in
                                    carga._perg.get((cid, dia), [])
                                }
                                acoes = [
                                    a for i in t["instancias"]
                                    for a in i["acoes"]
                                ]
                                pa = {
                                    a["porta"] for a in acoes
                                    if isinstance(a["porta"], int)
                                } & alim["fila_ids"]
                                cel = set(alim["fila_cel"].get(
                                    t["celula_id"], ()
                                )) & alim["fila_ids"]
                                anc = {
                                    chamado_que_lista(p)
                                    for p in perg.values()
                                } & alim["fila_ids"]
                                fdia = set(
                                    alim.get("fila_dia", {}).get((
                                        t.get("colaborador_id"),
                                        t.get("dia_do_turno"),
                                    ), ())
                                ) & alim["fila_ids"]
                                portas = pa | cel | anc | fdia

                                # Seleção literal da remede colada:
                                # [nao verificado] por arquivo local.
                                # Sem filtro extra de porta para prazo.
                                vencido = False
                                for i in t["instancias"]:
                                    for acao in i["acoes"]:
                                        if (
                                            acao["acao"] == "responder"
                                            and str(acao["de"]).lower()
                                                .startswith("colab")
                                            and i["pergunta_id"]
                                        ):
                                            p = perg.get(
                                                i["pergunta_id"]
                                            )
                                            if p is None:
                                                p = (
                                                    PerguntaDisputa.objects
                                                    .select_related(
                                                        "disputa__chamado"
                                                    )
                                                    .get(pk=i["pergunta_id"])
                                                )
                                            pai = p.disputa.chamado
                                            if (
                                                pai is not None
                                                and pai.prazo_sla
                                                and prazo_estourou(
                                                    pai.prazo_sla, AGORA
                                                )
                                            ):
                                                vencido = True

                                admin = {
                                    a["acao"] for a in acoes
                                    if a["de"] == "admin"
                                    and (
                                        not portas
                                        or a["porta"] in pa
                                    )
                                }
                                if vencido and not admin:
                                    universo[chave] = colab
                            except EscritaProibida:
                                raise
                            except Exception:
                                erro(
                                    "selecao_dia",
                                    emp.pk, cid, dia, comp,
                                )

            completo = not erros
            emitir(
                "CONFERENCIA",
                tsv_li

[truncado em 20000 de 31156 caracteres]

## 15/11 00:50 · sessao 8fb4cbca634a · MADRUGADA-PREPARA · censo · origem=Code

## (A) MATRIZ DE TRANSIÇÃO

Referências conferidas na cópia local; correspondência material com o hash informado: [nao verificado]. O envelope não contém registros individuais de disputa/pergunta; incidência atual em produção: [nao verificado].

**Estados e campos**

| Objeto | Estado/valor | Definição |
|---|---|---|
| Disputa | Aberta: `fechada_em=None`; fechada: instante preenchido. | `app/chamados/models.py:1804` |
| Fechamento | `motivo_fechamento`: `''`, `veredito`, `vazia`, `desligamento`, `admin_encerra`, `competencia_trancada`. `''` também admite legado. | `app/chamados/models.py:1827`; `app/chamados/models.py:1840` |
| Complemento do fechamento | `fechada_por`: autor ou nulo; `resolucao`: texto ou `''`. | `app/chamados/models.py:1813`; `app/chamados/models.py:1820` |
| Pergunta | Rótulos **derivados**, sem coluna de status: `pendente`, `respondida`, `validada`, `resolvida`. | `app/chamados/models.py:2694`; `app/chamados/models.py:3117` |
| Projeção da pergunta | `resposta_colab=''`/texto; `respondida_em` e `validada_em` nulos/preenchidos; `materializada_em` nulo/preenchido. | `app/chamados/models.py:2525`; `app/chamados/models.py:3502` |
| Precedência do rótulo | Validação preenchida resulta em `validada`, ou `resolvida` com materialização; nos demais casos, consulta C6 e resposta/carimbo. | `app/chamados/models.py:3124` |
| Vitalidade | `viva` é outro resultado: delega a `pergunta_viva`; não equivale ao rótulo `pendente`. | `app/chamados/models.py:3473`; `app/chamados/juizes.py:1399` |
| Via | `''`, `declaracao`, `completude_turno`, `fantasma`, `sem_turno_aberto`, `juiz_aprovou`, `admin_decidiu`, `paridade_recusada`, `celula_absolveu`, `cadastro_folga`, `adesao`, `fato_ocupado`, `fato_materializado`, `chamado_encerrado`, `duplicata`, `ausencia_dia`, `legado_sem_lastro`, `competencia_trancada`, `motivo_retratado`. | `app/chamados/models.py:2546`; `app/chamados/models.py:2562`; `app/chamados/models.py:2571`; `app/chamados/models.py:2639` |
| Fora do fato | Conjunto `duplicata`, `motivo_retratado`; as portas impedem retorno dessas linhas ao fato. | `app/chamados/models.py:2471`; `app/chamados/models.py:3263`; `app/chamados/models.py:3289` |
| Destino/dona | `chamado_destino`: FK nula/preenchida, `SET_NULL`; `disputa`: FK obrigatória, `CASCADE`. | `app/chamados/models.py:3494`; `app/chamados/models.py:2494` |
| Identidade | Colaborador, instância de marco, conversa e episódio; dia/célula identificam o fato datado. | `app/chamados/models.py:2491`; `app/chamados/models.py:2505`; `app/chamados/models.py:2518` |
| Lavra | `veredito_lavrado`: `''`, `sim`, `nao`, `indeterminado`; acompanha `julgado_em` e `janela_expira_em`. | `app/chamados/models.py:2655`; `app/chamados/models.py:2663`; `app/chamados/models.py:2668` |
| Trilha | Oito atos: `resposta`, `validacao`, `devolucao`, `recusa`, `reabertura`, `silencio`, `retratacao`, `absorcao`. | `app/chamados/models.py:4000` |
| Auxiliares | Contadores administrativos/colaborador; conferência de data; pendência administrativa e motivo de recusa. Não constituem novos rótulos da pergunta. | `app/chamados/models.py:1805`; `app/chamados/models.py:2536`; `app/chamados/models.py:2636` |

Para identificar os leitores nas matrizes:

| Referência | Leitor real |
|---|---|
| **APP** | `perguntas_do_app` e `particao_do_app`: viva sem resposta em dona aberta entra em pendentes; demais vivas entram no histórico. `app/chamados/services/pergunta_no_app.py:45`; `app/chamados/services/pergunta_no_app.py:76` |
| **ESTADO/C6** | `PerguntaDisputa.estado`, `pergunta_viva`/`_pergunta_viva_calc`. `app/chamados/models.py:3117`; `app/chamados/juizes.py:862`; `app/chamados/juizes.py:1399` |
| **VALIDAÇÃO** | `validar_pergunta`. `app/chamados/services/validacao.py:418` |
| **MATERIALIZAÇÃO** | `materializar_perguntas_validadas_da_disputa`. `app/chamados/services/materializacao.py:1344` |
| **HISTÓRICO** | `PerguntaDisputa.respostas_anteriores`. `app/chamados/models.py:3643` |
| **LAVRA** | `esta_stale` e leitores do raio X. `app/chamados/veredito_lavrado.py:224`; `app/chamados/services/raiox.py:136` |
| **RAIZ** | `colaborador_em_disputa`, signal do chamado-pai, relatório e motor. `app/chamados/services/disputa_emissao.py:34`; `app/chamados/signals.py:29`; `app/chamados/relatorios_disputa.py:11`; `app/ponto/services/disputa.py:31` |

**Transições da disputa**

| Transição | Escritor — arquivo::função:linha | Guarda conferida | Quem lê |
|---|---|---|---|
| **N1 — inexistente → aberta, núcleo** | `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa:587` | Trava universo do colaborador; procura aberta; cria na ausência. `app/chamados/services/disputa_emissao.py:530`; `app/chamados/services/disputa_emissao.py:531` | APP, RAIZ. |
| **N1 — inexistente → aberta, repasse** | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1123` | Exclui dias da lista administrativa; filtra regularizáveis/perguntas existentes; procura aberta. `app/chamados/services/disputa_emissao.py:1047`; `app/chamados/services/disputa_emissao.py:1086`; `app/chamados/services/disputa_emissao.py:1096` | APP, RAIZ. |
| **N1 — inexistente → aberta, âncora** | `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1358` | Categoria `ancora_limbo`, chamado não encerrado, sem pergunta anterior não materializada; procura aberta. `app/chamados/services/disputa_emissao.py:1316`; `app/chamados/services/disputa_emissao.py:1323`; `app/chamados/services/disputa_emissao.py:1332` | APP, RAIZ. |
| **N2 — aberta A → aberta A∪B** | Núcleo `::_abrir_ou_agregar_disputa:565`; repasse `::criar_disputa_a_partir_de_chamados_orfaos:1132`, `:1161`, `:1256`; âncora `::criar_disputa_ancora_de_chamado:1367`, todos em `app/chamados/services/disputa_emissao.py`. | Núcleo compara conjuntos e salva campos alterados; repasse/âncora agregam em ramos próprios. `app/chamados/services/disputa_emissao.py:537`; `app/chamados/services/disputa_emissao.py:564`; `app/chamados/services/disputa_emissao.py:1131`; `app/chamados/services/disputa_emissao.py:1366` | Gerador e relatório. `app/chamados/services/disputa_emissao.py:300`; `app/chamados/relatorios_disputa.py:11` |
| **N3 — fechada A → continua fechada, com complemento B** | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1084` | Seleciona pergunta sem materialização **sem exigir dona aberta**; exige dia, ata e complemento criado. `app/chamados/services/disputa_emissao.py:1070`; `app/chamados/services/disputa_emissao.py:1077`; `app/chamados/services/disputa_emissao.py:1081` | APP envia viva dessa dona ao histórico; vigia mede a classe. `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/vigia_fio.py:758` |
| **F1 — aberta → fechada `veredito`/`vazia`** | `app/chamados/models.py::DisputaSupervisao.fechar:1996`; persistência `app/chamados/models.py:2000`. | Autor; nenhuma resposta sem veredito; nenhuma viva. Zero perguntas escolhe `vazia`, demais `veredito`. `app/chamados/models.py:1948`; `app/chamados/models.py:1950`; `app/chamados/models.py:1960`; `app/chamados/models.py:1967` | APP, signal do pai; contador de vazias. `app/chamados/services/vigia_fio.py:748` |
| **F2 — aberta → fechada `desligamento`** | Mesmo `fechar:1996`; origem `app/colaboradores/services/desligamento.py:167`. | Exceção nomeada, autor e recusa de resposta sem veredito. Vivas são encaminhadas a `fechar_por_admin`, com ato `silencio`, antes do fechamento. `app/chamados/models.py:1950`; `app/chamados/models.py:1982`; `app/chamados/models.py:1988` | APP, ESTADO/C6, signal do pai. |
| **F3 — aberta → fechada `admin_encerra`** | `app/chamados/models.py::DisputaSupervisao.admin_encerra:2014` → `fechar:1996`. | Motivo escrito obrigatório; mantém guardas de autor/resposta sem veredito; trata vivas no mesmo ramo de F2. `app/chamados/models.py:2011`; `app/chamados/models.py:1950`; `app/chamados/models.py:1982` | APP, ESTADO/C6, signal do pai. |
| **F4 — aberta → fechada `competencia_trancada`** | Mesmo `fechar:1996`; origem `app/chamados/services/competencia_trancada.py::encerrar:229`. | Chamador exige `pode_fechar`; porta exige autor e ausência de resposta sem veredito; esse motivo não entra no carimbo das mudas. `app/chamados/services/competencia_trancada.py:227`; `app/chamados/models.py:1950`; `app/chamados/models.py:1982` | APP, RAIZ; C6 lê a tranca. `app/chamados/juizes.py:1405` |
| **R1 — fechada → aberta, admin** | `app/chamados/models.py::DisputaSupervisao.reabrir_admin:2213`; save `app/chamados/models.py:2225`. | Fechada, contador menor que dois, autor; casca exige perfil, motivo válido e justificativa de ao menos 20 caracteres. `app/chamados/models.py:2202`; `app/chamados/models.py:2204`; `app/chamados/models.py:2206`; `app/chamados/services/acoes_disputa.py:245`; `app/chamados/services/acoes_disputa.py:253` | APP; procura de aberta pelo emissor. `app/chamados/services/disputa_emissao.py:531` |
| **R2 — fechada → aberta, sistema** | `app/chamados/models.py::DisputaSupervisao.reabrir_sistema:2237`; save `app/chamados/models.py:2241`. | Só exige fechada; aberta retorna `False`; não incrementa contador. `app/chamados/models.py:2231`; `app/chamados/models.py:2241` | APP, RAIZ. |
| **R3 — aberta/corrigível → aberta/questionário pendente** | `app/chamados/models.py::DisputaSupervisao.reabrir_por_colab:2301`; contador/save `app/chamados/models.py:2303`. | Autor, dona aberta, contador menor que dois e perguntas corrigíveis; delega limpeza à correção. `app/chamados/models.py:2282`; `app/chamados/models.py:2288`; `app/chamados/models.py:2292`; `app/chamados/models.py:2301` | APP, VALIDAÇÃO. |
| **M1 — aberta A → aberta A−motivo; último motivo encaminha F1** | `app/chamados/models.py::DisputaSupervisao.remover_motivo:2127`; save `app/chamados/models.py:2129`; fecho `app/chamados/models.py:2136`. | Autor; releitura sob trava; dona aberta; motivo presente. Conserva perguntas com retratação e via de identidade. `app/chamados/models.py:2095`; `app/chamados/models.py:2100`; `app/chamados/models.py:2120`; `app/chamados/models.py:2125` | ESTADO/C6, gerador e relatório. |
| **M2 — com motivo de foto → sem esse motivo** | `app/chamados/models.py::DisputaSupervisao.aparar_motivo_respondido:2032`. | Motivo presente; remove motivo/contexto. Fechamento é outro ato do chamador. `app/chamados/models.py:2025`; `app/chamados/models.py:2029`; `app/api/foto_recebida.py:80` | Relatório e fechamento. |
| **Aberta/resolução residual → aberta/`resolucao=''`** | `app/chamados/models.py::DisputaSupervisao.limpar_resolucao_residual:2329`; save `app/chamados/models.py:2330`. | Aberta e texto não vazio. `app/chamados/models.py:2326` | Signal do fechamento. `app/chamados/signals.py:46` |
| **Contexto anterior → contexto anotado/renovado; abertura preservada** | `app/chamados/models.py::DisputaSupervisao.anotar_contexto:2169`; `app/chamados/models.py::DisputaSupervisao.renovar_contexto:2187`. | Anotar exige chave sem valor; renovar exige valor diferente. `app/chamados/models.py:2164`; `app/chamados/models.py:2182` | Censo completo dos leitores por chave: [nao verificado]. |
| **Contexto → contexto com dia de entrada pendente** | `app/chamados/services/disputa_emissao.py::perguntar_entradas_pendentes:296`. | Classificador retornou motivo e a geração criou pergunta. `app/chamados/services/disputa_emissao.py:288`; `app/chamados/services/disputa_emissao.py:293`; `app/chamados/services/disputa_emissao.py:295` | Gerador da própria operação. `app/chamados/services/disputa_emissao.py:294` |

**Transições da pergunta**

| Transição | Escritor — arquivo::função:linha | Guarda conferida | Quem lê |
|---|---|---|---|
| **P1 — inexistente → pergunta** | `app/chamados/models.py::PerguntaDisputa._nascer:3095`; insert `app/chamados/models.py:3099`. | Contrato de nascimento, dia do turno, identidade/dedup, destino válido e par resposta/instante permitido. `app/chamados/models.py:2917`; `app/chamados/models.py:2939`; `app/chamados/models.py:2964`; `app/chamados/models.py:2983` | ESTADO/C6, APP. |
| **P2 — canônica → mesma pergunta, eventualmente outra célula/destino** | Mesma `_nascer`: destino `app/chamados/models.py:3065`; save `app/chamados/models.py:3089`. | Canônica do fato; destino válido; costura não mata fio. Conserva a dona encontrada. `app/chamados/models.py:3004`; `app/chamados/models.py:3059`; `app/chamados/models.py:3063` | ESTADO/C6, APP, LAVRA. |
| **P3 — pendente/respondida/validada → nova resposta** | `app/chamados/services/disputa_emissao.py::responder_pergunta_disputa_core:1467`; save `app/chamados/services/disputa_emissao.py:1508`. | Texto/formato/opções por tipo; limpa validação anterior e pode desfazer carimbo sem lastro; registra resposta. `app/chamados/services/disputa_emissao.py:1432`; `app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/disputa_emissao.py:1511` | ESTADO/C6, VALIDAÇÃO, HISTÓRICO. |
| **P4 — sem texto → resposta automática** | `app/chamados/services/disputa_emissao.py::responder_automaticamente:1662`. | Não sobrescreve texto existente; recusa par resposta/instante ilegal; registra resposta. `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1657`; `app/chamados/services/disputa_emissao.py:1663` | ESTADO/C6, VALIDAÇÃO, HISTÓRICO. |
| **P4 — texto sem instante → resposta com instante** | `app/chamados/services/disputa_emissao.py::carimbar_resposta_pela_trilha:1683`. | Exige texto e instante ausente; registra ato. `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1684` | Mesmos leitores anteriores. |
| **P5 — respondida → validada; com carimbo, resolvida** | `app/chamados/services/validacao.py::_carimbar_validacao:159`; save `app/chamados/services/validacao.py:162`. | Chamador exige resposta, ausência de validação, plausibilidade e tratamento de folga; segue materialização. `app/chamados/services/validacao.py:418`; `app/chamados/services/validacao.py:421`; `app/chamados/services/validacao.py:429`; `app/chamados/services/validacao.py:437`; `app/chamados/services/validacao.py:487` | ESTADO/C6, MATERIALIZAÇÃO, HISTÓRICO. |
| **P6 — validação recusada → respondida aguardando admin** | `app/chamados/services/validacao.py::devolver_validacao_recusada:224`; save `app/chamados/services/validacao.py:227`. | Resposta de hora, sem materialização, recusa com motivo; conserva resposta. Pode chamar R2, condicionado a `reabrir`. `app/chamados/services/validacao.py:215`; `app/chamados/services/validacao.py:217`; `app/chamados/services/validacao.py:242` | ESTADO/C6, VALIDAÇÃO, HISTÓRICO. |
| **P7 — respondida não validada → pendente, recusa humana** | Motivo: `app/chamados/services/validacao.py::recusar_pergunta:625`; ciclo delegado à correção em `app/chamados/services/validacao.py:628`. | Motivo obrigatório; recusa validada e fora do fato; pendente é no-op. `app/chamados/services/validacao.py:591`; `app/chamados/services/validacao.py:602`; `app/chamados/services/validacao.py:605`; `app/chamados/services/validacao.py:609` | APP, ESTADO/C6, HISTÓRICO. |
| **P8 — respondida/validada/carimbada → pendente, correção** | `app/chamados/services/disputa_emissao.py::reabrir_para_correcao:1634` delega a `app/chamados/models.py::PerguntaDisputa.descarimbar_em_lote:3363`. | Limpa resposta, validação **e carimbo**; exclui fora do fato; trilha só para linhas alteradas do fato. `app/chamados/services/disputa_emissao.py:1595`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/models.py:3343`; `app/chamados/models.py:3363` | APP, ESTADO/C6, HISTÓRICO. |
| **P9 — sem carimbo → carimbo de fato** | `app/chamados/models.py::PerguntaDisputa.carimbar:3235`; atribuição central `app/chamados/models.py:3180`. | Recusa via de identidade/fora do fato; aplica pós-estado e exige C6 morto; restaura memória na recusa. Pode completar resposta/validação com texto. `app/chamados/models.py:3201`; `app/chamados/models.py:3215`; `app/chamados/models.py:3222`; `app/chamados/models.py:3227` | ESTADO/C6, LAVRA. |
| **P10 — pergunta → carimbo de identidade** | `app/chamados/models.py::PerguntaDisputa.carimbar_identidade:3273`. | Via pertencente à lista de identidade; fora do fato não retorna ao fato. Não consulta C6 como guarda dessa porta. `app/chamados/models.py:3261`; `app/chamados/models.py:3263` | ESTADO/C6, identidade/dedup. |
| **P11 — carimbada → sem carimbo** | `app/chamados/models.py::PerguntaDisputa.descarimbar:3295`; lote `app/chamados/models.py::PerguntaDisputa.descarimbar_em_lote:3363`. | Individual veta fora do fato; lote também exclui essas linhas, exige campos permitidos e protege par resposta/instante. `app/chamados/models.py:3289`; `app/chamados/models.py:3338`; `app/chamados/models.py:3343`; `app/chamados/models.py:3358` | ESTADO/C6, APP. |
| **P12 — carimbo de outra via → `fato_materializado`** | `app/chamados/models.py::PerguntaDisputa.encerrar_por_fato_no_espelho:3375`. | Chamador prova marco no espelho; porta veta via já igual ou fora do fato. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; `app/chamados/models.py:3372` | ESTADO/C6, lastro. `app/chamados/juizes.py:689` |
| **P13 — via derivada antiga → via atual + lavra** | `app/chamados/services/relavra_via.py::relavrar:127`. | Via derivada; calcula `via_de_hoje`; só escreve se mudou. `app/chamados/services/relavra_via.py:118`; `app/chamados/services/relavra_via.py:120`; `app/chamados/services/relavra_via.py:121` | ESTADO/C6, LAVRA. |
| **V1 — lavra vazia/antiga → `sim`/`nao`/`indeterminado`** | `app/chamados/veredito_lavrado.py::lavrar:270`; save `app/chamados/veredito_lavrado.py:275`. | Juiz do lastro decide; exceção produz `indeterminado`; grava também relógio/janela. `app/chamados/veredito_lavrado.py:262`; `app/chamados/veredito_lavrado.py:269`; `app/chamados/veredito_lavrado.py:272` | LAVRA. |
| **V2 — lavra divergente → lavra de backfill** | Cálculo/atribuições: `app/chamados/management/commands/backfill_veredito_lavrado.py::Command.handle:138`; persistência `app/chamados/veredito_lavrado.py::gravar_lavras_em_lote:250`. | Divergência de veredito; não substitui preenchido por indeterminado decorrente de falha. `app/chamados/management/commands/backfill_veredito_lavrado.py:120`; `app/chamados/management/commands/backfill_veredito_lavrado.py:124` | LAVRA. |
| **T1 — ato inexistente → nova linha de decisão** | `app/chamados/models.py::DecisaoPergunta.registrar:4096`. | Lista vazia não escreve; linha existente não admite edição/apagamento. `app/chamados/models.py:4090`; `app/chamados/models.py:4047`; `app/chamados/models.py:4051` | HISTÓRICO. |
| **I1 — identidade histórica vazia → preenchida** | `app/chamados/migrations/0051_decisao_pergunta.py::_preencher_chave:25`; `app/chamados/migrations/0051_decisao_pergunta.py:27`. | Mapeamento congelado acionado por `RunPython`; separado das portas de produção. `app/chamados/migrations/0051_decisao_pergunta.py:22`; `app/chamados/migrations/0051_decisao_pergunta.py:101` | Identidade/dedup. `app/chamados/models.py:3568` |
| **Destino nulo/anterior → destino recebido, nascimento/reuso** | `app/chamados/models.py::PerguntaDisputa._nascer:3065`; constructor `app/chamados/models.py:3095`. | Destino válido; no reuso, costura não mata fio. `app/chamados/models.py:2964`; `app/chamados/models.py:3063` | ESTADO/C6, materializador, LAVRA. |
| **Destino anterior → novo destino, reaponte** | `app/chamados/services/fio.py::reapontar_resolvedora:964`; save `app/chamados/services/fio.py:966`. | `resolvedora_valida` e `_costura_nao_mata_o_fio`; lavra no mesmo save. `app/chamados/services/fio.py:946`; `app/chamados/services/fio.py:948`; `app/chamados/services/fio.py:965` | Mesmos leitores anteriores. |
| **Destino preenchido → nulo por exclusão do chamado** | Ação referencial `SET_NULL`; sem função escritora Python. `app/chamados/models.py:3494` | Exclusão do chamado referenciado. `app/chamados/models.py:3496` | ESTADO/C6, mater

[truncado em 20000 de 42751 caracteres]

## 15/11 00:49 · sessao 2b5f0dfe31a1 · MADRUGADA-PREPARA · leitura · origem=Code

A incompatibilidade central **SEGUE**: o retro entrega `foto_ausente.batida_id`; o gerador e o derivador de origem procuram `ultima_batida.id`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:3606`)

O vínculo Git desta cópia ao hash solicitado não pôde ser confirmado: `git rev-parse HEAD` não reconheceu repositório. As referências abaixo são das linhas lidas no diretório fornecido. **[nao verificado quanto à identidade Git e à comparação histórica entre commits]** [nao verificado]

## RECONFERÊNCIA DAS CITAÇÕES

As ocorrências repetidas entre cef, matriz e r.r estão reunidas nas tabelas. **MUDOU** indica referência ou enunciado que precisa de atualização; não certifica quando ocorreu a alteração. **[nao verificado quanto ao histórico]**

### Referências que MUDARAM

| Citação dos anexos | Referência atual | Conferência |
|---|---|---|
| `models.py:2437` — r.r | `app/chamados/models.py:2514` | Contrato da coluna de episódio BATIDA; campo em `app/chamados/models.py:2518`. |
| `models.py:2784` — r.r | `app/chamados/models.py:2861` | Pergunta datada recebe dia do turno; conversa tem porta própria. |
| `models.py:2795`, `:2802` — r.r | `app/chamados/models.py:2872`; `app/chamados/models.py:2879` | `abrir_conversa`; grava dia civil local do pedido. |
| `models.py:2850-2859` — matriz | `app/chamados/models.py:2872` | Nascimento da conversa; `conversa=True` e `localdate()` em `app/chamados/models.py:2879`. |
| `models.py:2867` — r.r | `app/chamados/models.py:2949` | `_nascer` recebe ou deriva episódio antes de perguntar o dia. |
| `models.py:2876` — r.r | `app/chamados/models.py:2958` | Consulta `dia_do_turno_da_linha`, passando o dia pedido como insumo. |
| `models.py:2965-2976` — matriz | `app/chamados/models.py:2992` | A recusa citada é de episódio já representado por linha recusada em outro dia; não é recusa genérica de qualquer dia pedido divergente. O nascimento toma o dia do juiz. (`app/chamados/models.py:2938`) |
| `models.py:2982` — matriz | `app/chamados/models.py:3009` | Recusa de episódio retratado antes do reuso. |
| `models.py:3450` — r.r | `app/chamados/models.py:3592` | `_chave_de_identidade` deriva origem quando não é conversa e a origem está ausente. |
| `models.py:3453` — r.r | `app/chamados/models.py:3595` | Definição de `batida_origem_do_nascimento`. |
| `models.py:3457` — cef/r.r | `app/chamados/models.py:3599` | Advertência sobre contexto fundido; ele pode apontar outra anomalia e não recompõe a origem do acervo. |
| `models.py:3462-3466`, particularmente `:3464` — cef/r.r | `app/chamados/models.py:3603`; `app/chamados/models.py:3606` | Família BATIDA e leitura de `ultima_batida.id`. |
| `models.py:3468` — r.r | `app/chamados/models.py:3610` | Exige batida pertencente ao colaborador da disputa. |
| `models.py:3490` — cef/r.r | `app/chamados/models.py:3631` | Índice por episódio e motivo; parcial, condicionado a horizonte, origem preenchida, ausência de conversa e vias admitidas. |
| `models.py:3508-3523` — matriz | `app/chamados/models.py:3595` | Derivador completo: família, envelope, existência e posse da batida. |
| `models.py:3537-3550` — matriz | `app/chamados/models.py:3624` | Índices por família; condição do episódio em `app/chamados/models.py:3633`. |
| `models.py:3604` — r.r | `app/chamados/models.py:3746` | Literal do catálogo legado; não é o envelope da disputa. |
| `models.py:4094` — r.r | `app/chamados/models.py:4236` | Reexportação da porta e do gerador. |
| `core/juizes.py:208` — r.r | `app/core/juizes.py:212` | Registro de `turnos_do_colab`; a antiga linha aponta hoje outro registro. |
| `core/juizes.py:1199` — r.r | `app/core/juizes.py:1205` | Registro de `selfie_ausente`. |
| `docs/CORTES.md:283` — r.r | `app/docs/CORTES.md:334` | Corte integral da selfie; também resumido em `app/docs/CORTES.md:94`. |
| `docs/LEIS.md:152` como L-114 — r.r | `app/docs/LEIS.md:153` | Certificado, impacto e termômetro. |
| `docs/LEIS.md:153` como L-113 — r.r | `app/docs/LEIS.md:154` | Nascimento permitido em exportada; regime de alteração do gravado continua distinto. |
| `BACKLOG.md:2109` — matriz | `app/docs/BACKLOG.md:2166` | Mesmo desencontro entre envelope emitido e consumido; texto atualizado com aval posterior. |

### Referências que SEGUEM

Em cada linha abaixo, as referências dos anexos permanecem no mesmo arquivo, linha e significado indicado. As referências relativas da matriz foram expandidas para `app/`. (`app/config/crons.py:686`; `app/chamados/services/disputa_emissao.py:300`)

| Arquivo e citações conferidas — SEGUE | Significado conservado |
|---|---|
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:45`, `:65`, `:66-69` | Janela desde agora, padrão de três dias de duração, bordas inclusivas, origem de aparelho e exclusão de retratadas. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:70-77` | Duas fontes de foto; delegação ao juiz; minutos reais desde a batida. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:81`, `:89-92` | Cabeçalho temporal e retorno anterior à emissão quando não há `--apply`. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:95-106`, especialmente `:96`, `:98-102` | Emissão com `foto_ausente`, sem `ultima_batida`; PK, tipo, instante local do fato e proveniência. |
| `app/ponto/registro_batida.py:72`, `:75-77`, `:80-91` | PWA zero; APK/NULL noventa minutos; override preserva PWA; comparação `>=`. A lista de origens começa em `app/ponto/registro_batida.py:71`. |
| `app/config/crons.py:686` | Cron `26 7 * * *`, retro com `--apply`. |
| `app/api/views.py:447-453`, `:507`, `:655` | Juiz da selfie no ato; obtenção de instante efetivo; entrega desse instante à porta da batida. |
| `app/api/views.py:758-766` | Envelope com PK, tipo e `timestamp=agora`; chamada da porta. |
| `app/api/views.py:833-859` | Upload: definição em `app/api/views.py:835`, anexação em `:847`, resolução em `:857`, resposta em `:859`. O intervalo citado continua correto. |
| `app/api/views_core.py:439`, `:752-758` | Mesmo formato do envelope na API gêmea. |
| `app/ponto/services/triagem_batida.py:735`, `:739` | Emissão de `saida_sem_entrada`; dia do juiz em `app/ponto/services/triagem_batida.py:740`. |
| `app/chamados/services/fabrica_da_celula.py:71`, `:139`, `:172`, `:174` | Motivos pelos marcos da ata; transporte de dia; chamadas da porta e do gerador. |
| `app/chamados/services/declaracao_texto.py:42` | Tabela de motivo por posição e tipo de marco. |
| `app/chamados/management/commands/smoke_chamados.py:103` | Chamada direta do gerador. |
| `app/chamados/services/disputa_emissao.py:72`, `:294`, `:300`, `:325-329` | Referência de batida para grade; chamada e definição do gerador; dedup de topo quando falta episódio. |
| `app/chamados/services/disputa_emissao.py:371-377`, `:387-392` | Primeiro lê `motivo.data`; depois procura episódio e pergunta seu dia ao juiz. |
| `app/chamados/services/disputa_emissao.py:436-441`, `:470-471` | Dedup sequencial de conversa, nascimento e retorno das perguntas. |
| `app/chamados/services/disputa_emissao.py:535-566`, especialmente `:548`, `:552-558`, `:560-565` | Agregação do contexto, contador, instante do merge, substituição das demais chaves de topo e persistência. |
| `app/chamados/services/disputa_emissao.py:568-603`, especialmente `:568`, `:587-592`, `:597` | Nascimento do chamado e da disputa; contexto inicial e trilha. |
| `app/chamados/services/disputa_emissao.py:634-688`, especialmente `:659`, `:664`, `:670`, `:682` | Porta pública, motivos sem repetição, descrição de origem e chamadas do gerador. |
| `app/api/foto_recebida.py:28-54`, `:57-85` | Resposta automática pela porta, validação pelo dono, seleção por última batida e tratamento coletivo das perguntas. |
| `app/chamados/juizes.py:138`, `:141-168`, `:226` | Último degrau temporal, compartilhado entre singular e lote. A entidade de entrada precisa da correção descrita abaixo. |
| `app/ponto/turnos.py:2927-2936`, `:2955` | Dia do turno da batida; não reclamada usa dia local; retratada fica fora do universo. |
| `app/chamados/catalogo/motivos.py:16`, `:35`, `:43`, `:53` | Identidade BATIDA por episódio; motivo da triagem MARCO; família pelo catálogo. |
| `app/chamados/catalogo/query.py:45-66`; `app/chamados/catalogo/perguntas.py:93-98` | Interpolação de data quando o texto a pede; texto de foto permanece escolha sem `{data}`. |
| `app/chamados/tests/test_e1_v5_familia.py:25`, `:98` | Selo da declaração de família e das chaves dos índices; não prende o envelope dos emissores. |
| `CLAUDE.md:142`, `:143`, `:146`, `:148`, `:178`, `:179`, `:411` | Zonas invioláveis, fuso, previsto, idempotência, separação dos donos e regime do gravado exportado. |
| `app/docs/LEIS.md:107`, `:136`, `:145` | Fuso; casos pela regra antes do código; horizonte e backfill único. |
| `app/docs/JUIZES.md:14`, `:20`, `:38` | Prevalência do vivo, juiz do turno e dono da validação. |
| `app/colaboradores/services/ficha.py:215`; `app/escala/services/furos_diarios.py:281`; `app/relatorios/views.py:1675` | Homônimos de ficha/relatório, separados do envelope da disputa; a ficha tem escritor em `app/colaboradores/services/ficha.py:147`. |

### Testes citados: símbolo SEGUE, alcance conferido [nao verificado]

Os métodos citados existem nos sítios abaixo. Execução e resultado dos testes: **[nao verificado]**. [nao verificado]

| Citação da matriz/preparo | Local atual e alcance lido |
|---|---|
| A1 — dry-run | `app/ponto/tests/test_bug126_selfie_invisivel.py:103`: verifica ausência de disputa, não ausência de toda escrita possível. |
| A3 — origem retroativa | `app/ponto/tests/test_bug126_selfie_invisivel.py:91`: exclui origem retroativa; não demonstra o caso `retratada_em`. |
| A4 — manual e origem do juiz | `app/ponto/tests/test_bug126_selfie_invisivel.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:45`. |
| A5 — FotoBatida | `app/ponto/tests/test_bug126_selfie_invisivel.py:74`; `app/ponto/tests/test_selfie_ausente_juiz.py:103`. |
| A6 — Batida.foto | `app/ponto/tests/test_selfie_ausente_juiz.py:112`. |
| A7 — prazo e canal NULL | `app/ponto/tests/test_bug126_selfie_invisivel.py:84`; `app/ponto/tests/test_selfie_ausente_juiz.py:41`. |
| A8 — retro elegível | `app/ponto/tests/test_bug126_selfie_invisivel.py:65`: confere `batida_id`/`_por` em `app/ponto/tests/test_bug126_selfie_invisivel.py:71`; não confere pergunta ou episódio. |
| A9 — agregação sobre PWA | `app/ponto/tests/test_selfie_ausente_juiz.py:119`: confere disputas; payload sintético também sem `ultima_batida` em `app/ponto/tests/test_selfie_ausente_juiz.py:122`. |
| A10 — repetição | `app/ponto/tests/test_bug126_selfie_invisivel.py:108`: asserção conta disputas em `app/ponto/tests/test_bug126_selfie_invisivel.py:113`. |
| A12 — API PWA | `app/api/tests/test_api_bater_pwa.py:64`, `:78`, `:95`: motivo, contador e foto; não identidade da pergunta. |
| A13 — prazo delegado | `app/ponto/tests/test_selfie_ausente_juiz.py:82`. |
| B1 — conversa sem envelope | `app/chamados/tests/test_a2_data_evento_batida.py:53`: motivo diferente de foto; data civil conferida em `app/chamados/tests/test_a2_data_evento_batida.py:56`. |
| B4 — corrida no índice | `app/chamados/tests/test_e1_v5_indice.py:69`: inserção crua por episódio, usando GPS. |
| B5 — episódios distintos e repetição | `app/chamados/tests/test_e1_v5_indice.py:84`; `app/chamados/tests/test_a2_data_evento_batida.py:22`. |
| B6 — segunda batida | `app/chamados/tests/test_e1_v6_horizonte.py:262`: contexto sintético; não executa o retro. |
| B9 — texto de foto sem data | `app/chamados/tests/test_cenarios_disputa.py:43`. |
| B10 — conversa fora do índice | `app/chamados/tests/test_e1_v5_indice.py:96`: permissividade do índice, não dedup sequencial do gerador. |
| C1/C4 — upload | `app/chamados/tests/test_b8_smoke_portas.py:60`: confere Batida.foto e FotoBatida em `app/chamados/tests/test_b8_smoke_portas.py:68`; não confere fecho. |
| C2 — aparar motivo | `app/chamados/tests/test_porta_chamado_e3.py:56`, `:58`, `:62`: porta direta e repetição; não upload. |
| D2 — selo da selfie | `app/ponto/tests/test_selfie_ausente_juiz.py:130`: AST nas APIs; não demonstra contrato de episódio do retro. |

“SEM PROVA” não foi convertido em inexistência universal de teste: cobertura indireta ou dinâmica de todos esses cenários permanece **[nao verificado]**. Há teste de conversa com episódio retratado usando GPS, sem demonstrar especificamente foto. (`app/chamados/tests/test_e1_v6_horizonte.py:445`)

### SUMIU / fontes ausentes

Nenhuma das funções citadas acima SUMIU; seus sítios atuais constam das tabelas, incluindo `app/chamados/models.py:3595`, `app/chamados/services/disputa_emissao.py:300` e `app/chamados/services/disputa_emissao.py:634`.

**SUMIU da cópia disponível — existência histórica [nao verificado]:** o caminho fonte `logs/PREPARACAO/censos/CONTRATO-DE-ENTRADA-DO-FIO/r.r`. Os textos cef/matriz/q.q/r.r/aval foram conferidos pelo envelope, sem arquivo local numerado. **[nao verificado quanto à proveniência material desses anexos]**

`backlog_4obras.txt` e `arquivos_em_voo.txt`, referidos em q.q/r.r, não estão disponíveis no envelope nem como arquivos desta cópia. O cruzamento histórico de raias e a conclusão “arquivos em comum = 0” ficam **[nao verificado]**.

### Correções de significado da matriz

| Trecho | Estado da afirmação |
|---|---|
| A1–A7, A12–A13 | **SEGUE** o comportamento de entrada descrito; os limites das provas estão na tabela de testes. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66`; `app/ponto/registro_batida.py:80`; `app/api/views.py:447`) |
| A8–A9 | **SEGUE** o envelope incompatível e a preservação de chave antiga ausente na emissão nova. O resultado concreto em produção não está demonstrado pelo envelope. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:548`) |
| A10 | **MUDOU o alcance da conclusão:** não duplicar disputa não prova “mesmo estado”; o merge incrementa contador e troca instante. Os campos estão dentro de `contexto[motivo]`, não em colunas próprias da disputa. (`app/chamados/services/disputa_emissao.py:552`; `app/chamados/services/disputa_emissao.py:558`; `app/ponto/tests/test_bug126_selfie_invisivel.py:113`) |
| A11 | **MUDOU o alcance:** a porta procura disputa aberta; a garantia do índice de pergunta é parcial e não autoriza enunciado irrestrito para todo acervo. (`app/chamados/services/disputa_emissao.py:531`; `app/chamados/models.py:3633`) |
| B1–B2 | **SEGUE** a conversa quando falta dia e o dedup de topo quando falta episódio. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/services/disputa_emissao.py:431`) |
| B3 | **SEGUE** a possibilidade de consumir episódio antigo; “nasce uma pergunta errada” não é resultado inevitável, pois a porta também pode reutilizar linha. (`app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:2990`; `app/chamados/models.py:3091`) |
| B4–B6 | **SEGUE**, respeitadas as condições do índice e o domínio do fato. (`app/chamados/models.py:2795`; `app/chamados/models.py:3631`) |
| B7 | **SEGUE com ramos separados:** gerador sem dia pode produzir conversa; porta datada com origem retratada recusa. (`app/chamados/services/disputa_emissao.py:431`; `app/chamados/services/fio.py:1685`) |
| B8 | **MUDOU a explicação:** dia pedido diferente não provoca, por si, a recusa citada; o episódio determina o dia pelo juiz. (`app/chamados/models.py:2938`; `app/chamados/models.py:2992`) |
| B9–B10 | **SEGUE** o texto sem data e o dedup sequencial de conversa; mudar o texto continua pergunta separada. (`app/chamados/catalogo/perguntas.py:93`; `app/chamados/services/disputa_emissao.py:436`) |
| C1–C4 | **SEGUE** o código de seleção por disputa/última batida e validação coletiva. Há corte escrito posterior específico para o alcance por episódio. (`app/api/foto_recebida.py:67`; `app/api/foto_recebida.py:73`; `app/api/foto_recebida.py:79`; `app/docs/CORTES.md:441`) |
| D1 | **MUDOU a entidade descrita:** o contexto fraco vem do chamado, não diretamente da disputa. O nascimento recebe cópias separadas; o merge da disputa não atualiza o chamado nesse ramo. (`app/chamados/juizes.py:100`; `app/chamados/juizes.py:138`; `app/chamados/services/disputa_emissao.py:580`; `app/chamados/services/disputa_emissao.py:590`; `app/chamados/services/disputa_emissao.py:562`) |
| D2 | **SEGUE** o alcance limitado do selo existente. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`) |
| D3 | Os números e a distribuição do passivo continuam apenas como dado anexado; reconstrução individual e alcance da recuperação **[nao verificado]**. A restrição do contexto fundido permanece escrita em `app/chamados/models.py:3599`. |

## BRIEF ATUALIZADO — CONTRATO-DE-ENTRADA-DO-FIO

1. **Objeto da leitura:** contrato entre emissão retro, disputa e nascimento da pergunta; origem em `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`.
2. **Desencontro confirmado:** retro grava PK em `foto_ausente.batida_id`; origem e dia procuram `ultima_batida.id`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/models.py:3606`; `app/chamados/services/disputa_emissao.py:387`)
3. **Dono:** a classificação ESTRUTURA do brief anterior era inferência; permanece PERGUNTA separada, sob a separação escrita dos donos. (`CLAUDE.md:179`)
4. **Regra de preparação:** casos vêm da regra antes do código; escolha sem resposta escrita permanece pergunta. (`app/docs/LEIS.md:136`)
5. **Identidade escrita:** `foto_ausente` pertence à família BATIDA, cuja chave é episódio e motivo. (`app/chamados/catalogo/motivos.py:16`; `app/chamados/catalogo/motivos.py:43`)
6. **Dia escrito:** o episódio consulta o juiz do turno; uma batida não reclamada usa seu dia local, não uma conversão livre do envelope. (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2955`)
7. **Repetição:** a porta de pergunta e o índice tratam identidade por episódio; o contador de emissões da disputa continua diferente dessa identidade. (`app/chamados/models.py:2795`; `app/chamados/models.py:3631`; `app/chamados/services/disputa_emissao.py:552`)
8. **Contexto acumulado:** o próprio derivador declara que contexto fundido pode apontar outra anomalia e não recupera a origem histórica do acervo. (`app/chamados/models.py:3599`)
9. **Tempo:** instante efetivo da batida, recepção e dia do turno são insumos distintos; o envelope atual da API contém recepção. (`app/api/views.py:507`; `app/api/views.py:655`; `app/api/views.py:761`; `app/ponto/turnos.py:2928`)
10. **Selos existentes:** família/índice e delegação à selfie estão cobertos pelos selos citados; a obrigação e o alcance de selo de envelope ficam como PERGUNTA. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:130`)
11. **Outros consumidores:** upload seleciona disputa por `ultima_batida.id`; dia fraco lê o contexto do chamado e extrai dia civil do ISO. (`app/api/foto_recebida.py:67`; `app/chamados/juizes.py:100`; `app/chamados/juizes.py:166`)
12. **Restrições escritas:** motor exige aval; escrita de batida tem porta; previsto tem fonte única. (`CLAUDE.md:142`; `CLAUDE.md:143`; `CLAUDE.md:148`)
13. **Exportada:** nascimento e alteração do gravado têm regimes distintos; não há autorização de alteração genérica do fechamento nesta leitura. (`app/docs/LEIS.md:154`; `CLAUDE.md:411`)
14. **Passivo:** 135/15/44/76 são números do anexo; critério de classificação, contaminação por contexto e recuperação individual **[nao verificado]**. O uso posterior do contexto fundido tem a limitação escrita em `app/chamados/models.py:3599`.
15. **E3 no nascimento:** existe recusa de resposta carimbada sem texto antes das travas e também no `save`; dia e projeção de decisão passam pelos pontos atuais de escrita. (`app/chamados/models.py:2919`; `app/ch

[truncado em 20000 de 36567 caracteres]

## 15/11 00:49 · sessao a309270b1b28 · MADRUGADA-PREPARA · sonda · origem=Code

A quantidade atual de vínculos inválidos e o efeito de um saneamento permanecem **[nao verificado]**. A cópia já contém a constraint no modelo e uma migration com `CHECK ... NOT VALID`; sua aplicação no banco é **[nao verificado]**. (`app/escala/models.py:1037`, `app/escala/migrations/0041_ec_vigencia_not_valid.py:43`.)

Os escritores operacionais das datas são:

| Escritor | Escrita |
|---|---|
| `executar_vinculo` | Fecha pela porta; reabre com fim nulo; cria com início informado. `app/colaboradores/services/vinculo.py:214`, `app/colaboradores/services/vinculo.py:241`, `app/colaboradores/services/vinculo.py:277`, `app/colaboradores/services/vinculo.py:283`, `app/colaboradores/services/vinculo.py:287`, `app/colaboradores/services/vinculo.py:291`. |
| `fechar_vigencia` | Atualiza fim e desativa, somente onde início ≤ fim informado. `app/colaboradores/services/vinculo.py:641`. |
| `reabrir_vigencia_impossivel` | Atualiza fim e reativa. `app/colaboradores/services/vinculo.py:730`. |
| `encerrar_vinculo` | Atribui fim e salva. `app/colaboradores/services/vinculo.py:844`, `app/colaboradores/services/vinculo.py:845`. |
| `criar_trecho_retroativo` | Cria com início e fim. `app/colaboradores/services/vinculo.py:970`, `app/colaboradores/services/vinculo.py:972`. |
| `corrigir_vinculo_retroativo` | Atribui fim e salva. `app/colaboradores/services/vinculo.py:982`, `app/colaboradores/services/vinculo.py:983`. |
| `reverter_para_snapshot` | Reescreve início e fim do snapshot. `app/colaboradores/services/vinculo.py:1031`, `app/colaboradores/services/vinculo.py:1032`. |
| `ajustar_vinculo_pelo_sistema` | Fecha pela porta e cria outro início. `app/colaboradores/services/vinculo.py:1087`, `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1090`. |
| `desfazer_ajuste_do_sistema` | Reabre com fim nulo ou fecha pela porta e cria outro início. `app/colaboradores/services/vinculo.py:1106`, `app/colaboradores/services/vinculo.py:1108`, `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1111`. |
| `fechar_vinculos_ativos` | Delega o fechamento dos vínculos `ativa=True`. `app/colaboradores/services/vinculo.py:862`, `app/colaboradores/services/vinculo.py:863`. |
| `gravar_regua` | Aceita `**campos` e faz `update(**campos)`, sem lista restritiva de campos nessa função. Os chamadores encontrados passam campos de régua. `app/colaboradores/services/vinculo.py:1048`, `app/colaboradores/services/vinculo.py:1070`, `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:742`, `app/escala/regua_defesa.py:765`. |

As entradas desses escritores incluem wizard, chat, régua, comando de fase, desvinculação, desligamento, correção retroativa, recusa de aceite, ajuste automático e reparo de vigência. (`app/colaboradores/views.py:423`, `app/colaboradores/services/troca_pelo_chat.py:75`, `app/escala/regua_defesa.py:321`, `app/ponto/management/commands/censo_fase_12x36.py:423`, `app/escala/views.py:269`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`, `app/colaboradores/services/desligamento.py:148`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`, `app/chamados/services/acoes_chamado.py:149`, `app/escala/services/escala_auto_executor.py:103`, `app/escala/services/escala_auto_executor.py:145`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:107`.)

O admin de `EscalaColaborador` usa `SoLeitura`, que recusa adição, alteração e exclusão. Fixtures de testes também criam vínculos; não são entradas operacionais. (`app/escala/admin.py:19`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/tenants/tests.py:44`, `app/colaboradores/tests/test_ficha_do_colab.py:88`.)

Os leitores diretos do intervalo apresentam estes comportamentos:

| Leitor | Tratamento observável de fim < início |
|---|---|
| Carregamento de vínculos do período | Filtra interseção com a janela, sem validar o par início/fim; `mais=` também carrega geradoras fora desse filtro. `app/escala/alimentacao.py:149`, `app/escala/alimentacao.py:151`, `app/escala/alimentacao.py:155`. |
| `escalas_do_periodo` | Delega ao carregador plural. `app/escala/alimentacao.py:185`. |
| `vinculo_cobre_a_data` e versão SQL | Exigem início ≤ dia e fim nulo ou fim ≥ dia; um par invertido não cobre dia algum. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:223`. |
| `vinculo_do_dia` | Confere limites da geradora; no caminho sem geradora usa `vinculo_cobre_a_data`. O invertido não responde pelo dia. `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`. |
| `previstas_do_periodo` | Recorta a fatia e pula explicitamente fim < início. `app/escala/alimentacao.py:390`, `app/escala/alimentacao.py:392`. |
| Apuração por fatias | Recorta pelos limites do vínculo e pula explicitamente fatia invertida. `app/ponto/services/fechamento.py:206`, `app/ponto/services/fechamento.py:207`, `app/ponto/services/fechamento.py:242`. |
| Censo `vigencias_impossiveis` | Chama o validador real e recolhe as recusas. `app/colaboradores/services/vinculo.py:496`, `app/colaboradores/services/vinculo.py:499`, `app/colaboradores/services/vinculo.py:500`. |
| `vinculos_inconsistentes` | Identifica invertidos e os separa do cálculo de sobreposição. `app/escala/services/vinculos_inconsistentes.py:24`, `app/escala/services/vinculos_inconsistentes.py:30`, `app/escala/services/vinculos_inconsistentes.py:37`. |
| `vigencia_sem_trilha` | Separa o passivo usando `vigencias_impossiveis`. `app/escala/services/vigencia_sem_trilha.py:66`, `app/escala/services/vigencia_sem_trilha.py:77`. |
| Rejulgamento de furos | Usa `data__range` com início e fim recortado; não valida o par nesse trecho. `app/escala/regua_defesa.py:686`. |
| Piso da fase | Consulta cobertura pelo juiz SQL; sem cobertura, usa o menor início, sem consultar fim nessa alternativa. `app/escala/services/fase_na_tela.py:160`, `app/escala/services/fase_na_tela.py:165`. |
| Propositor | Escolhe pelo juiz do dia e depois exclui encerramentos anteriores ao piso de aplicação. `app/escala/services/propositor.py:448`, `app/escala/services/propositor.py:460`. |
| Explicação de cadastro zero no export | Alimenta o previsto por dia; também lê finais para explicar encerramento. `app/folha/export.py:938`, `app/folha/export.py:957`. |
| Signals | Capturam e comparam os pares anterior/atual e calculam janelas de regeneração; esses trechos não validam fim ≥ início. `app/escala/signals.py:25`, `app/escala/signals.py:57`, `app/escala/signals.py:60`, `app/escala/signals.py:68`, `app/escala/signals.py:121`, `app/escala/signals.py:200`. |
| Diagnóstico comparativo | Conserva a seleção antiga como referência de comparação. `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:117`. |
| Gerador de batidas smoke | Usa interseção de datas sem validar o par nesse filtro. `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73`. |

Outros consumidores chegam à cobertura/seleção pelas funções reais acima:

| Família | Sítios |
|---|---|
| Folgas, calendário e jornada | `app/escala/folgas.py:451`, `app/escala/folgas.py:487`, `app/escala/folgas.py:493`, `app/escala/folgas.py:539`, `app/escala/folgas.py:624`, `app/escala/services/semana_do_posto.py:75`, `app/colaboradores/services/calendario.py:505`, `app/escala/servico_jornada.py:63`. |
| Cadastro e ficha | `app/escala/services/cadastro_realidade.py:489`, `app/escala/services/cadastro_realidade.py:509`, `app/escala/services/cadastro_realidade.py:756`, `app/escala/services/cadastro_realidade.py:760`, `app/colaboradores/queries.py:64`, `app/colaboradores/services/ficha.py:129`. |
| Células e regeneração | `app/ponto/management/commands/gerar_celulas.py:283`, `app/ponto/portas/celula.py:229`, `app/ponto/portas/celula.py:339`, `app/ponto/portas/celula.py:906`, `app/ponto/portas/regen_exportada.py:147`, `app/ponto/services/cadastro_futuro.py:70`. |
| Espelho, diagnóstico e PDF | `app/ponto/services/diagnostico_escala.py:53`, `app/ponto/services/espelho.py:286`, `app/ponto/services/espelho.py:1065`, `app/relatorios/pdf_espelho.py:307`, `app/relatorios/pdf_espelho.py:571`. |
| Calculador e turnos | `app/ponto/calculador/alimentacao.py:192`, `app/ponto/turnos.py:1960`, `app/ponto/turnos.py:2166`, `app/ponto/turnos.py:2361`, `app/ponto/turnos.py:2986`, `app/ponto/turnos.py:3058`. |
| Precedência, núcleo e suprajuiz | `app/ponto/precedencia.py:312`, `app/ponto/precedencia.py:323`, `app/ponto/precedencia.py:349`, `app/ponto/supra_juiz.py:174`, `app/ponto/supra_juiz.py:185`, `app/ponto/nucleo.py:15`, `app/ponto/nucleo.py:26`. |
| Folha, feriados e CCT | `app/folha/previa.py:62`, `app/folha/previa.py:81`, `app/folha/export.py:359`, `app/folha/export.py:1252`, `app/core/feriados.py:154`, `app/core/feriados.py:161`, `app/core/regua_cct.py:329`. |
| Chamados e API | `app/chamados/services/limbo_folgas.py:200`, `app/chamados/management/commands/auditar_invariantes_chamados.py:361`, `app/chamados/management/commands/auditar_invariantes_chamados.py:391`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1538`, `app/api/views_mensageria.py:979`. |
| Triagem, cartório e vigias | `app/ponto/services/triagem_batida.py:149`, `app/ponto/services/triagem_batida.py:402`, `app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1903`, `app/ponto/services/vigia_cadastro_futuro.py:58`, `app/ponto/management/commands/vigiar_motor_autoridade.py:51`. |

Há ainda leituras somente do início e consultas somente de vínculos abertos; elas não demonstram uma precondição sobre a ordem das duas datas. (`app/ponto/services/espelho.py:43`, `app/ponto/management/commands/detectar_ausencias.py:177`, `app/ponto/management/commands/detectar_ausencias.py:247`, `app/ponto/management/commands/supra_juiz.py:149`, `app/ponto/management/commands/tripwire_celulas.py:67`, `app/chamados/detector_anomalias.py:58`, `app/escala/services/escala_auto.py:172`, `app/ponto/services/cluster_espurio.py:185`, `app/relatorios/furos.py:61`, `app/chamados/services/disputa_emissao.py:341`.)

Uma suposição adicional de que “fim sempre é maior ou igual ao início”, além dos predicados descritos, é **[nao verificado]**. O censo AST existente também declara limites de detecção; ele não prova ausência universal de acessos dinâmicos. (`app/core/censo_pergunta2.py:10`, `app/core/censo_pergunta2.py:17`.)

Para interpretar a sonda:

- `empN` usa `Colaborador.empresa_id`; a competência respeita o corte dessa empresa e recebe mês/ano pelo fim da janela. (`app/colaboradores/models.py:321`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:60`.)
- “Exportada” significa resposta verdadeira de `competencia_entregue`; “aberta”, neste relatório, significa resposta falsa desse juiz. (`app/folha/export.py:556`, `app/folha/export.py:588`, `app/folha/export.py:594`.)
- `desligado` vem de `esta_desligado`; `situacao` preserva também `afastado` e `ferias`. (`app/core/juizes.py:271`, `app/colaboradores/services/desligamento.py:23`, `app/colaboradores/models.py:295`.)
- `EscalaColaborador` não tem campo de criação/alteração; `regua_avaliada_em` e `regua_corroborada_em` pertencem à régua. (`app/escala/models.py:871`, `app/escala/models.py:884`, `app/escala/models.py:888`, `app/escala/models.py:1399`.)
- As batidas são contadas pelo instante local no intervalo completo, usando `batidas_apuraveis`; sua atribuição a um dia de turno é outra função. (`app/ponto/turnos.py:45`, `app/ponto/turnos.py:2875`, `app/ponto/turnos.py:2886`.)

Segue somente o texto. O envelope aceita `--sombra`, mas também escreve credencial, temporário e trilha; por isso não foi chamado nesta sessão. (`bin/sonda_leitura.sh:67`, `bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:248`.) Execução desta sonda no Django e resultados: **[nao verificado]**.

```python
# Texto para: bin/sonda_leitura.sh --sombra /caminho/sonda_o50.py
# Censo real: app/colaboradores/services/vinculo.py:479
# Cobertura: app/escala/alimentacao.py:205
# Juizes declarados: app/core/juizes.py:271 e :337

import json
import re
from collections import Counter, defaultdict
from contextlib import ExitStack
from datetime import datetime, time, timedelta

from django.conf import settings
from django.db import connections
from django.db.models import Count
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from colaboradores.services.desligamento import esta_desligado
from colaboradores.services.vinculo import vigencias_impossiveis
from escala.alimentacao import vinculo_cobre_a_data
from escala.models import CelulaDia, EscalaColaborador
from folha.export import competencia_entregue
from ponto.janelas import janela_atual
from ponto.turnos import batidas_apuraveis, data_local


def somente_select(execute, sql, params, many, context):
    # Precedente:
    # app/chamados/management/commands/diff_e3_sombra.py:30
    if many or not isinstance(sql, str):
        raise RuntimeError("SQL recusado")

    s = sql.strip()
    if not re.match(r"^SELECT\b", s, re.I):
        raise RuntimeError("SQL recusado")

    # Recusa comentarios, literais SQL e comandos multiplos.
    # Os valores destas consultas seguem nos parametros do ORM.
    if any(x in s for x in (";", "--", "/*", "*/", "'")):
        raise RuntimeError("SQL recusado")

    t = re.sub(r'"(?:[^"]|"")*"', "IDENT", s)
    if re.search(
        r"\b(WITH|INTO|INSERT|UPDATE|DELETE|MERGE|CREATE|ALTER|DROP|"
        r"TRUNCATE|COPY|CALL|DO|SET|RESET|BEGIN|COMMIT|ROLLBACK|" [nao verificado]
        r"SAVEPOINT|RELEASE|GRANT|REVOKE|FOR|LOCK)\b",
        t,
        re.I,
    ):
        raise RuntimeError("SQL recusado")

    # Recusa chamadas SQL fora do necessario para estas consultas.
    permitidos = {
        "COUNT", "IN", "AND", "OR", "NOT", "WHERE",
        "FROM", "SELECT", "EXISTS", "AS",
    }
    chamadas = re.findall(r"([A-Za-z_][A-Za-z_0-9$]*)\s*\(", t)
    if any(nome.upper() not in permitidos for nome in chamadas):
        raise RuntimeError("SQL recusado")

    return execute(sql, params, many, context)


def coletar():
    conn = connections["default"]
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or conn.settings_dict.get("NAME") != "sombra"
        or conn.settings_dict.get("HOST") != "sombra_db"
        or conn.settings_dict.get("USER") != "leitor"
        or getattr(conn, "schema_name", None) != "juliani"
        or not conn.get_autocommit()
    ):
        raise RuntimeError("Envelope recusado")

    # Sem limitar empresa, ativa, periodo ou quantidade esperada.
    # app/colaboradores/services/vinculo.py:491
    censo = vigencias_impossiveis()
    esperados = {
        pk: (cid, ini, fim, ativa)
        for pk, cid, ini, fim, ativa in censo
    }
    if len(esperados) != len(censo):
        raise RuntimeError("Censo duplicado")

    cids = {cid for _, cid, _, _, _ in censo}
    campos = (
        "pk", "colaborador_id", "data_inicio", "data_fim", "ativa",
    )
    por_colab = defaultdict(list)
    por_pk = {}

    for ec in (
        EscalaColaborador.objects.filter(colaborador_id__in=cids)
        .only(*campos)
        .order_by("pk")
    ):
        por_colab[ec.colaborador_id].append(ec)
        por_pk[ec.pk] = ec

    colabs = {
        c.pk: c
        for c in Colaborador.objects.filter(pk__in=cids).only(
            "pk", "empresa_id", "situacao"
        )
    }
    empresas = {
        e.pk: e
        for e in Empresa.objects.only("pk", "dia_inicio_competencia")
    }

    totais_emp = Counter({eid: 0 for eid in empresas})
    entregue = {}
    linhas = []
    um_dia = timedelta(days=1)
    tz = timezone.get_default_timezone()

    with timezone.override(tz):
        for pk in sorted(esperados):
            ec = por_pk[pk]
            if (
                ec.colaborador_id,
                ec.data_inicio,
                ec.data_fim,
                ec.ativa,
            ) != esperados[pk]:
                raise RuntimeError("Universo mudou durante a leitura")

            c = colabs[ec.colaborador_id]
            empresa = empresas[c.empresa_id]

            # Intervalo solicitado: [data_fim, data_inicio].
            lo, hi = ec.data_fim, ec.data_inicio
            grupos = {}

            def grupo(dia):
                # app/ponto/janelas.py:60
                ji, jf = janela_atual(dia, empresa)
                k = (jf.year, jf.month)
                tripla = (empresa.pk, jf.month, jf.year)

                # app/folha/export.py:556
                if tripla not in entregue:
                    entregue[tripla] = bool(
                        competencia_entregue(*tripla)
                    )

                if k not in grupos:
                    grupos[k] = {
                        "competencia": (
                            f"{jf.year:04d}-{jf.month:02d}"
                        ),
                        "janela": [ji.isoformat(), jf.isoformat()],
                        "entregue": entregue[tripla],
                        "estado": (
                            "exportada"
                            if entregue[tripla]
                            else "aberta"
                        ),
                        "recorte_intervalo": None,
                        "dias_intervalo": 0,
                        "celulas_intervalo": 0,
                        "celulas_depois_inicio": 0,
                        "celulas_intervalo_geradas_por_este": 0,
                        "celulas_depois_inicio_geradas_por_este": 0,
                        "batidas_intervalo": 0,
                        "_dias_celulas_intervalo": set(),
                        "_dias_celulas_depois_inicio": set(),
                    }

                return grupos[k], jf

            # Inclui competencias sem celulas ou batidas.
            d = lo
            while d <= hi:
                g, jf = grupo(d)
                ate = min(hi, jf)
                g["recorte_intervalo"] = [
                    d.isoformat(), ate.isoformat(),
                ]
                g["dias_intervalo"] += (ate - d).days + 1
                if ate == hi:
                    break
                d = ate + um_dia

            # Todas as celulas do colaborador:
            # [lo, hi] e, separadamente, data > hi, sem teto.
            # Proveniencia: app/escala/models.py:1692
            celulas = (
                CelulaDia.objects
                .filter(colaborador_id=c.pk, data__gte=lo)
                .order_by()
                .values("data", "escala_geradora_id")
                .annotate(n=Count("pk"))
            )
            for cel in celulas:
                dia, n = cel["data"], cel["n"]
                g, _ = grupo(dia)
                parte = (
                    "intervalo" if dia <= hi else "depois_inicio"
                )
                g["celulas_" + parte] += n
                g["_dias_celulas_" + parte].add(dia)
                if cel["escala_geradora_id"] == ec.pk:
                    g[
                        "celulas_" + parte + "_geradas_por_este"
                    ] += n

            # Inclui todas as horas do ultimo dia.
            # app/ponto/turnos.py:2875; data_local em :45
            ti = timezone.make_aware(
                datetime.combine(lo, time.min), tz
            )
            tf = timezone.make_aware(
                datetime.combine(hi, time.max), tz
            )
            for ts in batidas_apuraveis(c, ti, tf).values_list(
                "timestamp", flat=True
            ):
                g, _ = grupo(data_local(ts))
                g["batidas_intervalo"] += 1

            # Um outro vinculo individual cobrindo ambas as pontas.
            # app/escala/alimentacao.py:214
            outros = [
                f"#{outro.pk}"
           

[truncado em 20000 de 23309 caracteres]

## 15/11 00:47 · sessao 05c12876a823 · MADRUGADA-PREPARA · sonda · origem=Code

**A18/A19 da matriz são identificáveis; sua equivalência aos “itens 18–19 da E4B” não está demonstrada.** A árvore também usa “18–19” para o fim do programa E4B. O `fatias.md` fornecido descreve leitores; atribuir a projeção legada R/V a uma fatia executora da E4c permanece **[nao verificado]**. (`app/docs/BACKLOG.md:1977`; `app/docs/GRADE-MESTRA.md:717`)

A leitura abaixo usa **A18/A19** e identifica separadamente a operação real **R/V → DecisaoPergunta**. O comando E4B chama fábrica, registro de decisões e materialização; não contém uma aplicação automática de recusa humana A19. Exportadas são recusadas pelo planejamento e novamente antes da execução. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:260`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:304`)

| Operação | Modelo, campos e escritor na cópia |
|---|---|
| **A18: recusa técnica** | `PerguntaDisputa.recusa_motivo` recebe o motivo técnico truncado em 240 caracteres. Em colisão sem carimbo, delega também `materializada_em`, `materializada_por_id` e `via_resolucao=fato_ocupado`. Escritores: `app/chamados/services/materializacao.py:1096`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1108`; executor do carimbo: `app/chamados/models.py:3182`, `app/chamados/models.py:3268`, `app/chamados/models.py:3273`. |
| **A19: recusa humana** | `PerguntaDisputa.recusa_motivo` recebe autor e motivo; a reabertura limpa `respondida_em`, `resposta_colab`, `validada_em`, `validada_por`, `materializada_em`, `materializada_por` e `via_resolucao`. Escritores: `app/chamados/services/validacao.py:619`, `app/chamados/services/validacao.py:628`; operação delegada: `app/chamados/services/disputa_emissao.py:1595`, `app/chamados/services/disputa_emissao.py:1634`; executor: `app/chamados/models.py:3363`. |
| **A19: decisão preservada** | Insere `DecisaoPergunta` com `pergunta_id`, `ato=recusa`, `ato_texto`, `respondida_original_em`, `validada_original_em`, `validada_original_por_id`, `ato_em`, `ato_por`, `ato_motivo`. Produtor: `app/chamados/services/disputa_emissao.py:1617`; executor: `app/chamados/models.py:4096`. |
| **Projeção legada R/V** | Insere DP `resposta`: `pergunta_id`, `ato`, `ato_em=respondida_em`, `ato_texto=resposta_colab`, `ato_motivo`; DP `validacao`: os mesmos campos, com `ato_em=validada_em` e `ato_por_id=validada_por_id`. Produtor: `app/chamados/models.py:4074`, `app/chamados/models.py:4078`; executor: `app/chamados/models.py:4096`. Identificação como E4c: **[nao verificado]**. |
| **Notas e auditoria** | A18 e A19 podem inserir `FollowupChamado`: `chamado_id`, `autor_tipo`, `autor_user`, `conteudo`, `anexo_hash`, e chave de evento quando cabível. A19 também chama os adaptadores de `LogAuditoria`. (`app/chamados/services/materializacao.py:1113`; `app/chamados/services/validacao.py:633`; `app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:101`; `app/chamados/services/validacao.py:637`; `app/chamados/services/validacao.py:643`; `app/core/observ.py:134`) |
| **Eventos decorrentes** | DP registrada e reabertura chamam `acordar_celulas`; A18 com carimbo também pode chegar por signal. A escrita é `EventoCelula(celula_id, colaborador_id, data)`, com os defaults do modelo; localizar a célula apenas a lê. (`app/chamados/models.py:4097`; `app/chamados/services/disputa_emissao.py:1635`; `app/chamados/signals.py:257`; `app/chamados/models.py:3325`; `app/ponto/portas/celula.py:954`; `app/escala/models.py:1860`) |

Isso não fecha todas as cascatas: salvar pergunta agenda um callback de beco; quando elegível, ele resolve perguntas e fecha a disputa. O consumidor da outbox é outra execução. Linhas e efeitos efetivamente atingidos por essas cascatas: **[nao verificado]**. (`app/chamados/signals.py:230`; `app/chamados/signals.py:240`; `app/chamados/services/becos.py:54`; `app/chamados/services/becos.py:57`; `app/chamados/reconciliador.py:1316`)

**Nos leitores examinados do GRAVADO/TXT, não há leitura direta dos campos R/V/C/recusa, das DP, notas ou eventos acima.** A montagem seleciona `FechamentoMensal`, lê seus valores e consulta autoridades de ausência e retenção. Cobertura de todo despacho transitivo/dinâmico: **[nao verificado]**. (`app/folha/export.py:714`; `app/folha/export.py:719`; `app/folha/export.py:730`; `app/folha/export.py:740`; `app/folha/export.py:968`)

| Leitor | O que efetivamente lê |
|---|---|
| **Valores do TXT** | FM: `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_folga_trabalhada`, `horas_noturnas`, `horas_intra_indenizada`, `horas_atraso`, `horas_saida_antecipada`. (`app/folha/export.py:420`; `app/folha/export.py:439`) |
| **Entrada/retenção** | FM e cadastro; a retenção consulta o juiz da célula. (`app/folha/export.py:785`; `app/folha/export.py:831`; `app/folha/export.py:968`; `app/folha/export.py:1306`) |
| **Faltas e rubricas** | Datas de falta vêm da autoridade de ausência; o código da rubrica vem do mapa de rubricas. (`app/folha/export.py:169`; `app/folha/export.py:184`; `app/folha/export.py:735`; `app/folha/export.py:740`; `app/folha/export.py:984`) |
| **TXT já registrado** | `ExportacaoDominio` guarda `conteudo`, `hash_sha256` e `total_linhas`; `registrar` recebe o conteúdo pronto. (`app/folha/models.py:82`; `app/folha/models.py:174`; `app/folha/models.py:182`) |

Portanto, **ausência de escrita direta em horas não certifica dinheiro zero**. `fechamento_lido()` deriva o estado atual sem persistir; `txt_vigente_x_gravado()` compara o TXT vigente com a montagem atual. Nenhuma dessas funções fornece, por si, o estado posterior hipotético de A18/A19. Delta atribuível em minutos ou rubricas: **[nao verificado]**. (`app/ponto/services/fechamento.py:1759`; `app/folha/export.py:655`)

A sonda abaixo usa dois universos distintos, sem somar suas unidades:

- **E4B: dia-colaborador**, `sem_porta` de `_alcance`, mais o complemento declarado de classe e em `[hoje−45, hoje)`. Esse complemento não representa todo o histórico exportado. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:153`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:248`)
- **Projeção R/V: novas linhas DP**, pelo alcance real `linhas_a_criar()`: R ou V presente, horizonte canônico/sem dia ou conversa; conversa e sem dia ficam separados. O comando existente declara que não aplica essa projeção. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:73`)

A18/A19 não expõem alcance planejável em leitura; a sonda conta esses **dois escritores sem alcance chamável**, deixando suas quantidades de linhas e mudanças como `null`, nunca zero. A atribuição da projeção à E4c também fica explícita como não verificada. (`app/chamados/services/materializacao.py:1089`; `app/chamados/services/validacao.py:562`)

Texto para o envelope `bin/sonda_leitura.sh --sombra <sonda.py>`; execução e resultados: **[nao verificado]**. (`bin/sonda_leitura.sh:56`)

```python
# Apenas TEXTO: nao salvar nem executar nesta leitura.
# Envelope: bin/sonda_leitura.sh --sombra <sonda.py>
# RV_DP nomeia a operacao real; atribuicao a E4c nao verificada.

import io
import json
import logging
import re
from collections import Counter
from contextlib import ExitStack, redirect_stdout, redirect_stderr


class SQLRecusado(Exception):
    pass


def so_select(execute, sql, params, many, context):
    texto = sql.strip()
    if (
        many
        or not re.match(r"^SELECT\b", texto, re.I)
        or ";" in texto
        or re.search(
            r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
            texto, re.I,
        )
    ):
        raise SQLRecusado()
    return execute(sql, params, many, context)


def medir():
    from django.db import connections
    from django.utils import timezone

    saida = []
    with ExitStack() as pilha:
        for conn in connections.all():
            pilha.enter_context(conn.execute_wrapper(so_select))

        from folha.export import competencia_entregue
        from folha.models import ExportacaoDominio
        from colaboradores.models import Colaborador
        from chamados.management.commands.backfill_fio_morto_e4b import (
            _alcance, _exportadas_classe_e,
        )
        from chamados.management.commands.backfill_decisoes_pergunta import (
            linhas_a_criar,
        )

        agora = timezone.localtime()
        hoje = agora.date()

        # Enumerar triplas nao decide entrega: somente K6 decide.
        # folha/export.py:556,580
        triplas = set(
            ExportacaoDominio.objects.order_by()
            .values_list("empresa_id", "mes", "ano").distinct()
        )
        entregue = {}

        def k6(chave):
            if chave not in entregue:
                entregue[chave] = competencia_entregue(*chave) is True
            return entregue[chave]

        exportadas = {k for k in triplas if k6(k)}

        # E4B: alcance declarado, unidade dia-colaborador.
        # backfill_fio_morto_e4b.py:76,153,237
        _, base = _alcance(None, hoje, agora)
        extra = _exportadas_classe_e(None, hoje)
        dias = {(x[1], x[2]): x for x in base}
        for x in extra:
            dias.setdefault((x[1], x[2]), x)

        empresas = dict(
            Colaborador.objects.filter(
                pk__in={cid for cid, dia in dias}
            ).values_list("pk", "empresa_id")
        )
        e4b_exp = Counter()
        e4b_partes = Counter(
            exportada=0, nao_exportada=0, indeterminada=0,
        )
        for (cid, dia), x in dias.items():
            emp = empresas.get(cid)
            try:
                # Competencia produzida pelo alcance real.
                mes, ano = map(int, x[3].split("/"))
                chave = (emp, mes, ano)
            except (ValueError, TypeError, AttributeError):
                e4b_partes["indeterminada"] += 1
                continue

            if emp is None:
                e4b_partes["indeterminada"] += 1
            elif k6(chave):
                e4b_partes["exportada"] += 1
                e4b_exp[chave] += 1
                exportadas.add(chave)
            else:
                e4b_partes["nao_exportada"] += 1

        assert sum(e4b_partes.values()) == len(dias)
        assert sum(e4b_exp.values()) == e4b_partes["exportada"]

        # Projecao: alcance real, unidade nova linha DP.
        # backfill_decisoes_pergunta.py:44,67,73,79,82
        rv = linhas_a_criar()
        rv_partes = Counter(
            exportada=0, nao_exportada=0,
            conversa=0, sem_competencia=0,
        )
        rv_exp = {}
        total_rv = 0

        for chave, atos in rv.items():
            r = atos.get("resposta", [])
            v = atos.get("validacao", [])
            n = len(r) + len(v)
            assert n == sum(len(itens) for itens in atos.values())
            total_rv += n
            emp, mes, ano = chave

            if mes is None or ano is None or emp is None:
                balde = (
                    "conversa" if ano == "conversa"
                    else "sem_competencia"
                )
                rv_partes[balde] += n
            elif k6(chave):
                rv_partes["exportada"] += n
                exportadas.add(chave)
                rv_exp[chave] = dict(
                    resposta=len(r),
                    validacao=len(v),
                    total=n,
                    perguntas=len({pk for cid, pk in r + v}),
                )
            else:
                rv_partes["nao_exportada"] += n

        assert sum(rv_partes.values()) == total_rv
        assert (
            sum(x["total"] for x in rv_exp.values())
            == rv_partes["exportada"]
        )

        for emp, mes, ano in sorted(exportadas):
            desconhecido = dict(
                alcance="sem alcance chamavel",
                linhas=None,
                mudariam_campo_lido_gravado_TXT=None,
            )
            proj = rv_exp.get(
                (emp, mes, ano),
                dict(resposta=0, validacao=0, total=0, perguntas=0),
            )
            saida.append(dict(
                empresa="emp%d" % emp,
                competencia="%02d/%04d" % (mes, ano),
                E4B_dias_no_alcance=e4b_exp[(emp, mes, ano)],
                A18=desconhecido,
                A19=desconhecido,
                sem_alcance_chamavel=2,
                RV_DP=dict(
                    proj, mudariam_campo_lido_gravado_TXT=None,
                ),
                atribuicao_RV_DP_a_E4c="nao_verificado",
            ))

        saida.append(dict(
            TOTAL_E4B_dias=len(dias),
            partes=dict(e4b_partes),
        ))
        saida.append(dict(
            TOTAL_RV_DP=total_rv,
            partes=dict(rv_partes),
        ))
        saida.append(dict(
            TOTAL_exportadas=len(exportadas),
            escritores_distintos_sem_alcance_chamavel=2,
            pares_escritor_competencia_sem_alcance_chamavel=(
                2 * len(exportadas)
            ),
            delta_minutos_rubricas="nao_verificado",
        ))
    return saida


ok = False
nivel_anterior = logging.root.manager.disable
try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
        linhas = medir()
    for linha in linhas:
        print(json.dumps(linha, sort_keys=True, ensure_ascii=True))
    ok = True
except Exception:
    # Nao imprimir excecao, parametros SQL ou dados pessoais.
    print("ERRO leitura incompleta; resultados nao verificados")
finally:
    logging.disable(nivel_anterior)
    print("FIM completo=%d" % int(ok))
```

## 15/11 00:46 · sessao 81c7a97b4c43 · - · - · origem=Ronald (tmux)

Censo consolidado da pergunta: **17 funções persistentes do núcleo, 18 sítios**; nascimento e reuso pertencem à mesma `_nascer` (`app/chamados/models.py:3089`, `app/chamados/models.py:3099`). O mecanismo existente reconheceu 21 funções/21 sítios amplos; quatro gravam apenas campos auxiliares e o insert por `cls(...)` exigiu complemento manual (`app/core/censo_escritas.py:469`, `app/chamados/services/materializacao.py:857`, `app/chamados/services/materializacao.py:1109`, `app/chamados/services/validacao.py:625`, `app/chamados/services/validacao.py:701`).

As 17 funções persistentes:

| Função | Escrita |
|---|---|
| `PerguntaDisputa._nascer` | `app/chamados/models.py:3089`, `:3099` |
| `PerguntaDisputa.carimbar` | `app/chamados/models.py:3235` |
| `PerguntaDisputa.carimbar_identidade` | `app/chamados/models.py:3273` |
| `PerguntaDisputa.descarimbar` | `app/chamados/models.py:3295` |
| `PerguntaDisputa.descarimbar_em_lote` | `app/chamados/models.py:3363` |
| `PerguntaDisputa.encerrar_por_fato_no_espelho` | `app/chamados/models.py:3375` |
| `DecisaoPergunta.registrar` | `app/chamados/models.py:4096` |
| `gravar_lavras_em_lote` | `app/chamados/veredito_lavrado.py:250` |
| `lavrar` | `app/chamados/veredito_lavrado.py:275` |
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1508` |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1662` |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1683` |
| `reapontar_resolvedora` | `app/chamados/services/fio.py:966` |
| `rechavear_data_evento` | `app/chamados/services/fio.py:1082` |
| `relavrar` | `app/chamados/services/relavra_via.py:127` |
| `_carimbar_validacao` | `app/chamados/services/validacao.py:162` |
| `devolver_validacao_recusada` | `app/chamados/services/validacao.py:227` |

Categorias sobrepostas: destino tem **2 funções/3 sítios**; ciclo resposta/validação/carimbo/via tem **12 funções**, incluindo nascimento; lavra tem duas portas próprias, além dos saves que persistem a lavra calculada com `salvar=False`; trilha tem uma porta (`app/chamados/models.py:3065`, `app/chamados/models.py:3095`, `app/chamados/services/fio.py:964`, `app/chamados/veredito_lavrado.py:240`, `app/chamados/veredito_lavrado.py:254`, `app/chamados/models.py:4086`).

Delta decisivo E3: correção agora **sempre** limpa resposta, validação e carimbo pela porta do lote; não toca linha fora do fato nem registra falsa reabertura para ela; `desfazer_absorcao` foi removido (`app/chamados/services/disputa_emissao.py:1586`, `:1595`, `:1631`, `:1634`). Recusa humana ganhou veto explícito a pergunta fora do fato (`app/chamados/services/validacao.py:609`). O par `respondida_em` preenchido/texto vazio ganhou guarda no nascimento, save, lote e resposta automática (`app/chamados/models.py:2917`, `:3529`, `:3343`; `app/chamados/services/disputa_emissao.py:1657`).

Identidade: `_chave_de_identidade` deriva colaborador, instância e episódio apenas no save de inserção; migração histórica I1 é separada do universo vivo (`app/chamados/models.py:3530`, `:3568`, `:3575`, `:3583`, `:3592`; `app/chamados/migrations/0051_decisao_pergunta.py:22`, `:25`, `:27`).

Leitores abreviáveis: `estado`, C6, app, validação, materializador, histórico e stale da lavra (`app/chamados/models.py:3117`; `app/chamados/juizes.py:1399`; `app/chamados/services/pergunta_no_app.py:45`, `:76`; `app/chamados/services/validacao.py:418`; `app/chamados/services/materializacao.py:1344`; `app/chamados/models.py:3643`; `app/chamados/veredito_lavrado.py:224`).

## 15/11 00:46 · sessao fb6bf50f898f · - · - · origem=Ronald (tmux)

Na cópia atual, a raiz mantém **2 estados**: aberta (`fechada_em=None`) e fechada (`fechada_em!=None`); o motivo do fechamento aceita `veredito`, `vazia`, `desligamento`, `admin_encerra` ou `competencia_trancada`, enquanto vazio representa aberta ou legado. (`app/chamados/models.py:1804`, `:1827`, `:1840`, `:1870`)

**Matriz da raiz**

| Transição | Escritor real | Guarda | Leitores |
|---|---|---|---|
| Inexistente → aberta, núcleo | `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa:587` | Trava universo do colaborador; procura aberta; cria somente na ausência. (`:530`, `:531`, `:535`, `:587`) | `colaborador_em_disputa` (`mesmo arquivo:34`); `particao_do_app` (`app/chamados/services/pergunta_no_app.py:76`) |
| Inexistente → aberta, repasse | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1123` | Exclui dia na lista admin; regularizável; chamado sem pergunta não materializada, salvo caminho da ata; ausência de disputa aberta. (`:1047`, `:1086`, `:1096`, `:1103`) | Mesmos leitores anteriores. (`:34`; `app/chamados/services/pergunta_no_app.py:81`) |
| Inexistente → aberta, âncora | `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1358` | Categoria `ancora_limbo`; chamado não encerrado; nenhuma pergunta de âncora não materializada; ausência de disputa aberta. (`:1316`, `:1318`, `:1323`, `:1332`, `:1338`) | Mesmos leitores anteriores. (`:34`; `app/chamados/services/pergunta_no_app.py:81`) |
| Aberta A → aberta A∪B | Núcleo `:565`; repasse `:1132`, `:1161`, `:1256`; âncora `:1367`, todos em `app/chamados/services/disputa_emissao.py` | Núcleo compara conjuntos e só salva campos alterados; repasse/âncora atualizam motivos nos seus ramos. (`:537`, `:540`, `:564`, `:1131`, `:1160`, `:1255`, `:1366`) | Gerador de perguntas (`mesmo arquivo:300`); classificação do relatório (`app/chamados/relatorios_disputa.py:11`) |
| Fechada A → fechada A∪B, complemento | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1084` | Pergunta do destino sem materialização, sem filtro de dona aberta; dia identificado; ata disponível; complemento criou pergunta. (`:1070`, `:1074`, `:1077`, `:1081`, `:1082`) | Pergunta viva em dona fechada entra no histórico do app (`app/chamados/services/pergunta_no_app.py:81`); medida pronta em `vigia_fio.viva_em_disputa_fechada` (`app/chamados/services/vigia_fio.py:758`) |
| Aberta → fechada `vazia`/`veredito` | `app/chamados/models.py::DisputaSupervisao.fechar:1996`, persistência `:2000` | Autor; ausência de resposta sem veredito; nenhuma pergunta viva; zero perguntas escolhe `vazia`, demais escolhem `veredito`. (`:1948`, `:1950`, `:1960`, `:1962`, `:1969`) | Signal do pai (`app/chamados/signals.py:29`); app (`app/chamados/services/pergunta_no_app.py:81`); contador de vazias (`app/chamados/services/vigia_fio.py:748`) |
| Aberta → fechada `desligamento` | Mesmo `fechar:1996`; chamada `app/colaboradores/services/desligamento.py:167` | Exceção aceita; autor; ainda recusa resposta sem veredito; carimba perguntas vivas por decisão administrativa e registra silêncio antes do fechamento. (`app/chamados/models.py:1940`, `:1948`, `:1950`, `:1982`, `:1988`) | Signal e app. (`app/chamados/signals.py:29`; `app/chamados/services/pergunta_no_app.py:81`) |
| Aberta → fechada `admin_encerra` | `app/chamados/models.py::DisputaSupervisao.admin_encerra:2014` → `fechar:1996` | Motivo escrito não vazio; demais guardas anteriores. (`:2011`, `:2012`, `:1950`, `:1982`) | Signal e app. (`app/chamados/signals.py:29`; `app/chamados/services/pergunta_no_app.py:81`) |
| Aberta → fechada `competencia_trancada` | Mesmo `fechar:1996`; chamada `app/chamados/services/competencia_trancada.py::encerrar:229` | Chamador pede `pode_fechar`; porta exige autor e ausência de respostas sem veredito; não entra no ramo que carimba mudas. (`app/chamados/services/competencia_trancada.py:227`; `app/chamados/models.py:1948`, `:1950`, `:1982`) | Signal e app. (`app/chamados/signals.py:29`; `app/chamados/services/pergunta_no_app.py:81`) |
| Fechada → aberta, admin | `app/chamados/models.py::DisputaSupervisao.reabrir_admin:2213`, save `:2225` | Fechada; contador <2; autor. Casca exige staff/grupo, motivo válido e justificativa ≥20 caracteres. (`:2202`, `:2204`, `:2206`; `app/chamados/services/acoes_disputa.py:245`, `:251`, `:253`, `:255`) | App; procura de aberta pelo emissor. (`app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/disputa_emissao.py:531`) |
| Fechada → aberta, sistema | `app/chamados/models.py::DisputaSupervisao.reabrir_sistema:2237`, save `:2241` | Somente fechada; aberta retorna `False`; não incrementa contador; anexa nota se há chamado. (`:2231`, `:2242`, `:2244`) | App, helper/motor. (`app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/disputa_emissao.py:34`; `app/ponto/services/disputa.py:31`) |
| Aberta/questionário corrigível → aberta/questionário pendente | `app/chamados/models.py::DisputaSupervisao.reabrir_por_colab:2301`, contador `:2303`, save `:2304` | Autor; dona aberta; contador <2; `perguntas_corrigiveis` não vazio. (`:2282`, `:2284`, `:2288`, `:2290`, `:2292`) | App e relatório. (`app/chamados/services/pergunta_no_app.py:81`; `app/chamados/relatorios_disputa.py:24`) |
| Aberta A → aberta A−motivo; último motivo encaminha fechamento | `app/chamados/models.py::DisputaSupervisao.remover_motivo:2127`, save `:2129`, eventual `fechar:2136` | Autor; relê sob trava; aberta; motivo presente. Retrai perguntas preservando trilha/identidade; se nenhum motivo resta, pede fechamento normal, que pode recusar. (`:2079`, `:2090`, `:2095`, `:2097`, `:2100`, `:2120`, `:2125`, `:2130`, `:2136`) | Gerador e relatório. (`app/chamados/services/disputa_emissao.py:300`; `app/chamados/relatorios_disputa.py:11`) |
| Motivos com foto ausente → motivos sem foto ausente | `app/chamados/models.py::DisputaSupervisao.aparar_motivo_respondido:2032` | Motivo precisa existir; remove motivo/contexto, sem fechar por conta própria. (`:2025`, `:2029`, `:2032`; chamada `app/api/foto_recebida.py:72`) | Fechamento separado do chamador e relatório. (`app/api/foto_recebida.py:80`; `app/chamados/relatorios_disputa.py:11`) |
| Aberta com resolução residual → aberta com resolução vazia | `app/chamados/models.py::DisputaSupervisao.limpar_resolucao_residual:2329`, save `:2330` | Aberta e texto não vazio; nota se há chamado. (`:2326`, `:2331`, `:2333`) | Signal lê resolução nos próximos fechamentos; app lê estado de abertura. (`app/chamados/signals.py:46`; `app/chamados/services/pergunta_no_app.py:81`) |
| Contexto sem chave → contexto com chave, preserva estado | `app/chamados/models.py::DisputaSupervisao.anotar_contexto:2169` | Só se chave ainda não tem valor. (`:2164`, `:2165`, `:2167`) | Os consumidores da chave dependem do chamador; inventário completo por chave [nao verificado]. |
| Contexto anterior → contexto novo, preserva estado | `app/chamados/models.py::DisputaSupervisao.renovar_contexto:2187` | Só se valor diferente. (`:2182`, `:2183`, `:2185`) | Os consumidores da chave dependem do chamador; inventário completo por chave [nao verificado]. |
| Contexto da disputa → contexto com dia de pergunta de entrada pendente | `app/chamados/services/disputa_emissao.py::perguntar_entradas_pendentes:296` | Classificação do dia devolveu motivo; só salva contexto se criou pergunta. (`:288`, `:289`, `:293`, `:295`) | `gerar_perguntas_disputa` chamado na própria operação. (`:294`) |

**Censo executado sem importar a aplicação**

Executei apenas em memória os helpers AST do selo: 765 arquivos de produção; zero violações no selo `status_local/fechada_em`; para a raiz, três funções persistem `fechada_em`: `fechar`, `reabrir_admin`, `reabrir_sistema`. A fronteira/exclusões e o mecanismo estão em `app/chamados/tests/test_selo_status_so_pelas_portas.py:15`, `:319`, `:332`, `:340`; os seis achados são atributo/save em `app/chamados/models.py:1996`, `:2000`, `:2213`, `:2225`, `:2237`, `:2241`.

Ampliando a leitura do mesmo helper aos campos `fechada_em`, `fechada_por[_id]`, `resolucao`, `motivo_fechamento`, as únicas funções persistentes são essas três e `limpar_resolucao_residual`: **4 funções**. Os dois splats sinalizados adicionalmente pertencem ao outro modelo, `ChamadoColaborador.abrir/criar`, nos seus próprios gerentes `cls.objects`; não são gravações da raiz. (`app/chamados/models.py:1018`, `:1142`, `:1996`, `:2213`, `:2237`, `:2329`)

O censo geral puro da cópia identificou **19 sítios persistentes em 13 funções da raiz**: nove saves em métodos da raiz; dez sítios no emissor distribuídos entre `perguntar_entradas_pendentes`, núcleo, repasse e âncora. Seu mecanismo atribui import tardio antes da varredura e distingue construção/save/gerente relacionado. (`app/core/censo_escritas.py:3`, `:6`, `:451`, `:455`, `:469`; sítios: `app/chamados/models.py:2000`, `:2032`, `:2129`, `:2169`, `:2187`, `:2225`, `:2241`, `:2304`, `:2330`; `app/chamados/services/disputa_emissao.py:296`, `:565`, `:587`, `:1084`, `:1123`, `:1132`, `:1161`, `:1256`, `:1358`, `:1367`)

Esse censo geral contém **131 operações não atribuíveis na árvore inteira**; esse total não pode fundamentar exaustividade universal por modelo sem classificação adicional. A limitação é parte do mecanismo. (`app/core/censo_escritas.py:43`, `:469`) O selo específico também declara limites para receptor/tipo genérico, nome dinâmico de campo e splat não resolvido; portanto despacho dinâmico arbitrário permanece [nao verificado]. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `:291`)

A varredura AST das chamadas encontrou **15 chamadas diretas de fechamento da raiz**, mais um homônimo `ChamadoColaborador.fechar`; cinco de reabertura sistema; uma admin; duas colaborador; duas remoção de motivo; uma aparagem; nenhuma chamada produtiva de `admin_encerra` ou limpeza residual. (`app/chamados/reconciliador.py:413`, `:613`, `:1172`; `app/chamados/services/materializacao.py:592`, `:1339`; `app/chamados/services/disputa_emissao.py:1284`, `:2099`; `app/chamados/models.py:2014`, `:2136`; `app/chamados/services/becos.py:57`; `app/chamados/services/competencia_trancada.py:229`; `app/colaboradores/services/desligamento.py:167`; `app/ponto/services/entrada_sincronizada.py:60`; `app/api/foto_recebida.py:80`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`; homônimo `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`; demais chamadas explicitadas na matriz e abaixo)

Os cinco chamadores de sistema continuam: ressuscitação, validação recusada, ausência rejeitada, comando de órfãs e carimbo sem lastro. (`app/chamados/models.py:1622`; `app/chamados/services/validacao.py:243`; `app/chamados/services/materializacao.py:692`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`; `app/chamados/services/relavra_via.py:261`)

A leitura AST considerou import local e apelido: `DisputaSupervisao as _DS` em `ChamadoColaborador.transicionar`, e `_DSup_S93` em `meu_atendimento`; não apareceu referência de porta armazenada como bound method. (`app/chamados/models.py:189`; `app/chamados/views.py:620`; resultado da varredura em memória: [nao verificado por artefato persistido])

Não apareceu referência a esses tokens da raiz em `__init__.py`; o reexport relevante está no final de `models.py`, incluindo `colaborador_em_disputa` e funções do emissor. (`app/chamados/models.py:4235`, `:4236`; ausência em `__init__.py`: [nao verificado por artefato persistido])

As migrações que definem o estado são esquema/defaults: criação original e adição do motivo; gravação de registros da raiz em `RunPython` não apareceu na pesquisa desses campos. (`app/chamados/migrations/0013_disputasupervisao.py:24`, `:25`, `:28`; `app/chamados/migrations/0049_disputa_motivo_fechamento.py:14`; ausência operacional: [nao verificado por artefato persistido])

**Deltas materiais úteis**

- O fechamento/portas da raiz estão deslocados em relação ao anexo: escrita do fechamento `1996`, administrativa `2213`, sistema `2237`, residual `2329`. (`app/chamados/models.py:1996`, `:2213`, `:2237`, `:2329`)
- `pode_fechar` possui **14 chamadas diretas atuais**, incluindo relatório e explicador que agora leem o juiz completo. (`app/chamados/relatorios_disputa.py:24`; `app/chamados/services/explicador.py:139`; demais: `app/chamados/reconciliador.py:412`, `:612`, `:1171`; `app/ponto/services/entrada_sincronizada.py:59`; `app/chamados/services/materializacao.py:591`, `:1322`; `app/chamados/services/disputa_emissao.py:1278`, `:2098`; `app/chamados/services/competencia_trancada.py:178`, `:227`; `app/chamados/services/becos.py:33`, `:56`)
- O predicado de resposta sem veredito explicitamente mantém resposta carimbada sem texto: `respondida_em != None OU texto`, desde que não validada/materializada e não trancada. (`app/chamados/models.py:1899`, `:1905`)
- Para medir o caminho “pergunta viva em dona fechada” já existe a função real `vigia_fio.viva_em_disputa_fechada(empresas=None)`, que retorna perguntas e disputas distintas, com `q_vivas()` e julgamento final pelo mesmo `pergunta_viva`. (`app/chamados/services/vigia_fio.py:758`, `:769`, `:775`, `:780`)
- Para medir nascimentos sem perguntas encerrados, a função real é `vigia_fio.disputa_vazia()`, que conta motivo persistido `vazia`. (`app/chamados/services/vigia_fio.py:748`, `:755`)
- Não há número atual desses dois indicadores no envelope fornecido. [nao verificado]

## 15/11 00:45 · sessao 0c16e9d4443f · - · - · origem=Ronald (tmux)

Mapa consolidado; nenhum símbolo citado desapareceu nos arquivos examinados. Não há comparação histórica Git nesta cópia; estados abaixo conferem os sítios e significados descritos pelos anexos contra o código atual. [nao verificado quanto ao histórico]

**MUDOU — `app/chamados/models.py`; distinguir as duas bases dos anexos.**

| Citação antiga | Sítio atual | Conferência |
|---|---|---|
| r.r `:2437` | `:2514–2519` | Episódio BATIDA em coluna; escrito no nascimento e protegido contra apagar batida. |
| r.r `:2784` | `:2861–2864` | `data_evento` da pergunta datada é dia do turno; conversa tem porta própria. |
| r.r `:2795`, `:2802` | `:2872–2880` | `abrir_conversa`: dia civil do pedido, atribuído por `localdate()` em `:2879`. |
| matriz `:2850–2859` | `:2872–2880` | Mesmo nascimento de conversa; `conversa=True`, sem derivar episódio. |
| r.r `:2867` | `:2949–2951` | `_nascer` deriva/recebe episódio antes do juiz do dia. |
| r.r `:2876` | `:2958–2961` | Dia recebido de `fio.dia_do_turno_da_linha`, com episódio e `pedido=data_evento`. |
| matriz `:2965–2976` | `:2992–3003` | `FatoEmOutroDia` trata episódio/fecho recusado noutro dia; **não** recusa genericamente uma data pedida divergente. O nascimento grava o dia do juiz (`:2938–2942`). |
| matriz `:2982` | `:3009` | Recusa episódio retratado antes de reusar a linha eleita. |
| r.r `:3450` | `:3591–3592` | `_chave_de_identidade` deriva origem se pergunta não é conversa e origem está ausente. |
| r.r `:3453` | `:3595` | Definição de `batida_origem_do_nascimento`. |
| cef/r.r `:3457` | `:3599–3601` | Contexto fundido pode apontar outro episódio; acervo não ganha origem por essa derivação posterior. |
| cef `:3462–3466`; r.r `:3464` | `:3603–3608` | Família BATIDA e leitura de `contexto.ultima_batida.id`, exatamente em `:3606`. |
| r.r `:3468` | `:3610` | PK só retorna se batida pertence ao colaborador da disputa. |
| cef/r.r `:3490` | `:3631–3636` | Índice por `(batida_origem, motivo)`, parcial, com horizonte, sem conversa e fora das vias retiradas do fato. |
| matriz `:3508–3523` | `:3595–3610` | Derivador do episódio: família, envelope, existência e colaborador. |
| matriz `:3537–3550` | `:3624–3636` | Índices por família; BATIDA exige origem não nula e dia `>= HORIZONTE_DO_FATO`. |
| r.r `:3604` | `:3746` | Literal `foto_ausente` do catálogo legado de perguntas; não é envelope. |
| r.r `:4094` | `:4235–4236` | Reexportação da porta e do gerador de `services.disputa_emissao`. |

**Mudança relevante ao nascimento com E3:** há recusa de `respondida_em` sem texto antes das travas em `app/chamados/models.py:2919–2923`; `save` também chama a guarda em `:3529`, com regra documentada em `:3536–3540`. O nascimento persiste o dia via escritor único em `:3096–3100` e registra projeções de decisão em `:3101–3102`.

**SEGUE — demais referências examinadas.**

| Arquivo e referências citadas | Conferência atual |
|---|---|
| `app/chamados/juizes.py:138`, `:141–168`, `:151`, `:159–166`, `:226` | Mesmos locais; carimbos fracos são último degrau. |
| `app/api/foto_recebida.py:28–54`, `:51`, `:57–85`, `:61–69`, `:67–69`, `:70–73`, `:73`, `:79–83` | Mesmos locais; seleciona disputa pela última batida, valida conjunto de perguntas e apara/fecha disputa. |
| `app/ponto/turnos.py:2927–2936`, `:2928`, `:2935`, `:2955` | Mesmo adaptador do juiz; batida não reclamada usa data local, retratada fica fora do universo. |
| `app/chamados/catalogo/motivos.py:16`, `:35`, `:43`, `:53` | Mesmos locais; identidade BATIDA por episódio, triagem MARCO e família no catálogo. |
| `app/chamados/catalogo/query.py:45–66` | Mesmo texto interpolado; template sem `{data}` retorna texto em `:56–57`. |
| `app/chamados/catalogo/perguntas.py:93–98` | Mesmo texto de `foto_ausente`, tipo escolha, sem data interpolada. |
| `app/chamados/tests/test_e1_v5_familia.py:25`, `:98` | Mesmo selo: AST da declaração `Motivo(familia=...)` e chave dos índices; não confere envelope dos emissores. |
| `app/chamados/tests/test_e1_v5_indice.py:69` | `CorridaCruaNoIndiceTest::test_MORDE_batida_duas_cruas_no_mesmo_episodio_dao_UMA_linha`, mesmo teste. |
| `app/chamados/tests/test_e1_v5_indice.py:84` | `IndiceNaoSobrepoeTest::test_episodios_diferentes_no_mesmo_dia_sao_duas_linhas`, mesmo teste. |
| `app/chamados/tests/test_e1_v5_indice.py:96` | `IndiceNaoSobrepoeTest::test_conversa_e_a_linha_fora_do_fato_nao_entram`, mesmo teste. |
| `app/chamados/tests/test_a2_data_evento_batida.py:22` | `DataEventoBatidaTest::test_pergunta_de_batida_nasce_datada_e_dedupa`; duas origens distintas e reemissão da mesma em `:34–44`. |
| `app/chamados/tests/test_a2_data_evento_batida.py:53` | `DataEventoBatidaTest::test_contexto_sem_ultima_batida_nasce_conversa`; confere data civil em `:56`. |
| `app/chamados/tests/test_e1_v6_horizonte.py:262` | `EmissorBatidaPorEpisodioTest::test_RED_segunda_batida_na_disputa_aberta_ganha_pergunta`; origens em `:272–273`, repetição em `:275–276`. |
| `app/chamados/tests/test_cenarios_disputa.py:43` | `HelperTextoPerguntaTest::test_cenario_11_foto_ausente_sem_data`; texto sem data em `:44–47`. |
| `app/chamados/tests/test_b8_smoke_portas.py:60` | `test_upload_foto_carimba_batida_e_fotobatida`; verifica Batida.foto e FotoBatida em `:68–69`, não fechamento de disputa. |
| `app/chamados/tests/test_porta_chamado_e3.py:58`, `:62` | Mesmo teste da porta `aparar_motivo_respondido('foto_ausente')`, repetição idempotente; não é teste do upload. |
| `app/docs/JUIZES.md:14`, `:20`, `:38` | Mesmo lookup: vivo prevalece, geometria do turno e dono de validação. |

**Duas referências ao registro vivo MUDARAM:** `app/core/juizes.py:208` do r.r, que pretendia registrar `turnos_do_colab`, corresponde agora a `app/core/juizes.py:212`; a entrada de `selfie_ausente` antes citada em `:1199` fica em `app/core/juizes.py:1205`.

**Correção necessária da matriz D1:** o juiz fraco lê `ChamadoColaborador.contexto_json`, não diretamente `DisputaSupervisao.contexto`: leitura singular em `app/chamados/juizes.py:100`, chamada em `:138`; lote em `:226`. A ponte no nascimento do chamado existe em `app/chamados/services/disputa_emissao.py:580`; a disputa recebe o contexto separadamente em `:590`. A agregação de disputa escreve seu contexto em `:562–565` e não atualiza o contexto do chamado nesse ramo (`:535–566`).

**Semântica temporal:** o campo da API contém recepção: `agora=timezone.now()` em `app/api/views.py:497`, serializado em `:761`. O leitor fraco extrai `.date()` do ISO **sem converter fuso** em `app/chamados/juizes.py:166`; portanto obtém o dia civil do offset serializado. O retro serializa o instante da batida convertido para local em `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`. Esse dia civil, o instante e o dia do turno têm contratos distintos (`app/ponto/turnos.py:2928–2936`).

**Escritores/leitores adicionais indispensáveis ao CENSO:**

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `batida_origem_id` | `app/chamados/models.py:2949–2951`; `:3591–3592`; persistência do nascimento em `:3100` | `models.py:2795–2804` dedup por episódio/acervo do dia; `fio.py:1295` chave do episódio; `:1526–1533` pergunta o dia ao juiz; `:1554–1570` verifica episódio sem dia; `:1685–1687` dia/recusa; `:1846` universo do fato; `models.py:3631–3636` índice. |
| `data_evento` | Atribuição única `app/chamados/services/fio.py:839`; nascimento chama em `models.py:3099`; conversa passa data civil em `models.py:2879`; portas de mudança declaradas em `fio.py:835–837` | `fio.py:1660–1702` resolve dia do fato; `fio.py:1178–1187` contrato de saída; `models.py:2798–2804` chave para localizar linhas; `juizes.py:76–83` exclui conversa e lê dias dos fatos. |
| `colaborador_id` da pergunta | `app/chamados/models.py:3573–3578` deriva da disputa e recusa divergência | `models.py:3610` confere posse da batida; `fio.py:1527` chave do cache leva colaborador e episódio; `fio.py:1705–1714` lê dono da linha. |
| `instancia_marco` / `conversa` | `app/chamados/models.py:3579–3589` deriva instância; portas datada/conversa em `:2867` / `:2879`; nascimento em `:3097` | `fio.py:1669–1677` separa conversa/dia civil, episódio/dia do turno e MARCO/célula; `models.py:3625–3636` condições dos índices. |
| Resposta automática / validação no upload | `app/api/foto_recebida.py:48` chama porta da resposta; `:51` chama dono da validação | Escopo de seleção é disputa em `:67–69`, todas as perguntas de foto em `:73` ou todas da disputa em `:79`; fechamento em `:80`. |

Para B7, há teste do ramo de episódio retratado com `gps_ausente`, em `app/chamados/tests/test_e1_v6_horizonte.py:445–454`; cobertura específica de `foto_ausente` continua [nao verificado]. O gerador pode fazer conversa quando não obtém dia (`app/chamados/services/disputa_emissao.py:431–441`); a porta datada recusa origem retratada pelo juiz (`app/chamados/services/fio.py:1685–1687`).

Execução dos testes e comportamento de produção: [nao verificado]. [nao verificado]

## 15/11 00:45 · sessao 4ede009033c2 · - · - · origem=Ronald (tmux)

Auditoria estática dos emissores e da porta concluída; nenhuma alteração realizada, nenhuma aplicação importada, nenhum teste executado **[nao verificado em arquivo — registro desta execução]**. [nao verificado]

### Mapa das citações sob minha responsabilidade

Todas as referências abaixo **SEGUEM**, no mesmo arquivo, linha e significado:

| Arquivo | Citações conferidas | Significado |
|---|---|---|
| `app/ponto/management/commands/marcar_foto_ausente_retro.py` | `:45`, `:65`, `:66-69`, `:70-72`, `:74-77`, `:81`, `:89-92`, `:95-106`, `:96`, `:98-102` | Janela de três durações de 24h por padrão; bordas inclusivas; filtro de origem/retração; foto por ambos os registros; juiz compartilhado; dry-run; emissor entrega somente chave do motivo, sem `ultima_batida`. |
| `app/ponto/registro_batida.py` | `:72`, `:75-77`, `:80-91`, `:83`, `:89`, `:91` | PWA=0, APK/NULL=90; override só APK/NULL; foto por OR; vencimento `>=`. A origem `('app',)` está em `:71`. |
| `app/config/crons.py` | `:686` | Cron `26 7 * * *`, comando retro com `--apply`. A referência da matriz é relativa a `app/`, como declarado no próprio anexo. |
| `app/api/views.py` | `:447-453`, `:453`, `:507`, `:655`, `:758-766`, `:759`, `:760`, `:761` | Juiz no ato; instante efetivo da batida separado de `agora`; PK/tipo/recepção no envelope; chamada da porta. |
| `app/api/views.py` | `:833-859` | **SEGUE** o intervalo da matriz: decorators `:833-834`, definição `api_upload_foto :835`, anexo da foto `:847`, resolução da disputa `:857`, resposta `:859`. Não há prova de mudança só porque a definição está em `:835`: o anexo citou um intervalo. |
| `app/api/views_core.py` | `:439`, `:752`, `:753`, `:754`, `:755`, `:758` | API gêmea com o mesmo envelope e mesma separação entre instante efetivo e recepção. |
| `app/ponto/services/triagem_batida.py` | `:735`, `:739` | Emissor de `saida_sem_entrada`, com dia do juiz em `:740` e hora civil da tentativa em `:741`; delegação de dia em `:729-730`. |
| `app/chamados/services/fabrica_da_celula.py` | `:71`, `:139`, `:172`, `:174` | Motivos por marcos da ata; envelope `{motivo: {'data': dia.isoformat()}}`; porta e gerador. |
| `app/chamados/management/commands/smoke_chamados.py` | `:103` | Chamada direta do gerador. |
| `app/chamados/services/disputa_emissao.py` | `:72`, `:294`, `:300`, `:325-329`, `:326`, `:371-377`, `:372`, `:387-392`, `:436-441`, `:436-438`, `:470-471` | Leitura do episódio para dia; gerador; dedup sem episódio; data de motivo antes da batida; conversa quando falta dia; retorno das perguntas. |
| Mesmo arquivo | `:535-566`, `:548`, `:552-554`, `:556`, `:557`, `:558`, `:560-561`, `:562`, `:564-565` | Agregação; preservação de chaves antigas ausentes no payload; merge dos motivos; contador por emissão; recepção do merge; substituição das demais chaves de topo; persistência. |
| Mesmo arquivo | `:568-603`, `:568`, `:587-592`, `:590`, `:597`, `:634-688`, `:634`, `:659`, `:664`, `:670`, `:682` | Nascimento do chamado/disputa, trilha de abertura, porta pública, normalização de motivos e duas chamadas ao gerador. |

**Nenhum SUMIU encontrado neste conjunto.** As linhas do modelo que encontrei mudaram, como já encaminhado: chamadas `2867→2949`, `3450→3592`; definição `3453→3595`; leitor `3464→3606` (`app/chamados/models.py:2949`; `app/chamados/models.py:3592`; `app/chamados/models.py:3595`; `app/chamados/models.py:3606`).

### Funções, escritores e leitores

| Campo/contrato | Escritores atuais | Leitores atuais / significado |
|---|---|---|
| `ultima_batida.id` | API principal `app/api/views.py:759`; gêmea `app/api/views_core.py:753`; persistência genérica no nascimento `app/chamados/services/disputa_emissao.py:590` e agregado `:561-565`. | Episódio do colaborador no derivador `app/chamados/models.py:3606`; episódio para dia do turno no gerador `app/chamados/services/disputa_emissao.py:387-392`; referência para grade `:72-76`; seleção da disputa no upload `app/api/foto_recebida.py:68`. |
| `ultima_batida.tipo` | `app/api/views.py:760`; `app/api/views_core.py:754`. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `ultima_batida.timestamp` | `app/api/views.py:761`; `app/api/views_core.py:755`. É `agora`, instante da recepção, proveniente de `app/api/views.py:497` / `app/api/views_core.py:483`. | `_dia_do_contexto_fraco` lê em `app/chamados/juizes.py:160` e extrai `.date()` em `:166`: dia civil do timestamp recebido, sem converter esse instante ao juiz do turno. |
| `foto_ausente.tipo_batida` | `app/api/views.py:453`; `app/api/views_core.py:439`; retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:99`. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `foto_ausente.batida_id` | Retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:100`: PK da batida. | Nenhum leitor específico dessa chave aninhada localizado **[nao verificado quanto a leitura dinâmica/genérica]**. Os consumidores de episódio leem `ultima_batida.id` (`app/chamados/models.py:3606`; `app/chamados/services/disputa_emissao.py:387`). |
| `foto_ausente.batida_em` | Retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`: instante efetivo da batida convertido ao fuso local e serializado ISO. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `foto_ausente._por` | Retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:102`: proveniência fixa do passe. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `contexto[motivo].data` | Fábrica `app/chamados/services/fabrica_da_celula.py:172`; triagem `app/ponto/services/triagem_batida.py:740`; enriquecimentos do gerador `app/chamados/services/disputa_emissao.py:292`, `:353`. | Dia de turno/furo para gerador `app/chamados/services/disputa_emissao.py:372-375`; destino `:191-196`. Retro não grava esta chave (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`). |
| `contexto[motivo].qtd` | `_abrir_ou_agregar_disputa`, `app/chamados/services/disputa_emissao.py:552-557`: default anterior 1 e incremento em cada merge do mesmo motivo. | O próprio merge lê o contador anterior em `:552`; não é cálculo de batidas distintas (`:553-557`). Outro leitor semântico do envelope não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `contexto[motivo].ultima_em` | `app/chamados/services/disputa_emissao.py:558`: `now()` do merge. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `Batida.timestamp` no caminho das APIs | Principal passa `ts_efetivo` à porta em `app/api/views.py:655`; gêmea em `app/api/views_core.py:646`. | Retro mede minutos reais `(agora - b.timestamp).total_seconds()/60` em `app/ponto/management/commands/marcar_foto_ausente_retro.py:76`; janela inclusiva em `:67`; instante local transportado em `:101`. |
| `tem_foto` | Retro computa OR em `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; API no ato informa ausência em `app/api/views.py:450` / `app/api/views_core.py:436`. | Juiz recusa cobrança com foto em `app/ponto/registro_batida.py:86`. |
| `decorrido_min`, `prazo_apk_min` | Retro fornece minutos e override em `app/ponto/management/commands/marcar_foto_ausente_retro.py:76-77`; APIs fornecem zero em `app/api/views.py:450` / gêmea `:436`. | Juiz escolhe prazo em `app/ponto/registro_batida.py:88-90` e compara inclusivamente em `:91`. |
| `--dias` | Argumento inteiro em `app/ponto/management/commands/marcar_foto_ausente_retro.py:54`. | Duração desde agora em `:65`, filtro inclusivo `:67`; não é contagem de dias do turno. |
| `origem` da porta | APIs `app/api/views.py:765`, `app/api/views_core.py:759`; retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:104`; triagem `app/ponto/services/triagem_batida.py:744`; fábrica `app/chamados/services/fabrica_da_celula.py:172`. | Texto e trilha `app/chamados/services/disputa_emissao.py:664`, `:597-599`; não substitui identidade do episódio. |

As cinco chamadas da porta permanecem nas APIs `app/api/views.py:764`, `app/api/views_core.py:758`, retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, triagem `app/ponto/services/triagem_batida.py:735`, fábrica `app/chamados/services/fabrica_da_celula.py:172`.

As cinco chamadas do gerador permanecem em `app/chamados/management/commands/smoke_chamados.py:103`, `app/chamados/services/disputa_emissao.py:294`, `:670`, `:682`, `app/chamados/services/fabrica_da_celula.py:174`.

O AST leu 892 arquivos sem erro sintático, contra os 887 registrados no anexo; contagem instrumental **[nao verificado em arquivo]**. Mantém o alcance limitado por literais/nomes/aliases de importação; despacho dinâmico **[nao verificado]**.

### Provas citadas na matriz: existência e alcance

Todos os testes citados abaixo **SEGUEM**, como código lido; resultado de execução **[nao verificado]**. [nao verificado]

| Referência da matriz | Localização atual e limite |
|---|---|
| A1 `test_dry_run_nao_escreve` | `app/ponto/tests/test_bug126_selfie_invisivel.py:103`: testa ausência de disputa, não ausência de toda escrita. |
| A3 retroativa | `app/ponto/tests/test_bug126_selfie_invisivel.py:91`: exclui origem retroativa; não testa `retratada_em`. |
| A4 manual / juiz origem | `app/ponto/tests/test_bug126_selfie_invisivel.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:45`. |
| A5 FotoBatida | `app/ponto/tests/test_bug126_selfie_invisivel.py:74`; `app/ponto/tests/test_selfie_ausente_juiz.py:103`. |
| A6 Batida.foto | `app/ponto/tests/test_selfie_ausente_juiz.py:112`. |
| A7 prazo / NULL | `app/ponto/tests/test_bug126_selfie_invisivel.py:84`; `app/ponto/tests/test_selfie_ausente_juiz.py:41`. |
| A8 emissor retro | `app/ponto/tests/test_bug126_selfie_invisivel.py:65`, asserts `batida_id`/`_por` em `:71-72`; não confere pergunta/episódio. |
| A9 agregação PWA | `app/ponto/tests/test_selfie_ausente_juiz.py:119`; payload sintético em `:122` também omite `ultima_batida`. Confere número de disputas, não episódio. |
| A10 repetição | `app/ponto/tests/test_bug126_selfie_invisivel.py:108`, assert `:113`: somente número de disputas. |
| A12 API PWA | `app/api/tests/test_api_bater_pwa.py:64`, `:78`, `:95`: presença de motivo, contador e ausência com foto; não confere identidade da pergunta. |
| A13 prazo delegado | `app/ponto/tests/test_selfie_ausente_juiz.py:82`: troca prazo do juiz e observa retro. |
| B1 conversa genérica | `app/chamados/tests/test_a2_data_evento_batida.py:53`: motivo usado é `batida_fora_escala_grave`, sem retro; assert `data_evento=localdate()` em `:56`. |
| B4 índice episódio | `app/chamados/tests/test_e1_v5_indice.py:69`: corrida crua, motivo `gps_ausente`; assert de origem em `:74`. |
| B5 episódios diferentes | `app/chamados/tests/test_e1_v5_indice.py:84`; `app/chamados/tests/test_a2_data_evento_batida.py:22`. |
| B6 segunda batida | `app/chamados/tests/test_e1_v6_horizonte.py:262`: usa contexto sintético com instante da batida `:260`, não o `agora` da API; não executa retro. |
| B9 texto foto | `app/chamados/tests/test_cenarios_disputa.py:43`. |
| B10 conversas fora do índice | `app/chamados/tests/test_e1_v5_indice.py:96`: confere permissividade do índice, não dedup sequencial do gerador. A repetição de conversa do episódio retratado tem teste próprio em `app/chamados/tests/test_e1_v6_horizonte.py:445`, com motivo `gps_ausente` em `:451`. |
| C1/C4 upload | `app/chamados/tests/test_b8_smoke_portas.py:60`: teste existe; fecho da disputa não demonstrado por esta referência **[nao verificado quanto ao fecho no teste]**. |
| C2 porta aparar | `app/chamados/tests/test_porta_chamado_e3.py:56`, asserts em `:58`, `:62`: porta diretamente, sem upload. |
| Selo delegação selfie | `app/ponto/tests/test_selfie_ausente_juiz.py:130`: AST só nas duas APIs, não prende envelope do retro. |

### Distinções que não podem virar resposta de desenho

- A8/B1 “estado pretendido” está apresentado como cura no anexo, enquanto o código atual apenas fornece o payload incompatível e cai na conversa se não houver data/episódio; escolha do transporte continua pergunta D1 (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:387`; `:431-441`).
- A9/B3 preservam o risco de contexto anterior porque chaves ausentes no payload não são removidas; modelo preciso evento/agregado continua pergunta D2 (`app/chamados/services/disputa_emissao.py:548-561`).
- A10 “2x = mesmo estado” excede a prova do teste: o contador e `ultima_em` mudam a cada merge; sem corte específico, não transformar essa frase em decisão D6 (`app/chamados/services/disputa_emissao.py:552-558`; `app/ponto/tests/test_bug126_selfie_invisivel.py:113`).
- “Instante do fato” e “recepção” continuam distintos no próprio código; qual significado deve ter o envelope é D3, sem resposta aqui (`app/api/views.py:507`; `:655`; `:761`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`).
- B7 “conversa ou recusa” são ramos distintos: gerador conhece episódio mas juiz sem dia → conversa, com dedup de conversa; a recusa do modelo requer conferir a entrada datada (`app/chamados/services/disputa_emissao.py:431-441`; `app/chamados/tests/test_e1_v6_horizonte.py:445`).
- “SEM PROVA” dos anexos deve ser registrado como **teste específico não demonstrado**, não como inexistência universal de prova **[nao verificado quanto a todos os testes indiretos/dinâmicos]**. [nao verificado]

## 15/11 00:44 · sessao f865e2f688d8 · - · - · origem=Ronald (tmux)

Censo dos escritores da cópia, sem alterações:

| Função | Escrita de datas |
|---|---|
| `executar_vinculo` | Reabre `data_fim=None` em `app/colaboradores/services/vinculo.py:277`, persiste em `:283`; cria com `data_inicio` em `:287`/`:291`; fecha vínculos pela porta em `:214` e `:241`. |
| `fechar_vigencia` | Atualiza `data_fim` em `app/colaboradores/services/vinculo.py:641`, restringindo a `data_inicio <= data_fim`. |
| `reabrir_vigencia_impossivel` | Atualiza `data_fim` em `app/colaboradores/services/vinculo.py:730`; valida em `:724`. |
| `encerrar_vinculo` | Atribui e salva `data_fim` em `app/colaboradores/services/vinculo.py:844`/`:845`; valida em `:835`. |
| `criar_trecho_retroativo` | Cria com ambas as datas em `app/colaboradores/services/vinculo.py:970`/`:972`; valida em `:969`. |
| `corrigir_vinculo_retroativo` | Atribui e salva `data_fim` em `app/colaboradores/services/vinculo.py:982`/`:983`; valida em `:979`. |
| `reverter_para_snapshot` | Reescreve ambas as datas em `app/colaboradores/services/vinculo.py:1031`/`:1032`; valida cada par em `:1015`. |
| `ajustar_vinculo_pelo_sistema` | Fecha pela porta em `app/colaboradores/services/vinculo.py:1087`; cria com início em `:1089`/`:1090`. |
| `desfazer_ajuste_do_sistema` | Reabre fim nulo em `app/colaboradores/services/vinculo.py:1106`; fecha pela porta em `:1108`; cria com início em `:1110`/`:1111`. |
| `fechar_vinculos_ativos` | Delega a `fechar_vigencia` em `app/colaboradores/services/vinculo.py:862`; seleciona somente `ativa=True` em `:863`. |
| `gravar_regua` | Porta genérica aceita `**campos` em `app/colaboradores/services/vinculo.py:1048` e atualiza sem allowlist em `:1070`; seus chamadores atuais passam campos de régua em `app/escala/regua_defesa.py:662`, `:719`, `:742`, `:765`. |

Rotas que chegam a esses escritores:

- Wizard: `app/colaboradores/views.py:423`; chat: `app/colaboradores/services/troca_pelo_chat.py:75`; régua: `app/escala/regua_defesa.py:321`; comando de fase: `app/ponto/management/commands/censo_fase_12x36.py:423`.
- Desvinculação: `app/escala/views.py:269`; backfill de desligados: `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`; desligamento: `app/colaboradores/services/desligamento.py:148`.
- Retroativo: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196` e `:212`; snapshot: `app/chamados/services/acoes_chamado.py:149`.
- Automático: `app/escala/services/escala_auto_executor.py:103` e `:145`; reparo: `app/escala/management/commands/corrigir_vinculo_vigencia.py:86` e `:107`.

O admin de EC herda `SoLeitura` em `app/escala/admin.py:19`; adição, alteração e exclusão retornam `False` em `app/core/admin.py:59`, `:62`, `:65`. O bootstrap cria Empresa, Setores e User em `app/tenants/bootstrap.py:25`, `:32`, `:34`.

Há escritores de fixtures em testes, por exemplo `app/tenants/tests.py:44`, `app/colaboradores/tests/test_ficha_do_colab.py:88` e `app/colaboradores/tests/test_situacional_pergunta_em_lote.py:62`. A varredura AST contou 829 sítios diretos com keywords dessas datas em 643 arquivos de teste; não existe artefato persistido dessa contagem [não verificado].

A cópia já declara a constraint em `app/escala/models.py:1037` e contém migration com `CHECK ... NOT VALID` em `app/escala/migrations/0041_ec_vigencia_not_valid.py:43`. Aplicação em prod [não verificado].

## 15/11 00:44 · sessao db1f8fd7598c · - · - · origem=Ronald (tmux)

Textos escritos que sustentam casos de especificação, sem usar o código como gabarito: a L-110 exige “cadastro tal, batidas tais” e manda a pergunta sem resposta pela regra para a lista separada (`app/docs/LEIS.md:136`).

| Transição do mapa | Caso escrito pela regra → esperado | Texto da regra |
|---|---|---|
| N1 + P1 | Cadastro com intervalo fixo; entrada, retorno do intervalo e saída registrados, saída para intervalo ausente; pergunta inexistente → emitir UMA pergunta do marco apagado, a partir do sinal da célula. | “lampada apagada no miolo passa a gerar UMA pergunta […] nascida do sinal da celula (nunca de varredura)” (`app/docs/CORTES.json:772`). |
| N1 — selfie | Cadastro de colaborador com batida de aparelho APK sem foto; prazo de 90 minutos vencido → cobrança abre disputa; em PWA, ausência de foto no ato → cobrança abre disputa. | “foto ausente e COBRANCA (abre disputa […]): […] prazo do CANAL (PWA = no ato; APK = 90 min)” (`app/docs/CORTES.md:94`). |
| P2 — dedup | Mesmo cadastro, dia, marco faltante e pergunta do mesmo fato; sinal repetido → uma pergunta desse fato, sem duplicação. O texto não decide a dona fechada nem autoriza reapontamento arbitrário. | “UMA pergunta” e “dedup pela chave do fato” (`app/docs/CORTES.json:772`). |
| F1 | Cadastro com dia já resolvido; perguntas do questionário sem viva e nenhuma resposta sem veredito → disputa elegível a fechar; resposta sem veredito → fechamento recusado. | “fecha quando `pergunta_viva(disputa)` esta vazio (C6), e a porta […] recusa resposta sem veredito” (`app/docs/BACKLOG.md:1549`). O motivo persistido `vazia` para zero perguntas não recebe gabarito específico nesse texto. |
| F1 — recusa por idade | Cadastro e batidas ainda deixando fato sem solução; pergunta muda há 30 dias → idade não absolve nem fecha; pergunta parada vai à lista administrativa. | “silencio de 30 dias e QUALQUER fechamento por idade MORREM como absolvicao — pergunta parada vira lista do admin, nao fechamento” (`app/docs/BACKLOG.md:1549`). |
| F2 + P10 no desligamento | Cadastro desligado com pergunta já respondida sem veredito → encerrar também essa pergunta por via própria de desligamento e listar dia/pergunta/resposta no chamado de revisão; muda também recebe via própria. | O corte antigo de manter respondida viva foi substituído (`app/docs/PROMPTS.md:2222`); novo conteúdo registrado em `app/docs/BACKLOG.md:732`; “via propria desligamento tambem para as mudas” (`app/docs/CORTES.json:1610`). Não declarar a cura implementada com base nessa regra. |
| F3 — respondida | Cadastro ativo, pergunta respondida sem decisão do dia; mero encerramento administrativo da disputa → a regra escrita de 28/10 rejeita tratá-la como decisão humana que não ocorreu. A extensão exata da revogação de 13/11 ao `admin_encerra`, fora de desligamento, fica [nao verificado]. | “encerrar a disputa […] nao decide o dia” e proibição de `admin_decidiu` em já respondida (`app/docs/CORTES.json:1258`); o corte sucessor trata desligamento (`app/docs/BACKLOG.md:732`). |
| F4 / tranca por leitura | Cadastro com dia em competência trancada, sem toque humano posterior → C6 lê a tranca sem novo carimbo; destrancar devolve vitalidade; toque humano posterior exclui aquele colab-dia da tranca. | “C6 LE a tranca […] sem carimbo e sem estado novo; destrancar devolve a pergunta viva” (`app/docs/PROMPTS.md:1338`); exceção do toque humano (`app/docs/PROMPTS.md:1342`). A escolha de fechar a raiz pelo motivo `competencia_trancada` continua sem gabarito específico nessas cláusulas. |
| R1/R2 — desligado | Cadastro desligado e disputa fechada → não reabrir a disputa do desligado. | “so chamado; disputa de desligado nao reabre” (`app/docs/CORTES.json:1617`). O texto não decide as demais reaberturas. |
| M2 + P4, por episódio de foto | Cadastro com batida que originou cobrança de foto; foto entregue, inclusive fora do prazo → extinguir a cobrança do episódio identificado por `batida_origem`. | “por episodio via batida_origem” e “foto fora do prazo tambem extingue” (`app/docs/CORTES.json:1596`). O texto não determina a cadeia exata de campos de resposta/validação nem o fechamento de toda raiz. |
| P3, primeira resposta humana | Cadastro ativo, pergunta sem resposta; admin tenta dar a primeira resposta em nome do colaborador → não permitido. | “admin nao da a primeira resposta pelo colab” (`app/docs/CORTES.json:1659`). Essa cláusula não responde ao caso de nova resposta invalidar validação anterior. |
| P5 + P9 — intervalo | Cadastro com intervalo fixo faltante, pergunta respondida com hora e validada pelo admin → registrar intervalo e retirar indenização do dia; sem resposta ou recusada → indenizar pela política escrita. | “Resposta validada pelo admin vira o registro do intervalo e a indenizacao do dia sai; sem resposta ou recusada […] Art. 71 par. 4, indeniza” (`app/docs/CORTES.json:772`). Não usar isso como regra genérica de plausibilidade/folga de toda pergunta. |
| P9 — fato que chega após resposta | Cadastro coerente com escala/posto, pergunta respondida, batida posterior acende o marco: concorda com a resposta pela régua de validação automática → pergunta morre sozinha; discorda → permanece viva para admin. | Corte literal registrado em `app/docs/BACKLOG.md:1731`. |
| P10 — duplicata com episódio | Mesmo cadastro, batida/dia/motivo comprovadamente iguais; pergunta COM episódio e outra SEM episódio → conservar a COM episódio e retirar a SEM pela via fora do fato, com trilha; sem prova de mesmo fato, conservar ambas. | D8 RAIZ (`app/docs/BACKLOG.md:2016`). Essa regra não responde a todas as outras vias de identidade. |
| Destino F1 | Cadastro, dia e fato únicos, dois chamados candidatos do mesmo fato com PKs distintos; mesma pergunta reapresentada em ordens diferentes → destino eleito pelo fato, empate pelo menor PK; um escritor. | “escritor unico […] eleitor por fato com empate menor PK” (`app/docs/PROMPTS.md:2085`); L-007: “campo de estado tem 1 escritor canonico + tripwire” (`app/docs/LEIS.md:65`). |
| T1 — trilha de ação crítica | Mesmo cadastro/batidas; ação crítica muda estado → registrar usuário, antes/depois e motivo. | “acao critica grava usuario, antes/depois e motivo” (`app/docs/LEIS.md:65`). Isso não define sozinho os oito atos nem imutabilidade da sequência. |

A regra da L-033 dá um caso de conservação: cadastro com jornada, entrada e saída presentes e só intervalo faltando → dia pagável e pergunta segue aberta (`app/docs/LEIS.md:81`). Não transformá-la em autorização genérica para fechar a pergunta.

Os invariantes escritos para a matriz são: aberta tem ao menos uma viva; push somente com viva pendente daquele colaborador; eleitor único do destino; fonte única do dia (`app/docs/CORTES.json:1694`). Eles permitem casos positivos/negativos de emissão e push, mas não resolvem a escolha entre agregar à dona fechada, reabrir ou criar outra: essa decisão permanece expressamente a pergunta N5 da F1 (`app/docs/CORTES.json:1708`).

Transições/partes sem resposta normativa específica encontrada nos textos examinados e que devem ir a C, preservando o comportamento atual somente em A:

- N2: critério de agregar motivos/contexto à mesma raiz; N3: motivo/pergunta novo em dona fechada; N1: quando criar raiz vazia e como tratar geração sem pergunta viva — N5 já está preservada no corte (`app/docs/CORTES.json:1708`); escolha normativa adicional [nao verificado].
- F1: motivo `vazia` versus `veredito` e precisão de exceções; F3: mecanismo das mudas e alcance da revogação; F4: fechamento da raiz por tranca, além de C6 por leitura [nao verificado].
- R1: teto administrativo 2 e justificativa mínima 20; R2: conjunto de causas sistêmicas além da proibição para desligado; R3: teto colaborador 2 e critérios exatos de corrigibilidade [nao verificado].
- M1: destino das perguntas na retratação de motivo; M2: campos exatos do automatismo da foto, além da extinção por episódio [nao verificado].
- P2: troca de célula/destino e conservação da dona; P3: sobrescrita de respondida/validada e invalidação anterior; P4: reparo pela trilha e data do carimbo [nao verificado].
- P5: demais tipos de resposta, plausibilidade e ramo folga; P6: devolução da validação recusada e reabertura da dona; P7: motivo obrigatório e limpeza da resposta na recusa; P8: limpeza completa na correção/desabsorção [nao verificado].
- P9: todas as demais vias e condições de carimbo; P10: `admin_decidiu`, `motivo_retratado`, `paridade_recusada`, `fato_ocupado` fora dos recortes normativos acima; P11: condições gerais de descarimbo/ressuscitação; P12/P13: troca de via e relavra [nao verificado].
- V1/V2: escolha dos três valores e política de falha/backfill; T1: oito atos e imutabilidade; I1: regra do preenchimento histórico de identidade [nao verificado].

Cuidado com cortes curtos sem a pergunta de origem: `LU-20 = nao validavel` e `LU-21 = reinicia` estão escritos, mas o referente completo não está nesses registros; não atribuir escopo inventado (`app/docs/CORTES.json:1673`, `app/docs/CORTES.json:1680`).

_5459 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
