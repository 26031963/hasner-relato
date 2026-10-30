# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 02:20** · publicado em **30/10/2026 02:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 02:20 · sessao 27807bab5270 · - · - · origem=Ronald (tmux)

| Motivo | Catálogo | `motivos_atendidos` × ramo HIST |
|---|---|---|
| `batida_fora_escala_grave` | Resposta `escolha`: `app/chamados/catalogo/perguntas.py:82-85`; espelho `app/chamados/models.py:2395-2399`. | Ausente do conjunto devolvido pelo juiz (`app/chamados/models.py:2502-2503`) e dos valores dos mapas HIST (`app/chamados/models.py:2466-2483`): concordam em não reconhecê-lo como motivo atendido pelo chamado do dia. |
| `sem_posto` | Resposta `posto_dropdown`: `app/chamados/catalogo/perguntas.py:36-39`; espelho `app/chamados/models.py:2356-2360`. | Ausente do conjunto devolvido pelo juiz (`app/chamados/models.py:2502-2503`) e dos valores dos mapas HIST (`app/chamados/models.py:2466-2483`): mesma exclusão. |
| `foto_ausente` | Resposta `escolha`: `app/chamados/catalogo/perguntas.py:94-97`; espelho `app/chamados/models.py:2405-2409`. | Ausente do conjunto devolvido pelo juiz (`app/chamados/models.py:2502-2503`) e dos valores dos mapas HIST (`app/chamados/models.py:2466-2483`): mesma exclusão. |
| `saida_sem_entrada` | Resposta `hora`: `app/chamados/catalogo/perguntas.py:56-58`; espelho `app/chamados/models.py:2373-2378`. | Juiz aceita qualquer categoria/subcategoria presente nos mapas e devolve os quatro motivos (`app/chamados/juizes.py:1597-1599`). HIST só aceita quando o motivo principal resultante é exatamente `saida_sem_entrada` (`app/chamados/reconciliador.py:1391-1394`), cujo único mapeamento é a categoria homônima (`app/chamados/models.py:2468`). Portanto divergem para chamados mapeados como `orfao_14h`. |
| `orfao_14h` | Resposta `hora`: `app/chamados/catalogo/perguntas.py:48-50`; espelho `app/chamados/models.py:2366-2371`. | Juiz também aceita chamado da categoria `saida_sem_entrada` (`app/chamados/juizes.py:1597-1599`; `app/chamados/models.py:2502`). HIST exige resultado principal `orfao_14h` (`app/chamados/reconciliador.py:1391-1394`), encontrado nas demais categorias e em todas as subcategorias dos mapas (`app/chamados/models.py:2469-2482`). Divergem para categoria `saida_sem_entrada` sem subcategoria que prevaleça. |
| `intervalo_volta` | Resposta `hora`: `app/chamados/catalogo/perguntas.py:132-134`; espelho `app/chamados/models.py:2437-2442`. | Juiz inclui explicitamente (`app/chamados/models.py:2503`; `app/chamados/juizes.py:1599`); nenhum mapa tem esse valor (`app/chamados/models.py:2466-2483`). HIST nunca o acrescenta pela igualdade exata (`app/chamados/reconciliador.py:1393-1394`): divergem. |
| `intervalo_saida` | Resposta `hora`: `app/chamados/catalogo/perguntas.py:124-126`; espelho `app/chamados/models.py:2430-2435`. | Juiz inclui explicitamente (`app/chamados/models.py:2503`; `app/chamados/juizes.py:1599`); nenhum mapa tem esse valor (`app/chamados/models.py:2466-2483`). HIST nunca o acrescenta (`app/chamados/reconciliador.py:1393-1394`): divergem. |

Linhas adicionais para consolidação:

- Implementação do juiz: `app/chamados/juizes.py:1589-1600`; normalização das entradas em `:1595-1596`; nenhum teste de status, competência ou data nessa função.
- Declaração de autoridade dos mapas: `app/chamados/models.py:2504-2505`. A tabela `'chamado'` do registro central está em `app/core/juizes.py:226-242` e não contém `motivos_atendidos`; contém `pergunta_viva` em `:232`, competência em `:240` e fio em `:241`.
- Derivação viva efetivamente consulta o juiz: `app/chamados/services/disputa_emissao.py:194-196`. Seus candidatos exigem colaborador da disputa, status em `VIVOS` e categoria diferente de `disputa_supervisao` (`:181-184`); pergunta do mesmo motivo ainda não materializada exclui candidato (`:209-212`); datas conhecidas divergentes excluem candidato (`:214-217`); quantidade final diferente de um retorna `(None,None)` (`:219-227`).
- Na criação, intervalo pode nascer costurado pelo derivador (`app/chamados/services/disputa_emissao.py:379-380`); os motivos do conjunto que exige destino também derivam (`:381-383`); pergunta recém-criada sem destino nos quatro motivos do dia chama `reatar_pergunta_orfa` (`:404-409`).
- `resolvedora_valida` veta divergência comprovada de turno/data em `app/chamados/juizes.py:1737-1738`; permite mesma data (`:1721-1722`) e diferença de um dia quando o turno atravessa meia-noite ou a travessia é desconhecida (`:1730-1736`). Sem data da pergunta, sem chamado, módulo fora de escopo DIA ou dia do chamado desconhecido/inválido, permite (`:1702-1720`).

## 30/10 00:24 · sessao 47c16c4ae35b · mariaB · verificador · origem=Code

**Encontrei duas incompletudes de fechamento e uma falha temporal na fonte que o novo caminho passa a gravar.** A centralização da elegibilidade está correta: coleta, porta e vigia usam o mesmo Q; quem decide continua sendo C6. Não há contorno por variável local nesta volta. (`app/chamados/services/relavra_via.py:170`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/vigia_fio.py:399`)

1. **O selo de custo não mede a cura no caminho da batida.**  
   Cenário: célula rejulgada com perguntas carimbadas elegíveis → cada candidata acrescenta transação, releitura travada e C6; cada cura acrescenta UPDATE, trilha e um `on_commit` de beco. A coleta reaproveita a query existente, mas esse trabalho posterior é novo. (`app/chamados/services/fabrica_da_celula.py:101`, `app/chamados/services/fabrica_da_celula.py:146`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:234`, `app/chamados/signals.py:202`)

   O teste novo chama somente a porta, com perguntas **validadas**: C6 retorna imediatamente. Ele certifica `custo(3) == 3*custo(1)`, sem executar batida, cura, trilha ou callback. Os exemplos pertencem a colaboradores diferentes. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:57`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:191`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:197`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:200`, `app/chamados/juizes.py:1006`)

   O selo existente também cobra callbacks, mas suas perguntas não têm carimbo para cair. Portanto pode continuar verde sem exercer o ramo novo. **Classe: selo sem mordida no caminho exigido.** Não afirmo estouro numérico do teto sem medição. (`app/ponto/tests/test_selo_chokepoint_escrita.py:169`, `app/ponto/tests/test_selo_chokepoint_escrita.py:207`, `app/ponto/tests/test_selo_chokepoint_escrita.py:237`; medição absoluta: [não verificado])

2. **O gancho não resolve a frota estável.**  
   Cenário: célula passada acusa, pergunta sem resposta está materializada e viva, disputa aberta, impressão dos insumos permanece igual → cartório pula a célula, a fábrica não é chamada e o carimbo permanece. O lote de perguntas também exclui o fato porque a pergunta já existe; seu universo de fio mudo pula perguntas que C6 já considera vivas. **Classe: cura parcial de alcance / frota incompleta.** (`app/ponto/services/cartorio.py:1092`, `app/ponto/services/cartorio.py:768`, `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:163`)

   Isso não atende sozinho ao literal “escritor e frota”. Uma aplicação externa pela porta poderia completar essa parte, mas sua execução não está comprovada nesta revisão. (`app/docs/CORTES.md:381`; aplicação da frota: [não verificado])

3. **A coleta pode derrubar carimbo de marco ainda não vencido.**  
   Cenário: turno noturno 22h–06h, agora 00h40; entrada faltante já tornou a célula `furo`, saída permanece `pendente`; existe pergunta `orfao_14h` carimbada → a coleta encaminha essa pergunta apesar de `hf` não ter vencido. C6 converte sua lâmpada `acesa=None` em falso e retorna viva pelo veredito acusatório do dia; a porta limpa o carimbo antes do teto da saída. **Classe: julgamento antes do pré-requisito temporal / cura incompleta na fonte.** (`app/chamados/services/fabrica_da_celula.py:85`, `app/chamados/services/fabrica_da_celula.py:108`, `app/ponto/services/cartorio.py:703`, `app/chamados/juizes.py:825`, `app/chamados/juizes.py:869`, `app/chamados/juizes.py:1056`, `app/chamados/services/relavra_via.py:231`, `CLAUDE.md:330`)

   A classificação errada já existia em C6; o diff passa a materializá-la nesse escritor. A correção pertence ao juiz registrado. Não atribuo nova cobrança ao diff nesse cenário: `estado` já consultava C6 antes do carimbo. (`app/chamados/juizes.py:1005`, `app/chamados/models.py:2111`)

**Chamadores e custo:** todos chegam à fábrica pelo `julgar_colab`, somente nas datas julgadas naquela passada. (`app/ponto/services/cartorio.py:754`, `app/ponto/services/cartorio.py:768`)

- **Batida:** criação → signal síncrono → julgamento da véspera e data local → fábrica. (`app/ponto/registro_batida.py:157`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:93`)
- **Cron de hora:** a cada dez minutos → `julgar_celula(..., teto_de_hoje=True)` → mesmo caminho. (`app/config/crons.py:348`, `app/ponto/services/vigia_de_hora.py:130`)
- **Lote do cartório:** `processar_cartorio --apply` → `julgar_colab`. (`app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:65`)
- **Lote de perguntas:** não chama `perguntar_pela_celula`; chama `perguntar` depois de construir seu próprio universo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:276`)

O acréscimo estrutural é: **zero consultas adicionais na coleta; uma SELECT travada e transação por candidata, mais consultas de C6; havendo cura, UPDATE, trilha e callback**. O total real por batida continua sem certificação neste diff. (`app/chamados/services/fabrica_da_celula.py:101`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:234`, `app/core/observ.py:133`, `app/chamados/signals.py:202`; total medido: [não verificado])

**Depois da queda:** nesse novo ramo não reabre disputa fechada, não fabrica resposta/batida e não envia push diretamente; limpa três campos e registra evento. O callback examina beco, respeitando C7. Não encontrei efeito indevido demonstrado nesses atos. (`app/chamados/services/fabrica_da_celula.py:146`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:231`, `app/chamados/services/becos.py:33`)

A pergunta órfã pode passar a ser reatada e alcançar emissão de chamado; esse emissor conserva seus gates. O push diário já abrangia disputas abertas, e a cobrança de 48h continua dependendo de ato explícito. (`app/chamados/reconciliador.py:1317`, `app/chamados/reconciliador.py:1411`, `app/chamados/reconciliador.py:743`, `app/chamados/reconciliador.py:770`, `app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `app/chamados/services/cobranca.py:140`)

**Quem cobra os não alcançados:** `materializada_viva` conta o residual, e o painel compara com o esperado zero; não há cura nem alarme desse contador. Outra lacuna concreta é destrancar uma competência com carimbo de outra via: C6 pode voltar a considerá-lo vivo, enquanto a reabertura só limpa `competencia_trancada`. Disputas fechadas continuam sob o dono admin, conforme o corte existente. (`app/chamados/services/vigia_fio.py:395`, `bin/fio_estado.py:72`, `app/chamados/juizes.py:1010`, `app/chamados/services/competencia_trancada.py:244`, `app/docs/CORTES.md:326`)

**Mordida dos testes novos, por análise estática:** execução RED/GREEN [não verificado]. [nao verificado]

| Teste — linha em `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py` | Sem a mudança |
|---|---|
| Julgamento limpa viva — `:88` | RED funcional: carimbo sobra. |
| Morta conserva — `:102` | Verde; controle negativo. | [nao verificado]
| Identidade conserva — `:113` | Verde; controle negativo. | [nao verificado]
| Idempotência — `:122` | RED funcional: exige cura na primeira passada. |
| Fechada conserva — `:135` | Verde; caminho antigo já não a alcança. | [nao verificado]
| Decisão humana posterior — `:148` | RED funcional: instância velha apaga a decisão. |
| Fecha após coleta — `:161` | RED de interface; com assinatura mantida, morde a retirada do filtro. |
| Coleta exclui identidade — `:175` | RED de interface; não prende a integração. |
| Custo constante — `:183` | RED de interface; não certifica a batida. |
| Vigia conta e zera — `:208` | RED de import; mantendo o vigia, morde a integração na contagem final. |
| Vigia exclui identidade/morta — `:215` | RED de import; controle negativo. |

O teste existente do JSON passa a exigir a chave nova, mas não certifica os caminhos de cura acima. (`app/chamados/tests/test_o261_vigia_fio.py:701`, `app/chamados/tests/test_o261_vigia_fio.py:707`)

A varredura AST de vitalidade retornou zero ofensores fora da allowlist; a exceção nova é explícita e compatível com filtro de entrada seguido do juiz. Os testes agora injetam `HOJE/AGORA`; não sustento nova reprovação por relógio solto. Suite com banco, concorrência real e aplicação da frota: **[não verificado]**. (`app/chamados/tests/test_contract_leitores_vitalidade.py:33`, `app/chamados/tests/test_contract_leitores_vitalidade.py:300`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:34`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:83`)

## 30/10 00:21 · sessao d8722e009888 · - · - · origem=Ronald (tmux)

A centralização da elegibilidade satisfaz o padrão do selo de vitalidade: a allowlist é explícita, o filtro só seleciona candidatos e tanto a porta como o vigia consultam `pergunta_viva` antes de decidir. Não encontrei contorno por variável local nesta volta. (`app/chamados/tests/test_contract_leitores_vitalidade.py:16`, `app/chamados/tests/test_contract_leitores_vitalidade.py:33`, `app/chamados/services/relavra_via.py:170`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/vigia_fio.py:398`)

