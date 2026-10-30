# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 21:55** · publicado em **29/10/2026 21:55** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 21:55 · sessao c6476a4dda70 · - · - · origem=Ronald (tmux)

**Censo: 6 chamadas produtivas a `PerguntaDisputa.abrir`, mais 1 inserção física no próprio gate.** A outra classe `Pergunta` encontrada é `dataclass` de catálogo, sem ORM (`app/chamados/catalogo/perguntas.py:14`, `:17`, `:18`); o modelo persistente é `PerguntaDisputa`, com `disputa` FK obrigatória (`app/chamados/models.py:1849`), e a disputa aponta para `chamado` opcional (`app/chamados/models.py:1324`).

| Sítio / função | Escolha da disputa/chamado pai | Destino escolhido | Terminal no nascimento |
|---|---|---|---|
| `app/chamados/models.py:2085` — `PerguntaDisputa.abrir`, assinatura `:2028` | Recebe `disputa` do caller; não escolhe nem testa `fechada_em`/`disputa.chamado.status_local` (`:2036`, `:2085`). Dedup de fato pode devolver pergunta de outra disputa, sem trocar `ex.disputa`: consulta `do_fato(...).last()` em `:2061`, retorna em `:2084`; identidade filtra apenas colaborador, motivo, data (`:2022`). | Recebe `chamado_destino`; `resolvedora_valida` é chamada em `:2043-2051` e verifica datas/turno, sem status (`app/chamados/juizes.py:1702-1738`). | **Sim**, ambos os vínculos podem apontar para pais terminais se fornecidos; gate não impõe vitalidade. |
| `app/chamados/services/disputa_emissao.py:389` — `gerar_perguntas_disputa`, definição `:280` | Recebe disputa; fluxo usual `criar_ou_atualizar_disputa` chama núcleo em `:598` e gera perguntas em `:606`, `:618`. Núcleo escolhe `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:467-469`), **sem filtrar status do chamado pai**, retorna existente em `:502`. Sem disputa: `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao', status_local__in=VIVOS, defaults=...)` (`:504-519`), depois cria disputa com esse pai (`:523-528`). | `_derivar_chamado_destino` chamado em `:380`, `:382`; query exata: `ChamadoColaborador.objects.filter(colaborador=disputa.colaborador, status_local__in=VIVOS).exclude(categoria='disputa_supervisao')` (`:181-184`); filtros adicionais motivo atendido (`:194-196`), ausência de pergunta mesmo motivo não materializada (`:209-212`), dia (`:214-217`), desempate por turno (`:219-220`), exige candidato único (`:221-237`). Pode nascer sem destino e chamar reatamento depois (`:404-410`). | **Pai da disputa: sim**, ramo disputa existente só confere `fechada_em`; **destino derivado: terminais excluídos pela query VIVOS** (conclusão sobre elegibilidade, sem afirmar proteção contra corrida). |
| `app/chamados/services/disputa_emissao.py:770` — `_criar_pergunta_se_couber`, definição `:756` | Recebe disputa. No agrupador `criar_disputa_a_partir_de_chamados_orfaos`, há **dois ramos**: repasse escolhe `_pv = PerguntaDisputa.objects.filter(chamado_destino=ch, materializada_em__isnull=True).select_related('disputa').first()` (`:874`) e usa `_dv = _pv.disputa` (`:877`), **sem filtrar fechamento/status**, gerando irmãs em `:885` → `:813`; ramo geral usa `DisputaSupervisao.objects.filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:900-902`). Sem disputa, pai por `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao_manual', subcategoria='orfao_14h', status_local__in=VIVOS, defaults=...)` (`:908-923`), FK em `:931`. | Recebe destinos da lista do caller: intervalo `_chi` (`:956-958`); por ata `ch` (`:978-979`); grupo por batida escolhe `canonico = chamados_grupo[0]`, primeiro da ordem herdada (`:998-1001`), passado em `:1012-1018`; sem origem passa `ch` (`:1043-1049`); repasse passa `ch` (`:885`). Listas submetidas a `chamado_orfao_e_regularizavel` (`:870`, `:892`), que chama `motivo_fora_da_fabrica` (`:729`). Esse juiz exclui apenas `ENCERRADOS` (`:691-692`), não todos TERMINAIS. | **Disputa: pode estar fechada no repasse** (`:874-885`). **Chamado pai: pode ser terminal**, nenhum teste no reuso. **Destino: resolvido/fechado excluídos; cancelado/superado não excluídos pelo teste de status**, podendo passar demais guardas (`:689-720`). |
| `app/chamados/services/disputa_emissao.py:1160` — `criar_disputa_ancora_de_chamado`, definição `:1093` | `DisputaSupervisao.objects.select_for_update().filter(colaborador=colaborador, fechada_em__isnull=True).first()` (`:1121-1123`), sem status do chamado pai. Sem disputa: `ChamadoColaborador.abrir(colaborador=..., categoria='disputa_supervisao', modulo_origem='disputa_supervisao_manual', subcategoria='ancora_ausente', status_local__in=VIVOS, defaults=...)` (`:1128-1143`), FK em `:1151`. | Argumento `chamado_limbo`, exigindo categoria `ancora_limbo` (`:1107-1108`) e recusando apenas `status_local in ENCERRADOS` (`:1109-1110`); passado como destino em `:1166`. | **Pai da disputa: sim** no reuso; **destino: cancelado/superado permitidos pelo teste de status**, resolvido/fechado recusados. |
| `app/chamados/services/declaracao_texto.py:322` — `declarar_hora_colab`, definição `:257` | `disputa_para_declaracao(...)` (`:307-309`) → núcleo (`app/chamados/services/disputa_emissao.py:563-566`), mesma query/novo pai da linha de `gerar_perguntas_disputa` acima (`:467-528`). | Argumento `ch`, passado em `:323`. Função não filtra status; exige módulo de furo (`:271-272`), data e hora casando marco faltante (`:281-295`). | **Contrato da função: permite destino terminal**, pois não testa status. **HTTP atual protege**: `app/chamados/views.py:1046` pega por `pk,colaborador`, sem status, mas chama `resposta_tardia.receber` em `:1053`; retorna antes da declaração quando tardia (`:1054-1055`). `receber` deixa vivos seguir (`app/chamados/services/resposta_tardia.py:35-36`), ou renasce e atualiza objeto (`:45-47`), ou devolve `tardia` (`:67`). **Chamado pai da disputa continua podendo ser terminal**, por núcleo reutilizar disputa aberta sem testar pai. |
| `app/chamados/services/declaracao_texto.py:611` — `aplicar_declaracao`, definição `:589` | `disputa_para_declaracao(...)` (`:605-607`), mesmo núcleo/queries (`app/chamados/services/disputa_emissao.py:467-528`, `:563-566`). | Argumento `ch`, passado em `:612`. Propostas exigem módulo/dia/marco faltante, **sem status** (`app/chamados/services/declaracao_texto.py:551-568`). | **Sim**, pai da disputa e destino podem ser terminais. HTTP busca `get_object_or_404(ChamadoColaborador, pk=chamado_id, ticket_id_glpi__isnull=True)` (`app/chamados/views.py:363`), sem status; gestor chama aplicar em `:478-480`, sem `resposta_tardia` nesse ramo. |
| `app/chamados/services/acoes_disputa.py:259` — `acao_reabrir_disputa_admin`, definição `:232` | Query **explicitamente fechada**: `DisputaSupervisao.objects.filter(chamado=ch, fechada_em__isnull=False).order_by('-fechada_em').first()` (`:252`); reabre disputa antes (`:256`), via `reabrir_admin` que zera fechamento (`app/chamados/models.py:1701-1713`). | Não passa destino (`app/chamados/services/acoes_disputa.py:259-264`), portanto `None` pelo default (`app/chamados/models.py:2028`). | **Chamado pai pode estar resolvido/fechado no instante exato da criação**: `.abrir` ocorre em `:259`, `ch.reabrir()` apenas **depois**, em `:265-266`. Disputa já está reaberta. |

**Atenção ao significado de `ChamadoColaborador.abrir(... status_local__in=VIVOS)`: não garante retorno vivo em todos os ramos.** O gate primeiro faz `cls.objects.filter(colaborador=colaborador, **lookup).first()` (`app/chamados/models.py:670`), mas depois remove todos os filtros `status_local` (`:691`) e procura objetos com `contexto_json__has_key='agrupado_em'` (`:693-694`), retornando `_f` quando `agrupado_sob_pai_vivo()` (`:695-696`). Esse juiz verifica o status do **outro chamado referido em `agrupado_em`**, não do próprio `_f` (`:581`, `:588-596`). Assim, mesmo a obtenção de pai “novo” nos três emissores pode devolver um chamado terminal agrupado sob outro vivo. O comentário declara expressamente “devolve o filho” (`:689-690`), e a docstring identifica filho resolvido (`:575-580`).

Estados exatos: `VIVOS=('aberto','em_analise','registrado')`, `ENCERRADOS=('resolvido','fechado')`, `TERMINAIS=ENCERRADOS+('cancelado','superado')` (`app/chamados/catalogo/motor.py:24`, `:30-31`).

A enumeração negativa (“nenhum outro create/admin/SQL/serializer”) resulta de busca estática em todos os Python de produção e buscas por SQL/aliases/reverse manager; não há referência única de código capaz de provar ausência global **[não verificado]**. Os seis callers e a inserção física acima são os sítios concretos encontrados.

## 29/10 21:54 · sessao 77a2372554f7 · - · - · origem=Ronald (tmux)

**Mapa dos vínculos:** pergunta → `disputa` é FK obrigatória; o chamado pai é `pergunta.disputa.chamado`; existe também `pergunta.chamado_destino` opcional. Referências: `app/chamados/models.py:1849`, `app/chamados/models.py:1324`, `app/chamados/models.py:2283`.

| Leitor | Dependência e comportamento |
|---|---|
| Fila administrativa `qs_na_fila` | **Não lê perguntas diretamente**: toma `qs_vivos` (`status_local__in=VIVOS`) e restringe a `COBRANCA`, excluindo `silenciado_ate > now`. Portanto chamado terminal não entra nessa fila, mesmo tendo pergunta viva. `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:187`. `VIVOS`, `COBRANCA`, `TERMINAIS`: `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:31`. |
| Partição administrativa por verbos | `particionar_verbos` chama `creditos_perguntas`: `app/chamados/services/verbos.py:31`. Este busca perguntas cujo destino **ou** pai pertence ao queryset de chamados vivos: `app/chamados/services/raiox.py:54`. Usa `q_vivas`, exclui materializada com veredito `VL_SIM`: `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`. Credita ao destino se ele pertence ao universo vivo fornecido; senão ao pai: `app/chamados/services/raiox.py:140`. Se ambos terminais, pergunta fica fora do universo dessa consulta: inferência direta de `app/chamados/services/raiox.py:54`. |
| `fio.chamado_que_lista` | Retorna **sempre** `p.chamado_destino_id or p.disputa.chamado_id`; não consulta status de nenhum deles: `app/chamados/services/fio.py:116`, `app/chamados/services/fio.py:117`. |
| Montagem do fio admin | Busca todos os chamados do colaborador, sem filtro de status: `app/chamados/services/fio.py:174`. Busca disputas com chamado não nulo: `app/chamados/services/fio.py:181`. Carrega perguntas das disputas abertas e, no admin, perguntas candidatas a vivas das disputas fechadas; confirma estas com `pergunta_viva`: `app/chamados/services/fio.py:213`, `app/chamados/services/fio.py:218`, `app/chamados/services/fio.py:231`. Agrupa pela âncora `chamado_que_lista` e anexa ao card correspondente: `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:249`. Assim, pai terminal não é excluído só pelo status no fio admin; destino existente ganha a âncora mesmo terminal: `app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:117`. |
| Fio PWA do colaborador | `meu_atendimento` chama `montar_chamados_fio(..., so_janela=True)`: `app/chamados/views.py:566`, `app/chamados/views.py:569`. Nesse modo usa `chamados_visiveis_colab` e **não** inclui perguntas de disputas fechadas: `app/chamados/services/fio.py:169`, `app/chamados/services/fio.py:213`. Os formulários usam as perguntas anexadas ao card e `p.viva`: `app/templates/chamados/partials/_card_atendimento.html:73`, `app/templates/chamados/partials/_card_atendimento.html:77`. |
| Janela dos chamados do colaborador | Estende a janela com IDs de **destinos**, não pais, de perguntas `q_vivas`: `app/chamados/services/janela_colab.py:61`. Consulta chamados por janela OR esses destinos, colaborador, `silenciado_ate IS NULL`, excluindo módulos internos. **Não há filtro de status atual nessa função**: `app/chamados/services/janela_colab.py:67`, `app/chamados/services/janela_colab.py:70`. Portanto não reproduzir literalmente comentários antigos dizendo que todo terminal some da lista. |
| API nativa `/chamados` (`api_chamados_v2`) | Usa a mesma janela: `app/api/views.py:1015`. Considera apenas disputas abertas: `app/api/views.py:1028`. Sua âncora difere do fio: destino vence somente quando `status_local in VIVOS`; caso contrário usa `p.disputa.chamado_id`: `app/api/views.py:1043`, `app/api/views.py:1047`. Serializa perguntas por card presente na lista: `app/api/views.py:1122`, `app/api/views.py:1158`, `app/api/views.py:1177`. Campo `pode_responder` é falso para `ENCERRADOS`: `app/api/views.py:1155`. |
| Questionário separado do app (`api_disputa_perguntas`) | Usa `perguntas_do_app`: `app/api/views.py:2603`. A seleção filtra colaborador por disputa, `q_vivas` e `pergunta_viva`, **sem exigir disputa aberta nem pai vivo**: `app/chamados/services/pergunta_no_app.py:42`. Cada item informa `chamado_id=p.chamado_destino_id`: `app/api/views.py:2626`. O `chamado_id` do envelope é o primeiro destino não nulo de pergunta não RESPONDIDA, ou o pai da primeira disputa; **não valida status desse destino/pai**: `app/api/views.py:2615`, `app/api/views.py:2642`. |
| Detalhe local administrativo | Resolve apenas disputa aberta vinculada diretamente ao chamado exibido; depois filtra suas perguntas por `pergunta_viva`. Não usa âncora destino para selecionar a disputa no GET: `app/chamados/services/acoes_chamado.py:335`, `app/chamados/services/acoes_chamado.py:341`, `app/chamados/services/acoes_chamado.py:348`. |
| Portas administrativas que consomem a listagem | Validar/recusar/corrigir no detalhe verificam igualdade com `chamado_que_lista`, exigindo disputa aberta: `app/chamados/views.py:493`, `app/chamados/views.py:499`, `app/chamados/views.py:510`, `app/chamados/views.py:516`, `app/chamados/views.py:528`, `app/chamados/views.py:534`. No fio, validação permite disputa fechada se pergunta continua viva: `app/chamados/services/fio.py:589`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:594`, `app/chamados/services/fio.py:599`; recusar/corrigir ainda exigem disputa aberta: `app/chamados/services/fio.py:626`. |
| Fila de validação em lote / IA | `chamado_da_pergunta` também retorna destino ou pai, sem status: `app/chamados/services/validacao.py:786`. Consumidores: montagem do lote `app/chamados/services/validacao.py:869` e resposta da API de mensageria `app/api/views_mensageria.py:542`, `app/api/views_mensageria.py:546`. |
| Juiz de irmãs com resposta pendente | Seleciona pergunta por destino `=ch` OU pai `=ch` quando destino não está em `VIVOS`; depende expressamente da mesma regra de crédito: `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:1474`. |

**Conclusão para o relatório:** há três regras de âncora diferentes: fio/lote usam destino não nulo ou pai; crédito administrativo usa destino pertencente ao universo vivo ou pai; API nativa usa destino de status VIVOS ou pai. Referências respectivas: `app/chamados/services/fio.py:117`, `app/chamados/services/validacao.py:788`, `app/chamados/services/raiox.py:144`, `app/api/views.py:1047`.

## 29/10 21:54 · sessao 293a5aca92dc · - · - · origem=Ronald (tmux)

Evidências para o item 3:

- **Há seletor explícito de chamado vivo do dia:** `cobranca_viva_do_dia(colaborador, modulo_origem, dia, excluir=None)`, em `app/chamados/juizes.py:1610`. Filtra `colaborador`, `modulo_origem__in` e `status_local__in=VIVOS`; restringe a `contexto_json__data_turno=dia.isoformat()` **ou** `celula__data=dia`; ordena por `-criado_em, -pk`; exclui opcionalmente o próprio chamado; retorna o primeiro para o qual `dia_do_fato(c, fortes=True) == dia` (`app/chamados/juizes.py:1620–1626`).
- Seus chamadores diretos de produção são `_gate_premissa_do_fechamento`, em `app/chamados/models.py:498`; `sobrevivente_por_ultimo`, em `app/chamados/juizes.py:1644`; `_autoresolve_f2_f4`, em `app/ponto/services/triagem_batida.py:589`; e `resolver_cobranca_da_entrada`, em `app/ponto/services/entrada_sincronizada.py:28`.
- `sobrevivente_por_ultimo` põe o chamado escolhido por aquele juiz por último no lote (`app/chamados/juizes.py:1641–1647`). É chamado por `fechar_o_fio_do_dia` (`app/ponto/services/veredito_celula.py:182`), pelo cartório (`app/ponto/services/cartorio.py:1196`) e pelo serviço de cobertura (`app/ponto/services/cobertura.py:310`).

**Status declarados:**

```python
VIVOS      = ('aberto', 'em_analise', 'registrado')
COBRANCA   = ('aberto', 'em_analise')
ENCERRADOS = ('resolvido', 'fechado')
TERMINAIS  = ENCERRADOS + ('cancelado', 'superado')
```

A declaração está em `app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`. O registro de juízes formula C1 como “o chamado está vivo?” (`app/core/juizes.py:107`) e aponta a autoridade para `chamados/catalogo/motor.py::VIVOS` (`app/core/juizes.py:227`); separa a pergunta sobre o fio em C15 (`app/core/juizes.py:123–125`, `:241`).

**Há função que testa terminalidade**, embora local ao serviço de becos: `_resolv(ch)` retorna `ch is not None and getattr(ch, 'status_local', None) in RESOLVIDO` (`app/chamados/services/becos.py:21–22`), sendo `RESOLVIDO = TERMINAIS` (`app/chamados/services/becos.py:11–13`). Portanto, não cabe afirmar ausência absoluta de função “pai terminal”.

- `classificar(d)` usa `_resolv` para destinos das perguntas (`app/chamados/services/becos.py:30`) e para `d.chamado`, o pai (`:31`).
- `fechar_beco(d, sistema)` usa para o pai (`app/chamados/services/becos.py:43`) e destinos (`:46`).
- `classificar` é chamado pelo wrapper `fechar_beco_da_disputa` (`app/chamados/services/becos.py:69`) e pelo comando `fechar_becos_disputa` (`app/chamados/management/commands/fechar_becos_disputa.py:39`).
- `fechar_beco` é chamado pelo wrapper (`app/chamados/services/becos.py:73`) e pelo comando (`app/chamados/management/commands/fechar_becos_disputa.py:57`).
- O wrapper é chamado após commit pelo signal `post_save` de `PerguntaDisputa` (`app/chamados/signals.py:191–202`).

Outros seletores do dia existentes:

- `chamados_vivos_do_dia(colaborador, data, cel=None)` reúne chamados com `status_local__in=VIVOS` e `contexto_json__data_turno=data.isoformat()`, somando os `cel.chamados` também vivos sem duplicar (`app/ponto/services/veredito_celula.py:136–157`). Chamador direto: `fechar_o_fio_do_dia`, `app/ponto/services/veredito_celula.py:182`.
- `_chamados_vivos_do_dia(titular, dia)` percorre chamados do titular em `VIVOS`, ordenados por `-criado_em,-pk`; aceita data da célula igual ao dia ou, sem data de célula, a data declarada pelo catálogo (`app/ponto/services/cobertura.py:218–231`). Chamador: `app/ponto/services/cobertura.py:310`.
- `agrupado_sob_pai_vivo` é específico do pai registrado em `contexto_json.agrupado_em`: busca por PK, verifica mesmo colaborador e retorna `pai.status_local in VIVOS` (`app/chamados/models.py:581–596`). Seus leitores de produção: fechamento (`app/chamados/models.py:495`), dedup de abertura (`:695`), impedimento de reabertura (`:1012`), badge do espelho (`app/escala/utils.py:1036`) e calendário do colaborador (`app/colaboradores/services/calendario.py:556`).

## 29/10 21:48 · sessao 613a61b3159d · piloto-desligamento · leitura · origem=Code

A leitura mostra dois limites importantes: **o resumo do espelho pode usar a competência inteira, mesmo quando os dias foram limitados por uma data**, e **o saldo cadastrado de férias não calcula proporcionais até essa data**. São comportamentos distintos no código, detalhados abaixo. (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1288`; `app/ferias/models.py:216`.)

