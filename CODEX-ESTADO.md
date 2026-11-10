# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 15:00** · publicado em **10/11/2026 15:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 15:00 · sessao e5626d6a372f · BO-0611-05 · construtor · origem=Code

Cura B1/B2/B3 implementada. A certificação por Code/opus, o POST com banco e o smoke nas duas cascas permanecem **[nao verificado]**.

Arquivos de produção tocados:

- [api/views_core.py](/home/ronald/wt-codex-bo-0611-05-144805/app/api/views_core.py:393) e [api/views.py](/home/ronald/wt-codex-bo-0611-05-144805/app/api/views.py:406): retirados os 403 por GPS/situação e a auditoria de recusa sem GPS.
- [bater_ponto_v2.html](/home/ronald/wt-codex-bo-0611-05-144805/app/templates/ponto/bater_ponto_v2.html:285): retiradas as três interrupções por coordenada ausente, o tratamento específico de `gps_obrigatorio` e a função morta `v2AvisoGps`.
- [triagem/apto.html](/home/ronald/wt-codex-bo-0611-05-144805/app/templates/ponto/triagem/apto.html:275): aviso sem confirmação; o fluxo segue para a fila.

Os testes invertidos/adaptados foram preservados: [nao verificado]

| Arquivo tocado | Asserção resultante |
|---|---|
| [test_gps_obrigatorio.py](/home/ronald/wt-codex-bo-0611-05-144805/app/api/tests/test_gps_obrigatorio.py:19) | 200, uma batida com nulos e nenhuma auditoria de recusa |
| [test_api_bater_ponto.py](/home/ronald/wt-codex-bo-0611-05-144805/app/api/tests/test_api_bater_ponto.py:62) | Desligado/férias registram |
| [test_batida_sideeffects.py](/home/ronald/wt-codex-bo-0611-05-144805/app/ponto/tests/test_batida_sideeffects.py:119) | Uma batida, alerta `sem_gps` e chamado existente |
| [test_afastado_nunca_bloqueia.py](/home/ronald/wt-codex-bo-0611-05-144805/app/ponto/tests/test_afastado_nunca_bloqueia.py:30) | Selo AST nos dois kernels |
| [test_afastado_na_batida.py](/home/ronald/wt-codex-bo-0611-05-144805/app/ponto/tests/test_afastado_na_batida.py:109) | Férias registra mesmo com afastamento cobrindo hoje |
| [test_invariantes_disputa.py](/home/ronald/wt-codex-bo-0611-05-144805/app/chamados/tests/test_invariantes_disputa.py:291) | Removida a exceção que aceitava recusa por GPS |
| [test_selo_modal_nao_nasce_vazio.py](/home/ronald/wt-codex-bo-0611-05-144805/app/core/tests/test_selo_modal_nao_nasce_vazio.py:138) | Exige `naoBloquear` quando existe chamada a `hxConfirmar` |

Quatro arquivos novos:

- [test_l030_selo_porta.py](/home/ronald/wt-codex-bo-0611-05-144805/app/api/tests/test_l030_selo_porta.py:73): AST, prosa não confundida com código e mutantes.
- [test_l030_forma_js.py](/home/ronald/wt-codex-bo-0611-05-144805/app/ponto/tests/test_l030_forma_js.py:69): forma dos ramos JS e mutantes das quatro interrupções.
- [test_l030_batida_pela_porta.py](/home/ronald/wt-codex-bo-0611-05-144805/app/api/tests/test_l030_batida_pela_porta.py:36): POST nos dois kernels, espião delegando ao escritor real, exatamente uma batida; cobre situações, nulos, vazios, zero e coordenada parcial.
- [test_l030_templates_renderizados.py](/home/ronald/wt-codex-bo-0611-05-144805/app/ponto/tests/test_l030_templates_renderizados.py:14): render dos dois templates compostos com cada casca real, mediante loader de teste. [nao verificado]

O RED local antes da cura apresentou **quatro falhas pretendidas**, uma por kernel/template. Depois da cura, os cinco testes locais passaram, incluindo controles verdes e detecção de **14 mutantes de API e quatro de front**. Os oráculos estão em `api/tests/test_l030_selo_porta.py:70` e `ponto/tests/test_l030_forma_js.py:31`.

Comando do RED na base e GREEN na cura:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  api.tests.test_l030_selo_porta.PortaSemRecusaTest.test_MORDE_os_dois_kernels \
  ponto.tests.test_l030_forma_js.FormaGpsTest.test_MORDE_fonte_dos_dois_templates -v
```

Comando dos mutantes, cada qual com controle verde antes da mutação: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  api.tests.test_l030_selo_porta.PortaSemRecusaTest.test_mutantes_em_memoria_com_controle_verde \
  ponto.tests.test_l030_forma_js.FormaGpsTest.test_mutantes_em_memoria_com_controle_verde -v
```

Sobre escritores e leitores:

- **Latitude/longitude/accuracy:** as APIs recebem os campos e entregam à porta; `registrar_batida` sanitiza por campo e grava. O clássico já entrega nulos à mesma porta. (`api/views_core.py:403`, `api/views_core.py:640`, `api/views.py:416`, `api/views.py:648`, `ponto/registro_batida.py:5`, `ponto/registro_batida.py:157`, `ponto/services/triagem_batida.py:701`)
- **Leitores de geo conferidos:** detalhe, fio, situacional, pino, drawer, dashboard, pendentes, relatório e cerca. Há tratamentos de nulos nesses caminhos. (`colaboradores/services/detalhe.py:22`, `chamados/views.py:692`, `colaboradores/services/situacional.py:141`, `colaboradores/painel_cell.py:37`, `colaboradores/services/drawer.py:63`, `core/views.py:1024`, `ponto/views.py:485`, `templates/chamados/partials/_relatorio_geo.html:30`, `ponto/services/geofence.py:247`)
- **Zero não é uniformemente interpretado:** o escritor preserva coordenada `0.0`; cerca, drawer e dashboard usam truthiness e podem apresentá-la como ausência. Esses leitores ficaram fora da cura. (`ponto/registro_batida.py:8`, `ponto/services/geofence.py:247`, `colaboradores/services/drawer.py:63`, `templates/core/dashboard2.html:242`)
- **Coordenada parcial pode passar:** a API recebe os campos separadamente. Latitude preenchida, longitude nula e accuracy preenchida podem alcançar `float(None)` no detector de vínculo. Ocorrência em produção **[nao verificado]**. (`api/views_core.py:403`, `api/views_core.py:646`, `ponto/services/vinculo_na_lista.py:49`, `ponto/services/vinculo_na_lista.py:56`)
- **Situação:** o diff não cria fotografia histórica; o cadastro segue mutável. Desligamento/reativação escrevem a situação atual. Cartório e reconciliação de vínculo leem essa situação atual; férias são verificadas pelo juiz de hoje. (`colaboradores/services/desligamento.py:104`, `colaboradores/services/desligamento.py:210`, `ponto/services/cartorio.py:1708`, `ponto/services/geofence.py:46`, `ponto/utils.py:85`)
- **Pós-escritor:** recomposição e fechamento por evento possuem captura de exceções; o alerta `sem_gps` continua posterior à gravação. Execução completa desses efeitos para desligado/férias **[nao verificado]**. (`ponto/signals.py:19`, `ponto/registro_batida.py:175`, `api/views_core.py:769`, `ponto/services/geofence.py:247`)

Não executados: RED/GREEN HTTP com banco, renderização Django, suíte solicitada, Ruff e smoke de clique nas duas cascas. As tentativas locais de carregar os testes Django falharam por ausência de `django`; Ruff falhou com `No module named ruff`. Sintaxe AST dos 13 Python tocados e comparação do selo de relógio passaram; certificação permanece **[nao verificado]**. [nao verificado]

Comando pendente para Code, na raia autorizada:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  api.tests.test_gps_obrigatorio api.tests.test_api_bater_ponto \
  ponto.tests.test_afastado_nunca_bloqueia ponto.tests.test_afastado_na_batida \
  ponto.tests.test_batida_sideeffects \
  api.tests.test_l030_selo_porta api.tests.test_l030_batida_pela_porta \
  ponto.tests.test_l030_forma_js ponto.tests.test_l030_templates_renderizados \
  core.tests.test_selo_modal_nao_nasce_vazio core.tests.test_selo_teste_sem_relogio \
  chamados.tests.test_invariantes_disputa
```

## 10/11 15:00 · sessao 6d9c0cab7930 · - · - · origem=Ronald (tmux)

Foram encontrados **3 sítios de criação produtivos**, todos em `app/chamados/services/disputa_emissao.py`: **587, 1123 e 1358**. Varredura AST de 3.493 arquivos, seguindo importações com alias, aliases por atribuição, managers, cadeias ORM e `apps.get_model`: nenhum outro `create/get_or_create/update_or_create/bulk_create` ou construção de instância nova de `DisputaSupervisao` fora de testes. A contagem é resultado da análise em memória, sem arquivo persistido: [nao verificado].

| Sítio | Motivos no nascimento | Transação/pergunta | Saídas com D nova sem P |
|---|---|---|---|
| `_abrir_ou_agregar_disputa`, **587** | `list(motivos_lista)`, sem validar catálogo (**589**). Pode gravar grave. | `atomic` próprio **521**, termina retornando **603**, sem criar P. `criar_ou_atualizar_disputa` chama núcleo **662** e gerador posteriormente **670**. | Núcleo sempre retorna D antes de P (**603**). Gerador pode suprimir todos motivos ou lançar antes de inserir. Atomic externo pode evitar persistência em exceção; não impede reuso de P pertencente a outra D (**models.py:2907,2921,3008**). |
| `criar_disputa_a_partir_de_chamados_orfaos`, **1123** | Nova começa `['orfao_14h']` (**1100–1101**), depois agrega motivos de marco. Grave não nasce deste create. | Atomic comum **1052**, pergunta depois de D, até retorno **1291**. | Guardas podem deixar zero P; ausência de ata impede fechamento automático (**1279–1281**). Exceção não capturada desfaz D; `UniversoMudouSobATrava` capturada **1293** somente após rollback. |
| `criar_disputa_ancora_de_chamado`, **1358** | Nova somente `['ancora_ausente']` (**1335**). Grave não nasce deste create. | Atomic comum **1322**, `abrir_conversa` **1371**, retorno **1382**. | Guardas **1316,1318,1323** precedem create. Nenhuma saída entre create e pergunta. Exceções propagam e desfazem D. Conversa não usa dedup de fato (**models.py:2893**). |

**Entrada de grave:** `app/api/views_core.py:730` e `app/api/views.py:733` acrescentam literalmente `batida_fora_escala_grave`; contexto `ultima_batida` em ambos **788** e chamada do helper **794**. Catálogo `app/chamados/catalogo/perguntas.py:82` define pergunta grave como `escolha` (**84**), sem `{data}` (**83**). Portanto as guardas de texto sem data e de pergunta `hora` não explicam, por si, uma disputa cujo único motivo é grave: `disputa_emissao.py:346,360,425`.

**Ramo compatível com grave sem P:** `PerguntaDisputa._nascer` busca o fato global por colaborador/motivo/dia/episódio em `app/chamados/models.py:2907`, escolhe canônica em **2921**, atualiza apenas célula/destino em **2963–3006**, retorna a P existente em **3008**. Não transfere `disputa_id`. O gerador aceita a P devolvida e a acrescenta ao resultado em `disputa_emissao.py:470`; o chamador ignora o resultado em **670** e retorna D em **672**. Assim, P canônica de outra disputa permite sucesso normal deixando a recém-criada sem P. Atribuição das sete ocorrências a esse ramo: [nao verificado].

**Todas guardas relevantes do gerador genérico:** motivo sem catálogo **315–316**; dedup local **325–329**; texto datado sem dia da grade **346–366**; motivo datado sem data ou já existente **398–401**; dedup por destino/data **417–420**; hora sem dia **425–430**; dedup de conversa **436–438**. Dedups locais implicam alguma P já existente na própria D; não explicam zero P numa D realmente nova. Exceção engolida de reatar em **462–469** ocorre depois do nascimento da P. Todas essas referências: `app/chamados/services/disputa_emissao.py`.

**Guardas do agrupador de órfãos:** intervalo malformado/data inválida **1144,1149**; ata ausente **1172,1210,1241**; classificador sem motivo **1201,1238**; sem dia resolvido **1206**; sem origem e sem dia/base **1233,1235**. `_criar_pergunta_se_couber` acrescenta motivo antes das guardas (**828**), suprime por ausência coberta (**830–831**) ou dedup (**832–833**); ignora a informação de criação retornada por `abrir` (**837–847**). Complemento sem célula retorna falso (**862–867**), e erro de leitura é engolido (**868–871**). Fechamento final: **1270–1291**. Todas em `disputa_emissao.py`.

O classificador delegado em `disputa_emissao.py:799–806` somente emite `saida_sem_entrada`, `orfao_14h` ou nenhum motivo: `app/escala/servico_jornada.py:445–471`. Irmãs vêm de `_MOTIVO_POR_MARCO`, somente `saida_sem_entrada`, `orfao_14h`, `intervalo_saida`, `intervalo_volta`: `app/chamados/services/declaracao_texto.py:42–47`.

**Todos chamadores do núcleo:** helper genérico `disputa_emissao.py:662`; declaração `disputa_emissao.py:627`, chamada por `declaracao_texto.py:360,659`, dentro dos atomics **348,654**; `reconciliar_grade.py:424` passa motivos `[]` e retorna sem perguntar **428** — seu helper `_disputa` não tem chamador encontrado na cópia [nao verificado]; `smoke_chamados.py:99` percorre todos `MOTIVOS_CHOICES` (**91**), incluindo grave (`models.py:1725`), mas recusa banco cujo nome não contenha `test` (**32–35**) e força rollback (**39–43**).

**Todos chamadores do helper genérico:** APIs **794** citadas; `app/ponto/services/triagem_batida.py:734`, somente `saida_sem_entrada` (**736**); `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, somente `foto_ausente` (**97**); `app/chamados/services/fabrica_da_celula.py:172`, recebe motivos do argumento de `perguntar` (**160**).

