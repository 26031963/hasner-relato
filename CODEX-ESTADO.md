# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 14:04** · publicado em **16/11/2026 14:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 14:04 · sessao 8d94586da715 · BUG-PAR-O-PAR · construtor · origem=Code

Cura c2 escrita: contador e U103 consultam o carregador pelo módulo dono; o aviso preserva o contrato existente; `bloqueada_competencia` entrou em `ESTADOS`. ([materializacao.py:259](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:259), [materializacao.py:313](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:313), [validacao.py:58](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/validacao.py:58), [validacao.py:696](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/validacao.py:696))

Na tabela, **D = defeito da cura**. Nenhuma das 13 exigiu inverter a expectativa antiga; a análise individual, com linhas da BASE e da c2, está em [BUG_PAR_C2.md:9](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:9).

| # | Falha | Causa | Classe | Cura — arquivo:linha |
|---|---|---|---|---|
| 1 | Contador das recusadas / IndexError | Loader capturado ignora U60 injetada. | D | `chamados/services/materializacao.py:313` |
| 2 | Aviso das irmãs | `outra(s)` rompe o contrato `1 pergunta(s)`. | D | `chamados/services/validacao.py:696` |
| 3 | K6 falso / retry | Elegibilidade ignora U103 injetada; porta retorna antes do ato. | D | `chamados/services/materializacao.py:259` |
| 4 | 103 elegível no contador | Recorte ignora U163 injetada. | D | `chamados/services/materializacao.py:313` |
| 5 | Situação da irmã | Contador não percorre a PK da fixture. | D | `chamados/services/materializacao.py:313` |
| 6 | 103 devolvida espera | Elegibilidade impede alcançar a devolução. | D | `chamados/services/materializacao.py:259` |
| 7 | Laço / contador de espera | Escrita e contador enxergam bindings diferentes do loader. | D | `chamados/services/materializacao.py:313` |
| 8 | Recusa legítima | Contador ignora a PK declarada dentro163. | D | `chamados/services/materializacao.py:313` |
| 9 | Rótulo 103 no modal | Retorno precoce impede gerar DEVOLUÇÃO. | D | `chamados/services/materializacao.py:259` |
| 10 | Foto com `materializada_por` | Retorno precoce impede gerar foto/evento. | D | `chamados/services/materializacao.py:259` |
| 11 | Trilha não gravada desfaz | Retorno precoce impede alcançar a verificação da trilha. | D | `chamados/services/materializacao.py:259` |
| 12 | Universo/contador F1–F2 | Contador usa o loader capturado e retorna total zero. | D | `chamados/services/materializacao.py:313` |
| 13 | Vocabulário | Estado devolvido pela porta não estava em `ESTADOS`. | D | `chamados/services/validacao.py:58` |

As fixtures deixaram de semear `recusa_motivo`; texto e instante continuam escritos juntos. A catraca registra o deslocamento da mesma consulta de 403 para 404. ([fixture:319](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/tests/test_bug_par_projetado.py:319), [catraca:271](/home/ronald/wt-codex-bug-par-o-par-131207/app/core/tests/test_medida_turno_do_par.py:271))

Resultados locais observados:

- **RED sobre a BASE:** `Ran 5 tests` / `FAILED (failures=5)`. [Registro:66](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:66)
- **Provas sem banco:** `Ran 14 tests` / `OK`. [Registro:102](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:102)
- **Cinco mutantes c2:** mortos por asserção, sem erros. [Registro:107](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:107)
- **Catraca adaptada em memória:** `Ran 35 tests` / `OK`. [Registro:794](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:794)

As tentativas com `--settings=config.settings.ci` pararam antes da descoberta por ausência de `django.core`. Não emitiram `Ran N` nem veredito de testes: [nao verificado]

| Módulo | Resultado |
|---|---|
| `chamados.tests.test_bug_par_projetado` | **[nao rodado]** — [registro:695](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:695) |
| `chamados.tests.test_bug29_30_log_validacao` | **[nao rodado]** — [registro:711](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:711) |
| `chamados.tests.test_bug_par_103` | **[nao rodado]** — [registro:727](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:727) |
| `chamados.tests.test_bug_par_volta` | **[nao rodado]** — [registro:743](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:743) |
| `chamados.tests.test_porta_validacao_pergunta` | **[nao rodado]** — [registro:759](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:759) |
| `core.tests.test_medida_turno_do_par` | **[nao rodado]** via CI — [registro:775](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:775) |

Comandos dos REDs, dos mutantes e de cada módulo CI estão completos em [BUG_PAR_C2.md:36](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:36). Execução conjunta dos cinco mutantes:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.bug_par_c2_mutantes --mutante todos
```

Arquivos tocados: `materializacao.py`, `validacao.py`, `test_bug_par_projetado.py`, `test_medida_turno_do_par.py`, os dois arquivos novos de provas/mutantes c2 e três documentos. A lista com caminhos e linhas está em [BUG_PAR_C2.md:91](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:91).

A matriz **QUEM ESCREVE / QUEM LÊ**, por campo e função alcançados pelo diff, está em [BUG_PAR_C2.md:72](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:72). Resultado integrado dos 13 casos e certificação Code/opus final: **[nao verificado]**.

LEI-AKITA: origem=universo congelado e porta canonica do carregador; testemunha=estado_aplicacao_pergunta e competencia_entregue existentes; RED=cinco provas c2 e mutantes em memoria; quem-mais-le=docs/BUG_PAR_C2.md:72; juizes novos=0; padrao=Single Writer Principle.

## 16/11 14:04 · sessao d2148e39a772 · - · - · origem=Ronald (tmux)

Complemento de amplitude:

| Caminho | Alcança célula-dia futura além da competência corrente? | Evidência |
|---|---|---|
| **Espelho do admin (`espelho_do_colab`)** | A janela visual normal termina em hoje: autoridade e montagem dos dias recebem `hoje` como fim. Há uma leitura auxiliar da semana seg–dom no cabeçalho para escala semanal, que pode alcançar datas futuras da semana. | `app/ponto/services/espelho.py:1225`, `app/ponto/services/espelho.py:1227`, `app/ponto/services/espelho.py:1262`, `app/ponto/services/espelho.py:222`, `app/ponto/services/espelho.py:224` |
| **Cartão/PDF espelho** | Não imprime linhas futuras: chama o espelho com ontem e recorta suas linhas ao período. A coleta interna usa `max(data_ini, ate_ontem(...))`; se todo período pedido for futuro, pode ainda consultar a primeira data futura internamente, sem imprimi-la no conjunto de dias. | `app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:354`, `app/relatorios/pdf_espelho.py:598`, `app/relatorios/pdf_espelho.py:602` |
| **`cartao_pela_celula`** | O helper `orfas_e_isencao_da_ata` aceita qualquer intervalo fornecido, inclusive futuro, e lê ata; não decide trabalho/folga. A parcela “dias em aberto” recebe teto de ontem, mesmo quando o período pedido é futuro. | `app/relatorios/cartao_pela_celula.py:277`, `app/relatorios/cartao_pela_celula.py:286`, `app/relatorios/cartao_pela_celula.py:288`, `app/relatorios/cartao_pela_celula.py:37`, `app/relatorios/cartao_pela_celula.py:41`, `app/relatorios/cartao_pela_celula.py:413`, `app/relatorios/cartao_pela_celula.py:419` |
| **`api/views_core.py`, bloco escala de `/api/me/`** | Só hoje: o endpoint calcula a data local atual e passa a `_escala_de_hoje`; True publica `trabalha_hoje=True` com horários do template. | `app/api/views_core.py:281`, `app/api/views_core.py:282`, `app/api/views_core.py:354`, `app/api/views_core.py:248`, `app/api/views_core.py:249`, `app/api/views_core.py:257` |
| **Painel operacional admin** | Lê células de ontem e hoje; previsão é de hoje e “previsto agora” compara os marcos desses dois dias ao instante atual. Não alcança a unidade futura além da competência por essa janela. | `app/core/services/painel_op.py:136`, `app/core/services/painel_op.py:137`, `app/core/services/painel_op.py:149`, `app/core/services/painel_op.py:151`, `app/core/services/painel_op.py:156`, `app/core/services/painel_op.py:231` |
| **Painel situacional `core/views.py`: ausentes** | Só células de hoje; True sem entrada inclui o colaborador em `ausentes_hoje`. | `app/core/views.py:396`, `app/core/views.py:397`, `app/core/views.py:399`, `app/core/views.py:404`, `app/core/views.py:405` |
| **Prévia de fase do cadastro** | Aceita mês/ano futuros, mas a projeção do vínculo proposto vem do template + âncora, não da célula futura existente; a célula é lida apenas para datas anteriores a `desde`, ou para a pergunta “mesma fase” na âncora. | `app/colaboradores/views_fase.py:94`, `app/colaboradores/views_fase.py:95`, `app/colaboradores/views_fase.py:98`, `app/colaboradores/views_fase.py:101`, `app/escala/services/fase_na_tela.py:191`, `app/escala/services/fase_na_tela.py:194`, `app/escala/services/fase_na_tela.py:195`, `app/escala/services/fase_na_tela.py:196` |
| **Sugestão de vigência da fase** | Procura datas até hoje+28, porém calcula fase pelo template; batidas só até hoje. Não lê célula futura nesse cálculo. | `app/escala/services/fase_na_tela.py:19`, `app/escala/services/fase_na_tela.py:109`, `app/escala/services/fase_na_tela.py:110`, `app/escala/services/fase_na_tela.py:112`, `app/escala/services/fase_na_tela.py:113` |
| **Cadastro × Realidade: leitores diretos abertos** | C2 consulta células até hoje; C5 “nunca bateu”, C4 “afastado sem registro” e C11 “folga não definida sem porta” terminam em ontem. Não detectam por essas consultas o resíduo da unidade futura além da competência corrente. | `app/escala/services/cadastro_realidade.py:224`, `app/escala/services/cadastro_realidade.py:226`, `app/escala/services/cadastro_realidade.py:233`, `app/escala/services/cadastro_realidade.py:166`, `app/escala/services/cadastro_realidade.py:167`, `app/escala/services/cadastro_realidade.py:603`, `app/escala/services/cadastro_realidade.py:604`, `app/escala/services/cadastro_realidade.py:553`, `app/escala/services/cadastro_realidade.py:555` |

Não expandi as fontes de lavra de todas as assinaturas de Cadastro × Realidade; o alcance temporal integral dessas fontes é **[não verificado]**. A C10 aqui apenas lê `ultimo_lavrado(hoje)`, e a lista agrega também C7/C8/C9/C12 por outras fontes; logo não usar esta amostra para afirmar que a lista inteira nunca recebe datas futuras. `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:843`, `app/escala/services/cadastro_realidade.py:857`

## 16/11 14:04 · sessao a2fc8078c176 · - · - · origem=Ronald (tmux)

Q2: **REPROVA o censo de carregadores.** O teste só percorre `templates/` e `static/`, em `.html`/`.js`; não acompanha links produzidos em Python (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:278-284`).

Existe um carregador real omitido:

1. O mapa oferece `colaboradores:painel_vinculo` para tipos `escala`, `folga` e `sem_escala` (`app/folha/retrato_folha.py:1004-1006`).
2. `portas_da_linha` resolve essa rota por `reverse` e publica `href` (`app/folha/retrato_folha.py:1066-1075`).
3. A view entrega `retrato_portas` ao painel do colaborador, condicionada a competência, permissão e retrato existente (`app/colaboradores/views.py:996-1016`).
4. O painel inclui `_retrato_o_que_fazer.html`, cujo botão chama `window.abrirDrawer(..., p.href, ...)` (`app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:73`).
5. `abrirDrawer` pede a rota por `fetch`, injeta HTML e processa HTMX (`app/templates/core/_drawer_generico.html:19-33`).

Há entrada pela lista do retrato e pelo Raio-X (`app/templates/folha/_retrato_lista.html:61`; `app/templates/folha/_retrato_raiox.html:68-70`). Logo, o comentário “único carregador” e a lista esperada não representam o grafo real (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:271-285`).

A regex captura o nome `painel_vinculo` terminado por aspa simples e uma concatenação específica com variável `\w+`; não cobre nome com aspas duplas, template literal nem todas as strings montadas (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:276`).

As duas cascas incluem o drawer (`app/templates/base.html:399`; `app/templates/base_app.html:154`). Um fluxo efetivamente alcançável de produto na `base_app` para esse botão: **[não verificado]**; existência de registros/permissões em produção para acionar o caminho: **[não verificado]**.

## 16/11 14:04 · sessao 18084491e621 · - · - · origem=Ronald (tmux)

- **Percorre `carregarPainel` real:** a sonda chama a função diretamente; o HTML vem da view pelo `Client`, e a função executa fetch → wrapper → scripts → `htmx.process`. `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:83`, `:215`, `:240`; `app/templates/colaboradores/vincular.html:143`.
- **Pode ficar verde com clique quebrado:** o teste pula o `onclick` da linha e `abrirPainelVinculo`, que também escreve `_painelAtualCid`. `app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136`; teste `:216`.
- **Pode ficar verde sem processamento HTMX:** a alternância usa `onchange` nativo, e a sonda não envia o formulário; remover `htmx.process` não é diretamente observado pelas asserções. `app/templates/colaboradores/partials/painel_vinculo.html:191`, `:196`; `app/templates/colaboradores/vincular.html:161`; teste `:221`.
- **Visibilidade incompleta:** conta `display` e área, sem exigir `visibility`, `opacity`, ausência de recorte ou oclusão. `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:207`.
- **CSS não comprovado:** o helper aponta para `STATIC_ROOT`, sem validar recursos carregados; só recolhe linhas `:ERROR:CONSOLE`. `app/core/tests/test_smoke_chromium.py:93`, `:124`.
- **MORDE é válido para a alternância:** remove exatamente um `onchange` e exige o calendário inicial repetido nas três medições, resultado incompatível com a asserção principal. `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:243`, `:261`, `:265`. Execução efetiva: [não verificado].

## 16/11 14:03 · sessao 2675e78cc9e0 · - · - · origem=Ronald (tmux)

Censo de perguntas, cobranças e crons:

