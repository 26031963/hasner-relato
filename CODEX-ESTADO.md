# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 23:59** · publicado em **11/11/2026 00:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 23:59 · sessao a8e3ccca6aa3 · FIO-E0-V4-E4B · leitura · origem=Code

**Não é possível separar os 45 em a/b com o anexo fornecido.** Ele identifica dias, mas não informa chamados, autores ou atos de fechamento. Distribuição efetiva em produção: **[não verificado]**. Além disso, `via_encerramento='admin'` não comprova decisão real: houve backfill desse valor apenas pela presença de um usuário não sistêmico. `app/chamados/migrations/0039_a3_via_backfill.py:14`, `app/chamados/migrations/0039_a3_via_backfill.py:26`.

**1. Caminhos para estado terminal**

Terminal compreende `resolvido`, `fechado`, `cancelado` e `superado`; a investigação precisa cobrir os quatro. `app/chamados/catalogo/motor.py:30`.

Nos gates de transição abaixo, o ritual comum escreve `status_local`, `via_encerramento`, `resolvido_em`, `atualizado_em` e rebaixa a urgência preservando a original. Escreve `respondido_por` somente quando `set_user=True`. `app/chamados/models.py:228`, `app/chamados/models.py:233`, `app/chamados/models.py:459`.

A auditoria comum é `LogAuditoria.acao='estado_chamado'`, com antes `{status_local}` e depois `{status_local, via}`. O usuário pode vir do argumento/request; fora de request existe o ator de processo `sistema:<comando>`. A gravação da auditoria é best-effort. `app/chamados/models.py:292`, `app/core/observ.py:94`, `app/core/observ.py:108`.

| Entrada | Escrita específica | Lastro ou limite |
|---|---|---|
| `transicionar → _aplicar` | Destino humano; via `admin`; substitui `respondido_por`. | Pode acompanhar decisão real, mas também mera mudança de status sem texto. `app/chamados/models.py:128`, `app/chamados/models.py:447`, `app/chamados/services/acoes_chamado.py:283`, `app/chamados/services/acoes_chamado.py:313`. |
| `fechar(user, novo_status, resolucao)` | Delega a `transicionar`; resolução opcional vira nota no fio depois da transição. | Não exige resolução preenchida. `app/chamados/models.py:452`. |
| `reconciliar` | `resolvido`, via `sistema`; preserva o usuário anteriormente gravado. | Hoje consulta as guardas da premissa e da pergunta viva. Permite também agrupamento, dedup e fechamento sem dia identificável, este último com trilha específica. `app/chamados/models.py:641`, `app/chamados/models.py:682`, `app/chamados/models.py:717`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:750`. |
| `cancelar(motivo, user)` | `cancelado`, via `cancelado`; troca usuário somente se fornecido; motivo opcional vira nota. | Descarte administrativo/emissor aposentado; a porta não demonstra por si uma premissa factual morta. `app/chamados/models.py:773`, `app/chamados/models.py:788`. |
| `encerrar` | `fechado`, via `cascade`; preserva usuário. | Não consulta a guarda da premissa usada por `reconciliar`; consulta a guarda de pergunta viva. A prova substantiva depende do chamador. `app/chamados/models.py:1605`, `app/chamados/models.py:1617`. |
| `encerrar_por_competencia_trancada` | `fechado`, via `competencia_trancada`; serviço acrescenta nota e evento com competência/empresa. | O código denomina isso lastro de tranca efetiva, mas esclarece que não é resolução pela realidade. Não equivale a simples idade/janela. `app/chamados/models.py:1622`, `app/chamados/services/competencia_trancada.py:3`, `app/chamados/services/competencia_trancada.py:12`, `app/chamados/services/competencia_trancada.py:184`. |
| Migração 0039 | Cancelado com irmão único vira `superado`, via `superado`, `superado_por_id`. | Escrita histórica direta; esse trecho não grava ator, log nem timestamp. A porta corrente `superar()` foi removida. `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/models.py:794`. |

Há ainda **nascimento admitido com status terminal**, fora do ritual de transição: `criar(status=...)` aceita status explícito; `abrir(defaults=...)` permite sobrescrever o status inicial. Ambos deixam `nascimento_chamado`, não necessariamente uma travessia vivo→terminal. `app/chamados/models.py:1054`, `app/chamados/models.py:1063`, `app/chamados/models.py:1095`, `app/chamados/models.py:938`, `app/chamados/models.py:971`, `app/chamados/models.py:1218`. O chamador explícito encontrado de `criar(status=...)` é o smoke, limitado a banco de teste e com rollback. `app/chamados/management/commands/smoke_chamados.py:31`, `app/chamados/management/commands/smoke_chamados.py:39`, `app/chamados/management/commands/smoke_chamados.py:123`.

Os chamadores encontrados dos gates terminais são:

- **Transição humana:** aprovação de exceção, aceite/recusa de agenda e escala, autorização/recusa de geofence e resposta livre com mudança opcional de status. `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:277`, `app/chamados/services/acoes_chamado.py:315`. Também resposta pelo fio e decisão de benefício. `app/chamados/services/fio.py:721`, `app/colaboradores/views.py:2068`.
- **Fechamento assinado automaticamente como admin:** comando de meio período escolhe o primeiro superuser e chama `fechar`. É mais uma razão para não interpretar `via=admin` como identidade do agente real. `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`.
- **Cascade:** desligamento, substituição de aceite antigo, regularização externa e fecho com retenção usado pela justificativa. `app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/reconciliador.py:485`, `app/ponto/services/justificativa.py:87`, `app/ponto/services/justificativa.py:400`.
- **Cancelamento:** avisos de régua e revisão de vínculo transferidos para a lista Cadastro x Realidade. `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`.
- **Reconciliação por disputa/retratacão/materialização:** signal da disputa, reconciliador, materialização de ausência/pergunta, adesão, pós-vínculo e agrupamento/folga. `app/chamados/signals.py:42`, `app/chamados/reconciliador.py:523`, `app/chamados/services/materializacao.py:582`, `app/chamados/services/materializacao.py:771`, `app/chamados/services/adesao.py:115`, `app/chamados/services/adesao.py:150`, `app/colaboradores/services/pos_vinculo.py:42`, `app/chamados/services/limbo_folgas.py:101`, `app/chamados/services/limbo_folgas.py:283`, `app/chamados/services/limbo_folgas.py:286`.
- **Reconciliação por fatos do dia:** feriado/folga, ausência decidida/aprovada, cobertura, cluster, marcos acesos, entrada sincronizada, triagem e decisão administrativa da célula. `app/chamados/services/feriado_reavaliar.py:47`, `app/chamados/services/silencio_ausencia.py:101`, `app/ponto/services/ausencia.py:694`, `app/ponto/services/cobertura.py:122`, `app/ponto/services/cobertura.py:317`, `app/ponto/services/cluster_espurio.py:124`, `app/ponto/services/lastro.py:400`, `app/ponto/services/entrada_sincronizada.py:31`, `app/ponto/services/triagem_batida.py:597`, `app/ponto/services/veredito_celula.py:202`.
- **Cartório:** reconciliações nas passadas de lavra, marcos satisfeitos/inexistentes, ausência de protesto e duplicidade do fio. `app/ponto/services/cartorio.py:974`, `app/ponto/services/cartorio.py:1499`, `app/ponto/services/cartorio.py:1512`, `app/ponto/services/cartorio.py:1533`, `app/ponto/services/cartorio.py:1611`, `app/ponto/services/cartorio.py:1626`.
- **Comandos adicionais:** auditoria de órfãos, feriado do posto, intervalo ausente, cura BUG115 e detecção de ausências por premissa/afastamento/cobertura/batida. `app/chamados/management/commands/auditar_chamados_orfaos.py:67`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`, `app/ponto/management/commands/detectar_intervalo_ausente.py:138`, `app/ponto/management/commands/curar_residuo_bug115.py:151`, `app/ponto/management/commands/detectar_ausencias.py:158`, `app/ponto/management/commands/detectar_ausencias.py:824`, `app/ponto/management/commands/detectar_ausencias.py:834`, `app/ponto/management/commands/detectar_ausencias.py:845`.

**Fechamento por idade antigo:** há lápide expressa da expiração de disputa parada por 30 dias, revogada pela C7. O cron de prescrição corrente somente silencia, sem terminalizar. `app/chamados/management/commands/arquivar_competencia_encerrada.py:30`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`, `app/chamados/management/commands/arquivar_prescritos.py:6`, `app/chamados/management/commands/arquivar_prescritos.py:94`. O cluster também documenta a antiga confusão “fora da janela = premissa falsa”; hoje consulta a célula do próprio dia, sem limite etário para fechar. `app/ponto/services/cluster_espurio.py:53`. Campos e trilhas exatos das versões removidas: **[não verificado]**.

**2. O que o banco permite distinguir retrospectivamente**

O chamado possui `status_local`, `via_encerramento`, `respondido_por`, `resolvido_em`, `atualizado_em` e `superado_por`. `fechada_por`, `motivo_fechamento` e `resolucao` pertencem à **Disputa**, não ao chamado. `app/chamados/models.py:52`, `app/chamados/models.py:58`, `app/chamados/models.py:1766`, `app/chamados/models.py:1773`, `app/chamados/models.py:1793`.

| Evidência gravada | O que permite concluir |
|---|---|
| Via `admin`, usuário preenchido | Identifica uma via/atribuição, sem provar decisão substantiva; pode ser backfill ou comando assinado por superuser. `app/chamados/migrations/0039_a3_via_backfill.py:26`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`. |
| Via `sistema`, `cascade`, `cancelado`, `legado` | Não identifica, sozinha, a prova que autorizou o fechamento. O evento genérico guarda somente estado/via. `app/chamados/catalogo/motor.py:86`, `app/chamados/models.py:292`. |
| `respondido_por` | Pode ser resíduo de ato/ciclo anterior, pois os fechamentos automáticos não o substituem. `app/chamados/models.py:230`, `app/chamados/models.py:654`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`. |
| `resolvido_em` | Marca a saída de vivo, mas `resolvido→fechado` preserva esse instante e sobrescreve a via. A última linha terminal pode não ser quem matou a vida. `app/chamados/models.py:242`. |
| `fechamento_por_dedup` | Prova que foi invocada a sobrevivência de outra cobrança; traz dia e chamado sobrevivente. Não demonstra premissa factual morta. `app/chamados/models.py:750`. |
| `fechamento_sem_dia_do_fato` | Declara que a guarda da premissa não conseguiu julgar. Não prova, sozinho, nem lastro nem inexistência de outro ato. `app/chamados/models.py:732`. |
| `reconciliar_feriado` / `reconciliar_ausencia_lancada` | Evidência específica com dia e, no segundo caso, ausência identificada; precisa ser atribuída ao ciclo correto. `app/chamados/services/feriado_reavaliar.py:52`, `app/chamados/services/silencio_ausencia.py:108`. |
| Disputa `admin_encerra`, resolução, autor, `disputa_fechada` e decisões das perguntas | Registram ato explícito sobre a disputa/perguntas. O fechamento do chamado-pai é outro passo, tentado pelo signal. `app/chamados/models.py:1918`, `app/chamados/models.py:1932`, `app/chamados/models.py:1943`, `app/chamados/signals.py:42`. |
| `DecisaoPergunta` | Registra ato, instante, autor e motivo sobre a pergunta; não possui vínculo explícito com o evento que terminalizou determinado ciclo do chamado. `app/chamados/models.py:3839`, `app/chamados/models.py:3871`. |
| Nota do fio | Guarda conteúdo/autor/instante, mas não persiste o parâmetro `tipo` nem `objeto_ref` como campos recuperáveis. `app/chamados/models.py:1668`, `app/chamados/models.py:1688`, `app/chamados/services/nota.py:87`, `app/chamados/services/nota.py:95`. |
| Ausência de auditoria/decisão | Não prova ausência de ato: a auditoria é best-effort e existe legado anterior à sequência de decisões. `app/core/observ.py:94`, `app/chamados/models.py:3913`. |

Portanto, a investigação deve localizar a **travessia vivo→terminal do ciclo corrente**, conferir sua cadeia com o estado do banco e procurar a prova substantiva correspondente. Sem prova positiva de fechamento por idade/ausência de lastro, não é correto transformar lacuna de trilha em classe **b**. Sem vínculo suficiente entre decisão/prova e fechamento, permanece **indeterminado**. `app/chamados/models.py:242`, `app/chamados/models.py:292`, `app/core/observ.py:98`.

**3. Portas existentes**

- **O261-E F1 existe como guarda do encerramento do chamado.** Busca perguntas ancoradas, consulta C6 no pós-estado e restringe o alcance à competência reconhecida como aberta. No sistema, reata todas a uma cobrança sobrevivente válida ou recusa; na transição humana, recusa. Há tripwire no núcleo e guarda equivalente na costura. `app/chamados/models.py:319`, `app/chamados/models.py:374`, `app/chamados/models.py:410`, `app/chamados/models.py:148`, `app/chamados/models.py:208`, `app/chamados/services/fio.py:745`. O BACKLOG distingue F1 da porta F2 de `materializada_em`. `app/docs/BACKLOG.md:874`.
- **O encerramento gravado da pergunta passa por F2:** `carimbar` aplica os campos em memória, consulta C6 e somente grava se a pergunta morrer; `carimbar_identidade` trata as vias de identidade. `fechar_por_admin` usa `admin_decidiu`. `app/chamados/models.py:3050`, `app/chamados/models.py:3097`, `app/chamados/models.py:3103`, `app/chamados/models.py:3168`.
- **Não há via genérica denominada `premissa_morta`.** Há vias distintas como `celula_absolveu`, `chamado_encerrado`, `admin_decidiu` e `motivo_retratado`; não cabe escolher uma delas apenas pelo rótulo do aval. `app/chamados/models.py:2525`.
- **Outros escritores dos campos da pergunta:** limpeza por `descarimbar`/`descarimbar_em_lote`; troca isolada da via por `encerrar_por_fato_no_espelho`; e `relavra_via.relavrar`. `app/chamados/models.py:3203`, `app/chamados/models.py:3251`, `app/chamados/models.py:3268`, `app/chamados/services/relavra_via.py:106`.
- **A reabertura do chamado existe:** `reabrir` consulta `recusa_da_reabertura`, grava `em_analise`, limpa via/instante pelo núcleo e chama `ressuscitar_perguntas`. Há recusas por veto, admin sem fato novo, premissa regularizada, agrupamento, irmão vivo e transição impossível. `app/chamados/models.py:1318`, `app/chamados/models.py:1362`, `app/chamados/models.py:1439`.
- **L-125 é a lei da família cluster, não uma API especial de reabertura.** Ela exige leitura da célula do próprio dia para fechar; ausência de célula/insumos não vira premissa falsa. `app/docs/LEIS.md:146`, `app/ponto/services/cluster_espurio.py:15`, `app/ponto/services/cluster_espurio.py:53`. Além de `reabrir`, `analisar` e as transições humanas escrevem os mesmos campos de estado pelo núcleo; nascimentos e migração 0039 são as exceções descritas acima. `app/chamados/models.py:800`, `app/chamados/models.py:447`, `app/chamados/models.py:1095`, `app/chamados/migrations/0039_a3_via_backfill.py:24`.

A guarda atual de reabertura pode tratar `via='admin'` como impedimento sem procurar um ato substantivo registrado. Logo, ela também não serve como juiz histórico de a/b. `app/chamados/models.py:1342`.

**4. Classe e e relação com o DRY**

Não localizei `backfill_fio_morto_e4b` nesta árvore. Sua implementação e sua chamada exata: **[não verificado]**. A obra consta pelo nome no BACKLOG. `app/docs/BACKLOG.md:1875`.

O sítio disponível que produz o detalhe `pergunta_viva_sob_chamado_terminal` é `vigia_fio.censo_alcance`:

1. Obtém os dias retidos por empresa/competência e encontra a célula por `(colaborador_id, data)`. `app/chamados/services/vigia_fio.py:581`, `app/chamados/services/vigia_fio.py:593`.
2. Chama `alimentar_portas`; suas vivas vêm de **todas** as perguntas da célula, passando pelo juiz `pergunta_viva`, com mapa de identidade do colaborador. `app/chamados/services/vigia_fio.py:603`, `app/chamados/juizes.py:1501`.
3. Resolve a âncora por `chamado_que_lista(p)`: destino, senão chamado da disputa; consulta o status dessas âncoras. `app/chamados/services/fio.py:116`, `app/chamados/services/vigia_fio.py:617`.
4. Somente depois das precedências anteriores atribui classe e e o detalhe terminal. Uma pergunta respondida sob terminal pode cair antes em **c**; “viva sob terminal” isoladamente não reproduz toda a classificação do censo. `app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`.

**5. Sonda de leitura**

O script abaixo mede as perguntas pelo **mesmo C6 e pela mesma consulta da alimentação disponível**, e resolve o dono pela mesma função do fio. Não reexecuta o universo inteiro nem afirma reproduzir a seleção do comando ausente. `app/chamados/juizes.py:1501`, `app/chamados/services/fio.py:116`. Execução no tenant: **[não verificado]**.

**Limitação deliberada:** esta sonda não inventa um juiz retrospectivo a/b. Ela imprime as evidências e deixa a classificação como `indeterminado`, indicando o campo/lacuna que impediu a conclusão. Os baldes a/b contam classificações comprovadas pela sonda, não presumem que o universo real tenha zero casos. O predicado faltante é: **“qual ato/prova substantiva autorizou a travessia vivo→terminal deste ciclo?”** O evento genérico existente não registra essa relação. `app/chamados/models.py:292`, `app/chamados/models.py:3871`.

As competências/empresas do resumo são as do anexo; a empresa atual é impressa separadamente. A lista fornecida e sua correspondência atual no banco: **[não verificado]**.

```python
# Colar no manage.py shell JA no schema do tenant.
# Lista: anexo classe_e_45.txt fornecido; banco [nao verificado].
# C6/consulta: app/chamados/juizes.py:1501.
# Ancora: app/chamados/services/fio.py:116.
# Trilha: app/chamados/models.py:292.
# Limite historico: migrations/0039_a3_via_backfill.py:26.

