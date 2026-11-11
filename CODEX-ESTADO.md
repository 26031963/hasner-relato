# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 08:42** · publicado em **11/11/2026 08:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 08:42 · sessao cb191a43d778 · FIO-E0-V4-E3 · construtor · origem=Code

Cura escrita: marcador e exceção ao `nunca_bateu` ficam limitados à comp 11/2026 em diante pelo mesmo corte. [cartorio.py:1241](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1241).

Entrega: [diff completo](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/fio_e0_v4_e3_e3fix9.diff:1), [log das execuções](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_e3fix9_execucao.txt:1).

- **(a)** O isento recebe `regra=versao_para`, calculada pela competência de `janela_atual`, e entra com `regra >= 4`. Uma v5 mudaria `regra:4` para `regra:5` em toda célula de comp 11+, alterando o hash e provocando rejulgamento da frota. [cartorio.py:248](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:248), [cartorio.py:489](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:489), [cartorio.py:538](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:538).
- **(b)** Foi declarado `HORIZONTE_SEM_ADESAO='2026-11'`, com recusa dura de formato no import. O leitor usa o fim de `janela_atual`; não houve bump. [cartorio.py:270](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:270).
- **(c)** Somente NB passa `sem_adesao` em produção; o ramo normal omite o argumento. [cartorio.py:1245](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1245), [cartorio.py:1384](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1384).
- **N4:** classifico a projeção temporal como auxiliar: devolve cinco valores; `_pergunta_viva_calc` aplica a decisão de domínio. [juizes.py:1362](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/juizes.py:1362), [juizes.py:1383](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/juizes.py:1383).

Resultados locais, com comandos e tracebacks no [log](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_e3fix9_execucao.txt:4):

| Execução | RC | Resultado |
|---|---:|---|
| RED antes da cura | 1 | 2 FAIL, 0 ERROR |
| `test_e3_nb_impressao_memoria` | 0 | 24 testes OK | [nao verificado]
| `x1_mutantes --modo green --sem-banco` | 0 | 24 testes OK | [nao verificado]
| `sem_horizonte_x1` | 1 | Morreu nas comps 09 e 10 |
| `horizonte_na_10` | 1 | Morreu na comp 10 |
| Quatro mutantes anteriores sem banco | 1 cada | Mortos por assertion |

Os REDs históricos mordem no `assertEqual` do hash em [memória:284](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:284). Os demais mordem nas assertions **269, 271, 341 e 381**, discriminadas com o comando de cada um no [relato:146](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_x1.txt:146).

C4/09 exportada e C4b/10 exigem foto NB e conservação dos registros de fechamento/exportação; a assertion ORM está em [teste:283](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:283). Execução ORM: **[nao verificado]**. [nao verificado]

N3 foi atualizado; comandos completos de ORM, mutantes ORM, suite e ruff estão no [relato:106](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_x1.txt:106). `git diff --check`: RC=0. [log:254](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_e3fix9_execucao.txt:254). Certificação Code/opus e efeito N2 após consumidor: **[nao verificado]**. [nao verificado]

## 11/11 08:33 · sessao 14c7f9cfa684 · BO-ADMIN-1111 · sonda · origem=Code

**08 — Dono: FALTA INFORMACAO — falta individualizar o dia e demonstrar o estado do fio.** A classificação ESTRUTURA exige divergência comprovada, conforme [LEIS.md:122](app/docs/LEIS.md:122).

1. **Candidato e prova dos DADOS:** a sonda informa `candidatos=1 [(62, None, 3)]`, col62. Contudo, o dia do BO é [nao verificado]: há **dois dias de trabalho sem entrada exibida**, **12/11/2026**, célula **140442**, e **14/11/2026**, célula **140444**; ambas com `trabalha=True`. Não escolho entre elas. O trecho também mostra **11/11 como folga**, célula **140441**, `trabalha=False`. Esses são valores do recorte fornecido, sem conferência externa [nao verificado].

2. **Chamado, pergunta e fio vivo:** os DADOS exibem `37684 ... status_local=resolvido` e `37685 ... celula_id=140438`, correspondente a **08/11**. Não exibem perguntas de col62. Isso não comprova inexistência de chamado anterior à janela de `criado_em`, nem ausência de pergunta/fio vivo em 12 ou 14/11 [nao verificado].

   **Universo do contador:** `fio_morto_de_hoje_pra_frente` é uma chave produzida por `censo()`. A função exige primeiro chamado vivo com célula, módulo de furo, célula acusadora e ausência de fio vivo; somente depois testa `data >= hoje`. Portanto, dia futuro sem entrada/chamado não entra automaticamente nesse contador ([vigia_fio.py:335](app/chamados/services/vigia_fio.py:335), [349](app/chamados/services/vigia_fio.py:349), [378](app/chamados/services/vigia_fio.py:378), [417](app/chamados/services/vigia_fio.py:417), [420](app/chamados/services/vigia_fio.py:420)). **A pertença de 12 e 14/11 ao universo permanece [nao verificado]**: faltam os vereditos integrais e os vínculos/estados de chamados e perguntas dessas células. O texto atual de `app/docs/ESTADO.md` também é [nao verificado]; o arquivo não foi localizado nesta cópia.

3. **Complemento:** a conferência do universo e do leitor cabe em **FIO-E0-V4-E3-LEITORES**, aberta e incluindo `vigia_fio`; sem ID novo ([BACKLOG.md:1865](app/docs/BACKLOG.md:1865)).

4. **Texto ao admin:** “Abra o calendário de col62 e informe a data exata que aparece sem entrada e sem chamado.” A ação fica no calendário ([views.py:1947](app/colaboradores/views.py:1947)).

**09 — Dono: FALTA INFORMACAO — faltam nome/MIME recebidos, conteúdo do arquivo e resposta efetiva do download `.ms`.** A causa da extensão relatada é [nao verificado].

1. **Candidato e prova dos DADOS:** a sonda informa `candidatos=1 [(872, None, 2)]`, col872; exibe apenas `Ausencia id=5422 | tipo=atestado | data=06/11/2026 | documento=…heic | status=aguardando_decisao`. Há **zero anexos gravados com `.ms` exibidos no recorte**; a associação desse registro ao download relatado é [nao verificado]. Não seleciono outro anexo.

2. **Upload e persistência pelo código:** a API recebe `documento`, registra seu `content_type` em log e normaliza o nome antes de seguir ([api/views.py:1772](app/api/views.py:1772), [1777](app/api/views.py:1777), [1784](app/api/views.py:1784)). A normalização reconhece o conteúdo e altera somente `f.name`; não altera o MIME recebido ([ausencia.py:587](app/ponto/services/ausencia.py:587)). O documento é persistido em `FileField`, sem MIME associado nesse campo ([models.py:777](app/ponto/models.py:777)). **Nome original e `content_type` efetivamente recebidos para #5422: [nao verificado].**

3. **Download pelo código:** usa o documento da referência ou do próprio registro, lê o conteúdo e, quando reconhecido, ajusta a extensão da resposta e seu `Content-Type`; entrega o arquivo aberto por `FileResponse`, sem conversão ([views.py:2115](app/ponto/views.py:2115), [2122](app/ponto/views.py:2122)). **Se o conteúdo for HEIC reconhecido**, devolve nome terminado em `.heic` e `image/heic` ([ausencia.py:582](app/ponto/services/ausencia.py:582)). Se desconhecido, o detector retorna `(None, None)` e a view preserva o basename ([ausencia.py:584](app/ponto/services/ausencia.py:584), [views.py:2124](app/ponto/views.py:2124)). **Conteúdo real e headers/nome efetivamente devolvidos para #5422: [nao verificado].**

4. **Complemento:** a conferência cabe em **UI-ANEXO-AUSENCIA**, já registrada e abrangendo conteúdo, nome e HEIC; sem ID novo e sem atribuir `.ms` à falta de conversão ([BACKLOG.md:1130](app/docs/BACKLOG.md:1130)).

5. **Texto ao admin:** “Em Ausências, abra o atestado de 06/11 de col872, clique em **Abrir** e **Baixar** e informe o nome completo salvo pelo navegador.” Os dois botões existem no painel ([painel_ausencia.html:84](app/templates/ponto/partials/painel_ausencia.html:84)).

**Lei de desenho e censo ESTRUTURA:** não aplicados aos dois itens, pois a divergência estrutural permanece [nao verificado]; L-099 não autoriza transformar ausência de informação em causa estrutural ([LEIS.md:122](app/docs/LEIS.md:122)).

## 11/11 08:33 · sessao a2f9ab32bc39 · - · - · origem=Ronald (tmux)

Item 08 — **FALTA INFORMACAO**: há 1 colaborador candidato (`col62`), mas o dado não individualiza o dia. Há dois dias de trabalho futuros sem batida exibida: **12/11**, célula **140442**, e **14/11**, célula **140444** (`DADOS: CelulaDia`). Não escolher entre eles. A causa da ausência de entrada é [nao verificado].

- **Célula:** ambas existem e têm `trabalha=True`; `DiaPago` mostra previsto de 720 minutos e zero trabalhado (`DADOS: CelulaDia 140442/140444; DiaPago 1978122/1978123/1978139/1978140`). O veredito integral dessas células é [nao verificado].
- **Chamado/pergunta/fio vivo:** o recorte traz `37684` resolvido e `37685` ligado à célula **140438**, de **08/11** (`DADOS: ChamadoColaborador; CelulaDia`). Não traz pergunta para `col62`; inexistência global ou vitalidade nas datas 12/14 é [nao verificado]. A seleção de chamados é por `criado_em`, portanto a lista não prova ausência de chamado anterior à janela [nao verificado].
- **Universo do contador:** `fio_morto_de_hoje_pra_frente` é chave produzida por `censo()`, não função autônoma. A função primeiro exige chamado em `VIVOS` com célula ([vigia_fio.py](/home/ronald/codex_ws/d0e71018b45e104698b8aa111e6fb148ce6751e8/app/chamados/services/vigia_fio.py:335)), módulo de furo (:349), célula acusando (:378), ausência de fio vivo (:417); somente depois classifica `data >= hoje` nessa chave (:420). **Ser futuro, ter escala e não ter entrada não basta.** A pertença das duas datas ao universo não está demonstrada [nao verificado].
- O juiz do cartório recusa data futura ([cartorio.py](/home/ronald/codex_ws/d0e71018b45e104698b8aa111e6fb148ce6751e8/app/ponto/services/cartorio.py:1985)); os vereditos acusatórios são declarados em [models.py](/home/ronald/codex_ws/d0e71018b45e104698b8aa111e6fb148ce6751e8/app/escala/models.py:1767). Não há prova de contradição estrutural nesses dois dias [nao verificado].
- **Lei/censo:** sem ESTRUTURA demonstrada, não atribuir lei de desenho violada nem apresentar escritores/leitores como causa. L-099 separa ESTRUTURA de CADASTRO/BATIDA ([LEIS.md](/home/ronald/codex_ws/d0e71018b45e104698b8aa111e6fb148ce6751e8/app/docs/LEIS.md:122)).
- **Complemento:** a conferência do leitor e do universo cabe em **FIO-E0-V4-E3-LEITORES**, que segue aberta e inclui `vigia_fio`; sem novo ID ([BACKLOG.md](/home/ronald/codex_ws/d0e71018b45e104698b8aa111e6fb148ce6751e8/app/docs/BACKLOG.md:1865)).
- **Texto ao admin:** “Abra o calendário de col62 e informe a data exata que aparece acusando sem entrada e sem chamado.” A porta de resolução fica na célula do calendário ([views.py](/home/ronald/codex_ws/d0e71018b45e104698b8aa111e6fb148ce6751e8/app/colaboradores/views.py:1947)).

`app/docs/ESTADO.md` não foi localizado nesta cópia; seu texto atual é [nao verificado].

## 11/11 08:33 · sessao 9d2d38bb528a · - · - · origem=Ronald (tmux)

**09 — dono: FALTA INFORMACAO** para confirmar o `.ms`: faltam nome original e `content_type` recebidos, bytes do anexo e headers/nome efetivos do download. O único candidato informado é col872, `Ausencia #5422`, `data=06/11/2026`, `tipo=atestado`, `documento=… .heic`, `status=aguardando_decisao`; esses valores de produção só constam dos DADOS [nao verificado].

- **Upload:** `app/api/views.py:1772` recebe `documento`; `:1777-1782` apenas registra seu `content_type` em log; `:1784-1786` chama a normalização. `app/ponto/services/ausencia.py:587-596` detecta pelo conteúdo e altera apenas `f.name`; não grava MIME. `Ausencia.documento` é `FileField` em `app/ponto/models.py:777-781`, sem campo MIME no modelo.
- **Persistência:** `criar_ausencia` passa o documento ao modelo (`app/ponto/services/ausencia.py:350-351`, `:385`, `:417-418`); o anexo posterior é escrito por `anexar_documento` (`:846`, `:856-859`). Entradas normalizadas incluem API inicial (`app/api/views.py:1784-1786`), API de anexo (`:3114-3120`), web de anexo (`app/ponto/views.py:2141-2151`) e web de lançamento (`:2404-2405`, `:2492-2493`).
- **Download pelo código:** `app/ponto/views.py:2115-2129` resolve a referência, usa o basename persistido, abre o arquivo e chama `tipo_real_documento`; se reconhecido, troca a extensão no nome da resposta e define o MIME. Devolve os bytes abertos em `FileResponse`, sem conversão. HEIC reconhecido → `*.heic` e `image/heic` (`app/ponto/services/ausencia.py:582-583`); PDF/JPG/PNG → respectivos pares (`:556-578`); desconhecido → `(None,None)` (`:584`) e basename preservado (`app/ponto/views.py:2124-2129`). Resposta real `.ms` [nao verificado].
- **Lei e censo ESTRUTURA:** não aplicáveis como classificação comprovada. Se surgir divergência de leitores, referência canônica existente: **TESTEMUNHA LE, NAO RECALCULA**, `CLAUDE.md:20-22`; o leitor `Ausencia.tipo_real` também usa o detector (`app/ponto/models.py:844-854`).
- **Complemento:** cabe registrar a conferência do contrato de entrada/download em **UI-ANEXO-AUSENCIA**, já registrada e contendo a parte de HEIC/original/conversão, sem novo ID (`app/docs/BACKLOG.md:1130`). A linha histórica documenta bloqueio de conversão; disponibilidade atual de decodificador [nao verificado].
- **Texto ao admin:** “Em Ausências, abra o atestado de 06/11 de col872, clique **Abrir** e **Baixar** e informe o nome completo salvo pelo navegador.” Botões: `app/templates/ponto/partials/painel_ausencia.html:84-85`.

