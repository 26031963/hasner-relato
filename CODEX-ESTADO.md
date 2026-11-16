# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 11:43** · publicado em **16/11/2026 11:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 11:43 · sessao 6cd97d087b6c · - · - · origem=Ronald (tmux)

Q4 — **estrutura coerente; proveniência dos logs e comparação histórica de dados pessoais [não verificado].**

- São **19 entradas acrescentadas**: 10 restauradas (`app/docs/CORTES.json:1725`, `1732`, `1739`, `1746`, `1753`, `1760`, `1767`, `1774`, `1781`, `1788`); 8 avais (`1802`, `1810`, `1818`, `1826`, `1834`, `1842`, `1850`, `1858`); nascimento de `pergunta_respondida` (`1866`). A validação em memória encontrou 255 IDs únicos no JSON inteiro e nenhum campo obrigatório vazio nas 19 entradas; todas as datas são aceitas pela conversão de `x` para `0` do gerador (`bin/gerar_cortes.py:35`).

- As 10 restaurações não têm chave `onde`; registram origem histórica e/ou referências na frase/estado (`app/docs/CORTES.json:1727`, `1729`, `1734`, `1741`, `1748`, `1755`, `1762`, `1769`, `1776`, `1783`, `1790`). As outras 9 têm `onde` apontando para `logs/AVAIS-1515/` (`app/docs/CORTES.json:1806`, `1814`, `1822`, `1830`, `1838`, `1846`, `1854`, `1862`, `1870`). Esses arquivos e o diretório `logs/` estão ausentes da cópia: existência e conteúdo **[não verificado]**; `logs/` é ignorado pelo repositório (`.gitignore:53`).

- As autoridades referidas pelas restaurações existem: `dia_da_pergunta_trancado` (`app/chamados/juizes.py:978`), `dia_sem_trabalho` (`445`), `esta_desligado` (`app/colaboradores/services/desligamento.py:23`), `folga_lancada_do_dia` (`app/ponto/precedencia.py:212`), `motivo_do_veto` (`app/ponto/services/furos_vetados.py:27`), `pauta_cadastro_existente` (`app/chamados/services/fabrica_da_celula.py:317`), `porta_alcancavel_do_dia` (`app/chamados/juizes.py:1693`), `status_do_dia_lavrado` (`app/ponto/status_dia.py:201`), `tarefa_do_dia` (`app/chamados/services/tarefa_do_dia.py:733`), `tem_gps` (`app/ponto/services/geofence.py:126`).

- Há também **13 alterações de estado**, todas atribuindo comprovação ao censo `logs/CORTES-RECEBIDO-FORA-DA-PAUSA/c1.r`; os resultados desse censo ficam **[não verificado]** (`app/docs/CORTES.json:902`, `909`, `916`, `923`, `1021`, `1028`, `1056`, `1063`, `1070`, `1091`, `1098`, `1154`, `1161`).

- Dados pessoais: a restauração associa primeiro nome a `p#34375`, mas essa mesma associação já consta em outros documentos da cópia (`app/docs/CORTES.json:1771`; `app/docs/BACKLOG.md:961`, `963`; `app/docs/RELATO.md:2856`). Os interlocutores nomeados nos novos avais também aparecem em entradas anteriores (`app/docs/CORTES.json:1807`, `1815`, `1831`; anteriores em `1188`, `1223`, `1342`, `1470`). A busca de padrões não encontrou CPF, telefone ou email nas 19 novas entradas; ausência de dado pessoal adicional em relação às árvores anteriores: **[não verificado]**, porque não há histórico Git disponível.

O Markdown é reproduzível integralmente pela função pura `markdown(JSON, agora=2026-11-16 11:38)`; a seção “Todos” coincide independentemente do horário (`bin/gerar_cortes.py:97`, `106`). Isso comprova reprodutibilidade do derivado atual; a execução histórica do gerador permanece **[não verificado]**.

## 16/11 11:43 · sessao b0195f44117c · - · - · origem=Ronald (tmux)

Pergunta 2 — **comparação histórica [não verificado]**. A cópia não contém `.git`; não consegui executar os dois `git diff` pedidos nem comparar o conteúdo histórico dos 56 arquivos do anexo. Os 56 caminhos existem na cópia; isso não prova preservação do merge. [não verificado]

O `RELATO.md` documenta a certificação anterior em `51678cbb2` e um ensaio de merge do comboio, mas não contém os dois diffs solicitados nem prova do delta `be4ebe85b → 99eccedf9`: `app/docs/RELATO.md:18`, `app/docs/RELATO.md:19`, `app/docs/RELATO.md:21`.

Para o arquivo compartilhado, o resultado final preserva a catraca de identidades, quantidades, composição e linhas reais: `app/core/tests/test_medida_turno_do_par.py:100`, `:151`, `:247`, `:541`, `:557`. Há registros correspondentes a ambos os grupos:

- Obra: `_dia_da_batida` e `_dia_do_contexto_fraco` registrados em `app/core/tests/test_medida_turno_do_par.py:258` e `:259`, correspondendo às expressões em `app/chamados/juizes.py:54` e `:170`.
- Main: `regularizar_retorno` registrado em `app/core/tests/test_medida_turno_do_par.py:339`, correspondendo à expressão em `app/ponto/views.py:3311`.

Executei o teste inteiro em memória, sem Django, banco ou bytecode, conforme o procedimento documentado em `app/core/tests/test_medida_turno_do_par.py:20`: **35 testes passaram, zero falhas/erros**. Isso confirma coerência do retrato final, mas **“preservou as duas intenções do merge” continua [não verificado]** sem as versões históricas.