import json
import logging
from collections import Counter
from contextlib import ExitStack
from datetime import date

from django.db import connection, connections, transaction
from django.contrib.auth import get_user_model
from django.db.models import Q
from django.utils import timezone

from chamados.models import (
    ChamadoColaborador, PerguntaDisputa, DisputaSupervisao,
    DecisaoPergunta, FollowupChamado,
)
from chamados.catalogo.motor import (
    ESTADOS, VIVOS, TERMINAIS, VIAS_ENCERRAMENTO,
)
from chamados.juizes import (
    pergunta_viva, um_colaborador_por_identidade, tranca_em_lote,
)
from chamados.services.fio import chamado_que_lista
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from core.observ import engolidos

DADOS = """
114 2026-11-03 11/2026 3
114 2026-11-06 11/2026 3
209 2026-10-22 11/2026 2
209 2026-10-24 11/2026 2
258 2026-10-26 11/2026 2
267 2026-10-24 11/2026 2
268 2026-10-22 11/2026 2
268 2026-10-24 11/2026 2
298 2026-10-21 11/2026 2
355 2026-10-25 11/2026 2
427 2026-10-22 11/2026 2
427 2026-10-24 11/2026 2
427 2026-10-26 11/2026 2
534 2026-10-23 11/2026 2
534 2026-10-24 11/2026 2
534 2026-10-25 11/2026 2
650 2026-10-23 11/2026 2
650 2026-10-25 11/2026 2
718 2026-10-22 11/2026 2
718 2026-10-24 11/2026 2
718 2026-10-26 11/2026 2
718 2026-10-28 11/2026 2
723 2026-10-28 11/2026 2
865 2026-10-23 11/2026 2
900 2026-11-01 11/2026 3
900 2026-11-03 11/2026 3
900 2026-11-05 11/2026 3
900 2026-11-07 11/2026 3
900 2026-11-09 11/2026 3
918 2026-10-25 11/2026 2
921 2026-10-21 11/2026 2
921 2026-10-22 11/2026 2
921 2026-10-23 11/2026 2
921 2026-10-25 11/2026 2
921 2026-10-26 11/2026 2
921 2026-10-27 11/2026 2
921 2026-10-28 11/2026 2
921 2026-10-29 11/2026 2
923 2026-10-25 11/2026 2
1005 2026-10-28 11/2026 2
1005 2026-10-29 11/2026 2
1005 2026-10-30 11/2026 2
42 2026-10-08 10/2026 4
42 2026-10-15 10/2026 4
258 2026-10-02 10/2026 2
"""

itens = [x.split() for x in DADOS.splitlines() if x.strip()]
saida = []
bloqueios = [0]
ERRO = "ERRO-NAO-MEDIDO"

ACOES = {
    "nascimento_chamado", "estado_chamado",
    "fechament

[truncado em 20000 de 32480 caracteres]

## 10/11 23:54 · sessao bf810b28509c · - · - · origem=Ronald (tmux)

Complemento de exceções ao núcleo:

- `criar(status=...)` aceita explicitamente status terminal, sem validar contra o motor; grava por `objects.create` e deixa `nascimento_chamado`, sem ritual terminal do `_gravar_estado`. `app/chamados/models.py:1054`, `app/chamados/models.py:1063`, `app/chamados/models.py:1095`, `app/chamados/models.py:1218`.
- `abrir(defaults=...)` também admite sobrescrever status inicial: `_def={'status_local':motor...}` recebe `_def.update(defaults)` e segue ao `get_or_create`. `app/chamados/models.py:938`, `app/chamados/models.py:940`, `app/chamados/models.py:971`.
- Busca AST em memória encontrou um chamador explícito de `criar(status=...)`: smoke usa estados do grafo, incluindo terminais. Esse comando recusa banco sem `test` no nome e força rollback. `app/chamados/management/commands/smoke_chamados.py:31`, `app/chamados/management/commands/smoke_chamados.py:39`, `app/chamados/management/commands/smoke_chamados.py:123`, `app/chamados/management/commands/smoke_chamados.py:130`.
- Nenhum chamador atual de `abrir` com defaults terminal foi localizado; possibilidades de argumentos externos ao código inspecionado são **[não verificado]**.
- `app/chamados/admin.py` está vazio; não encontrei registro de `ChamadoColaborador` em Django admin na árvore. Registro em runtime: **[não verificado]**. A existência genérica de `/admin/` está em `app/config/urls.py:24`; não prova uma tela do Chamado.

## 10/11 23:52 · sessao bf810b28509c · - · - · origem=Ronald (tmux)

Auditoria somente leitura. Resultado consolidável:

| Caminho terminal | Escrita específica | Lastro/limite |
|---|---|---|
| `transicionar → _aplicar` | `via_encerramento='admin'`, `respondido_por=user`; destino humano `resolvido/fechado` | Pode representar decisão explícita, mas também troca de status sem texto e comando assinado pelo primeiro superuser. Não classificar automaticamente por via/usuário. `app/chamados/models.py:128`, `app/chamados/models.py:199`, `app/chamados/models.py:447`, `app/chamados/services/acoes_chamado.py:283`, `app/chamados/services/acoes_chamado.py:313`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103` |
| `fechar(user, ..., resolucao)` | Delega a `transicionar`; resolução vira Followup após transição | Resolução opcional; não há campo motivo próprio do chamado. `app/chamados/models.py:452` |
| `reconciliar` | Destino `resolvido`, via `sistema`; não substitui `respondido_por` | Guarda atual pergunta premissa do furo e pergunta viva; permite agrupamento, dedup e chamado sem dia com trilha específica. A via não preserva qual prova matou a premissa. `app/chamados/models.py:641`, `app/chamados/models.py:673`, `app/chamados/models.py:717`, `app/chamados/models.py:732`, `app/chamados/models.py:738`, `app/chamados/models.py:750` |
| `cancelar(motivo, user)` | `cancelado`, via `cancelado`; substitui usuário somente se fornecido; motivo opcional vira Followup | Cancelamento administrativo/emissor aposentado; motivo não é persistido em coluna tipada. `app/chamados/models.py:773`, `app/chamados/models.py:788` |
| `encerrar` | `fechado`, via `cascade`; não substitui usuário | Usado por desligamento, ajuste retroativo, substituição de aceite antigo e regularização externa. O gate não exige prova de premissa morta; só pergunta viva atual. `app/chamados/models.py:1605`, `app/colaboradores/services/desligamento.py:155`, `app/ponto/services/justificativa.py:400`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/services/regularizacao_ext.py:205` |
| `encerrar_por_competencia_trancada` | `fechado`, via `competencia_trancada`; serviço acrescenta nota e evento com competência/empresa | Código chama de lastro, mas esclarece que não significa resolução pela realidade. Tranca efetiva é diferente de mera idade/janela. `app/chamados/models.py:1622`, `app/chamados/services/competencia_trancada.py:3`, `app/chamados/services/competencia_trancada.py:12`, `app/chamados/services/competencia_trancada.py:181` |
| Migração 0039 | Cancelado com irmão único vira `superado`, via `superado`, `superado_por_id`; demais cancelados recebem via `cancelado` | Acervo histórico; não grava log, ator nem timestamps nesse backfill. Porta atual `superar()` foi removida. `app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/models.py:794` |

Ritual comum dos gates atuais: escreve `status_local`, `via_encerramento`, `resolvido_em`, `atualizado_em`, rebaixa urgência preservando a original; `respondido_por` só muda quando `set_user=True`. Registra `estado_chamado` com antes `{status_local}`, depois `{status_local, via}`, ator indicado ou contexto da request/processo. `app/chamados/models.py:228`, `app/chamados/models.py:240`, `app/chamados/models.py:292`, `app/core/observ.py:94`, `app/core/observ.py:108`.

Pontos forenses essenciais:

- Não existe `fechado_por` nem campo próprio de motivo no Chamado; existem `respondido_por`, `via_encerramento`, `resolvido_em`. `fechada_por`, `motivo_fechamento`, `resolucao` pertencem à Disputa. `app/chamados/models.py:52`, `app/chamados/models.py:58`, `app/chamados/models.py:1757`, `app/chamados/models.py:1766`, `app/chamados/models.py:1773`, `app/chamados/models.py:1793`.
- **`via='admin'` não prova ato real:** migração 0039 inferiu esse valor exclusivamente de `respondido_por` diferente dos usernames sistema/system/cron; o comando meio período também produz via admin assinada por superuser escolhido automaticamente. `app/chamados/migrations/0039_a3_via_backfill.py:14`, `app/chamados/migrations/0039_a3_via_backfill.py:26`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:103`.
- **Usuário pode ser resíduo de outro ciclo:** reconciliar/cascade/tranca não o substituem. `resolvido → fechado` preserva `resolvido_em` e sobrescreve via; a última linha terminal pode não ser o ato que matou a vida. Necessário reconstruir a travessia `VIVOS → TERMINAIS` do ciclo corrente e verificar coerência da cadeia com o banco. `app/chamados/models.py:230`, `app/chamados/models.py:242`, `app/chamados/models.py:654`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`.
- **Followup não persiste `tipo` nem `objeto_ref`.** Só autor/tipo de autor/conteúdo/instante/hash; `tipo` participa de hash apenas quando há referência. Sonda não pode filtrar `followups__tipo`. `app/chamados/models.py:1668`, `app/chamados/models.py:1688`, `app/chamados/services/nota.py:87`, `app/chamados/services/nota.py:95`.
- Auditoria é best-effort: ausência de linha não prova ausência de ato, logo não basta para classificar **b**. `app/core/observ.py:94`, `app/core/observ.py:105`, `app/core/observ.py:133`.
- Eventos estruturados auxiliares: `fechamento_por_dedup` traz dia/sobrevivente/motivo; `fechamento_sem_dia_do_fato` declara julgamento impossível; `disputa_fechada` traz motivo/resolução/usuário/perguntas vivas anteriores; serviço da tranca traz competência/empresa. Nenhum fornece FK do evento substantivo para o evento genérico que encerrou o chamado. `app/chamados/models.py:690`, `app/chamados/models.py:732`, `app/chamados/models.py:750`, `app/chamados/models.py:1937`, `app/chamados/services/competencia_trancada.py:190`.

Lastros específicos e caminhos indiretos:

- Disputa `fechar()` exige usuário, recusa resposta sem veredito; sem exceção exige C6 vazio. Exceções tipadas: `desligamento`, `admin_encerra`, `competencia_trancada`; admin exige motivo. Escreve `fechada_em/por`, `resolucao`, `motivo_fechamento`; em silêncio de admin/desligamento grava decisão por pergunta e trilha. Signal tenta reconciliar pai e acrescenta nota Defesa 1. `app/chamados/models.py:1855`, `app/chamados/models.py:1884`, `app/chamados/models.py:1895`, `app/chamados/models.py:1918`, `app/chamados/models.py:1932`, `app/chamados/models.py:1943`, `app/chamados/signals.py:42`.
- Decisão da célula fecha pergunta, registra `DecisaoPergunta.SILENCIO`, reconcilia chamados e deixa Followup; também lavra célula/log do veredito. `app/ponto/services/veredito_celula.py:179`, `app/ponto/services/veredito_celula.py:194`, `app/ponto/services/veredito_celula.py:201`, `app/ponto/services/veredito_celula.py:345`.
- Marcos acesos: `lastro.py:394`, `lastro.py:400`; sync: `entrada_sincronizada.py:29`, `entrada_sincronizada.py:31`; todos em `app/ponto/services/`.
- Ausência decidida: `app/ponto/services/ausencia.py:692`, `app/ponto/services/ausencia.py:694`; ausência aprovada/cobranca já sem premissa: `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/silencio_ausencia.py:108`.
- Feriado/folga: juiz e evento `reconciliar_feriado` em `app/chamados/services/feriado_reavaliar.py:23`, `app/chamados/services/feriado_reavaliar.py:43`.
- Cluster: a janela recorta abertura; fechamento hoje lê célula do próprio dia. Lápide confirma que antes fechamento por janela confundia não medido com falso. `app/ponto/services/cluster_espurio.py:40`, `app/ponto/services/cluster_espurio.py:53`, `app/ponto/services/cluster_espurio.py:124`.
- Cron de prescrição atual **não encerra**: silencia; `arquivar_competencia_encerrada` tem lápide da expiração antiga por conversa parada 30 dias, revogada na C7. Estado/campos históricos exatos desse código removido: **[não verificado]**. `app/chamados/management/commands/arquivar_prescritos.py:6`, `app/chamados/management/commands/arquivar_prescritos.py:94`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:30`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`.

Inventário completo de chamadas diretas a `reconciliar` enviado nas mensagens anteriores; todas convergem ao mesmo núcleo. O código não contém um juiz retrospectivo completo de **a/b**. Uma sonda rigorosa deve imprimir evidências estruturadas e manter indeterminado quando não houver prova suficiente de autoria substantiva e ligação ao fechamento daquele ciclo. `app/chamados/models.py:292`, `app/chamados/migrations/0039_a3_via_backfill.py:26`, `app/core/observ.py:98`.

## 10/11 23:50 · sessao 0d6967cc6e14 · - · - · origem=Ronald (tmux)

**Portas existentes e limites, para Q3.**

- **O261-E F1 é a guarda no encerramento do chamado**, não um método genérico “encerre a pergunta porque o chamado morreu”. Ela compõe `pergunta_viva` (C6), `competencia_do_dia_entregue` e a âncora `chamado_que_lista`; busca perguntas não validadas cujo destino é o chamado ou, sem destino, cujo pai da disputa é o chamado. Julga no pós-estado terminal e só alcança competência reconhecida como aberta (`False`); entregue ou indeterminada fica fora. `app/chamados/models.py:319-369`.
- Pelo sistema, `_gate_pergunta_viva` reata **todas** as vivas a uma cobrança sobrevivente válida do mesmo colaborador/dia ou recusa o fechamento com `fechamento_recusado`, motivo `pergunta_viva_na_competencia_aberta`. O reaponte usa `fio.reapontar_resolvedora` e registra `pergunta_reatada`. `app/chamados/models.py:374-445`.
- Pela transição humana, `transicionar` recusa com mensagem se houver pergunta viva ancorada; não reata automaticamente. `app/chamados/models.py:148-171`.
- `_gravar_estado` contém tripwire adicional antes de tocar nos campos: travessia vivo→terminal com pergunta viva levanta `ValueError`. `app/chamados/models.py:208-227`.
- A guarda equivalente na **costura** é `fio._costura_nao_mata_o_fio`; lê status fresco do banco e recusa costurar pergunta viva da competência aberta em terminal. `app/chamados/services/fio.py:745-787`.
- O BACKLOG identifica F1 “porta de encerramento”, F2 “porta de materializada_em”, e declara F1/F2 no ar. `app/docs/BACKLOG.md:874`.

**A porta que efetivamente grava o encerramento da pergunta é o carimbo F2.**

- `carimbar(via, autor, ...)` aplica em memória `materializada_em`, `materializada_por_id`, `via_resolucao`; opcionalmente `respondida_em` e validação com texto; consulta C6; se continuar viva, desfaz e retorna `False`; só persiste se morta. `app/chamados/models.py:3097-3157`.
- `carimbar_identidade` trata as vias cujo próprio ato decide identidade: `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`, `motivo_retratado`. Não pergunta ao C6 porque circular; grava os mesmos três campos e opcional `recusa_motivo`. `app/chamados/models.py:2503-2510`, `app/chamados/models.py:3168-3191`.
- `fechar_por_admin` é wrapper idempotente da via `admin_decidiu`. `app/chamados/models.py:3050-3064`.
- `fechar_o_fio_do_dia` chama esse wrapper e registra `DecisaoPergunta.SILENCIO` com `ato_em=materializada_em`, autor e motivo `dia_decidido_pelo_admin: DATA`; depois reconcilia os chamados vivos daquele dia. `app/ponto/services/veredito_celula.py:160-206`.
- Não há nessa relação uma via chamada `premissa_morta`; as vias declaradas incluem `celula_absolveu`, `chamado_encerrado` e `legado_sem_lastro`, cujos significados são distintos. Não se deve inventar uma via para aplicar o aval. `app/chamados/models.py:2525-2542`.
- A antiga porta de escrita da via pergunta `competencia_trancada` foi removida; hoje a tranca mata por leitura no C6. `app/chamados/models.py:3066-3071`.

**Quem escreve esses campos além da aplicação normal do carimbo.**

- O ponto de atribuição conjunta é `_escrever_carimbo`; limpeza individual vai por `descarimbar`; limpeza em lote por `descarimbar_em_lote`. `app/chamados/models.py:3097-3101`, `app/chamados/models.py:3203-3213`, `app/chamados/models.py:3251-3266`.
- `via_resolucao` tem dois escritores próprios adicionais: `encerrar_por_fato_no_espelho` troca só a via para `fato_materializado`; `relavra_via.relavrar` reescreve via derivada (`declaracao`/`completude_turno`) e grava trilha antes/depois. `app/chamados/models.py:3268-3276`, `app/chamados/services/relavra_via.py:76-78`, `app/chamados/services/relavra_via.py:106-138`.
- O selo AST explicita essas exceções; `RegularizacaoExterna.materializada_em` é homônimo de outro modelo. O selo não alcança `__dict__`, campo vindo de outro módulo/atributo, nem o `SET_NULL` que apaga o autor quando usuário é removido. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:43-58`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111-115`.

**Reabertura do chamado e L-125.**

- A porta existente é `ChamadoColaborador.reabrir`, que consulta a guarda pura `recusa_da_reabertura`, passa por `decidir(..., 'REABRIR')`, grava `em_analise` pelo núcleo e chama `ressuscitar_perguntas`. `app/chamados/models.py:1362-1452`.
- As guardas incluem veto do dia, `admin_sem_fato_novo`, premissa regularizada, agrupamento, irmão vivo e transição não permitida. São leituras **do estado atual**, não classificação histórica de quem fechou. `app/chamados/models.py:1318-1360`.
- `via_encerramento='admin'` com `resolvido_em` pode bloquear reabertura se não houve fato novo; a guarda não procura ato humano registrado. `app/chamados/models.py:1342-1347`.
- Núcleo de escrita comum `_gravar_estado`: vivo limpa `resolvido_em` e via; terminal carimba/preserva instante, via e urgência; opcionalmente grava `respondido_por`; trilha `estado_chamado` com antes/depois e ator. `app/chamados/models.py:228-253`, `app/chamados/models.py:292-296`.
- **L-125 é lei da família cluster**, não nome de API de reabertura: cluster morre somente pelo lastro da célula do próprio dia; janela recorta abertura. `app/docs/LEIS.md:146`, `app/ponto/services/cluster_espurio.py:53-78`.
- A rotina atual de cluster mede ausência de assinatura pela ata, distinguindo `None` (“não medido”), `()` (“medido sem assinatura”) e assinatura; reconcilia fechamentos sem limite etário. Isso mede premissa atual, não prova o que havia no fechamento antigo. `app/ponto/services/cluster_espurio.py:15-37`, `app/ponto/services/cluster_espurio.py:53-78`, `app/ponto/services/cluster_espurio.py:121-125`.

**Achado forense crítico para Q2/sonda:** `via_encerramento='admin'` **não comprova lastro**, porque a migration legada preencheu esse valor por mera existência de `respondido_por` que não estivesse na lista de usuários sistema; os demais terminais receberam `legado`. `app/chamados/migrations/0039_a3_via_backfill.py:26-28`. O catálogo atual também diz apenas por qual evento a via sai (`TRANSICAO_HUMANA→admin`, `RECONCILIAR→sistema`, etc.), não certifica a existência de decisão real. `app/chamados/catalogo/motor.py:86-96`.

## 10/11 23:50 · sessao c39da2d246d7 · - · - · origem=Ronald (tmux)

- `backfill_fio_morto_e4b` não está entre os arquivos localizados; o comando da cópia e sua chamada exata não podem ser certificados. **[não verificado]** O BACKLOG registra a obra como backfill DRY das classes a–e, pela porta canônica, em `app/docs/BACKLOG.md:1875`.

- O sítio disponível que mede a classe **e / pergunta_viva_sob_chamado_terminal** é `censo_alcance`, em `app/chamados/services/vigia_fio.py:542`. O universo vem de `dias_que_seguram` por empresa/competência, em `:581–587`; a célula é ligada por `(colaborador_id, data)`, em `:593–597`; a alimentação vem de `alimentar_portas`, em `:603–610`.

- **Consulta fiel das perguntas vivas**: `PerguntaDisputa.objects.filter(celula_id__in=_ids).select_related('disputa', 'celula').order_by('pk')`; percorre `um_colaborador_por_identidade` e conserva somente `pergunta_viva(p)`, em `app/chamados/juizes.py:1501–1505`. Não usar somente `q_vivas`: ele é filtro generoso, com diferenças documentadas que exigem avaliação do juiz, em `app/chamados/models.py:3336–3343`.

- **Dono/âncora fiel**: `chamado_que_lista(p)` retorna `p.chamado_destino_id` ou, na falta dele, `p.disputa.chamado_id`, em `app/chamados/services/fio.py:116–117`. O vigia consulta o status dessas âncoras em `app/chamados/services/vigia_fio.py:617–618`. Terminal significa `resolvido`, `fechado`, `cancelado` ou `superado`, em `app/chamados/catalogo/motor.py:30–31`.

- A classe **e** depende da precedência: ausência de célula, véu, resposta vazia, respondida sob destino terminal e cadastro são tratados antes; somente depois o vigia testa `any(s in TERMINAIS for s in anc_st)` e dá o detalhe pedido, em `app/chamados/services/vigia_fio.py:667–688`. Portanto “alguma viva sob terminal” isoladamente não reproduz a classificação completa do censo. `ps` já é a lista julgada por C6, em `:640` e `:671–672`.

- Para a sonda dos dias, buscar `CelulaDia` pelo `(colaborador_id, data)` é fiel ao vigia; a unicidade é declarada em `app/escala/models.py:1833`. Buscar somente `PerguntaDisputa.data_evento` pode divergir: o código reconhece que a célula vinculada pode não ser a de `data_evento`, em `app/chamados/juizes.py:627–641`.

- **Erro engolido é medida inválida**: C6 captura falhas e retorna fallback em `app/chamados/juizes.py:1396–1398` e `:1407–1410`; a janela também captura falhas em `:599–602`. Comparar `core.observ.engolidos()['total']` antes/depois de cada avaliação detecta esses erros: `engolidos()` devolve cópia do contador em `app/core/observ.py:60–63`; `registrar_engolido` incrementa-o em `:73–77`. Se aumentar, emitir `ERRO-NAO-MEDIDO`, nunca contar como falsa/morta/zero.

- Evitar imprimir `str(exc)`, `descricao`, respostas e JSON brutos: tais campos são texto livre (`app/core/models.py:299–301`, `app/chamados/models.py:2447`). Exibir apenas classes de exceção e identificadores numéricos permite a saída raspada solicitada. A presença efetiva de nomes nesses campos dos 45 é **[não verificado]**.

Esqueleto de leitura equivalente ao trecho de alimentação do vigia, sem a fila desnecessária à pergunta específica:

```python
from chamados.juizes import pergunta_viva, um_colaborador_por_identidade
from chamados.models import PerguntaDisputa
from chamados.services.fio import chamado_que_lista
from core.observ import engolidos

# cel e col já lidos pelo (colaborador_id, data).
qs = (
    PerguntaDisputa.objects.filter(celula_id=cel.pk)
    .select_related("disputa", "celula")
    .order_by("pk")
)
vivas = []
for p in um_colaborador_por_identidade(qs, {col.pk: col}):
    antes = engolidos()["total"]
    viva = pergunta_viva(p)
    if engolidos()["total"] != antes:
        raise RuntimeError("ERRO-NAO-MEDIDO")
    if viva:
        vivas.append((p, chamado_que_lista(p)))
```

Esse esqueleto mede o estado atual pelo juiz disponível; reconstruir a vitalidade na data do DRY sem snapshot/trilha suficiente é **[não verificado]**.

## 10/11 23:47 · sessao b1b98eda2f6e · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura K3 aplicada: seed preservado na nova/vazia, exceto quando `orfao_14h` do ato é estrangeiro ([fonte](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/services/disputa_emissao.py:1451)). [nao verificado]

- [Entrega com censo e comandos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5k_entrega.txt:1).
- [Diff completo](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5k.diff:1).
- 114 testes de memória verdes; RED anterior e mutantes documentados nas [saídas](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5k_saidas.txt:1). [nao verificado]

Suíte ORM e certificação final: **[nao verificado]**, reservadas ao Code e opus. [nao verificado]

## 10/11 23:38 · sessao 808c2ef83d03 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

[Entrega K1–K5 e arquivos tocados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_entrega.txt:1), com [comandos e saídas integrais](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_saidas.txt:8).

147 testes em memória passaram; 37 mutantes produziram RED com controle verde ([registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_entrega.txt:5)). [nao verificado]

K3 permanece ACHADO. ORM e ruff=0: **[nao verificado]**, por ausência de Django e ruff ([limites](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5j_entrega.txt:9)).

## 10/11 23:24 · sessao fd6adff78494 · - · - · origem=Ronald (tmux)

K4: `27 != 28` compara **PKs**, não contagens (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:138`). A porta seleciona a primeira disputa aberta (`chamados/services/disputa_emissao.py:1290`), ordenada por `-aberta_em` (`chamados/models.py:1804`). O campo usa `auto_now_add=True`; não há evidência de defeito no patch de relógio (`chamados/models.py:1756`).

A local vazia nasce antes dos chamados (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:130`). Criar chamados aciona o cartório (`chamados/signals.py:188`); um encerramento aciona S143 (`chamados/signals.py:91`), cujo resolver varre todas as disputas abertas e fecha as vazias (`chamados/services/disputa_emissao.py:2130`, `chamados/services/disputa_emissao.py:2178`). **Qual chamado efetivamente provocou esse caminho no traceback: [nao verificado].** Correção defensável da fixture: criar a local vazia por último e verificar, antes da chamada, que continua aberta, sem motivos/perguntas e posterior à dona; preservar todas as asserções finais.

K5: o repasse chama `ch.analisar()` (`chamados/services/disputa_emissao.py:1492`, `chamados/services/disputa_emissao.py:1496`), acordando o cartório; ele ressuscita perguntas (`ponto/services/cartorio.py:1633`), cuja porta reabre a dona (`chamados/models.py:1573`). A reabertura limpa somente `fechada_em`, `fechada_por`, `resolucao` e `motivo_fechamento`, anotando o chamado quando existe (`chamados/models.py:2173`). O teste deve exigir essa transformação exata e preservar os demais campos.

Na segunda passada do replay, a dona já aberta pode ser selecionada, receber o motivo e ser retornada com push; exigir novamente `None` e nenhum push conflita com esse caminho (`chamados/services/disputa_emissao.py:1290`, `chamados/services/disputa_emissao.py:1453`, `chamados/services/disputa_emissao.py:1516`). Separar as expectativas da primeira passagem das da segunda.

O `SystemExit(2)` do auditor é um alarme deliberado quando F1/F2 são positivos; o teste deve capturá-lo, exigir código 2 e continuar verificando I23 (`chamados/management/commands/auditar_invariantes_chamados.py:1942`).

## 10/11 23:13 · sessao cc7466557084 · FIO-E0-V4-E4B · leitura · origem=Code

**Os dois resultados vêm de perguntas diferentes no código:** o command consulta ações exigíveis por instância de marco; o vigia usado pelo command consulta retenção da folha e alcançabilidade de uma porta. O vigia não consulta `tarefa_do_dia`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–76`; `app/chamados/services/tarefa_do_dia.py:235–288`; `app/chamados/services/vigia_fio.py:635–699`)

Os números 253, 116, 64, 1 resolvido e 385 → 384 são as medições fornecidas no enunciado. A distribuição dos 116 e dos 64 entre os ramos abaixo, e os insumos de cada dia, permanecem **[nao verificado]**.

**P1. Quem diz “nada” e quem diz “morto”.**

O caminho da tarefa é `ato_do_dia` → `tarefa_do_dia` → `CargaDaTarefa.tarefa` → `_tarefa_pura`. A função pública exige `agora`; na chamada avulsa do command, carrega exatamente `[dia, dia]`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:73–76`; `app/chamados/services/tarefa_do_dia.py:396–406`; `app/chamados/services/tarefa_do_dia.py:442–452`)