## 11/11 08:29 · sessao 40b0bd3a1e53 · BO-ADMIN-1111 · sonda · origem=Code

**01/04/13 pertencem à classe BUG-PAR já aberta.** A sonda mostra respostas recebidas e recusa de aplicação do intervalo; a obra cita expressamente col960/disputa 5644. Não é o caso “disputa sem pergunta”, pois existem perguntas respondidas. (`app/docs/BACKLOG.md:1929`, `app/docs/BACKLOG.md:1877`; trechos D1 abaixo.)

A classificação segue **L-099 — O ESTRUTURAL SE SEPARA DO DADO**. O juiz citado no pedido está nesta cópia em `app/ponto/management/commands/e6_oraculo.py`, com critérios em `:98` e montagem dos insumos em `:182`; não se pode completar seus insumos por palpite. (`app/docs/LEIS.md:122`.)

O texto recebido termina durante o item 06. Identificação, candidatos e registros dos itens 08/09, assim como o valor “3 às 06:48” do ESTADO, ficam **[nao verificado]**.

**Provas da sonda.** Os trechos abaixo foram copiados de DADOS; `[…]` indica somente campos omitidos.

**D1 — col960, itens 01/04/13:**

```text
candidatos=1 [(960, None, 2)]
id=51522 | celula_id=128547 | disputa_id=5644 | motivo=intervalo_saida
[…] resposta_colab=01:00 | respondida_em=08/11 11:04:01
| recusa_motivo=Pergunta #51522: batida vizinha #133794
(S 22/10 06:30) tem o mesmo tipo do plantio (S) - par quebrado;
nao materializado (guarda de paridad

id=51523 | celula_id=128547 | disputa_id=5644 | motivo=intervalo_volta
[…] resposta_colab=02:00 | respondida_em=08/11 11:04:11
| recusa_motivo=Pergunta #51523: batida vizinha #133756
(E 21/10 18:32) tem o mesmo tipo do plantio (E) - par quebrado;
nao materializado (guarda de paridad
```

**D2 — col129, item 02:**

```text
candidatos=1 [(129, None, 3)]
id=149397 | timestamp=08/11 17:30:08 | tipo=E | sequencia=1

id=140128 | data=08/11/2026 | trabalha=True | origem=gerada
| escala_geradora_id=853
| dna={'marcos': {'hf': '05:42', 'hi': '21:30', 'hfi': None, 'hii': None},
[…] protestos=['FURO_PARCIAL', 'BATIDA_ORFA_FORA_TOLERANCIA']
| ata={'orfas': ['17:30'], 'n_orfas': 1,
```

**D3 — col33, item 03:**

```text
candidatos=1 [(33, None, 4)]
id=5311 | tipo=atestado | data=29/10/2026 | data_fim=30/10/2026
[…] status=aprovada
[…] aprovada_em=09/11 08:50:45

id=141772 | data=29/10/2026
[…] veredito=concorde
[…] ausencia_id=5311
[…] status_dia=ausencia | status_rotulo=Ausencia registrada — o dia tem abono proprio

id=1989491 | mes=11 | ano=2026 | data=29/10/2026
[…] minutos_abonados=660.0 | minutos_previstos=660.0
| minutos_realizados=0.0 | furo_apurado=False
```

**D5 — col51, item 05:**

```text
candidatos=1 [(51, None, 4)]
id=150717 | timestamp=09/11 16:04:05 | tipo=S
[…] criado_em=10/11 06:59:56

id=37996
[…] 09/11 — marco 16:00 (saida) sem batida
[…] status_local=resolvido | via_encerramento=sistema
[…] resolvido_em=10/11 06:59:56
[…] criado_em=09/11 16:30:09

id=129423 | data=09/11/2026
[…] veredito=concorde
[…] status_dia=ok
```

**D6 — col49, item 06:**

```text
candidatos=1 [(49, None, 3)]
id=145526 | timestamp=04/11 06:57:48 | tipo=E | sequencia=1

id=132704 | data=04/11/2026 | trabalha=False
[…] veredito=fato_sem_previsao
[…] protestos=['FATO_SEM_PREVISAO']
| ata={'orfas': ['06:57'], 'n_orfas': 1,
[…] status_dia=folga_trabalhada
```

