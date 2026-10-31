# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 18:10** · publicado em **31/10/2026 18:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 18:10 · sessao 98021fb68766 · - · - · origem=Ronald (tmux)

Alta certeza por leitura estática; execução dos casos e resultados em produção [não verificado].

- **Pergunta estreita — “esta FolgaDia vale neste dia?”:** **1 predicado**, `folga_lancada_vale_no_dia`: `None` não vale, manual vale, calendário exige mesmo vínculo (`app/ponto/precedencia.py:236`). **1 seletor**, que percorre todas as candidatas (`app/ponto/precedencia.py:217`). **2 consumidores diretos** do seletor: precedência (`app/ponto/precedencia.py:358`) e overlay da grade (`app/escala/utils.py:1124`). O carregador também é único, consulta a pessoa inteira e guarda todas por data em ordem de PK (`app/ponto/precedencia.py:207`).

- **Pergunta ampla — “este dia é folga?”:** não cabe contar apenas esse predicado. O registro separa trabalho do vínculo, fato vencedor e status (`app/core/juizes.py:158`, `app/core/juizes.py:160`, `app/core/juizes.py:173`). O calendário continua classificando status por `_classificar`, pendência já declarada da família tela (`app/colaboradores/services/calendario.py:108`; `app/core/juizes.py:819`). A grade continua publicando folga em outros ramos além do overlay (`app/escala/utils.py:727`, `app/escala/utils.py:989`, `app/escala/utils.py:1751`), e a lavra tem seu próprio mapeamento para status folga (`app/ponto/status_dia.py:105`). Logo “um sítio” é verdadeiro somente para a validade da **FolgaDia lançada**, não para todos os significados de folga.

- **Duplicação residual no novo helper:** `folga_humana_ou_feriado` já reconhece feriado ou faixa folga humana (`app/ponto/precedencia.py:188`); `trabalho_do_dia` volta a testar faixa feriado e faixa folga junto do booleano (`app/colaboradores/services/calendario.py:102`). Com o dict produzido pelo fluxo normal, é redundante; o helper recebe também a previsão e projeta `False` sobre ela (`app/colaboradores/services/calendario.py:105`, `app/colaboradores/services/calendario.py:819`). Não é segundo predicado de validade de vínculo, mas mantém uma regra local de dia de trabalho no calendário, enquanto o registro nomeia `EscalaColaborador.eh_dia_trabalho` (`app/core/juizes.py:158`).

- **Não há função morta da volta 4 identificada:** `trabalho_do_dia` tem chamadas de produção em `_classificar` e na publicação (`app/colaboradores/services/calendario.py:126`, `app/colaboradores/services/calendario.py:840`). `grade_espelho_janela` tem consumidores por alias no PDF, espelho e mapa, além de simulação (`app/relatorios/pdf_espelho.py:193`, `app/relatorios/pdf_espelho.py:471`, `app/relatorios/pdf_espelho.py:539`; `app/ponto/services/espelho.py:496`, `app/ponto/services/espelho.py:503`; `app/ponto/management/commands/mapa_divergencia.py:104`; `app/ponto/services/simular_dia.py:7`). `_folga_avulsa` tem chamador no veto (`app/ponto/precedencia.py:501`).

- **Portão:** busca textual encontrou `VERSAO_GRADE_FOLGA`, `_ata_abaixo_da_regra_de_folga` e o AST antigo somente em comentários/documentação, sem definição Python viva; a retirada está descrita em `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:5` e `app/ponto/services/cartorio.py:178`. O corpo vivo da porta chama builder e clipa, sem ler ata ou versão (`app/escala/utils.py:2286`). `versao_para` conserva consumidor no cartório (`app/ponto/services/cartorio.py:961`), não render nos resultados da busca. Ausência universal de referência dinâmica [não verificado]. Não há lápide local da função removida em torno da porta; só essa descrição histórica (`app/escala/utils.py:2272`; `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:5`).

**Escritores e fontes**

- `FolgaDia` armazena vínculo/data/origem e unicidade vínculo+data (`app/escala/models.py:1553`, `app/escala/models.py:1557`, `app/escala/models.py:1571`). Foto de calendário é escrita por `gerar_foto_folgas`, com `bulk_create` e origem preenchida (`app/escala/folgas.py:150`, `app/escala/folgas.py:173`); manual passa por `lancar_folga`, com veto e relavra (`app/escala/folgas.py:257`); edição interna por `_foto_do_dia` (`app/escala/folgas.py:267`, `app/escala/folgas.py:284`). Reparentamento escreve apenas vínculo e exclui calendário (`app/escala/folgas.py:405`, `app/escala/folgas.py:435`). O Plano chama essas portas (`app/escala/views_plano_folgas.py:124`).

- Grade escreve `tipo_dia` no builder base (`app/escala/utils.py:727`, `app/escala/utils.py:752`), no por turno (`app/escala/utils.py:1751`, `app/escala/utils.py:1757`), no overlay folga (`app/escala/utils.py:1126`) e ausência (`app/escala/utils.py:1182`, `app/escala/utils.py:1188`). A precedência escreve apenas dict de fatos, incluindo `folga_lancada` booleano, sem persistência (`app/ponto/precedencia.py:398`, `app/ponto/precedencia.py:408`).

**Contraexemplos**

1. **Encerrado no meio da competência:** geradora válida dentro da vigência ganha mesmo inativa; geradora fora da vigência devolve `None`, sem escolher outra (`app/escala/alimentacao.py:237`). Sem geradora, desempate `(ativa, data_inicio)` entre quem cobre (`app/escala/alimentacao.py:253`). Folga de calendário do antigo depois da troca é rejeitada pelo predicado; manual é aceita (`app/ponto/precedencia.py:238`). Certeza alta.

2. **Dois vínculos/duas folgas no mesmo dia:** modelo permite duas FolgaDia desde que vínculos diferentes (`app/escala/models.py:1571`); carregador guarda ambas e seletor pula a inválida (`app/ponto/precedencia.py:207`, `app/ponto/precedencia.py:217`). Vínculo do dia vem da geradora ou do desempate acima (`app/escala/alimentacao.py:237`). Certeza alta.

3. **Ausência de subtipo folga:** **divergência residual confirmada no fluxo**. `folga_compensatoria` e `feriado_folga` entram na faixa folga da A2 (`app/ponto/precedencia.py:62`), e helper humano reconhece subtipo (`app/ponto/precedencia.py:191`). A cobertura aprovada não exclui esses tipos (`app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2734`); a grade transforma todo slug diferente de falta em `tipo_dia='ausencia'` (`app/escala/utils.py:1187`). Calendário transforma esse fato humano em `eh_trabalho=False` e status folga sem batida (`app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:147`). Assim, não há igualdade geral grade `tipo_dia` × precedência `faixa` × calendário `status`; essa diferença preexiste ao diff, que não altera `_marcar_ausencias` (`app/escala/utils.py:1131`). Certeza alta por fluxo; execução [não verificado].

4. **Sem vínculo e sem outros fatos:** A2 diz sem jurisdição/`faixa=None` (`app/ponto/precedencia.py:385`); grade legacy diz folga (`app/escala/utils.py:987`) e por turno também publica folga quando não montou dia de turno (`app/escala/utils.py:1730`, `app/escala/utils.py:1751`). Certeza alta. Manual histórica ainda pode ser aceita mesmo com vínculo do dia `None` (`app/ponto/precedencia.py:238`); sem nenhum vínculo em toda a pessoa, não existirá FolgaDia associada por essa FK (`app/escala/models.py:1553`).

**Classes estruturais adicionais**

- **Relógio fora da borda:** por turno e porta usam `agora` recebido (`app/escala/utils.py:1242`, `app/escala/utils.py:2281`). Legacy e single conservam `timezone.localdate()` se chamados diretamente sem ref (`app/escala/utils.py:964`, `app/escala/utils.py:658`). Wrapper sempre lhes fornece ref (`app/escala/utils.py:2038`); portanto presença residual alta certeza, introdução pelo diff não demonstrada.

- **Fallback:** novo predicado usa `getattr(..., None)` e aceita objeto sem `calendario_origem_id` como manual (`app/ponto/precedencia.py:238`); comentário ainda diz “calendário passa True”, embora calendário agora deixe carregar objetos pelo carregador (`app/ponto/precedencia.py:230`; `app/colaboradores/services/calendario.py:617`). Precedência engole exceção de aritmética e vira `previsto=None` (`app/ponto/precedencia.py:367`). Esses comportamentos não são prova de erro no caso normal, mas são fallback vivo.

- **Derivação paralela:** a regra estreita antiga foi removida do overlay, que delega ao mesmo seletor (`app/escala/utils.py:1115`, `app/escala/utils.py:1124`). A regra ampla segue espalhada nos sítios acima; a própria pendência de status do calendário permanece registrada (`app/core/juizes.py:819`).

## 31/10 18:10 · sessao 86998d0f1cc8 · - · - · origem=Ronald (tmux)

Auditoria estática, sem executar testes nem acessar banco/log. Certeza **alta** = fluxo diretamente lido; **média** = conclusão negativa por busca/caminho; execução real de RED/suite/ensaio = **[não verificado]**. [nao verificado]

1. **Exportada pode mudar na célula/ata, sem trocar DNA por esse caminho — alta.** `processar_cartorio` seleciona todas as células passadas da empresa, sem piso por competência, e repassa `--forcar`: `app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`. O cartório monta a grade viva em `app/ponto/services/cartorio.py:945`; `ata_do_dia` copia `tipo_dia` dessa grade em `:581`; a lavra usa essa ata em `:1571`, `:1573`; salva ata/veredito e carimbos pela porta em `:1593`. O escritor físico da ata é `app/ponto/portas/celula.py:735`, `:767`; `carimbar` reúne campos e chama `salvar_e_emitir` em `:805`, `:807`; save + evento estão no mesmo atomic em `:639`. Não há regeneração de DNA nesse fluxo: campos persistidos estão delimitados em `:773`, `:805`.

2. **Fechamento/TXT intactos: propriedade estrutural deste caminho, com testemunha de teste; não imunidade universal — alta/média.** Cartório grava pelas portas acima; a porta salva `CelulaDia` e cria `EventoCelula`, `app/ponto/portas/celula.py:630`, `:640`. O teste exerce o command real e confere mudança de ata junto à manutenção do hash e TXT, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:83`, `:99`, `:103`, `:104`, `:106`. Contudo, “só REGEN pode alterar o fechamento” não é verdade universal da árvore: `recalcular_fechamento_mes` admite exportadas e tem escritor canônico direto `FechamentoMensal.objects.filter(...).update(...)`, `app/ponto/services/fechamento.py:469`, `:516`, `:1128`. A trava do `FechamentoMensal.save` só cobre status aprovado e é contornada deliberadamente pelo update canônico, `app/ponto/models.py:492`, `:498`; `app/ponto/services/fechamento.py:1115`.

3. **Pauta DP da cura não está conectada — média/alta.** A promessa está apenas na declaração da v3: `app/ponto/services/cartorio.py:179`. Código real de pago x devido existe em `app/chamados/services/passivo_validado.py::_pauta_da_competencia`, `:148`; monta “pago -> devido” em `:178`, `:192`, grava cabeçalho e filhas pela porta `escrever` em `:229`, `:233`. Chamador vem de outro fluxo, plantio do passivo seguido de REGEN, e só gera essa pauta se mudou o hash, `:427`, `:438`, `:442`, `:443`. REGEN devolve antes/depois, hashes, campos alterados e réguas em `app/ponto/portas/regen_exportada.py:216`; não cria a pauta por si. Logo não se pode afirmar que rejulgar pela v5 automaticamente entrega Pauta DP; entrega dessa diferença específica = **[não verificado]**. Há comentário antigo explicitando que Pauta é ato próprio posterior, `app/ponto/services/cartorio.py:155`.

4. **Portão de versão não sobrevive como seleção de regra/render — alta/média.** Busca de símbolos em todos os `.py` encontrou uso produtivo de `REGRA_VERSOES` e `versao_para` só no cartório: declaração `app/ponto/services/cartorio.py:110`, validação `:181`, leitor puro `:192`, cache `:959`. Versão entra na impressão em `:366`, consumida nos ramos nunca-bateu `:1013` e normal `:1165`; igualdade da impressão pula sem `--forcar`, `:1018`, `:1175`. Portanto há horizonte de relavratura automática, não seleção de regra da grade. `VERSAO_GRADE_FOLGA`, `_ata_abaixo_da_regra_de_folga` e `OsQuatroLeitoresChegamPeloPortaoTest` aparecem apenas em documentação histórica, `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:7`, `:15`; nenhuma definição/chamada viva encontrada. A afirmação negativa vem de busca, portanto certeza média.

5. **Testes da exportada têm mordida parcial — alta por inspeção, resultado RED [não verificado].** [nao verificado]
   - `test_MORDE_rejulgado...`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:92`, deveria matar a restauração de regra velha no escritor/ata, pois exige folga→trabalho. Sozinho deixa passar mutação que altera fechamento/TXT junto.
   - `test_o_gravado...`, `:97`, exige a mudança de ata e invariância dos observados. Deixa passar mutante que toca só campos excluídos do hash: `atualizado_em`, `aprovado_em`, `aprovado_por`, `previsto_em`, `derivado_em` etc., `app/ponto/portas/regen_exportada.py:37`, filtrados em `:56`. Não é igualdade da linha inteira.
   - `_txt`, teste `:79`, compara apenas `(hash_sha256, conteudo, total_linhas, invalidada_em)` do registro antigo. Deixa passar criação de nova exportação vigente e mudança de outros metadados. [nao verificado]
   - `test_a_testemunha...`, `:108`, exige só igualdade entre ata/grade no dia corrigido e trabalho no velho; mutante que faz ambos responderem folga no dia corrigido passaria essa função isolada, `:113`.
   - `test_premissa...`, `:86`, não prova correção, só fixture/versão.

6. **Relógio fora do parâmetro injetado já existe — alta, preexistente ao diff.** `julgar_celula(..., agora=None)` recebe relógio em `app/ponto/services/cartorio.py:1700`, mas calcula `hoje = _tz.localdate()` em `:1705` e compara `_tz.now()` com fim do turno em `:1726`; só depois usa o `agora` fornecido em `:1729`. Uma simulação com `agora` histórico cruza dois relógios. No command, relógio está na borda, `app/ponto/management/commands/processar_cartorio.py:38`. `versao_para` é puro e sem relógio, `app/ponto/services/cartorio.py:205`.

7. **Ordem de trava: nenhum lock novo no diff; sem inversão demonstrada — média.** A ordem da casa é Disputa→Colaborador, `app/chamados/models.py:841`; consumidor de eventos declara que não toma esses locks por conta própria, `app/chamados/reconciliador.py:1268`. A porta da célula faz atomic curto save+evento, `app/ponto/portas/celula.py:639`. Concorrência real e deadlock = **[não verificado]**.

8. **Pré-requisitos de pouso continuam sem prova disponível — alta quanto à exigência.** A própria declaração v3 exige ensaio `--apply` na sombra antes do pouso, com dias rejulgados/movimento de minutos/FORA-DO-HORIZONTE, `app/ponto/services/cartorio.py:173`. A regra de pista exige suite inteira verde antes de opus final e proíbe Codex aprovar pouso sozinho, `CLAUDE.md:549`, `:552`. Os anexos concedem mudança de lei; não fornecem execução desses requisitos. RED/GREEN v5, suite v5, ensaio v5, reversão e opus final = **[não verificado]**. RELATO contém números históricos da v4 e referências a logs, mas não equivalem a ensaio v5, `app/docs/RELATO.md:22`, `:32`.

## 31/10 18:08 · sessao 55f75766a99e · - · - · origem=Ronald (tmux)

**Paridade estática das cargas:** não achei contraexemplo determinístico entre lote e corpo antigo para uma fotografia estável e entradas válidas; execução contra banco/histórico integral permanece [nao verificado]. As cargas seguem as mesmas portas e janelas:

- **Células:** `celulas_do_periodo` produz `{(cid,data):cel}`; a casca converte para `{data:cel}`, e o lote faz a mesma conversão por colaborador. Pad comum: `ini−2` até `fim+1`. `app/escala/alimentacao.py:41-46`; `app/ponto/turnos.py:1832-1845`, `1882-1884`, `2951-2955`, `2974-2976`, `3005`.
- **Candidatos:** casca e lote usam a mesma implementação plural; janela `pad_ini..fim`, geradoras incluídas por `mais`, mesmos filtros e ordenação. O corpo comum reordena por `data_inicio`. `app/escala/alimentacao.py:116-127`, `153-154`; `app/ponto/turnos.py:1885-1886`, `1928`, `2957-2958`.
- **Ata:** a casca chega a `_escalas_da_ata` por `papel_por_minuto_da_ata`; o lote pré-carrega a mesma função. `app/ponto/turnos.py:1556-1559`, `1590-1591`, `1937`, `2963`.
- **Batidas:** mesmo filtro de retratadas e mesma janela. A ordenação por colaborador/timestamp é equivalente dentro de um colaborador; não há desempate explícito entre timestamps iguais. `app/ponto/models.py:105`; `app/ponto/turnos.py:1861-1864`, `2871-2879`, `2990-2993`. Divergência concreta causada por esse empate: [nao verificado].
- **Carimbos:** lote copia antes de `_marca_ecos_flush`, e o corpo comum copia novamente por segmento antes do pareador. `app/ponto/turnos.py:1934`, `1977-1981`, `3004`.
- **Última batida:** mesmo universo vivo da vida inteira; casca agrega individualmente, lote agrega todos uma vez. `app/ponto/turnos.py:1896-1901`, `2967-2972`.
- **DNA:** casca consulta preguiçosamente; lote fornece as células completas e faz zero queries nesse provedor. `app/ponto/turnos.py:1907-1911`, `2999-3002`; `app/escala/utils.py:96-107`.
- **Vínculos de encerramento:** ambas carregam todos os vínculos com `tipo_escala` e executam o mesmo corpo. `app/ponto/turnos.py:2145-2148`, `2164-2165`, `2960-2962`, `3012-3013`.

**Cascas:** as assinaturas/defaults permanecem e a extração preserva estaticamente os carregamentos, formatos documentados e predicados anteriores. `turnos_do_colab` espera células por `data`; `dias_encerrados` espera chaves `(cid,data)`. A incompatibilidade desses dois formatos ao repassar células sem fornecer turnos já existia no corpo anterior apresentado; não foi introduzida pela extração. `app/ponto/turnos.py:1848-1853`, `1913`, `1954`, `2125-2131`, `2145-2148`, `2907-2913`.

**Custo:** há teto constante de queries por janela, não igualdade absoluta para qualquer distribuição de dados: a segunda query dos candidatos e o agregado da última batida são condicionais. O lote também cresce com o número de datas distintas. Não identifiquei provedor novo que consulte por colaborador dentro de `CargaDaJanela`. `app/escala/alimentacao.py:123-127`; `app/ponto/turnos.py:2967-2972`, `2999-3006`; `app/ponto/status_dia.py:188-196`.

**Mordida dos testes:** [nao verificado]

