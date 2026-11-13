# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 00:43** · publicado em **13/11/2026 00:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 00:43 · sessao 7adadbd22ff9 · BUG-CADASTRO-C5E · sonda · origem=Code

Saída fornecida, reproduzida integralmente; nova execução [nao verificado]:

```text
sonda_leitura: sonda_dna_orfao.py [95a58eba670edb78] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

ecs_existentes=1397
geradora_DIFERE_do_dna_passado_gerada                               1  amostra=[19829]
geradora_ok_futuro_gerada                                        8114  amostra=[138708, 138709, 138710, 138711, 138712, 138713, 138714, 138739, 138740, 138741]
geradora_ok_hoje_gerada                                           607  amostra=[138707, 138738, 138769, 138800, 138831, 138862, 138893, 138924, 138955, 138986]
geradora_ok_passado_gerada                                     135480  amostra=[110646, 111453, 138688, 116363, 113623, 113533, 110654, 16987, 122814, 116366]
total                                                          144202  amostra=[]
total_erros=0
```

Conferência sobre o dado fornecido: `1 + 8114 + 607 + 135480 = 144202`; classes somam o total e `total_erros=0`. [nao verificado]

## 13/11 00:41 · sessao e5e906e62ba5 · - · - · origem=Ronald (tmux)

Cuidados para a reescrita:

- Particionar perguntas pela coluna própria `PD.colaborador_id`; o modelo declara essa identidade e seu vínculo com a disputa. As disputas devem ser particionadas por `DS.colaborador_id`, mantendo todas as perguntas prefetched. [app/chamados/models.py:2448; app/chamados/models.py:2454; app/chamados/models.py:1859]
- `Registro` do tipo assume IDs crescentes e eventos contíguos. A v1 tem relatórios ordenados por categoria/dias, fios por criação decrescente e colaboradores repetidos nas janelas. Usar segmentos por chamada/contexto, ou acumuladores comutativos por faixa de IDs; não aplicar silenciosamente a precondição do tipo. [app/chamados/relatorios_disputa.py:52; app/chamados/relatorios_disputa.py:60; app/chamados/services/fio.py:206; e3_diff_leitores_v1.py:1098]
- Limpar `SEQUENCIAS` por chamada raiz exige salvar/restaurar os contadores ao entrar em `rodar` aninhado, para preservar as ordens das observações do pai. [e3_diff_leitores_v1.py:331; e3_diff_leitores_v1.py:501; e3_diff_leitores_v1.py:1200]
- Preservar `carga_da_tarefa`, `tarefa_do_dia` e `dias_trancados` com a janela e os pares completos no agregado. `agora` é obrigatório nas duas funções da tarefa; a adaptação da v1 o injeta. [app/chamados/services/tarefa_do_dia.py:568; app/chamados/services/tarefa_do_dia.py:573; app/chamados/juizes.py:1004; e3_diff_leitores_v1.py:491]
- `sincronizar` compara o alvo com todas as pautas vivas da família: deve receber o apurado inteiro. [app/ponto/services/pautas_dos_discordantes.py:155; app/ponto/services/pautas_dos_discordantes.py:169]
- O resultado de `relatorios_disputa.coletar` permite contar as quatro categorias fixas, incluindo `PRONTA_FECHAR`, sem nomes. O retorno real contém nomes, mas a projeção da v1 os exclui. [app/chamados/relatorios_disputa.py:6; app/chamados/relatorios_disputa.py:52; e3_diff_leitores_v1.py:1001]
- A descrição literal de “PRONTO 7 do aval” não está no dado fornecido nem foi localizada nos arquivos consultados. [nao verificado]

Referências para cobertura:

| Leitor | Local |
|---|---|
| Juiz e faces SQL sem texto | [app/chamados/juizes.py:1515]; [app/chamados/juizes.py:1531]; [app/chamados/juizes.py:1541] |
| Property `.viva` / queryset global `q_vivas` | [app/chamados/models.py:3417]; [app/chamados/models.py:3368] |
| Disputa inteira / respostas sem veredito | [app/chamados/models.py:1827]; [app/chamados/models.py:1854] |
| Explicador inteiro | [app/chamados/services/explicador.py:108]; [app/chamados/services/explicador.py:139] |
| Fio app/admin/explicação | [app/chamados/services/fio.py:190]; [app/chamados/services/fio.py:343] |
| Flags do cartão / resposta enviada | [app/chamados/services/fio.py:354]; [app/chamados/services/fio.py:377] |
| Raiox créditos globais | [app/chamados/services/raiox.py:51]; [app/chamados/services/raiox.py:135] |
| Cobrança direta e via pauta | [app/chamados/services/cobranca.py:221]; [app/chamados/services/cobranca.py:266] |
| Folha por empresa/janela | [app/folha/fora_da_folha.py:113]; [app/folha/fora_da_folha.py:153]; [app/folha/fora_da_folha.py:215] |
| Retrato e contadores | [app/folha/retrato_folha.py:1120]; [app/folha/retrato_folha.py:114] |
| Beco explícito/corrente | [app/escala/services/cadastro_realidade.py:442]; [app/escala/services/cadastro_realidade.py:553] |
| Furo por competência/corrente | [app/chamados/services/furo_sem_canal.py:43] |
| Pautas apurar/dias/sincronizar | [app/ponto/management/commands/pautas_discordantes.py:56]; [app/ponto/services/pautas_dos_discordantes.py:46]; [app/ponto/services/pautas_dos_discordantes.py:155] |

A observação de tarefa interna continua junto de cada chamador: a função real chama a alimentação global sem filtro adicional. [app/chamados/services/tarefa_do_dia.py:517; app/chamados/services/tarefa_do_dia.py:531; app/chamados/juizes.py:1558]

## 13/11 00:40 · sessao 79ed2ad98727 · - · - · origem=Ronald (tmux)

Recomendações para o script:

- Preserve o `perfil` da v1: o teste inicial `retorno is None` deixa `vigia.ramo` sem eventos porque `_listar` só incrementa contadores e acrescenta linhas, sem `return`. Declare esse limite; não mova a guarda. [app/chamados/services/vigia_fio.py:629] [nao verificado]
- O alcance real devolve linhas `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`. Com `limite=sys.maxsize`, confira `alcance_dias_omitidos == 0` e `len(alcance_dias) == alcance_universo`. [app/chamados/services/vigia_fio.py:549; app/chamados/services/vigia_fio.py:632]
- Para o plano da v1, distribua **todas** essas linhas pelo colaborador, incluindo rotas com porta e baldes legais. Não use a segunda posição de `BF._alcance`: ela contém exclusivamente `sem_porta`. [e3_diff_leitores_v1.py:1349; app/chamados/management/commands/backfill_fio_morto_e4b.py:76]
- `BF._alcance(empresas, hoje, agora)` aceita `agora` obrigatório; o `adaptar` da v1 já o injeta. `ato_do_dia(colab, dia, agora, *, hoje)` chama a tarefa real e a porta de reabertura com `executar=False`. [app/chamados/management/commands/backfill_fio_morto_e4b.py:76; app/chamados/management/commands/backfill_fio_morto_e4b.py:82; app/chamados/management/commands/backfill_fio_morto_e4b.py:90]

Emita os contadores nativos, separados do digest, para explicar mudanças:

| Classe | Campos reais |
|---|---|
| Rotas | `alcance_porta_fila`, `alcance_porta_pergunta`, `alcance_de_hoje_pra_frente`, `fio_morto_alcance`. [app/chamados/services/vigia_fio.py:575] |
| Quatro leis | `alcance_` + cada item de `BALDES_FORA_DO_FIO`; a constante contém decisão pendente, ausência pendente, escala não descreve o dia e fio do admin. [app/chamados/services/vigia_fio.py:536; app/chamados/services/vigia_fio.py:578] |
| Causas | `alcance_a` até `alcance_e`; leia `CAUSAS_ALCANCE`. [app/chamados/services/vigia_fio.py:535; app/chamados/services/vigia_fio.py:705] |
| Detalhes de `e` | `alcance_e_detalhe`; a função distingue sem célula, dono do dia na lista, sem pergunta viva, terminal, sem âncora, fora do app e outra. [app/chamados/services/vigia_fio.py:676; app/chamados/services/vigia_fio.py:690; app/chamados/services/vigia_fio.py:707] |
| Papéis | `alcance_por_rota_papel` e `alcance_porta_exige_o_admin`. [app/chamados/services/vigia_fio.py:630; app/chamados/services/vigia_fio.py:653] |

Identidades para emitir como `{esquerda, direita, igual}`:

1. Portas fila + pergunta + hoje em diante + quatro baldes legais + fio morto = universo. [app/chamados/services/vigia_fio.py:531]
2. Causas `a+b+c+d+e` = fio morto. [app/chamados/services/vigia_fio.py:532]
3. Soma dos detalhes de `e` = `alcance_e`. [app/chamados/services/vigia_fio.py:705]
4. Soma de `alcance_por_rota_papel` = universo. [app/chamados/services/vigia_fio.py:554]
5. Quantidade de dias emitidos + omitidos = universo. [app/chamados/services/vigia_fio.py:592; app/chamados/services/vigia_fio.py:632]
6. No censo antigo: fio vivo + quatro leis + fio morto + hoje em diante + fora do vocabulário = lâmpadas acesas. [app/chamados/services/vigia_fio.py:104]
7. Sem pergunta nenhuma + pergunta morreu = fio morto. [app/chamados/services/vigia_fio.py:449]
8. Com alvo + sem alvo + sem pergunta na porta = fio morto. [app/chamados/services/vigia_fio.py:467]
9. Soma de `porta_<classe>` para `CLASSES_DA_PORTA` = `porta_universo_nos_dias_mortos`. [app/chamados/services/vigia_fio.py:369; app/chamados/services/vigia_fio.py:463]

Papéis são rótulos das fontes: fio da tarefa; permissão do Resolver dia; descrição do status da ausência; ou dono da lista do admin. Para impedir texto livre inesperado, use vocabulário fechado extraído dessas constantes/choices e digest de qualquer rótulo desconhecido. [app/chamados/services/vigia_fio.py:171; app/chamados/services/vigia_fio.py:304; app/chamados/services/vigia_fio.py:238]

A queda real da E3 e o resultado de RSS continuam sem medição nesta tarefa. [nao verificado]

## 13/11 00:39 · sessao 8f78cf174fb0 · BUG-CADASTRO-C5E · verificador · origem=Code

**REPROVA.** Há um contraexemplo estrutural na reversão por snapshot: a exclusão do vínculo pode zerar a geradora da célula futura sem retirar do DNA o vínculo apagado. O vigia apenas conta essa divergência. (`app/colaboradores/services/vinculo.py:1029,1034`; `app/escala/models.py:1693,1697`; `app/ponto/services/vigia_cadastro_futuro.py:60`)

**Limite desta verificação:** a cópia fornecida não contém `.git`; o comando de diff solicitado falhou. Portanto, identidade do HEAD, pertencimento de cada função ao diff inteiro e exaustividade de A/B/G ficam **[não verificado]**. Abaixo separo o delta fornecido dos achados verificáveis no código.

**A. O delta cura o achado 4/6 específico: sim.** `_ec_vigencia_regenera_celulas` lê `localdate()` uma vez, usa esse valor em `_ate` e entrega o mesmo dia aos caminhos de passado e futuro. O arquivo entrou no universo com teto 1 para cada signal. (`app/escala/signals.py:67,68,74,82`; `app/ponto/tests/test_cadastro_futuro_c5e.py:332,352,353`)

O código ainda contém estas funções com múltiplos sítios de leitura dentro do universo da catraca; pertencimento de **cada função** ao diff inteiro: **[não verificado]**.

| Função | Contagem e natureza |
|---|---|
| `julgar_desligamento` | **2** `date.today()`. Ambas executam quando a data é omitida: a demissão pode receber D e a classificação usar D+1. (`app/colaboradores/services/desligamento.py:20,40,58`) |
| `reabrir_vigencia_impossivel` | **2**, com autor: `date.today()` para o histórico e `localdate()` para o suplemento. (`app/colaboradores/services/vinculo.py:740,752`) |
| `regenerar_celulas_vinculo` | **2 sítios** de `now()`, ambos carimbos de regeneração; dentro do laço, podem executar uma vez por célula reescrita. Recebe `hoje`. (`app/ponto/portas/celula.py:113,263,308,319`) |
| `julgar_celula` | **3**: dia local, comparação com fim do turno e instante do julgamento. As três podem executar para hoje, turno vencido e `agora` omitido. (`app/ponto/services/cartorio.py:2016,2037,2040`) |

Funções que **recebem `hoje` e contêm leitura**, no universo examinado; novamente, pertencimento individual ao diff: **[não verificado]**.

| Função | Sítios de leitura |
|---|---|
| `executar_desligamento` | **1**, `now()` para carimbo. (`app/colaboradores/services/desligamento.py:62,135`) |
| `regenerar_celulas_vinculo` | **2**, carimbos. (`app/ponto/portas/celula.py:113,263,308`) |
| `reverter_regeneracao_dia` | **1**, carimbo. (`app/ponto/portas/celula.py:779,834`) |
| `aplicar_folgas_em_lote` | **1**, fallback. (`app/escala/folgas.py:385,401`) |
| `reparentar_folga` | **1**, fallback. (`app/escala/folgas.py:471,512`) |
| `recusa_ancora_futura` | **1**, fallback. (`app/escala/models.py:938,965`) |
| `inicio_do_efeito` | **1**, fallback. (`app/escala/services/escala_auto_executor.py:24,29`) |
| `aplicar_no_vinculo` | **1**, fallback. (`app/escala/services/escala_auto_executor.py:87,98`) |
| `desfazer` | **1**, fallback; a conversão de `historico.criado_em` não é nova leitura. (`app/escala/services/escala_auto_executor.py:118,127,130`) |
| `apurar_colab`, `apurar`, `ranking`, `ranking_colabs`, `universo_empresa` | **1 cada**. (`app/escala/services/furos_diarios.py:25,31,60,67,123,130,167,175,247,258`) |

Nos fallbacks, um `hoje` válido evita a leitura: **sítio AST não significa leitura executada em toda chamada**. Os carimbos continuam lendo o instante mesmo quando o dia foi injetado. (`app/escala/folgas.py:401`; `app/ponto/portas/celula.py:308`)

Fora do universo, também há `vinculos_5x2_6x1_trabalha_feriado(hoje=None)`, com **1** `date.today()`; o recorte de `escala/models.py` cobre somente `recusa_ancora_futura`. (`app/escala/models.py:920,926`; `app/ponto/tests/test_cadastro_futuro_c5e.py:331`)

**B. Cobertura integral do diff: [não verificado].** O censo percorre exclusivamente `UNIVERSO_RELOGIO`; não descobre arquivos pelo diff. Acrescentar leitura em arquivo ausente desse universo não altera sua contagem. (`app/ponto/tests/test_cadastro_futuro_c5e.py:578,580,581`)

Estes arquivos ligados aos caminhos examinados leem relógio e estão fora do universo declarado; **quais efetivamente pertencem ao diff inteiro: [não verificado]**. (`app/ponto/tests/test_cadastro_futuro_c5e.py:326`)

| Arquivo fora do universo | Leitura verificável |
|---|---|
| `app/core/signals.py` | `localdate()` nos dois signals de feriado. (`app/core/signals.py:36,43`) |
| `app/escala/admin.py` | `save_model`: `localdate()`. (`app/escala/admin.py:15`) |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | `_rodar` e `_aplicar`: uma leitura cada. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:67,227`) |
| `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py` | `localdate()`. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`) |
| `app/escala/management/commands/corrigir_vinculo_vigencia.py` | `localdate()`. (`app/escala/management/commands/corrigir_vinculo_vigencia.py:48`) |
| `app/colaboradores/views.py` | Exemplo: desligamento passa `localdate()` à porta. (`app/colaboradores/views.py:1437`) |
| `app/chamados/management/commands/materializar_passivo_validado.py` | `localtime()` sem valor e `now()` no ensaio. (`app/chamados/management/commands/materializar_passivo_validado.py:87,103`) |
| `app/ponto/management/commands/aplicar_col900.py` | `localdate()` e `localtime()` sem valor. (`app/ponto/management/commands/aplicar_col900.py:146,253`) |
| `app/ponto/management/commands/diff_col900.py` | `localdate()` e dois `localtime()` sem valor. (`app/ponto/management/commands/diff_col900.py:57,119,128`) |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `localdate()`. (`app/ponto/management/commands/curar_residuo_bug115.py:74`) |
| `app/api/views_mensageria.py` | Exemplo: dia local e instante. (`app/api/views_mensageria.py:1026,1041`) |

`localtime()` sem valor está separado das quatro formas citadas em A porque também lê o relógio e é contado pela própria catraca. (`app/ponto/tests/test_cadastro_futuro_c5e.py:321,376,562`)

**C. Grafo e efeito dos sítios do achado 1.** Uso D para o dia capturado pelo executor e D+1 para uma leitura posterior; ocorrência desses cruzamentos em produção: **[não verificado]**. O executor captura D e o entrega aos dois fechamentos, à regeneração e ao suplemento. (`app/colaboradores/services/vinculo.py:194,219,247,330,339`)

1. **`_copiar_folga_padrao`: chamada direta do executor**, tanto na atualização quanto na criação. Sua leitura escolhe os meses da foto: dentro do mesmo mês, D→D+1 não muda o par; na virada mensal, passa a fotografar o novo mês e o seguinte, deixando de fotografar o mês que continha D nesse caminho. Só entra se houver calendário padrão e faltar calendário no vínculo. (`app/colaboradores/services/vinculo.py:157,158,163,164,165,166,284,298`)

2. **`relavrar_celulas_da_foto`: chamada transitiva do executor:** cópia → `gerar_foto_folgas` → `_gerar_foto_folgas_no_ato` → relavra. Se sua leitura for D+1, regenera até D+1 pelo caminho passado/hoje e seleciona futuras somente desde D+2. D+1 pode receber DNA novo sem a limpeza reservada a `d > hoje`. (`app/colaboradores/services/vinculo.py:165`; `app/escala/folgas.py:163,215,280,293,298`; `app/ponto/services/cadastro_futuro.py:27`; `app/ponto/portas/celula.py:312`)

3. **Signal de vigência:** o `save()` da atualização pode acioná-lo quando a vigência muda; criação é recusada pelo receiver. Se ele capturar D+1, passado/hoje vai até D+1 e futuras começam em D+2. O delta elimina a divergência **dentro desse receiver**, mas ele continua capturando um dia independente do executor. (`app/colaboradores/services/vinculo.py:283`; `app/escala/signals.py:34,54,58,67,74,82`; `app/ponto/services/cadastro_futuro.py:27`)

4. **Signal de âncora:** outro receiver do `save()`, condicionado à mudança de âncora. Capturando D+1, usa a competência de D+1, regenera até esse dia e suplementa desde D+2; na virada da competência, também muda o piso da regeneração. (`app/escala/signals.py:86,111,115,119,120,121,126,133`; `app/ponto/services/cadastro_futuro.py:27`)

5. **`_o_chao_do_dia_mudou`: chamada direta pela porta `regenerar_celulas_vinculo`**, sem receber o `hoje` dela. Contraexemplo: a porta recebe D, reescreve D+1 e limpa seu julgamento; o helper lê D+1 e chama o juiz para esse mesmo dia. Se D+1 for folga, o cartório pode julgá-lo, pois a espera pelo fim do turno só se aplica quando `trabalha is not False`. Assim pode voltar julgamento numa célula considerada futura pelo dia injetado no ato. (`app/ponto/portas/celula.py:193,312,314,392,404,464,469,472`; `app/ponto/services/cartorio.py:2016,2025,2033,2040`)

   O suplemento final só limpa novamente se encontrar diferença em trabalho, geradora ou DNA; com DNA já igual, não entra nesse ramo. (`app/ponto/portas/celula.py:297,312,314`)

6. **`reabrir_vigencia_impossivel`: outro ato**, chamado pelo command de correção de vigência. Com autor, pode registrar histórico em D e suplementar com D+1; nesse caso, a célula existente de D+1 fica fora do suplemento, que começa em D+2. O command captura seu próprio dia, mas não o entrega à reabertura. (`app/escala/management/commands/corrigir_vinculo_vigencia.py:48,86,107`; `app/colaboradores/services/vinculo.py:740,752`; `app/ponto/services/cadastro_futuro.py:27`)

Há ainda chamada direta ao pós-vínculo sem passagem de `hoje`; ele relê o dia e usa esse novo teto para julgamento e apuração. Não classifico esses caminhos como “resto do ato”; descrevo a passagem efetiva dos valores. (`app/colaboradores/services/vinculo.py:345,347`; `app/colaboradores/services/pos_vinculo.py:56,57,75,82`)