**Censo compartilhado dos ESTRUTURA 01/04/13.** O caminho envolvido grava a resposta, tenta materializar cada pergunta separadamente e recusa diante da vizinha do mesmo tipo. A lei quebrada é **L-099**, na exigência de coerência; os nomes canônicos do desenho da obra são **Aggregate Root / fonte única** e **Single Writer Principle / escritor único**, com o par como unidade já nomeada no BUG-PAR. (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/materializacao.py:1375`, `app/chamados/services/materializacao.py:875`, `app/docs/LEIS.md:122`, `app/docs/GLOSSARIO.md:11`, `app/docs/GLOSSARIO.md:12`, `app/docs/BACKLOG.md:1929`.)

| Campo/estado envolvido | Quem escreve | Quem lê |
|---|---|---|
| `resposta_colab`, `respondida_em` | `app/chamados/services/disputa_emissao.py:1467`; persistência `:1508` | `app/chamados/services/materializacao.py:432`; seleção para aplicação `:1369` |
| Recusa de paridade / `recusa_motivo` | Produção da recusa em `app/chamados/services/materializacao.py:878`; encaminhamento `:1297`; persistência `:1102` | `app/templates/chamados/partials/modal_fio.html:226`; explicação de bloqueio derivada em `app/chamados/services/materializacao.py:452` |
| `materializada_em`, autor e `via_resolucao` | Atribuição central em `app/chamados/models.py:3097`; chamada da porta em `app/chamados/services/materializacao.py:1277` | `app/chamados/services/materializacao.py:434`; seleção das ainda não aplicadas `:1369` |
| Batida resultante da aplicação | `app/chamados/services/materializacao.py:927` chama `registrar_batida` | Verificação de batida existente em `app/chamados/services/materializacao.py:447`; apresentação do efeito via `app/chamados/services/fio.py:335` e `app/templates/chamados/partials/modal_fio.html:230` |

A classe já tem obra: **BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS**, complemento **BUG-PAR-FROTA-1110**, dentro de **RAIZ-DA-DISPUTA F3/F4**. (`app/docs/BACKLOG.md:1929`, `app/docs/BACKLOG.md:1882`, `app/docs/BACKLOG.md:1883`.)

**Quadro único dos 13 itens.** Nos itens 07/10, “DECISAO DO ADMIN” indica orientação de uso das telas existentes, sem falha estrutural demonstrada pelos formulários e condições examinados. (`app/api/views_core.py:315`, `app/templates/colaboradores/partials/painel_vinculo.html:112`, `app/templates/colaboradores/partials/painel_vinculo.html:136`.)

| Item / pessoa | Dono e conclusão sustentada | Código e sonda | Obra / complemento | Texto ao admin |
|---|---|---|---|---|
| **01 — col960** | **ESTRUTURA.** Resposta recebida; aplicação recusada pela guarda de paridade. Mesma classe BUG-PAR. **Lei: L-099.** | **D1**; gravação em `app/chamados/services/disputa_emissao.py:1467`; aplicação individual em `app/chamados/services/materializacao.py:1375`; recusa em `:875`. | **COMPLEMENTO de BUG-PAR**, já identificado com col960/disputa 5644; RAIZ F3/F4. `app/docs/BACKLOG.md:1929`, `:1882`, `:1883`. | “Abra o chamado e confira saída e volta do intervalo do mesmo dia com o espelho; as respostas já estão registradas.” **D1**; `app/templates/chamados/partials/modal_fio.html:217`. |
| **02 — col129** | **FALTA INFORMACAO.** Há entrada às 17:30 contra previsão às 21:30; isso não decide se o cadastro ou o fato está errado. Faltam horário pretendido nas datas, janela de HE, CXR do motor e marcação concreta de “espúria”. | **D2**; o dono depende dos insumos em `app/ponto/management/commands/e6_oraculo.py:182`; CXR é publicado pelo motor em `app/ponto/motor_calculo_v2.py:1655`. Causa do rótulo **[nao verificado]**. | Candidato: **ESPURIA-E-O-QUE-E**, com apresentação em **ESPURIA-TELA**. Encaixe causal ainda **[nao verificado]**. `app/docs/BACKLOG.md:1805`, `:1815`. | “Confira nas datas apontadas o horário cadastrado e o trabalho realizado antes de decidir as horas extras.” **D2**; `app/ponto/management/commands/e6_oraculo.py:182`. |
| **03 — col33** | **FALTA INFORMACAO sobre a trava atual.** O atestado está aprovado, o dia está em ausência e há 660 minutos abonados. Faltam tela, competência e mensagem de bloqueio. | **D3**; aprovação em `app/ponto/services/ausencia.py:897`; efeito de abono no fechamento em `app/ponto/services/fechamento.py:693`. Trava causada pelo atestado **[nao verificado]**. | Não há encaixe em obra aberta comprovado. A linhagem **AUSENCIA-COBRE** consta feita em `app/docs/TICKETS.md:1221`; não sustenta atribuir nova falha. | “Confira o atestado aprovado de 29/10. Se a folha ainda bloquear, informe a competência e o recado exibido.” **D3**; aprovação `app/ponto/services/ausencia.py:897`. |
| **04 — col960** | **ESTRUTURA**, compartilhando a prova do 01: resposta persistida e plantio recusado. **Lei: L-099.** | **D1**; `app/chamados/services/materializacao.py:875`, `:1375`; `app/docs/LEIS.md:122`. | **COMPLEMENTO de BUG-PAR / BUG-PAR-FROTA-1110 / RAIZ F3/F4**. `app/docs/BACKLOG.md:1929`, `:1882`, `:1883`. | “No chamado, confira as duas respostas do intervalo e os horários do espelho do mesmo dia.” **D1**; `app/templates/chamados/partials/modal_fio.html:217`. |
| **05 — col51** | **BATIDA — chegada posterior, já resolvida.** A saída de 09/11 chegou em 10/11, no instante da resolução. Rede/offline como causa **[nao verificado]**. | **D5**; baixa condicionada aos marcos cobrados acesos em `app/ponto/management/commands/detectar_ausencias.py:403`. Não há chamado ainda aberto nesse registro. **D5**. | Correção estrutural não demonstrada. Lastro consta na obra fechada em `app/docs/BACKLOG.md:735`; recorrência comprovada pode ser examinada na linhagem **LASTRO-PARTE-2**, `:1760`. | “Consulte 09/11 no espelho e o chamado 37996: o registro chegou, o chamado está resolvido e o dia está regular.” **D5**. |
| **06 — col49** | **FALTA INFORMACAO.** Existem entradas isoladas em dias sem previsão, mas faltam a data/batida do relato e o lançamento da diária. Relação da diária com a marcação **[nao verificado]**. | **D6**. Existe retratação humana, com permissão e motivo, em `app/ponto/views.py:3274` e `app/ponto/services/retratar_humano.py:150`. O calendário omite o menu no ramo de batida órfã: `app/templates/colaboradores/partials/_calendario_grade.html:121`; ligação com a tela relatada **[nao verificado]**. | **ESPURIA-TELA** é candidato condicionado à identificação da tela; `app/docs/BACKLOG.md:1815`. A porta original de retratação já consta fechada em `app/docs/TICKETS.md:1256`. | “Identifique a data e a marcação e confira a diária lançada e se houve trabalho antes de decidir a regularização.” **D6**; motivo obrigatório em `app/ponto/services/retratar_humano.py:153`. |
| **07 — tela** | **DECISAO DO ADMIN — orientação de uso.** Na API, pendentes > 0 → **Responder DP**; senão mensagem do DP não lida em chamado vivo → **Chamados**; senão badge vazio. Na PWA, o cartão usa pendentes → **Responder DP**, senão **Chamados**. | `app/api/views_core.py:307`, `:315`; `app/templates/core/dashboard_colaborador_v2.html:80`. Pendente = pergunta viva, sem resposta e com disputa aberta: `app/chamados/services/pergunta_no_app.py:45`, `:71`. Sonda não se aplica. | Linhagem **FIO-E0-V4-E3-LEITORES**, `app/docs/BACKLOG.md:1865`; necessidade de complemento por divergência concreta **[nao verificado]**. | “Abra Responder DP para responder às perguntas pendentes; use Chamados para consultar o atendimento.” `app/templates/core/dashboard_colaborador_v2.html:78`, `:80`. |
| **08 — colNNN [nao verificado]** | **FALTA INFORMACAO.** Faltam candidato, pessoa, dia, célula e registros do chamado/pergunta. Pertencer aos três casos de 06:48 **[nao verificado]**. | O código conta célula sem fio vivo com `data >= hoje` em `fio_morto_de_hoje_pra_frente`; isso não identifica a pessoa. `app/chamados/services/vigia_fio.py:417`, `:420`. Trecho da sonda e valor do ESTADO **[nao verificado]**. | Candidato: **FIO-E0-V4-E4B**, `app/docs/BACKLOG.md:1875`; cobertura deste item **[nao verificado]**. | “Informe a data e o que aparece no espelho desse dia para conferir a cobrança.” Identificação e situação desse dia **[nao verificado]**. |
| **09 — colNNN [nao verificado]** | **FALTA INFORMACAO.** Faltam registro do anexo, nome/MIME do upload, bytes identificadores, rota usada e cabeçalhos/nome do download. Origem de `.ms` **[nao verificado]**. | Upload da API lê `content_type` e normaliza o nome: `app/api/views.py:1772`, `:1781`, `:1784`. PDF/JPEG/PNG/HEIC são reconhecidos pelo conteúdo; desconhecido retorna sem tipo: `app/ponto/services/ausencia.py:556`, `:563`. Download corrige extensão e MIME quando reconhecidos: `app/ponto/views.py:2118`. Sonda **[nao verificado]**. | Candidato: **UI-ANEXO-AUSENCIA**, `app/docs/BACKLOG.md:1130`; enquadramento do `.ms` depende da prova faltante. **[nao verificado]**. | “No painel da ausência, use Baixar e informe qual nome o arquivo recebeu.” `app/templates/ponto/partials/painel_ausencia.html:85`. |
| **10 — tela** | **DECISAO DO ADMIN — formulários distintos.** “Salvar posto” salva posto; “Salvar” no bloco da escala envia o vínculo da escala. A própria tela orienta vincular posto primeiro. | `app/templates/colaboradores/partials/painel_vinculo.html:112`, `:124`, `:129`, `:136`, `:175`. Escritas em `app/colaboradores/views.py:322` e `:409`. Sonda não se aplica. | Não há complemento causal demonstrado pelos dois botões. PORT-35/36/38 tratam verificação dessas portas em `app/docs/BACKLOG.md:2158`; defeito deste relato **[nao verificado]**. | “Salve o posto no primeiro bloco; depois escolha a escala, confira as datas e salve no bloco da escala.” `app/templates/colaboradores/partials/painel_vinculo.html:124`, `:133`, `:175`. |
| **11 — tela/regra** | **FALTA INFORMACAO sobre o cadastro vigente e a regra aplicável.** Existe cadastro genérico de tipos; **não há quota anual nos campos, formulário ou leitor examinados**. Tipo acompanhante cadastrado hoje e concessões já usadas **[nao verificado]**. | `app/ponto/models.py:562`, `:577`; `app/ponto/views_tipos_ausencia.py:67`; `app/ponto/catalogo/ausencias.py:240`. O legado `atestado_acompanhante` é mapeado para `declaracao` em `app/api/views.py:1765`. Regra automática específica “1 por ano” **[nao verificado]**. | O124 trata consumo do cadastro, já fechado: `app/docs/BACKLOG.md:1208`, `app/docs/TICKETS.md:240`. Obra aberta específica de quota anual **[nao verificado]**. | “Confira o tipo em Tipos de ausência e o histórico do colaborador antes de decidir o lançamento.” Cadastro em `app/ponto/models.py:562`; regra anual aplicável **[nao verificado]**. |
| **12 — tela** | **FALTA INFORMACAO para explicar a recusa concreta.** Faltam usuário, grupos, setores/competências, chamado e rota. Hoje há guarda de entrada do gestor e guarda contextual pelo departamento do chamado. | Entrada: `app/chamados/views.py:24`, `:35`, `:662`. Resposta rápida: `app/chamados/services/fio.py:708`. Dono do departamento e autorização: `app/chamados/utils.py:32`, `:47`, `:52`. Gestor geral passa; sem competências declaradas há permissão legada; demais exigem competência/perfil correspondente. `app/chamados/utils.py:57`. | Linhagem **RBAC-ESCRITA-POR-PAPEL**, aberta em `app/docs/BACKLOG.md:1741`; **AGIR-POR-DONO** consta feito em `app/docs/TICKETS.md:1167`. Relação com a recusa específica **[nao verificado]**. | “Confira o departamento responsável no chamado e encaminhe a resposta a quem já tem acesso a esse departamento.” `app/chamados/services/fio.py:714`; `app/chamados/utils.py:32`. |
| **13 — col960** | **ESTRUTURA**, mesma prova e mesma classe dos itens 01/04. **Lei: L-099.** | **D1**; `app/chamados/services/disputa_emissao.py:1467`; `app/chamados/services/materializacao.py:875`, `:1375`; `app/docs/LEIS.md:122`. | **COMPLEMENTO de BUG-PAR / BUG-PAR-FROTA-1110 / RAIZ F3/F4**. `app/docs/BACKLOG.md:1929`, `:1882`, `:1883`. | “Confira no chamado as duas respostas do intervalo junto com o espelho do mesmo dia; elas já estão registradas.” **D1**; `app/templates/chamados/partials/modal_fio.html:217`. |
| **Soma = 13** | **ESTRUTURA 3 + BATIDA 1 + DECISAO DO ADMIN 2 + FALTA INFORMACAO 7.** Contagem das linhas acima. | **01 a 13, sem novo ID atribuído.** | **Censo dos três ESTRUTURA compartilhado acima.** | — |

## 11/11 08:27 · sessao e04bd2b59315 · - · - · origem=Ronald (tmux)

**07 — DECISAO DO ADMIN (consulta sobre o rótulo).** Na API, `Responder DP` tem prioridade quando há perguntas pendentes; sem elas, mensagens do DP não lidas em chamados vivos produzem `Chamados`; sem ambos, o badge é `None` (`app/api/views.py:317-331`; cópia em `app/api/views_core.py:305-320`). Pergunta pendente significa pergunta viva, sem `respondida_em` e com disputa aberta (`app/chamados/services/pergunta_no_app.py:45-75`). Na PWA, o cartão mostra `Responder DP` com pendências e `Chamados` sem elas (`app/templates/core/dashboard_colaborador_v2.html:80`; `app/core/views.py:252-253`). **Admin:** “Abra Responder DP para responder às perguntas pendentes; Chamados permite consultar o atendimento.” Obras relacionadas: `FIO-E0-V4-E3-LEITORES`, ainda aberta (`app/docs/BACKLOG.md:1865`); nenhuma divergência concreta entre leitores foi demonstrada [nao verificado]. Sonda: item exclusivamente de código, conforme pedido; não há trecho de DADOS.

**10 — DECISAO DO ADMIN (uso dos formulários).** São formulários distintos: `Salvar posto` envia a `vincular_posto` (`app/templates/colaboradores/partials/painel_vinculo.html:112-125`), e `Salvar` da escala envia a `vincular_escala` (`app/templates/colaboradores/partials/painel_vinculo.html:136-180`). O posto é gravado por `salvar_inteiro(..., 'posto')` (`app/colaboradores/views.py:322-329`); a escala passa por `julgar_vinculo` e `executar_vinculo` (`app/colaboradores/views.py:409-423`). A tela orienta posto primeiro (`app/templates/colaboradores/partials/painel_vinculo.html:129-133`) e também tem `Salvar folgas` (`app/templates/colaboradores/partials/painel_vinculo.html:191-205`). **Admin:** “Salve o posto no primeiro bloco; depois escolha a escala, confira as datas e salve no bloco da escala.” Dois botões, por si, não demonstram quebra estrutural [nao verificado]. Obras próximas: PORT-35/36/38, ainda livres, tratam smoke dessas portas (`app/docs/BACKLOG.md:2158-2161`); BO2910-ESTRUTURA inclui outro defeito de `vincular_posto`, sem provar relação com os dois botões (`app/docs/BACKLOG.md:1039`). Não cabe inventar complemento causal [nao verificado]. Sonda: exclusivamente código.

**11 — FALTA INFORMACAO: falta demonstrar a regra aplicável e o cadastro atual em produção.** Existe cadastro genérico de tipos de ausência (`app/ponto/models.py:562-569`), mas seus campos não incluem limite anual (`app/ponto/models.py:577-609`); o formulário também não recebe quota anual (`app/ponto/views_tipos_ausencia.py:67-81`), e o leitor consome efeito, nome, documento, bloqueio, parcial, fim e médico (`app/ponto/catalogo/ausencias.py:240-252`). A porta de lançamento valida tipo, período, parcialidade, documento e sobreposição; não foi encontrada regra específica de acompanhante uma vez por ano nos caminhos examinados (`app/ponto/services/ausencia.py:193-209`; `app/ponto/services/checar_ausencia.py:15-40`). Existência atual do tipo e concessões já usadas: [nao verificado]. **Admin:** “Confira o tipo em Tipos de ausência e o histórico do colaborador antes de decidir o lançamento.” Obra relacionada O124 já fechada, sobre consumo da flag médico, sem regra anual (`app/docs/BACKLOG.md:1208`; `app/docs/TICKETS.md:240`). Obra aberta específica para quota anual: [nao verificado]. Sonda: exclusivamente código.

**12 — FALTA INFORMACAO: faltam usuário/perfis/setores e chamado/rota concretos.** O fio administrativo exige `_is_gestor`, que aceita `is_staff` ou grupos `dp_rh`, `gestor`, `admin_ti` (`app/chamados/views.py:24`; `app/chamados/views.py:35-38`; `app/chamados/views.py:659-674`); o modal aplica a mesma entrada (`app/chamados/views.py:868-874`). A resposta rápida ainda passa por `pode_agir_chamado` (`app/chamados/services/fio.py:708-718`). O departamento vem do dono do módulo ou da área da solicitação (`app/chamados/utils.py:24-44`); gestor geral age em tudo, usuário sem competências declaradas segue autorizado pelo legado, e os demais precisam da competência/perfil correspondente (`app/chamados/utils.py:47-72`). Competências vêm dos setores ativos (`app/colaboradores/mixins.py:126-151`). **Admin:** “Confira o departamento responsável indicado no chamado; a resposta deve ser registrada por quem já tem acesso a esse departamento.” A recusa específica do supervisor não está demonstrada [nao verificado]. Complemento, se houver necessidade comprovada: `RBAC-ESCRITA-POR-PAPEL` aberta (`app/docs/BACKLOG.md:1741`); comportamento AGIR-POR-DONO já registrado como feito (`app/docs/TICKETS.md:1167`). Sonda: exclusivamente código.

Nenhum dos quatro itens exige censo de ESTRUTURA pela prova disponível: 07 e 10 descrevem comportamento existente; 11 e 12 carecem da informação indicada, conforme separação da L-099 (`app/docs/LEIS.md:122`).

## 11/11 08:27 · sessao 5bbade1dc496 · - · - · origem=Ronald (tmux)

- **02 — FALTA INFORMAÇÃO.** DADO: `id=149397 | timestamp=08/11 17:30:08 | tipo=E` versus célula `id=140128 ... dna={'marcos': {'hf': '05:42', 'hi': '21:30'...}} ... protestos=['FURO_PARCIAL', 'BATIDA_ORFA_FORA_TOLERANCIA'] ... ata={'orfas': ['17:30']...}`. Isso prova divergência entre fato e previsão; faltam cadastro da janela, declaração de HE, CXR lavrado e tela que marcou “espúria”. O juiz exige esses insumos: `app/ponto/management/commands/e6_oraculo.py:182-195`; não classificaria por palpite. O motor publica CXR em `app/ponto/motor_calculo_v2.py:1655-1672`, acionado quando a entrada excede o teto em `:1709-1712`. **Complemento:** `ESPURIA-E-O-QUE-E`, aberta em `app/docs/BACKLOG.md:1805`; tela em `:1815`. `DIA-SEM-TRABALHO-PORTA` (`:1813`) só cabe se comprovado dia sem previsão; as células mostradas dizem `trabalha=True`. **Admin:** conferir horário cadastrado e trabalho realizado nas datas apontadas; decisão de pagar HE depende dessa conferência. [nao verificado quanto ao clique específico]

- **03 — FALTA INFORMAÇÃO sobre a trava atual.** DADO: `id=5311 | tipo=atestado | data=29/10/2026 | data_fim=30/10/2026 ... status=aprovada ... aprovada_em=09/11 08:50:45`; célula `id=141772 ... veredito=concorde ... ausencia_id=5311 ... status_dia=ausencia`; DiaPago `id=1989491 ... data=29/10/2026 ... minutos_abonados=660.0 ... furo_apurado=False`. Aprovação passa por `app/ponto/services/ausencia.py:897-918`; abono no fechamento lê o efeito em `app/ponto/services/fechamento.py:693-701`. Falta tela/competência/motivo de bloqueio: o recorte não traz FechamentoMensal desse colab. **Obra:** não há encaixe comprovado numa obra aberta só com essa evidência; não abrir ID. Linhagem existente de abono é `AUSENCIA-COBRE`, já feita (`app/docs/TICKETS.md:1221`), e `AUSENCIA-ATA-DISPENSA`, já feita (`:1035`). **Admin:** abrir 29/10 e conferir atestado aprovado; informar o motivo exibido na folha se ainda houver bloqueio. [nao verificado quanto à trava]

- **05 — BATIDA, chegada posterior já resolvida; causa de rede/offline [nao verificado].** DADO: `id=150717 | timestamp=09/11 16:04:05 | tipo=S ... criado_em=10/11 06:59:56`; `id=37996 ... 09/11 — marco 16:00 (saida) sem batida ... status_local=resolvido ... resolvido_em=10/11 06:59:56 ... criado_em=09/11 16:30:09`; célula `id=129423 ... veredito=concorde ... status_dia=ok`. A abertura nomeia marcos faltantes em `app/ponto/management/commands/detectar_ausencias.py:693-711`; baixa por marco aceso em `app/ponto/services/lastro.py:390-402`, pelo juiz `:153-158`. **Obra:** nenhuma correção demonstrada; lastro já pertence à obra fechada `CONTRATO-20-LASTRO-E-ORFAS`, `app/docs/BACKLOG.md:735`; eventual recorrência comprovada cabe na linhagem `LASTRO-PARTE-2`, `:1760`, sem novo ID. **Admin:** consultar 09/11 no espelho e o chamado 37996 resolvido; o registro já chegou e o dia está regular. [sonda citada]

- **06 — FALTA INFORMAÇÃO.** DADO mostra entradas isoladas em dias sem previsão: `id=145526 | timestamp=04/11 06:57:48 | tipo=E`; célula `id=132704 ... trabalha=False ... veredito=fato_sem_previsao ... ata={'orfas': ['06:57']...}`. Não mostra diária nem qual batida se pretende retirar. A porta humana existe em `app/ponto/views.py:3274-3283`, exige `regularizar_batida` e motivo; serviço em `app/ponto/services/retratar_humano.py:150-185`. **Achado de código:** `app/templates/colaboradores/partials/_calendario_grade.html:121` desenha batida órfã no ramo sem menu; “Não foi trabalho (retratar)” está no ramo `else`, das não órfãs. Isso pode explicar ausência de ação nessa tela, mas a ligação com o relato é [nao verificado]. **Complemento candidato:** `ESPURIA-TELA`, aberta em `app/docs/BACKLOG.md:1815`; porta original O6 já fechada em `:1236` e `app/docs/TICKETS.md:1256`. **Admin:** identificar data e marcação, conferir se houve trabalho e qual diária foi lançada; falta isso antes de orientar retratação. [nao verificado]

**Censo:** nenhum desses quatro itens foi classificado ESTRUTURA com a evidência disponível; portanto nenhuma lei quebrada deve ser atribuída a eles. Critério: `app/docs/LEIS.md:122`.

## 11/11 08:26 · sessao 7258489be938 · - · - · origem=Ronald (tmux)

01/04/13 — **ESTRUTURA, mesma classe BUG-PAR**, como complemento de `BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS` e sua ampliação `BUG-PAR-FROTA-1110`, dentro de `RAIZ-DA-DISPUTA` F3/F4. A obra já cita expressamente **col960, disputa #5644**, com saída e volta validadas isoladamente produzindo recusa S-S/E-E. (`app/docs/BACKLOG.md:1929`, `app/docs/BACKLOG.md:1882`, `app/docs/BACKLOG.md:1883`)

A prova da sonda é a existência das duas respostas persistidas:

> id=51522 | celula_id=128547 | disputa_id=5644 | motivo=intervalo_saida [...] resposta_colab=01:00 | respondida_em=08/11 11:04:01 | recusa_motivo=Pergunta #51522: batida vizinha #133794 (S 22/10 06:30) tem o mesmo tipo do plantio (S) - par quebrado; nao materializado (guarda de paridad [...]

> id=51523 | celula_id=128547 | disputa_id=5644 | motivo=intervalo_volta [...] resposta_colab=02:00 | respondida_em=08/11 11:04:11 | recusa_motivo=Pergunta #51523: batida vizinha #133756 (E 21/10 18:32) tem o mesmo tipo do plantio (E) - par quebrado; nao materializado (guarda de paridad [...]

Portanto, o dado comprova **resposta recebida e plantio recusado**; não comprova falha de gravação da resposta. O código grava resposta e instante, processa as perguntas individualmente e recusa o plantio diante da vizinha do mesmo tipo. (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1508`, `app/chamados/services/materializacao.py:1375`, `app/chamados/services/materializacao.py:875`)