Há uma distinção relevante: **`tarefa_nada` é um rótulo construído pelo command**, não o nome de um retorno direto do juiz. Ele aparece quando:

- não foram acumulados `registrar_decisao`, `perguntar`, `materializar` ou `rejulgar`;
- não foi encontrada ação com `porta is None`;
- `t['nao_sei']` é falso.

Ações humanas **com porta preenchida** não entram na lista `humanos`; portanto, isoladamente, `tarefa_nada` não prova que todas as listas `acoes` estavam vazias. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`)

| Aspecto | Predicado da tarefa |
|---|---|
| Universo de perguntas | `PerguntaDisputa` pela coluna `colaborador_id`, `conversa=False`, `data_evento` dentro de `[ini,fim]`, ordenadas por colaborador e PK. Não filtra por pergunta viva, disputa aberta, validação ou materialização. (`app/chamados/services/tarefa_do_dia.py:323–329`) |
| Identidade considerada | Somente motivos de marco; exclui vias fora do fato, com a exceção temporal da retratação posterior a `agora`; exclui última decisão de retratação. Agrupa por `data_evento == dia` e posição/instância do marco. (`app/chamados/services/tarefa_do_dia.py:223–234`) |
| Decisão considerada | Sequência de `DecisaoPergunta`, ordenada por `(ato_em,pk)`, considerando somente `ato_em <= agora`; a última linha dessa sequência governa a ação. (`app/chamados/services/tarefa_do_dia.py:337–342`; `app/chamados/services/tarefa_do_dia.py:129–159`) |
| Dispensas do dia | `fora_vinculo`, isenção e competência trancada retornam sem ação. Sem célula retorna `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:180–201`) |
| Ausência de lâmpadas | Em dia que não é dispensado por trabalho, sem instâncias: havendo marcos e nenhum vencido, retorna `nao_venceu`; caso contrário, `nao_sei/ata_sem_lampadas`. (`app/chamados/services/tarefa_do_dia.py:202–210`) |
| Dispensas por instância | Sem ação quando dia sem trabalho, lâmpada `True`, marco não vencido ou ausência aprovada. Lâmpada `None` ou teto desconhecido produzem `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:241–260`) |
| Marco que exige pergunta | Lâmpada apagada, marco vencido, sem cobertura que desvie a ação e sem pergunta da posição: exige `PERGUNTAR`. (`app/chamados/services/tarefa_do_dia.py:258–279`) |
| Encerramentos pela sequência | Validação com lavra vigente `sim` e sem Batida de efeito: `encerrada_pelo_juiz`; silêncio: `silencio_decidido`; absorção em ausência aprovada: `absorvida_aprovada`. Todos retornam ações vazias. (`app/chamados/services/tarefa_do_dia.py:140–154`) |
| Demais ações | Resposta, recusa, reabertura, devolução e absorção não final podem exigir ação humana; validação com lavra `nao` exige materialização; efeito presente com lâmpada ainda apagada exige rejulgamento. (`app/chamados/services/tarefa_do_dia.py:129–159`) |

O `dia_sem_trabalho` compartilhado considera `trabalha is False` **ou** `ata.tipo_dia` preenchido e diferente de `trabalho`. Para ausência parcial que cobra o resto, somente `trabalha is False` dispensa o dia inteiro. (`app/chamados/juizes.py:413–441`; `app/ponto/supra_juiz.py:65–71`)

O vigia usado neste backfill é **`censo_alcance`**, selecionando `alcance_dias` com rota `sem_porta`; não é o `censo` antigo que começa em chamado vivo com célula. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–70`; `app/chamados/services/vigia_fio.py:493–499`)

| Aspecto | Predicado do vigia usado |
|---|---|
| Empresas | Empresas visíveis; o recorte solicitado é uma interseção. A fonte de visibilidade exige empresa ativa e exclui o prefixo de CNPJ declarado nela. (`app/chamados/services/vigia_fio.py:566–570`; `app/colaboradores/queries.py:78–87`) |
| Competências | A atual entra sempre no enumerador; a anterior entra se não estiver entregue. Não há teste de entrega da **atual** nesse enumerador. (`app/ponto/services/he_pendente_lavrado.py:218–225`) |
| Universo inicial | `dias_que_seguram`: lê `classificar_export`, seleciona colaboradores cujo motivo é `furo_espelho` e obtém seus dias pela retenção da célula. O censo deduplica `(colaborador,data)`. (`app/folha/fora_da_folha.py:95–99`; `app/folha/fora_da_folha.py:118–137`; `app/chamados/services/vigia_fio.py:581–588`) |
| Modelos/status da folha | `classificar_export` começa em `FechamentoMensal` da empresa/mês/ano, sem filtro de status; `furo_espelho` só é atribuído quando a cadeia chega ao teste da retenção. (`app/folha/export.py:757–780`; `app/folha/export.py:960–965`) |
| Retenção do dia | Exclui isento, recorta admissão/demissão e lê `CelulaDia` por `data__range`. Retém acusação, salvo protestos inteiramente não cobráveis ou exceção da pausa; também retém cegueira passada nas condições expressas. (`app/folha/export.py:1192–1206`; `app/folha/export.py:1224–1257`) |
| Vereditos acusadores | `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`. (`app/escala/models.py:1767–1768`) |
| Perguntas examinadas | Por **`celula_id`**, sem filtro explícito `conversa=False` ou `data_evento`; seleciona as vivas pelo C6 `pergunta_viva`. (`app/chamados/juizes.py:1502–1505`) |
| Porta alcançável, C17 | Chamado da célula na fila **ou** pergunta viva da célula, presente no universo do app, com âncora na fila e sem resposta vazia ilegal. (`app/chamados/juizes.py:1574–1589`) |
| Fila/status | `VIVOS = aberto, em_analise, registrado`; a fila exige `COBRANCA = aberto, em_analise` e exclui `silenciado_ate > agora`. (`app/chamados/catalogo/motor.py:24–31`; `app/chamados/services/painel.py:188–190`) |
| Exclusões antes de contar morto | Havendo porta, sai; hoje/futuro sai; depois saem Resolver dia oferecido, ausência aberta cobrindo, aceite de folga validado e dia na lista CxR com dono BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:635–660`; `app/chamados/services/vigia_fio.py:306–315`) |

O C6 lê projeção/carimbos, tranca, vias, resposta, veredito da célula, fato do marco e materialização. Seu corpo não decide pela última `DecisaoPergunta`, como faz a tarefa. (`app/chamados/juizes.py:1324–1410`; `app/chamados/services/tarefa_do_dia.py:129–159`)

A condição exata da divergência é a conjunção:

- **“Morto”**: dia no universo retido, C17 sem porta, `data < hoje` e nenhum dos quatro baldes de dono.
- **`tarefa_nada`**: redução do command sem ato de sistema/passivo, sem ação com porta nula e `nao_sei=False`.

Assim, retenção agregada e ausência de porta podem coexistir com dispensa ou encerramento das ações por marco. O código desses dois caminhos não exige equivalência entre as respostas. (`app/chamados/services/vigia_fio.py:635–699`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`; `app/chamados/services/tarefa_do_dia.py:180–288`)