O selo novo de custo é incompleto: mede a porta isolada com perguntas **validadas**, para as quais C6 retorna imediatamente; não mede julgamento por batida nem custo de C6 em candidatas vivas. O selo existente identifica precisamente o risco de voltar a julgar perguntas em cada batida. Classe: **selo sem mordida no caminho real**. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:183`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:191`, `app/chamados/juizes.py:1006`, `app/ponto/tests/test_selo_chokepoint_escrita.py:5`)

Matriz estática dos testes novos; execução não verificada. [não verificado] [nao verificado]

| Linha no arquivo novo | Sem mudança |
|---|---|
| 88 — julgamento limpa viva | RED funcional: carimbo permanece. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`) |
| 102 — morta conserva | VERDE: controle negativo. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:111`) |
| 113 — identidade conserva | VERDE: controle negativo. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:119`) |
| 122 — idempotência | RED funcional: primeira passada precisa curar. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:128`) |
| 135 — fechada conserva | VERDE: caminho anterior não chega à porta. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:143`) |
| 148 — decisão humana posterior | RED funcional: porta antiga escreve sobre instância velha. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:154`) |
| 161 — fecha após coleta | RED de interface: argumento novo; com assinatura preservada, morde a retirada do filtro de fechada. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:170`) |
| 175 — coleta identidade | RED de interface; sem integração, com helper novo mantido, continua verde. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:180`) |
| 183 — custo constante | RED de interface; cenário funcional do chokepoint ausente. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:197`) |
| 208 — vigia conta e zera | RED de import; mantendo o vigia e retirando integração, RED funcional na contagem final. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:209`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:213`) |
| 215 — vigia exclui identidade/morta | RED de import; controle negativo com função existente. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:216`) |

O risco temporal existe na escrita: `venc` é calculado, mas a coleta encaminha todos os motivos; uma saída ainda pendente pode perder o carimbo se outro marco já tornou a célula `furo`, porque C6 retorna viva pelo veredito antes de examinar pendências. A fonte do defeito é anterior, em C6; este diff passa a escrever sua classificação. Não afirmaria nova cobrança: `estado` e `q_vivas` já expunham essa pergunta antes da queda. (`app/chamados/services/fabrica_da_celula.py:85`, `app/chamados/services/fabrica_da_celula.py:101`, `app/chamados/juizes.py:869`, `app/chamados/juizes.py:1054`, `app/chamados/models.py:2111`, `app/chamados/models.py:2256`)

Os testes injetam datas no julgamento; a lavra da fixture ainda usa relógio real por omissão, mas não encontrei dependência do resultado nesse instante que sustente achado de relógio solto. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:34`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:65`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:83`, `app/ponto/portas/celula.py:694`)

## 30/10 00:21 · sessao 30f1e1df3cae · - · - · origem=Ronald (tmux)

Efeitos da cura:

- No novo caminho, disputa fechada fica excluída na coleta e na releitura travada; portanto não há reabertura por essa chamada. A porta limpa somente `materializada_em`, `materializada_por` e `via_resolucao`, preservando resposta/validação e gravando evento. `app/chamados/services/fabrica_da_celula.py:103`, `app/chamados/services/fabrica_da_celula.py:146`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:231`.
- O `save()` agenda o examinador de beco; ele consulta `pode_fechar()` e não fecha a disputa com pergunta viva. Não identifiquei efeito indevido nesse callback. `app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:69`.
- Não existe push direto na porta. O alerta diário já selecionava todas as disputas abertas, independentemente do carimbo. A cobrança com prazo de 48h continua dependendo de `cobrar_colaborador`. `app/chamados/services/relavra_via.py:227`, `app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `app/ponto/management/commands/alertar_disputas_pendentes.py:59`, `app/chamados/services/cobranca.py:140`, `app/chamados/services/cobranca.py:176`.
- Há efeito indireto possível: a pergunta órfã passa a satisfazer a elegibilidade do reatador; no signal da batida o reconciliador roda depois do cartório, podendo costurar/criar chamado ainda no ato. O c2 aplica situação ativa, horizonte, isenção, ausência, protestos cobráveis e veto da célula; não encontrei emissão indevida causada pela limpeza. `app/chamados/reconciliador.py:1317`, `app/ponto/signals.py:207`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:1265`, `app/chamados/reconciliador.py:1411`, `app/chamados/reconciliador.py:723`, `app/chamados/reconciliador.py:743`, `app/chamados/reconciliador.py:753`, `app/chamados/reconciliador.py:770`.

Lacuna concreta de alcance/frota:

**Célula já estável continua com carimbo sem lastro.** Estado: célula passada acusa, pergunta sem resposta está materializada e viva, disputa aberta, insumos iguais à impressão gravada. Saída: cartório pula a célula; o novo desfazedor nunca roda. O lote também não salva: seu conjunto `ja` elimina perguntas existentes, e o caminho do fio mudo pula a pergunta que C6 já reconhece viva. Classe: **cura prospectiva sem cobertura da frota / origem não alcançada**. É incompletude se esta entrega pretende fechar o literal “escritor e frota”; aplicação externa da frota permanece [não verificado]. `app/ponto/services/cartorio.py:1092`, `app/ponto/services/cartorio.py:754`, `app/ponto/services/cartorio.py:769`, `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:129`, `app/chamados/management/commands/disparar_perguntas_competencia.py:163`.

Quem cobra o resíduo: o novo `materializada_viva` conta pelo Q compartilhado e C6; não aplica cura nem alarma. O esperado zero vai ao painel, que compara dinamicamente as chaves do carimbo. As órfãs também aparecem em I8; resolvedoras inválidas carimbadas podem aparecer em I8bc/I8bcx. `app/chamados/services/vigia_fio.py:395`, `app/chamados/services/vigia_fio.py:399`, `app/chamados/services/vigia_fio.py:404`, `bin/fio_estado.py:69`, `bin/fio_estado.py:105`, `app/chamados/management/commands/auditar_invariantes_chamados.py:854`, `app/chamados/management/commands/auditar_invariantes_chamados.py:904`, `app/chamados/management/commands/auditar_invariantes_chamados.py:940`.

Outro motivo de mudança do juiz: C6 lê tranca dinamicamente, enquanto `reabrir_periodo` só desfaz carimbos da via `competencia_trancada`, dentro de `cartorio.calado()`. Um carimbo de outra via pode voltar a ser vivo ao destrancar sem passar pela nova cura; será medido pelo novo vigia e dependerá de julgamento futuro/aplicação pela porta. `app/chamados/juizes.py:1008`, `app/chamados/juizes.py:902`, `app/ponto/services/fechamento.py:1413`, `app/chamados/services/competencia_trancada.py:243`, `app/chamados/services/competencia_trancada.py:244`.

## 30/10 00:20 · sessao bbae8a8379e4 · - · - · origem=Ronald (tmux)

Achado procedente: **o custo novo ainda não está selado no caminho da batida**. A coleta reaproveita uma query, mas cada candidata executa `atomic`, releitura travada e C6; cada cura também grava pergunta e trilha (`app/chamados/services/fabrica_da_celula.py:101`, `app/chamados/services/fabrica_da_celula.py:145`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:225`, `app/chamados/services/relavra_via.py:234`).

Cenário: dia rejulgado com N perguntas carimbadas elegíveis → a batida faz trabalho proporcional a N e, havendo cura, registra um callback adicional por pergunta. O `save` dispara `_becos_post_save_pergunta`, que registra `transaction.on_commit`; o selo existente cobra também a contagem exata desses registros (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/ponto/tests/test_selo_chokepoint_escrita.py:169`, `app/ponto/tests/test_selo_chokepoint_escrita.py:207`). **Classe: selo sem mordida / custo no chokepoint.**

O teste novo mede diretamente a porta, usando perguntas já validadas; C6 retorna imediatamente nesse estado. Ele exige `custo(3)==3*custo(1)`, portanto certifica crescimento linear, sem medir a batida, uma cura, sua trilha ou seus callbacks (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:183`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:191`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:197`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:200`, `app/chamados/juizes.py:1006`). As três perguntas são montadas para colaboradores distintos, pois cada `_cenario` cria outro colaborador (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:55`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:57`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:192`).

As fixtures do selo existente criam perguntas sem `materializada_em`, logo não exercitam a cura nova (`app/ponto/tests/test_selo_chokepoint_escrita.py:226`, `app/ponto/tests/test_selo_chokepoint_escrita.py:237`). A cerca declara expressamente que impede reintroduzir o juiz por pergunta na marcação; a própria cadeia de signals documenta uma tentativa anterior removida pelo custo nesse caminho (`app/ponto/tests/test_selo_chokepoint_escrita.py:5`, `app/ponto/signals.py:94`).

Chamadores e alcance:

- **Batida:** `registrar_batida` cria a batida; o signal síncrono julga véspera e data local, chegando à fábrica somente para datas julgadas na passada (`app/ponto/registro_batida.py:157`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:92`, `app/ponto/services/cartorio.py:768`).
- **Cron de hora:** roda a cada dez minutos e chama `julgar_celula(..., teto_de_hoje=True)`; este encaminha ao mesmo `julgar_colab` (`app/config/crons.py:348`, `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/cartorio.py:1560`).
- **Lote do cartório:** cron `processar_cartorio --apply` reúne células passadas por colaborador e chama `julgar_colab` (`app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`).
- **Lote de perguntas:** `disparar_perguntas_competencia` tem caminho próprio; seu primeiro universo continua retirando todo fato com pergunta existente, portanto não resolve o passivo carimbado por esse universo (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:129`).

Não executei testes nem medi valores absolutos de queries, em respeito à restrição de somente leitura. [não verificado] [nao verificado]

## 30/10 00:07 · sessao 4553ca97db0b · mariaB · verificador · origem=Code

**Há 1 achado procedente: o custo novo por candidata no caminho da batida ficou sem selo.** A coleta reaproveita a consulta existente, mas a chamada seguinte acrescenta releitura travada e julgamento por pergunta; portanto, o comentário “0 query a mais na batida” não descreve o caminho completo. (`app/chamados/services/fabrica_da_celula.py:138`, `app/chamados/services/fabrica_da_celula.py:142`, `app/chamados/services/relavra_via.py:203`)

1. **Cenário:** célula efetivamente rejulgada e acusadora → disputa aberta → pergunta materializada, via não identitária, **já validada** → coleta encaminha a pergunta → porta abre transação, relê com trava e C6 imediatamente responde morta → nenhuma mudança de estado, mas custo adicional por candidata. **Classe:** custo no chokepoint / selo sem mordida. (`app/chamados/services/fabrica_da_celula.py:86`, `app/chamados/services/fabrica_da_celula.py:104`, `app/chamados/services/relavra_via.py:202`, `app/chamados/juizes.py:1006`)

   O selo existente cria suas três perguntas sem `materializada_em`; o teste novo de pergunta morta verifica apenas preservação do carimbo. Nenhum desses casos mede esse ramo novo com carimbadas na batida. Isso permite crescimento por candidata passar sem fiscalização do custo exigida pelo próprio selo. **Estouro numérico dos tetos: [não verificado].** (`app/ponto/tests/test_selo_chokepoint_escrita.py:54`, `app/ponto/tests/test_selo_chokepoint_escrita.py:237`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:102`)

Os caminhos e custos ficam assim:

