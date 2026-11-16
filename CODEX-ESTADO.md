# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 22:39** · publicado em **15/11/2026 22:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 22:39 · sessao d907d4904e3b · - · - · origem=Ronald (tmux)

Classificaria `app/relatorios/views.py:1540-1541` como **observação**, sem violação demonstrada de L-090: a tela aceita período livre, e o contrato distingue competência default de datas explícitas. O contraexemplo prova a normalização para mês civil, mas não que esse resultado seja apresentado como competência. As datas efetivas aparecem nos campos e nos cards. (`app/relatorios/tests/test_relatorio_janela_da_empresa.py:44-60`; `app/templates/relatorios/furos.html:12-13`; `:41`.)

Não encontrei relógio adicional ativo nos caminhos examinados com `agora` válido: o calendário recebe o instante, usa-o no desfazer e no D4; `NAO_PERGUNTADO` pula o ramo que consultaria `autoridade_do_periodo`/`folha_manda`. (`app/relatorios/furos.py:168`; `app/colaboradores/services/calendario.py:192-193`; `:279-283`; `:594-600`; `:638`.)

O `localdate()` sem argumento de `flip_batida.py:38` pertence ao escritor `flip_tipo`, não à leitura `flips_de` usada pelo calendário; esta apenas converte carimbo explícito. (`app/ponto/services/flip_batida.py:10`; `:38`; `:56`; `:77`; `app/colaboradores/services/calendario.py:644`.)

## 15/11 22:39 · sessao 58f3c671885b · - · - · origem=Ronald (tmux)

Auditoria dos selos e leis, só leitura:

- **Testes c17 têm mordida comportamental.** `funcao` extrai e compila o corpo real, substituindo infraestrutura (`app/chamados/tests/e4_apoio.py:8`); os casos 01–05 verificam universo visível/interseção, ausência de foto, alarme estrutural parcial, exclusão de competências antigas e unidades/legado/ordenação (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:97`, `:107`, `:116`, `:129`, `:142`). Os casos 06–07 verificam dia obrigatório do limbo e transporte idêntico de instante/carga ao D4 e calendário, inclusive recusa de omissão/None/tipos inválidos (`:167`, `:193`, `:219`). O caso 08 exercita o detector de chamadas repetidas, soltas e aliases (`:225`).
- **Caso 09 é documental somente.** Ele verifica substrings de docstrings; não demonstra por si mesmo que exclusões, universo ou relógios estejam corretos (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:252`–`:269`). Isso não torna os outros oito casos vazios.
- Executei os nove testes c17 com `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ... -v`: todos passaram. Execução RED na base e ordem histórica “testes antes das curas” continuam **[nao verificado]**; são afirmações do docstring (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:3`). Cópia local não tem `.git`; `git show f31a8d09e` falhou **[nao verificado por arquivo:linha]**.
- **Limite real do selo de relógio:** `_chamadas_relogio` considera `localdate(valor)`/`localtime(valor)` conversões sempre que o argumento não seja o literal `None`, mesmo quando `valor` possa ser `None` em runtime (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:184`). Contraexemplo: `frescor_dos_retratos(agora=None)` satisfaz a assinatura obrigatória e chama `timezone.localtime(None)`, permitindo novo relógio (`app/folha/retrato_folha.py:1387`, `:1409`); o selo exige só ausência de chamadas sintáticas diretas e ausência de default (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:231`). **Observação de fragilidade preexistente ao c17 segundo o diff fornecido**, sem chamador real que forneça None demonstrado.
- **Dataset valida datetime, não awareness.** `dataset_furos(..., agora=datetime.datetime(2026,11,15))` passa pela guarda de tipo (`app/relatorios/furos.py:113`), mas downstream `timezone.localdate(agora)` exige instante aware (`app/ponto/status_dia.py:254`). Teste 07 não exercita instante naive (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:213`). **Observação de contrato incompleto**, não ocorrência em chamador real demonstrada.
- **Teste 07 não prova significado transitivo de “hoje”.** Seus HOJE e AGORA são propositalmente datas diferentes, com D4/calendário substituídos por mocks; ele prova identidade de transporte (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:25`, `:200`, `:219`). O próprio selo declara “sem fecho transitivo” (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:169`).
- **C5 não alarga []:** `colabs_nunca_bateu([], hoje)` usa `Empresa.objects.filter(pk__in=[])` (`app/escala/services/cadastro_realidade.py:163`). Tem fallback próprio `hoje=None -> localdate()` (`:137`, `:161`), mas o vigia injeta hoje explicitamente (`app/folha/vigia_retido_calado.py:72`), sem acioná-lo no caminho normal.
- **Teste c5 R2 não ficou stale:** ele espera TypeError por ausência de `hoje`, antes de qualquer import/ORM; o mock já possui `.filter` (`app/chamados/tests/test_e3_r3_c5_sem_banco.py:232`, `:236`, `:245`).
- **Limbo segue pendência antiga registrada**, não juiz novo criado por c17: `core/juizes.py` mantém o sítio e apenas atualiza a assinatura (`app/core/juizes.py:794`). Limbo consulta `eh_dia_trabalho(hoje)` e amanhã (`app/relatorios/furos.py:67`); `FolgaDia` mensal é diagnóstico de foto civil, não decisão de competência (`:86`, doc `:56`), portanto não é automaticamente violação L-090.

Leis relevantes para consolidar:

- Testemunha lê autoridade única: `app/docs/LEIS.md:60`; consumidor lê lâmpada: `:95`.
- Competência por corte da empresa, não calendário civil: `app/docs/LEIS.md:123`.
- Relógio uma vez na borda e parâmetro até julgamento: `app/docs/GLOSSARIO.md:19`, `app/docs/PLANO-PADROES.md:92`; default None/fallback interno é armadilha explicitada em `:94`.
- Vigia somente alarma, contador esperado zero: `CLAUDE.md:172`; um juiz e um escritor por domínio: `CLAUDE.md:178`.
- UTC no banco/apresentação local: `CLAUDE.md:146`; configuração `America/Sao_Paulo`, `USE_TZ=True`: `app/config/settings/base.py:93`, `:95`.
- Corte de publicação do Retrato não prova pré-requisitos de deploy pela inspeção desta cópia: `app/docs/CORTES.md:78`; ocorrência/deploy **[nao verificado]**.

Nenhum **PROCEDENTE novo de c17** demonstrado nesta fatia de selos/leis; as fragilidades de None/naive e a prova documental do caso 09 devem constar como observações com os limites acima.

## 15/11 22:38 · sessao 31006ac546a5 · - · - · origem=Ronald (tmux)

Inventário e conclusões do vigia:

- **Escritor persistente do retrato:** `lavrar_retrato_folha.handle` grava `empresa/mes/ano`, `linhas` e `lavrado_em` por `update_or_create`, somente com `--apply`; lê um único `_agora=timezone.now()`, deriva `_hoje=timezone.localtime(_agora).date()`, passa ambos a `montar_linhas`, e conserva `_agora` como início da rodada (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:50`, `:65`, `:72`, `:77`). Há constraint por empresa/competência (`app/folha/models.py:365`). Nos sítios de produção que nomeiam `RetratoFolha`, não encontrei segundo escritor; os demais usos são consultas (`app/folha/views_retrato.py:194`, `app/folha/retrato_folha.py:1413`, `app/colaboradores/views.py:982`, `app/folha/vigia_retido_calado.py:59`).

- **Competências:** escritor da lista = `competencias_do_retrato`; lê `janela_anterior` e `janela_atual`, extrai mês/ano do fim e elimina repetição (`app/folha/retrato_folha.py:1369`, `:1378`, `:1380`, `:1382`). Leitores de produção = lavrador automático, frescor e vigia (`app/folha/management/commands/lavrar_retrato_folha.py:61`; `app/folha/retrato_folha.py:1412`; `app/folha/vigia_retido_calado.py:66`). `--mes/--ano` é seleção explícita do lavrador, não derivação concorrente (`app/folha/management/commands/lavrar_retrato_folha.py:57`). O juiz respeita corte por empresa e datas inclusivas corte→corte−1 (`app/ponto/janelas.py:10`, `:16`, `:20`, `:60`, `:66`).

- **Universo mudou:** agora parte de todas as `empresas_visiveis`, inclusive sem nenhuma foto, intersecta IDs pedidos e consulta retratos só desses IDs (`app/folha/vigia_retido_calado.py:54`, `:55`, `:58`, `:59`, `:61`). `empresas_visiveis` exclui inativas e CNPJ diagnóstico `11222333*` (`app/colaboradores/queries.py:78`, `:85`). Antes, segundo o diff anexado, partia das empresas encontradas em retratos e C5 recebia recorte original apenas havendo foto; comparação independente via Git [nao verificado] porque a cópia não tem repositório Git. Agora pares faltantes saem até para empresa sem foto, ordenados empresa/ano/mês; C5 recebe sempre IDs efetivos (`app/folha/vigia_retido_calado.py:65`, `:69`, `:72`).

- **Quem escreve/lê o envelope de nove chaves:** só `censo` constrói e acumula `retratos`, `competencias_sem_retrato`, `bloqueados`, `calados`, `roteados`, `dias_sem_leitura_sistema`, `linhas_sem_campo_leitura`, `por_empresa` e `cego` (`app/folha/vigia_retido_calado.py:73`, `:88`, `:95`, `:100`, `:101`). `linhas` lê o envelope e emite texto; `handle` lê as chaves de alarme (`app/folha/vigia_retido_calado.py:105`; `app/folha/management/commands/vigiar_retido_calado.py:34`, `:38`). Trios das listas são `(empresa,mes,ano)`; chaves de `por_empresa` são `(empresa,ano,mes)`, distintos e coerentes no consumidor (`app/folha/vigia_retido_calado.py:88`, `:95`, `:108`, `:110`).

- **Unidades e desconhecimento:** bloqueados/calados/roteados contam pessoa por retrato/competência; dias sem leitura somam listas das linhas bloqueadas; chave ausente marca legado desconhecido, chave presente `None` soma zero; positivo conhecido sobrevive ao legado (`app/folha/vigia_retido_calado.py:77`, `:79`, `:80`, `:84`, `:85`, `:87`, `:100`). Tests efetivos desses contratos: `app/chamados/tests/test_e3_r3_c17_sem_banco.py:97`, `:107`, `:116`, `:129`, `:142`. Incluem interseção, empresa sem foto, alarme parcial, preservação do positivo, unidade repetida por competência e virada do ano (`:103`, `:110`, `:123`, `:150`, `:162`).

- **Relógio do vigia:** único chamador de produção encontrado é o command, que fornece `hoje=timezone.localdate()` uma vez; `censo` exige keyword `hoje` e o repassa a competência, dono e C5 (`app/folha/management/commands/vigiar_retido_calado.py:33`; `app/folha/vigia_retido_calado.py:41`, `:66`, `:72`, `:78`). `dono_da_linha` compara datas gravadas com o dia civil local, inclusive `>=hoje` para o balde Hoje; não decide fim de turno (`app/folha/retrato_folha.py:474`, `:480`, `:491`). Frescor e lavrador usam a mesma conversão local (`app/folha/retrato_folha.py:1409`; `app/folha/management/commands/lavrar_retrato_folha.py:50`). Drawer também usa instante convertido localmente antes de `.date()` (`app/colaboradores/views.py:930`, `:986`).

- **Fallbacks existentes, não acionados pelo caminho normal:** `colabs_nunca_bateu(hoje=None)` lê relógio internamente (`app/escala/services/cadastro_realidade.py:137`, `:161`), mas vigia fornece o dia (`app/folha/vigia_retido_calado.py:72`). `montar_linhas(hoje=None)`→`_informativo_rescisao`→`informativo_rescisao`→`_dias_sem_decisao` pode ler relógio no interior; dívida expressamente declarada (`app/folha/retrato_folha.py:1174`, `:1220`, `:1271`; `app/folha/export.py:1216`, `:1238`). Lavrador fornece dia e evita fallback (`app/folha/management/commands/lavrar_retrato_folha.py:65`).

- **Continua vigia:** sem escrita ORM nem transição no `censo`; só contagem e retorno; command apenas stdout e `SystemExit(2)` (`app/folha/vigia_retido_calado.py:59`, `:77`, `:102`; `app/folha/management/commands/vigiar_retido_calado.py:35`, `:42`). Cron chama com `--alarme`, depois do retrato, declarado papel vigia (`app/config/crons.py:1681`, `:1683`, `:2260`). `calados` e dias continuam esperado zero, mas só entram no alarme com `--alarme-passivo`; alarme estrutural inclui agora cego e competência sem foto (`app/folha/management/commands/vigiar_retido_calado.py:16`, `:17`, `:37`). Coerente com L-060/CLAUDE 4a (`app/docs/LEIS.md:94`; `CLAUDE.md:172`).

**Observação concreta:** comentário do cron ficou velho: diz `--alarme` “só pelo cego”, mas falta parcial de retrato também produz rc2 no command (`app/config/crons.py:1679`; `app/folha/management/commands/vigiar_retido_calado.py:16`, `:38`, `:42`). Contraexemplo já construído pelo teste: foto só de uma das duas competências, bloqueados positivos e cego zero, mesmo assim alarma `competencias_sem_retrato` (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:117`, `:119`, `:123`).

**PROCEDENTES de comportamento neste subconjunto:** nenhum identificado. Não há evidência disponível de número real de produção nem de execução dos testes com banco [nao verificado]. [nao verificado]

## 15/11 22:38 · sessao d907d4904e3b · - · - · origem=Ronald (tmux)

Inventário dos alvos:

| Alvo | Quem fornece / quem lê | Assinatura antiga |
|---|---|---|
| `escalas_em_limbo(empresa=None, *, hoje)` | Produção: `furos_espelho` fornece `hoje` em `app/relatorios/views.py:1554`; a borda lê `now()` uma vez e converte com `localtime(...).date()` em `:1519-1520`. A função pergunta ao juiz `ec.eh_dia_trabalho` para hoje e amanhã em `app/relatorios/furos.py:67` e usa o mês civil da foto `FolgaDia` em `:86-89`. | Não identificada. Teste atualizado em `app/relatorios/tests/test_selo_ancora_do_vinculo.py:45`, com dia fixo em `:23`. |
| `dataset_furos(..., *, agora)` | Produção: `app/relatorios/views.py:1543` fornece `_agora`; dataset envia o mesmo objeto ao D4 em `app/relatorios/furos.py:127` e ao calendário em `:168`. | Não identificada. Testes em `app/relatorios/tests/test_e4_8_furos_le_a_lavra.py:94` e `app/ponto/tests/test_cobparc_v4_procedentes.py:288` fornecem instante. |

O D4 deriva o dia com `timezone.localdate(agora)`; o calendário deriva com `timezone.localtime(_agora).date()`. Ambos usam o fuso ativo; a configuração padrão é `America/Sao_Paulo`, `USE_TZ=True`. São conversões equivalentes para um instante aware no mesmo contexto. (`app/ponto/status_dia.py:252-254`; `app/colaboradores/services/calendario.py:192-193`; `app/config/settings/base.py:93-95`.)

Os intervalos do dataset são inclusivos nas duas pontas; o julgamento de encerramento compara `agora >= fim` e usa `data_turno` como dia do turno. Portanto dia civil e dia do turno são insumos distintos, sem evidência de confusão nova neste caminho. (`app/relatorios/furos.py:126`; `app/ponto/status_dia.py:273-274`; `app/ponto/turnos.py:2111`; `app/ponto/turnos.py:2124`.)

As exclusões têm caminhos reais:

- PDF do retrato: `retrato_pdf` chama `_recorte_da_barra` e depois `gerar_pdf_auditavel`, sem transmitir o instante do recorte. A função auditável lê `timezone.now()` e usa esse valor nos rodapés das duas passagens; também grava um `RegistroPDFAuditavel`. Isso não altera a seleção das linhas, já capturada em `_rec`. (`app/folha/views_retrato.py:388`; `:443`; `app/relatorios/pdf_base.py:133`; `:139`; `:152`; `:170`.)
- Aceite: `retrato_raiox` chama `_recorte_da_barra`, mas transmite somente `empresa_id` ao placar quando Dono=Colab. `placar_aceite` lê `now()` e `localdate()` separadamente, permitindo atravessar meia-noite entre as leituras. Ele usa janelas próprias hoje/7d e não recebe competência, praça, pessoas ou intervalo do recorte. (`app/folha/views_retrato.py:489`; `:513-515`; `app/chamados/services/verbos.py:277-288`; `:295-301`.)
- O PDF da auditoria de espelhos passa os cards a `gerar_pdf_furos_bytes`; não usa nenhuma dessas duas funções excluídas. (`app/relatorios/views.py:1562-1564`; `app/relatorios/furos_pdf.py:73-103`.)

A exclusão é coerente com a promessa limitada de transporte direto, expressamente “sem fecho transitivo”; não permite afirmar relógio único em toda a tela e seus componentes. O reuso do placar no Raio-X é ordem expressa do corte O256. (`app/folha/views_retrato.py:146-147`; `app/relatorios/furos.py:111`; `app/docs/CORTES.md:309`.)

Não confundir o mês civil da foto `FolgaDia` com competência de folha: a função diagnostica cadastro e consulta `eh_dia_trabalho`, sem construir competência. A leitura da foto é nominalmente permitida na catraca, e o registro FABRICANTE explica que substituir por `janela_atual` contrariaria o contrato da foto. (`app/relatorios/furos.py:67`; `:86-89`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:169`; `app/docs/FABRICANTE.md:63`.)

Os testes C17 do limbo e dataset exercitam comportamento: conferem o dia recebido, o mês da consulta, ausência de leitura do mock de relógio, obrigatoriedade do parâmetro, recusa dos tipos inválidos e identidade do instante nos dois fornecedores. Já o item 09 cobra somente termos dos docstrings. (`app/chamados/tests/test_e3_r3_c17_sem_banco.py:183-191`; `:212-223`; `:252-269`.)

Observações:

- A validação de `dataset_furos` aceita qualquer `datetime`, inclusive naive. Os leitores seguintes exigem conversão local de instante aware; o contrato do juiz também documenta aware. Nenhum chamador real passando naive foi identificado, portanto isso é limite de validação, não regressão demonstrada. (`app/relatorios/furos.py:113-114`; `app/ponto/status_dia.py:254`; `app/ponto/turnos.py:2071`.)
- D4 e calendário conservam fallback de relógio com `agora=None`, mas o caminho atualizado do dataset transmite instante obrigatório e não aciona esses fallbacks. (`app/ponto/status_dia.py:252-254`; `app/colaboradores/services/calendario.py:192`; `app/relatorios/furos.py:103`; `:127`; `:168`.)
- `escalas_em_limbo` permanece registrado em PENDENTES; o diff ajusta sua âncora à assinatura nova. O critério atual já consulta o juiz, e a leitura de vínculos abertos é declarada como diagnóstico de cadastro. (`app/core/juizes.py:794`; `app/relatorios/furos.py:67`; `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:207-208`.)

Achados PROCEDENTES novos de execução nesses dois alvos: nenhum demonstrado pela leitura acima; ocorrência em produção [nao verificado].

## 15/11 22:32 · sessao c02c80d002cb · E4C-F1NOVA-B1 · verificador · origem=Code

**ACHADOS: cinco médios e dois menores.** A recusa simétrica existe, mas a garantia de impedir todo meio-par ainda tem um furo em `save(update_fields=...)`; também há migração incompleta de leitor equivalente ao juiz e mudança de predicado no I21. (`app/chamados/models.py:3578`; `app/chamados/services/fio.py:187`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1572`)

