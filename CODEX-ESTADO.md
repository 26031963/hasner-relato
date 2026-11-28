# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 23:27** · publicado em **27/11/2026 23:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 23:27 · sessao 739aeb1477ca · PISTA3-NA-ESTEIRA · leitura · origem=Code

## (a) Onde o repo assume um host só

| Sítio | Efeito com `pista` e `pista3` |
|---|---|
| **`bin/suite.sh` · sondagem e fila** | `ping`, `estado` e a coleta dos recibos históricos miram `pista`. Se esse host falha ou devolve estado ilegível, a rota vai direto à VM. O parser do `ping` também exige o prefixo literal `pista ok`. A admissão, a estimativa e a linha de diagnóstico fixam duas vagas. `bin/suite.sh:378,379,381,382,388,396,397,403,413,414,415` |
| **`bin/suite.sh` · pedido e coleta** | `pede`, `espera` e `log` miram `pista`. Se só o envio mudar, a coleta consulta o host errado. A linha de aceite, a de fim e os erros identificam o job sem host. A rota tem chamadas de espera e log, e o cancelamento documentado é da sessão, via SSH. `bin/suite.sh:445,452,459,471,477,481,485,497` `app/docs/RELATO.md:306,3625` |
| **`bin/disjuntor.py`** | Há três pontos que fixam `pista`: a regex de laço sobre `estado`, a regex de `espera` direto e a extração da chave de polling. A mensagem de recusa também imprime `ssh pista espera` literalmente. `bin/disjuntor.py:1974,1975,2132,2133,2134,2136,2137,2165,2166` |
| **`bin/tests/test_suite_rota_pista.sh`** | O SSH falso procura o argumento `pista`, descarta o host ao registrar a chamada e usa um modo de falha único. Uma chamada a `pista3` cai no caso de comando desconhecido, e as asserções de `espera` e `log` conferem só o ID. Recorte, inteira pre-opus e foto suja também exigem o formato antigo da linha `ROTA PISTA`. `bin/tests/test_suite_rota_pista.sh:40,42,43,44,47,126,146,160,162,163,165,180,183,264,299,300,392` |
| **`bin/pista_certifica.sh` e seu selo** | Os exemplos de coleta fixam `ssh pista`, e a mensagem de OOM nomeia `srv-glpi`. O certificador recebe dois arquivos locais e compara ID e SHA, sem conferir a origem por host. O selo fabrica os arquivos, sem SSH, e cobre ID diferente. Se o mesmo ID e SHA existirem nos dois hosts, misturar os arquivos pode passar pela checagem de procedência. `bin/pista_certifica.sh:13,14,15,16,22,29,48,54,55,60` `bin/tests/test_pista_certifica.sh:41,48,50,57,120` |

A instrução operacional em `CLAUDE.md` descreve a rota e o formato antigos, nomeia `ssh pista` para certificar e limita as regras de espera e polling a esse alias. O contrato anterior no RELATO ainda mede IDs por um único `estado`. Seguir esses comandos para um job da `pista3` consulta o host errado ou deixa esse job fora da contagem. `CLAUDE.md:100,101,129,337,339,340,346` `app/docs/RELATO.md:197,199`

As demais ocorrências literais de `ssh pista` estão nos registros e exemplos abaixo. Servem para os jobs ali registrados. Reusá-las como instruções para todo o pool consultaria apenas `pista`. `app/docs/BACKLOG.md:2375,2376` `app/docs/CORTES.md:567` `app/docs/CORTES.json:1823` `app/docs/PROMPTS.md:2308,2975,2988,3055,3057` `app/docs/RELATO.md:159,201,306,367,517,773,953,961,1190,1267,1754,1843,1929,2081,2269,2276,2531,2636,2872,3188,3545,3624,3697`

## (b) O que falta fechar no contrato

- **Queda após aceite com ID recebido:** o item 3 cobre o destino, `ERRO-INFRA` rc 71. O código atual já faz isso quando perde `espera` ou `log`, mas ainda cai na VM em duas variantes de rc 71 classificado como recusa sem execução, depois de imprimir o aceite. O “como hoje” deixa essa exceção ambígua. Também falta um prazo para o caso em que o SSH continua respondendo e o job fica preso. Os keepalives verificam a conexão, e `--espera` limita a estimativa anterior ao envio. `bin/suite.sh:391,415,452,458,459,468,470,471,476,481,485` `bin/tests/test_suite_rota_pista.sh:206,209,210,302,314,375`
- **Queda no `pede` antes de o ID chegar:** hoje um ID válido já conta como aceite, mesmo com erro do SSH. Sem ID, a rota cai na VM. Se o servidor admitiu o job e a resposta se perdeu, o contrato deixa indefinido como distinguir esse caso de recusa certa antes de reenviar ao outro host ou à VM. `bin/suite.sh:445,446,447,449,450` `bin/tests/test_suite_rota_pista.sh:57,59,60,368,369,370`
- **ID igual e cancelamento errado:** “usar esse host” cobre o caminho normal. Falta explicitar que **`(host, id)` identifica o job** também na retomada, na coleta para certificação, na contagem e no cancelamento. O ID aceito localmente tem dez dígitos e oito caracteres do SHA. Com o mesmo ID e SHA nos dois hosts, o certificador não distingue arquivos cruzados. O contrato histórico de `cancela` prevê rc 2 para ID inexistente. Se o ID existe no host errado, há risco de cancelar outro job. `bin/suite.sh:446,452,460,497` `bin/pista_certifica.sh:48,54,55` `app/docs/RELATO.md:199,3625`
- **Fila por host:** falta definir o que ocorre quando nenhum host elegível tem vaga, inclusive com um fora e o outro cheio, ou quando duas chamadas enxergam a mesma última vaga entre `estado` e `pede`. A regra antiga ainda admite enfileirar abaixo do limite estimado. Se a estimativa continuar valendo, ela precisa ter relógio, recibos e capacidade do próprio host. Hoje esses dados vêm de `pista` e o divisor é fixo em dois. `bin/suite.sh:378,382,384,396,403,409,413,415,445`
- **Prova de pronto:** falta atribuir quem pede a inteira de pouso na `pista3` e registra a origem dos dois arquivos. A rota atual exclui `pouso`, e `pista_certifica.sh` rc 0 prova o conteúdo dos arquivos sem provar o host que os produziu. `bin/suite.sh:500,502,504` `bin/pista_certifica.sh:22,48,54,55,94,95`

## (c) O disjuntor reconhece `pista3`?

**Não.** Na sessão principal, `polling_nega` não conta `ssh pista3 estado`, porque o extrator exige a palavra exata `pista`. `espera_nega` não classifica `ssh pista3 espera` como espera por alvo único, nem um laço com `sleep` que só consulta `ssh pista3 estado` como espera de obra. A coleta redirecionada da `pista3` passa por ausência de reconhecimento. Para `pista`, ela passa pela exceção explícita de recibo. `bin/disjuntor.py:1971,1974,1975,1977,2132,2137,2165,2167,2180,2182,2225,2226` `bin/tests/test_agente_no_modelo.sh:191,204,206,228,229`

Hash Git da cópia, comportamento dos lançadores remotos e disponibilidade real dos dois hosts [nao verificado].

## 27/11 23:19 · sessao 9b5089c5b761 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **Uma falha técnica do oráculo vira recusa contada, inclusive na revisão com `propagar_falha=True`.** O mesmo `except Exception` recebe a falta de dono, um erro de alimentação e uma falha de escrita. Todos incrementam `recusas_oraculo`, ficam no log e deixam o colaborador contar como processado se a lavra `motor` passou. O teste de falha SQL injeta esse caminho e exige retorno `1` e contador `1`. `app/ponto/services/fechamento.py:1296` `app/ponto/services/fechamento.py:1329` `app/ponto/services/fechamento.py:1334` `app/ponto/tests/test_feriado_um_juiz.py:461`
2. **A c10 passou a recusar a sombra no caso válido de saída antecipada do ver7.** Para `06:00 E / 06:30 S`, a escala `07:00–15:00` produz zero trabalhado e **420 min de saída antecipada**. A base esperava as duas lavras. O R3 atual exige recusa da sombra e preserva os 420 min no mensal e no `motor`. Reexecutei também o dia fechado de **7 h** isolado e junto ao BUG-144. Isolado, atualizou as duas versões. Junto ao dia aberto, atualizou o `motor`, e a sombra inteira ficou na versão anterior, inclusive para o dia fechado. `597fdc40d:app/ponto/tests/test_feriado_um_juiz.py:248` `app/ponto/tests/test_feriado_um_juiz.py:351` `app/ponto/tests/test_feriado_um_juiz.py:383` `app/ponto/tests/test_feriado_um_juiz.py:475` `app/ponto/services/dia_pago.py:317`

## Ver7 e fila do Opus

| Item | Resultado |
|---|---|
| **Ver7-1 — rollback do fechamento válido e dependência de outro dia** | **Curado para o mensal e o `motor`.** O R1 mantém **357 min / 5,95 h** e retorna `1`. O R2 mantém o mesmo valor com outro dia no mapa e acusa a falta de dono na data certa. O savepoint próprio começa antes da alimentação do oráculo. `app/ponto/services/fechamento.py:1296` `app/ponto/services/dia_pago.py:317` `app/ponto/tests/test_feriado_um_juiz.py:340` `app/ponto/tests/test_feriado_um_juiz.py:351` |
| **Ver7-2 — literal de pontualidade e RED sem mordida** | **Curado na guarda e no RED.** A guarda deriva os campos preenchidos de `CAMPOS_DIA`. Reexecutei M3 e M3_literal. O R3 falhou tanto ao limitar a guarda a `horas_trabalhadas` quanto ao recolocar a exclusão literal. O caso funcional de saída antecipada é o bloqueio 2. `app/ponto/services/dia_pago.py:258` `app/ponto/tests/test_feriado_um_juiz.py:383` `app/ponto/tests/feriado_um_juiz_mutantes.py:506` |
| **Opus (a) — default morto** | **Curado.** A presença da chave determina a exigência de todos os campos. Sem a chave, o laço já garante uma entrada em `campos_dos_periodos`, acessada diretamente. `app/ponto/services/dia_pago.py:318` |
| **Opus (b) — `do_motor` nos dois ramos** | **Curado.** A mesma chamada recebe `do_motor` com ou sem entrada em `rubricas`. Reexecutei o mutante que o retira no segundo caso, e o RED falhou nesse subcaso. `app/ponto/services/dia_pago.py:321` `app/ponto/tests/test_feriado_um_juiz.py:418` `app/ponto/tests/feriado_um_juiz_mutantes.py:172` |
| **Opus (c) — RED de dia só com FT** | **Curado.** A coleta ocorre depois dos três laços de FT. Reexecutei o mutante que deixa esses dias fora, e o RED falhou nos três subcasos, FT certa, FT feriado e FT sem escala. `app/ponto/services/dia_pago.py:224` `app/ponto/services/dia_pago.py:234` `app/ponto/services/dia_pago.py:248` `app/ponto/services/dia_pago.py:258` `app/ponto/tests/test_feriado_um_juiz.py:395` `app/ponto/tests/feriado_um_juiz_mutantes.py:167` |

## Quem escreve e quem lê

| Campo ou valor | Escritor, leitor e significado |
|---|---|
| `tentativas_oraculo`, `recusas_oraculo`, `processados` | `recalcular_fechamento_mes` inicializa os três por chamada. Conta uma tentativa por colaborador que chega à sombra, a partir de 10/2026, e uma recusa por exceção capturada nesse bloco. O log lê o acumulado no erro individual e publica o resumo se houve tentativa. Os testes conferem o resumo. O SaaS encaminha o logger para `/app/logs/ponto.log`. Leitor automatizado do contador em produção **[nao verificado]**. Tela e comando de recálculo leem `processados`, que mede o sucesso da lavra paga nesse caminho. `app/ponto/services/fechamento.py:680` `app/ponto/services/fechamento.py:1293` `app/ponto/services/fechamento.py:1329` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/test_feriado_um_juiz.py:334` `app/config/settings/saas.py:160` `app/config/settings/saas.py:181` `app/ponto/views.py:1280` `app/ponto/management/commands/recalcular_fechamento.py:130` |
| `propagar_falha` | O padrão é `False`. `recalcular_na_revisao` passa `True` para cada competência elegível. O serviço lê a flag na falha da lavra `motor` e no `except` externo. A falha da sombra segue o bloqueio 1. O teste da revisão confere a flag em julho e agosto. `app/ponto/services/fechamento.py:513` `app/ponto/services/fechamento.py:1274` `app/ponto/services/fechamento.py:1336` `app/ponto/services/fechamento.py:1888` `app/ponto/tests/test_revisar_par_relampago.py:163` |
| Donos declarados por dia | `_DONOS_DECLARADOS` lê as chaves de `valores` e `nao_decide` da data e acrescenta os mapas fornecidos de falta e grade e os campos de `do_motor`. `lavrar` usa o conjunto antes de qualquer escrita. **Há uma diferença de contrato.** O DIFF lê `nao_decide` como rubrica sem afirmação do calculador. A guarda aceita a chave como declaração de dono. Em chamada direta em memória, confirmei que `horas_falta` em `NAO_DECIDE` passa e grava `0.0` mesmo sem `faltas_por_dia`. O fechamento fornece esse mapa explicitamente. `app/ponto/services/dia_pago.py:50` `app/ponto/services/dia_pago.py:166` `app/ponto/services/dia_pago.py:317` `app/ponto/calculador/regras.py:532` `app/ponto/management/commands/diff_calculador.py:508` `app/ponto/services/fechamento.py:946` `app/ponto/services/fechamento.py:1313` |
| `do_motor` | O chamador nomeia os campos que devem conservar o valor já montado na linha. A guarda aceita esses campos e a sobreposição os preserva. As duas chamadas do fechamento usam o padrão `()`. O RED declara `horas_extras_100` explicitamente. `app/ponto/services/dia_pago.py:69` `app/ponto/services/dia_pago.py:140` `app/ponto/services/dia_pago.py:322` `app/ponto/services/dia_pago.py:328` `app/ponto/services/fechamento.py:1237` `app/ponto/services/fechamento.py:1309` `app/ponto/tests/test_feriado_um_juiz.py:424` |
| `CAMPOS_DIA` e valores lavrados | `dia_pago.py` declara os 15 campos. `lavrar` monta horas de períodos e FT dividindo os minutos por 60, incorpora atraso líquido, órfãos e faltas por seus mapas e mantém a grade em minutos. DSR e banco vão no ajuste da competência. O agregador do intervalo, o relatório e os leitores do calendário e da folha consultam o gravado. A HE100 de feriado e os splits noturnos são parcelas dos totais correspondentes. `app/ponto/services/dia_pago.py:34` `app/ponto/services/dia_pago.py:172` `app/ponto/services/dia_pago.py:224` `app/ponto/services/dia_pago.py:266` `app/ponto/services/dia_pago.py:292` `app/ponto/services/dia_pago.py:354` `app/ponto/services/dia_pago.py:791` `app/relatorios/views.py:547` `app/colaboradores/services/calendario.py:319` `app/folha/porta_export.py:290` `app/folha/export.py:434` |
| `rubricas` do dia | `rubricas_por_dia` monta `{date: Rubricas}` e `do_dia` produz `valores` em horas, `nao_decide` com causas e `detalhe` para o DIFF. O fechamento passa o mapa à lavra da sombra. `lavrar` substitui os campos decididos e grava as chaves de `nao_decide` no detalhe. `diff_calculador` usa a mesma alimentação e lê os valores e as causas para comparação. `app/ponto/calculador/alimentacao.py:571` `app/ponto/calculador/regras.py:663` `app/ponto/calculador/regras.py:962` `app/ponto/services/fechamento.py:1320` `app/ponto/services/dia_pago.py:326` `app/ponto/management/commands/diff_calculador.py:406` `app/ponto/management/commands/diff_calculador.py:460` `app/ponto/management/commands/diff_calculador.py:508` |

Os períodos, a FT e as rubricas usam o dia da jornada do motor quando há mapa. **O atraso de ligação ainda agrupa pela data civil da entrada do período**, e o O85 também usa data civil. A pontualidade do calculador lê o mapa da jornada. Portanto as chaves podem divergir após a meia-noite. Efeito em produção **[nao verificado]**. As janelas de datas usadas pela alimentação e pela soma lavrada incluem as duas pontas. No corte 21, a competência vai de 21 do mês anterior a 20 do mês de referência. `app/ponto/services/dia_pago.py:109` `app/ponto/calculador/alimentacao.py:73` `app/ponto/calculador/alimentacao.py:259` `app/ponto/services/credito_parcial.py:73` `app/ponto/services/fechamento.py:905` `app/ponto/calculador/alimentacao.py:524` `app/ponto/services/dia_pago.py:791` `app/ponto/janelas.py:10`

## Savepoint, catraca e classes de reprovação

- **Atomicidade.** Mensal e `motor` ficam na transação externa. A guarda da sombra roda antes do `delete`, e `delete` e `bulk_create` são atômicos por versão. A recusa preserva o mensal e o `motor` completos e deixa a sombra anterior inteira, conforme o R7. O teste ORM cobre falha no `bulk_create`, depois do `delete`. O mutante que remove só o savepoint da alimentação é detectado pelo teste com transação simulada. A execução individual do teste ORM na VM está **[nao verificado]** pelo resumo agregado fornecido. `app/ponto/services/fechamento.py:927` `app/ponto/services/fechamento.py:1174` `app/ponto/services/fechamento.py:1197` `app/ponto/services/fechamento.py:1296` `app/ponto/services/dia_pago.py:317` `app/ponto/services/dia_pago.py:365` `app/ponto/tests/test_feriado_um_juiz.py:199` `app/ponto/tests/test_feriado_um_juiz.py:475` `app/ponto/tests/test_feriado_um_juiz_porta.py:486` `app/ponto/tests/feriado_um_juiz_mutantes.py:494`
- **C12.** Conferi as chamadas reais por AST. O registro de `recalcular_fechamento_mes` bate em **721, 723, 729, 749, 820, 859, 905 e 917**. `_so_a_data` bate em **1609** e `apurar_por_fatia` em **257**. Um B1 em função nova faz subir o contador. Um B1 em linha nova de função já contada muda o registro de linhas. Há um limite concreto. Em memória, aumentei de duas para quatro as chamadas `.date()` na linha 723 e o registro B1 ficou idêntico, pois o detector deduplica por linha. A c12 acertou o registro e herdou esse ponto cego. `app/core/tests/test_medida_turno_do_par.py:332` `app/core/tests/test_medida_turno_do_par.py:565` `app/core/tests/test_medida_turno_do_par.py:848` `app/core/medida_turno_do_par.py:217` `app/core/medida_turno_do_par.py:228` `app/ponto/services/fechamento.py:257` `app/ponto/services/fechamento.py:723` `app/ponto/services/fechamento.py:1609`
- **Juiz, escritor, relógio e travas.** O delta acrescenta a verificação de procedência. A classificação segue pela partição existente, e as duas versões escrevem pela mesma `lavrar`. O relógio do furo é lido uma vez na borda da passada, e as duas lavras recebem a mesma lista. A revisão mantém Empresa por PK → Colaborador → Batida por PK. O savepoint entra depois dessas travas. O degrau da sombra exige sucesso da lavra `motor` e competência a partir de 10/2026. A classificação de feriado do mensal tem seu degrau em 11/2026. `app/ponto/services/dia_pago.py:317` `app/ponto/services/dia_pago.py:365` `app/ponto/services/fechamento.py:680` `app/ponto/services/fechamento.py:997` `app/ponto/services/fechamento.py:1152` `app/ponto/services/fechamento.py:1262` `app/ponto/services/fechamento.py:1279` `app/ponto/services/fechamento.py:1293` `app/ponto/services/fechamento.py:1325` `app/ponto/services/par_relampago_na_lista.py:172` `app/ponto/services/par_relampago_na_lista.py:479` `app/folha/export.py:385`
- **Contagem e prova.** O delta acrescenta **0 cláusulas `except`** e remove **0**. O `except` da sombra já existia e mudou de efeito. A ocorrência textual acrescentada no mutante é uma string. Rodei **52 testes de controle**, com uma falha esperada, **41 estruturais** e oito comandos de mutação, todos com o resultado previsto. Os seis arquivos alterados batem com os hashes de saída do diff fornecido. A identidade da árvore inteira pelo Git, a execução individual dos testes na VM, o DIFF de frota e os efeitos em produção ficam **[nao verificado]**. `app/ponto/services/fechamento.py:1329` `app/ponto/tests/feriado_um_juiz_mutantes.py:496` `app/ponto/tests/feriado_um_juiz_mutantes.py:545` `app/ponto/tests/feriado_um_juiz_mutantes.py:716` `app/ponto/tests/test_feriado_um_juiz.py:942`

## 27/11 23:05 · sessao e0a1f79b52c6 · DESLIGADO-PORTA-DO-CHAMADO · censo · origem=Code

# DESLIGADO-PORTA-DO-CHAMADO — censo c3

## CENSO

Os sete bloqueantes procedem na cópia. Duas ressalvas delimitam a cura: `abrir` já serializa a inserção com o cadastro e a falha dessa porta está no retorno antecipado do dedup. As três falhas do C12 vêm de referências deslocadas aos mesmos escritores. `app/chamados/models.py:988`, `app/chamados/models.py:1010`, `app/chamados/models.py:1064`, `app/chamados/tests/e4b_c12_ast.py:27`, `app/chamados/models.py:1593`, `app/chamados/models.py:1882`, `app/chamados/models.py:1902`.

Reproduzi **11 testes, 3 falhas** no selo C12 sem banco. Os **11 mutantes** compilaram em memória. A execução dos testes Django/PostgreSQL, a morte dos mutantes nesses testes, o hash Git da cópia, o volume atual do passivo e a execução em produção ficam [nao verificado]. `app/chamados/tests/test_e4b_c12_selo.py:22`, `app/chamados/tests/desligado_porta_do_chamado_mutantes.py:102`.

### decisões

#### 1. Passivo e universo da cascata

- **Origem da cura.** A seleção do universo da cascata mora em `app/colaboradores/services/desligamento.py:executar_desligamento:153`. O fecho de perguntas mora na exceção `FECHOU_DESLIGAMENTO` de `app/chamados/models.py:DisputaSupervisao.fechar:2214`, com releitura sob trava e persistência por `app/chamados/models.py:PerguntaDisputa.fechar_por_desligamento:3490`. Na exceção de desligamento, usar a opção `ignorar_tranca=True` do juiz existente para incluir a resposta sem veredito. Na disputa previamente fechada, a mesma exceção deve tratar a pergunta residual sem reabrir a disputa nem substituir autor, data, motivo ou resolução do fecho anterior. Confirmar o cadastro com `esta_desligado` sob a trava. `app/chamados/models.py:2089`, `app/chamados/models.py:2130`, `app/chamados/models.py:2184`, `app/chamados/models.py:2253`, `app/chamados/models.py:2265`, `app/core/juizes.py:283`, `app/docs/CORTES.md:546`.
- **RED do passivo em dia livre.** Cadastro já desligado, disputa já fechada, chamado vivo do sistema e pergunta `hora` respondida `07:15`, sem validação, carimbo ou luz concordante. O DRY deve achar os alvos. O APPLY deve tornar a pergunta terminal pela via `desligamento`, conservar seu texto, instante e ato de resposta, manter o fecho original da disputa e encerrar o chamado pela porta. Rerrodar deve produzir zero novas alterações. Hoje o cron exclui o cadastro, o executor exclui a disputa e o fecho comum devolve `False` para repetição idêntica ou levanta `ValueError` para outra entrada. `app/chamados/juizes.py:1525`, `app/chamados/juizes.py:1537`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/services/desligamento.py:156`, `app/chamados/models.py:2184`, `app/chamados/models.py:2287`.
- **RED da tranca.** Executar o desligamento com disputa aberta e repetir com cadastro já desligado e disputa fechada. Em ambos, usar pergunta respondida sem veredito e sem carimbo, num dia trancado, que voltaria à fila após perder a tranca. Ela deve receber a via `desligamento` e seguir fora da fila depois de reabrir a competência. Controle com pergunta muda viva em dia livre deve registrar `SILENCIO`. Controle com resposta já decidida deve conservar o ato anterior. O C6 hoje retira o dia trancado antes de ler a resposta, e o fecho usa `respostas_sem_veredito` com a tranca ligada. O teste da respondida parte de uma pergunta já viva, o teste do vigia fecha o passivo manualmente e o teste do cron de cadastro já desligado espera zero efetivações. Nenhum deles exige reparar esses estoques. `app/chamados/juizes.py:1503`, `app/chamados/models.py:2130`, `app/chamados/models.py:2213`, `app/chamados/models.py:2294`, `app/chamados/tests/test_desligado_porta_do_chamado.py:353`, `app/chamados/tests/test_desligado_porta_do_chamado.py:436`, `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:192`.
- **Porta operacional e reversão.** Proposta para o brief: expor `--passivo` explícito em `efetivar_desligamentos_agendados`, com DRY padrão e APPLY explícito. O modo chama o trecho compartilhado da cascata, pelas portas `DisputaSupervisao.fechar` e `ChamadoColaborador.encerrar` via `transicionar_com_pauta`. A seleção dos já desligados é própria do passivo. Reexecutar o desligamento integral repetiria ações de credencial, histórico, vínculo e S89. O cron atual tem DRY somente para agendados. `vigiar_fio_morto` mede o estoque visível pelo C6. O APPLY de cluster trata encerrados por `RECONCILIAR` e já pula desligados antes de escrever. `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:19`, `app/colaboradores/services/desligamento.py:127`, `app/colaboradores/services/desligamento.py:153`, `app/chamados/services/fecho_runtime.py:146`, `app/chamados/management/commands/vigiar_fio_morto.py:192`, `app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:275`, `app/ponto/services/cluster_espurio.py:286`, `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:168`.
- **Contrato do arquivo proposto.** `logs/desligado_porta_do_chamado/reversao_<schema>_<instante>.json` é nome proposto. Exigir `--reversao <arquivo>` no APPLY e gravar o antes de forma exclusiva antes da primeira mutação. O DRY imprime por empresa e PK de cadastro, disputa, pergunta e chamado, com o dia, estado, via e ação prevista, sem escrever banco ou arquivo. O JSON precisa conter as pré-imagens do fecho da disputa, do chamado e da pergunta, incluindo via, autor, instante, resposta, validação, cache de veredito, vínculos e alterações indiretas feitas pelas portas. Registrar a identidade dos atos, notas e trilhas gerados para a reversão compensatória. Sob a trava, conferir que o antes corresponde ao arquivo. Na reversão, conferir o estado produzido pelo lote e recusar divergência posterior. RED com falha ao gravar o arquivo deve deixar zero mutações. RED de ida e volta deve restaurar o estado de negócio e manter a trilha dos dois atos. O molde de DRY, arquivo anterior à escrita e reversão pela porta existe em `reatar_beco_folga_ao_limbo`. A 0057 só declara a choice e o caminho de reversão deste passivo fica [nao verificado]. `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:33`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:123`, `app/chamados/models.py:2294`, `app/chamados/signals.py:42`, `app/chamados/migrations/0057_via_desligamento.py:12`.

#### 2. Decisão antes da trava

- **Origem da cura.** `app/chamados/models.py:ChamadoColaborador.criar:1153` deve ler a linha entregue por `trava_na_ordem` dentro do atomic que insere. `app/chamados/models.py:ChamadoColaborador.abrir:919` já tem a trava. Consultar o juiz também no ramo encontrado pelo dedup e impedir que um vivo de desligado seja entregue ao emissor como alvo utilizável. O encerrado já consulta o juiz no renascimento e tem contrato de retornar a peça histórica com `criado=False`. `app/chamados/models.py:ChamadoColaborador.transicionar:140` e `app/chamados/models.py:ChamadoColaborador.reabrir:1507` precisam travar e reler antes de decidir, até o último efeito da operação. `_gravar_estado` já trava para salvar, depois das decisões em questão. A transição `analisar`, que também decide sobre a instância antes de aguardar a trava do escritor, precisa herdar a proteção contra foto obsoleta. `app/chamados/models.py:314`, `app/chamados/models.py:781`, `app/chamados/models.py:886`, `app/chamados/models.py:988`, `app/chamados/models.py:1007`, `app/chamados/models.py:1100`, `app/chamados/models.py:1159`, `app/chamados/models.py:1197`, `app/chamados/models.py:1519`, `app/chamados/models.py:1537`, `app/chamados/models.py:3094`, `app/chamados/tests/test_desligado_porta_do_chamado.py:161`.
- **Segundo desligamento.** A validação definitiva mora em `app/colaboradores/services/desligamento.py:executar_desligamento:82`, para view e cron herdarem. Usar o mesmo `julgar_desligamento` sobre a linha relida depois de travar, com o dia da borda. A guarda de `salvar_inteiro` impede que uma cópia velha volte para outro estado. Para o destino `desligado`, ela retorna cedo e aceita o segundo ato. Tanto a view quanto o cron precisam consumir a recusa da operação efetiva. `app/colaboradores/services/desligamento.py:38`, `app/colaboradores/services/desligamento.py:106`, `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/colaborador.py:63`, `app/colaboradores/services/colaborador.py:90`, `app/colaboradores/services/colaborador.py:159`, `app/colaboradores/views.py:1472`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`.
- **RED com concorrência real.** Usar `TransactionTestCase`, duas conexões e pontos de sincronização na leitura e na tentativa de trava. No código atual, A lê o ativo em `criar`, B efetiva e confirma, A prossegue para o INSERT. No reparo, forçar B a vencer a trava e A a esperar, liberando B sem esperar a conclusão de A. A deve reler desligado, devolver `None` e não criar chamado ou evento de célula. Testar a ordem inversa também: A cria enquanto detém a trava, B espera e a cascata alcança o chamado criado. O teste chamado `Concorrencia` completa o desligamento antes de chamar ambas as portas. O teste de duas threads da C2 disputa dois `abrir`, sem efetivação. `app/chamados/models.py:1159`, `app/chamados/models.py:1197`, `app/chamados/models.py:1200`, `app/colaboradores/services/desligamento.py:158`, `app/chamados/tests/test_desligado_porta_do_chamado.py:391`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`.
- **Mais três interleavings.** A carrega `aberto`, B fecha na cascata, A tenta `em_analise` sem motivo. Deve continuar fechado, sem nova linha de estado. A lê chamado já fechado e cadastro ativo em `reabrir`, B efetiva, A tenta gravar: deve receber recusa e deixar chamado e perguntas intactos. Dois POSTs obtêm veredito enquanto ativos, com datas ou motivos distintos: só o vencedor deve gravar desligamento, vínculo, credencial e S89. O teste de releitura de `reabrir` cobre uma instância velha cujo chamado já está vivo antes da chamada. O teste de redesligar faz os POSTs em sequência. Acrescentar também `abrir` com vivo legado preexistente em cadastro já desligado, pois o atual RED de inserção usa lookup vazio. `app/chamados/models.py:146`, `app/chamados/models.py:224`, `app/chamados/models.py:244`, `app/chamados/models.py:1519`, `app/chamados/models.py:1593`, `app/colaboradores/services/desligamento.py:128`, `app/chamados/tests/test_reabrir_decide_pelo_banco.py:57`, `app/colaboradores/tests/test_desligar_guards.py:36`, `app/chamados/tests/test_desligado_porta_do_chamado.py:140`.

#### 3. Chamadores e resgate pela tranca

- **Origem da cura.** O contrato de recusa sai de `app/chamados/models.py:ChamadoColaborador.criar:1161` e `app/chamados/models.py:ChamadoColaborador.abrir:1017`. Cada escritor dependente deve parar antes dos seus próprios efeitos, com as ações por sítio na lista abaixo. No resgate, a coleta mora no formulário/botão de `app/templates/chamados/partials/_lista_chamados.html:120`, e a resposta em `app/chamados/views_arquivar.py:desarquivar_chamado:52` deve refletir a recusa da porta de reabertura. Hoje o `fetch` só manda o cabeçalho CSRF e a view atribui qualquer `False` ao motivo do desligado. `app/chamados/views_arquivar.py:64`, `app/chamados/models.py:1498`.
- **RED das rotas.** Cadastro desligado, sem duplicata, acionar abertura DP, mensagem rápida e envio de agenda com rascunhos. Esperar resposta de recusa, zero push/GLPI/nota de abertura e agenda ainda em rascunho. Repetir pelas rotas de cada efeito da lista de chamadores com controle ativo. Os testes de abertura e mensagem exercitam o sucesso e o teste de agenda cobra justamente a promoção para `proposta`. O teste de nascimento de desligado para na porta do modelo. `app/chamados/services/abertura.py:194`, `app/chamados/views.py:982`, `app/chamados/views.py:995`, `app/colaboradores/views.py:2412`, `app/colaboradores/views.py:2427`, `app/colaboradores/views.py:2443`, `app/chamados/tests/test_views.py:100`, `app/chamados/tests/test_views.py:182`, `app/colaboradores/tests/test_agenda_endpoints.py:124`, `app/chamados/tests/test_desligado_porta_do_chamado.py:124`.
- **RED do resgate.** Cadastro desligado e chamado fechado por tranca. Acionar o botão renderizado, mandar motivo próprio, conferir a travessia e o autor na trilha e comparar a foto de disputa/perguntas. Motivo vazio deixa arquivo e estado intactos. Cadastro ativo, mesma via e irmão vivo: mostrar a causa real da recusa, preservando o redirecionamento da porta quando couber. O teste atual cria um POST já preenchido por `RequestFactory`, portanto o botão pode omitir o corpo sem que a asserção perceba. `app/templates/chamados/partials/_lista_chamados.html:120`, `app/chamados/views_arquivar.py:66`, `app/chamados/views_arquivar.py:75`, `app/chamados/models.py:1562`, `app/chamados/tests/test_desligado_porta_do_chamado.py:242`, `app/chamados/tests/test_desligado_porta_do_chamado.py:250`.

#### 4. Reaberturas divergentes

- **Origem da cura.** `app/chamados/models.py:ChamadoColaborador.recusa_da_reabertura:1455` já reúne o veredito de domínio. `transicionar` deve consultá-lo na travessia `ENCERRADOS → VIVOS`, mantendo a matriz de perfil e destino no motor. `reabrir` deve usar o mesmo veredito com o cadastro e o estado sob trava. Pedir ator identificado e motivo próprio para o desligado. A excepcionalidade humana não derruba a exigência vigente de fato novo no fecho de admin. `app/chamados/models.py:140`, `app/chamados/models.py:1477`, `app/chamados/models.py:1487`, `app/chamados/models.py:1537`, `app/chamados/catalogo/motor.py:139`, `app/docs/BACKLOG.md:733`, `app/docs/BACKLOG.md:1823`.
- **Disputa.** O veto do LU-12 deve morar nas duas portas que limpam o fecho, `app/chamados/models.py:DisputaSupervisao.reabrir_admin:2502` e `app/chamados/models.py:DisputaSupervisao.reabrir_sistema:2539`, com o juiz cadastral sob a trava. `acao_reabrir_disputa_admin` precisa consumir o resultado antes de criar conversa, nota e pushes. A reabertura por colaborador é correção de questionário de disputa aberta, já recusa disputa fechada sob trava. A reabertura humana do chamado de desligado deve ser apenas do chamado, inclusive quando o resgate tenta ressuscitar perguntas pela segunda vez. Hoje a exclusão da ressuscitação identifica somente a via nova e as vias fora do fato. `app/docs/CORTES.md:546`, `app/core/juizes.py:283`, `app/chamados/services/acoes_disputa.py:296`, `app/chamados/services/acoes_disputa.py:300`, `app/chamados/services/acoes_disputa.py:304`, `app/chamados/services/acoes_disputa.py:311`, `app/chamados/services/acoes_disputa.py:340`, `app/chamados/models.py:1637`, `app/chamados/models.py:2614`, `app/chamados/views_arquivar.py:84`.
- **RED.** Desligado, chamado fechado via `admin`, dia identificável sem fato novo, ator autorizado e motivo bom. Detalhe e `reabrir` devem recusar pelo mesmo motivo. Com fato novo e demais guardas satisfeitas, os dois caminhos só reabrem o chamado. Em disputa fechada de desligado, bater tanto no POST administrativo quanto em `reabrir_sistema`: fecho, quota, perguntas, notas e notificações ficam sem alteração. O teste atual de motivo no detalhe usa o fecho da cascata e prova a reabertura por essa rota. O teste do POST administrativo prova o caminho feliz sem exigir o veto LU-12. `app/chamados/models.py:150`, `app/chamados/models.py:1491`, `app/chamados/models.py:1539`, `app/chamados/services/acoes_disputa.py:300`, `app/chamados/tests/test_desligado_porta_do_chamado.py:229`, `app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:22`.

#### 5. Testemunha, I5e/I5f e S89

- **Origem da cura.** `app/chamados/services/materializacao.py:estado_aplicacao_pergunta:880` deve representar `desligamento` como encerramento sem aplicação antes do atalho por carimbo. O juiz de fato já responde `False` para a via. Fio e filtro de template interpretam `None` em hora respondida como testemunha indisponível, portanto precisam de um resultado inteligível para esse terminal. O histórico também precisa ler a via antes de dizer que o fato foi registrado. `app/chamados/services/materializacao.py:890`, `app/chamados/services/materializacao.py:903`, `app/chamados/juizes.py:729`, `app/chamados/services/fio.py:461`, `app/chamados/templatetags/chamados_tags.py:131`, `app/chamados/services/historico.py:104`.
- **Ator e atomicidade.** `app/chamados/models.py:ChamadoColaborador._gravar_estado:226` é a origem da atribuição durável do ato humano, no JSON de `estado_chamado`, além da FK. `app/chamados/management/commands/auditar_invariantes_chamados.py:Command.handle:673` deve ler essa evidência, com tratamento compatível para linhas anteriores. A FK é `SET_NULL`. Para S89, `app/colaboradores/services/desligamento.py:executar_desligamento:167` deve exigir confirmação da trilha no atomic da efetivação, usando o escritor de auditoria existente. `registrar_log` descarta o retorno e `evento` engole sua falha num savepoint. Há precedente de exigir o retorno de `evento` na operação crítica de cluster. `app/chamados/models.py:319`, `app/core/models.py:295`, `app/core/auditoria.py:17`, `app/core/observ.py:124`, `app/core/observ.py:133`, `app/ponto/services/cluster_espurio.py:296`, `app/docs/LEIS.md:65`.
- **RED de aplicação.** Hora `07:15` carimbada por desligamento, com resposta preservada, sem batida e sem validação. C6 e SQL devem tirá-la da fila. A testemunha, o cartão e o histórico devem dizer encerrada por desligamento, sem oferecer aplicação nem alegar plantio. Ter controles de hora efetivamente plantada e das recusas de paridade e colisão. O teste da respondida confere carimbo e preservação da sequência. O teste da muda usa escolha e não entra no ramo errado da testemunha de hora. `app/chamados/juizes.py:1518`, `app/chamados/models.py:3903`, `app/chamados/services/materializacao.py:868`, `app/chamados/services/materializacao.py:901`, `app/chamados/services/historico.py:104`, `app/chamados/tests/test_desligado_porta_do_chamado.py:353`, `app/chamados/tests/test_desligado_porta_do_chamado.py:378`.
- **RED de auditoria.** Reabrir com usuário real e motivo, apagar o usuário e rodar o vigia. A reabertura deve continuar em I5f e fora de I5e. Separadamente, fazer falhar somente o INSERT do S89 e conferir o rollback da efetivação, credencial, vínculo, perguntas e chamados, com recusa visível à borda. O teste de vigia mantém o usuário, e o teste de S89 lê uma trilha inserida normalmente. Adicionar controle pelo cron com autoria declarada de sistema. `app/core/models.py:295`, `app/chamados/management/commands/auditar_invariantes_chamados.py:678`, `app/colaboradores/services/desligamento.py:127`, `app/colaboradores/services/desligamento.py:167`, `app/chamados/tests/test_desligado_porta_do_chamado.py:300`, `app/chamados/tests/test_desligado_porta_do_chamado.py:436`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:64`.