- **Célula futura `trabalha=True` não gera automaticamente pergunta de falta:** a fábrica chama `dia_sem_trabalho` e retorna `[]` para `cel.data > hoje`; antes de perguntar exige marcos vencidos por instante e ata com faltantes. Fontes: `app/chamados/services/fabrica_da_celula.py:110-121`, `app/chamados/services/fabrica_da_celula.py:52-73`.
- **Cartório avulso recusa futuro:** `julgar_celula` retorna `None` se `data > hoje`; para hoje com `trabalha is not False`, sem `teto_de_hoje`, espera fim do turno. Fontes: `app/ponto/services/cartorio.py:2025-2038`.
- **Cartório diário não lê futuro:** as duas consultas do lote usam `data__lt=hoje`; lê as células passadas e delega para `julgar_colab`. Fonte: `app/ponto/management/commands/processar_cartorio.py:49-65`.
- **Vigia de hora não alcança futuro:** candidatas são células `trabalha=True` de hoje e ontem; ontem precisa estar na janela do turno; sem marco vencido pula; julga com `teto_de_hoje=True`. Fontes: `app/ponto/services/vigia_de_hora.py:20-23`, `app/ponto/services/vigia_de_hora.py:123-134`.
- **`detectar_ausencias` examina hoje e ontem:** lê `ec.eh_dia_trabalho(data_ref)`; não há futura na lista de passadas. Fonte: `app/ponto/management/commands/detectar_ausencias.py:137-145`.
- **Vigia de perguntas da competência só conta passado:** consulta `data__lt=hoje`; aplica `motivos_que_o_evento_deixou` e exclui ontem ainda na janela do vigia. Fonte: `app/chamados/management/commands/disparar_perguntas_competencia.py:177-189`.
- **Vigia “furos sem cobrança” só chega até ontem:** exige veredito `furo`, protesto que cobra, início da competência corrente até ontem, colaborador ativo e empresa em rollout. Fonte: `app/ponto/services/furos_sem_cobranca.py:53-63`.
- **Vigia de cadastro futuro alcança futuras da janela passada pelo gerador:** filtra `data__gt=hoje, data__range=(ini,fim)`; compara `trabalha` e DNA com o cadastro da própria geradora; não escreve células. Fontes: `app/ponto/services/vigia_cadastro_futuro.py:24-31`, `app/ponto/services/vigia_cadastro_futuro.py:75-87`. Na execução padrão `gerar_celulas`, a medida recebe `_janela_default(hoje)` e ocorre antes da regeneração de hoje e geração; portanto o resíduo além dessa janela não entra na medida padrão. Fonte: `app/ponto/management/commands/gerar_celulas.py:337-353`.

**O que `trabalha=True` significa para os juízes de cobrança:**

- `dia_sem_trabalho` não consulta FolgaDia: retorna verdadeiro com `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`; ausência parcial mantém cobrança do resto, exceto célula `False`. Portanto `True` + ata ausente não constitui dispensa nesse juiz. Fonte: `app/chamados/juizes.py:445-460`.
- O gate `celula_veta_cobranca` também não consulta FolgaDia: lê apenas `trabalha` e `veredito`; `False` veta como `FOLGA`; `True` depende dos vereditos de veto. Fonte: `app/ponto/marcos.py:154-163`.
- `_celula_furada` lê a célula/ata; com `True` e sem `lampadas` devolve `None` (“não sei”), não `True` (“furada”). Fonte: `app/chamados/juizes.py:491-510`.
- A tarefa do dia consome `grade_da_celula`; sem dispensa e sem instâncias, se nenhum marco venceu retorna `nao_venceu`; sem lâmpadas e sem essa confirmação retorna `nao_sei=True, ata_sem_lampadas`; instância não vencida também retorna `nao_venceu`. Fontes: `app/chamados/services/tarefa_do_dia.py:194-203`, `app/chamados/services/tarefa_do_dia.py:230-240`, `app/chamados/services/tarefa_do_dia.py:282-308`.
- O app consulta perguntas existentes e filtra pelo C6 `pergunta_viva`; divide pendentes por `respondida_em is None` e disputa aberta; badge “Responder DP” conta essas pendentes. Não deriva uma pergunta de `trabalha=True`. Fontes: `app/chamados/services/pergunta_no_app.py:45-61`, `app/chamados/services/pergunta_no_app.py:64-85`, `app/chamados/services/pergunta_no_app.py:100-119`.
- A cobrança manual usa exatamente `consulta_do_app(colaborador)['pendentes']`; permite cobrar quando existem pendentes e nenhum pai tem prazo SLA futuro. Fontes: `app/chamados/services/cobranca.py:45-48`, `app/chamados/services/cobranca.py:75-94`.
- **Limite da conclusão:** o C6 não possui uma recusa geral por `data_evento > hoje` no corpo aberto; pergunta existente sem materialização pode retornar viva. Logo não afirmar que uma pergunta futura preexistente seria necessariamente invisível; existência de tal pergunta no dado fornecido: `[não verificado]`. Fontes: `app/chamados/juizes.py:1403-1495`, especialmente `:1482-1483`.

**Divergência verificável na leitura de FolgaDia:**

- `fatos_do_periodo` lê a célula `True` como `previsto=True`, mas também lê FolgaDia válida para o vínculo do dia; adiciona **folga e trabalho** e escolhe folga pela precedência. Assim pode devolver `era_dia_previsto=True`, `tipo_do_dia='folga'` e trabalho suprimido para a mesma célula. Fontes: `app/ponto/precedencia.py:348-363`, `app/ponto/precedencia.py:379-413`; ordem em `app/ponto/precedencia.py:46`.
- Folga de calendário só vale no vínculo do dia; a seleção percorre todas as FolgaDia da pessoa por dia, não apenas uma linha. Fontes: `app/ponto/precedencia.py:194-214` e `app/ponto/precedencia.py:350-356`.
- O vigia “faltas de hoje sem cobrança” começa por `eh_dia_trabalho(hoje)` e DNA, mas exclui o dia se `fatos_do_dia.tipo_do_dia` não for `trabalho`/`None`; portanto uma FolgaDia válida pode excluí-lo apesar da célula `True`. Fonte: `app/ponto/services/faltas_de_hoje.py:39-62`.
- O funil emissor de falta consulta precedência apenas para veto de **feriado**, depois consulta o gate da célula; não há veto FolgaDia nessa parte do funil. Fonte: `app/ponto/management/commands/detectar_ausencias.py:607-634`.
- `_protesta` do cartório continua usando `trabalha is not False` e marcos não acesos, enquanto a classificação recebe o juiz de dia sem trabalho com ata do julgamento atual. Fontes: `app/ponto/services/cartorio.py:1458-1468`.
- A precedência é lavrada **depois** do julgamento avulso e o comentário declara que descreve o dia, não decide o veredito do cartório. Fonte: `app/ponto/services/cartorio.py:2043-2048`.

**Crons configurados no código, execução efetiva em produção `[não verificado]`:**

- `detectar_ausencias` a cada 5 min; `vigia_de_hora --apply` a cada 10 min: `app/config/crons.py:364-367`.
- `lavrar_foto_folgas --apply` 05:47; `gerar_celulas` 05:50 dependente da foto: `app/config/crons.py:425-426`.
- `reconciliar_grade` 06:20, sem `--apply`; comentário o mantém como sombra: `app/config/crons.py:460-462`.
- `processar_cartorio --apply` 06:27, por empresa: `app/config/crons.py:614-616`.
- `disparar_perguntas_competencia --comp-corrente --alarme` encadeado após apurar furos: `app/config/crons.py:1539`.
- `reavaliar_ausencias_feriado --alarme` 07:02: `app/config/crons.py:608`; comando só mede chamados abertos que precedência considera feriado/folga e alarma, não relavra célula: `app/ponto/management/commands/reavaliar_ausencias_feriado.py:27-45`.

**DECISÕES atuais para consolidação, sem propor lei:**

1. Quem decide dispensa prevista: célula/DNA para gates de cobrança versus FolgaDia válida + precedência para fato vencedor; a mesma combinação `True` + FolgaDia pode ser “previsto trabalho” e “fato folga”. Fontes: `app/ponto/marcos.py:154-163`, `app/ponto/precedencia.py:348-413`.
2. Quem decide quando nasce cobrança/pergunta: relógio do cartório/fábrica, com recusa de futuro e vencimento de marco; `True` sozinho não autoriza perguntar antes do fato. Fontes: `app/ponto/services/cartorio.py:2025-2038`, `app/chamados/services/fabrica_da_celula.py:110-121`.
3. Quem decide o universo do alarme de cadastro futuro: janela do gerador, não todo acervo futuro já existente; além da competência padrão fica fora. Fontes: `app/ponto/management/commands/gerar_celulas.py:344-346`, `app/ponto/services/vigia_cadastro_futuro.py:31`.
4. Quem decide se FolgaDia cala cobrança: o contador de falta de hoje consulta precedência completa; gate emissor consulta só feriado e célula; são portas diferentes com critérios observavelmente distintos. Fontes: `app/ponto/services/faltas_de_hoje.py:61-62`, `app/ponto/management/commands/detectar_ausencias.py:616-634`.

## 16/11 14:03 · sessao d2148e39a772 · - · - · origem=Ronald (tmux)

Censo dos leitores de tela, somente pelo código aberto:

| Leitor | Janela | Resultado para célula `trabalha=True` em dia com FolgaDia |
|---|---|---|
| **Minha Escala HTML** | Hoje e próximos 13 dias. `app/escala/views.py:38` | Lê `EC.eh_dia_trabalho`, que retorna a célula antes da foto; mostra trabalho verde com horários do DNA da célula. `app/escala/views.py:40`, `app/escala/views.py:43`, `app/templates/escala/minha_escala.html:66`, `app/templates/escala/minha_escala.html:76` |
| **API escala/v2 do colaborador** | Hoje e próximos 13 dias. `app/api/views.py:918` | Publica `trabalho=True`; horários vêm **do template**, por `te.marcos_do_dia`, não do DNA. `app/api/views.py:924`, `app/api/views.py:929`, `app/api/views.py:940` |
| **API espelho/v2** | Mês civil solicitado, inclusive futuro. `app/api/views.py:1344`, `app/api/views.py:1347` | No dia sem card/batida, consulta a célula para horários, mas usa D4 para status: pode publicar **`status='folga'` e horários previstos preenchidos ao mesmo tempo**, se a FolgaDia for válida e o dia chegar ao ramo sem lavra do D4. `app/api/views.py:1587`, `app/api/views.py:1589`, `app/api/views.py:1591`, `app/api/views.py:1611`, `app/api/views.py:1615`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:312` |
| **Calendário de ponto do colaborador, exibido ao gestor/admin** | Mês, semana, quinzena, competência ou custom; custom aceita futuro e limita intervalo a 366 dias. `app/colaboradores/services/calendario.py:7`, `app/colaboradores/services/calendario.py:227`, `app/colaboradores/services/calendario.py:234`, `app/colaboradores/services/calendario.py:239` | Lê D4, não usa diretamente `eh_dia_trabalho` para a palavra do dia; em futuro sem batida/lavra com FolgaDia válida, mostra **folga**, apesar da célula True. `app/colaboradores/services/calendario.py:634`, `app/colaboradores/services/calendario.py:638`, `app/colaboradores/services/calendario.py:695`, `app/templates/colaboradores/partials/_calendario_grade.html:106`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:312` |
| **Drawer do colaborador** | Competência anterior e corrente até o fim da corrente, respeitando piso visual; não alcança além do fim da corrente por essa rota. `app/colaboradores/services/drawer.py:115`, `app/colaboradores/services/drawer.py:116`, `app/colaboradores/services/drawer.py:123` | Usa o mesmo `contexto_calendario`, logo o mesmo D4. `app/colaboradores/services/drawer.py:122` |
| **Painel do vínculo: agenda** | Hoje e próximos 13 dias carregados; template mostra os primeiros sete. `app/colaboradores/views.py:280`, `app/colaboradores/views.py:285`, `app/colaboradores/views.py:287`, `app/templates/colaboradores/partials/painel_vinculo.html:217` | True vira trabalho/horário do DNA; havendo AgendaDia, seu conteúdo tem prioridade visual. `app/colaboradores/views.py:289`, `app/colaboradores/views.py:292`, `app/templates/colaboradores/partials/painel_vinculo.html:218`, `app/templates/colaboradores/partials/painel_vinculo.html:230` |
| **Modal da agenda do admin** | Semana navegável por parâmetro, inclusive futura. `app/colaboradores/views.py:2239`, `app/colaboradores/views.py:2250` | Mesmo True e horários DNA; AgendaDia tem prioridade; sem agenda, mostra horário/“Trabalho”. `app/colaboradores/views.py:2256`, `app/colaboradores/views.py:2265`, `app/colaboradores/views.py:2266`, `app/templates/colaboradores/partials/agenda_modal.html:97`, `app/templates/colaboradores/partials/agenda_modal.html:123` |
| **Plano de Folgas do admin** | Mês solicitado, inclusive futuro. `app/escala/views_plano_folgas.py:33`, `app/escala/views_plano_folgas.py:52` | A FolgaDia gravada ganha antes da leitura da célula: mostra seu tipo de folga como gravado; só sem FolgaDia consulta `eh_dia_trabalho`. `app/escala/views_plano_folgas.py:60`, `app/escala/views_plano_folgas.py:70`, `app/escala/views_plano_folgas.py:71`, `app/escala/views_plano_folgas.py:76`, `app/templates/escala/plano_folgas.html:81` |
| **Disponibilidade: voltando de folga** | Hoje até hoje+7. `app/colaboradores/views.py:2903`, `app/colaboradores/views.py:2905` | Exige célula de hoje False; primeiro futuro True vira data de retorno, sem consulta a FolgaDia nessa decisão. `app/colaboradores/views.py:2912`, `app/colaboradores/views.py:2914`, `app/colaboradores/views.py:2916`, `app/colaboradores/views.py:2917` |
| **Semana do posto / mensageria** | Semana recebida por `inicio`; grade consulta célula em toda a semana. `app/escala/services/semana_do_posto.py:126`, `app/escala/services/semana_do_posto.py:132`, `app/escala/services/semana_do_posto.py:85` | Publica `previsto=True`; se não bateu, pergunta a A2 e FolgaDia válida vira `explicado_por='folga'`, não buraco. `app/escala/services/semana_do_posto.py:143`, `app/escala/services/semana_do_posto.py:151`, `app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:157` |

Autoridades que explicam as diferenças:

- **`EC.eh_dia_trabalho` ignora a FolgaDia quando a célula existe:** devolve `cel.trabalha` imediatamente; a foto só participa da aritmética de fallback quando não há célula. `app/escala/models.py:1092`, `app/escala/models.py:1094`, `app/escala/models.py:1096`
- **`EC.marcos_do_dia` lê DNA congelado:** usa `dna.marcos` quando preenchido; sem marcos no DNA, cai no template. `app/escala/models.py:1124`, `app/escala/models.py:1125`, `app/escala/models.py:1131`, `app/escala/models.py:1133`
- **A2 pode dizer folga e manter `era_dia_previsto=True`:** lê o True da célula como previsto, admite a FolgaDia válida como fato, acrescenta trabalho também e ordena pela precedência. `app/ponto/precedencia.py:356`, `app/ponto/precedencia.py:357`, `app/ponto/precedencia.py:363`, `app/ponto/precedencia.py:384`, `app/ponto/precedencia.py:386`, `app/ponto/precedencia.py:396`, `app/ponto/precedencia.py:407`
- **FolgaDia de calendário precisa ser do vínculo do dia; manual vale para a pessoa:** esses são os predicados que determinam “válida” nas linhas anteriores. `app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:237`, `app/ponto/precedencia.py:239`
- **D4 não equivale ao booleano de previsão:** sem batida e sem status lavrado aplicável, faixa A2 `folga` vira status `folga`; com lavra e dia não retido pelo relógio, usa o status persistido. `app/ponto/status_dia.py:286`, `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:312`
- **Grade também aplica overlay FolgaDia válida:** transforma `tipo_dia='folga'`, limpa marcos e órfãs mesmo após montar por célula True; aplicação acontece no fim do builder. `app/escala/utils.py:1128`, `app/escala/utils.py:1149`, `app/escala/utils.py:1151`, `app/escala/utils.py:1152`, `app/escala/utils.py:1844`