Identidade independente do HEAD, comparação Git com `e84a167f4`, suíte com banco e prova de sombra: **[não verificado]**. Esta revisão usa a cópia e o patch fornecidos; não houve acesso a banco ou logs **[não verificado por arquivo]**.

**ACHADOS**

1. **Médio — `save` valida o par em memória, não necessariamente o par que será persistido.** `update_fields` apenas decide se a guarda será chamada; a validação seguinte usa os dois atributos atuais. Assim, partindo de `(None, '')`, atribuir em memória `(I, 'bati')` e salvar somente `resposta_colab` passa pela guarda, deixando texto sem instante; salvar somente `respondida_em` permite o inverso. Depois, `_par_carregado` recebe novamente o par inteiro em memória. É um furo herdado na porta alterada, agora relevante também para a nova direção; não encontrei escritor atual que faça essa combinação. (`app/chamados/models.py:3548`; `app/chamados/models.py:3553`; `app/chamados/models.py:3578`)

   O teste novo altera somente uma metade em memória e não cobre essa divergência entre memória e `update_fields`. A aceitação pela guarda foi reproduzida isoladamente em memória; execução desse contraexemplo no banco: **[não verificado]**. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:108`)

2. **Médio — I21 muda de predicado já no B1.** Antes, `respondida_em` presente e texto diferente de `''` incluíam respostas somente com espaços; agora a face SQL aplica trim e as exclui. O próprio comentário novo reconhece a diferença. Isso contraria o literal “nenhum leitor muda de predicado” do B1; existência e quantidade desses registros em produção: **[não verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; `construir_r3_c17_e_f1_nova.txt:1`, anexo)

3. **Médio — resta uma derivação equivalente ao juiz na classe que deveria apenas nomeá-lo.** `espera_o_texto_do_colaborador` mantém `instante ausente OU respondida_sem_texto`, exatamente o complemento de `pergunta_respondida`. Continua decidindo por conta própria para cobrança/cartão, embora seu resultado pudesse ser preservado delegando ao juiz. Sua classificação nominal no censo ausente: **[não verificado]**. (`app/chamados/services/fio.py:174`; `app/chamados/services/fio.py:177`; `app/chamados/services/fio.py:187`)

4. **Médio — o contrato estático ganha simetria, mas enfraquece a detecção pela “guarda”.** Qualquer leitura da outra metade em qualquer `if` absolve a função. Não exige condição correta, proteção da escrita ou correspondência entre a instância lida e o queryset atualizado. Os contraexemplos abaixo são absolvidos pelo detector real; a própria autoprova aceita guarda truthy, que também aceita espaços. (`app/chamados/tests/test_contract_par_resposta.py:58`; `app/chamados/tests/test_contract_par_resposta.py:78`; `app/chamados/tests/test_contract_par_resposta.py:129`)

   ```python
   if not p.resposta_colab:
       qs.update(respondida_em=I)

   if p.respondida_em:
       pass
   qs.update(resposta_colab='bati')
   ```

   Essas formas podem escrever meios-pares por `update`, sem `save`; são contraexemplos do selo, não escritores encontrados na produção. (`app/chamados/tests/test_contract_par_resposta.py:35`; `app/chamados/tests/test_contract_par_resposta.py:78`)

5. **Médio — uma fixture muda o sujeito do teste.** No teste de disputa mista, as duas perguntas antes tinham texto sem instante; agora a do chamado resolvido nasce muda e a restante nasce respondida, sem veredito. Os asserts continuam verificando uma materialização e disputa aberta, mas a causa que segura a disputa mudou. O cenário legado anterior deveria permanecer explicitamente coberto, como foi feito em outras fixtures por `update`. (`app/chamados/tests/test_fio_disputa_resolvido.py:56`; `app/chamados/tests/test_fio_disputa_resolvido.py:79`; `app/chamados/tests/test_fio_disputa_resolvido.py:84`; `app/chamados/tests/test_e4b_c12_orm.py:60`)

6. **Menor — `JUIZES.md` ficou desatualizado.** O registro vivo contém o novo juiz, mas a tabela de consulta não o acompanha, apesar da instrução de corrigi-la no mesmo commit. (`app/core/juizes.py:306`; `app/docs/JUIZES.md:14`; `app/docs/JUIZES.md:59`)

7. **Menor — comentário atribui significado incorreto ao instante da resposta automática.** Diz “instante da foto”, mas o chamador fornece `timezone.now()` do processamento. Continua sendo um instante; a descrição da origem é que diverge. (`app/chamados/services/disputa_emissao.py:1671`; `app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:48`)

**Conferido e conforme — respostas às oito perguntas**

**1. Escritores, leitores e significado dos campos.**

| Campo | Quem escreve na produção |
|---|---|
| `resposta_colab` | Nascimento por `_nascer`/`save`; resposta web/API; automática; reabertura pelo executor de lote. A simulação da ressuscitação altera somente memória. (`app/chamados/models.py:2940`; `app/chamados/models.py:3118`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/models.py:1556`) |
| `respondida_em` | Os anteriores, mais `carimbar`, condicionado a texto, e `carimbar_resposta_pela_trilha`. `_desfazer_em_memoria` apenas restaura a instância. (`app/chamados/models.py:3231`; `app/chamados/models.py:3233`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/models.py:3264`) |
| `validada_em` | Nascimento; `_carimbar_validacao`; `carimbar(resposta='validar')`; resposta nova, devolução e reabertura apagam a validação. (`app/chamados/models.py:3115`; `app/chamados/services/validacao.py:159`; `app/chamados/models.py:3236`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/validacao.py:224`; `app/chamados/services/disputa_emissao.py:1609`) |
| `materializada_em` | Atribuidor `_escrever_carimbo`, usado por carimbar/identidade/descarimbar; executor de lote; nascimento genérico. (`app/chamados/models.py:3200`; `app/chamados/models.py:3284`; `app/chamados/models.py:3309`; `app/chamados/models.py:3384`; `app/chamados/models.py:3115`) |
| `via_resolucao` | Mesmo atribuidor e lote; também encerramento por fato no espelho e relavra da via. (`app/chamados/models.py:3202`; `app/chamados/models.py:3384`; `app/chamados/models.py:3393`; `app/chamados/services/relavra_via.py:125`) |
| Atos `DecisaoPergunta` | Escrita física por `registrar`/`bulk_create`. Recebe atos do nascimento, ressuscitação, fechamento, retratação, resposta, reabertura/recusa, automática, trilha, validação/devolução, absorção e silêncio administrativo. (`app/chamados/models.py:4115`; `app/chamados/models.py:3120`; `app/chamados/models.py:1612`; `app/chamados/models.py:1996`; `app/chamados/models.py:2128`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1699`; `app/chamados/services/validacao.py:164`; `app/chamados/services/validacao.py:230`; `app/chamados/services/materializacao.py:569`; `app/ponto/services/veredito_celula.py:195`) |

Os leitores encontrados se distribuem assim; os leitores que usam regra própria de resposta estão discriminados na resposta 3. (`app/chamados/juizes.py:1389`; `app/chamados/models.py:1914`; `app/chamados/models.py:3660`)

| Campo | Leitores de produção encontrados |
|---|---|
| Par instante/texto | Vitalidade e projeção temporal; SQL de vivas e fila; retenção; estado/property; resposta web/API; cobrança/fio; materialização; validação; auditorias; deduplicação; calendário/ficha/prévia/cartório; apresentação e histórico. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1423`; `app/chamados/models.py:3473`; `app/chamados/juizes.py:1739`; `app/chamados/models.py:1914`; `app/chamados/models.py:3146`; `app/chamados/models.py:3660`; `app/api/views.py:2620`; `app/chamados/services/fio.py:187`; `app/chamados/services/materializacao.py:1376`; `app/chamados/services/validacao.py:418`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`; `app/chamados/services/fio.py:1271`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/cartorio.py:1641`; `app/chamados/services/historico.py:99`) |
| `validada_em` | Retenção/estado/vitalidade; reconciliação; materializador; validação/recusa/lote; emissão; vigias; declaração; placar/painel/histórico; procedência; auditorias e comandos. (`app/chamados/models.py:1914`; `app/chamados/models.py:3142`; `app/chamados/models.py:3487`; `app/chamados/reconciliador.py:631`; `app/chamados/services/materializacao.py:1377`; `app/chamados/services/validacao.py:421`; `app/chamados/services/validacao.py:602`; `app/chamados/services/validacao.py:767`; `app/chamados/services/disputa_emissao.py:2062`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/passivo_validado.py:66`; `app/chamados/services/declaracao_texto.py:227`; `app/chamados/services/verbos.py:264`; `app/chamados/services/painel.py:67`; `app/chamados/services/historico.py:101`; `app/ponto/services/procedencia_batida.py:74`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1576`) |
| `materializada_em` | Ressuscitação; retenção/estado; portas de carimbo/reabertura; vitalidade/lastro; frescor da lavra; reconciliação; resposta; materialização/absorção; validação; emissão/fábrica; raio X; histórico/painel/foto; procedência; auditorias e comandos. (`app/chamados/models.py:1531`; `app/chamados/models.py:1914`; `app/chamados/models.py:3144`; `app/chamados/models.py:3283`; `app/chamados/models.py:3414`; `app/chamados/juizes.py:683`; `app/chamados/juizes.py:1390`; `app/chamados/veredito_lavrado.py:203`; `app/chamados/reconciliador.py:630`; `app/chamados/views.py:1257`; `app/chamados/services/materializacao.py:631`; `app/chamados/services/materializacao.py:1378`; `app/chamados/services/validacao.py:215`; `app/chamados/services/disputa_emissao.py:1930`; `app/chamados/services/fabrica_da_celula.py:171`; `app/chamados/services/raiox.py:135`; `app/chamados/services/historico.py:103`; `app/chamados/services/painel.py:67`; `app/chamados/services/foto_e4b.py:180`; `app/ponto/services/procedencia_batida.py:74`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1577`) |
| `via_resolucao` | Identidade/vitalidade/reaberturas; reconciliação; universo/costura/deduplicação do fio; emissão; absorção; passivo validado; fábrica; tarefa; explicador; competência trancada; relavra; flip/cartório; auditorias e apresentação. (`app/chamados/models.py:2820`; `app/chamados/models.py:3302`; `app/chamados/models.py:3438`; `app/chamados/models.py:3474`; `app/chamados/juizes.py:1390`; `app/chamados/reconciliador.py:1144`; `app/chamados/services/fio.py:1139`; `app/chamados/services/fio.py:1389`; `app/chamados/services/fio.py:1971`; `app/chamados/services/disputa_emissao.py:2072`; `app/chamados/services/materializacao.py:674`; `app/chamados/services/passivo_validado.py:141`; `app/chamados/services/fabrica_da_celula.py:149`; `app/chamados/services/tarefa_do_dia.py:245`; `app/chamados/services/explicador.py:95`; `app/chamados/services/competencia_trancada.py:276`; `app/chamados/services/relavra_via.py:118`; `app/ponto/services/flip_auto.py:129`; `app/ponto/services/cartorio.py:1650`; `app/templates/chamados/partials/_card_atendimento.html:192`) |
| Atos | Histórico anterior; correspondência projeção↔ato; prefetch do fio; última decisão da tarefa; captura de foto; comandos de preenchimento do histórico. (`app/chamados/models.py:3670`; `app/chamados/models.py:4089`; `app/chamados/services/fio.py:249`; `app/chamados/services/tarefa_do_dia.py:147`; `app/chamados/services/tarefa_do_dia.py:410`; `app/chamados/services/foto_e4b.py:280`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`) |

**Não, todos ainda não usam o mesmo significado de “respondida”.** O juiz usa **I E T após trim**; retenção conserva **I OU T**; property/estado conservam somente **I**. Essa divergência remanescente está declarada para B2. (`app/chamados/juizes.py:1552`; `app/chamados/models.py:1914`; `app/chamados/models.py:3660`; `app/chamados/models.py:3146`; `app/chamados/juizes.py:1550`)

Os campos temporais são instantes, comparados como instantes; a conversão para dia no placar é derivação de apresentação. `materializada_em` também acompanha encerramentos sem batida: a via qualifica o desfecho. Não encontrei introdução de significado “dia” nesses campos pelo diff. (`app/chamados/models.py:2544`; `app/chamados/models.py:2545`; `app/chamados/models.py:3521`; `app/chamados/juizes.py:1392`; `app/chamados/services/verbos.py:345`; `app/chamados/models.py:3269`)

**2. Escrita sem `save`.**

O bypass físico do par encontrado é o executor `descarimbar_em_lote`: ele usa `QuerySet.update`, mas valida o par fornecido ou combina a metade fornecida com a existente antes de escrever. Seus chamadores atuais de reabertura fornecem o par vazio completo. (`app/chamados/models.py:3361`; `app/chamados/models.py:3382`; `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1648`)

Não identifiquei outro `update`, `bulk_update` ou SQL cru de produção que escreva o par fora dessa porta. O `bulk_update` da lavra escreve apenas seus três campos próprios; a migration de dados identificada preenche identidade. Completude absoluta contra escrita dinâmica: **[não verificado]**. (`app/chamados/veredito_lavrado.py:237`; `app/chamados/veredito_lavrado.py:250`; `app/chamados/migrations/0051_decisao_pergunta.py:25`)

**3. Leitores com regra própria.**

- **B1:** resta o complemento equivalente em `espera_o_texto_do_colaborador`; I21 foi migrado com alteração de predicado, conforme achados 2 e 3. Os leitores alterados C6, S128 e raio X efetivamente chamam o juiz. (`app/chamados/services/fio.py:187`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`; `app/chamados/juizes.py:1423`; `app/chamados/services/disputa_emissao.py:2002`; `app/chamados/services/raiox.py:147`)
- **Retenção, B2:** `respostas_sem_veredito` conserva instante OU texto. (`app/chamados/models.py:1914`)
- **Somente instante, B2:** property/estado; web/API; cobrança; ações; becos; declaração; contagem da emissão; fio; seleção do materializador; app; validação e vigia. (`app/chamados/models.py:3660`; `app/chamados/models.py:3146`; `app/chamados/views.py:1280`; `app/chamados/views.py:1416`; `app/api/views.py:2577`; `app/api/views.py:2620`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176`; `app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/acoes_disputa.py:130`; `app/chamados/services/becos.py:30`; `app/chamados/services/declaracao_texto.py:687`; `app/chamados/services/disputa_emissao.py:1735`; `app/chamados/services/fio.py:698`; `app/chamados/services/materializacao.py:1376`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/validacao.py:418`; `app/chamados/services/vigia_fio.py:694`)
- **Templates e instrumento externo:** também conservam decisões pelo instante. (`app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`; `bin/censo_folha.py:28`; `bin/censo_folha.py:29`)
- **Somente texto:** reconciliação, escolha da canônica, dedup, prévia, ficha, calendário e cartório. A classificação nominal exata D versus E/F depende do censo não fornecido **[não verificado]**. (`app/chamados/reconciliador.py:365`; `app/chamados/reconciliador.py:1083`; `app/chamados/services/fio.py:1271`; `app/chamados/services/fio.py:1283`; `app/chamados/management/commands/dedup_perguntas_fato.py:75`; `app/folha/previa.py:142`; `app/colaboradores/services/ficha.py:171`; `app/colaboradores/services/calendario.py:474`; `app/ponto/services/cartorio.py:1641`)

Guardas de anuência, proteção contra sobrescrita, parsers e auditoria de canal também leem texto isoladamente; não devem ser automaticamente tratados como pergunta “está respondida?”. As classes E/F foram excluídas da migração pelo aval. (`app/chamados/services/materializacao.py:835`; `app/chamados/services/disputa_emissao.py:1663`; `app/chamados/templatetags/chamados_tags.py:142`; `app/chamados/canal.py:65`; `construir_r3_c17_e_f1_nova.txt:1`, anexo)

A correção das vivas 38251/38252 pertence ao B2 pelo aval; seu estado atual não aparece no dado recebido **[não verificado]**. (`construir_r3_c17_e_f1_nova.txt:1`, anexo)

**4. Escritores legítimos passam a levantar exceção?**

Não encontrei caminho legítimo atual que passe a produzir meio-par pela mudança:

| Caminho | Conferência |
|---|---|
| Web/API/app | Normaliza e rejeita texto vazio; grava instante e texto juntos. (`app/chamados/services/disputa_emissao.py:1446`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1522`) |
| Automática/foto | Texto constante, instante do processamento, recusa antes de alterar a instância e save com ambas as metades. (`app/api/foto_recebida.py:25`; `app/api/foto_recebida.py:42`; `app/chamados/services/disputa_emissao.py:1672`; `app/chamados/services/disputa_emissao.py:1677`) |
| Materializador/carimbar | Só acrescenta instante quando há texto após trim; inclui o campo no save. (`app/chamados/services/materializacao.py:1283`; `app/chamados/models.py:3231`; `app/chamados/models.py:3234`; `app/chamados/models.py:3253`) |
| Carimbar pela trilha | Exige texto e instante ausente; acrescenta somente o instante, legalizando o legado. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1696`) |
| Validar/devolver | Alteram validação/autoria; `update_fields` sem o par não aciona sua recusa. (`app/chamados/services/validacao.py:162`; `app/chamados/services/validacao.py:227`; `app/chamados/models.py:3579`) |
| Ressuscitar/reabrir para correção | Envia as duas metades vazias ao executor. (`app/chamados/models.py:1622`; `app/chamados/services/disputa_emissao.py:1609`) |
| Migration de dados identificada | Preenche identidade, sem escrever o par. (`app/chamados/migrations/0051_decisao_pergunta.py:25`) |

Uso de admin genérico, integrações externas ou execução real desses caminhos no estoque: **[não verificado]**.

**5. Os três `reabrir_*`.**

O diff preserva a resposta nos três. `reabrir_fantasma` e `reabrir_por_competencia` descarimbam imediatamente; `reabrir_por_resposta` prepara o descarimbo em memória para o escritor salvar junto. Todos zeram materialização/autoria/via, sem zerar instante ou texto da resposta. Portanto não fabricam novo meio-par; um meio-par legado pode permanecer. Isso corresponde à exceção expressa do aval. (`app/chamados/models.py:3416`; `app/chamados/models.py:3433`; `app/chamados/models.py:3440`; `app/chamados/models.py:3309`; `construir_r3_c17_e_f1_nova.txt:1`, anexo)

**6. Classes de reprovação.**

| Classe | Veredito |
|---|---|
| Segundo juiz registrado | O novo juiz está registrado; os dois predicados de ilegalidade têm pergunta distinta. Resta a derivação paralela apontada no achado 3. (`app/core/juizes.py:306`; `app/chamados/juizes.py:1531`; `app/chamados/juizes.py:1537`; `app/chamados/services/fio.py:187`) |
| Segundo escritor fora da porta | A ressuscitação passa a delegar ao escritor existente. O furo encontrado é na garantia do `save`, não um novo escritor identificado. (`app/chamados/models.py:1622`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:3578`) |
| Reló

[truncado em 20000 de 23637 caracteres]

## 15/11 22:28 · sessao a9b4abcfaf19 · - · - · origem=Ronald (tmux)

Achados confirmados:

- **Médio — classe A ainda sem juiz:** `espera_o_texto_do_colaborador` conserva `respondida_em is None or respondida_sem_texto(p)`, complemento exato de “instante E texto após trim”; deveria apenas perguntar ao novo juiz. Afeta cobrança e cartão por delegação. `app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:186`, `app/chamados/services/fio.py:174`.
- **Médio — B1 muda predicado de I21:** o diff substitui `resposta_colab != ''` por trim. Instante com resposta somente espaços deixava entrar e agora fica fora; portanto a afirmação “nenhum leitor muda de predicado” não vale literalmente. Existência desses registros em produção: **[não verificado]**. `app/chamados/management/commands/auditar_invariantes_chamados.py:1572`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`.
- **Menor — lookup desatualizado:** `JUIZES.md` exige acompanhar o código no mesmo commit, mas sua tabela de chamados não contém `pergunta_respondida`. O registro vivo contém. `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:59`, `app/core/juizes.py:306`.

Conferido:

- Juiz Python usa instante presente E texto após `strip`; face SQL usa o conjunto de espaços do próprio Python. `app/chamados/juizes.py:1526`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1560`, `app/chamados/juizes.py:1580`.
- Não há significado único ainda em todos os consumidores: retenção conserva **instante OU texto**, property conserva **instante apenas**, conforme migração B2 declarada. `app/chamados/models.py:1914`, `app/chamados/models.py:3660`, `app/chamados/juizes.py:1550`.
- Campos temporais são instantes (`DateTimeField`), com comparações temporais no C6. Conversão para dia em placar é derivação de apresentação, sem regravação. `app/chamados/models.py:2544`, `app/chamados/models.py:2545`, `app/chamados/models.py:3521`, `app/chamados/juizes.py:1392`, `app/chamados/services/verbos.py:345`.
- Classificador existente continua encontrando **21 sítios** de carimbo cru; são a dívida B2. Também existe instrumento fora de `app/`: `bin/censo_folha.py:28`, `bin/censo_folha.py:29`.
- Atos já são lidos pela tarefa: nova REABERTURA passa a produzir `RESPONDER`; não basta equivalência do juiz para demonstrar zero efeito sobre fila após futuras ressuscitações. Prova sombra não consta do dado recebido. `app/chamados/services/tarefa_do_dia.py:148`, `app/chamados/services/tarefa_do_dia.py:152`, `app/chamados/models.py:1621`.

Inventários completos de leitores dos três outros campos, atos e listas de regras próprias foram enviados ao agente principal por mensagens, com funções e linhas.

## 15/11 22:27 · sessao 3e2bfb3b1490 · - · - · origem=Ronald (tmux)

ACHADOS:

- **Médio — contrato estático enfraquece a garantia pela “guarda”.** Qualquer leitura da outra metade em qualquer `if` absolve a função, sem exigir condição correta, domínio do ramo ou mesma instância. Os mutantes `if not p.resposta_colab: qs.update(respondida_em=I)` e `if p.resposta_colab: pass; qs.update(respondida_em=I)` passam. Espaços também passam na guarda truthy. Prova em memória: detector real retornou `None` para os três; contrato atual ficou verde, 2 testes. Referências: `app/chamados/tests/test_contract_par_resposta.py:58`, `:78`, `:129`.

- **Médio — save valida memória, não o par efetivamente persistido por `update_fields`.** Carregado `(None, '')`, alterar memória para `(I, 'bati')` e salvar só `resposta_colab` passa, gravando `(None, 'bati')`; salvar só `respondida_em` passa, gravando `(I, '')`. Provei usando AST dos corpos reais, sem banco ou arquivos. Falha preexistente para uma direção, ampliada para texto sem instante; chamador concreto de produção com essa sequência **[não verificado]**. Referências: `app/chamados/models.py:3548`, `:3552`, `:3578`. Teste novo não cobre discrepância entre memória e campos persistidos: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:108`.

- **Médio — fixture muda o sujeito no teste de disputa mista.** Antes eram duas perguntas com texto sem instante; agora uma nasce muda e a restante nasce respondida, sem veredito. Os asserts permanecem sobre quantidade materializada e disputa aberta, cobrindo uma causa diferente de retenção. Referências: `app/chamados/tests/test_fio_disputa_resolvido.py:56`, `:79`, `:84`, `:87`. O próprio comentário documenta a mudança: `:81`.

- **Menor — lookup JUIZES não acompanha juiz novo.** O arquivo exige atualização no mesmo commit, mas sua tabela CHAMADO continua sem `pergunta_respondida`; o registro vivo inclui o novo juiz. Referências: `app/docs/JUIZES.md:14`, `:59`, `app/core/juizes.py:306`.

CONFORME:

- **O teste novo morde mutantes do juiz.** Executei apenas a tabela verdade extraída, em memória: real = 0 falhas; só instante = 3; só texto = 1; sem trim = 1; constante falsa = 2. Portanto não procede acusar ausência de mordida só porque não existe runner explícito de mutação no arquivo. Referência: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:26`.

- Ressuscitação passa pelo escritor existente; conserva ordem inicial das travas e registra REABERTURA antes do lote. Referências: `app/chamados/models.py:1611`, `:1622`, `app/chamados/services/disputa_emissao.py:1631`, `:1648`.