#### 6. Dia civil lido duas vezes

- **Origem da cura.** `app/colaboradores/services/desligamento.py:julgar_desligamento:35` recebe `hoje` obrigatório. A view e o cron leem uma vez e passam o mesmo dia à seleção, ao julgamento e ao executor. A data de demissão é dia civil. Os instantes `executado_em`, `fechada_em` e o do carimbo continuam instantes dos atos. `app/colaboradores/services/desligamento.py:40`, `app/colaboradores/services/desligamento.py:60`, `app/colaboradores/services/desligamento.py:82`, `app/colaboradores/services/desligamento.py:151`, `app/colaboradores/views.py:1472`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `app/chamados/models.py:2305`, `app/chamados/models.py:3598`, `app/docs/PLANO-PADROES.md:101`.
- **RED.** Cadastro ativo, São Paulo ainda em 27/11 às 22h30, processo de teste emulando dia 28/11, pedido para 28/11. Esperar agendamento, acesso e vínculo vigentes e zero

[truncado em 20000 de 57061 caracteres]

## 27/11 22:51 · sessao 1a331d144fe3 · DESLIGADO-PORTA-DO-CHAMADO · verificador · origem=Code

O diff realiza parte do aval. O passivo ficou sem reparo e reversão, há corridas que permitem criar ou reabrir depois do desligamento, e três subtestes do selo E4B C12 falham na cópia. Os três arquivos centrais conferidos têm os hashes de blob do diff fornecido. O hash do commit, a execução em produção e o tamanho do passivo ficam [nao verificado]. `app/colaboradores/services/desligamento.py:156`, `app/chamados/models.py:1159`, `app/chamados/models.py:1593`, `app/chamados/migrations/0057_via_desligamento.py:12`, `app/chamados/tests/test_e4b_c12_selo.py:22`.

## 1. As quatro frases do aval

| Frase | Onde é realizada | O que fica de fora |
|---|---|---|
| **“portas criar/abrir perguntam esta_desligado”** | `criar` relê o cadastro e recusa com trilha. `abrir` consulta o cadastro da própria trava no caminho de inserção. Um encerrado encontrado no dedup passa pelo renascimento, que também pergunta ao juiz. `app/chamados/models.py:991`, `:1007`, `:1013`, `:1100`, `:1159`, `:1312`, `:1321`. | `abrir` devolve um vivo encontrado sem consultar o juiz. O retorno `None` quebra chamadores existentes, e em `criar` cabe um desligamento inteiro entre a leitura e a inserção. `app/chamados/models.py:992`, `:1010`, `:1197`, `app/chamados/services/abertura.py:172`, `:194`, `app/chamados/views.py:982`, `:995`. |
| **“S89 informativo”** | O executor efetivo grava `editar/Colaborador` com data, motivo, autor e instante. O detalhe do chamado mostra dia, pergunta e resposta preservada. O cron do agendado chama o mesmo executor. `app/colaboradores/services/desligamento.py:167`, `app/chamados/services/acoes_chamado.py:416`, `app/templates/chamados/detalhe_local.html:374`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`. | A gravação do informativo é best-effort, de modo que o desligamento pode concluir sem essa linha. O quadro de perguntas depende de estas já terem a via nova e estarem ligadas ao chamado exibido. `app/core/auditoria.py:17`, `app/core/observ.py:94`, `:133`, `app/chamados/services/acoes_chamado.py:418`. |
| **“pergunta sem veredito morre no desligamento”** | A cascata visita disputas abertas. A porta reúne as vivas e as respondidas sem veredito retornadas pelo juiz, grava `desligamento` antes de salvar a disputa, conserva a resposta e só registra `SILENCIO` quando a pergunta estava muda. Depois vêm o signal do pai e `encerrar` dos chamados restantes. `app/colaboradores/services/desligamento.py:155`, `:163`, `app/chamados/models.py:2212`, `:2281`, `:3490`, `app/chamados/signals.py:29`. | Disputa já fechada fica fora. Pergunta respondida sem veredito em dia trancado pode ficar fora **das duas** listas. A testemunha de aplicação ainda interpreta o carimbo da via nova como batida aplicada. `app/colaboradores/services/desligamento.py:156`, `app/chamados/models.py:2130`, `app/chamados/juizes.py:1511`, `app/chamados/services/materializacao.py:890`, `:903`. |
| **“passivo pela porta com reversao”** | O vigia mede vivos de desligados. O vigia de fio já mede perguntas vivas em disputas fechadas. O APPLY de cluster trata outro universo, o de clusters encerrados por `RECONCILIAR` com assinatura ainda viva. `app/chamados/management/commands/auditar_invariantes_chamados.py:660`, `:683`, `app/chamados/services/vigia_fio.py:746`, `app/ponto/services/cluster_espurio.py:88`, `:107`. | A parte executável do passivo e a reversão estão ausentes do diff fornecido. O executor só visita disputas abertas, a porta comum recusa disputa já fechada, e a 0057 só altera as `choices`. O backlog descreve DRY e APPLY pela porta. `app/colaboradores/services/desligamento.py:156`, `app/chamados/models.py:2184`, `app/chamados/migrations/0057_via_desligamento.py:12`, `app/docs/BACKLOG.md:732`. |

## 2. Escritores, leitores e significado

**Cadastro e relógio.** O registro vivo aponta para `esta_desligado`, que responde exclusivamente pelo estado gravado. `ativo` com demissão passada, de hoje ou futura dá `False` até a efetivação. `desligado` dá `True` mesmo com data futura ou nula. `afastado` dá `False`. Agendar grava data e motivo, efetivar grava também situação e reativar limpa a demissão e volta a `ativo`. O `DateField` representa dia civil. O juiz de agendamento usa `>` para futuro e aceita o teto de 90 dias inclusive. O cron seleciona `<= hoje` com `timezone.localdate()`. Na folha, `sai_pela_rescisao` usa `<= fim_janela`, e a regra de cadastro do `supra_juiz` usa `data > data_demissao`, preservando o próprio dia. São perguntas diferentes. A divergência de relógio é entre `date.today()` no juiz do agendamento e o dia local da borda, configurado como `America/Sao_Paulo`. `app/core/juizes.py:283`, `app/colaboradores/models.py:325`, `app/colaboradores/services/desligamento.py:23`, `:40`, `:60`, `:78`, `:106`, `:128`, `:190`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `app/colaboradores/views.py:1472`, `app/folha/export.py:54`, `app/ponto/supra_juiz.py:207`, `app/config/settings/base.py:93`.

**Leitores de `esta_desligado`.** Na cópia, as chamadas diretas estão na própria validação de desligamento, nas portas de situação e reativação do cadastro, no adaptador dos chamados, no detalhe, no vigia I5, no disparo por competência, no cadastro versus realidade e nas leituras da folha. As decisões cruas que continuam na rota de emissão incluem adesão e cartório, que comparam com `desligado`, e reconciliador e renascimento, que exigem `ativo`. Afastado passa pela primeira pergunta e falha na segunda. A própria via `cascade` continua vetando renascimento após reativação. `app/colaboradores/services/desligamento.py:38`, `app/colaboradores/services/colaborador.py:63`, `:66`, `:196`, `app/chamados/models.py:1325`, `:1104`, `:1106`, `app/chamados/views.py:557`, `app/chamados/management/commands/auditar_invariantes_chamados.py:660`, `app/chamados/management/commands/disparar_perguntas_competencia.py:61`, `app/escala/services/cadastro_realidade.py:171`, `app/folha/export.py:1216`, `:1235`, `app/folha/fora_da_folha.py:172`, `app/folha/retrato_folha.py:1286`, `app/chamados/services/adesao.py:67`, `app/chamados/reconciliador.py:942`, `app/ponto/services/cartorio.py:1803`, `:1868`.

| Campo ou valor | Quem escreve | Quem lê e divergências |
|---|---|---|
| **`PerguntaDisputa.via_resolucao='desligamento'`** | O único produtor nominal do valor é `fechar_por_desligamento` → `carimbar` → `_escrever_carimbo`. A mesma escrita grava `materializada_em` e `materializada_por`. A disputa gera a trilha `pergunta_encerrada_com_a_disputa` e, para a muda, `DecisaoPergunta.SILENCIO`. A 0057 declara o valor permitido. `app/chamados/models.py:2281`, `:2294`, `:3490`, `:3559`, `:3598`, `:3643`, `app/chamados/migrations/0057_via_desligamento.py:14`. | Leitores específicos são a idempotência da porta, o filtro de ressuscitação, o juiz de materialização de fato, o C6 e sua face SQL pela tupla comum, e o detalhe. O card de atendimento usa o display do campo. C6 lê um **instante** e considera o carimbo no próprio instante, pois só o retira da projeção quando `materializada > agora`. O estado derivado pode ser `RESOLVIDA`, enquanto a aplicação responde `aplicada` para uma `hora` respondida, mesmo sem plantio. `app/chamados/models.py:1643`, `:2947`, `:3484`, `:3497`, `:3903`, `app/chamados/juizes.py:729`, `:1480`, `:1518`, `app/chamados/services/acoes_chamado.py:421`, `app/templates/chamados/partials/_card_atendimento.html:195`, `app/chamados/services/materializacao.py:897`. |
| **`motivo_reabertura` do chamado** | `detalhe_local.html` fornece texto livre. `acao_resposta_livre` e `desarquivar_chamado` leem o POST, tiram espaços e passam a `transicionar` e `reabrir`, respectivamente. `_gravar_estado` grava o texto na descrição e no JSON de `estado_chamado`. O escritor também aceita o texto em transições que não são reaberturas. `app/templates/chamados/detalhe_local.html:411`, `app/chamados/services/acoes_chamado.py:323`, `app/chamados/views_arquivar.py:66`, `app/chamados/models.py:149`, `:224`, `:319`, `:1593`. | Leitores específicos são as duas guardas e I5f, que exige transição `ENCERRADOS → VIVOS`, motivo não branco e FK de usuário. Histórico e tela de auditoria leem a trilha/descrição. O mesmo nome no POST de **disputa** é um código do catálogo e tem `justificativa_admin` separada. O antigo campo homônimo de `DecisaoPergunta` virou `ato_motivo`. `app/chamados/models.py:150`, `:1477`, `app/chamados/management/commands/auditar_invariantes_chamados.py:673`, `app/chamados/services/historico.py:86`, `app/templates/core/log_auditoria.html:73`, `app/templates/chamados/partials/modal_fio.html:424`, `app/chamados/services/acoes_disputa.py:287`, `app/chamados/migrations/0050_resposta_anterior.py:24`, `app/chamados/migrations/0051_decisao_pergunta.py:45`. |
| **`LogAuditoria.acao='reabertura_recusada'`** | `_trilha_do_desligado` escreve `motivo='desligado'` para `renascer` e `reabrir`, e usa `nascimento_recusado` no `criar` e no `abrir`. Os escritores anteriores desse mesmo verbo são `_trilha_do_veto`, a recusa por fechamento de admin sem fato novo, a premissa regularizada e a mudança de universo sob trava. A transição manual sem motivo só levanta `ValueError`, que a ação de tela devolve. O `INSERT` é de `core.observ.evento`. `app/chamados/models.py:1270`, `:1335`, `:1101`, `:1539`, `:1547`, `:1553`, `:1572`, `:154`, `app/chamados/services/acoes_chamado.py:336`, `app/core/observ.py:133`. | I5g soma **linhas de tentativas**, sem janela, para os dois verbos com `motivo='desligado'` e sem chave `veto`, apesar do rótulo falar só em nascimento. `_trilha_do_veto` lê a sua própria trilha para dedup. A auditoria geral lê as linhas pelo verbo/modelo. `app/chamados/management/commands/auditar_invariantes_chamados.py:691`, `app/chamados/models.py:1275`, `app/core/views.py:705`. |
| **`ChamadoColaborador.status_local` e `via_encerramento`** | `abrir` e `criar` escrevem o estado inicial. As transições produtivas localizadas escrevem por `_gravar_estado`, chamado de `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada`. As migrations 0038 e 0039 e suas reversões escrevem diretamente. `app/chamados/models.py:1031`, `:1064`, `:1198`, `:253`, `:270`, `:327`, `:488`, `:694`, `:874`, `:892`, `:1593`, `:1882`, `:1902`, `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `:33`. | O signal da disputa tenta `resolvido/sistema`; a cascata tenta `fechado/cascade`. `registrado` é `VIVOS` e fica fora de `COBRANCA`. A via `cascade` identifica o verbo `ENCERRAR`, usado também na regularização externa, no vínculo e no reconciliador. É insuficiente para identificar um desligamento. Os leitores diretos da via, além das portas do modelo, estão em adesão, arquivo, competência, emissão de disputa, I16, cluster, furos sem cobrança e justificativa. `app/chamados/signals.py:34`, `:43`, `app/colaboradores/services/desligamento.py:158`, `app/chamados/catalogo/motor.py:24`, `:95`, `app/chamados/services/regularizacao_ext.py:206`, `app/colaboradores/services/vinculo.py:406`, `app/chamados/reconciliador.py:543`, `app/chamados/services/adesao.py:91`, `app/chamados/views_arquivar.py:64`, `:163`, `app/chamados/services/competencia_trancada.py:288`, `app/chamados/services/disputa_emissao.py:1994`, `:2033`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1096`, `app/ponto/services/cluster_espurio.py:110`, `:289`, `app/ponto/services/furos_sem_cobranca.py:69`, `app/ponto/services/justificativa.py:372`. |
| **S89** | A emissão antiga de `desligamento_revisao` foi removida, junto com o push. O produtor atual do informativo é o executor, por `registrar_log` → `evento`. `data_demissao` é dia civil em texto ISO. `executado_em` é instante ISO de `timezone.now()`, distinto de `LogAuditoria.criado_em`, preenchido no `INSERT`. No cron, `executado_por_id` pode ser nulo e o nome distingue o sistema. `app/colaboradores/services/desligamento.py:94`, `:149`, `:167`, `app/core/auditoria.py:8`, `app/core/models.py:295`, `:307`. | A auditoria geral exibe o novo informativo. Para o S89 antigo, continuam os leitores de natureza de registro, dia do fato/chave `data_demissao`, exceção à tranca, rótulo, comentário de rotina e janela do colaborador. A migration 0038 também reconhece o módulo antigo. Os leitores legados consomem um chamado, e o registro novo está em outra entidade. `app/core/views.py:705`, `app/templates/core/log_auditoria.html:73`, `app/chamados/catalogo/natureza.py:20`, `app/chamados/catalogo/modulos.py:141`, `:219`, `:265`, `app/chamados/services/desenlace.py:30`, `app/chamados/services/verbos.py:176`, `app/chamados/services/janela_colab.py:36`, `app/chamados/migrations/0038_a3_registrado_backfill.py:3`. |

**Outras portas e leitores da pergunta.** `fechar` grava estado da disputa, e `reabrir_admin` e `reabrir_sistema` limpam o fecho. A via da pergunta em geral pode ser trocada ou limpa por `carimbar_identidade`, `descarimbar`, `descarimbar_em_lote`, fechamento por fato no espelho e relavra. Os dois descarimbos só protegem `VIAS_FORA_DO_FATO`, que exclui `desligamento`. A relavra ordinária exige via derivada, e a que desfaz carimbo consulta C6. O comando de encerrar por fato restringe seu universo às vias sem história. `app/chamados/models.py:2305`, `:2525`, `:2549`, `:3659`, `:3691`, `:3701`, `:3787`, `:3791`, `app/chamados/services/relavra_via.py:118`, `:238`, `:242`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:41`.