Fixtures detectadas: **388 sítios em 296 arquivos**, sendo 387 `create` e um `get_or_create` (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:81`). Nenhum argumento `motivos` desses nós contém literalmente grave; alguns recebem variáveis. Contagem e ausência global: [nao verificado]. Exemplo que demonstra criação direta sem pergunta: `app/core/tests/test_placar_situacional_um_so_numero.py:75`.

## 10/11 14:59 · sessao 5d1065688923 · - · - · origem=Ronald (tmux)

**4 — Leitores e comportamento com zero perguntas**

Não é correto dizer que S127 supõe existência: ele trata explicitamente a lista vazia como disputa bloqueante. A consulta pega a primeira disputa aberta do chamado, lê todas as perguntas e retorna a disputa quando `not perguntas` **ou** `fio_vivo(perguntas)`. [app/chamados/juizes.py:2308](app/chamados/juizes.py:2308), [app/chamados/juizes.py:2316](app/chamados/juizes.py:2316), [app/chamados/juizes.py:2319](app/chamados/juizes.py:2319)

| Leitor | Resultado relevante |
|---|---|
| Guarda do encerramento do chamado | Usa S127 e levanta `ValueError` inclusive para disputa vazia; a mensagem manda cobrar resposta/validação e diz que a disputa fecha quando colaborador responde e supervisão valida. [app/chamados/models.py:148](app/chamados/models.py:148), [app/chamados/models.py:153](app/chamados/models.py:153), [app/chamados/models.py:155](app/chamados/models.py:155) |
| `respostas_sem_veredito` / `pode_fechar` | Sobre zero P, a lista de respostas fica vazia e `fio_vivo([])` é `False`, portanto `pode_fechar()` devolve `True`. [app/chamados/models.py:1842](app/chamados/models.py:1842), [app/chamados/models.py:1853](app/chamados/models.py:1853), [app/chamados/juizes.py:1425](app/chamados/juizes.py:1425), [app/chamados/juizes.py:1434](app/chamados/juizes.py:1434) |
| `DisputaSupervisao.fechar` | Distingue explicitamente zero P de P existentes sem viva: zero recebe `FECHOU_VAZIA`; demais recebem `FECHOU_VEREDITO`. [app/chamados/models.py:1895](app/chamados/models.py:1895), [app/chamados/models.py:1903](app/chamados/models.py:1903) |
| Relatório de pendências | Zero P vira categoria técnica `SEM_PERGUNTA`; P existentes todas mortas viram `PRONTA_FECHAR`. [app/chamados/relatorios_disputa.py:6](app/chamados/relatorios_disputa.py:6), [app/chamados/relatorios_disputa.py:11](app/chamados/relatorios_disputa.py:11), [app/chamados/relatorios_disputa.py:18](app/chamados/relatorios_disputa.py:18) |
| Consulta do app | Parte de `PerguntaDisputa`, filtra perguntas vivas e separa pendentes/histórico; uma D sem P não produz item nessa consulta. [app/chamados/services/pergunta_no_app.py:45](app/chamados/services/pergunta_no_app.py:45), [app/chamados/services/pergunta_no_app.py:49](app/chamados/services/pergunta_no_app.py:49), [app/chamados/services/pergunta_no_app.py:69](app/chamados/services/pergunta_no_app.py:69) |
| API `/disputa/perguntas/` | Deriva `em_disputa` de `bool(perguntas)` e `disputa_id` da primeira P apenas se houver P; não deriva esses campos da existência de D aberta. [app/api/views.py:2560](app/api/views.py:2560), [app/api/views.py:2590](app/api/views.py:2590), [app/api/views.py:2592](app/api/views.py:2592) |
| Montagem do fio | Carrega D separadamente, carrega P por outra query e usa `[]` se nenhuma; nesse caso marca `todas_respondidas=True` e `pode_solicitar_correcao=False`. [app/chamados/services/fio.py:184](app/chamados/services/fio.py:184), [app/chamados/services/fio.py:225](app/chamados/services/fio.py:225), [app/chamados/services/fio.py:253](app/chamados/services/fio.py:253), [app/chamados/services/fio.py:339](app/chamados/services/fio.py:339), [app/chamados/services/fio.py:344](app/chamados/services/fio.py:344) |
| Juiz do dia do chamado | Usa datas das P das D; se nenhuma fornece data, retorna `None`. [app/chamados/juizes.py:56](app/chamados/juizes.py:56), [app/chamados/juizes.py:74](app/chamados/juizes.py:74), [app/chamados/juizes.py:84](app/chamados/juizes.py:84) |
| `perguntas_vivas_de` | Retorna lista vazia normalmente. [app/chamados/juizes.py:1420](app/chamados/juizes.py:1420) |
| Classificação de beco | D aberta sem P e com pai terminal é `FECHAVEL`, pois a lista `vis` fica vazia e chama `pode_fechar()`. [app/chamados/services/becos.py:25](app/chamados/services/becos.py:25), [app/chamados/services/becos.py:29](app/chamados/services/becos.py:29), [app/chamados/services/becos.py:33](app/chamados/services/becos.py:33) |
| Gatilho do beco | É `post_save` de **PerguntaDisputa**; chama fechamento do beco em `on_commit`. D que nasce sem nenhuma P não produz por esse nascimento o evento desse receiver. [app/chamados/signals.py:227](app/chamados/signals.py:227), [app/chamados/signals.py:233](app/chamados/signals.py:233), [app/chamados/signals.py:238](app/chamados/signals.py:238) |
| Resolver perguntas por chamado resolvido | No ramo sem credor, zero P provoca `continue` em `_abertas`; posteriormente há um laço separado que fecha D aberta se `pode_fechar()`. [app/chamados/services/disputa_emissao.py:1912](app/chamados/services/disputa_emissao.py:1912), [app/chamados/services/disputa_emissao.py:1916](app/chamados/services/disputa_emissao.py:1916), [app/chamados/services/disputa_emissao.py:1951](app/chamados/services/disputa_emissao.py:1951), [app/chamados/services/disputa_emissao.py:1963](app/chamados/services/disputa_emissao.py:1963) |
| `_disputa_viva` do arquivamento | Retorna D somente quando `any(p.viva...)`; vazio retorna `None`. [app/chamados/management/commands/arquivar_competencia_encerrada.py:148](app/chamados/management/commands/arquivar_competencia_encerrada.py:148), [app/chamados/management/commands/arquivar_competencia_encerrada.py:158](app/chamados/management/commands/arquivar_competencia_encerrada.py:158) |

**5 — Proteções existentes e limites**

- A classe D declara `Meta` com índice `(colaborador, fechada_em)`, sem constraint de existência de P; sua migração de criação também só declara esse índice. A FK é no sentido **P → D**, obrigatória para P, com `CASCADE`: isso não exige que D tenha P. [app/chamados/models.py:1801](app/chamados/models.py:1801), [app/chamados/migrations/0013_disputasupervisao.py:18](app/chamados/migrations/0013_disputasupervisao.py:18), [app/chamados/migrations/0013_disputasupervisao.py:30](app/chamados/migrations/0013_disputasupervisao.py:30), [app/chamados/models.py:2416](app/chamados/models.py:2416)
- Os índices únicos atuais exigem unicidade das **P existentes** por marco/episódio; não exigem quantidade mínima de P por D. [app/chamados/models.py:3481](app/chamados/models.py:3481), [app/chamados/migrations/0053_pergunta_unica_por_familia.py:57](app/chamados/migrations/0053_pergunta_unica_por_familia.py:57)
- O `save` da P torna **P + EventoCelula** atômicos e deriva a identidade da P; esse código só roda quando se salva uma P, sem impor nascimento conjunto de D e P. [app/chamados/models.py:3413](app/chamados/models.py:3413), [app/chamados/models.py:3420](app/chamados/models.py:3420)
- Os três receivers de `post_save` de D tratam fechamento; não geram P no nascimento: o primeiro retorna se `created`, e os outros dois retornam com `raw or created or not instance.fechada_em`. [app/chamados/signals.py:18](app/chamados/signals.py:18), [app/chamados/signals.py:28](app/chamados/signals.py:28), [app/chamados/signals.py:120](app/chamados/signals.py:120), [app/chamados/signals.py:129](app/chamados/signals.py:129), [app/chamados/signals.py:147](app/chamados/signals.py:147), [app/chamados/signals.py:153](app/chamados/signals.py:153)

Existe **detector**, não bloqueio de nascimento:

- `auditar_disputas` percorre D abertas, compara motivos das P e motivos da D, e acusa `motivo_sem_pergunta` quando o motivo existe no catálogo e `texto_pergunta_com_data(...) is not None`. [app/chamados/management/commands/auditar_disputas.py:161](app/chamados/management/commands/auditar_disputas.py:161), [app/chamados/management/commands/auditar_disputas.py:182](app/chamados/management/commands/auditar_disputas.py:182), [app/chamados/management/commands/auditar_disputas.py:188](app/chamados/management/commands/auditar_disputas.py:188)
- Para `batida_fora_escala_grave`, a pergunta do catálogo é estática, sem `{data}`; o helper devolve o texto literal sem depender do contexto. Portanto, uma D aberta que contenha esse motivo e não tenha P correspondente satisfaz o detector. [app/chamados/catalogo/perguntas.py:81](app/chamados/catalogo/perguntas.py:81), [app/chamados/catalogo/query.py:52](app/chamados/catalogo/query.py:52), [app/chamados/catalogo/query.py:56](app/chamados/catalogo/query.py:56), [app/chamados/management/commands/auditar_disputas.py:188](app/chamados/management/commands/auditar_disputas.py:188)
- O comando relata os resultados; se executou e se alguém leu o alerta referente às sete D: **[nao verificado]**. [app/chamados/management/commands/auditar_disputas.py:221](app/chamados/management/commands/auditar_disputas.py:221), [app/chamados/management/commands/auditar_disputas.py:233](app/chamados/management/commands/auditar_disputas.py:233)
- Seu contador `disputa_aberta_vazia` significa **motivos=[]**, não zero P. Com motivo grave preservado, esse contador específico não acusa. [app/chamados/management/commands/auditar_disputas.py:169](app/chamados/management/commands/auditar_disputas.py:169), [app/chamados/management/commands/auditar_disputas.py:171](app/chamados/management/commands/auditar_disputas.py:171)
- Outro contador, `vigia_fio.disputa_vazia`, só conta D com `motivo_fechamento=FECHOU_VAZIA`; não conta as abertas sem P. [app/chamados/services/vigia_fio.py:727](app/chamados/services/vigia_fio.py:727), [app/chamados/services/vigia_fio.py:734](app/chamados/services/vigia_fio.py:734)

Os testes localizados não impõem a regra global pedida; alguns aceitam explicitamente zero P: [nao verificado]

- S127 testa que D sem P bloqueia o encerramento, sem impedir seu nascimento. [app/chamados/tests/test_disputa_s127.py:46](app/chamados/tests/test_disputa_s127.py:46)
- C7 cria D vazia, fecha pela porta, exige motivo `vazia` e incremento do contador. [app/chamados/tests/test_c7_red_minimos.py:169](app/chamados/tests/test_c7_red_minimos.py:169), [app/chamados/tests/test_c7_red_minimos.py:174](app/chamados/tests/test_c7_red_minimos.py:174)
- `DisputaMudaSemAtaTest` exige que criação a partir de chamado órfão resulte em D **aberta com zero perguntas** quando não há ata. [app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51](app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51), [app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58](app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58)
- INV2 exige que pergunta com `{data}` não nasça quando faltar a data no contexto; INV4/5 verificam recalibração de posto/âncora e preparam a fixture criando D e chamando o gerador separadamente. [app/chamados/tests/test_invariantes_disputa.py:81](app/chamados/tests/test_invariantes_disputa.py:81), [app/chamados/tests/test_invariantes_disputa.py:90](app/chamados/tests/test_invariantes_disputa.py:90), [app/chamados/tests/test_invariantes_disputa.py:127](app/chamados/tests/test_invariantes_disputa.py:127), [app/chamados/tests/test_invariantes_disputa.py:134](app/chamados/tests/test_invariantes_disputa.py:134)

A existência de constraint ou trigger **instalado em prod** além do que consta nesta cópia é **[nao verificado]**.

## 10/11 14:59 · sessao 4d8d5d0b2654 · - · - · origem=Ronald (tmux)

Censo Q2, somente leitura. Separe **zero linhas `PerguntaDisputa`** de **zero perguntas vivas**: os fechamentos abaixo conservam a linha e o vínculo com a disputa; o estado é derivado de carimbos/juiz, não há coluna `status` da pergunta nesse trecho do modelo. (`app/chamados/models.py:3033`, `:3041`, `:2416`)

**Remoção física e desvinculação da disputa**

- Não encontrei escritor produtivo que apague `PerguntaDisputa` ou atribua `None`/outra disputa à FK `disputa`; a busca incluiu o censo AST existente, imports com alias e busca complementar por atribuições/SQL. Exaustividade para escrita dinâmica impossível de resolver estaticamente: **[nao verificado]**. O censo reconhece aliases de import em `app/core/censo_escritas.py:281` e `:285`.
- `PerguntaDisputa.disputa` é obrigatória e `CASCADE`: excluir a própria disputa também remove suas perguntas, sem conservar uma disputa aberta. (`app/chamados/models.py:2416`)
- `DecisaoPergunta.pergunta` usa `PROTECT`, impedindo excluir pergunta que tenha decisão relacionada. (`app/chamados/models.py:3871`)
- `celula` e `chamado_destino` usam `SET_NULL`, mas sua nulificação conserva a pergunta e sua FK `disputa`. (`app/chamados/models.py:2414`, `:3385`)
- `fio.reapontar_resolvedora` aceita destino `None`, valida em `:907`, atribui em `:925` e salva em `:927`, no `atomic(savepoint=False)` iniciado em `:881`. A reversão de `reatar_beco_folga_ao_limbo` passa `.first()` do destino anterior, podendo obter `None`: `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`. Isso desvincula a âncora do chamado, **não remove a relação disputa→pergunta**. (`app/chamados/services/fio.py:925`)

**Retratação de motivo**

- `DisputaSupervisao.remover_motivo` hoje conserva cada pergunta e registra retratação, depois carimba `VIA_MOTIVO_RETRATADO`; não executa o `delete` histórico referido no comentário. (`app/chamados/models.py:2046`, `:2054`, `:2056`, `:2061`)
- O ato de retratar perguntas e salvar motivos/contexto ocorre no mesmo `atomic` (`:2025`–`:2065`). Havendo motivos restantes, retorna com disputa aberta (`:2066`); sem restantes, só depois do bloco chama `fechar` (`:2072`). Assim, exceção na última chamada pode deixar **perguntas retratadas e disputa aberta**, mas as linhas permanecem. (`app/chamados/models.py:2025`, `:2066`, `:2072`)
- Call sites produtivos encontrados: reativação de posto remove exclusivamente `posto_desativado`; calibração de âncora remove exclusivamente `ancora_ausente`. Não retiram `batida_fora_escala_grave`. (`app/colaboradores/signals.py:77`, `app/escala/signals.py:161`)

**Retratação por duplicidade**

- `fio.retratar_duplicata` carimba `VIA_DUPLICATA` e motivo indicando canônica, preservando pergunta e resposta. (`app/chamados/services/fio.py:1345`, `:1359`)
- Seu chamador é `retratar_para_canonica`, que trata N retratações no mesmo `atomic` e trava as linhas antes. (`app/chamados/services/fio.py:1336`, `:1338`, `:1341`)
- Chamadores dessa porta: reapontamento que também data a pergunta (`fio.py:921`), migração de resolvedoras (`fio.py:2180`) e aplicação do backfill (`backfill_decisoes_pergunta.py:331`). O comando de dedup usa a mesma aplicação do backfill. (`app/chamados/services/fio.py:921`, `:2180`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:331`; `app/chamados/management/commands/dedup_perguntas_fato.py:105`)
- A migração tem `atomic` externo e pode retratar a única pergunta viva de uma disputa sem fechar diretamente aquela disputa; continua existindo sua linha retratada. (`app/chamados/services/fio.py:2083`, `:2180`)
- `duplicata` e `motivo_retratado` são terminais e vencem inclusive resposta prévia na leitura de vitalidade. (`app/chamados/models.py:2516`, `:2524`)

**Todos os call sites produtivos diretos de carimbo/fechamento encontrados**

| Sítio | Ato e relação com fecho da disputa |
|---|---|
| `app/chamados/models.py:3064` | `fechar_por_admin` → `admin_decidiu`; método não fecha a disputa. |
| `app/chamados/models.py:1921` | Chama `fechar_por_admin` ao fechar disputa por desligamento/admin; pergunta e disputa fecham no mesmo `atomic` (`:1905`, `:1932`). |
| `app/ponto/services/veredito_celula.py:189` | Fecha perguntas do dia por `admin_decidiu`, depois reconcilia chamados (`:201`), sem chamada direta para fechar disputa. Chamadores assumem `atomic` e travam primeiro (`:173`); `veredito_celula` abre o seu em `:223`. |
| `app/chamados/services/validacao.py:289`, `:329` | Aceite de folga chama `fechar_por_admin`, em `atomic` (`:281`, `:314`); chamado do fio do dia em `:296`; recusa da declaração faz rollback (`:343`). |
| `app/chamados/models.py:2061` | Retratação de motivo, detalhada acima. |
| `app/chamados/services/fio.py:1359` | Retratação de duplicata, detalhada acima. |
| `app/chamados/services/materializacao.py:924` | `paridade_recusada`: guarda exige marco no espelho; sem ele retorna sem este carimbo (`:900`–`:916`). Conserva linha. |
| `app/chamados/services/materializacao.py:1099` | `fato_ocupado` quando recusa traduzida é `COLISAO` e ainda não materializada (`:1093`); ocorre inclusive no `finally` da unidade (`:1297`), depois de seu bloco de tentativa. Conserva linha. |
| `app/chamados/services/materializacao.py:561` | Absorção de irmãs por ausência, via declaração ou ausência (`:549`), com carimbo+decisão no mesmo `atomic` (`:557`). Reconciliar destino e tentativa de fechar disputa vêm depois (`:581`, `:585`). |
| `app/chamados/services/materializacao.py:1277` | Materialização de uma pergunta: declaração ou via do fato; recusa do C6 levanta `_Reprovada`, desfazendo plantio; exceções entram em erros (`:1279`, `:1283`, `:1285`). Fecho da disputa fica na cauda própria (`:1302`, `:1316`, `:1333`). |
| `app/chamados/services/materializacao.py:1431` | S144: `juiz_aprovou` só depois de lastro verdadeiro, ignorando P em voo (`:1425`, `:1427`); não fecha disputa nesse método, que pode ser acionado pelo fecho dela. |
| `app/chamados/reconciliador.py:405` | `sem_turno_aberto`, só se C6 já considera morta (`:397`); depois tenta fechar disputa se `pode_fechar` (`:411`). |
| `app/chamados/reconciliador.py:595` | `celula_absolveu`, cada carimbo com seu `atomic` (`:592`); fecho de disputas é posterior (`:608`), com exceção de fecho engolida/logada (`:615`). Pode conservar disputa aberta com perguntas mortas, não sem linhas. |
| `app/chamados/reconciliador.py:1147` | Fantasma pela lâmpada; cada carimbo+evento com `atomic` (`:1146`). Fecha disputas depois (`:1158`); resposta sem veredito barra (`:1163`), exceção de fecho é logada e adicionada às falhas (`:1173`). |
| `app/chamados/models.py:3287` | Porta `encerrar_fantasma` → `carimbar(VIA_FANTASMA)`; não remove linha. |
| `app/chamados/services/disputa_emissao.py:1863` | S128 carimba via do fato para destino resolvido; ignora em voo, vivas, trancadas e resposta sem testemunha (`:1837`, `:1843`, `:1847`, `:1856`). |
| `app/chamados/services/disputa_emissao.py:1937` | S128 sem credor vivo; só carimba perguntas que C6 considera mortas (`:1925`). Fecho da disputa ocorre no mesmo ato externo se não há P em voo e `pode_fechar` (`:1961`, `:1963`); wrapper fornece atomic quando necessário (`:1796`). |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | Troca via para `fato_materializado` pela porta `models.py:3268`, somente carimbadas sem história e com marco aceso (`:31`, `:40`, `:41`). Cada pergunta tem atomic (`:67`); disputas fecham num laço posterior (`:80`, `:87`, `:88`). |