- O teste de ressuscitação confere instante original, validação original, texto e motivo no ato; também confere idempotência. Referências: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:214`, `:226`, `:230`. Não acusaria cegueira por remover o reset de texto nesse cenário: o universo seleciona texto `''`/NULL, portanto o mutante seria equivalente nessa fixture. Referência: `app/chamados/models.py:1533`.

- Frase exigida para nascimento do juiz e registro foram incluídos. Referências: `CLAUDE.md:74`, `app/docs/CORTES.md:460`, `app/core/juizes.py:306`.

Prova de sombra B1 com zero mudanças e execução da suíte de banco no hash: **[não verificado]**; `logs/` não está disponível nesta cópia. [nao verificado]

## 15/11 22:27 · sessao a5ce278cd6e7 · - · - · origem=Ronald (tmux)

Auditoria dos escritores, somente leitura:

- **Não identifiquei escritor concreto de produção que passe a falhar pela nova recusa.** Resposta web/API normaliza e recusa vazio antes de atribuir o par completo (`app/chamados/services/disputa_emissao.py:1446`, `:1481`, `:1522`); automática recusa o par antes da escrita (`:1672`, `:1677`), com texto constante e instante não nulo do chamador (`app/api/foto_recebida.py:25`, `:42`, `:48`).
- **Materializador permanece compatível:** chama `carimbar(..., resposta='validar')` (`app/chamados/services/materializacao.py:1283`); essa porta só acrescenta `respondida_em` quando há texto (`app/chamados/models.py:3231`), e inclui o campo no save (`:3234`, `:3253`). Absorção segue a mesma porta (`app/chamados/services/materializacao.py:567`).
- **Carimbo pela trilha continua legal:** exige texto e ausência de instante (`app/chamados/services/disputa_emissao.py:1692`), atribui instante e salva essa metade (`:1696`, `:1698`).
- **Validação/devolução não alteram o par:** salvam somente validação/autoria (`app/chamados/services/validacao.py:159`, `:162`, `:224`, `:227`); a recusa retorna imediatamente quando `update_fields` não toca o par (`app/chamados/models.py:3579`).
- **Ressuscitação delega à reabertura única** (`app/chamados/models.py:1622`); esta prepara instante/texto vazios (`app/chamados/services/disputa_emissao.py:1609`), registra a foto anterior (`:1631`) e chama o executor (`:1648`). A simulação também zera os dois (`app/chamados/models.py:1555`).
- **Reabrir fantasma, competência e resposta preservam o par:** delegam a descarimbar (`app/chamados/models.py:3416`, `:3440`, `:3433`), que zera materialização, autoria e via (`:3309`), sem mexer na resposta.
- **Bypass físico encontrado:** `PerguntaDisputa.descarimbar_em_lote` faz `QuerySet.update` (`app/chamados/models.py:3382`), mas confere as duas metades fornecidas, ou combina a metade fornecida com a existente, antes (`:3361`). Nenhum outro bypass do par foi identificado na varredura; completude absoluta [não verificado].
- **Migrations:** as de dados identificadas para PD só preenchem identidade (`app/chamados/migrations/0051_decisao_pergunta.py:25`); não alteram o par. Aplicação real de migrations [não verificado].

**Furo de contrato herdado, confirmado por leitura:** `save(update_fields=...)` valida o par inteiro em memória (`app/chamados/models.py:3581`), mas não monta o par efetivamente persistido. Uma instância pendente com ambas as metades atribuídas pode passar na recusa e salvar só texto, deixando o banco ilegal. A linha carregada também recebe as duas metades em memória após um save parcial (`:3553`). Não identifiquei chamador atual que realize essa combinação; incidência em produção [não verificado].

**Escritores dos campos:**

| Campo | Escritores |
|---|---|
| `resposta_colab` | resposta core `app/chamados/services/disputa_emissao.py:1481`; automática `:1674`; reabertura via lote `:1609`, `:1648`; nascimento `app/chamados/models.py:2939`; save genérico `:3541` |
| `respondida_em` | os anteriores; carimbo condicionado a texto `app/chamados/models.py:3233`; trilha `app/chamados/services/disputa_emissao.py:1696` |
| `validada_em` | validação `app/chamados/services/validacao.py:159`; devolução `:224`; resposta nova `app/chamados/services/disputa_emissao.py:1494`; reabertura `:1609`; carimbo condicionado `app/chamados/models.py:3236`; nascimento/save genérico |
| `materializada_em` | atribuidor único em memória `app/chamados/models.py:3200`, usado por carimbar/identidade/descarimbar; lote `:3384`; nascimento/save genérico |
| `via_resolucao` | atribuidor `app/chamados/models.py:3202`; encerrar por fato `:3393`; relavra `app/chamados/services/relavra_via.py:125`; lote `app/chamados/models.py:3384`; nascimento/save genérico |

**Atos `DecisaoPergunta`:** escrita física única em `app/chamados/models.py:4115`; chamadores em nascimento (`:3120`), ressuscitação (`:1612`), fechamento (`:1996`), retratação (`:2128`), resposta/reabertura/automática/trilha (`app/chamados/services/disputa_emissao.py:1525`, `:1631`, `:1678`, `:1699`), validação/devolução (`app/chamados/services/validacao.py:164`, `:230`), absorção (`app/chamados/services/materializacao.py:569`) e silêncio administrativo (`app/ponto/services/veredito_celula.py:195`). Existem ainda comandos antigos de preenchimento (`app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`), sem alteração no diff anexado.

**Nota documental menor:** comentário novo diz que a resposta automática recebe “instante da foto” (`app/chamados/services/disputa_emissao.py:1671`), mas o chamador passa relógio do processamento (`app/api/foto_recebida.py:42`, `:48`).

**Leitor fora de app:** `bin/censo_folha.py:28`/`:29` ainda calcula pendência e resposta somente pelo instante; migração/classe no censo ausente [não verificado].

## 15/11 22:24 · sessao 24507f4b97fb · E3-RESTO-R3 · construtor · origem=Code

Cura e provas preparadas; execução de RED/GREEN e morte dos mutantes: **[nao verificado]**.
`T` = [chamados/tests/test_e3_r3_c17_sem_banco.py::Contrato](/home/ronald/wt-codex-e3-resto-r3-221106/app/chamados/tests/test_e3_r3_c17_sem_banco.py:96). Instrumento e mutantes: [e3_r3_c17_mutantes.py](/home/ronald/wt-codex-e3-resto-r3-221106/app/chamados/tests/e3_r3_c17_mutantes.py:27).

Comandos para a sessão, dentro de `app/` ([instrumento:97](/home/ronald/wt-codex-e3-resto-r3-221106/app/chamados/tests/e3_r3_c17_mutantes.py:97)):

```bash
R(){ PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_r3_c17_mutantes --red-base "$@"; }
G(){ PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_r3_c17_mutantes --green "$@"; }
M(){ PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_r3_c17_mutantes "$@"; }
```

| Item | Teste RED — T::teste | Cura — arquivo::função | Mutantes | RED / GREEN / mutantes | [nao verificado]
|---|---|---|---|---|
| 1 | `test_01_visiveis_intersecao_e_ids_efetivos_na_C5` | [folha/vigia_retido_calado.py::censo](/home/ronald/wt-codex-e3-resto-r3-221106/app/folha/vigia_retido_calado.py:41) | `sem_intersecao`, `C5_recebe_recorte_bruto` | `R 01` / `G 01` / `M 01` |
| 2 | `test_02_sem_foto_nomeia_todos_os_pares_e_saida` | [folha/vigia_retido_calado.py::censo](/home/ronald/wt-codex-e3-resto-r3-221106/app/folha/vigia_retido_calado.py:61) | `empresa_sem_foto_some` | `R 02` / `G 02` / `M 02` |
| 3 | `test_03_command_real_alarma_parcial_sem_passivo_e_preserva_positivo` | [folha/management/commands/vigiar_retido_calado.py::Command.handle](/home/ronald/wt-codex-e3-resto-r3-221106/app/folha/management/commands/vigiar_retido_calado.py:37) | `parcial_nao_alarma` | `R 03` / `G 03` / `M 03` |
| 4 | `test_04_janela_por_empresa_meia_noite_relavra_e_velha_fora` | [folha/vigia_retido_calado.py::censo — preservada](/home/ronald/wt-codex-e3-resto-r3-221106/app/folha/vigia_retido_calado.py:65) | `inclui_velha`, `dia_civil`, `relavra_apaga_outra`, `corte_unico` | `R 04` / `G 04` / `M 04` |
| 5 | `test_05_unidades_legado_none_positivo_e_ordem_dos_trios` | [folha/vigia_retido_calado.py::censo — preservada e documentada](/home/ronald/wt-codex-e3-resto-r3-221106/app/folha/vigia_retido_calado.py:44) | `perde_pessoa_competencia`, `legado_apaga_positivo`, `trio_invertido` | `R 05` / `G 05` / `M 05` |
| 6 | `test_06_limbo_recebe_dia_da_borda_perto_da_meia_noite` | [relatorios/furos.py::escalas_em_limbo](/home/ronald/wt-codex-e3-resto-r3-221106/app/relatorios/furos.py:51), [relatorios/views.py::furos_espelho](/home/ronald/wt-codex-e3-resto-r3-221106/app/relatorios/views.py:1554) | `limbo_relogio_proprio`, `view_omite_dia` | `R 06` / `G 06` / `M 06` |
| 7 | `test_07_dataset_exige_instante_e_transporta_identico_ao_D4_e_calendario` | [relatorios/furos.py::dataset_furos](/home/ronald/wt-codex-e3-resto-r3-221106/app/relatorios/furos.py:103) | `agora_opcional`, `aceita_none`, `D4_sem_instante`, `calendario_sem_instante` | `R 07` / `G 07` / `M 07` |
| 8 | `test_08_selo_conta_chamadas_soltas_repetidas_apelidos_e_conversoes` | [chamados/tests/test_e3_r3_c15_sem_banco.py::_chamadas_relogio, _le_relogio, _instante_da_borda](/home/ronald/wt-codex-e3-resto-r3-221106/app/chamados/tests/test_e3_r3_c15_sem_banco.py:166) | `nomes_distintos`, `ignora_chamada_solta`, `ignora_alias`, `conversao_conta`, `alvos_contam` | `R 08` / `G 08` / `M 08` |
| 9 | `test_09_docstrings_G3_nomeiam_contratos_e_exclusoes_sem_fecho_transitivo` | [folha/retrato_folha.py::competencias_do_retrato](/home/ronald/wt-codex-e3-resto-r3-221106/app/folha/retrato_folha.py:1369), [folha/views_retrato.py::_recorte_da_barra](/home/ronald/wt-codex-e3-resto-r3-221106/app/folha/views_retrato.py:145), `censo` e `dataset_furos` acima | `G3_omite_exclusoes` | `R 09` / `G 09` / `M 09` |

Arquivos tocados: os nove arquivos identificados acima, mais `chamados/tests/test_e3_r3_c4_sem_banco.py:135`, `test_e3_r3_c5_sem_banco.py:87`, `test_e3_r3_c6_sem_banco.py:231`, `test_e3_r3_c11_sem_banco.py:275`, `test_e3_r3_c14_sem_banco.py:329`, `test_e3_r3_c16_sem_banco.py:63`, `e3_r3_c16_mutantes.py:28`; `folha/tests/test_vigia_retido_calado.py:65`; `relatorios/tests/test_e4_8_furos_le_a_lavra.py:58`, `relatorios/tests/test_selo_ancora_do_vinculo.py:23`.

Escritor/leitores de `empresas/ids`: command `handle` (`folha/management/commands/vigiar_retido_calado.py:31`) → interseção em `folha/vigia_retido_calado.py:54` → consulta de retratos `:59` e C5 `:72`; C5 lê IDs em `escala/services/cadastro_realidade.py:163`.
Escritor/leitores de `competencias_sem_retrato`: `folha/vigia_retido_calado.py:69` → formatador `:108` e alarme em `folha/management/commands/vigiar_retido_calado.py:38`; envelope de nove chaves descrito em `folha/vigia_retido_calado.py:44`.
Escritor/leitor de `hoje`: `relatorios/views.py:1520` → chamada `:1554` → `relatorios/furos.py:65` e filtro mensal `:88`.
Escritor/leitores de `agora`: `relatorios/views.py:1519` → chamada `:1543` → validação `relatorios/furos.py:113`, D4 `:127` e calendário `:168`.
Escritor/leitor de `_imports_relogio`: `chamados/tests/test_e3_r3_c15_sem_banco.py:162` → `:172`; contagem por chamada em `:193`, consumida pelos selos em `:242` e `chamados/tests/test_e3_r3_c16_sem_banco.py:152`.

Pergunta aberta: `gerar_pdf_auditavel` e `placar_aceite` recebem exceção nomeada em RELOGIO-FORA-DA-BORDA ou permanecem sem linha? Exclusão documentada em `folha/views_retrato.py:145`.
Pergunta aberta: quem ajustará a assinatura antiga registrada em `core/juizes.py:794`, diante de `relatorios/furos.py:51`? Impacto na suite: **[nao verificado]**.

## 15/11 22:20 · sessao efe4fe89bdcb · RETOMA-HE-LOTE3 · sonda · origem=Code

Referências `saída:Lx`: contando `sonda_leitura: ...` como L1, a linha vazia como L2 e `AGORA ...` como L3.

1. **0 linhas de ExportacaoDominio** para 2026 com mês ≥ 10. — `saída:L4`: `ED 2026 mes>=10 linhas 0`.

2. **K6 por empresa e competência:**

   | Empresa | 10/2026 | 11/2026 | Fonte |
   |---|---|---|---|
   | 2 | False | False | `saída:L5` |
   | 3 | False | False | `saída:L6` |
   | 4 | False | False | `saída:L7` |

3. **58 DecisaoHE não vigentes**, com **data mínima 2026-09-22**. — `saída:L8`: `NAO_VIGENTES 58 data_min 2026-09-22`.

4. **Nas seis combinações empresa/competência consultadas, nenhuma decisão pode coincidir com K6 True**, pois todos os seis resultados são False. — `saída:L5–L7`.

   **Para o conjunto completo das 58 decisões, a existência de alguma em competência com K6 True está [não verificado].** A contagem e a data mínima não fornecem o cruzamento individual por empresa/competência nem o K6 de setembro/2026. — `saída:L5–L8`.

## 15/11 22:19 · sessao 26ef99325962 · JORNADA-FORA-DA-ESCALA · leitura · origem=Code

**Hoje, “formou turno” e “está em turno agora” têm respostas diferentes.** `parear_turnos` forma o turno; `turno_aberto_de` consulta essa geometria e aplica a expiração; `proximo_tipo_de` sugere E/S a partir do turno **vivo**. Por isso, uma E pode formar um turno aberto e, imediatamente, receber sugestão de outra E. (`app/ponto/turnos.py:1417`, `app/ponto/turnos.py:2278`, `app/ponto/turnos.py:2283`, `app/ponto/turnos.py:2474`)

Abaixo, os resultados para E avulsa pressupõem ausência de carimbo de intervalo, papel substituto da ata ou outras batidas que mudem o pareamento. Esses tratamentos antecedem o ramo normal de E. (`app/ponto/turnos.py:1310`, `app/ponto/turnos.py:1326`, `app/ponto/turnos.py:1382`, `app/ponto/turnos.py:1398`) O histórico completo e o estado persistido do caso nominal não foram consultados em banco. **[não verificado]**

**1. Quem responde em cada caso**

| Caso | Formação do turno | Presença e próximo tipo |
|---|---|---|
| **(a) E gravada depois da expiração da aberta do próprio dia** | `parear_turnos` conserva a anterior aberta, caso exista, e inicia outra com a nova E. Não consulta a expiração nesse ramo. (`app/ponto/turnos.py:1417–1420`) | `expiracao_da_aberta` devolve **fim previsto da `data_turno` + 4h**. `turno_aberto_vivo` devolve `None` quando `agora > expiração`; a nova entrada não recebe um prazo contado da sua chegada. `proximo_tipo_de` então devolve **`'E'`**. (`app/ponto/turnos.py:2009–2030`, `app/ponto/turnos.py:2474–2476`) |
| **(b) E em FOLGA da escala/célula** | O ramo normal de E também abre turno: não consulta `trabalha`. A folga continua sendo folga para o previsto. (`app/ponto/turnos.py:1402–1420`, `app/escala/models.py:1094–1096`) | Folga não veta diretamente o turno vivo. A expiração consulta os marcos do **tipo**, que não decide trabalho/folga. Assim, numa folga com tipo 08–17, a E pode estar viva até 21h e sugerir **S**; depois disso, retorna `None` e sugere **E**. Sem tipo ou fim previsto, essa expiração devolve `None`. (`app/ponto/turnos.py:2024–2030`, `app/ponto/selecao_periodo.py:151–169`, `app/escala/models.py:326–345`, `app/ponto/turnos.py:2474–2478`) |
| **(c) E fora da janela num dia de trabalho** | Não existe veto horário no ramo normal de abertura: a E abre turno geométrico. (`app/ponto/turnos.py:1402–1420`) | “Fora da janela” sozinho não determina a sugestão. No exemplo **08–17 / E 21:36**, o limite é 21h: turno vivo **`None`**, próximo tipo **E**. Uma E depois das 17h, mas ainda antes da expiração, pode permanecer viva e sugerir **S**. (`app/ponto/turnos.py:2010–2030`, `app/ponto/turnos.py:2474–2478`) |

A comparação de expiração é estritamente `>`: no instante exato do limite ela ainda não elimina a aberta. A função apenas deixa de devolvê-la como viva; não fecha nem apaga o turno formado. (`app/ponto/turnos.py:2006–2013`)

**2. O que os leitores mostram**

| Leitor | (a) Após expiração / (c) exemplo 21:36 | (b) Folga |
|---|---|---|
| **Espelho do colaborador e espelho do admin** | Usam a mesma fonte. O aberto geométrico alimenta `realizado_aberto`; a tela mostra **“EM ABERTO”**, eventual frase dos marcos faltantes e “parcial provado” quando houver minutos. A expiração da presença não elimina esse badge. (`app/ponto/views.py:117–125`, `app/ponto/views.py:160–161`, `app/ponto/services/espelho.py:783–794`, `app/templates/ponto/espelho.html:187`) | A mesma geometria pode produzir o badge aberto. Na grade de folga não há marcos de trabalho; as batidas sobrantes são conservadas como órfãs, exibidas pela tela. (`app/escala/utils.py:1801–1803`, `app/escala/utils.py:1814–1834`, `app/templates/ponto/espelho.html:240–248`) |
| **Calendário do admin** | Consulta `dias_encerrados`: enquanto houver aberta viva, mostra aberto; depois da expiração pode mostrar **o status lavrado**, ou **`sem_certidao`** se houver batida sem lavra. Portanto, não é garantido que continue dizendo “aberto” como o badge geométrico do espelho. (`app/ponto/status_dia.py:273–294`) | Depois da precedência de andamento, uma folga lavrada com batida recebe **`folga_trabalhada`**. Esse status não prova que existam horas pagas. (`app/ponto/status_dia.py:157–158`, `app/ponto/status_dia.py:280–289`, `app/colaboradores/services/calendario.py:651–665`) |
| **Pendências do admin** | A aba lê disputas e perguntas existentes; não decide novamente se a E abre turno. Distribui entre validar, materializar, cobrar colaborador e disputa sem pergunta. A ocorrência concreta depende da emissão e do estado dessas entidades. (`app/colaboradores/views.py:2998–3013`, `app/chamados/relatorios_disputa.py:11–28`) | A disputa imediata da API e a cobrança posterior por marcos seguem caminhos distintos, detalhados abaixo. (`app/api/views.py:702–770`, `app/chamados/services/fabrica_da_celula.py:110–113`) |
| **App — resposta do servidor** | `/me/` publica `turno_aberto=false`, `hora_entrada=null` e `proxima_batida='E'` quando o juiz não encontra aberta viva. Ao gravar, responde **“Entrada registrada às …”** com o tipo efetivo. (`app/api/views.py:306–309`, `app/api/views.py:354–356`, `app/api/views.py:803–823`) | Antes da expiração pode publicar aberta e sugerir S; depois, E, pela mesma autoridade. (`app/api/views.py:306–309`, `app/ponto/turnos.py:2474–2478`) |
| **PWA — sugestão e confirmação** | A triagem consulta o juiz; a V2 envia `tipo_sugerido`. A confirmação de **nova entrada com turno aberto** exige `context.turno_aberto`; após expiração esse ramo não é acionado. (`app/ponto/services/triagem_batida.py:198–204`, `app/templates/ponto/bater_ponto_v2.html:104`, `app/ponto/views.py:319–333`) | Se houver aberta viva e o usuário enviar E, esse ramo pode perguntar: **“Entrada às … sem saída. Registrar nova entrada mesmo assim?”** (`app/ponto/views.py:319–333`, `app/templates/ponto/triagem/turno_confirmar.html:6`) |

O espelho também mostra o registro cru nas últimas batidas e, havendo disputa, o aviso **“Em revisão pela supervisão”**, mantendo os cálculos visíveis. (`app/templates/ponto/espelho.html:13–21`, `app/templates/ponto/espelho.html:72–80`) A interface nativa efetivamente instalada, incluindo sua confirmação visual, não foi inspecionada; acima está o contrato do servidor. **[não verificado]**

**3. Fechamento, HE, autorização e Pauta**

- **Uma E sem S não vira HE só por estar fora da escala.** No comercial, `calcular_periodo(entrada, None)` marca `turno_aberto`, acrescenta “saída não registrada” e retorna **antes** da janela de HE e do cálculo da duração. Para duas E isoladas, o realizado é zero, mas com `sem_turno=False` e `aberto=True`: existem turnos, faltam saídas. (`app/ponto/motor_calculo_v2.py:3215–3226`, `app/ponto/turnos.py:719–739`, `app/ponto/turnos.py:771`)

- **Nos casos (a)/(c), em dia de trabalho**, batidas não casadas com realizado zero podem gerar `BATIDA_ORFA_FORA_TOLERANCIA`; marcos faltantes também podem gerar `FURO_PARCIAL`. Se a célula ficar acusadora e não alcançar uma isenção, o export a retém como pendência do espelho. Não é possível afirmar o motivo final do colaborador nominal sem sua célula e demais decisões. (`app/ponto/supra_juiz.py:450–463`, `app/ponto/supra_juiz.py:527–537`, `app/folha/export.py:1273–1294`, `app/folha/export.py:968–973`) **Estado nominal: [não verificado].**

- **Na folga**, havendo realizado **ou órfãs**, dia encerrado e previsto zero, o suprajuiz pode lavrar `FATO_SEM_PREVISAO`, que não está entre os vereditos acusadores. Isso não cria horas a partir da E avulsa. (`app/ponto/supra_juiz.py:392–393`, `app/ponto/supra_juiz.py:423–428`, `app/escala/models.py:1767–1768`, `app/ponto/motor_calculo_v2.py:3217–3220`)

- **Folga com par fechado é outra figura:** com janela empresarial aplicável, previsto zero e sem autorização, `_dia_sem_previsto_fora_da_janela` pode bloquear todos os minutos e publicá-los como pendência na Gestão de HE. Existe exceção para folga sem escala certa. O espelho mostra essa pendência como “dia sem jornada prevista”. (`app/ponto/motor_calculo_v2.py:1440–1488`, `app/ponto/services/espelho.py:743–749`, `app/templates/ponto/espelho.html:189`)

- **A aprovação tem um crivo adicional:** fechamento com `turnos_abertos > 0` recebe a trava `turno_aberto`, independentemente de a consulta de presença já ter expirado aquela entrada. (`app/ponto/services/fechamento.py:1005`, `app/folha/export.py:819–834`)

- **Não há Pauta DP automática demonstrada apenas por E 21:36.** Na porta de autorização de HE, a Pauta exige competência exportada e autorização com efeito. A existência de outra Pauta para o caso nominal não foi verificada. (`app/ponto/services/autorizacao_he_periodo.py:290–302`) **[não verificado]**

A L-084 exige **as duas pontas** a mais de três horas dos marcos para tratar o turno inteiro como Cadastro × Realidade. A E 21:36 isolada não basta para aplicar essa figura; a L-097, por sua vez, disciplina os minutos fora dos marcos e sua autorização. (`app/docs/LEIS.md:115–116`)

**4. Quais perguntas chegam ao colaborador**

- **(a), e (c) no exemplo 5x2:** não há pergunta específica “por que entrou depois da expiração?”. O PWA pula o controle de janela para `5x2`, `6x1` e `personalizado`; a API pula o detector horário quando `tipo_base='comercial'`, embora continue verificando folga antes. Ter ciclo 5x2 e ter tipo-base comercial são condições diferentes no código. (`app/ponto/services/triagem_batida.py:403–407`, `app/chamados/detector_anomalias.py:170–189`)

- **Depois, pela célula**, podem surgir perguntas dos **marcos previstos apagados e vencidos**: “Confirme o horário da entrada no dia…” e “Confirme o horário da saída no dia…”, além das de intervalo. Portanto, o sistema pode perguntar pelos marcos 08h/17h mesmo existindo a batida 21:36. A emissão depende de ativo, veredito acusador, vencimento, demais guardas e deduplicação; essa fábrica silencia o push. (`app/chamados/services/fabrica_da_celula.py:52–74`, `app/chamados/services/fabrica_da_celula.py:105–157`, `app/chamados/services/fabrica_da_celula.py:217–219`, `app/chamados/services/declaracao_texto.py:42–46`, `app/chamados/catalogo/perguntas.py:48–61`)

- **(b), folga pela API:** a batida é gravada; depois o detector retorna `grave`, contexto `batida_em_folga`, e a API encaminha `batida_fora_escala_grave` à disputa. O texto catalogado é **“Confirme o posto e o horário da batida”**, opções **“Confirma” / “Está diferente”**. É pergunta posterior ao registro, não autorização prévia de HE. (`app/api/views.py:654`, `app/chamados/detector_anomalias.py:170–177`, `app/api/views.py:702–707`, `app/api/views.py:758–770`, `app/chamados/services/disputa_emissao.py:683–696`, `app/chamados/catalogo/perguntas.py:82–85`)

- **Folga pelo PWA 5x2:** aquele controle de janela é pulado; a fábrica posterior por célula também não pergunta entrada/saída de dia sem trabalho. Assim, não há equivalência entre os dois canais nesse tratamento imediato. (`app/ponto/services/triagem_batida.py:403–407`, `app/chamados/services/fabrica_da_celula.py:110–113`, `app/chamados/juizes.py:444–459`)

- **Fora da janela em outros tipos:** o PWA pode registrar para revisão e oferecer “Motivo — por que está fora do horário?”. Na API, uma batida sem marco correspondente pode retornar apenas `informativa`; o chamador mostrado trata `grave` e `leve`, sem abrir essa pergunta pela informação. (`app/ponto/services/triagem_batida.py:428–496`, `app/ponto/services/triagem_batida.py:685–710`, `app/templates/ponto/triagem/fora_horario.html:26`, `app/chamados/detector_anomalias.py:212–224`, `app/api/views.py:705–741`)

**5. Segunda entrada: E 21:36 / E 21:49, escala 08–17**

Para a sequência isolada informada, a cadeia produz:

1. Às **21:36**, abre turno geométrico, mas a consulta de vivo já o considera expirado pelo limite das 21h; sugere **E** novamente. (`app/ponto/turnos.py:1419`, `app/ponto/turnos.py:2010–2030`, `app/ponto/turnos.py:2474–2476`)
2. Às **21:49**, a API ao vivo também escolhe E pelo juiz, mesmo se o cliente enviar outro tipo. Os 13 minutos ultrapassam a deduplicação de cinco minutos. (`app/api/views.py:529–531`, `app/api/views.py:569–577`)
3. O pareador conserva **dois turnos abertos**, sem formar um par de 13 minutos. A grade não elimina a segunda pelo dedupe de menos de dez minutos; o eco de flush exige distância dos eventos de pelo menos trinta minutos. (`app/ponto/turnos.py:1417–1420`, `app/ponto/turnos.py:1497–1498`, `app/escala/utils.py:1324–1339`, `app/ponto/turnos.py:366–372`)
4. O realizado dessas duas entradas isoladas é **zero, com turno existente e aberto**; o próximo tipo continua E depois delas. (`app/ponto/turnos.py:733–739`, `app/ponto/turnos.py:771`, `app/ponto/turnos.py:2474–2476`)

Antes da expiração, inclusive numa folga, a situação é diferente: uma primeira E viva normalmente faz o juiz sugerir S, e a API ao vivo pode converter a segunda tentativa em S. Se **duas E realmente persistirem**, o ramo normal conserva dois abertos; tentativas muito próximas ainda podem ser reutilizadas pela deduplicação. (`app/ponto/turnos.py:2474–2478`, `app/api/views.py:529–545`, `app/api/views.py:569–577`, `app/ponto/turnos.py:1417–1420`)

**Ata/cartório não corrigem automaticamente essa dupla E isolada.** O mapa de papel da ata ignora dia aberto ou sem entrada e saída reconhecidas; a reconciliação pela célula pula turnos abertos. O cartório pode lavrar novamente, mas essas guardas continuam presentes. (`app/ponto/turnos.py:1682–1683`, `app/escala/utils.py:1945–1948`, `app/ponto/signals.py:79–93`, `app/ponto/services/cartorio.py:730–762`)

Há correções posicionais para outras figuras, especialmente depois de pausa fechada; não se aplicam automaticamente a duas E isoladas. A própria L-115 preserva a última E solta sem pausa fechada anterior. (`app/ponto/turnos.py:1004–1007`, `app/docs/LEIS.md:153`) Se o dia real tiver batidas anteriores, o resultado completo precisa desse histórico. **[não verificado]**

**perguntas de lei: 6**

Não localizei decisão expressa em `LEIS.md`/`CORTES.md` para os seis recortes abaixo; a ausência de decisão é **[não verificado]**. As referências identificam o comportamento escolhido pelo código e as normas próximas.

1. **Entrada nova já expirada:** a E gravada após o limite do próprio dia deve nascer imediatamente sem turno vivo, apesar de abrir turno geométrico? Hoje o limite é absoluto, fim previsto + 4h. (`app/ponto/turnos.py:2010–2030`, `app/ponto/turnos.py:1419`) **Decisão expressa: [não verificado].**
2. **Abertura na folga:** uma E na folga deve abrir turno e ter sua vitalidade limitada pelos horários do tipo de trabalho? Hoje isso acontece; o previsto continua folga. (`app/ponto/turnos.py:1402–1420`, `app/ponto/turnos.py:2027`, `app/escala/models.py:1094–1095`, `app/escala/models.py:326–345`) **Decisão expressa: [não verificado].**
3. **Abertura fora da janela em dia útil:** basta a E para abrir geometria, sem uma decisão específica sobre a jornada excepcional? Hoje o ramo E aceita; L-097 trata dinheiro e L-084 exige duas pontas. (`app/ponto/turnos.py:1417–1420`, `app/docs/LEIS.md:115–116`, `app/docs/CORTES.md:405`) **Decisão expressa: [não verificado].**
4. **Repetição depois da expiração:** a segunda E deve acumular outro aberto e continuar sugerindo E? Hoje é esse o resultado para 21:36/21:49 isoladas. A L-115 preserva E solta, mas não explicita esse ciclo de sugestão. (`app/ponto/turnos.py:1417–1420`, `app/ponto/turnos.py:2474–2476`, `app/docs/LEIS.md:153`) **Decisão expressa: [não verificado].**
5. **Fonte da expiração:** deve valer o template vivo ou o DNA congelado do dia? Hoje a geometria pode ler DNA, mas a expiração chama a janela sem `ec` e usa o tipo. (`app/ponto/turnos.py:2250–2257`, `app/ponto/turnos.py:2026–2027`, `app/ponto/selecao_periodo.py:151–169`) **Decisão expressa: [não verificado].**
6. **Diferença de canal:** a mesma folga/deslocamento deve produzir pergunta imediata pela API e não pelo PWA 5x2, com isenções diferentes por ciclo e tipo-base? Hoje os caminhos fazem essas escolhas. (`app/chamados/detector_anomalias.py:170–189`, `app/ponto/services/triagem_batida.py:403–407`, `app/api/views.py:702–770`) **Decisão expressa: [não verificado].**

## 15/11 22:16 · sessao 0b63094086b7 · - · - · origem=Ronald (tmux)

Rastreio app/PWA e perguntas concluído, somente leitura.

- **Juiz da próxima batida:** `proximo_tipo_de` consulta `_turno_aberto_calc`; sem turno vivo retorna `'E'`, aberto fora de intervalo retorna `'S'` (`app/ponto/turnos.py:2450`, `app/ponto/turnos.py:2474`). `_turno_aberto_puro` pareia com ata e marcos e passa ao `turno_aberto_vivo` (`app/ponto/turnos.py:2272`, `app/ponto/turnos.py:2278`). Expiração usa fim previsto +4h e `agora > expiração`, independentemente da hora da nova E (`app/ponto/turnos.py:2009`, `app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2026`, `app/ponto/turnos.py:2030`).
- **08–17, E21:36/E21:49:** ambas ocorrem após21h; portanto juiz vivo retorna `None` e próximo tipo continua `'E'` após cada entrada, por dedução desses ramos (`app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2030`, `app/ponto/turnos.py:2475`). API ao vivo substitui o tipo enviado pelo sugerido (`app/api/views.py:569`, `app/api/views.py:577`). Intervalo13min ultrapassa dedup5min (`app/api/views.py:529`, `app/api/views.py:531`).
- **Payload app:** `/me/` publica `turno_aberto`, `hora_entrada` e `proxima_batida` (`app/api/views.py:354`). Após gravação responde `Entrada registrada às ...`, tipo efetivamente gravado, sucesso e motivos de disputa (`app/api/views.py:803`, `app/api/views.py:813`). Interface nativa e seu texto exato de confirmação: **[não verificado]**.
- **PWA:** triagem consulta vivo e sugestão (`app/ponto/services/triagem_batida.py:198`). V2 usa `tipo_sugerido` (`app/templates/ponto/bater_ponto_v2.html:104`); PWA antigo usa somente `turno_aberto` para E/S (`app/templates/ponto/triagem/apto.html:88`). Nova E pede confirmação apenas se `context.turno_aberto` estiver presente (`app/ponto/views.py:319`). Após expiração não entra nesse ramo; a pergunta daquele ramo seria “Entrada às ... sem saída. Registrar nova entrada mesmo assim?” (`app/templates/ponto/triagem/turno_confirmar.html:6`).
- **5x2 fora do horário em dia útil:** PWA pula explicitamente controle de janela para5x2/6x1/personalizado (`app/ponto/services/triagem_batida.py:403`). API pula detector por horário quando `tipo_base == 'comercial'`, depois de verificar folga (`app/chamados/detector_anomalias.py:170`, `app/chamados/detector_anomalias.py:188`). Assim não nasce pergunta por desvio dessa causa; outros motivos dependem do cadastro e evidências da batida **[não verificado]**.
- **Rígida fora da janela:** PWA considera janela hoje/ontem com tolerância10min; cria chamado `batida_fora_horario`, mas registra (`app/ponto/services/triagem_batida.py:412`, `app/ponto/services/triagem_batida.py:428`, `app/ponto/services/triagem_batida.py:459`, `app/ponto/services/triagem_batida.py:487`). Marca `aprovada=False` (`app/ponto/services/triagem_batida.py:685`, `app/ponto/services/triagem_batida.py:710`); pergunta “Motivo — por que está fora do horário?” (`app/templates/ponto/triagem/fora_horario.html:26`). API rígida sem marco correspondente retorna `informativa`, e seu chamador trata somente grave/leve: não emite pergunta dessa informação (`app/chamados/detector_anomalias.py:212`, `app/chamados/detector_anomalias.py:216`, `app/api/views.py:705`).
- **Folga API:** grava primeiro (`app/api/views.py:654`), depois detector devolve `grave`, `tipo=batida_em_folga` (`app/chamados/detector_anomalias.py:170`). Adiciona motivo `batida_fora_escala_grave` (`app/api/views.py:704`), cria/atualiza disputa depois da gravação (`app/api/views.py:758`), cuja fábrica gera a pergunta (`app/chamados/services/disputa_emissao.py:683`, `app/chamados/services/disputa_emissao.py:696`): “Confirme o posto e o horário da batida”, opções “Confirma”/“Está diferente” (`app/chamados/catalogo/perguntas.py:82`). Não é autorização prévia: a batida já foi gravada antes desse detector (`app/api/views.py:654`, `app/api/views.py:704`).
- **Perguntas posteriores por célula:** cartório chama `perguntar_pela_celula` após julgar (`app/ponto/services/cartorio.py:1030`, `app/ponto/services/cartorio.py:1045`). Fábrica usa marcos apagados e vencidos (`app/chamados/services/fabrica_da_celula.py:52`, `app/chamados/services/fabrica_da_celula.py:68`). Mapeamento: inícioE→`saida_sem_entrada`; fimS→`orfao_14h`; intervalos→`intervalo_saida`/`intervalo_volta` (`app/chamados/services/declaracao_texto.py:42`). Textos pedem horário da entrada, saída ou intervalo do dia (`app/chamados/catalogo/perguntas.py:48`, `app/chamados/catalogo/perguntas.py:56`, `app/chamados/catalogo/perguntas.py:124`, `app/chamados/catalogo/perguntas.py:132`).
- **E avulsa longe em dia útil:** se houver marco apagado, encerramento e demais condições, suprajuiz acusa `FURO_PARCIAL` mesmo sem par fechado (`app/ponto/supra_juiz.py:450`), além de `BATIDA_ORFA_FORA_TOLERANCIA` (`app/ponto/supra_juiz.py:528`). Cartório emite chamado dos marcos faltantes (`app/ponto/services/cartorio.py:1813`, `app/ponto/services/cartorio.py:1828`). A pergunta posterior pode cobrar08h/17h e intervalos apagados, não “por que21:36”, conforme mapeamento (`app/chamados/services/declaracao_texto.py:42`). Emissão efetiva depende dos gates: ativo, não futuro, veredito acusador, parcial fora da lista e dedup (`app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/services/fabrica_da_celula.py:120`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:157`). Push dessa fábrica é silenciado (`app/chamados/services/fabrica_da_celula.py:217`).
- **Folga posteriormente:** `classificar_falta` devolve `(None,None,None)` quando célula diz `trabalha=False` (`app/escala/servico_jornada.py:308`). Fábrica não pergunta marcos de dia sem trabalho (`app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/juizes.py:459`); leitor de faltantes também pula folga (`app/chamados/services/declaracao_texto.py:119`). Havendo realizado **ou órfãs**, dia encerrado e previsto0 pode lavrar `FATO_SEM_PREVISAO` (`app/ponto/supra_juiz.py:392`, `app/ponto/supra_juiz.py:423`), que não acusa (`app/ponto/supra_juiz.py:424`). Isso difere da pergunta imediata de batida fora da escala da API (`app/api/views.py:704`).
- **Órfã envelhecida:** detector paralelo de órfãs lê geometria sem expiração, lista abertas com idade>0,5h (`app/ponto/utils_turno_orfao.py:31`), aplica faixa14≤idade<96h (`app/ponto/services/triagem_batida.py:25`) e pode produzir motivo `orfao_14h` (`app/chamados/detector_anomalias.py:31`). Essa pergunta posterior depende também da lei e dos gates da fábrica (`app/chamados/services/disputa_emissao.py:287`).

