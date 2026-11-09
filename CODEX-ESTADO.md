# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 02:59** · publicado em **09/11/2026 03:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 02:59 · sessao adbb51c775ba · E1-FILA-V2-2-TRAVA · construtor · origem=Code

B1/B2 implementados:

| Arquivo::função | Alteração |
|---|---|
| [pergunta_no_app.py:175](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/pergunta_no_app.py:175)::`perguntar` | Conta chamados recusados em `recusados`, excluindo-os de `chamados`. |
| [perguntar_chamados_sem_pergunta.py:19](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19)::`Command.handle` | Imprime `colabs`, `chamados` e `recusados` separadamente. |
| [detectar_ausencias.py:802](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/ponto/management/commands/detectar_ausencias.py:802)::`Command._abrir_chamado` | Recusa gera uma linha em **stderr**, com pk e mensagem oficial. |
| [processar_alertas_turno.py:385](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/ponto/management/commands/processar_alertas_turno.py:385)::`Command._abrir_chamado_critico` | Recusa gera uma linha em **stdout**, com pk e mensagem oficial. |
| [processar_alertas_avancados.py:159](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/ponto/management/commands/processar_alertas_avancados.py:159)::`Command.handle` | Recusa gera uma linha em **stdout**, com pk e mensagem oficial. |

Grep dos leitores do dict encontrou como consumidor de produção o comando acima, agora ajustado ([importação:20](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20), [leitura:29](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29)):

```bash
rg -n 'pergunta_no_app.*import.*perguntar|pergunta_no_app\.perguntar|\bperguntar\(alvo' --glob '*.py' --glob '!**/migrations/**' .
```

Testes novos em [test_e1_fila_v2_2_consumidores.py:131](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:131)::`ConsumidoresRecusaSemBancoTest`. Os seis RED falharam antes da cura; após a cura, passaram ([RED:157](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:157)). [nao verificado]

Comandos reproduzíveis:

```bash
export PYTHONDONTWRITEBYTECODE=1
T=chamados.tests.test_e1_fila_v2_2_consumidores

python3 -B -m unittest -v "$T" -k RED

for caso in \
  MORDE_passivo_sem_checagem_da_recusa \
  MORDE_passivo_conta_lotes_em_vez_de_chamados_recusados \
  MORDE_comando_omite_recusados \
  MORDE_ausencias_sem_checagem_da_recusa \
  MORDE_turno_sem_checagem_da_recusa \
  MORDE_avancados_sem_checagem_da_recusa
do
  python3 -B -m unittest -v "$T" -k "$caso"
done

python3 -B -m unittest -q "$T" \
  chamados.tests.test_e1_fila_v2_2_recusa \
  chamados.tests.test_e1_fila_v2_2_trava
```

Resultados: **6 RED verdes; seis mutantes detectados, cada MORDE individual OK; bateria de 40 testes OK** ([MORDE:222](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:222)). Os testes executam os trechos reais por AST, sem banco; os crons completos ficam [nao verificado] ([limite do teste:1](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:1)). [nao verificado]

Ruff indisponível no ambiente; lint: [nao verificado].

## 09/11 02:56 · sessao 996a00785e71 · C7-DISPUTA-FECHA-POR-UM-JUIZ · censo · origem=Code

**CENSO**

O núcleo C7 e o escritor de Chamado já estão implementados nos sítios do plano. A falta principal encontrada é **idade ainda absolvendo indiretamente dentro do C6**; também restam apresentações que anunciam “pode fechar” sem consultar toda a resposta do C7. (`app/chamados/models.py:1856`; `app/chamados/juizes.py:674`; `app/chamados/services/explicador.py:137`; `app/chamados/tests/test_chokepoint_chamado_gate.py:152`)

Execução de suíte, impacto atual em produção e correspondência independente da cópia ao HASH informado: **[não verificado]**. [nao verificado]

**leitores:**

| Sítio | Resultado da leitura |
|---|---|
| `app/core/juizes.py:266`, `app/core/juizes.py:267` | C6 e C7 já registrados: `pergunta_viva` e `DisputaSupervisao.pode_fechar`. |
| `app/chamados/models.py:1856` | C7 exige ausência de respostas sem veredito **e** ausência de `fio_vivo`. |
| `app/chamados/models.py:1894`, `app/chamados/models.py:1904` | `.fechar` reaplica a guarda de resposta e consulta C6; fechamento normal recusa pergunta viva. |
| `app/chamados/models.py:3375` | `p.viva` já delega diretamente a `pergunta_viva`; não mantém regra independente. |
| `app/chamados/models.py:3336` | `q_vivas` é pré-filtro deliberadamente amplo; não representa a resposta exata do juiz. |
| `app/chamados/services/materializacao.py:1314` | Contagem de pendentes apenas compõe retorno; quem libera fechamento é `pode_fechar`, em `:1316`. |
| `app/chamados/services/becos.py:29` | Visibilidade, resposta e destino identificam o **recorte de beco**; `FECHAVEL` depende de C7 em `:33`, reaplicado em `:56`. |
| `app/chamados/services/acoes_disputa.py:130`, `app/chamados/services/fio.py:660` | Ainda barram **regularização/materialização** quando existe viva sem resposta. São guardas da ação anterior ao fechamento; a autorização final volta a C7 em `app/chamados/services/materializacao.py:1316`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83` | Pré-checagem usa somente `fio_vivo`; tenta `.fechar` em `:88` e captura a recusa adicional da porta em `:92`. Não contorna a porta. |

**Falta efetiva: relógio dentro do juiz consumido pelo fechamento.**

- `_dentro_da_janela_viva` usa `date.today()` e a janela corrente/anterior. (`app/chamados/juizes.py:569`, `app/chamados/juizes.py:575`, `app/chamados/juizes.py:601`)
- Em `_pergunta_materializada_de_fato`, pergunta carimbada por via sem testemunha recebe `True` simplesmente por estar **fora da janela**, antes da consulta ao fato. (`app/chamados/juizes.py:660`, `app/chamados/juizes.py:674`, `app/chamados/juizes.py:679`)
- No fallback sem slot, destino encerrado só é confrontado com a falta quando o dia está dentro da janela; fora dela, `_dest_ok` pode resolver a pergunta. (`app/chamados/juizes.py:743`, `app/chamados/juizes.py:764`, `app/chamados/juizes.py:774`)
- C6 restringe a rechecagem da falta à janela e depois consulta esse lastro; portanto o relógio ainda pode retirar vitalidade e liberar C7. Trata-se de caminho estático identificado; quantidade atualmente afetada: **[não verificado]**. (`app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1234`; `app/chamados/models.py:1861`)
- O caso C7 de silêncio testa disputa aberta há 45 dias com pergunta recente **sem carimbo**; não cobre esses dois caminhos carimbados. (`app/chamados/tests/test_c7_red_minimos.py:115`, `app/chamados/tests/test_c7_red_minimos.py:289`, `app/chamados/tests/test_c7_red_minimos.py:299`)

**Apresentação e outros usos, sem autorização de fechamento:**

| Sítio | O que decide |
|---|---|
| `app/chamados/services/explicador.py:137` | Produz `pode_fechar = not fio_vivo(ps)`, sem a guarda adicional de respostas sem veredito. O consumidor transforma isso em orientação “Fechar/regularizar”; há possibilidade de apresentação divergir da porta. (`app/chamados/services/desenlace.py:63`; `app/chamados/models.py:1861`) |
| `app/chamados/services/fio.py:312` | Carrega a explicação para apresentação; pode passar perguntas do chamado, enquanto C7 avalia a disputa inteira. (`app/chamados/models.py:1861`) |
| `app/chamados/relatorios_disputa.py:18` | Classifica `PRONTA_FECHAR` apenas pela ausência de vivas; não consulta a guarda adicional C7. (`app/chamados/relatorios_disputa.py:20`) |
| `app/chamados/services/explicador.py:93` | Lastro explica o estado individual; a decisão agregada está separada em `:137`. |
| `app/chamados/services/fio.py:283`, `app/chamados/services/raiox.py:185` | `.viva` e lastro/cache escolhem disponibilidade de ação na tela. |
| `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133` | `q_vivas` alimenta crédito/badge. |
| `app/chamados/services/janela_colab.py:63`, `app/chamados/services/cobranca.py:229` | Visibilidade fora da janela e contagem para cobrança/SLA. |
| `app/chamados/services/aviso_pendencia.py:11`, `app/chamados/services/pergunta_no_app.py:47`, `app/chamados/services/vigia_fio.py:748` | Pré-filtro SQL seguido de C6 para aviso, app e vigia, respectivamente. |
| `app/chamados/management/commands/arquivar_competencia_encerrada.py:158` | `p.viva` impede arquivamento de **Chamado** com conversa viva; não fecha disputa. |
| `app/chamados/services/disputa_emissao.py:1781` | Lastro protege pergunta respondida contra carimbo S128; fechamento fica separado em `:1887`. |

A reaplicação estática do detector C7 não encontrou `q_vivas`, lastro ou `.viva` autorizando diretamente fechamento. Esse detector examina `if/while` cujo corpo fecha, com aliases e derivações locais; **não alcança toda dependência indireta**, como a janela acima. (`app/chamados/tests/test_c7_red_minimos.py:475`, `app/chamados/tests/test_c7_red_minimos.py:491`, `app/chamados/tests/test_c7_red_minimos.py:496`)

**escritores:**

O estado aberta/fechada da disputa é `fechada_em`; a propriedade `aberta` deriva desse campo. O único escritor material de **fechamento** encontrado em produção foi `DisputaSupervisao.fechar`, que salva data, autor, resolução e motivo juntos, com trilha. (`app/chamados/models.py:1754`, `app/chamados/models.py:1832`, `app/chamados/models.py:1940`, `app/chamados/models.py:1944`, `app/chamados/models.py:1945`)

Os **15 sítios de chamada direta** encontrados:

| Sítio | Porta e consulta anterior |
|---|---|
| `app/api/foto_recebida.py:80` | `.fechar` normal; valida antes em `:79`; C6 é consultado dentro da porta. |
| `app/chamados/models.py:1958` | `admin_encerra` → `.fechar`, exceção nomeada; exige motivo preenchido em `:1955`. |
| `app/chamados/models.py:2080` | `remover_motivo` → `.fechar` normal quando acabam motivos; C6 fica na porta. |
| `app/colaboradores/services/desligamento.py:142` | `.fechar`, exceção `desligamento`; consulta respostas sem veredito em `:140`. |
| `app/ponto/services/entrada_sincronizada.py:59` | `.fechar` normal; `pode_fechar` em `:58`. |
| `app/chamados/services/becos.py:57` | `.fechar` normal; `pode_fechar` em `:56`. |
| `app/chamados/services/competencia_trancada.py:204` | `.fechar`, exceção `competencia_trancada`; `pode_fechar` em `:202`. |
| `app/chamados/services/disputa_emissao.py:1216` | `.fechar` normal; `pode_fechar` em `:1210`; ausência de ata ainda impede autoencerramento em `:1211`. |
| `app/chamados/services/disputa_emissao.py:1888` | `.fechar` normal; `pode_fechar` em `:1887`; pergunta em voo adia fechamento em `:1885`. |
| `app/chamados/services/materializacao.py:586` | `.fechar` normal; `pode_fechar` em `:585`. |
| `app/chamados/services/materializacao.py:1333` | `.fechar` normal; `pode_fechar` em `:1316`. |
| `app/chamados/reconciliador.py:412` | `.fechar` normal; `pode_fechar` em `:411`. |
| `app/chamados/reconciliador.py:612` | `.fechar` normal; `pode_fechar` em `:611`. |
| `app/chamados/reconciliador.py:1171` | `.fechar` normal; respostas sem veredito em `:1163`, C7 em `:1170`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | `.fechar` normal; `fio_vivo` em `:83`; recusa da porta capturada em `:92`. |

Esses chamadores não calculam idade para fechar; a dependência de relógio identificada está **dentro do C6**, alcançado pela porta. (`app/chamados/juizes.py:674`, `app/chamados/juizes.py:764`, `app/chamados/juizes.py:1234`; `app/chamados/models.py:1904`)

Não foi encontrado `update/bulk_update` de produção fechando disputa fora da porta. As outras atribuições materiais de `fechada_em` encontradas são **reaberturas**, que escrevem `None`. (`app/chamados/models.py:1944`, `app/chamados/models.py:2157`, `app/chamados/models.py:2169`, `app/chamados/models.py:2181`, `app/chamados/models.py:2185`) Escritor dinâmico ou externo à cópia: **[não verificado]**.

Os caminhos indiretos relevantes também chegam às portas:

- Signals de posto/âncora → `remover_motivo` → `.fechar`. (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`; `app/chamados/models.py:2080`)
- Signal de pergunta → serviço de becos → C7 → `.fechar`. (`app/chamados/signals.py:223`; `app/chamados/services/becos.py:56`)
- Cron de becos está configurado com `--alarme`; o comando só aplica quando `--apply` e sem `--alarme`. (`app/config/crons.py:663`; `app/chamados/management/commands/fechar_becos_disputa.py:30`)
- S143 → resolver perguntas → C7 → `.fechar`. (`app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1887`)
- Ato de trancar e comando de passivo usam o mesmo serviço de competência trancada. (`app/ponto/services/fechamento.py:1427`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`; `app/chamados/services/competencia_trancada.py:204`)
- Materialização comum e passivo compartilham a cauda de fechamento. (`app/chamados/services/materializacao.py:1378`; `app/chamados/services/passivo_validado.py:405`)
- O signal posterior ao fechamento da disputa reconcilia **Chamado**, não regrava fechamento da disputa. (`app/chamados/signals.py:42`)

**L-111 e desligamento:**

| Item | Situação na cópia |
|---|---|
| `reconciliar_disputa_zumbi` | Command ausente, sem chamada executável encontrada; lápide em `app/config/crons.py:835`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:57`. |
| `heal_disputas_legado` | Command ausente, sem chamada executável encontrada; lápide em `app/config/crons.py:1444`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:61`. |
| `veredito_do_explicador` | Definição e chamadas ausentes; lápide em `app/chamados/juizes.py:2115`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:51`. |
| `disputa_fecha_na_cascata_de_desligamento` | Definição e chamadas ausentes; lápide em `app/chamados/juizes.py:2116`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:54`. |
| Expiração por silêncio de 30 dias | Bloco removido, com lápide; teste cobra ausência das funções e da opção antiga. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:61`; `app/chamados/tests/test_c7_red_minimos.py:307`) |
| `.update` do desligamento | **Já saiu**. Hoje há guarda em `app/colaboradores/services/desligamento.py:140` e `.fechar(..., motivo=FECHOU_DESLIGAMENTO)` em `:142`. |

