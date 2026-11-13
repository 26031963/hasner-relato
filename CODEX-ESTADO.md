# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 21:41** · publicado em **12/11/2026 21:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 21:41 · sessao a7c71ef5ca12 · BUG-CADASTRO-C5E · verificador · origem=Code

**REPROVA.**

Os bloqueantes são dos **selos exigidos pelo aval**; não estou tratando os interiores expressamente adiados como autorização para ampliar a cura. A própria catraca declara esse adiamento. (`app/ponto/tests/test_cadastro_futuro_c5e.py:207`)

1. **A catraca deixa uma leitura nova sobreviver.** Acrescentar em `reconciliar_apos_vinculo`, antes da leitura existente, `_ler_dia = _tz.localdate` seguido de `_dia_extra = _ler_dia()` introduz outra leitura, mas o detector não resolve chamadas por variável atribuída: ele reconhece bases e funções provenientes de imports. O RED também não observa esse destino, substituído por `Mock`. (`app/colaboradores/services/pos_vinculo.py:56`; `app/ponto/tests/test_cadastro_futuro_c5e.py:278`; `app/ponto/tests/test_cadastro_futuro_c5e.py:303`; `app/ponto/tests/test_cadastro_futuro_c5e.py:310`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:350`)

2. **O RED obrigatório não protege as duas chamadas de `fechar_vigencia`.** Trocar somente a segunda chamada para `hoje=_hoje + datetime.timedelta(days=1)` não torna a diferença observável na fixture: ela prepara o vínculo **ativo**, sem uma inativa sobreposta que produza efeito na segunda chamada. Esse mutante viola diretamente o requisito do mesmo dia nas duas chamadas. (`app/colaboradores/services/vinculo.py:239`; `app/colaboradores/services/vinculo.py:245`; `app/ponto/tests/test_cadastro_futuro_c5e.py:59`; `app/ponto/tests/test_cadastro_futuro_c5e.py:72`)

Observação das execuções em memória, sem relatório persistido: suíte original **9/9 verde**; os dois mutantes acima também **9/9 verdes**. [nao verificado] [nao verificado]

A seguir, caminhos relativos a `app/`.

**1. Portas literais do aval**

| Porta | `hoje` keyword obrigatório, sem default | Recusa `None` antes dos efeitos | Lê dia no próprio corpo? |
|---|---|---|---|
| `regenerar_celulas_vinculo` | Sim — `ponto/portas/celula.py:112` | Sim — `ponto/portas/celula.py:138` | Não; usa o recebido — `ponto/portas/celula.py:193` |
| `regenerar_celulas_dia` | Sim — `ponto/portas/celula.py:741` | Por delegação imediata — `ponto/portas/celula.py:773` | Não — `ponto/portas/celula.py:773` |
| `reverter_regeneracao_dia` | Sim — `ponto/portas/celula.py:780` | Sim — `ponto/portas/celula.py:812` | Não; usa o recebido — `ponto/portas/celula.py:839` |
| `gerar_celulas_periodo` | Sim — `ponto/portas/celula.py:863` | Sim — `ponto/portas/celula.py:865` | Não — `ponto/portas/celula.py:908`, `ponto/portas/celula.py:921` |
| `regenerar_futuras_do_cadastro` | Sim — `ponto/services/cadastro_futuro.py:9` | Sim — `ponto/services/cadastro_futuro.py:24` | Não — `ponto/services/cadastro_futuro.py:26` |
| `regenerar_futuras_por_feriado` | Sim — `ponto/services/cadastro_futuro.py:49` | Sim — `ponto/services/cadastro_futuro.py:55` | Não — `ponto/services/cadastro_futuro.py:57` |
| `fechar_vigencia` | Sim — `colaboradores/services/vinculo.py:578` | Sim — `colaboradores/services/vinculo.py:616` | Não; repassa o recebido ao suplemento — `colaboradores/services/vinculo.py:656`, `colaboradores/services/vinculo.py:661` |

Os `timezone.now()` que permanecem na regeneração e reversão escrevem **instantes de regeneração**, não escolhem o dia: `ponto/portas/celula.py:263`, `ponto/portas/celula.py:308`, `ponto/portas/celula.py:834`.

O selo amplia sua lista para auxiliares cujo contrato de `None` não é verdadeiro:

- `desfazer_ajuste_do_sistema(..., hoje=None)` pode excluir o novo vínculo e reabrir o antigo sem recusar: o ramo `volta == novo.data_inicio` não delega ao fechamento. (`colaboradores/services/vinculo.py:1093`; `colaboradores/services/vinculo.py:1096`; `colaboradores/services/vinculo.py:1100`; `colaboradores/services/vinculo.py:1101`)
- `_trabalhar_o_par(..., hoje=None)` planta primeiro; em competência não exportada retorna sem alcançar a guarda da regeneração. `reparar` também não valida `None` na entrada. (`chamados/services/passivo_validado.py:316`; `chamados/services/passivo_validado.py:372`; `chamados/services/passivo_validado.py:427`; `chamados/services/passivo_validado.py:429`)
- Esses auxiliares estão em `PORTAS`, mas fora de `RECUSAM_NONE`; portanto o teste dispensa justamente a verificação necessária nesses casos. Isso é defeito da promessa adicional do selo, distinto das portas literais acima. (`ponto/tests/test_cadastro_futuro_c5e.py:104`; `ponto/tests/test_cadastro_futuro_c5e.py:108`; `ponto/tests/test_cadastro_futuro_c5e.py:112`; `ponto/tests/test_cadastro_futuro_c5e.py:138`)

**2. Relógio por ato e propagação**

A coluna “leituras” abaixo conta o trecho indicado; o total transitivo depende dos ramos, células e julgamentos executados. Não existe um total constante por ato completo. (`ponto/portas/celula.py:460`; `ponto/portas/celula.py:468`; `ponto/services/cartorio.py:2016`)

| Ato | Leituras e propagação |
|---|---|
| **Salvar vínculo** | `executar_vinculo` lê uma vez e passa `_hoje` às duas chamadas de fechamento, à regeneração e ao suplemento. (`colaboradores/services/vinculo.py:192`; `colaboradores/services/vinculo.py:217`; `colaboradores/services/vinculo.py:245`; `colaboradores/services/vinculo.py:328`; `colaboradores/services/vinculo.py:337`) |
| **Salvar completo** | A view chama antes `julgar_vinculo`, que lê `date.today()` para a janela anterior. Portanto o ato da view já pode ter duas leituras antes dos destinos adicionais. (`colaboradores/views.py:411`; `colaboradores/views.py:423`; `colaboradores/services/vinculo.py:71`) |
| **Desligamento pela view** | O juiz lê `date.today()` uma vez, mais outra se a data foi omitida; depois a view lê `localdate()` para o executor: **duas ou três leituras de dia** nesse trecho. A decisão `agendado` vem do dia anterior à leitura injetada. (`colaboradores/services/desligamento.py:20`; `colaboradores/services/desligamento.py:40`; `colaboradores/services/desligamento.py:58`; `colaboradores/views.py:1434`; `colaboradores/views.py:1437`) |
| **Desligamentos agendados** | Uma leitura inicial, mais uma no juiz por colaborador aplicado. O executor recebe o dia inicial do lote. (`colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`; `colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`; `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`; `colaboradores/services/desligamento.py:40`) |
| **Executor automático: aplicar/desfazer** | Uma leitura local se `hoje` não veio do chamador; o mesmo valor chega ao ajuste e à regeneração. `localdate(historico.criado_em)` é conversão de um instante fornecido. (`escala/services/escala_auto_executor.py:98`; `escala/services/escala_auto_executor.py:103`; `escala/services/escala_auto_executor.py:108`; `escala/services/escala_auto_executor.py:127`; `escala/services/escala_auto_executor.py:130`; `escala/services/escala_auto_executor.py:145`; `escala/services/escala_auto_executor.py:148`) |
| **`gerar_celulas`** | Uma leitura `date.today()`; mesmo valor para vigia, regeneração de hoje, geração e lavratura. (`ponto/management/commands/gerar_celulas.py:339`; `ponto/management/commands/gerar_celulas.py:346`; `ponto/management/commands/gerar_celulas.py:348`; `ponto/management/commands/gerar_celulas.py:349`; `ponto/management/commands/gerar_celulas.py:353`) |
| **`aplicar_col900`** | Uma leitura de dia para ambas as portas. Há leituras adicionais de instante na preparação/conclusão da reversão. (`ponto/management/commands/aplicar_col900.py:146`; `ponto/management/commands/aplicar_col900.py:186`; `ponto/management/commands/aplicar_col900.py:238`; `ponto/management/commands/aplicar_col900.py:70`; `ponto/management/commands/aplicar_col900.py:102`) |
| **`diff_col900`** | Uma leitura de dia, capturada pela closure; JSON e impressão final leem outros instantes. (`ponto/management/commands/diff_col900.py:57`; `ponto/management/commands/diff_col900.py:75`; `ponto/management/commands/diff_col900.py:119`; `ponto/management/commands/diff_col900.py:128`) |
| **`curar_residuo_bug115`** | Uma leitura de dia desce ao gerador; depois `_lavrar` chama o cartório sem testemunha temporal, e ele relê o dia. (`ponto/management/commands/curar_residuo_bug115.py:74`; `ponto/management/commands/curar_residuo_bug115.py:138`; `ponto/management/commands/curar_residuo_bug115.py:146`; `ponto/services/cartorio.py:2016`) |
| **`corrigir_vinculo_vigencia`** | Uma leitura na cabeça chega aos fechamentos; o passo de reabertura lê outros dias para histórico e suplemento. (`escala/management/commands/corrigir_vinculo_vigencia.py:48`; `escala/management/commands/corrigir_vinculo_vigencia.py:86`; `escala/management/commands/corrigir_vinculo_vigencia.py:108`; `colaboradores/services/vinculo.py:738`; `colaboradores/services/vinculo.py:750`) |
| **Passivo validado → exportada** | Uma leitura `localtime()` fornece carimbo e `hoje`; o mesmo dia desce por `reparar`, `_trabalhar_o_par`, exportada e regeneração. Ensaio acrescenta `now()` para localizar trilha. (`chamados/management/commands/materializar_passivo_validado.py:87`; `chamados/management/commands/materializar_passivo_validado.py:89`; `chamados/management/commands/materializar_passivo_validado.py:103`; `chamados/services/passivo_validado.py:373`; `chamados/services/passivo_validado.py:439`; `ponto/portas/regen_exportada.py:179`) |

**Contraexemplos concretos ao “mesmo dia em todo efeito”:**

- Regeneração e reversão chamam `_o_chao_do_dia_mudou` sem `hoje`. Ele relê `localdate()` e decide os dias julgáveis com esse novo valor; chama então um cartório que também lê seu próprio dia. Assim, um D+1 tratado como futuro na regeneração pode tornar-se elegível para julgamento no rabo do mesmo ato. (`ponto/portas/celula.py:312`; `ponto/portas/celula.py:392`; `ponto/portas/celula.py:464`; `ponto/portas/celula.py:469`; `ponto/portas/celula.py:472`; `ponto/portas/celula.py:858`; `ponto/services/cartorio.py:2016`)
- O pós-vínculo não recebe `_hoje`: lê outro dia, escolhe outro teto e reapura usando esse valor. (`colaboradores/services/vinculo.py:345`; `colaboradores/services/pos_vinculo.py:56`; `colaboradores/services/pos_vinculo.py:57`; `colaboradores/services/pos_vinculo.py:82`)
- Na edição, `existente.save()` pode alcançar os signals: o de vigência lê **duas vezes**; o de âncora lê uma vez. As portas recebem esses dias, não o da cabeça do Salvar. (`colaboradores/services/vinculo.py:281`; `escala/signals.py:67`; `escala/signals.py:68`; `escala/signals.py:74`; `escala/signals.py:82`; `escala/signals.py:119`; `escala/signals.py:126`; `escala/signals.py:133`)
- Havendo calendário padrão a copiar, `_copiar_folga_padrao` também lê `date.today()` no meio do Salvar. (`colaboradores/services/vinculo.py:158`; `colaboradores/services/vinculo.py:163`; `colaboradores/services/vinculo.py:282`; `colaboradores/services/vinculo.py:296`)

Esses interiores constam parcialmente do resíduo declarado; isso impede afirmar que a obra curou o ato inteiro, mas não autoriza exigir sua migração nesta fatia. (`ponto/tests/test_cadastro_futuro_c5e.py:207`; `ponto/tests/test_cadastro_futuro_c5e.py:229`; `ponto/tests/test_cadastro_futuro_c5e.py:230`; `ponto/tests/test_cadastro_futuro_c5e.py:257`; `ponto/tests/test_cadastro_futuro_c5e.py:261`)

**3. Chamadores de produção**

Não encontrei omissão nem `None` literal nos chamadores identificados abaixo. Há, porém, dias obtidos em momentos diferentes do mesmo ato, especialmente nos signals e na reabertura já apontados. O selo só verifica a presença do keyword ou de algum `**kwargs`; não verifica seu valor nem sua proveniência. (`ponto/tests/test_cadastro_futuro_c5e.py:199`; `ponto/tests/test_cadastro_futuro_c5e.py:200`; `escala/signals.py:68`; `escala/signals.py:119`; `colaboradores/services/vinculo.py:750`)

Inventário completo das chamadas identificadas aos alvos do selo:

| Destino | Chamadores → expressão passada em `hoje` |
|---|---|
| `regenerar_celulas_vinculo` | `colaboradores/management/commands/corrigir_escala_retroativa.py:231` → `hoje`; `colaboradores/services/vinculo.py:326` → `_hoje`; `colaboradores/services/vinculo.py:902`, `colaboradores/services/vinculo.py:954` → `hoje`; `escala/folgas.py:293` → `hoje`; `escala/services/cadastro_tipo.py:353` → `hoje`; `escala/services/escala_auto_executor.py:107`, `escala/services/escala_auto_executor.py:147` → `hoje`; `escala/signals.py:73`, `escala/signals.py:125` → `_hoje`; `ponto/management/commands/gerar_celulas.py:323` → `hoje`; `ponto/portas/celula.py:773` → `hoje`; `ponto/portas/regen_exportada.py:179` → `hoje`; `ponto/services/cadastro_futuro.py:44` → `hoje` |
| `regenerar_celulas_dia` | `ponto/management/commands/aplicar_col900.py:234` → `hoje`; `ponto/management/commands/diff_col900.py:72` → `_hoje_do_ato` |
| `reverter_regeneracao_dia` | `ponto/management/commands/aplicar_col900.py:186` → `hoje` |
| `gerar_celulas_periodo` | `ponto/management/commands/curar_residuo_bug115.py:138`; `ponto/management/commands/gerar_celulas.py:349` → `hoje` |
| `regenerar_futuras_do_cadastro` | `colaboradores/management/commands/corrigir_escala_retroativa.py:243` → `hoje`; `colaboradores/services/vinculo.py:336`, `colaboradores/services/vinculo.py:660`, `colaboradores/services/vinculo.py:1033` → `_hoje`; `colaboradores/services/vinculo.py:748` → `timezone.localdate()`; `colaboradores/services/vinculo.py:908`, `colaboradores/services/vinculo.py:959` → `hoje`; `escala/folgas.py:298`, `escala/folgas.py:640` → `hoje`; `escala/services/cadastro_tipo.py:376` → `hoje`; `escala/signals.py:81`, `escala/signals.py:132` → `_hoje`; `ponto/services/cadastro_futuro.py:77` → `hoje` |
| `regenerar_futuras_por_feriado` | `core/signals.py:35`, `core/signals.py:43` → `timezone.localdate()` |
| `fechar_vigencia` | `colaboradores/services/vinculo.py:212`, `colaboradores/services/vinculo.py:239` → `_hoje`; `colaboradores/services/vinculo.py:860`, `colaboradores/services/vinculo.py:1085`, `colaboradores/services/vinculo.py:1103` → `hoje`; `escala/management/commands/corrigir_vinculo_vigencia.py:107` → `hoje` |
| `fechar_vinculos_ativos` | `colaboradores/services/desligamento.py:128` → `hoje` |
| `ajustar_vinculo_pelo_sistema` | `escala/services/escala_auto_executor.py:103` → `hoje` |
| `desfazer_ajuste_do_sistema` | `escala/services/escala_auto_executor.py:145` → `hoje` |
| `executar_desligamento` | `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57` → `hoje`; `colaboradores/views.py:1437` → `timezone.localdate()` |
| `regenerar_em_exportada` | `chamados/services/passivo_validado.py:438` → `hoje` |
| `reparar` | `chamados/management/commands/materializar_passivo_validado.py:104`, `chamados/management/commands/materializar_passivo_validado.py:113` → `hoje` |
| `_trabalhar_o_par` | `chamados/services/passivo_validado.py:372` → `hoje` |

O filtro textual “fora de `tests/`” ainda inclui `tenants/tests.py`, embora seja teste; essa chamada passa `data + timedelta(days=1)`. (`ponto/tests/test_cadastro_futuro_c5e.py:184`; `tenants/tests.py:56`)

**4. Escritores, leitores e significados**

| Valor/decisão | Escritor ou produtor → leitores; avaliação |
|---|---|
| **Dia do ato** | A cabeça produz `_hoje` e os destinos imediatos o leem. É um `date`; o instante separado aparece nos carimbos de regeneração. (`colaboradores/services/vinculo.py:192`; `colaboradores/services/vinculo.py:328`; `ponto/portas/celula.py:193`; `ponto/portas/celula.py:308`) |
| **Futura versus passada** | O suplemento seleciona estritamente `d > hoje`; regeneração e reversão usam a mesma fronteira para desfazer julgamento. (`ponto/services/cadastro_futuro.py:27`; `ponto/services/cadastro_futuro.py:60`; `ponto/portas/celula.py:251`; `ponto/portas/celula.py:312`; `ponto/portas/celula.py:840`) |
| **Desfazer julgamento** | A porta escreve os campos do julgamento como `None`, emite evento e exige trilha; recusa `cel.data <= hoje`. Veredito com via `admin` é conservado. (`ponto/portas/celula.py:1146`; `ponto/portas/celula.py:1155`; `ponto/portas/celula.py:1165`; `ponto/portas/celula.py:1168`; `ponto/portas/celula.py:1172`) |
| **`rejulgar`** | O chamador fornece os dias; a porta filtra futuro usando o mesmo `_hoje`. O problema volta no rabo, que escolhe outro dia antes de chamar o juiz. (`escala/folgas.py:283`; `escala/folgas.py:296`; `ponto/portas/celula.py:334`; `ponto/portas/celula.py:338`; `ponto/portas/celula.py:464`) |
| **Janela de competência** | `janela_atual(data, empresa)` continua sendo a autoridade; regeneração usa o dia recebido para o horizonte. O suplemento usa a união das janelas das empresas ativas. Não nasceu derivação paralela de corte. (`ponto/janelas.py:60`; `ponto/portas/celula.py:197`; `ponto/management/commands/gerar_celulas.py:299`; `ponto/management/commands/gerar_celulas.py:300`) |
| **Fechamento de vigência** | `fechar_vigencia` escreve `ativa=False,data_fim`; aceita início igual ao fim, portanto o fim é inclusivo. O suplemento começa no menor fim alterado **mais um dia**. (`colaboradores/services/vinculo.py:637`; `colaboradores/services/vinculo.py:639`; `colaboradores/services/vinculo.py:659`) |
| **Fronteira do gerador** | `d < hoje` é passado para enriquecimento; `d >= hoje` segue como vigia da existente. Já a célula ausente de vínculo inativo só nasce em `d > hoje`. São políticas distintas explícitas, não evidência isolada de erro inclusivo/exclusivo. (`ponto/portas/celula.py:908`; `ponto/portas/celula.py:921`; `ponto/portas/celula.py:930`) |
| **Dia civil versus turno** | O cartório primeiro recusa dia civil futuro; para hoje, considera o fim do turno e o instante atual. `hoje` não substitui `agora`. (`ponto/services/cartorio.py:2025`; `ponto/services/cartorio.py:2033`; `ponto/services/cartorio.py:2037`; `ponto/services/cartorio.py:2040`) |
| **Teto exclusivo da apuração** | O pós-vínculo passa `hoje+1` porque `apurar_colab` corta em `hoje-1`; aqui o argumento funciona como limite exclusivo. Essa diferença de significado já existe no interior. (`colaboradores/services/pos_vinculo.py:82`; `escala/services/furos_diarios.py:27`; `escala/services/furos_diarios.py:32`) |

O fuso configurado é São Paulo; equivalência entre `date.today()` do processo e o dia local configurado em produção: **[nao verificado]**. (`config/settings/base.py:93`; `config/settings/base.py:95`; `ponto/management/commands/gerar_celulas.py:339`)

Há ainda duas classificações de “humano”: futura conserva por `veredito_via == 'admin'`; o julgamento passado exclui pelos valores de `_HUMANOS`. A equivalência dessas populações no banco não foi demonstrada. (`ponto/portas/celula.py:1155`; `ponto/portas/celula.py:465`; `colaboradores/services/pos_vinculo.py:15`) [nao verificado]

**5. Mordida dos selos**

| Classe | Mutante que pega | Mutante que deixa passar |
|---|---|---|
| `MeiaNoiteNoAtoTest` | Leitura direta adicional em `executar_vinculo`: a contagem e os dias recebidos acusam. (`ponto/tests/test_cadastro_futuro_c5e.py:68`; `ponto/tests/test_cadastro_futuro_c5e.py:80`; `ponto/tests/test_cadastro_futuro_c5e.py:82`) | Segunda `fechar_vigencia` recebendo D+1, sem inativa sobreposta na fixture. Também não executa pós-vínculo, rabo e cópia de folga. (`ponto/tests/test_cadastro_futuro_c5e.py:59`; `ponto/tests/test_cadastro_futuro_sem_banco.py:350`; `ponto/tests/test_cadastro_futuro_sem_banco.py:364`; `ponto/tests/test_cadastro_futuro_sem_banco.py:371`) |
| `FormaDaPortaTest` | Acrescentar default `hoje=None` à assinatura. (`ponto/tests/test_cadastro_futuro_c5e.py:130`; `ponto/tests/test_cadastro_futuro_c5e.py:131`) | Alterar a reabertura de `desfazer_ajuste_do_sistema`, ou deixá-la efetivar com `hoje=None`; essa função não recebe a cobrança de guarda. (`colaboradores/services/vinculo.py:1101`; `ponto/tests/test_cadastro_futuro_c5e.py:138`) |
| `ChamadorDeProducaoPassaHojeTest` | Retirar o keyword `hoje` de uma chamada reconhecida. (`ponto/tests/test_cadastro_futuro_c5e.py:200`) | Passar `hoje=None`, um dia diferente, ou apenas `**{}`. O teste aceita a presença sintática, inclusive qualquer expansão. (`ponto/tests/test_cadastro_futuro_c5e.py:199`; `

[truncado em 20000 de 25234 caracteres]

## 12/11 21:38 · sessao 1889c32d0c80 · - · - · origem=Ronald (tmux)

Auditoria somente leitura; executei com `python3 -B`, usando substituição de fonte em memória. A suíte original C5E passa 9/9; isso não cobre os contraexemplos abaixo. A estrutura dos nove testes está em `app/ponto/tests/test_cadastro_futuro_c5e.py:75`, `:86`, `:123`, `:134`, `:155`, `:179`, `:338`, `:346`, `:359`.

1. **Catraca deixa leitura nova passar.** Inserir antes de `pos_vinculo.py:56`:

   ```python
   _ler_dia = _tz.localdate
   _dia_extra = _ler_dia()
   ```

   deixa **a suíte completa 9/9 verde**, comprovado em memória. O detector só resolve aliases declarados por `import` e `from-import`; chamada via atribuição não entra em `bases`/`funcoes`. O destino é Mock na simulação de meia-noite. Referências: `app/ponto/tests/test_cadastro_futuro_c5e.py:278`, `:303`, `:310`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:350`.

2. **O segundo fechamento pode receber outro dia sem derrubar nenhum selo.** Alterar apenas `hoje=_hoje` para `hoje=_hoje + datetime.timedelta(days=1)` na segunda chamada de fechamento deixa **a suíte completa 9/9 verde**, comprovado em memória. A fixture faz fechar o vínculo ativo, sem criar vínculo inativo sobreposto. Referências: `app/colaboradores/services/vinculo.py:239`, `:245`; `app/ponto/tests/test_cadastro_futuro_c5e.py:59`.

3. **A alegação de recusa delegada de None é falsa para `desfazer_ajuste_do_sistema`.** Executei o corpo real, com ORM da fixture: `hoje=None` e `volta == novo.data_inicio` excluem o novo vínculo e reabrem o anterior, sem recusa. A função só delega ao fechamento no outro ramo. O selo não exige guarda dessa função porque a exclui de `RECUSAM_NONE`. Referências: `app/colaboradores/services/vinculo.py:1093`, `:1096`, `:1100`, `:1101`, `:1103`; `app/ponto/tests/test_cadastro_futuro_c5e.py:112`, `:138`.

Por classe, mutantes executados em memória:

| Classe | Mata | Deixa passar | Referências |
|---|---|---|---|
| `MeiaNoiteNoAtoTest` | Leitura extra `timezone.localdate()` em `executar_vinculo`: dois testes falham. | Segundo `fechar_vigencia` recebe D+1; suíte inteira permanece verde. | `c5e.py:68`, `:80`, `:82`, `:95`; `vinculo.py:245` | [nao verificado]
| `FormaDaPortaTest` | Default `hoje=None` em `regenerar_celulas_vinculo`: falha. | Reabertura em `desfazer_ajuste_do_sistema` alterada para `data_fim=volta`: três testes permanecem verdes. | `c5e.py:130`, `:131`, `:138`; `vinculo.py:1101` | [nao verificado]
| `ChamadorDeProducaoPassaHojeTest` | Remover `hoje` da chamada de desligamento da view: falha. | Passar `hoje=None` **ou `**{}`**: ambos permanecem verdes. | `c5e.py:199`, `:200`; `colaboradores/views.py:1437` |
| `CatracaDoRelogioNoInteriorTest` | Novos imports com alias e leitura direta, cobertos pelo próprio metateste. | Alias atribuído acima; também qualquer `localdate/localtime` com argumento. | `c5e.py:301`, `:303`, `:310`, `:364` |

Os caminhos abreviados da tabela são `app/ponto/tests/test_cadastro_futuro_c5e.py` e `app/colaboradores/services/vinculo.py`.

Sobre o número 42: o censo atual e a soma dos tetos deram 42. São **ocorrências sintáticas reconhecidas**, com limites por função e igualdade final, incluindo bordas e carimbos de instante; não são todas as leituras possíveis em execução. Referências: `app/ponto/tests/test_cadastro_futuro_c5e.py:224`, `:252`, `:258`, `:263`, `:327`, `:338`.

Aliases de import funcionam nos casos anunciados: `import datetime as dt; dt.datetime.now(None)` e `from django.utils.timezone import now as x; x(None)` foram contados. O ternário de `bases.add` não apresentou erro nesses casos. Referências: `app/ponto/tests/test_cadastro_futuro_c5e.py:281`, `:282`, `:285`, `:288`, `:303`, `:310`.

Há outros furos demonstrados no detector: `timezone.localdate(None)`, `localdate(value=None)`, `localdate(timezone=z)`, `time.time()`, `from time import time as x; x()`, `from django import utils as u; u.timezone.localdate()` resultaram em lista vazia. Os três primeiros dependem da semântica Django para afirmar que leem relógio em runtime — **[nao verificado]**, pois Django não está disponível e não consultei fontes externas. A omissão sintática é diretamente verificável em `app/ponto/tests/test_cadastro_futuro_c5e.py:264`, `:281`, `:285`, `:288`, `:301`, `:303`, `:310`.

Não encontrei neste subescopo evidência nova sobre ordem de trava ou pouso antes de pré-requisito; **[nao verificado]**. [nao verificado]

## 12/11 21:37 · sessao b91cfcfc16e9 · - · - · origem=Ronald (tmux)

Commands: contagem direta e destinos; o total transitivo varia com os ramos e dias processados. Os destinos abaixo demonstram leituras adicionais. (`app/ponto/portas/celula.py:464`, `app/ponto/services/cartorio.py:2016`)

| Ato | Leitura na borda | Mesmo `hoje` nas portas / outras leituras |
|---|---|---|
| `gerar_celulas` | Uma: `date.today()` (`app/ponto/management/commands/gerar_celulas.py:339`). | Mesmo valor para vigia, regeneração de hoje, geração e lavratura (`:346`, `:348`, `:349`, `:353`). |
| `aplicar_col900` | Uma leitura de dia: `localdate()` (`app/ponto/management/commands/aplicar_col900.py:146`). | Mesmo valor para reversão e regeneração (`:186`, `:238`). Carimbos separados: preparação `now()` (`:70`), conclusão `now()` (`:102`) e impressão final `localtime()` (`:253`). |
| `diff_col900` | Uma leitura de dia: `localdate()` (`app/ponto/management/commands/diff_col900.py:57`). | Closure passa esse valor (`:75`). JSON opcional lê `localtime()` (`:119`); impressão final relê (`:128`). |
| `curar_residuo_bug115` | Uma leitura de dia (`app/ponto/management/commands/curar_residuo_bug115.py:74`). | Passa às duas `_lavrar` (`:101`, `:103`) e ao gerador (`:138`), mas chama o cartório sem testemunha temporal (`:146`); cartório lê dia próprio (`app/ponto/services/cartorio.py:2016`). |
| `corrigir_vinculo_vigencia` | Uma leitura de dia (`app/escala/management/commands/corrigir_vinculo_vigencia.py:48`). | Passa a `fechar_vigencia` (`:108`). Passo 1 chama reabertura sem dia (`:86`), que lê `date.today()` e `localdate()` próprios (`app/colaboradores/services/vinculo.py:738`, `:750`). |
| `materializar_passivo_validado` | Uma: `localtime()`, da qual deriva `hoje` (`app/chamados/management/commands/materializar_passivo_validado.py:87`, `:89`). | Mesmo dia para `reparar` nos dois modos (`:106`, `:114`). Ensaio lê outro instante para localizar trilha, `_t0 = now()` (`:103`). |
| desligamentos agendados | Uma leitura inicial (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`). | Passa esse dia ao executor (`:57`), mas o juiz chamado por colaborador (`:51`) lê dia próprio (`app/colaboradores/services/desligamento.py:40`). |

