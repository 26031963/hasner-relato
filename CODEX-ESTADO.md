# CODEX-ESTADO

> Ultima resposta processada: **24/11/2026 10:28** · publicado em **24/11/2026 10:33** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 24/11 10:28 · sessao 8540913dc18e · R1-REMEDE-2411 · sonda · origem=Code

No dado fornecido, `rc_sombra=0` é a única linha `rc_sombra`. A transcrição abaixo reproduz o dado fornecido linha a linha. A execução em produção está [nao verificado].

```text
sonda_leitura: script.py [e6495196abed8215] no cpuset 4-7, lendo sombra como leitor (LEITURA)

AUTORIDADE {'dia_sem_lavratura_no_espelho': 8808, 'dia_batida_impar': 435, 'dias_comparados': 7763, 'diverge_acima_60': 300, 'dono_BATIDA': 275, 'impar_que_DIVERGE': 280, 'impar_no_conjunto_do_dia': 276, 'impar_dentro_da_faixa': 240, 'bate_ate_10min': 7372, 'impar_FORA_da_faixa': 40, 'dono_ESTRUTURA': 37, 'dia_sem_trabalho_ambos': 151, 'dia_isento_ou_ausencia': 1683, 'e6_zero_esp_trabalho': 26, 'dono_CADASTRO': 79, 'esp_zero_e6_trabalho': 42, 'impar_que_BATE': 155, 'esp_desconta_janela_declarada': 288, 'diverge_10_60': 23, 'impar_SO_POR_CORTE_do_oraculo': 4}
AUTORIDADE erros no espelho: 0
AUTORIDADE BATE (ate 10 min): 95.0% de 7763 dias comparados
AUTORIDADE dos que divergem: 240 dentro da faixa piso..teto (indecidivel sem DNA), 40 FORA da faixa (contraditorio)
AUTORIDADE paridade de quem: 276 com contagem impar no conjunto do dia (falta OU sobra batida), 4 partidos SO pelo corte do proprio oraculo
AUTORIDADE ESTRUTURA    37 dia-colab     152.9 h   25 colabs   o sistema se contradiz ou calcula errado DADO o cadastro -- fila 1
AUTORIDADE CADASTRO     79 dia-colab     315.3 h   37 colabs   o DNA nao descreve as batidas -- lista do admin, nao codigo
AUTORIDADE BATIDA      275 dia-colab    1473.0 h  129 colabs   falta ou sobra batida -- lista do admin, nao codigo
AUTORIDADE SOMA        391   <- tem de ser igual a soma das classes (391)
AUTORIDADE colabs com divergencia: 164
CSV_POR_DONO {'BATIDA': 275, 'ESTRUTURA': 37, 'CADASTRO': 79} TOTAL 391
CSV_PARIDADE {('IMPAR', 'BATIDA'): 226, ('par', 'ESTRUTURA'): 36, ('par', 'CADASTRO'): 26, ('IMPAR', 'CADASTRO'): 53, ('par', 'BATIDA'): 49, ('IMPAR', 'ESTRUTURA'): 1}
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 4|37|2026-11-14|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|110|2026-11-12|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|112|2026-10-21|ESTRUTURA|546|838|838|292|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|112|2026-10-22|ESTRUTURA|541|835|835|294|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|113|2026-10-30|ESTRUTURA|242|0|0|-242|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|113|2026-11-01|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|114|2026-11-08|ESTRUTURA|439|429|499|-10|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 3|114|2026-11-15|ESTRUTURA|499|440|499|-59|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 3|130|2026-10-29|ESTRUTURA|335|672|730|337|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|200|2026-10-29|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|200|2026-11-02|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|218|2026-11-14|ESTRUTURA|103|0|0|-103|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|220|2026-11-10|ESTRUTURA|337|430|486|93|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-09|ESTRUTURA|551|597|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-10|ESTRUTURA|551|598|611|47|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-11|ESTRUTURA|550|598|611|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-13|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-17|ESTRUTURA|553|598|612|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-18|ESTRUTURA|550|596|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-19|ESTRUTURA|554|599|613|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|296|2026-11-02|ESTRUTURA|345|0|0|-345|e6_zero_esp_trabalho|par|0|trabalhou |n_batidas=0
ESTRUTURA 2|302|2026-11-20|ESTRUTURA|296|0|0|-296|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|334|2026-11-16|ESTRUTURA|209|591|683|382|diverge_acima_60|par|3|folga |n_batidas=4
ESTRUTURA 2|382|2026-11-10|ESTRUTURA|418|0|0|-418|e6_zero_esp_trabalho|par|3|trabalhou |n_batidas=0
ESTRUTURA 2|516|2026-10-23|ESTRUTURA|361|0|0|-361|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|727|2026-11-10|ESTRUTURA|307|0|0|-307|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|841|2026-11-16|ESTRUTURA|178|0|0|-178|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|874|2026-10-22|ESTRUTURA|667|383|383|-284|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 2|884|2026-11-08|ESTRUTURA|679|699|739|20|diverge_10_60|par|4|rejeitado |n_batidas=4
ESTRUTURA 3|909|2026-11-11|ESTRUTURA|831|245|374|-586|diverge_acima_60/FORA|IMPAR|CORTE_do_oraculo|em_aberto |n_batidas=4
ESTRUTURA 2|922|2026-11-01|ESTRUTURA|1382|68|68|-1314|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|922|2026-11-04|ESTRUTURA|676|458|458|-218|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 1|950|2026-11-11|ESTRUTURA|480|240|240|-240|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|989|2026-10-23|ESTRUTURA|721|567|567|-154|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|996|2026-10-28|ESTRUTURA|0|580|580|580|esp_zero_e6_trabalho|par|1|folga |n_batidas=2
ESTRUTURA 3|996|2026-11-10|ESTRUTURA|443|1026|1116|583|diverge_acima_60|par|5|em_aberto |n_batidas=6
ESTRUTURA 2|1044|2026-11-13|ESTRUTURA|204|135|339|-69|diverge_acima_60|par|4|em_aberto |n_batidas=4
FIM_LISTA
rc_sombra=0
```

## 24/11 10:21 · sessao 0bcf3a448c61 · R1-REMEDE-2411 · sonda · origem=Code

No dado fornecido, não há `rc_sombra` diferente de `0`. A execução original está [nao verificado].

```text
sonda_leitura: script.py [0d4e6e058a9c5469] no cpuset 4-7, lendo sombra como leitor (LEITURA)

AUTORIDADE {'dia_sem_lavratura_no_espelho': 8912, 'dias_comparados': 7521, 'bate_ate_10min': 7203, 'dia_isento_ou_ausencia': 1878, 'dia_batida_impar': 361, 'impar_que_BATE': 148, 'esp_zero_e6_trabalho': 35, 'dono_BATIDA': 217, 'diverge_acima_60': 235, 'impar_que_DIVERGE': 213, 'impar_no_conjunto_do_dia': 212, 'impar_FORA_da_faixa': 32, 'impar_dentro_da_faixa': 181, 'dono_CADASTRO': 75, 'e6_zero_esp_trabalho': 19, 'dia_sem_trabalho_ambos': 151, 'esp_desconta_janela_declarada': 258, 'dono_ESTRUTURA': 26, 'diverge_10_60': 29, 'impar_SO_POR_CORTE_do_oraculo': 1}
AUTORIDADE erros no espelho: 0
AUTORIDADE BATE (ate 10 min): 95.8% de 7521 dias comparados
AUTORIDADE dos que divergem: 181 dentro da faixa piso..teto (indecidivel sem DNA), 32 FORA da faixa (contraditorio)
AUTORIDADE paridade de quem: 212 com contagem impar no conjunto do dia (falta OU sobra batida), 1 partidos SO pelo corte do proprio oraculo
AUTORIDADE ESTRUTURA    26 dia-colab      88.4 h   16 colabs   o sistema se contradiz ou calcula errado DADO o cadastro -- fila 1
AUTORIDADE CADASTRO     75 dia-colab     295.0 h   31 colabs   o DNA nao descreve as batidas -- lista do admin, nao codigo
AUTORIDADE BATIDA      217 dia-colab    1021.0 h  117 colabs   falta ou sobra batida -- lista do admin, nao codigo
AUTORIDADE SOMA        318   <- tem de ser igual a soma das classes (318)
AUTORIDADE colabs com divergencia: 146
CSV_POR_DONO {'BATIDA': 217, 'CADASTRO': 75, 'ESTRUTURA': 26} TOTAL 318
CSV_PARIDADE {('par', 'BATIDA'): 43, ('IMPAR', 'BATIDA'): 174, ('IMPAR', 'CADASTRO'): 39, ('par', 'CADASTRO'): 36, ('par', 'ESTRUTURA'): 26}
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 4|37|2026-10-05|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 4|37|2026-10-09|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|129|2026-10-12|ESTRUTURA|0|779|779|779|esp_zero_e6_trabalho|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|218|2026-10-15|ESTRUTURA|105|0|0|-105|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|252|2026-09-23|ESTRUTURA|548|594|608|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-24|ESTRUTURA|552|598|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-25|ESTRUTURA|556|604|616|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-28|ESTRUTURA|535|581|595|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-01|ESTRUTURA|545|590|605|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-02|ESTRUTURA|545|593|606|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-06|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-09|ESTRUTURA|544|592|604|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|369|2026-10-12|ESTRUTURA|476|416|476|-60|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 2|382|2026-09-30|ESTRUTURA|364|0|0|-364|e6_zero_esp_trabalho|par|2|trabalhou |n_batidas=0
ESTRUTURA 2|451|2026-09-23|ESTRUTURA|707|718|718|11|diverge_10_60|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|451|2026-09-25|ESTRUTURA|705|718|718|13|diverge_10_60|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|478|2026-09-25|ESTRUTURA|719|8|8|-711|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|515|2026-10-09|ESTRUTURA|244|0|0|-244|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|515|2026-10-11|ESTRUTURA|245|0|0|-245|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|723|2026-10-08|ESTRUTURA|440|345|500|-95|diverge_acima_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 3|746|2026-09-24|ESTRUTURA|289|193|350|-96|diverge_acima_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|841|2026-10-20|ESTRUTURA|681|522|701|-159|diverge_acima_60|par|4|em_aberto |n_batidas=4
ESTRUTURA 2|854|2026-10-04|ESTRUTURA|718|7|7|-711|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 2|864|2026-10-12|ESTRUTURA|509|449|509|-60|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 3|868|2026-09-22|ESTRUTURA|0|679|733|679|esp_zero_e6_trabalho|par|1|em_aberto |n_batidas=4
ESTRUTURA 3|876|2026-09-21|ESTRUTURA|550|665|722|115|diverge_acima_60|par|1|em_aberto |n_batidas=4
FIM_LISTA
rc_sombra=0
```

## 24/11 09:45 · sessao 01fb331cff27 · RESOLVER-PASSA-NA-FRENTE · verificador · origem=Code

**Pré-verificação reprovada.** O calendário preserva o veto da pergunta viva respondida, mas o POST pode resolver o mesmo dia. A recusa por tranca funciona por dia na porta, mas um lote misto ainda encaminha os dias recusados à reconciliação. O histórico novo deixa de fora resoluções de competências que o calendário ainda permite resolver. `app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:506`, `app/ponto/views.py:3520`, `app/ponto/views.py:3533`, `app/chamados/services/pergunta_no_app.py:91`

Usei o diff fornecido e a cópia somente para leitura. Hash e base Git [nao verificado], pois a cópia não tem metadados Git. Execução dos testes, renderização no APK, sonda das 1.346 respondidas e impacto em produção [nao verificado]. O corte pede a sonda e o RED 3/3. `app/docs/CORTES.md:642`

## A. Escopo do aval

1. **Parcial.** O calendário ainda combina a face SQL de C21 com C6 e veta a oferta quando há pergunta viva respondida. A tarefa consulta essa mesma oferta. Já o POST valida permissão, data e veredito, e a porta acrescenta motivo e C20, sem consultar a oferta. Assim, aceita em princípio o dia vetado, inclusive o dia que os próprios testes novos resolvem com uma respondida viva. Também inexiste um veto geral para `nao_sei`: a tarefa pode publicar `resolver_dia` quando a oferta é verdadeira mesmo com a flag ligada, e a Folha prioriza a rota nesse caso. Os três testes **do 4b de 15/11** estão em `:132`, `:136` e `:143` e esperam ausência de porta e fio `nada`; verde [nao verificado]. `app/colaboradores/services/calendario.py:117`, `app/colaboradores/services/calendario.py:501`, `app/chamados/services/tarefa_do_dia.py:702`, `app/chamados/services/tarefa_do_dia.py:781`, `app/folha/fora_da_folha.py:86`, `app/ponto/views.py:3491`, `app/ponto/services/veredito_celula.py:251`, `app/ponto/tests/test_resolver_passa_na_frente.py:53`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:132`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:136`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:143`
2. **Parcial.** A API entrega `rotulo` no item histórico, deixa a encerrada fora de `respondidas` no fluxo normal e publica a nota quando o chamado transiciona. O calendário admite qualquer data anterior à competência corrente, enquanto o histórico só busca a anterior e a corrente. Uma resolução elegível mais antiga já nasce fora desse histórico. Também há um caminho em que a pergunta perde a via e o rótulo antes de recebê-los: a falta ou a batida aciona o cartório antes de `fechar_o_fio_do_dia`, e o fechamento só carimba quem **ainda** passa no C6 depois desses efeitos. Uma respondida cujo marco a própria batida satisfez pode cair nessa ordem. `app/api/views.py:2591`, `app/api/views.py:2595`, `app/api/views.py:1123`, `app/chamados/services/pergunta_no_app.py:91`, `app/colaboradores/services/calendario.py:117`, `app/ponto/services/veredito_celula.py:198`, `app/ponto/services/veredito_celula.py:365`, `app/ponto/services/veredito_celula.py:381`, `app/ponto/signals.py:132`, `app/ponto/signals.py:143`, `app/chamados/juizes.py:1516`
3. **Parcial.** Para um dia que C20 dá por trancado, com motivo preenchido, a porta recusa antes de escrever o veredito e devolve a causa ao envelope. Em lote misto, um único sucesso dispara `reconciliar_apos_vinculo(min(datas), max(datas))` sobre o intervalo original. Esse caminho reavalia a faixa inteira e pode atualizar ou apagar `FuroDiario` no dia recusado. O envelope registra cada recusa, mas o texto do lote mostra apenas as quatro primeiras. `app/ponto/services/veredito_celula.py:254`, `app/ponto/services/veredito_celula.py:258`, `app/ponto/views.py:3526`, `app/ponto/views.py:3533`, `app/ponto/views.py:3548`, `app/colaboradores/services/pos_vinculo.py:62`, `app/colaboradores/services/pos_vinculo.py:79`, `app/ponto/portas/celula.py:1292`, `app/core/respostas.py:54`
4. **Sim, nos dois sítios nomeados.** Ressuscitar exclui `resolver_dia` antes de simular. `merito_perdedor` reconhece a via em perdedora com resposta pelo C21, e o plano a retém como mérito. `materializacao.py:879` ficou fora do diff fornecido. `app/chamados/models.py:1553`, `app/chamados/services/fio.py:1394`, `app/chamados/services/fio.py:1453`, `app/chamados/services/materializacao.py:879`

**Além do literal dos quatro itens.** Motivo obrigatório e trilha assinada vêm da lei de 23/11. O diff também publica o **texto livre integral** do supervisor na nota do chamado, libera qualquer nota de sistema que contenha a frase fixa e acrescenta `"motivo": null` ao log dos chamadores internos, inclusive `declarar_dia` falta. O limite de 200 caracteres é só do HTML. Na porta, basta texto após `strip()`. A trilha da pergunta corta o texto para reservar espaço para a frase. `app/docs/CORTES.md:628`, `app/ponto/services/veredito_celula.py:166`, `app/ponto/services/veredito_celula.py:254`, `app/ponto/services/veredito_celula.py:396`, `app/ponto/services/veredito_celula.py:404`, `app/api/views.py:62`, `app/api/views.py:1128`, `app/ponto/services/declarar_dia.py:213`, `app/templates/ponto/partials/_veredito_dia.html:23`, `app/templates/ponto/partials/_veredito_lote_barra.html:7`

## B. Escritores, leitores e semântica

| Campo ou valor | Quem escreve, quem lê e o que significa |
|---|---|
| `via_resolucao` e `VIA_RESOLVER_DIA` | O modelo declara a via. O Resolver seleciona perguntas do fato, chama `fechar_pelo_resolver` e persiste por `carimbar_identidade` → `_escrever_carimbo`. `admin_decidiu` continua com `fechar_por_admin`. O campo também tem portas preexistentes de carimbo comum, descarimbo individual/em lote, relavra de vias derivadas e troca pelo fato no espelho. O único produtor de `resolver_dia` localizado em produção passa pela porta de identidade. Os leitores diretos novos são a guarda de idempotência, a ressuscitação, `merito_perdedor`, o histórico e o rótulo. `app/ponto/services/veredito_celula.py:198`, `app/chamados/models.py:2786`, `app/chamados/models.py:3349`, `app/chamados/models.py:3365`, `app/chamados/models.py:3415`, `app/chamados/models.py:3517`, `app/chamados/models.py:3559`, `app/chamados/models.py:3645`, `app/chamados/models.py:3649`, `app/chamados/services/relavra_via.py:106`, `app/chamados/models.py:1559`, `app/chamados/services/fio.py:1406`, `app/chamados/services/pergunta_no_app.py:95` |
| `VIAS_QUE_VENCEM_A_RESPOSTA`, C6 | A classe define o conjunto. C6 lê via **com** carimbo, na projeção temporal, antes de olhar a resposta. `q_vivas` lê a mesma constante na face SQL. A via permanece na identidade do fato e C21 continua decidindo se existe resposta. Histórico e rótulo leem a via crua, sem conferir carimbo ou instante, embora a rota já tenha um `agora` da borda. Uma gravação entre as duas consultas pode deixar a mesma pergunta no retrato anterior e no histórico novo. `app/chamados/models.py:2794`, `app/chamados/models.py:2811`, `app/chamados/models.py:3761`, `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:1494`, `app/chamados/services/pergunta_no_app.py:91`, `app/api/views.py:2555`, `app/api/views.py:2595` |
| `FRASE_RESOLVIDO_ANTES` e `rotulo` | A constante nasce no despachante e entra em `DecisaoPergunta.ato_motivo`, na nota do chamado e no JSON por `rotulo_do_app`. A nota é gravada por `nota.anexar_ocorrencia`. A API usa a frase também como autorização de visibilidade por substring. Há dois sentidos concretos: a frase afirma **antes da resposta**, mas o escritor a aplica à pergunta já respondida e conserva a resposta. A renderização pelo APK é [nao verificado]. `app/ponto/services/veredito_celula.py:162`, `app/ponto/services/veredito_celula.py:219`, `app/ponto/services/veredito_celula.py:229`, `app/ponto/services/veredito_celula.py:396`, `app/chamados/services/nota.py:75`, `app/chamados/services/pergunta_no_app.py:101`, `app/api/views.py:62`, `app/api/views.py:2591`, `app/chamados/models.py:4440`, `app/ponto/tests/test_resolver_passa_na_frente.py:142` |
| `dias_trancados` / C20 | C20 lê `PeriodoFechado` por K7 e a trilha posterior de Cobrar, Resolver ou desarquivar. Consome `(colaborador, dia do turno)`. O leitor avulso deriva esse dia da pergunta e exclui conversa. C6, a carga da tarefa, modelos, reconciliador, emissão e agora Resolver leem o mesmo juiz. Os escritores da tranca são `trancar_periodo` e `reabrir_periodo`. **Dia trancado por C20** pode ser falso em competência ainda trancada, quando houve toque administrativo anterior. `app/chamados/juizes.py:339`, `app/chamados/juizes.py:1023`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1130`, `app/chamados/juizes.py:1330`, `app/chamados/juizes.py:1502`, `app/chamados/models.py:2050`, `app/chamados/reconciliador.py:789`, `app/chamados/services/disputa_emissao.py:2102`, `app/chamados/services/tarefa_do_dia.py:550`, `app/ponto/services/fechamento.py:1400`, `app/ponto/services/fechamento.py:1432` |
| `motivo` dos forms | O supervisor escreve texto de decisão. A view o passa a todos os dias selecionados, e o serviço usa a presença do parâmetro também para **identificar o ato**. `None` seleciona o caminho interno, string vazia produz recusa e texto aciona a via nova. O `motivo` de `recusas` é outro dado: a causa de erro devolvida pela porta. Cobrar usa outra rota e ignora o campo. `app/templates/ponto/partials/_veredito_dia.html:23`, `app/templates/ponto/partials/_veredito_dia.html:28`, `app/templates/ponto/partials/_veredito_lote_barra.html:7`, `app/ponto/views.py:3523`, `app/ponto/views.py:3530`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/views_cobrar.py:337` |

A via é terminal para C6 enquanto carimbada. **Há uma porta preexistente que pode apagar esse carimbo.** Em disputa aberta, a recusa administrativa aceita uma pergunta respondida, não validada e ainda no fato. `resolver_dia` satisfaz essas condições e o descarimbo em lote só protege `VIAS_FORA_DO_FATO`. Assim, corrigir por um novo Resolver fica como regra incompleta. Esse caminho usa a porta existente, sem um segundo escritor novo da via. `app/docs/CORTES.md:628`, `app/chamados/models.py:2809`, `app/chamados/services/validacao.py:948`, `app/chamados/services/validacao.py:979`, `app/chamados/services/validacao.py:999`, `app/chamados/models.py:3645`

## C. Classes de reprovação e mordida dos testes [nao verificado]

- **Juiz, escritor e fonte única.** C6, C20 e C21 são reutilizados. A via entra nas listas com propósitos distintos e preserva a FK. As catracas verificam a lista de identidade, o chamador e a via que ele pode gravar. A divergência de autoridade é entre a oferta `_TT11` e a execução. A derivação paralela adicionada é usar texto livre da nota como identidade de evento para decidir visibilidade. `app/core/juizes.py:290`, `app/core/juizes.py:304`, `app/core/juizes.py:751`, `app/ponto/views.py:3520`, `app/chamados/tests/test_o261e_f2_fila.py:137`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:491`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:500`, `app/api/views.py:62`
- **Relógio.** A API injeta `agora`, mas o helper novo aceita omiti-lo e cai em `localdate()`. Mais importante, `fechar_pelo_resolver` lê `_tz.now()` por pergunta, embora o serviço já tenha `_agora_do_ato`. `SILENCIO` herda o instante do carimbo de cada pergunta. O teste congela o relógio e não distingue um instante do ato de várias leituras. `app/api/views.py:2556`, `app/chamados/services/pergunta_no_app.py:80`, `app/chamados/services/pergunta_no_app.py:91`, `app/ponto/services/veredito_celula.py:251`, `app/chamados/models.py:3373`, `app/ponto/services/veredito_celula.py:219`, `app/ponto/tests/test_resolver_passa_na_frente.py:24`
- **Trava e exceções.** A ordem do fio está correta no trecho novo: colaborador primeiro, C20 sob a trava, depois disputa e chamado antes do carimbo das perguntas. K7 faz leitura simples e a porta que grava `PeriodoFechado` não compartilha essa trava, portanto a leitura não serializa o Resolver contra um trancamento concorrente. A guarda também herda uma escrita lateral: em base sem usuário `sistema`, C20 pode criá-lo por `get_or_create`. A preparação do teste já chama C20 antes da fotografia. O diff não adiciona `except`. `app/ponto/services/veredito_celula.py:192`, `app/ponto/services/veredito_celula.py:258`, `app/ponto/services/fechamento.py:1400`, `app/ponto/services/fechamento.py:1464`, `app/chamados/juizes.py:1316`, `app/core/utils.py:6`, `app/ponto/tests/test_resolver_passa_na_frente.py:216`
- **Mutantes, por inspeção.** **(1)** Os testes novos não mordem retirar o veto do calendário nem aceitar um POST de dia vetado. Os três cenários de FK mordem retirar a precedência da nova via da respondida, e o espelho cobre `declarar_dia` falta. **(2)** As asserções mordem sumir com o item, com `rotulo` ou com a nota no JSON. Não cobrem competência mais antiga, renderização, nem pergunta que perde vitalidade durante o veredito. **(3)** Mordem retirar C20 e omitir o motivo no avulso. O lote do teste é todo trancado, não confere mensagem de tela e não fotografa `FuroDiario`. **(4)** Mordem retirar a exclusão da ressuscitação e retirar a nova via do mérito. Execução dos mutantes [nao verificado]. `app/ponto/tests/test_resolver_passa_na_frente.py:60`, `app/ponto/tests/test_resolver_passa_na_frente.py:76`, `app/ponto/tests/test_resolver_passa_na_frente.py:91`, `app/ponto/tests/test_resolver_passa_na_frente.py:199`, `app/ponto/tests/test_resolver_passa_na_frente.py:223`, `app/ponto/tests/test_resolver_passa_na_frente.py:251`, `app/ponto/tests/test_resolver_passa_na_frente.py:273`, `app/ponto/tests/test_resolver_passa_na_frente.py:290`, `app/ponto/tests/test_resolver_passa_na_frente.py:309`, `app/ponto/tests/test_resolver_passa_na_frente.py:328`

## D. Migration e leitores fora do diff

**Sim, só de `choices`.** A comparação estática com a última definição anterior confirmou os mesmos atributos e todas as escolhas anteriores, com apenas `resolver_dia` ao final. A 0057 só declara `AlterField`; sua dependência 0056 contém um backfill próprio de outro campo. `app/chamados/migrations/0051_decisao_pergunta.py:109`, `app/chamados/migrations/0057_via_resolver_dia.py:9`, `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12`

**Há leitores com resultado diferente.** O juiz de lastro aceita a via nova como decisão suficiente, a relavra exclui o carimbo por identidade, e o card e o explicador usam o display da choice. O backfill `admin_decidiu_sem_ato` só audita a via antiga. Já `estado_aplicacao_pergunta` cai em `aplicada` para pergunta de hora respondida e carimbada pela nova via, mesmo sem aplicação da hora. Esse é o achado da obra separada citada no aval. `app/chamados/juizes.py:729`, `app/chamados/services/relavra_via.py:174`, `app/templates/chamados/partials/_card_atendimento.html:195`, `app/chamados/services/explicador.py:122`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:121`, `app/chamados/services/materializacao.py:879`, `app/chamados/services/materializacao.py:889`, `app/chamados/services/materializacao.py:900`