- Custo exige 1/50/500 encerrados, todos com D4 e status `ok`, e igualdade das contagens; retorno ao singular por linha acrescentaria queries. Mordida estática identificada; execução RED real [nao verificado]. `app/core/tests/test_e4_7iii_lote_d4.py:47-61`.
- Paridade não prova equivalência com 973753a2: o singular atual chama o próprio lote. Uma regressão compartilhada que preserve as fontes passa — por exemplo, trocar o resultado `'previsto'` por `'aberto'` no ramo de relógio. O teste compara as duas formas e exige somente o conjunto de fontes. `app/ponto/status_dia.py:169-170`, `198-201`; `app/core/tests/test_e4_7iii_lote_d4.py:111-122`.

**Contraexemplo painel × calendário, normal e herdado:** use o colaborador da fixture `b`: 12x36 ativo 07–19, âncora 17/09/2026; E às 07h, S às 19h; célula `trabalha=True`, DNA 07–19, sem ata/veredito/status; agora local 20h. Esse cadastro e essas batidas já estão descritos nas fixtures. `app/core/tests/test_e4_7iii_painel_e_presenca.py:50-59`; `app/core/tests/test_e4_7iii_lote_d4.py:64`, `75-81`.

Nesse cenário, o painel marca `encerrado`, passa pelo encerramento do previsto e recebe D4 **`previsto/nao_lavrado`**, pois a data é hoje. `app/core/services/painel_op.py:192-194`, `332-338`; `app/ponto/turnos.py:2113-2122`; `app/ponto/status_dia.py:207-208`.

O calendário recebe células vazias da ata, não preenche `celstat_map`, vê E/S e turno fechado e cai em **`sem_certidao`**. `app/escala/services/leitor_celula.py:146-148`, `330-338`, `354-365`; `app/colaboradores/services/calendario.py:475-496`, `715-726`, `788-815`, `152-154`. Isso é inferência determinística do fluxo; ensaio com banco [nao verificado]. O ramo D4 de hoje já está no corpo antigo fornecido, e calendário/espelho estão fora do delta: diferença herdada, não regressão demonstrada da v3. O antecedente antigo depende do diff fornecido, sem arquivo local histórico: [nao verificado].

No espelho, a view usa `montar_semanas`, cujo status com batidas é `ok`, ou `atencao` se houver inconsistência — ambos distintos de `previsto`; a classificação exata do motor para essa fixture não foi executada. `app/ponto/views.py:183-184`; `app/ponto/services/espelho.py:159-164`, `666-670`.

## 31/10 18:07 · sessao 7dbdc6ddabbe · - · - · origem=Ronald (tmux)

Inventário de produção completo enviado nas mensagens anteriores, com os chamadores das três cascas, puros, métodos de `CargaDaJanela`, painel, commands, views e testes. [nao verificado]

Pontos adicionais para consolidar:

- O registro continua apontando ao singular em `app/core/juizes.py:173`; o singular delega integralmente ao plural em `app/ponto/status_dia.py:170`. O contrato verifica existência da referência, preservada, em `app/ponto/tests/test_contract_juiz_celula.py:65`.
- Escrita persistente de `status_dia/status_rotulo`: `app/ponto/portas/celula.py:705`, pela lavra declarada em `app/ponto/portas/celula.py:709`; D4 só lê a coluna em `app/ponto/status_dia.py:203`.
- Escrita de `linha['dia']`: inicial `None` em `app/core/services/painel_op.py:232`, preenchimento em `app/core/services/painel_op.py:338`; HTML lê status/rótulo em `app/templates/core/_painel_card.html:66`.
- Escrita de `linha['data_turno']`: inicial em `app/core/services/painel_op.py:222`, correção em `app/core/services/painel_op.py:333`. O botão Designar não recebe esse campo em `app/templates/core/_painel_card.html:69`; datas do modal saem do relógio do navegador e misturam data UTC com hora local em `app/templates/core/painel_operacional.html:294`.
- A atualização automática perde `status=`: URL contém apenas `fragment` e empresa em `app/templates/core/painel_operacional.html:147`. A view mantém filtro bruto no contexto em `app/core/views.py:669`, embora o serviço normalize-o em `app/core/services/painel_op.py:62`.
- Cada lote visual reconstrói o painel inteiro em `app/core/views.py:607`, depois fatia os postos em `app/core/views.py:613`; constante em queries não significa custo total independente do número de requisições de lote.
- Espelho continua com classificação própria em `app/ponto/services/espelho.py:152`; calendário também em `app/colaboradores/services/calendario.py:93`. Por exemplo sintético, uma lavra `'irregular'` mostrada pelo painel não pode aparecer com a mesma palavra no espelho: o D4 lê `'irregular'` em `app/ponto/status_dia.py:203`, enquanto espelho retorna `'atencao'` ou `'ok'` havendo batidas sem ausência em `app/ponto/services/espelho.py:159`. Execução desse cenário [nao verificado].
- B5-j atual vê casca e `Carga.turnos` porque ambas chamam literalmente o corpo em `app/ponto/turnos.py:1913` e `app/ponto/turnos.py:3003`. Entretanto, alias do novo corpo não é resolvido: condição cobre apenas `parear_turnos` em `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:71`. E o censo de arquivos continua exigindo texto `parear_turnos` em `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:143`; módulo só com `_turnos_do_colab_puro` escaparia.
- Outro selo perde cobertura para o nome plural: `PROIBIDOS` do cron só lista singular em `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:125`; o teste usa essa lista em `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:148`.
- Medidor e seu leitor host seguem a mesma implementação: `app/core/medida_leitor_do_dia.py:296` acompanha o corpo plural; `bin/leitor_dia_estado.py:35` chama o censo; `app/core/tests/test_medida_leitor_do_dia.py:74` cobra exclusões mortas. Não há referência de chave singular fixa nesse teste; a contagem usa chaves encontradas em `app/core/medida_leitor_do_dia.py:365`.
- Não execute `bin/tests/test_juiz_novo_tem_corte.sh` nesta auditoria: ele pode escrever sua base em `bin/tests/test_juiz_novo_tem_corte.sh:32` e `bin/tests/test_juiz_novo_tem_corte.sh:52`.

## 31/10 18:07 · sessao d0acfab94381 · - · - · origem=Ronald (tmux)

Conclusões dos selos e classes de reprovação:

- **Custo tem mordida estrutural contra singular por linha:** cria cumulativamente 1/50/500 colaboradores, exige todas as linhas encerradas e todos os D4 presentes com `ok`, mede a segunda montagem real e cobra igualdade das contagens. Voltar ao singular atual por linha cria `CargaDaJanela` por chamada, portanto volta a consultar por linha. A execução real/RED em `c2717f8c` é [nao verificado]. Referências: `app/core/tests/test_e4_7iii_lote_d4.py:33`, `:40`, `:47`, `:55`, `:57`, `:60`; `app/ponto/status_dia.py:170`, `:189`.

- **Paridade nova não prova equivalência com 973753a2:** o teste compara lote atual com singular atual, e o singular atual chama exatamente o lote com um par. Pode pegar contaminação entre colaboradores/datas, mas erro comum ao carregador plural de um e muitos passa. Sua cobertura só exige o conjunto de quatro fontes; não exige valores esperados independentes em cada par. Referências: `app/core/tests/test_e4_7iii_lote_d4.py:111`, `:118`, `:122`; `app/ponto/status_dia.py:169`. Existem expectativas independentes nos testes E3 antigos, portanto não afirmar ausência total de testes semânticos: `app/ponto/tests/test_e3_status_lavrado.py:209`, `:217`, `:240`, `:243`, `:249`.

- **B5-j vê os dois alimentadores atuais:** adiciona o nome `_turnos_do_colab_puro`, e ambos seus chamadores estão em `turnos.py`, arquivo que contém `parear_turnos`; Carga carrega as batidas dentro do método que chama o puro. Mutação exclusivamente em memória substituindo a porta de Carga por `Batida.objects` foi denunciada na linha 2990. Referências: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:67`, `:114`, `:143`; `app/ponto/turnos.py:2990`, `:3003`.

- **B5-j não vê “todo universo” irrestritamente:** `_producao` só inclui arquivo contendo a string `parear_turnos`; um novo módulo que somente importe/chame `_turnos_do_colab_puro` fica fora. `_apelidos` resolve alias apenas de `parear_turnos`; alias do puro passa. Seu teste novo não testa alias nem seleção de produção. Demonstração em memória de import `..._turnos_do_colab_puro as puro` com query crua devolveu `([], [])`. Referências: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:71`, `:143`, `:187`. É lacuna adicional ao gap interprocedural já documentado em `:26`.

- **Censo B5-j puro executado sem Django/escrita:** zero crus, seis copiadores; teto continua oito. Isso aceita cópias manuais e deixa folga para duas novas, em vez de garantir porta única. Critério e teto: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:122`, `:155`, `:169`.

- **Não há segundo juiz D4 de predicado:** registro vivo continua apontando ao singular, que delega ao único corpo plural; puros compartilhados alimentam as duas cargas. Referências: `app/core/juizes.py:173`; `app/ponto/status_dia.py:170`, `:195`; `app/ponto/turnos.py:3003`, `:3012`, `:3018`. `JUIZES.md:74` continua consistente com a entrada registrada.

- **Não há segundo escritor persistente introduzido:** D4 só monta resposta; painel altera campos do dict de apresentação. A porta aplica `status_dia/status_rotulo` e salva no ato da lavra. Censo AST do selo escritor, executado em memória sem runner/Django, retornou zero violações. Referências: `app/ponto/status_dia.py:193`, `:204`; `app/core/services/painel_op.py:333`, `:338`; `app/ponto/portas/celula.py:703`, `:773`; critério em `app/ponto/tests/test_selo_veredito_so_pela_porta.py:171`, `:181`, mordida em `:259`.

- **Relógio:** Carga não consulta relógio; `_d4_em_lote` recebe `hoje/agora` e os repassa; painel captura uma vez. Plural tem fallback `timezone.now()` quando `agora=None`, movido do singular segundo o diff fornecido, e `localdate(agora)` usa esse instante. Não é leitura por linha nem relógio novo no carregador. Referências: `app/ponto/turnos.py:2944`, `:3009`; `app/core/services/painel_op.py:51`, `:313`, `:334`, `:336`; `app/ponto/status_dia.py:182`, `:207`.

- **Medidor com chave movida funciona:** rodado apenas como módulo puro via `compile/exec`, censo deu `numero=5`, `status=5`, `valor=0`, e listas `novos/sumidos/sitios_mudaram/exclusoes_mortas/ilegiveis` vazias. A exclusão plural está viva com dois literais detectados (`previsto/sem_certidao`); singular delegado deixa de produzir palavras. Referências: `app/core/medida_leitor_do_dia.py:296`, `:350`, `:392`, `:406`; `app/ponto/status_dia.py:170`, `:207`. Isso não certifica procedência: limitações explícitas do detector incluem indireção, relógio sem palavra, templates/JS e troca de palavras mantendo contagem — `app/core/medida_leitor_do_dia.py:337`.

## 31/10 18:06 · sessao 36c105c9bd16 · F1C4-3-EMISSORES · censo · origem=Code

**A classificação sustentada pelos anexos é: (b) para os três leitores e X1/X2; (c) para RED e as duas pendências de sombra; (e) para a decisão documental do BLOQUEIA 1, os rascunhos e o estado do BACKLOG.** É uma classificação dos trechos fornecidos; eventual cobertura por perguntas anteriores não anexadas permanece **[não verificado]**.

As citações de `.q/.r` e triagem abaixo reproduzem a colagem: seu arquivo físico e sua numeração são **[não verificado]**. As referências ao código têm arquivo:linha da cópia.

1. **BLOQUEIA 1 — (e) NÃO PERGUNTADO: formalizar a interpretação estreita do aval.** A restrição a `trabalha=None` está explícita no código; o aval registrado diz “pergunta so em competencia aberta; exportada -> Pauta DP”. A PARADA 3 exige publicar a decisão contestável, com números, reversão e prova. (`app/chamados/services/fabrica_da_celula.py:149`; `app/chamados/services/fabrica_da_celula.py:153`; `app/docs/PROMPTS.md:1719`; `CLAUDE.md:454`)

   **Pergunta:** v12 apresenta como avais “JUIZ dia_sem_trabalho” e “F1C4-3-NONE-COBRA”; o item 5 pergunta se os documentos “descrevem o que o codigo faz hoje”. Não cobra conciliar o aval das **11:5x** com L-050/O44 nem publicar essa escolha pela PARADA 3. **Resposta:** v12, item 5, examina preenchimento do tipo, `None`, ata corrente e `_protesta`; não denuncia essa decisão documental ausente. **[não verificado]**

   A v8.r viu “A exceção do limbo exportado pode ser contornada pelo lote”, mas esse é outro achado: bypass do caminho existente. Isso não demonstra que tenha visto a falta de publicação da interpretação. O CONTESTÁVEL em `ESTACIONADO.md`, item 4, consta somente do opus fornecido. **[não verificado]**

2. **FILA — `flip_auto.py:212` — (b) VIU E A TRIAGEM DESCARTOU.** O código exige véspera `trabalha=True` nesse ramo. (`app/ponto/services/flip_auto.py:212`)

   **Pergunta v12.3:** “qualquer outro sitio em chamados/ e ponto/ que decida ‘dia sem trabalho’ ou ‘dia de trabalho’ […] liste-os”. **Resposta v12.3:** “`flip_auto` […] no noturno exige véspera `True` para recuar”; qualifica como “decisão geométrica entre dia/véspera”. **Triagem 3:** “DESCARTADO p/ esta obra — respondem o PREVISTO […] ou outra pergunta”. Portanto, o leitor foi identificado; a necessidade de uma linha individual de encaminhamento não foi denunciada expressamente. **[não verificado]**

3. **FILA — `auditar_invariantes_chamados.py:1542` — (b) VIU E A TRIAGEM DESCARTOU.** A seleção pula célula ausente ou `trabalha is not True`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1542`)

   **Pergunta v12.3:** “qualquer outro sitio […] liste-os”. **Resposta v12.3:** “auditor de chamados (`not trabalha`/`is not True`)”, citando inclusive `:1542`; distingue seu universo de invariantes/diagnósticos da pergunta de cobrança. **Triagem 3:** descarte por “PREVISTO […] ou outra pergunta”. Novamente, houve identificação do leitor, sem denúncia expressa da falta de linha individual. **[não verificado]**

4. **FILA — `declaracao_texto.py:90–91` — (b) VIU E A TRIAGEM DESCARTOU, com motivo específico.** O trecho trata os tipos projetados; o veto adicional a tipo diferente de trabalho está em `:95`. (`app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/declaracao_texto.py:95`)

   **Pergunta v12.1:** “Para cada leitor […] mesma autoridade […] ou tem predicado proprio?”. **Resposta v12.1:** “sobrevive o veto próprio `tipo_dia != 'trabalho'`”. **Triagem 1b:** “DESCARTADO p/ esta obra — pergunta de MARCOS (quais marcos faltam), ja na fila F1C4-3-NONE-LEITORES / EMISSORES-FILA […] vai ao opus”. A triagem considerou suficiente o encaminhamento genérico; o opus cobra a pendência nominal. **[não verificado]**

5. **FILA — rascunhos `c5c43360`/`fbf3882d` sem LEI-AKITA no histórico — (e) NÃO PERGUNTADO.** O postmortem anterior já registra essa classificação para `c5c43360` e promete incluir mensagens de commit na pergunta futura. (`app/docs/RELATO.md:14`)

   **Pergunta v12:** “Leia o codigo na copia”; não solicita examinar mensagens dos commits. **Resposta:** não apresenta auditoria das mensagens desses dois rascunhos. O pedido genérico de “qualquer outro defeito no delta” não explicita esse tema. A ausência efetiva da linha em ambos os históricos, especialmente `fbf3882d`, permanece **[não verificado]**.

6. **FILA — RED dos quatro emissores sem log encontrado — (c) FORA DO ALCANCE DEMONSTRADO.** Os quatro casos existem no código; isso comprova presença dos testes, não execução histórica em vermelho. (`app/chamados/tests/test_f1c4_3_emissores.py:80`; `:100`; `:118`; `:140`)

   **Perguntas:** v12.4 e v8.3 pedem “qual mutacao minima […] o deixa vermelho”, não fornecem o log histórico desses quatro emissores. **Respostas:** v12 declara “Execução dos testes Django e dos mutantes completos: [não verificado]”; v8 também limita suas conclusões à análise do código. **[não verificado]** [nao verificado]

   O RELATO afirma “RED fbf3882d evidenciado”, mas essa declaração documental não substitui o log procurado pelo opus. (`app/docs/RELATO.md:140`) O log e seu resultado efetivo permanecem **[não verificado]**.

7. **FILA — sombra medida em `c5c43360`, não na árvore que pousa — (c) FORA DO ALCANCE DEMONSTRADO.** A obrigação de sombra com chamados novos por emissor antes do pouso está documentada. (`app/docs/BACKLOG.md:833`)

   **Pergunta v12.5:** documentos “descrevem o que o codigo faz hoje?”. **Resposta v12.5:** “As quantidades e resultados de sombra registrados no BACKLOG são declarações documentais; sua confirmação independente permanece [não verificado]”. **Triagem 6:** afirma medição “pelo cartorio real (`v12_sombra.log`)”. Esse log não foi fornecido; a atribuição à árvore `c5c43360` vem do opus e permanece **[não verificado]**.

8. **FILA — segunda passada da sombra sem log — (c) FORA DO ALCANCE DEMONSTRADO.**

   **Pergunta v8.3:** pede analisar os testes novos; **resposta v8.3:** “A segunda passada escreve outra pauta” descreve o efeito previsto de uma mutação no teste, não uma segunda execução registrada da sombra de produção. **Resposta v12.5:** mantém a confirmação independente da sombra como “[não verificado]”. Portanto, essa menção à segunda passada dos testes não comprova a segunda passada exigida pelo opus. **[não verificado]** [nao verificado]

9. **FILA — BACKLOG F1C4-3-EMISSORES “em construcao” — (e) NÃO PERGUNTADO quanto à atualização do estado.** O texto está presente. (`app/docs/BACKLOG.md:833`)

   **Pergunta v12.5:** nomeia o BACKLOG com foco em “SUPRA-DELEGA-DIA-SEM-TRABALHO, CARTORIO-PROTESTA-1A-CLAUSULA” e correspondência com o código. **Resposta v12.5:** examina esses contratos, sem apontar o estado da linha EMISSORES. Classifico como **(e)** porque o encerramento/estado da obra não foi explicitamente perguntado; ler a pergunta como auditoria de todo o BACKLOG permitiria **(a)**, mas ampliaria seu recorte expresso. Além disso, código implementado não comprova obra encerrada. **[não verificado]**