A competência contendo a DATA é obtida por `app/ponto/janelas.py::janela_atual(data, empresa=None)`. O corte vem de `Empresa.dia_inicio_competencia`, com padrão 21 e limite efetivo entre 2 e 28; `janela_fechamento(mes, ano, empresa=None)` identifica a competência pelo mês/ano do **fim**. Com corte 21, retorna dia 21 do mês anterior até dia 20 do mês informado. ([janelas.py:10](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ponto/janelas.py:10), `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`.)

**B1 — Espelho apurado: há uma fachada atual, mas ela não garante o resumo parcial até a DATA.** (`app/ponto/services/espelho.py:1198`, `app/ponto/services/espelho.py:1288`.)

- **FONTE:** `app/ponto/services/espelho.py::espelho_do_colab(colaborador, hoje, apur_ini, apur_fim, *, ano=None, modo_gestor=False, agora)`. Retorna, entre outras chaves, `dias`, `resumo` e `resultado`. É exatamente a função chamada pela tela atual, que publica `dias` e `resumo_mes`. ([espelho.py:1198](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ponto/services/espelho.py:1198), `app/ponto/services/espelho.py:1298`, `app/ponto/views.py:154`, `app/ponto/views.py:158`.)
- **UNIDADE:** trabalhadas, extras, atrasos e `horas_falta` são **horas decimais**; `dias_falta` é contagem de dias e `datas_falta` é lista de datas. A tela formata as horas com uma casa decimal seguida de `h`, não em h:min. (`app/relatorios/cartao_pela_celula.py:93`, `app/relatorios/cartao_pela_celula.py:102`, `app/relatorios/cartao_pela_celula.py:104`, `app/relatorios/cartao_pela_celula.py:111`; `app/templates/ponto/espelho.html:38`, `app/templates/ponto/espelho.html:62`.)
- **UNIVERSO:** um colaborador explícito. Os dias visuais vão de `min(piso_visual(colaborador, hoje), apur_ini)` até `hoje`; portanto podem começar **antes da competência**. O resumo inicial restringe as rubricas aos períodos cuja entrada local pertence a `[apur_ini, apur_fim]`. Depois, se essas pontas forem uma competência completa, `folha_manda` aplica os totais dessa competência. (`app/ponto/services/espelho.py:1229`, `app/ponto/services/espelho.py:1266`, `app/ponto/services/espelho.py:83`, `app/ponto/services/espelho.py:450`; `app/relatorios/cartao_pela_celula.py:321`.)
- **EXCLUSÕES:** a apuração exclui batidas retratadas; pendentes do DP continuam entrando. Na fonte final folha/lavratura, `total_trabalhadas` usa horas normais, enquanto folga trabalhada fica separada em `trab_folga`. Furo apurado sem decisão não se torna automaticamente falta decidida. (`app/ponto/turnos.py:2818`, `app/ponto/turnos.py:2838`; `app/relatorios/cartao_pela_celula.py:111`, `app/relatorios/cartao_pela_celula.py:116`, `app/relatorios/cartao_pela_celula.py:174`, `app/relatorios/cartao_pela_celula.py:373`.)

A fonte diária e a fonte do resumo são diferentes:

| Parte | Fonte efetiva e limite |
|---|---|
| Registros por dia | `app/ponto/services/dia_pago.py::linhas_do_periodo(colab, ini, fim, *, versao='motor')`: mapa `{date: DiaPago}`, somente linhas `tipo=dia`, com datas inclusivas. (`app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:852`.) |
| Rubricas que o HTML imprime por dia | `app/ponto/services/dia_pago.py::rubricas_do_dia(linha)`: trabalhadas, extras 50/100, atraso, antecipada, noturnas e intra, em horas. **Não retorna `horas_falta`**. Sem linha, sinaliza `lavrada=False`; o template informa ausência de apuração. (`app/ponto/services/dia_pago.py:899`, `app/ponto/services/dia_pago.py:908`, `app/ponto/services/dia_pago.py:914`; `app/templates/ponto/espelho.html:263`.) |
| Outra medida diária disponível | `app/ponto/services/dia_pago.py::pago_do_dia(linha)`: trabalhadas + folga trabalhada + folga-feriado; pode retornar `None`. Alimenta `dia.pago_h`, enquanto a linha de rubricas do HTML imprime `rubricas.trabalhadas`. (`app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`, `app/ponto/services/dia_pago.py:895`; `app/ponto/services/espelho.py:822`; `app/templates/ponto/espelho.html:268`.) |
| Resumo final | `app/relatorios/cartao_pela_celula.py::folha_manda(colaborador, data_ini, data_fim_pedido, resumo)`: tenta folha, depois lavratura; sem ambas, conserva o resumo do motor com `fonte_dos_totais='sem apuracao ainda'`. (`app/relatorios/cartao_pela_celula.py:296`, `app/relatorios/cartao_pela_celula.py:321`, `app/relatorios/cartao_pela_celula.py:340`.) |

**Corte e relógio:** a fachada recebe `hoje` e `agora`, mas não os transmite a toda a cadeia. `folha_manda` recebe as pontas da competência, sem `hoje`; `fechamento_lido` recebe apenas mês/ano e o recalculador captura seu próprio `timezone.now()`. A construção dos dias em aberto também chama `ate_ontem` e `turno_aberto_de` sem injetar o relógio da fachada. Portanto, passar a DATA em `hoje` **não garante** totais e estados inteiramente avaliados nesse corte. (`app/ponto/services/espelho.py:1288`; `app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`, `app/ponto/services/fechamento.py:595`; `app/relatorios/cartao_pela_celula.py:37`, `app/relatorios/cartao_pela_celula.py:398`, `app/relatorios/cartao_pela_celula.py:412`.)

Há outras candidatas, com contratos diferentes:

| Candidata e assinatura | O que responde |
|---|---|
| `app/ponto/services/dia_pago.py::soma_do_periodo(colaboradores, ini, fim, *, versao='motor')` | **Totais lavrados até um corte explícito**, por colaborador, incluindo trabalhadas, extras, atraso e `horas_falta`. Retorna também `sem_lavratura` e `sem_apuracao`. Exclui ajustes de competência, DSR e banco; não consulta relógio no corpo. É a soma por intervalo, mas não reproduz a seleção de fontes do resumo atual. (`app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:750`, `app/ponto/services/dia_pago.py:782`, `app/ponto/services/dia_pago.py:828`.) |
| `app/ponto/services/espelho.py::autoridade_do_periodo(colaborador, ini, fim)` | Resultado do motor para intervalo explícito, utilizado pela fachada; sozinho não é o resumo final da tela. (`app/ponto/services/espelho.py:982`, `app/ponto/services/espelho.py:1195`, `app/ponto/services/espelho.py:1231`.) |
| `app/ponto/services/espelho.py::somar_periodos(periodos, periodos_ft, dentro=None)` | Soma rubricas do motor, em horas; aceita filtro de intervalo, não soma faltas e não escolhe folha/lavratura. (`app/ponto/services/espelho.py:51`, `app/ponto/services/espelho.py:77`, `app/ponto/services/espelho.py:117`.) |
| `app/ponto/services/fechamento.py::fechamento_lido(colaborador, mes, ano)` | Totais online da competência inteira; pode devolver `None`, sem parâmetro de corte. (`app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`.) |
| `app/relatorios/cartao_pela_celula.py::totais_da_folha(colab, mes, ano)` | Mapeia o fechamento online para o vocabulário do resumo; exige existir fechamento cadastrado. Sem corte livre. (`app/relatorios/cartao_pela_celula.py:44`, `app/relatorios/cartao_pela_celula.py:69`.) |
| `app/relatorios/cartao_pela_celula.py::_totais_da_lavratura(colab, mes, ano)` | Alternativa da competência inteira quando não há totais da folha. Usa `soma_da_competencia`, não a soma parcial. (`app/relatorios/cartao_pela_celula.py:126`, `app/relatorios/cartao_pela_celula.py:141`.) |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho(colaborador, data_ini, data_fim, _turnos_juiz=None, *, agora)` | Coletor do PDF: limita dias a ontem, lê a fachada e reaplica `folha_manda`; não é a função escolhida pela tela HTML. (`app/relatorios/pdf_espelho.py:187`, `app/relatorios/pdf_espelho.py:286`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:624`.) |

A própria seleção da janela difere entre consumidores: a web usa `janela_fechamento` para mês/ano do GET, com padrão no mês civil atual; a API usa `janela_atual` quando mês/ano coincidem com o mês civil atual. Ambas chamam `espelho_do_colab`. (`app/ponto/views.py:97`, `app/ponto/views.py:101`, `app/ponto/views.py:154`; `app/api/views.py:1362`, `app/api/views.py:1369`.)

**B2 — Faltas e ausências justificadas: não há uma saída única nas fontes identificadas que reúna os dois conjuntos com tipo.** Há lista de faltas decididas, cobertura por dia e lista de registros. (`app/folha/export.py:158`; `app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2707`.)

- **FONTE das faltas:** `app/folha/export.py::datas_falta_janela(colaborador, ini, fim)`. **UNIDADE:** lista de datas; a função não retorna horas nem o tipo junto de cada data. **UNIVERSO:** um colaborador, intervalo inclusivo informado, sem competência obrigatória. **EXCLUSÕES:** só entram ausências aprovadas de tipos que o catálogo vigente declara `DESCONTA`; ficam fora pendentes, rejeitadas, tipos abonados/suprimidos e furo sem decisão. Recebe as pontas e não consulta relógio no corpo. ([export.py:158](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/folha/export.py:158), `app/folha/export.py:75`, `app/folha/export.py:135`, `app/folha/export.py:168`.)
- **FONTE por dia, com tipo:** `app/ponto/turnos.py::cobertura_ausencia_periodo(colab, ini, fim, *, para)`. **UNIDADE:** mapa `{date: Ausencia}`, com `tipo`, status e minutos no registro. **UNIVERSO:** um colaborador e dias inclusivos do intervalo, expandindo dias únicos, intervalos fechados e abertos permitidos. **EXCLUSÕES:** `para='folha'` aceita só aprovadas; `'cobranca'` exclui apenas rejeitadas. Em sobreposição, conserva somente a ausência de maior `(data, pk)`. Não restringe automaticamente a “justificadas”; também pode devolver falta ou ausência que suprime o dia. Recebe as pontas, sem relógio no corpo. ([turnos.py:2707](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ponto/turnos.py:2707), `app/ponto/turnos.py:2475`, `app/ponto/turnos.py:2727`, `app/ponto/turnos.py:2743`; `app/ponto/models.py:749`.)
- **FONTE de todos os registros com tipo:** `app/ponto/turnos.py::ausencias_do_periodo(colab, ini, fim, *, para)`. Retorna queryset dos registros que intersectam o intervalo, com o mesmo filtro folha/cobrança, **sem eliminar sobreposições**. Não retorna uma contagem pronta de dias nem filtra apenas justificadas; recebe datas, sem relógio no corpo. (`app/ponto/turnos.py:2487`.)