## E. Templates

**Avulso.** O campo obrigatório corresponde à porta. O texto promete encerramento imediato inclusive da respondida, mas a tela esconde o botão para a viva respondida e pode exibi-lo para dia que C20 vai recusar. “Cobrar este dia” está corretamente ligado à outra rota e dispensa a validação do motivo. Essa rota ainda pode registrar toque em dia trancado. `app/templates/ponto/partials/_veredito_dia.html:15`, `app/templates/ponto/partials/_veredito_dia.html:23`, `app/templates/ponto/partials/_veredito_dia.html:28`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/colaboradores/services/calendario.py:777`, `app/ponto/services/veredito_celula.py:265`, `app/chamados/services/cobrar_dia.py:31`

**Lote.** Os três verbos e o motivo correspondem ao POST. A barra dá a entender que os dias marcados são acionáveis, mas a seleção usa a mesma oferta sem C20. Um dia selecionável pode ser recusado, e o intervalo da reconciliação ainda pode incluí-lo. `app/templates/ponto/partials/_veredito_lote_barra.html:2`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/ponto/views.py:3520`, `app/ponto/views.py:3533`

## F. Leis dos sítios

- **Viola LEIS-UI L2 e L4.** A oferta e a ação não compartilham a elegibilidade, e a tela oferece ação onde a tranca a impede. **Viola L7** na ajuda do avulso: a promessa de encerramento foi escrita no template e diverge das condições da porta. `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:39`, `app/docs/LEIS-UI.md:77`, `app/templates/ponto/partials/_veredito_dia.html:8`, `app/colaboradores/services/calendario.py:97`, `app/ponto/services/veredito_celula.py:265`
- **Viola a disciplina de relógio da L-128** na leitura nova por pergunta. **Cumpre C21/L-130** ao reconhecer resposta no mérito e preservar o par no ato de silêncio. **Cumpre o desenho de identidade** do corte de 01:10 ao usar a via no degrau existente, sem escrever a FK. A garantia de correção posterior pelo próprio Resolver fica incompleta pela recusa administrativa descrita em B. `app/docs/PLANO-PADROES.md:97`, `app/chamados/models.py:3373`, `app/docs/LEIS.md:151`, `app/chamados/services/fio.py:1400`, `app/chamados/models.py:4440`, `app/docs/CORTES.md:628`, `app/docs/CORTES.md:642`, `app/chamados/juizes.py:1509`, `app/chamados/services/validacao.py:999`
- **L-050, L-051, L-102 e L-104.** A guarda nova usa C20 e preserva a exceção de toque anterior da L-051, sem restaurar fronteira de exportação da L-050. O hunk do calendário só comenta o veto, sem alterar a palavra do dia sem par da L-102 ou o recorte civil do holerite da L-104. O corte posterior da tela pela lavra segue como outra obra. Pouso dos pré-requisitos F0 e E4C-F5 e efeito sobre exportadas em produção [nao verificado]. `app/docs/LEIS.md:91`, `app/docs/LEIS.md:92`, `app/docs/LEIS.md:128`, `app/docs/LEIS.md:130`, `app/chamados/juizes.py:1130`, `app/colaboradores/services/calendario.py:484`, `app/docs/CORTES.md:628`, `app/docs/CORTES.md:639`, `app/docs/CORTES.md:642`

## 24/11 09:22 · sessao e77d23685b46 · A-TELA-DO-DIA-LE-UM-JUIZ · censo · origem=Code

### 1. Escritores do chamado de furo

**Há quatro caminhos que podem gravar `modulo_origem='batida_ausente'` na cópia.** Três emitem a cobrança automática do dia com categoria `Ausencia`, subcategoria `Batida nao realizada` e área `supervisao`. O quarto é a abertura web pelo colaborador, que pode gravar o mesmo módulo sem associar a solicitação ao dia do furo. O hash `32e4bd52a` é o informado no pedido. A cópia disponível veio sem metadados Git para confirmá-lo. `[nao verificado]` `app/ponto/management/commands/detectar_ausencias.py:694` `app/chamados/reconciliador.py:957` `app/chamados/reconciliador.py:1032` `app/ponto/management/commands/reconciliar_grade.py:507` `app/chamados/views.py:167` `app/chamados/services/abertura.py:47`

| Caminho de criação | Quem chama e o que grava |
|---|---|
| `detectar_ausencias.Command._abrir_chamado` | O `handle` do detector chama quando falta entrada. `emitir_furo_retroativo --apply` chama para `FURO_SEM_COBRANCA`. Ambos usam a entrada prevista `E`, com `{tipo, hora, motivo}` em `contexto_json.marcos_faltantes` quando o motivo é mapeável. `contexto_json.horario_previsto` recebe o instante ISO da âncora. O cartório também chama o mesmo método em dois ramos. No `FURO_PARCIAL`, passa todos os marcos apagados que conseguiu nomear, em ordem temporal, e usa o primeiro como âncora. No ramo de furo integral, cobrança morta ou limbo de hoje, pode cair no padrão de entrada. `app/ponto/management/commands/detectar_ausencias.py:424` `app/ponto/management/commands/detectar_ausencias.py:476` `app/ponto/management/commands/detectar_ausencias.py:694` `app/ponto/management/commands/emitir_furo_retroativo.py:116` `app/ponto/management/commands/emitir_furo_retroativo.py:158` `app/ponto/services/cartorio.py:1850` `app/ponto/services/cartorio.py:1879` `app/ponto/services/cartorio.py:1931` `app/ponto/services/cartorio.py:2032` |
| `chamados.reconciliador.reemitir_chamado_do_dia`, ramo **c2** | `reatar_pergunta_orfa` pode chegar ao c2 pelo signal de batida ou pela fábrica, quando esta cria uma pergunta do dia sem destino. O c2 grava `data_turno`, `origem='reconciliador_c2'` e, se houver, todos os apagados em `contexto_json.marcos_faltantes`, com `motivo='lampada_apagada'` e `_marco_por='lampada_da_celula'`. Lista vazia ou falha ao nomear permite que o contexto saia sem o campo. `app/ponto/signals.py:207` `app/chamados/reconciliador.py:1568` `app/chamados/reconciliador.py:1662` `app/chamados/reconciliador.py:1755` `app/chamados/services/disputa_emissao.py:468` `app/chamados/reconciliador.py:811` `app/chamados/reconciliador.py:1032` `app/ponto/services/lastro.py:127` |
| `reconciliar_grade.Command._emitir_furo` | O `handle` chama com `--apply` e sem `--so-retratar`. Grava os marcos `missing` da grade que venceram e têm motivo mapeável em `contexto_json.marcos_faltantes`, com `{tipo, hora, motivo}`, `data_turno` e `origem='grade'`. A entrada do cron na cópia roda em sombra, sem `--apply`. `app/ponto/management/commands/reconciliar_grade.py:66` `app/ponto/management/commands/reconciliar_grade.py:151` `app/ponto/management/commands/reconciliar_grade.py:250` `app/ponto/management/commands/reconciliar_grade.py:507` `app/config/crons.py:462` |
| `chamados.views.abrir_chamado` → `abrir_chamado_local` | O POST web aceita `batida_ausente` por ele constar em `MOTIVOS`. A categoria vem da opção válida do formulário ou do classificador, que usa o texto. Se o módulo for preservado, `ChamadoColaborador.criar` grava a solicitação sem `marcos_faltantes`, `horario_previsto` ou `data_turno`. A abertura nativa da API usa `app_colaborador`. `app/chamados/views.py:167` `app/chamados/views.py:196` `app/chamados/catalogo/modulos.py:105` `app/chamados/services/abertura.py:28` `app/chamados/services/abertura.py:47` `app/chamados/classificador.py:277` `app/api/views.py:2189` |

Os três caminhos automáticos convergem em `ChamadoColaborador.abrir`, com deduplicação por colaborador, módulo e `data_turno`. Um existente encerrado pode passar pela porta de renascimento. O lote do cartório, o julgamento pontual, **Cobrar este dia** e o passe `cobrar_furos_sem_cobranca` chegam ao mesmo emissor do cartório. `app/chamados/models.py:968` `app/chamados/models.py:1037` `app/ponto/management/commands/processar_cartorio.py:36` `app/ponto/management/commands/processar_cartorio.py:65` `app/ponto/services/cartorio.py:2143` `app/chamados/services/cobrar_dia.py:41` `app/ponto/services/furos_sem_cobranca.py:117`

No vocabulário mais amplo de furo, `intervalo_pendente` tem emissão automática desativada no detector de intervalo. `turno_aberto` nasce em `processar_alertas_turno`, com categoria `turno_aberto`, subcategoria `saida_pendente` e `contexto_json.entrada_timestamp`. `entrada_bloqueada_turno_aberto` nasce no POST de batida web, com categoria `turno_aberto`, subcategoria `entrada_bloqueada` e `contexto_json.{pendentes,total}`. Os passes de saída de 24h usam módulo e categoria `turno_aberto_24h`, com `data_turno` e `entrada_timestamp`. Esses são fluxos de turno ou intervalo distintos da cobrança diária da tabela. `app/chamados/catalogo/furo_modulos.py:15` `app/ponto/management/commands/detectar_intervalo_ausente.py:150` `app/ponto/management/commands/processar_alertas_turno.py:335` `app/ponto/views.py:294` `app/ponto/views.py:310` `app/ponto/management/commands/processar_alertas_avancados.py:120` `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:128` `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`

### 2. Leitores do marco gravado e efeito de uma lâmpada acesa

| Leitor | Efeito |
|---|---|
| **Lastro** | `_marcos_cobrados` lê todos os pares hora/tipo de `contexto_json.marcos_faltantes`. `marcos_acesos` dá `True` quando todos acenderam, `False` ao encontrar uma apagada e `None` quando falta identificação suficiente. `fechar_pelo_marco_colab` só tenta fechar com `True`, via `reconciliar`, e só grava a nota se fechar. `fechar_cobranca_com_lastro` mede e alarma. `app/ponto/services/lastro.py:62` `app/ponto/services/lastro.py:166` `app/ponto/services/lastro.py:401` `app/ponto/management/commands/fechar_cobranca_com_lastro.py:49` |
| **`reconciliar` e reabertura** | `premissa_regularizada` considera regularizado o chamado quando todos os marcos nomeados acenderam, mesmo que outro marco do dia continue apagado. `reconciliar` ainda passa pelo motor e pela guarda da pergunta viva. Na reabertura, premissa regularizada barra o ato. Se o emissor pediu renascimento por outros marcos, a porta examina os marcos **do pedido**, e a cobrança pode reabrir pelo marco novo ainda apagado. `app/chamados/models.py:683` `app/chamados/models.py:804` `app/chamados/models.py:988` `app/chamados/models.py:1352` `app/chamados/models.py:1414` `app/ponto/management/commands/detectar_ausencias.py:744` |
| **Cartório** | `_protesta` exclui da acusação de cobrança o chamado cujos marcos já acenderam na ata em construção. Em `apply`, tenta reconciliá-lo. Se houver fio vivo entre os chamados que deixaram de protestar, adia o lote até lavrar e rodar o carimbo das perguntas absolvidas. Um fecho pode provocar nova passada e cobrança pelo marco que continua apagado. `app/ponto/services/cartorio.py:1546` `app/ponto/services/cartorio.py:1623` `app/ponto/services/cartorio.py:1047` `app/ponto/services/cartorio.py:1073` `app/ponto/services/cartorio.py:1931` |
| **Signals e registro de batida** | Os signals de batida acionam cartório e reconciliador. O signal da pergunta alterada acorda o evento da célula, cujo consumidor chama `fechar_pelo_marco_colab`. São gatilhos. O caminho rápido do detector e as duas versões da API de batida sincronizada só tentam o fecho se `marcos_acesos_do_dia` der `True`. A batida web também pode tentar reconciliar pela guarda de premissa do chamado. `app/ponto/signals.py:131` `app/ponto/signals.py:206` `app/chamados/signals.py:252` `app/chamados/reconciliador.py:1515` `app/ponto/management/commands/detectar_ausencias.py:396` `app/api/views.py:794` `app/api/views_core.py:788` `app/ponto/services/entrada_sincronizada.py:19` `app/ponto/services/triagem_batida.py:589` |
| **Perguntas, telas e API de chamados** | `entrada_de_marco` lê o campo para identificar o marco da pergunta por motivo, com fallback por tipo só no legado. Materialização e validação o usam para tipo, hora ou comparação de resposta. O fio web usa a hora para exibir a previsão. Já o formulário de declaração do card web e `api_chamados_v2` projetam os faltantes atuais da célula. Um marco aceso sai dessa projeção, sem transição de chamado pela leitura. O espelho de gestor usa o campo gravado para o badge de chamado vivo, e o calendário indexa chamado vivo por célula ou dia. `app/chamados/juizes.py:588` `app/chamados/juizes.py:2039` `app/chamados/juizes.py:2166` `app/chamados/services/materializacao.py:567` `app/chamados/services/materializacao.py:706` `app/chamados/services/fio.py:451` `app/chamados/services/fio.py:495` `app/chamados/services/declaracao_texto.py:129` `app/api/views.py:1078` `app/escala/utils.py:1058` `app/ponto/services/espelho.py:615` `app/colaboradores/services/calendario.py:456` |
| **Pauta** | A Pauta de discordantes lê `veredito='discordante'` e a porta publicada pela tarefa do dia. Se o dia ganha fio ou muda de veredito e sai do alvo, pode marcar a **Pauta** como feita. A leitura não fecha nem reabre o chamado pelo marco gravado. `transicionar_com_pauta`, usado pelo lastro, executa o fecho delegado e entrega ao DP eventuais recusas S143 do ato. `app/ponto/services/pautas_dos_discordantes.py:40` `app/ponto/services/pautas_dos_discordantes.py:75` `app/ponto/services/pautas_dos_discordantes.py:195` `app/folha/fora_da_folha.py:70` `app/chamados/services/fecho_runtime.py:146` |

Há também leitores de manutenção do campo. `nomear_marco_por_lampada` preenche o acervo sem marco a partir de apagados. `reconciliar_grade` pode atualizar os marcos de uma cobrança órfã em `--apply`, sujeito à guarda do fio respondido e vivo. A fila de regeneração usa o campo para identificar e exibir cobranças afetadas por reescrita. A emissão de disputa verifica se o chamado passado nomeia marcos e então lê os faltantes da ata para formular perguntas. `app/chamados/management/commands/nomear_marco_por_lampada.py:61` `app/chamados/management/commands/nomear_marco_por_lampada.py:105` `app/ponto/management/commands/reconciliar_grade.py:442` `app/ponto/management/commands/reconciliar_grade.py:461` `app/escala/services/regeneracao.py:141` `app/escala/services/regeneracao.py:186` `app/chamados/services/disputa_emissao.py:744` `app/chamados/services/disputa_emissao.py:1180`

### 3. Predicado de fechamento e efeito sobre um eventual marco “sem par”

O fecho reativo do lastro exige um `batida_ausente` na fila, dia identificável, célula gravada e `marcos_acesos(ch, cel) is True`. O casamento é por **hora e tipo** para todos os marcos nomeados. A seguir, `reconciliar()` precisa aceitar o fecho. Já o vigia `julgar` tenta casar uma batida do mesmo tipo dentro da tolerância quando falta a lâmpada correspondente e usa `reconciliaria()` para classificar o resultado, sem fechar. `app/ponto/services/lastro.py:82` `app/ponto/services/lastro.py:183` `app/ponto/services/lastro.py:284` `app/ponto/services/lastro.py:312` `app/ponto/services/lastro.py:329` `app/ponto/services/lastro.py:401` `app/chamados/models.py:661`

**O efeito de criar um chamado para a sobra depende de como ela for gravada.**

- Se a sobra for gravada como um par hora/tipo que coincide com uma lâmpada já acesa, o lastro pode fechar a cobrança na primeira avaliação, se o gate permitir. O predicado não consulta se aquela batida ainda está sem par. `app/ponto/services/lastro.py:183` `app/ponto/services/lastro.py:407` `app/chamados/models.py:702`
- Se o marco for gravado com hora e tipo próprios, ausentes de uma ata com lâmpadas, o fecho por **marco aceso** não entra. Mas `marcos_inexistentes` pode dar `True`. O cartório tenta reconciliar por esse outro caminho no julgamento, e `premissa_regularizada` aceita essa condição. Um marco sintético ausente da ata, portanto, também pode ser encerrado. `app/ponto/services/lastro.py:98` `app/ponto/services/lastro.py:187` `app/ponto/services/cartorio.py:1651` `app/chamados/models.py:1363`
- Se a sobra estiver apenas em outro campo, sem marco conferível em `marcos_faltantes`, o caminho por marco do lastro não fecha. Na cópia não há formato específico de cobrança da sobra nesses predicados. `app/ponto/services/lastro.py:62` `app/ponto/services/lastro.py:110` `app/ponto/services/lastro.py:183` `app/ponto/services/lastro.py:286`

### 4. Retenção em `folha/export.py`

- **`_dias_sem_decisao`** tem dois chamadores diretos. `motivos_retencao_celula` devolve os motivos e, opcionalmente, os dias retidos. `informativo_rescisao` usa os dias para informar o DP sobre desligados. A retenção chega a `classificar_export` por `motivos_retencao_espelho` e marca `fora / furo_espelho`. Prévia, Fora da Folha e vigia da pausa também consomem a lista de dias pelo mesmo ponto. `app/folha/export.py:1220` `app/folha/export.py:1229` `app/folha/export.py:1316` `app/folha/export.py:968` `app/folha/previa.py:170` `app/folha/fora_da_folha.py:120` `app/folha/services/vigia_pausa.py:125`
- **`nao_retem_pela_pausa`** é lido por `_dias_sem_decisao` e pelo vigia da pausa. Exige o conjunto exato `{'FURO_PARCIAL'}`, pelo menos um papel apagado e todos os apagados pertencentes à pausa. Ata desalinhada, em aberto ou indeterminada falha no predicado. `app/folha/export.py:1148` `app/folha/export.py:1173` `app/folha/export.py:1308` `app/folha/services/vigia_pausa.py:62` `app/folha/services/vigia_pausa.py:127` `app/folha/services/vigia_pausa.py:157`
- **`nao_retem_sem_chamado`** é lido por `_dias_sem_decisao`. Exige o conjunto exato `{'FURO_PARCIAL'}` e ata com pelo menos uma lâmpada, todas com `acesa is True`. Ata vazia, lâmpada desconhecida ou protesto misto não ganham essa dispensa. `app/folha/export.py:1176` `app/folha/export.py:1308` `app/ponto/services/lastro.py:138`
- **A existência ou vitalidade do chamado não é condição da retenção.** O ramo do veredito acusador usa os protestos e as duas dispensas. Assim, `FURO_PARCIAL` puro com tudo aceso é dispensado mesmo se houver chamado vivo. Com saída apagada, por exemplo, o dia pode reter sem chamado algum. O teste de retenção cria só a célula e exige `espelho_furo` para esse caso. `app/folha/export.py:1260` `app/folha/export.py:1286` `app/folha/export.py:1308` `app/folha/tests/test_a_tela_furo_sem_marco_nao_retem.py:52` `app/folha/tests/test_a_tela_furo_sem_marco_nao_retem.py:61`

### 5. Detecção e gravação da batida sem par

O pareador entrega um `Turno` aberto se há entrada e falta saída. Uma saída órfã é um `Turno` com saída e sem entrada. `sem_par_do_pareador` reduz esses objetos a dois booleanos por `data_turno`. O cartório carrega os turnos da mesma janela usada por `dias_encerrados`, só passa o sinal quando o turno do dia está encerrado e aplica o horizonte de competência `2026-11` em diante. `app/ponto/turnos.py:234` `app/ponto/turnos.py:312` `app/ponto/turnos.py:1495` `app/ponto/supra_juiz.py:86` `app/ponto/services/cartorio.py:316` `app/ponto/services/cartorio.py:333` `app/ponto/services/cartorio.py:1208` `app/ponto/services/cartorio.py:1223`

`supra_juiz.classificar_dia` gera `FURO_PARCIAL` no ramo sem marco `missing` quando o dia de trabalho encerrou, há previsto, o déficit supera 10 minutos e há `E` sem par ou `S` órfã com realizado positivo. Também exige âncora, ausência aprovada que não esteja cobrindo esse ramo e dia sem justificativa. Sem vínculo, o ramo gera `PREVISTO_COM_DIA_INDEFINIDO`. O cartório passa os sinais, conserva apenas os códigos, chama `lavrar_veredito` e persiste `CelulaDia.protestos` e a ata pelo carimbo. `app/ponto/supra_juiz.py:253` `app/ponto/supra_juiz.py:507` `app/ponto/motor_calculo_v2.py:51` `app/ponto/services/cartorio.py:1553` `app/ponto/services/cartorio.py:1953` `app/ponto/portas/celula.py:1104` `app/ponto/portas/celula.py:1126` `app/ponto/portas/celula.py:1235`

