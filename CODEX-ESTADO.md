# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 13:29** · publicado em **13/11/2026 13:29** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 13:29 · sessao 05c3d77a4e24 · DESLIGADO-NAO-TRAVA · verificador · origem=Code

**Pré-verificação: BLOQUEANTE. O contador `desligados_retidos`, esperado 0 e lido pelo ESTADO, não existe na cópia examinada.** `barrados_desligado` mede outra coisa: pares excluídos do comparador C5. A integração existente do ESTADO lê o carimbo de fio/C5. (`bin/gerar_estado.py:260`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/chamados/management/commands/disparar_perguntas_competencia.py:61`)

A identidade da cópia com o commit/base informados não pôde ser conferida por Git: a cópia não contém repositório `.git`. `[nao verificado]` [nao verificado]

**1. BLOQUEANTE — o informativo foi produzido, mas não integrado à apresentação do DP.**

`informativo_rescisao` usa o mesmo laço do retentor e aparece no retorno de `o_que_segura`. Entretanto, o lavrador do Retrato não grava esse informativo; a tela e o drawer continuam lendo os campos antigos do Retrato. Existe acesso manual pela ferramenta; não está implementada nesses caminhos a apresentação dos dias ao DP. (`app/folha/export.py:1228`; `app/folha/fora_da_folha.py:232`; `app/folha/retrato_folha.py:1177`; `app/folha/views_retrato.py:190`; `app/colaboradores/views.py:984`)

O teste comprova a chave no retorno da função, sem percorrer tela/PDF do DP. (`app/folha/tests/test_lei_desligado_nao_trava.py:100`)

**2. BLOQUEANTE — desligado ainda pode sofrer trava de aprovação por outro caminho.**

`classificar_export` acrescenta `turno_aberto` a `travas_aprovacao` sem excluir desligados. A aprovação individual recusa essa trava; o lote também exclui esses fechamentos. Portanto, zerar `_dias_que_retem` não zerou todas as travas da folha. (`app/folha/export.py:833`; `app/folha/export.py:848`; `app/ponto/views.py:1509`; `app/ponto/views.py:1308`)

O carimbo automático na geração também conserva esse crivo. Isso **não demonstra impedimento de emissão da linha no TXT**: a montagem aceita fechamentos `aberto` e `aprovado`, selecionados pelo resultado `entra`. (`app/folha/views.py:244`; `app/folha/export.py:687`; `app/folha/export.py:716`)

O teste novo verifica somente que o desligado não seja `('fora', 'furo_espelho')`; não exige ausência de `travas_aprovacao`. (`app/folha/tests/test_lei_desligado_nao_trava.py:89`)

**3. BLOQUEANTE — o requisito “nenhum leitor deriva por conta própria” não está cumprido.**

Há leitores fora do diff com respostas próprias:

| Leitor | Derivação sobrevivente |
|---|---|
| `raiox_export` | Seleciona os retidos pelo export, mas reconstrói os dias com `furos_por_dia`, recortando diretamente por `data_demissao`. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`) |
| Preview do export | Apresenta como quantidade de dias o comprimento de `motivos_espelho`, que contém motivos, não datas. (`app/folha/views.py:398`; `app/folha/views.py:402`) |
| Lista de fechamento | Traduz qualquer motivo não previsto no mapa para `furo_espelho`; o motivo original fica separado em `motivo_juiz`. (`app/ponto/services/fechamento_lista.py:25`; `app/ponto/services/fechamento_lista.py:36`) |
| API de mensageria | Chama de `retidos` todos os itens `fora`, incluindo saída pelo módulo de rescisão. Esse número não é “dias desligados retidos”. (`app/api/views_mensageria.py:283`; `app/api/views_mensageria.py:301`; `app/folha/export.py:867`) |

O Raio-X depende previamente do balde `furo_espelho` do export; portanto, sua derivação paralela não prova, sozinha, que o desligado voltou a reter. Prova que o censo não pode ser declarado limpo. (`app/folha/views.py:589`; `app/folha/views.py:605`)

**4. BLOQUEANTE — INSS pode apagar o estado usado para reconhecer o desligado.**

A lista da tela oferece apenas ativos/afastados, mas o POST busca o colaborador somente por `pk`. Após lançar INSS, grava `situacao='afastado'`, sem preservar `desligado`. A escrita passa pela porta legítima: é outro escritor semântico do campo, não escritor fora da porta. (`app/ponto/views.py:2372`; `app/ponto/views.py:2406`; `app/ponto/views.py:2433`; `app/colaboradores/services/colaborador.py:32`; `app/colaboradores/services/colaborador.py:55`)

Depois dessa troca, `esta_desligado` responde falso, o retentor volta ao laço e a C5 deixa de excluir a pessoa. Contraexemplo estático: desligado com saída depois da competência, furos anteriores e INSS posterior aos furos; o lançamento pode devolver os furos à retenção. Execução desse cenário em banco/produção: `[nao verificado]`. (`app/colaboradores/services/desligamento.py:24`; `app/folha/export.py:1207`; `app/escala/services/cadastro_realidade.py:169`)

O controle novo de afastado apenas cria outra pessoa com `situacao='afastado'`; não cobre essa transição nem uma ausência INSS real. (`app/folha/tests/test_lei_desligado_nao_trava.py:50`; `app/folha/tests/test_lei_desligado_nao_trava.py:79`)

**5. BLOQUEANTE para declarar PRONTO — faltam as provas nominais e de produção.**

O teste usa três dias da competência 10 e declara que col718/col270/col373 serão medidos por sonda no pouso. Não reproduz os RED nominais de 8/3/1 dias da competência 11. (`app/folha/tests/test_lei_desligado_nao_trava.py:8`; `app/folha/tests/test_lei_desligado_nao_trava.py:26`)

RED/GREEN nominal, suíte completa, opus, pouso e medição “comp11: desligados retidos = 0” não constam do dado fornecido. `[nao verificado]` [nao verificado]

Os recortes executados em memória passaram: C5 **9 testes** e matriz **20 testes**; não foi salvo registro de execução. Os módulos carregam código real por AST e declaram execução sem banco/escrita. (`app/chamados/tests/test_e4a2_c5_memoria.py:1`; `app/chamados/tests/test_e4a2_matriz_memoria.py:1`; registro persistido: `[nao verificado]`)

**6. NAO-BLOQ — o mutante do retentor tem efeito, mas o FAIL exigido não foi demonstrado.**

O teste chamado “mutante” força `esta_desligado=False` e **espera que os dias voltem**. Ele passa com esse comportamento; não reaplica a asserção original esperando reprovação. A asserção normal de vazio mataria essa mutação, mas sua execução sobre o mutante permanece `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:135`; `app/folha/tests/test_lei_desligado_nao_trava.py:67`)

Já o harness C5 realmente executa a verificação original, modifica a fonte em memória e exige `AssertionError`. (`app/chamados/tests/test_e4a2_c5_memoria.py:211`)

O selo AST do retentor exige apenas encontrar uma chamada chamada `esta_desligado`; aceita chamada sem influência no retorno, inclusive em ramo morto. O selo C5 possui tratamento explícito para esse caso. (`app/folha/tests/test_lei_desligado_nao_trava.py:157`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:285`)

**7. NAO-BLOQ — relógio interno e lavra antiga continuam exigindo atenção.**

O laço extraído lê `localdate()` quando não recebe `hoje`; `_dias_que_retem` não passa esse argumento. A C5 também mantém relógio interno opcional. Logo, não há leitura exclusivamente na borda. (`app/folha/export.py:1238`; `app/folha/fora_da_folha.py:98`; `app/escala/services/cadastro_realidade.py:161`)

`julgar_desligamento`, descrito como puro, lê `date.today()` duas vezes. É problema preexistente; ocorrência de divergência temporal em produção: `[nao verificado]`. (`app/colaboradores/services/desligamento.py:3`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/desligamento.py:52`)

Retratos antigos continuam devolvendo o JSON gravado, sem reaplicar a cura. O indicador de frescor compara com atualização de `FechamentoMensal`, sem considerar mudança de código ou situação. Relavratura no pouso: `[nao verificado]`. (`app/folha/retrato_folha.py:110`; `app/folha/retrato_folha.py:1298`)

**8. NAO-BLOQ — contraexemplos e limites do predicado.**

| Caso | Resultado sustentado pelo código |
|---|---|
| Desligado reativado | Reativação volta a ativo e apaga demissão. Furos históricos podem voltar ao retentor/C5. Isso decorre da unidade por situação atual; não há base para exigir isenção histórica após reativação. (`app/colaboradores/services/desligamento.py:227`; `app/folha/export.py:1207`; `app/escala/services/cadastro_realidade.py:166`) |
| Desligado com INSS já existente | Enquanto conservar `situacao='desligado'`, retorna vazio antes de examinar células/ausências. O lançamento posterior de INSS tem o risco do achado 4. (`app/folha/export.py:1207`; `app/ponto/views.py:2435`) |
| Dois vínculos/empresas | O estado é da pessoa; o juiz não recebe vínculo/empresa. O cadastro tem uma empresa, e a demissão fecha os vínculos ativos elegíveis. Caso produtivo e regra de desligamento parcial: `[nao verificado]`. (`app/colaboradores/models.py:321`; `app/colaboradores/services/desligamento.py:24`; `app/colaboradores/services/vinculo.py:863`) |
| Desligado sem data | O retentor retorna vazio; a C5 possui teste desse cadastro. Entretanto, o universo de fechamento exclui desligado sem data, podendo não produzir linha para o DP. (`app/folha/export.py:1207`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:126`; `app/ponto/services/fechamento.py:432`) |
| Desligamento agendado | Grava data/motivo, conserva situação e retorna. Portanto, ainda pode reter como ativo; isso está de acordo com a unidade literal por situação. (`app/colaboradores/services/desligamento.py:99`; `app/colaboradores/services/desligamento.py:103`; `app/colaboradores/services/desligamento.py:118`) |
| Dias após a saída | Com situação desligado, o retentor retorna vazio antes do laço. A trava agregada `turno_aberto` do achado 2 permanece; não oferece datas para determinar se veio de dia posterior à saída. (`app/folha/export.py:1207`; `app/folha/export.py:833`) |

Não encontrei juiz **registrado** concorrente para “está desligado?”. `afastado_hoje` responde outra pergunta; `sai_pela_rescisao` decide destino da competência por data. Há comparação crua duplicada no formatador de situação exibida, mas isso não comprova uma segunda autoridade registrada. (`app/core/juizes.py:263`; `app/core/juizes.py:267`; `app/folha/export.py:60`; `app/colaboradores/services/situacao_exibida.py:56`)

**9. NAO-BLOQ — mapa de escritores e leitores relevantes aos campos do diff.**

| Campo/valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `situacao` | Demissão e reativação; INSS; reversor de afastamento, pela porta. (`app/colaboradores/services/desligamento.py:121`; `:227`; `app/ponto/views.py:2435`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`) | Juiz novo; badge; universo de fechamento; guarda de vínculo. (`app/colaboradores/services/desligamento.py:24`; `app/colaboradores/services/situacao_exibida.py:56`; `app/ponto/services/fechamento.py:440`; `app/colaboradores/services/vinculo.py:91`) |
| Data de desligamento | O campo real é `data_demissao`; agendamento/demissão escrevem, reativação limpa. (`app/colaboradores/models.py:325`; `app/colaboradores/services/desligamento.py:103`; `:122`; `:228`) | Cron; destino rescisão; teto do laço; universo; Raio-X. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/folha/export.py:60`; `:1241`; `app/ponto/services/fechamento.py:432`; `app/folha/views.py:612`) |
| `o_que_segura` | Produz retorno em memória, incluindo o informativo. (`app/folha/fora_da_folha.py:235`) | Teste novo; o drawer produtivo lê o Retrato, não essa função. (`app/folha/tests/test_lei_desligado_nao_trava.py:100`; `app/colaboradores/views.py:984`) |
| `informativo_rescisao` | Produzido pelo laço compartilhado; transformado em chave de retorno. (`app/folha/export.py:1228`; `app/folha/fora_da_folha.py:233`) | `o_que_segura` e testes; integração ao DP faltante no achado 1. (`app/folha/fora_da_folha.py:234`; `app/folha/tests/test_lei_desligado_nao_trava.py:119`) |
| `retidos` | Há métricas diferentes: lista do gerador por `furo_espelho`; API por qualquer `fora`. (`app/folha/views.py:259`; `app/api/views_mensageria.py:301`) | Mensagem do gerador, trilha e resposta da API; não equivalem ao contador solicitado. (`app/folha/views.py:293`; `app/folha/views.py:304`; `app/api/views_mensageria.py:295`) |
| C5 `colabs_nunca_bateu` | Lê células e exclui pelo juiz; retorna mapa pessoa→datas. (`app/escala/services/cadastro_realidade.py:166`; `:169`; `:172`) | Formatador C5, contador do disparar e vigia de retido calado. (`app/escala/services/cadastro_realidade.py:179`; `app/chamados/management/commands/disparar_perguntas_competencia.py:56`; `app/folha/vigia_retido_calado.py:43`) |
| `barrados_desligado` | Conta pares únicos excluídos antes do filtro temporal. (`app/chamados/management/commands/disparar_perguntas_competencia.py:61`; `:72`) | Viaja no bloco informativo do carimbo; não entra como contador próprio em `censo/esperado`. (`app/chamados/management/commands/vigiar_fio_morto.py:233`; `:239`; `:242`) |

**10. NAO-BLOQ — balanço literal do aval, com as pendências acima.**

| Item | Cumprimento |
|---|---|
| Lei/unidade no retentor | **Cumprido no caminho central:** situação desligado retorna vazio, inclusive com `com_dias=True`. (`app/colaboradores/services/desligamento.py:24`; `app/folha/export.py:1207`) |
| `_dias_que_retem` e ramo do export iguais | **Cumprido por delegação:** ambos chegam a `motivos_retencao_celula`; o desligado sai antes de `_retem`. (`app/folha/fora_da_folha.py:98`; `app/folha/export.py:1307`; `app/folha/export.py:1207`) |
| Censo de leitores | **Incompleto**, pelas derivações do achado 3. (`app/folha/views.py:616`; `app/ponto/services/fechamento_lista.py:36`) |
| RED col718/270/373 | **Não demonstrado.** Fixture substituto comp10; sonda nominal `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:8`; `:26`) |
| Ativo com mesmo furo | **Há teste de controle.** Execução ORM `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:79`) |
| Afastado intocado | **Regra central preservada**, mas cobertura INSS é parcial e há o contraexemplo do achado 4. (`app/folha/export.py:1207`; `app/folha/tests/test_lei_desligado_nao_trava.py:50`; `app/ponto/views.py:2435`) |
| Contador no ESTADO | **Não cumprido: não existe `desligados_retidos` nesta cópia.** A métrica publicada é outra. (`app/chamados/management/commands/vigiar_fio_morto.py:233`; `bin/gerar_estado.py:260`) |
| Mutante → FAIL | **Parcial:** comportamento mutado demonstrado no teste; execução do FAIL do retentor `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:135`) |
| PROIBIDOS | Os novos caminhos leem células e retornam dados; não lançam falta. O selo de imutabilidade compara somente `CelulaDia`, sem selar Batida/Ausência. (`app/folha/export.py:1247`; `app/folha/export.py:1298`; `app/folha/tests/test_lei_desligado_nao_trava.py:128`) |
| Suíte + opus + pouso + comp11=0 | **Não demonstrado pelo material fornecido.** `[nao verificado]` | [nao verificado]

## 13/11 13:26 · sessao 21c2dc54bcdf · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — INSS pode apagar o estado que sustenta a isenção.** A tela só oferece ativos/afastados, mas o POST busca qualquer colaborador pelo `pk` e, se `tipo == 'afastamento_inss'`, grava `situacao='afastado'` sem preservar `desligado`: `app/ponto/views.py:2372`, `:2406`, `:2433`. A porta aceita essa escrita (`app/colaboradores/services/colaborador.py:32`, `:55`); portanto é **outro escritor semântico pela porta**, não escritor fora dela. Depois disso, `esta_desligado` responde falso (`app/colaboradores/services/desligamento.py:24`), o retentor volta ao laço (`app/folha/export.py:1207`) e a C5 volta a incluir os dias `nunca_bateu` (`app/escala/services/cadastro_realidade.py:169`). Cenário estático concreto: desligado com saída depois do fim da competência, furos antigos antes do INSS; lançar INSS posterior aos furos altera o estado e devolve os furos à retenção. Execução/banco/prod desse cenário: `[nao verificado]`. O teste só usa outro colaborador já marcado afastado, não a transição desligado→INSS (`app/folha/tests/test_lei_desligado_nao_trava.py:48`, `:79`).

2. **NAO-BLOQ para o novo predicado; problema temporal preexistente — relógio fora da borda.** `julgar_desligamento`, apresentado como puro, lê `date.today()` duas vezes (`app/colaboradores/services/desligamento.py:3`, `:32`, `:52`). A view só lê `timezone.localdate()` depois, ao chamar o executor (`app/colaboradores/views.py:1434`, `:1437`); o cron lê `hoje` local e não o passa ao juiz (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `:51`). Logo, a mesma operação pode usar datas distintas na borda e no julgamento; ocorrência real `[nao verificado]`. O laço extraído ainda lê relógio internamente (`app/folha/export.py:1238`) e a C5 também (`app/escala/services/cadastro_realidade.py:161`).

3. **NAO-BLOQ — não encontrei prova de juiz registrado duplicado.** O registro declara perguntas distintas para afastamento e desligamento (`app/core/juizes.py:263`, `:267`). `situacao_exibida.palavra` já repete a comparação crua para a precedência do badge (`app/colaboradores/services/situacao_exibida.py:56`), mas não achei ali uma autoridade registrada concorrente para a pergunta “desligado?”. `sai_pela_rescisao` pergunta sobre destino da competência e usa data, não situação (`app/folha/export.py:54`, `:60`); não equivale automaticamente ao novo juiz.

4. **NAO-BLOQ, contraexemplo com efeito histórico — reativação.** Reativar volta a `ativo`, apaga `data_demissao` e motivo e salva pela porta (`app/colaboradores/services/desligamento.py:220`, `:227`, `:231`). Não reativa escala automaticamente (`:223`). A partir daí, os furos históricos continuam na C5 (`app/escala/services/cadastro_realidade.py:166`) e podem voltar ao retentor (`app/folha/export.py:1207`); não há recorte histórico de períodos desligados nesse predicado (`app/colaboradores/services/desligamento.py:24`). A unidade literal “colaborador com situação desligado” pode autorizar esse efeito; chamar isso de violação da lei exigiria esclarecimento `[nao verificado]`. Os novos testes não cobrem a sequência de reativação `[nao verificado]`.

5. **NAO-BLOQ — aviso agendado e dia da saída.** Agendar grava data/motivo e retorna sem mudar situação (`app/colaboradores/services/desligamento.py:99`, `:102`, `:118`); a efetivação grava `desligado` (`:121`). O cron está configurado para 06:06 (`app/config/crons.py:440`). Antes da efetivação, inclusive no dia com data de saída vencida, o novo juiz ainda responde falso se a situação continua ativa (`app/colaboradores/services/desligamento.py:24`; seleção do cron em `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`). Isso segue a escolha explícita por estado atual, enquanto rescisão e teto do laço seguem data (`app/folha/export.py:60`, `:1241`). Estado real do cron/produção `[nao verificado]`.

6. **NAO-BLOQ — sem data.** O campo real é `data_demissao`; `situacao` e a data são campos independentes (`app/colaboradores/models.py:323`, `:325`). `esta_desligado` não exige data (`app/colaboradores/services/desligamento.py:24`), então o retentor devolve vazio para desligado sem data (`app/folha/export.py:1207`). Entretanto, o universo de fechamento exclui desligado sem data (`app/ponto/services/fechamento.py:432`, `:439`), podendo impedir que ele tenha linha nos leitores de fechamento. O novo teste C5 cobre falta de data (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:126`); cobertura equivalente de exportação/DP para cadastro sem fechamento `[nao verificado]`.

7. **NAO-BLOQ — dois vínculos/empresas não têm isolamento no estado.** Há um `Colaborador.empresa`, um estado e CPF único (`app/colaboradores/models.py:319`, `:321`, `:323`); vínculos apontam para colaborador e posto (`app/escala/models.py:872`, `:873`). A demissão fecha todos os vínculos ativos do colaborador (`app/colaboradores/services/vinculo.py:855`, `:863`) e o predicado isenta a pessoa inteira (`app/colaboradores/services/desligamento.py:24`), sem empresa ou vínculo como argumento. C5 agrupa pelo cadastro atual da empresa (`app/escala/services/cadastro_realidade.py:166`). Caso real de vínculo simultâneo noutra empresa e regra esperada para ele `[nao verificado]`.

Mapa de escrita/leitura relevante:

- **`situacao`**: escreve demissão/reativação (`app/colaboradores/services/desligamento.py:121`, `:227`), INSS (`app/ponto/views.py:2435`) e reversor de afastamento (`app/ponto/management/commands/reverter_situacao_afastado.py:73`), todos pela porta. Lê novo juiz (`app/colaboradores/services/desligamento.py:24`), badge (`app/colaboradores/services/situacao_exibida.py:56`), universo fechamento (`app/ponto/services/fechamento.py:439`), guarda de vínculo (`app/colaboradores/services/vinculo.py:91`), guarda de reativação ainda crua (`app/colaboradores/views.py:1417`). Admin é `SoLeitura` (`app/colaboradores/admin.py:48`).
- **`data_demissao`**: escreve agendamento/demissão/reativação (`app/colaboradores/services/desligamento.py:103`, `:122`, `:228`). Lê cron de efetivação (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`), destino rescisão (`app/folha/export.py:60`), teto do laço (`:1241`), universo fechamento (`app/ponto/services/fechamento.py:432`), corte na tela folha (`app/folha/views.py:612`), cadastro do dia (`app/ponto/supra_juiz.py:154`, `:194`).
- **C5**: produz `colabs_nunca_bateu` com juiz (`app/escala/services/cadastro_realidade.py:169`); formata `_nunca_bateu` (`:176`); lê contador (`app/chamados/management/commands/disparar_perguntas_competencia.py:56`).
- **`barrados_desligado`**: produzido como quantidade de pares únicos excluídos (`app/chamados/management/commands/disparar_perguntas_competencia.py:58`, `:62`, `:72`); é contador de exclusão, não prova de desligados ainda retidos (`:61`).

## 13/11 13:26 · sessao dbab1381c92f · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — não existe o contador `desligados_retidos` pedido no aval.** A busca literal em toda a cópia não encontrou definição nem leitor; o ESTADO conecta `fio_estado`, e o carimbo publica contadores de fio/C5, sem esse contador. `bin/gerar_estado.py:260`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/chamados/management/commands/vigiar_fio_morto.py:239`. Ausência literal no conjunto da cópia: `[nao verificado]` por arquivo:linha.

2. **BLOQUEANTE para declarar PRONTO — golden e medição de produção ausentes.** O teste diz que col718/col270/col373 serão medidos por sonda no pouso; seu fixture é genérico, com três dias da competência 10, não os 8/3/1 dias da competência 11. `app/folha/tests/test_lei_desligado_nao_trava.py:8`; `app/folha/tests/test_lei_desligado_nao_trava.py:26`. Saídas RED/GREEN nominais, suite completa, opus, pouso e comp11=0: `[nao verificado]`.

3. **NAO-BLOQ — `barrados_desligado` não substitui `desligados_retidos`.** Conta pares desligados excluídos do comparador C5, antes da filtragem pelo piso temporal; não consulta retenção da folha. `app/chamados/management/commands/disparar_perguntas_competencia.py:60`; `app/chamados/management/commands/disparar_perguntas_competencia.py:72`. Viaja no bloco informativo inteiro `nunca_bateu_sem_adesao` do carimbo, enquanto `censo/esperado` levam apenas `numero/esperado` do comparador. `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/chamados/management/commands/vigiar_fio_morto.py:242`. O ESTADO compara chaves do `esperado` e mostra divergências, sem ler `barrados_desligado` como contador próprio. `bin/fio_estado.py:121`; `bin/fio_estado.py:191`.

4. **NAO-BLOQ — mutante do retentor é testemunha positiva, não execução do FAIL exigido.** O teste denominado mutante força `esta_desligado=False` e afirma que os dias voltaram; não executa a asserção original esperando reprovação. `app/folha/tests/test_lei_desligado_nao_trava.py:135`. A asserção normal de vazio mataria esse mutante se executada sobre ele; portanto não há base para afirmar que o mutante é semanticamente inofensivo. `app/folha/tests/test_lei_desligado_nao_trava.py:67`. Execução ORM dessa mutação: `[nao verificado]`.

5. **NAO-BLOQ — cobertura INSS/PROIBIDOS é parcial.** O controle afastado apenas cria `situacao='afastado'`, sem afastamento INSS materializado. `app/folha/tests/test_lei_desligado_nao_trava.py:50`; `app/folha/tests/test_lei_desligado_nao_trava.py:79`. O teste “nada escrito” compara apenas `CelulaDia`; não sela Batida, Ausencia ou lançamento de falta. `app/folha/tests/test_lei_desligado_nao_trava.py:128`.

6. **NAO-BLOQ — selo AST do retentor aceita chamada em ramo morto.** Examina todos os nós e exige apenas o nome `esta_desligado`, sem verificar ramo morto ou influência no resultado. `app/folha/tests/test_lei_desligado_nao_trava.py:157`. O selo C5 possui tratamento/testemunha explícitos para ramo morto. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:285`.

Verificação executada sem banco/rede/escrita: C5 memória **9 OK** e matriz memória **20 OK**, incluindo seus catálogos de mutantes; registros de execução não salvos: `[nao verificado]`. Os módulos declaram execução em memória e carregamento das funções reais por AST. `app/chamados/tests/test_e4a2_c5_memoria.py:1`; `app/chamados/tests/test_e4a2_matriz_memoria.py:1`. O harness C5 realmente reaplica a asserção e exige `AssertionError` após mutação. `app/chamados/tests/test_e4a2_c5_memoria.py:211`.

## 13/11 13:26 · sessao 9647e3caf3e2 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — ainda existe trava de aprovação para desligado.** `classificar_export` cria `turno_aberto` sem consultar `esta_desligado` e grava a resposta em `travas_aprovacao` (`app/folha/export.py:830-848`). O lote exclui o colaborador da aprovação (`app/ponto/views.py:1295-1312`), e a aprovação individual recusa explicitamente quem tem essa trava (`app/ponto/views.py:1505-1513`). A tela também usa essas travas para habilitar o botão (`app/ponto/views.py:1168-1171`). O carimbo automático do TXT mantém esse crivo (`app/folha/views.py:244-248`). **Precisão:** isso não impede necessariamente a linha no TXT: `montar_linhas_export` aceita fechamentos abertos e aprovados e seleciona pelo `status='entra'` (`app/folha/export.py:687`, `app/folha/export.py:714-726`).

2. **BLOQUEANTE — informativo não integrado à apresentação do DP.** A lista nova só é acrescentada ao retorno da ferramenta `o_que_segura` (`app/folha/fora_da_folha.py:232-238`). O escritor do Retrato continua gravando somente campos antigos, sem `informativo_rescisao` (`app/folha/retrato_folha.py:1177-1189`); a tela lê as linhas gravadas (`app/folha/views_retrato.py:190-196`). O drawer lê o mesmo Retrato e seus campos antigos (`app/colaboradores/views.py:977-983`). Acesso manual pela ferramenta existe; entrega efetiva do informativo ao DP por tela/PDF é **[nao verificado]**.

3. **BLOQUEANTE para o requisito literal de censo/juiz único — sobrevivem derivações paralelas.**
   - O painel de retenção pergunta quem está fora ao export (`app/folha/views.py:586-591`), mas reconstrói os dias via `furos_por_dia`, cortando pela `data_demissao` crua (`app/folha/views.py:605-619`), em vez de ler `_dias_que_retem`.
   - O preview apresenta como dias o tamanho da lista de motivos (`app/folha/views.py:398-402`).
   - Esses caminhos não recolocam por si o desligado no balde `furo_espelho`, pois dependem previamente do veredito do export (`app/folha/views.py:589-591`); entretanto não satisfazem “nenhum leitor deriva por conta própria”.

4. **NAO-BLOQ — leitores alinhados ao novo retentor.**
   - `_dias_que_retem` delega diretamente a `motivos_retencao_celula(com_dias=True)` (`app/folha/fora_da_folha.py:95-99`).
   - `dias_que_seguram` usa `classificar_export` e só busca dias dos itens `furo_espelho` (`app/folha/fora_da_folha.py:118-137`).
   - `fora_da_folha` conta os dias recebidos dessa autoridade (`app/folha/fora_da_folha.py:160-193`).
   - Prévia: `_cobre_a_retencao` consulta o retentor e usa suas datas (`app/folha/previa.py:169-173`).
   - Vigia de pausa: consulta o mesmo retentor (`app/folha/services/vigia_pausa.py:123-130`).
   - Vigia de fio: lê `dias_que_seguram` (`app/chamados/services/vigia_fio.py:583-587`).
   - Retidos do gerador: deriva a lista do resultado `status/motivo` do export (`app/folha/views.py:257-260`).
   - PDF de retidos: chama o mesmo `classificar_export` (`app/folha/views.py:858-859`).
   - Retrato novo: `montar_linhas` lê `fora_da_folha` e grava os campos resultantes (`app/folha/retrato_folha.py:1156-1189`); único escritor persistente nessa porta é `lavrar_retrato_folha --apply` (`app/folha/management/commands/lavrar_retrato_folha.py:65-76`).

5. **NAO-BLOQ — lavra antiga continua sendo lida sem reaplicar a cura.** O leitor de Retrato devolve as linhas JSON gravadas, sem consultar o estado atual do colaborador ou o novo retentor (`app/folha/retrato_folha.py:98-111`). A tela usa esse leitor (`app/folha/views_retrato.py:190-196`); o vigia de retidos calados também (`app/folha/vigia_retido_calado.py:45-55`). Portanto o pouso precisa incluir relavratura; ocorrência/execução dessa relavratura em produção é **[nao verificado]**. O indicador de frescor compara apenas o horário do Retrato com `FechamentoMensal.atualizado_em`, não com mudança de código ou de situação (`app/folha/retrato_folha.py:1295-1306`).

6. **BLOQUEANTE — contador `desligados_retidos` no ESTADO ausente.** A busca literal em toda a cópia não encontrou o identificador **[nao verificado por referência positiva: ausência]**. O gerador tem integração explícita para outros medidores, como fio morto (`bin/gerar_estado.py:253-261`), leitores do dia (`bin/gerar_estado.py:284-293`) e turno do par (`bin/gerar_estado.py:294-302`); não há medidor/integração equivalente para desligados no código pesquisado **[nao verificado por referência positiva: ausência]**.

## 13/11 12:59 · sessao c2c9fc70ece1 · HJ-090 · censo · origem=Code

**(A) Não há arquivo em comum nas listas fornecidas. Nenhuma das três listas contém migration de `ponto`.** Para citar os anexos colados, conto o primeiro caminho depois de cada marcador como linha 1.

| Obra | Interseção com os arquivos declarados da HJ-091 | Migration de `ponto` listada | Fonte fornecida |
|---|---|---|---|
| DESLIGADO | Vazia | Não | `arquivos_wt-desligado-112447.txt:1–14` |
| FERIADO | Vazia | Não | `arquivos_wt-feriado1609-110424.txt:1–2` |
| E3 | Vazia | Não | `arquivos_e3.txt:1–73` |

O painel efetivamente usado é `app/templates/ponto/partials/painel_ausencia.html`, renderizado por `painel_ausencia`. Seus includes são `_cabecalho_drawer.html` e `_btn_pdf.html`; este inclui `_icone_pdf.html`. Nenhum desses caminhos aparece nas três listas fornecidas. (`app/ponto/views.py:2079`; `app/templates/ponto/partials/painel_ausencia.html:24`, `:206`, `:244`; `app/templates/core/_btn_pdf.html:37`; anexos acima.)

`app/folha/export.py` aparece em DESLIGADO, mas o complemento declara expressamente que esse arquivo **não muda** na HJ-091. (`arquivos_wt-desligado-112447.txt:10`; `complemento_hj_091.txt:2`, numeração do texto fornecido.)

Assim, os anexos não demonstram conflito de arquivo ou de número de migration. O número da migration futura e mudanças omitidas das listas são **[nao verificado]**. Existe na cópia a migration `0077`, dependente de `0076`; isso não demonstra reserva do próximo número. (`app/ponto/migrations/0077_ausencia_minutos_help_text_grade.py:8`.)

**(B) Escritores de `Ausencia` que alcançam férias.**

“Derivada” abaixo designa a linha criada por `espelhar_ferias`, que grava `Ferias agendamento #<pk>` na descrição. O espelhador seleciona existentes por colaborador, tipo e janela, sem filtrar status ou conferir esse texto; qualquer dia encontrado é pulado. (`app/ponto/services/ausencia.py:1183`, `:1200`, `:1203`, `:1207`.)

`ABERTAS` significa `aguardando_documento` e `aguardando_decisao`. A máquina permite aprovação a partir de `aguardando_decisao`; `aguardando_documento` não tem transição direta para aprovada. (`app/ponto/services/ausencia.py:766`, `:768`.)

| Função | Escrita e alcance de férias derivada |
|---|---|
| `criar_ausencia` | Valida tipo pelo catálogo; cria e salva. Também encontra rejeitada do mesmo colaborador/tipo/data e chama `_reabrir`, sem conferir agendamento de origem. **Alcança derivada rejeitada por esse caminho.** (`app/ponto/services/ausencia.py:194`, `:254`, `:291`, `:385`, `:418`.) |
| `garantir_ausencia` | Delega ao criador com `se_existir='retornar'`; não acrescenta exclusão de férias. **Função genérica; seus chamadores de regularização/abandono fornecem `falta`.** (`app/ponto/services/ausencia.py:453`, `:461`; `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`.) |
| `editar_ausencia_registro` | Troca tipo, data, fim, dias, descrição não vazia e minutos; salva, invalida fechamento e rejulga estado quando muda o tipo. **Alcança derivada**, sem guarda de origem. (`app/ponto/services/ausencia.py:465`, `:474`, `:498`, `:509`, `:524`, `:528`.) |
| `rejulgar_status_pelo_tipo` | Só atua em estados abertos; calcula estado inicial, muda estado e eventualmente grava prazo. **Alcança férias abertas**; para férias, o estado inicial é `aguardando_decisao`, sem prazo documental. (`app/ponto/services/ausencia.py:534`, `:541`, `:546`, `:551`, `:794`.) |
| `mudar_estado` | Valida transição, grava status, limpa prazo quando cabível e salva os campos derivados. **Alcança derivada**, sem filtro de tipo/origem. (`app/ponto/services/ausencia.py:809`, `:818`, `:825`, `:836`.) |
| `decidir` | Aceita aprovada/rejeitada; chama `mudar_estado` e grava decisor, data, descrição e período quando fornecidos. **Alcança derivada.** (`app/ponto/services/ausencia.py:898`, `:905`, `:912`, `:914`, `:928`.) |
| `_validar_periodo` / `_aplicar_periodo` | Auxiliares de `decidir`: exigem ausência parcial e atribuem horas. **Não excluem férias por literal**, mas a escrita depende de `cobre_parte_do_dia`. (`app/ponto/services/ausencia.py:880`, `:888`, `:892`, `:909`, `:926`.) |
| `anexar_documento` | Grava documento; em `aguardando_documento`, muda para `aguardando_decisao`. **Alcança derivada**, sem filtro de tipo. (`app/ponto/services/ausencia.py:847`, `:858`, `:860`, `:874`.) |
| `reverter_rejeicao` | Exige rejeitada e motivo; delega a `_reabrir`. **Alcança derivada rejeitada.** (`app/ponto/services/ausencia.py:1072`, `:1098`, `:1101`, `:1103`.) |
| `_reabrir` | Pode gravar documento/descrição; muda ao estado inicial calculado pelo tipo/anexo e grava prazo quando necessário. **Alcança derivada**, sem conferir origem. (`app/ponto/services/ausencia.py:1107`, `:1120`, `:1131`, `:1141`, `:1142`, `:1149`.) |
| `escalonar_aguardando_documento` | Seleciona por estado e prazo, sem tipo; com `aplicar`, vencidas passam a `aguardando_decisao`. **Alcança férias que estejam nessa seleção.** (`app/ponto/services/ausencia.py:974`, `:993`, `:1002`.) |
| `espelhar_ferias` | Agendamento ativo: cria férias aprovadas nos dias sem linha existente. Inativo: apaga as férias selecionadas na janela. **Alcança derivadas e outras férias da mesma janela**, sem conferir origem. Não busca linhas deslocadas para fora dela. (`app/ponto/services/ausencia.py:1181`, `:1183`, `:1189`, `:1200`, `:1203`.) |
| `apagar_ausencias_de_ferias` | Apaga por colaborador, `tipo='ferias'` e data inicial dentro da janela; `exceto` preserva a janela indicada. **Alcança qualquer derivada selecionada**, sem origem. (`app/ponto/services/ausencia.py:1263`, `:1267`, `:1269`, `:1271`.) |
| `rejeitar_ausencias_de_ferias` | Faz `update` para rejeitada nas férias não rejeitadas da janela, com atualização do carimbo; não passa por `save`. **Alcança qualquer derivada selecionada.** (`app/ponto/services/ausencia.py:1274`, `:1282`.) |
| `apagar_ausencias_em_ensaio` | Exige banco de ensaio e apaga o queryset recebido. **Alcança derivada incluída nele.** (`app/ponto/services/ausencia.py:1287`, `:1289`, `:1291`.) |
| `rederivar_fim` | Salva novamente a ausência e recarrega. **Alcança derivada**, sem filtro próprio. (`app/ponto/services/ausencia.py:1294`, `:1297`.) |
| `Ausencia.save` | Carimba decisão; deriva dias pelo intervalo e fim pelos dias quando cabível. **Alcança derivada**, sem guarda de origem. (`app/ponto/models.py:868`, `:872`, `:894`, `:902`, `:908`.) |

O adaptador `_a4_decidir` das views delega a `decidir`; não acrescenta guarda para férias derivadas. (`app/ponto/views.py:82`, `:85`.)

**Portas HTTP de ausência.** As rotas de ponto recebem prefixo `/ponto/`; as da API, `/api/`. (`app/config/urls.py:29`, `:37`.)

| View / rota | Comportamento e alcance |
|---|---|
| `lancar_ausencia` — `ausencias/lancar/` | Tipo vem do POST; chama criador com `status='aprovada'`. **Cria férias sem agendamento e alcança reabertura pelo criador**; não seleciona derivada por ID. (`app/ponto/urls.py:57`; `app/ponto/views.py:2378`, `:2415`, `:2420`; `app/ponto/services/ausencia.py:254`, `:291`.) |
| `submeter_ausencia` — `ausencias/submeter/` | Valida catálogo/sobreposição e chama criador, sem status explícito nem agendamento. **Não exclui férias**; aplica os vetos e a reabertura do criador. (`app/ponto/urls.py:64`; `app/ponto/views.py:2509`, `:2525`, `:2536`.) |
| `editar_ausencia` — `ausencias/<aid>/editar/` | Busca só por PK e chama editor geral. **Edita derivada**, conservando férias ou trocando o tipo, sujeita às validações do editor. (`app/ponto/urls.py:76`; `app/ponto/views.py:2761`, `:2794`, `:2796`.) |
| `cancelar_ausencia` — `ausencias/<aid>/cancelar/` | Busca por PK; recusa apenas rejeitada; decide rejeitada e prefixa descrição. **Cancela derivada aprovada.** (`app/ponto/urls.py:75`; `app/ponto/views.py:2728`, `:2729`, `:2734`.) |
| `cancelar_ausencia_lote` — `ausencias/lote/cancelar/` | Para cada PK, exclui só rejeitada e decide rejeitada. **Cancela derivada aprovada.** (`app/ponto/urls.py:56`; `app/ponto/views.py:2345`, `:2351`.) |
| `rejeitar_ausencia` — `ausencias/<aid>/rejeitar/` | Seleciona ausência e raiz/irmãos documentais em estados abertos; rejeita cada um. **Alcança derivada aberta.** (`app/ponto/urls.py:74`; `app/ponto/views.py:2687`, `:2689`, `:2701`.) |
| `rejeitar_ausencia_lote` — `ausencias/lote/rejeitar/` | PK + estados abertos; rejeita e pode acrescentar motivo à descrição. **Alcança derivada aberta.** (`app/ponto/urls.py:55`; `app/ponto/views.py:2316`, `:2319`.) |
| `reverter_rejeicao_ausencia` — `ausencias/<aid>/reverter-rejeicao/` | Busca só por PK e chama reversor. **Reabre derivada rejeitada**, exigindo motivo. (`app/ponto/urls.py:68`; `app/ponto/views.py:2583`, `:2584`; `app/ponto/services/ausencia.py:1098`, `:1101`.) |
| `aprovar_ausencia` — `ausencias/<aid>/aprovar/` | Seleciona aberta e raiz/irmãos abertos; passa pelo gate e decide aprovação. **Não exclui derivada**; gate e máquina de estados limitam a aprovação. (`app/ponto/urls.py:65`; `app/ponto/views.py:2606`, `:2608`, `:2616`, `:2643`.) |
| `aprovar_ausencia_lote` — `ausencias/lote/aprovar/` | PK + estados abertos, gate e decisão. **Não exclui derivada**, com as mesmas limitações de aprovação. (`app/ponto/urls.py:54`; `app/ponto/views.py:2282`, `:2284`, `:2290`.) |
| `editar_periodo_ausencia` — `ausencias/<aid>/periodo/` | Busca aprovada por PK e envia horas ao decisor. **Sem exclusão literal de férias**; exige tipo parcial no catálogo vigente, dia único e minutos entre zero e o dia inteiro. O catálogo vigente de produção é **[nao verificado]**. (`app/ponto/urls.py:66`; `app/ponto/views.py:2670`, `:2673`; `app/ponto/services/ausencia.py:888`; `app/ponto/turnos.py:2600`.) |
| `anexar_documento_ausencia` — `ausencias/<aid>/anexar-documento/` | Busca por PK sem filtro de tipo/status e chama anexador. **Alcança derivada.** (`app/ponto/urls.py:71`; `app/ponto/views.py:2140`, `:2151`.) |
| `api_ausencias` — `ausencias/` | POST fornece tipo; criador recebe `devolver_mesmo_fato`. Reenvio pode anexar documento na existente. **Cria férias, reabre rejeitada e anexa na derivada existente**, conforme esses caminhos. (`app/api/urls.py:82`; `app/api/views.py:1705`, `:1805`, `:1811`, `:1833`, `:1838`; `app/ponto/services/ausencia.py:254`, `:291`.) |
| `api_ausencia_documento` — `ausencias/<aid>/documento/` | Busca PK + colaborador, exige estado aberto e chama anexador. **Alcança derivada aberta do próprio colaborador.** (`app/api/urls.py:83`; `app/api/views.py:3069`, `:3078`, `:3108`.) |
| `veredito_celula_view` — `celula/veredito/` | Encaminha `tipo_ausencia` recebido ao serviço `veredito_celula`; no ramo justificado ele cria ausência aprovada sem veto próprio a férias. **Cria férias sem agendamento; a reabertura segue o criador.** (`app/ponto/urls.py:58`; `app/ponto/views.py:3312`, `:3340`; `app/ponto/services/veredito_celula.py:211`, `:333`, `:337`.) |

O veto a férias de `AusenciaForm.clean_tipo` não protege lançar/submeter/editar: essas views chamam diretamente os serviços, que validam pelo catálogo. (`app/ponto/forms.py:65`; `app/ponto/views.py:2415`, `:2536`, `:2794`; `app/ponto/services/ausencia.py:194`, `:474`.)

Há ainda o escritor genérico `lancar_ausencia_porta`, chamado por `_aplicar_ausencia` através de `ia_perguntar → _aplicar_proposta`, em `/ia/perguntar/`. O escritor recebe tipo e cria aprovada; **o fluxo normal de prévia recusa férias antes de guardar a proposta**. (`app/ponto/services/checar_ausencia.py:43`, `:46`, `:10`, `:21`; `app/core/views_ia.py:931`, `:935`, `:985`, `:153`, `:874`; `app/core/urls.py:15`.)

**Escrita pelo agendamento e respectivas portas.**

`AgendamentoFerias.save → _sync_ausencia → espelhar_ferias` executa em save completo ou alteração de status/datas/período aquisitivo. `AgendamentoFerias.delete` chama o apagador de férias da janela inteira, sem conferir `#N`. **Ambos alcançam as derivadas selecionadas.** (`app/ferias/models.py:386`, `:392`, `:444`, `:446`, `:450`, `:453`, `:455`.)

As rotas desta tabela têm prefixo `/ferias/`. (`app/config/urls.py:32`.)

| Escritor | Efeito nas ausências | View/porta que chega nele |
|---|---|---|
| `registrar_gozo_historico` | Cria agendamento e salva, disparando espelho. (`app/ferias/services.py:270`, `:278`, `:298`.) | `criar_periodo_aquisitivo → registrar_gozo_historico`. (`app/ferias/views.py:124`, `:211`; `app/ferias/urls.py:15`.) |
| `executar_agendamento` | Cria agendamento, disparando espelho. (`app/ferias/services.py:306`, `:321`.) | `agendar_ferias` e `agendar_lote`, pelo wrapper `_executar_agendamento`. (`app/ferias/views.py:299`, `:636`, `:260`, `:264`; `app/ferias/urls.py:16`, `:10`.) |
| `agendar_ferias_porta` | Delega a `executar_agendamento`. (`app/ferias/services.py:128`, `:157`.) | `ia_perguntar → _aplicar_proposta → _aplicar_ferias → escritor registrado`. (`app/core/views_ia.py:874`, `:145`, `:668`, `:684`; `app/core/intake.py:34`; `app/core/urls.py:15`.) |
| `cancelar_agendamento_dp` | Salva cancelado; o espelho apaga férias da janela. (`app/ferias/services.py:372`, `:384`, `:385`; `app/ponto/services/ausencia.py:1189`.) | `cancelar_agendamento`. (`app/ferias/views.py:322`, `:330`; `app/ferias/urls.py:18`.) |
| `executar_cancelamento_admin` | Primeiro rejeita férias da janela; depois salva cancelado e o espelho as apaga. (`app/ferias/services.py:390`, `:411`, `:417`.) | `cancelar_agendamento_admin`, `corrigir_concluida_admin` e `cancelar_admin_massa`, pelo wrapper `_executar_cancelamento_admin`; correção de concluída passa `permitir_concluido=True`. (`app/ferias/views.py:370`, `:418`, `:575`, `:527`, `:531`; `app/ferias/urls.py:19`, `:20`, `:9`.) |
| `_trocar_janela` | Apaga trecho antigo fora da janela nova; salva datas/status e espelha a nova janela. (`app/ferias/services.py:422`, `:445`, `:450`.) | `editar_datas_admin → corrigir_datas_agendamento → _trocar_janela`; `editar_agendamento → remarcar_agendamento → _trocar_janela`. (`app/ferias/views.py:503`, `:688`; `app/ferias/services.py:457`, `:459`, `:462`, `:466`; `app/ferias/urls.py:21`, `:22`.) |
| `aprovar_solicitacao` | Havendo ciclo e saldo, cria/salva agendamento, disparando espelho. (`app/ferias/services.py:469`, `:483`, `:489`, `:493`, `:503`.) | `avaliar_solicitacao`, ação aprovar. (`app/ferias/views.py:733`, `:750`, `:753`; `app/ferias/urls.py:12`.) |
| `alinhar_status_por_data` | Relê sob trava; mudança de status de não cancelado dispara espelho. (`app/ferias/services.py:588`, `:600`, `:602`, `:604`.) | `sincronizar_status_ferias.Command.handle`, somente com `--apply`. (`app/ferias/management/commands/sincronizar_status_ferias.py:24`, `:44`, `:49`.) |

Pedir confirmação e registrar resposta do colaborador salvam somente campos de confirmação, que não disparam esse espelho. (`app/ferias/services.py:553`, `:571`, `:575`; `app/ferias/models.py:444`, `:450`.)

**Demais comandos, função genérica e escrita auxiliar.**

| Entrada/função | Comportamento e alcance |
|---|---|
| `curar_ausencia_sem_fim.Command.handle` | Seleciona não rejeitadas com dias > 1, fim nulo e tipo fora dos abertos; `--apply` chama `rederivar_fim`. **Não exclui férias derivadas.** (`app/ponto/management/commands/curar_ausencia_sem_fim.py:26`, `:29`, `:38`, `:46`.) |
| `rejulgar_status_por_tipo.Command.handle` | Seleciona `aguardando_documento`, sem tipo; `--apply` chama rejulgador. **Alcança derivadas nesse estado.** (`app/ponto/management/commands/rejulgar_status_por_tipo.py:19`, `:23`, `:30`.) |
| `escalonar_documentos_ausencia.Command.handle` | Encaminha `--apply` ao escalonador geral. **Alcance dado pela seleção de estado/prazo, sem exclusão de férias.** (`app/ponto/management/commands/escalonar_documentos_ausencia.py:24`, `:26`; `app/ponto/services/ausencia.py:993`.) |
| `zerar_dados_operacionais.Command.handle` | Com confirmação, envia todas as ausências ao apagador de ensaio. **Inclui derivadas em banco de ensaio.** (`app/ponto/management/commands/zerar_dados_operacionais.py:71`, `:89`; `app/ponto/services/ausencia.py:1289`.) |
| `rederivar_minutos_h06_comp11.tratar_linha` | Busca PK, confere identidade e chama editor geral, sem veto próprio de tipo. **Função genérica; o plano atual do comando contém somente atestado/falta.** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:126`, `:138`, `:142`, `:169`, `:50`, `:244`, `:265`.) |
| `_materializar_ausencia_de_resposta` | Recebe tipo/status e chama criador sem veto próprio a férias. **O fluxo atual fornece somente falta/atestado**, pelo mapa de escapes. (`app/chamados/services/materializacao.py:502`, `:527`, `:603`; `app/chamados/services/disputa_emissao.py:1538`.) |
| Portas desse materializador | `responder_pergunta_disputa` e `api_disputa_responder` fornecem resposta ao core; o mapa acima restringe os tipos produzidos. (`app/chamados/views.py:1228`, `:1283`; `app/chamados/urls.py:17`; `app/api/views.py:2577`, `:2620`; `app/api/urls.py:93`.) |
| Código de módulo `sombra_mascara_pessoal.py` | Exige banco `sombra`; atualiza documento e descrição sem filtro de tipo. **Alcança derivadas na sombra**, fora das portas da aplicação. (`bin/sombra_mascara_pessoal.py:10`, `:22`, `:44`, `:62`; `bin/sombra_regras_pessoais.py:64`, `:109`.) |

Também existem escritores históricos nas migrations de dados, sem filtro que exclua férias:

- `0057._para_frente` transforma `pendente` em estado documental/decisório; `_para_tras` retorna os estados abertos para `pendente`. **Alcançam férias selecionadas.** (`app/ponto/migrations/0057_a4_backfill_pendente.py:21`, `:35`, `:43`, `:58`, `:63`.)
- `0058._para_frente` e `_para_tras` alteram prazo das ausências em `aguardando_documento`. **Alcançam férias selecionadas.** (`app/ponto/migrations/0058_a4_relogio_recomeca.py:29`, `:37`, `:42`, `:57`, `:59`, `:63`.)
- `0059._mover`, chamado por `_para_frente`/`_para_tras`, altera prazo por estado, sem tipo. **Alcança férias selecionadas.** (`app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:26`, `:32`, `:36`, `:48`, `:53`.)

O Django admin de `Ausencia` está em somente leitura: adicionar, alterar e apagar retornam falso. (`app/ponto/admin.py:38`; `app/core/admin.py:59`, `:62`, `:65`.)

**Leitores que casam férias com agendamento pelo `#N`: nenhum localizado; inexistência universal [nao verificado].** Os comportamentos concretos encontrados são:

| Leitor | O que efetivamente usa |
|---|---|
| `espelhar_ferias` | Colaborador + tipo férias + janela; pula dia existente. O `#N` é **escrito**, sem ser usado para casar a existente. (`app/ponto/services/ausencia.py:1183`, `:1200`, `:1207`.) |
| `AgendamentoFerias._guard_sobreposicao` | Trata como `_minhas` todas as férias do colaborador na janela antiga; não compara descrição. (`app/ferias/models.py:427`, `:430`, `:435`.) |
| `painel_ausencia` / template | Agrupa por `documento_referencia`; mostra intervalo/data da ausência e descrição como texto. (`app/ponto/views.py:2073`, `:2076`, `:2079`; `app/templates/ponto/partials/painel_ausencia.html:50`, `:57`.) |
| `dias_ferias_com_batida` | Lê agendamentos por `ferias_na_janela` e cruza suas datas com batidas; não casa descrição de ausência. (`app/folha/export.py:484`, `:491`, `:499`.) |

Ocorrências, contagens e situação atual de produção além do texto fornecido permanecem **[nao verificado]**.

## 13/11 12:57 · sessao 0858a2d44305 · - · - · origem=Ronald (tmux)

As duas portas que fornecem resposta arbitrária ao ramo escape são:

- `responder_pergunta_disputa`: recebe `resposta` do POST e chama o core. `app/chamados/views.py:1228`, `:1265`, `:1283`; rota `app/chamados/urls.py:17`.
- `api_disputa_responder`: recebe `resposta` de `request.data` e chama o core. `app/api/views.py:2577`, `:2614`, `:2620`; rota `app/api/urls.py:93`.

O core consulta `ESCAPES_QUE_VIRAM_AUSENCIA` e chama `_materializar_ausencia_de_resposta`; o mapa atual contém somente falta e atestado. `app/chamados/services/disputa_emissao.py:1538`; `app/chamados/services/materializacao.py:603`.

Outros caminhos chegam ao core, mas fornecem apenas hora e não alcançam esse ramo:

- `aplicar_declaracao`: passa `p['hora']`, produzida pelo parser de horas. `app/chamados/services/declaracao_texto.py:28`, `:35`, `:624`, `:635`, `:693`. HTTP `detalhe_chamado_local` com ação `aplicar_declaracao_texto`: `app/chamados/views.py:359`, `:478`; rota `app/chamados/urls.py:14`.
- `acao_corrigir_resposta`: exige regex `HH:MM` e pergunta de hora antes de chamar o core. `app/chamados/services/acoes_disputa.py:302`, `:336`, `:349`, `:371`. Entradas: `detalhe_chamado_local` → `:536`; `fio_colaborador` → `app/chamados/views.py:674`; `modal_fio_colaborador` → `:954`; dispatcher `app/chamados/services/fio.py:485`, `:639`. Rotas `app/chamados/urls.py:14`, `:21`, `:39`.
- `declarar_hora_colab`: chama core com `hora`. `app/chamados/services/declaracao_texto.py:306`, `:419`; entrada `responder_chamado_colaborador`: `app/chamados/views.py:1041`, `:1089`.

`declarar_nao_trabalhou` não chama o core nem cria Ausência: atualiza contexto e anexa nota. `app/chamados/services/declaracao_texto.py:189`, `:208`, `:211`.

## 13/11 12:55 · sessao 0858a2d44305 · - · - · origem=Ronald (tmux)

Censo externo ao produtor e às views centrais:

| Função/porta | Comportamento e alcance |
|---|---|
| `api_ausencias` | Lê `tipo` do POST e chama `criar_ausencia`, sem exclusão de férias; pode criar ou reabrir a linha pelo produtor. Reenvio pode anexar documento na existente. `app/api/views.py:1671`, `:1705`, `:1805`, `:1833`. Rota `ausencias/`: `app/api/urls.py:82`. |
| `api_ausencia_documento` | Busca por PK+colaborador, sem filtro de tipo; exige estado aberto e chama `anexar_documento`. Alcança férias abertas. `app/api/views.py:3052`, `:3069`, `:3078`, `:3107`. Rota: `app/api/urls.py:83`. |
| `veredito_celula` | Para justificado usa `tipo_ausencia` recebido e cria aprovada, sem veto próprio a férias. `app/ponto/services/veredito_celula.py:211`, `:333`, `:337`. HTTP `veredito_celula_view` aceita o tipo do POST: `app/ponto/views.py:3312`, `:3340`; rota: `app/ponto/urls.py:58`. |
| `lancar_ausencia_porta` | Recebe qualquer tipo e cria aprovada; não veta férias. `app/ponto/services/checar_ausencia.py:43`. Chamada `_aplicar_ausencia`: `app/core/views_ia.py:971`, `:985`; despacho `:153`; HTTP `ia_perguntar`: `:859`, `:874`; rota `app/core/urls.py:15`. **O preview normal veta férias expressamente**: `app/ponto/services/checar_ausencia.py:10`, `:21`. |
| `Ausencia.save` | Sem veto à origem derivada: carimba decisão; deriva dias do range e fim a partir dos dias quando cabível. `app/ponto/models.py:868`, `:872`, `:894`, `:902`. |
| `curar_ausencia_sem_fim.Command.handle` | Seleciona não rejeitadas, dias>1, fim nulo e tipo fora de `EM_ABERTO`, sem excluir férias; `--apply` chama `rederivar_fim`. `app/ponto/management/commands/curar_ausencia_sem_fim.py:26`, `:29`, `:38`, `:46`. |
| `rejulgar_status_por_tipo.Command.handle` | Seleciona `aguardando_documento`, sem filtro de tipo; `--apply` chama `rejulgar_status_pelo_tipo`. Alcança férias nesse estado. `app/ponto/management/commands/rejulgar_status_por_tipo.py:19`, `:23`, `:30`. |
| `escalonar_documentos_ausencia.Command.handle` | Encaminha `--apply` ao escalonador geral, sem filtro próprio de tipo. `app/ponto/management/commands/escalonar_documentos_ausencia.py:24`. |
| `zerar_dados_operacionais.Command.handle` | Exige banco de ensaio; com `--confirmar`, manda `Ausencia.objects.all()` para exclusão. Alcança férias em ensaio. `app/ponto/management/commands/zerar_dados_operacionais.py:24`, `:27`, `:71`, `:89`. |
| `rederivar_minutos_h06_comp11.tratar_linha` | Busca por PK, compara identidade e usa editor geral sem veto próprio a férias. Porém o plano atual do comando usa lista fechada exclusivamente de atestado/falta. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:126`, `:138`, `:142`, `:169`, `:50`, `:244`, `:265`. |
| `_materializar_ausencia_de_resposta` | Função recebe tipo sem veto e cria pelo produtor; chamador runtime usa mapa somente falta/atestado, portanto o caminho atual não fornece férias. `app/chamados/services/materializacao.py:502`, `:527`, `:603`; chamador: `app/chamados/services/disputa_emissao.py:1425`, `:1538`. |

Exclusões concretas:

- `_declarar_ausencia_de_pergunta` recebe tipo de mapa exclusivamente falta/atestado: `app/chamados/services/materializacao.py:468`, `:475`; `app/chamados/models.py:3717`.
- Regularização e abandono provisório fixam `tipo='falta'`: `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`.
- `AusenciaAdmin` herda `SoLeitura`; add/change/delete retornam falso: `app/ponto/admin.py:38`; `app/core/admin.py:59`.
- `trocar_plantao` lê/trava Ausência e altera a célula pelo `editar_dia`, sem escrever Ausência: `app/ponto/portas/celula.py:670`, `:695`, `:713`.
- `decididos_do_periodo` e `linha_do_tempo` apenas leem e montam cartões: `app/ponto/services/dia_decidido.py:529`, `:550`, `:561`, `:566`, `:587`, `:599`.

Script auxiliar que também escreve:

- Código de módulo de `bin/sombra_mascara_pessoal.py` exige banco `sombra`, usa schema `juliani`, atualiza `documento` e faz `bulk_update` de `descricao`, sem filtro tipo; portanto alcança férias na sombra. `bin/sombra_mascara_pessoal.py:10`, `:16`, `:22`, `:44`, `:62`; inventário: `bin/sombra_regras_pessoais.py:64`, `:109`. Chamadas shell: `bin/sombra.sh:204`, `:272`.

## 13/11 12:55 · sessao 7c7524a0578f · - · - · origem=Ronald (tmux)

Censo das portas de `ponto`, sem alteração de arquivos. Os caminhos abaixo têm prefixo `/ponto/` em `app/config/urls.py:29`.

| Porta / função | Filtro e escrita | Alcança férias derivadas |
|---|---|---|
| `ausencias/lote/aprovar/` → `aprovar_ausencia_lote` | Obtém por PK e `status__in=ABERTAS`, sem tipo; passa pelo gate e chama `_a4_decidir(..., 'aprovada')`. `app/ponto/urls.py:54`; `app/ponto/views.py:2282`; `app/ponto/views.py:2290` | Sim, se estiver em estado aberto e passar no gate. `app/ponto/views.py:2282`; `app/ponto/views.py:2284` |
| `ausencias/lote/rejeitar/` → `rejeitar_ausencia_lote` | PK + estados abertos, sem tipo; rejeita e acrescenta motivo à descrição. `app/ponto/urls.py:55`; `app/ponto/views.py:2316`; `app/ponto/views.py:2319` | Sim, em estado aberto. `app/ponto/views.py:2316` |
| `ausencias/lote/cancelar/` → `cancelar_ausencia_lote` | PK; exclui somente `rejeitada`; decide `rejeitada` e prefixa descrição. `app/ponto/urls.py:56`; `app/ponto/views.py:2345`; `app/ponto/views.py:2351` | Sim, inclusive aprovada. `app/ponto/views.py:2345` |
| `ausencias/lancar/` → `lancar_ausencia` | Lê tipo POST e chama `criar_ausencia` com status aprovado. `app/ponto/urls.py:57`; `app/ponto/views.py:2378`; `app/ponto/views.py:2415`; `app/ponto/views.py:2420` | Pode criar `ferias` avulsa; não seleciona uma derivada existente por ID. A chamada fornece colaborador/tipo/dia, sem agendamento. `app/ponto/views.py:2415` |
| `ausencias/submeter/` → `submeter_ausencia` | Lê tipo POST, valida catálogo e chama `criar_ausencia`, sem status explícito. `app/ponto/urls.py:64`; `app/ponto/views.py:2476`; `app/ponto/views.py:2509`; `app/ponto/views.py:2536` | Pode criar `ferias` avulsa, sujeita aos vetos de criação; não recebe agendamento. `app/ponto/views.py:2525`; `app/ponto/views.py:2536` |
| `ausencias/<aid>/aprovar/` → `aprovar_ausencia` | PK + estados abertos; seleciona raiz/irmãos documentais abertos e aprova cada um. Sem tipo. `app/ponto/urls.py:65`; `app/ponto/views.py:2606`; `app/ponto/views.py:2616`; `app/ponto/views.py:2643` | Sim, em estado aberto, passando no gate. `app/ponto/views.py:2606`; `app/ponto/views.py:2608` |
| `ausencias/<aid>/periodo/` → `editar_periodo_ausencia` | PK + aprovada; passa horas para `decidir`. `app/ponto/urls.py:66`; `app/ponto/views.py:2670`; `app/ponto/views.py:2673` | Não há exclusão literal de férias; exige que `cobre_parte_do_dia` seja verdadeiro. Esse juiz exige tipo parcial no catálogo vivo, dia único e `0 < minutos < 480`. `app/ponto/services/ausencia.py:888`; `app/ponto/turnos.py:2600` |
| `ausencias/<aid>/reverter-rejeicao/` → `reverter_rejeicao_ausencia` | Obtém somente por PK e chama `reverter_rejeicao`, sem tipo. `app/ponto/urls.py:68`; `app/ponto/views.py:2583`; `app/ponto/views.py:2584` | Sim, se o serviço aceitar o estado/motivo. `app/ponto/views.py:2584` |
| `ausencias/<aid>/anexar-documento/` → `anexar_documento_ausencia` | Obtém por PK, sem tipo/status; chama `anexar_documento`. `app/ponto/urls.py:71`; `app/ponto/views.py:2140`; `app/ponto/views.py:2151` | Sim. `app/ponto/views.py:2140` |
| `ausencias/<aid>/rejeitar/` → `rejeitar_ausencia` | PK + estados abertos; raiz/irmãos documentais abertos; rejeita cada um. Sem tipo. `app/ponto/urls.py:74`; `app/ponto/views.py:2687`; `app/ponto/views.py:2689`; `app/ponto/views.py:2701` | Sim, em estado aberto. `app/ponto/views.py:2687` |
| `ausencias/<aid>/cancelar/` → `cancelar_ausencia` | Obtém por PK; recusa somente rejeitada; decide rejeitada e prefixa descrição. `app/ponto/urls.py:75`; `app/ponto/views.py:2728`; `app/ponto/views.py:2729`; `app/ponto/views.py:2734` | Sim, inclusive aprovada. `app/ponto/views.py:2728` |
| `ausencias/<aid>/editar/` → `editar_ausencia` | Obtém por PK, sem tipo/status; passa tipo, data, dias, descrição, indeterminado e minutos ao serviço. `app/ponto/urls.py:76`; `app/ponto/views.py:2761`; `app/ponto/views.py:2794` | Sim, incluindo conservar tipo férias ou trocá-lo. `app/ponto/views.py:2796`; `app/ponto/services/ausencia.py:474` |

`ABERTAS` contém `aguardando_documento` e `aguardando_decisao`; o adaptador `_a4_decidir` chama `decidir`. `app/ponto/services/ausencia.py:766`; `app/ponto/views.py:82`.

**Atenção ao formulário sem consumidor:** `AusenciaForm.clean_tipo` veda férias, mas as três views acima chamam os serviços diretamente; esses serviços validam o tipo pelo catálogo, sem consultar `TIPOS_VEDADOS`. `app/ponto/forms.py:65`; `app/ponto/views.py:2415`; `app/ponto/views.py:2536`; `app/ponto/views.py:2794`; `app/ponto/services/ausencia.py:194`; `app/ponto/services/ausencia.py:474`. A anotação existente também registra “AusenciaForm sem consumidor”. `app/docs/RELATO-ARQUIVO.md:17493`.

O painel efetivamente renderizado é `app/templates/ponto/partials/painel_ausencia.html`. `app/ponto/views.py:2079`. Ele inclui `colaboradores/partials/_cabecalho_drawer.html` e `core/_btn_pdf.html`. `app/templates/ponto/partials/painel_ausencia.html:24`; `app/templates/ponto/partials/painel_ausencia.html:206`; `app/templates/ponto/partials/painel_ausencia.html:244`. Exibe edição quando `pode_editar`, reversão quando rejeitada e aprovação/rejeição quando aberta, sem guarda de tipo férias nesses trechos. `app/templates/ponto/partials/painel_ausencia.html:131`; `app/templates/ponto/partials/painel_ausencia.html:279`; `app/templates/ponto/partials/painel_ausencia.html:306`.

A listagem de migrations termina em `0077_ausencia_minutos_help_text_grade.py` [nao verificado por arquivo:linha para a ausência de posteriores]. Essa migration depende de `0076` e altera `Ausencia.minutos`. `app/ponto/migrations/0077_ausencia_minutos_help_text_grade.py:8`; `app/ponto/migrations/0077_ausencia_minutos_help_text_grade.py:12`.

## 13/11 12:54 · sessao a65599f8d14d · - · - · origem=Ronald (tmux)

Censo de `ferias` e leitores de `#N`, somente por leitura estática:

| Função | Efeito em Ausencia de férias | Evidência |
|---|---|---|
| `AgendamentoFerias._sync_ausencia` | Chama `espelhar_ferias(self)`. | `app/ferias/models.py:386`, `app/ferias/models.py:391` |
| `AgendamentoFerias.save` | Espelha após salvar se `update_fields` é `None` ou inclui status, início, fim ou período aquisitivo. | `app/ferias/models.py:444`, `app/ferias/models.py:446`, `app/ferias/models.py:450` |
| `AgendamentoFerias.delete` | Apaga ausências tipo férias do colaborador na janela inteira do objeto; não seleciona origem por `#N`. | `app/ferias/models.py:453`, `app/ferias/models.py:455` |
| `agendar_ferias_porta` | Delega criação para `executar_agendamento`, que cria agendamento e dispara `save`/espelho. | `app/ferias/services.py:128`, `app/ferias/services.py:157`, `app/ferias/services.py:321` |
| `registrar_gozo_historico` | Cria agendamento histórico e faz `ag.save()`, disparando espelho. | `app/ferias/services.py:270`, `app/ferias/services.py:278`, `app/ferias/services.py:298` |
| `executar_agendamento` | Cria agendamento por `objects.create`. | `app/ferias/services.py:306`, `app/ferias/services.py:321` |
| `cancelar_agendamento_dp` | Salva status cancelado; o espelho apaga férias da janela. | `app/ferias/services.py:372`, `app/ferias/services.py:384`, `app/ponto/services/ausencia.py:1189` |
| `executar_cancelamento_admin` | Primeiro rejeita férias da janela, depois salva agendamento cancelado e o espelho as apaga. | `app/ferias/services.py:390`, `app/ferias/services.py:411`, `app/ferias/services.py:416`, `app/ponto/services/ausencia.py:1189` |
| `_trocar_janela` | Apaga férias da janela antiga fora da nova; salva datas/status e dispara espelho na nova janela. | `app/ferias/services.py:422`, `app/ferias/services.py:445`, `app/ferias/services.py:450` |
| `corrigir_datas_agendamento` / `remarcar_agendamento` | Ambas delegam a `_trocar_janela`. | `app/ferias/services.py:457`, `app/ferias/services.py:459`, `app/ferias/services.py:462`, `app/ferias/services.py:466` |
| `aprovar_solicitacao` | Havendo PA e saldo, cria agendamento e faz `ag.save()`. | `app/ferias/services.py:469`, `app/ferias/services.py:483`, `app/ferias/services.py:493`, `app/ferias/services.py:503` |
| `alinhar_status_por_data` | Relê sob trava; se não cancelado e status diverge da data, salva status e dispara espelho. | `app/ferias/services.py:588`, `app/ferias/services.py:600`, `app/ferias/services.py:602` |

Todas as linhas acima alcançam o espelho de férias por código; o alcance de uma linha de produção concreta é `[nao verificado]`.

Portas HTTP:

| View → escritor | Evidência do chamado | Rota (prefixo `/ferias/` em `app/config/urls.py:32`) |
|---|---|---|
| `criar_periodo_aquisitivo` → `registrar_gozo_historico` | `app/ferias/views.py:124`, `app/ferias/views.py:128`, `app/ferias/views.py:211` | `app/ferias/urls.py:15` |
| `agendar_ferias` → `_executar_agendamento` → `executar_agendamento` | `app/ferias/views.py:270`, `app/ferias/views.py:299`, `app/ferias/views.py:264` | `app/ferias/urls.py:16` |
| `agendar_lote` → mesmo wrapper | `app/ferias/views.py:588`, `app/ferias/views.py:636` | `app/ferias/urls.py:10` |
| `cancelar_agendamento` → `cancelar_agendamento_dp` | `app/ferias/views.py:322`, `app/ferias/views.py:330` | `app/ferias/urls.py:18` |
| `cancelar_agendamento_admin` → wrapper → `executar_cancelamento_admin` | `app/ferias/views.py:345`, `app/ferias/views.py:370`, `app/ferias/views.py:531` | `app/ferias/urls.py:19` |
| `corrigir_concluida_admin` → mesmo wrapper, `permitir_concluido=True` | `app/ferias/views.py:388`, `app/ferias/views.py:418` | `app/ferias/urls.py:20` |
| `cancelar_admin_massa` → mesmo wrapper | `app/ferias/views.py:534`, `app/ferias/views.py:575` | `app/ferias/urls.py:9` |
| `editar_datas_admin` → `corrigir_datas_agendamento` | `app/ferias/views.py:434`, `app/ferias/views.py:503` | `app/ferias/urls.py:21` |
| `editar_agendamento` → `remarcar_agendamento` | `app/ferias/views.py:652`, `app/ferias/views.py:688` | `app/ferias/urls.py:22` |
| `avaliar_solicitacao` (ação aprovar) → `aprovar_solicitacao` | `app/ferias/views.py:733`, `app/ferias/views.py:750`, `app/ferias/views.py:753` | `app/ferias/urls.py:12` |
| `ia_perguntar` → `_aplicar_proposta` → `_aplicar_ferias` → escritor registrado `agendar_ferias_porta` | `app/core/views_ia.py:859`, `app/core/views_ia.py:871`, `app/core/views_ia.py:874`, `app/core/views_ia.py:145`, `app/core/views_ia.py:684`, `app/core/intake.py:34` | `/ia/perguntar/`, `app/core/urls.py:15`, `app/config/urls.py:39` |

O comando `sincronizar_status_ferias` chama `alinhar_status_por_data` apenas com `--apply`; sem ele só relata. `app/ferias/management/commands/sincronizar_status_ferias.py:21`, `app/ferias/management/commands/sincronizar_status_ferias.py:44`, `app/ferias/management/commands/sincronizar_status_ferias.py:49`.

`pedir_confirmacao` e `registrar_resposta_do_colab` salvam somente confirmação/carimbo, cujos campos não estão em `_CAMPOS_DO_ESPELHO`, portanto essas operações não disparam o espelho. `app/ferias/services.py:553`, `app/ferias/services.py:571`, `app/ferias/services.py:575`, `app/ferias/models.py:444`, `app/ferias/models.py:450`.

Sobre leitores de `#N`: não encontrei no código pesquisado parser/junção que use `Ausencia.descricao` para buscar `AgendamentoFerias`; a afirmação universal de inexistência deve ficar `[nao verificado]`. Os usos concretos relevantes são:

- `espelhar_ferias` **escreve** `Ferias agendamento #<pk>` na descrição, porém seleciona existentes apenas por colaborador, tipo e janela, e pula qualquer dia já presente sem comparar descrição/status/origem. `app/ponto/services/ausencia.py:1183`, `app/ponto/services/ausencia.py:1200`, `app/ponto/services/ausencia.py:1207`.
- `AgendamentoFerias._guard_sobreposicao` chama de `_minhas` todas as ausências de férias do colaborador dentro da janela antiga; não lê `#N`. `app/ferias/models.py:427`, `app/ferias/models.py:430`.
- `painel_ausencia` agrupa irmãos por `documento_referencia`, e o template mostra descrição como texto; não recebe agendamento de origem no contexto. `app/ponto/views.py:2073`, `app/ponto/views.py:2076`, `app/ponto/views.py:2079`, `app/templates/ponto/partials/painel_ausencia.html:57`.
- `dias_ferias_com_batida` usa `ferias_na_janela` e datas dos agendamentos diretamente, sem casar descrição de Ausencia. `app/folha/export.py:484`, `app/folha/export.py:491`.

## 13/11 12:54 · sessao 54fa72808182 · HJ-090 · sonda · origem=Code

Não localizei `passivo.py`; não houve nova execução. Segue a saída integral fornecida. [nao verificado]

```text
sonda_leitura: passivo.py [eb27488373f8215e] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

HJ091 2026-11-13 12:53:06.709124-03:00
universo ausencias_ferias 3001 agendamentos 116 dias_vivos 2943
1 ferias_ausencia_sem_agendamento (aprovada sem agendamento vivo cobrindo) 58 colabs 53
[(4635, 29, '2026-10-29'), (4839, 220, '2026-10-20'), (4165, 168, '2026-10-20'), (4613, 252, '2026-10-19'), (4630, 106, '2026-10-07'), (4634, 141, '2026-10-07'), (4633, 150, '2026-10-07'), (4631, 177, '2026-10-05'), (4615, 379, '2026-10-01'), (4614, 327, '2026-10-01'), (4620, 250, '2026-10-01'), (4617, 225, '2026-10-01'), (4611, 254, '2026-09-29'), (4623, 289, '2026-09-23'), (4628, 110, '2026-09-21'), (4618, 370, '2026-09-17'), (4624, 272, '2026-09-16'), (4625, 237, '2026-09-15'), (4627, 59, '2026-09-10'), (4629, 133, '2026-09-09'), (4621, 214, '2026-09-08'), (4626, 355, '2026-09-01'), (4622, 228, '2026-08-25'), (4651, 196, '2026-05-26'), (4652, 277, '2026-05-19'), (4654, 334, '2026-05-18'), (4653, 252, '2026-05-18'), (4656, 284, '2026-05-11'), (4664, 267, '2026-05-05'), (4663, 301, '2026-05-05'), (4661, 278, '2026-05-05'), (4662, 298, '2026-05-05'), (4655, 202, '2026-05-04'), (4658, 280, '2026-04-23'), (4659, 231, '2026-04-22'), (4660, 205, '2026-04-22'), (4657, 226, '2026-04-16'), (4665, 335, '2026-04-13'), (4641, 129, '2026-04-13'), (4666, 262, '2026-04-13'), (4667, 195, '2026-04-07'), (4642, 100, '2026-04-01'), (4636, 40, '2026-03-31'), (4669, 292, '2026-03-24'), (4643, 150, '2026-03-19'), (4668, 255, '2026-03-17'), (4644, 134, '2026-03-02'), (4645, 173, '2026-02-25'), (4637, 44, '2026-02-10'), (4646, 107, '2026-02-09'), (4649, 747, '2026-02-05'), (4648, 165, '2026-01-26'), (4647, 168, '2026-01-26'), (4650, 177, '2026-01-26'), (4638, 28, '2026-01-05'), (4639, 32, '2026-01-05'), (4640, 42, '2026-01-05'), (4632, 150, '0026-10-07')]
2 ferias_dia_sem_ausencia (dia de agendamento vivo sem ausencia aprovada) 2 colabs 2
[(127, '2026-10-02', [114]), (344, '2026-11-07', [103])]
3 rejeitada_ou_cancelada_dentro_de_agendamento_vivo 1 colabs 1
[(3977, 344, '2026-11-07', 'rejeitada', [103])]
4 backfill: sem #N na descricao 58
[(4635, 29, '2026-10-29', 'aprovada'), (4612, 354, '2026-10-29', 'rejeitada'), (4839, 220, '2026-10-20', 'aprovada'), (4613, 252, '2026-10-19', 'aprovada'), (4630, 106, '2026-10-07', 'aprovada'), (4634, 141, '2026-10-07', 'aprovada'), (4633, 150, '2026-10-07', 'aprovada'), (4631, 177, '2026-10-05', 'aprovada'), (4615, 379, '2026-10-01', 'aprovada'), (4614, 327, '2026-10-01', 'aprovada'), (4620, 250, '2026-10-01', 'aprovada'), (4617, 225, '2026-10-01', 'aprovada'), (4611, 254, '2026-09-29', 'aprovada'), (4623, 289, '2026-09-23', 'aprovada'), (4628, 110, '2026-09-21', 'aprovada'), (4618, 370, '2026-09-17', 'aprovada'), (4624, 272, '2026-09-16', 'aprovada'), (4625, 237, '2026-09-15', 'aprovada'), (4627, 59, '2026-09-10', 'aprovada'), (4629, 133, '2026-09-09', 'aprovada'), (4621, 214, '2026-09-08', 'aprovada'), (4626, 355, '2026-09-01', 'aprovada'), (4622, 228, '2026-08-25', 'aprovada'), (4651, 196, '2026-05-26', 'aprovada'), (4652, 277, '2026-05-19', 'aprovada'), (4654, 334, '2026-05-18', 'aprovada'), (4653, 252, '2026-05-18', 'aprovada'), (4656, 284, '2026-05-11', 'aprovada'), (4664, 267, '2026-05-05', 'aprovada'), (4663, 301, '2026-05-05', 'aprovada'), (4661, 278, '2026-05-05', 'aprovada'), (4662, 298, '2026-05-05', 'aprovada'), (4655, 202, '2026-05-04', 'aprovada'), (4658, 280, '2026-04-23', 'aprovada'), (4659, 231, '2026-04-22', 'aprovada'), (4660, 205, '2026-04-22', 'aprovada'), (4657, 226, '2026-04-16', 'aprovada'), (4665, 335, '2026-04-13', 'aprovada'), (4641, 129, '2026-04-13', 'aprovada'), (4666, 262, '2026-04-13', 'aprovada'), (4667, 195, '2026-04-07', 'aprovada'), (4642, 100, '2026-04-01', 'aprovada'), (4636, 40, '2026-03-31', 'aprovada'), (4669, 292, '2026-03-24', 'aprovada'), (4643, 150, '2026-03-19', 'aprovada'), (4668, 255, '2026-03-17', 'aprovada'), (4644, 134, '2026-03-02', 'aprovada'), (4645, 173, '2026-02-25', 'aprovada'), (4637, 44, '2026-02-10', 'aprovada'), (4646, 107, '2026-02-09', 'aprovada'), (4649, 747, '2026-02-05', 'aprovada'), (4648, 165, '2026-01-26', 'aprovada'), (4647, 168, '2026-01-26', 'aprovada'), (4650, 177, '2026-01-26', 'aprovada'), (4638, 28, '2026-01-05', 'aprovada'), (4639, 32, '2026-01-05', 'aprovada'), (4640, 42, '2026-01-05', 'aprovada'), (4632, 150, '0026-10-07', 'aprovada')]
5 backfill: #N nao e agendamento vivo cobrindo o dia 2
[(4165, 168, '2026-10-20', 'aprovada', 109), (2634, 127, '2026-09-02', 'aprovada', 77)]
status das ausencias ferias ['aprovada', 'rejeitada']
caso 2634 (127, '2026-09-02', 'ferias', 'aprovada', 'Cancelada pelo admin (JSP02, 05/10/2026): Cancelada pelo admin (JSP02, 01/10/2026): Ferias agendamento #77 | veredito_celula: justificado') cobre [114]
caso 3977 (344, '2026-11-07', 'ferias', 'rejeitada', 'Cancelada pelo admin (JSP02, 13/11/2026): Ferias agendamento #103') cobre [103]
ERROS 0
```

Totais: 1 = 58, 53 colaboradores; 2 = 2, 2 colaboradores; 3 = 1, 1 colaborador. [nao verificado]  
Backfill: total 4 = 58; total 5 = 2. [nao verificado]  
ERROS = 0: sim, conforme saída fornecida. [nao verificado]

## 13/11 12:40 · sessao b5801a0e755c · TIPO-NA-FONTE · leitura · origem=Code

Os dois desenhos ainda exigem definições para fechar a classe inteira. O SELO atual não segue o valor entre funções; o TIPO atual expõe campos crus e não proíbe valor-verdade. Não há fundamento nesta cópia para declarar nenhum dos dois desenhos novos certificado. (`app/escala/tests/test_tipo_item3_selo_classe_fallback.py:76`, `app/escala/tests/test_tipo_item3_selo_classe_fallback.py:124`, `app/escala/services/ata.py:81`)

Abaixo, os caminhos abreviados começam em `app/`. Os anexos transcritos no prompt não foram tratados como arquivos presentes na cópia; suas informações de execução permanecem **[não verificado]**.

**1. Censo dos sítios e dimensão das mudanças**

A unidade usada é ocorrência AST: chamada de acessor ou operação de leitura de chave. Duas chamadas na mesma linha contam duas vezes; uma função pode conter vários sítios. Isso distingue as duas chamadas do vigia na linha 76. (`ponto/services/vigia_pareador_celula.py:76`)

| Acessor | Produção fora de `ata.py` | Testes: chamadas diretas | Sítios de produção | [nao verificado]
|---|---:|---:|---|
| `horas_orfas` | 6 | 5 | `chamados/services/materializacao.py:182`; `escala/services/leitor_celula.py:194`; `ponto/juiz_batida.py:163`; `ponto/services/esmeril_espelho.py:126`; `ponto/services/vigia_pareador_celula.py:76`; `relatorios/cartao_pela_celula.py:289` |
| `contar_orfas` | 3 | 5 | `escala/services/leitor_celula.py:195`; `ponto/services/cartorio.py:841`; `ponto/services/vigia_pareador_celula.py:76` |
| `leitura_orfas`, inclusive `_leitura_orfas` | 1 | 3 | `ponto/management/commands/tripwire_celulas.py:267` |
| `horas_orfas_medidas` | 1 | 1 | `ponto/services/cluster_espurio.py:31` |
| `tem_orfas` | 1 | 3 | `chamados/juizes.py:511` |
| `instantes_orfas` | 0 reconhecido | 5 | Chamadas de teste: `escala/tests/test_tipo_item3_v5_expand.py:79`, `:85`, `:92`, `:103`, `:123` |
| **Total externo** | **12 chamadas, 11 posições, 10 funções/arquivos** | **22 chamadas** | Sítios enumerados acima. |

As outras chamadas diretas de teste estão todas em `escala/tests/test_tipo_item3_v5_expand.py`: H em `:101`, `:118`, `:130`, `:334`, `:339`; C em `:102`, `:119`, `:335`, `:340`, `:345`; R em `:100`, `:122`, `:333`; M em `:124`; T em `:120`, `:336`, `:341`.

Há também nove chamadas internas da porta: sete a `leitura_orfas`, uma a `_valores_orfas` e uma a `_instantes_gravados`. O wrapper `_orfas_da_ata` tem uma chamada de produção e três chamadas diretas de teste; o teste também passa funções leitoras por valor e depois as chama indiretamente. (`escala/services/ata.py:112`, `:142`, `:148`, `:154`, `:160`, `:173`, `:176`, `:231`, `:262`; `escala/services/leitor_celula.py:425`; `escala/tests/test_tipo_item3_v5_expand.py:121`, `:140`, `:141`, `:352`, `:357`)

| Chave crua da **ata** | Produção fora da porta | Dentro da porta | Leituras em testes | [nao verificado]
|---|---:|---:|---:|
| `orfas` | 0 reconhecido | 1 | 19 |
| `n_orfas` | 0 reconhecido | 1 | 11 |
| `orfas_instantes`, antes do corte A | 0 reconhecido | 1 `.get`, além da verificação de presença | 2 |
| Fontes | Completude dinâmica: **[não verificado]** | `escala/services/ata.py:92`, `:168`, `:360` | Sítios enumerados a seguir. |

Os 19 sítios de teste de `orfas` são: `escala/tests/test_tipo_item3_v5_expand.py:75`, `:76`, `:201`, `:202`, `:203`, `:212`, `:334`, `:336`; `ponto/tests/test_bateria_lavratura.py:637`, `:960`, `:1057`; `ponto/tests/test_lampada_acende_pela_batida.py:78`, `:81`; `ponto/tests/test_o118_orfa_que_acendeu.py:90`, `:108`, `:133`, `:162`, `:165`, `:167`.

Os 11 de `n_orfas` são: `escala/tests/test_tipo_item3_v5_expand.py:77`, `:204`, `:335`, `:336`; `ponto/tests/test_bateria_lavratura.py:542`, `:552`, `:646`, `:798`, `:950`; `ponto/tests/test_o118_orfa_que_acendeu.py:104`, `:108`. Os dois do paralelo são `escala/tests/test_tipo_item3_v5_expand.py:78`, `:205`.

`lampadas` não contém diretamente a lista de órfãs: guarda os marcos, `acesa` e `luz`; o escritor recebe órfãs por outro parâmetro. O subconjunto comprovado de co-leitura com órfãs tem **10 expressões em nove leitores de produção**. (`ponto/services/cartorio.py:600`, `:629`, `:712`, `:723`)

Essas dez expressões são: `chamados/juizes.py:509`; `chamados/services/materializacao.py:174`; `ponto/juiz_batida.py:161`; `ponto/management/commands/tripwire_celulas.py:267`; `ponto/services/cartorio.py:835`; `ponto/services/cluster_espurio.py:32`, `:35`; `ponto/services/esmeril_espelho.py:130`; `ponto/services/vigia_pareador_celula.py:80`; `relatorios/cartao_pela_celula.py:290`.

Nos testes, a mesma medida de co-leitura encontra **11 expressões em dez métodos**, sem significar que `lampadas` armazene órfãs. (`ponto/tests/test_o118_orfa_que_acendeu.py:89`, `:101`, `:132`, `:166`; `ponto/tests/test_bateria_lavratura.py:645`, `:793`, `:945`, `:959`, `:1042`, `:1054`; `ponto/tests/test_lampada_acende_pela_batida.py:65`)

Existem ainda **33 leituras de `orfas` da grade/saída-dia**, distintas do acesso à chave da ata. A cópia as distribui assim:

| Arquivo | Linhas |
|---|---|
| `colaboradores/services/calendario.py` | `:380`, `:381` |
| `escala/orfa_classe.py` | `:58` |
| `escala/services/furos_diarios.py` | `:22` |
| `escala/utils.py` | `:802`, `:805`, `:806`, `:1046`, `:1113`, `:1835`, `:1837`, `:1840`, `:1841`, `:1897`, `:1898`, `:1971`, `:1976` |
| `folha/causas.py` | `:43`, `:100` |
| `ponto/management/commands/detectar_par_relampago.py` | `:140` |
| `ponto/management/commands/propor_reorganizacao.py` | `:104` |
| `ponto/propositor.py` | `:39` |
| `ponto/services/bordas_realizado.py` | `:49` |
| `ponto/services/cartorio.py` | `:737`, `:772`, `:1428` |
| `ponto/services/simular_dia.py` | `:12` |
| `ponto/status_dia.py` | `:188` |
| `ponto/supra_juiz.py` | `:379`, `:514` |
| `ponto/turnos.py` | `:2667` |
| `relatorios/pdf_espelho.py` | `:477`, `:552` |

Outro homônimo trata perguntas fora do cadastro de juízes, não órfãs de batida. (`core/management/commands/gerar_diagrama.py:854`)

Quanto muda em cada desenho:

| Desenho | Dimensão demonstrável | Quantidade final de mudanças |
|---|---|---|
| **i — SELO de fluxo** | As 12 chamadas externas podem conservar a assinatura. Dois leitores atuais já apresentam as formas especificadas: parâmetro que chega a `or []` e ternário controlado por `contar_orfas`. (`tripwire_celulas.py:267`, `:168`; `vigia_pareador_celula.py:76`) | **Pelo menos dois leitores exigem resolução sob esse critério**, além do selo. Se a marca atravessa compreensões e conversões, também alcança a lista derivada e o `or` de `_orfas_da_ata`; a fronteira exata dessa propagação não está implementada. (`escala/services/leitor_celula.py:194`, `:195`) Total: **[não verificado]**. |
| **ii — TIPO** | Há **11 expressões de retorno não booleano** em nove leitores, mais o predicado `tem_orfas` no décimo. Campos crus também precisam entrar na análise: proteger apenas o objeto externo deixa `.horas` desprotegido. (`escala/services/ata.py:81`; sítios da tabela anterior) | O teste de verdade de C no vigia necessariamente quebra se C passar a devolver o objeto descrito. Compreensões mudam se a iteração também for proibida. O destino do retorno booleano de `tem_orfas`, da igualdade e da extração explícita não foi definido: total de edições **[não verificado]**. (`vigia_pareador_celula.py:76`; `leitor_celula.py:194`; `ata.py:158`) |

**2. O que escapa**

No **SELO atual**, a conferência em memória confirmou os escapes abaixo. Seu código liga nomes a `leitura_orfas`, mas não propaga resultados de H/C por atribuições e parâmetros; a varredura só examina `BoolOp(Or)`. (`escala/tests/test_tipo_item3_selo_classe_fallback.py:76`, `:86`, `:124`)

```python
h = horas_orfas(a)
return h or []

return (h := horas_orfas(a)) or []

x = leitura_orfas(a)
return x.horas if x.horas else []

return a.get("n_orfas", 0)

def consumir(v):
    return v or []
return consumir(leitura_orfas(a).horas)
```

Essas são formas que o **desenho i solicitado pretende cobrir**, não escapes que se possam atribuir a uma implementação nova inexistente. Import relativo e atributo intermediário também escapam do reconhecimento atual. (`escala/tests/test_tipo_item3_selo_classe_fallback.py:49`, `:65`)

Para o desenho i, continuam sem cobertura demonstrada a identificação dinâmica da origem, recipientes intermediários e a travessia por bibliotecas/conversões. Exemplo hipotético que o selo atual deixa passar; comportamento do selo novo: **[não verificado]**.

```python
f = getattr(porta, "horas_" + sufixo)
return f(a) or []
```

A cópia já declara limites de identificação estática, inclusive chave dinâmica, fluxo de bindings e construções sem lacuna emitida; isso não constitui prova de completude do futuro selo de fluxo. (`bin/leitores_de_chave.py:53`)

No **desenho ii**, há dois limites distintos:

- Proibir verdade apenas em `LeituraOrfas` deixa passar `leitura_orfas(a).horas or []`, pois o campo atual é `list | None`. Esse é precisamente o campo repassado pelo tripwire. (`escala/services/ata.py:83`; `ponto/management/commands/tripwire_celulas.py:267`)
- Mesmo protegendo todos os valores retornados, proibir `bool` não obriga o leitor a consultar `.estado`. Testes de identidade, tipo ou igualdade podem escolher um default sem chamar `__bool__`. O comportamento de `__eq__`, da extração e das conversões no tipo novo é **[não verificado]**. [nao verificado]

Exemplo hipotético que não solicita verdade, comprimento ou iteração do objeto; não consulta estado e descarta o valor: **[não verificado]**.

```python
x = horas_orfas(a)
return [] if x is None else []
```

Se o novo objeto expuser novamente um payload cru, o fallback pode reaparecer depois dessa extração. A existência dessa API é **[não verificado]**; o precedente concreto é a exposição atual de `.horas` e `.quantidade`. (`escala/services/ata.py:83`)

Proibir iteração tem custo próprio: os leitores atuais iteram H diretamente. Portanto essa proibição alcança operações válidas de consumo, além do teste de verdade que motivou o desenho. (`escala/services/leitor_celula.py:194`; `chamados/services/materializacao.py:182`; `ponto/juiz_batida.py:163`)

**3. Fixtures e testes** [nao verificado]

O levantamento direto reconheceu **203 expressões de dict cru em 129 arquivos**: 76 expressões dentro de **74 métodos `test_`**, e 127 em contextos auxiliares, setup ou classe. É **cota inferior**, usando apenas dict literal em `ata=`, atribuição `.ata`, variável com nome contendo `ata` ou `setattr(..., 'ata', dict)`. Exemplos contados: `escala/tests/test_t41_leitor_celula.py:16`, `:41`, `:64`, `:87`, `:156`; `ponto/tests/test_juiz_batida.py:156`, `:171`, `:185`, `:199`; `ponto/tests/test_porta_celula.py:83`, `:114`, `:139`, `:152`, `:209`.

O total de testes alcançados por helpers, herança, aliases e tabelas é **[não verificado]**. Há exclusões concretas do critério direto nas tabelas de casos de `escala/tests/test_tipo_item3_v5_expand.py:110`, `:237`, `:316`.

Esses 74 testes **não equivalem a 74 conversões necessárias**: a entrada já aceita `Mapping`, `AtaVigiada` aceita dict e o descriptor de CI envolve o valor automaticamente. Alterar apenas o retorno não exige tornar a fixture estrita. (`escala/services/ata.py:90`, `:272`; `escala/services/ata_ci.py:20`, `:26`)

Há **dez comparações diretas de retorno com lista/int, em seis testes**: `escala/tests/test_tipo_item3_v5_expand.py:79`, `:92`, `:101`, `:102`, `:118`, `:119`, `:334`, `:335`, `:339`, `:340`.

| Desenho | Efeito nesses testes | [nao verificado]
|---|---|
| **i — SELO** | Não exige trocar dicts ou expectativas lista/int por mudança do retorno. O selo existente e seus mutantes precisariam passar a provar propagação; hoje o mutante principal cobre `_l.horas or []` local. (`escala/tests/test_tipo_item3_selo_classe_fallback.py:154`) |
| **ii — TIPO** | Os seis testes comparadores precisam ser revistos contra o contrato do objeto. Se houver extração explícita, passam a conferir estado e valor extraído; igualdade compatível poderia conservar alguns asserts. API e quantidade efetiva de edições: **[não verificado]**. (`escala/tests/test_tipo_item3_v5_expand.py:79`, `:334`) |
| **Corte A, comum** | **Dois testes exigem explicitamente a chave recém-gravada** e contradizem sua retirada. A fixture de classe também contém o paralelo. O custo dos demais testes de `instantes_orfas` depende do destino dessa API, não definido apenas pela retirada da chave. (`escala/tests/test_tipo_item3_v5_expand.py:78`, `:205`, `:135`, `:85`, `:103`, `:123`) |

A exceção atual que permite leitura crua por testes é explícita; nenhum dos dois desenhos elimina automaticamente esses testemunhos. (`escala/services/ata.py:294`)

**4. Matriz de transições**

H=`horas_orfas`; C=`contar_orfas`; R=`leitura_orfas`; M=`horas_orfas_medidas`; T=`tem_orfas`. (`escala/services/ata.py:108`, `:140`, `:146`, `:152`, `:158`)

Esta é a matriz executável **da cópia**, compartilhada como linha de base pelos dois desenhos. No desenho i, um selo isolado não altera esses retornos. No desenho ii, o retorno passaria ao objeto descrito no pedido; sua representação, extração e ações adaptadas ainda são **[não verificado]**.

| Estado | R atual | H atual | C atual | M atual | T atual |
|---|---|---|---|---|---|
| N — `NAO_LAVRADA`, `None` | `(None,None,N)` | `[]` | `0` | `None` | `False` |
| S — `LEGADO_SEM_CHAVES`, `{}` | `(None,None,S)` | `[]` | `0` | `None` | `False` |
| C — `LEGADO_SO_CONTAGEM` | `(None,n,C)` | `[]` | `n` se verdadeiro, senão `0` | `None` | verdade da contagem |
| L — `LEGADO_SO_LISTA` | `(v,None,L)` | `v` se verdadeiro, senão `[]` | `0` | `v` se lista, senão `None` | verdade de H |
| D — `LEGADO_DESCASADA` | valores crus e D | valor verdadeiro ou `[]` | valor verdadeiro ou `0` | lista crua ou `None` | verdade de H ou C |
| V — `LAVRADA` | lista, inteiro e V | lista | inteiro | lista | verdade de H ou C |
| I — `ILEGAL`, não Mapping | exceção | exceção | exceção | exceção | exceção |
| Fontes | `ata.py:108` | `ata.py:128`, `:140` | `ata.py:152` | `ata.py:146` | `ata.py:158` |

Os nomes L/C não garantem tipos: a classificação verifica ausência antes de validar os dois valores em conjunto. Também trata valor `None` como ausência, mesmo quando a chave existe. D inclui contagem inconsistente e tipos inadequados; o teste inclui `orfas='08:00'`. (`escala/services/ata.py:95`; `escala/tests/test_tipo_item3_v5_expand.py:327`)

Nas tabelas seguintes, **h/c** são os retornos efetivos de H/C da tabela anterior. As guardas são parte da transição: não se pode substituir “acessor não chamado” por uma classificação suposta. `{}` e `{'lampadas': [...]}` podem ter o mesmo estado S e tomar caminhos diferentes. (`chamados/juizes.py:506`; `chamados/services/materializacao.py:176`)

Cada linha descreve o leitor atual. Para ii, preservação da ação após adaptar o retorno tipado: **[não verificado]**.

| Estado | Leitor | Acessor → retorno → ação atual | Teste que prova | [nao verificado]
|---|---|---|---|
| N | `_orfas_da_ata` | Não chama; guarda devolve `[]`. `escala/services/leitor_celula.py:189` | `escala/tests/test_tipo_item3_v5_expand.py:141` |
| S | `_orfas_da_ata` | `{}` não chama; S não vazio lê H=`[]`, depois C=`0`; devolve `[]`. `leitor_celula.py:189`, `:194`, `:195` | S não vazio: `test_tipo_item3_v5_expand.py:111`, `:121`; `{}` específico: **[não verificado]** |
| C | `_orfas_da_ata` | H=`[]`; C=c; devolve `['?'] * int(c)`. `leitor_celula.py:194`, `:195` | `test_tipo_item3_v5_expand.py:112`; `escala/tests/test_t41_leitor_celula.py:93` |
| L | `_orfas_da_ata` | H=h; filtra/converte; C só se lista derivada vazia. `leitor_celula.py:194`, `:195` | `test_tipo_item3_v5_expand.py:113` |
| D | `_orfas_da_ata` | H=h; se derivada vazia, usa C=c para `?`. `leitor_celula.py:195` | Exemplo D: `test_tipo_item3_v5_expand.py:114` |
| V | `_orfas_da_ata` | H=h; repassa strings; C somente se derivada vazia. `leitor_celula.py:194` | `test_tipo_item3_v5_expand.py:140` |
| I | `_orfas_da_ata` | Verdadeiro: H levanta; falso: guarda devolve `[]`. `leitor_celula.py:189`; `ata.py:90` | **[não verificado]** |
| N | materialização | `None→{}`; sem lâmpadas pula, sem H. `chamados/services/materializacao.py:173`, `:176` | **[não verificado]** |
| S | materialização | `{}` pula; com lâmpadas úteis, H=`[]`, sem marcas órfãs. `materializacao.py:174`, `:182` | **[não verificado]** |
| C | materialização | Se guarda permite, H=`[]`; acrescenta nenhuma marca órfã. `materializacao.py:176`, `:182` | **[não verificado]** |
| L | materialização | H=h; filtra HH:MM e une às luzes. `materializacao.py:178`, `:182` | **[não verificado]** |
| D | materialização | H=h; filtra; erro pode ser capturado e virar False. `materializacao.py:182`, `:186` | **[não verificado]** |
| V | materialização | H=h; marcas alimentam `plantio_preenche_furo`. `materializacao.py:181`, `:183` | Caso com lista vazia: `chamados/tests/test_hx_plantio_celula.py:30`, `:35` |
| I | materialização | Não Mapping verdadeiro pode falhar em `.get` antes de H; captura devolve False. `materializacao.py:174`, `:186` | **[não verificado]** |
| N | `_celula_furada` | `None→{}`; falta `lampadas`; None, sem T. `chamados/juizes.py:494`, `:506` | **[não verificado]** |
| S | `_celula_furada` | `{}` retorna None; com lâmpadas e chegando a T, T=False. `juizes.py:506`, `:511` | **[não verificado]** |
| C | `_celula_furada` | Se chega a T, verdade de c; órfã prende somente com régua confiável. `juizes.py:511`, `:517` | **[não verificado]** |
| L | `_celula_furada` | T=verdade de h; lâmpada apagada pode retornar antes. `juizes.py:509`, `:511` | **[não verificado]** |
| D | `_celula_furada` | T=verdade de h ou c; consulta régua. `juizes.py:511`, `:517` | **[não verificado]** |
| V | `_celula_furada` | T e régua decidem a contribuição das órfãs. `juizes.py:511`, `:517` | `chamados/tests/test_s122_orfa_regua.py:52`, `:60` |
| I | `_celula_furada` | Pode sair por falta de `lampadas`; demais erros capturados retornam None. `juizes.py:506`, `:520` | **[não verificado]** |
| N | cartão | `None→{}`; sem H; conjunto vazio. `relatorios/cartao_pela_celula.py:288`, `:289` | **[não verificado]** |
| S | cartão | `{}` não chama H; S não vazio chama H=`[]`; conjunto vazio. `cartao_pela_celula.py:289` | **[não verificado]** |
| C | cartão | H=`[]`; conjunto vazio. `cartao_pela_celula.py:289` | **[não verificado]** |
| L | cartão | H=h; conjunto dos elementos strings. `cartao_pela_celula.py:289` | `relatorios/tests/test_cartao_pela_celula.py:163`, `:167` |
| D | cartão | H=h; itera/filtra; forma inadequada pode falhar. `cartao_pela_celula.py:289` | **[não verificado]** |
| V | cartão | H=h; conjunto por data. `cartao_pela_celula.py:289` | Prova específica V: **[não verificado]** |
| I | cartão | Verdadeiro chega a H e levanta; falso é convertido em `{}`. `cartao_pela_celula.py:288`, `:289`; `ata.py:90` | **[não verificado]** |
| N | juiz_batida | Não Mapping→`{}`; sem H; `orfas=[]`. `ponto/juiz_batida.py:158`, `:163` | **[não verificado]** |
| S | juiz_batida | `{}` não chama H; S não vazio lê H=`[]`. `juiz_batida.py:163` | **[não verificado]** |
| C | juiz_batida | H=`[]`; `orfas=[]`. `juiz_batida.py:163` | **[não verificado]** |
| L | juiz_batida | H=h; lista de `str(h)` no resultado. `juiz_batida.py:163` | `ponto/tests/test_juiz_batida.py:179`, `:189` |
| D | juiz_batida | H=h; itera/converte; pode falhar por forma. `juiz_batida.py:163` | **[não verificado]** |
| V | juiz_batida | H=h; órfãs separadas dos períodos. `juiz_batida.py:163`, `:166` | Prova específica V: **[não verificado]** |
| I | juiz_batida | Não Mapping→`{}`; sem H; `orfas=[]`. `juiz_batida.py:158` | **[não verificado]** |
| N | esmeril | Não chama H; nenhuma contribuição A5. `ponto/services/esmeril_espelho.py:126` | **[não verificado]** |
| S | esmeril | `{}` não chama; S não vazio lê H=`[]`. `esmeril_espelho.py:126` | **[não verificado]** |
| C | esmeril | H=`[]`; nenhuma contribuição A5. `esmeril_espelho.py:126` | **[não verificado]** |
| L | esmeril | H=h; converte hora e conta faixas de 

[truncado em 20000 de 32737 caracteres]

## 13/11 12:35 · sessao b52f3ed8eaa9 · - · - · origem=Ronald (tmux)

Censo por ocorrência AST (`Call`, `Subscript` em leitura ou `.get`), sem executar aplicação nem suite. Comple­tude para formas dinâmicas/reflexivas: [não verificado]. [nao verificado]

| Acessor | Produção fora de ata.py | Teste direto | [nao verificado]
|---|---:|---:|
| `horas_orfas` | 6 | 5 |
| `contar_orfas` | 3 | 5 |
| `leitura_orfas` | 1 | 3 |
| `horas_orfas_medidas` | 1 | 1 |
| `tem_orfas` | 1 | 3 |
| `instantes_orfas` | 0 | 5 |
| Total | **12 expressões, 11 posições, 10 arquivos** | **22 expressões** |

Referências produção: `app/chamados/juizes.py:511`; `app/chamados/services/materializacao.py:182`; `app/escala/services/leitor_celula.py:194-195`; `app/ponto/juiz_batida.py:163`; `app/ponto/management/commands/tripwire_celulas.py:267`; `app/ponto/services/cartorio.py:841`; `app/ponto/services/cluster_espurio.py:31`; `app/ponto/services/esmeril_espelho.py:126`; `app/ponto/services/vigia_pareador_celula.py:76`; `app/relatorios/cartao_pela_celula.py:289`.

Referências teste, todas em `app/escala/tests/test_tipo_item3_v5_expand.py`: horas `101,118,130,334,339`; contar `102,119,335,340,345`; leitura `100,122,333`; medidas `124`; tem `120,336,341`; instantes `79,85,92,103,123`.

Adicionais: nove chamadas internas da porta (`ata.py:112,142,148,154,160,173,176,231,262`); três chamadas diretas de `_orfas_da_ata` em teste (`test_tipo_item3_v5_expand.py:121,140,141`), uma em produção (`leitor_celula.py:425`); quatro funções leitoras passadas como valores e chamadas indiretamente no teste (`test_tipo_item3_v5_expand.py:352-357`). [nao verificado]

Chaves cruas comprovadas da ATA:

| Chave | Produção fora da porta | Dentro da porta | Testes | [nao verificado]
|---|---:|---:|---:|
| `orfas` | 0 reconhecido | 1 | 19 |
| `n_orfas` | 0 reconhecido | 1 | 11 |
| `orfas_instantes` | 0 reconhecido | 1 | 2 |

Porta: `app/escala/services/ata.py:92,168`. Testes `orfas`: `test_tipo_item3_v5_expand.py:75,76,201,202,203,212,334,336`; `test_bateria_lavratura.py:637,960,1057`; `test_lampada_acende_pela_batida.py:78,81`; `test_o118_orfa_que_acendeu.py:90,108,133,162,165,167`. Testes `n_orfas`: expand `77,204,335,336`; bateria `542,552,646,798,950`; o118 `104,108`. Instantes: expand `78,205`.

São 12 métodos de teste lendo `orfas`, nove lendo `n_orfas`, união de 16 métodos; referências nos sites enumerados acima. A exceção aos testes é explícita em `app/escala/services/ata.py:294-297`.

As **33 leituras de `orfas` da grade/saída-dia**, fora da porta:

| Arquivo em `app/` | Linhas |
|---|---|
| `colaboradores/services/calendario.py` | 380,381 |
| `escala/orfa_classe.py` | 58 |
| `escala/services/furos_diarios.py` | 22 |
| `escala/utils.py` | 802,805,806,1046,1113,1835,1837,1840,1841,1897,1898,1971,1976 |
| `folha/causas.py` | 43,100 |
| `ponto/management/commands/detectar_par_relampago.py` | 140 |
| `ponto/management/commands/propor_reorganizacao.py` | 104 |
| `ponto/propositor.py` | 39 |
| `ponto/services/bordas_realizado.py` | 49 |
| `ponto/services/cartorio.py` | 737,772,1428 |
| `ponto/services/simular_dia.py` | 12 |
| `ponto/status_dia.py` | 188 |
| `ponto/supra_juiz.py` | 379,514 |
| `ponto/turnos.py` | 2667 |
| `relatorios/pdf_espelho.py` | 477,552 |

O 34º homônimo bruto não trata órfãs de batida: são perguntas fora do cadastro de juízes (`app/core/management/commands/gerar_diagrama.py:854`).

Nos testes, 82 leituras sintáticas de `orfas` se dividem em 19 ATA e 63 grade/saída-dia, conforme os sites ATA acima e exemplos de grade `app/escala/tests/test_grade_fuzz.py:80,83,213,236`, `app/escala/tests/test_t41_leitor_celula.py:71,93`, `app/escala/tests/test_tipo_item3_v5_expand.py:194-196,201`.

`lampadas`: 40 leituras sintáticas em produção fora da porta, uma delas DNA (`app/escala/utils.py:154`); 70 em teste. **Não equivalem a leitores do valor órfão**: o produtor guarda `luz`/`acesa` e órfãs separadamente (`app/ponto/services/cartorio.py:629,712-725`).

Subconjunto objetivo de `lampadas` co-lido com o valor órfão no mesmo leitor: dez expressões em nove leitores de produção (`juizes.py:509`; `materializacao.py:174`; `juiz_batida.py:161`; `tripwire_celulas.py:267`; `cartorio.py:835`; `cluster_espurio.py:32,35`; `esmeril_espelho.py:130`; `vigia_pareador_celula.py:80`; `cartao_pela_celula.py:290`). Testes: 11 expressões em dez métodos (`test_o118_orfa_que_acendeu.py:89,101,132,166`; `test_bateria_lavratura.py:645,793,945,959,1042,1054`; `test_lampada_acende_pela_batida.py:65`). [nao verificado]

Fixtures: **cota inferior de 203 expressões `Dict` em 129 arquivos**, reconhecendo apenas `ata=dict`, atribuição `.ata`, nome de variável contendo `ata` e `setattr(...,'ata',dict)`; 76 expressões pertencem diretamente a **74 métodos `test_` de 39 arquivos**, outras 127 estão em 121 contextos auxiliares/setup/classe. Exemplos diretos: `app/escala/tests/test_t41_leitor_celula.py:16,41,64,87,156`; `app/ponto/tests/test_juiz_batida.py:156,171,185,199`; `app/ponto/tests/test_porta_celula.py:83,114,139,152,209`. Total com aliases, retornos de helper, herança, tabelas de casos e kwargs dinâmicos: [não verificado]. Exclusões concretas do reconhecimento direto: `app/escala/tests/test_tipo_item3_v5_expand.py:110-115,237-247,316-328`.

A entrada dict pode permanecer: `AtaVigiada` aceita dict, e o descriptor envolve `Mapping` automaticamente; portanto esses 74 testes **não representam 74 mudanças obrigatórias** (`app/escala/services/ata.py:272-273`; `app/escala/services/ata_ci.py:20-29`). Exigir conversão de fixtures por estriteza seria outra escolha: [não verificado].

Comparações diretas de retorno do acessor com lista/int: **dez expressões em seis testes**, todas em `test_tipo_item3_v5_expand.py:79,92,101,102,118,119,334,335,339,340` — métodos iniciados nas linhas `71,87,99,106,330,338`. Mudam para extração explícita do resultado se TIPO retornar objeto opaco sem igualdade compatível; detalhes dessa API: [não verificado]. [nao verificado]

O corte (A) atinge diretamente dois testes que exigem presença da chave paralela (`test_tipo_item3_v5_expand.py:78,205`). Os demais cinco sites chamadores de `instantes_orfas` só têm custo integral determinado quando se decidir o destino dessa API; a remoção da chave, isoladamente, não especifica sua substituição (`test_tipo_item3_v5_expand.py:79,85,92,103,123`). [nao verificado]

## 13/11 12:33 · sessao 1a0cd66ebe6f · - · - · origem=Ronald (tmux)

As fontes abaixo usam o prefixo `app/`. A matriz descreve a parcela de órfãs; o resultado integral também depende de lâmpadas, DNA, cobertura e régua, portanto o estado sozinho não determina a saída integral. (`chamados/juizes.py:494`, `chamados/services/materializacao.py:174`, `ponto/services/cartorio.py:835`, `ponto/management/commands/tripwire_celulas.py:264`)

Leitores diretos: **12 chamadas em 10 funções/arquivos**; H=6, C=3, T=1, M=1, R=1. O wrapper `grade_da_celula` chama `_orfas_da_ata` e põe sua lista no vocabulário da grade. (`chamados/services/materializacao.py:182`, `relatorios/cartao_pela_celula.py:289`, `ponto/juiz_batida.py:163`, `ponto/services/esmeril_espelho.py:126`, `ponto/services/vigia_pareador_celula.py:76`, `escala/services/leitor_celula.py:194`, `escala/services/leitor_celula.py:195`, `ponto/services/cartorio.py:841`, `chamados/juizes.py:511`, `ponto/services/cluster_espurio.py:31`, `ponto/management/commands/tripwire_celulas.py:267`, `escala/services/leitor_celula.py:425`)

Definições para as 70 linhas:

- **H**=`horas_orfas`, **C**=`contar_orfas`, **T**=`tem_orfas`, **M**=`horas_orfas_medidas`, **R**=`leitura_orfas`. (`escala/services/ata.py:108`, `escala/services/ata.py:140`, `escala/services/ata.py:146`, `escala/services/ata.py:152`, `escala/services/ata.py:158`)
- **N**=NAO_LAVRADA/None; **S**=LEGADO_SEM_CHAVES/{}; **C2**=LEGADO_SO_CONTAGEM, exemplo `{'n_orfas':2}`; **L**=LEGADO_SO_LISTA; **D**=LEGADO_DESCASADA; **V**=LAVRADA/completa; **I**=não Mapping, exemplo `['x']`. (`escala/services/ata.py:26`, `escala/services/ata.py:95`, `escala/services/ata.py:110`)
- Em L/D/V, `h` e `c` são os valores efetivamente devolvidos por H/C; D **não** garante lista/int: o caso `orfas='08:00'` é explicitamente testado. (`escala/services/ata.py:128`, `escala/tests/test_tipo_item3_v5_expand.py:327`, `escala/tests/test_tipo_item3_v5_expand.py:334`)
- Para S com outras chaves, por exemplo `{'lampadas':[...]}`, valem as guardas da linha do leitor; não se pode extrapolar o caminho de `{}` para toda a classe S. (`escala/services/ata.py:95`, `escala/tests/test_tipo_item3_v5_expand.py:319`)

| Estado | Leitor | Acessor → retorno → ação | Prova |
|---|---|---|---|
| N | `_orfas_da_ata` | não chamado → `[]` pela guarda | `escala/services/leitor_celula.py:189`; teste `escala/tests/test_tipo_item3_v5_expand.py:141` |
| S={} | `_orfas_da_ata` | não chamado → `[]` pela guarda | `escala/services/leitor_celula.py:189`; prova específica de `{}` no leitor [não verificado] |
| C2 | `_orfas_da_ata` | H→`[]`, C→2 → `['?','?']` | `escala/services/leitor_celula.py:194`; teste `escala/tests/test_tipo_item3_v5_expand.py:112` |
| L | `_orfas_da_ata` | H→h → strings não vazias; C só se `reais` vazio | `escala/services/leitor_celula.py:194`; teste `escala/tests/test_tipo_item3_v5_expand.py:113` |
| D | `_orfas_da_ata` | H→h; se `reais` vazio, C→c → `'?' * int(c)` | `escala/services/leitor_celula.py:195`; exemplo D testado `escala/tests/test_tipo_item3_v5_expand.py:114` |
| V | `_orfas_da_ata` | H→h; repassa strings; C só se `reais` vazio | `escala/services/leitor_celula.py:194`; teste `escala/tests/test_tipo_item3_v5_expand.py:140` |
| I | `_orfas_da_ata` | se truthy, H levanta; se falsy, guarda devolve `[]` | `escala/services/leitor_celula.py:189`, `escala/services/ata.py:90`; teste deste leitor [não verificado] |
| N | materialização | `None→{}`; sem lâmpadas → pula, sem H | `chamados/services/materializacao.py:173`; teste específico [não verificado] |
| S={} | materialização | sem lâmpadas → pula, sem H | `chamados/services/materializacao.py:174`; teste específico [não verificado] |
| C2 | materialização | se lâmpadas permitem: H→`[]`; acrescenta nenhuma marca órfã | `chamados/services/materializacao.py:176`, `chamados/services/materializacao.py:182`; teste específico [não verificado] |
| L | materialização | se guarda permite: H→h; filtra HH:MM válidos e une às luzes | `chamados/services/materializacao.py:178`; teste específico [não verificado] |
| D | materialização | se guarda permite: H→h; filtro pode rejeitar valores/erro ser capturado | `chamados/services/materializacao.py:182`, `chamados/services/materializacao.py:186`; teste específico [não verificado] |
| V | materialização | H→h; marcas alimentam `plantio_preenche_furo` | `chamados/services/materializacao.py:181`; teste com lista vazia `chamados/tests/test_hx_plantio_celula.py:30`, `chamados/tests/test_hx_plantio_celula.py:35` |
| I | materialização | exemplo lista truthy falha no `.get` antes de H; captura → False | `chamados/services/materializacao.py:174`, `chamados/services/materializacao.py:186`; teste específico [não verificado] |
| N | `_celula_furada` | `None→{}`; falta `lampadas` → None, sem T | `chamados/juizes.py:494`, `chamados/juizes.py:506`; teste específico [não verificado] |
| S={} | `_celula_furada` | falta `lampadas` → None, sem T | `chamados/juizes.py:506`; teste específico [não verificado] |
| C2 | `_celula_furada` | se chega a T: True; prende só se régua confiável | `chamados/juizes.py:511`, `chamados/juizes.py:517`; teste específico [não verificado] |
| L | `_celula_furada` | T→bool(h); True consulta régua; lâmpada apagada pode retornar antes | `chamados/juizes.py:509`; teste específico [não verificado] |
| D | `_celula_furada` | T→bool(h) ou bool(c); mesma régua | `chamados/juizes.py:511`, `escala/services/ata.py:163`; teste específico [não verificado] |
| V | `_celula_furada` | T→bool(h) ou bool(c); régua decide se órfã prende | `chamados/juizes.py:517`; teste `chamados/tests/test_s122_orfa_regua.py:52`, `chamados/tests/test_s122_orfa_regua.py:60` |
| I | `_celula_furada` | exemplo `['x']`: falta `lampadas` → None; outros erros capturados → None | `chamados/juizes.py:506`, `chamados/juizes.py:520`; teste específico [não verificado] |
| N | cartão | `None→{}`; não H → conjunto vazio | `relatorios/cartao_pela_celula.py:288`; teste específico [não verificado] |
| S={} | cartão | não H → conjunto vazio | `relatorios/cartao_pela_celula.py:289`; teste específico [não verificado] |
| C2 | cartão | H→`[]` → conjunto vazio | `relatorios/cartao_pela_celula.py:289`; teste específico [não verificado] |
| L | cartão | H→h → conjunto só com strings | `relatorios/cartao_pela_celula.py:289`; teste `relatorios/tests/test_cartao_pela_celula.py:163`, `relatorios/tests/test_cartao_pela_celula.py:167` |
| D | cartão | H→h → itera e conserva strings; tipo inadequado pode falhar | `relatorios/cartao_pela_celula.py:289`; teste específico [não verificado] |
| V | cartão | H→h → conjunto por data | `relatorios/cartao_pela_celula.py:289`; teste específico V [não verificado] |
| I | cartão | exemplo lista truthy: H levanta | `relatorios/cartao_pela_celula.py:289`, `escala/services/ata.py:90`; teste específico [não verificado] |
| N | juiz_batida | não Mapping→{}; não H → `orfas=[]` | `ponto/juiz_batida.py:158`; teste específico [não verificado] |
| S={} | juiz_batida | não H → `orfas=[]` | `ponto/juiz_batida.py:163`; teste específico [não verificado] |
| C2 | juiz_batida | H→`[]` → `orfas=[]` | `ponto/juiz_batida.py:163`; teste específico [não verificado] |
| L | juiz_batida | H→h → lista `str(h)` no resultado | `ponto/juiz_batida.py:163`; teste `ponto/tests/test_juiz_batida.py:179` |
| D | juiz_batida | H→h → itera/converte; tipo inadequado pode falhar | `ponto/juiz_batida.py:163`; teste específico [não verificado] |
| V | juiz_batida | H→h → lista exibida, separada dos períodos | `ponto/juiz_batida.py:166`; teste específico V [não verificado] |
| I | juiz_batida | não Mapping→{}; não H → `orfas=[]` | `ponto/juiz_batida.py:158`; teste específico [não verificado] |
| N | esmeril | não H → nenhuma contribuição A5 | `ponto/services/esmeril_espelho.py:126`; teste específico [não verificado] |
| S={} | esmeril | não H → nenhuma contribuição A5 | `ponto/services/esmeril_espelho.py:126`; teste específico [não verificado] |
| C2 | esmeril | H→`[]` → nenhuma contribuição A5 | `ponto/services/esmeril_espelho.py:126`; teste específico [não verificado] |
| L | esmeril | H→h; `_hm(str(h))`; conta faixas de meia hora | `ponto/services/esmeril_espelho.py:127`; teste A5 [não verificado] |
| D | esmeril | H→h; mesma iteração/conversão, sem garantia de forma | `ponto/services/esmeril_espelho.py:126`; teste específico [não verificado] |
| V | esmeril | H→h; repetição >=3 produz A5 | `ponto/services/esmeril_espelho.py:140`; teste A5 [não verificado] |
| I | esmeril | exemplo lista truthy: H levanta | `ponto/services/esmeril_espelho.py:126`, `escala/services/ata.py:90`; teste específico [não verificado] |
| N | vigia | `None→{}`; C→0; não H; conjunto vazio | `ponto/services/vigia_pareador_celula.py:74`; teste específico [não verificado] |
| S={} | vigia | C→0; não H; conjunto vazio | `ponto/services/vigia_pareador_celula.py:76`; teste específico [não verificado] |
| C2 | vigia | C→2; H→`[]`; conjunto vazio apesar da contagem | `ponto/services/vigia_pareador_celula.py:76`; teste específico [não verificado] |
| L | vigia | C→0; não H; **descarta a lista existente** | `ponto/services/vigia_pareador_celula.py:76`; teste específico [não verificado] |
| D | vigia | se C truthy, conjunto(H); se C falsy, conjunto vazio | `ponto/services/vigia_pareador_celula.py:76`; teste específico [não verificado] |
| V | vigia | C controla leitura H; conjunto usado para posicionar bordas | `ponto/services/vigia_pareador_celula.py:76`; teste `ponto/tests/test_vigia_pareador_celula.py:44`, `ponto/tests/test_vigia_pareador_celula.py:47` |
| I | vigia | exemplo lista truthy: C levanta | `ponto/services/vigia_pareador_celula.py:76`, `escala/services/ata.py:90`; teste específico [não verificado] |
| N | cluster | `None→{}`; não M; retorna None=NAO MEDIDO | `ponto/services/cluster_espurio.py:27`; teste `escala/tests/test_tipo_item3_v5_expand.py:148` |
| S={} | cluster | não M; retorna None | `ponto/services/cluster_espurio.py:31`; teste específico [não verificado] |
| C2 | cluster | M→None; retorna None | `ponto/services/cluster_espurio.py:31`; M isolado testado `escala/tests/test_tipo_item3_v5_expand.py:124`; leitor [não verificado] |
| L | cluster | M→lista; só mede se `lampadas` também lista | `ponto/services/cluster_espurio.py:32`; teste específico [não verificado] |
| D | cluster | M→lista crua ou None conforme tipo; decide assinatura por rajada/missing | `ponto/services/cluster_espurio.py:34`, `escala/services/ata.py:149`; teste específico [não verificado] |
| V | cluster | M→lista; >=2 válidas, <=15min e missing → assinatura; senão `()` | `ponto/services/cluster_espurio.py:36`; teste `escala/tests/test_tipo_item3_v5_expand.py:143` |
| I | cluster | exemplo lista truthy: M levanta | `ponto/services/cluster_espurio.py:31`, `escala/services/ata.py:90`; teste específico [não verificado] |
| N | cartório/insumos | não C → `(False,False)` | `ponto/services/cartorio.py:841`; teste `escala/tests/test_tipo_item3_v5_expand.py:104` |
| S={} | cartório/insumos | não C → `(False,False)` | `ponto/services/cartorio.py:841`; teste específico [não verificado] |
| C2 | cartório/insumos | sem lâmpadas: C→2 → `(None,None)` | `ponto/services/cartorio.py:835`; teste equivalente n=3 `escala/tests/test_selo_b5f3_lavra_batida.py:79` |
| L | cartório/insumos | sem lâmpadas: C→0 → `(False,False)`; com lâmpadas, não C | `ponto/services/cartorio.py:835`; teste específico [não verificado] |
| D | cartório/insumos | sem lâmpadas: C→c; truthy→None/None, falsy→False/False | `ponto/services/cartorio.py:841`; teste específico [não verificado] |
| V | cartório/insumos | sem lâmpadas: C→c; com lâmpadas, ramo dos marcos | `ponto/services/cartorio.py:841`, `ponto/services/cartorio.py:844`; teste específico V+órfãs [não verificado] |
| I | cartório/insumos | exemplo lista truthy: `.get` falha antes de C; falsy normaliza para{} | `ponto/services/cartorio.py:834`; teste específico [não verificado] |
| N | tripwire | Mapping? não → None; guarda pula, sem R | `ponto/management/commands/tripwire_celulas.py:261`; teste específico [não verificado] |
| S={} | tripwire | guarda pula, sem R | `ponto/management/commands/tripwire_celulas.py:264`; teste específico [não verificado] |
| C2 | tripwire | se >=2 marcos: R.horas→None; parâmetro cai em `orfas or []` | `ponto/management/commands/tripwire_celulas.py:267`, `ponto/management/commands/tripwire_celulas.py:168`; teste estado×leitor [não verificado] |
| L | tripwire | se guarda permite: R.horas→lista; peneira por HH:MM | `ponto/management/commands/tripwire_celulas.py:267`; predicado isolado testado `escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:434`; ligação do estado [não verificado] |
| D | tripwire | R.horas→cru; `or []` colapsa valor falsy; valor truthy é iterado | `ponto/management/commands/tripwire_celulas.py:168`; teste estado×leitor [não verificado] |
| V | tripwire | R.horas→lista; peneira e confirmação posterior por instante | `ponto/management/commands/tripwire_celulas.py:267`, `ponto/management/commands/tripwire_celulas.py:283`; prova integral estado×leitor [não verificado] |
| I | tripwire | não Mapping→None; guarda pula, sem R | `ponto/management/commands/tripwire_celulas.py:261`; teste específico [não verificado] |

A matriz é a mesma linha de base para os dois desenhos; os retornos dos desenhos propostos ainda não existem nesta cópia. O tipo atual é `LeituraOrfas` com `.horas: list|None`, `.quantidade: int|None` e `.estado: str`, sem proibição de valor-verdade declarada. (`escala/services/ata.py:81`)

Além dos três `None→{}` do veredito, há `cluster_espurio.py:27`, `leitor_celula.py:384` e `cartao_pela_celula.py:288`; `juiz_batida.py:158` transforma todo não Mapping em `{}`. Isso afeta o nome/caminho observado antes do acessor, independentemente da escolha SELO/TIPO. (`ponto/services/cluster_espurio.py:27`, `escala/services/leitor_celula.py:384`, `relatorios/cartao_pela_celula.py:288`, `ponto/juiz_batida.py:158`)

Os testes de estados do acessor cobrem None, ausência das chaves, só contagem/lista, descasamento, inclusive lista substituída por string; isso **não é prova do produto estado×cada leitor**. (`escala/tests/test_tipo_item3_v5_expand.py:316`, `escala/tests/test_tipo_item3_v5_expand.py:330`, `escala/tests/test_tipo_item3_v5_expand.py:338`)

## 13/11 12:32 · sessao f4ef51503017 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos somente por leitura:

- **Escritor da grade confirmado:** `_cj` é cópia em memória; recebe ata com `lampadas` e `orfas`, sem `n_orfas`, e alimenta `turnos_do_colab`. Essa ata corresponde a `LEGADO_SO_LISTA`; o censo consulta somente `CelulaDia.objects`, portanto não inclui esse universo transitório. `app/escala/utils.py:1889`, `app/escala/utils.py:1890`, `app/escala/utils.py:1898`, `app/escala/utils.py:1901`, `app/escala/services/ata.py:100`, `app/escala/services/ata.py:228`.
- **Retirar `orfas_instantes` evita a diferença causada exclusivamente pela futura adição dessa chave.** A comparação é do envelope inteiro; somente `precedencia` é conservada da ata anterior. Uma ata diferente pode atualizar `veredito_em`, salvar/emitir e registrar evento. No ramo de decisão administrativa anterior, atualiza ata/outbox e retorna sem renovar `veredito_em`. Quantidade real atingida: [não verificado]. `app/ponto/portas/celula.py:104`, `app/ponto/portas/celula.py:1003`, `app/ponto/portas/celula.py:1005`, `app/ponto/portas/celula.py:1040`, `app/ponto/portas/celula.py:1043`, `app/ponto/portas/celula.py:1045`.
- **None→{} confirmado nos três pontos citados**, mas materialização pode sair antes de chamar o acessor (`not lamp`), e chamados pode sair por ausência da chave `lampadas`; não afirmar que todos executam classificação como legado em toda chamada. `app/ponto/services/vigia_pareador_celula.py:74`, `app/ponto/services/vigia_pareador_celula.py:76`, `app/chamados/services/materializacao.py:173`, `app/chamados/services/materializacao.py:176`, `app/chamados/juizes.py:494`, `app/chamados/juizes.py:506`.
- **Selo atual só examina `BoolOp(Or)`**, reconhecendo acessor direto e atributos de nomes ligados a `leitura_orfas`; não há propagação interprocedural. O resíduo passa `.horas` ao parâmetro que executa `or []`. `app/escala/tests/test_tipo_item3_selo_classe_fallback.py:76`, `app/escala/tests/test_tipo_item3_selo_classe_fallback.py:86`, `app/escala/tests/test_tipo_item3_selo_classe_fallback.py:124`, `app/ponto/management/commands/tripwire_celulas.py:267`, `app/ponto/management/commands/tripwire_celulas.py:168`.
- **`__bool__` apenas em `LeituraOrfas` não resolve:** `.horas` e `.quantidade` continuam campos crus `list|None`/`int|None`; o mesmo resíduo permanece se os campos não forem protegidos. Proibir iteração também altera comprehensions atuais. `app/escala/services/ata.py:81`, `app/escala/services/ata.py:83`, `app/escala/services/leitor_celula.py:194`, `app/ponto/juiz_batida.py:163`, `app/chamados/services/materializacao.py:182`.
- **Mesmo um valor protegido contra bool não obriga consulta ao estado:** exemplo hipotético `return [] if x is None else []` não solicita bool/len/iter. API proposta para extração explícita e seu policiamento: [não verificado].
- **RED por AttributeError:** o conteúdo do log só foi fornecido no prompt; não encontrei o artefato na cópia. [não verificado].
- **Custo próprio do (A):** seis métodos de teste têm chamadas/asserts diretamente ligados ao paralelo; há também uma fixture de classe com a chave. `app/escala/tests/test_tipo_item3_v5_expand.py:71`, `app/escala/tests/test_tipo_item3_v5_expand.py:81`, `app/escala/tests/test_tipo_item3_v5_expand.py:87`, `app/escala/tests/test_tipo_item3_v5_expand.py:99`, `app/escala/tests/test_tipo_item3_v5_expand.py:106`, `app/escala/tests/test_tipo_item3_v5_expand.py:198`, `app/escala/tests/test_tipo_item3_v5_expand.py:135`.

O instrumento retornou **79 operações reconhecidas de topo, 11 chaves e 27 arquivos**, com **58 lacunas totais**; é dimensão, não censo completo. O próprio instrumento declara que inclui o acessor, mas não reconheceu seus parâmetros como origem da ata; os acessos de `ata.py` precisam da complementação manual já enviada. `bin/leitores_de_chave.py:50`, `bin/leitores_de_chave.py:53`, `bin/leitores_de_chave.py:64`, `bin/leitores_de_chave.py:739`, `app/escala/services/ata.py:92`.

O mapa das 12 chaves e a lista das 40 leituras reconhecidas de `lampadas` foram enviados nas mensagens anteriores, com todas as referências; estas 40 não equivalem a 40 leitores de órfãs. `bin/leitores_de_chave.py:851`, `app/ponto/services/lastro.py:89`, `app/relatorios/cartao_pela_celula.py:290`.

## 13/11 12:28 · sessao 9e24452e1e42 · HJ-090 · censo · origem=Code

Censo da cópia: execução efetiva desse código em produção, quantidade de períodos divergentes e valores atuais dos registros: **[nao verificado]**.

1. **Escritores de `dias_concedidos` e `dias_vendidos`**

| Escritor | Lê → altera → grava | `atomic` | `select_for_update` do período |
|---|---|---|---|
| `registrar_gozo_historico` | Sim, sobre objeto recebido: `concedidos=min(direito, concedidos+ag.dias)`; `vendidos+=vendidos`. | Próprio. | Não. [services.py:270–302](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:270) |
| `executar_agendamento` | Sim, sobre objeto recebido: cria agendamento e faz `concedidos+=dias`. | Próprio. | Não; a consulta de duplicata também não trava. [services.py:306–326](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:306) |
| `agendar_ferias_porta` | Delega o gozo ao anterior; relê com `refresh_from_db` e faz `vendidos+=dias_vendidos`. | Próprio, envolvendo gozo e venda. | Não; `refresh_from_db` não acrescenta trava. [services.py:156–165](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:156) |
| `cancelar_agendamento_dp` | Sim, sobre linha relida: `concedidos-=vivo.dias`. | Próprio. | **Sim**, via `_travar_agendamento`. [services.py:379–385](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:379) |
| `executar_cancelamento_admin` | Sim, sobre linha relida: `concedidos-=vivo.dias`. | Próprio. | **Sim**, pelo mesmo helper. [services.py:402–417](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:402) |
| `_trocar_janela` | Sim: `concedidos=concedidos-vivo.dias+novos_dias`. | Próprio. | **Sim**, já nesta cópia. [services.py:433–452](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:433) |
| `aprovar_solicitacao` | Sim: lê período, cria agendamento e faz `concedidos+=solic.dias`. | Sem `atomic` próprio; o chamador de produção encontrado fornece `atomic`. | **Não do período**; a view trava a **solicitação**. [services.py:469–508](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:469), [views.py:739–753](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/views.py:739) |
| `vender_dias_do_periodo` | Sim, sobre objeto recebido: `vendidos+=dias_v`. | Sem `atomic` próprio ou no chamador encontrado. | Não. [services.py:580–584](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:580), [views.py:938–958](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/views.py:938) |

O helper trava primeiro o agendamento, recusa pelo status relido e depois trava o período. Portanto, a proteção FERIAS-CANCELAR-2X já existe nos dois cancelamentos e na troca de janela. Cancelado é recusado; concluído também é recusado no cancelamento comum, mas pode entrar no cancelamento administrativo especial. (`app/ferias/services.py:357–369,380,406–407`; `app/ferias/models.py:302–305`)

Há também **inicialização**: `garantir_periodo_aquisitivo` usa `get_or_create`, recebendo os dois contadores zerados da view; a criação dos ciclos está em `atomic`. Quando o período existe, essa porta atualiza somente status/carimbo. (`app/ferias/services.py:254–266`; `app/ferias/views.py:165,181–185`)

As migrations dos campos são `CreateModel` com concedidos zero e `AddField` com vendidos zero; não calculam soma de agendamentos. O admin de férias registra apenas `Afastamento`; o `save` de período só verifica sobreposição e delega ao Django, sem recompor contadores. (`app/ferias/migrations/0001_initial.py:39–51`; `app/ferias/migrations/0004_periodoaquisitivo_dias_vendidos.py:12–16`; `app/ferias/admin.py:1–5`; `app/ferias/models.py:201–203`)

Não foram localizados outros escritores desses contadores em commands, signals ou migrations de dados da cópia; escritores externos à cópia: **[nao verificado]**.

O clamp relevante é o `min` do histórico: o agendamento conserva os dias integrais, mas concedidos fica limitado ao direito. **Inferência aritmética do código:** partindo de concedidos zero/direito 30, um histórico de 33 grava agendamento 33 e concedidos 30. Ocorrência concreta em produção: **[nao verificado]**. (`app/ferias/services.py:281,298–302`)

Os cancelamentos subtraem os dias integrais, sem piso e sem devolver vendidos. Assim, **se** a linha travada tiver concedidos 30 e o agendamento cancelável tiver 33, o resultado gravado será **−3**, mesmo com a proteção contra segundo cancelamento. (`app/ferias/services.py:380–385,407–410`)

O BACKLOG ainda descreve `_trocar_janela` como sem trava e aprovação como sem `atomic`; a implementação atual já apresenta trava na primeira e transação fornecida pela view na segunda. (`app/docs/BACKLOG.md:1849`; `app/ferias/services.py:433–434`; `app/ferias/views.py:739–753`)

2. **Leitores e ESMERIL-SEMANTICA**

O significado central é **concedidos = gozo reservado/consumado; vendidos separado**. O saldo é `direito − concedidos − vendidos`, sem clamp. Nenhuma dessas properties recompõe concedidos pela soma dos agendamentos. (`app/templates/ferias/_secao_colab.html:49–51`; `app/ferias/models.py:216–239`)

Além das leituras dos próprios escritores inventariadas acima, os leitores diretos encontrados são:

| Leitor | O que usa ou pressupõe |
|---|---|
| `status_de_periodo` | Calcula direito−concedidos−vendidos; saldo **≤0**, inclusive negativo, vira `concedido`. (`app/ferias/models.py:95–102`) |
| `dias_saldo` | Retorna a diferença crua; admite negativo. (`app/ferias/models.py:216–217`) |
| `saldo_para_remarcar` | Retorna saldo+`agendamento.dias`; pressupõe que a reserva antiga está debitada em concedidos. Não verifica soma, status ou vínculo nesse método. (`app/ferias/models.py:219–222`) |
| `pode_agendar` | Exige status armazenado `adquirido` e saldo positivo. (`app/ferias/models.py:230–231`) |
| `dias_vendaveis` / `pode_vender` | `max(0,...)` limita somente o teto restante de venda; vender exige adquirido, teto positivo e saldo positivo. (`app/ferias/models.py:234–239`) |
| KPI/filtro `sem_agend` | Equipara adquirido+concedidos zero a “sem agendamento”; não consulta existência/soma e não considera vendidos nesse filtro. (`app/ferias/views.py:31,60`; `app/relatorios/services.py:290`) |
| `_validar_agendamento` | Compara pedido ao saldo acumulado; consulta não cancelados para duplicata/fracionamento, sem recompor saldo. (`app/ferias/views.py:227–243`) |
| `checar_ferias_com_abono` | Compara gozo+venda ao saldo; `saldo_depois` também não tem piso. (`app/ferias/services.py:195–205`) |
| Aprovação de solicitação | Compara pedido ao saldo do primeiro período adquirido; excesso gera aviso sem materialização naquele ramo. (`app/ferias/services.py:483–506`) |
| Correção/remarcação | Compara novos dias a `saldo_para_remarcar`. (`app/ferias/views.py:478–480,680–682`) |
| Auditoria administrativa | Fotografa concedidos antes/depois de cancelar, desfazer concluída e corrigir datas. (`app/ferias/views.py:368,383,416,427,486,518`) |
| Edição do período | Impede direito menor que concedidos; **não acrescenta vendidos nessa guarda**. (`app/ferias/views.py:812–813`) |
| Venda pela tela | Compara venda ao teto vendável e ao saldo, separadamente. (`app/ferias/views.py:950–953`) |
| `situacao_de_ferias` | Delega à property; detecta negativo como inconsistência, mas soma somente saldos positivos e devolve somente períodos positivos. (`app/ferias/situacao.py:68–69,178–189,296–297`) |
| Avisos vencido/vencendo | Consideram somente saldo positivo. (`app/ferias/situacao.py:192–216`) |
| `pautas_de_ferias` | Usa a property; negativo entra em cadastro inconsistente. (`app/ferias/management/commands/pautas_de_ferias.py:73,79–80,96–97`) |
| `alertar_ferias_vencendo` | Guarda saldo cru após sincronizar status e o apresenta como dias a usar. (`app/ferias/management/commands/alertar_ferias_vencendo.py:31–41,71–83`) |
| IA: seleção/defaults | Seleciona períodos com saldo positivo, sem exigir adquirido nessa seleção; duração padrão `min(30,saldo)`. (`app/core/views_ia.py:258–272,489–503`) |
| IA: “vender o resto” | `max(0,saldo−dias)` corta a **venda proposta**, não o saldo individual. (`app/core/views_ia.py:532–538`) |
| API de situação | Publica o total/lista já filtrados por `situacao_de_ferias`; mantém os avisos. (`app/api/views_mensageria.py:835–846`) |
| API geral/ficha | Publica saldo cru dos períodos selecionados para vencimento. (`app/api/views_mensageria.py:954,1103`) |
| PDF/CSV | Exibe saldo cru, sem recompor soma. (`app/relatorios/services.py:316`; `app/relatorios/views.py:370`) |
| Tela de saldo | Exibe concedidos e saldo cru; botão exige adquirido+saldo positivo. (`app/templates/ferias/saldo.html:37,41,46`) |
| Seção do colaborador | Exibe “Gozo”, saldo e vendidos separados; formulário usa `pode_agendar`, saldo como valor/max; negativo cai na mensagem genérica de indisponibilidade. (`app/templates/ferias/_secao_colab.html:49–57,67–68,77`) |
| Listagem, formulário individual, lote e relatório HTML | Exibem saldo cru; formulário individual também usa saldo como valor/max. (`app/templates/ferias/_tabela_ferias.html:27`; `app/templates/ferias/agendar.html:7,22`; `app/templates/ferias/agendar_lote.html:40`; `app/templates/relatorios/ferias_vencendo.html:48`) |

**Não é semanticamente uniforme em todos os usos:**

- A tela distingue gozo de vendidos, mas o aviso de inconsistência chama **concedidos+vendidos** de “dias gozados”. (`app/templates/ferias/_secao_colab.html:49–51`; `app/ferias/situacao.py:178–183`)
- O saldo individual negativo aparece nas telas citadas; o resumo/API **omite negativos da soma e da lista**, embora produza aviso. (`app/templates/ferias/saldo.html:41`; `app/ferias/situacao.py:178–189,296–297`; `app/api/views_mensageria.py:835–846`)
- O docstring de `saldo_total` fala em períodos adquiridos; a implementação carrega todos os períodos e filtra apenas saldo positivo. (`app/ferias/situacao.py:146,162–163,296–297`)
- A igualdade concedidos=soma dos não cancelados é uma pressuposição operacional das contas de devolver/substituir dias, **não uma igualdade conferida por esses leitores**. (`app/ferias/models.py:217–222`; `app/ferias/services.py:381,408,436`)

3. **Duas views, entrada da porta e demais chamadores**

`editar_datas_admin` consulta `saldo_para_remarcar` em 478 e chama `corrigir_datas_agendamento`; `editar_agendamento` consulta em 680 e chama `remarcar_agendamento`. Ambos os wrappers encaminham diretamente a `_trocar_janela`. (`app/ferias/views.py:478–503,680–688`; `app/ferias/services.py:457–466`)

A porta recebe **quatro argumentos**: objeto de agendamento, novo início, novo fim e novos dias. Não recebe saldo, request, usuário ou motivo. Ela relê o agendamento pelo PK e obtém o período vinculado à linha relida. (`app/ferias/services.py:422,434`; `app/ferias/services.py:364–368`)

O ponto correspondente à descida da pergunta descrita no aval fica **após `_travar_agendamento`, na linha 434, antes da alteração de concedidos na 436 e da substituição de `vivo.dias` na 437**: ali existem período e agendamento relidos sob trava, ainda com a reserva antiga. Atualmente esse bloco não consulta `saldo_para_remarcar`. (`app/ferias/services.py:433–437`)

Os dois chamadores diretos de produção encontrados de `_trocar_janela` são esses wrappers; os testes também exercitam ambos. (`app/ferias/services.py:459,466`; `app/ferias/test_cancelar_2x.py:239–248`; `app/ferias/test_remarcar_limpa_janela_f346.py:198`)

4. **Status que entram na soma “não cancelados”**

O catálogo contém somente **`agendado`, `em_gozo`, `concluido`, `cancelado`**. Portanto, excluir cancelados inclui os outros três, inclusive concluídos; o fracionamento já usa explicitamente esse trio. Não há quinto status declarado. Valores fora do catálogo existentes em produção: **[nao verificado]**. (`app/ferias/models.py:293–298,360–363`)

`recusado` pertence a **`confirmacao`**, não ao status do agendamento. A resposta de recusa grava apenas confirmação; não cancela nem devolve dias, portanto permanece incluída na definição “não cancelados”. (`app/ferias/models.py:321–324`; `app/ferias/services.py:567–575`)

“Agendamentos vivos” em `situacao_de_ferias` é outro recorte: apenas agendado/em_gozo. Não equivale a “não cancelados”, que inclui concluído. (`app/ferias/situacao.py:164–168`; `app/ferias/models.py:293–298`)

5. **Trilha e molde DRY/apply/reversão**

**Existe infraestrutura compatível:** `LogAuditoria` guarda modelo, objeto, descrição, JSON antes/depois, usuário/ator e data. `core.observ.evento` aceita esses elementos sem request, identifica processo `manage.py` como `sistema:<comando>` e retorna registro ou `None`. (`app/core/models.py:295–307`; `app/core/observ.py:22–32,94–102,133–139`)

Essa porta é **best effort**, com savepoint próprio; falha da trilha não desfaz automaticamente o negócio. O adaptador `registrar_log` também captura falhas e não devolve o registro. (`app/core/observ.py:105,133–139`; `app/core/auditoria.py:8–29`)

Em férias, o molde de antes/depois/motivo já aparece nos cancelamentos administrativos e na correção de datas. Entretanto, os logs ficam **nas views, após a chamada ao serviço**, com o “antes” capturado anteriormente à trava; `_trocar_janela` não registra auditoria no seu próprio bloco. (`app/ferias/views.py:366–384,414–428,484–518`; `app/ferias/services.py:422–454`)

O molde vizinho mais completo encontrado é **`rederivar_minutos_h06_comp11`**, pela combinação dos seguintes elementos:

| Elemento existente | Evidência |
|---|---|
| DRY padrão, `--apply`, `--reverter SNAPSHOT`, autor obrigatório para escrita. | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:195–214` |
| Transação e releitura sob trava por linha; DRY retorna antes de chamar escritor. | Mesmo arquivo: `137–164`. |
| Snapshot antes da escrita; chamada à porta existente; conferência do depois. | Mesmo arquivo: `165–180`. |
| Auditoria antes/depois/motivo; trilha ausente provoca desfazimento da linha. | Mesmo arquivo: `181–192`. |
| Snapshot em `logs/h12h06b`, criação exclusiva e `flush/fsync`; confirmação posterior à aplicação. | Mesmo arquivo: `215,248–257,273–274`. |
| Reversão apenas de tentativas confirmadas, com guardas contra atos posteriores; listagem, resultado e contadores. | Mesmo arquivo: `152–159,220–241,261–288`. |

No próprio férias, `sincronizar_status_ferias` fornece DRY/apply, listagem, contador e escrita pela porta, mas seu corpo não contém snapshot/reversão. (`app/ferias/management/commands/sincronizar_status_ferias.py:20–57`)

Porta específica já existente para realinhar concedidos à soma e implementação do contador `ferias_concedidos_x_soma`: **não localizadas na cópia [nao verificado]**. O contador atual de divergência examina status de agendamentos/períodos. (`app/ferias/services.py:71–95`)

6. **Entidade de `#103` e `#114`**

**Não é possível identificar a entidade desses dois números do aval: [nao verificado].**

A ocorrência exata de `#103` encontrada no repositório está em documentação sobre **disputas**, junto de `#1257`; não estabelece vínculo com HJ-090 nem prova que seja o mesmo registro citado no aval. (`app/docs/RELATO-ARQUIVO.md:2834–2837`)

Não foram localizados `#114` exato, HJ-090 ou o arquivo `complemento_hj_090_1221.txt` na cópia que qualificassem esses IDs. Classificá-los como PK de período, agendamento, chamado ou HJ permanece **[nao verificado]**.

## 13/11 12:27 · sessao dc1313b29315 · HJ-090 · sonda · origem=Code

Não executei: `sonda2.py` não foi localizado na cópia. [nao verificado] [nao verificado]

Saída integral fornecida no enunciado:

```text
sonda_leitura: sonda2.py [092f19c733df6958] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

total 52
por origem {'agendamento': 1, 'historico': 51}
por status do periodo usado {'concedido': 40, 'em_curso': 12}
origem x status {'agendamento/concedido': 1, 'historico/concedido': 39, 'historico/em_curso': 12}
ag colab inicio dias status_ag origem pa_usado status_pa anteriores_com_saldo(pa:saldo)
12 170 2026-05-28 30 concluido agendamento 1019 concedido 1018:30
56 95 2026-07-13 30 concluido historico 350 concedido 349:30
57 247 2026-07-28 30 concluido historico 427 concedido 426:30
58 369 2026-08-17 30 concluido historico 1051 concedido 631:30
59 41 2026-07-13 30 concluido historico 74 concedido 73:30
61 125 2026-08-03 30 concluido historico 587 concedido 586:30
62 121 2026-08-11 30 concluido historico 547 concedido 546:30
63 163 2026-08-13 30 concluido historico 966 concedido 965:30
64 104 2026-07-20 30 concluido historico 398 concedido 397:30
65 128 2026-08-06 20 concluido historico 615 em_curso 614:30
66 138 2026-08-03 30 concluido historico 706 concedido 705:30
67 149 2026-08-06 30 concluido historico 813 concedido 812:30
74 70 2026-09-08 30 concluido historico 157 concedido 156:30
75 118 2026-09-16 30 concluido historico 534 concedido 533:30
76 136 2026-09-15 20 concluido historico 701 em_curso 700:30
79 357 2026-07-07 20 concluido historico 387 em_curso 386:30
81 191 2026-07-08 30 concluido historico 343 concedido 342:30
83 219 2026-07-16 30 concluido historico 384 concedido 383:30
84 198 2026-08-05 30 concluido historico 1005 concedido 1004:30
87 355 2026-08-12 20 concluido historico 946 em_curso 945:30
88 367 2026-08-06 20 concluido historico 1054 em_curso 1047:30
89 189 2026-08-10 30 concluido historico 329 concedido 328:30
91 340 2026-09-02 30 concluido historico 294 concedido 293:30
92 218 2026-09-01 30 concluido historico 646 concedido 645:30
93 187 2026-09-08 30 concluido historico 412 concedido 411:30
94 193 2026-09-22 30 concluido historico 990 concedido 989:30
95 325 2026-08-12 30 concluido historico 187 concedido 186:30
96 188 2026-09-02 30 concluido historico 789 concedido 788:30
97 245 2026-08-19 30 concluido historico 775 concedido 774:30
98 354 2026-08-10 30 concluido historico 650 concedido 649:30
99 341 2026-08-20 30 concluido historico 133 concedido 132:30
100 371 2026-09-16 24 concluido historico 1056 em_curso 467:30
101 373 2026-09-02 24 concluido historico 1058 em_curso 664:30
102 306 2026-10-01 30 concluido historico 216 concedido 215:30
103 344 2026-10-06 33 concluido historico 639 concedido 638:30
104 347 2026-10-08 30 concluido historico 375 concedido 374:30
105 303 2026-10-20 30 em_gozo historico 496 concedido 495:30
106 274 2026-10-22 30 em_gozo historico 1059 concedido 207:30
107 374 2026-09-15 30 concluido historico 1060 concedido 601:30
108 236 2026-10-05 30 concluido historico 873 concedido 872:30
109 168 2026-10-22 28 em_gozo historico 997 em_curso 996:30
110 119 2026-10-22 30 em_gozo historico 536 concedido 535:30
111 215 2026-10-05 30 concluido historico 544 concedido 543:30
112 366 2026-09-01 30 concluido historico 1057 concedido 408:30
114 127 2026-09-02 31 concluido historico 598 concedido 597:30
115 101 2026-11-09 20 em_gozo historico 394 em_curso 393:30
116 61 2026-11-11 30 em_gozo historico 68 concedido 67:30
117 80 2026-11-16 24 agendado historico 238 em_curso 237:30
118 165 2026-11-24 20 agendado historico 972 em_curso 971:30
119 148 2026-12-14 20 agendado historico 809 em_curso 808:30
120 93 2026-12-21 30 agendado historico 1063 concedido 319:30
122 309 2026-10-29 30 em_gozo historico 652 concedido 1066:30
ERROS 0
```

## 13/11 12:26 · sessao 96d571efac7b · TIPO-NA-FONTE · censo · origem=Code

Referências às linhas das transcrições fornecidas, contando linhas vazias e excluindo os separadores `== anexo... ==`. Em **(a)**, distingo descrever um comportamento de denunciá-lo como achado.

1. **(a) Codex não apontou o achado, com o tema presente.**  
   Pergunta: “Algum leitor trata LEGADO por fallback mudo [...] em vez de pelo nome?” (`preverif.q:6`). Resposta: o tripwire “Entrega `None`”, mas sem denunciar o fallback posterior por parâmetro; a denúncia ficou nos acessores e em `_orfas_da_ata` (`preverif.r:17,35`). A triagem registra esse terceiro resíduo somente na matriz posterior ao opus; os anteriores foram aceitos e curados, não descartados (`triagem_linhas.md:1,4`).

2. **(a) Codex não apontou a irregularidade, com o tema presente.**  
   Pergunta: “QUEM ESCREVE e QUEM LE” inclui expressamente `ata['orfas_instantes']`; também pergunta por “derivacao paralela de fonte unica” (`preverif.q:10,12`). Resposta: descreve o campo adicional e seus leitores em testes, mas não denuncia a proibição nem a onda de regravações; chega a registrar “ISO entra em chave adicional” (`preverif.r:80,93`). **A presença da proibição literal do aval `tipo_na_fonte_o168_teto` nos anexos efetivamente entregues ao Codex é [não verificado]**: o conteúdo desses avais não foi transcrito (`preverif.q:2`; `veredito.md:4`). [nao verificado]

3. **(a) Codex não apontou o achado, com o tema presente.**  
   Pergunta: “QUEM ESCREVE” e “2o escritor fora da porta” (`preverif.q:10,12`). Resposta: “O caminho visível de escrita continua passando por `gravar_orfas` e pela lavratura”; não identifica o escritor da grade nem sua exclusão do universo do censo (`preverif.r:102`). Na segunda resposta, “Segundo escritor produtivo fora da porta: Não identificado”, expressamente limitado ao delta (`preverif2.r:85,88`). A triagem nomeia o escritor da grade na matriz posterior ao opus (`triagem_linhas.md:1`).

4. **(a) Codex descreveu o comportamento, mas não apontou a perda do estado.**  
   Perguntas: preservação para “ata NULA”, tratamento pelo nome e comportamento do vigia para “ata None” (`preverif.q:5,6`; `preverif2.q:6`). Resposta: “Todos viram `{}`” e “`None` é normalizado antes da porta”, apresentados como preservação do resultado; não denuncia `NAO_LAVRADA` sendo classificada como `LEGADO_SEM_CHAVES` (`preverif2.r:28,32,33`). Não há descarte fundamentado desse achado na triagem anterior; ele aparece na matriz posterior (`triagem_linhas.md:1,4,6`).

5. **(c) Fora do alcance daquela verificação: o selo específico veio depois.**  
   Pergunta: a segunda volta é “so o delta” até `ed4b38773` e pergunta pelo selo `ChaveVelhaSoNaPorta` (`preverif2.q:1,2,6`). Resposta: examina esse selo de constantes literais e suas exclusões (`preverif2.r:45,47`). O selo AST `test_tipo_item3_selo_classe_fallback.py` é registrado posteriormente, após a segunda reprovação e sua cura; suas brechas constam do veredito posterior (`triagem_linhas.md:3,4`; `veredito.md:7`).

6. **(c) Fora do alcance: causa efetiva do RED.**  
   Pergunta: “qual teste fica vermelho em cada?” (`preverif.q:9`). Resposta: “Por inspeção das asserções; execução dos mutantes [não verificado]” (`preverif.r:65`). Isso não certifica que o RED ocorreu por valor; o `AttributeError` é informação do veredito fornecido (`veredito.md:8`). [nao verificado]

7. **(c) Fora do alcance, tomando o número agregado como certificação do pouso.**   [nao verificado]
   Perguntas: preservação por leitor e publicação do contador no tripwire (`preverif.q:5,7`). Respostas: DIFF na frota, suíte e mutantes permanecem “[não verificado]” (`preverif.r:3,109`; `preverif2.r:94`). A própria triagem determina “FORA DO ALCANCE — DIFF por leitor = a cadeia” (`triagem_linhas.md:6`). **Ressalva do agrupamento:** a exigência específica de publicação em `gerar_estado.py` não aparece nas perguntas transcritas; sua inclusão em anexos não transcritos é [não verificado] (`preverif.q:7`; `preverif2.q:5–9`). A divergência da árvore certificada é informação posterior do veredito (`veredito.md:9`). [nao verificado]

Não atribuo **(b)**: a triagem aceita e cura os bloqueantes anteriores; “não bloqueantes → backlog” não identifica descarte fundamentado destes achados específicos (`triagem_linhas.md:4,6`). Tampouco atribuo **(d)**: afirmar que ninguém da sessão viu anteriormente excederia o registro fornecido **[não verificado]**.

**Contagem, uma classe por número:** **(a) 4; (b) 0; (c) 3; (d) 0; (e) 0. Total: 7**, com a ressalva do item agregado 7 (`veredito.md:3–9`).

## 13/11 12:26 · sessao 4ca8c4e65a17 · - · - · origem=Ronald (tmux)

Censo dos leitores diretos de produção e semântica:

| Leitor | Uso / hipótese |
|---|---|
| `app/ferias/models.py:95` | `status_de_periodo` calcula direito − concedidos − vendidos; saldo ≤0 vira `concedido`, portanto saldo negativo recebe o mesmo status de saldo zerado (`:96-97`). |
| `app/ferias/models.py:217` | `dias_saldo` devolve direito − concedidos − vendidos sem clamp; concedidos e vendidos são gastos distintos. |
| `app/ferias/models.py:222` | `saldo_para_remarcar` devolve saldo + dias do agendamento; pressupõe que os dias desse agendamento estão debitados no campo concedidos, sem conferir soma/status/vínculo. |
| `app/ferias/models.py:231` | `pode_agendar` exige status armazenado `adquirido` e saldo positivo. |
| `app/ferias/models.py:235` e `:239` | `dias_vendaveis` corta em zero apenas o teto restante de venda; `pode_vender` exige adquirido, teto positivo e saldo positivo. Não corta `dias_saldo`. |
| `app/ferias/views.py:31` e `:60` | KPI/filtro `sem_agend` equiparam adquirido + concedidos=0 a “sem agendamento”; não consultam existência/soma dos agendamentos e não consideram vendidos nesse filtro. |
| `app/ferias/views.py:233-234` | `_validar_agendamento` compara pedido ao saldo armazenado; usa não cancelados para duplicata (`:235-243`), não para recalcular saldo. |
| `app/ferias/services.py:197-205` | `checar_ferias_com_abono` verifica teto de venda, gozo+abono ≤saldo; `saldo_depois` também fica sem clamp. |
| `app/ferias/views.py:368`, `:383`, `:416`, `:427`, `:486`, `:518` | Fotografias antes/depois de concedidos para auditoria em cancelar-admin, desfazer concluída e corrigir datas. |
| `app/ferias/views.py:478-480` e `:680-682` | Correção administrativa e remarcação comparam novos dias ao saldo+reserva antiga fornecido por `saldo_para_remarcar`. |
| `app/ferias/views.py:812-813` | Editar período impede direito<concedidos; **não inclui vendidos nessa guarda**, apesar de a fórmula do saldo subtrair os dois. |
| `app/ferias/views.py:950-953` | Venda verifica teto de venda e saldo restante, separadamente. |
| `app/ferias/services.py:489-490` | Aprovação de solicitação compara solic.dias ao saldo do primeiro período adquirido; excesso vira aviso, não materializa agendamento nesse ramo (`:483-493`). |
| `app/ferias/situacao.py:68-69` | `_saldo` delega integralmente a `dias_saldo`. |
| `app/ferias/situacao.py:178-183` | Negativo gera aviso de cadastro inconsistente; contudo o texto chama **concedidos+vendidos** de “mais dias gozados”, mistura terminológica. |
| `app/ferias/situacao.py:192-216` | Avisos de vencido/vencendo só consideram saldo>0. |
| `app/ferias/situacao.py:296-297` | `saldo_total` soma só saldos positivos e `periodos` devolve só PAs com saldo>0. Valores negativos são **omitidos da agregação/lista**, embora detectados no aviso. O docstring fala “períodos já adquiridos” (`:146`), mas implementação carrega todos os PAs (`:162-163`) e só filtra pelo saldo (`:296-297`), sem status ou aquisição. |
| `app/ferias/management/commands/pautas_de_ferias.py:73`, `:80`, `:96-97` | `_saldo` usa fórmula do modelo; vencendo exige >0; saldo<0 entra em cadastro inconsistente. Texto nomeia explicitamente “saldo negativo” (`:136-138`). |
| `app/ferias/management/commands/alertar_ferias_vencendo.py:36`, `:41` | Guarda o saldo cru nos alertas após sincronizar status (`:31-38`); push individual mostra o número como “dias a usar” (`:71-83`). |
| `app/core/views_ia.py:261-269` | `_periodo_do_colab` seleciona só saldos>0 e informa o número de dias; sem filtro por status. |
| `app/core/views_ia.py:492-503` | Defaults da IA escolhem PA mais antigo com saldo>0; duração padrão min(30,saldo); sem filtro por status nessa escolha. |
| `app/core/views_ia.py:532` | “Vender o resto” usa max(0,saldo−dias), clamp da **quantidade proposta para venda**, não do saldo. |
| `app/api/views_mensageria.py:835-841` | API `situacao-ferias` expõe `saldo_dias=saldo_total` e os períodos filtrados de `situacao_de_ferias`; portanto o total/lista omite negativos, mantendo avisos (`:846`). |
| `app/api/views_mensageria.py:954` e `:1103` | Payloads gerais/ficha de vencimento expõem saldo cru dos PAs selecionados como vencendo; não recalculam soma. |
| `app/relatorios/services.py:290` | PDF “sem agendamento” usa o mesmo adquirido+concedidos=0. |
| `app/relatorios/services.py:316` | PDF mostra saldo cru como string. |
| `app/relatorios/views.py:370` | CSV mostra saldo cru; universo anterior é filtrado pelo juiz `vencendo` (`:358-359`). |
| `app/templates/ferias/saldo.html:37`, `:41`, `:46` | Exibe concedidos e saldo cru, inclusive negativo; botão só com adquirido+saldo>0. |
| `app/templates/ferias/_secao_colab.html:49-51` | Nomeia concedidos “Gozo” com tooltip “já agendados/gozados”, saldo “Direito − Gozo − Vendidos”, vendidos separados; número do saldo cru. |
| `app/templates/ferias/_secao_colab.html:57`, `:67-68`, `:77` | `pode_agendar` controla formulário; input recebe saldo como valor/max. Mensagem “sem saldo” exige ==0, negativo cai no genérico “Indisponível”. |
| `app/templates/ferias/_tabela_ferias.html:27` | Saldo cru na listagem. |
| `app/templates/ferias/agendar.html:7`, `:22` | Saldo cru no texto e valor/max do input. |
| `app/templates/ferias/agendar_lote.html:40` | Saldo cru no lote. |
| `app/templates/relatorios/ferias_vencendo.html:48` | Saldo cru no texto e `data-value`. |

Os leitores de saldo usam o acumulador `dias_concedidos`, não uma soma derivada de agendamentos (`app/ferias/models.py:217`, `app/ferias/views.py:233-235`). A semântica pretendida de concedidos como gozo reservado/consumado separado de vendidos está explícita na tela (`app/templates/ferias/_secao_colab.html:49-51`), mas não é idêntica em todos os usos: o aviso soma vendido ao que chama “gozado” (`app/ferias/situacao.py:178-183`), a edição do direito ignora vendido na guarda (`app/ferias/views.py:812-813`) e o total da API exclui períodos negativos (`app/ferias/situacao.py:296-297`; `app/api/views_mensageria.py:835-841`).

Status:

- Catálogo de `AgendamentoFerias` contém apenas `agendado`, `em_gozo`, `concluido`, `cancelado`; “não cancelados” abrange os três primeiros (`app/ferias/models.py:293-298`). O juiz por datas produz exatamente agendado/em_gozo/concluido e deixa cancelamento humano de fora (`app/ferias/models.py:273-285`).
- O fracionamento já conta explicitamente os três não cancelados (`app/ferias/models.py:360-363`). Não há quinto status declarado nesse catálogo (`app/ferias/models.py:293-298`); presença de valores fora do catálogo na produção [nao verificado].
- `recusado` existe em **confirmacao**, não em status (`app/ferias/models.py:321-324`). Recusar pelo colaborador só grava confirmacao=recusado; não cancela nem devolve dias (`app/ferias/services.py:567-575`). Logo continua não cancelado e continua contando pela definição solicitada.
- “Agendamentos vivos” de `situacao_de_ferias` significa apenas agendado/em_gozo (`app/ferias/situacao.py:164-168`), enquanto “não cancelados” também inclui concluído (`app/ferias/models.py:293-298`); são universos distintos.

## 13/11 12:26 · sessao 035078559140 · - · - · origem=Ronald (tmux)

Censo só leitura dos escritores encontrados na cópia:

| Escritor | Campos / conta | Lê-altera-grava? | `atomic` | `select_for_update` do período |
|---|---|---|---|---|
| `registrar_gozo_historico` | `concedidos=min(direito, concedidos+ag.dias)`; `vendidos+=vendidos` | Sim, objeto recebido | Próprio | Não — corpo inteiro usa `alvo` recebido. `app/ferias/services.py:270`, `:277`, `:295`, `:299`, `:302` |
| `executar_agendamento` | `concedidos+=dias` após criar agendamento | Sim, objeto recebido | Próprio | Não; consulta duplicata por existência, sem lock. `app/ferias/services.py:306`, `:315`, `:316`, `:321`, `:324`, `:326` |
| `agendar_ferias_porta` | Chama anterior e depois `vendidos+=dias_vendidos` | Sim; `refresh_from_db` antes da venda | Próprio, cobrindo gozo e venda | Não; refresh não usa lock. `app/ferias/services.py:156`, `:157`, `:161`, `:162`, `:165` |
| `cancelar_agendamento_dp` | `concedidos-=vivo.dias` | Sim, linha relida | Próprio | Sim, helper trava agendamento e período. `app/ferias/services.py:379`, `:380`, `:381`, `:383`, `:364`, `:367` |
| `executar_cancelamento_admin` | `concedidos-=vivo.dias` | Sim, linha relida | Próprio | Sim, mesmo helper. `app/ferias/services.py:402`, `:406`, `:407`, `:408`, `:410`, `:364`, `:367` |
| `_trocar_janela` | `concedidos=concedidos-vivo.dias+novos_dias` | Sim, linha relida | Próprio | **Sim, já nesta cópia**. `app/ferias/services.py:433`, `:434`, `:436`, `:452`, `:364`, `:367` |
| `aprovar_solicitacao` | Cria agendamento e `concedidos+=solic.dias` | Sim | Não próprio; **sim no único chamador de produção encontrado**, `avaliar_solicitacao` | Não do período; view trava somente `SolicitacaoFerias`. `app/ferias/services.py:469`, `:483`, `:503`, `:504`, `:506`; `app/ferias/views.py:739`, `:740`, `:753` |
| `vender_dias_do_periodo` | `vendidos+=dias_v` | Sim, objeto recebido | Não próprio, nem no único chamador de produção encontrado | Não. `app/ferias/services.py:580`, `:582`, `:584`; `app/ferias/views.py:938`, `:942`, `:958` |

Inicialização: `garantir_periodo_aquisitivo` cria período com `defaults` do chamador; a view passa ambos os contadores zerados e envolve a criação de ciclos em `atomic`; não é RMW desses contadores nem lock explícito de período. Se já existe, a porta só atualiza status/carimbo. `app/ferias/services.py:254`, `:259`, `:261`, `:266`; `app/ferias/views.py:165`, `:181`, `:185`.

Migrations encontradas para os campos são de esquema/default: `CreateModel` com `dias_concedidos=0` e `AddField` de `dias_vendidos=0`; esta última preenche o campo novo, sem calcular soma. Não encontrei migration de dados que escreva esses contadores; ausência global fora da cópia é [nao verificado]. `app/ferias/migrations/0001_initial.py:39`, `:47`; `app/ferias/migrations/0004_periodoaquisitivo_dias_vendidos.py:12`, `:16`.

O `save` de `PeriodoAquisitivo` apenas executa guarda de sobreposição e delega ao Django; não recalcula os contadores pela soma. `app/ferias/models.py:201`.

O Django admin do app registra somente `Afastamento`; não há escritor desses contadores nele. Não encontrei signal ou command com atribuição desses campos na cópia; execução em produção é [nao verificado]. `app/ferias/admin.py:1`, `:4`; `app/ferias/apps.py:3`.

**Clamp e cancelamento:**

- O único clamp direto encontrado em `dias_concedidos` é o `min` do histórico. Exemplo aritmético inferido do código: histórico de 33 dias num período de direito 30 e concedidos zero salva agendamento com 33 e contador com 30, criando diferença de 3; ocorrência real é [nao verificado]. `app/ferias/services.py:281`, `:298`, `:299`.
- Cancelamento não tem clamp nem piso: subtrai os dias inteiros do agendamento. Se a linha travada tiver 30 concedidos e o agendamento cancelável tiver 33, grava `-3`; o código não compara contador com `vivo.dias`. `app/ferias/services.py:380`, `:381`, `:383`; `:407`, `:408`, `:410`.
- FERIAS-CANCELAR-2X já está incorporado: cancelado é recusado antes de obter o período; DP/admin comum também recusam concluído; admin especial com `permitir_concluido=True` recusa somente cancelado. `app/ferias/services.py:364`, `:365`, `:367`, `:380`, `:406`, `:407`; `app/ferias/models.py:302`, `:304`, `:305`.
- Vendidos não são devolvidos pelos cancelamentos, pois ambos salvam somente `dias_concedidos/status`. `app/ferias/services.py:383`, `:410`.
- Não encontrei `max(0, concedidos...)` ou outro clamp na escrita desses contadores. O `max(0,...)` existente em `dias_vendaveis` é propriedade de leitura do teto de abono. `app/ferias/models.py:234`, `:235`.

**Divergência documental importante:** BACKLOG ainda enumera `_trocar_janela` como sem trava, mas a implementação atual já trava ambos os registros dentro de `atomic`; também chama `aprovar_solicitacao` de “sem atomic”, embora a view atual forneça transação e lock da solicitação. A porta continua sem transação própria e sem lock do período. `app/docs/BACKLOG.md:1849`; `app/ferias/services.py:433`, `:434`, `:367`, `:469`; `app/ferias/views.py:739`, `:740`, `:753`.

As chamadas reais do gozo histórico, agendamento individual/lote, cancelamento DP/admin e venda são delegações para essas portas, sem segundo escritor dos contadores nas views. `app/ferias/views.py:211`, `:264`, `:299`, `:330`, `:370`, `:418`, `:531`, `:575`, `:636`, `:753`, `:958`.

## 13/11 12:26 · sessao 4283216c4ef4 · - · - · origem=Ronald (tmux)

5. **Há infraestrutura de auditoria reutilizável; não há trilha de realinhamento de `PeriodoAquisitivo` identificada na porta atual.** `LogAuditoria` comporta modelo, objeto_id, descrição/motivo, JSON antes/depois, usuário, ator de sistema e data; `core.observ.evento(...)` é a porta que retorna o registro criado ou `None` em falha. [app/core/models.py:285; app/core/models.py:295; app/core/models.py:300; app/core/observ.py:94; app/core/observ.py:133; app/core/observ.py:139]

- **Service/comando sem request:** pode chamar `evento(acao, modelo, objeto_id, descricao=..., antes=..., depois=..., ator=...)`; o processo `manage.py` recebe ator `sistema:<comando>`. A escrita da trilha usa savepoint próprio e é **best effort**: falhar não desfaz automaticamente o negócio. [app/core/observ.py:22; app/core/observ.py:94; app/core/observ.py:105; app/core/observ.py:117; app/core/observ.py:133]
- **Adaptador para view:** `registrar_log(request, ..., valor_antes, valor_depois)` extrai IP/usuário e chama `evento`; captura exceção e não devolve a linha criada. [app/core/auditoria.py:8; app/core/auditoria.py:17; app/core/auditoria.py:22; app/core/auditoria.py:25]
- **Molde existente dentro de férias:** cancelamento admin registra status/dias/datas/concedidos antes e status/concedidos/ausências rejeitadas depois; correção de concluídas acrescenta justificativa; correção de datas também registra datas/dias/status/concedidos antes/depois e justificativa. Os três logs são feitos **na view, após a porta e refresh**, com a fotografia de “antes” coletada antes de entrar no escritor. [app/ferias/views.py:366; app/ferias/views.py:370; app/ferias/views.py:376; app/ferias/views.py:378; app/ferias/views.py:414; app/ferias/views.py:418; app/ferias/views.py:423; app/ferias/views.py:484; app/ferias/views.py:503; app/ferias/views.py:511]
- `_trocar_janela` recebe somente agendamento, início, fim e quantidade; seu bloco atual altera agendamento/período e retorna, sem chamar a auditoria nesse bloco. Portanto não há motivo/autor explícitos na assinatura dessa porta. [app/ferias/services.py:422; app/ferias/services.py:433; app/ferias/services.py:450; app/ferias/services.py:452; app/ferias/services.py:454]

**Molde mais próximo de DRY/apply com reversão em `logs/`:** `ponto/management/commands/rederivar_minutos_h06_comp11.py`, vizinho de férias, combina todos os elementos pedidos; a classificação “mais próximo” é comparação do código examinado. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:126; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:195; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:215]

