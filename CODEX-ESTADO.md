# CODEX-ESTADO

> Ultima resposta processada: **22/11/2026 00:36** · publicado em **22/11/2026 00:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 22/11 00:36 · sessao 1aee7c6e498d · - · - · origem=Ronald (tmux)

- **Posto.** `Posto.opera_em_feriado` é triestado, com `None` para marca ausente. A escrita explícita é `estrutura.marcar_opera_em_feriado`, chamada pela porta da marca. A edição comum recusa a coluna e limita `update_fields`. Leem a marca o veto de emissão, a presença/sugestões, o nascimento da chave, os contadores, o sync, os segmentos da marca e a CLI. `None` deixa a cobrança seguir, fica elegível para sugestão e, no nascimento, permite decisão pelo cargo/tipo ou herança conforme a porta. [app/colaboradores/models.py:256] [app/colaboradores/services/estrutura.py:66–95] [app/colaboradores/services/feriado_do_posto.py:60–71,95–119,128–145,192–204,226–237,280–293,322–337,343–366] [app/ponto/management/commands/detectar_ausencias.py:616–630] [app/escala/models.py:905–944] [app/colaboradores/management/commands/sincronizar_feriado_admin.py:39–55] [app/colaboradores/management/commands/marcar_feriado_do_posto.py:130–143]
- **Vínculo.** A alteração explícita da flag ficou em `marcar_trabalha_em_feriado`, e os nascimentos ou cópias a inicializam por juiz, herança ou `chave_ao_nascer`. Existe um segundo escritor **indireto e preexistente**. `executar_vinculo` recebe o objeto obtido sem trava, atualiza posto, template e vigência e faz `existente.save()` inteiro. Pode repor a flag antiga depois de uma alteração concorrente pela porta. Leitores de negócio passam por `feriado_vira_folga` no gerador, precedência, alimentação do motor e fechamento. A flag nasce em todos os ciclos, mas só muda o previsto nos weekday. O veto por marca do posto é separado e pode alcançar vínculo de outro ciclo. [app/colaboradores/services/vinculo.py:100–103,147,186,274–299,898–908,983–987,1123–1129,1148–1155] [app/colaboradores/views.py:429–442] [app/escala/models.py:896–928,1378–1406] [app/ponto/precedencia.py:357–387] [app/ponto/calculador/alimentacao.py:521] [app/ponto/services/fechamento.py:940–949] [app/ponto/management/commands/detectar_ausencias.py:616–630]
- **Célula.** Geração, regeneração, edição e reversão escrevem `trabalha`, `dna`, `escala_geradora` e origem no arquivo da porta. `montar_dna` é o produtor. A deleção de vínculo pode zerar indiretamente a geradora por `SET_NULL`; o restaurador de snapshot recusa fazê-lo para editadas. Leem os campos o próprio serviço da marca, o juiz do vínculo, a escala para trabalho/marcos/pausas/extras, a precedência, o juiz de dia sem trabalho, cartório, folha por ata e vigia de futura. `trabalha=None` é indeterminado. O leitor de presença exclui `None` e `False`; o juiz de cobrança só cala por `False` ou ata de natureza sem trabalho, observada a exceção de cobertura parcial. [app/escala/models.py:1094–1108,1135,1200,1246,1692–1713] [app/ponto/management/commands/gerar_celulas.py:73–132] [app/ponto/portas/celula.py:251–318,637–674,834–868,881–944] [app/colaboradores/services/vinculo.py:1030–1043] [app/colaboradores/services/feriado_do_posto.py:95–119,423–452] [app/escala/alimentacao.py:228–255,350–358] [app/ponto/precedencia.py:357–387] [app/chamados/juizes.py:451–460] [app/ponto/services/cartorio.py:1280–1289,1427–1438] [app/escala/services/leitor_celula.py:350–385] [app/ponto/services/vigia_cadastro_futuro.py:21,43–87]
- **Carimbos e julgamento.** Regeneração e reversão escrevem `regenerada_em`, `regeneracoes`, `dna_anterior`; a reversão consome e limpa `dna_anterior`. A regravação residual passada continua sem os três carimbos. Edição escreve `editada_em/editada_por`. `lavrar_veredito` e `carimbar` escrevem ata, veredito e carimbos. `desfazer_julgamento_da_futura` limpa os campos do julgamento e preserva decisão administrativa com trilha. Leem `dna_anterior` a reversão, `aplicar_col900` e o passivo. Os carimbos são lidos pela cobrança órfã, ponte do dia, cartório, fase 12x36, lavra de perguntas e passivo. O contador `regeneracoes` é consumido no incremento; outro leitor de negócio [nao verificado]. [app/ponto/portas/celula.py:251–279,297–315,643–657,834–868,1080–1125,1130–1131,1167–1255] [app/docs/LEIS.md:165] [app/ponto/management/commands/aplicar_col900.py:175] [app/chamados/management/commands/materializar_passivo_validado.py:217–225] [app/escala/services/regeneracao.py:72–126,141–155] [app/ponto/services/dia_do_colab.py:91–95] [app/ponto/services/cartorio.py:409–414] [app/escala/services/fase_12x36.py:189–190] [app/chamados/veredito_lavrado.py:184–194]
- **Trilhas.** A marca chama `evento` com PK do posto, usuário, antes/depois e motivo e agora exige retorno da inserção. O `None` da auditoria desfaz a marca; `autor is None` recusa, porém um objeto sem PK ainda pode produzir linha sem `usuario`. Para a flag, `registrar_log` não testa o retorno de `evento`, e o sync não passa autor nem motivo. Os copiadores também emitem `editar` da flag ao nascer divergente. A auditoria genérica lê as duas famílias; sua UI exibe o evento, mas o seletor de ações não inclui `marcar_feriado_do_posto`, e a tela não mostra o ator de sistema nem os JSONs. O vigia de vigência considera qualquer linha do modelo por PK do vínculo **ou** do colaborador, então uma trilha que só prova a flag pode contar como lastro de vigência. A trilha de regeneração tem ação única com dois payloads preexistentes. A ida grava a janela pedida, e a volta grava `dna/revertida`. O leitor de janelas pula a volta sem datas. [app/colaboradores/services/feriado_do_posto.py:313–337] [app/core/observ.py:94–139] [app/core/auditoria.py:17–30] [app/colaboradores/services/vinculo.py:302–306,898–908,1097–1111] [app/colaboradores/management/commands/sincronizar_feriado_admin.py:76] [app/core/views.py:697–738] [app/core/models.py:285–307] [app/templates/core/log_auditoria.html:23–27,55–74] [app/escala/services/vigencia_sem_trilha.py:43–59,69–84] [app/ponto/portas/celula.py:373–387,864–868] [app/escala/services/regeneracao.py:58–67]
- **Semântica temporal.** `hoje` da CLI deriva de instante na zona ativa, configurada como São Paulo na base. Presença, feriados, vigência e regeneração são inclusivos nas pontas; futura é estritamente `> hoje`. Prévia usa o dia do turno para agrupar e a hora local do instante ao exibir. Retratação usa o juiz de dia do fato, que pode cair em data civil da batida e portanto divergir do dia do turno depois de meia-noite. Encerramento admite `agora >= fim`; fato novo no cartório usa `(desde, ate]`; cobrança órfã exige estrita posterioridade do instante e janela inclusiva de datas. O contador antigo 5x2/6x1 ainda usa `date.today()` na ausência de parâmetro. A troca compartilha `editada_em` e deriva dela o dia civil, enquanto o log tem `criado_em` próprio. [app/config/settings/base.py:93–95] [app/colaboradores/management/commands/marcar_feriado_do_posto.py:46–50] [app/colaboradores/services/feriado_do_posto.py:88–96,252–257,464–491] [app/core/feriados.py:27–29] [app/escala/alimentacao.py:214–225] [app/colaboradores/services/vinculo.py:912–924] [app/ponto/portas/celula.py:225–229,643–657,675–677,723–730] [app/ponto/services/cadastro_futuro.py:19–24,43–59] [app/ponto/turnos.py:43–54,2127–2130] [app/escala/servico_jornada.py:69–101] [app/chamados/juizes.py:175–231] [app/ponto/services/cartorio.py:409–414] [app/escala/services/regeneracao.py:108–126] [app/escala/models.py:931–944] [app/core/models.py:307]
- **Derivação de presença.** A presença chama `any(acesa)` de batida. O cartório acende lâmpada dispensada com `luz=None`, e o leitor de casadas desconta dispensadas. Há falsos comparecimentos possíveis, uma divergência preexistente que a guarda de relógio conserva. Outra divergência preexistente é a precedência poder classificar como feriado uma célula editada com `trabalha=True` quando a flag do vínculo manda folgar. Os novos cortes reutilizam os juízes registrados de vínculo, feriado e encerramento. [app/colaboradores/services/feriado_do_posto.py:84–87,95–119,463–478] [app/ponto/services/cartorio.py:800–832] [app/escala/services/leitor_celula.py:63–68,85–105] [app/ponto/precedencia.py:357–387] [app/core/juizes.py:190–196,243–248,310–313,1324–1325]
- **Travas.** Sync e marca adquirem `Posto` antes do `UPDATE` do vínculo. A leitura do vínculo ocorre sem trava, e a porta decorada com `atomic` compara o objeto recebido antes de adquirir implicitamente a trava na escrita. O sync também deixa de reaplicar o filtro de ativo/empresa na releitura do candidato. Há um ciclo possível por inspeção em PostgreSQL com a edição de vínculo. Marca/sync seguram `Posto P` e esperam vínculo `E`; outra transação fecha `E` e, para criar sucessor com FK para `P`, pode esperar a trava de chave do posto. A cadeia de código existe; ocorrência em produção [nao verificado]. [app/colaboradores/management/commands/sincronizar_feriado_admin.py:65–76,87–89] [app/colaboradores/services/feriado_do_posto.py:318–320,359–367] [app/colaboradores/services/vinculo.py:214–219,286–299,640–643,870–904] [app/escala/models.py:873] [app/config/settings/prod.py:9–11] [app/config/settings/saas.py:83–85]

## 22/11 00:36 · sessao ad615d266c10 · - · - · origem=Ronald (tmux)

**P1 corrige o despacho de R6, P2 corrige A2, P3 corrige A1 e a trilha do posto, P4 corrige A3 contra a declaração concorrente do mesmo posto e entrega A4, P5 corrige C2/C4 para PKs que chegam à validação e entrega C9 pela CLI.** A CLI ainda trata `--posto 0` como posto omitido e retorna antes de C2/C4. O cartório ainda lê o próprio dia e instante e toma decisões diferentes do despacho sob relógios divergentes. [app/ponto/portas/celula.py:392–405,467–481] [app/colaboradores/services/vinculo.py:898–925] [app/colaboradores/services/feriado_do_posto.py:74–89,137–153,313–337] [app/colaboradores/management/commands/sincronizar_feriado_admin.py:65–88] [app/colaboradores/management/commands/marcar_feriado_do_posto.py:29,49–75,111–127] [app/ponto/services/cartorio.py:2059–2089]

### Chamadores de produção na cópia

| Função | Chamadores e parâmetros |
|---|---|
| `presenca_em_feriado` | `sugestoes` passa `hoje`. O serviço também aceita as duas datas explícitas. [app/colaboradores/services/feriado_do_posto.py:74–89,141] |
| `sugestoes` | A CLI e `linhas_para_a_pauta` passam `hoje`. [app/colaboradores/management/commands/marcar_feriado_do_posto.py:46–50] [app/colaboradores/services/feriado_do_posto.py:153] |
| `linhas_para_a_pauta` | Zero chamadores executáveis encontrados para esta função. A agregação de pauta reúne homônimos de outras famílias. [app/colaboradores/services/feriado_do_posto.py:149–163] [app/escala/services/fase_12x36.py:267–274] |
| `aplicar_marca` | CLI, `reaplicar_marca` e `previa_da_marca` passam `autor`, `motivo` e `hoje`. [app/colaboradores/management/commands/marcar_feriado_do_posto.py:117–121] [app/colaboradores/services/feriado_do_posto.py:373,413–415] |
| `reaplicar_marca` | Zero chamadores executáveis encontrados. O comando contador a menciona na documentação. [app/colaboradores/services/feriado_do_posto.py:371–373] [app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:3,17–20] |
| `previa_da_marca` | A CLI passa `autor`, `hoje` e `agora`. Usuário inexistente entrega `None` e, na sombra, a porta recusa com `CommandError` traduzido pela CLI. A existência do login default em produção é [nao verificado]. [app/colaboradores/management/commands/marcar_feriado_do_posto.py:34,117–127] [app/colaboradores/services/feriado_do_posto.py:313–317,393–415] |
| `marcar_trabalha_em_feriado` | A propagação passa `hoje`, `autor` e `motivo`. O sync passa `hoje` e usa os defaults de autor e motivo. [app/colaboradores/services/feriado_do_posto.py:365–366] [app/colaboradores/management/commands/sincronizar_feriado_admin.py:76] [app/colaboradores/services/vinculo.py:871,906–908] |
| `regenerar_celulas_vinculo` | Todos passam `hoje`, incluindo o alias `_regen` e as importações via reexportação do comando. [app/colaboradores/services/vinculo.py:323,330–332,917–918,969–970] [app/escala/signals.py:73–75,125–127] [app/ponto/portas/regen_exportada.py:179–180] [app/colaboradores/management/commands/corrigir_escala_retroativa.py:231–234] [app/escala/services/cadastro_tipo.py:353–355] [app/escala/services/escala_auto_executor.py:107–108,147–148] [app/escala/folgas.py:293–296] [app/ponto/management/commands/gerar_celulas.py:13,323–325] [app/ponto/services/cadastro_futuro.py:58–59] [app/ponto/portas/celula.py:790–792] |
| `_o_chao_do_dia_mudou` | Regeneração, edição e reversão passam `hoje`. Edição converte `agora` em dia local. [app/ponto/portas/celula.py:392–394,675–677,875–877] |
| `reverter_regeneracao_dia` | `aplicar_col900` passa `hoje`. [app/ponto/management/commands/aplicar_col900.py:186–187] |
| `editar_dia` | `trocar_plantao` injeta o mesmo `agora` nos dois lados através de `_kw`. O parâmetro novo é opcional, mas a assinatura literal mudou. [app/ponto/portas/celula.py:579,643–649,723–730] |
| `trocar_plantao` | Agora exige `agora`. Zero chamadores executáveis encontrados fora de testes. A chamada histórica de lote só aparece na documentação. [app/ponto/portas/celula.py:681,699–700] [app/docs/RELATO.md:4521] | [nao verificado]

**Duas regressões de prova.** Executei 85 testes sem banco e sem geração de bytecode. Passaram 84. A única falha foi a catraca, com 42 leituras observadas frente a 43 esperadas. A diferença única é a entrada de `marcar_trabalha_em_feriado`, removida em P2. O mutante anterior `volta5_motivo` também perdeu sua âncora após P5 mover a resolução de usuário. A âncora tem zero ocorrências e o executor levanta antes de exercitar o teste. As sete âncoras novas de P1 batem exatamente uma vez. Os cenários Django de P1–P5 são [nao verificado]. [app/ponto/tests/test_cadastro_futuro_c5e.py:344,375,592–598] [app/colaboradores/services/vinculo.py:898–911] [app/colaboradores/tests/test_feriado_a5_volta3.py:343–350,392–402] [app/colaboradores/management/commands/marcar_feriado_do_posto.py:117] [app/colaboradores/tests/test_feriado_matriz_p1.py:55–84] [nao verificado]

**Concorrência.** Sync e marca travam o posto antes de escrever o vínculo. Ambos leem o vínculo sem `SELECT FOR UPDATE`, e a porta do vínculo compara o objeto recebido antes do `UPDATE`. O sync não revalida `ativa` nem a empresa no momento da escrita. O editor de vínculo pode mudar o posto e fazer `save()` integral de uma instância antiga, o que pode reaplicar uma chave antiga. Um ciclo de deadlock entre os dois caminhos comparados e a ocorrência da corrida em produção são [nao verificado]. [app/colaboradores/management/commands/sincronizar_feriado_admin.py:65–76,87–101] [app/colaboradores/services/feriado_do_posto.py:318–320,357–366] [app/colaboradores/services/vinculo.py:259–284,870–871,900–908]

**Duas divergências anteriores.** `dias_do_fato`, usado pela retratação, parseia `data_turno` diretamente, enquanto `dia_do_fato` pergunta primeiro à chave de data declarada do catálogo. O fallback da batida usa dia civil local e o de carimbo usa a data do offset codificado. Presença consulta a juíza do vínculo com lista vazia quando a célula não tem geradora, descartando essa célula sem oferecer candidatos da vigência para o fallback. [app/colaboradores/services/feriado_do_posto.py:95–107,241–257] [app/chamados/juizes.py:99–122,142–170,175–208] [app/core/juizes.py:289,1435] [app/escala/alimentacao.py:228–255]

Sem `.git` na cópia, a cadeia e a divisão efetiva por commit são [nao verificado]. Pelo diff anexado, P2 isolado quebra o sync anterior, que omite `hoje`, e P3 isolado quebra a prévia da CLI anterior, que omite `autor`. [diff.txt, hunk anterior do sync :69–72 e hunk anterior da CLI :117–123] [nao verificado]

## 22/11 00:35 · sessao 82464a7b064b · - · - · origem=Ronald (tmux)

**Suíte com regressão comprovada.** Rodei 64 testes sem banco, em somente leitura. Quatro chamadas legadas ainda omitem `hoje` e levantam `ValueError` antes de testar o comportamento original. A catraca falha com 42 leituras reais e teto 43, cuja única entrada excedente é o relógio extinto em `marcar_trabalha_em_feriado`. Os módulos de cadastro futuro sem banco e reversão somaram outros 55 testes, todos verdes. [app/ponto/tests/test_cadastro_futuro_c5b.py:124,138,144] [app/ponto/tests/test_cadastro_futuro_c3.py:114] [app/colaboradores/services/vinculo.py:898–899] [app/ponto/tests/test_cadastro_futuro_c5e.py:344,375,592–598] [app/ponto/tests/test_cadastro_futuro_sem_banco.py:445–447] [app/ponto/tests/test_reverter_regen_sem_banco.py:81–149] [nao verificado]

**Selo antigo quebrado pelo P5.** O mutante `volta5_motivo` busca a atribuição de autor com a indentação anterior. A atribuição mudou de bloco e a contagem literal é 0 onde o mutante exige 1. O runner levanta `RuntimeError` antes de iniciar o teste correspondente. [app/colaboradores/tests/test_feriado_a5_volta3.py:343–350,391–402] [app/colaboradores/management/commands/marcar_feriado_do_posto.py:117] [nao verificado]

**REDs contra o código anterior, por inspeção do diff.** Execução ORM de P1–P5 e execução contra o checkout base [nao verificado].

- **P1.** O cenário real da CLI acusa despacho indevido de D+1 e o inverso acusa ausência de despacho de D. A troca acusa carimbos diferentes com o helper antigo, porém o helper atualizado encontra `TypeError` antes da comparação ao chamar a assinatura antiga. Dos sete mutantes, todos os alvos literais estão presentes uma vez na cópia atual. Cinco dependem de trechos recém-criados e dão erro de preparação na base. Os outros dois já encontram seus alvos na base e aceitam qualquer `AssertionError`, inclusive de uma pré-condição, portanto seu sucesso isolado não prova o ponto da mutação. [app/colaboradores/tests/test_feriado_matriz_p1.py:55–86,102–161,169–203] [app/ponto/tests/test_o10b_editar_dia_troca.py:110–117] [r.txt:103] [diff.txt, hunk de celula.py:461]
- **P2.** As duas recusas mordem a base, que grava e usa fallback no divergente ou retorna no idempotente. A inspeção AST acusa os dois `del` antigos. [app/colaboradores/tests/test_feriado_matriz_p2.py:29–43,58–62] [app/colaboradores/tests/test_feriado_do_posto.py:111–127] [diff.txt, hunks de vinculo.py:890 e test_feriado_do_posto.py:110]
- **P3.** Os dois REDs de presença, o de sugestões e o da pauta exigem exceções que não existiam. Autor `None` passava, e a falha de trilha era engolida. Os dois REDs da API de prévia encontram `TypeError` por keyword indisponível na base antes de comprovar o repasse ou a recusa. O controle de intervalo explícito só verifica uma consulta vazia. [app/colaboradores/tests/test_feriado_matriz_p3.py:25–49,55–82,96–110] [app/core/observ.py:85–105,133–139] [diff.txt, hunks de feriado_do_posto.py:79,304,318,367]
- **P4.** A3 morde a lista antiga e exige que a costura dispare, mas passa com releitura sem trava. A4 morde falta de `hoje=` e passa com múltiplas leituras no mesmo dia congelado. [app/colaboradores/tests/test_feriado_matriz_p4.py:37–65] [diff.txt, hunk de sincronizar_feriado_admin.py:66]
- **P5.** Os quatro testes de C2/C4 exigem `CommandError` onde a base retornava. A espia de autor veria `None`, e o login inexistente era ignorado no dry-run. [app/colaboradores/tests/test_feriado_matriz_p5.py:26–69] [diff.txt, hunks de marcar_feriado_do_posto.py:68,111] [nao verificado]

**Fatiamento e pouso.** A ordem nominal cria dois estados intermediários incompatíveis. P2 antes de P4 deixa o sync antigo chamar o vínculo sem `hoje`. P3 antes de P5 deixa a CLI antiga chamar a prévia sem `autor`. O aval prevê suíte e opus após P5. A condição anterior `sombra_c2b = 0` segue documentada. Hash Git, commits individuais, execução da condição e pouso real [nao verificado]. [aval.txt:1] [app/docs/BACKLOG.md:1966] [app/docs/RELATO.md:1346,1366] [app/colaboradores/services/vinculo.py:898–899] [app/colaboradores/services/feriado_do_posto.py:384] [diff.txt, hunks de sincronizar_feriado_admin.py:66 e marcar_feriado_do_posto.py:111] [nao verificado]

**Concorrência residual.** O sync adquire só a trava do posto antes de ler e comparar o vínculo. Alterar o posto do vínculo ou desativá-lo entre a segunda leitura e a escrita pode deixar o sync aplicar a decisão anterior. O sync relê pelo PK, sem reaplicar os filtros de ativo e empresa. Existe também ciclo de espera possível com `executar_vinculo`: o sync segura `FOR UPDATE` no posto e espera o vínculo antigo, enquanto a outra transação segura o vínculo antigo ao fechá-lo e insere o sucessor com FK para o mesmo posto. O deadlock é uma inferência para o PostgreSQL da cópia, condicionada à FK vigente. Ocorrência ou reprodução [nao verificado]. [app/colaboradores/management/commands/sincronizar_feriado_admin.py:65–89] [app/colaboradores/services/vinculo.py:196–219,259–299,642–643,844–847,898–908] [app/escala/models.py:873] [app/escala/migrations/0001_initial.py:43] [app/config/settings/prod.py:11]

## 22/11 00:16 · sessao 2e8be03c5219 · E4C-F5-CONSTROI · sonda · origem=Code

Saída raspada integral fornecida no enunciado. Uma nova execução pelo envelope exigiria escrever arquivos, conforme [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/bin/sonda_leitura.sh:126) e [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/bin/sonda_leitura.sh:216).

```text
sonda_leitura: sonda.py [75e17b072bd167d4] no cpuset 4-7, lendo sombra como leitor (LEITURA)

DEGRAU hora: 49695
DEGRAU hora materializada: 37170
DEGRAU + sem via: 0
DEGRAU + sem validada_em fisica: 0
DEGRAU hora com texto sem respondida_em: 0
UNIVERSO hora materializada sem via sem validacao fisica: 0
LEGADO (carimbo_legado=True): 0
POR EMPRESA: {}
DESTINO ENCERRADO: 0 COM BATIDA VINCULADA VIVA: 0
```

1. **Sim.** `LEGADO = 0` e `UNIVERSO = 0`, portanto `LEGADO <= UNIVERSO` (`sonda.py:29–30`, anexo).
2. **Sim, o código da sonda referencia as duas funções reais.** As chamadas estão dentro do laço, que teve zero iterações na saída fornecida (`sonda.py:5–6` e `sonda.py:20–25`, anexo). [app/chamados/juizes.py:1590](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/app/chamados/juizes.py:1590) · [app/chamados/models.py:4382](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/app/chamados/models.py:4382)
3. **Nenhum erro ou exceção aparece na saída fornecida.** Código de retorno da execução [nao verificado].

## 22/11 00:15 · sessao cf2459c7b468 · E4C-F5-CONSTROI · sonda · origem=Code

Não reexecutei a sonda. O envelope `--sombra` grava arquivos de credencial e cria uma cópia temporária da sonda, o que viola a regra de só leitura. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/bin/sonda_leitura.sh:126), [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/bin/sonda_leitura.sh:216)

**Saída raspada fornecida, na íntegra** [nao verificado]

```text
sonda_leitura: sonda.py [d461dfc2305a9e0f] no cpuset 4-7, lendo sombra como leitor (LEITURA)

UNIVERSO hora materializada sem via sem validacao fisica: 0
LEGADO (carimbo_legado=True): 0
POR EMPRESA: {}
DESTINO ENCERRADO: 0 COM BATIDA VINCULADA VIVA: 0
```