**O `pk` e o instante completo estão disponíveis no objeto `Batida` durante o pareamento.** O caminho do protesto perde essa identidade ao reduzir os turnos a `(E, S)` booleanos. A impressão guarda um hash que inclui as batidas da janela e o sinal `E`, `S` ou `ES`. `ata.orfas` guarda horários `HH:MM` das sobras do casamento da grade, sem `pk` e sem vínculo explícito com a batida que produziu o sinal do pareador. O protesto lavrado, sozinho, não identifica a sobra por `pk` ou instante. `app/ponto/turnos.py:43` `app/ponto/turnos.py:312` `app/ponto/turnos.py:1868` `app/ponto/models.py:23` `app/ponto/supra_juiz.py:99` `app/ponto/services/cartorio.py:522` `app/ponto/services/cartorio.py:601` `app/ponto/services/cartorio.py:1233` `app/ponto/services/cartorio.py:785` `app/escala/utils.py:673` `app/ponto/services/cartorio.py:1553`

### 6. `col348` e os guardas que deixam furo apagado sem chamado

Os 21 dias são a premissa do item 6. O anexo fornecido não traz as linhas por dia, o vínculo, `CelulaDia.trabalha`, a ata ou o resultado dos vetos. Qual guarda ocorreu em cada um dos 21 dias fica `[nao verificado]`.

**O guarda de escala que basta por si só é `CelulaDia.trabalha=False`.** O vínculo lê esse valor, o detector pula em `if not resultado_dia`, o cartório retorna `None` em `_marcos_do_furo_parcial` porque exige `eh_dia_trabalho(...) is True`, e o funil ainda tem `VETO_CELULA_FOLGA` se for chamado. O regime intermitente responde `False` no cálculo do template, enquanto a grade tem um caminho próprio para dia intermitente com batida. Essa combinação pode produzir dia com fato e marco posicional apagado sem passar pelo emissor. A folha retém pelo veredito e pelas exceções do laço, sem exigir `trabalha=True` no ramo acusador. `app/escala/models.py:820` `app/escala/models.py:1098` `app/escala/models.py:1330` `app/escala/utils.py:1614` `app/escala/utils.py:1784` `app/ponto/management/commands/detectar_ausencias.py:306` `app/ponto/services/cartorio.py:2006` `app/ponto/marcos.py:155` `app/folha/export.py:1286`

Os demais guardas relevantes estão nestes pontos:

- **No `handle` do detector**, o universo é de colaboradores em operação, com rollout e vínculo vigente hoje. O laço pula isento, data fora de hoje/ontem, dia sem vínculo ou sem previsão de trabalho, sem início previsto, sem marco vencido ou com turno já encerrado. Também pula ausência que cobre, substituição aplicável, entrada encontrada para o dia-turno, chamado já existente pela chave do dia e `--dry-run`. Em particular, uma entrada existente basta para o detector não abrir chamado por saída ou pausa faltante. `app/colaboradores/queries.py:50` `app/ponto/management/commands/detectar_ausencias.py:97` `app/ponto/management/commands/detectar_ausencias.py:137` `app/ponto/management/commands/detectar_ausencias.py:306` `app/ponto/management/commands/detectar_ausencias.py:337` `app/ponto/management/commands/detectar_ausencias.py:347` `app/ponto/management/commands/detectar_ausencias.py:364` `app/ponto/management/commands/detectar_ausencias.py:396` `app/ponto/management/commands/detectar_ausencias.py:424` `app/ponto/management/commands/detectar_ausencias.py:461`
- **No cartório**, pode não haver nova avaliação, por exemplo com impressão igual sem `forcar`. No ramo parcial, o portão exige o protesto, colaborador não desligado, piso de competência ou toque, ausência de afastamento agora, marco nomeável, modo `apply`, nenhum chamado vivo do dia e ausência da condição `parcial_so_lista`. Esta última cobre parcial sem período ou parcial já julgada sem fato novo. Mesmo com lâmpada apagada, a extração pode ficar vazia se faltar vínculo, se o vínculo não disser trabalho, ou se hora/tipo não tiverem instante na cadeia e motivo mapeável. Lâmpada pendente também fica fora. `app/ponto/services/cartorio.py:1491` `app/ponto/services/cartorio.py:1830` `app/ponto/services/cartorio.py:1916` `app/ponto/services/cartorio.py:465` `app/ponto/services/cartori

[truncado em 20000 de 26687 caracteres]

## 24/11 08:38 · sessao e6153ad48c0f · CLASSE-D-NA-FRENTE-DA-E4D · censo · origem=Code

No HEAD `7a4e79eefb7bd2613e4826449b0b22a43401cfb5`, o **C5 ainda emite um item por colaborador**, sem `data` e sem `dono_do_dia`. O alcance do vigia ainda usa `empresas_visiveis()`, e o juiz `dono_da_divergencia` ainda não recebe `FATO_EM_AUSENCIA`. `app/escala/services/cadastro_realidade.py:176-182,1079` · `app/chamados/services/vigia_fio.py:517-520` · `app/ponto/management/commands/e6_oraculo.py:53-54,98-110`

Comparei a cópia com os dois objetos Git e consultei o histórico no repositório de origem, cujo `HEAD` coincide com o hash pedido. As contagens e os casos individuais em produção ficam `[nao verificado]`.

## 1. Citações das seções 1, 3 e 4

O prefixo `app/` está implícito nas duas tabelas. Em “mudou?”, a comparação é com o código de `77dfa94fa`.

| citacao_antiga | hoje | mudou? |
|---|---|---|
| `escala/services/cadastro_realidade.py:78,137-173,176-182` (inclui `:180`) | `escala/services/cadastro_realidade.py:78,137-173,176-182` | **Não.** O coletor traz datas por colaborador. O formatador C5 emite um item por colaborador e põe até três datas no texto. |
| `escala/services/cadastro_realidade.py:342-355,358-376,383-397` | `escala/services/cadastro_realidade.py:342-355,358-376,383-397` | **Não.** C9 e C10 mantêm os itens datados. C12 mantém um item por publicação do motor, inclusive com `data=None` se a origem não trouxer dia. |
| `escala/services/cadastro_realidade.py:414` | `escala/services/cadastro_realidade.py:414-530`, em especial `:497-510` | **Sim.** Além do caminho por âncora terminal, o C11 aceita M1, com todas as vivas respondidas no horizonte e sem âncora. Os dois caminhos exigem folga não definida. |
| `escala/services/cadastro_realidade.py:450-453` | `escala/services/cadastro_realidade.py:452-455` | **Não.** A consulta mantém a seleção do colaborador ativo, veredito `indefinida`, protesto e janela. |
| `escala/services/cadastro_realidade.py:469` | `escala/services/cadastro_realidade.py:470-473` | **Sim, pelo retorno consultado.** O `if` é idêntico, mas agora também exclui o envelope de leitura desconhecida sem rota. `chamados/services/tarefa_do_dia.py:809-822` |
| `escala/services/cadastro_realidade.py:503-507,512` | `escala/services/cadastro_realidade.py:513-517,522` | **Não.** A montagem do dono continua com `sem_motor=True` e CxR lavrado. O item continua gravando o ISO da célula e o retorno de `.dono(d)`. |
| `escala/services/cadastro_realidade.py:671` | `escala/services/cadastro_realidade.py:682-684` | **Não.** A ação C5 ainda aponta a `TELA_COLAB` para adesão ou lançamento de ausência. |
| `escala/services/cadastro_realidade.py:768` | `escala/services/cadastro_realidade.py:860-868,1059-1073` | **Sim.** `lista()` passou a publicar também `linhas` ordenadas pela carga e `carga_resumo`. `postos` segue no retorno. |
| `escala/services/cadastro_realidade.py:857-863,866` | `escala/services/cadastro_realidade.py:956-962,965` | **Não.** A inclusão de C5 segue sob `try/except`, antes da chamada a `um_dono_por_dia`. |
| `escala/services/cadastro_realidade.py:885-916` (inclui `:887-904,910-912`) | `escala/services/cadastro_realidade.py:996-1027` (ramos `:998-1015`, contador `:1021-1023`) | **Não.** Só C9–C12 preenchem `_extra['dono']`. Incluir C5 apenas no conjunto faria o acesso em `:1023` levantar `KeyError` fora do `try` do produtor. |
| `escala/services/cadastro_realidade.py:941-947` | `escala/services/cadastro_realidade.py:1052-1058` | **Não.** `colabs` conta IDs distintos e `ocorrencias` conta itens incluídos. Para o C5 atual, os números coincidem. `escala/services/cadastro_realidade.py:180,1025-1027` |
| `escala/services/cadastro_realidade.py:963,966-1011` | `escala/services/cadastro_realidade.py:1079,1082-1127` | **Não.** Mesmo conjunto C9–C12 e mesma reconciliação de donos divergentes por dia. |
| `escala/services/cadastro_realidade.py:1014-1022,1025-1045` (inclui `:1042`) | `escala/services/cadastro_realidade.py:1130-1138,1141-1161` (guarda em `:1158`) | **Não.** A projeção exige `data` preenchida. O nível do juiz tem precedência. Conflito no nível escolhido dá `None`. |
| `chamados/services/vigia_fio.py:188-255` (inclui `:222-228,230-236,248-251`) | `chamados/services/vigia_fio.py:166-233` (métodos em `:200-206,208-214,226-229`) | **Não.** A classe conserva as consultas separadas de dia, colaborador e dono da mesa. |
| `chamados/services/vigia_fio.py:274-315` (inclui `:313`) | `chamados/services/vigia_fio.py:240-263` (`fio_do_admin` em `:261-262`) | **Sim.** O ramo `decisao_do_dia_pendente` saiu da função. `Resolver dia` passou a ser uma porta publicada pela tarefa. `chamados/services/tarefa_do_dia.py:781-790` |
| `chamados/services/vigia_fio.py:542,592` | `chamados/services/vigia_fio.py:487,537` | **Sim.** O alcance hoje testa a `rota` publicada, contabiliza `porta_acao` e passa o `hoje` da rodada a `dias_que_seguram`. `chamados/services/vigia_fio.py:595-609` |
| `chamados/services/vigia_fio.py:621,654` | `chamados/services/vigia_fio.py:566,598` | **Não.** `ps` continua vindo das vivas da célula em `alim['vivas']`. |
| `chamados/services/vigia_fio.py:696,700-703` | `chamados/services/vigia_fio.py:640-647` | **Não.** Os predicados de d e de `dia_na_lista_dono_*` são os mesmos. A porta e a causa c, avaliadas antes, mudaram. `chamados/services/vigia_fio.py:595-609,635-639` |
| `chamados/services/vigia_fio.py:715,737` | `chamados/services/vigia_fio.py:659,681` (`ROTULO_CAUSA` em `:483-484`) | **Não.** Mesmo incremento por causa e emissão de seu rótulo. |
| `chamados/management/commands/vigiar_fio_morto.py:175,181` | `chamados/management/commands/vigiar_fio_morto.py:184,190` | **Não.** O comando ainda passa a mesma `ListaDoAdmin` e o mesmo instante aos dois censos. |
| `chamados/management/commands/backfill_fio_morto_e4b.py:78` | `chamados/management/commands/backfill_fio_morto_e4b.py:78-79` | **Não.** `_alcance` ainda chama o censo real e seleciona as linhas `sem_porta`. |
| `chamados/management/commands/disparar_perguntas_competencia.py:56` | `chamados/management/commands/disparar_perguntas_competencia.py:56-57,68-71` | **Não.** O contador desdobra o dict em pares `(cid, dia)`. |
| `folha/vigia_retido_calado.py:43` | `folha/vigia_retido_calado.py:54-58,71-80` | **Sim.** Agora chama com os IDs visíveis intersectados com o recorte, sem a guarda antiga de existência de retrato. Tanto antes quanto hoje, `set(dict)` usa as **chaves de colaborador**. |
| `chamados/services/pergunta_no_app.py:35` | `chamados/services/pergunta_no_app.py:35` | **Não.** O motivo `celula_nunca_bateu` continua apontando a lista C5 do admin. |
| `chamados/services/tarefa_do_dia.py:638,648` | `chamados/services/tarefa_do_dia.py:809-822,825-827` | **Sim** para `porta_da_tarefa`, que inclui ações diretas e envelope de desconhecimento sem rota. **Não** para o delegador `carga_da_tarefa`. |
| `chamados/juizes.py:1595` | `chamados/juizes.py:1723,1748-1761` | **Sim** na carga. Passou a trazer decisões em lote. Continua publicando as vivas pelo juiz. |
| `folha/fora_da_folha.py:109` | `folha/fora_da_folha.py:125-140,165` | **Sim.** `dias_que_seguram` exige `hoje` da borda para a leitura dos dias. A seleção dos retidos ainda chama `classificar_export` em `:147`. |
| `ponto/management/commands/e6_oraculo.py:98-110,113-195` (inclui `:182-195,193-194`) | `ponto/management/commands/e6_oraculo.py:98-110,113-195` | **Não.** Mesmas precedências e mesmo `None` para o residual que dependeria de CxR quando `sem_motor=True` e a lavra falta. |
| `ponto/motor_calculo_v2.py:1623,1997,1528` | `ponto/motor_calculo_v2.py:1623,1997,1528` | **Não.** Os dois primeiros publicam dia do turno. `:1528` continua tomando `entrada.date()`. |
| `test_MORDE_F5_queries_nao_crescem_com_os_dias`, `escala/tests/test_f3_beco1_folga_na_lista.py:351` | `escala/tests/test_f3_beco1_folga_na_lista.py:395` | **Não** quanto ao contrato medido pelo teste, que segue sendo custo por número de dias do C11. |

## 2. Escritores e leitores no HEAD

| Dado | Escritores e leitores |
|---|---|
| `item['data']` | **Escrevem** C9 `escala/services/cadastro_realidade.py:350`, C10 `:395`, C11 `:522` e C12 `:374`. **Leem** o encaminhamento C11 `:1010` → `:714-719`, `um_dono_por_dia` `:1100-1101` e `dias_na_lista` `:1158-1160`. C11 usa `CelulaDia.data`; C9 publica `data_turno`, e C12 copia o dia publicado pelo motor, quando presente. `ponto/services/teto_na_lista.py:68-79` · `ponto/services/cxr_na_lista.py:84-103` |
| `item['dono_do_dia']` | **Escrevem** C9 `escala/services/cadastro_realidade.py:351`, C10 `:396`, C11 `:522` e C12 `:375`. **Reescreve** `um_dono_por_dia` em `:1125`, quando há divergência. Ele lê os candidatos em `:1104-1108`. O laço lê o dono para ação e exibição em `:998-1023`. `dias_na_lista` lê o valor cru em `:1160`. Tela e PDF exibem o `dono` formatado. `templates/escala/cadastro_x_realidade.html:31` · `escala/services/cadastro_realidade.py:1203-1206` |
| `dias_na_lista` | **Produz** `{(cid, ISO): dono\|None}` em `escala/services/cadastro_realidade.py:1141-1161`. O leitor de produção é `ListaDoAdmin._carregar`, `chamados/services/vigia_fio.py:189-196`. |
| `ListaDoAdmin` | **Preenche** `_dias` e `_colabs` em `chamados/services/vigia_fio.py:183-198` e registra dias sem resposta em `:200-233`. **Instanciam** o comando `chamados/management/commands/vigiar_fio_morto.py:184` e os dois censos quando chamados sem instância, `chamados/services/vigia_fio.py:337,580`. **Leem** `dono_fora_do_fio` em `:259-262`, as causas d/e em `:622,640-647` e o comando, que lê `sem_resposta` em `chamados/management/commands/vigiar_fio_morto.py:203`. |
| `censo_alcance`, d e e | **Escreve** d para célula sem viva, com `dia_na_lista == (False, None)` e colaborador presente, depois das exclusões anteriores. **Escreve** e para `sem_celula` ou o residual, incluindo dia datado sem viva com dono `ESTRUTURA` ou `None`. Incrementa `alcance_d/e`, `alcance_e_detalhe` e publica a linha em `chamados/services/vigia_fio.py:616-662`. **Leem** `linhas_alcance` em `:666-684`, o comando que imprime e carimba o agregado em `chamados/management/commands/vigiar_fio_morto.py:190,215,284` e o backfill que recolhe as linhas `sem_porta` em `chamados/management/commands/backfill_fio_morto_e4b.py:76-79,313-326`. A chave do balde no alcance é `alcance_fio_do_admin`. `chamados/services/vigia_fio.py:481-482,526-527,616-621` |
| `colabs_nunca_bateu` | **Produz** `{cid: [date, ...]}` a partir de `CelulaDia.veredito='nunca_bateu'`, da janela anterior até ontem, excluindo desligados. **Leem** `_nunca_bateu`, o contador de disparo por par e o vigia da Folha por colaborador. `escala/services/cadastro_realidade.py:137-182` · `ponto/services/cartorio.py:1317-1321` · `chamados/management/commands/disparar_perguntas_competencia.py:52-71` · `folha/vigia_retido_calado.py:71-80` |
| `DONO_PELO_JUIZ` | **Define** `frozenset({'C9','C10','C11','C12'})` em `escala/services/cadastro_realidade.py:1079`. **Leem** o contador `por_dono` em `:1021-1023`, a seleção para reconciliação em `:1100-1101` e os dois níveis de `_dono_resolvido` em `:1135-1138`. |

**`Empresa.em_rollout`.** Leitura direta no formulário `templates/colaboradores/empresa_form.html:34`, nos recortes `colaboradores/views.py:2630` e `chamados/services/painel.py:411`, no queryset `colaboradores/queries.py:65` e nas consultas `escala/management/commands/apurar_furos_diarios.py:9`, `ponto/services/furos_sem_cobranca.py:59` e `relatorios/management/commands/selo_leitores_no_mesmo_numero.py:51`. A borda do cadastro recebe a caixa e encaminha o valor nas operações de atualização e criação. `colaboradores/models.py:45` · `colaboradores/views.py:513,539-542,550-556`

**`empresas_visiveis()`.** O helper filtra `ativa=True` e exclui CNPJ iniciado por `11222333`. Chamadas de produção encontradas por arquivo. `colaboradores/queries.py:78-87`

- **Vigias, Folha e backfill.** `chamados/services/vigia_fio.py:191,517` · `chamados/management/commands/backfill_fio_morto_e4b.py:183,301` · `folha/vigia_retido_calado.py:54` · `folha/retrato_folha.py:1411` · `folha/views_retrato.py:149` · `folha/management/commands/lavrar_retrato_folha.py:51` · `folha/views.py:36,89,462,493`
- **Cadastro, escala, chamados e ponto.** `colaboradores/views.py:124,499,716,737,1002,1074,1124,1263,1787,1901,2221,2739,2977` · `escala/views.py:492,524` · `escala/views_plano_folgas.py:37` · `chamados/services/painel.py:438,544` · `chamados/views.py:763,1492` · `ponto/views.py:598,668,807,933,1130,1775`
- **Demais chamadas.** `api/views_mensageria.py:1222` · `comunicados/views.py:122` · `core/services/painel_op.py:66` · `core/views.py:767` · `core/views_config.py:399,486` · `core/views_usuarios.py:118` · `ferias/views.py:50` · `holerite/views.py:57,799` · `inteligencia/views.py:61` · `relatorios/views.py:43,118,169,238,326,377,426,589,671,1165,1499,1501,1556,1608` · `relatorios/views_pend.py:15`

## 3. Complementos e estado da cura

| Complemento | Sítios do HEAD e o que já existe |
|---|---|
| **CORTE-LU2-07-EMPRESA-1** | O sítio do recorte é `chamados/services/vigia_fio.py:517-520`, antes da iteração por empresa e competência em `:534-543`. A autoridade já existe em `colaboradores/models.py:45` e `templates/colaboradores/empresa_form.html:34-35`. **O recorte ainda não usa o campo.** Essas linhas do vigia vêm de `fc9beecea`, anterior à base. O mesmo `censo_alcance` é chamado pelo backfill em `chamados/management/commands/backfill_fio_morto_e4b.py:76-79`. A lista do admin tem seu próprio cruzamento em `chamados/services/vigia_fio.py:189-196`. Alterar o helper global alcançaria a lavra da Folha em `folha/management/commands/lavrar_retrato_folha.py:51`. O registro separado de `EMPRESAS=(2,3,4)` corresponde a `config/crons.py:17`. O anexo documenta 46 como subconjunto de 80. A recontagem no HEAD é `[nao verificado]`. |
| **1711-1005 item 6, `SEM_LEITURA`** | A cadeia a conferir para col265/col407 é `chamados/models.py:4377-4409` → `chamados/services/tarefa_do_dia.py:430-434,154-155,340-347` → `chamados/management/commands/backfill_fio_morto_e4b.py:98-100,158-161`. O backfill **já tem** a escrita pela porta `registrar`, anterior à base (`1250dbd98`), e `registrar` persiste a sequência em `chamados/models.py:4551-4576`. Desde a base, `d948d2ab5` e `20f5a0338` acrescentaram transporte e contagem de sem leitura. As testemunhas atuais são `chamados/services/vigia_fio.py:599-602,683`, `folha/fora_da_folha.py:215-238`, `folha/retrato_folha.py:1244` e `folha/vigia_retido_calado.py:84-100,115-117`. O anexo identifica a origem e tira col838 do recorte histórico. A linha faltante, a causa histórica da lacuna, o estado atual dos dois casos e qual alteração adicional seria necessária ficam `[nao verificado]`. |
| **1711-1050 item 6, `FATO_EM_AUSENCIA`** | Os dois sítios de implementação são **o juiz e seus insumos**, `ponto/management/commands/e6_oraculo.py:53-54,98-110,182-195`, e **o produtor e encaminhamento diário**, `escala/services/cadastro_realidade.py:928-965,996-1027,1079-1127`. A referência antiga `:1047` cai hoje na linha em branco `:1163`, entre `dias_na_lista` e `pdf_bytes`. A classificação já existe em `ponto/supra_juiz.py:395-418,450-457`, com mapeamento em `escala/models.py:1622-1631`. A tarefa já lê esse veredito numa faixa específica e emite `VALIDAR` para admin sem âncora nessa faixa. Esse ramo antecede a base e a porta é resolvida separadamente. `chamados/services/tarefa_do_dia.py:358-370,751-756`. **A entrega do item 6 ainda falta:** o juiz segue sem esse insumo e o laço da lista segue sem esse produtor. A edição de ausência e a retratação já têm portas em `ponto/views.py:2930-2937,2973,3453-3462`; a retratação anota e preserva a batida em `ponto/registro_batida.py:287-302`. |

Os leitores do juiz a cruzar para `FATO_EM_AUSENCIA` são `ponto/services/teto_na_lista.py:72,86`, `ponto/services/cxr_na_lista.py:99-103`, `ponto/management/commands/censo_fase_12x36.py:280-289`, `ferias/management/commands/listar_ferias_com_batida.py:53-57` e o próprio comando `ponto/management/commands/e6_oraculo.py:328,378`. `pautas_discordantes` tem uma chamada **direta** ao juiz para decidir se precisa do motor, além da chamada final via `.dono()`. `ponto/management/commands/pautas_discordantes.py:100-106,118`

## 4. Commits desde `77dfa94fa`

Hashes na ordem do `git log --oneline 77dfa94fa..HEAD -- <arquivo>`, consultado por caminho. Os números de linha identificam os sítios acima.

| Arquivo no HEAD | Commits | [nao verificado]
|---|---|
| `escala/services/cadastro_realidade.py:176,956,996,1079` | `a81f888dc` `d0210a400` `550c810b6` `4536fe161` `106920782` `a5ff3627f` `d948d2ab5` |
| `chamados/services/vigia_fio.py:517,640` | `57582ee14` `a65c4b712` `b9acb6828` `96d16af7d` `20f5a0338` `d948d2ab5` `c5a1bf90c` `b8681db0e` `f39d24d58` |
| `ponto/management/commands/e6_oraculo.py:53,182` | **Nenhum.** |
| `chamados/management/commands/backfill_fio_morto_e4b.py:98,161` | `ec46c1d4f` `67f5606ee` `568c14c0a` `b240b2f52` `57582ee14` |
| `chamados/services/tarefa_do_dia.py:154,363,433` | `eaf69fe33` `0dc6512a8` `c983ccc90` `0ee6521f5` `8542d4e72` `dae3bfca0` `96d16af7d` `57c1c9b94` `efcf3937a` `20f5a0338` `af1df2fe8` `a5ff3627f` `d948d2ab5` `517b6f16c` `c5a1bf90c` `b8681db0e` `f39d24d58` |
| `chamados/models.py:4377,4551` | `81c096c32` `4346eddae` `b240b2f52` `a1b5eafbe` `928c5cad1` `dfffac9b1` `131f1f6cf` `55f6689a0` `af495e9ab` `38da8c04d` `55d268b05` `05cb513bb` `c2601465f` `c9a67ed52` `f1e9c487e` `23e80acde` `d34dbfc84` `a3c23d10b` `9d386d1b1` `acdcd556d` `0db6afc73` `18bf86f01` `9500f8063` `41620774b` `5d9e1b0e4` `f54d7d15a` `dae3bfca0` `afda61fbd` `21f1e9e49` `e1386f13c` `a65c4b712` `b9acb6828` `f90ead1e0` `fbac30539` `0340fc1ca` `1fcb5d0b6` `d0cf621ac` |
| `templates/escala/cadastro_x_realidade.html:20,31` | `cff719f7d` `550c810b6` `106920782` |
| `escala/tests/test_f3_beco1_folga_na_lista.py:395` | `57c1c9b94` `efcf3937a` `6fb74c6d2` `714a92a61` `a5ff3627f` `d948d2ab5` `c5a1bf90c` `b8681db0e` |
| `escala/tests/test_cadastro_x_realidade.py:53,70` | **Nenhum.** |
| `chamados/tests/test_o261e_f5_alcance.py:260,308` | `a65c4b712` `efcf3937a` `20f5a0338` `714a92a61` `b8681db0e` `f39d24d58` |
| `chamados/tests/test_e4_backfill.py:151-162` | `c5a1bf90c` |

Entre os leitores do juiz citados no complemento, houve commits em `ponto/management/commands/pautas_discordantes.py:100-118` (`57c1c9b94`, `20f5a0338`, `a5ff3627f`) e `ponto/management/commands/censo_fase_12x36.py:280-289` (`0dd5f8323`). O histórico por caminho ficou vazio para `ponto/services/teto_na_lista.py:72`, `ponto/services/cxr_na_lista.py:99`, `ferias/management/commands/listar_ferias_com_batida.py:53` e a fonte de classificação `ponto/supra_juiz.py:395-418`.

## 5. Contra-exemplos

- **Algum produtor já emite C5 por dia? Não.** A busca de C5 no produtor encontra o cadastro da assinatura, `_nunca_bateu`, a ação e a ligação em `lista()`. O único ponto de emissão é `out[cid] = [{...}]`. Os pares produzidos pelo contador de disparo vêm do dict de datas e ficam no contador. `escala/services/cadastro_realidade.py:78,176-182,682-684,956-962` · `chamados/management/commands/disparar_perguntas_competencia.py:56-71`
- **Algum leitor de `por_ass['C5']['ocorrencias']` em tela ou PDF supõe unidade COLAB? Não encontrei leitor desse campo nesses dois caminhos.** A etiqueta da tela lê explicitamente `por_assinatura[*].colabs`. Na linha, tela e PDF leem primeiro `detalhe`, com fallback para `ocorrencias` **do item**. O C5 atual tem `detalhe`, e `lista()` atribui `ocorrencias=1` a cada assinatura de cadastro. A previsão do brief de mudança no número exibido na tela e no PDF não se confirma para o agregado `ocorrencias` no HEAD. `templates/escala/cadastro_x_realidade.html:20,31` · `escala/services/cadastro_realidade.py:180-181,1018-1020,1052-1058,1197-1206`

## 24/11 04:07 · sessao 0d2157c46e78 · RESOLVER-PASSA-NA-FRENTE · verificador · origem=Code

**Reprovado.** O Resolver pode lavrar o dia e deixar uma pergunta viva. Quando consegue encerrá-la, a pergunta some do histórico do app e a API normalmente oculta a única nota que diz “Resolvido pelo supervisor”. `app/ponto/services/veredito_celula.py:198–223,381–399` · `app/chamados/juizes.py:1502–1528` · `app/chamados/services/pergunta_no_app.py:45–92` · `app/api/views.py:62–65,1118–1127`

**PROCEDENTE** indica falha confirmada por leitura. A revisão usa o diff fornecido e os arquivos da cópia. Identidade Git da cópia, execução dos testes, tela nativa e incidência em produção [nao verificado]. [nao verificado]

1. **PROCEDENTE — escritores e leitores.** `fechar_pelo_resolver` chama `carimbar_identidade`, que grava `via_resolucao`, `materializada_em` e `materializada_por`. `fechar_o_fio_do_dia` registra `SILENCIO` por `DecisaoPergunta.registrar`, com autor, instante do carimbo, resposta preservada e `ato_motivo`. O motivo mora em **`DecisaoPergunta`**. A célula recebe `veredito` e `veredito_via='admin'` pela porta `lavrar_veredito`. `app/chamados/models.py:3360–3368,3410–3414,3526–3542,4334,4575–4586` · `app/ponto/services/veredito_celula.py:160–170,214–223,389–390` · `app/ponto/portas/celula.py:1128–1139`

   C6 e `q_vivas` conhecem a via pelo conjunto que vence a resposta. O fio filtra as disputas fechadas por essas autoridades e ainda pode mostrar a pergunta de uma disputa aberta. O card usa o rótulo das choices e o modal lê autor, data e motivo da decisão. O auditor conta por valor de via, o juiz do lastro aceita a identidade sem exigir batida, e a relavra sem lastro poupa a via. A célula é lida pelo C6 e pela marca de decisão humana do calendário. `app/chamados/juizes.py:729–742,1494–1537` · `app/chamados/models.py:3746–3771,4183–4188` · `app/chamados/services/fio.py:296–322` · `app/templates/chamados/partials/_card_atendimento.html:195` · `app/templates/chamados/partials/modal_fio.html:212` · `app/chamados/management/commands/auditar_invariantes_chamados.py:197–209,435` · `app/chamados/services/relavra_via.py:174–184` · `app/colaboradores/services/calendario.py:446,762` · `app/ponto/services/veredito_celula.py:105–125`

   **Dois leitores falham.** A testemunha de aplicação do fio chama de `aplicada` uma resposta de hora carimbada por `resolver_dia`, inclusive se o veredito foi `faltou` e nenhuma batida foi plantada. E `merito_perdedor` protege exclusivamente `admin_decidiu` com resposta. Se uma `resolver_dia` respondida chega como perdedora e os demais vetos liberam, o plano a manda para retratação e a porta troca sua via para `duplicata`. O backfill de `admin_decidiu` sem ato também é nominal, mas o fluxo normal do Resolver já grava seu próprio ato. `app/chamados/services/materializacao.py:879–904` · `app/chamados/services/fio.py:460–466,1394–1403,1446–1456,1665–1691` · `app/ponto/services/veredito_celula.py:365–374` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:129,360–361`