A validação também mata a vitalidade por `validada_em`: escritor central `_carimbar_validacao` em `app/chamados/services/validacao.py:134`, `:137`; call sites em `:288`, `:320`, `:435`. O fluxo normal tem `atomic` envolvendo validação+decisão da porta (`:425`) e devolve a validação quando a materialização recusa (`:461`–`:466`); depois materializa a disputa inteira e sua cauda decide o fecho (`:469`; `app/chamados/services/materializacao.py:1316`). Não exclui a pergunta.

**Mecanismo automático não é garantia atômica de fechar disputa junto com pergunta**

Todo `save` de pergunta agenda o beco por `on_commit`; o callback usa `engolir`, portanto sua falha não desfaz o carimbo já confirmado. O beco só age se pai terminal e sem perguntas visíveis, e ainda consulta `pode_fechar`. (`app/chamados/signals.py:227`, `:235`, `:238`; `app/chamados/services/becos.py:29`, `:31`, `:33`, `:74`)

Conclusão aplicável ao fato: os sítios acima explicam **disputa aberta com perguntas encerradas/retratadas**, mas conservam suas linhas. Atribuir qualquer um deles às sete disputas com **zero perguntas** é **[nao verificado]**.

## 10/11 14:57 · sessao a97568efbc2e · - · - · origem=Ronald (tmux)

Encontrei uma lacuna concreta no selo AST: `api/tests/test_l030_selo_porta.py:27`–`:29` reconhece apenas nomes/atributos. Uma guarda como `if not request.data.get('latitude'): return Response({}, status=403)` escapa, porque `latitude` aparece como `ast.Constant`. Incluir chaves literais de `.get`, subscritos e `getattr`, com mutante específico, fecha essa evasão.

Não encontrei outro defeito concreto nos quatro testes novos por leitura. O spy delega ao escritor real (`api/tests/test_l030_batida_pela_porta.py:36`); a matriz usa os dois URLconfs (`:50`); o loader locmem compõe os filhos com cada casca real (`ponto/tests/test_l030_templates_renderizados.py:15`, `:21`); os mutantes JS cobrem os quatro cortes GPS removidos (`ponto/tests/test_l030_forma_js.py:75`).

Execução Django, renderização real e lint: **[nao verificado]**.

## 10/11 14:50 · sessao a97568efbc2e · - · - · origem=Ronald (tmux)

Censo estático confirmado; nada escrito ou executado contra banco/rede.

| Campo/caminho | Escritor e leitores confirmados |
|---|---|
| `Batida.latitude/longitude/gps_accuracy` | A API lê campos independentes e os passa à porta: `app/api/views_core.py:420`, `:421`, `:422`, `:687`; gêmeo `app/api/views.py:433`, `:434`, `:435`, `:690`. A porta sanitiza cada campo separadamente (`app/ponto/registro_batida.py:8`) e cria (`:157`). Campos aceitam nulos (`app/ponto/models.py:58`). |
| Web clássico | Já passa os campos à mesma porta sem guarda nesse trecho: `app/ponto/services/triagem_batida.py:700`, `:707`. |
| Ausência de geo | `verificar_geofence` escreve `AlertaGeofence.tipo='sem_gps'`, coordenadas nulas, havendo posto: `app/ponto/services/geofence.py:247`, `:249`, `:252`. Depois publica e abre/anexa ocorrência existente: `:309`, `:322`, `:335`. Sem posto, não cria esse alerta (`:248`). |
| Mapas e contagem | Detalhe exclui ambos os nulos antes de converter (`app/colaboradores/services/detalhe.py:22`, `:53`, `:80`); fio também (`app/chamados/views.py:692`). Situacional propaga coordenadas+accuracy (`app/colaboradores/services/situacional.py:137`), painel também (`app/colaboradores/painel_cell.py:37`); pino distingue explicitamente `None` (`app/colaboradores/views.py:2474`). |
| Outros leitores de nulos | Dashboard serializa nulos (`app/core/views.py:1024`); offline conta apenas latitude presente (`app/ponto/views.py:485`); relatório mostra ausência (`app/templates/chamados/partials/_relatorio_geo.html:30`); validade de alerta conserva ausência (`app/ponto/models.py:263`); reconciliação de geo filtra os dois nulos (`app/ponto/services/geofence.py:403`). |
| Vínculo | Reconciliação aceita apenas coordenadas ambas presentes e não julga cadastro não ativo (`app/ponto/services/geofence.py:46`, `:51`). Detector da lista filtra somente latitude+accuracy (`app/ponto/services/vinculo_na_lista.py:49`) e faz `float(lon)` (`:56`). |

**Riscos que devem constar na entrega:**

- A API **pode passar só uma coordenada**. A remoção da guarda permite alcançar a porta com latitude preenchida e longitude ausente; não existe normalização do par na leitura/passagem citadas acima. Se accuracy também vier preenchida, o detector chega a `float(None)`. Caminho estático comprovado; ocorrência em produção **[nao verificado]**. `app/api/views_core.py:420`, `:421`, `:687`; `app/ponto/registro_batida.py:8`; `app/ponto/services/vinculo_na_lista.py:49`, `:56`.
- `0.0` é preservado pelo escritor, pois não é `''` nem `None`; porém **alguns leitores equiparam zero à ausência**: geofence (`app/ponto/services/geofence.py:247`), drawer (`app/colaboradores/services/drawer.py:63`), dashboard (`app/templates/core/dashboard2.html:242`, `:287`) e contexto de ocorrência geo (`app/ponto/services/geofence.py:327`). Detalhe e pino distinguem `None` de zero nos pontos acima. Não alterar esses leitores nesta obra.
- A API não grava uma fotografia histórica de `situacao` em `Batida`; usa o colaborador associado e passa os campos listados à porta (`app/api/views_core.py:681`, `:687`; `app/ponto/models.py:58`). `situacao` do cadastro é mutável: desligamento escreve `desligado` (`app/colaboradores/services/desligamento.py:104`) e reativação escreve `ativo` (`:210`), persistindo pela porta `salvar_inteiro` (`app/colaboradores/services/colaborador.py:73`).

**Pós-escritor para desligado/férias:**

- Núcleo recebe toda Batida e recompõe dias do timestamp; exceções são capturadas (`app/ponto/signals.py:19`, `:29`, `:34`).
- Cartório recebe toda Batida e julga os dias derivados do timestamp, dentro de `engolir` (`app/ponto/signals.py:85`, `:108`, `:131`). A emissão de cobrança já suprime desligados pelo cadastro atual (`app/ponto/services/cartorio.py:1708`, `:1773`).
- Reconciliador recebe toda Batida e captura exceções (`app/ponto/signals.py:206`); um ramo de ressuscitação já exige cadastro `ativo` (`app/chamados/reconciliador.py:820`).
- Recalcular por evento é acionado depois do create, captura exceções e agenda por `on_commit`; competência vem do instante do fato (`app/ponto/registro_batida.py:157`, `:160`, `:175`; `app/ponto/services/fechamento.py:1798`, `:1809`, `:1824`).
- Geofence não exige situação ativa no handler: lê posto e geo (`app/ponto/services/geofence.py:232`, `:236`, `:247`). Sua reconciliação de vínculo, separadamente, exige situação atual ativa (`:46`).
- Férias já são detectadas pelo agendamento que cobre **hoje**, via juiz existente, e seguem como anomalia (`app/chamados/detector_anomalias.py:113`; `app/ponto/utils.py:85`, `:88`). Afastamento também pergunta ao juiz do dia, usando cadastro apenas para divergência (`app/ponto/services/afastado_avisa.py:38`).
- Não encontrei, nesses handlers examinados, uma premissa de que desligado jamais possa chegar que force exceção antes/depois do create. Comportamento completo executado dos efeitos e demais transitivos **[nao verificado]**.

## 10/11 14:43 · sessao 753a9802ce09 · URGENCIA-E4B · sonda · origem=Code

Base: saída raspada fornecida. Nova execução de `sonda4.py`, role leitor, ordem (c)→(a)→(b) e rollback: [nao verificado]. As referências abaixo sustentam as regras do código; as quantidades são exclusivamente do dado fornecido [nao verificado].

1) **ESCOPO e B_FECHA**, no retrato fornecido [nao verificado]:

| Medida | Atual | Base sonda3 informada | Delta |
|---|---:|---:|---:|
| Validar + Decidir | **1.214** | 1.215 | **−1** |
| (b) | **263** | 263 | **0** |
| Soma dos baldes MOTIVO_B | **263** | — | — |

O delta de escopo usa a base solicitada; o registro `ESCOPO` nomeia essa base como `sonda2`, não `sonda3` [nao verificado]. A definição de Validar+Decidir está em `app/chamados/services/verbos.py:22`.

2) **MOTIVO_B**: combinações exclusivas por destino; cada item pertence a um único balde, conforme `B_FECHA` fornecido [nao verificado].

| Combinação | GLOBAL | Empresa 2 | Empresa 3 | Empresa 4 | Regra |
|---|---:|---:|---:|---:|---|
| resolvido:156(disputa_com_pergunta_viva) + fechado:156(disputa_com_pergunta_viva) | 108 | 83 | 21 | 4 | `app/chamados/models.py:156`; `app/chamados/juizes.py:2320` |
| resolvido:156(disputa_sem_pergunta) + fechado:156(disputa_sem_pergunta) | 6 | 4 | 2 | 0 | `app/chamados/models.py:156`; `app/chamados/juizes.py:2320` |
| resolvido:168 + fechado:139 | 2 | 2 | 0 | 0 | `app/chamados/models.py:168`; `app/chamados/models.py:139` |
| resolvido:168 + fechado:168 | 139 | 106 | 31 | 2 | `app/chamados/models.py:168` |
| resolvido:192 + fechado:139 | 8 | 6 | 2 | 0 | `app/chamados/models.py:192`; `app/chamados/models.py:139` |
| sem_caixa | 0 | 0 | 0 | 0 | `app/templates/chamados/partials/modal_fio.html:650` |
| **Soma = (b)** | **263** | **201** | **56** | **6** | Saída `B_FECHA` [nao verificado] |

Os zeros representam ausência de registros desses baldes na saída fornecida, cuja soma fecha com (b) [nao verificado].

3) **INCIDENCIA_LINHA**: itens em que a linha aparece, uma vez por item, mesmo quando aparece nos dois destinos; sem somar linhas [nao verificado].

| Linha de recusa | GLOBAL | Empresa 2 | Empresa 3 | Empresa 4 |
|---|---:|---:|---:|---:|
| `app/chamados/models.py:134` | 0 | 0 | 0 | 0 |
| `app/chamados/models.py:139` | 10 | 8 | 2 | 0 |
| `app/chamados/models.py:156` | 114 | 87 | 23 | 4 |
| `app/chamados/models.py:168` | 141 | 108 | 31 | 2 |
| `app/chamados/models.py:192` | 8 | 6 | 2 | 0 |

4) **Agregado do aval**, incluindo todas as categorias enumeradas [nao verificado]:

| Categoria | GLOBAL | Empresa 2 | Empresa 3 | Empresa 4 | Regra |
|---|---:|---:|---:|---:|---|
| :139 justificativa pendente | 10 | 8 | 2 | 0 | `app/chamados/models.py:139` |
| :156 disputa SEM pergunta | 6 | 4 | 2 | 0 | `app/chamados/juizes.py:2320` |
| :156 disputa com pergunta VIVA | 108 | 83 | 21 | 4 | `app/chamados/juizes.py:2320` |
| :168 pergunta viva ancorada | 141 | 108 | 31 | 2 | `app/chamados/models.py:168` |
| :192 premissa do furo | 8 | 6 | 2 | 0 | `app/chamados/models.py:192` |
| Formulário ausente/sem permissão ou GLPI só followup | 0 | 0 | 0 | 0 | `app/templates/chamados/partials/modal_fio.html:650`; GLPI só followup [nao verificado] |
| Outras | 0 | 0 | 0 | 0 | Saída fornecida [nao verificado] |

Regra adotada no agregado: **incidência por categoria**, uma vez por item em cada categoria presente. Assim, `resolvido:192 + fechado:139` entra em :192 e :139; `resolvido:168 + fechado:168` entra uma vez em :168. Esse agregado admite sobreposição; quem fecha **263** é a tabela de combinações MOTIVO_B [nao verificado].

## 10/11 13:45 · sessao 4cca0a32929a · URGENCIA-E4B · sonda · origem=Code

**ESCOPO: 1.215 itens em Validar+Decidir; delta de −9 sobre os 1.224 da sonda2**, conforme o registro ESCOPO fornecido, com referências declaradas em `app/chamados/services/painel.py:198` e `app/chamados/services/verbos.py:30`.

Consolidação da saída raspada de **10/11/2026, 13:42:56 −03:00**. Nova execução local de `sonda3.py`: **[nao verificado]**, arquivo não localizado na cópia. O registro `ROLLBACK_CONFIRMADO` fornecido também traz fonte **[nao verificado]**.

O papel informado é **gestor geral simulado**; permissões de um usuário real de produção: **[nao verificado]**. Referências declaradas: `app/chamados/utils.py:47` e `app/colaboradores/mixins.py:163`.

**Tabelas**

Valores transcritos dos registros CELULA/TOTAL_EMPRESA/TOTAL_UNIVERSO fornecidos. Referências declaradas para todas as linhas: `app/chamados/services/painel.py:198`, `app/chamados/services/verbos.py:30`, `app/templates/chamados/partials/modal_fio.html:650`, `app/chamados/models.py:128` e `app/ponto/management/commands/e6_oraculo.py:182`.

Legenda: **(a)** tem porta e a função real aceita; **(b)** sem porta ou recusada; **(c)** dono CADASTRO/BATIDA. A coluna **L-099 [nao verificado]** reproduz `l099_nao_verificado`; indeterminação não vira ESTRUTURA (`app/ponto/management/commands/e6_oraculo.py:193`).

**Empresa 2** — referências das linhas acima.

| Fonte | Total | (a) | (b) | (c) | Cadastro | Batida | L-099 [nao verificado] |
|---|---:|---:|---:|---:|---:|---:|---:|
| batida_ausente | 400 | 90 | 105 | 205 | 44 | 161 | 75 |
| disputa_supervisao | 130 | 1 | 77 | 52 | 23 | 29 | 27 |
| chamados | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| gps_fora_geofence | 81 | 52 | 0 | 29 | 21 | 8 | 0 |
| turno_aberto | 50 | 3 | 9 | 38 | 10 | 28 | 3 |
| sem_previsao_escala | 53 | 11 | 0 | 42 | 41 | 1 | 4 |
| sem vinculo | 129 | 129 | 0 | 0 | 0 | 0 | 129 |
| outras | 54 | 16 | 10 | 28 | 10 | 18 | 9 |
| **Total** | **897** | **302** | **201** | **394** | **149** | **245** | **247** |

**Empresa 3** — mesmas referências declaradas.

| Fonte | Total | (a) | (b) | (c) | Cadastro | Batida | L-099 [nao verificado] |
|---|---:|---:|---:|---:|---:|---:|---:|
| batida_ausente | 113 | 33 | 30 | 50 | 16 | 34 | 18 |
| disputa_supervisao | 28 | 0 | 19 | 9 | 4 | 5 | 2 |
| chamados | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| gps_fora_geofence | 37 | 26 | 0 | 11 | 6 | 5 | 0 |
| turno_aberto | 23 | 0 | 3 | 20 | 2 | 18 | 0 |
| sem_previsao_escala | 14 | 0 | 0 | 14 | 12 | 2 | 0 |
| sem vinculo | 44 | 44 | 0 | 0 | 0 | 0 | 44 |
| outras | 12 | 3 | 4 | 5 | 0 | 5 | 2 |
| **Total** | **271** | **106** | **56** | **109** | **40** | **69** | **66** |