**D. A mudança de universo amplia candidatos; não provoca nova regravação de passado/hoje por si só.** O predicado novo exclui somente `ativa=False AND data_fim IS NULL`. Logo, entram os inativos com fim declarado; nenhum ativo sai. (`app/escala/alimentacao.py:199,202`; `app/ponto/portas/celula.py:883`)

| Classe | Resultado |
|---|---|
| `ativa=True` | Continua candidata. (`app/ponto/portas/celula.py:883`) |
| `ativa=False AND data_fim IS NULL` | Continua fora. (`app/escala/alimentacao.py:202`) |
| `ativa=False AND data_fim IS NOT NULL` | Entra como candidata; só alcança dia coberto pela vigência. (`app/ponto/portas/celula.py:883,906`; `app/escala/alimentacao.py:214`) |

Para o inativo, exige-se adicionalmente **`d > hoje`, escolha pela juíza e célula ausente**. Passado e hoje são barrados; futura existente é pulada. Portanto essa ampliação não passa a regravar passado/hoje. (`app/ponto/portas/celula.py:908,909,913,930,935`)

**E. Achados 6 e 7.**

- **6 procede quanto ao comportamento:** o cron chama `_regenerar_hoje_existentes`, seleciona geradas de hoje e manda regenerar `[hoje, hoje]`. Havendo diferença, a porta reescreve trabalho, geradora e DNA, respeitando suas guardas. Isso é independente de D7. Se o aval proíbe especificamente essa escrita de hoje: **[não verificado]** no literal disponibilizado. (`app/ponto/management/commands/gerar_celulas.py:316,323,324,347`; `app/ponto/portas/celula.py:284,297,306,310`)

- **7 procede como possibilidade estrutural:** o snapshot apaga vínculos ausentes do retrato; `SET_NULL` altera a FK, enquanto o DNA é um JSON independente. Depois, somente os vínculos do snapshot recebem regeneração. (`app/colaboradores/services/vinculo.py:1022,1029,1034,1035`; `app/escala/models.py:1693,1697`)

  Contraexemplo sem depender de snapshot vazio: o vínculo removido gerou uma célula futura, e todos os restaurados terminam antes dela. Após `SET_NULL`, o ramo que limparia a futura fora de vigência exige `escala_geradora_id == ec.id`, condição que já não vale. O DNA pode continuar nomeando o vínculo apagado; o vigia incrementa o contador e o gerador pula a futura existente. Ocorrência em produção: **[não verificado]**. (`app/ponto/portas/celula.py:251,252,930,935`; `app/ponto/services/vigia_cadastro_futuro.py:55,60,61`)

**F. Escritores e leitores no delta literal.**

A única função de produção alterada é `_ec_vigencia_regenera_celulas`: **escreve `_hoje`** com `localdate()`; **leem `_hoje`** o cálculo de `_ate`, os limites da regeneração e os dois argumentos `hoje=`. A função não recebe `hoje` por parâmetro. (`app/escala/signals.py:35,67,68,72,74,82`)

Os destinos usam o valor como **dia civil local que separa passado/hoje de futuro**, não como dia de início do turno: a porta atribui `_hoje = hoje`, e o suplemento começa em `hoje + 1`. (`app/ponto/portas/celula.py:193,312`; `app/ponto/services/cadastro_futuro.py:26,27`)

O outro arquivo do delta altera constantes da catraca, sem adicionar função que produza ou consuma `hoje` em produção. (`app/ponto/tests/test_cadastro_futuro_c5e.py:332,352,353,375`)

Essa equivalência não pode ser estendida automaticamente a toda a obra: existem leitores de `date.today()` e de `timezone.localdate()`; a configuração Django declara São Paulo, mas o fuso efetivo dos processos em produção é **[não verificado]**. (`app/colaboradores/services/vinculo.py:163,194`; `app/config/settings/base.py:93,95`)

**G. Classes adversariais.**

| Classe | Resultado sustentado |
|---|---|
| Relógio fora da borda | **Presente no grafo**, inclusive nos caminhos descritos em C; há permissões explícitas na catraca para esses interiores. Isso não decide sua inclusão no trecho “resto do ato”. (`app/ponto/tests/test_cadastro_futuro_c5e.py:316,340,344,369`; `app/ponto/portas/celula.py:464`) |
| Segundo escritor fora da porta | **Efeito de escrita na célula pelo apagamento da geradora:** o collector aplica `SET_NULL`, sem passar pela regeneração do DNA. É o mecanismo do achado 7. (`app/colaboradores/services/vinculo.py:1029`; `app/escala/models.py:1693`) |
| Selo/RED insuficiente para o enunciado amplo | **Presente:** o teste de meia-noite substitui cópia de folga por lambda, helper de consequências por `Mock` e pós-vínculo por `Mock`. O assert de uma leitura certifica esse caminho reduzido. (`app/ponto/tests/test_cadastro_futuro_c5e.py:38,93`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:370,384,391`) |
| Segundo juiz / derivação paralela | Os caminhos centrais examinados consultam `vinculo_do_dia` e usam `montar_dna`; ausência dessas classes no diff inteiro: **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:38,39`; `app/ponto/portas/celula.py:290,909`) |
| Ordem de trava | O snapshot adquire vínculos em ordem de PK antes das células; inversão no diff inteiro: **[não verificado]**. (`app/colaboradores/services/vinculo.py:1020,1025`) |
| Pouso antes do pré-requisito | **[não verificado]**: sem histórico Git ou evidência de execução disponibilizada. | [nao verificado]
| Campo com dois significados | No delta, `_hoje` conserva o significado de dia civil; ocorrência nova dessa classe no diff inteiro: **[não verificado]**. (`app/escala/signals.py:67,74,82`) |

**Bloqueante comprovado:** reversão por snapshot capaz de deixar DNA futuro nomeando vínculo apagado, sem reparação pelo caminho subsequente. (`app/colaboradores/services/vinculo.py:1029,1034`; `app/ponto/portas/celula.py:252`; `app/ponto/services/vigia_cadastro_futuro.py:60`)