2. **PROCEDENTE — o carimbo inicial é único, mas pode ser desfeito pelo sistema.** O diff acrescenta um chamador de `carimbar_identidade` para a via nova e nenhuma escrita da FK `celula`. Reuso e rechaveamento já tinham suas próprias escritas dessa FK. Para dois Resolvers serializados pela trava do colaborador, mantendo-se o carimbo, o segundo fica fora de `q_vivas`. A guarda do método também devolve `False` sobre a instância atualizada, e a linha de decisão só nasce quando ele grava. `app/chamados/models.py:3248–3256,3360–3368,3526–3542,3754–3769` · `app/chamados/services/fio.py:1189–1198` · `app/ponto/services/veredito_celula.py:194–223,258–261`

   A ressuscitação existente pode selecionar uma pergunta **sem resposta** do Resolver. Ela exclui só `VIAS_FORA_DO_FATO`, limpa o carimbo na cópia antes de perguntar ao C6 e, se a cópia ficar viva, grava `REABERTURA` e limpa o carimbo real pelo lote. O cartório tem caminho para chamar essa porta. Assim, a terminalidade da via no C6 vale enquanto ela está gravada. `app/chamados/models.py:1548–1555,1575–1599,1676–1678,1731–1740,3639–3642` · `app/ponto/services/cartorio.py:1690–1721`

3. **PROCEDENTE — a transação é a mesma, mas o filtro roda na hora errada.** O serviço pega a trava do colaborador no `atomic`, fecha o fio e só então lavra a célula. Uma exceção propagada no registro da decisão desfaz as escritas do dia. O caso sem célula fecha a pergunta sem lavra de célula, conforme o corte. Uma recusa booleana do carimbo, porém, pula a decisão e ainda acrescenta “pergunta fechada” às ações. `app/ponto/services/veredito_celula.py:194–225,258–261,377–400` · `app/chamados/models.py:3528–3532,4575–4586`

   - **Célula lavrada, pergunta viva.** Em competência trancada, com célula e pergunta respondida sem FK, o C6 a exclui antes da lavra. A posterior lavra humana ou a auditoria do Resolver vale como toque e tira a tranca daquele dia. Na leitura seguinte, a pergunta sem FK pode voltar viva pelo degrau da resposta, sem carimbo `resolver_dia`. Com FK de outro dia, o mesmo vale se aquela célula não tiver decisão administrativa nem o marco concordante. A view aceita esse toque em dia trancado. `app/ponto/services/veredito_celula.py:198–214,381–399` · `app/ponto/views.py:3516–3523` · `app/chamados/juizes.py:1130–1139,1373–1396,1502–1528`
   - **Pergunta encerrada sem a trilha do Resolver.** Batidas e ausências são gravadas antes da seleção. Seus sinais chamam o cartório na hora. Se a nova lavra já absolveu uma pergunta muda com FK, `q_vivas` pode excluí-la antes da assinatura do supervisor. Se nenhuma pergunta chega à assinatura, a auditoria do dia leva o motivo, mas omite a frase fixa e usa uma porta que pode falhar sem reverter o ato. `app/ponto/services/veredito_celula.py:198–223,347–399` · `app/ponto/signals.py:85–93,131–147` · `app/ponto/services/cartorio.py:1015–1033` · `app/chamados/models.py:3764–3771` · `app/core/observ.py:94–105,124–139`

4. **NÃO PROCEDENTE — o motivo e a via nova não vazaram para `declarar_dia`, validação ou cobrança.** Falta delega sem `motivo_do_resolver`; folga fecha o fio sem esse argumento; a validação de folga continua usando `fechar_por_admin`. O botão de cobrança usa `formnovalidate` e a view lê colaborador, data e volta. **Há uma lacuna distinta:** o POST de validação no fio reconsulta C6 só para disputa fechada. Se ainda passar pela âncora de uma disputa aberta, pode validar a resposta preservada pelo Resolver. No aceite de folga, a validação pode inclusive chamar `fechar_por_admin` e substituir a via nova. `app/ponto/services/declarar_dia.py:205–218` · `app/chamados/services/validacao.py:547–563,587–596,755–787,810–819` · `app/chamados/services/fio.py:750–760` · `app/templates/ponto/partials/_veredito_dia.html:28` · `app/chamados/views_cobrar.py:324–344`

5. **PROCEDENTE — a oferta tem uma autoridade, mas a paridade tem limites.** Saiu o veto da pergunta viva **respondida**. Continuam obrigatórios dia passado, anterior à competência corrente e status entre `falta`, `irregular`, `aberto` e `sem_certidao`. O plural lê o campo publicado pelo mesmo calendário. Em caso de falha na autoridade de dia em aberto, a tela publica `None`; o plural com `agora` já pode ter respondido `False` para a competência corrente, e a função individual chamada diretamente devolve `bool`. `app/colaboradores/services/calendario.py:94–104,117–168,485–501,604–614,763,798–802`

   `ponto/views.py:3600–3609` lê o campo de **feriado vira trabalho**. O POST do Resolver já aceitava datas sem consultar a oferta, inclusive o toque explícito em trancada. O calendário também não usa K6 como veto, e o aval de exportada mantém a correção humana por essa porta. Há um caso de oferta sem decisão pendente no sentido literal de TT11: uma falta já lavrada pelo admin continua com status `falta`, que habilita o botão em competência anterior. A repetição da falta pode então ser recusada por sobreposição. Esse caso decorre do predicado de status que já existia. `app/ponto/views.py:3491–3523` · `app/docs/CORTES.md:354,462` · `app/core/juizes.py:737,751` · `app/ponto/status_dia.py:63–65` · `app/ponto/portas/celula.py:1045–1054` · `app/ponto/services/ausencia.py:451–471`

6. **NÃO PROCEDENTE — motivo vazio passa pela recusa antes de efeito de negócio.** Avulso e lote têm campo obrigatório e fazem POST na mesma view. Ela manda `''` quando o campo falta. O serviço recusa vazio e espaços antes do `atomic`; a view registra a recusa por data e só reconcilia se houver sucesso. O outro chamador de produção encontrado é `declarar_dia` para falta, que usa a entrada interna sem motivo. Não apareceu outro chamador de Resolver por cron, API ou command no censo da cópia. `app/templates/ponto/partials/_veredito_dia.html:18–26` · `app/templates/ponto/partials/_veredito_lote_barra.html:2–7` · `app/ponto/views.py:3513–3533` · `app/ponto/services/veredito_celula.py:238–260` · `app/ponto/services/declarar_dia.py:213–218`

7. **PROCEDENTE — o app perde a pergunta e normalmente esconde o texto.** Pendências, `historico` e `respondidas` vêm do universo vivo. Após o carimbo, a pergunta sai dos três. O JSON da pergunta não inclui `via_resolucao` nem `ato_motivo`. A mensagem só é anexada se houver chamado vivo do dia que o gate consiga reconciliar. Sai como nota `sistema`, e a API só publica notas de sistema cujo conteúdo tenha a palavra `justificativa`. Portanto até um chamado reconciliado e visível normalmente fica sem a frase prometida. `app/chamados/services/pergunta_no_app.py:45–92` · `app/api/views.py:62–65,1118–1127,2553–2556,2566–2604` · `app/ponto/services/veredito_celula.py:147–156,229–234,381–388` · `app/chamados/services/nota.py:75–89`

   A saída disponível no código é a abertura genérica pelo endpoint `chamados/abrir/`, que recebe assunto e descrição, sem vínculo específico com a pergunta ou a data resolvida. A tela web “Meu atendimento” também oferece “Nova solicitação”. A posição dessa ação no app nativo [nao verificado]. `app/api/urls.py:87` · `app/api/views.py:2141–2165,2189–2202` · `app/templates/chamados/meu_atendimento.html:48`

**7b. PROCEDENTE — a nova rota muda o que a Folha e os vigias concluem.** A carga consulta a oferta para dias sem rota e, com `True`, publica `admin / porta_acao / resolver_dia`. Uma âncora terminal continua sem modal de validação, mas pode ganhar uma porta real no calendário. A tranca, isenção, fora do vínculo e marco ainda não vencido ficam fora dessa consulta. O veto K6 da tarefa incide nas exposições administrativas da E2, sem alcançar o fallback do Resolver, que segue a oferta do calendário. Uma ausência com status `ausencia` segue fora da oferta. Uma ausência aprovada com veredito `fato_em_ausencia` pode ter status `irregular`, de modo que a dispensa do marco pela tarefa não impede a oferta do calendário. Incidência nesses grupos [nao verificado]. `app/chamados/services/tarefa_do_dia.py:74–77,273–278,315–324,358–370,702–717,751–790` · `app/ponto/status_dia.py:67–70,139–162` · `app/colaboradores/services/calendario.py:94–104`

- **Folha:** o universo retido vem do export. Dentro dele, muda o fio, pode mudar o grupo da pessoa para A e aparece o link “Resolver dia no calendário” com a data. Um dia que tinha `nao_sei` pode sumir da lista `dias_sem_leitura` mesmo com o booleano preservado no detalhe. `app/folha/fora_da_folha.py:86–122,197–247,275–295` · `app/folha/retrato_folha.py:1015–1020,1053–1059`
- **Vigia e furo sem canal:** uma rota passa a contar como fio vivo e alcance do admin. O dia sai dos baldes sem rota e de `sem_canal`; o `nao_sei` original ainda é contado. `porta_acao` por si não dispara o alarme do vigia. `app/chamados/services/vigia_fio.py:359–405,593–609,633–659` · `app/chamados/services/furo_sem_canal.py:96–119` · `app/chamados/management/commands/vigiar_fio_morto.py:85–94,210–211,302–303`
- **Pauta e beco:** a pauta descarta o dia com fio admin. Se ele era `nao_sei`, pode perder a proteção que mantinha aberta uma pauta já existente, embora a tarefa ainda carregue o desconhecimento. O beco C11 descarta o dia quando existe envelope de porta, inclusive o envelope anterior de desconhecimento. `app/ponto/services/pautas_dos_discordantes.py:86–97,194–213` · `app/escala/services/cadastro_realidade.py:469–474`

8. **PROCEDENTE — relógio e cobertura dos REDs.** A via nova entra no degrau 0b já existente, o SQL lê o mesmo conjunto e o selo de identidade foi atualizado. O diff fornecido não introduz outro juiz nem um novo `except`. O `now()` novo é lido **sob a trava**, portanto não antecipa o carimbo à aquisição. Ainda assim, a leitura fica dentro do método chamado para **cada pergunta**, separada do instante capturado pelo despachante. Carimbo e `SILENCIO` da mesma pergunta coincidem; perguntas do mesmo ato e a lavra podem ter instantes distintos. Há também a divergência de significado de `materializada_em` na testemunha do item 1. Na sequência, `ato_motivo` continua sendo texto de trilha. `app/chamados/juizes.py:1494–1510` · `app/chamados/models.py:2789–2806,3360–3368,3746–3769,4334–4355` · `app/ponto/services/veredito_celula.py:210–223,251,389–390` · `app/ponto/portas/celula.py:1128–1134` · `app/chamados/tests/test_o261e_f2_fila.py:106–149`

   Por inspeção, os REDs das três amarrações mordem a sobrevivência no C6. Os de motivo e trilha têm asserções específicas. O teste de idempotência chama a guarda diretamente, mas o segundo POST de falta pode ser recusado por sobreposição, então ele não prova dois Resolvers bem-sucedidos. O teste do app **exige o desaparecimento** e passa sem testar a mensagem. O RED do calendário morde o retorno do veto via `pode_ultimo_recurso`; a paridade aceita `False == False` e não prova botão presente. Tranca após o toque, sinal do cartório antes da assinatura, ressuscitação e os efeitos nos leitores de 7b ficam sem prova nesses testes. Execução e resultado contra mutantes [nao verificado]. `app/ponto/tests/test_resolver_passa_na_frente.py:80–105,116–182` · `app/ponto/services/ausencia.py:451–471` · `app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:65–87`

9. **NÃO PROCEDENTE — a migration está conforme.** A 0057 depende da 0056, a última anterior na cópia, e tem uma única operação `AlterField` em `PerguntaDisputa.via_resolucao`. A comparação com a última definição do campo na cadeia confirmou os mesmos parâmetros e choices anteriores, com apenas `resolver_dia` acrescentada. `app/chamados/migrations/0051_decisao_pergunta.py:109–112` · `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:17–27` · `app/chamados/migrations/0057_via_resolver_dia.py:8–18`

## 24/11 02:40 · sessao 8353229698a2 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

Nova execução [nao verificado]. O executor grava arquivos. [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/1f80df53084f4f5c8ea6abae96379375674459d4/bin/sonda_leitura.sh:126)

Saída fornecida [nao verificado]

```text
25
sonda_leitura: sonda_divergencia.py [284cfa46e4b59aba] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/wt-janela-turno-020503/app (copia), banco segue sombra como leitor

TOT diverge 25
TOT diverge_com_decisaohe 13
TOT diverge_janela_ligada 25
TOT pares_com_chave 23948
TOT sem_dia_do_juiz 107
rc_sombra=0
```

## 24/11 02:31 · sessao 98532e6761ac · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · verificador · origem=Code

**BLOQUEIA.** A nova âncora corrige a volta noturna antes do `hf`, mas cria um par invertido quando a volta e a saída já ocorreram depois do `hf`. Na geometria do selo B-I28, a janela da cópia devolveu **07:32 → 07:00 e 43 min fora**, para **11 min brutos**, em execução isolada em memória. A catraca do dia do par também falhou em duas verificações. `app/ponto/tests/test_o280_bateria_da_lei.py:5007,5081` · `app/ponto/motor_calculo_v2.py:1574,1775` · `app/ponto/janela_he.py:168` · `app/core/tests/test_medida_turno_do_par.py:544,560`

## 1. Escritores, leitores e chamadores