Conversão ISO e resolução da janela não acrescentam leitura: `fromisoformat` interpreta entradas; `janela_atual` usa a data recebida. (`app/ponto/management/commands/gerar_celulas.py:341`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:58`, `app/ponto/janelas.py:60`)

Contraexemplo central: a regeneração recebe `hoje`, mas chama `_o_chao_do_dia_mudou` sem ele; esse destino relê o dia e decide quem rejulgar. A reversão segue o mesmo caminho. (`app/ponto/portas/celula.py:392`, `:464`, `:469`, `:858`)

O RED de meia-noite não executa esse destino nem o pós-vínculo: ambos são `Mock`; a cópia de folga padrão também foi substituída. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:350`, `:364`, `:371`)

## 12/11 21:36 · sessao b6d561b0bef0 · - · - · origem=Ronald (tmux)

Achados e inventário completo enviados ao pai. Duas provas executadas apenas em memória confirmaram aceitação de `hoje=None` com efeitos: `_trabalhar_o_par` planta antes de retornar em competência não exportada (`app/chamados/services/passivo_validado.py:427`); `desfazer_ajuste_do_sistema` apaga o novo e reabre o antigo no ramo `volta == novo.data_inicio` (`app/colaboradores/services/vinculo.py:1096`, `:1100`, `:1101`). São auxiliares incluídos em `PORTAS` pelo selo, distintos das portas literais resumidas no aval (`app/ponto/tests/test_cadastro_futuro_c5e.py:104`).

