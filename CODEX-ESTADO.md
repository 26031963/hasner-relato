# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:05** · publicado em **05/11/2026 15:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:05 · sessao 343e7f874e87 · REVISAO-BUG-SW · verificador · origem=Code

1. **Quem escreve e quem lê.** O diff altera o retorno do fallback; não altera campos cadastrais (`app/static/sw.js:29`, `app/static/sw.js:30`).
   
   | Campo/valor | Escritor/origem | Leitor |
   |---|---|---|
   | `request` do helper | Chamadas dos três ramos GET: `app/static/sw.js:48`, `app/static/sw.js:77`, `app/static/sw.js:84` | `caches.match(request)`: `app/static/sw.js:30` |
   | Resposta guardada | Instalação com `addAll`: `app/static/sw.js:12`; gravações com `cache.put`: `app/static/sw.js:59`, `app/static/sw.js:74` | Novo fallback: `app/static/sw.js:30`; ramo estático existente: `app/static/sw.js:56` |
   | Retorno sem cópia | Novo `Response.error()`: `app/static/sw.js:30` | `respondWith` dos três ramos: `app/static/sw.js:47`, `app/static/sw.js:70`, `app/static/sw.js:83` |
   | Versão do cache, afetada pelo conteúdo do SW | Hash dos arquivos governados: `app/core/views.py:524`, `app/core/views.py:548`, `app/core/views.py:555`; substituição de `__BUILD_ID__`: `app/core/views.py:572` | Nome, abertura e descarte de caches: `app/static/sw.js:1`, `app/static/sw.js:12`, `app/static/sw.js:20` |
   | Valores simulados e resultado dos testes | `cache` produz `guardado`; `rede` seleciona o fetch; harness produz `chamou`, `response`, `tipo`, `corpo`: `app/core/tests/test_sw_offline_fallback.py:33`, `app/core/tests/test_sw_offline_fallback.py:39`, `app/core/tests/test_sw_offline_fallback.py:45`, `app/core/tests/test_sw_offline_fallback.py:49` | Asserções: `app/core/tests/test_sw_offline_fallback.py:83`, `app/core/tests/test_sw_offline_fallback.py:87`, `app/core/tests/test_sw_offline_fallback.py:94`, `app/core/tests/test_sw_offline_fallback.py:101` |

   Fora do diff, `/sw.js` é servido pela view e registrado pelo cliente; a view pode escolher `staticfiles`, enquanto o teste novo aponta diretamente para a fonte (`app/core/views.py:529`, `app/core/views.py:570`, `app/static/pwa.js:14`, `app/core/tests/test_sw_offline_fallback.py:23`). Correspondência com o arquivo efetivamente servido em produção: **[nao verificado]**.

2. **Contraexemplo com cadastro e data.** O material fornecido não contém cadastro raspado com valores e datas nem cláusulas no anexo `BUG-SW.leis`; portanto, um contraexemplo de produção contra essas regras fica **[nao verificado]**.

   O cenário técnico está definido: GET sem rede e sem cópia passa a retornar `Response.error()` nos três ramos; esse é precisamente o resultado exigido pelo teste RED (`app/static/sw.js:30`, `app/core/tests/test_sw_offline_fallback.py:80`, `app/core/tests/test_sw_offline_fallback.py:87`).

3. **Regra de negócio alterada sem lastro.** Não identifiquei alteração de regra cadastral ou financeira nesses caminhos: o helper consulta cache e escolhe a resposta de fallback, sem interpretar seu conteúdo (`app/static/sw.js:29`, `app/static/sw.js:30`). Lastro normativo específico para essa escolha nos anexos LEIS/CORTES: **[nao verificado]**. `JUIZES.md` declara ser tabela de consulta, com a lei e o registro vivo em outros arquivos (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:10`).

4. **Selo que não morde a reversão.** Pela leitura do código, estes dois testes passariam com a cura revertida: [nao verificado]
   
   - `test_MORDE_sem_rede_COM_cache_a_copia_continua_vencendo`: exige apenas `DO-CACHE`, que o antigo `caches.match` também entregava (`app/core/tests/test_sw_offline_fallback.py:89`, `app/core/tests/test_sw_offline_fallback.py:94`).
   - `test_MORDE_com_rede_a_rede_vence_o_cache`: exige `DA-REDE`; com sucesso da rede, o fallback alterado não é executado (`app/core/tests/test_sw_offline_fallback.py:96`, `app/core/tests/test_sw_offline_fallback.py:101`, `app/static/sw.js:48`, `app/static/sw.js:77`, `app/static/sw.js:84`).

   São controles de preservação; o teste RED distingue a cura ao exigir uma `Response` de tipo `error` sem rede e sem cache (`app/core/tests/test_sw_offline_fallback.py:79`, `app/core/tests/test_sw_offline_fallback.py:84`, `app/core/tests/test_sw_offline_fallback.py:87`). Confirmação dinâmica: **[nao verificado]**; a execução parou na exigência de Node disponível (`app/core/tests/test_sw_offline_fallback.py:70`, `app/core/tests/test_sw_offline_fallback.py:71`).

5. **Cadastro de hoje decidindo passado.** Os caminhos revisados não consultam `situacao`, `data_demissao`, `posto`, `empresa`, `regime`, `isento_ponto` ou vínculo atual, nem decidem dia ou competência: encaminham respostas de rede/cache (`app/static/sw.js:29`, `app/static/sw.js:33`, `app/static/sw.js:67`, `app/static/sw.js:82`). Caso concreto de retroatividade cadastral em produção: **[nao verificado]**.

**VEREDITO: ACHADO**

1. `app/core/tests/test_sw_offline_fallback.py:89` e `app/core/tests/test_sw_offline_fallback.py:96` — **item 4; toca dinheiro: nao**. Individualmente sobrevivem à reversão; o teste RED cobre a falha específica (`app/core/tests/test_sw_offline_fallback.py:84`, `app/core/tests/test_sw_offline_fallback.py:87`).

## 05/11 15:05 · sessao 27e72e3a65d7 · - · - · origem=Ronald (tmux)

1. **Selo de vazamento não exercita lote nem request.** `test_lote_nao_vaza_e_request_segue_guardando_so_o_SIM` nunca entra em `_lote()` e nunca instala `REQUISICAO_DA_TRILHA`; verifica `LOTE_DA_TRANCA` antes de qualquer abertura nesse teste. A expressão com `getattr(..., None)` também aceita a ausência completa da implementação. Portanto, esse teste passa com a cura revertida; execução efetiva [não verificado]. Evidência: `app/chamados/tests/test_snc2_toque_em_lote.py:116`, `app/chamados/tests/test_snc2_toque_em_lote.py:122`.

2. **Equivalência estática também aceita reversão.** `_lote()` cai em `contextlib.nullcontext` se `tranca_em_lote` não existir; assim o teste de equivalência compara duas execuções avulsas quando a cura é revertida. Todos os toques da fixture são gravados antes da avaliação em lote. Evidência: `app/chamados/tests/test_snc2_toque_em_lote.py:22`, `app/chamados/tests/test_snc2_toque_em_lote.py:74`, `app/chamados/tests/test_snc2_toque_em_lote.py:92`.

3. **Não declarar a suíte inteira sem mordida.** O teste de custo exige queries presentes e custo igual com uma e cinco perguntas, carregando instâncias frescas. Ele distingue uma otimização efetiva da mera manutenção do veredito; resultado executado [não verificado]. Evidência: `app/chamados/tests/test_snc2_toque_em_lote.py:66`, `app/chamados/tests/test_snc2_toque_em_lote.py:79`, `app/chamados/tests/test_snc2_toque_em_lote.py:86`.

4. **Cache negativo condicionado a request escaparia deste arquivo.** Uma mutação que guardasse o NÃO somente quando há `REQUISICAO_DA_TRILHA` não seria exercitada: o teste não configura esse ContextVar. Uma mutação indiscriminada que guarde NÃO também na instância, ao contrário, deve ser detectada pela segunda chamada direta a `dia_da_pergunta_trancado`; resultado executado [não verificado]. Evidência: `app/chamados/tests/test_snc2_toque_em_lote.py:116`, `app/chamados/juizes.py:971`, `app/chamados/juizes.py:1046`.

5. **O selo pós-toque evita o C6.** A primeira leitura usa `pergunta_viva(p)`; a posterior usa diretamente `dia_da_pergunta_trancado(p)`. A chave `_pv_memo` não contém trilha, tranca ou identidade do lote, e `pergunta_viva` retorna o memo antes de recalcular. Logo o teste não prova atualização do veredito C6 na mesma instância após o toque ou após sair de um lote. A insuficiência desse memo já existe fora do diff; não atribuir sua origem a esta obra. Evidência: `app/chamados/tests/test_snc2_toque_em_lote.py:121`, `app/chamados/tests/test_snc2_toque_em_lote.py:124`, `app/chamados/juizes.py:812`, `app/chamados/juizes.py:836`.

6. **Autoridade e contratos aplicáveis.** C6 continua registrado como `chamados/juizes.py::pergunta_viva`; `JUIZES.md` declara ser lookup e remete ao registro vivo. Os contratos exigem leitor sem derivação própria, escritor canônico, aval literal e revisão das classes enumeradas pelo usuário. Pré-requisitos de pouso incluem suíte inteira verde e opus final; o cumprimento nesta obra é [não verificado]. Evidência: `app/core/juizes.py:112`, `app/core/juizes.py:266`, `app/docs/JUIZES.md:7`, `CLAUDE.md:20`, `CLAUDE.md:32`, `CLAUDE.md:37`, `CLAUDE.md:364`.

## 05/11 15:05 · sessao 3e35454efb88 · - · - · origem=Ronald (tmux)

Censo do mapa e dos valores lidos:

- `_celulas_do_ato`: escritor temporário `celulas_do_ato`, com restauração no `finally` (`app/escala/alimentacao.py:43`, `:44`, `:47`, `:49`, `:51`). Leitor central `_celulas_do_ato_para`, que limita o uso à janela e a datas sem hora (`app/escala/models.py:1063`, `:1064`, `:1067`). Os quatro consumidores estão em `app/escala/models.py:1088`, `:1118`, `:1183`, `:1228`. A instalação no fluxo ocorre em `app/ponto/services/fechamento.py:269`.
- `trabalha`: lido por `eh_dia_trabalho`; ausência de célula mantém a aritmética anterior (`app/escala/models.py:1090`, `:1095`, `:1096`).
- `dna.marcos`: lido por `marcos_do_dia`; ausência de marcos mantém o template anterior (`app/escala/models.py:1124`, `:1131`, `:1133`).
- Pausas em `dna.marcos`: lidas por `pausas_do_dia`, incluindo os pares declarados em `CelulaDia.PARES_PAUSA_DNA`; célula existente sem marcos responde nenhuma pausa (`app/escala/models.py:1189`, `:1197`, `:1202`, `:1207`). `intervalo_do_dia` lê a primeira pausa dessa função (`app/escala/models.py:1163`).
- `dna.extra_declarada`: lido por `extra_declarada_do_dia`; célula existente sem declaração responde `(0,0)` (`app/escala/models.py:1234`, `:1235`, `:1238`).
- Produtor do DNA: `montar_dna` lê marcos e pausas do template e congela extras declaradas (`app/ponto/management/commands/gerar_celulas.py:73`, `:81`, `:82`, `:119`, `:122`).
- Escritores persistentes de `trabalha`/`dna`: criação e regeneração (`app/ponto/portas/celula.py:247`, `:260`, `:264`); retirada de previsão fora da vigência (`:231`, `:232`, `:233`); reversão (`:530`, `:532`, `:538`); geração e enriquecimento (`:593`, `:604`, `:605`); atualização presente/futura (`:612`, `:614`, `:615`). O diff não acrescenta escrita persistente desses campos: acrescenta o atributo temporário (`app/escala/alimentacao.py:44`).

Os três chamadores de produção entregam mapas completos:

- Fechamento: lote do envelope entre cortes 2 e 28, seguido da chamada com a janela própria (`app/ponto/services/fechamento.py:575`, `:591`, `:592`, `:736`).
- Espelho: `celulas_do_periodo([pk], ini, fim)` e chamada com os mesmos limites (`app/ponto/services/espelho.py:1049`, `:1164`).
- PDF: carga `data_ini..data_fim_mes` e chamada com os mesmos limites; motor recebe o mesmo mapa (`app/relatorios/pdf_espelho.py:350`, `:401`, `:415`, `:416`).

Não encontrei contraexemplo introduzido pelo mapa sob esses contratos: o caminho alterado troca a consulta pela célula já carregada e conserva as mesmas derivações (`app/escala/models.py:1087`, `:1094`, `:1117`, `:1124`, `:1182`, `:1189`, `:1227`, `:1234`). Ocorrência real em produção de mapa incompleto, concorrência ou célula alterada durante a apuração: [não verificado].

Selo semanticamente independente da cura: `test_a_apuracao_do_motor_e_a_da_REGRA_pela_celula` apenas exige os números do DNA (`app/ponto/tests/test_he_pendente_identity_map.py:149`, `:151`); a leitura anterior por `.first()` já consultava esse DNA (`app/escala/models.py:1122`, `:1124`). Sua passagem com a cura revertida é dedução estática; execução: [não verificado]. O teste de queries cobra diretamente a finalidade do diff (`app/ponto/tests/test_he_pendente_identity_map.py:116`, `:124`).

## 05/11 15:05 · sessao dbe579d6c72a · - · - · origem=Ronald (tmux)

Item 5: há leitura de cadastro atual decidindo passado, anterior ao diff.

- **Situação e demissão:** `apurar` seleciona somente `empresa=empresa, situacao='ativo'`, sem considerar `data_demissao`; só depois chama o espelho da competência (`app/ponto/services/he_pendente_lavrado.py:89`, `:92`, `:94`). A autoridade do fechamento inclui desligados cuja demissão alcança o início da janela (`app/ponto/services/fechamento.py:423`, `:430`), e declara explicitamente que a situação deve ser escopada à competência, não ao estado de hoje (`app/ponto/services/fechamento.py:388`).
  
  Contraexemplo sintético: cadastro do próprio teste HEIM, corte 21, DNA 08–17, batidas 07:50/12:00/13:00/17:00 em 01/10/2026; o caso espera 10 minutos de pendência nesse dia (`app/ponto/tests/test_he_pendente_identity_map.py:69`, `:84`, `:95`, `:109`). Depois de desligar em 25/10/2026 e consultar a competência 10 em 05/11/2026, o filtro de ativos elimina o colaborador e suas pendências, embora a demissão seja posterior à janela e o universo da competência continue incluindo-o (`app/ponto/services/he_pendente_lavrado.py:92`; `app/ponto/services/fechamento.py:423`). É caso sintético: ocorrência em produção [não verificado].

- **Posto:** o município do posto atual define os feriados de todo o período (`app/core/feriados.py:113`; `app/ponto/services/espelho.py:146`, `:1030`), compartilhados entre todas as fatias do motor (`app/ponto/services/espelho.py:1111`). O fechamento usa a mesma leitura do posto atual (`app/ponto/services/fechamento.py:628`). Uma transferência posterior pode alterar quais feriados entram na apuração anterior; vítima e valor em produção [não verificado].

- **Empresa e regime:** `regua_para` consulta a empresa atual do colaborador e seu regime atual, mesmo recebendo uma competência passada (`app/core/regua_cct.py:352`). A aplicação de convenção considera essa empresa e aplicações atualmente ativas, filtrando a vigência da CCT pela data solicitada (`app/core/regua_cct.py:253`, `:261`). A janela da competência também usa o corte da empresa atual (`app/ponto/services/fechamento.py:607`). Mudança real de valor em produção [não verificado].

- **Isenção:** o juiz lê somente o `Colaborador.isento_ponto` atual, sem argumento de data (`app/core/constants.py:13`). Isso elimina furos de datas passadas no fechamento (`app/ponto/services/fechamento.py:355`) e inconsistências históricas no espelho (`app/ponto/services/espelho.py:606`, `:676`). Mudança real de desconto em produção [não verificado].

- **Vínculo:** o caminho principal alcançado pelo diff consulta células e vínculos da janela e resolve o vínculo pelo juiz do dia; não usa simplesmente `ativa=True` de hoje (`app/ponto/services/espelho.py:1049`, `:1053`, `:1065`; `app/ponto/services/fechamento.py:189`). `piso_visual` ainda consulta o vínculo ativo atual (`app/ponto/services/espelho.py:42`), mas isoladamente isso não demonstra corte do período explicitamente pedido [não verificado].

Escritores correspondentes:

- Situação/demissão: desligamento atribui os campos e salva, fechando os vínculos na demissão (`app/colaboradores/services/desligamento.py:104`, `:108`, `:123`).
- Posto/empresa: transferência atribui ambos e salva; registra histórico com a data atual (`app/colaboradores/views.py:1385`, `:1388`, `:1392`). O editor também atribui empresa (`app/colaboradores/views.py:1313`).
- Regime: formulário lê e valida o valor e chama `atualizar_empresa` (`app/colaboradores/views.py:512`, `:521`).
- Isenção: passe livre chama `gravar` com o novo valor (`app/colaboradores/views.py:175`).
- Porta comum: campos permitidos em `OPERACOES`, atribuição e gravação em `gravar`, e gravações alternativas em `salvar_campos`/`salvar_inteiro` (`app/colaboradores/services/colaborador.py:23`, `:47`, `:57`, `:65`).

O retrato lavrado não é a medição usada pelo portão de exportação; portanto não atribuir a sua omissão um bypass direto do TXT (`app/ponto/services/he_pendente_lavrado.py:16`).

## 05/11 15:05 · sessao 317a82cced6f · - · - · origem=Ronald (tmux)

Rastro dos escritores/leitores, somente por inspeção:

- **Trilha:** `LogAuditoria.usuario/acao/modelo/objeto_id/descricao/criado_em` estão em `app/core/models.py:295-307`; `usuario` usa `SET_NULL`, `criado_em` usa `auto_now_add`. O escritor central é `app/core/observ.py:134-137`, chamado pelo adaptador `app/core/auditoria.py:22-24`. O leitor alterado filtra timestamp/usuário em `app/chamados/juizes.py:1077-1078` e lê os três atos em `:1082-1110`.
- **Cobrar:** grava ação/modelo/colaborador/data em `app/chamados/services/cobrar_dia.py:37-38`, antes do cartório `:39` e de `pergunta_no_app.do_dia` `:43`.
- **Resolver:** grava ação `veredito_celula`, modelo `CelulaDia`, objeto `cel.pk` ou `0`, autor e descrição em `app/ponto/services/veredito_celula.py:334-336`, depois de fechar o fio `:325` e lavrar o veredito `:330`.
- **Desarquivar:** primeiro chama `reabrir` em `app/chamados/views_arquivar.py:70`; depois `arquivo.levantar` em `:77`; finalmente repete `ressuscitar_perguntas` em `:86`, explicitamente para reler o toque recém-gravado (`:81-85`). `levantar` salva em `app/chamados/services/arquivo.py:208`, chama `_trilha` em `:209`, que chama o escritor `evento` em `:137`.
- **Sistema:** `get_user_sistema` lê **e pode criar** usuário, pois usa `get_or_create(username='sistema')` em `app/core/utils.py:18`. Outros escritores identificados: migração `app/core/migrations/0003_user_sistema.py:9-19`; comando `app/core/management/commands/garantir_user_sistema.py:13-26`, normalização em `:33-41`. O novo cache do PK escreve/lê `memo[('sistema',)]` em `app/chamados/juizes.py:1074-1078`, inclusive fora do lote, porque o mesmo carregador atende avulso em `:1049`. Ausência efetiva do usuário e criação real em produção: **[não verificado]**.
- **Request:** escritor do ContextVar `REQUISICAO_DA_TRILHA` é o middleware em `app/core/middleware.py:78`, reset em `:82`; leitores são `app/core/observ.py:108-118` (ator/IP) e `app/chamados/juizes.py:971-975` (memo request). O contexto do lote precede a request em `app/chamados/juizes.py:968-970`.
- **Lote:** único chamador de produção localizado é `app/chamados/management/commands/vigiar_fio_morto.py:171`; criação/reentrada/reset em `app/chamados/juizes.py:989-998`. A implementação não verifica request nem proíbe escrita dentro do contexto (`:989-998`); chamada real por porta request/escritora na cópia: **[não verificado]**.
- **Cadastro corrente das referências:** `CelulaDia.colaborador/data` em `app/escala/models.py:1681-1685`; nascimento por `app/ponto/portas/celula.py:247` e `:601-604`. `ChamadoColaborador.colaborador/celula` em `app/chamados/models.py:32,44`; `abrir` resolve FK em `:914-919`, grava em `:932`. FK tardia é escrita pela porta `app/chamados/services/vinculos.py:92-99`, chamada por `app/chamados/rejuncao.py:80`, `app/chamados/services/celula_carimbada.py:109` e `app/chamados/management/commands/carimbar_por_batida_coincidente.py:100`. O plural resolve os logs contra essas relações correntes em `app/chamados/juizes.py:1104-1110`.
- **Cadastro atual da competência:** o juiz usa `colab.empresa_id`, carrega `colab.empresa` e pede a janela para `data_evento` em `app/chamados/juizes.py:933-945`; `janela_atual` lê o corte corrente por `app/ponto/janelas.py:60-63`, cujo campo atual é lido em `:49-51`. Essa leitura já está no caminho comum avulso/lote, não nasceu no novo ramo plural (`app/chamados/juizes.py:938-945`).

## 05/11 15:04 · sessao db38cd2a170e · - · - · origem=Ronald (tmux)

- **Item 4:** `test_o_retrato_e_o_da_REGRA_pela_celula` apenas exige os três dias de 10 minutos; não exige uso do mapa (`app/ponto/tests/test_he_pendente_identity_map.py:109`). A própria documentação do teste declara que o retrato vem de `montar_dias`, fora de `apurar_por_fatia` (`app/ponto/tests/test_he_pendente_identity_map.py:21`); o montador recebe as células explicitamente para consultar extra declarada (`app/ponto/services/espelho.py:733`). Logo, a reversão da otimização preserva esse resultado por leitura do fluxo; execução da reversão **[não verificado]**.

- **Outro teste que preserva o resultado com reversão:** `test_a_apuracao_do_motor_e_a_da_REGRA_pela_celula` exige `480` trabalhados e `10` fora, mas não exige mapa (`app/ponto/tests/test_he_pendente_identity_map.py:149`). Sem mapa, `marcos_do_dia` consulta a célula no banco e lê o mesmo DNA congelado (`app/escala/models.py:1122`, `app/escala/models.py:1124`, `app/escala/models.py:1131`). Resultado do teste executado contra reversão **[não verificado]**.

- **Não acusar a suíte inteira de selo inerte:** o teste RED exige zero consultas por dia dentro de `apurar_por_fatia` (`app/ponto/tests/test_he_pendente_identity_map.py:116`, `app/ponto/tests/test_he_pendente_identity_map.py:124`); essa exigência distingue a otimização da consulta anterior (`app/escala/models.py:1122`). O teste de vazamento também usa diretamente a API adicionada (`app/ponto/tests/test_he_pendente_identity_map.py:130`, `app/ponto/tests/test_he_pendente_identity_map.py:133`).

- **O MORDE não representa reversão:** injeta deliberadamente `{}` como mapa completo (`app/ponto/tests/test_he_pendente_identity_map.py:161`), fazendo a autoridade entender “sem célula” e cair no template (`app/escala/models.py:1120`, `app/escala/models.py:1133`). A reversão consultaria a célula existente no banco (`app/escala/models.py:1122`). O teste prova dependência do mapa e sensibilidade a uma alimentação errada; não prova divergência financeira entre o código anterior e o patch (`app/ponto/tests/test_he_pendente_identity_map.py:165`, `app/ponto/tests/test_he_pendente_identity_map.py:169`).

- **Lastro da preservação semântica:** DNA irretroativo e template apenas como gerador estão expressos em `CLAUDE.md:146` e `CLAUDE.md:151`; a autoridade de marcos continua `EscalaColaborador.marcos_do_dia` (`app/docs/JUIZES.md:22`). O novo contexto grava estado temporário e o restaura no `finally` (`app/escala/alimentacao.py:44`, `app/escala/alimentacao.py:46`, `app/escala/alimentacao.py:49`, `app/escala/alimentacao.py:51`). Não identifiquei regra de negócio nova somente nesses testes/contexto; ausência global de regra alterada **[não verificado]**.

## 04/11 15:44 · sessao a3f628326e9b · E4-APP-MAPA · verificador · origem=Code

**Parecer: há um defeito demonstrável no payload:** dia sem batidas lavrado `irregular` retorna `status='alerta'`, mas `inconsistente=False`; com batidas, o mesmo alerta produz `inconsistente=True`. Severidade **média**; efeito na tela compilada **[nao verificado]**. (`app/api/views.py:1518`, `app/api/views.py:1599`, `app/api/views.py:1615`)

**1. Quem escreve e quem lê**

| Chave | Escritor no payload |
|---|---|
| `status` | Tradução do D4 pelo adaptador, nos dois ramos. (`app/api/views.py:1515`, `app/api/views.py:1516`, `app/api/views.py:1599`, `app/api/views.py:1600`) |
| `status_dia` | Palavra original do D4, nos dois ramos. (`app/api/views.py:1562`, `app/api/views.py:1618`) |
| `falta` | Somente no card sem batidas: `_st == 'falta'`. O dict com batidas não inclui essa chave. (`app/api/views.py:1543`, `app/api/views.py:1620`) |
| `inconsistente` | Com batidas: turno aberto ou alerta; sem batidas: `False` literal. **Achado médio acima.** (`app/api/views.py:1518`, `app/api/views.py:1559`, `app/api/views.py:1615`) |
| `turno_aberto` | Com batidas: geometria de `turnos_do_colab`; sem batidas: `False`. (`app/api/views.py:1449`, `app/api/views.py:1514`, `app/api/views.py:1560`, `app/api/views.py:1616`) |
| `ausencia_pendente` | Cobertura para cobrança, comparada com `ABERTAS`, aplicada aos dois ramos. (`app/api/views.py:1575`, `app/api/views.py:1582`, `app/api/views.py:1584`, `app/api/views.py:1592`, `app/api/views.py:1625`) |
| `previsto_inicio`, `previsto_fim`, dois intervalos | Somente sem batidas: `eh_dia_trabalho` determina se haverá horários; o template fornece os quatro marcos. (`app/api/views.py:1595`, `app/api/views.py:1597`, `app/api/views.py:1621`) |

As leituras internas relevantes são `status → inconsistente` e `_st → falta`; a resposta entrega a lista construída localmente. (`app/api/views.py:1518`, `app/api/views.py:1620`, `app/api/views.py:1628`, `app/api/views.py:1675`)

Não encontrei consumidor de produção desse payload em `static/`, `templates/` ou outro endpoint por busca textual; leitores indiretos **[nao verificado]**. A rota é `api/urls.py:81`; a v1 apenas responde 410 com endereço da v2, sem reutilizar seu dict. (`app/api/urls.py:81`, `app/api/views.py:839`)

O HTML do espelho consome outro dict, proveniente de `_esp['dias']`; suas leituras de `status`, `inconsistente`, `turno_aberto` e previsto não comprovam compatibilidade com a API. (`app/ponto/views.py:157`, `app/ponto/views.py:193`, `app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:166`, `app/templates/ponto/espelho.html:193`, `app/templates/ponto/espelho.html:207`)

Os leitores de teste dessas chaves incluem mapa/status/falta, geometria, pendência/previsto e isenção. (`app/api/tests/test_e4_app_mapa.py:83`, `app/api/tests/test_e4_app_mapa.py:90`, `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `app/api/tests/test_espelho_app_ausencia_pendente_aditivo.py:16`, `app/escala/tests/test_art62_nunca_acusa.py:221`)