As candidatas próximas e suas diferenças são:

| Candidata e assinatura | Diferença |
|---|---|
| `app/folha/export.py::datas_falta(colaborador, empresa, mes, ano)` | Mesma pergunta das faltas, pela competência inteira da empresa; delega a `datas_falta_janela`. Não recebe corte parcial. (`app/folha/export.py:173`.) |
| `app/folha/export.py::ausencias_na_janela(colaborador, tipos, ini, fim)` | Registros aprovados dos tipos pedidos, preservando tipo; aceita corte explícito. É utilizada pela expansão das faltas. (`app/folha/export.py:60`, `app/folha/export.py:149`.) |
| `app/folha/export.py::datas_abonadas(colaborador, empresa, mes, ano)` | Datas da rubrica 8932, **sem tipo no retorno**. Exclui férias, declaração e licença maternidade; portanto não é lista geral de justificadas. Só recebe competência. (`app/folha/export.py:182`, `app/folha/export.py:190`; `app/ponto/catalogo/ausencias.py:290`.) |
| `app/ponto/services/efeito_ausencia.py::efeitos_da_ausencia(colaborador, data_ini, data_fim)` | Conjuntos de justificadas/suprimidas/dia inteiro e minutos parciais. Inclui `Justificativa` aprovada, além de ausências; não devolve tipo. (`app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:36`, `app/ponto/services/efeito_ausencia.py:59`.) |
| `app/ponto/services/dia_decidido.py::decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)` | Histórico por dia com tipo/status/minutos, **incluindo rejeitadas**. Sobreposições têm desempate; registro sem fim expande só o dia inicial nesse corpo. (`app/ponto/services/dia_decidido.py:504`, `app/ponto/services/dia_decidido.py:521`, `app/ponto/services/dia_decidido.py:542`.) |
| `app/ponto/turnos.py::ausencias_vigentes(de, ate, colaborador=None)` e `::ausencias_do_dp(de, ate, colaborador=None)` | A primeira não filtra status/tipo; a segunda exclui férias e rejeitadas. Omitir colaborador permite universo coletivo. Não equivalem a justificadas aprovadas. (`app/ponto/turnos.py:2921`, `app/ponto/turnos.py:2944`, `app/ponto/turnos.py:2948`.) |

**O espelho atual usa várias delas:** cobertura `'cobranca'` para o ícone, cobertura `'folha'` para o rótulo, `decididos_do_periodo` para decisões e `efeitos_da_ausencia` na alimentação do motor. As faltas decididas do resumo passam por `folha_manda → datas_falta_janela`. (`app/ponto/services/espelho.py:590`, `app/ponto/services/espelho.py:599`, `app/ponto/services/espelho.py:658`, `app/ponto/services/espelho.py:1072`; `app/relatorios/cartao_pela_celula.py:373`.)

**B3 — Férias vencidas e proporcionais: as fontes localizadas não atendem o bloco completo.** Há saldo cadastrado, status por data e avisos; cálculo de proporcionais/avos até o corte não foi localizado. A inexistência absoluta de outra implementação permanece **[não verificado]**.

- **FONTE do saldo por aquisitivo:** `app/ferias/models.py::PeriodoAquisitivo.dias_saldo(self)`, uma **property**. **UNIDADE:** dias. **UNIVERSO:** aquele período aquisitivo cadastrado, sem competência mensal. **EXCLUSÕES:** subtrai concedidos e vendidos. Não recebe data, não calcula proporcionalidade nem reconstrói saldo histórico: retorna `dias_direito - dias_concedidos - dias_vendidos`. ([models.py:216](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ferias/models.py:216), `app/ferias/models.py:119`, `app/ferias/models.py:124`.)
- **FONTE da classificação na DATA:** `app/ferias/models.py::status_de_periodo(pa, hoje=None)`. **UNIDADE:** categoria textual. Saldo não positivo resulta em `concedido`; fim do aquisitivo maior ou igual à referência resulta em `em_curso`; depois, limite de concessão menor que a referência resulta em `vencido`; o restante é `adquirido`. Recebe data; se omitida, usa `date.today()`. Avalia os contadores atuais de saldo antes das datas. ([models.py:65](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ferias/models.py:65), `app/ferias/models.py:94`.)
- **FONTE agregada:** `app/ferias/situacao.py::situacao_de_ferias(colab, hoje=None)`. Retorna `saldo_total` em dias, períodos, agendamentos e avisos. Busca todos os aquisitivos do colaborador, sem recorte de competência ou de início até a DATA; recebe referência ou usa `date.today()`. O aviso de vencido exige limite de concessão `< hoje` e saldo positivo. ([situacao.py:138](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/ferias/situacao.py:138), `app/ferias/situacao.py:155`, `app/ferias/situacao.py:162`, `app/ferias/situacao.py:192`.)

Há uma diferença entre a descrição e o corpo de `situacao_de_ferias`: embora a docstring fale em períodos já adquiridos, `saldo_total` soma **todos os períodos com saldo positivo**, sem excluir em curso ou futuros cadastrados. O retorno também não separa um total numérico exclusivo de vencidas; o aviso textual mostra no máximo três períodos. (`app/ferias/situacao.py:146`, `app/ferias/situacao.py:200`, `app/ferias/situacao.py:296`.)

As outras candidatas não calculam proporcionais:

- `app/ferias/models.py::vencendo(pa, dias=DIAS_VENCENDO, hoje=None)` retorna booleano para adquirido próximo do limite; exclui vencidos por exigir `adquirido`. `app/ferias/services.py::periodos_vencendo(hoje=None, dias=JANELA_VENCIMENTO_DIAS, colaborador=None, empresa=None)` retorna queryset por **status armazenado**, com universo coletivo quando colaborador é omitido. (`app/ferias/models.py:252`; `app/ferias/services.py:111`.)
- `app/ferias/models.py::dias_direito_por_faltas(faltas)` aplica faixas de direito; `PeriodoAquisitivo.faltas_periodo(self)` conta registros aprovados do tipo literal `falta`, pela data inicial dentro do aquisitivo. Não expande intervalos nem calcula avos. (`app/ferias/models.py:52`, `app/ferias/models.py:140`.)
- `app/ferias/views.py::saldo_ferias(request, cid)` lê o relógio e sincroniza os períodos, em cadeia que pode salvar alterações; não é um leitor puro. O espelho de ponto usa a cobertura diária de férias, sem agregar saldo por aquisitivo nesse fluxo. (`app/ferias/views.py:98`, `app/ferias/views.py:108`, `app/ferias/views.py:113`; `app/ferias/services.py:238`; `app/ponto/services/espelho.py:590`.)

**B4 — O que segura a folha: existe a função individual; motivos e portas não vêm numa fonte única completa.** (`app/folha/fora_da_folha.py:179`; `app/folha/retrato_folha.py:974`.)

- **FONTE:** `app/folha/fora_da_folha.py::o_que_segura(colab, mes, ano)`. Retorna `entra`, `motivo`, `grupo`, `motivos_retencao` e lista de dias com data/veredito/motivo/fio/ação/IDs de chamados. **Não retorna URLs**, nem as travas de aprovação. ([fora_da_folha.py:179](/home/ronald/codex_ws/2b13d209d948b4af36015ad7b7ed815c024ccaee/app/folha/fora_da_folha.py:179), `app/folha/fora_da_folha.py:209`, `app/folha/fora_da_folha.py:211`.)
- **UNIDADE:** categorias, datas ISO e IDs; `dias` é uma lista, não horas ou dinheiro. `entra` contém o status textual da classificação, podendo ser `None` quando não existe item para o colaborador. (`app/folha/fora_da_folha.py:191`, `app/folha/fora_da_folha.py:209`, `app/folha/fora_da_folha.py:212`.)
- **UNIVERSO:** um colaborador, competência inteira da empresa. A classificação geral vem dos fechamentos cadastrados para empresa/mês/ano; os dias são consultados separadamente, mesmo quando o motivo geral não é `furo_espelho`. (`app/folha/fora_da_folha.py:188`, `app/folha/fora_da_folha.py:193`; `app/folha/export.py:764`.)
- **CORTE/RELÓGIO:** não recebe a DATA. A autoridade diária é `app/folha/export.py::motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)`, mas a fachada não injeta `hoje`, levando ao uso de `timezone.localdate()`. Nessa autoridade, `hoje` distingue passado para ausência de célula/veredito; **não limita todos os motivos**, pois vereditos acusadores não passam pelo teste `d < hoje`. (`app/folha/fora_da_folha.py:90`; `app/folha/export.py:1150`, `app/folha/export.py:1179`, `app/folha/export.py:1213`, `app/folha/export.py:1215`.)
- **EXCLUSÕES diárias:** isentos; dias anteriores à admissão/posteriores à demissão; dias sem célula e sem vínculo; ausência de célula/veredito em hoje/futuro ou no dia da demissão; protestos integralmente não cobráveis; furo exclusivamente da pausa nas condições específicas da função. Uma célula acusadora no dia da demissão ainda pode reter. (`app/folha/export.py:1174`, `app/folha/export.py:1180`, `app/folha/export.py:1209`, `app/folha/export.py:1211`, `app/folha/export.py:1235`, `app/folha/export.py:1237`.)

Os **motivos gerais do TXT**, em ordem de precedência, são estes. A classificação para no primeiro aplicável; portanto não enumera todos os problemas gerais simultâneos. (`app/folha/export.py:746`, `app/folha/export.py:840`, `app/folha/export.py:948`.)