O Salvar mantém destinos com relógio próprio: cópia de folga padrão (`app/colaboradores/services/vinculo.py:163`, `:282`, `:296`), pós vínculo (`app/colaboradores/services/vinculo.py:345`; `app/colaboradores/services/pos_vinculo.py:56`) e signals da gravação (`app/colaboradores/services/vinculo.py:281`; `app/escala/signals.py:67`, `:68`, `:119`).

Censo AST: 49 chamadas reais de produção aos 14 alvos do selo, todas com `hoje` explícito; o filtro literal do selo também conta `app/tenants/tests.py:56`, elevando a 50, embora seja teste (`app/ponto/tests/test_cadastro_futuro_c5e.py:184`). Não encontrei omissão nem `None` literal nesses chamadores; os signals e a reabertura recebem dias lidos em outro momento do ato (`app/escala/signals.py:73`, `:81`, `:125`, `:132`; `app/colaboradores/services/vinculo.py:748`).

## 12/11 21:00 · sessao 9ee32d89260d · E3-FORMA-SQL · verificador · origem=Code

**REPROVA.** Há quebra demonstrável da continência de `q_vivas` e brechas nos dois sentidos do classificador VALOR/booleano. A ocorrência desses casos em produção é [não verificado]. (`app/chamados/models.py:3400`; `app/chamados/juizes.py:1438`; `app/chamados/tests/test_e3_catraca_respondedores.py:169`, `app/chamados/tests/test_e3_catraca_respondedores.py:208`)

1. **Bloqueante: a troca de `_resposta_viva` pode esconder pergunta viva.**

   Contraexemplo permitido pelo modelo: pergunta não validada, sem materialização nem via terminal, competência destrancada, resposta `'   '` com carimbo, célula com `veredito=''` e `ata={'lampadas': []}`. O campo permite veredito vazio. (`app/escala/models.py:1770`)

   O Python ignora a resposta branca, ignora o veredito vazio e retorna viva por falta de materialização. O SQL considera essa célula absolvente: exige veredito não NULL, fora dos acusatórios, sem pendência. Como espaços deixaram de satisfazer `_resposta_viva`, exclui a pergunta. Antes, ela entrava pelo ramo da resposta. Isso viola o contrato declarado de continência; existência desse estado em produção é [não verificado]. (`app/chamados/juizes.py:1407`, `app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1467`; `app/chamados/models.py:3391`, `app/chamados/models.py:3400`, `app/chamados/models.py:3405`)

2. **A enumeração dos brancos está completa contra o Python; a equivalência integral no Postgres não foi comprovada.**

   `_BRANCOS` percorre todos os pontos de código e seleciona exatamente `str.isspace()`. Avaliando essa expressão em memória, obtive 29 caracteres e nenhuma diferença entre `isspace()` e vazio por `strip()` para caracteres isolados. (`app/chamados/juizes.py:1526`)

   | Entrada | Vazio no Python / pertence a `_BRANCOS` |
   |---|---|
   | `''`, espaços, `\x85`, U+2028, U+3000 | Sim. (`app/chamados/juizes.py:1512`, `app/chamados/juizes.py:1526`) |
   | U+FEFF, U+200B | Não; contam como texto. (`app/chamados/juizes.py:1512`, `app/chamados/juizes.py:1526`) |
   | `None` | Vazio por `(t or '')`; a face sem texto inclui ramo SQL NULL. (`app/chamados/juizes.py:1512`, `app/chamados/juizes.py:1538`) |

   As regexes usam uma **lista explícita de caracteres**, sem `\s`, intervalos ou classe POSIX. A interpretação esperada é “somente membros dessa lista” e “existe algum caractere fora dela”; não encontrei contraexemplo textual nessa construção. Sua execução integral no Postgres é [não verificado]. (`app/chamados/juizes.py:1527`, `app/chamados/juizes.py:1528`)

   A expressão não fixa `COLLATE`. Encoding, collation efetiva e comportamento do servidor são [não verificado]; a afirmação de medição em `en_US.utf8` aparece como comentário, sem resultado de execução no dado fornecido. Classes POSIX/`\s` e listas literais não devem ser confundidas, mas a semântica efetiva deste servidor permanece [não verificado]. (`app/chamados/juizes.py:1524`, `app/chamados/juizes.py:1538`, `app/chamados/juizes.py:1546`)

   As faces também não respondem à mesma pergunta entre si: `q_respondida_sem_texto` corresponde ao juiz; `q_respondida_com_texto` corresponde a **carimbada com texto**, seu complemento apenas dentro das carimbadas. (`app/chamados/juizes.py:1518`, `app/chamados/juizes.py:1538`, `app/chamados/juizes.py:1542`)

3. **Censo dos leitores: a alteração alcança o app e outros consumidores; “só o degrau 1 lê resposta” é falso transitivamente.**

   | Leitor direto de `q_vivas` | Uso posterior |
   |---|---|
   | Fila de validar | Refina por `pergunta_viva`. (`app/chamados/juizes.py:1700`, `app/chamados/juizes.py:1703`) |
   | Fio administrativo | Refina as disputas fechadas por `pergunta_viva`. (`app/chamados/services/fio.py:254`, `app/chamados/services/fio.py:268`) |
   | Perguntas do app | Refina por `pergunta_viva`. (`app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`) |
   | Cobrança | Refina por espera pelo texto e `pergunta_viva`. (`app/chamados/services/cobranca.py:237`, `app/chamados/services/cobranca.py:241`) |
   | Vigia de disputa fechada | Refina por `pergunta_viva`. (`app/chamados/services/vigia_fio.py:757`, `app/chamados/services/vigia_fio.py:764`) |
   | Janela do colaborador | Usa diretamente os destinos para ampliar a janela. (`app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:69`) |
   | Raio-X | Usa o queryset e depois o veredito persistido. (`app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:120`) |
   | Fechamento administrativo | Entrega diretamente os resultados a `fechar_por_admin`. (`app/ponto/services/veredito_celula.py:181`, `app/ponto/services/veredito_celula.py:190`) |

   Os consumidores diretos de `perguntas_respondidas_a_validar` são a API de mensageria e o classificador da fila; em `models` há reexportação. (`app/api/views_mensageria.py:540`; `app/chamados/services/validacao.py:929`; `app/chamados/models.py:4161`)

   Não encontrei nesses corpos uma regra explícita exigindo “brancos = viva”. Entretanto, o filtro Python posterior não recupera uma linha já excluída pelo SQL, como no achado 1. (`app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`; `app/chamados/models.py:3405`)

   O degrau 6 também alcança a resposta: `_pergunta_viva_calc` → `_pergunta_materializada_de_fato` → `_slot_esperado`, que lê `resposta_colab` ou delega ao plantio. Essa cadeia, isoladamente, não prova quebra de continência; invalida a justificativa literal de que apenas o degrau 1 lê a resposta. (`app/chamados/juizes.py:1473`, `app/chamados/juizes.py:752`, `app/chamados/juizes.py:360`, `app/chamados/juizes.py:372`)

4. **Escritores: não identifiquei escritor novo ou divergência textual comprovada contra a nova face.**

   | Escritor persistente do par | Guarda/atribuição |
   |---|---|
   | `_nascer` | Recebe payload, verifica o par e insere. (`app/chamados/models.py:2846`, `app/chamados/models.py:2865`) |
   | `save` | Tripwire antes da persistência. (`app/chamados/models.py:3464`, `app/chamados/models.py:3468`) |
   | `carimbar` | Completa o instante somente havendo texto pelo juiz Python. (`app/chamados/models.py:3156`, `app/chamados/models.py:3158`) |
   | `descarimbar_em_lote` | Verifica o par proposto e escreve por update. (`app/chamados/models.py:3286`, `app/chamados/models.py:3306`) |
   | `responder_pergunta_disputa_core` | Aplica `strip`, recusa vazio e escreve texto/instante. (`app/chamados/services/disputa_emissao.py:1432`, `app/chamados/services/disputa_emissao.py:1467`) |
   | `responder_automaticamente` | Consulta a recusa canônica e escreve ambos. (`app/chamados/services/disputa_emissao.py:1657`, `app/chamados/services/disputa_emissao.py:1659`) |
   | `carimbar_resposta_pela_trilha` | Exige texto por `strip` e escreve o instante ausente. (`app/chamados/services/disputa_emissao.py:1677`, `app/chamados/services/disputa_emissao.py:1681`) |

   Reabertura e ressuscitação delegam a escrita ao lote; simulação e desfazer alteram somente memória. Esses papéis estão explicitados no selo de escritores. (`app/chamados/tests/test_e3_porta_A_escritor_do_par.py:42`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:46`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:47`)

   U+FEFF e U+200B podem sobreviver ao `strip` como texto; isso concorda com a enumeração nova. Divergência efetiva de escritor contra a regex do Postgres e ocorrência desses caracteres em produção são [não verificado]. (`app/chamados/services/disputa_emissao.py:1432`; `app/chamados/juizes.py:1526`)

5. **Bloqueante: a catraca tem falso VALOR e falso RESPONDEDOR.**

   Este booleano escapa como VALOR porque `guarda()` reconhece o campo, mas não exige o mesmo objeto: (`app/chamados/tests/test_e3_catraca_respondedores.py:150`, `app/chamados/tests/test_e3_catraca_respondedores.py:172`)

   ```python
   return p.respondida_em is not None and q.respondida_em.isoformat()
   ```

   Este também escapa: o agregado de outro queryset contamina o statement inteiro. (`app/chamados/tests/test_e3_catraca_respondedores.py:208`, `app/chamados/tests/test_e3_catraca_respondedores.py:237`)

   ```python
   return (
       qs.filter(respondida_em__isnull=False).exists(),
       other.aggregate(m=Max("respondida_em")),
   )
   ```

   No sentido inverso, esta serialização recebe RESPONDEDOR, pois o classificador examina somente `IfExp.body`, ignorando `orelse`. Os exemplos novos não cobrem esses contraexemplos. (`app/chamados/tests/test_e3_catraca_respondedores.py:169`, `app/chamados/tests/test_e3_catraca_respondedores.py:388`)

   ```python
   return None if p.respondida_em is None else p.respondida_em.isoformat()
   ```

   Não encontrei falso VALOR concreto entre os sítios atuais classificados assim; os três retirados são usos legítimos de instante: resposta posterior à cobrança, máximo instante para invalidar lavra e idade da resposta. (`app/chamados/services/verbos.py:342`; `app/chamados/veredito_lavrado.py:207`; `app/chamados/services/validacao.py:932`)

   | Dos quatro restantes | Pergunta efetiva |
   |---|---|
   | `_slot_esperado` | A resposta atual é posterior ao horizonte consultado? (`app/chamados/juizes.py:347`) |
   | Backfill | Há projeção carimbada cujo ato legado precisa ser preservado? (`app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`) |
   | `linhas_da_projecao_sem_linha` | Falta linha de mesmo ato e instante, inclusive quando o texto legado é vazio? (`app/chamados/models.py:3993`, `app/chamados/models.py:4009`) |
   | Cura de limbo | O carimbo continua ausente antes da limpeza pela porta? (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`) |

   Portanto, migrar esses quatro mecanicamente para “com texto” perderia a distinção entre validade da resposta, horizonte temporal e preservação da trilha. (`app/chamados/juizes.py:349`; `app/chamados/models.py:3995`)

6. **Selos: têm controles contra vacuidade, mas não cobrem os bloqueios encontrados.**

   A equivalência compara os mesmos PKs em três composições; o universo tem 20 linhas e exige ambos os resultados do juiz. O mutante sem trim exige divergência num conjunto não vazio; o mutante `\s` exige NBSP e U+001F. Execução SQL e resultado RED/GREEN são [não verificado]. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:45`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:60`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:74`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:93`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:102`)

   A amostra fixa não exercita todos os brancos nem U+0085/U+2028/U+3000/U+FEFF. NULL SQL também não é exercitado: o campo é NOT NULL e o teste de `None` chama apenas Python. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:28`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:89`; `app/chamados/models.py:2468`)

   A fixture está visível, recarrega o passivo e exige controle positivo na fila. O `setUp` é próprio; não identifiquei o empréstimo descrito como causador do vazamento anterior. Ausência de vazamento na suíte é [não verificado]. O caso `veredito=''` não está coberto. (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:118`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:120`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:152`; `app/chamados/tests/test_viva_delega_ao_juiz.py:58`, `app/chamados/tests/test_viva_delega_ao_juiz.py:81`)

7. **Classes anteriores e pré-requisito: persistem dois significados; item (3) não é o único problema de pouso.** [nao verificado]

   Os corpos novos constroem filtros: não acrescentam escritor, relógio ou aquisição explícita de trava. A enumeração vem do próprio Python; não identifiquei segundo juiz independente nesses corpos. (`app/chamados/juizes.py:1526`, `app/chamados/juizes.py:1531`, `app/chamados/juizes.py:1541`; `app/chamados/models.py:3392`)

   O ESMERIL reprova a uniformidade atual: `respondida`, `estado` e a separação de pendências do app usam apenas carimbo; `espera_o_texto_do_colaborador` considera carimbo sem texto como espera. O passivo inalterado continua salvável por desenho. (`app/chamados/models.py:3576`, `app/chamados/models.py:3071`, `app/chamados/models.py:3498`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/services/fio.py:187`)

   Não há no aval fornecido uma dependência explícita tornando toda entrega de (1)(2) subordinada à execução dos 17 do item (3) [não verificado em arquivo local]. Porém os nove leitores SQL foram pedidos no próprio item (1), e permanecem booleanos crus, por exemplo nos contadores de pendência; a catraca conserva 27 sítios como migração futura. Isso não demonstra cumprimento integral de (1)(2), sob a regra de escopo literal. (`app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/disputa_emissao.py:1720`; `app/chamados/tests/test_e3_catraca_respondedores.py:23`, `app/chamados/tests/test_e3_catraca_respondedores.py:37`; `CLAUDE.md:37`)