**Empresa 4** — mesmas referências declaradas.

| Fonte | Total | (a) | (b) | (c) | Cadastro | Batida | L-099 [nao verificado] |
|---|---:|---:|---:|---:|---:|---:|---:|
| batida_ausente | 23 | 11 | 2 | 10 | 0 | 10 | 6 |
| disputa_supervisao | 5 | 0 | 3 | 2 | 0 | 2 | 0 |
| chamados | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| gps_fora_geofence | 5 | 2 | 0 | 3 | 0 | 3 | 0 |
| turno_aberto | 3 | 0 | 0 | 3 | 0 | 3 | 0 |
| sem_previsao_escala | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| sem vinculo | 7 | 7 | 0 | 0 | 0 | 0 | 7 |
| outras | 4 | 3 | 1 | 0 | 0 | 0 | 3 |
| **Total** | **47** | **23** | **6** | **18** | **0** | **18** | **16** |

**Geral** — registro TOTAL_UNIVERSO, mesmas referências declaradas.

| Total | (a) | (b) | (c) | Cadastro | Batida | L-099 [nao verificado] |
|---:|---:|---:|---:|---:|---:|---:|
| **1.215** | **431** | **263** | **521** | **189** | **332** | **329** |

“Sem vinculo” é o balde em que `dia_do_fato` retorna `None`, conforme ESCOPO; não comprova ausência de vínculo cadastral (`app/chamados/juizes.py:87`). Portas escritoras alternativas não promovem item a (a): aceite sem escrita permanece **[nao verificado]**, conforme limite declarado em `app/chamados/services/validacao.py:435` e `app/chamados/services/validacao.py:490`.

**Exemplos de cada linha com (b)**

As siglas abaixo identificam os motivos e as linhas de recusa presentes nos exemplos fornecidos:

| Sigla | Motivo / destino recusado | Linha |
|---|---|---|
| P | Pergunta viva ancorada na competência aberta; `resolvido` e `fechado` | `app/chamados/models.py:168` |
| D | Disputa aberta sem perguntas ou com pergunta viva; `resolvido` e `fechado` | `app/chamados/models.py:156` |
| F/J | Premissa do furo ainda não regularizada → `resolvido`; justificativa relacionada ainda pendente → `fechado` | `app/chamados/models.py:192`; `app/chamados/models.py:139` |

Cada par abaixo é **chamado pk / col**, transcrito de `exemplos_b`; a sigla remete à linha da recusa acima.

| Empresa | Fonte | Exemplos e recusa |
|---|---|---|
| 2 | batida_ausente | 25810 / col964 — P; 25954 / col826 — P; 26314 / col827 — P (`app/chamados/models.py:168`) |
| 2 | disputa_supervisao | 6443 / col841 — D; 7548 / col849 — D; 9074 / col374 — D (`app/chamados/models.py:156`) |
| 2 | turno_aberto | 28571 / col360 — P (`app/chamados/models.py:168`); 35582 / col949 e 36062 / col516 — F/J (`app/chamados/models.py:192`; `app/chamados/models.py:139`) |
| 2 | outras | 5163 / col821 — D; 5251 / col468 — D; 19962 / col366 — D (`app/chamados/models.py:156`) |
| 3 | batida_ausente | 25756 / col169 — P; 27095 / col099 — P; 27922 / col829 — P (`app/chamados/models.py:168`) |
| 3 | disputa_supervisao | 10260 / col118 — D; 14344 / col081 — D; 26634 / col751 — D (`app/chamados/models.py:156`) |
| 3 | turno_aberto | 33961 / col911 — P (`app/chamados/models.py:168`); 37274 / col165 e 37421 / col060 — F/J (`app/chamados/models.py:192`; `app/chamados/models.py:139`) |
| 3 | outras | 22837 / col769 — D; 28584 / col151 — D; 35571 / col163 — D (`app/chamados/models.py:156`) |
| 4 | batida_ausente | 36283 / col032 — P; 36545 / col040 — P (`app/chamados/models.py:168`) |
| 4 | disputa_supervisao | 31775 / col993 — D; 35614 / col040 — D; 38057 / col040 — D (`app/chamados/models.py:156`) |
| 4 | outras | 23248 / col857 — D (`app/chamados/models.py:156`) |

**Contagem agregada dos motivos de (b)**

A distribuição global por motivo está **[nao verificado]**: a saída fornece totais de (b) por célula e exemplos, sem o agregado por recusa.

| Motivo | Contagem global |
|---|---|
| Formulário ausente / sem permissão (`app/templates/chamados/partials/modal_fio.html:650`; `app/templates/chamados/partials/modal_fio.html:651`) | [nao verificado] |
| GLPI só followup | [nao verificado] |
| Justificativa pendente (`app/chamados/models.py:139`) | [nao verificado] |
| Disputa viva (`app/chamados/models.py:156`) | [nao verificado] |
| Pergunta viva ancorada (`app/chamados/models.py:168`) | [nao verificado] |
| Premissa do furo não regularizada (`app/chamados/models.py:192`) | [nao verificado] |
| Outras recusas de `models.py` | [nao verificado] |

## 10/11 12:01 · sessao 5a57ae77d877 · URGENCIA-E4B · leitura · origem=Code

A execução em produção, os novos totais e a identidade dos IDs com o retrato histórico permanecem **[nao verificado]**. O script usa o mesmo critério da sonda2 e exige fechamento em 1.224; se o universo corrente divergir, publica somente `ERRO_SONDA` (`app/chamados/services/painel.py:198`; `app/chamados/services/verbos.py:30`).

Na tabela, **G** significa chamar `transicionar` real nos dois destinos oferecidos, interrompendo antes de `_aplicar`. Assim são executadas as guardas originais, sem entrar no escritor (`app/chamados/models.py:128`; `app/chamados/models.py:199`).

| Fonte | Tela/ação | View → função real | Aceite somente leitura | Sonda chama |
|---|---|---|---|---|
| batida_ausente | Central → fio → Encerrar/arquivar (`app/templates/chamados/partials/modal_fio.html:650`) | `processar_post_fio` → `transicionar` (`app/chamados/services/fio.py:721`) | Prefixo real até `_aplicar` (`app/chamados/models.py:199`) | G |
| disputa_supervisao | Mesma caixa (`app/templates/chamados/partials/modal_fio.html:650`) | Mesma chamada (`app/chamados/services/fio.py:721`) | Mesmo prefixo (`app/chamados/models.py:128`) | G |
| chamados | Mesma caixa, quando local (`app/templates/chamados/partials/modal_fio.html:650`) | Mesma chamada (`app/chamados/services/fio.py:721`) | Mesmo prefixo (`app/chamados/models.py:128`) | G |
| gps_fora_geofence | Mesma caixa (`app/templates/chamados/partials/modal_fio.html:650`) | Mesma chamada (`app/chamados/services/fio.py:721`) | Mesmo prefixo (`app/chamados/models.py:128`) | G |
| turno_aberto | Mesma caixa (`app/templates/chamados/partials/modal_fio.html:650`) | Mesma chamada (`app/chamados/services/fio.py:721`) | Mesmo prefixo (`app/chamados/models.py:128`) | G |
| sem_previsao_escala | Mesma caixa (`app/templates/chamados/partials/modal_fio.html:650`) | Mesma chamada (`app/chamados/services/fio.py:721`) | Mesmo prefixo (`app/chamados/models.py:128`) | G |
| sem vinculo | Mesma caixa; balde significa dia indeterminado (`app/chamados/juizes.py:87`) | Mesma chamada (`app/chamados/services/fio.py:721`) | Mesmo prefixo (`app/chamados/models.py:128`) | G |
| outras | Mesma caixa, sem filtro por módulo (`app/templates/chamados/partials/modal_fio.html:650`) | Mesma chamada (`app/chamados/services/fio.py:721`) | Mesmo prefixo (`app/chamados/models.py:128`) | G |
| Qualquer fonte com ticket GLPI | Fio → detalhe GLPI (`app/templates/chamados/partials/modal_fio.html:680`) | `adicionar_followup` ignora `novo_status` e só envia mensagem (`app/chamados/views.py:292`; `app/chamados/views.py:303`) | Não há conclusão efetiva nesse formulário | **(b), porta faltante** |

As alternativas específicas não fornecem aceite completo de conclusão antes dos efeitos abaixo; não são usadas para promover um item a **(a)**:

| Fonte/alternativa | Função real e limite |
|---|---|
| Perguntas: validar, inline ou lote | A validação carimba antes de materializar e decide o resultado depois; as verificações iniciais não bastam (`app/chamados/services/validacao.py:404`; `app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:490`). |
| Disputa: regularizar | Materializa antes de conhecer o fechamento; `pode_fechar()` responde pela disputa, enquanto concluir o chamado depende do signal (`app/chamados/services/materializacao.py:1376`; `app/chamados/models.py:1848`; `app/chamados/signals.py:42`). |
| GPS: autorizar/recusar | Escreve ocorrência ou retrata batida antes da transição final (`app/chamados/services/acoes_chamado.py:218`; `app/chamados/services/acoes_chamado.py:269`; `app/chamados/services/acoes_chamado.py:277`). |
| sem_previsao_escala: aceitar folga | Grava o template antes de reconciliar os chamados (`app/chamados/services/limbo_folgas.py:91`; `app/chamados/services/limbo_folgas.py:101`). |
| outras: aprovar exceção / ausência | Registra batida antes de transicionar; ausência depende de decisão/evento e reconciliação posteriores (`app/chamados/services/acoes_chamado.py:44`; `app/chamados/services/acoes_chamado.py:70`; `app/ponto/services/ausencia.py:686`). |

`reconciliaria()` também não é chamada: dentro do `atomic` chega a `SELECT FOR UPDATE`, incompatível com esse envelope READ ONLY (`app/chamados/models.py:711`; `app/chamados/models.py:2666`).

O juiz L-099 mora em **`app/ponto/management/commands/e6_oraculo.py:53`**, com assinatura `dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)`. A sonda usa a montagem oficial `InsumosDoDono(..., sem_motor=True, cxr=...).dono(dia)`, que chama esse juiz (`app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/management/commands/e6_oraculo.py:182`).

Dia ausente ou L-099 sem resposta ficam explicitamente **[nao verificado]**, contabilizados em cada linha; não são convertidos em dono ESTRUTURA. O rótulo ESTRUTURA da coluna **(b)** é o balde operacional solicitado no aval (`app/chamados/juizes.py:87`; `app/ponto/management/commands/e6_oraculo.py:193`; `aval_urgencia_revista_1110.txt:1`, anexo fornecido).