A classe **disputa sem pergunta** é distinta: a obra descreve sete disputas abertas sem nenhuma pergunta; col960 possui perguntas identificadas e respondidas na sonda. (`app/docs/BACKLOG.md:1877`; trechos acima)

Lei quebrada: **L-099 — O ESTRUTURAL SE SEPARA DO DADO**, pela incoerência operacional já documentada no caso. O desenho existente emprega **Aggregate Root** e **Single Writer Principle**; a unidade do par está nomeada na própria obra BUG-PAR. Não há necessidade de inventar lei ou classe. (`app/docs/LEIS.md:122`, `app/docs/GLOSSARIO.md:11`, `app/docs/GLOSSARIO.md:12`, `app/docs/BACKLOG.md:1929`)

Censo dos campos envolvidos:

| Campo/estado | Quem escreve | Quem lê |
|---|---|---|
| `resposta_colab`, `respondida_em` | `app/chamados/services/disputa_emissao.py:1467`, save `:1508` | `app/chamados/services/materializacao.py:829`, seleção `:1369`; estado `app/chamados/models.py:3041` |
| Recusa por paridade | `app/chamados/services/materializacao.py:878`; encaminhamento `:1297`; persistência `:1102` | `app/chamados/services/materializacao.py:452`; apresentação `app/chamados/services/fio.py:335`; recado `app/templates/chamados/partials/modal_fio.html:230` |
| `materializada_em`, `materializada_por_id`, `via_resolucao` | Atribuição central `app/chamados/models.py:3097`; porta `:3103`; chamada do materializador `app/chamados/services/materializacao.py:1277` | Estado `app/chamados/models.py:3041`; efeito na tela `app/chamados/services/materializacao.py:434` |
| Batida de aplicação | `app/chamados/services/materializacao.py:927` chama `registrar_batida` | Verificação antes do carimbo `app/chamados/services/materializacao.py:1233` |

Texto ao admin, para os três relatos: **“Abra o chamado de col960 e confira as respostas da saída e da volta do intervalo do mesmo dia, junto com o espelho; as respostas de 01:00 e 02:00 já estão registradas.”** (`app/templates/chamados/partials/modal_fio.html:217`; trechos da sonda acima)

08 — **FALTA INFORMACAO**: o texto recebido termina durante o item 06, sem candidato, colNNN, dia ou linhas do item 08; identificação e ligação com os três casos são **[nao verificado]**. `app/docs/ESTADO.md` não está presente na cópia consultada, então o número de 06:48 também permanece **[nao verificado]**. O código define `fio_morto_de_hoje_pra_frente` para célula sem fio vivo cuja data seja `>= hoje`; isso é um balde temporal, não uma prova da identidade do item. (`app/chamados/services/vigia_fio.py:417`, `app/chamados/services/vigia_fio.py:420`)

## 11/11 08:18 · sessao a1519926172e · FIO-E0-V4-E4B · construtor · origem=Code

Cura escrita na cópia. **Os sete resultados ORM continuam [nao verificado]**; a execução fica para o Code e a certificação final para o opus.

O diff altera cinco arquivos: a cerca executa callbacks permitidos antes da foto posterior, preserva a sentinela e mantém a recusa de DML fora da foto ([backfill:315](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:315)). A classificação usa identidade do código da função; nome semelhante não concede permissão ([foto:30](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:30)).

`EventoCelula` entra por colaborador/data. `ContadorPauta` entra pelos destinatários; o alcance da alteração vem das pautas realmente mudadas, dos dois lados. Sem pauta correspondente, permanece indeterminado ([foto:268](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:268), [foto:114](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:114)). O import `MOD` foi removido ([ORM:17](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:17)).

Diff completo:

```bash
GIT_OPTIONAL_LOCKS=0 git diff -- \
  chamados/management/commands/backfill_fio_morto_e4b.py \
  chamados/services/foto_e4b.py \
  chamados/tests/test_e4b_c4_orm.py \
  chamados/tests/test_e4b_c4v7_memoria.py \
  chamados/tests/test_e4b_classe_e_memoria.py
```

O censo respondido antes da edição resultou neste tratamento:

| Callback registrado | Escritas possíveis pelas portas chamadas | Tratamento |
|---|---|---|
| `_TravaDoColaborador.__call__` | Nenhuma; no-op — `trava_do_colaborador.py:22` | Neutro; preservado durante o ato |
| `_becos_post_save_pergunta._fechar` | Perguntas/decisões/disputa, fechamento de chamado, notas, auditoria e eventos pela cascata — `signals.py:233`, `becos.py:36`, `models.py:3943` | Executado sob captura e foto |
| `_isentos_post_save_colaborador._silenciar` | Chamado e auditoria — `signals.py:274`, `arquivo.py:170` | Executado sob captura e foto |
| `_isentos_post_save_chamado._silenciar` | Mesma porta de silêncio — `signals.py:301`, `silencio_isentos.py:34` | Executado sob captura e foto |
| `retoma_regularizacoes_ao_fechar_disputa._retomar` | Regularização, itens, batidas, nota/chamado e auditoria — `signals.py:133`, `regularizacao_ext.py:121` | Recusado pelo nome |
| `_feriado_post_save._reavaliar` | Fechamento de chamados, notas/auditoria e cascata; percorre colaboradores — `signals.py:330`, `feriado_reavaliar.py:43` | Recusado pelo nome |
| `_filho_morto_acorda_o_pai._reconciliar` | Fio e também `Justificativa` — `signals.py:363`, `reconciliador.py:529` | Recusado pelo nome |

A fábrica e a reabertura podem registrar silêncio/beco pelos saves; `decisao_pergunta` emite `EventoCelula` sincronicamente; materialização pode alcançar os callbacks de fechamento. `cartorio` continua sem execução pelo backfill (`fabrica_da_celula.py:160`, `disputa_emissao.py:1849`, `models.py:3943`, `materializacao.py:1338`, `backfill_fio_morto_e4b.py:148`).

Escolhi a recusa permitida pelo item 1(b) para retomada, feriado e reconciliação. Regularizações/itens não entram na foto porque a retomada não é executada; os motivos estão no censo do código ([foto:38](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:38)).

Resultados sem banco:

| Comando | Resultado | Fonte |
|---|---|---|
| `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4b_c4v7_memoria chamados.tests.test_e4b_classe_e_memoria chamados.tests.test_e4b_foto_memoria chamados.tests.test_e4b_c4_tranca_memoria -q` | **rc 0**, 79 testes | `test_e4b_c4v7_memoria.py:55`; demais módulos indicados | [nao verificado]
| `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v7_memoria --red-base` | **rc 1**, 16 testes, 11 falhas por assertion; zero errors | `test_e4b_c4v7_memoria.py:23` | [nao verificado]
| `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos` | **rc 0**, 40 testes | `e4b_c4_verificar.py:239` | [nao verificado]
| `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm-preflight` | **rc 0**, sete enxertos/AST | `e4b_c4_verificar.py:79` |
| `GIT_OPTIONAL_LOCKS=0 git diff --check` | **rc 0** | Diff dos cinco arquivos acima |
| `PYTHONDONTWRITEBYTECODE=1 python3 -m ruff --version` | **rc 1**, `No module named ruff` | Lint completo [nao verificado] |

Os REDs principais e suas assertions que mordem:

| RED / mutante | Assertion |
|---|---|
| Desconhecido nomeado, inclusive nome forjado e segunda entrada da fila / `callback_desconhecido_livre` | [memória:172](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:172) |
| Tabela não fotografada / `tabela_fora_livre` | [memória:186](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:186) |
| Evento inserido/alterado aparece / `evento_omitido_da_foto` | [memória:352](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:352) |
| Contador medido e cercado / `contador_omitido_da_foto` | [memória:362](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:362) |
| Callback legítimo, cascata e sentinela / `callback_legitimo_recusado`, `sentinela_recusada` | [memória:207](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:207) |
| DML de callback dentro da captura / `callback_sem_captura` | [memória:222](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:222) |
| DRY elimina fila nova / `dry_deixa_fila` | [memória:260](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:260) |
| Lote recusado reverte sentinela anterior ao savepoint manual / `recusa_deixa_sentinela_C` | [memória:284](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4v7_memoria.py:284) |

**23/23 mutantes retornaram rc 1 por assertion, sem errors.** O comando de cada um é o corpo deste loop; os enxertos estão em `test_e4b_c4v7_memoria.py:406`:

```bash
for nome in \
  roteiro_so_emissao roteiro_so_dois_REDs comp_nao_corrente \
  callback_livre callback_desconhecido_livre callback_na_fila \
  sem_captura_sql sql_so_insert escrita_livre tabela_fora_livre \
  evento_omitido_da_foto contador_omitido_da_foto callback_sem_captura \
  callback_legitimo_recusado callback_chamado_recusado \
  callbacks_fora_alcance_livres sentinela_recusada dry_deixa_fila \
  recusa_deixa_sentinela_C evento_recusado_por_tabela \
  contador_recusado_por_tabela dia_cru carimbo_online
do
  PYTHONDONTWRITEBYTECODE=1 python3 \
    -m chamados.tests.test_e4b_c4v7_memoria --mutante "$nome"
done
```

Para a sessão/Code, sem execução nesta volta:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm
```

O roteiro cobre `test_e4b_c4_orm`, `test_o261e_f1_porta_encerramento`, `test_e4_passivo`, `test_e4_backfill`, `test_b2d_resposta_visivel` e `ponto.tests.test_selo_chokepoint_escrita`; depois roda `sem_cerca`, `sem_rollback`, `sem_alvo`, `foto_so_dia`, `foto_antes_C`, `tranca_sem_mapa` e `tranca_avulsa` (`e4b_c4_verificar.py:117`). Acrescentei assertions ORM para desconhecido, tabela externa e EventoCelula no DIFF ([ORM:183](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:183), [ORM:198](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:198), [ORM:218](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:218)).

## 11/11 08:02 · sessao bc65f5408f95 · TRAVA-SELO · verificador · origem=Code

**1. NÃO PROCEDE fechar a classe; PROCEDEM curas pontuais.** Exigir conteúdo elimina o falso módulo `testar_pwa`, mas a cobertura continua comparando apenas módulos e seus ancestrais. Selecionar todas as classes de um módulo não o cobre para essa comparação. (`bin/suite.sh:157`, `bin/suite.sh:163`; `app/ponto/management/commands/testar_pwa.py:31`)

Contraexemplo estático nesta cópia: substituir `comunicados` pelas duas seleções abaixo, mantendo as demais LABELS. A guarda considera `comunicados.tests_casca_a13` descoberto e não coberto, retornando “recorte”. (`bin/regua.sh:23`; `bin/suite.sh:163`, `bin/suite.sh:164`)

```text
--only 'api chamados colaboradores comunicados.tests comunicados.tests_casca_a13.CascaComunicadosA13Test core escala ferias folha holerite inteligencia pautas ponto relatorios'
```

O segundo módulo contém uma única classe `TestCase`, com quatro testes; selecionar essa classe preserva estaticamente os casos do módulo. Igualdade efetiva dos IDs produzidos pelo Django da imagem: **[nao verificado]**. (`app/comunicados/tests_casca_a13.py:11`, `app/comunicados/tests_casca_a13.py:16`, `app/comunicados/tests_casca_a13.py:33`, `app/comunicados/tests_casca_a13.py:43`, `app/comunicados/tests_casca_a13.py:51`, `app/comunicados/tests_casca_a13.py:58`)

O tratamento **local** do grep está correto:

| Retorno | Tratamento |
|---|---|
| 0 | Processa os nomes encontrados. (`bin/suite.sh:159`, `bin/suite.sh:160`) |
| 1, inclusive app sem módulos correspondentes | Ignora a lista vazia e continua nos demais apps. (`bin/suite.sh:159`, `bin/suite.sh:161`) |
| ≥2, inclusive arquivo ilegível | Retorna inteira antes de consumir eventual lista parcial. (`bin/suite.sh:159`) |

Entretanto, isso não garante consultar erros **em todos os apps**: o primeiro módulo não coberto encerra a função antes das buscas seguintes; diretório de app ausente é pulado. Isso limita a promessa universal de falha conservadora, embora não demonstre sozinho ampliação da seleção. (`bin/suite.sh:155`, `bin/suite.sh:156`, `bin/suite.sh:164`)

Há possibilidade de recorte legítimo recusado pelo universo reduzido: cobrir todos os módulos contados e deixar de fora um módulo que apenas herda testes produz “inteira”. É consequência conservadora assumida pelo próprio comentário; ocorrência concreta desse formato nesta cópia: **[nao verificado]**. (`bin/suite.sh:148`, `bin/suite.sh:167`)

**2. NÃO PROCEDE a equivalência entre juiz e porta.** `echo` e motivo duplo na mesma linha foram curados, mas a duplicidade em continuação permanece: a extração conserva as duas linhas, enquanto o primeiro grep entrega ao segundo apenas a linha que contém o comando. O parser recebe os dois motivos e conserva o último. (`bin/tests/test_suite_sh.sh:26`, `bin/tests/test_suite_sh.sh:27`, `bin/tests/test_suite_sh.sh:39`, `bin/tests/test_suite_sh.sh:40`; `bin/suite.sh:60`)

Assim, o juiz aprova este bloco, cuja porta recusa pela forma do último motivo. (`bin/tests/test_suite_sh.sh:39`, `bin/tests/test_suite_sh.sh:40`; `bin/suite.sh:150`, `bin/suite.sh:169`)

```bash
bash ~/saas-hasner/bin/suite.sh --motivo "pre-opus X" \ [nao verificado]
  --motivo "quero ver"