**Impedimentos adicionais para certificar a obra inteira:** censo exaustivo do diff **[não verificado]**; RED atual não cobre os caminhos de releitura de produção identificados em C. Não transformo automaticamente todos os interiores permitidos pela catraca em novos bloqueantes de escopo. (`app/ponto/tests/test_cadastro_futuro_c5e.py:316`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:370,384,391`)

## 13/11 00:38 · sessao 631294d63c6e · - · - · origem=Ronald (tmux)

O delta cura especificamente a dupla leitura de `_ec_vigencia_regenera_celulas`: `_hoje` recebe uma única `timezone.localdate()` em `app/escala/signals.py:67`; `_ate` reutiliza esse valor em `:68`; passado e futuro recebem o mesmo dia em `:74` e `:82`. O signal entrou no universo (`app/ponto/tests/test_cadastro_futuro_c5e.py:332`) com teto 1 (`:353`).

Executei somente o extrato AST, em memória, de `leituras_de_relogio` e `censo_relogio`: **44 sítios, 39 funções; contagem igual a `TETO_RELOGIO`**. A implementação do censo está em `app/ponto/tests/test_cadastro_futuro_c5e.py:575`, e as comparações em `:592`. Não executei suite nem banco. [não verificado: execução de testes]

Funções do universo com mais de um sítio de relógio:

| Função | Contagem | Referências e natureza |
|---|---:|---|
| `julgar_desligamento` | 2 | `app/colaboradores/services/desligamento.py:20`, `:40`: `date.today()`; ambas executam se colaborador não desligado e data omitida. A data escolhida pode ser D e a classificação `agendado` usar D+1 (`:58`). |
| `reabrir_vigencia_impossivel` | 2 | `app/colaboradores/services/vinculo.py:740` (`date.today`, histórico; requer autor), `:752` (`localdate`, suplemento futuro). |
| `regenerar_celulas_vinculo` | 2 | `app/ponto/portas/celula.py:263`, `:308`: carimbos `regenerada_em`, não escolha de dia. Recebe `hoje` em `:113`. |
| `julgar_celula` | 3 | `app/ponto/services/cartorio.py:2016`, `:2037`, `:2040`: dia local, teste do fim do turno e instante do julgamento. As três podem executar no mesmo ato para hoje, turno já vencido e `agora` omitido. |

Pertença dessas funções ao diff inteiro: **[não verificado]**, pois a cópia não contém `.git`.

Funções que recebem `hoje` e também contêm leitura:

| Função | Sítios | Arquivo:linha |
|---|---:|---|
| `aplicar_folgas_em_lote` | 1 | `app/escala/folgas.py:385`, `:401` |
| `reparentar_folga` | 1 | `app/escala/folgas.py:512` |
| `recusa_ancora_futura` | 1 | `app/escala/models.py:938`, `:965` |
| `inicio_do_efeito` | 1 | `app/escala/services/escala_auto_executor.py:24`, `:29` |
| `aplicar_no_vinculo` | 1 | `app/escala/services/escala_auto_executor.py:98` |
| `desfazer` | 1 | `app/escala/services/escala_auto_executor.py:127` |
| `apurar_colab` | 1 | `app/escala/services/furos_diarios.py:25`, `:31` |
| `apurar` | 1 | `app/escala/services/furos_diarios.py:60`, `:67` |
| `ranking` | 1 | `app/escala/services/furos_diarios.py:123`, `:130` |
| `ranking_colabs` | 1 | `app/escala/services/furos_diarios.py:167`, `:175` |
| `universo_empresa` | 1 | `app/escala/services/furos_diarios.py:247`, `:258` |
| `executar_desligamento` | 1 | `app/colaboradores/services/desligamento.py:62`, `:135` (`now`, carimbo) |
| `regenerar_celulas_vinculo` | 2 | `app/ponto/portas/celula.py:113`, `:263`, `:308` |
| `reverter_regeneracao_dia` | 1 | `app/ponto/portas/celula.py:779`, `:834` (`now`, carimbo) |

Nos fallbacks `hoje or relógio`/`if hoje is None`, fornecer um `hoje` válido evita a leitura; é preciso distinguir **sítio AST** de **leitura realmente executada**. Exemplos: `app/escala/folgas.py:401`, `app/escala/services/furos_diarios.py:30`.

Há ainda uma função do mesmo arquivo de modelos **não coberta** pelo recorte `escala/models.py::recusa_ancora_futura`: `vinculos_5x2_6x1_trabalha_feriado(hoje=None)`, com uma `date.today()` em `app/escala/models.py:920`, `:926`; o universo restringe esse arquivo a uma única função em `app/ponto/tests/test_cadastro_futuro_c5e.py:331`.

Arquivos candidatos da obra que leem relógio e estão fora do universo; pertença exata ao diff **[não verificado]**:

| Arquivo | Leituras verificadas |
|---|---|
| `app/core/signals.py` | `:36`, `:43`: um `localdate` por signal. O teste carrega o módulo em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:508`. |
| `app/escala/admin.py` | `:15`: um `localdate` em `save_model`; método real extraído em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:493`. |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py` | `:67` em `_rodar`, `:227` em `_aplicar`; teste carrega `_aplicar` em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:623`. |
| `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py` | `:37`: um `localdate`. |
| `app/escala/management/commands/corrigir_vinculo_vigencia.py` | `:48`: um `localdate`. |
| `app/chamados/management/commands/materializar_passivo_validado.py` | `:87` (`localtime` sem argumento) e `:103` (`now` no ensaio): **2 normalizadas**, **1 na lista literal do usuário**. |
| `app/ponto/management/commands/aplicar_col900.py` | `:146` (`localdate`), `:253` (`localtime` sem argumento): **2 normalizadas**, **1 literal**; auxiliares também leem `now` em `:70`, `:102`. |
| `app/ponto/management/commands/diff_col900.py` | `:57` (`localdate`), `:119`, `:128` (`localtime` sem argumento): **3 normalizadas**, **1 literal**. |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `:74`: um `localdate`. |
| `app/api/views_mensageria.py` | Exemplo relevante: `api_mensageria_pessoa`, **2**, `:1026` (`localdate`) e `:1041` (`now`). Fonte lida pelo teste em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:895`. |
| `app/colaboradores/models.py` | `Colaborador.salario_em`: uma `today` em `:471`; `LinhaTransporte.custo_mensal`: uma `now` em `:824`. |

`app/chamados/signals.py` não apresentou chamada de relógio no censo AST. [não verificado: pertinência ao diff inteiro]

F, **delta literal**: a única função de produção alterada é `_ec_vigencia_regenera_celulas`; escritor de `_hoje` em `app/escala/signals.py:67`, leitores em `:68`, `:72`, `:74`, `:82`. Trata-se de dia civil local que separa passado/hoje de futuro. O outro arquivo do delta altera dados da catraca, não funções que escrevam/leiam o `hoje` de produção (`app/ponto/tests/test_cadastro_futuro_c5e.py:332`, `:352`, `:353`, `:375`).

Limite adversarial do RED: `MeiaNoiteNoAtoTest` reutiliza a fixture em `app/ponto/tests/test_cadastro_futuro_c5e.py:38`; essa fixture troca `_copiar_folga_padrao` por lambda (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:391`), `_o_chao_do_dia_mudou` por `Mock` (`:384`) e `reconciliar_apos_vinculo` por `Mock` (`:370`). Portanto o assert de uma leitura em `app/ponto/tests/test_cadastro_futuro_c5e.py:93` verifica esse caminho reduzido; não prova uma leitura no grafo inteiro de produção. A cobertura do grafo completo é **[não verificado]**.

## 13/11 00:36 · sessao eac820637f03 · - · - · origem=Ronald (tmux)

Para C, tomando D como dia capturado em `executar_vinculo` e D+1 como leitura posterior à meia-noite; ocorrência real em produção [não verificado]. O executor captura D em `app/colaboradores/services/vinculo.py:194`, passa D aos dois fechamentos (`:219`, `:247`), à regeneração até D (`:328`) e ao suplemento futuro (`:339`).

| Sítio | Grafo observado | Efeito concreto se sua leitura devolver D+1 |
|---|---|---|
| `_copiar_folga_padrao` | **Chamada direta** por `executar_vinculo`, tanto na atualização quanto na criação: `app/colaboradores/services/vinculo.py:284`, `:298`. Somente entra com calendário padrão e sem calendário já escolhido: `:157`–`:159`. | `date.today()` escolhe o **mês da foto**, não o dia-turno: `:163`–`:166`. Dentro do mesmo mês, a mudança D→D+1 não muda os dois meses escolhidos; na virada de mês troca o par mês corrente/próximo pelo novo par, deixando de fotografar o mês que continha D nesse caminho (`:164`–`:166`). `date.today()` usa data do processo; equivalência com data civil local do Django [não verificado]. |
| `relavrar_celulas_da_foto` | **Chamada transitiva** do executor: `_copiar_folga_padrao` → `gerar_foto_folgas` (`vinculo.py:165`–`:166`) → `_gerar_foto_folgas_no_ato` (`app/escala/folgas.py:162`–`:163`) → `relavrar_celulas_da_foto` (`:215`–`:218`). | Lê seu próprio `hoje=D+1` (`folgas.py:280`), regenera `desde..D+1` como passado/hoje (`:290`–`:296`) e só seleciona futuras a partir de D+2 (`:298`–`:299`; `app/ponto/services/cadastro_futuro.py:27`). D+1 recebe DNA novo pelo caminho passado/hoje, sem a limpeza reservada a `d > hoje` (`app/ponto/portas/celula.py:289`–`:315`). Pode chegar ao julgamento de D+1 no rabo (`:392`, `:464`–`:472`); julgamento efetivo obedece às guardas descritas abaixo. |
| `_ec_vigencia_regenera_celulas` | **Signal de `post_save`** registrado em `app/escala/signals.py:34`; no executor a atualização existente faz `save()` (`app/colaboradores/services/vinculo.py:283`). Só roda se vigência mudou e não é criação/raw (`signals.py:54`–`:59`). | Uma leitura própria D+1 (`:67`) governa **ambas** as partes: passado/hoje até D+1 (`:72`–`:75`) e futuras desde D+2 (`:81`–`:83`; `cadastro_futuro.py:27`). O buraco interno antigo entre duas leituras foi eliminado; continua tendo um dia próprio distinto do D do executor (`vinculo.py:194`). DNA de D+1 é tratado no ramo passado/hoje; seu julgamento segue as guardas do cartório (`celula.py:312`; `cartorio.py:2025`, `:2033`). |
| `_ec_ancora_regenera_celulas` | **Outro signal de `post_save`**, `app/escala/signals.py:86`; `save()` do executor em `vinculo.py:283` pode acioná-lo quando muda a âncora. Recusa criação/raw, âncora nula ou inalterada (`signals.py:111`–`:116`). | Lê D+1 (`:119`), escolhe janela da competência por D+1 (`:120`), regenera até D+1 (`:124`–`:127`) e suplementa desde D+2 (`:132`–`:134`; `cadastro_futuro.py:27`). Na virada da competência, também muda o início da janela histórica (`signals.py:120`–`:121`). |
| `_o_chao_do_dia_mudou` | **Chamada direta pela porta do aval** `regenerar_celulas_vinculo` (`app/ponto/portas/celula.py:392`); recebe dias tocados, **não recebe `hoje`** (`:404`–`:405`). Também chamado por edição humana (`:665`) e reversão (`:858`). | A porta usa D (`:193`), reescreve DNA futuro D+1 e desfaz julgamento (`:306`–`:315`). Depois o rabo lê D+1 (`:464`), deixa D+1 passar no filtro (`:469`) e chama o juiz (`:472`). O juiz relê o relógio (`app/ponto/services/cartorio.py:2016`), recusa futuro (`:2025`) e, para hoje com trabalho, espera fim do turno (`:2033`–`:2038`). **Contraexemplo concreto: D+1 com `trabalha=False`** não passa pela espera de turno e pode ser julgado novamente (`:2033`, `:2040`), embora seja futuro segundo o D injetado na porta. |
| `reabrir_vigencia_impossivel` | **Outro ato**: chamador de produção encontrado é `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`; recebe `ec`, `data_fim`, autor/motivo, sem `hoje` (`app/colaboradores/services/vinculo.py:674`). Não há chamada a essa função no corpo de `executar_vinculo` (`:170`–`:387`). | Com autor, grava `HistoricoVinculo.data=D` por `date.today()` (`vinculo.py:740`; escritor do campo em `app/colaboradores/services/historico_vinculo.py:15`), depois passa D+1 ao suplemento (`vinculo.py:752`), que só seleciona desde D+2 (`cadastro_futuro.py:27`). Uma célula **existente** de D+1 com DNA antigo fica fora desse suplemento e não há regeneração do passado/hoje nessa função (`vinculo.py:729`–`:753`). O `update()` não emite signal (`:730`, `:746`). O command já havia capturado outro `hoje` na borda (`corrigir_vinculo_vigencia.py:48`), usado no fechamento seguinte (`:107`–`:108`) mas não entregue à reabertura (`:86`–`:88`). |

Atenção à interação: quando um caminho intermediário julga D+1, o suplemento final do executor, usando D, só limpa esse julgamento **se reescrever DNA**; o ramo exige diferença em `trabalha`, geradora ou DNA antes de chamar a limpeza (`app/ponto/portas/celula.py:297`–`:315`). Portanto um julgamento recém-criado pode permanecer se o suplemento encontrar o mesmo DNA (`:297`). Dado de produção demonstrando esse caso [não verificado].

Leituras adicionais no executor: `reconciliar_apos_vinculo` é chamado diretamente sem `hoje` (`app/colaboradores/services/vinculo.py:345`–`:348`), lê D+1 (`app/colaboradores/services/pos_vinculo.py:56`), inclui D+1 no intervalo (`:57`) e tenta julgá-lo (`:64`–`:75`), além de apurar furos até esse teto (`:82`). O próprio comentário do executor declara essa releitura como sítio interior fora da cura (`vinculo.py:189`–`:192`); isso descreve o código, não resolve a interpretação do aval.

## 13/11 00:35 · sessao 0575d92ce211 · - · - · origem=Ronald (tmux)

D. A troca amplia os **candidatos**, sem retirar nenhum ativo: antes `ativa=True`; agora ficam fora apenas `ativa=False AND data_fim IS NULL`. Entram os inativos com `data_fim` preenchido, inclusive passado, hoje, futuro e vigências impossíveis como candidatos; a cobertura efetiva ainda exige `data_inicio <= d <= data_fim`. (`app/escala/alimentacao.py:199-216`; `app/ponto/portas/celula.py:883-910`)

O candidato inativo só pode criar célula quando `d > hoje`, cobre `d`, a juíza `vinculo_do_dia` o escolhe e a célula está ausente. Nenhum passado ou hoje passa a ser escrito **por essa ampliação**, pois todos os inativos são barrados em `d <= hoje`; futura existente também é pulada. Ativos mantêm o comportamento: criam ausentes e podem enriquecer DNA nulo no passado. (`app/ponto/portas/celula.py:906-935`)

A seleção da juíza sem célula prefere `(ativa, data_inicio)`: o ativo vence o inativo sobreposto. (`app/escala/alimentacao.py:285-286`) Os testes existentes declaram casos para futura ausente inativa, passado/hoje preservados, limite do fim, sobreposição com ativo, futura existente e inativo sem fim; execução desses testes nesta verificação: [não verificado]. (`app/ponto/tests/test_cadastro_futuro_c5b.py:272-305`)

E/6. **Procede no código**: o cron chama `_regenerar_hoje_existentes` antes do gerador, seleciona células geradas de `data=hoje`, consulta a juíza e chama `regenerar_celulas_vinculo(ec, hoje, hoje, hoje=hoje, recalculo=False, estender_horizonte=False)`. Se DNA, geradora ou trabalho diferem, a porta reescreve a célula. Respeita mão humana e competência lavrada. Isso existe independentemente da ampliação D7. (`app/ponto/management/commands/gerar_celulas.py:303-325`, `:347-350`; `app/ponto/portas/celula.py:284-311`) O cron está declarado às 05:50 com papel `gerador`. (`app/config/crons.py:426`, `:2301`) Ocorrência concreta em produção: [não verificado].

E/7. **Procede como possibilidade estrutural**: snapshot remove ECs ausentes do retrato e depois regenera apenas os sobreviventes do snapshot; remover a EC zera a FK `escala_geradora` por `SET_NULL`, enquanto `dna` permanece um JSON independente. (`app/colaboradores/services/vinculo.py:1022-1035`; `app/escala/models.py:1692-1700`)

Contraexemplo: célula futura gerada pela EC removida; snapshot vazio, ou todos os restaurados terminam antes dessa data. Com snapshot vazio, nenhuma regeneração é chamada. Com restaurados que não cobrem a data, o ramo de desligar futura exige `cel.escala_geradora_id == ec.id`, condição falsa após `SET_NULL`; portanto o DNA pode continuar nomeando o PK removido. (`app/colaboradores/services/vinculo.py:1005`, `:1029-1035`; `app/ponto/portas/celula.py:229-254`)

O vigia compara `dna.vinculo_id` com o vínculo escolhido pela autoridade e apenas incrementa `CHAVE_DNA_DE_OUTRO`; o gerador pula a futura existente. (`app/ponto/services/vigia_cadastro_futuro.py:55-61`; `app/ponto/portas/celula.py:930-935`) Ocorrência em produção: [não verificado].

## 13/11 00:15 · sessao d165bc130172 · E3-FORMA-SQL · leitura · origem=Code

**Há uma incompatibilidade concreta entre (b) e a preservação dos leitores reais em (g). Não consigo entregar um script integral afirmando que cumpre ambos:** leitores por colaborador executam consultas globais internamente, inclusive consultas cujo recorte mudaria o resultado. [app/folha/fora_da_folha.py:227; app/chamados/juizes.py:1558; app/chamados/juizes.py:1581]

As referências a `e3_diff_leitores_v1.py` abaixo numeram o anexo fornecido, contando seu primeiro comentário como linha 1. A condição do perfil está em `:369`, corrigindo a referência intermediária a `:375`. [e3_diff_leitores_v1.py:1; e3_diff_leitores_v1.py:369]

**1. Onde a memória cresce na v1**

| Ponto | Retenção ou materialização |
|---|---|
| `REGISTROS[nome]["linhas"]` | Cada `guardar` acrescenta unidade e valor canonizado; nenhuma linha é descartada durante a sonda. Cresce com todos os resultados diretos e observados. [e3_diff_leitores_v1.py:319; e3_diff_leitores_v1.py:327] |
| `SEQUENCIAS` | Conserva uma chave por combinação distinta de leitor raiz, contexto, observação e unidade. Repetir a mesma chave aumenta seu contador; novas unidades aumentam o dicionário. Não existe limpeza entre chamadas. [e3_diff_leitores_v1.py:74; e3_diff_leitores_v1.py:336] |
| `celulas` | Materializa todas as células da envoltória das janelas. O fingerprint ainda constrói uma lista de dicionários dos campos e sua representação canônica. [e3_diff_leitores_v1.py:839; e3_diff_leitores_v1.py:851] |
| `UNIVERSO["_celula_para_dia"]` | Acrescenta um mapa completo, por célula, de colaborador e data. Só é removido perto do fim. [e3_diff_leitores_v1.py:843; e3_diff_leitores_v1.py:1380] |
| Empresas e colaboradores | `empresas` materializa empresas; `empresa_de` contém todos os colaboradores; `ids_selecionados` e `colabs` conservam os colaboradores com células na envoltória. [e3_diff_leitores_v1.py:812; e3_diff_leitores_v1.py:813; e3_diff_leitores_v1.py:858; e3_diff_leitores_v1.py:859] |
| Perguntas | `list(perguntas)` avalia o queryset completo, incluindo relações carregadas. `fase` constrói outra lista de referências; o queryset `perguntas` permanece referenciado com seu cache. [e3_diff_leitores_v1.py:906; e3_diff_leitores_v1.py:953; e3_diff_leitores_v1.py:526] |
| Disputas | Materializa todas as disputas selecionadas, com colaboradores, chamados e perguntas prefetched. `fase` acrescenta outra lista de referências. [e3_diff_leitores_v1.py:958; e3_diff_leitores_v1.py:526] |
| Faces e conjuntos globais | `todos_pks` contém todas as perguntas; cada projeção constrói `escolhidos`; `q_sem_texto` conserva outro conjunto. Além disso, registra um booleano por pergunta e por face. [e3_diff_leitores_v1.py:868; e3_diff_leitores_v1.py:872; e3_diff_leitores_v1.py:875; e3_diff_leitores_v1.py:902] |
| Créditos | Mantém `vivo_pks`, conjuntos de créditos e a união ordenada. Dentro do laço, reconstrói `set(vivo_pks)` para **cada** unidade: alocação transitória repetida, além da retenção em `REGISTROS`. [e3_diff_leitores_v1.py:1074; e3_diff_leitores_v1.py:1079; e3_diff_leitores_v1.py:1083] |
| Cada janela | Constrói `cids`, cópias de colaboradores, datas e o produto cartesiano `pares`. A projeção de `dias_trancados` ainda produz uma lista de retornos de `guardar`. [e3_diff_leitores_v1.py:1102; e3_diff_leitores_v1.py:1107; e3_diff_leitores_v1.py:1111; e3_diff_leitores_v1.py:1134] |
| Plano do backfill | Reconstrói `dias_vigia` a partir das linhas já retidas pelo registro; `fase` materializa outra lista de referências. [e3_diff_leitores_v1.py:1349; e3_diff_leitores_v1.py:526] |
| Emissão final | Ordena todas as linhas de cada leitor, constrói o conjunto de unidades distintas e fatia blocos. Nesse momento, as linhas originais continuam retidas. [e3_diff_leitores_v1.py:1438; e3_diff_leitores_v1.py:1443; e3_diff_leitores_v1.py:1448] |

Há também crescimento **durante cada chamada**, mesmo eliminando o armazenamento de linhas da sonda:

| Mecanismo | Crescimento por chamada |
|---|---|
| Instrumentação | `rodar` transforma retornos QuerySet em lista; `linhas` materializa seus itens; `canonico` constrói coleções recursivas; `resumo` mantém a representação canônica e serializações integrais transitórias. [e3_diff_leitores_v1.py:511; e3_diff_leitores_v1.py:554; e3_diff_leitores_v1.py:265; e3_diff_leitores_v1.py:546] |
| Cópias e observações | Perguntas/disputas são copiadas; o perfil constrói conjuntos/mapas auxiliares para furo e beco; comandos DRY acumulam stdout/stderr em `StringIO`. [e3_diff_leitores_v1.py:917; e3_diff_leitores_v1.py:972; e3_diff_leitores_v1.py:402; e3_diff_leitores_v1.py:459; e3_diff_leitores_v1.py:585] |
| Carga da tarefa | Materializa perguntas, decisões, pares e caches por colaborador/dia. Esses dados pertencem à carga da janela. [app/chamados/services/tarefa_do_dia.py:376; app/chamados/services/tarefa_do_dia.py:391; app/chamados/services/tarefa_do_dia.py:423; app/chamados/services/tarefa_do_dia.py:500] |
| Tranca | Materializa pares, vetor de resultados e vetores auxiliares. [app/chamados/juizes.py:1022] |
| Relatórios/créditos/cobrança | Os leitores reais constroem buckets, listas de perguntas e mapas por colaborador. Streaming na saída não elimina essas materializações internas. [app/chamados/relatorios_disputa.py:51; app/chamados/services/raiox.py:135; app/chamados/services/cobranca.py:239] |
| Vigia | Mantém universo de dias, colaboradores, alimentação, perguntas, portas e lista final de alcance. [app/chamados/services/vigia_fio.py:585; app/chamados/services/vigia_fio.py:607; app/chamados/services/vigia_fio.py:617; app/chamados/services/vigia_fio.py:633; app/chamados/services/vigia_fio.py:641] |

Isso identifica estruturas que crescem; **não quantifica sua participação no RSS da E3**. Não há medição de RSS da E3 no dado apresentado. [nao verificado]

**2. Por que não entrego um script apresentado como conforme**

O primeiro impedimento independe do tamanho da frota:

1. A parte precisa executar `o_que_segura` para seu colaborador, conforme (b). O leitor chama `classificar_export(empresa, mes, ano)` e só depois seleciona o colaborador em Python. O export consulta os fechamentos da empresa inteira. Manter essa chamada viola o filtro literal de (b); recortá-la exige alterar a alimentação do leitor. [app/folha/fora_da_folha.py:215; app/folha/fora_da_folha.py:227; app/folha/export.py:785]

2. `tarefa_do_dia` utiliza a carga real, que chama `alimentar_fila`. Esta executa `particionar_verbos(qs_vivos())` e percorre `qs_na_fila()` global. A partição chama novamente `creditos_perguntas`. Portanto, mover apenas a chamada direta de créditos para `"agregado"` não elimina as execuções globais dentro das partes. [app/chamados/services/tarefa_do_dia.py:531; app/chamados/juizes.py:1558; app/chamados/juizes.py:1561; app/chamados/services/verbos.py:34]

3. Acrescentar filtros nessas consultas não é uma transformação neutra: o código documenta que o crédito depende do conjunto vivo recebido e pode mudar quando esse conjunto é recortado. [app/chamados/juizes.py:1581]

Há ainda dois pontos que a reescrita precisa resolver explicitamente:

- **Manifesto entre processos:** a v1 executa `ato_do_dia` para todos os dias registrados pelo alcance, independentemente da rota. Esse universo vem dos dias retidos pela folha nas competências naturais do vigia. Para distribuir exatamente essas unidades sem repetir o vigia, o agregado precisa emitir IDs/datas consumíveis pelas partes; substituí-los apenas por hashes perde essa alimentação. [e3_diff_leitores_v1.py:1307; e3_diff_leitores_v1.py:1349; app/chamados/services/vigia_fio.py:587; app/chamados/services/vigia_fio.py:591]
- **Memória dos globais:** leitores integrais continuam materializando dados internamente. Um watchdog permite interromper uma execução excedente; não demonstra antecipadamente que o agregado terminará abaixo de 2 GB. Esse resultado permanece [nao verificado]. [app/chamados/services/raiox.py:135; app/chamados/services/vigia_fio.py:633]

A interpretação viável seria: **(b) restringe todas as consultas de seleção da sonda, com exceções declaradas para consultas globais internas; (c) restringe as chamadas diretas da sonda; o agregado emite um manifesto de IDs/datas.** Isso é uma alteração do contrato solicitado, não uma propriedade que eu possa assumir silenciosamente. [app/chamados/juizes.py:1558; app/folha/fora_da_folha.py:227; e3_diff_leitores_v1.py:1349]

**3. Diferenças de cobertura que precisam ser declaradas**

Não há uma versão nova entregue ou executada para comparar. Estas são as diferenças e perdas que uma implementação precisa evitar ou assumir expressamente. [nao verificado]

| Ponto | Consequência para a cobertura |
|---|---|
| Perguntas versus colaboradores selecionados | A v1 mede perguntas da frota inteira, incluindo perguntas sem célula e fora das janelas. Restringir perguntas aos colaboradores selecionados pelas células perderia cobertura. A divisão deve usar o colaborador da disputa sem acrescentar essa restrição. [e3_diff_leitores_v1.py:904; e3_diff_leitores_v1.py:906; e3_diff_leitores_v1.py:958] |
| Disputas inteiras | Dividir perguntas de uma mesma disputa mudaria a alimentação de `pode_fechar`, `respostas_sem_veredito` e explicador. A v1 entrega a disputa com todas as suas perguntas. [e3_diff_leitores_v1.py:958; e3_diff_leitores_v1.py:975] |
| Leitores de empresa/janela | `dias_que_seguram`, `fora_da_folha`, retrato/contadores/DRY, competência trancada e beco explícito também precisam ficar no agregado; não são consultas por colaborador só porque estavam dentro de `uma_janela`. [e3_diff_leitores_v1.py:1151; e3_diff_leitores_v1.py:1175; e3_diff_leitores_v1.py:1193; e3_diff_leitores_v1.py:1211; e3_diff_leitores_v1.py:1216; e3_diff_leitores_v1.py:1224] |
| Pautas | Particionar a entrada de `sincronizar` mudaria o cálculo de fechamento: ele compara a entrada com todas as pautas vivas da família. Precisa receber o resultado integral no agregado. [app/ponto/services/pautas_dos_discordantes.py:169] |
| Paridade da face SQL | A v1 confronta o juiz por pergunta com o conjunto global `q_sem_texto`. Manter esse leitor nas partes exige transportar a pertença calculada no agregado; recalcular a face nas partes muda sua alocação de execução. [e3_diff_leitores_v1.py:902; e3_diff_leitores_v1.py:933] |
| Plano do backfill | Reconstruir seus dias a partir das células das janelas não preserva o universo da v1. O manifesto precisa vir do alcance real e incluir todas as rotas. [e3_diff_leitores_v1.py:1349; app/chamados/services/vigia_fio.py:591] |
| Chamadas internas repetidas | A v1 chama `_calados` diretamente e através de `linhas_para_a_pauta`; também chama o alcance diretamente e através de `BF._alcance`. Suprimir essas chamadas internas modifica o que era observado. [e3_diff_leitores_v1.py:1092; app/chamados/services/cobranca.py:266; e3_diff_leitores_v1.py:1338; app/chamados/management/commands/backfill_fio_morto_e4b.py:78] |
| `vigia.ramo` | O gancho da v1 descarta retornos `None`, mas `_listar` não retorna valor. Corrigi-lo acrescenta observação efetiva. As projeções `vigia.censo_alcance.dia` já existem independentemente desse gancho. [e3_diff_leitores_v1.py:369; app/chamados/services/vigia_fio.py:629; e3_diff_leitores_v1.py:1307] |
| Fingerprints | A v1 calcula SHA256 de listas ordenadas e publica suas linhas. Um fingerprint novo por faixas precisa ser comparado entre base/cópia usando o mesmo texto novo; equivalência com os digests antigos depende do algoritmo escolhido. [e3_diff_leitores_v1.py:1438; e3_diff_leitores_v1.py:1452] |

Para explicar a queda por classe, precisam permanecer contagens das rotas, dos quatro baldes de lei, das causas `a`–`e`, dos detalhes de `e` e dos papéis, conservando as identidades de soma do vigia. A classificação deve vir do resultado real, sem implementar outro juiz. A queda efetiva e sua distribuição por classe não estão demonstradas no dado fornecido. [app/chamados/services/vigia_fio.py:531; app/chamados/services/vigia_fio.py:535; app/chamados/services/vigia_fio.py:648; app/chamados/services/vigia_fio.py:676; app/chamados/services/vigia_fio.py:690; nao verificado]

## 13/11 00:07 · sessao 38e18dd8d0ce · - · - · origem=Ronald (tmux)

O ponto decisivo é incompatibilidade literal entre (b), (c) e preservação dos leitores em (g): uma chamada por colaborador/dia a `tarefa_do_dia` abre alimentação global do painel. `CargaDaTarefa.tarefa` chama `alimentar_fila` em `app/chamados/services/tarefa_do_dia.py:532`; essa alimentação chama `particionar_verbos(qs_vivos())` em `app/chamados/juizes.py:1558` e percorre `qs_na_fila()` integral em `app/chamados/juizes.py:1561`; a partição chama `creditos_perguntas(vivo_qs)` em `app/chamados/services/verbos.py:34`.

Portanto, mover a chamada direta `RX.creditos_perguntas` para `"agregado"` não impede novas execuções globais dentro das partes. Acrescentar filtro a esses querysets viola o universo que a função documenta: crédito depende do conjunto vivo integral, e filtrar muda o crédito; `app/chamados/juizes.py:1579`.

Outros achados:

- `perguntas_respondidas_a_validar` retorna lista global e seu queryset tem cache integral: `app/chamados/juizes.py:1698`.
- `dias_trancados` materializa `pares`, vetor de saída e vetores auxiliares por chamada: `app/chamados/juizes.py:1022`.
- A carga de tarefa materializa perguntas, decisões e pares da janela: `app/chamados/services/tarefa_do_dia.py:376`, `app/chamados/services/tarefa_do_dia.py:391`, `app/chamados/services/tarefa_do_dia.py:422`.
- A tarefa mantém caches de grade por colaborador e instâncias/marcos por dia, limitados à carga: `app/chamados/services/tarefa_do_dia.py:500`, `app/chamados/services/tarefa_do_dia.py:508`, `app/chamados/services/tarefa_do_dia.py:514`.
- O fio carrega disputas e perguntas somente do colaborador, mas conserva dicionários/listas durante a chamada: `app/chamados/services/fio.py:204`, `app/chamados/services/fio.py:211`, `app/chamados/services/fio.py:237`, `app/chamados/services/fio.py:255`.
- A disputa deve conservar todas as suas perguntas: `pode_fechar` depende de universo integral, `app/chamados/models.py:1854`; `respostas_sem_veredito` recebe esse universo explicitamente, `app/chamados/models.py:1841`.
- O juiz memoiza por instância, sem cache global persistente: `app/chamados/juizes.py:879`, `app/chamados/juizes.py:901`.
- `q_respondida_sem_texto` e `q_respondida_com_texto` são expressões SQL puras: `app/chamados/juizes.py:1531`, `app/chamados/juizes.py:1541`. Paridade por pergunta pode usar `Exists`/anotação filtrada no banco, mas executa a face também nas partes, contrariando sua interpretação estrita de “somente agregado”. [nao verificado]
- Há duas FKs de colaborador relevantes: perguntas têm FK própria em `app/chamados/models.py:2454` e disputa em `app/chamados/models.py:2437`; a carga filtra pela própria FK da pergunta em `app/chamados/services/tarefa_do_dia.py:377`, enquanto a divisão pedida usa a disputa. Igualdade dessas FKs no dado: [nao verificado].

Proposta de fingerprint: multiconjunto por leitor/contexto/faixa de pk, contendo contagem e somas independentes de hashes de cada unidade; consolidação soma contagens/digests/histogramas. Para memória constante, emitir fragmentos de faixa periodicamente e consolidar externamente; armazenar todas as faixas em dicionário ainda cresce com sua quantidade. Isso muda o algoritmo do SHA da lista ordenada da v1, mas mantém comparação determinística base/cópia quando ambas usam o mesmo novo texto. [nao verificado]

`SEQUENCIAS` pode morrer ao final de cada chamada raiz, pois a chave v1 já inclui raiz/contexto; contador por unidade dentro de uma única chamada global continua podendo crescer com a frota. Eliminar também esse crescimento exige alterar representação da ordem dos observados ou emitir registros de ocorrência sem conservar o contador por identidade; essa diferença deve ser declarada. [nao verificado]

Não achei `AGENTS.md` no inventário consultado; a base e o hash não puderam ser confirmados por Git porque a cópia não é repositório Git. [nao verificado]

## 13/11 00:07 · sessao dff820e3ed41 · - · - · origem=Ronald (tmux)

Há incompatibilidades reais com a exigência literal de que **cada consulta interna dos leitores por colaborador já venha filtrada pela parte**:

- `o_que_segura(colab,...)` chama `classificar_export(empresa,mes,ano)` para a empresa inteira e só então escolhe o colaborador em Python. `app/folha/fora_da_folha.py:227`, `app/folha/fora_da_folha.py:228`.
- O export consulta todos os fechamentos da empresa naquela competência. `app/folha/export.py:785`.
- A carga da tarefa chama `alimentar_fila` ao avaliar a primeira tarefa. `app/chamados/services/tarefa_do_dia.py:529`.
- `alimentar_fila` chama `particionar_verbos(qs_vivos())` global e percorre toda `qs_na_fila`. `app/chamados/juizes.py:1558`, `app/chamados/juizes.py:1561`.
- A documentação declara que o crédito depende do conjunto global recebido e pode mudar com o recorte. Filtrar essa chamada muda a regra observada. `app/chamados/juizes.py:1577`.

Portanto, particionar os argumentos e as consultas da **sonda** é viável; prometer o mesmo para todas as consultas **internas de produção**, preservando os leitores reais sem alterações, não é. `app/chamados/juizes.py:1558`, `app/folha/fora_da_folha.py:227`.

O universo exato do plano do backfill tem de vir do agregado:

- `censo_alcance` enumera `empresas_visiveis()`, depois `competencias_a_lavrar(emp,hoje)`, depois os dias de `dias_que_seguram(... )["dias"]`; deduplica `(colaborador_id,data)` com `setdefault`. `app/chamados/services/vigia_fio.py:571`, `app/chamados/services/vigia_fio.py:587`, `app/chamados/services/vigia_fio.py:589`, `app/chamados/services/vigia_fio.py:591`.
- Não equivale às células das janelas, nem a colaborador×todos os dias. A seleção da folha parte de `classificar_export`, retém apenas `furo_espelho` e lê `_dias_que_retem` desses colaboradores. `app/folha/fora_da_folha.py:130`, `app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:148`.
- O plano v1 percorre todos os registros `vigia.censo_alcance.dia`, independentemente da rota, apesar do comentário anterior mencionar `sem_porta`. `e3_diff_leitores_v1.py:[numeração do anexo necessária]`.
- Para medir `ato_do_dia` nas partes sem repetir `censo_alcance`, o agregado precisa emitir um **manifesto de IDs/datas**, consumido pelas partes; só hashes não permitem recuperar essas unidades. Ou é preciso alterar a interface do leitor nativo, o que excede “reescrever só a sonda”. `app/chamados/services/vigia_fio.py:591`, `app/chamados/services/vigia_fio.py:633`.
- `ato_do_dia` é leitura da tarefa e consulta as perguntas daquele colaborador/dia com `executar=False`. `app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:89`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`.

Classes/ramificações que o relatório precisa conservar para explicar a queda:

- Primeiro, portas `porta_fila`/`porta_pergunta`, publicadas pela tarefa. `app/chamados/services/vigia_fio.py:648`.
- Depois, `de_hoje_pra_frente`. `app/chamados/services/vigia_fio.py:656`.
- Depois os quatro baldes de lei: decisão pendente, ausência aberta, escala não descreve o dia, fio do admin. `app/chamados/services/vigia_fio.py:662`, `app/chamados/services/vigia_fio.py:306`.
- Sem porta, causa **a** = véu/chamado vivo; **b** = respondida sem texto; **c** = respondida sob âncora terminal; **d** = colaborador na lista mas dia fora; **e** = demais detalhes declarados. `app/chamados/services/vigia_fio.py:676`, `app/chamados/services/vigia_fio.py:678`, `app/chamados/services/vigia_fio.py:682`, `app/chamados/services/vigia_fio.py:684`, `app/chamados/services/vigia_fio.py:686`.
- Detalhes e: `sem_celula`, `dia_na_lista_dono_*`, `sem_pergunta_viva`, `pergunta_viva_sob_chamado_terminal`, `pergunta_viva_sem_ancora`, `pergunta_viva_fora_do_app`, `outra`. `app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:690`.
- Invariantes: soma das rotas/baldes/sem_porta = universo; a+b+c+d+e = fio_morto_alcance. `app/chamados/services/vigia_fio.py:531`.
- O perfil `_listar` cobre todos os dias se `limite=sys.maxsize`; cada retorno informa causa, detalhe e rota sem implementar outro juiz. `app/chamados/services/vigia_fio.py:629`, `app/chamados/services/vigia_fio.py:632`.

A cópia não contém repositório Git acessível no cwd; comparação com base `78994522d`, mudanças exatas base→cópia e queda real por classe são **[nao verificado]**. Os números de produção mencionados em documentação não devem substituir o dado fornecido. **[nao verificado]**

## 13/11 00:06 · sessao 24dc542a8407 · - · - · origem=Ronald (tmux)

Leitores que devem ficar no agregado para preservar a chamada integral da v1:

| Família | Fonte e materialização nativa |
|---|---|
| `relatorios_disputa.coletar` | `app/chamados/relatorios_disputa.py:31`; queryset com prefetch e listas por categoria em `:35`, `:45`, `:51`, `:60`. |
| `raiox.creditos_perguntas` | `app/chamados/services/raiox.py:51`; conjunto de chamados vivos em `:58`, lista integral de perguntas em `:135`, listas respondidas/pendentes em `:143`, conjuntos de créditos em `:146`. |
| `cobranca._calados` | `app/chamados/services/cobranca.py:221`; queryset cacheado em `:236`, listas por colaborador em `:239`, mapa de colaboradores em `:247`. |
| `cobranca.linhas_para_a_pauta` | `app/chamados/services/cobranca.py:260`; chama `_calados` novamente em `:267`, repetição nativa que a v1 mede. |
| `dias_que_seguram` | `app/folha/fora_da_folha.py:113`; export integral, IDs, colaboradores e dias em `:129`, `:142`, `:143`. |
| `fora_da_folha` | `app/folha/fora_da_folha.py:153`; usa universo anterior, retidos/carga em `:174`, `:177`, lista de linhas em `:183`, retorno em `:209`. |
| `retrato_folha.montar_linhas` | `app/folha/retrato_folha.py:1120`; mantém retorno integral de folha em `:1158`, mapa por colaborador em `:1159`, lista de linhas em `:1162`. |
| `retrato_folha.contadores` | `app/folha/retrato_folha.py:114`; copia linhas para cada filtro via `linhas_do_retrato`, `:110`, `:120`. |
| `competencia_trancada.medir/encerrar` | `app/chamados/services/competencia_trancada.py:66`, `:164`; listas/sets por empresa e competência em `:77`, `:88`, `:110`; DRY `encerrar` chama `medir` novamente em `:166`. |
| `cadastro_realidade.beco_em_lote` | `app/escala/services/cadastro_realidade.py:442`; recebe somente janelas, sem filtro de colaboradores; mapas/cargas/lista em `:475`, `:485`, `:492`, `:505`, `:514`, `:521`, `:540`. |
| `cadastro_realidade._folga_nao_definida_sem_porta` | `app/escala/services/cadastro_realidade.py:553`; empresas ativas e competências nativas em `:571`, `:576`, chamada integral do beco em `:582`. |
| `furo_sem_canal.medir` | `app/chamados/services/furo_sem_canal.py:43`; empresas ativas em `:61`, mapa e alvo em `:75`, `:80`, carga em `:90`, lista final em `:101`. |
| `pautas.dias_sem_fio` | `app/ponto/services/pautas_dos_discordantes.py:46`; empresas ativas em `:66`, células materializadas em `:72`, carga em `:79`, lista final em `:84`. |
| `pautas.apurar` | `app/ponto/management/commands/pautas_discordantes.py:56`; lê universo anterior, agrupa colaboradores e acumula retorno em `:68`, `:71`, `:76`, `:111`. |
| `pautas.sincronizar.DRY` | `app/ponto/services/pautas_dos_discordantes.py:155`; lê todas as pautas vivas da família em `:169` e compara com entrada inteira em `:174`. Particionar sua entrada mudaria `fechar`, incluindo pautas pertencentes às demais partes. |

Há conflito concreto entre preservar leitores e exigir que toda consulta do leitor por colaborador esteja filtrada: `o_que_segura(colab,...)` chama `classificar_export(empresa,mes,ano)` para a empresa inteira e só depois escolhe o colaborador com `next`; não recebe filtro para corrigir isso sem alterar o leitor. `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:227`.

A remoção de `REGISTROS["linhas"]` não elimina essas materializações nativas. O agregado abaixo de 2 GB permanece [nao verificado]; em particular, uma chamada integral pode exceder o teto antes de retornar para a checagem cooperativa. As materializações acima demonstram o risco, mas não quantificam RSS em produção. `app/chamados/services/raiox.py:135`, `app/escala/services/cadastro_realidade.py:475`.

O molde colado pelo usuário é um gerador de substituições e não contém uma classe `Registro`; equivalência com essa classe mencionada no pedido é [nao verificado].

## 12/11 23:31 · sessao 82935945aa8f · E3-FORMA-SQL · leitura · origem=Code

O script abaixo não foi executado. A BASE foi reconstruída pelo diff fornecido; suas assinaturas não foram inspecionadas numa segunda árvore disponível. [nao verificado]

Há duas lacunas deliberadas: o GET da API escreve notas, e o DRY do backfill E4b executa atos antes do rollback. O script chama alternativas de leitura e identifica essas exclusões. (app/api/views.py:1039; app/chamados/management/commands/backfill_fio_morto_e4b.py:315)

```python
# SOMENTE TEXTO. Não executado; compatibilidade em execução: [nao verificado].
#
# Mesmo texto nas duas árvores; Django configurado, tenant juliani ativo.
# Mesma sombra imutável entre as rodadas: [nao verificado].
#
# Comparar LEITOR e UNIVERSO; ASSINATURAS/TEMPOS são diagnóstico.
# AUSENTE_BASE_ESPERADO não simula função nova na BASE.
#
# Fontes principais:
# app/chamados/services/vigia_fio.py:542
# app/chamados/services/tarefa_do_dia.py:568
# app/folha/fora_da_folha.py:153
# app/escala/services/cadastro_realidade.py:442
# app/chamados/services/fio.py:190
# app/chamados/services/furo_sem_canal.py:43
#
# Não chama:
# - API GET: app/api/views.py:1039, marca notas lidas.
# - vigiar_fio_morto.handle: grava arquivo; comando:127.
# - backfill E4b.handle: DRY executa atos; comando:315.
# - escritores sem DRY: lista em LIMITES, abaixo.
#
# Perfis de retorno observam LOCAIS DA FUNÇÃO REAL.
# Não substituem juiz, queryset, universo, argumento ou retorno.
#
# Quatro CPUs devem ser atribuídas pelo executor antes de carregar Django.
# O script confere afinidade, não cria processos/threads.

import sys
sys.dont_write_bytecode = True

import collections
import collections.abc
import contextlib
import copy
import dataclasses
import datetime as dt
import decimal
import enum
import hashlib
import importlib
import inspect
import io
import json
import logging
import math
import os
import re
import time
import uuid

from unittest.mock import patch
from zoneinfo import ZoneInfo

FIXO = dt.datetime.fromisoformat("2026-11-12T23:48:00+00:00")
ZONA = ZoneInfo("America/Sao_Paulo")
HOJE = FIXO.astimezone(ZONA).date()
DIA = dt.timedelta(days=1)
BLOCO = 1000

DATA_REAL = dt.date
INSTANTE_REAL = dt.datetime
Model = QuerySet = None
transaction = J = None

ASSINATURAS = {}
AUSENTES = set()
REGISTROS = {}
TEMPOS = {}
UNIVERSO = {}
INCANONIZAVEL = collections.Counter()
SQL_ERROS = collections.Counter()
OBS_ERROS = collections.Counter()
SEQUENCIAS = collections.Counter()
ATUAL = None
ALCANCADO_FIM = False
ROLLBACKS = 0

# Só vocabulário fixo do código aparece como texto no valor canonizado.
# Todo outro str vira comprimento + SHA256, inclusive prosa.
TOKENS = set("""
porta_fila porta_pergunta sem_porta de_hoje_pra_frente
admin colab colaborador sistema cadastro estrutura ninguem nada chamado nao_sei
a b c d e
AGUARDA_COLAB AGUARDA_SUPERVISAO SEM_PERGUNTA PRONTA_FECHAR
decisao_do_dia_pendente aguarda_decisao_de_ausencia
escala_nao_descreve_o_dia fio_do_admin
fora_vinculo isento_ponto competencia_trancada sem_celula nao_venceu
ata_sem_lampadas passivo_sem_linha respondida_sem_texto
aguarda_resposta aguarda_validacao sem_resposta resolvida pendencia_admin
aguarda_validacao validada_sem_gravar
devolvida_ao_colaborador materializacao_recusada
veredito_sem_lavra_vigente validada_sem_efeito efeito_sem_lampada
encerrada_pelo_juiz silencio_decidido absorvida_aprovada
absorcao_rejeitada absorcao_aguarda_documento absorcao_pendente
perguntar responder validar materializar rejulgar decidir_ausencia
entregar_documento emissor cartorio materializacao
furo cobrado discordante indefinida concordante
entra bloqueado modulo_proprio sim nao
""".split())

# Chaves textuais são nomes de campos, nunca conteúdo de usuário.
CHAVES = set("""
pk id colab colaborador_id empresa empresa_id pergunta_id disputa_id
celula_id chamado_id chamado_destino_id data dia dia_do_turno
agora competencia mes ano ini fim
rota fila verbos fio nao_sei razao instancias instancia tipo hora
data_civil lampada vencido motivo acoes acao de porta
pode_fechar todas_respondidas pode_solicitar_correcao resposta_enviada
tem_pergunta_viva perguntas perguntas_disputa itens estado dono resumo
status grupo grupos motivos motivos_retencao dias datas por_fio
total entram fora_por_motivo furo_espelho dias_por_fio
colaboradores esperado universo por_veredito amostra maiores
com_pergunta_aberta com_chamado_vivo com_porta_fila com_porta_pergunta
alcance_universo alcance_competencias alcance_porta_fila
alcance_porta_pergunta alcance_de_hoje_pra_frente fio_morto_alcance
alcance_porta_exige_o_admin alcance_e_detalhe alcance_por_rota_papel
alcance_dias alcance_dias_omitidos alcance_cego
alcance_a alcance_b alcance_c alcance_d alcance_e
alcance_decisao_do_dia_pendente alcance_aguarda_decisao_de_ausencia
alcance_escala_nao_descreve_o_dia alcance_fio_do_admin
causa detalhe papel porta_real perguntas_reais
retido sem_canal candidato vencido_cobranca sem_canal_push
creditada_respondida creditada_pendente pertence
q_vivas viva juiz respondida_sem_texto trancada fora_do_fato
paridade_q_sem_texto
colabs furo itens dias_ferias_com_batida
dono_do_dia ec_do_dia posto_do_dia pai_limbo
saldo codigo_tipo
chamados disputas fica_resposta_sem_veredito fica_pergunta_viva_fora
fica_resgatado perguntas_com_resposta_sem_veredito
disputas_com_pai_vivo_fora sem_dia fica_fora_da_tranca decidir_pelo_dp
vivas_apesar_da_tranca perguntas_lidas_trancadas acoes aplicadas [nao verificado]
ok pendentes prazo_ate canal erros
alvo abrir fechar mantidas alvo_dias estrutura por_dono
abrir_chaves fechar_chaves n chave
q_respondida_sem_texto q_respondida_com_texto
resultado ordem faltam saida_sha256 bytes
""".split())

# Não imprime nem conserva esses campos na representação canônica.
PRIVADOS = {
    "nome", "nome_completo", "cpf", "username", "email",
    "codigo", "codigo_dominio", "telefone", "matricula",
}


class DesfazerSonda(Exception):
    pass


class FalhaDaSonda(BaseException):
    pass


class EscritaBloqueada(FalhaDaSonda):
    pass


class FormatoInesperado(Exception):
    pass


class Sink:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass

    def isatty(self):
        return False


class MetaRelogio(type):
    def __instancecheck__(cls, objeto):
        return isinstance(objeto, cls._real)


class DataFixa(DATA_REAL, metaclass=MetaRelogio):
    _real = DATA_REAL

    @classmethod
    def today(cls):
        return HOJE


class InstanteFixo(INSTANTE_REAL, metaclass=MetaRelogio):
    _real = INSTANTE_REAL

    @classmethod
    def now(cls, tz=None):
        if tz is None:
            return FIXO.astimezone(ZONA).replace(tzinfo=None)
        return FIXO.astimezone(tz)

    @classmethod
    def today(cls):
        return cls.now()

    @classmethod
    def utcnow(cls):
        return FIXO.astimezone(dt.timezone.utc).replace(tzinfo=None)


def serial(valor):
    return json.dumps(
        valor, ensure_ascii=True, sort_keys=True,
        separators=(",", ":"), allow_nan=False,
    )


def sha(valor):
    texto = valor if isinstance(valor, str) else serial(valor)
    return hashlib.sha256(texto.encode("utf-8")).hexdigest()


def sem_forma(valor, prefixo="sem_forma"):
    tipo = type(valor)
    chave = tipo.__module__ + "." + tipo.__qualname__
    INCANONIZAVEL[prefixo + ":" + chave] += 1
    return ("incanonizavel", tipo.__module__, tipo.__qualname__)


def canonico(valor, ativos=None):
    """Não usa repr/str de objeto desconhecido; falha vira marcador por tipo."""
    if ativos is None:
        ativos = set()

    try:
        if valor is None or isinstance(valor, (bool, int)):
            return valor
        if isinstance(valor, enum.Enum):
            return canonico(valor.value, ativos)
        if isinstance(valor, str):
            if valor in TOKENS:
                return valor
            # Datas ISO completas, sem aceitar texto adicional.
            if re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
                return valor
            return ("str_sha256", len(valor), sha(valor))
        if isinstance(valor, decimal.Decimal):
            return ("decimal", str(valor))
        if isinstance(valor, (INSTANTE_REAL, DATA_REAL, dt.time)):
            return ("iso", valor.isoformat())
        if isinstance(valor, dt.timedelta):
            return ("duracao", valor.days, valor.seconds, valor.microseconds)
        if isinstance(valor, float):
            return ("float", valor.hex())
        if isinstance(valor, uuid.UUID):
            return ("uuid", str(valor))
        if isinstance(valor, bytes):
            return ("bytes_sha256", len(valor), hashlib.sha256(valor).hexdigest())

        ident = id(valor)
        if ident in ativos:
            return sem_forma(valor, "ciclo")
        ativos.add(ident)
        try:
            if Model is not None and isinstance(valor, Model):
                # Só identidade; não acessa relações ou campos diferidos.
                return ("model", valor._meta.label_lower, valor.pk)

            if QuerySet is not None and isinstance(valor, QuerySet):
                itens = [canonico(x, ativos) for x in valor]
                return ("queryset", sorted(itens, key=serial))

            if isinstance(valor, collections.abc.Mapping):
                itens = []
                for chave, conteudo in valor.items():
                    if isinstance(chave, str) and chave.lower() in PRIVADOS:
                        continue
                    k = (
                        chave if isinstance(chave, str) and chave in CHAVES
                        else canonico(chave, ativos)
                    )
                    itens.append((k, canonico(conteudo, ativos)))
                return ("dict", sorted(itens, key=lambda x: serial(x[0])))

            if isinstance(valor, (set, frozenset)):
                return (
                    "set",
                    sorted((canonico(x, ativos) for x in valor), key=serial),
                )

            if isinstance(valor, (list, tuple)):
                return (
                    "list" if isinstance(valor, list) else "tuple",
                    [canonico(x, ativos) for x in valor],
                )

            if dataclasses.is_dataclass(valor) and not isinstance(valor, type):
                return canonico({
                    f.name: getattr(valor, f.name)
                    for f in dataclasses.fields(valor)
                    if f.name.lower() not in PRIVADOS
                }, ativos)

            return sem_forma(valor)
        finally:
            ativos.remove(ident)
    except Exception:
        return sem_forma(valor, "erro_canonizador")


def notar(funcao):
    chave = funcao.__module__ + "." + funcao.__qualname__
    if chave not in ASSINATURAS:
        ASSINATURAS[chave] = {
            "assinatura": str(inspect.signature(funcao)),
            "arquivo": inspect.getsourcefile(funcao),
            "linha": inspect.getsourcelines(funcao)[1],
        }
    return inspect.signature(funcao)


def registro(nome):
    return REGISTROS.setdefault(nome, {
        "linhas": [],
        "excecoes": collections.Counter(),
    })


def guardar(nome, unidade, valor, excecao=None):
    # Unidade é montada pela sonda exclusivamente com IDs, datas e índices.
    r = registro(nome)
    r["linhas"].append([unidade, canonico(valor)])
    if excecao:
        r["excecoes"][excecao] += 1


def observar(nome, unidade, valor):
    if ATUAL is None:
        return
    raiz, contexto = ATUAL
    chave = (raiz, serial(contexto), nome, serial(unidade))
    ordem = SEQUENCIAS[chave]
    SEQUENCIAS[chave] += 1
    guardar(
        raiz + "::observado::" + nome,
        [contexto, unidade, ordem],
        valor,
    )


def selecionar(d, campos):
    return {k: d[k] for k in campos if k in d}


def explicacao_segura(e):
    if e is None:
        return None
    return {
        "pode_fechar": e.get("pode_fechar"),
        # Prosa só entra em digest.
        "resumo": e.get("resumo"),
        "itens": [
            selecionar(i, (
                "pergunta_id", "estado", "dono", "dia",
                "motivo", "acao",
            ))
            for i in e.get("itens", ())
        ],
    }


def perfil(frame, evento, retorno):
    """Observação passiva das funções reais, inclusive seus conjuntos finais."""
    if evento != "return" or retorno is None or ATUAL is None:
        return

    modulo = frame.f_globals.get("__name__")
    nome = frame.f_code.co_name
    locais = frame.f_locals

    try:
        # Cada ramo nativo do vigia passa por _listar.
        # app/chamados/services/vigia_fio.py:629
        if (
            modulo == "chamados.services.vigia_fio"
            and nome == "_listar"
            and frame.f_back is not None
            and frame.f_back.f_code.co_name == "censo_alcance"
        ):
            pai = frame.f_back.f_locals
            observar("vigia.ramo", [locais["cid"], locais["dt"]], {
                "celula_id": locais["c"],
                "competencia": locais["comp"],
                "rota": locais["rota"],
                "causa": locais.get("causa", ""),
                "detalhe": locais.get("detalhe", ""),
                "papel": locais["papel"],
                "porta_real": pai.get("porta"),
                "perguntas_reais": [p.pk for p in pai.get("ps", ())],
            })

        # Conjuntos calculados pela FUNÇÃO REAL; não pela sonda.
        # app/chamados/services/furo_sem_canal.py:79,103
        elif modulo == "chamados.services.furo_sem_canal" and nome == "medir":
            if "alvo" not in locais:
                return
            sem = {x[3] for x in locais["sem_canal"]}
            for cid, dia, veredito, pk in locais["alvo"]:
                observar("furo.unidade", [pk, cid, dia.isoformat()], {
                    "sem_canal": pk in sem,
                    "status": veredito,
                })

        # Os dois conjuntos finais pertencem ao cálculo nativo.
        # app/chamados/services/cobranca.py:239,246,257
        elif modulo == "chamados.services.cobranca" and nome == "_calados":
            por = locais.get("por_colab", {})
            vencidos = locais.get("vencidos", set())
            sem = locais.get("sem_canal", set())
            ps = locais.get("ps")
            # A iteração nativa já avaliou o queryset.
            carregadas = (
                ps._result_cache
                if QuerySet is not None and isinstance(ps, QuerySet)
                else None
            )
            if carregadas is not None:
                for p in carregadas:
                    observar("cobranca.entrada", [p.pk], {
                        "colaborador_id": p.disputa.colaborador_id,
                    })
            for cid in sorted(por):
                observar("cobranca.unidade", [cid], {
                    "perguntas": sorted(p.pk for p in por[cid]),
                    "vencido_cobranca": cid in vencidos,
                    "sem_canal_push": cid in sem,
                })

        # Inclui tarefas chamadas por vigia, folha, pauta e beco.
        # app/chamados/services/tarefa_do_dia.py:518
        elif modulo == "chamados.services.tarefa_do_dia" and nome == "tarefa":
            if "colab" in locais and "dia" in locais:
                c = locais["colab"]
                observar(
                    "carga.tarefa",
                    [getattr(c, "pk", c), locais["dia"].isoformat()],
                    retorno,
                )

        # Universo e alimentação efetivamente usados por cada chamador.
        # app/chamados/juizes.py:1570
        elif modulo == "chamados.juizes" and nome == "alimentar_portas":
            for pk, cid in sorted(retorno.get("colab", {}).items()):
                observar("alimentar_portas", [pk, cid], {
                    "perguntas": sorted(
                        p.pk for p in retorno.get("vivas", {}).get(pk, ())
                    ),
                    "fila": retorno.get("fila_cel", {}).get(pk, ()),
                })

        # Candidatos e resultado final do beco real.
        # app/escala/services/cadastro_realidade.py:475,540
        elif modulo == "escala.services.cadastro_realidade" and nome == "beco_em_lote":
            saida = {
                (cid, item["data"]): item
                for cid, item in retorno
            }
            for pk, (cid, dia, janela) in sorted(locais.get("cels", {}).items()):
                item = saida.get((cid, dia.isoformat()))
                observar("beco.unidade", [pk, cid, dia.isoformat()], {
                    "pertence": item is not None,
                    "resultado": (
                        None if item is None else selecionar(item, (
                            "data", "dono_do_dia", "ec_do_dia", "posto_do_dia",
                            "pai_limbo", "perguntas",
                        ))
                    ),
                })

        # Privada com assinatura diferente: observa chamadas dos callers reais.
        # app/folha/fora_da_folha.py:77
        elif modulo == "folha.fora_da_folha" and nome == "_fio_do_dia":
            if "colab" in locais:
                unidade = [locais["colab"].pk, locais["dia"].isoformat()]
            else:
                pk = locais.get("celula_id")
                unidade = UNIVERSO["_celula_para_dia"].get(pk, [None, pk])
            observar("folha.fio_do_dia", unidade, retorno)

    except Exception as erro:
        # Uma observação que falhou invalida medicao_sem_falhas.
        OBS_ERROS[type(erro).__module__ + "." + type(erro).__qualname__] += 1


def adaptar(funcao, args, kwargs):
    """Só adapta parâmetros declarados; não tenta chamada alternativa."""
    assinatura = notar(funcao)
    ligados = assinatura.bind_partial(*args, **kwargs)
    resultado = dict(kwargs)
    if "agora" in assinatura.parameters and "agora" not in ligados.arguments:
        resultado["agora"] = FIXO
    assinatura.bind(*args, **resultado)
    return resultado


def rodar(nome, unidade, funcao, *args, projetar=None, **kwargs):
    global ATUAL
    anterior = ATUAL
    ATUAL = (nome, unidade)
    try:
        kwargs = adaptar(funcao, args, kwargs)
        with transaction.atomic(using="default"):
            with J.tranca_em_lote():
                valor = funcao(*args, **kwargs)
                if QuerySet is not None and isinstance(valor, QuerySet):
                    valor = list(valor)
                if projetar is not None:
                    projetar(valor)
                else:
                    guardar(nome, unidade, valor)
        return True, valor
    except Exception as erro:
        classe = type(erro).__module__ + "." + type(erro).__qualname__
        guardar(nome, unidade, ("EXC", classe), excecao=classe)
        return False, None
    finally:
        ATUAL = anterior


def fase(nome, itens, executar):
    itens = list(itens)
    inicio = time.perf_counter()
    estimativa = None
    amostra = min(10, len(itens))
    for n, item in enumerate(itens, 1):
        executar(item)
        if n == amostra:
            estimativa = (time.perf_counter() - inicio) * len(itens) / n
    TEMPOS[nome] = {
        "unidades_planejadas": len(itens),
        "segundos": round(time.perf_counter() - inicio, 3),
        "amostra": amostra,
        "estimativa_segundos_pelas_primeiras_unidades": (
            None if estimativa is None else round(estimativa, 3)
        ),
    }


def resumo(nome, unidade, valor):
    """Resumo integral redigido; as projeções abaixo enumeram as unidades."""
    c = canonico(valor)
    guardar(nome + "::integral", unidade, {
        "saida_sha256": sha(c),
        "bytes": len(serial(c).encode("utf-8")),
    })


def linhas(nome, contexto, itens, identidade, transformar=lambda x: x):
    itens = list(itens)
    guardar(nome + "::universo", contexto, len(itens))
    for item in itens:
        guardar(
            nome, [contexto, identidade(item)],
            transformar(item),
        )


def observar_excecoes(stack, modulo, atributo):
    """Conta exceções mesmo quando 

[truncado em 20000 de 61994 caracteres]

## 12/11 23:21 · sessao 4015c410eddb · - · - · origem=Ronald (tmux)

Leitura concluída, sem sonda nem banco. A BASE não está disponível via Git neste workspace; as linhas abaixo da BASE foram derivadas dos hunks fornecidos. [nao verificado]

| Função | BASE → CÓPIA | Assinatura |
|---|---|---|
| `texto_vazio` | `app/chamados/juizes.py:1510` → `:1510` | `(t)` |
| `respondida_sem_texto` | `app/chamados/juizes.py:1515` → `:1515` | `(p)` |
| `q_respondida_sem_texto` | ausente → `app/chamados/juizes.py:1531` | `()` |
| `q_respondida_com_texto` | ausente → `app/chamados/juizes.py:1541` | `()` |
| `alimentar_portas` | `app/chamados/juizes.py:1542` → `:1570` | `(celula_ids, colabs=(), *, agora=None)` |
| `perguntas_respondidas_a_validar` | `app/chamados/juizes.py:1668` → `:1698` | `()` |
| `DisputaSupervisao.respostas_sem_veredito` | `app/chamados/models.py:1827` → `:1827` | BASE `(self, ignorar_tranca=False)`; CÓPIA acrescenta `perguntas=None` |
| `DisputaSupervisao.pode_fechar` | `app/chamados/models.py:1848` → `:1854` | BASE `(self)`; CÓPIA `(self, perguntas=None)` |
| `PerguntaDisputa.q_vivas` | `app/chamados/models.py:3327` → `:3368` | `(cls)` |
| `carga_da_tarefa` | `app/chamados/services/tarefa_do_dia.py:557` → `:568` | `(colaboradores, ini, fim, agora)` |
| `tarefa_do_dia` | `app/chamados/services/tarefa_do_dia.py:562` → `:573` | `(colab, dia_do_turno, agora, *, carga=None)` |
| `porta_da_tarefa` | ausente → `app/chamados/services/tarefa_do_dia.py:559` | `(tarefa)` |
| `furo_sem_canal.medir` | `app/chamados/services/furo_sem_canal.py:38` → `:43` | BASE `(mes=None, ano=None)`; CÓPIA acrescenta `*, agora` |
| `competencia_trancada.medir` | `app/chamados/services/competencia_trancada.py:66` nas duas | `(empresa_id, mes, ano)` |
| `competencia_trancada.encerrar` | `app/chamados/services/competencia_trancada.py:164` nas duas | `(empresa_id, mes, ano, *, aplicar, autor=None)` |
| `backfill_fio_morto_e4b._alcance` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:76` nas duas | BASE `(empresas, hoje)`; CÓPIA acrescenta `agora` |
| `backfill_fio_morto_e4b.ato_do_dia` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:82` nas duas | `(colab, dia, agora, *, hoje)` |

A adaptação deve selecionar os argumentos por `inspect.signature(funcao).parameters`, fazendo `bind` antes da chamada; para métodos da disputa, passar `perguntas=` somente quando existir. O universo desse argumento precisa ser a disputa inteira: a CÓPIA documenta e consome esse contrato em `app/chamados/models.py:1845`, `app/chamados/models.py:1859`.

`perguntas_respondidas_a_validar()` deve ser chamada uma vez globalmente, conservando todos os pks devolvidos; o chamador de produção não passa recorte por empresa ou competência (`app/api/views_mensageria.py:540`, `app/chamados/services/validacao.py:929`). Para `q_vivas`, registrar também a frota inteira de pks e projetar os recortes solicitados depois, sem trocar seu predicado (`app/chamados/models.py:3368`).

A carga da tarefa aceita os colaboradores e a janela inteira, e `tarefa_do_dia` aceita a carga pronta (`app/chamados/services/tarefa_do_dia.py:568`, `app/chamados/services/tarefa_do_dia.py:573`). O objeto retornado é uma classe local: `CargaDaTarefa.__init__` em `app/chamados/services/tarefa_do_dia.py:362`, `.tarefa` em `:518`; a carga já consulta perguntas, decisões, efeitos, fila e tranca por lote (`app/chamados/services/tarefa_do_dia.py:372`, `:388`, `:409`, `:415`, `:423`).

Para `furo_sem_canal`, o retorno público só contém até vinte exemplos, portanto ele sozinho não satisfaz a lista completa (`app/chamados/services/furo_sem_canal.py:116`). Uma observação de retorno via `sys.setprofile`, limitada ao `func.__code__` verdadeiro, pode colher os locais `alvo` e `sem_canal`: ambos existem nas duas árvores, pelo diff; registrar cada tupla `(cid, dia, veredito, celula_pk)` do universo e a pertença ao resultado, sem reproduzir a seleção (`app/chamados/services/furo_sem_canal.py:79`, `:103`). Os locais novos `rota` e `nao_sei` permitem diagnóstico adicional apenas na CÓPIA (`app/chamados/services/furo_sem_canal.py:88`).

`lavrar_furo_sem_canal.Command.handle(..., apply=False)` é DRY efetivo: retorna antes de `MetricaSnapshot.objects.update_or_create` (`app/chamados/management/commands/lavrar_furo_sem_canal.py:41`, `:49`). `competencia_trancada.encerrar(..., aplicar=False)` também retorna antes das escritas e antes de obter/criar o usuário de sistema (`app/chamados/services/competencia_trancada.py:166`, `:168`, `:176`).

**Não chamar o DRY de `backfill_fio_morto_e4b.handle`.** Ele executa os atos e só desfaz ao final: entra no bloco em `app/chamados/management/commands/backfill_fio_morto_e4b.py:279`, trava em `:280`, chama `_executar_dia` em `:315` e levanta `_Desfaz` em `:357`. A alternativa real sem escrita é `_alcance(...)` e, para o plano por dia, `ato_do_dia(...)`; este chama a reabertura com `executar=False` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:82`, `:90`). A reabertura documenta e implementa o retorno sem escrita antes do ato (`app/chamados/services/disputa_emissao.py:1820`, `:1856`).

Os demais escritores alterados não têm DRY equivalente: `reabrir_para_correcao` grava trilha e descarimba (`app/chamados/services/disputa_emissao.py:1618`, `:1635`); `responder_automaticamente` escreve resposta e decisão (`app/chamados/services/disputa_emissao.py:1640`); `recusar_pergunta` salva, reabre e envia notificações (`app/chamados/services/validacao.py:596`, `:600`, `:620`); `PerguntaDisputa.descarimbar_em_lote` faz `update` (`app/chamados/models.py:3306`). Alternativas próximas: observar `p.fora_do_fato()`, `respondida_sem_texto(p)`, `respostas_sem_veredito`, `pode_fechar` e `tarefa_do_dia`; essas leituras não equivalem ao resultado completo dos escritores (`app/chamados/models.py:3219`, `app/chamados/juizes.py:1515`, `app/chamados/models.py:1827`, `:1854`, `app/chamados/services/tarefa_do_dia.py:573`).

Competência/tranca adicional: chamar `competencia_trancada.medir` e/ou `encerrar(aplicar=False)` por empresa e competência; o universo inclui chamados vivos da empresa classificados pelo juiz da competência, perguntas da janela e disputas atingidas (`app/chamados/services/competencia_trancada.py:89`, `:110`, `:148`). Para todos os dia-colab com célula, chamar `dias_trancados(pares)` uma vez por janela e projetar os booleanos por par (`app/chamados/juizes.py:1004`). `dia_da_pergunta_trancado(p)` usa esse mesmo juiz sobre o dia real da pergunta (`app/chamados/juizes.py:974`, `:991`).

As janelas precisam vir das funções reais por empresa, registrando os limites encontrados: `janela_fechamento` respeita o corte da empresa (`app/ponto/janelas.py:54`), e `janela_atual` determina a corrente (`app/ponto/janelas.py:60`). O padrão é 21–20, mas a existência de empresas com outro corte no banco não foi verificada (`app/ponto/janelas.py:36`) [nao verificado].

O alcance do vigia usa a corrente e a anterior enquanto não entregue, e não necessariamente todas as competências 10 e 11: a fonte real é `competencias_a_lavrar` (`app/ponto/services/he_pendente_lavrado.py:209`, `:220`, `:223`). Não se deve forçar o censo a mudar esse universo; complementar com leituras explícitas por competência e declarar o recorte observado.

## 12/11 23:21 · sessao c35eb9326c3f · - · - · origem=Ronald (tmux)

Leitura concluída; BASE abaixo deriva dos hunks do diff fornecido, pois o workspace não tem `.git`. [nao verificado]

| Função | BASE | CÓPIA |
|---|---|---|
| `censo_alcance(empresas=None, hoje=None, limite=50, lista=None[, *, agora])` | `app/chamados/services/vigia_fio.py:542` | `app/chamados/services/vigia_fio.py:542` |
| `fora_da_folha(empresa, mes, ano, com_itens=False[, *, agora])` | `app/folha/fora_da_folha.py:142` | `app/folha/fora_da_folha.py:153` |
| `o_que_segura(colab, mes, ano[, *, agora])` | `app/folha/fora_da_folha.py:203` | `app/folha/fora_da_folha.py:215` |
| `montar_linhas(empresa, mes, ano[, *, agora])` | `app/folha/retrato_folha.py:1120` | `app/folha/retrato_folha.py:1120` |
| `dias_sem_fio(mes, ano, empresas=None[, *, agora])` | `app/ponto/services/pautas_dos_discordantes.py:46` | `app/ponto/services/pautas_dos_discordantes.py:46` |
| `beco_em_lote(janelas, hoje, *, agora)` | `app/escala/services/cadastro_realidade.py:442` | `app/escala/services/cadastro_realidade.py:442` |

As privadas de folha mudaram: `_porta(celula_id, alim)` BASE:66 vira `_porta(colab, dia, carga)` CÓPIA:71; `_fio_do_dia(celula_id, alim)` BASE:71 vira `_fio_do_dia(colab, dia, carga)` CÓPIA:77; `_alimentar` BASE:56 desaparece e `_carga` CÓPIA:62 nasce. Comparar seus chamadores comuns preserva a lógica real. `app/folha/fora_da_folha.py:62`, `app/folha/fora_da_folha.py:71`, `app/folha/fora_da_folha.py:77`.

`censo_alcance` mede somente os dias retidos de `competencias_a_lavrar`; esta fornece corrente e anterior apenas se anterior ainda não entregue. Competência 10 entregue não entra no censo real em novembro; não mudar esse universo para alegar cobertura. `app/chamados/services/vigia_fio.py:587`, `app/ponto/services/he_pendente_lavrado.py:209`.

Cada ramo dia-colab já sai integralmente em `alcance_dias` usando `limite=sys.maxsize`: `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`. A função `_listar` recebe exatamente esses dados. `app/chamados/services/vigia_fio.py:627`.

Para instrumentar diretamente o ramo, `sys.setprofile` em retorno de `_listar`, verificando módulo e chamador `censo_alcance`, permite ler os argumentos reais; o frame pai também contém `porta` e `ps` para seus PKs. Nenhum predicado precisa ser refeito. `app/chamados/services/vigia_fio.py:627`, `app/chamados/services/vigia_fio.py:644`.

Os totais empresa/competência são redução das linhas reais `rota == "sem_porta"`; o incremento original acontece imediatamente antes de `_listar`. `app/chamados/services/vigia_fio.py:704`.

Universos:

- Folha: `classificar_export`; somente retidos `furo_espelho` recebem dias/fio. `app/folha/fora_da_folha.py:129`.
- Retrato: todos os itens do mesmo `fora_da_folha(..., com_itens=True)`, uma linha por pessoa. `app/folha/retrato_folha.py:1158`.
- Pautas: empresas ativas, colaboradores ativos, células `discordante` dentro da janela de fechamento. `app/ponto/services/pautas_dos_discordantes.py:66`.
- Beco: colaboradores ativos com célula `indefinida`, protesto `PREVISTO_COM_DIA_INDEFINIDO`, dentro das janelas recebidas; depois os demais juízes reais filtram. `app/escala/services/cadastro_realidade.py:476`.
- Beco corrente: janelas de `competencias_a_lavrar`, cortadas em ontem. `app/escala/services/cadastro_realidade.py:575`.

Escritores com alternativa segura:

- `lavrar_retrato_folha`: `apply=False`, ou chamar `montar_linhas` e `contadores` numa instância não salva de `RetratoFolha`. `app/folha/management/commands/lavrar_retrato_folha.py:67`.
- `pautas_discordantes`: `Command.apurar(...)`, depois `sincronizar(apurado, apply=False)`; retorna planos de abrir/fechar sem escrever. `app/ponto/management/commands/pautas_discordantes.py:56`, `app/ponto/services/pautas_dos_discordantes.py:155`.

Projeções: remover `nome`, `destrava`, `detalhe`, `codigo_dominio` e `codigo`; conservar IDs, datas, rótulos de decisão e números. `app/folha/fora_da_folha.py:195`, `app/folha/fora_da_folha.py:245`, `app/folha/retrato_folha.py:1180`, `app/escala/services/cadastro_realidade.py:540`.

## 12/11 23:20 · sessao 1587c185a14e · - · - · origem=Ronald (tmux)

Conferido por leitura; nenhuma sonda executada. [nao verificado]

| Leitor | Assinatura | BASE → CÓPIA |
|---|---|---|
| fio | `montar_chamados_fio(colaborador, so_janela=False, com_explicacao=False)` | `app/chamados/services/fio.py:163` → `:190` |
| explicador | `por_que_nao_fecha(disputa, perguntas=None)` | `app/chamados/services/explicador.py:108` → `:108` |
| raiox | `creditos_perguntas(vivo)` | `app/chamados/services/raiox.py:51` → `:51` |
| relatório | `classificar(disputa)` | `app/chamados/relatorios_disputa.py:11` → `:11` |
| relatório global | `coletar()` | `app/chamados/relatorios_disputa.py:26` → `:31` |
| cobrança | `_calados(agora=None)` | `app/chamados/services/cobranca.py:221` → `:221` |
| pauta cobrança | `linhas_para_a_pauta(hoje=None)` | `app/chamados/services/cobranca.py:251` → `:260` |

Referências BASE derivadas exclusivamente dos deslocamentos do diff anexado; não existe `.git` disponível nesta cópia. [nao verificado]

O fio tem três chamadas reais distintas, que devem ser medidas separadamente: aplicativo com `so_janela=True`, administração com parâmetros padrão e modal administrativo com `com_explicacao=True`. Fontes: `app/chamados/views.py:569`, `:677`, `:889`. A função devolve lista anotada, permitindo registrar por chamado os flags `todas_respondidas`, `pode_solicitar_correcao`, as perguntas participantes e a explicação efetivamente anexada. Fontes: `app/chamados/services/fio.py:375`, `:455`.

A explicação tem textos com pergunta e resposta; projeção segura deve conservar `pode_fechar`, IDs, `estado` e `dono`, e converter os campos textuais em SHA256. Fontes: `app/chamados/services/explicador.py:71`, `:79`, `:173`. Há captura interna da explicação, que vira `None`; um wrapper observador de `por_que_nao_fecha` deve registrar exceção e relançá-la para preservar comportamento. Fonte: `app/chamados/services/fio.py:335`.

`creditos_perguntas` recebe queryset de chamados vivos; seu consumidor `particionar_verbos` passa o mesmo queryset, e `alimentar_fila` usa `qs_vivos()` global. Fontes: `app/chamados/services/verbos.py:30`, `:34`, `app/chamados/juizes.py:1558`. Chamar com `painel.qs_vivos()` preserva o universo global; restringir aos colaboradores da janela seria uma sonda adicional. Fonte: `app/chamados/services/painel.py:135`. Seu retorno contém dois conjuntos de chamados, permitindo registrar, para todo chamado do universo, pertença a cada conjunto. Fonte: `app/chamados/services/raiox.py:155`.

`relatorios_disputa.coletar()` percorre todas as disputas abertas, com perguntas pré-carregadas, e retorna categorias por disputa; o nome deve ser removido antes da saída. Fontes: `app/chamados/relatorios_disputa.py:47`, `:55`. A versão `classificar(disputa)` pode ser medida adicionalmente sobre todas as disputas dos colaboradores selecionados, deixando esse universo adicional explícito. Fonte: `app/chamados/relatorios_disputa.py:11`.

`_calados()` trabalha globalmente sobre perguntas selecionadas por `q_vivas()` em disputas abertas; retorna somente dois números, sem lista dos colaboradores contados. Fontes: `app/chamados/services/cobranca.py:235`, `:257`. Para conservar o cálculo real e atribuir seu percurso às pessoas, observar chamadas a `tem_canal`, `_chamados_pais` e `prazo_estourou`, preservando argumentos e retorno; o conjunto final não é exposto pela função. Fontes: `app/chamados/services/cobranca.py:249`, `:251`, `:254`.

O GET da API escreve: chama `nota.marcar_lidas`, que executa `update()`. Portanto deve constar como não coberto integralmente, sem DRY existente; alternativa próxima é o fio com `so_janela=True`, reconhecendo que o universo da API usa `consulta_do_app` e não é idêntico. Fontes: `app/api/views.py:989`, `:1039`, `app/chamados/services/nota.py:126`, `:132`, `app/chamados/views.py:569`.

A ação de recusa também escreve e não apresenta DRY; a decisão `fora_do_fato()` é a leitura mais próxima, sem medir o resultado completo da ação. Fontes: `app/chamados/services/validacao.py:535`, `:582`, `:592`, `app/chamados/services/acoes_disputa.py:79`.

`checar_cobranca(colaborador=...)` declara DRY explícito e pode substituir a chamada escritora `cobrar_colaborador`, embora esta não seja um leitor alterado no diff. Fontes: `app/chamados/services/cobranca.py:75`, `:140`.

## 12/11 22:30 · sessao 613fa5a43db3 · TIPO-NA-FONTE · construtor · origem=Code

A [tabela individual das falhas → classe → cura](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_na_fonte_volta4b.txt:5) contabiliza os **82 FAIL e 3 ERROR** do anexo. Nenhum caso P foi demonstrado nesses caminhos; as atas incompletas vieram das fixtures ([análise](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_na_fonte_volta4b.txt:57)).

Produção alterada:

| Arquivo::função | Cura |
|---|---|
| [cartorio.py::insumos_da_ata](/home/ronald/wt-codex-tipo-na-fonte-184856/app/ponto/services/cartorio.py:840) | `NAO_LAVRADA → (False, False)`, como a base. |
| [leitor_celula.py::_vazia](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/services/leitor_celula.py:174) | Agregada declara `instante=None`. |
| [calendario.py::contexto_calendario](/home/ronald/wt-codex-tipo-na-fonte-184856/app/colaboradores/services/calendario.py:298) | Inicializa `fora_map` antes do `try`. |

O [RED reproduzível](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_na_fonte_volta4b_evidencias.txt:6) morde a volta 4. **69 testes isolados passaram**; os [comandos de cada mutante e seus resultados](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_na_fonte_volta4b_evidencias.txt:68) estão registrados. [nao verificado]

Os 14 módulos completos pararam por falta de Django ([evidências](/home/ronald/wt-codex-tipo-na-fonte-184856/app/escala/tests/tipo_na_fonte_volta4b_evidencias.txt:135)). Suíte integral e DIFF da frota: **[não verificado]**. [nao verificado]

## 12/11 22:25 · sessao 4e3b7fd18bef · - · - · origem=Ronald (tmux)

```python
# Sonda fornecida como TEXTO; execução e paridade: [nao verificado].
# B = 78994522d; C = 69a3d1570.
# Django configurado e tenant juliani ativo, conforme premissa fornecida.
#
# LIMITES EXPLÍCITOS:
# - APIs agregadas ficam em unidade="janela"; não inventam resultado por célula.
# - Funções ausentes na BASE ficam indisponíveis; seus pais comuns são medidos.
# - classificar_orfas é uma sonda suplementar; chamador de produção: [nao verificado].
# - Calendário: janelas enumeradas abaixo, não histórico desconhecido de GETs.
#
# Não executar reconciliar_colab:
# B ponto/services/cluster_espurio.py:176; C :185.
# Alternativa real de leitura: pendencias, B :153; C :162.

import sys
sys.dont_write_bytecode = True

import collections
import copy
import dataclasses
import datetime as dt
import decimal
import enum
import functools
import hashlib
import inspect
import io
import json
import logging
import math
import os
import re
import uuid
from collections.abc import Mapping
from contextlib import ExitStack, redirect_stdout, redirect_stderr

from django.db import connection, connections, transaction
from django.db.models import Model, QuerySet
from django.db.models.fields.files import FieldFile
from django.utils import timezone
from django.utils.functional import Promise
from freezegun import freeze_time
import freezegun.api as fg

# Instante idêntico nas duas execuções.
# Calendário lê timezone.now internamente:
# B/C colaboradores/services/calendario.py:163,250.
# freezegun consta em C app/requirements.txt:43.
INSTANTE = "2026-11-12T20:48:00-03:00"
TAMANHO_BLOCO = 1000


class FimDaSonda(Exception):
    pass


class EscritaBloqueada(Exception):
    pass


class FalhaDaSonda(BaseException):
    """Não deve ser engolida pelos except Exception da aplicação."""


class Descartar(io.TextIOBase):
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


def relogio():
    # Relógio real, mesmo dentro do freeze_time.
    return fg.real_perf_counter()


def canon(valor, pilha=None):
    """Representação sem endereço de memória; não consulta relações lazy."""
    if pilha is None:
        pilha = set()

    if valor is None or type(valor) in (bool, int, str):
        return valor
    if isinstance(valor, decimal.Decimal):
        return str(valor)
    if isinstance(valor, (dt.datetime, dt.date, dt.time)):
        return valor.isoformat()
    if isinstance(valor, dt.timedelta):
        return ("timedelta", valor.days, valor.seconds, valor.microseconds)
    if isinstance(valor, float):
        if math.isnan(valor):
            return ("float", "nan")
        if math.isinf(valor):
            return ("float", "inf" if valor > 0 else "-inf")
        return valor
    if isinstance(valor, enum.Enum):
        return canon(valor.value, pilha)
    if isinstance(valor, uuid.UUID):
        return str(valor)
    if isinstance(valor, bytes):
        return ("bytes", valor.hex())
    if isinstance(valor, FieldFile):
        return ("arquivo", valor.name)
    if isinstance(valor, Promise):
        return str(valor)

    identidade = id(valor)
    if identidade in pilha:
        raise FalhaDaSonda("CicloNaSerializacao")
    pilha.add(identidade)
    try:
        if isinstance(valor, QuerySet):
            itens = [canon(v, pilha) for v in valor]
            return ("queryset", tuple(sorted(itens, key=repr)))

        if isinstance(valor, Model):
            # Inclui campos já carregados E atributos transitórios públicos,
            # inclusive Batida.orfa; não usa __str__ nem acessa campos deferred.
            dados = {
                k: v for k, v in vars(valor).items()
                if not k.startswith("_")
            }
            return ("model", valor._meta.label_lower, canon(dados, pilha))

        if isinstance(valor, Mapping):
            pares = [
                (canon(k, pilha), canon(v, pilha))
                for k, v in valor.items()
            ]
            return ("dict", tuple(sorted(pares, key=repr)))

        if isinstance(valor, (set, frozenset)):
            itens = [canon(v, pilha) for v in valor]
            return ("set", tuple(sorted(itens, key=repr)))

        if dataclasses.is_dataclass(valor) and not isinstance(valor, type):
            dados = {
                f.name: getattr(valor, f.name)
                for f in dataclasses.fields(valor)
            }
            return (
                "dataclass", type(valor).__module__,
                type(valor).__qualname__, canon(dados, pilha)
            )

        if isinstance(valor, list):
            return [canon(v, pilha) for v in valor]
        if isinstance(valor, tuple):
            return tuple(canon(v, pilha) for v in valor)

        if hasattr(valor, "__dict__"):
            dados = {
                k: v for k, v in vars(valor).items()
                if not k.startswith("_")
            }
            if dados:
                return (
                    "objeto", type(valor).__module__,
                    type(valor).__qualname__, canon(dados, pilha)
                )

        raise FalhaDaSonda("TipoNaoSerializavel:" + type(valor).__name__)
    finally:
        pilha.remove(identidade)


def sha(valor):
    return hashlib.sha256(repr(canon(valor)).encode("utf-8")).hexdigest()


def escalar(valor):
    return (
        valor is None
        or isinstance(
            valor,
            (bool, int, float, str, decimal.Decimal,
             dt.datetime, dt.date, dt.time)
        )
    )


def valor_histograma(valor):
    # Strings arbitrárias podem conter nomes: somente seu digest sai.
    if valor is None or type(valor) in (bool, int, float):
        return repr(canon(valor))
    if isinstance(valor, str) and re.fullmatch(
        r"EXC:[A-Za-z_][A-Za-z_0-9]*", valor
    ):
        return valor
    return "VAL_SHA256:" + sha(valor)


class Acumulador:
    def __init__(self, nome, unidade):
        self.nome = nome
        self.unidade = unidade
        self.disponivel = True
        self.chamadas = 0
        self.excecoes = collections.Counter()
        self.histograma = collections.Counter()
        self.escalares = 0
        self.segundos = 0.0
        self.blocos = []
        self.ultima_chave = None
        self._abrir()

    def _abrir(self):
        self.digest = hashlib.sha256(b"[")
        self.digest_ids = hashlib.sha256(b"[")
        self.primeiro = None
        self.ultimo = None
        self.ids = 0
        self.registros = 0

    def _fechar(self):
        if not self.registros:
            return
        self.digest.update(b"]")
        self.digest_ids.update(b"]")
        self.blocos.append({
            "primeira_chave": self.primeiro,
            "ultima_chave": self.ultimo,
            "ids_ou_chaves": self.ids,
            "chamadas": self.registros,
            "sha256_ids": self.digest_ids.hexdigest(),
            "sha256_resultados": self.digest.hexdigest(),
        })
        self._abrir()

    def adicionar(self, chave, subchave, valor, exc=None, segundos=0.0):
        chave = canon(chave)
        if self.ultima_chave is not None and chave < self.ultima_chave:
            raise FalhaDaSonda("IdsForaDeOrdem:" + self.nome)

        nova = self.ultima_chave is None or chave != self.ultima_chave
        if nova and self.ids == TAMANHO_BLOCO:
            self._fechar()

        if nova:
            if self.ids:
                self.digest_ids.update(b", ")
            self.digest_ids.update(repr(chave).encode("utf-8"))
            self.ids += 1
            if self.primeiro is None:
                self.primeiro = chave
            self.ultimo = chave

        registro = (chave, canon(subchave), canon(valor))
        if self.registros:
            self.digest.update(b", ")
        self.digest.update(repr(registro).encode("utf-8"))
        self.registros += 1
        self.chamadas += 1
        self.ultima_chave = chave
        self.segundos += segundos

        if exc:
            self.excecoes[exc] += 1
        if escalar(valor):
            self.escalares += 1
            self.histograma[valor_histograma(valor)] += 1

    def saida(self):
        self._fechar()
        dez = sorted(
            self.histograma.items(), key=lambda par: (-par[1], par[0])
        )[:10]
        return {
            "leitor": self.nome,
            "unidade": self.unidade,
            "disponivel": self.disponivel,
            "universo_chamadas": self.chamadas,
            "excecoes_por_classe": dict(sorted(self.excecoes.items())),
            "resultados_escalares": self.escalares,
            "histograma_ate_10": [
                {"valor": v, "quantidade": n} for v, n in dez
            ],
            "escalares_fora_dos_10": self.escalares - sum(n for _, n in dez),
            "blocos": self.blocos,
        }


acumuladores = {}
assinaturas = {}
contexto = None
ordinais = collections.Counter()
meta = {
    "instante": INSTANTE,
    "prova_completa": False,
    "tentativas_de_escrita": 0,
    "consultas_outro_alias": 0,
    "ausentes_na_arvore": [],
    "estimativas_segundos": {},
}
inicio_total = relogio()


def acum(nome, unidade):
    return acumuladores.setdefault(
        (nome, unidade), Acumulador(nome, unidade)
    )


def indisponivel(nome, unidade):
    acum(nome, unidade).disponivel = False
    meta["ausentes_na_arvore"].append(nome)


def registrar_assinatura(nome, funcao):
    assinatura = inspect.signature(funcao)
    assinaturas[nome] = str(assinatura)
    return assinatura


def chamar(funcao, *args, **kwargs):
    # Erro de assinatura é falha da SONDA, não resultado do leitor.
    try:
        inspect.signature(funcao).bind(*args, **kwargs)
    except TypeError:
        raise FalhaDaSonda("AssinaturaIncompativel:" + funcao.__name__)
    return funcao(*args, **kwargs)


def medir(nome, unidade, chave, subchave, funcao, *args, **kwargs):
    global contexto
    anterior = contexto
    contexto = (unidade, chave, subchave)
    t0 = relogio()
    try:
        try:
            # Savepoint mantém a transação externa utilizável após erro SQL.
            with transaction.atomic():
                resultado = chamar(funcao, *args, **kwargs)
        except Exception as erro:
            classe = type(erro).__name__
            acum(nome, unidade).adicionar(
                chave, subchave, "EXC:" + classe,
                exc=classe, segundos=relogio() - t0
            )
            return False, None
        acum(nome, unidade).adicionar(
            chave, subchave, resultado, segundos=relogio() - t0
        )
        return True, resultado
    finally:
        contexto = anterior


def observar(pilha, modulo, atributo, nome, projecao=None):
    """Envolve e chama a função REAL; preserva argumentos, retorno e exceção."""
    if not hasattr(modulo, atributo):
        indisponivel(nome, "colab")
        return

    original = getattr(modulo, atributo)
    assinatura = registrar_assinatura(nome, original)
    acum(nome, "colab")

    @functools.wraps(original)
    def envoltoria(*args, **kwargs):
        if contexto is None:
            raise FalhaDaSonda("ChamadaSemContexto:" + nome)
        try:
            ligados = assinatura.bind(*args, **kwargs)
        except TypeError:
            raise FalhaDaSonda("AssinaturaIncompativel:" + nome)

        unidade, chave, subchave = contexto
        indice = ordinais[(nome, unidade, chave, subchave)]
        ordinais[(nome, unidade, chave, subchave)] += 1
        t0 = relogio()
        try:
            resultado = original(*args, **kwargs)
        except Exception as erro:
            classe = type(erro).__name__
            acum(nome, unidade).adicionar(
                chave, (subchave, indice), "EXC:" + classe,
                exc=classe, segundos=relogio() - t0
            )
            raise

        observado = (
            projecao(ligados.arguments, resultado)
            if projecao is not None else resultado
        )
        acum(nome, unidade).adicionar(
            chave, (subchave, indice), observado,
            segundos=relogio() - t0
        )
        return resultado

    setattr(modulo, atributo, envoltoria)
    pilha.callback(setattr, modulo, atributo, original)


def guardar_sql(execute, sql, params, many, context):
    if context["connection"].alias != "default":
        meta["consultas_outro_alias"] += 1
        raise EscritaBloqueada("OutroAlias")

    texto = " ".join(sql.lstrip().upper().split())
    permitido = (
        texto.startswith(("SELECT ", "WITH ", "SAVEPOINT ", "RELEASE "))
        or texto.startswith("ROLLBACK TO ")
        or texto.startswith("SET SEARCH_PATH")
        or texto.startswith("SET LOCAL SEARCH_PATH")
    )
    if not permitido:
        meta["tentativas_de_escrita"] += 1
        raise EscritaBloqueada("InstrucaoBloqueada")

    # READ ONLY do PostgreSQL também protege contra escrita em CTE/função SQL.
    return execute(sql, params, many, context)


def comando_capturado(classe, metodo, *args, **kwargs):
    saida, erros = io.StringIO(), io.StringIO()
    comando = classe(stdout=saida, stderr=erros, no_color=True)
    retorno = chamar(getattr(comando, metodo), *args, **kwargs)
    # Texto pode conter nomes: participa apenas do hash, nunca é impresso.
    return {
        "retorno": retorno,
        "stdout": saida.getvalue(),
        "stderr": erros.getvalue(),
    }


def copia(objeto):
    # Cada leitor recebe uma instância separada; memos não viajam entre leitores.
    return copy.deepcopy(objeto)


def estado_bruto(celula, hoje):
    """Somente censo de formato; não substitui função de produção."""
    ata = celula.ata
    if ata is None:
        tempo = (
            "passado" if celula.data < hoje
            else "hoje" if celula.data == hoje else "futuro"
        )
        return "ata_nula_" + tempo

    if not isinstance(ata, Mapping) or not isinstance(ata.get("orfas"), list):
        return "lavrada_outros"

    valores = ata["orfas"]
    if not valores:
        # [] não permite descobrir se sua origem temporal era ISO ou HH:MM.
        return "lavrada_lista_vazia"
    if all(
        isinstance(v, str)
        and re.fullmatch(r"(?:[01]\d|2[0-3]):[0-5]\d", v)
        for v in valores
    ):
        return "lavrada_HHMM"

    try:
        iso = all(
            isinstance(v, str) and "T" in v
            and dt.datetime.fromisoformat(v).utcoffset() is not None
            for v in valores
        )
    except (TypeError, ValueError):
        iso = False
    return "lavrada_ISO" if iso else "lavrada_outros"


def ordenar_tripwire(resultado):
    achados, universo = resultado
    return sorted(achados, key=lambda v: repr(canon(v))), universo


def ordenar_censo(resultado):
    resultado = dict(resultado)
    resultado["casos"] = sorted(
        resultado["casos"], key=lambda v: repr(canon(v))
    )
    return resultado


try:
    if connection.in_atomic_block:
        raise FalhaDaSonda("ExigeTransacaoExternaPropria")

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )

        with ExitStack() as pilha:
            nivel_log = logging.root.manager.disable
            logging.disable(sys.maxsize)
            pilha.callback(logging.disable, nivel_log)
            pilha.enter_context(redirect_stdout(Descartar()))
            pilha.enter_context(redirect_stderr(Descartar()))
            pilha.enter_context(freeze_time(INSTANTE))
            pilha.enter_context(timezone.override("America/Sao_Paulo"))

            for alias in connections:
                pilha.enter_context(
                    connections[alias].execute_wrapper(guardar_sql)
                )

            if connection.vendor != "postgresql":
                raise FalhaDaSonda("ExigePostgreSQL")
            if getattr(connection, "schema_name", None) != "juliani":
                raise FalhaDaSonda("TenantIncorreto")

            cpus = sorted(os.sched_getaffinity(0))
            if len(cpus) != 4:
                raise FalhaDaSonda("ExigeCpusetDe4VCPU")
            meta["vcpus"] = len(cpus)

            from colaboradores.models import Colaborador, Empresa
            from escala.models import CelulaDia, EscalaColaborador
            from escala.services import leitor_celula as lc
            from escala import orfa_classe
            from ponto import juiz_batida, propositor, turnos
            from ponto.services import cluster_espurio, esmeril_espelho
            from ponto.services import vigia_pareador_celula
            from ponto.management.commands import tripwire_celulas
            from ponto.management.commands.propor_reorganizacao import (
                Command as Propor,
            )
            from ponto.management.commands.detectar_cluster_espurio import (
                Command as Detectar,
            )
            from chamados import juizes
            from chamados.models import PerguntaDisputa
            from chamados.services import materializacao
            from relatorios import pdf_espelho
            from colaboradores.services.calendario import contexto_calendario
            from ponto.janelas import janela_atual

            agora = timezone.now()
            hoje = timezone.localdate(agora)

            funcoes = {
                "grade_da_celula": lc.grade_da_celula,
                "periodos_do_dia": juiz_batida.periodos_do_dia,
                "_leitura_da_celula": cluster_espurio._leitura_da_celula,
                "assinaturas_do_colab": esmeril_espelho.assinaturas_do_colab,
                "janela_esmeril": esmeril_espelho.janela,
                "censo": vigia_pareador_celula.censo,
                "medir_sobra_com_tipo_trocado":
                    tripwire_celulas.medir_sobra_com_tipo_trocado,
                "_celula_furada": juizes._celula_furada,
                "_plantio_preenche_furo_na_data":
                    materializacao._plantio_preenche_furo_na_data,
                "_ts_tipo_do_plantio": materializacao._ts_tipo_do_plantio,
                "estado_aplicacao_pergunta":
                    materializacao.estado_aplicacao_pergunta,
                "pendencias": cluster_espurio.pendencias,
                "reconciliar_colab_NAO_EXECUTAR":
                    cluster_espurio.reconciliar_colab,
                "contexto_calendario": contexto_calendario,
                "_coletar_dados_espelho_mes":
                    pdf_espelho._coletar_dados_espelho_mes,
                "propor_reorganizacao._run": Propor._run,
                "detectar_cluster_espurio.handle": Detectar.handle,
                "classificar_orfas": orfa_classe.classificar_orfas,
            }
            for nome, funcao in funcoes.items():
                registrar_assinatura(nome, funcao)

            # Mudança explícita da porta e de seu argumento:
            # B escala/services/leitor_celula.py:180,379,420;
            # C escala/services/leitor_celula.py:183,428.
            if hasattr(lc, "orfas_da_ata"):
                ler_orfas = lc.orfas_da_ata
                parametro_esperado = ("celula",)
                usa_celula = True
            else:
                ler_orfas = lc._orfas_da_ata
                parametro_esperado = ("ata",)
                usa_celula = False

            assinatura = registrar_assinatura("orfas_da_ata", ler_orfas)
            if tuple(assinatura.parameters) != parametro_esperado:
                raise FalhaDaSonda("PortaDeOrfasDesconhecida")

            tem_contagem = hasattr(lc, "contagem_orfas_da_ata")
            if tem_contagem:
                registrar_assinatura(
                    "contagem_orfas_da_ata", lc.contagem_orfas_da_ata
                )
            else:
                indisponivel("contagem_orfas_da_ata", "celula")

            # Observam chamadas reais feitas pelos pais, com seus argumentos.
            # Propositor: C ponto/management/commands/propor_reorganizacao.py:71,91.
            observar(
             

[truncado em 20000 de 44760 caracteres]

## 12/11 22:25 · sessao 424e40a0f738 · BUG-CADASTRO-C5E · verificador · origem=Code

REPROVA

1. **A classe do bloqueante anterior continua aberta: leituras novas ainda deixam a catraca verde.** `_atribuir` visita o alvo e apaga bindings antes de resolver novamente o valor; `_e_none` não reconhece walrus; e a anotação sem valor apaga um binding que deveria preservar. (`app/ponto/tests/test_cadastro_futuro_c5e.py:478`, `app/ponto/tests/test_cadastro_futuro_c5e.py:443`, `app/ponto/tests/test_cadastro_futuro_c5e.py:497`, `app/ponto/tests/test_cadastro_futuro_c5e.py:492`)

   Construí estes **três mutantes novos**, inseridos individualmente antes da leitura original de `reconciliar_apos_vinculo`. São sequências diretas, fora dos limites declarados de fluxo, defaults, retornos e transporte de alias. (`app/colaboradores/services/pos_vinculo.py:56`)

   | Mutante novo | Por que escapa |
   |---|---|
   | `_ler = _tz.localdate; _ler = _ler; _ler()` | Visitar o segundo alvo apaga `_ler`; `_ligar` já não encontra a função no valor. (`app/ponto/tests/test_cadastro_futuro_c5e.py:481`, `app/ponto/tests/test_cadastro_futuro_c5e.py:461`) |
   | `_tz.localdate(_instante := None)` | `_e_none` aceita somente constante ou nome; a chamada é classificada antes da visita ao argumento. (`app/ponto/tests/test_cadastro_futuro_c5e.py:497`, `app/ponto/tests/test_cadastro_futuro_c5e.py:501`) |
   | `_v = None; _v: object; _tz.localdate(_v)` | A anotação sem valor visita o alvo e esquece o `None`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:487`, `app/ponto/tests/test_cadastro_futuro_c5e.py:443`) |

   **Resultado de cada mutante:** total permaneceu 42, função permaneceu com 1, `_furos=[]` e os **23 testes C5E passaram**. Execuções em memória, sem relatório persistido. [nao verificado] [nao verificado]

   Cada inserção deveria produzir `reconciliar_apos_vinculo: 2 > teto 1`, reprovando a asserção principal; o teto dessa função continua sendo 1. (`app/ponto/tests/test_cadastro_futuro_c5e.py:338`, `app/ponto/tests/test_cadastro_futuro_c5e.py:529`, `app/ponto/tests/test_cadastro_futuro_c5e.py:533`)