| Caminho | Como chega e custo novo |
|---|---|
| Cron do cartório | `processar_cartorio --apply` → `julgar_colab` → `perguntar_pela_celula`, somente nas datas julgadas. (`app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:754`, `app/ponto/services/cartorio.py:770`) |
| Signal da batida | `post_save` → `_julgar_dias` → `julgar_celula` → `julgar_colab`; considera dia anterior e dia da batida. (`app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1560`) |
| Vigia de hora | Também entra por `julgar_celula`, com teto de hoje. (`app/ponto/services/vigia_de_hora.py:130`) |
| Lote de perguntas | Não chama o wrapper: monta seu universo e chama `perguntar`, sob transação externa. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:276`, `app/chamados/management/commands/disparar_perguntas_competencia.py:295`) |

A coleta acrescenta **zero consultas**, mas cada candidata acrescenta **um `SELECT FOR UPDATE`**, além dos comandos transacionais. Nas não validadas, C6 pode carregar colaborador, empresa e tranca: a releitura não traz `disputa__colaborador`. Se houver cura, entram também `UPDATE`, auditoria e callback de beco. Totais medidos por cenário: [não verificado]. (`app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/relavra_via.py:203`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:896`, `app/chamados/juizes.py:900`, `app/chamados/services/relavra_via.py:221`, `app/chamados/signals.py:202`)

Depois da queda, **não identifiquei efeito indevido demonstrável**: a porta limpa apenas materialização, autor e via, preservando resposta e validação; o julgamento passa `reabrir_fechada=False` e a disputa é relida travada antes da decisão. (`app/chamados/services/relavra_via.py:203`, `app/chamados/services/relavra_via.py:210`, `app/chamados/services/relavra_via.py:218`, `app/chamados/services/fabrica_da_celula.py:142`)

Não há emissão direta de push ou chamado nessa cadeia. O `save` agenda fechamento de beco, cuja classificação consulta `pode_fechar`; o app já mostrava perguntas C6-vivas mesmo carimbadas. A queda permite futura entrada no reatador, mas emissão continua passando pelos gates existentes; materialização exige resposta e validação. (`app/chamados/signals.py:191`, `app/chamados/services/becos.py:33`, `app/chamados/services/pergunta_no_app.py:39`, `app/chamados/reconciliador.py:1317`, `app/chamados/reconciliador.py:743`, `app/chamados/reconciliador.py:768`, `app/chamados/services/materializacao.py:1286`)

**Há resíduos fora da cura:** célula não rejulgada; retornos antes da coleta por trabalho, situação, data ou veredito; e mudanças de C6 por tranca ou lastro sem nova lavra. Eles continuam alcançáveis pelo `materializada_viva`, que não repete esses filtros da célula, publica esperado zero e é cobrado pelo painel. Disputas fechadas permanecem no contador separado, com dono admin. Não reabro o achado descartado sobre concorde provisória com carimbo legado. Aplicação efetiva do passivo: [não verificado]. (`app/ponto/services/cartorio.py:768`, `app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/juizes.py:1008`, `app/chamados/juizes.py:1066`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/management/commands/vigiar_fio_morto.py:197`, `bin/fio_estado.py:71`, `app/chamados/services/vigia_fio.py:341`)

A mordida dos testes, por leitura estática, é esta; execução RED/GREEN: [não verificado]. [nao verificado]

| Teste novo | Sem a mudança | [nao verificado]
|---|---|
| Julgamento desfaz carimbo vivo | Vermelho funcional no carimbo ainda preenchido. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`) |
| Morta mantém carimbo | Já verde; controle negativo. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:102`) |
| Identidade preservada | Já verde; controle negativo. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:113`) |
| Duas passadas idempotentes | Vermelho na exigência de cura da primeira passada. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:128`) |
| Disputa previamente fechada | Já verde no wrapper anterior. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:135`) |
| Decisão humana após leitura | Vermelho funcional: a versão anterior escrevia sobre a instância velha. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:155`, `app/chamados/services/relavra_via.py:199`) |
| Disputa fecha após coleta | Vermelho de API sem o argumento novo; simulação sequencial, sem concorrência real. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:169`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:172`) |
| Coleta exclui identidade | Vermelho de API sem `carimbadas`; não mede queries. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:182`) |
| Vigia conta e zera | Vermelho de importação sem o vigia; mantendo só o vigia, remover a cura quebra a segunda contagem. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:191`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:195`) |
| Vigia exclui identidade/morta | Vermelho de importação; com o vigia presente, controle negativo. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:198`) |

O teste alterado do carimbo cobra a nova chave e seu esperado. A allowlist continua permitindo filtro de entrada seguido do juiz canônico; não encontrei ali troca da pergunta do contrato. (`app/chamados/tests/test_o261_vigia_fio.py:701`, `app/chamados/tests/test_o261_vigia_fio.py:707`, `app/chamados/tests/test_contract_leitores_vitalidade.py:33`, `app/chamados/services/vigia_fio.py:405`)

## 30/10 00:05 · sessao a6fa894c36bb · - · - · origem=Ronald (tmux)

Sem achado procedente nos efeitos e na cobertura residual examinados; a queda só ocorre após C6 afirmar vida e preserva resposta/validação. (`app/chamados/services/relavra_via.py:189`, `:214`, `:218`)

- **Disputa:** julgamento passa `reabrir_fechada=False`; a porta relê a disputa travada e deixa fechadas intocadas, inclusive fechamento ocorrido após coleta. (`app/chamados/services/fabrica_da_celula.py:142`; `app/chamados/services/relavra_via.py:203`, `:210`)
- **Push/chamado:** a nova chamada limpa apenas três campos da pergunta e grava auditoria; seu `post_save` agenda o fechamento de beco, cuja decisão exige ausência de pergunta viva. Não encontrei emissão direta de push ou chamado nessa cadeia. (`app/chamados/services/relavra_via.py:218`; `app/chamados/signals.py:191`; `app/chamados/services/becos.py:25`; `app/chamados/models.py:1434`)
- **Cobrança e visibilidade:** app, badge e estado já liam C6 com o carimbo presente; uma pergunta C6-viva já aparecia como pendente/respondida antes dessa limpeza. Portanto, limpar carimbo de marco ainda pendente não prova, sozinho, nova cobrança antecipada introduzida pelo diff. (`app/chamados/services/pergunta_no_app.py:39`; `app/chamados/models.py:1381`; `app/chamados/models.py:2111`)
- **Elegibilidade indireta:** queda permite entrada no reatador de órfãs; eventual emissão passa pelos gates de horizonte, isenção, ausência, protesto cobrável e veto da célula. Materialização exige resposta e validação, ausentes no cenário B literal. (`app/chamados/reconciliador.py:1317`; `:743`; `:753`; `:768`; `app/chamados/services/materializacao.py:1286`)
- **Resíduos:** células não rejulgadas e retornos anteriores à coleta — não trabalha, colaborador inativo, dia futuro, veredito não acusador — permanecem fora da cura; mudanças da tranca ou do lastro também podem alterar C6 sem passar por essa coleta. (`app/chamados/services/fabrica_da_celula.py:78`; `app/chamados/juizes.py:1008`; `:1066`)
- **Quem cobra resíduos:** `materializada_viva` não aplica esses filtros da célula: conta todas as carimbadas não identitárias em disputa aberta que C6 considera vivas; grava esperado zero e o painel compara a chave automaticamente. Fechadas têm contador separado e dono admin. (`app/chamados/services/vigia_fio.py:400`; `app/chamados/management/commands/vigiar_fio_morto.py:91`; `:197`; `bin/fio_estado.py:71`; `:105`; `app/chamados/services/vigia_fio.py:341`)

Execução efetiva do passivo e efeitos em produção: [não verificado].

## 30/10 00:05 · sessao d3bfe486e8d7 · - · - · origem=Ronald (tmux)

Achado procedente: **o custo novo por carimbo no caminho da batida não está selado; “0 query a mais na batida” é falso para candidata materializada**. A coleta reaproveita a query, mas cada candidata chama a porta (`app/chamados/services/fabrica_da_celula.py:104`, `:140`, `:142`), que abre `atomic` e executa outro `SELECT FOR UPDATE` por pergunta (`app/chamados/services/relavra_via.py:198`, `:202`, `:203`).

Cenário concreto: célula acusando, disputa aberta, pergunta materializada por via não identidade **já validada** → a coleta a encaminha mesmo assim; a porta paga a transação e a releitura, e C6 imediatamente devolve morta, mantendo tudo igual (`app/chamados/services/fabrica_da_celula.py:104`; `app/chamados/services/relavra_via.py:202`; `app/chamados/juizes.py:1006`). Em `TestCase` sob transação, isso representa pelo menos `SAVEPOINT + SELECT + RELEASE` por candidata, antes de qualquer cura [não verificado por execução]. Para candidata não validada, a releitura não traz `disputa__colaborador`; C6 ainda carrega colaborador, empresa e tranca em instâncias frescas (`app/chamados/services/relavra_via.py:204`; `app/chamados/juizes.py:888`, `:896`, `:900`). Classe: **selo sem mordida / custo no chokepoint**.

O selo atual exige custo apertado e justificativa para aumento, mas sua fixture de três perguntas não tem `materializada_em`; portanto não entra no novo ramo (`app/ponto/tests/test_selo_chokepoint_escrita.py:54`, `:237`, `:258`). O teste novo de pergunta morta verifica apenas preservação do carimbo, sem contar queries (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:102`). Não afirmo estouro dos tetos existentes: isso exigiria execução [não verificado].

Chamadores/caminhos:

- **Chamada direta única:** `julgar_colab`, com `apply_`, apenas nas datas presentes em `out['cods']` (`app/ponto/services/cartorio.py:754`, `:758`, `:768`).
- **Cron:** `processar_cartorio --apply` → comando → `julgar_colab` (`app/config/crons.py:597`; `app/ponto/management/commands/processar_cartorio.py:59`, `:65`).
- **Batida síncrona:** `post_save` → `_julgar_dias` → `julgar_celula` → `julgar_colab`; considera dia anterior e dia da batida (`app/ponto/signals.py:131`, `:135`, `:85`, `:93`, `:108`; `app/ponto/services/cartorio.py:1560`).
- **Vigia de hora:** também chega por `julgar_celula` (`app/ponto/services/vigia_de_hora.py:130`).
- **Lote `disparar_perguntas_competencia`:** não usa `perguntar_pela_celula`; monta seu universo e chama `perguntar`, dentro de transação externa (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `:124`, `:276`, `:288`, `:295`). Essa porta compartilhada agora também paga a releitura adicional de cada materializada selecionada (`app/chamados/services/fabrica_da_celula.py:116`; `app/chamados/services/relavra_via.py:203`).

A nova porta trava pergunta e disputa na mesma consulta, e o ramo do julgamento revalida disputa fechada antes de escrever (`app/chamados/services/relavra_via.py:203`, `:210`). Não estabeleci um ciclo concreto de deadlock contra outro escritor [não verificado].

## 30/10 00:05 · sessao 6e42cd0f7cb9 · - · - · origem=Ronald (tmux)

Matriz estática; execução RED/verde [não verificado]. Referências abaixo ao arquivo `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py`.

| Teste | Sem a mudança | [nao verificado]
|---|---|
| `o_julgamento_desfaz...` | Vermelho funcional: carimbo continua preenchido no assert da linha 98. (`:88`, `:98`) |
| `pergunta_...MORTA...` | Já verde: controle negativo, carimbo permanece. (`:102`, `:111`) | [nao verificado]
| `via_de_identidade...` | Já verde: controle negativo, wrapper antigo não toca a pergunta existente. (`:113`, `:119`) | [nao verificado]
| `julgar_duas_vezes...` | Vermelho funcional na pré-condição de cura da primeira passada. (`:122`, `:128`) |
| `disputa_FECHADA...` | Já verde: wrapper antigo também deixa o carimbo e a disputa intactos. (`:135`, `:146`) | [nao verificado]
| `a_porta_nao_apaga_decisao...` | Vermelho funcional: função antiga usa a instância velha e apaga `admin_decidiu`. (`:149`, `:155`, `:159`; diff fornecido) |
| `disputa_que_FECHA...` | Vermelho de API: argumento `reabrir_fechada` inexistente antes; simula mudança sequencial, sem concorrência real. (`:162`, `:169`, `:172`; diff fornecido) |
| `a_coleta_nao_encaminha...` | Vermelho de API: argumento `carimbadas` inexistente antes; não mede custo. (`:177`, `:182`; diff fornecido) |
| `o_vigia_conta_e_zera...` | Vermelho de importação sem a nova função; removendo apenas a cura, vermelho funcional na segunda contagem. (`:190`, `:191`, `:195`) |
| `vigia_nao_conta...` | Vermelho de importação; com o vigia presente, controle negativo. (`:197`, `:198`, `:201`) |

O teste alterado do carimbo fica vermelho por importação ou ausência da nova chave; ele confere conteúdo e esperado publicados. (`app/chamados/tests/test_o261_vigia_fio.py:689`, `:701`, `:707`)

A alteração da allowlist não troca a pergunta do contrato: permite filtro de entrada por carimbo seguido do juiz canônico, que o código efetivamente consulta. (`app/chamados/tests/test_contract_leitores_vitalidade.py:33`, `:300`; `app/chamados/services/vigia_fio.py:400`, `:405`)

Achado de cobertura: **selo sem mordida no novo ramo de custo**. Uma batida que encontra pergunta viva já carimbada entra na nova porta, relê e trava a linha, consulta o juiz e salva; o save registra mais um `on_commit`. (`app/chamados/services/fabrica_da_celula.py:141`; `app/chamados/services/relavra_via.py:202`, `:214`, `:221`; `app/chamados/signals.py:191`, `:202`)