**Veredito final: REPROVA**, pela pergunta viva excluída de `q_vivas` e pela catraca que aceita booleanos como VALOR e rejeita usos legítimos de instante. (`app/chamados/models.py:3400`; `app/chamados/juizes.py:1438`; `app/chamados/tests/test_e3_catraca_respondedores.py:169`, `app/chamados/tests/test_e3_catraca_respondedores.py:208`)

## 12/11 20:58 · sessao 365b9372c0b5 · - · - · origem=Ronald (tmux)

Achado bloqueante da catraca: `guarda()` compara apenas o nome do campo, sem comprovar que é o mesmo objeto; `valor_no_orm()` procura agregado/ordenação em todo o statement, sem comprovar ligação com o filtro booleano (`app/chamados/tests/test_e3_catraca_respondedores.py:150`, `:158`, `:165`, `:194`, `:208`). Contraexemplos executados em memória:

```python
return p.respondida_em is not None and q.respondida_em.isoformat()
```

```python
return (
    qs.filter(respondida_em__isnull=False).exists(),
    other.aggregate(m=Max("respondida_em")),
)
```

Ambos recebem `VALOR` e escapam da contagem; isso decorre diretamente das regras citadas (`app/chamados/tests/test_e3_catraca_respondedores.py:172`, `:209`, `:237`). Também escapa a união `qs.filter(respondida_em__isnull=False) | qs.filter(respondida_em__gt=d)`, embora o primeiro ramo continue perguntando apenas pelo carimbo (`app/chamados/tests/test_e3_catraca_respondedores.py:212`).

Há erro inverso: `return None if p.respondida_em is None else p.respondida_em.isoformat()` recebe `RESPONDEDOR`, porque a regra examina somente o `body`, nunca o `orelse`; guarda equivalente num bloco `if` também é contada (`app/chamados/tests/test_e3_catraca_respondedores.py:169`, `:178`, `:188`). Os mutantes novos cobrem pares favoráveis à implementação e não esses contraexemplos (`app/chamados/tests/test_e3_catraca_respondedores.py:388`).

Os três removidos são usos legítimos de instante:

- `placar_aceite`: resposta posterior à cobrança e data da resposta (`app/chamados/services/verbos.py:325`, `:342`, `:345`).
- `marcos_do_lote`: máximo instante de resposta como insumo de invalidação da lavra (`app/chamados/veredito_lavrado.py:197`, `:207`, `:208`).
- `fila_validar_classificada`: idade em dias da resposta (`app/chamados/services/validacao.py:929`, `:932`, `:935`).

O classificador produziu oito ocorrências `VALOR`; as demais são serialização ou fotografia para trilha, sem falso VALOR concreto encontrado: `app/api/views.py:1161`, `:2559`; `app/chamados/services/disputa_emissao.py:2059`, `:2076`; `app/chamados/services/materializacao.py:675`.

Os quatro restantes perguntam coisas diferentes:

- `_slot_esperado`: a resposta existe hoje, mas é posterior ao horizonte consultado; nesse caso não fornece slot (`app/chamados/juizes.py:344`, `:347`, `:348`).
- `linhas_a_criar`: pré-seleção das projeções com carimbo para reconstruir atos legados, delegada a `linhas_da_projecao_sem_linha` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:47`, `:56`, `:67`).
- `linhas_da_projecao_sem_linha`: preservar resposta/validação cuja linha de mesmo ato e instante não existe; preserva inclusive texto vazio, por ser trilha (`app/chamados/models.py:3993`, `:4002`, `:4009`, `:4011`).
- `curar_perguntas_em_limbo`: verificar que o carimbo continua nulo antes de limpar a projeção pela porta; universo e fotografia obrigatória aparecem antes (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `:47`, `:69`).

Executadas as oito catracas por extração AST em memória, sem importação da aplicação ou banco: passaram, com 27 sítios contados; isso confirma a insuficiência dos exemplos existentes diante dos contraexemplos acima (`app/chamados/tests/test_e3_catraca_respondedores.py:305`, `:351`, `:388`).

## 12/11 20:58 · sessao 1ac4d4ffa056 · - · - · origem=Ronald (tmux)

1. **Selos têm universo e controles positivos; não há vacuidade evidente no código.** São criadas 20 perguntas pelo produto de `TEXTOS × CARIMBOS`, com o passivo inserido por `update`; há verificação explícita de cardinalidade e dos dois resultados do juiz. A comparação verifica os mesmos PKs em `filter`, `exclude` e `filter(~face)`. — `app/chamados/tests/test_e3_forma_sql_do_juiz.py:28`, `:45`, `:53`, `:60`, `:74`.

2. **Mutante sem trim tem mordida estrutural demonstrável, execução [não verificado].** O teste exige divergência exatamente no conjunto não vazio de textos brancos carimbados; não passa com divergência vazia. O mutante `\s` exige divergência em NBSP e U+001F; que isso aconteça no Postgres/collation empregado é [não verificado]. — `app/chamados/tests/test_e3_forma_sql_do_juiz.py:93`, `:98`, `:102`, `:108`.

3. **A equivalência não foi selada para todo Unicode nem NULL SQL.** A amostra fixa contém dez strings, sem U+0085, U+2028, U+3000 ou U+FEFF. O campo não admite NULL; o teste de `None` chama somente o juiz Python. A classe completa é calculada no código, mas sua interpretação integral pelo Postgres não está demonstrada pelos testes disponíveis. — `app/chamados/tests/test_e3_forma_sql_do_juiz.py:28`, `:89`; `app/chamados/models.py:2468`; `app/chamados/juizes.py:1526`.

4. **Fixture dos selos de integração está visível e verifica resultados reais.** A classe reutiliza apenas `_pergunta`, tem `setUp` próprio e cria o passivo por `update` seguido de `refresh_from_db`. `_confere` recarrega a pergunta e exige que o juiz produza o resultado esperado antes de conferir a propriedade/continência. A fila tem controle positivo obrigatório, impedindo aprovação por fila vazia. — `app/chamados/tests/test_e3_forma_sql_do_juiz.py:118`, `:120`, `:152`; `app/chamados/tests/test_viva_delega_ao_juiz.py:34`, `:42`, `:73`, `:81`.

5. **Não encontrei o empréstimo de `setUp` responsável pelo vazamento descrito; vazamento em execução permanece [não verificado].** O comentário identifica esse defeito anterior e a implementação atual define `setUp` próprio, mantendo somente o empréstimo de `_pergunta`. — `app/chamados/tests/test_e3_forma_sql_do_juiz.py:116`, `:118`, `:120`.

6. **Separar pendência do item (3) da incompletude do item (1).** O aval fornecido pede explicitamente que os nove sítios SQL leiam a face no item (1); o diff não os migra. A cópia declara as migrações restantes como obra própria e conserva `PAROU_EM = 27`. O item (3), pelo texto fornecido, exige sombra antes das migrações que movem passivo; uma dependência explícita proibindo pousar qualquer parte de (1)(2) antes dele é [não verificado]. A obrigação de cumprir literalmente o aval e incluir leitores/escritores no escopo está registrada. — `app/chamados/tests/test_e3_catraca_respondedores.py:23`, `:37`; `CLAUDE.md:37`, `:379`; aval fornecido [não verificado em arquivo local].

7. **Classes estruturais no trecho novo:** as faces constroem `Q`, e o consumidor novo filtra e ordena; esses corpos não introduzem escrita, leitura de relógio ou aquisição explícita de trava. A fonte da classe de brancos é `str.isspace`, compartilhada pelos dois padrões, enquanto o juiz mantém `strip`; a equivalência efetiva da tradução SQL depende da questão Unicode/Postgres do item 1. — `app/chamados/juizes.py:1510`, `:1526`, `:1531`, `:1541`, `:1698`; `app/chamados/models.py:3392`.

**Para o veredito consolidado:** estes selos sustentam os casos presentes e evitam universo/fila vazios; não certificam execução, Unicode completo, NULL SQL ou ausência de vazamento em suite. — `app/chamados/tests/test_e3_forma_sql_do_juiz.py:28`, `:74`, `:89`, `:152`; execução [não verificado].

## 12/11 20:58 · sessao 0f7b5833836e · - · - · origem=Ronald (tmux)

Achado principal: há contraprova estática à continência no delta. Considere pergunta não validada, sem via terminal/identidade, sem materialização, carimbada com resposta `'   '`, ligada a célula de `veredito=''` e `ata={'lampadas': []}`, sem competência trancada. Esse veredito é permitido pelo campo (`app/escala/models.py:1770`). Python ignora a resposta branca (`app/chamados/juizes.py:1407`), ignora o veredito vazio (`app/chamados/juizes.py:1438`) e retorna viva quando não materializada (`app/chamados/juizes.py:1467`). SQL considera essa célula absolvente porque exige apenas veredito não NULL e fora de `VEREDITOS_ACUSAM` (`app/chamados/models.py:3400`), e agora exclui a pergunta porque a resposta branca deixou de satisfazer `_resposta_viva` (`app/chamados/models.py:3393`, `app/chamados/models.py:3405`). A forma anterior incluía esse caso pelo ramo da resposta; existência em produção [não verificado].

Leitores diretos de `q_vivas`:

- Fila de validar, seguida de `pergunta_viva`: `app/chamados/juizes.py:1700`, `app/chamados/juizes.py:1703`.
- Fio administrativo de disputa fechada, seguido de `pergunta_viva`: `app/chamados/services/fio.py:254`, `app/chamados/services/fio.py:268`.
- Perguntas do app, seguido de `pergunta_viva`: `app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`.
- Cobrança, seguida de espera pelo texto e `pergunta_viva`: `app/chamados/services/cobranca.py:237`, `app/chamados/services/cobranca.py:241`.
- Vigia de disputa fechada, seguido de `pergunta_viva`: `app/chamados/services/vigia_fio.py:757`, `app/chamados/services/vigia_fio.py:764`.
- Janela visível do colaborador: usa diretamente os IDs resultantes, ampliando a janela; não refina com juiz Python (`app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:69`).
- Raio-X: usa diretamente o queryset para crédito e depois filtra pelo veredito persistido; não chama `pergunta_viva` nesse ponto (`app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:120`).
- Fechamento pelo admin: carimba diretamente cada resultado, sem refinamento por `pergunta_viva` (`app/ponto/services/veredito_celula.py:181`, `app/ponto/services/veredito_celula.py:190`).

Não encontrei nesses corpos regra explícita “brancos significam viva”; os efeitos reais sobre seus universos em produção são [não verificado]. A contraprova acima faz os consumidores perderem uma pergunta que o juiz pode considerar viva (`app/chamados/juizes.py:1467`, `app/chamados/models.py:3405`).

Leitores da fila `perguntas_respondidas_a_validar`: API de mensageria (`app/api/views_mensageria.py:540`) e classificação por lote (`app/chamados/services/validacao.py:929`); a exportação em models é somente reexportação (`app/chamados/models.py:4161`). A API usa o carimbo como instante para serialização e idade (`app/api/views_mensageria.py:569`); o classificador calcula idade, com guarda de nulo (`app/chamados/services/validacao.py:932`).

A frase “só o degrau 1 lê resposta” é falsa transitivamente: degrau 6 chama `_pergunta_materializada_de_fato` (`app/chamados/juizes.py:1473`), este chama `_slot_esperado` (`app/chamados/juizes.py:752`), que lê `resposta_colab` para `data_hora` (`app/chamados/juizes.py:360`) ou delega ao plantio (`app/chamados/juizes.py:372`), que também lê o texto (`app/chamados/services/materializacao.py:359`). Só o teste explícito de vazio para promover a viva está no degrau 1 (`app/chamados/juizes.py:1407`); a cadeia posterior não prova, sozinha, quebra da continência.

Escritores do par:

- Nascimento: recebe o payload, recusa carimbo sem texto antes da trava e grava pelo corpo `_nascer` (`app/chamados/models.py:2846`, `app/chamados/models.py:2865`).
- `save` genérico: consulta o tripwire antes de persistir; preserva passivo inalterado (`app/chamados/models.py:3464`, `app/chamados/models.py:3498`, `app/chamados/models.py:3501`).
- `carimbar`: só acrescenta `respondida_em` quando há texto pelo juiz Python (`app/chamados/models.py:3156`, `app/chamados/models.py:3158`).
- `descarimbar_em_lote`: verifica o par proposto, depois escreve via update (`app/chamados/models.py:3286`, `app/chamados/models.py:3306`).
- Resposta humana: aplica `.strip()`, recusa vazio, atribui texto e instante (`app/chamados/services/disputa_emissao.py:1432`, `app/chamados/services/disputa_emissao.py:1467`).
- Resposta automática: consulta `recusa_respondida_sem_texto` e escreve ambos (`app/chamados/services/disputa_emissao.py:1657`, `app/chamados/services/disputa_emissao.py:1659`).
- Carimbo pela trilha: exige texto pelo `.strip()` e carimbo ausente; escreve o instante (`app/chamados/services/disputa_emissao.py:1677`, `app/chamados/services/disputa_emissao.py:1681`).

Reabertura delega par vazio ao lote (`app/chamados/services/disputa_emissao.py:1595`, `app/chamados/services/disputa_emissao.py:1634`); ressuscitação delega limpeza de `respondida_em` (`app/chamados/models.py:1567`). As atribuições da simulação de ressuscitação e do desfazer da recusa são em memória (`app/chamados/models.py:1509`, `app/chamados/models.py:3189`). O selo declara e varre esses sete escritores, duas delegações e dois casos em memória (`app/chamados/tests/test_e3_porta_A_escritor_do_par.py:42`).

Não identifiquei escritor novo deste diff; os escritores examinados usam o vazio Python ou sua porta, portanto diferença deles contra a face SQL depende da semântica efetiva da regex no Postgres [não verificado]. Os caracteres FEFF e U+200B passam como texto para esses escritores quando a modalidade aceita texto livre, pois a resposta humana só aplica `.strip()` e recusa o resultado vazio (`app/chamados/services/disputa_emissao.py:1432`); sua ocorrência em produção [não verificado].

ESMERIL: o significado ainda não é uniforme entre leitores: `respondida` equivale somente a carimbo (`app/chamados/models.py:3576`), `estado` também traduz carimbo em RESPONDIDA (`app/chamados/models.py:3071`), e o app separa pendência por carimbo (`app/chamados/services/pergunta_no_app.py:71`); já `espera_o_texto_do_colaborador` inclui o carimbo sem texto como espera (`app/chamados/services/fio.py:187`). O passivo com par inalterado continua salvável por desenho (`app/chamados/models.py:3498`), portanto a divergência dos leitores não se resolve só pela porta de entrada.

Meu veredito no recorte de continência: **REPROVA**, pela contraprova `veredito=''` acima (`app/chamados/juizes.py:1438`, `app/chamados/models.py:3400`).

## 12/11 20:48 · sessao 0d5c462a6a28 · TIPO-NA-FONTE · sonda · origem=Code

Reproduzo a saída fornecida, sem nova execução. [não verificado] O envelope escreve arquivos, contrariando a restrição de somente leitura. [bin/sonda_leitura.sh:132](bin/sonda_leitura.sh:132)

```text
sonda_leitura: sonda_nao_lavradas.py [8b136f1f57fab7fb] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