```python
# sonda3.py -- proposta; execucao e novos totais: [nao verificado].
# Referencias da copia: app/chamados/services/painel.py:198;
# app/chamados/services/verbos.py:30; app/chamados/models.py:128.
import sys
sys.dont_write_bytecode = True

import contextlib
import hashlib
import inspect
import io
import json
import logging
from collections import Counter
from html.parser import HTMLParser
from pathlib import Path
from types import SimpleNamespace
from unittest.mock import patch

from django.db import connection, connections, transaction
from django.template import Context
from django.template.defaulttags import IfNode
from django.template.loader import get_template
from django.utils import timezone

EMPRESAS = (2, 3, 4)
TOTAL_ESPERADO = 1224
[nome] = (
    "batida_ausente", "disputa_supervisao", "chamados",
    "gps_fora_geofence", "turno_aberto", "sem_previsao_escala",
    "sem vinculo", "outras",
)
PASSO = "inicio"
SAIDA = []
FALHAS_SQL = []
ENGOLIDOS = 0
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
FONTE = (
    "app/chamados/services/painel.py:198; "
    "app/chamados/services/verbos.py:30; "
    "app/templates/chamados/partials/modal_fio.html:650; "
    "app/chamados/models.py:128; "
    "app/ponto/management/commands/e6_oraculo.py:182"
)
USUARIO = SimpleNamespace(
    is_authenticated=True, is_superuser=True, is_staff=True,
)


class FalhaSonda(BaseException):
    pass


class RollbackFinal(BaseException):
    pass


class AceiteAntesDaEscrita(BaseException):
    pass


def exigir(condicao, codigo):
    if not condicao:
        raise FalhaSonda(codigo)


def emitir(tipo, fonte=FONTE, **dados):
    SAIDA.append(dict(tipo=tipo, fonte=fonte, **dados))


def col(pk):
    return None if pk is None else "col%03d" % pk


def erro_seguro(exc):
    # Nunca publicar str(exc), SQL, parametros, nomes ou CPF.
    cadeia, vistos = [], set()
    atual = exc
    while atual is not None and id(atual) not in vistos:
        vistos.add(id(atual))
        locais = []
        tb = atual.__traceback__
        while tb:
            nome = tb.tb_frame.f_code.co_filename
            nome = (
                "app/" + nome.split("/app/", 1)[1]
                if "/app/" in nome else Path(nome).name
            )
            locais.append("%s:%d" % (nome, tb.tb_lineno))
            tb = tb.tb_next
        cadeia.append(dict(
            tipo=type(atual).__module__ + "." + type(atual).__name__,
            codigo=atual.args[0] if isinstance(atual, FalhaSonda) else None,
            sqlstate=getattr(atual, "sqlstate", getattr(atual, "pgcode", None)),
            locais=locais,
        ))
        atual = atual.__cause__ or atual.__context__
    return cadeia


def sql_so_leitura(execute, sql, params, many, context):
    # Mesma guarda de sonda2.py (anexo; linhas: [nao verificado]).
    if context["connection"].alias != connection.alias:
        raise FalhaSonda("OUTRA_CONEXAO")
    s = str(sql).lstrip().upper()
    permitido = (
        s.startswith(("SELECT ", "WITH ", "SHOW ", "SAVEPOINT ",
                      "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT "))
        or s.startswith("SET SEARCH_PATH")
        or s.startswith("SET SEARCH PATH")
    )
    exigir(permitido, "SQL_NAO_AUTORIZADO")
    try:
        return execute(sql, params, many, context)
    except Exception as exc:
        FALHAS_SQL.append(type(exc).__name__)
        raise FalhaSonda("ERRO_SQL") from exc


def log_estrito(self, level, msg, args, **kwargs):
    if level >= logging.WARNING:
        exc = kwargs.get("exc_info")
        if isinstance(exc, tuple) and isinstance(exc[1], BaseException):
            raise FalhaSonda("LOG_WARNING_OU_ERRO") from exc[1]
        raise FalhaSonda("LOG_WARNING_OU_ERRO")
    # Nenhum handler e chamado.


def engolido_estrito(*args, **kwargs):
    # app/core/observ.py:73: contar em memoria, sem registrar.
    global ENGOLIDOS
    ENGOLIDOS += 1


def chamada_estrita(func):
    def chamar(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except Exception as exc:
            raise FalhaSonda("ERRO_INTERNO_AUTORIDADE") from exc
    return chamar


class OpcoesDeConclusao(HTMLParser):
    def __init__(self):
        super().__init__()
        self.no_select = False
        self.destinos = set()

    def handle_starttag(self, tag, attrs):
        a = dict(attrs)
        if tag == "select":
            self.no_select = a.get("name") == "novo_status"
        elif tag == "option" and self.no_select:
            v = a.get("value")
            if v in ("resolvido", "fechado"):
                self.destinos.add(v)

    def handle_endtag(self, tag):
        if tag == "select":
            self.no_select = False


def preparar_porta(Classe):
    # Trava de versao: estes hashes sao da copia inspecionada.
    # app/chamados/models.py:128; modal_fio.html:650.
    exigir(
        hashlib.sha256(
            inspect.getsource(Classe.transicionar).encode()
        ).hexdigest()
        == "0e51c4fd8666653ed3face11668188ce5575f0063d55e644300f83c2afc7b507",
        "TRANSICIONAR_DIFERE_DA_COPIA",
    )
    tpl = get_template("chamados/partials/modal_fio.html").template
    exigir(
        hashlib.sha256(tpl.source.encode()).hexdigest()
        == "84d946ff6da7230a841228ce640c33eb4e7da823317cf8007047f3189439adcd",
        "TEMPLATE_DIFERE_DA_COPIA",
    )
    nos = [
        n for n in tpl.nodelist.get_nodes_by_type(IfNode)
        if n.token.lineno == 650
    ]
    exigir(len(nos) == 1, "PORTA_DO_TEMPLATE_NAO_IDENTIFICADA")
    return tpl, nos[0]


def destinos_na_tela(ch, tpl, no):
    from chamados.utils import pode_agir_chamado
    # Renderiza o IF REAL da caixa de resposta, incluindo seu IF de permissao.
    # Nao chama a view: abrir o modal escreve ciencia (views.py:880).
    ctx = Context(dict(
        chamado=ch, colaborador=ch.colaborador,
        pode_agir_map={ch.pk: pode_agir_chamado(USUARIO, ch)},
        modo_beneficio=False, csrf_token="NOTPROVIDED",
    ), use_l10n=False)
    with ctx.bind_template(tpl):
        html = no.render(ctx)
    parser = OpcoesDeConclusao()
    parser.feed(html)
    parser.close()
    return tuple(
        v for v in ("resolvido", "fechado")
        if v in parser.destinos
    )


RECUSAS = {
    134: "matriz/perfil recusa a transicao",
    139: "justificativa relacionada ainda pendente",
    156: "disputa aberta sem perguntas ou com pergunta viva",
    168: "pergunta viva ancorada na competencia aberta",
    192: "premissa do furo ainda nao regularizada",
}


def medir_transicao(ch, destino):
    codigo_real = type(ch).transicionar.__code__
    antes = ch.status_local

    def fronteira(novo_status, *, user):
        exigir(
            novo_status == destino and user is USUARIO,
            "FRONTEIRA_INESPERADA",
        )
        # models.py:199: parar ANTES de entrar no unico escritor _aplicar.
        raise AceiteAntesDaEscrita()

    try:
        with patch.object(ch, "_aplicar", fronteira):
            ch.transicionar(destino, USUARIO)
    except AceiteAntesDaEscrita:
        return dict(
            destino=destino, aceita=True,
            fonte="app/chamados/models.py:199",
        )
    except ValueError as exc:
        tb = exc.__traceback__
        while tb.tb_next is not None:
            tb = tb.tb_next
        # So recusas EXPLICITAS da funcao real viram (b).
        # ValueError de uma dependencia e falha da sonda, nao uma recusa.
        exigir(
            tb.tb_frame.f_code is codigo_real
            and tb.tb_lineno in RECUSAS,
            "VALUEERROR_FORA_DAS_GUARDAS_REAIS",
        )
        return dict(
            destino=destino, aceita=False,
            motivo=RECUSAS[tb.tb_lineno],
            fonte="app/chamados/models.py:%d" % tb.tb_lineno,
        )
    finally:
        exigir(
            ch.status_local == antes,
            "ESTADO_EM_MEMORIA_NAO_RESTAURADO",
        )
    raise FalhaSonda("TRANSICAO_NAO_ATINGIU_FRONTEIRA")


def medir_porta(ch, tpl, no):
    destinos = destinos_na_tela(ch, tpl, no)
    if not destinos:
        if ch.ticket_id_glpi:
            return False, dict(
                motivo=(
                    "sem porta efetiva de conclusao: "
                    "formulario GLPI so envia followup"
                ),
                fonte=(
                    "app/templates/chamados/partials/modal_fio.html:680; "
                    "app/chamados/views.py:292; "
                    "app/chamados/views.py:303"
                ),
            )
        return False, dict(
            motivo="formulario de conclusao ausente ou sem permissao",
            fonte=(
                "app/templates/chamados/partials/modal_fio.html:650; "
                "app/chamados/utils.py:68"
            ),
        )
    recusas = []
    for destino in destinos:
        r = medir_transicao(ch, destino)
        if r["aceita"]:
            return True, None
        recusas.append(r)
    return False, dict(
        motivo="recusada pela funcao real",
        recusas=recusas,
        aceite_das_alternativas_escritoras=(
            "aceite nao mensuravel sem escrita"
        ),
        fonte=(
            "app/chamados/models.py:128; "
            "app/chamados/services/validacao.py:435; "
            "app/chamados/services/validacao.py:490; "
            "app/chamados/services/materializacao.py:1376; "
            "app/chamados/signals.py:42"
        ),
    )


def executar():
    global PASSO
    from chamados.services import painel as P
    from chamados.services.verbos import (
        particionar_verbos, exige_o_admin, VERBOS_DO_ADMIN,
    )
    from chamados.catalogo import modulos
    from chamados.juizes import dia_do_fato, tranca_em_lote
    from chamados.models import ChamadoColaborador
    from colaboradores.models import Empresa
    from ponto.janelas import janela_atual
    from ponto.management.commands.e6_oraculo import InsumosDoDono
    from ponto.services.cxr_na_lista import cxr_lavrado

    PASSO = "schema_role_e_transacao"
    exigir(
        connection.schema_name == "juliani",
        "SCHEMA_DIFERENTE_DE_JULIANI",
    )
    with connection.cursor() as cur:
        cur.execute(
            "SELECT current_user, session_user, "
            "current_setting('transaction_read_only'), "
            "current_setting('transaction_isolation')"
        )
        role, login, leitura, isolamento = cur.fetchone()
    exigir(role == login == "leitor", "ROLE_DIFERENTE_DE_LEITOR")
    exigir(
        leitura == "on" and isolamento == "repeatable read",
        "TRANSACAO_INCORRETA",
    )
    exigir(
        set(
            Empresa.objects.filter(pk__in=EMPRESAS)
            .values_list("pk", flat=True)
        ) == set(EMPRESAS),
        "EMPRESA_AUSENTE",
    )
    tpl, no = preparar_porta(ChamadoColaborador)

    PASSO = "universo_mesma_funcao_da_sonda2"
    conjuntos, totais_empresa = {}, {}
    for emp in (None,) + EMPRESAS:
        f = "" if emp is None else str(emp)
        ctx = P.montar_contexto_painel(
            f_status="", f_urgencia="", f_q="", f_colab="", f_empresa=f,
            f_praca="", pagina=1, f_modo="lista",
        )
        vp = particionar_verbos(
            P.qs_vivos(f_empresa=f), agora=AGORA,
        )
        exigir(
            vp["contagens"] == ctx["verbos_contagens"],
            "PARTICAO_DIVERGENTE",
        )
        ids = {
            pk for pk, v in vp["mapa"].items()
            if v in VERBOS_DO_ADMIN
        }
        exigir(
            len(ids) == exige_o_admin(ctx["verbos_contagens"]),
            "CONTADOR_DIVERGENTE",
        )
        conjuntos[emp] = ids
        if emp is not None:
            totais_empresa[emp] = len(ids)

    ids = conjuntos[None]
    exigir(
        len(ids) == TOTAL_ESPERADO,
        "UNIVERSO_ATUAL_DIFERENTE_DE_1224",
    )
    uniao = set()
    for emp in EMPRESAS:
        exigir(not (uniao & conjuntos[emp]), "EMPRESAS_SOBREPOSTAS")
        uniao |= conjuntos[emp]
    exigir(
        uniao == ids,
        "UNIVERSO_FORA_DAS_EMPRESAS_2_3_4",
    )

    linhas = {
        (e, f): dict(
            a=0, b=0, c=0, total=0, cadastro=0, batida=0,
            l099_nao_verificado=0, exemplos_b=[],
        )
        for e in EMPRESAS for f in [nome]
    }
    cxr_do = cxr_lavrado()
    insumos, donos = {}, {}
    vistos = set()
    # Propagar o erro que dia_do_fato normalmente capturaria (juizes.py:115).
    with patch.object(
        modulos, "data_do_chamado",
        chamada_estrita(modulos.data_do_chamado),
    ), tranca_em_lote():
        qs = (
            P.qs_vivos().prefetch_related(None)
            .filter(pk__in=ids).select_related("celula")
            .order_by("pk")
        )
        for ch in qs:
            PASSO = "item_pk_%d" % ch.pk
            exigir(ch.pk not in vistos, "ITEM_DUPLICADO")
            vistos.add(ch.pk)
            emp = ch.colaborador.empresa_id
            exigir(
                emp in EMPRESAS and ch.pk in conjuntos[emp],
                "EMPRESA_DIVERGENTE",
            )
            if ch.celula_id:
                exigir(
                    ch.celula.colaborador_id == ch.colaborador_id,
                    "CELULA_DE_OUTRO_COLABORADOR",
                )
            dia = dia_do_fato(ch)
            fonte = (
                "sem vinculo" if dia is None
                else ch.modulo_origem
                if ch.modulo_origem in [nome][:6]
                else "outras"
            )
            dono = None
            if dia is not None:
                chave_dia = (ch.colaborador_id, dia)
                if chave_dia not in donos:
                    ini, fim = janela_atual(dia, ch.colaborador.empresa)
                    chave = (ch.colaborador_id, ini)
                    if chave not in insumos:
                        insumos[chave] = InsumosDoDono(
                            ch.colaborador, ini, fim, HOJE,
                            agora=AGORA, sem_motor=True,
                            cxr=cxr_do(ch.colaborador_id, ini),
                        )
                    donos[chave_dia] = insumos[chave].dono(dia)
                dono = donos[chave_dia]
            exigir(
                dono in ("CADASTRO", "BATIDA", "ESTRUTURA", None),
                "DONO_INVALIDO",
            )
            r = linhas[(emp, fonte)]
            r["total"] += 1
            if dono is None:
                r["l099_nao_verificado"] += 1

            # Ordem literal: (c), depois (a), depois (b).
            if dono in ("CADASTRO", "BATIDA"):
                r["c"] += 1
                r["cadastro" if dono == "CADASTRO" else "batida"] += 1
                continue
            aceita, detalhe = medir_porta(ch, tpl, no)
            if aceita:
                r["a"] += 1
            else:
                r["b"] += 1
                if len(r["exemplos_b"]) < 3:
                    r["exemplos_b"].append(dict(
               

[truncado em 20000 de 25148 caracteres]

## 10/11 11:56 · sessao 0a7c8a61e347 · FIO-E0-V4-E4A-2 · verificador · origem=Code

1. **Não é idêntica ao main informado.** A diferença integral da fábrica é: novo leitor da lâmpada `dia_nunca_bateu_sem_adesao` e duas guardas que retornam antes de emitir pergunta ou pauta. A barreira de veredito não acusador continua incondicional; a forma (B) saiu. (`app/chamados/services/fabrica_da_celula.py:87`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/services/fabrica_da_celula.py:183`.)

   Conferi os blobs em memória: atual `8ab8001ca…`; retirando somente essas três adições, resulta `8a4052f6c…`, exatamente os índices do diff integral fornecido. Comparação direta com o commit main, sem `.git` nesta cópia: [nao verificado]. (`app/chamados/services/fabrica_da_celula.py:87`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:183`.)

   Quem alcança a diferença:

   - Cartório → `perguntar_pela_celula` → guarda de `nunca_bateu`. (`app/ponto/services/cartorio.py:1008`, `app/chamados/services/fabrica_da_celula.py:183`.)
   - Reconciliador → `perguntar_por_eventos` → `perguntar_pela_celula`. (`app/chamados/reconciliador.py:1386`, `app/chamados/services/fabrica_da_celula.py:301`.)
   - `contador_fio_mudo` lê o mesmo predicado para separar retidos; para os demais, pergunta a `motivos_a_perguntar`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`.)
   - Repasse também lê o predicado e retém `nunca_bateu`. (`app/chamados/services/disputa_emissao.py:757`.)

2. **Sim: a obra inteira muda emissão em produção.** Retira a escrita do lote e impede emissão para `nunca_bateu` na fábrica e no repasse; isso não deve ser confundido com a forma (B), retirada nas voltas finais. (`app/chamados/management/commands/disparar_perguntas_competencia.py:189`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/disputa_emissao.py:758`.)

   Inventário dos arquivos produtivos alterados e dos escritores alcançados:

   | Sítio | Efeito e escrita |
   |---|---|
   | `disparar_perguntas_competencia` | O escritor anterior foi removido pelo diff fornecido. Agora seleciona, conta, imprime e eventualmente retorna 2; não cria/reabre/fecha pergunta, chamado ou notificação. (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`, `app/chamados/management/commands/disparar_perguntas_competencia.py:189`, `app/chamados/management/commands/disparar_perguntas_competencia.py:219`.) |
   | Fábrica: `motivos_a_perguntar` | Leitura; `nunca_bateu` retorna vazio. (`app/chamados/services/fabrica_da_celula.py:105`.) |
   | Fábrica: `perguntar_pela_celula` | **Pode escrever**, mas agora `nunca_bateu` sai antes de pauta, descarimbo e pergunta. Demais casos continuam pelas portas existentes, com push silenciado. (`app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/services/fabrica_da_celula.py:204`, `app/chamados/services/fabrica_da_celula.py:209`, `app/chamados/services/fabrica_da_celula.py:218`.) |
   | Fábrica: `perguntar` | Escritor alcançado: desfaz carimbo, cria/atualiza disputa e gera perguntas; seu corpo não mudou no diff integral. (`app/chamados/services/fabrica_da_celula.py:169`, `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`.) |
   | Repasse | Novo corte em `motivo_fora_da_fabrica`, usado antes dos caminhos de complemento e criação. Os caminhos restantes **podem escrever** chamado pai, disputa, perguntas, transição do chamado, fechamento e push. (`app/chamados/services/disputa_emissao.py:758`, `app/chamados/services/disputa_emissao.py:1066`, `app/chamados/services/disputa_emissao.py:1088`, `app/chamados/services/disputa_emissao.py:1104`, `app/chamados/services/disputa_emissao.py:1175`, `app/chamados/services/disputa_emissao.py:1284`, `app/chamados/services/disputa_emissao.py:1290`.) |
   | `pergunta_no_app` | Acrescenta descrição/dono da retenção; não emite pergunta ou push. (`app/chamados/services/pergunta_no_app.py:35`.) |
   | `resposta_tardia` | Diff apenas de formatação. Continua **podendo reabrir** pela porta existente, ou escrever nota, auditoria e pauta; não ganhou corte de `nunca_bateu`. (`app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/resposta_tardia.py:53`, `app/chamados/services/resposta_tardia.py:55`, `app/chamados/services/resposta_tardia.py:63`.) |
   | `vigiar_fio_morto` | **Escreve arquivo JSON**, não pergunta/chamado/push. Acrescenta os dois contadores ao carimbo. (`app/chamados/management/commands/vigiar_fio_morto.py:135`, `app/chamados/management/commands/vigiar_fio_morto.py:233`, `app/chamados/management/commands/vigiar_fio_morto.py:241`.) |
   | Crons / placar | Cron troca `--apply` por `--alarme`; placar lê o carimbo e alarma, sem emitir entidades de atendimento. (`app/config/crons.py:1521`, `app/core/management/commands/pipeline_placar.py:92`, `app/core/management/commands/pipeline_placar.py:159`.) |

   Portanto, não nasceu escritor novo de notificações; foram retirados caminhos que poderiam provocá-las, inclusive o antigo `--com-push` do lote. (`app/chamados/management/commands/disparar_perguntas_competencia.py:132`, `app/chamados/services/fabrica_da_celula.py:218`, `app/chamados/services/disputa_emissao.py:1289`.)

3. **O `FIO MUDO: 1` vem do juiz real, com entradas controladas.** O teste chama diretamente `motivos_a_perguntar` e exige exatamente `['intervalo_volta']` às 13:40; o contador também chama essa função. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:454`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`.)

   **Há escolhas explícitas na fixture:** grava `furo`/`concorde`, usa `bulk_create` para evitar sinais e dubla `_ata_do_dia` com faltas prescritas. Ela não substitui o resultado do juiz, mas também não prova que a lavra real produziria aquele veredito/ata. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:52`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:445`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:448`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:456`.)

   **Concorde → 0 tem mordida contra (B):** exige vazio diretamente do juiz, com intervalo vencido e turno ainda em curso. A forma (B) liberaria esse caminho; execução Django dessa mutação: [nao verificado]. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:17`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:479`, `app/chamados/services/fabrica_da_celula.py:139`.)