```

Também aprova `"pre-opus X"'!'`: falta fronteira após as aspas no regex; o shell entrega `pre-opus X!`, recusado pela guarda. No sentido contrário, motivo com aspas simples ou colocado depois de `--parallel 2` é aceito pelo parser e rejeitado pelo juiz. (`bin/tests/test_suite_sh.sh:39`; `bin/suite.sh:56`, `bin/suite.sh:60`, `bin/suite.sh:150`)

**Bloco real:** a linha atual passa pelo juiz graças à exceção explícita para `<OBRA>`. Literalmente, esse marcador não passa pela guarda; substituído por `TRAVA-SELO`, satisfaz sua forma. Isso é uma convenção documental deliberada, distinta dos escapes acima. (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:35`, `bin/tests/test_suite_sh.sh:39`; `bin/suite.sh:150`)

**3. PROCEDEM os discriminantes dos quatro mutantes, por leitura estática; execução integral dos “26 mortos”: [nao verificado].**

| Mutante | Razão correta da morte |
|---|---|
| `prefixo_sem_ponto` | Passa a considerar `test_x_extra` coberto por `test_x`, recusando o recorte positivo que omite o irmão. (`bin/tests/test_suite_motivo.sh:113`, `bin/tests/test_suite_motivo.sh:194`) |
| `busca_falha_vira_recorte` | Ignora rc 2 e encontra módulos não cobertos; libera a chamada que o caso ilegível exige recusar. (`bin/tests/test_suite_motivo.sh:118`, `bin/tests/test_suite_motivo.sh:120`, `bin/tests/test_suite_motivo.sh:196`; `bin/suite.sh:164`) |
| `nome_sem_conteudo` | Inclui `testar_algo.py`, que só define `handle`, e libera sem motivo a enumeração completa dos módulos de teste. (`bin/tests/test_suite_motivo.sh:24`, `bin/tests/test_suite_motivo.sh:111`, `bin/tests/test_suite_motivo.sh:197`) |
| `seco_argv_sem_extra` | Retira `--failfast` da linha argv, quebrando a comparação exata. (`bin/tests/test_suite_motivo.sh:133`, `bin/tests/test_suite_motivo.sh:137`, `bin/tests/test_suite_motivo.sh:195`) |

**NÃO SEI se a execução relatada ocorreu como usuário sem privilégios. [nao verificado]** A régua herda o usuário ao chamar `bash`; o caso chmod verifica legibilidade e avisa quando não pode exercitar a asserção. Nesse caminho, o mutante de busca permanece vivo e o harness acusa `MORDE`, em vez de aprovar silenciosamente os 26. (`bin/regua.sh:140`; `bin/tests/test_suite_motivo.sh:119`, `bin/tests/test_suite_motivo.sh:179`, `bin/tests/test_suite_motivo.sh:196`)

Sobrevivente plausível: trocar `[ $rc -le 1 ] || return 0` por `[ $rc -eq 0 ] || return 0`. A fixture fornece testes nos dois apps e não exercita rc 1; essa mutação preservaria os casos existentes, mas recusaria prematuramente um recorte diante de app sem módulos. Sobrevivência deduzida estaticamente; execução: **[nao verificado]**. (`bin/suite.sh:159`; `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:26`)

**4. PROCEDE a compatibilidade dos leitores examinados, com uma regressão de transporte em `lst`.**

| Campo | QUEM ESCREVE → QUEM LÊ |
|---|---|
| `lst` | `grep -l` separado por LF → `read -r` separado por LF; não sobrou leitor esperando NUL. (`bin/suite.sh:157`, `bin/suite.sh:160`, `bin/suite.sh:165`) |
| `_porq` / `VOLTA_PORQUE` | Wrapper fornece caminho; suite grava stderr do helper; suite lê diagnósticos e wrapper conta ` <- `. (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:34`; `bin/suite.sh:87`, `bin/suite.sh:88`, `bin/suite.sh:89`; `bin/vizinhos_da_volta.py:130`) |
| Limpeza `_porq` | Temporário removido em sucesso e falha; caminho fornecido preservado. (`bin/suite.sh:88`, `bin/suite.sh:90`) |
| SECO argv | Escritor usa as expansões do executor; novo leitor exige a linha exata. (`bin/suite.sh:184`, `bin/suite.sh:228`, `bin/suite.sh:229`; `bin/tests/test_suite_motivo.sh:137`) |

A regressão: LF no caminho externo de `--dir` é aceito, mas divide cada nome em linhas e impede retirar corretamente o prefixo da árvore. Pela transformação implementada, até todas as LABELS podem parecer recorte. Existência/execução de uma cópia com esse caminho: **[nao verificado]**. (`bin/suite.sh:54`, `bin/suite.sh:157`, `bin/suite.sh:160`, `bin/suite.sh:162`, `bin/suite.sh:164`)

**5. Custo: PROCEDE o descarte como impedimento; enumeração classe/método: NÃO PROCEDE.**

A busca tem atalho para seleção vazia, mas continua antecedendo a decisão sobre motivo e SECO. Os 0,62 s são a medição fornecida na triagem, sem repetição independente **[nao verificado]**; o código não sustenta transformá-los em limite geral. Isso, sozinho, não demonstra defeito de segurança da guarda. (`bin/suite.sh:154`, `bin/suite.sh:169`, `bin/suite.sh:179`)

Reduzir o universo por conteúdo não resolve enumeração por classe/método: uma seleção descendente do módulo continua sendo julgada “não coberta”, que é justamente o retorno permissivo. O exemplo de `CascaComunicadosA13Test` materializa essa direção da comparação. (`bin/suite.sh:163`, `bin/suite.sh:164`; `app/comunicados/tests_casca_a13.py:16`)

**Fecho: NÃO LIBERA**, pela comparação de cobertura ainda aberta e pelas divergências demonstráveis entre juiz documental e parser, independentemente da pergunta de desenho separada. (`bin/suite.sh:163`, `bin/suite.sh:164`; `bin/tests/test_suite_sh.sh:39`, `bin/tests/test_suite_sh.sh:40`; `bin/suite.sh:60`)

## 11/11 08:01 · sessao 41e0f2988e8c · - · - · origem=Ronald (tmux)

**NÃO PROCEDE fechar a classe nem descartar enumeração por classe.** Contraexemplo estático compacto:

```text
--only 'api chamados colaboradores comunicados.tests comunicados.tests_casca_a13.CascaComunicadosA13Test core escala ferias folha holerite inteligencia pautas ponto relatorios'
```

A seleção preserva as LABELS e substitui o app `comunicados` pelos seus dois módulos, representando o segundo pela sua única classe de teste; a guarda exige módulo igual à seleção ou descendente dela, portanto não reconhece `comunicados.tests_casca_a13` como coberto. Emulação em memória da busca e comparação confirmou esse primeiro módulo descoberto não coberto. (`bin/regua.sh:23`; `bin/suite.sh:157`; `bin/suite.sh:163`; `bin/suite.sh:164`; `app/comunicados/tests.py:10`; `app/comunicados/tests_casca_a13.py:16`)

O segundo módulo contém uma única `TestCase`, com quatro métodos de teste; selecioná-la preserva estaticamente seus casos. Igualdade efetiva no Django instalado: **[nao verificado]**. (`app/comunicados/tests_casca_a13.py:11`; `app/comunicados/tests_casca_a13.py:16`; `app/comunicados/tests_casca_a13.py:33`; `app/comunicados/tests_casca_a13.py:43`; `app/comunicados/tests_casca_a13.py:51`; `app/comunicados/tests_casca_a13.py:58`)

**PROCEDE a leitura local do rc:** 0 processa a lista; 1 permite lista vazia, cujas linhas vazias são ignoradas; ≥2 retorna inteira. Porém, “qualquer falha da busca resulta inteira” não vale em todo caminho: um módulo não coberto provoca retorno antecipado antes das buscas nos apps seguintes; app ausente também é pulado. Isso não demonstra ampliação da seleção que já omitia um módulo válido, mas limita a promessa universal de tratamento das falhas. (`bin/suite.sh:155`; `bin/suite.sh:156`; `bin/suite.sh:159`; `bin/suite.sh:161`; `bin/suite.sh:164`)

Não encontrei contraexemplo concreto adicional de arquivo atual contendo apenas falso `def test`, módulo só herdando testes, symlink ou pasta descoberta sem `__init__.py`; execução integral da descoberta Django: **[nao verificado]**. [nao verificado]

## 11/11 08:00 · sessao 9aba64bbf79e · - · - · origem=Ronald (tmux)

**3. PROCEDEM os discriminantes dos quatro mutantes, por análise estática; execução integral dos “26 mortos” [nao verificado].**

- `prefixo_sem_ponto`: a fixture contém `test_x` e `test_x_extra`; a seleção que omite o segundo deve chegar à trava. O mutante o considera coberto pelo primeiro e recusa esse recorte. (`bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:113`, `bin/tests/test_suite_motivo.sh:194`)
- `nome_sem_conteudo`: incluir `testar_algo.py`, que só define `handle`, faz a enumeração completa dos quatro módulos parecer parcial e ser liberada sem motivo. (`bin/tests/test_suite_motivo.sh:24`, `bin/tests/test_suite_motivo.sh:111`, `bin/tests/test_suite_motivo.sh:197`)
- `seco_argv_sem_extra`: retirar EXTRA da linha SECO remove `--failfast`, contrariando a comparação exata. (`bin/tests/test_suite_motivo.sh:133`, `bin/tests/test_suite_motivo.sh:137`, `bin/tests/test_suite_motivo.sh:195`)
- `busca_falha_vira_recorte`: com `test_w.py` ilegível, o vivo retorna inteira pelo rc 2; o mutante continua e encontra módulos não cobertos por `appA.tests.test_x`, retornando recorte. (`bin/tests/test_suite_motivo.sh:118`, `bin/tests/test_suite_motivo.sh:120`, `bin/tests/test_suite_motivo.sh:196`; `bin/suite.sh:159`, `bin/suite.sh:164`)

**chmod 000 é condicionalmente honesto.** O teste pergunta `-r` e avisa quando o arquivo continua legível; nessa situação não executa a asserção. A régua chama o selo por `bash` sem trocar o usuário. A identidade da execução relatada em produção é **[nao verificado]**. (`bin/tests/test_suite_motivo.sh:119`; `bin/regua.sh:138`, `bin/regua.sh:140`)

Se rodar com privilégios que permitam leitura, o mutante de falha da busca permanece vivo e o harness acusa `MORDE`; não há aprovação silenciosa dos 26 nesse caminho. (`bin/tests/test_suite_motivo.sh:119`, `bin/tests/test_suite_motivo.sh:179`, `bin/tests/test_suite_motivo.sh:196`, `bin/tests/test_suite_motivo.sh:204`)

**Sobrevivente plausível:** trocar `[ $rc -le 1 ] || return 0` por `[ $rc -eq 0 ] || return 0`. A fixture sempre fornece módulos com teste nos dois apps, portanto não exercita rc 1; a mutação preserva rc 0/2 e passa pelos mesmos casos, mas transforma app sem módulos em recusa imediata mesmo diante de recorte legítimo em outro app. Isso é dedução estática, não execução do mutante. (`bin/suite.sh:159`; `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:26`, `bin/tests/test_suite_motivo.sh:118`)

**4. PROCEDE a migração dos leitores examinados, com um problema novo no transporte de `lst`.**

| Campo | Escritor → leitor |
|---|---|
| `lst` | `grep -l`, separado por LF → `while read -r`, também por LF; não ficou `read -d ''` antigo. (`bin/suite.sh:157`, `bin/suite.sh:160`, `bin/suite.sh:165`) |
| `_porq` / `VOLTA_PORQUE` | Wrapper fornece caminho; suite grava stderr do helper; helper mantém `<label> <- <motivos>`, além de diagnósticos; suite lê `cat`/`AST-FALHOU`, wrapper conta ` <- `. (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:34`; `bin/suite.sh:86`, `bin/suite.sh:87`, `bin/suite.sh:88`, `bin/suite.sh:89`; `bin/vizinhos_da_volta.py:120`, `bin/vizinhos_da_volta.py:130`) |
| Limpeza `_porq` | Temporário removido em sucesso e falha; caminho fornecido preservado em ambos. (`bin/suite.sh:88`, `bin/suite.sh:90`) |
| SECO argv | `%q` com as mesmas expansões do executor → comparação exata nova no selo. (`bin/suite.sh:184`, `bin/suite.sh:228`, `bin/suite.sh:229`; `bin/tests/test_suite_motivo.sh:137`) |
| `trava.chamada` | `%q` por argumento → `_args` e greps; labels aceitas são ASCII pontuado, portanto extração das labels continua compatível. (`bin/tests/test_suite_motivo.sh:28`, `bin/tests/test_suite_motivo.sh:33`, `bin/tests/test_suite_motivo.sh:104`, `bin/tests/test_suite_motivo.sh:108`; `bin/suite.sh:104`) |

**Risco de `lst`:** LF no caminho externo de `--dir` é aceito pelo parser, mas divide cada nome emitido pelo grep em duas linhas; o leitor não consegue retirar o prefixo completo e encontra módulo “não coberto”. Assim até todas as LABELS podem ser julgadas recorte. O bind usa o caminho canonicalizado e citado. Diretório concreto com LF não foi criado: execução desse cenário **[nao verificado]**. (`bin/suite.sh:54`, `bin/suite.sh:157`, `bin/suite.sh:160`, `bin/suite.sh:162`, `bin/suite.sh:164`, `bin/suite.sh:200`, `bin/suite.sh:226`)

## 11/11 07:59 · sessao 4d1543765765 · - · - · origem=Ronald (tmux)

**2. NÃO PROCEDE o fechamento da classe documental.** Os casos antigos `echo` e dois motivos na mesma linha são barrados, mas o juiz continua divergindo da porta. (`bin/tests/test_suite_sh.sh:39`, `bin/tests/test_suite_sh.sh:40`)

Contraexemplos confirmados executando somente as funções/parser em memória, sem escrever arquivos:

```bash
bash ~/saas-hasner/bin/suite.sh --motivo "pre-opus X"'!' [nao verificado]
```

O juiz aprova: seu regex termina nas aspas, sem exigir fronteira do argumento. O shell concatena o sufixo, entregando `pre-opus X!`; a porta recusa com rc 2. (`bin/tests/test_suite_sh.sh:39`; `bin/suite.sh:60`, `bin/suite.sh:150`, `bin/suite.sh:169`, `bin/suite.sh:176`)

```bash
bash ~/saas-hasner/bin/suite.sh --motivo "pre-opus X" \ [nao verificado]
  --motivo "quero ver"
```

O juiz também aprova: a segunda linha não chega ao filtro de duplicidade, pois não casa com o primeiro grep. A extração inclui ambas por causa da continuação, e o parser conserva o último motivo, recusado pela guarda. (`bin/tests/test_suite_sh.sh:26`, `bin/tests/test_suite_sh.sh:27`, `bin/tests/test_suite_sh.sh:39`, `bin/tests/test_suite_sh.sh:40`; `bin/suite.sh:60`, `bin/suite.sh:150`, `bin/suite.sh:169`)