campos=id,colaborador,data,trabalha,origem,escala_geradora,dna,gerada_em,regenerada_em,regeneracoes,dna_anterior,editada_em,editada_por,julgada_em,impressao,veredito,veredito_via,veredito_em,protestos,ata,ausencia,tem_entrada,tem_saida,insumos_em,status_dia,status_rotulo
ata_lavrada_hoje                  274  amostra=[138954, 139016, 139512, 139698, 139915, 140008, 140163, 140535, 140628, 140969]
ata_lavrada_passado            134799  amostra=[137148, 123129, 111993, 109909, 22590, 104313, 138874, 22594, 143296, 28678]
ata_nula_futuro                  8721  amostra=[138866, 138867, 138868, 138869, 138893, 138894, 138895, 138896, 138897, 138898]
ata_nula_hoje                     333  amostra=[138892, 138923, 138985, 139078, 139171, 139202, 139233, 139264, 139326, 139357]
ata_nula_passado                   75  amostra=[121488, 121494, 124325, 124326, 123980, 123981, 123991, 123982, 123983, 123984]
total                          144202  amostra=[]
total_erros=0
```

Conferência do dado fornecido: 274 + 134799 + 8721 + 333 + 75 = 144202; `total_erros=0`. [não verificado]

## 12/11 20:45 · sessao f2b03b2454de · TIPO-NA-FONTE · verificador · origem=Code

1. **BLOQUEANTE — segundo juiz / derivação paralela: uma batida sai ACESA e ÓRFÃ.** A pergunta “que marco previsto a batida ocupa?” pertence a `_match_marcos`. O cartório volta a respondê-la: incorpora órfãs em `_reais`, escolhe uma por proximidade, acende a lâmpada e depois grava a lista original de órfãs. (`app/core/juizes.py:212`; `app/ponto/services/cartorio.py:629`; `app/ponto/services/cartorio.py:664`; `app/ponto/services/cartorio.py:670`; `app/ponto/services/cartorio.py:770`)

   O próprio teste novo fornece o contraexemplo: DNA 23:00→00:30, régua contendo somente E23:00 e órfã em D+1 00:30. A saída acende com esse instante, que permanece em `orfas`, com `n_orfas=1`. O teste verifica apenas a lâmpada acesa, deixando passar a contradição. (`app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:92`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:95`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:98`; `app/ponto/services/cartorio.py:672`; `app/escala/services/ata.py:233`)

   A aproximação já aparece no trecho anterior do diff fornecido; sua comparação histórica literal com `0a747193e` é [nao verificado]. A contradição permanece no ramo reescrito e exercitado pela obra. (`app/ponto/services/cartorio.py:661`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:91`)

2. **BLOQUEANTE — célula não lavrada passa a disparar corrupção, inclusive em produção sem ci.** `None` levanta “ata não é mapping”; `{}` e somente `precedencia` levantam “chave obrigatória ausente”. Isso ocorre independentemente de `AtaVigiada`: o acessor estrito também atua sobre dict comum. (`app/escala/services/ata.py:42`; `app/escala/services/ata.py:45`; `app/escala/services/ata.py:47`)

   A fronteira reconhece ata ausente como estado legítimo para transportar precedência; os leitores abaixo não distinguem esse estado de uma ata lavrada corrompida. (`app/escala/services/ata.py:83`; `app/escala/services/ata.py:87`; `app/ponto/portas/celula.py:1086`)

   | Chamador de produção | Entrada e efeito |
   |---|---|
   | `grade_da_celula` → `orfas_da_ata` e contagem | Célula inexistente é tratada antes; célula existente dentro da vida funcional chega aos acessores mesmo com `None/{}/precedencia`, levantando. (`app/escala/services/leitor_celula.py:394`; `app/escala/services/leitor_celula.py:396`; `app/escala/services/leitor_celula.py:422`) |
   | `periodos_do_dia` → `orfas_da_ata` | Chamada incondicional, antes das verificações de marcos e lâmpadas; as três formas não lavradas levantam. (`app/ponto/juiz_batida.py:158`; `app/ponto/juiz_batida.py:163`; `app/ponto/juiz_batida.py:176`) |
   | `_leitura_da_celula`, cluster | Dia sem trabalho retorna antes; célula de trabalho não lavrada chega ao acessor e levanta, antes do retorno “não medido”. (`app/ponto/services/cluster_espurio.py:25`; `app/ponto/services/cluster_espurio.py:30`; `app/ponto/services/cluster_espurio.py:31`) |
   | `assinaturas_do_colab`, esmeril | Carrega todas as células do período, sem filtro de lavra; chega ao acessor salvo saída antecipada por `so_escala`. (`app/ponto/services/esmeril_espelho.py:96`; `app/ponto/services/esmeril_espelho.py:102`; `app/ponto/services/esmeril_espelho.py:126`) |
   | `censo`, vigia pareador | Carrega todas as atas da janela ampliada; transforma `None` em `{}` e chama o acessor, levantando. (`app/ponto/services/vigia_pareador_celula.py:71`; `app/ponto/services/vigia_pareador_celula.py:75`; `app/ponto/services/vigia_pareador_celula.py:76`) |
   | `medir_sobra_com_tipo_trocado` | Consulta células de trabalho; com dois marcos, ata não lavrada chega ao acessor e levanta. (`app/ponto/management/commands/tripwire_celulas.py:228`; `app/ponto/management/commands/tripwire_celulas.py:233`; `app/ponto/management/commands/tripwire_celulas.py:235`) |
   | `_celula_furada` → contagem | `None/{}/precedencia` sem `lampadas` retornam `None` antes do acessor. Com lâmpadas, chaves faltantes podem chegar à validação; a exceção vira `None` no `except` amplo. (`app/chamados/juizes.py:493`; `app/chamados/juizes.py:505`; `app/chamados/juizes.py:508`; `app/chamados/juizes.py:511`; `app/chamados/juizes.py:521`) |
   | Plantio → `horas_orfas` | Sem lâmpadas, as formas não lavradas são puladas. Com lâmpadas e chaves ausentes, a exceção é capturada e a resposta vira `False`. (`app/chamados/services/materializacao.py:172`; `app/chamados/services/materializacao.py:175`; `app/chamados/services/materializacao.py:182`; `app/chamados/services/materializacao.py:186`) |
   | `_ata_ate_o_teto` → `copiar_ata` | `None` ou ausência de pendências retorna antes. Mapping vazio/somente precedência, com pendências, seria validado e rejeitado. Os dois chamadores ordinários constroem a ata com `ata_do_dia`. (`app/ponto/services/cartorio.py:918`; `app/ponto/services/cartorio.py:927`; `app/ponto/services/cartorio.py:1444`; `app/ponto/services/cartorio.py:1859`) |
   | `insumos_da_ata` → `contar_orfas` | Valida incondicionalmente, inclusive quando existem lâmpadas. Cartório fornece atas construídas; backfill e vigia filtram `julgada_em`. (`app/ponto/services/cartorio.py:841`; `app/ponto/services/cartorio.py:1880`; `app/ponto/management/commands/backfill_b5_insumos.py:61`; `app/ponto/management/commands/backfill_b5_insumos.py:73`; `app/ponto/management/commands/vigia_b5_ausencia.py:55`; `app/ponto/management/commands/vigia_b5_ausencia.py:79`) |

   No ramo `nunca_bateu`, `_ata_nb` pode ser construído como `None` quando falta `_dia_nb`; existe chamada posterior a `insumos_da_ata`. A ocorrência real dessa ausência de grade não está demonstrada pela sonda: [nao verificado]. (`app/ponto/services/cartorio.py:1247`; `app/ponto/services/cartorio.py:1261`; `app/ponto/services/cartorio.py:1321`)

   **Exceções engolidas também alteram o resultado:** turnos exclui a célula da alimentação dos papéis; motor registra e ignora o dia; espelho captura a falha da grade e segue com o mapa vazio; calendário captura a falha; inteligência exclui o colaborador da métrica; materialização devolve régua vazia. (`app/ponto/turnos.py:1633`; `app/ponto/turnos.py:1634`; `app/ponto/motor_calculo_v2.py:616`; `app/ponto/motor_calculo_v2.py:621`; `app/ponto/services/espelho.py:569`; `app/ponto/services/espelho.py:571`; `app/colaboradores/services/calendario.py:399`; `app/inteligencia/calculadores.py:43`; `app/inteligencia/calculadores.py:46`; `app/chamados/services/materializacao.py:237`; `app/chamados/services/materializacao.py:241`)

   Há ainda chamadas sem captura local em cadastro da realidade e na avaliação da pausa para exportação. (`app/escala/services/cadastro_realidade.py:261`; `app/escala/services/cadastro_realidade.py:266`; `app/folha/export.py:1150`)

   Portanto, `c=0` nas **lavradas** não cobre o universo das não lavradas informado no prompt. Os números da sonda não possuem arquivo correspondente aberto nesta cópia: [nao verificado em arquivo].

3. **BLOQUEANTE — ainda existem transportes crus de produção; o resíduo da precedência foi corrigido.** Permanecem `dict(ata)` em `celula_como_o_julgamento_deixa` e `lavrar_veredito`. Recebendo `AtaVigiada` com as chaves migradas, essas cópias atravessam a iteração fiscalizada e levantam. (`app/ponto/portas/celula.py:945`; `app/ponto/portas/celula.py:995`; `app/escala/services/ata.py:192`; `app/escala/services/ata.py:195`)

   O script operacional `snapshot_o118_col221.py` também lê `ata.orfas` cru; sua execução atual em produção é [nao verificado]. (`bin/snapshot_o118_col221.py:18`; `bin/snapshot_o118_col221.py:32`)

   Já `carimbar_precedencia` usa agora `copiar_envelope`; o resíduo citado pelo relato em torno de `:1083` não corresponde mais ao código atual. Esse transporte aceita `None`, conserva a catraca e não fabrica `n_orfas=0`. (`app/ponto/portas/celula.py:1085`; `app/ponto/portas/celula.py:1086`; `app/escala/services/ata.py:87`; `app/escala/services/ata.py:90`)