| Motivo | Porta existente comprovada |
|---|---|
| `rescisao_modulo_proprio` | O ramo orienta ao módulo de rescisão do Domínio, fora do TXT mensal; não fornece URL Django. (`app/folha/export.py:840`.) |
| `sem_codigo_dominio` | `/colaboradores/<cid>/editar/` → `colaboradores.views::editar_colaborador`; a gravação atribui `codigo_dominio`. (`app/folha/export.py:846`; `app/colaboradores/urls.py:50`; `app/colaboradores/views.py:1334`.) |
| `ferias_com_batida` | Não há escolha única codificada. Existem `/ferias/agendamento/<ag_id>/editar-datas-admin/` → `ferias.views::editar_datas_admin`, cancelamento do agendamento e `/ponto/batida/<batida_id>/retratar/` → `ponto.views::retratar_

[truncado em 20000 de 25389 caracteres]

## 29/10 21:46 · sessao e24e3f8ecbfa · - · - · origem=Ronald (tmux)

B4 tem fonte pronta para “o que segura”, mas a resposta e as portas de resolução estão em funções distintas; o mapa de portas é explicitamente parcial. `o_que_segura` devolve textos de ação e IDs de chamados, sem URLs. (`app/folha/fora_da_folha.py:179-213`; `app/folha/retrato_folha.py:974-1025`)

**FONTE e assinatura**

- `app/folha/fora_da_folha.py::o_que_segura(colab, mes, ano)`: retorna competência, colaborador/nome, `entra` (valor textual do `status`, não booleano), `motivo`, `grupo`, `motivos_retencao` e lista `dias`; cada dia tem `data`, `veredito`, `motivo`, `fio`, `destrava`, `chamado`. (`app/folha/fora_da_folha.py:179-213`)
- A classificação de entrada vem de `app/folha/export.py::classificar_export(empresa, mes, ano, fechamentos=None)`, buscando o item daquele colaborador entre os fechamentos da empresa; quando o colaborador não aparece, `entra`/`motivo` ficam `None`. (`app/folha/fora_da_folha.py:188-193`, `app/folha/fora_da_folha.py:211-213`; `app/folha/export.py:746-769`)
- Os dias vêm da autoridade `app/folha/export.py::motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)`, chamada com `com_dias=True`. (`app/folha/fora_da_folha.py:90-94`; `app/folha/export.py:1150`, `app/folha/export.py:1200-1240`)

**UNIDADE**

É uma resposta categórica com **lista de dias ISO**, motivos e IDs, sem valor monetário ou total de horas; a função não traz contador `dias`, mas uma lista. (`app/folha/fora_da_folha.py:209-213`)

**UNIVERSO, corte e relógio**

- Um colaborador na competência `mes/ano`, com janela completa da empresa obtida por `janela_fechamento`; não recebe data de último dia trabalhado. (`app/folha/fora_da_folha.py:179`, `app/folha/fora_da_folha.py:188-193`)
- A janela usa `Empresa.dia_inicio_competencia` por `corte_da_empresa`, default 21 e clamp 2–28. (`app/ponto/janelas.py:36-57`)
- A retenção diária recorta início pela admissão e fim pela demissão; janela colapsada retorna vazia. (`app/folha/export.py:1180-1188`)
- `o_que_segura` não injeta `hoje`, portanto a retenção lê `timezone.localdate()` internamente. O núcleo diário permite receber `ini`, `fim` e `hoje`, mas `hoje` serve para decidir se um dia sem célula/veredito é passado: **não é teto geral de corte**, porque o ramo de vereditos acusadores não testa `d < hoje`. (`app/folha/fora_da_folha.py:90-94`; `app/folha/export.py:1179`, `app/folha/export.py:1206-1238`)
- Mesmo que o motivo geral do TXT seja cadastro, férias ou rescisão, `o_que_segura` também busca os dias por célula; não condiciona a lista diária a `motivo == furo_espelho`. (`app/folha/fora_da_folha.py:191-194`)

**EXCLUSÕES diárias**

- Isento de ponto: nenhum motivo/dia. (`app/folha/export.py:1173-1178`)
- Dias fora de admissão/demissão; sem célula **e** sem vínculo cobrindo o dia; ausências de célula/veredito no próprio dia da demissão; ausência de célula/veredito em hoje ou futuro. (`app/folha/export.py:1180-1187`, `app/folha/export.py:1209-1214`, `app/folha/export.py:1237-1238`)
- Dia acusador com protestos todos em `NAO_COBRAVEIS_DO_COLABORADOR`; hoje esse conjunto contém apenas `FURO_PRE_ADOCAO`. (`app/folha/export.py:1227-1236`; `app/ponto/supra_juiz.py:34-36`)
- Furo exclusivamente de pausa com protestos exatamente `{'FURO_PARCIAL'}` e confirmação do juiz de marcos; entrada/saída abertas, desalinhamento ou desconhecimento não recebem essa isenção. (`app/folha/export.py:1122-1129`, `app/folha/export.py:1132-1147`, `app/folha/export.py:1235-1236`)

**Todos os motivos gerais do TXT e portas encontradas**

| Motivo | Condição e porta existente comprovada |
|---|---|
| `rescisao_modulo_proprio` | Qualquer `data_demissao` preenchida exclui do TXT mensal; o detalhe manda para módulo de RESCISÃO do Domínio. Não há URL Django fornecida nesse ramo. (`app/folha/export.py:840-845`) |
| `sem_codigo_dominio` | Código vazio; cadastro pode ser editado em `/colaboradores/<cid>/editar/` → `colaboradores.views::editar_colaborador`, cujo formulário contém `codigo_folha` e cuja gravação atribui `codigo_dominio`. (`app/folha/export.py:846-849`; `app/colaboradores/urls.py:50`; `app/templates/colaboradores/editar.html:62`; `app/colaboradores/views.py:1237`, `app/colaboradores/views.py:1334`) |
| `ferias_com_batida` | Datas de batida dentro de férias não canceladas; exclui batidas retratadas. Não há porta única no mapa: tipo `ferias` é `None`. Existem portas específicas de editar datas/cancelar agendamento, exigindo `ag_id`, e retratar batida, exigindo `batida_id`. Escolher qual ato resolve cada caso não está codificado em `o_que_segura`. (`app/folha/export.py:453-494`, `app/folha/export.py:850-858`; `app/folha/retrato_folha.py:993`; `app/ferias/urls.py:18-22`; `app/ponto/urls.py:61`) |
| `cadastro_zero` | Fechamento zero, ≥2 batidas apuráveis e nenhum previsto na janela; o detalhe manda cadastrar vínculo/escala e recalcular. Vínculo: `/colaboradores/vincular/<cid>/painel/` → `painel_vinculo`; gravação `/colaboradores/<cid>/vincular/escala/` → `vincular_escala`; recálculo POST `/ponto/fechamento/recalcular/` → `recalcular_fechamento`. (`app/folha/export.py:876-940`; `app/colaboradores/urls.py:12-13`; `app/ponto/urls.py:31`; `app/ponto/views.py:1227-1254`) |
| `furo_espelho` | Algum motivo diário da autoridade de célula; encaminhamento depende do fio/causa, não apenas deste rótulo. O raio-X atual aponta `/chamados/painel/?colab_id=<cid>`; há também a porta de decidir fato do dia POST `/ponto/celula/veredito/` → `veredito_celula_view`, com `colab`, `datas`/`data`, `veredito` e opcional `tipo_ausencia`. (`app/folha/export.py:942-947`; `app/folha/views.py:602`, `app/folha/views.py:667`; `app/ponto/urls.py:58`; `app/ponto/views.py:3302-3331`) |
| `nao_homologado` | Códigos de eventos fora do catálogo Domínio; porta de rubricas `/folha/rubricas/` → `folha.views::lista_rubricas`, com vincular/desvincular evento/código por empresa. O mapa do retrato classifica como cadastro e abre cadastro do colaborador, não fornece um link específico de rubricas por este motivo. (`app/folha/export.py:948-953`; `app/folha/urls.py:8`; `app/folha/views.py:27-55`; `app/folha/retrato_folha.py:211-213`, `app/folha/retrato_folha.py:992`) |

As URLs acima recebem os prefixos do projeto em `app/config/urls.py:26-34`.

A ordem de precedência do motivo geral é a da tabela: a função sai no primeiro ramo aplicável; portanto não é inventário completo de todos os motivos gerais simultâneos. (`app/folha/export.py:840-858`, `app/folha/export.py:933-953`)

**Todos os motivos diários e portas por fio**

Os motivos possíveis são `sem_celula` e `espelho_` concatenado aos sete `VEREDITOS_ACUSAM`: `espelho_furo`, `espelho_discordante`, `espelho_indefinida`, `espelho_cobranca_indevida`, `espelho_cobrado`, `espelho_nunca_bateu`, `espelho_fato_em_ausencia`. Não existe nesse código um mapa “cada um desses oito motivos → uma URL”; o retorno aplica o fio do dia. (`app/folha/export.py:1212-1238`; `app/escala/models.py:1735-1736`; `app/folha/fora_da_folha.py:203-213`)

| Fio | Texto/porta comprovada |
|---|---|
| `admin` | Validar ou recusar resposta; ações POST no detalhe local `/chamados/local/<chamado_id>/`, `acao=validar_pergunta` ou `recusar_pergunta`; alternativa POST `/chamados/validar-inline/<cid>/` → `views_cobrar::validar_inline`. O mapa de portas do retrato dá `None` ao fio admin e delega ao tipo. (`app/folha/fora_da_folha.py:38-42`; `app/chamados/urls.py:14`, `app/chamados/urls.py:33`; `app/chamados/views.py:483-518`; `app/chamados/views_cobrar.py:145-154`; `app/folha/retrato_folha.py:975`) |
| `colab` | Colaborador responde e depois admin valida; `/chamados/disputa/responder/<pergunta_id>/` → `responder_pergunta_disputa`; mapa do retrato abre `/chamados/modal/<colaborador_id>/` → `modal_fio_colaborador` com rótulo “Cobrar no fio”. `o_que_segura` não fornece `pergunta_id`. (`app/folha/fora_da_folha.py:40`, `app/folha/fora_da_folha.py:209-213`; `app/chamados/urls.py:17`, `app/chamados/urls.py:39`; `app/folha/retrato_folha.py:976`) |
| `chamado` | Resolver chamados vivos cujos IDs saem em `chamado`; detalhe `/chamados/local/<chamado_id>/`; mapa do retrato abre modal do fio com “Validar no fio”. (`app/folha/fora_da_folha.py:198-210`; `app/chamados/urls.py:14`, `app/chamados/urls.py:39`; `app/folha/retrato_folha.py:977`) |
| `nada` | Texto “SEM FIO… este dia precisa de alguém para olhar”; nenhum URL fornecido. No retrato esse estado gravado aparece como `ninguem`, com `None` explícito no mapa. (`app/folha/fora_da_folha.py:42`, `app/folha/fora_da_folha.py:59-73`; `app/folha/retrato_folha.py:978`) |

O mapa por **tipo** abre vínculo para `escala`, `folga`, `sem_escala`, e cadastro para `cadastro`; dá `None` para `nao_bate`, `falta`, `ferias` e sem causa. O próprio comentário registra que a porta de ausência/justificativa exige chamado, não colaborador; `/chamados/local/<chamado_id>/ir-para-ausencia/` redireciona para `/ponto/ausencias/?registro=<aid>`. (`app/folha/retrato_folha.py:980-995`; `app/chamados/urls.py:44`; `app/chamados/views.py:1494-1519`)

**Pergunta distinta: travas de aprovação**

`classificar_export` também devolve `travas_aprovacao`, separada do motivo de entrada no TXT: `conta_vazia` se horas trabalhadas zero, `turno_aberto` se contador >0, `sem_ancora_12x36` para escala ativa 12x36 sem âncora de colaborador ativo. (`app/folha/export.py:751-754`, `app/folha/export.py:786-814`, `app/folha/export.py:826`)

- `conta_vazia`: modo selecionado explicitamente ignora a trava; aprovação individual `/ponto/fechamento/aprovar-colaborador/` e lote `/ponto/fechamento/aprovar/`. Não representa necessariamente um dado a corrigir. (`app/ponto/views.py:25-33`, `app/ponto/views.py:1506-1517`; `app/ponto/urls.py:32-33`)
- `turno_aberto`: porta de decidir dia `/ponto/celula/veredito/`, e fio do chamado quando houver; a lista `/ponto/turnos-abertos/` é apenas leitura e limita-se a ativos/turnos recentes de 26h, portanto não cobre universalmente esta trava mensal. (`app/ponto/urls.py:15`, `app/ponto/urls.py:58`; `app/ponto/views.py:362-409`, `app/ponto/views.py:3302-3331`)
- `sem_ancora_12x36`: painel de vínculo e formulário de fase; campo `data_ancora` no painel, gravação pela porta já existente de vincular escala. (`app/colaboradores/urls.py:12-15`; `app/templates/colaboradores/partials/painel_vinculo.html:156-159`; `app/colaboradores/views_fase.py:1-9`)

**Candidatas e tela atual**

- `motivos_retencao_espelho(turnos_abertos, inconsistencias, grade, fech=None, hoje=None)` é compatibilidade, ignora os três primeiros parâmetros e delega à autoridade de célula na competência do fechamento; não é outro cálculo. (`app/folha/export.py:1243-1249`)
- `fora_da_folha(empresa, mes, ano, com_itens=False)` responde pelo conjunto da empresa e só detalha por dia/fio os classificados como `furo_espelho`; não substitui a resposta individual geral. (`app/folha/fora_da_folha.py:97-105`, `app/folha/fora_da_folha.py:119-139`)
- `portas_da_linha(linha)` e `o_que_fazer(linha, hoje=None)` leem linha de retrato gravado e fornecem links/rótulos; não substituem a investigação viva por dia de `o_que_segura`. (`app/folha/retrato_folha.py:915-938`, `app/folha/retrato_folha.py:998-1035`)
- A view atual de espelho usa `espelho_do_colab` para dias/resumo; não chama `o_que_segura` nesse fluxo. Os marcos faltantes do template apontam ao painel de chamados se `c.chamado_id` existe e ficam sem link caso contrário. (`app/ponto/views.py:153-165`, `app/ponto/views.py:193-213`; `app/templates/ponto/espelho.html:228-230`)

## 29/10 21:46 · sessao d3d6975b1ed8 · - · - · origem=Ronald (tmux)

B2 não tem uma função única que devolva simultaneamente “faltas + ausências justificadas, com tipo”; existem fontes distintas para dias decididos, cobertura e histórico. Isso aparece nas saídas de `datas_falta_janela` (lista de datas), `cobertura_ausencia_periodo` (mapa data→Ausencia) e `decididos_do_periodo` (mapa data→cartão com tipo/status). (`app/folha/export.py:158`, `app/ponto/turnos.py:2707`, `app/ponto/services/dia_decidido.py:504`, `app/ponto/services/dia_decidido.py:521`)

**B2 — fontes e limites**

- **Faltas decididas:** `app/folha/export.py::datas_falta_janela(colaborador, ini, fim)` retorna `[date]`; `datas_falta(colaborador, empresa, mes, ano)` é a candidata equivalente por competência, delegando à mesma função após `janela_competencia`. (`app/folha/export.py:158`, `app/folha/export.py:173`)
- **Universo/exclusões:** um colaborador, intervalo inclusivo informado; somente `Ausencia.status='aprovada'` de tipos cujo catálogo vigente declara `DESCONTA`, expandindo intervalos e recortando pelas pontas. Portanto não é restrito ao código literal `'falta'`; não inclui furo sem decisão, ausência pendente/rejeitada nem tipos abonados/suprimidos. (`app/folha/export.py:60`, `app/folha/export.py:75`, `app/folha/export.py:135`, `app/folha/export.py:168`, `app/ponto/catalogo/ausencias.py:331`)
- **Corte/relógio:** as duas funções recebem datas ou mes/ano; os corpos não consultam relógio. O wrapper utiliza `janela_competencia(empresa, mes, ano)`, que delega a `ponto.janelas.janela_fechamento`. (`app/folha/export.py:47`, `app/folha/export.py:158`, `app/folha/export.py:173`)
- **Detalhe com tipo, por dia:** `app/ponto/turnos.py::cobertura_ausencia_periodo(colab, ini, fim, *, para)` retorna `{date: Ausencia}`; `Ausencia` traz `tipo`, `status`, `minutos`, `data`, `data_fim`. Unidade: dias identificados por data, acompanhados do registro, não um total de horas. (`app/ponto/turnos.py:2707`, `app/ponto/turnos.py:2732`, `app/ponto/models.py:749`, `app/ponto/models.py:755`)
- **Universo/exclusões da cobertura:** um colaborador; datas dentro de `[ini,fim]`; range fechado, dia único e range sem fim somente para `TIPOS_EM_ABERTO`. Com `para='folha'` entram só aprovadas; com `'cobranca'` entram todas exceto rejeitadas. Em sobreposição fica uma ausência por dia: maior `(data, pk)`. Não há filtro automático apenas de “justificadas”; o tipo e o efeito do registro distinguem abonadas, descontadas e suprimidas. (`app/ponto/turnos.py:2475`, `app/ponto/turnos.py:2727`, `app/ponto/turnos.py:2734`, `app/ponto/turnos.py:2743`, `app/ponto/services/dia_decidido.py:453`)
- **Todas as ausências, sem desempate:** `app/ponto/turnos.py::ausencias_do_periodo(colab, ini, fim, *, para)` retorna queryset de todos os registros que tocam o período com o mesmo filtro por efeito folha/cobrança. Pode preservar múltiplos lançamentos sobre o mesmo dia. (`app/ponto/turnos.py:2487`)
- **Histórico com decisões rejeitadas:** `app/ponto/services/dia_decidido.py::decididos_do_periodo(colaborador, data_ini, data_fim, *, ausencias=None)` retorna mapa por dia; inclui rejeitadas, maior `(data,pk)` vence; o cartão contém `tipo`, `status`, `minutos`, `ausencia_id`, autor e instante da decisão. Não filtra só aprovadas e, no corpo, `data_fim=None` expande apenas o dia inicial, diferindo da cobertura aberta. (`app/ponto/services/dia_decidido.py:504`, `app/ponto/services/dia_decidido.py:521`, `app/ponto/services/dia_decidido.py:542`, `app/ponto/services/dia_decidido.py:550`)
- **Espelho atual:** `montar_dias` consulta cobertura `'cobranca'` para ícone, `'folha'` para rótulo e também `decididos_do_periodo`; não escolhe uma única dessas respostas. O resumo consulta faltas decididas por `folha_manda → datas_falta_janela`. (`app/ponto/services/espelho.py:590`, `app/ponto/services/espelho.py:599`, `app/ponto/services/espelho.py:658`, `app/ponto/services/espelho.py:1287`, `app/relatorios/cartao_pela_celula.py:373`)

Outras candidatas que respondem perguntas mais estreitas:

- `app/folha/export.py::datas_abonadas(colaborador, empresa, mes, ano)` devolve datas da rubrica 8932, aprovadas, pelos tipos abonados vigentes **excluindo férias, declaração e licença maternidade**. Perde o tipo no retorno; não é lista geral de justificadas e só aceita competência, não corte livre. (`app/folha/export.py:182`, `app/folha/export.py:190`, `app/ponto/catalogo/ausencias.py:288`)
- `app/ponto/services/efeito_ausencia.py::efeitos_da_ausencia(colaborador, data_ini, data_fim)` retorna sets `justificadas`, `suprimidas`, `dia_inteiro` e `min_parcial_pagos`. Soma `Justificativa` aprovada por `data_turno` às ausências aprovadas que abonam; parcial por minutos não abona o dia inteiro. Não devolve tipo. É usado na alimentação do motor do espelho. (`app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:36`, `app/ponto/services/efeito_ausencia.py:49`, `app/ponto/services/efeito_ausencia.py:59`, `app/ponto/services/espelho.py:1072`)
- `app/ponto/turnos.py::ausencias_vigentes(de, ate, colaborador=None)` é a fonte da lista administrativa: não filtra status nem tipo, e permite todos os colaboradores se o argumento for omitido. A view usa mês civil por padrão, com `de/ate` opcionais; não deriva competência. (`app/ponto/turnos.py:2948`, `app/ponto/turnos.py:2964`, `app/ponto/views.py:2177`, `app/ponto/views.py:2179`)
- `app/ponto/turnos.py::ausencias_do_dp(de, ate, colaborador=None)` delega a vigentes, exclui férias e rejeitadas; não é equivalente à lista de justificadas aprovadas. (`app/ponto/turnos.py:2921`, `app/ponto/turnos.py:2944`)

**B3 — existe saldo/status, não uma resposta completa de proporcionais até o corte**

A inexistência absoluta de outra função de férias proporcionais em toda a árvore permanece **[não verificado]**. Nos corpos efetivamente identificados, não há cálculo de avos ou proporcional: existem o saldo aritmético cadastrado e a classificação por datas, abaixo. (`app/ferias/models.py:94`, `app/ferias/models.py:215`, `app/ferias/situacao.py:296`)

- **FONTE por período:** `app/ferias/models.py::PeriodoAquisitivo.dias_saldo(self)` é uma **property**, sem data, retornando `dias_direito - dias_concedidos - dias_vendidos`, em **dias**. Universo: o PA desse objeto, com datas de início/fim e limite de concessão cadastrados. Exclui dias concedidos e vendidos; não rateia um PA em curso, não reconstrói saldo histórico na data de corte e não transforma dias em dinheiro. (`app/ferias/models.py:119`, `app/ferias/models.py:124`, `app/ferias/models.py:215`)
- **Status na data:** `app/ferias/models.py::status_de_periodo(pa, hoje=None)` devolve uma palavra, não dias. Usa a data fornecida; se omitida, `date.today()`. Saldo não positivo → `'concedido'`; `data_fim is None` ou `data_fim >= hoje` → `'em_curso'`; limite de concessão `< hoje` → `'vencido'`; restante → `'adquirido'`. O saldo atual é avaliado antes das datas. (`app/ferias/models.py:65`, `app/ferias/models.py:94`)
- **Leitor agregado:** `app/ferias/situacao.py::situacao_de_ferias(colab, hoje=None)` aceita a referência; se omitida lê `datetime.date.today()`. Busca **todos** os PAs daquele colaborador ordenados por início, sem competência e sem filtro de início/fim até o corte. Retorna `saldo_total`, `periodos`, agendamentos e avisos. (`app/ferias/situacao.py:138`, `app/ferias/situacao.py:155`, `app/ferias/situacao.py:162`)
- **Vencidas nesse leitor:** aviso `pa_vencido` para PAs com limite `< hoje` e saldo positivo; o texto lista no máximo três períodos e a âncora aponta ao primeiro. O retorno `periodos` preserva todos os PAs com saldo positivo, mas não traz lista numérica separada de vencidos. (`app/ferias/situacao.py:192`, `app/ferias/situacao.py:200`, `app/ferias/situacao.py:296`)
- **Cuidado documentado pelo corpo:** embora a docstring diga “saldo somando períodos já adquiridos”, a execução soma **todos os PAs com saldo positivo**, inclusive em curso/futuros se cadastrados; não há filtro por aquisição completa. (`app/ferias/situacao.py:146`, `app/ferias/situacao.py:162`, `app/ferias/situacao.py:296`)
- **Corte não vira histórico:** `dias_concedidos` é incrementado ao agendar, antes do gozo; portanto passar uma data antiga a `situacao_de_ferias` ou `status_de_periodo` muda comparação de datas, mas as funções continuam lendo saldo cadastrado atual, já afetado pelo agendamento. (`app/ferias/services.py:306`, `app/ferias/services.py:321`, `app/ferias/models.py:95`, `app/ferias/situacao.py:68`)

Candidatas próximas, mas não equivalentes ao bloco:

- `app/ferias/models.py::vencendo(pa, dias=DIAS_VENCENDO, hoje=None)` retorna booleano; usa `status_de_periodo == 'adquirido'` e limite até `hoje+dias`. Exclui vencidos porque exige adquirido. (`app/ferias/models.py:252`)
- `app/ferias/services.py::periodos_vencendo(hoje=None, dias=JANELA_VENCIMENTO_DIAS, colaborador=None, empresa=None)` retorna queryset filtrando **status armazenado `'adquirido'`**, sem limite inferior, sem filtro explícito por saldo. Pode consultar todos os colaboradores quando omitido; não é o mesmo predicado de `vencendo`. (`app/ferias/services.py:111`)
- `app/ferias/models.py::PeriodoAquisitivo.faltas_periodo(self)` conta registros com tipo literal `'falta'`, aprovados e **data inicial** entre início/fim do PA; não expande ranges nem recorta por desligamento. `dias_direito_por_faltas(faltas)` aplica faixas 30/24/18/12/0. Isso é redução cadastral por faltas, não férias proporcionais. (`app/ferias/models.py:52`, `app/ferias/models.py:140`)
- `app/ferias/views.py::saldo_ferias(request, cid)` lê `date.today()` e chama `p.sincronizar_status(hoje)`; essa cadeia pode salvar PA e recalcular art.130. Não é função de leitura apropriada para executar neste trabalho. (`app/ferias/views.py:98`, `app/ferias/views.py:108`, `app/ferias/views.py:113`, `app/ferias/services.py:215`, `app/ferias/services.py:238`)
- O espelho de ponto chama `espelho_do_colab`, que não agrega saldos por PA; ele consulta as **ausências de férias que cobrem dias**, via cobertura. Não há chamada a `situacao_de_ferias`/saldo de PA nesse caminho identificado. (`app/ponto/views.py:157`, `app/ponto/services/espelho.py:590`, `app/ponto/services/espelho.py:1198`)

## 29/10 21:46 · sessao 93e33cd7acec · - · - · origem=Ronald (tmux)

B1 tem uma fachada usada pelo espelho atual, mas **não há nela garantia de devolver todos os totais até uma DATA arbitrária**: `hoje` limita o motor e os dias; ao final, os totais podem ser substituídos pelos da competência inteira. Isso está no encadeamento de `espelho_do_colab` e `folha_manda` (`app/ponto/services/espelho.py:1229`, `app/ponto/services/espelho.py:1266`, `app/ponto/services/espelho.py:1287`, `app/relatorios/cartao_pela_celula.py:321`).

- **FONTE da tela:** `app/ponto/services/espelho.py::espelho_do_colab(colaborador, hoje, apur_ini, apur_fim, *, ano=None, modo_gestor=False, agora)`. Devolve `dias`, `resumo`, `resultado`, batidas e escala (`app/ponto/services/espelho.py:1198`, `app/ponto/services/espelho.py:1298`). A view atual chama exatamente essa função e publica `dias`/`resumo_mes` (`app/ponto/views.py:157`).  
- **UNIDADE:** totais trabalhados, extras, atrasos, saída antecipada, faltas e abonos em **horas decimais**; `dias_falta` é contagem de dias e `datas_falta` lista de datas (`app/relatorios/cartao_pela_celula.py:93`, `app/relatorios/cartao_pela_celula.py:102`, `app/relatorios/cartao_pela_celula.py:104`, `app/relatorios/cartao_pela_celula.py:111`). O template imprime uma casa decimal seguida de `h`, inclusive extras e atrasos (`app/templates/ponto/espelho.html:38`, `app/templates/ponto/espelho.html:51`, `app/templates/ponto/espelho.html:62`, `app/templates/ponto/espelho.html:269`).
- **UNIVERSO:** um colaborador passado explicitamente; dias visuais desde `min(piso_visual(colaborador, hoje), apur_ini)` até `hoje`, portanto os dias podem começar antes da competência. O motor usa essa mesma janela visual (`app/ponto/services/espelho.py:1229`, `app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1266`). O resumo inicial restringe rubricas aos períodos cuja entrada local cai em `[apur_ini, apur_fim]` (`app/ponto/services/espelho.py:83`, `app/ponto/services/espelho.py:450`). Depois `folha_manda` identifica se essas duas pontas correspondem a uma competência inteira, e então tenta fontes da competência inteira (`app/relatorios/cartao_pela_celula.py:27`, `app/relatorios/cartao_pela_celula.py:321`).
- **EXCLUSÕES:** batidas retratadas ficam fora da apuração; batidas pendentes do DP continuam contando (`app/ponto/turnos.py:2818`, `app/ponto/turnos.py:2838`). No resumo inicial, períodos fora das pontas da competência não entram (`app/ponto/services/espelho.py:83`). Folga trabalhada não entra no atraso, saída antecipada nem HE 50/100 dessa soma; entra em trabalhadas, noturno, intra e em `trab_folga`/`total_extras` por tratamento próprio (`app/ponto/services/espelho.py:95`, `app/ponto/services/espelho.py:105`). Na fonte final folha/lavratura, `total_trabalhadas` usa somente `horas_trabalhadas`, e folga vai separadamente para `trab_folga` (`app/relatorios/cartao_pela_celula.py:111`, `app/relatorios/cartao_pela_celula.py:116`, `app/relatorios/cartao_pela_celula.py:174`).

**Fontes reais por dia:** a fachada carrega `DiaPago` com `app/ponto/services/dia_pago.py::linhas_do_periodo(colab, ini, fim, *, versao='motor')`; são somente linhas `tipo=dia`, versão pedida e datas inclusivas, devolvidas como `{date: DiaPago}` (`app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:852`). Cada dia recebe `dia_pago`, `pago_h` e `rubricas` (`app/ponto/services/espelho.py:816`).

- A linha monetária da **tela HTML** lê `rubricas_do_dia(linha)`, que devolve horas trabalhadas, extras 50/100, atraso, antecipada, noturno, intra e `lavrada`; não devolve `horas_falta`. Linha ausente devolve zeros com `lavrada=False`, e o template mostra “sem apuração ainda” (`app/ponto/services/dia_pago.py:899`, `app/ponto/services/dia_pago.py:908`, `app/ponto/services/dia_pago.py:914`, `app/templates/ponto/espelho.html:263`).
- `pago_do_dia(linha)` é outra medida: soma trabalhadas + folga trabalhada + folga-feriado; devolve `None` para linha ausente ou sem rubricas monetárias. Essa medida está em `dia['pago_h']`, mas a linha HTML acima imprime `rubricas.trabalhadas` (`app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`, `app/ponto/services/dia_pago.py:895`, `app/ponto/services/espelho.py:822`, `app/templates/ponto/espelho.html:268`).
- Faltas também aparecem como ausência/tipo e palavra do dia, não como campo `horas_falta` em `rubricas_do_dia` (`app/templates/ponto/espelho.html:186`, `app/ponto/services/espelho.py:856`, `app/ponto/services/espelho.py:1295`, `app/ponto/services/dia_pago.py:914`).

**Hierarquia dos totais finais:**

1. `app/relatorios/cartao_pela_celula.py::totais_da_folha(colab, mes, ano)` exige existência de `FechamentoMensal`, mas lê os números online por `fechamento_lido`; se qualquer um faltar, devolve `None` (`app/relatorios/cartao_pela_celula.py:44`, `app/relatorios/cartao_pela_celula.py:69`).
2. Sem totais da folha, `::_totais_da_lavratura(colab, mes, ano)` usa `soma_da_competencia` e datas de falta de `folha.export.datas_falta`; soma a competência, incluindo ajustes, e devolve faltas decididas em dias/horas (`app/relatorios/cartao_pela_celula.py:126`, `app/relatorios/cartao_pela_celula.py:141`, `app/relatorios/cartao_pela_celula.py:166`, `app/relatorios/cartao_pela_celula.py:183`, `app/ponto/services/dia_pago.py:548`, `app/ponto/services/dia_pago.py:753`).
3. Sem ambas, conserva o resumo do motor, carimbando `fonte_dos_totais='sem apuracao ainda'`; com lavratura usa `'lavratura'`; com folha, `'folha'` (`app/relatorios/cartao_pela_celula.py:340`). No resumo inicial de `espelho_do_colab`, não há inicialização de `dias_falta`/`horas_falta`; sua lista do motor é explicitamente `datas_furo_apurado`, enquanto as faltas decididas vêm das fontes finais (`app/ponto/services/espelho.py:1234`, `app/ponto/services/espelho.py:1253`, `app/relatorios/cartao_pela_celula.py:104`, `app/relatorios/cartao_pela_celula.py:183`).

**DATA/relógio:** a fachada recebe `hoje` e `agora`, mas a cadeia não fica presa a eles. `folha_manda` não recebe nenhum desses parâmetros, chama `ate_ontem(data_fim_pedido)` sem `hoje`, que usa `timezone.localdate()`, e consulta `turno_aberto_de(colaborador)` sem `agora`, que usa `timezone.now()` (`app/relatorios/cartao_pela_celula.py:296`, `app/relatorios/cartao_pela_celula.py:398`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:37`, `app/ponto/turnos.py:2247`). `fechamento_lido(colaborador, mes, ano)` só recebe competência e chama o recalculador em leitura; esse recalculador captura seu próprio `timezone.now()` e usa a janela inteira da empresa (`app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`, `app/ponto/services/fechamento.py:595`, `app/ponto/services/fechamento.py:600`).