- **OK — Memo e chave.** Há três escritas em `_dia_jornada_do_par`: inicialização vazia a cada `calcular_mes`, gravação no laço principal e gravação no crédito do turno aberto. O único leitor é `_janela_do_dia`. Nos dois caminhos, o motor grava `localizar(entrada)` e repassa a mesma entrada a `calcular_periodo`. As três implementações que chamam `_janela_do_par` e a segunda leitura de `Motor12x36ComEscala` usam a mesma `localizar`, que converte ao fuso configurado e preserva segundos e microssegundos. O clipe vem depois da busca. Não encontrei caminho normal com a entrada ajustada, truncada ou em outro fuso usada como chave. `app/ponto/motor_calculo_v2.py:28,73,1549,2343,2493,2593,2800,2927,3265,3593`
- **BLOQUEIA — A chave pode existir com um dia diferente do juiz.** `_dia_do_turno` transpõe `Turno.data_turno` para um mapa por instante localizado das batidas que o juiz classificou como `E`. `calcular_mes` grava esse mapa em `_dia_turno_juiz_calc`. Para `data_jornada`, o laço usa a data civil local na ausência da chave e, com saída anterior fechada e vão inclusivo de **0 a 6 h**, herda `_prev_dj` mesmo quando há resposta diferente do juiz. A janela lê `data_jornada`. O BO-B e a acusação de `aplicar_tolerancia` leem o mapa do juiz. Os overrides comercial e partido também reagrupam pelo juiz após `super().calcular_mes`. `app/ponto/motor_calculo_v2.py:867,884,1612,2030,2432,2439,2459,2494,2949,2983,3355,3367`
- **OK — Crédito do aberto, subclasses e FT.** `_d_a` vem do `data_jornada` já atribuído ao período aberto. O crédito seleciona turnos cujo `t.data_turno == _d_a` e grava `_d_a` para a entrada materializada, inclusive `_za.timestamp` na borda posicional. Essa seleção prova o dia **do grupo**. A chave individual pode faltar no mapa do juiz, que só indexa `E`. `MotorIntermitente` herda o 12x36, `MotorPlantao` delega ao 12x36 e `MotorTurnoPartidoNoturno` herda o partido. A FT do laço principal grava os dois memos e sai da janela pela guarda de folga. O crédito do aberto grava apenas o memo de dia e parte de `resultado.periodos`, cuja classificação é anterior. `app/ponto/motor_calculo_v2.py:889,1532,2483,2493,2529,2550,2559,2572,2593,3575,3601,3610`
- **OK — Marcos e parâmetros.** `_marcos_do_dia_fn` vem do vínculo ou do template. `_janela_do_dia` usa `hi`/`hf` para construir `m_entrada`/`m_saida` e grava `j['dia']`, `j['marco_entrada']` e `j['marco_saida']`. `marco_na_jornada` recebe `date`, hora cadastrada e referência local, mantém o `tzinfo` da referência e zera segundos e microssegundos do **marco**. Seus chamadores são as construções de entrada e saída do par e do BO-B. As pontas efetivas leem os respectivos `datetime`, a extra declarada, o piso, o teto e o estado autorizado. `hi`/`hf` continuam sendo as horas cruas usadas no relato. O cadastro de empresa é lido uma vez por cálculo, e o motor verifica a vigência pelo dia recebido. `app/escala/models.py:1128` · `app/colaboradores/models.py:54,117` · `app/ponto/janela_he.py:33,46` · `app/ponto/motor_calculo_v2.py:1372,1394,1562,1574,1633,1679,1735,1744,1767,1775,1782,2323,2334`

**Censo dos chamadores diretos na cópia**

| Severidade e método | Chamadores |
|---|---|
| **OK — `calcular_periodo`, código de execução** | Laço e crédito do aberto em `app/ponto/motor_calculo_v2.py:2495,2594`. Delegações de `MotorPlantao` e `Motor12x36ComEscala` em `app/ponto/motor_calculo_v2.py:3576,3588`. O censo não encontrou chamada direta em view, serviço, cron, management command ou script da cópia. |
| **OK — `_janela_do_dia`** | `_entrada_efetiva` e `_saida_efetiva` quando `_j` é omitido, e `_janela_do_par`. `app/ponto/motor_calculo_v2.py:1724,1766,1793` |
| **OK — `_janela_do_par`** | `Motor12x36`, `MotorTurnoPartido` e `MotorComercial`. `app/ponto/motor_calculo_v2.py:2800,2927,3265` |
| **FILA — `_entrada_efetiva` e `_saida_efetiva`** | `_janela_do_par` chama ambas passando `j`. `Motor12x36ComEscala` chama `_entrada_efetiva` de novo, sem `j`, e repete a consulta da janela e os efeitos de relato. Essa releitura já existia no comportamento descrito pelo selo B-O272b. `app/ponto/motor_calculo_v2.py:1793,3593` · `app/ponto/tests/test_o280_bateria_da_lei.py:2571` |
| **OK — `calcular_periodo`, testes diretos existentes** | `app/ponto/tests/test_plantao_he_distribuicao.py:27` · `app/ponto/tests/test_plantao_he_8a_hora.py:38` · `app/ponto/tests/test_noturno_desconta_intervalo.py:195,198` · `app/ponto/tests/test_o271_pontualidade_no_comercial.py:60,175` · `app/ponto/tests/test_antecipada_intervalo_12x36.py:53,59,65,70,79` · `app/ponto/tests/test_l084_vence_o_t8.py:65,173` · `app/ponto/tests/test_a6_12x36_tolerancia.py:33` · `app/ponto/tests/test_12x36_atraso_pela_autoridade.py:56,61,66,70,74` · `app/ponto/tests/test_12x36_com_escala_noturna.py:41,53,66` · `app/ponto/tests/test_motor_escala_marcos_do_dia.py:31` |
| **OK — `calcular_periodo`, teste novo** | Chamada com janela ligada que espera `RuntimeError`. `app/ponto/tests/test_janela_no_dia_do_turno.py:197,206` |

## 2. Semântica do dia em cada leitor

- **BLOQUEIA — L-084 por par × BO-B por dia.** A primeira usa `data_jornada` para buscar os marcos, mede as pontas **cruas do par** e pode dispensar a janela antes de consultar o BO-B. O BO-B usa `_dt_b` do juiz e mede a primeira entrada e a última saída do grupo, ou a borda posicional. Ambos exigem **duas** distâncias absolutas estritamente acima de 180 min. Mesmo quando os dias coincidem, uma volta de pausa pode ficar a mais de 180 min de `hi` pela própria posição dentro da jornada. Quando os dias divergem, o BO-B pode dispensar a janela com base em outro dia. Sua acusação grava `_dt_b`, mas o contador grava `dia` do par. `app/ponto/motor_calculo_v2.py:1562,1589,1612,1633,1641,1650,1980`
- **BLOQUEIA — Relato × espelho × Gestão de HE.** `_acusa_cadastro_x_realidade` recebe `j['dia'] = data_jornada` e grava batida e marco em `HH:MM` e minutos arredondados. `pontas_do_relato` exige igualdade com a data da linha, mesma hora de marco, célula da ponta `missing` e ausência de ponta já marcada no mesmo sentido. O espelho apresenta as batidas no dia de `dia_das_batidas`, que usa o juiz por PK e data civil local para a batida que nenhum turno reclama. Portanto uma cauda herdada pelo motor em **D**, mas exibida em **D+1**, pode ter sua acusação ignorada. O caminho normal da grade também usa **primeira E e última S**, enquanto o motor agora pode recortar a saída de **cada par**. A Gestão recebe as pendências do espelho. `app/ponto/motor_calculo_v2.py:1702,1742,1780,2441` · `app/ponto/janela_he.py:217,300,316,334` · `app/ponto/turnos.py:2907` · `app/ponto/services/espelho.py:464,741,754` · `app/ponto/services/he_pendente_lavrado.py:114,132,148`
- **BLOQUEIA — Extra declarada e vigência.** O cadastro escreve minutos inteiros por ponta no tipo de escala. A geração congela os valores no DNA. O vínculo lê `(colaborador, data)`, responde zero se a célula existe sem declaração e lê o template se a célula falta. O motor consulta `data_jornada` pelo vínculo da fatia. Espelho e calendário consultam a data da linha e escolhem o vínculo desse dia. Concordam quando dia e vínculo coincidem. Na herança divergente, podem ler DNA, extra e vigência diferentes. A vigência inclui o dia inicial, o piso inclui a igualdade e a acusação por teto exige sobra estritamente maior. A janela calcula em minutos fracionários e as telas arredondam as pontas publicadas. `app/escala/services/cadastro_tipo.py:256` · `app/ponto/management/commands/gerar_celulas.py:128` · `app/escala/models.py:1238` · `app/ponto/motor_calculo_v2.py:1356,1382,1679,2329` · `app/ponto/services/espelho.py:736` · `app/colaboradores/services/calendario.py:413` · `app/ponto/janela_he.py:70,123,173,242`
- **BLOQUEIA — `DecisaoHE.data`.** O campo é `DateField`. A porta grava **a data recebida**, sem deduzir dia civil nem dia do turno. O formulário individual manda `s.data` do retrato do espelho. Lote e autorização por período leem o mesmo retrato, e o cron de recusa passa a data do item à porta. Assim, para turno reconhecido pelo juiz a origem usual é o **dia exibido do turno**. A divergência aparece quando ele difere de `data_jornada`, ou quando um chamador envia outra data. O motor carrega `{d.data: d.estado}` e `autorizado` solta ambas as pontas dos pares que consultam aquela chave. Uma decisão exibida em **D+1** pode ficar sem efeito no par herdado em **D**, ou a autorização de **D** pode alcançar um par exibido em **D+1**. O selo novo cria a linha em **D** diretamente no ORM e deixa essa ligação sem exercício. Datas de decisões reais [nao verificado]. `app/ponto/models.py:1300` · `app/ponto/portas/he.py:66,195,206,265,369,595` · `app/templates/ponto/gestao_he.html:259` · `app/ponto/views.py:543,630,684` · `app/ponto/services/gestao_he.py:397` · `app/ponto/management/commands/recusar_ponta_pequena.py:156,170` · `app/ponto/motor_calculo_v2.py:1428,1686,1730,1772` · `app/ponto/tests/test_janela_no_dia_do_turno.py:95`
- **FILA — `_janela_fora_por_l084`.** O motor inicializa a lista e insere `dia` com deduplicação nos dois ramos. O leitor externo efetivo encontrado é o teste que mede seu comprimento. O comando `diff_janela_he` conta `len(resultado.dias_cadastro_x_realidade)`, que também recebe acusações de teto da janela e da pontualidade. Assim, esse comando não mede dias únicos retirados pela L-084. `app/ponto/motor_calculo_v2.py:1596,1640,1702,2025,2336,2760` · `app/ponto/tests/test_o280_bateria_da_lei.py:2819` · `app/ponto/management/commands/diff_janela_he.py:63,70`

## 3. `RuntimeError` e produção

- **OK — Alcance no código atual.** A view e o comando de recálculo entram pelo fechamento. O recálculo por evento, inclusive disparado por um fato de um dia, recalcula a competência e chega a `calcular_mes`. Espelho e simulação de autorização passam pela autoridade do período. PDF e diagnóstico também chamam `calcular_mes`. O cron `vigiar_motor_autoridade` e os comandos `diff_espelho_alimentacao` e `diff_extrato_parcial` fazem o mesmo. A entrega de holerite busca o documento gravado. Não identifiquei caminho dessas rotas até o novo `raise` com a chave ausente. Uso fora da cópia e ocorrência em produção [nao verificado]. `app/ponto/views.py:1280` · `app/ponto/services/fechamento.py:286,754,827,1810` · `app/ponto/management/commands/recalcular_fechamento.py:118` · `app/ponto/services/espelho.py:1200,1215` · `app/ponto/services/autorizacao_he_periodo.py:96` · `app/relatorios/pdf_espelho.py:434` · `app/ponto/services/esmeril_espelho.py:174` · `app/config/crons.py:596` · `app/ponto/management/commands/vigiar_motor_autoridade.py:58` · `app/ponto/management/commands/diff_espelho_alimentacao.py:58` · `app/relatorios/management/commands/diff_extrato_parcial.py:68` · `app/holerite/views.py:1304,1393,1445`
- **FILA — Mudança de contrato do chamador direto.** Com par fechado, chave ausente e empresa identificada com janela ligada, o `raise` acontece **antes** da vigência e da consulta de marcos. Pode levantar mesmo com `janela_he_desde` ausente ou posterior ao par. É uma falha explícita, sem retorno à data civil. Se um chamador novo atingir esse ramo, a exceção propaga onde não houver captura. O recálculo por evento tem captura externa. Entre os testes diretos do censo, só o teste novo injeta o contexto de empresa com janela ligada, e espera a exceção. `app/ponto/motor_calculo_v2.py:1379,1394,1532,1549,1556,2793` · `app/ponto/services/fechamento.py:1807,1818` · `app/ponto/tests/test_janela_no_dia_do_turno.py:197,206`

## 4. Marco de saída e valor do par

- **OK — 15:00–23:50 com saída 00:10.** No diff anterior, `m_saida` usava a data civil **da saída**, logo o marco era 23:50 de **D+1**. Com a âncora em **D**, fica 23:50 de **D**. Com a janela aplicável, piso e extra zero, a mesma regra L-097 passa a recortar 20 min, de 550 para 530 min brutos após a janela. Isso é consequência direta da âncora dentro do aval. `app/ponto/motor_calculo_v2.py:1574,1775` · `app/ponto/janela_he.py:166` · `app/ponto/tests/test_janela_no_dia_do_turno.py:177`
- **BLOQUEIA — Par inteiro depois do marco.** No B-I28, a cauda de **07:32–07:43** herda o dia do turno 19–07. O motor conserva a entrada às 07:32 e recorta a saída a 07:00. Para a L-084, só a distância da entrada passa de 180 min. O cálculo carimba a duração invertida, limita trabalhadas a zero e conserva **43 min fora**. Antes, a fixture mantinha o par cru de 11 min. A nova saída viola as duas propriedades do aval. Resultado da execução isolada da janela em memória, com os insumos da fixture. Incidência em produção [nao verificado]. `app/ponto/tests/test_o280_bateria_da_lei.py:4993,5007,5081,5127` · `app/ponto/motor_calculo_v2.py:1574,1589,2800,2826` · `app/ponto/janela_he.py:168,175` · `app/ponto/tests/test_janela_no_dia_do_turno.py:121`
- **BLOQUEIA — A troca também altera a dispensa pela L-084 sem troca do dia civil da entrada.** Com `data_jornada=D`, escala **19:00–00:10**, par **D 00:00–23:55**, piso e extra zero, a geometria anterior via duas pontas longe e preservava **1.435 min brutos**. Com `hf` em **D+1**, a saída fica a 15 min do marco e a janela corta **1.140 min** na entrada. Restam **295 min brutos**. O caso foi reproduzido no recorte em memória. O pareamento dessa geometria em produção [nao verificado]. `app/ponto/motor_calculo_v2.py:1574,1589,1980` · `app/ponto/janela_he.py:115,125`
- **FILA — FT e 24x48.** A FT marcada no laço sai antes da construção dos marcos. Para plantão com `hi == hf`, a autoridade registra que cruza a meia-noite. Um plantão 07:00 de **D** até 07:00 de **D+1** conserva o `hf`. Para **06:40–07:30 de D**, com `hi == hf == 07:00`, a janela isolada passa de **0 min efetivos e 50 fora** para **30 efetivos e 20 fora**, pois o `hf` muda de **D** para **D+1**. O exemplo mostra a mudança de valor na borda de igualdade. Um caso real de 24x48 com essa geometria [nao verificado]. `app/ponto/motor_calculo_v2.py:1538,1578,2493,3575` · `app/escala/servico_jornada.py:104` · `app/ponto/janela_he.py:125,175`

## 5. Classes de reprovação

- **BLOQUEIA — Derivação paralela presente.** O juiz registrado da atribuição da batida é `turnos_do_colab`. O diff não introduz outro pareador nem outro predicado `cruza?`. Ele eleva a herança e a degradação civil já existentes a fonte do dinheiro da janela, enquanto BO-B e apresentação continuam lendo o dia do juiz. Há também composição paralela do **instante de saída**: `marcos_turno` encadeia todos os marcos, inclusive pausa, enquanto os dois ramos da janela usam apenas `hi/hf` e `+1`. Por exemplo, a sequência `18:00 / 23:00 / 01:00 / 19:00` põe `hf` em **D+1** no encadeador e em **D** na janela. Uso dessa sequência em produção [nao verificado]. `app/docs/JUIZES.md:20,24` · `app/core/juizes.py:215` · `app/escala/servico_jornada.py:69,82,104` · `app/ponto/motor_calculo_v2.py:1574,1612,1633,2439`
- **OK — Segundo escritor persistente, relógio fora da borda e mudança de trava ausentes no diff.** As escritas novas são no memo da instância, antes dos dois cálculos. A porta de HE continua adquirindo a trava e gravando a decisão. O marco usa a referência recebida. A L-084 segue antes do clipe e da leitura do estado autorizado. Há **zero `except` adicionados e zero removidos** no diff fornecido. O `except Exception` da consulta de empresa já existia. A chave ausente ganhou um `raise` e o retorno sem janela para empresa desligada ou indisponível. `app/ponto/motor_calculo_v2.py:1394,1549,1589,1674,2494,2593` · `app/ponto/portas/he.py:158,195,206` · `app/ponto/janela_he.py:46`
- **BLOQUEIA — Campo com dois significados presente.** `dia` no relato de janela e no contador vem do período com herança. `dia` da acusação BO-B e da pontualidade vem do juiz. `DecisaoHE.data` recebe o dia apresentado e o motor a consulta pelo dia herdado. `app/ponto/motor_calculo_v2.py:1641,1655,1686,1703,2030,2444` · `app/ponto/services/he_pendente_lavrado.py:148` · `app/ponto/portas/he.py:66`
- **OK — Os dois mutantes têm asserções capazes de morder.** Restaurar `entrada.date()` perde, por exemplo, o recorte de **2 min** esperado na cauda literal de col113. Suprimir o `+1` do marco **do par** quebra a guarda **18:40 → 07:00**, que espera saída em **D+1**, 720 min trabalhados e 20 fora. A execução em memória confirmou a quebra da geometria da guarda sem `+1`. O mutante que remove **só** o `+1` do BO-B não tem prova dedicada localizada. `app/ponto/tests/test_janela_no_dia_do_turno.py:160,231,310,323` · `app/ponto/motor_calculo_v2.py:1579,1635`
- **BLOQUEIA — Matriz incompleta e pré-requisito de pouso aberto.** As fixtures comercial e vespertina gravam `tipo_base='5x2'`, que a fábrica rejeita antes das asserções. A propriedade não cobre a cauda depois do `hf` do B-I28. O caso col200 cobra apenas a entrada, e o DNA criado na fixture não inclui `extra_declarada`, ainda que o tipo tenha minutos cadastrados. Suite integral verde, sonda dos 57 dias e dos ~16 mil pares e zero violações na frota [nao verificado]. `app/ponto/tests/test_janela_no_dia_do_turno.py:68,73,106,121,243,279,290,321` · `app/ponto/motor_calculo_v2.py:3634,3692` · `app/escala/models.py:1245` · `CLAUDE.md:383` · `CORTES.md:643` (anexo)
- **OK — `.replace(hour=...)` fora do módulo da janela ausente nos sítios vigiados.** O novo `replace` fica em `janela_he.py`. O teste estrutural verifica o módulo, os quatro métodos da janela e `calculador/regras.py`. `app/ponto/janela_he.py:48` · `app/ponto/tests/test_janela_he_regra.py:124,136,147,154`

## 6. Selos existentes e assinatura

- **OK — Assinatura no censo da cópia.** `entrada_efetiva` agora exige o marco como `datetime`. Um consumidor com `hi=` falharia pela assinatura, e um `time` posicional falharia na subtração. O chamador executável encontrado já passa `j['marco_entrada']`. `calculador/regras.py` só importa a função. `test_janela_he_regra.py` usa `calcular_mes` e verifica o import e a AST. O teste direto de `saida_efetiva` já passa `datetime`, e as chamadas com aliases no selo do calculador são texto de fixture AST. `app/ponto/janela_he.py:95,115` · `app/ponto/motor_calculo_v2.py:1734` · `app/ponto/calculador/regras.py:30` · `app/ponto/tests/test_janela_he_regra.py:67,124,136` · `app/ponto/tests/test_janela_he_saida.py:134` · `app/ponto/tests/test_chamador_do_calculador_aplica_a_janela.py:134`
- **BLOQUEIA — Duas falhas confirmadas na catraca.** Executei a versão em memória indicada no próprio teste, com `unittest` e sem Django. `test_CATRACA_numeros_exatos_e_registro_no_mesmo_commit` falha porque o contador desce **84 → 83**. `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` falha porque a entrada civil da janela saiu e o registro mantém posições antigas, inclusive `2408 → 2444` na herança. `app/core/tests/test_medida_turno_do_par.py:20,299,544,560` · `app/ponto/motor_calculo_v2.py:1549,2444`
- **BLOQUEIA — A bateria O280 fixa comportamentos que a obra muda.** B-P4a exige o par antigo invertido e carrega um `expectedFailure` para os **150 min** da lei. B-I28 exige a cauda crua e **731 min** no dia, além de ter outro `expectedFailure`. São incompatibilidades identificadas por leitura e pela execução isolada da janela. Resultado integral desses testes Django e dos pinos [nao verificado]. `app/ponto/tests/test_o280_bateria_da_lei.py:4926,4935,4961,4982,5081,5113,5123`

## 7. Limite do aval, atraso e competência exportada