Os testes remanescentes dos quatro apagados verificam ausência; não os invocam. Há escritas cruas de estado legado em fixtures, separadas do inventário de produção. (`app/chamados/tests/test_c7_red_minimos.py:51`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:141`; `app/chamados/tests/test_signals.py:268`)

**Escritor de Chamado — cinco mecânicos:**

| Origem | Escrita atual |
|---|---|
| `app/ponto/management/commands/reconciliar_grade.py:454` | `retirar_contexto`. |
| `app/ponto/management/commands/reconciliar_grade.py:473` | `renovar_contexto` e `retirar_contexto` em `:474`. |
| `app/ponto/management/commands/gerar_celulas.py:255` | `renovar_contexto`. |
| `app/chamados/management/commands/nomear_marco_por_lampada.py:109` | `renovar_contexto`; evento condicionado à escrita em `:111`. |
| `app/ponto/views.py:288` | `renovar_contexto`, preservando o restante do contexto. |

A persistência desses atos fica nas portas de contexto; **nenhum dos cinco sítios continua salvando cru**. (`app/chamados/services/contexto.py:104`, `app/chamados/services/contexto.py:127`)

**Escritor de Chamado — seis operações/sítios:**

| Origem | Campo | Porta atual |
|---|---|---|
| `app/chamados/services/cobranca.py:178` | `prazo_sla` | `definir_prazo_sla`; persistência em `app/chamados/models.py:591`. |
| `app/chamados/services/disputa_emissao.py:1642` | `prazo_sla` | Mesma porta, `app/chamados/models.py:573`. |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` | `escalonado_para_dp`, instante | `marcar_escalonado_dp`; persistência em `app/chamados/models.py:610`. |
| `app/chamados/services/abertura.py:115` | Nome real: `ticket_id_glpi` | `apontar_ticket_glpi`; persistência em `app/chamados/services/vinculos.py:117`. |
| `app/chamados/services/limbo_folgas.py:69` | `descricao` | `reescrever_texto`; persistência em `app/chamados/services/texto.py:36`. |
| `app/api/views.py:3160` | `titulo`, `assunto` | `reescrever_texto`, definida em `app/chamados/services/texto.py:21`. |

São seis **sítios**, com SLA em dois deles; todos já passam por porta. A enumeração está também no selo E1. (`app/chamados/tests/test_porta_chamado_e1.py:132`)

A matriz já declara `teste=` e `idempotencia=`. O censo AST atribuível retornou zero escritores de Chamado fora dos módulos declarados como porta; seu alcance exclui testes/migrations e admite escritas não atribuíveis. (`app/core/contratos_estruturais.py:497`, `app/core/contratos_estruturais.py:500`; `app/core/portas.py:196`; `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:54`)

Há documentação envelhecida: a nota que ainda acusa `.update` no desligamento e a que lista quatro vizinhos fora de porta não descrevem mais essas chamadas atuais. (`app/core/contratos_estruturais.py:730`, `app/core/contratos_estruturais.py:1015`; `app/colaboradores/services/desligamento.py:142`; `app/chamados/tests/test_chokepoint_chamado_gate.py:152`, `app/chamados/tests/test_chokepoint_chamado_gate.py:161`)

**DECISÕES:**

1. **Encaixe da retirada de idade:** corrigir os ramos temporais dentro do C6 existente, preservando C7 como consumidor; escolher a organização dos auxiliares e cobrir os caminhos carimbados e sem slot. A lei já proíbe absolvê-los por relógio; não exige juiz novo. (`app/docs/BACKLOG.md:1466`; `app/chamados/juizes.py:674`, `app/chamados/juizes.py:764`)

2. **Unificar a resposta apresentada:** decidir como explicador e relatório consumirão `d.pode_fechar()` e como receberão o conjunto completo da disputa. Hoje seus booleanos usam só C6, enquanto C7 acrescenta respostas sem veredito. (`app/chamados/services/explicador.py:137`; `app/chamados/services/fio.py:312`; `app/chamados/relatorios_disputa.py:20`; `app/chamados/models.py:1861`)

3. **Trava e releitura da porta:** definir o encaixe da avaliação sob trava. Atualmente respostas e vivas são lidas antes do `atomic` e da tomada de travas; possível divergência concorrente é inferência estática, reprodução **[não verificado]**. (`app/chamados/models.py:1894`, `app/chamados/models.py:1903`, `app/chamados/models.py:1913`, `app/chamados/models.py:1918`)

4. **Tela de encerramento:** falta oferecer a porta existente por uma ação fina com motivo obrigatório; o backlog a mantém como obra própria `C7-TELA-ADMIN-ENCERRA`. Nome da rota, botão e apresentação são desenho; a regra já está na porta. (`app/docs/BACKLOG.md:1469`; `app/chamados/models.py:1951`, `app/chamados/models.py:1955`)

5. **Não reconstruir E1/E2 do escritor de Chamado:** os onze sítios pedidos já migraram; resta reconciliar a descrição antiga do backlog/matriz com o código e manter os selos existentes. (`app/docs/BACKLOG.md:1466`; `app/chamados/tests/test_chokepoint_chamado_gate.py:152`, `app/chamados/tests/test_chokepoint_chamado_gate.py:161`; `app/core/contratos_estruturais.py:497`)

6. **Sobreposição documental com obras em curso:**

   | Arquivo | Sobreposição a coordenar |
   |---|---|
   | `chamados/services/disputa_emissao.py` | Autoencerramento/S128 do censo e fila `RETRATADA-CONVERSA-E-REABRIR`; a E4 também prevê reduzir escritores de estado da pergunta. (`app/chamados/services/disputa_emissao.py:1210`, `:1887`; `app/docs/BACKLOG.md:782`, `:1859`) |
   | `chamados/services/fio.py` | Explicação/regularização e migração dos leitores para `tarefa_do_dia` na E3, que absorve FIO-MUDO-B. (`app/chamados/services/fio.py:312`, `:660`; `app/docs/BACKLOG.md:1807`, `:1858`) |
   | `chamados/models.py` | Porta C7 já contém mudanças de trava/decisão da E1; E4 prevê encolher portas de estado da pergunta. (`app/chamados/models.py:1914`, `:1930`; `app/docs/BACKLOG.md:1859`) |
   | `chamados/juizes.py` | A retirada de idade toca C6, também nomeado pela obra E2 e pelo passivo O261-b. (`app/chamados/juizes.py:674`; `app/docs/BACKLOG.md:1857`, `:2237`) |

   Conflitos efetivos de merge entre branches/worktrees: **[não verificado]**.

As faltas identificadas acima são de implementação, apresentação e encaixe; a lei fornecida já determina juiz único, exceções por porta e proibição da absolvência por idade. Não identifiquei decisão normativa adicional necessária para essas correções. (`app/docs/BACKLOG.md:1466`)

perguntas de lei: 0

## 09/11 02:54 · sessao f4a9775a4f46 · - · - · origem=Ronald (tmux)

Censo por AST de 3.459 arquivos `.py` da cópia, sem falhas de parse; incluídos arquivos fora de `app`. Não há definição nem chamada AST dos quatro nomes L-111, inclusive `call_command('nome')`; referências restantes são lápides, documentação e testes de ausência (`app/chamados/juizes.py:2115`; `app/config/crons.py:835`; `app/config/crons.py:1444`; `app/chamados/tests/test_c7_red_minimos.py:45`). Execução de produção, Git/HEAD real e banco: [não verificado].

**Escritor material da disputa**

- `DisputaSupervisao.fechar`: atribui `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` e salva somente esses campos; consulta respostas sem veredito e `pergunta_viva`, grava trilha; `app/chamados/models.py:1894`, `:1904`, `:1940`, `:1944`, `:1945`.
- `admin_encerra` exige motivo preenchido e delega à porta; `app/chamados/models.py:1951`, `:1955`, `:1958`.
- Reaberturas escrevem **abertura**, não fechamento: `reabrir_admin` zera campos e salva; `app/chamados/models.py:2157`, `:2169`. `reabrir_sistema` idem; `:2181`, `:2185`.
- Nenhum `update`/`bulk_update` de `fechada_em` de produção encontrado na varredura; contrato explícito e tripwire em `app/core/portas.py:424` e `app/chamados/tests/test_c7_red_minimos.py:574`. Limite: AST estático não prova inexistência de escritor dinâmico externo [não verificado].
- `DisputaSupervisao` usa `fechada_em` como estado aberta/fechada (`app/chamados/models.py:1754`, `:1832`); `status_local` refere-se a Chamado, não a esse campo da disputa (`app/chamados/signals.py:33`).

**Todos os 15 chamadores diretos de `.fechar` de disputa**

| Local | Consulta anterior / seleção | Porta |
|---|---|---|
| `app/api/foto_recebida.py:80` | Último motivo foto; valida respostas em `:79`; sem pré-`pode_fechar` | Normal; própria porta consulta C6 |
| `app/chamados/models.py:1958` | Motivo obrigatório `:1955` | `admin_encerra` |
| `app/chamados/models.py:2080` | Sem motivos restantes `:2074`; sem pré-`pode_fechar` | Normal; própria porta consulta C6 |
| `app/colaboradores/services/desligamento.py:142` | `respostas_sem_veredito` em `:140` | Exceção `desligamento` |
| `app/ponto/services/entrada_sincronizada.py:59` | `pode_fechar` em `:58`; seleção de pai com um único órfão `:54` | Normal |
| `app/chamados/services/becos.py:57` | `pode_fechar` em `:56` | Normal |
| `app/chamados/services/competencia_trancada.py:204` | `pode_fechar` em `:202` | Exceção `competencia_trancada` |
| `app/chamados/services/disputa_emissao.py:1216` | `pode_fechar` em `:1210`; sem ata impede fecho em `:1211` | Normal |
| `app/chamados/services/disputa_emissao.py:1888` | `pode_fechar` em `:1887`; pergunta em voo impede em `:1885` | Normal |
| `app/chamados/services/materializacao.py:586` | `pode_fechar` em `:585` | Normal |
| `app/chamados/services/materializacao.py:1333` | `pode_fechar` em `:1316`; contagem `perguntas_vivas_de` em `:1314` só compõe retorno | Normal |
| `app/chamados/reconciliador.py:412` | `pode_fechar` em `:411` | Normal |
| `app/chamados/reconciliador.py:612` | `pode_fechar` em `:611` | Normal |
| `app/chamados/reconciliador.py:1171` | Respostas sem veredito em `:1163`; `pode_fechar` em `:1170` | Normal |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | `fio_vivo` em `:83`; porta recusa respostas e comando captura `ValueError` em `:92` | Normal |

Nenhum desses **chamadores diretos** calcula idade para executar fechamento; porém o juiz consultado mantém dependência indireta de relógio: `app/chamados/juizes.py:674`–`:675`, `:764`, alcançados por C6 em `:1234` e pela porta em `app/chamados/models.py:1904`. Portanto não afirmar “idade totalmente eliminada”.

**Desligamento**

O update direto saiu: há comentário explícito em `app/colaboradores/services/desligamento.py:133`, guarda de respostas em `:140`, chamada `.fechar(... motivo=FECHOU_DESLIGAMENTO)` em `:142`. O teste busca especificamente `update(fechada_em=...)` em `app/chamados/tests/test_c7_red_minimos.py:34`.

**Signals, crons e commands indiretos**

- `app/escala/signals.py:161` e `app/colaboradores/signals.py:77` chamam `remover_motivo`, que fecha pela porta em `app/chamados/models.py:2080`; a seleção “acabaram motivos” continua em `:2074`, mas a porta valida C6.
- Evento de Pergunta chama fechamento de beco em `app/chamados/signals.py:223`–`:225` → `app/chamados/services/becos.py:74`, `:78` → `.fechar` em `:57`; classificação própria de beco usa visibilidade/resposta/status do destino em `:29`–`:33`, e decisão final consulta `pode_fechar`.
- Command beco aplica via serviço em `app/chamados/management/commands/fechar_becos_disputa.py:57`; sua agenda usa **`--alarme`**, que não aplica (`app/config/crons.py:663`; `app/chamados/management/commands/fechar_becos_disputa.py:30`).
- S143 chama resolver ao salvar status terminal de Chamado (`app/chamados/signals.py:104`, `:115`) → wrapper em `app/chamados/services/disputa_emissao.py:1706`, `:1722` → fecho em `:1888`.
- O post-save de disputa fecha **Chamado**, não disputa, por `ch.reconciliar()` em `app/chamados/signals.py:42`; os demais receivers de disputa retomam regularizações e materialização (`:134`, `:163`).
- Tranca dispara pelo ato em `app/ponto/services/fechamento.py:1427`; comando de passivo chama o mesmo serviço em `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`; está fora de rotina em `app/config/crons.py:1268`.
- `encerrar_perguntas_com_fato` é passivo declarado fora de rotina em `app/config/crons.py:1329`; escreve disputa exclusivamente pela chamada da tabela.
- Materialização comum e passivo compartilham `_fechar_disputa_se_quitada`: `app/chamados/services/materializacao.py:1378`; `app/chamados/services/passivo_validado.py:405`.
- Reconciliador de colaborador alcança fechamento por célula e órfãs em `app/chamados/reconciliador.py:1365`, `:1375`; lavra sem turno em `:1615`.
- Command `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` chama `.fechar` em **ChamadoColaborador** (universo em `:49`, `:55`), portanto não entra nos 15 de disputa.

**L-111**

- `veredito_do_explicador` e `disputa_fecha_na_cascata_de_desligamento`: apagados; lápide em `app/chamados/juizes.py:2115`–`:2119`; testes verificam ausência de definição em `app/chamados/tests/test_c7_red_minimos.py:51`, `:54`.
- `reconciliar_disputa_zumbi`: arquivo ausente; lápide de registro em `app/config/crons.py:835`; teste verifica ausência de arquivo em `app/chamados/tests/test_c7_red_minimos.py:57`.
- `heal_disputas_legado`: arquivo ausente; lápide em `app/config/crons.py:1444`; teste verifica ausência em `app/chamados/tests/test_c7_red_minimos.py:61`.
- Nenhum chamador executável produção/teste desses quatro nomes; testes remanescentes testam ausência, não invocam portas apagadas (`app/chamados/tests/test_c7_red_minimos.py:51`–`:63`). Documentação histórica ainda cita implementação antiga, por exemplo `app/docs/TICKETS.md:1070` e `app/chamados/tests/test_porta_disputa_ciclo.py:51`; não tratar prosa antiga como código vivo.
- Bloco de expiração de 30 dias foi apagado com lápide em `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`–`:64`. Ainda há prosa antiga no cabeçalho, explicitamente marcada revogada em `:38`.

**Fixtures/migrations**

Migrations apenas declaram campos/choices (`app/chamados/migrations/0013_disputasupervisao.py:24`; `app/chamados/migrations/0049_disputa_motivo_fechamento.py:15`). Há escritores crus em **testes** que montam estado legado, por exemplo `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:141`, `:168`; `app/chamados/tests/test_reabrir_orfas_enterradas.py:81`; `app/chamados/tests/test_selo_universo_reconciliador.py:104`; `app/chamados/tests/test_signals.py:268`. Eles não são escritores de produção; nenhum teste foi executado [não verificado].

## 09/11 02:53 · sessao f5e8149ba135 · E1-FILA-V2-2-TRAVA · verificador · origem=Code

**Não daria o aval completo: A1 continua parcial.** As duas telas distinguem a recusa, mas quatro chamadores descartam o retorno e o passivo ainda conta chamados recusados como “APLICADO”. (`app/chamados/services/acoes_disputa.py:225`; `app/chamados/services/cobrar_dia.py:50`; `app/chamados/services/pergunta_no_app.py:186`; `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`)

1. **A1 — recusa falante e contagem**

   | Chamador | Comportamento diante de `REPASSE_RECUSADO` |
   |---|---|
   | `acoes_disputa` | Testa identidade antes de testar falsidade ou acessar `.pk`; devolve aviso explícito. `app/chamados/services/acoes_disputa.py:225`, `app/chamados/services/acoes_disputa.py:229` |
   | `cobrar_dia` | Testa identidade e devolve `ok=False`; a view apresenta aviso. `app/chamados/services/cobrar_dia.py:50`; `app/chamados/views_cobrar.py:326` |
   | `pergunta_no_app` | Descarta retorno, soma `len(chs)` e devolve os totais; o comando imprime **APLICADO**, inclusive para repasses recusados. `app/chamados/services/pergunta_no_app.py:186`; `app/chamados/services/pergunta_no_app.py:187`; `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29` |
   | `detectar_ausencias` | Descarta retorno. O catch genérico não anuncia uma recusa que agora chegou como retorno normal. `app/ponto/management/commands/detectar_ausencias.py:801`; `app/ponto/management/commands/detectar_ausencias.py:802` |
   | `processar_alertas_turno` | Descarta retorno e continua devolvendo o chamado. `app/ponto/management/commands/processar_alertas_turno.py:384`; `app/ponto/management/commands/processar_alertas_turno.py:387` |
   | `processar_alertas_avancados` | Descarta retorno e continua. “Chamado criado” corresponde ao Ch criado antes; esse contador não demonstra repasse concluído. `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/ponto/management/commands/processar_alertas_avancados.py:158`; `app/ponto/management/commands/processar_alertas_avancados.py:162` |

   **Nenhum desses seis usa atualmente o objeto truthy como Disputa:** dois fazem comparação por identidade; quatro ignoram o resultado. A contagem enganosa concreta está no passivo. As referências de cada consumidor estão na tabela acima.

2. **Atomic externo, estado parcial e evento**

   O `except` está fora **somente do atomic próprio do repasse**. Existe caminho dentro de transação externa: `_gravar_estado` abre atomic, trava e salva Ch; o signal chama cartório, que usa o emissor de ausências e pode alcançar o repasse. (`app/chamados/services/disputa_emissao.py:1049`; `app/chamados/services/disputa_emissao.py:1290`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/signals.py:203`; `app/ponto/services/cartorio.py:1964`; `app/ponto/services/cartorio.py:1990`; `app/ponto/management/commands/detectar_ausencias.py:801`)

   **Sim: o repasse pode ser desfeito e o trabalho anterior do chamador permanecer.** Isso é inclusive exigido pelo teste do marcador anterior. Em `cobrar_dia`, toque e julgamento precedem a chamada recusada; portanto “nada feito” descreve o repasse, não todo o pedido. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:238`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:265`; `app/chamados/services/cobrar_dia.py:39`; `app/chamados/services/cobrar_dia.py:41`; `app/chamados/services/cobrar_dia.py:49`; `app/chamados/services/disputa_emissao.py:1021`)

   **O evento não sobrevive ao rollback externo.** Seu gravador abre atomic na mesma conexão, sem persistência independente. Além disso, é best-effort: pode retornar `None`, enquanto o agrupador devolve o sentinel sem conferir a gravação. Logo, `REPASSE_RECUSADO` **não garante evento persistido**. (`app/core/observ.py:103`; `app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/chamados/services/disputa_emissao.py:1293`; `app/chamados/services/disputa_emissao.py:1296`)

   Não encontrei segundo produtor nominal de `repasse_recusado`: a porta produz o evento e delega a persistência à autoridade genérica existente. (`app/chamados/services/disputa_emissao.py:1293`; `app/core/observ.py:134`)

3. **A2 e mutantes da volta 2**

   **A2 ganhou mordida no código do teste:** o novo cenário chama a porta real sem wrapper da helper nem cauda injetada, exige uma Disputa, exige C estrangeiro antes da primeira D e verifica a ordem. O MORDE injeta exatamente a lambda local antes do `with`. **Passagem efetiva no Postgres desta volta: [nao verificado].** (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:190`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:199`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:202`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:204`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:211`)

   Para as remoções pedidas, há testes que rejeitam: ausência do catch, retorno `None`, ausência do evento e remoção das duas checagens das telas. **Não identifiquei sobrevivente dessas remoções diretas** nos oráculos examinados; isso não prova a suíte completa da volta 2 verde. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:164`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:173`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:179`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:188`)

   A lacuna permanece nos quatro consumidores ignorados: os testes de recusa exercitam agrupador, iniciar e cobrar, não exigem correção da contagem do passivo nem anúncio nos três crons. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:126`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:244`; `app/chamados/services/pergunta_no_app.py:187`)

4. **S1 / S2 / S3**

   - **S1:** a fixture move a P materializada para `alvo`, preservando `ch2` sem P; conserva o assert de chegada à cauda e o de reabertura da D. A causa anterior foi tratada sem afrouxar esses asserts; alcançar a cauda no Postgres permanece **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:79`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:118`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:120`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:127`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:129`)
   - **S2:** B023 foi tratado na origem, capturando variáveis pelos argumentos default das closures. Ruff completo nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_trava.py:254`; `app/chamados/tests/test_e1_fila_v2_2_trava.py:270`; `app/chamados/tests/test_e1_fila_v2_2_trava.py:367`)
   - **S3:** **1196 é a linha real**, `data_entrada = _tz.localtime(batida_origem.timestamp).date()`. O registro corresponde ao sítio B1 e o selo compara as linhas reais por igualdade. (`app/chamados/services/disputa_emissao.py:1196`; `app/core/tests/test_medida_turno_do_par.py:268`; `app/core/tests/test_medida_turno_do_par.py:533`; `app/core/medida_turno_do_par.py:225`)