Pontos candidatos para perguntas de lei: expirar E recém-gravada pelo relógio previsto; repetir sugestãoE indefinidamente após expiração; caminhos API/PWA distintos para folga/desvio; dispensar ciclos5x2 de janela PWA e comercial do detector API; aceitar `informativa` sem pergunta imediata; coexistência da expiração viva e da lista geométrica de órfãs. Ausência de decisão expressa em LEIS/CORTES sobre cada item: **[não verificado]**.

## 15/11 22:15 · sessao a77e910b7b2b · - · - · origem=Ronald (tmux)

O pareamento bruto **abre um turno para cada `E`**; havendo outra `E`, conserva o anterior aberto e inicia outro. Isso ocorre em `parear_turnos`, `app/ponto/turnos.py:1402`, `:1417`, `:1419`; o último aberto é anexado em `:1497`. A expiração é outra pergunta: `turno_aberto_vivo` elimina da resposta “vivo agora” o último aberto se `agora > expiracao_da_aberta`, sem fechar ou apagar a geometria (`app/ponto/turnos.py:2006`, `:2010`, `:2011`). `expiracao_da_aberta` devolve fim previsto da data_turno + 4h; sem tipo/fim, `None` (`app/ponto/turnos.py:2016`, `:2024`, `:2027`, `:2030`).

**Folga e fora da janela não são guardas de abertura em `parear_turnos`**: o ramo `E` não consulta `trabalha` ou horário previsto (`app/ponto/turnos.py:1402`, `:1417`). A consulta “vivo” usa template inclusive na folga: chama `janela_turno_de` sem `ec` (`app/ponto/turnos.py:2027`), cuja fonte sem `ec` são os marcos do tipo (`app/ponto/selecao_periodo.py:151`, `:154`, `:164`, `:169`).

Para **08–17, E21:36 e E21:49**, no cenário com apenas essas batidas e sem outras particularidades:

- A primeira e a segunda abrem dois turnos geométricos, ambos sem saída (`app/ponto/turnos.py:1417`, `:1419`, `:1497`).
- Nenhum está vivo às 21:36/21:49, pois o fim 17h + 4h já passou (`app/ponto/turnos.py:2011`, `:2027`, `:2030`).
- `proximo_tipo_de` recebe `None` e sugere `E` outra vez (`app/ponto/turnos.py:2474`, `:2475`, `:2476`).
- Os 13min não caracterizam eco offline: `_marca_ecos_flush` exige distância dos eventos ≥1800s, além de chegada conjunta e prova de flush (`app/ponto/turnos.py:366`, `:370`).
- O realizado é zero com `sem_turno=False, aberto=True`, pois existem turnos mas nenhum fim fechado: `realizado_dos_turnos` marca aberto e ignora fim inexistente, retornando total zero (`app/ponto/turnos.py:719`, `:733`, `:736`, `:738`, `:771`).
- A forma inteira do dia real nominal, com possíveis batidas anteriores, não foi consultada em banco: **[não verificado]**. Havendo batidas anteriores, existe a exceção L-115, que exige um turno com fim fechado, ≥4 batidas e uma pausa interior única (`app/ponto/turnos.py:675`, `:689`, `:697`; `app/docs/LEIS.md:153`).

**Ata/cartório não corrigem esse E–E depois automaticamente**:

- `papel_por_minuto_da_ata` só fornece papel quando a ata descreve entrada e saída fechadas; dias abertos, sem as duas pontas, são ignorados (`app/ponto/turnos.py:1682`, `:1683`). Sem papel, o pareador cai no tipo gravado (`app/ponto/turnos.py:1382`).
- Na folga, o builder pula matching de marcos (`app/escala/utils.py:1605`), monta folga sem células de trabalho (`:1801`), conserva sobras como órfãs (`:1817`, `:1834`). Sem lâmpadas, `periodos_do_dia` responde desalinhado, sem entrada/saída (`app/ponto/juiz_batida.py:163`, `:195`, `:199`, `:204`).
- A reconciliação posterior pela célula só consome bordas de turnos fechados (`app/escala/utils.py:1945`, `:1947`); o alinhamento inicial pelo juiz exige exatamente um turno fechado (`:1593`, `:1594`). Dois abertos não passam.
- O cartório é chamado pelo signal após salvar batida (`app/ponto/signals.py:131`, `:135`; chamada `:93`), mas lavra a grade e suas lâmpadas/orfas/minutos, não inventa uma saída (`app/ponto/services/cartorio.py:730`, `:735`, `:738`, `:745`). Carimbo idêntico pode ser pulado (`:1409`, `:1413`).

**Leis úteis para consolidar e distinguir lacunas**:

- L-097 decide dinheiro fora dos marcos e autorização; não define expressamente elegibilidade para abrir turno (`app/docs/LEIS.md:115`; cobertura expressa dessa elegibilidade: **[não verificado]**).
- L-084 exige **duas** pontas >3h para Cadastro x Realidade e preserva trabalhado; entrada isolada 21:36 não satisfaz a figura da lei (`app/docs/LEIS.md:116`).
- L-103 decide zero declarado sem turno pareado (`app/docs/LEIS.md:129`).
- L-115 preserva a última `E` solta sem pausa fechada (`app/docs/LEIS.md:153`).
- O corte “DUAS-ENTRADAS” determina juiz único e casos nominais de divergência, não regra explícita para duas entradas consecutivas às21:36/21:49 (`app/docs/CORTES.md:369`; regra específica: **[não verificado]**).
- O corte de 28/10 exige reconhecer par real longe do marco pelo mesmo juiz e encaminhar o que não casa para Cadastro x Realidade (`app/docs/CORTES.md:405`). Esse corte não fornece resposta explícita para entrada sem saída pós-expiração (**[não verificado]**).
- Exportada não muda gravado; diferença vira Pauta DP (`app/docs/LEIS.md:125`).
- HE folga com par completo depende da janela ativa: teste documenta bloqueio integral até autorização e publica pendência dia (`app/ponto/tests/test_he100_dia_sem_previsto.py:4`, `:7`, `:69`, `:77`, `:83`). Empresa sem janela paga como antes (`:101`, `:105`). O aval específico HE100-sem-previsto aparece apenas referido no CORTES (`app/docs/CORTES.md:352`, `:357`), não achei texto integral explícito nesses dois documentos **[não verificado]**.

Possíveis perguntas de lei distintas para a conclusão: (1) entrada nova do próprio dia pode nascer já expirada pelo fim do turno previsto; (2) folga permite abertura geométrica mas sua vitalidade expira pelo template de trabalho; (3) entrada fora da janela em dia de trabalho abre geometria sem condição horária; (4) segunda entrada nessas condições acumula aberto, em vez de outra interpretação. A decisão expressa dessas quatro figuras em LEIS/CORTES não foi localizada **[não verificado]**.

## 15/11 22:15 · sessao 085c54a5b017 · - · - · origem=Ronald (tmux)

Leitores e destinos rastreados, somente por leitura estática:

- **Espelho colaborador e admin:** ambos chamam `espelho_do_colab`, diferenciados por `modo_gestor` (`app/ponto/views.py:117`, `:160-161`). O dia entrega batidas, períodos e inconsistência (`app/ponto/services/espelho.py:655-657`, `:765-776`). Entrada aberta recebe `realizado_aberto`; a tela mostra **“EM ABERTO — [frase do marco faltante]”** e eventual “parcial provado” (`app/ponto/services/espelho.py:783-794`; `app/templates/ponto/espelho.html:187`). Batida que a grade não casa aparece como órfã roxa (`app/templates/ponto/espelho.html:240-242`). Na ausência de grade, imprime os tipos crus E/S (`app/templates/ponto/espelho.html:243-248`).

- **Dinheiro no espelho:** vem de `DiaPago`, incluindo a indicação “sem apuração ainda” quando não lavrado (`app/ponto/services/espelho.py:801-813`; `app/templates/ponto/espelho.html:268-283`). Portanto não deduzir dinheiro pago só do estado aberto exibido: essas fontes são distintas (`app/ponto/services/espelho.py:837-842`).

- **Calendário admin:** `calendario_ponto` chama `contexto_calendario` (`app/colaboradores/views.py:1938`, `:1948-1949`). Este usa `CargaDaJanela` e seus turnos (`app/colaboradores/services/calendario.py:214-224`); marca batidas que a grade chama órfãs (`:622-627`) e lê o status de `status_dos_dias_lavrados` (`:604-607`, `:662-665`). Rubricas e selo HE vêm de `DiaPago`, não de novo cálculo local (`:651-665`, `:682-686`).

- **O relógio pode divergir do badge geométrico:** D4 mostra `aberto` quando `dias_encerrados` devolve turno aberto vivo; encerrado com lavra lê o status armazenado; encerrado sem lavra e com batida mostra `sem_certidao` (`app/ponto/status_dia.py:273-295`). Assim, após expiração, não é correto prometer que o calendário continuará mostrando “aberto”: depende da lavra; a geometria pode continuar aberta (`app/ponto/status_dia.py:280-294`; `app/ponto/services/espelho.py:837-842`).

- **Folga lavrada:** com batida, `status_da_lavra` devolve `folga_trabalhada` quando `tipo_dia == 'folga'` ou veredito `fato_sem_previsao`; rótulo “Trabalhou em dia sem jornada prevista” (`app/ponto/status_dia.py:36`, `:157-158`). Antes do encerramento, a precedência de andamento acima pode mostrar aberto (`app/ponto/status_dia.py:278-285`).

- **Entrada avulsa não vira HE por si:** no comercial, `calcular_periodo(entrada, None)` marca `turno_aberto`, alerta “Periodo aberto — saida nao registrada.” e retorna antes da janela e duração (`app/ponto/motor_calculo_v2.py:3215-3226`). O adaptador representa aberta como `(entrada, None)` e anomalia “Entrada sem saída” (`app/ponto/adaptador_motor.py:24-25`, `:37-43`). O mesmo retorno antecipado aparece nos demais calculadores (`app/ponto/motor_calculo_v2.py:2752-2755`, `:2879-2882`).

- **Folga com par fechado e minutos:** `_dia_sem_previsto_fora_da_janela` bloqueia integralmente quando folga/feriado, empresa dentro da configuração de janela, previsto zero, sem autorização; folga sem escala certa é exceção e retorna sem bloquear (`app/ponto/motor_calculo_v2.py:1440-1470`). Zera rubricas e publica minutos pendentes (`:1471-1488`). O espelho traduz para pendência `sentido='dia'` (`app/ponto/services/espelho.py:743-749`) e mostra “HE fora da janela … sem decisão” (`app/templates/ponto/espelho.html:189`).

- **Gestão HE:** lê retrato lavrado (`app/ponto/views.py:932-946`), cujo apurador só inclui dias com `he_fora_da_janela`, rotulando ausência de decisão como `sem_decisao` (`app/ponto/services/he_pendente_lavrado.py:132-152`). O fato “E às 21:36” isoladamente não basta para afirmar que entrou nessa fila; precisa existir pendência publicada pelas fontes acima (`app/ponto/services/he_pendente_lavrado.py:132-134`).

- **Fechamento/folha:** grava HE 50/100 do resultado (`app/ponto/services/fechamento.py:918-924`). Folga com escala certa vai a `horas_folga_trabalhada` (`:928-935`, `:965-966`); sem escala certa, horas vão a trabalhadas sem adicional de 100% (`:967-984`). Exportação tem rubrica própria de folga (`app/folha/export.py:441-446`). **Pauta DP por autorização não nasce da entrada:** exige ato com efeito e competência exportada (`app/ponto/services/autorizacao_he_periodo.py:290-302`).

- **Pendências admin:** a aba de disputas lê disputas ainda não fechadas e as classifica por perguntas/possibilidade de fechar, não recalcula se a E abre turno (`app/colaboradores/views.py:2998-3013`; `app/chamados/relatorios_disputa.py:11-28`, `:45-58`). Os grupos mostrados são “Você valida agora”, “Falta materializar”, “Cobrar o colaborador” e “Técnico: disputa sem pergunta” (`app/colaboradores/views.py:3002-3006`). Existência concreta de uma disputa para o caso real: **[não verificado]**.

- **Ata/célula não corrige automaticamente uma dupla E que ficou aberta:** reconciliação de bordas só consome turnos fechados; aberta é explicitamente pulada (`app/escala/utils.py:1919-1928`, `:1945-1948`). Também pula classe de cadastro com ambas as pontas a mais de 180 min dos marcos (`:1990-1992`). Não há realimentação dentro da mesma montagem; uma ata curada só alimenta o pareador na montagem seguinte (`:1937-1942`). Em folga não se enumeram marcos (`:1605-1606`), o dia sai sem células (`:1801-1803`), e batidas não consumidas são conservadas em `orfas` (`:1814-1834`).

- **E 21:36 / E 21:49:** dedupe específico da grade só elimina mesmo tipo com diferença **menor que 10 min**; 13 min não é eliminado por ele (`app/escala/utils.py:1324-1339`). Decisão do pareador sobre essas duas E deve ser consolidada com o rastreio principal; não a determinei aqui **[não verificado]**.

- **Cartório/supra para sobras fora da janela de atribuição:** dia-trabalho previsto com realizado zero e órfãs publica `BATIDA_ORFA_FORA_TOLERANCIA` (“batida existe e nao foi pareada”) (`app/ponto/supra_juiz.py:527-537`). Folga com fato, encerrada e sem previsto publica `FATO_SEM_PREVISAO`, sem acusação de falta (`:392-393`, `:423-428`). Isso descreve classificação; pergunta efetivamente emitida deve ser confirmada na cadeia do cartório **[não verificado]**.

## 15/11 22:06 · sessao c2e5795c5eb3 · RESCALDO-CORTE-1511 · censo · origem=Code

`*` marca a BASE; os demais itens ficam como referências. As citações de `paradas49.txt` numeram as entradas fornecidas de 1 a 49; `emvoo.txt:1–2` identifica seus dois blocos; numeração física dos anexos [nao verificado]. Os materiais foram limitados à listagem fornecida; conteúdo dos logs [nao verificado].

LASTRO-FOLGA foi incorporada à LASTRO-FAIXA pela absorção expressa no topo (app/docs/RELATO.md:4). O item conjunto QUARENTENA/SVV foi contabilizado somente em QUARENTENA; essa atribuição contábil é uma convenção [nao verificado]. EM VOO conserva o rótulo do anexo mesmo nas cópias retidas (emvoo.txt:1).