Não encontrei fonte nativo pelas extensões usuais; comportamento do compilado **[nao verificado]**. A casca aponta para um aplicativo da Play Store. (`app/templates/core/gate_apk.html:6`)

Há mudanças semânticas deliberadas: falta automática sem decisão passa a neutro; entrada antiga sem saída pode conservar `turno_aberto=True` com status neutro; `aberto` também pode significar dia em andamento com saídas registradas. Dependência do compilado nesses sentidos antigos **[nao verificado]**. (`app/api/tests/test_espelho_app_ausencia_pela_autoridade.py:61`, `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `app/ponto/status_dia.py:283`)

**2. Vocabulário e possíveis KeyErrors**

O conjunto produzido pelo adaptador é exatamente `{ok, falta, previsto, folga, neutro, alerta, aberto}`; suas entradas cobrem as onze palavras do núcleo. (`app/ponto/status_dia.py:79`, `app/ponto/status_dia.py:86`)

Não há omissão de chave por vínculo, desligamento, futuro ou ausência de escala: a view solicita o mês inteiro e o D4 escreve uma resposta para cada par. Sem escala, fatos, batidas ou lavra, chega a `sem_dados`. Isso prova completude das chaves quando a execução termina normalmente, não correção de cadastro residual. (`app/api/views.py:1497`, `app/ponto/status_dia.py:255`, `app/ponto/status_dia.py:269`, `app/ponto/status_dia.py:319`, `app/ponto/precedencia.py:385`)

**Existe caminho condicional para 500:** o D4 repassa `cel.status_dia` persistido sem validar sua pertença ao vocabulário. Palavra inválida com rótulo preenchido chega ao adaptador e causa `KeyError`; sem rótulo, pode quebrar antes em `ROTULO_STATUS`. Os escritores normais derivam de regras fechadas; existência desse dado inválido em produção **[nao verificado]**. (`app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:289`, `app/ponto/status_dia.py:91`, `app/ponto/portas/celula.py:686`)

**3. Relógio**

O instante de julgamento é compartilhado: `agora` nasce na view e é repassado ao espelho, ao D4 e à regra de encerramento; o D4 só chama `now()` quando não recebe parâmetro. (`app/api/views.py:1350`, `app/api/views.py:1369`, `app/api/views.py:1498`, `app/ponto/status_dia.py:252`, `app/ponto/turnos.py:2164`)

Entretanto, há leitura de relógio fora da borda, **herdada**: carga → pareamento → interpretação da ata → `intervalo_cadastrado` → `date.today()`. Efeito dessa leitura sobre o status retornado **[nao verificado]**. (`app/ponto/turnos.py:2995`, `app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1631`, `app/ponto/juiz_batida.py:270`, `app/ponto/juiz_batida.py:125`)

**4. Custo**

A carga acrescenta **cinco consultas básicas**, com consultas condicionais para geradoras ausentes e última batida. Não são cinco consultas por dia. (`app/ponto/turnos.py:2943`, `app/ponto/turnos.py:2949`, `app/ponto/turnos.py:2953`, `app/ponto/turnos.py:2955`, `app/ponto/turnos.py:2982`, `app/escala/alimentacao.py:123`, `app/ponto/turnos.py:2962`)

Quando algum dia precisa da natureza, A2 calcula o mês inteiro uma vez por colaborador: aproximadamente mais cinco consultas básicas com vínculos, além das condicionais e relações carregadas sob demanda. (`app/ponto/status_dia.py:303`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:289`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:333`)

**Achado médio de desempenho: existe N+1 por dia comum.** Sem célula com `trabalha` definido, A2 chama a aritmética da escala; em ciclos aplicáveis, cada dia previsto consulta feriado, mesmo quando não é feriado. Há ainda consulta por feriado para seu nome. Portanto, “custo constante para 31 dias” seria incorreto. (`app/ponto/precedencia.py:359`, `app/ponto/precedencia.py:368`, `app/escala/models.py:1340`, `app/escala/models.py:1363`, `app/core/feriados.py:95`, `app/ponto/precedencia.py:407`)

Também há repetição de CPU: para cada data, o D4 pede novamente o encerramento da janela inteira — cerca de 31 varreduras de 31 dias; os turnos permanecem em cache. (`app/ponto/status_dia.py:274`, `app/ponto/turnos.py:3004`, `app/ponto/turnos.py:2158`, `app/ponto/turnos.py:2987`)

O bloco removido custava uma consulta básica de grade, mas também podia consultar por dia quando faltavam minutos previstos na ata. Saldo exato e latência por chamada **[nao verificado]**. (`app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:376`)

**5. Classes de reprovação**

- **Segundo juiz: não identificado no adaptador.** É tradução por tabela; o juiz registrado continua sendo `status_do_dia_lavrado`, que delega ao lote. (`app/ponto/status_dia.py:79`, `app/ponto/status_dia.py:89`, `app/core/juizes.py:187`, `app/ponto/status_dia.py:217`)
- **Segundo escritor persistente: não identificado no trecho alterado.** A view monta payload; a atribuição à célula continua na porta. (`app/api/views.py:1543`, `app/api/views.py:1603`, `app/ponto/portas/celula.py:701`)
- **Status paralelo remanescente: não nesses três insumos.** `_trab` e marcos alimentam previsto; `_cob_app` alimenta pendência. Porém, há **achado médio herdado nos marcos**: a view consulta o template, contrariando a autoridade de marcos congelados do vínculo. (`app/api/views.py:1584`, `app/api/views.py:1595`, `app/api/views.py:1597`, `app/docs/JUIZES.md:22`, `app/escala/models.py:1094`)
- **Mordida real, parcial.** Os testes exigem irregular com/sem batida e falta decidida; o teste invertido compara o resultado sob três respostas da grade. O teste de colaboradores que apenas procura nomes no arquivo inteiro é fraco isoladamente, pois comentários bastam. (`app/api/tests/test_e4_app_mapa.py:77`, `app/api/tests/test_e4_app_mapa.py:85`, `app/api/tests/test_e4_app_mapa.py:92`, `app/colaboradores/tests/test_t4_lei_status_none_alerta.py:167`, `app/colaboradores/tests/test_f1c_lote3_status_celula.py:67`)
- **Lacuna dos testes:** o caso irregular sem batidas não verifica `inconsistente`; o teste mensal compara carga mensal com D4 sem carga, embora a janela possa alterar o pareamento. (`app/api/tests/test_e4_app_mapa.py:90`, `app/api/tests/test_e4_app_mapa.py:109`, `app/ponto/turnos.py:2930`)
- **Pré-requisito:** há exigência documental de D5 anterior às telas e ordem de pouso; há também dívida registrada para competências antigas. Cumprimento efetivo em produção e ordem real **[nao verificado]**. (`app/docs/BACKLOG.md:802`, `app/docs/BACKLOG.md:846`, `app/docs/BACKLOG.md:1732`, `app/docs/BACKLOG.md:1736`)

O censo local executado retornou `numero=0`, `novos=[]`; isso não certifica booleanos, indireções ou HTML/JS, limitações declaradas pelo instrumento. Execução da suíte Django e prova RED/GREEN **[nao verificado]**. (`app/core/medida_leitor_do_dia.py:355`, `app/core/medida_leitor_do_dia.py:342`)

**6. Contraexemplos pela regra**

| Cenário | Resultado verificável no código |
|---|---|
| Ausência parcial lavrada | Se a parcial acusa o restante, lavra `irregular → alerta`; sem essa acusação, pode lavrar `ausencia → neutro`. Integração completa no app **[nao verificado]**. (`app/ponto/status_dia.py:104`, `app/ponto/status_dia.py:139`, `app/ponto/status_dia.py:154`, `app/ponto/status_dia.py:79`) |
| Folga trabalhada encerrada | Lavra `folga_trabalhada → ok`. (`app/ponto/status_dia.py:157`, `app/ponto/status_dia.py:80`) |
| Feriado sem batidas e sem lavra | Natureza de feriado retorna `folga`; trabalho em feriado depende do vínculo. (`app/ponto/status_dia.py:312`, `app/ponto/precedencia.py:373`) |
| Hoje antes/durante o turno | Trabalho sem batida não encerrado retorna `previsto`; turno vivo ou batida no dia ainda em curso retorna `aberto`, inclusive com saídas registradas. (`app/ponto/status_dia.py:280`, `app/ponto/status_dia.py:283`, `app/ponto/status_dia.py:316`) |
| Depois do encerramento | Lê a lavra; trabalho sem lavra retorna `sem_certidao → neutro`. (`app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:294`, `app/ponto/status_dia.py:316`, `app/ponto/status_dia.py:84`) |
| Cruza meia-noite | O fim previsto é encadeado para o dia seguinte; há, porém, dívida de janela no pareamento de saída órfã: mapa mensal não vazio sem marcos da véspera impede o uso do template. (`app/escala/servico_jornada.py:79`, `app/ponto/turnos.py:1297`, `app/ponto/turnos.py:285`) |

A dívida noturna não demonstra, sozinha, regressão exclusiva deste diff: a view anterior já perguntava geometria na janela mensal. Um dia concreto de produção que o app mostrava corretamente e agora mostra incorretamente **[nao verificado]**. (`app/api/views.py:1445`, `app/api/views.py:1449`)

**7. Código morto**

Não identifiquei variável, import ou helper tornado órfão dentro da função: cobertura continua necessária para pendência; células/folgas, para previsto; períodos, para horas; conversão de horário, para decisão. Permanecem comentários desatualizados atribuindo à cobertura a escolha do status. (`app/api/views.py:1540`, `app/api/views.py:1553`, `app/api/views.py:1568`, `app/api/views.py:1581`, `app/api/views.py:1584`, `app/api/views.py:1595`)

## 04/11 15:42 · sessao 09b04ac65aaa · - · - · origem=Ronald (tmux)

Correção do custo: a estimativa “~10” só vale quando não há consultas diárias de feriado; existe N+1 em dias comuns sem `celula.trabalha` definido (`app/ponto/precedencia.py:359–368`; `app/escala/models.py:1340–1363`; `app/core/feriados.py:29`, `:82`, `:95`).

Fórmula estática para o trecho acrescentado:

`5 + G + L + A × (4 + F + G2 + U + H + FK) + C`

- `5`: carga básica — células, candidatos, vínculos, escala da ata e batidas (`app/ponto/turnos.py:2943–2955`, `:2982–2985`; `app/ponto/turnos.py:1556–1558`).
- `G/G2`: consulta adicional quando faltam geradoras nos candidatos da carga/A2; `L`: consulta preguiçosa de última batida (`app/escala/alimentacao.py:123–127`; `app/ponto/turnos.py:2962–2963`; `app/ponto/precedencia.py:319–331`).
- `A`: A2 foi necessária, 0 ou 1; A2 calcula a janela inteira uma única vez por colaborador (`app/ponto/status_dia.py:303–307`).
- `4`: ausências, folgas lançadas, feriados e candidatos; `F`: mais uma consulta da foto quando há vínculos (`app/ponto/precedencia.py:273`, `:282`, `:287–289`, `:319–333`).
- `U`: datas da janela inteira sem `celula.trabalha` definido que atingem `base=True`, ciclo weekday e `trabalha_em_feriado=False`; **uma consulta por data, mesmo sem feriado** (`app/ponto/precedencia.py:359–368`; `app/escala/models.py:1337–1363`; `app/core/feriados.py:95`, `:82`, `:29`).
- `H`: mais uma consulta por feriado para seu nome; `FK`: relações lazy do local, inclusive `self.colaborador`, posto e município (`app/ponto/precedencia.py:407`; `app/core/feriados.py:100`, `:113–114`; `app/escala/models.py:1363`).
- `C`: catálogo frio, normalmente 0 ou 1, condicionado ao cache de 60 segundos e ao aquecimento anterior da view (`app/ponto/catalogo/ausencias.py:249`, `:257–280`, `:293–295`).

Exemplo estrutural: mês futuro 5x2 sem células pode acrescentar `U` próximo do número de dias úteis, além da base; número medido [nao verificado] (`app/escala/models.py:32`, `:1340–1363`; `app/ponto/precedencia.py:359–368`).

`fim_turno`/`marcos_do_dia` no percurso de `Carga.dias_encerrados` usam as células alimentadas e não criam consulta por dia: a carga entrega o dict até `ec.marcos_do_dia`, que o consulta diretamente; `tipo_escala` já vem por `select_related` (`app/ponto/turnos.py:2953`, `:3004–3005`, `:2164–2165`, `:2119`; `app/escala/servico_jornada.py:92`, `:69`; `app/escala/models.py:1094–1095`).

O saldo deve descontar a grade removida: uma consulta básica e eventuais consultas antigas por célula sem `ata.minutos_previstos`; portanto o saldo exato da chamada continua [nao verificado] (`app/escala/services/leitor_celula.py:350`, `:376–383`; `app/escala/utils.py:2279–2290`).

## 04/11 15:42 · sessao ac38caed6925 · - · - · origem=Ronald (tmux)

Q5 — estrutura e testes: [nao verificado]

- **Não identifiquei segundo juiz no adaptador:** `STATUS_DO_APP` é tabela e `status_do_app` apenas indexa o status recebido (`app/ponto/status_dia.py:79-91`). O juiz registrado continua sendo `status_do_dia_lavrado` (`app/core/juizes.py:183-187`), cujo corpo singular delega ao lote (`app/ponto/status_dia.py:217-218`).
- **Não há novo escritor persistente no trecho alterado:** a view consulta D4 e monta dicionários (`app/api/views.py:1495-1499`, `1543-1563`, `1603-1626`); a gravação de `CelulaDia.status_dia` continua na porta (`app/ponto/portas/celula.py:701-706`, `752-775`).
- **Os remanescentes não escolhem `status`:** `_trab` e `marcos_do_dia` montam horários; `_cob_app` alimenta `ausencia_pendente` (`app/api/views.py:1584-1586`, `1595-1600`, `1621-1625`). **Há derivação paralela de marcos:** usa o template, apesar da autoridade registrada ser `EscalaColaborador.marcos_do_dia` (`app/api/views.py:1597`; `app/core/juizes.py:201`).
- **Mordida real, mas parcial:** a bateria nova exige `irregular→alerta` com e sem batida e `falta→falta/True` (`app/api/tests/test_e4_app_mapa.py:77-97`); o teste invertido da grade compara resultados sob `CEL_OK`, `CEL_MISSING` e `CEL_NAO_SEI` (`app/colaboradores/tests/test_t4_lei_status_none_alerta.py:167-173`). A asserção do outro teste é fraca isoladamente: procura dois nomes no arquivo inteiro, permitindo satisfação por comentário (`app/colaboradores/tests/test_f1c_lote3_status_celula.py:62-69`). Execução RED/GREEN destes bytes: **[nao verificado]**.
- **Pré-requisitos:** a ordem documental exige leitores anteriores e portão de 04/11 às 18h (`app/docs/BACKLOG.md:846`, `1732`). D5 e L1 aparecem registrados como publicados (`app/docs/BACKLOG.md:802`, `839`); estado efetivo em produção e ordem real de pouso: **[nao verificado]**.

Q6 — cenários e achados:

- **Média: `alerta` sem `inconsistente`.** Dia sem batidas e lavrado `irregular` recebe `status='alerta'`, porém `inconsistente=False` literal (`app/api/views.py:1599-1620`). Com batidas, o mesmo alerta determina `inconsistente=True` (`app/api/views.py:1515-1518`). O teste novo verifica `falta=False`, mas não verifica esse booleano (`app/api/tests/test_e4_app_mapa.py:85-90`).
- **Parcial aprovada:** a lavra parcial que acusa o restante vira `irregular`, chegando ao app como `alerta`; sem acusação vira `ausencia→neutro` (`app/ponto/status_dia.py:104-110`, `139-155`, `79-84`). A regra possui testes diretos, inclusive exceção do isento (`app/ponto/tests/test_e3_status_lavrado.py:78-98`); integração desses casos no app: **[nao verificado]**.
- **Folga trabalhada e feriado:** lavra `folga_trabalhada→ok`; sem batidas e sem lavra, natureza de feriado/folga retorna `folga` (`app/ponto/status_dia.py:79-84`, `287-319`). Antes do encerramento, turno vivo ou batida no dia em curso retorna `aberto`; sem batida de trabalho retorna `previsto`; encerrado sem lavra retorna `sem_certidao→neutro` (`app/ponto/status_dia.py:273-319`).
- **Dívida concreta de janela, sem atribuição de regressão exclusiva ao diff:** template 21–05, uma única saída em 18/09 às 05h e DNA somente em 01/09 fazem o pareador atribuir a saída a 17/09 quando o mapa é vazio/`None`, mas a 18/09 quando o mapa mensal contém apenas aquele DNA distante. O ramo usa template somente quando o mapa inteiro é vazio (`app/ponto/turnos.py:1297-1300`); sem marcos da véspera, `_data_do_turno` fica na data civil (`app/ponto/turnos.py:283-296`). A carga transforma mapa vazio em `None` (`app/ponto/turnos.py:2991-2998`; `app/escala/utils.py:95-108`). Reproduzi as funções puras em memória, com adaptador de timezone da biblioteca padrão; integração Django: **[nao verificado]**.
- **O teste mensal não cobre essa dívida:** consulta a view com carga mensal, mas compara com D4 sem carga (`app/api/views.py:1498-1499`; `app/api/tests/test_e4_app_mapa.py:99-113`). A geometria mensal já existia na view (`app/api/views.py:1445-1451`), portanto esse exemplo sozinho não demonstra “app antes certo, depois errado”. Contraexemplo completo dessa forma: **[nao verificado]**.

## 04/11 15:41 · sessao 09b04ac65aaa · - · - · origem=Ronald (tmux)

Q2–Q4, somente inspeção estática; contagens executadas e incidência em produção [nao verificado].

- O mapa entrega exclusivamente `{ok, falta, previsto, folga, neutro, alerta, aberto}` e cobre as 11 palavras do núcleo; `status_do_app` indexa diretamente a tabela (`app/ponto/status_dia.py:79–91`).
- Nenhuma data solicitada desaparece por falta de vínculo, desligamento, futuro ou ausência de escala: a view solicita o mês completo; o D4 percorre cada par e todos os ramos escrevem sua saída (`app/api/views.py:1497–1499`; `app/ponto/status_dia.py:255–319`). Sem escala/natureza/lavra/batidas, termina em `sem_dados` (`app/ponto/precedencia.py:385–390`; `app/ponto/status_dia.py:308–319`).
- Há caminho condicional de `KeyError`: o D4 repassa `cel.status_dia` sem validar o vocabulário; com palavra inválida e `status_rotulo` preenchido, o erro ocorre no adaptador; sem rótulo, pode ocorrer antes em `ROTULO_STATUS` (`app/ponto/status_dia.py:287–289`, `:91`). Escritores normais da porta derivam de regras fechadas; ocorrência de palavra inválida no dado [nao verificado] (`app/ponto/portas/celula.py:686–705`; `app/escala/models.py:1781–1785`).
- O `agora` é o mesmo: capturado na view, entregue ao espelho e ao D4, depois à regra de encerramento (`app/api/views.py:1350`, `:1369`, `:1498`; `app/ponto/status_dia.py:252–254`, `:274`; `app/ponto/turnos.py:3001–3005`, `:2164–2166`).
- Existe leitura de relógio fora da borda, herdada: `Carga.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado → date.today()` (`app/ponto/turnos.py:2995`, `:1937`, `:1631`; `app/ponto/juiz_batida.py:270`, `:125`). O consumidor de papel usa entrada/saída/papéis, sem consumir o intervalo cadastrado; efeito dessa leitura no status D4 [nao verificado] (`app/ponto/turnos.py:1636`, `:1680–1683`).
- A carga acrescenta **5 consultas base**, mais até duas condicionais: células, candidatos, vínculos, escala da ata, batidas; condicionais para geradora ausente e última batida (`app/ponto/turnos.py:2943–2955`, `:2962–2963`, `:2982–2985`; `app/escala/alimentacao.py:116–127`; `app/ponto/turnos.py:1556–1558`).
- Com a carga mensal, A2 é chamada **uma vez por colaborador**, quando necessário, aproximadamente mais cinco consultas usuais, além de condicionais/FKs e uma consulta por feriado para buscar seu nome (`app/ponto/status_dia.py:303–307`; `app/ponto/precedencia.py:273`, `:282`, `:287–289`, `:319–333`, `:407`; `app/core/feriados.py:100`, `:113–114`). Não são cinco consultas multiplicadas por 31 dias; existe N+1 herdado por feriado (`app/ponto/precedencia.py:407`; `app/core/feriados.py:100`).
- Comparação: saiu uma consulta básica da grade; a grade antiga também podia consultar por dia quando faltava `ata.minutos_previstos` (`app/escala/services/leitor_celula.py:350`, `:376–383`; `app/escala/utils.py:2279–2290`). Saldo exato de consultas [nao verificado].
- Há custo de CPU repetido: o D4 pede `dias_encerrados` a cada data, e a carga recalcula a janela inteira em cada chamada; para 31 datas, aproximadamente 31 varreduras de 31 dias, sem novas consultas dessas varreduras (`app/ponto/status_dia.py:274`; `app/ponto/turnos.py:3004–3005`, `:2158–2167`). Os turnos ficam em cache (`app/ponto/turnos.py:2987–2999`).
- Ressalva de semântica: o D4 não consulta admissão/demissão para filtrar dias; a grade removida fazia esse filtro. Célula residual fora da vida pode agora fornecer status lavrado; incidência no dado [nao verificado] (`app/ponto/status_dia.py:269–319`; `app/escala/services/leitor_celula.py:351`, `:363–370`).

## 04/11 15:40 · sessao 93f2b159d10b · - · - · origem=Ronald (tmux)

Q1 — escritores e leitores do payload:

| Chave | Quem escreve |
|---|---|
| `status`, `status_dia` | D4 + adaptador nos dois ramos: `app/api/views.py:1498`, `:1515`, `:1516`, `:1561`, `:1562`, `:1599`, `:1600`, `:1617`, `:1618`. |
| `falta` | Apenas card sem batida: `_st == 'falta'`, `app/api/views.py:1620`. Card com batida não inclui essa chave: dict `app/api/views.py:1543`. |
| `inconsistente` | Com batida: turno aberto ou `status == 'alerta'`, `app/api/views.py:1518`, `:1559`. Sem batida: constante `False`, `app/api/views.py:1615`. |
| `turno_aberto` | Geometria carregada em `app/api/views.py:1445`, `:1449`, avaliada em `:1514`, escrita em `:1560`; sem batida, constante `False`, `:1616`. |
| `ausencia_pendente` | Cobertura para cobrança + `ABERTAS`, `app/api/views.py:1574`, `:1575`, `:1582`, `:1584`; inserida em ambos os ramos, `:1592`, `:1625`. |
| `previsto_inicio`, `previsto_fim`, intervalos | Apenas card sem batida: `eh_dia_trabalho` + template `TipoEscala.marcos_do_dia`, `app/api/views.py:1595`, `:1597`, escrita em `:1621`, `:1622`, `:1623`, `:1624`. |

Leituras internas: `status` alimenta `inconsistente`, `app/api/views.py:1518`; `_st` alimenta `falta`, `:1620`. O dict local é entregue em `:1675`.

Nenhum consumidor de produção do endpoint encontrado em `static/`, `templates/` ou outros endpoints por busca textual; leitores indiretos [nao verificado]. Rota encontrada: `app/api/urls.py:81`. V1 somente responde 410 com endereço v2, sem reutilizar dict: `app/api/views.py:839`, `:843`, `:844`.

O HTML do espelho usa **outro dict**, `_esp['dias']`: `app/ponto/views.py:157`, `:159`, `:193`, `:196`. Seus leitores são `d.status/status_rotulo`, `app/templates/ponto/espelho.html:151`; `dia.inconsistente`, `:166`; `t.par.turno_aberto`, `:193`; `dia.previsto/previsto_canon`, `:207`, `:218`. Não são consumidores do payload API.

Casca inclui `pwa.js`, `app/templates/base_app.html:20`; SW apenas repassa os demais fetches, `app/static/sw.js:75`. Nenhum fonte nativo encontrado pelas extensões usuais; contrato efetivo do compilado [nao verificado]. Existe referência ao pacote Play Store `com.hasner.wk`, `app/templates/core/gate_apk.html:6`, `:7`.

Leitores de teste relevantes: [nao verificado]

- `status/status_dia/falta`: `app/api/tests/test_e4_app_mapa.py:83`, `:90`, `:97`, `:112`; `app/api/tests/test_espelho_app_ausencia_pela_autoridade.py:53`, `:59`, `:66`, `:70`; `app/api/tests/test_espelho_app_hoje_no_teto.py:47`, `:52`, `:57`.
- `turno_aberto/inconsistente`: `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `:344`, `:345`, `:346`; `app/api/tests/test_espelho_app_le_o_juiz_do_dia.py:77`.
- `ausencia_pendente` e presença das antigas chaves, incluindo previsto: `app/api/tests/test_espelho_app_ausencia_pendente_aditivo.py:16`, `:30`, `:36`, `:40`, `:46`.
- Comparação de status/inconsistente sob mock da grade: `app/colaboradores/tests/test_t4_lei_status_none_alerta.py:164`, `:165`, `:171`.
- Isento: lê status/falta/inconsistente/turno_aberto, `app/escala/tests/test_art62_nunca_acusa.py:221`.

Achado **MÉDIO**: `status='alerta', status_dia='irregular', inconsistente=False` ocorre para irregular lavrado sem batida, enquanto irregular com batida produz `inconsistente=True`; derivações divergentes em `app/api/views.py:1518` e `:1615`. O caso novo sem batida não verifica `inconsistente`, `app/api/tests/test_e4_app_mapa.py:85`, `:90`. Impacto em tela compilada [nao verificado].

Mudanças semânticas que exigem confirmar consumidor externo: falta automática virou somente decidida, `app/api/tests/test_espelho_app_ausencia_pela_autoridade.py:61`, `:66`; turno aberto antigo pode ter status neutro com `turno_aberto=True`, `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `:340`, `:344`. Leitor externo dependente desses sentidos [nao verificado].

## 04/11 15:15 · sessao 8855056474c6 · E4-ESPELHO · verificador · origem=Code

**Há dois achados P2 de custo: consultas podem crescer por dia e o encerramento da janela é recalculado quadraticamente. Não encontrei contraexemplo funcional confirmado de cor ausente ou `KeyError` com entradas válidas.** (`app/ponto/precedencia.py:368`, `app/ponto/precedencia.py:407`, `app/ponto/status_dia.py:253`, `app/ponto/turnos.py:2163`, `app/templates/ponto/espelho.html:113`)

1. **Quem escreve e quem lê**

   | Fluxo | Escritor | Leitor |
   |---|---|---|
   | Faixa do espelho | Copia `status` e `rotulo` do D4 para os dois campos. | `<a>` e `<span>` usam classe e `title`. |
   | Preenchimento semanal | Cria `status='fora'`, sem `status_rotulo`. | A mesma faixa, com `.st-fora`. |
   | Calendário | Monta outro dicionário com status/rótulo do D4. | Resolvedor, resumo, grade e relatório de furos. |
   | Painel | Coloca a resposta D4 em `linha['dia']`. | Card do painel. |
   | Previsão de escala | Escreve outro `status_rotulo`. | Template da escala. |

   Referências: espelho em `app/ponto/services/espelho.py:181`, preenchimento em `app/ponto/services/espelho.py:192`, leitores em `app/templates/ponto/espelho.html:151` e CSS em `app/templates/ponto/espelho.html:124`; calendário em `app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:700`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:746`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/relatorios/views.py:1550`; painel em `app/core/services/painel_op.py:338`, `app/templates/core/_painel_card.html:67`; escala em `app/escala/views.py:60`, `app/templates/escala/minha_escala.html:75`.

   **PDF não consome os campos alterados:** chama `espelho_do_colab`, recorta seus dias e não passa por `montar_semanas`. O app também chama esse miolo, mas escreve outro payload `status`, com vocabulário próprio — inclusive `alerta/neutro`. Essa derivação paralela permanece; não foi introduzida pelo diff. (`app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`, `app/api/views.py:1369`, `app/api/views.py:1570`, `app/api/views.py:1591`, `app/api/views.py:1650`, `app/api/views.py:1673`)

   Os includes do espelho são o ícone de ausência e o relógio riscado; não leem esses campos. `dia.ausencia.status` pertence à ausência; `c.status`, ao marco. O JavaScript da faixa só posiciona a semana atual. (`app/templates/ponto/espelho.html:191`, `app/templates/ponto/espelho.html:237`, `app/templates/core/_relogio_riscado.html:15`, `app/templates/ponto/espelho.html:159`)

   Não identifiquei consumidor dos campos alterados esperando `atencao/abono/sem_registro/vazio`; essas palavras continuam no detector histórico. Os testes alterados leem status/rótulo para comparar com D4. (`app/templates/ponto/espelho.html:151`, `app/core/medida_leitor_do_dia.py:131`, `app/ponto/tests/test_caracterizacao_espelho.py:87`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:77`)

