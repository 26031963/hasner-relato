# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 08:41** · publicado em **09/11/2026 08:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 08:41 · sessao 880e3d56c067 · FIO-E0-V4-E3 · construtor · origem=Code

| Hunk | Alteração / escrita e leitura |
|---|---|
| [core/juizes.py:759](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/core/juizes.py:759) | Baixa pontual do dashboard; registro lido pelo selo em `core/tests/test_contract_juiz_tela.py:34`. |
| [chamados/models.py:1795](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/models.py:1795) | Método removido, lápide no sítio. Guarda contra retorno em `chamados/tests/test_e3_pendentes_app.py:285`. |
| [tarefa_do_dia.py:363](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/tarefa_do_dia.py:363) | Carga escreve `_verbos`, `_fila`, `_fila_cel`; tarefa lê em `:411` e `:422`. Fontes chamadas: `particionar_verbos(qs_vivos())['mapa']` e `qs_na_fila()`, também lidas por C17 em `chamados/juizes.py:1447` e `:1450`. |
| [tarefa_do_dia.py:418](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/tarefa_do_dia.py:418) | Escreve campos aditivos `rota/fila/verbos/fio`; leitores novos: DIFF `:120` e testes `test_e3_tarefa_portas.py:96`. | [nao verificado]
| [verbos.py:30](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/verbos.py:30) | Recebe `agora` da tarefa em `tarefa_do_dia.py:367`; conserva valor padrão dos leitores existentes, incluindo `painel.py:230` e `juizes.py:1447`. |
| [test_e3_pendentes_app.py:99](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_pendentes_app.py:99) | M1–M5 com fixtures ORM, relógio congelado e views completas; M6/M7 mantidos em `:306` e `:321`. |
| [test_esmeril2_left_join.py:19](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_esmeril2_left_join.py:19) | Migra para `consulta_do_app`: preserva ausência de fantasma e exige duas perguntas → duas pendências. |
| [test_fio_le_o_juiz.py:263](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_fio_le_o_juiz.py:263) | Migra leitura para `consulta_do_app`; preserva desaparecimento da pergunta absolvida. |
| [test_dashboard_badge.py:37](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/core/tests/test_dashboard_badge.py:37) | Migra comparação da tela para `consulta_do_app`. Seus leitores de produção continuam em `api/views.py:325`, `api/views_core.py:314`, `core/views.py:253` e `api/views.py:2562`. |
| `test_badge_disputa_pendente.py` — excluído | Sustentava unidade por disputa; substituição por pergunta em `test_e3_pendentes_app.py:176`. |
| [test_e3_tarefa_portas.py:1](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_tarefa_portas.py:1) | Quatro respostas escritas antes do aditivo; fixtures escrevem fatos, testes leem a tarefa. | [nao verificado]
| [diff_tarefa_portas.py:23](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/management/commands/diff_tarefa_portas.py:23) | Barreira SQL; comparação lê `_porta` e tarefa em `:118`. Escreve somente saída agregada em `:132`. |
| [test_e3_diff_portas.py:49](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_diff_portas.py:49) | Selo injeta escrita indireta e exige aborto antes do driver; identidade antes da raspagem em `:72`. |
| [e3_mutantes.py:15](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/e3_mutantes.py:15) | Catálogo de mutações; `aplicar_mutante`, em `:53`, remenda funções somente em memória e restaura após cada teste. | [nao verificado]

**DECISAO-ABERTA:** a interpretação “sem ações ⇒ sem porta” está explícita em `test_e3_tarefa_portas.py:10`. Medição na sombra por L-099/SONDA-SEM-GRANT e efeito na folha: **[nao verificado]**. C17 conserva os três chamadores em `fora_da_folha.py:67`, `vigia_fio.py:635` e `cadastro_realidade.py:488`.

Comandos preparados, **não executados**; RED, mortes dos mutantes e resultados da suíte: **[nao verificado]**. [nao verificado]

```bash
t() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    --settings=config.settings.ci "$@"
}
SELOS="chamados.tests.test_e3_pendentes_app chamados.tests.test_e3_tarefa_portas chamados.tests.test_e3_diff_portas"

t $SELOS core.tests.test_contract_juiz_tela \
  chamados.tests.test_esmeril2_left_join \
  chamados.tests.test_fio_le_o_juiz core.tests.test_dashboard_badge

# RED: retirar o aditivo.
E3_MUTANTE=sem_aditivo t chamados.tests.test_e3_tarefa_portas

# Um comando por mutante catalogado; esperado RED.
for m in $(PYTHONDONTWRITEBYTECODE=1 python3 -c \
  'from chamados.tests.e3_mutantes import MUTANTES; print(*MUTANTES, "legado_publico", "registro_dashboard", "quarto_c17_alias")')
do
  E3_MUTANTE="$m" t $SELOS
done

# DIFF: NÃO RODADO; universo a fornecer pela sombra.
PYTHONDONTWRITEBYTECODE=1 python3 manage.py diff_tarefa_portas \
  --colaborador ID --inicio AAAA-MM-DD --fim AAAA-MM-DD
```

Saída observada da compilação e auditoria estática dos arquivos acima:

```text
py_compile: OK nos 12 .py presentes; saída interceptada em memória.
Arquivos de bytecode escritos: 0.
Mutantes: 18 alvos textuais únicos, sintaxe compilável; sem execução.
C17: 3 chamadores diretos por AST.
Legado removido: 0 chamadas de produção por AST.
git diff --check: sem saída.
```

## 09/11 08:34 · sessao b4de1741aa55 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**PROCEDENTE: a cura do par reconhecido está estruturada, mas identidade, leitores da espera e rollback do O330 ainda impedem afirmar o contrato universal.** `app/chamados/services/materializacao.py:84`, `app/chamados/services/explicador.py:97`, `app/chamados/services/validacao.py:190`.

1. **ATO ÚNICO — NAO PROCEDE para o par identificado; a universalidade não está atendida.**

   O par reconhecido entra no mesmo `atomic`, põe ambas em voo, desliga a guarda individual e julga paridade/lastro depois das duas chamadas. Carimbos e reconciliação vêm depois. Toda batida nova desse ramo passa pela porta; reuso pode dispensar `registrar_batida`. `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:1335`, `app/chamados/services/materializacao.py:1340`, `app/chamados/services/materializacao.py:1347`, `app/chamados/services/materializacao.py:919`, `app/chamados/services/materializacao.py:987`.

   **Ainda existe aplicação individual de intervalo:** `sem_par`, `par_ambiguo` e `marcos_sem_par` caem no avulso. A irmã já aplicada é explicitamente cobrada como controle desse comportamento. Não equivale a garantir ato conjunto para todo motivo de intervalo. `app/chamados/services/materializacao.py:77`, `app/chamados/services/materializacao.py:1400`, `app/chamados/services/materializacao.py:1447`, `app/chamados/tests/test_bug_par_intervalo.py:444`.

   Chamadores encontrados por AST:

   | Função chamada | Chamadores; apelido |
   |---|---|
   | `_materializar_uma` | `validar_pergunta`, import sem alias: `app/chamados/services/validacao.py:466`, chamadas `:469`, `:474`; varredura, nome local: `app/chamados/services/materializacao.py:1565`; `_plantar_o_dia`, import sem alias: `app/chamados/services/passivo_validado.py:39`, chamada `:529`. |
   | `_materializar_resposta_hora` | `_materializar_par` e `_materializar_avulsa`, nomes locais: `app/chamados/services/materializacao.py:1336`, `app/chamados/services/materializacao.py:1447`; reexport sem chamada: `app/chamados/models.py:4077`. |
   | `registrar_batida`, neste materializador | Import sem alias; ramo hora e ramo data/hora: `app/chamados/services/materializacao.py:986`, `app/chamados/services/materializacao.py:987`, `app/chamados/services/materializacao.py:1068`. |

   **FILA — outros atos de preenchimento de intervalo também admitem resultado parcial:** `veredito_celula` captura falha por escrita e só desfaz quando nenhuma ação deu certo; regularização externa continua após uma criação recusada. São decisões do dia/regularização externa, com unidades próprias, não o aceite destas duas `PerguntaDisputa`. Cura: auditar atomicidade dessas unidades separadamente; não incluí-las numa promessa universal já cumprida. `app/ponto/services/veredito_celula.py:320`, `app/ponto/services/veredito_celula.py:327`, `app/ponto/services/veredito_celula.py:342`, `app/chamados/services/regularizacao_ext.py:172`, `app/chamados/services/regularizacao_ext.py:176`, `app/chamados/services/regularizacao_ext.py:197`.

2. **TRAVAS — NAO PROCEDE a inversão entre os dois validadores descritos.**

   `_travar_ato_materializacao` trava primeiro Colaborador; depois entrega o universo à porta que adquire Disputas → Chamados → Perguntas, cada tabela por PK. Dois validadores do mesmo colaborador serializam antes de alcançar qualquer metade. `app/chamados/services/materializacao.py:95`, `app/chamados/services/materializacao.py:104`, `app/chamados/models.py:2674`, `app/chamados/models.py:2683`, `app/chamados/models.py:2696`, `app/chamados/models.py:2700`.

   Seus quatro sítios de chamada estão dentro de transação: validação, envelope comum, competência O330 e dia O330; todos usam import sem alias ou nome local. `app/chamados/services/validacao.py:428`, `app/chamados/services/validacao.py:434`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/materializacao.py:1379`, `app/chamados/services/passivo_validado.py:371`, `app/chamados/services/passivo_validado.py:432`, `app/chamados/services/passivo_validado.py:519`, `app/chamados/services/passivo_validado.py:520`.

   **FILA — prova concorrente:** R3 executa as validações sequencialmente; não exercita duas conexões disputando locks. Deadlock executado ou ausência dele: **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:174`.

3. **ESTADO — PROCEDENTE: a espera ganhou significado que vários leitores não conhecem.**

   A primeira metade fica `VALIDADA`, sem `materializada_em`, morta para C6 e fora de `respostas_sem_veredito`. A porta nova preserva essa validação e retorna `aguardando_par`. `app/chamados/models.py:3041`, `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851`, `app/chamados/services/validacao.py:192`, `app/chamados/services/validacao.py:482`.

   | Leitor | Consequência |
   |---|---|
   | Explicador/modal | Diagnostica `validada_sem_gravar`, manda corrigir escala ou TI/DP e anuncia “o sistema não conseguiu gravar”, mesmo com espera legítima. **PROCEDENTE.** `app/chamados/services/explicador.py:97`, `app/templates/chamados/partials/modal_fio.html:449`. |
   | Declaração/admin/app | Considera validação sem registro uma falha e manda refazer o horário; a projeção da API reutiliza essa família de leitores. **PROCEDENTE.** `app/chamados/services/declaracao_texto.py:221`, `app/chamados/services/declaracao_texto.py:406`, `app/chamados/services/declaracao_texto.py:677`, `app/api/views.py:1119`. |
   | Vigia do carimbo/O330 | Inclui a espera em `sem_plantio_nenhum` e `resposta_enviada_sem_gravacao`, “esperado 0”; o reparador seleciona esse universo. **PROCEDENTE.** `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/vigia_carimbo.py:116`, `app/chamados/services/vigia_carimbo.py:140`, `app/chamados/services/passivo_validado.py:324`. |
   | Cron desse vigia | O padrão não alarma esse passivo: usa apenas `--alarme`; o contador entra com `--alarme-passivo`. **NAO PROCEDE** alarme automático padrão por essa causa. `app/config/crons.py:1751`, `app/chamados/management/commands/vigiar_carimbo_sem_registro.py:73`. |
   | Placar | A metade sai do crédito de perguntas vivas; conta como validação humana, sem exigir aplicação. **NAO PROCEDE** chamar essa contagem de materialização. `app/chamados/services/raiox.py:72`, `app/chamados/services/verbos.py:34`, `app/chamados/services/painel.py:268`. |
   | Auditor I18 | Não a acusa como sem dono, pois exige `recusa_motivo=''`. **NAO PROCEDE** essa falsa orfandade específica. `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`. |

   **PROCEDENTE — fechamento contingente à irmã:** a espera não bloqueia C7 por si. Se a irmã sair do conjunto vivo/sem-veredito, `pode_fechar()` pode autorizar fechamento com a primeira ainda sem aplicação; há fechadores automáticos que consomem esse resultado. Cura: representar a pendência do ato no juiz de fechamento e testar retirada da irmã entre aceites. `app/chamados/models.py:1851`, `app/chamados/models.py:1861`, `app/chamados/services/becos.py:56`.

   `vigia_fio` incorpora classes do vigia do carimbo; `o_que_segura` delega à porta alcançável. Reabertura ou impacto quantitativo dessas esperas em produção: **[nao verificado]**. `app/chamados/services/vigia_fio.py:463`, `app/chamados/services/vigia_fio.py:485`, `app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:221`.

   **Cura comum:** os leitores acima devem consumir um estado canônico de espera antes de diagnosticar falha, preservando separadamente a dívida real de gravação. `app/chamados/services/materializacao.py:496`.

4. **IDENTIDADE — PROCEDENTE: contar uma saída e uma volta não prova a mesma pausa.**

   O identificador consulta disputa/data/motivo e compara o mapa global, que hoje fornece sempre `S/hii` e `E/hfi`. Não consulta os slots reais do dia. Assim, uma saída remanescente da primeira pausa e uma volta da segunda passam pela identificação. As duas pausas compartilham os mesmos motivos. Cura mínima: abster quando houver múltiplas pausas reais sem identidade suficiente; solução completa: vincular os slots pela autoridade existente de marcos. `app/chamados/services/materializacao.py:70`, `app/chamados/services/materializacao.py:84`, `app/chamados/juizes.py:885`, `app/ponto/marcos.py:35`.

   `MARCO_DA_PERGUNTA`, `PARES_PAUSA_DNA`, `instancia_do_motivo` e a leitura das lâmpadas respondem perguntas diferentes; não são outro agrupador equivalente de perguntas. Não precisam chamar o agrupador novo. A divergência é ele afirmar identidade sem consumir a distinção real entre pausas. `app/chamados/juizes.py:882`, `app/escala/models.py:1652`, `app/chamados/services/declaracao_texto.py:42`, `app/chamados/juizes.py:917`.

   **PROCEDENTE — prova insuficiente de preservação de HEAD:** R7 conserva escala de uma pausa e cria quatro perguntas; prova ambiguidade por quantidade, não duas pausas no DNA nem o casal cruzado remanescente. Para quatro elegíveis há fallback avulso; preservação universal com duas pausas reais: **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:45`, `app/chamados/tests/test_bug_par_intervalo.py:255`, `app/chamados/services/materializacao.py:77`, `app/chamados/services/materializacao.py:1400`.

5. **TEXTO — parcialmente atendido.**

   **NAO PROCEDE** espera por substring: existe chave exata, reconhecida antes das regras textuais, mantendo retorno triplo. Os consumidores desempacotam esse contrato sem enumeração fechada de códigos. `app/chamados/catalogo/recusa.py:25`, `app/chamados/catalogo/recusa.py:95`, `app/chamados/services/acoes_disputa.py:34`, `app/chamados/services/fio.py:703`, `app/chamados/services/validacao.py:548`.

   **NAO PROCEDE** “colada” por vizinha distante no novo ramo: ele exige mesmo colaborador/tipo, não retratada, dentro de ±60 segundos inclusivos. **PROCEDENTE**, porém, a duplicação dessa regra fora da porta. Cura: extrair o predicado de leitura em `registro_batida.py` e reutilizá-lo nos dois consumidores. `app/chamados/services/materializacao.py:1328`, `app/chamados/services/materializacao.py:1333`, `app/ponto/registro_batida.py:106`.

   O prefixo de recusa conjunta conserva a causa para classificação; a casca de validação repassa a mensagem de espera sem anunciar ponto gravado. **NAO PROCEDE** quebra desses contratos. Permanecem as divergências de leitores do item 3. `app/chamados/services/materializacao.py:1362`, `app/chamados/catalogo/recusa.py:99`, `app/chamados/services/acoes_disputa.py:50`.

   O modal ganhou caixas de espera nos ramos validada/não validada. É renderizado pela casca modal e pelo retorno parcial compartilhado pelos POSTs das duas cascas; o GET do fio completo usa outro template. O cartão de detalhe continua apenas com “validada”. **PROCEDENTE** apresentação incompleta; cura: compartilhar a apresentação do estado. `app/templates/chamados/partials/modal_fio.html:229`, `app/templates/chamados/partials/modal_fio.html:243`, `app/chamados/views.py:927`, `app/chamados/services/fio.py:494`, `app/chamados/views.py:674`, `app/chamados/views.py:697`, `app/templates/chamados/partials/_pergunta_card.html:20`.

6. **ROLLBACK/E1 — atendido no caso normal; PROCEDENTE a exceção O330.**

   A falha da segunda escrita desfaz as novas batidas/carimbos antes da recusa; depois há releitura e devolução das validações, acrescentando decisões. Não há edição/apagamento de decisões anteriores nesse fluxo. **NAO PROCEDE** quebra append-only demonstrada pelo diff. `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:1356`, `app/chamados/services/materializacao.py:1361`, `app/chamados/services/validacao.py:202`, `app/chamados/services/validacao.py:208`, `app/chamados/models.py:3943`.

   **PROCEDENTE — carimbo prévio sem lastro:** O330 admite esse estado; rollback restaura o carimbo anterior, e a devolução retorna sem limpar validação quando `materializada_em` permanece preenchido. Portanto não garante “zero materializada_em e ambas devolvidas” nesse universo. Cura: tratar explicitamente o descarimbo sem lastro pela porta dona antes da devolução, preservando E1. R4 não cobre essa combinação. `app/chamados/services/passivo_validado.py:531`, `app/chamados/services/materializacao.py:73`, `app/chamados/services/validacao.py:190`, `app/chamados/models.py:3203`, `app/chamados/tests/test_bug_par_intervalo.py:182`, `app/chamados/tests/test_bug_par_intervalo.py:390`.

7. **CLASSES/PROVAS — não tomar GREEN ou RED agregado como demonstração completa.**

   **NAO PROCEDE** segundo escritor novo de batida/carimbo fora das portas nesse ramo: criação, vínculo e carimbo usam as portas existentes. **PROCEDENTE** derivação paralela de proximidade e identidade insuficiente, conforme itens 4–5. `app/chamados/services/materializacao.py:987`, `app/chamados/services/materializacao.py:1008`, `app/chamados/services/materializacao.py:1256`.

   **FILA — relógio:** o helper extraído lê `now()` por metade, e a aprovação lê outro instante. A expressão já existia no corpo anterior fornecido; regressão temporal concreta: **[nao verificado]**. Cura sugerida: receber o instante da borda do ato. `app/chamados/services/materializacao.py:1223`, `app/chamados/services/materializacao.py:1348`, `app/chamados/services/materializacao.py:993`.

   Os mutantes substituem função real no módulo e executam teste-alvo; morte efetiva por asserção de M1–M18: **[nao verificado]**. M1/M10 miram rollback/devolução; M2/M3 espera/guarda individual; M4–M6 julgamento final; M7/M13 ambiguidade/contagem; M8 proximidade; M9 mapa; M11–M18 reconciliação, contagem, voo, travas e universos. `app/chamados/tests/mutantes_bug_par_intervalo.py:18`, `app/chamados/tests/mutantes_bug_par_intervalo.py:43`, `app/chamados/tests/mutantes_bug_par_intervalo.py:53`, `app/chamados/tests/mutantes_bug_par_intervalo.py:127`.

   **PROCEDENTE:** M9 só altera artificialmente o mapa; não morde identidade real de duas pausas. **FILA:** M15/M18 conferem argumentos de trava, não concorrência SQL. R9 contém controles de comportamento legado que podem falhar na base apenas pelo import novo; isso não demonstra necessidade funcional da cura. `app/chamados/tests/test_bug_par_intervalo.py:463`, `app/chamados/tests/test_bug_par_intervalo.py:332`, `app/chamados/tests/test_bug_par_intervalo.py:361`, `app/chamados/tests/test_bug_par_intervalo.py:419`, `app/chamados/tests/test_bug_par_intervalo.py:446`.

8. **ESCRITORES/LEITORES E SEMÂNTICA — inventário central, complementando os sítios acima.**

   | Campo/fonte | Quem escreve | Quem lê |
   |---|---|---|
   | `validada_em/por` | Validação/devolução: `app/chamados/services/validacao.py:134`, `:202`; nova resposta: `app/chamados/services/disputa_emissao.py:1477`; carimbo/ressuscitação: `app/chamados/models.py:3135`, `:1565`. | C6/C7/estado: `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851`, `:3041`; declaração, explicador, vigias e placares: item 3; projeção E1: `app/chamados/models.py:3924`; reconciliação: `app/chamados/reconciliador.py:629`, `:1049`. |
   | `materializada_em/por`, `via_resolucao` | Porta interna e descarimbo em lote: `app/chamados/models.py:3099`, `:3266`; entradas `carimbar`, identidade, descarimbar: `:3103`, `:3168`, `:3203`. | Identificador/seletor: `app/chamados/services/materializacao.py:73`, `:1560`; fato/C6: `app/chamados/juizes.py:660`, `:1164`; relavra: `app/chamados/services/relavra_via.py:181`, `:244`; placar/veredito: `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:188`. |
   | `recusa_motivo`, `pendencia_admin` | Ramo hora/recusa/limpeza conjunta: `app/chamados/services/materializacao.py:910`, `:1163`, `:1352`; recusa humana: `app/chamados/services/validacao.py:604`; escape: `app/chamados/services/disputa_emissao.py:1494`; identidade: `app/chamados/models.py:3188`. | Devolução/catálogo: `app/chamados/services/validacao.py:193`, `app/chamados/catalogo/recusa.py:95`; explicador: `app/chamados/services/explicador.py:85`; auditorias: `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; flip: `app/ponto/services/flip_auto.py:132`; apresentação residual: `app/templates/chamados/partials/_card_atendimento.html:186`. |
   | `resposta_colab`, `respondida_em` | Resposta: `app/chamados/services/disputa_emissao.py:1464`; auto-resposta por carimbo: `app/chamados/models.py:3130`. | Prontidão/derivação: `app/chamados/services/materializacao.py:1384`, `:410`; C6/C7: `app/chamados/juizes.py:1169`, `app/chamados/models.py:1852`; E1: `app/chamados/models.py:3931`. |
   | Identidade, motivo, dia, destino, conversa | Abertura factual/conversa: `app/chamados/models.py:2784`, `:2809`, `:3016`. | Identificador/travas: `app/chamados/services/materializacao.py:67`, `:99`; derivação: `:393`; fechamento/alcance: `app/chamados/models.py:1850`, `app/chamados/juizes.py:948`. |
   | `contexto_json.marcos_faltantes` | Nascimento/reconstrução/renovação: `app/chamados/reconciliador.py:707`, `app/ponto/services/cartorio.py:1742`, `app/ponto/management/commands/reconciliar_grade.py:473`; porta de renovação: `app/chamados/services/contexto.py:102`. | Casamento/plantio: `app/chamados/juizes.py:534`, `app/chamados/services/materializacao.py:327`, `:463`; emissão: `app/chamados/services/disputa_emissao.py:1140`; lastro/grade: `app/ponto/services/lastro.py:75`, `app/escala/utils.py:1066`; regeneração: `app/escala/services/regeneracao.py:149`. |
   | Batida: `timestamp`, tipo, origem, aprovação, vínculo | Criação: `app/ponto/registro_batida.py:157`; anotação: `:254`; vínculo: `app/chamados/services/materializacao.py:1008`. | Proximidade/paridade: `app/ponto/registro_batida.py:107`, `app/chamados/services/materializacao.py:148`; lastro: `app/chamados/juizes.py:267`; competência: `app/ponto/janelas.py:84`. |

   **PROCEDENTE** a divergência semântica de `validada_em`: aceite pendente no escritor novo, terminalidade/falha em leitores antigos. **FILA** a fronteira de competência: O330 agrupa pelo dia do turno, enquanto a porta verifica o dia civil local do timestamp; são cálculos distintos já presentes, sem contraprova produtiva neste dado. A derivação overnight continua centralizada e aware; não encontrei nova conversão de fuso nesse diff. `app/chamados/services/materializacao.py:1402`, `app/chamados/juizes.py:1153`, `app/chamados/services/passivo_validado.py:113`, `app/ponto/janelas.py:84`, `app/chamados/services/materializacao.py:468`, `app/chamados/services/materializacao.py:475`.

   Exaustividade de **todos** os leitores/escritores de cada atributo incidental transitivamente alcançado pelo diff, além deste inventário: **[nao verificado]**. Execução independente de RED/GREEN, população completa e resultados dos mutantes: **[nao verificado]**.