Os novos testes chamam diretamente o wrapper e não capturam queries nem callbacks; as fixtures do selo existente não preparam esse estado carimbado. Assim, crescimento de custo nesse ramo pode passar pelos selos atuais; estouro numérico de teto [não verificado]. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:75`, `:81`; `app/ponto/tests/test_selo_chokepoint_escrita.py:235`, `:321`, `:361`)

## 29/10 23:58 · sessao 25e3a63159a1 · mariaB · verificador · origem=Code

Encontrei **3 achados**. A análise é estática; execução da suíte, contagem dinâmica de queries e reprodução concorrente: **[nao verificado]**. [nao verificado]

1. **O custo novo na batida não é zero.**  
   **Local:** `app/chamados/services/fabrica_da_celula.py:103` e `app/chamados/services/relavra_via.py:202`.

   **Cenário:** célula continua acusando um marco; existe outra pergunta do dia carimbada, em disputa aberta, inclusive com `admin_decidiu`. A coleta a encaminha; a porta abre `atomic` e faz `SELECT FOR UPDATE` **antes** de excluir identidade. Portanto, com *k* candidatas há *k* releituras adicionais, além do custo transacional; cada cura acrescenta escrita, auditoria e callback. Reutilizar a query do `ja` elimina apenas o custo da coleta. (`fabrica_da_celula.py:98–103`; `relavra_via.py:202–222`; `app/chamados/signals.py:191–202`.)

   O selo existente cobra queries e quantidade de callbacks, mas sua fixture com três perguntas não cria carimbos e não exercita esse ramo. **Classe:** custo no chokepoint; selo sem mordida para o estado acrescentado. (`app/ponto/tests/test_selo_chokepoint_escrita.py:169–218`, `:226–240`.)

2. **A restrição “só disputa aberta” pode ser perdida entre coleta e escrita.**  
   **Local:** `app/chamados/services/relavra_via.py:203` e `:230`.

   **Cenário concorrente concreto:**

   - B calcula que pode fechar a disputa enquanto a célula ainda absolve. (`app/chamados/models.py:1481–1489`.)
   - A rejulga a célula como acusadora e coleta a pergunta enquanto a disputa ainda está aberta. (`app/ponto/services/cartorio.py:754–770`; `fabrica_da_celula.py:98–103`.)
   - B grava o fechamento normal; esse fechamento não carimba identidade. (`models.py:1490–1502`.)
   - A relê e trava somente a pergunta, encontra C6 vivo e **reabre a disputa agora fechada**. (`relavra_via.py:203–231`; `app/chamados/juizes.py:1054–1057`.)

   Isso contraria o recorte declarado pelo próprio diff e pode recolocar a disputa no push diário, que seleciona abertas. **Classe:** ordem de trava/transação; condição de entrada não preservada na escrita. (`fabrica_da_celula.py:134–135`; `app/ponto/management/commands/alertar_disputas_pendentes.py:33–64`.) Reprodução concorrente: **[nao verificado]**.

3. **A cura herda filtros de emissão que excluem carimbos que C6 considera sem lastro.**  
   **Local:** `app/chamados/services/fabrica_da_celula.py:102`.

   **Cenário:** pergunta de saída sem resposta, carimbada por `legado_sem_lastro`, sem plantio nem destino que a sustente; célula `concorde` provisória, com saída pendente. C6 não aceita o veredito provisório como absolvição e pode manter essa pergunta viva pelo último degrau. Porém `motivos_a_perguntar` retorna pelo veredito não acusador **antes de preencher `carimbadas`**: mesmo passando pelo julgamento, o carimbo permanece. (`app/chamados/models.py:2562–2563`; `app/chamados/juizes.py:675–715`, `:1058–1083`; `fabrica_da_celula.py:86–103`.)

   A distinção entre `concorde` provisório e fato encerrado já está selada; a coleta nova não a acompanha. **Classe:** incompletude contra o juiz existente e teto temporal. (`app/chamados/tests/test_o261_pergunta_viva_teto.py:43–60`.) Reprodução desse cenário com carimbo: **[nao verificado]**.

Os caminhos pedidos ficam assim:

| Caminho | Encadeamento |
|---|---|
| Batida | `post_save`/`post_delete` → `_julgar_dias`, em D−1 e D → `julgar_celula` → `julgar_colab` → `perguntar_pela_celula`. (`app/ponto/signals.py:85–112`, `:131–140`; `app/ponto/services/cartorio.py:1560`, `:770`.) |
| Cron diário | `processar_cartorio --apply` → `julgar_colab` → mesma fábrica. (`app/config/crons.py:597`; `app/ponto/management/commands/processar_cartorio.py:59–65`.) |
| Vigia de hora | Chama `julgar_celula(..., teto_de_hoje=True)` e chega à mesma fábrica. (`app/ponto/services/vigia_de_hora.py:130`; `app/ponto/services/cartorio.py:770`.) |
| Lote | Chama `perguntar`, sem passar por `perguntar_pela_celula`; o dedup anterior normalmente exclui a pergunta já existente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124–129`, `:276–289`.) |

Depois da queda normal, são limpos os três campos e gravada auditoria; não há chamada direta de push ou criação de chamado nessa porta. O `post_save` agenda a verificação de beco, cujo fechamento respeita C6. A reabertura ocorre no ramo de disputa fechada descrito no achado 2. (`app/chamados/services/relavra_via.py:213–231`; `app/chamados/signals.py:191–202`; `app/chamados/services/becos.py:25–33`, `:67–70`.)

As não alcançadas **têm cobrança no painel**, embora não tenham cura automática garantida: `materializada_viva` varre abertas sem exigir célula ou rejulgamento; o comando carimba esperado zero e `fio_estado` publica divergências. Destrancar a competência também pode devolver vida pelo juiz sem rejulgar a célula. (`app/chamados/services/vigia_fio.py:400–405`; `app/chamados/management/commands/vigiar_fio_morto.py:197–203`; `bin/fio_estado.py:69–73`, `:105–108`; `app/chamados/juizes.py:875–907`.)

Quanto ao RED de **cada teste novo**, a previsão estática é: [nao verificado]

| Teste | Sem a mudança correspondente | [nao verificado]
|---|---|
| Cura principal | Vermelho no carimbo preservado. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`.) |
| Morta mantém carimbo | Já verde; controle negativo. (`:102–111`.) | [nao verificado]
| Identidade preservada | Já verde; controle negativo. (`:113–120`.) | [nao verificado]
| Idempotência | Vermelho na exigência de primeira cura; evita idempotência vazia. (`:128`.) |
| Fechada não reabre | Já verde no anterior; não cobre fechamento entre coleta e escrita. (`:135–147`.) | [nao verificado]
| Decisão humana após leitura | Vermelho esperado: a implementação anterior usava a instância velha. (`:149–160`.) |
| Vigia conta e zera | Sem contador: erro de importação; mantendo só o contador novo, falha após julgamento. (`:167–172`.) |
| Vigia ignora identidade/morta | Sem contador: erro de importação; com contador, é controle independente da cura. (`:174–178`.) |
| Teste alterado do carimbo | Cobra as novas chaves, mas suas fixtures não criam materializada viva positiva. (`app/chamados/tests/test_o261_vigia_fio.py:690–710`.) |

Esses testes chamam o helper diretamente e não executam os callbacks `on_commit`; não demonstram o custo da batida carimbada nem os efeitos após commit. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:39`, `:81–83`; `app/ponto/tests/test_selo_chokepoint_escrita.py:169–170`.) RED/verde efetivamente executado: **[nao verificado]**.

## 29/10 23:57 · sessao 811b7882f6ff · - · - · origem=Ronald (tmux)

Achado sustentado por interleaving: **a exclusão de disputa fechada não é revalidada na porta**. A candidata é coletada aberta em `app/chamados/services/fabrica_da_celula.py:98-103`; a releitura trava somente a pergunta em `app/chamados/services/relavra_via.py:203-204`, aceita a disputa agora fechada e a reabre em `:229-231`.

Cenário concreto: B calcula `vivas=[]` ao fechar normalmente quando a célula ainda absolvia (`app/chamados/models.py:1481-1489`); A grava julgamento acusador e coleta a candidata ainda aberta; B grava fechamento (`:1490-1494`); A relê pergunta carimbada e disputa fechada, obtém viva pela célula acusadora (`app/chamados/juizes.py:1054-1057`) e reabre. O fechamento normal não ganha identidade, reservada às exceções (`app/chamados/models.py:1499-1502`). Classe: **ordem de trava/transação; guarda aberta fora da unidade da escrita**. Interleaving deduzido, reprodução concorrente não executada [não verificado].

A reabertura indevida entra no push diário: seleção de todas abertas em `app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`, push ao colaborador em `:48-64`, supervisão em `:103-108`; cron às 08h em `app/config/crons.py:719`.

No caminho normal, limpar o carimbo grava três campos e auditoria (`app/chamados/services/relavra_via.py:213-222`), sem push direto; o `post_save` agenda fechamento de beco (`app/chamados/signals.py:191-202`), que respeita C6 e não fecha pergunta viva (`app/chamados/services/becos.py:25-33`). A cobrança manual já via essa pergunta viva antes da limpeza (`app/chamados/services/aviso_pendencia.py:7-13`).

Há cobrança das não alcançadas: `materializada_viva` inclui abertas com ou sem célula e sem exigir rejulgamento (`app/chamados/services/vigia_fio.py:400-405`); o comando grava contador e esperado (`app/chamados/management/commands/vigiar_fio_morto.py:197-203`), e o ESTADO publica divergência (`bin/fio_estado.py:69-73,105-108`). O lote não oferece recuperação: deduplica perguntas existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:124-129`) e pula fio que C6 já dá por vivo (`:163-164`). Destrancar também pode mudar C6 sem julgamento da célula (`app/chamados/juizes.py:875-907`).

## 29/10 23:55 · sessao fdfed644648e · - · - · origem=Ronald (tmux)

Caminhos e custo confirmados por leitura estática:

- **Batida:** `post_save`/`post_delete` → `_julgar_dias` → `julgar_celula`, nos dias d−1 e d → `julgar_colab` → `perguntar_pela_celula`. Referências: `app/ponto/signals.py:85`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/services/cartorio.py:1560`, `app/ponto/services/cartorio.py:770`.
- **Cron diário:** `processar_cartorio --apply` → laço de colaboradores → `julgar_colab` → mesma fábrica. Referências: `app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`.
- **Vigia de hora:** roda a cada dez minutos e chama `julgar_celula(..., teto_de_hoje=True)` → mesma fábrica. Referências: `app/config/crons.py:348`, `app/ponto/services/vigia_de_hora.py:130`.
- **Lote:** não chama `perguntar_pela_celula`; chama `perguntar`. Seu dedup exclui perguntas já existentes antes disso, portanto não recupera normalmente o carimbo existente. Referências: `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`.

**A afirmação “0 query a mais na batida” só vale sem candidatas.** A coleta reaproveita uma query, mas cada candidata agora passa por `atomic` e um `SELECT FOR UPDATE`, inclusive identidade que será poupada. Cada cura acrescenta `UPDATE`, trilha e um registro de `on_commit`; o callback consulta/fecha becos após o commit. Referências: `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/relavra_via.py:202`, `app/chamados/services/relavra_via.py:216`, `app/chamados/signals.py:191`.

**Lacuna do selo:** o cenário explícito com três perguntas não cria carimbos, portanto não exercita esse ramo novo. O selo cobra queries e quantidade exata de callbacks, mas não inclui cenário explícito de batida com carimbo sem lastro. Referências: `app/ponto/tests/test_selo_chokepoint_escrita.py:226`, `app/ponto/tests/test_selo_chokepoint_escrita.py:237`, `app/ponto/tests/test_selo_chokepoint_escrita.py:207`. Quantificação dinâmica: **[não verificado]**.