2. **O visitante novo também rejeita código legítimo.** Dentro de uma função, `_v: _tz.localdate()` passa a ser contado pela visita incondicional à anotação. (`app/ponto/tests/test_cadastro_futuro_c5e.py:488`)

   No ensaio, Python não executou essa anotação local: zero chamadas runtime. Inserida em `reconciliar_apos_vinculo`, porém, ela elevou o censo para 43 e produziu `2 > teto 1`. Portanto, o selo fica vermelho sem leitura adicional de relógio. Execução em memória, sem relatório persistido. [nao verificado]

**Informativos — cura dos exemplos da volta 2**

**Os exemplos exatos foram curados; a classe não foi fechada**, pelos bloqueantes acima. A cobertura acrescentada é esta: (`app/ponto/tests/test_cadastro_futuro_c5e.py:632`, `app/ponto/tests/test_cadastro_futuro_c5e.py:642`, `app/ponto/tests/test_cadastro_futuro_c5e.py:657`)

| Forma anterior | Implementação atual | Metateste/mutante que morde |
|---|---|---|
| Leitura no alvo de `Assign` | `_atribuir` percorre o alvo. (`app/ponto/tests/test_cadastro_futuro_c5e.py:481`) | `_cache[_tz.localdate()] = 1`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:634`) |
| Leitura no alvo de `AnnAssign` | Delega a `_atribuir`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:490`) | `_cache[_tz.localdate()]: int = 1`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:635`) |
| Leitura no alvo de `AugAssign` | Permanece na travessia herdada de `ast.NodeVisitor`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:401`) | `_cache[_tz.localdate()] += 1`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:636`) |
| Walrus que fornece a função chamada | `_atributo` resolve `NamedExpr`; a atribuição tem visitante próprio. (`app/ponto/tests/test_cadastro_futuro_c5e.py:410`, `app/ponto/tests/test_cadastro_futuro_c5e.py:494`) | `(_ler := _tz.localdate)()`. **Não cobre walrus no argumento**, mostrado no bloqueante 1. (`app/ponto/tests/test_cadastro_futuro_c5e.py:649`) |
| Alias por desempacotamento | `_ligar` associa elementos de tupla/lista; formas não casadas recebem contagem conservadora. (`app/ponto/tests/test_cadastro_futuro_c5e.py:466`, `app/ponto/tests/test_cadastro_futuro_c5e.py:472`) | Tupla, lista, estrutura aninhada e starred. (`app/ponto/tests/test_cadastro_futuro_c5e.py:645`, `app/ponto/tests/test_cadastro_futuro_c5e.py:651`) |
| `None` ligado diretamente a nome | `_ligar` registra a constante; `_e_none` consulta o nome. (`app/ponto/tests/test_cadastro_futuro_c5e.py:464`, `app/ponto/tests/test_cadastro_futuro_c5e.py:497`) | Atribuição simples, anotada, desempacotamento e chamada por alias. (`app/ponto/tests/test_cadastro_futuro_c5e.py:659`) |

Esses metatestes exigem expressamente o furo `2 > teto 1`, substituindo a fonte produtiva somente em memória. (`app/ponto/tests/test_cadastro_futuro_c5e.py:621`, `app/ponto/tests/test_cadastro_futuro_c5e.py:629`)

**Contagem da árvore real**

O teto continua em **42**, e o teste exige igualdade integral entre o censo e o mapa por função. (`app/ponto/tests/test_cadastro_futuro_c5e.py:372`, `app/ponto/tests/test_cadastro_futuro_c5e.py:535`, `app/ponto/tests/test_cadastro_futuro_c5e.py:537`)

Comparei o visitante anterior reconstruído em memória com o atual: ambos produziram **42 leituras em 37 funções**, com os mesmos achados `(função, linha, atributo)` em todo o universo. **Não observei leitura existente duplicada nem perdida pelo delta.** Resultado efêmero, sem relatório persistido. [nao verificado]

Como conferência pontual, permanecem uma leitura no pós-vínculo, dois carimbos na regeneração e três leituras em `julgar_celula`; a conversão externa em `localtime(now())` não acrescenta uma quarta leitura. (`app/colaboradores/services/pos_vinculo.py:56`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:308`, `app/ponto/services/cartorio.py:2016`, `app/ponto/services/cartorio.py:2037`, `app/ponto/services/cartorio.py:2040`, `app/ponto/tests/test_cadastro_futuro_c5e.py:503`)