**Outras candidatas existentes, com perguntas diferentes:**

| Candidata e assinatura | O que já devolve / limite |
|---|---|
| `app/ponto/services/espelho.py::autoridade_do_periodo(colaborador, ini, fim)` | Motor apurado para intervalo explícito; devolve `Autoridade(resultado, batidas, esc, tipo_escala, feriados, motor, regua_por_fatia)`. É chamada pela fachada, mas não é sozinha o resumo final da tela (`app/ponto/services/espelho.py:982`, `app/ponto/services/espelho.py:1195`, `app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1287`). |
| `app/ponto/services/espelho.py::somar_periodos(periodos, periodos_ft, dentro=None)` | Soma rubricas do motor em horas, com filtro opcional de intervalo; não soma faltas e não aplica a seleção folha/lavratura (`app/ponto/services/espelho.py:51`, `app/ponto/services/espelho.py:77`, `app/ponto/services/espelho.py:117`). |
| `app/ponto/services/dia_pago.py::soma_do_periodo(colaboradores, ini, fim, *, versao='motor')` | Soma todas as rubricas diárias, incluindo `horas_falta`, por colaborador, entre datas inclusivas; retorna também `sem_lavratura`/`sem_apuracao`. Exclui linhas de ajuste, DSR e banco. Não lê relógio no corpo (`app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:750`, `app/ponto/services/dia_pago.py:754`, `app/ponto/services/dia_pago.py:782`, `app/ponto/services/dia_pago.py:828`). |
| `app/ponto/services/fechamento.py::fechamento_lido(colaborador, mes, ano)` | Totais online da competência inteira; sem corte parametrizado. Pode devolver `None`; universo inclui não desligados ou desligados com demissão a partir do início da competência da empresa (`app/ponto/services/fechamento.py:1703`, `app/ponto/services/fechamento.py:1727`, `app/ponto/services/fechamento.py:415`, `app/ponto/services/fechamento.py:423`). |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho(colaborador, data_ini, data_fim, _turnos_juiz=None, *, agora)` | Coletor do PDF, também retorna resumo/dias; limita seu fim a ontem em relação ao `agora`, lê os dias de `espelho_do_colab`, filtra-os para o intervalo e reaplica `folha_manda` com as pontas originais (`app/relatorios/pdf_espelho.py:187`, `app/relatorios/pdf_espelho.py:286`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:624`). |