**Incompletude possível para avaliar na consolidação:** a coleta fica depois dos retornos por `trabalha`, situação e veredito não acusador; entretanto C6 pode continuar dando viva uma pergunta com resposta divergente, ou sem resposta quando o veredito é provisório e não existe testemunha. Nesse caso a fábrica não oferece a pergunta à porta, embora o vigia possa contá-la. Referências: `app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/services/fabrica_da_celula.py:86`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1063`, `app/chamados/juizes.py:1083`. A pertinência do caso respondido ao escopo literal B exige distinguir a cura geral da porta do pedido “sem resposta”.

## 29/10 23:55 · sessao ffedafeaa28e · - · - · origem=Ronald (tmux)

Análise estática; RED/verde executado: **[não verificado]**. [nao verificado]

| Teste novo | Sem mudança de produção | [nao verificado]
|---|---|
| Cura principal | Vermelho esperado no carimbo preservado: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`. |
| Morta mantém carimbo | Já verde: controle negativo; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:102`. |
| Identidade preservada | Já verde: controle negativo; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:113`. |
| Idempotência | Vermelho esperado na pré-condição da primeira cura; não passa por ausência de escrita: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:128`. |
| Fechada permanece fechada | Já verde no anterior, que não encaminhava candidata alguma à porta: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:135`. |
| Instância velha versus decisão humana nova | Vermelho esperado: código anterior sobrescreveria `admin_decidiu`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:155`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:159`. |
| Vigia conta e zera | Sem vigia: ImportError; mantendo apenas o contador, vermelho esperado após julgamento: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:168`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:172`. |
| Vigia ignora identidade/morta | Sem vigia: ImportError; mantendo contador, controle independente da cura: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:174`. |

O teste alterado do carimbo cobra a chave nova tanto em `censo` como em `esperado`, mas suas fixtures não criam materializada viva positiva; essa cobertura fica no novo arquivo: `app/chamados/tests/test_o261_vigia_fio.py:690`, `app/chamados/tests/test_o261_vigia_fio.py:701`, `app/chamados/tests/test_o261_vigia_fio.py:707`.

A allowlist nova não constitui RED novo; o contrato existente filtra ofensores pelo conjunto autorizado: `app/chamados/tests/test_contract_leitores_vitalidade.py:35`, `app/chamados/tests/test_contract_leitores_vitalidade.py:299`.

**Limite material dos selos novos:** todos herdam `TestCase`, usam o helper diretamente e não executam os callbacks `on_commit`; portanto não demonstram comportamento após commit, disputa fechando concorrentemente ou custo do caminho real da batida com candidata carimbada. Referências: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:30`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:39`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:81`, `app/chamados/signals.py:191`, `app/chamados/signals.py:202`. O selo preexistente conta consultas e registros de callbacks, exigindo cenário que efetivamente passe por essa cura: `app/ponto/tests/test_selo_chokepoint_escrita.py:169`, `app/ponto/tests/test_selo_chokepoint_escrita.py:207`.

## 29/10 23:44 · sessao 1b05e128f7e5 · o261c · leitura · origem=Code

**Sim: há recusas que decorrem de filtros divergentes da regra de costura.** O caso mais direto é o histórico: ele conserva o mapa de **motivo principal**, enquanto o juiz de motivos atendidos permite que o chamado do dia responda pelos quatro motivos. Já o `False` de c1 diante de pergunta VIVA é deliberado e correto. (`app/chamados/reconciliador.py:1391–1394`; `app/chamados/juizes.py:1589–1600`; `app/chamados/reconciliador.py:398–400`)

Não é possível distribuir os ~140 casos entre essas causas apenas pela árvore; a incidência de cada ramo nessa amostra permanece **[não verificado]**.

**1. Fluxo externo e retornos finais**

`reatar_pergunta_orfa` tem quatro pontos explícitos de `return False`:

| Caminho | Condição | Retorno final |
|---|---|---|
| Derivador não encontrou destino; falta data | Nem `p.data_evento` nem `contexto[p.motivo]['data']` fornecem data | `app/chamados/reconciliador.py:1382–1383` |
| Derivador não encontrou destino; cardinalidade do histórico | `hist >= 2`; ou `hist == 0` e **ambos** c1/c2 recusaram | `app/chamados/reconciliador.py:1395–1416` |
| Histórico único; costura recusada | `reapontar_resolvedora(...hist[0]...)` devolveu `_ok=False` | `app/chamados/reconciliador.py:1433–1436` |
| Candidato vivo; costura recusada | `reapontar_resolvedora(...ch...)` devolveu `_ok=False` | `app/chamados/reconciliador.py:1445–1448` |

Portanto, `None` do derivador **não encerra a tentativa**. Mas histórico ambíguo encerra sem tentar c1/c2; candidato vivo recusado pelo juiz também encerra sem tentar os demais caminhos. (`app/chamados/reconciliador.py:1371–1416`, `:1445–1449`)

Exceções não são convertidas em `False` por essa função: o laço externo as captura e registra separadamente. (`app/chamados/reconciliador.py:1338–1343`)

**2. `_derivar_chamado_destino`: quando retorna `(None, None)`**

A implementação está em `services/disputa_emissao.py`, reexportada por `models.py`. O reatamento passa a data da própria pergunta; o contexto da disputa é fallback. (`app/chamados/models.py:2778`; `app/chamados/reconciliador.py:1369–1370`; `app/chamados/services/disputa_emissao.py:174–179`)

| Filtro | Como elimina o candidato |
|---|---|
| Colaborador e estado | Busca somente chamados do colaborador da disputa com `status_local in VIVOS`; exclui categoria exatamente `disputa_supervisao`. `VIVOS = aberto, em_analise, registrado`. (`app/chamados/services/disputa_emissao.py:181–184`; `app/chamados/catalogo/motor.py:24`) |
| Categoria/subcategoria | Normaliza ambos e exige `motivo in motivos_atendidos(cat, sub)`. Se nenhum rótulo pertence aos mapas, descarta. **Não há filtro direto por `modulo_origem`**, nem consulta à lâmpada nessa classificação. (`app/chamados/services/disputa_emissao.py:186–196`; `app/chamados/juizes.py:1595–1600`) |
| Candidato ocupado | Descarta se existe pergunta apontando para aquele chamado, do **mesmo motivo**, com `materializada_em IS NULL`. Não exige disputa aberta, mesma data ou `pergunta_viva=True`. (`app/chamados/services/disputa_emissao.py:209–212`) |
| Dia diferente | Obtém `dia_do_fato(ch)`; quando as duas datas existem, qualquer diferença textual elimina o candidato. Se alguma data falta, esse filtro não elimina. (`app/chamados/services/disputa_emissao.py:214–218`) |
| Nenhum candidato ou ambiguidade residual | Depois do desempate, exige exatamente um; caso contrário retorna `(None, None)` em **227**. (`app/chamados/services/disputa_emissao.py:219–227`) |

`motivos_atendidos` devolve **os quatro motivos** — `orfao_14h`, `saida_sem_entrada`, `intervalo_saida`, `intervalo_volta` — quando categoria **ou** subcategoria está no mapa. Portanto, no derivador vivo, um chamado de turno pode atender intervalo; não se exige igualdade com seu motivo principal. (`app/chamados/juizes.py:1597–1600`; `app/chamados/models.py:2502–2503`)

O desempate só roda com mais de um candidato. Sem data válida ou sem entradas de turnos abertos, mantém a lista original. Com entradas, seleciona os candidatos cujo `entrada_timestamp` casa com elas; um único vence; vários da mesma entrada e das categorias `turno_aberto`/`turno_aberto_24h` escolhem o estágio mais avançado; outras ambiguidades permanecem. (`app/chamados/services/disputa_emissao.py:110–146`, `:219–220`)

A ocupação por carimbo é uma escolha expressa dessa função, inclusive preservada em testes; sua diferença para `pergunta_viva` **não basta, isoladamente, para declarar defeito**. (`app/chamados/services/disputa_emissao.py:206–212`; `app/chamados/tests/test_selo_mapa_motivos_do_dia.py:122–143`)

**3. Histórico: seleção, bloqueios e diferença para o derivador**

O histórico exige simultaneamente:

- Mesmo colaborador; `status_local in ENCERRADOS`, que contém somente `resolvido` e `fechado`, excluindo `cancelado`/`superado`. (`app/chamados/reconciliador.py:1385–1386`; `app/chamados/catalogo/motor.py:30–31`)
- `contexto_json__data_turno == str(data_ctx)`; exclui categoria exatamente `disputa_supervisao`. (`app/chamados/reconciliador.py:1387–1388`)
- Motivo calculado por `MAPA_SUBCATEGORIA.get(sub) or MAPA_CATEGORIA.get(cat)` **igual** ao motivo da pergunta; a subcategoria tem precedência. (`app/chamados/reconciliador.py:1389–1394`)

Os desfechos são:

| Quantidade | Comportamento |
|---|---|
| Zero | Faz parse da data e tenta c1, depois c2. Se ambos recusarem, `False` em **1416**. (`app/chamados/reconciliador.py:1395–1416`) |
| Dois ou mais | `False` em **1416**, sem c1/c2 e sem desempate pelo turno. (`app/chamados/reconciliador.py:1395`, `:1415–1416`) |
| Um | Tenta costura; recusa dá `False` em **1436**. Costura aceita chama `resolver_perguntas_por_chamado_resolvido` e retorna `True`. (`app/chamados/reconciliador.py:1433–1440`) |

**Intervalos nunca entram nesse histórico:** nenhum valor dos dois mapas é `intervalo_saida` ou `intervalo_volta`; seus valores são apenas `orfao_14h` e `saida_sem_entrada`. (`app/chamados/models.py:2466–2483`; `app/chamados/reconciliador.py:1391–1394`)

**4. c1 — `lavrar_sem_turno_aberto`**

| Guarda | `False` |
|---|---|
| Motivo fora de `MOTIVOS_C1 = ('orfao_14h', 'saida_sem_entrada')` | `app/chamados/reconciliador.py:363–364`; conjunto em `:332` |
| Resposta do colaborador não vazia após `strip()` | `app/chamados/reconciliador.py:365–366` |
| Já materializada | `app/chamados/reconciliador.py:367–368`; excluído pelas premissas apresentadas |
| Existe turno aberto na janela do dia | `app/chamados/reconciliador.py:372–373` |
| `pergunta_viva(p)` é verdadeira | `app/chamados/reconciliador.py:398–400` |

Com dia, `_tem_turno_aberto` usa turnos com entrada e sem saída em `(dia−1, dia)`; há veto quando todos esses turnos têm seu próprio dia posicionalmente completo. Sem dia, usa consulta global de turnos abertos. (`app/chamados/reconciliador.py:96–114`, `:171–178`)

**Para a pergunta VIVA da questão, c1 necessariamente recusa:** ou alguma guarda anterior dispara, ou chega ao `False` de **400**. Isso protege o furo real contra carimbo sem lastro; existe teste que combina ausência de turno aberto, pergunta VIVA, lâmpada apagada e recusa de c1. (`app/chamados/reconciliador.py:374–400`; `app/chamados/tests/test_o261_lavra_le_a_lampada.py:129–146`)

Intervalos recusam na primeira guarda. A justificativa expressa é que ausência de turno aberto não resolve um marco no meio do turno. (`app/chamados/reconciliador.py:329–332`, `:363–364`)

**5. c2 — `reemitir_chamado_do_dia`**

C2 também aceita **somente `orfao_14h` e `saida_sem_entrada`**, porque reutiliza `MOTIVOS_C1`. `intervalo_saida` e `intervalo_volta` retornam `False` imediatamente em **713**, antes de examinar célula, protestos ou existência de chamado. (`app/chamados/reconciliador.py:332`, `:712–713`)

Para os motivos aceitos, a ordem das recusas é:

| Gate | Condição de recusa e linha |
|---|---|
| Data ausente | `not data_ctx` → `False` em **716**. (`app/chamados/reconciliador.py:714–716`) |
| Situação | `colab.situacao != 'ativo'` → **724**. (`app/chamados/reconciliador.py:723–724`) |
| Data inválida | `date.fromisoformat` levanta `ValueError` → **736**. (`app/chamados/reconciliador.py:732–736`) |
| Piso de emissão | Sem chamado vivo encontrado pela chave de c2 e fora de `emissao_no_horizonte` → **744**. (`app/chamados/reconciliador.py:738–744`) |
| Isenção | `cala_para_isento('batida_ausente') and isento_ponto(colab)` → **754**. O módulo pertence aos calados; isenção lê a flag do colaborador. (`app/chamados/reconciliador.py:753–754`; `app/chamados/catalogo/modulos.py:235–236`; `app/core/constants.py:13–18`) |
| Ausência | `ausencia_cobre(..., para='cobranca') is not None` → **757**. Para cobrança, ausência não rejeitada suspende, inclusive pendente. (`app/chamados/reconciliador.py:755–757`; `app/ponto/turnos.py:2475–2483`, `:2518–2523`) |
| Protesto | Sem costura em vivo e sem interseção dos protestos da célula com `PROTESTOS_QUE_COBRAM` → **771**. Célula ausente também cai aqui. (`app/chamados/reconciliador.py:766–771`) |
| Veto da célula | `celula_veta_cobranca` retorna motivo → **776**. (`app/chamados/reconciliador.py:772–776`) |
| Motor | `decidir(ESTADO_NASCENTE, 'EMITIR').permitido == False` → **778**. Na tabela atual essa chamada sempre permite. (`app/chamados/reconciliador.py:777–778`; `app/chamados/catalogo/motor.py:130–132`) |
| Resultado de `abrir` | Chamado devolvido continua fora de `VIVOS` → **806**. (`app/chamados/reconciliador.py:802–806`) |
| Costura | `reapontar_resolvedora` recusa → **822**. (`app/chamados/reconciliador.py:819–822`) |

O piso é **competência `2026-09`**, calculada pela janela da empresa, não pelo mês civil do dia. Não é teto temporal. (`app/ponto/services/cartorio.py:190`, `:232–246`)