1. **Sim.** Na saída fornecida, `LEGADO <= UNIVERSO` vale (`0 <= 0`). [nao verificado]
2. **Sim, no código.** O anexo chama `texto_sem_carimbo(p)` e `DecisaoPergunta.composta(p)` diretamente dentro do laço. Se `UNIVERSO = 0`, o laço não executa nenhuma das duas chamadas. `sonda.py:14`, `sonda.py:16`, `sonda.py:18`, [app/chamados/juizes.py:1590](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/app/chamados/juizes.py:1590), [app/chamados/models.py:4382](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/app/chamados/models.py:4382)
3. **Nenhum erro ou exceção aparece na saída fornecida.** Código de saída [nao verificado].

## 22/11 00:04 · sessao f1f4ca749695 · E4C-F5-CONSTROI · verificador · origem=Code

**Pré-verificação da volta 4 — a fórmula e P1–P4 conferem no código. O aval completo fica pendente do registro de `SELO-C12-POR-NOME`, exigido em `aval.txt:1` e ausente do diff entregue.** Os três blobs da cópia batem com os destinos do anexo. Reconstruí os três blobs anteriores em memória e eles batem com as origens do anexo. A associação desses blobs aos commits `af495e9abb22f03efbb4b8e21aad48d0faf6c5f6` e `ed7d24934` é **[nao verificado]**, pois a cópia não tem metadados Git disponíveis. `app/chamados/models.py:1707–1735`, `app/chamados/tests/e4b_c12_ast.py:26–27`, `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:217–348`.

## 1. Cláusulas do aval

| Cláusula | Verificação |
|---|---|
| Fórmula sem `+1 us`, com desempate pela pk | O diff remove o incremento e grava `max([_agora_r] + [d.ato_em ...])`. O desempate já existente continua em `seq.sort` pela tripla `(ato_em, classe da linha, pk)`. Entre linhas reais, a classe é constante. `app/chamados/models.py:1707–1728`, `:4377–4386`. |
| P1 | `_caso` mede o maior `ato_em` da pergunta excluindo a reabertura compensadora. `_coerente` exige que a reabertura seja igual ou posterior a esse máximo e ao relógio da borda. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:205–226`. |
| P2 nova | `r1` e `r3_recuo` exigem empate real com o último `ato_em`. A leitura roda numa pergunta relida e verifica que a validação caiu na sequência e na coluna. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:207–213`, `:227–244`, `:282–291`. |
| P3 | M3 move a leitura do relógio para antes da trava e preserva o `max`. O teste exige que o carimbo mutado preceda a borda e que `_coerente` falhe. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:112–121`, `:226`, `:307–312`. |
| P4 | O teste exige uma reabertura acima de `AGORA + 1 min` e chama o auditor real, com contagem `0` antes e `1` depois. Está no diff do teste. O contador já existia. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:329–348`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1132`. |
| Dois empates do par | Intocados no diff anexo. Ambos continuam exigindo o instante exato do empate e a validação nula na leitura e na coluna. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:364–389`. |
| Selo `e4b_c12` | Só os dois números da tabela mudam, com os comentários de justificativa pedidos. No blob anterior reconstruído, as mesmas chamadas a `_gravar_estado` estão em `1792` e `1812`. Na cópia estão em `1791` e `1811`, com os mesmos argumentos. Rodei os 11 testes do selo em modo somente leitura, todos OK. `app/chamados/tests/e4b_c12_ast.py:18–29`, `app/chamados/models.py:1791`, `:1811`, `app/chamados/tests/test_e4b_c12_selo.py:21–52`. |
| Mutante que inverte a pk | O diff troca apenas a chave de ordenação por `(x[0], x[1], -x[2])` e aplica o mutante aos dois cenários de recuo. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:34–50`, `:140–145`, `:293–303`. |
| Fila congelada | O registro de `SELO-C12-POR-NOME` não está no diff e não foi localizado em `app/`. Registro fora da cópia **[nao verificado]**. `aval.txt:1`, `app/chamados/tests/e4b_c12_ast.py:17–27`. |

## 2. Quem escreve e quem lê

**O relógio e a escrita.** `ChamadoColaborador.ressuscitar_perguntas` atribui `_agora_r` com uma chamada a `timezone.now()` e o lê para calcular o máximo. A mesma porta monta a linha de `REABERTURA` com `da_ressuscitacao=True`. `DecisaoPergunta.registrar` materializa a linha via `bulk_create`. Os chamadores são `ChamadoColaborador.reabrir`, o cartório e o desarquivamento após registrar o toque administrativo. `app/chamados/models.py:1517`, `:1716–1737`, `:4443–4454`, `app/ponto/services/cartorio.py:1721`, `app/chamados/views_arquivar.py:63–86`.

**As fontes dos `ato_em` do máximo.** Ele lê todas as decisões reais da mesma pergunta. Podem alimentá-lo os atos de resposta humana, automática e pela trilha, a correção ou recusa, a validação e suas devoluções, o silêncio, a retratação, a absorção, as reaberturas compensadoras anteriores e as fotos do passivo. Os backfills também podem inserir silêncio ou fotos do passivo pelos mesmos campos. `app/chamados/models.py:1679–1680`, `:1720–1735`, `:2126–2128`, `:2258–2262`, `:3250`, `:4324–4338`, `app/chamados/services/disputa_emissao.py:1535`, `:1664–1681`, `:1713`, `:1735`, `app/chamados/services/validacao.py:168–175`, `:293–296`, `:370–372`, `:452–455`, `app/chamados/services/materializacao.py:1087–1090`, `app/ponto/services/veredito_celula.py:195–197`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:350–354`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:149–150`.

| Leitor | O que acontece no empate |
|---|---|
| `DecisaoPergunta.leitura` e `composta` | Entre linhas reais, a maior pk é processada depois. A reabertura de maior pk limpa resposta e validação. O passivo virtual vem **depois de todas as linhas reais** no mesmo instante. Na ressuscitação, o passivo é registrado antes do cálculo e a projeção é zerada na mesma transação. `app/chamados/models.py:1679–1680`, `:1735–1738`, `:4346–4347`, `:4377–4406`. |
| Face SQL e autor da validação | `q_validada_pelo_ato` usa `ato_em >` ou `ato_em =` com `pk >` entre atos reais. No ramo do passivo sem linha correspondente, usa `>` estrito, em acordo com a precedência do passivo virtual. A foto do autor percorre as validações no mesmo instante e conserva a última da ordem padrão `(ato_em, pk)`. `app/chamados/models.py:1722–1727`, `:3617–3629`, `:4268–4269`. |
| Sonda de ressuscitação | Recorta por `d.ato_em > _agora_sonda`. Todos os atos no instante da sonda caem do recorte, independentemente da pk. Os remanescentes seguem para C6, que lê `composta`. É um corte temporal do pré-filtro. `app/chamados/models.py:1565–1567`, `:1587–1602`, `app/chamados/juizes.py:1406–1411`. |
| `disputa_emissao.py` | A prévia delega à porta, as leituras do estado delegam a `composta` e o histórico a `respostas_anteriores`. A guarda de absorção é específica. Ela percorre por **pk** e compara `ato_em` ao carimbo para reconhecer a absorção. Reabertura ou recusa de pk posterior derruba a absorção, qualquer que seja seu `ato_em`. `app/chamados/services/disputa_emissao.py:1489`, `:1660–1663`, `:1851–1859`, `:2052`, `:2112`, `:2169–2173`, `app/chamados/models.py:4433–4441`. |
| Cartório e desarquivamento | O cartório pré-carrega `decisoes` e usa `fio_vivo` e `composta`. Com ambas as linhas carregadas, herda o desempate da leitura. `views_arquivar.py` chama as portas e não escolhe uma decisão por timestamp. `app/ponto/services/cartorio.py:144–146`, `:1683–1697`, `:1721`, `app/chamados/juizes.py:1516–1526`, `app/chamados/views_arquivar.py:63–86`. |
| `modal_fio.html` | O histórico recebe `respostas_anteriores`, que percorre a ordem do modelo, limpa a resposta vigente ao encontrar a reabertura e inverte a lista para exibição. O template só formata `ato_em`. As etiquetas de validação leem a coluna, que a ressuscitação limpa. `app/chamados/models.py:3862–3878`, `:4268–4269`, `:1737–1738`, `app/chamados/services/fio.py:263–267`, `app/templates/chamados/partials/modal_fio.html:204–229`, `:345–348`. |
| `validacao.py` | As guardas que leem o ato usam `composta`. A foto da devolução consulta a coluna primeiro e usa `composta` se a coluna estiver vazia. O lote recarrega a instância antes da conferência final. Não há uma ordenação própria por `ato_em`. `app/chamados/services/validacao.py:178–188`, `:269–293`, `:309–320`, `:656–657`, `:697–704`, `:915–923`, `:1074–1105`. |
| Tarefa do dia | Na sequência para ações, ambos os caminhos de carga ordenam por `(ato_em, pk)` por pergunta. A reabertura compensadora é **excluída pelo predicado antes** do corte `<= agora` e da escolha do último ato. Esse leitor pode, por desenho, continuar usando o ato anterior para a tarefa. A consulta separada de vitalidade C6 recebe a pergunta com a sequência integral e usa a leitura composta. `app/chamados/services/tarefa_do_dia.py:142–170`, `:387–403`, `:424–434`, `:533–545`, `:625–637`, `app/chamados/juizes.py:1406–1411`. |
| I23 e demais comparações diretas | I23 compara ao limiar e lista pks. A correspondência do passivo usa a chave composta `(pergunta_id, ato, ato_em)` para verificar existência e preserva todas as linhas reais na leitura. A foto E4b identifica as linhas pela pk. No censo dos leitores de produção em `app/`, não encontrei seleção de vigente ordenada apenas por `ato_em` ou `-ato_em`, nem uso de `ato_em` isolado como chave única. `app/chamados/management/commands/auditar_invariantes_chamados.py:1129–1132`, `app/chamados/models.py:4323–4338`, `:4377–4386`, `app/chamados/services/foto_e4b.py:247–252`, `:280–281`. |

## 3. A pk da reabertura é sempre maior?

**No fluxo com sequência crescente e pks automáticas, sim para as linhas da pergunta já visíveis no prefetch.** A pk é `BigAutoField`, herdada da tabela renomeada. O lote de reaberturas é montado sem pk, após adquirir as travas e registrar o passivo. A inserção acontece no `bulk_create`. A ordem por pk no laço é a das **perguntas**. A pk da **decisão** vem da inserção. Não encontrei chamador de produção que pré-atribua `pk` ou `id` à decisão, inclusive nos backfills. `app/chamados/migrations/0050_resposta_anterior.py:16–23`, `app/chamados/migrations/0051_decisao_pergunta.py:39–45`, `app/chamados/models.py:1652–1657`, `:1679–1680`, `:1720–1735`, `:4443–4454`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:350–354`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:149–150`.

**O “sempre” não é imposto pela porta.** `registrar` repassa `**ln` ao `bulk_create` sem rejeitar pk explícita e sem checar a relação entre a pk gerada e as existentes. A guarda de `save` está em outro caminho. Estado e configuração efetiva da sequência em produção **[nao verificado]**. Se a reabertura tiver pk menor que uma validação anterior com o mesmo `ato_em`, a validação será processada depois e ficará vigente, inclusive na face SQL. Uma validação inserida depois com pk menor, no mesmo instante, terá o efeito inverso. `app/chamados/models.py:4296–4299`, `:4443–4454`, `:4379–4395`, `:3619–3623`.

## 4. Quando o relógio e os atos são lidos

A seleção inicial avalia seu próprio prefetch antes da trava e entrega **ids**. No `atomic`, a porta adquire Colaborador → Disputa → Chamado → Pergunta, relê o conjunto gravável e grava o passivo. Depois lê `_agora_r`. Só então a iteração avalia o novo `prefetch_related('decisoes')`. A leitura, a foto do autor e o máximo usam esse mesmo lote. As instâncias e o cache da sonda não alimentam o máximo em nenhum caminho encontrado. Visibilidade efetiva entre transações em produção **[nao verificado]**. `app/chamados/models.py:1549–1567`, `:1597–1602`, `:1638–1657`, `:1675–1680`, `:1716–1728`, `:2891–2920`.

## 5. Mordida dos testes e relógio [nao verificado]

- **P2 e M3 mordem nas propriedades esperadas.** Em memória, usando a `leitura`, `_coerente` e os transformadores AST da cópia, confirmei que inverter a pk deixa a validação vigente e dispara a divergência ato/coluna. Para M3, confirmei a relocação da atribuição no AST e que o carimbo anterior à borda dispara a asserção temporal. Nos testes de integração, a aplicação do mutante e a execução do caso ficam fora de `assertRaises`. O `assertRaises` aceita qualquer falha de `_coerente`, mas as testemunhas de empate e de relógio ficam fora dele. Execução independente com ORM/banco **[nao verificado]**. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:34–50`, `:112–145`, `:217–230`, `:293–312`.
- **M1/M2, M4/M5.** M1/M2 exigem `n=0` após antecipar a sonda à iteração. M4 tira o `max`, M5 coloca a coleta antes da trava, e ambos exigem o carimbo antigo antes de cobrar `_coerente`. Nesses casos o ato injetado já faz falhar a comparação da linha 223. A nova asserção da linha 225 não tem uma mordida independente com outro ato mais alto. A “corrida” injeta na mesma execução, antes de chamar a trava real, sem reproduzir um commit em outra conexão. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:73–86`, `:124–137`, `:167–204`, `:223–226`, `:247–268`, `:314–326`.
- **P4 tem positivo e negativos.** O positivo contém também uma validação adiantada, então omitir o predicado inteiro faria a contagem exceder `1`. Falta caso em `+60 s` exatos e falta uma reabertura adiantada **sem** a marca, portanto os mutantes `>=` e “só checar o tipo `REABERTURA`” podem sobreviver. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:329–348`.
- **Relógio dos testes.** Não achei `timezone.now()` ou `localdate()` soltos nos testes do delta. O caso e a segunda chamada ficam no `freeze_time`, assim como a chamada ao auditor. A fixture vem da classe do par decorada com `freeze_time(AGORA)`. O recorte de 10.938 testes com `rc 0` é o informado no enunciado. A execução desse recorte é **[nao verificado]** independentemente. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:148–204`, `:329–336`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:49–85`.

## 6. I23

O universo é exatamente **reaberturas da ressuscitação com `ato_em > agora + 1 min`**. O SQL aplica o `>` estrito e o código pergunta `DecisaoPergunta.es_da_ressuscitacao(d)`, que exige tipo `REABERTURA` e marca. O contador ordena por pk para a amostra. O comentário ainda diz **`+1 us`**. O rótulo da tarefa do dia em `JUIZES.md` é resumido como “por `ato_em`”. No código, a tarefa inclui pk na ordenação e chama o mesmo predicado para excluir a compensação. Contagem I23 em produção **[nao verificado]**. `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1132`, `app/chamados/models.py:4289–4294`, `app/docs/JUIZES.md:81–87`, `app/chamados/services/tarefa_do_dia.py:387–403`, `:537–545`.

## 7. Checklist

| Classe | Resultado no delta |
|---|---|
| `except`, fallback, default e silêncio | **49 linhas `+`**. **0** `except` ou `except*`, **0** fallbacks/defaults que escondam erro e **0** ramos novos que troquem erro por silêncio. Há **1** default técnico em `getattr(real, '__func__', real)`, que aceita função comum e `classmethod`. Há **1** novo `assertRaises(AssertionError)` no teste do mutante, restrito à propriedade. Falha de aplicação do mutante sobe. O `except` da trilha de ressuscitação já existia. `app/chamados/models.py:1709–1728`, `:1759–1774`, `app/chamados/tests/e4b_c12_ast.py:26–27`, `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:34–50`, `:89–99`, `:293–312`. |
| Segundo juiz | **0 novos**. O máximo usa os atos da pergunta e o estado continua lido pela sequência. `app/chamados/models.py:1720–1728`, `:4377–4406`, `app/core/juizes.py:290–303`. |
| Segundo escritor fora da porta | **0 novos**. O escritor da marca segue chamando `registrar`. A catraca exige uma ocorrência na porta e nenhuma fora. `app/chamados/models.py:1729–1735`, `:4443–4454`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:65–79`. |
| Relógio fora da borda | **0 novos**. `_agora_r` está depois da trava. Fora da unidade, a retratação já lê seu relógio antes da trava e a foto automática recebe o relógio do lote do upload. `app/chamados/models.py:1652–1657`, `:1716–1717`, `:2226–2232`, `app/api/foto_recebida.py:38–48`, `app/chamados/services/disputa_emissao.py:1707–1715`. |
| Derivação paralela | **0 novas**. A sonda anterior só seleciona, o máximo usa o novo lote e o I23 pergunta ao predicado existente. `app/chamados/models.py:1597–1602`, `:1638–1646`, `:1720–1728`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1127–1132`. |
| Selo ou RED sem mordida | P2 e M3 têm mordida discriminada em memória. O limite da prova de concorrência e os controles que faltam ao I23 estão no item 5. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:194–204`, `:293–312`, `:329–348`. |
| Ordem de trava | A ordem e o local da aquisição são os mesmos. O primeiro consumo do prefetch da escrita vem depois. `app/chamados/models.py:1652–1657`, `:1720–1728`, `:2891–2920`. |
| Pouso antes do pré-requisito | O delta usa a marca e o leitor já presentes. A migração `0054` é o pré-requisito da marca. Ordem de migração e pouso em produção **[nao verificado]**. `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:14–25`, `app/chamados/models.py:4256`, `:4289–4294`, `app/chamados/services/tarefa_do_dia.py:393–401`. |
| ESMERIL-SEMÂNTICA | O duplo uso permanece. O campo documenta o instante do ato, mas a ressuscitação pode gravar o instante maior de outro ato para garantir posição na sequência. Se `ultimo > agora_sob_trava`, o carimbo fica adiantado. O I23 observa o adiantamento acima de um minuto e o modal exibe o carimbo como data da reabertura. É o risco já aceito no aval. `aval.txt:1`, `app/chamados/models.py:1707–1728`, `:4203–4213`, `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1124–1132`. |

## 8. Alcance permitido

No diff anexo, **zero alterações fora do permitido**. Em `models.py`, os dois hunks estão no instante da reabertura. No `e4b_c12_ast.py`, só mudam `1792 → 1791`, `1812 → 1811` e os comentários justificativos dessas duas linhas. O terceiro arquivo é o teste da unidade. `core/juizes.py`, `chamados/juizes.py`, `motor_calculo_v2.py`, `ponto/portas/celula.py`, `config/crons.py`, `bin/`, `CLAUDE.md` e migrations não constam do delta anexo. Diferenças adicionais entre os commits completos **[nao verificado]**. `app/chamados/models.py:1707–1730`, `app/chamados/tests/e4b_c12_ast.py:26–27`, `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:34–348`.

## ACHADOS

1. **Bloqueia — registro exigido pelo aval ausente da entrega.** `SELO-C12-POR-NOME` deve constar da fila congelada. O diff comprova a atualização dos números e a justificativa. O registro externo é **[nao verificado]**. `aval.txt:1`, `app/chamados/tests/e4b_c12_ast.py:17–27`.
2. **Fila — a garantia no empate depende da atribuição da pk.** A porta não impõe monotonicidade, e os testes injetam na mesma execução. Não encontrei chamador de produção atribuindo pk explicitamente a uma decisão. `app/chamados/models.py:1729–1735`, `:4379–4395`, `:4443–4454`, `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:167–204`.
3. **Fila — `ato_em` continua servindo de instante exibido e posição de ordenação.** Com relógio recuado, o timestamp de outro ato pode aparecer como data da reabertura. A ocorrência em produção é **[nao verificado]**. `app/chamados/models.py:1707–1728`, `app/templates/chamados/partials/modal_fio.html:212`.
4. **Nota — dois comentários ainda mencionam `+1 us`.** Um no I23 e outro no teste `r3`. O predicado do contador e o teste positivo já cobram a propriedade vigente. `app/chamados/management/commands/auditar_invariantes_chamados.py:1124–1132`, `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:274–280`, `:342–348`.
5. **Nota — controles que faltam ao I23 e à mordida independente de P1.** Faltam o limiar exato, uma reabertura sem marca e um caso em que o maior ato seja diferente do ato injetado. `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:208–226`, `:314–348`.

## 21/11 23:54 · sessao 528d3bd6a4c2 · E4C-F5-CONSTROI · verificador · origem=Code

BLOQUEIA — A captura nominal está no ponto certo, mas a recusa nova pode produzir trilha de sucesso no fio e perder ou trocar o motivo no inline e no copiloto. Correção de resposta e declaração em texto ainda podem devolver 500 ao admin. `app/chamados/services/acoes_disputa.py:20–32,389–397` · `app/chamados/services/fio.py:657–667` · `app/templates/chamados/partials/_lista_chamados.html:145–165` · `app/core/views_ia.py:169–195` · `app/chamados/services/declaracao_texto.py:689–699`

Conferi os cinco blobs de destino do `delta.patch` contra os arquivos da cópia. A cópia é distribuída por `git archive`, e a identidade do commit e sua ancestralidade são [nao verificado]. Ocorrência e frequência em produção também são [nao verificado]. `CLAUDE.md:344–349` [nao verificado]

## 1. Item (2) do aval

| Cláusula | Resultado |
|---|---|
| **NOVO — captura só `AtoDoPlantioMudou`** | Sim. O `try` envolve a chamada ao serviço e o único `except` novo nomeia essa classe. Na execução isolada em memória, um `ValueError` injetado no serviço subiu intacto. `app/chamados/services/acoes_disputa.py:18–25` |
| **NOVO — formato e motivo** | A casca devolve o `resp(tipo, msg)` existente, com `aviso` e o literal `a pergunta mudou enquanto voce validava; recarregue`. O formato local confere. O motivo efetivamente entregue depende do consumidor, com perdas descritas no item 3. `app/chamados/services/acoes_disputa.py:13–17,32,40–50,60–61` · `app/templates/chamados/partials/_lista_chamados.html:145–161` · `app/core/views_ia.py:185–195` |
| **NOVO — trilha** | Solicita uma linha `validar_pergunta_plantio_mudou`, com modelo, PK, usuário do request e `str(e)`. O teste exige uma linha e a causa interna esperada. A persistência usa a trilha best-effort preexistente, que registra falhas e permite que a recusa prossiga. `app/chamados/services/acoes_disputa.py:25–32` · `app/core/auditoria.py:17–30` · `app/core/observ.py:94–105,124–139` · `app/chamados/tests/test_e4c_f5_validacao_plantio.py:189–195` |
| **NOVO — RED e mutante sem captura** | O teste chama a casca real e exige retorno, motivo, trilha e corrida comprovada. Executei em memória a função da cópia com o serviço dublado: com a captura, devolveu o aviso e chamou a trilha. Retirando só a captura, a exceção original subiu. O relato fornecido declara RED `rc=1` e green `rc=0`, `Ran 54`. A execução independente do RED/green e do mutante em PostgreSQL é [nao verificado]. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:166–195` · `app/chamados/services/acoes_disputa.py:18–32` |

## 2. Rollback, ID e chamadores

**NOVO — posição correta.** Os três levantamentos acontecem na reconferência da trava, antes da releitura de validação, do carimbo, da `DecisaoPergunta`, do plantio de `Batida` e dos logs do serviço. A aquisição inscreve um sentinela sem efeito em `on_commit`. O rollback do bloco ou do savepoint remove esse sentinela. Os callbacks de negócio ficam depois do ponto de recusa. `registrar_log` roda depois da saída do `atomic` que falhou e abre seu próprio bloco de escrita. Um `atomic` externo eventualmente aberto pelo chamador ainda se aplica. `app/chamados/services/materializacao.py:2135–2160` · `app/chamados/models.py:2875–2904` · `app/chamados/services/trava_do_colaborador.py:9–23,37–47` · `app/chamados/services/validacao.py:161–175,713–758,772–799` · `app/chamados/signals.py:229–259` · `app/core/observ.py:124–139`

**NOVO — `int()` do log.** Nos chamadores da casca, detalhe e fio já convertem o valor, e inline e lote escrevem `str(perg.pk)` no POST. O próprio serviço só chega à trava depois de buscar a pergunta pelo mesmo valor. Vazio, inválido ou inexistente sai por `nao_encontrada` antes da captura. A releitura do POST é redundante. `app/chamados/views.py:492–501` · `app/chamados/services/fio.py:639–658` · `app/chamados/views_cobrar.py:202–210` · `app/chamados/services/validacao.py:697–716,1076–1082` · `app/chamados/services/acoes_disputa.py:20–35`