**Rebindings, escopos e falsos positivos**

- **Rebind simples funciona:** `_v=None; _v=instante; _tz.localtime(_v)` tem expectativa de zero leituras no metateste. (`app/ponto/tests/test_cadastro_futuro_c5e.py:672`, `app/ponto/tests/test_cadastro_futuro_c5e.py:678`)
- **Comprehension tem erro de escopo:** não recebe o tratamento de escopo reservado a funções/classes, e seus nomes passam pelo apagamento genérico. (`app/ponto/tests/test_cadastro_futuro_c5e.py:441`, `app/ponto/tests/test_cadastro_futuro_c5e.py:443`)
- `_v=None; [_tz.localdate(_v) for _v in instantes]`, com instantes válidos, contou uma leitura indevida; o ensaio runtime fez somente conversão. Na direção inversa, `_v=None; [_v for _v in ()]; _tz.localdate(_v)` escondeu uma leitura real porque o visitante esqueceu o `_v` externo. Resultados em memória. [nao verificado]
- **Lambda também não ganha escopo próprio:** o tratamento explícito cobre `FunctionDef`, `AsyncFunctionDef` e `ClassDef`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:441`) `_v=None; (lambda _v: _tz.localdate(_v))(instante)` produziu falso positivo no ensaio. [nao verificado]
- **Closures, `global` e `nonlocal` não têm propagação de bindings entre chamadas:** o visitante copia e depois restaura o estado ao visitar a definição. (`app/ponto/tests/test_cadastro_futuro_c5e.py:430`, `app/ponto/tests/test_cadastro_futuro_c5e.py:440`) Helpers que trocam `None` por um instante antes da conversão produziram falsos positivos; por envolverem fluxo entre funções, mantenho esses exemplos como informativos dentro da limitação declarada. [nao verificado]
- **Redefinir um alias com `def`, classe ou import pode conservar o alias antigo:** `_escopo` não invalida o nome definido no escopo externo, e o apagamento depende de `ast.Name`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:430`, `app/ponto/tests/test_cadastro_futuro_c5e.py:443`) Atribuir esse defeito especificamente ao último delta permanece [nao verificado].