5. **Classes históricas de reprovação**

   **Segundo juiz / derivação paralela / relógio novo:** não identifiquei nas funções acrescentadas. O fecho percorre relações; o mérito do fechamento continua em `pode_fechar()`. (`app/chamados/services/disputa_emissao.py:957`; `app/chamados/services/disputa_emissao.py:959`; `app/chamados/services/disputa_emissao.py:974`; `app/chamados/services/disputa_emissao.py:1275`)

   **Ordem interna:** a helper pede C ordenados, depois D/Ch/P pela autoridade existente, e recusa crescimento. As pré-travas da porta externa continuam existindo; não foram curadas aqui. (`app/chamados/services/disputa_emissao.py:1001`; `app/chamados/services/disputa_emissao.py:1007`; `app/chamados/services/disputa_emissao.py:1012`; `app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2696`; `app/chamados/models.py:2700`; `app/chamados/models.py:290`)

   **Terminal reatável:** está incluído pela semente `VIVOS + ENCERRADOS`; HIST também procura `ENCERRADOS`. A exclusão de cancelado/superado dessa semente não demonstra quebra desse aval. (`app/chamados/services/disputa_emissao.py:960`; `app/chamados/reconciliador.py:1582`; `app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:31`)

   **Retorno com três significados:** está documentado e separado por identidade nas telas, mas não consumido integralmente. Atenção adicional: `None` pode ocorrer **depois de complementar perguntas e gravar D/Ch**; não equivale a “zero escrita”. (`app/chamados/services/disputa_emissao.py:1029`; `app/chamados/services/disputa_emissao.py:1078`; `app/chamados/services/disputa_emissao.py:1081`; `app/chamados/services/disputa_emissao.py:1091`)

6. **Quem escreve e quem lê**

   | Objeto | Escritor | Leitores |
   |---|---|---|
   | Retorno | `None`: `app/chamados/services/disputa_emissao.py:1046`, `app/chamados/services/disputa_emissao.py:1091`; D: `app/chamados/services/disputa_emissao.py:1285`, `app/chamados/services/disputa_emissao.py:1288`; sentinel: `app/chamados/services/disputa_emissao.py:1296` | Identidade: `app/chamados/services/acoes_disputa.py:225`, `app/chamados/services/cobrar_dia.py:50`; quatro descartadores identificados no item 1. |
   | Evento `repasse_recusado` | Produção: `app/chamados/services/disputa_emissao.py:1293`; persistência: `app/core/observ.py:134` | Leitor específico encontrado: teste em `app/chamados/tests/test_e1_fila_v2_2_integracao.py:262`. Leitor genérico de produção: auditoria em `app/core/views.py:705`, com filtro livre de ação em `app/core/views.py:710`. |

   O histórico dos chamados não inclui esse evento de modelo `Colaborador`, pois filtra `Chamado`/`ChamadoColaborador`. Consumidor operacional específico, retry e visibilidade efetiva em produção: **[nao verificado]**. (`app/chamados/services/historico.py:86`; `app/chamados/services/disputa_emissao.py:1293`)

   Executei os **23 testes sem banco: OK**, usando `python3 -B`; relatório persistido dessa execução: **[nao verificado]**. Postgres e suíte completa da volta 2: **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_trava.py:1`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:1`)

## 09/11 02:53 · sessao 5149a4a62a93 · - · - · origem=Ronald (tmux)

Censo de leitores por AST/leitura, sem alteração de arquivos:

- **Juiz C7 existente:** `app/chamados/models.py:1856` → `pode_fechar()` exige ausência de `respostas_sem_veredito()` e ausência de `fio_vivo`; `app/chamados/models.py:1834` define a primeira lista; `app/chamados/juizes.py:1253` define `fio_vivo` como `any(pergunta_viva(p))`. A porta reaplica a guarda de resposta e o C6 em `app/chamados/models.py:1894` e `app/chamados/models.py:1904`.
- **Registro vivo:** autoridades C6/C7 declaradas em `app/core/juizes.py:266` e `app/core/juizes.py:267`.
- **`p.viva` já não é cópia:** retorna diretamente `pergunta_viva(self)` em `app/chamados/models.py:3375`; `q_vivas` declara ser pré-filtro amplo, não valor exato, em `app/chamados/models.py:3336`.

**Leitores que efetivamente liberam fechamento e já consultam C7:**

| Local | Decisão |
|---|---|
| `app/chamados/services/disputa_emissao.py:1210` | Autoencerramento após emissão usa `pode_fechar`; ausência de ata ainda impede autoencerramento em `:1211`. |
| `app/chamados/services/disputa_emissao.py:1887` | S128 usa `pode_fechar`; pergunta em materialização impede reentrada em `:1885`. |
| `app/chamados/services/materializacao.py:585` | Ausência declarada: `pode_fechar`. |
| `app/chamados/services/materializacao.py:1316` | Quitação granular: `pode_fechar`. O contador `pendentes` de `:1314` apenas compõe retorno, não decide fechamento. |
| `app/chamados/reconciliador.py:411` | Reconciliação por pergunta: `pode_fechar`. |
| `app/chamados/reconciliador.py:611` | Retratação por célula: `pode_fechar`. |
| `app/chamados/reconciliador.py:1163` | Respostas sem veredito são relatadas; autorização final por `pode_fechar` em `:1170`. |
| `app/chamados/services/becos.py:33` | Classificação `FECHAVEL` vem de C7; `:56` reaplica antes da porta. O recorte `vis` de `:29` identifica beco, não libera fechamento. |
| `app/chamados/services/competencia_trancada.py:153` | Medição usa `pode_fechar`; aplicação reaplica em `:202` e chama porta com motivo em `:204`, sem carimbar perguntas. |
| `app/ponto/services/entrada_sincronizada.py:58` | Disputa existente usa `pode_fechar`. |
| `app/api/foto_recebida.py:79` | Valida antes e chama diretamente `.fechar` em `:80`; o juiz fica na porta. O recorte por motivos de `:70` escolhe aparar ou tentar fechar, não permite bypass. |

**Resíduos de decisão/apresentação a distinguir:**

- `app/chamados/services/acoes_disputa.py:130` e `app/chamados/services/fio.py:660`: recusam a ação de **regularização/materialização** se alguma pergunta viva está sem resposta. São guardas anteriores à materialização, não autorização de fechamento; fechamento real volta a C7 em `app/chamados/services/materializacao.py:1316`.
- `app/chamados/services/explicador.py:137`: rótulo `pode_fechar` usa apenas `not fio_vivo(ps)`, sem a guarda `respostas_sem_veredito` de C7. Seu consumidor de produção é apresentação: `app/chamados/services/fio.py:312`; `app/chamados/services/desenlace.py:63` transforma o booleano em texto “Fechar/regularizar”.
- `app/chamados/relatorios_disputa.py:18`: declara `PRONTA_FECHAR` quando não há vivas, também sem consultar a guarda adicional C7 (`:20`); é relatório, não escritor (`coletar` em `:26`).
- `app/chamados/services/explicador.py:93`: lastro continua usado para explicar estado individual; a decisão agregada já usa C6 em `:137`.
- `app/chamados/services/fio.py:283`: `.viva` + lastro/cache só escolhe presença de ação na tela.
- `app/chamados/services/raiox.py:72` e `:133`: `q_vivas` + veredito persistido constroem crédito/badge; não fecham disputa. Lastro direto é fallback de botão em `:185`.
- `app/chamados/services/janela_colab.py:63`: `q_vivas` amplia visibilidade de chamados fora da janela; não fecha disputa.
- `app/chamados/services/cobranca.py:229`: `q_vivas` alimenta contagem de pessoas caladas/SLA; não fecha disputa.
- `app/chamados/services/aviso_pendencia.py:11` e `:13`: pré-filtro SQL seguido de C6, só aviso.
- `app/chamados/services/pergunta_no_app.py:47` e `:53`: pré-filtro SQL seguido de C6, lista do app.
- `app/chamados/services/vigia_fio.py:748` e `:755`: pré-filtro SQL seguido de C6, conta vivas em disputa fechada.
- `app/chamados/management/commands/arquivar_competencia_encerrada.py:158`: `any(p.viva)` impede arquivamento de **chamado** com conversa viva, sem fechamento da disputa; expiração por silêncio foi removida, com lápide em `:61`.
- `app/ponto/services/veredito_celula.py:180`: `q_vivas` seleciona perguntas para a decisão humana do dia; carimbo pela porta da pergunta em `:189`, reconciliação de chamados em `:202`; não escreve fechamento da disputa diretamente.
- `app/chamados/services/disputa_emissao.py:1781`: lastro protege pergunta respondida contra o carimbo S128; autorização de fechamento está separada, em C7 (`:1887`).

**Idade ainda subsiste indiretamente no juiz C6 — lacuna relevante:**

1. `app/chamados/juizes.py:569` / `:575` / `:601`: `_dentro_da_janela_viva` depende de `date.today()` e competências corrente/anterior.
2. `app/chamados/juizes.py:660`: pergunta com via sem testemunha e carimbo chega ao retorno **True por estar fora da janela**, em `:674`–`:675`, dispensando a rechecagem do fato que existe em `:679`.
3. `app/chamados/juizes.py:764`: fallback de slot ausente só contradiz destino encerrado com o fato quando dentro da janela; fora dela aceita `_dest_ok` em `:774`.
4. `app/chamados/juizes.py:1217`: C6 só reconsulta falta dentro da janela; depois consulta esse lastro em `:1234`. Assim, há caminho de vitalidade/fechamento dependente do relógio mesmo com todos os chamadores passando por C7. Quantos registros de produção entram nele: **[não verificado]**.
5. O caso de silêncio em `app/chamados/tests/test_c7_red_minimos.py:289` usa pergunta muda criada pelo helper sem carimbo (`:115`); não cobre esses caminhos carimbados.

**Resultado AST específico:** reapliquei em memória os auxiliares de `app/chamados/tests/test_c7_red_minimos.py:452`–`:500` aos arquivos Python de `app`, excluindo testes/migrações/dependências: zero achados para `q_vivas`/lastro/`.viva` autorizando fechamento e zero por `disputa_so_de_falta`/`MOTIVOS_SO_DE_FALTA`. A abrangência é a do detector: `if`/`while` cujo corpo fecha (`:496`), inclusive aliases e derivações locais (`:485`–`:494`); não demonstra ausência de regras indiretas como a janela acima.

## 09/11 02:53 · sessao c85e4c4944fc · - · - · origem=Ronald (tmux)

Item 5 está implementado nos sítios nomeados pelo plano: E1 cobre seis sítios e E2 cobre os cinco mecânicos; as listas dos testes confirmam esse recorte. `app/chamados/tests/test_porta_chamado_e1.py:132`; `app/chamados/tests/test_porta_chamado_e2.py:80`.

| Sítio | Escrita atual | Porta |
|---|---|---|
| `app/chamados/services/cobranca.py:178` | `prazo_sla` do lote | `ChamadoColaborador.definir_prazo_sla`, persistência em `app/chamados/models.py:591` |
| `app/chamados/services/disputa_emissao.py:1642` | `prazo_sla` da recobrança | mesma porta, `app/chamados/models.py:573` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` | `escalonado_para_dp` e instante | `marcar_escalonado_dp`, persistência em `app/chamados/models.py:610` |
| `app/chamados/services/abertura.py:115` | campo real `ticket_id_glpi` | `apontar_ticket_glpi`, persistência em `app/chamados/services/vinculos.py:117` |
| `app/chamados/services/limbo_folgas.py:69` | `descricao` | `reescrever_texto`, persistência em `app/chamados/services/texto.py:36` |
| `app/api/views.py:3160` | `titulo` e `assunto` | mesma porta, `app/chamados/services/texto.py:21` |

São **seis sítios**, contendo cinco nomes de campo do aval; `prazo_sla` aparece em dois sítios e o sítio do título também escreve `assunto`. `app/chamados/tests/test_porta_chamado_e1.py:1`; `app/chamados/tests/test_porta_chamado_e1.py:132`; `app/api/views.py:3160`.

| Cinco mecânicos | Escrita atual |
|---|---|
| `app/ponto/management/commands/reconciliar_grade.py:454` | `retirar_contexto(ch, 'orfa_regeneracao')` |
| `app/ponto/management/commands/reconciliar_grade.py:473` | `renovar_contexto` seguido de `retirar_contexto` em atomic, na linha 474 |
| `app/ponto/management/commands/gerar_celulas.py:255` | `renovar_contexto` |
| `app/chamados/management/commands/nomear_marco_por_lampada.py:109` | `renovar_contexto`; evento somente quando a porta escreveu, linha 111 |
| `app/ponto/views.py:288` | `renovar_contexto`, fundindo as chaves informadas |

As persistências do contexto ficam em `app/chamados/services/contexto.py:104` e `app/chamados/services/contexto.py:127`; o nascimento permanece tratado separadamente pela porta de abertura. `app/chamados/services/contexto.py:7`.

O `delete` vizinho também migrou: `app/ponto/management/commands/zerar_dados_operacionais.py:84` chama `ChamadoColaborador.apagar_em_ensaio`; a porta exige banco de ensaio antes de apagar. `app/chamados/models.py:563`; `app/chamados/models.py:569`.

A matriz já declara:

- `teste='chamados.tests.test_chokepoint_chamado_gate.ChokepointChamadoGate'`. `app/core/contratos_estruturais.py:497`.
- `idempotencia='chamados.tests.test_porta_chamado_e3.IdempotenciaDaFamiliaChamado'`. `app/core/contratos_estruturais.py:500`.
- `verde=True`. `app/core/contratos_estruturais.py:501`.

A frase “QUATRO vizinhos fora” em `app/core/contratos_estruturais.py:1015` é relato histórico da C1 e está desatualizada perante as chamadas efetivas acima; o próprio comentário posterior do gate registra E1/E2 e teto zero. `app/chamados/tests/test_chokepoint_chamado_gate.py:152`; `app/chamados/tests/test_chokepoint_chamado_gate.py:161`; `app/chamados/tests/test_chokepoint_chamado_gate.py:313`.

Executei somente o varredor AST puro, com bytecode desabilitado: retornou **16 escritas atribuíveis de ChamadoColaborador, todas em módulos declarados como porta; zero fora**. Esse resultado tem o alcance do varredor, que exclui testes/migrations e mantém balde de não atribuíveis; não prova ausência de escritores que ele não reconheça. `app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:54`; `app/core/censo_escritas.py:469`; `app/core/portas.py:196`.

A leitura AST complementar das atribuições e `save/update` dos campos nomeados não encontrou escrita crua adicional de `ChamadoColaborador` fora dessas portas; os demais `descricao` encontrados pertencem a outras entidades. [não verificado por teste de execução] [nao verificado]

Os testes E1 possuem casos de idempotência para SLA, DP, GLPI e texto, e E2 possui caso de retirada ausente sem query; não executei testes nem consultei produção. `app/chamados/tests/test_porta_chamado_e1.py:57`; `app/chamados/tests/test_porta_chamado_e1.py:81`; `app/chamados/tests/test_porta_chamado_e1.py:94`; `app/chamados/tests/test_porta_chamado_e1.py:117`; `app/chamados/tests/test_porta_chamado_e2.py:45`.

## 09/11 02:52 · sessao 9e4f32c36ea4 · - · - · origem=Ronald (tmux)

- **A2 recebeu mordida por porta real:** `_agrupador_com_C_estrangeiro` chama o agrupador sem wrapper da helper nem cauda injetada, exige Disputa real, C estrangeiro antes da primeira D e ordem sem violações. O MORDE injeta exatamente a lambda local antes do `with` e exige falha por ausência do C estrangeiro. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:190`, `:199`, `:202`, `:204`, `:206`, `:211`) **Execução PostgreSQL: [nao verificado]**; não há Django/freezegun instalado neste ambiente **[nao verificado]**.

- **Os quatro mutantes da recusa têm oráculo que os rejeita:** identidade do retorno e mensagem nas telas, evento único depois do atomic e nenhuma D/P nova. Há MORDE para catch, evento, `None` e leitores das duas telas. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:126`, `:140`, `:164`, `:173`, `:179`, `:188`) Rodei os 23 testes sem banco de `trava` e `recusa`, com `python3 -B`: **23/23 OK**; resultado não persistido **[nao verificado]**.

- **S1: fixture corrigida sem afrouxar assert.** P materializada passa para `alvo`, preservando `ch2` sem P; ressuscitação usa `alvo`; permanecem assert de chegada à cauda e de D reaberta. Isso remove a causa identificada de vacuidade, mas passagem no Postgres da volta 2 segue **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:118`, `:120`, `:121`, `:127`, `:129`, `:79`; origem de `ch2` sem P: `app/chamados/tests/test_e1_fila.py:260`)

- **S2: B023 curado na origem**, capturando variáveis pelos defaults das closures; configuração continua selecionando B e não ignora B023. Ruff completo: **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_trava.py:254`, `:270`, `:367`; `app/ruff.toml:5`, `:6`)

- **S3: 1196 corresponde ao sítio real congelado**, `data_entrada = _tz.localtime(batida_origem.timestamp).date()`. O censo marca `.date()` de operando rastreado como B1; o selo compara todas as linhas registradas por igualdade. (`app/core/tests/test_medida_turno_do_par.py:268`, `:533`; `app/chamados/services/disputa_emissao.py:1196`; `app/core/medida_turno_do_par.py:217`, `:225`) Rodei esse teste isolado em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`: **OK**; resultado não persistido **[nao verificado]**.

- **O teste de savepoint não demonstra durabilidade diante do rollback externo.** Injeta recusa na helper antes das escritas do corpo; confirma marcador anterior ainda presente dentro de atomic externo e conjuntos D/P iguais. Não força rollback externo nem consulta o evento depois dele. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:238`, `:241`, `:262`, `:265`, `:266`; primeira escrita do corpo: `app/chamados/services/disputa_emissao.py:1081`) O nível zero exigido pelo teste sem banco refere-se apenas à transação simulada. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:68`, `:146`)

## 09/11 02:52 · sessao bb3c9cd21af8 · - · - · origem=Ronald (tmux)

- **Um produtor nominal do evento em produção:** `criar_disputa_a_partir_de_chamados_orfaos` captura `UniversoMudouSobATrava` depois de sair de seu atomic e chama `evento('repasse_recusado', 'Colaborador', colaborador.pk, ...)`; depois devolve o sentinel. Não encontrei segundo produtor nominal fora dessa porta. (`app/chamados/services/disputa_emissao.py:1290`, `app/chamados/services/disputa_emissao.py:1293`, `app/chamados/services/disputa_emissao.py:1296`)

