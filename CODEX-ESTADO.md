# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 00:47** · publicado em **10/11/2026 00:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 00:47 · sessao 557d940ccf3a · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · construtor · origem=Code

Cura implementada para certificação: uma guarda em [pautas/services.py:38](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/services.py:38), chamada pelas portas humanas de [escrever:76](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/views.py:76) e [responder:114](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/views.py:114).

1. **Direção.** Sim: `import_module` escapa do detector A16, que examina apenas `Import`/`ImportFrom` de topo entre `ponto` e `chamados` (`core/tests/test_contract_direcao_a16.py:78`). Escolhi o módulo-folha [pautas/marcadores.py:1](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/marcadores.py:1): os donos importam constantes com seus aliases anteriores, e a guarda importa a mesma fonte. O detector real do A16 passou na execução isolada; a suíte A16 completa continua pendente (`pautas/tests/test_prefixos_reservados_ast.py:236`). O novo selo também recusa imports estáticos e dinâmicos no catálogo (`pautas/tests/test_prefixos_reservados_ast.py:28`).

2. **Prefixos novos.** Entram os quatro: afastamento, sem documento, férias recusadas e esmeril. Todos identificam buscas reais por `texto__startswith` (`ponto/services/afastado_avisa.py:63`, `ponto/services/ausencia.py:1044`, `ponto/services/ausencia.py:1234`, `ponto/services/pautas_do_esmeril.py:66`). Reservar `Bateu ponto durante afastamento. A batid` bloqueia também prosa humana iniciada nesses 40 caracteres; ocorrência legítima em produção é **[nao verificado]** (`pautas/marcadores.py:77`, `pautas/services.py:44`).

3. **Filhas.** Guardei também `responder`: há leitores sem filtro de pai e um leitor deliberado de filhas marcadas. Guardar somente cabeças deixaria a falsificação disponível por resposta (`ponto/services/ausencia.py:1044`, `pautas/management/commands/pauta_pago_x_devido_exportada.py:250`, `pautas/views.py:114`).

Os leitores conservam os prefixos exatos e suas derivações por semana, competência, colaborador ou chave. A normalização nova fica somente na guarda humana; o escritor comum continua aplicando seu `strip` anterior (`pautas/services.py:44`, `pautas/services.py:177`, `pautas/semanal.py:199`, `chamados/services/passivo_validado.py:159`).

Os donos abaixo foram tocados para importar a fonte única e reaproveitar as constantes nas buscas e escritas:

| Constante/família | Escritor | Leitor |
|---|---|---|
| `PAUTA_LIMBO_EXPORTADA` | `chamados/services/fabrica_da_celula.py:274` | `chamados/services/fabrica_da_celula.py:254` |
| `PAUTA_SEM_TURNO` | `chamados/services/fabrica_da_celula.py:362` | `chamados/services/fabrica_da_celula.py:305` |
| `TRANCA` | `chamados/services/competencia_trancada.py:235` | `chamados/services/competencia_trancada.py:221` |
| `ORFA_ENTERRADA` | `chamados/management/commands/reabrir_orfas_enterradas.py:209` | `chamados/management/commands/reabrir_orfas_enterradas.py:202` |
| `PASSIVO_VALIDADO` | `chamados/services/passivo_validado.py:230` | `chamados/services/passivo_validado.py:164` |
| `PASSIVO_VALIDADO_FORA` | `chamados/services/passivo_validado.py:267` | `chamados/services/passivo_validado.py:256` |
| `ARVORE_VERMELHA` | `core/management/commands/alarme_arvore.py:52` | `core/management/commands/alarme_arvore.py:16` |
| `ESTEIRA` | `core/management/commands/alarme_esteira.py:56` | `core/management/commands/alarme_esteira.py:15` |
| `CRON_QUEBRADO` | `core/services/crons_quebrados.py:72` | `core/services/crons_quebrados.py:54` |
| `FERIAS` — três marcadores | `ferias/management/commands/pautas_de_ferias.py:164` | `ferias/management/commands/pautas_de_ferias.py:152` |
| `PRE_FECHAMENTO` | `folha/management/commands/pre_fechamento.py:111` | `folha/management/commands/pre_fechamento.py:92`, `:98` |
| `DIA_EM_ABERTO` | `pautas/management/commands/pauta_dia_em_aberto.py:140` | `pautas/management/commands/pauta_dia_em_aberto.py:108` |
| `FOLGA_EXPORTADA` | `pautas/management/commands/pauta_folga_nao_definida_exportada.py:135` | `pautas/management/commands/pauta_folga_nao_definida_exportada.py:126` |
| `PAGO_DEVIDO_EXPORTADA` | `pautas/management/commands/pauta_pago_x_devido_exportada.py:208`, `:212` | `pautas/management/commands/pauta_pago_x_devido_exportada.py:243`, `:250` |
| `REVISAO_AUSENCIA` | `ponto/services/vigia_ausencia.py:239` → `pautas/semanal.py:278` | `pautas/semanal.py:199`, `:228` |
| `REVISAO_SUPERVISAO` | `escala/services/fase_12x36.py:278` → `pautas/semanal.py:278` | `pautas/semanal.py:199`, `:228` |
| `TEXTO_AFASTADO` / `AFASTADO` | `ponto/services/afastado_avisa.py:76` | `ponto/services/afastado_avisa.py:63` |
| `SEM_DOCUMENTO` | `ponto/services/ausencia.py:1060` | `ponto/services/ausencia.py:1044` |
| `FERIAS_RECUSADA` | `ponto/services/ausencia.py:1250` | `ponto/services/ausencia.py:1234` |
| `HE_EXPORTADA` | `ponto/services/autorizacao_he_periodo.py:245` | `ponto/services/autorizacao_he_periodo.py:242` |
| `COBERTURA_A_CONFIRMAR` | `ponto/services/cobertura.py:336` | `ponto/services/cobertura.py:243` |
| `ESMERIL` | `ponto/services/pautas_do_esmeril.py:79` | `ponto/services/pautas_do_esmeril.py:66`, `:67` |
| `APP_401` | `ponto/services/pautas_dos_deslogados.py:78` | `ponto/services/pautas_dos_deslogados.py:66` |
| `DISCORDANTE` | `ponto/services/pautas_dos_discordantes.py:180` | `ponto/services/pautas_dos_discordantes.py:98`, `:166` |

Completam os arquivos tocados: catálogo, service e views citados acima; [teste da porta](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/tests/test_prefixos_reservados.py:73), [selo AST](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/tests/test_prefixos_reservados_ast.py:167) e [executor de RED/mutantes](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-003109/app/pautas/tests/mutar_prefixos_reservados.py:85). [nao verificado]