- DRY sem flag; `--apply`; `--reverter SNAPSHOT`; `--por` obrigatório para escrever. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:196; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:199; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:205]
- Por linha, abre atomic, relê com `select_for_update`, verifica identidade/valor esperado; DRY retorna `mudaria` sem chamar escritor; apply fotografa antes e chama o escritor existente. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:149; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:163; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:165; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169]
- Exige trilha com antes/depois/motivo; se `evento` retorna `None`, lança exceção e desfaz a transação da linha. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:181; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:186; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:191]
- Snapshot `logs/h12h06b/snapshot_<ato>_<timestamp>.jsonl`, com flush/fsync; confirmação posterior ao commit identifica linhas efetivamente aplicadas; reversão confere identidade, valor e marcas de atos posteriores. Há resultado JSON, hash do snapshot, listagem e contadores. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:215; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:220; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:248; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:261; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:273; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:282; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:288]
- No próprio férias, `sincronizar_status_ferias` oferece DRY/apply, lista/contador e apply pela porta; não tem snapshot/reversão nesse comando. `pautas_de_ferias` também oferece DRY/apply, mas produz pautas, não realinha contadores. [app/ferias/management/commands/sincronizar_status_ferias.py:20; app/ferias/management/commands/sincronizar_status_ferias.py:37; app/ferias/management/commands/sincronizar_status_ferias.py:44; app/ferias/management/commands/sincronizar_status_ferias.py:55; app/ferias/management/commands/pautas_de_ferias.py:39; app/ferias/management/commands/pautas_de_ferias.py:82; app/ferias/management/commands/pautas_de_ferias.py:91]