2. **Contrato e cobertura da carga**

   D4 exige datas dentro da carga e colaboradores presentes nela; rejeita violações com `ValueError`. O parâmetro opcional `fatos` exige carga e, quando fornecido, suas chaves são acessadas diretamente. (`app/ponto/status_dia.py:238`, `app/ponto/status_dia.py:241`, `app/ponto/status_dia.py:244`, `app/ponto/status_dia.py:280`)

   **A carga cobre todas as datas originais:** `min/max` vêm da mesma lista usada nos pares. O preenchimento fora desses limites nasce depois, com `status='fora'`, sem consultar D4. (`app/ponto/services/espelho.py:176`, `app/ponto/services/espelho.py:178`, `app/ponto/services/espelho.py:192`)

   **Não há caminho visível nessa view para `colaborador=None` com dias não vazios:** começa com `dias=[]` e só preenche dentro de `if colaborador`; a montagem retorna imediatamente para lista vazia. Chamador externo passando `None` com dias viola essa condição e pode atingir acesso a `.pk`. (`app/ponto/views.py:124`, `app/ponto/views.py:143`, `app/ponto/views.py:159`, `app/ponto/services/espelho.py:172`, `app/ponto/turnos.py:2164`)

   A janela visual pode começar antes do vínculo/cadastro, pois usa `min(piso_visual, apur_ini)`. Outro vínculo da mesma pessoa é resolvido pela geradora e vigência do dia; geradora fora da vigência devolve `None`, sem escolher outra silenciosamente. (`app/ponto/services/espelho.py:1225`, `app/ponto/services/espelho.py:1262`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`)

   Antes do cadastro, sem célula, vínculo, batida, ausência ou feriado, o resultado é `sem_dados`. Ausência/feriado podem fornecer natureza; uma célula já lavrada é lida antes desse ramo, mesmo sem vínculo vigente. Não há descarte universal por admissão nesse leitor. (`app/ponto/precedencia.py:385`, `app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:289`, `app/ponto/status_dia.py:297`)

3. **Relógio**

   O trecho novo passa corretamente o mesmo instante da borda para dias e semanas. D4 só chama `timezone.now()` se `agora is None`. (`app/ponto/views.py:97`, `app/ponto/views.py:158`, `app/ponto/views.py:184`, `app/ponto/status_dia.py:231`)

   **Existe leitura transitiva herdada fora da borda:** carga → pareamento → leitura da ata → `periodos_do_dia` → `intervalo_cadastrado` → `date.today()`. Não é chamada textual nova no diff. (`app/ponto/turnos.py:2995`, `app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1631`, `app/ponto/juiz_batida.py:270`, `app/ponto/juiz_batida.py:125`)

   Essa leitura alimenta intervalo; o consumidor da ata usa papéis e bordas. Alteração do novo status por esse relógio: **[nao verificado]**. (`app/ponto/juiz_batida.py:299`, `app/ponto/turnos.py:1636`, `app/ponto/turnos.py:1680`)

4. **Custo — dois achados P2**

   **P2 — não é estritamente O(1) em dias.** Para lista não vazia, a carga possui cinco consultas estruturais: células, candidatos, vínculos, ata e batidas; pode acrescentar uma para completar geradoras e uma para última batida. Lista vazia acrescenta zero. (`app/ponto/turnos.py:2943`, `app/ponto/turnos.py:2949`, `app/ponto/turnos.py:2953`, `app/ponto/turnos.py:2955`, `app/ponto/turnos.py:2962`, `app/ponto/turnos.py:2982`, `app/escala/alimentacao.py:123`, `app/ponto/services/espelho.py:172`)

   Quando algum dia alcança natureza, D4 chama A2 uma vez para a janela inteira. Há quatro consultas estruturais adicionais, mais complementação de geradoras, foto de folgas e carregamentos condicionais. (`app/ponto/status_dia.py:282`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:289`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:333`)

   Há dois caminhos concretos de N+1:

   - Cada feriado da janela pode consultar novamente seu nome. (`app/ponto/precedencia.py:407`, `app/core/feriados.py:100`)
   - Sem célula com `trabalha` definido, o cálculo pelo vínculo pode consultar feriado por data nos ciclos aplicáveis. Portanto, o pior caso cresce com os dias. (`app/ponto/precedencia.py:368`, `app/escala/models.py:1340`, `app/escala/models.py:1363`, `app/core/feriados.py:82`)

   **P2 — CPU O(D²):** para cada data pedida, D4 chama `carga.dias_encerrados`; esse método não memoriza a resposta e percorre novamente toda a janela. O pareamento é memorizado, o encerramento não. (`app/ponto/status_dia.py:248`, `app/ponto/status_dia.py:253`, `app/ponto/turnos.py:2987`, `app/ponto/turnos.py:3004`, `app/ponto/turnos.py:2163`)

   Quantidade efetiva de consultas, duração e incidência em produção: **[nao verificado]**.

5. **Classes de reprovação**

   - **Segundo juiz:** não identificado no trecho novo; ele copia D4, cuja autoridade já está registrada. (`app/ponto/services/espelho.py:177`, `app/ponto/services/espelho.py:181`, `app/core/juizes.py:187`)
   - **Segundo escritor fora da porta:** não identificado; a atribuição nova é no dicionário de apresentação. A escrita persistente permanece na porta. (`app/ponto/services/espelho.py:181`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:709`)
   - **Derivação paralela:** removida da faixa; a do app permanece declarada. (`app/ponto/services/espelho.py:152`, `app/core/medida_leitor_do_dia.py:335`)
   - **Dublê unitário:** morde geometria, cards e preenchimento; não protege status/rótulo nem argumentos da carga. Executei as quatro funções de teste isoladas em memória: passaram também removendo a atribuição dos dois campos. Isso corresponde ao escopo declarado, portanto não reprova sozinho a migração. (`app/ponto/tests/test_caracterizacao_espelho_unit.py:21`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:27`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:46`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:53`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:68`)
   - **Selo dedicado:** possui dois resultados fixos `irregular`, além da paridade; não depende apenas de comparar o leitor consigo mesmo. Já o teste auxiliar da regex só testa uma string local — **P3, sem mordida no produto**. (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:46`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:56`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:64`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:110`)
   - **Pré-requisito e ordem de trava/pouso:** há exigências documentadas de D5 anterior aos leitores, reset às 18h e sequência da NOITE-0411. Execução real, RED observado e suíte certificada: **[nao verificado]**. (`app/docs/BACKLOG.md:802`, `app/docs/BACKLOG.md:1732`)