Os protestos aceitos são exatamente `FURO_PARCIAL`, `FURO_SEM_COBRANCA`, `FURO_COM_COBRANCA_MORTA`. Estar em `VEREDITOS_ACUSAM` **não implica** ter um deles: a lista de vereditos também contém `indefinida`, `cobrado`, `nunca_bateu`, `cobranca_indevida` e outros. (`app/ponto/services/furos_sem_cobranca.py:35`; `app/escala/models.py:1735–1736`)

O veto da célula recusa `trabalha is False` ou veredito em `nunca_bateu`, `trabalhou`, `justificado`, `coberta`, `cobranca_indevida`. Assim, até alguns vereditos que **acusam** podem legitimamente vetar essa emissão. (`app/ponto/marcos.py:119`, `:155–163`; `app/escala/models.py:1735–1736`)

A exceção `_costura_no_vivo` dispensa **piso e protesto**; não dispensa situação, isenção, ausência, veto da célula ou costura final. Sua busca usa exclusivamente módulo `batida_ausente` e `contexto_json__data_turno`. (`app/chamados/reconciliador.py:738–777`)

Quanto ao **teto temporal**, c2 não chama diretamente `dia_encerrado`: lê os protestos já gravados. O classificador que produz os protestos de furo exige turno encerrado nos respectivos ramos; portanto o teto chega por essa classificação, sem rechecagem explícita em c2. (`app/chamados/reconciliador.py:712–823`; `app/ponto/supra_juiz.py:245–253`, `:280–283`)

Quanto a **“cala”**, o gate direto é `cala_para_isento`. O `_cala_emissao` do cartório é outro predicado — chamado vivo ou fora do piso — e não é chamado por c2; `cartorio.calado()` controla reentrada de julgamento, não devolve veto de c2. (`app/chamados/reconciliador.py:753–754`; `app/ponto/services/cartorio.py:1319–1327`, `:58–68`)

**6. Gates transitivos de `abrir` que terminam no `False` de c2:806**

`abrir` encontra o existente pela chave, tenta renascer apenas se estiver em `ENCERRADOS` e devolve o mesmo objeto, mesmo quando o renascimento recusa. Cancelado/superado não entram nessa tentativa. (`app/chamados/models.py:670–688`; `app/chamados/catalogo/motor.py:30–31`)

O renascimento recusa por:

- Colaborador não ativo, via `cascade`, competência trancada ou recusa de `reabrir`. (`app/chamados/models.py:764–771`)
- Na reabertura: fechamento pelo admin sem fato novo; premissa regularizada; agrupamento sob pai vivo; irmão vivo do turno; transição não permitida. As recusas retornam em **1056, 1062, 1064, 1083 e 1085**. (`app/chamados/models.py:1003–1020`, `:1050–1085`)

A premissa regularizada consulta justificativa aprovada, decisão do admin na célula, dia sem trabalho, lâmpadas dos marcos pedidos, marcos inexistentes e, depois, o coletor do furo. Portanto, **esse gate lê lâmpadas**, não apenas o status do chamado. (`app/chamados/models.py:937–985`)

Competência **exportada** deixou de ser veto; competência **trancada** permanece no renascimento. O juiz da pergunta, por sua vez, permite exceção à tranca quando houve toque posterior do admin; assim, “pergunta VIVA” não elimina universalmente a possibilidade do gate transitivo de tranca. (`app/chamados/models.py:753–769`; `app/chamados/juizes.py:875–907`)

**7. Recusa comum da costura**

Nos três caminhos que chamam `reapontar_resolvedora` — vivo, histórico e c2 — a recusa vem de `resolvedora_valida`; a porta desfaz eventual data acrescentada e não grava o destino. (`app/chamados/services/fio.py:788–801`)

Esse juiz permite ignorância de data/escopo; com datas conhecidas, permite igualdade e diferença de um dia quando o turno cruza meia-noite **ou quando isso é desconhecido**. Fora disso, retorna `(False, motivo)` em **1737**. (`app/chamados/juizes.py:1702–1738`)

**Sobre o defeito pedido, a evidência permite distinguir:**

- **Defeito demonstrável no histórico:** aplica o mapa principal 1:1 onde a regra de atendimento permite os quatro motivos. Pode ignorar chamado encerrado do mesmo dia que atende a pergunta; para intervalos, essa exclusão é absoluta. A ausência de candidato em `hist` não prova ausência legítima de chamado. (`app/chamados/reconciliador.py:1391–1394`; `app/chamados/models.py:2486–2503`; `app/chamados/juizes.py:1597–1599`)
- **Lacuna demonstrável para intervalos:** após falhar o derivador vivo, o histórico nunca os seleciona e c1/c2 recusam pelo motivo. A justificativa escrita para excluir intervalo pertence à **lavra por ausência de turno**, enquanto o chamado do dia atende intervalos e o emissor do cartório trata `FURO_PARCIAL`. Isso impede concluir que esses `False` significam “não há cobrança cabível”. (`app/chamados/reconciliador.py:329–332`, `:712–713`, `:1391–1416`; `app/chamados/juizes.py:1597–1599`; `app/ponto/services/cartorio.py:1381–1396`)
- **Pré-filtros divergentes do juiz:** o derivador elimina qualquer diferença de dia antes de alcançar o juiz que aceita certos casos adjacentes; o histórico busca apenas JSON `data_turno`, enquanto o juiz do dia prioriza célula e chave declarada no catálogo. Podem excluir candidatos admissíveis; ocorrência nesses ~140 **[não verificado]**. (`app/chamados/services/disputa_emissao.py:214–217`; `app/chamados/juizes.py:1730–1738`, `:89–112`; `app/chamados/reconciliador.py:1387`)
- **c1 recusando VIVA é correto:** sua própria regra exige essa recusa para não enterrar furo real. (`app/chamados/reconciliador.py:374–400`)
- **Não há veto pelo status terminal do pai da disputa nesses caminhos.** O derivador filtra o estado dos candidatos; c2 verifica o chamado devolvido por `abrir`. O gate transitivo que lê um pai é `agrupado_em`, e só bloqueia quando esse pai está **vivo** — pai terminal não satisfaz essa guarda. (`app/chamados/services/disputa_emissao.py:181–184`; `app/chamados/reconciliador.py:802–806`; `app/chamados/models.py:581–596`, `:1012–1013`)

## 29/10 23:40 · sessao 936da2887a4a · - · - · origem=Ronald (tmux)

`_derivar_chamado_destino` é a função de `services/disputa_emissao.py`, reexportada por `models.py`; o chamador fornece `p.data_evento`, que precede a data do contexto da disputa. (`app/chamados/models.py:2778`; `app/chamados/reconciliador.py:1369-1370`; `app/chamados/services/disputa_emissao.py:174-179`)

- Candidatos iniciais: colaborador da disputa, `status_local ∈ VIVOS`, excluindo categoria exatamente `disputa_supervisao`; não há filtro direto por `modulo_origem`. `VIVOS` inclui aberto, em análise e registrado. (`app/chamados/services/disputa_emissao.py:181-184`; `app/chamados/catalogo/motor.py:24`)
- Categoria/subcategoria: normaliza os dois rótulos; descarta quando o motivo não pertence a `motivos_atendidos(cat, sub)`. Esse juiz devolve os quatro motivos do dia inteiro quando **qualquer** rótulo pertence aos mapas; módulo e lâmpadas não participam dessa classificação. (`app/chamados/services/disputa_emissao.py:186-196`; `app/chamados/juizes.py:1589-1600`; `app/chamados/models.py:2502-2503`)
- Ocupado: descarta se houver qualquer pergunta com `chamado_destino=ch`, mesmo motivo e `materializada_em IS NULL`; não filtra disputa aberta, dia da pergunta ou `pergunta_viva`. O comentário e os testes declaram intencionalmente o carimbo como régua desse dedup; portanto isso sozinho não prova defeito sob a regra local. (`app/chamados/services/disputa_emissao.py:206-212`; `app/chamados/tests/test_contract_leitores_vitalidade.py:84`; `app/chamados/tests/test_selo_mapa_motivos_do_dia.py:122-143`)
- Dia incompatível: obtém `dia_do_fato(ch)` e descarta quando ambas as datas existem e diferem textualmente. Sem uma das datas, o candidato passa. (`app/chamados/services/disputa_emissao.py:213-218`)
- Ambiguidade: com mais de um, tenta desempatar pelo turno; sem data válida ou entradas abertas devolve candidatos inalterados; casa `entrada_timestamp` com entradas de turnos abertos; dois estágios da mesma entrada escolhem o mais avançado. Sobrando zero ou dois ou mais, retorna `(None, None)` em **227**. (`app/chamados/services/disputa_emissao.py:110-146`; `app/chamados/services/disputa_emissao.py:219-227`)

`None` do derivador **não retorna False diretamente**: entra no histórico. Se faltar data da pergunta e do contexto, retorna False em **1383**. O histórico exige mesmo colaborador, estado em `ENCERRADOS` — somente resolvido/fechado, excluindo cancelado/superado —, `contexto_json__data_turno` exatamente igual a `str(data_ctx)` e categoria diferente de `disputa_supervisao`. (`app/chamados/reconciliador.py:1371-1388`; `app/chamados/catalogo/motor.py:30-31`)

No histórico, o motivo é `MAPA_SUBCATEGORIA.get(sub) or MAPA_CATEGORIA.get(cat)`, com igualdade estrita ao motivo da pergunta; não usa `motivos_atendidos`. Zero históricos tenta c1 e depois c2; nenhum sucesso termina no False **1416**. Dois ou mais históricos também retornam False **1416**, **sem tentar c1/c2**. Exatamente um chama `reapontar_resolvedora`; recusa retorna False **1436**, sucesso chama o resolver e retorna True **1440**. (`app/chamados/reconciliador.py:1389-1416`; `app/chamados/reconciliador.py:1433-1440`)

Com candidato vivo, recusa de `reapontar_resolvedora` retorna False **1448**; não tenta histórico/c1/c2. A porta chama `resolvedora_valida`; recusa não salva destino e desfaz a data que tenha preenchido em memória. (`app/chamados/reconciliador.py:1445-1449`; `app/chamados/services/fio.py:788-801`)

Duas inconsistências verificáveis, com conclusão condicional sobre os 140 casos:

1. **Histórico conserva o mapa de motivo principal.** Seus mapas só produzem `orfao_14h` ou `saida_sem_entrada`, logo `intervalo_saida`/`intervalo_volta` jamais entram em `hist`, mesmo diante do chamado encerrado do dia. A regra já declarada diz que o chamado do turno atende os quatro motivos; há teste explícito de intervalo costurado no turno vivo. Isso demonstra divergência entre o ramo histórico e a regra de motivos atendidos; não prova quantos dos 140 possuem histórico elegível. (`app/chamados/reconciliador.py:1391-1394`; `app/chamados/models.py:2466-2483`; `app/chamados/models.py:2486-2503`; `app/chamados/juizes.py:1597-1599`; `app/chamados/tests/test_selo_mapa_motivos_do_dia.py:60-68`)

2. **Pré-filtro de dia mais estrito que o juiz da costura.** O derivador rejeita qualquer diferença de dia; `resolvedora_valida` autoriza diferença de um dia se o turno cruza meia-noite **ou se não foi possível determinar isso**. Assim, sob essa condição, pode eliminar antes da porta um candidato cuja própria regra da costura permitiria. O histórico também procura exclusivamente JSON `data_turno`, enquanto `dia_do_fato` prioriza a célula e a chave de catálogo; portanto pode deixar de enxergar um encerrado cujo dia oficial coincide. São divergências demonstradas no código; ocorrência concreta nesses 140 não verificada. (`app/chamados/services/disputa_emissao.py:214-217`; `app/chamados/juizes.py:1730-1738`; `app/chamados/reconciliador.py:1385-1388`; `app/chamados/juizes.py:89-112`)

Nenhum desses trechos lê o **estado do pai da disputa** como veto; o derivador filtra o estado dos próprios candidatos e o histórico filtra o estado dos próprios encerrados. (`app/chamados/services/disputa_emissao.py:181-184`; `app/chamados/reconciliador.py:1385-1388`)

## 29/10 23:39 · sessao eeca2e3104d5 · mariaB · verificador · origem=Code

1. **O diff deixa um selo existente vermelho.** O filtro novo em [fabrica_da_celula.py:128](/home/ronald/codex_ws/83bc31eb46db22d481dedfed7b3d9805b1896044/app/chamados/services/fabrica_da_celula.py:128) é detectado pelo scanner de vitalidade, mas `perguntar_pela_celula` não está na allowlist. Executei o scanner em memória: o único ofensor retornado foi essa linha. A asserção existente exige lista vazia. **Classe: pré-requisito de pouso não atendido.** Isso exige registrar corretamente a elegibilidade do escritor; não demonstra, sozinho, um segundo juiz. (`app/chamados/tests/test_contract_leitores_vitalidade.py:161`, `:286`, `:297`)