| obra | ESTADO | DESTRAVA | DONO | MATERIAL | PROXIMA ACAO | itens do quadro |
|---|---|---|---|---|---|---|
| CODEX-CONTEXTO-DA-CASA | PARADA — briefs pendentes | nada | Code | ref CODEX-CONTEXTO-DA-CASA@4435ec647; brief: BACKLOG:1095 | Conferir a entrega dos briefs com PERGUNTA antes de certificar o pouso (app/docs/BACKLOG.md:1095). | CODEX-CONTEXTO-DA-CASA* (paradas49.txt:1) |
| JUSTIFICATIVA-PAGA-O-PERIODO | PARADA — aguarda juiz | pouso de COMPETENCIA-PAGA-UM-JUIZ | Code | logs/JUSTIFICATIVA-PAGA/c17/brief.md; logs/JUSTIFICATIVA-PAGA-O-PERIODO/brief_construtor.md; ref wt-codex-justificativa-paga-o-periodo-220358@74ec17605 | Após o juiz, certificar a árvore de pouso e cumprir as condições preservadas do opus (app/docs/BACKLOG.md:1948; app/docs/RELATO.md:210). | wt-codex-justificativa-paga-o-periodo-220358*; ref JUSTIFICATIVA-PAGA-O-PERIODO; ref wt-justif-red-095036 (paradas49.txt:2,4,38) |
| DATA-EVENTO-CRU | PARADA — matriz obrigatória | desenho | Claude-desenho | logs/DATA-EVENTO-CRU/censo_v2.r; ref wt-catraca-dec-151606@3e86f61fe | Fechar a matriz dos três bloqueantes usando a v4 documentada, pois a precedência cronológica de 70dfbc62a é [nao verificado] (app/docs/RELATO.md:1059; app/docs/RELATO.md:9123). | wt-catraca-dec-151606*; ref wt-dec-sobre-main-205403 (paradas49.txt:3,32) |
| CODEX-2-RAIAS | PARADA — quatro procedentes | nada | Code | logs/CODEX-2-RAIAS/matriz.r; logs/codex_2_raias/construtor_v1_brief.txt; ref wt-codex2raias-090624@e381017bf | Curar os quatro procedentes e concluir a volta 3 com certificação e opus (app/docs/RELATO.md:1488; app/docs/RELATO.md:1503). | wt-codex2raias-090624* (paradas49.txt:5) |
| FRAC5-MINIMO-5 | PARADA — cura guardada | ! novo corte FRAC5-MINIMO-5 | Ronald | ref wt-frac5-cura-100047@b7d6b6aed; brief: BACKLOG:788 | Dar o novo corte de pouso considerando as pendências preservadas da cura (app/docs/BACKLOG.md:788). | wt-frac5-cura-100047*; ref wt-frac5-100005 (paradas49.txt:6,36) |
| FATO-DE-AUSENCIA-ACORDA-A-PROJECAO / LOTE7 | PARADA — c6 anunciada | desenho | Claude-desenho | logs/ESPINHA-BRIEFS/MATRIZ-LOTE7/entrega.md; ref wt-lote7-ausencia-100250@22677d0db | Resolver o acordador pela matriz e conferir a liberação anunciada para c6 após R3 c17, cujo teor permanece [nao verificado] (app/docs/RELATO.md:59; emvoo.txt:2). | wt-lote7-ausencia-100250* (paradas49.txt:7) |
| COBPARC-FILA-RELOGIO | PARADA — provas pendentes | nada | Code | ref COBPARC-FILA-RELOGIO@ab828ddcf; brief: BACKLOG:916 | Concluir mutantes_v4, suíte e opus com o padrão já admitido (app/docs/BACKLOG.md:916; app/docs/RELATO.md:582). | COBPARC-FILA-RELOGIO* (paradas49.txt:8) |
| LASTRO-DEGRAU-2 | PARADA — cura anterior | nada | Code | ref LASTRO-DEGRAU-2@de6097212; brief: BACKLOG:1814 | Curar o construtor e migrar o casamento ao juiz conforme o censo posterior (app/docs/BACKLOG.md:1814; app/docs/RELATO.md:582). | LASTRO-DEGRAU-2* (paradas49.txt:9) |
| O10-C | PARADA — UI incompleta | nada | Code | ref O10-C@a40577fba; brief: BACKLOG:2196 | Construir a lista do admin e a porta alcançável da troca, preservando o ! separado do apply na 09 (app/docs/RELATO.md:1987; app/docs/RELATO.md:1992). | O10-C* (paradas49.txt:10) |
| O275 / SDF-FERIADO-E-DIA | PARADA — formas divergentes | lei FORMA A x FORMA B | Ronald | logs/o275/censo.md; ref O275@32c8c259e | Cortar FORMA A/B antes de atualizar a construção (app/docs/RELATO.md:582). | O275* (paradas49.txt:11) |
| OTIMIZA-1 | PARADA — aguarda instrumento | pouso de SUITE-POR-IMPACTO | Code | ref OTIMIZA-1@2d91d5905; brief: BACKLOG:2197 | Após SUITE-POR-IMPACTO, concluir suíte, replay, verificação e pouso próprio (app/docs/BACKLOG.md:2197). | OTIMIZA-1* (paradas49.txt:12) |
| PADRAO-DE-CIMA | PARADA — harness aberto | desenho | Claude-desenho | ref PADRAO-DE-CIMA@7635c1c8a; brief: BACKLOG:892 | Resolver como o harness identifica a causa da morte do mutante antes de retomar (app/docs/RELATO.md:2494). | PADRAO-DE-CIMA* (paradas49.txt:13) |
| QUARENTENA-DO-COLAB | PARADA — RED não rodado | desenho | Claude-desenho | logs/QUARENTENA-REABRE/censo.r; ref wt-quarentena-svv-151703@c9a2a3846 | Resolver 16 versus 15 janelas e preparar a retomada após o pouso E3-RESTO-R3 (app/docs/RELATO.md:840; app/docs/BACKLOG.md:2586). | wt-quarentena-svv-151703*; ref QUARENTENA-DO-COLAB (paradas49.txt:14,41) |
| SILENCIO-VENCIDO-VOLTA | PARADA — retomada conjunta | pouso de QUARENTENA-DO-COLAB | Code | logs/QUARENTENA-REABRE/censo.r; ref SILENCIO-VENCIDO-VOLTA@7e8648704 | Retomar junto da QUARENTENA usando o censo compartilhado e a cópia conjunta documentada (app/docs/BACKLOG.md:838; app/docs/RELATO.md:839). | SILENCIO-VENCIDO-VOLTA* (paradas49.txt:15) |
| SUITE-POR-IMPACTO | PARADA — PRONTO 5 incompleto | ! aval PRONTO-5 | Ronald | ref SUITE-POR-IMPACTO@2d91d5905; brief: BACKLOG:1093 | Resolver o aval do PRONTO 5, preservando a entrada após E3 e o corte separado do gate pela classe (app/docs/RELATO.md:556; app/docs/BACKLOG.md:1093). | SUITE-POR-IMPACTO* (paradas49.txt:16) |
| V3-E1 | PARADA — lotes não validados | lei V3-E1 C06/C16/24x48 | Ronald | ref V3-E1@2430fcc5b; brief: BACKLOG:2195 | Cortar as questões C06/C16/24x48 antes da validação e integração dos lotes (app/docs/RELATO.md:556; app/docs/RELATO.md:2714). | V3-E1* (paradas49.txt:17) |
| V3-RETOMA-FILA | PARADA — migração pendente | pouso de V3 | Code | ref V3-RETOMA-FILA@0448d88f1; brief: BACKLOG:1033 | Demonstrar o pouso exigido e então migrar o instrumento e resolver CCT pelo dia (app/docs/BACKLOG.md:1033). | V3-RETOMA-FILA* (paradas49.txt:18) |
| V3-RETOMA-PERIODOS-DA-CELULA | PARADA — P4 incompleto | lei período não guardado na célula | Ronald | ref V3-RETOMA-PERIODOS-DA-CELULA@c71b1038b; brief: BACKLOG:1021 | Resolver o período ausente e concluir P4, considerando P5 já pousada isoladamente (app/docs/BACKLOG.md:1021; app/docs/RELATO.md:8395). | V3-RETOMA-PERIODOS-DA-CELULA* (paradas49.txt:19) |
| LASTRO-FAIXA-SEM-DEFAULT | PARADA — inclui LASTRO-FOLGA | pouso de TIPO-NA-FONTE | Code | logs/LASTRO-FAIXA/redesenho/predesenho.r; logs/LASTRO-FAIXA-SEM-DEFAULT/brief_c3.md; ref wt-codex-lastro-redesenho-164050@f3ebd821b | Construir sobre o acessor tipado usando a base escolhida e o RED que absorveu LASTRO-FOLGA (app/docs/BACKLOG.md:1895; app/docs/RELATO.md:4). | wt-codex-lastro-redesenho-164050*; ref mut-lastro-c3-210250; ref red-lastro-c3-210250; ref wt-lastro-folga-133123 (paradas49.txt:20,22,26,39) |
| NÃO IDENTIFICADA — recon_onprem | PARADA — identificação pendente | nada | Code | ref recon_onprem@612be166a; brief: BACKLOG:[nao verificado] | Identificar as obras pelo diff integral antes de selecionar uma base de retomada (app/docs/RELATO.md:328; conteúdo integral [nao verificado]). | recon_onprem* (paradas49.txt:21) |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | PARADA — leis do predesenho | lei PRE-DESENHO-BUG-PAR-VOLTA | Ronald | logs/ESPINHA-BRIEFS/MATRIZ-C7-BUGPAR/entrega.md; logs/BUG-PAR-O-PAR/predesenho/entrega.md; logs/BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS/brief.md; ref wt-bugpar-main-095024@6e2fc9133 | Cortar a regra do universo 163 e o alcance exportado, mantendo a volta condicionada ao pouso R3 e a novo aval (app/docs/RELATO.md:18). | wt-bugpar-main-095024* (paradas49.txt:23) |
| BO-DISPUTA-SEM-PERGUNTA | PARADA — c25 estacionada | desenho | Claude-desenho | logs/BO-DISPUTA-SEM-PERGUNTA/matriz/m1.r; logs/BO-DISPUTA-SEM-PERGUNTA/brief_construtor.md; ref wt-codex-bo-disputa-sem-pergunta-183316@cabdb0b4c | Fechar nascimento, destino e dia na RAIZ-DA-DISPUTA antes de nova implementação (app/docs/CORTES.md:457). | wt-codex-bo-disputa-sem-pergunta-183316*; ref wt-c22-red-152256; ref wt-red-bodisputa-175220 (paradas49.txt:24,25,42) |
| PAR-NAO-ADJACENTE | PARADA — v5 por construir | nada | Code | logs/par_nao_adjacente/censo.r; ref wt-par-rebase-060915@84a400e1c | Implementar a v5 já determinada pela LEI-PAR-ANINHADO, com externa prevalecendo e interna absorvida (app/docs/RELATO.md:1599; app/docs/RELATO.md:1601). | wt-par-rebase-060915*; ref wt-codex-par-nao-adjacente-031500 (paradas49.txt:27,40) |
| PAUTA-SEM-TURNO-PREFIXO-RESERVADO | PARADA — quatro furos | desenho | Claude-desenho | logs/PAUTA-SEM-TURNO-PREFIXO-RESERVADO/brief_construtor.md; ref wt-codex-pauta-sem-turno-prefixo-reservado-030404@b054a6d69 | Resolver os quatro furos de identidade textual antes de autorizar outra volta (app/docs/BACKLOG.md:2123). | wt-codex-pauta-sem-turno-prefixo-reservado-030404* (paradas49.txt:28) |
| TIPO-NA-FONTE — item 3 | PARADA — alternativa aberta | desenho | Claude-desenho | logs/TIPO-NA-FONTE/brief_item3.md; logs/TIPO-NA-FONTE/item3/predesenho/matriz_v5c.txt; logs/TIPO-NA-FONTE/item3/predesenho/predesenho.r; ref wt-codex-tipo-na-fonte-184856@12e424d5f | Escolher entre selo do valor e acessor tipado antes de curar leitores e escritor (app/docs/BACKLOG.md:1891; app/docs/RELATO.md:716). | wt-codex-tipo-na-fonte-184856* (paradas49.txt:29) |
| TRIAGEM-OFFLINE-RED-HEADLESS | PARADA — apenas RED | nada | Code | logs/TRIAGEM-OFFLINE-SEM-CAMERA/censo_v1.r; ref wt-codex-triagem-offline-red-headless-101137@d680518b0 | Curar o SW e provar clique offline e reload nas duas cascas pela classe A (app/docs/RELATO.md:962). | wt-codex-triagem-offline-red-headless-101137* (paradas49.txt:30) |
| COMPETENCIA-PAGA-UM-JUIZ | PARADA — corte da 10 paga | ! corte LU2-06 sobre competência 10 paga | Ronald | logs/COMPETENCIA-PAGA/censo.md; ref wt-comp-paga-101739@1ec224519 | Resolver o alcance com a competência 10 marcada paga e concluir as provas finais (app/docs/RELATO.md:69; app/docs/RELATO.md:97). | wt-comp-paga-101739* (paradas49.txt:31) |
| DEPLOY-EXCLUSAO-ENVELOPE | PARADA — volta 3 pendente | nada | Code | logs/deploy_exclusao/censo_v1.r; ref wt-deploy-exclusao-061631@f11329237 | Curar Q1–Q5 e certificar a exclusão compartilhada entre cron e deploy (app/docs/RELATO.md:1592). | wt-deploy-exclusao-061631* (paradas49.txt:33) |
| E4C-F2 | PARADA — matriz e leis | lei F2 L1/L2 | Ronald | logs/ESPINHA-BRIEFS/F2/matriz.md; ref wt-e4c-f2-103933@8c6577245 | Cortar efeito e face SQL, preservando o pouso prévio da F1 e o corte próprio para exportadas (app/docs/RELATO.md:17; app/docs/RELATO.md:100). | wt-e4c-f2-103933* (paradas49.txt:34) |
| COERENCIA-PELO-EFEITO | PARADA — ensaio incompleto | nada | Code | ref wt-ensaio-coerencia-100453@f7510b95a; brief: BACKLOG:753 | Conferir o diff integral do ensaio e determinar a entrega restante pelo censo (app/docs/BACKLOG.md:753; diff integral [nao verificado]). | wt-ensaio-coerencia-100453* (paradas49.txt:35) |
| CORTE-DAS-TRAVAS | PARADA — v5 sem certificação | nada | Code | ref wt-instr-travas-002004@b05de7432; brief: BACKLOG:754 | Curar B1–B4 e certificar a versão corrigida com suíte e opus (app/docs/RELATO.md:484). | wt-instr-travas-002004* (paradas49.txt:37) |
| RELAVRA-11-DOS-50 | PARADA — foto e reversão | pouso de COMPETENCIA-PAGA-UM-JUIZ | Code | ref wt-relavra11-113556@d36594345; brief: BACKLOG:1941 | Após COMPETENCIA-PAGA, concluir foto anterior ao delete, reversão e fechamento das contas exigidas (app/docs/BACKLOG.md:1941). | wt-relavra11-113556* (paradas49.txt:43) |
| SAIDA-NOTURNA-NO-DIA-CERTO | PARADA — D1/D3 pendentes | nada | Code | logs/SAIDA-NOTURNA/censo.r; ref wt-saida-noturna-135512@6fd676c84 | Concluir D1/D3 e certificar a obra, considerando o pouso já registrado da E3 anterior (app/docs/RELATO.md:893; app/docs/RELATO.md:333). | wt-saida-noturna-135512* (paradas49.txt:44) |
| SELO-ESPERA-PROCESSO-CEGO-A-FIXTURE | PARADA — sem v2 | ! novo aval SELO-ESPERA-ESTACIONA | Ronald | ref wt-selo-espera-084620@23af328bc; brief: BACKLOG:2575 | Dar novo aval de retomada antes de corrigir o filtro de RAIZ (app/docs/BACKLOG.md:2575). | wt-selo-espera-084620* (paradas49.txt:45) |
| SELOS-CANON-I3 | PARADA — matriz pendente | desenho | Claude-desenho | logs/SELOS-CANON-I3/brief.md; logs/SELOS-CANON-I3/matriz.q; ref wt-selos-canon-i3-113049@4561d76e6 | Concluir a matriz e preparar a volta 4, preservando a dependência de CODEX-2-RAIAS (app/docs/RELATO.md:1489; app/docs/RELATO.md:1507). | wt-selos-canon-i3-113049* (paradas49.txt:46) |
| SONDA-SEM-GRANT | PARADA — volta 8 pendente | pouso de CODEX-2-RAIAS | Code | ref wt-sonda-sem-grant-075803@c5e7df309; brief: BACKLOG:2131 | Após CODEX-2-RAIAS, executar a volta 8, rebase e certificações previstas (app/docs/RELATO.md:1515). | wt-sonda-sem-grant-075803* (paradas49.txt:47) |
| NÃO IDENTIFICADA — wt-ui | PARADA — resto documental | nada | Code | ref wt-ui@6bd163b34; brief: BACKLOG:[nao verificado] | Identificar a obra e conferir o sujo integral antes de atribuir entrega de comportamento (paradas49.txt:48; vínculo com obra [nao verificado]). | wt-ui* (paradas49.txt:48) |
| O280 / V3-BATERIA-DE-CASOS-PELA-REGRA | PARADA — cópia RED | ! troca do motor V3 | Ronald | logs/o280/matriz_recebida.md; logs/v3_bateria/matriz_recebida.md; ref wt-v3@bb3a588d6 | Conferir os testes sujos e demonstrar o fechamento atual do portão antes do ! da troca (app/docs/BACKLOG.md:1620; conteúdo sujo [nao verificado]). | wt-v3* (paradas49.txt:49) |
| BUG-ENC-CELULAS-FORMA | EM VOO — opus final | nada | Code | logs/ORDEM-VIVA-1511/BUG-ENC/censo.r; ref wt-bug-enc-201609@380d20c27 | Aguardar o opus final preservando a cópia em voo (emvoo.txt:1). | — (emvoo.txt:1) |
| E3-RESTO-R3 | EM VOO — c17 retida | ! novo aval R3 c17 | Ronald | logs/E3-RESTO-R3/c16/censo_fronteira.r; logs/E3-RESTO-R3/c17/predesenho.md; logs/E3-RESTO-R3/matriz/matriz.md; ref wt-codex-e3-resto-r3-093851@f31a8d09e | Obter aval novo citando o predesenho antes de construir c17 (app/docs/RELATO.md:22). | — (emvoo.txt:1) |
| E4C-F1-NOVA-JUIZ-RESPONDIDA | EM VOO — B1 no anexo | ! novo aval F1 nova | Ronald | logs/E4C/f1nova/predesenho.md; logs/JUIZ-RESPONDIDA/censo.md; ref wt-f1nova-b1-203152@[nao verificado] | Obter aval novo citando a entrega do predesenho após o corte JUIZ-RESPONDIDA (app/docs/BACKLOG.md:1945). | — (emvoo.txt:1) |
| FERIADO-USA-A-LEI-DE-1609 / FERIADO-G | EM VOO — PAREI 3(ii) | ! FERIADO-G item 3(ii) | Ronald | ref wt-feriado1609-110424@adb3f954c; brief: BACKLOG:729 | Resolver a condição 3(ii), que mantém a construção parada (app/docs/RELATO.md:7; app/docs/RELATO.md:8). | — (emvoo.txt:1) |
| HE-REVOGADA-NA-PORTA | EM VOO — pre-opus | nada | Code | logs/RETOMA-HE-LOTE3/pv_he.r; ref wt-he-revogada-095943@019e5157a | Aguardar a suíte pre-opus e o opus final, preservando o escopo da 11 aberta (app/docs/RELATO.md:171; app/docs/RELATO.md:186). | — (emvoo.txt:1) |
| SITUACAO-DESLIGADO / LOTE 3 desligado | EM VOO — c6 na fila | ! VINCULO | Ronald | logs/RETOMA-HE-LOTE3/pv_lote3.r; ref wt-lote3-desligado-100246@874fb76d1 — c5 documentada; head c6 [nao verificado] | Concluir os dois procedentes anunciados da c6 e obter o ! de VINCULO para pousar (emvoo.txt:1; app/docs/RELATO.md:82). | — (emvoo.txt:1) |
| E4C-F3 | PARADA — matriz entregue | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Code | logs/ESPINHA-BRIEFS/F3/matriz.md; logs/ESPINHA-BRIEFS/F3/brief.md; ref [nao verificado]@[nao verificado] | Retomar a fatia após o pouso da F1 usando a matriz entregue (app/docs/RELATO.md:33). | — (emvoo.txt:2) |
| E4C-F4 | PARADA — duas leis abertas | lei F4 absorção e guardas do limbo | Ronald | logs/ESPINHA-BRIEFS/F4/matriz.md; logs/ESPINHA-BRIEFS/F4/brief.md; ref [nao verificado]@[nao verificado] | Cortar absorção pelo ato e guardas do limbo, preservando a construção após F1 (app/docs/RELATO.md:17). | — (emvoo.txt:2) |
| E4C-F5 | PARADA — lei respondida | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Code | logs/ESPINHA-BRIEFS/F5/matriz.md; logs/ESPINHA-BRIEFS/F5/brief.md; ref [nao verificado]@[nao verificado] | Adequar a fatia ao corte JUIZ-RESPONDIDA e construir após F1 (app/docs/RELATO.md:13; app/docs/RELATO.md:17). | — (emvoo.txt:2) |
| E4C-F6 | PARADA — lei residual | lei F6 aguarda_admin nos estados de absorção | Ronald | logs/ESPINHA-BRIEFS/F6/matriz.md; logs/ESPINHA-BRIEFS/F6/brief.md; ref [nao verificado]@[nao verificado] | Resolver a lei residual de aguarda_admin antes da construção posterior à F1 (app/docs/RELATO.md:17). | — (emvoo.txt:2) |
| E4C-F7 | PARADA — matriz entregue | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Code | logs/ESPINHA-BRIEFS/F7/matriz.md; logs/ESPINHA-BRIEFS/F7/brief.md; ref [nao verificado]@[nao verificado] | Preparar a fatia pela matriz e preservar as dependências de F1, F2 e BUG-PAR (app/docs/RELATO.md:33; app/docs/RELATO.md:149). | — (emvoo.txt:2) |
| E4C-F8 | PARADA — lei residual | lei F8 V legada com R sintética posterior | Ronald | logs/ESPINHA-BRIEFS/F8/matriz.md; logs/ESPINHA-BRIEFS/F8/brief.md; ref [nao verificado]@[nao verificado] | Cortar a validade da V legada após R sintética e construir somente após F1 (app/docs/RELATO.md:17). | — (emvoo.txt:2) |
| E4C-F9 | PARADA — matriz entregue | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Code | logs/ESPINHA-BRIEFS/F9/matriz.md; logs/ESPINHA-BRIEFS/F9/brief.md; ref [nao verificado]@[nao verificado] | Preparar a retomada da fatia após o pouso da F1 (app/docs/RELATO.md:17; app/docs/RELATO.md:33). | — (emvoo.txt:2) |
| PAR-CONFIRMADO-PELO-SUPERVISOR | PARADA — só registrada | lei PAR-CONFIRMADO perfil e juiz | Ronald | logs/PAR-CONFIRMADO/censo.md; ref [nao verificado]@[nao verificado] | Cortar o perfil que confirma e o nascimento do juiz antes de construir (app/docs/RELATO.md:39). | — (emvoo.txt:2) |