4. **As alterações dos testes existentes não são somente cosméticas.** Inventário versus o main representado pelo diff fornecido; comparação Git independente: [nao verificado]. [nao verificado]

   | Teste/arquivo atual | Alteração da asserção | [nao verificado]
   |---|---|
   | `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:54` | Rótulos `[EMITE]`→`VIGIA` e ALVOS novo; números 1/1 e 2/2 preservados. |
   | `app/chamados/tests/test_fabrica_pela_ata.py:51`, `app/chamados/tests/test_fabrica_pela_ata.py:64`, `app/chamados/tests/test_fabrica_pela_ata.py:71` | Só texto ALVOS, zero preservado. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:140`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:141` | Alvo continua 1; perguntas **1→0**. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:151`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:158` | Retratada: primeira pergunta passa a nascer pelo evento; estoque final **2→1**. Acrescenta pendência da célula 1 e rc 2. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:168` | Limbo: perguntas **1→0**; pendência 1 e rc 2 permanecem. |
   | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177` | Retira diagnóstico `PARCIAL NA LISTA DO ADMIN 1`; exige total `VIGIA FIO MUDO: 0`. |
   | `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26` | Exige `--alarme` em lugar de `--apply`; acrescenta proibição de `--apply`. |
   | `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:103` | Para `nunca_bateu`, `None`→causa `celula_nunca_bateu`; demais acusadores mantêm `None`. |
   | `app/chamados/tests/test_o261_vigia_fio.py:815`, `app/chamados/tests/test_o261_vigia_fio.py:825` | Igualdades completas de censo/esperado ganham os dois contadores. |
   | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106` | ALVOS zero preservado, texto novo. |
   | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:424` | `assertNotIn` da linha zero→regex exigindo linha existente com número positivo: **reforço**. |
   | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:477` | Igualdade das perguntas emitidas `['intervalo_volta']`→ausência de perguntas; adiciona a mesma lista como resultado direto do juiz. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:119`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:173` | ALVOS novo, número 1 preservado. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:91`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:147` | Zero preservado; retiradas as exigências `desligado 1` e `FIO DECIDIDO 1`. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:133`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:164` | Só `[EMITE]`→`VIGIA`, mesmos números; o método de `:164` já é `_test_`, não coletável. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:188`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:201` | Cron exige alarme; mensagem dry-run substituída por alvo 1 e ausência de DRY/APLICADO. |
   | `app/chamados/tests/test_selo_emissor_fio_mudo.py:270` | `assertTrue(vivas)`→`assertFalse(vivas)`; acrescenta conservação do estoque, contadores e rc. |
   | `app/core/tests/test_medida_turno_do_par.py:268` | Posição registrada **1196→1199**, ajuste de localização. |
   | `app/escala/tests/test_termometro_regua_prazo.py:19`, `app/ponto/tests/test_selfie_ausente_juiz.py:148`, `app/ponto/tests/test_varredura_lote_prazo.py:62` | Juízes por varredura **1→0**; acrescenta papel `vigia`. |
   | `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:338`, `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:420` | Catraca troca obrigação de leitura direta no handle por delegação obrigatória do contador ao juiz. |
   | `bin/tests/test_papel_prazo_nao_deriva.sh:305`, `bin/tests/test_papel_prazo_nao_deriva.sh:384` | Exceção real fica vazia; prova nominal usa exceção sintética e acrescenta verificação contra vazamento. |

   **Há perda de cobertura diagnóstica nos três casos nomeados acima:** total zero não comprova a causa do bloqueio. Isso merece ser registrado como retirada de asserção, embora acompanhe a remoção desses diagnósticos do comando. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:91`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:147`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177`.)

   No diff fornecido, não identifiquei `assertEqual`→`assertTrue`, encurtamento de `assertIn` para ocultar incompatibilidade, nem remoção integral de caso coletável; há inversões explícitas do contrato escritor→vigia e acréscimos de conservação de estoque. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:477`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:270`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:271`.)

   As demais mudanças preexistentes são de fixture/chamada, preservando asserções: COBPARC, resposta tardia e desligado; há novas verificações de ausência de escrita. (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:198`, `app/chamados/tests/test_resposta_tardia.py:34`, `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:90`, `app/chamados/tests/test_fabrica_por_evento.py:84`.)

5. **Matriz e bancos 1–4 permanecem realizados no código examinado; não encontrei regressão deles no delta 9e/9f.** A restauração da barreira para não acusadores atende à última errata. (`app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/tests/test_e4a2_matriz_memoria.py:198`.)

   | Requisito | Realização |
   |---|---|
   | (a) Retido | Lê a lâmpada, sem recalcular adesão; fábrica retorna antes da emissão. Contador compara pares únicos com a C5 real, na interseção declarada; desligado somente nos pares conservados pela C5. (`app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/management/commands/disparar_perguntas_competencia.py:54`, `app/chamados/management/commands/disparar_perguntas_competencia.py:59`, `app/chamados/management/commands/disparar_perguntas_competencia.py:66`.) |
   | (b) Alvo | Pergunta ao juiz inteiro, deduplica todos os motivos; esperado 0, dono ESTRUTURA. Alvo isolado não produz rc 2 no disparar. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`, `app/chamados/management/commands/disparar_perguntas_competencia.py:122`, `app/chamados/management/commands/disparar_perguntas_competencia.py:219`.) |
   | (c) Materializada | Pergunta viva exclui antes da separação dos dois contadores; materializada+retratada não é equiparada automaticamente a viva. (`app/chamados/management/commands/disparar_perguntas_competencia.py:109`, `app/chamados/services/fabrica_da_celula.py:145`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:153`.) |
   | (d) Tranca | Contador seleciona VIVOS; prova de integração exige que a tranca encerre primeiro e depois ambos zerem. Execução ORM: [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`, `app/chamados/tests/test_e4a2_nunca_bateu.py:164`.) |
   | Errata de unidade | Fronteira recebe iterável, normaliza para **PKs de empresa**, inclusive na chamada direta da C5; escalar é erro. A C5 filtra `Empresa.pk`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:25`, `app/chamados/management/commands/disparar_perguntas_competencia.py:52`, `app/chamados/management/commands/disparar_perguntas_competencia.py:104`, `app/escala/services/cadastro_realidade.py:177`.) |
   | Banco 1 | Fora da lista de juízes; parser sem `--apply`; cron em alarme. (`app/config/crons.py:2044`, `app/chamados/management/commands/disparar_perguntas_competencia.py:132`, `app/config/crons.py:1521`.) |
   | Bancos 2/3 | Mesma função fornece os dois números/esperados ao carimbo; placar lê ambos e compara. Totais C5 ficam informativos. (`app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:233`, `app/chamados/management/commands/vigiar_fio_morto.py:242`, `app/core/management/commands/pipeline_placar.py:135`.) |
   | Banco 4 | Lista real vazia, mordida sintética com isolamento. Executei o selo: **OK, rc 0, juízes por varredura=0**. (`bin/tests/test_papel_prazo_nao_deriva.sh:305`, `bin/tests/test_papel_prazo_nao_deriva.sh:384`, `bin/tests/test_papel_prazo_nao_deriva.sh:406`.) |

6. **Escritores e leitores relevantes aos campos do diff:**

   | Campo/valor | Quem escreve / quem lê |
   |---|---|
   | `veredito` | Porta `lavrar_veredito` escreve; fábrica, repasse, resposta tardia e C5 leem. (`app/ponto/portas/celula.py:1035`, `app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/services/disputa_emissao.py:758`, `app/chamados/services/resposta_tardia.py:45`, `app/escala/services/cadastro_realidade.py:181`.) |
   | `ata` | Lavra escreve a ata; `carimbar_precedencia` possui sua chave. Fábrica lê pelo predicado parcial e por `_ata_do_dia`. (`app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:1084`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:64`.) |
   | `julgada_em` | `carimbar` escreve. Cartório captura o anterior e o entrega à fábrica; fora do ato, fábrica usa o persistido. (`app/ponto/portas/celula.py:1065`, `app/ponto/services/cartorio.py:954`, `app/ponto/services/cartorio.py:1008`, `app/chamados/services/fabrica_da_celula.py:124`.) |
   | Linha `ALVOS` | Disparar imprime motivos devidos, não promessa de perguntas emitidas; admite encaminhamento ao DP. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`.) |
   | `fio_mudo_alvos` | Contador produz; vigiar persiste; pipeline e leitor genérico do ESTADO comparam número/esperado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:122`, `app/chamados/management/commands/vigiar_fio_morto.py:241`, `app/core/management/commands/pipeline_placar.py:98`, `bin/fio_estado.py:125`.) |
   | `nunca_bateu_sem_adesao` | Contador produz pares/interseção; vigiar publica número, esperado e detalhes; pipeline/ESTADO comparam. (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`, `app/chamados/management/commands/vigiar_fio_morto.py:234`, `app/chamados/management/commands/vigiar_fio_morto.py:242`, `app/core/management/commands/pipeline_placar.py:136`, `bin/fio_estado.py:125`.) |
   | `rc` | Disparar: 2 somente com alarme e pendência da célula. Pipeline: 2 com alarme e divergência. Corrente registra erro e continua os elos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:219`, `app/core/management/commands/pipeline_placar.py:159`, `bin/encadeado.sh:64`.) |
   | ESTADO | Vigiar grava o carimbo; gerador chama `fio_estado`, que inclui divergências genéricas no documento final. (`app/chamados/management/commands/vigiar_fio_morto.py:135`, `bin/gerar_estado.py:260`, `bin/fio_estado.py:191`, `bin/gerar_estado.py:390`.) |

   **Não localizei raspador produtivo do texto antigo de ALVOS na cópia.** O raspador identificado usa `FABRICA-POR-EVENTO: o julgamento deixou N`, preservado na saída. Leitores externos e logs reais: [nao verificado]. (`bin/placar_code.sh:390`, `app/chamados/management/commands/disparar_perguntas_competencia.py:209`.)

   `nunca_bateu` continua acusador para outros contratos; a obra altera seu destino de emissão, não a tupla geral. A causa `celula_nunca_bateu` e o contador de pares têm nomes/unidades separados. (`app/escala/models.py:1767`, `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/pergunta_no_app.py:35`, `app/chamados/management/commands/disparar_perguntas_competencia.py:66`.)

7. **Os mutantes mordem com controle verde e sem passagem vazia.** O executor exige uma única ocorrência da âncora, executa o controle e depois exige `AssertionError` com a mensagem pretendida. As âncoras dos três mutantes pedidos existem na fábrica atual. (`app/chamados/tests/test_e4a2_matriz_memoria.py:61`, `app/chamados/tests/test_e4a2_matriz_memoria.py:372`, `app/chamados/services/fabrica_da_celula.py:120`.)

   Executei matriz+C5 em memória: **29 testes OK, rc 0**, cobrindo os catálogos de 19 e 13 mutantes. Também foram executados avulsos `juiz_reintroduz_forma_b`, `juiz_libera_concorde_em_curso` e `juiz_libera_concorde_noturno`: um teste OK/rc 0 cada, significando controle verde e mutante rejeitado. (`app/chamados/tests/test_e4a2_matriz_memoria.py:85`, `app/chamados/tests/test_e4a2_matriz_memoria.py:365`, `app/chamados/tests/test_e4a2_c5_memoria.py:68`, `app/chamados/tests/test_e4a2_c5_memoria.py:204`.)

   O juiz nesses casos é a função real por AST, envolvida por `Mock(wraps=...)`; ORM, ata e outras bordas são dublados. O `dia_encerrado=False` é sentinela, e o caminho correto deve nunca chamá-lo. (`app/chamados/tests/test_e4a2_matriz_memoria.py:143`, `app/chamados/tests/test_e4a2_matriz_memoria.py:154`, `app/chamados/tests/test_e4a2_matriz_memoria.py:160`, `app/chamados/tests/test_e4a2_matriz_memoria.py:188`.)

8. **Classes de reprovação:**

   - **Segundo juiz:** não encontrei no cont

[truncado em 20000 de 22595 caracteres]

## 10/11 11:56 · sessao b29a0f87403e · - · - · origem=Ronald (tmux)

Concordo: regularização vazia fica em **(b), “aceite nao mensuravel sem escrita”**. A porta existe, mas `pode_fechar()` mede apenas a disputa; concluir o chamado depende de `reconciliar()` no signal (`app/templates/chamados/partials/modal_fio.html:352`; `app/chamados/models.py:1848`; `app/chamados/signals.py:42`).

Corrijo minha sugestão anterior: `reconciliaria()` chega a `trava_na_ordem()` dentro do `atomic`, que executa `SELECT FOR UPDATE`; não deve ser chamada nesse envelope (`app/chamados/models.py:661`; `app/chamados/models.py:672`; `app/chamados/models.py:707`; `app/chamados/models.py:711`; `app/chamados/models.py:2666`).

## 10/11 11:53 · sessao b29a0f87403e · - · - · origem=Ronald (tmux)

Complemento: há uma alternativa mensurável para disputa vazia que a transição genérica recusa: regularizar pelo modal, sem materialização pendente, usando `DisputaSupervisao.pode_fechar()` e `ChamadoColaborador.reconciliaria()`; o botão aparece porque `todas_respondidas=True` quando não há perguntas (`app/chamados/juizes.py:2308`; `app/chamados/services/fio.py:339`; `app/templates/chamados/partials/modal_fio.html:325`; `app/chamados/services/materializacao.py:1369`; `app/chamados/services/materializacao.py:1378`; `app/chamados/models.py:1848`; `app/chamados/models.py:657`).

O item precisa ser o chamado-pai da disputa: o signal só reconcilia esse pai (`app/chamados/signals.py:28`; `app/chamados/signals.py:32`; `app/chamados/signals.py:42`).

As demais alternativas inspecionadas dependem de escrita para demonstrar o fechamento:

| Alternativa | Evidência / tratamento |
|---|---|
| Ausência: Abrir registro → Aprovar/Rejeitar | Link no detalhe; aprovação/rejeição decide a ausência e o evento depois pede `ch.reconciliar()`. `gate_aprovacao_ausencia` sozinho não prova conclusão do chamado; usar motivo “aceite nao mensuravel sem escrita” se genéricas recusarem (`app/templates/chamados/detalhe_local.html:233`; `app/templates/ponto/partials/painel_ausencia.html:309`; `app/templates/ponto/partials/painel_ausencia.html:330`; `app/ponto/services/ausencia.py:616`; `app/ponto/services/ausencia.py:686`; `app/ponto/services/ausencia.py:911`) |
| Aceitar folga limbo | Grava template antes de reconciliar chamados; guardas iniciais não provam fechamento (`app/templates/chamados/partials/modal_fio.html:543`; `app/chamados/services/limbo_folgas.py:81`; `app/chamados/services/limbo_folgas.py:91`; `app/chamados/services/limbo_folgas.py:101`) |
| Aprovar exceção | Registra batida antes de transicionar chamado (`app/templates/chamados/detalhe_local.html:244`; `app/chamados/services/acoes_chamado.py:44`; `app/chamados/services/acoes_chamado.py:70`) |
| Aplicar declaração textual | Cria/reusa pergunta, responde e chama validação escritora (`app/chamados/services/declaracao_texto.py:641`; `app/chamados/services/declaracao_texto.py:659`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/services/declaracao_texto.py:696`) |
| Corroborar régua | Grava defesa e ocorrência; não encerra chamado diretamente (`app/escala/regua_defesa.py:647`; `app/escala/regua_defesa.py:662`; `app/escala/regua_defesa.py:670`) |
| Realocar régua | `julgar_vinculo` é leitura, mas execução e efeito sobre chamados dependem da escrita (`app/escala/regua_defesa.py:316`; `app/escala/regua_defesa.py:321`; `app/escala/regua_defesa.py:326`) |
| Benefício | Quando vinculado a chamado, tenta primeiro o mesmo `transicionar('resolvido'/'fechado')`; não contorna as recusas genéricas (`app/colaboradores/views.py:2066`; `app/colaboradores/views.py:2068`) |

`ir_para_ausencia` não deve ser chamada pela sonda: até o GET escreve ciência e ocorrência (`app/chamados/views.py:1513`; `app/chamados/views.py:1516`).

## 10/11 11:53 · sessao bab3a66b07ee · - · - · origem=Ronald (tmux)

Itens 2 e 6 — leitura da cópia:

- **Sim, a obra inteira altera quem recebe pergunta em produção.** `nunca_bateu` agora retorna sem perguntar em `app/chamados/services/fabrica_da_celula.py:105` e `:183`; isso alcança cartório (`app/ponto/services/cartorio.py:1008`) e consumidor (`app/chamados/reconciliador.py:1386` → `app/chamados/services/fabrica_da_celula.py:301`). O caminho segue silenciando push em `app/chamados/services/fabrica_da_celula.py:218`.
- **Repasse também muda:** `app/chamados/services/disputa_emissao.py:757` devolve causa para `nunca_bateu`; `:796` a transforma em “não regularizável”; filtros `:1066` e `:1088` o retiram antes do pai (`:1104`), perguntas (`:837`, `:1213`, `:1244`) e push (`:1289`). O catálogo apenas nomeia a retenção/admin (`app/chamados/services/pergunta_no_app.py:35`), lida por `:99`.
- **Disparar deixa de escrever:** contador apenas seleciona/deduplica (`app/chamados/management/commands/disparar_perguntas_competencia.py:94`–`:124`), comando apenas imprime e eventualmente sai 2 (`:189`–`:223`); cron passa `--alarme` (`app/config/crons.py:1521`). A prova da remoção do escritor é o diff fornecido; execução real em produção [nao verificado].
- **Vigiar grava arquivo, sem escrever perguntas/chamados:** chama contador em `app/chamados/management/commands/vigiar_fio_morto.py:181`; publica números/esperados em `:233` e `:241`; `_carimbar` grava JSON e faz replace em `:135`–`:137`. Só frota carimba (`:227`); recorte declara ausência de carimbo (`:249`).
- **Resposta tardia:** alteração do diff é apenas quebra de linha; comportamento permanece consultando renascimento (`app/chamados/services/resposta_tardia.py:45`–`:48`), podendo gravar nota/log/pauta (`:53`, `:55`, `:63`). Não foi adicionado corte de `nunca_bateu` nessa porta (`:45`).
- **Placar:** lê arquivo e compara dados, sem derivar alvos (`app/core/management/commands/pipeline_placar.py:92`–`:106`); consulta ambos contadores (`:135`–`:136`) e sai 2 com chave (`:158`–`:161`).