**A competência contendo DATA** já vem de `app/ponto/janelas.py::janela_atual(data, empresa=None)`; usa o corte da empresa e devolve o intervalo que contém a data, inclusive depois do dia 21. `janela_fechamento(mes, ano, empresa=None)` identifica a competência pelo mês/ano do seu fim, do corte do mês anterior ao corte−1 do mês informado; corte é limitado a `[2,28]`, padrão 21 (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`).

**Consumidores não têm exatamente o mesmo recorte:** a web usa sempre `janela_fechamento(mes, ano)` escolhido no GET; por padrão usa mês/ano civis do relógio. A API usa `janela_atual(agora.date())` quando o pedido é o mês/ano civil atual, e `janela_fechamento` nos demais. Ambos chamam `espelho_do_colab` (`app/ponto/views.py:97`, `app/ponto/views.py:101`, `app/ponto/views.py:154`, `app/api/views.py:1350`, `app/api/views.py:1365`). A ficha também chama a mesma fachada, mas recorta sua resposta pública para trabalhadas, extras, turnos e fonte (`app/colaboradores/services/ficha.py:196`).

## 29/10 21:45 · sessao 567a32481603 · piloto-desligamento · verificador · origem=Code

O desenho precisa resolver estes pontos antes de virar código:

1. **A exceção de escrita conflita com a regra vigente.** A seção 7b proíbe sandbox diferente de `read-only` e repete “Codex escrevendo” entre os proibidos. O corte precisa delimitar a exceção do piloto nessas regras. (`CLAUDE.md:565–579,588–599`)

2. **Adicionar `construtor` à lista não basta.** Hoje ele recebe rc 2; o destino continua sendo a cópia por hash, criada sem permissão de escrita; o cabeçalho proíbe criar, editar e apagar; a execução usa `read-only`. São quatro pontos distintos a separar por papel. O selo atual também rejeita qualquer ocorrência de `workspace-write` no código do wrapper. (`bin/codex_pergunta.sh:48,87–100,123,157–158`; `bin/tests/test_codex_pergunta.sh:11–18`)

3. **`ESPERANDO: <wrapper> | pid=N` exige identidade exata.** Use `codex_pergunta.sh` ou seu caminho absoluto; `bin/codex_pergunta.sh` é recusado por ser relativo com `/`. O PID precisa ser do processo que corresponde ao wrapper, não de um shell externo ou de `timeout`. A declaração deve ser a primeira linha útil, começando literalmente por `ESPERANDO:`. (`bin/hook_stop_fila1.py:400–413,474–522,620–639`)

4. **Shell de fundo com rc em arquivo não substitui a prova de espera.** O hook só reconhece os campos declarados em `ESPERANDO:`. Arquivo recém-observado não prova avanço; declarar PID e arquivo exige que ambos passem. O wrapper atual retorna rc ao chamador, mas não cria o arquivo de rc: falta fixar quem o escreve e seu caminho. (`bin/hook_stop_fila1.py:605–645`; `bin/codex_pergunta.sh:157–177`; `app/docs/RELATO.md:13`)

5. **O timeout não limita a chamada inteira.** `flock 9` bloqueia sem prazo; o laço dos slots continua até conseguir um. `CODEX_TIMEOUT` só envolve `codex exec`, depois dessas esperas. Portanto, rc 124 não cobre a espera por capacidade. (`bin/codex_pergunta.sh:137–158`)

6. **“Teto 6 herdado” é apenas o padrão configurável.** `CODEX_TETO` pode substituir 6; `CODEX_LOGD` muda os arquivos usados como locks. Além disso, os slots limitam quantidade, sem exclusão por worktree: não impedem dois construtores no mesmo destino. O contrato deixa essa exclusão sem regra. (`bin/codex_pergunta.sh:47,51,138–153`)

7. **Falhas e voltas ainda têm regras conflitantes ou incompletas.** O wrapper manda seguir pelo sonnet no rc 75; também possui saídas 2, 3 e 4 que o contrato não trata. A seção 7b exige opus final e determina subida ao opus após mais de três voltas; “3 voltas e o Code decide” precisa preservar ou alterar explicitamente essa obrigação. (`bin/codex_pergunta.sh:23–24,39–50,99,105–114,169`; `CLAUDE.md:583–585,594–599`)

8. **`git checkout .` não cumpre “descarta o diff inteiro”.** Sem commit indicado, checkout restaura a árvore a partir do índice: alterações já staged continuam sendo a fonte, e arquivos novos fora do índice não são abrangidos por essa restauração. Falta definir a base e quais estados entram no descarte. (`/usr/share/man/man1/git-checkout.1.gz:142–149`, linhas do texto descomprimido) [nao verificado]

9. **O log atual não mede o resultado prometido.** Ele registra obra, papel, hash fornecido, PID, sessão, rc e duração; não registra hash da worktree, volta, escopo, selos, retrabalho ou deploy validado. O contador existente classifica `construtor` como `outro`. Faltam fonte e vínculo das medidas por entrega. (`bin/codex_pergunta.sh:42,53–60,88–89`; `bin/codex_limite_estado.py:23,49–78`)

10. **Remover o papel “no mesmo marco” do pouso conflita com o pouso separado de instrumentos.** A regra vigente exige pouso próprio para `bin/`, depois do produto. O desligamento precisa indicar essa sequência ou declarar a exceção. (`CLAUDE.md:733–738`) [nao verificado]

## 29/10 21:34 · sessao 45d6d74dee1e · o261 · sonda · origem=Code

[nao verificado] — Transcrição do dado fornecido:

```text
MODO MEDIR | universo (respondida sem resposta, VIVA, nao validada) 263, esperado 263 | competencia ABERTA 115 | EXPORTADA -> Pauta DP 148 em 2 empresa x competencia | sem data_evento 0 | colabs 23
ABERTA por competencia: {'2:10/2026': 88, '3:10/2026': 5, '2:11/2026': 22}
EXPORTADA por competencia: {'2:09/2026': 147, '3:09/2026': 1}
ABERTA por via_resolucao: {'chamado_encerrado': 113, 'ausencia_dia': 1, 'completude_turno': 1}
ABERTA com disputa fechada (badge do colab nao alcanca: cura (4) alcancabilidade): 0
porta reabrir_para_correcao alcanca (perguntas_corrigiveis): 115 de 115; fora: []
VIGIA desde 17:20: respondida sem resposta 0 | com respondida==materializada (ramo sem credor) 0 | vias {}
PROVA universo depois 263 (esperado = exportadas 148 se APLICAR, = 263 se MEDIR) | competencia ABERTA ainda no universo 115 (esperado 0 depois do APLICAR: 2a passada = 0)
PROVA estado antes->depois das ABERTA: {('respondida', 'respondida'): 115} | vivas depois 115 de 115
PROVA exportadas vigentes intactas: True (7)
ERROS=0
FIM-DA-SONDA
```

## 29/10 21:15 · sessao c435b9a9bb3f · o261 · verificador · origem=Code

**A v3 corrige a reprovação da volta 2:** o contrato do carimbo agora inclui o contador e seu esperado. Sem achado bloqueante por inspeção; permanece o achado baixo do seletor da auditoria. (`app/chamados/tests/test_o261_vigia_fio.py:701,706`, pós-diff; `app/core/models.py:286`.)

As referências “pós-diff” usam a numeração resultante do patch fornecido. Execução dos testes, REDs e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **Sítios criados ou tocados — cumprimento do aval**

   - **`disputa_emissao.py` — sem achado na cura.** A guarda exige texto com `strip()`, preserva materialização, autor e via, mantém a consulta ao C6 e acrescenta trilha com antes/depois. Não acrescenta escrita de `validada_em`. (`app/chamados/services/disputa_emissao.py:1641,1643,1655,1659,1661`, pós-diff; `app/docs/CORTES.md:383`.)
   - **`vigia_fio.py` — sem achado.** O contador lê o par persistido, aplica o mesmo `strip()`, aceita recorte por empresa e restringe-se ao carimbo posterior ao marco. (`app/chamados/services/vigia_fio.py:369,372,381,383,385`, pós-diff.)
   - **`vigiar_fio_morto.py` — sem achado.** Imprime o contador, inclui medido/esperado no carimbo e conserva os grupos de alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:79,89,172,193,198`, pós-diff.)
   - **Novo `test_o261_resposta_vazia.py` — sem achado por inspeção.** Cobra vazio, espaços, texto, conteúdo da trilha, repetição sequencial, corte histórico e integração com impressão/carimbo. (`app/chamados/tests/test_o261_resposta_vazia.py:77,89,97,106,124,134,145`, pós-diff.)
   - **`test_o261_vigia_fio.py` — sem achado remanescente.** As duas igualdades agora conferem a composição nova sem retirar a comparação dos contadores anteriores. (`app/chamados/tests/test_o261_vigia_fio.py:701,706`, pós-diff.)

   A reparação das 263 vivas, reversão/prova e encaminhamento das exportadas são exigências do aval cuja realização permanece **[nao verificado]**; este diff não demonstra essa etapa. (`app/docs/CORTES.md:383`.)