**Outros leitores diretos da coluna genérica `via_resolucao`.** Há filtros de identidade, projeção e diagnóstico em `app/chamados/services/fio.py:1260`, `:1327`, `:1407`, `:1684`, `:1767`, `:2208`, `:2244`, `:2266`, `:2312`, `app/chamados/services/disputa_emissao.py:337`, `:409`, `:428`, `:446`, `:1688`, `:2182`, `app/chamados/services/tarefa_do_dia.py:262`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/competencia_trancada.py:280`, `app/chamados/services/passivo_validado.py:69`, `:147`, `:578`, `app/chamados/services/validacao.py:377`, `:471`, `app/chamados/services/materializacao.py:275`, `:307`, `:1202`, `:1245`, `:1786`, `app/chamados/reconciliador.py:1287`, `app/api/foto_recebida.py:45`, `app/ponto/services/flip_auto.py:129`, `app/ponto/services/cartorio.py:1705`. Há leitores em comandos de auditoria, backfill, relavra e reparo em `app/chamados/management/commands/auditar_invariantes_chamados.py:204`, `:435`, `:546`, `:982`, `:1759`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`, `:364`, `:432`, `:540`, `app/chamados/management/commands/relavrar_via_i6.py:58`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:59`, `app/chamados/management/commands/bug_par_103.py:67`.

**Censo de `status_local`.** Além das portas e leitores da via já citados, os consumidores centrais de estado são os signals, os juízes de fio e tarefa, o reconciliador, a fila, painel, vigias, histórico, APIs e telas. O censo E4B varre os escritores em produção por AST e exclui testes e migrations. A exaustividade de todos os leitores indiretos do campo genérico, incluindo aliases, projeções e consumidores de API, fica [nao verificado]. `app/chamados/signals.py:87`, `:186`, `:203`, `:287`, `:356`, `app/chamados/juizes.py:839`, `:2619`, `app/chamados/services/tarefa_do_dia.py:454`, `app/chamados/reconciliador.py:247`, `:961`, `:1058`, `app/chamados/services/fila.py:126`, `app/chamados/services/painel.py:139`, `:213`, `app/chamados/services/vigia_loop.py:29`, `:114`, `app/chamados/services/vigia_fio.py:322`, `app/chamados/services/historico.py:158`, `app/api/views.py:1037`, `:1135`, `app/api/views_mensageria.py:417`, `app/relatorios/views.py:811`, `app/templates/chamados/partials/_lista_chamados.html:33`, `app/chamados/tests/e4b_c12_ast.py:34`.

## 3. Classes de reprovação

| Classe | Resposta |
|---|---|
| **(a) Segundo juiz** | **Sim no repositório.** As portas novas delegam ao registrado, mas adesão e cartório ainda respondem à mesma pergunta comparando a situação crua. O registro está em `app/core/juizes.py:283`. A tabela se declara consulta ao registro vivo. `app/docs/JUIZES.md:10`, `app/chamados/models.py:1321`, `app/chamados/services/adesao.py:67`, `app/ponto/services/cartorio.py:1803`, `:1868`. |
| **(b) Segundo escritor fora da porta** | **Sim no sentido literal, em migrations históricas e fixtures. Não encontrei um novo escritor produtivo direto de status/via do chamado.** As três falhas do selo apontam chamadas ao núcleo do modelo em linhas deslocadas. `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/tests/test_desligado_porta_do_chamado.py:440`, `app/chamados/tests/e4b_c12_ast.py:27`, `app/chamados/models.py:1593`, `:1882`, `:1902`. |
| **(c) Relógio fora da borda** | **Sim, herdado no juiz de agendamento.** `julgar_desligamento` lê `date.today()` para default e de novo para decidir. A view e o cron têm outro `hoje`. O juiz `esta_desligado` é puro, e os `now()` do executor e do carimbo são instantes de escrita. `app/colaboradores/services/desligamento.py:32`, `:40`, `:60`, `:151`, `app/colaboradores/views.py:1475`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `app/chamados/models.py:3598`, `app/docs/PLANO-PADROES.md:101`. |
| **(d) Derivação paralela** | **Sim.** A alteração manual decide a exceção de reabertura separadamente de `recusa_da_reabertura`. O detalhe mostra o campo com base na matriz de estados. A testemunha de aplicação infere plantio pelo carimbo, apesar do juiz de fato. `app/chamados/models.py:140`, `:1477`, `:1487`, `app/chamados/views.py:555`, `app/chamados/services/materializacao.py:903`, `app/chamados/juizes.py:729`. |
| **(e) Selo/RED sem mordida** | **Sim.** Rodei o selo E4B C12 sem banco. Foram **11 testes e 3 falhas**. A tabela espera `1587`, `1876`, `1896`, e as chamadas estão em `1593`, `1882`, `1902`. Os testes novos do detalhe substituem `render`, e o de desarquivar monta o POST com o motivo à mão. Retirar o `<input>` ou deixar o botão real sem POST do motivo não contraria essas asserções. `app/chamados/tests/test_e4b_c12_selo.py:22`, `app/chamados/tests/e4b_c12_ast.py:27`, `app/chamados/models.py:1593`, `:1882`, `:1902`, `app/chamados/tests/test_desligado_porta_do_chamado_bordas.py:25`, `app/chamados/tests/test_desligado_porta_do_chamado.py:250`, `app/templates/chamados/detalhe_local.html:414`, `app/templates/chamados/partials/_lista_chamados.html:120`. |
| **(f) Ordem de trava** | **Sim.** `abrir` trava antes do lookup e lê a situação daquela mesma consulta. `criar` lê antes do `atomic` da inserção. `reabrir` relê status sem trava e julga antes da trava tomada na gravação. `transicionar` usa o status da instância antes de qualquer releitura do chamado. `DisputaSupervisao.fechar` reavalia o universo sob trava. `app/chamados/models.py:988`, `:1159`, `:1197`, `:1519`, `:1537`, `:314`, `:146`, `:2253`, `:2272`. |
| **(g) Pouso antes do pré-requisito** | **Não no diff.** 0057 e o escritor estão no mesmo material entregue, e a migration depende da 0056 e altera as `choices` do `CharField` existente. O script normal migra antes do reload. Ordem efetiva em produção e leitura por workers antigos durante um rollback [nao verificado]. `app/chamados/migrations/0057_via_desligamento.py:1`, `:8`, `:12`, `app/chamados/models.py:3499`, `bin/deploy.sh:308`, `:317`, `:320`. |
| **(h) Campo com dois significados** | **Sim.** `materializada_em` passa a significar também encerramento sem aplicação, mas a testemunha ainda o lê como `aplicada`. `via_encerramento='cascade'` também reúne desligamento e outros fechamentos, embora a adesão a descreva como cascata de desligamento. `app/chamados/models.py:3499`, `:3949`, `app/chamados/services/materializacao.py:903`, `app/chamados/catalogo/

[truncado em 20000 de 29645 caracteres]

## 27/11 22:13 · sessao da1300ed39b7 · FERIADO-L097-MOTOR · verificador · origem=Code

**REPROVA.** A cura libera o par do caso golden, mas pode pagar **240 min de folga comum sem autorização** e manter **240 min de feriado bloqueados** quando há dois turnos distintos na virada. A guarda decide pelo dia herdado pelo motor. A partição pode depois atribuir o mesmo par ao dia e ao vínculo do turno. `aval.txt:1` · [motor_calculo_v2.py:2523](/home/ronald/codex_ws/05ec19a6f09fb340ca601b29388c2a103dc02d10/app/ponto/motor_calculo_v2.py:2523) · [regras.py:410](/home/ronald/codex_ws/05ec19a6f09fb340ca601b29388c2a103dc02d10/app/ponto/calculador/regras.py:410)

## Achados por gravidade

1. **Bloqueante — a guarda e a rubrica podem julgar dias e vínculos diferentes.** Executei em memória o laço do motor e a partição da cópia, alimentados com dois turnos que o juiz distingue, `D 18:00–22:00` e `D+1 03:00–07:00`, ambos em folga certa, previsto zero, janela ativa e sem autorização. O vão de cinco horas faz o segundo par herdar `D` antes da guarda. Com feriado em `D`, a guarda libera os dois pares e a partição devolve 240 min ao dia comum `D+1`, em `horas_folga_trabalhada`. Com feriado em `D+1`, a guarda zera os dois e publica as duas pendências em `D`. A partição devolve o segundo par ao feriado já zerado. O oráculo ainda pode classificar esse par como invertido, pois procura o bloqueio pelo dia remapeado e pela hora da entrada. Reproduzi os mesmos efeitos com troca **6x1 → 12x36** no primeiro sentido e **12x36 → 6x1** no segundo. A prova alimentou os dias do juiz e os pares já fechados. A frequência em produção é [nao verificado]. `app/ponto/motor_calculo_v2.py:2523–2555,2570–2584` · `app/ponto/services/fechamento.py:297–351` · `app/ponto/calculador/regras.py:410–453` · `app/ponto/calculador/alimentacao.py:73–75,114–123`

2. **Bloqueante — “exportada intocada” fica sem garantia no recálculo.** O TXT já registrado tem conteúdo e hash próprios. O caminho de recálculo por evento, porém, chama o escritor normalmente. Na cópia, a guarda de escrita cobre a comp. 09/2026 para empresas que a entregaram. Para uma competência entregue fora da guarda, o escritor avisa e pode regravar `FechamentoMensal` e `DiaPago` com o motor novo. O teste anterior de outubro entregue usa `LIMBO`, que já saía do bloqueio antes da cura. A lista da comp. 11 J.A. por colaborador, dia e minutos, a reversão e qualquer efeito real em competência entregue ficam [nao verificado]. `aval.txt:1` · `app/folha/models.py:174–186` · `app/ponto/services/fechamento.py:37–51,603–629,1197–1228,1838–1853` · `app/ponto/tests/test_feriado_um_juiz_porta.py:239–289`

3. **Alto — o RED morde o pagamento, mas deixa passar falhas da testemunha.** Retirei em memória só o `if` acrescentado. No golden, o par caiu de **240 para 0 min trabalhados**, publicou uma pendência de 240 min e `segmentos_do_motor` passou de **240 para 0 min de segmentos**. Isso atinge as expectativas dos REDs de 4 h, inclusive a igualdade certa/incerta. O controle de folga comum pega uma liberação indevida, pois exige zero sem autorização e 4 h com autorização. Ele deixa passar a supressão isolada da pendência, pois só lê o mensal. Há um teste anterior que exige a entrada `sentido='dia'` no espelho, com fixture 12x36. Os REDs novos também conferem eventos e duas lavras, mas os valores-base de FT do oráculo vêm da partição do próprio motor. A igualdade das lavras pode passar com uma duração errada em um segmento que ainda mantém o dia visitável. Os testes de 12x36 cobrem o ciclo com e sem dobra ligada, em vínculo único. O caso anterior 20/21 com troca segue `expectedFailure` e seu auxiliar desliga a janela. Execução da suíte Django [nao verificado]. `app/ponto/tests/test_feriado_um_juiz_porta.py:67–92,549–649` · `app/ponto/tests/test_he100_dia_sem_previsto.py:112–130` · `app/ponto/tests/test_feriado_um_juiz.py:903–959` · `app/ponto/calculador/alimentacao.py:353–377,494–518,571–577` · `app/ponto/calculador/regras.py:788–818`

## Alcance exato do diff

- O único acréscimo executável no motor fica no bloco autorizado. Ele preserva o par já calculado quando há entrada e minutos trabalhados, janela vigente, previsto exatamente zero, decisão diferente de `autorizado`, `_folga_do_par` verdadeiro, ID disponível, escala certa e feriado no **dia que chegou à guarda**. O ciclo de `self.escala` precisa estar fora de `CICLOS_FERIADO_SIMPLES`, que contém só `12x36`. O teste é pelo ciclo, inclusive quando a CCT liga a dobra no 12x36. `None` passa pelo teste de ciclo. A decisão de HE ainda é consultada antes do retorno. `aval.txt:1` · `app/ponto/motor_calculo_v2.py:1486–1509,1554–1594,3749–3750` · `app/core/regua_cct.py:473–479`
- Com dia e vínculo concordantes, folga comum certa continua pendente. No intermitente de fábrica, o calendário fica vazio e `eh_feriado` devolve falso. Folga incerta já preservava o par. Na comp. 11, a partição usa HE100-feriado na incerta quando o ciclo e o dia dão dobra, e horas normais quando dispensam a dobra. Na certa, usa HE100-feriado quando o vínculo folga **por causa** do feriado e a dobra vale, ou FT quando a folga tem outra causa. A prova isolada deu 240 min tanto na certa quanto na incerta do golden. `app/ponto/motor_calculo_v2.py:1000–1011,1562–1594,3696–3708,3773–3777` · `app/ponto/calculador/regras.py:371–377,410–453`
- Um par único `22:00–02:00` iniciado no feriado preservou os 240 min no dia de início na prova isolada. Um par que inicia na véspera comum segue o dia da véspera. Feriado com **previsto > 0** sai antes do bloqueio total e continua sujeito à janela de pontas e à regra de dobra do ciclo. Feriado **sem folga, mas com previsto zero**, por exemplo célula de trabalho sem marcos suficientes, segue bloqueado. Se a frase geral do aval abrange esse último caso, ele ficou fora da cura. A presença dele na comp. 11 J.A. é [nao verificado]. `aval.txt:1` · `leis.txt:117–118` · `app/ponto/motor_calculo_v2.py:941–978,1554–1594,1644–1670,3470–3488` · `app/escala/utils.py:2314–2325`

## Escritores, leitores e significado dos campos

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| **Minutos e pontas do par** | `calcular_periodo` apura trabalhado, noturno de relógio e legal, HE total/50/100, subconjuntos de feriado e noturno e intra indenizada. `_carimbar_segmento` grava pontas efetivas e intervalos. No bloqueio, a guarda soma o trabalhado a `minutos_fora_janela`, zera esses minutos de apuração e iguala a saída efetiva à entrada efetiva. O retorno novo preserva o que já foi apurado. A L-093 lê trabalhado mais fora da janela. O oráculo lê pontas, intervalo efetivamente descontado, minutos de HE para o split e o mapa de dia. `app/ponto/motor_calculo_v2.py:1276–1278,1595–1607,1956–1965,3374–3399` · `app/ponto/calculador/alimentacao.py:73–138` |
| **Bloqueio e alertas** | O motor acrescenta alerta e publica `{dia, minutos, hora}` em `dias_sem_previsto_fora_da_janela`. Esse registro representa o bloqueio total. `folga_trabalhada` identifica a classe do par e é marcado mesmo quando ele zera. O espelho traduz o registro para `sentido='dia'`. PDF, retrato da Gestão de HE e portão do TXT leem a projeção. O portão aplica o limite e o cadastro de trava. O alerta genérico de FT ainda pede validação do responsável, embora o caso liberado já entre no dinheiro. `app/ponto/motor_calculo_v2.py:1608–1612,2591–2617,2841–2844` · `app/ponto/services/espelho.py:757–763,939–948` · `app/ponto/services/he_pendente_lavrado.py:138–153,183–211` · `app/ponto/services/gestao_he.py:426–433` · `app/folha/porta_export.py:181–201,687–697` · `app/relatorios/pdf_espelho.py:934–935,968–980` |
| **Períodos, rubricas e total pago** | O motor põe o objeto em `periodos` ou `periodos_ft`. `particionar_ft` dá à FT uma rubrica-base. O fechamento grava o mensal e chama `dia_pago.lavrar` para as duas versões. O TXT lê o mensal, subtrai o subconjunto feriado da HE100 normal e emite FT e adicional noturno separadamente. Espelho, calendário e PDF leem a lavra diária. No golden, esse caminho conta uma vez o par no pago do dia. A divergência demonstrada é de **qual dia e ciclo autorizam o pagamento**. `app/ponto/motor_calculo_v2.py:2591–2617` · `app/ponto/calculador/regras.py:428–453` · `app/ponto/services/fechamento.py:988–1021,1235–1263,1293–1328` · `app/ponto/services/dia_pago.py:224–247,874–905` · `app/folha/export.py:434–462,734–760` · `app/ponto/services/espelho.py:819–831` · `app/colaboradores/services/calendario.py:684–689` · `app/relatorios/pdf_espelho.py:904–913` |

**Classes de reprovação.** O diff reutiliza o calendário do motor, o juiz de escala certa, a fonte de previsto e a constante de ciclo registrada. As adições têm zero novo escritor persistente, zero leitura nova de relógio e zero `except` novo. A derivação paralela relevante já existia na atribuição do dia pela regra das seis horas. A exceção nova dá consequência financeira a essa diferença e deixa o mesmo par com uma data para bloqueio e outra para pagamento. Os blobs dos dois arquivos conferem com o anexo nos dois sentidos, revertendo o diff em memória. O pai `1d2eb496c`, a ancestralidade de `05ec19a6f` e a ordem efetiva de pouso ficam [nao verificado]. `obra.diff:1–23` · `app/core/juizes.py:318–323` · `app/ponto/motor_calculo_v2.py:1317–1354,1579–1594,2523–2532` · `app/ponto/calculador/regras.py:410–432`

## 27/11 21:43 · sessao 4b6e1be2ce2a · DESLIGADO-PORTA-DO-CHAMADO · construtor · origem=Code

- O APPLY agora consulta `recusa_da_reabertura()` sob a trava, depois de reconferir o universo. Só `desligado` recebe o novo `parado` e pula a porta de renascimento. [cluster_espurio.py:276](/home/ronald/wt-codex-desligado-porta-do-chamado-170518/app/ponto/services/cluster_espurio.py:276)
- A busca do autor ocorre após o renascimento. O APPLY só guarda o autor para reutilização depois que a transação da linha termina com sucesso. [cluster_espurio.py:290](/home/ronald/wt-codex-desligado-porta-do-chamado-170518/app/ponto/services/cluster_espurio.py:290)
- Em memória, o RED mordeu pela trilha simulada, o controle passou e os seis mutantes foram mordidos. O comando serializou o novo `parado` com `aplicadas=0`. O RED existente ainda exige o texto antigo na linha 175. Suite Django [nao verificado]. [detectar_cluster_espurio.py:55](/home/ronald/wt-codex-desligado-porta-do-chamado-170518/app/ponto/management/commands/detectar_cluster_espurio.py:55) · [test_o169_legado_encerrado_por_idade.py:175](/home/ronald/wt-codex-desligado-porta-do-chamado-170518/app/ponto/tests/test_o169_legado_encerrado_por_idade.py:175) [nao verificado]

```diff
--- a/app/ponto/services/cluster_espurio.py
+++ b/app/ponto/services/cluster_espurio.py
@@ -271,8 +271,6 @@
     for linha in r['linhas']:
         if linha['acao'] != REABRIR:
             continue
-        if autor is None:
-            autor = get_user_sistema()
         cid = ChamadoColaborador.objects.filter(pk=linha['pk']).values_list('colaborador_id', flat=True).get()
         try:
             with transaction.atomic():
@@ -283,21 +281,28 @@
                 if ch.pk not in contador_cluster_encerrado_sem_lastro(colaborador_id=cid)['pks']:
                     linha['parado'] = 'saiu do universo antes do apply'
                     continue
+                # DESLIGADO-PORTA-DO-CHAMADO c2: o APPLY legado nao escreve trilha de recusa do desligado.
+                # A trilha cabe as portas criar/abrir, que I5g conta.
+                if ch.recusa_da_reabertura()[0] == 'desligado':
+                    linha['parado'] = 'colaborador desligado: a porta recusaria'
+                    continue
                 antes = {'status_local': ch.status_local, 'via_encerramento': ch.via_encerramento}
                 if not ch._renascer_por_premissa_viva():
                     linha['parado'] = 'a porta de renascimento recusou'
                     continue
                 ch.refresh_from_db(fields=['status_local', 'via_encerramento'])
+                ator = get_user_sistema() if autor is None else autor
                 if evento(ACAO_LEGADO_REABRIR, 'ChamadoColaborador', ch.pk,
                           descricao='O169 legado: encerrado por RECONCILIAR com a assinatura viva no dia %s (universo '
                                     'do contador cluster_encerrado_sem_lastro); reaberto pela porta de renascimento '
                                     '(aval TRES-ITENS (2) 10/11).' % linha['dia'],
                           antes=antes, depois={'status_local': ch.status_local, 'via_encerramento': ch.via_encerramento},
-                          ator=autor) is None:
+                          ator=ator) is None:
                     raise _SemTrilha
         except _SemTrilha:
             linha['parado'] = SEM_TRILHA
             continue
+        autor = ator
         linha['aplicado'] = True [nao verificado]
         r['aplicadas'] += 1 [nao verificado]
     return r
```

<details>
<summary>Comandos do RED e dos mutantes em memória</summary>

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$1" <<'PY'
import sys
from contextlib import nullcontext
from pathlib import Path
from types import SimpleNamespace as NS
from unittest.mock import Mock, patch

mode = sys.argv[1]
path = Path('ponto/services/cluster_espurio.py')
source = path.read_text()
guard = "ch.recusa_da_reabertura()[0] == 'desligado'"
lookup = 'ator = get_user_sistema() if autor is None else autor'
mutants = {
    'sem-guarda': (guard, 'False'),
    'recusa-antiga': (guard, "linha['recusa'] == 'desligado'"),
    'toda-recusa': (guard, 'ch.recusa_da_reabertura()[0] is not None'),
    'autor-cedo': ('        cid = ChamadoColaborador',
                   '        if autor is None:\n            autor = get_user_sistema()\n        cid = ChamadoColaborador'),
    'sem-memo': (lookup, 'ator = get_user_sistema()'),
    'memo-rollback': (lookup, 'autor = ' + lookup),
}
mutation = mutants.get('sem-guarda' if mode == 'red' else mode)
if mutation:
    old, new = mutation
    assert source.count(old) == 1, 'alvo de mutacao'
    source = source.replace(old, new, 1)
scope = {}
exec(compile(source, str(path), 'exec'), scope)

def apply(reasons, event_results=()):
    trace, audit = [], []
    pks = list(range(1, len(reasons) + 1))

    def ticket(pk, reason):
        def renascer():
            trace.append('porta')
            if reason == 'desligado':
                audit.append('reabertura_recusada')
            return reason is None
        return Mock(pk=pk, colaborador=NS(situacao='ativo'), status_local='fechado',
                    via_encerramento='sistema', _dia_em_competencia_trancada=Mock(return_value=False),
                    recusa_da_reabertura=Mock(return_value=(reason, None)),
                    _renascer_por_premissa_viva=renascer)

    query = Mock()
    query.filter.return_value = query.values_list.return_value = query.select_related.return_value = query
    query.order_by.return_value = [ticket(pk, None) for pk in pks]
    current = {pk: ticket(pk, reason) for pk, reason in zip(pks, reasons)}
    query.get.side_effect = lambda **kw: current[kw['pk']] if kw else 7
    results = iter(event_results)
    modules = {
        'django.db': NS(transaction=NS(atomic=nullcontext)),
        'chamados.juizes': NS(dia_do_fato=Mock()),
        'chamados.models': NS(ChamadoColaborador=NS(objects=query), PerguntaDisputa=NS(trava_na_ordem=Mock())),
        'core.utils': NS(get_user_sistema=lambda: trace.append('autor') or object()),
        'core.observ': NS(evento=lambda *a, **kw: trace.append('evento') or next(results, True)),
    }
    scope['contador_cluster_encerrado_sem_lastro'] = lambda **kw: {'pks': pks}
    with patch.dict(sys.modules, modules):
        report = scope['tratar_legado_encerrado_sem_lastro'](aplicar=True, esperados=pks)
    return report, trace, audit