Consequência para a redação do censo: a frase raspada “colaborador e admin veem trabalho em dia de folga futura” **não pode ser universalizada para todas as telas desta cópia**, porque Minha Escala/painel de vínculo seguem o True, enquanto calendário D4 e Plano de Folgas seguem a folga válida nos ramos descritos. `app/escala/views.py:40`, `app/colaboradores/views.py:289`, `app/colaboradores/services/calendario.py:638`, `app/ponto/status_dia.py:312`, `app/escala/views_plano_folgas.py:71`

Renderização efetiva do app nativo e telas observadas em produção: **[não verificado]**; aqui há prova do payload das APIs e dos templates HTML citados. `app/api/views.py:935`, `app/api/views.py:1595`, `app/templates/escala/minha_escala.html:62`, `app/templates/colaboradores/partials/_calendario_grade.html:106`

DECISÕES constatáveis para consolidação: horizonte da previsão versus horizonte da atualização pertence aos escritores (outro agente); booleano da célula versus fato FolgaDia na palavra da tela é decidido hoje por `EC.eh_dia_trabalho` versus A2/D4; horários DNA versus template são decididos pelo leitor HTML versus APIs. `app/escala/models.py:1094`, `app/ponto/precedencia.py:384`, `app/ponto/status_dia.py:312`, `app/escala/views.py:43`, `app/api/views.py:929`, `app/api/views.py:1589`

## 16/11 14:02 · sessao 4319b5e98d3c · - · - · origem=Ronald (tmux)

(b) O `gerar_celulas` sem argumentos cria as ausentes entre `min(inícios)` e `max(fins)` das competências correntes de todas as empresas ativas; sem empresa ativa, usa o default do juiz. Portanto, seu horizonte padrão **não é a competência seguinte**, mas a união das correntes. (`app/ponto/management/commands/gerar_celulas.py:290-300`, `:337-350`)

- A data concreta do fim em produção não pode ser extraída sem os cortes das empresas ativas. Com corte 21, a competência que contém 16/11/2026 termina em **20/11/2026**; a aplicação dessa hipótese à produção é [não verificado]. (`app/ponto/janelas.py:10-33`, `:36-63`)
- A CLI aceita `--ini` e `--fim` explícitos, substituindo o default e permitindo criar além da competência corrente. (`app/ponto/management/commands/gerar_celulas.py:332-344`)
- A criação depende de vínculo que cubra a data; o inativo com fim declarado entra somente em dia futuro em que o juiz o nomeia dono. (`app/ponto/portas/celula.py:883-917`)

Os três pontos de criação encontrados no código de produção são:

| Criador | Horizonte/condição |
|---|---|
| `regenerar_celulas_vinculo` | Percorre `[desde, ate]`, cria ausentes e reescreve geradas divergentes; a extensão até o fim da competência apenas aumenta `ate`, nunca reduz horizonte solicitado maior. (`app/ponto/portas/celula.py:112-119`, `:178-199`, `:226`, `:289-311`) |
| `editar_dia` | Cria célula humana para a data solicitada, se existe vínculo responsável e passam autor, motivo e guarda de competência lavrada; não tem limite superior pela competência corrente. (`app/ponto/portas/celula.py:572-588`, `:597-639`) |
| `gerar_celulas_periodo` | Cria ausentes em todo `[ini,fim]` recebido. (`app/ponto/portas/celula.py:863-886`, `:901-918`) |

O censo executado da própria cópia, `core/censo_escritas.py::varrer_arvore('app')`, também retornou somente esses três `create` atribuídos a `CelulaDia`; seu algoritmo considera construtores, gerentes, apelidos/inferências e exclui testes/migrações, tendo as limitações documentadas para escrita não atribuível. (`app/core/censo_escritas.py:6-55`, `:61-82`; pontos encontrados em `app/ponto/portas/celula.py:293`, `:638`, `:914`)

Um caminho concreto além da competência é ESCALA-AUTO: seu início padrão é o primeiro dia da próxima competência; `_ate` retorna pelo menos esse início ou a última célula já gerada, e a aplicação chama regeneração explícita que cria ausentes. O desfazer também regenera até a última célula existente. Isso demonstra uma origem possível, **não demonstra que criou as células da sonda**. (`app/escala/services/escala_auto_executor.py:24-31`, `:52-58`, `:87-108`, `:143-148`; origem das células da sonda [não verificado])

(c) **Não existe relavra geral de células futuras existentes disparada pela virada de competência no fluxo agendado examinado**: o cron chama `gerar_celulas` diariamente; `_regenerar_hoje_existentes` seleciona somente `data=hoje` e regenera somente `[hoje,hoje]`; o gerador pula as geradas existentes de hoje em diante. (`app/config/crons.py:425-429`, `app/ponto/management/commands/gerar_celulas.py:303-325`, `app/ponto/portas/celula.py:919-935`)

A mudança da janela pode fazer a célula entrar na medição, mas essa medição não a relavra: `medir` filtra futuras na janela, conta divergências, e `lavrar` escreve `MetricaSnapshot`. (`app/ponto/services/vigia_cadastro_futuro.py:24-38`, `:75-87`, `:99-108`)

Há uma ressalva: o cron de foto pode provocar uma relavra futura **como efeito de gravar foto faltante**, não como varredura geral na virada; ele pula vínculo-mês sem `falta`, chama `gerar_foto_folgas` apenas nos alvos e essa escrita chama a relavra. Se a foto já está completa, esse gatilho não existe. (`app/escala/management/commands/lavrar_foto_folgas.py:123-129`, `:163-170`, `app/escala/folgas.py:213-218`, `:298-299`)

(d) `lavrar_foto_folgas` pergunta a `_janela_default(hoje)` e novamente no dia posterior ao fim, cobre os meses civis até o maior fim e assim inclui corrente + seguinte. O motivo registrado é antecipar a foto e tornar o template vazio de novembro visível ainda em outubro, enquanto há tempo para cadastrar; a foto deve chegar antes da célula. (`app/escala/management/commands/lavrar_foto_folgas.py:59-82`, `:24-29`)

O cron está declarado às 05:47 com `--apply`; o gerador vem às 05:50 e depende dele. (`app/config/crons.py:425-426`)

A lavra de foto é aditiva: bloqueia quando há `sobra`, quando não falta foto ou quando a competência está fechada; seu corpo reconsulta a divergência antes de gravar. (`app/escala/management/commands/lavrar_foto_folgas.py:119-129`, `:163-170`)

O que impede tecnicamente a mesma abrangência no suplemento é o seu próprio filtro: ele faz **uma** chamada a `_janela_default(hoje)`, limita o início a amanhã e seleciona existentes apenas em `data__range=(ini,fim)`; os dias selecionados são enviados à porta com `estender_horizonte=False`. (`app/ponto/services/cadastro_futuro.py:26-45`)

O tratamento de feriado também corta as datas na corrente antes de delegar ao mesmo suplemento. (`app/ponto/services/cadastro_futuro.py:57-60`, `:76-78`)

Não há, nesses corpos, recusa técnica genérica da competência seguinte: a porta de regeneração aceita `ate` maior e não o encurta; a janela mais curta nasce no seletor de cadastro. Eventual proibição normativa adicional de usar corrente + seguinte em cadastro_futuro é [não verificado]. (`app/ponto/portas/celula.py:178-199`, `app/ponto/services/cadastro_futuro.py:26-45`)

Outro descompasso verificável é que a sincronização de calendário seleciona células apenas na corrente, mas materializa a foto futura **até o fim dos meses civis envolvidos**; o comentário declara que a foto mensal pode precisar do dia 22 para definir a fase do dia 13. A relavra final continua limitada ao conjunto de células selecionado. (`app/escala/folgas.py:619-642`)

## 16/11 13:56 · sessao f6c24406d92e · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**VEREDITO: REPROVA.** P1/P2 ficaram parcialmente cobertos: o navegador testa o fragmento em um hospedeiro artificial, sem percorrer o carregamento real do painel. O selo pode permanecer verde com esse carregamento quebrado (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:247`; `app/templates/colaboradores/vincular.html:143`).

1. **P1/P2 — a alternância é medida, mas a integração real continua descoberta.** O teste confirma a casca renderizada, dispara `change` e compara o resultado; remover `onchange` produz o estado antigo. Há mordida para esse comportamento isolado (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:237`, `:201`, `:255`, `:262`).

   Porém, inserir o fragmento antes de `</body>` **não representa sua chegada real**:

   - Na página de vínculos, que estende `base.html`, ele chega por `fetch`, entra via `innerHTML` em `.vinc-painel`, tem scripts executados explicitamente e passa por `htmx.process` (`app/templates/colaboradores/vincular.html:1`, `:147`, `:150`, `:156`, `:161`).
   - No drawer, o destino é `#drawer-geral-corpo`; a abertura altera a transformação do ancestral, e o processamento HTMX acontece depois da inserção (`app/templates/core/_drawer_generico.html:8`, `:16`, `:26`, `:33`).
   - O teste usa apenas `<div id="uvc-painel">`, como HTML inicial. Não chama nenhum desses carregadores. Uma regressão que impeça `carregarPainel` de inserir o fragmento não interfere nessa medição (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:240`; `app/templates/colaboradores/vincular.html:143`).

   Os ancestrais também importam: a tela real possui largura limitada, `overflow:hidden` e área de rolagem; o drawer nasce deslocado para fora da tela. A sonda consulta somente `display` do próprio bloco, sem verificar abertura, área ou recorte por ancestrais. Portanto, “bloco com display” não certifica “painel visível e utilizável” (`app/templates/colaboradores/vincular.html:5`; `app/templates/core/_drawer_generico.html:3`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:194`).

2. **A prova da cascata também admite falso verde.** O helper religa os estáticos, mas o teste não confirma que o CSS carregou e descarta os erros retornados pelo navegador. Como a alternância usa `hidden` e um handler inline, o resultado esperado pode sobreviver à ausência da folha externa (`app/core/tests/test_smoke_chromium.py:93`, `:123`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:250`; `app/templates/colaboradores/partials/painel_vinculo.html:196`, `:205`). Carregamento efetivo nesta execução: [nao verificado].

3. **P3 — respondido no ponto levantado.** Exigir HTTP 200 e dois/sete blocos elimina o verde por duas respostas de erro iguais. A medição continua sendo quantidade de queries; bytes são apenas impressos, sem limite ou medição de tempo (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174`, `:177`, `:179`).

4. **P4 — agora confere argumentos, mas não associa cada retorno à renderização.** A asserção exige que as duas chamadas corretas estejam presentes; permite chamadas adicionais. Como todas recebem a mesma sentinela, esse teste isolado aceita chamadas corretas descartadas e renderização alimentada por outra chamada. Os testes de datas acrescentam cobertura, mas a nova asserção não distingue os retornos por calendário/mês (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:150`, `:154`, `:157`, `:107`, `:114`).

Quanto aos descartes:

- **D1:** a linha citada confirma o problema semântico: vazio resolve o padrão do tipo no POST, enquanto a caixa vazia oculta todas as projeções. A fixture não configura padrão no `TipoEscala`, deixando descoberta a combinação “vazio com padrão não nulo” (`app/colaboradores/views.py:236`; `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`). A restrição de alterar somente contexto está registrada; já a afirmação de que qualquer solução necessariamente duplicaria regra ou alteraria o POST não está demonstrada pelas linhas citadas (`app/docs/BACKLOG.md:820`; [nao verificado]).
- **D2:** manter os demais links dentro de blocos ocultos corresponde ao desenho implementado; isso não é, por si, link visível indevido. A alegação de aprovação literal depende do anexo, sem o documento original conferido (`app/templates/colaboradores/partials/painel_vinculo.html:196`, `:205`, `:207`; [nao verificado]).
- **D3:** a projeção reutiliza o mesmo `hoje` da agenda. O código configura São Paulo, mas isso não demonstra a igualdade incondicional alegada entre os dois relógios (`app/colaboradores/views.py:280`, `:302`; `app/config/settings/base.py:93`). Inicialização efetiva de `TZ` e igualdade em produção: [nao verificado].
- **D4:** motivo consistente: a fonte calcula a definição do calendário, e essa é a pergunta registrada para a obra; não pretende mostrar a foto individual (`app/escala/folgas.py:43`, `:54`, `:56`; `app/docs/BACKLOG.md:820`).
- **D5:** motivo consistente como descrição de snapshot: um novo GET recalcula agenda e projeção; trocar a caixa apenas alterna blocos já recebidos (`app/colaboradores/views.py:280`, `:294`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:196`).
- **D6:** “custo constante” só se sustenta como quantidade de queries. Processamento e HTML crescem por calendário; o teste não limita esse crescimento (`app/colaboradores/views.py:299`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:204`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:177`). A medição do log e a extrapolação fornecida não foram revalidadas: [nao verificado].

O mapa dos quatro valores é:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `folgas_projecao` | View consulta a fonte, monta e publica o contexto: `app/colaboradores/views.py:304`, `:308`. | Template percorre calendários, meses e datas: `app/templates/colaboradores/partials/painel_vinculo.html:204`, `:206`. |
| `pode_abrir_calendario` | View consulta `tem_acao(..., 'editar_escalas')`: `app/colaboradores/views.py:309`. | Template condiciona o link: `app/templates/colaboradores/partials/painel_vinculo.html:207`. Destino exige a mesma ação: `app/escala/views.py:315`. |
| `data-folgas-cal` | Template grava o PK: `app/templates/colaboradores/partials/painel_vinculo.html:205`. | Handler compara com a caixa e escreve `hidden`: `app/templates/colaboradores/partials/painel_vinculo.html:196`. Testes leem atributo/conteúdo/display: `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:35`, `:41`, `:194`. |
| `folga_calendario_id`, formulário | Template produz opções e seleção inicial; sonda altera `.value`: `app/templates/colaboradores/partials/painel_vinculo.html:195`, `:200`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:201`, `:203`. | Handler lê a seleção; POST lê o campo e resolve calendário ou padrão: `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/views.py:230`, `:236`. |

O `folga_calendario_id` persistente pertence à FK nullable `EscalaColaborador.folga_calendario`, com `SET_NULL` (`app/escala/models.py:890`).

- **Escritores:** cópia do padrão ao vincular; troca explícita usada pela tela individual e pelo lote; criação de vínculos nos ajustes automáticos. A fixture também grava a FK diretamente (`app/colaboradores/services/vinculo.py:161`, `:284`, `:298`, `:461`, `:1092`, `:1113`; `app/colaboradores/views.py:239`; `app/escala/views.py:409`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78`).
- **Leitores:** seleção/ocultação inicial, retorno do POST e trilha (`app/templates/colaboradores/partials/painel_vinculo.html:200`, `:205`; `app/colaboradores/views.py:237`, `:258`; `app/colaboradores/services/vinculo.py:42`, `:459`, `:471`).
- **Outros consumidores:** universo de vínculos, divergência, vigias, materialização e sincronização futura; lista de usuários do calendário pela relação reversa; diagnóstico de vínculos sem plano; comando de lavra (`app/escala/folgas.py:75`, `:99`, `:127`, `:157`, `:600`, `:627`; `app/escala/views.py:300`; `app/api/views_mensageria.py:1223`; `app/escala/management/commands/lavrar_foto_folgas.py:113`, `:170`).