**Quem escreve e quem lê os três símbolos**

| Símbolo | Quem escreve | Quem lê e quais selos dependem |
|---|---|---|
| `TETO_RELOGIO` | Literal de módulo. (`app/ponto/tests/test_cadastro_futuro_c5e.py:333`) | `_furos`; soma e igualdade integral do teste principal; metatestes indiretamente por `_furos`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:528`, `app/ponto/tests/test_cadastro_futuro_c5e.py:535`, `app/ponto/tests/test_cadastro_futuro_c5e.py:537`, `app/ponto/tests/test_cadastro_futuro_c5e.py:629`) |
| `TOTAL_RELOGIO` | Literal `42`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:372`) | Limite da soma medida e conferência da soma dos tetos. (`app/ponto/tests/test_cadastro_futuro_c5e.py:534`, `app/ponto/tests/test_cadastro_futuro_c5e.py:535`) |
| `leituras_de_relogio` | Definição da função. (`app/ponto/tests/test_cadastro_futuro_c5e.py:383`) | `censo_relogio`; selo de portas sem leitura de dia; selos dos serviços/auxiliares sem relógio; metatestes de conversão, rebinding e não duplicação. (`app/ponto/tests/test_cadastro_futuro_c5e.py:520`, `app/ponto/tests/test_cadastro_futuro_c5e.py:543`, `app/ponto/tests/test_cadastro_futuro_c5e.py:546`, `app/ponto/tests/test_cadastro_futuro_c5e.py:550`, `app/ponto/tests/test_cadastro_futuro_c5e.py:617`, `app/ponto/tests/test_cadastro_futuro_c5e.py:681`) |