6. **A entidade de `#103` e `#114` citados no aval é [nao verificado].** O texto fornecido não qualifica os números e não contém fotografia de linhas dessas entidades. [nao verificado]

- Busca exata no repositório por `#103`/`#114` com fronteira numérica, `HJ-090`, `FERIAS-UM-JUIZ`, `complemento_hj_090` e `concedidos_x_soma`: somente `#103` aparece, em contexto de **disputas**, junto de `#1257`; o trecho anterior identifica “disputa 2336” e perguntas vivas. Essa ocorrência não vincula o `#103` ao HJ-090 nem prova que seja o mesmo número do aval. [app/docs/RELATO-ARQUIVO.md:2834; app/docs/RELATO-ARQUIVO.md:2837]
- Não localizei o arquivo `complemento_hj_090_1221.txt`, `HJ-090`, `#114` exato nem referência dos dois números no app férias que estabeleça a entidade; ausência de correspondência positiva permanece [nao verificado].
- A referência documental pertinente encontrada é a fila **FERIAS-SALDO-SEM-TRAVA**, que enumera escritores e a ausência de locks/atomic no estado então documentado; não identifica `#103/#114`. [app/docs/BACKLOG.md:1849]
- Não apareceram `AGENTS.md` na árvore pesquisada nem nos ancestrais `/`, `/home`, `/home/ronald`, `/home/ronald/codex_ws` e raiz da cópia; existência de instruções adicionais fora desse alcance [nao verificado].

_4590 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