10. **FILA — X1, concorrência na Pauta — (b) VIU E A TRIAGEM DESCARTOU.** A consulta de deduplicação antecede a chamada de escrita; as constraints do modelo não estabelecem unicidade por dia/motivo. (`app/chamados/services/fabrica_da_celula.py:180`; `:185`; `app/pautas/models.py:123`)

    **Pergunta v12:** “Aponte tambem qualquer outro defeito no delta”. **Resposta:** “a nova deduplicação de Pauta permite duplicatas sob concorrência”. **Triagem X1:** “DESCARTADO p/ esta volta — desenho da volta 3, sem unicidade no modelo; fila; vai ao opus”. A v8.r também já identificava a possibilidade. Duplicação efetiva em produção permanece **[não verificado]**.

11. **FILA — X2, catraca AST não prova fluxo — (b) VIU E A TRIAGEM DESCARTOU COMO BLOQUEIO.** Para os commands, a catraca procura a chamada pelo nome no arquivo; o adaptador exige a chave por acesso direto. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:228`; `app/ponto/supra_juiz.py:70`)

    **Pergunta v12:** “Aponte tambem qualquer outro defeito no delta”. **Resposta:** “Manter essa chamada e trocar o argumento de `classificar_dia` por `{}` continua satisfazendo a verificação por nomes”. **Triagem X2:** “DESCARTADO como bloqueio — `{}` com vivos>0/prev 0/cob None levanta KeyError na porta […] fila; vai ao opus”. O motivo aceita a falha em runtime como contenção; não refuta a deficiência da catraca apontada pelo Codex. **[não verificado]**

## 31/10 18:05 · sessao 8b041bea5bba · - · - · origem=Ronald (tmux)

- **Rascunhos sem LEI-AKITA: (e) NÃO PERGUNTADO.** O postmortem anterior já registra essa classe para `c5c43360` e promete incluir mensagens dos commits na pergunta seguinte (`app/docs/RELATO.md:14`). O `v12.q` fornecido pede “Leia o codigo na copia” e correspondência documental; não pede mensagens/histórico. A ausência de LEI-AKITA em `fbf3882d` permanece **[não verificado]**.

- **RED dos quatro emissores sem log: (c), execução fora do alcance demonstrado.** Existem casos dos quatro emissores em `app/chamados/tests/test_f1c4_3_emissores.py:80`, `:100`, `:118`, `:140`. `RELATO.md:140` declara “RED fbf3882d evidenciado (Ran 12 FAILED failures=4...)”, mas isso é registro documental, não o log. No anexo `v12.r`, o limite é explícito: “Execução dos testes Django e dos mutantes completos: [não verificado]”. Log efetivo ausente e resultado histórico: **[não verificado]**.

- **Sombra em árvore anterior e segunda passada sem log: (c).** O requisito de sombra com chamados novos por emissor antes do pouso está em `app/docs/BACKLOG.md:833`. O `v12.r` fornecido diz: “As quantidades e resultados de sombra registrados no BACKLOG são declarações documentais; sua confirmação independente permanece [não verificado]”. Árvore `c5c43360` e segunda passada específica: **[não verificado]**.

- **BACKLOG “em construcao”: ocorrência confirmada; classe exige cuidado.** O texto existe em `app/docs/BACKLOG.md:833`. O `v12.q5` pergunta se BACKLOG e outros documentos “descrevem o que o codigo faz hoje”; o `v12.r5` não menciona esse estado **[não verificado: anexos fornecidos sem arquivo local]**. Pode ser **(a)** sob leitura ampla da pergunta documental; sob leitura estrita, o estado de encerramento não foi perguntado, portanto **(e)**. Código implementado não demonstra obra encerrada, e o opus fornecido ainda a reprova **[não verificado]**.

## 31/10 18:05 · sessao 47f011ccdfdd · - · - · origem=Ronald (tmux)

**BLOQUEIA 1: classificação recomendada (e) — NÃO PERGUNTADO**, limitada às perguntas fornecidas; a falha documental é verificável em princípio por leitura, portanto não cabe classificá-la automaticamente como (c). O aval literal manda “pergunta so em competencia aberta; exportada -> Pauta DP”, enquanto a implementação restringe a exceção a `cel.trabalha is None` e invoca O44 para continuar perguntando quando `True`. (`app/docs/PROMPTS.md:1719`; `app/chamados/services/fabrica_da_celula.py:149-157`)

A obrigação de publicar a escolha contestável com números e reversão consta em `CLAUDE.md:454-458`; a lei L-050 registra que a fronteira de exportado para chamado/pergunta deixou de existir por O44. (`app/docs/LEIS.md:91`)

Trechos dos anexos que sustentam **(e)**:

- **Pergunta v12:** “Avais do dono que regem a obra: ‘JUIZ dia_sem_trabalho’ [...] e F1C4-3-NONE-COBRA”; pergunta 5: “Os textos de CORTES.md, BACKLOG.md (SUPRA-DELEGA-DIA-SEM-TRABALHO, CARTORIO-PROTESTA-1A-CLAUSULA), JUIZES.md e o comentario de _C18 descrevem o que o codigo faz hoje?”. Não solicita conferir a interpretação do aval das 11:5x nem a publicação pela PARADA 3. `[não verificado: anexos colados, sem arquivo:linha na cópia]`
- **Resposta v12, item 5:** “Correspondência entre documentos e código” trata de preenchimento do tipo, ressalva de `None`, ata corrente e `_protesta`; não apresenta a falta de decisão formal sobre exportadas. `[não verificado: anexo colado, sem arquivo:linha]`
- **Resposta v8, outros defeitos:** “A exceção do limbo exportado pode ser contornada pelo lote.” Isso identifica outro problema — bypass pelo FIO MUDO —, não a ausência de publicação da interpretação contestável; logo não demonstra classe (b) para o BLOQUEIA 1. `[não verificado: anexo colado, sem arquivo:linha]`

A pergunta genérica “qualquer outro defeito” oferece oportunidade de descoberta, mas não torna explicitamente perguntada a exigência documental específica da PARADA 3. `[não verificado: inferência de classificação sobre os anexos]`

**Limite:** não localizei `ESTACIONADO.md` na cópia; a afirmação de que seu item 4 já dizia CONTESTÁVEL depende exclusivamente do opus fornecido. `[não verificado]`

## 31/10 17:58 · sessao b1f133212c08 · PADRAO-DE-CIMA · verificador · origem=Code

**Pré-veredito: REPROVA.** Há caminhos que liberam por falta de sinal, uma liberação anterior que pode valer na primeira observação e uma corrida entre estado e escopo. **Certeza alta sobre os fluxos; ocorrência em produção [não verificado].** (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1477`, `bin/disjuntor.py:1374`, `bin/disjuntor.py:1401`, `bin/disjuntor.py:1421`)

Fiz leitura estática e sondas isoladas em memória, com I/O substituído. Resultado das suítes completas: **[não verificado]**; executá-las criaria e apagaria arquivos, contrariando sua restrição. (`bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:36`, `bin/tests/test_lei_akita.sh:168`)

**1. Quem escreve e quem lê**

Certeza **alta** nesta tabela; autoria humana efetiva e execução em produção ficam **[não verificado]**.

| Campo/arquivo | Quem escreve | Quem lê |
|---|---|---|
| **RELATO — topo** | Escrita manual é reconhecida pelo código; o diff não escreve `DESENHO` nem `PADRAO-DE-CIMA`. A porta de alarmes preserva o topo. (`app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:839`, `app/core/esteira_vigia.py:872`) | Recorte central em `_bloco_do_topo`; PAREI/ESPERANDO por `_linha_do_topo`; B3 por `_topo_consistente`; PDC por `_topo_de` sobre os bytes do corpo. (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:415`, `bin/disjuntor.py:676`, `bin/disjuntor.py:1359`) |
| **RELATO — corpo e negativas** | `relato_negacao` chama `esteira_vigia.no_relato`, que toma flock do diretório e publica por `replace`. **Também existem escritores fora dessa porta:** `gerar_cortes` e `deploy_agendado`. (`bin/disjuntor.py:234`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:848`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`) | PDC varre o arquivo inteiro; fora do diff, Stop verifica prova, handoff lê o bloco inicial e publicação lê o RELATO. (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1365`, `bin/hook_stop_fila1.py:1176`, `bin/handoff_sessao.sh:92`, `bin/relato.sh:56`) |
| **BACKLOG — bloco OBRAS** | Bloco declarado manual; `gerar_backlog` preserva-o e regrava o documento; `backlog_estado` altera a célula de estado diretamente. (`bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:119`, `bin/gerar_backlog.py:140`, `bin/backlog_estado.py:58`, `bin/backlog_estado.py:59`) | Selo PDC, Stop/fila, handoff e gerador do resumo. **O resolvedor PDC não consulta BACKLOG.** (`bin/tests/test_padrao_de_cima.sh:328`, `bin/hook_stop_fila1.py:173`, `bin/handoff_sessao.sh:54`, `bin/gerar_backlog_topo.py:12`, `bin/disjuntor.py:1282`) |
| **`PROGRAMAS`** | Cadastro literal no código, separado do BACKLOG. (`bin/disjuntor.py:1110`) | `_PdcResolvedor.entidades`, somente sem entidade; selo verifica as chaves contra OBRAS. (`bin/disjuntor.py:1282`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **GLOSSARIO / PLANO-PADROES** | Estes leitores não os alteram; padrão novo exige corte. Autor efetivo de cada alteração: **[não verificado]**. (`bin/padrao_vocab.py:34`, `CLAUDE.md:70`) | `padrao_vocab.vocabulario`: duas primeiras colunas do GLOSSARIO, negritos do NORTE e itens dos ADRs. (`bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`, `bin/padrao_vocab.py:53`) |
| **Vocabulário em memória** | Produzido por `padrao_vocab.vocabulario`, normalizado por `limpo`. (`bin/padrao_vocab.py:21`, `bin/padrao_vocab.py:57`) | LEI-AKITA e PDC. (`bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1327`, `bin/disjuntor.py:1315`) |
| **Estado PDC:** `eventos`, `visto_em`, `entidades`, `nomeados`, `libera_vistas`, `disparadas` | `decide_pdc`, persistido por `_com_estado` sob flock. (`bin/disjuntor.py:1360`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:260`, `bin/disjuntor.py:281`) | Janela/agrupamento, `estado_pdc` e painel. (`bin/disjuntor.py:1287`, `bin/disjuntor.py:1292`, `bin/disjuntor.py:1458`, `bin/disjuntor.py:1537`) |
| **Escopo:** `em`, `disparadas`, `obras_com_evento` | `_pdc_grava_escopo`, depois da transição do estado. (`bin/disjuntor.py:1421`, `bin/disjuntor.py:1432`) | Fallback de `estado_pdc`. A decisão consulta `disparadas`; não valida frescor de `em` nem usa `obras_com_evento` nesse ramo. (`bin/disjuntor.py:1446`, `bin/disjuntor.py:1463`) |
| **`disjuntor.log` / painel** | `trilha`; PDC anuncia disparo/liberação após tentativa de persistência. (`bin/disjuntor.py:135`, `bin/disjuntor.py:1413`) | `ler` conta eventos; `_pdc_painel` lê o estado; `linha` apresenta os contadores. (`bin/disjuntor.py:2055`, `bin/disjuntor.py:2077`, `bin/disjuntor.py:1537`, `bin/disjuntor.py:2189`) |
| **`DISJUNTOR_DESLIGADO`** | Escritor autorizado: terminal do Ronald; Code recebe guarda contra ferramentas que nomeiem a chave. Fixtures também a criam. (`bin/disjuntor_settings.json:3`, `bin/disjuntor.py:167`, `bin/disjuntor.py:171`, `bin/tests/test_padrao_de_cima.sh:313`) | `desligado`, por `lexists`, inclusive symlink; usado nas portas B3/PDC, PreToolUse e Stop. (`bin/disjuntor.py:153`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`) |
| **B3: estado e triagem** | `decide_b3`, `rearma_b3` e `triagem`, pela trava comum do estado. (`bin/disjuntor.py:985`, `bin/disjuntor.py:1058`, `bin/disjuntor.py:838`, `bin/disjuntor.py:844`) | `julga_b3` → `estado_b3` → `b3_parada`/`_nega_b3`; Stop chama `julga_b3`. (`bin/disjuntor.py:1006`, `bin/disjuntor.py:1015`, `bin/disjuntor.py:1072`, `bin/disjuntor.py:1889`, `bin/hook_stop_fila1.py:1048`) |

Os hooks configurados em `.claude/` apontam para caminhos absolutos da árvore viva. Que a sessão de produção executava exatamente este hash: **[não verificado]**. (`.claude/settings.json:9`, `.claude/settings.json:68`)

**2. O vocabulário tem uma fonte?**

**Uma implementação da regra, alimentada por dois documentos.** Os dois consumidores chamam `padrao_vocab.vocabulario`; a extração saiu do heredoc do LEI-AKITA. **Certeza alta.** (`bin/padrao_vocab.py:34`, `bin/tests/test_lei_akita.sh:89`, `bin/disjuntor.py:1327`)

**Não é garantida a mesma lista concreta:** LEI-AKITA lê os documentos de `ALVO` por `git show`; PDC lê o disco da raiz. A igualdade depende desses conteúdos coincidirem. **Certeza alta; coincidência em produção [não verificado].** (`bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:77`, `bin/disjuntor.py:1325`)

Há regex próprias para **formatos diferentes**: campo LEI-AKITA e quatro campos da liberação PDC. Ambas delegam a validade do termo ao vocabulário compartilhado. Há ainda termos esperados no MORDE e o conjunto `B3_LEIS`, que julga o desenho B3, não o universo de `padrao=`. **Certeza alta.** (`bin/tests/test_lei_akita.sh:99`, `bin/disjuntor.py:1130`, `bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1315`, `bin/tests/test_lei_akita.sh:163`, `bin/disjuntor.py:87`)

Outra implementação equivalente escondida em toda a árvore: **[não verificado]**; não foi localizada na busca textual.

**3. Falhas e limites**

| Situação | Comportamento e certeza |
|---|---|
| **GLOSSARIO ausente** | **Alta:** LEI-AKITA recebe `VocabIlegivel` e falha pela ausência de conclusão do juiz. PDC não libera nem consome a candidata. Porém o vocabulário só é consultado quando há linha candidata no topo: ausência do documento não provoca recusa global. (`bin/padrao_vocab.py:46`, `bin/tests/test_lei_akita.sh:90`, `bin/tests/test_lei_akita.sh:298`, `bin/disjuntor.py:1384`, `bin/disjuntor.py:1388`, `bin/disjuntor.py:1390`) |
| **RELATO legível, sem topo útil** | **Alta:** o recorte pode retornar `[]`; isso não libera entidade disparada. Um arquivo vazio também não fornece novos eventos. Não existe validação separada de “topo obrigatório”. (`bin/hook_stop_fila1.py:427`, `bin/hook_stop_fila1.py:443`, `bin/disjuntor.py:1365`, `bin/disjuntor.py:1383`) |
| **RELATO ilegível com estado legível** | **Alta, confirmado em memória:** `decide_pdc` preenche `nao_sei`, mas `estado_pdc` ignora-o. Sem disparada conhecida para a obra, retorna **LIVRE**. Com estado vazio, o portão fica verde sem ter lido a fonte. (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1477`) |
| **BACKLOG perde uma chave cadastrada** | **Alta:** runtime permanece igual, pois usa o dict literal; o selo reprova a divergência. Se a obra faltar no próprio dict e não resolver modelo, recebe entidade vazia e não agrupa. (`bin/disjuntor.py:1110`, `bin/disjuntor.py:1282`, `bin/disjuntor.py:1284`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **Codex rc 75** | **Alta:** limite identificado registra `limite.log`, orienta seguir por sonnet e retorna 75, sem sidecar válido desta chamada. A continuação efetiva da obra é **[não verificado]**; o wrapper apenas emite a orientação. (`bin/codex_pergunta.sh:186`, `bin/codex_pergunta.sh:191`, `bin/codex_pergunta.sh:192`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:201`) |
| **Disjuntor desligado** | **Alta:** portas B3/PDC retornam zero; PreToolUse e Stop pulam os disjuntores. A guarda contra alterar a chave continua ativa. (`bin/disjuntor.py:1070`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1978`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`) |
| **Falha do censo/resolvedor** | **Alta, confirmado em memória:** exceção vira `entidades=[]`, persistida no evento. Restaurar o censo não reclassifica o mesmo SHA, porque só eventos novos são resolvidos. Pode permanecer verde por ausência de entidade. (`bin/disjuntor.py:1366`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1298`) |
| **Falha ao gravar liberação** | **Alta:** a liberação é recolocada no resultado como disparada; nesta chamada continua recusada. (`bin/disjuntor.py:1418`, `bin/disjuntor.py:1419`) |
| **Estado ilegível** | **Alta:** usa escopo legível; fora dele retorna LIVRE. Sem escopo, recusa obras com evento; sem RELATO legível, recusa todas. (`bin/disjuntor.py:1461`, `bin/disjuntor.py:1465`, `bin/disjuntor.py:1471`) |

Mais dois bloqueios:

- **Liberação anterior aceita na primeira observação — alta, confirmado em memória.** Encontrando pela primeira vez dois desenhos e uma liberação válida já no topo, o ciclo dispara e libera imediatamente. O mtime é descartado; não existe fotografia das linhas anteriores ao disparo como no B3. O selo cobre somente liberação consumida numa observação anterior com um desenho. (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1374`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:197`)
- **Escopo pode regredir — alta, inferência de concorrência.** A gravação ocorre após liberar a trava do estado. A decide primeiro e pausa; B decide depois e publica escopo novo; A publica o antigo por cima. Na próxima falha do estado, uma obra omitida pelo escopo antigo recebe LIVRE. `os.replace` evita arquivo parcial, mas não essa inversão temporal. (`bin/disjuntor.py:295`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1421`, `bin/disjuntor.py:1437`, `bin/disjuntor.py:1465`)

**4. Os selos mordem?**

**Há mordida comportamental, com lacunas.** Execução integral e sobrevivência experimental dos mutantes abaixo: **[não verificado]**; os graus indicados são por inspeção.

| Selo | Mordida existente | Mutante simples que pode escapar |
|---|---|---|
| **`test_padrao_de_cima.sh`** | **Alta:** portas reais, liberações, redisparo, janela, entidades distintas, desligamento e controles sem mutação. Apagar o gate do wrapper tem mutante declarado. (`bin/tests/test_padrao_de_cima.sh:110`, `bin/tests/test_padrao_de_cima.sh:179`, `bin/tests/test_padrao_de_cima.sh:285`, `bin/tests/test_padrao_de_cima.sh:377`, `bin/tests/test_padrao_de_cima.sh:388`) | **Alta por inspeção:** remover somente `O261-E-CELULA-20-UM-DONO` do cadastro, ou trocar seu destino. O selo exige apenas chaves existentes e dict não vazio; os cenários de programa usam outras duas obras. (`bin/disjuntor.py:1113`, `bin/tests/test_padrao_de_cima.sh:157`, `bin/tests/test_padrao_de_cima.sh:333`) |
| **`test_lei_akita.sh`** | **Alta:** lista vazia, inversão da aceitação e retirada da validação são atingidas pelos termos obrigatórios e casos positivos/negativos. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:203`) | **Alta por inspeção:** substituir o `raise VocabIlegivel` do GLOSSARIO por `pass`. Os cenários fornecem as fontes; não exercitam sua ausência. O mutante permitiria vocabulário parcial quando o GLOSSARIO faltasse. (`bin/padrao_vocab.py:45`, `bin/padrao_vocab.py:46`, `bin/tests/test_lei_akita.sh:172`, `bin/tests/test_padrao_de_cima.sh:74`) |
| **`test_disjuntor.sh`** | **Alta:** verifica leituras dentro das transições e agora exige duas tomadas da trava no verificador final, B3 e PDC. (`bin/tests/test_disjuntor.sh:845`, `bin/tests/test_disjuntor.sh:879`, `bin/tests/test_disjuntor.sh:882`) | **Alta por inspeção, considerando este selo isoladamente:** apagar a consulta ao vocabulário em `_pdc_libera`. Seus acréscimos não exercitam liberação PDC; o selo específico é quem possui esse mutante. (`bin/disjuntor.py:1315`, `bin/tests/test_disjuntor.sh:432`, `bin/tests/test_disjuntor.sh:882`, `bin/tests/test_padrao_de_cima.sh:396`) |

Há duas mordidas contaminadas:

- **Alta:** `B3-foto-parcial-engolida` injeta `H._bloco_do_topo()` onde `H` não está definido; os imports são locais a outras funções. A morte pode ser `NameError`, aceito pelo harness como qualquer falha. (`bin/tests/test_disjuntor.sh:1663`, `bin/tests/test_disjuntor.sh:1667`, `bin/disjuntor.py:676`, `bin/disjuntor.py:688`, `bin/disjuntor.py:702`, `bin/tests/test_disjuntor.sh:1491`)
- **Alta:** retirar a guarda “entidade já disparada” leva a `disp[ent]` inexistente no cenário antecipado; o harness também aceita esse erro como morte. Isso não prova a semântica temporal pretendida. (`bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:397`, `bin/disjuntor.py:1402`, `bin/tests/test_padrao_de_cima.sh:375`)

**5. BUG 6**

**Curado na porta defeituosa do wrapper. Certeza alta.** `b3_parada` testa a chave antes de consultar estado; o teste específico remove exatamente essa guarda no mutante. (`bin/disjuntor.py:1070`, `bin/tests/test_padrao_de_cima.sh:319`, `bin/tests/test_padrao_de_cima.sh:385`)

Os outros caminhos produtivos encontrados também têm guarda: PreToolUse antes de `pretool`, e Stop antes de `julga_b3`. **Não encontrei outro chamador produtivo desprotegido.** As funções internas `decide_b3`/`estado_b3` continuam sem guarda própria; uma chamada direta não herda o desligamento. **Certeza alta nos caminhos citados; ausência absoluta de outro chamador [não verificado].** (`bin/disjuntor.py:1979`, `bin/disjuntor.py:1930`, `bin/disjuntor.py:1944`, `bin/hook_stop_fila1.py:1039`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:903`, `bin/disjuntor.py:1015`)