- **Persistência não é garantida pelo retorno:** `core.observ.evento` grava `LogAuditoria` dentro de atomic próprio, sob `engolir`; falhas são consumidas e a função retorna `None`. O agrupador ignora esse retorno e ainda devolve `REPASSE_RECUSADO`. Portanto “sentinel ⇒ evento persistido” não é contrato efetivo. (`app/core/observ.py:85`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`, `app/chamados/services/disputa_emissao.py:1293`)

- **O evento participa da transação externa:** seu atomic não abre conexão independente nem agenda commit separado. Assim, sobrevive ao rollback do savepoint do repasse já encerrado; não sobrevive a rollback posterior do atomic do chamador. O teste verifica existência antes de sair de um atomic externo bem-sucedido, sem ensaio de rollback externo. Persistência real desta volta no Postgres: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1049`, `app/chamados/services/disputa_emissao.py:1290`, `app/core/observ.py:133`, `app/chamados/tests/test_e1_fila_v2_2_integracao.py:237`, `app/chamados/tests/test_e1_fila_v2_2_integracao.py:262`)

- **Leitores do evento:** leitura específica encontrada apenas na integração (`LogAuditoria.objects.get(acao='repasse_recusado', ...)`), com assertions de descrição/motivo. A tela genérica de auditoria pode ler e exibir o evento; aceita filtro de ação livre e mostra ação/modelo/objeto/descrição. O histórico de chamados do colaborador o exclui, pois filtra modelos `Chamado` e `ChamadoColaborador`, enquanto este evento usa `Colaborador`. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:262`, `app/chamados/tests/test_e1_fila_v2_2_integracao.py:264`, `app/core/views.py:705`, `app/core/views.py:710`, `app/core/views.py:733`, `app/templates/core/log_auditoria.html:68`, `app/templates/core/log_auditoria.html:71`, `app/templates/core/log_auditoria.html:73`, `app/chamados/services/historico.py:86`, `app/chamados/services/disputa_emissao.py:1293`)

- **Fecho sem segundo juiz/derivação paralela identificados:** o código novo lê relações C/D/Ch/P até ponto fixo; o fechamento continua delegado a `disputa.pode_fechar()`, autoridade registrada. Fecho/helper não consultam relógio; o limite de 128 provoca recusa declarada. (`app/chamados/services/disputa_emissao.py:957`, `app/chamados/services/disputa_emissao.py:959`, `app/chamados/services/disputa_emissao.py:964`, `app/chamados/services/disputa_emissao.py:974`, `app/chamados/services/disputa_emissao.py:985`, `app/chamados/services/disputa_emissao.py:1275`, `app/core/juizes.py:267`)

- **Sem segundo adquiridor de trava introduzido:** a helper passa conjuntos ordenados à autoridade existente; os `select_for_update` continuam em `PerguntaDisputa.trava_na_ordem`, na ordem C→D→Ch→P. Isso não elimina pré-travas de portas externas. (`app/chamados/services/disputa_emissao.py:1001`, `app/chamados/services/disputa_emissao.py:1007`, `app/chamados/models.py:2674`, `app/chamados/models.py:2683`, `app/chamados/models.py:2696`, `app/chamados/models.py:2700`, `app/chamados/models.py:290`)

- **Terminal reatável incluído:** o fecho semeia `VIVOS + ENCERRADOS`; o ramo HIST de `reatar_pergunta_orfa` também procura `ENCERRADOS`. `cancelado`/`superado` pertencem a `TERMINAIS`, mas não a esse universo HIST; sua exclusão da semente não demonstra quebra do aval “terminal reatável”. Pais/destinos explicitamente alcançados entram independentemente do status. (`app/chamados/services/disputa_emissao.py:960`, `app/chamados/services/disputa_emissao.py:973`, `app/chamados/services/disputa_emissao.py:980`, `app/chamados/reconciliador.py:1582`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`)

## 09/11 02:52 · sessao 0f44a4ea4bdd · - · - · origem=Ronald (tmux)

A1 está **parcialmente curado**: apenas dois dos seis chamadores distinguem a identidade de recusa; os quatro demais descartam o retorno. (`app/chamados/services/acoes_disputa.py:225`, `app/chamados/services/cobrar_dia.py:50`, `app/chamados/services/pergunta_no_app.py:186`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/processar_alertas_avancados.py:158`)

| Chamador | Leitura do retorno / efeito |
|---|---|
| `acoes_disputa` | Testa `is REPASSE_RECUSADO` antes do falso/`None` e do acesso `.pk`; retorna aviso falante. `app/chamados/services/acoes_disputa.py:225`, `:227`, `:229` |
| `cobrar_dia` | Testa identidade, devolve `ok=False` e mensagem explícita; view mapeia para aviso. `app/chamados/services/cobrar_dia.py:50`, `:51`; `app/chamados/views_cobrar.py:325`, `:326` |
| `pergunta_no_app.perguntar` | Ignora retorno e soma todos os chamados; comando imprime `APLICADO` mesmo que todos os repasses tenham recusado. **Contagem mentirosa concreta.** `app/chamados/services/pergunta_no_app.py:186`, `:187`, `:188`; `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29` |
| `detectar_ausencias` | Ignora retorno; catch genérico só se ativa para exceção, portanto não escreve o antigo erro stderr quando chega sentinel. `app/ponto/management/commands/detectar_ausencias.py:801`, `:802`, `:803` |
| `processar_alertas_turno` | Ignora retorno; devolve o **chamado**, não o sentinel, portanto não há confusão sentinel→D nessa porta. `app/ponto/management/commands/processar_alertas_turno.py:384`, `:385`, `:387` |
| `processar_alertas_avancados` | Ignora retorno; imprime “Chamado criado” e incrementa processados, rótulos relativos ao Ch efetivamente criado antes. Não demonstra que contou repasse como feito. `app/ponto/management/commands/processar_alertas_avancados.py:121`, `:141`, `:158`, `:161`, `:164` |

Não encontrei chamador de produção que use o sentinel como Disputa: os dois consumidores efetivos fazem identidade; os outros quatro o descartam. (`app/chamados/services/acoes_disputa.py:225`, `app/chamados/services/cobrar_dia.py:50`, `app/chamados/services/pergunta_no_app.py:186`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/processar_alertas_avancados.py:158`)

**Atomicidade:** o catch fica fora somente do atomic interno do repasse, não necessariamente da transação inteira; escrita anterior do chamador pode permanecer. (`app/chamados/services/disputa_emissao.py:1049`, `:1290`, `:1291`)

Existe cadeia por atomic externo: `_gravar_estado` abre atomic, trava e escreve Ch; o post-save chama cartório; cartório usa o emissor `detectar_ausencias`, que chega ao repasse. (`app/chamados/models.py:280`, `:290`, `:296`; `app/chamados/signals.py:187`, `:203`; `app/ponto/services/cartorio.py:1964`, `:1990`, `:1740`, `:1778`; `app/ponto/management/commands/detectar_ausencias.py:801`)

Nessa cadeia, o savepoint do repasse pode voltar e o ato externo continuar; a guarda da ordem não cobre as pré-travas já adquiridas antes da entrada, questão A3 que a triagem fornecida declarou fora desta obra. (`app/chamados/models.py:290`, `:296`; `app/chamados/services/disputa_emissao.py:1056`, `:1290`; `triagem_v1.txt:4` **[numeração do anexo nao verificada]**)

O evento **não tem persistência independente**: `core.observ.evento` insere `LogAuditoria` com atomic/savepoint na mesma conexão padrão; rollback do chamador o desfaz. Também é best-effort: pode devolver `None` após falha engolida, e o repasse devolve sentinel sem verificar a gravação. (`app/core/observ.py:103`, `:105`, `:133`, `:134`, `:139`; `app/chamados/services/disputa_emissao.py:1293`, `:1296`)

`cobrar_dia` já gravou toque e chamou julgamento antes do repasse; “nada feito” descreve somente o repasse desfeito, não todo o pedido. (`app/chamados/services/cobrar_dia.py:39`, `:41`, `:49`; `app/chamados/services/disputa_emissao.py:1021`, `:1294`)

**Censo retorno/evento:**

- Produtor da identidade: definição em `app/chamados/services/disputa_emissao.py:1020`; retorno de recusa em `:1296`; retorno `None` em `:1046` e `:1090`; retorno D em `:1285` e `:1288`.
- Exportação da função antiga por `models`: `app/chamados/models.py:4082`; os crons e ação continuam importando por esse alias. `app/chamados/services/acoes_disputa.py:213`; `app/ponto/management/commands/detectar_ausencias.py:792`; `app/ponto/management/commands/processar_alertas_turno.py:375`; `app/ponto/management/commands/processar_alertas_avancados.py:149`.
- Único produtor literal de produção localizado para ação `repasse_recusado`: `app/chamados/services/disputa_emissao.py:1293`; persistidor genérico único usado por ele: `app/core/observ.py:134`.
- Leitores de produção do retorno: ação e cobrança; descartadores: os quatro listados acima. (`app/chamados/services/acoes_disputa.py:225`; `app/chamados/services/cobrar_dia.py:50`; `app/chamados/services/pergunta_no_app.py:186`; `app/ponto/management/commands/detectar_ausencias.py:801`; `app/ponto/management/commands/processar_alertas_turno.py:384`; `app/ponto/management/commands/processar_alertas_avancados.py:158`)
- Leitores específicos do evento localizados são testes: conferem literal/descrição/motivo em memória e consultam `LogAuditoria`. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:143`, `:144`, `:145`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:262`, `:263`, `:264`)
- Consumidor operacional específico de `repasse_recusado`, retry e visibilidade em produção: **[nao verificado]**.

## 09/11 02:49 · sessao 07ae55071934 · E1-FILA-V2-2-TRAVA · construtor · origem=Code

Implementei A1 com **captura única fora do `atomic`**: o agrupador registra `repasse_recusado` e devolve `REPASSE_RECUSADO`, distinto de `None`. O savepoint permite preservar o ato externo; a verificação PostgreSQL dessa preservação está **[nao verificado]**. [disputa_emissao.py:1048](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:1048), [captura:1290](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:1290)

Quem escreve o evento/retorno é o agrupador, via `core.observ.evento`. Quem lê o retorno são `acao_iniciar_disputa_orfaos` e `cobrar_dia`, com mensagens próprias; o evento é consultado pelo RED SQL. Leitura operacional da trilha: **[nao verificado]**. [ação:225](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/acoes_disputa.py:225), [cobrar_dia:50](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/cobrar_dia.py:50), [RED SQL:262](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:262)

Também alterei:

- **S1:** `_caso_b` usa destino independente para a P materializada, preservando `ch2` sem P e o assert da cauda. [integração:118](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:118)
- **S2:** os três closures capturam as variáveis por defaults. [trava:254](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:254)
- **S3:** o registro acompanha a linha `1196`. [catraca:268](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:268)

Os REDs de A1 exercitam agrupador e telas; os mutantes retiram captura, evento, retorno distinguível e leitores. Passaram **23 testes sem banco**. A catraca passou **35 testes**; reaplicar `[1131]` em memória fez seu RED falhar. Os sete Python tocados passaram por `py_compile` em memória, sem gravar bytecode. [resultados e comandos:4](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/e1_fila_v2_2_verificacao.txt:4) [nao verificado]

**A2:** acrescentei agrupador real com P estrangeira e detector exigindo C estrangeiro antes da primeira D. O mutante insere a lambda antes do `with` e exige falha nesse assert. Consegui compilá-lo; **não consegui executar o caso PostgreSQL [nao verificado]**. [integração:190](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:190)

A relação completa `arquivo::funcao` e o comando de cada RED/mutante estão no [registro de entrega:137](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/e1_fila_v2_2_verificacao.txt:137). PostgreSQL A1/A2/S1, Ruff e suíte inteira permanecem **[nao verificado]**; certificação reservada ao Code e opus final. [nao verificado]

## 09/11 02:40 · sessao 26af97663c42 · - · - · origem=Ronald (tmux)

S2: capturar por defaults as variáveis dos laços:

- `crescer(*args, original=original, lidas=lidas, classe=classe, pk=pk)` nos dois pontos ([test_e1_fila_v2_2_trava.py:254](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:254), [mesmo arquivo:367](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:367)).
- `lambda c, o, m=m: _fecho_antigo(m, c, o)` ([mesmo arquivo:270](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:270)).

S3: atualizar o registro da linha B1 do agrupador após a cura A1; hoje `[1131]`, enquanto o censo atual calcula `[1188]` ([test_medida_turno_do_par.py:268](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:268)). A regra exige registro no mesmo diff e compara sem consultar Git ([mesmo arquivo:17](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:17), [mesmo arquivo:533](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:533)). Preserve a identidade da função: extrair o corpo para outra função exigiria também rever `REGISTRO` ([mesmo arquivo:526](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:526)).

Comandos executados, sem editar:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e1_fila_v2_2_trava
```

Resultado baseline: 15 testes OK [nao verificado por arquivo persistido]. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from core.medida_turno_do_par import censo
c = censo('.')
print([(i['chave'], i['linhas'])
       for i in c['derivacoes_de_dia']['itens']
       if 'disputa_emissao.py' in i['chave']])
PY
```

O ambiente tem `python3`, sem Django ou Ruff; execução desses verificadores reais fica [nao verificado]. O runner da catraca sem Django está documentado em [test_medida_turno_do_par.py:20](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:20).

## 09/11 02:40 · sessao 75dba09147d1 · - · - · origem=Ronald (tmux)

S1: a fixture ocupa `self.ch2` com P materializada ([integracao.py:118](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:118)), embora ele exista para atravessar o filtro como segundo órfão ([test_e1_fila.py:260](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila.py:260)). A regra pode excluí-lo por P materializada sem célula acusando ([disputa_emissao.py:759](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:759)); então o agrupador retorna antes da cauda ([disputa_emissao.py:1082](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:1082)). Cura sugerida: criar chamado independente como destino da P materializada e ressuscitá-lo na cauda, preservando `self.ch2` sem P; execução PostgreSQL [nao verificado].

A2: acrescentar teste que execute o agrupador real com D/P estrangeiras apontando o chamado local, sem wrapper da helper e sem cauda artificial; inspecionar `linhas_travadas` e exigir C estrangeiro por `for_update` antes da primeira D. O detector distingue aquisição e atualização ([test_e1_v9.py:71](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_v9.py:71)). O fecho encontra o estrangeiro por P reversa ao destino ([disputa_emissao.py:965](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:965)). [nao verificado]

MORDE sugerido: gerar função mutante usando `fila.mutante_ou_falha` ([test_e1_fila.py:48](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila.py:48)), inserir `_trava_do_repasse = lambda *a, **k: None` imediatamente antes do `with` e reutilizar o teste, exigindo falha no assert do C estrangeiro. Sem execução [nao verificado]. [nao verificado]

## 09/11 02:24 · sessao 69b9fc6d95cb · SELOS-CANON-I1 · verificador · origem=Code

**Parecer: o diff realiza parte do I-1, mas não sustenta aprovação integral.** Há escapes de abertura, a negativa não escreve o PAREI exigido e as medidas contam decisões intermediárias como abertura/ociosidade. (`bin/disjuntor.py:1332`, `bin/disjuntor.py:1521`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1607`, `bin/disjuntor.py:1777`)

As referências abaixo são da cópia. Identidade Git com o hash/base informados e resultados em produção: **[nao verificado]**. A execução integral dos selos também fica **[nao verificado]**; seus scripts criam arquivos e cópias. (`bin/tests/test_censo_antes_da_vaga.sh:21`, `bin/tests/test_censo_antes_da_vaga.sh:160`)

1. **O diff realiza o aval literal? Parcialmente.**

   Na tabela, “aval I-1” refere-se ao parágrafo fornecido no anexo `aval_selos_do_canon_0811.txt`; a numeração do arquivo original é **[nao verificado]**.

   | Exigência do aval I-1 | Realização ou falta |
   |---|---|
   | Interceptar abertura de construtor ESTRUTURA/fila 1 | Há chamadas no ramo Agent e no ramo Bash/Codex, condicionadas a reconhecedores textuais. A cobertura não alcança todas as chamadas aceitas pelo wrapper; detalhes na resposta 3. (`bin/disjuntor.py:1583`, `bin/disjuntor.py:1603`) |
   | Ler `logs/<obra>/brief*.md` | Lê os caminhos **citados na abertura**, com um nível de diretório sob `RAIZ/logs`. Não descobre automaticamente o brief da obra nem verifica que o diretório/conteúdo corresponde à obra declarada. (`bin/disjuntor.py:1413`, `bin/disjuntor.py:1475`, `bin/disjuntor.py:1513`) |
   | CENSO com leitores/escritores por AST e arquivo:linha | Verifica título, campos e existência de **alguma referência por regex em cada categoria**. Não verifica AST, existência dos sítios, completude nem referência em cada item. Isso não exige necessariamente executar AST na abertura, mas o check não comprova a procedência exigida. (`bin/disjuntor.py:1409`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`) |
   | DECISOES listadas | Exige algum conteúdo não vazio em `decisoes`; aceita também texto na própria linha, sem lista. (`bin/disjuntor.py:1452`, `bin/disjuntor.py:1463`) |
   | Linha literal `perguntas de lei: N` | Implementada, com recusa de repetição e conferência da quantidade de itens reconhecidos abaixo. (`bin/disjuntor.py:1408`, `bin/disjuntor.py:1445`, `bin/disjuntor.py:1465`) |
   | N>0: corte em cada pergunta | Percorre cada pergunta reconhecida e exige a substring `cortada por Ronald` seguida de token. Não verifica autenticidade do aval nem negação textual: `NAO cortada por Ronald X` satisfaz a regex. (`bin/disjuntor.py:1411`, `bin/disjuntor.py:1469`) |
   | Falha = rc 1; sessão só abre com rc 0 | A CLI `--censo` retorna 0/1. A abertura interceptada usa JSON `deny`, com retorno zero do hook; não há chamada obrigatória da CLI pelo wrapper. Há equivalência de bloqueio nas chamadas reconhecidas, mas não implementação literal do protocolo rc na abertura. (`bin/disjuntor.py:1532`, `bin/disjuntor.py:1654`, `bin/disjuntor.py:1905`, `bin/codex_pergunta.sh:394`) |
   | Linha PAREI no RELATO | **Falta escrita automática:** a negativa manda a sessão escrevê-la. A escrita automática existente produz outra linha, `(NEGOU CENSO)`, na seção de alarmes. (`bin/disjuntor.py:1521`, `bin/disjuntor.py:221`, `bin/disjuntor.py:236`) |
   | PAREI censo vale como ESPERANDO PROVADO | Implementado com prova adicional de wrapper vivo, obra e papel, compartilhando o caminho de espera do Stop e do orçamento. (`bin/hook_stop_fila1.py:703`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`) |
   | Duas medidas no ESTADO | Os campos chegam ao gerador, mas seus significados não comprovam abertura efetiva nem vaga ociosa; resposta 8. (`bin/disjuntor.py:1777`, `bin/disjuntor.py:1817`, `bin/gerar_estado.py:280`) |
   | Allowlist vazia; desligamento só Ronald | Não aparece allowlist específica no novo check. Continua existindo a chave global, com proteção contra sua manipulação pelo Code. Ausência de obra reconhecida ou BACKLOG, porém, libera sem constituir allowlist explícita. (`bin/disjuntor.py:151`, `bin/disjuntor.py:156`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1507`) |
   | Selo com mutante de linha ausente | Presente e com asserção que o atinge; cobertura adicional tem lacunas, resposta 7. (`bin/tests/test_censo_antes_da_vaga.sh:70`, `bin/tests/test_censo_antes_da_vaga.sh:175`) |

   Pouso próprio, código somente depois da E1 e cumprimento da ordem I-1/I-3/I-2: **[nao verificado]**. O registro disponível ainda descreve a condição “código só depois do pouso da E1”. (`app/docs/PROMPTS.md:1983`, `app/docs/BACKLOG.md:728`)