6. **Pendente reancorado e dente do contrato**

   **A impressão nova existe exatamente uma vez no arquivo alvo**, na chamada D4, e há um registro correspondente. O pendente foi mantido explicitamente, sem baixa silenciosa. (`app/ponto/services/espelho.py:177`, `app/core/juizes.py:863`, `app/core/juizes.py:866`)

   **O dente sintático continua; o semântico é limitado — P3.** O contrato verifica presença da substring. Não exige chamada executável, uso da resposta ou ocorrência única no fonte; comentário contendo a impressão bastaria. O teste de duplicidade verifica registros repetidos, não ocorrências no arquivo. (`app/core/tests/test_contract_juiz_tela.py:34`, `app/core/tests/test_contract_juiz_tela.py:37`, `app/core/tests/test_contract_juiz_tela.py:52`)

7. **Contraexemplo de cor ou `KeyError`**

   Não encontrei palavra válida do núcleo sem CSS: conferi as 11, todas cobertas. `fora` é preenchimento externo ao vocabulário e possui regra própria. (`app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/templates/ponto/espelho.html:113`, `app/templates/ponto/espelho.html:124`, `app/ponto/services/espelho.py:192`)

   Há uma hipótese condicional: D4 devolve `cel.status_dia` sem validar o vocabulário nesse ponto. Valor inválido com rótulo preenchido chegaria sem CSS; sem rótulo, causaria `KeyError` em `ROTULO_STATUS[...]`. O modelo declara as 11 escolhas e a porta deriva os valores pelo núcleo; existência de coluna inválida ou vítima em produção: **[nao verificado]**. (`app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:268`, `app/escala/models.py:1781`, `app/ponto/portas/celula.py:693`)