**6. Classes históricas procuradas**

| Classe | Resultado e grau de certeza |
|---|---|
| **Segundo juiz duplicando registrado** | Reuso do censo e do juiz do vocabulário, confirmado. `_so_modelos` acrescenta filtro de herança; `censo.modelos` coleta classes mais amplamente. Duplicação de juiz de negócio não demonstrada; ausência absoluta **[não verificado]**. (`bin/disjuntor.py:1197`, `bin/disjuntor.py:1216`, `bin/disjuntor.py:1242`, `app/core/censo_escritas.py:90`, `app/core/censo_escritas.py:109`) |
| **Segundo escritor fora da porta** | **Encontrado, alta:** `gerar_cortes` e `deploy_agendado` regravam RELATO fora do flock comum; risco de sobrescrever atualização concorrente. Resíduo já declarado no código. (`bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`) |
| **Relógio fora da borda** | **Encontrado, alta:** hooks injetam `agora`, mas CLIs deixam `decide_pdc` e `decide_b3` chamar `time.time()` internamente. Trilha também lê relógio próprio. (`bin/disjuntor.py:1977`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1344`, `bin/disjuntor.py:903`, `bin/disjuntor.py:140`) |
| **TOCTOU / fotos diferentes** | **RELATO corpo/topo curado, alta:** recorte dos mesmos bytes validados. **Modelos ainda expostos, alta condicional:** censo lê `models`, filtro relê sem fotografia comum. O painel também abre a trilha quatro vezes, podendo misturar instantes. (`bin/disjuntor.py:709`, `bin/disjuntor.py:713`, `bin/disjuntor.py:1359`, `app/core/censo_escritas.py:105`, `bin/disjuntor.py:1232`, `bin/disjuntor.py:2055`, `bin/disjuntor.py:2058`) |
| **Selo sem mordida** | **Encontradas lacunas e mortes por exceção, alta**, detalhadas no item 4. Os selos estão ligados à régua por glob; LEI-AKITA também ao pre-push. Execução efetiva **[não verificado]**. (`bin/tests/test_disjuntor.sh:1667`, `bin/tests/test_padrao_de_cima.sh:375`, `bin/regua.sh:138`, `bin/pre-push.sh:38`) |
| **Ordem de trava** | **Alta:** wrapper toma slot → resposta → estado; triagem toma resposta compartilhada → estado. Não identifiquei inversão nesses percursos. O defeito concreto é publicar escopo fora da trava. (`bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/disjuntor.py:812`, `bin/disjuntor.py:844`, `bin/disjuntor.py:1421`) |
| **Fallback** | **Encontrado, alta:** escopo antigo pode liberar; RELATO ilegível perde `nao_sei`; falha de entidade vira vazio persistente. O fallback por programa, por sua vez, é explicitamente cadastrado. (`bin/disjuntor.py:1465`, `bin/disjuntor.py:1423`, `bin/disjuntor.py:1477`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1282`) |
| **argv com dado sensível** | **Alta quanto à forma:** novos gates passam somente `$OBRA`; pergunta e anexos chegam ao Codex pelo stdin. Porém `CODEX_OBRA` só é validado como não vazio, sem raspagem. Dado sensível efetivamente presente nele: **[não verificado]**. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:178`) |

## 31/10 17:56 · sessao 5c5a8d6e50e5 · V3-RETOMA-PERIODOS · verificador · origem=Code

**Veredito: o diff corrige os casos apresentados, mas não fecha a classe do aval.** Continua existindo pausa reconstruída pelas pontas; a origem na célula não é exigida; o censo deixa passar reconstruções simples; e há contraexemplo com HE100 noturna maior que a HE100 total. **Certeza alta.** (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/alimentacao.py:79`, `app/ponto/tests/test_v3_periodos_da_celula.py:218`, `app/ponto/motor_calculo_v2.py:391`)

Executei cálculos e mutantes em memória, carregando funções da cópia por AST. Execução da suíte Django, integração com banco, impacto em produção e comprovação criptográfica do hash: **[nao verificado]**. [nao verificado]

**1. Autoridade, escritores e leitores — certeza alta**

A autoridade registrada para **“que períodos e que intervalo teve este dia?”** é `ponto/juiz_batida.py::periodos_do_dia`. Quanto vale o par é outra pergunta, atribuída a `MotorBase.calcular_periodo`. O próprio JUIZES.md manda prevalecer o registro vivo. (`app/core/juizes.py:1143`, `app/core/juizes.py:1144`, `app/core/juizes.py:1145`, `app/docs/JUIZES.md:14`)

O v3 lê **o mesmo resultado do motor que o espelho lê**. Isso comprova compartilhamento da fonte intermediária, mas não comprova que todos os períodos vieram da célula: o motor conserva geometria alternativa quando não consegue usar o juiz da ata. (`app/ponto/services/espelho.py:432`, `app/ponto/calculador/alimentacao.py:79`, `app/ponto/motor_calculo_v2.py:480`, `app/ponto/motor_calculo_v2.py:523`, `app/ponto/motor_calculo_v2.py:582`)

| Campo/fluxo | Quem escreve | Quem lê |
|---|---|---|
| `resultado.periodos` e `periodos_ft` | Motor publica os períodos calculados. `app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:2430` | Espelho: `app/ponto/services/espelho.py:432`; leitor comum: `app/ponto/calculador/nucleo.py:393`; alimentação: `app/ponto/calculador/alimentacao.py:79`; releitura: `app/ponto/calculador/periodo.py:607` |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor` | Carimbo: `app/ponto/motor_calculo_v2.py:1778`; **também há escrita direta de `efetivo_saida`**: `app/ponto/motor_calculo_v2.py:1421` | Porta: `app/ponto/calculador/alimentacao.py:86`, `app/ponto/calculador/alimentacao.py:107`; releitura: `app/ponto/calculador/periodo.py:742`, `app/ponto/calculador/periodo.py:752` |
| `dia_da_jornada` | Motor expõe/mescla o mapa. `app/ponto/motor_calculo_v2.py:1011`, `app/ponto/motor_calculo_v2.py:2897` | `app/ponto/calculador/alimentacao.py:76`, `app/ponto/calculador/periodo.py:594` |
| `pares`: segmentos somáveis | Porta recorta conforme intervalos e `intra_descontada`. `app/ponto/calculador/alimentacao.py:134`, `app/ponto/calculador/alimentacao.py:140`, `app/ponto/calculador/alimentacao.py:164` | Núcleo chamado em `app/ponto/calculador/alimentacao.py:478`; soma em `app/ponto/calculador/nucleo.py:147` |
| `pares_por_dia`: períodos inteiros | `app/ponto/calculador/alimentacao.py:165` | Porta transmite em `app/ponto/calculador/alimentacao.py:560`; noturnas em `app/ponto/calculador/regras.py:885` |
| `intervalos_por_dia` | `app/ponto/calculador/alimentacao.py:166` | `app/ponto/calculador/alimentacao.py:482`, `app/ponto/calculador/alimentacao.py:562`; desconto em `app/ponto/calculador/regras.py:86` |
| `he_pontas_por_dia`: entrada, saída, HE50/100 antigas, intervalos, teto | Criação em `app/ponto/calculador/alimentacao.py:115`; teto pela soma dos segmentos em `app/ponto/calculador/alimentacao.py:169` | Transmissão em `app/ponto/calculador/alimentacao.py:561`; distribuição em `app/ponto/calculador/regras.py:858`; HE antiga serve ao diagnóstico em `app/ponto/calculador/regras.py:880` |
| `pausa_min` | **Gap entre segmentos**, em `app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152` | Porta transmite em `app/ponto/calculador/alimentacao.py:550`; intra e limite de HE usam em `app/ponto/calculador/regras.py:749`, `app/ponto/calculador/regras.py:760`, `app/ponto/calculador/regras.py:767` |
| `fora_por_dia` | Soma de `minutos_fora_janela`. `app/ponto/calculador/alimentacao.py:96` | Pontualidade recebe em `app/ponto/calculador/alimentacao.py:548` |
| Abertos e contadores | `app/ponto/calculador/alimentacao.py:90`, `app/ponto/calculador/alimentacao.py:117`, `app/ponto/calculador/alimentacao.py:163` | Retornam nos insumos em `app/ponto/calculador/alimentacao.py:325`; não constituem bloqueio das rubricas em `app/ponto/calculador/alimentacao.py:478` |
| Split HE50/100 noturna | Função comum chamada por período em `app/ponto/calculador/regras.py:864`; valores escritos em `app/ponto/calculador/regras.py:874`, `app/ponto/calculador/regras.py:876` | Fechamento entrega as rubricas à lavra em `app/ponto/services/fechamento.py:1256`; lavrador percorre em `app/ponto/services/dia_pago.py:320` |
| Transbordo e saldo | Toma, desconta e registra sobra em `app/ponto/calculador/regras.py:861`, `app/ponto/calculador/regras.py:869`, `app/ponto/calculador/regras.py:872` | Cada parcela alimenta `he_noturna` em `app/ponto/calculador/regras.py:864`; bloqueio específico por `he_sem_periodo_min`: **[nao verificado]** |

Portanto: **o v3 lê o resultado compartilhado com o espelho; não exige a autoridade da célula em todos os caminhos.** `periodos_do_resultado` exige atributos existentes e diferentes de `None`, mas aceita listas vazias e não verifica proveniência. (`app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:400`)

**2. Ramos restantes pelas pontas — certeza alta**

- **Produção: pausa entre segmentos.** O núcleo calcula `entrada_atual − saída_anterior`, inclusive entre períodos distintos. Para 00–01 + 06–08, reproduzi **180 minutos trabalhados, 300 de pausa e envelope 480**, embora os períodos não tenham intervalos julgados. Essa pausa chega à intra. (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/alimentacao.py:478`, `app/ponto/calculador/alimentacao.py:550`)
- **Ramo cru, quando `pares=None`.** Há agrupamento pelo gap da última batida, vão desde a primeira, cortes próprios, data da primeira batida, envelope última−primeira, pareamento sequencial e pausa entre pontas. A chamada de produção examinada passa lista e não entra nesse ramo. (`app/ponto/calculador/nucleo.py:188`, `app/ponto/calculador/nucleo.py:215`, `app/ponto/calculador/nucleo.py:237`, `app/ponto/calculador/nucleo.py:239`, `app/ponto/calculador/nucleo.py:245`, `app/ponto/calculador/nucleo.py:253`, `app/ponto/calculador/alimentacao.py:478`)
- **Decisão pelo meio do gap.** O ramo cru consulta envelopes pelas datas das pontas e decide se o vão é interno pelo ponto médio. (`app/ponto/calculador/nucleo.py:180`, `app/ponto/calculador/nucleo.py:183`)
- **Releitura em `periodo.py`.** Recorta o par pelos intervalos julgados e volta a obter pausa/envelope pelo núcleo. Aqui a geometria recebida é lavrada, mas a pausa continua recuperada pelos segmentos. (`app/ponto/calculador/periodo.py:304`, `app/ponto/calculador/periodo.py:355`, `app/ponto/calculador/periodo.py:357`)
- **Intervalo existente pode desaparecer de `pausa_min`.** Se `intra_descontada=0`, a porta mantém um segmento inteiro, mesmo havendo intervalo julgado. O noturno recebe o intervalo; a pausa escalar não o recebe. (`app/ponto/calculador/alimentacao.py:134`, `app/ponto/calculador/alimentacao.py:136`, `app/ponto/calculador/alimentacao.py:144`, `app/ponto/calculador/alimentacao.py:550`, `app/ponto/calculador/alimentacao.py:562`)

Não encontrei envelope executável por `min/max` de timestamps nos cinco módulos; `max(saidas)` aparece como relato de implementação anterior. A ausência além da cópia examinada é **[nao verificado]**. (`app/ponto/calculador/alimentacao.py:261`)

**3. O censo morde? Parcialmente — certeza alta**

Ele reconhece três grafias: vazio literal em keyword `intervalos`, subscript duplo diretamente nas keywords `entrada/saida` e vizinho `X[i+1][j]`. Não acompanha variáveis nem a semântica da reconstrução. (`app/ponto/tests/test_v3_periodos_da_celula.py:189`, `app/ponto/tests/test_v3_periodos_da_celula.py:195`, `app/ponto/tests/test_v3_periodos_da_celula.py:218`)

Executei o censo: os cinco arquivos retornam vazio. Estes mutantes também retornam vazio: [nao verificado]

```python
vazio = ()
he(entrada=p.e, saida=p.s, intervalos=vazio)

e, s = pts[0][0], pts[-1][1]
he(entrada=e, saida=s, intervalos=ivs)

he(entrada=min(ts), saida=max(ts), intervalos=ivs)
```

São falsos negativos decorrentes dos próprios predicados; a pausa existente com variável “anterior” já é uma testemunha concreta. A busca também usa `glob('*.py')`, sem recursão. (`app/ponto/tests/test_v3_periodos_da_celula.py:220`, `app/ponto/tests/test_v3_periodos_da_celula.py:222`, `app/ponto/tests/test_v3_periodos_da_celula.py:235`, `app/ponto/calculador/nucleo.py:149`)

**4. Regra, asserts e contraexemplos — certeza alta nos cálculos**

Os números do aval conferem **nas unidades declaradas pelos testes**: split em horas de relógio; hora reduzida é tratamento separado do adicional noturno. Validação jurídica externa da CLT/CCT: **[nao verificado]**. (`app/ponto/calculador/regras.py:88`, `app/ponto/calculador/regras.py:876`, `app/ponto/tests/test_v3_periodos_da_celula.py:12`)

| Caso | Resultado pela regra fornecida | Asserts |
|---|---|---|
| Feriado 21–05, pausa 23–00 | Trabalho 7h; noturno 22–23 + 00–05 = **6h HE100 noturna** | Conferem: `app/ponto/tests/test_v3_periodos_da_celula.py:111`, `app/ponto/tests/test_v3_periodos_da_celula.py:113` |
| Períodos 00–01 + 06–08 | **3h HE100**, das quais **1h noturna** | Conferem: `app/ponto/tests/test_v3_periodos_da_celula.py:169`, `app/ponto/tests/test_v3_periodos_da_celula.py:170` |
| 22–07, pausa 04h30–05h30, prorrogação e hora de 60min | **8h noturnas** | Confere: `app/ponto/tests/test_v3_periodos_da_celula.py:132` |

**Contraexemplos reproduzidos:**