## 09/11 08:31 · sessao 0c3a374ff426 · - · - · origem=Ronald (tmux)

Achados:

- **PROCEDENTE — explicador contradiz a espera.** A primeira metade torna-se `VALIDADA` (`app/chamados/models.py:3041`); sem batida, `explicar_pergunta` a classifica `validada_sem_gravar`, manda corrigir cadastro/escala ou TI/DP, sem verificar a espera (`app/chamados/services/explicador.py:97`, `app/chamados/services/explicador.py:103`). Com batidas no entorno, atribui intervalo deslocado ao cadastro (`app/chamados/services/explicador.py:47`, `app/chamados/services/explicador.py:58`). Esse explicador é incluído no fio administrativo (`app/chamados/services/fio.py:302`, `app/chamados/services/fio.py:312`) e o modal mostra “o sistema não conseguiu gravar”, com dono cadastro/TI (`app/templates/chamados/partials/modal_fio.html:449`). **Cura:** explicador consumir o estado canônico `aguardando_par`, antes de diagnosticar falha.

- **PROCEDENTE — declaração/app trata a espera como falha.** `_resposta_validada` lê somente `validada_em` (`app/chamados/services/declaracao_texto.py:221`). As portas de declaração devolvem `validada_sem_registro` e mandam refazer o horário pela supervisão (`app/chamados/services/declaracao_texto.py:406`, `app/chamados/services/declaracao_texto.py:677`); a consulta usada pela apresentação replica essa guarda (`app/chamados/services/declaracao_texto.py:272`). Os cards e o formulário continuam anunciando falha/refação (`app/templates/chamados/partials/_card_atendimento.html:215`, `app/templates/chamados/partials/_card_atendimento.html:290`, `app/templates/chamados/partials/modal_fio.html:146`). **Cura:** distinguir explicitamente espera legítima da validação sem gravação defeituosa.

- **PROCEDENTE — proteção de fechamento é contingente à irmã.** A metade validada fica morta para C6 (`app/chamados/juizes.py:1153`), sai de `q_vivas` (`app/chamados/models.py:3359`) e de `respostas_sem_veredito` (`app/chamados/models.py:1851`). `pode_fechar` não exige aplicação das metades validadas (`app/chamados/models.py:1861`). Portanto, se a irmã for retirada/absolvida/trancada e não restar outra pergunta viva/sem-veredito, a disputa pode fechar com aquela metade ainda pendente; o fecho automático usa esse juiz (`app/chamados/services/disputa_emissao.py:1960`, `app/chamados/services/becos.py:56`). No caso normal da fixture, a irmã continua viva e impede fechar (`app/chamados/tests/test_bug_par_intervalo.py:115`). **Cura:** representar a pendência do ato no juiz de fechamento e testar retirada da irmã entre os dois aceites.

- **PROCEDENTE — espera infla dívida de gravação.** `vigia_carimbo.classificar` seleciona todas as horas validadas, inclusive espera (`app/chamados/services/vigia_carimbo.py:97`), e classifica a metade sem batida como `sem_plantio_nenhum` (`app/chamados/services/vigia_carimbo.py:116`); o censo a soma em `resposta_enviada_sem_gravacao`, rotulado “esperado 0” (`app/chamados/services/vigia_carimbo.py:129`, `app/chamados/services/vigia_carimbo.py:140`). O reparador seleciona esse universo (`app/chamados/services/passivo_validado.py:324`). **Cura:** classe explícita de espera, preservando separadamente o contador de falhas reais.

- **NÃO PROCEDE — alarme de passivo no cron padrão.** O command só acrescenta esse contador ao alarme com `--alarme-passivo` (`app/chamados/management/commands/vigiar_carimbo_sem_registro.py:73`); o cron usa apenas `--alarme` (`app/config/crons.py:1751`). A combinação dos dois argumentos pode alarmar a espera legítima (`app/chamados/management/commands/vigiar_carimbo_sem_registro.py:77`).

- **NÃO PROCEDE — materializada no estado comum.** Sem `materializada_em`, o estado é `VALIDADA`, não `RESOLVIDA` (`app/chamados/models.py:3043`); `pergunta_sem_acao_possivel` devolve False (`app/chamados/services/raiox.py:173`). A espera fica fora do crédito de perguntas vivas, pois este lê `q_vivas` (`app/chamados/services/raiox.py:72`). Badge e verbos seguem esse crédito (`app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`); a irmã determina a ação remanescente.

- **NÃO PROCEDE — devolução automática da espera pela porta nova.** A devolução exclui exatamente `ESPERA_PAR_INTERVALO` (`app/chamados/services/validacao.py:192`); a validação retorna antes da varredura/fecho (`app/chamados/services/validacao.py:482`, `app/chamados/services/validacao.py:489`).

- **FILA — impacto efetivo em `vigia_fio`/`o_que_segura`.** `vigia_fio.fio_do_dia` delega ao C6 (`app/chamados/services/vigia_fio.py:165`); seus dias mortos e decisões pendentes recebem classes de `vigia_carimbo` (`app/chamados/services/vigia_fio.py:463`, `app/chamados/services/vigia_fio.py:485`). `o_que_segura` lê `porta_alcancavel_do_dia` e relata o papel/fila dessa porta (`app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:221`). A ocorrência numérica em produção e reabertura por esses leitores permanecem **[nao verificado]**.

Inventário de escritores dos três campos de PerguntaDisputa, por código executável:

| Campo | Escritores |
|---|---|
| `validada_em` | `_carimbar_validacao` (`app/chamados/services/validacao.py:134`); devolução (`app/chamados/services/validacao.py:202`); resposta nova (`app/chamados/services/disputa_emissao.py:1477`); `carimbar` (`app/chamados/models.py:3135`); ressuscitação em lote (`app/chamados/models.py:1565`); restauração apenas em memória (`app/chamados/models.py:3164`). |
| `materializada_em` | `_escrever_carimbo` (`app/chamados/models.py:3099`), chamado pelas portas `carimbar`/identidade; descarimbo em lote (`app/chamados/models.py:3266`). `regularizacao_ext.py:198` escreve campo homônimo de outro modelo. |
| `recusa_motivo` | `_materializar_resposta_hora` (`app/chamados/services/materializacao.py:910`); `_carimbar_recusa` (`app/chamados/services/materializacao.py:1163`); limpeza do par (`app/chamados/services/materializacao.py:1352`); recusa humana (`app/chamados/services/validacao.py:604`); resposta escape (`app/chamados/services/disputa_emissao.py:1494`); `carimbar_identidade` (`app/chamados/models.py:3188`), também usado pela deduplicação (`app/chamados/services/fio.py:1351`). |

Outros leitores relevantes fora do diff:

- `validada_em`: inscrições/âncoras (`app/chamados/models.py:310`, `app/chamados/models.py:344`); C7 (`app/chamados/models.py:1851`); projeção E1 (`app/chamados/models.py:3924`); irmãs sem consumo (`app/chamados/juizes.py:1769`); reconciliação de órfãs (`app/chamados/reconciliador.py:629`, `app/chamados/reconciliador.py:1049`); resposta/correção (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/acoes_disputa.py:345`); contagem de validações da equipe (`app/chamados/services/verbos.py:240`, `app/chamados/services/painel.py:268`); procedência de batida (`app/ponto/services/procedencia_batida.py:74`); auditoria (`app/chamados/management/commands/auditar_invariantes_chamados.py:1075`).
- `materializada_em`: juiz do fato (`app/chamados/juizes.py:660`, `app/chamados/juizes.py:687`); C6 (`app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1227`); reutilização (`app/chamados/juizes.py:1245`); leitor da ação possível (`app/chamados/services/raiox.py:173`); marco do placar (`app/chamados/services/painel.py:67`); marco do veredito (`app/chamados/veredito_lavrado.py:188`); abertura de perguntas (`app/chamados/services/disputa_emissao.py:228`, `app/chamados/services/disputa_emissao.py:1067`, `app/chamados/services/disputa_emissao.py:1323`); reconciliação (`app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:1500`); desfazedor de carimbo (`app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:244`).
- `recusa_motivo`: devolução (`app/chamados/services/validacao.py:193`); explicador (`app/chamados/services/explicador.py:90`); fila de flip (`app/ponto/services/flip_auto.py:132`); auditorias de ausência de dono (`app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`); apresentação crua residual no card (`app/templates/chamados/partials/_card_atendimento.html:186`).

A espera não perde o registro de validação E1: `_carimbar_validacao` grava projeção e decisão na mesma transação, usando o mesmo instante (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:139`). A contagem da equipe aumenta pelo aceite mesmo sem aplicação, coerente com contar validações (`app/chamados/services/verbos.py:240`); chamá-la de aplicação seria **[nao verificado]**.

## 09/11 08:31 · sessao 1c264a102439 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura:

- **PROCEDENTE — identidade não prova “mesma pausa”.** `identificar_par_intervalo` agrupa apenas disputa/data/motivo e consulta o mapa global; não lê DNA do dia, hora do marco, destino nem `instancia_marco`. Com o mapa atual, a comparação canônica sempre recebe `S/hii + E/hfi`. Uma saída sobrevivente da primeira pausa e uma volta sobrevivente da segunda passam como par. `app/chamados/services/materializacao.py:70`; `app/chamados/services/materializacao.py:84`; `app/chamados/juizes.py:885`. As duas pausas efetivamente compartilham motivos no emissor: `app/ponto/marcos.py:35`. Cura: identificar os slots reais pela autoridade de marcos, ou abster quando o DNA tiver múltiplas pausas sem identidade suficiente; não inventar índice.

- **PROCEDENTE — a contraprova R7 não cobre duas pausas reais.** O teste cria quatro perguntas, mas conserva a escala de uma pausa e testa a ambiguidade por contagem. Não cobre duas perguntas remanescentes de pausas distintas nem duas pausas no DNA. `app/chamados/tests/test_bug_par_intervalo.py:45`; `app/chamados/tests/test_bug_par_intervalo.py:255`. A preservação universal de HEAD com duas pausas é **[nao verificado]**; para quatro perguntas elegíveis o código cai no avulso. `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:1400`.

- **PROCEDENTE — duplicação do predicado de proximidade.** O par repete consulta e tolerância ±60s inclusivos da porta. `app/chamados/services/materializacao.py:1328`; `app/ponto/registro_batida.py:106`. Cura: extrair consulta/predicado de leitura no dono `registro_batida.py`, consumido pela porta e pelo par. A mensagem “colada” tem testemunha concreta nesse novo caminho, filtrada por mesmo colaborador/tipo e não retratada. `app/chamados/services/materializacao.py:1329`; `app/chamados/services/materializacao.py:1333`. O catálogo isoladamente apenas reconhece substring, sem medir batida. `app/chamados/catalogo/recusa.py:39`; `app/chamados/catalogo/recusa.py:99`.

- **PROCEDENTE — leitor do app interpreta espera como defeito.** `declarar_hora_colab` e `aplicar_declaracao` consideram qualquer `validada_em` uma `validada_sem_registro` quando o marco falta e mandam a supervisão refazer o horário, sem distinguir a nova espera legítima. `app/chamados/services/declaracao_texto.py:210`; `app/chamados/services/declaracao_texto.py:372`; `app/chamados/services/declaracao_texto.py:406`; `app/chamados/services/declaracao_texto.py:662`; `app/chamados/services/declaracao_texto.py:677`. Cura: consultar estado de aplicação/espera antes desse retorno e devolver “falta a outra metade”, preservando a declaração.

- **NAO PROCEDE — espera depende de substring.** Há chave exata e retorno triplo compatível com `traduzir_recusa`; reconhecimento ocorre antes das regras por substring. `app/chamados/catalogo/recusa.py:25`; `app/chamados/catalogo/recusa.py:95`. Recusa conjunta preserva os marcadores de classificação porque apenas prefixa a causa. `app/chamados/services/materializacao.py:1362`; `app/chamados/catalogo/recusa.py:99`. Chamadores desempacotam três valores sem enumeração fechada: `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:172`; `app/chamados/services/fio.py:703`; `app/chamados/services/passivo_validado.py:144`; `app/chamados/services/validacao.py:548`.

- **NAO PROCEDE — casca de validação anuncia ponto gravado para espera.** Estado diferente de `validada` e `ok=True` usa a mensagem do serviço. `app/chamados/services/acoes_disputa.py:50`; `app/chamados/services/acoes_disputa.py:52`; `app/chamados/services/validacao.py:485`. `aplicar_declaracao` também repassa estado/mensagem do serviço na primeira aplicação. `app/chamados/services/declaracao_texto.py:690`.

- **PROCEDENTE — apresentação não alcança todas as telas.** O modal acrescentou caixas de espera nos ramos validada e não validada. `app/templates/chamados/partials/modal_fio.html:229`; `app/templates/chamados/partials/modal_fio.html:243`. O cartão da página de detalhe só mostra “validada” e não lê `estado_aplicacao`. `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/detalhe_local.html:310`. Cura: compartilhar apresentação do estado de aplicação. O modal é renderizado diretamente pela casca modal e nos retornos parciais usados pelas duas cascas; a página completa do fio usa outro template, sem incluir o modal. `app/chamados/views.py:927`; `app/chamados/services/fio.py:494`; `app/chamados/views.py:674`; `app/chamados/views.py:697`. Há outro render do modal no retorno de recobrança. `app/chamados/views.py:1399`.