2. **Há outro juiz de “brief tem censo”?**

   Nos caminhos examinados, há **um reconhecedor do conteúdo**: `censo_do_brief`, chamado tanto por `_censo_nega` quanto por `--censo`. `_prova_censo` decide existência de produtor vivo; `gerar_estado` apresenta medidas; o handoff consulta fila/PAREI. Eles não repetem a validação do bloco. (`bin/disjuntor.py:1417`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`, `bin/hook_stop_fila1.py:649`, `bin/gerar_estado.py:280`, `bin/handoff_sessao.sh:120`)

   Não localizei autoridade anterior dessa pergunta no registro ou no lookup examinados. `JUIZES.md` declara que é consulta e aponta `app/core/juizes.py` como registro vivo; a existência de equivalente não identificado fica **[nao verificado]**. (`app/docs/JUIZES.md:7`, `app/core/juizes.py:170`, `app/core/juizes.py:706`, `app/core/juizes.py:1025`, `app/core/juizes.py:1173`, `app/core/juizes.py:1285`)

   Portanto, há cobertura incompleta da porta; não há evidência suficiente para acusar **segundo juiz que duplica juiz registrado**. (`bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`; autoridade anterior equivalente: **[nao verificado]**)

3. **Formas de abrir construtor e escapes.**

   | Forma demonstrável | Trava e classificação |
   |---|---|
   | Agent fora de `SO_LEITURA`, com caminho absoluto `wt-*` e obra reconhecida/andável | Passa pela trava. (`bin/disjuntor.py:98`, `bin/disjuntor.py:100`, `bin/disjuntor.py:1603`) |
   | Mesmo Agent, mas sem âncora reconhecível de obra — por exemplo, `Construa OBRA-E3...` | Obra pode virar `None`, que libera. **Escape real se continua sendo construtor estrutural da fila 1.** (`bin/disjuntor.py:111`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1507`) |
   | Agent com cópia sem nome `wt-*`, caminho relativo ou cópia não citada | Não entra no ramo. **Escape real quando o trabalho pertence à classe do aval**; ausência desse padrão textual não prova ausência de construção. (`bin/disjuntor.py:100`, `bin/disjuntor.py:1603`) |
   | Bash com atribuições imediatamente antes do wrapper: `CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh --construtor ...` | Passa pela trava. (`bin/disjuntor.py:1332`, `bin/disjuntor.py:1583`) |
   | `export CODEX_OBRA=X CODEX_PAPEL=construtor; bin/codex_pergunta.sh --construtor ...` | Hook lê papel `?` e obra vazia; wrapper aceita o ambiente herdado. **Escape real na própria via autorizada.** (`bin/disjuntor.py:1333`, `bin/disjuntor.py:1343`, `bin/disjuntor.py:1584`, `bin/codex_pergunta.sh:81`) |
   | `env CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh ...` | A regex admite prefixos, mas não essas atribuições depois de `env`. **Escape real.** (`bin/disjuntor.py:1250`, `bin/disjuntor.py:1338`, `bin/codex_pergunta.sh:81`) |
   | Wrapper dentro de `bash -c`, script externo ou encapsulamentos residuais | Limitação expressamente declarada; o wrapper não impõe CENSO na sua entrada. **Escape real se abre construtor abrangido.** (`bin/disjuntor.py:1247`, `bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:394`) |
   | Wrapper executado fora do PreToolUse | Não tem guarda própria de CENSO; toma vaga e executa o construtor. Uso concreto dessa via em produção: **[nao verificado]**. (`bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:370`, `bin/codex_pergunta.sh:394`) |
   | `codex exec` direto | Escape técnico declarado, mas via proibida pela regra do wrapper; não demonstra uma segunda abertura autorizada. (`bin/disjuntor.py:1248`, `CLAUDE.md:347`) |
   | `bin/fabricante.sh`, que inicia `claude -p` | A abertura da sessão não é Agent e não recebe essa trava. Enquadramento de cada fabricação como obra fila 1 do aval: **[nao verificado]**. (`bin/fabricante.sh:2`, `bin/fabricante.sh:150`, `bin/fabricante.sh:162`) |
   | Leitura/censo/verificador legítimos, obra fechada ou fila 2 | Fora da classe construtor estrutural fila 1. (`bin/disjuntor.py:1507`, `bin/tests/test_censo_antes_da_vaga.sh:123`, `bin/tests/test_censo_antes_da_vaga.sh:125`, `bin/tests/test_censo_antes_da_vaga.sh:127`, `bin/tests/test_censo_antes_da_vaga.sh:135`) |

   Outras formas não localizadas: **[nao verificado]**. A configuração fornecida chama um caminho absoluto da árvore viva; instalação deste diff naquele caminho também é **[nao verificado]**. (`.claude/settings.json:68`)

4. **Fail-open contradiz lei ou contrato?**

   **O contrato existente declara fail-open explicitamente:** “toda excecao vira linha FALHA na trilha e a ferramenta passa”. O tratamento externo preserva isso, com exceção expressa do B3. O novo CENSO herda a política e retorna `None` após `ABRIU-SEM`. (`bin/disjuntor.py:34`, `bin/disjuntor.py:1643`, `bin/disjuntor.py:1645`, `bin/disjuntor.py:1525`)

   Logo, não contradiz essa cláusula do disjuntor. Isso, porém, não demonstra autorização para excepcionar o aval específico de abertura somente com censo fechado; essa compatibilidade permanece **[nao verificado]**. (`bin/disjuntor.py:44`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1525`)

   LEI-AKITA 1 não permite concluir que qualquer tratamento de exceção seja automaticamente “fallback”: seu texto associa o termo à sobrevivência de derivador paralelo. Existe tensão mais direta com “exceção [...] nunca destravando a trava” e com a literalidade do aval. (`CLAUDE.md:17`, `CLAUDE.md:19`, `CLAUDE.md:32`, `CLAUDE.md:34`, `CLAUDE.md:37`)

   A trilha também não é garantida: `trilha` engole falhas de escrita. Assim, uma liberação excepcional pode ficar sem `ABRIU-SEM` observável. (`bin/disjuntor.py:142`, `bin/disjuntor.py:1526`)

5. **“Fila 1” é definição paralela?**

   **Nesta costura, não.** `_fila1_viva` chama diretamente `_itens_andaveis`, que aplica `_item_parado`; o hook usa esse mesmo predicado para decidir quais linhas andam. (`bin/disjuntor.py:1489`, `bin/disjuntor.py:1498`, `bin/hook_stop_fila1.py:158`, `bin/hook_stop_fila1.py:212`, `bin/hook_stop_fila1.py:218`)

   “Todos os andáveis” e “item em curso” são perguntas diferentes: o conjunto vem de OBRAS; o item em curso vem de `ORDEM-VIVA-TOPO`. Não selecionar apenas o item do marcador não constitui divergência de fila. (`bin/hook_stop_fila1.py:179`, `bin/hook_stop_fila1.py:218`, `bin/hook_stop_fila1.py:243`, `bin/hook_stop_fila1.py:1292`)

   B3 decide parada/liberação por reprovações e triagens da obra; não é autoridade de pertencimento à fila 1. (`bin/disjuntor.py:903`, `bin/disjuntor.py:909`, `bin/disjuntor.py:993`)

   Há outro defeito: BACKLOG ausente vira conjunto vazio, liberando sem `ABRIU-SEM`. É falha de fonte interpretada como “fora do universo”, não derivação paralela. (`bin/disjuntor.py:1493`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1507`)

6. **A prova de ESPERANDO é a mesma? Quais mutantes passam?**

   A vivacidade usa a autoridade existente `_prova_pid`: identidade de executável/script, ancestralidade, estabilidade do processo e rejeição de Z/X. A nova função a chama, em vez de copiar esses testes. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:529`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:546`, `bin/hook_stop_fila1.py:682`)

   O predicado completo é diferente: CENSO descobre o wrapper por `/proc` e exige raiz, UID, obra exata e papel censo/leitura; ESPERANDO genérico recebe produtor e PID/arquivo declarados. É uma composição específica nova, não uma segunda implementação da vivacidade. Ambos compartilham decisão, memoização, orçamento e liberação final. (`bin/hook_stop_fila1.py:661`, `bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:682`, `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`)

   Os exemplos “aceitar construtor”, “casar prefixo” e “aceitar ausência após morte” têm casos negativos específicos. Não cabe declará-los sobreviventes por simples leitura. (`bin/tests/test_hook_esperando_com_prova.sh:206`, `bin/tests/test_hook_esperando_com_prova.sh:209`, `bin/tests/test_hook_esperando_com_prova.sh:214`)

   Remover a guarda de UID ou a comparação adicional de starttime são candidatos sem caso dedicado no bloco novo; sobrevivência executada desses mutantes: **[nao verificado]**. O teste de morte faz `wait`, não simula reciclagem do PID. (`bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:685`, `bin/tests/test_hook_esperando_com_prova.sh:31`, `bin/tests/test_hook_esperando_com_prova.sh:194`)

7. **Os quatro mutantes mordem? Há sobreviventes?**

   **Os quatro têm mordida nas asserções:**

   | Mutante | Caso que o atinge |
   |---|---|
   | Linha ausente vale zero | Exige rc 1 sem a linha. (`bin/tests/test_censo_antes_da_vaga.sh:70`, `bin/tests/test_censo_antes_da_vaga.sh:175`) |
   | Não exigir corte | Casos retiram o corte da última pergunta. (`bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:176`) |
   | Agent sem porta | Negativos exigem `deny`, motivo CENSO e PAREI específico. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:112`, `bin/tests/test_censo_antes_da_vaga.sh:177`) |
   | Codex sem porta | A mesma exigência no construtor sem brief. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:131`, `bin/tests/test_censo_antes_da_vaga.sh:178`) |

   A harness verifica âncora única e exige que a função de casos falhe após a mutação. Resultado da rodada integral: **[nao verificado]**. (`bin/tests/test_censo_antes_da_vaga.sh:165`, `bin/tests/test_censo_antes_da_vaga.sh:170`, `bin/tests/test_censo_antes_da_vaga.sh:173`)

   **Sobrevivente concreto por análise e reprodução em memória:** trocar `for q in perguntas:` por `for q in perguntas[-1:]:`. Todos os negativos de corte alteram a última pergunta; a primeira poderia ficar sem corte. A costura não acrescenta caso com N>0. (`bin/disjuntor.py:1469`, `bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:99`, `bin/tests/test_censo_antes_da_vaga.sh:100`)

   Outro candidato em `_censo_nega` é retirar a trilha da exceção: não há cenário que provoque essa exceção e confira `ABRIU-SEM`. A medida só exige rótulos e número decimal, sem duração ou contagem esperada. (`bin/disjuntor.py:1525`, `bin/tests/test_censo_antes_da_vaga.sh:141`, `bin/tests/test_censo_antes_da_vaga.sh:147`)

   A asserção positiva também é insuficiente: `_passa_censo` aceita qualquer saída sem “CENSO”, inclusive negativa B4 ou saída vazia; o status de `_pre` não é conferido. (`bin/tests/test_censo_antes_da_vaga.sh:97`, `bin/tests/test_censo_antes_da_vaga.sh:106`)

8. **Quem escreve, quem lê e onde muda o significado?**

   | Objeto | Escritor | Leitor |
   |---|---|---|
   | Brief: CENSO, leitores, escritores, decisões e perguntas | Escritor real/procedência AST: **[nao verificado]** | `censo_do_brief`. (`bin/disjuntor.py:1420`, `bin/disjuntor.py:1438`) |
   | BACKLOG/OBRAS usado no universo | Escritor real desta evidência: **[nao verificado]** | `_fila1_viva` → `_itens_andaveis`. (`bin/disjuntor.py:1494`, `bin/disjuntor.py:1498`) |
   | Ambiente `CODEX_OBRA`/`CODEX_PAPEL` do produtor | Fornecido pelo lançador; o wrapper o recebe. (`bin/codex_pergunta.sh:81`) | `_prova_censo` lê `/proc/<pid>/environ`. (`bin/hook_stop_fila1.py:678`, `bin/hook_stop_fila1.py:682`) |
   | `disjuntor.log` | Porta `trilha`, com carimbo e detalhe limitado a 300 caracteres. (`bin/disjuntor.py:135`, `bin/disjuntor.py:140`) | Leitor geral e `_censo_24h`; hook o proíbe como arquivo de prova de progresso próprio. (`bin/disjuntor.py:1717`, `bin/disjuntor.py:1784`, `bin/hook_stop_fila1.py:585`) |
   | `CENSO OK`, com obra/via/brief | `_censo_nega`. (`bin/disjuntor.py:1518`) | Conta `ok` e encerra intervalo da obra. (`bin/disjuntor.py:1797`) |
   | `CENSO ABRIU-SEM`, com obra/via/falha | Exceção de `_censo_nega`. (`bin/disjuntor.py:1525`) | Conta `abertos_sem`. (`bin/disjuntor.py:1801`) |
   | `NEGOU DISJUNTOR CENSO...` | `main_pretool`, antes do JSON deny. (`bin/disjuntor.py:1636`, `bin/disjuntor.py:1639`) | Inicia intervalo e conta `negados`. (`bin/disjuntor.py:1794`) |
   | `abertos_sem`, `negados`, `ok`, `ociosa_h`, `esperando` | `_censo_24h`, em memória. (`bin/disjuntor.py:1807`) | `_censo_txt` → `linha` → `gerar_estado`. (`bin/disjuntor.py:1812`, `bin/disjuntor.py:1895`, `bin/gerar_estado.py:280`) |
   | ESTADO publicado | Gerador grava destino; `relato.sh` entrega o temporário à raspagem. (`bin/gerar_estado.py:390`, `bin/relato.sh:38`, `bin/relato.sh:105`) | Consumo posterior efetivo: **[nao verificado]** |
   | PAREI censo no topo | O diff instrui a sessão; escritor concreto: **[nao verificado]**. (`bin/disjuntor.py:1521`, `bin/disjuntor.py:1908`) | Stop, handoff e alarme, com sentidos divergentes abaixo. (`bin/hook_stop_fila1.py:703`, `bin/handoff_sessao.sh:108`, `bin/alarme_sessao_ociosa.py:127`) |
   | Linha automática `(NEGOU CENSO)` no RELATO | `relato_negacao` pela porta `esteira_vigia.no_relato`, preservando o topo. (`bin/disjuntor.py:221`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:829`) | Não substitui a linha literal consumida pelo reconhecedor de espera. (`bin/hook_stop_fila1.py:705`) |

   **ESMERIL-SEMANTICA presente nas medidas:** `OK` e `ABRIU-SEM` acontecem antes de B4/`codex_decide` e das validações do wrapper. Provam resultado do check, não abertura efetiva. (`bin/disjuntor.py:1518`, `bin/disjuntor.py:1526`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1607`, `bin/codex_pergunta.sh:387`)

   `ociosa_h` soma intervalos por obra entre NEGOU e OK/agora; não consulta vaga. Pode somar esperas simultâneas e encerrar em OK seguido de outra negativa. Correspondência com “horas de vaga ociosa”: **[nao verificado]**. Trilha ausente vira zero, embora falha de escrita seja silenciada. (`bin/disjuntor.py:1795`, `bin/disjuntor.py:1800`, `bin/disjuntor.py:1803`, `bin/disjuntor.py:1807`, `bin/disjuntor.py:142`)

   **Divergência no PAREI:** com produtor vivo, Stop libera por espera provada; handoff consulta somente PAREI genérico, que exige `!`, e pode informar “não devolve”. Com produtor morto, o alarme ainda desarma pela substring PAREI, enquanto Stop acusa falta de prova. (`bin/hook_stop_fila1.py:1171`, `bin/hook_stop_fila1.py:1283`, `bin/handoff_sessao.sh:120`, `bin/handoff_sessao.sh:123`, `bin/alarme_sessao_ociosa.py:42`, `bin/alarme_sessao_ociosa.py:127`)