- **Período único cruzando meia-noite, pausa inteiramente após 05h:** 22–07, pausa 05h30–06h30, prorrogação ativa, feriado em dobra, 480 trabalhados. `do_dia` retorna **HE100=8h e HE100 noturna=9h**. O período ganha prorrogação, mas a pausa é julgada isoladamente, começa após 05h e desconta zero. Viola a propriedade parte≤todo. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:145`, `app/ponto/motor_calculo_v2.py:391`, `app/ponto/calculador/regras.py:864`)
- **Pausa antes da janela:** 20–05, pausa 20h30–21h30, sem prorrogação: retorna **7h noturnas**, corretamente sem descontar a pausa diurna. (`app/ponto/motor_calculo_v2.py:99`, `app/ponto/motor_calculo_v2.py:143`)
- **HE somente na cauda prorrogada:** período 22–07, sem pausa, 120 minutos extras: `he_noturna` retorna **zero noturno**, porque recalcula a janela sobre 05–07, perdendo o contexto do período original. É outro limite da função compartilhada, preexistente ao diff. (`app/ponto/motor_calculo_v2.py:390`, `app/ponto/motor_calculo_v2.py:391`)
- **Sem períodos:** `pares=[]`, `he_pontas=[]`, 180 trabalhados em feriado retorna **HE100=3h, split=0h e `he_sem_periodo_min=180`**, sem recusar a decisão. Com `he_pontas=None`, declara ausência do split. (`app/ponto/calculador/regras.py:627`, `app/ponto/calculador/regras.py:845`, `app/ponto/calculador/regras.py:872`, `app/ponto/calculador/regras.py:876`)

Além disso, ausência de período **lavrado na célula** pode continuar como período geométrico do motor; o comportamento não satisfaz o “PAREI” literal do anexo `aval.txt:1754`. (`app/ponto/motor_calculo_v2.py:520`, `app/ponto/motor_calculo_v2.py:526`, `app/ponto/motor_calculo_v2.py:563`)

**5. Transbordo — implementado, mas teste não prova a ordem** [nao verificado]

Sim: ordena por **saída**, percorre em reverso, toma até o teto, chama o split com os intervalos próprios e desconta do restante. **Certeza alta.** (`app/ponto/calculador/regras.py:854`, `app/ponto/calculador/regras.py:858`, `app/ponto/calculador/regras.py:861`, `app/ponto/calculador/regras.py:864`, `app/ponto/calculador/regras.py:869`)

O C2 consome todos os 180 minutos disponíveis. Trocar `reversed(_pts)` por `_pts` preserva seu resultado: os dois períodos acabam preenchidos integralmente. Logo, esse mutante passa nos asserts do C2 e não é detectado pelo censo. **Certeza alta.** (`app/ponto/tests/test_v3_periodos_da_celula.py:158`, `app/ponto/tests/test_v3_periodos_da_celula.py:161`, `app/ponto/tests/test_v3_periodos_da_celula.py:170`, `app/ponto/tests/test_v3_periodos_da_celula.py:218`)

Um caso discriminante usa os mesmos períodos, mas apenas 120 minutos de HE: reversa ocupa 06–08 e dá zero noturno; direta ocupa 00–01 primeiro e dá 60 minutos noturnos. **Certeza alta por aplicação do algoritmo; execução desse caso: [nao verificado].** (`app/ponto/calculador/regras.py:858`, `app/ponto/calculador/regras.py:861`)

**6. Assinaturas e chamadores — certeza alta na análise estática**

O diff **não muda assinaturas formais** dessas funções; muda o contrato dos dados: `he_pontas` passa de quatro para seis componentes, `pares_por_dia` passa a conter períodos e surge a chave obrigatória `intervalos_por_dia`. Em `periodo.py`, muda apenas prosa. (`app/ponto/calculador/alimentacao.py:35`, `app/ponto/calculador/alimentacao.py:364`, `app/ponto/calculador/alimentacao.py:165`, `app/ponto/calculador/alimentacao.py:482`, `app/ponto/calculador/regras.py:572`, `app/ponto/calculador/regras.py:858`, `app/ponto/calculador/periodo.py:98`)

| API | Chamadores relevantes |
|---|---|
| `segmentos_do_motor` | Porta: `app/ponto/calculador/alimentacao.py:325`; teste: `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:157` |
| `rubricas_por_dia` | Fechamento: `app/ponto/services/fechamento.py:1239`; instrumento: `app/ponto/management/commands/diff_calculador.py:417`. Ambos montam insumos pela porta: `app/ponto/services/fechamento.py:1237`, `app/ponto/management/commands/diff_calculador.py:312` |
| `do_dia` em produção | `app/ponto/calculador/alimentacao.py:549` |
| Funções de `periodo.py` | Chamadas internas em `app/ponto/calculador/periodo.py:742`, `app/ponto/calculador/periodo.py:752`, `app/ponto/calculador/periodo.py:784`, `app/ponto/calculador/periodo.py:820`; bateria em `app/ponto/tests/test_o280_bateria_da_lei.py:3310` |

Não encontrei chamador fornecendo a tupla antiga: os testes explícitos foram migrados para seis componentes. Chamadores externos à cópia e compatibilidade integrada: **[nao verificado]**. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:304`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:41`, `app/ponto/tests/test_v3_periodos_da_celula.py:158`)

**7. Classes que já reprovaram obras**

| Classe | Resultado e certeza |
|---|---|
| **Segundo juiz duplicando registrado** | **Presente na classe da pergunta, alta:** o registro atribui períodos/intervalo ao juiz da batida, enquanto o núcleo decide pausas por gaps e mantém pareamento cru próprio. Não é autoridade nova registrada pelo diff; é derivação sobrevivente. `app/core/juizes.py:1144`, `app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/nucleo.py:245` |
| **Segundo escritor fora da porta** | **Presente, preexistente, alta:** `_carimbar_segmento` declara exclusividade, mas o bloqueio escreve `efetivo_saida` diretamente. Há também escrita de rubricas noturnas pela releitura fora de `_escrever_noturnas`; o selo desse escritor examina apenas o módulo do motor. `app/ponto/motor_calculo_v2.py:1773`, `app/ponto/motor_calculo_v2.py:1421`, `app/ponto/calculador/periodo.py:753`, `app/ponto/tests/test_noturno_sitio_unico.py:100` |
| **Relógio lido fora da borda** | Não identifiquei leitura da hora atual nos cinco módulos. Os `localtime` examinados recebem timestamp explícito. **Certeza alta nesse escopo**; sistema inteiro: **[nao verificado]**. `app/ponto/calculador/alimentacao.py:126`, `app/ponto/calculador/alimentacao.py:281` |
| **Derivação paralela de fonte única** | **Presente, alta:** pausa reconstruída; dois recortadores; preservação de geometria quando a ata não responde. `app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/alimentacao.py:140`, `app/ponto/calculador/periodo.py:304`, `app/ponto/motor_calculo_v2.py:582` |
| **Selo/RED sem mordida** | **Presente parcialmente, alta:** o censo pega suas três grafias, mas deixa passar os mutantes demonstrados; C2 não distingue ordem direta/reversa; propriedade parte≤todo cobre C2 e não a pausa após 05h. `app/ponto/tests/test_v3_periodos_da_celula.py:218`, `app/ponto/tests/test_v3_periodos_da_celula.py:161`, `app/ponto/tests/test_v3_periodos_da_celula.py:174` |
| **Ordem de trava** | Não demonstrada como defeito do diff. Ordem efetiva de locks, commit, deploy e aplicação em produção: **[nao verificado]**. | [nao verificado]
| **Pouso antes do pré-requisito** | **Pré-requisito não garantido, alta:** origem na célula não é exigida; sem dia/carimbo só conta e segue. O fechamento ainda captura falha da lavra e continua. Pouso efetivamente ocorrido: **[nao verificado]**. `app/ponto/motor_calculo_v2.py:526`, `app/ponto/calculador/alimentacao.py:82`, `app/ponto/calculador/alimentacao.py:94`, `app/ponto/services/fechamento.py:1265` |
| **Fallback** | **Presente, alta:** geometria sobrevive sem célula/veredito utilizável; template do chamador sobrevive à falha do vínculo; intervalo ausente vira vazio; falha ao decidir trabalho vira `True`. `app/ponto/motor_calculo_v2.py:526`, `app/ponto/motor_calculo_v2.py:582`, `app/ponto/calculador/alimentacao.py:520`, `app/ponto/calculador/alimentacao.py:531`, `app/ponto/calculador/alimentacao.py:107`, `app/ponto/calculador/alimentacao.py:475` |

## 31/10 17:55 · sessao 11dd9efae639 · - · - · origem=Ronald (tmux)

Certeza **alta** = leitura direta do código; efeitos de concorrência são **inferências estáticas**; execução dos selos e estado real de produção **[não verificado]**.

- **Vocabulário:** há uma regra única em `bin/padrao_vocab.py:34`, alimentada por **dois documentos**, `GLOSSARIO.md` e `PLANO-PADROES.md` (`bin/padrao_vocab.py:10`, `bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`). O selo usa essa função (`bin/tests/test_lei_akita.sh:85`, `bin/tests/test_lei_akita.sh:89`); o disjuntor também (`bin/disjuntor.py:1322`, `bin/disjuntor.py:1327`). **Não é garantida a mesma lista:** o selo lê `git show ALVO:arquivo` (`bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:77`); o disjuntor lê os arquivos do disco da raiz (`bin/disjuntor.py:1325`). Igualdade dos conteúdos em produção **[não verificado]**.
- **Cópias/regex:** existe um subconjunto literal de termos esperados no MORDE (`bin/tests/test_lei_akita.sh:163`) e fixtures próprias no selo PDC (`bin/tests/test_padrao_de_cima.sh:74`, `bin/tests/test_padrao_de_cima.sh:79`); não são outro produtor do vocabulário. O selo tem parser próprio do campo da mensagem (`bin/tests/test_lei_akita.sh:99`); o PDC tem parser próprio dos quatro campos do RELATO (`bin/disjuntor.py:1130`, `bin/disjuntor.py:1311`), mas ambos delegam a comparação do termo à regra compartilhada (`bin/tests/test_lei_akita.sh:110`, `bin/disjuntor.py:1315`). Não localizei outra extração de GLOSSARIO/PLANO na busca textual; ausência absoluta de implementação equivalente **[não verificado]**.
- **Quem escreve os documentos:** a regra autoriza padrão novo só por corte e proposta do Code no topo (`CLAUDE.md:65`, `CLAUDE.md:70`, `app/docs/PLANO-PADROES.md:49`). Os leitores novos não escrevem esses documentos (`bin/padrao_vocab.py:34`, `bin/disjuntor.py:1324`, `bin/tests/test_lei_akita.sh:76`). Identidade de quem efetivamente escreveu cada linha dos documentos **[não verificado]**.

**RELATO — escritores e leitores**

- As linhas de negativa saem de `relato_negacao` (`bin/disjuntor.py:198`), por `no_relato` (`bin/disjuntor.py:234`, `bin/disjuntor.py:236`). A porta toma flock do diretório (`app/core/esteira_vigia.py:844`, `app/core/esteira_vigia.py:848`) e grava temporário com replace (`app/core/esteira_vigia.py:879`, `app/core/esteira_vigia.py:884`). A inserção precede a primeira seção e preserva o topo (`app/core/esteira_vigia.py:869`, `app/core/esteira_vigia.py:872`).
- **Há escritores fora dessa porta**, explicitamente declarados como resíduo (`app/core/esteira_vigia.py:839`): `gerar_cortes` lê e regrava o arquivo inteiro (`bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`); `deploy_agendado` também (`bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`). **Inferência alta:** podem sobrescrever atualização concorrente da porta, pois não tomam seu flock nesses trechos.
- A regra de duas linhas úteis é `hook_stop_fila1._bloco_do_topo` (`bin/hook_stop_fila1.py:415`, `bin/hook_stop_fila1.py:429`, `bin/hook_stop_fila1.py:439`). PAREI e ESPERANDO usam `_linha_do_topo` (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:1104`). O B3 delega o recorte (`bin/disjuntor.py:676`, `bin/disjuntor.py:690`); o PDC usa corpo e topo dos mesmos bytes (`bin/disjuntor.py:1351`, `bin/disjuntor.py:1359`).
- Outros leitores: checador de prova no Stop (`bin/hook_stop_fila1.py:1176`); handoff lê todo o bloco até `##` com regra própria para PAREI/lei/! (`bin/handoff_sessao.sh:92`, `bin/handoff_sessao.sh:102`, `bin/handoff_sessao.sh:108`); publicação lê RELATO e BACKLOG (`bin/relato.sh:56`, `bin/relato.sh:104`, `bin/relato.sh:111`).
- Os hooks da cópia apontam para **caminhos absolutos da árvore viva**, Stop e PreToolUse (`.claude/settings.json:9`, `.claude/settings.json:68`); executar esta cópia não prova que aqueles hooks executem o hash auditado **[não verificado]**.

**BACKLOG / PROGRAMAS**

- `PROGRAMAS` é cadastro manual no código (`bin/disjuntor.py:1110`), consultado somente quando nenhuma entidade foi resolvida (`bin/disjuntor.py:1282`, `bin/disjuntor.py:1283`). O runtime PDC **não consulta BACKLOG** nesse caminho.
- O selo lê BACKLOG e valida só que cada chave do cadastro aparece e que o dict não está vazio (`bin/tests/test_padrao_de_cima.sh:328`, `bin/tests/test_padrao_de_cima.sh:333`, `bin/tests/test_padrao_de_cima.sh:334`). Não valida os **valores** obra→programa nem se toda obra que deveria estar cadastrada está presente.
- **Certeza alta:** apagar a linha do BACKLOG não muda o cadastro carregado no runtime (`bin/disjuntor.py:1110`, `bin/disjuntor.py:1282`); o selo reprova aquela divergência (`bin/tests/test_padrao_de_cima.sh:333`). Uma obra fora do cadastro e sem modelo resulta em `[]`, portanto não agrupa (`bin/disjuntor.py:1280`, `bin/disjuntor.py:1284`).
- BACKLOG é escrito pelo gerador, que preserva o bloco OBRAS escrito à mão (`bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:111`, `bin/gerar_backlog.py:119`) e publica por replace (`bin/gerar_backlog.py:138`, `bin/gerar_backlog.py:140`). Há outro escritor de célula de estado (`bin/backlog_estado.py:31`, `bin/backlog_estado.py:58`, `bin/backlog_estado.py:59`).
- Leitores fora do diff: Stop, por `_backlog_texto`, lista de paradas e travas (`bin/hook_stop_fila1.py:170`, `bin/hook_stop_fila1.py:317`, `bin/hook_stop_fila1.py:351`); resumo publicado por `gerar_backlog_topo` (`bin/gerar_backlog_topo.py:12`, `bin/gerar_backlog_topo.py:16`).

**Chave**

- O leitor central é `desligado`, usando `lexists`, inclusive symlink (`bin/disjuntor.py:151`, `bin/disjuntor.py:153`). O escritor autorizado é o terminal do Ronald segundo configuração (`bin/disjuntor_settings.json:3`); o PreToolUse tenta impedir escrita pelo Code (`bin/disjuntor.py:167`, `bin/disjuntor.py:171`). Existência atual da chave **[não verificado]**.
- Stop testa a chave antes de chamar B3 (`bin/hook_stop_fila1.py:1039`, `bin/hook_stop_fila1.py:1048`). Fixtures escrevem a chave nos selos (`bin/tests/test_padrao_de_cima.sh:303`, `bin/tests/test_padrao_de_cima.sh:313`).

**Riscos adicionais para consolidar**

1. **TOCTOU nos modelos — inferência alta:** o censo lê models (`bin/disjuntor.py:1216`, `app/core/censo_escritas.py:105`), `_so_modelos` relê os mesmos arquivos (`bin/disjuntor.py:1218`, `bin/disjuntor.py:1232`), e o arquivo citado pode ser lido depois (`bin/disjuntor.py:1258`). Não há uma foto comum equivalente à do RELATO.
2. **TOCTOU no vocabulário — inferência alta:** GLOSSARIO e PLANO são lidos sequencialmente sem snapshot conjunto (`bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`).
3. **Liberação prévia na primeira observação — certeza alta sobre comportamento:** se a primeira leitura já contém dois desenhos e uma linha válida de liberação, o mesmo ciclo dispara e libera (`bin/disjuntor.py:1374`, `bin/disjuntor.py:1381`, `bin/disjuntor.py:1397`, `bin/disjuntor.py:1401`). O mtime foi descartado (`bin/disjuntor.py:1351`). O teste “liberação antes” cobre apenas uma chamada prévia com um desenho, consumindo a linha antes do segundo (`bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:198`).
4. **Falha de resolver entidade vira ausência de sinal persistida:** qualquer exceção vira `ents=[]` e o sha fica registrado (`bin/disjuntor.py:1369`, `bin/disjuntor.py:1373`); só sha novo é resolvido (`bin/disjuntor.py:1366`). **Inferência alta:** reparar o censo não reclassifica o evento já gravado, mantendo verde para evento sem entidade.
5. **Juiz duplicado registrado:** não identifiquei duplicação de autoridade de negócio; o PDC chama o censo existente (`bin/disjuntor.py:1197`, `bin/disjuntor.py:1216`). A filtragem de subclasse é regra nova de reconhecimento (`bin/disjuntor.py:1222`, `bin/disjuntor.py:1242`), enquanto `censo_escritas.modelos` declara coletar toda classe de topo com bases (`app/core/censo_escritas.py:90`, `app/core/censo_escritas.py:109`). Ausência absoluta de juiz equivalente registrado **[não verificado]**.

## 31/10 17:55 · sessao 5bfc595274a8 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura estática; ocorrência em produção [não verificado].