- **FILA — relógio fora da borda.** `_carimbar_aplicacao`, extração agora compartilhada por duas metades, lê relógio por chamada; aprovação da batida lê outro instante. Não existe instante único recebido pelo ato. `app/chamados/services/materializacao.py:1223`; `app/chamados/services/materializacao.py:1348`; `app/chamados/services/materializacao.py:993`. É extração de comportamento anterior no diff fornecido, não prova de regressão temporal. Cura sugerida: instante recebido da borda e repassado às duas escritas/carimbos. Efeito em produção **[nao verificado]**.

Mutantes e RED:

- **NAO PROCEDE quanto a mutação apenas de fonte fictícia:** o executor recompila a função real no dicionário global do módulo e chama o teste selecionado; verifica âncora única. `app/chamados/tests/mutantes_bug_par_intervalo.py:76`; `app/chamados/tests/mutantes_bug_par_intervalo.py:127`. Execução/morte efetiva M1–M18: **[nao verificado]**.
- **FILA — R3 não prova ausência de deadlock:** faz duas validações sequenciais. `app/chamados/tests/test_bug_par_intervalo.py:174`. M15 e M18 verificam os IDs passados a `trava_na_ordem`, sem duas conexões concorrentes nem observação de locks SQL. `app/chamados/tests/test_bug_par_intervalo.py:332`; `app/chamados/tests/test_bug_par_intervalo.py:361`. Cura: teste concorrente de duas transações reais.
- M1/M10 apontam ao rollback/devolução; M3 à guarda individual; M4 à paridade final; M5 à ordem temporal; M6 ao lastro; M8 à proximidade; M11 à reconciliação; M12 à contagem; M14 ao voo; M16 ao universo; M17 ao filtro de carimbo; M7/M13 à ambiguidade/contagem; M9 à tabela global. Correspondências estão declaradas no executor. `app/chamados/tests/mutantes_bug_par_intervalo.py:18`; `app/chamados/tests/mutantes_bug_par_intervalo.py:30`; `app/chamados/tests/mutantes_bug_par_intervalo.py:43`; `app/chamados/tests/mutantes_bug_par_intervalo.py:53`; `app/chamados/tests/mutantes_bug_par_intervalo.py:60`.
- **PROCEDENTE — M9 não morde identidade real de pausa:** só altera `MARCO_DA_PERGUNTA` artificialmente; não testa DNA com duas pausas/casal cruzado. `app/chamados/tests/test_bug_par_intervalo.py:463`.
- **FILA — RED por símbolo ausente não demonstra cura funcional:** controles R9 de ausência/irmã já aplicada exigem a função nova por import, embora seus resultados preservem o caminho avulso; a base pode errar no import antes de testar o comportamento. `app/chamados/tests/test_bug_par_intervalo.py:419`; `app/chamados/tests/test_bug_par_intervalo.py:431`; `app/chamados/tests/test_bug_par_intervalo.py:446`. R7 também exige a chave nova de contagem depois de conferir a recusa legada. `app/chamados/tests/test_bug_par_intervalo.py:261`; `app/chamados/tests/test_bug_par_intervalo.py:270`. R1/R2/R3, por sua vez, exigem explicitamente espera e aplicação das duas; não são meros controles de HEAD. `app/chamados/tests/test_bug_par_intervalo.py:151`; `app/chamados/tests/test_bug_par_intervalo.py:164`; `app/chamados/tests/test_bug_par_intervalo.py:174`.

Fontes paralelas existentes, com perguntas diferentes: mapa motivo→marco `app/chamados/juizes.py:882`; classificação marco→motivo `app/ponto/marcos.py:14`; posição→motivo/instância `app/chamados/services/declaracao_texto.py:42`; pares canônicos de DNA `app/escala/models.py:1652`; leitura de todas as luzes das pausas `app/chamados/juizes.py:917`; defesa que se abstém com duas pausas `app/chamados/juizes.py:1124`. Não são outro agrupador de perguntas equivalente a `identificar_par_intervalo`; exigirem chamar esse agrupador seria inverter dependências. A divergência concreta é o agrupador novo afirmar identidade sem usar os slots que essas autoridades distinguem. `app/chamados/services/materializacao.py:84`.

## 09/11 08:31 · sessao f854c2b1485a · - · - · origem=Ronald (tmux)

**ATO/TRAVAS — NAO PROCEDE para par identificado; universalidade exige ressalvas.**

- Chamadores AST de `_materializar_uma`: `validar_pergunta`, import sem alias, `app/chamados/services/validacao.py:466`, chamadas `:469`/`:474`; varredura, nome local, `app/chamados/services/materializacao.py:1565`; `_plantar_o_dia`, import sem alias `app/chamados/services/passivo_validado.py:39`, chamada `:529`.
- Chamadores AST de `_materializar_resposta_hora`: somente `_materializar_par`, nome local, `app/chamados/services/materializacao.py:1336`, e `_materializar_avulsa`, nome local, `:1447`. Existe reexport, sem chamada, em `app/chamados/models.py:4077`.
- Par identificado: ambas entram em voo antes da primeira escrita; mesmo savepoint; ramo individual ignora guarda intermediária; guarda final, lastro, carimbos e reconciliação vêm depois das duas chamadas. `app/chamados/services/materializacao.py:1315`, `:1317`, `:1336`, `:935`, `:1338`, `:1345`, `:1348`, `:1355`.
- Toda **nova criação** desse ramo usa `registrar_batida`, import sem alias; porém reuso consulta batida e pula a chamada à porta, portanto “ambas sempre chamam registrar” literalmente não é verdadeiro. `app/chamados/services/materializacao.py:919`, `:925`, `:931`, `:986`, `:987`; `app/chamados/juizes.py:235`.
- Continua havendo aplicação avulsa de pergunta de intervalo quando identidade retorna `sem_par`, `par_ambiguo` ou `marcos_sem_par`; pode criar só uma metade. Isso preserva a rota individual para esse universo. `app/chamados/services/materializacao.py:77`, `:79`, `:85`, `:1400`, `:1447`. O teste de irmã já aplicada exige esse comportamento. `app/chamados/tests/test_bug_par_intervalo.py:444`.

**TRAVAS — NAO PROCEDE para inversão entre esses dois validadores.**

- Chamadores AST de `_travar_ato_materializacao`, todos sem alias: `validacao.py:434` (import `:433`), `materializacao.py:1379` (local), `passivo_validado.py:432` (import `:431`) e `:520` (import `:513`). Transações anteriores: `validacao.py:428`, `materializacao.py:1378`, `passivo_validado.py:371`/`:519`.
- O helper pega Colaborador antes de enumerar o universo; depois trava Disputas, Chamados e Perguntas. A porta ordena cada grupo por PK. `app/chamados/services/materializacao.py:95`, `:97`, `:104`; `app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`.
- Mesmo colaborador serializa os dois validadores antes da primeira Pergunta; validar saída/volta em ordem inversa não cria a inversão de travas descrita. `app/chamados/services/validacao.py:434`, `:439`; `app/chamados/services/materializacao.py:95`. R3 é sequencial; concorrência executada **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:174`.

**ROLLBACK — NAO PROCEDE no caso normal; PROCEDENTE na promessa universal para O330.**

- Falha da segunda escrita sai do savepoint antes da recusa; releitura recupera estado anterior; recusa usa `encerrar_colisao=False`; devolução limpa validação e acrescenta decisão. Novas batidas/carimbos do par são revertidos pela estrutura transacional. `app/chamados/services/materializacao.py:1315`, `:1356`, `:1361`, `:1364`; `app/chamados/services/validacao.py:198`, `:202`, `:205`, `:208`.
- **Contraexemplo O330:** o reparador inclui perguntas com `materializada_em` anterior sem lastro; rollback restaura esse carimbo; `devolver_validacao_recusada` retorna antes de limpar validação se `materializada_em` não é nulo. Portanto não garante “zero materializada_em e devolução das duas” nesse universo. Cura sugerida: tratamento explícito pelo dono de descarimbo para carimbo sem lastro, antes da devolução, preservando E1; ou delimitar formalmente o contrato como preservação do estado anterior. `app/chamados/services/passivo_validado.py:531`; `app/chamados/services/materializacao.py:73`, `:74`, `:1361`, `:1365`; `app/chamados/services/validacao.py:188`, `:190`; porta de descarimbo `app/chamados/models.py:3203`.
- E1 do fluxo novo acrescenta `VALIDACAO`/`DEVOLUCAO`; não atualiza/apaga decisão anterior. A projeção legada é capturada antes da troca. `app/chamados/services/validacao.py:133`, `:139`, `:201`, `:208`. Modelo recusa edição/apagamento e porta usa `bulk_create`. `app/chamados/models.py:3903`, `:3908`, `:3943`.
- R4 verifica falha da segunda escrita no validador normal; não cobre esse rollback de O330 com carimbo prévio. `app/chamados/tests/test_bug_par_intervalo.py:182`, `:194`; fixtures O330 com carimbos falsos só verificam sucesso/exclusão de universo. `app/chamados/tests/test_bug_par_intervalo.py:382`, `:390`, `:403`.

**OUTROS CAMINHOS — FILA, por serem atos diferentes de aplicar respostas desse par.**

- `veredito_celula` pode preencher marcos de intervalo pela porta e depois fechar perguntas do dia; simula o conjunto antes, mas captura erro por escrita e só dá rollback quando nenhuma ação teve sucesso. Falha posterior pode conservar a primeira batida. É decisão do dia, sem chamar `_materializar_resposta_hora`/identificador de perguntas. Cura da unidade desse outro ato fica fora da correção estreita, mas impede afirmar atomicidade universal de todo preenchimento de intervalo. `app/ponto/services/veredito_celula.py:290`, `:305`, `:320`, `:322`, `:327`, `:342`, `:346`, `:179`, `:189`.
- Regularização externa também tem ramo próprio de par de intervalo; `_cria_batida` transforma recusa em retorno `None`, permitindo prosseguir com a irmã e materializar o lote. Não aplica `PerguntaDisputa`. `app/chamados/services/regularizacao_ext.py:77`, `:83`, `:88`, `:121`, `:167`, `:172`, `:176`, `:197`.

## 09/11 08:26 · sessao 2170a5030b34 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 08:26 · sessao c216c7c7d88f · - · - · origem=Ronald (tmux)

Fixtures e riscos consolidados:

- Dois motivos válidos no mesmo dia: `saida_sem_entrada` e `orfao_14h`; derivam instâncias `inicio`/`fim`, evitando colisão da identidade. `chamados/services/declaracao_texto.py:44`; `chamados/models.py:3425`.
- Crie `Empresa`, `User`, `Colaborador(nome='col001', usuario=user, deve_trocar_senha=False, plataforma_principal='android')`, `CelulaDia` com DNA 08–17, disputa e perguntas por `.objects.create()`. Reafirme após montar as linhas `veredito='furo'`, `veredito_via='cartorio'`, ata com ambas as lâmpadas apagadas. C6 responde viva pelo veredito acusador. `chamados/juizes.py:1354`; exemplo existente em `chamados/tests/test_fio_le_o_juiz.py:50`.
- Para M2/M3/morta, use `QuerySet.update()` nos carimbos `fechada_em`, `respondida_em`/`resposta_colab` ou `validada_em`. Evita efeitos dos signals de fechamento e continua sendo ORM real. `chamados/signals.py:18`; `chamados/signals.py:237`.
- M5: para cada colaborador, repita `orfao_14h` em datas distintas; a unicidade inclui dia e motivo. `chamados/models.py:2437`.
- Ambos os kernels: `APIRequestFactory().get()`, `force_authenticate()`, chamada à função importada de cada módulo; padrão existente em `api/tests/test_me_posto_geo.py:103`.
- Dashboard: `Client.force_login()` e `reverse('core:dashboard')`, já empregado em `core/tests/test_dashboard_badge.py:21`. Plataforma preenchida evita escrita de classificação durante GET. `core/views.py:143`.
- Registro a baixar: exclusivamente a expressão `_p(...)` de duas linhas em `core/juizes.py:759`.
- Delete `test_badge_disputa_pendente.py`: sustenta explicitamente contagem por disputa. `chamados/tests/test_badge_disputa_pendente.py:8`.
- Migre `test_esmeril2_left_join.py`: preserve defesa contra pai sem filho e altere duas perguntas para contagem **2** pela consulta única. `chamados/tests/test_esmeril2_left_join.py:14`.
- Migre referências do fio e dashboard para `len(consulta_do_app(... )['pendentes'])`; preservam comparação com vitalidade e tela. `chamados/tests/test_fio_le_o_juiz.py:261`; `core/tests/test_dashboard_badge.py:37`.
- Preserve M6: alimentação pronta, sem mock de ORM/C6. `chamados/tests/test_e3_pendentes_app.py:374`. Mutantes da matriz precisam recompilar funções reais em memória para alcançar os novos testes ORM; hoje só alteram `SOURCES` consumido pelos executores fake. `chamados/tests/test_e3_pendentes_app.py:414`.

Não escrevi arquivos nem executei testes. [nao verificado] [nao verificado]

## 09/11 08:25 · sessao afb90b29fac5 · - · - · origem=Ronald (tmux)

- Fontes que C17 já lê: `chamados/juizes.py::alimentar_portas` fornece `verbo` por `particionar_verbos(qs_vivos())` (`:1290-1293`), `fila_ids`/`fila_cel` por `qs_na_fila()` (`:1296-1301`). C17 compõe essas fontes em `:1376-1392`. Usar diretamente as autoridades dentro da carga da tarefa evita copiar C17.
- Atenção ao relógio: `particionar_verbos` lê `timezone.now()` em `chamados/services/verbos.py:35`. Acrescentar parâmetro opcional `agora` permite à tarefa preservar seu relógio explícito; `qs_na_fila(agora=)` já existe em `painel.py:158-190`.
- A carga da tarefa já coleta âncoras de perguntas e sua pertença à fila, mas não chamados da célula: `tarefa_do_dia.py:358-361`. O retorno atual sai por `_tarefa_pura` em `:399-402`. Ali cabe enriquecer aditivamente com `rota`, `fila`, `verbos`, `fio`, preservando campos anteriores.
- Fixture ORM de referência: `test_e2_tarefa_do_dia.py:69-138`: Empresa, Praça, Posto, Colaborador, TipoEscala, EscalaColaborador, CelulaDia com DNA e lâmpadas. Reafirmar a ata depois de criar pergunta porque os sinais relavram (`:104-108`). Aplicar `freeze_time` na classe nova e nomes `colNNN`.
- Caso A/B real já demonstrado em `test_o261e_f5b_juiz_porta.py:124-141`: A aberto da célula, B aberto sem célula, pergunta destinada a B. Painel responde `{A: 'decidir', B: 'cobrar'}`; C17 responde `fila=(A,)`.
- Dia isento: fonte é `Colaborador.isento_ponto`, lida por `core/constants.py:13-18`; tarefa extingue ações em `tarefa_do_dia.py:179-187`.
- DECISAO-ABERTA: os anexos determinam que prevalece a tarefa, mas não fixam os valores literais dos campos aditivos nos casos sem pergunta/sem ações. A interpretação precisa ficar explícita nos quatro testes escritos antes da implementação. Execução/comportamento futuro: [nao verificado]. [nao verificado]

## 09/11 08:24 · sessao 2e5a8dc4c11c · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 08:24 · sessao ce974f8b148b · - · - · origem=Ronald (tmux)

6. Os selos têm mordida parcial, sem prova integral do contrato:

- **Leitura sem escrita no preparo capturado:** executam o envelope real nos dois modos, exigem consulta positiva, cluster, `sql_escrita=0`, arquivo intacto e host no argv. A detecção externa é regex sobre `sql.log`. (`bin/tests/test_sonda_sem_grant.sh:82`, `bin/tests/test_sonda_sem_grant.sh:85`, `bin/tests/test_sonda_sem_grant.sh:91`, `bin/tests/test_sonda_sem_grant.sh:93`, `bin/tests/test_sonda_sem_grant.sh:96`)
- **Recusa nomeando falta e porta:** cobrem tabela sem grant, role ausente, catálogo vazio/falho, login falho, HOST antigo e arquivo ausente; exigem rc 5 e ausência da execução simulada. Não há casos separados para arquivo sem senha, permissões erradas, login com usuário/read-only errado, falta exclusiva de sequência/default/USAGE ou tentativa de SQL proibido pelo classificador. (`bin/tests/test_sonda_sem_grant.sh:100`, `bin/tests/test_sonda_sem_grant.sh:109`, `bin/tests/test_sonda_sem_grant.sh:123`; caminhos omitidos em `bin/sonda_leitura.sh:170`, `bin/sonda_leitura.sh:173`, `bin/sonda_leitura.sh:204`)
- **Porta existe e escreve:** cobram algumas frases de DCL/DDL, senha sem HOST, modo 600 e trilha, nos dois destinos. Não procuram outro escritor na árvore; copiam apenas cinco scripts e substituem `sombra_db.sh` por stub. Também não exigem cluster correto para o provisionador. (`bin/tests/test_sonda_sem_grant.sh:67`, `bin/tests/test_sonda_sem_grant.sh:70`, `bin/tests/test_sonda_sem_grant.sh:129`, `bin/tests/test_sonda_sem_grant.sh:133`, `bin/tests/test_sonda_sem_grant.sh:141`)
- **Propagação rc 5 no wrapper:** o novo caso exige rc 5, Codex não chamado, mensagem/porta visíveis e sentinelas brutas escondidas; usa envelope falso. Não cobre integração com envelope real nem payload terminando em 5. (`bin/tests/test_codex_sonda.sh:40`, `bin/tests/test_codex_sonda.sh:55`, `bin/tests/test_codex_sonda.sh:85`)
- **Refazer chama porta antes de carimbar:** verifica somente presença textual e posição das duas linhas; não executa `refazer` nem força falha do provisionador. (`bin/tests/test_sonda_sem_grant.sh:150`)

**O docker falso não cobre o payload.** Captura stdin/argv somente de `docker exec` contendo `psql`; `docker run` apenas imprime `SONDA_RODOU` e devolve zero. Não executa Django, driver ou `/sonda/sonda.py`; `sombra_db.sh` também não participa. (`bin/tests/test_sonda_sem_grant.sh:41`, `bin/tests/test_sonda_sem_grant.sh:46`, `bin/tests/test_sonda_sem_grant.sh:59`, `bin/tests/test_sonda_sem_grant.sh:70`; execução real em `bin/sonda_leitura.sh:241`)

Mutantes plausíveis sobreviventes, por inspeção; execução dos mutantes **[nao verificado]**:

1. Fazer `_escritas_no_texto` sempre devolver zero: nenhum caso injeta comando proibido; o preparo atual continua SELECT. (`bin/sonda_leitura.sh:147`; casos em `bin/tests/test_sonda_sem_grant.sh:85`, `bin/tests/test_sonda_sem_grant.sh:99`)
2. Acrescentar `SELECT ... INTO ...` ou SELECT de função com efeito: o detector externo não contempla `INTO` nem identifica efeitos de funções. (`bin/tests/test_sonda_sem_grant.sh:32`)
3. Remover o `falhou` após provisionamento, mantendo a chamada antes do carimbo: o teste de forma continua satisfeito. (`bin/sombra.sh:306`, `bin/sombra.sh:307`; `bin/tests/test_sonda_sem_grant.sh:150`)
4. Acrescentar escritor em outro arquivo ou no bootstrap verdadeiro: não há censo/assertiva global de exclusividade. (`bin/tests/test_sonda_sem_grant.sh:67`, `bin/tests/test_sonda_sem_grant.sh:70`)
5. Fazer o provisionador usar sempre `saas_db`: suas asserções procuram frases e `destino=` na trilha, sem conferir container/banco dos grants em cada modo. (`bin/tests/test_sonda_sem_grant.sh:129`, `bin/tests/test_sonda_sem_grant.sh:133`, `bin/tests/test_sonda_sem_grant.sh:141`)

7. Classes presentes ou parcialmente resolvidas:

- **Derivação paralela:** a pergunta de catálogo foi centralizada corretamente; ambos chamam as mesmas funções. Mas “pronto” operacional continua desigual: o provisionador anuncia pronto após catálogo; o leitor exige ainda arquivo válido e login TCP com `leitor|on`. (`bin/sonda_leitor_comum.sh:33`, `bin/sonda_leitor_comum.sh:69`; `bin/sonda_provisionar.sh:90`; `bin/sonda_leitura.sh:169`, `bin/sonda_leitura.sh:198`)
- **Selo sem mordida:** o comentário remete o mutante a log externo; o próprio selo não o constrói/executa. O novo caso do wrapper demonstra as assertivas contra resposta simulada, sem provar confinamento do payload. Resultados/log citado **[nao verificado]**. (`bin/tests/test_sonda_sem_grant.sh:21`; `bin/tests/test_codex_sonda.sh:40`, `bin/tests/test_codex_sonda.sh:85`)
- **`sql_escrita` não mede todo SQL escrito:** incrementa somente texto bloqueado pelo classificador, antes de enviar. Login usa caminho próprio, payload usa Django e bootstrap é outra chamada. Zero significa nenhuma tentativa classificada nesses pontos, não ausência integral de SQL de escrita. (`bin/sonda_leitura.sh:111`, `bin/sonda_leitura.sh:157`, `bin/sonda_leitura.sh:162`, `bin/sonda_leitura.sh:200`, `bin/sonda_leitura.sh:241`)
- **rc 5 mistura causas:** credencial indisponível, catálogo/readiness, login e tentativa interna de SQL de escrita produzem o mesmo código; `recusa=` conserva texto normalizado, sem classe estruturada. (`bin/sonda_leitura.sh:125`, `bin/sonda_leitura.sh:127`, `bin/sonda_leitura.sh:160`, `bin/sonda_leitura.sh:169`, `bin/sonda_leitura.sh:181`, `bin/sonda_leitura.sh:202`)
- **Recusa silenciosa ao bloquear SQL:** `_sql` chama `_recusa`, que executa `exit 5`; sua invocação na conferência redireciona stderr inteiro a `/dev/null`. Esse `exit` encerra o shell antes do `|| _recusa` externo, escondendo mensagem/porta justamente na tentativa de escrita; execução concreta **[nao verificado]**. (`bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:131`, `bin/sonda_leitura.sh:158`, `bin/sonda_leitura.sh:179`)
- **Ausência de trava global para credencial:** a porta usa o mesmo `$ENVFILE.novo` e reescreve o segredo antes do banco sem flock próprio; duas invocações independentes prod/sombra podem competir. A trava de `sombra.sh` protege aquele fluxo, não a invocação administrativa direta. Corrida concreta **[nao verificado]**. (`bin/sonda_provisionar.sh:61`, `bin/sonda_provisionar.sh:64`, `bin/sonda_provisionar.sh:69`; `bin/sombra_trava.sh:21`, `bin/sombra_trava.sh:28`)

Os selos entram no loop da régua; sua existência não prova que executaram ou passaram. O pre-push documenta que esse loop não roda ali. Resultados **[nao verificado]**. (`bin/regua.sh:138`; `bin/pre-push.sh:13`)

## 09/11 08:23 · sessao 7707844abd3f · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 08:20 · sessao 7e7cab7dc194 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

A cura está escrita: o par identificado aplica as duas metades no mesmo `atomic`, com paridade e lastro julgados após as duas escritas. **[RED nao executado]**; GREEN e mortalidade dos mutantes permanecem **[nao verificado]**. Fonte: [materializacao.py:1302](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/services/materializacao.py:1302).

Por hunk:

| Arquivo:linha | Alteração |
|---|---|
| `chamados/services/materializacao.py:55` | Predicado único `identificar_par_intervalo`: disputa/data, unicidade das metades e mesmo par canônico. Ambiguidade devolve `par_ambiguo`; consulta `chamados/juizes.py:882` e `escala/models.py:1652`. |
| `chamados/services/materializacao.py:91` | Reúne disputas, pais, destinos, clones e perguntas antes da escrita; adquire travas pela porta existente de `chamados/models.py:2637`. |
| `chamados/services/materializacao.py:844` | `par_em_ato` adia a guarda individual e a reconciliação. Escrita continua por `registrar_batida`, em `:987`. |
| `chamados/services/materializacao.py:1220` | Extrai o carimbo compartilhado; `:1264` extrai a conferência de data. Ambos servem ao par e ao caminho avulso. |
| `chamados/services/materializacao.py:1302` | Mantém ambas em voo; grava, julga conjunto, exige lastro, carimba e reconcilia. Falha desfaz o savepoint e devolve as duas validações pelo dono. |
| `chamados/services/materializacao.py:1372` | Despacha par/avulsa, espera e contadores. Respeita o universo selecionado; `:1415` conserva o caminho avulso. |
| `chamados/services/validacao.py:434` | Trava antes de validar; preserva o primeiro aceite e retorna `aguardando_par`. A devolução distingue espera de recusa em `:193`. |
| `chamados/services/materializacao.py:1526` | Varredura processa cada unidade uma vez e conta ambas as perguntas aplicadas. |
| `chamados/services/passivo_validado.py:432` | Pré-trava toda a competência; `:529` entrega ao ato apenas o universo selecionado pelo O330, incluindo carimbos sem lastro. |
| `chamados/catalogo/recusa.py:25` | Espera com código próprio e tradução por igualdade. `COLADA` exige testemunha de mesmo tipo dentro de ±60s; paridade distante perde esse texto. |
| `templates/chamados/partials/modal_fio.html:229` | Exibe a espera nos ramos validado e não validado; segundo ponto em `:243`. |
| `chamados/tests/test_contract_leitores_vitalidade.py:124` | Atualiza o censo para o corpo avulso extraído e a elegibilidade do par. |

Os escritores e leitores dos campos envolvidos ficam assim:

| Campo/função | Quem escreve | Quem lê |
|---|---|---|
| `resposta_colab/respondida_em` | Porta existente, `chamados/services/disputa_emissao.py:1422` | Derivação `materializacao.py:384`; prontidão `:1383`. |
| `validada_em/por` e decisões E1 | Dono existente, `validacao.py:127` e `:163`; sequência em `chamados/models.py:3943` | Dispatcher `materializacao.py:1383`; C6 `chamados/juizes.py:1153`; consulta `chamados/models.py:3359`. |
| `recusa_motivo` | `materializacao.py:1144`; limpeza após aplicação em `:1352` | Devolução/espera `validacao.py:193`; catálogo `recusa.py:92`; apresentação `materializacao.py:479`. |
| Batida: instante, tipo e origem | Porta existente `ponto/registro_batida.py:94` | Guarda `materializacao.py:139`; lastro existente `chamados/juizes.py:645`. |
| `pergunta_origem` | `anotar`, chamado em `materializacao.py:1008`; porta em `ponto/registro_batida.py:235` | Lastro `chamados/juizes.py:703`. |
| Materialização, via e lavra | `materializacao.py:1220` → `chamados/models.py:3103` → `chamados/veredito_lavrado.py:234` | Identidade `materializacao.py:55`; estado `:479`; seleção `:1557`; estado do modelo `chamados/models.py:3035`. |
| `pendencia_admin` | Ramo existente `materializacao.py:910` | Template `templates/chamados/partials/modal_fio.html:221`. |
| Estado de aplicação | `materializacao.py:479`, montado por `chamados/services/fio.py:336` | Template `modal_fio.html:229`; casca da validação `chamados/services/acoes_disputa.py:13`. |
| `processadas/aplicadas/pares` | Dispatcher `materializacao.py:1372` | Varredura `:1568`; O330 `passivo_validado.py:570`; consolidação `:385`; saída JSON existente `chamados/management/commands/materializar_passivo_validado.py:135`. |
| Fechamento | Cauda existente `materializacao.py:1490` → `chamados/models.py:1863` | Autoridade `pode_fechar`, `chamados/models.py:1856`. |

O primeiro aceite permanece validado e aparece com “Falta a outra metade do intervalo”. Para C6, essa primeira pergunta continua morta pelo degrau de validação; a outra metade segura o fechamento nominal. O RED cobra isso, inclusive executando callbacks após o primeiro aceite. Fontes: `validacao.py:482`, `chamados/juizes.py:1153`, `chamados/models.py:1856`, [test_bug_par_intervalo.py:104](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/test_bug_par_intervalo.py:104) e `:164`. Resultado funcional: **[nao verificado]**.

O RED cobre R1–R9, rollback da segunda escrita, lastro, reconciliação, proteção em voo, travas e limites do O330. R7 usa 20/09 porque o índice parcial impede duplicar os motivos na data nominal. Fontes: `chamados/tests/test_bug_par_intervalo.py:35`, `:151`, `:182`, `:255`, `:342`, `:390`, `:401`, `:418`; índice em `chamados/models.py:3482`.

Comando reservado à sessão para RED na base e GREEN na cura: `chamados/tests/test_bug_par_intervalo.py:1`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo --noinput
```