| Chamador de produção | Destino de `AtoDoPlantioMudou` |
|---|---|
| **NOVO — detalhe, fio e modal** | A validação passa pela casca e recebe a recusa. O fio em disputa fechada ainda executa a releitura posterior que pode gravar a trilha indevida do item 3. `app/chamados/views.py:483–501,667–674,951–954` · `app/chamados/services/fio.py:634–667` |
| **NOVO — inline, lote JSON, lote da tela e “sim” do copiloto** | Todos passam pela casca e seguem após a recusa do item. Os três lotes usam `validar_lote_classe_a`. `app/chamados/views_cobrar.py:202–216,249–269,374–388` · `app/chamados/services/validacao.py:1074–1099` · `app/core/views_ia.py:157–176,870–881` |
| **PREEXISTENTE no delta — `acao_corrigir_resposta`** | Detalhe e fio/modal chegam à chamada direta, que pode subir até HTTP 500. Followup, resposta corrigida e trilha da correção precedem a tentativa de validação. `app/chamados/views.py:520–536` · `app/chamados/services/fio.py:674–688` · `app/chamados/services/acoes_disputa.py:368–398` · `app/chamados/services/disputa_emissao.py:1478–1538` |
| **PREEXISTENTE no delta — declaração em texto** | A chamada direta pode subir até HTTP 500 no detalhe, depois do bloco que pode emitir a pergunta e registrar a resposta. `app/chamados/views.py:477–481` · `app/chamados/services/declaracao_texto.py:656–699` |
| **PREEXISTENTE no delta — API de upload de foto** | Os dois endpoints chamam o mesmo kernel, que chama o serviço diretamente. A exceção pode subir até HTTP 500 depois de gravar `Batida.foto`, `FotoBatida` e eventual resposta automática. `foto_ausente` está exposta à mudança de escopo da principal. `app/api/views.py:839–863` · `app/api/views_core.py:823–847` · `app/api/foto_recebida.py:38–54,65–76` · `app/ponto/registro_batida.py:316–332` · `app/chamados/services/disputa_emissao.py:1698–1716` · `app/chamados/services/materializacao.py:2148–2158` |
| **PREEXISTENTE no delta — comando `validar_classe_a --apply`** | A chamada direta interrompe o comando antes do resumo. Itens anteriores podem já ter confirmado. `app/chamados/management/commands/validar_classe_a.py:88–108` · `app/chamados/services/validacao.py:713–790` |

Os **três chamadores diretos da trava** são `validar_pergunta`, que propaga para seu chamador, o laço `materializar_perguntas_validadas_da_disputa`, que converte em `erros` dentro do `atomic` do item, e `passivo_validado.reparar`, que converte em recusa do grupo fora do `atomic`. O caminho de regularização que chama o laço recebe o resultado com `erros`. **PREEXISTENTE no delta.** `app/chamados/services/validacao.py:713–718,787–790` · `app/chamados/services/materializacao.py:2205–2235` · `app/chamados/services/passivo_validado.py:392–415` · `app/chamados/services/acoes_disputa.py:146–159`

## 3. Quem escreve e quem lê `aviso` e a trilha

- **NOVO — trilha de sucesso indevida no fio/modal.** Em disputa fechada, o fio reconsulta `_p.estado` depois que a casca devolve a recusa. Se outra requisição plantar o par entre a primeira foto e a trava, a irmã materializada sai da eleição e a primeira requisição recebe `AtoDoPlantioMudou`. A releitura pode então ver a principal `VALIDADA` ou `RESOLVIDA` pelo outro admin e gravar `validar_viva_em_disputa_fechada` no nome de quem recebeu a recusa. O leitor de estado é preexistente. A continuação por esse leitor após **esta exceção** é nova. `app/chamados/services/fio.py:649–667` · `app/chamados/services/materializacao.py:149–157,2137–2158` · `app/chamados/services/validacao.py:646–660` · `app/chamados/models.py:3249–3262` · `app/core/auditoria.py:22–25`
- **NOVO — motivo some no inline misto.** O endpoint põe `aviso` em `avisos` e marca `ok` se algum item teve sucesso. O JS do ramo `j.ok` mostra o total e “Exigem revisao no fio”, descartando o pedido de recarregar. Com zero sucessos, lê só o primeiro aviso. O adaptador `hxToast` recebe um booleano, mas o inline lhe passa strings, então tanto `aviso` quanto `sucesso` viram toast de erro. O defeito do adaptador é **PREEXISTENTE**. `app/chamados/views_cobrar.py:202–216` · `app/templates/chamados/partials/_lista_chamados.html:145–165` · `app/templates/core/_dialogo.html:245`
- **NOVO — lote e copiloto podem narrar outro resultado.** O lote relê o estado global. Se outra requisição validou e plantou enquanto esta foi recusada, pode somar a validação alheia e colocar o caso em `nao_gravados`, cujo rótulo diz “Validadas sem gravar o ponto”. Quando a pergunta segue sem validação, a tela do lote exibe `x.motivo` e o JSON o transporta. O botão do copiloto usa só contagens. No “sim” do chat, a frase atribui as recusas a “saiu da classe A ou ja estava validada” e o retorno usa `escrita_aplicada` mesmo com zero validações. Esses leitores são **PREEXISTENTES**, agora alcançados pela recusa de corrida. `app/chamados/services/validacao.py:1074–1099` · `app/chamados/views_cobrar.py:263–269,381–388` · `app/templates/chamados/partials/_lote_preview.html:5–9` · `app/templates/core/_copiloto.html:247–257` · `app/core/views_ia.py:169–195,873–880`
- **PREEXISTENTE — envelopes e significados.** O detalhe HTMX põe `aviso` em `HX-Trigger` e troca o card. Fio e modal usam o parcial com `resposta_render`. O JS comum lê `showToast.tipo` e renderiza `aviso` em amarelo. No POST comum do detalhe, `resposta_acao` usa Django `WARNING` e infere `ok=True` de `aviso`, enquanto a trilha de recusa crítica inclui `aviso`. A própria casca já usava o tipo para implausível, recusa integral, validação sem plantio e sucesso com ressalva sobre irmãs. Portanto, `aviso` sozinho é severidade de apresentação e serve a resultados distintos. O detalhe HTMX monta o header direto e passa fora do registrador de recusa crítica do envelope. `app/chamados/views.py:394–416` · `app/chamados/services/fio.py:541–547,657–658` · `app/core/respostas.py:12–17,47–85` · `app/core/recusa_critica.py:30–34` · `app/templates/core/_dialogo.html:26–37,78–87` · `app/chamados/services/acoes_disputa.py:40–74`
- **NOVO — verbo de auditoria livre.** `LogAuditoria.ACOES` enumera escolhas do campo e alimenta o seletor da tela. A escrita canônica usa `objects.create()` e admite verbos livres com até 64 caracteres. O verbo novo aparece na listagem genérica e pode ser filtrado por `?acao=...`, embora fique fora das opções do seletor. A catraca encontrada cobra o escritor único, e o teste de tamanho demonstra verbos fora das escolhas. Nenhum consumidor de produção específico para o novo literal foi identificado. `app/chamados/services/acoes_disputa.py:29–31` · `app/core/models.py:285–316` · `app/core/observ.py:124–138` · `app/core/views.py:697–710,726–740` · `app/templates/core/log_auditoria.html:23–27,55–73` · `app/core/tests/test_contract_trilha.py:56–68` · `app/core/tests/test_logauditoria_acao_larga.py:7–34`

## 4. Mordida dos testes e relógio [nao verificado]

- **NOVO — o RED da casca não passa sem corrida.** O gancho exige uma leitura da irmã antes de iniciar o worker. O teste exige um PID escritor distinto, o vínculo concorrente persistido, um único aviso, uma única trilha com a causa, as mesmas batidas, carimbos nulos e ausência do ato `VALIDACAO`. `all([])` isoladamente seria verdadeiro, mas a lista vazia falha no gancho ou na contagem do worker. O teste usa `resp` dublado e deixa sem asserção os consumidores reais, os logs de validação concorrentes e a fila `on_commit`. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:82–92,102–143,166–195`
- **NOVO — M1 perdeu a capacidade de isolar a leitura fora do `atomic`.** Ele aplica M2 e depois desloca `_irma` e `_ch_irma`. Ambos retiram a reconferência, de modo que a falta da exceção esperada já pode derrubar o RED antes da asserção de leitura em `atomic`. As duas transformações aplicaram e compilaram em memória. A asserção que falha em PostgreSQL é [nao verificado]. O controle sem corrida exige o PK da irmã, os dois carimbos, via e batidas esperadas. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:21–63,130–143,151–164,197–202` · `app/chamados/tests/test_bug_par_projetado.py:106–115`
- **NOVO — ajustes das outras fixtures.** Os sete testes sem banco passaram, incluindo o K6 que exige `False`, motivo e pergunta intacta. O caso 5644 fixa o `dia_fato` da instância em 21/10 e exige que o plantio complete a célula. O S144 fixa as variáveis por iteração e exige recusa e zero chamada ao plantio. O relógio de negócio do RED e do 5644 está congelado com fuso declarado. O S144 congela `AGORA` no `setUp`, embora a criação de dados de classe ocorra antes desse congelamento. O SQL do worker usa PID e timeout, sem consultar a hora do servidor. As execuções ORM são [nao verificado]. `app/chamados/tests/test_bug_par_sem_banco.py:153–168` · `app/chamados/tests/test_bug_par_volta.py:420–453` · `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:30–42,165–202` · `app/chamados/tests/cortes_f2_f9_apoio.py:30–34,45–49` · `app/chamados/tests/test_e4c_f5_validacao_plantio.py:66–92`

## 5. Checklist e triagem da volta 5

| Checagem | Resultado |
|---|---|
| **`except` e fallback** | **NOVO — 1 `except` em produção, 0 em teste no delta fornecido.** A exceção nominal deixa de subir nesse ponto e vira recusa com motivo. Outras classes sobem. Zero fallback novo de negócio. O ramo avulso e a trilha best-effort são **PREEXISTENTES**. `app/chamados/services/acoes_disputa.py:18–32` · `app/chamados/services/validacao.py:772–778` · `app/core/auditoria.py:17–30` · `app/chamados/tests/test_e4c_f5_validacao_plantio.py:82–195` |
| **Segundo juiz, escritor fora da porta, relógio e derivação** | **NOVO —** a casca traduz o resultado e chama o escritor canônico da trilha. Zero juiz, escritor de validação, leitura de relógio ou derivação de horário adicionados em produção pelo delta. **PREEXISTENTE —** fio e lote usam estado posterior para atribuir o resultado à tentativa, e a recusa nova expõe essa atribuição. `app/chamados/services/acoes_disputa.py:18–32` · `app/core/tests/test_contract_trilha.py:56–68` · `app/docs/JUIZES.md:38–42` · `app/chamados/services/fio.py:658–666` · `app/chamados/services/validacao.py:1082–1098` |
| **Ordem de trava** | **PREEXISTENTE —** aquisição inicial em `Colaborador → Disputa → Chamado → Pergunta`, por PK. A folga ainda pode pedir outras linhas no fechamento do fio depois da trava inicial. Ausência de inversão em todos os escritores [nao verificado]. `app/chamados/models.py:2875–2904` · `app/chamados/services/validacao.py:527–543,561–576,713–738` · `app/ponto/services/veredito_celula.py:174–190` |
| **Campo com dois significados** | **PREEXISTENTE —** `aviso` agrega resultados diferentes, e `data_evento` já distingue dia do turno e dia civil da conversa pelo discriminador `conversa`. **NOVO —** a recusa integral acrescenta um produtor de `aviso`, com os efeitos nos leitores acima. `app/chamados/services/acoes_disputa.py:32,40–74` · `app/chamados/models.py:2993–3012` · `app/chamados/services/disputa_emissao.py:430–450` |

- **Triagem (1) — descarte condicionado, PREEXISTENTE.** A trava pega as candidatas SQL e reconfere a foto. A guarda anterior ao carimbo ainda elege sem o mapa confirmado, instalado só depois. O teste vizinho demonstra eleição diferente após escrita na mesma transação. A divergência por escritor de produção entre essas duas leituras, após a reconferência, é [nao verificado]. O delta não fornece evidência para reclassificar esse achado como bloqueio novo. `app/chamados/services/materializacao.py:139–157,202–207,2135–2160` · `app/chamados/services/validacao.py:713–731,772–778` · `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:175–187`
- **Triagem (2) — parcialmente resolvida.** A fila antiga de tradução foi promovida pelo aval (2) e a casca passou a traduzir. Correção, declaração e API continuam com propagação direta, **PREEXISTENTE** em relação ao delta. Os efeitos dos consumidores após a captura são **NOVOS** e sustentam o bloqueio desta volta. `app/chamados/services/acoes_disputa.py:18–32,384–397` · `app/chamados/services/declaracao_texto.py:695–699` · `app/api/foto_recebida.py:47–54` · `app/chamados/services/fio.py:657–667` · `app/templates/chamados/partials/_lista_chamados.html:145–165`
- **Triagem (3) — segue em fila quanto à cobertura das travas, com ressalva sobre M1.** O teste vizinho exige SQL, ordem e PKs das perguntas. O RED público comprova a reconferência, sem observar diretamente os PKs de disputas e chamados. **NOVO —** M1 e M2 passaram a compartilhar a retirada da reconferência. Selos medidos da candidata e suíte inteira [nao verificado]. `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:105–117,189–202` · `app/chamados/tests/test_e4c_f5_validacao_plantio.py:21–43,102–164` · `CLAUDE.md:372–381`

## 21/11 23:52 · sessao 0662fe710717 · E4C-F5-CONSTROI · verificador · origem=Code

BLOQUEIA

**O texto residual ainda decide quando o C21 nega vigência.** A guarda `carimbo_legado` é **PREEXISTENTE** em `f3d23021f`. O delta passa a usá-la também para decidir quem escapa de `SEM_RESPOSTA_VIGENTE`. Reproduzi com os corpos reais em memória, mantendo materialização visível, via vazia e os instantes de resposta e validação ausentes. Com reabertura na trilha e batida às `06:30`, texto vazio deu `SEM_RESPOSTA_VIGENTE / False` e texto `06:30` deu `SLOT / True`. Com `hora invalida`, uma batida vinculada às `05:24` bastou para dar `PLANTIO_SEM_TS / True`. Sem batida, um destino resolvido com a lei calada também deu `PLANTIO_SEM_TS / True`. O efeito contraria o aval 1930 e continua possível no hash. [app/chamados/juizes.py:409](/home/ronald/codex_ws/4e22e8fba1ea0c2969eb7e6634274e9e1083b051/app/chamados/juizes.py:409) · `app/chamados/juizes.py:801–848` · `app/chamados/models.py:4368–4390` · `app/docs/LEIS.md:151`

**A mordida de `bc_beco` sob `engolido_sobrevive` depende da ordem dos chamados.** Isso é **NOVO na prova S2/S3**. O beco percorre um `set`. Se o destino entra primeiro, o resolver encontra a pergunta validada e sem carimbo, tenta gravar e o veto sobe diretamente. O mutante ainda captura esse veto e o caso fica verde. Se o pai entra primeiro, a varredura do resolver pode fechar a disputa pelo C7, o S144 engole o veto e o mutante deixa o fechamento sobreviver, produzindo `[OK]`. Reproduzi as duas ordens com os corpos reais do beco, do resolver e da cerca, usando dublês de persistência e do receiver. A execução ORM desta matriz está [nao verificado]. `app/chamados/services/becos.py:43–73` · `app/chamados/services/disputa_emissao.py:1939–1952,1978–1982,2038–2059,2098–2112,2143–2165` · `app/chamados/signals.py:149–167` · `app/chamados/tests/test_e4c_b2_cerca_retratar.py:84–99,435–462,585`

### 1. Achados da volta 2

| Achado | Situação neste hash |
|---|---|
| Reabertura com `respondida_em` físico ainda presente, `06:30` residual e vínculo às `05:24` | **Fechado para a foto reproduzida na volta 2.** Reconstruí a base pelo patch em memória e obtive `PLANTIO_SEM_TS / True` na base e `SEM_RESPOSTA_VIGENTE / False` na cópia. O mesmo veto corta a absolvição pelo destino quando a guarda de legado é falsa. **Aberto** para o meio-par legado com instante físico ausente, como demonstrado acima. `app/chamados/juizes.py:368–382,409–423,801–848` · `app/chamados/tests/test_sentinela_volta5_sem_banco.py:55–71` |
| S1 da volta 2, falha da consulta virava classe B | **Fechado no código, PREEXISTENTE ao delta.** O `.exists()` propaga a exceção. O teste exige a chamada e a identidade da exceção. `app/chamados/services/validacao.py:1224–1232` · `app/chamados/tests/test_fila_validar_lote.py:110–123` |
| S2 da volta 2, explicador, vínculo, destino e retratação | **Fechado para os controles de `data_hora` e para a foto de `hora` com instante físico residual.** O explicador exige `validada_sem_gravar`, a causa e a ausência do texto residual. A retratação exige carimbo e trilha em avulso e em carga. **Aberto** para `hora` com carimbo legado. `app/chamados/tests/test_explicador_disputa.py:80–101` · `app/chamados/tests/test_marco_por_motivo.py:231–282` · `app/chamados/tests/test_sentinela_volta5_sem_banco.py:55–107` · `app/chamados/juizes.py:409–423` |
| S2b da volta 2, fixture HX | **Superado pelo aval 1930.** A fixture agora exige `SEM_RESPOSTA_VIGENTE`. Lei calada e dia fora da janela passam de `True` para `False`. Lei acusando segue `False`. `app/chamados/tests/test_hx_fallback_sem_lei.py:41–45,61–80` · `app/chamados/juizes.py:419–423,801–848` |
| S3 da volta 2, foto velha no passivo | **Fechado para o cache de `decisoes`. Aberto, PREEXISTENTE, para os demais insumos.** A seleção, o dia, o agrupamento e a foto de reversão vêm antes da trava. Depois dela, o passivo só atualiza materialização, via e autor. A leitura composta ainda pode combinar texto antigo da instância com instante novo do ato. `app/chamados/services/vigia_carimbo.py:103–123` · `app/chamados/services/passivo_validado.py:330–384,532–545` · `app/chamados/models.py:4382–4399` · `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:95–129` |
| S3 da volta 2, PRONTO e pouso | **Aberto como comprovação.** Suíte inteira da árvore integrada, opus posterior, publicação no RELATO, registro dos SHAs, censo de divergência anterior à cura e medição ORM do vigia [nao verificado]. Pelo complemento 2240, a tabela por empresa dos chamados que a cura segura é condição do **pouso**. Total, aplicação do limite de 50 e entrega da lista ao admin [nao verificado]. `CLAUDE.md:320–329,372–381` | [nao verificado]

### 2. Mudança de veredito por caso

Na tabela, `True` e `False` são do juiz **de materialização de fato**. No fechamento do chamado, a cerca avalia o C6 no pós-estado em memória, para perguntas ancoradas, sem validação vigente e de competência aberta. Se continuam vivas, o sistema tenta reatá-las a uma cobrança sobrevivente e recusa se isso for impossível. No beco, o C7 autoriza o fechamento da disputa e a cerca de escrita pode desfazer o ato por competência entregue. `app/chamados/models.py:349–455,661–681,2029–2035` · `app/chamados/services/becos.py:43–73` · `app/chamados/services/fio.py:1394–1464`

| Caso | `f3d23021f` → cópia |
|---|---|
| `hora` com resposta vigente e slot derivável | `SLOT → SLOT`. Vínculo vivo na janela sustenta `True`. Vínculo fora dela, sozinho, dá `False`. Uma batida viva compatível pela consulta da janela ou a retratação com trilha ainda podem dar `True`. O status do destino fica fora desse julgamento. `app/chamados/juizes.py:240–284,409–423,801–816,863–866` |
| `hora` vigente sem instante derivável | `PLANTIO_SEM_TS → PLANTIO_SEM_TS`. Um vínculo vivo ainda basta. Destino `resolvido` ou `fechado` ainda basta se o dia estiver fora da janela, se a lei calar ou se a consulta da lei falhar. `cancelado` e `superado` ficam fora de `ENCERRADOS`. `app/chamados/juizes.py:409–423,805–848` · `app/chamados/catalogo/motor.py:30–31` · `app/chamados/tests/test_sentinela_volta5_sem_banco.py:109–114` |
| Sem resposta vigente, sem marco fixo e fora do legado, inclusive resposta desfeita com instante físico ainda presente | `PLANTIO_SEM_TS → SEM_RESPOSTA_VIGENTE`. Vínculo sozinho, dentro ou fora da hora residual, e destino sozinho deixam de absolver. Carimbo com trilha de retratação ainda pode absolver. Se o C6 chega a esse degrau, a pergunta segue viva mesmo com o destino simulado como encerrado. `app/chamados/juizes.py:287–298,409–423,801–848,1533–1546` · `app/chamados/models.py:368–391` |
| Sem resposta vigente, com marco fixo e fora do legado | `SLOT → SLOT`, desde que dia e marco sejam deriváveis. A batida precisa provar o slot. A resposta composta posterior ao horizonte continua tendo `VETO_TEMPORAL` **antes** do marco, comportamento **PREEXISTENTE** com prova avulsa e em carga. `app/chamados/juizes.py:368–387,414–423,801–812,863–866` · `app/chamados/services/materializacao.py:624–635,691–710` · `app/chamados/tests/test_marco_por_motivo.py:86–102` |
| Sem resposta vigente, com carimbo legado | Texto horário ainda fornece `SLOT`. Texto inválido ainda deixa alcançar os atalhos de vínculo e destino por `PLANTIO_SEM_TS`. Até um marco fixo pode ser perdido: no ensaio em memória, trocar vazio por `ATESTADO` trocou `SLOT` por `PLANTIO_SEM_TS`, porque o escape é tratado antes de consultar o marco. `app/chamados/juizes.py:409–423,805–848` · `app/chamados/services/materializacao.py:641–645,691–704` |
| Validação vigente, devolução e reabertura | Validação vigente tira a pergunta do universo da cerca do chamado e encerra a leitura do C6 cedo. Ela, por si, não prova uma batida para o juiz de fato. `devolucao` limpa a validação e preserva a resposta. `reabertura` e `recusa` limpam ambas na leitura do ato. As vias próprias, a tranca, a lâmpada e a célula podem decidir antes do slot. `app/chamados/models.py:349–366,4325–4334,4368–4379` · `app/chamados/juizes.py:728–754,1452–1529` |

O aval 1930 está **parcialmente realizado**. A ordem marco fixo → veto foi implementada no caminho comum. `texto_residual nunca decide` continua violado pela guarda de legado. O controle positivo de S1 depende precisamente dessa violação. `app/chamados/juizes.py:409–423` · `app/chamados/services/materializacao.py:691–710` · `app/chamados/tests/test_o261e_f1_porta_encerramento.py:181–198`

### 3. Escritores, leitores e contrato do slot

| Insumo ou saída | Quem escreve e quem lê |
|---|---|
| `resposta_colab`, `respondida_em`, `validada_em`, `decisoes` | A resposta humana grava o par e pode zerar a validação. A automática grava o par e preserva a validação. A cura pela trilha completa o instante. A validação e a devolução gravam coluna e ato. Reabertura e recusa registram a trilha e limpam pela porta em lote. `carimbar` também pode completar instantes físicos. `DecisaoPergunta.registrar` escreve a sequência e invalida cache e memo na instância recebida. `app/chamados/services/disputa_emissao.py:1493–1517,1630–1635,1666–1684,1701–1737` · `app/chamados/services/validacao.py:161–175,276–296` · `app/chamados/models.py:3361–3374,4428–4452` |
| Semântica do par e da validação | A composta entrega instantes do ato e **texto da coluna**. `resposta_display` lê **texto vigente da sequência**. C21 lê a composta no slot, na retenção, na propriedade `respondida`, no lote e na porta de plantio. A guarda de legado lê o par físico e compara a validação física com a composta **anterior ao corte temporal**. É um uso **PREEXISTENTE** do predicado de integridade `texto_sem_carimbo` como condição de lastro. `app/chamados/models.py:2022–2027,3838–3869,4382–4399` · `app/chamados/juizes.py:369–382,409–416,1436–1449,1590–1606` · `app/chamados/services/fio.py:1704–1722` · `app/chamados/services/validacao.py:1224–1228` · `app/chamados/services/materializacao.py:1332–1341` |
| `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo` atribui e as portas gravam. Composta, projeção temporal, C6, juiz de fato, explicador e passivo leem. S144 consulta o juiz de fato e escreve pela porta `carimbar`. `app/chamados/models.py:3324–3328,3384–3405,4382–4390` · `app/chamados/juizes.py:728–754,1436–1468,1533–1546` · `app/chamados/services/explicador.py:122–125` · `app/chamados/services/passivo_validado.py:532–545` · `app/chamados/services/materializacao.py:2205–2221` |
| `pk`, `tipo_resposta`, `motivo`, `data_evento`, disputa, destino, status, `marcos_faltantes`, grade e vínculo | Nascimento fornece identidade e tipo. Fio grava o dia e, com o nascimento, o destino. `_gravar_estado` grava status e `_vivas_entre` simula o pós-estado. O reconciliador monta marcos a partir da célula, o cartório monta a ata e a porta da célula a grava. Slot e derivador leem identidade, contexto, grade e vínculo para achar data, hora e tipo. `app/chamados/models.py:213–308,368–391,3035–3055` · `app/chamados/services/fio.py:843–867` · `app/chamados/reconciliador.py:741–765` · `app/ponto/services/cartorio.py:756` · `app/ponto/portas/celula.py:1032–1065` · `app/chamados/juizes.py:301–336,368–423,572–629` · `app/chamados/services/materializacao.py:556–592,624–710` |
| `agora`, `hoje`, `slots[pk]`, batidas e trilha | A tarefa fornece o horizonte e a carga guarda o `_ResultadoSlot` ou a exceção por PK. O slot relança a exceção quando alcança a entrada. Registro de batida cria a linha, `anotar` grava a origem no reuso e a retratação grava a anulação. O juiz lê janela, origem e trilha por consulta avulsa ou carga. A chave literal `retratar_pergunta_obsoleta` tem escrita verificada nas fixtures. Escritor de produção dessa chave [nao verificado]. `app/chamados/services/tarefa_do_dia.py:520–539,630–637` · `app/chamados/services/carga_pergunta_viva.py:88–95,110–145,186–195` · `app/chamados/juizes.py:240–298,362–387,801–866` · `app/ponto/registro_batida.py:118–180,255–297` · `app/chamados/services/materializacao.py:1578` · `app/chamados/tests/test_marco_por_motivo.py:260–269` |
| `_ResultadoSlot(motivo, ts, tipo)` e seus cinco chamadores | `_slot_esperado` produz a dataclass, que exige `ts` e `tipo` para `SLOT` e os proíbe nas demais saídas. **Nenhum dos cinco desempacota o objeto como tupla.** Juiz e explicador atribuem `ts, tipo` a partir de **dois atributos**. Carga, vigia e `classe_do_lote` leem atributos. A tupla `(ts, tipo)` vem de `_ts_tipo_do_plantio`. A tripla `(p, classe, ts)` vem de `vigia_carimbo.classificar` e é desempacotada pelo censo, pelo passivo e pelo vigia do fio. `app/chamados/juizes.py:346–357,417–423,801–804` · `app/chamados/services/explicador.py:48–62` · `app/chamados/services/carga_pergunta_viva.py:117–145` · `app/chamados/services/validacao.py:1224–1232` · `app/chamados/services/vigia_carimbo.py:106–138` · `app/chamados/services/passivo_validado.py:330–338` · `app/chamados/services/vigia_fio.py:415–418` |