| Item | Resultado e certeza | Evidência |
|---|---|---|
| **Verde com RELATO ilegível** | **Alta.** `decide_pdc` informa `nao_sei`, mas `estado_pdc` ignora esse atributo e retorna LIVRE quando a obra não aparece nas disparadas; estado válido vazio + RELATO ilegível libera. | `bin/disjuntor.py:1423`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1477` |
| **Escopo pode regredir** | **Alta, inferência de concorrência.** A trava do estado é liberada antes da gravação do escopo. A decide primeiro e pausa; B decide depois e publica escopo novo; A sobrescreve com escopo antigo. Se o estado ficar ilegível, a obra omitida pelo escopo antigo fica LIVRE. | `bin/disjuntor.py:293`, `bin/disjuntor.py:295`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1421`, `bin/disjuntor.py:1461`, `bin/disjuntor.py:1465` |
| **LIBERA anterior sem observação intermediária** | **Alta.** Primeira decisão com dois desenhos e LIBERA já no topo dispara e libera na mesma transição. Não há fotografia/mtime como no B3. O selo cobre LIBERA previamente consumida com um desenho; não cobre esse primeiro encontro. | `bin/disjuntor.py:1374`, `bin/disjuntor.py:1381`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1401`, `bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:197` |
| **Falha do censo vira entidade vazia permanente** | **Alta.** Exceção produz `ents=[]`, grava o evento e não tenta resolver novamente porque SHA já existe. Não agrupa e pode liberar por falta de entidade. | `bin/disjuntor.py:1366`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1373`, `bin/disjuntor.py:1298` |
| **BUG 6** | **Alta.** Curado na porta `b3_parada`. Outros chamadores de produção encontrados passam pelo desligamento do PreToolUse ou do Stop. As funções internas de julgamento continuam sem guarda própria, mas não encontrei outro chamador produtivo desprotegido. | `bin/disjuntor.py:1070`, `bin/disjuntor.py:1979`, `bin/hook_stop_fila1.py:1039`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:1889`, `bin/disjuntor.py:1930`, `bin/disjuntor.py:1944` |
| **RELATO corpo/topo** | **Alta.** Mesma foto no PDC. As duas leituras físicas são intencionais e comparadas; o topo recorta os bytes aceitos. | `bin/disjuntor.py:709`, `bin/disjuntor.py:713`, `bin/disjuntor.py:1351`, `bin/disjuntor.py:1359` |
| **TOCTOU dos modelos** | **Alta, condicional a alteração concorrente.** O censo lê os modelos e `_so_modelos` relê os mesmos arquivos sem foto comum. Pode combinar campos/classes da versão A com bases da versão B. | `app/core/censo_escritas.py:105`, `bin/disjuntor.py:1216`, `bin/disjuntor.py:1218`, `bin/disjuntor.py:1232` |
| **Relógio fora da borda** | **Alta.** Hooks fornecem `agora`; caminhos CLI deixam as decisões consultarem `time.time()` internamente. Trilha também consulta relógio próprio. | `bin/disjuntor.py:1977`, `bin/disjuntor.py:1949`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1344`, `bin/disjuntor.py:903`, `bin/disjuntor.py:140` |
| **Ordem das travas** | **Alta.** Wrapper: slot → resposta → estado. Triagem: resposta compartilhada → estado. Não encontrei inversão nesse percurso; escopo fora da trava é o problema concreto. | `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/disjuntor.py:812`, `bin/disjuntor.py:844`, `bin/disjuntor.py:260` |
| **Codex rc 75** | **Alta.** Identifica limite/indisponibilidade em erro, registra `limite.log`, manda seguir por sonnet e retorna 75. Não emite sidecar válido dessa chamada; automatização efetiva da continuação [não verificado]. | `bin/codex_pergunta.sh:186`, `bin/codex_pergunta.sh:191`, `bin/codex_pergunta.sh:192`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:201` |
| **argv sensível** | **Alta quanto à forma.** Novas chamadas passam somente obra em argv; pergunta/anexos seguem pelo stdin. `CODEX_OBRA` só é validado como não vazio, portanto conteúdo sensível nesse identificador não é impedido; presença efetiva [não verificado]. | `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:178` |

GLOSSÁRIO ausente impede LIBERA quando o topo tenta usá-lo; sem candidata a LIBERA, o vocabulário nem é consultado, portanto ausência não é recusa global. RELATO legível vazio produz topo `[]`, não erro: não libera disparadas, mas também não cria eventos. BACKLOG não é lido pelo resolvedor produtivo: ele usa `PROGRAMAS` literal; o selo só verifica presença das chaves. **Certeza alta.** `bin/disjuntor.py:1384`, `bin/disjuntor.py:1388`, `bin/disjuntor.py:1390`, `bin/hook_stop_fila1.py:443`, `bin/disjuntor.py:1110`, `bin/disjuntor.py:1282`, `bin/tests/test_padrao_de_cima.sh:321`.

Os hooks configurados apontam para a árvore viva absoluta; execução/configuração efetiva de produção [não verificado]. `.claude/settings.json:9`, `.claude/settings.json:68`.

## 31/10 17:55 · sessao 2358df259c1e · - · - · origem=Ronald (tmux)

Certeza **alta por leitura estática**; não rodei os selos porque criam árvores temporárias e arquivos (`bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:36`, `bin/tests/test_lei_akita.sh:168`).

- **PADRAO-DE-CIMA tem mordida útil:** exige recusas e liberações pelas portas reais; possui controles sem mutação e mutantes para gate, agrupamento, vocabulário, janela e desligamento (`bin/tests/test_padrao_de_cima.sh:110`, `bin/tests/test_padrao_de_cima.sh:285`, `bin/tests/test_padrao_de_cima.sh:377`, `bin/tests/test_padrao_de_cima.sh:380`).
- **PROGRAMAS é selo parcial:** verifica somente chaves existentes no BACKLOG e lista não vazia; não verifica valores nem completude (`bin/tests/test_padrao_de_cima.sh:332`). Remover a chave `O261-E-CELULA-20-UM-DONO`, ou trocar somente seu destino, escapa por inspeção: os cenários comportamentais usam apenas duas E4 (`bin/disjuntor.py:1113`, `bin/tests/test_padrao_de_cima.sh:157`).
- **Atomicidade do escopo não está demonstrada:** caso verifica conteúdo final; não observa escrita concorrente. Substituir `os.replace` por escrita direta pode passar (`bin/tests/test_padrao_de_cima.sh:118`, `bin/disjuntor.py:1437`). Certeza **média sobre sobrevivência sem executar**.
- **Faltam casos de ausência:** fixtures sempre fornecem GLOSSARIO/PLANO; RELATO ausente só é testado com estado já corrompido (`bin/tests/test_padrao_de_cima.sh:74`, `bin/tests/test_padrao_de_cima.sh:250`, `bin/tests/test_padrao_de_cima.sh:257`). Isso deixa sem cobertura o descarte de `d.nao_sei` com estado legível (`bin/disjuntor.py:1423`, `bin/disjuntor.py:1474`).
- **Mutante inválido no test_disjuntor:** `B3-foto-parcial-engolida` injeta `H._bloco_do_topo()` numa função sem `H` local/global; os imports existentes são locais a outras funções (`bin/tests/test_disjuntor.sh:1663`, `bin/disjuntor.py:669`, `bin/disjuntor.py:688`, `bin/disjuntor.py:702`). Reprodução isolada em memória produziu `NameError`; harness conta qualquer falha como morte (`bin/tests/test_disjuntor.sh:1491`). Certeza **alta, confirmada em memória**.
- **Outro mutante morre por exceção:** remover a guarda de entidade não disparada conduz a `disp[ent]` inexistente no primeiro cenário de LIBERA antecipada (`bin/tests/test_padrao_de_cima.sh:195`, `bin/tests/test_padrao_de_cima.sh:397`, `bin/disjuntor.py:1402`). Harness também aceita esse crash como mordida (`bin/tests/test_padrao_de_cima.sh:375`). Certeza **alta por fluxo estático**.
- **LEI-AKITA morde lista vazia e inversão do predicado:** exige termos conhecidos, casos positivos e negativos e presença do marcador final (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:298`, `bin/tests/test_lei_akita.sh:302`). Porém um fallback em `VocabIlegivel` não é exercitado pelos casos: nenhuma fonte é removida; trocar a exceção por vocabulário parcial poderia sobreviver em condições saudáveis (`bin/tests/test_lei_akita.sh:87`, `bin/tests/test_lei_akita.sh:172`).
- **Não são selos órfãos no código:** a régua percorre `tests/test_*.sh`; LEI-AKITA tem chamada explícita no pre-push (`bin/regua.sh:138`, `bin/pre-push.sh:38`). Execução efetiva em produção: **[não verificado]**.

## 31/10 17:54 · sessao 7bc4e88cb9e1 · - · - · origem=Ronald (tmux)

**Achados certos, por leitura estática e execução em memória de funções da cópia:**

- **Sobra pausa reconstruída pelas pontas no caminho de produção.** `minutos_do_oraculo` recebe os segmentos em `app/ponto/calculador/alimentacao.py:478`; calcula pausa entre fim anterior e próxima entrada em `app/ponto/calculador/nucleo.py:149`; essa pausa alimenta as rubricas em `app/ponto/calculador/alimentacao.py:550`. Para períodos 00–01 + 06–08, executei a função real: 180 minutos trabalhados, **300 minutos de pausa** e envelope 480; esses 300 minutos não vêm de intervalos julgados. Certeza alta; origens: `app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/nucleo.py:153`.

- **Existe também o ramo completo de reconstrução por batidas cruas.** Quando `pares is None`, o núcleo identifica turnos pelo gap da última batida (`app/ponto/calculador/nucleo.py:188`), pelo vão desde a primeira (`app/ponto/calculador/nucleo.py:215`), aplica cortes próprios (`app/ponto/calculador/nucleo.py:216`), toma a data da primeira (`app/ponto/calculador/nucleo.py:237`), envelope última menos primeira (`app/ponto/calculador/nucleo.py:239`), pareia sequencialmente (`app/ponto/calculador/nucleo.py:245`) e calcula pausas entre pontas (`app/ponto/calculador/nucleo.py:253`). A porta de produção examinada passa lista, portanto esse ramo não é acionado por essa chamada específica (`app/ponto/calculador/alimentacao.py:478`). Certeza alta.

- **O ramo cru ainda decide se um gap pertence ao envelope pelo ponto médio e pelas datas de suas pontas.** `app/ponto/calculador/nucleo.py:180`, `app/ponto/calculador/nucleo.py:183`. Certeza alta.

- **`periodo.py` também volta a calcular pausa/envelope pelos segmentos.** Recorta o período com intervalos julgados (`app/ponto/calculador/periodo.py:304`), chama o núcleo (`app/ponto/calculador/periodo.py:355`) e lê a pausa e o envelope calculados (`app/ponto/calculador/periodo.py:357`, `app/ponto/calculador/periodo.py:358`). Diferentemente do ramo cru, recebe a geometria lavrada; ainda há cálculo da pausa pelos segmentos. Certeza alta.

- **A pausa muda conforme o desconto do motor.** A porta só recorta intervalos quando `intra_descontada > 0` (`app/ponto/calculador/alimentacao.py:134`, `app/ponto/calculador/alimentacao.py:136`). Com intervalo julgado presente e desconto zero, o núcleo recebe um segmento inteiro, portanto não vê a pausa, enquanto `do_dia` recebe os intervalos julgados separadamente (`app/ponto/calculador/alimentacao.py:144`, `app/ponto/calculador/alimentacao.py:550`, `app/ponto/calculador/alimentacao.py:562`). Certeza alta sobre o fluxo.

**Censo AST: morde três grafias, não a classe do defeito.**

Executei as funções AST do próprio teste em memória, sem importar Django: os cinco módulos retornam conjunto vazio. Os seguintes mutantes também retornam conjunto vazio, por limitações explícitas de `app/ponto/tests/test_v3_periodos_da_celula.py:189`, `app/ponto/tests/test_v3_periodos_da_celula.py:195`, `app/ponto/tests/test_v3_periodos_da_celula.py:220`, `app/ponto/tests/test_v3_periodos_da_celula.py:222`:

```python
vazio = ()
he(entrada=p.e, saida=p.s, intervalos=vazio)
```

```python
e = pts[0][0]
s = pts[-1][1]
he(entrada=e, saida=s, intervalos=ivs)
```

```python
he(entrada=min(ts), saida=max(ts), intervalos=ivs)
```

```python
he(pts[0][0], pts[-1][1], ivs)
```

A pausa calculada com variável `anterior` também passa; o próprio código existente constitui testemunha desse falso negativo (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152`). Além disso, o universo usa `glob('*.py')`, sem recursão (`app/ponto/tests/test_v3_periodos_da_celula.py:235`). Certeza alta.

**Classes do item 7:**

- **Derivação paralela/fonte única:** ocorrência concreta na pausa da produção acima; AST verde apesar dela (`app/ponto/calculador/nucleo.py:152`, `app/ponto/calculador/alimentacao.py:550`, `app/ponto/tests/test_v3_periodos_da_celula.py:220`). Certeza alta.
- **Fallback:** mantém template/cadastro/folga do chamador quando o juiz não responde ou levanta (`app/ponto/calculador/alimentacao.py:520`, `app/ponto/calculador/alimentacao.py:523`, `app/ponto/calculador/alimentacao.py:531`); ausência de vínculo e exceção também viram `dia_de_trabalho=True` (`app/ponto/calculador/alimentacao.py:470`, `app/ponto/calculador/alimentacao.py:475`). Certeza alta.
- **Ausência de intervalo vira vazio:** `getattr(..., ()) or ()` em `app/ponto/calculador/alimentacao.py:107`. Não distingue “motor declarou nenhum intervalo” de “campo ausente”. Certeza alta.
- **Pouso antes do pré-requisito/PAREI:** período sem dia ou sem carimbo apenas incrementa contador e segue (`app/ponto/calculador/alimentacao.py:82`, `app/ponto/calculador/alimentacao.py:87`). Já ausência dos campos de períodos efetivamente levanta `ResultadoSemPeriodos` (`app/ponto/calculador/nucleo.py:393`). A equivalência disso com “período que a célula não guarda” precisa ser estabelecida pelo pai com a fonte da célula; [nao verificado].
- **Segundo escritor:** em `calculador`, não encontrei escrita de `efetivo_entrada`, `efetivo_saida` ou `intervalos_do_motor`; há escrita de rubricas nos objetos pela releitura (`app/ponto/calculador/periodo.py:743`, `app/ponto/calculador/periodo.py:753`, `app/ponto/calculador/periodo.py:839`). A irregularidade como “fora da porta” depende de seu contrato; [nao verificado].
- **Relógio fora da borda/ordem de trava:** não encontrei chamadas de relógio atual, transação ou lock nos cinco módulos; as chamadas `localtime` recebem instante explícito (`app/ponto/calculador/alimentacao.py:126`, `app/ponto/calculador/alimentacao.py:281`). Ordem de travas dos chamadores externos: [nao verificado].
- **Segundo juiz registrado:** a pausa reconstruída está provada, mas seu enquadramento contra pergunta específica registrada em `core/juizes.py` depende da inspeção do registro; [nao verificado].

## 31/10 17:54 · sessao 8d16ad74d24b · - · - · origem=Ronald (tmux)

**Autoridade e origem — certeza alta.** `JUIZES.md` é lookup e manda consultar o registro vivo (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`). Quem responde “que períodos e que intervalo teve este dia?” é `ponto/juiz_batida.py::periodos_do_dia`; quem responde quanto vale o PAR é `MotorBase.calcular_periodo` (`app/core/juizes.py:1143`, `app/core/juizes.py:1144`, `app/core/juizes.py:1145`). `periodos_do_resultado` é leitor dos objetos publicados pelo motor, e `_carimbar_segmento` copia a geometria efetivamente usada; nenhum deles substitui o juiz do dia (`app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:400`, `app/ponto/motor_calculo_v2.py:1772`, `app/ponto/motor_calculo_v2.py:1778`).

**O v3 lê a mesma fonte do espelho, mas a proveniência da célula é condicional — certeza alta.** Espelho percorre `resultado_v2.periodos + periodos_ft` (`app/ponto/services/espelho.py:432`). A porta v3 percorre ambos via `periodos_do_resultado`, lendo carimbo efetivo e intervalos (`app/ponto/calculador/alimentacao.py:79`, `app/ponto/calculador/alimentacao.py:86`, `app/ponto/calculador/alimentacao.py:107`). O motor começa com geometria e só a substitui pelo veredito da célula quando as guardas permitem (`app/ponto/motor_calculo_v2.py:480`, `app/ponto/motor_calculo_v2.py:566`, `app/ponto/motor_calculo_v2.py:635`, `app/ponto/motor_calculo_v2.py:638`).

**A condição “período que a célula não guarda = PAREI” não está garantida por essa cadeia — certeza alta sobre o código; aplicação jurídica do aval cabe ao principal.** Sem células alimentadas, retorna o trio geométrico; turno partido também retorna esse trio; sem célula para o dia, juiz levantando, ata desalinhada/aberta ou luz sem batida, conserva a geometria anterior (`app/ponto/motor_calculo_v2.py:509`, `app/ponto/motor_calculo_v2.py:520`, `app/ponto/motor_calculo_v2.py:523`, `app/ponto/motor_calculo_v2.py:526`, `app/ponto/motor_calculo_v2.py:563`, `app/ponto/motor_calculo_v2.py:567`, `app/ponto/motor_calculo_v2.py:582`, `app/ponto/motor_calculo_v2.py:591`). A porta só distingue atributo de resultado ausente/`None` de lista vazia; não verifica origem na célula (`app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:397`). Sem mapa de jornada ou sem carimbo fechado, conta e pula em vez de parar (`app/ponto/calculador/alimentacao.py:81`, `app/ponto/calculador/alimentacao.py:94`).