4. **Censo de escritores e leitores das portas, incluindo sítios fora do diff.** Os escritores de formato JSON encontrados são as duas atribuições em `gravar_orfas`; cartório e grade sintética chamam esse escritor. A persistência do envelope continua na porta da célula. (`app/escala/services/ata.py:233`; `app/escala/services/ata.py:234`; `app/ponto/services/cartorio.py:770`; `app/escala/utils.py:1903`; `app/ponto/portas/celula.py:1006`; `app/ponto/portas/celula.py:1033`)

   | Campo/valor | Origem e leitores encontrados |
   |---|---|
   | `ata.orfas`, `ata.n_orfas` | Escritor: `gravar_orfas`; leitor semântico central: `_valores_orfas`. Cópias/leitor cru: achado 3. (`app/escala/services/ata.py:233`; `app/escala/services/ata.py:234`; `app/escala/services/ata.py:45`) |
   | `ata.precedencia` | Conteúdo vem de `fatos_do_dia`, preparado pelo cartório; atribuição em `carimbar_precedencia`; comparação para idempotência e preservação dinâmica na lavra. (`app/ponto/services/cartorio.py:2007`; `app/ponto/services/cartorio.py:2008`; `app/ponto/portas/celula.py:1087`; `app/ponto/portas/celula.py:1081`; `app/ponto/portas/celula.py:104`; `app/ponto/portas/celula.py:997`) |
   | Retorno de `leitura_orfas` | Produzido nas saídas legado/ISO; lido por `ler_orfas`, `contar_orfas`, `copiar_ata` e `horas_orfas`. `medida` tem default `True`. (`app/escala/services/ata.py:37`; `app/escala/services/ata.py:58`; `app/escala/services/ata.py:70`; `app/escala/services/ata.py:74`; `app/escala/services/ata.py:78`; `app/escala/services/ata.py:101`; `app/escala/services/ata.py:129`) |
   | Retorno de `ler_orfas` | Consumido por `orfas_da_ata`. (`app/escala/services/ata.py:74`; `app/escala/services/leitor_celula.py:186`) |
   | Retorno de `contar_orfas` | Consumido por `contagem_orfas_da_ata` e `insumos_da_ata`. (`app/escala/services/ata.py:78`; `app/escala/services/leitor_celula.py:192`; `app/ponto/services/cartorio.py:841`) |
   | Retorno de `orfas_da_ata` | Grade, juiz da batida, tripwire, esmeril, vigia pareador e cluster. (`app/escala/services/leitor_celula.py:422`; `app/ponto/juiz_batida.py:163`; `app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/vigia_pareador_celula.py:76`; `app/ponto/services/cluster_espurio.py:30`) |
   | Retorno de `contagem_orfas_da_ata` | Grade e `_celula_furada`. (`app/escala/services/leitor_celula.py:422`; `app/chamados/juizes.py:511`) |
   | Retorno de `contar_orfas_da_grade` | Furos, causas, status, supra-juiz, bordas, score de par relâmpago e cartório. O nome `contar_orfas` no supra é alias desta porta da grade. (`app/escala/services/furos_diarios.py:23`; `app/folha/causas.py:44`; `app/ponto/status_dia.py:189`; `app/ponto/supra_juiz.py:379`; `app/ponto/supra_juiz.py:380`; `app/ponto/services/bordas_realizado.py:50`; `app/ponto/management/commands/detectar_par_relampago.py:141`; `app/ponto/services/cartorio.py:820`) |
   | Retorno de `horas_orfas` | Legado copiado como HH:MM; ISO convertido ao fuso local; consumidor de produção: plantio. (`app/escala/services/ata.py:131`; `app/escala/services/ata.py:133`; `app/chamados/services/materializacao.py:182`) |
   | Retorno de `copiar_ata` | `_ata_ate_o_teto`; delega o transporte a `copiar_envelope` depois de validar. (`app/ponto/services/cartorio.py:927`; `app/escala/services/ata.py:101`; `app/escala/services/ata.py:102`) |
   | Retorno de `copiar_envelope` | `copiar_ata` e `carimbar_precedencia`. (`app/escala/services/ata.py:102`; `app/ponto/portas/celula.py:1086`) |
   | `AtaVigiada` | Produzida pelo descriptor e pelas cópias; reconhecida pelo preparo JSON; leitura/escrita fiscalizadas pelos métodos próprios. (`app/escala/services/ata_ci.py:22`; `app/escala/services/ata_ci.py:28`; `app/escala/services/ata.py:90`; `app/escala/services/ata.py:213`; `app/escala/services/ata_ci.py:43`; `app/escala/services/ata_ci.py:44`; `app/escala/services/ata.py:180`; `app/escala/services/ata.py:184`) |

   A projeção `dia.orfas` tem escritores próprios nos builders, overlays e leitor da ata; não é o JSON persistido. Ela é consumida pelo calendário, propositor, cobertura parcial, adaptador de realizado e PDF. (`app/escala/utils.py:622`; `app/escala/utils.py:663`; `app/escala/utils.py:800`; `app/escala/utils.py:964`; `app/escala/utils.py:1157`; `app/escala/utils.py:1816`; `app/escala/utils.py:1839`; `app/escala/services/leitor_celula.py:422`; `app/colaboradores/services/calendario.py:380`; `app/ponto/propositor.py:36`; `app/ponto/propositor.py:81`; `app/ponto/turnos.py:2667`; `app/escala/utils.py:1114`; `app/relatorios/pdf_espelho.py:203`)

   Também existem escritores/leitores diretos em fixtures e testes, inclusive fora dos dirigidos: calendário, juiz da batida, T4.1, O118 e lâmpadas. Eles não desapareceram com o acessor. (`app/colaboradores/tests/test_calendario_orfa.py:79`; `app/ponto/tests/test_juiz_batida.py:188`; `app/escala/tests/test_t41_leitor_celula.py:88`; `app/ponto/tests/test_o118_orfa_que_acendeu.py:90`; `app/ponto/tests/test_lampada_acende_pela_batida.py:84`)

   Os consumidores adicionais das portas nos testes incluem as cinco assertivas migradas e bateria, provas da matriz, fluxo, cópia e ativação. (`app/escala/tests/test_ata_x_turno_entrada.py:144`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:104`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:137`; `app/ponto/tests/test_bateria_lavratura.py:639`; `app/ponto/tests/test_bateria_lavratura.py:1060`; `app/ponto/tests/test_bateria_lavratura.py:544`; `app/ponto/tests/test_bateria_lavratura.py:963`; `app/escala/tests/test_tipo_na_fonte_orfas.py:34`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:74`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:45`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:46`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:127`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:131`; `app/escala/tests/test_ata_ci.py:64`)

   Completude de aliases dinâmicos, acesso indireto por serialização e todas as fixtures do archive: [nao verificado].

5. **Mudanças efetivas para o legado HH:MM, mesmo com contagem coerente.** HH:MM não vazio produz `instantes=None`, preservando quantidade; vazio explícito produz `[]` e zero. Não há exceção nessas duas formas válidas. (`app/escala/services/ata.py:56`; `app/escala/services/ata.py:58`; `app/escala/services/ata.py:70`)

   | Consumidor | Resultado para HH:MM não vazio |
   |---|---|
   | Cluster | Retorna não medido, sem procurar rajada legada. (`app/ponto/services/cluster_espurio.py:30`; `app/ponto/services/cluster_espurio.py:31`) |
   | Esmeril A5 | Não acrescenta essas órfãs aos buckets. (`app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/esmeril_espelho.py:127`; `app/ponto/services/esmeril_espelho.py:131`) |
   | Tripwire | Pula a célula antes de incrementar o universo. (`app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/management/commands/tripwire_celulas.py:236`; `app/ponto/management/commands/tripwire_celulas.py:238`) |
   | Vigia pareador | Não conta a célula como tendo órfã nem inclui o colaborador por essa informação. (`app/ponto/services/vigia_pareador_celula.py:77`; `app/ponto/services/vigia_pareador_celula.py:79`; `app/ponto/services/vigia_pareador_celula.py:82`) |
   | Propositor/classificador | Propositor retorna não avaliável; classificador retorna `None`. (`app/ponto/propositor.py:36`; `app/ponto/propositor.py:37`; `app/escala/orfa_classe.py:54`; `app/escala/orfa_classe.py:56`) |
   | Ausência parcial | A leitura temporal inclui desconhecimento, `[None]`. (`app/ponto/turnos.py:2667`; `app/ponto/turnos.py:2669`) |
   | PDF/calendário | Batidas podem receber `orfa=None`; o PDF não incrementa abertos quando a classificação é desconhecida. (`app/relatorios/pdf_espelho.py:185`; `app/relatorios/pdf_espelho.py:214`; `app/relatorios/pdf_espelho.py:215`; `app/colaboradores/services/calendario.py:624`; `app/colaboradores/services/calendario.py:628`) |

   As portas escalares preservam a quantidade legada; o plantio preserva suas horas HH:MM pelo adaptador. Essas mudanças temporais decorrem do contrato implementado, mas seus totais reais por consumidor não constam da sonda: [nao verificado]. (`app/escala/services/ata.py:78`; `app/escala/services/ata.py:118`; `app/escala/services/ata.py:131`)

6. **CI: a suite canônica usa ci; a catraca não fica desligada por escolha de settings.** `bin/suite.sh` executa explicitamente `--settings=config.settings.ci`; esse perfil ativa a flag e seleciona `RunnerAtaVigiada`. (`bin/suite.sh:232`; `bin/suite.sh:233`; `app/config/settings/ci.py:40`; `app/config/settings/ci.py:42`)

   | Execução/prova | Settings e alcance |
   |---|---|
   | Suite canônica, inclusive testes Django/selos | `config.settings.ci`. (`bin/suite.sh:233`) |
   | Dirigidos puros por `python -m unittest` | O carregador AST executa funções sem inicializar settings Django. (`app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:19`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:28`) |
   | Prova local de `ready` | `django.conf` falso, com flag definida pelo teste. (`app/escala/tests/test_ata_ci.py:52`; `app/escala/tests/test_ata_ci.py:55`) |
   | Prova da registry real | Usa settings já ativos, exige a flag e pula quando Django/registry não está disponível. (`app/escala/tests/test_ata_ci.py:129`; `app/escala/tests/test_ata_ci.py:132`; `app/escala/tests/test_ata_ci.py:135`) |
   | Prova do runner fora do caso | Executa a classe real com runner pai simulado. (`app/escala/tests/test_ata_ci.py:109`; `app/escala/tests/test_ata_ci.py:119`) |

   A instalação depende da flag em `ready`. Prod importa base, SaaS importa base, sombra importa SaaS; esses caminhos não importam ci. Não há ativação normal pelo código desses perfis. (`app/escala/apps.py:11`; `app/config/settings/prod.py:1`; `app/config/settings/saas.py:10`; `app/config/settings/sombra.py:27`)

   Entretanto, depois de instalada, a instrumentação modifica a classe do modelo, o field e `unittest.TestCase.run` sem desinstalação. Trocar settings no **mesmo processo** não desfaz isso; ocorrência real dessa troca é [nao verificado]. (`app/escala/services/ata_ci.py:37`; `app/escala/services/ata_ci.py:47`; `app/escala/services/ata_ci.py:72`)