Mapa dos campos:

| Campo | Escritor | Leitor relevante |
|---|---|---|
| `veredito` | `app/ponto/portas/celula.py:1035`, persistência `:1040`; chamada delegada `app/escala/models.py:1826` | fábrica `:89`, `:120`; repasse `app/chamados/services/disputa_emissao.py:758`; resposta tardia `:45`; C5 `app/escala/services/cadastro_realidade.py:181` |
| `ata` | lavra `app/ponto/portas/celula.py:1033`; chave precedência `:1084`–`:1087` | fábrica `app/chamados/services/fabrica_da_celula.py:123`; motivos por `_ata_do_dia` `:64`, leitor `app/chamados/services/disputa_emissao.py:895` |
| `julgada_em` | `app/ponto/portas/celula.py:1065`, persistência `:1072` | cartório captura anterior `app/ponto/services/cartorio.py:954`, repassa `:1008`; fábrica usa `app/chamados/services/fabrica_da_celula.py:124` |
| `ALVOS` | stdout `app/chamados/management/commands/disparar_perguntas_competencia.py:215` | raspador antigo em produção externa [nao verificado]; não encontrei leitor do texto antigo na cópia |
| `fio_mudo_alvos` | retorno contador `app/chamados/management/commands/disparar_perguntas_competencia.py:122`; JSON `app/chamados/management/commands/vigiar_fio_morto.py:241` | placar `app/core/management/commands/pipeline_placar.py:85`, `:135`; ESTADO genérico abaixo |
| `nunca_bateu_sem_adesao` | pares/interseção C5 `app/chamados/management/commands/disparar_perguntas_competencia.py:66`–`:72`; JSON `app/chamados/management/commands/vigiar_fio_morto.py:234`, `:241`–`:242` | placar `app/core/management/commands/pipeline_placar.py:136`; ESTADO genérico |
| `rc` | disparar `:219`–`:223`; placar `:158`–`:161` | corrente captura e registra, continua para próximo elo: `bin/encadeado.sh:64`–`:68` |
| ESTADO | carimbo acima; publicação final `bin/gerar_estado.py:390` | `bin/gerar_estado.py:260`–`:261` chama `bin/fio_estado.py`; este compara **todas** as chaves `esperado`/`censo` (`:125`–`:126`) e imprime divergências (`:191`–`:194`) |

- **Raspagem preservada:** `bin/placar_code.sh:390` raspa exclusivamente prefixo `FABRICA-POR-EVENTO: o julgamento deixou N`; saída permanece em `app/chamados/management/commands/disparar_perguntas_competencia.py:209`. Não achei raspador antigo `ALVOS: N perguntas`, `[EMITE] FIO MUDO`, `FIO DECIDIDO` ou `PARCIAL NA LISTA DO ADMIN` em código de produção da cópia; leitores externos/logs de produção [nao verificado].
- **Significados separados:** `nunca_bateu` continua em `VEREDITOS_ACUSAM` (`app/escala/models.py:1767`), mas seu destino de pergunta é explicitamente cortado pela fábrica (`app/chamados/services/fabrica_da_celula.py:105`) e repasse (`app/chamados/services/disputa_emissao.py:758`); causa `celula_nunca_bateu` (`app/chamados/services/pergunta_no_app.py:35`) não se confunde com contador de pares (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`).

## 10/11 11:53 · sessao 03582047d95e · - · - · origem=Ronald (tmux)

Entrega bloqueada: o caminho autorizado `/home/ronald/wt-codex-fio-e0-v4-e4b-114449/app` desapareceu durante o trabalho. A persistência dos arquivos está [nao verificado].

Os casos a–e foram escritos e o RED inicial acusou módulo ausente; a gravação da implementação falhou depois. Cura, mutantes, Ruff e suite ORM não foram concluídos. [nao verificado] [nao verificado]

Restaure essa mesma cópia para eu continuar dentro da cerca autorizada.

## 10/11 11:53 · sessao 48f77b56afe0 · - · - · origem=Ronald (tmux)

Item 4 — inventário das asserções preexistentes alteradas no `diff_obra_v9f` fornecido; os valores anteriores vêm do anexo, sem comparação Git independente [nao verificado].

| Arquivo/linhas atuais | Alteração versus main informado |
|---|---|
| `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`, `:49`, `:54` | Rótulo `[EMITE]`→`VIGIA`; ALVOS recebe descrição nova, preservando 1 motivo/1 pessoa e 2/2. Acrescenta veto a escrita em `:41`. |
| `app/chamados/tests/test_fabrica_pela_ata.py:51`, `:64`, `:71` | Só texto de ALVOS, conservando zero. Adiciona igualdade de contagem antes/depois em `:41`; renomeia caso positivo sem mudar sua asserção 1 (`:44`, `:47`). |
| `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:140` | Rótulo mais explícito, mesmo FIO MUDO 1. |
| Mesmo, `:141` | Número/sentido: perguntas 1→0; vigia deve conservar estoque. |
| Mesmo, `:143` | rc continua 0; mensagem passa a dizer que alarme é do placar. |
| Mesmo, `:151`, `:152` | Primeira pergunta agora nasce explicitamente pelo evento; quantidade 1 mantida, acrescida precondição positiva. |
| Mesmo, `:158`, `:159`, `:160` | Retratada: perguntas 2→1; pendência da célula 0→1; rc 0→2. A fixture passa de concorde a acusadora. |
| Mesmo, `:168` | Limbo: perguntas 1→0; FIO MUDO1, pendência1 e rc2 permanecem (`:166`, `:167`, `:169`). |
| Mesmo, `:177` | Remove exigência diagnóstica `PARCIAL NA LISTA DO ADMIN 1`; substitui por total `VIGIA FIO MUDO: 0`. Não prova mais o número do motivo de bloqueio; pergunta0/pendência0/rc0 permanecem (`:175`, `:178`, `:179`). |
| `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26`, `:27` | Sentido: cron deve ter `--alarme`, passa a proibir `--apply`; não é mudança cosmética. |
| `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:103`, `:105` | Para nunca_bateu, `assertIsNone` vira igualdade com causa `celula_nunca_bateu`; demais acusadores conservam `None`. |
| `app/chamados/tests/test_o261_vigia_fio.py:815`, `:825` | Igualdades completas de censo/esperado recebem as duas novas chaves. Sem redução para igualdade parcial ou truthiness. |
| `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106` | ALVOS continua zero; texto novo. |
| Mesmo, `:424` | `assertNotIn` da linha zero vira regex exigindo linha existente com número estritamente positivo: reforço. Acrescenta mesma regex no outro cenário (`:429`). |
| Mesmo, `:476`, `:477` | Guarda preserva FIO MUDO1; igualdade de perguntas `['intervalo_volta']` é substituída por ausência de perguntas. Mudança de contrato escritor→vigia, não só texto. Acrescenta prova direta do juiz `['intervalo_volta']` em `:475`. |
| `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:119`, `:173` | Texto ALVOS novo, número1 preservado. |
| Mesmo, `:91` | `[EMITE] FIO MUDO0`→`VIGIA FIO MUDO0`; remove `desligado 1`. Perda explícita da cobertura do diagnóstico do motivo. |
| Mesmo, `:133` | Mesmo zero, rótulo VIGIA. |
| Mesmo, `:147` | Mesmo zero, rótulo VIGIA; remove `FIO DECIDIDO 1`. Perda explícita da cobertura desse diagnóstico. |
| Mesmo, `:164` | Apenas rótulo, mesmo1; método já era `_test_lapide...`, portanto já não era teste coletável (`:149`). | [nao verificado]
| Mesmo, `:188`, `:189` | Exige `--alarme`, proíbe `--apply`. |
| Mesmo, `:201`, `:202`, `:203`, `:204` | Troca exigência da mensagem dry-run por alvo1 e ausência de DRY/APLICADO; igualdade de estoque preservada. | [nao verificado]
| Mesmo, `:270` | `assertTrue(vivas)`→`assertFalse(vivas)`: inversão de sentido autorizada pela transformação em vigia; acrescenta igualdade do estoque e contadores/rc (`:271`–`:274`). |
| `app/core/tests/test_medida_turno_do_par.py:268`, `:540` | Registro de posição1196→1199; igualdade do censo continua, é ajuste de localização. |
| `app/escala/tests/test_termometro_regua_prazo.py:19`, `app/ponto/tests/test_selfie_ausente_juiz.py:148`, `app/ponto/tests/test_varredura_lote_prazo.py:62` | Número de juízes1→0. Último adiciona igualdade de papel vigia (`test_varredura_lote_prazo.py:63`). |
| `bin/tests/test_papel_prazo_nao_deriva.sh:305`, `:308`, `:384` | Lista real da exceção fica vazia; teste do cron nominal passa a testar exceção sintética com isolamento da real; acrescenta teste de não vazamento (`:389`). Mudança do objeto da prova, não remoção da mordida. |

Não vi no diff fornecido troca de `assertEqual` por `assertTrue`, encurtamento puro de `assertIn` para fazer caber saída incompatível, ou eliminação integral de caso coletável; há os três diagnósticos retirados acima, renomeações e inversões de contrato bem visíveis. Ausência integral de outros casos removidos depende da completude do diff fornecido [nao verificado].

Arquivos preexistentes modificados apenas em fixture/chamada, sem substituir asserção existente: `test_cobparc5_parcial_sem_periodo_so_lista.py:198`, `test_resposta_tardia.py:34`, `test_selo_disparo_nao_cobra_desligado.py:90`; foram acrescentadas novas asserções/testes em `test_fabrica_por_evento.py:84`, `test_pergunta_do_dia_em_curso.py:194`, `test_resposta_tardia.py:93`. A catraca COBPARC deixa de exigir leitura direta do antigo handle e exige delegação do contador ao juiz, incluindo delegação na própria verificação de obrigatórios (`test_cobparc5_parcial_sem_periodo_so_lista.py:324`, `:338`, `:420`, `:474`). [nao verificado]

Item 5 — realização estática e provas presentes:

- (a) A autoridade lê a lâmpada nunca_bateu, sem afirmar pertença C5 (`fabrica_da_celula.py:87`); juiz retorna vazio (`:105`) e escritor retorna antes de pauta/pergunta (`:183`). Contador separa retidos antes de alvos (`disparar_perguntas_competencia.py:111`), deduplica pares e compara com interseção da C5 (`:54`, `:65`, `:66`); desligados só entram nos pares realmente retidos por C5 (`:59`). A delimitação é declarada: chamados vivos do recorte ∩ piso anterior por empresa até ontem (`:43`, `:64`, `:71`).
- (b) Todos os motivos vêm de `motivos_a_perguntar(cel, hoje, agora)` (`disparar...py:115`), dedup por pessoa/dia/motivo (`:116`), esperado0 e donoESTRUTURA (`:122`); relatório retorna2 somente para pendência da célula e chave alarme (`:219`).
- (c) Pergunta viva exclui de ambos os contadores antes da separação de retidos (`disparar...py:109`). Integração da materialização está escrita em `test_e4a2_nunca_bateu.py:149`; não executei integração [nao verificado]. [nao verificado]
- (d) Universo filtra VIVOS (`disparar...py:99`); teste exige fechamento pela porta real da tranca antes de zerar ambos os contadores (`test_e4a2_nunca_bateu.py:164`). Execução [nao verificado]. [nao verificado]
- Errata unidade: fronteira normaliza iterável de empresas/PKs e rejeita escalar (`disparar...py:25`, `:30`, `:36`); aplica na entrada de ambos contadores (`:52`, `:88`), chama C5 com PKs (`:54`), ORM usa PKs (`:104`). C5 real filtra `Empresa.pk__in`, confirmando a unidade (`cadastro_realidade.py:177`).
- Banco1: parser só oferece ini/fim/comp-corrente/alarme/colab (`disparar...py:132`); retirada do cadastro de varredura documentada em `config/crons.py:2044`; selo específico verifica parser e cadastro (`test_e4a2_matriz_memoria.py:90`).
- Bancos2/3: vigia usa mesmo contador (`vigiar_fio_morto.py:181`), publica numero/esperado para ambos (`:233`, `:241`) e totais C5 informativos fora de censo/esperado (`:242`). Placar lê os dados e compara número≠esperado (`pipeline_placar.py:98`, `:105`), invocando os dois nomes (`:135`, `:136`).
- Banco4: exceções reais vazias (`bin/tests/test_papel_prazo_nao_deriva.sh:305`), sintética prova exceção nominal, não vazamento e não contaminação da real (`:384`–`:390`); resultado executado [nao verificado].
- Pelo delta9e+9f fornecido, as alterações produtivas são restauração da barreira não acusadora, enquanto retidos, fronteiras, carimbo, parser e retorno não foram alterados; não há regressão estática encontrada nos bancos1–4. A barreira atual rejeita concorde incondicionalmente (`fabrica_da_celula.py:120`), conforme errata. Execução geral/produção [nao verificado].

## 10/11 11:52 · sessao 0471b4770d31 · - · - · origem=Ronald (tmux)

3. **O 1 vem do juiz real, com insumos controlados pela fixture.** `_motivos_do_juiz` importa e chama `motivos_a_perguntar`, sem substituí-lo; exige exatamente `['intervalo_volta']` às 13:40 antes de conferir `FIO MUDO: 1`. O contador também chama esse juiz. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:454`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:475`, `app/chamados/management/commands/disparar_perguntas_competencia.py:117`.)

A fixture escolhe `furo` ou `concorde` por `update`, relê a célula, monta chamado aberto por `bulk_create` sem sinais e substitui `_ata_do_dia` por duas faltas prescritas. Portanto, prova a decisão do juiz sobre esses insumos; não prova que a lavra real produziria esse veredito/ata. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:52`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:442`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:456`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:462`.)

O `concorde -> 0` próprio exige `[]` diretamente do juiz antes do comando: a reintrodução de (B) deve falhar nessa primeira asserção, pois o turno das 08–18 ainda está em curso às 13:40 e há intervalo vencido na ata fornecida. A execução Django dessa mordida está [nao verificado]. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:17`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:479`, `app/chamados/services/fabrica_da_celula.py:139`.)

7. **Os três mutantes pedidos mordem com controle verde; não passam por alvo ausente.** Os alvos estão declarados e o executor exige uma única ocorrência, executa o controle antes da mutação e depois exige `AssertionError` com a mensagem pretendida. (`app/chamados/tests/test_e4a2_matriz_memoria.py:61`, `app/chamados/tests/test_e4a2_matriz_memoria.py:365`.)

Executei, com `PYTHONDONTWRITEBYTECODE=1`, a suíte de memória matriz+C5: **29 testes, OK, rc 0**; também cada mutante avulso: `juiz_reintroduz_forma_b`, `juiz_libera_concorde_em_curso`, `juiz_libera_concorde_noturno`: **um teste OK, rc 0 cada**. O mecanismo que dá significado a esse resultado está em `app/chamados/tests/test_e4a2_matriz_memoria.py:372`; saída persistida em arquivo [nao verificado].

Nesses casos, o juiz é a função real carregada por AST e envolvida por `Mock(wraps=...)`; `_ata_do_dia`, vitalidade, ORM, parcial e outras bordas são dublados. O `dia_encerrado=False` funciona como sentinela: o caminho correto deve nunca chamá-lo. (`app/chamados/tests/test_e4a2_matriz_memoria.py:143`, `app/chamados/tests/test_e4a2_matriz_memoria.py:154`, `app/chamados/tests/test_e4a2_matriz_memoria.py:160`, `app/chamados/tests/test_e4a2_matriz_memoria.py:188`.)

Limite relevante: `test_RED_selos_dos_catalogos_reais_sem_django` verifica o mecanismo de rejeição de catálogos vazios/encolhidos; não executa os cenários ORM dos catálogos de integração. Não confundir seu verde com integração Django verificada. (`app/chamados/tests/test_e4a2_matriz_memoria.py:321`, `app/chamados/tests/test_e4a2_matriz_memoria.py:340`.)

## 10/11 11:51 · sessao 68011ca7a87c · - · - · origem=Ronald (tmux)

A validação e a regularização podem concluir o chamado, mas o aceite completo depende de estado produzido pela própria escrita; os predicados individuais de leitura não provam esse desfecho (`app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:463`; `app/chamados/services/materializacao.py:1233`; `app/chamados/services/materializacao.py:1277`).

| Porta real | Encadeamento | Parte somente leitura / limite |
|---|---|---|
| Validar pergunta | `acao_validar_pergunta` → `validar_pergunta` (`app/chamados/services/acoes_disputa.py:13`; `app/chamados/services/validacao.py:373`) | Seleção, resposta, carimbo anterior e implausibilidade são conferidos antes da escrita (`app/chamados/services/validacao.py:385`; `app/chamados/services/validacao.py:393`; `app/chamados/services/validacao.py:404`). O aceite final só aparece depois de carimbar, materializar e reler (`app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:469`; `app/chamados/services/validacao.py:490`). |
| Validar inline | `validar_inline` → `acao_validar_pergunta` (`app/chamados/views_cobrar.py:145`; `app/chamados/views_cobrar.py:208`) | Resolve disputa aberta e perguntas vivas respondidas em leitura (`app/chamados/views_cobrar.py:160`; `app/chamados/views_cobrar.py:169`); herda a dependência de escrita da validação (`app/chamados/views_cobrar.py:208`). |
| Validar lote | `validar_lote_classe_a` → `acao_validar_pergunta` (`app/chamados/services/validacao.py:695`; `app/chamados/services/validacao.py:746`) | `classe_do_lote(perg)` é seleção real somente leitura, mas o resultado contado vem de `refresh_from_db` posterior à ação (`app/chamados/services/validacao.py:735`; `app/chamados/services/validacao.py:752`; `app/chamados/services/validacao.py:845`). |
| Regularizar disputa e fechar | Formulário condicionado a disputa aberta, sem `ancora_ausente` e com todas respondidas (`app/templates/chamados/partials/modal_fio.html:325`; `app/templates/chamados/partials/modal_fio.html:333`; `app/templates/chamados/partials/modal_fio.html:352`) → `acao_regularizar_disputa` (`app/chamados/services/acoes_disputa.py:118`) | Guarda de anuência usa `perguntas_vivas_de(d)` (`app/chamados/services/acoes_disputa.py:129`). Depois materializa; só então obtém `disputa_fechada` (`app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/acoes_disputa.py:143`). |
| Aceitar geofence | Formulário `autorizar_geofence` (`app/templates/chamados/partials/modal_fio.html:505`) → `acao_autorizar_geofence` (`app/chamados/services/acoes_chamado.py:165`) | Recusa terminal em leitura; escreve nota antes de chamar `transicionar('resolvido')` (`app/chamados/services/acoes_chamado.py:176`; `app/chamados/services/acoes_chamado.py:218`; `app/chamados/services/acoes_chamado.py:222`). A medição segura deve usar somente o prefixo real de `transicionar`, sem executar essa ação. |
| Recusar geofence | Formulário `recusar_geofence` (`app/templates/chamados/partials/modal_fio.html:527`) → `acao_recusar_geofence` (`app/chamados/services/acoes_chamado.py:243`) | Recusa terminal, motivo e pertencimento da batida são lidos antes de `retratar_batida`; esta escreve antes da transição final (`app/chamados/services/acoes_chamado.py:252`; `app/chamados/services/acoes_chamado.py:254`; `app/chamados/services/acoes_chamado.py:262`; `app/chamados/services/acoes_chamado.py:269`; `app/chamados/services/acoes_chamado.py:277`). |

Para medir a porta genérica, a chamada real `ch.transicionar(destino, user)` pode ser interrompida por sentinela em `_aplicar`, executando as guardas originais sem entrar no escritor: `_aplicar` é o último chamado de `transicionar` e só encaminha a `_gravar_estado` (`app/chamados/models.py:199`; `app/chamados/models.py:447`). Os destinos oferecidos na tela são `resolvido` e `fechado` (`app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/partials/modal_fio.html:669`).

**É necessário experimentar ambos:** a guarda S122 da premissa do furo só barra `resolvido`; `fechado` tem a guarda de justificativa pendente e compartilha as guardas de disputa e perguntas ancoradas (`app/chamados/models.py:136`; `app/chamados/models.py:148`; `app/chamados/models.py:165`; `app/chamados/models.py:181`).

`estado_aplicacao_pergunta(disputa, p)` é uma leitura real, mas não prova conclusão: não cobre o gate `DATA_EM_DISPUTA`, a avaliação do lastro após plantar, o carimbo C6 nem retenção por irmãs sem consumo (`app/chamados/services/materializacao.py:425`; `app/chamados/services/materializacao.py:1192`; `app/chamados/services/materializacao.py:1233`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:761`).