Cada mutante altera o fonte real **em memória** e executa seu teste-alvo. Os comandos individuais abaixo correspondem às definições de [mutantes_bug_par_intervalo.py:17](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/mutantes_bug_par_intervalo.py:17). Não foram executados; falha de ambiente não demonstra mutante morto. Fonte: `mutantes_bug_par_intervalo.py:1`. [nao verificado]

```bash
mutar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo "$1"
}
mutar M1  # remove atomic do par
mutar M2  # remove espera
mutar M3  # restaura guarda individual
mutar M4  # remove paridade final
mutar M5  # remove ordem temporal
mutar M6  # remove lastro
mutar M7  # escolhe metades ambiguas
mutar M8  # remove testemunha de colada
mutar M9  # remove par canonico
mutar M10 # remove devolucao das validacoes
mutar M11 # reconcilia entre metades
mutar M12 # conta apenas uma aplicacao
mutar M13 # zera contador
mutar M14 # remove protecao em voo
mutar M15 # remove trava das perguntas
mutar M16 # aplica fora do universo O330
mutar M17 # repara carimbo pela porta normal
mutar M18 # ignora disputas adicionais
```

DECISAO-ABERTA: nenhuma escolha adicional de identidade foi introduzida; ambiguidades seguem pelo caminho avulso, conforme `materializacao.py:55` e `:1400`. Permanecem **[nao verificado]**: execução RED/GREEN, mortalidade dos mutantes, concorrência real, renderização Django, censo populacional e commit introdutor. [nao verificado]

Log final: executei o comando abaixo, que usa `py_compile` sem persistir bytecode; sete arquivos Python compilaram e os 18 mutantes tiveram âncora única, sintaxe e teste-alvo conferidos. Implementação da checagem: `chamados/tests/mutantes_bug_par_intervalo.py:87`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

```text
py_compile: 7 arquivos, rc 0, sem persistencia
M1–M18: ancora unica, sintaxe e teste-alvo conferidos [nao verificado]
RED/GREEN: NAO EXECUTADOS
Mutantes funcionais: NAO EXECUTADOS
```

## 09/11 08:12 · sessao 79c1c84e8d87 · - · - · origem=Ronald (tmux)

Encontrei dois problemas concretos:

- **Ordem de travas no O330:** a transação percorre várias disputas, mas trava cada disputa apenas ao chegar à unidade. Uma segunda disputa fechada pode ser travada depois das perguntas da primeira. Precisa reunir todas as disputas, destinos, clones e perguntas no topo da transação. `chamados/services/passivo_validado.py:371`; `chamados/services/passivo_validado.py:429`; `chamados/services/materializacao.py:90`; `chamados/services/materializacao.py:1372`.
- **Aplicação fora do universo do reparador:** a identidade admite irmã carimbada sem lastro; essa irmã pode ter sido excluída do lote por `plantio_fora_do_slot`. A unidade a aplica mesmo assim, e o contador só contabiliza perguntas do grupo original. Precisa exigir autorização das duas metades pelo universo do lote, recusando o conjunto se faltar uma. `chamados/services/materializacao.py:73`; `chamados/services/materializacao.py:1329`; `chamados/services/passivo_validado.py:330`; `chamados/services/passivo_validado.py:523`; `chamados/services/vigia_carimbo.py:113`.

O segundo problema também amplia o universo da porta normal: uma volta já carimbada sem lastro volta a ser aplicada ao validar a saída, embora a seleção normal exclua perguntas materializadas. O teste R9 atual cobre apenas motivo retratado. `chamados/services/materializacao.py:73`; `chamados/services/materializacao.py:1542`; `chamados/tests/test_bug_par_intervalo.py:310`.

O R8 atual usa perguntas sem carimbo; falta cobrir o formato real do passivo com carimbo falso e a irmã fora do universo. `chamados/tests/test_bug_par_intervalo.py:297`; `chamados/tests/test_o330_plantio_nao_abre_turno.py:101`.

Revisão por leitura; testes não executados. [nao verificado] [nao verificado]

## 09/11 08:04 · sessao 47aa24b5876b · DEPLOY-EXCLUSAO-ENVELOPE · verificador · origem=Code

**Não daria VERDE irrestrito nesta volta.** P2 ainda permite alarme sem compartilhada quando a biblioteca não carrega; o marcador de P1 pode sobreviver ao envelope e provocar falsa recusa; o RELATO continua publicando parâmetros anteriores. (`bin/cron_run.sh:54`, `app/core/esteira_vigia.py:917`, `bin/trava_deploy_exclusao.sh:61`, `app/docs/RELATO.md:25`)

Resultados executados do protocolo, RED/GREEN, m1–m5 e ocorrência dos cenários em produção: **[nao verificado]**. O selo cria, altera e remove arquivos, portanto sua execução não cabe nesta revisão somente leitura. (`bin/tests/test_deploy_exclusao_cron.sh:60`, `bin/tests/test_deploy_exclusao_cron.sh:78`, `bin/tests/test_deploy_exclusao_cron.sh:170`)

1. **P1–P6: cura efetiva e limites**

| Item | Caminho corrigido | O que permanece aberto |
|---|---|---|
| **P1 — parcial** | Envelope toma compartilhada e exporta marcador; deploy com migration chega à aquisição exclusiva, reconhece marcador e retorna 2 sem executar o `flock`; deploy traduz isso em recusa rc 1, nomeando envelope e trava. Remove a espera de 600 s contra o ancestral nesse caminho. (`bin/trava_deploy_exclusao.sh:57`, `bin/trava_deploy_exclusao.sh:58`, `bin/trava_deploy_exclusao.sh:61`, `bin/deploy.sh:328`, `bin/deploy.sh:330`, `bin/deploy.sh:95`) | Marcador não prova posse atual: não valida PID nem compartilhada. Há caminho assíncrono concreto descrito abaixo. A recusa também ocorre somente depois dos portões anteriores, não imediatamente na entrada do script. (`bin/trava_deploy_exclusao.sh:61`, `bin/deploy.sh:308`, `bin/deploy.sh:325`) |
| **P2 — parcial** | Timeout com biblioteca carregada → fecha fd 4 → RC75/placar → readquire compartilhada sem teto → chama alarme → grava FLAG se a chamada funcionar. Assim o alarme espera a exclusiva sair. (`bin/trava_deploy_exclusao.sh:53`, `bin/trava_deploy_exclusao.sh:64`, `bin/cron_run.sh:28`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`) | Biblioteca ausente → função inexistente → `! declare -F` permite o `docker exec` sem compartilhada. Falha na readquisição ou no Docker também não tem retry nesse epílogo; a FLAG não nasce. (`bin/cron_run.sh:17`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`) |
| **P3 — corrigido no caminho que executa comando** | `ini` e `T0` agora nascem depois da aquisição; espera fica em campo separado. Elimina a inclusão da fila em `fim−ini` daquele caminho. (`bin/cron_run.sh:17`, `bin/cron_run.sh:19`, `bin/cron_run.sh:20`, `bin/cron_run.sh:31`) | SEM VEZ também recebe `ini`, embora nenhum comando comece; `dur` inclui preparação da marca. Os consumidores continuam contando tentativas recusadas, conforme detalhado adiante. (`bin/cron_run.sh:23`, `bin/cron_run.sh:26`, `bin/cron_run.sh:31`, `app/core/management/commands/pipeline_placar.py:74`) |
| **P4 — corrigido para os filhos escolhidos** | Docker de top/HUP, árvore de disponibilidade e Docker da prova recebem fd 4 fechado. Fecha a herança pelo neto da prova contemplado pelo selo. (`bin/deploy.sh:391`, `bin/deploy.sh:395`, `bin/deploy.sh:417`, `bin/deploy.sh:436`) | Não equivale a fechar em todo filho; pipelines auxiliares e `sleep` continuam herdando. Migrate/restart ficam deliberadamente abertos. Morte do deploy durante essas operações não é provada pelo caso f. (`bin/deploy.sh:351`, `bin/deploy.sh:378`, `bin/deploy.sh:401`, `bin/deploy.sh:405`, `bin/tests/test_deploy_exclusao_cron.sh:304`) |
| **P5 — asserção acrescentada; mordida não certificada** | Caso g introduz provas lentas e exige três provas terminadas antes do comando, em normal e restart. (`bin/tests/test_deploy_exclusao_cron.sh:104`, `bin/tests/test_deploy_exclusao_cron.sh:320`, `bin/tests/test_deploy_exclusao_cron.sh:333`) | Não comprova que o cron chegou durante a prova; definições/resultados dos mutantes não foram fornecidos. Mordida observada: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:324`, `bin/tests/test_deploy_exclusao_cron.sh:329`) |
| **P6 — parcial documental** | O contrato v3 anexado descreve 300/600, retomada sem teto, marcador e `espera_deploy`. (`contrato_v3.txt:3`, `contrato_v3.txt:5`, anexo) | RELATO ainda contém cron600/deploy não bloqueante/`espera_lock`/dois contadores; acima há somente resumo de v2. Envio “uma vez” e publicação física do v3: **[nao verificado]**. (`app/docs/RELATO.md:21`, `app/docs/RELATO.md:25`) |

2. **Aninhamento: existe caminho legítimo para marcador obsoleto**

O caminho encontrado é condicional, mas está no código:

- `vigia_arvore` é RAW envelopado; quando passa a vermelho, chama `esteira_vigia.py curar`. (`app/config/crons.py:296`, `app/config/crons.py:1863`, `bin/vigia_arvore.sh:172`, `bin/vigia_arvore.sh:176`)
- **`curar` não é opção reconhecida por `main()`**: cai em `passada()`. O auto-revert possui outra opção, `auto-revert`, e também pode ser acionado pela passada em `cura_sem_efeito`. (`app/core/esteira_vigia.py:1578`, `app/core/esteira_vigia.py:1606`, `app/core/esteira_vigia.py:1549`, `app/core/esteira_vigia.py:1552`)
- A passada pode escolher um relance e chamar `relancar → _lancar`. Se `systemd-run` falhar, `_lancar` usa `Popen(nohup bash esteira.sh, start_new_session=True)`, sem substituir ambiente nem preservar fd 4 explicitamente. O processo destacado pode conservar o marcador depois que o envelope termina. (`app/core/esteira_vigia.py:542`, `app/core/esteira_vigia.py:562`, `app/core/esteira_vigia.py:1516`, `app/core/esteira_vigia.py:936`, `app/core/esteira_vigia.py:913`, `app/core/esteira_vigia.py:917`)
- Existe esteira legítima que chega a deploy: o código cria fatias pelo molde de autocura; seu `esteira.sh` chama `cadeia.sh`, que chama deploy. Se chegar depois da saída do envelope e houver migration pendente, o marcador antigo provoca falsa recusa de aninhamento. (`app/core/esteira_vigia.py:1426`, `app/core/esteira_vigia.py:1429`, `bin/molde_autocura/esteira.sh:23`, `bin/molde_autocura/cadeia.sh:86`, `bin/deploy.sh:325`, `bin/trava_deploy_exclusao.sh:61`)

Falha efetiva do `systemd-run`, duração desse descendente e migration pendente nessa ocasião: **[nao verificado]**.

Nos demais caminhos pedidos:

| Caminho | Resultado da leitura |
|---|---|
| Humano | Deploy não limpa nem valida a procedência do marcador; se recebido, basta estar preenchido para recusar a aquisição exclusiva. Shell interativo filho efetivamente lançado por esses fluxos: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:61`) |
| `deploy_agendado` | A linha gravada em cron.d não transporta o ambiente do agendador; `rodar` executa esteiras sincronamente. Uma chamada direta de `rodar` tampouco limpa marcador herdado. (`bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:56`, `bin/deploy_agendado.sh:73`) |
| Reload 03:30 | É envelopado, mas recusa migration pendente e, no HUP puro, não pede exclusiva. (`app/config/crons.py:326`, `bin/deploy.sh:214`, `bin/deploy.sh:325`) |
| Cadeia/esteira | O caminho destacado acima alcança a cadeia de autocura. A cadeia chama deploy sem limpar o marcador. (`app/core/esteira_vigia.py:917`, `bin/molde_autocura/esteira.sh:23`, `bin/molde_autocura/cadeia.sh:86`) |
| `pos_push` | O script atual atualiza e commita TICKETS; não contém porta direta de deploy. (`bin/pos_push.sh:19`, `bin/pos_push.sh:29`, `bin/pos_push.sh:39`) |