Execução da suíte e resultados de produção: [nao verificado]. [nao verificado]

## 16/11 13:54 · sessao 1062dd8ea6bd · - · - · origem=Ronald (tmux)

D1–D6, com distinção entre código confirmado e justificativas históricas:

- **D1:** o fato citado está correto: vazio no POST resolve `tipo_escala.folga_calendario_padrao`, e passa o resultado ao escritor (`app/colaboradores/views.py:230`, `:236`, `:239`). A tela, por outro lado, associa vazio à opção “padrão do template” e oculta todos os blocos (`app/templates/colaboradores/partials/painel_vinculo.html:198`, `:205`). A fixture não define calendário padrão no tipo; portanto o caso “vazio com padrão não nulo” permanece descoberto (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`). A restrição “view só contexto” consta do escopo registrado (`app/docs/BACKLOG.md:820`). Ser pré-existente e ser impossível corrigir sem duplicar regra ou violar o aval são justificativas históricas/de desenho, não demonstradas só pelas linhas citadas [nao verificado].

- **D2:** o preload de todos os blocos e a alternância por valor existem (`app/colaboradores/views.py:301`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:196`, `:204`). Os links permanecem nos blocos ocultos quando há permissão (`app/templates/colaboradores/partials/painel_vinculo.html:205`, `:207`). A aprovação literal desse desenho é relatada no anexo; o documento original da proposta/revisão não foi localizado [nao verificado].

- **D3:** `hoje` vem de `date.today()` e alimenta tanto a agenda anterior quanto a projeção nova (`app/colaboradores/views.py:280`, `:285`, `:302`). O fuso configurado é São Paulo, com `USE_TZ=True` (`app/config/settings/base.py:93`, `:95`). Isso sustenta a alegação de reutilização do relógio existente; a afirmação universal de igualdade com `timezone.localdate()` e a inicialização de `TZ` pelo Django não estão demonstradas pelo código da cópia [nao verificado]. Não há evidência fornecida de divergência em produção [nao verificado].

- **D4:** correto quanto à pergunta implementada: a fonte recebe calendário/ano/mês e calcula a definição, sem ler a foto individual (`app/escala/folgas.py:43`, `:54`, `:56`). A view usa exatamente essa fonte (`app/colaboradores/views.py:304`), coerente com o escopo registrado (`app/docs/BACKLOG.md:820`).

- **D5:** correto como descrição de snapshot: agenda e projeção são montadas durante o GET e entregues no mesmo fragmento (`app/colaboradores/views.py:280`, `:294`, `:304`, `:306`). A alternância apenas escolhe os blocos já entregues (`app/templates/colaboradores/partials/painel_vinculo.html:196`). O painel da página de vínculos pode ser recarregado por novo `fetch` (`app/templates/colaboradores/vincular.html:143`, `:147`).

- **D6:** crescimento linear do trabalho e do HTML é confirmado pelos loops por calendário e por mês (`app/colaboradores/views.py:304`; `app/templates/colaboradores/partials/painel_vinculo.html:204`, `:206`). O teste mede igualdade da quantidade de queries entre duas e sete opções; imprime bytes, sem impor limite de bytes, tempo ou processamento (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174`, `:177`, `:179`). “~581 bytes” é aritmeticamente coerente com os números do anexo, mas a execução e `green_modulo.log` não foram verificadas [nao verificado]. “140 calendários” não é contagem de produção demonstrada pelo dado fornecido [nao verificado].

Não identifiquei linha citada que contradiga frontalmente o fato central de D1, D4 ou D5; D2 e D3 contêm premissas adicionais sem prova suficiente, e D6 só sustenta custo constante **em queries**, não custo total (`app/colaboradores/views.py:299`, `:304`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:179`).

## 16/11 13:54 · sessao 18019f974ac0 · - · - · origem=Ronald (tmux)

Censo dos quatro campos/contextos:

| Campo | Escritor | Leitor |
|---|---|---|
| `folgas_projecao` | `painel_vinculo` monta `(calendário, [(mês, dias ordenados)])` consultando a fonte e publica no contexto — `app/colaboradores/views.py:304`, `app/colaboradores/views.py:308`. | Template percorre calendários/meses e imprime datas — `app/templates/colaboradores/partials/painel_vinculo.html:204`, `app/templates/colaboradores/partials/painel_vinculo.html:206`. |
| `pode_abrir_calendario` | `painel_vinculo` consulta `tem_acao(request.user, 'editar_escalas')` — `app/colaboradores/views.py:309`. | Template condiciona o link — `app/templates/colaboradores/partials/painel_vinculo.html:207`. O destino exige a mesma ação — `app/escala/views.py:314`, `app/escala/views.py:315`. |
| `data-folgas-cal` | Template grava o PK de cada calendário no atributo e determina `hidden` inicial pelo FK salvo — `app/templates/colaboradores/partials/painel_vinculo.html:205`. | `onchange` lê o atributo, compara com o valor da caixa e grava `e.hidden` — `app/templates/colaboradores/partials/painel_vinculo.html:196`. Regex do teste lê PK/hidden/conteúdo — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:35`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:41`; sonda lê atributo e `display` — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:193`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:194`. |
| `folga_calendario_id` como formulário | Template produz `select`, marca opção pelo FK salvo e permite vazio — `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/templates/colaboradores/partials/painel_vinculo.html:198`, `app/templates/colaboradores/partials/painel_vinculo.html:200`. Usuário/sonda muda `.value` — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:201`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:203`. | POST lê e resolve PK; vazio resolve `tipo_escala.folga_calendario_padrao` — `app/colaboradores/views.py:230`, `app/colaboradores/views.py:232`, `app/colaboradores/views.py:236`. Sonda seleciona o campo — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:190`. |

`folga_calendario_id` persistente corresponde à FK nullable `EscalaColaborador.folga_calendario`, com `on_delete=SET_NULL` — `app/escala/models.py:890`, `app/escala/models.py:891`, `app/escala/models.py:892`.

Escritores persistentes relevantes:

- `_copiar_folga_padrao` preenche somente vínculo sem calendário, atribui a relação e salva — `app/colaboradores/services/vinculo.py:157`, `app/colaboradores/services/vinculo.py:158`, `app/colaboradores/services/vinculo.py:161`, `app/colaboradores/services/vinculo.py:162`; chamadas após atualizar/criar vínculo — `app/colaboradores/services/vinculo.py:284`, `app/colaboradores/services/vinculo.py:298`.
- `trocar_calendario_de_folga` atribui/salva FK — `app/colaboradores/services/vinculo.py:461`, `app/colaboradores/services/vinculo.py:462`; chamado pela tela individual — `app/colaboradores/views.py:239` — e aplicação em lote — `app/escala/views.py:409`.
- Ajuste automático e desfazimento criam vínculos copiando calendário — `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1092`, `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1113`.

Leitores persistentes relevantes:

- UI: seleção e bloco inicial — `app/templates/colaboradores/partials/painel_vinculo.html:200`, `app/templates/colaboradores/partials/painel_vinculo.html:205`.
- POST: rótulo anterior e JSON de retorno — `app/colaboradores/views.py:237`, `app/colaboradores/views.py:258`.
- Trilha: snapshot e antes/depois da troca — `app/colaboradores/services/vinculo.py:42`, `app/colaboradores/services/vinculo.py:459`, `app/colaboradores/services/vinculo.py:463`, `app/colaboradores/services/vinculo.py:471`.
- Folgas: universo com calendário — `app/escala/folgas.py:75`, `app/escala/folgas.py:79`; divergência — `app/escala/folgas.py:99`; contador de calendário sem datas — `app/escala/folgas.py:126`, `app/escala/folgas.py:129`; materialização — `app/escala/folgas.py:157`, `app/escala/folgas.py:158`; sincronização futura — `app/escala/folgas.py:627`, `app/escala/folgas.py:629`.
- Salvar definição do calendário procura vínculos por essa relação e sincroniza — `app/escala/folgas.py:600`, `app/escala/folgas.py:601`.

A consequência de D1 está no código: mudar a caixa para vazio oculta todos os blocos imediatamente, mas salvar pode persistir o calendário padrão do tipo — `app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/colaboradores/views.py:236`, `app/colaboradores/views.py:239`. A fixture “sem calendário” não configura calendário padrão no tipo, logo não cobre essa combinação — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:58`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:69`.

## 16/11 13:54 · sessao 1d6a3422f25b · - · - · origem=Ronald (tmux)

- **P1/P2 parcialmente atendidos.** O teste executa `change` e mede `display` nas duas cascas, com mutante que remove `onchange`; portanto comprova a alternância do fragmento naquele hospedeiro artificial (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:188`, `:201`, `:255`, `:262`). Não comprova a chegada real: injeta HTML inicial em `#uvc-painel` antes de `</body>` (`:240-250`); a tela real usa `fetch`, `innerHTML`, wrapper `.vinc-painel`, execução dos scripts e `htmx.process` (`app/templates/colaboradores/vincular.html:143-161`). O drawer usa `#drawer-geral-corpo`, abre por transformação e processa HTMX após injeção (`app/templates/core/_drawer_generico.html:3`, `:16-34`).
- **Selo pode ficar verde com integração quebrada.** A sonda só verifica `display` do próprio bloco (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:193-195`); não mede ancestrais, posição, área, clipping, abertura nem processamento do fragmento real. O hospedeiro real limita largura e tem `overflow:hidden`/rolagem, ausentes do wrapper artificial (`app/templates/colaboradores/vincular.html:5-11`; teste `:247`). O requisito local exige duas cascas e afirma que o helper é piso, não teto (`CLAUDE.md:202-203`).
- **Cascata sem garantia de CSS carregado.** O helper apenas liga `STATIC_ROOT` e reescreve URLs (`app/core/tests/test_smoke_chromium.py:93-100`); o novo teste descarta os erros retornados (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:250`). Como `hidden` já funciona pelo navegador e `onchange` é inline, CSS externo ausente pode manter o resultado esperado (`app/templates/colaboradores/partials/painel_vinculo.html:195-205`). Carregamento efetivo em execução: [nao verificado].
- **P3 atendido no ponto relatado.** Exigir HTTP 200 e dois/sete blocos fecha o verde por duas respostas de erro iguais (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174-176`). A métrica continua sendo quantidade de queries: bytes são impressos, sem limite, e CPU/tempo não são medidos (`:177-179`).
- **P4 melhora, mas não fecha o vínculo argumento→retorno→tela.** O teste exige que as duas chamadas corretas estejam presentes no conjunto de chamadas, mas permite chamadas adicionais erradas (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:154-156`). Todas recebem a mesma sentinela, e a comparação agrega datas do bloco, permitindo consultar corretamente sem usar esses retornos e usar retorno de outra chamada (`:150-158`). Não valida quantidade, exclusividade nem correspondência de cada mês/calendário com sua resposta (`:154-158`).
- **Execução dos testes nesta auditoria: [nao verificado].** O próprio caminho de teste cria diretório e arquivos temporários e os apaga; executá-lo contrariaria a restrição somente leitura (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:248-252`; `app/core/tests/test_smoke_chromium.py:94-100`).

## 16/11 13:47 · sessao 25e4b8416f04 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**Parecer: não fecharia esta versão.** O caminho “vazio → calendário padrão” deixa a projeção divergente do calendário salvo; o teste de alternância não executa a interação e o selo novo não cobre as duas cascas. (`app/colaboradores/views.py:235-257`; `app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`; `CLAUDE.md:203`)

Execução dos testes, smoke, números efetivos de queries e confirmação Git do hash/base: **[nao verificado]**. [nao verificado]

1. **Quem escreve, quem lê e significado**

   | Campo/valor | Escritor → leitores |
   |---|---|
   | `folga_calendarios` | A view produz uma lista ordenada, com `tamanhos` prefetched; a própria view percorre essa lista para projetar e o template percorre para montar opções. (`app/colaboradores/views.py:301-307`; `app/templates/colaboradores/partials/painel_vinculo.html:199-200`) |
   | `folgas_projecao` | A view produz pares calendário/meses, contendo datas ordenadas da fonte; o template consome meses, datas e calendário do link. (`app/colaboradores/views.py:302-308`; `app/templates/colaboradores/partials/painel_vinculo.html:204-207`) |
   | `pode_abrir_calendario` | A view chama `tem_acao('editar_escalas')`; o template condiciona o link. O destino exige a mesma ação por um decorator que chama o mesmo juiz. (`app/colaboradores/views.py:309`; `app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/escala/views.py:314-316`; `app/colaboradores/mixins.py:334-346`) |
   | `escala_ativa.folga_calendario_id` | A view carrega a primeira escala `ativa=True`; o template usa o FK para seleção e visibilidade inicial. O POST escreve pela porta `trocar_calendario_de_folga`; a herança inicial também escreve no serviço de vínculo, e operações de cópia preservam o FK. (`app/colaboradores/views.py:268`; `app/templates/colaboradores/partials/painel_vinculo.html:200-205`; `app/colaboradores/services/vinculo.py:151-162`; `app/colaboradores/services/vinculo.py:450-462`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113`) |
   | `FolgaCalendario.modo/celulas/tamanhos` | As portas de salvar/criar calendário escrevem esses dados. A fonte lê `modo`, depois `celulas` ou `dias_por_tamanho()`, que percorre `tamanhos.all()`. Editor e listagem também leem a definição. (`app/escala/folgas.py:571-595`; `app/escala/folgas.py:645-661`; `app/escala/folgas.py:52-56`; `app/escala/models.py:1558-1560`; `app/escala/views.py:292-307`; `app/escala/views.py:362-371`) |
   | `hoje` | É variável local, **não contexto enviado ao template**. Já vinha de `date.today()` para a agenda; agora também determina os dois meses da projeção. (`app/colaboradores/views.py:280-288`; `app/colaboradores/views.py:302-318`) |

   Os meses projetados são **meses civis completos**, inclusive dias anteriores a hoje; a fonte retorna `date`, sem hora. Não há recorte por competência 21–20 nesse caminho. (`app/colaboradores/views.py:302-305`; `app/escala/folgas.py:6-15`; `app/escala/folgas.py:18-36`)

   **Não há uniformidade explícita de fuso:** painel e POST usam `date.today()`, enquanto a porta de troca e o salvamento do calendário usam `timezone.localdate()`. Os settings declaram `America/Sao_Paulo` e `USE_TZ=True`; o fuso efetivo do processo e eventual divergência em produção são **[nao verificado]**. (`app/colaboradores/views.py:243`; `app/colaboradores/views.py:280`; `app/colaboradores/services/vinculo.py:465`; `app/escala/folgas.py:599`; `app/config/settings/base.py:93-95`)

   Há duas interpretações do **vazio**: na apresentação significa “nenhuma projeção”; no POST significa “resolver o padrão do tipo”, que pode ser um calendário concreto. (`app/templates/colaboradores/partials/painel_vinculo.html:196-205`; `app/colaboradores/views.py:230-239`)