Para ser especificamente **classe `d`**, ainda é necessário: célula existente; nenhum `veu` de chamado vivo da célula ou âncora viva fora da fila; nenhuma pergunta C6 viva ligada à célula; dia fora da lista CxR; colaborador presente na lista CxR. Isso não significa “nenhuma pergunta histórica existe para o fato”. (`app/chamados/services/vigia_fio.py:661–678`; `app/chamados/services/fabrica_da_celula.py:145–157`)

Um caminho suficiente demonstrável é: dia que permanece retido e sem porta/dono, mas cujo juiz de tranca responde verdadeiro; a tarefa retorna sem ação por `competencia_trancada`, enquanto a retenção continua lendo veredito/protestos. Ocorrência desse caminho nos 116: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:190–198`; `app/folha/export.py:1233–1256`; `app/chamados/services/vigia_fio.py:635–699`)

**Existe juiz registrado:** `_C19` aponta para `tarefa_do_dia`; o command chama essa autoridade. O vigia consulta C17/C6 e a cadeia de dono, sem chamar C19 no corpo de `censo_alcance`. O registro declara que os cinco respondedores anteriores continuam vivos durante essa etapa. (`app/core/juizes.py:150–155`; `app/core/juizes.py:289–292`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:75–76`; `app/chamados/services/vigia_fio.py:555–564`; `app/chamados/services/vigia_fio.py:635–656`)

**P2. Ramos da fábrica sem inserção de `PerguntaDisputa`.**

Primeiro, o limite da medição: **`segue_sem_porta` não prova zero perguntas criadas nem zero perguntas vivas depois do ato**. O command descarta o retorno da fábrica e classifica pelo reaparecimento do `celula_id` no recenso `sem_porta`; a classe/detalhe impressos são os guardados no plano anterior. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:195–196`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:240–258`)

O caminho é `perguntar_pela_celula` → `motivos_a_perguntar` → `motivos_pela_ata`; com motivos, segue por `perguntar` → `criar_ou_atualizar_disputa` → `gerar_perguntas_disputa`. Depois de obter a disputa, `perguntar` chama o gerador novamente. (`app/chamados/services/fabrica_da_celula.py:160–175`; `app/chamados/services/fabrica_da_celula.py:201–220`; `app/chamados/services/disputa_emissao.py:669–688`)

**Nenhum retorno normal abaixo é distinguido pelo command:** tanto `[]` quanto uma lista de motivos são descartados. Exceção que escapa recebe tratamento diferente: rollback do dia e `recusa:<tipo>`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116–117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:236–239`)

| Ramo | Condição e saída |
|---|---|
| Sem adesão | `veredito == 'nunca_bateu'`: retorno imediato `[]`, inclusive antes da pauta de cadastro. Esse veredito pertence ao conjunto acusador da retenção. (`app/chamados/services/fabrica_da_celula.py:87–89`; `app/chamados/services/fabrica_da_celula.py:183–184`; `app/escala/models.py:1767–1768`) |
| Passado sem turno previsto | Dia passado, colaborador ativo, não dispensado por `dia_sem_trabalho`, e nenhum marco vencido **nem pendente**: `vencidos_no_instante` devolve `None`; tenta pauta de cadastro e retorna `[]`. Vínculo/tipo ausente ou cadeia vazia podem produzir ausência de marcos. (`app/chamados/services/fabrica_da_celula.py:45–49`; `app/chamados/services/fabrica_da_celula.py:201–205`; `app/ponto/services/cartorio.py:840–859`) |
| Pauta de cadastro já entregue | Após a trava, a pauta não é repetida se existe cabeça viva do colaborador ou pauta fechada em data civil local `>= dia`. O booleano do escritor é ignorado; com pauta nova ou antiga a fábrica retorna `[]`. (`app/chamados/services/fabrica_da_celula.py:327–335`; `app/chamados/services/fabrica_da_celula.py:364–373`; `app/chamados/services/fabrica_da_celula.py:204–205`) |
| Guardas gerais | `dia_sem_trabalho`, situação atual diferente de `ativo` ou dia futuro: motivos vazios. Veredito preenchido fora de `VEREDITOS_ACUSAM` também devolve vazio; veredito vazio não cai nessa guarda. (`app/chamados/services/fabrica_da_celula.py:105–121`) |
| Parcial somente na lista | `cobertura_parcial` com `periodo_parcial == 'sem_periodo'`; ou parcial com julgamento anterior e sem fato novo desde ele. O command não passa `julgada_antes`, portanto a fábrica usa `cel.julgada_em`. Retorna motivos vazios. (`app/ponto/services/cartorio.py:395–400`; `app/chamados/services/fabrica_da_celula.py:123–125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:117`) |
| Nenhum marco venceu | `motivos_pela_ata` retorna `[]` para teto vazio ou `None`. Havendo somente marcos pendentes, não abre pauta de falta de turno: existem marcos, apenas nenhum venceu. (`app/chamados/services/fabrica_da_celula.py:45–49`; `app/chamados/services/fabrica_da_celula.py:58–61`) |
| Exceção engolida ao ler ata | `_ata_do_dia` captura a exceção e retorna `(True,None)`. Apesar do comentário “falha de leitura não barra”, este chamador barra porque `faltantes is None`. (`app/chamados/services/disputa_emissao.py:894–901`; `app/chamados/services/fabrica_da_celula.py:64–66`) |
| Ata ausente/agregada | Estado `sem_celula` com escala vigente retorna `(False,None)`; sem escala retorna `(True,None)`. Ambos impedem motivos neste caminho. A ausência de pares de marcos no leitor também pode produzir `sem_celula`. (`app/chamados/services/disputa_emissao.py:902–913`; `app/chamados/services/declaracao_texto.py:121–127`; `app/chamados/services/fabrica_da_celula.py:65–66`) |
| Nenhum faltante elegível | O leitor exclui isento/fora do vínculo, dia não trabalho sem parcial cobrando resto, marcos sem E/S ou hora e células sem `missing`. A fábrica exige adicionalmente marco vencido, data igual à célula e posição/tipo mapeados. A contribuição da véspera não passa por `f[0] == dia`. (`app/chamados/services/declaracao_texto.py:101–143`; `app/chamados/services/fabrica_da_celula.py:68–74`) |
| Dedup anterior à emissão | Remove motivos que já têm linha do fato: colaborador, dia, motivo, instância, `conversa=False`, fora `duplicata/motivo_retratado`. **Não exige vitalidade, disputa aberta ou ausência de validação/materialização.** Se todos foram removidos, não chama `perguntar`; ainda pode tentar desfazer carimbos existentes. (`app/chamados/services/fabrica_da_celula.py:145–157`; `app/chamados/models.py:2746–2755`; `app/chamados/models.py:2393`; `app/chamados/services/fabrica_da_celula.py:207–220`) |
| Limbo entregue | `trabalha is None`, motivos presentes e competência entregue: pauta DP e `[]`. Dedup da pauta considera cabeça do dia, DP e marcador, inclusive feita/cancelada. (`app/chamados/services/fabrica_da_celula.py:214–231`; `app/chamados/services/fabrica_da_celula.py:257–285`) |

Esses ramos não têm todos a mesma possibilidade de explicar os 64: dia futuro sai do vigia antes de `sem_porta`; entrega detectada pelo backfill sai como `bang:*` antes da fábrica. Em estado estável, essas condições não explicam um `segue_sem_porta` executado. A participação das demais guardas nos 64 é **[nao verificado]**. (`app/chamados/services/vigia_fio.py:648–651`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:194–197`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:227–233`)

Mesmo recebendo motivos, o gerador ainda pode terminar sem inserir:

| Ramo interno | Condição |
|---|---|
| Catálogo/texto/data | Motivo sem pergunta no catálogo; texto datado sem data e sem fallback da grade; pergunta de hora sem dia do turno: `continue`. A leitura de fallback da grade também captura exceção e devolve `None`. (`app/chamados/services/disputa_emissao.py:75–89`; `app/chamados/services/disputa_emissao.py:314–316`; `app/chamados/services/disputa_emissao.py:346–366`; `app/chamados/services/disputa_emissao.py:425–430`) |
| Dedup genérico | Motivo não datado sem episódio já existente; intervalo sem data ou já existente na disputa/dia; conversa repetida: `continue`. Os quatro motivos normais da fábrica são datados, de hora, e recebem contexto ISO do dia; os ramos não datados/conversa não são seu caminho normal. (`app/chamados/services/disputa_emissao.py:26`; `app/chamados/services/disputa_emissao.py:325–329`; `app/chamados/services/disputa_emissao.py:395–401`; `app/chamados/services/disputa_emissao.py:431–438`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/services/declaracao_texto.py:42–47`; `app/chamados/catalogo/perguntas.py:48–58`; `app/chamados/catalogo/perguntas.py:124–134`) |
| Dedup com destino não derivado | Para motivos que exigem destino, zero ou múltiplos candidatos devolvem `(None,None)`. O dedup seguinte usa **`data_evento=_data_ev`**, antes de escolher o fallback. Assim, uma pergunta legada da mesma disputa/motivo com data nula pode impedir a emissão de um dia cujo fallback é válido. Ocorrência nos 64: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:238–244`; `app/chamados/services/disputa_emissao.py:410–424`) |
| Reuso na porta final | `PerguntaDisputa.abrir` pode encontrar uma canônica do fato, inclusive em outra disputa, e devolver a existente com `criada=False`. O gerador acrescenta essa pergunta à lista `criadas` mesmo sem inserção; a fábrica devolve motivos, não essa lista. (`app/chamados/models.py:2776–2781`; `app/chamados/models.py:2905–2922`; `app/chamados/models.py:3008`; `app/chamados/services/disputa_emissao.py:444–471`; `app/chamados/services/fabrica_da_celula.py:219–220`) |

Não achei filtro geral de período de apuração em `motivos_a_perguntar` ou `gerar_perguntas_disputa`; procurei os corpos completos dessas funções. Os recortes encontrados são dia/teto, parcial, identidade/dedup e encaminhamento específico do limbo entregue; no reuso há também a porta de rechaveamento. (`app/chamados/services/fabrica_da_celula.py:92–157`; `app/chamados/services/fabrica_da_celula.py:214–231`; `app/chamados/services/disputa_emissao.py:300–471`; `app/chamados/models.py:2940–2956`)

Recusa de destino cruzado, fato em outro dia, batida retratada, rechaveamento recusado ou contrato de saída **levanta exceção**; quando escapa, produz `recusa:<tipo>`, não `segue_sem_porta`. (`app/chamados/models.py:2818–2822`; `app/chamados/models.py:2875–2890`; `app/chamados/models.py:2909–2926`; `app/chamados/models.py:2952–2956`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:236–239`)

As exceções engolidas de reatar órfã e de registrar nascimento ocorrem **depois** das inserções. A primeira pode deixar a pergunta órfã, mas não é um ramo de ausência de criação. (`app/chamados/services/disputa_emissao.py:459–470`; `app/chamados/services/disputa_emissao.py:587–603`; `app/chamados/models.py:3014–3031`)

**P3. Escritores, leitores e significado dos insumos.**

A tabela identifica as portas escritoras e os leitores pertinentes aos caminhos acima; não atribui a elas a autoria dos valores concretos da medição, que é **[nao verificado]**.

| Campo/família | Quem escreve ou produz | Quem lê / significado |
|---|---|---|
| `CelulaDia.data`, `trabalha`, DNA/geradora | Geração/regeneração e edição pelas portas da célula. (`app/ponto/portas/celula.py:243–272`; `app/ponto/portas/celula.py:534`; `app/ponto/portas/celula.py:603–604`; `app/ponto/portas/celula.py:816–875`) | Retenção lê por data; tarefa e fábrica usam o vínculo/marcos do dia. `trabalha=None` não equivale a folga no juiz de cobrança. (`app/folha/export.py:1206`; `app/ponto/turnos.py:3049–3062`; `app/ponto/services/cartorio.py:840–859`; `app/chamados/juizes.py:426–441`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `lavrar_veredito`; a ata é produzida por `ata_do_dia`. (`app/ponto/portas/celula.py:974–1040`; `app/ponto/services/cartorio.py:696–738`) | Retenção lê acusação/protestos; C6 lê veredito/via; tarefa e fábrica leem a ata por finalidades distintas. (`app/folha/export.py:1233–1256`; `app/chamados/juizes.py:1338–1387`; `app/chamados/services/tarefa_do_dia.py:84–102`; `app/chamados/services/fabrica_da_celula.py:120–139`) |
| `ata.lampadas`: tipo/hora/acesa/luz | `ata_do_dia` chama o produtor das lâmpadas e as inclui na ata. (`app/ponto/services/cartorio.py:701–713`) | O leitor produz `missing` e `acesa_lavrada`; fábrica usa o primeiro, tarefa usa o segundo. `acesa=None` vira `missing=Fals

[truncado em 20000 de 33148 caracteres]

## 10/11 23:10 · sessao f3099e60799a · - · - · origem=Ronald (tmux)

Matriz complementar P3, somente leitura; escritores abaixo são os encontrados nos caminhos examinados, sem afirmar censo exaustivo de todos os chamadores da árvore. [não verificado]

| Campo/função | Quem escreve/produz | Quem lê | Significado e diferenças |
|---|---|---|---|
| `Ausencia.tipo/data/data_fim/status` | Nascimento em `criar_ausencia`: deriva fim e monta `kwargs`, incluindo `status` explícito ou inicial; instancia e salva a ausência (`app/ponto/services/ausencia.py:197`, `:331`, `:341`, `:385`, `:418`). Edição de tipo/range em `editar_ausencia_registro` (`:465`, `:477`, `:498`, `:509`). Transição de status em `mudar_estado` (`:808`, `:824`, `:835`). | Carga da tarefa chama `_pelo_efeito` + `_q_cobertura` e `_cobertura_pura` (`app/ponto/turnos.py:3064`, `:3072`); tarefa examina status (`app/chamados/services/tarefa_do_dia.py:258`). Vigia consulta cobertura na cadeia de dono (`app/chamados/services/vigia_fio.py:274` e bloco correspondente abaixo, já no agente P1). | Para cobrança, exclui apenas `rejeitada`; para folha, exige `aprovada` (`app/ponto/turnos.py:2511`). Cobertura por data é inclusiva; sem fim, estende só tipos abertos; empate vence maior `(data,pk)` pela ordem e sobrescrita (`:2753`, `:2757`, `:2765`, `:2780`). `aguardando_documento` é bola colaborador; `aguardando_decisao` é DP (`app/ponto/services/ausencia.py:779`, `:791`, `:795`); `ABERTAS` contém esses dois, sem `pendente` (`:765`). |
| `PeriodoFechado` / tranca | `trancar_periodo` cria/retranca e limpa reabertura (`app/ponto/services/fechamento.py:1400`, `:1408`, `:1412`). `reabrir_periodo` grava `ativo=False` e `reaberto_em` (`:1432`, `:1437`). | `periodo_trancado` procura `reaberto_em IS NULL` (`:1464`, `:1470`); `dias_trancados` mapeia dia do turno à competência com empresa e chama esse juiz (`app/chamados/juizes.py:966`, `:1024`, `:1028`); tarefa carrega resultado em lote (`app/chamados/services/tarefa_do_dia.py:365`). | `ativo` é documento; a leitura da tranca usa `reaberto_em` (`app/ponto/services/fechamento.py:1464`). Tranca e exportação são perguntas distintas; a própria descrição separa aprovada/exportada/trancada/paga (`:1455`). |
| `ExportacaoDominio` / entregue | `ExportacaoDominio.registrar` cria histórico por geração (`app/folha/models.py:174`, `:182`). `invalidar` carimba invalidada/substituta (`:143`, `:158`, `:162`). | `competencia_entregue` usa vigente por empresa/mês/ano e recusa vigente invalidada (`app/folha/export.py:556`, `:580`). Composição dia→competência chama `janela_atual(dia, empresa)` (`app/chamados/services/vigia_fio.py:822`, `:838`). Backfill lê antes e novamente sob trava (`app/chamados/management/commands/backfill_fio_morto_e4b.py:194`, `:227`); fábrica lê para limbo (`app/chamados/services/fabrica_da_celula.py:223`, `:230`). | Competência é do dia, não mês civil, e usa corte da empresa (`app/chamados/services/vigia_fio.py:838`; `app/ponto/janelas.py:60`). |
| Lista CxR / `dono_do_dia` / presença colab e dia | `cadastro_realidade.lista` monta dicionários de fontes atuais e lavras, unifica donos em memória e retorna `postos` (`app/escala/services/cadastro_realidade.py:787`, `:800`, `:885`, `:886`, `:926`, `:970`). `um_dono_por_dia` reconsulta o juiz quando assinaturas divergem e atribui `a['dono_do_dia']` em memória (`:985`, `:1017`, `:1024`, `:1028`). Não achei persistência da **lista montada** nesses corpos; suas fontes incluem lavras persistidas (`:793`, `:886`). | `dias_na_lista` extrai somente assinaturas com `data`, resolvendo dono (`:1044`, `:1058`). `ListaDoAdmin` lê lista, guarda mapa de dias e conjunto de colaboradores (`app/chamados/services/vigia_fio.py:205`, `:216`, `:218`); `dia_na_lista` e `colab_na_lista` são leituras separadas (`:230`, `:248`). | Colaborador constar na lista não implica que o dia conste: assinatura de vínculo inteiro não nomeia dia (`app/escala/services/cadastro_realidade.py:1044`). Classe `d` usa precisamente ausência do dia mais presença do colab (`app/chamados/services/vigia_fio.py:677`). |
| Cadastro: `situacao`, `data_demissao`, `data_admissao`, `isento_ponto`, empresa | Porta central declara operações e salva: `gravar`, `salvar_inteiro`, `criar_colaborador` (`app/colaboradores/services/colaborador.py:23`, `:55`, `:73`, `:91`). Desligamento/reativação atribuem situação/data_demissão e salvam pela porta (`app/colaboradores/services/desligamento.py:103`, `:210`). Tela atribui admissão e empresa (`app/colaboradores/views.py:1313`); nascimento recebe admissão e ativo (`:1188`, `:1205`); Passe Livre grava flag pela porta (`:176`). | Leitor da célula lê admissão/demissão e flag (`app/escala/services/leitor_celula.py:358`, `:365`); tarefa usa esse leitor (`app/chamados/services/tarefa_do_dia.py:380`); fábrica exige situação atual ativo (`app/chamados/services/fabrica_da_celula.py:112`). | Leitor considera fora antes da admissão ou após demissão; no dia da demissão, depende de fato lavrado (`app/escala/services/leitor_celula.py:385`, `:389`). Fábrica usa status cadastral atual, sem reconstruir situação no dia (`app/chamados/services/fabrica_da_celula.py:112`). Portanto os dois não aplicam o mesmo predicado de elegibilidade (`app/escala/services/leitor_celula.py:389`; `app/chamados/services/fabrica_da_celula.py:112`). |

Pontos temporais que merecem aparecer na consolidação:

- O backfill separa `agora=timezone.localtime()` de `hoje=--hoje` opcional: vigia recebe `hoje`, tarefa recebe `agora`, e fábrica recebe ambos. Portanto `--hoje` não rebobina o relógio da tarefa/teto; se isso foi usado na medição, [não verificado]. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:169`, `:176`, `:193`, `:117`.)
- Vigia `censo_alcance` chama `alimentar_portas` sem `agora`; alimentação chama C6 sem `hoje`; `ListaDoAdmin` criada dentro desse censo também não recebe `agora`. A tarefa injeta `agora` na fila e corta decisões por ele. (`app/chamados/services/vigia_fio.py:604`, `:623`; `app/chamados/juizes.py:1497`, `:1504`; `app/chamados/services/tarefa_do_dia.py:337`, `:363`.)
- Janela da competência é fechada nos dois extremos: corte do mês anterior até corte−1 do mês nomeado (`app/ponto/janelas.py:10`); tarefa exige `ini <= dia <= fim` (`app/chamados/services/tarefa_do_dia.py:398`); retenção folha usa `data__range` e laço `<= fim` (`app/folha/export.py:1206`, `:1225`). Não achei borda exclusiva nesses três caminhos examinados; teto de marco é inclusivo em `marco + 30min <= agora` (`app/ponto/services/cartorio.py:828`, `:858`).
- C6 fallback `_dentro_da_janela_viva` chama janelas **sem empresa**, portanto usa corte default 21; a expiração da lavra faz o mesmo. Já entregue e tranca usam empresa. Isso é diferença verificável de parâmetros, sem demonstrar diferença efetiva para as empresas da medição. (`app/chamados/juizes.py:578`, `:597`; `app/chamados/veredito_lavrado.py:143`; `app/ponto/janelas.py:36`, `:50`; `app/chamados/services/vigia_fio.py:838`; `app/chamados/juizes.py:1024`.)
- C6 fallback sem `hoje` usa `datetime.date.today()`; bordas e expiração usam data local Django. Settings-base define São Paulo e `USE_TZ=True`; fuso efetivo do processo/prod e coincidência de datas nesta rodada são [não verificado]. (`app/chamados/juizes.py:577`; `app/chamados/veredito_lavrado.py:95`, `:104`; `app/config/settings/base.py:93`.)
- `CargaDaTarefa` declara mistura explícita de horizonte: decisões/lavra/efeito cortados por `agora`, mas identidade/pergunta/ata/cobertura/status/fila/cadastro/vínculo/tranca atuais. Logo não é fotografia histórica integral. (`app/chamados/services/tarefa_do_dia.py:301`.)