O único escritor funcional localizado é o `export` da biblioteca; o único leitor decisório é o teste de variável não vazia. Deploy também a imprime. `solta_trava_deploy` apenas destrava/fecha fd, sem limpar marcador. (`bin/trava_deploy_exclusao.sh:58`, `bin/trava_deploy_exclusao.sh:61`, `bin/trava_deploy_exclusao.sh:65`, `bin/deploy.sh:331`)

Ainda em P1/F6: os deploys de `auto_revert` usam `capture_output=True` e descartam o retorno. Portanto a recusa emitida pelo deploy não está demonstrada como visível no log desse chamador, apesar do comentário novo. (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1275`, `bin/deploy.sh:321`)

3. **Epílogo: espera infinita possível; ciclo de deadlock não demonstrado**

**Sim, o cron pode ficar indefinidamente no epílogo.** A retomada chama `flock -s` sem `-w`; migrate e prova de rota não têm timeout. A exclusiva só é solta no caminho normal posterior às provas. (`bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:64`, `bin/deploy.sh:351`, `bin/deploy.sh:433`, `bin/deploy.sh:456`)

Isso demonstra retenção ilimitada, não um ciclo por si só. No trecho sob exclusiva, a consulta de cron do restart **recusa imediatamente**, sem esperar o envelope; os demais passos esperam workers, disponibilidade e rotas. Não encontrei nesse percurso exclusiva → espera cron → espera compartilhada. Bloqueios externos de banco/runtime: **[nao verificado]**. (`bin/deploy.sh:113`, `bin/deploy.sh:115`, `bin/deploy.sh:377`, `bin/deploy.sh:400`, `bin/deploy.sh:417`, `bin/deploy.sh:433`)

O percurso do alarme é `cron_quebrado.handle → avisar_quebra → escrever`: cria Pauta, grava trilha em LogAuditoria e atualiza ContadorPauta. Recuperação atualiza Pauta, trilha e contador. Não encontrei chamada a deploy ou outro `cron_run` nesse percurso de escrita. (`app/core/management/commands/cron_quebrado.py:22`, `app/core/management/commands/cron_quebrado.py:37`, `app/core/services/crons_quebrados.py:71`, `app/pautas/services.py:180`, `app/pautas/services.py:186`, `app/pautas/services.py:188`, `app/pautas/services.py:272`, `app/core/observ.py:133`)

Há também **tempo invisível**: placar, `fim`, `dur` e `espera_deploy` são gravados antes da retomada ilimitada. O envelope pode continuar vivo por tempo arbitrário depois do `fim` registrado. (`bin/cron_run.sh:18`, `bin/cron_run.sh:30`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`)

4. **`ini`, leitores e histórico**

A alteração restaura o sentido aproximado anterior para comandos executados: início depois da fila, ainda antes da preparação da marca. Não restaura literalmente “início do comando” para SEM VEZ, pois nesse ramo não existe comando iniciado. (`bin/cron_run.sh:19`, `bin/cron_run.sh:23`, `bin/cron_run.sh:25`, `bin/cron_run.sh:26`)

| Leitor | Consequência |
|---|---|
| `corrida_no_horario` | Compara o minuto de `ini` com tolerância `<3`. Um disparo legítimo que espere 3–5 minutos pela trava pode sair da medição de duração. (`app/config/crons.py:2304`, `app/config/crons.py:2329`, `app/config/crons.py:2332`, `bin/trava_deploy_exclusao.sh:37`) |
| `crons_duracao` | Filtra data/horário de `ini`, mas aceita `dur` sem filtrar exit. Tentativa SEM VEZ pode entrar como amostra curta. (`bin/crons_duracao.py:69`, `bin/crons_duracao.py:71`, `bin/crons_duracao.py:73`, `bin/crons_duracao.py:75`) |
| `pipeline_placar` | Agrupa pelo dia de `ini`; toda linha incrementa `rodou`, inclusive comando recusado; 75 incrementa erros. Espera atravessando meia-noite pode mudar o dia de atribuição. (`app/core/management/commands/pipeline_placar.py:70`, `app/core/management/commands/pipeline_placar.py:74`, `app/core/management/commands/pipeline_placar.py:78`) |
| `crons_quebrados` | Usa `ini` na janela de 26 horas e na seleção da corrida mais recente; 75 continua erro. (`app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`) |

O escritor apenas apenda; não regrava histórico. Esses leitores não normalizam versões usando `espera_deploy`. Portanto, **se existirem linhas da v1**, continuam interpretadas literalmente com o sentido antigo de chegada, enquanto novas linhas usam o instante posterior à aquisição. Existência e quantidade dessas linhas em produção: **[nao verificado]**. (`bin/cron_run.sh:31`, `bin/crons_duracao.py:69`, `app/core/management/commands/pipeline_placar.py:70`, `app/core/services/crons_quebrados.py:36`)

Na busca por `placar.jsonl`, não localizei outro leitor operacional direto além desses; as referências de `vigiar_fio_morto` e `sombra.sh` são explicativas. (`app/chamados/management/commands/vigiar_fio_morto.py:19`, `bin/sombra.sh:122`)

5. **fd 4: cobertura útil, sem garantia universal**

Os fechamentos cobrem as chamadas Docker apontadas por P4 e toda a árvore iniciada por `timeout` na disponibilidade. Contudo, `awk`, `tr`, `sed`, `tail`, substituições de comando e o `sleep 2` não receberam fechamento próprio. Logo “todo filho nasce sem fd 4” seria falso; não localizei lançamento explicitamente destacado desses auxiliares sob a exclusiva. Sobrevivência efetiva após morte do deploy: **[nao verificado]**. (`bin/deploy.sh:391`, `bin/deploy.sh:401`, `bin/deploy.sh:405`, `bin/deploy.sh:417`, `bin/deploy.sh:436`)

**Migrate e Docker restart conservarem fd é coerente com a intenção declarada:** enquanto esses clientes conservarem o descritor, podem conservar a exclusão após a saída do shell. Isso não prova a promessa mais forte de exclusão enquanto qualquer trabalho interno ao container sobreviver; o selo observa o fd do Docker falso no host. Essa garantia dentro do container: **[nao verificado]**. (`bin/deploy.sh:94`, `bin/deploy.sh:351`, `bin/deploy.sh:378`, `bin/tests/test_deploy_exclusao_cron.sh:92`)

O caso f verifica diretamente fd fechado nos filhos classificados e aberto no migrate. Porém executa deploy com sucesso, chegando ao unlock explícito; não mata o deploy durante migrate/restart. Assim não certifica F1 nem a retenção depois de abandono anormal. (`bin/tests/test_deploy_exclusao_cron.sh:304`, `bin/tests/test_deploy_exclusao_cron.sh:307`, `bin/tests/test_deploy_exclusao_cron.sh:310`, `bin/deploy.sh:456`)

6. **Selo, m1–m5 e casos vazios**

**m1, m2, m3, m4 e m5: [nao verificado] individualmente.** Não foram fornecidas suas definições nem resultados; o selo aceita fontes alternativas, mas isso não identifica qual transformação corresponde a cada nome. (`bin/tests/test_deploy_exclusao_cron.sh:43`, `bin/tests/test_deploy_exclusao_cron.sh:44`, `bin/tests/test_deploy_exclusao_cron.sh:45`)

Há asserções dirigidas às propriedades: b2 cobra alarme depois da última prova e FLAG; e cobra recusa aninhada rápida e identificada; f cobra descritores; g cobra comando depois das três provas. Capacidade observada de cada mutante violá-las: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:217`, `bin/tests/test_deploy_exclusao_cron.sh:293`, `bin/tests/test_deploy_exclusao_cron.sh:310`, `bin/tests/test_deploy_exclusao_cron.sh:333`)

Quanto ao **RED de g**, o delta fornecido não move a soltura: a v1 já a fazia depois das provas. Portanto não há soltura prematura real demonstrada na v1; o novo caso protege essa propriedade contra regressão introduzida. RED executado contra a base sem exclusão ou contra mutante de soltura antecipada: **[nao verificado]**. (`diff_v2_delta.diff`, anexo; `bin/deploy.sh:451`, `bin/deploy.sh:456`)

Há lacunas concretas:

- **d3 pode passar sem comprovar seu cenário:** ignora retorno da espera por `CMD-INI` e do `wait`; basta qualquer falha antecipada do deploy e zero restart. Não exige diagnóstico de recusa por cron nem sucesso do cron. (`bin/tests/test_deploy_exclusao_cron.sh:278`, `bin/tests/test_deploy_exclusao_cron.sh:280`, `bin/tests/test_deploy_exclusao_cron.sh:282`)
- **g exige provas e comando presentes**, mas não sobreposição. Os retornos de espera são ignorados; se o cron só for escalonado depois das provas, passa sem disputar aquela janela. (`bin/tests/test_deploy_exclusao_cron.sh:324`, `bin/tests/test_deploy_exclusao_cron.sh:327`, `bin/tests/test_deploy_exclusao_cron.sh:329`, `bin/tests/test_deploy_exclusao_cron.sh:333`)
- **b1 aceita folga negativa arbitrária:** cobra somente `(fim−ini)−dur <=1`, sem limite inferior nem confronto com timestamp do comando. (`bin/tests/test_deploy_exclusao_cron.sh:144`, `bin/tests/test_deploy_exclusao_cron.sh:197`)
- **d4 verifica desigualdade numérica**, não orçamento completo de deploy. (`bin/tests/test_deploy_exclusao_cron.sh:344`)

7. **Classes anteriores de reprovação**

| Classe | Situação nesta cópia |
|---|---|
| Segunda fonte do caminho | Uma definição funcional localizada. Entretanto, deriva da biblioteca carregada: deploy usa sua própria raiz e envelope usa a viva absoluta. Cópias distintas continuam podendo disputar arquivos distintos enquanto chamam os mesmos containers; não há guarda de raiz viva nesses trechos. (`bin/trava_deploy_exclusao.sh:32`, `bin/deploy.sh:53`, `bin/deploy.sh:326`, `bin/cron_run.sh:17`, `bin/deploy.sh:351`) |
| Derivação paralela | Aquisição, tetos e fd estão centralizados. Já a inferência “marcador preenchido = ancestral ainda segura compartilhada” não é validada e falha no caminho destacado acima. (`bin/trava_deploy_exclusao.sh:44`, `bin/trava_deploy_exclusao.sh:61`, `app/core/esteira_vigia.py:917`) |
| Selo sem mordida | Novas asserções existem; d3 permanece permissivo e g não assegura sobreposição. Mordida de m1–m5: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:282`, `bin/tests/test_deploy_exclusao_cron.sh:333`) |
| Ordem de trava | Cadeia toma árvore, régua e depois exclusiva. A autoespera compartilhada → exclusiva recebe recusa; não demonstrei ciclo inverso sob exclusiva. (`bin/molde_autocura/cadeia.sh:10`, `bin/molde_autocura/cadeia.sh:13`, `bin/molde_autocura/cadeia.sh:86`, `bin/trava_deploy_exclusao.sh:61`) |
| Relógio | Espera e duração usam relógio de parede. Mesmo sem contenção, cruzar um segundo entre os dois `date` pode produzir espera 1; d2 exige exatamente zero. Saltos efetivos de relógio: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:49`, `bin/trava_deploy_exclusao.sh:51`, `bin/cron_run.sh:20`, `bin/cron_run.sh:30`, `bin/tests/test_deploy_exclusao_cron.sh:268`) |
| ESMERIL — `espera_deploy` | Mede aquisição inicial, exclui retomada do alarme e não identifica causa de 75. Um comando pode esperar, obter vez e devolver 75; falha imediata pode registrar zero. (`bin/cron_run.sh:18`, `bin/cron_run.sh:25`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`, `bin/trava_deploy_exclusao.sh:46`) |
| ESMERIL — `ini`/`dur` | Sucesso recupera início depois da fila; recusa recebe início sem comando; duração inclui preparação e exclui epílogo. O leitor `rodou` continua somando tentativa sem execução. (`bin/cron_run.sh:19`, `bin/cron_run.sh:23`, `bin/cron_run.sh:26`, `bin/cron_run.sh:54`, `app/core/management/commands/pipeline_placar.py:74`) |

## 09/11 08:01 · sessao ac563cc2105e · - · - · origem=Ronald (tmux)

**Há caminho legítimo no código para marcador herdado sobreviver ao envelope, condicionado à falha de `systemd-run`; ocorrência em produção [nao verificado].** O teste usa somente variável não vazia, sem validar PID, arquivo ou posse da compartilhada. (`bin/trava_deploy_exclusao.sh:61`)

Caminho completo:

1. O cron agenda `vigia_arvore.sh`, envelopado pelo gerador de crontab. (`app/config/crons.py:296`, `app/config/crons.py:1863`)
2. O envelope exporta o marcador e chama o comando sincronamente. (`bin/trava_deploy_exclusao.sh:58`, `bin/cron_run.sh:25`)
3. Na transição para vermelho, o script chama **`esteira_vigia.py curar`**. (`bin/vigia_arvore.sh:172`, `bin/vigia_arvore.sh:176`)
4. **`curar` não é opção tratada em `main()`**: a opção de auto-revert se chama `auto-revert`; `curar` cai na chamada geral de `passada()`. (`app/core/esteira_vigia.py:1577`, `app/core/esteira_vigia.py:1578`, `app/core/esteira_vigia.py:1606`)
5. A passada pode selecionar um relance e chamar `relancar()`, que chama `_lancar()`. (`app/core/esteira_vigia.py:542`, `app/core/esteira_vigia.py:562`, `app/core/esteira_vigia.py:1513`, `app/core/esteira_vigia.py:1516`, `app/core/esteira_vigia.py:936`)
6. `_lancar()` tenta `systemd-run`; **se ele falhar**, usa `Popen(['nohup','bash','esteira.sh'], start_new_session=True)`, sem fornecer ambiente substituto nem `pass_fds`. O descendente herda o marcador e pode continuar depois do retorno do Python e da saída do envelope; sua efetiva duração nessa produção [nao verificado]. (`app/core/esteira_vigia.py:913`, `app/core/esteira_vigia.py:915`, `app/core/esteira_vigia.py:917`, `bin/cron_run.sh:59`)
7. Há fatia legítima capaz de chegar a deploy: o próprio código cria a partir de `bin/molde_autocura` e enfileira; seu `esteira.sh` chama `cadeia.sh`, que chama deploy. (`app/core/esteira_vigia.py:1414`, `app/core/esteira_vigia.py:1417`, `app/core/esteira_vigia.py:1424`, `app/core/esteira_vigia.py:1426`, `app/core/esteira_vigia.py:1428`, `bin/molde_autocura/esteira.sh:23`, `bin/molde_autocura/cadeia.sh:86`)
8. Se esse deploy precisar de migration, o marcador antigo o faz recusar como aninhado, mesmo depois do envelope ancestral terminar. Pendência efetiva de migration nesse cenário [nao verificado]. (`bin/deploy.sh:236`, `bin/deploy.sh:325`, `bin/trava_deploy_exclusao.sh:61`, `bin/deploy.sh:329`)

**Limites e distinções:**