2. **Derivação fora da fonte**

   Não identifiquei derivação paralela no trecho novo: a view chama `datas_que_o_calendario_manda` e apenas ordena; o template itera e formata. A escolha entre os dois modos continua dentro da fonte. (`app/colaboradores/views.py:300-305`; `app/templates/colaboradores/partials/painel_vinculo.html:204-206`; `app/escala/folgas.py:43-56`)

   No GET, cada projeção contém todas as datas retornadas para aquele calendário/mês. Ela representa **a definição do calendário**, sem filtrar vigência do vínculo ou consultar a foto efetiva da pessoa; portanto não certifica os dias materializados ou julgados para ela. (`app/colaboradores/views.py:304-305`; `app/templates/colaboradores/partials/painel_vinculo.html:206`; `app/escala/folgas.py:157-158`; `app/escala/models.py:1069-1096`)

3. **Quando fica velha ou divergente; o que o admin vê**

   | Caminho | Resultado pelo código |
   |---|---|
   | Trocar a caixa sem salvar | Mostra imediatamente a projeção e o link do candidato selecionado; o vínculo só muda no POST. Isso atende à prévia do calendário **selecionado**. (`app/templates/colaboradores/partials/painel_vinculo.html:195-207`; `app/colaboradores/views.py:230-239`) |
   | Salvar calendário explícito | Permanece a projeção calculada no GET; a resposta é somente toast. Não há atualização da definição após salvar. (`app/templates/colaboradores/partials/painel_vinculo.html:191-196`; `app/colaboradores/views.py:251-257`) |
   | Salvar vazio, com padrão não nulo | **Falha:** a caixa continua vazia e todos os blocos ficam ocultos, embora o POST tenha salvo o calendário padrão. Novo GET seleciona esse FK e mostra sua projeção. (`app/templates/colaboradores/partials/painel_vinculo.html:196-205`; `app/colaboradores/views.py:235-239`; `app/colaboradores/views.py:251-257`) |
   | Salvar vazio, sem padrão | Continua sem calendário e sem projeção visível, coerente com o valor salvo. (`app/colaboradores/views.py:235-239`; `app/templates/colaboradores/partials/painel_vinculo.html:205`) |
   | Sem escala ativa | Exibe “vincule a escala primeiro”, bloco esmaecido e `pointer-events:none`; ainda calcula todas as projeções, inicialmente ocultas. POST retorna 400. Os controles não recebem `disabled`; comportamento por teclado: **[nao verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:184-211`; `app/colaboradores/views.py:226-229`; `app/colaboradores/views.py:301-305`) |
   | Calendário editado depois | Um painel ainda aberto conserva a definição antiga; alternar a caixa apenas alterna HTML existente. Novo GET recalcula. O editor salva e redireciona à lista; retorno pelo histórico/cache do navegador: **[nao verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/views.py:301-305`; `app/escala/views.py:354-361`) |
   | Virada de mês com painel aberto | Conserva os dois meses do GET; o handler não consulta relógio nem servidor. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/templates/colaboradores/partials/painel_vinculo.html:196`) |

   Na leitura literal de **“sem calendário = sem link”**, também há desvio: com permissão, os links dos calendários oferecidos continuam no HTML, dentro de blocos `hidden`. O teste exige somente ausência de blocos visíveis. (`app/templates/colaboradores/partials/painel_vinculo.html:204-209`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129`)

4. **Duas cascas e raiz da busca**

   O endpoint entrega um fragmento, sem renderizar uma casca. O caminho direto vem de `vincular.html`, que estende `base.html`: a linha chama `abrirPainelVinculo`, depois `carregarPainel`, que busca **`/colaboradores/vincular/<cid>/painel/` a partir da raiz do host**, injeta o HTML e processa HTMX. (`app/colaboradores/views.py:306`; `app/templates/colaboradores/vincular.html:1`; `app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136-161`; `app/config/urls.py:26`; `app/colaboradores/urls.py:12`)

   Existe outro caminho pelo retrato: tipos escala/folga/sem_escala geram a URL do painel e o bloco chama `window.abrirDrawer`. Esse carregador faz `fetch(url)` e está incluído nas duas cascas. Isso demonstra infraestrutura compartilhada; sessão real oferecendo e abrindo o novo painel em `base_app.html`: **[nao verificado]**. (`app/folha/retrato_folha.py:1003-1006`; `app/folha/retrato_folha.py:1066-1075`; `app/templates/folha/_retrato_o_que_fazer.html:73`; `app/templates/core/_drawer_generico.html:11-33`; `app/templates/base.html:399`; `app/templates/base_app.html:154`)

5. **Leis de UI e conduta de front**

   - **L2:** a derivação de datas e a autorização do link usam autoridades existentes. A inconsistência está na resolução do vazio descrita acima. (`app/docs/LEIS-UI.md:23-25`; `app/colaboradores/views.py:304-309`; `app/colaboradores/views.py:235-239`)
   - **L4:** o novo link só é renderizado com `editar_escalas`, mesma autorização exigida pelo destino. (`app/docs/LEIS-UI.md:39-49`; `app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/escala/views.py:315`; `app/colaboradores/mixins.py:342`)
   - **L7/L8 e JS por posição:** o trecho acrescentado não traz ajuda explicativa, `title` cru ou diálogo nativo. O handler busca pelo atributo `data-folgas-cal`, dentro do formulário, sem índice ou posição no DOM. (`app/templates/colaboradores/partials/painel_vinculo.html:195-209`; `app/docs/LEIS-UI.md:77-89`; `app/core/selo_js.py:3-7`)
   - **L1/L3/L6/L9/L10/L11:** o trecho novo não acrescenta contador, classificação de estado, filtro, PDF ou ícone; não identifiquei violação nova dessas leis nesse bloco. (`app/templates/colaboradores/partials/painel_vinculo.html:195-209`; `app/docs/LEIS-UI.md:12`; `app/docs/LEIS-UI.md:31`; `app/docs/LEIS-UI.md:68`; `app/docs/LEIS-UI.md:95`; `app/docs/LEIS-UI.md:120`; `app/docs/LEIS-UI.md:174`)
   - **L5:** o botão existente continua sem indicador/trava, mas duração superior a um segundo é **[nao verificado]**; não há base para declarar nova violação por lentidão. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/docs/LEIS-UI.md:56-60`)
   - **Duas cascas:** o selo novo só pede o fragmento; não renderiza ambas nem prova seu grafo, contrariando a conduta explícita. Smoke de clique e eventual execução de verificações JS: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78-83`; `CLAUDE.md:202-203`)

   O `title` cru da agenda e o emoji de “Ver agenda completa” já existem fora das adições apresentadas; não são introduzidos por esta obra. (`app/templates/colaboradores/partials/painel_vinculo.html:226`; `app/templates/colaboradores/partials/painel_vinculo.html:249`)

6. **O teste morde?** [nao verificado]

   - **Fonte: sim, parcialmente.** Exige sentinela não vazia na saída e ausência do dia antigo. Porém o retorno igual para todas as chamadas e a conversão em conjuntos não verificam argumentos, separação por mês, ordem ou duplicações. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:41-42`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:90-91`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:143-150`)
   - **Alternância: não prova funcionamento.** Verifica estado inicial e uma regex do `onchange`; um handler quebrado contendo o texto esperado pode passar. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`)
   - **Sem calendário:** tem proteção contra ausência total de blocos, mas verifica visibilidade, não ausência literal de links. A fixture tem escala ativa e tipo sem padrão; não alcança o caso vazio com padrão concreto. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:52-75`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129`)
   - **Sem ação:** preserva datas não vazias e exige ausência da URL; existe também teste positivo do link. Testa o consumo de um juiz mockado, sem validar navegação de uma conta real sem permissão. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-103`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:131-140`)
   - **Custo:** isoladamente pode passar com duas respostas iguais de erro/redirect, pois não exige status 200 nem projeções nas respostas medidas. Não fixa teto absoluto. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167`)
   - **Relógio:** a classe está congelada; não depende do dia da execução. O horário escolhido não testa divergência na fronteira de dia/mês entre fusos. RED/GREEN efetivamente executados: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:45`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-108`)

7. **Lista, outros leitores e queries**

   O consumidor do painel apenas itera, portanto não depende de métodos de queryset. O wizard usa o mesmo nome de contexto, mas recebe querysets produzidos por suas próprias views; a mudança local não altera esses produtores. (`app/templates/colaboradores/partials/painel_vinculo.html:199`; `app/templates/escala/_wizard_cal_select.html:4`; `app/escala/views_wizard.py:86`; `app/escala/views_wizard.py:176`; `app/escala/views_wizard.py:197`)

   **Contagem estrutural esperada do carregamento de calendários:** uma consulta de calendários mais uma de `tamanhos` quando há calendários; sem calendários, somente a primeira. O prefetch atende `tamanhos.all()` usado pela fonte. Frente ao queryset anterior das opções, acrescenta uma consulta quando a lista não está vazia. (`app/colaboradores/views.py:299-305`; `app/escala/models.py:1558-1560`; `app/escala/folgas.py:56`)

   A quantidade dessas consultas é constante em relação ao número de calendários, mas há **duas chamadas à fonte por calendário**, além de memória e HTML proporcionais à quantidade oferecida. (`app/colaboradores/views.py:304-305`; `app/templates/colaboradores/partials/painel_vinculo.html:204-209`)

   **Número total efetivo por request: [nao verificado].** O endpoint também executa a agenda existente, com consultas dependentes dos caminhos de células/fotos; o teste fornecido apenas compara dois totais e imprime números cuja saída não foi fornecida. (`app/colaboradores/views.py:283-294`; `app/escala/models.py:1092-1096`; `app/escala/models.py:1122-1123`; `app/escala/models.py:1286-1288`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:165-167`)

8. **Classes que já reprovaram obras**

   - **Segundo juiz/derivação paralela:** não aparece no trecho novo; datas vêm da fonte declarada e a permissão vem do juiz existente. (`app/colaboradores/views.py:300-309`; `app/escala/folgas.py:43-56`; `app/colaboradores/mixins.py:322-342`)
   - **Segundo escritor fora da porta:** não aparece nas adições; elas consultam e montam contexto, e o JS altera somente visibilidade. (`app/colaboradores/views.py:300-309`; `app/templates/colaboradores/partials/painel_vinculo.html:196`)
   - **Relógio fora da borda:** não foi acrescentado; a view reutiliza seu `hoje`. Permanece a diferença de convenção de fuso apontada no item 1. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/colaboradores/services/vinculo.py:465`)
   - **Selo sem mordida suficiente:** presente na promessa de alternância e na verificação isolada de custo; comprovação histórica de RED: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167`)
   - **Campo com dois significados:** presente no vazio da caixa — ausência de projeção na UI versus resolução de calendário padrão no POST — e agravado pela manutenção do HTML após salvar. (`app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/views.py:235-257`)

## 16/11 13:45 · sessao 444ae62b7f9f · - · - · origem=Ronald (tmux)

1. **Produtores/leitores e significado**

- `folga_calendarios`: escrito por `painel_vinculo` como lista ordenada de todos os `FolgaCalendario`, com prefetch de `tamanhos`; lido na própria view para produzir projeções e no template para opções `pk/nome` (`app/colaboradores/views.py:301`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).
- `folgas_projecao`: escrito somente pela view como `(calendário, [(primeiro dia do mês, datas ordenadas da fonte)])`; lido no template para datas e link (`app/colaboradores/views.py:302`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:204`).
- `pode_abrir_calendario`: view chama `tem_acao(user, 'editar_escalas')`; template consome o booleano; destino usa `@acao_required('editar_escalas')`, cujo corpo chama o mesmo `tem_acao`. Não há segundo juiz de permissão neste trecho (`app/colaboradores/views.py:309`, `app/templates/colaboradores/partials/painel_vinculo.html:207`, `app/escala/views.py:315`, `app/colaboradores/mixins.py:342`).
- `escala_ativa.folga_calendario_id`: carregado da primeira escala `ativa=True`, usado para marcar opção e mostrar bloco inicial. Escritor do POST é `trocar_calendario_de_folga`; a herança inicial `_copiar_folga_padrao` também escreve dentro do serviço de vínculo. O novo JavaScript só troca o bloco conforme o valor escolhido, sem escrever banco (`app/colaboradores/views.py:268`, `app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/templates/colaboradores/partials/painel_vinculo.html:200`, `app/templates/colaboradores/partials/painel_vinculo.html:205`, `app/colaboradores/services/vinculo.py:161`, `app/colaboradores/services/vinculo.py:461`).
- `FolgaCalendario.modo/celulas/tamanhos`: campos definidos como modo `dias_mes` ou `semana_dia`, pares de ocorrência semanal e dias por tamanho de mês. Editor chama `salvar_calendario_de_folga`, que grava modo/células e apaga ou atualiza tamanhos; wizard chama a porta `criar_calendario_de_folga`. A fonte lê `modo/celulas`, ou `dias_por_tamanho()` que lê `tamanhos.all()` (`app/escala/models.py:1539`, `app/escala/models.py:1544`, `app/escala/models.py:1558`, `app/escala/models.py:1566`, `app/escala/views.py:354`, `app/escala/folgas.py:584`, `app/escala/folgas.py:593`, `app/escala/folgas.py:645`, `app/escala/folgas.py:654`, `app/escala/folgas.py:54`).
- `hoje` **não é contexto enviado ao template**: variável local da view, escrita por `_dt.date.today()`, usada na agenda e agora na seleção dos dois meses. As datas são civis, sem hora; a fonte produz `date(ano, mes, dia)`. A projeção cobre meses civis completos, incluindo dias anteriores a hoje, exatamente conforme item 1, e não a competência 21–20 (`app/colaboradores/views.py:280`, `app/colaboradores/views.py:285`, `app/colaboradores/views.py:302`, `app/colaboradores/views.py:306`, `app/escala/folgas.py:15`, `app/escala/folgas.py:36`). A competência tem outro contrato explícito (`app/ponto/janelas.py:10`).
- **Fuso não é uniformizado no código**: view e POST usam `date.today()`, mas troca pelo serviço e salvamento do calendário usam `timezone.localdate()`; settings declaram `America/Sao_Paulo` e `USE_TZ=True`. Se o fuso do processo diferir, a virada de mês pode divergir. Configuração real do processo e divergência em produção: **[nao verificado]** (`app/colaboradores/views.py:243`, `app/colaboradores/views.py:280`, `app/colaboradores/services/vinculo.py:465`, `app/escala/folgas.py:599`, `app/config/settings/base.py:93`).
- **O vazio tem duas leituras de interface/ação**: a seleção vazia oculta todas as projeções; no POST, vazio resolve para `ec.tipo_escala.folga_calendario_padrao`, que pode ser calendário concreto. Após sucesso HTMX só há toast, portanto a interface fica vazia embora tenha sido salvo um calendário padrão (`app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/templates/colaboradores/partials/painel_vinculo.html:198`, `app/colaboradores/views.py:230`, `app/colaboradores/views.py:236`, `app/colaboradores/views.py:251`).

2. **Fonte única**

- Não há derivação paralela de folgas no trecho novo: a view chama `datas_que_o_calendario_manda` duas vezes por calendário e só ordena; template só itera e formata. Os dois modos são interpretados exclusivamente pela fonte (`app/colaboradores/views.py:300`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:206`, `app/escala/folgas.py:52`).
- A tela apresenta todas as datas devolvidas para cada mês; não filtra hoje, vigência, foto materializada ou folgas manuais. Isso é coerente com “o calendário manda”, mas não prova os dias efetivamente materializados ou julgados para a pessoa (`app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:206`, `app/escala/folgas.py:158`, `app/escala/folgas.py:184`, `app/escala/folgas.py:187`).
- O relógio continua lido na borda view, e a fonte recebe ano/mês explicitamente; o diff não acrescenta relógio dentro do resolvedor (`app/colaboradores/views.py:280`, `app/colaboradores/views.py:304`, `app/escala/folgas.py:43`).