Há falsos negativos: aspas simples no motivo; motivo depois de `--parallel 2`; ou um comentário final contendo outro `--motivo`. Os dois primeiros não casam com a forma rígida; o terceiro é contado como duplicidade textual. O parser aceita os respectivos argumentos válidos. (`bin/tests/test_suite_sh.sh:39`, `bin/tests/test_suite_sh.sh:40`; `bin/suite.sh:56`, `bin/suite.sh:60`, `bin/suite.sh:150`)

**Bloco real:** a linha atual é aprovada graças à exceção explícita para `<OBRA>`. Copiada literalmente, porém, a guarda recusa esse marcador com rc 2; substituído por `TRAVA-SELO`, o motivo satisfaz a guarda. Essa exceção já é assumida no comentário do juiz, portanto convém distinguir o exemplo documental parametrizado da alegação de equivalência literal. (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:35`, `bin/tests/test_suite_sh.sh:39`; `bin/suite.sh:150`, `bin/suite.sh:169`, `bin/suite.sh:176`)

## 11/11 07:59 · sessao 4b295e1a5bf7 · FIO-E0-V4-E3 · construtor · origem=Code

Diff aplicado nos três arquivos autorizados: [nao verificado]

- C2b exige `concorde`, mantendo igualdade antes/depois e exclusão dos acusadores. [teste:135](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:135) [nao verificado]
- C2c cobre `aguardando_decisao`, marcador e rejulgamento sem força após batida distante. [teste:165](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:165) [nao verificado]
- Executor recebeu BASE `52a0022…`, novo mutante e contagem ORM ajustada para 13. [executor:21](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:21)
- Relato corrigido para `sem_trabalho`, exceto cobertura integral. [relato:22](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_x1.txt:22)

Resultados sem banco, com comandos individuais registrados no [relato:161](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_x1.txt:161):

- Memória: **RC=0**, 19 testes. [nao verificado]
- Executor green: **RC=0**, 19 testes. [nao verificado]
- Quatro mutantes de memória: **RC=1 cada**, um `AssertionError`, zero `ERROR`.

Por leitura, o novo mutante encontra a assertion da impressão no C2c; sintaxe e alvo foram conferidos. Morte efetiva ORM e ruff: **[nao verificado]**. [análise:156](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_x1.txt:156)

## 11/11 07:54 · sessao 55a5a29c6fd1 · FIO-E0-V4-E4B · construtor · origem=Code

[Diff completo: 7 arquivos](FIO-E0-V4-E4B-c4v7.patch). A validação ORM e a certificação pelo Code/opus permanecem [nao verificado].

| Item | Alteração | Assertion RED na base `dee18744a` |
|---|---|---|
| 1c | Apply exige competência corrente; recusa informa pedida/corrente. [origem](chamados/management/commands/backfill_fio_morto_e4b.py:221) | [teste:102](chamados/tests/test_e4b_c4v7_memoria.py:102) |
| 1e | Dia da pergunta passa pelo juiz; conversa fica indeterminada. [origem](chamados/services/foto_e4b.py:176) | [teste:187](chamados/tests/test_e4b_c4v7_memoria.py:187) |
| 1b | Foto online exclui somente `previsto_em`. [origem](chamados/services/foto_e4b.py:244) | [teste:198](chamados/tests/test_e4b_c4v7_memoria.py:198) |
| 1a | Callback novo recusa/desfaz; callback recusado sai da fila. [origem](chamados/management/commands/backfill_fio_morto_e4b.py:327) | [teste:119](chamados/tests/test_e4b_c4v7_memoria.py:119), [fila:123](chamados/tests/test_e4b_c4v7_memoria.py:123) |
| 1d | DML fora das tabelas fotografadas recusa/desfaz. [origem](chamados/management/commands/backfill_fio_morto_e4b.py:323) | [teste:132](chamados/tests/test_e4b_c4v7_memoria.py:132) |
| 8 | Base troca todos os arquivos produtivos do diff; executa todos os REDs de memória. [origem](chamados/tests/e4b_c4_verificar.py:40) | [teste:75](chamados/tests/test_e4b_c4v7_memoria.py:75) |
| 6 | Captura de `ns` vinculada ao argumento da lambda. | [correção:471](chamados/tests/test_e4b_classe_e_memoria.py:471) |

RELATO do relógio: `fechamento_lido` não oferece parâmetro de relógio; o ramo de leitura atribui `timezone.now()` a `previsto_em`. Por isso, a exclusão ficou somente na foto online; os demais carimbos continuam comparados. [assinatura](ponto/services/fechamento.py:1735), [escritor](ponto/services/fechamento.py:1086), [ramo de leitura](ponto/services/fechamento.py:1135).

Comandos executados sem banco:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4b_classe_e_memoria chamados.tests.test_e4b_c4_tranca_memoria chamados.tests.test_e4b_foto_memoria chamados.tests.test_e4b_c4_orm_protocolo chamados.tests.test_e4b_c4_base chamados.tests.test_e4b_c4v7_memoria -q
# rc=0: 79 testes [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos
# rc=0: 40 testes [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm-preflight
# rc=0: sete enxertos conferidos; não executa ORM

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v7_memoria --red-base
# rc=1: seis REDs, oito falhas por assertion, zero erros

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --red-base
# rc=1: 28 testes, cinco falhas e 16 erros de APIs ausentes em b20 [nao verificado]
```

Os **16 erros da base b20 não foram contabilizados como mordidas**. A base completa inclui a ausência de `foto_e4b.py`, sem conservar corpo HEAD. [representação da ausência](chamados/tests/e4b_c4_verificar.py:51).

Cada mutante abaixo retornou **rc=1 por `AssertionError`, sem erro de execução**; os alvos estão no [catálogo:220](chamados/tests/test_e4b_c4v7_memoria.py:220):

```bash
for mutante_e4b in \
  roteiro_so_emissao roteiro_so_dois_REDs comp_nao_corrente \
  callback_livre callback_na_fila sem_captura_sql sql_so_insert \
  escrita_livre dia_cru carimbo_online
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4v7_memoria --mutante "$mutante_e4b"
done
```

Para a sessão, o roteiro ORM inclui `tranca_avulsa` e os novos testes de dois `freeze_time`, callback produtivo e EventoCelula pela porta. [QUERY](chamados/tests/e4b_c4_verificar.py:137), [relógio](chamados/tests/test_e4b_c4_orm.py:127), [callback](chamados/tests/test_e4b_c4_orm.py:149), [EventoCelula](chamados/tests/test_e4b_c4_orm.py:170):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm
```

Ruff foi tentado com `python3 -m ruff check --no-cache`: **rc=1, módulo ausente**. Execução do lint, ORM, suíte inteira e amostra factual das 131: [nao verificado]. [nao verificado]

## 11/11 07:51 · sessao 926be1d0ec43 · TRAVA-SELO · verificador · origem=Code

**NÃO LIBERA.** As correções pontuais funcionam, mas a descoberta ainda confunde arquivos candidatos com os testes efetivos, e o selo documental continua aceitando comandos que a porta recusaria. Isso independe da pergunta de desenho do item 6. (`bin/suite.sh:155`, `bin/suite.sh:157`; `bin/tests/test_suite_sh.sh:37`; `bin/suite.sh:60`)

**1. NÃO PROCEDE o fechamento da classe; PROCEDEM curas pontuais.**

Há um contraexemplo compacto nesta cópia:

```text
--only 'api chamados colaboradores comunicados core escala ferias folha holerite inteligencia pautas ponto.tests relatorios'
```

São as LABELS com apenas `ponto` substituído por `ponto.tests`. Executando somente `_suite_inteira` em memória, obtive retorno **1**, portanto “não inteira”. A diferença é `ponto.management.commands.testar_pwa`: o `find` exige sua cobertura porque seu nome começa com `test`, embora ele defina `Command(BaseCommand)` e execute métodos `t01_...` por `handle`. Pela análise estática, deixá-lo fora não retira casos de teste; igualdade da suíte no Django instalado é **[nao verificado]**. (`bin/regua.sh:23`; `bin/suite.sh:155`, `bin/suite.sh:157`; `app/ponto/management/commands/testar_pwa.py:31`, `app/ponto/management/commands/testar_pwa.py:70`, `app/ponto/management/commands/testar_pwa.py:100`)

Logo, “todos os módulos encontrados” e “todos menos um” não discriminam necessariamente inteira e recorte: o módulo omitido pode não fornecer teste algum. Enumerar classes ou métodos também fica fora da comparação implementada, que exige selecionar o módulo ou um ancestral dele; uma enumeração integral concreta por classes nesta cópia é **[nao verificado]**. (`bin/suite.sh:156`; `app/ponto/management/commands/testar_pwa.py:31`)

Quanto às formas perguntadas:

| Forma | Resultado |
|---|---|
| Pasta com nome fora de `test*` | **Não é escape isoladamente:** o padrão restringe arquivos; o `find` atravessa as demais pastas. (`bin/suite.sh:158`, `bin/suite.sh:159`) |
| Pasta sem `__init__.py` | A guarda não verifica importabilidade nem `__init__`; pode exigir arquivos que a descoberta efetiva ignore. Efeito no Django instalado: **[nao verificado]**. (`bin/suite.sh:158`; `app/core/runner_quarentena.py:76`) |
| Pasta prunada | Os quatro nomes são excluídos pelo script, sem regra equivalente no override do runner. Se houver testes efetivos ali, um recorte que cubra apenas o restante será recusado como inteira. Ocorrência atual: **[nao verificado]**. (`bin/suite.sh:158`; `app/core/runner_quarentena.py:75`) |
| Symlink | A busca não usa `-L`, e exige `-type f`; não representa integralmente arquivos e diretórios alcançáveis por links. Divergência concreta da suíte desta cópia: **[nao verificado]**. (`bin/suite.sh:158`, `bin/suite.sh:159`) |
| Espaço/acento no caminho externo | O caminho está citado na busca. Nomes selecionáveis de app/módulo, porém, continuam limitados à forma ASCII. (`bin/suite.sh:104`, `bin/suite.sh:149`, `bin/suite.sh:158`) |
| `--dir` buscando outra árvore | Não encontrei troca ordinária: a busca usa `<DIR>/app`, e o bind principal usa esse caminho canonicalizado. As sobreposições adicionais são staticfiles, caches, logs e media. (`bin/suite.sh:149`, `bin/suite.sh:194`, `bin/suite.sh:220`; `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:80`) |

Há custo adicional até proporcional a **módulos × palavras selecionadas**, inclusive com motivo válido e em `--seco`: a classificação vem antes dessas decisões. Duração em produção: **[nao verificado]**. (`bin/suite.sh:154`, `bin/suite.sh:156`, `bin/suite.sh:163`, `bin/suite.sh:173`)

As demais curas:

- **PROCEDE:** `_valor` agora recusa um próximo flag como valor; EXTRA continua preservado como array; SECO ganhou representação `%q`. (`bin/suite.sh:51`, `bin/suite.sh:178`, `bin/suite.sh:223`)
- **PROCEDE:** `RecursionError` no parse cai no regex, e `AST-FALHOU` passa ao stderr da chamada direta. (`bin/vizinhos_da_volta.py:68`, `bin/vizinhos_da_volta.py:120`, `bin/vizinhos_da_volta.py:123`; `bin/suite.sh:89`)
- **NÃO PROCEDE fechar a classe documental:** os dois exemplos citados foram barrados, mas o juiz ainda aceita `bash bin/suite.sh --motivo "pre-opus X" --motivo "quero ver"`. Conferi o juiz em memória; o parser usa o último motivo, que a guarda recusaria. Também passa uma linha `echo` contendo o texto do comando. (`bin/tests/test_suite_sh.sh:37`; `bin/suite.sh:60`, `bin/suite.sh:163`)

**2. PROCEDE a ordem antes de env-file/Docker; NÃO PROCEDE tratamento confiável da falha da busca.**

A classificação ocorre antes de `teste_envfile`, da montagem e do executor Docker. O `source recursos.sh` e, com `--volta`, o helper e o arquivo de diagnóstico ocorrem antes dela. (`bin/suite.sh:39`, `bin/suite.sh:86`, `bin/suite.sh:163`, `bin/suite.sh:184`, `bin/suite.sh:197`, `bin/suite.sh:216`)

O status de `find` é perdido na substituição de processo. Diretório ausente é ignorado por `continue`; busca sem resultados termina como **inteira**, pedindo motivo — não vira automaticamente “não inteira”. (`bin/suite.sh:153`, `bin/suite.sh:158`, `bin/suite.sh:161`)

O retorno “não inteira” acontece quando algum resultado emitido não está coberto. Isso também pode ocorrer com uma busca incompleta ou com erro, pois não existe validação posterior de sua conclusão. Uma falha real por permissão nesta cópia é **[nao verificado]**. (`bin/suite.sh:157`, `bin/suite.sh:158`)

**3. PROCEDE a razão dos três novos; NÃO PROCEDE suficiência da cobertura.**

| Mutante | Discriminante correto |
|---|---|
| `executor_extra_junta` | `%q` distingue `--tag x --failfast` de `--tag\ x\ --failfast`; o grep exige argumentos separados. (`bin/tests/test_suite_motivo.sh:23`, `bin/tests/test_suite_motivo.sh:103`, `bin/tests/test_suite_motivo.sh:177`) |
| `valor_aceita_flag` | Os quatro casos `flag --seco` exigem rc 2 **e** “precisa de valor”; eventual recusa posterior não satisfaz o diagnóstico. (`bin/tests/test_suite_motivo.sh:111`, `bin/tests/test_suite_motivo.sh:112`, `bin/tests/test_suite_motivo.sh:178`) |
| `descoberta_por_nome_de_app` | A enumeração completa dos três módulos deve ser inteira; o mutante não reconhece esses nomes e a libera. (`bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:179`) |

**Sobrevivente:** trocar `"$s".*` por `"$s"*` mantém o resultado das seleções da fixture, que não contém irmãos como `test_x` e `test_x_extra`. A mutação passaria a cobrir indevidamente o segundo pelo primeiro, podendo recusar um recorte legítimo. A comparação foi conferida em memória. (`bin/suite.sh:156`; `bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:109`)

Remover todo o prune **morre**: inclui `migrations/test_0001.py`, ausente da enumeração esperada como inteira. Trocar o `return 1` por `continue` também **morre**: os recortes passam a ser classificados como inteiros e quebram os positivos. (`bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:109`; `bin/suite.sh:157`, `bin/suite.sh:161`)

Execução integral do selo e confirmação dos “22 mortos”: **[nao verificado]**; ele cria, escreve e remove arquivos. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:31`)