- O molde de fatia moderno entrega ao integrador e não chama cadeia/deploy nesse fluxo; o caminho completo acima usa o molde de autocura ainda criado pelo código. (`bin/molde_fatia/esteira.sh:24`, `bin/molde_fatia/esteira.sh:26`, `app/core/esteira_vigia.py:1426`)
- O caminho síncrono para auto-revert existe na passada, no ramo `cura_sem_efeito`; não é correto dizer que a opção literal `curar` chama diretamente `auto_revert()`. (`app/core/esteira_vigia.py:1549`, `app/core/esteira_vigia.py:1552`, `app/core/esteira_vigia.py:1578`, `app/core/esteira_vigia.py:1606`)
- `auto_revert()` chama ambos os deploys com `subprocess.run`; enquanto essa chamada síncrona provém do envelope, o ancestral continua vivo. Os retornos são descartados; `capture_output=True` também captura a recusa sem publicá-la nesse trecho, portanto o comentário “a recusa fala no log dele” não está demonstrado pelo chamador. (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1268`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1275`, `bin/deploy.sh:321`)
- `deploy_agendado` agenda uma nova execução direta em `/etc/cron.d` e roda as esteiras sincronamente. Agendar dentro de um envelope não grava esse marcador na linha gerada; invocar diretamente `rodar` em ambiente já contaminado tampouco o limpa. (`bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:56`, `bin/deploy_agendado.sh:73`)
- Reload 03:30 é envelopado, mas recusa migration pendente e evita aquisição exclusiva quando faz somente HUP. (`app/config/crons.py:326`, `app/config/crons.py:1863`, `bin/deploy.sh:214`, `bin/deploy.sh:325`)
- `pos_push.sh` atual escreve/commita TICKETS e termina; não é porta direta de deploy. (`bin/pos_push.sh:18`, `bin/pos_push.sh:19`, `bin/pos_push.sh:20`, `bin/pos_push.sh:29`, `bin/pos_push.sh:39`)
- Shell interativo destacado com marcador antigo é possível em tese, mas não identifiquei lançamento desse tipo nos caminhos lidos: **[nao verificado]**.
- O observador de árvore é assíncrono, fecha fds 6–9 e conserva 4 se herdado, mas apenas espera o pai e grava evento; não chama deploy. (`bin/trava.sh:67`, `bin/trava.sh:68`, `bin/trava.sh:69`)
- O fabricante também lança esteira destacada, porém caminho de cron até ele não foi demonstrado: **[nao verificado]**. (`bin/fabricante.sh:187`)

O único escritor funcional localizado do marcador é o export da biblioteca; o único leitor decisório é seu teste de não vazio; deploy apenas o imprime. Não existe limpeza no `solta_trava_deploy()`, que somente destrava/fecha o fd. (`bin/trava_deploy_exclusao.sh:58`, `bin/trava_deploy_exclusao.sh:61`, `bin/trava_deploy_exclusao.sh:65`, `bin/deploy.sh:331`, `bin/deploy.sh:332`)

## 09/11 08:01 · sessao 76b19fbb493b · - · - · origem=Ronald (tmux)

P2 está curado no caminho normal de timeout com biblioteca carregada: aquisição compartilhada falha, fd4 fecha, comando não roda, RC75 entra no placar e o epílogo readquire compartilhada sem teto antes do `docker exec cron_quebrado`; FLAG nasce somente com retorno zero. (`bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:53`, `bin/trava_deploy_exclusao.sh:64`, `bin/cron_run.sh:17`, `bin/cron_run.sh:28`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`)

**P2 não está fechado para biblioteca ausente/ilegível:** esse ramo também é oficialmente SEM VEZ/75; porém a condição `! declare -F trava_alarme_compartilhada` libera o `docker exec` sem readquirir qualquer compartilhada. Ocorrência em produção: [nao verificado]. (`bin/cron_run.sh:10`, `bin/cron_run.sh:16`, `bin/cron_run.sh:17`, `bin/cron_run.sh:28`, `bin/cron_run.sh:54`)

**Pode esperar para sempre:** a readquisição é `flock -s` sem `-w`; o deploy pode conservar exclusiva dentro do migrate ou de uma prova sem timeout. O cron fica vivo no epílogo, sem FIM e sem MARCA no ramo SEM VEZ. Isso é retenção ilimitada demonstrável, não prova de ciclo de deadlock. (`bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:64`, `bin/deploy.sh:351`, `bin/deploy.sh:433`, `bin/deploy.sh:456`, `bin/cron_run.sh:21`, `bin/cron_run.sh:40`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`)

No trecho com exclusiva, não encontrei deploy esperando um cron/envelope: o segundo `recusa_restart_com_cron` consulta marcas e falha imediatamente; troca de workers e provas esperam workers/HTTP, não carimbos da corrente. Não há ciclo concreto deploy exclusiva → espera cron → espera compartilhada nesses caminhos lidos. Bloqueios externos de banco/runtime: [nao verificado]. (`bin/deploy.sh:113`, `bin/deploy.sh:115`, `bin/deploy.sh:377`, `bin/deploy.sh:400`, `bin/deploy.sh:417`, `bin/deploy.sh:433`)

O escritor do alarme segue `cron_quebrado.handle → avisar_quebra → pautas.services.escrever`: consulta User/Empresa/Pauta, cria Pauta, escreve LogAuditoria e atualiza ContadorPauta. Recuperação atualiza Pauta, trilha e contador. Não encontrei chamada a deploy/cron_run nesse percurso nem receptor de sinais para Pauta/ContadorPauta/LogAuditoria; os modelos lidos não sobrescrevem `save`. Registro dinâmico externo: [nao verificado]. (`app/core/management/commands/cron_quebrado.py:22`, `app/core/management/commands/cron_quebrado.py:35`, `app/core/management/commands/cron_quebrado.py:37`, `app/core/services/crons_quebrados.py:63`, `app/core/services/crons_quebrados.py:66`, `app/core/services/crons_quebrados.py:71`, `app/core/services/crons_quebrados.py:80`, `app/pautas/services.py:180`, `app/pautas/services.py:186`, `app/pautas/services.py:188`, `app/pautas/services.py:272`, `app/pautas/services.py:344`, `app/pautas/services.py:369`, `app/core/observ.py:133`, `app/pautas/models.py:67`, `app/pautas/models.py:143`)

P3 restaura `ini` após aquisição no caminho que executa CMD; conserva assinatura legível pelos leitores existentes. Literalmente, `ini` antecede mkdir/MARCA e `dur` inclui essa preparação. No SEM VEZ não existe início de comando: `ini` representa o instante após aquisição recusada; `dur` mede o curto ramo de recusa. Logo “ini = início do COMANDO” ainda não descreve todas as linhas. (`bin/cron_run.sh:17`, `bin/cron_run.sh:19`, `bin/cron_run.sh:20`, `bin/cron_run.sh:23`, `bin/cron_run.sh:25`, `bin/cron_run.sh:26`, `bin/cron_run.sh:31`)

| Leitor real | Efeito |
|---|---|
| `crons_quebrados` | Usa `ini` para janela de 26h e corrida mais recente; 75 continua erro. Mudança de ordenação possível quando há linhas concorrentes e semânticas históricas distintas. (`app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`) |
| `pipeline_placar` | Usa data de `ini`; cada linha soma `rodou`, inclusive SEM VEZ; 75 soma erros; `dur` entra no máximo. Espera cruzando meia-noite desloca a tentativa ao dia do fim da aquisição. (`app/core/management/commands/pipeline_placar.py:70`, `app/core/management/commands/pipeline_placar.py:74`, `app/core/management/commands/pipeline_placar.py:78`, `app/core/management/commands/pipeline_placar.py:80`) |
| `crons_duracao` | Usa data e filtro de horário de `ini`; inclui qualquer `dur` inteiro, sem filtrar exit. SEM VEZ pode contribuir amostra curta se estiver no horário. (`bin/crons_duracao.py:69`, `bin/crons_duracao.py:71`, `bin/crons_duracao.py:73`, `bin/crons_duracao.py:75`) |
| `corrida_no_horario` | Tolerância padrão de 3 minutos, comparação `<3`; comando atrasado pela trava em 3–5 minutos pode sair da medição mesmo sendo disparo agendado legítimo. (`app/config/crons.py:2304`, `app/config/crons.py:2329`, `app/config/crons.py:2332`, `bin/trava_deploy_exclusao.sh:37`) |

Não achei outro leitor operacional direto de JSONL pelo grep de `placar.jsonl`; `vigiar_fio_morto` apenas comenta o arquivo, e `sombra.sh` referencia `pipeline_placar`. Não há consumo operacional de `espera_deploy` nesses leitores. (`app/chamados/management/commands/vigiar_fio_morto.py:19`, `bin/sombra.sh:122`, `bin/crons_duracao.py:69`, `app/core/management/commands/pipeline_placar.py:70`, `app/core/services/crons_quebrados.py:36`)

Não há migração/regravação histórica no escritor: ele só apenda JSON. Linhas antigas continuam interpretadas pelo seu `ini` literal; leitores não usam versão nem `espera_deploy` para normalizar registros v1/v2. Existência de registros v1 em produção: [nao verificado]. (`bin/cron_run.sh:31`, `app/core/services/crons_quebrados.py:36`, `app/core/management/commands/pipeline_placar.py:70`, `bin/crons_duracao.py:69`)

**Métrica invisível do P2:** `ESPERA` é capturada e placar gravado antes da readquisição ilimitada. Portanto o tempo de epílogo não aparece em `espera_deploy`, `dur` ou `fim`, embora o envelope continue vivo. (`bin/cron_run.sh:18`, `bin/cron_run.sh:30`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`)

**F4 não se resolve por `espera_deploy>0`:** sucesso de aquisição também pode esperar e CMD retornar 75; falha imediata de aquisição pode registrar espera zero; ajuste de relógio pode alterar o valor. É duração de aquisição, não causa. (`bin/trava_deploy_exclusao.sh:46`, `bin/trava_deploy_exclusao.sh:49`, `bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:51`, `bin/cron_run.sh:25`, `bin/cron_run.sh:31`)

P6 continua divergente no RELATO real: linha 25 ainda publica cron600/deploy não bloqueante/espera_lock/dois contadores/desliga só corte; linha21 fala contrato v2. Não há atualização para v3 nesse trecho. Envio único ao Codex não pode ser comprovado pelo código. (`app/docs/RELATO.md:21`, `app/docs/RELATO.md:25`; envio: [nao verificado])

## 09/11 08:01 · sessao 98ec70eba7b9 · - · - · origem=Ronald (tmux)

- **P5 tem asserção nova útil:** `g` executa três provas com atraso e exige `ultima(PROVA-FIM) < CMD-INI`, tanto normal quanto restart. Não passa se faltarem comando ou provas. (`bin/tests/test_deploy_exclusao_cron.sh:104`, `bin/tests/test_deploy_exclusao_cron.sh:319`, `bin/tests/test_deploy_exclusao_cron.sh:333`)

- **Mordida observada de m1–m5: [nao verificado].** As definições e resultados desses cinco mutantes não estão nos anexos fornecidos nem foram localizados na cópia. O selo somente aceita caminhos alternativos para deploy, envelope e biblioteca; não gera nem executa uma bateria de mutantes própria. (`bin/tests/test_deploy_exclusao_cron.sh:43`, `bin/tests/test_deploy_exclusao_cron.sh:44`, `bin/tests/test_deploy_exclusao_cron.sh:45`)

- **RED de `g`: distinguir bases.** A volta 1 já soltava depois das provas, conforme o `preverif_v1.r` fornecido, portanto não há demonstração de soltura prematura real nessa versão. Contra a base anterior sem protocolo de exclusão, a leitura indica que `g` alcança a janela: inicia cron após HUP/restart e compara seu comando com a última prova atrasada. Resultado executado contra qualquer base ou mutante: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:323`, `bin/tests/test_deploy_exclusao_cron.sh:327`, `bin/tests/test_deploy_exclusao_cron.sh:329`, `bin/tests/test_deploy_exclusao_cron.sh:333`; anexo `preverif_v1.r`, trecho sobre `bin/deploy.sh:442`)

- **Há caso que passa sem comprovar seu cenário:** `d3` ignora retorno da espera por `CMD-INI` e do `wait`, e aceita qualquer deploy não zero sem restart. Assim uma falha antecipada independente da concorrência satisfaz o teste. Não exige execução do cron nem diagnóstico de recusa por marca/trava. (`bin/tests/test_deploy_exclusao_cron.sh:278`, `bin/tests/test_deploy_exclusao_cron.sh:280`, `bin/tests/test_deploy_exclusao_cron.sh:282`)

- **`g` não prova sobreposição efetiva:** os retornos de `espera_linha` são ignorados; não exige que o cron tenha chegado antes de `PROVA-FIM`. Um escalonamento que só execute o cron após as três provas satisfaz a ordem sem testar contenção naquela corrida. (`bin/tests/test_deploy_exclusao_cron.sh:324`, `bin/tests/test_deploy_exclusao_cron.sh:327`, `bin/tests/test_deploy_exclusao_cron.sh:329`, `bin/tests/test_deploy_exclusao_cron.sh:333`)

- **P3 tem asserção incompleta:** b1 aceita qualquer `(fim-ini)-dur <= 1`, inclusive negativo arbitrário; não confronta `ini` com timestamp do comando. (`bin/tests/test_deploy_exclusao_cron.sh:144`, `bin/tests/test_deploy_exclusao_cron.sh:197`)

- **P4 tem cobertura direta, mas limitada:** f exige zero filhos classificados com fd aberto, ao menos oito fechados e migrate aberto; cria netos na prova. Não testa morte do deploy durante migrate/restart nem verifica fd do restart nesse caso. A passagem normal chega ao unlock explícito, portanto a parte “neto não impede cron após deploy” não demonstra comportamento no abandono anormal. (`bin/tests/test_deploy_exclusao_cron.sh:106`, `bin/tests/test_deploy_exclusao_cron.sh:304`, `bin/tests/test_deploy_exclusao_cron.sh:307`, `bin/tests/test_deploy_exclusao_cron.sh:310`, `bin/deploy.sh:456`)

- **P6 documental é parcial:** o anexo `contrato_v3.txt` descreve o desenho novo; a biblioteca aponta para ele. Porém RELATO mantém o contrato antigo de cron 600/deploy não bloqueante/`espera_lock`/contadores, com somente um resumo de v2 acima. Arquivo v3 físico, sequência de versões e envio “uma vez”: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:14`, `app/docs/RELATO.md:21`, `app/docs/RELATO.md:25`; anexo `contrato_v3.txt:2`, `contrato_v3.txt:3`, `contrato_v3.txt:5`)

- **Relógio e causa:** d2 exige espera zero, mas a medição envolve dois `date +%s` e o processo flock; cruzar um segundo sem contenção pode produzir 1. `espera_deploy > 0` também não identifica causa de rc 75: um comando pode obter vez após espera e devolver 75 por si. (`bin/tests/test_deploy_exclusao_cron.sh:268`, `bin/trava_deploy_exclusao.sh:49`, `bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:51`, `bin/cron_run.sh:17`, `bin/cron_run.sh:25`)

Execução dos selos e mutantes: **[nao verificado]**; nenhuma execução realizada porque o selo cria, modifica e remove arquivos temporários. (`bin/tests/test_deploy_exclusao_cron.sh:60`, `bin/tests/test_deploy_exclusao_cron.sh:78`, `bin/tests/test_deploy_exclusao_cron.sh:170`)

## 09/11 08:00 · sessao 79c1c84e8d87 · - · - · origem=Ronald (tmux)

- Identidade disponível: `MARCO_DA_PERGUNTA` fixa saída em `S/hii` e volta em `E/hfi`; ambos pertencem ao primeiro par canônico de `CelulaDia.PARES_PAUSA_DNA`. `chamados/juizes.py:882`; `escala/models.py:1652`.
- `instancia_marco` não identifica duas pausas: deriva apenas `int_ida/int_volta`; o modelo recusa valores inventados. `chamados/services/declaracao_texto.py:42`; `chamados/services/declaracao_texto.py:48`; `chamados/models.py:3436`.
- C6 considera qualquer pergunta validada morta; seu prefiltro SQL também exige validação nula. Preservar a primeira validação significa deixá-la morta para C6, enquanto a outra metade mantém o caso pendente. Alterar esse comportamento exigiria compatibilizar o modelo proibido. `chamados/juizes.py:1153`; `chamados/models.py:3359`.
- Fechamento consulta respostas sem veredito e `fio_vivo`; a metade ainda não validada bloqueia o caso nominal. Há risco fora do nominal se essa metade for absolvida pelo próprio C6. `chamados/models.py:1848`; `chamados/models.py:1856`; `chamados/juizes.py:1262`.
- A espera precisa aparecer em `estado_aplicacao_pergunta` e no template: o ramo de validada atualmente renderiza apenas paridade e reuso. `chamados/services/materializacao.py:425`; `chamados/services/fio.py:335`; `templates/chamados/partials/modal_fio.html:227`.
- A trava deve reunir destinos, clones e IDs das duas perguntas antes de validar; a porta central oferece todos esses argumentos. `chamados/models.py:2637`; `chamados/models.py:2648`; `chamados/models.py:2655`.

Nenhuma escrita ou execução Django realizada. [nao verificado]

## 09/11 07:53 · sessao 0e7deca660a7 · AUDITORIA-0911 · leitura · origem=Code

**Não há número novo de produção nem commit de cura nesta auditoria.** As sondas abaixo não foram executadas; o hash informado não foi comprovado como commit Git nesta cópia. [nao verificado] [nao verificado]

**A — Os contadores não medem o mesmo universo.**

| Grandeza | Quem conta | Universo literal e unidade |
|---|---|---|
| Badge **1.177** do anexo [nao verificado] | `lavrar_badge_navbar` → `exige_o_admin` → `particionar_verbos` | `ChamadoColaborador.objects.filter(status_local__in=VIVOS)`, com `VIVOS=('aberto','em_analise','registrado')`; soma somente verbos `validar` e `decidir`. **Chamados**, não perguntas/toques. `app/core/management/commands/lavrar_badge_navbar.py:72`; `app/chamados/services/painel.py:155`; `app/chamados/catalogo/motor.py:24`; `app/chamados/services/verbos.py:22`. |
| **4.323** referido no aval [nao verificado] | Se corresponde à coluna `chamados_vivos`, é `vivos.count()` | Mesmo filtro `status_local__in=VIVOS`; **chamados**. Não corresponde a `lavradas` nem a `sobra`. `app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:71`. |
| `universo=77xx` do stale | `universo.count()` | `PerguntaDisputa.objects.filter(chamado_destino__in=vivos)`; **perguntas**, sem o fallback pelo chamado da disputa que o badge utiliza. `app/chamados/management/commands/lavrar_perguntas_stale.py:57`; `app/chamados/services/raiox.py:54`. |
| `perguntas_stale_agora` | `len(pks_stale_por_varredura(universo))` | Perguntas daquele universo com lavra ausente, relógio vencido ou insumo posterior à lavra. `app/chamados/services/raiox.py:31`. |
| `lavradas / sobra` | Laço `lavrar(p)` / `max(len(stale)-teto,0)` | **Perguntas processadas nesta rodada / perguntas fora do teto**; teto padrão 500. `app/chamados/management/commands/lavrar_perguntas_stale.py:35`; `app/chamados/management/commands/lavrar_perguntas_stale.py:60`; `app/chamados/management/commands/lavrar_perguntas_stale.py:84`. |
| **7.716 literal encontrado** | Não é apresentado como contador do stale | A ocorrência documental é a **meta histórica de 98% do oráculo**, em **dia-colab**: `7.716−7.193=523`, sobre 7.873 comparados. Atribuir exatamente 7.716 ao universo de perguntas de uma rodada é [nao verificado]. `app/docs/RELATO-ARQUIVO.md:23683`; `app/docs/RELATO-ARQUIVO.md:23695`. |

O arquivo de log mistura dois lavradores: `lavrar_placar_situacional` conta o universo de colaboradores em operação; o badge pertence ao `lavrar_badge_navbar`. A diferença entre esses universos não demonstra, sozinha, defeito estrutural. `app/core/management/commands/lavrar_placar_situacional.py:54`; `app/core/management/commands/lavrar_badge_navbar.py:85`.

A separação solicitada tem estes limites de autoridade:

| Classe pedida | Juiz existente |
|---|---|
| Legítimo | **SEM JUIZ composto.** Premissa viva inclui resultado conservador quando falta insumo; não prova legitimidade geral. `app/chamados/catalogo/premissa.py:128`. |
| Repetido | O registro aponta para **`ChamadoColaborador.abrir`**, uma porta de escrita. **SEM JUIZ equivalente de leitura** para censar todos os repetidos; não chamar essa porta na sonda. `app/core/juizes.py:268`. |
| Sem porta | `porta_alcancavel_do_dia`; seu `None` significa ausência das portas daquele corte. “Resolver dia” e acesso manual por URL estão explicitamente fora desse juiz. `app/core/juizes.py:277`; `app/chamados/juizes.py:1361`; `app/chamados/juizes.py:1387`. |
| Premissa superada | `premissa_morta`, alimentada por `_veredito_premissa`; respeitar `escopo` e `viva`. `app/core/juizes.py:263`; `app/chamados/reconciliador.py:247`. |
| Exige humano | Medida pelo verbo existente `validar/decidir`; **SEM JUIZ composto adicional** para esse rótulo. `app/chamados/services/verbos.py:22`. |
| Pergunta viva/encerrada atualmente | C6, `pergunta_viva`. Via `duplicata` registrada é encerramento por identidade; não equivale a censo de repetição ativa. `app/core/juizes.py:266`; `app/chamados/juizes.py:1163`. |

Esses resultados podem sobrepor-se. Não há autoridade citada que estabeleça uma partição exclusiva entre os cinco rótulos; a sonda mantém os eixos separados. `app/core/juizes.py:263`; `app/core/juizes.py:266`; `app/core/juizes.py:277`.

**Nasce/permanece/encerra históricos antes/depois do deploy não são reconstruíveis com exatidão pelos campos atuais.** A pergunta não tem carimbo próprio de nascimento; `data_evento` é a data do fato. O chamado pode reabrir; seus carimbos atuais não fornecem todos os intervalos. `DecisaoPergunta` registra atos, sem registrar todas as mudanças de C6 provocadas por insumos externos. O fluxo histórico pedido permanece [nao verificado]. `app/chamados/models.py:2431`; `app/chamados/models.py:2455`; `app/chamados/models.py:292`; `app/chamados/models.py:3857`.

A sonda A mede os contadores literais, o estoque atual e os carimbos disponíveis por competência; não apresenta carimbo de fechamento como fluxo completo. As competências usam o juiz da empresa: com corte 21, outubro é 21/09–20/10 e novembro é 21/10–20/11. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:36`; `app/chamados/juizes.py:2090`.