Quando ambas as transições genéricas forem recusadas e restarem somente validar/materializar/regularizar, o resultado seguro é coluna **(b)** com **`aceite nao mensuravel sem escrita`**, preservando também as recusas medidas das transições: essas ações só conhecem o aceite definitivo depois dos efeitos descritos acima (`app/chamados/services/validacao.py:490`; `app/chamados/services/materializacao.py:1376`; `app/chamados/services/materializacao.py:1378`).

## 10/11 11:51 · sessao b29a0f87403e · - · - · origem=Ronald (tmux)

A porta genérica existe para chamados locais não fechados, sem restrição por fonte: modal da Central oferece `resolvido` (“Encerrar”) e `fechado` (“Encerrar e arquivar”), condicionado a `pode_agir_map`; detalhe local oferece os mesmos destinos (`app/templates/chamados/partials/modal_fio.html:649`; `app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/detalhe_local.html:374`; `app/templates/chamados/detalhe_local.html:396`).

| Fontes | Porta / função real | Parte somente leitura |
|---|---|---|
| Todas as fontes pedidas, quando chamado local | Modal → `processar_post_fio` → `chamado.transicionar` (`app/chamados/services/fio.py:708`; `app/chamados/services/fio.py:721`) | Corpo real de `transicionar`, antes de `_aplicar` (`app/chamados/models.py:128`; `app/chamados/models.py:199`) |
| Mesmas fontes no detalhe local | `detalhe_chamado_local` → `acao_resposta_livre` → `transicionar` (`app/chamados/views.py:547`; `app/chamados/services/acoes_chamado.py:313`) | Mesmo corpo real (`app/chamados/models.py:128`) |
| `gps_fora_geofence` | Autorizar, autorizar com advertência, recusar (`app/templates/chamados/partials/modal_fio.html:480`; `app/templates/chamados/partials/modal_fio.html:505`; `app/templates/chamados/partials/modal_fio.html:527`) → funções de `acoes_chamado` (`app/chamados/services/fio.py:533`; `app/chamados/services/fio.py:546`) | Autorizar escreve ocorrência antes de chamar transição: não pode medir aceite completo dessa ação sem escrita; a porta genérica permanece mensurável (`app/chamados/services/acoes_chamado.py:217`; `app/chamados/services/acoes_chamado.py:222`) |
| Fontes com perguntas listadas | Validar pergunta no modal → `acao_validar_pergunta` → `validar_pergunta` (`app/templates/chamados/partials/modal_fio.html:278`; `app/chamados/services/fio.py:608`; `app/chamados/services/acoes_disputa.py:19`) | Pré-condições até `validacao.py:413` são leitura; aceite completo depende de materialização depois de carimbar a validação (`app/chamados/services/validacao.py:393`; `app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:452`) |
| Fontes com disputa | Regularizar disputa (`app/templates/chamados/partials/modal_fio.html:352`) → materialização (`app/chamados/services/fio.py:646`; `app/chamados/services/fio.py:670`) | Verificação de perguntas sem resposta é leitura, mas sucesso depende do materializador escritor (`app/chamados/services/fio.py:659`; `app/chamados/services/fio.py:670`) |
| Chamado GLPI, independentemente da fonte | Modal só linka detalhe (`app/templates/chamados/partials/modal_fio.html:680`). Detalhe mostra dropdown de status (`app/templates/chamados/detalhe.html:130`) | A view que recebe esse formulário ignora `novo_status` e apenas envia followup ao GLPI: não há conclusão efetiva por esse formulário no código da cópia (`app/chamados/views.py:292`; `app/chamados/views.py:303`; `app/chamados/views.py:309`) |

Para medir a guarda completa da transição sem replicá-la, chamar `ch.transicionar(destino, user)` real e substituir **somente `_aplicar`** por sentinela `BaseException`: atingir essa fronteira significa que o corpo real de recusa passou; nenhum método escritor é executado (`app/chamados/models.py:128`; `app/chamados/models.py:199`; `app/chamados/models.py:447`).

Guardas executadas nesse corpo: matriz/perfil, justificativa pendente para `fechado`, disputa viva, perguntas vivas ancoradas, premissa de furo para `resolvido` (`app/chamados/models.py:132`; `app/chamados/models.py:136`; `app/chamados/models.py:153`; `app/chamados/models.py:166`; `app/chamados/models.py:181`). **Tentar os dois destinos**: `fechado` não executa a guarda de premissa restrita a `resolvido` (`app/chamados/models.py:181`).

A verificação de perguntas vivas apenas altera estado em memória e o restaura em `finally`; não grava (`app/chamados/models.py:349`; `app/chamados/models.py:355`; `app/chamados/models.py:370`). O núcleo escritor repete esse tripwire antes de escrever; a decisão já passou pela mesma leitura para o item vivo (`app/chamados/models.py:165`; `app/chamados/models.py:221`; `app/chamados/models.py:224`).

Permissão real: `pode_agir_chamado(user, ch)`; superuser é gestor geral e passa por esse gate, e seu perfil é `admin_ti` (`app/chamados/utils.py:47`; `app/chamados/utils.py:57`; `app/chamados/utils.py:68`; `app/colaboradores/mixins.py:163`).

## 10/11 11:50 · sessao 6ebdcd75cedc · - · - · origem=Ronald (tmux)

O juiz real está em `app/ponto/management/commands/e6_oraculo.py:53`:

`dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)`.

Ele recebe os horários do dia e os insumos das autoridades existentes; retorna CADASTRO por folga não definida/sem vínculo, BATIDA por aberto com batida real ou quantidade ímpar, CADASTRO por fase deslocada/CXR, e ESTRUTURA no resíduo (`app/ponto/management/commands/e6_oraculo.py:98`).

A composição oficial, sem motor, é:

```python
from ponto.janelas import janela_atual
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.services.cxr_na_lista import cxr_lavrado

cxr_do = cxr_lavrado()
ini, fim = janela_atual(dia, colaborador.empresa)
insumos = InsumosDoDono(
    colaborador, ini, fim, HOJE,
    agora=AGORA, sem_motor=True,
    cxr=cxr_do(colaborador.pk, ini),
)
dono = insumos.dono(dia)
```

Essa é a própria receita da casa (`app/escala/services/cadastro_realidade.py:1017`; `app/escala/services/cadastro_realidade.py:1024`). Cachear por `(colaborador.pk, ini)` evita reconstruir a competência (`app/escala/services/cadastro_realidade.py:1018`).

`InsumosDoDono.dono(data)` monta cadastro, fase e batida fora de turno, e chama o juiz real (`app/ponto/management/commands/e6_oraculo.py:182`). O cadastro vem de `cadastro_do_dia_carregado(colaborador_id, data)` (`app/ponto/supra_juiz.py:166`). A CXR vem da lavra; `None` significa competência não coberta, enquanto conjunto vazio significa cobertura sem dias CXR (`app/ponto/services/cxr_na_lista.py:165`; `app/ponto/services/cxr_na_lista.py:182`).

Atenção: sem cobertura CXR, `sem_motor=True` pode devolver `None`; nunca transformar esse retorno em ESTRUTURA ou lista vazia (`app/ponto/management/commands/e6_oraculo.py:189`; `app/ponto/management/commands/e6_oraculo.py:193`).

O dia do chamado deve vir de `dia_do_fato(chamado, fortes=False)`, como na sonda2; retorna `date | None`, observando célula, contexto do catálogo/data_turno, batida, disputa e carimbos fracos (`app/chamados/juizes.py:87`; `app/chamados/juizes.py:98`; `app/chamados/juizes.py:129`; `app/chamados/juizes.py:138`). O balde **“sem vínculo” da sonda2 é dia ausente**, não equivale ao flag cadastral `sem_vinculo_no_dia`; este último vem da autoridade do vínculo do dia (`app/chamados/juizes.py:44`; `app/ponto/supra_juiz.py:172`; `app/ponto/supra_juiz.py:193`). Não chamar `InsumosDoDono` com dia `None`.

O envelope usa `ROLE=leitor`, monta código e sonda em leitura e define `PYTHONDONTWRITEBYTECODE=1` (`bin/sonda_leitura.sh:42`; `bin/sonda_leitura.sh:237`; `bin/sonda_leitura.sh:238`). Contudo, o próprio envelope escreve credencial/temporário/trilha e reafirma role/GRANTs; a ausência de escrita deve ser afirmada especificamente sobre a **sonda Python**, sem prometer que o shell inteiro não escreve (`bin/sonda_leitura.sh:126`; `bin/sonda_leitura.sh:140`; `bin/sonda_leitura.sh:157`; `bin/sonda_leitura.sh:216`; `bin/sonda_leitura.sh:248`).

## 10/11 11:47 · sessao f8b606e0ca44 · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; nenhuma escrita ou acesso a banco/rede/log executado. [nao verificado]

- **Autoridade exportada:** `competencia_do_dia_entregue` compõe `ponto.janelas.janela_atual` e `folha.export.competencia_entregue` (K6); `None` significa dia indeterminado. Reconsultar sem memo imediatamente antes de operar. `chamados/services/vigia_fio.py:822`
- **Competência real:** `competencia_do_fato(colaborador, dia)` delega à autoridade de competências do intervalo; usar para o rótulo empresa/competência, sem mês civil. `ponto/services/fechamento.py:1602`
- **Inventário:** `censo_alcance(..., limite=sys.maxsize)` mantém todos os dias. `limite=0` ainda inclui todo `sem_porta`, mas omite dias com porta; não serve ao inventário integral. `chamados/services/vigia_fio.py:627`
- **Classes reais:** a=âncora/chamado vivo fora da fila; b=respondida sem texto; c=respondida sob terminal; d=colaborador na lista, dia ausente; e=demais, inclusive sem célula. Não atribuir CADASTRO à classe d. `chamados/services/vigia_fio.py:667`
- **História:** chamar `DecisaoPergunta.linhas_da_projecao_sem_linha` e então `registrar`, sob trava, preserva correspondência pergunta+ato+instante. `registrar` não deduplica sozinho. `chamados/models.py:3907`
- **Travas:** usar `PerguntaDisputa.trava_na_ordem` antes da primeira escrita, com universo de disputas/chamados/perguntas atingido. Ordem C→D→Ch→P; múltiplos colaboradores devem ser travados previamente por PK. `chamados/models.py:2629`
- **Faltante:** `perguntar_pela_celula` atravessa teto, lista, dedup factual e fábrica; retorno vazio pode ser recusa legítima, e não autoriza alterar filtros para zerar. `chamados/services/fabrica_da_celula.py:162`
- **Terminal/âncora errada:** `reemitir_chamado_do_dia(p,colab,dia)` reúne os gates canônicos e termina pela porta de costura; pode recusar sem escrever. `chamados/reconciliador.py:761`
- **Órfã:** a porta real nesta cópia é `reatar_pergunta_orfa`, com derivação, histórico único, lavra ou reemissão. `chamados/reconciliador.py:1533`
- **Carimbo:** `desfazer_carimbo_sem_lastro` relê travado, consulta C6, preserva R/V e registra evento somente quando efetivamente desfaz. `chamados/services/relavra_via.py:187`
- **Batida/Ausência:** `_materializar_uma` é unidade canônica; não consulta exportada. Ausência passa por `_declarar_ausencia_de_pergunta`→`criar_ausencia(se_existir='retornar_mesmo_tipo')`. `chamados/services/materializacao.py:1158`, `chamados/services/materializacao.py:462`
- **Decisão não é efeito:** tarefa autoriza MATERIALIZAR após DP VALIDACAO e lavra vigente negativa; lavra ausente resulta em desconhecimento. `chamados/services/tarefa_do_dia.py:140`
- **Sem invenção:** classe b não permite reconstruir texto inexistente; classe a não prova véu temporário e não autoriza levantar silêncio automaticamente. `chamados/services/vigia_fio.py:666`, `chamados/services/tarefa_do_dia.py:132`
- **Precedente inadequado:** `passivo_validado.reparar` declara DRY que não chama a unidade; não reutilizá-lo como implementação de C3.5. `chamados/services/passivo_validado.py:359`

_3400 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