def check():
    report, trace, audit = apply(['desligado'])
    assert not audit, 'APPLY gravou reabertura_recusada'
    assert not trace, 'APPLY buscou autor sem reabrir'
    assert report['linhas'][0]['parado'] == 'colaborador desligado: a porta recusaria'
    assert report['aplicadas'] == 0 [nao verificado]
    for reason in ('veto_do_dia', 'admin_sem_fato_novo'):
        report, trace, _ = apply([reason])
        assert trace == ['porta'], 'outra recusa deixou de passar pela porta'
        assert report['linhas'][0]['parado'] == 'a porta de renascimento recusou'
    report, trace, _ = apply([None, None])
    assert trace == ['porta', 'autor', 'evento', 'porta', 'evento'], 'ordem ou memoizacao do autor'
    assert report['aplicadas'] == 2 [nao verificado]
    report, trace, _ = apply([None, None], (None, True))
    assert trace == ['porta', 'autor', 'evento', 'porta', 'autor', 'evento'], 'autor reutilizado apos rollback'
    assert report['aplicadas'] == 1 and not report['linhas'][0]['aplicado'] [nao verificado]

if mode in mutants:
    try:
        check()
    except AssertionError as err:
        print(f'{mode}: mordido ({err})')
    else:
        raise AssertionError(f'mutante sobreviveu: {mode}')
else:
    check()
    print(f'{mode}: passou') [nao verificado]
PY
}

prova red             # rc 1 esperado
prova controle
prova sem-guarda
prova recusa-antiga
prova toda-recusa
prova autor-cedo
prova sem-memo
prova memo-rollback
```

</details>

## 27/11 21:36 · sessao 894a7f223a11 · DESLIGADO-PORTA-DO-CHAMADO · leitura · origem=Code

A falha 2 é uma escrita de auditoria no APPLY legado. Ela também basta para reprovar o controle de mutantes da falha 1. Nas falhas 3 e 4, a divergência reproduzida em memória está no `MAPA.md`. No teto, o anexo comprova **149/148 para um furo**. O caso de três furos usa **224** na cópia e veio sem a asserção da VM. `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75–78,168–177,250–252,309–313`, `app/core/tests/test_selo_diagrama_do_codigo.py:44–47,85–98`, `app/ponto/tests/test_selo_chokepoint_escrita.py:277,291,526–534`.

## Falhas 1 e 2 · legado

**1. `test_controle_fonte_intacta_passa` — DEFEITO-DA-OBRA, por consequência da falha 2.** O controle executa os testes-alvo sobre a fonte intacta e exige listas vazias de erros e falhas. A falha 2 é o alvo explícito de `forca_desligado`, portanto sua quebra basta para derrubar o controle. Outros resultados internos na VM ficaram [nao verificado]. **Cura mínima:** corrigir o APPLY da falha 2. No mesmo arquivo de testes, cabe ao Opus atualizar `forca_desligado` para simular uma reabertura de fato e fazer a asserção morder pela mudança de estado. A chamada mutada atual, `reabrir(humano=True)`, já cai na nova recusa por falta de motivo e autor e pode produzir uma diferença só na trilha. `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:250–252,259–277,309–313`, `app/chamados/models.py:1467–1473,1531–1535`.

**2. `test_RED_desligado_a_porta_recusa_e_nada_se_escreve` — DEFEITO-DA-OBRA no APPLY legado, sob a exigência de zero escrita.** O **quinto elemento**, índice 4, passou de 2 para 3. Ele conta todas as linhas de `LogAuditoria` com `modelo='ChamadoColaborador'` e `objeto_id=ch.pk`. O terceiro elemento é `resolvido_em` e ficou igual. O teste já tinha confirmado a recusa e `aplicadas == 0` quando falhou em `self.assertEqual(self._foto(ch), antes)`. `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75–78,168–177`.

O APPLY relê o chamado sob trava e chama `_renascer_por_premissa_viva`. O hunk de `obra.diff` acrescenta, antes da guarda silenciosa que já recusava `situacao != 'ativo'`, a chamada a `_trilha_do_desligado`. Ela monta **`reabertura_recusada`**, com `porta='renascer'`, `motivo='desligado'` e o PK do chamado. **`core.observ.evento` insere a terceira linha.** O `continue` do APPLY sai normalmente do `atomic` e pula o evento de sucesso do legado. Essa escrita decorre do bloco novo da obra. `app/ponto/services/cluster_espurio.py:278–297`, `app/chamados/models.py:1098–1105,1322–1337`, `app/core/observ.py:133–138`.

**Cura mínima no app:** em `app/ponto/services/cluster_espurio.py`, usar a recusa de leitura `recusa_da_reabertura()` sob a trava já existente. Se ela devolver `desligado`, preencher `parado` e seguir sem chamar a porta que grava. Adiar também `get_user_sistema()` até a reabertura bem-sucedida, pois seu `get_or_create` pode inserir um usuário, efeito fora da foto. A obra exige trilha quando `abrir` tenta renascer um chamado, e o I5g consome essa trilha. Se o aval também exigir **persistir** a recusa no APPLY legado, a igualdade da foto vira **CARACTERIZACAO-A-INVERTER**. Nesse caso, o Opus deve preservar os quatro primeiros valores e exigir exatamente um evento com a ação, porta e motivo esperados. Essa extensão do aval ao APPLY está [nao verificado]. `app/ponto/services/cluster_espurio.py:270–297`, `app/core/utils.py:6–18`, `app/chamados/models.py:1449–1453,1467–1473`, `app/chamados/tests/test_desligado_porta_do_chamado.py:161–171`, `app/chamados/management/commands/auditar_invariantes_chamados.py:691–694`.

## Falhas 3 e 4 · derivados

**3. `test_MORDE_o_mmd_commitado_bate_com_o_codigo` — DERIVADO-A-REGERAR.** O `request.POST.get('motivo_reabertura', ...)` novo faz o censo reconhecer `desarquivar_chamado` como porta de escrita. A guarda anterior usava `request.method != 'POST'`, que fica fora dos padrões reconhecidos por `_e_post`. O censo também conta a view entre as que não contêm uma chamada direta a `registrar_log`. O delta isolado é **+1 porta e +1 no contador estático “sem trilha”**, sobre os 170/150 gravados no MAPA. A mensagem do teste cita o `.mmd`, mas a saída do gerador no anexo aponta `MAPA.md`. **Cura mínima:** regenerar `app/docs/MAPA.md` pelo gerador. `app/chamados/views_arquivar.py:52–77`, `app/chamados/urls.py:27`, `app/core/portas_da_tela.py:53–55,112–124`, `app/core/management/commands/gerar_diagrama.py:841–844,921–930`, `app/docs/MAPA.md:236`, `app/core/tests/test_selo_diagrama_do_codigo.py:44–47`.

**4. `test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` — DERIVADO-A-REGERAR.** O teste espera `rc=1` com a aresta injetada. Depois restaura o `.mmd` e exige `rc=0` para **os dois** derivados. A checagem em memória reproduziu `rc=1` após a restauração, com divergência só no MAPA. A linha exata da falha na VM está [nao verificado], pois o anexo termina antes da asserção. **Cura mínima:** a mesma regeneração da falha 3, via `python3 bin/gerar_diagrama.py`. O comando gera ambos os arquivos e o `.mmd` saiu idêntico na comparação em memória. `app/core/tests/test_selo_diagrama_do_codigo.py:85–98`, `app/core/management/commands/gerar_diagrama.py:911–935`, `bin/gerar_diagrama.py:15–16`.

## Falhas 5 e 6 · teto de escrita

**5. `test_registrar_batida_com_1_furo_vivo_que_fica` — TETO, 149/148.** A consulta nova identificável pelo código é o **`SELECT` de `Colaborador` por PK com `.first()`**, que `_desligado_relido` faz para ler `situacao`. O sítio é `ChamadoColaborador.abrir`, após a trava e quando o primeiro dedup não encontra chamado. O hunk acrescenta essa releitura. O `criar` dos furos da fixture acontece antes da captura. A linha SQL efetivamente capturada na VM está [nao verificado]. `app/chamados/models.py:988–1017,1312–1319`, `app/colaboradores/services/desligamento.py:23–32`, `app/ponto/tests/test_selo_chokepoint_escrita.py:277,512–529`.

Na pilha síncrona, o `create` da batida dispara o cartório, que chama a fábrica da célula. A fábrica pode abrir o pai `disputa_supervisao` quando ainda não há disputa aberta. **É uma consulta por entrada nesse ramo de abertura, constante em relação aos furos vivos das duas fixtures**, pois o pai é compartilhado. O ramo que atualiza os marcos dos furos vivos percorre os chamados, mas esse laço chama `_espelhar_marcos`. `app/ponto/registro_batida.py:176–180`, `app/ponto/signals.py:85–93,136–140`, `app/ponto/services/cartorio.py:1057–1074,1883–1895`, `app/chamados/services/fabrica_da_celula.py:168–174,217–219`, `app/chamados/services/disputa_emissao.py:539–544,580–606`.

**6. `test_registrar_batida_com_3_furos_vivos_que_ficam` — TETO relatado.** O teste usa a chave `3 chamados FICAM`, com teto **224**. Logo, 149 consultas passariam na comparação de teto dessa chave. A releitura do pai é a mesma candidata pelo código. O total, o delta e a asserção que falhou na VM estão [nao verificado]. `app/ponto/tests/test_selo_chokepoint_escrita.py:291,343–354,531–534`, `app/chamados/models.py:1013,1318`.

**Cura mínima para 5 e 6:** cabe ao Opus fazer o `SELECT FOR UPDATE` já usado por `abrir` devolver também a `situacao` fresca e entregá-la a `esta_desligado`, preservando a decisão sob trava. Se a medição justificar manter o `SELECT` separado, o C8 exige escrever ao lado de cada teto alterado qual consulta entrou, por que ela é necessária e quantas vezes roda. O próprio plano manda retirar o custo em caso de dúvida. `app/chamados/models.py:988–1017,3047–3056,3079–3087`, `app/colaboradores/services/desligamento.py:23–32`, `PLANO_PISCADA.md:8–15`, `app/ponto/tests/test_selo_chokepoint_escrita.py:277,291`.

## Quem escreve e quem lê

| Campo | Escrita | Leitura relevante |
|---|---|---|
| `Colaborador.situacao` | O executor atribui `desligado` e persiste por `salvar_inteiro`. A fixture do legado faz `update` antes da foto. `app/colaboradores/services/desligamento.py:127–132`, `app/colaboradores/services/colaborador.py:152–160`, `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:168–172` | `_desligado_relido` carrega o cadastro e `esta_desligado` lê `situacao`. O relatório do legado também lê a situação para preencher `ativo`. `app/chamados/models.py:1312–1319`, `app/colaboradores/services/desligamento.py:23–32`, `app/ponto/services/cluster_espurio.py:253–261` |
| `ChamadoColaborador.status_local`, `via_encerramento`, `resolvido_em` e `followups` | `_gravar_estado` grava os três campos. `nota.anexar_ocorrencia` insere o follow-up, chamado no renascimento após reabrir. `app/chamados/models.py:253–278,327,1118–1123`, `app/chamados/services/nota.py:75–89,102–109` | `_foto` lê os três campos e conta os follow-ups. O legado também lê estado e via no relatório. `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75–78`, `app/ponto/services/cluster_espurio.py:253–261` |
| `LogAuditoria.acao`, `modelo`, `objeto_id`, `valor_depois` e autoria | `_trilha_do_desligado` monta a recusa. `_gravar_estado` monta `estado_chamado` com `motivo_reabertura` e ator quando fornecidos. `core.observ.evento` persiste. `app/chamados/models.py:317–327,1322–1337`, `app/core/observ.py:118–138` | `_foto` conta por modelo e PK. I5g lê ação e `valor_depois.motivo`, excluindo `veto`. I5f lê a transição, `usuario_id` e `valor_depois.motivo_reabertura`. `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75–78`, `app/chamados/management/commands/auditar_invariantes_chamados.py:671–694` |
| `motivo_reabertura` do POST | O formulário do detalhe envia o campo. Na trilha ele é uma chave do JSON de `estado_chamado`. `app/templates/chamados/detalhe_local.html:411–415`, `app/chamados/models.py:319–326` | `acao_resposta_livre` e `desarquivar_chamado` leem o POST. `transicionar` e `recusa_da_reabertura` exigem o motivo na porta do desligado. A leitura em `desarquivar_chamado` é a que muda o censo. `app/chamados/services/acoes_chamado.py:322–330`, `app/chamados/views_arquivar.py:64–67`, `app/chamados/models.py:149–159,1467–1473`, `app/core/portas_da_tela.py:53–55` |
| `PerguntaDisputa.via_resolucao`, `materializada_em`, `materializada_por` | `DisputaSupervisao.fechar` chama `fechar_por_desligamento`, que usa `carimbar` para gravar os três, preservando a resposta. `app/chamados/models.py:2281–2298,3479–3488,3546–3550,3587–3600,3632` | C6 e `q_vivas` leem a via com o carimbo. O universo de ressuscitação exclui a via nova dentro do queryset existente, e o detalhe lê as perguntas encerradas para o informativo. `app/chamados/juizes.py:1513–1519`, `app/chamados/models.py:1628–1638,3889–3905`, `app/chamados/services/acoes_chamado.py:416–425` |

## 27/11 21:35 · sessao b4aff2e2a574 · - · - · origem=Ronald (tmux)

**Falha 3 — DERIVADO-A-REGERAR.** A leitura nova de `request.POST` em `app/chamados/views_arquivar.py:66` faz o censo reconhecer `desarquivar_chamado` como porta de escrita. A guarda anterior, `request.method != 'POST'` na linha 56, ficava fora do detector textual. Confirmei com o detector real sobre o HEAD e o trecho anterior reconstruído do `obra.diff`: `True` e `False`, respectivamente. A rota está em `app/chamados/urls.py:27`, incluída em `app/config/urls_ci.py:9`. O censo considera a view sem trilha porque seu fonte não contém o padrão de `registrar_log` que ele procura. O delta isolado é +1 porta e +1 sem trilha, de 170/150 para 171/151. O número do resolvedor Django completo na VM fica [nao verificado]. `app/core/portas_da_tela.py:44–55,103–124`, `app/core/management/commands/gerar_diagrama.py:841–844`, `app/docs/MAPA.md:236`.

**Falha 4 — DERIVADO-A-REGERAR.** O teste aceita `rc=1` com a aresta adulterada, restaura o `.mmd` e então exige `rc=0`. Essa última checagem também compara o `MAPA.md`. Em simulação só em memória, com o delta isolado como entrada do censo, o gerador real acusou ambos os derivados com a aresta injetada e só o MAPA após restaurar o `.mmd`. O assert específico no traceback da VM está [nao verificado] porque o anexo termina antes dele. `app/core/tests/test_selo_diagrama_do_codigo.py:85–98`, `app/core/management/commands/gerar_diagrama.py:911–930`.

A cura mínima das duas é regenerar `app/docs/MAPA.md` com `python3 bin/gerar_diagrama.py`. O comando gera também o `.mmd`, que saiu idêntico na comparação em memória. `bin/gerar_diagrama.py:15–16`, `app/core/management/commands/gerar_diagrama.py:931–945`.

## 27/11 21:35 · sessao ed1b7b494117 · - · - · origem=Ronald (tmux)

**Falha 2 — DEFEITO-DA-OBRA sob a exigência literal de zero escrita no APPLY.** O valor 2 → 3 é o quinto elemento da tupla, índice 4, que conta linhas de `LogAuditoria` do chamado. O terceiro elemento é `resolvido_em` e permanece igual no traceback. O caso marca `Colaborador.situacao='desligado'` antes da foto, confirma a recusa e zero aplicações, e falha somente na comparação final. `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75–78,168–177`.

O APPLY relê o chamado com o colaborador sob trava e chama `_renascer_por_premissa_viva`. A obra acrescentou uma releitura do cadastro e a chamada que emite `reabertura_recusada`, `modelo='ChamadoColaborador'`, `objeto_id=ch.pk` e `valor_depois` com `motivo='desligado'` e `porta='renascer'`. `core.observ.evento` é quem insere a linha. O `continue` encerra o `atomic` normalmente e pula a auditoria de sucesso. O anexo mostra que antes só havia a guarda silenciosa `situacao != 'ativo'`, ainda presente logo após o bloco novo. `app/ponto/services/cluster_espurio.py:278–297`, `app/chamados/models.py:1098–1105,1312–1337`, `app/core/observ.py:133–138`.

A cura localizada no código de app é impedir que a recusa do desligado no APPLY legado deixe escrita persistida, sob a trava existente. Uma opção é desfazer a transação da linha apenas quando a porta recusa e o juiz confirma que o cadastro relido está desligado. Se o requisito inclui qualquer escrita do APPLY, adiar também `get_user_sistema()` até a reabertura bem-sucedida, pois chama `get_or_create`; esse possível INSERT de `User` não entra na foto. A obra também exige trilha no renascimento disparado por `abrir` e I5g consome o evento. Se o aval autorizar essa trilha também no APPLY, a classificação seria **CARACTERIZACAO-A-INVERTER**, preservando os quatro primeiros elementos e exigindo exatamente um evento com a ação, a porta e o motivo esperados. O literal independente do aval para esse APPLY está [nao verificado]. `app/ponto/services/cluster_espurio.py:270–297`, `app/core/utils.py:6–18`, `app/chamados/models.py:1098–1103`, `app/chamados/tests/test_desligado_porta_do_chamado.py:161–171`, `app/chamados/management/commands/auditar_invariantes_chamados.py:691–694`.

**Falha 1 — mesma causa e classe da falha 2.** O controle roda os alvos das mutações sobre a fonte intacta e exige zero erros e falhas. A falha 2 é alvo explícito de `forca_desligado`, portanto basta para reprovar o controle. Os detalhes completos da execução interna na VM estão [nao verificado]. A cura base é a mesma da falha 2. O Opus precisa ainda verificar o mutante: `reabrir(humano=True)` agora recusa com trilha pela porta `reabrir` e pode matar o mutante pela auditoria sem demonstrar reabertura indevida. A mutação pode simular a violação da regra passando motivo e autor e o teste pode morder pela mudança de estado. `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:250–252,259–277,309–313`, `app/chamados/models.py:1467–1473,1531–1535`.

**Escritores e leitores da foto.** O executor de desligamento atribui `situacao`, salva por `salvar_inteiro` e o juiz `esta_desligado` lê a situação relida pela porta. O núcleo `_gravar_estado` escreve `status_local`, `via_encerramento` e `resolvido_em`, lidos por `_foto`, pelo relatório e pelas guardas do modelo. A porta de nota escreve `FollowupChamado`; o renascimento só a chama após reabrir. `_trilha_do_desligado` monta o evento e `core.observ.evento` persiste os campos auditados, lidos pela foto e pelo I5g. `app/colaboradores/services/desligamento.py:23–32,127–132`, `app/chamados/models.py:253–278,327,1118–1123,1312–1337,1471–1485`, `app/chamados/services/nota.py:75–89,102–109`, `app/ponto/services/cluster_espurio.py:253–261`, `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75–78`, `app/core/observ.py:133–138`, `app/chamados/management/commands/auditar_invariantes_chamados.py:691–694`.

## 27/11 21:34 · sessao 77a0589c1548 · - · - · origem=Ronald (tmux)

**5. `1 furo vivo que fica` — TETO.** O anexo comprova 149 consultas contra o teto 148. A consulta nova identificável no código é o `SELECT` simples da linha de `Colaborador` por PK, feito por `_desligado_relido` no `abrir` quando o dedup chega ao ramo sem chamado existente. O juiz usa `situacao`, gravada pelo executor do desligamento. A atribuição do SQL executado na VM está [nao verificado]. `app/ponto/tests/test_selo_chokepoint_escrita.py:277,526-529`, `app/chamados/models.py:992-1017,1312-1319`, `app/colaboradores/services/desligamento.py:23-32,127-132`.

**6. `3 furos vivos que ficam` — TETO, com divergência de números.** O anexo lista a falha, mas omite sua asserção. Na cópia, esse teste lê a chave `3 chamados FICAM`, cujo teto é **224**. A dupla `149/148` do enunciado não explicaria a falha nesse código. Total da VM e delta efetivo [nao verificado]. `app/ponto/tests/test_selo_chokepoint_escrita.py:291,343-354,531-534`.

**Caminho e cardinalidade para as duas.** O `create` da batida dispara o cartório sincronamente. A fábrica lê a saída ainda apagada, abre/atualiza a disputa e, quando a disputa ainda não existe, chama `ChamadoColaborador.abrir` para o pai comum `disputa_supervisao`. A guarda nova é uma leitura por entrada no ramo de abertura sem dedup e, no desenho das duas fixtures, incide sobre o pai comum, independentemente do número de chamados já vivos. O `criar` dos chamados da fixture ocorre antes da captura e o ramo do dia já coberto atualiza os marcos dos chamados vivos. `app/ponto/registro_batida.py:176-178`, `app/ponto/signals.py:85-93,136-140`, `app/ponto/services/cartorio.py:1057-1074,1883-1895,2113-2115`, `app/chamados/services/fabrica_da_celula.py:52-74,168-174,217-219`, `app/chamados/services/disputa_emissao.py:539-544,582-606,676-685`, `app/ponto/tests/test_selo_chokepoint_escrita.py:501-524`.

**Cura mínima.** Opus pode devolver a `situacao` fresca pelo `SELECT FOR UPDATE` que `abrir` já usa e entregar essa projeção ao mesmo juiz, eliminando a leitura adicional sob trava. Se a medição sustentar manter a consulta, o C8 pede justificação escrita junto de cada teto alterado com a consulta, o ponto e a cardinalidade. A medição da segunda falha segue pendente. `app/chamados/models.py:988-1017,3047-3087`, `app/colaboradores/services/desligamento.py:23-32`, `PLANO_PISCADA.md:10-15`, `app/ponto/tests/test_selo_chokepoint_escrita.py:277,291`.

## 27/11 21:24 · sessao e7f54b76e2c7 · FERIADO-L097-MOTOR · leitura · origem=Code

1. `janela_fechamento` define a janela da empresa 2. O universo inclui colaboradores com qualquer batida registrada na janela. `colabs` conta os colaboradores distintos com entradas em a–d. [app/ponto/janelas.py:54](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/janelas.py:54) [app/ponto/models.py:22](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/models.py:22)
2. `recalcular_fechamento_mes` em modo leitura monta os motores do fechamento e chama `apurar_por_fatia`, que executa `motor.calcular_mes` com o vínculo da fatia. [app/ponto/services/fechamento.py:744](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:744) [app/ponto/services/fechamento.py:774](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:774) [app/ponto/services/fechamento.py:306](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:306)
3. A classificação usa a resposta de certeza observada no próprio bloqueio e os minutos publicados pelo motor. A associação por identidade segue as entradas do resultado composto. [app/ponto/motor_calculo_v2.py:1579](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:1579) [app/ponto/motor_calculo_v2.py:1601](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:1601) [app/ponto/services/fechamento.py:314](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:314)
4. O ramo de leitura monta um `FechamentoMensal` em memória e retorna os campos calculados. `lavrar_dias_pagos=False` deixa a lavratura desativada, e a sonda força `READ ONLY` com rollback. [app/ponto/services/fechamento.py:896](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:896) [app/ponto/services/fechamento.py:1152](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1152) [app/ponto/services/fechamento.py:1215](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1215)
5. O runner usa o schema `juliani` e tem escritas próprias de credencial, permissões e trilha. Entradas e totais reais da sombra [nao verificado]. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:126) [bin/sonda_leitura.sh:153](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:153) [bin/sonda_leitura.sh:239](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:239) [bin/sonda_leitura.sh:249](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:249)

```python
if getattr(__import__("django.conf", fromlist=["settings"]).settings, "HASNER_SOMBRA", False) is not True:
    print("L097|PAREI|universo_colabs=NA|erros=1|motivo=settings_sombra", flush=True)
    raise SystemExit(1)

import logging
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from datetime import datetime, time, timedelta
from functools import wraps
from io import StringIO
from unittest.mock import patch


def parar_em_log_de_erro(_logger, registro):
    # O fechamento registra nome e segue no erro (app/ponto/services/fechamento.py:1327).
    if registro.levelno >= logging.ERROR:
        raise SystemExit(1)


def medir(estado):
    from django.db import connection, transaction
    from django.utils import timezone

    from colaboradores.models import Colaborador, Empresa
    from escala.services import escala_certa
    from ponto.janelas import janela_fechamento
    from ponto.models import Batida
    from ponto.motor_calculo_v2 import CICLOS_FERIADO_SIMPLES, MotorBase, localizar
    from ponto.services import fechamento

    compor = fechamento.apurar_por_fatia
    bloquear = MotorBase._dia_sem_previsto_fora_da_janela
    consultar_certeza = escala_certa.escala_certa_no_dia
    contagem = Counter()
    minutos = Counter()
    colabs_com_bloqueio = set()
    apurados = set()
    linhas = []

    @wraps(compor)
    def observar_composicao(colab, *args, **kwargs):
        capturas = {}
        certezas = {}

        @wraps(consultar_certeza)
        def observar_certeza(colab_id, dia, cache=None):
            certa = consultar_certeza(colab_id, dia, cache)
            certezas[(colab_id, dia, id(cache))] = certa
            return certa

        @wraps(bloquear)
        def observar_bloqueio(motor, dia, entrada, periodo):
            # O motor anexa o dict com os minutos já arredondados (app/ponto/motor_calculo_v2.py:1601).
            antes = len(motor._sem_previsto_fora)
            retorno = bloquear(motor, dia, entrada, periodo)
            novos = motor._sem_previsto_fora[antes:]
            if novos:
                folga = bool(motor._folga_do_par.get(localizar(entrada)))
                feriado = motor.eh_feriado(dia)
                ciclo = getattr(motor.escala, "tipo_ciclo", None)
                chave = (colab.pk, dia, id(motor._escala_certa_cache))

                grupo = "d"
                if folga and feriado and certezas[chave] is True:
                    grupo = "b" if ciclo in CICLOS_FERIADO_SIMPLES else "a"
                elif feriado and not folga:
                    grupo = "c"

                for item in novos:
                    capturas[id(item)] = (item, grupo, ciclo)
            return retorno

        with (
            patch.object(MotorBase, "_dia_sem_previsto_fora_da_janela", observar_bloqueio),
            patch.object(escala_certa, "escala_certa_no_dia", observar_certeza),
        ):
            apuracao = compor(colab, *args, **kwargs)

        # A composição preserva cada dict publicado pela fatia (app/ponto/services/fechamento.py:314).
        for item in apuracao.resultado.dias_sem_previsto_fora_da_janela:
            _, grupo, ciclo = capturas[id(item)]
            colabs_com_bloqueio.add(colab.pk)
            contagem[grupo] += 1
            if grupo != "d":
                valor = item["minutos"]
                minutos[grupo] += valor
                linhas.append(
                    f"L097|grupo={grupo}|colab=col{colab.pk}|dia={item['dia'].isoformat()}"
                    f"|minutos={valor}|ciclo={ciclo}"
                )
        apurados.add(colab.pk)
        return apuracao

    with transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("banco_ou_schema")

        empresa = Empresa.objects.get(pk=2)
        ini, fim = janela_fechamento(11, 2026, empresa)
        inicio = timezone.make_aware(datetime.combine(ini, time.min))
        limite = timezone.make_aware(datetime.combine(fim + timedelta(days=1), time.min))
        ids = set(Batida.objects.filter(
            colaborador__empresa_id=empresa.pk, timestamp__gte=inicio, timestamp__lt=limite,
        ).order_by().values_list("colaborador_id", flat=True).distinct())
        estado["universo"] = len(ids)

        # O universo da sonda substitui só a seleção de colaboradores (app/ponto/services/fechamento.py:574).
        universo = Colaborador.objects.filter(empresa=empresa, pk__in=ids).order_by("pk")
        if ids:
            with (
                patch.object(fechamento, "universo_da_competencia", return_value=universo),
                patch.object(fechamento, "apurar_por_fatia", observar_composicao),
            ):
                lidos = fechamento.recalcular_fechamento_mes(
                    11, 2026, empresa_id=empresa.pk, colaborador_ids=sorted(ids),
                    somente_leitura=True, lavrar_dias_pagos=False,
                )
            if apurados != ids or lidos.keys() != ids:
                raise RuntimeError("universo_incompleto")
        transaction.set_rollback(True)

    total = (
        f"L097|TOTAL|a={contagem['a']}|a_min={minutos['a']}"
        f"|b={contagem['b']}|b_min={minutos['b']}|c={contagem['c']}|c_min={minutos['c']}"
        f"|d={contagem['d']}|colabs={len(colabs_com_bloqueio)}|universo_colabs={len(ids)}"
    )
    return [f"L097|UNIVERSO|universo_colabs={len(ids)}|erros=0", *linhas, total]