A busca na cópia encontrou esses três nomes somente nesse arquivo, sem outro binding ou dependência textual externa. Resultado de busca sem relatório persistido. [nao verificado]

**Classes de reprovação e restante da obra**

- **Selo/RED sem mordida: presente**, pelo bloqueante 1; a promessa continua sendo denunciar qualquer leitura adicional dentro do universo. (`app/ponto/tests/test_cadastro_futuro_c5e.py:318`)
- **Segundo juiz, derivação paralela e segundo escritor fora da porta:** não identifiquei outro caso concreto nos caminhos inspecionados. O suplemento consulta `vinculos_dos_dias` e delega a regeneração; a limpeza do julgamento também passa pela porta. (`app/ponto/services/cadastro_futuro.py:38`, `app/ponto/services/cadastro_futuro.py:44`, `app/ponto/portas/celula.py:314`, `app/ponto/portas/celula.py:1167`)
- **Ordem de trava:** o snapshot mantém vínculos antes de células, com selo exigindo `E → C`. Concorrência real não foi demonstrada por esse teste sem banco. (`app/colaboradores/services/vinculo.py:1018`, `app/colaboradores/services/vinculo.py:1023`, `app/ponto/tests/test_cadastro_futuro_c3.py:242`, `app/ponto/tests/test_cadastro_futuro_c3.py:3`)
- **Relógio interior:** permanece como passivo autorizado. Não o transformo em bloqueante nem proponho tornar `hoje` obrigatório nesses sítios; o próprio selo registra a proibição de ampliar esta cura. (`app/ponto/tests/test_cadastro_futuro_c5e.py:316`)
- **Campo com dois significados:** não encontrei ocorrência nova nos valores examinados: `_hoje` é separado do dia de efeito e do carimbo `regenerada_em`. (`app/colaboradores/services/vinculo.py:192`, `app/colaboradores/services/vinculo.py:209`, `app/ponto/portas/celula.py:263`)
- **Comentário inexato:** `executar_vinculo` promete transmitir o mesmo dia ao pós-vínculo, mas essa chamada não passa `hoje`, e o destinatário lê o relógio. Corrigir a promessa documental não exige ampliar aos interiores. (`app/colaboradores/services/vinculo.py:189`, `app/colaboradores/services/vinculo.py:345`, `app/colaboradores/services/pos_vinculo.py:56`)
- **Triagem de `reparar` confirmada:** remover sua guarda faz a recusa chegar por `_trabalhar_o_par`; a regex exige a origem `reparar`. O mutante já alcança `classificar`, mas a delegada impede o plantio. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:45`, `app/ponto/tests/test_cadastro_futuro_c5e.py:231`, `app/chamados/services/passivo_validado.py:327`, `app/chamados/services/passivo_validado.py:375`, `app/chamados/services/passivo_validado.py:430`)
- **Validação executada:** C5E + sem banco, **66/66**; C3, C3_folgas, C4, C5, C5b e C5d, **83/83**. Execuções sem artefato persistido. [nao verificado]
- **Limites da conclusão:** não houve acesso a banco/log nem execução da suíte com banco; implantação e pouso efetivos permanecem [nao verificado]. Sem histórico Git disponível nesta cópia, a comparação integral e a atribuição de outros defeitos ao intervalo `8dc931da9..4b34ed21a` permanecem [nao verificado].

Não discordo dos limites declarados como limites desta pré-verificação; **os bloqueantes encontrados independem deles** e ficam no próprio mecanismo da catraca. (`app/ponto/tests/test_cadastro_futuro_c5e.py:478`, `app/ponto/tests/test_cadastro_futuro_c5e.py:488`, `app/ponto/tests/test_cadastro_futuro_c5e.py:497`)

## 12/11 22:23 · sessao 0c5331b82277 · TIPO-NA-FONTE · leitura · origem=Code

As funções novas não têm chamada direta equivalente na base; `reconciliar_colab` escreve. O script distingue essas lacunas da comparação pelos chamadores comuns, sem recriar a lógica antiga (app/ponto/propositor.py:88; app/relatorios/pdf_espelho.py:186; app/ponto/services/cluster_espurio.py:185,199,205).

```python
# SOMENTE TEXTO: não executado nesta revisão. [nao verificado]
#
# Django configurado; tenant juliani ativo; executar o MESMO texto nas duas
# árvores, contra a MESMA sombra imutável. Imutabilidade da sombra:
# [nao verificado].
#
# Não renderiza PDF, não materializa pergunta, não reconcilia chamado.
# Fontes:
#   app/relatorios/pdf_espelho.py:236
#   app/chamados/services/materializacao.py:437
#   app/ponto/services/cluster_espurio.py:162,185
#
# Comparar os blocos LEITOR e UNIVERSO. TEMPOS e ASSINATURAS são diagnóstico.
# Hashes de adaptadores novos não possuem equivalente direto na BASE:
# comparar também os respectivos chamadores comuns.
#
# O script não declara aprovação automática: igualdade de EXC não é prova
# de leitura bem-sucedida; há capturas internas:
# app/chamados/juizes.py:525
# app/chamados/services/materializacao.py:191

import sys
sys.dont_write_bytecode = True

import collections
import contextlib
import copy
import dataclasses
import datetime as dt
import decimal
import enum
import functools
import hashlib
import importlib
import inspect
import io
import json
import logging
import os
import re
import time
import uuid

from unittest.mock import patch
from zoneinfo import ZoneInfo

# Corte de ensaio deliberadamente fixo; não afirma horário de execução.
FIXO = dt.datetime.fromisoformat("2026-11-12T23:48:00+00:00")
ZONA = ZoneInfo("America/Sao_Paulo")
HOJE = FIXO.astimezone(ZONA).date()
DIA = dt.timedelta(days=1)
BLOCO = 1000
REAL_DATE, REAL_DATETIME = dt.date, dt.datetime

SAIDA = {}
ASSINATURAS = {}
AUSENTES = []
TEMPOS = {}
SQL_ERROS = collections.Counter()
ATUAL = None
SEQUENCIAS = collections.Counter()


class DesfazerSonda(Exception):
    pass


class FalhaDaSonda(BaseException):
    """Não pode ser engolida por except Exception dos leitores."""


class AssinaturaInesperada(FalhaDaSonda):
    pass