7. **Outros leitores e custo**

- Outro leitor com o mesmo nome de contexto é `_wizard_cal_select.html`, mas ele recebe seu próprio queryset de `views_wizard`; a mudança local para lista no painel não o afeta. No painel, o leitor só itera, sem métodos de queryset (`app/templates/escala/_wizard_cal_select.html:4`, `app/escala/views_wizard.py:86`, `app/escala/views_wizard.py:176`, `app/escala/views_wizard.py:197`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).
- Custo estrutural do trecho novo: **1 consulta de calendários + 1 prefetch de tamanhos quando há calendários**; sem calendários, só a primeira. Antes, a lista de opções exigia uma consulta. A fonte lê `self.tamanhos.all()` de objetos prefetched, sem `.filter()` que descartaria o cache (`app/colaboradores/views.py:301`, `app/escala/models.py:1558`, `app/escala/folgas.py:56`).
- Constante em número de consultas de calendário para `K>0`; CPU/memória/HTML crescem com `K`, pois há `2K` chamadas à fonte e um bloco por calendário (`app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:204`).
- **Número total medido por request: [nao verificado]**. Não confundir a constância relativa a `K` com custo total fixo: a agenda existente chama `eh_dia_trabalho` até 14 vezes e `marcos_do_dia` nos dias de trabalho, cujos caminhos consultam células e, no fallback, fotos (`app/colaboradores/views.py:287`, `app/colaboradores/views.py:289`, `app/colaboradores/views.py:292`, `app/escala/models.py:1092`, `app/escala/models.py:1122`, `app/escala/models.py:1286`).

## 16/11 13:45 · sessao 8e6f7eb5845c · - · - · origem=Ronald (tmux)

3. Caminhos de atualização:

- **Trocar a caixa sem salvar:** o `onchange` mostra imediatamente o bloco do valor escolhido e esconde os demais. Portanto mostra a prévia do candidato, embora o vínculo persistido continue sendo o anterior até o POST; isso corresponde ao requisito “SELECIONADO”. (`app/templates/colaboradores/partials/painel_vinculo.html:195-207`; `app/colaboradores/views.py:230-239`)
- **Salvar calendário explícito:** o POST chama a porta de escrita; a resposta HTMX só dispara toast, enquanto o formulário usa `hx-swap="none"`. A projeção selecionada permanece aquela calculada no GET. Sem alteração concorrente de definição ou virada de mês, ela continua representando o calendário escolhido; esses eventos concretos em produção são [nao verificado]. (`app/colaboradores/views.py:238-256`; `app/templates/colaboradores/partials/painel_vinculo.html:191-207`)
- **Salvar vazio com padrão não nulo:** há divergência demonstrável pelo código. Vazio oculta todos os blocos, mas o POST transforma vazio em `ec.tipo_escala.folga_calendario_padrao` e grava esse calendário. Como a resposta não repõe o HTML, o admin continua vendo a opção vazia, sem projeção/link, embora o vínculo agora tenha calendário. Novo GET selecionará esse padrão e mostrará seu bloco. (`app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/views.py:235-239`; `app/colaboradores/views.py:251-256`; `app/templates/colaboradores/partials/painel_vinculo.html:200`)
- **Salvar vazio sem padrão:** fica sem calendário e sem bloco visível; nesse caso a seleção exibida corresponde ao valor persistido. (`app/colaboradores/views.py:235-239`; `app/templates/colaboradores/partials/painel_vinculo.html:196-205`)
- **Sem escala ativa:** o GET ainda calcula projeções de todos os calendários, mas nenhum bloco é selecionado; o bloco do formulário aparece esmaecido, com “vincule a escala primeiro” e `pointer-events:none`. Não há `disabled` nos controles; bloqueio efetivo por teclado é [nao verificado]. Um POST nessa condição retorna 400 “sem vinculo de escala ativo”. (`app/colaboradores/views.py:268`; `app/colaboradores/views.py:301-305`; `app/templates/colaboradores/partials/painel_vinculo.html:184-211`; `app/colaboradores/views.py:226-229`)
- **Calendário editado depois do GET:** os dias são uma fotografia no HTML, calculada naquele GET; alternar a caixa só alterna blocos existentes, sem consultar a fonte novamente. O editor grava pela porta e redireciona à lista; um novo GET do painel recalcula, enquanto um painel ainda aberto conserva o HTML anterior. Comportamento de voltar pelo histórico/cache do navegador é [nao verificado]. (`app/colaboradores/views.py:301-306`; `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/escala/views.py:354-361`; `app/escala/folgas.py:571-601`)
- **Virada de mês com painel aberto:** os dois meses ficam aqueles calculados no GET; o `onchange` não avança meses nem recalcula datas. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/templates/colaboradores/partials/painel_vinculo.html:196`)

4. Cascas e carregadores:

- O painel é **fragmento AJAX**, sem `extends`; a view devolve diretamente esse partial. Não há render de uma casca no endpoint. (`app/templates/colaboradores/partials/painel_vinculo.html:1`; `app/colaboradores/views.py:306`)
- O caminho direto confirmado é a página `vincular.html`, que estende `base.html`. A linha chama `abrirPainelVinculo`, que chama `carregarPainel`; este faz `fetch('/colaboradores/vincular/' + cid + '/painel/')`, injeta o HTML e executa `htmx.process(p)`. A URL parte da raiz do host, não do caminho atual. (`app/templates/colaboradores/vincular.html:1`; `app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136-161`)
- A montagem de rotas confirma `/colaboradores/` + `vincular/<cid>/painel/`. (`app/config/urls.py:26`; `app/colaboradores/urls.py:12`)
- Há também porta dinâmica: `PORTA_DO_TIPO` aponta escala/folga/sem_escala para `colaboradores:painel_vinculo`; `portas_da_linha` resolve a URL; o bloco “o que fazer” chama `window.abrirDrawer` com esse endereço. Esse bloco é incluído condicionalmente pelo painel do colaborador. (`app/folha/retrato_folha.py:1003-1006`; `app/folha/retrato_folha.py:1066-1075`; `app/templates/folha/_retrato_o_que_fazer.html:66-74`; `app/templates/colaboradores/partials/painel_colaborador.html:26`)
- O carregador compartilhado `abrirDrawer` faz `fetch(url)`, injeta por `innerHTML` e processa HTMX. Ele existe nas duas cascas por include de `core/_drawer_generico.html`. Isso prova disponibilidade da infraestrutura nas duas raízes; não prova que uma sessão real de `base_app.html` ofereça a porta ou consiga passar pelas permissões. (`app/templates/core/_drawer_generico.html:11-33`; `app/templates/base.html:399`; `app/templates/base_app.html:154`; `app/colaboradores/views.py:264-265`)
- O teste antigo de duas cascas verifica grafo/includes e presença da função, explicitamente sem renderizar uma página inteira. Render/click real do painel novo nas duas cascas: [nao verificado]. (`app/colaboradores/tests/test_retrato_no_drawer.py:259-277`)

## 16/11 13:45 · sessao cb5f0c45d200 · - · - · origem=Ronald (tmux)

5. No trecho novo, não há diálogo nativo, `title`, ícone/emoji ou ajuda explicativa; o JavaScript busca `data-folgas-cal` dentro de `this.form`, sem posição no DOM. Portanto não identifiquei violação nova de L7/L8/L10/L11 ou JS por posição nesse bloco. O `title` cru da agenda já está fora do diff. (app/templates/colaboradores/partials/painel_vinculo.html:195-209; app/templates/colaboradores/partials/painel_vinculo.html:226; app/docs/LEIS-UI.md:77-89; app/core/selo_js.py:3-7)

O link respeita L4 e usa o mesmo juiz de permissão do destino: contexto chama `tem_acao('editar_escalas')`; `acao_required` também chama `tem_acao`, e o destino exige essa ação. (app/colaboradores/views.py:309; app/colaboradores/mixins.py:322-342; app/escala/views.py:314-316; app/docs/LEIS-UI.md:39-49)

O selo novo não renderiza ambas as cascas nem prova o grafo delas: faz GET apenas do partial. Isso não satisfaz a conduta explícita de selo de front nas duas raízes. Smoke de clique do Ronald e RED/GREEN executados: [nao verificado]. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78-83; CLAUDE.md:202-203; app/docs/CONDUTAS.md:50-68)

6. O teste tem mordidas úteis, mas cobertura incompleta: [nao verificado]

- **Fonte:** a sentinela não vazia precisa aparecer na saída e o dia antigo precisa desaparecer; isso detecta derivação própria ignorando o retorno da fonte. Entretanto não confere argumentos/quantidade das chamadas, separação por mês ou duplicação: `_dias` converte em conjunto e `_fonte` une os meses. Um código que trocasse os rótulos dos meses ou repetisse datas poderia continuar passando. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:32-42; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:90-91; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:143-150)
- **Alternância:** confere o estado inicial e a presença de um `onchange` contendo `data-folgas-cal`; nunca executa o handler. Um handler com esse texto e comportamento quebrado pode passar. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120)
- **Sem calendário:** impede vacuidade pela ausência completa de blocos, pois exige blocos oferecidos e nenhum visível; não exige ausência literal de links no HTML, apenas blocos ocultos. A fixture possui escala ativa e tipo sem calendário padrão; não cobre vínculo sem ativa nem opção vazia resolvendo um padrão não nulo. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:52-75; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129)
- **Sem ação:** preserva dias não vazios e exige ausência da URL; existe teste positivo da URL, então o conjunto não se sustenta só em ausência. É teste do contexto com juiz mockado, não teste de uma conta real sem permissão navegando ao destino. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-103; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:131-140)
- **Custo:** compara número de queries entre 2 e 7 calendários, mas não verifica status das duas respostas, presença de projeções ou aumento dos blocos. Pode passar em duas respostas iguais de erro/redirect sem projeção. Também não fixa teto absoluto nem prova tempo/memória constantes. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167)
- **Relógio:** a classe inteira congela 10/12/2026 às 15h; não há relógio solto nesses testes. A constante `HOJE` é declarada, mas a expectativa usa `MESES` e literais. Resultado efetivo da execução: [nao verificado]. (app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:30-31; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:45; app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:90-108)

8. Neste diff não aparece segundo juiz de datas, segundo escritor persistente ou nova leitura de relógio: a view reutiliza `hoje` já lido na borda e chama a fonte única, ordenando o retorno; o template apenas formata datas. Há lacuna real na força do selo de front/custo descrita acima; evidência histórica de RED e smoke é [nao verificado]. (app/colaboradores/views.py:280; app/colaboradores/views.py:300-309; app/templates/colaboradores/partials/painel_vinculo.html:204-207; app/escala/folgas.py:43-56; CLAUDE.md:193-194; CLAUDE.md:202-203)

## 16/11 13:43 · sessao 8c89640a4b0e · BUG-PAR-O-PAR · construtor · origem=Code

**Entrega incompleta: faltam os arrays U60/U103 do anexo `u163.py`.** O JSON não foi inventado; seu teste termina com `FileNotFoundError`. [RELATO.md:5](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:5) [nao verificado]

A cura escrita limita DXG aos PKs congelados, consulta K6 antes das escritas e usa a célula dona na guarda. [materializacao.py:703](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:703), [validacao.py:289](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/validacao.py:289), [materializacao.py:402](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:402)

Arquivos tocados, relativos a `app/`:

```text
chamados/services/universo_bug_par.py
chamados/services/materializacao.py
chamados/services/validacao.py
chamados/services/fio.py
chamados/management/commands/bug_par_103.py
chamados/templatetags/chamados_tags.py
templates/chamados/partials/_pergunta_card.html
templates/chamados/partials/modal_fio.html
chamados/tests/test_universo_bug_par.py
chamados/tests/test_bug_par_volta.py
chamados/tests/test_bug_par_103.py
chamados/tests/test_bug_par_sem_banco.py
chamados/tests/test_bug_par_projetado.py
chamados/tests/test_s139.py
chamados/tests/bug_par_mutantes.py
core/tests/test_medida_turno_do_par.py
docs/JUIZES.md
docs/RELATO.md
```

A entrega detalhada está registrada com referências:

- **Cada RED, `arquivo::teste`, afirmação e mutante:** [RELATO.md:21](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:21). [nao verificado]
- **Comando de cada mutante:** [RELATO.md:46](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:46).
- **Quem escreve e quem lê cada campo/função:** [RELATO.md:64](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:64).
- **Lookup reconciliado:** [JUIZES.md:38](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/JUIZES.md:38).

Medição local, sem certificação: **8 testes OK, 15 mutantes mortos por asserção e 35 testes da catraca OK**. Integração Django, vizinhos O261/O317, dez mutantes de integração e reprodução de 84/19+75 permanecem **[nao verificado]**. [RELATO.md:48](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:48) [nao verificado]

Comandos para a sessão, após fornecer o JSON exato:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_universo_bug_par chamados.tests.test_bug_par_sem_banco chamados.tests.test_bug_par_volta chamados.tests.test_bug_par_103 chamados.tests.test_bug_par_projetado chamados.tests.test_s139 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_o261_vespera_nao_absolve chamados.tests.test_o317_passivo_pela_porta chamados.tests.test_o261_paridade_respeita_a_lampada core.tests.test_medida_turno_do_par chamados.tests.test_e2_tarefa_do_dia chamados.tests.test_e3_portas_tarefa --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.bug_par_mutantes --django --mutante todos
```

LEI-AKITA: origem=universo congelado pendente do anexo + portão da materialização, testemunha=estado_aplicacao_pergunta e competencia_entregue, RED=blocos (a)-(c) + fora163 + 5644, quem-mais-le=docs/RELATO.md:64, juizes novos=0, padrao=Single Writer Principle.