## 04/11 15:14 · sessao 8cc010661e79 · - · - · origem=Ronald (tmux)

**Q2 — contrato e cobertura**

- `montar_semanas` monta a carga com `min/max` das próprias datas perguntadas: nenhuma data original de `dias` pode ficar fora dessa janela. O preenchimento das semanas acontece depois e usa `status='fora'`, sem consultar D4 para esses espaços. `app/ponto/services/espelho.py:176-178,183-192`.
- D4 exige datas em `[carga.ini,carga.fim]` e colaboradores presentes em `carga._colabs`; violações levantam `ValueError`. `fatos` fornecidos exigem carga e mapa completo; chave ausente não recebe tentativa alternativa. `app/ponto/status_dia.py:238-246,279-286`.
- Na view, colaborador ausente deixa `dias=[]`; a chamada a `espelho_do_colab` só ocorre dentro de `if colaborador`, e `montar_semanas` retorna antes de acessar `.pk` para lista vazia. Não há caminho visível nessa view para `None` com dias não vazios. Chamadas externas incompatíveis continuam capazes de falhar. `app/ponto/views.py:110-124,143-159,184`; `app/ponto/services/espelho.py:172-180`.
- A janela visual pode começar antes do vínculo/admissão: `vis_ini=min(piso_visual(...),apur_ini)` e `montar_dias` enumera todas as datas até hoje; não há filtro por admissão no D4. `app/ponto/services/espelho.py:1225-1227,1262-1266,482,660-661`; `app/ponto/status_dia.py:248-298`.
- Outro vínculo da mesma pessoa é tratado pelo vínculo do dia, escolhido pela geradora e vigência; a carga inclui todos os vínculos para encerramento, além dos candidatos e geradoras para pareamento. Geradora fora da vigência devolve `None`, sem escolher outra silenciosamente. `app/ponto/turnos.py:2943-2955,3004-3005,2164`; `app/escala/alimentacao.py:237-254`.
- Antes do cadastro, sem célula, vínculo, batida, ausência ou feriado: A2 produz faixa `None` e D4 responde `sem_dados`; ausência/feriado existentes podem responder sua natureza. Uma célula já lavrada é lida antes da natureza, inclusive sem vínculo vigente. `app/ponto/precedencia.py:349-385,386-390`; `app/ponto/status_dia.py:265-298`.

**Q3 — relógio**

- O diff passa corretamente o mesmo `_agora_do_ato` da borda à montagem dos dias e das semanas; D4 só chama `timezone.now()` quando recebe `agora=None`. `app/ponto/views.py:97-98,158,182-184`; `app/ponto/status_dia.py:231-233`.
- **Ressalva herdada:** existe leitura transitiva fora da borda: `Carga.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado → date.today()`. Não é linha nova do diff. `app/ponto/turnos.py:2995-2998,1937,1631`; `app/ponto/juiz_batida.py:270,125`.
- Essa leitura alimenta o resultado de intervalo; o consumidor do pareamento usa papéis, entrada, saída e `em_aberto`, sem ler aquele intervalo. Efeito dessa leitura sobre o novo status não foi demonstrado. `app/ponto/juiz_batida.py:299-309`; `app/ponto/turnos.py:1634-1636,1680-1683` — efeito no status `[nao verificado]`.

**Q4 — custo e achados**

- **Média — a consulta não é estritamente O(1) em dias.** Para render não vazio, a carga adiciona **5 consultas base + G + U**, com `G∈{0,1}` para completar geradoras e `U∈{0,1}` para última batida: células, candidatos, vínculos, ata e batidas. `app/ponto/turnos.py:2943-2963,2978-2998`; `app/escala/alimentacao.py:116-127`; `app/ponto/turnos.py:1554-1559`.
- Havendo algum dia sem lavra/batida que alcance natureza, A2 roda uma vez para toda a janela: **+4 + G₂ + F + H + M + R + C**, onde `G₂` completa geradoras, `F` consulta foto de folgas se houver vínculos, `H` conta nomes de feriados, `M` conta consultas de feriado nos dias calculados sem célula, `R` são relações carregadas sob demanda e `C` eventual leitura do catálogo sem cache válido. `app/ponto/status_dia.py:275-286`; `app/ponto/precedencia.py:273-289,319-333,407`; `app/ponto/catalogo/ausencias.py:267-280,293`.
- **N+1 concreto:** para cada feriado encontrado na janela, A2 chama `nome_do_feriado`, que consulta novamente aquele dia. `app/ponto/precedencia.py:407`; `app/core/feriados.py:98-102,27-29`.
- **N+1 concreto e O(dias) no pior caso:** faltando célula com `trabalha` definido, A2 calcula o dia pelo vínculo; em ciclos `5x2/6x1/semanal`, trabalho pode chamar `eh_feriado` novamente por data. O vínculo carregado inclui `tipo_escala`, mas não `colaborador/posto/municipio`, podendo acrescentar consultas por vínculo usado. `app/ponto/precedencia.py:359-370`; `app/escala/models.py:1340,1357-1363,32`; `app/core/feriados.py:91-95,70-83,113-114`; `app/escala/alimentacao.py:120`.
- **Média — CPU quadrática:** D4 chama `carga.dias_encerrados` para cada data; esse método não memoriza o resultado e percorre novamente toda a janela. Portanto, há O(D²) avaliações do encerramento para D dias, além do pareamento já memorizado. `app/ponto/status_dia.py:248-253`; `app/ponto/turnos.py:3001-3005,2158-2167,2987-2999`.
- Essa repetição de encerramento **não** consulta célula/marcos por dia: passa o mapa completo até `EC.marcos_do_dia`; célula ausente usa o template carregado. O N+1 identificado está na natureza/A2. `app/ponto/turnos.py:2164-2165,2119`; `app/escala/servico_jornada.py:68-71,89-93`; `app/escala/models.py:1094-1108`.
- Número efetivo de consultas, duração e incidência desses caminhos em produção: `[nao verificado]`.

## 04/11 15:13 · sessao 1c67dccb65e3 · - · - · origem=Ronald (tmux)

1. No fluxo do espelho, `montar_semanas` escreve `dia['status']` e `dia['status_rotulo']` copiando o D4; escreve também `status='fora'` nos preenchimentos da semana, sem rótulo. O leitor dos dois campos é a faixa, nos elementos `<a>` e `<span>`. (`app/ponto/services/espelho.py:177`, `app/ponto/services/espelho.py:181`, `app/ponto/services/espelho.py:192`, `app/templates/ponto/espelho.html:151`)

2. PDF e app usam o miolo `espelho_do_colab`, sem passar por `montar_semanas`: o PDF coleta/recorta seus dias; o app constrói outro payload e calcula seu próprio `status` — `aberto/alerta/ok` com batidas, `ok/falta/neutro/previsto/folga` sem batidas. Portanto o vocabulário paralelo do app permanece, mas não é consumidor do campo alterado neste diff. (`app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`, `app/api/views.py:1369`, `app/api/views.py:1570`, `app/api/views.py:1591`, `app/api/views.py:1643`, `app/api/views.py:1650`, `app/api/views.py:1653`, `app/api/views.py:1656`, `app/api/views.py:1673`)

3. Os includes diretos do espelho são o ícone de ausência e o relógio riscado. O caminho do primeiro vem do catálogo e produz SVG; o segundo recebe contexto `only` e lê minutos/sentido/marco. As leituras `dia.ausencia.status` e `c.status` existentes no template pertencem à ausência e ao marco, respectivamente. (`app/templates/ponto/espelho.html:191`, `app/templates/ponto/espelho.html:237`, `app/ponto/services/espelho.py:856`, `app/ponto/catalogo/ausencias.py:178`, `app/templates/core/_icone_dia_falta.html:9`, `app/templates/core/_relogio_riscado.html:15`)

4. Outros produtores de dicionários de dia na árvore: calendário escreve os dois campos; previsão da escala escreve apenas `status_rotulo`; painel coloca a resposta D4 em `linha['dia']`. Seus leitores são o resolvedor e resumo do calendário, a grade do calendário, o mapa de furos, o card do painel e a previsão de escala. Nenhum desses pontos consome os campos produzidos por `montar_semanas`. (`app/colaboradores/services/calendario.py:637`, `app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:700`, `app/escala/views.py:54`, `app/escala/views.py:60`, `app/core/services/painel_op.py:336`, `app/core/services/painel_op.py:338`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:746`, `app/colaboradores/services/calendario.py:764`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/templates/colaboradores/partials/_calendario_grade.html:99`, `app/templates/colaboradores/partials/_calendario_grade.html:143`, `app/relatorios/views.py:1550`, `app/templates/core/_painel_card.html:67`, `app/templates/escala/minha_escala.html:75`, `app/templates/escala/minha_escala.html:84`)

5. Não identifiquei leitor de **status do dia** ainda comparando `atencao/abono/sem_registro/vazio`; as palavras permanecem na catraca como vocabulário histórico, e `abono` continua sendo tipo de ausência. (`app/core/medida_leitor_do_dia.py:131`, `app/core/medida_leitor_do_dia.py:134`, `app/ponto/catalogo/ausencias.py:102`, `app/templates/ponto/espelho.html:151`)

7. Não há contraexemplo confirmado de palavra válida do núcleo sem regra CSS: as 11 palavras têm regra; o preenchimento `fora`, externo às 11, também tem CSS próprio e já é gerado separadamente. (`app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/templates/ponto/espelho.html:113`, `app/templates/ponto/espelho.html:124`, `app/ponto/services/espelho.py:192`)

Possibilidade condicional, **sem ocorrência demonstrada**: no ramo lavrado, D4 devolve `cel.status_dia` sem validar pertinência ao vocabulário. Uma coluna inválida com rótulo preenchido chega à classe sem CSS; sem rótulo, `ROTULO_STATUS[cel.status_dia]` levanta `KeyError`. O modelo declara somente as 11 escolhas e a porta deriva o valor pelas funções do núcleo; existência de coluna inválida em produção é `[nao verificado]`. (`app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:268`, `app/escala/models.py:1781`, `app/escala/models.py:1784`, `app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:698`)

## 04/11 15:13 · sessao caedc4e36b15 · - · - · origem=Ronald (tmux)

5. Classes conferidas no diff:

- **Segundo juiz:** não nasce nova derivação no trecho; `montar_semanas` lê D4 e copia status/rótulo. O juiz já está registrado e `_status_dia` foi removida, deixando lápide. (`app/ponto/services/espelho.py:152`, `app/ponto/services/espelho.py:177`, `app/ponto/services/espelho.py:181`, `app/core/juizes.py:187`)
- **Segundo escritor fora da porta:** o trecho novo escreve apenas o dicionário de apresentação; a escrita persistente continua na porta de célula. Não há novo escritor de `CelulaDia.status_dia/status_rotulo` nesse trecho. (`app/ponto/services/espelho.py:181`, `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:709`)
- **Fonte única:** o novo caminho lê a resposta do D4; preserva-se o vocabulário antigo no detector para acusar sua volta, e a retirada de `DECLARADOS` corresponde à eliminação da função. (`app/ponto/services/espelho.py:177`, `app/core/medida_leitor_do_dia.py:131`, `app/core/medida_leitor_do_dia.py:329`)
- **Selo unitário com dublê — limitação, baixa:** ele não morde status, rótulo, passagem de `agora` nem construção correta da carga: substitui ambos e só afirma geometria/cards/padding. Uma atribuição constante de status aos dias reais passaria por seus asserts. Isso corresponde ao escopo declarado de geometria, portanto não reprova isoladamente a migração. (`app/ponto/tests/test_caracterizacao_espelho_unit.py:21`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:27`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:46`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:53`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:68`)
- **Selo dedicado tem resultado discriminante:** os dois casos fixam `irregular`, contrariando as respostas antigas `ok`/`sem_registro`; outro compara status e rótulo com o D4. Não é tudo tautologia/paridade. (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:46`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:56`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:64`)
- **Selo auxiliar sem mordida no produto — baixa:** `test_MORDE_o_padrao_de_cor_acusa_palavra_sem_regra` só testa regex contra string local; nenhuma mutação do template o derruba. O teste anterior, porém, lê o template e cobra todas as palavras. (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:99`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:110`)
- **RED efetivamente observado, suite verde, pré-requisitos e ordem operacional de trava/pouso:** `[nao verificado]`. Há requisito escrito de reset 18:00, item 3 após E4-8 e OTIMIZA 1–3, com um pouso por vez; isso não prova sua execução. (`app/docs/BACKLOG.md:1732`) A própria regra de revisão exige suite inteira antes da leitura final. (`CLAUDE.md:369`)

6. Pendente reancorado:

- A nova impressão ocorre **exatamente uma vez no fonte alvo**, linha 177; o registro exato também ocorre uma vez, linha 866. Conferi por contagem de texto e AST em memória, sem import de aplicação. (`app/ponto/services/espelho.py:177`, `app/core/juizes.py:866`)
- **Dente sintático preservado:** remover essa substring faz o contrato detectar impressão desaparecida. O teste de duplicidade verifica apenas duplicação de registros `(arquivo, impressão)`. (`app/core/tests/test_contract_juiz_tela.py:34`, `app/core/tests/test_contract_juiz_tela.py:52`)
- **Limitação semântica, baixa:** o contrato não exige chamada executável, posição em `montar_semanas`, cópia da resposta nem ocorrência única; comentário/chamada morta com a impressão bastaria. Logo, continua guardando presença do endereço, não a cura. (`app/core/tests/test_contract_juiz_tela.py:35`, `app/core/tests/test_contract_juiz_tela.py:37`)
- A retenção do pendente é explícita e remete à L-118; não houve baixa silenciosa do registro. A L-118 exige medição de frota, contador e reversão, que o material fornecido não comprova. (`app/core/juizes.py:863`, `app/docs/LEIS.md:139`)

## 04/11 12:25 -- HISTORICO (mais de 24 h) · sessao b12a53e8fff6 · DEPLOY-[nome]-ELO-DA-CORRENTE · verificador · origem=Code

**Reprovo o aval literal:** há consumidores que continuam ignorando falha de reload, a trava nova não estabelece Single Writer da árvore, e `--so-conferir` pode executar deploy. (`app/core/integrador_lote.py:638`; `app/core/esteira_vigia.py:1267`; `bin/vigia_logins.sh:71`; `bin/trava.sh:64`; `bin/deploy.sh:60`.)

1. **Modos e passagem pela recusa**

   - Normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` e `--reload-agendado`, quando passam pelas guardas, convergem no mesmo `docker kill -s HUP`, **sem chamar `recusa_restart_com_cron`**. Isso é deliberado para o HUP; não há restart nesses ramos. (`bin/deploy.sh:295`; `bin/deploy.sh:300`; `bin/deploy.sh:315`.)
   - O único `docker restart` fica no ramo `RESTART=1`, imediatamente depois da recusa; há outra recusa antes das migrations. Sem motivo, `--restart` termina com rc 2. (`bin/deploy.sh:72`; `bin/deploy.sh:112`; `bin/deploy.sh:297`; `bin/deploy.sh:298`.)
   - **`--so-conferir` não é reconhecido.** O parser reconhece `--conferir`; a grafia perguntada mantém `SO_CONFERIR=0` e pode chegar a migration e HUP. Argumentos desconhecidos também não são rejeitados. (`bin/deploy.sh:56`; `bin/deploy.sh:60`; `bin/deploy.sh:167`; `bin/deploy.sh:315`.)
   - O verdadeiro `--conferir` termina sem reload e com rc 0, inclusive quando o mensageria não respondeu ao check. Os demais encerramentos anteriores ao sinal são recusas explícitas. (`bin/deploy.sh:147`; `bin/deploy.sh:171`; `bin/deploy.sh:89`.)
   - Há **sucesso possível sem provar a troca dos workers**: lista anterior vazia apenas avisa e pula; lista atual vazia satisfaz “nenhum antigo restou”. A prova posterior abre outro Python por `docker exec`, não consulta os workers HTTP. (`bin/deploy.sh:318`; `bin/deploy.sh:321`; `bin/deploy.sh:324`; `bin/deploy.sh:353`.)

2. **Segundo caminho, fallback e Single Writer**

   Não localizei outro emissor operacional de HUP/restart das três cascas em `bin/`: as emissões encontradas estão no deploy. Falha do HUP encerra; o laço posterior somente observa workers e também encerra por falha. Não há fallback por `ESPERA_CRON_S` nem operação compose nesse fluxo. (`bin/deploy.sh:298`; `bin/deploy.sh:315`; `bin/deploy.sh:320`; `bin/deploy.sh:328`.)

   **Passar pela recusa não garante ausência de cron no instante do restart.** Um envelope pode criar sua marca depois da segunda consulta e antes do comando seguinte: criação e restart não compartilham trava. (`bin/cron_run.sh:9`; `bin/cron_run.sh:11`; `bin/deploy.sh:297`; `bin/deploy.sh:298`.)

   **Single Writer global continua furado:** o vigia usa sua trava própria para executar `git revert`, enquanto a porta da árvore usa `/tmp/cadeia.lock`. A nova trava impede duas passadas desse vigia, mas não impede concorrência com uma cadeia. (`bin/vigia_logins.sh:71`; `bin/vigia_logins.sh:87`; `bin/trava.sh:64`; `bin/molde_fatia/cadeia.sh:11`.)

3. **Guarda de migration do mensageria**

   A guarda fecha corretamente diante de ausência de resposta: qualquer rc diferente de zero vira `PEND_MSG=1`, incluindo rc diferente de 1 e container indisponível. Ela descarta a distinção entre pendência e erro de execução, além de ocultar stderr. (`bin/deploy.sh:141`; `bin/deploy.sh:144`.)

   Isso bloqueia normal, cópia e agendado **antes da migration do core e do reload**. Só conferência e restart são exceções; cópia/agendado não ligam nenhuma dessas exceções. (`bin/deploy.sh:57`; `bin/deploy.sh:79`; `bin/deploy.sh:147`; `bin/deploy.sh:167`.)

   **Recusar às 03:30 é coerente com o contrato:** o HUP publica a árvore viva e não executa a migration existente no comando de boot. Liberá-lo com pendência publicaria código sem cumprir esse pré-requisito. O custo é perder a higiene de memória daquela rodada; necessidade concreta ou ocorrência em produção: **[não verificado]**. (`bin/deploy.sh:187`; `docker-compose.mensageria.yml:20`; `app/config/crons.py:300`; `app/config/crons.py:305`.)

4. **Escritores, leitores e consumidores restantes**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `RESTART` | Inicialização em 0; apenas `--restart` põe 1. (`bin/deploy.sh:67`; `:70`.) | Recusa inicial, exceção da guarda, escolha do sinal e mensagem final. (`bin/deploy.sh:112`; `:147`; `:295`; `:400`.) |
   | `PEND_MSG` | Resultado do check: sucesso 0; qualquer falha 1. (`bin/deploy.sh:141`; `:144`.) | Diagnóstico e guarda. (`bin/deploy.sh:146`; `:147`.) |
   | `logs/crons_em_curso` | Envelope cria/escreve e remove por `trap EXIT`; **deploy também remove** marcas cujo PID considera morto. (`bin/cron_run.sh:9`; `:10`; `bin/deploy.sh:106`.) | `em_curso`, chamado pela recusa. (`bin/deploy.sh:105`; `:108`.) |
   | `logs/vigia_logins.lock` | Vigia abre em append e adquire flock; selo ocupa o mesmo arquivo no fixture. (`bin/vigia_logins.sh:71`; `:72`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:58`.) | O flock decide entrada ou recusa. (`bin/vigia_logins.sh:72`.) |
   | `deploy: OK` | Ambos os finais do deploy preservam o prefixo. (`bin/deploy.sh:401`; `:403`.) | Os dois moldes procuram apenas `^deploy: OK`: continuam compatíveis. (`bin/molde_fatia/cadeia.sh:90`; `bin/molde_autocura/cadeia.sh:87`.) |
   | rc em `_subir` | `_rodar` devolve o returncode real; `_subir` o captura. (`app/core/integrador_lote.py:247`; `:753`.) | Falha grava diagnóstico e retorna antes de FIM/NO_AR; `_fechar` persiste o registro. (`app/core/integrador_lote.py:754`; `:761`; `:763`; `:766`; `:825`.) |

   **Consumidores não migrados e afirmações excessivas:**

   - O integrador ignora o rc do `--reload-copia` e começa a suíte mesmo após recusa do reload. (`app/core/integrador_lote.py:638`; `:639`.)
   - `esteira_vigia.auto_revert` ignora os resultados dos dois deploys e pode anunciar restauração da tela com base apenas nos testes. (`app/core/esteira_vigia.py:1267`; `:1269`; `:1275`; `:1287`.)
   - Os dois `desfazer_copia` usam `set +e`, ignoram o rc do reload e anunciam “recarregada”. (`bin/molde_fatia/cadeia.sh:26`; `:32`; `:33`; `bin/molde_autocura/cadeia.sh:25`; `:31`; `:32`.)
   - O CLI do integrador ainda devolve 0 depois de `rodar`, mesmo com veredito `deploy falhou`. (`app/core/integrador_lote.py:856`; `:858`.)
   - A mensagem nova “PROD SEGUE no código de antes” **não decorre de rc não zero**: pode haver falha depois do sinal, nas rotas ou no selo. O estado efetivo nesse caso é **[não verificado]**. (`app/core/integrador_lote.py:758`; `bin/deploy.sh:315`; `:374`; `:381`.)

5. **Os selos mordem, mas deixam lacunas**

   O selo do deploy tem mordida sobre a escolha do comando: exige sucesso, um HUP, zero restart e três trocas; o restart falso mata os sleeps registrados. Porém, a sobrevivência ao HUP é comportamento implementado pelo próprio falso, não prova do runtime Docker. (`bin/tests/test_deploy_reload_gracioso.sh:62`; `:64`; `:67`; `:103`.)

   Há cobertura ausente para cópia, conferência, mensageria com rc diferente de 1, HUP falho e workers sem troca. O sleep também pode terminar antes do sinal se a execução demorar, tornando vácua a parte temporal da sobrevivência. (`bin/tests/test_deploy_reload_gracioso.sh:83`; `:91`; `:111`; `:145`.)

   O selo do vigia possui controle positivo útil e verifica recusa com trava ocupada. Mas o deploy falso termina imediatamente: não testa manutenção da trava durante um deploy demorado nem detecta retirada do `7>&-`. O caso `Revert` só exige ausência de commits/deploy, sem exigir mensagem ou rc do ramo esperado. (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`; `:52`; `:62`; `:70`; `:71`.)

   **Existe risco condicional de tocar Docker real:** ambos preservam o restante do `PATH`, sem verificar qual executável foi resolvido. Se o stub não puder executar — por exemplo, temporário em filesystem `noexec` — a busca pode alcançar um Docker posterior no `PATH`; o deploy copiado mantém os nomes reais das cascas. As guardas de raiz não isolam o daemon. Condições reais desse cenário: **[não verificado]**. (`bin/tests/test_deploy_reload_gracioso.sh:36`; `:73`; `:88`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:26`; `:38`; `:45`; `bin/deploy.sh:315`.)

   O teste Python acrescentado também é textual: remover o retorno antecipado de `_subir` não viola suas asserções sobre presença e ordem das strings. (`app/core/tests/test_selo_pronta_nao_e_residuo.py:219`; `:224`; `app/core/integrador_lote.py:761`.)

   Execução GREEN/RED dos dois selos: **[não verificado]**; eles criam e removem arquivos, contrariando a regra desta verificação. (`bin/tests/test_deploy_reload_gracioso.sh:33`; `:98`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:22`; `:54`.)

6. **fd 7, fechamento e `Revert "`**

   O uso mostrado está correto para serializar este vigia: o pai mantém o fd 7 aberto enquanto espera; `7>&-` fecha apenas o descritor recebido pelo deploy. A trava cobre as leituras decisórias, o revert e a espera pela publicação. (`bin/vigia_logins.sh:71`; `:76`; `:87`.)

   A guarda é apenas um **prefixo literal e sensível a maiúsculas**: títulos editados podem escapar, e um commit comum intitulado `Revert "..."` também será bloqueado. O fixture testa justamente esse segundo caso, criando um commit comum com esse título. (`bin/vigia_logins.sh:78`; `:80`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:42`; `:70`.)

   **O buraco principal é de recuperação:** se o revert funcionar e o deploy falhar antes do HUP, a próxima passada vê `Revert "` e não tenta publicar novamente. A reversão pode ficar somente no disco, sem retry automático; o selo não cobre porque seu deploy sempre retorna zero. (`bin/vigia_logins.sh:80`; `:87`; `bin/deploy.sh:147`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`.)

## 04/11 12:23 -- HISTORICO (mais de 24 h) · sessao 9bfce67a1876 · - · - · origem=Ronald (tmux)

Achados dos itens 5–6 (análise estática; execução GREEN/RED e condições reais de produção [não verificado]):

1. **Buraco de recuperação:** se `git revert` funcionar e o deploy falhar antes do HUP, o próximo ciclo encontra `Revert "` e pula também a publicação. Assim, pode deixar o rollback apenas no disco, com os workers anteriores ainda vivos. O ramo de erro só imprime mensagem; não registra publicação pendente nem tenta publicar novamente (`bin/vigia_logins.sh:80–87`; exemplo de falha antes do HUP: `bin/deploy.sh:147–149`). O selo não cobre: seu deploy falso sempre retorna zero (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`).

2. **Os selos podem alcançar Docker real sob condição de ambiente:** os stubs são escolhidos exclusivamente por prefixo de `PATH`, preservando o restante; não há verificação de que o `docker` resolvido é o falso (`bin/tests/test_deploy_reload_gracioso.sh:73–89`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:38–45`). Se o temporário estiver em filesystem `noexec` ou o stub não puder executar, a busca pode encontrar outro Docker no `PATH`. O selo do deploy mantém os nomes das cascas reais, e o script copiado contém o HUP/restart desses nomes (`bin/deploy.sh:298`, `bin/deploy.sh:315`). As guardas de raiz só procuram o literal `/home/ronald/saas-hasner`; não isolam o daemon Docker (`bin/tests/test_deploy_reload_gracioso.sh:36–39`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:25–28`). Existência de Docker real acessível/noexec neste ambiente: [não verificado].

3. **O selo do deploy morde regressões de escolha do comando:** exige rc zero, um HUP, zero restart e três mensagens de troca nos modos que exercita; o restart falso mata os sleeps registrados (`bin/tests/test_deploy_reload_gracioso.sh:64–69`, `bin/tests/test_deploy_reload_gracioso.sh:101–117`). Entretanto:
   - Não exercita `--reload-copia`, `--conferir`, rc de mensageria diferente de 1, falha do HUP ou workers que não trocam (`bin/tests/test_deploy_reload_gracioso.sh:110–150`).
   - “O exec sobreviveu ao HUP” é comportamento imposto pelo próprio falso; o ramo `kill` só avança gerações. Não é prova do runtime Docker (`bin/tests/test_deploy_reload_gracioso.sh:59–69`).
   - O sleep dura 5/9 s e não há confirmação de que ainda estava vivo no instante do sinal: essa parte pode ser temporalmente vácua se o deploy demorar (`bin/tests/test_deploy_reload_gracioso.sh:83–91`, `bin/tests/test_deploy_reload_gracioso.sh:111–120`).

4. **O selo do vigia possui controle útil, mas cobertura incompleta:** o primeiro caso exige que o ramo automático faça um novo commit e uma chamada de deploy; o segundo exige recusa com lock externo ocupado (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:50–65`). Contudo, o deploy falso retorna imediatamente, portanto remover `7>&-` ou liberar a trava antes do término do deploy não seria detectado por esses casos (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`, `bin/tests/test_vigia_logins_sem_sobreposicao.sh:58–65`). O caso `Revert` só exige ausência de commits/deploy: não exige rc nem mensagem esperados, aceitando também erro que ocorra nesse caso (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:70–72`).

5. **fd 7/fechamento estão corretos no caminho mostrado:** abre o arquivo em append, adquire `flock -n 7` antes das leituras usadas para decidir o revert e mantém o descritor do processo pai até terminar. `7>&-` fecha somente a cópia do descritor recebida pelo deploy; o pai continua segurando a trava enquanto aguarda o comando (`bin/vigia_logins.sh:71–87`). Isso coordena passadas deste vigia; outros escritores da árvore não adquirem essa trava automaticamente (`bin/vigia_logins.sh:71–87`).

6. **A guarda `Revert "` é apenas um prefixo literal e sensível a maiúsculas:** cobre o assunto convencional produzido pelo próprio `git revert --no-edit`, mas não identifica semanticamente reversões com título editado; também bloqueia qualquer commit normal cujo título comece assim (`bin/vigia_logins.sh:78–87`). O fixture cria um commit normal intitulado `Revert "..."`, não uma reversão real, e por isso testa precisamente o prefixo (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:39–42`, `bin/tests/test_vigia_logins_sem_sobreposicao.sh:70`).

## 04/11 12:22 -- HISTORICO (mais de 24 h) · sessao c2861ff43f2f · - · - · origem=Ronald (tmux)

Mapa de escritores/leitores e falhas:

- **RESTART**: valor local inicializado em `0`, alterado para `1` apenas por `MODO=--restart`; motivo obrigatório (`bin/deploy.sh:67-72`). Leitores: primeira recusa (`bin/deploy.sh:112-114`), exceção da guarda mensageria (`bin/deploy.sh:147`), escolha restart/HUP (`bin/deploy.sh:295-315`), frase final (`bin/deploy.sh:400-403`). O contrato examina textualmente esse ramo (`bin/contrato_deploy.sh:41-49`).

- **PEND_MSG**: escrito pelo resultado de `docker exec mensageria … migrate --check`, `0` em sucesso e `1` em qualquer falha (`bin/deploy.sh:141-145`); lido na saída e na guarda (`bin/deploy.sh:146-149`). O fake do selo fornece o rc por `FAKE_PEND_MSG` (`bin/tests/test_deploy_reload_gracioso.sh:55`).

- **logs/crons_em_curso**: envelope cria diretório/marca `${N}_${E}.$$`, escreve horário e remove em `trap EXIT` (`bin/cron_run.sh:9-10`). Deploy percorre marcas e consulta `/proc/<pid>`; **também escreve**, removendo marcas consideradas mortas (`bin/deploy.sh:105-106`). Leitura para recusa usa `em_curso` (`bin/deploy.sh:107-110`). O selo cria marca falsa de PID vivo (`bin/tests/test_deploy_reload_gracioso.sh:85`).

- **logs/vigia_logins.lock**: `vigia_logins` abre fd 7 e tenta flock exclusivo não bloqueante (`bin/vigia_logins.sh:71-75`); o selo mantém a mesma trava ocupada no fixture (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:58`). A trava cobre as leituras decisórias e o revert/deploy (`bin/vigia_logins.sh:76-87`).

- **“deploy: OK”**: escritores preservam prefixo nos dois modos (`bin/deploy.sh:401-403`); leitores usam apenas `^deploy: OK`, portanto compatíveis com nova redação (`bin/molde_fatia/cadeia.sh:89-90`, `bin/molde_autocura/cadeia.sh:86-87`). Ambos têm `set -e`, além da conferência textual (`bin/molde_fatia/cadeia.sh:4`, `bin/molde_autocura/cadeia.sh:3`).

- **rc em `_subir`**: `_rodar` retorna o `subprocess.run(...).returncode` real (`app/core/integrador_lote.py:242-248`); novo ramo lê o rc, grava `veredito`/`deploy_rc` e retorna antes de FIM/NO_AR (`app/core/integrador_lote.py:753-766`). `_fechar` persiste o registro em LOTES e no log (`app/core/integrador_lote.py:825-829`). Entretanto, `main rodar` sempre retorna `0`, portanto o shell ainda recebe sucesso mesmo com veredito `deploy falhou` (`app/core/integrador_lote.py:856-858`, `bin/integrador.sh:52`).

Falhas a destacar:

1. **Consumidor não migrado: reload da cópia do integrador ignora rc** e entra na suite imediatamente, mesmo quando a nova guarda recusou reload (`app/core/integrador_lote.py:638-640`).

2. **Consumidor não migrado: `esteira_vigia.auto_revert` ignora rc dos dois deploys**, inicial e revert do revert (`app/core/esteira_vigia.py:1267-1268`, `app/core/esteira_vigia.py:1275`). Pode concluir “a tela volta” porque testes passaram sem provar publicação (`app/core/esteira_vigia.py:1269-1273`, `app/core/esteira_vigia.py:1287-1291`).

3. **Alarme novo afirma estado de prod que o rc não prova.** Para qualquer rc não zero informa “PROD SEGUE no codigo de antes do lote” (`app/core/integrador_lote.py:754-760`), porém falhas podem acontecer depois de restart/HUP, na espera de workers, rotas ou selo (`bin/deploy.sh:298`, `bin/deploy.sh:315`, `bin/deploy.sh:328`, `bin/deploy.sh:374`, `bin/deploy.sh:381`). Estado efetivo nessas falhas: **[não verificado]**.

4. **Trava do vigia não estabelece Single Writer global.** Vigia usa trava própria e faz `git revert` (`bin/vigia_logins.sh:71-87`), enquanto a porta compartilhada da árvore é `/tmp/cadeia.lock` (`bin/trava.sh:57-70`), usada pelas cadeias (`bin/molde_fatia/cadeia.sh:11`, `bin/molde_autocura/cadeia.sh:10`). `auto_revert` também reverte sem adquirir essa porta dentro da função (`app/core/esteira_vigia.py:1242-1291`). Sobreposição real em prod: **[não verificado]**.

## 04/11 12:22 -- HISTORICO (mais de 24 h) · sessao 00bdd655fe03 · - · - · origem=Ronald (tmux)

1. **Existe HUP sem `recusa_restart_com_cron`, por desenho:** normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` e `--reload-agendado` convergem no único `docker kill -s HUP`; a recusa só é chamada para `RESTART=1` (`bin/deploy.sh:112-114`, `bin/deploy.sh:295-315`). O único `docker restart` está imediatamente após a segunda recusa (`bin/deploy.sh:297-298`).

   **`--so-conferir` não existe no parser.** Só `--conferir` liga `SO_CONFERIR`; `--so-conferir` atravessa o fluxo normal, podendo migrar e emitir HUP (`bin/deploy.sh:56-62`, `bin/deploy.sh:167`, `bin/deploy.sh:171-174`, `bin/deploy.sh:315`). Não encontrei um modo válido silenciosamente esquecendo reload: as saídas anteriores são recusas e o modo de conferência (`bin/deploy.sh:89`, `bin/deploy.sh:147-174`, `bin/deploy.sh:213-235`, `bin/deploy.sh:267-273`).

2. **No deploy não há retry de sinal nem fallback restart:** HUP falho chama `falhou`; o laço apenas observa PIDs e também termina em falha (`bin/deploy.sh:315-328`). O único restart explícito é `bin/deploy.sh:298`; os outros comandos Docker de partida localizados em scripts operacionais são para os bancos de teste/sombra (`bin/db_teste.sh:70`, `bin/sombra_db.sh:100`). Não há operação compose no fluxo de deploy; só comentários indicando o ato separado de infraestrutura (`bin/deploy.sh:20-22`, `bin/deploy.sh:65-66`).

   **Limitação Single Writer:** as duas recusas não excluem um cron que comece entre checagem e restart; o envelope cria a marca sem trava compartilhada, e a segunda consulta é seguida pelo restart (`bin/cron_run.sh:9-11`, `bin/deploy.sh:105-110`, `bin/deploy.sh:297-298`). Há passagem pela recusa, mas não exclusão mútua com o início dos crons nesses pontos.

   **Prova de workers permite vazio:** PIDs anteriores vazios só avisam e pulam; consulta atual vazia satisfaz “nenhum antigo restou” e aprova (`bin/deploy.sh:318`, `bin/deploy.sh:321-324`). A prova de rota executa um processo Python novo por `docker exec`, sem consultar o worker HTTP (`bin/deploy.sh:353-356`).

3. **Guarda mensageria:** qualquer rc não zero, incluindo container indisponível e rc diferente de 1, define `PEND_MSG=1`; bloqueia antes da migration do core e do reload em todos os modos, salvo `--conferir` e `--restart` (`bin/deploy.sh:141-150`, `bin/deploy.sh:167`). Isso inclui agendado e cópia, pois os dois mantêm `SO_CONFERIR=0`, `RESTART=0` (`bin/deploy.sh:57-79`). Conferência pode sair zero mesmo sem resposta do mensageria, apenas imprimindo o estado (`bin/deploy.sh:146-147`, `bin/deploy.sh:171-174`).

   Recusar às 03:30 com migration pendente é coerente com o desenho registrado: o cron declara que não deve publicar fatia inacabada, o HUP publica a árvore viva, e a migration mensageria está no comando de boot (`app/config/crons.py:303-308`, `bin/deploy.sh:187-189`, `docker-compose.mensageria.yml:20`). O custo é deixar a higiene de memória sem ocorrer até correção; o próprio propósito desse cron é pagar o vazamento retirado da reciclagem por contagem (`app/config/crons.py:300-306`). Ocorrência real em produção: [não verificado].

**Nota para consolidação do item 4:** rc não zero não prova “PROD SEGUE no código de antes”: erros podem acontecer depois de emitir HUP, por rota ou selo (`bin/deploy.sh:315`, `bin/deploy.sh:374`, `bin/deploy.sh:381`); a mensagem do integrador é forte demais nesse aspecto (`app/core/integrador_lote.py:757-760`).

## 04/11 11:33 -- HISTORICO (mais de 24 h) · sessao c5de69a10b63 · DEPLOY-[nome]-ELO · verificador · origem=Code

1. **Veredito: bloqueia. As curas da marca e da pauta são parciais.** Há caminho para restart com cliente vivo e caminho para pauta aberta sem fechamento automático (`bin/em_casca.sh:28-38`; `bin/admissao_cron.sh:202-208`; `bin/deploy.sh:368-375`). Execução da suíte, morte efetiva dos mutantes e ocorrência desses caminhos em produção: **[nao verificado]**.

   Para cada PROCEDENTE da seção `codex_v2.r`:

   | Item | Resultado em e8f72370 | Gravidade |
   |---|---|---|
   | SIGKILL com exec vivo | Cura quando a transferência **já terminou**. Continua descoberto entre lançar o filho e publicar sua marca, ou quando `sed/mv` falha. A transferência ainda introduz a corrida de varredura descrita no item 2 (`bin/em_casca.sh:28-38`). | **bloqueia** |
   | Pauta perdida; resolver falhando | Resolver não zero conserva a recusa. Entretanto, pauta pode nascer sem recusa ou depois de sua remoção; nesses casos `_tira_recusa` não a alcança (`bin/deploy.sh:368-375`; `app/core/integrador_lote.py:773-780`). | **bloqueia** |
   | CLI por AST aceitava `!=` | O caso agora executa a porta positiva e rejeita a inversão direta dessa condição. Falta testar outra porta; uma condição excessivamente abrangente ainda passa (`bin/tests/test_deploy_nao_mata_elo.sh:930-934`; `app/core/esteira_vigia.py:1652`). | Curado no alvo específico; lacuna em **fila** |
   | Falha da pauta do lote | O cenário injeta 70 e exige aviso no relato; a produção trata qualquer retorno verdadeiro de `_pauta` (`bin/tests/test_deploy_nao_mata_elo.sh:789-792`; `app/core/integrador_lote.py:780-781`). | Curado por leitura; execução **[nao verificado]** |
   | Justificativa da espera por env | A correção procede: `ADM_ESPERA_ADMISSAO_S` pode alongar a espera, pois não há limite superior. Isso não altera o teto declarado do restart (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:62-68`, `bin/admissao_cron.sh:243`). | **fila** |

   A regressão mais direta é o segundo escritor de marcas, fora da trava usada pela porta original: publicar a marca nova antes de remover a velha **não garante** que uma varredura veja alguma delas (`bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:202-203`).

2. **Caminhos de `bin/em_casca.sh`.**

   | Caminho | Marca, processo e retorno | Gravidade |
   |---|---|---|
   | Filho termina antes da leitura do starttime | Se `/proc` já sumiu, não transfere; `wait` recupera o resultado e EXIT normalmente remove a marca do wrapper. Não resta comando vivo nesse caminho normal (`bin/admissao_cron.sh:84-91`; `bin/em_casca.sh:23`, `bin/em_casca.sh:32`, `bin/em_casca.sh:41-42`). | Sem bloqueio nesse caminho |
   | `sed` ou `mv` falha | O comando **já começou**; permanece somente a marca do wrapper. Enquanto ele vive, protege. Se morrer antes do filho, o reaper remove essa marca pelo PID morto do wrapper, deixando o cliente vivo sem proteção (`bin/em_casca.sh:28`, `bin/em_casca.sh:34-38`; `bin/admissao_cron.sh:169-175`, `bin/admissao_cron.sh:208`). | **bloqueia** |
   | SIGTERM no wrapper | Há somente trap EXIT. Após transferência, preserva a marca do filho vivo; antes dela ou no caminho de falha, preserva um arquivo cuja identidade ainda é a do wrapper morto. O retorno observado é o término do wrapper, não o resultado futuro do comando (`bin/em_casca.sh:23`, `bin/em_casca.sh:28-38`). | **bloqueia** antes da transferência; retorno do comando **[nao verificado]** após o sinal |
   | SIGKILL no wrapper | Depois da transferência, a marca continua identificando o filho. Antes dela, ou após falha de transferência, a marca identifica o wrapper morto e pode ser recolhida enquanto o cliente vive (`bin/em_casca.sh:28-38`; `bin/admissao_cron.sh:169-177`). | **bloqueia** |
   | Filho acaba após SIGKILL do wrapper | Não há wrapper para desmarcar: fica arquivo órfão até a próxima varredura, que verifica identidade e o remove (`bin/em_casca.sh:23`, `bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:199-208`). | **fila**, comportamento de reaper |
   | PID do filho reutilizado | EXIT usa apenas `kill -0`, podendo conservar arquivo por causa de outro processo. O reaper compara starttime; se a reutilização anteceder a captura, pode registrar a identidade substituta. `exe` é substring da cmdline, não identidade do executável (`bin/em_casca.sh:23`, `bin/em_casca.sh:32-34`; `bin/admissao_cron.sh:175-177`). | **fila** |
   | rc 64 | Uso/nome inválido sai antes de marcar. Alvo fora das cascas marca primeiro, mas sai antes de lançar o comando e desmarca pelo EXIT (`bin/em_casca.sh:12-13`, `bin/em_casca.sh:21-28`). | Sem execução escondida |
   | Admissão 75/70 | Retorna antes de lançar o comando. 75 vem da espera pela trava; 70, das falhas de preparação/publicação da marca (`bin/em_casca.sh:16-19`; `bin/admissao_cron.sh:241-248`). | Retornos previstos |
   | Execução normal | O resultado de `wait` é devolvido; o resultado da transferência não o substitui (`bin/em_casca.sh:41-42`). | Sem troca de rc nesse caminho |

   **A corrida existe mesmo sem sinal:** a varredura final expande o glob contendo apenas a marca velha; o wrapper publica a nova e remove a velha; o teste `-e` pula a velha desaparecida; a nova não pertence à enumeração. A varredura retorna vazio e o restart começa com o cliente vivo (`bin/admissao_cron.sh:202-203`; `bin/em_casca.sh:35-36`; `bin/reinicio_sob_admissao.sh:43-45`, `bin/reinicio_sob_admissao.sh:66`). **Bloqueia.**

   **Sim, a marca nova também pode ser publicada depois da verificação vazia:** se o wrapper morrer após lançar o processo externo `mv`, mas antes de ele publicar, a varredura pode recolher a marca do wrapper morto e concluir vazio; o `mv` remanescente publica sem consultar fd8 (`bin/em_casca.sh:35`; `bin/admissao_cron.sh:170`, `bin/admissao_cron.sh:208`; `bin/reinicio_sob_admissao.sh:44-45`). É uma interleaving deduzida do código; reprodução e ocorrência em prod **[nao verificado]**.

3. **Quem escreve, lê, remove e trata os retornos.**

   **Marcas `logs/crons_em_curso/*`:**

   | Papel | Implementação |
   |---|---|
   | Escritor original | `adm_marcar` publica `${nome}_${emp}.$$`, com `ini`, `pid`, `starttime`, `exe`, `casca` e `cmd`, usando temporário e `mv` sob fd8 (`bin/admissao_cron.sh:239-250`). |
   | Chamadores da porta | Comando e alarme do cron; descoberta da corrente; `em_casca` (`bin/cron_run.sh:16`, `bin/cron_run.sh:52`; `bin/encadeado.sh:48`; `bin/em_casca.sh:16`). |
   | Segundo escritor | `em_casca` publica `${N}_-.${filho}`, altera pid/starttime/exe, conserva os demais campos e remove a marca anterior, sem retomar fd8 (`bin/em_casca.sh:33-36`). |
   | Leitor da identidade | `adm_marca_viva` usa o PID **do nome do arquivo**; não usa `pid=`. Compara starttime e procura `exe` na cmdline (`bin/admissao_cron.sh:169-177`). |
   | Leitor da proteção | `adm_segura` lê `casca`; formato antigo segura; `casca=0` ainda consulta descendentes (`bin/admissao_cron.sh:181-186`). |
   | Varredor/reaper | `adm_varrer` verifica identidade, desconsidera ancestrais como bloqueadores e remove órfãs (`bin/admissao_cron.sh:199-208`). |
   | Consumidor do restart | `adm_seguram` filtra a varredura; o restart consulta antes e depois de tomar fd8 (`bin/admissao_cron.sh:213`; `bin/reinicio_sob_admissao.sh:40-45`). |
   | Remoção explícita | `adm_desmarcar`, chamado pelos envelopes e pelo EXIT (`bin/admissao_cron.sh:254`; `bin/cron_run.sh:22`, `bin/cron_run.sh:51`, `bin/cron_run.sh:58`; `bin/encadeado.sh:50`; `bin/em_casca.sh:23`). |

   O contrato verifica a presença textual dessa infraestrutura; os casos do selo também leem as marcas nas raízes falsas (`bin/contrato_deploy.sh:55-57`; `bin/tests/test_deploy_nao_mata_elo.sh:635`, `bin/tests/test_deploy_nao_mata_elo.sh:675`).

   **Recusas e pautas:**

   | Estado | Cria/abre | Lê | Apaga/fecha |
   |---|---|---|---|
   | `recusa_cron.<id>.pendente` | Deploy, somente no ramo restart75, com ID e argv separados por NUL (`bin/deploy.sh:284-287`). | Agendado valida ID e lê argv; retentar lê `auto_revert`; contador conta nomes; limpeza lê ID/mtime (`bin/deploy_agendado.sh:83-87`, `bin/deploy_agendado.sh:101`; `app/core/esteira_vigia.py:1344-1354`; `bin/deploy_recusado.sh:22-27`; `bin/deploy.sh:369`, `bin/deploy.sh:382`). | Deploy por `_tira_recusa`; **também agendado**, preparando IDs e substituindo a recusa anterior (`bin/deploy.sh:375-382`; `bin/deploy_agendado.sh:95`, `bin/deploy_agendado.sh:104`, `bin/deploy_agendado.sh:110`). |
   | `deploy_recusado:integrador` | `_deploy_do_lote` (`app/core/integrador_lote.py:780`). | `pauta_viva` (`app/core/management/commands/alarme_esteira.py:13-15`). | `_tira_recusa` chama o resolver (`bin/deploy.sh:370-375`). |
   | `deploy_recusado:auto_revert` | Primeiro deploy e retentativa (`app/core/esteira_vigia.py:1331`, `app/core/esteira_vigia.py:1356`). | Mesmo leitor (`app/core/management/commands/alarme_esteira.py:13-15`). | Mesmo resolvedor (`bin/deploy.sh:370-375`). |
   | `deploy_recusado:vigia_logins` | Falha do deploy do revert (`bin/vigia_logins.sh:91-95`). | Mesmo leitor (`app/core/management/commands/alarme_esteira.py:13-15`). | Mesmo resolvedor (`bin/deploy.sh:370-375`). |

   A escrita da pauta continua pela porta `escrever`; o fechamento passa por `marcar_feita_pelo_sistema`. Não surgiu escrita direta de `feito_em` nesse trecho (`app/core/management/commands/alarme_esteira.py:27-31`, `app/core/management/commands/alarme_esteira.py:54-56`; `app/pautas/services.py:269-275`).

   **Os três IDs que abrem pauta podem ficar sem fechamento automático**, por dois caminhos:

   - Todos abrem para rc diferente de zero, mas somente restart75 produz recusa. Falha sem arquivo deixa `_tira_recusa` sem entrada para resolver (`app/core/integrador_lote.py:773-780`; `app/core/esteira_vigia.py:1326-1331`; `bin/vigia_logins.sh:91-95`; `bin/deploy.sh:284-290`, `bin/deploy.sh:368`). **Bloqueia.**
   - Outro deploy pode resolver antes de o chamador abrir a pauta e apagar a recusa. Resolver pauta inexistente retorna normalmente; depois o chamador abre a pauta sem arquivo restante (`app/core/management/commands/alarme_esteira.py:28-33`; `bin/deploy.sh:370-375`; `app/core/esteira_vigia.py:1325-1331`). **Bloqueia.**

   `manual` e `agendado.*` também produzem recusas, mas os abridores acima usam os três IDs fixos; não há abertura correspondente a cada ID do agendado (`bin/deploy.sh:52-54`; `bin/deploy_agendado.sh:94-105`; `app/core/integrador_lote.py:780`; `bin/vigia_logins.sh:95`).

   **Retorno de `deploy.sh` nos chamadores:** [nao verificado]

   | Chamador | Tratamento |
   |---|---|
   | Integrador: reload inicial | Descarta rc e segue para a suíte (`app/core/integrador_lote.py:638-639`). **Fila.** |
   | Integrador: publicação final | Lê rc; falha impede FIM e retirada da fila (`app/core/integrador_lote.py:748-752`, `app/core/integrador_lote.py:771-782`). |
   | Auto-revert inicial | Lê rc e interrompe antes da conferência em falha (`app/core/esteira_vigia.py:1276-1284`). |
   | Revert do revert | Lê rc e tenta incluí-lo na pauta `auto_revert`; pauta já viva não é reescrita (`app/core/esteira_vigia.py:1289-1294`; `app/core/management/commands/alarme_esteira.py:38-41`). **Fila.** |
   | Retentar | Lê rc; CLI retorna 0 e vigia usa `tail`, sem decidir pelo status (`app/core/esteira_vigia.py:1354-1362`, `app/core/esteira_vigia.py:1652-1654`; `bin/vigia_arvore.sh:28`). |
   | Vigia de logins | Lê rc e abre pauta em falha; o próprio vigia termina em 2 nesse caminho de alarme (`bin/vigia_logins.sh:90-96`, `bin/vigia_logins.sh:105`). |
   | Agendado | Captura rc da esteira e da repetição; repetição bem-sucedida vira 3 pela cauda não executada; devolve `PIOR` (`bin/deploy_agendado.sh:96-106`, `bin/deploy_agendado.sh:123-135`). |
   | Dois moldes de cadeia | Reload inicial guardado; publicação final sujeita a `set -e`; rollback ignora rc sob `set +e` (`bin/molde_fatia/cadeia.sh:4`, `bin/molde_fatia/cadeia.sh:26-33`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`; `bin/molde_autocura/cadeia.sh:3`, `bin/molde_autocura/cadeia.sh:25-32`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`). Rollbacks: **fila**. |
   | `o135-teto20` | Conferência guardada; publicação captura e propaga rc (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:116-124`). |
   | `abono-no-ar` | `pipefail` e condicional convertem falha do deploy em 1 (`fatias_agendadas/abono-no-ar/esteira.sh:21`, `fatias_agendadas/abono-no-ar/esteira.sh:37-38`). |
   | Reload diário RAW | Passa pelo envelope que registra e devolve rc (`app/config/crons.py:307`; `bin/cron_run.sh:24`, `bin/cron_run.sh:29`, `bin/cron_run.sh:70`). |

   **RC do resolver:** qualquer não zero é reduzido a 1 em `_tira_recusa`, conservando arquivo. Esse 1 é ignorado pelos chamadores internos, e o deploy termina em `echo`, devolvendo 0; portanto rc0 não prova pauta fechada nem recusa removida (`bin/deploy.sh:370-385`). **Fila**, pois o restart pode ter passado e a pendência continua visível.

4. **Os casos mordem alguns erros, mas deixam estas mutações plausíveis passar.** Sobrevivência experimental dos mutantes: **[nao verificado]**; abaixo, dedução por leitura.

   | Caso | Mordida e mutação sobrevivente |
   |---|---|
   | 19(a) | Cobra marca protetora, recusa75 e zero restart. Remover a transferência ainda passa esse caso isolado: o wrapper continua vivo; o laço não exige que encontrou PID diferente (`bin/tests/test_deploy_nao_mata_elo.sh:632-640`). |
   | 19(f) | Cobra SIGKILL **depois** de esperar a transferência. Trocar EXIT por `trap adm_desmarcar EXIT` passa (a)/(f), mas quebra TERM com filho vivo; SIGKILL não executa EXIT (`bin/tests/test_deploy_nao_mata_elo.sh:670-676`; `bin/em_casca.sh:23`). |
   | 21 limpeza | Executa o trecho real e verifica os resolvedores. Apagar a criação real do marcador passa: o teste fabrica `.restart_inicio.$$`; a extração do caso8 também começa depois da criação (`bin/tests/test_deploy_nao_mata_elo.sh:233`, `bin/tests/test_deploy_nao_mata_elo.sh:752-766`; `bin/deploy.sh:282`). |
   | 21 resolver | Injeta somente 0 e 1. Reter arquivo só para rc1, apagando para 70/75, passa esses cenários e perde a pendência em falha de admissão (`bin/tests/test_deploy_nao_mata_elo.sh:765-771`; `bin/deploy.sh:370-375`). |
   | 21 pauta70 | Trocar `if rp` por `if rp == 70` passa e omite aviso para 75; o teste também não exige o número correto no aviso (`app/core/integrador_lote.py:781`; `bin/tests/test_deploy_nao_mata_elo.sh:789-792`). |
   | 23 retentar | Trocar `if rc != 0` por `if rc == 75` passa os cenários 75/0 e anuncia sucesso para rc1 (`app/core/esteira_vigia.py:1355`; `bin/tests/test_deploy_nao_mata_elo.sh:907-917`). |
   | 23 CLI | Usar `if argv[1:2] != ['auto-revert']` aceita retentar e passa a sentinela, mas sequestra outras portas. Não há chamada negativa, apesar do comentário (`app/core/esteira_vigia.py:1652`; `bin/tests/test_deploy_nao_mata_elo.sh:925-934`). |

   As lacunas isoladas são **fila de mordida**; a falta de cobertura das janelas de transferência acompanha o **bloqueio** do item 2 (`bin/tests/test_deploy_nao_mata_elo.sh:670-671`; `bin/em_casca.sh:28-38`).

5. **Classes históricas no delta da volta 3:**

   | Classe | Resultado |
   |---|---|
   | (a) Segundo juiz registrado | Não identifiquei duplicação de juiz de domínio: o novo fechamento chama o comando existente, que usa as portas de pauta (`bin/deploy.sh:370-371`; `app/core/management/commands/alarme_esteira.py:27-31`). O registro vivo, e não a tabela de consulta, é a autoridade declarada (`app/docs/JUIZES.md:10-12`). |
   | (b) Segundo escritor | **Aparece:** transferência escreve marca fora de `adm_marcar` (`bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:239-250`). **Bloqueia.** |
   | (c) Relógio fora da borda | Não identifiquei leitura nova de relógio no trecho de transferência ou limpeza acrescentado (`bin/em_casca.sh:32-38`; `bin/deploy.sh:366-383`). |
   | (d) Derivação paralela | A lista fixa foi retirada; o fechamento deriva ID do arquivo. Essa parte melhora a origem (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1359`). |
   | (e) Selo sem mordida | Há mordida parcial e lacunas concretas, enumeradas no item 4 (`bin/tests/test_deploy_nao_mata_elo.sh:759`, `bin/tests/test_deploy_nao_mata_elo.sh:789-792`, `bin/tests/test_deploy_nao_mata_elo.sh:930-934`). **Fila**, além da prova insuficiente do caminho bloqueante. |
   | (f) Trava/fd | **Aparece:** troca fora de fd8 quebra a garantia da segunda varredura. O fechamento original do fd antes dos filhos continua correto (`bin/admissao_cron.sh:250`; `bin/em_casca.sh:33-36`; `bin/reinicio_sob_admissao.sh:43-45`). **Bloqueia.** |
   | (g) Pouso antes do pré-requisito | Pouso e atendimento dos pré-requisitos operacionais: **[nao verificado]**. O aval os exige antes de PRONTO (`AVAL_DEPLOY-ELO-B3.txt:14`, anexo). | [nao verificado]
   | (h) Fallback/env solta | **Aparece fallback:** falha da transferência mantém a marca do wrapper, insuficiente após sua morte (`bin/em_casca.sh:37-38`). **Bloqueia.** A env sem limite superior permanece na admissão (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:243`). **Fila.** |
   | (i) Erro engolido | Não há novo `try/except` nesse trecho; há equivalente em shell: erro de leitura pode virar ID vazio e remoção, e erro do resolver não altera o rc final do deploy (`bin/deploy.sh:369-385`). **Fila.** |
   | (j) Restart com exec vivo | **Aparece** pela troca de marca e permanece no caminho deploy × deploy (`bin/em_casca.sh:33-36`; `bin/reinicio_sob_admissao.sh:44-66`; `bin/deploy.sh:300`, `bin/deploy.sh:316`). **Bloqueia.** |

6. **Triagem para FILA: motivos sustentados e exceções.**

   - **Deploy × deploy: não sustenta como autorização de fechamento. Bloqueia.** A exclusão do vigia inteiro permite que ele lance o deploy automático sem marca permanente protetora; migrate/provas continuam fora da admissão. Podem começar depois da última varredura de outro restart. A existência anterior da corrida em outros chamadores não protege esse caminho, nem revoga a proibição escrita de reiniciar sobre exec vivo (`bin/admissao_cron.sh:36`, `bin/admissao_cron.sh:229-231`; `bin/vigia_arvore.sh:28`; `bin/deploy.sh:102`, `bin/deploy.sh:124`, `bin/deploy.sh:300`; `bin/reinicio_sob_admissao.sh:43-66`; `AVAL_DEPLOY-ELO-B3.txt:13`, anexo).

   - **SIGKILL não trapável: não sustenta a transferência incompleta. Bloqueia.** O problema não exige trapar KILL: exige que o comando lançado continue representado durante toda a janela. Hoje ele começa antes da transferência e há fallback para a identidade do wrapper (`bin/em_casca.sh:28-38`).

   - **Outros consumidores que ignoram `_pauta`: motivo de escopo sustenta fila.** `_pauta` anuncia falha em stderr, mas chamadas continuam sem tratar seu retorno; isso não prova persistência da pauta nesses usos (`app/core/esteira_vigia.py:900-905`, `app/core/esteira_vigia.py:1274`, `app/core/esteira_vigia.py:1291`, `app/core/esteira_vigia.py:1632-1637`). **Fila.**

   - **Revert do revert: fila, com justificativa imprecisa.** O rc pode ser engolido por pauta já viva. Também não há ID explícito: usa o ambiente do processo pai ou `manual`; o `env` local do primeiro deploy não altera esse ambiente (`app/core/esteira_vigia.py:1289-1294`, `app/core/esteira_vigia.py:1324-1325`; `bin/deploy.sh:52`; `app/core/management/commands/alarme_esteira.py:38-41`).

   - **Chave antiga sem sufixo: fila; existência em prod [nao verificado].** O fechamento atual deriva `deploy_recusado:<id>` do arquivo, e os abridores atuais usam sufixos (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1331`; `app/core/integrador_lote.py:780`; `bin/vigia_logins.sh:95`).

   - **Lacunas do caso8, marcador fabricado, segundo deploy só75 e universo derivado do classificador: fila de mordida sustentada.** Os testes cobram existência da recusa, fabricam o marcador, injetam75 no segundo deploy e derivam o universo da própria autoridade (`bin/tests/test_deploy_nao_mata_elo.sh:208`, `bin/tests/test_deploy_nao_mata_elo.sh:759`, `bin/tests/test_

[truncado em 20000 de 24144 caracteres]

_1533 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