estado = {"universo": "NA"}
sucesso = False
try:
    with (
        patch.object(logging.Logger, "handle", parar_em_log_de_erro),
        redirect_stdout(StringIO()),
        redirect_stderr(StringIO()),
    ):
        saida = medir(estado)
    sucesso = True
finally:
    if not sucesso:
        print(f"L097|PAREI|universo_colabs={estado['universo']}|erros=1", flush=True)
        raise SystemExit(1) from None

print("\n".join(saida), flush=True)
```

## 27/11 21:13 · sessao 682dadce1e1b · FIM-DE-SEMANA-JA · leitura · origem=Code

A S1 informa **38 chamados distintos e 10 colabs**. A S4 reutiliza `c2` do anexo e lista todos os pares do terceiro retorno. Um par com **Conferir data** continua visível para auditoria, com `sem_acao_no_recorte=nao`, e entra no subtotal de falsos positivos. O subtotal de chamados conta os distintos com pelo menos um par marcado. Um mesmo chamado pode ter outro par ainda sem ação. [hasner.txt:53, citado no anexo S2] [app/chamados/services/fio.py:610] [app/templates/chamados/partials/modal_fio.html:173] [app/chamados/views.py:1308]

A sonda lê o anexo original de `sonda_S1.py` ou do texto em `S4_S1_FONTE`. Carrega as definições de C2 em memória, pois o corpo de topo do anexo dispara a coleta completa. Os IDs dos pares, os novos totais e a execução na Sombra ficam [nao verificado].

```python
import sys

sys.dont_write_bytecode = True

import ast
import json
import logging
import os
import tokenize
from collections import defaultdict
from functools import lru_cache
from html.parser import HTMLParser

from django.conf import settings
from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from django.contrib.auth.models import User
from django.template import Context, engines

from chamados.catalogo.motor import VIVOS, decidir
from chamados.juizes import competencia_do_chamado, pergunta_viva
from chamados.models import ChamadoColaborador, DisputaSupervisao
from chamados.services.fio import (
    chamado_que_lista,
    contexto_parcial_fio,
    data_do_fato_de,
    espera_o_texto_do_colaborador,
)
from chamados.services.materializacao import estado_aplicacao_pergunta
from chamados.services.tarefa_do_dia import exigencia_das_perguntas
from chamados.services.verbos import particionar_verbos
from chamados.utils import departamento_do_chamado, pode_agir_chamado
from colaboradores.models import Empresa
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.supra_juiz import perguntas_que_seguram_csf

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
ESPERADO = 38
NV = "[nao verificado]"

FONTE_DONO = "app/ponto/management/commands/e6_oraculo.py:182,53"
OBRA_A3 = "E4C-F5-82-FALTA"
FONTE_A3 = "app/docs/BACKLOG.md:2059"
ESCRITOR_VALIDAR = (
    "app/chamados/services/validacao.py:_carimbar_validacao:164"
)
ESCRITOR_RESPOSTA = (
    "app/chamados/services/disputa_emissao.py:responder_pergunta_disputa_core:1471"
)
PORTAS = {
    "conferir_data": (
        ("app/chamados/views.py:conferir_data_pergunta:1288",),
        "app/chamados/services/validacao.py:conferir_data_do_fato:1031",
    ),
    "validar": (
        (
            "app/chamados/services/fio.py:processar_post_fio:721",
            "app/chamados/views.py:detalhe_chamado_local:483",
            "app/chamados/views_cobrar.py:validar_inline:145",
            "app/chamados/views_cobrar.py:validar_lote:250",
        ),
        ESCRITOR_VALIDAR,
    ),
    "recusar_pergunta": (
        (
            "app/chamados/services/fio.py:processar_post_fio:776",
            "app/chamados/views.py:detalhe_chamado_local:506",
        ),
        "app/chamados/services/validacao.py:recusar_pergunta:902",
    ),
    "corrigir_resposta": (
        ("app/chamados/services/acoes_disputa.py:acao_corrigir_resposta:343",),
        ESCRITOR_RESPOSTA,
    ),
    "responder": (
        (
            "app/api/views.py:api_disputa_responder:2609",
            "app/chamados/views.py:responder_pergunta_disputa:1229",
        ),
        ESCRITOR_RESPOSTA,
    ),
    "materializar": (
        ("app/chamados/services/materializacao.py:materializar_perguntas_validadas_da_disputa:2238",),
        "app/chamados/services/materializacao.py:_materializar_uma:1955",
    ),
    "rejulgar": (
        ("app/ponto/services/cartorio.py:julgar_celula:2078",),
        "app/ponto/portas/celula.py:lavrar_veredito:1069",
    ),
    "decidir_ausencia": (
        (
            "app/ponto/views.py:aprovar_ausencia:2770",
            "app/ponto/views.py:rejeitar_ausencia:2854",
        ),
        "app/ponto/services/ausencia.py:_decidir_no_ato:1084",
    ),
    "entregar_documento": (
        (
            "app/api/views.py:api_ausencia_documento:3085",
            "app/ponto/views.py:anexar_documento_ausencia:2165",
        ),
        "app/ponto/services/ausencia.py:anexar_documento:1005",
    ),
}


def carregar_s1():
    # Use apenas o fonte original do anexo. Seus imports e as definicoes
    # abaixo bastam para chamar a propria c2 sem disparar medir() no topo.
    fonte = os.environ.get("S4_S1_FONTE")
    if fonte is None:
        with tokenize.open(os.environ.get("S4_S1_ARQUIVO", "sonda_S1.py")) as arquivo:
            fonte = arquivo.read()
    nome = "sonda_S1.py"
    arvore = ast.parse(fonte, filename=nome)
    nomes = {
        "Linha", "valor", "ids", "dia_da_pergunta", "CartoesHTML", "templates",
        "chamado_de", "validar_em_leitura", "PortasGestor", "portas_colab",
        "c2", "ator",
    }
    nos = []
    for no in arvore.body:
        if isinstance(no, (ast.Import, ast.ImportFrom)):
            nos.append(no)
        elif isinstance(no, (ast.FunctionDef, ast.ClassDef)) and no.name in nomes:
            nos.append(no)
        elif isinstance(no, ast.Assign) and any(
            isinstance(alvo, ast.Name) and alvo.id == "VERBOS_DA_PERGUNTA"
            for alvo in no.targets
        ):
            nos.append(no)
    s1 = dict(__name__=__name__, EMPRESA_ID=EMPRESA_ID, AGORA=AGORA, HOJE=HOJE, NV=NV)
    exec(compile(ast.Module(body=nos, type_ignores=[]), nome, "exec"), s1)
    return s1


class FormulariosConferirData(HTMLParser):
    def __init__(self, html):
        super().__init__()
        self.cartoes_da_pergunta = defaultdict(set)
        self.feed(html)
        self.close()

    def handle_starttag(self, tag, attrs):
        a = dict(attrs)
        if tag == "form" and "data-s4-pergunta" in a:
            pergunta = int(a["data-s4-pergunta"])
            self.cartoes_da_pergunta[pergunta].add(int(a["data-s4-chamado"]))


@lru_cache(maxsize=1)
def template_conferir_data():
    # Marca em memoria apenas o formulario do POST real.
    # app/templates/chamados/partials/modal_fio.html:163-178
    engine = engines["django"].engine
    fonte = engine.get_template("chamados/partials/modal_fio.html").source
    alvo = """<form hx-post="{% url 'chamados:conferir_data_pergunta' perg.pk %}" """
    if fonte.count(alvo) != 1:
        raise SystemExit("PAREI: formulario Conferir data mudou")
    marca = 'data-s4-pergunta="{{ perg.pk }}" data-s4-chamado="{{ chamado.pk }}" '
    return engine.from_string(fonte.replace(alvo, alvo + marca, 1))


@lru_cache(maxsize=None)
def foto_do_fechamento(ch):
    # Mesma decisao seca que reconciliar, fora de atomic no molde da S3.
    # app/chamados/models.py:661-716,761-781
    fecha = ch.reconciliaria()
    motor = decidir(ch.status_local, "RECONCILIAR")
    premissa_ok = (
        ch._gate_premissa_do_fechamento(registrar=False) if motor.permitido else None
    )
    csf_ok = ch._gate_conversa_do_pai(registrar=False)
    csf = perguntas_que_seguram_csf(ch)
    vivas, reatas, pergunta_ok = [], None, None
    if motor.permitido:
        plano = ch._plano_pergunta_viva(motor.novo_estado)
        vivas, reatas = plano
        pergunta_ok = ch._gate_pergunta_viva(motor.novo_estado, registrar=False, plano=plano)

    # A ordem rotula a recusa da leitura acima. Cada guarda responde por si.
    motivo = None
    if not fecha:
        if not motor.permitido:
            motivo = "motor_RECONCILIAR"
        elif not premissa_ok:
            motivo = "dia_furado_para_a_folha"
        elif not csf_ok:
            motivo = "conversa_exige_decisao_humana"
        elif not pergunta_ok:
            motivo = "pergunta_viva_na_competencia_aberta"
        else:
            motivo = NV

    return {
        "fecha": fecha,
        "motivo": motivo,
        "vivas": [p.pk for p in vivas],
        "reatas": None if reatas is None else {p.pk: destino.pk for p, destino in reatas},
        "destino_motor": motor.novo_estado if motor.permitido else None,
        "gate_pergunta_viva": pergunta_ok,
        "gate_pai": csf_ok,
        "csf": csf,
    }


def origem_da_acao(acao):
    portas, escritor = PORTAS.get(acao, (NV, NV))
    return {"portas": portas, "escritor": escritor}


def linhas_c2(empresa, inicio, fim):
    s1 = carregar_s1()
    conta = s1["ator"]("S1_GESTOR_PK")
    observacoes = {}

    class GestorDaS1(s1["PortasGestor"]):
        def consultar(self, p, ch=None):
            leitura = super().consultar(p, ch)
            if ch is not None:
                observacoes[(ch.pk, p.pk)] = p, leitura
            return leitura

    # Mesmos insumos entregues a c2 por colher() de sonda_S1.py.
    base = ChamadoColaborador.objects.filter(
        colaborador__empresa_id=empresa.pk, status_local__in=VIVOS,
    ).select_related("colaborador__empresa", "celula").order_by("pk")
    chamados = {}
    for ch in base:
        comp = competencia_do_chamado(ch)
        dia = comp.dia if comp is not None else None
        if dia is None or inicio <= dia <= fim:
            chamados[ch.pk] = ch, dia
    gestor = GestorDaS1(conta, particionar_verbos(base, agora=AGORA)["mapa"])
    _, _, pares = s1["c2"](chamados, inicio, fim, gestor)
    exigencias = exigencia_das_perguntas(sorted({r.pergunta for r in pares}), AGORA)
    cxr_da_competencia = cxr_lavrado()

    @lru_cache(maxsize=None)
    def insumos(colab):
        return InsumosDoDono(
            colab, inicio, fim, HOJE, agora=AGORA, sem_motor=True,
            cxr=cxr_da_competencia(colab.pk, inicio),
        )

    @lru_cache(maxsize=None)
    def parcial(colab):
        # O GET da view marca lidas. O construtor do parcial so fornece contexto.
        # app/chamados/views.py:870-881; app/chamados/services/fio.py:573-610
        ctx = contexto_parcial_fio(gestor.req, colab)
        ctx["cor_disputas_map"] = DisputaSupervisao.cor_map_por_colaborador(colab)
        html = template_conferir_data().render(Context(ctx, use_l10n=False))
        return (
            FormulariosConferirData(html).cartoes_da_pergunta,
            {ch.pk: ch for ch in ctx["chamados"]},
        )

    linhas = []
    for r in sorted(pares, key=lambda r: (r.chamado, r.pergunta)):
        ch = chamados[r.chamado][0]
        p, leitura = observacoes[(r.chamado, r.pergunta)]
        local, fio, gaveta, glpi_local, remoto, lote, acoes_s1 = leitura
        d = p.disputa
        colab = d.colaborador
        ancora = chamado_que_lista(p)
        pai = s1["chamado_de"](d.chamado_id) if d.chamado_id is not None else None
        alvo = s1["chamado_de"](ancora) if ancora is not None else None
        colabs = {colab, ch.colaborador}
        colabs.update(c.colaborador for c in (pai, alvo) if c is not None)
        cartoes_conferir = sorted({
            pk for c in colabs for pk in parcial(c)[0].get(p.pk, ())
        })
        dia_evento, diverge, dia_ancora = data_do_fato_de(p)
        # A porta confere permissao de acao e pergunta por PK.
        # A justificativa e entrada humana. app/chamados/views.py:1288-1309
        # app/chamados/services/validacao.py:1057-1072
        conferir = bool(
            cartoes_conferir and gestor.pode_inline and diverge
            and p.data_conferida_em is None
        )

        foto = foto_do_fechamento(ch)
        c6 = pergunta_viva(p, hoje=HOJE, agora=AGORA)
        app, web, app_login, web_login = s1["portas_colab"](colab)
        razao, exigidas = exigencias.get(p.pk, (NV, None))
        aplicacao = estado_aplicacao_pergunta(d, p) or {}
        espera_texto = espera_o_texto_do_colaborador(p)
        pode_agir_ancora = pode_agir_chamado(conta, alvo) if alvo is not None else None

        forms_fio = {
            (card.pk, f["acao"])
            for c in colabs
            for card, _, f in gestor.modal(c)[1].get(p.pk, ())
        }
        cartao = parcial(alvo.colaborador)[1].get(ancora) if alvo is not None else None
        eleita = getattr(cartao, "disputa_obj", None)
        # Testemunha em leitura do caso A3, com a propria disputa fechada,
        # outra aberta eleita no container e falta de Validar no cartao.
        a3 = bool(
            not conferir and pode_agir_ancora and c6
            and alvo.colaborador_id == d.colaborador_id
            and d.fechada_em is not None and d.chamado_id != ancora
            and eleita is not None and eleita.chamado_id == ancora
            and (ancora, "validar_pergunta") not in forms_fio
            and s1["validar_em_leitura"](p, False)
        )

        verbos = {a["acao"] for a in exigidas or ()}
        verbos.update(
            "validar" if acao == "validar_pergunta" else acao for _, acao in forms_fio
        )
        if p.pk in foto["csf"]:
            verbos.add("validar")
            if d.aberta:
                verbos.add("recusar_pergunta")
        if conferir:
            verbos.add("conferir_data")

        linhas.append({
            "chamado": r.chamado,
            "pergunta": p.pk,
            "origem/destino": {
                "disputa": d.pk, "pai": d.chamado_id,
                "destino": p.chamado_destino_id, "ancora": ancora,
                "disputa_fechada": d.fechada_em is not None,
                "disputa_eleita_no_cartao": getattr(eleita, "pk", None),
                "dia_do_recorte_S1": r.dia, "data_evento": dia_evento,
                "dia_da_ancora": dia_ancora,
            },
            "plano_de_reata": {
                "reconciliaria": foto["fecha"], "destino_motor": foto["destino_motor"],
                "gate_pergunta_viva": foto["gate_pergunta_viva"],
                "vivas_no_pos_estado": foto["vivas"], "reatas": foto["reatas"],
                "neste_plano": p.pk in foto["vivas"],
            },
            "CSF": {
                "gate_pai_permite": foto["gate_pai"],
                "perguntas_que_seguram": foto["csf"],
                "neste_par": p.pk in foto["csf"],
            },
            "C6": c6,
            "ator/perfil": {
                "gestor_S1_pk": conta.pk, "gestor_ativo": conta.is_active,
                "gestor_staff": conta.is_staff, "gestor_superuser": conta.is_superuser,
                "gerir_chamados": gestor.pode_inline, "pode_agir_ancora": pode_agir_ancora,
                "departamento_ancora": departamento_do_chamado(alvo) if alvo else NV,
                "colab_do_fato": f"col{colab.pk}",
                "login_app_estimado": app_login, "conta_web_ativa": web_login,
            },
            "canal/acao": {
                "sub_S1": r.sub,
                "local": sorted(local), "fio": sorted(fio), "gaveta": sorted(gaveta),
                "glpi_local": sorted(glpi_local), "glpi_remoto": NV if remoto else False,
                "lote_S1": lote, "formularios_fio": sorted(forms_fio),
                "acoes_gestor_S1": sorted(acoes_s1),
                "app_S1": p.pk in app, "web_S1": p.pk in web,
                "conferir_data_no_parcial": cartoes_conferir,
                "exigidas_pelo_juiz_da_tarefa": exigidas if exigidas is not None else NV,
                "sem_acao_no_recorte": "nao" if conferir else "sim",
            },
            "motivo_da_recusa": {
                "fechamento_em_seco": foto["motivo"],
                "razao_tarefa": razao.partition(":")[0],
                "estado_aplicacao": aplicacao.get("estado", NV),
                "espera_texto_colab": espera_texto,
                "data_diverge": diverge, "data_ja_conferida": p.data_conferida_em is not None,
                "resultado_POST": NV,
            },
            "dono": (insumos(colab).dono(r.dia) or NV) if r.dia is not None else NV,
            "colab": f"col{r.colab}",
            "conferir_data": "sim" if conferir else "nao",
            "porta/escritor": {acao: origem_da_acao(acao) for acao in sorted(verbos)} or NV,
            "obra_dona": NV,
            "obra_candidata": {"obra": OBRA_A3, "fonte": FONTE_A3} if a3 else NV,
            "fontes": {
                "universo": f"sonda_S1.py:{s1['c2'].__code__.co_firstlineno}",
                "origem/destino": "app/chamados/services/fio.py:97,136",
                "gate": "app/chamados/models.py:393,429,677,683,718,783",
                "CSF": "app/ponto/supra_juiz.py:561",
                "C6": "app/chamados/juizes.py:911",
                "ator": "app/chamados/views.py:35;app/chamados/utils.py:68;app/api/login_helpers.py:110",
                "conferir_data": "app/chamados/services/fio.py:610;app/templates/chamados/partials/modal_fio.html:173;app/chamados/views.py:1306",
                "tarefa/aplicacao": "app/chamados/services/tarefa_do_dia.py:461;app/chamados/services/materializacao.py:880",
                "dono": FONTE_DONO,
            },
        })
    return linhas


def colher():
    with schema_context("juliani"):
        connection.set_autocommit(False)
        try:
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
                cursor.execute(
                    "SELECT current_schema(), current_setting('transaction_read_only'), "
                    "current_setting('transaction_isolation')"
                )
                if cursor.fetchone() != ("juliani", "on", "repeatable read"):
                    raise SystemExit("PAREI: schema ou modo de transacao incorreto")
            if not User.objects.filter(username="sistema").exists():
                raise SystemExit("PAREI: usuario de sistema ausente na Sombra")
            empresa = Empresa.objects.get(pk=EMPRESA_ID)
            inicio, fim = janela_fechamento(MES, ANO, empresa)
            linhas = linhas_c2(empresa, inicio, fim)
            with connection.cursor() as cursor:
                cursor.execute("SELECT 1")
            return linhas
        finally:
            connection.rollback()
            connection.set_autocommit(True)


class SemErroComDadosLivres:
    def __enter__(self):
        return self

    def __exit__(self, tipo, erro, quadro):
        if tipo is None or issubclass(tipo, (SystemExit, KeyboardInterrupt)):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        arquivo = quadro.tb_frame.f_code.co_filename.rsplit("/", 1)[-1]
        raise SystemExit(f"PAREI: {tipo.__name__} em {arquivo}:{quadro.tb_lineno}") from None


def valor(x):
    if x is None:
        return "-"
    if isinstance(x, str):
        return x.replace("|", "\\u007c")
    return json.dumps(x, default=lambda v: v.isoformat(), ensure_ascii=True).replace("|", "\\u007c")