## 10/11 23:09 · sessao 60d5ae8dd978 · - · - · origem=Ronald (tmux)

P1 — rastreamento do código, sem inferir a distribuição interna dos 116 dias:

- O command usa **`tarefa_do_dia`**, chamada por `ato_do_dia`; o rótulo `tarefa_nada` é construído pelo próprio command quando não acumulou atos de sistema/passivo, não acumulou ações com `porta is None` e `t['nao_sei']` é falso. Portanto o rótulo, isoladamente, não prova que `acoes == []`: ações humanas **com porta preenchida** também não entram em `humanos`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:73–98`)
- A tarefa efetivamente julgada é `_tarefa_pura`, chamada por `CargaDaTarefa.tarefa`, com cadastro, grade, instâncias de marco, cobertura, perguntas, decisões, efeitos, fila, passivo, lavra e tranca. (`app/chamados/services/tarefa_do_dia.py:396–406`)
- O universo das perguntas da tarefa é `PerguntaDisputa(colaborador_id__in=ids, conversa=False, data_evento__gte=ini, data_evento__lte=fim)`, ordenado por colaborador e pk; não exige pergunta viva, disputa aberta ou carimbo não validado. (`app/chamados/services/tarefa_do_dia.py:323–329`)
- Dentro desse universo, só considera motivos de marco; exclui `VIAS_FORA_DO_FATO`, salvo retratação posterior ao `agora`, e exclui última decisão de retratação; associa pelo `data_evento == dia` e pela instância/posição do motivo. (`app/chamados/services/tarefa_do_dia.py:223–234`)
- A decisão considerada é a última linha em ordem `(ato_em, pk)` com `ato_em <= agora`; os atos posteriores só alimentam o conjunto de retratações posteriores. (`app/chamados/services/tarefa_do_dia.py:337–342`)

Predicado da tarefa:

- Retorna nada para `fora_vinculo`, isento ou competência trancada; sem célula retorna `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:180–201`)
- Em dia de trabalho sem instâncias de lâmpada, retorna nada se existem marcos e nenhum venceu; caso contrário retorna `nao_sei/ata_sem_lampadas`. (`app/chamados/services/tarefa_do_dia.py:202–210`)
- Por instância, não exige ação se `dia_sem_trabalho`, lâmpada `True`, marco ainda não vencido ou ausência aprovada; lâmpada `None` ou marco sem teto produzem `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:241–260`)
- Para marco apagado, vencido e não coberto, sem pergunta da identidade exige `PERGUNTAR`; havendo pergunta, julga a última decisão. (`app/chamados/services/tarefa_do_dia.py:268–288`)
- Última validação com lavra vigente `sim` e sem efeito de Batida produz `encerrada_pelo_juiz/[]`; silêncio produz `silencio_decidido/[]`; absorção com ausência aprovada produz `absorvida_aprovada/[]`. (`app/chamados/services/tarefa_do_dia.py:140–154`)
- Última resposta, recusa, reabertura, devolução ou absorção não final gera ação humana; validação sem efeito gera materialização, e efeito sem lâmpada gera rejulgamento. (`app/chamados/services/tarefa_do_dia.py:129–159`)
- `dia_sem_trabalho` lê `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de trabalho; ausência parcial com resto cobravel só é calada por `trabalha is False`. (`app/chamados/juizes.py:426–441`)

Predicado do vigia usado pelo backfill:

- O command usa **`censo_alcance`**, selecionando `alcance_dias` cuja rota é `sem_porta`; não usa o `censo` legado para definir seu plano. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–70`)
- O universo são os dias retornados por `dias_que_seguram` para cada empresa visível e cada competência de `competencias_a_lavrar`, deduplicados por `(colab_id, data)`. (`app/chamados/services/vigia_fio.py:581–588`)
- Empresas visíveis são ativas, excluindo CNPJ iniciado por `11222333`. (`app/colaboradores/queries.py:78–87`)
- Competências são a atual e a anterior se esta não estiver entregue; a atual entra sem teste de entregue nesse enumerador. (`app/ponto/services/he_pendente_lavrado.py:218–225`)
- `dias_que_seguram` parte de `classificar_export`, retém somente colaboradores com motivo `furo_espelho`, depois obtém seus dias pela mesma `motivos_retencao_celula`. (`app/folha/fora_da_folha.py:118–137`; `app/folha/fora_da_folha.py:95–99`)
- `classificar_export` lê `FechamentoMensal` da empresa/mês/ano sem filtro de status; sua cadeia pode classificar antes por rescisão, cadastro etc., e só atribui `furo_espelho` quando chega ao teste dos motivos de retenção. (`app/folha/export.py:757–780`; `app/folha/export.py:960–965`)
- Retenção usa `CelulaDia` por `data__range=(ini,fim)`, recorta admissão/demissão e exclui isento; retém célula acusadora salvo todos os protestos não cobraveis ou exceção da pausa; também retém cegueira passada, sob as condições do código. (`app/folha/export.py:1192–1206`; `app/folha/export.py:1224–1257`)
- Acusadores são `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`; o conjunto não cobravel contém `FURO_PRE_ADOCAO`. (`app/escala/models.py:1767–1768`; `app/ponto/supra_juiz.py:34–36`)
- O vigia pergunta ao C17 `porta_alcancavel_do_dia`; havendo porta, sai do morto; dia de hoje/futuro também sai; depois saem os quatro baldes de `dono_fora_do_fio`. O restante incrementa `fio_morto_alcance` e recebe `sem_porta`. (`app/chamados/services/vigia_fio.py:635–660`; `app/chamados/services/vigia_fio.py:695–699`)
- Os quatro baldes são Resolver dia oferecido; ausência cobrindo com status em `ABERTAS`; aceite de folga validado; dia na lista CxR com dono BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:297–315`)
- C17 considera porta um chamado da célula na fila ou pergunta viva da célula, no app, com âncora na fila e sem resposta vazia ilegal. (`app/chamados/juizes.py:1574–1589`)
- A fila exige status em `COBRANCA` e exclui silêncio no futuro; `COBRANCA=('aberto','em_analise')`, enquanto `VIVOS` também inclui `registrado`. (`app/chamados/services/painel.py:188–190`; `app/chamados/catalogo/motor.py:24–31`)
- As perguntas do vigia são carregadas **por `celula_id`**, sem `conversa=False` nem filtro de `data_evento`; a seleção de vivas usa C6. (`app/chamados/juizes.py:1502–1505`)
- C6 aplica estado da projeção/carimbos, tranca, vias, resposta, veredito da célula, lâmpada do marco, classificador e materialização; ele não usa a sequência de decisões da tarefa para responder. (`app/chamados/juizes.py:1324–1410`)

Condição exata de divergência:

`morto = dia retido ∧ sem C17 ∧ data < hoje ∧ nenhum balde de dono`; simultaneamente `tarefa_nada = sem ato de sistema/passivo ∧ sem ação com porta None ∧ nao_sei=False`. O primeiro testa retenção agregada e alcançabilidade por célula; o segundo testa exigibilidade por instância/decisão e depois passa pela redução do command. Não existe equivalência imposta entre os predicados. (`app/chamados/services/vigia_fio.py:635–699`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78–98`; `app/chamados/services/tarefa_do_dia.py:235–288`)

Para a **classe d**, além do predicado de morto: existe célula; não existe `veu` de chamado vivo da célula ou âncora viva fora da fila; `ps` vivas está vazio; `dia_na_lista == (False,None)`; o colaborador aparece na lista CxR. Isso não identifica a razão da tarefa dos 116 dias; não achei essa distribuição no dado fornecido. (`app/chamados/services/vigia_fio.py:661–678`; [não verificado quanto à distribuição dos 116])

Juízes:

- C19 está registrado para a pergunta das ações exigíveis e aponta a `tarefa_do_dia`; o command o lê. (`app/core/juizes.py:150–155`; `app/core/juizes.py:291`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:75–76`)
- O vigia lê C17, C6 e os baldes de dono; não chama C19 em `censo_alcance`. (`app/chamados/services/vigia_fio.py:555–564`; `app/chamados/services/vigia_fio.py:604–610`; `app/chamados/services/vigia_fio.py:635–656`)
- O registro declara que os cinco respondedores anteriores seguem vivos na E2; não prova que todos os leitores do HEAD já migraram. (`app/core/juizes.py:153–154`)
- C17, C6, C15 e C18 também estão registrados. (`app/core/juizes.py:278–291`)

Nuances de significado/relógio para P3:

- A tarefa usa dia do turno em `data_evento`; calcula `data_civil` separadamente a partir do instante local do marco. (`app/chamados/services/tarefa_do_dia.py:213–215`; `app/chamados/services/tarefa_do_dia.py:91–102`)
- A tarefa lê perguntas por colaborador/data do turno; o vigia lê perguntas ligadas à célula. Uma pergunta com chave ou FK divergente pode entrar num universo e não no outro; ocorrência nos dados fornecidos é [não verificado]. (`app/chamados/services/tarefa_do_dia.py:324–329`; `app/chamados/juizes.py:1502–1505`)
- O backfill captura `agora=timezone.localtime()` uma vez; `hoje` pode ser sobrescrito independentemente por `--hoje`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:169–170`)
- Esse `agora` chega à tarefa e à fábrica, mas `_alcance` recebe apenas `hoje`; `censo_alcance` chama `alimentar_portas` sem `agora`, C6 sem `hoje`, e cria `ListaDoAdmin` sem `agora`. Logo o vigia não está inteiramente congelado no mesmo instante da tarefa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:67–69`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:117`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:193`; `app/chamados/services/vigia_fio.py:604`; `app/chamados/services/vigia_fio.py:623`; `app/chamados/juizes.py:1504`)
- Sem `agora`, a fila usa `timezone.now()`; a retenção chamada pelo vigia também não recebe `hoje`, usando `timezone.localdate()` internamente. Isso permite diferença temporal sem demonstrá-la nos 116/64 casos. (`app/chamados/services/painel.py:188–190`; `app/folha/fora_da_folha.py:95–99`; `app/folha/export.py:1197`)
- Os intervalos de dias da tarefa e da retenção são inclusivos; os marcos podem cair em outro dia civil. (`app/chamados/services/tarefa_do_dia.py:325–326`; `app/chamados/services/tarefa_do_dia.py:398–399`; `app/folha/export.py:1206`; `app/folha/export.py:1225`; `app/chamados/services/tarefa_do_dia.py:102`)
- O leitor da grade preserva a lâmpada triestado `acesa_lavrada`, enquanto seu campo antigo `missing` trata `acesa=None` como acesa; a tarefa escolhe explicitamente o triestado. (`app/escala/services/leitor_celula.py:119–137`; `app/chamados/services/tarefa_do_dia.py:84–88`)
- O leitor pode declarar `fora_vinculo` no dia da demissão sem fato lavrado, enquanto a retenção ainda retém célula acusadora nesse dia; este é um caminho de divergência permitido pelo código, participação nos 116 dias [não verificado]. (`app/escala/services/leitor_celula.py:385–393`; `app/folha/export.py:1229–1254`)

## 10/11 23:08 · sessao c225cf111a84 · - · - · origem=Ronald (tmux)

P2 — A saída `segue_sem_porta` não demonstra ausência de criação: ela só demonstra que o recenso ainda encontrou a célula na rota `sem_porta`; o command não inspeciona o retorno da fábrica nem conta perguntas novas (`app/chamados/management/commands/backfill_fio_morto_e4b.py:116-117`, `:240`, `:246-249`). Qual ramo ocorreu em cada um dos 64 dias: [não verificado]; o dado fornecido não contém os insumos dessas guardas.

O caminho efetivo é `perguntar_pela_celula` → `motivos_a_perguntar` → `motivos_pela_ata`; com motivos, `perguntar` → `criar_ou_atualizar_disputa` → `gerar_perguntas_disputa`, e `perguntar` chama o gerador novamente quando recebe uma disputa (`app/chamados/services/fabrica_da_celula.py:178`, `:207`, `:217-220`, `:160-175`; `app/chamados/services/disputa_emissao.py:669-688`).

Ramos que terminam sem criar `PerguntaDisputa`:

| Condição | Resultado / referências | Command distingue? |
|---|---|---|
| `cel.veredito == 'nunca_bateu'` | Retorna `[]` imediatamente; nem pauta cadastro (`app/chamados/services/fabrica_da_celula.py:87-89`, `:183-184`). `nunca_bateu` consta em `VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`). | Não: retorno descartado (`backfill_fio_morto_e4b.py:116-117`). |
| Passado, colaborador ativo, `not dia_sem_trabalho(cel)`, e teto não encontra marcos vencidos **nem pendentes** | `vencidos_no_instante` devolve `None`; escreve/tenta deduplicar pauta cadastro e retorna `[]` (`fabrica_da_celula.py:45-49`, `:201-205`). Marcos vazios podem vir de vínculo/tipo ausente ou `marcos_turno` vazio (`app/ponto/services/cartorio.py:840-845`, `:855-859`). | Não; o booleano da pauta também é ignorado (`fabrica_da_celula.py:204-205`; command `:117`). |
| Pauta cadastro já existe | O escritor consulta **depois da trava**; retorna `False` sem escrever (`fabrica_da_celula.py:364-368`). O juiz aceita cabeça viva por colaborador ou fechada em data **civil local >= dia** (`:327-335`); não deduplica por dia (`:313-314`, `:327-335`). A fábrica retorna `[]` em ambos os casos (`:204-205`). | Não (`backfill...:117`). |
| Dia sem trabalho, colaborador não ativo, dia futuro | `motivos_a_perguntar` retorna `[]` (`fabrica_da_celula.py:110-115`). | Não (`backfill...:117`). Dia futuro é incompatível com `sem_porta` no mesmo `hoje`, pois o alcance o retira antes (`vigia_fio.py:648-651`). |
| Veredito preenchido fora de `CelulaDia.VEREDITOS_ACUSAM` | Retorna `[]`; veredito vazio **não** cai nessa guarda (`fabrica_da_celula.py:120-121`). | Não (`backfill...:117`). |
| Ausência parcial mandada somente à lista | `parcial_so_lista` retorna verdadeiro para `cobertura_parcial` e `periodo_parcial == 'sem_periodo'`, ou parcial com julgamento anterior e sem fato novo desde ele; a fábrica usa `cel.ata`, `cel.julgada_em` por default e `agora` (`app/ponto/services/cartorio.py:395-400`; `fabrica_da_celula.py:123-125`). O command não passa `julgada_antes`, portanto vale esse default (`backfill...:117`; `fabrica...:178`, `:123-124`). | Não (`backfill...:117`). |
| Nenhum marco vencido | `motivos_pela_ata` recebe `None` ou conjunto vazio e retorna `[]`; no passado sem turno o ramo anterior já direcionou a cadastro (`fabrica_da_celula.py:58-61`, `:201-205`). Há turno ainda pendente ⇒ conjunto vencido vazio, sem cadastro (`:46-49`). | Não (`backfill...:117`). |
| Leitura da ata falha ou ata não autoriza | `_ata_do_dia` engole exceção, registra e retorna `(True,None)`; embora o comentário diga “falha de LEITURA nao barra”, este chamador **barra** quando `faltantes is None` (`app/chamados/services/disputa_emissao.py:894-901`; `fabrica_da_celula.py:64-66`). Estado `sem_celula` com escala vigente ⇒ `(False,None)`; sem escala ⇒ `(True,None)`; ambos não geram motivos neste caminho (`disputa_emissao.py:902-913`; `fabrica...:65-66`). | Não: exceção engolida vira `[]` (`backfill...:117`). |
| Ata lida, mas nenhum faltante elegível | `_marcos_faltantes` ignora isento/fora vínculo, dia diferente de trabalho sem parcial cobrando resto, marcos sem tipo E/S ou hora, célula não `missing`; sem pares registra `sem_celula` (`app/chamados/services/declaracao_texto.py:101-143`). A fábrica ainda exige `(tipo,hora)` no teto vencido, `f[0] == cel.data`, mapeamento `(posição,tipo)` conhecido e motivo único (`fabrica_da_celula.py:68-74`). A contribuição da véspera de `_marcos_faltantes` não passa por `f[0] == dia` aqui (`declaracao_texto.py:135-142`; `fabrica...:71`). | Não (`backfill...:117`). |
| Todos os motivos já têm linha do fato | Consulta por `disputa__colaborador=colab`, `data_evento=cel.data`, `conversa=False`, exclui duplicata; anota `q_dos_fatos`; remove todo motivo em `ja` (`fabrica_da_celula.py:145-157`). `q_dos_fatos` exige coluna `colaborador_id`, motivo, dia, instância derivada do motivo, `conversa=False`, via fora de `('duplicata','motivo_retratado')` (`app/chamados/models.py:2746-2755`, `:2393`). **Não exige pergunta viva, disputa aberta ou não validada/materializada** (`models.py:2753-2755`). | Não; retorna `[]`, embora possa desfazer carimbo de candidatas existentes (`fabrica...:208-209`, `:220`; `backfill...:117`). |
| Limbo (`trabalha is None`), motivos e competência entregue | Encaminha pauta DP e retorna `[]` (`fabrica_da_celula.py:214-216`, `:228-231`). Pauta deduplica por cabeça ancorada em colab:dia, DP e prefixo, incluindo feita/cancelada (`:257-265`, `:276-285`). | Não (`backfill...:117`). No backfill, entregue/indeterminada já é barrado duas vezes e vira `bang:*`, portanto este ramo não explica por si um `segue_sem_porta` num estado estável (`backfill...:194-197`, `:227-231`). |

O alcance que produz **classe d** não exige “chamado vivo com célula”: a rota vem dos dias que seguram a folha; `d` requer nenhum `ps` vivo, dia fora da lista, colaborador na lista, depois de excluir porta, hoje/futuro e donos por lei, e depois de `veu` vazio (`app/chamados/services/vigia_fio.py:577-587`, `:635-678`). Portanto guardas da fábrica relativas a falta de chamado não contradizem automaticamente a classe d (`vigia_fio.py:663-678`).

Ramos no gerador:

- Ausência de pergunta no catálogo → `continue` (`app/chamados/services/disputa_emissao.py:314-316`). Não corresponde aos quatro motivos normais desta fábrica: o mapa produz somente entrada/saída/intervalos, todos presentes e de tipo `hora` no catálogo (`app/chamados/services/declaracao_texto.py:42-47`; `app/chamados/catalogo/perguntas.py:48-58`, `:124-134`).
- Dedup não datado sem episódio; texto sem data e fallback de grade sem dia; datado de intervalo sem data ou com linha existente; pergunta de hora sem dia; conversa repetida: todos são `continue` explícitos (`disputa_emissao.py:325-329`, `:346-366`, `:398-401`, `:425-438`). Os ramos de não datado/conversa não são o caminho normal dos quatro motivos da fábrica, que entrega contexto `{motivo: {'data': dia.isoformat()}}` e catálogo de hora (`fabrica_da_celula.py:172`; `disputa_emissao.py:26`; `models.py:3657`; `catalogo/perguntas.py:48-58`, `:124-134`).
- **Dedup relevante que não usa a mesma chave do primeiro filtro:** para `saida_sem_entrada`/`orfao_14h`, `_derivar_chamado_destino` com 0 ou 2+ candidatos devolve `(None,None)`; depois o gerador deduplica por `disputa`, `motivo`, **`data_evento=_data_ev`**, `conversa=False`, excluindo vias fora do fato (`disputa_emissao.py:238-244`, `:410-420`). Assim uma linha legada com `data_evento=None` pode impedir a criação mesmo que `_data_fallback` seja o dia válido da célula; sem essa linha, `_dia_p` usa o fallback (`:417-424`). Ocorrência nos 64: [não verificado].
- Intervalos deduplicam por disputa+motivo+dia fallback; a porta final `abrir` também pode reutilizar uma canônica global em outra disputa/estado em vez de criar; o retorno é `(ex,False,...)`, sem exigir vitalidade ou disputa aberta nesse ponto (`disputa_emissao.py:395-401`; `app/chamados/models.py:2905-2922`, `:3008`). O gerador ainda acrescenta a reutilizada a `criadas` sem testar `_criada`, então sua lista tampouco é contagem estrita de inserções (`disputa_emissao.py:444`, `:459`, `:470-471`). A fábrica devolve `ms`, não a lista do gerador (`fabrica_da_celula.py:219-220`).
- Não achei filtro geral de competência/período de apuração em `motivos_a_perguntar` ou `gerar_perguntas_disputa`; procurei o corpo completo das duas funções. Há somente o encaminhamento específico de limbo entregue na fábrica, as guardas de dia/teto/parcial e as recusas de rechaveamento no reuso (`fabrica_da_celula.py:92-157`, `:214-231`; `disputa_emissao.py:300-471`; `models.py:2940-2956`).
- A recusa de rechavear, fato em outro dia, destino cruzado, batida retratada ou contrato de saída levanta exceção, não retorna silenciosamente; o command faz rollback do dia e escreve `recusa:<tipo>`, portanto não são `segue_sem_porta` quando escapam (`models.py:2875-2890`, `:2909-2926`, `:2952-2956`, `:2818-2822`; `backfill_fio_morto_e4b.py:236-239`).
- Exceção engolida de reatar órfã ocorre **depois** da pergunta criada e a mantém órfã; não é ramo “sem criar”, embora possa deixar a porta inalcançável (`disputa_emissao.py:459-470`). Logs de nascimento de disputa/pergunta também têm `except`, mas após as inserções, portanto não explicam ausência de criação (`disputa_emissao.py:587-603`; `models.py:3014-3031`).

Para P3, os pontos de significado mais úteis são: instância derivada de posição da ata (`declaracao_texto.py:42-57`, `:75-83`); o leitor de ata converte `acesa=None` em `missing=False`, mas preserva `acesa_lavrada=None` para tarefa (`app/escala/services/leitor_celula.py:119-137`); fábrica exige `missing`, tarefa pode ler o triestado. Teto usa instante `quando + 30min <= agora`, com marco ancorado no dia do turno e horário formatado no fuso local (`app/ponto/services/cartorio.py:828`, `:831-859`). Pergunta datada usa dia do turno; conversa grava `localdate()` civil e não participa do fato (`app/chamados/models.py:2776-2803`). A pauta de cadastro fechada usa data civil local inclusiva (`fabrica_da_celula.py:331-335`).

## 10/11 22:53 · sessao 0902fbce9ab3 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Aplicadas as correções nos três testes: imports/variável mortos removidos e capturas amarradas por defaults. Arquivos tocados em [entrega:5](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:5). [nao verificado]

Todos os B023 têm chamada dentro da mesma iteração:

| Fecho | Variáveis |
|---|---|
| 5c:126 | `filtro`, `a` |
| 5f:87 | `filtro`, `a` |
| 5f:147 | `filtro`, `a` |
| 5f:149 | `ps`, `dona`, `outro_dia`, `criada`, `outra` |

Prova por ocorrência e rastreamento em [entrega:11](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:11).

Os 14 `--mutante` completos pararam no controle: 5c com 3 erros; 5f com 1 falha e 3 erros, iguais à base. Cinco mutantes de 5f deram controle verde + RED limpo usando `--item`. Comandos e saídas completos em [entrega:166](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:166) e [entrega:1316](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:1316). [nao verificado]

**Ruff zero e GREEN+RED integral: [não verificado].** Ruff está ausente; os controles encontram um módulo double ausente. Limites registrados em [entrega:33](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5i_entrega.txt:33).

## 10/11 22:42 · sessao 4744e2951238 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura aplicada: contador novo `I23`, corte histórico do `I8` restaurado, chave HOJE atualizada e cinco achados de lint tratados. Os nove arquivos tocados estão na [lista da entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:5). [nao verificado]

RED do ID único observado na base; controle verde e quatro mutantes com uma assertion RED cada. [Resultados e comandos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:29). [nao verificado]

**Ruff zero [nao verificado].** `ruff check --no-cache app/` retornou `ruff: command not found`, rc 127. Django também está ausente; ORM e dois selos [nao verificado]. [Registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:63).

Comandos para a sessão, dentro de `app/`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5h_forma.py --red-base
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5h_forma.py --mutante id_colidido

PYTHONDONTWRITEBYTECODE=1 RUFF_NO_CACHE=1 python3 -B manage.py test core.tests.test_contract_esmeril escala.tests.test_semselo5_ativo_nao_responde_por_dia --settings=config.settings_test --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_bo_disputa_volta5g --settings=config.settings_test --noinput
```

A memória 5g passou nos dez casos; 5b/5c apresentaram os mesmos erros de importação reproduzidos na base. [Detalhes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5h_entrega.txt:51). [nao verificado]

## 10/11 22:34 · sessao e0e7d447ad36 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura implementada nos seis arquivos listados na [entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:5): emissor, serviço de trilha, vigia, testes em memória, testes ORM e registro da entrega. [nao verificado]

A [escrita única via `core.observ.evento`](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/services/descarte_disputa.py:31) ocorre após commit, com deduplicação por **dona + motivo + pergunta_id**. Os sítios de encaminhamento e a sobrevivência aos rollbacks estão [documentados por caminho](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:13). [nao verificado]

Resultados observados e comandos de cada RED/mutante:

- **10 testes em memória passando; três REDs contra a base; oito mutantes mortos por assertions**, sem aceitar erro de montagem. [Comandos e resultados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:66). [nao verificado]
- **I8 lê somente a trilha**; o I8 histórico permanece, com descrição distinta. [Vigia](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/management/commands/auditar_invariantes_chamados.py:77).
- **Seis casos ORM entregues, sem execução pelo Codex**. ORM, regressão completa e certificação final: **[nao verificado]**. [Comando para o Code](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:100).

[perguntas de lei: 0](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/tests/bo_disputa_volta5g_entrega.txt:109)

## 10/11 22:23 · sessao be870e2c21a5 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura implementada em [disputa_emissao.py:502](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/chamados/services/disputa_emissao.py:502): episódio explícito no replay, motivos locais sem seed estrangeiro, análise condicionada ao replay confirmado, âncora pelo agregador e declaração pública com pergunta obrigatória (`:724`, `:1279`, `:1474`, `:1577`).

Arquivos tocados, além do emissor:

- `chamados/tests/test_bo_disputa_volta5f_memoria.py:80` — REDs e mutantes novos.
- `chamados/tests/test_bo_disputa_volta5_memoria.py:68` — montagem única e execução do alvo.
- `chamados/tests/test_bo_disputa_volta3_memoria.py:234` — repetibilidade, sem alegar comparação histórica.
- `chamados/tests/test_bo_disputa_volta5b_orfaos.py:126` — RED ORM do lote misto.
- `chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:183` — expectativa sem seed indevido.
- `chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:400`
- `chamados/tests/test_bo_disputa_sem_pergunta.py:70`
- `chamados/tests/test_porta_disputa_emissao.py:86`
- `chamados/tests/test_sla_pelo_juiz.py:133`

Os oito testes novos passaram em memória; cada mutante abaixo produziu falha por assertion no alvo. Referências: `chamados/tests/test_bo_disputa_volta5f_memoria.py:80`, `:101`, `:111`, `:135`, `:165`, `:181`, `:187`, `:204`.

Comandos dos REDs com seus mutantes; cada comando exige primeiro controle verde: [nao verificado]

```bash
red5f() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5f_memoria.py "$@"
}

# F1
red5f --item F1 --mutante seed_local
red5f --item F1 --mutante ausencia_admite_motivo
# F2
red5f --item F2 --mutante episodio_da_dona
# F3
red5f --item F3 --mutante analisa_divergente
# F4
red5f --item F4 --mutante ancora_save_proprio
# F5
red5f --item F5 --mutante publica_sem_pergunta
# F6
red5f --item F6
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py --mutante nasce_sem_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py --mutante replay_insere
# F7
red5f --item F7 --mutante alega_paridade
```

F1 ORM escrito em `chamados/tests/test_bo_disputa_volta5b_orfaos.py:126`. Comando reservado ao Code; execução ORM/banco: **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_bo_disputa_volta5b_orfaos.OrfaosVolta5bTest.test_RED_F1_lote_misto_local_vazia_nao_ganha_seed_estrangeiro
```

Certificação pelo Code e pelo opus final: **[nao verificado]**.

perguntas de lei: 0

## 10/11 22:12 · sessao ef42db15fa9d · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**Não há base para liberar: permanecem duplicação de motivo no repasse, replay com insumo implícito variável e selos de mutação quebrados.** Referências: [disputa_emissao.py:1275](/home/ronald/codex_ws/e70f2e7eecfdf93c543b68a61b90f6764bafc649/app/chamados/services/disputa_emissao.py:1275), [models.py:2866](/home/ronald/codex_ws/e70f2e7eecfdf93c543b68a61b90f6764bafc649/app/chamados/models.py:2866), [test_bo_disputa_volta5_memoria.py:81](/home/ronald/codex_ws/e70f2e7eecfdf93c543b68a61b90f6764bafc649/app/chamados/tests/test_bo_disputa_volta5_memoria.py:81).

1. **Estado dos achados anteriores.**

   `opus5d.txt`, `preverif3.r`, `matriz2.r` e os três briefs não estão no material textual efetivamente recebido nem foram encontrados na cópia. Não consigo enumerar seus achados e atribuir honestamente “curado/parcial/igual” a cada um. Conteúdo e confronto individual: **[nao verificado]**.

   Quanto aos problemas identificáveis no pedido e nos patches:

   | Problema | Estado atual verificável |
   |---|---|
   | Exceção do gerador deixar nascimento incompleto | **Curado no núcleo:** criação do pai, DS e perguntas ficam sob atomic; a exceção propagada desfaz o conjunto. `app/chamados/services/disputa_emissao.py:623`, `:653`, `:670`, `:674`. |
   | Pergunta estrangeira deixar nova DS sem pergunta | **Parcial:** núcleo e declaração desfazem o ensaio; órfãos também desfazem quando só houve estrangeiras. Persistem os problemas de motivos e replay descritos abaixo. `app/chamados/services/disputa_emissao.py:681`, `:774`, `:1444`. |
   | Dois escritores de agregação | **Parcial:** o helper reúne as escritas alteradas, mas a âncora conserva união e save próprios. `app/chamados/services/disputa_emissao.py:558`, `:1531`, `:1562`. |
   | Luz HX sem leitor | **Curado no instrumento:** I7a conta o defeito estrutural; I7b publica a abstinência por falta de ata como INFO. Medição atual de produção: **[nao verificado]**. `app/chamados/management/commands/auditar_invariantes_chamados.py:57`, `:73`, `:82`. |
   | Dona fechada | **Pergunta de lei aberta no reuso genérico:** o agregador não a salva; a ressuscitação pelo cartório continua podendo reabri-la pela lei existente. `app/chamados/services/disputa_emissao.py:563`; `app/chamados/models.py:1574`. |