**Efeito semântico nos leitores.** O lote já barra no C21 antes de pedir o slot. O vigia seleciona por **validação física**. Se a coluna sobrou após a reabertura, uma `hora` antes aceita pelo destino pode cair de `com_registro` para `slot_nao_derivavel`. O censo soma essa linha sob `resposta_enviada_sem_gravacao` e o passivo a conta fora do escopo de plantio. O rótulo agregado já existia e agora pode ganhar casos de `hora` sem resposta vigente. O explicador consegue nomear essa causa quando alcança `_explicar_validada`. Incidência real [nao verificado]. `app/chamados/services/validacao.py:1224–1232` · `app/chamados/services/vigia_carimbo.py:97–138` · `app/chamados/services/passivo_validado.py:54–60,330–338` · `app/chamados/services/explicador.py:48–61,122–150`

### 4. Mordida e relógio

- **S1.** O `test_d` invertido exige recusa na porta pública, pergunta ainda viva e chamado aberto. O mutante exige a falha da asserção específica de fechamento. O controle com célula acusando pode passar mesmo sem o veto do slot, pois o C6 retorna antes. O controle positivo grava somente `resposta_colab='17:00'` via `update` e consagra o meio-par que o aval veda. Relógio congelado na classe base. Execução ORM por mim [nao verificado]. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:37–47,168–225` · `app/chamados/juizes.py:1504–1508`
- **S2/S3.** `bc_beco` atesta o `SLOT`, a permissão do C7, o alvo intacto, a recusa e a trilha. Falta garantir que o veto nasça no S144. Acrescentar `bc_beco: 'OK'` torna a igualdade da tabela **mais estrita**, mas fixa uma das duas ordens possíveis. A intenção de quem alterou a tabela [nao verificado]. O caso congela `T0` e avança para `T1` antes do ato. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:57–61,226–232,435–462,537–590` · `app/chamados/services/becos.py:43–61`
- **Volta 5 sem banco.** Rodei **43 testes verdes** em C8b, C8c e Sentinela, incluindo os seis da Sentinela. Os três mutantes deram `rc 1` com asserções específicas: m1 derrubou R1/R2, m2 derrubou R3 e m3 derrubou R5. R5 deixa passar o defeito do legado porque a fixture conserva `respondida_em` físico e não põe carimbo de materialização. O módulo entra pela LU2 e o selo de C8b continua exigindo 86 asserções. A primeira rodada combinada falhou por um caminho relativo do C8c a partir da raiz. Ao rodar de `app/`, os 43 passaram. `app/chamados/tests/e3_lu2_suite.py:8–14` · `app/chamados/tests/test_sentinela_volta5_sem_banco.py:21–33,55–107` · `app/chamados/tests/sentinela_volta5_mutantes.py:14–45` · `app/chamados/tests/test_e4c_f5_duble_conforme.py:14–25,76–80` · `app/chamados/tests/test_e3_c8c_sem_banco.py:105–116`
- **HX.** Relógio solto **PREEXISTENTE**. A fixture usa `timezone.localdate()` e `timezone.now()`, e o juiz avulso pode usar `date.today()`. Os REDs novos do HX ORM verificam a saída anterior à consulta da lei. Os nomes dos testes, sozinhos, não provam que a janela e a classificação da lei foram exercitadas. O HX sem banco passa `hoje` e usa janelas controladas. `app/chamados/tests/test_hx_fallback_sem_lei.py:14–45,61–80` · `app/chamados/juizes.py:633–667,801–803` · `app/chamados/tests/test_sentinela_volta5_sem_banco.py:85–94` · `app/chamados/tests/e3_c8b_apoio.py:167–169`

### 5. Checklist 5(a)

| Item | NOVO × PREEXISTENTE |
|---|---|
| `except` e fallback | **NOVO no delta fornecido: 0 `except` em produção, 0 em testes e 0 fallbacks adicionados.** A saída nova é o veto, que entra no tratamento já existente de carimbo com trilha. **PREEXISTENTES**: o parser converte entrada inválida em ausência de instante, o juiz registra falhas de consulta de batida ou lei e segue, e o C6 captura falha do juiz e devolve `False`. A carga guarda a exceção e a relança no slot. Os oito blobs de saída conferem com os prefixos do patch. O histórico Git da base e a contagem desde uma base anterior a `f3d23021f` estão [nao verificado]. `app/chamados/juizes.py:383–401,409–423,813–848,1539–1546` · `app/chamados/services/materializacao.py:699–704` · `app/chamados/services/carga_pergunta_viva.py:117–128` · `app/chamados/tests/sentinela_volta5_mutantes.py:26–46` |
| Segundo juiz, derivação paralela, campo com dois significados | **NOVO**: `texto_elegivel` passa a comandar também a saída de veto. **PREEXISTENTE**: a exceção de legado usa o meio-par ilegal como insumo de admissibilidade e lê as duas validações. C21 segue sendo chamado e a derivação de `hora` continua compartilhada. A duplicação do parser de `data_hora` permanece preexistente. `app/core/juizes.py:305` · `app/chamados/juizes.py:379–423,1590–1606` · `app/chamados/services/materializacao.py:691–710,1624–1637` |
| Segundo escritor e relógio fora da borda | **Nenhum escritor de produção nem leitura de relógio adicionados pelo delta**. A foto e o retorno são locais. `date.today()` na janela viva e a leitura avulsa de relógio no S144 são **PREEXISTENTES**. Os `update` e `bulk_create` novos são de fixture. `app/chamados/juizes.py:409–423,633–667` · `app/chamados/services/materializacao.py:2200–2221` · `app/chamados/tests/test_e4c_b2_cerca_retratar.py:447–453` · `app/chamados/tests/test_o261e_f1_porta_encerramento.py:185–193` |
| Ordem de trava e foto anterior à trava | **PREEXISTENTE**: ordem Colaborador → Disputa → Chamado → Pergunta, com PK crescente. O beco reúne chamados antes da trava e só depois entra no resolver. A ordem variável encontrada é a dos **atos de resolução**. No lote, classe e conflito continuam avaliados antes da porta. No passivo, ficam as leituras pré-trava descritas no item 1. A mudança da irmã pedida no item 2 do aval 1930 pertence à integração F5. Nesta cópia, `validar_pergunta` ainda lê `_irma` antes do `atomic`. `app/chamados/models.py:2839–2857` · `app/chamados/services/becos.py:43–67` · `app/chamados/services/validacao.py:741–760,1074–1096,1224–1232` · `app/chamados/services/passivo_validado.py:330–384,532–545` |
| Selo e pré-requisito do pouso | **NOVO**: R5 deixa a exceção de legado fora da prova e S3 exige `[OK]` em um caso cujo primeiro veto pode subir diretamente. Pouso, ordem suíte inteira → opus → pouso e sonda por empresa do complemento 2240 [nao verificado]. `app/chamados/tests/test_sentinela_volta5_sem_banco.py:21–33,96–107` · `app/chamados/tests/test_e4c_b2_cerca_retratar.py:84–99,435–462,585` · `CLAUDE.md:320–329,372–381` |

## 21/11 22:46 · sessao 693971cfddfb · E4C-F5-CONSTROI · leitura · origem=Code

# FICHA-INTEGRAÇÃO-F5

Proposta para o aval: **quatro patches finais sobre uma cópia nova do HEAD do main, na ordem 1.1+SENTINELA → 1.2 → 4c.1 → 3.3**. Os diffs têm dois arquivos em comum. Há uma colisão textual em `vigia_carimbo.py` e uma incompatibilidade de contrato entre o novo retorno de `_slot_esperado` e o consumidor introduzido pela 1.2. Esta ficha foi feita em leitura. [medidas.txt:14] [app/chamados/juizes.py:346] [app/chamados/services/vigia_carimbo.py:68, diff 1.2] [CLAUDE.md:344]

## (a) Medidas conferidas

| Sítio e cópia da sessão | HEAD informado | Arquivos vs. `ab351aa9b` | Arquivos vs. `b0935c1f1` | Commits listados desde a base |
|---|---|---:|---:|---:|
| 1.1+SENTINELA · `141636` | `868bfd6371206a00b81b2120f340ff1b1663f4b9` | 134 [nao verificado] | **19**, conferidos no diff e pelos blobs locais | **13** [medidas.txt:8] [commits.txt:1] |
| 1.2 · `193405` | `efc46b0b8985fce730b6d312c1ddefa0fd6c4c18` | 128 [nao verificado] | **14**, conferidos no diff | **13** [medidas.txt:2] [commits.txt:15] |
| 3.3 · `213412` | `7e4b30b7f5c5ec310529f7afcf34875932ad9719` | 121 [nao verificado] | **4**, conferidos no diff | **10** [medidas.txt:11] [commits.txt:29] |
| 4c.1 · `135123` | `af495e9abb22f03efbb4b8e21aad48d0faf6c5f6` | 122 [nao verificado] | **5**, conferidos no diff | **11** [medidas.txt:5] [commits.txt:40] |

**Conferência.** São **42 ocorrências de arquivo nos diffs, 40 caminhos distintos e 47 commits listados**. Os 19 blobs locais da 1.1+SENTINELA batem com o lado novo do seu diff. O checkout de leitura é um `git archive` sem `.git`. Os HEADs vivos, o `status_sujo=0`, o merge-base `ebfdcc4df`, a ancestralidade da base e os totais contra o main são dados da sessão [nao verificado]. [medidas.txt:2] [medidas.txt:14] [commits.txt:1] [CLAUDE.md:344]

**Correções à ficha da sessão.**

- O caminho `161213` do aval está errado. A 3.3 indicada pela medida é `213412`, em `7e4b30b7f`. A fotografia da 4c.1 já inclui a **cura V4**, `af495e9ab`, com o máximo sem `+1 µs`. O resultado do verificador da V4 é [nao verificado]. [aval.txt:1] [medidas.txt:1] [medidas.txt:11] [commits.txt:41] [app/chamados/models.py:1728, diff 4c.1]
- A série completa do chokepoint é **234 → 236 → 238** para um chamado e **298 → 300 → 302** para três. `300` é o intermediário da 3.3. O `237/301` da volta 3 foi corrigido no próprio selo. Os dumps SQL citados pelo selo são [nao verificado]. [app/ponto/tests/test_selo_chokepoint_escrita.py:206] [app/ponto/tests/test_selo_chokepoint_escrita.py:230] [app/ponto/tests/test_selo_chokepoint_escrita.py:205, diff 3.3] [app/ponto/tests/test_selo_chokepoint_escrita.py:241, diff 3.3]
- **357 é uma linha de código.** A lista da 1.2 registra quatro entradas deslocadas. A SENTINELA desloca outras cinco que o registro da cópia local ainda aponta para os números antigos. A catraca executada em leitura mediu **82 funções B1**, **85 itens de derivação** e **9 de pareamento**. [app/core/medida_turno_do_par.py:448] [app/core/tests/test_medida_turno_do_par.py:100] [app/core/tests/test_medida_turno_do_par.py:258] [app/core/tests/test_medida_turno_do_par.py:269, diff 1.2]

## (b) Matriz textual e semântica — perguntas 1 e 3

| Arquivo comum | 1.1+SENTINELA | 1.2 | 3.3 | 4c.1 | Quem ganha e como provar |
|---|---|---|---|---|---|
| `app/chamados/services/validacao.py` | `classe_do_lote`, base a partir de 1199 | `validar_pergunta`, base a partir de 682 | — | — | **Dois trechos disjuntos.** A 1.2 fica com eleição, releitura e trava do plantio dentro do `atomic`, inclusive as guardas antes do carimbo e K6. A SENTINELA fica com C21, motivo `SLOT` e consulta de conflito para o lote A. Provas `ValidacaoPlantioConcorrenteTest`, `BugParIrmaCompostaTest` e os três REDs novos de `FilaValidarLoteTest`. [app/chamados/services/validacao.py:714, diff 1.2] [app/chamados/services/validacao.py:1224] [app/chamados/tests/test_e4c_f5_validacao_plantio.py:1, diff 1.2] [app/chamados/tests/test_fila_validar_lote.py:79] |
| `app/chamados/services/vigia_carimbo.py` | Prefetch, leitura por motivo e descarte da foto de `decisoes` | Extrai `classificar_pergunta`, usada pelo censo e pela releitura do passivo | — | — | **Colisão no corpo do `for` da base, a partir de 105.** A 1.2 fica com a função compartilhada. Dentro dela ganha o contrato da SENTINELA, com `slot.motivo` e `slot.ts`. Preservar o prefetch do censo e fazer a foto inicial do passivo corresponder à classificação inicial. Sob a trava, classificar a instância fresca. Provas O306, REDs de classe e TS do passivo e reconfirmação da composta. [app/chamados/services/vigia_carimbo.py:103] [app/chamados/services/vigia_carimbo.py:65, diff 1.2] [app/chamados/services/passivo_validado.py:362, diff 1.2] [app/chamados/services/passivo_validado.py:456, diff 1.2] [app/chamados/tests/test_o306_vigia_carimbo.py:100] |

`juizes.py`, `models.py`, `materializacao.py`, `reconciliador.py` e `test_e3_c8b_sem_banco.py` têm **um sítio editor cada** nos diffs entregues. A matriz de arquivos comuns da sessão confere. `cortes_f2_f9_apoio.py` também tem um só editor, a 1.2, e a 3.3 importa dele `mutante`. [medidas.txt:14] [app/chamados/juizes.py:360] [app/chamados/models.py:1565, diff 4c.1] [app/chamados/services/materializacao.py:139, diff 1.2] [app/chamados/reconciliador.py:647, diff 3.3] [app/chamados/tests/test_e3_c8b_sem_banco.py:369] [app/chamados/tests/cortes_f2_f9_apoio.py:78] [app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:26, diff 3.3]

| Dependência entre sítios | Quem lê o que o outro muda | Consequência para a costura |
|---|---|---|
| **1.1+SENTINELA → 1.2** | A 1.1 devolve `_ResultadoSlot`. O trecho novo `classificar_pergunta` da 1.2 ainda faz `ts, _tipo = _slot_esperado(p)`. `_reconferir_classificacao` chama esse trecho sob a trava, em outro arquivo. [app/chamados/juizes.py:346] [app/chamados/juizes.py:423] [app/chamados/services/vigia_carimbo.py:68, diff 1.2] [app/chamados/services/passivo_validado.py:459, diff 1.2] | É um erro de desempacotamento ao chegar nesse ramo, mesmo se a extração da função entrar limpa por patch. A adaptação tem um só lugar, `classificar_pergunta`. A guarda de lote A continua exigindo C21 mesmo quando o marco fixo dá `SLOT` ao juiz de lastro. [app/chamados/services/validacao.py:1224] [app/chamados/services/vigia_carimbo.py:112] [app/chamados/tests/test_sentinela_volta5_sem_banco.py:73] |
| **1.1+SENTINELA ↔ 1.2, foto do passivo** | A SENTINELA retira `decisoes` do cache antes do `yield`. A 1.2 obtém `composta(p)` depois de receber a classe e o TS. `composta` usa o prefetch quando presente. [app/chamados/services/vigia_carimbo.py:106] [app/chamados/services/vigia_carimbo.py:122] [app/chamados/services/passivo_validado.py:362, diff 1.2] [app/chamados/models.py:4357] | Colar os dois corpos abre uma janela para classe/TS da foto pré-lida e composta de uma segunda consulta. A foto do censo deve durar até a captura da classificação completa pelo passivo. A releitura sob trava usa a nova instância de `in_bulk`, com seu próprio prefetch. A prova de costura coloca a alteração entre a classificação e a captura da foto, além dos REDs existentes que alteram o estado entre a foto e a trava. [app/chamados/services/passivo_validado.py:397, diff 1.2] [app/chamados/tests/test_e4c_f5_passivo_releitura.py:1, diff 1.2] |
| **1.1+SENTINELA → 3.3** | A 3.3 entrega `fresca` a `carimbar`. A porta chama C6, que, quando chega ao degrau de lastro, lê o resultado novo do slot. O E2 é chamado no caminho de `registrar_batida` medido pelo chokepoint. [app/chamados/reconciliador.py:654, diff 3.3] [app/chamados/models.py:3375] [app/chamados/juizes.py:1539] [app/chamados/juizes.py:801] [app/ponto/services/cartorio.py:1025] | Preservar a ordem **trava → SELECT pelo seletor → prefetch fresco → C20 → porta C6**, com a mesma instância na consulta e na porta. Os testes A2 e os vetos C20 cobram a ordem e a entrada, além do resultado final. [app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:297, diff 3.3] [app/chamados/tests/test_e2_pergunta_so_ao_c6.py:130, diff 3.3] | [nao verificado]
| **1.2 → 3.3** | O RED do prefetch velho chama `_carimbar_validacao`, cujo corpo a 1.2 preserva. A 3.3 importa `mutante` de um arquivo que a 1.2 edita para extrair a fixture. [app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:339, diff 3.3] [app/chamados/services/validacao.py:161] [app/chamados/services/validacao.py:682, diff 1.2] [app/chamados/tests/cortes_f2_f9_apoio.py:36, diff 1.2] [app/chamados/tests/cortes_f2_f9_apoio.py:78] | São dependências compatíveis no diff. O RED da 3.3 continua isolando a validação na sequência. A prova da porta pública `validar_pergunta` vem da 1.2. [app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:339, diff 3.3] [app/chamados/tests/test_e4c_f5_validacao_plantio.py:1, diff 1.2] |
| **4c.1 → 1.1, 1.2 e 3.3** | A 4c.1 muda a ordem efetiva da `REABERTURA` compensadora. O slot da 1.1, a eleição e a reconfirmação da 1.2 e o C6 chamado pela 3.3 leem a composta. A sonda da própria 4c.1 chama C6 depois de limpar a materialização na cópia. [app/chamados/models.py:1728, diff 4c.1] [app/chamados/models.py:4367] [app/chamados/models.py:1581] [app/chamados/juizes.py:371] [app/chamados/services/materializacao.py:145, diff 1.2] [app/chamados/services/passivo_validado.py:456, diff 1.2] [app/chamados/models.py:3375] | Provar ato e coluna juntos no empate e no recuo. O RED de reabertura concorrente da 1.2 usa `reabrir_para_correcao`, portanto a prova da compensadora V4 vem dos REDs da 4c.1. [app/chamados/tests/test_e4c_f5_plantio_concorrente.py:1, diff 1.2] [app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:1, diff 4c.1] |

**Ordem proposta.** Primeiro a 1.1 fixa o contrato do slot. A 1.2 entra logo depois e resolve a única colisão textual já contra esse contrato. A 4c.1 fixa a borda do escritor da reabertura. A 3.3 entra por último, de modo que a leitura fresca do E2 e o custo C8 sejam verificados com os dois escritores e o juiz já na árvore. A ordem entre 4c.1 e 3.3 é uma escolha de verificação. Os dois diffs têm arquivos disjuntos. [medidas.txt:14] [app/chamados/juizes.py:360] [app/chamados/services/vigia_carimbo.py:103] [app/chamados/models.py:1728, diff 4c.1] [app/chamados/reconciliador.py:647, diff 3.3]

### Selos na árvore candidata

| Selo | O que registrar |
|---|---|
| **Chokepoint** | **238/302** são os tetos da 3.3 e o ponto de partida da comparação. Recontar na candidata os cenários de um e três chamados fechados, os controles em que ficam vivos e os quatro `on_commit` de cada cenário de fecho. O custo direto declarado do E2 é **N emitidas + K sobreviventes + D com empresa e dia**, mais leituras de toque quando houver tranca. A contagem integrada de SQL e `on_commit` é [nao verificado]. Nenhuma soma de tetos entre sítios substitui a medição. [app/ponto/tests/test_selo_chokepoint_escrita.py:205, diff 3.3] [app/ponto/tests/test_selo_chokepoint_escrita.py:283] [app/ponto/tests/test_selo_chokepoint_escrita.py:319] [app/ponto/tests/test_selo_chokepoint_escrita.py:467] |
| **Catraca B1** | **Cinco entradas medidas na SENTINELA**: em `juizes.py`, `_dia_da_batida` **55**, `_dia_do_contexto_fraco` **171**, `_plantio_na_hora_do_slot` **276**, `dias_do_fato` **205**. Em `explicador.py`, `_explicar_validada` **77**. **Quatro esperadas pelo diff 1.2**: `materializacao._plantio_preenche_furo_na_data` **448/469**, `passivo_validado._dia_do_plantio` **118**, `_plantar_o_dia` **595/597**, `reparar` **357**. Rodar `CatracaTurnoDoParTest` para conferir linhas, identidades e totais da candidata. [app/core/tests/test_medida_turno_do_par.py:258] [app/core/tests/test_medida_turno_do_par.py:269, diff 1.2] [app/core/tests/test_medida_turno_do_par.py:542] [app/core/tests/test_medida_turno_do_par.py:558] |
| **`e4b_c12`** | Recalcular as linhas pelo escritor. Com a base entregue e o delta líquido de **+16** da V4 antes dos dois chamadores, os valores esperados são **1791** para `encerrar`, **1811** para `encerrar_por_competencia_trancada` e **1508** para `reabrir`. O corpo nominal continua com **9** entradas e exceções vazias. Rodar a varredura de escritores e os mutantes do selo. [app/chamados/models.py:1508] [app/chamados/models.py:1775] [app/chamados/models.py:1795] [app/chamados/tests/e4b_c12_ast.py:25, diff 4c.1] [app/chamados/tests/test_e4b_c12_selo.py:22] [app/chamados/tests/test_e4b_c12_selo.py:34] |
| **`ASSERTS_MAIN`** | O AST do selo contou **86** no c8b e **29** no pós-suite. Os dois asserts com `juiz_de_fato.assert_called_once_with` ficam fora do contador, que conta `assert` e chamadas `self.assert*`. O módulo separado da SENTINELA tem **17** pelo mesmo contador e entra via `e3_lu2_suite`. Preservar 86/29 e recontar na candidata. [app/chamados/tests/test_e4c_f5_duble_conforme.py:15] [app/chamados/tests/test_e4c_f5_duble_conforme.py:28] [app/chamados/tests/test_e3_c8b_sem_banco.py:389] [app/chamados/tests/test_sentinela_volta5_sem_banco.py:55] [app/chamados/tests/e3_lu2_suite.py:11] | [nao verificado]

## (c) Formas de integração — pergunta 2

| Desenho | Custo e consequência |
|---|---|
| **Merge das quatro cópias na raia de integração** | Quatro incorporações, com ancestralidade preservada. Requer auditar o conteúdo comum desde o merge-base reportado e o delta contra o main, além de resolver `vigia_carimbo` e provar os contratos cruzados. Viável se a inclusão do histórico e da base comum estiver coberta pelo aval. [medidas.txt:2] [medidas.txt:14] [CLAUDE.md:37] |
| **Cherry-pick por commit** | Até **47 reaplicações**, com granularidade para atribuir regressão e bissetar. Exige revisar os intermediários, incluindo as reversões de teto da 3.3 e `959b9a5c6`, descrito como medição da opção B que não pousa. O custo de revisão é maior porque as voltas intermediárias também precisam ser triadas. [commits.txt:1] [commits.txt:29] [commits.txt:47] |
| **Patch por sítio sobre cópia nova do main — proposta** | Quatro deltas líquidos, **40 caminhos** contra a base informada, dois arquivos para compor e selos para recontar. Mantém os quatro SHAs como proveniência e evita reaplicar voltas intermediárias. Antes de aplicar, conferir no Git da sessão quais pré-requisitos de `b0935c1f1` estão no HEAD de destino e inventariar o que falta. O conjunto `main ↔ base F5` é [nao verificado]. [medidas.txt:2] [medidas.txt:14] [commits.txt:47] [CLAUDE.md:39] |

## Rol nominal de provas — pergunta 4

**1.1+SENTINELA**