| Campo/fluxo | Quem escreve | Quem lê |
|---|---|---|
| `resultado.periodos` / `periodos_ft` | Criação por `calcular_periodo`, publicação nas listas (`app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:2430`, `app/ponto/motor_calculo_v2.py:2432`) | Espelho (`app/ponto/services/espelho.py:432`); porta única (`app/ponto/calculador/nucleo.py:400`); v3 alimentação (`app/ponto/calculador/alimentacao.py:79`); releitura (`app/ponto/calculador/periodo.py:607`) |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor` | Carimbo (`app/ponto/motor_calculo_v2.py:1778`, `app/ponto/motor_calculo_v2.py:1779`, `app/ponto/motor_calculo_v2.py:1780`); **também `efetivo_saida` no bloqueio** (`app/ponto/motor_calculo_v2.py:1421`) | Porta (`app/ponto/calculador/alimentacao.py:86`, `app/ponto/calculador/alimentacao.py:107`); releitura (`app/ponto/calculador/periodo.py:645`, `app/ponto/calculador/periodo.py:742`, `app/ponto/calculador/periodo.py:752`); motor HE noturna lê intervalos (`app/ponto/motor_calculo_v2.py:956`) |
| `dia_da_jornada` | Exposição/mescla do mapa do motor (`app/ponto/motor_calculo_v2.py:1011`, `app/ponto/motor_calculo_v2.py:1013`, `app/ponto/motor_calculo_v2.py:2897`) | Porta (`app/ponto/calculador/alimentacao.py:76`, `app/ponto/calculador/alimentacao.py:81`); releitura (`app/ponto/calculador/periodo.py:594`) |
| `pares` segmentos | Porta (`app/ponto/calculador/alimentacao.py:140`, `app/ponto/calculador/alimentacao.py:144`, `app/ponto/calculador/alimentacao.py:164`) | Núcleo, chamado pela porta (`app/ponto/calculador/alimentacao.py:478`); soma (`app/ponto/calculador/nucleo.py:147`) |
| `pares_por_dia` períodos | Porta (`app/ponto/calculador/alimentacao.py:165`) | `rubricas_por_dia` (`app/ponto/calculador/alimentacao.py:481`, `app/ponto/calculador/alimentacao.py:560`); noturnas (`app/ponto/calculador/regras.py:885`, `app/ponto/calculador/regras.py:887`) |
| `intervalos_por_dia` | Porta (`app/ponto/calculador/alimentacao.py:166`) | Porta de rubricas (`app/ponto/calculador/alimentacao.py:482`, `app/ponto/calculador/alimentacao.py:562`); cálculo noturno (`app/ponto/calculador/regras.py:887`, `app/ponto/calculador/regras.py:86`) |
| `he_pontas_por_dia`, agora 6 componentes | Porta cria geometria+intervalos e atualiza teto pela soma dos segmentos (`app/ponto/calculador/alimentacao.py:115`, `app/ponto/calculador/alimentacao.py:168`, `app/ponto/calculador/alimentacao.py:169`) | Porta (`app/ponto/calculador/alimentacao.py:488`, `app/ponto/calculador/alimentacao.py:561`); split (`app/ponto/calculador/regras.py:854`, `app/ponto/calculador/regras.py:858`) |
| `pausa_min` | **Continua derivada do gap entre segmentos**, em `Veredito.pausas` (`app/ponto/calculador/nucleo.py:149`, `app/ponto/calculador/nucleo.py:152`) | Porta transmite (`app/ponto/calculador/alimentacao.py:550`); regras lê na base do piso (`app/ponto/calculador/regras.py:749`) |
| HE100 noturna e HE50 noturna | `he_noturna` emprestada do motor é chamada por período; valores do dia escritos pelo v3 (`app/ponto/calculador/regras.py:846`, `app/ponto/calculador/regras.py:864`, `app/ponto/calculador/regras.py:874`, `app/ponto/calculador/regras.py:876`) | Retorno de rubricas armazenado em `out[k]` (`app/ponto/calculador/alimentacao.py:549`); demais consumidores completos: [nao verificado] |
| Transbordo | Ordenação pela saída, iteração reversa, teto e decremento (`app/ponto/calculador/regras.py:854`, `app/ponto/calculador/regras.py:858`, `app/ponto/calculador/regras.py:861`, `app/ponto/calculador/regras.py:869`) | Cada parcela vai a `he_noturna` com intervalos próprios (`app/ponto/calculador/regras.py:864`, `app/ponto/calculador/regras.py:866`) |
| `he_sem_periodo_min` | Detalhe recebe saldo que não coube (`app/ponto/calculador/regras.py:872`) | Consumidor específico desse detalhe: [nao verificado] |

**Segundo escritor — certeza alta, preexistente e fora do diff.** `_carimbar_segmento` se declara escritor único dos três campos (`app/ponto/motor_calculo_v2.py:1773`), porém `_dia_sem_previsto_fora_da_janela` atribui diretamente `efetivo_saida = efetivo_entrada` (`app/ponto/motor_calculo_v2.py:1420`, `app/ponto/motor_calculo_v2.py:1421`), chamado após o cálculo do período (`app/ponto/motor_calculo_v2.py:2399`). É exceção de bloqueio explicitamente documentada; não prova, por si, erro monetário novo (`app/ponto/motor_calculo_v2.py:1415`).

**Derivação paralela/pausa — certeza alta.** `periodo.py` possui outro recortador de períodos, `segmentos_do_par`, além da porta; depois recupera pausa pelos gaps do núcleo (`app/ponto/calculador/periodo.py:290`, `app/ponto/calculador/periodo.py:303`, `app/ponto/calculador/periodo.py:355`, `app/ponto/calculador/periodo.py:357`). A releitura reescreve minutos trabalhados, intra e noturnas nos próprios objetos (`app/ponto/calculador/periodo.py:743`, `app/ponto/calculador/periodo.py:744`, `app/ponto/calculador/periodo.py:753`, `app/ponto/calculador/periodo.py:754`).

**Fallback — certeza alta.** Além da cadeia geométrica acima, se o vínculo do dia não existe ou levanta, `rubricas_por_dia` conserva o template declarado pelo chamador (`app/ponto/calculador/alimentacao.py:520`, `app/ponto/calculador/alimentacao.py:523`, `app/ponto/calculador/alimentacao.py:531`, `app/ponto/calculador/alimentacao.py:538`). O comentário chamar isso de “não é fallback” não altera o fluxo observado (`app/ponto/calculador/alimentacao.py:517`).

## 31/10 17:54 · sessao 48385a33de2a · - · - · origem=Ronald (tmux)

**5. Transbordo — certeza alta.** O código ordena períodos pela saída em `app/ponto/calculador/regras.py:854`, percorre `reversed(_pts)` em `:858`, limita cada fatia pelo teto em `:861`, chama `he_noturna` com entrada, saída e intervalos daquele período em `:864` e desconta a fatia do restante em `:869`. A sobra sem período fica em `he_sem_periodo_min`, `:872`.

**O teste C2 não prova a ordem — certeza alta.** Ele distribui 180 minutos em períodos com capacidade total exatamente 180; tanto ordem direta quanto reversa consomem os dois por inteiro e produzem 1 hora noturna. Retirar `reversed` deixa os asserts iguais (`app/ponto/tests/test_v3_periodos_da_celula.py:158`, `:161`, `:169`, `:170`). Contraexemplo discriminante: mesmos períodos 00–01 e 06–08, mas HE total de 120 minutos: ordem reversa ocupa 06–08, com zero noturno; direta ocupa 00–01 e mais uma hora diurna, com 60 minutos noturnos (`app/ponto/calculador/regras.py:858`, `:861`, `:864`). Execução desse contraexemplo: [nao verificado].

**6. Assinaturas/chamadores — certeza alta na análise estática.** O diff fornecido não altera assinaturas formais: `segmentos_do_motor` continua com `resultado` (`app/ponto/calculador/alimentacao.py:35`); `rubricas_por_dia`, com os mesmos parâmetros (`:364`); `do_dia`, idem (`app/ponto/calculador/regras.py:572`); em `periodo.py`, o diff só altera comentários (`app/ponto/calculador/periodo.py:98`, `:505`). A mudança incompatível é **o contrato dos dados**: nova chave obrigatória `intervalos_por_dia` (`app/ponto/calculador/alimentacao.py:482`), `pares_por_dia` contendo períodos (`:165`) e `he_pontas` passando de quatro para seis elementos, desempacotados em `app/ponto/calculador/regras.py:858`.

Chamadores encontrados:

| API | Chamadores |
|---|---|
| `segmentos_do_motor` | Porta `app/ponto/calculador/alimentacao.py:325`; teste `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:157`. |
| `rubricas_por_dia` em produção/instrumento | `app/ponto/services/fechamento.py:1239`, com insumos criados em `:1237`; `app/ponto/management/commands/diff_calculador.py:417`, com insumos criados em `:312`. |
| `rubricas_por_dia` em testes | `test_s5b_vinculo_do_dia_efeito.py:112`; `test_he100_dia_sem_previsto.py:272`; `test_v3_dobra_do_ciclo_do_dia.py:94`, `:178`; `test_v3_dobra_noturna_e_cct_do_dia.py:172`, `:361`; `test_o280_bateria_da_lei.py:2776`, `:2861`; `test_v3_periodos_da_celula.py:97`, `:101`; `test_lote_de_celulas_nao_troca_de_colaborador.py:121`, todos sob `app/ponto/tests/`. | [nao verificado]
| `regras.do_dia` em produção | `app/ponto/calculador/alimentacao.py:549`. Os demais usos encontrados são testes; entradas omitidas continuam opcionais e geram `SEM_ENTRADA`, conforme `app/ponto/calculador/regras.py:583`, `:610`, `:627`. |
| `periodo.py` | `reler` tem chamadas na bateria O280, por exemplo `app/ponto/tests/test_o280_bateria_da_lei.py:3310`, `:3712`, `:4051`, `:4382`, `:5181`. Chamadas internas: `periodo_do_dia` em `app/ponto/calculador/periodo.py:742`, `noturno_no_par` em `:752`, `pontualidade_no_dia` em `:784`, `excesso_do_dia` em `:820`. |

**Não encontrei chamada com a tupla antiga de quatro elementos — certeza alta no código estático pesquisado.** As chamadas explícitas com `he_pontas` são a porta (`app/ponto/calculador/alimentacao.py:561`) e testes: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:308`, cujas tuplas têm seis elementos em `:304`; `test_v3_dobra_noturna_e_cct_do_dia.py:41`, `:59`, `:102`, `:143`; `test_v3_periodos_da_celula.py:164`, cujas tuplas estão em `:158`. Chamadores externos à cópia: [nao verificado].

Outro leitor de `pares_por_dia`: o comando atribui esse mapa em `app/ponto/management/commands/diff_calculador.py:318`; sua chamada de rubricas usa integralmente os novos insumos em `:418`, sem remontar o contrato antigo. Compatibilidade executada com Django/DB: [nao verificado].

## 31/10 17:44 · sessao 7e00fa505aa6 · TRAVA-COBERTURA · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 17:42 · sessao bc4f2203bbdb · - · - · origem=Ronald (tmux)

Censo de reentrância: nos caminhos versionados examinados, nenhum ancestral identificado segura `/tmp/juliani_db_test.lock` antes do disparo; os ancestrais prendem arquivos distintos, e a trava global protege apenas o comando filho. Ancestrais externos e processos de produção são **[não verificado]**. (`bin/trava_teste.sh:35`; `bin/trava_teste.sh:42`)

| Chamador | Cadeia e lock ancestral identificado |
|---|---|
| `suite` | `suite → exec trava → docker`; não adquire flock antes do `exec`. (`bin/suite.sh:96`) |
| `regua` | `regua → trava → docker`; o próprio script segura `/tmp/regua.lock`, distinto do global. (`bin/regua.sh:16`; `bin/regua.sh:97`; `bin/regua.sh:165`) |
| `pre-push` | `git push → hook wrapper → bin/pre-push → _teste_irmao → trava`; prende `/tmp/regua.lock`, ou aceita `REGUA_TRAVA_HERDADA=1`. (`bin/hooks.sh:56`; `bin/pre-push.sh:80`; `bin/pre-push.sh:86`; `bin/pre-push.sh:142`) |
| `vigia_arvore` | Prende `/tmp/vigia_arvore.lock`; chama sequencialmente `suite → trava`, depois outra trava na confirmação viva. (`bin/vigia_arvore.sh:24`; `bin/vigia_arvore.sh:83`; `bin/vigia_arvore.sh:106`; `bin/vigia_arvore.sh:131`) |
| `regua_relogio` | Chama diretamente `trava → docker`; seu corpo anterior prepara diretório e env-file, sem flock próprio. (`bin/regua_relogio.sh:31`; `bin/regua_relogio.sh:46`; `bin/regua_relogio.sh:51`) |
| `molde_fatia` | `esteira → rodar → teste → trava`; ancestral segura `/tmp/esteira___FATIA__.lock`, distinto. (`bin/molde_fatia/esteira.sh:7`; `bin/molde_fatia/esteira.sh:15`; `bin/molde_fatia/rodar.sh:40`) |
| `molde_fatia MAPA` | Mesmo ancestral; MAPA ainda executa docker diretamente e termina antes da função `teste`. (`bin/molde_fatia/rodar.sh:28`; `bin/molde_fatia/rodar.sh:33`) |
| `molde_autocura` | `esteira → rodar → teste → docker`; ancestral segura `/tmp/esteira___FATIA__.lock`; a cadeia só começa após `rodar` retornar. (`bin/molde_autocura/esteira.sh:6`; `bin/molde_autocura/esteira.sh:14`; `bin/molde_autocura/esteira.sh:23`; `bin/molde_autocura/rodar.sh:20`) |
| `isolamento` | `cadeia → regua → trava → docker`, retorna; então `cadeia → isolar_ou_parar → _iso_roda`. A cadeia ainda segura `/tmp/regua.lock`, mas a trava do teste anterior já retornou. (`bin/molde_fatia/cadeia.sh:14`; `bin/molde_fatia/cadeia.sh:51`; `bin/molde_fatia/cadeia.sh:55`; `bin/isolamento.sh:45`) |
| `placar_code` | `_cred` abre subshell, gera env-file e dispara docker diretamente. Não há aquisição global nesse sítio. (`bin/placar_code.sh:121`; `bin/placar_code.sh:122`) |
| `esteira_vigia` | `vigia_esteira → Python → passada → _isolar/_rodar_testes`, sob `/tmp/vigia_esteira.lock`, distinto. (`bin/vigia_esteira.sh:9`; `bin/vigia_esteira.sh:11`; `app/core/esteira_vigia.py:1473`; `app/core/esteira_vigia.py:740`) |
| Python pelo integrador | `integrador → integrador_lote.rodar → _suite → regua → trava`; integrador segura `/tmp/integrador.lock`, os locks da árvore/raia e `/tmp/regua.lock`; chamadas Python adicionais acontecem fora do comando protegido da régua. (`bin/integrador.sh:9`; `bin/integrador.sh:32`; `bin/integrador.sh:51`; `bin/integrador.sh:52`; `app/core/integrador_lote.py:639`; `app/core/integrador_lote.py:375`) |

Há **sete sítios** chamadores de `_rodar_testes`: quatro internos (`_isolar`, `auto_revert`, `curar_por_relogio`, `autocura`) e três no integrador (`_suite`, `_isolamento`, `_conferir_quarentena`). (`app/core/esteira_vigia.py:740`; `app/core/esteira_vigia.py:1269`; `app/core/esteira_vigia.py:1319`; `app/core/esteira_vigia.py:1406`; `app/core/integrador_lote.py:373`; `app/core/integrador_lote.py:403`; `app/core/integrador_lote.py:769`)

Perda de rc e contraexemplos:

- `suite` usa `exec`, preservando o resultado da trava; `_teste_irmao` guarda `_rc`, distingue 75 e devolve `_rc`. (`bin/suite.sh:96`; `bin/pre-push.sh:141`; `bin/pre-push.sh:149`; `bin/pre-push.sh:157`)
- `regua` recebe diretamente o status, mas o descarta e decide por texto `Ran`/`OK`; um 75 vira “a suite nem rodou” e rc final 1. (`bin/regua.sh:165`; `bin/regua.sh:170`; `bin/regua.sh:178`; `bin/regua.sh:205`)
- `regua_relogio` continua após o pipeline e reinicia `RC=0`; ausência de teste pode terminar OK se a lista conhecida estiver vazia. (`bin/regua_relogio.sh:51`; `bin/regua_relogio.sh:58`; `bin/regua_relogio.sh:67`; `bin/regua_relogio.sh:77`)
- `regua_calendario` ignora o rc dos três docker e compara apenas nomes vermelhos; três falhas de infraestrutura podem terminar OK com lista vazia. (`bin/regua_calendario.sh:43`; `bin/regua_calendario.sh:48`; `bin/regua_calendario.sh:55`; `bin/regua_calendario.sh:59`)
- `vigia_arvore` troca ausência de `Ran` por rc 1; nas confirmações decide pelo log, sem preservar 75. (`bin/vigia_arvore.sh:88`; `bin/vigia_arvore.sh:90`; `bin/vigia_arvore.sh:106`; `bin/vigia_arvore.sh:136`)
- Moldes descartam os rc de RED/GREEN e decidem pelo texto; o molde de autocura termina com `echo`, podendo retornar 0 após falha do teste. (`bin/molde_fatia/rodar.sh:46`; `bin/molde_fatia/rodar.sh:48`; `bin/molde_autocura/rodar.sh:24`; `bin/molde_autocura/rodar.sh:27`)
- `_iso_roda` devolve o resultado do grep; falha de infraestrutura pode ser atribuída à árvore por `isolar_ou_parar`. (`bin/isolamento.sh:31`; `bin/isolamento.sh:34`; `bin/isolamento.sh:45`; `bin/isolamento.sh:49`)
- `placar_code` não guarda falha de `teste_envfile` nem do docker/pipeline; saída vazia vira `_cred=0`. (`bin/placar_code.sh:121`; `bin/placar_code.sh:127`; `bin/placar_code.sh:128`)
- `_rodar_testes` ignora `returncode`, devolvendo apenas presença de `OK`; após migração, 75 também se tornaria `False`, indistinto de vermelho. `_isolar` grava “arvore” e `_isolamento` devolve verdadeiro nessa situação. (`app/core/esteira_vigia.py:760`; `app/core/esteira_vigia.py:762`; `app/core/esteira_vigia.py:741`; `app/core/integrador_lote.py:403`)
- `_conferir_quarentena` ignora até esse booleano: log sem `FAIL/ERROR` libera todos os testes da lista, mesmo que nada tenha rodado. (`app/core/integrador_lote.py:769`; `app/core/integrador_lote.py:771`; `app/core/integrador_lote.py:772`)
- `_suite` ignora rc da régua e escolhe o arquivo `/tmp/regua_*` mais recente; falha antes de produzir log novo pode ler uma execução anterior. (`app/core/integrador_lote.py:375`; `app/core/integrador_lote.py:376`; `app/core/integrador_lote.py:379`)
- Existe cópia adicional de pre-push em `app/core/hooks`, fora do universo `bin/**/*.sh + Python` do contrato: docker direto em `_teste_irmao`, com duas chamadas de testes. Sua ativação é **[não verificado]**; o wrapper oficial gerado aponta a `bin/pre-push.sh`. (`app/core/hooks/pre-push.sh:54`; `app/core/hooks/pre-push.sh:82`; `app/core/hooks/pre-push.sh:87`; `bin/hooks.sh:56`)

Acoplamentos de testes ao texto/argv: [nao verificado]

- `test_arvore_vermelha` exige literais `desfazer_copia`, `exec bash "$script"`, `git archive HEAD app`, `SNAP=/tmp/vigia_arvore_commit_`, `sozinhos` e `alarme_arvore`; extrai o **primeiro** `grep -oP` de isolamento e executa só esse grep em log sintético. Não executa os scripts nem cria cadeia aninhada de trava. (`app/core/tests/test_arvore_vermelha.py:39`; `app/core/tests/test_arvore_vermelha.py:53`; `app/core/tests/test_arvore_vermelha.py:58`; `app/core/tests/test_arvore_vermelha.py:61`; `app/core/tests/test_arvore_vermelha.py:67`; `app/core/tests/test_arvore_vermelha.py:72`)
- `test_suite_por_lote` analisa o texto do molde com regex de linhas contendo `LABELS` e `regua.sh`/`$LABELS`; não executa a suite do integrador. (`app/core/tests/test_suite_por_lote.py:81`; `app/core/tests/test_suite_por_lote.py:86`; `app/core/integrador_lote.py:203`)
- `test_prepush_veredito_e_o_rc` recorta a função por `^_teste_irmao()`/`^}`, exige o literal `return "$_rc"` e proíbe `| tail -12; }`. (`bin/tests/test_prepush_veredito_e_o_rc.sh:61`; `bin/tests/test_prepush_veredito_e_o_rc.sh:64`; `bin/tests/test_prepush_veredito_e_o_rc.sh:68`)
- `test_prepush_testa_o_commit` exige o argv textual `-v "$ARVORE_PUSH/app":/app`, proíbe a montagem da árvore viva e cobra nomes dos auxiliares. (`bin/tests/test_prepush_testa_o_commit.sh:63`; `bin/tests/test_prepush_testa_o_commit.sh:65`; `bin/tests/test_prepush_testa_o_commit.sh:69`; `bin/tests/test_prepush_testa_o_commit.sh:71`)
- `test_recursos_sourced` junta continuações e exige `TESTE_DOCKER` ou `--cpuset-cpus` em cada linha contendo `docker run` e `manage.py test`. (`bin/tests/test_recursos_sourced.sh:38`; `bin/tests/test_recursos_sourced.sh:42`)
- `test_segredo_fora_do_argv` busca `-e DB_PASSWORD=` só em `bin`, exige os literais `teste_envfile()` e `umask 177`, além do modo 600 do arquivo existente. (`bin/tests/test_segredo_fora_do_argv.sh:24`; `bin/tests/test_segredo_fora_do_argv.sh:39`; `bin/tests/test_segredo_fora_do_argv.sh:42`; `bin/tests/test_segredo_fora_do_argv.sh:48`)
- `test_fonte_unica_antes_do_uso` depende de regex de `source ... recursos.sh` e posição da chamada das funções; aceita mesma linha, como no placar; varre só scripts diretos de `bin` e `molde_fatia`. (`bin/tests/test_fonte_unica_antes_do_uso.sh:38`; `bin/tests/test_fonte_unica_antes_do_uso.sh:41`; `bin/tests/test_fonte_unica_antes_do_uso.sh:47`; `bin/tests/test_fonte_unica_antes_do_uso.sh:54`)

`gate_cobranca` não dispara teste: cria `SONDA` única no host, mas copia para nome fixo `/tmp/gate_sonda.py` **dentro de `saas_core`**, e usa `manage.py shell`/`tenant_command`; esse temporário não fica no `logs/` do env-file. Falha de coleta vira zero pelo `_int`, portanto dado ausente pode parecer medição zero. (`bin/gate_cobranca.sh:18`; `bin/gate_cobranca.sh:36`; `bin/gate_cobranca.sh:37`; `bin/gate_cobranca.sh:40`; `bin/gate_cobranca.sh:54`)

## 31/10 17:42 · sessao 36dbb69fb8ed · E4-7-SAIDA-III · verificador · origem=Code

**A volta 2 ainda não fecha a verificação:** A1 corrige o noturno do teste, mas admite outro caso de data divergente; A2 reduz consultas, porém mantém N+1 nos encerrados; A3 e A4 têm a cura pedida no código. (`app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:214`; `app/templates/core/_painel_card.html:69`)