2. **Há custo síncrono novo na batida, inclusive sem carimbo para curar.** Célula julgada, nenhuma pergunta materializada → o queryset novo faz **um SELECT vazio adicional por chamada**, antes das guardas de emissão. Havendo candidatas, a ausência de `select_related` acrescenta carregamentos por pergunta: célula, disputa, colaborador, empresa e consulta da tranca. **Classe: custo no chokepoint sem certificação correspondente.** (`app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/relavra_via.py:195`, `app/chamados/juizes.py:757`, `:888`, `:896`, `:900`)

   O selo fixa tetos sem folga de 191/241 queries nos cenários que fecham chamados e 134/194 nos que os mantêm vivos; o diff não inclui medição nem justificativa desse acréscimo. Totais reais após a mudança e resultado da execução do selo: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:136`, `:157`, `:163`)

3. **A frota já julgada continua podendo conservar o estado ilegal.** Cenário: dia passado, impressão atual, entrada apagada, saída acesa, pergunta sem resposta carimbada `sem_turno_aberto` e viva → numa execução sem novo julgamento daquela data, o cartório pula pela impressão igual; a data não entra em `cods`, a fábrica não roda e o carimbo permanece. **Classe: cura parcial por alcance; frota sem passagem garantida pela porta.** (`app/ponto/services/cartorio.py:1092`, `:1120`, `:759`, `:769`)

   O lote não resolve esse resíduo: elimina fatos com pergunta existente; o caminho de fio mudo também barra pergunta viva ou já existente. O reatador exige materialização nula. Portanto, esses caminhos não compensam a lacuna. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `:163`, `:220`, `app/chamados/reconciliador.py:1317`)

   Há também mudança do juiz fora do julgamento da célula: pergunta sem `CelulaDia`, em data da janela viva, recebe uma saída sem entrada → `classificar_falta` passa a acusar e o C6 pode considerá-la viva; entretanto, `julgar_celula` retorna imediatamente sem célula. A cura nova nunca chega nela. (`app/escala/servico_jornada.py:452`, `app/chamados/juizes.py:1066`, `app/ponto/services/cartorio.py:1547`)

   **Quem cobraria:** I8 denuncia vivas sem destino; I8bc cobre materializadas com destino inválido. São recortes parciais. O vigia de carimbo examina validadas, enquanto o vigia do fio considera uma pergunta viva como fio existente. Não há nesses caminhos uma cobrança geral desta ilegalidade. (`app/chamados/management/commands/auditar_invariantes_chamados.py:853`, `:890`, `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/vigia_fio.py:155`)

4. **Derrubar o carimbo também reabre disputas históricas e pode duplicar a cobrança.** Pergunta viva carimbada em D1 fechada → a porta limpa materialização/autor/via, audita e chama `reabrir_sistema`, apagando fechamento, resolução e motivo da disputa. O pai recebe nota, mas seu status não é reaberto por esse método. **Classe: efeitos de reabertura sem os pré-requisitos do ciclo.** (`app/chamados/services/relavra_via.py:204`, `:209`, `:220`, `app/chamados/models.py:1725`)

   Dois cenários concretos:

   - **Legado:** pergunta histórica em disputa fechada → julgamento posterior reabre a disputa e devolve-a à cobrança diária. Isso contraria o corte registrado para as vivas históricas em disputas fechadas: aparecer ao admin para validar, sem reabrir e sem push. O aval B registra cura do estado ilegal, mas não explicita revogação desse tratamento histórico. (`app/docs/CORTES.md:326`, `:381`, `app/chamados/services/relavra_via.py:220`)
   - **Outra disputa já aberta:** D1 fechada contém o carimbo; D2 foi aberta posteriormente → curar D1 deixa **duas abertas para o colaborador**, pois a reabertura não consulta D2. O emissor declara expressamente esse resultado como corrupção do invariante. (`app/chamados/models.py:1719`, `app/chamados/services/disputa_emissao.py:437`, `:466`)

   Não há push imediato em `reabrir_sistema`. Porém, o cron posterior percorre todas as abertas, podendo cobrar D1 e D2 separadamente, enviar dois pushes e contar duas pendências como dois colaboradores. Usa ainda a idade original de abertura. Esses são efeitos observáveis além da queda do carimbo. (`app/chamados/models.py:1729`, `app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `:48`, `:52`, `:59`, `:103`)

5. **A varredura pode apagar decisão humana concorrente.** A carrega a pergunta com `sem_turno_aberto`; B executa encerramento administrativo e grava `admin_decidiu`, materialização e autor humano; A continua usando a instância antiga, passa pela guarda de identidade e salva os três campos vazios. A decisão humana desaparece; se A carregar a disputa depois do fechamento, também poderá reabri-la. **Classe: ordem de trava/transação; decisão sobre instância velha.** O queryset novo não trava a pergunta, e a porta escreve sem conferir novamente o estado gravado. É uma fragilidade preexistente da porta agora exposta à varredura automática. (`app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/relavra_via.py:198`, `:207`, `:220`, `app/chamados/models.py:1499`, `:2131`)

Os caminhos e o custo associado à chamada nova são:

| Caminho | Encadeamento |
|---|---|
| Batida | `registrar_batida` → signal → julgamento de D−1 e D → `julgar_colab` → fábrica; paga a consulta nova quando alcança a fábrica. (`app/ponto/registro_batida.py:157`, `app/ponto/signals.py:92`, `:108`, `:131`, `app/ponto/services/cartorio.py:770`) |
| Cron diário | `processar_cartorio --apply` → `julgar_colab` → fábrica. (`app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:65`) |
| Cron de dez minutos | `vigia_de_hora` → `julgar_celula(teto_de_hoje=True)` → mesmo caminho. (`app/config/crons.py:348`, `app/ponto/services/vigia_de_hora.py:130`) |
| Lote de perguntas | Chama `perguntar` diretamente; **não passa pela função alterada**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:276`, `:288`) |

Quanto aos quatro testes novos, a análise sem a mudança é: [nao verificado]

| Teste | Resultado esperado sem o diff | [nao verificado]
|---|---|
| Cura da pergunta viva | **Vermelho:** exige materialização nula. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:95`) |
| Morta conserva carimbo | **Verde:** não fazer nada já preserva o estado. (`:108`) | [nao verificado]
| Identidade conserva carimbo | **Verde:** mesmo motivo. (`:116`) | [nao verificado]
| Idempotência | **Verde sem curar:** fotografa a primeira passagem, mas não exige que ela tenha removido o carimbo. (`:122`, `:125`, `:128`) | [nao verificado]