**Preâmbulo comum às sondas.** Executar somente após a cura do envelope, em Python/Django já inicializado e com conexões abertas em leitura. O envelope atual executa `GRANT` antes da sonda; este bloqueio interno não cobre aquela execução anterior. `bin/sonda_leitura.sh:157`; `bin/sonda_leitura.sh:161`; `bin/sonda_leitura.sh:192`.

```python
# PREAMBULO COMUM — não executado nesta auditoria.
import sys
sys.dont_write_bytecode = True

import json
import logging
import re
from contextlib import ExitStack, contextmanager
from django.db import connections

class SondaRecusada(BaseException):
    pass  # não pode ser engolida pelos except Exception da aplicação

def saida(**dados):
    print(json.dumps(dados, ensure_ascii=False, default=str))

@contextmanager
def leitura():
    anterior = logging.root.manager.disable
    logging.disable(10**9)  # nenhuma saída incidental nominal dos leitores
    try:
        with ExitStack() as pilha:
            for conn in connections.all():
                # O envelope curado deve inicializar as conexões antes daqui.
                if conn.connection is None:
                    raise SondaRecusada("conexao nao inicializada pelo envelope")
                if conn.in_atomic_block or not conn.get_autocommit():
                    raise SondaRecusada("exige autocommit, fora de atomic")

                def somente_select(execute, sql, params, many, context):
                    texto = str(sql).strip()
                    if (
                        not re.match(r"(?is)^SELECT\b", texto)
                        or ";" in texto
                        or re.search(
                            r"(?is)\bFOR\s+(UPDATE|SHARE|NO\s+KEY|KEY\s+SHARE)\b"
                            r"|\b(nextval|setval|set_config|pg_advisory\w*)\s*\(",
                            texto,
                        )
                    ):
                        raise SondaRecusada("SQL fora do contrato SELECT")
                    return execute(sql, params, many, context)

                pilha.enter_context(conn.execute_wrapper(somente_select))
                with conn.cursor() as cur:
                    cur.execute(
                        "SELECT current_setting('transaction_read_only'), "
                        "rolsuper FROM pg_roles WHERE rolname=current_user"
                    )
                    estado = cur.fetchone()
                if estado != ("on", False):
                    raise SondaRecusada("conexao sem contrato de leitura")
            yield
    finally:
        logging.disable(anterior)
```

Não envolver as sondas em `atomic`: `reconciliaria()` tem caminho de trava quando já está dentro de transação. O bloqueio usa `BaseException` porque o fechamento captura `Exception` e poderia ocultar uma tentativa recusada. `app/chamados/models.py:705`; `app/ponto/services/fechamento.py:1303`.

```python
# SONDA A — contadores e eixos atuais, competências 10 e 11.
# Fontes: lavrar_perguntas_stale.py:56; verbos.py:25;
# core/juizes.py:263,266,274,277.
from collections import Counter
from datetime import datetime
from zoneinfo import ZoneInfo

def sonda_A():
    from chamados.models import ChamadoColaborador as CH, PerguntaDisputa as PD
    from chamados.catalogo.motor import VIVOS
    from chamados.services.verbos import (
        particionar_verbos, exige_o_admin, VERBOS_DO_ADMIN,
    )
    from chamados.services.raiox import pks_stale_por_varredura
    from chamados.juizes import (
        competencia_do_chamado, pergunta_viva,
        alimentar_portas, porta_alcancavel_do_dia,
    )
    from chamados.reconciliador import _veredito_premissa
    from ponto.janelas import janela_atual

    deploy = datetime(2026, 11, 9, 1, 52, [nao verificado]
                      tzinfo=ZoneInfo("America/Sao_Paulo"))
    vivos = CH.objects.filter(status_local__in=VIVOS)
    universo = PD.objects.filter(chamado_destino__in=vivos)
    part = particionar_verbos(vivos)
    stale = set(pks_stale_por_varredura(universo))
    saida(
        item="A", recorte="global_literal",
        badge=exige_o_admin(part["contagens"]),
        chamados_vivos=vivos.count(),
        universo_perguntas=universo.count(),
        perguntas_stale_agora=len(stale),
        lavraria_com_teto_500=min(500, len(stale)),
        sobra_com_teto_500=max(len(stale)-500, 0),
    )

    chamados = list(CH.objects.select_related("colaborador__empresa", "celula"))
    alimentacao = alimentar_portas(
        {c.celula_id for c in chamados if c.celula_id}
    )
    cont = Counter()

    def contar(mes, entidade, causa, eixo, resultado):
        cont[(mes, entidade, causa, eixo, resultado)] += 1

    def lado(ts):
        return ("SEM CARIMBO" if ts is None else
                "antes_deploy" if ts < deploy else "depois_deploy") [nao verificado]

    for c in chamados:
        cp = competencia_do_chamado(c)
        if cp is None:
            contar(None, "chamado", (c.modulo_origem, c.subcategoria),
                   "competencia", "SEM FATO DATADO")
            continue
        if cp.ano != 2026 or cp.mes not in (10, 11):
            continue
        causa = (c.modulo_origem, c.subcategoria)
        contar(cp.mes, "chamado", causa, "status_atual", c.status_local)
        contar(cp.mes, "chamado", causa, "nascimento_registrado", lado(c.criado_em))
        contar(cp.mes, "chamado", causa, "carimbo_fecho_atual", lado(c.resolvido_em))
        contar(cp.mes, "chamado", causa, "legitimo", "SEM JUIZ")
        contar(cp.mes, "chamado", causa, "repetido_leitura", "SEM JUIZ")
        try:
            v = _veredito_premissa(c)
            resultado = (
                "SEM JUIZ" if v is None else
                "superada" if not v.viva else v.motivo
            )
            contar(cp.mes, "chamado", causa, "premissa", resultado)
        except Exception as exc:
            contar(cp.mes, "chamado", causa, "premissa",
                   "JUIZ FALHOU:" + type(exc).__name__)
        if c.status_local in VIVOS:
            porta = porta_alcancavel_do_dia(
                c.celula_id, alimentacao=alimentacao
            )
            contar(cp.mes, "chamado", causa, "porta_do_corte",
                   porta["rota"] if porta else "sem porta")
            verbo = part["mapa"].get(c.pk)
            contar(cp.mes, "chamado", causa, "verbo", verbo)
            if verbo in VERBOS_DO_ADMIN:
                contar(cp.mes, "chamado", causa, "exige_humano",
                       "verbo_do_admin")

    for p in PD.objects.select_related(
        "disputa__colaborador__empresa", "celula", "chamado_destino"
    ).iterator():
        causa = (p.motivo, p.instancia_marco)
        if p.conversa or p.data_evento is None:
            contar(None, "pergunta", causa, "competencia",
                   "SEM DIA DE TURNO")
            continue
        fim = janela_atual(p.data_evento, p.disputa.colaborador.empresa)[1]
        if fim.year != 2026 or fim.month not in (10, 11):
            continue
        contar(fim.month, "pergunta", causa, "C6_atual",
               "viva" if pergunta_viva(p) else "encerrada")
        contar(fim.month, "pergunta", causa, "via_registrada", p.via_resolucao)
        contar(fim.month, "pergunta", causa, "legitimo", "SEM JUIZ")
        if p.chamado_destino_id in part["mapa"]:
            contar(fim.month, "pergunta", causa, "universo_stale", "destino_vivo")
        if p.pk in stale:
            contar(fim.month, "pergunta", causa, "stale", "sim")

    for (mes, entidade, causa, eixo, resultado), n in sorted(
        cont.items(), key=lambda x: str(x[0])
    ):
        saida(item="A", competencia=mes, entidade=entidade,
              causa=causa, eixo=eixo, resultado=resultado, n=n)
    saida(
        item="A", nasce_permanece_encerra_historico=None,
        limite="[nao verificado]: sem fotos e insumos historicos; "
               "carimbos atuais nao reconstroem C6 e portas anteriores",
    )

with leitura():
    sonda_A()
```

**Dono A:** divergência entre um contador e sua própria lista seria provavelmente **ESTRUTURA**, padrão **Single Source of Truth / Aggregate Root**. Números diferentes com universos diferentes não bastam para essa atribuição. `app/docs/GLOSSARIO.md:11`; `app/ponto/management/commands/e6_oraculo.py:57`.

**B — Ausência aprovada até DiaPago/export.**

| Degrau | Leitura/efeito existente |
|---|---|
| Cobertura | Para folha, somente `status='aprovada'`; para cobrança, exclui rejeitada. Sobreposição vence por `(data,pk)` mais recente. `app/ponto/turnos.py:2511`; `app/ponto/turnos.py:2735`. |
| Célula/grade | Overlay da ausência na grade; leitor da célula entrega a ata lavrada. `app/escala/utils.py:1188`; `app/escala/services/leitor_celula.py:345`. |
| Resolver dia | Lê batidas cruas de D até D+2 e chama `grade_dia`. No pedido `trabalhou`, recusa se `tipo_dia != 'trabalho'`. Isso não demonstra perda no motor. `app/ponto/services/veredito_celula.py:234`; `app/ponto/services/veredito_celula.py:241`; `app/ponto/services/simular_dia.py:5`. |
| Motor | Fechamento lê cobertura, acrescenta ABONA a `datas_justificadas` e entrega ausências integrais ao motor. A integral zera atraso/saída antecipada, preservando horas trabalhadas. `app/ponto/services/fechamento.py:683`; `app/ponto/services/fechamento.py:751`; `app/ponto/motor_calculo_v2.py:1085`. |
| Abono diário | `por_dia_da_grade`: previsto menos cobertura das batidas no dia de ausência. O comentário cita col39/15-10 como precedente, não como medição atual de #4879. `app/ponto/services/dia_pago.py:364`; `app/ponto/services/dia_pago.py:375`. |
| DiaPago | Lavra recebe os mesmos períodos e mapas do fechamento. `somente_leitura=True` só permanece leitura com `lavrar_dias_pagos=False`. `app/ponto/services/fechamento.py:1191`; `app/ponto/services/fechamento.py:1201`. |
| Export | `classificar_export` julga entrada; `montar_linhas_export` lê fechamento e eventos, sem recalcular. Registros de horas são mensais; datas individuais existem no bloco de faltas. `app/folha/export.py:757`; `app/folha/export.py:717`; `app/folha/export.py:224`. |

O dado fornecido não contém os valores dos degraus de #4879. Portanto, o ponto concreto da perda de cobertura permanece [nao verificado].

```python
# SONDA B — ausência/célula/Resolver/motor/projeção/export, sem escritores.
# Fontes: fechamento.py:448,683,751,1128,1191; dia_pago.py:364;
# veredito_celula.py:234; export.py:682.
def sonda_B():
    import datetime as dt
    from collections import Counter
    from django.utils import timezone
    from colaboradores.models import Colaborador
    from ponto.models import Ausencia, Batida, DiaPago, FechamentoMensal
    from ponto.turnos import cobertura_ausencia_periodo, cobre_parte_do_dia
    from ponto.services.efeito_ausencia import efeitos_da_ausencia
    from escala.services.leitor_celula import grade_da_celula
    from ponto.services.simular_dia import grade_dia
    from ponto.services.dia_pago import CAMPOS_DIA, por_dia_da_grade
    from ponto.services.fechamento import recalcular_fechamento_mes
    from folha.export import (
        janela_competencia, classificar_export,
        montar_linhas_export, partes_da_linha,
    )
    from folha.models import ExportacaoDominio, anotar_vigencia, so_vigentes

    c = Colaborador.objects.select_related("empresa").get(pk=39)
    d = dt.date(2026, 10, 15)
    agora = timezone.now()
    ini, fim = janela_competencia(c.empresa, 10, 2026)
    a = Ausencia.objects.get(pk=4879, colaborador=c)
    campos_a = ("status", "tipo", "data", "data_fim",
                "minutos", "hora_ini", "hora_fim")
    saida(item="B", degrau="ausencia",
          lido={k: getattr(a, k) for k in campos_a},
          efeito=a.efeito, parcial=cobre_parte_do_dia(a))

    for para in ("folha", "cobranca"):
        vencedora = cobertura_ausencia_periodo(c, d, d, para=para).get(d)
        saida(item="B", degrau="cobertura", para=para,
              ausencia_vencedora=getattr(vencedora, "pk", None),
              efeito=getattr(vencedora, "efeito", None))
    efeitos = efeitos_da_ausencia(c, d, d)
    saida(item="B", degrau="efeitos",
          lido={k: d in efeitos[k]
                for k in ("justificadas", "suprimidas", "dia_inteiro")})

    campos_grade = (
        "tipo_dia", "cobertura_parcial", "ausencia_id",
        "minutos_previstos", "minutos_realizados", "n_missing", "n_ok",
    )
    g = grade_da_celula(c, d, d)
    saida(item="B", degrau="celula",
          lido=[{k: x.get(k) for k in campos_grade}
                for x in g.get("dias") or []])
    mapas = por_dia_da_grade(g)
    def valor_dia(m):
        return m.get(d, m.get(str(d)))
    saida(item="B", degrau="grade_para_DiaPago",
          lido=dict(zip(("abono", "previsto", "realizado"),
                        (valor_dia(m) for m in mapas))))

    j0 = timezone.make_aware(dt.datetime.combine(d, dt.time.min))
    reais = list(Batida.objects.filter(
        colaborador=c, timestamp__gte=j0,
        timestamp__lt=j0+dt.timedelta(days=2),
    ).order_by("timestamp"))
    saida(item="B", degrau="batidas_lidas_pelo_Resolver",
          lido=[{k: getattr(b, k) for k in (
              "timestamp", "tipo", "origem", "is_intervalo",
              "aprovada", "retratada_em",
          )} for b in reais])
    marcos, dd = grade_dia(c, reais, d, agora=agora)
    saida(item="B", degrau="Resolver_grade",
          lido={k: dd.get(k) for k in campos_grade},
          marcos=[{k: m.get(k) for k in ("tipo", "hora")}
                  if isinstance(m, dict) else None for m in marcos])

    # Observa os objetos/locais do caminho real, em memória.
    def perfil(frame, event, arg):
        if event != "return":
            return
        path, fn = frame.f_code.co_filename, frame.f_code.co_name
        loc = frame.f_locals
        if path.endswith("/ponto/services/fechamento.py") and fn == "_montar_motor":
            saida(item="B", degrau="motor_recebeu",
                  integral=d in arg.dias_com_ausencia_inteira,
                  justificada=d in arg.datas_justificadas)
        if path.endswith("/ponto/services/fechamento.py") and fn == "recalcular_fechamento_mes":
            r = loc.get("resultado")
            mapa = getattr(r, "dia_da_jornada", {}) or {}
            periodos = [p for p in (getattr(r, "periodos", []) or [])
                        if mapa.get(id(p)) == d]
            saida(item="B", degrau="motor_devolveu",
                  periodos=[{k: getattr(p, k, None) for k in (
                      "entrada", "saida", "minutos_trabalhados",
                      "minutos_atraso", "minutos_saida_antecipada",
                  )} for p in periodos],
                  furo_motor=d in (getattr(r, "datas_falta", []) or []),
                  mapas_para_DiaPago={
                      k: valor_dia(loc.get(k) or {})
                      for k in ("_abono_dia", "_prev_dia_m", "_real_dia_m",
       

[truncado em 20000 de 44487 caracteres]

## 09/11 07:52 · sessao 7c9934466d7c · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · leitura · origem=Code

O bloqueio está na aplicação **por pergunta**, antes da porta de escrita: cada metade encontra uma vizinha do mesmo tipo e é recusada. A palavra “colada” traduz uma guarda cujo raio atual é **12 horas**. `app/chamados/services/validacao.py:461`; `app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:875`; `app/chamados/catalogo/recusa.py:26`.

O commit introdutor, o censo de produção e o resultado executado do RED permanecem **[nao verificado]**. Esta cópia não disponibilizou histórico Git; o dado raspado fornece o caso nominal, sem população completa. **[nao verificado]** [nao verificado]

1. **Origem da recusa.**

A recusa concreta deste caminho é:

```python
_viz = _vizinha_mesmo_tipo(disputa.colaborador, ts, _tipo)
if _viz is not None and not _plantio_preenche_furo_na_data(
        disputa.colaborador, ts):
    ...