2. **Retornos, dona dos motivos e nascimento sem pergunta.**

   Há seis chamadas datadas a `PerguntaDisputa.abrir`. Cinco consomem pergunta e indicador de criação; a primeira tentativa da declaração descarta `criada` em `_`, embora confira a dona da pergunta. Portanto, literalmente, **não são todos os retornos `(p, criada)` integralmente lidos**. `app/chamados/services/disputa_emissao.py:502`, `:710`, `:773`, `:781`, `:990`, `:1453`.

   No núcleo, os motivos admitidos em A vêm das perguntas locais; os estrangeiros são encaminhados a B. Se A já possui pergunta local do mesmo motivo em outro dia, o motivo pode legitimamente aparecer nas duas listas: o próprio teste de ciclo exige esse estado. `app/chamados/services/disputa_emissao.py:598`, `:638`, `:648`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:36`.

   **Existe também duplicação indevida no repasse:** A aberta com `motivos=[]` recebe o seed `orfao_14h`. Num lote com intervalo local e pergunta `orfao_14h` estrangeira em B aberta, o helper retorna `motivos_antes`, que já contém o seed. A conserva `orfao_14h` sem possuir essa pergunta, B recebe o motivo, e a pergunta local de intervalo impede o rollback. Isso independe da pergunta de lei sobre dona fechada. `app/chamados/services/disputa_emissao.py:1274`, `:1275`, `:980`, `:1002`, `:1007`, `:1335`, `:1349`, `:1429`, `:1444`.

   Para dona aberta, os caminhos regulares conferidos encaminham o motivo; não demonstram exclusividade global nem removem cópias preexistentes. O contador também não detecta o caso acima, porque exige DS sem **nenhuma** pergunta. `app/chamados/services/disputa_emissao.py:566`, `:613`; `app/chamados/management/commands/auditar_invariantes_chamados.py:62`.

   Nos dois consumidores produtivos da declaração, os argumentos da pergunta agora entram no helper transacional. A estrangeira desfaz a agregação/nascimento local e repete a porta na dona. `app/chamados/services/declaracao_texto.py:362`, `:650`; `app/chamados/services/disputa_emissao.py:763`, `:777`, `:781`.

   Entretanto, a API pública ainda aceita `pergunta=None` e pode gravar DS aberta sem pergunta fora de HX/h2. Não encontrei consumidor produtivo externo dessa forma; ela é usada internamente e expressamente exercitada pelos testes. `app/chamados/services/disputa_emissao.py:747`, `:750`, `:754`, `:764`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:397`.

   O h2 só estrangeiro agora desfaz o nascimento. Com ata completa e nenhuma estrangeira, permanece o nascimento seguido de fechamento vazio; sem ata, permanece aberta e acende a luz. `app/chamados/services/disputa_emissao.py:1444`, `:1474`, `:1477`; `app/chamados/models.py:1903`.

3. **Push: momento, disputa e motivos.** [nao verificado]

   | Caminho | Comportamento atual |
   |---|---|
   | Nascimento pelo wrapper | Envia para a DS resultante, com motivos admitidos; exige que continue aberta. `app/chamados/services/disputa_emissao.py:699`, `:827`, `:838`. |
   | Agregação pelo wrapper | Envia os novos motivos de A; motivo repetido com pergunta de outro dia não notifica. `app/chamados/services/disputa_emissao.py:647`, `:832`, `:838`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:252`. |
   | Encaminhamento para B | **Não transporta os novos motivos de B para o push:** o retorno do agregador é usado apenas para verificar dona aberta. `app/chamados/services/disputa_emissao.py:565`, `:613`, `:817`, `:832`. |
   | Órfãos, principal | Envia `motivos_atualizados`, inclusive na repetição do mesmo ato e no caso HX sem pergunta. `app/chamados/services/disputa_emissao.py:1484`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:154`, `:162`. |
   | Complementos antes do filtro | Agrupa motivos por dona; exclui a principal para evitar repetir sua notificação nesse coletor. `app/chamados/services/disputa_emissao.py:1226`, `:1257`, `:1465`. |
   | Declaração | Não há envio direto nesse helper. `app/chamados/services/disputa_emissao.py:758`. |
   | Âncora | Envia `ancora_ausente` para a local após analisar o chamado. `app/chamados/services/disputa_emissao.py:1575`, `:1577`. |

   Assim, o wrapper mantém nascimento=admitidos e agregação=novos **da local**; órfãos mantém `motivos_atualizados`. A admissão nova na dona estrangeira não recebe tratamento equivalente de notificação. Aprovação específica dessa omissão e comparação histórica executável com `6c0517eb9`: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:613`, `:832`, `:1002`, `:1486`.

   Não encontrei envio direto da principal antes do rollback dos ensaios estrangeiros. O repasse desfeito retorna antes do push principal; os complementos notificados nesse ramo são anteriores ao savepoint interno. `app/chamados/services/disputa_emissao.py:1278`, `:1447`, `:1461`, `:1462`.

   Isso **não prova ausência de push por cascata**: o cartório é síncrono e há emissores indiretos. Cenário completo que comprove push escapando especificamente desse rollback: **[nao verificado]**. `app/chamados/signals.py:188`, `:216`; `app/ponto/management/commands/detectar_ausencias.py:778`, `:802`.

   O envio é imediato, sem `on_commit`. Portanto, uma falha depois da notificação dos complementos pode desfazer o atomic externo após o envio. Ocorrência concreta: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:1230`, `:1465`, `:1473`, `:1963`, `:1971`.

4. **Savepoints e replay.**

   Núcleo, órfãos e declaração repetem a porta com os argumentos conservados, trocando a disputa pela dona. As guardas verificam criação ou PK diferente; núcleo e órfãos desfazem o replay divergente, enquanto declaração levanta `ValueError`. `app/chamados/services/disputa_emissao.py:710`, `:711`, `:781`, `:782`, `:1453`, `:1454`.

   **Argumentos iguais não significam todos os insumos iguais:** no replay genérico BATIDA, o coletor não conserva `batida_origem_id`; `_nascer` volta a derivá-la do contexto da disputa recebida. O contexto da dona pode apontar outro episódio e alterar a identidade/dia procurados. Reprodução ORM desse caso: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:500`, `:504`; `app/chamados/models.py:2866`, `:2875`, `:3463`.

   Esse risco de episódio não se aplica da mesma forma aos motivos MARCO usados na declaração/repasse: o derivador retorna `None` para essa família. `app/chamados/models.py:3461`; `app/chamados/catalogo/motivos.py:34`, `:48`.

   **Órfãos continua analisando todos os chamados mesmo se o replay divergir.** A análise não está condicionada ao sucesso do replay correspondente; pode chamar o cartório depois de a costura ter sido desfeita. `app/chamados/services/disputa_emissao.py:1454`, `:1456`, `:1459`; `app/chamados/signals.py:216`.

   Os signals já executaram antes de a porta devolver o resultado: o callback de beco é registrado com `on_commit`, e o outbox síncrono escreve no banco. Esses mecanismos não demonstram, por si, vazamento externo; os testes de memória não certificam o comportamento transacional real desses receivers. `app/chamados/models.py:3006`, `:3017`; `app/chamados/signals.py:238`, `:257`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:50`.

5. **Agregador, segundo predicado e censo AST.**

   `_agregar_motivos_na_dona` é o ponto comum das agregações modificadas, mas **não é o único escritor de motivos/contexto do emissor**. A âncora conserva outro predicado de união, criação própria e atribuição/save direto de motivos. `app/chamados/services/disputa_emissao.py:558`, `:1531`, `:1554`, `:1562`.

   `ATOS_EMISSAO` corresponde ao censo executado pelo gate, mas declara explicitamente duas escritas da âncora. Logo, gate verde não demonstra agregador único. `app/chamados/tests/test_porta_disputa_emissao.py:83`, `:105`, `:118`, `:128`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:283`.

6. **Ordem C → D → Ch → P.**

   No núcleo, as donas são consulta lazy, avaliada depois de C; as travas externas permanecem durante o rollback interno e o replay. `app/chamados/services/disputa_emissao.py:623`, `:625`, `:627`, `:653`, `:701`; `app/chamados/models.py:2665`, `:2675`, `:2688`, `:2692`.

   No repasse, o fecho é travado antes das escritas, relido sob C e recusado se crescer; ensaio, replay e análise permanecem no atomic externo. `app/chamados/services/disputa_emissao.py:1156`, `:1167`, `:1173`, `:1178`, `:1215`.

   Nos dois consumidores da declaração, o atomic externo e `_trava_do_ato` antecedem o novo savepoint; a pré-trava inclui donas abertas/fechadas, chamados e perguntas do fato. `app/chamados/services/declaracao_texto.py:299`, `:301`, `:348`, `:352`, `:643`, `:645`.

   **A garantia não cobre autonomamente pendentes do wrapper:** ele chama `perguntar_entradas_pendentes` depois de sair do atomic do núcleo. O helper abre P e depois salva DS, sem atomic/trava próprios. Sem transação externa, P pode estar commitada antes da agregação de motivos/contexto; com transação externa, as travas anteriores permanecem. Não é prova de inversão simultânea de locks, mas é uma fronteira sem a atomicidade anunciada. `app/chamados/services/disputa_emissao.py:650`, `:830`, `:310`, `:313`, `:575`; `app/chamados/models.py:2819`.

7. **Testes e mordida.** [nao verificado]

   | Família | O que verifica / o que não prova |
   |---|---|
   | `sem_pergunta` | Rollback, preservação da batida e I7a/I7b. O ORM “depois da pergunta” não afirma que P nasceu antes de levantar; a irmã memória possui essa testemunha positiva. `app/chamados/tests/test_bo_disputa_sem_pergunta.py:58`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:301`. |
   | Volta2 | Consulta depois de C, dona fechada antes de Ch e trava repetida; mede doubles, não concorrência PostgreSQL. `app/chamados/tests/test_bo_disputa_volta2_memoria.py:143`, `:150`. |
   | Volta3/4 | Dias, ciclos e replay conservado. A “paridade BASE versus atual” executa a fonte atual duas vezes: prova repetibilidade, não paridade histórica. `app/chamados/tests/test_bo_disputa_volta3_memoria.py:236`, `:245`, `:300`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:20`, `:45`. |
   | Volta5 | Possui cenário adversarial de replay criado/PK diferente no núcleo. Porém `nasce_sem_pergunta` encontra cinco sítios e `replay_insere` encontra dois; ambos exigem um e abortam na montagem. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:66`, `:73`, `:77`, `:81`, `:194`. |
   | 5b | Reuso, critérios de push, HX e h2; a memória simula `analisar` só como mudança de status, sem cartório. `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:120`, `:152`, `:185`, `:258`. |
   | 5c | Exercita ambos os consumidores da declaração e agregação estrangeira. Não cobre candidata/costura real, pois candidata sempre retorna `None`, trava é Mock e a porta é double. O lote misto usa `intervalo_saida`, deixando passar o seed `orfao_14h`. `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:77`, `:89`, `:95`, `:209`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:22`. |
   | 5e | Demonstra que os asserts rejeitam dona fechada e ausência de nota. Chama a reabertura diretamente, com save/refresh simulados, e muta asserts do teste; não prova a integração cartório→ressuscitação. `app/chamados/tests/test_bo_disputa_volta5e_memoria.py:30`, `:49`, `:59`, `:72`, `:84`. |
   | Ajuste E1 | O mutante desliga `_trava_do_repasse` no agrupador real; não certifica todos os novos replays. `app/chamados/tests/test_e1_fila_v2_2_integracao.py:211`. |
   | Reuso 746 | Agora usa motivo com insumo de pergunta disponível; conserva asserts de reuso do pai/nota, mas deixa de exercitar o antigo caso `orfao_14h` sem insumo. `app/chamados/tests/test_3b_reuso_746.py:23`, `:28`, `:31`. |

   Os asserts protegidos estão presentes nos endereços indicados. Permanecerem **historicamente intocados** não pode ser certificado sem a base comparável. `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:63`; `app/chamados/tests/test_hx_disputa_muda.py:24`, `:44`; `app/chamados/tests/test_pergunta_no_app.py:89`, `:103`. Integridade histórica: **[nao verificado]**.

   Os 683 testes ORM são informação recebida, não certificação desta leitura. Suíte inteira verde: **[nao verificado]**. [nao verificado]

8. **Quem escreve, quem lê e semântica.**

   | Campo | Escritores e leitores confirmados |
   |---|---|
   | `DS.motivos` | Escrevem agregador, nascimentos, âncora, aparo/remoção e reabertura administrativa. Leem admissão, push e resolução de motivos. `app/chamados/services/disputa_emissao.py:558`, `:670`, `:1299`, `:1562`; `app/chamados/models.py:1966`, `:2063`, `:2159`; `app/api/foto_recebida.py:63`. |
   | `DS.contexto` | Escrevem agregador/nascimentos e portas de contexto/aparo/remoção. Leem resolvedor de pergunta, derivador de episódio e auditor. `app/chamados/services/disputa_emissao.py:572`, `:670`; `app/chamados/models.py:1967`, `:2064`, `:2100`, `:2118`, `:3463`; `app/chamados/management/commands/auditar_invariantes_chamados.py:31`. |
   | `DS.fechada_em` | Escrevem fechamento e reaberturas admin/sistema. Leem eleição, agregador, push, receivers e app. `app/chamados/models.py:1932`, `:2149`, `:2173`; `app/chamados/services/disputa_emissao.py:563`, `:630`, `:827`; `app/chamados/signals.py:28`; `app/api/views.py:1059`. |
   | `P.disputa` | Nasce no construtor; reuso retorna a canônica preservando a FK. Leem roteamento do motivo, autorização e fallback de listagem. `app/chamados/models.py:3008`, `:3014`; `app/chamados/services/disputa_emissao.py:606`; `app/chamados/services/fio.py:598`; `app/api/views.py:1063`. |
   | `P.chamado_destino` | Escrevem nascimento, reuso e costura; leem juiz da resolvedora e seleção do card. `app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`; `app/api/views.py:1061`. |

   Há tensão semântica verificável: `motivos` é documentado como cenários ativos; o núcleo admite pelo vínculo com pergunta local, enquanto órfãos conserva o seed sem essa pergunta. Além disso, contexto da DS é usado para derivar identidade BATIDA, embora seja atualizado ao longo dos atos. `app/chamados/models.py:1741`, `:3455`; `app/chamados/services/disputa_emissao.py:598`, `:1275`.

   Esse mapa não certifica censo exaustivo de todos os leitores nem ausência de outras premissas divergentes. **ESMERIL-SEMANTICA completo: [nao verificado].**

9. **Classes de reprovação e proibidos.**

   Permanecem segundo escritor/predicado na âncora, insumo de identidade rederivado no replay, selo sem mordida em dois mutantes e fronteira transacional incompleta nos pendentes. `app/chamados/services/disputa_emissao.py:1531`, `:1562`, `:500`, `:830`; `app/chamados/models.py:2866`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`.

   O cartório continua sendo chamado pela análise; a ressuscitação continua usando a lei HX existente. A falta de ata continua impedindo fechamento e mantendo o aviso, agora acompanhado do contador. `app/chamados/signals.py:216`; `app/chamados/models.py:1574`; `app/chamados/services/disputa_emissao.py:1474`; `app/chamados/management/commands/auditar_invariantes_chamados.py:74`.

   Alteração das sete disputas existentes, execução em produção, identidade criptográfica da cópia, aprovação de critérios adicionais pelos briefs ausentes e ausência de qualquer outra lei nova: **[nao verificado]**.

**NAO LIBERA. Achados que permanecem:**