2. **Classes de reprovação**

   - **Segundo juiz — sem achado. Derivação paralela — sem achado.** O escritor mantém `pergunta_viva`; o contador verifica a inconsistência dos campos, sem decidir vitalidade ou via. (`app/chamados/services/disputa_emissao.py:1641`; `app/chamados/services/vigia_fio.py:372`, pós-diff.)
   - **Segundo escritor fora da porta — sem achado.** Permanecem os dois sítios declarados da função; a auditoria passa pelo escritor único `evento`. (`app/chamados/tests/test_porta_resposta_pergunta.py:68,95`; `app/chamados/services/disputa_emissao.py:1659,1661`, pós-diff; `app/core/observ.py:94`.)
   - **Relógio fora da borda — sem achado novo.** `_tz.now()` já existia; o contador acrescenta uma constante temporal e o teste novo congela o relógio. (`app/chamados/services/disputa_emissao.py:1638`, cópia; `app/chamados/services/vigia_fio.py:369`; `app/chamados/tests/test_o261_resposta_vazia.py:32`, pós-diff.)
   - **Ordem de trava — sem achado novo.** O diff não acrescenta bloqueio de negócio; a auditoria usa seu `atomic` existente. (`app/chamados/services/disputa_emissao.py:1659,1661`, pós-diff; `app/core/observ.py:133`.)
   - **Idempotência sequencial — sem achado.** A pergunta materializada sai de `_abertas`; o teste exige estado igual e uma única linha após duas chamadas. Garantia concorrente: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1634`, cópia; `app/chamados/tests/test_o261_resposta_vazia.py:106`, pós-diff.)
   - **Selo sem mordida — sem achado por inspeção.** Há casos positivos e negativos para a guarda, a trilha e o contador; execução efetiva: **[nao verificado]**. (`app/chamados/tests/test_o261_resposta_vazia.py:77,97,106,134,145`, pós-diff.)

3. **Leitores, contratos e testes existentes** [nao verificado]

   - **Baixa — ação ausente do seletor da auditoria.** `fecho_sem_credor` não integra `ACOES`, usada pelo dropdown. A gravação aceita verbo livre; listagem geral e filtro GET explícito continuam disponíveis. (`app/chamados/services/disputa_emissao.py:1661`, pós-diff; `app/core/models.py:286,296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:25`.)
   - **Carimbo, painel e saída `VFM|` — sem achado remanescente.** O painel compara genericamente `esperado` contra `censo`; os testes shell usam carimbos sintéticos compatíveis; a linha antiga `VFM|ESPERADO|` permanece baseada no mesmo dicionário. (`bin/fio_estado.py:69,105`; `bin/tests/test_fio_estado.sh:40,93`; `app/chamados/management/commands/vigiar_fio_morto.py:175`, pós-diff.)
   - **Leitores dos campos — sem achado funcional.** Os leitores de pendência/vitalidade continuam usando o juiz; o teste existente da órfã exige materialização, via e fecho, sem exigir carimbo de resposta. (`app/chamados/models.py:1381,2111`; `app/folha/fora_da_folha.py:66`; `app/chamados/tests/test_b2d_resposta_visivel.py:76,97`.)
   - **Baixa, preexistente e fora do diff — contrato textual desatualizado.** O teste do par ainda chama carimbo sem texto de “inofensivo” e “legítimo”. Seu detector unidirecional não quebra com a v3, mas essa explicação contraria o aval novo. (`app/chamados/tests/test_contract_par_resposta.py:28,81,113`; `app/docs/CORTES.md:383`.)

4. **Mensagem do commit** [nao verificado]

   **Sem achado funcional contra o que ela descreve.** Os irmãos conferidos só criam o carimbo com texto; ausência pode regravar o valor existente. (`app/chamados/services/disputa_emissao.py:1570`; `app/chamados/services/materializacao.py:511,528,1190`.)

   Duas qualificações: “instante do aval” é precisão não demonstrada por `17:2x`; e “cada fecho deixa UMA linha” descreve o caminho normal, pois `evento` é *best-effort*. O instante exato e a gravação efetiva de cada trilha são **[nao verificado]**. (`app/docs/CORTES.md:383`; `app/chamados/services/vigia_fio.py:369`, pós-diff; `app/core/observ.py:98,105,139`.)

## 29/10 21:14 · sessao f85af26a11ef · - · - · origem=Ronald (tmux)

Sem quebra executável identificada nos testes existentes do escritor: o caso sem credor exige materialização/via/fecho, sem exigir `respondida_em`; os casos que exigem carimbo têm texto. (`app/chamados/tests/test_b2d_resposta_visivel.py:76`; `app/chamados/tests/test_materializar_carimba_respondida.py:62`; `app/chamados/tests/test_materializar_carimba_respondida.py:76`.)

**Baixa, documentação de contrato desatualizada:** o contrato existente ainda declara carimbo sem texto “inofensivo” e “legítimo”, contrário ao novo aval; o detector permanece unidirecional e não falha com a v3. Não é quebra funcional. (`app/chamados/tests/test_contract_par_resposta.py:28`; `app/chamados/tests/test_contract_par_resposta.py:81`; `app/chamados/tests/test_contract_par_resposta.py:113`; `app/docs/CORTES.md:383`.)

**Censo de escritores — sem achado contra a mensagem:** os irmãos de materialização e S128 só criam o carimbo com texto; ausência inclui o campo no `save` mesmo sem atribuir valor novo. A resposta automática recebe texto constante no único chamador localizado. (`app/chamados/services/materializacao.py:511`; `app/chamados/services/materializacao.py:528`; `app/chamados/services/materializacao.py:1190`; `app/chamados/services/disputa_emissao.py:1570`; `app/api/foto_recebida.py:25`; `app/api/foto_recebida.py:48`.)

**Relógio/contrato estrutural — sem achado:** o teste novo congela o relógio e conserva os dois sítios de escrita já declarados; não acrescenta atribuição isolada de texto. (`diff_v3.patch`, `app/chamados/tests/test_o261_resposta_vazia.py:32`; `app/chamados/tests/test_porta_resposta_pergunta.py:68`; `app/chamados/tests/test_contract_par_resposta.py:81`; `app/core/tests/test_selo_teste_sem_relogio.py:34`.)

Execução dos testes e efeitos reais: [nao verificado]. [nao verificado]

## 29/10 21:14 · sessao d9a6926f4fa7 · - · - · origem=Ronald (tmux)

- **Baixa — ação nova ausente do seletor de auditoria.** `fecho_sem_credor` não integra `ACOES`; a tela alimenta o dropdown por essa lista. A gravação aceita verbos livres, e a listagem geral e o filtro GET explícito permanecem disponíveis. (`app/core/models.py:286,296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:25`; ação nova: `app/chamados/services/disputa_emissao.py:1661`, diff.)

- **Outros testes de auditoria e portas — sem achado.** O novo evento continua usando o escritor único; a função conserva seus dois sítios de escrita de pergunta. (`app/core/observ.py:94,134`; `app/core/tests/test_contract_trilha.py:57`; `app/chamados/tests/test_porta_resposta_pergunta.py:66,95`; `app/chamados/services/disputa_emissao.py:1659,1661`, diff.)

- **Contratos dos campos — sem achado.** O teste existente da órfã sem resposta exige materialização, via e fechamento, sem exigir `respondida_em`; os casos existentes de carimbo com texto pertencem ao ramo S128 preservado. (`app/chamados/tests/test_b2d_resposta_visivel.py:76,97`; `app/chamados/tests/test_materializar_carimba_respondida.py:66,81`; `app/chamados/services/disputa_emissao.py:1570`.)

- **Leitores da pergunta — sem achado.** Pendências consultam vitalidade; `.estado` mantém `RESOLVIDA` para a morta materializada; fora da folha considera somente vivas. (`app/chamados/models.py:1381,2111,2113`; `app/chamados/services/aviso_pendencia.py:13`; `app/folha/fora_da_folha.py:66`.)

Execução dos testes e efeitos em produção: **[nao verificado]**. [nao verificado]

## 29/10 21:13 · sessao 2e2b8e5f00e7 · - · - · origem=Ronald (tmux)

- **Contrato do carimbo — sem achado remanescente.** O v3 atualiza a igualdade integral de `censo` e o conjunto de `esperado` para conter `respondida_sem_resposta`, corrigindo as duas incompatibilidades apontadas na v2. (`app/chamados/tests/test_o261_vigia_fio.py:701,706-709`, diff pós-aplicação.)

- **Leitor do painel — sem achado.** A comparação percorre genericamente todas as chaves de `esperado`; a chave nova divergente aparecerá em `FORA DO ESPERADO`, mesmo sem constar de `gritou`. (`bin/fio_estado.py:69-73,105-108,118-119`.) O ESTADO usa esse leitor e compositor. (`bin/gerar_estado.py:256-260`.)

- **Outros testes do carimbo — sem achado.** O selo shell usa carimbos sintéticos e não exige um conjunto fechado de chaves do comando real; as verificações existentes de caminho, recorte e alarme permanecem compatíveis. (`bin/tests/test_fio_estado.sh:40-47,93-102`; `app/chamados/tests/test_o261_vigia_fio.py:717-750`, cópia.)

- **Saída `VFM|` — sem achado.** A nova linha é adicional; a linha `VFM|ESPERADO|` continua renderizando o mesmo `ESPERADO`, e os grupos de alarme não mudam. (`app/chamados/management/commands/vigiar_fio_morto.py:79-89,168-183`, diff pós-aplicação.)

- **Novo teste do comando — sem achado de isolamento sob settings CI.** Embora use o caminho padrão do comando, CI já redireciona o carimbo para um diretório temporário. (`app/chamados/tests/test_o261_resposta_vazia.py:154-160`, diff; `app/config/settings/ci.py:30-33`; `app/chamados/management/commands/vigiar_fio_morto.py:100-101`, cópia.)

Execução dos testes: **[nao verificado]**. [nao verificado]

## 29/10 21:09 · sessao c7d5d7152cde · o261 · verificador · origem=Code

**Ainda não aprovaria a v2: ela quebra um teste existente do carimbo em duas asserções.** (`app/chamados/tests/test_o261_vigia_fio.py:699,704`.)

“Diff” abaixo indica numeração pós-aplicação; demais referências são da cópia. Execução dos testes e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **Cumprimento do aval**

   **Sem achado no escritor:** a guarda usa texto com `strip()`, conserva materialização, autor e via, e mantém a consulta ao C6. A trilha agora captura o estado anterior e descreve a presença de resposta independentemente de acrescentar um carimbo nesta chamada. (`app/docs/CORTES.md:383`; `app/chamados/services/disputa_emissao.py:1641,1643,1649,1655,1661`, diff.)

   O contador passa a aplicar o mesmo teste Python e recebe o recorte de empresas. **Sem achado nesses ajustes.** A reparação das 263, com reversão/prova e encaminhamento do exportado, permanece **[nao verificado]**. (`app/chamados/services/vigia_fio.py:372,381,383,384`, diff; `app/chamados/management/commands/vigiar_fio_morto.py:172`, diff; `app/docs/CORTES.md:383`.)

2. **Classes de reprovação**

   - **Segundo juiz / derivação paralela — sem achado.** O contador verifica a inconsistência dos campos, usando o mesmo `strip()`; não decide vitalidade, via ou destino da pergunta. (`app/chamados/services/vigia_fio.py:372,384`, diff; `app/chamados/juizes.py:1020`.)
   - **Segundo escritor fora da porta — sem achado.** A alteração permanece no ato já declarado; a trilha usa `core.observ.evento`. (`app/chamados/tests/test_porta_resposta_pergunta.py:66,68,95`; `app/chamados/services/disputa_emissao.py:1659,1661`, diff; `app/core/observ.py:94`.)
   - **Relógio fora da borda — sem achado novo.** O `_tz.now()` deste ramo já existia; o contador acrescenta uma constante temporal, sem nova leitura do relógio. (`app/chamados/services/disputa_emissao.py:1638`, cópia; `app/chamados/services/vigia_fio.py:369`, diff; `app/docs/CORTES.md:369`.)
   - **Ordem de trava — sem achado novo.** O diff não acrescenta trava de negócio; o evento usa o `atomic` da porta de auditoria. (`app/chamados/services/disputa_emissao.py:1659,1661`, diff; `app/core/observ.py:133`.)
   - **Idempotência sequencial — sem achado.** A segunda chamada encontra a pergunta materializada, fora de `_abertas`; o teste novo também exige estado igual e uma única linha de trilha. (`app/chamados/services/disputa_emissao.py:1634`, cópia; `app/chamados/tests/test_o261_resposta_vazia.py:106`, diff.)
   - **Selo sem mordida — sem achado nos casos novos por inspeção.** Há asserções para vazio, espaços, texto, conteúdo da trilha e impressão/carimbo do comando. Isso não resolve a incompatibilidade com o selo existente, descrita abaixo. RED e execução efetivos: **[nao verificado]**. (`app/chamados/tests/test_o261_resposta_vazia.py:77,89,97,106,124,145`, diff; `app/chamados/tests/test_o261_vigia_fio.py:699,704`.)

3. **Leitores, contratos e testes existentes** [nao verificado]

   - **Média — contrato do carimbo quebrado.** O comando acrescenta `respondida_sem_resposta` a `reg['censo']`, mas `censo()` continua sem essa chave. Portanto, a igualdade integral exigida pelo teste existente falhará **mesmo quando o contador for zero**. A segunda incompatibilidade é `esperado`: o teste exige exatamente as quatro chaves antigas. É necessário atualizar o contrato e sua prova para a composição nova, preservando a conferência dos contadores. (`app/chamados/management/commands/vigiar_fio_morto.py:193,198`, diff; `app/chamados/services/vigia_fio.py:205,326`, cópia; `app/chamados/tests/test_o261_vigia_fio.py:699,704`.)

   - **Baixa — permanece a ausência da ação no seletor da auditoria.** `fecho_sem_credor` não está em `ACOES`, que alimenta o dropdown. A linha continua acessível pela listagem geral e pelo filtro GET explícito; a gravação admite o verbo livre. (`app/core/models.py:286,296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:23`.)

   - **`bin/fio_estado.py` — sem achado.** O leitor compara genericamente `esperado` com `censo` e publica a divergência com valor medido e esperado. Estar fora de `gritou` não impede a exibição. (`bin/fio_estado.py:69,72,105,118`.)

   - **Leitores da pergunta — sem achado.** O badge exige disputa aberta e pergunta viva; `.estado` conserva `RESOLVIDA` para a pergunta morta materializada; `_fio_do_dia` considera somente perguntas vivas. (`app/chamados/models.py:1381,2111,2113`; `app/folha/fora_da_folha.py:66`.)

4. **Triagem dos descartados**

   - **Marco 17:20 — descarte como bloqueio procede.** É o início conservador da janela `17:2x`: pode contar ocorrências anteriores ao instante efetivo, sem excluir as posteriores. Não demonstra o instante exato do aval; esse instante e a existência de ocorrências nesse intervalo são **[nao verificado]**. (`app/chamados/services/vigia_fio.py:369,381`, diff; `app/docs/CORTES.md:383`.)
   - **Concorrência — descarte procede no contrato sequencial informado.** A leitura sem bloqueio e o `save` já existiam. A trilha nova também pode duplicar sob concorrência, mas isso não demonstra quebra das duas chamadas sequenciais exigidas. (`app/chamados/services/disputa_emissao.py:1630,1634,1651`, cópia; `:1661`, diff; `app/chamados/tests/test_fio_disputa_resolvido.py:82`.)

## 29/10 21:08 · sessao 1fff0cdcd0a0 · - · - · origem=Ronald (tmux)

- **Baixa — permanece a limitação do filtro da auditoria:** `fecho_sem_credor` não aparece no seletor de ações, pois `ACOES` enumera somente sete verbos e a tela usa essa lista. A gravação aceita o verbo livre; listagem geral e filtro GET continuam alcançando o evento. (`app/core/models.py:286-296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:23-27`.)

- **Trilha v2 — sem achado:** `_antes` captura via e carimbo anteriores; `materializada_em=None` corresponde ao filtro de entrada. A descrição passa a distinguir presença de texto, independentemente de acrescentar carimbo nesta chamada. (`app/chamados/services/disputa_emissao.py:1634-1635`, cópia; `:1643,1649-1656,1661-1668`, diff.)

- **Segundo escritor / juiz / trava / relógio — sem achado novo:** permanecem os dois saves declarados do ato, a consulta a C6 e o relógio já existente; a auditoria usa a porta canônica `evento`, que abre seu próprio `atomic`. (`app/chamados/tests/test_porta_resposta_pergunta.py:66-68,95-103`; `app/chamados/services/disputa_emissao.py:1641,1638`, cópia; `app/core/observ.py:94,133-138`.)

- **Idempotência — descarte da concorrência procede no contrato informado:** leitura sem bloqueio e save já existiam; os testes e a definição da casa exigem duas chamadas sequenciais. A auditoria duplicada sob concorrência é possível, mas sua ocorrência e exigência contratual são **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1630-1651`, cópia; `app/chamados/tests/test_fio_disputa_resolvido.py:82-89`; `app/chamados/models.py:2124-2128`.)

- **Leitores da pergunta — sem regressão identificada:** o badge exige disputa aberta e C6 vivo; `.estado` continua retornando resolvida para pergunta morta materializada mesmo com `respondida_em=None`. (`app/chamados/models.py:1381-1385,2111-2114`.)

- **Selos do escritor — sem achado por leitura:** os testes novos cobrem vazio, espaços, controle com texto, estado e trilha sequenciais. Execução e RED efetivo: **[nao verificado]**. (`app/chamados/tests/test_o261_resposta_vazia.py:77-130`, diff.)

## 29/10 21:08 · sessao b7b7ed5f0e24 · - · - · origem=Ronald (tmux)