RED/GREEN da suíte Django, número efetivamente medido pela sonda, ocorrência dos contraexemplos em produção e ordem histórica de pouso: **[não verificado]**. [nao verificado]

1) **Quem escreve e quem lê.**

Os campos abaixo são escritos no contexto em memória do painel; `dia` lê o estado persistido pelo D4. O escritor persistente de `status_dia/status_rotulo` continua sendo a porta de lavratura. (`app/core/services/painel_op.py:225`; `app/ponto/status_dia.py:171`; `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:709`)

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `status=normal/geofence/turno_aberto` | O juiz fornece `_tm`; o painel escolhe os rótulos usando alerta geo e fim previsto +30 minutos. **Os quatro rótulos não vêm prontos do juiz.** (`app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:172`) | Filtro, contadores, agregado geo, ordenação, avatar, badge e filtro JS via atributo. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:244`, `app/core/services/painel_op.py:288`, `app/core/services/painel_op.py:296`; `app/templates/core/_painel_card.html:6`, `app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`) |
| `status=encerrado` | Painel: existe E recente e não existe `_tm` vivo. Não exige encontrar um turno efetivamente fechado. (`app/core/services/painel_op.py:193`) | Além dos leitores anteriores, abre o caminho de data/D4. (`app/core/services/painel_op.py:214`; `app/templates/core/_painel_card.html:67`) |
| `status=None` | Painel: sem turno vivo e sem E selecionada. (`app/core/services/painel_op.py:196`) | Avatar/badge usam `previsao`; Designar exige também `previsto_agora`. (`app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`, `app/templates/core/_painel_card.html:69`) |
| `previsao=folga/escala_suspeita/previsto/None` | `_previsao_de_hoje`: pergunta `EC.eh_dia_trabalho`; ausência de EC/tipo produz `None`. (`app/core/services/painel_op.py:27`; `app/escala/models.py:1048`) | Avatar e badge; testes dos dois primeiros casos. (`app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:94`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:103`) |
| `dia=None` ou `{status,rotulo,fonte}` | Inicialização local; D4 somente após encontrar `_d_turno` e obter encerramento verdadeiro. (`app/core/services/painel_op.py:212`) | Tooltip e texto `enc. · status` do card. (`app/templates/core/_painel_card.html:66`) |
| `data_turno` | `_tm.data_turno`; sem vivo, `_d_turno`; sem mapeamento, **hoje**. (`app/core/services/painel_op.py:230`) | Testes de ancoragem e novo A1. Leitor produtivo adicional encontrado: **[não verificado]**. (`app/core/tests/test_painel_op_turno_aberto_ancora.py:50`; `app/core/tests/test_painel_op_turno_pelo_juiz.py:80`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:263`) |
| `filtro_status` | GET da página/lote; serviço transforma palavra fora de `statuses` em `todos`. (`app/core/views.py:609`, `app/core/views.py:627`; `app/core/services/painel_op.py:64`) | Recorte das linhas e exclusão de postos vazios; a view ainda publica no contexto o GET bruto. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:284`; `app/core/views.py:669`) |
| `statuses` | Lista fixa dos quatro valores, rótulos e cores. (`app/core/services/painel_op.py:56`) | Validação do filtro, retorno e contexto da view; consumidor atual no HTML: **[não verificado]**. (`app/core/services/painel_op.py:64`, `app/core/services/painel_op.py:314`; `app/core/views.py:639`, `app/core/views.py:672`) |
| `_turno_vivo` | `turnos_abertos_de`, descartando respostas sem turno/entrada. (`app/core/services/painel_op.py:97`) | `_em_turno_ids`, presença/cobertura, duração, status e data da linha. (`app/core/services/painel_op.py:101`, `app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:176`, `app/core/services/painel_op.py:230`, `app/core/services/painel_op.py:286`) |
| `contadores` | Quatro somas sobre **linhas já filtradas**; linhas com `status=None` não entram nessas somas. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:244`) | View e stats da página/fragmento. (`app/core/views.py:637`, `app/core/views.py:670`; `app/templates/core/_painel_stats.html:32`; `app/templates/core/_painel_fragment.html:2`) |
| `previsto_agora` | Composição local de `eh_dia_trabalho` e `marcos_turno`, para ontem/hoje, usando EC ativa. (`app/core/services/painel_op.py:109`, `app/core/services/painel_op.py:144`) | Cobertura do posto e condição de Designar. (`app/core/services/painel_op.py:290`; `app/templates/core/_painel_card.html:69`) |

A entrada selecionada é a última E apurável por timestamp, com limite inferior de 24 horas e **sem limite superior**; ela também fornece horário, ID, coordenadas e entrada usada na classificação geo. (`app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:223`, `app/core/services/painel_op.py:228`; `app/ponto/turnos.py:2844`)

| Atributo/ação do card | Escritor | Leitor |
|---|---|---|
| `data-search` | Nome do posto/empresa. (`app/templates/core/_painel_card.html:3`) | Busca por `dataset.search`. (`app/templates/core/painel_operacional.html:198`) |
| `data-cobertura` | Cobertura calculada do posto. (`app/templates/core/_painel_card.html:4`) | Filtro Descobertos. (`app/templates/core/painel_operacional.html:176`) |
| `data-tem-geofence` | `p.geofence > 0` → `1/0`. (`app/templates/core/_painel_card.html:5`) | Filtro Geofence. (`app/templates/core/painel_operacional.html:177`) |
| `data-tem-aberto` | Concatena um `1` por colaborador aberto. (`app/templates/core/_painel_card.html:6`) | Filtro Aberto exige exatamente `'1'`. (`app/templates/core/painel_operacional.html:178`) |
| `data-tem-ausente` | Removido; o card atual publica apenas os atributos anteriores. (`app/templates/core/_painel_card.html:3`) | O filtro atual conserva Descobertos/Geofence/Aberto. (`app/templates/core/painel_operacional.html:173`) |
| Designar | `previsto_agora and not status`; publica titular ID/nome, posto ID/nome e empresa ID. (`app/templates/core/_painel_card.html:69`) | `abrirSubstModal` copia os cinco atributos para `estado`. (`app/templates/core/painel_operacional.html:280`) |

O modal envia titular/posto/janela ao GET de disponíveis e titular/substituto/posto/escala/janela ao POST de criação; o POST delega à porta `designar_cobertura`. `empresa_id` não integra esse POST, e o botão não transporta `data_turno`. (`app/templates/core/painel_operacional.html:343`, `app/templates/core/painel_operacional.html:410`; `app/ponto/views.py:3027`, `app/ponto/views.py:3065`; `app/templates/core/_painel_card.html:69`)

As portas chamadas pelo diff têm estes produtores e leitores externos:

| Porta | Quem produz a resposta | Outros leitores diretos encontrados |
|---|---|---|
| `turnos_abertos_de` | Carregador plural chama o mesmo `_turno_aberto_puro` do singular. (`app/ponto/turnos.py:2266`, `app/ponto/turnos.py:2333`) | Serviço situacional. (`app/colaboradores/services/situacional.py:169`) |
| `dia_das_batidas` | Dias dos turnos recebidos; batida não reclamada recebe dia civil. (`app/ponto/turnos.py:2883`) | API e espelho. (`app/api/views.py:1446`, `app/api/views.py:2090`; `app/ponto/services/espelho.py:462`) |
| `dias_encerrados` | Carrega células/vínculos e chama `_dia_encerrado_motivo_puro` por dia. (`app/ponto/turnos.py:2129`) | Calendário, cartório, D4, emissão retroativa e suprajuiz. (`app/colaboradores/services/calendario.py:697`; `app/ponto/services/cartorio.py:910`; `app/ponto/status_dia.py:176`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/management/commands/supra_juiz.py:108`) |
| `status_do_dia_lavrado` | Lê célula e pergunta encerramento; devolve lavra ou resposta temporal. (`app/ponto/status_dia.py:157`) | Chamador produtivo direto encontrado nesta cópia: painel. (`app/core/services/painel_op.py:220`) |
| `turnos_do_colab` | Carrega universo vivo, células, vínculos/ata; pareia e recorta por `data_turno`. (`app/ponto/turnos.py:1848`, `app/ponto/turnos.py:1973`) | Calendário, espelho, drawer, API, PDF, núcleo, escala e caminhos de chamados/apuração listados abaixo. (`app/colaboradores/services/calendario.py:301`; `app/ponto/services/espelho.py:625`; `app/colaboradores/services/drawer.py:45`; `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213`; `app/ponto/nucleo.py:47`) |

O restante do censo direto de `turnos_do_colab` inclui:

- Chamados: detector, juízes, modelo, emissão e materialização. (`app/chamados/detector_anomalias.py:155`; `app/chamados/juizes.py:369`, `app/chamados/juizes.py:1727`; `app/chamados/models.py:1260`; `app/chamados/services/disputa_emissao.py:644`; `app/chamados/services/materializacao.py:109`, `app/chamados/services/materializacao.py:204`)
- Escala: defesa, escala automática, impacto de troca, jornada e grade. (`app/escala/regua_defesa.py:358`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`; `app/escala/servico_jornada.py:250`, `app/escala/servico_jornada.py:392`; `app/escala/utils.py:1524`, `app/escala/utils.py:1838`)
- Ponto: bordas, esmeril, passivo, regularização, teto, triagem, vigia, leitura de turno e views. (`app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/passivo_s84.py:82`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/turno_leitura.py:89`, `app/ponto/turno_leitura.py:95`; `app/ponto/views.py:377`, `app/ponto/views.py:1586`, `app/ponto/views.py:3167`)
- Comandos: censos, ausências, pares relâmpago, DNA, oráculo, turno retroativo, abandono e vigias. (`app/ponto/management/commands/censo_fase_12x36.py:180`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/e6_oraculo.py:147`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`)
- No próprio módulo: realizado, encerramento, par completo, mapa de dias e batidas do dia-turno. (`app/ponto/turnos.py:778`, `app/ponto/turnos.py:2079`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2152`, `app/ponto/turnos.py:2882`, `app/ponto/turnos.py:2917`)

`ALLOWLIST` e `LOOKUP_ALLOWLIST` são escritas como conjuntos no módulo do contrato. A primeira alimenta os testes de novos `.date()` e entradas podres; a segunda alimenta os testes equivalentes de lookups ORM. Retirar o painel da primeira é coerente com a mudança sintática; mantê-lo na segunda corresponde aos lookups de geo/atividade ainda existentes. Isso **não verifica a semântica da data retornada indiretamente** por `dia_das_batidas`. (`app/ponto/tests/test_contract_no_batida_date.py:10`, `app/ponto/tests/test_contract_no_batida_date.py:98`, `app/ponto/tests/test_contract_no_batida_date.py:233`, `app/ponto/tests/test_contract_no_batida_date.py:247`, `app/ponto/tests/test_contract_no_batida_date.py:266`; `app/core/services/painel_op.py:104`, `app/core/services/painel_op.py:217`, `app/core/services/painel_op.py:274`)

Fora da tela, o registro vivo mantém pendentes os contadores, badge/atributo de aberto e filtro JS. O host lê o mesmo medidor por `bin/leitor_dia_estado.py`; o gerador de estado consome esse resultado. (`app/core/juizes.py:715`, `app/core/juizes.py:727`; `bin/leitor_dia_estado.py:25`, `bin/leitor_dia_estado.py:35`; `bin/gerar_estado.py:284`)

2) **A1..A4: cura e contraexemplos.**

| Item | Resultado da inspeção |
|---|---|
| **A1** | Cura a fixture 19–07, pausa 00–01, consulta 08h: agora busca o dia do turno e o teste exige esse dia. **Não fecha toda a classe**, conforme contraexemplo abaixo. (`app/core/services/painel_op.py:215`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:241`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:263`) |
| **A2** | Cura o custo do D4 para linhas excluídas pelo filtro e troca o turno vivo por lote. Para linhas encerradas preservadas, continuam chamadas individuais e recargas internas. Número medido exigido na triagem: **[não verificado]**. (`app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:214`; `app/ponto/status_dia.py:171`) |
| **A3** | Cura a cobertura parcial: o botão deixou de exigir posto descoberto. O teste tem dois previstos, um presente e exige botão apenas no outro. Saída antecipada continua barrada por `status=encerrado`, como já reconhecido na fila fornecida. (`app/templates/core/_painel_card.html:69`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:335`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:361`; `app/core/services/painel_op.py:193`) |
| **A4** | Cura `ausente/folga/lixo` na porta compartilhada por página e lote. O teste preserva também o filtro válido `encerrado`. Há quebra herdada do refresh para filtros válidos, detalhada no item 5. (`app/core/services/painel_op.py:64`; `app/core/views.py:607`, `app/core/views.py:632`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:371`) |

**Contraexemplo construído de A1, somente com batidas passadas:** colaborador ativo, vínculo diurno 07–19 com intervalo 12–13; agora **17/09/2026 às 20h local**; E viva em **14/09 às 07h** e E viva em **16/09 às 20h com `is_intervalo=True`**, nenhuma outra batida. A flag existe no modelo; o pareador a absorve no turno corrente, e a guarda temporal desse trecho exige `not _iv`. Assim, o turno de 14/09 contém as duas batidas. (`app/ponto/models.py:26`; `app/ponto/turnos.py:1324`, `app/ponto/turnos.py:1347`, `app/ponto/turnos.py:1369`, `app/ponto/turnos.py:1495`)

Nesse caso:

- O plural lê 15–17/09, não recebe a E inicial e ignora a E intervalada isolada. O painel encontra a E de 16/09 nas 24 horas e a chama `encerrado`. (`app/ponto/turnos.py:2323`, `app/ponto/turnos.py:1369`; `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:193`)
- `turnos_do_colab(15,17)` lê a E antiga pelo pad, mas elimina o turno de 14/09 pelo recorte final. `dia_das_batidas` então usa o dia civil **16/09**, que vira `data_turno` da linha. (`app/ponto/turnos.py:1857`, `app/ponto/turnos.py:1973`, `app/ponto/turnos.py:2889`; `app/core/services/painel_op.py:217`, `app/core/services/painel_op.py:230`)
- O espelho mensal atribui essa mesma E ao turno de **14/09**; o calendário mensal também a recebe nesse turno. Portanto, há divergência concreta de pertencimento/data entre as janelas. Ocorrência em produção: **[não verificado]**. (`app/ponto/services/espelho.py:462`; `app/ponto/turnos.py:2884`; `app/colaboradores/services/calendario.py:301`)

Para A2, o próprio cenário do teste — passar de três para seis colaboradores E07/S19, consulta 20h, filtro `todos` — percorre o bloco D4 mais vezes. `turnos_do_colab` consulta batidas/células/vínculos; `dias_encerrados` recarrega células/vínculos; existindo célula, D4 consulta-a e recalcula turnos/encerramento. O custo residual cresce por linha; seu coeficiente efetivo permanece **[não verificado]**. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:269`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:275`; `app/ponto/turnos.py:1858`, `app/ponto/turnos.py:2129`; `app/ponto/status_dia.py:171`)

Os novos testes plantam `status_dia` diretamente e não comparam painel, espelho e calendário para o mesmo caso. Os outros dois leitores ainda possuem classificadores próprios; igualdade entre suas palavras e o D4 não foi demonstrada por esses testes. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:76`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:260`; `app/ponto/services/espelho.py:152`; `app/colaboradores/services/calendario.py:93`)

3) **Universos e janelas não são integralmente iguais.**

| Caminho | Universo/janela efetivos |
|---|---|
| Entrada/candidatos do painel | Vivas desde `agora−24h`, sem teto superior; colaboradores ativos do recorte empresarial. (`app/core/services/painel_op.py:70`, `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:94`) |
| Turno vivo singular/plural | Vivas por data civil de hoje−2 até hoje; mesmo corpo puro. O painel restringe antes os candidatos a 24h. (`app/ponto/turnos.py:2190`, `app/ponto/turnos.py:2323`, `app/ponto/turnos.py:2333`; `app/core/services/painel_op.py:94`) |
| Turnos para D4 do painel | Dias de turno `[hoje−2,hoje]`; consulta de batidas recebe pad `[hoje−4,hoje+1]`, mas o resultado volta a ser recortado por `data_turno`. (`app/core/services/painel_op.py:215`; `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1973`) |
| Mapa e portão do painel | Recebem corretamente a **mesma lista** e a mesma janela anterior. (`app/core/services/painel_op.py:216`) |
| D4 interno | Recarrega turnos e encerramento para `[data,data]`, não reutiliza a lista do painel. (`app/ponto/status_dia.py:175`) |
| Espelho/calendário | Usam o período apresentado, mensal ou personalizado. O calendário passa seus próprios turnos à juíza da mesma janela. (`app/ponto/services/espelho.py:462`, `app/ponto/services/espelho.py:625`; `app/colaboradores/services/calendario.py:285`, `app/colaboradores/services/calendario.py:301`, `app/colaboradores/services/calendario.py:697`) |

A identidade do filtro `retratada_em IS NULL` foi preservada nesses carregadores; isso não torna suas janelas equivalentes. O próprio contrato de alimentação avisa que a resposta do pareamento depende da janela. (`app/ponto/turnos.py:1858`, `app/ponto/turnos.py:2111`, `app/ponto/turnos.py:2324`, `app/ponto/turnos.py:2890`)

**Não: dois dias não cobrem universalmente todo turno cuja E recente vira “encerrado”.** O caso intervalado acima refuta a afirmação, porque a autoridade de apresentação dá dia civil à batida cujo turno foi excluído da lista recebida. (`app/ponto/turnos.py:1976`, `app/ponto/turnos.py:2884`, `app/ponto/turnos.py:2893`)

Há ainda um recorte herdado que perde presença normal: **24x48 07–07, E ontem às 07h, sem S, consulta hoje às 08h**. O juiz pode mantê-la viva até 11h, mas a E já tem 25 horas e fica fora dos candidatos do painel. A troca para plural não corrige isso. (`app/ponto/selecao_periodo.py:164`; `app/ponto/turnos.py:1987`, `app/ponto/turnos.py:2005`; `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:94`)

Também não há garantia para E futura: uma E depois de amanhã passa pelo limite inferior, fica fora da janela do plural/mapa e pode resultar em `encerrado`, `dia=None`, `data_turno=hoje`. Existência desse dado em produção: **[não verificado]**. (`app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:193`, `app/core/services/painel_op.py:230`; `app/ponto/turnos.py:2844`)

4) **Classes de reprovação e mordida dos testes.** [nao verificado]

- **Segundo juiz:** a troca singular→plural não cria um: ambos chegam ao mesmo corpo. O dia do turno e o D4 já têm autoridades registradas. Entretanto, `dia_das_batidas` responde também à apresentação de batidas não reclamadas; usar sua resposta como identidade de turno é justamente a fragilidade de A1. (`app/ponto/turnos.py:2266`, `app/ponto/turnos.py:2333`, `app/ponto/t

[truncado em 20000 de 28318 caracteres]

## 31/10 17:42 · sessao 834d7c0b4f9f · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

_1180 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