- **BLOQUEIA — O recorte excede a correção pedida no B-I28.** Os corpos de `calcular_per

[truncado em 20000 de 22110 caracteres]

## 24/11 02:29 · sessao 08df8f8609fb · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

# DEPOIS — NOTURNO-A-JANELA-NO-DIA-DO-TURNO

A soma das 18 classes é **402**, igual ao `C1 402` do anexo. Em `iguais_so_fechados`, são **320** casos, dos quais **308** têm `motor_agora=0` e **12** têm `motor_agora>0`. [nao verificado]

As marcações `[nao verificado]` nas tabelas indicam que a fonte é o anexo raspado fornecido, sem verificação independente na sombra. [nao verificado]

## 1. Contagem por classe

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | [nao verificado] |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | [nao verificado] |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | [nao verificado] |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | [nao verificado] |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | [nao verificado] |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | [nao verificado] |
| `entrada_repetida+virada_ou_outro_dia` | 1 | [nao verificado] |
| `iguais_com_aberto` | 62 | [nao verificado] |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | [nao verificado] |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | [nao verificado] |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 45 | [nao verificado] |
| `iguais_so_fechados\|bruto>30\|motor_agora>0` | 12 | [nao verificado] |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | [nao verificado] |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | [nao verificado] |
| `virada_ou_outro_dia` | 3 | [nao verificado] |
| **Soma = C1** | **402** | [nao verificado] |

O anexo termina em `rc_sombra=0` e não traz traceback para transcrever. [nao verificado]

## 2. Exemplos `fech` e corte da faixa 22h–05h

Cada `pares` mantém a ordem do anexo `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]`. O teste usa a borda da faixa que o par bruto alcançou. Nos 12 exemplos com motor zero, a saída efetiva eliminou o trecho após as 22h. Nos quatro com motor positivo, a entrada efetiva ficou antes das 05h do dia seguinte e o par preservou esse trecho da madrugada. [nao verificado] [nao verificado]

### `iguais_so_fechados|bruto5-30|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 108 | 2026-10-19 | 10.5 | 0.0 | `[["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-21 | 6.3 | 0.0 | `[["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-25 | 17.6 | 0.0 | `[["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-11-16 | 15.6 | 0.0 | `[["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto<=5|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 60 | 2026-10-31 | 3.5 | 0.0 | `[["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]]` | Sim, saída efetiva 19:00:00 < 22:00. [nao verificado] |
| 108 | 2026-10-01 | 0.7 | 0.0 | `[["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-03 | 0.5 | 0.0 | `[["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 108 | 2026-10-05 | 0.9 | 0.0 | `[["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto>30|motor_agora=0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 373 | 2026-10-22 | 48.8 | 0.0 | `[["2026-10-22 16:58:11", "2026-10-22 22:48:50", "2026-10-22 16:58:11", "2026-10-22 22:00:00", 301.8, 48.8]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-24 | 48.9 | 0.0 | `[["2026-10-24 16:52:22", "2026-10-24 22:48:55", "2026-10-24 16:52:22", "2026-10-24 22:00:00", 307.6, 48.9]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-26 | 49.6 | 0.0 | `[["2026-10-26 16:56:11", "2026-10-26 22:49:38", "2026-10-26 16:56:11", "2026-10-26 22:00:00", 303.8, 49.6]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |
| 373 | 2026-10-28 | 48.9 | 0.0 | `[["2026-10-28 16:59:53", "2026-10-28 22:48:56", "2026-10-28 16:59:53", "2026-10-28 22:00:00", 300.1, 48.9]]` | Sim, saída efetiva = 22:00:00. [nao verificado] |

### `iguais_so_fechados|bruto>30|motor_agora>0`

| Colab | Dia | Bruto | Leg | Pares | Cortou a faixa? |
|---:|---|---:|---:|---|---|
| 37 | 2026-10-05 | 119.4 | 119.4 | `[["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 03:00:38", "2026-10-06 07:00:00", 239.4, 1.0]]` | Não, preservou 03:00:38–05:00 do dia seguinte. [nao verificado] |
| 37 | 2026-10-09 | 119.3 | 119.3 | `[["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 03:00:40", "2026-10-10 07:00:00", 239.3, 1.8]]` | Não, preservou 03:00:40–05:00 do dia seguinte. [nao verificado] |
| 113 | 2026-10-30 | 120.0 | 137.1 | `[["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 03:00:00", "2026-10-31 07:00:00", 240.0, 2.0]]` | Não, preservou 03:00:00–05:00 do dia seguinte. [nao verificado] |
| 113 | 2026-11-01 | 120.0 | 137.1 | `[["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 03:00:00", "2026-11-02 07:00:00", 240.0, 1.0]]` | Não, preservou 03:00:00–05:00 do dia seguinte. [nao verificado] |

## 3. `motor_agora>0` com `DiaPago=0`

**Sim, no sentido de divergência de valor frente ao recálculo da cópia.** Nos 12 casos dessa subclasse, a sonda reporta noturno legal positivo no motor atual e zero no `DiaPago` usado pelo C1. [nao verificado]

No código da cópia, a janela busca o dia da jornada atribuído ao par e coloca o marco de saída no dia seguinte quando o turno cruza a meia-noite. O motor calcula `minutos_noturnos_legais` sobre as pontas efetivas, desconta o noturno dos intervalos e aplica o fator de hora noturna, com prorrogação conforme a configuração. [`motor_calculo_v2.py:2494`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:2494), [`motor_calculo_v2.py:1549`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1549), [`motor_calculo_v2.py:1574`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1574), [`motor_calculo_v2.py:2833`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:2833), [`motor_calculo_v2.py:983`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:983)

Na lavra `versao='motor'`, o fechamento entrega os períodos e o mapa `dia_da_jornada`. `lavrar` soma `minutos_noturnos_legais / 60` em `horas_noturnas` do dia atribuído e substitui as linhas da competência para aquela versão. Se receber os mesmos períodos positivos do recálculo para o mesmo dia, a lavra desse dia sai positiva. [`fechamento.py:1201`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1201), [`fechamento.py:1217`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1217), [`fechamento.py:1227`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/fechamento.py:1227), [`dia_pago.py:173`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/dia_pago.py:173), [`dia_pago.py:359`](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/services/dia_pago.py:359)

**A causa histórica do zero fica em aberto.** Data e versão da lavra original, períodos e insumos que ela recebeu, eventual tentativa de relavra e atribuição dos 12 casos à mudança de ancoragem da janela estão [nao verificado].

## 24/11 02:26 · sessao 7e01b5d91af0 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## (1) UNIVERSO e emp_comp

```text
UNIVERSO pares_fechados 16167 invertidos 4 sha256 848979805ef7be0fd6fce6610e41ce9e53c68dcfe3403c902b916763c17bb11d
emp_comp 1 10/2026 fechados 12 invertidos 0
emp_comp 1 11/2026 fechados 14 invertidos 0
emp_comp 2 10/2026 fechados 6038 invertidos 2
emp_comp 2 11/2026 fechados 6337 invertidos 1
emp_comp 2 12/2026 fechados 275 invertidos 0
emp_comp 3 10/2026 fechados 1478 invertidos 1
emp_comp 3 11/2026 fechados 1408 invertidos 0
emp_comp 3 12/2026 fechados 61 invertidos 0
emp_comp 4 10/2026 fechados 276 invertidos 0
emp_comp 4 11/2026 fechados 258 invertidos 0
emp_comp 4 12/2026 fechados 10 invertidos 0
```

## (2) Linhas INV

```text
INV 109 3 10/2026 N 2026-10-05 2026-10-05 22:00:41 2026-10-05 22:10:41 2026-10-05 22:00:41 2026-10-05 22:00:00 0.0 10.7 0.0 649.3 INV
INV 399 2 10/2026 N 2026-09-30 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:00 0.0 0.5 0.0 0.0 INV
INV 456 2 11/2026 N 2026-11-14 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:00 0.0 0.1 0.0 0.0 INV
INV 473 2 10/2026 N 2026-09-21 2026-09-21 19:00:18 2026-09-21 19:23:54 2026-09-21 19:00:18 2026-09-21 19:00:00 0.0 23.9 0.0 0.0 INV
```

## (3) Classificação e contas

**Os quatro casos são B.** Em cada linha do anexo, o dia civil da entrada coincide com o dia de jornada da coluna 5, a entrada efetiva é igual à entrada bruta e a saída foi recortada para antes de ambas as batidas. A cópia ancora os marcos no dia da jornada e grava as pontas resultantes do recorte. O limite de saída pode incorporar extra declarada. O marco cadastrado e a extra declarada, separadamente para cada par desta execução, ficam [nao verificado]. [app/ponto/motor_calculo_v2.py:1549](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1549) · [app/ponto/motor_calculo_v2.py:1574](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1574) · [app/ponto/janela_he.py:168](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/janela_he.py:168) · [app/ponto/motor_calculo_v2.py:1873](/home/ronald/codex_ws/b4528cb2fe8ea8de737890fbdca26169931c8ab4/app/ponto/motor_calculo_v2.py:1873)

| INV | Classe | Dia civil da entrada = dia de jornada | Saída efetiva < entrada bruta ≤ saída bruta | Saída efetiva − entrada efetiva |
|---|---|---|---|---|
| 109 | B | `2026-10-05` | `22:00:00 < 22:00:41 < 22:10:41` | `22:00:00 − 22:00:41 = −41 s` |
| 399 | B | `2026-09-30` | `18:30:00 < 18:30:29 = 18:30:29` | `18:30:00 − 18:30:29 = −29 s` |
| 456 | B | `2026-11-14` | `19:00:00 < 19:00:08 = 19:00:08` | `19:00:00 − 19:00:08 = −8 s` |
| 473 | B | `2026-09-21` | `19:00:00 < 19:00:18 < 19:23:54` | `19:00:00 − 19:00:18 = −18 s` |

## 24/11 02:23 · sessao e938a376abf2 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

A sonda `DEPOIS` parou na inicialização do contêiner, com `rc_sombra=125`. O trecho raspado termina sem contagens de C1 e sem linhas `fech`. (`DADO RASPADO:3–6`, contando a partir da primeira linha `sonda_leitura`.)

### 1. Contagens e soma C1

| Classe | Noturno bruto | Motor agora | Contagem |
|---|---:|---:|---|
| `iguais_so_fechados` | ≤ 5 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | ≤ 5 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | > 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | = 0 | [nao verificado] |
| `iguais_so_fechados` | > 30 min | > 0 | [nao verificado] |
| Demais classes, inclusive seus nomes | | | [nao verificado] |
| **Soma = C1** | | | **[nao verificado]** |

O trecho contém o erro do Docker abaixo. Traceback Python [nao verificado]. (`DADO RASPADO:3–6`)

```text
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "/home/ronald/saas-hasner/app/staticfiles" to rootfs at "/app/staticfiles": create mountpoint for /app/staticfiles mount: make mountpoint "/app/staticfiles": mkdirat /var/lib/docker/rootfs/overlayfs/ff272d7d2278fe95edf6641b5e8d4057eb88fbf061c4bd2fb5824188b871dde0/app/staticfiles: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

### 2. Exemplos `fech` e corte da faixa

Nas seis subclasses, os pares a transcrever e o resultado dos testes `efetivo_saida <= 22:00` e `efetivo_entrada >= 05:00` ficam [nao verificado]. O motor registra as pontas efetivas e usa o trecho recortado para calcular o noturno, com desconto das pausas e prorrogação conforme a configuração. [`motor_calculo_v2.py:1867`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:1867), [`motor_calculo_v2.py:2927`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:2927), [`motor_calculo_v2.py:983`](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:983) [nao verificado]

### 3. `motor_agora > 0` com `DiaPago = 0`

Se o positivo vier de um período que a lavratura `versao='motor'` incluiria na mesma linha de colaborador, competência e dia da jornada, o zero gravado diverge do que essa lavratura produziria hoje. `lavrar` soma `minutos_noturnos_legais / 60` dos períodos recebidos no dia atribuído e grava o resultado quando é chamada. [`fechamento.py:1200`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1200), [`fechamento.py:1217`](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1217), [`dia_pago.py:173`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:173), [`dia_pago.py:359`](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:359)

A ocorrência dessa divergência na sombra, a data e a versão da lavra e a hipótese de que o zero veio de uma lavra anterior à mudança do motor ficam [nao verificado].

## 24/11 02:22 · sessao 42fa281c12fe · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

1. `UNIVERSO` e `emp_comp` — nenhuma dessas linhas aparece no trecho fornecido. [nao verificado]
2. `INV` — nenhuma linha para transcrever. O trecho mostra um erro de montagem do Docker e termina com `rc_sombra=125`. A quantidade de `INV` fica [nao verificado].
3. Classes A, B e C — sem linhas `INV`, faltam os horários de cada par para fazer as contas e atribuir uma classe. [nao verificado]

## 24/11 02:22 · sessao 9d23ce02ad2a · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

### Transcrição verbatim do anexo

```text
CASO 37 2026-10-05 emp 4 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 517 2026-05-21 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-04 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-05 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-06 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-04 False gerada None
  CEL 2026-10-05 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-06 False gerada None
  BAT 2026-10-06 02:00:15 S app
  BAT 2026-10-06 03:00:38 E app
  BAT 2026-10-06 07:01:01 S app
CASO 113 2026-10-30 emp 3 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 818 2026-06-24 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-29 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-30 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-31 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-29 False gerada None
  CEL 2026-10-30 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-31 False gerada None
  BAT 2026-10-31 02:00:00 S disputa_s84_retro
  BAT 2026-10-31 03:00:00 E disputa_s84_retro
  BAT 2026-10-31 07:02:00 S disputa_s84_retro
CASO 113 2026-11-01 emp 3 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 818 2026-06-24 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-10-31 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-11-01 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-02 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-10-31 False gerada None
  CEL 2026-11-01 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-02 False gerada None
  BAT 2026-11-02 02:01:00 S disputa_s84_retro
  BAT 2026-11-02 03:00:00 E disputa_s84_retro
  BAT 2026-11-02 07:01:00 S disputa_s84_retro
CASO 174 2026-09-23 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1616 2026-11-16 None TE 12x36 12x36 18:00:00 06:00:00 01:00:00 02:00:00 720 he_extra 0 0
    marcos_do_dia 2026-09-22 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-09-23 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-09-24 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-09-22 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-09-23 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-09-24 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-09-22 20:58:21 E app
  BAT 2026-09-23 02:46:44 S app
  BAT 2026-09-23 03:46:54 E app
  BAT 2026-09-23 05:02:44 S app
  BAT 2026-09-24 02:32:11 E app
  BAT 2026-09-24 03:24:34 S app
  BAT 2026-09-24 05:00:58 S app
CASO 200 2026-11-02 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1548 2026-11-10 None TE 6x1 turno_partido 15:00:00 23:00:00 18:00:00 19:00:00 420 he_extra 10 10
    marcos_do_dia 2026-11-01 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-02 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-03 (datetime.time(22, 0), datetime.time(6, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-11-01 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-02 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-03 True gerada {'hf': '06:00', 'hi': '22:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-11-03 02:00:00 S disputa_s84_retro
  BAT 2026-11-03 02:00:33 E app
  BAT 2026-11-03 03:02:17 S app
  BAT 2026-11-03 06:00:15 S app
  BAT 2026-11-03 22:00:47 E app
  BAT 2026-11-04 02:00:24 S app
  BAT 2026-11-04 03:00:36 E app
  BAT 2026-11-04 06:00:48 S app
CASO 218 2026-10-15 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 1262 2026-09-20 None TE 6x1 turno_partido 21:00:00 05:00:00 02:00:00 03:00:00 420 he_extra 0 0
    marcos_do_dia 2026-10-14 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-10-15 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-10-16 (datetime.time(21, 0), datetime.time(5, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
  CEL 2026-10-14 False gerada None
  CEL 2026-10-15 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-10-16 True gerada {'hf': '05:00', 'hi': '21:00', 'hfi': '03:00', 'hii': '02:00'}
  BAT 2026-10-16 02:15:08 S app
  BAT 2026-10-16 03:15:06 E app
  BAT 2026-10-16 05:00:01 S app
  BAT 2026-10-16 20:59:13 E app
  BAT 2026-10-17 02:15:42 S app
  BAT 2026-10-17 03:15:07 E app
  BAT 2026-10-17 05:00:33 S app
CASO 296 2026-11-02 emp 2 janela True 2026-08-21 piso 0 teto 60 saida True 60
  EC 261 2026-07-21 None TE 12x36 12x36 19:00:00 07:00:00 02:00:00 03:00:00 720 he_extra 0 0
    marcos_do_dia 2026-11-01 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
    marcos_do_dia 2026-11-02 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha True
    marcos_do_dia 2026-11-03 (datetime.time(19, 0), datetime.time(7, 0), datetime.time(2, 0), datetime.time(3, 0)) trabalha False
  CEL 2026-11-01 False gerada None
  CEL 2026-11-02 True gerada {'hf': '07:00', 'hi': '19:00', 'hfi': '03:00', 'hii': '02:00'}
  CEL 2026-11-03 False gerada None
  BAT 2026-11-03 00:15:19 E app
  BAT 2026-11-03 01:29:03 E app
  BAT 2026-11-03 07:00:41 S app
```

### Turno de D por caso

`hi` e `hf` vêm da célula de D. Quando o turno previsto cruza a meia-noite, `hf` cai em D+1. [app/escala/models.py:1135] [app/ponto/selecao_periodo.py:138] [app/escala/servico_jornada.py:111]

A lista de batidas resulta do pareador da cópia alimentado com os marcos e os tipos `E`/`S` do anexo. O pareamento efetivo na sombra, que também pode usar o papel dado pela ata, fica [nao verificado]. [app/ponto/turnos.py:1192] [app/ponto/turnos.py:1944] [app/ponto/turnos.py:3125]

- **CASO 37 · 2026-10-05** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-10-06: `02:00:15 S`, `03:00:38 E`, `07:01:01 S`. [app/ponto/turnos.py:293]
- **CASO 113 · 2026-10-30** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-10-31: `02:00:00 S`, `03:00:00 E`, `07:02:00 S`. [app/ponto/turnos.py:293]
- **CASO 113 · 2026-11-01** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-11-02: `02:01:00 S`, `03:00:00 E`, `07:01:00 S`. [app/ponto/turnos.py:293]
- **CASO 174 · 2026-09-23** — `hi` 21:00 em D, `hf` 05:00 em D+1, cruza a meia-noite. Batidas de 2026-09-24: `02:32:11 E`, `03:24:34 S`, `05:00:58 S`. [app/ponto/turnos.py:299] [app/ponto/turnos.py:1371]
- **CASO 200 · 2026-11-02** — `hi` 22:00 em D, `hf` 06:00 em D+1, cruza a meia-noite. Batidas de 2026-11-03: `02:00:00 S`, `02:00:33 E`, `03:02:17 S`, `06:00:15 S`. [app/ponto/turnos.py:293] [app/ponto/turnos.py:1371]
- **CASO 218 · 2026-10-15** — `hi` 21:00 em D, `hf` 05:00 em D+1, cruza a meia-noite. Batidas de 2026-10-16: `02:15:08 S`, `03:15:06 E`, `05:00:01 S`. [app/ponto/turnos.py:293]
- **CASO 296 · 2026-11-02** — `hi` 19:00 em D, `hf` 07:00 em D+1, cruza a meia-noite. Batidas de 2026-11-03: `00:15:19 E`, `01:29:03 E`, `07:00:41 S`. [app/ponto/turnos.py:299] [app/ponto/turnos.py:1371]

## 24/11 02:21 · sessao cebe1e5caa38 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## 1. DEPOIS — contagem e saída da sonda

O Docker falhou ao montar `/app/logs` durante a inicialização do contêiner, antes da chamada ao Python. O trecho fornecido contém o erro abaixo. Traceback Python [nao verificado]. [bin/sonda_leitura.sh:203](/home/ronald/wt-janela-turno-020503/bin/sonda_leitura.sh:203) · [bin/sonda_leitura.sh:235](/home/ronald/wt-janela-turno-020503/bin/sonda_leitura.sh:235)

```text
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/2f6ba2df7015b1f000d88291ba0c235856952da9e2aea595a35b059ad3fbbb1a/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

| Classe | Noturno bruto | Motor agora `=0` | Motor agora `>0` |
|---|---:|---|---|
| `iguais_so_fechados` | ≤ 5 min | [nao verificado] | [nao verificado] |
| `iguais_so_fechados` | > 5 a 30 min | [nao verificado] | [nao verificado] |
| `iguais_so_fechados` | > 30 min | [nao verificado] | [nao verificado] |

Demais classes, total C1 e conferência da soma das classes com C1 [nao verificado].

## 2. Exemplos `fech` e corte da faixa noturna

Para cada uma das seis subclasses, os pares `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]` e o resultado do teste por par ficam [nao verificado]. O teste usa as datas completas da faixa em questão, com `efetivo_saida <= 22:00` do início da faixa ou `efetivo_entrada >= 05:00` do fim dela. O motor carimba as pontas efetivas e calcula a interseção em horário local. [app/ponto/motor_calculo_v2.py:89](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:89) · [app/ponto/motor_calculo_v2.py:1867](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:1867) [nao verificado]

## 3. `motor_agora > 0` com `DiaPago = 0`

**Se a comparação for para o mesmo colaborador, competência, dia de jornada e `DiaPago` de versão `motor`, com os mesmos períodos elegíveis à lavra, o zero gravado está divergente do que o motor da cópia lavraria hoje.** O motor calcula `minutos_noturnos_legais` pelas pontas efetivas, com desconto de intervalo, prorrogação conforme a configuração e aplicação do fator de hora noturna. A lavra da versão `motor` recebe os períodos e o mapa do dia de jornada, soma os minutos legais divididos por 60 e substitui as linhas daquela versão na competência. [app/ponto/motor_calculo_v2.py:983](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:983) · [app/ponto/motor_calculo_v2.py:2929](/home/ronald/wt-janela-turno-020503/app/ponto/motor_calculo_v2.py:2929) · [app/ponto/services/fechamento.py:1201](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1201) · [app/ponto/services/fechamento.py:1227](/home/ronald/wt-janela-turno-020503/app/ponto/services/fechamento.py:1227) · [app/ponto/services/dia_pago.py:173](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:173) · [app/ponto/services/dia_pago.py:359](/home/ronald/wt-janela-turno-020503/app/ponto/services/dia_pago.py:359)

Nesta execução, a ocorrência dessa divergência, a equivalência dos insumos, a data da lavra e a causa do eventual zero, inclusive se veio de uma lavra anterior à correção, ficam [nao verificado].

## 24/11 02:20 · sessao 481b8b509dc8 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

1. `UNIVERSO` e `emp_comp` — nenhuma linha fornecida no trecho da sonda. [nao verificado]
2. `INV` — nenhuma linha fornecida no trecho da sonda. [nao verificado]
3. Classes A, B e C — sem linhas `INV`, não há horários para calcular nem casos para classificar. [nao verificado]

O trecho fornecido termina com um erro do Docker na montagem de `/app/logs` e `rc_sombra=125`. [nao verificado]

## 24/11 02:17 · sessao fc72fded9863 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

**ANTES — C1 = 402.** As 17 classes somam 402. Entre os 320 casos `iguais_so_fechados`, todos estão em `motor_agora=0`. `anexo-sonda:3–20`

As referências `anexo-sonda:N` numeram a saída fornecida a partir de `sonda_leitura`, incluindo a linha em branco. A correspondência da cópia local com o hash informado e o conteúdo do script `sonda_c1c.py` ficam [nao verificado].

### 1. Contagem por classe

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | `anexo-sonda:4` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | `anexo-sonda:5` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `anexo-sonda:6` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `anexo-sonda:7` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | `anexo-sonda:8` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | `anexo-sonda:9` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | `anexo-sonda:10` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | `anexo-sonda:11` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | `anexo-sonda:12` |
| `entrada_repetida+virada_ou_outro_dia` | 1 | `anexo-sonda:13` |
| `iguais_com_aberto` | 62 | `anexo-sonda:14` |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | `anexo-sonda:15` |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | `anexo-sonda:16` |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 57 | `anexo-sonda:17` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | `anexo-sonda:18` |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | `anexo-sonda:19` |
| `virada_ou_outro_dia` | 3 | `anexo-sonda:20` |
| **Soma = C1** | **402** | `anexo-sonda:3–20` |

O trecho fornecido termina em `rc_sombra=0`, sem traceback para transcrever. `anexo-sonda:1–125`

### 2. Exemplos `fech` e corte da faixa noturna

A comparação usa a data e a hora dos limites da faixa que o par bruto alcançou. Nos pares da tarde, o limite relevante é 22h do próprio dia. Nos pares da madrugada, 05h é do dia civil das batidas. `anexo-sonda:73–84` · [motor_calculo_v2.py:97](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:97)

| Subclasse de `iguais_so_fechados` | `colab` | `dia` | `bruto` | `leg` | `pares` | A ponta efetiva cortou a faixa? | Fonte |
|---|---:|---|---:|---:|---|---|---|
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-19 | 10.5 | 0.0 | `[["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:73` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-21 | 6.3 | 0.0 | `[["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:74` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-10-25 | 17.6 | 0.0 | `[["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:75` |
| `bruto5-30\|motor_agora=0` | 108 | 2026-11-16 | 15.6 | 0.0 | `[["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:76` |
| `bruto<=5\|motor_agora=0` | 60 | 2026-10-31 | 3.5 | 0.0 | `[["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]]` | Sim, saída efetiva = 19:00 < 22:00 | `anexo-sonda:77` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-01 | 0.7 | 0.0 | `[["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:78` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-03 | 0.5 | 0.0 | `[["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:79` |
| `bruto<=5\|motor_agora=0` | 108 | 2026-10-05 | 0.9 | 0.0 | `[["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]]` | Sim, saída efetiva = 22:00 | `anexo-sonda:80` |
| `bruto>30\|motor_agora=0` | 37 | 2026-10-05 | 119.4 | 0.0 | `[["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 19:00:00", "2026-10-06 07:00:00", 0, 960.4]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:81` |
| `bruto>30\|motor_agora=0` | 37 | 2026-10-09 | 119.3 | 0.0 | `[["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 19:00:00", "2026-10-10 07:00:00", 0, 961.2]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:82` |
| `bruto>30\|motor_agora=0` | 113 | 2026-10-30 | 120.0 | 0.0 | `[["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 19:00:00", "2026-10-31 07:00:00", 0, 962.0]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:83` |
| `bruto>30\|motor_agora=0` | 113 | 2026-11-01 | 120.0 | 0.0 | `[["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 19:00:00", "2026-11-02 07:00:00", 0, 961.0]]` | Sim, entrada efetiva = 19:00 > 05:00 e posterior à saída | `anexo-sonda:84` |

Nos quatro exemplos `bruto>30`, o `dia` do registro antecede a data civil da entrada bruta. No código, `_janela_do_dia` busca os marcos por `entrada.date()`, e `entrada_efetiva` ancora o marco na data da batida. Essa lógica é compatível com o par efetivo invertido mostrado no anexo. O motor devolve zero quando a entrada é igual ou posterior à saída e só soma interseções de duração positiva. O corte nos demais casos de cada subclasse fica [nao verificado]. `anexo-sonda:81–84` · [motor_calculo_v2.py:1528](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1528) · [janela_he.py:20](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:20) · [janela_he.py:94](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:94) · [motor_calculo_v2.py:91](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:91) · [motor_calculo_v2.py:112](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:112)

### 3. `motor_agora>0` com `DiaPago=0`

**Para a mesma linha de `DiaPago` e o mesmo conjunto de períodos que entrariam na lavra, sim, no sentido de divergência frente ao cálculo atual.** Na versão `motor`, o fechamento entrega os períodos e o mapa `dia_da_jornada` a `lavrar`. A função usa esse mapa quando há dia para o período, soma `minutos_noturnos_legais / 60` em `horas_noturnas` e grava as linhas da competência e da versão. Se o total comparável agora é positivo, uma lavra com esses períodos atribuiria horas noturnas positivas à linha hoje zerada. [fechamento.py:1201](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1201) · [fechamento.py:1217](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1217) · [dia_pago.py:126](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:126) · [dia_pago.py:173](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:173) · [dia_pago.py:224](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:224) · [dia_pago.py:359](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:359)

O anexo mostra **zero casos `motor_agora>0` entre os 320 `iguais_so_fechados`**. A presença dessa divergência nas outras classes, a data e os insumos da lavra original e o motivo de eventual defasagem ficam [nao verificado]. `anexo-sonda:4–20`

## 24/11 02:15 · sessao d29f00865ac3 · NOTURNO-A-JANELA-NO-DIA-DO-TURNO · sonda · origem=Code

## 1. UNIVERSO e emp_comp

Transcrição literal do dado raspado fornecido. Execução da sonda e hashes [nao verificado] de forma independente.

```text
UNIVERSO pares_fechados 16167 invertidos 25 sha256 1247d31fd41f181b3e4f14c331ac9a98cf5d99d830c1833d6ce403daaa6aff5b
emp_comp 1 10/2026 fechados 12 invertidos 0
emp_comp 1 11/2026 fechados 14 invertidos 0
emp_comp 2 10/2026 fechados 6038 invertidos 6
emp_comp 2 11/2026 fechados 6337 invertidos 10
emp_comp 2 12/2026 fechados 275 invertidos 1
emp_comp 3 10/2026 fechados 1478 invertidos 4
emp_comp 3 11/2026 fechados 1408 invertidos 2
emp_comp 3 12/2026 fechados 61 invertidos 0
emp_comp 4 10/2026 fechados 276 invertidos 2
emp_comp 4 11/2026 fechados 258 invertidos 0
emp_comp 4 12/2026 fechados 10 invertidos 0
```

## 2. Linhas INV

Transcrição literal do mesmo dado raspado.

```text
INV 1010 2 11/2026 N 2026-11-05 2026-11-05 01:04:44 2026-11-05 05:59:33 2026-11-05 18:00:00 2026-11-05 05:59:33 0.0 1015.3 0.0 0.0 INV
INV 109 3 10/2026 N 2026-10-05 2026-10-05 22:00:41 2026-10-05 22:10:41 2026-10-05 22:00:41 2026-10-05 22:00:00 0.0 10.7 0.0 649.3 INV
INV 113 3 11/2026 N 2026-10-30 2026-10-31 03:00:00 2026-10-31 07:02:00 2026-10-31 19:00:00 2026-10-31 07:00:00 0.0 962.0 0.0 0.0 INV
INV 113 3 11/2026 N 2026-11-01 2026-11-02 03:00:00 2026-11-02 07:01:00 2026-11-02 19:00:00 2026-11-02 07:00:00 0.0 961.0 0.0 0.0 INV
INV 174 2 10/2026 N 2026-09-23 2026-09-24 02:32:11 2026-09-24 05:00:58 2026-09-24 21:00:00 2026-09-24 05:00:00 0.0 1108.8 0.0 0.0 INV
INV 174 2 11/2026 N 2026-10-25 2026-10-25 02:32:15 2026-10-25 04:57:35 2026-10-25 21:00:00 2026-10-25 04:57:35 0.0 1107.7 0.0 0.0 INV
INV 200 2 11/2026 N 2026-11-02 2026-11-03 02:00:33 2026-11-03 06:00:15 2026-11-03 22:00:00 2026-11-03 06:00:00 0.0 1199.7 0.0 0.0 INV
INV 218 2 10/2026 N 2026-10-15 2026-10-16 03:15:06 2026-10-16 05:00:01 2026-10-16 21:00:00 2026-10-16 05:00:00 0.0 1064.9 0.0 0.0 INV
INV 218 2 11/2026 N 2026-11-14 2026-11-15 03:17:11 2026-11-15 05:00:03 2026-11-15 21:00:00 2026-11-15 05:00:00 0.0 1062.9 0.0 0.0 INV
INV 218 2 12/2026 N 2026-11-21 2026-11-22 03:22:03 2026-11-22 05:00:16 2026-11-22 21:00:00 2026-11-22 05:00:00 0.0 1058.2 0.0 0.0 INV
INV 296 2 11/2026 N 2026-11-02 2026-11-03 00:15:19 2026-11-03 07:00:41 2026-11-03 19:00:00 2026-11-03 07:00:00 0.0 1125.4 0.0 0.0 INV
INV 37 4 10/2026 N 2026-10-05 2026-10-06 03:00:38 2026-10-06 07:01:01 2026-10-06 19:00:00 2026-10-06 07:00:00 0.0 960.4 0.0 0.0 INV
INV 37 4 10/2026 N 2026-10-09 2026-10-10 03:00:40 2026-10-10 07:01:50 2026-10-10 19:00:00 2026-10-10 07:00:00 0.0 961.2 0.0 0.0 INV
INV 382 2 10/2026 N 2026-09-30 2026-10-01 01:01:26 2026-10-01 07:50:17 2026-10-01 23:50:00 2026-10-01 07:50:00 0.0 1368.9 0.0 0.0 INV
INV 382 2 11/2026 N 2026-11-10 2026-11-11 00:02:41 2026-11-11 07:51:20 2026-11-11 23:50:00 2026-11-11 07:50:00 0.0 1428.7 0.0 0.0 INV
INV 399 2 10/2026 N 2026-09-30 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:29 2026-09-30 18:30:00 0.0 0.5 0.0 0.0 INV
INV 456 2 11/2026 N 2026-11-14 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:08 2026-11-14 19:00:00 0.0 0.1 0.0 0.0 INV
INV 473 2 10/2026 N 2026-09-21 2026-09-21 19:00:18 2026-09-21 19:23:54 2026-09-21 19:00:18 2026-09-21 19:00:00 0.0 23.9 0.0 0.0 INV
INV 515 3 10/2026 N 2026-10-09 2026-10-10 02:59:26 2026-10-10 07:03:44 2026-10-10 19:00:00 2026-10-10 07:00:00 0.0 964.3 0.0 0.0 INV
INV 515 3 10/2026 N 2026-10-11 2026-10-12 03:00:00 2026-10-12 07:05:26 2026-10-12 19:00:00 2026-10-12 07:00:00 0.0 965.4 0.0 0.0 INV
INV 673 2 11/2026 N 2026-10-21 2026-10-22 06:57:21 2026-10-22 07:00:00 2026-10-22 19:00:00 2026-10-22 07:00:00 0.0 722.6 0.0 0.0 INV
INV 841 2 11/2026 N 2026-11-13 2026-11-14 06:53:31 2026-11-14 07:05:54 2026-11-14 19:00:00 2026-11-14 07:00:00 0.0 732.4 0.0 0.0 INV
INV 868 3 10/2026 N 2026-10-10 2026-10-10 02:29:34 2026-10-10 06:58:56 2026-10-10 19:00:00 2026-10-10 06:58:56 0.0 990.4 0.0 0.0 INV
INV 922 2 11/2026 N 2026-10-27 2026-10-29 04:07:39 2026-10-29 07:02:13 2026-10-29 23:00:00 2026-10-29 07:00:00 0.0 1134.6 0.0 0.0 INV
INV 973 2 10/2026 N 2026-10-07 2026-10-08 06:55:11 2026-10-08 07:00:43 2026-10-08 19:00:00 2026-10-08 07:00:00 0.0 725.5 0.0 0.0 INV
```

## 3. Classificação e conta por INV

- **A\* — 18 linhas pelo limite efetivo de entrada.** A data civil da entrada difere do dia da jornada. O motor consulta o marco pela data civil da batida, e o recorte leva a entrada efetiva para depois da saída bruta em todas as 18 linhas. A regra admite `limite efetivo = marco nominal − extra declarada antes`. A igualdade com o marco nominal em cada linha fica **[nao verificado]**. [app/ponto/motor_calculo_v2.py:1528][dia] · [app/ponto/motor_calculo_v2.py:1702][entrada-motor] · [app/ponto/janela_he.py:94][entrada]
- **B — 4 linhas.** Em cada uma, `saída efetiva < entrada bruta ≤ saída bruta`, com a entrada intacta. A saída recortada fica no marco de saída ou depois dele se houver extra declarada. A desigualdade põe o par bruto inteiro depois do marco. [app/ponto/janela_he.py:143][saida] · [app/escala/models.py:83][extra-saida]
- **C — 3 linhas.** A entrada bruta está no próprio dia da jornada. O recorte desloca apenas a entrada, que passa da saída bruta. A saída efetiva permanece igual à saída bruta. [app/ponto/motor_calculo_v2.py:1760][pontas] · [app/ponto/janela_he.py:94][entrada] · [app/ponto/janela_he.py:143][saida]

Cada linha abaixo usa os horários da INV na mesma posição do bloco acima. A última coluna faz `efetivo_saida − efetivo_entrada` em `hh:mm:ss`, antes de limitar a duração trabalhada a zero. [app/ponto/motor_calculo_v2.py:2888][duracao] · [app/ponto/motor_calculo_v2.py:2893][trabalhado]

| INV · colab | Classe | Jornada → dia civil da entrada | Entrada bruta → efetiva | Saída bruta → efetiva | Saída efetiva − entrada efetiva |
|---|---|---|---|---|---:|
| 01 · 1010 | [C][pontas] | 05/11 → 05/11 | 01:04:44 → 18:00:00 | 05:59:33 → 05:59:33 | −12:00:27 |
| 02 · 109 | [B][saida] | 05/10 → 05/10 | 22:00:41 → 22:00:41 | 22:10:41 → 22:00:00 | −00:00:41 |
| 03 · 113 | [A*][entrada] | 30/10 → 31/10 | 03:00:00 → 19:00:00 | 07:02:00 → 07:00:00 | −12:00:00 |
| 04 · 113 | [A*][entrada] | 01/11 → 02/11 | 03:00:00 → 19:00:00 | 07:01:00 → 07:00:00 | −12:00:00 |
| 05 · 174 | [A*][entrada] | 23/09 → 24/09 | 02:32:11 → 21:00:00 | 05:00:58 → 05:00:00 | −16:00:00 |
| 06 · 174 | [C][pontas] | 25/10 → 25/10 | 02:32:15 → 21:00:00 | 04:57:35 → 04:57:35 | −16:02:25 |
| 07 · 200 | [A*][entrada] | 02/11 → 03/11 | 02:00:33 → 22:00:00 | 06:00:15 → 06:00:00 | −16:00:00 |
| 08 · 218 | [A*][entrada] | 15/10 → 16/10 | 03:15:06 → 21:00:00 | 05:00:01 → 05:00:00 | −16:00:00 |
| 09 · 218 | [A*][entrada] | 14/11 → 15/11 | 03:17:11 → 21:00:00 | 05:00:03 → 05:00:00 | −16:00:00 |
| 10 · 218 | [A*][entrada] | 21/11 → 22/11 | 03:22:03 → 21:00:00 | 05:00:16 → 05:00:00 | −16:00:00 |
| 11 · 296 | [A*][entrada] | 02/11 → 03/11 | 00:15:19 → 19:00:00 | 07:00:41 → 07:00:00 | −12:00:00 |
| 12 · 37 | [A*][entrada] | 05/10 → 06/10 | 03:00:38 → 19:00:00 | 07:01:01 → 07:00:00 | −12:00:00 |
| 13 · 37 | [A*][entrada] | 09/10 → 10/10 | 03:00:40 → 19:00:00 | 07:01:50 → 07:00:00 | −12:00:00 |
| 14 · 382 | [A*][entrada] | 30/09 → 01/10 | 01:01:26 → 23:50:00 | 07:50:17 → 07:50:00 | −16:00:00 |
| 15 · 382 | [A*][entrada] | 10/11 → 11/11 | 00:02:41 → 23:50:00 | 07:51:20 → 07:50:00 | −16:00:00 |
| 16 · 399 | [B][saida] | 30/09 → 30/09 | 18:30:29 → 18:30:29 | 18:30:29 → 18:30:00 | −00:00:29 |
| 17 · 456 | [B][saida] | 14/11 → 14/11 | 19:00:08 → 19:00:08 | 19:00:08 → 19:00:00 | −00:00:08 |
| 18 · 473 | [B][saida] | 21/09 → 21/09 | 19:00:18 → 19:00:18 | 19:23:54 → 19:00:00 | −00:00:18 |
| 19 · 515 | [A*][entrada] | 09/10 → 10/10 | 02:59:26 → 19:00:00 | 07:03:44 → 07:00:00 | −12:00:00 |
| 20 · 515 | [A*][entrada] | 11/10 → 12/10 | 03:00:00 → 19:00:00 | 07:05:26 → 07:00:00 | −12:00:00 |
| 21 · 673 | [A*][entrada] | 21/10 → 22/10 | 06:57:21 → 19:00:00 | 07:00:00 → 07:00:00 | −12:00:00 |
| 22 · 841 | [A*][entrada] | 13/11 → 14/11 | 06:53:31 → 19:00:00 | 07:05:54 → 07:00:00 | −12:00:00 |
| 23 · 868 | [C][pontas] | 10/10 → 10/10 | 02:29:34 → 19:00:00 | 06:58:56 → 06:58:56 | −12:01:04 |
| 24 · 922 | [A*][entrada] | 27/10 → 29/10 | 04:07:39 → 23:00:00 | 07:02:13 → 07:00:00 | −16:00:00 |
| 25 · 973 | [A*][entrada] | 07/10 → 08/10 | 06:55:11 → 19:00:00 | 07:00:43 → 07:00:00 | −12:00:00 |

[dia]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1528
[entrada-motor]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1702
[entrada]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:94
[saida]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/janela_he.py:143
[extra-saida]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/escala/models.py:83
[pontas]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:1760
[duracao]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2888
[trabalhado]: /home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2893

## 24/11 02:12 · sessao e9d3f1c19b9b · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

**Pré-verificação da parte (1) — bloqueia.** A regra nova pode alcançar uma competência exportada, o mesmo dia pode receber impressões diferentes no cron e no julgamento avulso, e o novo `FURO_PARCIAL` pode reter a folha sem produzir uma pergunta que a cobrança saiba fazer. `app/ponto/services/cartorio.py:333–337,1188–1235,1480–1497,1916–1934,2143–2145`, `app/folha/export.py:1285–1294`

Usei o diff fornecido e o código da cópia, em leitura. Confirmação independente do SHA e do pai, execução do RED e da suíte, volumes de produção e efeito em valores ou rubricas [nao verificado]. [nao verificado]

## Achados

1. **Bloqueia — o horizonte não protege a competência exportada.** `HORIZONTE_SEM_PAR = '2026-11'` habilita toda competência de novembro de 2026 em diante pelo mês do fim da janela da empresa. Uma competência 11, 12 ou posterior com TXT vigente passa pelo mesmo teste. O cartório pode relavrar a célula porque o caminho novo não consulta `competencia_entregue` (K6). A guarda temporária existente seleciona setembro de 2026 e atua no recálculo de `FechamentoMensal`, em outra porta. O selo novo verifica datas anteriores ao piso e declara que a guarda de exportação fica fora do teste. O piso preserva o alcance para frente da **L-124**, mas não cumpre a restrição particular desta obra. A **L-113** altera a **L-092** para recálculos e preserva o TXT anterior. Essa regra geral não substitui o recorte explícito desta parte. `app/ponto/services/cartorio.py:316–337,1223–1235,1953–1977`, `app/ponto/supra_juiz.py:97–105`, `app/folha/export.py:556–589`, `app/ponto/services/fechamento.py:27–42,558–575`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:221–247`, `app/docs/LEIS.md:125,145,155`

2. **Bloqueia — a janela altera o sinal e o pulo do cron.** O cron das 06:27 chama `julgar_colab` com todas as células passadas do colaborador, em ordem, sem `--forcar`. `julgar_celula` passa `[cel]`. A segunda passada do próprio lote pode usar só as células reconciliadas. Cada corpo tira `d0,d1` das pontas da lista. O pareador monta segmentos por vínculo, lê de `ini−2` a `fim+1` e só depois filtra por `data_turno`. O contrato de `dias_encerrados` explicita que o pareamento depende da janela. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59–65`, `app/ponto/services/cartorio.py:1073–1077,1188–1229,2142–2145`, `app/ponto/turnos.py:1834–1847,1867–1871,1963–2002,2133–2139`

   Um contraexemplo por leitura estática é a E às 23:30 de D e a S às 00:30 de D+2, em vínculo diurno 08–14, sem outras batidas nem papel de pausa ou eco. A janela avulsa exclui a S e deixa a E aberta. A ampla pode fechar o par de 25h no envelope de 26h porque as duas pontas estão longe dos respectivos marcos. Depois de expirar a aberta, o avulso pode gravar `sem_par:E` e o cron omitir a parte. A S em D+2 também fica fora das batidas diretamente hasheadas para D. Com os demais insumos iguais, o cron pode pular um dia que o avulso rejulgaria ou relavrar após o avulso sem fato novo. Isso quebra a coerência e a idempotência exigidas pela **L-099**. Incidência em produção [nao verificado]. `app/ponto/turnos.py:312–340,1377–1422,1430–1454,1491–1502,2005–2035`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/services/cartorio.py:1233–1235,601–603,1435–1440,1479–1497`, `app/docs/LEIS.md:122`

3. **Bloqueia — o sinal e o realizado podem vir de pareações diferentes dentro da mesma lavra.** Encerramento e sinal compartilham uma lista, de `turnos_do_colab(c,d0,d1)`, alimentada com a ata persistida. Já a grade usada na comparação de dia cheio pareia com `ref=d1+1` e com uma cópia das lâmpadas e órfãs **desta montagem**. A grade documenta um caso em que a ata anterior mudou os minutos para as mesmas batidas. É o mesmo algoritmo registrado, chamado com duas alimentações. Nos comandos, a mistura é entre a ata lavrada e o sinal calculado agora. O diff não introduz fórmula de minutos nem outro algoritmo de pareamento, mas introduz essa segunda resposta como insumo da decisão. **L-002 e L-099.** `app/ponto/services/cartorio.py:1208–1231,1252–1259`, `app/ponto/turnos.py:1889–1892,1940–1944`, `app/escala/utils.py:1873–1909,2052–2072`, `app/escala/services/leitor_celula.py:397–424`, `app/ponto/management/commands/supra_juiz.py:120–132`, `app/ponto/management/commands/emitir_furo_retroativo.py:60–66`, `app/docs/LEIS.md:60,122`

4. **Bloqueia — a condição diverge do corte em “marcos acesos” e na destinação da espúria.** O ramo novo faz o seguinte. `app/ponto/supra_juiz.py:253–259,290–314,340–341,493–512`

   | Cláusula | Implementação e divergência |
   |---|---|
   | Dia fechado | Usa o veredito de `dias_encerrados`, que consulta turno aberto vivo e compara o instante com o fim previsto. O cartório zera E/S enquanto esse juiz não fecha o dia, inclusive quando o vigia passa `turno_encerrado=True` para julgar só até o marco vencido. O ramo novo atende ao teto da **L-073**, sujeito à alimentação descrita acima. `app/ponto/turnos.py:2121–2130,2166–2182`, `app/ponto/services/cartorio.py:1223–1231,1560–1564`, `app/docs/LEIS.md:100` |
   | E sem par com marcos acesos | Aceita a E inclusive com realizado zero, como pede o literal. Porém `not _n_missing` só conta dicionários com `missing` verdadeiro. Lista vazia ou coluna `None` passa. A montagem da ata pode lavrar `acesa=False` ou `None` para um marco do DNA nessas condições. Há um segundo predicado para a mesma resposta que a lâmpada já expressa. **L-061 e L-002.** `app/ponto/supra_juiz.py:340–341,504–512`, `app/ponto/services/cartorio.py:685–728`, `app/docs/LEIS.md:60,95` |
   | S órfã com realizado positivo | Usa a S do pareador e exige `real > 0`. A condição de zero `missing` também foi aplicada à S, embora o literal associe “marcos acesos” à E. Os ramos antigos ainda podem acusar quando há `missing`. O novo exige trabalho e previsto não zero, exclui isento, sem âncora, justificado e qualquer cobertura aprovada, inclusive parcial. Se passar com `sem_vinculo`, emite código de cadastro. `app/ponto/supra_juiz.py:253–259,290–304,425–485,504–512` |
   | Dia cheio e espúria | `real < prev` exclui igualdade e excedente do ramo novo, em minutos do dicionário da grade. Os ramos antigos de marco apagado continuam aplicáveis. Para a E residual, o ramo apenas suprime o furo novo. A pergunta de espúria está em `SEM_JUIZ` e o fluxo existente de cluster exige ao menos duas órfãs e uma lâmpada apagada. A E única que sobra em dia cheio não é encaminhada por esse fluxo. O exemplo 726/720 consta como relato e fixture, sem confirmação independente da lavra de col63 [nao verificado]. `app/ponto/supra_juiz.py:257–259,451–461,499–512`, `app/core/juizes.py:1223–1225`, `app/ponto/services/cluster_espurio.py:15–36`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:57–64`, `app/docs/RELATO.md:1` |
   | Só competência não exportada | Falta a consulta do estado de entrega, conforme achado 1. `app/ponto/services/cartorio.py:333–337`, `app/folha/export.py:556–589` |

5. **Bloqueia — `FURO_PARCIAL` adquire dois significados que os leitores tratam como um só.** Antes identificava marco apagado com fato. Agora também identifica um resíduo do pareador quando a grade não tem `missing`. Ambos têm precedência para `veredito='furo'`, no mesmo escritor. A **L-088** liga furo a marco não cumprido e pergunta em aberto. O corte novo manda acusar na lavra, mas os seguintes consumidores continuam operando pelo contrato anterior. `app/ponto/supra_juiz.py:451–485,504–512`, `app/escala/models.py:1622–1642`, `app/ponto/portas/celula.py:1096–1105,1126–1134`, `app/docs/LEIS.md:120`

   - **Chamados e cobrança.** O emissor só abre ou espelha cobrança se achar marco apagado nomeável na ata. A fábrica também precisa de motivo de marco. Com as lâmpadas realmente acesas, o novo protesto pode alimentar o contador de furos cobrados e o reprocessamento, que ainda pode devolver `sem_cobranca_depois`. C6 e o reconciliador passam a ler um veredito acusador nos caminhos de perguntas existentes, sujeitos às suas guardas. `app/ponto/services/cartorio.py:1916–1943,2032–2046`, `app/chamados/services/fabrica_da_celula.py:52–74,119–139`, `app/ponto/services/furos_sem_cobranca.py:35,53–63,82–108,117–131`, `app/chamados/juizes.py:1539–1562`, `app/chamados/reconciliador.py:928–963,979–1004`
   - **Folha.** A retenção lê o código e o veredito gravados. Um dia que passe de `concorde` a `furo`, com todos os marcos acesos, pode virar `espelho_furo` e deixar o colaborador fora do TXT. A exceção da **L-033** exige ao menos uma pausa apagada e o conjunto exato `{'FURO_PARCIAL'}`. Com todas acesas, reter demonstra a mudança de elegibilidade, sem demonstrar infração à cláusula *só pausa*. Prévia e lista de fora da folha também leem o resultado. Valores, rubricas e quantidade de retidos em produção [nao verificado]. `app/folha/export.py:968–973,1148–1155,1173,1273–1294`, `app/folha/previa.py:170–171`, `app/folha/fora_da_folha.py:120–121`, `app/docs/LEIS.md:81`
   - **Status, tela e Pauta DP.** O status lavrado passa de `ok` a `irregular` quando o `furo` vence, depois da guarda temporal. Calendário, relatório, painel e app leem o novo estado. A palavra `Em aberto` do calendário ainda vem da lista de E/S do pareador e pode persistir em dia cheio com status `ok`. O novo furo também pode vencer `REALIZADO_ZERO_COM_TURNO` e tirar o dia da Pauta DP dos `discordantes`, embora não haja marco para cobrar. A igualdade entre palavra e status no caso cheio continua pendente pela **L-061/L-099**. `app/ponto/status_dia.py:65–70,154–162,273–289`, `app/colaboradores/services/calendario.py:595–616,748–763`, `app/ponto/services/dia_decidido.py:346–355,419–430`, `app/relatorios/furos.py:119–127`, `app/core/services/painel_op.py:325–336`, `app/api/views.py:1493–1514`, `app/ponto/supra_juiz.py:316–319,504–512`, `app/ponto/services/pautas_dos_discordantes.py:27–31,39–41,76–81,196–213`, `app/docs/LEIS.md:95,122`

6. **Bloqueia — os selos mordem o ramo simples, mas deixam as fronteiras acima sem prova.** Os casos puros de E e S exigem `FURO_PARCIAL`, o de hash distingue `''`, `E`, `S` e `ES`, e o teste avulso morde o pulo com e sem sinal na mesma janela. A comparação entre `sem_par=''` e o default só compara duas chamadas ao código novo. Os casos integrados positivos verificam o sinal bruto e `out['cods']`, sem afirmar a ata persistida com todos os marcos acesos nem o realizado usado. O caso cheio integrado só verifica ausência do código. Faltam ao selo a paridade cron/avulso, a competência **dentro** do horizonte com exportação vigente e o caminho folha/cobrança. A atualização do censo de linhas mantém a catraca de sítios, sem testar essas propriedades. **L-110 e Lei-Akita 5.** RED efetivamente rodado contra o pai e suíte inteira [nao verificado]. `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–102,155–247,255–263`, `app/core/tests/test_medida_turno_do_par.py:291,297,319`, `app/docs/LEIS.md:136`, `CLAUDE.md:28–29,383–392`

7. **Fila — relógio, ordem de trava, pouso e fallbacks.** O comando `supra_juiz` já omitia `agora`, mas a extração muda o instante da leitura interna para **depois** da carga do pareador. O retroativo injeta o instante da borda. Ambos ainda consultam `localdate()` por linha. O cartório injeta `agora` no encerramento, e o avulso conserva as leituras pré-existentes de `localdate()` e `now()` em sua guarda. É desvio do padrão de borda da **L-128**, distinto de usar data civil como teto do ramo novo. `app/ponto/management/commands/supra_juiz.py:120–124,235–237`, `app/ponto/management/commands/emitir_furo_retroativo.py:47,63–66,113–115`, `app/ponto/turnos.py:2148–2163`, `app/ponto/services/cartorio.py:1216–1221,2119–2145`, `app/docs/LEIS.md:150`, `app/docs/PLANO-PADROES.md:97–103`

   - **Escritor, trava e `except`.** O diff acrescenta leituras e código de saída, preserva as portas `lavrar_veredito` e `carimbar` e não introduz `except` nem aquisição nova de trava. O save e seu evento seguem juntos na porta existente. Há sobreposição com o juiz registrado de **marco faltante**, pela aproximação de “acesos” e pelo código reutilizado. O helper em si agrega a saída do pareador já registrado. `app/ponto/services/cartorio.py:1208–1235,1953–1977`, `app/ponto/portas/celula.py:993–1001,1217–1237`, `app/core/juizes.py:193–207,216–248`, `app/escala/servico_jornada.py:452–489`, `app/ponto/supra_juiz.py:86–105,340–341,504–512`
   - **Fallback e estados comprimidos.** Os kwargs são obrigatórios. O mapa ausente e `(False, False)` colapsam ausência de sinal e exclusão pelo horizonte, e o cartório também usa o par falso para dia em curso. `turnos or ()`, `getattr(t, 'aberto', False)` e o `getattr` da empresa ainda convertem carga ou atributo ausente em vazio/falso ou corte padrão. Os caminhos de produção examinados fornecem a lista e o modelo exige empresa. Ocorrência de entrada malformada [nao verificado]. `app/ponto/supra_juiz.py:97–105,239–244`, `app/ponto/services/cartorio.py:1223–1235`, `app/ponto/janelas.py:36–51`, `app/colaboradores/models.py:321`
   - **Pré-requisito.** O corte de 14:16 pôs PAR-PELO-TURNO à frente. O aval de 23:55 autorizou construção em raias paralelas e pousos separados. A cópia ainda usa o teto previsto+4h que o V6 manda retirar do corte do par. O commit em cópia não comprova pouso antecipado. Ordem efetiva, reconstrução das projeções pela **L-121** e DIFF/aval de impacto aplicável pela **L-094** [nao verificado]. `app/docs/CORTES.md:629–630`, `app/docs/PROMPTS.md:2852,2863`, `app/ponto/turnos.py:1423–1454`, `app/docs/LEIS.md:112,142`
   - **L-102/L-103.** O ramo lê minutos e acrescenta protesto. A regra de apresentação para dia inteiro sem turno pareado e a escrita explícita de zero continuam nas respectivas fontes. Uma E ou S residual pode coexistir com pares fechados. `app/docs/LEIS.md:128–129`, `app/escala/utils.py:2052–2072`, `app/ponto/services/dia_decidido.py:410–418`, `app/ponto/supra_juiz.py:257–259,504–512`

## Censo de origem, leitores e chamadores

| Valor | Quem produz e quem lê |
|---|---|
| `entrada_sem_par`, `saida_orfa` | `_mk` grava `Turno.aberto` quando há entrada e falta saída. `datas_saida_orfa` lê `Turno` com saída presente e entrada ausente. `sem_par_do_pareador` agrega por `data_turno` em `{date: (bool E, bool S)}` e filtra pelo horizonte. `classificar_dia` recebe os dois kwargs e os lê no ramo novo. O calendário lê os sinais de origem em sua própria carga. `app/ponto/turnos.py:234–245,312–324`, `app/ponto/supra_juiz.py:86–105,239–244,504–512`, `app/colaboradores/services/calendario.py:602–616` |
| `HORIZONTE_SEM_PAR` | Constante de módulo `2026-11`, validada no import. `sem_par_no_horizonte` lê a competência do dia do turno por `janela_atual`, pelo corte da empresa, e o helper consome esse resultado. Os testes usam também `empresa=None`, com corte padrão 21. `app/ponto/services/cartorio.py:316–337`, `app/ponto/janelas.py:24–33,36–51,60–63`, `app/ponto/supra_juiz.py:97–105`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:108–123,236–243` |
| `sem_par:` e `CelulaDia.impressao` | `_sem_par_da_impressao` serializa `''`, `E`, `S` ou `ES`, já com o filtro de encerramento. `impressao_insumos` inclui o texto quando preenchido e devolve SHA-256 em hexadecimal. A porta `carimbar` persiste o digest. Os dois ramos do cartório comparam o digest para pular, e `celula_veredito_velho` o copia para a fotografia de reversão. O ramo sem adesão omite `sem_par` na impressão e só entrega os kwargs ao classificador depois do pulo. O ramo normal pode trocar a impressão até em dia cheio ou coberto, pois calcula o hash antes da decisão. `app/ponto/services/cartorio.py:513–514,598–603,1223–1235,1283–1292,1337–1355,1480–1500`, `app/ponto/portas/celula.py:1217–1237`, `app/ponto/management/commands/celula_veredito_velho.py:103–110` |
| Previsto, realizado, marcos e encerramento | O cartório usa a grade nova. Ela materializa minutos líquidos do dia pelos marcos e pausas, e usa `realizado_do_dia` sobre os turnos da montagem. Os dois comandos usam `grade_da_celula`, que transporta minutos da ata e consulta o juiz de previsto quando falta o campo. A grade também fornece `celulas`, `tipo_dia` e `justificado`, e o cartório escreve a ata pela porta de lavra. O encerramento vem do juiz registrado `dia_encerrado`, pela carga de janela. `app/ponto/services/cartorio.py:1216–1221,1252–1259,777–815,1953–1977`, `app/escala/utils.py:1620–1650,1818–1822,2052–2081,2241–2245`, `app/escala/services/leitor_celula.py:397–440`, `app/core/juizes.py:193–202,243`, `app/ponto/turnos.py:2121–2182` |

**Chamadores de produção.** `classificar_dia` tem quatro sítios diretos. Cartório sem adesão e normal passam o par filtrado pelo encerramento. `supra_juiz` e `emitir_furo_retroativo` passam E/S após o horizonte, junto do encerramento em outro kwarg. Naqueles quatro sítios a chave é dia do turno, os sinais são booleanos e a referência de tempo é o fuso Django ativo, cujo padrão é `America/Sao_Paulo`. A S de madrugada pode pertencer à véspera. Há uma diferença pré-existente no insumo vizinho `batidas_no_dia`, que os dois comandos contam por data civil e o cartório por células/órfãs do dia-turno. No comando `supra_juiz`, o código novo alimenta placar e exemplos, e a retratação lê a lavra. O retroativo só emite mediante `FURO_SEM_COBRANCA`. `impressao_insumos` tem os dois sítios do cartório, com reexportação pelo comando do cron. `app/ponto/services/cartorio.py:1337–1355,1480–1486,1501–1508,1553–1565`, `app/ponto/management/commands/supra_juiz.py:225–255`, `app/ponto/management/commands/emitir_furo_retroativo.py:103–117`, `app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/turnos.py:43–48,285–324`, `app/config/settings/base.py:93`

**Chamadores diretos em testes.** As linhas abaixo identificam os sítios. Os testes antigos do classificador injetam `False, False`, inclusive em fixtures com órfãs da grade, que são outro conceito. O teste novo injeta E/S sintéticos e omite os kwargs deliberadamente no teste de `TypeError`. Para a impressão, os testes antigos usam o default e o teste novo exercita os quatro valores. Os proxies e spies transportam os kwargs ou substituem a carga, de modo que os testes sintéticos não comprovam paridade de janela, data ou fuso entre as portas reais. `app/ponto/tests/test_a_tela_sem_par_acusa.py:34–102,255–263`, `app/ponto/tests/test_supra_juiz_orfa_builder.py:15–27`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:145–159,191–212,271,337–382,404–409`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:135–181`, `app/chamados/tests/test_e4c_f5_voltaC_lotes.py:208–215`

- `classificar_dia` em `app/escala/tests/` — `test_cobertura_parcial_periodo.py:118`, `test_hx_numero_honesto.py:79,88,99,108,124,138,150`, `test_montador_realizado_pela_autoridade.py:173`.
- `classificar_dia` em `app/ponto/tests/` — `test_a_tela_sem_par_acusa.py:43,102`, `test_bo_inss_nunca_bateu_le_a_precedencia.py:273`, `test_bug_lastro_vai_e_volta.py:26,41`, `test_cobparc8_leitor_transporta_justificado.py:80`, `test_cobparc_marco_no_periodo.py:99,170,281,293,489`, `test_concorde_sem_entrada.py:31`, `test_contagem_unica.py:47,51,61,65,74,85,142`, `test_f1c4_3_supra_indefinido_nao_retrata.py:24`, `test_f1c4_3_supra_pergunta_ao_juiz.py:32`, `test_fnd_pergunta_a_juiza.py:86,104,189`, `test_grade_da_celula_cobertura_parcial.py:45`, `test_hx_cobranca_morta.py:23`, `test_supra_juiz.py:26,35,42,55,62,69,77,94,103,110,130,137,144,159,163,169,184,185,215,222,235,245,251`, `test_supra_juiz_cobranca.py:21,70,96`, `test_supra_juiz_fase.py:22`, `test_supra_juiz_indefinido.py:19`, `test_supra_juiz_inflado.py:23,28,33,38,43`, `test_supra_juiz_orfa.py:28,31,34,37,40,43`, `test_supra_juiz_orfa_builder.py:18,20,27`, `test_supra_juiz_teto.py:15,28`.
- `impressao_insumos` em `app/escala/tests/test_cobertura_parcial_periodo.py:205–209` e em `app/ponto/tests/` — `test_a_tela_sem_par_acusa.py:257,261–262`, `test_cartorio.py:16–35`, `test_celula_concorde_meio_turno.py:56–57,86–88`, `test_e3_isento_na_impressao.py:49–55,81–100`, `test_o146_extra_decla

[truncado em 20000 de 20290 caracteres]

## 24/11 02:10 · sessao 5a41b3c84321 · - · - · origem=Ronald (tmux)

**Bloqueia.** O mesmo colaborador-dia recebe sinais de pareamento produzidos com janelas distintas. O cron entrega toda a série de células passadas, o avulso entrega `[cel]`, e o segundo passe do cron entrega somente as reconciliadas. O pareador lê a janela com `ini−2` e `fim+1`, segmenta conforme o vínculo e filtra os resultados por `data_turno`. O sinal entra no hash só quando presente. Quando os sinais divergem, o mesmo dia pode pular no cron ou ser relavrado com outra impressão. Casos e volume em produção [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:49–65`, `app/ponto/services/cartorio.py:1073–1077`, `app/ponto/services/cartorio.py:1188–1235`, `app/ponto/services/cartorio.py:1480–1497`, `app/ponto/services/cartorio.py:2142–2145`, `app/ponto/turnos.py:1834–1847`, `app/ponto/turnos.py:1961–2002`.

**Bloqueia.** O sinal lê geometria alimentada por atas gravadas. O realizado do cartório lê geometria alimentada por lâmpadas e órfãs da montagem corrente, em outra janela. Os dois caminhos usam o mesmo pareador, com entradas diferentes. Os comandos combinam o pareamento atual com minutos da última ata lavrada. `app/ponto/services/cartorio.py:1213`, `app/ponto/services/cartorio.py:1252–1259`, `app/ponto/turnos.py:1889–1892`, `app/ponto/turnos.py:1942–1944`, `app/escala/utils.py:1889–1909`, `app/escala/utils.py:2052–2072`, `app/escala/services/leitor_celula.py:397–424`.

**Bloqueia.** `not _n_missing` aceita ausência de células, posições `None` e dispensas, porque a contagem só considera dicionários com `missing` verdadeiro. O ramo exige essa condição também para S órfã, além de trabalho, previsão positiva na prática, falta de cobertura aprovada, falta de justificativa e âncora. Nas coberturas parciais, o novo ramo fecha a porta que a classificação anterior admite para o resto do dia. `app/ponto/supra_juiz.py:339–347`, `app/ponto/supra_juiz.py:472–485`, `app/ponto/supra_juiz.py:504–512`.

**Censo.** `Turno.aberto` é escrito pelo pareador, a lista de S órfãs lê o `Turno` com entrada ausente e saída presente, e `sem_par_do_pareador` agrega ambos em `date: (bool E, bool S)`. É dia-turno atribuído no fuso local Django, inclusive véspera no noturno, e os minutos são inteiros. As quatro chamadas de produção usam essa semântica, mas o cartório suprime os dois flags antes da chamada se o turno está em curso, enquanto os comandos passam o resultado bruto pós-horizonte com `turno_encerrado` separado. As contagens de `batidas_no_dia` que os acompanham diferem, dia civil nos comandos e dia-turno no cartório. `app/ponto/turnos.py:43–48`, `app/ponto/turnos.py:234–245`, `app/ponto/turnos.py:285–324`, `app/config/settings/base.py:93`, `app/ponto/supra_juiz.py:88–105`, `app/ponto/services/cartorio.py:1223–1235`, `app/ponto/services/cartorio.py:1349–1355`, `app/ponto/services/cartorio.py:1501–1508`, `app/ponto/services/cartorio.py:1553–1565`, `app/ponto/management/commands/supra_juiz.py:225–239`, `app/ponto/management/commands/emitir_furo_retroativo.py:103–116`.

**Censo.** As duas chamadas de produção de `impressao_insumos` estão no cartório. A normal passa `E/S/ES` ou `''`. A sem adesão usa o default `''` no hash, embora entregue flags ao classificador se passar da guarda, e vem de ausência de primeira batida apurável. O token entra no SHA-256 e só o digest é persistido, por `carimbar`. O comando de snapshot lê o digest. `HORIZONTE_SEM_PAR` é constante validada `2026-11`, com único leitor funcional na comparação da competência pelo fim da janela da empresa. Censo de testes com todas as linhas enviado à coordenação. `app/ponto/services/cartorio.py:316–337`, `app/ponto/services/cartorio.py:513–514`, `app/ponto/services/cartorio.py:598–603`, `app/ponto/services/cartorio.py:1283–1285`, `app/ponto/services/cartorio.py:1337–1355`, `app/ponto/services/cartorio.py:1480–1486`, `app/ponto/services/cartorio.py:1976–1977`, `app/ponto/portas/celula.py:1217–1237`, `app/ponto/management/commands/celula_veredito_velho.py:103–110`, `app/ponto/supra_juiz.py:105`, `app/ponto/janelas.py:24–33`, `app/ponto/janelas.py:60–63`.

## 24/11 02:09 · sessao afd447896a19 · - · - · origem=Ronald (tmux)

1. **Bloqueia — a janela muda o sinal e a decisão de pular.** O cron real roda sem `--forcar` e passa todas as células passadas. O avulso passa `[cel]`. A segunda passada do mesmo ato pode usar só as células reconciliadas. Cada corpo busca os turnos entre as pontas da sua lista, alimentando encerramento e sem-par com essa mesma carga. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1073`, `app/ponto/services/cartorio.py:1188`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:2143`.

   A janela do pareador chega a `fim+1`. Por leitura estática, com vínculo diurno 08–14, uma E em D às 23:30 e uma S em D+2 às 00:30, sem outras batidas nem papel de intervalo ou eco, dão `E` em `[D,D]`. Uma janela ampla do mesmo vínculo inclui a S e admite fechar em 25h pelo envelope de 26h, pois ambas as pontas estão a mais de três horas dos respectivos marcos. Depois da expiração, o avulso pode imprimir `sem_par:E` e o cron imprimir sem esse componente. A S em D+2 também fica de fora das batidas diretamente hasheadas de D. Com os demais insumos iguais, o cron pode pular um dia que o avulso rejulgaria, ou relavrar depois do avulso sem fato novo. Incidência real [nao verificado]. `app/ponto/turnos.py:1834`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1977`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:2005`, `app/ponto/services/cartorio.py:1439`, `app/ponto/services/cartorio.py:1480`, `app/ponto/services/cartorio.py:1491`, `app/docs/LEIS.md:122` (L-099).

2. **Bloqueia — duas respostas do pareador na mesma lavra.** O sinal/encerramento lê `turnos_do_colab(c,d0,d1)` com a ata persistida. O realizado da grade compara o mesmo dia usando `turnos_do_colab` com janela que estende `ref` a `d1+1` e cópia em memória da ata desta montagem. A própria grade explica por que ler papel velho já produziu minuto diferente para as mesmas batidas. É a mesma autoridade com alimentações distintas. `app/ponto/services/cartorio.py:1213`, `app/ponto/services/cartorio.py:1253`, `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:1944`, `app/escala/utils.py:1873`, `app/escala/utils.py:1889`, `app/escala/utils.py:1909`, `app/escala/utils.py:2056`.

3. **Bloqueia — o horizonte permite a nova regra em exportada.** O piso `2026-11` compara a competência derivada do corte da empresa. Uma 11, 12 ou posterior com TXT vigente passa pelo piso. O juiz de entrega K6 existe. A guarda temporária da 09 age no recálculo de `FechamentoMensal`/`DiaPago`, sem proteger o cartório. O teste antigo da 09 distingue ata relavrável de FM/TXT intactos. O selo novo declara que não prova a guarda da exportada. L-092 e L-113 coexistem com o recorte particular da obra, e a regra para frente está em L-124. `app/ponto/services/cartorio.py:316`, `app/ponto/services/cartorio.py:333`, `app/ponto/supra_juiz.py:105`, `app/folha/export.py:556`, `app/ponto/services/fechamento.py:27`, `app/ponto/services/fechamento.py:558`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:113`, `app/ponto/tests/test_a_tela_sem_par_acusa.py:230`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:145`, `app/docs/LEIS.md:155`.

4. **Bloqueia — o protesto novo pode parar folha e entrar na fila de cobrança sem marco cobrável.** O emissor parcial precisa de marco apagado nomeado. A fila fora do diff inclui `FURO_PARCIAL` no universo cobravel e força relavra avulsa, que pode devolver `sem_cobranca_depois`. A pauta existente para os dias sem fio exige `discordante`, enquanto a nova lavra classifica `furo`. A folha retém `espelho_furo` sob as guardas usuais, pois a exceção L-033 exige pelo menos uma pausa apagada. Incidência real [nao verificado]. `app/ponto/services/cartorio.py:1916`, `app/ponto/services/cartorio.py:2032`, `app/ponto/services/furos_sem_cobranca.py:35`, `app/ponto/services/furos_sem_cobranca.py:82`, `app/ponto/services/furos_sem_cobranca.py:117`, `app/ponto/services/pautas_dos_discordantes.py:27`, `app/ponto/services/pautas_dos_discordantes.py:40`, `app/escala/models.py:1623`, `app/folha/export.py:1150`, `app/folha/export.py:1293`, `app/docs/LEIS.md:81`.

5. **Fila — relógio e ordem.** O comando `supra_juiz` conserva a leitura implícita de `now()` dentro de `dias_encerrados`, agora após a carga explicitada pelo diff. Retroativo passa `agora` da borda, mas ambos leem `localdate()` por linha. No cartório, o novo ramo segue o encerramento por fato/instante conforme L-073. A fila de 14:16 punha PAR-PELO-TURNO à frente. O aval posterior de 23:55 autoriza construção paralela e pousos separados. O V6 prevê mudar o teto previsto+4h, que ainda rege esta cópia. Ordem efetiva de pouso [nao verificado]. `app/ponto/management/commands/supra_juiz.py:122`, `app/ponto/management/commands/supra_juiz.py:235`, `app/ponto/management/commands/emitir_furo_retroativo.py:47`, `app/ponto/management/commands/emitir_furo_retroativo.py:113`, `app/ponto/turnos.py:2121`, `app/ponto/turnos.py:2148`, `app/docs/LEIS.md:100`, `app/docs/LEIS.md:150`, `app/docs/CORTES.md:630`, `app/docs/PROMPTS.md:2852`, `app/docs/PROMPTS.md:2863`.

_8045 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