## 16/11 13:25 · sessao 77f8120193e8 · UI-VINCULO-MOSTRA-O-CALENDARIO · leitura · origem=Code

**(a) As duas leituras existem no código:** a foto deve chegar à célula pela geração/regeneração; grade e precedência também leem `FolgaDia` por cima do previsto, sem alterar `CelulaDia.trabalha`. Já `EC.eh_dia_trabalho` devolve o valor da célula existente. Portanto, gravar a foto não garante, por si só, que todos esses leitores responderão igual. (`app/escala/folgas.py:215-218`; `app/escala/utils.py:1147-1153`; `app/ponto/precedencia.py:356-387`; `app/escala/models.py:1087-1096`.)

**Quem escreve `FolgaDia`.** O módulo autorizado é `app/escala/folgas.py`. (`app/core/portas.py:470-473`.)

| Porta | Escrita e entradas |
|---|---|
| `gerar_foto_folgas` | Apaga a foto de calendário do mês, preserva manuais, recria as datas e chama a relavratura na mesma transação. Entradas: vínculo individual, aplicação ao tipo, herança do padrão e comando `lavrar_foto_folgas --apply`. (`app/escala/folgas.py:150-218`; `app/colaboradores/views.py:239-246`; `app/escala/views.py:408-412`; `app/colaboradores/services/vinculo.py:151-166`; `app/escala/management/commands/lavrar_foto_folgas.py:159-170`.) |
| `sincronizar_foto_futura_do_calendario` | Usa o mesmo escritor com recorte de datas futuras e depois regenera futuras. É chamada ao salvar o calendário ou mudar o calendário do vínculo. (`app/escala/folgas.py:597-642`; `app/colaboradores/services/vinculo.py:459-465`.) |
| `lancar_folga` | `get_or_create` manual seguido de relavratura. Entradas: toggle do Plano e declaração pela supervisão. (`app/escala/folgas.py:329-339`; `app/escala/views_plano_folgas.py:124-132`; `app/ponto/services/declarar_dia.py:179-193`.) |
| `alterar_folga` | `_foto_do_dia` apaga, cria ou altera `tipo`; depois relavra. Entrada: toggle do Plano. (`app/escala/folgas.py:343-382`; `app/escala/views_plano_folgas.py:134-137`.) |
| `aplicar_folgas_em_lote` | Usa `_foto_do_dia`, recusa competência anterior à corrente e relavra o intervalo gravado. Entrada: Plano em lote. (`app/escala/folgas.py:401-421`; `app/escala/views_plano_folgas.py:163-185`.) |
| `reparentar_folga` | Muda o vínculo da folga manual e chama relavratura; calendário é recusado. Na competência passada usa `so_rejulgar`, preservando o DNA. Entradas: signal de criação/vigência e comando de reparentamento. (`app/escala/folgas.py:484-525`; `app/escala/signals.py:185-207`; `app/escala/management/commands/reparentar_folgas.py:22-34`.) |

Há ainda efeitos referenciais declarados: excluir o vínculo apaga suas folgas por `CASCADE`; excluir o calendário deixa `calendario_origem` nulo por `SET_NULL`. (`app/escala/models.py:1585-1591`.)

**Quem escreve `CelulaDia.trabalha`.**

- **Geração:** `gerar_celulas_periodo` calcula `valor = ec.eh_dia_trabalho_calculado(d)` e cria a célula com esse valor e DNA. A data presente na foto do próprio vínculo resulta em `False`. Célula existente editada é preservada; passado existente só pode receber DNA ausente; hoje/futuro existente é pulado nessa função. (`app/ponto/portas/celula.py:911-935`; `app/escala/models.py:1280-1288`; `app/escala/models.py:1309-1317`.)
- **Command `gerar_celulas`:** antes da geração, possui suplemento que regenera células existentes de **hoje**. Isso não constitui uma atualização geral das futuras existentes. (`app/ponto/management/commands/gerar_celulas.py:303-325`; `app/ponto/management/commands/gerar_celulas.py:345-350`.)
- **Regeneração:** recalcula pela foto/cadastro; cria ausentes e reescreve existentes divergentes somente com `origem='gerada'`. Guarda DNA anterior, instante e incrementa `regeneracoes`. Também pode desligar células fora da vigência. (`app/ponto/portas/celula.py:229-311`.)
- **Edição humana:** `editar_dia` escreve `trabalha`, DNA, geradora, `origem='editada'`, autor e instante; não incrementa os campos de regeneração. Troca de plantão delega a essa edição. (`app/ponto/portas/celula.py:575-580`; `app/ponto/portas/celula.py:637-647`; `app/ponto/portas/celula.py:670-715`.)
- **Reversão:** `reverter_regeneracao_dia` restaura `trabalha` pelo DNA anterior e incrementa os carimbos de regeneração. (`app/ponto/portas/celula.py:817-837`.)

**Quem lê cada fonte.**