class EscritaBloqueada(FalhaDaSonda):
    pass


class CanonicoIncompleto(FalhaDaSonda):
    pass


class OrdemDeIdsInvalida(FalhaDaSonda):
    pass


class Sink:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass

    def isatty(self):
        return False


# Preserva isinstance() para datas reais já carregadas pelo ORM.
class MetaRelogio(type):
    def __instancecheck__(cls, obj):
        return isinstance(obj, cls._real)


class DataFixa(REAL_DATE, metaclass=MetaRelogio):
    _real = REAL_DATE

    @classmethod
    def today(cls):
        return HOJE


class InstanteFixo(REAL_DATETIME, metaclass=MetaRelogio):
    _real = REAL_DATETIME

    @classmethod
    def now(cls, tz=None):
        if tz is None:
            return FIXO.astimezone(ZONA).replace(tzinfo=None)
        return FIXO.astimezone(tz)

    @classmethod
    def today(cls):
        return cls.now()

    @classmethod
    def utcnow(cls):
        return FIXO.astimezone(dt.timezone.utc).replace(tzinfo=None)


def sha(texto):
    return hashlib.sha256(texto.encode("utf-8")).hexdigest()


# Classes Django são atribuídas dentro do atomic.
Model = QuerySet = None


def canonico(valor, ativos=None):
    """Sem repr/str de modelos, endereços de memória ou consultas deferred.

    Dict/set: ordenados. List/tuple: preservam ordem semântica.
    Datetime/date/time: isoformat. Decimal: str.
    Tipos desconhecidos abortam; não viram um hash aparentemente válido.
    """
    if ativos is None:
        ativos = set()

    if valor is None or isinstance(valor, (bool, int, str)):
        return valor
    if isinstance(valor, decimal.Decimal):
        return str(valor)
    if isinstance(valor, (REAL_DATETIME, REAL_DATE, dt.time)):
        return valor.isoformat()
    if isinstance(valor, dt.timedelta):
        return ("timedelta", valor.days, valor.seconds, valor.microseconds)
    if isinstance(valor, float):
        return ("float", valor.hex())
    if isinstance(valor, uuid.UUID):
        return ("uuid", str(valor))
    if isinstance(valor, enum.Enum):
        return canonico(valor.value, ativos)
    if isinstance(valor, bytes):
        return ("bytes", valor.hex())

    identidade = id(valor)
    if identidade in ativos:
        if isinstance(valor, Model):
            return ("referencia_modelo", valor._meta.label_lower, valor.pk)
        raise CanonicoIncompleto()

    ativos.add(identidade)
    try:
        if isinstance(valor, Model):
            # Campos efetivamente carregados e atributos de apresentação
            # (inclusive Batida.orfa); não acessa relações nem __str__.
            campos = {
                k: v for k, v in vars(valor).items()
                if not k.startswith("_")
            }
            return (
                "model",
                valor._meta.label_lower,
                canonico(campos, ativos),
            )
        if isinstance(valor, QuerySet):
            itens = [canonico(x, ativos) for x in valor]
            return ("queryset", tuple(sorted(itens, key=repr)))
        if isinstance(valor, collections.abc.Mapping):
            itens = [
                (canonico(k, ativos), canonico(v, ativos))
                for k, v in valor.items()
            ]
            return ("dict", tuple(sorted(itens, key=lambda x: repr(x[0]))))
        if isinstance(valor, (set, frozenset)):
            return (
                "set",
                tuple(sorted((canonico(x, ativos) for x in valor), key=repr)),
            )
        if isinstance(valor, (list, tuple)):
            return (
                "list" if isinstance(valor, list) else "tuple",
                tuple(canonico(x, ativos) for x in valor),
            )
        if dataclasses.is_dataclass(valor) and not isinstance(valor, type):
            return canonico(
                {f.name: getattr(valor, f.name)
                 for f in dataclasses.fields(valor)},
                ativos,
            )
        if hasattr(valor, "__dict__"):
            return (
                "objeto",
                type(valor).__module__,
                type(valor).__qualname__,
                canonico(
                    {k: v for k, v in vars(valor).items()
                     if not k.startswith("_")},
                    ativos,
                ),
            )
        raise CanonicoIncompleto()
    finally:
        ativos.remove(identidade)


class Registro:
    """Hash streaming do repr de tuplas ordenadas, sem arquivo temporário.

    Cada linha é (id, eventos_ordenados).
    Cada bloco é uma tupla com até 1000 linhas de ids distintos.
    Conserva somente os eventos do id corrente, mais os hashes concluídos.
    """
    def __init__(self):
        self.chamadas = 0
        self.projecoes = 0
        self.excecoes = collections.Counter()
        self.hist = collections.Counter()
        self.escalares = 0
        self.blocos = []
        self.agregados = []
        self.ident = None
        self.eventos = []
        self.h = None
        self.n_ids = 0
        self.primeiro = self.ultimo = None

    def terminar_id(self):
        if self.ident is None:
            return
        if self.h is None:
            self.h = hashlib.sha256()
            self.h.update(b"(")
            self.primeiro = self.ident
        if self.n_ids:
            self.h.update(b", ")
        linha = (
            self.ident,
            tuple(sorted(self.eventos, key=repr)),
        )
        self.h.update(repr(linha).encode("utf-8"))
        self.n_ids += 1
        self.ultimo = self.ident
        self.eventos = []
        self.ident = None
        if self.n_ids == BLOCO:
            self.terminar_bloco()

    def terminar_bloco(self):
        if self.h is None:
            return
        if self.n_ids == 1:
            self.h.update(b",")
        self.h.update(b")")
        self.blocos.append({
            "n_ids": self.n_ids,
            "primeiro_id": self.primeiro,
            "ultimo_id": self.ultimo,
            "sha256": self.h.hexdigest(),
        })
        self.h = None
        self.n_ids = 0

    def adicionar(self, ident, contexto, valor, exc=None, projecao=False):
        if projecao:
            self.projecoes += 1
        else:
            self.chamadas += 1
            if exc:
                self.excecoes[exc] += 1
            if valor is None or isinstance(
                valor, (bool, int, float, str, decimal.Decimal)
            ):
                self.escalares += 1
                # Texto livre nunca aparece no histograma.
                if isinstance(valor, str) and not exc:
                    rotulo = "STR_SHA256:" + sha(valor)
                else:
                    rotulo = repr(canonico(valor))
                self.hist[rotulo] += 1

        evento = (canonico(contexto), canonico(valor))
        if ident is None:
            self.agregados.append({
                "contexto": contexto,
                "sha256_integral": sha(repr(evento)),
            })
            return
        if self.ident is not None and ident < self.ident:
            raise OrdemDeIdsInvalida()
        if self.ident != ident:
            self.terminar_id()
            if self.ultimo is not None and ident <= self.ultimo:
                raise OrdemDeIdsInvalida()
            self.ident = ident
        self.eventos.append(evento)

    def resultado(self):
        self.terminar_id()
        self.terminar_bloco()
        hist = sorted(self.hist.items(), key=lambda x: (-x[1], x[0]))
        return {
            "universo_chamadas": self.chamadas,
            "projecoes_de_resultado": self.projecoes,
            "excecoes_por_classe": dict(sorted(self.excecoes.items())),
            "resultados_escalares": self.escalares,
            "histograma_ate_10": hist[:10],
            "escalares_fora_do_histograma": sum(n for _, n in hist[10:]),
            "blocos": self.blocos,
            "agregados_integrais": self.agregados,
        }


REGISTROS = collections.defaultdict(Registro)


def registrar(nome, unidade, ident, fase, contexto, valor,
              exc=None, projecao=False):
    REGISTROS[(nome, unidade, fase)].adicionar(
        ident, contexto, valor, exc=exc, projecao=projecao,
    )


def conferir(funcao, alternativas):
    assinatura = inspect.signature(funcao)
    nomes = tuple(assinatura.parameters)
    chave = funcao.__module__ + "." + funcao.__qualname__
    ASSINATURAS[chave] = {
        "assinatura": str(assinatura),
        "arquivo": inspect.getsourcefile(funcao),
        "linha": inspect.getsourcelines(funcao)[1],
    }
    if nomes not in alternativas:
        raise AssinaturaInesperada()
    return funcao


def observar(stack, modulo, atributo, alternativas, apos=None):
    original = getattr(modulo, atributo, None)
    nome = modulo.__name__ + "." + atributo
    if original is None:
        AUSENTES.append(nome)
        return
    conferir(original, alternativas)
    assinatura = inspect.signature(original)

    @functools.wraps(original)
    def wrapper(*args, **kwargs):
        assinatura.bind(*args, **kwargs)
        if ATUAL is None:
            raise FalhaDaSonda()
        unidade, ident, fase, contexto = ATUAL
        chave = (nome, unidade, ident, fase, repr(contexto))
        ordem = SEQUENCIAS[chave]
        SEQUENCIAS[chave] += 1
        contexto_real = (contexto, ordem)
        if atributo == "_plantio_preenche_furo_na_data":
            ligados = assinatura.bind(*args, **kwargs).arguments
            contexto_real += (ligados["ts"].isoformat(),)
        try:
            retorno = original(*args, **kwargs)
        except Exception as erro:
            classe = type(erro).__name__
            registrar(
                "observado." + nome, unidade, ident, fase, contexto_real,
                "EXC:" + classe, exc=classe,
            )
            raise
        observado = retorno if apos is None else apos(retorno, args, kwargs)
        registrar(
            "observado." + nome, unidade, ident, fase, contexto_real, observado,
        )
        return retorno

    stack.enter_context(patch.object(modulo, atributo, wrapper))


def rodar(nome, unidade, ident, fase, contexto, funcao, *args, **kwargs):
    global ATUAL
    inspect.signature(funcao).bind(*args, **kwargs)
    anterior = ATUAL
    ATUAL = (unidade, ident, fase, contexto)
    try:
        try:
            # Savepoint permite continuar após erro de banco de UMA chamada.
            with transaction.atomic(using="default"):
                valor = funcao(*args, **kwargs)
                registrar(nome, unidade, ident, fase, contexto, valor)
            return True, valor
        except Exception as erro:
            classe = type(erro).__name__
            registrar(
                nome, unidade, ident, fase, contexto,
                "EXC:" + classe, exc=classe,
            )
            return False, None
    finally:
        ATUAL = anterior


def fase(nome, itens, quantidade, executar):
    inicio = time.perf_counter()
    amostra = min(10, quantidade)
    estimativa = None
    feitos = 0
    for item in itens:
        executar(item)
        feitos += 1
        if feitos == amostra:
            estimativa = (
                (time.perf_counter() - inicio) / feitos * quantidade
            )
    TEMPOS[nome] = {
        "unidades": feitos,
        "segundos": round(time.perf_counter() - inicio, 3),
        "projecao_segundos_pelas_primeiras_unidades": (
            None if estimativa is None else round(estimativa, 3)
        ),
        "tamanho_amostra": amostra,
    }


def comando_cluste_dry(classe, cid):
    # app/ponto/management/commands/detectar_cluster_espurio.py:65,85,87
    out, err = io.StringIO(), io.StringIO()
    comando = classe(stdout=out, stderr=err, no_color=True)
    retorno = comando.handle(
        apply=False, alarme=False, legado=False,
        dias=14, colab=cid, esperados=None,
    )
    # Conteúdo completo entra somente no hash; não é impresso.
    return retorno, out.getvalue(), err.getvalue()


def comando_propositor(classe, colab, ini, fim):
    # app/ponto/management/commands/propor_reorganizacao.py:25,65,91
    out, err = io.StringIO(), io.StringIO()
    comando = classe(stdout=out, stderr=err, no_color=True)
    retorno = comando._run({
        "empresa": colab.empresa_id,
        "colab": colab.pk,
        "ref": fim.isoformat(),
        "dias": (fim - ini).days + 1,
        "detalhe": True,
    })
    return retorno, out.getvalue(), err.getvalue()


def meses_entre(ini, fim):
    ano, mes = ini.year, ini.month
    while (ano, mes) <= (fim.year, fim.month):
        yield ano, mes
        ano, mes = (ano + 1, 1) if mes == 12 else (ano, mes + 1)


def mes_anterior(data):
    return (
        REAL_DATE(data.year - 1, 12, 1)
        if data.month == 1
        else REAL_DATE(data.year, data.month - 1, 1)
    )


def mes_seguinte(data):
    return (
        REAL_DATE(data.year + 1, 1, 1)
        if data.month == 12
        else REAL_DATE(data.year, data.month + 1, 1)
    )


def estado_bruto(cel):
    # Censo de formato, não substitui nenhum leitor.
    # Lista vazia não permite inferir o formato temporal de suas órfãs.
    if cel.ata is None:
        return (
            "ata_nula_passado" if cel.data < HOJE else
            "ata_nula_hoje" if cel.data == HOJE else
            "ata_nula_futuro"
        )
    if not isinstance(cel.ata, collections.abc.Mapping):
        return "lavrada_outro_estado"
    orfas = cel.ata.get("orfas")
    if not isinstance(orfas, list):
        return "lavrada_outro_estado"
    if not orfas:
        return "lavrada_lista_vazia"
    if all(
        isinstance(x, str)
        and re.fullmatch(r"(?:[01]\d|2[0-3]):[0-5]\d", x)
        for x in orfas
    ):
        return "lavrada_HHMM"
    for x in orfas:
        if not isinstance(x, str) or "T" not in x:
            return "lavrada_outro_estado"
        try:
            instante = REAL_DATETIME.fromisoformat(x)
        except ValueError:
            return "lavrada_outro_estado"
        if instante.utcoffset() is None:
            return "lavrada_outro_estado"
    return "lavrada_ISO"


def guarda_sql(executar, sql, params, many, contexto):
    conexao = contexto["connection"]
    if conexao.alias != "default":
        raise EscritaBloqueada()
    texto = str(sql).lstrip()
    primeira = texto.split(None, 1)[0].upper() if texto else ""
    permitido = primeira in {
        "SELECT", "WITH", "SHOW", "SAVEPOINT", "RELEASE", "ROLLBACK",
    }
    # django-tenants ajusta search_path; não libera SET arbitrário.
    permitido |= bool(re.match(
        r"SET\s+(?:LOCAL\s+)?search_path\b", texto, re.I,
    ))
    if not permitido:
        raise EscritaBloqueada()
    try:
        return executar(sql, params, many, contexto)
    except Exception as erro:
        SQL_ERROS[type(erro).__name__] += 1
        raise


def executar_sonda(stack):
    global Model, QuerySet, transaction

    from django.db import connections, transaction
    from django.db.models import Min, Max, Count, Prefetch
    from django.db.models import Model, QuerySet
    from django.utils import timezone

    conexao = connections["default"]
    if conexao.in_atomic_block:
        raise FalhaDaSonda()
    if conexao.vendor != "postgresql":
        raise FalhaDaSonda()
    if getattr(conexao, "schema_name", None) != "juliani":
        raise FalhaDaSonda()

    afinidade = sorted(os.sched_getaffinity(0))
    if len(afinidade) != 4:
        raise FalhaDaSonda()
    SAIDA["cpu"] = {"vcpus": len(afinidade), "paralelismo": 1}

    with transaction.atomic(using="default"):
        # Deve preceder a primeira consulta de dados desta transação.
        with conexao.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute("SELECT current_schema()")
            if cursor.fetchone()[0] != "juliani":
                raise FalhaDaSonda()

        for banco in connections.all():
            stack.enter_context(banco.execute_wrapper(guarda_sql))

        stack.enter_context(timezone.override(ZONA))
        stack.enter_context(patch.object(timezone, "now", lambda: FIXO))
        stack.enter_context(patch.object(dt, "date", DataFixa))
        stack.enter_context(patch.object(dt, "datetime", InstanteFixo))

        from colaboradores.models import Colaborador
        from escala.models import CelulaDia, EscalaColaborador
        from chamados.models import PerguntaDisputa
        from escala.services import leitor_celula as lc
        from escala import orfa_classe as oc
        from ponto import juiz_batida as jb, propositor as prop, turnos
        from ponto.services import cluster_espurio as cluster
        from ponto.services import esmeril_espelho as esmeril
        from ponto.services import vigia_pareador_celula as vigia
        from ponto.management.commands import tripwire_celulas as tripwire
        from chamados import juizes
        from chamados.services import materializacao as mat
        from colaboradores.services import calendario
        from relatorios import pdf_espelho as pdf
        from relatorios.cartao_pela_celula import periodo_da_competencia
        from ponto.management.commands.detectar_cluster_espurio import (
            Command as DetectarCluster,
        )
        from ponto.management.commands.propor_reorganizacao import (
            Command as ProporReorganizacao,
        )
        from core.observ import engolidos

        inicio_engolidos = engolidos()

        # Seleção explícita pela API existente e assinatura, sem tentar chamadas.
        if hasattr(lc, "orfas_da_ata"):
            conferir(lc.orfas_da_ata, {("celula",)})
            ler_orfas = lambda cel: lc.orfas_da_ata(cel)
        else:
            conferir(lc._orfas_da_ata, {("ata",)})
            # Ex

[truncado em 20000 de 41401 caracteres]

## 12/11 22:23 · sessao e0e86e4b36b6 · - · - · origem=Ronald (tmux)

O visitante ainda tem escapes e falsos positivos fora dos limites declarados.

| Mutante novo, inserido antes de `pos_vinculo.py:56` | Resultado do censo em memória | Causa |
|---|---|---|
| `_ler=_tz.localdate; _ler=_ler; _ler()` | 42, sem furos; escapou. [nao verificado] | O alvo apaga o alias antes de `_ligar` resolver novamente o valor. (`app/ponto/tests/test_cadastro_futuro_c5e.py:478`, `:443`, `:459`) |
| `_ler=_tz.localdate; _outro,_ler=_ler,1; _outro()` | 42, sem furos; escapou. [nao verificado] | Mesmo defeito aplicado ao desempacotamento. (`app/ponto/tests/test_cadastro_futuro_c5e.py:466`, `:478`) |
| `_tz.localdate((_v := None))` | 42, sem furos; escapou. [nao verificado] | `_e_none` aceita apenas `Constant` ou `Name`, embora o visitante suporte `NamedExpr` em outros lugares. (`app/ponto/tests/test_cadastro_futuro_c5e.py:497`, `:409`, `:494`) |
| `_v=None; _v: object; _tz.localdate(_v)` | 42, sem furos; escapou. [nao verificado] | Anotação sem valor preserva o valor Python, mas `visit_AnnAssign` visita o alvo e esquece o `None`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:487`, `:443`) |
| `_v=None; [_v for _v in ()]; _tz.localdate(_v)` | 42, sem furos; escapou. [nao verificado] | Comprehension não altera o `_v` externo em Python; o visitante compartilha o estado e o apaga. (`app/ponto/tests/test_cadastro_futuro_c5e.py:443`, `:509`) |

Falsos positivos concretos:

- `_v=None; [_tz.localdate(_v) for _v in instantes]`: sendo `instantes` uma lista de instantes válidos, só converte; o visitante conta uma leitura, elevando o censo para 43 e produzindo `reconciliar_apos_vinculo: 2 > teto 1`. A travessia genérica visita a expressão antes de visitar o alvo da comprehension. (`app/ponto/tests/test_cadastro_futuro_c5e.py:501`, `:509`) Resultado executado sem artefato: [nao verificado].
- `def f(): _v: _tz.localdate()`: conta leitura, embora Python não execute anotações locais. O comportamento novo vem da visita incondicional à anotação. (`app/ponto/tests/test_cadastro_futuro_c5e.py:488`) Confirmei em memória com um objeto que registra chamadas: nenhum evento runtime, um achado AST. [nao verificado]
- Closures, `global` e `nonlocal`: uma função interna que muda `_v=None` para um instante antes da conversão externa deixa o estado antigo no visitante, porque `_escopo` restaura o estado anterior. Há falsos positivos, embora envolvam fluxo entre funções e possam ser tratados como limite declarado. (`app/ponto/tests/test_cadastro_futuro_c5e.py:430`, `:440`)
- Rebind simples `_v=None; _v=instante; localdate(_v)` funciona e não conta, conforme o metateste existente. (`app/ponto/tests/test_cadastro_futuro_c5e.py:672`)
- Rebind de alias por `def`, `class`, import ou `except ... as` pode deixar o alias antigo e contar chamadas legítimas; parecem defeitos preexistentes. Esses bindings têm nomes em campos string e não passam por `visit_Name`; `_escopo` também não esquece o nome definido no escopo externo. (`app/ponto/tests/test_cadastro_futuro_c5e.py:430`, `:443`)

A base real permaneceu com 42 e sem furos na execução ad hoc. [nao verificado] O contrato exige censo igual ao teto, logo os escapes acima conservam a asserção verde apesar da leitura adicional. (`app/ponto/tests/test_cadastro_futuro_c5e.py:531`)

_4480 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