- `MarcoPorMotivoTest` inteiro. Inclui marco pelo motivo, empate, ato desfeito versus posterior, veto com vínculo e com destino encerrado, `data_hora` desfeita, ausência de resposta com vínculo/destino, retratação com carimbo e trilha, marco fixo, exceção em cache e horizonte divergente antes da leitura. Rodar ainda `DerivacaoUnicaTest.test_MORDE_MA3_juiz_e_plantio_derivam_o_mesmo_slot`. [app/chamados/tests/test_marco_por_motivo.py:77] [app/chamados/tests/test_marco_por_motivo.py:185] [app/chamados/tests/test_marco_por_motivo.py:284] [app/chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:96]
- `SentinelaVolta5` inteiro, incluindo r1–r5 e `plantio_sem_ts_de_resposta_vigente`. Mutantes `m1` → r1/r2, `m2` → r3 e `m3` → r5. Rodar `FallbackSemLeiTest`, `F1PosEstadoTest` com `test_mutante_sem_veto_de_resposta_ausente` e o controle com batida no slot. Para `bc_beco`, rodar `CercaRetratarTest.test_bc_beco` e `CercaMordeTest`, em particular `sem_cerca` e `sem_escopo:fechar_beco`, com `[ALVO]` no PK do caso. [app/chamados/tests/test_sentinela_volta5_sem_banco.py:55] [app/chamados/tests/sentinela_volta5_mutantes.py:16] [app/chamados/tests/test_hx_fallback_sem_lei.py:66] [app/chamados/tests/test_o261e_f1_porta_encerramento.py:173] [app/chamados/tests/test_o261e_f1_porta_encerramento.py:212] [app/chamados/tests/test_e4c_b2_cerca_retratar.py:435] [app/chamados/tests/test_e4c_b2_cerca_retratar.py:572]
- `FilaValidarLoteTest`, com os três REDs de resposta desfeita, data impossível e erro de consulta de conflito. `ExplicadorDisputaTest.test_RED_explicador_nao_cita_texto_residual_como_resposta_vigente`, `VigiaCarimboSemRegistroTest`, `AlimentacaoC6` do c8b e `PosSuiteSemBanco.test_resposta_futura_nao_vence_celula_nem_alimenta_slot`. Rodar também `SeloChamadaTest`, `SeloPurezaTest` e `DublePerguntaConformeTest` com o mutante de assert removido. [app/chamados/tests/test_fila_validar_lote.py:79] [app/chamados/tests/test_explicador_disputa.py:80] [app/chamados/tests/test_o306_vigia_carimbo.py:100] [app/chamados/tests/test_e3_c8b_sem_banco.py:369] [app/chamados/tests/test_e3_pos_suite_sem_banco.py:103] [app/chamados/tests/test_e4c_f5_selo_chamada.py:188] [app/chamados/tests/test_e4c_f5_duble_conforme.py:82]

**1.2**

- `BugParIrmaCompostaTest` inteiro. Irmã vigente sem instante físico, desfeita com carimbo, duas irmãs vigentes e competência entregue sem escrita. `BugParPlantioConcorrenteTest` inteiro. Barreira rompida, duas ordens inversas, reabertura entre eleição e trava, irmã que só fica única depois da trava, todas as candidatas e mudança de conjunto com eleição igual. Mordidas nominais: candidata sem PK travado, principal sem PK travado, principal com dados mudados e restauração do contexto em exceção. [app/chamados/tests/test_bug_par_projetado.py:1351, diff 1.2] [app/chamados/tests/test_e4c_f5_plantio_concorrente.py:1, diff 1.2]
- `GuardaPlantioS144ConcorrenteTest.test_RED_passivo_foto_velha_recusa_escopo_e_mesa_valida_mesmo_pk`, `GuardaPlantioS144Test.test_RED_passivo_reconfirma_a_composta_da_principal`, o controle `test_RED_passivo_b_persistido_recusa_par_e_mesa_valida_mesmo_pk` e os casos S144. Em `PassivoReleituraTest`, rodar conferência da avulsa, exportação nova, data do evento, corte da empresa, slot do contexto, marco fixo, classe por batida fora do slot e TXT invalidado/vigente. Mutantes `f5_passivo_sem_data_conferida_em`, `f5_passivo_sem_exportada`, `f5_passivo_sem_dia`, `f5_passivo_sem_competencia`, `f5_passivo_sem_ts` nos dois casos e `f5_passivo_sem_classe`. [app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:162, diff 1.2] [app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:295, diff 1.2] [app/chamados/tests/test_e4c_f5_passivo_releitura.py:1, diff 1.2]
- `ValidacaoPlantioConcorrenteTest.test_RED_validacao_recusa_irma_com_escopo_alterado_em_outra_conexao` e `test_controle_validacao_planta_o_par_sem_mudanca`. Mutantes `RunnerMutanteM1` (`f5_validacao_m1`, leitura antes do `atomic`) e `RunnerMutanteM2` (`f5_validacao_m2`, trava anterior dentro do `atomic`). Repetir os vizinhos `test_unidade_entregue_sai_antes_de_recusa_e_carimbo`, `BugPar5644Test`, `FormaDaPortaTest.test_reparar_none_recusa_sem_escrever` e `test_o199_aceite_de_folga_decide_o_dia`, pois o aceite também passa a entrar depois da trava externa. [app/chamados/tests/test_e4c_f5_validacao_plantio.py:1, diff 1.2] [app/chamados/tests/test_bug_par_sem_banco.py:152] [app/chamados/tests/test_bug_par_volta.py:430, diff 1.2] [app/ponto/tests/test_cadastro_futuro_c5e.py:230] [app/chamados/services/validacao.py:710, diff 1.2] [app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:140]

**3.3**

- Em `E2PerguntaSoAoC6Test`, rodar `test_f5_e2_absolvicao_resposta_entre_prefetch_e_trava_preserva_pergunta_viva`, `test_f5_e2_absolvicao_sem_corrida_carimba_e_conta`, `test_f5_e2_tranca_entre_emissao_e_trava`, `test_f5_e2_tranca_entre_emissao_e_trava_em_request` e `test_f5_e2_tranca_le_corte_novo_dentro_de_consulta_externa`. Mutante `f5_e2_carimbo_sob_veto_c20` nos três vetos. Cobrar **zero entradas** na porta quando C20 veta. [app/chamados/tests/test_e2_pergunta_so_ao_c6.py:29, diff 3.3] [app/chamados/tests/test_e2_pergunta_so_ao_c6.py:130, diff 3.3]
- Em `GuardaReconciliadorTest`, rodar `test_A2_evento_celula_absolveu_entrega_coluna_nula_e_resposta_none`, `test_A2_evento_celula_absolveu_exclui_validada_apos_selecao` e `test_RED_prefetch_velho_esconde_validacao_so_na_sequencia`. Mutantes `f5_e2_sql_antes_da_trava` nos dois A2 e `f5_e2_prefetch_velho` no RED da sequência. Rodar os controles A2 da órfã e o chokepoint com um e três chamados, 

[truncado em 20000 de 27281 caracteres]

## 21/11 22:09 · sessao 528e13d013f8 · E4C-F5-CONSTROI · construtor · origem=Code

**INCOMPLETO.** Os dois pedaços estão no diff da cópia. O HEAD segue em `f1d73910ba305d4ca43e61f4dd33c640d83ae6dd`, conforme a regra que proíbe `git commit`.

- **S1 — +44/−8.** O RED exige `reconciliar() == False`, pergunta viva e chamado `aberto`. O controle mantém a mesma batida e acrescenta apenas a hora elegível ao carimbo legado. Com o slot comprovado, exige que o chamado feche. O `test_d_RED_MORDE` foi preservado e o novo mutante exige que o RED morda ao restaurar o veredito antigo. [RED e controle](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/tests/test_o261e_f1_porta_encerramento.py:173) · [Mutante](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/tests/test_o261e_f1_porta_encerramento.py:212)
- **S2 — +15/−1.** `bc_beco` agora verifica o slot da resposta validada e confirma que o C7 permite fechar antes do ato cercado. O mutante e o harness foram preservados. O veredito esperado continua sendo `ALVO`. [Fixture](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/tests/test_e4c_b2_cerca_retratar.py:435) · [Veredito esperado](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/tests/test_e4c_b2_cerca_retratar.py:580)

O `git diff --check` passou, os dois arquivos compilaram em memória e os seis testes existentes da sentinela sem banco passaram. A execução dos recortes e a mordida dos mutantes ficam **[nao verificado]**. A pista usa Docker e banco. [Sentinela sem banco](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/tests/test_sentinela_volta5_sem_banco.py:21) · [Pista](/home/ronald/wt-codex-e4c-f5-constroi-141636/bin/suite.sh:316) [nao verificado]

```bash
copia=/home/ronald/wt-codex-e4c-f5-constroi-141636
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k F1PosEstadoTest
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k CercaMordeTest
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_d_RED_hora_sem_resposta_segue_viva_e_impede_fechamento
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_mutante_sem_veto_de_resposta_ausente
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_mutante_sem_escopo_fechar_beco
```

**Autoconferência final — except/fallback novos no diff: 0.** `git diff | grep -n except` sem ocorrências.

## 21/11 22:02 · sessao e24f7350c635 · WRAPPER-N-VAGAS · verificador · origem=Code

BLOQUEIA