**4. PROCEDE a compatibilidade dos leitores examinados.**

| Testemunho | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `_porq` / `VOLTA_PORQUE` | Wrapper fornece o caminho; `suite.sh` redireciona para ele o stderr produzido pelo helper. (`bin/volta_intermediaria.sh:32`; `bin/suite.sh:86`, `bin/suite.sh:87`; `bin/vizinhos_da_volta.py:130`) | `suite.sh` reproduz falhas/`AST-FALHOU`; wrapper conta linhas com ` <- `. Esse formato permanece. (`bin/suite.sh:88`, `bin/suite.sh:89`; `bin/volta_intermediaria.sh:34`) |
| `trava.chamada` | Trava falsa grava `%q` por argumento. (`bin/tests/test_suite_motivo.sh:23`) | `_args` extrai labels; greps verificam sufixos. As labels admitidas não precisam de escapes, portanto esses leitores continuam compatíveis. (`bin/tests/test_suite_motivo.sh:28`, `bin/tests/test_suite_motivo.sh:99`, `bin/tests/test_suite_motivo.sh:103`; `bin/suite.sh:104`) |
| `SECO argv` | `suite.sh` imprime as mesmas expansões de seleção, paralelismo e EXTRA do executor. (`bin/suite.sh:178`, `bin/suite.sh:222`, `bin/suite.sh:223`) | O selo ainda verifica apenas o resumo anterior; não protege a nova linha `argv`. (`bin/tests/test_suite_motivo.sh:121`, `bin/tests/test_suite_motivo.sh:122`) |

O wrapper passa a receber `AST-FALHOU` também em seu log principal, pois captura stderr. A régua executa os selos e lê seu status, sem interpretar esses três formatos. (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:42`; `bin/regua.sh:138`, `bin/regua.sh:140`)

Há um resíduo novo: se o helper falhar, o ramo sai antes de remover o temporário automático `_porq`. (`bin/suite.sh:86`, `bin/suite.sh:88`, `bin/suite.sh:90`)

**5. PROCEDEM os descartes sintáticos; NÃO PROCEDEM as generalizações de fechamento.**

- **Último `--only` vence:** descarte correto como fuga isolada; seleção julgada e executada usam o mesmo valor final. (`bin/suite.sh:55`, `bin/suite.sh:150`, `bin/suite.sh:222`)
- **Assimetria de `--tag`:** existe, mas não demonstra ampliação; o valor separado é consumido e EXTRA chega citado. Efeito no runner instalado: **[nao verificado]**. (`bin/suite.sh:121`, `bin/suite.sh:124`, `bin/suite.sh:125`, `bin/suite.sh:223`)
- **Mapa de campos:** as novas representações têm leitores compatíveis nos caminhos examinados. (`bin/suite.sh:87`, `bin/suite.sh:178`; `bin/tests/test_suite_motivo.sh:23`, `bin/tests/test_suite_motivo.sh:28`)
- **“Descoberta curou a enumeração” e frase documental agora verdadeira:** não fecham a classe, pelo contraexemplo de `testar_pwa` e pela comparação somente entre prefixos de módulos. (`bin/suite.sh:156`; `app/ponto/management/commands/testar_pwa.py:31`; `CLAUDE.md:101`)
- **Item 6:** não reaberto. Os defeitos acima podem ser julgados sem decidir a unidade de régua/pre-push; o delta mantém a guarda em `suite.sh`. (`bin/suite.sh:163`; `bin/regua.sh:165`, `bin/regua.sh:168`)

**Fecho: NÃO LIBERA**, pela classe de descoberta ainda aberta e pelo selo documental ainda permissivo, independentemente do item 6. (`bin/suite.sh:155`, `bin/suite.sh:157`; `bin/tests/test_suite_sh.sh:37`)

## 11/11 07:50 · sessao 1971b03a273e · - · - · origem=Ronald (tmux)

A cura por descoberta **não fecha a classe**, pois compara nomes de arquivos, sem conferir se esses arquivos contêm casos da suíte. (`bin/suite.sh:155`, `bin/suite.sh:156`, `bin/suite.sh:157`)

Contraexemplo compacto nesta cópia:

```text
--only 'api chamados colaboradores comunicados core escala ferias folha holerite inteligencia pautas ponto.tests relatorios'
```

São as LABELS, trocando apenas `ponto` por `ponto.tests`. A guarda retorna **1**, confirmado executando somente `_suite_inteira` extraída em memória; com as LABELS originais retorna **0**. A diferença é `ponto.management.commands.testar_pwa`, incluído pelo `find test*.py`, mas fora do prefixo `ponto.tests`. (`bin/regua.sh:23`, `bin/suite.sh:148`, `bin/suite.sh:155`, `bin/suite.sh:157`, `app/ponto/management/commands/testar_pwa.py:31`)

Esse arquivo é comando de produção: importa `BaseCommand` e define `Command(BaseCommand)`; os seus métodos são `t01_...`, etc., chamados por `handle`, sem classes `TestCase` ou métodos `test_*`. Assim, sua exclusão não retira casos unittest pela análise estática; igualdade de casos no Django efetivamente instalado **[nao verificado]**. (`app/ponto/management/commands/testar_pwa.py:12`, `app/ponto/management/commands/testar_pwa.py:31`, `app/ponto/management/commands/testar_pwa.py:70`, `app/ponto/management/commands/testar_pwa.py:100`, `app/ponto/management/commands/testar_pwa.py:134`)

Outros limites:

- Enumerar todas as classes/métodos pode somar a mesma suíte, mas nunca cobre o nome mais curto do módulo na comparação implementada. Seleção integral concreta por classes nesta cópia **[nao verificado]**. (`bin/suite.sh:156`)
- Nome da **pasta** fora de `test*.py` não é problema isolado: `find` percorre pastas sem exigir esse padrão; aplica-o somente ao arquivo. (`bin/suite.sh:158`, `bin/suite.sh:159`)
- A guarda não verifica `__init__.py` e pode contar módulos que a descoberta real não percorre; efeito exato no Django instalado **[nao verificado]**. (`bin/suite.sh:158`, `app/requirements.txt:13`, `app/core/runner_quarentena.py:76`)
- `migrations`, `__pycache__`, `node_modules`, `staticfiles` são excluídos explicitamente, embora isso não esteja espelhado em override do runner da cópia. Pode haver recorte falso positivo quando testes reais estejam nesses pacotes; ocorrência atual **[nao verificado]**. (`bin/suite.sh:158`, `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`)
- `find` sem `-L` e com `-type f` não segue symlinks de arquivos/pastas; pode subcontar descoberta e chamar recorte de inteira. Ocorrência atual **[nao verificado]**. (`bin/suite.sh:158`, `bin/suite.sh:159`)
- Caminho externo com espaço/acento está citado no cálculo da árvore e no argumento do `find`; não há quebra de palavras ali. Já os nomes selecionáveis permanecem ASCII. (`bin/suite.sh:104`, `bin/suite.sh:149`, `bin/suite.sh:155`, `bin/suite.sh:158`)
- `--dir` escolhe `<DIR>/app` tanto para a guarda quanto para o bind principal, que canonicaliza `DIR`; não achei troca ordinária de árvore nesses caminhos. A montagem adicional sobrepõe `staticfiles` e caches/logs/media; `staticfiles` já é prunado pela guarda. (`bin/suite.sh:149`, `bin/suite.sh:194`, `bin/suite.sh:197`, `bin/suite.sh:220`, `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:80`, `bin/arvore_do_push.sh:81`)
- Custo da classificação é até número de módulos × número de palavras selecionadas, inclusive com motivo válido e em `--seco`, pois `_suite_inteira` é avaliada primeiro. (`bin/suite.sh:154`, `bin/suite.sh:156`, `bin/suite.sh:163`, `bin/suite.sh:173`)

O erro de `find` é ignorado pela substituição de processo; ausência de árvore é ignorada por `continue`. Erro sem resultados conduz a **inteira**, não a “não inteira”; o único retorno 1 explícito é um módulo emitido sem cobertura. Assim, falha de busca pode causar recusa falsa de recorte, mas não há conversão direta `find falhou → não inteira`. (`bin/suite.sh:153`, `bin/suite.sh:157`, `bin/suite.sh:158`, `bin/suite.sh:161`)

O host tem Python 3.12 e nenhum módulo Django instalado; a cópia declara imagem Python 3.13 e Django 5.2.14. Logo não usei a stdlib do host como prova da descoberta instalada, nem executei Django ou scripts que escrevem. (`app/Dockerfile:1`, `app/requirements.txt:13`; ambiente instalado **[nao verificado]**)

## 11/11 07:49 · sessao 103424a4cba2 · - · - · origem=Ronald (tmux)

Os três mutantes novos têm discriminantes pela causa pretendida; a cobertura da guarda continua incompleta. (`bin/tests/test_suite_motivo.sh:102`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:112`, `bin/tests/test_suite_motivo.sh:177`)

- **executor_extra_junta:** `%q` registra o vivo como `--tag x --failfast` e o mutante como `--tag\ x\ --failfast`; o grep exige três palavras. Conferi essa diferença em Bash, em memória. (`bin/tests/test_suite_motivo.sh:23`, `bin/tests/test_suite_motivo.sh:102`, `bin/tests/test_suite_motivo.sh:103`, `bin/tests/test_suite_motivo.sh:177`)
- **valor_aceita_flag:** a função viva recusa os quatro `f --seco` com rc 2 e “precisa de valor”; a mutada aceita o argumento. Mesmo quando a guarda posterior der rc 2, faltará o diagnóstico exigido: não é morte colateral apenas por rc. Conferi a função em memória. (`bin/suite.sh:51`, `bin/tests/test_suite_motivo.sh:111`, `bin/tests/test_suite_motivo.sh:112`, `bin/tests/test_suite_motivo.sh:178`)
- **descoberta_por_nome_de_app:** a enumeração completa dos três módulos é inteira no vivo, recorte no mutante, porque os módulos não são `appA`/`appA.tests` ou `appB`/`appB.tests`; portanto o mutante deixa passar precisamente a classe que devia barrar. Conferi a função em memória com o `find` da fixture simulado. (`bin/suite.sh:156`, `bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`, `bin/tests/test_suite_motivo.sh:179`)