A intenção funcional explícita do LOTE3 c7 está presente: posto e histórico dentro do mesmo `atomic`, com releitura do gravado após recusa (`app/colaboradores/views.py:331`, `:333`, `:343`); seu teste exercita falha depois da escrita real do histórico e exige rollback dos dois (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:230`, `:253`, `:256`). A intenção B1 c2 também está explícita no resultado: ressuscitar zera o par inteiro, conserva a trilha anterior e evita o ato `REABERTURA` que mudaria o dono da tarefa (`app/chamados/models.py:1613`, `:1622`, `:1629`). Isso comprova presença no código atual, não equivalência com os diffs anteriores.

## 16/11 11:42 · sessao 3147dc5e9e8d · - · - · origem=Ronald (tmux)

(c) Censo da cópia, referências relativas à raiz:

| Leitor/caminho | Evidência e efeito |
|---|---|
| Exportação da função por `chamados.models` | `app/chamados/models.py:4231` reexporta a função real. |
| Único chamador de produção localizado | `montar_chamados_fio`, `app/chamados/services/fio.py:373-376`: importa como `_eap`, grava `p.estado_aplicacao`; sem disputa ou em exceção, usa `None`. |
| Modal admin | `app/chamados/views.py:889` monta o fio com explicação; `:927-931` renderiza `modal_fio.html`. Re-render parcial passa pelo mesmo montador em `app/chamados/services/fio.py:484`, renderizado em `:534-535`. |
| Template consumidor efetivo | `app/templates/chamados/partials/modal_fio.html:230-246`: só mostra `detalhe` dos estados `bloqueada_paridade` (vermelho) e `ja_satisfeita` (azul), tanto validada quanto ainda não validada. |
| Precedência visual | `modal_fio.html:222-226`: `pendencia_admin` mostra “Pendencia do admin”, seu display e `recusa_motivo`, antes do ramo validada. `:228-229` mostra “✓ Validada”. Não há ramo que imprima `aplicada`, `recusada_paridade`, `aplicavel` ou `invalida` nesse bloco (`:230-248`). Portanto o anexo prova o retorno lógico `'aplicada'`, mas a legenda literal “aplicada” na tela não está confirmada pelo template desta cópia. | [nao verificado]
| Meu Atendimento | Também executa o montador (`app/chamados/views.py:569`) e renderiza `chamados/meu_atendimento.html` (`:632-635`), porém não foi localizado consumo de `estado_aplicacao` nos templates dessa tela. `[não verificado]` quanto a outros clientes fora da cópia. |
| Fio admin em página inteira | Executa o montador em `app/chamados/views.py:677` e renderiza `fio_colaborador.html` em `:697-703`; esse template não lê `estado_aplicacao`. `[não verificado]` quanto a alguma apresentação fora da cópia. |
| API de perguntas | `app/api/views.py:2550-2564` serve `p.estado` e `aguarda_admin`, não `estado_aplicacao_pergunta`; `:2568-2578` monta a resposta com pendentes/histórico. Não confundir esses estados com `'aplicada'`. |
| Contadores do modal | `app/chamados/views.py:900-912`: ativos vêm de `qs_na_fila`, mensagens do queryset de followups e disputas abertas de `fechada_em__isnull`; nenhum deriva do estado de aplicação. Não localizei contador de `'aplicada'` na cópia. `[não verificado]` quanto a fontes externas. |
| Testes que chamam a função | `app/chamados/tests/test_estado_aplicacao.py:48`, `:56`, `:63`, `:68-70`; `app/chamados/tests/test_badge_via_paridade.py:30-32`. O teste de paridade confirma apenas o dict da função; não verifica render de um badge. |
| Registro estrutural do leitor | `app/chamados/tests/test_contract_leitores_vitalidade.py:102` classifica a função como “exibicao: preve efeito da validacao”. |

Corroboração documental, sem substituir leitura do código: `app/docs/RELATO.md:653` diz que estado_aplicacao só era consumido no modal; `:1009` registra que o modal omite `aplicavel` e que `_pergunta_card`/detalhe/central/lote/copiloto não recebem esse estado. Atenção: `RELATO.md:673` contém uma afirmação anterior sobre C7 usar essa testemunha, mas a própria linha a corrige e remete a RAIZ F1; não usar a frase anterior como prova de chamador atual.

(e) **Encaixe: F4 da E4c**, pois o defeito está no leitor de efeito do materializador (`app/chamados/services/materializacao.py:431-448`), e o escopo registrado de F4 é “materializacao/passivo” (`app/docs/RELATO.md:292`), incluindo o desacordo ato/projeção explicitamente referido à função em `:115` e `:133`; RAIZ trata a relação agregado/filhos, destino, fechamento e pares (`app/docs/BACKLOG.md:2031`).

O diretório `logs/ESPINHA-BRIEFS/F4` não está disponível nesta cópia; seu conteúdo literal é `[não verificado]`. Há descrição documental do brief em `app/docs/RELATO.md:254`, sem acesso à matriz referida.

## 16/11 11:42 · sessao a62ff595dc63 · - · - · origem=Ronald (tmux)

Não localizei teste existente de **login DRF com cookie de sessão válido e CSRF ativado**; os testes examinados não montam essa combinação: `app/api/tests/test_api_login.py:58-63`, `app/api/tests/test_login_campos.py:27-32`, `app/api/tests/test_login_app_sem_colaborador.py:25-30`, `app/core/tests/test_contrato_trocar_senha.py:127-142`.

| Cobertura existente | Evidência e limite |
|---|---|
| Login bem-sucedido, erros e tokens | `app/api/tests/test_api_login.py:58-63` cria `APIClient()` novo por requisição, sem sessão; `:167-180` verifica 200 e payload. |
| Campos alternativos e JSON/form | `app/api/tests/test_login_campos.py:27-32` cria `Client()` novo, sem `enforce_csrf_checks`; casos em `:34-51`. |
| Login app/gestão | `app/api/tests/test_login_app_sem_colaborador.py:25-30` usa `self.client`, sem estabelecimento prévio de sessão; sucesso em `:38-46`. |
| Login core e flag de senha | `app/core/tests/test_contrato_trocar_senha.py:127-142` chama apenas `api.views_core.api_login` diretamente com `APIRequestFactory()`, sem passagem pelos middlewares. |
| Paridade dos dois kernels | `app/api/tests/test_r15_token_por_credencial.py:149-169` testa emissão de tokens e fonte de refresh; não faz login HTTP com sessão. |
| CSRF ativo existente próximo | `app/api/tests/test_api_mensageria_push.py:67-78` usa `Client(enforce_csrf_checks=True)`, mas POSTa mensageria sem cookie de sessão; não cobre login. |

**Armadilha de cobertura:** a suite configura `config.urls_test` em `app/config/settings_test.py:9`; esse URLconf concatena API core antes da UI em `app/config/urls_test.py:8-15`. Assim, `/api/auth/login/` nessa configuração escolhe core primeiro (`app/api/urls_core.py:9`), embora a docstring diga `api.views.api_login` (`app/api/tests/test_api_login.py:2`). Para testar UI é necessário selecionar explicitamente seu URLconf, cuja inclusão está em `app/config/urls.py:37` e rota em `app/api/urls.py:60`.

Esboço RED proposto, **não criado nem executado [nao verificado]**:

```python
# Fixture: usuario_sessao ativo; outro colaborador ativo com credenciais válidas.
# Repetir com ROOT_URLCONF="config.urls_core" e "config.urls".
with override_settings(ROOT_URLCONF=urlconf):
    client = APIClient(enforce_csrf_checks=True)
    client.force_login(usuario_sessao)

    assert settings.SESSION_COOKIE_NAME in client.cookies
    client.cookies.pop(settings.CSRF_COOKIE_NAME, None)
    # Não enviar Authorization, X-CSRFToken nem csrfmiddlewaretoken.

    response = client.post(
        "/api/auth/login/",
        {"username": usuario_login.username, "password": senha_login},
        format="json",
        HTTP_HOST="juliani.hasner.com.br",
    )

    assert response.status_code == 200, response.content
    assert response.data["colaborador"]["id"] == colaborador_login.pk
    assert {"access", "refresh"} <= response.data["tokens"].keys()
```

Usar usuários distintos verifica também que o login responde pelas credenciais do corpo, conforme os handlers consultam `autenticar_colaborador` em `app/api/views_core.py:195-198` e `app/api/views.py:209-212`. O resultado RED esperado antes de correção é o bloqueio CSRF descrito no anexo; execução e mensagem exata nesta cópia **[nao verificado]**.

## 16/11 11:42 · sessao be3cf7792cd1 · - · - · origem=Ronald (tmux)

- **Referência incorreta:** `BACKLOG:2159` identifica SOMBRA-HOJE-NA-BORDA; O10-D-PERMUTA-RECIPROCA está em `app/docs/BACKLOG.md:2172`.
- **Contrato O10-D:** quatro células; duas chamadas a `trocar_plantao` num atomic; validar 11h/sobreposição/permissão/exportada antes da primeira escrita; rollback integral; recusa falante; correlação; seis RED e confirmação DP 1/3/6 — `app/docs/BACKLOG.md:2172`.
- **Cruzamento físico O10-D com obras em voo:** [nao verificado]. O registro disponível declara ausência de cópia de construção — `app/docs/RELATO.md:1592`. Não há conjunto de alterações O10-D para comparar.
- **B1/B2 e BUG-PAR:** cruzamento entre BUG-PAR e B1 está documentado em `fio.py`, `test_s139.py`, `test_medida_turno_do_par.py` — `app/docs/RELATO.md:36`; medida anterior inclui `JUIZES.md` — `app/docs/RELATO.md:70`. BUG-PAR não constrói e B2 espera pouso B1 — `app/docs/RELATO.md:51`. F2/F4 também esperam B1 — `app/docs/RELATO.md:6`.
- **LOTE7:** conjunto documentado abrange `ponto/services/ausencia.py`, `regularizacao.py`, `retratar_humano.py`, `ferias/views.py` — `app/docs/RELATO.md:50`; pouso está em PAREI — `app/docs/RELATO.md:5`. Cruzamento físico O10-D nesses arquivos: [nao verificado].
- **Alcance transitivo real de O10-D:** `trocar_plantao` chama `editar_dia` — `app/ponto/portas/celula.py:713`; esta marca cobranças órfãs e chama `_o_chao_do_dia_mudou` — `app/ponto/portas/celula.py:663`; o fluxo rejulga célula — `app/ponto/portas/celula.py:463`, recalcula fechamento — `app/ponto/portas/celula.py:496` e agenda rejunção/reavaliação de chamados — `app/ponto/portas/celula.py:531`. Isso prova alcance em chamados, sem provar arquivos alterados em comum.
- **Interface ausência/BUG-PAR:** materialização importa e chama `criar_ausencia` — `app/chamados/services/materializacao.py:472`, `app/chamados/services/materializacao.py:491`, `app/chamados/services/materializacao.py:519`, `app/chamados/services/materializacao.py:527`. Alteração de `ausencia.py` pela O10-D: [nao verificado].

Para Q3, distinguir **colisão demonstrada** de **dependência que exige medição**: as provas disponíveis mostram a segunda para O10-D; o próprio item 4 manda esperar quando houver cruzamento [nao verificado — anexo fornecido sem numeração].

## 16/11 11:42 · sessao 9acbf789d5c7 · - · - · origem=Ronald (tmux)

Achados confirmados:

- **Citação errada:** `app/docs/BACKLOG.md:2159` é SOMBRA-HOJE-NA-BORDA; O10-D está em `app/docs/BACKLOG.md:2172`.
- **Contrato O10-D existente:** quatro células, duas chamadas a `trocar_plantao` no mesmo `atomic`, validação prévia, rollback integral, recusa falante e correlação da trilha constam em `app/docs/BACKLOG.md:2172`.
- **Lei de 09/11:** a linha da O10-D registra LEI-O10-D-PERMUTA 12:0x, seis perguntas mais quatro residuais, caminho `logs/O10-D-PERMUTA-NA-TELA/aval_lei_permuta.txt` e RED por regras 1–6; o texto integral dessa resposta não foi conferido. `app/docs/BACKLOG.md:2172`.
- **Censo_v2:** o RELATO registra conclusão e resultado `rc 0`, com ressalvas de rollback, ordenação das quatro células e datas iguais permitidas. `app/docs/RELATO.md:1630`. O RELATO também afirma que censo e lei estão prontos, enquanto os RED 1–6 ainda faltam. `app/docs/RELATO.md:1592`.
- **O10-C:** a condição INTERROMPIDA está comprovada em `app/docs/BACKLOG.md:2219`; a tabela também a classifica PARADA em `app/docs/RELATO.md:975`.
- **Molde PREPARA:** há despacho e resultado documentados, com construção bloqueada pelo cruzamento de arquivos, espera pelos pousos da F1 B1/R3 e continuidade da fila. `app/docs/RELATO.md:70`, `app/docs/RELATO.md:71`.
- **Ordem:** o registro anterior proíbe lateral por iniciativa, mas o aval fornecido é decisão nominal de Ronald; a entrada O10-D, isoladamente, não demonstra colisão. `app/docs/BACKLOG.md:9`. A precedência da espinha consta em `app/docs/RELATO.md:293`.
- **Pista:** lateral usa PISTA como pré-filtro; o pouso continua certificado pela VM. Esse requisito precisa permanecer no brief. `app/docs/RELATO.md:27`, `app/docs/BACKLOG.md:830`.
- **Restrição do construtor:** L-126 proíbe ao Codex `ponto/portas/celula.py`, `core/juizes.py`, competência exportada e item que pede `!`; o brief precisará esclarecer arquivos/atos permitidos, sem presumir que o `!` futuro autoriza essas escritas. `app/docs/LEIS.md:147`.

## 16/11 11:42 · sessao 9134330fae03 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 16/11 11:42 · sessao 3a666dfaf3c9 · - · - · origem=Ronald (tmux)

Q3 — **Há interação; a leitura estática não mostrou conflito de intenção. A atribuição exata dessas linhas ao delta do main fica [não verificado], porque a cópia não contém `.git`.**

- **Travas compartilhadas:** a porta do main chama `PerguntaDisputa.trava_na_ordem(pks)`, usando a autoridade da obra; passa apenas colaboradores, sem travar/escrever perguntas nesse caminho (`app/colaboradores/services/colaborador.py:72`, `:80`). A implementação trava Colaborador, Disputa, Chamado e Pergunta nessa sequência (`app/chamados/models.py:2770`, `:2779`, `:2793`, `:2797`). A ressurreição usa a mesma porta com os quatro conjuntos (`app/chamados/models.py:1612`).

- **`vincular_posto`:** posto e trilha ficam no mesmo `atomic` (`app/colaboradores/views.py:333`); chama `salvar_inteiro` e `registrar_mudanca` (`:334`, `:335`). O save completo guarda `situacao` da linha travada antes de salvar (`app/colaboradores/services/colaborador.py:152`, `:158`). A operação não altera diretamente `PerguntaDisputa`/`DecisaoPergunta`/`CelulaDia` (`app/colaboradores/views.py:311`). A antiga regeneração por troca do posto cadastral está expressamente sepultada: o lugar do dia vem do vínculo (`app/colaboradores/services/colaborador.py:118`).

- **Interação concreta via desligamento:** a cascata usa `respostas_sem_veredito()` para preservar disputas com resposta por julgar, senão chama a porta `fechar` (`app/colaboradores/services/desligamento.py:164`). Essa porta fecha perguntas e registra `DecisaoPergunta.SILENCIO` (`app/chamados/models.py:1999`, `:2005`). `tarefa_do_dia` lê SILENCIO como `silencio_decidido`, sem ação, enquanto REABERTURA devolve ao colaborador (`app/chamados/services/tarefa_do_dia.py:163`, `:176`). Portanto importa preservar a distinção adotada pela B1: ressurreição não chama `reabrir_para_correcao` nem fabrica REABERTURA (`app/chamados/models.py:1622`, `:1629`).

| Entidade/campos | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.respondida_em/resposta_colab`, validação e materialização | Ressurreição salva a projeção anterior em decisões e zera o par pela porta (`app/chamados/models.py:1613`, `:1629`); correção registra o ato e chama a mesma porta (`app/chamados/services/disputa_emissao.py:1609`, `:1631`, `:1648`); update efetivo em `descarimbar_em_lote` (`app/chamados/models.py:3391`). | Juiz `pergunta_respondida`: instante **E** texto após trim (`app/chamados/juizes.py:1544`, `:1552`), registrado como `_C21` (`app/core/juizes.py:306`). Face SQL `q_respondida_com_texto` (`app/chamados/juizes.py:1575`), lida por `q_vivas` (`app/chamados/models.py:3481`) e fila de validação (`app/chamados/juizes.py:1751`). |
| `DecisaoPergunta.ato`, instante e texto/foto anterior | Porta `registrar`, via `bulk_create` (`app/chamados/models.py:4110`, `:4120`); chamada pela ressurreição (`:1613`), correção (`app/chamados/services/disputa_emissao.py:1631`) e desligamento pela porta da disputa (`app/chamados/models.py:2005`). | Carga de `tarefa_do_dia` ordena por pergunta/instante/pk e corta pelo `agora` (`app/chamados/services/tarefa_do_dia.py:421`); julgamento em `_pela_decisao` (`:142`). Histórico de respostas: `respostas_anteriores` (`app/chamados/models.py:3668`). |
| `CelulaDia` | Veredito pela porta `lavrar_veredito`, delegada pelo modelo (`app/escala/models.py:1819`, `:1826`); escrita de veredito/via/instante (`app/ponto/portas/celula.py:1093`). Reabertura/ressurreição acordam células emitindo evento, não escrevem diretamente o veredito (`app/chamados/models.py:3341`, `:3358`). | `q_vivas` lê veredito e ata (`app/chamados/models.py:3488`, `:3493`); `tarefa_do_dia` carrega perguntas com célula (`app/chamados/services/tarefa_do_dia.py:406`). Telas do main também leem células, incluindo `trabalha` (`app/core/views.py:395`, `app/colaboradores/views.py:2900`). |
| `HistoricoVinculo` | `registrar_mudanca` escreve tipo, antes/depois, data, vigência, posto e autor (`app/colaboradores/services/historico_vinculo.py:7`, `:13`); `registrar_evento_de_vinculo` escreve eventos (`:20`, `:25`). Chamados pelo posto (`app/colaboradores/views.py:335`) e desligamento (`app/colaboradores/services/desligamento.py:138`). | Tela de detalhe lê histórico e contagem (`app/colaboradores/views.py:306`). Nenhuma leitura dessa entidade foi encontrada nas funções da obra citadas; ausência global de leitores [não verificado]. |

Outra interação do main: INSS agora guarda situação e cria ausência dentro do mesmo `atomic` (`app/ponto/views.py:2469`, `:2471`, `:2472`, `:2489`); o signal da ausência chama julgamento das células (`app/ponto/signals.py:143`, `:147`). Isso alcança o domínio que os juízes da obra leem, com Colaborador travado antes da cascata (`app/colaboradores/services/colaborador.py:94`).

`core/auditoria.py` delega a escrita da trilha a `evento`, sem alterar os campos de resposta ou decisões (`app/core/auditoria.py:8`, `:22`). Execução concorrente, suite no hash e efeitos em produção: **[não verificado]**.

## 16/11 11:42 · sessao 61988738bd5d · - · - · origem=Ronald (tmux)

Censo estático: **3 implementações**, todas aceitam **POST**, sem `@authentication_classes` e sem `@csrf_exempt` explícitos nos blocos de decoração:

| Kernel | Endpoint | Implementação | Rota |
|---|---|---|---|
| core | `/api/auth/login/` | `app/api/views_core.py:188-190`: `@api_view(['POST'])`, `@permission_classes([AllowAny])` | `app/config/urls_core.py:6` + `app/api/urls_core.py:9` |
| ui | `/api/auth/login/` | `app/api/views.py:202-204`: `@api_view(['POST'])`, `@permission_classes([AllowAny])` | `app/config/urls.py:37` + `app/api/urls.py:60` |
| ui | `/api/diag/cam/` | `app/api/views.py:2642-2644`: `@api_view(['POST'])`, `@permission_classes([])` | `app/config/urls.py:37` + `app/api/urls.py:102` |

O padrão contém `JWTComCredencial` primeiro e `SessionAuthentication` depois; permissão padrão `IsAuthenticated`: `app/config/settings/base.py:32-44`.

Exclusões:

- `HasnerTokenRefreshView`: as duas subclasses locais não declaram autenticação, mas herdam de **`TokenRefreshView`**, não diretamente de `APIView`; não basta olhar a classe local para concluir herança de `SessionAuthentication`. Bases importadas em `app/api/views.py:7`, `app/api/views_core.py:8`; subclasses em `app/api/views.py:69-128`, `app/api/views_core.py:125-184`; rotas `app/api/urls.py:63`, `app/api/urls_core.py:12`. Fonte da biblioteca ausente da cópia pesquisada: atributos efetivos da base **[nao verificado]**.
- `api_app_version`: função Django simples, sem `@api_view`, retorna `JsonResponse`: `app/api/views.py:3042-3054`.
- Mensageria push: função Django `@csrf_exempt` e `@require_POST`, com guarda de token; fora do universo DRF: `app/api/views_mensageria.py:476-481`.
- Único DRF localizado fora de `app/api`: `api_pendentes`, apenas GET e `IsAuthenticated`; fora do censo público: `app/comunicados/views.py:83-88`, rota `app/api/urls.py:85`.

A declaração de rotas públicas registra login para as duas cascas e telemetria de câmera anônima: `app/core/rotas_publicas.py:49-62`.

Pesquisa global não localizou ViewSets, outras subclasses DRF, `authentication_classes`, `get_authenticators` ou outro `REST_FRAMEWORK` no código produtivo; a ausência global não possui uma linha única comprobatória **[nao verificado]**.

## 16/11 10:39 · sessao c49bf044a323 · E4C-F1-NOVA-POUSO-B1 · censo · origem=Code

**(e) NÃO PERGUNTADO** — nenhuma `.q` fornecida pede conferir se a frase de nascimento está na fonte `CORTES.json` e sobreviverá à regeneração; as listas incluem apenas classes genéricas próximas. (`codex_verificador_v1.q:23–26`; `codex_verificador_v2.q:5–18`; `codex_verificador_v3.q:4–14`; `codex_verificador_c2_v1.q:6–12`)

Nos anexos colados, numerei a primeira linha após cada delimitador como linha 1, contando linhas vazias.

O bloqueante é a **persistência do corte na fonte**: a frase aparece no Markdown, e o gerador reescreve esse arquivo inteiro a partir do JSON; a ausência no JSON é registrada pelo opus. (`app/docs/CORTES.md:461`; `bin/gerar_cortes.py:23–24`; `bin/gerar_cortes.py:114–116`; `opus_final.md:4–5`)

**Trechos de cada `.q`**

| Pergunta | Trecho exato e relação com o bloqueante |
|---|---|
| `codex_verificador_v1.q` | “O que o diff diz fazer: nasce o juiz `chamados/juizes.py::pergunta_respondida`” (`:5`). A lista contém “derivacao paralela de fonte unica; selo/RED sem mordida” (`:24`) e “pouso antes do pre-requisito” (`:25`). **Menciona nascimento e classes genéricas; não pergunta pela inscrição no JSON nem pela sobrevivência à geração.** (`:1–30`) |
| `codex_verificador_v2.q` | “PAPEL: verificador, VOLTA 2 (so o delta)” (`:1`); “Para cada achado da volta 1 marcado PROCEDENTE na triagem (1, 4, 5, 6, 7): o diff fecha o achado?” (`:6`). **Não menciona diretamente o bloqueante.** (`:5–18`). Eventual inclusão indireta pelos achados anteriores: **[nao verificado]**, pois a resposta anterior e sua triagem não foram transcritas. |
| `codex_verificador_v3.q` | “PAPEL: verificador, VOLTA 3 (so o delta 1fcb5d0b6..HEAD)” (`:1`); “Os itens 2a, 2b, 2c, 3 e 5 da volta 2: o diff os fecha?” (`:4`). **Não menciona diretamente o bloqueante.** (`:4–14`). Eventual inclusão indireta pelos itens anteriores: **[nao verificado]**. |
| `codex_verificador_c2_v1.q` | “Proibido na obra: tipo de ato novo, juiz novo, lista paralela, regra em tarefa_do_dia que reconheca ressuscitacao por texto de motivo.” (`:3`). O checklist contém “derivacao paralela de fonte unica; selo/RED sem mordida; ordem de trava; pouso antes do pre-requisito” (`:12`). **Não pergunta pela fonte canônica do corte.** (`:6–12`) | [nao verificado]

**Trechos de cada `.r`**

Das quatro respostas nomeadas, somente `codex_verificador_c2_v1.r` foi transcrita; o conteúdo de `codex_verificador_v1.r`, `codex_verificador_v2.r` e `codex_verificador_v3.r` está **[nao verificado]**. Não é possível fornecer trechos exatos dessas três respostas nem afirmar que omitiram o tema. **[nao verificado]**

Na resposta c2, os trechos mais próximos são:

> “**NAO HA nova no trecho c2:** universo, preservação e integridade reutilizam as respectivas autoridades” — classe “Derivação paralela de fonte única”. (`codex_verificador_c2_v1.r:97`)

> “**NAO HA quanto à volta de REABERTURA:** há proibição direta e comparação com SILENCIO; cobertura integral de fila/trilha permanece insuficiente” — classe “Selo/RED sem mordida”. (`codex_verificador_c2_v1.r:98`)

> “**[nao verificado]:** os anexos estabelecem os pré-requisitos, mas não fornecem sua execução nem prova do pouso.” — classe “Pouso antes do pré-requisito”. (`codex_verificador_c2_v1.r:100`) [nao verificado]

**A resposta c2 não menciona `CORTES.md`, `CORTES.json`, `gerar_cortes.py` ou a perda da frase na regeneração.** (`codex_verificador_c2_v1.r:1–101`)

**Triagem correspondente**

Não há item de triagem correspondente ao bloqueante entre os seis descartes apresentados; eles tratam de campos/leitores, projeção legada, preservação/concorrência, testes, chamadores e relógio. (`entrega.md:47–56`) [nao verificado]

Existe, fora dessa triagem, este alerta exato:

> “Resolucao proposta (nao aplicada): LEIS L-130 = a linha da copia; CORTES = manter as DUAS linhas (conferir o escritor   [nao verificado]
> canonico CORTES.json/gerar_cortes.py lendo a fonte -- nao rodar). Zero `.py` em conflito.” (`entrega.md:9–10`)

Esse trecho pede uma conferência; não registra a descoberta da entrada ausente nem seu descarte. (`entrega.md:9–10`)

**O tema estava na lista de classes?** **Não como classe específica.** Estava a expressão genérica **“derivacao paralela de fonte unica”**, nas perguntas v1 e c2; não estava a classe “frase de corte de juiz novo somente em arquivo derivado, ausente da fonte geradora”. (`codex_verificador_v1.q:23–26`; `codex_verificador_c2_v1.q:12`)

## 16/11 10:02 · sessao 780bea312735 · CORTES-F2-F9-1611 · construtor · origem=Code

1. **Confirmei inalcançável.** A porta descarta a pergunta não viva e retorna; a segunda guarda consulta a mesma autoridade sem alteração intermediária de `p` ([views.py:1238](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/views.py:1238), [views.py:1257](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/views.py:1257)). A autoridade memoiza por instância e estado ([juizes.py:893](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/juizes.py:893)).

2. Escrito [test_f6_u9b_ramo_inalcancavel.py:23](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:23): verificador AST, `SimpleTestCase`, fonte localizada pelo módulo importado e asserção RED. `test_a_costura_MORDE` cobre o ramo completo e suas duas condições isoladas; há controle positivo da lápide em comentário ([teste:91](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:91), [teste:126](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:126)). [nao verificado]

3. **MUTANTE declarado:** ramo presente ⇒ **FAIL esperado pela asserção**; ramo removido com lápide ⇒ **OK esperado** ([teste:84](/home/ronald/wt-codex-cortes-f2-f9-1611-095601/app/chamados/tests/test_f6_u9b_ramo_inalcancavel.py:84)). Comando para ambos os estados: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_f6_u9b_ramo_inalcancavel --settings=config.settings.ci -v 2
```

Comando dos mutantes sintéticos:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_f6_u9b_ramo_inalcancavel.RamoInalcancavelU9bTest.test_a_costura_MORDE --settings=config.settings.ci -v 2
```

4. Não executei testes. Resultados RED/OK, importação e setup em execução: **[nao verificado]**. [nao verificado]

## 16/11 09:59 · sessao 02a95393c9a8 · CORTES-F2-F9-1611 · censo · origem=Code

**U9b é inalcançável no fluxo normal desta cópia:** a porta anterior exige `pergunta_viva(p) == True`, e nada entre as duas chamadas altera a instância ou invalida seu memo. (`app/chamados/views.py:1238–1257`; `app/chamados/juizes.py:893–905`)

Correspondência da cópia com o hash informado `ab616d1c7dbb1bfda30e6accb10caeb6331071db`: [não verificado].

1. **LEITORES**

   - Na view, a primeira porta importa `chamados.models.pergunta_viva` como `_viva_porta`, consulta a pergunta e descarta aquela que não está viva; o retorno anterior usa “Pergunta nao encontrada ou disputa ja regularizada.” (`app/chamados/views.py:1238–1246`)
   - U9b importa a mesma função como `_viva` e lê `p.materializada_em`; pelo curto-circuito de `and`, só chama `_viva(p)` quando há carimbo. Sua mensagem está nas duas linhas seguintes indicadas. (`app/chamados/views.py:1256–1262`)
   - `chamados.models.pergunta_viva` é reexportação de `chamados.juizes.pergunta_viva`; o juiz também lê `materializada_em`, tanto na chave do memo quanto na projeção usada pelo cálculo. (`app/chamados/models.py:4224–4226`; `app/chamados/juizes.py:857,1389,1403`)
   - Há **citação explícita do ramo** na allowlist: `materializada AND not pergunta_viva`. Portanto, “ninguém cita o ramo” não corresponde à cópia. (`app/chamados/tests/test_contract_leitores_vitalidade.py:225`)
   - A função irmã `pergunta_sem_lastro_ou_viva` menciona a guarda da porta e lê os mesmos insumos pela expressão inversa: `materializada_em is None or pergunta_viva(p)`. (`app/chamados/juizes.py:1495–1499`)
   - Os dois formulários do template apontam para a view e ficam sob `p.viva`; essa propriedade delega ao juiz. (`app/templates/chamados/partials/_card_atendimento.html:77–78,169`; `app/chamados/models.py:3474–3484`)
   - A rota e os inventários citam a **view**, sem fixar U9b ou sua mensagem. (`app/chamados/urls.py:17`; `app/core/juizes.py:1065`; `app/docs/CENSO_PORTAS.md:23,180`; `app/docs/BACKLOG.md:2387`)
   - Não localizei outra ocorrência literal da mensagem fora da própria view. [não verificado]

2. **ESCRITORES — portas de `PerguntaDisputa.materializada_em`**

   | Porta | Escrita e persistência |
   |---|---|
   | `carimbar` | Aplica o carimbo, consulta o juiz, desfaz se viva e salva se aceito. `app/chamados/models.py:3212,3222–3228,3235` |
   | `carimbar_identidade` | Preserva ou atribui o instante e salva. `app/chamados/models.py:3265–3273` |
   | `descarimbar` | Zera instante, autor e via; salva por padrão, admitindo `salvar=False`. `app/chamados/models.py:3286–3295` |
   | `descarimbar_em_lote` | Zera os três campos por `update`. `app/chamados/models.py:3363–3365` |

   As portas de instância atribuem pelo helper `_escrever_carimbo`; `_desfazer_em_memoria` também o usa para restaurar valores, sem persistir nessa função. (`app/chamados/models.py:3180–3184,3242–3249`)

   O selo AST declara os dois sítios diretos da pergunta — helper e limpeza em lote — e verifica escritores fora dessa lista. A atribuição em `regularizacao_ext.py` pertence ao modelo homônimo de regularização externa. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:43–50,226–236`; `app/chamados/services/regularizacao_ext.py:199–201`)

3. **ALCANÇABILIDADE e mudança entre chamadas**

   - Se a primeira chamada retorna `False`, a view faz `p = None` e retorna antes de U9b. Logo, chegar a U9b implica que a primeira chamada retornou `True`. (`app/chamados/views.py:1242–1246`)
   - Entre essa porta e U9b há comentários e outro import; não há escrita, recarga da pergunta ou chamada a outra porta. As duas avaliações recebem o mesmo `p`, sem argumentos temporais ou carga. (`app/chamados/views.py:1242–1257`)
   - O juiz guarda **a chave posterior ao cálculo** e reutiliza o resultado quando ela coincide. A segunda chamada, nesse trecho, retorna o `True` memoizado; assim, `not _viva(p)` é falso. Uma alteração concorrente no banco não provoca nova consulta do cálculo nesse caminho. (`app/chamados/juizes.py:893–905`)
   - **Em geral, o resultado pode mudar:** mutações nos campos da chave invalidam o memo. Há testes de mudança por `validada_em`, carimbo/via e mutação sem `save`; essas mutações não aparecem entre as portas da view. (`app/chamados/juizes.py:854–861`; `app/chamados/tests/test_selo_memo_pergunta_viva.py:123–126,152–167`; `app/chamados/views.py:1242–1257`)

4. **TESTES EXISTENTES** [nao verificado]

   - A allowlist **cita**, mas não fixa a existência do ramo: a asserção só exige ausência de ofensores fora da lista. Remover U9b não viola essa exigência. (`app/chamados/tests/test_contract_leitores_vitalidade.py:225,319–322`)
   - `test_z1_pergunta_materializada_resposta_bloqueada` verifica preservação de texto/carimbo, mas envia para `/disputa/pergunta/<pk>/responder/`; a rota cadastrada é `/disputa/responder/<pk>/`. O caso não afirma status nem mensagem, portanto não comprova U9b. (`app/chamados/tests/test_chamados_views_s109.py:86–112`; `app/chamados/urls.py:17`)
   - O teste da função irmã fixa o predicado para materializada viva e morta, sem chamar a view. O contrato do template fixa o gate `p.viva`, também sem exigir U9b. (`app/chamados/tests/test_selo_juizes_orfaos.py:62–81`; `app/chamados/tests/test_contract_pergunta_viva.py:88–104`)
   - Não identifiquei teste existente cuja asserção fixe a mensagem ou exija o próprio ramo U9b. [não verificado] [nao verificado]

perguntas de lei: 0

## 16/11 09:58 · sessao 2e110cfbe865 · - · - · origem=Ronald (tmux)

Escrita de `PerguntaDisputa.materializada_em` concentrada em dois sítios: atribuição de instância em `app/chamados/models.py:3182` (`_escrever_carimbo`) e limpeza via `update` em `app/chamados/models.py:3363`/`:3365` (`descarimbar_em_lote`). O selo AST declara exatamente esses sítios para a pergunta e verifica escrita fora da lista em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:43` e `:226`.

Portas que persistem o campo:

- `carimbar`: aplica carimbo, consulta `pergunta_viva`, desfaz se viva e salva quando morta (`app/chamados/models.py:3212`, `:3222`, `:3226`, `:3235`).
- `carimbar_identidade`: preserva ou atribui instante e salva (`app/chamados/models.py:3265`, `:3268`, `:3273`).
- `descarimbar`: limpa os três campos e, por padrão, salva; admite `salvar=False` (`app/chamados/models.py:3286`, `:3291`, `:3294`).
- `descarimbar_em_lote`: limpa instante, autor e via conjuntamente (`app/chamados/models.py:3335`, `:3363`).

Chamadores diretos de produção encontrados:

| Porta | Chamadores |
|---|---|
| `carimbar` | `lavrar_sem_turno_aberto` (`app/chamados/reconciliador.py:406`); `retratar_perguntas_absolvidas_por_celula` (`app/chamados/reconciliador.py:596`); `encerrar_fantasma` (`app/chamados/models.py:3386`); `_materializar_ausencia_de_resposta` (`app/chamados/services/materializacao.py:567`); `_materializar_uma` (`app/chamados/services/materializacao.py:1283`); `materializar_pendentes_aprovadas` (`app/chamados/services/materializacao.py:1437`); `_resolver_perguntas_por_chamado_resolvido` (`app/chamados/services/disputa_emissao.py:2007`, `:2081`). |
| `carimbar_identidade` | `remover_motivo` (`app/chamados/models.py:2125`); `fechar_por_admin` (`app/chamados/models.py:3147`); `_materializar_resposta_hora` (`app/chamados/services/materializacao.py:930`); `_carimbar_recusa` (`app/chamados/services/materializacao.py:1105`); `retratar_duplicata` (`app/chamados/services/fio.py:1398`). |
| `descarimbar` | `_universo_da_ressuscitacao`, em memória (`app/chamados/models.py:1553`); `reabrir_fantasma` (`app/chamados/models.py:3397`); `reabrir_por_resposta`, sem salvar (`app/chamados/models.py:3414`); `reabrir_por_competencia` (`app/chamados/models.py:3421`); `desfazer_carimbo_sem_lastro` (`app/chamados/services/relavra_via.py:246`). |
| `descarimbar_em_lote` | `ressuscitar_perguntas` (`app/chamados/models.py:1613`); `reabrir_para_correcao` (`app/chamados/services/disputa_emissao.py:1648`). |

O campo homônimo escrito em `app/chamados/services/regularizacao_ext.py:199` pertence à regularização externa, excluída explicitamente do censo da pergunta pelo selo (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`). A migração cria o campo anulável, sem carga de dados (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21`).

Executei apenas em memória o scanner AST existente; retornou os dois sítios acima para a pergunta. Seu alcance exclui `__dict__` e nomes trazidos de outro módulo/atributo, conforme declaração explícita (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`). Nenhuma execução Django/banco foi feita [não verificado].

## 16/11 09:58 · sessao b46d93639d17 · - · - · origem=Ronald (tmux)

- A mensagem inteira está em `app/chamados/views.py:1261` e `:1262`; não encontrei outra ocorrência literal nas fontes `.py/.html/.md` pesquisadas. [não verificado]
- Há **citação explícita do ramo** na allowlist de `app/chamados/tests/test_contract_leitores_vitalidade.py:225`. Esse teste detecta comparações cruas de carimbo (`:307-314`) e só reprova ofensores **fora** da allowlist (`:319-322`); portanto não exige a existência do ramo, nem sua execução, nem a mensagem (`:225`, `:319-322`).
- Há regra irmã em `app/chamados/juizes.py:1495-1499`: `pergunta_sem_lastro_ou_viva(p)` devolve `materializada_em is None or pergunta_viva(p)`, com docstring remetendo à porta. O teste `app/chamados/tests/test_selo_juizes_orfaos.py:62-81` fixa essa função irmã para materializada viva e materializada morta; não chama a view (`:68-81`).
- `app/chamados/tests/test_chamados_views_s109.py:86-112` declara testar bloqueio da resposta materializada e verifica preservação do texto/carimbo (`:107-112`), mas o POST usa `/chamados/disputa/pergunta/<pk>/responder/` (`:102`), enquanto a rota real é `/chamados/disputa/responder/<pk>/` (`app/chamados/urls.py:17`). Não há asserção de status ou mensagem no caso (`test_chamados_views_s109.py:99-112`); logo ele não comprova U9b. O próprio `test_hx_simetria_toque.py:58-63` registra a correção desse problema de URL manual e impede 404 no seu helper.
- Outros testes da view: `test_hx_simetria_toque.py:66-92` fixa toque/reenvio/escape; `test_responder_web_falta.py:33-42` fixa materialização de falta pelo caminho web; `test_smoke.py:170`, `:181-185` fixa somente resolução de URL. Nenhum desses fixa a mensagem U9b nas asserções citadas. [nao verificado]
- Templates: formulários da rota em `app/templates/chamados/partials/_card_atendimento.html:78` e `:169`, dentro do gate `p.viva` aberto em `:77`; o contrato de template verifica esse gate e sua ordem em `app/chamados/tests/test_contract_pergunta_viva.py:88-104`.
- Outros registros da **view**, sem fixar U9b: inventário em `app/core/juizes.py:1065`; documentação em `app/docs/BACKLOG.md:2387`, `app/docs/CENSO_PORTAS.md:23` e `:180`.

## 16/11 09:37 · sessao d8971b8baaa8 · CORTES-F2-F9-1611 · verificador · origem=Code

Referências **F2**, **F4** e **F6** abaixo designam os respectivos anexos `matriz.md`, com linhas contadas desde a primeira linha de cada conteúdo.

1) **NAO PROCEDE.** “Integrais” e “parciais” já classificam a prova existente; não dependem do resultado dos testes novos. “Ramo sem mordida” corresponde às lacunas dos quatro parciais; U9b é um degrau identificado. (`F2:100–101`; `F6:44`) [nao verificado]

A seleção anterior ao primeiro teste é executável pela regra **mais restritiva**: F2 tem marca no contrato coletivo do núcleo; F4 tem marca no próprio título do contrato de entrada. Aplicar (b) literalmente faz esses candidatos esperarem. O complemento não manda liberar um degrau mediante uma cláusula mais estreita quando o contrato coletivo está marcado. (`complemento1_cortes_f2_f9_1611.txt:2`; `F2:21–29`; `F4:20`) [nao verificado]

2) **NAO PROCEDE.** Marca somente na linha, somente no contrato ou nos dois lugares produz o mesmo encaminhamento: espera F1. Sem mordida no mutante novo, registra “NAO MORDE”, mutante tentado e fila da fatia. Se morder, segue para revisão antes do brief. Nos parciais, os ramos já cobertos ficam fora da tentativa; os ramos sem prova seguem esses mesmos dois encaminhamentos. (`complemento1_cortes_f2_f9_1611.txt:2`; `cortes_f2_f9_1611.txt:10`; `F2:100–101`)

3) **NAO PROCEDE — nenhuma colisão demonstrada.** Registrar uma tentativa sem mordida não a credita como prova nem a admite no brief. A autorização continua limitada a testes próprios e revisados, como permite a regra escrita. (`complemento1_cortes_f2_f9_1611.txt:2`; `cortes_f2_f9_1611.txt:10`; `CLAUDE.md:349–350`; `app/docs/PISTA.md:143–145`)

A ordem “dez integrais antes dos quatro parciais” continua aplicável **depois da espera determinada por (b)**; ela não exige executar dez antes da F1. A exclusão de S1/S2 coincide com a retirada desses degraus do escopo F6. (`complemento1_cortes_f2_f9_1611.txt:2`; `F2:101`; `F6:48–49`)

4) **NAO PROCEDE.** A contagem literal, considerando também as marcas dos contratos coletivos, fica:

| Fatia | Entram na seleção antes da F1 | Esperam F1 | Fora do escopo | Fundamento |
|---|---:|---:|---:|---|
| F2 | **0** | **14** | 0 | `F2:21–29`, `:100–101`; complemento:2 |
| F4 | **0** | **21** | 0 | `F4:20`, `:126–127`; complemento:2 |
| F6 | **1** | **8** | **2** | `F6:33–49`, `:94`; complemento:2 |

**F2 — todos esperam**, preservando a ordem posterior:

- Dez integrais: **M1b** (`F2:37`), **M4** (`F2:40`), **M7** (`F2:43`), **M11** (`F2:47`), **M13** (`F2:49`), **M14** (`F2:50`), **M15** (`F2:51`), **M17** (`F2:53`), **M18** (`F2:54`), **M19** (`F2:55`).
- Depois, quatro parciais, somente nos ramos sem prova: **M3** (`F2:39`), **M9** (`F2:45`), **M10** (`F2:46`), **M20** (`F2:56`). A classificação dez/quatro está expressa em `F2:101`.

**F4 — todos os 21 esperam**, pela marca do contrato de entrada, inclusive os que não repetem a marca na linha. (`F4:20`; `complemento1_cortes_f2_f9_1611.txt:2`)

- **M1** (`F4:34`), **M2** (`F4:35`), **M9b** (`F4:43`), **M13** (`F4:47`), **M16** (`F4:50`), **M17** (`F4:51`), **M19** (`F4:53`), **M23** (`F4:57`), **M24** (`F4:58`).
- **E4** (`F4:67`), **E5** (`F4:68`), **E7** (`F4:70`), **E9** (`F4:72`), **E17** (`F4:80`), **E18** (`F4:81`), **E19** (`F4:82`), **E20** (`F4:83`).
- **X1** (`F4:89`), **X2** (`F4:90`), **J1** (`F4:91`), **J2** (`F4:92`).

**F6:**

- Entra agora: **U9b** (`F6:44`).
- Esperam: **U1b** (`F6:33`), **U1c** (`F6:34`), **U2** (`F6:35`), **U3** (`F6:36`), **U6** (`F6:39`), **U8** (`F6:42`), **U9a** (`F6:43`), **U12** (`F6:47`).
- Fora, sem contar como espera: **S1** (`F6:48`) e **S2** (`F6:49`; `complemento1_cortes_f2_f9_1611.txt:2`).

A elegibilidade de U9b pela marca não comprova ausência de cruzamento com arquivos das obras em voo: **[nao verificado]**. As esperas por cruzamento e pela vaga continuam expressas no item 9. (`cortes_f2_f9_1611.txt:10`)

VEREDITO: todos NAO PROCEDE

## 16/11 09:36 · sessao e0de1a397847 · - · - · origem=Ronald (tmux)

A leitura **F2=0, F4=0, F6=1 agora** é defensável como aplicação literal mais restritiva, sem inventar regra nova. Minha objeção anterior deu peso excessivo à seleção específica por cláusula. (`complemento1_cortes_f2_f9_1611.txt:2`)

**A favor da leitura global:** F2 nomeia expressamente um “Contrato lido pelos degraus (núcleo)” e inclui nele uma marca `[DEPENDE-F1]`; F4 marca o próprio título do “CONTRATO DE ENTRADA”. Como (b) determina esperar quando marcado em qualquer dos dois lugares, não exige provar independência efetiva nem procurar uma cláusula mais estreita para liberar testes. (`anexo F2/matriz.md:21`, `:27–29`; `anexo F4/matriz.md:20`; `complemento1_cortes_f2_f9_1611.txt:2`)

**Contra a leitura global:** F2 separa BASE, c3 e F1 dentro do núcleo, e F4 também enumera dependências por cláusula e por ID; isso permite uma leitura operacional mais específica. Porém, essa leitura liberaria degraus apesar da marca existente no contrato coletivo, contrariando a opção explicitamente mais restritiva. A existência de uma leitura menos restritiva não basta para tornar inexequível a regra que manda escolher a outra. (`anexo F2/matriz.md:22–29`, `:69–77`; `anexo F4/matriz.md:105–110`; `complemento1_cortes_f2_f9_1611.txt:2`)

**Conclusão independente:** eu não sustentaria PROCEDE apenas nessa ambiguidade. Declararia no cabeçalho a aplicação global dos contratos marcados: **F2 espera os 14 candidatos; F4 espera os 21; F6 entra somente U9b, espera oito e exclui S1/S2**. Depois da F1, F2 mantém dez integrais antes dos quatro parciais, estes somente nos ramos sem prova; isso não conflita com a espera anterior. (`anexo F2/matriz.md:100–101`; `anexo F4/matriz.md:126–127`; `anexo F6/matriz.md:33–49`, `:94`; `complemento1_cortes_f2_f9_1611.txt:2`)

## 16/11 09:34 · sessao e0de1a397847 · - · - · origem=Ronald (tmux)

F2, por vínculos expressos dos IDs às marcas:

- Agora: **7 integrais** — M11:47, M13:49, M14:50, M15:51, M17:53, M18:54, M19:55 — depois **M20 parcial:56**, só fora de `estado`. (`anexo F2/matriz.md:101`)
- Esperam: **3 integrais** — M1b:37, M4:40, M7:43 — e **3 parciais** — M3:39, M9:45, M10:46. Os contratos vinculam M1–M4/M8–M10 à dependência e mencionam M7 expressamente. (`anexo F2/matriz.md:74`, `:76`)

F4, por cláusulas específicas e aplicação restritiva da cláusula 3:

- Agora: **11** — M19:53, M24:58, E4:67, E5:68, E7:70, E17:80, E18:81, E19:82, E20:83, X1:89, X2:90. (`anexo F4/matriz.md`, linhas individuais)
- Esperam: **10** — M1:34, M2:35, M9b:43, M13:47, M16:50, M17:51, M23:57, E9:72, J1:91, J2:92. M1/M2/M9b/M13/M16/M17 estão no contrato marcado; M23 também; E9/J1 têm marca na linha; J2 aparece na cláusula marcada que o manda entrar junto ou virar fila. (`anexo F4/matriz.md:105–110`, `:72`, `:91`)
- Se a marca da cláusula 3 for limitada ao trecho anterior ao ponto e vírgula, J2 fica sem dependência: **12 agora/9 esperam**. (`anexo F4/matriz.md:109–110`)

F6:

- Agora: **U9b:44**, total **1**.
- Esperam: **8** — U1b:33, U1c:34, U2:35, U3:36, U6:39, U8:42, U9a:43, U12:47.
- Fora: **S1:48, S2:49**. (`anexo F6/matriz.md`, linhas individuais; `complemento1_cortes_f2_f9_1611.txt:2`)

A ambiguidade concreta restante é o alcance de **CONTRATO**: F4 tem o próprio título do contrato de entrada marcado, antes das cláusulas específicas; F2 tem núcleo contratual marcado sem uma lista fechada de IDs. Aplicar esses contratos globalmente faria **todos os respectivos candidatos esperarem**, produzindo seleção diferente. A operação “linha OU contrato” está definida; a associação de cada contrato aos degraus não está inteiramente fechada. (`anexo F4/matriz.md:20`, `:105–110`; `anexo F2/matriz.md:21–29`; `complemento1_cortes_f2_f9_1611.txt:2`)

Não há motivo para reprovar “integrais”, “parciais” ou “ramo sem mordida”: a matriz já define essas classes antes dos novos testes; o resultado futuro do RED não é necessário para classificá-las. A resposta (a) encaminha qualquer tentativa nova que não mate o mutante, incluindo tentativa num parcial. (`anexo F2/matriz.md:100–101`; `complemento1_cortes_f2_f9_1611.txt:2`)

## 16/11 09:33 · sessao 009e5360b3eb · COMPLEMENTO-1-CORTES-F2-F9-1611 · sonda · origem=Code

(1) **Total de linhas: 53. Total_erros: 0. Col237 está entre as linhas: sim.** [nao verificado]

(2) Empresa corresponde a `empresa_id`. “Sem historico” indica `troca_no_historico_para_o_cadastro: null`, sem afirmar ausência de outros registros históricos. [nao verificado]

| Col | Empresa (ID) | Posto do cadastro (ID — nome) | Posto do vínculo (ID — nome) | Início do vínculo | Troca no histórico para o cadastro | Referência |
|---|---:|---|---|---|---|---|
| col50 | 4 | 487 — ARAGUARI - PORTARIA — J.A Juliani Eireli | 459 — SHOPPING BOULEVARD - PORTARIA — J.A Juliani Eireli | 2026-08-01 | 2026-10-06 | [nao verificado] |
| col51 | 4 | 529 — PALHANO BUSINESS - TORRE 2 - ASG — R. A. de [nome] [nome] - G3 Force | 455 — PALHANO BUSINESS - SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-09-20 | 2026-07-10 | [nao verificado] |
| col55 | 2 | 516 — APOENA - VIGIA — R. A. de [nome] [nome] - G3 Force | 2 — APOENA - VIGIA — J.A Juliani Eireli | 2026-07-21 | 2026-07-09 | [nao verificado] |
| col69 | 3 | 644 — NORTE SHOPPING VGT — Juliani Segurança Patrimonial | 11 — CONDOMINIO GOLDEN PARK II — Juliani Segurança Patrimonial | 2026-10-29 | 2026-10-29 | [nao verificado] |
| col112 | 3 | 8 — ARTESANO - VIGILANTE — Juliani Segurança Patrimonial | 474 — FURGÕES IBIPORÃ - CONTROLADOR DE ACESSO — J.A Juliani Eireli | 2026-11-09 | 2026-11-10 | [nao verificado] |
| col128 | 3 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 448 — J.A BASE - SUPERVISOR DE VIGILANTE — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col142 | 4 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 22 — JULIANI SEGURANCA PATRIMONIAL LTDA — Juliani Segurança Patrimonial | 2026-06-01 | 2026-10-22 | [nao verificado] |
| col154 | 3 | 527 — BASE OPERACIONAL - J.A — J.A Juliani Eireli | 3 — CASA VISCARDI S/A COM E IMP (MTZ) — Juliani Segurança Patrimonial | 2026-06-24 | 2026-07-10 | [nao verificado] |
| col174 | 2 | null — null | 612 — ARCOS DOURADO LAT — J.A Juliani Eireli | 2026-08-26 | sem historico | [nao verificado] |
| col191 | 2 | 582 — ARCOS DOURADOS JSP — J.A Juliani Eireli | 590 — ARCOS DOURADOS JSO — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col216 | 2 | 536 — BASE OPERACIONAL — Juliani Segurança Patrimonial | 431 — VISCARDI QUINTINO-SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-07-21 | 2026-11-09 | [nao verificado] |
| col220 | 2 | 602 — ARCOS DOURADOS MOV — J.A Juliani Eireli | 599 — ARCOS DOURADOS DCA — J.A Juliani Eireli | 2026-11-10 | 2026-11-15 | [nao verificado] |
| col225 | 2 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 448 — J.A BASE - SUPERVISOR DE VIGILANTE — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col236 | 2 | 642 — FUNDACAO COPEL MARINGA — J.A Juliani Eireli | 1 — A definir — Confiance Force Ltda | 2026-07-21 | 2026-07-31 | [nao verificado] |
| col237 | 2 | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 487 — ARAGUARI - PORTARIA — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col263 | 2 | 593 — ARCOS DOURADOS FZD — J.A Juliani Eireli | 586 — ARCOS DOURADO RXV — J.A Juliani Eireli | 2026-11-04 | 2026-11-05 | [nao verificado] |
| col303 | 2 | 414 — PALHANO BUSINESS - TORRE 1 — J.A Juliani Eireli | 412 — LONDRINA NORTE SHOPPING - PORTEIRO — R. A. de [nome] [nome] - G3 Force | 2026-07-21 | 2026-08-28 | [nao verificado] |
| col306 | 2 | 414 — PALHANO BUSINESS - TORRE 1 — J.A Juliani Eireli | 24 — BASE ADMINISTRATIVA — J.A Juliani Eireli | 2026-07-21 | 2026-09-11 | [nao verificado] |
| col341 | 2 | 487 — ARAGUARI - PORTARIA — J.A Juliani Eireli | 457 — SOLAR ALTA VISTA - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | 2026-10-22 | [nao verificado] |
| col358 | 2 | 642 — FUNDACAO COPEL MARINGA — J.A Juliani Eireli | 446 — FUNDAÇÃO COPEL - SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-07-21 | 2026-07-31 | [nao verificado] |
| col389 | 2 | 415 — PALHANO BUSINESS - TORRE 2 - VIGIA — R. A. de [nome] [nome] - G3 Force | 457 — SOLAR ALTA VISTA - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | 2026-11-10 | [nao verificado] |
| col398 | 2 | 495 — CONCENTRIX CURITIBA MURICY — J.A Juliani Eireli | 493 — CONCENTRIX  CURITIBA JL3 — J.A Juliani Eireli | 2026-07-21 | 2026-07-13 | [nao verificado] |
| col442 | 2 | 469 — LEON - MRV — J.A Juliani Eireli | 466 — LE MONDE - MRV — J.A Juliani Eireli | 2026-07-21 | 2026-09-07 | [nao verificado] |
| col464 | 2 | 589 — LA ROCHE - MRV — J.A Juliani Eireli | 467 — LANCELOT - MRV — J.A Juliani Eireli | 2026-07-21 | 2026-08-05 | [nao verificado] |
| col468 | 2 | 622 — CONCENTRIX RENDICAO ALMOÇO — J.A Juliani Eireli | 495 — CONCENTRIX CURITIBA MURICY — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col511 | 2 | null — null | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 2026-08-05 | sem historico | [nao verificado] |
| col556 | 2 | 495 — CONCENTRIX CURITIBA MURICY — J.A Juliani Eireli | 493 — CONCENTRIX  CURITIBA JL3 — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col584 | 2 | 626 — ARCOS DOURADOS ANG — J.A Juliani Eireli | 590 — ARCOS DOURADOS JSO — J.A Juliani Eireli | 2026-08-28 | 2026-11-10 | [nao verificado] |
| col607 | 2 | 429 — VISCARDI DUQUE DE CAXIAS- SERVIÇOS GERAIS — J.A Juliani Eireli | 430 — VISCARDI GUARANIS — J.A Juliani Eireli | 2026-07-21 | 2026-07-10 | [nao verificado] |
| col608 | 2 | 413 — NORTE SHOPPING J.A - BOMBEIROS — J.A Juliani Eireli | 6 — LONDRINA NORTE ADMINISTRADORA DE SHOPPING - JSP — Juliani Segurança Patrimonial | 2026-07-21 | 2026-07-10 | [nao verificado] |
| col626 | 2 | 595 — SOLAR DI MILANO  PORTARIA — J.A Juliani Eireli | 509 — ASSOCIAÇÃO ARTESANO LONDRINA - PORTARIA — J.A Juliani Eireli | 2026-07-21 | 2026-08-17 | [nao verificado] |
| col627 | 2 | 11 — CONDOMINIO GOLDEN PARK II — Juliani Segurança Patrimonial | 439 — GOLDEN PARK 2 - VIGIA — J.A Juliani Eireli | 2026-07-21 | 2026-07-10 | [nao verificado] |
| col634 | 2 | 591 — ARCOS DOURADOS DPI — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-07-21 | 2026-07-16 | [nao verificado] |
| col642 | 3 | 651 — BASE RANGEL PESTANA — Juliani Segurança Patrimonial | 24 — BASE ADMINISTRATIVA — J.A Juliani Eireli | 2026-10-05 | 2026-10-22 | [nao verificado] |
| col647 | 2 | null — null | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col673 | 2 | 615 — CONCENTRIX M2 — J.A Juliani Eireli | 493 — CONCENTRIX  CURITIBA JL3 — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col717 | 2 | 583 — ARCOS DOURADOS DVH — J.A Juliani Eireli | 582 — ARCOS DOURADOS JSP — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col719 | 2 | 614 — CONCENTRIX M3 — J.A Juliani Eireli | 617 — CONCENTRIX JL2 — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col787 | 2 | 555 — CONDOMINIO VILLE CAMPESTRE - MULTI FUNCAO — J.A Juliani Eireli | 444 — CONDOMINIO VILLE CAMPESTRE - PORTEIRO — J.A Juliani Eireli | 2026-07-21 | 2026-07-02 | [nao verificado] |
| col849 | 2 | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 549 — ARCOS DOURADOS  MC . CBV — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col859 | 2 | 659 — SALGADOMAC — J.A Juliani Eireli | 415 — PALHANO BUSINESS - TORRE 2 - VIGIA — R. A. de [nome] [nome] - G3 Force | 2026-10-07 | 2026-11-13 | [nao verificado] |
| col861 | 2 | 650 — TUO - EMPREENDIMENTOS — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-07-21 | sem historico | [nao verificado] |
| col865 | 2 | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-10-01 | 2026-10-16 | [nao verificado] |
| col917 | 2 | 421 — ARCOS DOURADOS - ARC — J.A Juliani Eireli | 609 — ARCOS DOURADO MC — J.A Juliani Eireli | 2026-08-18 | 2026-10-02 | [nao verificado] |
| col920 | 2 | 585 — ARCOS DOURADOS CWB — J.A Juliani Eireli | 424 — ARCOS DOURADOS CAB — J.A Juliani Eireli | 2026-09-01 | 2026-11-06 | [nao verificado] |
| col932 | 2 | 656 — Sunset Dream Residence — J.A Juliani Eireli | 459 — SHOPPING BOULEVARD - PORTARIA — J.A Juliani Eireli | 2026-09-13 | 2026-11-09 | [nao verificado] |
| col933 | 2 | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 421 — ARCOS DOURADOS - ARC — J.A Juliani Eireli | 2026-09-04 | 2026-11-13 | [nao verificado] |
| col961 | 2 | 433 — VISCARDI ALPHAVILLE — J.A Juliani Eireli | 456 — SOLAR ALTA VISTA - SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-10-29 | 2026-11-04 | [nao verificado] |
| col962 | 2 | 431 — VISCARDI QUINTINO-SERVIÇOS GERAIS — J.A Juliani Eireli | 652 — CASA VISCARDI CD — J.A Juliani Eireli | 2026-09-25 | 2026-11-10 | [nao verificado] |
| col988 | 2 | 586 — ARCOS DOURADO RXV — J.A Juliani Eireli | 637 — ARCOS DOURADOS FOLGUISTA — J.A Juliani Eireli | 2026-11-04 | 2026-11-10 | [nao verificado] |
| col990 | 2 | 430 — VISCARDI GUARANIS — J.A Juliani Eireli | 431 — VISCARDI QUINTINO-SERVIÇOS GERAIS — J.A Juliani Eireli | 2026-10-14 | 2026-11-09 | [nao verificado] |
| col999 | 2 | 659 — SALGADOMAC — J.A Juliani Eireli | 656 — Sunset Dream Residence — J.A Juliani Eireli | 2026-11-09 | 2026-11-12 | [nao verificado] |
| col1021 | 3 | 16 — PACAEMBU CAMPAGNOLE — Juliani Segurança Patrimonial | 531 — PACAEMBU - HAROLDO — Juliani Segurança Patrimonial | 2026-11-03 | 2026-11-12 | [nao verificado] |

(3) **42 têm data de troca para o posto do cadastro; 11 não têm essa data.** [nao verificado]

(4) A saída não responde:

- Qual posto deveria prevalecer nem a causa ou o responsável pela divergência. [nao verificado]
- A data efetiva da alteração do cadastro ou do vínculo; a data apresentada é a informada pelo campo de histórico. [nao verificado]
- O histórico completo ou a inexistência de registros além dos exibidos. [nao verificado]
- O nome da empresa correspondente a cada `empresa_id`. [nao verificado]
- A situação fora do recorte informado ou após o dump de 2026-11-16. [nao verificado]

## 16/11 09:28 · sessao 152e2fd156c8 · E4C-F1-NOVA-POUSO-B1 · verificador · origem=Code

**A comparação c2 × main `ab616d1c7`, as 283 linhas da sonda, `total_erros=0` e a execução da suíte estão [nao verificado].** O código elimina a produção de `REABERTURA` pela ressuscitação, mas os testes não certificam todos os estados nem toda a saída publicada da tarefa (`app/chamados/models.py:1629`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`).

Referências abaixo relativas a `app/`.

**1. PROCEDE — os leitores não atribuem todos o mesmo significado aos campos.**

A ressuscitação entrega os valores novos à porta; quem efetivamente grava é `descarimbar_em_lote`, enquanto as atribuições em `_universo_da_ressuscitacao` são simulação em memória (`chamados/models.py:1553`; `chamados/models.py:1629`; `chamados/models.py:3391`).

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `respondida_em` | Resposta normal/automática e recuperação pela trilha; `carimbar` pode completar o instante; ressuscitar o limpa pela porta (`chamados/services/disputa_emissao.py:1482`, `chamados/services/disputa_emissao.py:1675`, `chamados/services/disputa_emissao.py:1696`, `chamados/models.py:3242`, `chamados/models.py:1629`). | **PROCEDE:** C6 exige instante **e texto**; `estado`, propriedade `respondida`, partição do app e seleção de corrigíveis ainda usam o instante isoladamente (`chamados/juizes.py:1423`; `chamados/models.py:3155`; `chamados/models.py:3664`; `chamados/services/pergunta_no_app.py:81`; `chamados/juizes.py:1739`). |
| `resposta_colab` | Resposta normal/automática; reabertura e ressuscitação zeram pelo lote (`chamados/services/disputa_emissao.py:1481`; `chamados/services/disputa_emissao.py:1674`; `chamados/services/disputa_emissao.py:1609`; `chamados/models.py:1630`). | **NAO HA** conflito no significado de texto atual para C6, materializador e apresentação; entretanto, a sequência pode continuar contendo a resposta que a projeção apagou (`chamados/juizes.py:1423`; `chamados/services/materializacao.py:835`; `chamados/models.py:3688`; `chamados/models.py:4101`). |
| `validada_em` | Validação carimba, devolução/nova resposta desfazem, `carimbar` pode completar e ressuscitar limpa (`chamados/services/validacao.py:159`; `chamados/services/validacao.py:224`; `chamados/services/disputa_emissao.py:1494`; `chamados/models.py:3245`; `chamados/models.py:1629`). | **PROCEDE:** C6/estado/materialização leem validação atual; painel e placar contam o mesmo campo apagável como atividade humana histórica (`chamados/juizes.py:1405`; `chamados/models.py:3151`; `chamados/services/materializacao.py:1377`; `chamados/services/painel.py:256`; `chamados/services/verbos.py:263`). |
| `validada_por` | Acompanha os escritores da validação e sua limpeza (`chamados/services/validacao.py:160`; `chamados/services/validacao.py:225`; `chamados/services/disputa_emissao.py:1495`; `chamados/models.py:3246`; `chamados/models.py:1629`). | **PROCEDE:** o auxiliar preserva o autor como assinatura histórica somente havendo `validada_em`; painel/placar e procedência da batida consultam a assinatura atual, que ressuscitar apaga (`chamados/models.py:4102`; `chamados/services/painel.py:261`; `chamados/services/verbos.py:263`; `ponto/services/procedencia_batida.py:72`). |
| `materializada_em` | Atribuição central em `_escrever_carimbo`, usada pelas portas; volta em lote em `descarimbar_em_lote` (`chamados/models.py:3209`; `chamados/models.py:3239`; `chamados/models.py:3318`; `chamados/models.py:3393`). | **PROCEDE:** C6, estado e materializador leem carimbo atual; procedência e marcos da lavra também usam esse instante como informação histórica/de mudança, que pode desaparecer (`chamados/juizes.py:1450`; `chamados/models.py:3153`; `chamados/services/materializacao.py:1378`; `ponto/services/procedencia_batida.py:74`; `chamados/veredito_lavrado.py:202`). |
| `materializada_por` | Mesmo escritor central e porta do lote (`chamados/models.py:3210`; `chamados/models.py:3393`). | **PROCEDE:** limpar o autor é coerente com retirar o carimbo atual, mas a procedência da batida procura nesse campo a autoria histórica; C6 não o inclui entre seus insumos mutáveis (`ponto/services/procedencia_batida.py:72`; `chamados/juizes.py:855`). |
| `via_resolucao` | Escritor central, lote, encerramento por fato e relavra (`chamados/models.py:3211`; `chamados/models.py:3393`; `chamados/models.py:3402`; `chamados/services/relavra_via.py:125`). | **NAO HA** significado novo introduzido pela c2: universo/C6/SQL leem via de encerramento ou identidade, e descarimbar retira a via junto do carimbo (`chamados/models.py:1535`; `chamados/juizes.py:1416`; `chamados/juizes.py:1444`; `chamados/models.py:3483`). |
| Linhas de `DecisaoPergunta` | Ressuscitar chama o auxiliar e entrega suas linhas ao escritor `registrar`, que insere por `bulk_create` (`chamados/models.py:1613`; `chamados/models.py:4100`; `chamados/models.py:4104`; `chamados/models.py:4120`). | **PROCEDE:** a tarefa lê as linhas como decisões que determinam a próxima ação; o histórico considera uma RESPOSTA vigente até outra resposta ou REABERTURA/RECUSA, mesmo depois de a ressuscitação apagar sua projeção (`chamados/services/tarefa_do_dia.py:148`; `chamados/models.py:3674`). |

A abertura também aceita projeção inicial por `**campos` e registra suas linhas de resposta/validação; o `save` verifica a integridade do par (`chamados/models.py:3124`; `chamados/models.py:3129`; `chamados/models.py:3558`).

**2. PROCEDE — o universo alcançável é maior que as duas classes novas dos testes.** [nao verificado]

O filtro exige vínculo ao chamado, materialização presente, texto exatamente vazio — ou SQL `NULL` — e via fora de `VIAS_FORA_DO_FATO`; depois exige marco não provadamente aceso e C6 vivo na cópia limpa (`chamados/models.py:1530`; `chamados/models.py:1548`; `chamados/models.py:1550`; `chamados/models.py:1557`).

| `respondida_em` anterior | `validada_em` anterior | Entrada admitida, satisfeitos os demais guardas |
|---|---|---|
| Nulo | Nulo | Sim: carimbo sem resposta/validação (`chamados/models.py:1530`). |
| Preenchido | Nulo | Sim: legado instante sem texto (`chamados/models.py:1533`; `chamados/models.py:1555`). |
| Nulo | Preenchido | Sim: validação sem instante de resposta (`chamados/models.py:1530`; `chamados/models.py:1554`). |
| Preenchido | Preenchido | Sim: ambos são retirados da cópia antes do C6 (`chamados/models.py:1554`). |

Autores, sequência anterior, disputa aberta/fechada e `conversa` não recebem filtro próprio nesse universo; texto apenas com espaços não entra, e `NULL` está contemplado no filtro embora o campo atual não tenha `null=True` (`chamados/models.py:1530`; `chamados/models.py:2552`).

**NAO HA — produção de novo ato de devolução pela ressuscitação c2:** ela preserva projeções sem linha e chama diretamente a porta do lote (`chamados/models.py:1613`; `chamados/models.py:1629`).

Depois da operação:

| Leitor | Resultado |
|---|---|
| `tarefa_do_dia` | Sem decisão: `aguarda_resposta`; RESPOSTA vazia: `respondida_sem_texto`; RESPOSTA com texto: `aguarda_validacao`; RECUSA/REABERTURA anterior: `devolvida_ao_colaborador`; SILENCIO: nenhuma ação (`chamados/services/tarefa_do_dia.py:145`). |
| `tarefa_do_dia`, demais decisões | VALIDACAO depende da lavra/efeito; ABSORCAO depende do estado da ausência; a carga só considera decisões até `agora` (`chamados/services/tarefa_do_dia.py:156`; `chamados/services/tarefa_do_dia.py:167`; `chamados/services/tarefa_do_dia.py:412`). |
| `pergunta_viva` | Sob os mesmos insumos usados na seleção, devolve vivo; isso não é garantia incondicional após concorrência ou quando a célula projetada do julgamento difere da persistida (`chamados/models.py:1548`; `chamados/models.py:1557`; `chamados/juizes.py:415`; `chamados/juizes.py:1403`). |
| `perguntas_do_app` | Inclui a pergunta se passar pelo SQL e C6, independentemente de SILENCIO na sequência; com resposta nula e disputa aberta, a partição a chama de pendente (`chamados/services/pergunta_no_app.py:52`; `chamados/services/pergunta_no_app.py:61`; `chamados/services/pergunta_no_app.py:81`). |
| `medir` | Usa os destinos de todas as vivas como `no_app`, retirando sua causa de retenção; não mede a ausência de ação determinada por SILENCIO na tarefa (`chamados/services/pergunta_no_app.py:208`; `chamados/services/pergunta_no_app.py:141`; `chamados/services/tarefa_do_dia.py:165`). |

**PROCEDE — antes/depois não é invariância geral:** uma projeção legada sem linha pode sair de `passivo_sem_linha` para uma ação derivada da RESPOSTA/VALIDACAO que a operação acabou de registrar (`chamados/services/tarefa_do_dia.py:143`; `chamados/services/tarefa_do_dia.py:419`; `chamados/models.py:1613`).

**Comparação com main:** na entrada estável admitida pelo esquema atual, limpar `resposta_colab` regrava texto já vazio; igualdade das demais operações e inexistência de qualquer divergência com `ab616d1c7` permanecem **[nao verificado]** (`chamados/models.py:1533`; `chamados/models.py:1630`).

**3. PROCEDE — há preservação condicional da resposta, mas não uma foto completa de tudo que é retirado.**

- **NAO HA perda demonstrada do anterior quando existe instante sem linha correspondente:** RESPOSTA conserva instante/texto, e VALIDACAO conserva instante/texto/autor, antes da limpeza e no mesmo `atomic` (`chamados/models.py:1607`; `chamados/models.py:1613`; `chamados/models.py:4098`).
- **PROCEDE — entrada sem ambos os instantes não produz decisão:** o auxiliar retorna vazio, mas o lote apaga materialização, autor e via; a auditoria posterior guarda IDs e rótulos de estado, sem os valores anteriores desses carimbos (`chamados/models.py:4091`; `chamados/models.py:3393`; `chamados/models.py:1653`).
- **PROCEDE — autor órfão de validação também pode sumir sem foto:** o universo não o recusa, o auxiliar só o registra quando existe `validada_em`, e o lote o limpa (`chamados/models.py:1530`; `chamados/models.py:4102`; `chamados/models.py:1629`).
- **PROCEDE — a garantia depende da coerência anterior da sequência:** correspondência significa apenas pergunta+tipo+instante, sem conferir texto ou autor; uma linha correspondente com conteúdo divergente impede nova preservação (`chamados/models.py:4094`; `chamados/models.py:4098`; `chamados/models.py:4102`).
- **PROCEDE — na corrida com resposta chegando antes da trava, a trilha bruta pode conservar o texto sem apresentá-lo como anterior:** a c2 limpa a projeção, mas o leitor da tela só encerra a resposta vigente diante de outra RESPOSTA ou ato com foto (`chamados/models.py:1613`; `chamados/models.py:1629`; `chamados/models.py:3674`).

A ocorrência desses estados em produção está **[nao verificado]**; no estado sem resposta/validação, o texto anterior já é vazio, portanto a falta de foto dos carimbos não demonstra, por si, perda de texto exigido pela L-130 (`chamados/models.py:1533`; `docs/LEIS.md:151`).

**4. NAO HA — a porta do lote não adquire as travas; ela exige a trava do colaborador.**

- Recusa kwargs que tentem controlar os próprios campos do carimbo e recusa par **futuro** ilegal; os `outros` desta chamada, `(None, '')`, formam par legal mesmo sobre legado ilegal (`chamados/models.py:3365`; `chamados/models.py:3370`; `chamados/services/fio.py:1605`; `chamados/models.py:1629`).
- Exige colaboradores das disputas registrados como travados, exclui vias fora do fato e executa o `update`; não repete C6 nem o guarda do marco (`chamados/models.py:3385`; `chamados/models.py:3391`).
- A ordem é universo/disputas antes do `atomic`, depois trava global, preservação/`registrar`, captura do evento, limpeza, evento e reabertura das disputas fechadas (`chamados/models.py:1593`; `chamados/models.py:1607`; `chamados/models.py:1612`; `chamados/models.py:1613`; `chamados/models.py:1628`; `chamados/models.py:1638`).
- A trava global segue Colaborador → Disputa → Chamado → Pergunta, por PK; `registrar` insere e emite evento sem adquirir essa ordem por conta própria (`chamados/models.py:2771`; `chamados/models.py:2780`; `chamados/models.py:2793`; `chamados/models.py:2797`; `chamados/models.py:4119`).

**5. PROCEDE — os testes mordem a volta de REABERTURA, com limites claros.** [nao verificado]

Abaixo, “pega” é avaliação estática das asserções; execução efetiva dos mutantes está **[nao verificado]**.

| Teste novo/alterado | Mutante que pega | Mutante que não pega | [nao verificado]
|---|---|---|
| `test_RED_ressuscitar_zera_o_par_com_o_anterior_na_trilha_sem_REABERTURA` | Reintroduzir REABERTURA ou retirar o registro do legado contraria as asserções de atos/trilha (`chamados/tests/test_f1nova_b1_juiz_respondida.py:281`). | Fazer o auxiliar sempre gravar `ato_texto=''`, perdendo texto chegado na corrida: a fixture e a expectativa já são vazias (`chamados/tests/test_f1nova_b1_juiz_respondida.py:264`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:285`). |
| `test_RED_ressuscitar_sem_decisao_nao_muda_a_tarefa_do_dia` | Recolocar `reabrir_para_correcao` muda a razão para devolução (`chamados/tests/test_f1nova_b1_juiz_respondida.py:331`; `chamados/services/tarefa_do_dia.py:152`). | Deixar de limpar `respondida_em` de legado: nesta fixture ele já é nulo (`chamados/tests/test_f1nova_b1_juiz_respondida.py:304`). |
| `test_RED_ressuscitar_com_SILENCIO_nao_devolve_ao_colaborador` | Reintroduzir REABERTURA substitui a leitura de SILENCIO pela devolução (`chamados/tests/test_f1nova_b1_juiz_respondida.py:334`; `chamados/services/tarefa_do_dia.py:152`; `chamados/services/tarefa_do_dia.py:165`). | Omitir o evento posterior à limpeza: as asserções comparam estado e leitura direta, não entrega do evento (`chamados/tests/test_f1nova_b1_juiz_respondida.py:325`). |
| `test_MORDE_2a_chamada_nao_dobra_o_ato` | Remover o filtro de materialização permite nova ressuscitação na segunda chamada (`chamados/models.py:1531`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:342`). | Criar REABERTURA somente na primeira chamada: a quantidade inicial é capturada livremente em `n1` (`chamados/tests/test_f1nova_b1_juiz_respondida.py:341`). |
| `test_a_arvore_real_tem_sitio_relacionado_e_o_reu_entre_eles` | Trocar a porta por `alvo.update(...)` perde a chamada exigida e reintroduz escrita relacionada (`core/tests/test_censo_escritas.py:206`; `core/tests/test_censo_escritas.py:217`). | Deixar chamada à porta em ramo morto e escrever diretamente por `PerguntaDisputa.objects...update`: o teste verifica presença sintática, sem fluxo de execução (`core/tests/test_censo_escritas.py:216`; `core/tests/test_censo_escritas.py:220`). |
| `test_MORDE_a_arvore_real_ainda_atribui_o_update_pelo_gerente_reverso` | Retirar `resposta_colab=''` da chamada faz desaparecer a âncora literal (`core/tests/test_censo_escritas.py:241`). | Inserir REABERTURA mantendo a chamada literal: suas verificações continuam limitadas à âncora e à inferência do varredor (`core/tests/test_censo_escritas.py:243`; `core/tests/test_censo_escritas.py:251`). |

**PROCEDE — a comparação chamada “fila” é parcial:** ela retorna somente razão/ações de uma instância, enquanto `fio`, `rota`, `fila` e `verbos` são calculados separadamente e dependem também das pendentes do app (`chamados/tests/test_f1nova_b1_juiz_respondida.py:311`; `chamados/services/tarefa_do_dia.py:610`; `chamados/services/tarefa_do_dia.py:635`).

**6. PROCEDE — existem chamadores e leitores externos ao diff afetados pelo protocolo de escrita.**

- Reabrir chamado, desarquivar e cartório chamam diretamente a porta; recebem a ressuscitação sem o ato REABERTURA da B1 anterior (`chamados/models.py:1498`; `chamados/views_arquivar.py:86`; `ponto/services/cartorio.py:1673`; `chamados/models.py:1623`).
- O cartório usa o retorno para contar ressuscitações e pedir rejulgamento; a c2 conserva esse protocolo de retorno (`ponto/services/cartorio.py:1692`; `chamados/models.py:1667`).
- O histórico da tela depende dos atos com foto e muda quando eles deixam de existir; esse efeito é particularmente relevante para resposta chegada na corrida (`chamados/models.py:3680`; `chamados/services/disputa_emissao.py:2119`).
- Materialização, validações históricas, procedência e marcos da lavra dependem dos campos apagados; atribuir essas consequências exclusivamente à c2, frente ao main, está **[nao verificado]** (`chamados/services/materializacao.py:1375`; `chamados/services/painel.py:261`; `ponto/services/procedencia_batida.py:72`; `chamados/veredito_lavrado.py:202`).

**7. Checklist das classes.**

| Classe | Achado |
|---|---|
| Segundo juiz duplicando registrado | **NAO HA no trecho c2:** seleção delega ao C6 e integridade delega aos predicados únicos (`chamados/models.py:1557`; `chamados/models.py:3372`; `chamados/services/fio.py:1603`). |
| Segundo escritor fora da porta | **NAO HA no trecho c2:** ressuscitar entrega a limpeza à porta do lote (`chamados/models.py:1629`; `chamados/models.py:3391`). |
| Relógio fora da borda | **PROCEDE, preexistente no caminho:** o universo chama C6 sem instante, e o fallback da janela lê `date.today()` internamente (`chamados/models.py:1557`; `chamados/juizes.py:1471`; `chamados/juizes.py:596`). |
| Derivação paralela de fonte única | **NAO HA nova no trecho c2:** universo, preservação e integridade reutilizam as respectivas autoridades (`chamados/models.py:1593`; `chamados/models.py:1613`; `chamados/models.py:3372`). |
| Selo/RED sem mordida | **NAO HA quanto à volta de REABERTURA:** há proibição direta e comparação com SILENCIO; cobertura integral de fila/trilha permanece insuficiente (`chamados/tests/test_f1nova_b1_juiz_respondida.py:281`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:334`; `chamados/tests/test_f1nova_b1_juiz_respondida.py:311`). |
| Ordem de trava | **NAO HA inversão nova demonstrada:** a ordem global antecede preservação e limpeza (`chamados/models.py:1612`; `chamados/models.py:1613`; `chamados/models.py:1629`). |
| Pouso antes do pré-requisito | **[nao verificado]:** os anexos estabelecem os pré-requisitos, mas não fornecem sua execução nem prova do pouso. | [nao verificado]
| Campo com dois significados | **PROCEDE, preexistente:** instante isolado ainda significa “respondida” em alguns leitores, enquanto C6 exige instante+texto; adicionalmente, a sequência pode tratar como vigente a resposta já retirada da projeção (`chamados/models.py:3664`; `chamados/juizes.py:1423`; `chamados/models.py:3674`; `chamados/models.py:1629`). |

## 16/11 09:25 · sessao cf0625b825b4 · - · - · origem=Ronald (tmux)

Auditoria estática concluída e enviada com referências.

- A porta valida o par futuro, exige colaborador travado e escreve sem adquirir novas travas; o chamador trava antes de registrar e zerar. `app/chamados/models.py:1612`, `app/chamados/models.py:3370`, `app/chamados/models.py:3385`.
- Os testes detectam a volta de REABERTURA, mas não provam preservação de texto na corrida; todas as fixtures alteradas têm texto vazio. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:264`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:285`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:304`.
- O teste do censo exige a chamada literal; inserir REABERTURA mantendo essa chamada escapa dele individualmente. `app/core/tests/test_censo_escritas.py:241`, `app/core/tests/test_censo_escritas.py:252`.
- Execução dos testes, sonda comparativa e pré-requisitos do pouso: [nao verificado]. [nao verificado]

_5997 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