**TOTAL PARADA: 46 entradas consolidadas**, incluindo duas de obra ainda não identificada; **TOTAL EM VOO: 6**; **total da tabela: 52** (paradas49.txt:1–49; emvoo.txt:1–2; identificação das duas obras [nao verificado]).

**Prova dos 49 itens:** 31 grupos com 1 item + 4 com 2 + 2 com 3 + 1 com 4 = **31 + 8 + 6 + 4 = 49**, distribuídos em **38 grupos**; os outros 14 registros não acrescentam itens ao quadro original (paradas49.txt:1–49; emvoo.txt:1–2).

DESTRAVA alterado em relação ao quadro:

- **FRAC5-MINIMO-5:** unificado em `! novo corte FRAC5-MINIMO-5`; a cópia RED dizia `lei`, mas a cura está guardada e depende de novo corte de pouso (app/docs/BACKLOG.md:788).
- **BUG-PAR:** de `desenho e pouso de E3-RESTO-R3` para `lei PRE-DESENHO-BUG-PAR-VOLTA`; o pouso R3 e o novo aval continuam exigidos na próxima ação (app/docs/RELATO.md:18).
- **PAR-NAO-ADJACENTE:** de `desenho` para `nada`; a lei foi respondida e a próxima construção já está especificada (app/docs/RELATO.md:1599; app/docs/RELATO.md:1601).

## 15/11 22:01 · sessao 49155812141f · - · - · origem=Ronald (tmux)

Conferências principais:

- **BUG-PAR**: mudar `desenho` para `lei PRE-DESENHO-BUG-PAR-VOLTA`, dono Ronald. Permanecem duas perguntas não decididas; construção exige aval novo após pouso R3. **app/docs/RELATO.md:18**. [nao verificado]
- **PAR-NAO-ADJACENTE**: proponho mudar `desenho` para `nada`, dono Code: lei respondida; falta implementar v5 já especificada. **app/docs/RELATO.md:1599**, **app/docs/RELATO.md:1601**, **app/docs/BACKLOG.md:1247**.
- **COMPETENCIA-PAGA**: manter `! corte LU2-06 sobre competência 10 paga`; P2 respondida não elimina corte sobre a 10 paga. **app/docs/RELATO.md:62**, **app/docs/RELATO.md:69**.
- **F2**: L1/L2 ainda aparecem abertas, além de corte exportado LU2-13. Manter lei F2 como quadro, mencionando demais condições na ação. **app/docs/RELATO.md:17**, **app/docs/RELATO.md:64**, **app/docs/RELATO.md:100**.

Bases:

- **LASTRO-FAIXA**: escolha explícita `wt-codex-lastro-redesenho-164050@f3ebd821b`. **app/docs/BACKLOG.md:1895**.
- **BO-DISPUTA**: `wt-codex-bo-disputa-sem-pergunta-183316@cabdb0b4c`, c25 guardada. **app/docs/RELATO.md:371**.
- **PAR-NAO-ADJACENTE**: `wt-par-rebase-060915@84a400e1c`, reaplicação posterior. **app/docs/RELATO.md:1605**, **app/docs/RELATO.md:1606**.
- **DATA-EVENTO-CRU**: `3e86f61fe` é a base documentada da catraca; cronologia de `70dfbc62a` não aparece nos quatro documentos pesquisados **[nao verificado]**. **app/docs/RELATO.md:9123**, **app/docs/RELATO.md:1067**.

Demais destravas preservadas:

| obra | destrava | fonte |
|---|---|---|
| BO-DISPUTA-SEM-PERGUNTA | desenho | app/docs/CORTES.md:457 |
| LASTRO-FAIXA-SEM-DEFAULT | pouso de TIPO-NA-FONTE | app/docs/BACKLOG.md:1895 |
| PAUTA-SEM-TURNO-PREFIXO-RESERVADO | desenho | app/docs/BACKLOG.md:2123 |
| TIPO-NA-FONTE | desenho | app/docs/BACKLOG.md:1891 |
| TRIAGEM-OFFLINE-RED-HEADLESS | nada | app/docs/RELATO.md:962 |
| DATA-EVENTO-CRU | desenho | app/docs/RELATO.md:1059 |
| DEPLOY-EXCLUSAO-ENVELOPE | nada | app/docs/RELATO.md:1592 |
| COERENCIA-PELO-EFEITO | nada | app/docs/BACKLOG.md:753 |
| FRAC5-MINIMO-5 | ! novo corte de pouso FRAC5-MINIMO-5 | app/docs/BACKLOG.md:788 |
| CORTE-DAS-TRAVAS | nada | app/docs/RELATO.md:484 |
| JUSTIFICATIVA-PAGA-O-PERIODO | pouso de COMPETENCIA-PAGA-UM-JUIZ | app/docs/BACKLOG.md:1948 |
| LASTRO-FOLGA-FORA-DO-CATALOGO | pouso de TIPO-NA-FONTE | app/docs/RELATO.md:4; app/docs/BACKLOG.md:1895 |
| QUARENTENA-DO-COLAB | desenho | app/docs/RELATO.md:840 |
| RELAVRA-11-DOS-50 | pouso de COMPETENCIA-PAGA-UM-JUIZ | app/docs/BACKLOG.md:1941 |
| SAIDA-NOTURNA-NO-DIA-CERTO | nada | app/docs/RELATO.md:839; app/docs/RELATO.md:333 |
| SELO-ESPERA-PROCESSO-CEGO-A-FIXTURE | ! novo aval SELO-ESPERA-ESTACIONA | app/docs/BACKLOG.md:2575 |
| SELOS-CANON-I3 | desenho | app/docs/RELATO.md:1489 |
| SONDA-SEM-GRANT | pouso de CODEX-2-RAIAS | app/docs/RELATO.md:1515 |
| O280 / V3-BATERIA | ! troca do motor V3 | app/docs/BACKLOG.md:1620 |

**Contagem do item conjunto:** `wt-quarentena-svv-151703` é cópia conjunta WIP, comprovada em **app/docs/RELATO.md:839**. Para não duplicar os 49 itens, atribuí-lo uma vez à QUARENTENA; a linha SVV conserva seu item nominal e referencia a cópia conjunta apenas como referência. Essa atribuição contábil é convenção **[nao verificado]**. SVV permanece obra própria e pousa junto da QUARENTENA. **app/docs/BACKLOG.md:838**.

**Absorção:** LASTRO-FOLGA está expressamente absorvida pelo RED pk5356 da LASTRO no topo, embora conserve entrada própria no BACKLOG. **app/docs/RELATO.md:4**, **app/docs/BACKLOG.md:1890**.

## 15/11 22:01 · sessao 7d68143a5daf · RESCALDO-CORTE-1511 · verificador · origem=Code

**(1) PROCEDE.** Dois itens de SAI são SPEC explícita de obras vivas:

| Item → obra viva | Prova |
|---|---|
| `wt-codex-lastro-faixa-sem-default-215253` → LASTRO-FAIXA-SEM-DEFAULT | “vira SPEC (…) nao base”; c13–c19 permanecem SPEC, enquanto a base nova recebe c3–c12. A obra está ABERTA. (`app/docs/RELATO.md:820`; `app/docs/RELATO.md:822`; `app/docs/BACKLOG.md:1895`) |
| `wt-silencio-2810-0915` → QUARENTENA-DO-COLAB + SILENCIO-VENCIDO-VOLTA | A célula nomeia essa cópia; a retomada manda “reaplica a copia antiga como SPEC em copia nova do HEAD”. A sucessora documentada contém somente RED não rodado. (`app/docs/BACKLOG.md:839`; `app/docs/BACKLOG.md:2586`; `app/docs/RELATO.md:839`) |

Isso exige preservar e nomear o material nas sucessoras; **não demonstra necessidade de conservar as worktrees**, pois D permite expressamente SPEC como ref nomeada. (`aval.txt:1`, D)

Não confundi ancestralidade com base atual: E3 antiga foi reaplicada e já tem sucessora c16b/c17; LASTRO c3–c12 foi transportada à base nova. (`app/docs/RELATO.md:320`; `app/docs/RELATO.md:12`; `app/docs/RELATO.md:823`)

A F1 c9 também tem guarda expressa em `refs/guardadas/e4c-f1-c9`; seu uso como base/SPEC da construção nova é **[nao verificado]**. (`app/docs/RELATO.md:4`; `app/docs/BACKLOG.md:1945`)

**(2) PROCEDE quanto às lacunas que impedem liberar integralmente SAI.** Existência de commit útil exclusivo ou perda efetiva: **[nao verificado]**; faltam histórico Git e os diffs integrais indicados abaixo. (`censo_1a4.r`, preâmbulos e ITEMs respectivos) [nao verificado]

**Nenhum dos 29 NO-MAIN apresenta prova exclusivamente documental**: todos citam código; portanto há **0** casos “só doc/RELATO” nesse grupo. Isso comprova os comportamentos indicados, sem comprovar todo conteúdo sujo. (`censo_1a4.r`, 29 ITEMs NO-MAIN)

| Item a suspender do SAI | Lacuna |
|---|---|
| `wt-codex-bo-disputa-sem-pergunta-221357` | Produção/testes/entrega c22 omitidos; sucessora c25 não comprova absorção desse sujo específico. (`censo_1a4.r`, ITEM correspondente; `app/docs/CORTES.md:455`) |
| `wt-codex-lastro-faixa-sem-default-215253` | Produção/teste c20 omitidos; c20 terminou sem commit, com parcial guardada. (`censo_1a4.r`, ITEM correspondente; `app/docs/RELATO.md:820`) |
| `wt-feriado-092818` | Produção/testes omitidos; “SUPERADA” decide a substituição, sem comparar integralmente o sujo. (`censo_1a4.r`, ITEM correspondente; `app/docs/RELATO.md:739`) |
| `wt-e4b-pouso-065207` | Produção/backfill com equivalência integral expressamente não verificada. O comportamento citado existe no main. (`censo_1a4.r`, ITEM correspondente; `app/chamados/services/fecho_runtime.py:146`) |
| `wt-merge-e4b-214239` | Integração de produção/testes c10 com inventário incompleto; há identificação da integração e pouso posterior, sem comparação integral apresentada. (`censo_1a4.r`, ITEM correspondente; `app/docs/RELATO.md:1053`; `app/docs/RELATO.md:998`) |
| `wt-hj090-v6m-204526` | Cura em command/services/views parcialmente cortada. O transporte de `hoje` existe, mas o restante permanece desconhecido. (`censo_1a4.r`, ITEM correspondente; `app/ferias/management/commands/realinhar_concedidos_ferias.py:114`; `app/ferias/services.py:220`) |
| `wt-e3-mig-171026` | “Restante do diff cortado”: falta identificar seu conteúdo; não há prova de que seja produção adicional. (`censo_1a4.r`, ITEM correspondente) |
| `wt-e3-mesclada-121238` | Identidade de todos os deltas cortados não demonstrada; pouso posterior da E3 não resolve sozinho essa lacuna. (`censo_1a4.r`, ITEM correspondente; `app/docs/RELATO.md:545`; `app/docs/RELATO.md:333`) |

As mutações de `ens-dupla-mut-212030` e `ens-orfao-mut-210938` estão identificadas como ensaio; não são produção desconhecida. Entretanto, o relato apenas promete incorporar os testes, e registra o ramo S111 do ÓRFÃO como não exercitado. Incorporação integral desses REDs em sucessora: **[nao verificado]**; devem entrar nominalmente na guarda C. (`app/docs/RELATO.md:1046`; `app/docs/RELATO.md:1056`; `app/docs/RELATO.md:1058`; `aval.txt:1`, C)

**(3) NAO PROCEDE quanto a contradição demonstrável no material disponível.** O cotejo integral com os cinco avais nomeados é **[nao verificado]**: faltam os textos de `logs/AVAIS-1515/`; não há como citar literalmente os dois lados de uma contradição com esses documentos ausentes. **[nao verificado]**

Os pares literais acessíveis são compatíveis:

| Aval proposto | Texto vigente disponível |
|---|---|
| “NAO mexe na ORDEM-VIVA-1511-NOITE nem no teto 2”. (`aval.txt:1`) | “teto 2 construtores e DESPACHO valem como escritos”. (`app/docs/RELATO.md:25`) |
| “instrumento por ultimo com pouso proprio”. (`aval.txt:1`, B) | “produto pousa primeiro, instrumento em pouso proprio”. (`app/docs/CORTES.md:289`) |
| “SO REGISTRAR, nao construir”. (`aval.txt:1`, F) | Não concede pouso à FRAC5; permanece a determinação “NAO pousam: ficam guardadas com ref”. (`app/docs/CORTES.md:426`; `aval.txt:1`, D) |

O topo antigo que exige novo aval para R3 c17/F1 não prova conflito com `CONSTRUIR-R3-C17-E-F1-NOVA`, cujo texto posterior não foi fornecido. (`app/docs/RELATO.md:4`; conteúdo e alcance do aval posterior **[nao verificado]**)

**(4) PROCEDE na mistura de unidades; os totais originais por classe fecham.**

| Conferência | Conta e conclusão |
|---|---|
| Classes das quatro faixas | NO-MAIN: `7+8+8+6=29`; MORTA: `12+6+7+10=35`; PARADA: `10+14+13+12=49`; total `113`. (`censo_1a4.r`, subtotais das quatro faixas) |
| Destinos originais | `7 POUSA +42 GUARDA +64 SAI=113`; `29+35=64`. (`quadro.md`, resumo e tabela) |
| Após triagem | `5 POUSA +44 GUARDA +64 SAI=113`: correto **por linhas**. Esses cinco registros representam **quatro obras**, pois JUSTIFICATIVA aparece duas vezes. (`quadro.md`, triagem final) |
| Em voo | **7 registros =6 cópias +1 célula**, representando **6 obras**; BUG-ENC aparece como worktree e como célula. (`quadro.md`, EM VOO) |
| Universo | `80+25+15=120` registros; `113+7=120`. Não são 120 worktrees: a decomposição fornecida corresponde a **105 cópias +15 células**, sendo **99 cópias +14 células no censo**. (`quadro.md`, universo e EM VOO; `bin/raias_estado.py:132`; `bin/interrompidas_estado.py:134`) |
| 49 PARADA | Há **35 cópias +14 células**. Agrupando os nomes das obras, resultam **37 obras identificadas +2 registros sem obra identificada** (`wt-ui`, `recon_onprem`); não “49 obras”. (`quadro.md`, 49 ITEMs PARADA) |

LASTRO-FOLGA conserva ID próprio ABERTA no BACKLOG, embora o RELATO registre absorção pela LASTRO-FAIXA; uni-las reduziria as 37 identificadas para 36, mas essa consolidação precisa ficar explícita. O total real de obras contidas nas duas cópias não identificadas permanece **[nao verificado]**. (`app/docs/BACKLOG.md:1890`; `app/docs/RELATO.md:4`; `quadro.md`, ITEMs `wt-ui` e `recon_onprem`)

`wt-ui` e `wt-v3` permanecem worktrees guardadas. `recon_onprem` tem material GUARDA, mas o aval manda retirar sua worktree: é **uma remoção adicional**, fora dos 64 SAI. As três declarações são **três arquivos**, sem equivaler a três remoções de worktree; o medidor lê essas declarações separadamente. (`aval.txt:1`, D; `quadro.md`, declarações; `bin/raias_estado.py:51`; `bin/raias_estado.py:120`)

A contagem de refs deve usar o inventário nominal das **99 cópias do censo**, distinguindo células que remetem às mesmas cópias; quantidade de tars depende de quais cópias estão sujas, não de 113/120 registros. A quantidade exata necessária de tars é **[nao verificado]**. (`aval.txt:1`, C; `quadro.md`, universo e ITEMs)