def main():
    nivel_anterior = logging.root.manager.disable
    logging.disable(sys.maxsize)
    try:
        with SemErroComDadosLivres():
            linhas = colher()
            chamados = {r["chamado"] for r in linhas}
            colabs = {r["colab"] for r in linhas}
            falsos = [r for r in linhas if r["conferir_data"] == "sim"]
            chamados_falsos = {r["chamado"] for r in falsos}
            saida = ["S4|C2|" + "|".join(f"{k}={valor(v)}" for k, v in r.items()) for r in linhas]
    finally:
        logging.disable(nivel_anterior)
    print(
        f"S4|UNIVERSO|chamados={len(chamados)}|colabs={len(colabs)}"
        f"|pares={len(linhas)}|esperado_chamados={ESPERADO}"
    )
    for linha in saida:
        print(linha)
    print(f"S4|FALSO_POSITIVO_CONFERIR_DATA|pares={len(falsos)}|chamados={len(chamado

[truncado em 20000 de 22521 caracteres]

## 27/11 21:13 · sessao 3ad2776bbb19 · FERIADO-L097-MOTOR · leitura · origem=Code

# FERIADO-L097-MOTOR · Dossiê de leitura

**Base para a cura integrada: FERIADO-UM-JUIZ em `1d2eb496c`, em cópia própria, ou a `main` depois que essa obra pousar.** O motor é idêntico nas duas cópias. A diferença que interessa está depois dele: na `main` do hash solicitado, 240 min de feriado em folga incerta entram em trabalhadas comuns. Na obra em voo, para um 6x1 elegível na competência 11, os mesmos 240 min entram em HE100-feriado. É nessa segunda base que o RED suspenso verifica a troca de rubrica mantendo 240 min. As perguntas de lei ficaram nos casos **só feriado com previsto zero** e **12x36 em folga no feriado**. `app/ponto/services/fechamento.py:948–1009` · `c11/app/ponto/calculador/regras.py:371–432` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:547–563` · `aval.txt:1` · `parei.txt:1`.

`c11/` indica a cópia `~/wt-codex-feriado-um-juiz-152427`. O checkout aponta para `1d2eb496c` e sua base comum com `main` é `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Os números de teste neste dossiê são expectativas lidas no código. Execução dos testes e resultado de produção [nao verificado]. `/home/ronald/saas-hasner/.git/packed-refs:165` · `~/wt-codex-feriado-um-juiz-152427/.git:1` · `/home/ronald/saas-hasner/.git/worktrees/wt-codex-feriado-um-juiz-c4-152427/HEAD:1`.

## 1. Mapa do bloco vivo

| Linhas | O que faz hoje |
|---|---|
| `app/ponto/motor_calculo_v2.py:2513–2522,2558–2574` | O laço resolve o dia da jornada, que pode herdar o do par anterior. Classifica a folga pelo vínculo ou pela regra semanal, guarda a resposta pela entrada localizada, calcula o par e chama `_dia_sem_previsto_fora_da_janela`. |
| `app/ponto/motor_calculo_v2.py:1554–1564` | Sai sem entrada ou sem minutos trabalhados. Continua se `_folga_do_par` ou `eh_feriado(dia)` for verdadeiro. |
| `app/ponto/motor_calculo_v2.py:1486–1509,1511–1534,1565–1571` | Exige janela HE vigente para a empresa e previsto **igual a zero**. Previsto positivo e `None` saem. Consulta a decisão vigente e sai se `autorizado`. |
| `app/ponto/motor_calculo_v2.py:1579–1584` | Só consulta `escala_certa_no_dia` se o par é folga e há identificador de colaborador. Se a escala é incerta, retorna com o par intacto. |
| `app/ponto/motor_calculo_v2.py:1585–1590` | Soma o trabalhado líquido a `minutos_fora_janela`. Zera `minutos_trabalhados`, noturno relógio e legal, extra total, extra 50, extra 100, extra 100 feriado, os dois splits noturnos de HE e intra indenizada. |
| `app/ponto/motor_calculo_v2.py:1591–1602,2833` | Colapsa `efetivo_saida` na entrada efetiva, acrescenta o alerta L-097 e publica `{dia, minutos arredondados, hora}` em `dias_sem_previsto_fora_da_janela`. A numeração viva põe o colapso e a publicação depois de `:1590`. Um retorno dentro do trecho avalizado já evita as três escritas. `aval.txt:1`. |
| `app/ponto/motor_calculo_v2.py:2581–2607` | Depois da chamada, o mesmo juízo de folga manda o par para `periodos_ft` e acrescenta o alerta genérico de FT. Os demais pares vão para `periodos`. |

| Caso | Resposta atual com janela vigente e sem autorização |
|---|---|
| **a. Feriado em folga, escala certa** | Com previsto zero, bloqueia o par inteiro. Sai em `periodos_ft` com zero, pontas efetivas iguais e pendência. `app/ponto/motor_calculo_v2.py:1562–1602,2581–2605`. |
| **b. Feriado em folga, escala incerta** | Retorna em `:1584` e preserva o par em `periodos_ft`, sem pendência deste bloco. `app/ponto/motor_calculo_v2.py:1579–1584,2581–2605`. |
| **c. Feriado sem flag de folga** | Com previsto zero, chega pelo `eh_feriado` e bloqueia sem consultar certeza. Com previsto positivo, sai em `:1568` e segue o cálculo do ciclo. Cadastro com marcos incompletos pode produzir o primeiro caso. A janela ordinária das pontas roda antes deste bloco. `app/escala/utils.py:2314–2325` · `app/ponto/motor_calculo_v2.py:1562–1586,1865–1876,2571–2574,3380–3397`. |
| **d. Folga comum, escala certa** | Com previsto zero, continua bloqueada, com alerta, par colapsado e pendência. `app/ponto/motor_calculo_v2.py:1579–1602` · `app/ponto/tests/test_he100_dia_sem_previsto.py:151–159`. |
| **e. 12x36** | Passa pela chamada comum. Plantão previsto sai pelo previsto positivo. Folga real com escala certa e previsto zero pode ser bloqueada, inclusive no feriado. O corte de **dobra** é independente: `CICLOS_FERIADO_SIMPLES` põe o 12x36 em horas simples e a CCT cadastrada pode reativar a dobra. `app/ponto/motor_calculo_v2.py:1567–1586,2574,2911–2945,3740,3810–3821` · `app/core/regua_cct.py:473–479` · `app/ponto/tests/test_he100_dia_sem_previsto.py:39–48,208–212`. |
| **f. Intermitente** | Passa pela chamada comum. A fábrica escolhe `MotorIntermitente` por ciclo ou base e o construtor injeta calendário vazio, logo `eh_feriado` é falso. O laço pula o juízo de folga do vínculo intermitente. Na fixture vigente, sai em `:1564` e apura o par. O `elif dias_folga` ainda existe para folga semanal configurada. Ocorrência desse cadastro na frota [nao verificado]. `app/ponto/motor_calculo_v2.py:1000–1011,2474–2478,2558–2574,3694–3698,3764–3767` · `app/ponto/tests/test_s6_intermitente_feriado.py:105–152` · `app/docs/PROMPTS.md:1231,2536`. |

## 2. Escritores, leitores e risco de duplicação

| Escritor ou leitor | Efeito ao preservar o par |
|---|---|
| **Motor e composição das fatias** | O cálculo por par carimba entrada, saída e intervalos e calcula minutos, noturno, intra e HE. A L-097 sobrescreve os campos e publica a pendência. O fechamento concatena as listas das fatias. Com a exceção, os valores calculados chegam aos leitores e o acréscimo de fora da janela deste bloco deixa de existir. O teto L-093 lê `trabalhado + fora`, cuja soma se conserva nessa troca. `app/ponto/motor_calculo_v2.py:1276–1278,1585–1602,1946–1955,2875–2932,3353–3389` · `app/ponto/services/fechamento.py:116–120`. |
| **`ResultadoMes` e fechamento atual** | HE50/100 do motor soma `periodos`. Noturno e intra incluem também `periodos_ft`. O fechamento consulta `_certa` sobre FT, usa `feriado_vira_folga` para separar a certa, promove o feriado pelo trabalhado preservado, põe a outra certa em FT e a incerta em trabalhadas. `app/ponto/motor_calculo_v2.py:295–370` · `app/ponto/services/fechamento.py:938–1009,1028`. |
| **Juiz de dobra e partição c11** | `_feriado_dobra_no_dia` lê política do ciclo e calendário no dia do turno. Ele já marca HE100 no cálculo por par. A partição c11 consulta esse juiz no contexto do dia, escolhe uma única rubrica para cada FT e entrega as mesmas listas ao mensal, ao `DiaPago` e ao oráculo. Preservar o par dá minutos a essa partição. `app/ponto/motor_calculo_v2.py:941–978,2925–2932,3385–3392` · `c11/app/ponto/calculador/regras.py:386–453` · `c11/app/ponto/services/fechamento.py:995–1021,1237–1241,1296–1320`. |
| **`segmentos_do_motor` e `regras.py`** | O endereço vivo é `app/ponto/calculador/alimentacao.py`. A função lê `efetivo_entrada/saida`, fora da janela, pontas HE e intervalos. Hoje descarta o par colapsado e conta `par_bloqueado_sem_previsto`. A lista de bloqueados faz `rubricas_por_dia` visitar o dia para afirmar zero. Preservado o par, o dia chega pelo segmento válido. Na base, a Regra 4 põe FT incerta em trabalhadas pela L-007. No c11, o oráculo recebe a projeção da partição de FT e usa suas rubricas. `app/ponto/calculador/alimentacao.py:35,73–123,124–159,459–471` · `app/ponto/calculador/regras.py:674–725` · `c11/app/ponto/calculador/alimentacao.py:353–389,512–518,571–591` · `c11/app/ponto/calculador/regras.py:767–818`. |
| **`DiaPago`, espelho, PDF e app** | A lavra lê minutos e rubricas da lista correspondente. Na FT promovida, registra também `folga_feriado_min`. `pago_do_dia` soma trabalhadas, FT e esse detalhe uma vez. Uma linha toda zerada devolve `None`. Com a cura, os leitores passam a mostrar as horas lavradas. O espelho/PDF também leem os alertas do par. O alerta L-097 some no caso liberado. O aviso genérico de FT, que ainda diz “Requer validacao”, nasce depois do bloco. `app/ponto/services/dia_pago.py:175–185,223–267,867–898` · `app/ponto/services/espelho.py:51–105,669–671,825–833` · `app/api/views.py:1517–1536` · `app/relatorios/pdf_espelho.py:973–977` · `app/templates/ponto/espelho.html:302–304` · `app/ponto/motor_calculo_v2.py:2598–2605`. |
| **Gestão de HE e portão** | O espelho traduz a lista do motor em `he_fora_da_janela`, com `sentido='dia'`. O retrato lavrado alimenta Gestão, lote e contador. Na próxima lavra do retrato, desaparece a pendência produzida por esse bloco. O portão deixa de contar essa ocorrência quando ela era a que travava por estar acima do limite, sem decisão e com a trava ligada. Marcações ordinárias de ponta têm origem própria. `app/ponto/services/espelho.py:735–763,790` · `app/ponto/services/he_pendente_lavrado.py:138–159,183–195` · `app/ponto/services/gestao_he.py:426–434,488–504` · `app/folha/porta_export.py:181–201,229–242,501,682–697`. |
| **TXT, 59-A e conferências** | O export separa HE100 comum pela diferença `HE100 total − HE100 feriado` e emite o evento de feriado uma vez. O split noturno de HE é subconjunto. Na base, o filtro 59-A é feito na emissão. No c11, desde 11/2026, a emissão lê o mensal já classificado por fatia e mantém a leitura histórica pelo 59-A. O cartão c11 lê o mesmo evento. O portão e os comandos de DIFF também leem minutos para conferência. `app/folha/export.py:402–448` · `c11/app/folha/export.py:383–394,434–462` · `c11/app/relatorios/cartao_pela_celula.py:90–100` · `app/folha/porta_export.py:106–111,541–554` · `app/ponto/calculador/periodo.py:747–790` · `app/ponto/management/commands/diff_calculador.py:253–268`. |

**Risco de pagar duas vezes.** O caminho normal de dobra existe e consome `periodos`. A promoção de FT consome `periodos_ft`. A separação por identidade impede que o mesmo FT promovido também entre em trabalhadas comuns. O próprio código registra a regressão anterior que fazia isso, inclusive duplicando noturno e intra. O RED deve cobrar uma parcela de 240 min no pago do dia e um único evento de pagamento da hora. Em um feriado de `periodos`, trabalhadas e adicional de feriado são parcelas da dobra do ciclo. `app/ponto/motor_calculo_v2.py:295–344,2581–2607` · `app/ponto/services/fechamento.py:975–1009` · `app/ponto/services/dia_pago.py:175–185,244–267,894–898` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:67–92`.

## 3. Os 240 min e a rubrica

| Cenário com o mesmo par líquido de 240 min | Resultado |
|---|---|
| **Hash solicitado, escala incerta** | **240 min em `horas_trabalhadas`**, sem adicional de 100% dessa FT. Retorno do motor → `_ft_sem_escala` → mensal e `DiaPago`. `app/ponto/motor_calculo_v2.py:1579–1584` · `app/ponto/services/fechamento.py:999–1009` · `app/ponto/services/dia_pago.py:259–267`. |
| **c11, competência 11, 6x1 elegível, incerta** | **240 min em HE100-feriado**, evento `hora_extra_feriado_diurna`. A fixture verifica LIMBO, A2 e A8. O controle histórico de outubro conserva trabalhadas comuns. `c11/app/ponto/calculador/regras.py:371–377,415–432` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:94–108` · `c11/app/ponto/tests/test_he100_dia_sem_previsto.py:234–268`. |
| **c11, 6x1 certa, após preservar o par** | **240 min em HE100-feriado** se o vínculo faz o feriado virar folga e o ciclo dobra. **240 min em FT** se já era uma folga da célula e o vínculo trabalha em feriados. O RED suspenso usa esta segunda configuração. Decidem `escala_certa_no_dia`, `feriado_vira_folga` e o juiz de dobra, consumidos por `particionar_ft`. `app/escala/services/escala_certa.py:19–51` · `app/escala/models.py:1384–1406` · `c11/app/ponto/calculador/regras.py:371–377,415–453` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:110–118,547–563`. |

O código efetivo do Domínio vem de `RubricaDominio` ativa, com empresa prevalecendo sobre global. **0200** aparece para feriado em cadastro explícito da fixture c11. As migrações também usam 200 para FT global e HE100 comum global. **0243** é o padrão de intrajornada. O código cadastrado para J.A., a emissão efetiva e qualquer igualdade em reais [nao verificado]. O anexo fixa a igualdade em **240 minutos pagos**. `c11/app/folha/export.py:1008–1015` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:151–176` · `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1–11` · `app/folha/migrations/0009_evento_he100_rubrica_200.py:1–10` · `app/ponto/models.py:987` · `LEIS-2711-1610.txt:1(C)`.

## 4. Leis herdadas

| Lei | Consequência para a obra |
|---|---|
| **L-097** | A janela de HE tem cadastro, decisão e portão. O aval substitui o corte anterior para o feriado trabalhado no bloco sem previsto e preserva o bloqueio da folga comum certa. O registro da L-097 também traz a emenda RAMO2 para a ponta de saída, uma regra distinta deste bloqueio integral. `app/docs/LEIS.md:115` · `app/ponto/motor_calculo_v2.py:1536–1552,1847–1876` · `aval.txt:1`. |
| **L-007 e HE100-ALCANCE-A-PREMISSA** | Escritor canônico e resultado explícito. A folga incerta escapa do bloqueio pela autoridade `escala_certa_no_dia`. O corte anterior ainda incluía feriado no alcance, ponto que o novo aval altera ao menos para o RED de folga certa. `app/docs/LEIS.md:65` · `app/docs/PROMPTS.md:1514–1516` · `app/ponto/calculador/regras.py:689–705` · `aval.txt:1`. |
| **FERIADO-UM-JUIZ-2711-1210** | O anexo C exige o mesmo trabalhado nos dois estados de certeza, com a rubrica resolvida pela regra existente. O `!` posterior abre o motor no bloco citado e resolve a proibição de tocá-lo registrada na parada. Fechamento e oráculo já têm a partição compartilhada no c11. `LEIS-2711-1610.txt:1(C)` · `parei.txt:1` · `aval.txt:1` · `c11/app/ponto/calculador/regras.py:371–453` · `CLAUDE.md:17–22`. |
| **L-092, L-113 e L-095** | O corte desta obra exige exportada intocada, lista nominal da comp. 11 publicada antes do apply e reversão. A permissão genérica da L-113 para recalcular certas exportadas não serve como guarda da obra. L-095 põe os leitores sobre o dinheiro lavrado pela porta. `app/docs/LEIS.md:113,125,155` · `CLAUDE.md:453–465` · `aval.txt:1`. |
| **BUG-144** | O realizado vem dos pares fechados pelo juiz, descontadas as pausas julgadas, com a borda posicional prevista pela L-115. A cura preserva o par efetivo que o motor já calculou, lido também pelo oráculo. `app/docs/TICKETS.md:1006` · `app/docs/LEIS.md:153` · `app/ponto/turnos.py:707–771,774–795` · `app/ponto/calculador/alimentacao.py:114–159`. |

## 5. Casos para o RED

| Cadastro e batidas | Resposta pela regra e molde |
|---|---|
| **Feriado em folga certa e incerta.** 6x1 comercial 07–15, comp. 11, feriado 02/11, célula `trabalha=False`, E07/S11 e janela vigente. Rodar a mesma batida com LIMBO e com escala certa, sem decisão e com autorização como controle. | **240 min nos dois.** Com `trabalha_em_feriado=True`, incerta → HE100-feriado e certa → FT. Repetir com `False` para provar certa → HE100-feriado quando o feriado causa a folga. Conferir mensal, as duas lavras, `pago_do_dia` e evento. O skip já contém a primeira comparação. Para a janela por cadastro, reaproveitar os campos da fixture da base. `c11/app/ponto/tests/test_feriado_um_juiz.py:9` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:23–118,547–563` · `app/ponto/tests/test_he100_dia_sem_previsto.py:31–34`. |
| **Folga comum certa.** Janela vigente, previsto zero, E06h30/S12h, sem decisão. | **0 pago, 330 fora, par colapsado, alerta L-097, pendência `sentido='dia'`**, zero nas duas lavras. `app/ponto/tests/test_he100_dia_sem_previsto.py:55–81,120–123,151–159,203–206,294–297` · `app/ponto/motor_calculo_v2.py:1585–1602`. |
| **Feriado sem folga.** 6x1, `trabalha_em_feriado=True`, célula de trabalho com marcos completos, E07/S11 dentro da janela. Repetir com marcos incompletos e afirmar previsto zero. | Com previsto positivo, preserva **240 min** e classifica a dobra pelo ciclo. Com previsto zero, hoje bloqueia. O esperado novo dessa segunda variante depende da pergunta 0. `app/escala/utils.py:2319–2325` · `app/ponto/motor_calculo_v2.py:1562–1602,3380–3392` · `app/ponto/tests/test_feriado_sumula146.py:68–86`. |
| **12x36 no feriado.** Plantão previsto E07/S19 no molde vigente, com e sem cláusula. Em separado, folga real de escala certa com janela vigente, E06h30/S12h. | Plantão previsto conserva o corte de horas simples no piso e a dobra com cláusula. A fixture com cláusula fixa **720 trabalhados e 720 de HE100-feriado**, sem pendência. O controle da folga real hoje fixa **zero nas duas lavras**. `app/ponto/tests/test_feriado_12x36_art59a.py:156–169,226–234` · `app/ponto/tests/test_s6_intermitente_feriado.py:168–177` · `app/ponto/tests/test_he100_dia_sem_previsto.py:39–48,179–193,208–212`. |
| **Intermitente em feriado e oráculo do mesmo par.** Intermitente E07/S19 na fixture vigente. Para o 6x1 liberado, entregar o próprio `ResultadoMes` do RED a `segmentos_do_motor`. | Intermitente **720 trabalhados, zero HE100-feriado, sem pendência integral**. Para o 6x1, cobrar E07/S11 nas pontas efetivas, **240 min** em `pares_por_dia`, ausência de bloqueio e paridade das lavras. Para a folga comum, cobrar o descarte do par e `par_bloqueado_sem_previsto`. `app/ponto/tests/test_s6_intermitente_feriado.py:105–152` · `app/ponto/tests/test_he100_dia_sem_previsto.py:259–297` · `app/ponto/calculador/alimentacao.py:73–123,157–159`. |

## 6. Arquivos e pouso [nao verificado]

- A mudança de produção cabe no bloco de `app/ponto/motor_calculo_v2.py`. Os testes de integração se sobrepõem à obra em voo, que já mudou fechamento, `dia_pago`, partição, alimentação e export. O RED mais direto para abrir é `test_RED_c4_l097_certeza_so_decide_rubrica_com_ou_sem_autorizacao`. O caracterizador atual `test_RED_folga_feriado_bloqueada_oraculo_zera_como_o_motor` é **12x36 de outubro**. Para provar a inversão autorizada do 6x1, usar a variante da comp. 11. A inversão da asserção do 12x36 depende da pergunta 1. `aval.txt:1` · `app/ponto/tests/test_he100_dia_sem_previsto.py:39–48,208–212` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:547–563` · `c11/app/ponto/services/fechamento.py:995–1000,1296–1320` · `CLAUDE.md:318–320,441–442`.
- O `expectedFailure` de troca 6x1→12x36 na borda 20/21 carrega a referência ao PAREI, mas seu helper desliga a janela e força certeza falsa. Ele não demonstra a falha L-097. Para TXT ponta a ponta, há ainda uma borda: a exceção de `horas_trabalhadas == 0` no c11 reconhece feriado emitível, mas a condição não inclui FT. O `_confere` da fixture verifica a função de eventos e as lavras, sem provar a passagem pelo classificador nesse mês composto só por FT. `c11/app/ponto/tests/test_feriado_um_juiz.py:903–922,942–959` · `c11/app/folha/export.py:924–960` · `c11/app/ponto/tests/test_feriado_um_juiz_porta.py:67–92`.
- **Comp. 11 J.A., `colab | dia | minutos` que passam a pagar: [nao verificado].** Os anexos trazem o alvo do RED e o requisito de publicação. A lista nominal, a situação de exportação da competência e a prova de reversão [nao verificado]. Esses itens seguem como condições do apply. `aval.txt:1` · `LEIS-2711-1610.txt:1(C)` · `CLAUDE.md:453–465`.

## 7. Padrão e perguntas de lei

**Padrão:** `Single Writer Principle` (*juiz único / escritor único*). O motor decide a elegibilidade do par. A partição existente escolhe a rubrica e as projeções leem essa decisão. No pouso, a reconstrução de projeções da competência aberta segue o padrão *projection rebuild* da L-121. `app/docs/GLOSSARIO.md:11–12,25–26` · `app/docs/PLANO-PADROES.md:28–34` · `c11/app/ponto/services/fechamento.py:995–1000,1296–1320`.

0. **Abre — feriado sem folga e com previsto zero.** A frase “feriado trabalhado [...] não entra no filtro” inclui o par que só chegou por `eh_feriado`, ou o `!` vale só para feriado em folga com escala certa, como pede o PAREI? Se também abranger feriado com marcos, a janela ordinária roda antes e está fora do bloco autorizado. `aval.txt:1` · `parei.txt:1` · `app/ponto/motor_calculo_v2.py:1562–1586,1865–1876,2571–2574`.
1. **12x36 em folga real no feriado.** “Segue pelos cortes vigentes” preserv

[truncado em 20000 de 20787 caracteres]

## 27/11 21:11 · sessao 346c9d3e4456 · - · - · origem=Ronald (tmux)

**Base integrada recomendada: c11 da FERIADO-UM-JUIZ.** `main` aponta para `59600163c` e o worktree indicado para `1d2eb496c`. A comparação Git encontrou 22 arquivos modificados e o motor idêntico. O c11 já tem o julgamento de rubrica, a prova de paridade e o RED exato suspenso. O commit declara que ainda aguardava certificação e pouso. A cura de produção cabe no bloco do motor e a prova se beneficia dos testes da obra em voo. `/home/ronald/saas-hasner/.git/packed-refs:165`, `/home/ronald/saas-hasner/.git/worktrees/wt-codex-feriado-um-juiz-c4-152427/HEAD:1`, `/home/ronald/saas-hasner/.git/worktrees/wt-codex-feriado-um-juiz-c4-152427/logs/HEAD:9`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:547`.

**Leis herdadas.**

- L-097 bloqueia HE de janela e publica pendência. A emenda RAMO2 preserva trabalhado e noturno após a saída e mantém a classificação da HE. O aval novo excepciona feriado no bloco nomeado. `app/docs/LEIS.md:115`, `app/ponto/motor_calculo_v2.py:1536–1602`, `aval.txt:1`.
- L-007 exige escritor canônico e trilha. A aplicação de “nada em branco” sobre folga incerta paga hora comum no caminho histórico. HE100-ALCANCE-A-PREMISSA exclui folga incerta do bloqueio. O critério posterior exige classificação nominal de cada dia como folga certa ou feriado. `app/docs/LEIS.md:65`, `app/ponto/calculador/regras.py:389–394`, `app/docs/PROMPTS.md:1514–1516`.
- FERIADO-UM-JUIZ mantém o juiz `_feriado_dobra_no_dia` e classifica FT no c11. A escala certa, o ciclo e `feriado_vira_folga` determinam o destino. A regra dos 240 minutos iguais consta do anexo C. O aval completo 2711-1210 segue [nao verificado]. `LEIS-2711-1610.txt:1`, `parei.txt:1`, `app/ponto/motor_calculo_v2.py:941–961`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/calculador/regras.py:371–432`, `app/escala/models.py:1384–1406`.
- L-095 impõe uma lavra de referência do dinheiro. BUG-144 preserva pares fechados do juiz, desconta pausas julgadas e tem a exceção posicional L-115. Corrigir a origem e usar o mesmo par no oráculo preserva ambos. `app/docs/LEIS.md:113,153`, `app/docs/TICKETS.md:1006`, `app/ponto/motor_calculo_v2.py:1591–1597`.
- L-092 deve ser lida com L-113 e a guarda atual, que contém 09/2026. A restrição desta obra é exportada intocada e DIFF nominal da 11/J.A. antes do apply com reversão. O dado entregue não contém linhas colab/dia/minutos nem estado de exportação da 11/J.A. [nao verificado]. `app/docs/LEIS.md:125,155,183`, `CLAUDE.md:453–465`, `app/ponto/services/fechamento.py:36–50,559–565,578–595`, `aval.txt:1`.

**RED e controles.** O c11 já tem 6x1 de 02/11, E 07:00/S 11:00, LIMBO para incerta e `None` para certa, com/sem autorização. Espera 4h HE feriado na incerta e 4h FT na certa se `trabalha_em_feriado=True`. A variante que faz feriado virar folga espera as mesmas 4h HE feriado na certa. A fixture compara mensal, duas lavras, `pago_do_dia` e eventos TXT. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:23–118,547–563`.

O teste existente que exige zero em feriado/certa é **12x36**, e seu conteúdo permaneceu no c11. Serve de guarda se “12x36 intocado” preservar o bloqueio em folga/certa. Para inverter com 6x1, use o RED suspenso. A mesma bateria cobre folga comum zerada, +330 fora da janela e publicação de pendência. `app/ponto/tests/test_he100_dia_sem_previsto.py:39–47,151–159,203–212`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_he100_dia_sem_previsto.py:208–212,250–268`.

**Perguntas de lei.**

0. Abre a obra liberar só a folga certa de ciclo com dobra ou também a folga certa 12x36? O bloco atual alcança 12x36 em folga com previsto zero. A regra 59-A e a cláusula são outro corte. `aval.txt:1`, `app/ponto/tests/test_he100_dia_sem_previsto.py:39–47,208–212`, `app/ponto/motor_calculo_v2.py:1562–1589,3733–3740`, `app/core/regua_cct.py:473–479`.
1. No feriado fora de folga com previsto zero, o par continua bloqueado como hoje ou entra na frase ampla do aval? Para provocar o caso, use 6x1, célula `trabalha=True`, janela vigente, marcos incompletos e E 07:00/S 11:00. Um irmão com previsto positivo sai do bloco em `:1567`. `parei.txt:1`, `aval.txt:1`, `app/escala/utils.py:2319–2325`, `app/ponto/motor_calculo_v2.py:1562–1589`.
2. A dispensa do filtro inclui pontas de jornada regular ou a mensagem posterior “Requer validação do responsável”? A janela regular e essa mensagem estão fora do bloco autorizado. `aval.txt:1`, `app/ponto/motor_calculo_v2.py:1865–1876,2571–2574,2598–2605`.

**Padrão:** *single source of truth / um juiz por pergunta*. O motor escreve o par efetivo, a partição chama o juiz de dobra e o oráculo lê as mesmas pontas. `app/docs/PLANO-PADROES.md:49,55–62`, `app/docs/GLOSSARIO.md:12`, `CLAUDE.md:17–22,182`, `app/ponto/motor_calculo_v2.py:1591–1597`, `~/wt-codex-feriado-um-juiz-152427/app/ponto/calculador/regras.py:410–432`.

## 27/11 21:11 · sessao 9b1e28f281d8 · - · - · origem=Ronald (tmux)

Censo concluído em leitura. As referências `wt` são da worktree `~/wt-codex-feriado-um-juiz-152427`, no commit `1d2eb496c`. O hash fornecido é ancestral direto da série e o motor ficou sem diff entre os dois pontos. [Verificação Git sem arquivo:linha — nao verificado pelo material citado.]

### Total e rubrica

- Na cópia do hash, o feriado em folga com escala incerta retorna antes do bloqueio e preserva os minutos líquidos. O mensal e o `DiaPago` os registram em `horas_trabalhadas`, sem o adicional de 100%. Um par 07h–11h vale 240 min; a cópia atual não gera evento TXT de `horas_trabalhadas`. `app/ponto/motor_calculo_v2.py:1579-1586`, `app/ponto/services/fechamento.py:999-1009`, `app/ponto/services/dia_pago.py:259-267`, `app/folha/export.py:439-461`.
- Na worktree, desde a comp. 11, o teste pulado usa 6x1 comercial, `trabalha_em_feriado=True`, célula de folga e 07h–11h. Incerta espera 240 min de HE100 feriado. Certa espera 240 min de folga trabalhada. Com `trabalha_em_feriado=False`, a certa também vai para HE100 feriado. O juiz é `particionar_ft` → `_feriado_dobra_no_dia`, `escala_certa_no_dia` e `feriado_vira_folga`. `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:23-38`, `:98-118`, `:547-563`, `wt/app/ponto/calculador/regras.py:371-377`, `:386-453`, `wt/app/folha/export.py:383-394`.
- Os eventos são `hora_extra_feriado_diurna` e `folga_trabalhada_diurna`. O cadastro `RubricaDominio` resolve empresa sobre global. A fixture cadastra `200` para o evento feriado e prova `0200` no TXT. `0243` é o default legado de intrajornada. Código ativo na J.A. [nao verificado]. `app/folha/export.py:439-448`, `:739-748`, `:993-1000`, `app/ponto/models.py:980-987`, `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:151-176`.

### Escritores, leitores e efeito

- O motor escreve as rubricas zeradas e soma trabalhado a `minutos_fora_janela`, colapsa a ponta efetiva, acrescenta o alerta e publica dia/minutos/hora. O carimbo original vem de `_carimbar_segmento` e a publicação sai em `ResultadoMes`. A composição do fechamento concatena a publicação. `app/ponto/motor_calculo_v2.py:1585-1602`, `:1954`, `:2833`, `app/ponto/services/fechamento.py:116-117`.
- `ResultadoMes` soma HE de `periodos`, e noturno e intra de `periodos` + `periodos_ft`. A dobra já escrita no par não gera outro pagamento para FT porque entra apenas em `periodos_ft`. A partição do mensal é disjunta por identidade e a lavra recebe as mesmas listas. O TXT desconta o subconjunto de feriado do total HE100 antes de emitir a HE100 normal. Duplicação reaparece se o mesmo FT entrar em dois destinos ou se o subconjunto for somado ao pai. `app/ponto/motor_calculo_v2.py:294-344`, `:370`, `:2581-2607`, `:2925-2932`, `:3385-3392`, `app/ponto/services/fechamento.py:978-1009`, `:1227`, `app/folha/export.py:420-446`.
- O oráculo lê pontas de **`app/ponto/calculador/alimentacao.py`**, soma `fora_por_dia`, distingue `par_bloqueado_sem_previsto` por dia/hora e descarta o colapso. Com a liberação volta a segmentar a duração efetiva, respeitando o intervalo que o motor descontou. A rubrica vem da partição compartilhada na worktree. Em paralelo, a releitura diagnóstica por período também recusa pontas colapsadas e usa `fora` para o teto. `app/ponto/calculador/alimentacao.py:73-159`, `:314`, `:459-471`, `wt/app/ponto/calculador/alimentacao.py:353-389`, `:512-523`, `app/ponto/calculador/periodo.py:608-706`, `:747-790`.
- O `DiaPago` registra HE100 feriado/FT/trabalhadas conforme o destino, junto de noturno e intra. `pago_do_dia` soma trabalhadas, FT e o detalhe `folga_feriado_min`. A linha só com rubricas zero recebe `None`. Os leitores de tela e conferência usam a lavra e os totais HE, sem outro evento de pagamento. `app/ponto/services/dia_pago.py:223-267`, `:867-928`, `app/ponto/services/espelho.py:421`, `app/folha/porta_export.py:118-121`.
- O espelho traduz a publicação em `he_fora_da_janela` com `sentido='dia'`. Dali saem PDF, retrato, listagem, lote e trava de export. Sem a publicação, o dia deixa de ser pendência integral no próximo retrato. O alerta L-097 também some, mas o motor ainda pode acrescentar `FOLGA TRABALHADA` e o espelho considera qualquer alerta para inconsistência. `app/ponto/services/espelho.py:671`, `:735-763`, `:790`, `:939-948`, `app/ponto/motor_calculo_v2.py:2598-2605`, `app/relatorios/pdf_espelho.py:931-932`, `:975`, `app/ponto/services/he_pendente_lavrado.py:68`, `:138-159`, `app/ponto/services/gestao_he.py:426-432`, `:488-504`, `app/folha/porta_export.py:181-201`, `:229-242`, `:687-697`, `:864-871`.
- A L-093 soma `minutos_trabalhados + minutos_fora_janela`. A mesma parcela mudando de coluna conserva essa soma. `app/ponto/motor_calculo_v2.py:1260-1278`.

### Bordas para levar ao dossiê

- O aviso do aval tem numeração antiga. No vivo, o bloco mutante vai até `1602`, incluindo colapso e publicação. Uma guarda colocada no começo da faixa autorizada consegue evitar todas essas escritas sem editar suas linhas; o sentido literal da faixa é questão de lei caso se exija alterar diretamente o colapso. `app/ponto/motor_calculo_v2.py:1574-1602`.
- O teste existente que bloqueia feriado em folga é **12x36 em outubro**, portanto constitui controle se o corte vigente de 12x36 for preservado. Para inverter use o RED pulado de novembro 6x1. A worktree já sela outubro 6x1 incerto comum e novembro 6x1 incerto HE100 feriado. `app/ponto/tests/test_he100_dia_sem_previsto.py:39-48`, `:175-212`, `wt/app/ponto/tests/test_he100_dia_sem_previsto.py:227-268`, `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:547-563`.
- Uma exceção por `eh_feriado` puro alcançaria o 12x36 em folga sem previsto; já o 12x36 em plantão com previsto positivo sai cedo. O intermitente limpa inclusive o calendário injetado, de modo que percorre o método, mas o feriado não ativa o filtro como feriado. `app/ponto/motor_calculo_v2.py:1562-1568`, `:2558-2574`, `:3686-3698`.
- Sem FT e com previsto 0, o feriado entra no bloqueio sem consultar certeza. Essa combinação pode nascer de marcos incompletos. Com previsto positivo, a regra integral não entra. A frase ampla do aval deixa em aberto se muda também o primeiro caso e se a janela ordinária do segundo permanece aplicável às pontas quando o dia dá dobra. `app/ponto/motor_calculo_v2.py:1562-1586`, `:1634-1660`, `app/escala/utils.py:2310-2325`.
- Tem mais uma borda de emissão a testar se “paga” for exigido ponta a ponta: desde comp. 11, a exceção ao gate de `horas_trabalhadas=0` é `feriado_emitivel`, condicionada à rubrica de feriado cadastrada. Ela não cobre um mensal todo zerado em trabalhadas que só tenha a rubrica de FT, que é um dos destinos do RED pulado. O próprio RED confere mensal, enquanto outros testes conferem TXT. `wt/app/folha/export.py:924-926`, `wt/app/ponto/tests/test_feriado_um_juiz_porta.py:151-176`, `:547-563`.

## 27/11 21:10 · sessao 3cbd4a5f872b · - · - · origem=Ronald (tmux)

- O RED da obra em voo está suspenso em `test_feriado_um_juiz_porta.py:547-563`. A fixture usa 6x1 comercial, célula de folga em 02/11 e batidas 07h–11h. Para escala incerta, espera 240 min em HE100-feriado. Para certa e `trabalha_em_feriado=True`, espera 240 min em FT. Para certa e `trabalha_em_feriado=False`, o controle espera 240 min em HE100-feriado. A função `_confere` também verifica export, mensal, lavra motor e oráculo, e a soma única de quatro horas. `test_feriado_um_juiz.py:9` e `test_feriado_um_juiz_porta.py:16-118`.
- O teste que fixa o bloqueio do feriado na base é `app/ponto/tests/test_he100_dia_sem_previsto.py:208-212`, com fixture **12x36** e competência 10, `:39-48,179-193`. Se o corte preservar esse bloqueio de 12x36 em folga, conservar a asserção e acrescentar o RED comercial em competência 11. A obra em voo já fixa 330 min de HE100-feriado para 6x1 incerta em novembro, e 330 min de trabalhadas para 12x36 incerta ou 6x1 incerta em outubro. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_he100_dia_sem_previsto.py:227-268`.
- A folga comum certa, 06h30–12h, mantém zero, 330 min fora, publicação do dia e pendência `sentido='dia'`. O oráculo descarta o par colapsado e conta `par_bloqueado_sem_previsto`. No feriado liberado, verificar o mesmo carimbo não colapsado e total 240 em `pares_por_dia`, sem esse contador e sem a publicação de pendência do bloco. `app/ponto/tests/test_he100_dia_sem_previsto.py:55-81,120-123,151-159,259-275,294-297`, `app/ponto/calculador/alimentacao.py:73-95,114-123,157-159`.
- Para feriado com célula trabalha=True, marcos completos e previsto positivo, o bloco já retorna. O ciclo 6x1 dobra pelo juiz. Com flag de folga falsa e previsto 0 por marcos incompletos ou `trabalha=None`, mesmo feriado de escala incerta hoje bloqueia. O aval não resolve inequivocamente a diferença entre a ordem de pagar feriado direto e o alcance explícito da exceção de folga certa. `app/escala/models.py:1098-1107`, `app/escala/utils.py:2319-2325`, `app/ponto/motor_calculo_v2.py:1562-1584,2560-2574,3465-3478`, `app/ponto/tests/test_feriado_sumula146.py:59-79`.
- O 12x36 com plantão previsto e janela ligada tem controle CCT com 720 min trabalhados, 720 min de HE100-feriado e sem pendência. Sem cláusula, a fixture 59-A fixa hora simples e alerta próprio. No intermitente a fixture 07h–19h com feriado oficial e janela ligada fixa 720 min trabalhados, HE100 zerada e nenhuma pendência. O corte de calendário injetado vazio neutraliza feriado, mas uma folga semanal de cadastro pode entrar pelo `elif dias_folga`; ocorrência em produção [nao verificado]. `app/ponto/tests/test_s6_intermitente_feriado.py:105-152,168-177`, `app/ponto/tests/test_feriado_12x36_art59a.py:156-169,226-234`, `app/ponto/motor_calculo_v2.py:2436-2451,2475-2478,2560-2566,3694-3698,1000-1011`.
- A borda `@expectedFailure` da obra em voo traz “PAREI ! FERIADO-L097-MOTOR”, mas o helper desliga a janela e força certeza falsa. O teste não verifica o bloqueio L-097. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:903-922,942-959`. [nao verificado]
- O alerta genérico de FT sobre validar antes do fechamento é gerado depois do bloco e permanece visível se a cura sair apenas da L-097. A pendência da Gestão de HE vem de `dias_sem_previsto_fora_da_janela`, uma lista distinta. `app/ponto/motor_calculo_v2.py:2598-2605`, `app/ponto/services/espelho.py:669-671,757-760`, `app/templates/ponto/espelho.html:302-304`, `app/relatorios/pdf_espelho.py:973-977`.
- A obra testa 0200 para HE100-feriado com mapeamento explícito. As migrações põem FT e HE100 normal em 200 global, e a empresa pode substituir o global. 0243 é padrão de intrajornada. Código real da rubrica para cada empresa e lista de dias da competência 11 [nao verificado]. `~/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:151-176`, `app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:1-11`, `app/folha/migrations/0009_evento_he100_rubrica_200.py:1-10`, `app/folha/export.py:993-1000`, `app/ponto/models.py:987`.

## 27/11 21:05 · sessao 96fd5c7b7b0f · FIM-DE-SEMANA-JA · leitura · origem=Code

## 1. Dono L‑099 por linha

**11 CADASTRO, 7 ESTRUTURA, 0 BATIDA**, em 18 dias. As referências a `sombra.out` contam a linha `sonda_leitura` como linha 1. O `dono_motivo` veio `-` nas 18 linhas. O motivo abaixo vem de seguir os insumos até o retorno de `dono_da_divergencia`, chamado por `InsumosDoDono.dono`. `sombra.out:1–21`, `app/docs/LEIS.md:122`, `app/ponto/management/commands/e6_oraculo.py:182–195`.

- **Vínculo ausente.** `sem_vinculo_no_dia` e `sem_vinculo_em_dia_de_trabalho` vieram `true`. Esse ramo prevalece sobre as batidas. A geradora nomeada pela célula estava fora da vigência do dia, e a leitora do vínculo devolve `None` nessa condição. `app/ponto/management/commands/e6_oraculo.py:98–99`, `app/escala/alimentacao.py:269–284`, `app/ponto/supra_juiz.py:206–210`.
- **Retorno residual.** Os três indicadores de cadastro do primeiro ramo vieram `false`, com zero ou quatro horas no oráculo, sem batida fora de turno, fase deslocada ou presença do dia na lavra CxR. O último retorno dá `ESTRUTURA`. `sombra.out:14–21`, `app/ponto/management/commands/e6_oraculo.py:98–110`.

| Colab · dia de 2026 | Dono | Evidência do dia e ramo decisivo |
|---|---|---|
| col97 · 01/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:4`, `e6_oraculo.py:98–99`. |
| col97 · 03/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:5`, `e6_oraculo.py:98–99`. |
| col97 · 05/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:6`, `e6_oraculo.py:98–99`. |
| col97 · 07/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:7`, `e6_oraculo.py:98–99`. |
| col97 · 09/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:8`, `e6_oraculo.py:98–99`. |
| col97 · 11/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:9`, `e6_oraculo.py:98–99`. |
| col97 · 13/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:10`, `e6_oraculo.py:98–99`. |
| col97 · 15/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:11`, `e6_oraculo.py:98–99`. |
| col97 · 17/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:12`, `e6_oraculo.py:98–99`. |
| col97 · 19/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:13`, `e6_oraculo.py:98–99`. |
| col296 · 10/11 | ESTRUTURA | `COBRANCA_SEM_FURO`, 660 previstos e 660 realizados. Quatro horas no oráculo e retorno residual. `sombra.out:14`, `e6_oraculo.py:110`. |
| col327 · 10/11 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 60 min sem início e fim. Quatro horas e retorno residual. `sombra.out:15`, `e6_oraculo.py:110`. |
| col344 · 07/11 | ESTRUTURA | Célula `concorde`, agendamento 103 cobre o dia e há batida civil apurável. Quatro horas e retorno residual. `sombra.out:16`, `e6_oraculo.py:110`. |
| col348 · 20/11 | CADASTRO | Célula editada, indefinida, sem vínculo no dia, com 712 min realizados. `sombra.out:17`, `e6_oraculo.py:98–99`. |
| col354 · 21/10 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 120 min sem início e fim. Quatro horas e retorno residual. `sombra.out:18`, `e6_oraculo.py:110`. |
| col407 · 29/10 | ESTRUTURA | `FURO_PARCIAL`, 720 previstos e 0 realizados. Oráculo vazio e retorno residual. `sombra.out:19`, `e6_oraculo.py:106–110`. |
| col655 · 21/10 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 180 min sem início e fim. Quatro horas e retorno residual. `sombra.out:20`, `e6_oraculo.py:110`. |
| col673 · 21/10 | ESTRUTURA | `FURO_PARCIAL`, 720 previstos e 3 realizados. Oráculo vazio e retorno residual. `sombra.out:21`, `e6_oraculo.py:106–110`. |

Nos 11 casos de vínculo, a célula traz `PREVISTO_COM_DIA_INDEFINIDO`, e `cadastro.falta_previsto` aponta um vínculo que responda pela data. O retrato lavrado inclui as datas dos 11. Para col97, a lista de portas do retrato veio vazia. Para col348, trouxe **Vínculo da escala** e duas entradas **Resolver dia no calendário**. Essas portas vêm da linha do colaborador na competência. `sombra.out:4–13`, `sombra.out:17`, `app/folha/retrato_folha.py:930–935`, `app/folha/retrato_folha.py:1024–1059`.

**Onde o S2 divergiu.** O S2 tratou o protesto `PARCIAL_SEM_PERIODO` como se já fosse a decisão L‑099. O supra-juiz identifica corretamente o campo faltante e manda cobrar o cadastro. O cartório também prevê encaminhar a parcial sem período à lista do admin. Para a **contagem pedida**, a L‑099 aponta para `dono_da_divergencia`. Esse juiz recebe vínculo, folga, batidas, fase e CxR, e os três parciais caíram no retorno `ESTRUTURA`. Os dois `FURO_PARCIAL` tinham dono do fato em aberto no S2 e agora têm `ESTRUTURA` medido. O S2 já descrevia col296 como estrutural na seção de cobrança, mas o deixou fora da soma C1 de 14 + 2 + 1. A S3 também identificou nominalmente o caso de férias e mediu seu dono. `S2_r.md:11–14`, `app/docs/LEIS.md:122`, `app/ponto/supra_juiz.py:395–402`, `app/ponto/services/cartorio.py:423–446`, `app/ponto/management/commands/e6_oraculo.py:53–54`, `app/ponto/management/commands/e6_oraculo.py:98–110`, `sombra.out:14–21`.

## 2. Lista para o Mauricio

**Vínculo da escala**, formulário **Vincular Escala**, é a tela indicada em `tela.previsto` para cada linha abaixo. URL name `colaboradores:painel_vinculo`, template `colaboradores/partials/painel_vinculo.html`. `app/folha/retrato_folha.py:1003–1006`, `app/colaboradores/urls.py:12–13`, `app/colaboradores/views.py:306`, `app/templates/colaboradores/partials/painel_vinculo.html:128–137`.

| Colab | Dia de 2026 | Motivo para o DP e cadastro faltante | Tela |
|---|---|---|---|
| col97 | 01/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:4`. | Vínculo da escala |
| col97 | 03/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:5`. | Vínculo da escala |
| col97 | 05/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:6`. | Vínculo da escala |
| col97 | 07/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:7`. | Vínculo da escala |
| col97 | 09/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:8`. | Vínculo da escala |
| col97 | 11/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:9`. | Vínculo da escala |
| col97 | 13/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:10`. | Vínculo da escala |
| col97 | 15/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:11`. | Vínculo da escala |
| col97 | 17/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:12`. | Vínculo da escala |
| col97 | 19/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:13`. | Vínculo da escala |
| col348 | 20/11 | Houve trabalho registrado e falta vínculo de escala que cubra o dia. A geradora intermitente terminou em 20/10. `sombra.out:17`. | Vínculo da escala |

## 3. ESTRUTURA, ato faltante, escritor e obra

Nas sete linhas, a S3 encontrou `colab_na_lista=true`, `dia_na_lista=false`, `dono_lavrado=null`, `mesa_admin=false` e `destino_DP=-`. A consulta à lista distingue a presença do colaborador da presença daquele dia. Em todas, o calendário da tarefa veio `false` e o retrato de 27/11 inclui a data. `sombra.out:14–16`, `sombra.out:18–21`, `app/chamados/services/vigia_fio.py:202–241`.

| Caso · célula | Linha C1 preenchida com a evidência disponível | Ato, escritor canônico e BACKLOG |
|---|---|---|
| **col296 · 10/11** · 137154 | Chamado 38343 `turno_aberto` ainda aberto, 38318 resolvido. Perguntas 56532 e 56545 ancoradas em 38343, C6 `true`, sem resposta ou ato. A 56612 tem C6 `false` e projeção `fantasma`. Tarefa sem porta e fila vazia. Retrato com **Cobrar no fio** e **Vínculo da escala**. `sombra.out:14`. | Baixa de 38343 e rejulgamento, **se o gate autorizar**. O cartório chama a reconciliação em `app/ponto/services/cartorio.py:_julgar_colab_corpo:1618–1629`. A baixa passa por `app/chamados/models.py:ChamadoColaborador.reconciliar:661–675`. **E4B-SCA-25-COBRANCA-INDEVIDA** é a obra da classe. Inclusão nominal no estoque dos 25 e motivo da permanência [nao verificado]. Faltam `vivos`/`_protestam` da lavra e resultado ou recusa da reconciliação de 38343. `app/docs/BACKLOG.md:2318`, `app/ponto/services/cartorio.py:1498–1507`, `app/chamados/models.py:683–712`. |
| **col327 · 10/11** · 122970 | Declaração 5515, 60 min, `hora_ini=null`, `hora_fim=null`. Chamado 38375 `ausencia_colab` resolvido. Sem pergunta, ato, porta ou link no retrato. `sombra.out:15`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col344 · 07/11** · 136655 | Agendamento 103, de 06/10 a 07/11, concluído, aquisitivo 639 concedido. Batida civil 148780, `E`, às 18:33:49 locais. Chamado 37604 registrado. Pergunta 55062, âncora e fila 37605, C6 `true`. `porta_pergunta` publica `validar` com `nao_sei=true`, e a instância está `passivo_sem_linha`. Retrato com **Validar no fio**. `sombra.out:16`. | No fio, falta `DecisaoPergunta` de **resposta** para 55062 em `2026-11-07T21:33:56.900522+00:00`. Escritor do passivo abaixo. Para férias, **HJ‑090, item 7**, é a obra da lista com dono L‑099. Ela referencia `CASOS-103-114-PELA-PORTA`, cuja decisão depende das datas do DP. Falta a linha `Ausencia(tipo=ferias)` de 07/11, com status e agendamento de origem, para confirmar entrada no relatório da obra. A correção do agendamento ou da batida fica [nao verificado]. `app/docs/BACKLOG.md:2137`, `app/docs/BACKLOG.md:751`, `app/ferias/management/commands/listar_ferias_com_batida.py:1–12`, `app/ferias/management/commands/listar_ferias_com_batida.py:41–65`. |
| **col354 · 21/10** · 128888 | Declaração 5125, 120 min, `hora_ini=null`, `hora_fim=null`. Sem chamado, pergunta ou ato. Tarefa sem porta. Retrato com **Resolver dia no calendário**. `sombra.out:18`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col407 · 29/10** · 133670 | Chamado e âncora 35801 fechados. Pergunta 51727, C6 `true`, sequência vazia, resposta só na projeção. Instância `passivo_sem_linha`, fio `nao_sei`, sem rota ou fila. Retrato com duas entradas **Resolver dia no calendário**. `sombra.out:19`. | Falta `DecisaoPergunta` de **resposta** para 51727 em `2026-11-01T10:02:34.925352+00:00`. **FIO-E0-V4-E4B** é a obra do passivo. Escritor abaixo. `app/docs/BACKLOG.md:2315`. |
| **col655 · 21/10** · 129497 | Declaração 5076, 180 min, `hora_ini=null`, `hora_fim=null`. Chamados 33158 e 33163 resolvidos, com 33158 no cartório. Sem pergunta ou ato. Tarefa sem porta. Retrato com **Cobrar no fio**. `sombra.out:20`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col673 · 21/10** · 135708 | Âncora 33282 fechada. 33494 resolvido, 34029 fechado. A pergunta 46935 tem C6 `true`, sequência vazia e `passivo_sem_linha`. A 46936 tem C6 `false` e projeção materializada por `declaracao`. Fio `nao_sei`, sem rota, fila ou link no retrato. `sombra.out:21`. | Falta `DecisaoPergunta` de **resposta** para 46935 em `2026-10-22T10:08:24.022007+00:00`. **FIO-E0-V4-E4B** é a obra do passivo. O BACKLOG nomeia col673 e a pergunta **46936** em **DIA-DA-BATIDA-JUIZ-UNICO**, para a atribuição da batida ao turno. `app/docs/BACKLOG.md:2315`, `app/docs/BACKLOG.md:1864`. |

**Três parciais.** O ato de cadastro é salvar o início e o fim declarados pelo DP no painel `ponto:painel_ausencia`, template `ponto/partials/painel_ausencia.html`. O POST `ponto:editar_periodo_ausencia` chama `decidir`, que grava pela função `app/ponto/services/ausencia.py:_aplicar_periodo:1051–1054` e persiste em `app/ponto/services/ausencia.py:_decidir_no_ato:1101–1104`. Para o encaminhamento ausente, **COBPARC-B-CAUSA-NO-MOTOR** descreve a classe. O escritor atual das causas CxR é `app/ponto/motor_calculo_v2.py:_acusa_cadastro_x_realidade:1766–1783`, com chamadas nas pontas da janela de HE. A ausência das três datas na lista está medida. A causa exata da omissão e o escritor que deveria publicar essas três linhas ficam [nao verificado]. Faltam a assinatura por `(colab, dia)` na origem da lista e o resultado da sua publicação. **BO2910-ESTRUTURA (05)** cobre a aprovação pela tela sem exigir período. Para atribuir essa falha histórica a uma das três declarações, faltam canal e trilha da aprovação. `app/ponto/urls.py:68–72`, `app/ponto/views.py:2836–2849`, `app/ponto/services/cartorio.py:423–446`, `app/ponto/motor_calculo_v2.py:1822–1824`, `app/ponto/motor_calculo_v2.py:1860–1862`, `app/docs/BACKLOG.md:1105`, `app/docs/BACKLOG.md:1191`, `sombra.out:15`, `sombra.out:18`, `sombra.out:20`.

**Três respostas sem linha.** O escritor canônico é `app/chamados/models.py:DecisaoPergunta.registrar:4590–4609`. O backfill despacha a recomposição em `app/chamados/management/commands/backfill_fio_morto_e4b.py:_executar_dia:161–164`. A correspondência exige a mesma pergunta, o mesmo ato e o mesmo instante. **E4C-F8** trata da migração de leitores da projeção para a leitura do ato. A inclusão nominal de 51727 ou 46935 nas nove unidades dessa obra fica [nao verificado], pois falta identificar o leitor específico na lista de escopo da F8. `app/chamados/models.py:4416–4447`, `app/docs/BACKLOG.md:2211`, `app/docs/BACKLOG.md:2315`, `sombra.out:16`, `sombra.out:19`, `sombra.out:21`.

**Férias de col344.** A Folha já publica a retenção a partir do agendamento e do dia civil da batida. A S3 incluiu o caso pelo ramo sem porta **do tipo** `ferias`. A tarefa e o retrato puderam publicar **Validar no fio** pela pergunta. O ato de férias ainda depende de confirmar o gozo real, o dia de turno, a origem da batida e a decisão do DP. Se o DP confirmar datas diferentes, o escritor das datas é `app/ferias/services.py:_trocar_janela:694–729`. A tela encontrada é `ferias:secao_ferias_colab`, template `ferias/_secao_colab.html`. `s3.py:92–97`, `sombra.out:16`, `app/folha/export.py:490–504`, `app/folha/export.py:885–893`, `app/folha/retrato_folha.py:1003–1017`, `app/ferias/views.py:851–861`.

## 4. O que segue [nao verificado]

- **Identidade Git da cópia, execução independente da S3 e equivalência com produção.** A cópia consultada está sem metadados Git. O cabeçalho do anexo identifica a sonda, e o código traz as guardas de ambiente da sombra. O resultado disponível é a saída raspada fornecida. `sombra.out:1–3`, `s3.py:16–22`. [nao verificado]
- **Valores corretos e histórico das correções.** Faltam o vínculo e a vigência corretos para col97/col348, os horários declarados das três parciais, o retorno histórico do gate de 38343 e a decisão do DP sobre a sobreposição de férias. A saída mostra o estado consultado e a sobreposição civil. `sombra.out:4–21`, `app/chamados/models.py:683–712`, `app/folha/export.py:490–504`. [nao verificado]
- **Origem e elegibilidade das três respostas sem linha.** Faltam o escritor original, a trilha do ato, a presença no plano do backfill e o estado da competência no gate. A sonda mede a falta da linha, e o backfill ainda aplica seus critérios de alcance, competência e entrega. `sombra.out:16`, `sombra.out:19`, `sombra.out:21`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:307–343`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:383–405`. [nao verificado]
- **Alcance por um ator e efeito na Folha.** O anexo traz rótulo e URL name das portas do retrato. Faltam o destino completo de cada link, o perfil e a autorização do ator, o resultado da ação e uma nova classificação da Folha. Liberação do TXT e efeito em pagamento ficam [nao verificado]. `sombra.out:14–21`, `app/folha/retrato_folha.py:1024–1065`, `app/folha/export.py:699`, `app/folha/export.py:774`.

## 27/11 20:59 · sessao 5e148d3706d5 · FERIADO-UM-JUIZ · construtor · origem=Code

A c11 está na cópia. O RED de FT passou na cura e falhou nos três subcasos do mutante com `ValueError not raised`. O teste de `do_motor` também passou na cura e falhou na c10 e no mutante quando o dia estava sem entrada em `rubricas`. [RED de FT](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:395) · [RED de `do_motor`](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:418) [nao verificado]

### Diff por arquivo

| Arquivo | Diff | Alteração |
|---|---:|---|
| [dia_pago.py:316](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/dia_pago.py:316) | +4 −6 | Uma chamada a `_DONOS_DECLARADOS` por dia, com `do_motor` nos dois casos. Saiu o `or CAMPOS_DIA`. |
| [fechamento.py:682](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:682) | +1 −0 | Comentário do contador registra recusas por colaborador e competência e remete ao R1. |
| [test_feriado_um_juiz.py:395](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:395) | +45 −0 | REDs de FT isolado e de `do_motor`. O R7 ganhou a docstring de [caracterização](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:476). |
| [feriado_um_juiz_mutantes.py:167](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:167) | +15 −4 | Dois mutantes novos e ajuste do alvo textual do [M1](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:484). |

**`except` no diff dos quatro arquivos acima: 0.** O controle local rodou 52 testes, com uma falha esperada. O M1 produziu as duas falhas previstas. [Controle](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:716) · [Alvos do M1](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:122) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes controle
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes c11_foto_sem_dia_so_com_ft
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes c11_ignora_do_motor_sem_rubrica
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M1
```

### `--only` para o Code

Da pasta `app/`. Suite e savepoint real: **[nao verificado]**. [Teste do savepoint real](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:462) [nao verificado]

```bash
../bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto.tests.test_feriado_um_juiz ponto.tests.test_feriado_um_juiz_porta"
```

## 27/11 20:58 · sessao 7c0cb70cf686 · FIM-DE-SEMANA-JA · leitura · origem=Code

A S1 registrou 18 ocorrências em 9 colaboradores. `col296`, em 10/11, está entre as 17 linhas nominais. A ocorrência adicional é `ferias_com_batida`, publicada apenas no total. A identidade, o dia e as portas dessa ocorrência, assim como o dono L‑099 de cada linha, ficam [nao verificado]. A identidade Git da cópia e a execução da S3 também ficam [nao verificado]. `c1_s1.txt:26`, `c1_s1.txt:2972`, `c1_s1.txt:2980`, `c1_s1.txt:3006`. [nao verificado]

## Peça 1 — Sonda S3

A seleção conserva o teste de saída da S1. A sonda lê as portas do retrato gravado depois de selecionar o universo, pois o drawer também consulta o fio e pode publicar um link genérico em férias. `dono=` é a resposta de `InsumosDoDono.dono`, que chama `dono_da_divergencia` com os insumos da casa e a lavra CxR, seguindo o caminho do relatório de férias. A S3 usa um `AGORA` e um `HOJE`. A classificação do export mantém o relógio interno já documentado no caminho da S1. `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1010`, `app/folha/retrato_folha.py:1066`, `app/colaboradores/views.py:1001`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:53`, `app/ferias/management/commands/listar_ferias_com_batida.py:53`, `app/folha/fora_da_folha.py:137`.

```python
import sys

sys.dont_write_bytecode = True

import json
import logging
from collections import defaultdict
from datetime import datetime, time
from functools import lru_cache
from urllib.parse import urlsplit

from django.conf import settings
from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from django.contrib.auth.models import User
from django.db.models import Q
from django.urls import resolve, reverse

from chamados.juizes import dia_do_fato, pergunta_viva
from chamados.models import ChamadoColaborador, DecisaoPergunta, PerguntaDisputa
from chamados.services.fio import chamado_que_lista
from chamados.services.tarefa_do_dia import porta_da_tarefa, tarefa_do_dia
from chamados.services.vigia_fio import ListaDoAdmin, dono_fora_do_fio
from colaboradores.models import Colaborador, Empresa
from colaboradores.queries import empresas_visiveis
from colaboradores.services.calendario import resolver_dia_oferecido_nas_datas
from escala.alimentacao import celulas_do_periodo, vinculo_do_dia_carregado
from ferias.services import ferias_na_janela
from folha.fora_da_folha import _carga, _fio_da_porta, dias_que_seguram
from folha.models import RetratoFolha
from folha.retrato_folha import (
    PORTA_DO_TIPO, _balde, linha_do_colab, portas_da_linha, tipo_da_linha,
)
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import InsumosDoDono, batida_fora_de_turno
from ponto.services.cartorio import chamados_que_falam_de_batida
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.supra_juiz import cadastro_do_dia_carregado
from ponto.turnos import (
    batidas_apuraveis, cobre_parte_do_dia, cobertura_ausencia_periodo,
    periodo_da_parcial_no_dia,
)

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
ESPERADO = 18
NV = "[nao verificado]"
FONTE_DONO = "app/ponto/management/commands/e6_oraculo.py:182,53"
ESCRITOR_PASSIVO = (
    "app/chamados/management/commands/backfill_fio_morto_e4b.py:164",
    "app/chamados/models.py:4590",
)


def origem_s1(empresa, inicio, fim, lista):
    # Mesmo critério de pertencimento da S1 c1. As leituras extras vêm depois.
    u = dias_que_seguram(empresa, MES, ANO, hoje=HOJE)
    itens = sorted(
        (i for i in u["itens"] if _balde(i) == "bloqueado"),
        key=lambda i: i["colaborador_id"],
    )
    datas = {
        cid: [r["data"] for r in dias if inicio <= r["data"] <= fim]
        for cid, (_, dias) in u["dias"].items() if dias
    }
    carga = _carga([u["colabs"][cid] for cid in datas], inicio, fim, AGORA) if datas else None
    cel_ids = {
        r["celula_id"] for _, dias in u["dias"].values() for r in dias
        if inicio <= r["data"] <= fim and r["celula_id"] is not None
    }
    perguntas = defaultdict(list)
    for p in PerguntaDisputa.objects.filter(celula_id__in=cel_ids).select_related(
        "disputa__colaborador__empresa", "celula"
    ).prefetch_related("decisoes"):
        perguntas[p.celula_id].append(p)

    casos = []
    for item in itens:
        cid = item["colaborador_id"]
        if item["motivo"] != "furo_espelho":
            if not PORTA_DO_TIPO.get(tipo_da_linha(item)):
                for dia in item.get("dias_ferias_com_batida") or (None,):
                    if dia is None or inicio <= dia <= fim:
                        casos.append((item, dia, None, None))
            continue

        colab = u["colabs"][cid]
        _, dias = u["dias"][cid]
        if not dias:
            casos.append((item, None, None, None))
        calendario = resolver_dia_oferecido_nas_datas(colab, datas.get(cid, ()), agora=AGORA)
        for r in dias:
            dia, cel_id = r["data"], r["celula_id"]
            if not inicio <= dia <= fim:
                continue
            tarefa = tarefa_do_dia(colab, dia, AGORA, carga=carga)
            publicada = porta_da_tarefa(tarefa) [nao verificado]
            fio = _fio_da_porta(publicada) [nao verificado]
            fora, papel = dono_fora_do_fio(colab, dia, perguntas[cel_id], lista, com_papel=True)
            lista.dia_na_lista(cid, dia.isoformat())
            mesa = lista.dia_do_admin(cid, dia.isoformat())
            destino = lista.destino_fora_da_mesa(cid, dia.isoformat())
            cal = calendario[dia]
            if (
                ((publicada or {}).get("rota") and fio in ("admin", "colab")) [nao verificado]
                or cal is True or fora or mesa or destino
            ):
                continue
            casos.append((item, dia, r, (tarefa, publicada, fio, cal, fora, papel))) [nao verificado]
    return casos


def campos(obj, *nomes):
    return {nome: getattr(obj, nome) for nome in nomes} if obj is not None else None


def tela(nome, template, post=None, args=(), **dados):
    out = {"url": resolve(reverse(nome, args=args)).view_name, "template": template}
    if post is not None:
        out["post"] = resolve(reverse(post, args=args)).view_name
    out.update(dados)
    return out


def dados_vinculo(ec):
    if ec is None:
        return None
    return {
        **campos(ec, "pk", "ativa", "data_inicio", "data_fim", "data_ancora_colaborador"),
        "escala": campos(ec.tipo_escala, "pk", "tipo_ciclo", "folga_dia_semana", "editavel"),
    }


def cadastro_e_tela(colab, dia, cel, protestos, cadastro):
    cid = colab.pk
    dados, telas = {}, {}
    if "PREVISTO_COM_DIA_INDEFINIDO" in protestos:
        ec = vinculo_do_dia_carregado(cid, dia)
        geradora = cel.escala_geradora if cel is not None and cel.escala_geradora_id else None
        dados["vinculo"] = dados_vinculo(ec)
        dados["geradora"] = dados_vinculo(geradora)
        telas["previsto"] = []
        if cadastro["sem_vinculo_no_dia"]:
            dados["falta_previsto"] = "vinculo_que_responda_pelo_dia"
            telas["previsto"].append(tela(
                "colaboradores:painel_vinculo", "colaboradores/partials/painel_vinculo.html",
                "colaboradores:vincular_escala", args=(cid,),
            ))
        if cadastro["folga_nao_definida"]:
            dados["falta_previsto"] = ["folga_semanal_no_tipo", "FolgaDia_no_mes"]
            if ec.ativa:
                telas["previsto"].append(tela(
                    "escala:plano_folgas", "escala/plano_folgas.html",
                    "escala:plano_folgas_aplicar",
                    filtros={"empresa": EMPRESA_ID, "ano": dia.year, "mes": dia.month},
                ))
            if ec.tipo_escala.editavel:
                telas["previsto"].append(tela(
                    "escala:wizard_tipo_editar", "escala/wizard_tipo_escala.html",
                    "escala:wizard_salvar_editar", args=(ec.tipo_escala_id,),
                ))
        if not telas["previsto"]:
            telas["previsto"] = NV
        dados.setdefault("falta_previsto", NV)

    if "PARCIAL_SEM_PERIODO" in protestos:
        a = cobertura_ausencia_periodo(colab, dia, dia, para="folha").get(dia)
        dados["ausencia_folha"] = campos(
            a, "pk", "tipo", "data", "data_fim", "status", "minutos", "hora_ini", "hora_fim",
        )
        dados["falta_periodo"] = NV
        telas["parcial"] = NV
        if (
            a is not None and a.status == "aprovada" and cobre_parte_do_dia(a)
            and periodo_da_parcial_no_dia(a, ()) == "sem_periodo"
        ):
            dados["falta_periodo"] = [k for k in ("hora_ini", "hora_fim") if getattr(a, k) is None]
            telas["parcial"] = tela(
                "ponto:painel_ausencia", "ponto/partials/painel_ausencia.html",
                "ponto:editar_periodo_ausencia", args=(a.pk,),
            )
    return dados, telas


def perguntas_no_dia(cid, dia, cel_id, instancias):
    # Relação da célula, relação por fato usada pela tarefa e IDs que ela publicou.
    q = Q(colaborador_id=cid, conversa=False, data_evento=dia)
    if cel_id is not None:
        q |= Q(celula_id=cel_id)
    q |= Q(pk__in=[i["pergunta_id"] for i in instancias if i["pergunta_id"] is not None])
    return list(PerguntaDisputa.objects.filter(q).select_related(
        "disputa__colaborador__empresa", "disputa__chamado", "chamado_destino", "celula",
    ).prefetch_related("decisoes").order_by("pk"))


def ato_e_projecao(perguntas, instancias):
    passivo = {i["pergunta_id"] for i in instancias if i["razao"] == "passivo_sem_linha"}
    faltantes = defaultdict(list)
    for ln in DecisaoPergunta.linhas_da_projecao_sem_linha([p for p in perguntas if p.pk in passivo]):
        if ln["ato_em"] <= AGORA:
            faltantes[ln["pergunta_id"]].append({"ato": ln["ato"], "em": ln["ato_em"]})
    out = []
    for p in perguntas:
        lido = DecisaoPergunta.composta(p)
        foto = {
            "pergunta": p.pk,
            "sequencia_gravada": [campos(d, "pk", "ato", "ato_em") for d in p.decisoes.all()],
            "projecao": campos(p, "respondida_em", "validada_em", "materializada_em", "via_resolucao"),
            "leitura_composta": campos(lido, "respondida_em", "validada_em", "materializada_em", "via_resolucao"),
        }
        if p.pk in passivo:
            foto["atos_faltantes"] = faltantes.get(p.pk) or NV
            foto["escritor_passivo"] = ESCRITOR_PASSIVO
        out.append(foto)
    return out


def dados_ferias(colab, dia):
    de = timezone.make_aware(datetime.combine(dia, time.min))
    ate = timezone.make_aware(datetime.combine(dia, time.max))
    ags = ferias_na_janela(dia, dia, colaborador_ids=[colab.pk]).select_related(
        "periodo_aquisitivo"
    ).order_by("pk")
    return {
        "agendamentos": [
            {
                **campos(ag, "pk", "data_inicio", "data_fim", "status"),
                "aquisitivo": campos(ag.periodo_aquisitivo, "pk", "data_inicio", "data_fim", "status"),
            }
            for ag in ags
        ],
        "batidas_apuraveis_civis": [
            {"pk": b.pk, "hora_local": timezone.localtime(b.timestamp), "tipo_gravado": b.tipo}
            for b in batidas_apuraveis(colab, de, ate).order_by("timestamp", "pk")
        ],
        "tela_do_modulo": tela("ferias:secao_ferias_colab", "ferias/_secao_colab.html", args=(colab.pk,)),
    }


def portas_retrato(retrato, cid, dia):
    linha = linha_do_colab(retrato, cid)
    if linha is None:
        return {"portas": NV, "motivo": "sem_linha_no_retrato_visivel"}
    return {
        "lavrado_em": retrato.lavrado_em,
        "motivo_lavrado": linha.get("motivo"),
        "dia_nas_datas_lavradas": str(dia) in linha["datas"] if linha.get("datas") is not None else NV,
        "portas": [
            {"url": resolve(urlsplit(p["href"]).path).view_name, "rotulo": p["rotulo"]}
            for p in portas_da_linha(linha)
        ],
    }


def molede_linhas(empresa, inicio, fim):
    lista = ListaDoAdmin(HOJE, [empresa], agora=AGORA)
    casos = origem_s1(empresa, inicio, fim, lista)
    colabs = Colaborador.objects.select_related("empresa").in_bulk(
        {i["colaborador_id"] for i, _, _, _ in casos}
    )
    cxr_da_competencia = cxr_lavrado()
    retrato = RetratoFolha.objects.filter(
        empresa_id=empresa.pk, mes=MES, ano=ANO, empresa__in=empresas_visiveis(),
    ).first()

    @lru_cache(maxsize=None)
    def cadastro_atual(cid, dia):
        return cadastro_do_dia_carregado(cid, dia)

    @lru_cache(maxsize=None)
    def fonte_dono(cid):
        cxr = cxr_da_competencia(cid, inicio)
        ins = InsumosDoDono(
            colabs[cid], inicio, fim, HOJE, agora=AGORA, sem_motor=True, cxr=cxr,
            cadastro=lambda dia: cadastro_atual(cid, dia),
        )
        return ins, cxr

    @lru_cache(maxsize=None)
    def chamados_do_colab(cid):
        por_dia = defaultdict(list)
        for ch in ChamadoColaborador.objects.filter(colaborador_id=cid).select_related("celula").order_by("pk"):
            por_dia[dia_do_fato(ch, fortes=True)].append(ch)
        return por_dia, chamados_que_falam_de_batida(colabs[cid])

    linhas = []
    for item, dia, retencao, leitura_s1 in casos:
        cid = item["colaborador_id"]
        colab = colabs[cid]
        membro = lista.colab_na_lista(cid)
        row = dict.fromkeys((
            "celula", "chamados/ancoras/C6", "rota/acao/razao", "ato/projecao",
            "dia_na_lista/dono", "insumos_L099", "destino_DP",
        ), NV)
        row.update(
            colab=f"col{cid}", dia=dia if dia is not None else NV, motivo=item["motivo"],
            colab_na_lista=NV if membro is None else membro,
            dono=NV, dono_fonte=FONTE_DONO, dono_motivo="S1_sem_dia_publicado",
            cadastro=None, tela=None, ferias=None,
            porta_tipo_S1=PORTA_DO_TIPO.get(tipo_da_linha(item)),
            retrato=portas_retrato(retrato, cid, dia),
        )
        if dia is None:
            linhas.append(row)
            continue

        cel = celulas_do_periodo([cid], dia, dia).get((cid, dia))
        cel_id = cel.pk if cel is not None else None
        protestos = sorted(cel.protestos or ()) if cel is not None else []
        ata = (cel.ata or {}) if cel is not None else {}
        if leitura_s1 is None:
            tarefa = tarefa_do_dia(colab, dia, AGORA)
            porta = porta_da_tarefa(tarefa)
            fio = _fio_da_porta(porta)
            cal = resolver_dia_oferecido_nas_datas(colab, [dia], agora=AGORA)[dia]
            ligadas = PerguntaDisputa.objects.filter(celula_id=cel_id) if cel_id is not None else ()
            fora, papel = dono_fora_do_fio(colab, dia, ligadas, lista, com_papel=True)
        else:
            tarefa, porta, fio, cal, fora, papel = leitura_s1

        instancias = tarefa["instancias"]
        perguntas = perguntas_no_dia(cid, dia, cel_id, instancias)
        por_dia, cartorio = chamados_do_colab(cid)
        cadastro = cadastro_atual(cid, dia)
        ins, cxr = fonte_dono(cid)
        iso = dia.isoformat()
        dono = ins.dono(dia)
        na_lista = lista.dia_na_lista(cid, iso)
        destino = lista.destino_fora_da_mesa(cid, iso)
        dados, telas = cadastro_e_tela(colab, dia, cel, protestos, cadastro)
        row.update({
            "celula": None if cel is None else {
                **campos(cel, "pk", "trabalha", "origem", "escala_geradora_id", "ausencia_id",
                         "julgada_em", "veredito", "veredito_via"),
                "protestos": protestos,
                "ata": {k: ata.get(k) for k in (
                    "tipo_dia", "minutos_previstos", "minutos_realizados",
                    "sem_ancora", "cobertura_parcial", "periodo_parcial",
                )},
            },
            "motivo_dia": retencao["motivo"] if retencao is not None else item["motivo"],
            "chamados/ancoras/C6": {
                "dia_forte": [campos(ch, "pk", "status_local", "modulo_origem") for ch in por_dia.get(dia, ())],
                "cartorio": [campos(ch, "pk", "status_local", "modulo_origem") for ch in cartorio.get(iso, ())],
                "fila_publicada": (porta or {}).get("fila", ()),
                "perguntas": [{
                    **campos(p, "pk", "disputa_id", "celula_id", "chamado_destino_id"),
                    "pai": p.disputa.chamado_id,
                    "ancora": chamado_que_lista(p),
                    "C6": pergunta_viva(p, hoje=HOJE, agora=AGORA),
                } for p in perguntas],
            },
            "rota/acao/razao": {
                "porta": porta, "fio": fio, "razao": tarefa["razao"],
                "instancias": [{k: i[k] for k in ("instancia", "pergunta_id", "razao", "acoes")} for i in instancias],
                "calendario": NV if cal is None else cal, "fora_fio": fora, "papel": papel,
            },
            "ato/projecao": ato_e_projecao(perguntas, instancias),
            "dia_na_lista/dono": NV if na_lista is None else {
                "dia_na_lista": na_lista[0], "dono_lavrado": na_lista[1],
                "mesa_admin": lista.dia_do_admin(cid, iso),
            },
            "insumos_L099": {
                **cadastro,
                "horas_oraculo": ins.horas.get(iso, []),
                "batida_fora_de_turno": batida_fora_de_turno(colab, dia, ins.turnos()),
                "fase_deslocada": iso in ins.fase(),
                "em_cadastro_x_realidade": NV if cxr is None else iso in cxr,
            },
            "dono": NV if dono is None else dono,
            "dono_motivo": "lavra_CxR_sem_cobertura_e_insumo_necessario" if dono is None else None,
            "destino_DP": NV if na_lista is None else destino,
            "cadastro": dados,
            "tela": telas,
            "ferias": dados_ferias(colab, dia) if item["motivo"] == "ferias_com_batida" else None,
        })
        linhas.append(row)
    return linhas


def colher():
    with schema_context("juliani"):
        connection.set_autocommit(False)
        try:
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
                cursor.execute(
                    "SELECT current_schema(), current_setting('transaction_read_only'), "
                    "current_setting('transaction_isolation')"
                )
                if cursor.fetchone() != ("juliani", "on", "repeatable read"):
                    raise SystemExit("PAREI: schema ou modo de transacao incorreto")
            if not User.objects.filter(username="sistema").exists():
                raise SystemExit("PAREI: usuario de sistema ausente na Sombra")
            empresa = Empresa.objects.get(pk=EMPRESA_ID)
            inicio, fim = janela_fechamento(MES, ANO, empresa)
            linhas = molede_linhas(empresa, inicio, fim)
            with connection.cursor() as cursor:
                cursor.execute("SELECT 1")
            return linhas
        finally:
            connection.rollback()
            connection.set_autocommit(True)


class SemErroComDadosLivres:
    def __enter__(self):
        return self

    def __exit__(self, tipo, erro, quadro):
        if tipo is None or issubclass(tipo, (SystemExit, KeyboardInterrupt)):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        arquivo = quadro.tb_frame.f_code.co_filename.rsplit("/", 1)[-1]
        raise SystemExit(f"PAREI: {tipo.__name__} em {arquivo}:{quadro.t

[truncado em 20000 de 32230 caracteres]

## 27/11 20:54 · sessao f4be0ffb7b4d · - · - · origem=Ronald (tmux)

- A autoridade do dono é `InsumosDoDono(..., sem_motor=True, cxr=cxr_lavrado()(cid, ini)).dono(dia)`, que chama `dono_da_divergencia`. O mesmo caminho já existe no relatório nominal de férias. `None` significa que a lavra CxR não cobre a competência do colaborador e que o juiz precisaria desse insumo para resolver o dono. Registre horas do oráculo, fase, turno aberto com batida e as três chaves de cadastro pelos mesmos leitores. O juiz pode devolver `BATIDA` ou `ESTRUTURA` para um protesto cuja origem o supra-juiz descreve como cadastro, pois `PARCIAL_SEM_PERIODO` não é um argumento do E6. `app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:198`, `app/ferias/management/commands/listar_ferias_com_batida.py:53`, `app/ponto/services/cxr_na_lista.py:165`, `app/ponto/supra_juiz.py:180`, `app/ponto/supra_juiz.py:395`.
- IDs exatos da pergunta do `passivo_sem_linha` vêm das instâncias da tarefa. `DecisaoPergunta.linhas_da_projecao_sem_linha` devolve o tipo e o instante do ato faltante. O leitor da tarefa corta em `ato_em <= AGORA`, e `composta` lê a projeção combinada. `DecisaoPergunta.registrar` é o escritor único da sequência. Resposta humana e validação são publicadas pelas respectivas portas junto à projeção, e o backfill tem a correção específica do passivo. A porta que omitiu a linha de cada caso segue [nao verificado]. `app/chamados/services/tarefa_do_dia.py:338`, `app/chamados/services/tarefa_do_dia.py:433`, `app/chamados/models.py:4416`, `app/chamados/models.py:4507`, `app/chamados/models.py:4590`, `app/chamados/services/disputa_emissao.py:1565`, `app/chamados/services/validacao.py:176`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164`.
- A S1 usa só `PORTA_DO_TIPO['ferias']`, que vale `None`. A publicação do drawer consulta também o fio. O contrato vigente da linha de férias produz fio `admin` e permite o link genérico `chamados:modal_fio`. Leia a publicação efetivamente lavrada por `linha_do_colab` + `portas_da_linha`, chamadas pela view do painel. A existência de pergunta ou ação nesse link para o caso nominal segue [nao verificado]. `app/folha/retrato_folha.py:351`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1066`, `app/colaboradores/views.py:996`, `app/folha/management/commands/lavrar_retrato_folha.py:77`.
- O cruzamento da Folha usa `ferias_na_janela` para o agendamento, e `batidas_apuraveis` pela data civil. O relatório nominal de férias usa um universo distinto, de `Ausencia` espelho. A seção administrativa de férias e a página de saldo chamam um escritor de status durante o GET. `app/folha/export.py:464`, `app/ferias/services.py:49`, `app/ponto/turnos.py:2941`, `app/ferias/management/commands/listar_ferias_com_batida.py:4`, `app/ferias/views.py:121`, `app/ferias/views.py:838`.
- Para `col296`, o leitor canônico de chamados que compõem o vocabulário do cartório é `chamados_que_falam_de_batida`. A versão de chamados vivos que protestou no julgamento é um subconjunto filtrado pela ata daquele ato. A data forte do chamado, as âncoras por pergunta (destino ou pai), o C6 e o histórico da disputa são leituras distintas. A causa da permanência da cobrança segue [nao verificado]. `app/ponto/services/cartorio.py:131`, `app/ponto/services/cartorio.py:1498`, `app/ponto/services/cartorio.py:1618`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:911`, `app/chamados/services/fio.py:97`.
- A lista administrativa lê a pertença do colaborador, a presença do dia, o dono lavrado e o destino DP por portas distintas. O encaminhamento `pauta-dp` é publicado para o C13 entregue, com fonte no serviço de pares-relâmpago. `app/chamados/services/vigia_fio.py:210`, `app/chamados/services/vigia_fio.py:228`, `app/chamados/services/vigia_fio.py:238`, `app/escala/services/cadastro_realidade.py:1274`, `app/ponto/services/par_relampago_na_lista.py:375`.

_8804 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