- Motivo `orfao_14h` pode ficar indevidamente nas duas disputas abertas no lote misto. `app/chamados/services/disputa_emissao.py:1275`, `:1007`, `:1429`.
- Replay genérico conserva kwargs, mas rederiva episódio pela dona; órfãos ainda analisa chamados após replay divergente. `app/chamados/services/disputa_emissao.py:500`, `:1459`; `app/chamados/models.py:2866`.
- Dois mutantes abortam por montagem, e a suposta comparação com a base compara a versão atual consigo mesma. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`; `app/chamados/tests/test_bo_disputa_volta3_memoria.py:245`.
- Agregador único não está estabelecido; pendentes podem agregar fora do atomic do núcleo; novas admissões na dona estrangeira não chegam ao coletor de push. `app/chamados/services/disputa_emissao.py:1562`, `:830`, `:613`.
- Não verificados: achados individualizados dos pareceres ausentes, histórico dos protegidos, reprodução ORM dos novos contraexemplos, efeitos externos completos das cascatas e suíte inteira. **[nao verificado]**. [nao verificado]

## 10/11 22:10 · sessao 093c3c32dff7 · - · - · origem=Ronald (tmux)

Travas e efeitos:

- Núcleo genérico prende C antes de avaliar `donas`, depois D e Ch; conserva essas travas fora do savepoint desfeito e durante o replay. A implementação de `trava_na_ordem` adquire C/D/Ch/P por essa sequência. `app/chamados/services/disputa_emissao.py:623`, `:652`, `:701`; `app/chamados/models.py:2665`, `:2675`, `:2688`, `:2692`.
- Órfãos adquire o fecho antes de escrever; relê sob C e recusa crescimento. O atomic externo engloba ensaio, replay e análise posterior. `app/chamados/services/disputa_emissao.py:1156`, `:1166`, `:1173`, `:1175`, `:1216`, `:1223`, `:1450`.
- Declaração pelos dois callers conserva atomic externo e pré-trava do fato antes do novo savepoint. `_trava_do_ato` inclui donas abertas/fechadas, chamados e perguntas do fato. `app/chamados/services/declaracao_texto.py:288`, `:299`, `:301`, `:348`, `:352`, `:643`, `:645`; `app/chamados/services/disputa_emissao.py:763`.
- **Lacuna em pendentes da varredura:** o wrapper termina o atomic do núcleo antes de chamar `perguntar_entradas_pendentes`; esse helper agora agrega/salva DS depois da porta de P, sem atomic/trava próprios. Sem atomic externo, a pergunta pode estar commitada antes da escrita de motivos/contexto, e a DS é salva sem conservar C/D. Com atomic externo, as travas do núcleo continuam cobrindo DS do colaborador. `app/chamados/services/disputa_emissao.py:623`, `:650`, `:817`, `:830`, `:310`, `:313`, `:575`. API chama diretamente: `app/api/views.py:794`; cron: `app/ponto/management/commands/marcar_foto_ausente_retro.py:94`.

Replay:

- Os kwargs conservam dia/texto/destino/opções, mas **não congelam todo o fato na família BATIDA**: o coletor omite `batida_origem_id`, e `_nascer` deriva novamente o episódio do contexto da disputa recebida. Trocar a local pela dona pode mudar episódio e, pelo juiz, dia/canônica. `app/chamados/services/disputa_emissao.py:500`, `:503`, `:710`; `app/chamados/models.py:2866`, `:2875`, `:3452`, `:3463`.
- `criada=True` ou pk diferente é desfeito no núcleo/órfãos e levanta na declaração. Antes dessa guarda, insert/save e seus signals já executaram. `app/chamados/services/disputa_emissao.py:710`, `:711`, `:1453`, `:1454`, `:781`, `:782`; `app/chamados/models.py:3006`, `:3017`.
- Os motivos usados nos caminhos de declaração/órfãos são MARCO; o derivador de episódio devolve `None` para essa família. O risco de rederivação de episódio acima se aplica ao replay genérico BATIDA. `app/chamados/catalogo/motivos.py:34`, `:35`, `:48`, `:49`; `app/chamados/models.py:3461`.

Rollback e push: [nao verificado]

- Signals de P agendam beco com `on_commit` e escrevem outbox síncrono no banco. Não são, por si, envio externo de push. `app/chamados/signals.py:227`, `:238`, `:250`, `:257`.
- `_enviar_push_disputa_colab` é envio imediato, webpush e FCM; não usa `on_commit`. `app/chamados/services/disputa_emissao.py:1947`, `:1963`, `:1971`; `app/chamados/push_utils.py:40`; `app/chamados/fcm_utils.py:85`.
- Nos blocos explicitamente desfeitos por estrangeira, não encontrei chamada direta de `_enviar_push_disputa_colab` antes de `set_rollback`. Núcleo envia no wrapper; órfãos envia após sair do savepoint; declaração não envia diretamente. `app/chamados/services/disputa_emissao.py:681`, `:838`, `:1444`, `:1461`, `:1465`, `:1486`, `:758`.
- Há cascata síncrona real: `Chamado.abrir` → `get_or_create` → signal do cartório → possível emissor de ausência → push de supervisão e repasse. `app/chamados/models.py:971`; `app/chamados/signals.py:188`, `:214`, `:216`; `app/ponto/services/cartorio.py:1751`, `:1789`; `app/ponto/management/commands/detectar_ausencias.py:778`, `:802`. **Não reproduzi cenário completo admissível que prove push escapando especificamente do rollback por estrangeira [nao verificado].**
- Push de complementos ocorre ainda dentro do atomic externo e antes de `pode_fechar`; falha posterior pode desfazer banco após esse envio imediato. Não há teste/reprodução desse caso nesta auditoria. `app/chamados/services/disputa_emissao.py:1216`, `:1230`, `:1465`, `:1473` [nao verificado quanto à ocorrência].

Escritores/leitores e semântica:

| Campo | Escritores identificados | Leitores relevantes |
|---|---|---|
| DS.motivos | agregador; creates do núcleo/órfãos; **âncora mantém união/save próprios**; portas de remoção/aparo/reabertura admin | admissão/push; foto recebida; fechamento/materialização/listagem | [nao verificado]
| DS.contexto | agregador; creates; portas de anotar/renovar/aparar/remover | resolução dia/episódio, auditor I7a/b, derivação de destino |
| DS.fechada_em | `fechar`, `reabrir_admin`, `reabrir_sistema` | eleição da aberta, guardas agregador/push, três receivers de fechamento | [nao verificado]
| P.disputa | construtor `_nascer`; reuso devolve `ex` preservando FK | dona do agregado; eleição local/estrangeira; fallback de listagem |
| P.chamado_destino | `_nascer` e `fio.reapontar_resolvedora`; há costura em `ChamadoColaborador` | âncora que lista, juiz de resolvedora, vida do fio, S143 |

Referências da tabela: `app/chamados/services/disputa_emissao.py:558`, `:670`, `:1299`, `:1531`, `:1554`, `:1562`; `app/chamados/models.py:1961`, `:2063`, `:2100`, `:2118`, `:2149`, `:2155`, `:2173`, `:1932`, `:2982`, `:3008`, `:3014`, `:367`; `app/chamados/services/fio.py:925`, `:116`; `app/api/foto_recebida.py:63`; `app/chamados/signals.py:28`, `:129`, `:153`.

**A frase “agregador único” não vale literalmente nem dentro do emissor:** a âncora continua com o segundo predicado de união e o segundo save; o gate declara duas escritas dessa função. `app/chamados/services/disputa_emissao.py:1531`, `:1562`; `app/chamados/tests/test_porta_disputa_emissao.py:83`.

DS.motivos continua documentado como “cenários ativos”, enquanto o núcleo passa a admitir motivos somente com pergunta local; órfãos ainda conserva seed `orfao_14h`, e contexto pode guardar informação de motivo encaminhado para outra dona. Há diferença semântica entre essas populações; impacto completo em todos os leitores **[nao verificado]**. `app/chamados/models.py:1741`; `app/chamados/services/disputa_emissao.py:598`, `:638`, `:633`, `:1275`.

## 10/11 22:09 · sessao 5fbc4ce8791f · - · - · origem=Ronald (tmux)

Achados concretos:

1. **Motivo pode terminar nas duas disputas abertas.** No repasse, `motivos_atualizados` recebe `['orfao_14h']` também quando A já existe com `motivos=[]`; esse seed é salvo antes de perguntar. Se a pergunta `orfao_14h` pertence a B, o helper devolve `motivos_antes`, que já contém o seed, e agrega o motivo em B. Um motivo local de intervalo mantém A com pergunta, impedindo desfazer A. Resultado: A tem `intervalo_saida` e `orfao_14h`, B tem `orfao_14h`, mas a pergunta desse motivo continua só em B. Referências: `app/chamados/services/disputa_emissao.py:1274`, `:1307`, `:980`, `:1002`, `:1007`, `:1313`, `:1349`, `:1429`, `:1444`. Confirmei em sonda inline `python3 -B`, usando as funções produtivas extraídas pelo ambiente de memória; persistência ORM desse cenário [nao verificado].

2. **Replay conserva kwargs, mas não todos os insumos implícitos da identidade.** O gerador conserva motivo/texto/opções/destino/dia, omitindo `batida_origem_id`; a porta deriva essa origem do contexto da DS fornecida. No replay, a DS passa a ser a dona, cujo contexto pode conter outro episódio. Referências: `app/chamados/services/disputa_emissao.py:500`, `:504`, `:710`; `app/chamados/models.py:2866`, `:3452`, `:3463`. O resultado pode divergir: as guardas verificam `criada` ou PK diferente e desfazem o banco: emissor `:711`, órfãos `:1454`, declaração `:782`. Reprodução ORM com família BATIDA [nao verificado].

3. **Replay divergente dos órfãos ainda analisa os chamados.** Depois de desfazer o replay divergente, o agrupador executa toda a lista `analisar`, independentemente de qual replay teve sucesso. Referências: `app/chamados/services/disputa_emissao.py:1454`, `:1456`, `:1459`. A declaração levanta `ValueError`; o núcleo registra `replay-divergente`, portanto as três respostas diferem: `:783`, `:715`. Consequências completas do cartório nessa divergência [nao verificado].

4. **O agregador não é o único escritor em todo o módulo.** A âncora continua criando DS com motivos/contexto e salvando diretamente `disputa.motivos`; também ignora o retorno de `abrir_conversa`. Referências: `app/chamados/services/disputa_emissao.py:1554`, `:1562`, `:1567`. São sítios preservados no diff integral fornecido; atribuir essa duplicidade especificamente ao delta v11 seria [nao verificado].

5. **Pendentes do wrapper ficam fora do atomic próprio do núcleo.** O núcleo retorna dentro de seu atomic; o wrapper chama pendentes depois. O helper abre P pelo atomic da porta, depois salva D no agregador sem atomic próprio. Referências: `app/chamados/services/disputa_emissao.py:623`, `:650`, `:699`, `:830`, `:831`, `:310`, `:313`, `:575`; `app/chamados/models.py:2819`. Garantia de trava externa em todos os consumidores [nao verificado].

Mapa de push: [nao verificado]

- Wrapper: nascimento usa admitidos; agregação usa novos de A; verifica DS aberta e, se pendentes chamou o gerador, verifica novamente. Referências: `app/chamados/services/disputa_emissao.py:699`, `:647`, `:817`, `:827`, `:832`, `:835`.
- Admissões em B não produzem push próprio: `agregar_estrangeiras` descarta a lista de novos do agregador; helper de órfãos também não coleta B para notificação. Referências: `app/chamados/services/disputa_emissao.py:565`, `:576`, `:613`, `:1002`. Se isso viola o aval versus apenas expõe um caso não especificado: [nao verificado].
- Órfãos: principal recebe `motivos_atualizados`; complementadas recebem união `_mot`; evita repetir a principal no coletor. Referências: `app/chamados/services/disputa_emissao.py:1257`, `:1258`, `:1228`, `:1465`, `:1484`.
- Ensaio de órfãos desfeito não chega ao push da principal; só notifica complementos anteriores ao savepoint. Referências: `app/chamados/services/disputa_emissao.py:1278`, `:1447`, `:1461`, `:1462`.
- Declaração não chama push nesse helper; âncora chama uma vez. Referências: `app/chamados/services/disputa_emissao.py:758`, `:785`, `:1577`.
- O envio efetivo é síncrono, Web Push e FCM, sem `on_commit` nessa função. Referências: `app/chamados/services/disputa_emissao.py:1963`, `:1971`. Entrega externa ou duplicidade efetiva [nao verificado].

Retornos:

- Chamadas datadas do gerador, replay do núcleo, helper de órfãos e replay dos órfãos consomem P e criada; a primeira tentativa da declaração usa P e descarta criada. Referências: `app/chamados/services/disputa_emissao.py:502`, `:523`, `:710`, `:990`, `:1008`, `:1453`, `:773`.
- `abrir_conversa` do gerador consome ambos; âncora descarta ambos. Referências: `app/chamados/services/disputa_emissao.py:495`, `:523`, `:1567`.
- Os dois consumidores produtivos externos da declaração passam `pergunta`; a forma sem pergunta aparece internamente e em testes. Referências: `app/chamados/services/declaracao_texto.py:362`, `:650`; `app/chamados/services/disputa_emissao.py:764`; `app/chamados/tests/test_porta_disputa_emissao.py:175`.
- API `disputa_para_declaracao(pergunta=None)` ainda pode criar DS sem P; isso é exercitado intencionalmente por teste. Referências: `app/chamados/services/disputa_emissao.py:747`, `:754`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:405`.
- H2 sem estrangeira mantém fecho vazio; com estrangeira e sem P local desfaz o nascimento. Referências: `app/chamados/services/disputa_emissao.py:1444`, `:1473`, `:1479`.

Lacunas de testes pertinentes: [nao verificado]

- O lote misto existente usa estrangeira `intervalo_saida`, não `orfao_14h`, portanto não prova remoção do seed. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:22`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:209`.
- O double de identidade compara apenas motivo e dia, sem episódio; não prova a estabilidade implícita do replay BATIDA. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:109`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:101`.
- Nenhuma alteração de arquivo foi necessária para as sondas inline; efeitos ORM, PostgreSQL, banco/log de produção e suite inteira [nao verificado]. [nao verificado]

## 10/11 22:08 · sessao cabc2df259bf · - · - · origem=Ronald (tmux)

Achados prioritários:

- **Selo quebrado:** `nasce_sem_pergunta` e `replay_insere` varrem o arquivo inteiro, mas exigem exatamente uma alteração. Hoje encontram respectivamente cinco e dois sítios; abortam por montagem antes de demonstrar assertion RED. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:66`, `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`; sítios em `app/chamados/services/disputa_emissao.py:682`, `:776`, `:778`, `:1446`, `:1448` e `:711`, `:1454`.
- **Paridade sobre a própria versão:** o teste anuncia gerador BASE versus atual, mas instancia `AmbienteVolta3()` nas duas repetições; a comparação final demonstra repetibilidade da fonte atual. As assertions de dia esperado continuam úteis. `app/chamados/tests/test_bo_disputa_volta3_memoria.py:236`, `:245`, `:246`, `:289`, `:292`, `:300`.
- **5e testa o assert, não a integração:** chama diretamente `reabrir_sistema`, com `save`/`refresh_from_db` simulados; os mutantes removem assertions do teste. Não certifica que `analisar()`/cartório efetivamente reabre a dona. `app/chamados/tests/test_bo_disputa_volta5e_memoria.py:30`, `:46`, `:49`, `:59`, `:70`, `:74`.
- **Declaração sem cobertura do ramo candidata/costura:** `_candidata_reusavel` sempre retorna `None`, trava é `Mock` e `PerguntaDisputa.abrir` é double. A cobertura não prova `disputa_emissao.py:765-771`, ordem real de travas nem efeitos dos receivers. `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:77`, `:89`, `:95`, `:97`.
- **Divergência nova sem selo específico:** os testes 5c demonstram replay bem sucedido e rollback inicial, mas não forçam pergunta criada/PK diferente no replay de declaração ou órfãos; o teste adversarial existente exerce o replay do núcleo. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:194`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:118`, `:192`; portas `app/chamados/services/disputa_emissao.py:781`, `:1453`.
- **Possível vacuidade no ORM “depois da pergunta”:** chama o gerador e levanta sem afirmar que alguma pergunta nasceu antes. A irmã memória contém essa assertion positiva. `app/chamados/tests/test_bo_disputa_sem_pergunta.py:58`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:301`.

Cobertura útil existente:

| Família | O que morde | Limite |
|---|---|---|
| Sem pergunta | Exceção antes/depois da escrita, preservação da batida, separação I7a/I7b e pergunta morta como linha. `test_bo_disputa_sem_pergunta_memoria.py:286`, `:296`, `:329`, `:355`, `:376` | Atomic/ORM/signals simulados. `test_bo_disputa_sem_pergunta_memoria.py:50` |
| Volta2 | Consulta lazy após C, dona fechada antes de Ch e ausência de trava repetida. `test_bo_disputa_volta2_memoria.py:143` | Conta eventos dos doubles; não SQL/concorrência PostgreSQL. `test_bo_disputa_volta2_memoria.py:1` |
| Volta3/4 | Ordem inicial, dias mistos/indeterminados, ciclos sem reentrada e argumentos conservados. `test_bo_disputa_volta3_memoria.py:156`; `test_bo_disputa_volta4_memoria.py:20`, `:45`, `:106` | Porta de pergunta simulada. `test_bo_disputa_volta4_memoria.py:31`, `:63` |
| Volta5 | Motivo somente na dona, rollback por divergência, idempotência, push e AST sem reentrada. `test_bo_disputa_volta5_memoria.py:121`, `:194`, `:270`, `:334` | Dois mutantes quebrados acima; cenário de dona aberta escondida é artificial e explicitamente declarado. `test_bo_disputa_volta5_memoria.py:233` | [nao verificado]
| 5b | Reuso estrangeiro, push por critério base, sem ata, h2 desfeito, criadas reais/pendentes. `test_bo_disputa_volta5b_orfaos_memoria.py:152`, `:185`, `:223`, `:239`, `:258` | Double de `analisar()` só muda status; não executa cartório. `test_bo_disputa_volta5b_orfaos_memoria.py:120` | [nao verificado]
| 5c | Dois callers da declaração, saídas normais/recusa, rollback, pendentes estrangeiros, agregador e critérios opostos de push. `test_bo_disputa_volta5c_memoria.py:118`, `:149`, `:176`, `:237`, `:252`, `:262` | Limites da declaração acima. | [nao verificado]
| Gate | Censo real por função confrontado em igualdade exata com `ATOS_EMISSAO`. `test_porta_disputa_emissao.py:105`, `:118`, `:128`; reutilizado em `test_bo_disputa_volta5c_memoria.py:283` | Conta sítios/escritores; não prova semântica de motivos/contexto. `test_porta_disputa_emissao.py:122` |
| 5e | Assert rejeita dona fechada mesmo com nota antiga e reabertura sem nota. `test_bo_disputa_volta5e_memoria.py:50`, `:84`, `:88` | Não prova integração. |

Os protegidos contêm os asserts solicitados: sem ata continua sem pergunta/aberta; com ata completa fecha; muda não notifica; pergunta antiga aponta ao dia correto; duas passadas conservam uma pergunta. `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:63`; `app/chamados/tests/test_hx_disputa_muda.py:24`, `:44`; `app/chamados/tests/test_pergunta_no_app.py:89`, `:103`. Permanecerem **historicamente intocados**: [nao verificado], pois não há `.git`/base disponível.

Executei previamente 82 testes de memória em nove módulos, todos verdes, com `PYTHONDONTWRITEBYTECODE=1 python3 -B`; não executei ORM nem suite inteira. Evidência em arquivo/log da execução: [nao verificado]. Após receber a regra ` .claude/agents/verificador.md:7`, limitei os próximos comandos a leitura/AST; não executei os mutantes.

`opus5d.txt`, `preverif3.r`, `matriz2.r` e briefs não foram encontrados na cópia; conteúdo/achados individualizados: [nao verificado].

_3529 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