**Sobrevivente plausível demonstrado:** trocar `"$s".*` por `"$s"*`. Todas as seleções da fixture mantêm o retorno do vivo, pois não há módulos irmãos com prefixo coincidente. Com um hipotético `test_x_extra.py`, selecionar somente `test_x` passa a cobrir indevidamente o irmão e pode recusar um recorte legítimo. A comparação das seleções existentes e desse discriminante adicional foi executada somente em memória. (`bin/suite.sh:156`, `bin/tests/test_suite_motivo.sh:18`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:109`)

Os outros dois exemplos **não sobrevivem** à cobertura atual:

- Remover todo o `prune` inclui `appA/tests/migrations/test_0001.py`; a enumeração esperada como completa deixa esse arquivo descoberto fora e passa, contrariando o teste negativo. (`bin/suite.sh:158`, `bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:106`)
- Trocar `[ $cob = 0 ] || return 1` por `continue` ignora módulos descobertos sem cobertura e acaba retornando inteira para todos os recortes, quebrando os positivos. (`bin/suite.sh:157`, `bin/suite.sh:161`, `bin/tests/test_suite_motivo.sh:56`, `bin/tests/test_suite_motivo.sh:109`)

Execução integral do selo e dos mutantes: **[nao verificado]**, pois o selo cria, escreve e remove arquivos. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:11`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:31`)

## 11/11 07:48 · sessao 40220f0ab37e · FIO-E0-V4-E3 · verificador · origem=Code

**1. PROCEDE o fechamento do sobrevivente parcial; NÃO PROCEDE considerar a matriz inteira coberta.** O marcador continua dependendo de `not _coberto_inteiro_nb and sem_trabalho`; ausência parcial não é integral, e `trabalha=False` prevalece no juiz. Os testes novos exigem o marcador justamente nessa combinação. `app/ponto/services/cartorio.py:1211`, `:1223`; `app/chamados/juizes.py:454`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:309`; `app/ponto/tests/test_x1_folga_sem_adesao.py:126`, `:141`.

| Recorte | Comportamento do código atual |
|---|---|
| Folga + ausência parcial aprovada | Mantém marcador. `app/ponto/services/cartorio.py:1223`; `app/chamados/juizes.py:454`. |
| Feriado + ausência parcial | Com `trabalha=False`, mantém; com `True`/`None`, a ata parcial não dispensa o trabalho pelo juiz existente. Ocorrência concreta desse segundo estado: **[nao verificado]**. `app/chamados/juizes.py:454`; `app/ponto/services/cartorio.py:1253`. |
| Folga dentro de afastamento aprovado integral | Fica sem marcador deliberadamente, pela exceção de integralidade; não reproduz a perda indevida da parcial. `app/ponto/services/cartorio.py:1211`, `:1223`. |
| Folga + ausência ainda não aprovada | Mantém marcador: cobrança aceita não rejeitadas, mas a grade só reconhece aprovadas para folha; `trabalha=False` mantém a dispensa. `app/ponto/turnos.py:2517`, `:2519`; `app/escala/utils.py:1188`; `app/chamados/juizes.py:456`. |

Não encontrei nesses recortes outra perda do marcador **no código atual**. Entretanto, ausência ainda não aprovada permanece uma lacuna dos testes examinados, demonstrada pelo mutante da pergunta 3. `app/ponto/tests/test_x1_folga_sem_adesao.py:99`, `:116`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:131`.

**2. PROCEDE a fragilidade da primeira testemunha, com uma distinção.**

- Se o veredito anterior estiver vazio e o julgamento forçado escrever `concorde`, a igualdade **morde**. Se ambos permanecerem `None`, ou ambos `''`, igualdade e exclusão dos acusadores **passam**. `app/ponto/tests/test_x1_folga_sem_adesao.py:132`, `:135`, `:136`; `app/escala/models.py:1767`.
- O C2b inteiro continua verificando ausência, hash, rejulgamento e veredito final derivado. Porém, esse último veredito é o **posterior à adesão**: não certifica sozinho que o NB inicial estava preenchido. O teste de memória exige positivamente `concorde` nessa etapa. `app/ponto/tests/test_x1_folga_sem_adesao.py:137`, `:141`, `:151`, `:160`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:316`.

O `forcar=True` contorna a guarda NB. A criação da ausência anteriormente aciona o signal sem força, mas o C2b não observa diretamente sua execução nem exige um veredito inicial positivo. A segunda metade cobre explicitamente o julgamento sem força **após a adesão**. `app/ponto/services/cartorio.py:1228`; `app/ponto/signals.py:93`, `:143`, `:147`; `app/ponto/tests/test_x1_folga_sem_adesao.py:116`, `:133`, `:151`.

Os caminhos ordinários sem força existem no signal, no cron e no reconciliador. Há também uma segunda passada produtiva que força condicionalmente; portanto, produção não é exclusivamente sem força. `app/ponto/signals.py:93`; `app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:29`, `:65`; `app/chamados/reconciliador.py:702`; `app/ponto/services/cartorio.py:985`, `:989`.

**3. PROCEDE uma lacuna com sobrevivente demonstrado sem banco; sobrevivência nos 116 ORM: NÃO SEI.**

Mutante plausível na chamada do marcador: acrescentar a condição abaixo, confundindo aprovação com necessidade de registrar a fonte sem adesão. `app/ponto/services/cartorio.py:1223`.

```python
and getattr(_cob_nb, 'status', None) in (None, 'aprovada')
```

Na execução efêmera desta volta, esse mutante passou **19 testes de memória + sete pós-suite**, com zero falhas/erros. Uma testemunha adicional de folga com `aguardando_decisao` produziu `concorde` nos dois códigos, mas somente o original manteve o marcador. Resultado sem transcrição em arquivo: **[nao verificado por arquivo:linha]**. [nao verificado]

A não equivalência tem fundamento no código: esse status entra em cobrança, não constitui cobertura integral aprovada na grade e não deve retirar o marcador da folga. As coberturas dos testes citados são `None` ou aprovadas. `app/ponto/turnos.py:2519`; `app/escala/utils.py:1188`; `app/ponto/services/cartorio.py:1211`, `:1223`; `app/ponto/tests/test_x1_folga_sem_adesao.py:101`, `:120`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:131`.

Os exemplos mais amplos de marcador apenas na integral ou hash completamente cego ao marcador já encontram testemunhas: folga simples exige hash diferente, e C3 exige rejulgamento após batida distante. `app/ponto/tests/test_e3_nb_impressao_memoria.py:264`; `app/ponto/tests/test_x1_folga_sem_adesao.py:181`, `:185`.

Dois limites adicionais: o executor fixa `BASE=7313a031…`, não `52a0022e9`; e `so_acusador` segue sempre pelo executor sem banco, mesmo sem a flag. Logo, “base RED” e essa morte não certificam, sozinhos, comparação ORM contra a base produtiva indicada. `app/ponto/tests/x1_mutantes.py:20`, `:94`, `:105`.

**4. NÃO PROCEDE segundo juiz novo no adaptador; NÃO SEI certificar exaustão do diff total sem a fonte histórica da base. [nao verificado]**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | NB deriva, aplica o rename e solicita lavra; a porta grava fisicamente. O ramo normal entrega códigos à mesma porta. `app/ponto/services/cartorio.py:1238`, `:1253`, `:1256`, `:1817`; `app/ponto/portas/celula.py:923`, `:1035`. | Guardas do cartório; retenção; universo de adesão/C5; fábrica; disputa; veto; reconciliador; grade leitora/status/relatório. `app/ponto/services/cartorio.py:1224`, `:1373`; `app/folha/export.py:1241`; `app/chamados/services/adesao.py:32`; `app/escala/services/cadastro_realidade.py:181`; `app/chamados/services/fabrica_da_celula.py:89`, `:120`; `app/chamados/services/disputa_emissao.py:758`; `app/ponto/marcos.py:155`; `app/chamados/reconciliador.py:637`; `app/escala/services/leitor_celula.py:424`; `app/ponto/status_dia.py:68`; `app/relatorios/dia_da_frota.py:51`. |
| `impressao` / `sem_adesao` | NB fornece o booleano; a função acrescenta **`sem_adesao:1` ao material do SHA256**. Não é chave persistida separadamente. A porta grava o hash; o ramo normal omite o argumento, cujo default é falso. `app/ponto/services/cartorio.py:444`, `:525`, `:527`, `:1223`, `:1368`; `app/ponto/portas/celula.py:1066`. | Guardas NB e normal e idempotência da porta. `app/ponto/services/cartorio.py:1224`, `:1373`; `app/ponto/portas/celula.py:1062`. |
| `out['cods']` | Ramo normal escreve; releitura agrega; costura devolve ao lote. NB retorna sem escrever. `app/ponto/services/cartorio.py:1441`, `:1055`, `:1084`, `:1282`. | Fábrica e cluster usam a presença da data julgada **neste ato**; não representa todo veredito gravado. `app/ponto/services/cartorio.py:1002`, `:1006`, `:1012`. |
| `data_civil` sintética | Ambas as espécies recebem `None`; marcos reais recebem a data local do instante. `app/chamados/services/tarefa_do_dia.py:103`, `:316`, `:331`. | Carga entrega o resultado; enriquecimento consulta ações/portas; sombra projeta rota/fila/verbos/fio. Outro consumidor direto: **[nao verificado]**. `app/chamados/services/tarefa_do_dia.py:523`, `:533`, `:545`; `app/chamados/management/commands/diff_e3_sombra.py:27`, `:44`. |
| Retorno de `dia_sem_trabalho` | Autoridade responde pelas duas cláusulas, com exceção explícita da parcial. `app/chamados/juizes.py:454`, `:456`. | Marcador/rename, suprajuiz, tarefa, fábrica, fechamento de chamado, materialização, cluster e vigia de intervalo. `app/ponto/services/cartorio.py:1220`, `:1223`, `:1253`; `app/ponto/supra_juiz.py:496`; `app/chamados/services/tarefa_do_dia.py:207`; `app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/models.py:1284`; `app/chamados/services/materializacao.py:174`; `app/ponto/services/cluster_espurio.py:25`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`. |

Os significados são **distintos e compatíveis**: julgamento usa dia do turno; `data_civil` pertence ao instante do marco. A janela civil `[data−1,data+2)` corresponde às datas inclusivas de `data−1` a `data+1`; ausência usa faixa fechada; período parcial inclui ambas as horas e ancora a madrugada no início do turno. `app/ponto/services/cartorio.py:445`, `:1146`, `:1322`, `:1383`; `app/chamados/services/tarefa_do_dia.py:103`; `app/ponto/turnos.py:2540`, `:2623`, `:2630`.

O adaptador apenas transporta insumos e chama a autoridade. **Não são todos os leitores que usam predicado idêntico:** `_protesta` continua parcialmente migrado, como declarado em JUIZES; trabalho previsto e dispensa de cobrança são perguntas diferentes. `app/ponto/supra_juiz.py:60`, `:71`; `app/ponto/services/cartorio.py:1423`; `app/docs/JUIZES.md:64`, `:66`, `:75`.

**5. PROCEDEM os descartes somente com seus limites; PROCEDE a causa técnica do e3fix6.**

- **Base versus d6:** o HEAD conserva NB e hash sem marcador para trabalho exigível. A história de pouso e o conteúdo exato de `52a0022e9` são dados fornecidos, sem comprovação independente nesta cópia. `app/ponto/services/cartorio.py:1223`, `:1253`; **[nao verificado: histórico/base]**.
- **Relógio:** o residual permanece; anterioridade não está demonstrada pelo HEAD isolado. `app/escala/servico_jornada.py:387`, `:566`; **[nao verificado: anterioridade]**.
- **Reconciliador:** zero candidatos no recorte informado não fecha a classe. O escritor continua carimbando perguntas e podendo fechar disputas; o teste atual exige esse carimbo. `app/chamados/reconciliador.py:595`, `:611`; `app/ponto/tests/test_x1_folga_sem_adesao.py:315`.
- **TXT:** estoque armazenado e regeneração são provas diferentes; C4 verifica registros/contagens, sem gerar outro TXT. `app/folha/views.py:561`; `app/folha/export.py:726`, `:968`; `app/ponto/tests/test_x1_folga_sem_adesao.py:213`, `:219`.
- **e3fix6:** forçar vence a guarda, mas NB retorna antes de escrever `cods`; a objeção ao acesso antigo procede. Diff historicamente vazio: **[nao verificado]**. `app/ponto/services/cartorio.py:1228`, `:1282`, `:1441`.

O relato também ficou desatualizado ao descrever rename por conjunto acusatório e marcador em todo o ramo; o código vivo usa `sem_trabalho` e exclui integralidade. `app/docs/relato_fio_e0_v4_e3_x1.txt:11`, `:22`, `:28`; `app/ponto/services/cartorio.py:1223`, `:1253`.

**Fecho: NÃO LIBERA como pré-verificação final.** Falta tornar positiva a testemunha NB inicial do C2b e cobrir o marcador com ausência ainda não aprovada. São lacunas de verificação; não demonstrei perda desse marcador no HEAD atual. `app/ponto/tests/test_x1_folga_sem_adesao.py:135`, `:136`, `:120`; `app/ponto/services/cartorio.py:1223`.

## 11/11 07:46 · sessao 44f945fbb965 · - · - · origem=Ronald (tmux)

Q4 — **NÃO PROCEDE duplicação nova de juiz; NÃO SEI certificar exaustão do diff total**, pois a cópia não fornece histórico Git verificável. [nao verificado]

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | Cartório NB calcula `derivar_veredito`, preserva-o quando `sem_trabalho` e renomeia trabalho para NB (`app/ponto/services/cartorio.py:1238`, `:1253`, `:1256`). Escrita física pela porta única (`app/ponto/portas/celula.py:1035`); método do modelo delega (`app/escala/models.py:1819`). | Guardas do cartório (`cartorio.py:1224`, `:1373`); retenção (`app/folha/export.py:1241`, `:1263`); universo adesão/C5 (`app/chamados/services/adesao.py:32`, `app/escala/services/cadastro_realidade.py:181`); fábrica (`app/chamados/services/fabrica_da_celula.py:89`, `:105`, `:120`, `:183`); disputa (`app/chamados/services/disputa_emissao.py:758`, `:772`); veto de cobrança (`app/ponto/marcos.py:119`, `:155`, `:161`); reconciliador (`app/chamados/reconciliador.py:637`); grade leitora (`app/escala/services/leitor_celula.py:424`), depois tarefa (`app/chamados/services/tarefa_do_dia.py:327`); status (`app/ponto/status_dia.py:68`); relatório (`app/relatorios/dia_da_frota.py:51`). |
| `impressao`, `sem_adesao` | NB passa o booleano produzido pelo juiz (`cartorio.py:1218`–`:1223`); função acrescenta fragmento **`sem_adesao:1`**, e produz SHA256 (`cartorio.py:525`–`:527`). Não é chave persistida separada. Porta grava hash (`app/ponto/portas/celula.py:1066`). | Guarda NB (`cartorio.py:1224`, `:1228`), guarda normal (`:1373`, `:1377`) e idempotência da porta (`app/ponto/portas/celula.py:1062`). Normal omite argumento e usa default falso (`cartorio.py:444`, `:1362`–`:1368`). |
| `out['cods']` | Ramo normal (`cartorio.py:1441`), releitura agrega (`:1055`), costura devolve ao lote (`:1084`–`:1089`). NB retorna sem escrever (`:1282`). | Fábrica e cluster, usando presença da **data julgada neste ato** (`cartorio.py:1002`–`:1013`). Não representa todos os vereditos gravados: NB fica fora (`:1282`). |
| `data_civil` sintética | Ambas as espécies recebem `None` (`app/chamados/services/tarefa_do_dia.py:316`, `:331`). Marcos reais usam data local do instante do marco (`:103`). | Carga entrega dicionário (`:523`, `:533`); enriquecedor lê ações/portas (`:545`–`:554`). Sombra projeta somente rota/fila/verbos/fio (`app/chamados/management/commands/diff_e3_sombra.py:27`, `:44`). Outro consumidor direto na cópia: [nao verificado]. |
| retorno de `dia_sem_trabalho` | Autoridade única: parcial em ata exige `trabalha is False`; demais casos aceitam `trabalha=False` ou tipo preenchido diferente de trabalho (`app/chamados/juizes.py:454`–`:456`). | Cartório marcador e renomeação (`cartorio.py:1220`, `:1223`, `:1253`), suprajuiz (`app/ponto/supra_juiz.py:496`), tarefa (`app/chamados/services/tarefa_do_dia.py:207`), fábrica (`app/chamados/services/fabrica_da_celula.py:112`, `:201`), fechamento de chamado (`app/chamados/models.py:1284`), materialização (`app/chamados/services/materializacao.py:174`), cluster (`app/ponto/services/cluster_espurio.py:25`), vigia de intervalo (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`), `_celula_furada` (`app/chamados/juizes.py:494`). |

Os significados são diferentes e explicitados: julgamento é por **dia do turno**, indexado por `cel.data`; `data_civil` pertence ao instante de cada marco e pode cair no dia seguinte. A janela de batidas `[data-1,data+2)` equivale ao filtro de datas civis inclusivas de `data-1` a `data+1`; a conversão entre `lt` e `lte` desconta um microssegundo. Cobertura de ausência fecha ambas as pontas; carga da tarefa também usa `[ini,fim]`. (`app/ponto/services/cartorio.py:445`, `:1135`, `:1140`, `:1322`, `:1383`; `app/ponto/turnos.py:2540`; `app/chamados/services/tarefa_do_dia.py:103`, `:517`, `:562`.)

**Não se deve afirmar que todo leitor usa predicado idêntico.** `_protesta` continua lendo somente `trabalha is not False` e marcos acesos; essa migração parcial já está declarada. Também há dois efeitos deliberados de cobertura: folha exige aprovada; cobrança admite não rejeitada. (`app/ponto/services/cartorio.py:1422`–`:1423`; `app/docs/JUIZES.md:75`–`:77`; `app/ponto/turnos.py:2511`–`:2519`.)

O adaptador **não é segundo juiz**: transporta `trabalha` e `ata`, exige o insumo e chama `chamados.juizes.dia_sem_trabalho`. A pergunta de trabalho **previsto** pertence a `EscalaColaborador.eh_dia_trabalho`; a de cobrança que pode calar pertence a `dia_sem_trabalho`. (`app/ponto/supra_juiz.py:60`–`:71`; `app/docs/JUIZES.md:64`–`:69`, `:103`.)

A mudança de NB para palavra não acusadora pode alcançar o reconciliador: seu seletor exige palavra preenchida fora dos acusadores, depois carimba perguntas e pode fechar disputa. Isso é leitura existente da nova palavra, sem recalcular a folga; zero candidatos na classe inteira não está demonstrado. (`app/chamados/reconciliador.py:595`, `:611`–`:614`, `:637`–`:641`; [nao verificado].)

_3729 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