```

Fonte: `app/chamados/services/materializacao.py:875`.

`_vizinha_mesmo_tipo` procura a batida imediatamente anterior e, depois, a imediatamente posterior, dentro de ±12h. Retorna a primeira vizinha encontrada do mesmo tipo; não filtra aprovação nem `retratada_em`. A exceção consulta se o plantio preenche um furo na célula da **data civil do plantio**. `app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:142`; `app/chamados/services/materializacao.py:170`.

Quando essa recusa acontece, o código não chega a `registrar_batida`. Se o marco cobrado não estiver comprovadamente aceso, registra a recusa e mantém a pergunta sem materialização; a validação recusada é devolvida ao administrador. `app/chamados/services/materializacao.py:901`; `app/chamados/services/materializacao.py:913`; `app/chamados/services/materializacao.py:926`; `app/chamados/services/validacao.py:464`.

A porta `registrar_batida` **não contém uma recusa geral de duas batidas iguais distantes**. Ela deduplica mesmo tipo próximo, verifica competência para escrita administrativa e pode recusar colisão com tipo oposto. `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:132`; `app/ponto/registro_batida.py:138`.

As tentativas de `git log -L` e `git blame` falharam com `fatal: not a git repository`; portanto, commit, data e mensagem de introdução de cada trecho abaixo são **[nao verificado]**. [nao verificado]

| Trecho | Evidência disponível na cópia | Introdução Git |
|---|---|---|
| Vizinha do mesmo tipo, raio 12h | Comentário atribui a guarda a “fio-quebrado 01/08”. `app/chamados/services/materializacao.py:85` | **[nao verificado]** |
| Recusa antes do plantio | Comentário também menciona 01/08. `app/chamados/services/materializacao.py:872` | **[nao verificado]** |
| Tratamento por lâmpada do marco | Comentário atribui a alteração a 18/10. `app/chamados/services/materializacao.py:889` | **[nao verificado]** |
| Frase “colada / duas seguidas iguais” | Regra `PARIDADE`. `app/chamados/catalogo/recusa.py:26` | **[nao verificado]** |
| Dedup/colisão na porta única | Predicados efetivos da porta. `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:138` | **[nao verificado]** |

O relato associa `225256c1` ao fecho/trava do repasse, `e017181f` a `REPASSE_RECUSADO` e `e9bf5de2` aos consumidores dessa recusa. Isso não prova a introdução da guarda de paridade nesses commits. `app/docs/RELATO.md:27`; `app/docs/RELATO.md:17`; `app/docs/RELATO.md:11`.

**Qual commit introduziu a recusa que trava o caso 960: [nao verificado].** Os comentários apontam antecedentes anteriores à E1; não substituem blame. A identificação do deploy às 01:52 também não foi confirmada: o relato da V2-2 registra pouso às 03:3x. `app/chamados/services/materializacao.py:872`; `app/docs/RELATO.md:3`.

2. **Unidade de trabalho hoje.**

O fluxo de validação é:

```text
view/fio
  → acao_validar_pergunta
  → validar_pergunta(UMA pergunta)
      atomic + travas
      → carimbar validação
      → _materializar_uma(pergunta atual)
      → devolver validação se recusada
  → varrer outras perguntas validadas da disputa
```

Fontes: `app/chamados/views.py:483`; `app/chamados/services/fio.py:585`; `app/chamados/services/acoes_disputa.py:13`; `app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:461`; `app/chamados/services/validacao.py:469`.

A varredura seleciona perguntas respondidas, validadas e sem materialização, chamando `_materializar_uma` individualmente. Cada unidade abre seu próprio `atomic`, planta, exige lastro e pede carimbo ao C6; reprovação desfaz aquela unidade. `app/chamados/services/materializacao.py:1369`; `app/chamados/services/materializacao.py:1220`; `app/chamados/services/materializacao.py:1232`; `app/chamados/services/materializacao.py:1277`.

Quem grava a batida é `registrar_batida`, chamado pelo ramo de hora com `autoridade_humana=False` e `fato_de_chao=False`. `app/chamados/services/materializacao.py:926`; `app/ponto/registro_batida.py:157`.

“Regularizar disputa” também entrega as perguntas ao materializador granular; não constitui um ato próprio para o par de intervalo. `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/fio.py:670`.

Existe ainda “decidir o dia”, via `veredito_celula_view → veredito_celula`, com simulação do conjunto e escrita pela mesma porta, mas esse caminho decide o dia e pode preencher marcos; não é o aceite das duas declarações deste par. `app/ponto/views.py:3312`; `app/ponto/views.py:3340`; `app/ponto/services/veredito_celula.py:223`; `app/ponto/services/veredito_celula.py:277`; `app/ponto/services/veredito_celula.py:322`.

**SEM JUIZ DO PAR DE PERGUNTAS.** Há vínculo por disputa, motivo, dia do turno e posição do marco, mas nenhuma identidade compartilhada da pausa. O código declara expressamente que duas pausas no mesmo dia ainda colidem nos mesmos motivos e que nenhum emissor carrega o índice da pausa. `app/chamados/models.py:2424`; `app/chamados/models.py:2431`; `app/chamados/models.py:2442`; `app/chamados/services/declaracao_texto.py:48`.

Os mais próximos são:

| Autoridade existente | O que responde |
|---|---|
| `intervalos_do_turno` | Intervalos entre **batidas** julgadas pelo pareador. `app/core/juizes.py:202`; `app/ponto/turnos.py:502` |
| `MARCO_DA_PERGUNTA` | Saída → S/hii; volta → E/hfi. `app/chamados/juizes.py:882` |
| `instancia_do_motivo` | Posições `int_ida` e `int_volta`, sem índice compartilhado da pausa. `app/chamados/services/declaracao_texto.py:42` |
| `CelulaDia.PARES_PAUSA_DNA` | Pares canônicos dos **marcos**: hii/hfi e hii2/hfi2. `app/escala/models.py:1652` |
| Emissor de intervalo ausente | Lê `marcos_faltantes` e emite uma pergunta por marco S/E. `app/chamados/services/disputa_emissao.py:1132` |

Preencher arbitrariamente `instancia_marco` para inventar essa identidade é recusado pelo modelo. `app/chamados/models.py:3436`.

3. **Texto e tolerância.**

“Marcação […] colada nesse horário” e “duas seguidas iguais” nascem na regra `PARIDADE`. A tradução reconhece substrings do erro; não mede distância temporal. A casca de validação e o followup usam essa tradução. `app/chamados/catalogo/recusa.py:26`; `app/chamados/catalogo/recusa.py:77`; `app/chamados/services/acoes_disputa.py:32`; `app/chamados/services/materializacao.py:1091`.

| Uso real | Tolerância efetiva | Consequência |
|---|---:|---|
| Guarda de paridade | ±12h | Recusa condicionada ao furo. `app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:875` |
| Mesmo tipo na porta | ±60s, inclusivos | Reutiliza a existente. `app/ponto/registro_batida.py:106` |
| Reuso do materializador | ±20min | Reutiliza a existente. `app/chamados/juizes.py:235` |
| Colisão de tipo oposto | ±300s com escala/marcos; ±60s sem | Pode recusar. `app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:138` |
| Exceção por marco próprio | ±5min | Afasta colisão oposta. `app/ponto/registro_batida.py:35`; `app/ponto/registro_batida.py:145` |

Não existe, nessa regra de apresentação, um predicado próprio de “colada”. Associar a frase automaticamente aos 5min da CCT seria introduzir uma ligação que o código atual não faz. `app/chamados/catalogo/recusa.py:77`; `app/core/models.py:59`.

4. **Desenho de cura — proposto, não implementado. [nao verificado]**

Para o par cuja identidade esteja comprovada, o ato deve:

- Registrar o aceite da primeira metade, preservar a declaração e responder **“falta a outra metade do intervalo”**, sem plantar nem reconciliar parcialmente. **[nao verificado: desenho]**
- Quando ambas estiverem respondidas e validadas, travar o conjunto, reler as duas, derivar os instantes pela função existente e gravar ambas numa transação pela porta única. **[nao verificado: desenho]**
- Julgar alternância e lastro sobre o conjunto completo; qualquer reprovação deve desfazer as duas batidas e os carimbos de aplicação. Só depois reconciliar destinos e avaliar fechamento. **[nao verificado: desenho]**

Um `atomic` ao redor do laço atual não basta: a primeira metade continuaria encontrando a guarda individual antes do plantio. O reparador O330 já julga geometria após um grupo do dia, mas ainda chama `_materializar_uma` para cada pergunta. `app/chamados/services/materializacao.py:875`; `app/chamados/services/passivo_validado.py:474`; `app/chamados/services/passivo_validado.py:516`; `app/chamados/services/passivo_validado.py:536`.

**Limite do desenho:** sem juiz novo, é necessário demonstrar como os juízes existentes de marco/turno e os pares canônicos do DNA identificam inequivocamente as duas perguntas. Esse adaptador não existe hoje; “mesma disputa + mesmo dia” sozinho não resolve a lacuna declarada das duas pausas. `app/escala/models.py:1652`; `app/chamados/services/declaracao_texto.py:48`. O desenho geral ainda não está fechado. **[nao verificado]**

| Sítio/campo | Quem escreve hoje | Quem lê / ponto afetado |
|---|---|---|
| `resposta_colab`, `respondida_em` | Porta de resposta. `app/chamados/services/disputa_emissao.py:1464` | Validação e derivação. `app/chamados/services/validacao.py:393`; `app/chamados/services/materializacao.py:846` |
| `validada_em/por` | `_carimbar_validacao`; devolução da recusa. `app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:195` | Seleção e estado da pergunta. `app/chamados/services/materializacao.py:1369`; `app/chamados/models.py:3330` |
| Unidade aplicada | `_materializar_uma`. `app/chamados/services/materializacao.py:1158` | Validação direta e varredura. `app/chamados/services/validacao.py:461`; `app/chamados/services/materializacao.py:1375` |
| Batida: instante/tipo/origem | `registrar_batida`. `app/ponto/registro_batida.py:157` | Juiz do lastro após plantio. `app/chamados/services/materializacao.py:1233` |
| `pergunta_origem` | Materializador por `anotar`. `app/chamados/services/materializacao.py:946` | Lastro vinculado à pergunta. `app/chamados/services/materializacao.py:941` |
| Materialização/via | `PerguntaDisputa.carimbar`. `app/chamados/models.py:3103` | C6 e seletor granular. `app/chamados/models.py:3139`; `app/chamados/services/materializacao.py:1372` |
| Recusa/espera e apresentação | Recusa canônica; casca traduz. `app/chamados/services/materializacao.py:1083`; `app/chamados/services/acoes_disputa.py:32` | Modal e estado de aplicação. `app/templates/chamados/partials/modal_fio.html:225`; `app/chamados/services/materializacao.py:425` |
| Fechamento | `DisputaSupervisao.fechar`. `app/chamados/models.py:1863` | `pode_fechar` e cauda materializadora. `app/chamados/models.py:1856`; `app/chamados/services/materializacao.py:1302` |
| Sequência E1 | `DecisaoPergunta.registrar`. `app/chamados/models.py:3942` | Histórico `respostas_anteriores`. `app/chamados/models.py:3849` |

Os riscos concretos são:

- **Fechamento antecipado:** `validada_em` mata a pergunta no primeiro degrau; guardar uma metade validada exige manter a pendência do par corretamente visível aos leitores e ao fechamento. `app/chamados/models.py:3330`; `app/chamados/models.py:1856`.
- **Reconciliação entre metades:** o ramo atual reconcilia imediatamente após cada plantio; ambas precisam permanecer sob o escopo de aplicação antes dessa cauda. `app/chamados/services/materializacao.py:974`; `app/chamados/services/materializacao.py:1114`.
- **Concorrência:** preservar ordem Colaborador → Disputas → Chamados → Perguntas, incluindo destinos e clones, antes da escrita. `app/chamados/models.py:2637`; `app/chamados/services/validacao.py:431`.
- **Chão e motor:** conservar a separação institucional `fato_de_chao=False`; não transferir essa recusa para a batida de chão nem produzir horas não declaradas. `app/chamados/services/materializacao.py:829`; `app/chamados/services/materializacao.py:927`; `app/ponto/registro_batida.py:129`.
- **Append-only E1:** acrescentar atos pela porta; nunca editar/apagar decisões anteriores nem perder projeção legada sem linha. `app/chamados/models.py:3821`; `app/chamados/models.py:3914`; `app/chamados/models.py:3942`.

5. **Sonda SELECT pronta como texto; não executada. [nao verificado]**

O contador do badge é `exige_o_admin(particionar_verbos(qs_vivos())['contagens'])`. O lavrador persiste esse total em `PlacarNavbar`; `PlacarSituacional` é outro placar. `app/chamados/services/verbos.py:25`; `app/core/context_processors.py:88`; `app/core/management/commands/lavrar_badge_navbar.py:72`; `app/core/management/commands/lavrar_placar_situacional.py:56`.

A sonda abaixo reutiliza derivação, reuso e guarda reais. Não chama materialização nem `registrar_batida`. Como falta juiz do par de perguntas, sua saída se chama **candidatos com dupla recusa**, sem certificar “mesma pausa”. O enquadramento de produção e os resultados continuam **[nao verificado]**.

```python
# Executar no shell Django do tenant já selecionado.
# Proposta não executada [nao verificado].
from collections import Counter, defaultdict
from contextlib import ExitStack
import json
import re
from unittest.mock import patch

from django.db import connections
from django.utils import timezone

from chamados.models import PerguntaDisputa as P
from chamados.services.materializacao import (
    _ts_tipo_do_plantio,
    _batida_existente_na_janela,
    _vizinha_mesmo_tipo,
    _plantio_preenche_furo_na_data,
)
from chamados.services.painel import qs_vivos
from chamados.services.verbos import (
    particionar_verbos, exige_o_admin, VERBOS_DO_ADMIN,
)
from core.models import PlacarNavbar
from ponto.janelas import janela_atual

COLAB_PK = None  # None: população inteira; 960: recorte nominal.

class ForaDoEnvelope(BaseException):
    pass

def somente_select(execute, sql, params, many, context):
    # BaseException evita ser engolida por helpers com except Exception.
    if many or not re.match(r"^\s*SELECT\b", sql, re.I):
        raise ForaDoEnvelope("Sonda interrompida: comando fora de SELECT")
    return execute(sql, params, many, context)

def novo_balde():
    return {
        "classes": Counter(),
        "disputas": set(),
        "disputas_no_badge": set(),
        "chamados_do_badge": set(),
        "colabs_pk": set(),
    }

agora = timezone.now()  # Um relógio para toda a rodada.

with ExitStack() as stack:
    for conn in connections.all():
        stack.enter_context(conn.execute_wrapper(somente_select))
    stack.enter_context(patch("django.utils.timezone.now", return_value=agora))

    # Fonte real do total e da pertença aos verbos do administrador.
    part = particionar_verbos(qs_vivos())
    badge = exige_o_admin(part["contagens"])
    admin_ids = {
        pk for pk, verbo in part["mapa"].items()
        if verbo in VERBOS_DO_ADMIN
    }
    assert len(admin_ids) == badge

    lavrado = PlacarNavbar.objects.filter(pk=1).values(
        "badge_chamados", "lavrado_em"
    ).first()
    if lavrado and lavrado["lavrado_em"]:
        lavrado["lavrado_em"] = timezone.localtime(
            lavrado["lavrado_em"]
        ).isoformat()

    perguntas = (
        P.objects.filter(
            disputa__fechada_em__isnull=True,
            motivo__in=("intervalo_saida", "intervalo_volta"),
            conversa=False,
        )
        .exclude(via_resolucao__in=P.VIAS_FORA_DO_FATO)
        .select_related(
            "disputa__colaborador__empresa", "chamado_destino", "celula"
        )
        .order_by("disputa_id", "data_evento", "pk")
    )
    if COLAB_PK is not None:
        perguntas = perguntas.filter(disputa__colaborador_id=COLAB_PK)

    grupos = defaultdict(lambda: defaultdict(list))
    for p in perguntas:
        grupos[(p.disputa_id, p.data_evento)][p.motivo].append(p)

    baldes = defaultdict(novo_balde)
    todas_disputas = set()
    todas_no_badge = set()

    for (disputa_pk, dia), metades in grupos.items():
        linhas = [p for lista in metades.values() for p in lista]
        col = linhas[0].disputa.colaborador

        if dia is None:
            chave = (str(col.empresa_id), "SEM_DATA", "", "")
        else:
            ini, fim = janela_atual(dia, col.empresa)
            chave = (
                str(col.empresa_id), fim.strftime("%Y-%m"),
                ini.isoformat(), fim.isoformat(),
            )
        b = baldes[chave]

        saidas = metades["intervalo_saida"]
        voltas = metades["intervalo_volta"]
        if dia is None:
            b["classes"]["sem_data"] += 1
            continue
        if not saidas or not voltas:
            b["classes"]["sem_duas_metades"] += 1
            continue
        if len(saidas) != 1 or len(voltas) != 1:
            b["classes"]["multiplas_linhas_ambiguas"] += 1
            continue

        par = [saidas[0], voltas[0]]
        if any(p.materializada_em is not None for p in par):
            b["classes"]["com_carimbo_nao_reaplicado_pela_porta_atual"] += 1
            continue
        if any(
            p.respondida_em is None or not (p.resposta_colab or "").strip()
            for p in par
        ):
            b["classes"]["sem_duas_respostas"] += 1
            continue

        recusas = []
        derivacao_ok = True
        for p in par:
            erros = []
            instante, tipo = _ts_tipo_do_plantio(p.disputa, p, erros)
            if instante is None:
                derivacao_ok = False
                break

            # Composição dos helpers efetivamente usados no plantio.
            existente = _batida_existente_na_janela(col, instante, tipo)
            vizinha = (
                _vizinha_mesmo_tipo(col, instante, tipo)
                if existente is None else None
            )
            recusas.append(
                existente is None
                and vizinha is not None
                and not _plantio_preenche_furo_na_data(col, instante)
            )

        if not derivacao_ok:
            b["classes"]["derivacao_indisponivel"] += 1
            continue
        if not all(recusas):
            b["classes"]["sem_dupla_recusa"] += 1
            continue

        b["classes"]["candidato_dupla_recusa"] += 1
        b["disputas"].add(disputa_pk)
        b["colabs_pk"].add(col.pk)
        todas_disputas.add(disputa_pk)

        # Associação explicitamente por pai/destinos do candidato.
        # Não confunde quantidade de disputas com quantidade de chamados.
        vinculados = {par[0].disputa.chamado_id}
        vinculados.update(p.chamado_destino_id for p in par)
        comuns = (vinculados - {None}) & admin_ids
        if comuns:
            b["disputas_no_badge"].add(disputa_pk)
            b["chamados_do_badge"].update(comuns)
            todas_no_badge.add(disputa_pk)

    saida = []
    for chave, b in sorted(baldes.items()):
        empresa_pk, competencia, ini, fim = chave
        saida.append({
            "empresa_pk": empresa_pk,
            "competencia": competencia,
            "inicio": ini,
            "fim": fim,
            "classes": dict(b["classes"]),
            "disputas_candidatas": len(b["disputas"]),
            "disputas_com_chamado_admin_vinculado": len(
                b["disputas_no_badge"]
            ),
            "chamados_admin_vinculados

[truncado em 20000 de 30506 caracteres]

_3074 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