**Lista final — tirar do SAI imediato, até resolver as lacunas da pergunta 2:** os oito itens da tabela daquela pergunta, pelos motivos ali documentados. Isso deixa **56 candidatos a SAI**, com **8 em guarda por revisão**; não altera suas classes originais. (`censo_1a4.r`, oito ITEMs; `aval.txt:1`, D)

**Correções numéricas:** `64−8=56` SAI; após a triagem e essa suspensão, `56 SAI +52 GUARDA +5 POUSA=113` registros. POUSA significa **5 registros/4 obras**; em voo significa **7 registros/6 obras**; PARADA significa **49 registros**, não 49 obras. As duas SPEC exigem vínculo nominal nas sucessoras; sua retirada física continua condicionada à guarda C, sem obrigação demonstrada de conservar as worktrees. (`quadro.md`, triagem final e EM VOO; `aval.txt:1`, C–D; `app/docs/RELATO.md:820`; `app/docs/BACKLOG.md:2586`)

## 15/11 22:01 · sessao e88de9ae1a7b · - · - · origem=Ronald (tmux)

Consolidação das obras de `emvoo.txt` e material da tarde; sobreposições com o quadro: E4C-F2, LOTE7/FATO-DE-AUSENCIA-ACORDA-A-PROJECAO e BUG-PAR devem conservar uma única linha por obra. As duas primeiras correspondências são explícitas no dado; BUG-PAR é associado à obra longa por `app/docs/RELATO.md:35-36`.

| obra | estado sugerido | DESTRAVA | fonte e próxima ação | ref disponível | material permitido |
|---|---|---|---|---|---|
| BUG-ENC-CELULAS-FORMA | EM VOO — opus final | nada | Aguardar o opus final sem tocar a cópia; estado vem do anexo [nao verificado]. A interrupção antiga está defasada segundo o anexo; histórico em `app/docs/BACKLOG.md:988`. | `wt-bug-enc-201609@380d20c27` — anexo [nao verificado] | `logs/ORDEM-VIVA-1511/BUG-ENC/censo.r` |
| E3-RESTO-R3 | EM VOO — c17 retida | ! novo aval R3 c17 | Obter aval novo citando a entrega do predesenho antes da c17 (`app/docs/RELATO.md:4`, `app/docs/RELATO.md:23`). | `wt-codex-e3-resto-r3-093851@f31a8d09e` — `app/docs/RELATO.md:23` + anexo [nao verificado] | `logs/E3-RESTO-R3/c17/predesenho.md`; `logs/E3-RESTO-R3/c16/censo_fronteira.r`; `logs/E3-RESTO-R3/matriz/matriz.md` |
| E4C-F1-NOVA-JUIZ-RESPONDIDA | EM VOO — B1 no anexo | ! novo aval F1 nova | Obter aval novo citando o predesenho; lei JUIZ-RESPONDIDA já cortada (`app/docs/BACKLOG.md:1945`; `app/docs/RELATO.md:4`). | Commit da nova `wt-f1nova-b1-203152` [nao verificado]; `d36ecbddd` pertence à c9 guardada, não provar head da nova (`app/docs/RELATO.md:4`). | `logs/E4C/f1nova/predesenho.md`; `logs/JUIZ-RESPONDIDA/censo.md`; `logs/E4C/matriz/matriz.md` |
| FERIADO-USA-A-LEI-DE-1609 / FERIADO-G | EM VOO — PAREI 3(ii) | ! FERIADO-G item 3(ii) | Aguardar Ronald para a condição 3(ii), que manda não construir (`app/docs/RELATO.md:7-8`). | `wt-feriado1609-110424@adb3f954c` (`app/docs/BACKLOG.md:729`). | `logs/FERIADO-ESCALA/censo/censo.r` é censo do escopo anterior; **não há matriz/predesenho/censo/brief da lei 1609 na lista**. Melhor usar só ref e `brief: BACKLOG:729`. |
| HE-REVOGADA-NA-PORTA | EM VOO — pre-opus | nada | Aguardar suite pre-opus e opus final; pouso na 11 aberta não espera LU2-04 (`app/docs/RELATO.md:171`). | `wt-he-revogada-095943@019e5157a` (`app/docs/RELATO.md:186`). | Nenhum matriz/predesenho/censo/brief listado para esta retomada; os `.r` disponíveis são preverificações. Usar ref + brief BACKLOG com linha ainda a localizar [nao verificado]. |
| SITUACAO-DESLIGADO / LOTE 3 desligado | EM VOO — c6 na fila | ! VINCULO | Concluir os dois procedentes da c6 e obter ! de VINCULO para o pouso; c6/procedentes no anexo [nao verificado], ! em `app/docs/RELATO.md:82`, `app/docs/RELATO.md:180`. | Commit c6 [nao verificado]; último explícito na documentação é c5 `874fb76d1` (`app/docs/RELATO.md:82`). | Nenhum matriz/predesenho/censo/brief listado para a retomada; `logs/RETOMA-HE-LOTE3/pv_lote3.r` existe, mas é preverificação. |
| E4C-F2 | PARADA — matriz entregue | lei F2 L1/L2 | Cortar efeito/face SQL e retomar após F1; a matriz mantém duas perguntas abertas (`app/docs/RELATO.md:17`, `app/docs/RELATO.md:33`). | Base do quadro `wt-e4c-f2-103933@8c6577245` (`app/docs/RELATO.md:89`). | `logs/ESPINHA-BRIEFS/F2/matriz.md` |
| E4C-F3 | PARADA — matriz entregue | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Retomar a fatia após o pouso da F1 (`app/docs/RELATO.md:17`, `app/docs/RELATO.md:33`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/ESPINHA-BRIEFS/F3/matriz.md`; `logs/ESPINHA-BRIEFS/F3/brief.md` |
| E4C-F4 | PARADA — duas leis | lei F4 absorção e guardas de limbo | Cortar absorção pelo ato e guardas de limbo antes da construção após F1 (`app/docs/RELATO.md:17`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/ESPINHA-BRIEFS/F4/matriz.md`; `logs/ESPINHA-BRIEFS/F4/brief.md` |
| E4C-F5 | PARADA — lei respondida | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Readequar a fatia ao corte JUIZ-RESPONDIDA após o pouso da F1 (`app/docs/RELATO.md:13`, `app/docs/RELATO.md:17`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/ESPINHA-BRIEFS/F5/matriz.md`; `logs/ESPINHA-BRIEFS/F5/brief.md` |
| E4C-F6 | PARADA — lei residual | lei F6 aguarda_admin nos estados de absorção | Resolver a lei residual e construir após F1; a segunda lei foi respondida (`app/docs/RELATO.md:17`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/ESPINHA-BRIEFS/F6/matriz.md`; `logs/ESPINHA-BRIEFS/F6/brief.md` |
| E4C-F7 | PARADA — matriz entregue | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Retomar após F1, respeitando também F2 e BUG-PAR e o smoke de pouso (`app/docs/RELATO.md:33`, `app/docs/RELATO.md:149`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/ESPINHA-BRIEFS/F7/matriz.md`; `logs/ESPINHA-BRIEFS/F7/brief.md` |
| E4C-F8 | PARADA — lei residual | lei F8 V legada com R sintética posterior | Cortar se V legada permanece válida após R sintética e construir após F1 (`app/docs/RELATO.md:17`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/ESPINHA-BRIEFS/F8/matriz.md`; `logs/ESPINHA-BRIEFS/F8/brief.md` |
| E4C-F9 | PARADA — matriz entregue | pouso de E4C-F1-NOVA-JUIZ-RESPONDIDA | Retomar após o pouso da F1 (`app/docs/RELATO.md:17`, `app/docs/RELATO.md:33`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/ESPINHA-BRIEFS/F9/matriz.md`; `logs/ESPINHA-BRIEFS/F9/brief.md` |
| FATO-DE-AUSENCIA-ACORDA-A-PROJECAO / LOTE7 | PARADA — c6 anunciada | desenho | Conferir a liberação “B3 LOTE7 DESENHO” indicada no anexo antes da c6; o código só registra pergunta aberta da c5 (`app/docs/RELATO.md:59`, `app/docs/RELATO.md:159`); teor da liberação [nao verificado]. | Base do quadro `wt-lote7-ausencia-100250@22677d0db` (`app/docs/RELATO.md:116`, `app/docs/RELATO.md:134`); commit c6 [nao verificado]. | `logs/ESPINHA-BRIEFS/MATRIZ-LOTE7/entrega.md` |
| BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS | PARADA — predesenho entregue | lei PRE-DESENHO-BUG-PAR-VOLTA | Cortar a regra do universo 163 e o alcance exportado; volta somente por aval novo depois de R3 (`app/docs/RELATO.md:18`). | `wt-bugpar-main-095024@6e2fc9133` — quadro [nao verificado]; commit/documentação em `app/docs/RELATO.md:85`. | `logs/ESPINHA-BRIEFS/MATRIZ-C7-BUGPAR/entrega.md`; `logs/BUG-PAR-O-PAR/predesenho/entrega.md`; `logs/BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS/brief.md` |
| PAR-CONFIRMADO-PELO-SUPERVISOR | PARADA — só registrada | lei PAR-CONFIRMADO perfil e juiz | Cortar perfil e juiz novo antes de construir (`app/docs/RELATO.md:39`; `app/docs/BACKLOG.md:744`). | Nenhuma cópia/item/commit fornecido [nao verificado]. | `logs/PAR-CONFIRMADO/censo.md` |

Pontos para a consolidação final:

- As seis cópias explicitamente listadas sob “EM VOO” devem permanecer distintas das obras que só têm material entregue; classificá-las EM VOO preserva o anexo, com a retenção na nota [nao verificado].
- Para novas obras sem cópia-base, a exigência `ref <item>@<commit>` não pode ser satisfeita literalmente sem marcar os campos ausentes `[nao verificado]`. [nao verificado]
- Mudança segura em relação ao quadro: **BUG-PAR**, de “desenho e pouso R3” para **lei PRE-DESENHO-BUG-PAR-VOLTA**, preservando em próxima ação o pouso R3 e novo aval (`app/docs/RELATO.md:18`).
- Em **LOTE7**, não encontrei no código o teor de “libera B3 LOTE7 DESENHO”; tratar isso como liberação já concedida e mudar para `nada` exige inferência `[nao verificado]`.

## 15/11 22:01 · sessao 813940632a06 · - · - · origem=Ronald (tmux)

Conferência das primeiras 22 linhas, somente leitura:

| obra | destrava recomendado | evidência / observação |
|---|---|---|
| CODEX-CONTEXTO-DA-CASA | nada | Briefs pendentes; censo registra zero lei — app/docs/BACKLOG.md:1095; app/docs/RELATO.md:578. |
| JUSTIFICATIVA-PAGA-O-PERIODO | pouso de COMPETENCIA-PAGA-UM-JUIZ | app/docs/BACKLOG.md:1948; base `74ec17605`, suíte inteira registrada — app/docs/RELATO.md:211. |
| DATA-EVENTO-CRU | desenho | Matriz obrigatória após opus — app/docs/RELATO.md:1059. Base `wt-catraca-dec-151606@3e86f61fe`, cura v4 — app/docs/RELATO.md:1067; app/docs/RELATO.md:9123. Próximo verificador também exige rearme Ronald — app/docs/RELATO.md:1067. |
| CODEX-2-RAIAS | nada | Construção/provas restantes — app/docs/RELATO.md:1488; app/docs/RELATO.md:1503. |
| FRAC5-MINIMO-5 | ! novo corte FRAC5-MINIMO-5 | Pouso somente por corte novo — app/docs/BACKLOG.md:788. Normaliza a cópia RED que dizia `lei`; base `wt-frac5-cura-100047@b7d6b6aed` — app/docs/BACKLOG.md:788. |
| FATO-DE-AUSENCIA-ACORDA-A-PROJECAO / LOTE7 | desenho | Quadro fornecido; substituição efetiva por c6 [nao verificado]. |
| COBPARC-FILA-RELOGIO | nada | Censo posterior: lei zero, padrão L-128; construção/provas pendentes — app/docs/RELATO.md:582; app/docs/BACKLOG.md:916. |
| LASTRO-DEGRAU-2 | nada | Dependência histórica JUIZ-DO-PAR consta pousada — app/docs/RELATO.md:3345; censo lei zero — app/docs/RELATO.md:582. Cura do construtor permanece — app/docs/BACKLOG.md:1814. |
| O10-C | nada | Desenho da UI decidido — app/docs/RELATO.md:1987. `!` da competência 09 aplica-se ao apply posterior — app/docs/RELATO.md:1991; app/docs/RELATO.md:1992. |
| O275 | lei FORMA A x FORMA B | app/docs/BACKLOG.md:1615; app/docs/RELATO.md:582. |
| OTIMIZA-1 | pouso de SUITE-POR-IMPACTO | app/docs/BACKLOG.md:2197. |
| PADRAO-DE-CIMA | desenho | app/docs/RELATO.md:2494; app/docs/RELATO.md:578. |
| QUARENTENA-DO-COLAB | desenho | 16x15 janelas; reabre depois do pouso E3 — app/docs/BACKLOG.md:2586. Base `c9a2a3846` explicitada ali. |
| SILENCIO-VENCIDO-VOLTA | pouso de QUARENTENA-DO-COLAB | Obra própria, pouso conjunto — app/docs/BACKLOG.md:838. Não duplicar o item conjunto `wt-quarentena-svv-151703` na soma [nao verificado: regra de atribuição a decidir na consolidação]. |
| SUITE-POR-IMPACTO | ! aval PRONTO-5 | Mantém a trava do quadro: rc2 é aval, não lei — app/docs/RELATO.md:556. Há também corte pendente do gate pela classe e entrada após E3 — app/docs/BACKLOG.md:1093; app/docs/RELATO.md:665. |
| V3-E1 | lei V3-E1 C06/C16/24x48 | app/docs/RELATO.md:556; pergunta DP em app/docs/BACKLOG.md:953. |
| V3-RETOMA-FILA | pouso de V3 | app/docs/BACKLOG.md:1033. |
| V3-RETOMA-PERIODOS-DA-CELULA | lei período não guardado na célula | app/docs/BACKLOG.md:1021; app/docs/RELATO.md:2497. |
| LASTRO-FAIXA-SEM-DEFAULT | pouso de TIPO-NA-FONTE | Base escolhida explicitamente `wt-codex-lastro-redesenho-164050@f3ebd821b` — app/docs/BACKLOG.md:1895. |
| obra não identificada — recon_onprem | nada | Trava não identificada [nao verificado]; árvore anterior à LEI-AKITA e inadequada ao pouso — app/docs/RELATO.md:328. |

A cópia não possui `.git`; comparação direta das datas dos commits não foi possível [nao verificado]. Usei as bases explicitadas nos documentos; `70dfbc62a` não apareceu na busca em BACKLOG/RELATO [nao verificado].

## 15/11 22:00 · sessao 23881c6d2de5 · - · - · origem=Ronald (tmux)

**Q2: PROCEDE quanto à insuficiência de prova para liberar integralmente o SAI; existência de perda definitiva ou de commit útil exclusivo é [nao verificado].** O censo informa sujeiras integrais omitidas; a cópia local não contém `.git`, portanto ancestralidade/identidade dos commits não foi verificável. [nao verificado]

**Nenhum dos 29 NO-MAIN tem prova exclusivamente documental:** todos citam código. A distribuição é 5 CADASTRO + 3 PERGUNTA-DUPLA + 3 ÓRFÃO + 1 FIO-E3 + 3 E4B + 2 UI + 1 DESLIGADO + 1 E3-MIG + 4 HJ090 + 1 HJ091 + 2 O168 + O320 + PLACAR + RÉGUA = 29 (anexo `censo_1a4.r`, respectivos ITEMs). Isso comprova apenas os comportamentos citados, não equivalência integral dos sujos.

**Itens com produção suja integralmente não demonstrada, a suspender do SAI até identificação:**

| ITEM | Lacuna e evidência disponível |
|---|---|
| `wt-codex-bo-disputa-sem-pergunta-221357` | Sujo produção/testes/entrega c22, conteúdo integral omitido. Sucessora c25 documentada, sem prova de absorção desse sujo específico (`censo_1a4.r:ITEM=...`; `app/docs/CORTES.md:455`, `app/docs/RELATO.md:371`). |
| `wt-codex-lastro-faixa-sem-default-215253` | Sujo produção/teste c20 omitido. Main registra c20 **sem commit**, parcial guardada; c13–c19 são SPEC, enquanto apenas c3–c12 foram reaplicadas (`app/docs/RELATO.md:820`, `:822`, `:823`; anexo ITEM). |
| `wt-feriado-092818` | Produção/testes omitidos. SUPERADA é decisão expressa, não comparação integral do sujo (`app/docs/RELATO.md:739`; anexo ITEM). |
| `wt-e4b-pouso-065207` | Produção/backfill com equivalência integral expressamente não verificada no anexo ITEM. O main contém a porta/backfill (`app/chamados/services/fecho_runtime.py:146`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:133`). |
| `wt-merge-e4b-214239` | Integração de produção/testes c10, inventário integral omitido. Identificação da árvore intermediária em `app/docs/RELATO.md:1053`; pouso posterior em `:998`; porta comum em `app/chamados/services/fecho_runtime.py:146`. |
| `wt-hj090-v6m-204526` | Cura produção command/services/views, restante cortado. Main confirma transporte do relógio (`app/ferias/management/commands/realinhar_concedidos_ferias.py:114`, `:132`; `app/ferias/services.py:220`), sem comprovar todo o sujo (anexo ITEM). |

`wt-e3-mig-171026` também exige esclarecimento do “restante do diff cortado”; o dado identifica adaptações de testes/mutantes, mas não permite afirmar que o restante seja produção. O comportamento citado existe (`app/chamados/services/tarefa_do_dia.py:596`, `:613`; anexo ITEM).

**REDs com preservação/absorção não demonstrada:**

- Três PERGUNTA-DUPLA: `ens-dupla-head-212030`, `ens-dupla-mut-212030`, `wt-codex-pergunta-dupla-211041`. Main confirma dia do turno sem +1 (`app/chamados/services/disputa_emissao.py:433`, `:460`); mutação identificada em `app/docs/RELATO.md:1046`. O mesmo trecho apenas promete pouso futuro dos testes.
- Três ÓRFÃO: `ens-orfao-head-210938`, `ens-orfao-mut-210938`, `wt-codex-bug-orfao-com-par-210341`. A guarda existe (`app/api/views_core.py:628`), mas o próprio relato diz “INCOMPLETO: ramo S111 [...] nao exercitado pelo ensaio” (`app/docs/RELATO.md:1056`). Testes prometidos no comboio em `:1046`; incorporação em sucessora [nao verificado].
- Busca `rg --files` não encontrou os dois testes nominais nas localizações citadas no RELATO; conteúdo em sucessora [nao verificado]. Isso não demonstra regressão no main. [nao verificado]

**Não classificar como produção desconhecida:** as mutações de `ens-dupla-mut-212030` e `ens-orfao-mut-210938` são intencionais e identificadas (`app/docs/RELATO.md:1046`, `:1058`). `wt-codex2raias-v3-151541` tem descarte expressamente autorizado, parcial guardada e proibição de servir de base (`app/docs/RELATO.md:1501`). `wt-placar-grade-210341` é instrumento identificado, cujo comportamento está no main (`bin/placar_grade.py:173`, `:198`; `bin/relato.sh:101`, `:128`; `app/docs/RELATO.md:589`).

A guarda C do aval preserva material, mas não transforma ausência de comparação em prova de absorção (`aval.txt:1`, cláusula C; comparação dos sujos [nao verificado]).

_5830 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