9. **Classes de reprovação presentes.**

   | Classe | Conclusão |
   |---|---|
   | Segundo juiz duplicando registrado | **Não demonstrado** para conteúdo do brief; as duas entradas chamam a mesma função. (`bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`) |
   | Segundo escritor fora da porta | **Não demonstrado** nas novas escritas: trilha e linha automática reutilizam portas existentes. O PAREI obrigatório, contudo, não é escrito pelo diff. (`bin/disjuntor.py:1518`, `bin/disjuntor.py:1639`, `bin/disjuntor.py:236`, `bin/disjuntor.py:1521`) |
   | Relógio novo fora da borda | **Não demonstrado**: `_censo_24h` recebe `agora`; o carimbo próprio pertence ao escritor já existente. (`bin/disjuntor.py:140`, `bin/disjuntor.py:1774`, `bin/disjuntor.py:1903`) |
   | Derivação paralela da fila | **Não**, nesta costura: chama o juiz do hook. (`bin/disjuntor.py:1498`) |
   | Selo/RED sem mordida | Os quatro declarados têm mordida; há sobrevivente de “cada pergunta”, positivos fracos e medida sem valor esperado. RED integral evidenciado: **[n

[truncado em 20000 de 20824 caracteres]

## 09/11 02:23 · sessao 4474b71ff4e2 · DEPLOY-EXCLUSAO-ENVELOPE · leitura · origem=Code

O código confirma a janela entre consultar a marca e reiniciar; ele próprio a nomeia como residual (ii). **A exclusão proposta ainda não está implementada no envelope desta cópia**, que executa o comando diretamente (`bin/deploy.sh:307-314`; `bin/cron_run.sh:9-13`). Estado efetivo de produção, crontab instalado e timers ativos: **[nao verificado]**.

**1. Quem lê e escreve `logs/crons_em_curso/`**

| Componente | Comportamento comprovado |
|---|---|
| `cron_run.sh` | Cria uma marca por corrida, com PID na extensão; remove no `EXIT`. A marca cobre comando, gravação do placar, carimbo de fim e tentativa de aviso ao vigia (`bin/cron_run.sh:9-37`). |
| `deploy.sh` | Lê as marcas, verifica `/proc/PID`, apaga as consideradas órfãs e recusa `--restart`. Consulta antes dos demais portões e novamente imediatamente antes do restart (`bin/deploy.sh:108-117`; `bin/deploy.sh:338-342`). |
| `contrato_deploy.sh` | Inspeciona o **texto dos scripts**, exigindo referência à marca; não consulta o diretório operacional (`bin/contrato_deploy.sh:79-82`). |
| Testes | Criam marcas em ambientes falsos e exigem recusa baseada nelas (`bin/tests/test_deploy_reload_gracioso.sh:91`; `bin/tests/test_deploy_reload_gracioso.sh:125-131`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`). |

Não localizei outro acesso operacional ao diretório em hook, vigias ou `gerar_estado`; acessos indiretos ou externos à cópia: **[nao verificado]**. O hook possui uma guarda diferente, baseada em `logs/sombra.lock` (`bin/hook_stop_fila1.py:785-794`).

Há um consumidor escrito do **significado** da marca: esperar o término da sombra pela ausência de `sombra.sh_-.*` (`CLAUDE.md:92`; `app/docs/INFRA.md:59-60`). O contrato não determina quando criar a marca em relação à aquisição do lock: se criada antes, também representará espera; se depois, não mostrará quem aguarda. Preservação desse significado na implementação: **[nao verificado]**.

Há ainda registro histórico de um gate que escrevia marca própria durante um apply, fora do envelope. O relato cita `logs/gate10/gate10_condicao1.sh`; presença e uso atual desse escritor, assim como sua futura adesão ao lock: **[nao verificado]** (`app/docs/RELATO.md:4472-4489`).

**2. Reiniciar ou recriar cascas: cobertura e escapes**

| Via | Resultado da leitura |
|---|---|
| `deploy.sh --restart "motivo"` | É o ramo que o contrato propõe proteger. Hoje exige motivo e chama `docker restart` das três cascas após consultar marcas (`bin/deploy.sh:72-75`; `bin/deploy.sh:338-342`). Implementação do exclusivo: **[nao verificado]**. |
| Normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` | Usam HUP; não têm fallback para restart. O código explica que o HUP vai ao master e preserva o comando executado por `docker exec` (`bin/deploy.sh:322-359`). |
| Reload das 03:30 | Está declarado como `--reload-agendado`, passa pelo envelope e chega ao mesmo HUP. Teria o compartilhado proposto, sem adquirir o exclusivo do ramo `--restart` (`app/config/crons.py:326-327`; `app/config/crons.py:1861-1863`; `bin/deploy.sh:346-359`). |
| `docker compose up -d`, rebuild/recreate | São explicitamente atos de infra externos ao deploy; escapam do exclusivo descrito (`bin/deploy.sh:20-22`; `bin/deploy.sh:66-69`). O README também contém uma sequência direta de remoção/recriação de `saas_web`; uso atual: **[nao verificado]** (`README.md:35-38`). |
| Restart automático | Mensageria declara `restart: unless-stopped`, sem participação no lock proposto. Política efetivamente instalada: **[nao verificado]** (`docker-compose.mensageria.yml:9`). |
| Outros caminhos examinados | Os moldes chamam `--reload-copia`; o vigia de login chama `--sem-migrate`, portanto chegam ao HUP. `recursos.sh` aplica `docker update` (`bin/molde_fatia/cadeia.sh:32`; `bin/molde_fatia/cadeia.sh:48`; `bin/vigia_logins.sh:87`; `bin/recursos.sh:140-145`). |

Reinício manual externo, host, daemon, OOM e configuração efetiva de core/ui: **[nao verificado]**. O compose principal é declarado específico do ambiente e ignorado (`.gitignore:40-41`).

Existe também uma ambiguidade no término do exclusivo: o contrato fala em “prova de casca depois do restart”, mas hoje `prova_de_casca.sh` roda **antes**; depois do restart há espera das cascas e provas de rota (`bin/deploy.sh:292-298`; `bin/deploy.sh:380-418`).

**3. Disparos fora de `cron_run.sh`**

**`RAW` não significa sem envelope:** todas as entradas de `CRONS`, inclusive `RAW`, passam por `_run`; cada elo executado também entra em `cron_run.sh` (`app/config/crons.py:1483-1486`; `app/config/crons.py:1860-1874`; `bin/encadeado.sh:102-104`).

Existem, porém, caminhos externos:

- **Despachante da corrente:** nasce depois que o envelope da raiz terminou e consulta `saas_core` para obter os elos antes de entrar em qualquer envelope. Essa consulta escapa do compartilhado; se falhar, registra ALARME e sai 2 sem passar pelo vigia do envelope (`app/config/crons.py:1884-1885`; `bin/encadeado.sh:40-52`).
- **Cron avulso em `/etc/cron.d`:** `deploy_agendado.sh` gera chamada direta a si próprio e executa as esteiras diretamente. Não ganha compartilhado automaticamente (`bin/deploy_agendado.sh:54-57`; `bin/deploy_agendado.sh:63-73`).
- **Timers:** esteira, fabricante, integrador, relato e teto dia/noite têm `ExecStart` direto, sem envelope (`bin/hasner-esteira.service:15`; `bin/hasner-fabricante.service:14`; `bin/hasner-integrador.service:13`; `bin/hasner-relato.service:15`; `bin/hasner-teto-dia.service:7`; `bin/hasner-teto-noite.service:7`).
- **Exemplo concreto de timer usando casca:** relato chama `relato_nomes.sh`, que faz `docker exec saas_core` diretamente (`bin/relato.sh:32`; `bin/relato_nomes.sh:17-21`).
- **Chamadas manuais:** `lavrar_deslogados.sh` mantém uma porta manual com `docker exec` direto (`bin/lavrar_deslogados.sh:11-18`).

Crontab cru, cron.d e timers efetivamente instalados em produção: **[nao verificado]**. Portanto, “todo cron” não está demonstrado como cobertura global; o código demonstra cobertura dos comandos envelopados e dos elos que chegam ao envelope (`app/config/crons.py:1860-1885`; `bin/encadeado.sh:102-104`).

**4. Crons longos e recusa prolongada**

Não localizei `placar.jsonl` na cópia; durações típicas atuais: **[nao verificado]**. Existe um snapshot datado de **31/10**, calculado como **p95 × 1,5, piso 30 s**, preservando medidas antigas quando faltam novas; os números abaixo não são duração típica bruta nem limite de execução (`app/config/crons_duracao.json:106-110`; `bin/crons_duracao.py:69-95`).

| Cron | Medida com folga | Fonte |
|---|---:|---|
| Sombra | 5.188 s — 86min28s | `app/config/crons_duracao.json:78` |
| Noturno | 1.800 s — 30min | `app/config/crons_duracao.json:56` |
| `lavrar_he_pendente` | 1.274 s — 21min14s | `app/config/crons_duracao.json:43` |
| Cartório | 760 s — 12min40s | `app/config/crons_duracao.json:63` |
| Auditoria de invariantes | 420 s — 7min | `app/config/crons_duracao.json:11` |
| Esmeril / scores | 340 s / 310 s | `app/config/crons_duracao.json:28`; `app/config/crons_duracao.json:14` |

A sombra é um único envelope para `--refazer && --bloco`. A corrente não mantém um envelope contínuo: cada elo espera carimbos antes de entrar no seu próprio envelope (`app/config/crons.py:350-352`; `app/config/crons.py:1884-1885`; `bin/encadeado.sh:84-104`).

O backup percorre schemas e faz dumps sem timeout declarado nesses scripts; seu valor de 30 s coincide com o piso e não prova rapidez nem duração de horas (`scripts/backup_all.sh:5-8`; `scripts/backup_tenant.sh:10-12`; `app/config/crons_duracao.json:12`).

O `vigia_arvore` pode permanecer bastante tempo: espera pelo retrato, pode aguardar até 3.600 s pela trava da suíte e executa suítes/repetições em primeiro plano. Horas são possíveis pelo fluxo; frequência e duração típica: **[nao verificado]** (`bin/vigia_arvore.sh:38-43`; `bin/vigia_arvore.sh:83-106`; `bin/trava_teste.sh:22-38`).

**Isso já recusa restart hoje:** a marca permanece durante esses comandos e o deploy recusa enquanto encontra seu PID vivo. Mantido o escopo, a duração longa não é uma restrição nova do compartilhado; a mudança fecha a janela entre consulta e restart. “Sempre recusado” não é demonstrável sem dados de execução (`bin/cron_run.sh:9-11`; `bin/deploy.sh:108-113`; `bin/deploy.sh:341-342`).

**5. Esperar 600 s, sair 75, corrente e TI**

**Rc 75 é falha para o envelope atual**, se chegar ao seu epílogo: grava `exit:75`, não escreve novo carimbo de fim, tenta avisar `cron_quebrado` e termina 75. Só 0 e 2 são aceitos como conclusão (`bin/cron_run.sh:11-38`). O serviço destina a pauta à TI (`app/core/services/crons_quebrados.py:57-73`).

Há quatro ressalvas:

- Um **`exit 75` antecipado** na aquisição do futuro lock pularia placar e aviso. A implementação precisa encaminhar essa falha ao epílogo; isso ainda está **[nao verificado]** (`bin/cron_run.sh:11-35`).
- A pauta é uma **tentativa** via `docker exec`, com saída ocultada e flag criada somente após sucesso. Pauta efetivamente criada: **[nao verificado]** (`bin/cron_run.sh:30-35`).
- A corrente é chamada mesmo se a raiz falhar, por causa do `;`. Sem carimbo, o dependente espera, alarma e sai 2 **antes do envelope**; esse timeout não gera automaticamente pauta daquele elo. O percurso continua nos elos seguintes (`app/config/crons.py:1884-1885`; `bin/encadeado.sh:64-68`; `bin/encadeado.sh:84-104`).
- Os carimbos são por dia. Um sucesso anterior no mesmo dia não é apagado por rc 75 posterior e pode satisfazer o dependente (`bin/cron_run.sh:22-25`; `bin/encadeado.sh:81-89`).

O contador atual considera a última corrida em **26 h**; não substitui a nova contagem contratual de ocorrências de rc 75 por lock em **24 h** (`app/core/services/crons_quebrados.py:17-47`).

Também falta definir `dur` versus `espera_lock`: conservar o relógio inicial antes da espera faria essa espera entrar no p95, pois o medidor não desconta `espera_lock` nem filtra falhas (`bin/cron_run.sh:7-13`; `bin/crons_duracao.py:69-76`).

**6. Confronto com a lei escrita**

- **“Recusa, não espera”: não há contradição demonstrada.** A regra é dirigida ao deploy `--restart`; o contrato faz o cron esperar e mantém o deploy não bloqueante. O mecanismo proibido é esperar e depois reiniciar por cima (`bin/deploy.sh:103-117`; `bin/contrato_deploy.sh:50-51`).
- **Seção 2:** permanece compatível se conservar HUP automático, restart manual e os demais portões; essa preservação futura está **[nao verificado]** (`CLAUDE.md:90`).
- **Seção 7 / lap-67:** o lock não exige mudar a fonte declarativa dos crons; afirmar equivalência com o instalado continua **[nao verificado]** (`CLAUDE.md:236-239`; `app/docs/LAPIDES.md:736-741`).
- **Lap-68:** a corrente deve continuar dependendo dos arquivos de fim. Rc 75 sem novo carimbo respeita essa condição, com a ressalva dos carimbos anteriores do mesmo dia (`CLAUDE.md:240-242`; `app/docs/LAPIDES.md:745-751`; `bin/encadeado.sh:81-94`).
- **GATE TEMPORAL:** esperar no processo do cron por exclusão em arquivo não demonstra retorno de `pgrep` nem de esperador de sessão do Code (`CLAUDE.md:204`; implementação proposta **[nao verificado]**).
- **Rc 75 “não é vermelho”:** essa exceção está no capítulo da suíte; o envelope dos crons aceita somente 0/2. Generalizá-la aos crons contradiz o tratamento atual (`CLAUDE.md:95-104`; `bin/cron_run.sh:27-35`).

Os selos atuais precisam acompanhar a troca de autoridade: hoje exigem referência à marca e simulam cron vivo criando apenas arquivo, sem adquirir o futuro compartilhado (`bin/contrato_deploy.sh:79-82`; `bin/tests/test_deploy_reload_gracioso.sh:125-131`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`).

Por fim, o contrato não remove expressamente a recusa residual de `--restart` com migration pendente no core. Essa recusa continua escrita e ocorre antes da aquisição final proposta; sua retirada ou alteração não está comprovada (`bin/deploy.sh:219-228`).

## 09/11 02:22 · sessao edca7285fe1d · - · - · origem=Ronald (tmux)

**Q4 — crons longos e efeito no restart**

Não há duração atual de produção comprovada pelo anexo; `placar.jsonl` de produção está [nao verificado]. A cópia contém um snapshot declarado como gerado em **31/10**, janela de sete dias, `p95 × 1,5`, piso de 30 segundos — não é duração típica bruta nem teto de execução (`app/config/crons_duracao.json:106-110`; `bin/crons_duracao.py:69-76`). Medições antigas são preservadas quando faltam novas (`bin/crons_duracao.py:83-95`).

| Cron | Medida versionada com folga | Fonte |
|---|---:|---|
| `sombra.sh` | 5188 s ≈ 86 min | `app/config/crons_duracao.json:78` |
| `noturno.py` | 1800 s = 30 min | `app/config/crons_duracao.json:56` |
| `lavrar_he_pendente` | 1274 s ≈ 21 min | `app/config/crons_duracao.json:43` |
| `processar_cartorio` | 760 s ≈ 13 min | `app/config/crons_duracao.json:63` |
| `auditar_invariantes_chamados` | 420 s = 7 min | `app/config/crons_duracao.json:11` |
| `esmeril_espelho` | 340 s | `app/config/crons_duracao.json:28` |
| `calcular_scores` | 310 s | `app/config/crons_duracao.json:14` |
| `backup_all.sh` | 30 s, sujeito ao piso | `app/config/crons_duracao.json:12,110` |

O backup percorre os schemas e executa um dump por schema; não há timeout declarado nesses scripts. Isso permite demora, mas não prova backup de horas (`scripts/backup_all.sh:5-8`; `scripts/backup_tenant.sh:10-12`).

A sombra é **um único envelope** para `--refazer && --bloco`; portanto, o compartilhado cobriria ambos e suas esperas internas (`app/config/crons.py:350-352,1861-1863`). A trava própria da sombra pode esperar 1800 s antes do trabalho (`bin/sombra_trava.sh:18,26-28`).

A corrente **não é um envelope contínuo**: o render coloca `; encadeado.sh --corrente ...` depois do `cron_run` da raiz (`app/config/crons.py:1873-1885`). A espera dos carimbos acontece antes de entrar no envelope de cada elo; o compartilhado seria adquirido apenas ao executar cada elo (`bin/encadeado.sh:84-104`). Portanto, somar todos os elos não prova um compartilhado contínuo de horas.

Há um caso adicional: `vigia_arvore`. O fallback declarado de 3300 s é **sobrescrito**, não combinado por máximo, pelo JSON que registra 30 s (`app/config/crons.py:139,159-160`; `app/config/crons_duracao.json:89`). O código pode esperar 600 s pelo retrato, esperar até 3600 s pela trava da suíte e executar suítes em primeiro plano, incluindo repetições; não há timeout global para a suíte (`bin/vigia_arvore.sh:38-43,83-88,106,131-135`; `bin/trava_teste.sh:22,35-38`). Horas são possíveis pelo código; duração típica de horas e restart “sempre recusado” permanecem [nao verificado].

**Esses tempos já bloqueiam o restart por marca:** ela nasce antes do comando e fica até a saída; o deploy recusa qualquer marca cujo PID exista (`bin/cron_run.sh:9-11`; `bin/deploy.sh:108-113`). Mantido o mesmo escopo, o compartilhado não acrescenta a duração do cron à recusa; acrescenta exclusão atômica na janela que hoje existe entre verificar e reiniciar (`bin/deploy.sh:341-342`).

**Q5 — rc 75, corrente e TI**

Se **o comando devolve 75 ao epílogo atual**, o envelope grava `exit:75`, não escreve `logs/fim/`, tenta chamar `cron_quebrado` e sai 75 (`bin/cron_run.sh:11-13,22-25,29-38`). O serviço classifica qualquer saída diferente de 0 e 2 como quebrado e escreve pauta destinada a TI (`app/core/services/crons_quebrados.py:16,45-47,57-73`).

Mas o contrato ainda precisa determinar o caminho de falha da aquisição: um **`exit 75` antecipado**, antes desse epílogo, não grava placar nem chama vigia. A aquisição proposta não está implementada em `cron_run.sh`; o fluxo atual executa diretamente o comando (`bin/cron_run.sh:7-13`). Garantia futura de passar RC=75 pelo epílogo: [nao verificado].

A pauta não é garantida pelo retorno 75 sozinho: o envelope usa `docker exec`, oculta sua saída e cria a flag apenas se a chamada der certo; com flag já existente, não repete a chamada (`bin/cron_run.sh:30-35`). Pauta efetivamente criada sem acesso ao banco/log: [nao verificado].

A raiz usa `;`, portanto a corrente é chamada mesmo depois de rc75 (`app/config/crons.py:1884-1885`). Sem carimbo do gatilho, cada dependente espera, escreve ALARME e sai 2 sem executar seu comando (`bin/encadeado.sh:84-97`). Essa expiração acontece **fora do cron_run**, logo não gera automaticamente placar/vigia daquele elo; o percorrimento continua nos elos seguintes (`bin/encadeado.sh:64-68,102-104`).

Há uma ressalva: carimbos são por **dia**, não por tentativa. O envelope não remove carimbo de sucesso anterior; o encadeado consulta apenas sua existência. Uma repetição com rc75 pode deixar disponível um carimbo bem-sucedido anterior do mesmo dia e permitir o dependente (`bin/cron_run.sh:22-25`; `bin/encadeado.sh:81-89`). Isso já é propriedade do protocolo atual.

O contador atual de quebrados usa a corrida mais recente nas últimas **26 horas**, não o contador contratual de rc75 por lock em 24 h (`app/core/services/crons_quebrados.py:17,24-47`).

**Ressalva de medição:** se o futuro envelope conservar `T0` antes da espera e `dur=T1-T0`, a espera entrará na duração usada pelo medidor; ele não filtra por `exit` nem desconta `espera_lock`. Assim, até timeouts rc75 podem inflar o p95 e alterar horários derivados (`bin/cron_run.sh:7,13`; `bin/crons_duracao.py:69-76`). A definição futura de `dur` versus `espera_lock` está [nao verificado].

## 09/11 02:22 · sessao 00b70fe1c898 · - · - · origem=Ronald (tmux)

**Q2 — Cobertura dos reinícios**

| Via | O que a cópia implementa | Cobertura pelo contrato proposto |
|---|---|---|
| `deploy.sh --restart "motivo"` | Modo manual, exige motivo; verifica marcas antes dos portões e novamente imediatamente antes de `docker restart saas_core saas_ui mensageria`. A própria cópia documenta a janela entre amostragem e restart. (`bin/deploy.sh:72-75,108-117,310-314,338-342`) | É a via nomeada pelo contrato fornecido; sua implementação com flock ainda é [nao verificado]. |
| Deploy normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` | Convergem para HUP das três cascas, com troca dos workers; falha do HUP não provoca fallback para restart. (`bin/deploy.sh:322-337,343-372`) | Ficam fora do exclusivo descrito, mas o código explica que `docker exec` não recebe esse HUP, destinado ao master PID 1. (`bin/deploy.sh:327-329`) Comportamento real das cascas de produção: [nao verificado]. |
| Reload diário 03:30 | `RAW('30 3 * * *', ...deploy.sh --reload-agendado)`; RAW também recebe `cron_run.sh`. (`app/config/crons.py:326-327,1475,1483-1486,1860-1864`) O modo usa o ramo HUP. (`bin/deploy.sh:65,346-359`) | Pelo contrato, teria o compartilhado do envelope, mas não o exclusivo do `--restart`. A premissa de que esse reload mata o cron não é sustentada pelo código: ele descreve preservação do exec. (`bin/deploy.sh:327-329`) |
| `docker compose up -d`, rebuild/recreate | Explicitamente fora de `deploy.sh`; imagem, command e Env novos precisam de ato de infra. (`bin/deploy.sh:20-22,66-69`) README ainda ensina `docker rm -f saas_web && docker-compose up -d --no-deps web`. (`README.md:35-38`) | Escapam: não chamam o exclusivo descrito no contrato. Execução atual desses comandos: [nao verificado]. |
| Restart automático da casca | Mensageria declara `restart: unless-stopped`. (`docker-compose.mensageria.yml:9`) | Não há aquisição de flock nessa definição; política/comportamento vivo: [nao verificado]. |
| Outros scripts examinados | `recursos.sh` aplica `docker update`, não restart; `db_teste.sh` e `sombra_db.sh` criam ou dão start nos próprios bancos auxiliares. (`bin/recursos.sh:140-145`; `bin/db_teste.sh:49-70`; `bin/sombra_db.sh:86-100`) | Esses trechos não reiniciam core/ui/mensageria. Outras vias externas, reinício de host/daemon, OOM e ações manuais: [nao verificado]. |

O `docker-compose.yml` principal não está na cópia e é declarado específico de produção/dev e ignorado; não se pode provar daqui a configuração core/ui instalada. (`.gitignore:40-41`; ausência na busca da cópia: [nao verificado])

As cadeias de fatia e auto-cura chamam `--reload-copia` e `--sem-sombra`, portanto chegam ao HUP, não a um restart adicional. (`bin/molde_fatia/cadeia.sh:32,48,89`; `bin/molde_autocura/cadeia.sh:31,45,86`; `bin/deploy.sh:338-359`) O auto-revert do vigia de login chama `--sem-migrate`, igualmente HUP. (`bin/vigia_logins.sh:87`; `bin/deploy.sh:338-359`)

**Q3 — Existem disparos fora do envelope**

- **Todos os `CRONS` declarados, inclusive `RAW`, são envelopados** por `_run`; os elos também entram por `cron_run.sh`. (`app/config/crons.py:1475,1483-1486,1860-1874`; `bin/encadeado.sh:102-104`) Crontab instalado de produção idêntico ao render: [nao verificado].
- **O despachante da corrente escapa antes dos elos:** a linha gerada executa `encadeado.sh --corrente` depois do wrapper da raiz; esse despachante faz `docker exec saas_core ... crons corrente` antes de entrar no wrapper dos elos. (`app/config/crons.py:1884-1885`; `bin/encadeado.sh:40-45,64,102-104`) Se esse render falhar, apenas escreve ALARME e sai 2: não chega a `cron_run.sh`/`cron_quebrado`. (`bin/encadeado.sh:47-52`) Portanto, o contrato de lock apenas no envelope **não protege o próprio nascimento da corrente** desse restart.
- **Cron avulso em `/etc/cron.d`:** `deploy_agendado.sh agendar` grava chamada direta a `deploy_agendado.sh rodar`, que executa as esteiras diretamente. (`bin/deploy_agendado.sh:54-57,63-73`) Esse disparo não passa pelo envelope; se a esteira chama `deploy.sh --restart`, terá a proteção do lado deploy prevista pelo contrato, mas seus demais comandos não ganham compartilhado automaticamente. Conteúdo e instalação dos cron.d vivos: [nao verificado].
- **Timers systemd diretos:** esteira, fabricante, integrador, relato e teto dia/noite chamam scripts em `ExecStart` diretamente, sem `cron_run.sh`. (`bin/hasner-esteira.service:15`; `bin/hasner-fabricante.service:14`; `bin/hasner-integrador.service:13`; `bin/hasner-relato.service:15`; `bin/hasner-teto-dia.service:7`; `bin/hasner-teto-noite.service:7`) Os respectivos disparos estão declarados nos timers. (`bin/hasner-esteira.timer:7`; `bin/hasner-fabricante.timer:8`; `bin/hasner-integrador.timer:6`; `bin/hasner-relato.timer:5-6`; `bin/hasner-teto-dia.timer:5`; `bin/hasner-teto-noite.timer:5`) Instalação/ativação viva: [nao verificado].
- **Exemplo concreto de timer que usa uma casca:** `hasner-relato.service` chama `relato.sh`; este chama `relato_nomes.sh`, que executa Django no core diretamente. (`bin/hasner-relato.service:15`; `bin/relato.sh:32`; `bin/relato_nomes.sh:17-21`) Esse trabalho escapa do compartilhado proposto; não é correto concluir que todo timer listado executa cron de negócio.
- **Chamadas manuais existentes também escapam:** `lavrar_deslogados.sh` documenta sua chamada manual e executa diretamente `docker exec ... lavrar_deslogados`; o agendamento normal desse comando pertence à corrente. (`bin/lavrar_deslogados.sh:11-18`) `gate_cobranca.sh` executa sondas e `cartao_x_txt_divergentes` diretamente; `placar_code.sh` executa diversos management commands diretamente. (`bin/gate_cobranca.sh:36-40`; `bin/placar_code.sh:215-218,238-259`) Uso efetivo dessas portas durante restart: [nao verificado].

Não há prova de crontab cru atualmente instalado; os exemplos antigos de linhas cruas em docstrings de commands são instruções, não evidência de instalação. (`app/ponto/management/commands/detectar_ausencias.py:9`; `app/ponto/management/commands/processar_alertas_avancados.py:7`; situação instalada: [nao verificado])

## 09/11 02:21 · sessao 2bd182e82d5b · - · - · origem=Ronald (tmux)

**1. Censo da marca na cópia**

- Escritor operacional localizado: `bin/cron_run.sh:9-10` cria `logs/crons_em_curso/${N}_${E}.$$`, grava início e remove no `EXIT`. A marca nasce antes do comando (`bin/cron_run.sh:11`) e permanece durante placar, carimbo e chamada ao vigia (`bin/cron_run.sh:13-37`).
- Leitor operacional localizado: `bin/deploy.sh:108-113`. Enumera arquivos, considera vivo o PID da extensão quando `/proc/PID` existe, apaga marcas sem esse PID e recusa restart. Chamado antes dos portões (`bin/deploy.sh:115-117`) e imediatamente antes do restart (`bin/deploy.sh:338-342`).
- Escritor secundário: o próprio deploy apaga marcas julgadas órfãs (`bin/deploy.sh:109`).
- `bin/contrato_deploy.sh:79-82` não lê o diretório em execução: inspeciona texto dos scripts e exige que o deploy e o envelope contenham `crons_em_curso`.
- Testes escrevem marcas em fixtures: `bin/tests/test_deploy_reload_gracioso.sh:34,91`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:63,76,88,110`. Em particular, o selo de reload exige recusa mediante marca, sem pegar o futuro lock (`bin/tests/test_deploy_reload_gracioso.sh:125-131`); os casos 15/16 também injetam marca durante a prova de casca (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`).

Não localizei leitores do diretório em hook, vigias ou `gerar_estado` mediante busca integral da cópia; outros acessos indiretos/dinâmicos são **[nao verificado]**. O hook mede trabalho por processos de teste/push (`bin/hook_stop_fila1.py:373-397`) e bloqueia clear por outro lock, `logs/sombra.lock` (`bin/hook_stop_fila1.py:785-794`). O vigia da esteira usa trava própria e delega ao Python (`bin/vigia_esteira.sh:8-11`); vigia da árvore também tem trava própria (`bin/vigia_arvore.sh:25`).

Existe consumidor **documental/humano** do significado da marca: esperar a conclusão da sombra pela ausência de `sombra.sh_-.*`, nunca por pgrep (`CLAUDE.md:92`; `app/docs/INFRA.md:59-60`). O contrato não define se a marca será criada antes ou depois da espera no lock; preservação exata desse significado é **[nao verificado]**. Se criada antes, passará a incluir cron esperando; se criada depois, não mostrará a fila — inferência de `bin/cron_run.sh:9-11` e do contrato fornecido **[nao verificado: implementação proposta ausente]**.

Há um escritor histórico fora do envelope mencionado no relato: `logs/gate10/gate10_condicao1.sh`, com marca própria durante apply de aproximadamente 45 minutos (`app/docs/RELATO.md:4472-4489`). Não está comprovado como código presente/ativo; existência atual e migração para lock são **[nao verificado]**.

**6. Confronto com a lei escrita**

- **Sem contradição demonstrada com seção 2:** ela manda publicar via deploy, usa HUP automático e reserva `--restart` manual com recusa por cron vivo (`CLAUDE.md:90`). O lock exclusivo não bloqueante conserva recusa; HUP permanece fora do problema do exec (`bin/deploy.sh:322-337`). Preservar todos os demais portões é **[nao verificado]**, pois implementação ausente.
- **Seção 7 / lap-67:** a autoridade declarativa é `config/crons.py`, não uma lista nova no contrato (`CLAUDE.md:236-239`; `app/docs/LAPIDES.md:736-741`). Introduzir lock no envelope não contradiz essa fonte. “Todo cron” continuar passando pelo envelope é **[nao verificado]** e depende do censo do outro explorer.
- **Seção 7 / lap-68:** corrente continua dependendo do arquivo do anterior, não do minuto (`CLAUDE.md:240-242`; `app/docs/LAPIDES.md:745-751`). Rc75 não carimba fim hoje, pois só 0/2 carimbam (`bin/cron_run.sh:19-25`); portanto dependente não recebe conclusão bem sucedida inventada.
- **GATE TEMPORAL:** o texto exige cron/arquivo para espera longa, proíbe sleep do Code e `pgrep -f` (`CLAUDE.md:204`). O contrato põe espera limitada no processo do cron, via lock em arquivo, e não propõe pgrep. Não demonstrada contradição com essa formulação; comportamento real é **[nao verificado]**.
- **“Recusa, não espera”:** a regra atual é dirigida ao deploy `--restart`, não ao cron (`bin/deploy.sh:103-117`). O selo condena explicitamente `ESPERA_CRON_S` e o retorno do “espera-e-reinicia” (`bin/contrato_deploy.sh:50-51`). Cron esperar deploy com `-w 600`, enquanto deploy usa `-n`, não reintroduz esse fallback; implementação é **[nao verificado]**.
- **Incompatibilidade mecânica a tratar:** substituir a decisão da marca pelo lock deixa os selos atuais presos à autoridade antiga: exigência textual (`bin/contrato_deploy.sh:79-82`) e fixtures que só criam marca (`bin/tests/test_deploy_reload_gracioso.sh:125-131`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`). Isso é incompatibilidade dos testes com a migração proposta, não prova de contradição com a lei.
- A frase “rc75 não é vermelho” existe no capítulo da **suite** (`CLAUDE.md:95-104`), enquanto o envelope de cron classifica qualquer saída diferente de 0/2 como falha (`bin/cron_run.sh:27-35`). Generalizar a exceção da suite a todos os crons não é sustentado pelo texto.

## 09/11 02:21 · sessao c79c9d113d29 · - · - · origem=Ronald (tmux)

Q4 — **O fail-open está declarado no contrato existente desta cópia:** “FALHA ABERTO: toda excecao vira linha FALHA na trilha e a ferramenta passa; medida ilegivel = ‘nao sei’, nunca age.” (`bin/disjuntor.py:34`). O tratamento externo mantém essa regra, exceto B3, que nega quando não sabe; CENSO ganha seu próprio `except Exception`, grava `CENSO ABRIU-SEM` e retorna `None`. (`bin/disjuntor.py:1643`, `bin/disjuntor.py:1645`, `bin/disjuntor.py:1653`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1525`).

**Isso não basta para realizar o aval específico:** o I-1 fornecido exige recusa de brief sem CENSO, enquanto essa exceção permite continuar sem obter a decisão do juiz. A compatibilidade autorizada dessa exceção com o aval é **[nao verificado]**. (`bin/disjuntor.py:1504`, `bin/disjuntor.py:1525`; aval literal fornecido, sem arquivo numerado na cópia).

**LEI-AKITA 1 não equivale simplesmente a “qualquer tratamento de exceção é proibido”:** ela caracteriza fallback como sobrevivência de derivador paralelo. Portanto, apenas chamar este ramo de fail-open não demonstra violação da linha 1; há, porém, conflito verificável com “exceção [...] nunca destravando a trava” e com a literalidade do aval. (`CLAUDE.md:17`, `CLAUDE.md:19`, `CLAUDE.md:32`, `CLAUDE.md:34`, `CLAUDE.md:37`).

Q8 — mapa dos escritores e leitores:

| Objeto | Quem escreve | Quem lê / significado |
|---|---|---|
| `logs/sessao/disjuntor.log` | Porta física `trilha`, append com carimbo próprio e detalhe limitado a 300 caracteres; exceção de escrita é engolida. (`bin/disjuntor.py:68`, `bin/disjuntor.py:135`, `bin/disjuntor.py:139`, `bin/disjuntor.py:140`, `bin/disjuntor.py:142`) | `ler` conta eventos gerais; `_censo_24h` interpreta eventos CENSO. O hook também reconhece o arquivo como trilha própria, proibida para provar espera por avanço de arquivo. (`bin/disjuntor.py:1717`, `bin/disjuntor.py:1725`, `bin/disjuntor.py:1784`, `bin/hook_stop_fila1.py:583`, `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:598`) |
| `CENSO OK <obra> :: via=... brief=...` | `_censo_nega`, depois de validar os briefs citados. (`bin/disjuntor.py:1513`, `bin/disjuntor.py:1517`) | `_censo_24h` incrementa `ok` e encerra o intervalo daquela obra. (`bin/disjuntor.py:1771`, `bin/disjuntor.py:1797`) |
| `CENSO ABRIU-SEM <obra> :: ...` | `_censo_nega`, se uma exceção escapar do check. (`bin/disjuntor.py:1525`) | `_censo_24h` incrementa `abertos_sem`. (`bin/disjuntor.py:1777`, `bin/disjuntor.py:1801`) |
| `NEGOU DISJUNTOR CENSO: construtor de <obra> sem CENSO...` | `main_pretool`, antes de enviar JSON deny. (`bin/disjuntor.py:1636`, `bin/disjuntor.py:1639`) | `_censo_24h` inicia intervalo por obra e incrementa `negados`. (`bin/disjuntor.py:1771`, `bin/disjuntor.py:1794`) |
| Medidas `abertos_sem`, `negados`, `ok`, `ociosa_h`, `esperando` | `_censo_24h` calcula o dicionário, consumido por `ler`. (`bin/disjuntor.py:1767`, `bin/disjuntor.py:1807`) | `_censo_txt` apresenta as medidas; `linha` a inclui; `gerar_estado` importa `ler/linha` e grava o destino recebido. (`bin/disjuntor.py:1812`, `bin/disjuntor.py:1820`, `bin/disjuntor.py:1895`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`, `bin/gerar_estado.py:390`) |
| `ESTADO.md` publicado | `relato.sh` pede `$TMP/ESTADO.md` ao gerador e o entrega à raspagem. (`bin/relato.sh:38`, `bin/relato.sh:103`, `bin/relato.sh:105`) | O destino público recebe esse artefato raspado; consumo posterior em produção **[nao verificado]**. (`bin/relato.sh:105`) |
| Linha literal `PAREI: censo-<obra> \| espera Codex` no topo | O diff **manda a sessão escrever**, não grava essa linha: é texto da negativa; `--censo` também só imprime instrução. Escritor concreto da linha em produção **[nao verificado]**. (`bin/disjuntor.py:1414`, `bin/disjuntor.py:1521`, `bin/disjuntor.py:1908`) | `_esperando_com_prova` lê o primeiro painel via `_linha_do_topo`, exige forma literal e chama `_prova_censo`. (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:702`, `bin/hook_stop_fila1.py:705`) |
| Linha automática `(NEGOU CENSO)` no RELATO | `relato_negacao` monta linha de máquina e usa `core.esteira_vigia.no_relato`; **não é** a linha PAREI do topo. (`bin/disjuntor.py:221`, `bin/disjuntor.py:234`, `bin/disjuntor.py:236`) | A porta insere em `## ALARMES DA ESTEIRA`, preservando o topo; a escrita usa flock no diretório e temporário único. (`app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:834`, `app/core/esteira_vigia.py:879`, `app/core/esteira_vigia.py:884`) |

**Há divergência de leitores da linha PAREI censo.** O Stop a trata como ESPERANDO provado e compartilha a mesma resposta memoizada com o orçamento; a liberação ocorre por essa prova. (`bin/hook_stop_fila1.py:703`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`).

O handoff exibe `PAREI`, mas decide “trava o item?”/“devolve o turno?” somente pelos predicados genéricos; esses exigem `!`, ausente na linha censo. Assim, com censo vivo, o Stop pode liberar enquanto o handoff imprime “item em curso ANDA” e “não [...] devolve”. (`bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`, `bin/handoff_sessao.sh:123`, `bin/hook_stop_fila1.py:1171`, `bin/hook_stop_fila1.py:1284`).

O alarme aceita qualquer substring `PAREI:` nas primeiras 40 linhas, sem prova; portanto desarma também com o produtor morto, situação em que o Stop registra falta de prova. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:124`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`, `bin/hook_stop_fila1.py:1286`).

**Achado material de semântica das medidas:** `OK` e `ABRIU-SEM` nascem durante a avaliação CENSO, antes de outras travas ainda capazes de negar a abertura. No Agent, CENSO antecede B4; no Codex, antecede `codex_decide`. Logo esses eventos provam passagem/falha do check, não abertura efetiva de construtor. (`bin/disjuntor.py:1517`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1583`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1603`, `bin/disjuntor.py:1607`, `bin/disjuntor.py:1621`).

`ociosa_h` soma, por obra, do primeiro NEGOU ao próximo OK ou até agora; não consulta ocupação da vaga nem processo censo. É duração de intervalos de negativas, e pode somar obras simultâneas ou encerrar num OK cuja abertura depois foi negada. A correspondência com “horas de vaga ociosa” é **[nao verificado]**. (`bin/disjuntor.py:1778`, `bin/disjuntor.py:1795`, `bin/disjuntor.py:1800`, `bin/disjuntor.py:1807`).

Além disso, trilha inexistente produz zero, enquanto falha ao gravar trilha é silenciada; logo zero nesse contador não prova zero aberturas reais sem censo. (`bin/disjuntor.py:142`, `bin/disjuntor.py:1803`).

**Relógio/ordem:** as funções novas recebem `agora` na medida e não introduzem leitura direta nova do relógio; o escritor existente `trilha` já usa `datetime.now`, e as bordas `main_pretool`/CLI obtêm `time.time`. Não há prova de novo relógio fora da borda no diff; permanece o carimbo existente da trilha. (`bin/disjuntor.py:140`, `bin/disjuntor.py:1632`, `bin/disjuntor.py:1774`, `bin/disjuntor.py:1780`, `bin/disjuntor.py:1903`).

Não há nova ordem entre locks demonstrada nesses trechos: CENSO usa a porta de append existente, declarada sem trava; a linha de máquina no RELATO continua usando a porta e a trava existentes. A ordem **entre verificações** afeta o significado dos eventos de abertura, conforme acima. (`bin/disjuntor.py:1637`, `bin/disjuntor.py:1642`, `app/core/esteira_vigia.py:834`, `bin/disjuntor.py:1603`).

Observação de fonte: `app/docs/CORTES.md` desta cópia termina na linha 425; as referências 428/430 pertencem ao raspado fornecido e não podem ser tratadas como linhas existentes desta cópia. (`app/docs/CORTES.md:425`; conteúdo integral das linhas raspadas: **[nao verificado]**).

## 09/11 02:21 · sessao 33daae8acd7e · - · - · origem=Ronald (tmux)

Q6 — Há uma só autoridade de vivacidade de processo: `_prova_censo` chama `_prova_pid`, reutilizando exclusão de ancestral, comparação do executável/script, estabilidade do PID e rejeição de Z/X (`bin/hook_stop_fila1.py:682`, `bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:529`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:546`).

A regra de vínculo com o censo é nova: procura wrapper desta raiz, mesmo UID, `CODEX_OBRA` exato e papel censo/leitura; o ESPERANDO genérico aceita produtor e pid/arquivo declarados, sem exigir obra/papel. Portanto, não são predicados idênticos; há composição com o juiz de processo existente e uma regra específica nova de identificação do produtor (`bin/hook_stop_fila1.py:661`, `bin/hook_stop_fila1.py:663`, `bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:682`, `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:729`).

Ambos entram na mesma decisão `_esperando_com_prova`, memoização, orçamento e liberação do Stop; não foi criada liberação final independente (`bin/hook_stop_fila1.py:703`, `bin/hook_stop_fila1.py:707`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`).

Os mutantes simples “aceitar construtor”, “casar prefixo de obra” e “liberar quando o processo já foi removido” encontram casos negativos específicos, com exigência de block/siga e motivo NOPROVA (`bin/tests/test_hook_esperando_com_prova.sh:80`, `bin/tests/test_hook_esperando_com_prova.sh:84`, `bin/tests/test_hook_esperando_com_prova.sh:87`, `bin/tests/test_hook_esperando_com_prova.sh:206`, `bin/tests/test_hook_esperando_com_prova.sh:209`, `bin/tests/test_hook_esperando_com_prova.sh:214`).

Sobreviventes plausíveis por leitura: remover proteção contra zumbi, UID ou troca de starttime. A fixture mata e faz `wait`, removendo o processo; não produz Z/X, outra identidade de usuário ou reciclagem de PID durante leitura. Execução desses mutantes: [nao verificado]. (`bin/hook_stop_fila1.py:546`, `bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:685`, `bin/tests/test_hook_esperando_com_prova.sh:31`, `bin/tests/test_hook_esperando_com_prova.sh:57`, `bin/tests/test_hook_esperando_com_prova.sh:194`, `bin/tests/test_hook_esperando_com_prova.sh:213`).

Q7 — Os quatro mutantes declarados têm mordida demonstrável por leitura das asserções:

- Linha ausente passa a zero: altera o resultado esperado do caso sem-linha, rc 1. (`bin/tests/test_censo_antes_da_vaga.sh:70`, `bin/tests/test_censo_antes_da_vaga.sh:175`).
- Remover corte: altera os dois casos que retiram o corte da última pergunta. (`bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:176`).
- Tirar porta Agent: deixa de produzir a negativa específica CENSO exigida nos casos Agent negativos. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:112`, `bin/tests/test_censo_antes_da_vaga.sh:177`).
- Tirar porta Codex: deixa de produzir aquela negativa no caso construtor sem brief. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:131`, `bin/tests/test_censo_antes_da_vaga.sh:178`).

A harness exige retorno não zero da função de casos depois da mutação e verifica âncora única; portanto, não basta declarar o mutante. Rodada integral dos selos: [nao verificado], pois criam arquivos (`bin/tests/test_censo_antes_da_vaga.sh:160`, `bin/tests/test_censo_antes_da_vaga.sh:165`, `bin/tests/test_censo_antes_da_vaga.sh:170`, `bin/tests/test_censo_antes_da_vaga.sh:173`).

**Mutante sobrevivente concreto:** trocar `for q in perguntas:` por `for q in perguntas[-1:]:`. Verifica somente a última pergunta; os dois casos que retiram corte mexem sempre na última. Reproduzi todos os 15 resultados de `casos_rc` em memória, extraindo AST e usando StringIO, sem gravar arquivos; também confirmei aceitação da primeira pergunta sem corte com a última cortada. A costura usa apenas briefs conforme/sem-linha, sem casos N>0 adicionais. (`bin/disjuntor.py:1469`, `bin/tests/test_censo_antes_da_vaga.sh:68`, `bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:99`, `bin/tests/test_censo_antes_da_vaga.sh:100`, `bin/tests/test_censo_antes_da_vaga.sh:112`).

A asserção positiva `_passa_censo` é fraca: qualquer saída sem `CENSO` passa, inclusive deny de outra trava ou saída vazia por erro/timeout; o status de `_pre` não é conferido. Já as negativas exigem deny+CENSO+PAREI específico. (`bin/tests/test_censo_antes_da_vaga.sh:97`, `bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:106`).

A medida verifica rótulos e presença de número decimal, sem valor esperado; não há caso que provoque exceção e valide `ABRIU-SEM`, nem duração conhecida para ociosidade. Assim, retirar a trilha do ramo de exceção de `_censo_nega` permanece sem cobertura demonstrada. (`bin/tests/test_censo_antes_da_vaga.sh:141`, `bin/tests/test_censo_antes_da_vaga.sh:144`, `bin/tests/test_censo_antes_da_vaga.sh:147`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1526`).

## 09/11 02:21 · sessao 341d3f8bce55 · - · - · origem=Ronald (tmux)

2. **Um juiz de conteúdo localizado, mas a cobertura da porta é incompleta.** `censo_do_brief` decide a forma do bloco; `_censo_nega` o chama para os briefs reconhecidos; a CLI `--censo` chama a mesma função. O Stop prova produtor vivo, não o conteúdo do brief. `gerar_estado` apenas consome `disjuntor.ler/linha`; handoff consome predicados de fila/PAREI. Não localizei outro reconhecedor de “brief tem CENSO” nesses leitores nem autoridade anterior dessa pergunta em `app/core/juizes.py`/`app/docs/JUIZES.md`; a ausência global de qualquer implementação equivalente fica **[nao verificado]**. (`bin/disjuntor.py:1417`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`, `bin/hook_stop_fila1.py:649`, `bin/gerar_estado.py:280`, `bin/handoff_sessao.sh:120`, `app/core/juizes.py:170`, `app/core/juizes.py:706`, `app/core/juizes.py:1025`, `app/core/juizes.py:1173`, `app/core/juizes.py:1285`, `app/docs/JUIZES.md:19`)

3. **Vias e escapes demonstráveis pela cópia:**

| Forma | Trava nova / classificação |
|---|---|
| `Agent`, tipo fora de `SO_LEITURA`, prompt com caminho absoluto terminado `wt-*`, obra reconhecida e presente nos andáveis | Passa em `_censo_nega`. (`bin/disjuntor.py:98`, `bin/disjuntor.py:100`, `bin/disjuntor.py:1603`, `bin/disjuntor.py:1507`) |
| Mesmo `Agent`, mas `Construa OBRA-E3 ...` sem âncora `obra X`/`leitura ... da X` | `_obra_de` retorna `None`; `_censo_nega` libera. Escape real se o trabalho é construtor estrutural da fila 1. (`bin/disjuntor.py:108`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1507`) |
| `Agent` construtor com cópia cujo nome não casa `wt-*`, caminho relativo ou sem cópia citada | Não entra no novo ramo. É escape real se ainda é construtor da classe avalizada; o teste define “sem cópia” como não construtor, mas não prova que escrever seja impossível. (`bin/disjuntor.py:100`, `bin/disjuntor.py:1603`, `bin/tests/test_censo_antes_da_vaga.sh:129`) |
| `Bash` com `CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh --construtor ...` | Interceptado pelo parser e `_censo_nega`. (`bin/disjuntor.py:1332`, `bin/disjuntor.py:1583`) |
| Wrapper com papel/obra herdados: `export CODEX_OBRA=X CODEX_PAPEL=construtor; bin/codex_pergunta.sh --construtor ...` | Parser lê `papel='?'`, obra vazia; novo ramo pula. Wrapper aceita ambiente herdado. **Escape real de uma via autorizada.** (`bin/disjuntor.py:1333`, `bin/disjuntor.py:1343`, `bin/disjuntor.py:1584`, `bin/codex_pergunta.sh:81`) |
| `env CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh ...` | `_INVOC_RE` admite `env` como prefixo, mas as atribuições só antes dos prefixos; não reconhece esta chamada. **Escape real**, wrapper lê o ambiente normalmente. (`bin/disjuntor.py:1250`, `bin/disjuntor.py:1342`, `bin/codex_pergunta.sh:81`) |
| `bash -c "..."`, invocação encapsulada em script externo | Residual expressamente declarado; wrapper não consulta CENSO antes da vaga nem do `codex exec`. Escape real se abre construtor abrangido pelo aval. (`bin/disjuntor.py:1247`, `bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:394`) |
| Wrapper chamado diretamente no terminal/fora do `PreToolUse` | Nenhuma guarda CENSO no wrapper; B3 já tem consulta própria para verificador. A abertura de construtor continua possível sem consulta CENSO. (`bin/codex_pergunta.sh:370`, `bin/codex_pergunta.sh:384`, `bin/codex_pergunta.sh:394`) |
| `codex exec` direto | Residual declarado, porém proibido pela regra de uso do wrapper; via irregular, não evidência de uma segunda abertura autorizada. (`bin/disjuntor.py:1248`, `CLAUDE.md:347`) |
| Construtor de obra fechada/fila 2, leitura/censo/verificador | Fora da classe construtor estrutural fila 1. (`bin/disjuntor.py:1507`, `bin/tests/test_censo_antes_da_vaga.sh:123`, `bin/tests/test_censo_antes_da_vaga.sh:125`, `bin/tests/test_censo_antes_da_vaga.sh:127`, `bin/tests/test_censo_antes_da_vaga.sh:135`) |
| `bin/fabricante.sh` abre `claude -p` para fabricar fatias | A abertura dessa sessão não é `Agent`; não passa pela trava nova. O próprio fabricante se declara esteira estrutural e distingue fabricar em `.esteira/` da aplicação por `construir.py`; enquadramento de cada fatia como “obra ESTRUTURA (fila 1)” do aval **[nao verificado]**. (`bin/fabricante.sh:2`, `bin/fabricante.sh:6`, `bin/fabricante.sh:150`, `bin/fabricante.sh:162`) |

As chamadas do hook configuradas na cópia apontam o caminho absoluto `/home/ronald/saas-hasner/bin/disjuntor.py`; que o arquivo vivo nesse caminho contenha este diff é **[nao verificado]**. (`.claude/settings.json:68`)

As respostas dos parsers acima foram confirmadas por chamadas puras em memória, com bytecode desabilitado; não executei wrapper nem teste com criação de arquivos. Referências da implementação: `bin/disjuntor.py:1250`, `bin/disjuntor.py:1332`, `bin/disjuntor.py:511`.

5. **A trava não inventa outra definição de andável.** `_fila1_viva` lê BACKLOG e chama diretamente `H._itens_andaveis`; este usa `_item_parado`, o mesmo predicado de `_item_da_ordem_viva` e `_ids_que_nao_andam`. Não há derivação paralela nova de pertencimento à fila nessa costura. (`bin/disjuntor.py:1489`, `bin/disjuntor.py:1498`, `bin/hook_stop_fila1.py:158`, `bin/hook_stop_fila1.py:212`, `bin/hook_stop_fila1.py:218`, `bin/hook_stop_fila1.py:239`, `bin/hook_stop_fila1.py:333`)

**Andáveis e item em curso são perguntas distintas:** os andáveis vêm do bloco OBRAS; o item em curso vem do marcador `ORDEM-VIVA-TOPO`, sem fallback pela ordem física. A própria documentação da função explicita essa distinção. (`bin/hook_stop_fila1.py:180`, `bin/hook_stop_fila1.py:196`, `bin/hook_stop_fila1.py:221`, `bin/hook_stop_fila1.py:245`, `bin/hook_stop_fila1.py:1292`)

**B3 não é juiz de fila 1.** Seu universo vem de reprovações/triagens por obra e o juiz `estado_b3` devolve parada/livre/não sei; não consulta BACKLOG para decidir pertencimento à fila. Compartilha com I-1 o reconhecedor de identidade `_obra_de`, o que não transforma B3 em outro juiz da fila. (`bin/disjuntor.py:903`, `bin/disjuntor.py:909`, `bin/disjuntor.py:993`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1604`)

**Fragilidade separada:** BACKLOG ausente vira texto vazio, logo conjunto de fila vazio e liberação silenciosa sem `CENSO ABRIU-SEM`; não é divergência de predicado, mas erro de fonte interpretado como “não abrangido”. (`bin/disjuntor.py:1493`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1498`, `bin/disjuntor.py:1507`)

_2988 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