**RED observado na base**, usando os corpos reais das funções com HTTP/persistência substituídos; não é prova de contagem em banco (`pautas/tests/test_prefixos_reservados.py:1`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados \
  --base a4b809999338e30088fc00d8e885d90c1e40bcb0
```

```text
testes=3 falhas=104 erros=0 [nao verificado]
AssertionError: 200 != 400
```

**Depois**, os testes isolados e AST terminaram com `Ran 12 tests — OK (skipped=1)`; o skip declara a integração Django indisponível (`pautas/tests/test_prefixos_reservados.py:143`, `pautas/tests/test_prefixos_reservados_ast.py:167`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  pautas.tests.test_prefixos_reservados \
  pautas.tests.test_prefixos_reservados_ast -v
```

**Mutantes:** saída observada `59 mortos; 0 sobreviventes`. Cobrem sete alterações de comportamento, retirada individual dos 26 reservados e troca individual dos 26 leitores; o executor imprime o comando de cada mutante (`pautas/tests/mutar_prefixos_reservados.py:37`, `:139`, `:147`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados

# Exemplo individual:
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados \
  --mutante sem_strip
```

Ruff nos 27 arquivos tocados devolveu `All checks passed!`, com as regras de `ruff.toml:5`; `git diff --check` também terminou sem saída.

**Não rodou:** testes Django novos, `pautas.tests` inteiro e os testes dos donos abaixo. O comando tentou iniciar, mas parou em `ModuleNotFoundError: No module named 'django'`, antes da descoberta (`manage.py:11`). Integração HTTP, contagem real de Pauta e suítes dos donos permanecem **[nao verificado]**; os testes de POST/400/zero Pauta e texto comum/200 estão em `pautas/tests/test_prefixos_reservados.py:173`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  pautas.tests \
  chamados.tests.test_competencia_trancada \
  chamados.tests.test_juiz_pauta_cadastro_sem_turno \
  chamados.tests.test_reabrir_orfas_enterradas \
  chamados.tests.test_o317_passivo_pela_porta \
  core.tests.test_crons_quebrados \
  core.tests.test_contract_direcao_a16 \
  core.tests.test_arvore_vermelha \
  core.tests.test_esteira_sem_humano \
  ferias.test_a3_situacao_de_ferias \
  folha.tests.test_pre_fechamento \
  ponto.tests.test_afastado_nunca_bloqueia \
  ponto.tests.test_porta_ausencia_ferias \
  ponto.tests.test_aus_f6_vencidas_pelo_juiz \
  ponto.tests.test_pautas_do_esmeril \
  ponto.tests.test_lista_viva_dos_deslogados \
  ponto.tests.test_o320_pauta_dos_discordantes \
  ponto.tests.test_a6_vigia_familia_ausencia \
  ponto.tests.test_declarar_cobertura_passada \
  ponto.tests.test_o214_item2_autorizar_periodo \
  escala.tests.test_fase_12x36 \
  --settings=config.settings_test --noinput
```

Para pautas históricas, o censo permite levantar candidatos pelos prefixos da tabela. Atribuir essas pautas a humanos apenas por `autor_user` não basta: o sistema pode assinar com superuser (`pautas/services.py:103`). Existência, quantidade e autoria humana em produção: **[nao verificado]**.

## 10/11 00:35 · sessao 12a663ea2904 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

Segue a **saída integral fornecida no enunciado**. Nova execução nesta sessão: [nao verificado]. O wrapper executa a sonda antes de chamar o Codex e incorpora a saída raspada ao prompt (`bin/codex_pergunta.sh:311–319`, `:337–344`).

```text
sonda_leitura: sonda4.py [7da39188eea4c4ff] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora_local 2026-11-10T00:32:57.861744-03:00 janela 2026-10-21 2026-11-20 N_CELS 48
col144 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col144 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col155 2026-10-26 COM_ADESAO dono=CADASTRO
col209 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col209 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col209 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col268 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col268 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col268 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col298 2026-11-05 SEM_ADESAO dono=CADASTRO
col298 2026-11-06 SEM_ADESAO dono=CADASTRO
col298 2026-11-07 SEM_ADESAO dono=CADASTRO
col298 2026-11-08 SEM_ADESAO dono=CADASTRO
col298 2026-11-09 SEM_ADESAO dono=CADASTRO
col312 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col312 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col427 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col427 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col534 2026-11-05 SEM_ADESAO dono=CADASTRO
col534 2026-11-06 SEM_ADESAO dono=CADASTRO
col534 2026-11-07 SEM_ADESAO dono=CADASTRO
col534 2026-11-08 SEM_ADESAO dono=CADASTRO
col534 2026-11-09 SEM_ADESAO dono=CADASTRO
col650 2026-11-06 SEM_ADESAO dono=ESTRUTURA
col650 2026-11-08 SEM_ADESAO dono=ESTRUTURA
col982 2026-10-21 COM_ADESAO dono=CADASTRO
col1002 2026-11-05 COM_ADESAO dono=ESTRUTURA
col1002 2026-11-07 COM_ADESAO dono=ESTRUTURA
col1005 2026-11-05 SEM_ADESAO dono=CADASTRO
col1005 2026-11-06 SEM_ADESAO dono=CADASTRO
col1005 2026-11-07 SEM_ADESAO dono=CADASTRO
col1005 2026-11-08 SEM_ADESAO dono=CADASTRO
col1005 2026-11-09 SEM_ADESAO dono=CADASTRO
col1009 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1009 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col1016 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col1016 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-06 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1018 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col1019 2026-11-05 SEM_ADESAO dono=ESTRUTURA
col1019 2026-11-07 SEM_ADESAO dono=ESTRUTURA
col1019 2026-11-09 SEM_ADESAO dono=ESTRUTURA
col1027 2026-11-04 SEM_ADESAO dono=ESTRUTURA
col1027 2026-11-06 SEM_ADESAO dono=ESTRUTURA
col1027 2026-11-08 SEM_ADESAO dono=ESTRUTURA
col1030 2026-11-09 SEM_ADESAO dono=ESTRUTURA
TABELA (ramo, dono) {('SEM_ADESAO', 'ESTRUTURA'): 29, ('COM_ADESAO', 'CADASTRO'): 2, ('SEM_ADESAO', 'CADASTRO'): 15, ('COM_ADESAO', 'ESTRUTURA'): 2}
ERROS 0
```

1. **Pela lei, sim: decide o juiz de dono**, ressalvada a competência exportada, que nunca vai ao app. A OS-19 também retira expressamente o atalho de agrupamento sob ADESAO/LIMBO como decisor de dono ([CORTES.md:326](/home/ronald/codex_ws/7b0dbff79fcf59c045dd89046e5576769f786624/app/docs/CORTES.md:326)). O juiz indicado pela L-099 é `dono_da_divergencia`; seus argumentos não incluem adesão (`app/docs/LEIS.md:122`; `app/ponto/management/commands/e6_oraculo.py:53–54`, `:98–110`).

   **Não encontrei, no caminho fábrica → app examinado, um sítio que leia esse dono para escolher app x lista.** `motivos_a_perguntar` não o consulta: usa dia sem trabalho, situação, data, teto, veredito, parcial na lista e deduplicação ([fabrica_da_celula.py:87–150](/home/ronald/codex_ws/7b0dbff79fcf59c045dd89046e5576769f786624/app/chamados/services/fabrica_da_celula.py:87)). A API lê `consulta_do_app`, cujo universo vem das perguntas vivas, sem leitura do dono L-099 (`app/api/views.py:2560–2562`; `app/chamados/services/pergunta_no_app.py:44–74`).

   Há leitores desse dono na **lista**: C9–C12 apresentam o dono e escolhem a ação por ele; `_porta_pelo_dono` distingue BATIDA, CADASTRO e ESTRUTURA (`app/escala/services/cadastro_realidade.py:906–928`, `:745–759`). O vigia também lê dono e presença do dia nessa lista para classificá-lo como `fio_do_admin`; isso classifica o contador, sem emitir pergunta (`app/chamados/services/vigia_fio.py:211–228`, `:313–314`).

2. **A entrada responde outra pergunta; o efeito também veta a pergunta no evento.** `adesao is None` significa ausência de primeira batida não retratada na vida, apurada em `app/ponto/services/cartorio.py:1165–1173`. O ramo lavra o veredito com `codigos=[]`, carimba e retorna (`app/ponto/services/cartorio.py:1253–1279`).

   Portanto, ele não é uma segunda implementação do juiz de **dono**, mas funciona como uma barreira independente à mesma saída “gerar pergunta”: não preenche `out['cods']`, e o wrapper só chama a fábrica para datas presentes nesse mapa. O ramo normal preenche o mapa (`app/ponto/services/cartorio.py:996–1008`, `:1437`). Dizer apenas “responde outra pergunta” esconderia esse efeito; chamar diretamente `motivos_a_perguntar` não reproduz essa barreira do cartório (`app/chamados/services/fabrica_da_celula.py:100–150`).

3. **Sim, existe na cópia a lista Cadastro x Realidade, assinatura C5, para “nunca bateu”.** Ela lê células lavradas com `veredito='nunca_bateu'`, desde a competência anterior até ontem; ativos entram sem a exigência de retrato usada para desligados (`app/escala/services/cadastro_realidade.py:175–200`). Essa leitura vira C5 e é incorporada à lista (`app/escala/services/cadastro_realidade.py:204–209`, `:876–879`).

   A ação exibida é “Fazer a adesao ao app ou lancar a ausencia do dia”, e há uma view que serve a lista ao usuário autorizado (`app/escala/services/cadastro_realidade.py:690–692`; [views.py:475–492](/home/ronald/codex_ws/7b0dbff79fcf59c045dd89046e5576769f786624/app/escala/views.py:475)).

A presença efetiva de cada célula fornecida no app ou nessa lista, seu veredito lavrado e sua condição de competência exportada são [nao verificado]: esses campos não aparecem no dado recebido.

## 10/11 00:34 · sessao 665dd54f0f38 · - · - · origem=Ronald (tmux)

- **Pela lei, sim, com a ressalva da exportação.** `app/docs/CORTES.md:326` determina: competência exportada nunca vai ao app; nas demais, decide o juiz de dono L-099. A mesma linha retira expressamente o atalho “agrupado sob ADESAO/LIMBO”: agrupamento não decide dono.
- A L-099 identifica o juiz em `app/docs/LEIS.md:122`: `dono_da_divergencia`, implementado em `app/ponto/management/commands/e6_oraculo.py:53`. Os retornos efetivos estão em `:98–110`; a montagem compartilhada o chama em `:182–195`. Ausência de adesão não é argumento desse juiz (`:53–54`).
- **Não identifiquei um sítio no fluxo da fábrica/app que leia esse dono para rotear genericamente SEM_ADESAO entre app e lista.** O endpoint usa `consulta_do_app` em `app/api/views.py:2560–2562`; a consulta separa pendentes/histórico por resposta e fechamento em `app/chamados/services/pergunta_no_app.py:69–74`. O universo vem de `q_vivas` e `pergunta_viva` em `:48–57`, sem consulta ao dono L-099. A fábrica `motivos_a_perguntar` também não o lê: corpo completo em `app/chamados/services/fabrica_da_celula.py:87–150`.
- **Há leitores de dono em produção, respondendo outras perguntas.** A lista Cadastro x Realidade apresenta os donos C9–C12 e escolhe as ações em `app/escala/services/cadastro_realidade.py:906–928`; `_porta_pelo_dono` envia BATIDA à regularização, CADASTRO à porta cadastral, ESTRUTURA ao sistema em `:745–759`. A view serve essa lista em `app/escala/views.py:484–492`.
- O vigia lê a lista e o dono lavrado em `app/chamados/services/vigia_fio.py:211–228`; quando o dia já está na lista com dono BATIDA/CADASTRO, classifica como `fio_do_admin` em `:313–314`. Isso classifica o contador; não é roteamento de emissão ao app (`:292–295`).
- Consequência sustentada pela cópia: a lei exige dono como autoridade, mas os fluxos de criação/consulta examinados não o usam nessa decisão (`CORTES.md:326`; `fabrica_da_celula.py:87–150`; `pergunta_no_app.py:44–74`). Presença efetiva dos 44 SEM_ADESAO na lista ou no app, e cobertura por competência exportada: **[nao verificado]** pelo dado fornecido.

## 10/11 00:34 · sessao 8b341e94a3fb · - · - · origem=Ronald (tmux)

(2) A entrada do ramo responde ao fato “há primeira batida não retratada na vida?”: consulta `Batida` sem retratação e define `adesao` pela primeira (`app/ponto/services/cartorio.py:1165–1167`). Porém o efeito não é só classificatório: ele lavra a célula com `codigos=[]` (`app/ponto/services/cartorio.py:1253`), carimba (`app/ponto/services/cartorio.py:1275`) e retorna antes dos emissores normais (`app/ponto/services/cartorio.py:1279`).

No evento, isso também barra a fábrica: `julgar_colab` só chama `perguntar_pela_celula` para datas presentes em `out['cods']` (`app/ponto/services/cartorio.py:996–1008`); o ramo sem adesão não registra essas datas, enquanto o ramo normal as registra (`app/ponto/services/cartorio.py:1437`). Portanto, a premissa é outra (adesão), mas há uma decisão independente com o mesmo efeito “não gerar pergunta no app”; não seria correto alegar que não interfere nessa pergunta. A própria documentação do ramo diz “carimba (registro) e nao emite” (`app/ponto/services/cartorio.py:1168–1173`).

`motivos_a_perguntar` não consulta adesão; suas guardas e retorno estão em `app/chamados/services/fabrica_da_celula.py:100–150`. Assim, chamá-la diretamente não equivale ao caminho do evento: a barreira sem adesão fica no cartório, antes da chamada.

(3) **Sim, existe na cópia a lista Cadastro x Realidade, assinatura C5.**

- `colabs_nunca_bateu` lê células com `veredito='nunca_bateu'`, desde o início da competência anterior até ontem (`app/escala/services/cadastro_realidade.py:175–183`); para ativos não exige confirmação no retrato da folha, que só entra para desligados (`app/escala/services/cadastro_realidade.py:185–200`).
- `_nunca_bateu` transforma isso na assinatura C5 (`app/escala/services/cadastro_realidade.py:204–209`), inserida na lista (`app/escala/services/cadastro_realidade.py:876–879`).
- C5 é rotulada como adesão, lavra do cartório (`app/escala/services/cadastro_realidade.py:78–79`), e aponta à ação “Fazer a adesao ao app ou lancar a ausencia do dia” (`app/escala/services/cadastro_realidade.py:690–692`).
- Há rota real (`app/escala/urls.py:14`), view com autorização `ver_escala` que chama a lista (`app/escala/views.py:475–492`) e template que mostra cada assinatura/dono/ação (`app/templates/escala/cadastro_x_realidade.html:28`).

A inclusão efetiva das células do dado raspado nessa tela hoje é **[nao verificado]**, pois o dado não traz veredito lavrado, resultado da lista nem renderização.

Existe também chamado de adesão para ativo com célula `nunca_bateu` (`app/chamados/services/adesao.py:32–40`), aberto na supervisão/cadastro (`app/chamados/services/adesao.py:74–86`), mas isso não substitui a evidência mais direta da lista C5.

## 10/11 00:33 · sessao 66d82483ec3b · - · - · origem=Ronald (tmux)

Achados na base, sem edições:

- Os quatro prefixos novos são identidades reais. Afastado procura `TEXTO[:40]` e escreve `TEXTO` (`ponto/services/afastado_avisa.py:65`, `:78`); ausência sem documento compartilha `marcador` entre busca e escrita (`ponto/services/ausencia.py:1038`, `:1041`, `:1060`); férias recusadas faz o mesmo com marcador por agendamento (`:1228`, `:1231`, `:1250`); esmeril escreve `[ESMERIL %s]` e lê `[ESMERIL ` (`ponto/services/pautas_do_esmeril.py:14`, `:49`, `:64`).

- O prefixo de afastado é `Bateu ponto durante afastamento. A batid`. Reservá-lo bloqueia qualquer texto humano iniciado nessa prosa; ocorrência legítima em produção é [nao verificado]. A reserva é necessária para cumprir a classe estabelecida, pois esse trecho controla a deduplicação (`ponto/services/afastado_avisa.py:62`).

- Os leitores preservam prefixos exatos, sem `strip` ou `casefold`; a normalização deve ficar apenas na guarda humana. O escritor comum já aplica `strip` (`pautas/services.py:163`), e os leitores usam diretamente os marcadores (`ponto/services/afastado_avisa.py:65`, `ponto/services/ausencia.py:1041`, `:1231`, `ponto/services/pautas_do_esmeril.py:64`).

- O selo AST precisa acompanhar os parâmetros de `semanal.py`: a busca semanal usa marcador mais semana (`pautas/semanal.py:199`), enquanto o fechamento usa marcador puro (`:228`). Os dois produtores reais passam `[A6/REVISAO-SEMANAL]` e `[SUP/REVISAO-SEMANAL]` (`ponto/services/vigia_ausencia.py:28`, `:239`; `escala/services/fase_12x36.py:237`, `:278`).

- Há bypass por respostas humanas: `views.responder` recebe texto livre (`pautas/views.py:112`) e `services.responder` grava uma Pauta filha (`pautas/services.py:194`). Muitos leitores não excluem filhas: semanal (`pautas/semanal.py:197`, `:226`), ausência (`ponto/services/ausencia.py:1039`, `:1229`), esmeril (`ponto/services/pautas_do_esmeril.py:63`) e TRANCA (`chamados/services/competencia_trancada.py:218`). `vivas()` também não exclui filhas (`pautas/models.py:57`).

- Existe leitor deliberado de filhas marcadas: `pauta_pago_x_devido_exportada` compara respostas iniciadas pela chave do sistema (`pautas/management/commands/pauta_pago_x_devido_exportada.py:248`). Portanto, acrescentar indiscriminadamente `pai__isnull=True` não resolve a classe; recomendo chamar a mesma guarda humana também em `views.responder`, preservando o escritor de sistema (`pautas/views.py:112`, `pautas/services.py:194`).

Testes relacionados: `ponto/tests/test_afastado_nunca_bloqueia.py:67`, `ponto/tests/test_porta_ausencia_ferias.py:83`, `ponto/tests/test_pautas_do_esmeril.py:27`, `ponto/tests/test_a6_vigia_familia_ausencia.py:166`, `escala/tests/test_fase_12x36.py:136`. Execução desses testes: [nao verificado].

## 10/11 00:32 · sessao e9da70ea9d32 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

O modo `--sonda` executa o envelope antes de chamar o agente e incorpora a saída raspada ao prompt; abaixo reproduzo integralmente o dado recebido, sem alegar nova execução. (`bin/codex_pergunta.sh:307-344`) Conteúdo de produção sem verificação independente: [nao verificado].

```text
sonda_leitura: sonda3.py [0d4b1316cd662228] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

col534 e2 situacao=ativo adm=2026-02-17 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1005 e2 situacao=ativo adm=2026-10-27 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col268 e2 situacao=ativo adm=2024-08-21 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col209 e2 situacao=ativo adm=2018-06-16 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col298 e2 situacao=ativo adm=2024-11-28 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col650 e2 situacao=ativo adm=2025-09-26 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col427 e2 situacao=ativo adm=2025-10-14 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1018 e2 situacao=ativo adm=2026-11-04 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1002 e2 situacao=ativo adm=2026-10-26 dem=None adesao=2026-11-09 vivas60=2 retratadas=0 ramo=COM_ADESAO
col1027 e2 situacao=ativo adm=2026-11-04 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1009 e2 situacao=ativo adm=2026-11-01 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1019 e2 situacao=ativo adm=2026-11-03 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col312 e2 situacao=ativo adm=2025-02-13 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col144 e3 situacao=ativo adm=2023-04-19 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1016 e2 situacao=ativo adm=2026-11-01 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col1030 e2 situacao=ativo adm=2026-11-09 dem=None adesao=None vivas60=0 retratadas=0 ramo=SEM_ADESAO
col982 e2 situacao=ativo adm=2026-10-07 dem=None adesao=2026-10-20 vivas60=20 retratadas=0 ramo=COM_ADESAO
col155 e3 situacao=ativo adm=2022-09-29 dem=None adesao=2026-05-25 vivas60=97 retratadas=0 ramo=COM_ADESAO
RAMO {'SEM_ADESAO': 15, 'COM_ADESAO': 3}
ERROS 0
== anexo: sonda1_resumo.txt ==
agora_local 2026-11-10T00:12:27.212274-03:00
janela 2026-10-21 2026-11-20 piso 2026-10-21
N_ITENS 139 CELULAS 48
POR_MOTIVO {'saida_sem_entrada': 45, 'orfao_14h': 48, 'intervalo_saida': 23, 'intervalo_volta': 23}
POR_DIA {'2026-10-21': 1, '2026-10-26': 1, '2026-11-04': 2, '2026-11-05': 35, '2026-11-06': 18, '2026-11-07': 38, '2026-11-08': 16, '2026-11-09': 28}
POR_EMPRESA {2: 134, 3: 5}
A 0 B 0 sem_evento_pos_dia 135 sem_evento_nenhum 0 com_evento_pendente 0
COMBO(A,B,ev_pos>0) {(False, False, False): 135, (False, False, True): 4}
TRABALHA {'True': 139} VEREDITO {'nunca_bateu': 137, 'indefinida': 1, 'cobrado': 1}
PERG_MESMO_DIA_QUALQUER>0 0
ERROS 0
 col1005 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
```

O material recebido registra `ERROS 0` nas duas saídas e três `COM_ADESAO`: col1002, col982 e col155. [nao verificado]

1. **COM_ADESAO: sim, o ramo normal rejulgaria após cada novo vencimento, se fosse chamado para aquela célula.** A adesão vem da primeira batida não retratada da vida, independentemente de haver batida naquele dia. (`app/ponto/services/cartorio.py:1165-1173`) Com `teto_de_hoje`, os marcos vencidos são calculados para hoje e ontem; sem nenhum vencido, a célula sai como `cedo`. (`app/ponto/services/cartorio.py:1324-1344`) A quantidade de vencidos entra na impressão, portanto um novo marco vencido muda esse insumo. (`app/ponto/services/cartorio.py:1359-1365`; `app/ponto/services/cartorio.py:463-464`) Além disso, `veredito == 'nunca_bateu'` torna `_em_dia=False` mesmo com impressão igual, impedindo o pulo em `:1373`. (`app/ponto/services/cartorio.py:1366-1375`) Essa guarda provoca rejuízo dentro de uma chamada; ela não agenda outra chamada. (`app/ponto/services/cartorio.py:1314-1378`)

2. **O caminho do julgamento até a fábrica depende da presença da data em `out['cods']`, inclusive quando os códigos são `[]`.** O wrapper retira os códigos acumulados do lote para isolar o ato atual e executa o corpo. (`app/ponto/services/cartorio.py:938-959`) O ramo normal classifica e grava `out.setdefault('cods', {})[data] = list(cods)`. (`app/ponto/services/cartorio.py:1424-1437`) Com `apply_=True`, lavra ata/veredito e carimba; depois, o wrapper lê `_julgadas = out.get('cods') or {}` e chama `perguntar_pela_celula` somente para datas presentes nesse dicionário. (`app/ponto/services/cartorio.py:1810-1835`; `app/ponto/services/cartorio.py:992-1008`) A fábrica calcula vencidos e motivos e, havendo motivos após as guardas, chama `perguntar`. (`app/chamados/services/fabrica_da_celula.py:190-210`)

3. **Por que os COM_ADESAO não emitiram os marcos seguintes em produção: [nao verificado].** O raspado não apresenta `out['cods']`, os resultados das chamadas por célula nem o cruzamento dos 139 itens com os três colaboradores desse ramo. [nao verificado] A ausência de evento posterior, isoladamente, não demonstra ausência de oportunidade de rejuízo: o vigia de hora também chama `julgar_celula(..., teto_de_hoje=True)` para candidatas que passam pelas suas guardas. (`app/ponto/services/vigia_de_hora.py:114-131`) Há ainda uma entrada independente de `cods`: o consumidor chama `perguntar_por_eventos`, que relê as células das datas do lote e chama `perguntar_pela_celula`. (`app/chamados/reconciliador.py:1384-1387`; `app/chamados/services/fabrica_da_celula.py:279-293`) Execução, candidatura e resultado desses caminhos para cada COM_ADESAO: [nao verificado].

4. **SEM_ADESAO: “carimba e não emite” corresponde ao encaminhamento do corpo do cartório.** Esse ramo tem sua própria idempotência; quando aplica, mantém `nunca_bateu` sem cobertura integral, lavra com `codigos=[]`, carimba e retorna em `:1279`. (`app/ponto/services/cartorio.py:1207-1216`; `app/ponto/services/cartorio.py:1243-1279`) Ele retorna antes da gravação de `out['cods']` em `:1437`; consequentemente, não encaminha essas datas à fábrica pelo laço do wrapper. (`app/ponto/services/cartorio.py:1279`; `app/ponto/services/cartorio.py:1437`; `app/ponto/services/cartorio.py:997-1008`) A guarda de rejuízo de `:1369` pertence ao ramo normal, alcançado quando existe adesão. (`app/ponto/services/cartorio.py:1173`; `app/ponto/services/cartorio.py:1279-1280`; `app/ponto/services/cartorio.py:1369`)

5. **Sim: fábrica e vigia podem contar motivos desses SEM_ADESAO como devidos; não há guarda de adesão nesse caminho.** `nunca_bateu` integra explicitamente `VEREDITOS_ACUSAM`. (`app/escala/models.py:1767-1768`) `motivos_a_perguntar` verifica dia sem trabalho, situação ativa, futuro, vencimento, veredito, parcial na lista e deduplicação; lê os faltantes pela ata, sem consultar adesão. (`app/chamados/services/fabrica_da_celula.py:100-150`; `app/chamados/services/fabrica_da_celula.py:52-74`) `motivos_que_o_evento_deixou` retorna esses motivos, tratando separadamente a pauta do limbo exportado. (`app/chamados/services/fabrica_da_celula.py:225-236`) O vigia do `disparar` acumula cada motivo retornado nas células passadas elegíveis, imprime a contagem e gera `SystemExit(2)` no caminho de alarme quando há pendências. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`; `app/chamados/management/commands/disparar_perguntas_competencia.py:239-259`)

Assim, há uma divergência demonstrável entre o ramo SEM_ADESAO, que retorna sem alimentar `cods`, e o leitor do vigia, que aceita seu veredito e pode encontrar marcos devidos na ata. (`app/ponto/services/cartorio.py:1253-1279`; `app/ponto/services/cartorio.py:997-1008`; `app/chamados/services/fabrica_da_celula.py:112-150,233-236`; `app/escala/models.py:1767-1768`) Quantificar essa divergência em todos os 139 itens ou atribuir a omissão concreta dos COM_ADESAO permanece [nao verificado].

## 10/11 00:30 · sessao 0f26bcd03582 · - · - · origem=Ronald (tmux)

- **SEM_ADESAO pode entrar como devido no vigia.** `motivos_que_o_evento_deixou` chama `motivos_a_perguntar` e retorna os motivos restantes, com desvio apenas para pauta do limbo exportado (`app/chamados/services/fabrica_da_celula.py:225-236`). `nunca_bateu` integra `VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`).

- **Não há guarda de adesão nesse caminho.** As guardas são dia sem trabalho/ativo, futuro, marcos vencidos, veredito, parcial e deduplicação (`app/chamados/services/fabrica_da_celula.py:102-150`). O teto consulta escala e vencimento por instante, com tolerância de 30 minutos (`app/ponto/services/cartorio.py:828-859`). A leitura dos faltantes usa ata, isenção, vínculo por admissão/demissão e tipo do dia; não consulta primeira batida/adesão (`app/chamados/services/declaracao_texto.py:100-143`; `app/escala/services/leitor_celula.py:365-398`).

- **O vigia do disparar contabiliza esses motivos.** Seleciona passado, exclui ontem enquanto ainda estiver na janela do vigia de hora, chama `motivos_que_o_evento_deixou` e acrescenta cada motivo em `deixou` (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`). Imprime o total e, no caminho de alarme, lança `SystemExit(2)` se houver pendências (`app/chamados/management/commands/disparar_perguntas_competencia.py:240-259`).

- **A divergência com “carimba e não emite” está no encaminhamento.** O ramo sem adesão lavra `codigos=[]`, carimba e retorna do corpo (`app/ponto/services/cartorio.py:1253-1279`). O wrapper encaminha à fábrica apenas datas presentes em `out['cods']` (`app/ponto/services/cartorio.py:992-1008`). Assim, o corpo pode deixar ata/veredito que o leitor do vigia considera cobravel sem colocar a data no conjunto encaminhado à fábrica (`app/ponto/services/cartorio.py:1253-1279`; `app/chamados/services/fabrica_da_celula.py:112-150`).

- Os quatro motivos vêm das posições dos marcos: início/E → `saida_sem_entrada`; fim/S → `orfao_14h`; intervalo ida/S → `intervalo_saida`; intervalo volta/E → `intervalo_volta` (`app/chamados/services/declaracao_texto.py:42-46`). A fábrica limita os faltantes aos marcos vencidos (`app/chamados/services/fabrica_da_celula.py:58-74`).

O raspado mostra oito itens exemplificados para `col1005`, identificado como SEM_ADESAO, e agregados gerais; não fornece o cruzamento completo de todos os 139 itens com os dois ramos, nem o histórico de execução de cada célula. [nao verificado]

## 10/11 00:30 · sessao e5762b0ed2d9 · - · - · origem=Ronald (tmux)

1. **COM_ADESAO: sim, se o julgamento for chamado.** A adesão é a primeira batida não retratada (`app/ponto/services/cartorio.py:1165-1167`); com adesão, o ramo sem adesão não é executado (`:1173`, `:1279`). No ramo normal, `nunca_bateu` torna `_em_dia=False` mesmo com impressão igual, impedindo o pulo por idempotência (`:1366-1375`). Com `teto_de_hoje=True`, hoje e ontem usam marcos vencidos por instante; nenhum vencido implica `continue` (`:1324-1344`). A quantidade de vencidos entra na impressão (`:1359-1365`; implementação em `:463-464`). Portanto, novo vencimento muda a impressão; e o antigo `nunca_bateu` já força rejuízo independentemente dessa mudança (`:1369`). Ainda há a saída `dia is None` (`:1376-1378`).

2. **Caminho exato até a fábrica no julgamento.** `julgar_colab` retira `cods` do lote para isolar o ato (`app/ponto/services/cartorio.py:938-946`), executa o corpo (`:959`) e eventual segunda passada (`:980-984`). O corpo normal calcula `cods` e registra a data **mesmo se a lista de códigos for vazia** (`:1424-1437`). Com `apply_=True`, o wrapper lê `_julgadas = out.get('cods') or {}` e chama `perguntar_pela_celula` somente para datas presentes nesse dicionário (`:992-1008`). A ata e o veredito são lavrados antes do retorno do corpo (`:1810-1834`). No `finally`, os códigos do ato voltam ao acumulado do lote (`:1077-1084`).

3. **SEM_ADESAO: o ramo não alimenta esse caminho.** Entra em `if adesao is None` (`app/ponto/services/cartorio.py:1173`); a impressão do ramo pode produzir pulo (`:1207-1216`). Se aplica, grava `nunca_bateu` sem cobertura integral e `codigos=[]` (`:1243-1253`), carimba (`:1275-1278`) e retorna (`:1279`), antes da única gravação de `out['cods'][data]` do corpo (`:1437`). Portanto, o wrapper não chama a fábrica para essas datas por esse ato (`:997-1008`). A guarda de `nunca_bateu` em `:1369` pertence ao **ramo normal**, alcançado depois de existir adesão, não ao ramo SEM_ADESAO (`:1173`, `:1279-1280`).

4. **Há outra entrada da fábrica, independente de `cods`.** O consumidor de eventos pendentes busca lotes (`app/chamados/reconciliador.py:1343-1352`) e chama `perguntar_por_eventos` (`:1384-1387`); esta relê as células atuais das datas dos eventos e chama `perguntar_pela_celula` (`app/chamados/services/fabrica_da_celula.py:288-292`). Por isso, não cabe afirmar que SEM_ADESAO nunca pode receber pergunta: a exclusão demonstrada acima é a entrada por `cods` no julgamento. Falhas do consumidor impedem marcar os eventos como processados (`app/chamados/reconciliador.py:1388-1401`).

5. **O relógio pode chamar sem evento novo.** O código agenda `vigia_de_hora --apply` a cada dez minutos (`app/config/crons.py:365-368`). A rotina candidata células `trabalha=True` de hoje e ontem (`app/ponto/services/vigia_de_hora.py:20-23`), aplica exclusões (`:117-129`) e chama `julgar_celula(..., teto_de_hoje=True)` (`:130-131`). Portanto, “sem evento posterior” sozinho não explica por que COM_ADESAO deixou de perguntar; a execução efetiva, candidatura e resultado dessas rodadas em produção são **[nao verificado]**. Ontem sai do alcance conforme `_ontem_em_janela` (`:47-58`, `:120-121`). O batch do cartório também chama `julgar_colab` para células passadas (`app/ponto/management/commands/processar_cartorio.py:49-65`).

6. **Limite do dado raspado.** A saída fornecida não contém `out['cods']`, instantes dos vencimentos, resultado das rodadas do vigia, nem cruzamento individual entre COM_ADESAO e cada uma das 139 faltas; logo, a causa concreta da omissão dos marcos seguintes para cada COM_ADESAO permanece **[nao verificado]**. O código demonstra a possibilidade e o caminho de rejulgamento, mas não permite atribuir o caso de produção a pulo por impressão, falta de rodada, guarda da fábrica ou falha do consumidor sem essas observações **[nao verificado]**.

## 10/11 00:29 · sessao 97b44640d8a7 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

**ERROS 18: a sonda não permite concluir a causa em produção nem validar a divisão COM_ADESAO/SEM_ADESAO.** O bloco recebido informa esse resultado; não houve nova execução nesta sessão. [nao verificado]

O `--sonda` é executado pelo wrapper **antes** de chamar esta sessão; depois, a saída raspada entra no prompt. Abaixo devolvo integralmente o bloco recebido, incluindo o anexo, sem apresentá-lo como nova execução. (`bin/codex_pergunta.sh:311-318`, `bin/codex_pergunta.sh:338-344`; conteúdo recebido: [nao verificado])

```text
sonda_leitura: sonda2.py [abace131b830b2ec] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

ERRO 534 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1005 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 268 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 209 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 298 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 650 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 427 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1018 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1002 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1027 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1009 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1019 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 312 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 144 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1016 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 1030 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 982 AttributeError 'Colaborador' object has no attribute 'ativo'
ERRO 155 AttributeError 'Colaborador' object has no attribute 'ativo'
RAMO {'SEM_ADESAO': 15, 'COM_ADESAO': 3}
ERROS 18
VEREDITO: SONDA COM ERRO -- numero nao conclui
== anexo: sonda1_resumo.txt ==
agora_local 2026-11-10T00:12:27.212274-03:00
janela 2026-10-21 2026-11-20 piso 2026-10-21
N_ITENS 139 CELULAS 48
POR_MOTIVO {'saida_sem_entrada': 45, 'orfao_14h': 48, 'intervalo_saida': 23, 'intervalo_volta': 23}
POR_DIA {'2026-10-21': 1, '2026-10-26': 1, '2026-11-04': 2, '2026-11-05': 35, '2026-11-06': 18, '2026-11-07': 38, '2026-11-08': 16, '2026-11-09': 28}
POR_EMPRESA {2: 134, 3: 5}
A 0 B 0 sem_evento_pos_dia 135 sem_evento_nenhum 0 com_evento_pendente 0
COMBO(A,B,ev_pos>0) {(False, False, False): 135, (False, False, True): 4}
TRABALHA {'True': 139} VEREDITO {'nunca_bateu': 137, 'indefinida': 1, 'cobrado': 1}
PERG_MESMO_DIA_QUALQUER>0 0
ERROS 0
 col1005 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
```

**COM_ADESAO: sim, condicionalmente à chamada do julgamento.** No código, adesão é a data da primeira batida não retratada; havendo essa batida, o ramo `adesao is None` não é tomado. (`app/ponto/services/cartorio.py:1165-1173`)

No ramo normal com `teto_de_hoje=True`:

- Hoje, sem marco vencido, a célula é pulada; havendo vencidos, sua quantidade entra na impressão. O vencimento considera marco + 30 minutos. (`app/ponto/services/cartorio.py:1324-1328`, `app/ponto/services/cartorio.py:1359-1361`, `app/ponto/services/cartorio.py:828`, `app/ponto/services/cartorio.py:858-859`)
- Cada aumento dessa quantidade muda a impressão, pois ela incorpora `teto`. Isso impede o pulo por impressão igual no vencimento seguinte. (`app/ponto/services/cartorio.py:463-464`, `app/ponto/services/cartorio.py:1369-1375`)
- Enquanto o veredito persistido for `nunca_bateu`, `_em_dia` será falso **mesmo com impressão igual**. Essa condição impede o pulo por idempotência; depois que o veredito mudar, deixa de fazê-lo. (`app/ponto/services/cartorio.py:1366-1375`)
- Isso não garante julgamento sem invocação, nem supera a ausência do dia na grade, que também causa `continue`. (`app/ponto/services/cartorio.py:1376-1378`)

Existe gatilho pelo relógio: a configuração agenda `vigia_de_hora --apply` a cada dez minutos, e o serviço chama `julgar_celula(..., teto_de_hoje=True)` nas candidatas que passam suas guardas. Portanto, **ausência de evento posterior, isoladamente, não explica a falta dos marcos seguintes nesse ramo**. (`app/config/crons.py:367`, `app/ponto/services/vigia_de_hora.py:114-131`)

O caminho até a fábrica é:

1. O julgamento normal calcula `cods` e grava `out['cods'][data] = list(cods)`, inclusive quando a lista está vazia. (`app/ponto/services/cartorio.py:1424-1437`)
2. O wrapper isola os códigos deste ato dos códigos anteriores do lote. (`app/ponto/services/cartorio.py:938-944`)
3. Com `apply_`, lê `_julgadas = out.get('cods') or {}` e chama `perguntar_pela_celula` para cada célula cuja **data esteja nesse dicionário**; não exige códigos não vazios naquela data. (`app/ponto/services/cartorio.py:992-1008`)
4. `perguntar_pela_celula` obtém os motivos; havendo motivos e sem encaminhamento à pauta, chama `perguntar`, que cria/atualiza a disputa e gera as perguntas. (`app/chamados/services/fabrica_da_celula.py:192-210`, `app/chamados/services/fabrica_da_celula.py:161-167`)

**Por que não emitiu nos COM_ADESAO observados? [nao verificado].** O dado recebido não demonstra qual chamada ocorreu, qual guarda impediu a passagem ou quais motivos retornaram em cada ato; com `ERROS 18`, não atribuo causa a esses casos. [nao verificado]

**SEM_ADESAO: há diferença demonstrável entre o caminho do cartório e a regra de contagem.** O comentário diz “carimba e nao emite”; esse ramo lavra/carimba e retorna sem preencher `out['cods']`. Assim, esse julgamento não habilita a chamada à fábrica pelo wrapper. (`app/ponto/services/cartorio.py:1168-1173`, `app/ponto/services/cartorio.py:1253-1279`, `app/ponto/services/cartorio.py:997-1008`)

Já a fábrica e o vigia **podem considerar devidos os motivos desses dias**, dependendo das demais guardas:

- `nunca_bateu` pertence expressamente a `VEREDITOS_ACUSAM`, portanto passa pelo filtro de veredito da fábrica. (`app/escala/models.py:1767-1768`, `app/chamados/services/fabrica_da_celula.py:113-114`)
- `motivos_a_perguntar` verifica dia sem trabalho, `situacao == 'ativo'`, futuro, vencimento, veredito, parcial na lista e pergunta já existente; **não verifica adesão**. A situação ativa é consultada por `situacao`, não por atributo `ativo`. (`app/chamados/services/fabrica_da_celula.py:102-150`)
- Os motivos precisam corresponder a marcos faltantes na ata e já vencidos; sem vencidos ou sem autorização/faltantes da ata, retorna lista vazia. (`app/chamados/services/fabrica_da_celula.py:58-74`)
- `motivos_que_o_evento_deixou` chama essa mesma função, com tratamento adicional da pauta do limbo, **sem acrescentar guarda de adesão**. (`app/chamados/services/fabrica_da_celula.py:225-236`)
- O vigia do `disparar` percorre células passadas da janela, exclui ontem enquanto ainda cabe ao vigia de hora e acrescenta cada motivo retornado a `deixou`, sem guarda de adesão nesse laço. Conta motivos e células distintas; esse caminho somente conta e alarma. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`, `app/chamados/management/commands/disparar_perguntas_competencia.py:240-251`)

Logo, o código permite **SEM_ADESAO carimbado sem fábrica pelo julgamento, mas com motivos contados pelo vigia** quando os demais filtros permitem. Isso demonstra a possibilidade estrutural; sua incidência e participação nos números recebidos permanecem [nao verificado]. (`app/ponto/services/cartorio.py:1279`, `app/ponto/services/cartorio.py:997-1008`, `app/chamados/services/fabrica_da_celula.py:102-150`, `app/chamados/services/fabrica_da_celula.py:233-236`)

## 10/11 00:28 · sessao b9ec0c8ed08e · - · - · origem=Ronald (tmux)

Donos com testes diretamente ligados aos marcadores, em labels Django: [nao verificado]

```text
chamados.tests.test_competencia_trancada
chamados.tests.test_reabrir_orfas_enterradas
chamados.tests.test_o317_passivo_pela_porta
chamados.tests.test_e4a_pauta_cadastro_contador
chamados.tests.test_juiz_pauta_cadastro_sem_turno
chamados.tests.test_e4_c20
chamados.tests.test_f1c4_3_emissores
core.tests.test_arvore_vermelha
core.tests.test_esteira_sem_humano
core.tests.test_crons_quebrados
ferias.test_a3_situacao_de_ferias
folha.tests.test_pre_fechamento
ponto.tests.test_o214_item4_dia_do_admin
ponto.tests.test_a6_vigia_familia_ausencia
escala.tests.test_fase_12x36
ponto.tests.test_afastado_nunca_bloqueia
ponto.tests.test_afastado_na_batida
ponto.tests.test_porta_ausencia_ferias
ponto.tests.test_a4_estados_com_porta
ponto.tests.test_o214_item2_autorizar_periodo
ponto.tests.test_declarar_cobertura_passada
ponto.tests.test_pautas_do_esmeril
ponto.tests.test_lista_viva_dos_deslogados
ponto.tests.test_o320_pauta_dos_discordantes
pautas.tests.test_pauta_dia_em_aberto_classe
pautas.tests.test_f3_beco1_pauta_exportada
pautas.tests.test_pauta_pago_x_devido_exportada
```

Referências diretas e pertinentes:

| Família | Teste | [nao verificado]
|---|---|
| TRANCA | `chamados/tests/test_competencia_trancada.py:158` |
| ÓRFÃS | `chamados/tests/test_reabrir_orfas_enterradas.py:234` |
| PASSIVO | `chamados/tests/test_o317_passivo_pela_porta.py:436` |
| CADASTRO | `chamados/tests/test_e4a_pauta_cadastro_contador.py:20`, `test_juiz_pauta_cadastro_sem_turno.py:16`, `test_e4_c20.py:99` |
| LIMBO | `chamados/tests/test_f1c4_3_emissores.py:190` |
| ÁRVORE | `core/tests/test_arvore_vermelha.py:32` |
| ESTEIRA | `core/tests/test_esteira_sem_humano.py:146` |
| CRON | `core/tests/test_crons_quebrados.py:56` |
| FÉRIAS | `ferias/test_a3_situacao_de_ferias.py:620` |
| PRÉ-FECHAMENTO | `folha/tests/test_pre_fechamento.py:81` |
| DIA | `pautas/tests/test_pauta_dia_em_aberto_classe.py:95` |
| FOLGA | `pautas/tests/test_f3_beco1_pauta_exportada.py:36` |
| PAGO | `pautas/tests/test_pauta_pago_x_devido_exportada.py:30` |
| A6 semanal | `ponto/tests/test_a6_vigia_familia_ausencia.py:173` |
| SUP semanal | `escala/tests/test_fase_12x36.py:151` |
| AFASTADO | `ponto/tests/test_afastado_nunca_bloqueia.py:84`, `test_afastado_na_batida.py:94` |
| A1 FÉRIAS | `ponto/tests/test_porta_ausencia_ferias.py:59` |
| A4 DOCUMENTO | `ponto/tests/test_a4_estados_com_porta.py:157` |
| HE | `ponto/tests/test_o214_item4_dia_do_admin.py:323`, `test_o214_item2_autorizar_periodo.py:169` |
| COBERTURA | `ponto/tests/test_declarar_cobertura_passada.py:93` |
| ESMERIL | `ponto/tests/test_pautas_do_esmeril.py:27` |
| APP | `ponto/tests/test_lista_viva_dos_deslogados.py:192` |
| DISCORDANTE | `ponto/tests/test_o320_pauta_dos_discordantes.py:59` |

O teste de férias fica fora de `*/tests/*`; o teste de cron acessa o dono por `call_command('cron_quebrado', ...)`, em `core/tests/test_crons_quebrados.py:56`. Logo ambos escapam de uma seleção feita só por nomes de constantes/imports.

Comando do grep das constantes específicas executado:

```bash
git grep -n -w \
  -e PAUTA_SEM_TURNO -e PAUTA_LIMBO_EXPORTADA \
  -e MARCADOR_FORA -e MARCADOR_PAUTA -e MARCADOR_A_CONFIRMAR \
  -- '*/tests/*'
```

Também executei busca por todos os literais dos marcadores e varredura AST de imports dos donos: esta última identificou 99 módulos dependentes, mas muitos exercitam funções do dono sem relação com a família reservada, especialmente `ponto.services.ausencia`. [nao verificado]

**Conflito concreto encontrado:** `chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:23` autoriza tocar `PAUTA_SEM_TURNO` apenas no seletor/escritor da fábrica. O selo captura import/nome/atributo/literal (`:115`) e reprova consumidores fora dessa lista (`:149`), no teste `:245`. A nova importação da constante por `pautas.services` precisa de ajuste estreito desse contrato e contraprova preservando a recusa de outro consumidor.

Nenhum arquivo alterado.

## 10/11 00:27 · sessao 6e2365d60a50 · - · - · origem=Ronald (tmux)

- O vigia do `disparar` conta os motivos devolvidos por `motivos_que_o_evento_deixou`, para células passadas na janela; ontem é excluído enquanto `_ontem_em_janela` o entrega ao vigia de hora. Não verifica adesão nesse laço. `app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`.

- `motivos_que_o_evento_deixou` chama `motivos_a_perguntar`; para limbo em competência entregue, desconta motivos já pautados, e nos demais casos devolve os motivos devidos diretamente. `app/chamados/services/fabrica_da_celula.py:225-236`.

- `motivos_a_perguntar` verifica dia sem trabalho, `situacao == 'ativo'`, futuro, vencidos, veredito, parcial na lista e dedup pela pergunta do fato; não tem guarda de adesão. `app/chamados/services/fabrica_da_celula.py:100-150`. `dia_sem_trabalho` lê `trabalha`, tipo da ata e cobertura parcial, sem adesão. `app/chamados/juizes.py:426-441`.

- `nunca_bateu` consta de `VEREDITOS_ACUSAM`; portanto esse veredito passa pela guarda da fábrica. Um SEM_ADESAO pode ser contado como devido quando satisfaz os demais filtros e tem motivos faltantes vencidos na ata, ainda sem pergunta do fato. Isso explica a possibilidade no código, não estabelece população ou causa real de produção. `app/escala/models.py:1767-1768`; `app/chamados/services/fabrica_da_celula.py:105-118`; `app/chamados/services/fabrica_da_celula.py:132-150`.

- O contador usa `len(deixou)`, conta células distintas e declara que o lote não escreve essas perguntas; o alarme sai com código 2 se `deixou` não estiver vazio. `app/chamados/management/commands/disparar_perguntas_competencia.py:240-251`.

- O vigia de hora candidata células `trabalha=True`, exclui desligado/isento, ontem fora de janela, nenhum marco vencido e batidas suficientes sem cobrança viva; chama `julgar_celula(...teto_de_hoje=True)` nas restantes. Não verifica adesão nesse percurso. `app/ponto/services/vigia_de_hora.py:21-23`; `app/ponto/services/vigia_de_hora.py:114-131`.

- A própria documentação alerta: rodadas perdidas além da janela de ontem não são recuperadas pelo `disparar`, que apenas conta e alarma; a pergunta depende de novo julgamento ou sinal de batida. `app/ponto/services/vigia_de_hora.py:39-43`.

## 10/11 00:27 · sessao 6de4b1b32b1f · - · - · origem=Ronald (tmux)

O caminho do código demonstra:

- `julgar_colab` retira o `cods` anterior do lote, mantendo o resultado por ato (`app/ponto/services/cartorio.py:944`). A fábrica roda com `apply_` e chama `perguntar_pela_celula` somente para datas presentes no `out['cods']` desse ato (`app/ponto/services/cartorio.py:992`, `app/ponto/services/cartorio.py:997`, `app/ponto/services/cartorio.py:1006-1008`).
- O ramo normal registra toda célula efetivamente julgada em `out['cods'][data]`, inclusive lista vazia (`app/ponto/services/cartorio.py:1436-1437`). A seleção usa a presença da chave da data, não a presença de códigos na lista (`app/ponto/services/cartorio.py:1007`).
- `adesao` é a primeira batida não retratada; ausência dela seleciona o ramo `nunca_bateu` (`app/ponto/services/cartorio.py:1165-1173`). Esse ramo carimba e retorna antes do ramo normal, sem registrar `out['cods']` (`app/ponto/services/cartorio.py:1253-1279`); portanto não habilita a fábrica do wrapper por esse ato (`app/ponto/services/cartorio.py:997-1008`).
- `motivos_a_perguntar` guarda dia sem trabalho, situação diferente de `ativo`, futuro, veredito fora dos acusadores, parcial destinada à lista e existência da pergunta do fato; não consulta adesão (`app/chamados/services/fabrica_da_celula.py:102-150`). `nunca_bateu` integra expressamente `VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`).
- Os motivos vêm dos marcos faltantes da ata e são recortados pelos vencidos por instante (`app/chamados/services/fabrica_da_celula.py:52-74`). O vencimento usa o mesmo `cartorio.marcos_vencidos`; nenhum marco vencido resulta em nenhum motivo (`app/chamados/services/fabrica_da_celula.py:45-49`, `app/chamados/services/fabrica_da_celula.py:58-61`). A margem é 30 minutos e a comparação é `quando + timedelta(minutes=teto_min) <= agora` (`app/ponto/services/cartorio.py:828`, `app/ponto/services/cartorio.py:858-859`).
- `perguntar_pela_celula` encaminha passado sem turno à pauta de cadastro, consulta os motivos e, havendo motivos, chama `perguntar` sob push silenciado; limbo de competência entregue vai à pauta DP (`app/chamados/services/fabrica_da_celula.py:192-210`). `perguntar` cria/atualiza a disputa e gera as perguntas (`app/chamados/services/fabrica_da_celula.py:161-167`).
- `motivos_que_o_evento_deixou` usa `motivos_a_perguntar` e a exceção de pauta do limbo; não tem guarda de adesão (`app/chamados/services/fabrica_da_celula.py:225-236`). Portanto SEM_ADESAO com `nunca_bateu` pode retornar motivos devidos quando as outras guardas permitem, apesar do comentário “carimba e nao emite” do cartório (`app/ponto/services/cartorio.py:1168-1172`; `app/escala/models.py:1767-1768`; `app/chamados/services/fabrica_da_celula.py:102-150`).
- O vigia do `disparar` seleciona passado da janela, exclui ontem ainda candidato do vigia de hora, acrescenta cada motivo devolvido à lista `deixou` e imprime sua contagem; também não consulta adesão nesse universo (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`, `app/chamados/management/commands/disparar_perguntas_competencia.py:240-243`).

Para COM_ADESAO, se a célula chega ao ramo normal com `teto_de_hoje` e há vencidos, `nunca_bateu` impede `_em_dia`; a mudança do número de vencidos também integra a impressão, fazendo o marco novo participar do rejulgamento (`app/ponto/services/cartorio.py:1324-1344`, `app/ponto/services/cartorio.py:1359-1375`). Isso depende de o chamador selecionar e invocar o julgamento da célula; causa real de não emissão na produção permanece [não verificado].

## 10/11 00:24 · sessao 70d18c1fa24a · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · censo · origem=Code

**Há um caminho que explica a perda de retorno por vencimento, mas ele depende de o colaborador continuar sem nenhuma batida apurável na vida.** Nesse ramo, o cartório considera a célula em dia pela impressão igual, sem incluir nela o avanço dos marcos. No ramo com adesão, ocorre o contrário: `nunca_bateu` impede o pulo. A sonda não demonstra a adesão atual de cada colaborador; atribuir esse mecanismo aos 137 itens seria **[nao verificado]**. (`app/ponto/services/cartorio.py:1165-1173`, `:1207-1216`, `:1366-1375`)

Uma precisão sobre o dado: o resumo fornecido registra **139/139 com `trabalha=True`**; os **135** correspondem aos itens sem evento posterior ao dia civil, e outros quatro têm evento posterior. As referências abaixo ao resumo contam as linhas do anexo textual. (`sonda1_resumo.txt:7-9`)

1. **Caminhos que podem voltar à célula e respectivas guardas**

| Caminho | Como deveria chegar ao julgamento/fábrica | O que impede ou limita o retorno |
|---|---|---|
| **Vigia de hora**, a cada dez minutos | Seleciona células de hoje e ontem, calcula vencidos e chama `julgar_celula(..., teto_de_hoje=True)`. (`app/config/crons.py:367`; `app/ponto/services/vigia_de_hora.py:114-131`) | Exige `trabalha=True`; exclui desligado/isento; ontem depende de `_ontem_em_janela`; sem vencidos pula; com batidas suficientes e sem cobrança viva pula. **Não tem guarda própria de “já julgada” ou `nunca_bateu`.** (`app/ponto/services/vigia_de_hora.py:21-23`, `:117-129`) |
| **Cartório chamado pelo vigia — sem adesão** | Entra no ramo `adesao is None`, definido pela ausência de qualquer batida apurável na vida. (`app/ponto/services/cartorio.py:1165-1173`) | `_em_dia_nb = bool(cel.julgada_em and cel.impressao == imp0 and cel.veredito)`; se verdadeiro e sem `forcar`, pula. `imp0` **não recebe teto nem encerramento**: o simples vencimento seguinte não muda esse insumo. (`app/ponto/services/cartorio.py:1207-1216`) |
| **Cartório chamado pelo vigia — com adesão** | Calcula teto de hoje/ontem e inclui a quantidade de vencidos na impressão. (`app/ponto/services/cartorio.py:1324-1344`, `:1359-1365`) | Pula impressão igual somente quando também `veredito != 'nunca_bateu'`. Portanto **`nunca_bateu` não bloqueia esse ramo**. (`app/ponto/services/cartorio.py:1369-1375`) |
| **`processar_cartorio`, madrugada** | Configurado com `--apply`; seleciona todas as células passadas da empresa e chama `julgar_colab`. O SQL não exclui julgadas nem `nunca_bateu`. (`app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:49-65`) | `data__lt=hoje` exclui o dia corrente; depois valem os dois pulos do cartório acima. O cron configurado não passa `--forcar`. (`app/ponto/management/commands/processar_cartorio.py:50`, `:62`, `:65`; `app/config/crons.py:616`) |
| **Consumidor de `EventoCelula`**, a cada minuto | Para as datas dos eventos, relê as células atuais e chama `perguntar_pela_celula`; **não rejulga previamente a célula**. (`app/config/crons.py:394`; `app/chamados/reconciliador.py:1384-1387`; `app/chamados/services/fabrica_da_celula.py:279-292`) | Só lê eventos não processados e com menos de cinco tentativas. Sem evento pendente, encerra o lote. Sem falha ou adiamento específico de órfãs, marca processado, mesmo havendo marcos futuros cortados pela fábrica. (`app/chamados/reconciliador.py:1209`, `:1249`, `:1343-1345`, `:1375-1378`, `:1393-1401`) |
| **`disparar_perguntas_competencia`**, corrente da manhã | No universo da célula, apenas chama `motivos_que_o_evento_deixou` e conta. (`app/config/crons.py:1521-1523`; `app/chamados/management/commands/disparar_perguntas_competencia.py:109-113`) | **Não fabrica nesse universo**, inclusive com `--apply`: alarma o estoque. Seu segundo universo ainda escreve, mas exige chamado vivo sem pergunta — o “fio mudo”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141-161`, `:240-251`, `:270-283`) |
| **Eventos de batida, ausência, justificativa e chamado** | Save/delete de batida ou ausência, save de justificativa e nascimento/mudança de status de chamado podem chamar o mesmo juiz. (`app/ponto/signals.py:131-165`; `app/chamados/signals.py:187-216`) | Dependem de ocorrer uma escrita; não são disparados pelo relógio. Há guarda contra reentrada durante julgamento; chamado sem mudança de status retorna. Na porta avulsa, hoje sem `teto_de_hoje` espera o fim do turno. (`app/ponto/signals.py:90-93`; `app/chamados/signals.py:192-193`, `:210-211`; `app/ponto/services/cartorio.py:1993-1998`) |

**Há ainda uma restrição anterior à fábrica direta:** `julgar_colab` só chama `perguntar_pela_celula` para datas presentes em `out['cods']` deste ato. O ramo sem adesão lavra/carimba e retorna sem registrar essa data em `cods`; o registro ocorre no ramo normal. Assim, naquele ramo, a fábrica pode ser alcançada pelo outbox da lavra, mas não por essa chamada direta. (`app/ponto/services/cartorio.py:938-944`, `:997-1008`, `:1253-1279`, `:1437`; `app/ponto/portas/celula.py:892-916`)

Os outros caminhos próximos não fornecem uma revisita geral de todos os marcos de uma célula sem batidas:

- **`detectar_ausencias`**, a cada cinco minutos, tenta abrir chamado; pula se já existe e seu funil veta `nunca_bateu`. Logo, não substitui o retorno marco a marco da fábrica. (`app/config/crons.py:364`; `app/ponto/management/commands/detectar_ausencias.py:424-439`, `:630-633`; `app/ponto/marcos.py:119`, `:155-162`)
- **`reconciliar_grade`** está configurado sem `--apply`; seu emissor também consulta esse veto e só emite chamado, sem pergunta própria. (`app/config/crons.py:460-462`; `app/ponto/management/commands/reconciliar_grade.py:502-504`, `:507-532`)
- **Alertas de turno** exigem entrada; **detector de intervalo** exige entrada e saída acesas, e sua emissão está aposentada. Esses requisitos excluem o caso de nenhuma batida no dia. (`app/ponto/management/commands/processar_alertas_turno.py:92-93`; `app/ponto/management/commands/detectar_intervalo_ausente.py:69-70`, `:149-156`)
- **`emitir_furo_retroativo`** é passe sob ordem, não esse cron periódico, e pula colaborador sem batidas na janela. **Cobrar dia manualmente** chama o juiz com `forcar=True`, mas só para passado. (`app/config/crons.py:1396`; `app/ponto/management/commands/emitir_furo_retroativo.py:53-56`; `app/chamados/services/cobrar_dia.py:28-41`)
- Mudança de vínculo, regeneração/reversão e queda de veto também têm chamadas ao juiz; dependem desses atos, sem agenda de vencimento de marco. (`app/colaboradores/services/pos_vinculo.py:61-72`; `app/ponto/portas/celula.py:422-434`, `:815-817`; `app/escala/regua_defesa.py:687-692`)

2. **Qual guarda importa depois do primeiro julgamento**

Para o caso central, não é `julgada_em` preenchida isoladamente: é a conjunção **carimbo + impressão igual + veredito preenchido**, no ramo sem adesão. O relógio dos marcos não entra na impressão desse ramo; no ramo normal, entra. (`app/ponto/services/cartorio.py:1207-1216`, `:1359-1375`)

Também há uma segunda idempotência na escrita: `carimbar` retorna sem salvar quando impressão, ausência e indicadores permanecem iguais e não houve nova lavra. Mesmo chegar novamente à porta não garante movimentar `julgada_em` ou produzir evento. O outbox normal só acorda por campos lavrados de **veredito/ata**. (`app/ponto/portas/celula.py:1070-1081`, `:892-916`)

O teto da fábrica tem outro papel: **corta motivos**, não agenda retorno. Além dele, a fábrica verifica dia sem trabalho/colaborador ativo, futuro, veredito que acusa, parcial na lista e identidade do fato já perguntado. `nunca_bateu` passa pelo teste de acusação porque pertence a `VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:105-150`; `app/escala/models.py:1767-1768`)

3. **O julgamento das 09:00 devolveu quais motivos? Quem deveria voltar?**

**A lista histórica efetivamente devolvida às 09:00 é [nao verificado].** O anexo não contém o retorno daquela execução nem seu DNA/ata/dedup histórico; os exemplos mostram horários de julgamento/evento e ausência atual de perguntas. (`sonda1_resumo.txt:10`, `:12-19`)

Pelo código, uma chamada à fábrica naquele instante só poderia devolver os motivos dos marcos faltantes **já vencidos há 30 minutos**. `vencidos_no_instante` chama `marcos_vencidos`; `motivos_pela_ata` exclui explicitamente cada `(tipo, hora)` fora desse conjunto. Portanto, **sim: os marcos seguintes ainda não vencidos são cortados**. (`app/ponto/services/cartorio.py:828`, `:856-859`; `app/chamados/services/fabrica_da_celula.py:35-49`, `:58-73`)

O mapa é:

| Marco faltante | Motivo |
|---|---|
| Início/E | `saida_sem_entrada` |
| Ida do intervalo/S | `intervalo_saida` |
| Volta do intervalo/E | `intervalo_volta` |
| Fim/S | `orfao_14h` |

Fonte: `app/chamados/services/declaracao_texto.py:42-46`.

O desenho geral diz “EVENTO carimba e julga”, consumidor lê a lâmpada e cron de varredura vigia. Entretanto, a configuração declara expressamente a exceção temporal: **o vigia de hora ocupa o papel `prazo`, e “a célula ainda não agenda o próprio marco”**. O responsável implementado pelo retorno por vencimento é esse vigia, delegando ao cartório; não aparece uma agenda independente de evento “marco venceu”. (`CLAUDE.md:166-170`; `app/config/crons.py:2092-2097`, `:2127-2131`)

`_marco_venceu_apagado` existe, mas apenas compara vencidos entre dois instantes e lê a ata; **não emite evento**. Os emissores do outbox registram lavra, chamado, fechamento de turno, alteração de pergunta, reversão ou semeadura. (`app/ponto/services/cartorio.py:403-423`; `app/ponto/portas/celula.py:794-796`, `:896-905`; `app/chamados/signals.py:194-207`)

4. **Por que 09/11 aparece no estoque observado às 00:12 de 10/11?**

O universo passa a admitir 09/11 quando `hoje` vira 10/11, pois exige `data < hoje`. Ontem só é excluído se `_ontem_em_janela(...)` devolver verdadeiro. Portanto, os 28 itens de 09/11 presentes na sonda **já passaram pelo resultado falso dessa função**; isso demonstra sua presença naquela medição, não o instante exato em que surgiram. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`; `sonda1_resumo.txt:1`, `:5`; instante de surgimento: **[nao verificado]**)

As condições de `_ontem_em_janela` são:

- Sem fim calculável: `False`. (`app/ponto/services/vigia_de_hora.py:48-50`)
- Último marco + 30 minutos anterior à meia-noite de hoje: `False`. (`app/ponto/services/vigia_de_hora.py:51-54`)
- Antes de fim + dois tetos: `True`; depois, depende de o dia ainda não estar encerrado em `agora - dois tetos`. (`app/ponto/services/vigia_de_hora.py:55-58`)

**O vigia de hora não alcança esses mesmos itens naquele instante**, pois usa precisamente a guarda inversa: se ontem e fora dessa janela, pula. No dia seguinte, eles também saem do universo hoje/ontem do vigia. Continuam possíveis o cartório de passado e eventos/atos posteriores, sujeitos às guardas anteriores. (`app/ponto/services/vigia_de_hora.py:21-23`, `:120-121`; `app/ponto/management/commands/processar_cartorio.py:49-65`)

5. **Quem escreve, quem lê e o esmeril semântico**

| Campo | Escrita efetiva |
|---|---|
| `julgada_em` e `impressao` | `carimbar`, nas atribuições e persistência da porta; chamado pelos dois ramos do cartório. (`app/ponto/portas/celula.py:1074-1081`; `app/ponto/services/cartorio.py:1275-1277`, `:1833`) |
| `veredito` | `lavrar_veredito`, pela porta; cartório e decisão administrativa a utilizam. Há também cópia explícita **somente em memória**, para leitura do julgamento em curso. (`app/ponto/portas/celula.py:1044-1049`, `:941-956`; `app/ponto/services/cartorio.py:1253`, `:1813`; `app/ponto/services/veredito_celula.py:351`) |

O comentário que ainda chama `processar_cartorio` de escritor único descreve a organização antiga; a atribuição efetiva está na porta acima. (`app/escala/models.py:1738-1746`; `app/ponto/portas/celula.py:1074-1081`)

**Leitores de `julgada_em` e da impressão comparada:**

| Uso | Leitores |
|---|---|
| Pulo e idempotência | Cartório nos dois ramos; `carimbar`. (`app/ponto/services/cartorio.py:1210-1216`, `:1369-1375`; `app/ponto/portas/celula.py:1070-1073`) |
| Julgamento anterior / parcial na lista | Cartório, fábrica, emissão, reconciliador, cluster, disparador e retroativo. (`app/ponto/services/cartorio.py:954`, `:1718`; `app/chamados/services/fabrica_da_celula.py:117`; `app/chamados/services/disputa_emissao.py:934-938`; `app/chamados/reconciliador.py:878-882`; `app/ponto/services/cluster_espurio.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `app/ponto/management/commands/emitir_furo_retroativo.py:121`) |
| Invalidação de leitura/lavra | Veredito lavrado e fase 12×36. (`app/chamados/veredito_lavrado.py:191-194`; `app/escala/services/fase_12x36.py:188-193`) |
| Auditoria/backfill/diagnóstico | Vigia B5, backfill B5, invariantes e diagnóstico de veredito velho; este último também lê `impressao`. (`app/ponto/management/commands/vigia_b5_ausencia.py:55`; `app/ponto/management/commands/backfill_b5_insumos.py:61`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1597`; `app/ponto/management/commands/celula_veredito_velho.py:109-110`) |

**Leitores operacionais de `veredito`, agrupados pelo efeito:**

- **Acusar/absolver e manter perguntas/chamados:** fábrica, juízes, modelo da pergunta, reconciliador, emissão, resposta tardia, limbo e silêncio por ausência. (`app/chamados/services/fabrica_da_celula.py:113`; `app/chamados/juizes.py:1321`, `:1372-1387`; `app/chamados/models.py:3356-3361`; `app/chamados/reconciliador.py:637-641`, `:1027-1030`; `app/chamados/services/disputa_emissao.py:769`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/silencio_ausencia.py:40-41`)
- **Vetar cobrança:** `celula_veta_cobranca`, lido pelos emissores e serviço de furos. (`app/ponto/marcos.py:119`, `:155-162`; `app/ponto/management/commands/detectar_ausencias.py:631-633`; `app/ponto/management/commands/reconciliar_grade.py:502-504`; `app/ponto/services/furos_sem_cobranca.py:98`)
- **Retenção, exposição e ata:** exportação, vigia de pausa, prévia, relatório da frota e leitor da célula. (`app/folha/export.py:1233-1255`; `app/folha/services/vigia_pausa.py:157`; `app/folha/previa.py:94`; `app/relatorios/dia_da_frota.py:51-60`; `app/escala/services/leitor_celula.py:424`)
- **Adesão/cadastro/proposição:** adesão, Cadastro × Realidade, propositor e régua de defesa. (`app/chamados/services/adesao.py:34`; `app/escala/services/cadastro_realidade.py:181`, `:477`, `:618`; `app/escala/services/propositor.py:474`; `app/escala/regua_defesa.py:688`)
- **Proteção da decisão humana:** calendário, serviço de veredito, mudanças de vínculo e portas de regeneração/reversão. (`app/colaboradores/services/calendario.py:411-416`; `app/ponto/services/veredito_celula.py:109`; `app/colaboradores/services/pos_vinculo.py:61`; `app/ponto/portas/celula.py:429`, `:816`)
- **Vigias, pautas e censos:** vigia do fio, furo sem canal, furos sem cobrança/vetados, discordantes, ausência × veredito, diagnóstico e pautas. (`app/chamados/services/vigia_fio.py:379`; `app/chamados/services/furo_sem_canal.py:35`, `:72`; `app/ponto/services/furos_sem_cobranca.py:61`; `app/ponto/services/furos_vetados.py:61`; `app/ponto/services/pautas_dos_discordantes.py:71`; `app/ponto/services/vigia_ausencia_veredito.py:42`, `:56`; `app/ponto/services/diagnostico_escala.py:177`; `app/pautas/services.py:422`)

**Sim, existem semânticas diferentes para `nunca_bateu`:**

- No ramo sem adesão, qualquer veredito preenchido com impressão igual basta para considerar o julgamento atualizado; no ramo com adesão, `nunca_bateu` exige rejulgamento. (`app/ponto/services/cartorio.py:1210-1216`, `:1369-1375`)
- No veto de emissão, `nunca_bateu` **cala cobrança**; na fábrica, nas perguntas e na retenção ele **continua acusando**, por pertencer a `VEREDITOS_ACUSAM`. (`app/ponto/marcos.py:119`, `:161-162`; `app/escala/models.py:1767-1768`; `app/chamados/services/fabrica_da_celula.py:113`; `app/folha/export.py:1233`)
- O comentário “`nunca_bateu` = fato resolvido” ainda existe imediatamente antes da tupla que o inclui entre os que acusam. É divergência documental explícita, não prova de encerramento uniforme no código. (`app/escala/models.py:1760-1768`)

## 10/11 00:23 · sessao f64053de7b9b · - · - · origem=Ronald (tmux)

O censo aponta três sentidos diferentes para `nunca_bateu`:

- **Ainda acusa** no vocabulário efetivo: está em `CelulaDia.VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`).
- **Veta cobrança de batida** no gate compartilhado: está em `VEREDITOS_QUE_VETAM` (`app/ponto/marcos.py:119`); `celula_veta_cobranca` lê `trabalha/veredito` e retorna veto (`app/ponto/marcos.py:155-162`). Os emissores obedecem em `app/ponto/management/commands/detectar_ausencias.py:631-633` e `app/ponto/management/commands/reconciliar_grade.py:502-504`.
- **Julgamento em dia enquanto não existe adesão**: o ramo `adesao is None` calcula impressão sem teto (`app/ponto/services/cartorio.py:1173`, `1207-1209`) e pula quando há `julgada_em`, impressão igual e qualquer veredito (`1210-1216`). Quando já existe adesão, a guarda deliberadamente **exclui** `nunca_bateu` do pulo (`1366-1375`).

Existe comentário contraditório dizendo `nunca_bateu = fato resolvido` em `app/escala/models.py:1761`, mas o conjunto executado abaixo inclui a palavra entre os que acusam (`1767-1768`). O comentário não demonstra execução.

**Escritores efetivos**

| Campo | Escritor e chamadas |
|---|---|
| `julgada_em`, `impressao` | `carimbar`: atribuições em `app/ponto/portas/celula.py:1074-1075`; persistência em `1081`. Chamado pelos dois ramos do cartório: `app/ponto/services/cartorio.py:1275`, `1833`. |
| `veredito` | `lavrar_veredito`: atribuição/persistência em `app/ponto/portas/celula.py:1044-1049`. O método do modelo delega (`app/escala/models.py:1819-1828`). Chamadores: cartório (`app/ponto/services/cartorio.py:900`, `1253`, `1813`) e decisão humana (`app/ponto/services/veredito_celula.py:351`). |
| Cópia de `veredito` em memória | `celula_como_o_julgamento_deixa`, explicitamente sem gravação (`app/ponto/portas/celula.py:941-956`). |

`carimbar` também lê ambos os campos para idempotência e não grava se os insumos e a lavra não mudaram (`app/ponto/portas/celula.py:1070-1073`). Somente mudança de `veredito` ou `ata` acorda o consumidor pelo save; alteração isolada do carimbo não basta (`app/ponto/portas/celula.py:893`, `908-916`).

**Leitores de `julgada_em`**

- Pulo e instantâneo anterior: `app/ponto/services/cartorio.py:954`, `1210`, `1369`.
- Cobertura parcial e fato novo: `app/ponto/services/cartorio.py:400`, `1718`; `app/chamados/services/fabrica_da_celula.py:117`; `app/chamados/services/disputa_emissao.py:934-938`; `app/chamados/reconciliador.py:878-882`; `app/ponto/services/cluster_espurio.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `app/ponto/management/commands/emitir_furo_retroativo.py:121`.
- Invalidação temporal: `app/chamados/veredito_lavrado.py:191-194`; `app/escala/services/fase_12x36.py:188-193`.
- Universo de auditoria/backfill: `app/ponto/management/commands/vigia_b5_ausencia.py:55`; `app/ponto/management/commands/backfill_b5_insumos.py:61`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1597`.
- Diagnóstico: `app/ponto/management/commands/celula_veredito_velho.py:109`.

**Leitores de `impressao` da célula**

- Pulo: `app/ponto/services/cartorio.py:1210`, `1369`.
- Idempotência: `app/ponto/portas/celula.py:1071`.
- Diagnóstico: `app/ponto/management/commands/celula_veredito_velho.py:110`.

O hash inclui o relógio quando o chamador entrega `teto` (`app/ponto/services/cartorio.py:460-464`); o ramo sem adesão não entrega esse argumento (`1207-1209`).

**Leitores operacionais de `veredito`, por grupo**

- Acusação/absolvição: fábrica (`app/chamados/services/fabrica_da_celula.py:113`), juiz de pergunta viva (`app/chamados/juizes.py:1372-1387`), consulta das perguntas (`app/chamados/models.py:3356-3361`), reconciliador (`app/chamados/reconciliador.py:637-641`, `1027-1030`), emissão (`app/chamados/services/disputa_emissao.py:769`), resposta tardia (`app/chamados/services/resposta_tardia.py:45`), limbo (`app/chamados/services/limbo_folgas.py:194`), silêncio por ausência (`app/chamados/services/silencio_ausencia.py:40-41`). Esses leitores usam o conjunto que inclui `nunca_bateu`; não o absolvem por essa palavra.
- Retenção/exibição: `app/folha/export.py:1233-1255`; `app/folha/services/vigia_pausa.py:157`; `app/folha/previa.py:94`; `app/relatorios/dia_da_frota.py:51-60`; `app/escala/services/leitor_celula.py:424`; `app/pautas/services.py:422`; `app/ponto/services/diagnostico_escala.py:177`.
- Cadastro/adesão: `app/chamados/services/adesao.py:34`; `app/escala/services/cadastro_realidade.py:181`, `477`, `618`; `app/escala/services/propositor.py:474`; `app/escala/regua_defesa.py:688`.
- Decisão humana e proteção: `app/colaboradores/services/calendario.py:411-416`; `app/ponto/services/veredito_celula.py:109`; `app/ponto/portas/celula.py:429`, `816`; `app/colaboradores/services/pos_vinculo.py:61`.
- Vigias: `app/chamados/services/vigia_fio.py:379`; `app/ponto/services/furos_sem_cobranca.py:61`; `app/ponto/services/furos_vetados.py:61`; `app/ponto/services/pautas_dos_discordantes.py:71`; `app/ponto/services/vigia_ausencia_veredito.py:42`.
- O vigia de furo sem canal só seleciona `furo/cobrado`, deixando `nunca_bateu` fora desse censo, sem declará-lo resolvido (`app/chamados/services/furo_sem_canal.py:35`, `72`).
- Auditorias: `app/chamados/management/commands/auditar_invariantes_chamados.py:198`, `377`, `1438`, `1597`, `1714`, `1802`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58-62`; `app/ponto/management/commands/recusar_ponta_pequena.py:145`; `app/ponto/management/commands/celula_veredito_velho.py:88`, `106`.

Não encontrei outro leitor executável que declare literalmente `nunca_bateu` como “resolvido”; o veto compartilhado e a guarda de impressão do ramo sem adesão são as duas traduções operacionais que impedem avanço apontadas acima (`app/ponto/marcos.py:119`, `161-162`; `app/ponto/services/cartorio.py:1210-1216`).

## 10/11 00:23 · sessao 18185c182d52 · - · - · origem=Ronald (tmux)

Caminhos e guardas encontrados, somente por leitura da cópia:

| Caminho | Comportamento e guarda exata |
|---|---|
| **Vigia de hora** | Cron a cada 10 minutos, `--apply` (`app/config/crons.py:367`). Busca `trabalha=True`, datas hoje e ontem (`app/ponto/services/vigia_de_hora.py:21-23`), exclui desligado/isento (`:117-119`), ontem fora da janela (`:120-121`), sem marco vencido (`:124-126`) e lâmpadas suficientes sem cobrança viva (`:127-129`). Chama `julgar_celula(...teto_de_hoje=True)` (`:131`). **Não exclui diretamente `nunca_bateu` ou `julgada_em` preenchida.** |
| **Cartório, ramo sem adesão** | Adesão significa primeira batida apurável **na vida**, sem filtro de data (`app/ponto/services/cartorio.py:1165-1167`); ramo começa quando `adesao is None` (`:1173`). `imp0` não recebe teto, vencidos ou encerramento (`:1207-1209`); considera em dia se `julgada_em`, impressão igual e qualquer veredito (`:1210`), e pula sem `forcar` (`:1214-1216`). O avanço dos marcos sozinho não altera essa impressão. Esse ramo também não alimenta `out['cods']`; retorna em `:1279`, enquanto a fábrica síncrona exige a data em `out['cods']` (`:997`, `:1007-1008`). |
| **Cartório, ramo com adesão** | Teto entra na impressão (`app/ponto/services/cartorio.py:1359-1365`). A guarda **exclui `nunca_bateu` do pulo** (`:1369`); demais células em dia pulam (`:1373-1375`). Logo, “sem batida no dia” e “sem batida na vida” têm comportamentos diferentes. |
| **Cartório diário** | Agenda 06:27, `--apply`, sem `--forcar` (`app/config/crons.py:616`). Universo passado, sem limite temporal inferior (`app/ponto/management/commands/processar_cartorio.py:49-51`, `:61-65`); limite opcional corta colaboradores (`:56-57`). Esbarra nas mesmas guardas do serviço, incluindo o ramo sem adesão. |
| **Sinais de fatos** | Batida julga d−1 e d (`app/ponto/signals.py:108-112`, `:131-140`); ausência (`:143-152`), justificativa (`:161-165`), criação ou mudança de status de chamado (`app/chamados/signals.py:192-193`, `:208-216`). São acionamentos por escrita de fatos. O juiz não julga hoje antes do fim do turno sem `teto_de_hoje` (`app/ponto/services/cartorio.py:1993-1998`), e mantém as guardas de impressão. |
| **Disparar perguntas da competência** | Primeiro universo **somente conta** (`app/chamados/management/commands/disparar_perguntas_competencia.py:109-113`) e alarma (`:247-251`); não chama fábrica para as células contadas. Segundo universo escreve somente para chamado vivo sem pergunta viva (`:141`, `:148-149`), pelo motivo da ata com teto (`:158-161`), chamando `perguntar` (`:271-283`). |
| **Detectar ausências** | Cron de cinco minutos (`app/config/crons.py:364`), hoje/ontem (`app/ponto/management/commands/detectar_ausencias.py:136-138`). Exige marco vencido e dia ainda não encerrado (`:470-473`); chamado existente causa `continue` (`:424-430`). Chama `_abrir_chamado` (`:439`), cujo funil veta `nunca_bateu` (`:630-633`, `app/ponto/marcos.py:119`, `:155-162`). Pode acordar o cartório indiretamente ao criar/mudar chamado, não a cada marco seguinte. |
| **Alertas de turno** | `processar_alertas_turno` pula sem entrada (`app/ponto/management/commands/processar_alertas_turno.py:92-93`). Avançados pula se `turno_aberto_ha` retorna `None` (`app/ponto/management/commands/processar_alertas_avancados.py:63-65`); juiz retorna `None` sem turno aberto ou sem entrada (`app/ponto/turnos.py:3190-3191`). |
| **Detectar intervalo ausente** | Exige entrada inicial e saída final acesas (`app/ponto/management/commands/detectar_intervalo_ausente.py:69-70`); emissão aposentada, `continue` incondicional (`:149-156`). Cron usa `--alarme` (`app/config/crons.py:491`). |
| **Reconciliar grade** | Cron sem `--apply` (`app/config/crons.py:462`); comando declara medição sem escritor (`app/ponto/management/commands/reconciliar_grade.py:3-11`). Eventual bancada emissora só cria chamado, com veto da célula (`:502-504`); caminho de pergunta eliminado (`:531-532`). |
| **Apurar furos diários** | Lê a ata (`app/escala/services/furos_diarios.py:41-43`) e grava `FuroDiario` (`:107-109`); não rejulga nem chama fábrica. Encadeado após cartório (`app/config/crons.py:1509`). |
| **Vigia de entrada faltante** | Apenas lê e imprime (`app/ponto/management/commands/vigia_entrada_faltante.py:23-53`, `app/ponto/services/vigia_entrada.py:78-85`). |
| **Emitir furo retroativo** | **Sob ordem**, não cron (`app/config/crons.py:1396`). Pula sem batida na janela (`app/ponto/management/commands/emitir_furo_retroativo.py:53-56`), exige `FURO_SEM_COBRANCA` (`:111-112`), pula se há chamado (`:148-150`), e delega ao mesmo funil que veta `nunca_bateu` (`:154`; `app/ponto/marcos.py:119`, `:161-162`). |
| **Portas administrativas/cadastro** | Cobrar dia exige passado e força julgamento (`app/chamados/services/cobrar_dia.py:28-30`, `:41`); pós-vínculo força, exceto humano (`app/colaboradores/services/pos_vinculo.py:61-72`); regeneração e reversão forçam nos dias tocados elegíveis (`app/ponto/portas/celula.py:422-434`, `:815-817`). Queda do veto seleciona somente `veredito='furo'`, deixando `nunca_bateu` fora (`app/escala/regua_defesa.py:687-688`). Não são agenda de vencimento de marco. |

**Evento de marco vencido:** o cadastro declara explicitamente que a célula **ainda não agenda o próprio marco**, e atribui ao vigia o papel `prazo` (`app/config/crons.py:2092-2097`, `:2130-2131`). Isso qualifica a regra geral EVENTO/cron do `CLAUDE.md:168-170`.

**Ontem às 00:12:** `disparar` inclui datas `< hoje` (`app/chamados/management/commands/disparar_perguntas_competencia.py:102`), excluindo ontem **somente** quando `_ontem_em_janela` é verdadeira (`:110-111`). O vigia usa a mesma função e exclui quando falsa (`app/ponto/services/vigia_de_hora.py:120-121`). Portanto, os itens de ontem efetivamente contados pela sonda já não estavam na janela do vigia naquele instante; não é prova de que nasceram precisamente às 00:12 [nao verificado].

A janela de ontem devolve falso sem fim de turno (`app/ponto/services/vigia_de_hora.py:49-50`) ou quando `fim + teto < meia-noite` (`:53-54`); mantém até `fim + 2*teto` (`:55-56`), depois depende de `not dia_encerrado(agora - 2*teto)` (`:58`). O próprio comentário registra que, fora disso, `disparar` apenas conta/alarma e `julgada_em` não serve como corte (`:39-43`).

**Limite da atribuição ao dado:** os horários e o veredito fornecidos não demonstram qual ramo de adesão cada célula executa atualmente; falta batida no dia não demonstra ausência de batida na vida [nao verificado].

## 10/11 00:22 · sessao e0bd137fd9b5 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos somente pela cópia:

1. **Não confundir “sem batida no dia” com o ramo `nunca_bateu`.** A seleção desse ramo pergunta pela primeira batida apurável **da vida**, excluindo retratadas: `app/ponto/services/cartorio.py:1165-1173`. Ter veredito `nunca_bateu` na sonda não demonstra que `adesao is None` ainda vale hoje: [nao verificado].

2. **Cartório, ramo sem adesão:** a impressão `imp0` recebe batidas vazias, cobertura, chamados vazios, DNA, regra, parcial e isenção, **sem `teto` nem `enc`** (`app/ponto/services/cartorio.py:1207-1209`). A guarda é:
   ```python
   _em_dia_nb = bool(cel.julgada_em and cel.impressao == imp0 and cel.veredito)
   if _em_dia_nb and not forcar:
       out['pulados'] += 1
       continue
   ```
   `app/ponto/services/cartorio.py:1210-1216`. Esse ramo retorna em `:1279`, antes da lógica normal de teto em `:1324-1344`. Portanto mero avanço dos marcos não altera esse hash nesse ramo (`:1207-1209`).

3. **Fábrica direta no julgamento:** só chama `perguntar_pela_celula` quando a data está no `out['cods']` **deste ato**, não no acumulado do lote: `app/ponto/services/cartorio.py:938-944`, `:992-1008`. A única atribuição de resultado por data do corpo é no ramo normal em `:1437`; o ramo sem adesão termina em `:1279` sem essa atribuição. Assim o ramo sem adesão pode lavrar/carimbar e emitir evento, mas não passa pela fábrica direta nessa estrutura (`:1253-1279`, `:997-1008`, `:1437`).

4. **Cartório, ramo com adesão:** o pulo faz o contrário do que uma leitura superficial sugeriria: `_em_dia` exige `cel.veredito != 'nunca_bateu'`. Logo, nesse ramo, `nunca_bateu` **não** é pulado por impressão igual (`app/ponto/services/cartorio.py:1366-1375`). O teto por quantidade de marcos vencidos entra no hash para hoje e para ontem alcançado pelo vigia (`:1324-1344`, `:1359-1365`); a função de hash acrescenta `teto:N` quando fornecido (`:460-464`). Comentário declara expressamente que isso evita deixar o marco da tarde esperando o cartório seguinte (`:460-462`).

5. **Cron do cartório:** `processar_cartorio` seleciona todas as células passadas da empresa, sem piso temporal, e chama `julgar_colab`; o filtro SQL **não** elimina julgadas nem `nunca_bateu` (`app/ponto/management/commands/processar_cartorio.py:49-65`). `data__lt=hoje` impede pegar o dia corrente (`:50`, `:62`); depois os pulos são os descritos acima. `--forcar` existe e é repassado (`:29`, `:65`); não inferir seu uso em produção: [nao verificado].

6. **Consumidor:** lê apenas eventos pendentes com `tentativas < 5`, não todas as células (`app/chamados/reconciliador.py:1209`, `:1246-1249`, `:1342-1345`). Para as datas do lote ele relê as células atuais e chama a mesma fábrica, sem rejulgar previamente (`app/chamados/reconciliador.py:1384-1387`; `app/chamados/services/fabrica_da_celula.py:279-292`). A execução trabalha em lotes de 500 e prazo de 45 s, podendo continuar no próximo cron (`app/chamados/reconciliador.py:1211`, `:1315`, `:1343`, `:1406-1407`). Sem falhas nem adiamento de órfãs, marca o evento processado mesmo que a fábrica tenha motivos futuros ainda cortados (`:1375-1387`, `:1393-1401`). O adiamento que preserva pendente depende de `_orf['adiadas'] > 0`, não da existência genérica de marcos futuros (`:1375-1378`, `:1396-1399`). Portanto a fila vazia não programa retorno para o próximo marco (`:1249`, `:1343-1345`, `:1401`).

7. **O que às 09h seria perguntável:** o teto é marco + 30 min, comparado por instante (`app/ponto/services/cartorio.py:828`, `:848-859`). `vencidos_no_instante` obtém esse conjunto (`app/chamados/services/fabrica_da_celula.py:35-49`); sem vencidos retorna nenhum motivo (`:58-61`), e exclui cada faltante cujo `(tipo,hora)` não esteja no conjunto (`:68-70`). O mapa é início/E → `saida_sem_entrada`; fim/S → `orfao_14h`; ida do intervalo/S → `intervalo_saida`; volta/E → `intervalo_volta` (`app/chamados/services/declaracao_texto.py:42-46`). Logo marcos posteriores ainda não vencidos não podiam aparecer naquela chamada (`app/chamados/services/fabrica_da_celula.py:68-73`). **Lista histórica efetivamente devolvida às 09h: [nao verificado]**: o anexo não traz o DNA/ata/dedup e retorno daquela execução. Não afirmar que a primeira pergunta nasceu: a sonda diz que hoje não há pergunta do fato, e o ramo especial nem passa pela fábrica direta como explicado acima.

8. **Não foi encontrado emissor de evento “marco venceu”.** O escritor de `EventoCelula` é `emitir_evento_celula`, que só insere identificadores da célula/colaborador/data (`app/ponto/portas/celula.py:896-905`). O mecanismo normal da lavra só emite quando alterou `veredito` ou `ata` (`:892-916`), e lavra idêntica retorna sem evento (`:1027-1030`). Chamadores declarados: lavra, nascimento de chamado de turno, fechamento de turno aberto, mudança de pergunta que solta chamado e semeadura (`app/escala/models.py:1845-1855`). Há ainda reversão explícita do DNA (`app/ponto/portas/celula.py:794-796`) e volta do chamado a vivos (`app/chamados/signals.py:194-207`). `_marco_venceu_apagado` existe, mas é **leitor puro**, compara vencidos entre dois instantes e lê a ata; não emite (`app/ponto/services/cartorio.py:403-423`). O desenho declarado exige evento julgar e consumidor ler a lâmpada; cron de varredura só vigia (`CLAUDE.md:166-170`). O código tem o relógio do vigia como mecanismo explícito de passagem de marco (`app/ponto/services/cartorio.py:825-828`), não um evento temporal independente nesses emissores (`app/ponto/portas/celula.py:896-916`).

9. **Outros gatilhos reativos:** save/delete de batida (`app/ponto/signals.py:131-140`), save/delete de ausência (`:143-152`), decisão/save de justificativa (`:161-165`), nascimento/mudança de status de chamado (`app/chamados/signals.py:187-216`). Não disparam pelo simples passar do relógio; o helper retorna durante julgamento para evitar recursão (`app/ponto/signals.py:85-93`); chamado sem mudança de status retorna (`app/chamados/signals.py:191-193`). A porta avulsa recusa futuro/inexistente (`app/ponto/services/cartorio.py:1985-1989`) e, no dia corrente sem `teto_de_hoje`, espera fim de turno e exige conseguir calculá-lo (`:1993-1998`). A ocorrência de algum desses fatos para cada item da sonda é [nao verificado].

10. **Guarda semântica relevante:** o próprio comentário do ramo sem adesão diz “carimba e não emite” (`app/ponto/services/cartorio.py:1168-1172`), mas ele hoje lavra ata/veredito e carimba (`:1253-1277`), o que pode emitir outbox via `salvar_e_emitir` (`app/ponto/portas/celula.py:892-916`). Outro comentário diz que `nunca_bateu` mantém rejulgamento “para sempre” quando chegar a adesão (`app/ponto/services/cartorio.py:1243-1249`), mas a guarda efetiva do ramo sem adesão aceita qualquer veredito preenchido como em dia (`:1210-1216`). Essa diferença entre os dois ramos deve ficar explícita na resposta; não generalizar o comportamento de um para os 137 registros apenas pelo veredito.

## 10/11 00:19 · sessao bdaf292c04d9 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

O modo `--sonda` executa o envelope antes de entregar o dado raspado ao agente. Não fiz nova execução; abaixo reproduzo integralmente a saída fornecida, até `ERROS 0`, sem incluir o anexo `censo1.r`. (`bin/codex_pergunta.sh:307–319,337–344`) Autenticidade e completude da execução original: [nao verificado].

```text
sonda_leitura: sonda1.py [d18d7cf94f65bba4] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora_local 2026-11-10T00:12:27.212274-03:00
janela 2026-10-21 2026-11-20 piso 2026-10-21
2026-11-10 00:12:29,540 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=266 data=2026-11-09 
2026-11-10 00:12:29,911 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=958 data=2026-11-09 
2026-11-10 00:12:30,492 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=994 data=2026-11-09 
2026-11-10 00:12:31,111 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=833 data=2026-11-09 
2026-11-10 00:12:31,717 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=887 data=2026-11-09 
2026-11-10 00:12:34,485 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=735 data=2026-11-09 
2026-11-10 00:12:35,077 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=89 data=2026-11-09 
2026-11-10 00:12:37,575 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=251 data=2026-11-09 
2026-11-10 00:12:37,731 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=238 data=2026-11-09 
2026-11-10 00:12:37,893 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=107 data=2026-11-09 
2026-11-10 00:12:37,935 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=49 data=2026-11-09 
2026-11-10 00:12:38,680 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=294 data=2026-11-09 
2026-11-10 00:12:39,339 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=556 data=2026-11-09 
2026-11-10 00:12:39,936 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=562 data=2026-11-09 
2026-11-10 00:12:40,954 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=364 data=2026-11-09 
2026-11-10 00:12:41,197 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=118 data=2026-11-09 
2026-11-10 00:12:41,840 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=945 data=2026-11-09 
2026-11-10 00:12:43,430 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=849 data=2026-11-09 
2026-11-10 00:12:43,503 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=301 data=2026-11-09 
2026-11-10 00:12:43,781 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=256 data=2026-11-09 
2026-11-10 00:12:45,555 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1000 data=2026-11-09 
2026-11-10 00:12:46,278 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=190 data=2026-11-09 
2026-11-10 00:12:46,571 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=139 data=2026-11-09 
2026-11-10 00:12:46,618 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=848 data=2026-11-09 
2026-11-10 00:12:46,712 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=575 data=2026-11-09 
2026-11-10 00:12:47,806 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=38 data=2026-11-09 
2026-11-10 00:12:49,023 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=814 data=2026-11-09 
2026-11-10 00:12:49,279 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=850 data=2026-11-09 
2026-11-10 00:12:50,961 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=631 data=2026-11-09 
2026-11-10 00:12:51,168 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=278 data=2026-11-09 
2026-11-10 00:12:51,334 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1001 data=2026-11-09 
2026-11-10 00:12:51,440 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=390 data=2026-11-09 
2026-11-10 00:12:51,553 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=259 data=2026-11-09 
2026-11-10 00:12:51,788 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1006 data=2026-11-09 
2026-11-10 00:12:51,873 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=577 data=2026-11-09 
2026-11-10 00:12:52,191 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=523 data=2026-11-09 
2026-11-10 00:12:53,084 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=493 data=2026-11-09 
2026-11-10 00:12:53,559 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=905 data=2026-11-09 
2026-11-10 00:12:53,766 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=587 data=2026-11-09 
2026-11-10 00:12:54,325 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1012 data=2026-11-09 
2026-11-10 00:12:54,335 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-21 
2026-11-10 00:12:54,347 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-22 
2026-11-10 00:12:54,360 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-23 
2026-11-10 00:12:54,369 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-27 
2026-11-10 00:12:54,381 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-30 
2026-11-10 00:12:54,394 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-03 
2026-11-10 00:12:54,404 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-04 
2026-11-10 00:12:54,417 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-05 
2026-11-10 00:12:54,427 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-06 
2026-11-10 00:12:54,439 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-09 
2026-11-10 00:12:54,451 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-22 
2026-11-10 00:12:54,460 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-24 
2026-11-10 00:12:54,472 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-26 
2026-11-10 00:12:54,484 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-28 
2026-11-10 00:12:54,494 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-30 
2026-11-10 00:12:54,506 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-01 
2026-11-10 00:12:54,518 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-03 
2026-11-10 00:12:54,528 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-05 
2026-11-10 00:12:54,544 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-07 
2026-11-10 00:12:54,555 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-09 
2026-11-10 00:12:54,818 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=701 data=2026-11-09 
2026-11-10 00:12:55,130 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=258 data=2026-11-09 
2026-11-10 00:12:56,041 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=968 data=2026-11-09 
2026-11-10 00:12:56,172 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=726 data=2026-11-09 
N_ITENS 139 CELULAS 48
POR_MOTIVO {'saida_sem_entrada': 45, 'orfao_14h': 48, 'intervalo_saida': 23, 'intervalo_volta': 23}
POR_DIA {'2026-10-21': 1, '2026-10-26': 1, '2026-11-04': 2, '2026-11-05': 35, '2026-11-06': 18, '2026-11-07': 38, '2026-11-08': 16, '2026-11-09': 28}
POR_EMPRESA {2: 134, 3: 5}
A 0 B 0 sem_evento_pos_dia 135 sem_evento_nenhum 0 com_evento_pendente 0
COMBO(A,B,ev_pos>0) {(False, False, False): 135, (False, False, True): 4}
TRABALHA {'True': 139} VEREDITO {'nunca_bateu': 137, 'indefinida': 1, 'cobrado': 1}
PERG_MESMO_DIA_QUALQUER>0 0
ITENS (colab,emp,dia,motivo,trab,A,B,n_ev,n_ev_pos,pend,perg_dia,julgada,ult_ev):
 col1009 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 07:30:07 ultev=11-07 07:30:25
 col1009 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 07:30:07 ultev=11-07 07:30:25
 col1009 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1009 e2 2026-11-09 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1009 e2 2026-11-09 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1009 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1005 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-07 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-07 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-08 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-08 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-08 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-08 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1005 e2 2026-11-09 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1005 e2 2026-11-09 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1005 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1030 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 15:00:07 ultev=11-09 15:00:24
 col1030 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 15:00:07 ultev=11-09 15:00:24
 col427 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col427 e2 2026-11-07 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col427 e2 2026-11-07 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col427 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col1027 e2 2026-11-04 saida_sem_entrada trab=True A=False B=False ev=2 pos=2 pend=0 perg_dia=0 julg=11-05 16:03:54 ultev=11-05 16:03:54
 col1027 e2 2026-11-04 orfao_14h trab=True A=False B=False ev=2 pos=2 pend=0 perg_dia=0 julg=11-05 16:03:54 ultev=11-05 16:03:54
 col1027 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 06:30:07 ultev=11-06 06:30:17
 col1027 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 06:30:07 ultev=11-06 06:30:17
 col1027 e2 2026-11-08 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 06:30:07 ultev=11-08 06:30:17
 col1027 e2 2026-11-08 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 06:30:07 ultev=11-08 06:30:17
 col268 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-07 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-07 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col268 e2 2026-11-09 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col268 e2 2026-11-09 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col268 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col1019 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 06:30:09 ultev=11-05 06:30:21
 col1019 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 06:30:08 ultev=11-07 06:30:17
 col1019 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 06:30:08 ultev=11-07 06:30:17
 col1019 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 06:30:08 ultev=11-09 06:30:19
 col1019 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 06:30:08 ultev=11-09 06:30:19
 col1016 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 18:30:09 ultev=11-05 18:30:28
 col1016 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 18:30:09 ultev=11-05 18:30:28
 col1016 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 18:30:08 ultev=11-07 18:30:29
 col1016 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 18:30:08 ultev=11-07 18:30:29
 col534 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:10
 col534 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=

[truncado em 20000 de 34482 caracteres]

## 10/11 00:18 · sessao b9ec0c8ed08e · - · - · origem=Ronald (tmux)

Censo na base `7b0dbff79`: 31 chamadas externas de `pautas.services.escrever`, mais `responder` interno (`pautas/services.py:194`); criação única em `pautas/services.py:180`. Portas humanas: cabeça em `pautas/views.py:76`, resposta em `pautas/views.py:113` → `pautas/services.py:194`.

Chamadores completos, por arquivo:

| Arquivo | Linhas de chamada |
|---|---|
| `chamados/management/commands/reabrir_orfas_enterradas.py` | 208 |
| `chamados/services/competencia_trancada.py` | 231 |
| `chamados/services/fabrica_da_celula.py` | 270, 360 |
| `chamados/services/passivo_validado.py` | 229, 233, 266 |
| `chamados/services/resposta_tardia.py` | 62 |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py` | 107 |
| `core/management/commands/alarme_arvore.py` | 50 |
| `core/management/commands/alarme_esteira.py` | 54 |
| `core/services/crons_quebrados.py` | 71 |
| `ferias/management/commands/pautas_de_ferias.py` | 167 |
| `folha/management/commands/pre_fechamento.py` | 110 |
| `pautas/management/commands/pauta_dia_em_aberto.py` | 127, 139, 142 |
| `pautas/management/commands/pauta_folga_nao_definida_exportada.py` | 134, 139 |
| `pautas/management/commands/pauta_pago_x_devido_exportada.py` | 207, 211, alias `_ps.escrever` |
| `pautas/semanal.py` | 276 |
| `pautas/views.py` | 76, alias `sv.escrever` |
| `ponto/services/afastado_avisa.py` | 74 |
| `ponto/services/ausencia.py` | 1055, 1245 |
| `ponto/services/autorizacao_he_periodo.py` | 244 |
| `ponto/services/cobertura.py` | 333 |
| `ponto/services/pautas_do_esmeril.py` | 76 |
| `ponto/services/pautas_dos_deslogados.py` | 77 |
| `ponto/services/pautas_dos_discordantes.py` | 177 |

Além das cabeças, escrevem filhas: `pautas/services.py:194`, `chamados/services/passivo_validado.py:233`, `pautas/management/commands/pauta_dia_em_aberto.py:127,142`, `pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`, `pautas/management/commands/pauta_pago_x_devido_exportada.py:211`, `ponto/services/autorizacao_he_periodo.py:244-246`.

AST encontra **26** keywords `texto__startswith`, todos sobre Pauta, em produção. Tabela de famílias (linhas da base):

| Marcador / dono | Escritor | Leitor | Cabeças somente? |
|---|---|---|---|
| `PAUTA_LIMBO_EXPORTADA`, `chamados/services/fabrica_da_celula.py:239` | mesmo:270,275 | mesmo:254 | Sim, `pai__isnull=True` em 253 |
| `PAUTA_SEM_TURNO`, mesmo:296 | mesmo:360,363 | mesmo:305 | Sim, 304 |
| literal `[TRANCA]`, `chamados/services/competencia_trancada.py:218,223` | mesmo:231 | mesmo:218 | **Não** |
| `MARCADOR` `[ORFA-ENTERRADA]`, `chamados/management/commands/reabrir_orfas_enterradas.py:32` | mesmo:208 | mesmo:202 | **Não** |
| `MARCADOR` `[PASSIVO-VALIDADO]`, `chamados/services/passivo_validado.py:44` | mesmo:229,233 | mesmo:164, via `cabeca` em 159 | Sim, 164 |
| `MARCADOR_FORA` `[PASSIVO-VALIDADO-FORA]`, mesmo:48 | mesmo:266 | mesmo:256, via `cabeca` em 254 | Sim, 256 |
| `MARCADOR` `ARVORE VERMELHA`, `core/management/commands/alarme_arvore.py:11` | mesmo:50,53 | mesmo:16 | **Não** |
| `MARCADOR` `ESTEIRA`, `core/management/commands/alarme_esteira.py:10` | mesmo:54,56 | mesmo:15, compõe motivo `chave` | **Não** |
| `MARCADOR` `[CRON QUEBRADO]`, `core/services/crons_quebrados.py:15` | mesmo:71 | mesmo:54, compõe cron/empresa | **Não** |
| `MARCADORES`, três valores `[FERIAS/PA-VENCIDO]`, `[FERIAS/PA-VENCENDO]`, `[FERIAS/CADASTRO]`, `ferias/management/commands/pautas_de_ferias.py:30-34` | mesmo:167, usa 149,159 | mesmo:156 | **Não** |
| `MARCADOR` `[PRE-FECHAMENTO]`, `folha/management/commands/pre_fechamento.py:29` | mesmo:110 | mesmo:92,98, segundo usa data `de_hoje` em 93 | **Não**, ambos |
| `MARCA` `[DIA EM ABERTO]`, `pautas/management/commands/pauta_dia_em_aberto.py:16` | mesmo:139; filhas 127,142 | mesmo:108 | **Não** |
| `MARCA` `[FOLGA NAO DEFINIDA - EXPORTADA]`, `pautas/management/commands/pauta_folga_nao_definida_exportada.py:28` | mesmo:134; filhas 139 | mesmo:126 | Sim, 125 |
| `MARCA` `[PAGO X DEVIDO - EXPORTADA]`, `pautas/management/commands/pauta_pago_x_devido_exportada.py:41` | mesmo:207; filhas 211 | mesmo:243,250, chave nasce em 54 | Cabeça: sim em 242; 250 lê `ja.respostas` **intencionalmente** |
| `[A6/REVISAO-SEMANAL]`, `ponto/services/vigia_ausencia.py:28` | chama semanal em 239 → `pautas/semanal.py:276` | `pautas/semanal.py:199,228` | **Não**, ambos |
| `[SUP/REVISAO-SEMANAL]`, `escala/services/fase_12x36.py:237` | chama semanal em 277-278 → `pautas/semanal.py:276` | `pautas/semanal.py:199,228` | **Não**, ambos |
| `TEXTO[:40]`, `ponto/services/afastado_avisa.py:11` | mesmo:74,78 | mesmo:65 | Sim, 63 |
| local `marcador` `[A4/SEM-DOCUMENTO]`, `ponto/services/ausencia.py:1038` | mesmo:1055,1060 | mesmo:1041 | **Não** |
| local dinâmico `marcador` `[A1-FERIAS/AG#%s]`, mesmo:1228 | mesmo:1245,1250 | mesmo:1231 | **Não** |
| `MARCADOR_PAUTA` `[HE-AUTORIZADA-EM-EXPORTADA]`, `ponto/services/autorizacao_he_periodo.py:124` | mesmo:244 | mesmo:242 | Sim |
| `MARCADOR_A_CONFIRMAR` `[COBERTURA-A-CONFIRMAR]`, `ponto/services/cobertura.py:207` | mesmo:333,337 | mesmo:243 | **Não** |
| `MARCADOR` `[ESMERIL %s]`, `ponto/services/pautas_do_esmeril.py:14` | mesmo:76, usa 49 | mesmo:64, literal `[ESMERIL ` | **Não** |
| `MARCADOR` `[APP-401]`, `ponto/services/pautas_dos_deslogados.py:26` | mesmo:77, usa 48 | mesmo:66 | **Não** |
| `MARCADOR` `[DISCORDANTE]`, `ponto/services/pautas_dos_discordantes.py:37` | mesmo:177, usa 93 | mesmo:166 | **Não** |

`vivas()` e `nao_canceladas()` **não** filtram pai (`pautas/models.py:57-64`). Portanto não há exclusão implícita de respostas nos leitores assinalados “Não”.

Dois escritores do censo não têm leitor `texto__startswith`: `[RESPOSTA-TARDIA]` em `chamados/services/resposta_tardia.py:65` e `APARELHO COMPARTILHADO` em `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:99`.

Semânticas que precisam sobreviver:

- `PAGO` usa marcador + colab + competência (`pautas/management/commands/pauta_pago_x_devido_exportada.py:54`); a cabeça exige espaço depois da chave em 243; o leitor de filhas exige ` > ` em 250. Ainda há leitor Python `p.texto.startswith(MARCA)` na reversão em 277.
- `PASSIVO` e `PASSIVO_FORA` são famílias distintas; o comentário explica que misturá-las falseia idempotência (`chamados/services/passivo_validado.py:245-248`).
- `ESTEIRA` distingue motivo por `'%s %s:'` (`core/management/commands/alarme_esteira.py:15`).
- `CRON` distingue nome/empresa (`core/services/crons_quebrados.py:54`).
- `ESMERIL` usa prefixo aberto `[ESMERIL ` e depois faz parsing da ação por posição (`ponto/services/pautas_do_esmeril.py:64-65`), enquanto a constante é template `[ESMERIL %s]` em 14.
- `DISCORDANTE` tem leitor Python adicional em `ponto/services/pautas_dos_discordantes.py:98`, seguido de parsing em 100-101.
- `afastado` reserva só os primeiros 40 caracteres de uma frase normal, não uma tag (`ponto/services/afastado_avisa.py:65`).
- `A1-FERIAS` é família dinâmica por agendamento (`ponto/services/ausencia.py:1228`).
- `escrever` remove espaços externos antes de persistir (`pautas/services.py:164`); prefixo com espaço inicial humano acaba normalizado na base. Marcador no meio não casa com `startswith` por definição dos lookups existentes.
- Os leitores usam `startswith`, não `istartswith` (26 sítios enumerados); o comportamento efetivo de collation do banco está `[não verificado]`. A normalização de caixa exigida para recusa pode ser aplicada só à guarda humana, conservando leitores. [nao verificado]
- Responder cria filha, mantém âncora e inverte destinatário para usuário autor ou departamento (`pautas/services.py:192-201`). Isso pode afastar respostas humanas dos leitores que exigem departamento, mas não equivale a filtro de pai. Sem alterar leitores sem filtro, a afirmação “toda resposta está fora do juiz” seria falsa.

Legado: é possível enumerar **candidatos** por prefixo + pai nulo, mas usuário não prova origem: `autor_sistema` recua a superuser (`pautas/services.py:89-93`); `Pauta.autor_user` só grava usuário (`pautas/models.py:102-106`). A trilha de criação grava remetente/remetente informado, sem canal humano/sistema (`pautas/services.py:186-188`, `_trilha` em 369-373). `LogAuditoria` oferece IP/ator (`core/models.py:302-307`), que poderia ajudar numa investigação autorizada, sem garantir classificação completa. Quantidade e identidade dos casos em produção: `[não verificado]`.

Comandos usados para o censo:

```bash
git grep -n -E 'pautas.services|texto__startswith|escrever\(' -- '*.py' ':!*/tests/*'
```

Varredura AST somente leitura executada com `PYTHONDONTWRITEBYTECODE=1 python3`, sobre `git ls-files '*.py'`, excluindo testes/migrations; enumerou os 26 keywords e validou os imports/chamadas diretos mais aliases. Nenhum arquivo alterado. [nao verificado]

## 10/11 00:14 · sessao d84e0a927d5d · - · - · origem=Ronald (tmux)

Para os **4 itens residuais** do dado — col1027/2026-11-04/`saida_sem_entrada`, col1027/2026-11-04/`orfao_14h`, col155/2026-10-26/`orfao_14h`, col982/2026-10-21/`orfao_14h` — a contagem é **4 itens, 3 células**; identificação proveniente somente do texto fornecido. [nao verificado]

Sugiro três feições separadoras, sem escolher cura:

1. **Estado e instante do consumo:** `processado_em`, `tentativas`, `erro`, comparados com o vencimento real do marco. `pend=0` não distingue processado de morto: a fila viva exige `processado_em IS NULL` e `tentativas<5`; falhas incrementam tentativas, enquanto processamento sem falha nem adiamento grava `processado_em`. O vencimento é marco do turno +30 minutos, não simplesmente mudança do dia civil. (`app/escala/models.py:1857–1863`; `app/chamados/reconciliador.py:1209,1246–1255,1392–1401`; `app/ponto/services/cartorio.py:828,831–859`.) Os valores necessários não aparecem no dado. [nao verificado]

2. **Elegibilidade no instante da execução:** motivos selecionados pela fábrica no consumo/julgamento, associados à ata, ao veredito, à situação do colaborador e ao teto daquele instante. O consumidor relê a célula atual da data, sem payload histórico; a seleção pode devolver vazio pelas guardas e o evento pode ser marcado processado. Isso separaria “houve execução quando ainda não devido/elegível” de “houve seleção devida que não virou pergunta”. (`app/escala/models.py:1848–1856`; `app/chamados/services/fabrica_da_celula.py:58–66,105–118,198,208–211,288–292`; `app/chamados/reconciliador.py:1387–1401`.) O estado histórico e a versão executada não estão demonstrados. [nao verificado]

3. **Resultado após a seleção:** data efetivamente deduplicada, identidade da linha bloqueadora e resultado/recusa da porta `PerguntaDisputa.abrir`. O pré-dedup usa disputa/motivo/data, enquanto a porta pode recusar destino incompatível ou violação do contrato de saída; a fábrica devolve motivos selecionados e ignora a lista efetivamente criada. Isso separaria “selecionou mas bloqueou/recusou” de “nem chegou à seleção devida”. (`app/chamados/services/disputa_emissao.py:395–424,444–452`; `app/chamados/models.py:2818–2822,2839–2840,2888–2890`; `app/chamados/services/fabrica_da_celula.py:165–167,208–211`.) Nenhuma dessas ocorrências individuais foi fornecida. [nao verificado]

Dois cuidados adicionais:

- O pulo por impressão igual **exclui explicitamente `nunca_bateu`**; portanto não se pode usar esse mecanismo genericamente para explicar o grupo que contém 137 itens com esse veredito agregado. (`app/ponto/services/cartorio.py:1369–1375`.) A associação do veredito a cada linha não foi dada. [nao verificado]
- `criar_ou_atualizar_disputa` só retorna `None` normalmente quando `motivos` está vazio; a fábrica passa `[motivo]`. Disputa existente e motivo repetido continuam chamando o gerador. Esse retorno não constitui explicação normal dos quatro residuais. (`app/chamados/services/disputa_emissao.py:656–682`; `app/chamados/services/fabrica_da_celula.py:161–167`.)

## 10/11 00:13 · sessao ee39037e9fe3 · - · - · origem=Ronald (tmux)

- **A limbo/rótulo:** `limbo_vai_a_pauta` só responde True quando há motivos, `cel.trabalha is None` e competência entregue; o escritor encaminha pauta e retorna sem perguntar. O vigia conta motivos sem pauta nesse ramo. (`app/chamados/services/fabrica_da_celula.py:205–207,214–222,233–236,253–256`)
- Todos os 139 itens fornecidos têm `trab=True`, portanto não satisfazem o ramo de limbo nessa fotografia; referência da fotografia em arquivo ausente: **[nao verificado]**. Isso não mede a outra variante de A, o retrato anterior à escrita da própria rodada. (`app/chamados/services/fabrica_da_celula.py:219–220`; `app/chamados/management/commands/disparar_perguntas_competencia.py:108–113,289–303`)
- **B NULL antes do fallback:** sem destino único, o derivador devolve `(None,None)`; para os motivos que exigem destino, o gerador procura pergunta usando `_data_ev`, e pode dar `continue` antes de `_dia_p` usar `_data_fallback`. (`app/chamados/services/disputa_emissao.py:238–244,410–424`)
- Os intervalos usam `_data_fallback` diretamente no pré-dedup; o mecanismo B específico de NULL não se transfere a eles. (`app/chamados/services/disputa_emissao.py:26,395–400`)
- **B=False não distingue a variante de identidades divergentes:** o pré-dedup filtra disputa/motivo/dia/conversa/vias, enquanto `q_dos_fatos` também exige coluna colaborador e instância do motivo. Comparar essas colunas é uma feição adicional possível para os residuais. (`app/chamados/services/disputa_emissao.py:395–420`; `app/chamados/models.py:2753–2755`)
- A inserção normal atual deriva/valida colaborador e instância. Estoque incoerente nessa amostra: **[nao verificado]**. (`app/chamados/models.py:3425–3447`)
- Outras feições que poderiam separar residuais: catálogo encontrado; texto/data obtidos; destino aceito; contrato de saída cumprido. Há recusas correspondentes no código, mas ocorrência individual no raspado é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:314–316,344–366,424–430`; `app/chamados/models.py:2818–2822,2839–2840,2888–2890`)
- Não localizei `AGENTS.md`, `sonda1.py` ou `censo1.r` nos nomes pesquisados da cópia; conclusão sobre anexos fora da cópia: **[nao verificado]**.

_3288 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