**NOVO contra `e1ac53dce`.** Uma vaga livre pode recusar todas as tentativas por causa de um JSON v1 deixado pela versão anterior. Uma vaga ocupada pelo lançador legado pode fazer o portão comparar os arquivos da chamada errada. E a espera nova do gate pode girar indefinidamente se a abertura do lock falhar. São contraexemplos de código. Ocorrência em produção e os resultados informados de 38 casos, 55 mutantes e 82 mutantes ficam **[nao verificado]**. A classificação do delta usa o patch fornecido. Conferi a sintaxe dos scripts e do helper em leitura; a identidade Git da cópia fica **[nao verificado]**. [`bin/codex_pergunta.sh:342–356,522–543,554–558,667–678`](bin/codex_pergunta.sh#L342) · [`bin/codex_admissao.py:139–162`](bin/codex_admissao.py#L139)

### 1. Decisões que mudam

| Estado | Decisão |
|---|---|
| **PREEXISTENTE** · Meta de admissão ocupada ou capacidade esgotada | Espera a meta até `meta_timeout_s`. No vencimento, `75 PAUSADO` se a flag estiver presente, senão `75 META_OCUPADA`. Capacidade esgotada dá `75 SEM_VAGA` sem esperar vaga. `bin/codex_pergunta.sh:500–506,511–524` |
| **NOVO** · Há outra vaga viva | O portão soma o delta commitado aos previstos de ambas as partes e mantém o `git status` da admitida. Conflito ou `COMUM-NAO-SEI` dá `2 PORTAO`. Sem outra vaga viva, pula M3, como na base. `bin/codex_pergunta.sh:279–320,525–543` |
| **NOVO** · Vaga escolhida, antes de `abriu` | Lê a geração da pausa sob a meta. Valor não numérico dá `2 PRE_CHECK`. Publica JSON v2 e arquiva o anterior. JSON anterior não vazio sem ID válido, histórico divergente ou erro na publicação dá `3 FALHA_TRILHA`, sem `abriu`. O `1` do projetor é interno. `bin/codex_pergunta.sh:342–362,392,501–507,552–558` |
| **NOVO** · Admitida esperando gate, slot ou resposta | Gate e slot repetem fatias de 1 s, sem prazo total. A construtora solta o gate para esperar o slot. Pausa viva dá `75 PAUSADO`. Na resposta, esgotar as tentativas sem pausa dá `3` e a chamada preserva o `.rc` alheio enquanto não pegou a trava. Os outros papéis seguem pelos ramos anteriores. `bin/codex_pergunta.sh:570–577,667–699,708–723` |
| **NOVO** · Com slot e resposta, antes do executor | Readquire a meta. Vencimento dá `75 PAUSADO` ou `75 META_OCUPADA`. Abertura da meta falha em `2 PRE_CHECK`. Pausa ou geração diferente dá `75 PAUSADO`. N inválido, lease recusada, worktree alterada ou insumo divergente dão `2 REVALIDACAO`. Falha na projeção de `iniciou` dá `3 FALHA_TRILHA`. Sucesso publica `iniciou`, libera a meta e envia `GO`. `bin/codex_pergunta.sh:739–778` |
| **NOVO** · Fecho | Persiste `fechou/rc/motivo` no JSON do mesmo ID e acrescenta o motivo no 10º campo de `chamadas.log` para o construtor. Falha de trilha converte `0` em `3` e preserva um rc já não zero. A normalização de `2` ou `75` bruto do executor para `1 FALHA_EXECUCAO`, sem evidência de limite, é **PREEXISTENTE**. `bin/codex_pergunta.sh:372–376,407–416,568–583,820–827` |

**Admissão simultânea indevida.** **NOVO** · A termina e deixa seu JSON na vaga 1. O lançador legado pega só a trava gêmea. B vê a vaga 1 ocupada, mas compara contra o JSON de A. Se a worktree de A continua legível e é disjunta de B, B pode entrar mesmo com arquivo em comum com o legado. O selo de estado alheio limpa os JSONs antes do cenário. **PREEXISTENTE** · Duas chamadas da mesma obra podem usar a mesma worktree limpa com briefs citados disjuntos. O código confere o nome e o registro Git da worktree, mas não compara sua identidade com a da ocupante. Também aceita `app/folha/../ponto/a.py` e `app/ponto/a.py` como declarações disjuntas quando os deltas estão vazios. `bin/codex_admissao.py:139–162` · `bin/codex_pergunta.sh:266–278,298–320,407–415,454–474,519–549` · `bin/disjuntor.py:1526,1612–1645` · `bin/tests/test_codex_n_vagas.sh:212–215,346–352`

**Recusa ou espera sem saída própria.** **NOVO** · Um JSON v1 não vazio deixado por morte abrupta fica preservado. A próxima tentativa escolhe a mesma menor vaga livre e volta a sair `3` até intervenção. O selo exige essa recusa para registro sem ID. Um `slot.gate` que virou diretório deixa o `exec 9>` falhar; o laço interpreta o erro de `flock` como contenção e repete com a lease presa. Um FIFO em `pausa.seq` ou no JSON anterior também pode bloquear leitura sob a meta. **PREEXISTENTE** · Gate ou slots legitimamente retidos sem soltura já podiam deixar a espera sem prazo. `bin/codex_admissao.py:146–151` · `bin/codex_pergunta.sh:72,329–356,397–398,501–507,554–556,667–698` · `bin/tests/test_codex_n_vagas.sh:574–577`

### 2. Pulso durante a espera do gate

**NOVO** · Com o gate ainda preso, a primeira fatia malsucedida depois da publicação vê a flag ou a geração alterada e sai `75 PAUSADO`. Em contenção normal, isso leva até aproximadamente **1 s mais o custo de execução e leitura**, contado do ato efetivo sobre a flag ou a geração. Se o `flock` adquire o gate na fatia, o ramo de sucesso pula essa consulta. A chamada segue e pode detectar a pausa na espera do slot, na resposta ou em M6. Ao passar do slot de volta para um gate ocupado, pode gastar outra fatia. M6 só consulta a pausa depois de adquirir a meta ou ao vencer seu prazo. `bin/codex_pergunta.sh:397–398,672–692,714–718,744–748`

**Trilha e relógio.** O prazo de cada fatia vem de `flock -w 1`. `date +%s` mede a espera e a duração para exibição. Num cancelamento no gate, `vagas.log` registra `abriu/fechou` sem causa. O JSON registra `75/PAUSADO` com `iniciou` nulo, `chamadas.log` registra `fim` com o motivo, e stderr identifica a espera do gate e emite `CODEX_RESULTADO`. Sem a trava da resposta, não há `.rc` novo. O leitor de `vagas.log` continua limitado aos cinco campos e o leitor de `chamadas.log` aceita o décimo, mas só consome os nove anteriores. `bin/codex_pergunta.sh:372–376,391–393,407–415,557–558,568–583,611–612,669–700` · `bin/codex_vagas_estado.py:20–49` · `bin/codex_limite_estado.py:49–68`

### 3. M3 e o “não sei”

| Caso | Saída |
|---|---|
| **NOVO** · `merge-base` ausente, ou diff falha, com outra vaga viva | `COMUM-NAO-SEI`, portanto `2 PORTAO`. Na própria worktree, timeout ou erro de decodificação pode derrubar o Python sem linha de resultado, e o wrapper também recusa. Na outra worktree, a exceção vai para o `except` preexistente. `bin/codex_pergunta.sh:279–313,540–543` |
| **NOVO** · `HEAD` destacado | Funciona quando `refs/heads/main` existe no repositório compartilhado e há ancestral comum. Os casos do selo usam worktrees destacadas. `bin/codex_pergunta.sh:279–288,469–472` · `bin/tests/test_codex_n_vagas.sh:149–151,323–343` |
| **PREEXISTENTE / NOVO** · Worktree sem `main` | Estar destacada em vez de estar na branch `main` é válido. Se a ref compartilhada está ausente na viva já no pre-check, dá `2 PRE_CHECK`, **PREEXISTENTE**. Se a ref deixa de estar disponível quando M3 roda, dá `2 PORTAO`. Se o blob de configuração fica ilegível na autorização, dá `2 REVALIDACAO`, **NOVO**. `bin/codex_pergunta.sh:279–288,425–430,540–543,749–752` |
| **PREEXISTENTE / NOVO** · Previstos vazios | Para quem entra, `if not meus` fecha antes do delta, **PREEXISTENTE**. Para a admitida, `decl` guarda o tamanho da lista original, de modo que status e delta não preenchem o vazio declarado, **NOVO**. Em concorrência, ambos dão `2 PORTAO`. Sozinha, a chamada pode entrar sem previstos ou sem ancestral comum, pois M3 é pulado. O CENSO mantém sua decisão própria para obra da fila 1. `bin/codex_pergunta.sh:289–315,483,540–549` · `bin/disjuntor.py:1723–1747` |

### 4. Escritor e leitores da pausa

- **NOVO** · `pausar.sh` tenta publicar e retirar sob a meta derivada do caminho fornecido. `--soltar` avança `pausa.seq` antes de remover a flag. **PREEXISTENTE** · `--ler` exige `-f`, exibe o conteúdo e calcula idade pelo mtime. `SAIDA` é texto obrigatório na publicação. A volta depende da retirada manual. `PAUSOU` e `SOLTOU` vão para `logs/pausas.log` da raiz **do script**, mesmo quando o alvo pertence a outra raiz. `bin/pausar.sh:14–50,53–72`
- **PREEXISTENTE** · O wrapper aponta para a viva pelo `git-common-dir`. Lê a presença no pre-check, no vencimento da meta de admissão e ao obtê-la. **NOVO** · Lê a sequência em M4 e usa flag ou diferença de geração no gate, slot, resposta e M6. Um pulso inteiro antes da amostra de M4 pode passar despercebido. O PreToolUse configurado trata B3, cota e CENSO, sem ler essa flag. O vigia que interpreta `QUEM/SAIDA` enumera as duas flags da esteira. `bin/codex_pergunta.sh:90–98,397–398,424,440,501–507,674–675,689–692,715–718,744–748` · `.claude/settings.json:62–69` · `bin/disjuntor.py:1851–1863` · `app/core/esteira_vigia.py:467–486`
- **ESMERIL-SEMANTICA.** **PREEXISTENTE** · Para o wrapper um diretório conta como pausa por `-e`, enquanto `--ler` e `--soltar` exigem `-f`. Os selos legados gravam só `QUEM`, inclusive diretamente enquanto a meta está retida. **NOVO** · O escritor pode resolver `pausas/../vagas` através de um symlink e ordenar em uma meta diferente da viva usada pelo wrapper. Dois `--soltar` podem passar pelo teste de presença antes da meta e incrementar duas vezes por uma única retirada, cancelando uma chamada admitida entre eles. Os selos novos leem presença e geração; não verificam `SAIDA` nem `pausas.log`. `bin/pausar.sh:23–50` · `bin/codex_pergunta.sh:397–398,424,440,491,501–507` · `bin/tests/test_codex_pergunta.sh:170–182,261–278,311–317` · `bin/tests/test_codex_n_vagas.sh:398–413,597–639,666–679`
- **PREEXISTENTE, ampliado pela espera nova.** A preparação usa o mesmo `$f.novo` antes da meta. Duas publicações podem trocar o conteúdo atribuído ao autor. O script também pode anunciar `PAUSOU` sem validar o `mv`, ou registrar `SOLTOU` e sair `0` mesmo se o `rm` falhar. `bin/pausar.sh:39–50,61–72`

### 5. Selos

**NOVO** · São **21 mutantes novos**, um removido (`limpa_estado`) e um preexistente reaplicado (`publica`). A tabela abaixo identifica a asserção de efeito disponível. Qual asserção matou cada mutante na execução informada fica **[nao verificado]**. O motor aceita qualquer falha com o prefixo esperado, inclusive uma falha de diagnóstico sob o mesmo rótulo. `bin/tests/test_codex_n_vagas.sh:184–198,691–707,735–745,757–781`

| Mutantes | Efeito que o caso exige |
|---|---|
| `m3_decl`, `m3_minha`, `m3_outra`, `m3_pontas` | `2` e Codex não invocado, com controle disjunto que entra. `bin/tests/test_codex_n_vagas.sh:189–198,323–343` |
| `pausa_seq`, `pausa_gate`, `gate_solta`, `pausa_slot`, `pausa_resp`, `rc_sidecar` | Saída com a trava ainda retida, gate adquirível por outro, `75 PAUSADO` ou bytes do `.rc` alheio preservados, conforme o mutante. `bin/tests/test_codex_n_vagas.sh:598–638,695–700` |
| `m6_meta`, `m6_confere`, `m6_inicia`, `adm:confere_ino`, `adm:confere_n` | Recusa com rc de negócio e sem Codex nas respectivas condições de meta, N, insumo e inode. `bin/tests/test_codex_n_vagas.sh:641–663,701–705` |
| `pz:meta`, `pz:seq` | Flag muda somente depois de soltar a meta e retirada produz sequência `1`. `bin/tests/test_codex_n_vagas.sh:605–610,666–679,706–707` |
| `hist_arquiva`, `proj_fecha`, `proj_dono`, `fim_motivo` | Histórico anterior legível, fecho persistido, rc `3` por dono divergente e 10º campo com o motivo. `proj_dono` verifica apenas a presença do ID antigo, que pode permanecer num JSON indevidamente alterado. `#V:publica` pode ser contado morto pelo erro de lançamento de `_prende` sob o mesmo rótulo da asserção de conteúdo. `bin/tests/test_codex_n_vagas.sh:169–176,438–458,546–577,735–744` |

**Vacuidade parcial nova.** `m5_gate_preso` pode passar com a flag ainda presente porque descarta as duas saídas de `pausar.sh` e não prova retirada nem avanço da sequência. `_ate_abriu` pode retornar sem um novo `abriu`; os casos M5 podem, assim, observar a recusa em outra fase. No selo legado `pausa_reval`, a espera da resposta agora pode detectar a pausa antes de M6, dar o `75` correto e falhar só pelo texto esperado. M3 usa o CLI `--censo` falso e não exercita ref ou merge-base ausente. `bin/tests/test_codex_n_vagas.sh:34–47,323–343,589–623` · `bin/tests/test_codex_pergunta.sh:261–278` · `bin/codex_pergunta.sh:715–718,748`

### 6. Checklist

- **Except e fallback.** **NOVO** · `0` cláusulas Python `except`, `0` `|| true` literais e `1` `raise OSError` capturado pelo `except` **PREEXISTENTE**. Há **1** `2>/dev/null` novo em produção (`_N6`), além dos dois sítios preexistentes movidos com `_COM` e `fim`. No selo são **21** literais adicionados e **3** retirados, mais **7** chamadas a `pausar.sh` que descartam stdout e stderr. Entre os tratamentos novos, os dois retornos `None` do Git fecham M3, sequência ausente vira `0`, e os três `flock -w 1` tratam qualquer falha como contenção. A checagem de `CONFERE` usa o conteúdo e abandona o status do `read`. Os `rev-parse` na publicação também podem deixar metadado vazio. `bin/codex_pergunta.sh:279–313,397–398,540,554–555,573–574,674,689–692,715,749–752` · `bin/pausar.sh:25–26,44–47` · `bin/tests/test_codex_n_vagas.sh:441–458,551–573,593–596,605–645,669–675`
- **Segundo escritor.** **PREEXISTENTE** · A reserva de caminhos filtra `RESPOSTA`, mas `CODEX_LOGD` pode criar diretório em um caminho reservado sob os logs da própria raiz. A interface genérica de `pausar.sh` pode substituir ou remover inode e JSON de vaga fora da porta. **NOVO** · Essas vias também alcançam os artefatos novos `pausa.seq` e `historico`. `bin/codex_pergunta.sh:98–99,122,442–443,475,562` · `bin/pausar.sh:23–25,39–50,53–69`
- **Ordem de trava.** **NOVO** · M6 pega meta com lease, slot e resposta já presos. O caminho de admissão vai de meta para vaga, mas tenta a vaga com `LOCK_NB`. Não há ciclo bloqueante entre esses caminhos normais. `CONFERE` aplica `outras + 1 <= N`; se a chamada ficou em `k > N` mas o total baixou até N, ela inicia. A seleção inicial exige `k <= N`. `bin/codex_pergunta.sh:500–524,699–723,739–766` · `bin/codex_admissao.py:55–60,84–103,139–151`
- **Relógio e significado.** **NOVO** · `iniciou` carimba a projeção da autorização, antes do `GO` e do `Popen`. O `abriu` do JSON usa `time.strftime` antes dos hashes e o `abriu` da trilha usa outro `date` depois da publicação. `QUANDO` da pausa é escrito antes da meta e pode anteceder a ativação, que tem outro carimbo em `PAUSOU`. A sequência é texto comparado por desigualdade no shell, inteiro no JSON e aritmética Bash no escritor. Por exemplo, `08` passa a regex e dá `rc 1` em `--soltar` antes da remoção. A janela do portão continua lendo `date +%H%M` na admissão, **PREEXISTENTE**. `bin/codex_pergunta.sh:335,357–371,397–398,507,534–537,557,760–778` · `bin/codex_admissao.py:164–177` · `bin/pausar.sh:44–49,61–70`
- **Insumo autorizado.** **NOVO** · M4 guarda o hash dos caminhos já resolvidos. O prompt lê os argumentos originais, e M6 confere os caminhos gravados. Se um symlink de pergunta ou anexo mudar de alvo entre M4 e a montagem do prompt, M6 pode validar o arquivo antigo intacto e autorizar bytes do novo alvo. Também não relê os briefs usados para formar `arq`, salvo a conferência de bytes quando forem anexos. `bin/codex_pergunta.sh:221–256,357–371,482–490,555,634,661,760–765`

## 21/11 22:02 · sessao 2c512c616764 · - · - · origem=Ronald (tmux)

BLOQUEIA

- **NOVO — espera sem saída.** Se a abertura de `slot.gate` falhar, o Bash continua com FD inválido. `_gate` repete qualquer erro do `flock` como se fosse contenção, segurando a vaga até observar pausa. `pausa.seq` FIFO também pode bloquear indefinidamente a consulta inicial sob a meta, o polling e o `--soltar`. `bin/codex_pergunta.sh:72,397–398,501–507,667–678`, `bin/pausar.sh:42–45`.
- **NOVO — retirada duplicada.** Dois `--soltar` podem observar a mesma flag antes da meta. O primeiro remove e incrementa. Uma chamada pode ser admitida depois, e o segundo incrementa sem reconferir a flag, cancelando essa chamada por uma retirada que já ocorreu. **PREEXISTENTE — publicação concorrente.** O `.novo` compartilhado é escrito antes da trava, de modo que uma publicação pode renomear o conteúdo da outra com autor diferente na trilha. `bin/pausar.sh:39–50,61–72`, `bin/codex_pergunta.sh:397–398,501–507`.
- **NOVO — M5 em condições normais.** Gate ocupado continuamente implica consulta a cada falha genuína de `flock -w 1`, cerca de uma fatia mais custo do processo. A aquisição bem-sucedida pula a leitura e segue para slot/resposta/M6. A resposta soma tentativas falhas, não mede tempo decorrido. Gate/slot têm espera sem prazo, **PREEXISTENTE**. `bin/codex_pergunta.sh:667–700,711–723,744–748`.
- **Trilha e relógio.** `vagas.log` só tem `abriu/fechou`, JSON fecha com `75/PAUSADO` e `iniciou` nulo antes de M6, `chamadas.log` tem o novo motivo no `fim`. Sem trava da resposta, nenhum `.rc` novo. `iniciou` é autorização, gravada antes de liberar a meta e antes do GO. `date` lê a janela, carimbos e métricas, Python lê outro relógio para o registro, e `flock` governa as fatias. `bin/codex_pergunta.sh:335,361–376,408–415,534–537,554–557,563,570–577,611–612,669–723,760–781`.
- **Semântica.** A flag é a da viva. Wrapper lê presença com `-e` e pulso pela geração; `--ler/--soltar` exigem `-f`; mero diretório pausa para o wrapper e não pode ser solto pela ferramenta. `SAIDA` é declaração humana, sem avaliação automática. `pausas.log` fica na raiz do script mesmo para flag de outra raiz, `SOLTOU` é escrito antes de um `rm` cujo erro é ignorado. Hooks usam portas distintas e não consomem flag/seq na cópia examinada. `bin/codex_pergunta.sh:90–98,397–398,424,440,504–507,675,690,717,746–748`, `bin/pausar.sh:14–16,33–50,53–72`, `.claude/settings.json:3–9,62–69`, `bin/disjuntor.py:1851–1862,1920–1925`, `bin/hook_stop_fila1.py:726–744,788–838`.
- **Selos.** `pausa_gate` morde saída enquanto gate preso, `gate_solta` morde aquisição efetiva, `pausa_slot/resp` mordem rc e motivo, `rc_sidecar` morde bytes, `pz:meta` morde existência e `pz:seq` morde geração/cancelamento. Mas `_ate_abriu` não exige abertura ao vencer prazo. O pulso pode coincidir com a leitura da flag e o teste da geração ficar inconclusivo. O selo legado de M6 exige mensagem específica, mas M5 pode primeiro devolver o mesmo `75 PAUSADO`. `proj_dono` altera campos do registro mantendo ID, de modo que a asserção de preservação por ID passa vazia, embora rc/marcador/mensagem matem o mutante. `bin/tests/test_codex_n_vagas.sh:439–444,591–639,666–679,695–707,740,770–775`, `bin/tests/test_codex_pergunta.sh:261–278,577`, `bin/codex_pergunta.sh:374–376,715–718,748`.

## 21/11 22:02 · sessao 3758c9c9e020 · - · - · origem=Ronald (tmux)

**BLOQUEIA.** O novo loop do gate transforma erro permanente de `flock` em espera sem saída. Se `slot.gate` for diretório, `exec 9>` falha e o shell prossegue. Com fd 9 inválido, `flock -w 1 9` retorna imediatamente e o loop retém a vaga até observar pausa. A abertura sem guarda é preexistente. O loop infinito é novo. `bin/codex_pergunta.sh:72,668–676`.

**Selos.** São 21 mutantes novos, um removido e `#V:publica` reaplicado. Reproduzi em memória a aplicação dos 55 mutantes e a validação sintática. As mortes efetivas estão [nao verificado]. `bin/tests/test_codex_n_vagas.sh:691–707,735–745,757–780`.

- `m3_decl`, `m3_minha`, `m3_outra`, `m3_pontas` testam admissão indevida via rc, captura e marcador. `bin/tests/test_codex_n_vagas.sh:189–193,323–343`.
- `pausa_seq`, `pausa_gate` testam saída com a trava ainda presa. `gate_solta` testa disponibilidade do gate. `pausa_slot`, `pausa_resp` testam `75 PAUSADO`. `rc_sidecar` testa preservação dos bytes do `.rc` alheio. `bin/tests/test_codex_n_vagas.sh:598–638`.
- `m6_meta`, `m6_confere`, `m6_inicia`, `adm:confere_ino`, `adm:confere_n` testam recusa antes do Codex. `pz:meta` testa publicação e retirada sob a meta. `pz:seq` testa a geração resultante. `bin/tests/test_codex_n_vagas.sh:607,641–679`.
- `hist_arquiva` testa preservação do registro anterior. `proj_fecha` testa `fechou/rc/motivo`. `proj_dono` testa rc 3 e diagnóstico, mas o `grep` preservaria o verde se os bytes alheios fossem modificados mantendo o ID. `fim_motivo` testa o campo 10, mas seleciona a última linha por obra, sem vínculo com a chamada corrente. `publica` testa os campos do registro e seu fecho. `bin/tests/test_codex_n_vagas.sh:440–458,546–577`.
- `m5_gate_preso` pode passar com flag persistente. Silencia `pausar.sh`, não exige que a flag tenha sumido e não verifica a geração. `_ate_abriu` também encerra no timeout ou no rc sem exigir o incremento. O motor aceita o primeiro `FALHA:` com o prefixo esperado, inclusive mensagem de fixture. O selo preexistente `test_codex_pergunta.sh` aceita qualquer ocorrência de `FALHA`. `bin/tests/test_codex_n_vagas.sh:589–592,613–623,769–780`; `bin/tests/test_codex_pergunta.sh:657–659`.

**Contagem literal do delta.** Zero cláusulas `except` novas, zero `|| true` novo e um novo `raise OSError` sob o `except Exception` preexistente. Há um `2>/dev/null` novo em produção e dois preservados em linhas alteradas. No selo há 21 adições e três remoções de `2>/dev/null`, mais sete chamadas que descartam ambos os canais. `bin/codex_pergunta.sh:305–313,540,574,749`; `bin/tests/test_codex_n_vagas.sh:441–458,551–573,593–607,619–645,669–675`.

**Outros fallbacks e escritas.** M3 traduz erro do Git em `None` e recusa, com stderr capturado. O teste de geração trata ausência como zero e em espera usa somente o texto de `_seq`. `pausa.seq` FIFO pode travar sob a meta. Revalidação lê somente o conteúdo do pipeline N e o token de `read`, e as três chamadas `flock -w 1` tratam qualquer erro como contenção. O `flock -u` não testa rc. A projeção retorna 1 internamente para registro anterior sem ID, e o wrapper converte para 3. `bin/codex_pergunta.sh:279–315,334–355,397–398,507,555–556,674–692,715–718,749–752`.

`pausar.sh` genérico ainda aceita caminho de vaga, fora da porta. A mesma via alcança os novos `pausa.seq` e histórico. A confirmação de existência de `--soltar` ocorre antes da meta, permitindo que dois soltas da mesma flag incrementem duas vezes e cancelem uma chamada admitida no intervalo. A publicação prepara um `.novo` compartilhado antes da meta e o `mv` e a trilha não são validados. `pausar.sh` aceita `0008` no regex, mas a soma Bash aborta por octal. `bin/pausar.sh:23–30,39–50,53–72`; `bin/codex_pergunta.sh:506–507`.

## 21/11 22:01 · sessao e1e882bdbf25 · - · - · origem=Ronald (tmux)

**BLOQUEIA.** A projeção persistida deixa o lançador legado herdar o conjunto de arquivos de uma chamada encerrada. Uma projeção v1 sobrevivente impede indefinidamente o reuso da vaga. O gate novo pode girar indefinidamente com fd inválido, e a conferência de insumos pode autorizar um prompt diferente do que foi registrado. `bin/codex_admissao.py:139–151`, `bin/codex_pergunta.sh:298–320,334–371,522–555,634,661,668–678`.

- **NOVO — estado alheio.** Depois de um fechamento normal, um legado segura só a trava gêmea de k=1. A nova chamada conta k=1 ocupada, mas compara o JSON persistido da ocupante anterior. Se a WT antiga existir e for disjunta, o candidato passa embora possa tocar o mesmo arquivo que o legado. A base truncava o JSON no fecho. O selo limpa os JSONs antes de testar o ocupante legado. `bin/codex_admissao.py:139–146`, `bin/codex_pergunta.sh:298–320,408–415,519–543`, `bin/tests/test_codex_n_vagas.sh:212–215,346–352`.
- **NOVO — reuso bloqueado.** Um JSON v1 não vazio, deixado por encerramento abrupto na base, falha por ausência de `id_chamada`. A vaga continua livre, mas é selecionada de novo no próximo lançamento e retorna novamente `3 FALHA_TRILHA` antes do `abriu`. Um histórico prévio de conteúdo divergente produz o mesmo efeito. O caso de JSON sem ID está explícito no selo. `bin/codex_admissao.py:146–151`, `bin/codex_pergunta.sh:342–356,554–558`, `bin/tests/test_codex_n_vagas.sh:574–577`.
- **NOVO — esperas sem saída.** A falha de abrir `slot.gate`, por exemplo se for diretório, deixa fd 9 inválido e o novo `until flock` interpreta o erro imediato como contenção. Um FIFO em `pausa.seq` prende `cat`, inclusive com meta em mãos. Um FIFO no JSON anterior de uma vaga livre prende a leitura do novo publicador, com meta e lease em mãos. **PREEXISTENTE:** segurar de fato o gate indefinidamente, sem pausa, não tinha prazo total. `bin/codex_pergunta.sh:72,334,397–398,501–507,555,668–678,748`, `bin/pausar.sh:42–47`.
- **NOVO — insumos M6.** O registro guarda hashes dos caminhos canônicos, mas o CAB lê os argumentos originais e a M6 reabre os caminhos gravados. Trocar um symlink de entrada entre M4 e CAB pode mudar o prompt sem mudar os arquivos conferidos. Trocar A→B antes de CAB e B→A antes de M6 também passa. Os briefs descobertos pelo coletor ficam fora dos hashes salvo anexação explícita. `bin/codex_pergunta.sh:221–256,363–371,482,489,555,634,661,760`.
- **PREEXISTENTE — admissão em comum.** Duas declarações como `app/folha/../ponto/a.py` e `app/ponto/a.py`, com HEADs sem delta e WT admitida limpa, apontam para o mesmo arquivo; a comparação não normaliza `..`. Também não há exclusão explícita pela identidade da WT. A mesma obra sem brief próprio pode citar um brief de outra pasta de `logs` em cada pergunta e apresentar listas disjuntas a partir da mesma WT limpa. `bin/disjuntor.py:1526,1612–1645,1723–1747`, `bin/codex_pergunta.sh:249–256,266–320,454–471,522–549`.
- **NOVO — demais decisões.** M3 soma os dois deltas e conserva as duas pontas do rename. HEAD destacado funciona com ref compartilhada. Sem merge-base na concorrência ou com lista declarada vazia, o portão retorna rc 2. Ausência inicial de main na viva já causava `2 PRE_CHECK`; primeira vaga continua sem M3. M6 relê N e confere o inode próprio; `k>N` ainda pode autorizar se a cardinalidade couber. Sob meta, a busca de vaga é não bloqueante, por isso a ordem `meta→vaga` e `vaga→meta` não forma ciclo nas rotas normais. `bin/codex_pergunta.sh:279–320,426–429,540–544,744–766`, `bin/codex_admissao.py:55–60,84–103,139–151`.
- **Checklist.** Zero `except` novo e zero `|| true` literal novo. Há um novo `2>/dev/null` em produção, em `_N6`. O selo introduz 19 usos literais em novos trechos, três substituindo supressões anteriores, e sete `>/dev/null 2>&1` adicionais. O `except Exception` antigo cobre as novas falhas do delta alheio. A ferramenta de pausa genérica continua podendo escrever em um nome de vaga por caminho explícito sem passar pela meta, que só é tomada para o basename `codex-construtor`. `bin/codex_pergunta.sh:297–313,749`, `bin/pausar.sh:23–30,39–50,53–69`, `bin/tests/test_codex_n_vagas.sh:441–674`.
- **Selos.** M3/M6 aferem rc e não chamada pelo fake além de texto. O motor aceita qualquer falha com o prefixo esperado, então a prova fornecida não identifica qual asserção matou cada mutante. `#V:publica` reaplicado para antes de conferir o JSON, pois M6 rejeita registro ausente e `_prende` acusa que não entrou no Codex. `proj_dono` muda o resultado, mas a suposta preservação verifica só o ID que permanece mesmo quando o mutante altera os demais campos. `bin/tests/test_codex_n_vagas.sh:169–193,438–458,646–663,691–705,735–740,757–775`, `bin/codex_pergunta.sh:334–340,760–764`.

## 21/11 21:40 · sessao b7bed28275bf · E4C-F5-CONSTROI · construtor · origem=Code

**INCOMPLETO**

T1 a T4 estão no diff da cópia. Os commits por pedaço ficaram pendentes por causa da regra «nunca git commit» do brief. O diff contém apenas os três arquivos de teste abaixo, com **167 inserções e 55 remoções**. [nao verificado]

| Pedaço | Mudança | Diff |
|---|---|---:|
| T1 | Os dois A2 exigem que o `SELECT` do PK alvo execute depois do `SELECT FOR UPDATE` do colaborador, no mesmo `atomic`. [Guarda do reconciliador](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:256) | |
| T2 | Os três cenários de C20 `True` exigem zero entradas em `carimbar`. O espião registra a entrada antes de chamar a porta. [RED do C20](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e2_pergunta_so_ao_c6.py:194) | +21/−2 |
| T3 | A candidata chega com `validada_em` nula e `VALIDACAO` vigente só na sequência. O RED exige que o C6 aceite o carimbo mesmo com a resposta discordante. [RED do prefetch velho](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:341) | T1 + T3 +132/−33 |
| T4 | As duas notas registram `N + K + D`, com um K7 por sobrevivente com empresa e dia de turno, compartilhado por C20 e C6, mais leitura de toque se houver tranca. Os tetos seguem em 238 e 302, com folga zero registrada. [Nota de 238](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/ponto/tests/test_selo_chokepoint_escrita.py:206) · [Nota de 302](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/ponto/tests/test_selo_chokepoint_escrita.py:242) | +14/−20 |

A checagem estática confirmou a compilação dos três mutantes, a ativação por tag e a restauração da função original. A AST do selo é idêntica à da base. Verde, vermelhos e selo na pista [nao verificado]. [Ativação dos mutantes](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:44) · [Nota do selo](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/ponto/tests/test_selo_chokepoint_escrita.py:206) [nao verificado]

**Comandos para o Code na pista**. A suite repassa `-k` e `--tag` ao Django e define `PYTHONDONTWRITEBYTECODE=1`. [bin/suite.sh:198](/home/ronald/wt-codex-e4c-f5-constroi-213412/bin/suite.sh:198) · [bin/suite.sh:319](/home/ronald/wt-codex-e4c-f5-constroi-213412/bin/suite.sh:319) [nao verificado]

```bash
COPIA=/home/ronald/wt-codex-e4c-f5-constroi-213412

# T1 verde e vermelho esperado nos dois A2 [nao verificado]
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_A2_evento_celula_absolveu
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_A2_evento_celula_absolveu --tag f5_e2_sql_antes_da_trava

# T2 verde e vermelho esperado nos três vetos C20 [nao verificado]
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_f5_e2_tranca_
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_f5_e2_tranca_ --tag f5_e2_carimbo_sob_veto_c20

# T3 verde e vermelho esperado na asserção do retorno da porta [nao verificado]
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_RED_prefetch_velho_esconde_validacao_so_na_sequencia
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_RED_prefetch_velho_esconde_validacao_so_na_sequencia --tag f5_e2_prefetch_velho

# T4 selo
bin/suite.sh --dir "$COPIA" --only ponto --espera 7200 -- -k test_selo_chokepoint_escrita
```

**Autoconferência** — except/fallback novos no diff: **0**. `git diff | grep -n except` na cópia retornou zero ocorrências.

## 21/11 21:32 · sessao 58b27750b928 · E4C-F5-CONSTROI · verificador · origem=Code

BLOQUEIA

**NOVO contra `b0935c1f1`.** O A2 ainda pode aceitar um SELECT executado antes da trava, porque observa o retorno de `.first()` e o savepoint, sem observar a execução do SQL. A nota do chokepoint também mantém uma atribuição incorreta de **dois K7 por sobrevivente**. O código faz **um K7 direto por sobrevivente com empresa e dia de turno**, compartilhado pelo veto C20 e pelo C6 da porta. A correção de `237/301` para `238/302` é compatível com a medição relatada na triagem. A contagem bruta permanece [nao verificado]. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:213–258` · `app/chamados/reconciliador.py:644–657` · `app/chamados/juizes.py:991–997,1027–1085,1144–1157` · `app/ponto/tests/test_selo_chokepoint_escrita.py:206–221,245–260`

## 1. Achados da volta 2 e triagem

| Achado | Estado em `3d8aa2c89` |
|---|---|
| Folga de uma instrução e atribuição do custo | **NOVO. Folga descartada na triagem, de forma condicional à medição corrigida. Atribuição aberta.** O código confirma os tetos `238/302` e a comparação `<=`. Se os totais corrigidos são `238/302`, a folga é zero. Os dumps e o vermelho em `237/301` são relatados na triagem e ficam [nao verificado]. A correção numérica manteve a afirmação de dois K7 diretos por sobrevivente. `app/ponto/tests/test_selo_chokepoint_escrita.py:206–221,245–260,349–360` · `app/chamados/juizes.py:991–997,1067–1075,1144–1157` |
| A2 podia passar sem releitura e o espião perdia chamadas que lançavam | **NOVO, curado em parte.** Uma releitura observada pelo PK é obrigatória. No positivo, uma única entrada bem-sucedida deve receber a mesma instância retornada. No negativo, a releitura deve retornar `None` e a porta deve receber zero chamadas. Agora o espião anota a entrada antes de invocar a porta real, portanto uma exceção dela deixa o registro. A ordem do SQL ainda escapa. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:83–96,217–273` |
| C20 entre emissão e trava | **NOVO, curado nessa janela.** Os REDs exigem emissão, mudança de tranca ou corte, um C20 `True` com o colaborador registrado como travado, trio vazio e contador zero. Cobrem execução direta, request e consulta externa. A entrada na porta sob veto ainda escapa. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:173–248` |
| Selo sem variar candidatas e competências, e `commit <a preencher>` | **Custo linear NOVO, aberto. Fixture PREEXISTENTE. Identificação NOVA, curada.** A nota identifica `701096390`, `78c9af575` e `4fec34a7c`. A fixture cria 1 ou 3 chamados no mesmo dia e confere quantos fecharam. Ela não conta candidatas emitidas, sobreviventes nem competências. `app/ponto/tests/test_selo_chokepoint_escrita.py:206–220,245–259,468–505` |
| Mutante sem `tranca_em_lote(sistema_pk=...)` | **NOVO, descarte procedente para o resultado funcional das fixtures.** A consulta privada continua fria para empresa, K7 e toque. Sem a semente, ela pode aproveitar o PK do lote externo ou buscar `get_user_sistema()` quando houver toque para conferir. Com o mesmo usuário de sistema, o predicado é igual. A possível leitura adicional faz a equivalência de custo cair. Execução do mutante [nao verificado]. `app/chamados/reconciliador.py:638–657` · `app/chamados/juizes.py:1101–1117,1144–1171,1217–1234` · `app/core/utils.py:6–18` |
| Mutante com prefetch velho de `decisoes` | **NOVO, descartado para fila pela triagem. O motivo é insuficiente.** O vigia conta divergências, mas C6 e C7 também consomem a composta. No RED da resposta, a projeção recém-gravada recompõe uma resposta ausente no prefetch velho. No A2 negativo, a coluna preenchida exclui a candidata antes de se lerem os atos. Esses testes podem passar com prefetch velho. Com uma `VALIDACAO` vigente só na sequência, a coluna nula deixa a candidata entrar e o C6 fresco a dá por morta no primeiro degrau. O C6 com o cache velho pode cair no degrau da resposta e recusá-la. Incidência do par no acervo e execução do mutante [nao verificado]. `app/chamados/models.py:4304–4322,4361–4399,4436–4452` · `app/chamados/juizes.py:1406–1440` · `app/chamados/services/vigia_ato_x_coluna.py:8–24` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:135–161` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:247–270` |
| C6 ainda depende da pré-seleção | **PREEXISTENTE, cura parcial, aberto no comportamento.** O motivo histórico da triagem se sustenta pelo anexo. A lacuna ainda incide no trecho tocado pelo 3.3. Uma pergunta muda com `data_evento=NULL` passa pela emissão com ata definitiva. Se, antes da trava, a ata ganha um marco pendente e a lâmpada perguntada fica sem luz, mantendo `concorde`, o SQL ainda aceita a pergunta. Na foto relida, C6 sem carimbo daria viva. A porta aplica o carimbo antes de consultar C6, que então aceita `celula_absolveu` pelo degrau de lastro com dia de turno `None`. O RED da lâmpada começa com a ata já provisória, deixando a pré-seleção barrar o caso. A coluna admite nulo e a porta de nascimento datado o recusa. Ocorrência em produção [nao verificado]. `app/chamados/reconciliador.py:647–657,689–703,724–735` · `app/chamados/models.py:2631–2636,2993–3000,3361–3383,4088–4094` · `app/chamados/juizes.py:337–341,684–699,1455–1496` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:27–33,48–86` |
| K7 ou relavra depois da leitura, e fecho posterior | **PREEXISTENTE, descarte de escopo sustentado para a janela residual.** A trava do colaborador serializa resposta e validação pelas portas citadas. O corte, `PeriodoFechado` e a lavra têm caminhos que podem escrever após a respectiva leitura. No fecho, a consulta privada já acabou, o E2 refresca só `fechada_em` e `fechar()` julga antes de adquirir sua trava. O contexto novo protege a foto usada no carimbo e termina antes do fecho. Concorrência observada em produção [nao verificado]. `app/chamados/services/disputa_emissao.py:1478–1485` · `app/chamados/services/validacao.py:746–760` · `app/colaboradores/services/estrutura.py:18–22` · `app/ponto/services/fechamento.py:1400–1448` · `app/ponto/portas/celula.py:957–965,1181–1202` · `app/chamados/reconciliador.py:654–678` · `app/chamados/models.py:2071–2097` |

## 2. Mutantes que escapam

- **A2, NOVO.** Dentro do mesmo `atomic`, montar o queryset relido com `.order_by('pk')` e avaliá-lo com `list(qs)` **antes** de `trava_do_colaborador`. Depois da trava, chamar `qs.first()` sobre o queryset já avaliado e entregar o resultado à porta. Por inspeção, `.first()` devolveria a instância do cache, ou `None` no negativo. O observador registraria a chamada depois da trava, e o `assertIs` do positivo continuaria satisfeito. A validação interposta já ocorreu antes do `yield`, portanto a fixture continuaria vendo a validação mesmo com o SELECT adiantado. Zero chamadas da consulta observada falha nos dois A2. Zero chamadas da porta é o resultado esperado apenas no negativo. Execução do mutante [nao verificado]. `app/chamados/reconciliador.py:644–657` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:174–193,213–270`
- **C20, NOVO.** No ramo em que C20 devolve `True`, inserir uma chamada a `carimbar` com `resposta` inválida antes do `continue`. A porta lança antes de chamar C6, o E2 engole a exceção e o RED ainda observa `[True]`, trio vazio e zero carimbos. O espião corrigido do A2 pegaria essa entrada se estivesse instalado nesse cenário. Execução do mutante [nao verificado]. `app/chamados/reconciliador.py:654–664` · `app/chamados/models.py:3353–3376` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:197–213` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:83–96`
- **Mordida preservada, NOVA.** Por inspeção, retirar os imports usados no corpo derruba o controle positivo. Retirar a releitura ou pular a candidata antes dela derruba a contagem A2. Carimbar a instância emitida derruba a identidade do positivo. Retirar o contexto privado ou recolocar o colaborador antigo expõe, respectivamente, os REDs de request e de corte. O RED da resposta também exige uma recusa real de C6. Resultados de execução [nao verificado]. `app/chamados/reconciliador.py:627–630,644–664` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:247–266` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:130–171,197–248`

## 3. Quanto custa e o que a nota explica

**NOVO.** Em fluxo sem falha, para `N` candidatas emitidas, `K` sobreviventes ao SQL e `D` sobreviventes com empresa e dia de turno, o trecho acrescenta **`N + K + D` SELECTs diretos**. São `N` releituras da pergunta com JOIN, `K` prefetches da sequência e `D` consultas de K7. Uma tranca encontrada pode acrescentar leituras de toque. O memo nasce a cada iteração, então duas sobreviventes na mesma competência fazem dois K7 no total. O custo cresce com as candidatas mesmo com uma única competência. C6 reutiliza o resultado local. C7 e o fecho podem consultar em outro escopo. `app/chamados/reconciliador.py:635–678` · `app/chamados/juizes.py:991–997,1027–1085,1132–1157,1217–1332,1414` · `app/ponto/services/fechamento.py:1464–1470` · `app/chamados/models.py:2017–2035,2071–2081`

**NOVO.** As duas notas explicam corretamente `N + K` e registram a correção aritmética, com `234→236→238` e `298→300→302` anotados na cópia. O `+2` total relatado por cenário não prova `+2` por sobrevivente. A origem do segundo K7 de cada cenário, o `N/K/D` de cada execução e a composição dos SQLs capturados ficam [nao verificado]. O selo tem mordida para excesso no total da fixture. A atribuição por sítio exigida pelo corte continua incorreta. `app/ponto/tests/test_selo_chokepoint_escrita.py:206–221,245–260,349–360,468–505` · `app/docs/RELATO.md:1292` · `app/chamados/reconciliador.py:647–657`

## 4. Escritores, leitores e semântica

| Campo ou valor | Quem escreve, quem lê e o que significa |
|---|---|
| PK, `disputa_id`, dono e candidata emitida | **PREEXISTENTE.** A abertura grava o dono e o chamado da disputa. O nascimento vincula a pergunta, e a chave de identidade da pergunta deriva o colaborador da disputa. O gerador emite pelo seletor e por C6/C20. O vigia conta emissões sem evento. **NOVO.** O E2 reaplica o seletor para o mesmo PK e a mesma disputa e exige o dono relido no registro da trava. Emitida, sobrevivente e gravada são etapas distintas. `app/chamados/services/disputa_emissao.py:601–606` · `app/chamados/models.py:3224–3234,3766–3776` · `app/chamados/reconciliador.py:635–662,682–735` · `app/chamados/management/commands/reconciliar_chamados.py:89–93,109` |
| `validada_em`, `validada_por`, `decisoes` | **PREEXISTENTE.** Validação grava coluna, autor e ato. Devolução, resposta humana e reabertura podem tirar a validação. `DecisaoPergunta.registrar` grava a sequência e invalida o prefetch e o memo de C6 na instância recebida. C6/C7 leem a composta, o vigia compara ato e coluna, e a porta recusa `(False, True)` quando a coluna está preenchida. **NOVO.** A releitura repete o filtro físico. `(False, True)` fica fora e `(True, False)` pode entrar e receber carimbo. Coluna física e validação vigente podem divergir. `app/chamados/services/validacao.py:161–175,277–296` · `app/chamados/services/disputa_emissao.py:1499–1537,1632–1684` · `app/chamados/models.py:2022–2035,3384–3393,4382–4399,4436–4452` · `app/chamados/reconciliador.py:647–649,689–703` · `app/chamados/juizes.py:1406–1411` |
| `respondida_em`, `resposta_colab`, `motivo`, `tipo_resposta` | **PREEXISTENTE.** Nascimento recebe motivo e tipo. As portas humana, automática, pela trilha e de reabertura operam a resposta e seus atos. C6/C7 entregam ao C21 instante vigente da sequência e texto da projeção. A face SQL usa as colunas físicas. Motivo e tipo guiam a interpretação da resposta e do marco. **NOVO.** O C6 do carimbo recebe esses insumos da instância relida. `app/chamados/models.py:2631–2659,3224–3234,4382–4390,2022–2027` · `app/chamados/services/disputa_emissao.py:1449–1473,1499–1537,1632–1737` · `app/chamados/juizes.py:927–953,1347–1380,1426–1440,1551–1584` · `app/chamados/reconciliador.py:647–657` |
| `celula_id`, `data_evento`, `conversa`, `veredito`, `veredito_via`, DNA e ata | **PREEXISTENTE.** Nascimento e reuso vinculam célula e dia. `gravar_dia_do_fato` atribui o dia. A porta da célula grava DNA e lavra com insumos do cartório ou do admin. O seletor lê o veredito, C6 lê via e lâmpada do marco, e C20 lê o dia via `conversa`. O outbox lê FK e dia. `data_evento` é dia do turno na pergunta datada e dia civil na conversa, com discriminador explícito. `concorde` no seletor e prova do marco na ata são condições distintas. **NOVO.** A célula para o carimbo vem da releitura. `app/chamados/models.py:2993–3012,3178–3234,3472–3496` · `app/chamados/services/fio.py:843–854` · `app/ponto/portas/celula.py:289–311,1050–1098` · `app/ponto/services/cartorio.py:925–935,1898–1922` · `app/ponto/services/veredito_celula.py:346–358` · `app/chamados/juizes.py:337–341,935–976,1423–1474` |
| Empresa, corte, competência, tranca, toque e `_user.pk` | **PREEXISTENTE.** Cadastro e transferência gravam a empresa. Cadastro de empresa grava o corte. A janela deriva mês e ano da competência. `trancar_periodo` e `reabrir_periodo` gravam a tranca. K7 lê `reaberto_em IS NULL`, e C20 usa `fechado_em` como limite do toque. Cobrar, resolver e desarquivar escrevem as assinaturas que C20 lê na trilha. `get_user_sistema()` fornece a identidade usada para autoria automática e para excluir o sistema do toque humano. **NOVO.** O JOIN relê empresa e corte. A consulta privada guarda o resultado por instância e compartilha o mesmo predicado entre C20 e C6. `app/colaboradores/views.py:538–557,1333–1365,1413–1418` · `app/ponto/janelas.py:36–63` · `app/ponto/services/fechamento.py:1400–1441,1464–1470` · `app/chamados/services/cobrar_dia.py:31–41` · `app/ponto/services/veredito_celula.py:352–358` · `app/chamados/views_arquivar.py:74–86` · `app/core/observ.py:133–138` · `app/core/utils.py:6–18` · `app/chamados/juizes.py:991–997,1067–1085,1144–1157,1217–1332` |
| `chamado_destino`, `status_local`, trio e recusa | **PREEXISTENTE.** Nascimento/reuso e costura escrevem o destino, e a porta de transição escreve o estado do chamado. C6 lê o destino e usa `status_local` na chave do memo. `carimbar` escreve `materializada_em`, `materializada_por` e `via_resolucao`. As portas de identidade e descarimbo também operam no trio. `recusa_do_carimbo` é diagnóstico em memória. O trio registra processamento, inclusive absolvição sem plantar batida, apesar do texto de ajuda da coluna. **NOVO.** O E2 entrega à porta o destino relido. `app/chamados/models.py:213–240,3197–3232,3317–3405,3425–3460,3537–3539,3691–3709,4088–4094` · `app/chamados/services/fio.py:857–876` · `app/chamados/juizes.py:784–805,846–864` · `app/chamados/reconciliador.py:647–657` |
| `_gravou`, `n`, `_disputas`, fecho e evento | **PREEXISTENTE.** A porta devolve o booleano de escrita. O E2 conta os sucessos, C7 lê a disputa, `fechar()` grava fecho, autor, resolução e motivo, e a reabertura os limpa. O signal lê o fecho e acorda o pai. A porta do outbox cria o evento e o consumidor grava `processado_em`. **NOVO.** O E2 enfileira a disputa da instância relida. `out['perguntas']` conta carimbos e `processado_em` marca processamento do evento, mesmo com zero carimbos. `app/chamados/reconciliador.py:657–679,1428–1433,1453–1474` · `app/chamados/models.py:2029–2035,2118–2127,2335–2366` · `app/chamados/signals.py:19–46,252–259` · `app/ponto/portas/celula.py:945–954` |
| Valores do espião e do selo | **NOVO.** O wrapper escreve `ENTROU_SEM_RETORNO` em `chamadas` e substitui a entrada pelo booleano, recusa e foto final se a porta voltar. As asserções leem essa lista e as fotos. A foto também lê `recusa_motivo`, texto persistido pelas portas de recusa, e o trio da lavra da pergunta, escrito por `veredito_lavrado.lavrar`. O selo lê `TETOS` em `_cobrar`, comparando com o total capturado. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:39–44,83–96,247–270` · `app/chamados/services/materializacao.py:1397–1400,1723–1737` · `app/chamados/veredito_lavrado.py:254–281` · `app/ponto/tests/test_selo_chokepoint_escrita.py:349–360,488–505` |

## 5. Checklist do pouso [nao verificado]

- **Except/fallback, NOVO:** no patch final fornecido, produção **0 `except`, 0 fallbacks** e testes **0 `except`, 0 fallbacks**. O delta 3.3 descrito no anexo também tem **0 novos** nas duas classes. Os dois `except Exception` do E2 são **PREEXISTENTES**. É **NOVO** o primeiro engolir também falhas na releitura ou no preparo de C20, permitindo ao consumidor marcar o evento como processado. `app/chamados/reconciliador.py:644–678,1428–1433,1453–1469` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:83–96` · `app/ponto/tests/test_selo_chokepoint_escrita.py:218–221,257–260`
- **Segundo juiz, derivação paralela e segundo escritor fora da porta, 0 NOVOS.** O E2 reutiliza o seletor existente e as autoridades C20, C6, K7 e C7. O trio e o fecho passam por `carimbar()` e `fechar()`. O problema do C6 é o estado em que ele é consultado. `app/core/juizes.py:290–305,343–344` · `app/chamados/reconciliador.py:647–703` · `app/chamados/models.py:3361–3405,2118–2126`
- **Relógio solto, 0 NOVOS nos REDs.** Ambos congelam o relógio e mantêm o congelamento nas intercalações. O selo SQL usa `timezone.localdate()` solto, **PREEXISTENTE**. Em produção, `carimbar` obtém o instante sob a trava. O C6 ainda pode usar `date.today()` e acionar `classificar_falta` sem `agora`, **PREEXISTENTE**. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:32–48` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:36,142–151,189–195` · `app/ponto/tests/test_selo_chokepoint_escrita.py:334–347` · `app/chamados/models.py:3361–3376` · `app/chamados/juizes.py:589–598,1475–1482` · `app/escala/servico_jornada.py:385–389`
- **Ordem de trava e mordida.** Na implementação, colaborador primeiro e depois leituras simples, com **zero inversões NOVAS**. O C7 julga antes da própria trava, **PREEXISTENTE**. São **NOVAS** as lacunas de prova do SELECT no A2 e da entrada na porta sob veto C20. `app/chamados/reconciliador.py:644–676` · `app/chamados/models.py:2071–2097,2830–2848,2874–2878` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:217–258` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:197–213`
- **Campo com dois significados.** Coluna de validação versus validação vigente, dia civil versus dia de turno e processamento versus lastro são distinções **PREEXISTENTES**. A repetição do filtro da coluna física sob a trava é **NOVA** e conserva a assimetria dos pares. `app/chamados/reconciliador.py:647–649,689–703` · `app/chamados/models.py:3004–3012,3699–3709,4382–4399` · `app/chamados/juizes.py:337–341,684–699`
- **Pré-requisito de pouso [nao verificado].** Os blobs dos dois arquivos conferem com o patch anexado. A identidade Git da cópia e a comparação direta com `b0935c1f1` ficam [nao verificado], por se tratar de uma exportação. Usei o anexo da volta 2 para a classificação cumulativa e a cópia para conferir o comportamento. O `Ran 31` é informado no enunciado. Execução dos testes, suite inteira, ordem do Opus, SQL bruto e ocorrência em produção [nao verificado]. `CLAUDE.md:344–351,369–381`

## 21/11 20:47 · sessao cbb6edf614fc · MEDIDA-FOLHA11-2111 · sonda · origem=Code

## (a) Retrato, prévia e reconstrução em memória

As referências `dado:N` numeram as linhas JSON do bloco fornecido, começando em `MEDIDA`. As chaves identificam os registros da sonda.

| PK | Retrato gravado | Prévia | Em memória | Datas do furo | Classificação | Fonte |
|---|---|---|---|---|---|---|
| 956 | `entra / null`, lavrado em `2026-11-21T09:45:30.477Z` | `fora / furo_espelho` | `fora / furo_espelho` | Gravado `null`. Em memória `2026-11-20`, com motivo `espelho_furo` e veredito `furo`. Lista própria da prévia [nao verificado]. | **DEFASAGEM**. O par gravado difere e a prévia coincide com a reconstrução. | `dado:2` `A/956` e `dado:3` `A_DIA/956/2026-11-20` |

## (b) Retenção diária, rotas e donos

Datas em 2026. `""` é a razão vazia da tarefa e `[]` é a lista vazia devolvida em `dono_r1`. A tabela mantém os dois campos de rotas que a sonda devolveu, `rotas_diarias` e `tela_retrato.rotas`.

| Chave `B/pk/dia` | Razão / fio / rota da tarefa | Retenção do dia | Rotas diárias | Rotas da tela do retrato | Marca “Sem porta” | Juiz / R1 | Fonte |
|---|---|---|---|---|---|---|---|
| 246 / 01/11 | `"" / nada / null`, verbo `cobrar` | `espelho_cobranca_indevida` | “Validar no fio”, `chamados:modal_fio`, `existe=true` | `[]` | `true` | `CADASTRO / []` | `dado:15` |
| 327 / 10/11 | `"" / nada / null` | `espelho_indefinida` | “Validar no fio”, `chamados:modal_fio`, `existe=true` | `[]` | `true` | `ESTRUTURA / []` | `dado:55` |
| 476 / 24/10 | `"" / sistema / null`, `fio_folha=nada`. A instância `int_volta` oferece `perguntar` no `emissor`, com `porta_com_chamado=false`. | `null` | `[]` | “Vínculo da escala”, `colaboradores:painel_vinculo`, `existe=true` | `false` | `CADASTRO / []` | `dado:69` |
| 476 / 25/10 | `"" / nada / null`. As instâncias `aguarda_resposta` oferecem `responder` com `porta=null` e `porta_com_chamado=false`. | `espelho_indefinida` | “Validar no fio”, `chamados:modal_fio`, `existe=true` | “Vínculo da escala”, `colaboradores:painel_vinculo`, `existe=true` | `true` | `CADASTRO / []` | `dado:70` |
| 476 / 06/11 | `"" / admin / porta_fila`, ação `validar`, `nao_sei=true` e `fio_folha=admin` | `null` | `[]` | “Vínculo da escala”, `colaboradores:painel_vinculo`, `existe=true`. O texto do retrato diz `SEM PORTA`. | `false` | `BATIDA / [BATIDA]` | `dado:82` |
| 97 / 01, 03, 05, 07, 09, 11, 13, 15, 17 e 19/11 | `"" / nada / null` em cada dia. Só em 03/11 consta o verbo `cobrar`. | `espelho_indefinida` em cada dia | “Validar no fio”, `chamados:modal_fio`, `existe=true` em cada dia | `[]` | `true` | `CADASTRO / []` | `dado:108–126`, linhas pares |

**Candidatos a BUG por retenção sem porta funcional.** São 246 em 01/11, 327 em 10/11, 476 em 25/10 e 97 em cada data retida da tabela. Em cada chave, `retencao_dia.motivo` prova a retenção, `fio_folha="nada"` e `tarefa.rota=null` provam a falta de porta publicada, e `tela_retrato.marca_sem_porta=true` confirma a sinalização. A rota genérica “Validar no fio” tem `existe=true`. Para 476 em 25/10, `tarefa.instancias[].acoes[].porta=null` também comprova a falta de porta para responder. Em 24/10 e 06/11, `retencao_dia=null` deixa os dois dias fora desse critério. `Dado:15,55,69,70,82,108–126`; [app/folha/fora_da_folha.py:86](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/folha/fora_da_folha.py:86).

**PK 97. A tela diz “Sem porta”.** Nos dias retidos, a marca é `true`, o dono exibido é “Sem ação disponível” e a lista de rotas do retrato é `[]`. `CADASTRO` vem de `dono_juiz`. A checagem separada `cadastro_97` registra `escala_a_definir=true` e confirma a existência do GET `colaboradores:painel_vinculo` e do POST `colaboradores:vincular_escala`. Envio do formulário e efeito sobre a retenção [nao verificado]. `Dado:108–126`, `B/97` nas datas retidas; [app/templates/folha/_retrato_o_que_fazer.html:46](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/templates/folha/_retrato_o_que_fazer.html:46) e [app/colaboradores/urls.py:12](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/colaboradores/urls.py:12).

## (c) PK 265 em 31/10

| Pergunta | Resultado | Fonte |
|---|---|---|
| Célula e vínculo do dia | Célula presente, `trabalha=true`, com geradora e lâmpadas. Vínculo vigente `12x36`, `isento_grade=false`. | `dado:128` `C/265/2026-10-31` |
| Grade e previsto pelo juiz | Grade `trabalho`, `sem_ancora=false`, **660 min**. `previsto_min_juiz=660`. Ata com **4** células e `n_missing=0`. | `dado:128` `C/265/2026-10-31`, `grade`, `previsto_min_juiz` e `celula.ata` |
| Vereditos | Célula e grade `cobrado`, via `cartorio`. D4 `irregular`, fonte `lavrado`. Retenção `espelho_cobrado`. No espelho visual, a linha existe, tem **4** marcos canônicos, `grade_pelo_builder=false`, veredito `trabalhou` e `dono_juiz=CADASTRO`. | `dado:128–129` `C` e `C_ESPELHO/265/2026-10-31` |
| Leitura da tarefa | `tarefa.nao_sei=true`, `fio_folha=nao_sei`, `tarefa.rota=null`. A instância `batida_fora_escala_grave` está `passivo_sem_linha`, com `acoes=[]`. O bloco D publica “SEM LEITURA (sistema)”. | `dado:128,248` `C` e `D/265/2026-10-31` |

**Candidato a bug estrutural na leitura da tarefa**, pela chave `265/2026-10-31`. O ramo `passivo_sem_linha` detecta projeção de resposta ou validação sem ato correspondente na sequência. A exposição do motivo fora dos marcos propaga `nao_sei=true` sem publicar ação, e a seleção do fio devolve `nao_sei` com rota nula. Qual ato faltou e por que a sequência ficou assim [nao verificado]. O espelho visual, por sua vez, tem linha e marcos previstos e usa um veredito visual próprio. `Dado:128–129,248`; [app/chamados/models.py:4238](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/models.py:4238), [app/chamados/services/tarefa_do_dia.py:154](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:154), [app/chamados/services/tarefa_do_dia.py:273](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:273), [app/chamados/services/tarefa_do_dia.py:350](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:350), [app/chamados/services/tarefa_do_dia.py:761](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:761) e [app/ponto/services/espelho.py:329](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/ponto/services/espelho.py:329).

## (d) Empresa 2, competência 11/2026

### Motivo × fio × guarda da porta

A contagem cobre **271 colab-dias com linha `D` completa**. O registro `D/349/2026-11-12` termina antes das guardas. A distribuição dos **1.131 colab-dias** do total por motivo, fio e guarda fica [nao verificado]. Cada célula abaixo conta as chaves `D/pk/data` com o `motivo` da linha e o filtro de campos da coluna. Os zeros valem para o trecho completo fornecido. `Dado:1,130–402`, `MEDIDA`, `D_PONTE/emp=2` e `D`.

| `D.motivo` | `colab` app oferece | `admin` calendário oferece | `admin` ausência libera | `admin` validar libera | `admin` guarda [nao verificado] | `nada` sem oferta | `nao_sei` oferta [nao verificado] | Total |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| `espelho_cobrado` | 101 | 0 | 1 | 0 | 1 | 0 | 1 | 104 |
| `espelho_furo` | 31 | 8 | 0 | 1 | 2 | 0 | 0 | 42 |
| `espelho_indefinida` | 48 | 0 | 0 | 0 | 0 | 11 | 0 | 59 |
| `espelho_nunca_bateu` | 25 | 34 | 0 | 0 | 0 | 0 | 0 | 59 |
| `espelho_discordante` | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 2 |
| `espelho_cobranca_indevida` | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 2 |
| `espelho_fato_em_ausencia` | 0 | 0 | 0 | 0 | 3 | 0 | 0 | 3 |
| **Total no trecho** | **206** | **43** | **1** | **1** | **6** | **13** | **1** | **271** |

- **App e calendário.** Os filtros são `fio=colab`, `rota_tarefa=porta_app`, `guardas_da_porta.resposta_oferecida_no_app=true`, e `fio=admin`, `acao=resolver_dia`, `guardas_da_porta.oferta_calendario=true`, respectivamente. `Dado:131–401`, bloco `D`, filtrado por esses campos e `motivo`.
- **Ausência e validação com leitura positiva.** `D/220/19-11` traz `decidir_ausencia`, com `oferta_aprovar`, `aprovar_raiz_sem_confirmacao` e `rejeitar_raiz_aberta` verdadeiros. `D/334/17-11` traz `validar`, com **4** entradas em `guardas_de_validar` e `libera_guardas_lidas_sem_confirmacao=true` em todas, contadas como **1** colab-dia. `Dado:202,356`.
- **Guardas administrativas sem resultado de leitura.** `validar` em `D/227/02-11`, `D/254/29-10` e `D/255/31-10`. `decidir` em `D/258/30-10`, `D/258/06-11` e `D/258/09-11`. Os campos são `guardas_de_validar` e `libera_acao_especifica`, respectivamente. `Dado:209,233,235,242–244`.
- **Sem oferta e sem leitura.** `oferta_publicada=false` nas chaves `D/97` das dez datas retidas, `D/327/10-11`, `D/246/01-11` e `D/296/10-11`. Em `D/265/31-10`, `fio=nao_sei` e `oferta_publicada=[nao verificado]`. Para todos os registros completos, o resultado da execução do ato é [nao verificado]. `Dado:131–401`, campos `guardas_da_porta` e `resultado_do_ato`.

### Ponte com o R1

| Medida | Sonda | Referência do anexo | Chave em `D_PONTE/emp=2` |
|---|---:|---:|---|
| Colabs pendentes | 240 | 240 | `pendentes`, `pendentes_anexo` |
| União de colabs do R1 | 128 | 128 | `ponte.uniao_r1`, `uniao_r1_anexo` |
| Colabs em comum | 94 | 94 | `ponte.em_comum_colabs`, `em_comum_anexo` |
| Só pendência | 146 | 146 | `ponte.so_pendencia_colabs`, confere com `240 − 94` |
| Só R1 | 34 | 34 | `ponte.so_uniao_r1_colabs`, confere com `128 − 94` |
| Colabs em comum por dono do R1 | `ESTRUTURA` 11, `CADASTRO` 26, `BATIDA` 73 | [nao verificado] | `ponte.por_dono_colabs_em_comum` |
| Colab-dias em comum | 35 | [nao verificado] | `ponte.em_comum_colab_dias` |
| Colab-dias de retenção | 1.131 | [nao verificado] | `colab_dias` |
| Pendentes sem dia | 0 | [nao verificado] | `pendentes_sem_dia` |
| Universo de colabs do R1 | 460 | [nao verificado] | `ponte.universo_colabs_r1` |

Os cinco totais comparáveis coincidem em `D_PONTE`, `dado:130`. Os totais por dono somam **110** ocorrências para **94** pks em comum, com sobreposição entre donos. A interseção exata entre donos [nao verificado]. A interseção por pk e dia é **35**, uma unidade diferente da interseção por pk. `Dado:130`, `ponte`.

**Universo e instante.** O trecho diário fornecido cobre **62** pks e **5** coincidências no mesmo dia, pelas chaves `D/174/01-11`, `D/218/14-11`, `D/288/22-10`, `D/296/02-11` e `D/320/20-11`. A ponte agrega a competência inteira. O instante da sonda é `2026-11-21T23:31:09.766Z`. O instante do R1 e a igualdade dos conjuntos de pks entre as medições ficam [nao verificado]. Entre os cinco números comparáveis, a sonda registra diferença numérica zero. `Dado:1,130,131–401`, em especial `143,199,269,274,342`.

O trecho fornecido não registra `ERRO_TIPO`. O restante da saída após o corte está [nao verificado]. `Dado:1–402`.

## 21/11 20:29 · sessao 47380138b4db · MEDIDA-FOLHA11-2111 · leitura · origem=Code

## A. Fontes e recortes

| Item | Fonte real | Unidade, universo e exclusões |
|---|---|---|
| **(a) 956** | A prévia chama `classificar_export`. O retrato lê `linhas_do_retrato` via `linha_do_colab`. Para comparar os dois critérios no instante da leitura, `montar_linhas` percorre em memória o mesmo caminho do lavrador até antes da persistência. `_dias_que_retem` fornece as datas e os motivos diários. `app/folha/views.py:350–351` · `app/folha/retrato_folha.py:98–111,930–953,1212–1243` · `app/folha/management/commands/lavrar_retrato_folha.py:65–79` · `app/folha/fora_da_folha.py:115–122` | **Colab-competência** para presença e `status/motivo`, **colab-dia** para o furo. Prévia e reconstrução partem dos fechamentos da empresa e competência, qualquer que seja o status. A foto parte das linhas gravadas e tem carimbo próprio. O detalhe diário exclui isentos, desligados e dias dispensados pelo retentor. `app/folha/export.py:765–788,1205–1213,1238–1296` · `app/folha/models.py:350–360` |
| **(b) 246, 327, 476, 97** | `tarefa_do_dia` e `porta_da_tarefa` publicam razão, ações, fio e rota. `o_que_segura` dá o motivo de retenção e traduz a porta diária. O drawer usa `linha_do_colab` e `portas_da_linha`. `InsumosDoDono.dono` chama `dono_da_divergencia` com os insumos reais. `app/chamados/services/tarefa_do_dia.py:804–835` · `app/folha/fora_da_folha.py:257–305` · `app/colaboradores/views.py:994–1006` · `app/ponto/management/commands/e6_oraculo.py:182–195` | **Colab-dia**, em **todos os dias inclusivos da competência 11/2026** para cada PK, pelo corte de `Empresa.dia_inicio_competencia` via `janela_atual`. Os dias da CSV estão [nao verificado]. A sonda mostra o parecer do juiz em cada dia. O R1 só registra dono para divergências acima de 10 minutos que passaram pelos seus filtros. `app/ponto/janelas.py:24–63` · `app/ponto/management/commands/e6_oraculo.py:331–378` |
| **(c) 265 em 31/10** | `celulas_do_periodo`, `vinculo_do_dia_carregado`, a `grade_da_celula` consumida pela tarefa, `minutos_previstos_do_dia`, `tarefa_do_dia`, `status_do_dia_lavrado` e `espelho_do_colab`. O resultado lavrado do supra-juiz fica em `CelulaDia.veredito` e `protestos`. `app/escala/alimentacao.py:54–76,269–317` · `app/chamados/services/tarefa_do_dia.py:635–643,825–835` · `app/escala/utils.py:2241–2282` · `app/ponto/status_dia.py:201–218` · `app/ponto/services/espelho.py:1210–1245` · `app/escala/models.py:1748–1778` | **Um colab-dia**, com janela pelo juiz `janela_atual`. A tarefa distingue fora do vínculo, isenção, tranca, célula ausente, marco ainda a vencer e ata sem lâmpadas. O espelho tem guardas próprias de escala, recorte e montagem da grade. Qual ramo explica o caso 265 está [nao verificado]. `app/ponto/janelas.py:60–63` · `app/chamados/services/tarefa_do_dia.py:214–246` · `app/ponto/services/espelho.py:489–501,523–585,687–706` |
| **(d) emp2** | `dias_que_seguram` seleciona o motivo principal `furo_espelho` e devolve seus dias. A porta vem da tarefa e a tela de `portas_da_linha`. Para liberação, a sonda consulta a oferta de Resolver dia, as guardas de validação do fio, o gate de aprovação da ausência e a lista pendente do app, conforme a ação. A ponte lê `Command.handle` via `ler_r1`. `app/folha/fora_da_folha.py:125–174,279–295` · `app/folha/retrato_folha.py:1024–1076` · `app/colaboradores/services/calendario.py:97–104,125–167` · `app/chamados/services/validacao.py:681–706,740` · `app/ponto/services/ausencia.py:762–780` · `app/chamados/services/pergunta_no_app.py:68–90` · `app/ponto/management/commands/e6_oraculo.py:284–404` | **Colab-dia** para pendência e coincidência diária, **colaboradores distintos** para a ponte com o R1. Outros motivos principais do export ficam fora da lista de pendentes. O R1 parte de fechamentos de empresas ativas e exclui dia sem linha, isento, com ausência, sem valor no espelho, zero nos dois lados ou diferença explicada pela pausa; diferenças de até 10 minutos concordam. `app/folha/fora_da_folha.py:146–174` · `app/folha/export.py:1205–1213,1238–1296` · `app/ponto/management/commands/e6_oraculo.py:303–307,331–378` |

O anexo registra **uma** troca de `entra` para `fora/furo_espelho` na `emp2`, com concordância entre prévia e reconstrução em memória no par `status/motivo`. A atribuição ao **956**, a data do furo e o evento que mudou o resultado estão [nao verificado]. `tabelas.txt:154,158` · `app/folha/retrato_folha.py:1212–1243`

Na `emp2`, o anexo registra **240 pendentes** na prévia e **128** colaboradores na união do R1. A ponte por pessoa dá **94 em comum**, **146 só na pendência** e **34 só no R1**. A coincidência por dia está [nao verificado]. `tabelas.txt:22,39`

Para o **97**, o juiz atribui `CADASTRO` quando o insumo aponta ausência de vínculo no dia. A tela de vínculo mostra **A definir** quando falta escala ativa e registra o GET `colaboradores:painel_vinculo` e o POST `colaboradores:vincular_escala`. O rótulo diário **Sem porta** vem da data gravada no retrato e pode coexistir com um link geral da linha. A sonda expõe os dois testemunhos para verificar o caso. As ocorrências individuais e os bugs em produção estão [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:98–110` · `app/colaboradores/views.py:262–268` · `app/colaboradores/urls.py:12–13` · `app/templates/colaboradores/partials/painel_vinculo.html:42–48,128–137` · `app/templates/folha/_retrato_o_que_fazer.html:42–46` · `app/folha/retrato_folha.py:1066–1075`

## B. `sonda.py`

O script sai no chat. A execução em produção, a conferência Git do hash e as permissões de um admin concreto estão [nao verificado]. O envelope usa a rede dedicada ao Postgres e, na própria inicialização, reescreve a credencial, reafirma privilégios e grava trilha. Nesta volta de leitura, ficou sem execução. `bin/sonda_leitura.sh:121–143,153–193,216–240,245–250`

```python
import json
import logging
import socket
import sys
from collections import defaultdict
from contextlib import ExitStack, redirect_stderr, redirect_stdout
from datetime import date, timedelta
from io import StringIO
from unittest.mock import patch
from urllib.parse import urlsplit

ANO, MES, EMP2 = 2026, 11, 2
REFERENCIA = date(ANO, MES, 1)
DESCONHECIDO = "[nao verificado]"


class EfeitoProibido(BaseException):
    pass


def proibir_efeito(*args, **kwargs):
    raise EfeitoProibido()


def ler_r1(Command, mes):
    # Anexo sonda.py. Os pontos de coleta são do handle real do E6.
    comando = Command(stdout=StringIO(), stderr=StringIO())
    codigo = comando.handle.__func__.__code__
    universo, div, falhas = {}, [], []

    def coletar(quadro, evento, argumento):
        # app/ponto/management/commands/e6_oraculo.py:307,316,401
        if evento == "line" and quadro.f_lineno == 307:
            dados = quadro.f_locals
            universo[dados["e"].pk] = set(dados["ids"])
        if evento == "exception" and quadro.f_lineno == 316 and not falhas:
            falhas.append(argumento[1:])
        if evento == "return":
            div.extend(quadro.f_locals.get("div", ()))
        return coletar

    def observar(quadro, evento, argumento):
        if evento == "call" and quadro.f_code is codigo:
            return coletar

    anterior = sys.gettrace()
    sys.settrace(observar)
    try:
        comando.handle(mes=mes, ano=ANO, hoje=None, so_txt=False, csv=None)
    finally:
        sys.settrace(anterior)
    if falhas:
        erro, pilha = falhas[0]
        raise erro.with_traceback(pilha)
    return universo, div


def selecionar(linha, *campos):
    return None if linha is None else {k: linha.get(k) for k in campos}


def medir(saida):
    from django.core.serializers.json import DjangoJSONEncoder
    from django.db import DatabaseError, connection, transaction
    from django.db.models import Model
    from django.urls import NoReverseMatch, Resolver404, get_urlconf, resolve, reverse, set_urlconf
    from django.utils import timezone

    from chamados.juizes import (
        FIO_SEM_PORTA, motivo_implausivel_validacao, pergunta_respondida, pergunta_viva,
    )
    from chamados.models import ChamadoColaborador, PerguntaDisputa
    from chamados.services.fio import chamado_que_lista
    from chamados.services.materializacao import (
        estado_aplicacao_pergunta, espera_declarado_x_gravado,
    )
    from chamados.services.pergunta_no_app import consulta_do_app
    from chamados.services.tarefa_do_dia import (
        carga_da_tarefa, porta_da_tarefa, tarefa_do_dia,
    )
    from chamados.services.validacao import _resposta_de_hora, entrega_que_barra_a_validacao
    from colaboradores.models import Colaborador, Empresa
    from colaboradores.services.calendario import resolver_dia_oferecido_nas_datas
    from escala.alimentacao import celulas_do_periodo, vinculo_do_dia_carregado
    from escala.models import EscalaColaborador
    from escala.utils import minutos_previstos_do_dia
    from folha.export import classificar_export
    from folha.fora_da_folha import DESTRAVA, _dias_que_retem, _fio_da_porta, dias_que_seguram, o_que_segura
    from folha.models import RetratoFolha
    from folha.retrato_folha import linha_do_colab, montar_linhas, o_que_fazer, portas_da_linha
    from ponto.janelas import janela_atual
    from ponto.management.commands.e6_oraculo import Command, InsumosDoDono
    from ponto.models import Ausencia
    from ponto.services import espelho as servico_espelho
    from ponto.services.ausencia import gate_aprovacao_ausencia
    from ponto.status_dia import status_do_dia_lavrado

    def emitir(bloco, pk=None, dia=None, **dados):
        registro = {"bloco": bloco, "pk": pk, "data": dia, **dados}
        print(json.dumps(registro, cls=DjangoJSONEncoder, ensure_ascii=False), file=saida)

    def erro_tipo(bloco, classe):
        print("ERRO_TIPO", bloco, classe, file=saida)

    def executar(bloco, leitor):
        falhas_sql = []

        def so_leitura(execute, sql, params, many, context):
            comando = sql.lstrip().upper()
            permitidos = ("SELECT ", "SAVEPOINT ", "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT ")
            if many or not comando.startswith(permitidos):
                raise EfeitoProibido()
            try:
                return execute(sql, params, many, context)
            except DatabaseError as erro:
                falhas_sql.append(erro)
                raise

        try:
            with transaction.atomic():
                with connection.execute_wrapper(so_leitura):
                    valor = leitor()
                    if falhas_sql:
                        raise falhas_sql[0]
            return valor
        except (Exception, EfeitoProibido) as erro:
            erro_tipo(bloco, type(erro).__name__)
            return None

    def colab(pk):
        return Colaborador.objects.select_related("empresa").get(pk=pk)

    def foto(c):
        return RetratoFolha.objects.filter(empresa=c.empresa, mes=MES, ano=ANO).first()

    def resolver_link(href):
        try:
            rota = resolve(urlsplit(href).path).view_name
        except Resolver404:
            return {"rota": None, "existe": False}
        return {"rota": rota, "existe": True}

    def rota_registrada(rota, pk):
        try:
            endereco = reverse(rota, args=[pk])
        except NoReverseMatch:
            return {"rota": rota, "existe": False}
        return {"rota": rota, "existe": resolver_link(endereco)["rota"] == rota}

    def links(portas):
        return [
            {"rotulo": p["rotulo"], **resolver_link(p["href"])}
            for p in portas
        ]

    def tarefa_publicada(t):
        campos = ("razao", "nao_sei", "fio", "rota", "acao")
        instancias = []
        for i in t["instancias"]:
            instancias.append({
                **selecionar(i, "instancia", "tipo", "razao", "motivo", "vencido"),
                "acoes": [
                    {**selecionar(a, "acao", "de"),
                     "porta": a["porta"] if isinstance(a["porta"], str) else None,
                     "porta_com_chamado": isinstance(a["porta"], int)}
                    for a in i["acoes"]
                ],
            })
        return {
            **selecionar(t, *campos),
            "tem_celula_na_tarefa": t["celula_id"] is not None,
            "instancias": instancias,
            "verbos": sorted({v for v in t["verbos"].values() if v is not None}),
        }

    def ler_a():
        c = colab(956)
        ini, fim = janela_atual(REFERENCIA, c.empresa)
        retrato = foto(c)
        gravado = linha_do_colab(retrato, c.pk)
        previa = next(
            (i for i in classificar_export(c.empresa, MES, ANO) if i["colaborador_id"] == c.pk),
            None,
        )
        atual = next(
            (i for i in montar_linhas(c.empresa, MES, ANO, agora=agora, hoje=hoje)
             if i["colab"] == c.pk),
            None,
        )

        def par(linha):
            return None if linha is None else (linha["status"], linha.get("motivo") or None)

        if par(previa) != par(atual):
            leitura = "leitores_atuais_divergem"
        elif previa is None:
            leitura = "sem_linha_nos_leitores_atuais"
        elif gravado is None:
            leitura = "sem_linha_na_foto"
        elif par(gravado) != par(previa):
            leitura = "defasagem_do_par_gravado"
        else:
            leitura = "pares_concordam"

        dias_gravados = None if gravado is None else gravado.get("datas")
        dias = _dias_que_retem(c, ini, fim, hoje=hoje)[1]
        return {
            "ini": ini, "fim": fim, "foto_existe": retrato is not None,
            "lavrado_em": None if retrato is None else retrato.lavrado_em,
            "retrato": par(gravado), "previa": par(previa), "em_memoria": par(atual),
            "comparacao": leitura, "datas_gravadas": dias_gravados,
            "datas_em_memoria": None if atual is None else atual.get("datas"),
        }, [
            (d["data"], selecionar(d, "motivo", "veredito")) for d in dias
        ]

    def ler_b(pk, r1):
        c = colab(pk)
        ini, fim = janela_atual(REFERENCIA, c.empresa)
        carga = carga_da_tarefa([c], ini, fim, agora)
        retencao = o_que_segura(c, MES, ANO, agora=agora)
        por_dia = {d["data"]: d for d in retencao["dias"]}
        insumos = InsumosDoDono(c, ini, fim, hoje, agora=agora)
        linha = linha_do_colab(foto(c), pk)
        tela = None
        if linha is not None:
            tela = {
                "campos": {r["chave"]: r["valor"] for r in o_que_fazer(linha, hoje=hoje)},
                "rotas": links(portas_da_linha(linha)),
            }
        cadastro = None
        if pk == 97:
            tem_escala_ativa = EscalaColaborador.objects.filter(colaborador=c, ativa=True).exists()
            cadastro = {
                "escala_a_definir": not tem_escala_ativa,
                "get": rota_registrada("colaboradores:painel_vinculo", pk),
                "post": rota_registrada("colaboradores:vincular_escala", pk),
                "envio_do_formulario": DESCONHECIDO,
            }

        linhas = []
        dia = ini
        while dia <= fim:
            t = tarefa_do_dia(c, dia, agora, carga=carga)
            fio = _fio_da_porta(porta_da_tarefa(t))
            retido = por_dia.get(dia.isoformat())
            marca = (
                None if linha is None or "dias_sem_porta" not in linha
                else dia.isoformat() in (linha["dias_sem_porta"] or ())
            )
            dono_dia = insumos.dono(dia)
            tela_dia = None
            if tela is not None:
                tela_dia = {**tela, "marca_sem_porta": marca}
            linhas.append((dia, {
                "escopo": "todos os dias da janela 11/2026; dias da CSV [nao verificado]",
                "ini": ini, "fim": fim,
                "tarefa": tarefa_publicada(t), "fio_folha": fio,
                "destrava": DESTRAVA.get(fio, DESCONHECIDO),
                "status_export": retencao["entra"], "motivo_export": retencao["motivo"],
                "retencao_dia": selecionar(retido, "motivo", "veredito"),
                "rotas_diarias": links(retido["portas"]) if retido is not None else [],
                "tela_retrato": tela_dia, "cadastro_97": cadastro,
                "dono_juiz": dono_dia,
                "dono_r1": (
                    DESCONHECIDO if r1 is None
                    else sorted(r1[1].get((c.empresa_id, pk, dia.isoformat()), ()))
                ),
            }))
            dia += timedelta(days=1)
        return linhas

    def ler_c():
        c, dia = colab(265), date(2026, 10, 31)
        ini, fim = janela_atual(dia, c.empresa)
        cel = celulas_do_periodo([c.pk], dia, dia).get((c.pk, dia))
        vinculo = vinculo_do_dia_carregado(c.pk, dia)
        carga = carga_da_tarefa([c], ini, fim, agora)
        grade, isento = carga.grade(c)
        gd = grade.get(dia)
        t = tarefa_do_dia(c, dia, agora, carga=carga)
        ata = {} if cel is None else cel.ata or {}
        return {
            "ini": ini, "fim": fim,
            "celula": None if cel is None else {
                "trabalha": cel.trabalha, "veredito": cel.veredito,
                "via": cel.veredito_via, "veredito_em": cel.veredito_em,
                "protestos": cel.protestos, "tem_geradora": bool(cel.escala_geradora_id),
                "tem_lampadas": bool(ata.get("lampadas")),
                "ata": selecionar(ata, "tipo_dia", "minutos_previstos", "n_celulas", "n_missing"),
            },
            "vinculo_do_dia": vinculo is not None,
            "ciclo_do_vinculo": None if vinculo is None else vinculo.tipo_escala.tipo_ciclo,
            "isento_grade": isento,
            "grade": selecionar(gd, "tipo_dia", "minutos_previstos", "veredito", "sem_ancora"),
            "previsto_min_juiz": minutos_previstos_do_dia(c, dia),
            "estado_d4": selecionar(status_do_dia_lavrado(c, dia, agora=agora), "status", "fonte"),
            "tarefa": tarefa_publicada(t),
            "fio_folha": _fio_da_porta(porta_da_tarefa(t)),
            "retencao_dia": next(
                (selecionar(d, "motivo", "veredito")
                 for d in _dias_que_retem(c, ini, fim, hoje=hoje)[1] if d["data"] == dia),
                None,
            ),
        }

    def ler_c_espelho():
        c, dia = colab(265), date(2026, 10, 31)
        ini, fim = janela_atual(dia, c.empresa)
        falhas_grade = []

        def observar_aviso(mensagem, *args, **kwargs):
            classe = sys.exc_info()[0]
            if mensagem.startswith("montar_dias: grade prevista falhou") and classe is not None:
                falhas_grade.append(classe.__name__)

        # app/ponto/services/espelho.py:583–585
        with patch.object(servico_espelho.logger, "warning", observar_aviso):
            esp = servico_espelho.espelho_do_colab(c, hoje, ini, fim, agora=agora)
        linha = next((d for d in esp["dias"] if str(d["data"])[:10] == dia.isoformat()), None)
        escala = esp["esc"]
        tipo = escala.tipo_escala if escala is not None else None
        insumos = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
        return {
            "ini_visual": esp["data_ini"], "fim_visual": esp["data_fim"],
            "ciclo_espelho": None if tipo is None else tipo.tipo_ciclo,
            "template_tem_hora_inicio": None if tipo is None else bool(tipo.hora_inicio),
            "grade_pelo_builder": bool(esp["resumo"].get("grade_pelo_builder")),
            "linha_existe": linha is not None,
            "marcos_previstos_canonicos": None if linha is None else len(linha["previsto_canon"]),
            "veredito_espelho": None if linha is None else linha["veredito_dia"],
            "dono_juiz": insumos.dono(dia),
        }, falhas_grade

    def indexar_r1():
        universo, div = ler_r1(Command, MES)
        por_dia = defaultdict(set)
        for eid, pk, dia, dono, *_ in div:
            por_dia[eid, pk, dia].add(dono)
        return universo, por_dia

    def

[truncado em 20000 de 30354 caracteres]

## 21/11 20:25 · sessao 9e691ebcaa22 · - · - · origem=Ronald (tmux)

O recorte de (d) sai de `dias_que_seguram` e `_dias_que_retem`, com porta diária publicada por `_porta` e traduzida por `portas_da_linha`. O leitor de referência por pessoa é `o_que_segura`. `app/folha/fora_da_folha.py:61,70,86,115,125,146,257,275,289` · `app/folha/retrato_folha.py:1024`

- **Validar no fio**. Ler `pergunta_respondida`, `espera_declarado_x_gravado`, `motivo_implausivel_validacao`, `entrega_que_barra_a_validacao` e `estado_aplicacao_pergunta`. O efeito no espelho e a oferta do botão têm respostas próprias. F6 pode impedir registro e conservar o botão, enquanto K6 e a espera do intervalo tiram o botão. O GET do modal marca follow-ups como lidos e deve ficar fora da execução. `app/chamados/services/validacao.py:681,697,703,740` · `app/chamados/services/materializacao.py:865,897` · `app/templates/chamados/partials/modal_fio.html:274` · `app/chamados/views.py:877`
- **Resolver dia**. `resolver_dia_oferecido_nas_datas` consulta o campo da tela e pode devolver `True`, `False` ou `None`. O POST depende de permissão, veredito e insumos adicionais, portanto seu resultado fica [nao verificado]. `app/colaboradores/services/calendario.py:97,125,157,804` · `app/ponto/views.py:3466,3482,3494`
- **Decidir ausência**. O painel lê `esta_aberta` e `pode_aprovar`. O POST aponta à raiz documental e consulta `gate_aprovacao_ausencia(raiz, confirmado=False)`. É possível publicar esse parecer e a abertura para rejeição, com resultado do ato [nao verificado]. `app/ponto/models.py:711,732` · `app/templates/ponto/partials/painel_ausencia.html:320,344` · `app/ponto/views.py:2749,2833` · `app/ponto/services/ausencia.py:762`
- **Fio residual ou app**. A publicação informa rumo e dono. O ato concreto de Decidir depende do tipo de chamado. Liberação de um POST sem ação e ator identificados [nao verificado]. `app/chamados/services/tarefa_do_dia.py:768,793,804` · `app/templates/chamados/partials/modal_fio.html:573,588,617`

Os agregados fornecidos para novembro e `emp2` somam 1.131 pares colab-dia pendentes, 25 sem porta e três sem leitura. A interseção por colaborador é 94 entre 240 pendentes e a união R1 de 128, com 146 só na pendência e 34 só na união. A identidade e a igualdade por dia estão [nao verificado]. A sonda pode recompô-las pela lista `div` do E6 capturada por `ler_r1`. `tabelas.txt` chaves C.P e B.P · `app/ponto/management/commands/e6_oraculo.py:378,401`

O envelope tem efeito próprio ao atualizar credencial, grants, rede e trilha. Ele deve ficar sem execução nesta volta de leitura. `bin/sonda_leitura.sh:121,140,153,184,216,248`

## 21/11 20:24 · sessao 36075f8a256b · - · - · origem=Ronald (tmux)

**(b)** Ler todos os dias inclusivos de `janela_atual(date(2026, 11, 1), empresa)`, com o corte de cada empresa. Consultar `tarefa_do_dia` com uma carga por colaborador e o mesmo `agora`. Usar `o_que_segura` para obter a retenção diária, o fio e o texto literal de `DESTRAVA`. Consultar `InsumosDoDono.dono` para cada dia e manter explícito que o R1 só registra esse dono quando o dia passou por suas exclusões e apresentou divergência. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/chamados/services/tarefa_do_dia.py:804`, `app/chamados/services/tarefa_do_dia.py:825`, `app/folha/fora_da_folha.py:46`, `app/folha/fora_da_folha.py:257`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:331`, `app/ponto/management/commands/e6_oraculo.py:378`.

O drawer da Folha usa `linha_do_colab` no retrato gravado e `portas_da_linha`. O rótulo diário **Sem porta** vem de `dias_sem_porta` e pode coexistir com links complementares da linha. Para o pk 97, a tela de vínculo exibe **A definir** quando falta a escala ativa. A rota de cadastro GET `colaboradores:painel_vinculo` contém o formulário para `colaboradores:vincular_escala` POST, condicionado ao posto e à permissão. O dono diário por ausência de vínculo é `CADASTRO` quando o juiz recebe esse insumo. A ocorrência e o estado da tela para os pks pedidos em produção estão [nao verificado]. `app/colaboradores/views.py:994`, `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1066`, `app/templates/colaboradores/partials/painel_vinculo.html:42`, `app/templates/colaboradores/partials/painel_vinculo.html:128`, `app/colaboradores/urls.py:12`, `app/colaboradores/views.py:401`, `app/ponto/management/commands/e6_oraculo.py:98`.

Resolver nomes de rota sem chamar views. O GET do fio marca follow-ups como lidos. Para Resolver dia, o leitor de oferta é `resolver_dia_oferecido_nas_datas`, que preserva desconhecimento como `None`. Para Validar, `entrega_que_barra_a_validacao` é uma guarda real, mas a execução ainda pode recusar por confirmação, materialização ou outros ramos. `app/chamados/views.py:869`, `app/colaboradores/services/calendario.py:125`, `app/chamados/services/tarefa_do_dia.py:697`, `app/chamados/services/validacao.py:209`, `app/chamados/services/validacao.py:693`, `app/chamados/services/validacao.py:740`, `app/chamados/services/validacao.py:769`.

_7770 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