7. **BLOQUEANTE — teste novo de mutação está vermelho na árvore atual.** `test_MORDE_mutante_copia_perde_catraca` altera texto de `copiar_envelope`, mas extrai somente `copiar_ata`. A função extraída chama o `copiar_envelope` original do namespace; a mutação não alcança a operação executada e o `assertRaises` externo falha. (`app/escala/tests/test_selo_tipo_na_fonte_orfas.py:143`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:146`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:149`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:24`; `app/escala/services/ata.py:102`)

   A rodada local dos dez módulos puros terminou em **74 testes, uma falha e um skip**; a falha foi essa. Registro persistido da execução: [nao verificado em arquivo]. O skip previsto depende da disponibilidade de Django/registry. (`app/escala/tests/test_ata_ci.py:129`; `app/escala/tests/test_ata_ci.py:132`)

   Além disso, fixtures existentes seguem incompatíveis: T4.1 lavra `n_orfas=2` sem `orfas` e espera `None`; calendário escreve ISO sem `n_orfas`. O novo acessor rejeita ambas. Execução Django desses cenários: [nao verificado]. (`app/escala/tests/test_t41_leitor_celula.py:90`; `app/escala/tests/test_t41_leitor_celula.py:94`; `app/colaboradores/tests/test_calendario_orfa.py:81`; `app/escala/services/ata.py:45`; `app/escala/services/ata.py:47`)

8. **BLOQUEANTE — selo anterior perdeu mordida pela troca de formato.** O selo O118 ainda intersecta `lampada.luz` HH:MM com `ata.orfas` agora ISO. Assim, a mesma batida acesa e órfã pode produzir interseção vazia. A ata usada nesse selo vem diretamente de `ata_do_dia`, como dict comum, sem passar pelo descriptor ORM. (`app/ponto/tests/test_o118_orfa_que_acendeu.py:84`; `app/ponto/tests/test_o118_orfa_que_acendeu.py:89`; `app/ponto/tests/test_o118_orfa_que_acendeu.py:90`; `app/ponto/tests/test_o118_orfa_que

[truncado em 20000 de 24670 caracteres]

## 12/11 20:41 · sessao 4eab9461e94e · - · - · origem=Ronald (tmux)

Confirmado com execução pura do cenário já existente em `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:91`: a saída fica acesa e o mesmo instante permanece órfão; o teste verifica somente que acendeu em `:98`.

**Bloqueante — segundo juiz funcional / derivação paralela.** O cartório incorpora a órfã em `_reais` (`app/ponto/services/cartorio.py:629`), atribui-a ao marco por proximidade (`:664`, `:670`, `:672`) e grava a lista original de órfãs (`:770`). A pergunta “que marco previsto a batida ocupa?” já pertence a `_match_marcos` (`app/core/juizes.py:212`; `app/docs/JUIZES.md:27`).

O ramo foi reescrito pelo diff fornecido e o novo teste aceita a contradição (`app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:98`). A aproximação já aparecia no ramo anterior mostrado no anexo; comparação literal com a base nesta cópia: [nao verificado].

## 12/11 20:40 · sessao 4eab9461e94e · - · - · origem=Ronald (tmux)

Censo estático consolidado:

| Valor | Origem/escritor | Leitores/transportadores de produção |
|---|---|---|
| JSON `orfas` e `n_orfas` | `app/escala/services/ata.py:233` e `:234`; chamado por `app/ponto/services/cartorio.py:770` e `app/escala/utils.py:1903` | Leitura semântica central `app/escala/services/ata.py:45`; persistência/cópias integrais `app/ponto/portas/celula.py:945`, `:995`, `:1006`, `:1033`; leitor cru operacional `bin/snapshot_o118_col221.py:32`. |
| `precedencia` | Conteúdo vem de `fatos_do_dia`, em `app/ponto/services/cartorio.py:2007`; preparado em `:2008`; escrito em `app/ponto/portas/celula.py:1087` | Comparação em `app/ponto/portas/celula.py:1081`; preservação dinâmica em `:997`, `:998`, `:999`, pela chave declarada em `:104`. Outro leitor de conteúdo por alias: [nao verificado]. |
| Retorno `leitura_orfas` | `app/escala/services/ata.py:55`, `:58`, `:70` | `ler_orfas` em `:74`, `contar_orfas` em `:78`, `copiar_ata` em `:101`, `horas_orfas` em `:129`. |
| Retorno `ler_orfas` | `app/escala/services/ata.py:74` | `app/escala/services/leitor_celula.py:186`. |
| Retorno `contar_orfas` | `app/escala/services/ata.py:78` | `app/escala/services/leitor_celula.py:192`; `app/ponto/services/cartorio.py:841`. |
| Retorno `horas_orfas` | Legado copiado em `app/escala/services/ata.py:131`; ISO convertido para hora local em `:133` | `app/chamados/services/materializacao.py:182`. |
| Retorno `orfas_da_ata` | `app/escala/services/leitor_celula.py:186` | `app/escala/services/leitor_celula.py:422`; `app/ponto/juiz_batida.py:163`; `app/ponto/management/commands/tripwire_celulas.py:235`; `app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/vigia_pareador_celula.py:76`; `app/ponto/services/cluster_espurio.py:30`. |
| Retorno `contagem_orfas_da_ata` | `app/escala/services/leitor_celula.py:192` | `app/escala/services/leitor_celula.py:422`; `app/chamados/juizes.py:511`. |
| Retorno `contar_orfas_da_grade` | `app/escala/services/ata.py:118`, `:124` | `app/escala/services/furos_diarios.py:23`; `app/folha/causas.py:44`; `app/ponto/status_dia.py:189`; `app/ponto/supra_juiz.py:380` por alias definido em `:379`; `app/ponto/services/bordas_realizado.py:50`; `app/ponto/management/commands/detectar_par_relampago.py:141`; `app/ponto/services/cartorio.py:820`. |
| `copiar_ata` | `app/escala/services/ata.py:99` | `app/ponto/services/cartorio.py:927`. |
| `copiar_envelope` | `app/escala/services/ata.py:81` | `app/escala/services/ata.py:102`; `app/ponto/portas/celula.py:1086`. |
| `AtaVigiada` | Construção automática em `app/escala/services/ata_ci.py:22`, `:28`; cópias em `app/escala/services/ata.py:90`, `:210`, `:213` | Reconhecimento em `app/escala/services/ata_ci.py:21`, `:27`, `:43`; transporte JSON interno em `:44`; fiscalização em `app/escala/services/ata.py:162`; métodos de leitura/escrita em `:180`, `:184`, `:188`, `:192`. |

Achados relevantes:

1. **Leitura crua residual.** `bin/snapshot_o118_col221.py:32` lê `ata.orfas` diretamente; a ata foi obtida do modelo em `:18`. É script operacional que escreve snapshot em `:30`; execução atual e uso em produção: [nao verificado].

2. **Cópias cruas residuais.** `celula_como_o_julgamento_deixa` mantém `dict(ata)` em `app/ponto/portas/celula.py:945`; `lavrar_veredito` mantém outro em `:995`. Ambos atravessam a iteração proibida de `AtaVigiada`, definida em `app/escala/services/ata.py:192`. O chamador do primeiro é `app/chamados/juizes.py:424`, alimentado pelo estado corrente do cartório em `:422`. A passagem efetiva de wrapper por cada ramo na suíte completa: [nao verificado].

3. **O resíduo de precedência mencionado no relato foi corrigido na cópia atual.** `app/ponto/portas/celula.py:1086` usa `copiar_envelope`; `None` vira somente envelope vazio em `app/escala/services/ata.py:87`, sem produzir contagem zero. A validação de órfãs continua exclusiva de `copiar_ata`, em `:101`.

4. **Dia civil versus dia do turno permanece divergente.** A grade guarda as órfãs do turno em `app/escala/utils.py:1763` e as projeta em `:1816`; as sobras são distribuídas civilmente em `:1837` e `:1839`. O vigia declara o entendimento civil em `app/ponto/services/vigia_pareador_celula.py:67` e procura pela data civil em `:112`. A preexistência literal frente a `0a747193e` não pôde ser conferida nesta cópia sem `.git`: [nao verificado].

5. **Plantio foi tocado, mas mantém comparação HH:MM.** O código atual introduz o adaptador em `app/chamados/services/materializacao.py:179` e o consome em `:182`; slots e instante plantado continuam reduzidos a HH:MM em `:180` e `:183`. O diff fornecido mostra essa alteração; igualdade literal com o HEAD proibido de alterar: [nao verificado]. `dna.lampadas[].instante` continua sendo produzido sem fuso em `app/ponto/management/commands/gerar_celulas.py:69`.

6. **Não identifiquei juiz novo duplicando autoridade registrada nas novas portas de ata.** O registro conserva a propriedade do turno em `app/core/juizes.py:208` e do casamento em `:212`; a tabela os aponta em `app/docs/JUIZES.md:20` e `:27`. O encadeamento dos marcos usado pelo cartório delega à função existente em `app/ponto/services/cartorio.py:623`. Completude dinâmica dessa conclusão: [nao verificado].

7. **Contrato da grade menos estrito que sua documentação.** `contar_orfas_da_grade` documenta lista de datetimes em `app/escala/services/ata.py:108`, mas valida somente lista e contagem em `:119` e `:122`; não verifica datetime nem fuso dos elementos. Um JSON comum passado por engano pode ser aceito; o teste só prova rejeição quando o argumento é `AtaVigiada`, em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:124`.

8. **Docstring de retorno desatualizada.** `classificar_orfas` ainda promete chaves HH:MM em `app/escala/orfa_classe.py:50`, mas retorna chaves datetime em `:64`, `:69`, `:75`, ou `None` em `:56`. Chamador de produção dessa função: [nao verificado]; o uso localizado em `app/ponto/turnos.py:1581` importa somente a constante.

Os retornos projetados são consumidos como instantes/`None` pelo calendário em `app/colaboradores/services/calendario.py:380` e `:623`, propositor em `app/ponto/propositor.py:36` e `:81`, cobertura parcial em `app/ponto/turnos.py:2667`, adaptador da grade em `app/escala/utils.py:1089` e `:1114`, PDF em `app/relatorios/pdf_espelho.py:183`, `:203`, `:218`, e template em `app/templates/ponto/espelho.html:247`.

Testes que consomem diretamente as portas, incluindo os existentes alterados: [nao verificado]

- `leitura_orfas`: `app/escala/tests/test_tipo_na_fonte_orfas.py:34`, `:36`.
- `ler_orfas`: `app/escala/tests/test_ata_ci.py:65`, `:71`, `:97`, `:139`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:59`, `:74`, `:87`, `:97`, `:120`, `:132`; `app/escala/tests/test_preverif_v3_asserts.py:74`, `:94`; `app/escala/tests/test_ata_x_turno_entrada.py:144`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:104`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:137`; `app/ponto/tests/test_bateria_lavratura.py:639`, `:963`, `:1060`.
- `contar_orfas`: `app/escala/tests/test_ata_ci.py:64`, `:82`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:60`; `app/escala/tests/test_preverif_v3_asserts.py:75`; `app/escala/tests/test_tipo_item3_cartorio.py:72`; `app/ponto/tests/test_bateria_lavratura.py:544`, `:554`, `:648`, `:800`, `:952`.
- `orfas_da_ata`: `app/escala/tests/test_tipo_na_fonte_orfas.py:14`, `:53`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:45`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:74`, `:89`, `:121`, `:133`; observação com wrapper em `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:181`.
- `contagem_orfas_da_ata`: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:46`.
- `contar_orfas_da_grade`: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:127`.
- `copiar_ata`: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:131`, `:137`, `:148`.
- `gravar_orfas`: `app/escala/tests/test_tipo_na_fonte_orfas.py:65`, `:70`; `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:58`; `app/escala/tests/test_preverif_v3_asserts.py:60`, `:63`, `:106`, `:112`, `:123`, `:130`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:187`, `:337`; `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:131`.
- `horas_orfas`: exercitado indiretamente pelo plantio em `app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:74`, `:79`, `:85`.
- `copiar_envelope`: teste direto específico: [nao verificado]; o teste de cópia atual passa por `copiar_ata` em `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:131`, que delega em `app/escala/services/ata.py:102`.

## 12/11 20:39 · sessao 962f0e17ae3b · - · - · origem=Ronald (tmux)

Achado bloqueante: **ata ainda não lavrada passa a ser corrupção**, em caminhos sem filtro de lavra. `None` falha em `app/escala/services/ata.py:43`; `{}` e só `precedencia` falham em `app/escala/services/ata.py:47`. O transporte de precedência aceita explicitamente esses envelopes, portanto a própria fronteira reconhece sua legitimidade antes da lavra (`app/escala/services/ata.py:81`, `app/ponto/portas/celula.py:1086`).

Matriz dos chamadores de produção:

| Porta e chamador | Entrada possível e resultado |
|---|---|
| `leitura_orfas` ← `ler_orfas` | Chamada sem guarda; valida mapping e ambas as chaves (`app/escala/services/ata.py:74`, `app/escala/services/ata.py:40`). |
| `leitura_orfas` ← `contar_orfas` | Mesmo contrato; HH:MM coerente conserva quantidade (`app/escala/services/ata.py:78`, `app/escala/services/ata.py:57`). |
| `leitura_orfas` ← `copiar_ata` | Valida antes de transportar; `None/{}/precedencia` falham (`app/escala/services/ata.py:101`). |
| `leitura_orfas` ← `horas_orfas` | Legado preserva HH:MM; ISO converte ao fuso local; inválido levanta (`app/escala/services/ata.py:129`). |
| `ler_orfas` ← `orfas_da_ata` | `getattr(..., None)` permite chegar `None`; não há guarda de lavra (`app/escala/services/leitor_celula.py:186`). |
| `contar_orfas` ← `contagem_orfas_da_ata` | Idem (`app/escala/services/leitor_celula.py:192`). |
| `copiar_ata` ← `_ata_ate_o_teto` | Só chama com mapping e pendências; `{}`/precedência seriam rejeitados. Chamadas ordinárias fornecem `ata_do_dia` recém construída (`app/ponto/services/cartorio.py:918`, `app/ponto/services/cartorio.py:927`, `app/ponto/services/cartorio.py:1444`, `app/ponto/services/cartorio.py:1859`). |
| `contar_orfas` ← `insumos_da_ata` | Valida incondicionalmente, mesmo quando lâmpadas seriam suficientes (`app/ponto/services/cartorio.py:841`). Cartório usa atas novas (`app/ponto/services/cartorio.py:1321`, `app/ponto/services/cartorio.py:1880`); backfill e vigia usam persistidas, filtrando `julgada_em` (`app/ponto/management/commands/backfill_b5_insumos.py:61`, `app/ponto/management/commands/backfill_b5_insumos.py:73`, `app/ponto/management/commands/vigia_b5_ausencia.py:55`, `app/ponto/management/commands/vigia_b5_ausencia.py:79`). |
| `contagem_orfas_da_ata` ← `_celula_furada` | `None/{}/precedência` retornam `None` antes do acessor por ausência de `lampadas`; chave faltante numa ata com lâmpadas chega ao acessor salvo apagada anterior; exceção é engolida, retornando `None` (`app/chamados/juizes.py:493`, `app/chamados/juizes.py:505`, `app/chamados/juizes.py:508`, `app/chamados/juizes.py:511`, `app/chamados/juizes.py:521`). |
| `orfas_da_ata` e contagem ← `grade_da_celula` | Célula inexistente não chama; célula existente, inclusive sem lavra, chama ambos sem guarda. `None/{}/precedência` levantam (`app/escala/services/leitor_celula.py:380`, `app/escala/services/leitor_celula.py:396`, `app/escala/services/leitor_celula.py:422`). |
| `orfas_da_ata` ← `periodos_do_dia` | Chamada incondicional antes das verificações do DNA/lâmpadas; `None/{}/precedência` levantam (`app/ponto/juiz_batida.py:158`, `app/ponto/juiz_batida.py:163`, `app/ponto/juiz_batida.py:176`). |
| `orfas_da_ata` ← cluster | Só dia sem trabalho retorna antes; célula de trabalho não lavrada levanta, em vez de devolver não medido (`app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:30`). |
| `orfas_da_ata` ← esmeril | Consulta todas as células do período; não filtra lavra. Só `so_escala=True` evita essa leitura (`app/ponto/services/esmeril_espelho.py:96`, `app/ponto/services/esmeril_espelho.py:102`, `app/ponto/services/esmeril_espelho.py:126`). |
| `orfas_da_ata` ← vigia pareador | Consulta todas as atas da janela ampliada; transforma `None` em `{}` e chama; não filtra lavra (`app/ponto/services/vigia_pareador_celula.py:71`, `app/ponto/services/vigia_pareador_celula.py:75`, `app/ponto/services/vigia_pareador_celula.py:76`). |
| `orfas_da_ata` ← tripwire | Consulta células `trabalha=True`; apenas menos de dois marcos evita leitura. Ata não lavrada com dois marcos levanta (`app/ponto/management/commands/tripwire_celulas.py:228`, `app/ponto/management/commands/tripwire_celulas.py:233`, `app/ponto/management/commands/tripwire_celulas.py:235`). |
| `horas_orfas` ← plantio | `None/{}/precedência` sem lâmpadas não chega; com lâmpadas e chaves ausentes levanta e `except Exception` devolve `False` (`app/chamados/services/materializacao.py:172`, `app/chamados/services/materializacao.py:175`, `app/chamados/services/materializacao.py:182`, `app/chamados/services/materializacao.py:186`). |

Propagação e mudanças silenciosas relevantes:

- `periodos_do_dia` chega de `turnos`: exceção engolida exclui célula da alimentação de papéis (`app/ponto/turnos.py:1633`, `app/ponto/turnos.py:1634`); chega do motor: exceção registrada e dia ignorado (`app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:617`); chega do censo de cadastro sem filtro de lavra e sem captura local (`app/escala/services/cadastro_realidade.py:261`, `app/escala/services/cadastro_realidade.py:266`); chega de retenção pela pausa sem captura local (`app/folha/export.py:1150`).
- Uma célula não lavrada dentro de uma janela faz `grade_da_celula` levantar; o espelho captura e **zera a grade do período**, sem cair no builder nesse ramo (`app/ponto/services/espelho.py:519`, `app/ponto/services/espelho.py:569`); o calendário captura e perde o mapa das órfãs (`app/colaboradores/services/calendario.py:334`, `app/colaboradores/services/calendario.py:399`).
- Calculadores de inteligência capturam a falha da grade e excluem o colaborador da métrica (`app/inteligencia/calculadores.py:42`, `app/inteligencia/calculadores.py:43`, `app/inteligencia/calculadores.py:143`, `app/inteligencia/calculadores.py:144`); materialização captura e devolve régua vazia (`app/chamados/services/materializacao.py:235`, `app/chamados/services/materializacao.py:237`).

Para HH:MM válido, as contagens permanecem conhecidas, mas os leitores temporais mudam resultados:

- Toda lista HH:MM não vazia vira `instantes=None` (`app/escala/services/ata.py:57`). Cluster devolve não medido, sem detectar rajada legada (`app/ponto/services/cluster_espurio.py:31`); esmeril deixa de preencher A5 (`app/ponto/services/esmeril_espelho.py:126`); tripwire exclui a célula do universo medido (`app/ponto/management/commands/tripwire_celulas.py:236`); vigia não a inclui em `celulas_com_orfa` nem em `com_orfa` (`app/ponto/services/vigia_pareador_celula.py:77`, `app/ponto/services/vigia_pareador_celula.py:79`, `app/ponto/services/vigia_pareador_celula.py:82`).
- O propositor não avalia o legado projetado (`app/ponto/propositor.py:36`); classificador devolve `None` (`app/escala/orfa_classe.py:54`); horas da ausência parcial passam a incluir desconhecido (`app/ponto/turnos.py:2667`); PDF marca batidas `orfa=None` e não soma turnos abertos cuja classificação é desconhecida (`app/relatorios/pdf_espelho.py:183`, `app/relatorios/pdf_espelho.py:214`).
- Essas mudanças temporais podem decorrer intencionalmente do contrato EXPAND; a perda de quantidade foi evitada nas portas escalares (`app/escala/services/ata.py:105`, `app/ponto/status_dia.py:189`, `app/ponto/services/cartorio.py:820`). O problema bloqueante distinto é aplicar corrupção às células legitimamente não lavradas (`app/escala/services/leitor_celula.py:422`, `app/escala/services/ata.py:43`).

Reprodução local, sem arquivos nem banco: executei accessor, tripwire com doubles existentes e grade completa com carregador de previsto isolado; as três entradas `None`, `{}`, só precedência levantaram `EstadoIlegalDaAta`; HH:MM coerente retornou `None` temporal, e tripwire `( [], 0 )`. As funções reais correspondentes estão em `app/escala/services/ata.py:40`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:39`, `app/escala/tests/test_tipo_na_fonte_orfas_v2.py:81`. Registro persistido dessa execução: [nao verificado].

## 12/11 20:38 · sessao 54a4e982ac49 · - · - · origem=Ronald (tmux)

1. **Bloqueante — selo/mutante quebrado na árvore atual.** `test_MORDE_mutante_copia_perde_catraca` modifica a expressão de `copiar_envelope`, mas carrega somente `copiar_ata`; esta chama a implementação original de `copiar_envelope`. Portanto o mutante não atinge a cópia executada e o teste falha em seu `assertRaises` externo. Referências: `app/escala/tests/test_selo_tipo_na_fonte_orfas.py:143`, `:146`, `:149`; `app/escala/services/ata.py:90`, `:102`.

   Executei agregado de dez módulos puros com `PYTHONDONTWRITEBYTECODE=1`: **74 testes, 1 falha, 1 skipped**, falha acima. Registro persistido da execução: [nao verificado]. O skip decorre da indisponibilidade de Django/registry, prevista em `app/escala/tests/test_ata_ci.py:129`.

2. **Bloqueante — selo anterior perdeu mordida pela troca de formato.** O selo O118 ainda intersecta `lampada.luz` HH:MM com as strings ISO de `ata.orfas`: `app/ponto/tests/test_o118_orfa_que_acendeu.py:89`, `:90`, `:92`; escritor ISO: `app/escala/services/ata.py:232`. Reproduzi por extração AST do método real com a mesma identidade ISO na lâmpada acesa e na lista de órfãs: o selo passou. Registro persistido dessa execução: [nao verificado]. A ata usada pelo selo nasce como dict normal de `ata_do_dia`, sem atravessar o descriptor ORM: `app/ponto/tests/test_o118_orfa_que_acendeu.py:84`; a catraca automática não cobre essa leitura.

3. **Bloqueante para alegação de suite verde — fixtures incompatíveis com contrato novo.** [nao verificado]
   - `app/escala/tests/test_t41_leitor_celula.py:88` lavra ata com `n_orfas=2` e sem `orfas`; em `:94` espera `None`. O acessor agora exige ambas as chaves e levanta por ausência: `app/escala/services/ata.py:45`, `:47`.
   - A fixture alterada do calendário escreve ISO sem `n_orfas`: `app/colaboradores/tests/test_calendario_orfa.py:79`, `:81`; mesma rejeição em `app/escala/services/ata.py:45`.
   - Execução Django desses cenários: [nao verificado].

4. **Settings: a hipótese de suite sem ci não procede.** A execução efetiva termina em `manage.py test ... --settings=config.settings.ci`: `bin/suite.sh:232`, `:233`; saída seca também mostra ci: `bin/suite.sh:188`. O perfil ci define o runner novo e ativa a flag: `app/config/settings/ci.py:40`, `:42`. `EscalaConfig.ready` instala descriptor e hook somente quando a flag é verdadeira: `app/escala/apps.py:11`, `:13`, `:14`.

   `prod` importa base, `saas` importa base e `sombra` importa saas: `app/config/settings/prod.py:1`, `app/config/settings/saas.py:10`, `app/config/settings/sombra.py:27`. Não há definição de `VIGIAR_ATA_ORFAS` nesses perfis na busca do archive; ativação normal neles é falsa pelo default explícito em `app/escala/apps.py:11`. Já depois de instalada, a instrumentação modifica a classe e `unittest.TestCase.run` permanentemente naquele processo; não existe desinstalação no código: `app/escala/services/ata_ci.py:37`, `:47`, `:72`. Uso real de troca ci→prod/sombra no mesmo processo: [nao verificado].

5. **Settings de cada tipo de prova.**
   - Selos Django lançados por `bin/suite.sh`: ci, `bin/suite.sh:233`.
   - Testes puros por `python -m unittest`: não inicializam settings no seu carregador AST, `app/escala/tests/test_tipo_na_fonte_orfas_fluxo.py:20`, `:29`.
   - Prova automática local de `ready`: usa `django.conf` falso com a flag escolhida pelo teste, `app/escala/tests/test_ata_ci.py:52`, `:55`.
   - Prova de registry real: usa os settings já ativos, exige a flag e pode pular se registry/Django faltar, `app/escala/tests/test_ata_ci.py:129`, `:132`, `:135`.
   - Prova do runner em teardown: usa pai simulado, `app/escala/tests/test_ata_ci.py:109`, `:119`; execução real paralela: [nao verificado].

6. **Resíduos de indicadores.** A busca do archive encontrou `nao_medidos` apenas em três asserts negativos: `app/escala/tests/test_tipo_item3_expand_contadores.py:37`, `:39`, `:146`. `cego_orfas`: nenhuma ocorrência na busca textual do archive [nao verificado — ausência global não possui linha positiva citável]. Nenhuma saída de produção desses indicadores foi encontrada [nao verificado].

## 12/11 20:25 · sessao 525ac3c7b50c · TIPO-NA-FONTE · construtor · origem=Code

Entrega parcial no [relato completo](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/RELATO_TIPO_ITEM3_V3.txt:1), com alterações, leitores/escritores, REDs e comandos dos mutantes.

- **92 testes passaram; 1 pulado.** [Evidência](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/TIPO_ITEM3_V3_TESTES.txt:214). [nao verificado]
- **Pendência:** a catraca detectou cópia crua em `ponto/portas/celula.py:1083`, arquivo proibido; a meta zero permanece aberta. [Reprodução](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/TIPO_ITEM3_V3_TESTES.txt:220).
- Suite Django: **[nao verificado]**, por ausência de `django.test`. [Tentativa](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/TIPO_ITEM3_V3_TESTES.txt:241). [nao verificado]

Sem commit, conforme a proibição do envelope. [Registro](/home/ronald/wt-codex-tipo-na-fonte-184856/app/docs/RELATO_TIPO_ITEM3_V3.txt:151). [nao verificado]

## 12/11 20:08 · sessao d89d76f9010a · - · - · origem=Ronald (tmux)

Item 5 editado: removidos `nao_medidos` e `cego_orfas`; contadores usam `contar_orfas_da_grade`, preservando quantidade legada conhecida. Referências: `escala/services/furos_diarios.py:22`, `folha/causas.py:43`, `ponto/status_dia.py:185`, `ponto/services/bordas_realizado.py:49`, `ponto/management/commands/detectar_par_relampago.py:138`.

Também removidas as saídas acrescentadas em `pos_vinculo`, esmeril, tripwire e PDF; o vigia conserva apenas seu `cego` histórico por universo. `ponto/services/vigia_pareador_celula.py:117`, `ponto/services/esmeril_espelho.py:195`, `relatorios/pdf_espelho.py:183`.

Teste novo: `escala/tests/test_tipo_item3_expand_contadores.py:16`: 6 REDs e 5 mutantes mordidos — furos, status, borda, causa e score. Comando executado:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
 escala.tests.test_tipo_item3_expand_contadores \
 escala.tests.test_tipo_na_fonte_orfas_consumidores \
 folha.tests.test_causas folha.tests.test_causas_sem_previsao -q
```

Resultado observado: **35 testes OK**. O negativo de estado ilegal isola o registro esperado e verifica três sítios; `escala/tests/test_tipo_item3_expand_contadores.py:47`.

Achado 6: **pre-existente, DECIDIDO PELA LEI TURNO-G5, obra própria**. Comparação por `git show 0a747193e:app/...`: naquele HEAD, `escala/utils.py:1758` guarda por dia do turno, `:1811` projeta nesse dia e `:1820` agrupa apenas sobras civis; `ponto/services/vigia_pareador_celula.py:109` procura na data civil. Equivalentes atuais: `escala/utils.py:1763`, `:1816`, `:1825` e `ponto/services/vigia_pareador_celula.py:112`. Nenhuma cura desse achado.

Limite para o relato: legado conserva quantidade, porém identidade temporal segue `None`; A5, PDF e vigia não recuperam datas ausentes. Efeito em produção: **[nao verificado]**.

## 12/11 20:06 · sessao 0062468afb3b · - · - · origem=Ronald (tmux)

Alterações concluídas nos arquivos autorizados:

- Cinco asserts via acessor: `test_ata_x_turno_entrada.py:144`, `test_saida_valida_cross_meia_noite.py:104`, `test_juiz_do_par_ata_casa_pelo_turno.py:137`, `test_bateria_lavratura.py:639/:1060`.
- Selo compara identidades com data/fuso: `ponto/tests/test_bateria_lavratura.py:961`.
- Fixture ganhou `orfas=[]`: `chamados/tests/test_emis_ausencia_parcial_cobra.py:60`.
- Fixtures de convergência usam escritor único: `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:187/:337`.

Provas em `escala/tests/test_preverif_v3_asserts.py:45`: seis testes passaram com:

```sh
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest escala.tests.test_preverif_v3_asserts -v
```

Mutantes mordidos: fixture perde chave (`:78`), escritor duplica a mesma batida acesa como órfã (`:109`) e selo volta a HH:MM versus ISO (`:128`). O segundo compila o escritor real mutado em memória.

AST dos seis arquivos e `git diff --check` passaram. Integração Django/ORM: [nao verificado].

## 12/11 20:04 · sessao 4febaafe94c6 · - · - · origem=Ronald (tmux)

Restaurei o plantio por HH:MM do HEAD; a única diferença preservada é ler órfãs por `horas_orfas(ata)` ([materializacao.py](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/services/materializacao.py:177)).

Prova em [test_tipo_na_fonte_plantio_expand.py](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:74):

- RED 1 executado antes da correção: falhou `False is not true`.
- Mutante que reintroduz instantes naive executado: mordeu, confirmando a exceção `marca exige fuso` ([teste:85](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:85)). [nao verificado]
- Incluídos contrastes dia completo e órfã legado/ISO ocupando a saída ([teste:77](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/tests/test_tipo_na_fonte_plantio_expand.py:77)). [nao verificado]

Comando completo, para rodar após sua implementação de `horas_orfas`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_tipo_na_fonte_plantio_expand -v
```

Na última execução, o RED positivo ainda falhava porque `horas_orfas` estava ausente; os outros três testes passaram. Não alterei `juiz_batida.py` nem `gerar_celulas.py`; o plantio corrigido deixou de consumir `periodos_do_dia` ([materializacao.py:179](/home/ronald/wt-codex-tipo-na-fonte-184856/app/chamados/services/materializacao.py:179)). [nao verificado]

## 12/11 20:01 · sessao ab67b3003d37 · TRANCA-VIVAS-08 · verificador · origem=Code

**REPROVA — bloqueante: o acumulador ainda pode perder competências sem contrariar os oráculos do arquivo.** A troca `tot[k] += linha[k]` → `tot[k] = linha[k]` sobrevive porque os casos medem uma única competência. (`app/chamados/services/competencia_trancada.py:355`; `app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:244`)

1. **A resgatada percorre as entradas reais de `medir()`.** A fixture cria o chamado pela porta, fornece `data_turno`, grava `resgatado_em = fechado_em + 5 minutos` e escreve `q.chamado_destino`. O leitor calcula `competencia_do_chamado`, exige `resgatado_em > fechado_em` e consulta exatamente `q.chamado_destino_id`. O resgate é montado por `update`; o ato de resgatar pela porta não é exercitado nesse teste. (`app/chamados/tests/test_tranca_vivas_08.py:226`; `app/chamados/tests/test_tranca_vivas_08.py:229`; `app/chamados/tests/test_tranca_vivas_08.py:231`; `app/chamados/services/competencia_trancada.py:105`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`)

   A criação resolve a célula pelo dia informado, e a competência usa `dia_do_fato`. O mock não desloca essa competência por perguntas vivas: esse percurso consulta `disputa__chamado_id`, enquanto a fixture escreve somente `chamado_destino`; a disputa nasce sem chamado-pai. (`app/chamados/models.py:1067`; `app/chamados/models.py:1235`; `app/chamados/juizes.py:2359`; `app/chamados/juizes.py:2343`; `app/chamados/tests/test_c7_red_minimos.py:108`)

2. **O mock altera a vida, mas não torna a agregação vácua.** `dia_da_pergunta_trancado=False` remove o degrau que mataria a pergunta pela tranca. Não substitui a formação de `resgatados`, os carimbos ou a FK. O oráculo exige explicitamente `([q.pk], [], [])` nas três parcelas e união **1**, tanto no total quanto na linha. Portanto, prova a agregação **condicionada à vida simulada**; não prova que o resgate real produz essa vida. (`app/chamados/juizes.py:1405`; `app/chamados/services/competencia_trancada.py:117`; `app/chamados/services/competencia_trancada.py:132`; `app/chamados/tests/test_tranca_vivas_08.py:239`; `app/chamados/tests/test_tranca_vivas_08.py:241`; `app/chamados/tests/test_tranca_vivas_08.py:247`)

3. **A perda da resgatada está curada; permanecem sobreviventes de agregação.**

   | Local | Resultado contra os oráculos |
   |---|---|
   | União, linha 348 | Retirar resgatadas é mordido pelo novo MORDE; trocar união por concatenação continua mordido pelo caso de sobreposição. (`app/chamados/tests/test_tranca_vivas_08.py:249`; `app/chamados/tests/test_tranca_vivas_08.py:203`) |
   | Cardinalidade da união, linha 351 | O antigo mutante `len(_com_o_admin) - len(m['vivas_resgatadas'])` agora devolve **0** onde linha e total exigem **1**. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/tests/test_tranca_vivas_08.py:244`; `app/chamados/tests/test_tranca_vivas_08.py:247`) |
   | Parcelas por competência, linha 353 | `len(m[k])` → `min(1, len(m[k]))` sobrevive: parcelas verificadas são zero ou um. Com duas resgatadas, publicaria parcela **1**, embora a união fosse **2**. (`app/chamados/services/competencia_trancada.py:353`; `app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/tests/test_tranca_vivas_08.py:244`) |
   | Acumulador, linha 355 | `+=` → `=` sobrevive. Com duas competências contendo uma resgatada cada, o original totaliza **2** e o mutante conserva apenas **1**. (`app/chamados/services/competencia_trancada.py:355`; `app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:246`) |

   Os dois sobreviventes foram conferidos em memória sobre o corpo real de `medir_frota`, com entradas sintéticas; preservam os resultados dos cenários do arquivo, inclusive os MORDE. Execução Django/ORM e sobrevivência na suíte completa: **[nao verificado]**. (`app/chamados/services/competencia_trancada.py:331`; `app/chamados/tests/test_tranca_vivas_08.py:206`; `app/chamados/tests/test_tranca_vivas_08.py:251`)

4. **A API morde troca entre os quatro campos.** Os valores **7, 3, 2, 4** são distintos, e cada chave é comparada com seu próprio valor; qualquer troca de pares falha. Conferi as seis trocas sobre o dicionário real da view. O residual também é certificado, com **5**. A projeção interna de `por_competencia` não recebe essa prova: o falso usa lista vazia. (`app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:57`; `app/api/tests/test_api_mensageria_chamados_em_competencia_trancada.py:64`; `app/api/views_mensageria.py:1573`)

5. **Quem escreve e quem lê os valores publicados:** [nao verificado]

   | Campo | Escritor do valor | Leitor |
   |---|---|---|
   | `decidir_pelo_dp` | `medir()` escreve IDs; agregador escreve contagens. (`app/chamados/services/competencia_trancada.py:139`; `app/chamados/services/competencia_trancada.py:353`) | União e pauta do DP. (`app/chamados/services/competencia_trancada.py:348`; `app/chamados/services/competencia_trancada.py:246`) |
   | `vivas_resgatadas` | `medir()` escreve IDs; agregador conta. (`app/chamados/services/competencia_trancada.py:149`; `app/chamados/services/competencia_trancada.py:353`) | União. (`app/chamados/services/competencia_trancada.py:348`) |
   | `vivas_tocadas_pelo_admin` | `medir()` escreve IDs; agregador conta. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:353`) | União. (`app/chamados/services/competencia_trancada.py:348`) |
   | `ficam_com_o_admin` | Cardinalidade por competência e acumulação total. (`app/chamados/services/competencia_trancada.py:351`; `app/chamados/services/competencia_trancada.py:355`) | CLI e API. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:22`; `app/api/views_mensageria.py:1573`) |
   | `vivas_apesar_da_tranca` | Classificação residual, cardinalidade e total. (`app/chamados/services/competencia_trancada.py:158`; `app/chamados/services/competencia_trancada.py:352`; `app/chamados/services/competencia_trancada.py:355`) | CLI e API. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:27`; `app/api/views_mensageria.py:1577`) |

   As três parcelas também são lidas pela CLI e pela API. O cliente da mensageria recebe o JSON inteiro; a ferramenta acrescenta `cliente` e repassa o payload. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:24`; `app/api/views_mensageria.py:1574`; `mensageria/nucleo/core_client.py:241`; `mensageria/nucleo/ferramentas.py:2038`)

Para fechar os sobreviventes demonstrados, faltam oráculos com **duas competências positivas** e **mais de um ID numa parcela**. Os atuais conferem uma competência e parcelas zero/um. (`app/chamados/tests/test_tranca_vivas_08.py:36`; `app/chamados/tests/test_tranca_vivas_08.py:186`; `app/chamados/tests/test_tranca_vivas_08.py:244`)

## 12/11 20:01 · sessao c24cf91fbb4c · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

C5e escrita. O [relato completo](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:6) contém funções alteradas, chamadores, origem/leitores de `hoje` e comandos.

- **164 testes sem banco passaram**; RED da meia-noite falhou contra o corpo da base. [Evidências](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:107). [nao verificado]
- **38 mutantes morderam por asserção**, incluindo leitura nova e `desfazer_apaga_admin`. [Comandos e resultados](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:162).
- Catraca final: **31**, abaixo do teto herdado 36; sítios remanescentes enumerados. [Censo](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-193752/app/docs/RELATO-BUG-CADASTRO-C5E.txt:61).

Suíte ORM: **[nao verificado]**, sem Django disponível e com acesso a banco vetado. Sem commit, pela proibição inicial. Certificação permanece com Code/suite e opus final. [nao verificado]

_4435 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