Os dois controles são úteis, mas não são REDs contra a ausência da cura. A bateria chama diretamente a fábrica, com dia passado; não verifica o alcance pelo cartório, os efeitos de reabertura, concorrência, teto temporal ou custo da batida. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:36`, `:81`, `:85`)

Execução Django, reprodução concorrente em banco e os números de sombra citados no docstring: **[nao verificado]**.

## 29/10 23:39 · sessao 754eea13af2e · - · - · origem=Ronald (tmux)

C1 só é chamado quando `_derivar_chamado_destino` não achou destino e `hist` ficou vazio; `False` de C1 prossegue para C2, e só retorna `False` da porta se C2 também falhar (`app/chamados/reconciliador.py:1371`, `:1395`, `:1411-1416`).

| Guarda de C1 | Condição | Retorno |
|---|---|---|
| Escopo | `p.motivo not in MOTIVOS_C1`; lista contém somente `orfao_14h`, `saida_sem_entrada` (`app/chamados/reconciliador.py:332`) | `False` em `app/chamados/reconciliador.py:364`, condição `:363` |
| Resposta | `(p.resposta_colab or '').strip()` preenchido | `False` em `app/chamados/reconciliador.py:366`, condição `:365` |
| Idempotência | `p.materializada_em is not None` | `False` em `app/chamados/reconciliador.py:368`, condição `:367`; excluído pela premissa do universo (`:1321`) |
| Núcleo | `_tem_turno_aberto(colab, dia=dia)` | `False` em `app/chamados/reconciliador.py:373`, condição `:372` |
| Juiz soberano | `pergunta_viva(p)` | `False` em `app/chamados/reconciliador.py:400`, condição `:399` |

`dia` fornecido por `reatar_pergunta_orfa` vem do parse de `data_ctx`; parse inválido produz `None`, e nesse caso C1 substitui por `p.data_evento` (`app/chamados/reconciliador.py:1403-1406`, `:1411`, `:370-371`). Com dia, `_tem_turno_aberto` delega a `turnos_abertos_de`, consultando entrada presente/saída ausente na janela `(dia−1, dia)`; se todos esses turnos têm seu próprio dia posicionalmente completo, o veto devolve lista vazia (`app/chamados/reconciliador.py:171-172`, `:96-99`, `:111-114`). Sem dia, consulta turnos abertos globalmente (`app/chamados/reconciliador.py:173-178`).

Sob a premissa de pergunta **VIVA**, C1 necessariamente recusa: se nenhuma guarda anterior disparar, `pergunta_viva(p)` dispara a última (`app/chamados/reconciliador.py:363-400`). Intervalos recusam já na primeira guarda; o comentário declara que ausência de turno aberto não responde ao marco no meio do turno (`app/chamados/reconciliador.py:329-332`).

**Essa recusa de C1 é legítima conforme a própria regra, não o defeito procurado:** C1 agora lê o juiz da pergunta antes de carimbar, precisamente para não matar pergunta cuja célula acusa (`app/chamados/reconciliador.py:374-400`). O teste monta saída real sem entrada, lâmpada de entrada apagada, célula `furo`, e verifica simultaneamente `_tem_turno_aberto=False`, `pergunta_viva=True`, `lavrar_sem_turno_aberto=False`, sem carimbo nem fechamento da disputa (`app/chamados/tests/test_o261_lavra_le_a_lampada.py:108-125`, `:129-146`). Há o contraponto: pergunta já validada é morta pelo juiz, C1 retorna `True` e carimba a via (`app/chamados/tests/test_o261_lavra_le_a_lampada.py:159-170`).

C1 não consulta o status do chamado pai: o corpo só lê disputa para obter colaborador e, depois de carimbar, verificar se pode fechar a disputa (`app/chamados/reconciliador.py:369`, `:398-416`). O juiz considera célula que acusa suficiente para manter a pergunta viva, após suas precedências de validação/tranca/resposta/identidade (`app/chamados/juizes.py:1005-1057`).

## 29/10 23:38 · sessao 7ded52124fe0 · orcamento-contexto · verificador · origem=Code

O ponto central é que **CLEAR precisa de uma transição própria no Stop**: o caminho atual de `PAREI` julga travas por item e pela existência de outros itens andáveis. Ampliar apenas o reconhecimento de `!` não resolve. (`bin/hook_stop_fila1.py:693`, `:724`, `:661`.)

1. **`PAREI: ! CLEAR` não libera pelo motivo pretendido.** `_SO_BANG` aceita o `!`, mas `_parei_do_em_curso` exige também o ID inteiro do item em curso. Cenário: O261 aberto e topo `PAREI: ! CLEAR | espera Ronald` → continua `siga`. Acrescentar O261 passa esse filtro, mas outro item andável produz “trava so O261”. Consequência: a forma proposta continua bloqueada; esse recado específico pressupõe que o topo também tenha casado O261. ([bin/hook_stop_fila1.py:724](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:724), `bin/hook_stop_fila1.py:73`, `:650`, `:272`, `:780`.)

2. **O caminho genérico também pode liberar CLEAR indefinidamente.** Cenário: topo `PAREI: ! CLEAR O261...`, com todos os demais itens travados. `_relato_parou()` autoriza e `main` libera antes de qualquer outra decisão, sem verificar prazo, sessão ou marco. Consequência: depois de 30 minutos, esse mesmo topo continua liberando se a expiração não tiver precedência explícita sobre o `PAREI` genérico. ([bin/hook_stop_fila1.py:769](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:769), `bin/hook_stop_fila1.py:661`, `:724`.)

3. **TETO pode mascarar o reconhecimento e vencer a expiração.** O código declara `TETO = 5`, mas libera quando `n > TETO`: a sexta tentativa no mesmo HEAD, sem trabalho detectado. `_libera` não limpa o contador. Cenário: CLEAR expirado volta ao fluxo comum com contador já acima do limite → sai silenciosamente pelo TETO, em vez do `siga` prometido. Com suite/push detectado, o teto não cresce nem libera; portanto ele tampouco representa um prazo. ([bin/hook_stop_fila1.py:827](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:827), `bin/hook_stop_fila1.py:37`, `:123`, `:815`, `:820`.)

4. **A retomada aos 30 minutos depende de outra chamada.** `_libera` termina o processo com `sys.exit(0)`. Cenário: CLEAR libera o Stop e Ronald não envia mais nada → essa execução não estará presente aos 30 minutos para devolver `siga`. Consequência: com o mecanismo descrito, a expiração pode ser aplicada **no próximo Stop**; retomada automática exatamente no prazo não está demonstrada. ([bin/hook_stop_fila1.py:133](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:133). Outro mecanismo de reentrada: **[nao verificado]**.)

5. **“Consulta a cada Stop” conflita com as saídas antecipadas de ESPERANDO e PAREI.** Hoje a ordem é prova do RELATO → PAREI → ESPERANDO comprovado → fila/TETO. Cenário: orçamento estoura com produtor vivo declarado em ESPERANDO; consulta colocada depois dessa porta nunca roda. Colocada antes e impondo `siga` incondicionalmente, altera a espera sem turno já selada. Além disso, ESPERANDO libera enquanto há produtor trabalhando: não comprova “nada em voo” nem atendimento do CLEAR. ([bin/hook_stop_fila1.py:763](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:763), `bin/hook_stop_fila1.py:774`; `bin/tests/test_hook_esperando_com_prova.sh:94`, `:106`, `:186`.)

6. **`transcript_path` e a forma de `usage` ainda não estão provados pela árvore.** O hook apenas lê e descarta o stdin. Os selos examinados enviam `{}` ou stdin vazio. Cenário: harness entrega campo diferente, transcript não contém `usage`, ou o leitor seleciona o registro errado → os selos atuais não detectam a medida ausente ou incorreta. Que o Stop real entregue `transcript_path`, onde estejam os três campos e qual seja a janela aplicável: **[nao verificado]**. ([bin/hook_stop_fila1.py:755](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:755), `bin/tests/test_hook_esperando_com_prova.sh:65`; `bin/tests/test_hook_stop_saida_valida.sh:26`.)

7. **Falha da medida pode virar liberação geral.** Cenário: parsing/importação do avaliador lança exceção dentro do `try` principal → o tratamento externo chama `_libera`, inclusive com fila aberta. Consequência: isso não equivale a marcar somente (a) como desconhecido e continuar julgando (b)/(c) e a fila; a falha precisa ser contida na fronteira da medida. ([bin/hook_stop_fila1.py:839](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:839), `bin/hook_stop_fila1.py:758`.)

8. **O ciclo `/clear`/compact não está coberto pelo selo de handoff.** O selo exige `SessionStart` matcher `compact`; procura o comando de impressão em qualquer entrada, sem vinculá-lo ao matcher. Cenário: acrescentar matcher `clear` errado → esse selo pode continuar passando sem provar marco nem retomada pelo handoff. Para compact, o comportamento documentado é imprimir o handoff; isso não demonstra o avanço da geração que deve rearmar o orçamento após compact. A configuração instalada está **[nao verificado]**; `.claude/` é ignorada pelo Git. ([bin/tests/test_handoff_sessao.sh:57](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/tests/test_handoff_sessao.sh:57), `CLAUDE.md:726`; `.gitignore:66`.)

9. **(c) por assunto de commit não tem forma canônica comprovada.** A regra manda escrever “MARCO FECHADO” no **painel**; o fabricante prescreve título `[NOME-DA-FATIA] <o que mudou>`, e o integrador usa a mensagem fornecida. Cenário: marco legítimo fechado com esse título → regex `POUSA`/`MARCO FECHADO` conta zero. Consequência: subcontagem. “Desde o marco” também precisa definir o tratamento dos merges: já existe selo usando `--first-parent` para distinguir pouso da história da raia. Histórico real do `main`: **[nao verificado]**. ([CLAUDE.md:720](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/CLAUDE.md:720), `bin/fabricante_prompt.md:49`; `app/core/integrador_lote.py:481`; `bin/tests/test_lei_akita.sh:107`.)

10. **(c) por deploy também precisa escolher uma testemunha suficiente.** Cenários concretos: [nao verificado]

    - Três deploys antes do próximo Stop deixam apenas o último `deploy.stamp`, sobrescrito → subcontagem. (`bin/deploy.sh:337`, `:341`.)
    - Agendamento termina com `rc=1`, mas grava `.done` → contar existência produz falso positivo; o censo alerta expressamente para isso. (`bin/deploy_agendado.sh:73`, `:79`; `bin/artefatos_de_estado.txt:30`.)
    - Push passa e deploy falha no integrador; o retorno do deploy não é condicionado antes de marcar `FIM`/`NO_AR` → contar finalizações de lote como publicação pode produzir falso positivo. (`app/core/integrador_lote.py:748`, `:750`, `:753`.)

11. **Os selos atuais podem ficar verdes sem provar CLEAR.** O selo de ESPERANDO já apaga o contador entre casos para evitar falso sucesso pelo sexto bloqueio e exige o motivo correto na trilha. Cenário: novo selo verifica apenas stdout vazio → passa pelo TETO ou pela falha aberta. Também é necessário preservar os casos atuais: `PAREI` comum bloqueia quando outro item anda e libera quando os demais estão travados; CLEAR exige sua própria exceção. ([bin/tests/test_hook_esperando_com_prova.sh:63](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/tests/test_hook_esperando_com_prova.sh:63), `bin/tests/test_hook_esperando_com_prova.sh:74`; `bin/tests/test_hook_parei_com_andavel.sh:45`, `:55`.)

## 29/10 23:37 · sessao a913b56dd371 · - · - · origem=Ronald (tmux)

Achados sobre (c), por leitura estática:

1. **Assunto `POUSA`/`MARCO FECHADO` não é a forma estabelecida para commits.** A norma escreve `MARCO FECHADO` no **painel**, após item/cura/lote; o fabricante exige título `[NOME-DA-FATIA] <o que mudou>`, e o integrador usa essa mensagem diretamente. Cenário: marco legítimo com título `[O123] corrige ...` e painel atualizado; a regex retorna zero, subcontando marcos e atrasando o gatilho. (`CLAUDE.md:720-727`; `bin/fabricante_prompt.md:49`; `app/core/integrador_lote.py:466-481`; exemplo de molde em `bin/molde_autocura/msg_commit.molde:1`.)

2. **`deploy.stamp` não oferece histórico contável.** O deploy grava `QUANDO`, `COMMIT`, `TITULO` com sobrescrita e troca do mesmo arquivo; o handoff exibe apenas o último. Cenário: três deploys desde clear, antes do próximo Stop; sobra um carimbo, insuficiente para reconstruir três eventos. (`bin/deploy.sh:335-341`; `bin/handoff_sessao.sh:237-240`.)

3. **Arquivo `.done` não comprova deploy bem sucedido.** O agendador registra qualquer `rc` e copia o resultado para `.done` ao terminar; o próprio censo declara que existência não prova sucesso. Cenário: esteira retorna `rc=1`; contar `.done` como deploy acrescenta marco inexistente e pode disparar (c) falsamente. (`bin/deploy_agendado.sh:73-79`; `bin/artefatos_de_estado.txt:30`.)

4. **Contar execuções de deploy também inclui higiene sem marco novo.** Existe cron diário `deploy.sh --reload-agendado`; o mesmo script termina gravando carimbo de publicação. Cenário: nenhuma obra fechada, apenas reload das 03:30; contador de deploys incrementa mesmo assim. É preciso decidir explicitamente se a unidade passa a ser reload ou marco. (`app/config/crons.py:300-308`; `bin/deploy.sh:48-54`; `bin/deploy.sh:335-342`.)

5. **Contar lotes verdes não garante deploy provado.** `_rodar` devolve `returncode`, mas `_subir` ignora o retorno do deploy e continua marcando fatias `FIM`/`NO_AR`; `_fechar` persiste o registro. Cenário: push passa, deploy falha; contar `logs/lotes.jsonl`/fatias finalizadas como publicação pode produzir falso positivo. (`app/core/integrador_lote.py:242-248`; `app/core/integrador_lote.py:739-758`; `app/core/integrador_lote.py:812-815`.)

Para a opção commits, o recorte também precisa definir tratamento de merges: a casa exige `--no-ff`, e outro selo usa `--first-parent` justamente para não julgar novamente a história da raia. Cenário inferido: contar todos os commits alcançáveis pode contar commits da raia e seu merge como várias unidades; contar por data pode perder commits antigos incorporados depois do clear. (`CLAUDE.md:171-173`; `bin/tests/test_lei_akita.sh:107-109`.)

Histórico real de assuntos no `main` e conteúdo dos logs do host: **[nao verificado]**. Os logs são dados ignorados pelo Git; sua ausência na árvore fornecida não demonstra ausência no host. (`.gitignore:10`; `.gitignore:53-54`.)

## 29/10 23:37 · sessao 5ba6333cbc63 · - · - · origem=Ronald (tmux)

Achados do hook, em leitura:

1. **`! CLEAR` não é forma reconhecida.** `_SO_BANG` aceita qualquer `!`, mas o juiz exige também o ID inteiro do item em curso; logo `PAREI: ! CLEAR | espera Ronald`, com O261 aberto, resulta em `siga: O261...`, mesmo com O261 como único item. Acrescentar O261 passa esse juiz, porém outro item andável impede liberar e produz exatamente “trava so O261”. Portanto o relato desse texto exige que o topo também tenha casado O261; o `! CLEAR` isolado não explica esse ramo. Fontes: `bin/hook_stop_fila1.py:73`, `:693-727`, `:650-663`, `:272-274`, `:779-781`.

2. **O ramo genérico pode liberar CLEAR indefinidamente.** Com topo `PAREI: ! CLEAR O261...` e os demais itens travados, `_relato_parou()` retorna verdadeiro e `main` libera sem consultar tempo, sessão ou marco. Depois de 30 minutos, o mesmo topo continua liberando. Fontes: `bin/hook_stop_fila1.py:661-663`, `:724-727`, `:769-770`.

3. **TETO pode simular sucesso e impedir a retomada.** O limite declarado é 5, mas a condição executada é `n > TETO`: no mesmo HEAD, a sexta tentativa sem trabalho detectado libera. `_libera` não zera o contador; uma implementação da expiração que apenas devolva ao trecho comum pode imediatamente liberar de novo por TETO, em vez de emitir o `siga` exigido. Fontes: `bin/hook_stop_fila1.py:37`, `:123-133`, `:798-836`. O próprio selo de ESPERANDO reconhece e evita a falsa liberação pelo sexto caso apagando o contador entre cenários: `bin/tests/test_hook_esperando_com_prova.sh:63-65`.

4. **A ordem de integração precisa ser explícita.** Hoje a ordem é prova do RELATO → PAREI → ESPERANDO comprovado → fila → TETO. Avaliar orçamento depois de ESPERANDO não consulta “a cada Stop” liberado por esse ramo; retornar antes do checador pula a prova obrigatória do RELATO. Fontes: `bin/hook_stop_fila1.py:763-792`; o selo exige que ESPERANDO válido não pule a prova: `bin/tests/test_hook_esperando_com_prova.sh:186-189`.

5. **ESPERANDO não prova preparação concluída.** Um produtor vivo ou arquivo avançando libera o Stop enquanto o trabalho continua; portanto não serve como evidência de “nada em voo”, nem como atendimento do CLEAR. Fontes: `bin/hook_stop_fila1.py:610-645`, `:774-776`; casos concretos no selo: `bin/tests/test_hook_esperando_com_prova.sh:93-114`.

6. **O teto não oferece prazo de 30 minutos.** Ele mede chamadas por HEAD; com suite/push detectado, não cresce nem se aplica. CLEAR sem ID pode, portanto, continuar recebendo `siga` enquanto esse trabalho existir, independentemente do relógio. Fontes: `bin/hook_stop_fila1.py:386-396`, `:805-827`. Os dois sentidos são exigidos em `bin/tests/test_hook_teto_nao_conta_espera.sh:57-67` e `:69-99`.

7. **Os selos existentes defendem a regra oposta à exceção global de CLEAR.** Exigem bloquear PAREI do item em curso quando há outro andável, liberar quando todos os demais estão travados e rejeitar PAREI de outro item/SONDA. CLEAR precisa de exceção própria sem transformar o `!` genérico em autorização global. Fontes: `bin/tests/test_hook_parei_com_andavel.sh:45-61`; `bin/tests/test_hook_parei_trava_o_item_em_curso.sh:52-75`.

8. **Falha da nova medida não pode escapar para o tratamento geral.** Uma exceção do avaliador dentro do `try` geral libera o Stop, inclusive com fila aberta; isso é diferente de simplesmente ignorar a medida (a) e continuar os demais julgamentos. Fontes: `bin/hook_stop_fila1.py:758`, `:839-840`.

9. **O hook atual descarta o payload inteiro.** `stdin` é lido e ignorado; não há extração de `transcript_path` nesse caminho. O conteúdo real recebido do harness não foi verificado por este explorer. Fonte: `bin/hook_stop_fila1.py:753-757`; payload real: `[nao verificado]`.

Sonda complementar executada apenas em memória, carregando o fonte com `compile/exec` e substituindo leitores por lambdas: reproduziu os quatro resultados de CLEAR descritos nos itens 1–2; nenhum teste shell foi executado nem arquivo escrito. A fundamentação reproduzível desses resultados está nos predicados citados acima. [nao verificado]

_309 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