**Média — contrato existente do carimbo quebrado.** O v2 acrescenta a chave `respondida_sem_resposta` em `reg['censo']`, mas deixa `censo()` sem essa chave. O teste existente exige igualdade integral entre ambos e falhará nessa comparação; também exige o dicionário `esperado` antigo, sem a chave nova. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:193,198` (**diff**, conforme hunk `+190`); `app/chamados/services/vigia_fio.py:205,325,326` (**cópia**); `app/chamados/tests/test_o261_vigia_fio.py:699,704-706` (**cópia**). Execução: **[nao verificado]**.

**`bin/fio_estado.py` — sem achado.** O leitor compara genericamente todas as chaves de `esperado` contra `censo` e renderiza as divergências com medição e esperado; portanto exibirá o contador novo quando diferente de zero. O fato de não estar em `gritou` não impede essa exibição. (`bin/fio_estado.py:69-73,105-108,118-119`.)

**Outros contratos do comando — sem achado.** O v2 conserva o recorte sem carimbo e as constantes de alarme, compatíveis com os respectivos testes existentes. (`app/chamados/management/commands/vigiar_fio_morto.py:79-80,177-197`, cópia; `app/chamados/tests/test_o261_vigia_fio.py:647-657,717-750`.)

**Teste shell do leitor — sem achado identificado por inspeção.** Ele usa carimbos sintéticos e verifica a comparação genérica, sem exigir o conjunto completo das chaves produzidas pelo comando. Execução: **[nao verificado]**. (`bin/tests/test_fio_estado.sh:36-51,82-91`.)

## 29/10 21:04 · sessao 17fc8315b18b · o261 · verificador · origem=Code

**Não aprovaria a v1 ainda:** a guarda corrige o carimbo sem texto, mas há falhas na trilha, na idempotência concorrente e no recorte do vigia. (`app/chamados/services/disputa_emissao.py:1652,1660`; `app/chamados/management/commands/vigiar_fio_morto.py:166`, diff.)

Referências marcadas **“diff”** usam a numeração após aplicação; as demais são da cópia. Execução dos testes, comportamento do PostgreSQL real e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **Cumprimento do aval**

   A cura principal está no lugar autorizado: condiciona `respondida_em` ao texto, conserva materialização, autor e via, consulta `pergunta_viva` e mantém o fechamento da disputa por `d.fechar`. Não acrescenta reparação do passivo das 263. **Sem achado nessa delimitação.** (`app/docs/CORTES.md:383`; `app/chamados/services/disputa_emissao.py:1641,1643,1652,1673`, diff.)

   - **Média — trilha pode afirmar um estado falso.** A descrição distingue os casos por `'respondida_em' in _c`, que significa **carimbo acrescentado nesta chamada**, não existência de resposta. Com carimbo anterior preservado, escreve “respondida_em fica nulo”; com texto anterior, pode ainda declarar “sem resposta” enquanto `tem_resposta=True`. `_abertas` não exclui perguntas já respondidas. (`app/chamados/services/disputa_emissao.py:1634,1653,1660,1663`, diff.)
   - **Média — falta o “antes” da trilha.** O evento registra somente `depois`, embora a regra exija usuário, antes/depois e motivo para ação crítica; `antes` ficará `None`. (`app/chamados/services/disputa_emissao.py:1658,1662`, diff; `app/core/observ.py:94`; `app/docs/LEIS.md:65`.)
   - **Lacuna de evidência — marco exato.** O código fixa **17:20**, mas o aval fornecido registra **17:2x**. Não é possível certificar esse instante como a fronteira autorizada entre histórico e novas ocorrências. (`app/chamados/services/vigia_fio.py:369`, diff; `app/docs/CORTES.md:383`.) Instante efetivo: **[nao verificado]**.

2. **Classes de reprovação**

   - **Segundo juiz / derivação paralela — Média.** O critério de texto já participa do juiz registrado C6 como `(resposta_colab or '').strip()`. O vigia cria outra implementação, por regex, sem compartilhar o predicado nem demonstrar equivalência. Deve compartilhar o critério de presença de texto; chamar `pergunta_viva` inteira também confundiria vazio com vitalidade. (`app/core/juizes.py:232`; `app/chamados/juizes.py:1005,1020`; `app/chamados/services/vigia_fio.py:375,381`, diff; `CLAUDE.md:20`.)
   - **Segundo escritor fora da porta — sem achado.** A alteração permanece no ato já declarado de resolução da pergunta; a auditoria nova passa por `core.observ.evento`, sem criação direta de `LogAuditoria`. (`app/chamados/tests/test_porta_resposta_pergunta.py:68,88`; `app/chamados/services/disputa_emissao.py:1657`, diff; `app/core/observ.py:94,134`.)
   - **Relógio — sem leitura nova identificada.** Permanecem dois `_tz.now()` internos para os carimbos. O complemento de injeção nomeia juiz/grade/cartório/reconciliador/fábrica; não basta, isoladamente, para concluir que esta porta de escrita está proibida de obter o instante. A constante nova delimita observação histórica, sem alterar uma decisão de negócio; sua precisão continua sem prova, conforme acima. (`app/chamados/services/disputa_emissao.py:1561,1638`; `app/docs/CORTES.md:369`; `app/chamados/services/vigia_fio.py:365,369`, diff.)
   - **Ordem de trava — sem inversão nova identificada.** O ramo não acrescenta trava explícita; `evento` abre um `atomic` para a trilha. Isso deixa a falha de serialização abaixo. (`app/chamados/services/disputa_emissao.py:1630,1656`, diff; `app/core/observ.py:133`.)
   - **Idempotência concorrente — Média.** Duas execuções podem ler a mesma pergunta aberta antes dos respectivos saves, sobrescrever a materialização e emitir duas linhas. Não há bloqueio da leitura nem atualização condicional; o `atomic` do chamador não reivindica a pergunta. O teste novo cobre apenas chamadas sequenciais. (`app/chamados/services/disputa_emissao.py:1630,1634,1656,1658`, diff; `app/chamados/services/becos.py:41,49`; `app/chamados/tests/test_o261_resposta_vazia.py:97`, diff.)

   **Selos/RED:**

   - **Sem achado de passagem vazia nos casos principais:** restaurar o carimbo indiscriminado contraria `assertIsNone`; retirar a materialização ou o evento contraria as asserções de materialização e contagem. O caso com texto é controle positivo, já atendido pelo código antigo. (`app/chamados/tests/test_o261_resposta_vazia.py:80,84,92,102`, diff; `app/chamados/services/disputa_emissao.py:1648`, cópia.)
   - **Baixa — cobertura incompleta do escritor:** substituir a guarda por `bool(q.resposta_colab)` deixaria passar resposta só com espaços; os testes do escritor usam apenas `''` e `'07:55'`. O teste com espaços chama somente o contador. (`app/chamados/tests/test_o261_resposta_vazia.py:77,88,115`, diff.)
   - **Média — integração do vigia sem mordida nova:** retirar do comando a impressão, o carimbo e o alarme novos não afeta o teste acrescentado, que chama diretamente a função contadora. Também faltam asserções sobre fechamento da disputa, via específica e conteúdo da trilha. (`app/chamados/tests/test_o261_resposta_vazia.py:77,97,106`, diff.)

3. **Leitores e contratos**

   - **Média — `--empresa` mistura recorte e frota.** Os contadores existentes recebem `empresas`; o novo é chamado sem recorte e consulta todas as perguntas. Uma infração de outra empresa pode aparecer no resultado selecionado e provocar saída 2 com `--alarme`. (`app/chamados/management/commands/vigiar_fio_morto.py:143,154,161,166,204`, diff; `app/chamados/services/vigia_fio.py:380`, diff.)
   - **Baixa — ESTADO perde o número e seu esperado.** O contador fica no topo do JSON, fora de `censo` e `ESPERADO`. O leitor compara somente esses dois conjuntos; mostrará o nome em `gritou`, mas não a medição nova nem seu esperado zero. (`app/chamados/management/commands/vigiar_fio_morto.py:86,195,197`, diff; `bin/fio_estado.py:69,82,118`.)
   - **Baixa — ação ausente no filtro enumerado da auditoria.** `fecho_sem_credor` não entra no dropdown alimentado por `ACOES`. A listagem geral e o filtro GET livre continuam alcançando a linha. (`app/core/models.py:286`; `app/core/views.py:705,738`; `app/templates/core/log_auditoria.html:23`.)
   - **Gravação da ação e código de saída — sem achado.** O campo admite verbos livres na gravação usada por `evento`; o comando conserva o carimbo antes da saída 2, e o placar já interpreta esse código como alarme. (`app/core/models.py:296`; `app/core/observ.py:134`; `app/chamados/management/commands/vigiar_fio_morto.py:184,204`, diff; `app/core/management/commands/pipeline_placar.py:75,98`.)

4. **Query, schema e PostgreSQL**

   - **`''`, NULL e fronteira inclusiva — sem achado.** `TextField(blank=True, default='')` não declara `null=True`; portanto o ramo `isnull` é redundante. O `*` contempla a string vazia, e `gte` inclui exatamente o marco. (`app/chamados/models.py:1865`; `app/chamados/services/vigia_fio.py:380,381`, diff.)
   - **Paridade com `strip()` — não demonstrada.** O teste cobre espaços ASCII, mas não tabulações, quebras de linha ou Unicode. A equivalência de `\s` do PostgreSQL com o critério Python, para a configuração real, é **[nao verificado]**; permanece o achado de definição paralela. (`app/chamados/juizes.py:1020`; `app/chamados/tests/test_o261_resposta_vazia.py:115`, diff.)
   - **“Novas” significa carimbo posterior ao marco.** A query não mede quando o estado ilegal surgiu. Por exemplo, apagar posteriormente o texto de uma resposta cujo carimbo é anterior continuaria fora da contagem; a existência desse caminho em produção é **[nao verificado]**. (`app/chamados/services/vigia_fio.py:380,381`, diff.)

## 29/10 21:03 · sessao e8be9c30180a · - · - · origem=Ronald (tmux)

Achados da parte juiz/vigia:

- **Média — derivação paralela do critério de resposta vazia.** O juiz registrado é `pergunta_viva` (`app/core/juizes.py:232`), cujo corpo exige texto por `(p.resposta_colab or '').strip()` (`app/chamados/juizes.py:1020`). O vigia novo implementa outra definição por regex (`app/chamados/services/vigia_fio.py:381`, pós-diff), contrariando o princípio de contador ler a mesma autoridade (`app/docs/LEIS.md:60`). Deve compartilhar o predicado de texto com o juiz; chamar `pergunta_viva` inteira também seria incorreto, porque vida e vazio são perguntas diferentes (`app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1020`).

- **Média — o marco exato não está provado.** A constante fixa 17:20 (`app/chamados/services/vigia_fio.py:369`, pós-diff), mas o aval registra apenas `17:2x` (`app/docs/CORTES.md:383`). A equivalência entre esse instante e o aval/deploy é **[nao verificado]**. É corte histórico pedido pelo aval, não relógio de negócio por si só; o problema comprovável é a precisão inventada (`app/docs/CORTES.md:383`).

- **Baixa — falta mordida do escritor para resposta só com espaços.** O novo teste do escritor usa apenas `''` e `'07:55'`; remover `.strip()` do guard novo e deixar `bool(q.resposta_colab)` sobreviveria a esses casos (`app/chamados/tests/test_o261_resposta_vazia.py`, anexo, testes `test_sem_texto...` e `test_MORDE_com_texto...`). O teste do vigia com `'   '` não exercita esse escritor (mesmo arquivo, `test_vigia_conta_so_as_novas`). O juiz existente já distingue espaços como vazio (`app/chamados/tests/test_pergunta_viva.py:26`).

Schema/query:

- `resposta_colab` não aceita NULL no schema: `TextField(blank=True, default='')`; logo `isnull=True` é redundante, sem perda funcional por si só (`app/chamados/models.py:1865`).
- A regex com `*` contempla `''`; a filtragem `respondida_em__gte` exclui carimbo NULL e inclui exatamente o marco (`app/chamados/services/vigia_fio.py:380`, pós-diff).
- PostgreSQL é o backend declarado (`app/config/settings/prod.py:11`). A equivalência completa entre `strip()` e `\s` do PostgreSQL/locale não está demonstrada pela cópia; diferenças de Unicode/locale e ocorrência em produção ficam **[nao verificado]**. Não há teste de paridade, tabulação, newline, Unicode ou igualdade ao marco no teste novo (anexo, `test_vigia_conta_so_as_novas`).
- Não achei escritor concreto remanescente que legitimamente gere novo carimbo vazio: irmãos têm guardas por `strip()` (`app/chamados/services/disputa_emissao.py:1219`, `app/chamados/services/disputa_emissao.py:1570`, `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:1190`). A prosa antiga que chama carimbo sem texto legítimo não basta para alegar falso alarme novo (`app/chamados/tests/test_contract_par_resposta.py:28`).

Mordida:

- **Sem achado de teste vazio nos três casos de cura/trilha:** o primeiro exige fechamento e ausência do carimbo; o segundo exige fechamento e carimbo; o terceiro exige estado idêntico e uma linha após duas chamadas (anexo `app/chamados/tests/test_o261_resposta_vazia.py`, três testes respectivos).
- O teste do vigia injeta o estado inválido diretamente: certifica o contador, não deve ser apresentado como prova da cura (mesmo arquivo, `test_vigia_conta_so_as_novas`). [nao verificado]
- Execução real dos testes e PostgreSQL: **[nao verificado]**. [nao verificado]

## 29/10 21:02 · sessao 27725cac3b86 · - · - · origem=Ronald (tmux)

Linhas do diff abaixo são pós-aplicação; demais referências são da cópia.

- **P2 — trilha contradiz o estado.** A descrição do novo evento decide se havia resposta por `'respondida_em' in _c`, que identifica apenas carimbo recém-escrito. Se `q.resposta_colab='07:55'` e `q.respondida_em` já existe, o ramo preserva ambos e registra **“sem resposta do colaborador, respondida_em fica nulo”**, enquanto `depois.tem_resposta=True`. A entrada é possível porque `_abertas` só exclui materializadas e validadas. Referências: `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1653` (diff), `app/chamados/services/disputa_emissao.py:1660` (diff).

- **P2 — a garantia nova de uma linha não cobre concorrência.** O ramo lê `_todas`/`_abertas` sem bloqueio ou atualização condicional; duas execuções podem capturar a mesma pergunta aberta, salvar e emitir `fecho_sem_credor` duas vezes. `atomic` dos chamadores não serializa essa leitura; há chamador com `atomic` e nenhuma trava (`fechar_beco`). O novo teste verifica duas chamadas sequenciais apenas. Referências: `app/chamados/services/disputa_emissao.py:1630`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1656` (diff), `app/chamados/services/disputa_emissao.py:1658` (diff), `app/chamados/services/becos.py:41`, `app/chamados/services/becos.py:49`, `app/chamados/tests/test_o261_resposta_vazia.py:97` (diff). Ocorrência concorrente real: **[nao verificado]**.

- **Aval:** a guarda `bool(...strip())` cura a escrita sem texto; mantém `materializada_em`, autor e via, usa o juiz existente `pergunta_viva`, e deixa o fecho da disputa na porta `d.fechar`. O passivo não é alterado por rotina adicionada neste diff. Referências: `app/docs/CORTES.md:383`, `app/chamados/services/disputa_emissao.py:1641`, `app/chamados/services/disputa_emissao.py:1643`, `app/chamados/services/disputa_emissao.py:1652` (diff), `app/chamados/services/disputa_emissao.py:1673` (diff).

- **Segundo escritor de LogAuditoria: sem achado.** O evento entra por `core.observ.evento`, declarado escritor único e implementado por `objects.create`; o ramo novo não escreve o modelo diretamente. Referências: `app/chamados/services/disputa_emissao.py:1657` (diff), `app/core/observ.py:94`, `app/core/observ.py:96`, `app/core/observ.py:134`.

- **Ordem de trava: sem inversão nova identificada; falta serialização descrita acima.** O resolver não acrescenta `select_for_update`; o `evento` abre somente `transaction.atomic` para a criação da trilha. Referências: `app/chamados/services/disputa_emissao.py:1514`, `app/chamados/services/disputa_emissao.py:1630`, `app/core/observ.py:133`.

- **Selos:** não passam integralmente vazios: o caso sem texto exige materialização, autor, via e ausência de carimbo; remover a guarda e voltar ao original viola `assertIsNone`. Remover o evento viola a contagem 1. O caso `MORDE_com_texto` preserva a simetria, mas já seria verde no código anterior, que carimba indiscriminadamente; isso é contraprova útil, não RED da cura. Não há teste novo que exija `d.fechada_em`, via específica, descrição correta da trilha ou concorrência. Referências: `app/chamados/tests/test_o261_resposta_vazia.py:77` (diff), `app/chamados/tests/test_o261_resposta_vazia.py:88` (diff), `app/chamados/tests/test_o261_resposta_vazia.py:97` (diff), `app/chamados/services/disputa_emissao.py:1648` (base). Execução/RED real: **[nao verificado]**.

- **Relógio:** não há leitura nova de `now()` neste ramo; os dois `_tz.now()` já existem no resolver. A regra registrada de injeção nomeia juiz/grade/cartório/reconciliador/fábrica e bordas, então não afirmo automaticamente que este service de escrita viola aquele complemento. Referências: `app/chamados/services/disputa_emissao.py:1561`, `app/chamados/services/disputa_emissao.py:1638`, `app/docs/CORTES.md:369`.

## 29/10 21:01 · sessao 32c02b5a9280 · - · - · origem=Ronald (tmux)

Achados:

- **P2 — recorte `--empresa` contaminado pela frota.** O comando entrega `empresas` ao censo e ao contador irmão, mas a nova chamada `respondida_sem_resposta()` não recebe recorte; a nova função consulta todas as perguntas. Assim uma empresa sem infração pode imprimir contagem de outra e sair com código 2. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:142-160`; `diff_v1.patch`, nova linha 166 do comando e novas linhas 372-382 do serviço. A ocorrência concreta em produção é [nao verificado].

- **P2/P3 — contador novo incompleto no contrato do carimbo/ESTADO.** Ele é guardado no topo do JSON, mas permanece fora de `censo`, `ESPERADO`, `ALARMA_SEMPRE` e `ALARMA_EM_0`. O leitor calcula `fora_do_esperado` exclusivamente de `censo` × `esperado` e não extrai a chave nova: `bin/fio_estado.py:69-92`. O ESTADO mostrará o nome em “a rodada ALARMOU” via `gritou`, mas não o valor medido nem seu esperado: `bin/fio_estado.py:101-119`; `bin/gerar_estado.py:260-261`. Portanto não alegar que o alarme fica totalmente oculto.

- **P3 — ação nova ausente no filtro enumerado da auditoria.** `fecho_sem_credor` não consta em `LogAuditoria.ACOES`: `app/core/models.py:286-296`. O dropdown usa essa lista: `app/core/views.py:738`; `app/templates/core/log_auditoria.html:23-27`. A linha continua na listagem geral, e GET manual com `acao=fecho_sem_credor` funciona pelo filtro livre: `app/core/views.py:705-710`. Não há falha de gravação por `choices`: o campo suporta verbos livres de até 64 caracteres, conforme o próprio comentário em `app/core/models.py:296`, e o escritor chama `objects.create` sem `full_clean`: `app/core/observ.py:133-138`.

Sem achado:

- **Código de saída e consumidor do pipeline:** o diff mantém carimbo antes do `SystemExit(2)` (`app/chamados/management/commands/vigiar_fio_morto.py:174-197`, linhas-base), e o placar já trata exit 2 como ALARME: `app/core/management/commands/pipeline_placar.py:75-79,98-99`.
- **Outros leitores por ações específicas:** histórico consulta apenas transições de Chamado, portanto a ação de PerguntaDisputa não entra por engano: `app/chamados/services/historico.py:44,86-88`; vigia de loop filtra `estado_chamado`: `app/chamados/services/vigia_loop.py:28-32`.
- **Segundo escritor de LogAuditoria:** a chamada nova usa a casa única `evento`, que efetivamente grava em `app/core/observ.py:94-105,133-138`.

Cobertura insuficiente: os testes existentes verificam os grupos declarados e esperado antigos, não a nova integração (`app/chamados/tests/test_o261_vigia_fio.py:643-657,703-707`); o teste novo do anexo chama só a função contadora, nunca o comando (`app/chamados/tests/test_o261_resposta_vazia.py:106-119`, arquivo novo do diff). Remover do comando a impressão, o carimbo e o alarme novos não é detectado por esse teste novo. Execução dos testes e consumidores externos à cópia: [nao verificado].

_255 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