| Consumidor | Fonte efetivamente lida |
|---|---|
| `EC.eh_dia_trabalho` | Existindo célula, retorna `cel.trabalha`; somente sem célula chama o cálculo que consulta a foto. (`app/escala/models.py:1087-1096`.) |
| `EC.marcos_do_dia` | Lê `dna.marcos`; sem marcos utilizáveis, retorna os marcos do template. Não aplica a foto nessa leitura. (`app/escala/models.py:1117-1133`.) |
| Grade/espelho | O overlay consulta `FolgaDia` pelo juiz, põe `tipo_dia='folga'`, limpa as colunas e órfãs **do dicionário da grade**. O espelho obtém seu flag `folga` desse `tipo_dia`. (`app/escala/utils.py:1140-1153`; `app/escala/utils.py:1844`; `app/ponto/services/espelho.py:673-679`; `app/ponto/services/espelho.py:764-771`.) |
| Precedência | Usa `trabalha` como previsto, mas inclui folga lançada válida mesmo com previsto `True`; folga vence trabalho na ordenação. Calendário vale apenas para o vínculo do dia; manual vale para a pessoa. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:235-239`; `app/ponto/precedencia.py:347-397`.) |
| Motor | No caminho com vínculo não intermitente, classifica folga trabalhada pela resposta `False` de `EC.eh_dia_trabalho`; os marcos vêm de `EC.marcos_do_dia`, com fallback ao template. (`app/ponto/motor_calculo_v2.py:2445-2469`; `app/ponto/motor_calculo_v2.py:2290-2291`; `app/ponto/motor_calculo_v2.py:1532-1540`.) |
| Folha | O fechamento monta datas previstas por `EC.eh_dia_trabalho`, alimenta o motor com células e grava os resultados, incluindo FT. O TXT lê os eventos do fechamento. (`app/ponto/services/fechamento.py:724-755`; `app/ponto/services/fechamento.py:918-924`; `app/ponto/services/fechamento.py:965-966`; `app/folha/export.py:439-446`; `app/folha/export.py:725-739`.) |

O dinheiro mostrado no espelho segue outro percurso que seu flag visual de folga: pago/rubricas do dia leem `DiaPago`; totais da competência passam por `folha_manda` e `fechamento_lido`. (`app/ponto/services/espelho.py:805-813`; `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/cartao_pela_celula.py:69-76`; `app/ponto/services/fechamento.py:1759-1761`.)

**Caso das cinco folgas de dezembro.** Para célula já existente, o percurso previsto é:

`gerar_foto_folgas → relavrar_celulas_da_foto → regenerar_futuras_do_cadastro → regenerar_celulas_vinculo → trabalha=False`.

As chamadas estão em `app/escala/folgas.py:215-218`, `app/escala/folgas.py:297-299` e `app/ponto/services/cadastro_futuro.py:43-45`; o cálculo da foto e a escrita estão em `app/escala/models.py:1309-1317` e `app/ponto/portas/celula.py:289-311`.

**O limite concreto encontrado:** a foto solicitada é mensal, mas a relavratura futura seleciona somente células geradas já existentes dentro de `_janela_default(hoje)`, a partir de amanhã, respeitando o vínculo do dia. Essa janela é a união das competências **correntes** das empresas ativas. Não é o mês inteiro pedido a `gerar_foto_folgas`. (`app/escala/folgas.py:180-197`; `app/ponto/services/cadastro_futuro.py:26-45`; `app/ponto/management/commands/gerar_celulas.py:290-300`.)

Logo, datas fora dessa janela não chegam à regeneração por esse percurso. **Que esse filtro tenha excluído precisamente as cinco datas relatadas: `[nao verificado]`.** Data do ato, cortes, origem/geradora das células e bloqueios do caso: `[nao verificado]`. `regeneracoes=0` também não prova ausência de chamada: o incremento ocorre na reescrita divergente, após seleção e guardas. (`app/ponto/portas/celula.py:284-311`.)

**(b) Sim: `gerar_foto_folgas` no mês corrente grava/apaga `FolgaDia` anterior a hoje.** A seleção é pelo mês inteiro; `delete` e `bulk_create` não têm corte por hoje. As manuais são preservadas. (`app/escala/folgas.py:180-197`.)

**A célula passada também pode ser tocada:** a função chama relavratura desde o primeiro dia do mês; o ramo passado encaminha `desde..hoje` à regeneração formal. Essa é precisamente a exceção à irretroatividade prevista na seção 4, sujeita às guardas de vigência, lavra e origem gerada. (`app/escala/folgas.py:212-218`; `app/escala/folgas.py:290-296`; `CLAUDE.md:149-156`; `app/ponto/portas/celula.py:147-162`; `app/ponto/portas/celula.py:284-311`.)

A sincronização **futura** preserva passado, mas as views de vincular/aplicar calendário também chamam depois a geração mensal sem esse recorte. Portanto, a proteção da primeira chamada não protege a segunda. (`app/escala/folgas.py:621-639`; `app/colaboradores/views.py:243-246`; `app/escala/views.py:411-412`.)

**DONO: NAO SEI** — o juiz exige insumos além dos três valores relatados; esses insumos do caso estão `[nao verificado]`. Decisão: `app/ponto/management/commands/e6_oraculo.py:98-110`.

## 16/11 13:24 · sessao 0e5369e6a2cb · UI-VINCULO-MOSTRA-O-CALENDARIO · sonda · origem=Code

# Calendarios de folga em uso com grade vazia (modo dias_mes)

UNIVERSO calendarios dias_mes em uso 72 | com grade vazia 68 | vinculos afetados 77. [nao verificado]

| Calendario (pk) | Vinculos ativos | Grades que faltam | Dias que ja tem (por grade) | Quem usa |
|---|---:|---|---|---|
| 69 | 5 | meses de 30; fevereiro | 31: [4, 11, 18, 25] | col390/ec929, col392/ec923, col468/ec402, col575/ec925, col825/ec926 [nao verificado] |
| 72 | 2 | fevereiro | 31: [1, 8, 15, 22, 25, 29]; 30: [5, 12, 15, 19, 26] | col309/ec1355, col491/ec1271 [nao verificado] |
| 78 | 2 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col921/ec1141, col1026/ec1581 [nao verificado] |
| 87 | 2 | fevereiro | 31: [4, 11, 18, 25]; 30: [1, 8, 15, 22, 29] | col235/ec202, col600/ec944 [nao verificado] |
| 122 | 2 | meses de 30; fevereiro | 31: [1, 2, 5, 6, 7, 8, 9, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 26, 27, 28, 29, 30] | col346/ec1383, col610/ec1420 [nao verificado] |
| 138 | 2 | meses de 31; fevereiro | 30: [1, 8, 15, 22, 29] | col476/ec1451, col864/ec1494 [nao verificado] |
| 39 | 1 | fevereiro | 31: [5, 6, 7, 12, 13, 19, 20, 26, 27]; 30: [5, 6, 7, 12, 13, 19, 20, 26, 27] | col216/ec184 [nao verificado] |
| 50 | 1 | meses de 31; fevereiro | 30: [30] | col128/ec910 [nao verificado] |
| 53 | 1 | meses de 30; fevereiro | 31: [3, 11, 17, 25, 31] | col384/ec1347 [nao verificado] |
| 54 | 1 | meses de 30; fevereiro | 31: [4, 9, 18, 23] | col310/ec1167 [nao verificado] |
| 55 | 1 | meses de 30; fevereiro | 31: [5, 11, 19, 25] | col204/ec1045 [nao verificado] |
| 56 | 1 | meses de 30; fevereiro | 31: [4, 13, 18, 27] | col962/ec1303 [nao verificado] |
| 57 | 1 | meses de 30; fevereiro | 31: [6, 11, 20, 25] | col577/ec491 [nao verificado] |
| 59 | 1 | meses de 30; fevereiro | 31: [4, 10, 18, 24] | col349/ec307 [nao verificado] |
| 60 | 1 | meses de 30; fevereiro | 31: [3, 11, 17, 25, 31] | col587/ec500 [nao verificado] |
| 61 | 1 | meses de 30; fevereiro | 31: [4, 12, 18, 26] | col205/ec1044 [nao verificado] |
| 62 | 1 | meses de 30; fevereiro | 31: [5, 11, 19, 25] | col214/ec182 [nao verificado] |
| 64 | 1 | meses de 30; fevereiro | 31: [8, 11, 15, 22, 29] | col919/ec1136 [nao verificado] |
| 66 | 1 | meses de 31; fevereiro | 30: [2, 9, 16, 22, 23, 30] | col243/ec953 [nao verificado] |
| 68 | 1 | fevereiro | 31: [6, 13, 18, 20, 27]; 30: [3, 10, 17, 22, 24] | col278/ec1350 [nao verificado] |
| 70 | 1 | fevereiro | 31: [6, 11, 13, 20, 27]; 30: [3, 8, 10, 17, 24] | col203/ec1409 [nao verificado] |
| 71 | 1 | meses de 31; fevereiro | 30: [4, 11, 15, 18, 25] | col617/ec1354 [nao verificado] |
| 73 | 1 | fevereiro | 31: [2, 9, 15, 16, 23, 30]; 30: [6, 13, 15, 20, 27] | col263/ec1526 [nao verificado] |
| 74 | 1 | fevereiro | 31: [3, 4, 10, 17, 24, 31]; 30: [6, 13, 15, 20, 27] | col922/ec1142 [nao verificado] |
| 76 | 1 | fevereiro | 31: [6, 8, 13, 20, 27]; 30: [3, 8, 10, 17, 24] | col968/ec1358 [nao verificado] |
| 77 | 1 | fevereiro | 31: [5, 12, 18, 19, 26]; 30: [2, 8, 9, 16, 23, 30] | col879/ec1359 [nao verificado] |
| 79 | 1 | fevereiro | 31: [4, 5, 12, 19, 26]; 30: [1, 2, 9, 16, 23, 30] | col327/ec285 [nao verificado] |
| 81 | 1 | fevereiro | 31: [2, 9, 16, 18, 23, 30]; 30: [6, 13, 15, 20, 27] | col522/ec446 [nao verificado] |
| 82 | 1 | meses de 30; fevereiro | 31: [3, 10, 17, 24, 25, 31] | col292/ec1267 [nao verificado] |
| 83 | 1 | fevereiro | 31: [5, 12, 18, 19, 26]; 30: [2, 9, 15, 16, 23, 30] | col917/ec1134 [nao verificado] |
| 84 | 1 | fevereiro | 31: [6, 13, 20, 25, 27]; 30: [3, 10, 17, 22, 24] | col301/ec265 [nao verificado] |
| 85 | 1 | fevereiro | 31: [4, 7, 14, 21, 28]; 30: [1, 4, 11, 18, 25] | col849/ec1001 [nao verificado] |
| 86 | 1 | fevereiro | 31: [1, 4, 8, 15, 22, 29]; 30: [1, 5, 12, 19, 26] | col382/ec1269 [nao verificado] |
| 89 | 1 | fevereiro | 31: [4, 11, 18, 25]; 30: [1, 8, 15, 22, 29] | col814/ec979 [nao verificado] |
| 90 | 1 | meses de 30; fevereiro | 31: [4, 7, 14, 20, 26] | col89/ec1039 [nao verificado] |
| 92 | 1 | meses de 30; fevereiro | 31: [2, 6, 12, 18, 24, 31] | col638/ec1306 [nao verificado] |
| 94 | 1 | meses de 30; fevereiro | 31: [5, 12, 18, 19, 26] | col81/ec812 [nao verificado] |
| 95 | 1 | meses de 30; fevereiro | 31: [1, 6, 11, 15, 21, 25, 28] | col900/ec1341 [nao verificado] |
| 96 | 1 | meses de 31; fevereiro | 30: [4, 8, 14, 20, 26] | col107/ec1203 [nao verificado] |
| 98 | 1 | meses de 31; fevereiro | 30: [5, 11, 18, 24, 29] | col114/ec1537 [nao verificado] |
| 101 | 1 | meses de 30; fevereiro | 31: [3, 10, 18, 27] | col829/ec947 [nao verificado] |
| 106 | 1 | meses de 30; fevereiro | 31: [1, 8, 16, 22, 29] | col749/ec1374 [nao verificado] |
| 107 | 1 | meses de 30; fevereiro | 31: [8, 15, 18, 23, 29] | col87/ec802 [nao verificado] |
| 109 | 1 | meses de 30; fevereiro | 31: [6, 13, 20, 27] | col245/ec1381 [nao verificado] |
| 115 | 1 | fevereiro | 31: [4, 11, 18, 20, 25]; 30: [5, 8, 12, 19, 26] | col822/ec1360 [nao verificado] |
| 118 | 1 | meses de 30; fevereiro | 31: [4, 10, 18, 24] | col43/ec1411 [nao verificado] |
| 123 | 1 | meses de 31; fevereiro | 30: [4, 11, 18, 22, 25] | col865/ec1357 [nao verificado] |
| 124 | 1 | meses de 30; fevereiro | 31: [4, 11, 18, 25] | col826/ec967 [nao verificado] |
| 127 | 1 | meses de 30; fevereiro | 31: [2, 10, 25, 28, 29] | col950/ec1321 [nao verificado] |
| 128 | 1 | meses de 30; fevereiro | 31: [2, 9, 11, 16, 23, 30] | col369/ec1313 [nao verificado] |
| 132 | 1 | meses de 30; fevereiro | 31: [5, 12, 19, 25] | col903/ec1460 [nao verificado] |
| 133 | 1 | meses de 30; fevereiro | 31: [4, 6, 13, 20, 27] | col129/ec853 [nao verificado] |
| 134 | 1 | meses de 30; fevereiro | 31: [1, 7, 13, 19, 25] | col789/ec1472 [nao verificado] |
| 135 | 1 | meses de 30; fevereiro | 31: [4, 10, 16, 22, 28] | col967/ec1316 [nao verificado] |
| 136 | 1 | meses de 30; fevereiro | 31: [6, 12, 18, 24, 30] | col446/ec384 [nao verificado] |
| 137 | 1 | meses de 30; fevereiro | 31: [2, 9, 16, 20, 25] | col502/ec1150 [nao verificado] |
| 140 | 1 | fevereiro | 31: [2, 8, 16, 22, 30]; 30: [2, 8, 16, 22, 30] | col899/ec1512 [nao verificado] |
| 141 | 1 | meses de 31; fevereiro | 30: [1, 7, 15, 21, 29] | col444/ec1513 [nao verificado] |
| 143 | 1 | meses de 31; fevereiro | 30: [11] | col1022/ec1521 [nao verificado] |
| 144 | 1 | meses de 31; fevereiro | 30: [8, 14, 22, 28] | col941/ec1556 [nao verificado] |
| 145 | 1 | meses de 31; fevereiro | 30: [7, 15, 21, 29] | col945/ec1255 [nao verificado] |
| 146 | 1 | meses de 31; fevereiro | 30: [1, 7, 15, 21, 29] | col985/ec1418 [nao verificado] |
| 149 | 1 | fevereiro | 31: [6, 13, 15, 20, 27]; 30: [7, 14, 15, 21, 28] | col191/ec161 [nao verificado] |
| 150 | 1 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col933/ec1184 [nao verificado] |
| 151 | 1 | meses de 31; fevereiro | 30: [1, 5, 12, 19, 26] | col991/ec1425 [nao verificado] |
| 152 | 1 | meses de 31; fevereiro | 30: [2, 9, 16, 23, 30] | col992/ec1584 [nao verificado] |
| 153 | 1 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col474/ec1270 [nao verificado] |
| 154 | 1 | meses de 30; fevereiro | 31: [1, 10, 15, 24, 29] | col529/ec1038 [nao verificado] |

Leitura em [nao verificado]; nada foi escrito no banco. [nao verificado] Cadastro e do admin (L-099): corrigir pela tela do calendario. [nao verificado]

## 16/11 13:24 · sessao 43aa1fa989b8 · - · - · origem=Ronald (tmux)

- **Célula versus foto:** `EC.eh_dia_trabalho` retorna diretamente `cel.trabalha` se existir célula; consulta cálculo/foto somente sem célula (`app/escala/models.py:1087-1096`). O cálculo puro lê `FolgaDia` do próprio vínculo/mês e responde `False` se a data estiver na foto (`app/escala/models.py:1280-1288`, `app/escala/models.py:1309-1317`). `EC.marcos_do_dia` lê `dna.marcos`; sem marcos, usa template (`app/escala/models.py:1117-1133`).

- **Grade/espelho leem folga por cima:** `_aplicar_folgas_foto` consulta juiz da folga e altera apenas o dicionário da grade: `tipo_dia='folga'`, colunas `None`, órfãs vazias (`app/escala/utils.py:1140-1153`); a grade composta chama esse overlay (`app/escala/utils.py:1844`). O espelho deriva seu flag `folga` desse `tipo_dia` e o publica no dia (`app/ponto/services/espelho.py:673-679`, `app/ponto/services/espelho.py:764-771`).

- **Precedência também sobrepõe:** lê o `trabalha` da célula como `previsto`; mesmo `previsto=True`, uma folga lançada válida entra na lista e vence trabalho pela ordem de precedência (`app/ponto/precedencia.py:347-387`, `app/ponto/precedencia.py:46`). Folga de calendário só vale para o vínculo do dia; manual vale para pessoa (`app/ponto/precedencia.py:235-239`). Espelho carrega esses fatos (`app/ponto/services/espelho.py:646-649`); o compositor responde `FOLGA` quando `tipo_do_dia='folga'` (`app/ponto/services/dia_decidido.py:370-372`).

- **Motor segue célula:** classificação de folga trabalhada pergunta a `EC.eh_dia_trabalho`; só sua resposta `False` aciona FT (`app/ponto/motor_calculo_v2.py:2445-2469`), separado em `periodos_ft` (`app/ponto/motor_calculo_v2.py:2492-2494`). Marcos vêm de EC e DNA, com fallback (`app/ponto/motor_calculo_v2.py:2290-2291`, `app/ponto/motor_calculo_v2.py:1532-1540`; outros consumidores `app/ponto/motor_calculo_v2.py:2686-2691`, `app/ponto/motor_calculo_v2.py:3014-3017`, `app/ponto/motor_calculo_v2.py:3391-3396`). Portanto, célula `True` e folga válida na foto permitem grade/precedência `folga` enquanto esta classificação do motor segue trabalho, pelas condições citadas.

- **Folha:** fechamento monta datas previstas por `esc.eh_dia_trabalho` (`app/ponto/services/fechamento.py:724-733`), alimenta motor com células e chama `apurar_por_fatia` (`app/ponto/services/fechamento.py:748-756`). Horas normais/extras leem resultado (`app/ponto/services/fechamento.py:918-924`); FT lê períodos reconhecidos e publica `horas_folga_trabalhada` (`app/ponto/services/fechamento.py:925-928`, `app/ponto/services/fechamento.py:965-966`). TXT lê fechamentos recebidos ou persistidos (`app/folha/export.py:717-722`) e seus eventos (`app/folha/export.py:725-739`), incluindo `horas_folga_trabalhada` (`app/folha/export.py:439-446`).

- **Totais/pago do espelho:** totais chamam `folha_manda` (`app/ponto/services/espelho.py:1283-1284`), que chama `totais_da_folha` (`app/relatorios/cartao_pela_celula.py:320-323`); esta chama `fechamento_lido` (`app/relatorios/cartao_pela_celula.py:69-76`), a mesma apuração em modo leitura (`app/ponto/services/fechamento.py:1759-1761`). Pago/rubricas do dia leem `DiaPago` (`app/ponto/services/espelho.py:805-813`).

- **Dono individual:** os insumos necessários para executar `dono_da_divergencia` não estão no dado recebido `[nao verificado]`. O juiz distingue cadastro por flags específicas, depois batida, fase e `em_cadastro_x_realidade`; só o resíduo é estrutura (`app/ponto/management/commands/e6_oraculo.py:98-110`). Causa executada e dono individual do relato não se deduzem de `FolgaDia`, `trabalha=True` e `regeneracoes=0` isoladamente `[nao verificado]`.

## 16/11 13:24 · sessao 325eca3c6fbc · - · - · origem=Ronald (tmux)

O módulo escritor de `CelulaDia` é `app/ponto/portas/celula.py`; o registro de portas aponta somente para ele e o selo tem allowlist vazia (`app/core/portas.py:107-113`; `app/ponto/tests/test_chokepoint_celula_gate.py:19-23,48-54`).

- **Geração normal:** `gerar_celulas_periodo` chama `ec.eh_dia_trabalho_calculado(d)` e cria a célula com `trabalha=valor`, geradora e DNA (`app/ponto/portas/celula.py:911-918`). Célula editada fica; passado existente só recebe DNA ausente, sem mudar `trabalha`; hoje/futuro existente é pulado pela função (`app/ponto/portas/celula.py:919-935`). O command possui suplemento próprio que regenera **hoje**, pela porta, antes da geração (`app/ponto/management/commands/gerar_celulas.py:303-325,345-350`).
- **Regeneração:** lê `eh_dia_trabalho_calculado`, monta DNA e cria ausentes; para existente `origem='gerada'` divergente, altera `trabalha`, geradora e DNA e incrementa `regeneracoes`, guardando DNA anterior e instante (`app/ponto/portas/celula.py:289-311`). Não depende de a data ser futura; é a exceção formal ao passado imutável (`app/ponto/portas/celula.py:112-135`; `CLAUDE.md:149-157`). Recusa vínculo sem vigência declarada e pula competência exportada, salvo ato explícito `apesar_da_lavra` (`app/ponto/portas/celula.py:147-162,218-221,284-288`).
- **Outras escritas de trabalha:** fora da vigência, a regeneração pode zerar `trabalha`, com ramos distintos para futuro e passado (`app/ponto/portas/celula.py:229-282`); `reverter_regeneracao_dia` restaura pelo DNA anterior e incrementa carimbos (`app/ponto/portas/celula.py:817-837`); `editar_dia` grava True/False, DNA, geradora, `origem='editada'`, autor/data, exigindo autor e motivo (`app/ponto/portas/celula.py:597-602,622-647`). Edição não incrementa os campos de regeneração (`app/ponto/portas/celula.py:575-580`). Troca de plantão delega duas edições ao mesmo escritor, cedente False e substituto True (`app/ponto/portas/celula.py:670-679,706-715`).

**Caminho foto → célula:** foto mensal é gravada e relavrada na mesma transação (`app/escala/folgas.py:150-163,193-218`). `eh_dia_trabalho_calculado` consulta `FolgaDia` do vínculo/mês; data presente na foto dá False (`app/escala/models.py:1280-1288,1296-1317`). A relavra chama a regeneração para o passado e `regenerar_futuras_do_cadastro` para futuro (`app/escala/folgas.py:280-300`). O suplemento futuro seleciona apenas células **geradas já existentes**, a partir de amanhã e dentro de `_janela_default`, respeitando o dono do dia (`app/ponto/services/cadastro_futuro.py:26-45`). Essa janela é a união das competências **correntes** das empresas ativas (`app/ponto/management/commands/gerar_celulas.py:290-300`).

Assim, uma foto de dezembro pode ser gravada sem alcançar células de dezembro que estejam fora daquela janela; o dado fornecido não informa dia do ato, cortes das empresas, origem/geradora das células nem se alguma guarda barrou — o motivo concreto é `[nao verificado]`. `regeneracoes=0` não prova que a função nunca foi chamada: o incremento só ocorre no ramo de reescrita divergente (`app/ponto/portas/celula.py:297-311`).

**Portas de cadastro relevantes:** vínculo criado/atualizado copia calendário padrão e gera foto do mês corrente e seguinte (`app/colaboradores/services/vinculo.py:151-166,283-298`); depois regenera passado e futuras existentes (`app/colaboradores/services/vinculo.py:315-340`). Troca do calendário do vínculo sincroniza foto futura quando o FK muda (`app/colaboradores/services/vinculo.py:450-465`). Salvar calendário sincroniza seus vínculos (`app/escala/folgas.py:571-601`). A sincronização específica poupa passado e seleciona células futuras existentes, reconcilia foto futura mensal e regenera pela porta (`app/escala/folgas.py:605-642`). Mudança de vigência e âncora tem signals separados para passado e futuro (`app/escala/signals.py:54-83,111-134`); edição de template também propaga pela regeneração (`app/escala/services/cadastro_tipo.py:332-366`).

**(b): sim, a chamada mensal `gerar_foto_folgas` alcança passado.** Sem `datas_foto`, seleciona foto de calendário pelo mês inteiro, apaga e recria todas as datas do calendário; não há filtro `>= hoje` (`app/escala/folgas.py:180-197`). Chama relavra desde o primeiro dia do mês (`app/escala/folgas.py:212-218`), e o ramo passado regenera de `desde` até **hoje**, sem `min(ate, hoje)` (`app/escala/folgas.py:290-296`). Portanto células passadas geradas podem ser tocadas pela exceção formal, respeitadas as guardas; não são genericamente protegidas dessa chamada pela irretroatividade (`app/ponto/portas/celula.py:284-311`; `CLAUDE.md:149-157`).

_6042 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
