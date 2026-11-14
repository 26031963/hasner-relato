# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 20:09** · publicado em **14/11/2026 20:09** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 20:09 · sessao b46ca7bab667 · - · - · origem=Ronald (tmux)

- **PROCEDENTE-candidato — estrutura/texto, P3:** “Revisar aprovadas” é segunda lista, sem ligação com a decisão de HE: calcula motivos em `app/ponto/views.py:1747`, acrescenta linhas em `:1753` e abre o painel da justificativa em `app/templates/ponto/fila_justificativas.html:154`; a porta de decisão efetiva permanece `app/ponto/portas/he.py:138`, com gravação em `:181` e recalculo em `:202`. Não vi segundo escritor de `DecisaoHE` nesses trechos.

  Cenário: escala 08–17, pausa12–13, previsto480, batidas E08/S18, trabalho540 e `esqueci_saida` aprovada na E08. A fonte conserva cobertura por turno fechado e devolve crédito0/motivo (`app/ponto/services/efeito_ausencia.py:128`, `:145`); o HE existente recebe a ponta60 (`app/ponto/services/espelho.py:722`, `:727`, `:776`; `app/ponto/services/he_pendente_lavrado.py:132`, `:148`). O mesmo dia entra também na revisão (`app/ponto/views.py:1747`). Sem justificativa, a revisão nem avalia o dia (`app/ponto/services/efeito_ausencia.py:98`, `:105`), embora o caminho existente de HE continue lendo a ponta (`app/ponto/services/he_pendente_lavrado.py:132`).

- **PROCEDENTE-candidato — texto/estrutura, P3:** o motivo/UI não significa necessariamente trabalho excedente: compara **bruto da justificativa + trabalho**, antes do limite, com previsto (`app/ponto/services/efeito_ausencia.py:143`, `:146`), mas a UI declara “Hora além do turno” (`app/templates/ponto/fila_justificativas.html:116`). O próprio caso420/59/pago361 exige esse motivo (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178`, `:185`, `:188`): trabalho59 não excede previsto420.

- **PROCEDENTE-candidato — estrutura/desempenho, P5:** o request processa todas as aprovadas, sem competência/janela/paginação (`app/ponto/views.py:1707`, `:1714`, `:1732`), e cada colaborador recebe intervalo do menor ao maior dia histórico (`:1745`, `:1747`). A contagem estática típica é **8 + 2D consultas por colaborador**, com `D` dias cobertos, além de opcionais por cadastro:
  - 3 próprias: justificativas, células e vínculos (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`);
  - 4 no carregamento de turnos: batidas, células, candidatos e vínculo usado pela ata (`app/ponto/turnos.py:1879`, `:1902`, `:1903`, `:1955`; `app/escala/alimentacao.py:148`; `app/ponto/turnos.py:1609`, `:1574`);
  - normalmente1 DNA (`app/ponto/turnos.py:1928`, `:2005`; `app/escala/utils.py:100`);
  - 2 por dia com célula/geradora: `realizado_do_dia` recarrega vínculo/célula e pausa, pois o chamador fornece somente turnos/previsto (`app/ponto/services/efeito_ausencia.py:140`; `app/ponto/turnos.py:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1187`).
  
  Há extras condicionais para geradora fora dos candidatos, agregação de última batida e dia sem geradora (`app/escala/alimentacao.py:157`; `app/ponto/turnos.py:1917`; `app/escala/servico_jornada.py:64`). Tempo real e timeout observado: **[nao verificado]**. A configuração é gthread/30s (`app/gunicorn.conf.py:19`, `:21`); o código registra que esse timeout não é teto rígido para request longo com gthread (`app/config/crons.py:1393`).

- **P6, não vi relógio novo no delta fornecido:** funções novas de cálculo/projeção/leitura não chamam `now()`/`today()` (`app/ponto/services/efeito_ausencia.py:98`, `:136`, `:151`; `app/escala/services/leitor_celula.py:354`). `tz.localdate()` da Central aparece como contexto preexistente no delta e alimenta SLA/cards (`app/ponto/views.py:1727`, `:1733`, `:1759`). O fechamento captura um instante vivo por rodada (`app/ponto/services/fechamento.py:616`), transmite-o ao filtro de furos (`:1120`) e consulta encerramento (`:377`; `app/ponto/turnos.py:2108`). Atribuir os nove furos de14/11 a isso, ou verificar novidade desde14b61c124: **[nao verificado]**.

## 14/11 20:08 · sessao 2dfc1fb51f5e · - · - · origem=Ronald (tmux)

P7/P8 — achados para consolidação:

1. **PROCEDENTE-candidato — texto: contrato antigo contradiz os testes atuais.** A docstring declara pagamento pelo período, C5=690, C7 sem justificativa e D1=420; as asserções atuais esperam respectivamente 720, cobertura inteira com 720 e 510. Referências: `app/ponto/tests/test_justificativa_paga_o_periodo.py:1`, `:17`, `:18`, `:20`, `:462`, `:492`, `:564`. D2/D3 dizem “nada se cobre”, porém esperam `cobre=None`, que representa cobertura inteira: `:496`, `:502`, `:509`, `:516`; semântica declarada em `app/ponto/services/efeito_ausencia.py:78`.

2. **PROCEDENTE-candidato — estrutura/texto: teste “sem perda DSR” não executa o cálculo semanal completo.** Ele extrai apenas o `If` que contém `_mj`, executa esse bloco e confere `dsr_ok`; não verifica `semanas_dsr_perdido`. Referências: `app/ponto/tests/test_justificativa_soma_sem_banco.py:262`, `:267`, `:273`, `:276`, `:277`. O contador real está depois do bloco e existe outro cálculo em `MotorComercial`: `app/ponto/motor_calculo_v2.py:2641`, `:2667`, `:3474`, `:3490`, `:3515`. Prova executada somente em memória: mudar `:3474` para iniciar `semanas_dsr_perdido=1` mantém os 24 testes `SomaDoDiaTest` verdes; resultado de execução sem arquivo persistido [nao verificado].

3. **PROCEDENTE-candidato — texto: “bordas ORM substituídas” omite substituição de autoridade de domínio.** A docstring diz isso em `app/ponto/tests/test_justificativa_soma_sem_banco.py:5`, mas `turnos_do_colab` inteiro é substituído por `self.ts` em `:43`, e os turnos são fabricados usando o tipo gravado para decidir entrada/saída em `:53`–`:58`. Assim, esses testes não exercitam o pareamento real, a atribuição do dia pelo envelope nem a aplicação real da L-087. Referências: `:43`, `:57`, `:58`.

4. **PROCEDENTE-candidato — texto: nome da propriedade promete uma desigualdade que o próprio teste permite violar.** `test_propriedade_coberto_implica_soma_dentro_do_previsto` só registra violação quando o crédito é positivo. Com previsto=100, trabalho=120 e crédito=0, a cobertura permanece e a soma=120 passa pelo teste — comportamento compatível com a lei atual, mas incompatível com o nome. Referências: `app/ponto/tests/test_justificativa_soma_sem_banco.py:163`, `:165`, `:168`, `:172`, `:174`.

**Mutantes declarados:** não vi mutante declarado que faça algo incompatível com seu nome. Li as 12 substituições em `app/ponto/tests/mutantes_justificativa_soma.py:19`–`:56`, o mecanismo de carregamento em `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:27`–`:43` e o critério de morte em `app/ponto/tests/mutantes_justificativa_soma.py:94`. Executei `--todos`: controle com 24 testes; 12/12 mortos por asserção; retorno 0. Resultado de execução sem arquivo persistido [nao verificado]. Nenhum desses 12 muta o pareador, o motor semanal ou a porta de HE: `app/ponto/tests/mutantes_justificativa_soma.py:19`–`:56`.

Não executei a suíte ORM; seus resultados permanecem [nao verificado]. [nao verificado]

## 14/11 20:06 · sessao 93faa118c4dc · - · - · origem=Ronald (tmux)

Censo somente leitura; nenhuma edição nem execução com banco. O recorte contém **65 métodos únicos**, correspondendo às **70 ocorrências**: cinco subtests A/S acrescentam quatro ocorrências; dois subtests R2 acrescentam uma. Isso corresponde aos 69 failures + 1 error fornecidos. As linhas abaixo são da base inspecionada.

Classificação proposta: **60 métodos (a)**, **1 método (b), com cinco subtests**, e **4 métodos (c)**. A exclusão dos quatro métodos (c) depende da remoção efetiva do balde e do censo exigido por L-111.

| Arquivo | Método | Classe | Adaptação proposta |
|---|---|---|---|
| `chamados/tests/test_o261_vigia_fio.py:624` | `test_MORDE_a_lista_e_perguntada_no_UNIVERSO_DA_TELA` | a | Usar dia efetivamente sem porta; a fixture atual publica residual `decidir`, evitando a lista. |
| mesmo arquivo`:418` | `test_a_abertura_por_PERGUNTA_soma_o_universo_da_PORTA` | a | Adequar contagem aos dias que continuam sem porta após o juiz. |
| mesmo arquivo`:455` | `test_a_LISTA_com_dono_leva_a_CLASSE_de_cada_dia_sem_precedencia` | a | Preservar classificação da lista apenas para dias sem porta publicada. | [nao verificado]
| mesmo arquivo`:727` | `test_as_linhas_do_log_NAO_LEVAM_NOME_de_ninguem` | a | Preservar ausência de PII; ajustar `FIO_MORTO=1` à fixture sem porta. |
| mesmo arquivo`:680` | `test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | a | Tirar parcela do balde extinto; Resolver oferecido integra `fio_vivo`. |
| mesmo arquivo`:368` | `test_as_TRES_classes_de_DIA_somam_o_fio_morto` | a | Classes contam apenas dias sem porta, não residual `decidir`. |
| mesmo arquivo`:280` | `test_dia_SEM_pergunta_nenhuma_e_a_classe_mais_crua` | a | Sem pergunta com `decidir` tem porta; inverter consequência ou construir ausência real de porta. |
| mesmo arquivo`:557` | `test_MORDE_aguardando_DOCUMENTO_tambem_e_bola_do_DP` | a | Manter contador LU-04/R3, com fixture realmente sem porta. |
| mesmo arquivo`:224` | `test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | a | Preservar asserts C6/property; consequência no vigia agora considera residual com porta. |
| mesmo arquivo`:540` | `test_MORDE_ausencia_ABERTA_cobrindo_o_dia_AGUARDA_O_DP_e_nao_e_fio_morto` | a | Ausência decidível tem ação/porta; balde permanece somente para ausência sem porta. |
| mesmo arquivo`:513` | `test_MORDE_chamado_em_analise_SEM_resolver_dia_e_FIO_MORTO` | a | Manter teste vivo; `decidir` residual alcançável implica fio vivo; retirar só assert do balde apagado. | [nao verificado]
| mesmo arquivo`:586` | `test_MORDE_dono_BATIDA_ou_CADASTRO_NA_LISTA_e_fio_do_admin_e_o_resto_fica` | a | Manter subdivisão do sem-porta e testar com dias sem porta. |
| mesmo arquivo`:614` | `test_MORDE_falha_da_lista_e_UMA_chamada_por_rodada_e_todos_ficam` | a | Preservar custo/falha da lista com fixture que efetivamente chega à lista. |
| mesmo arquivo`:659` | `test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | a | Preservar subdivisão; porta residual prevalece antes do contador. |
| mesmo arquivo`:789` | `test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | a | Preservar comparação com censo; mudar mistura e contadores antigos, passar instante do carimbo também ao censo. |
| mesmo arquivo`:163` | `test_MORDE_o_dia_de_HOJE_fica_FORA_do_contador_alarmado` | a | Testar teto somente em dia sem porta, pois porta válida vence essa classificação. |
| mesmo arquivo`:485` | `test_MORDE_o_dia_que_OFERECE_resolver_dia_e_DECISAO_PENDENTE_e_nao_fio_morto` | c | Exclusivo do balde; remover com L-111; cobertura nova M4 prova porta administrativa. |
| mesmo arquivo`:389` | `test_MORDE_o_dia_SO_de_plantio_fora_do_slot_NAO_e_ALVO` | a | ALVO continua vivo; montar universo sem porta ou adequar contagem. |
| mesmo arquivo`:128` | `test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | a | Pergunta morta não remove porta residual `decidir`; escolher par que difira na porta publicada. | [nao verificado]
| mesmo arquivo`:521` | `test_MORDE_o_passivo_ALVO_nao_some_dentro_da_decisao_pendente` | c | Exclusivo do balde e de seu contador ALVO; remover com balde. |
| mesmo arquivo`:197` | `test_MORDE_o_VOCABULARIO_do_cartorio_e_quem_diz_quais_dias_cobram` | a | Preservar vocabulário e recorte; adaptar fixture à porta válida. |
| mesmo arquivo`:145` | `test_MORDE_registrado_e_VIVO_mas_nao_COBRA_ninguem` | a | `registrado`/`historico` não dão porta LU-02; caso aberto com `decidir` dá porta. |
| mesmo arquivo`:404` | `test_o_dia_MISTO_conta_UMA_vez_e_no_ALVO` | a | Preservar unidade colab-dia/ALVO; contar apenas sem porta. |
| mesmo arquivo`:564` | `test_o_dia_que_OFERECE_resolver_dia_fica_na_DECISAO_DO_DIA_mesmo_com_ausencia_aberta` | c | Exclusivo da precedência do balde extinto. |
| mesmo arquivo`:495` | `test_o_juiz_REAL_oferece_o_dia_que_acusa_sem_lavra` | c | Assert exclusivo do balde; a integração real útil pode migrar para M4/M8. |
| mesmo arquivo`:606` | `test_sem_resposta_da_lista_o_dia_FICA_no_alarme` | a | Preservar falha explícita, com entrada sem porta. |
| `chamados/tests/test_e3_tarefa_admin_sem_banco.py:133` | `test_B_252_29_set_hipotese_ultima_resposta_textual` | a | Âncora existente fora da fila dá `admin/porta_acao/validar`. |
| mesmo arquivo`:136` | `test_B_841_10_out_hipotese_ultima_resposta_textual` | a | Mesma inversão LU-03. |
| `chamados/tests/test_e3_c8b_sem_banco.py:196` | `test_carga_da_tarefa_pergunta_ao_c6_avulso_lote_e_idempotencia` | a | Carga passa âncoras existentes: esperar `admin/porta_acao`, preservar custo e idempotência. |
| mesmo arquivo`:48` | `test_cinco_formas_acusadoras_sem_pergunta_viva` — cinco subtests | b | A/S sem oferta de Resolver não dá dono; manter `nada`, curar juiz e acrescentar controle oferecido. |
| mesmo arquivo`:166` | `test_grave_respondida_em_disputa_fechada_nao_e_filtrada_por_motivo` | a | Passar mapa de âncoras existente e esperar administração alcançável. |
| mesmo arquivo`:184` | `test_sem_trabalho_sem_lampadas_nao_esconde_resposta_viva` | a | Mesma inversão, preservando exposição da pergunta/marco. |
| `chamados/tests/test_e4_passivo.py:57` | `test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | a | Após registrar resposta E1, passa a `admin/porta_acao`; segunda rodada não seleciona mais sem-porta; preservar uma linha E1. |
| `chamados/tests/test_o261e_f5_alcance.py:484` | `test_a_ancora_VIVA_fora_da_fila_tambem_e_veu` | a | Resposta ancorada em registrado dá `porta_acao/admin`, sem causa sem-porta. |
| mesmo arquivo`:213` | `test_c_respondida_com_destino_FECHADO` | a | Âncora terminal existente também dá modal: `porta_acao/admin`. |
| `chamados/tests/test_e3_portas_tarefa.py:465` | `test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar` | a | Com app: `porta_app/colab`; sem app: preservar admin. |
| mesmo arquivo`:483` | `test_historico_tem_porta_mas_nao_e_nomeado_na_lista_padrao` | a | LU-02: projeção `None`, sem dono; manter verbo como evidência. |
| mesmo arquivo`:419` | `test_RED_A_na_fila_aparece_com_acao_sob_B` | a | Com app: `porta_app`; sem app: `porta_fila/chamado`; manter fila contextual. |
| mesmo arquivo`:115` | `test_tarefa_propaga_relogio_a_alimentacao` | a | A pergunta pendente no app continua dando `porta_app`; manter verificação do verbo/instante. |
| mesmo arquivo`:505` | `test_v8_R1_colab_alcancavel_e_leitores` | a | Trocar `porta_pergunta` por `porta_app`; todos os leitores seguem publicação. |
| mesmo arquivo`:231` | `test_v8_R2_alcance_admin_com_pergunta_colab` | a | `porta_app/colab`, preservando verbo administrativo contextual. |
| mesmo arquivo`:525` | `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia` — dois subtests | a | Ambos `porta_app/colab`, sem alterar ações originais. |
| mesmo arquivo`:265` | `test_v8_R7_membro_da_fila_sem_verbo` | a | LU-02: None não dá dono/porta. |
| mesmo arquivo`:572` | `test_v8_R7_membro_sem_verbo_conservado` | a | Alimentação conserva membro/None; projeção da tarefa é `None`. |
| `chamados/tests/test_o261e_f5b_juiz_porta.py:259` | `test_MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta` | a | C17 histórico pode continuar sem porta; tarefa/Folha dão `admin/porta_acao/validar`. |
| mesmo arquivo`:276` | `test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | a | C17 histórico mantém assert próprio; tarefa/Folha passam a sem porta por LU-02. |
| mesmo arquivo`:135` | `test_MORDE_ricardo_a_folha_le_o_juiz_e_diz_nada` | a | Folha deve ler `admin`, pois há âncora existente para validar. |
| mesmo arquivo`:140` | `test_MORDE_ricardo_folha_e_vigia_dizem_a_MESMA_coisa` | a | Ambos leem `porta_acao/admin`, sem causa c. |
| `escala/tests/test_f3_beco1_folga_na_lista.py:137` | `test_MORDE_competencia_anterior_NAO_exportada_vai` | a | Resposta ancorada tem modal; C11 vazio. |
| mesmo arquivo`:351` | `test_MORDE_F5_queries_nao_crescem_com_os_dias` | a | Preservar custo; ajustar cardinalidade dos resultados à exclusão por porta. |
| mesmo arquivo`:199` | `test_RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta` | a | Porta residual `decidir/chamado` permanece; atualizar projeção com `acao`, se publicada. | [nao verificado]
| mesmo arquivo`:219` | `test_RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta` | a | Agora `admin/porta_acao`, C11 vazio. |
| mesmo arquivo`:100` | `test_RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO` | a | Fixture respondida/ancorada já tem modal; esperar porta e ausência no C11. |
| mesmo arquivo`:244` | `test_RED_F1_lista_le_o_instante_de_quem_chama` | a | Preservar instante explícito e sua propagação, sem exigir o C11 antigo. |
| mesmo arquivo`:292` | `test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | a | Preservar dono C9, retirar expectativa de C11 para dia que tem porta administrativa. |
| `chamados/tests/test_f3_beco1_reata_limbo.py:67` | `test_MORDE_dry_nao_escreve` | a | Dia já tem porta: zero candidatos, nenhuma escrita. |
| mesmo arquivo`:149` | `test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` | a | Âncora atual já fornece modal; não há candidato a reata nesse cenário. |
| mesmo arquivo`:76` | `test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` | a | Não cria pai; dia com modal também não entra C11. |
| mesmo arquivo`:49` | `test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` | a | Já sai pelo modal: zero reatas, destino preservado. |
| mesmo arquivo`:124` | `test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` | a | Nenhuma seleção nesse cenário; preservar destino sem reata. |
| mesmo arquivo`:105` | `test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` | a | Sem seleção/reata; excluir expectativa de C11 para dia com modal. |
| `chamados/tests/test_e3_bloqueantes.py:179` | `test_RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal` | a | Assert agora exige `admin/porta_acao`; furo não inclui dia. |
| mesmo arquivo`:170` | `test_RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal` | a | Mesma inversão LU-03. |
| `chamados/tests/test_n1_perguntas_do_app.py:383` | `test_RED_censo_dono_fora_do_fio_le_uma_instancia_do_colab` | a | Fixture tem três `decidir`: não chega ao balde; verificar zero chamadas ou construir entrada sem porta para continuar testando identidade. |
| `chamados/tests/test_sombra_c1_resolver_em_lote.py:35` | `test_RED_censo_monta_um_calendario_por_colaborador` | a | Três dias com residual `decidir`: `fio_vivo=3`, zero calendários; acrescentar entrada sem dono para prova positiva uma vez por colaborador. |

Pontos adicionais fora do recorte:

- `chamados/tests/test_e3_c8b_sem_banco.py:81`: schema exato precisa incluir `acao`, caso publicada na tarefa.
- `chamados/tests/test_o261_vigia_fio.py:483`: stub plural `lambda colab, datas` precisa aceitar `agora` keyword.
- `chamados/tests/test_o261e_f5_alcance.py:293`: teste exclusivo do balde extinto, elegível a remoção L-111.
- `chamados/tests/test_o261e_f5_alcance.py:406`: teste misto continua vivo; oferta agora deve resultar `porta_acao/admin`, sem papel inventado `regularizar_batida`.
- `chamados/tests/test_sombra_c1_resolver_em_lote.py:49`: a prova de custo deve acompanhar a carga da tarefa; forçar `porta_da_tarefa=None` depois do julgamento já não força consulta ao Resolver.

Os B sem banco possuem âncoras existentes em `forma_b`, mas os testes diretos não passam o mapa ao juiz (`chamados/tests/test_e3_tarefa_admin_sem_banco.py:103`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:127`). Para provar LU-03, passar `{p.pk: chamado_que_lista(p)}`; `_pela_decisao` atualmente reduz a porta a None fora da fila (`chamados/services/tarefa_do_dia.py:140`).

Comentários V6: substituir a restrição “admin só chega por chamado na fila” por **LU-03: ação administrativa com tela dá porta, inclusive validação com âncora existente fora da fila**; None sem âncora não dá dono. Pontos: `test_e3_bloqueantes.py:97`, `test_o261e_f5_alcance.py:487`, `test_o261e_f5b_juiz_porta.py:264`, `folha/tests/test_haiku_fora_da_folha.py:160`.

Comando sem banco recomendado:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_degrau_dono_e_porta
```

Não executei esse comando para evitar disputar o RED/GREEN do construtor. Os demais arquivos listados importam `django.test.TestCase` e suas fixtures escrevem no banco; execução proibida nesta sessão (`chamados/tests/test_o261_vigia_fio.py:24`, `escala/tests/test_f3_beco1_folga_na_lista.py:18`, `chamados/tests/test_f3_beco1_reata_limbo.py:17`).

## 14/11 20:04 · sessao 0a6e6dad6330 · - · - · origem=Ronald (tmux)

Censo realizado somente por leitura.

- `vigia_fio.fio_do_dia` não tem chamador de produção encontrado: a busca encontra somente sua definição em `chamados/services/vigia_fio.py:156`. Os homônimos da Folha e do serviço de veredito são funções distintas. Comando:  
  `git grep -n -E '(vigia_fio import.*fio_do_dia|vigia_fio\.fio_do_dia|fio_do_dia\()' -- '*.py' ':!*/tests/*' ':!*/migrations/*'`

- `_resolver_da_rodada` tem somente os dois chamadores internos dos censos, em `chamados/services/vigia_fio.py:440` e `chamados/services/vigia_fio.py:661`; sua definição fica em `chamados/services/vigia_fio.py:258`. Depois de transferir a consulta para a carga da tarefa e remover essas chamadas, cabe apagá-lo com lápide L-111.

- O balde `decisao_do_dia_pendente` não tem consumidor de produção fora de `vigia_fio.py`: inicialização `:365`, classificação `:443`, passivo `:494`, enumeração `:547` e impressão `:906`. Comando:  
  `git grep -n 'decisao_do_dia_pendente' -- '*.py' ':!*/tests/*' ':!*/migrations/*'`

- `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia` e `fio_do_admin` são atualmente subdivisões dos dias sem porta: o censo já passou pela guarda da porta em `chamados/services/vigia_fio.py:428`; o alcance passou pela guarda em `chamados/services/vigia_fio.py:669`. As classificações em `chamados/services/vigia_fio.py:308` não escrevem campos da tarefa. Os comentários ainda atribuem “bola” e deveriam declarar explicitamente essa limitação LU-03.

- As duas construções avulsas de `ListaDoAdmin` perdem o instante em `chamados/services/vigia_fio.py:391` e `chamados/services/vigia_fio.py:645`. Devem receber `agora=agora`. A classe já encaminha `self.agora` em `chamados/services/vigia_fio.py:216`; o default de `escala/services/cadastro_realidade.py:775` deixa de ser acionado nesse caminho.

Leitores encontrados:

| Leitor | Uso atual |
|---|---|
| `folha/fora_da_folha.py:244` | Usa `fila` como números de chamados exibidos; a documentação `:241` declara chamados exigíveis da célula. |
| `folha/retrato_folha.py:1190` | Grava fio agregado como `dono`; `portas_da_linha`, `:1015`, consulta mapas de fio/tipo. |
| `chamados/services/vigia_fio.py:412` | Testa presença; o alcance `:673` conta rota e `:675` publica fio. |
| `chamados/services/furo_sem_canal.py:97` | Testa presença e conta rota em `:99`. |
| `escala/services/cadastro_realidade.py:469` | Exclui o beco quando existe porta. |
| `ponto/services/pautas_dos_discordantes.py:81` | Lê fio por projeção da Folha. |
| `chamados/management/commands/diff_e3_sombra.py:50` | Diagnóstico de `fila`; `:143` imprime rota, fio e tamanho. |

Nenhum deles interpreta `acao` atualmente, pela busca textual acima. Incidência dinâmica: `[não verificado]`.

Para M5, conservar `fila=(101,)` faz a Folha exibir 101 ao lado da porta administrativa da pergunta 202 (`folha/fora_da_folha.py:244`). Recomendo alinhar `fila` aos chamados da célula pertencentes às portas do dono e passar âncora/ação separadamente para produzir o link correto.

Consequências nos testes: [nao verificado]

| Teste/local | Tratamento sugerido | [nao verificado]
|---|---|
| `test_o261_vigia_fio.py:485` | Classe c: comportamento específico do balde removido; M4 passa a provar porta administrativa. |
| `test_o261_vigia_fio.py:521` | Classe c: contador `decisao_do_dia_pendente_com_alvo` removido. |
| `test_o261_vigia_fio.py:564` | Classe c quanto à precedência do balde; M4/M6 devem preservar a cobertura de porta. |
| `test_o261_vigia_fio.py:495` | Preservar: prova o juiz real; inverter resultado para `fio_vivo`, sem balde. |
| `test_o261_vigia_fio.py:513` | Preservar a contraprova sem oferta; retirar somente consulta à chave apagada. |
| `test_o261_vigia_fio.py:680` | Preservar soma; retirar termo do balde e contar o dia oferecido em `fio_vivo`. |
| `test_sombra_c1_resolver_em_lote.py:35` | Preservar desempenho; trocar termo do balde por `fio_vivo`. |
| `test_sombra_c1_resolver_em_lote.py:70` | Preservar paridade e fragmentação da plural, que continua viva. |
| `test_n1_perguntas_do_app.py:383` | Fixar oferta falsa para os dias chegarem ao subdivisor; a transferência da consulta para a tarefa muda o caminho. |

A fixture `_oferece` em `chamados/tests/test_o261_vigia_fio.py:483` precisa aceitar `**kw` para receber `agora`. A constante `PERMISSAO_DO_RESOLVER_DIA` tem teste de tela em `chamados/tests/test_o261e_f5_alcance.py:451`; não removê-la inadvertidamente junto do balde sem adaptar essa prova viva.

## 14/11 20:02 · sessao 941f6ed772c9 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita na fonte: `pago = max(0, previsto - trabalhado_do_dia)`, preservando `cobre` e nomeando o excedente para revisão. O bruto informa o motivo; não impõe outro teto (`ponto/services/efeito_ausencia.py:138`, `ponto/services/efeito_ausencia.py:145`).

**Q1.** O motor recebe o crédito pelo mapa alimentado em `ponto/services/fechamento.py:755`, `ponto/services/espelho.py:1134` e `relatorios/pdf_espelho.py:407`. Quando um dia justificado está ausente desse mapa, o motor usa o previsto inteiro (`ponto/motor_calculo_v2.py:2629`).

Com `cobre` vazio, a fonte exclui o dia de `justificadas`; portanto, os dados fornecidos não demonstram como os 420 chegaram ao motor. Além disso, o campo medido `minutos_abonados` é gravado da grade, por `por_dia_da_grade`, em `ponto/services/fechamento.py:1066`. A origem exata dos 420 produtivos permanece **[nao verificado]**. A cura utiliza o feed existente, sem editar o motor (`ponto/services/efeito_ausencia.py:47`).

**Q2.** A recusa integral podia retirar a justificativa usada pelo DSR. O leitor exige que o dia obrigatório tenha trabalho reconhecido ou esteja em `datas_justificadas`; não exige pagamento integral (`ponto/motor_calculo_v2.py:2605`, `ponto/motor_calculo_v2.py:3490`). O teste local executa o ramo real do motor com crédito 361, sem falta e com `dsr_ok=True` (`ponto/tests/test_justificativa_soma_sem_banco.py:262`). A explicação exata do 4→3 produtivo permanece **[nao verificado]**.

**Q3.** Existe `decidir_he`, destinada a minutos reais fora da janela (`ponto/portas/he.py:138`). A Gestão de HE lê essas pontas do retrato (`ponto/services/he_pendente_lavrado.py:19`). Para o excedente da justificativa, acrescentei a projeção `justificativas_para_revisao`, consumida pela Central no filtro “Revisar aprovadas”, uma linha por dia-colaborador (`ponto/services/efeito_ausencia.py:151`, `ponto/views.py:1738`, `templates/ponto/fila_justificativas.html:56`).

**Q4.** `cobre` mantém os marcos; somente o crédito muda. A grade dispensa os marcos cobertos e transporta o número; `dia_pago` soma esse número sem exigir a duração integral do período (`escala/utils.py:1266`, `escala/utils.py:1277`, `ponto/services/dia_pago.py:398`). Cobertura inteira também recebe mapa explícito, inclusive zero, evitando o fallback do motor (`ponto/services/efeito_ausencia.py:47`).

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `previsto` | Ata: `ponto/services/cartorio.py:739`; leitor comum: `escala/services/leitor_celula.py:354` | `ponto/services/efeito_ausencia.py:138` |
| `trabalhado` | Autoridade `realizado_do_dia`: `ponto/turnos.py:774`; sem turno retorna `None`: `ponto/turnos.py:721` | Adaptação para zero e conta: `ponto/services/efeito_ausencia.py:141` |
| `cobre` | Cobertura e união existentes: `ponto/services/efeito_ausencia.py:125`, `ponto/services/efeito_ausencia.py:133` | Efeito: `ponto/services/efeito_ausencia.py:46`; grade: `escala/utils.py:1257` |
| `minutos` | Saldo diário: `ponto/services/efeito_ausencia.py:145` | Feed: `ponto/services/efeito_ausencia.py:47`; grade: `escala/utils.py:1277` |
| `motivo` | `ponto/services/efeito_ausencia.py:147` | Projeção: `ponto/services/efeito_ausencia.py:153`; Central: `ponto/views.py:1747`; apresentação: `templates/ponto/fila_justificativas.html:116` |
| Feed do motor | Três chamadores citados em Q1, incluindo órfão em `ponto/services/fechamento.py:831` | Abono: `ponto/motor_calculo_v2.py:2628` |

Outros alimentadores permanecem lendo o mesmo mapa: esmeril (`ponto/services/esmeril_espelho.py:166`), extrato (`relatorios/management/commands/diff_extrato_parcial.py:62`) e comparação de alimentação (`ponto/management/commands/diff_espelho_alimentacao.py:124`).

Diff por arquivo::função:

- `efeito_ausencia.py::periodos_justificados`: substitui recusa integral pelo saldo diário; trata ausência de turno (`ponto/services/efeito_ausencia.py:136`).
- `efeitos_da_ausencia` e `utils.py::_marcar_ausencias`: transportam crédito explícito para cobertura inteira (`ponto/services/efeito_ausencia.py:47`, `escala/utils.py:1257`).
- `views.py::fila_justificativas` e template: revisão pelo motivo da fonte (`ponto/views.py:1738`, `templates/ponto/fila_justificativas.html:116`).
- Testes: S1–S6 invertidos, T1/T7, propriedades, feed real, ramo do motor e Central (`ponto/tests/test_justificativa_soma_sem_banco.py:83`, `ponto/tests/test_justificativa_soma_sem_banco.py:163`, `ponto/tests/test_justificativa_soma_sem_banco.py:185`).
- Caracterizações anteriores ajustadas à fórmula nova; ORM escrito, com relógio fixado (`ponto/tests/test_justificativa_periodo_v2_sem_banco.py:209`, `ponto/tests/test_justificativa_paga_o_periodo.py:600`).

RED antes da implementação: **20 testes, 15 falhas por asserção**, preservado em [justificativa_soma_red.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_red.txt:179). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco -v
```

GREEN local: **39 testes passando**, em [justificativa_soma_green.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_green.txt:42). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v
```

Mutantes: **controle verde e 12/12 mortos por asserção**. Os comandos individuais e resultados estão em [justificativa_soma_mutantes.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_mutantes.txt:1). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma recusa_integral_c5
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma sem_max_zero
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma so_turno_da_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma feed_motor_le_bruto
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos
```

ORM escrito para a sessão executar; resultado **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_justificativa_paga_o_periodo --settings=config.settings.ci
```

Não executei ORM, banco, rede, Docker, reconstrução, commit, merge, push, deploy ou crons. Não alterei motor, portas protegidas, modelos, migrations ou docs. Certificação independente dessas restrições: **[nao verificado]**. Frota dos 41, contagem produtiva por motivo, 9 colaboradores da competência 11 e exportadas: **[nao verificado]**. A certificação fica com Code/suíte e opus final. [nao verificado]

`LEI-AKITA: origem=Ronald 14/11 19:4x (ponto/services/efeito_ausencia.py:83); testemunha=feed real PDF e ramo real motor (ponto/tests/test_justificativa_soma_sem_banco.py:242, :262); RED=ponto/tests/justificativa_soma_red.txt:181; quem-mais-le=grade/fechamento/espelho/PDF/esmeril/extrato; juizes novos=0; padrao=single source of truth / um juiz por pergunta (docs/PLANO-PADROES.md:46).`

## 14/11 19:53 · sessao e037f87dec36 · E3-V10-DEGRAU-DONO-E-PORTA · verificador · origem=Code

**Parecer v2: há achados PROCEDENTES em LU-03 e LU-07.** A publicação de `porta_acao` aceita qualquer ação administrativa exigível, sem conferir sua tela; a classificação entre `porta_fila` e `porta_pergunta` ainda usa portas de outros chamados do dia. (`app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:627`)

1. **Escritores, leitores e aceitação de `porta_acao`.**

   | Campo/caminho | Escritor ou leitor encontrado |
   |---|---|
   | `fio`, `rota`, `fila`, `verbos` da tarefa | Escrita final em `_portas_da_tarefa`; `porta_da_tarefa` apenas projeta esses campos. (`app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/services/tarefa_do_dia.py:640`) |
   | `donos` | É variável local do juiz, não campo publicado da tarefa. (`app/chamados/services/tarefa_do_dia.py:608`, `app/chamados/services/tarefa_do_dia.py:630`) |
   | API do app | Lê `consulta_do_app`, sem consumir `rota`/`fio` da tarefa nesses endpoints. (`app/api/views.py:993`, `app/api/views.py:2533`) |
   | `fora_da_folha` | Lê a porta, extrai `fio` e usa `fila` para os números dos chamados. (`app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:82`, `app/folha/fora_da_folha.py:244`) |
   | `retrato_folha` | Recebe a agregação da Folha e grava o fio na chave histórica `dono`; o leitor usa esse valor gravado. (`app/folha/retrato_folha.py:1158`, `app/folha/retrato_folha.py:1190`, `app/folha/retrato_folha.py:438`) |
   | `vigia_fio` | `censo` testa presença da porta; `censo_alcance` lê sua rota e seu fio. (`app/chamados/services/vigia_fio.py:428`, `app/chamados/services/vigia_fio.py:669`) |
   | `furo_sem_canal` / lavrador | Presença da porta define canal; os quatro contadores de rota são calculados e impressos. (`app/chamados/services/furo_sem_canal.py:97`, `app/chamados/services/furo_sem_canal.py:114`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`) |
   | `cadastro_realidade` | Exclui o beco quando a tarefa publica qualquer porta. (`app/escala/services/cadastro_realidade.py:464`, `app/escala/services/cadastro_realidade.py:469`) |
   | Pauta dos discordantes | Usa o fio projetado pela Folha e compara com `FIO_SEM_PORTA`. (`app/ponto/services/pautas_dos_discordantes.py:64`, `app/ponto/services/pautas_dos_discordantes.py:81`) |
   | Diff sombra | Compara a projeção nova com o C17 histórico. (`app/chamados/management/commands/diff_e3_sombra.py:43`, `app/chamados/management/commands/diff_e3_sombra.py:127`) |
   | Relatórios e templates examinados | As telas de pendências usam suas fontes de classificação; o template do retrato recebe frases e links prontos, sem interpretar a enumeração da tarefa. (`app/relatorios/views_pend.py:23`, `app/relatorios/views_pend.py:91`, `app/templates/folha/_retrato_o_que_fazer.html:33`, `app/templates/folha/_retrato_o_que_fazer.html:72`) |

   **NÃO PROCEDENTE — `porta_acao` provocar KeyError ou desaparecer dos contadores identificados.** O alcance inicializa as quatro rotas antes do incremento dinâmico; furo e lavrador também incluem app e ação. (`app/chamados/services/vigia_fio.py:587`, `app/chamados/services/vigia_fio.py:673`, `app/chamados/services/vigia_fio.py:741`, `app/chamados/services/furo_sem_canal.py:114`)

   **PROCEDENTE — LU-07 ainda permite rota classificada por outro chamado.** Caso sintético reproduzido: ação `validar/admin/202`, pergunta ancorada em 202, chamado 101 da célula com `cobrar`. O juiz filtra a porta administrativa para 202, mas publica `rota='porta_fila'`, `fila=(101,)`, porque a escolha final consulta `da_celula` global. A porta do admin é a da pergunta; quem força “fila” é o cobrar sem dono. (`app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:611`, `app/chamados/services/tarefa_do_dia.py:619`, `app/chamados/services/tarefa_do_dia.py:627`)

2. **LU-03: tela efetiva e apresentação de `porta_acao`.**

   Existem telas para validar pergunta no modal do fio, decidir ausência e Resolver dia; esta última depende da oferta do calendário e da permissão. (`app/templates/chamados/partials/modal_fio.html:279`, `app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344`, `app/templates/colaboradores/partials/_calendario_grade.html:131`)

   **PROCEDENTE — a publicação não prova alcançabilidade.** `_portas_da_tarefa` aceita toda ação `ADMIN`; não pergunta se existe tela correspondente. O contraexemplo sem âncora está detalhado no item 8. (`app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`)

   **PROCEDENTE — a Folha pode mostrar “admin agir” sem link para essa ação.** `o_que_segura` descarta `rota`; o retrato tem `PORTA_DO_FIO['admin']=None`, e tipos `falta`, `nao_bate` e `ferias` também não fornecem link. Nessa combinação, `portas_da_linha` devolve lista vazia e o template omite os botões. (`app/folha/fora_da_folha.py:246`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:990`, `app/folha/retrato_folha.py:1015`, `app/templates/folha/_retrato_o_que_fazer.html:66`)

   **NÃO PROCEDENTE — âncora terminal ou silenciada, isoladamente, impedir validação.** O fio administrativo carrega todos os chamados da pessoa, e o POST de validar exige correspondência da âncora, mas não pertença à fila. (`app/chamados/services/fio.py:204`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`)

3. **Baldes do vigia e código morto.**

   **PROCEDENTE — ainda há decisão própria de dono fora da porta publicada.** Depois de receber porta `None`, `dono_fora_do_fio` classifica oferta de Resolver dia, ausência aberta, aceite de folga e dono da lista. Ambos os censos usam essa cadeia para retirar dias do fio morto. Portanto, os baldes ainda não se limitam a ler a porta da tarefa. (`app/chamados/services/vigia_fio.py:306`, `app/chamados/services/vigia_fio.py:308`, `app/chamados/services/vigia_fio.py:311`, `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:442`, `app/chamados/services/vigia_fio.py:683`)

   **NÃO PROCEDENTE — esses baldes ficaram sem chamador.** Permanecem chamados pelos dois censos. **PROCEDENTE — sobrou a função `vigia_fio.fio_do_dia` sem chamador de produção encontrado na busca estática**; os censos passaram a ler a porta. Chamador dinâmico: [não verificado]. (`app/chamados/services/vigia_fio.py:156`, `app/chamados/services/vigia_fio.py:428`, `app/chamados/services/vigia_fio.py:442`, `app/chamados/services/vigia_fio.py:683`)

4. **Segundo juiz/escritor e relógios.**

   **NÃO PROCEDENTE — segundo escritor dos campos da própria tarefa encontrado fora de `_portas_da_tarefa`.** A publicação final está concentrada no `out.update`; o C17 continua calculando outra projeção, mas seu consumidor operacional encontrado é o instrumento de comparação histórica. (`app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/juizes.py:1709`, `app/chamados/management/commands/diff_e3_sombra.py:127`)

   **PROCEDENTE — relógio fora da borda no caminho dos baldes.** `_resolver_da_rodada` chama o calendário sem instante; a plural monta `contexto_calendario`, que lê `timezone.now()`. A oferta de Resolver dia usa o novo “hoje”, inclusive quando o command já forneceu seu instante. (`app/chamados/services/vigia_fio.py:270`, `app/colaboradores/services/calendario.py:132`, `app/colaboradores/services/calendario.py:163`, `app/colaboradores/services/calendario.py:103`)

   **PROCEDENTE — censos avulsos perdem `agora` ao criar `ListaDoAdmin`.** Ambos omitem o argumento; a lista repassa `None`, levando `cadastro_realidade.lista` a ler outro relógio. O command principal fornece a lista com `agora` corretamente. (`app/chamados/services/vigia_fio.py:391`, `app/chamados/services/vigia_fio.py:645`, `app/chamados/services/vigia_fio.py:216`, `app/escala/services/cadastro_realidade.py:775`, `app/chamados/management/commands/vigiar_fio_morto.py:175`)

5. **Selos, testes e mutantes.** [nao verificado]

   **NÃO PROCEDENTE — os mutantes novos dispensam controle verde.** O executor exige alvo textual único, teste original verde e morte por assertion sem erro ou skip; a substituição é removida em `finally`. (`app/chamados/tests/e3_degrau_mutantes.py:116`, `app/chamados/tests/e3_degrau_mutantes.py:130`, `app/chamados/tests/e3_degrau_mutantes.py:135`)

   Execuções em memória nesta sessão: suite nova **47 OK**; mutantes **34/34 mortos**; suites `test_e3_c8b_sem_banco` e `test_e3_tarefa_admin_sem_banco`, juntas, **32 testes, 10 falhas, zero erros**. Sem artefato persistido de resultado: [não verificado]. [nao verificado]

   **PROCEDENTE — permaneceram assertivas antigas contrárias ao comportamento LU atual.** Há testes exigindo `nada` para ações diretas do admin e outros exigindo rota de fila/pergunta para colab com app. (`app/chamados/tests/test_e3_c8b_sem_banco.py:56`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:128`, `app/chamados/tests/test_e3_portas_tarefa.py:427`, `app/chamados/tests/test_e3_portas_tarefa.py:512`, `app/chamados/tests/test_e3_portas_tarefa.py:537`)

   **Limite de L1–L10/C1–C15:** exercitam o juiz real com ações e pendências sintéticas; não demonstram a existência da tela administrativa. C1/C2 realmente impedem consulta ao prazo, mas explicitamente não provam vencimento C9. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:100`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:105`, `app/chamados/tests/e3_portas_apoio.py:29`)

   **NÃO PROCEDENTE — lacuna para alias de import ou atributo simples no selo AST.** O resolvedor trata alias e atributos; a detecção bloqueia o nome resolvido e possui contraprovas para alias e `J.porta_alcancavel_do_dia`. Também exige chamadas efetivas da carga e projeção. (`app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:33`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:42`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:73`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:83`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:107`)

   **Partição: prova uma por chamada de `censo_alcance`, não uma pelo command inteiro.** O teste agora exige `call_count==1` dentro de cada `_alc` e separa a tarefa avulsa. Porém o command chama `censo` e `censo_alcance`, cada qual com carga própria; com ambos os universos não vazios, cada carga executa sua partição. (`app/chamados/tests/test_o261e_f5_alcance.py:274`, `app/chamados/tests/test_o261e_f5_alcance.py:280`, `app/chamados/management/commands/vigiar_fio_morto.py:180`, `app/chamados/services/vigia_fio.py:410`, `app/chamados/services/vigia_fio.py:658`, `app/chamados/services/tarefa_do_dia.py:513`)

6. **Esmeril semântica.**

   | Valor | Significado implementado |
   |---|---|
   | `porta_app` | Colab com `RESPONDER` exigível e pergunta nos pendentes do app. (`app/chamados/services/tarefa_do_dia.py:603`, `app/chamados/services/tarefa_do_dia.py:624`) |
   | `porta_acao` | Admin vencedor sem porta administrativa reconhecida na fila. (`app/chamados/services/tarefa_do_dia.py:619`, `app/chamados/services/tarefa_do_dia.py:629`) |
   | `porta_fila` / `porta_pergunta` | Classificação ainda calculada pelo conjunto global das portas, causando o achado LU-07. (`app/chamados/services/tarefa_do_dia.py:627`) |
   | `fio='nada'` | Nenhum dono selecionado, com `nao_sei=False`; rota `None`. (`app/chamados/services/tarefa_do_dia.py:615`, `app/chamados/services/tarefa_do_dia.py:622`) |
   | `fio='nao_sei'` | Nenhum dono selecionado, com desconhecimento explícito; rota `None`. (`app/chamados/services/tarefa_do_dia.py:615`, `app/chamados/services/tarefa_do_dia.py:622`) |
   | `FIO_SEM_PORTA` | Constante `'nada'`; a Folha devolve-a quando a projeção é `None`, preservando `nao_sei` separadamente. (`app/chamados/juizes.py:1652`, `app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:248`) |
   | `fio='sistema'`, rota `None` | Exceção existente: sistema pode conservar dono interno sem porta publicada; os leitores da projeção recebem `None`. (`app/chamados/services/tarefa_do_dia.py:607`, `app/chamados/services/tarefa_do_dia.py:629`, `app/chamados/services/tarefa_do_dia.py:640`) |

   **PROCEDENTE — os vocabulários não são idênticos em todas as camadas.** O retrato transforma o grupo em fio agregado, usa `ninguem` e agrupa esse valor em Admin; não publica o mesmo objeto colab-dia da tarefa. Esse é o comportamento legado relacionado ao LU-05 deixado fora deste pouso. (`app/folha/retrato_folha.py:368`, `app/folha/retrato_folha.py:192`, `app/folha/retrato_folha.py:1190`)

7. **Comentários/docstrings contraditórios do diff — PROCEDENTE.**

   Permanecem textos dizendo que ação administrativa sem chamado na fila não tem porta, contrariando a inclusão direta de `ADMIN` e a publicação de `porta_acao`. Exemplos: comentário da fixture bloqueante; comentários dos testes de alcance; docstring que inclui ação direta administrativa em `nada`. (`app/chamados/tests/test_e3_bloqueantes.py:97`, `app/chamados/tests/test_o261e_f5_alcance.py:487`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:264`, `app/folha/tests/test_haiku_fora_da_folha.py:160`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`)

8. **Contraexemplo pela lei — PROCEDENTE, sintético; incidência em produção [não verificado].**

   Considere dia passado da competência atual, marco vencido apagado e pergunta respondida com linha E1, sem chamado da disputa nem `chamado_destino`. Os dois vínculos admitem NULL. `_pela_decisao` produz `validar/admin/None`, e a publicação resulta em **`fio='admin'`, `rota='porta_acao'`, `fila=()`, `verbos={}`**. (`app/chamados/models.py:1796`, `app/chamados/models.py:3494`, `app/chamados/services/tarefa_do_dia.py:140`, `app/chamados/services/tarefa_do_dia.py:150`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`)

   Entretanto, o modal exclui disputa sem chamado e pergunta sem âncora; os POSTs de validação exigem chamado correspondente. Resolver dia não aparece na competência atual. Escolhendo resposta fora do envelope da classe A, o lote também não fornece saída: classifica como B e recusa validação pelo lote. Logo, publica dono administrativo sem ação alcançável nas telas identificadas, contrariando a condição de LU-03. (`app/chamados/services/fio.py:211`, `app/chamados/services/fio.py:271`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`, `app/chamados/views.py:499`, `app/colaboradores/services/calendario.py:738`, `app/chamados/services/validacao.py:902`, `app/chamados/services/validacao.py:770`)

9. **Pouso e dependências.** [nao verificado]

   **NÃO PROCEDENTE — necessidade de migration para os novos campos de saída.** São campos de dict e memo da carga, sem coluna persistente nova nesses trechos. A tarefa, porém, depende da sequência E1; projeção sem linha continua produzindo `passivo_sem_linha`. (`app/chamados/services/tarefa_do_dia.py:522`, `app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/services/tarefa_do_dia.py:142`)

   O diff sombra está registrado como instrumento fora do pipeline, não como cron novo obrigatório. Aplicação das migrations, backfills prévios, equivalência do hash e existência de outra raia já pousada: **[não verificado]**. (`app/config/crons.py:769`, `app/config/crons.py:788`, `app/config/crons.py:798`)

## 14/11 19:51 · sessao a4657ec7ecfc · - · - · origem=Ronald (tmux)

**PROCEDENTE — suites antigas continuam vermelhas contra LU-02/03/07.** Executei com `PYTHONDONTWRITEBYTECODE=1`, sem settings, Django real, banco, logs ou arquivos de saída; resultados de execução não persistidos [não verificado]. [nao verificado]

- `test_e3_c8b_sem_banco` + `test_e3_tarefa_admin_sem_banco`: **32 testes, 10 falhas, zero erros**. Exigem `fio='nada'` apesar de ação administrativa direta, agora publicada como admin: `app/chamados/tests/test_e3_c8b_sem_banco.py:56`, `:172`, `:190`, `:256`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:128`. A regra atual inclui toda ação ADMIN: `app/chamados/services/tarefa_do_dia.py:606`.
- Executei por AST, sem importar suas fixtures ORM, três classes originais de `test_e3_portas_tarefa`: **21 testes, sete falhas, zero erros**. Assertivas antigas de rota do colab em `app/chamados/tests/test_e3_portas_tarefa.py:427`, `:474`, `:512`, `:537`; histórico dando porta em `:487`; verbo None dando porta em `:582`. Contradizem a implementação em `app/chamados/services/tarefa_do_dia.py:611`, `:625`. Contagem de execução [não verificado].

**NÃO PROCEDENTE — mutantes novos mortos sem controle verde.** A suite nova passou **47 testes**; `--todos` concluiu **34/34 mortos**, exit 0 [não verificado]. O executor exige alvo único, verde antes de cada mutação, falha de assertion sem erro/skip e restaura a sobrescrita em memória: `app/chamados/tests/e3_degrau_mutantes.py:116`, `:127`, `:135`, `:154`. As fontes mutadas ficam em dict: `app/chamados/tests/e4_catracas.py:5`.

**Limite dos selos novos:** L1–L10/C1–C15 testam o juiz real com ações/perguntas sintéticas; não comprovam existência de tela administrativa. `julgar` injeta `dia_plano` e pendentes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:100`; formato sintético: `app/chamados/tests/e3_portas_apoio.py:29`. C1/C2 realmente armam traps de prazo local e no módulo motor e exigem ausência de chamada, mas explicitamente não provam vencimento C9: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:105`.

**NÃO PROCEDENTE — selo AST ignora alias ou atributo simples.** Resolve `from ... import funcao as apelido`, imports de módulo e cadeias de atributos: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:30`, `:38`. Bloqueia atributos e nomes resolvidos: `:69`, `:73`. Os contraprovas incluem alias e `J.porta_alcancavel_do_dia`: `:94`, `:106`, `:107`; exige chamadas efetivas da carga/projeção: `:83`.

**PROCEDENTE se “rodada” significa o command completo; NÃO PROCEDENTE para uma chamada de `censo_alcance`.** O teste passa a exigir exatamente uma partição dentro de cada `_alc`, separando depois a tarefa avulsa: `app/chamados/tests/test_o261e_f5_alcance.py:274`, `:280`, `:282`. Entretanto `_alc` chama apenas `censo_alcance`: `:116`. O command chama **censo e censo_alcance**: `app/chamados/management/commands/vigiar_fio_morto.py:180`. Ambos constroem carga própria: `app/chamados/services/vigia_fio.py:410`, `:658`. Cada carga executa sua partição: `app/chamados/services/tarefa_do_dia.py:513`. Assim, com ambos os universos não vazios, há duas partições no command, embora uma por carga. O comentário “por carga/rodada” amplia indevidamente o alcance: `app/chamados/tests/test_n1_alimentacao_unica.py:105`.

**PROCEDENTE — comentários/docstrings contraditórios remanescentes do diff:**

- “Sem porta da fila … dia não tem porta de ninguém”: `app/chamados/tests/test_e3_bloqueantes.py:97`; contraria ADMIN direto em `app/chamados/services/tarefa_do_dia.py:606`, `:629`.
- “admin só chega ao dia por chamado NA FILA”: `app/chamados/tests/test_o261e_f5_alcance.py:487` e `app/chamados/tests/test_o261e_f5b_juiz_porta.py:264`; mesma contradição.
- "`nada` inclui … ação direta do admin sem chamado na fila": `app/folha/tests/test_haiku_fora_da_folha.py:160`; mesma contradição.
- Comentários nas assertivas antigas que chamam validar direto de inalcançável: `app/chamados/tests/test_e3_c8b_sem_banco.py:56`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:128`.

**Pouso:** não executei migrations, cron nem Django. Os novos campos são projeção em dict e memo de instância (`app/chamados/services/tarefa_do_dia.py:630`, `:522`), sem escrita persistente nesses trechos. Ausência absoluta de dependência de outra raia ou migration no hash não pode ser certificada sem histórico Git; esta cópia não tem `.git` [não verificado].

## 14/11 19:50 · sessao 6656d8ac9316 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **PROCEDENTE — rota do admin ainda pode ser escolhida pela porta de outro dono.** `_portas_da_tarefa` filtra `portas_vivas` para o admin, mas escolhe `porta_fila` usando `da_celula` e `portas` globais. Reproduzi, sem escrita, ação `validar/admin/202`, pergunta 303 ancorada em 202, chamado 101 da célula com `cobrar`: saída `fio=admin`, `rota=porta_fila`, `fila=(101,)`, `verbos={101:cobrar,202:validar}`. A porta do admin é 202, via pergunta; o chamado que força a classificação de fila é 101, cujo cobrar não dá dono. Não constatei link quebrado: a enumeração não é convertida diretamente em URL pelos leitores encontrados. `app/chamados/services/tarefa_do_dia.py:591`, `:599`, `:611`, `:619`, `:627`.

- **PROCEDENTE — os baldes do vigia continuam decidindo além da porta publicada.** Com porta ausente, `dono_fora_do_fio` ainda classifica “Resolver dia”, ausência aberta, aceite de folga e dono da lista; os dois censos chamam essa cadeia. Portanto, não são somente leitura de `porta_da_tarefa`. Os baldes têm chamadores; não ficaram mortos. `app/chamados/services/vigia_fio.py:306`, `:308`, `:311`, `:313`, `:440`, `:442`, `:661`, `:683`.

- **PROCEDENTE — relógios adicionais no caminho dos baldes.** `_resolver_da_rodada` chama o calendário sem instante; a plural monta `contexto_calendario`, que lê `timezone.now()`. O resultado usa o “hoje” desse novo instante para oferecer Resolver dia. Isso ocorre também no command principal. `app/chamados/services/vigia_fio.py:270`; `app/colaboradores/services/calendario.py:103`, `:117`, `:132`, `:159`, `:163`. Há outra leitura para desfazer batida em `app/colaboradores/services/calendario.py:250`.

- **PROCEDENTE — censos avulsos perdem o instante ao criar a lista.** Mesmo recebendo `agora`, ambos criam `ListaDoAdmin` sem ele; a lista repassa `None` e Cadastro × Realidade lê outro relógio. O command principal passa a lista com instante corretamente. `app/chamados/services/vigia_fio.py:391`, `:645`, `:216`; `app/escala/services/cadastro_realidade.py:775`; `app/chamados/management/commands/vigiar_fio_morto.py:175`, `:180`.

- **PROCEDENTE — função morta de produção.** `vigia_fio.fio_do_dia` permanece definida, mas a busca de chamadas em código de produção só encontrou a definição; os censos agora usam a porta publicada. `app/chamados/services/vigia_fio.py:156`, `:428`, `:669`. A ausência de outros chamadores é resultado da busca estática; execução dinâmica: **[não verificado]**.

- **NÃO PROCEDENTE — `porta_acao` causar KeyError ou desaparecer dos contadores identificados.** O alcance inicializa as quatro rotas, incrementa pela enumeração publicada e imprime app/ação; furo conta as quatro rotas e o command as imprime. `app/chamados/services/vigia_fio.py:587`, `:588`, `:673`, `:741`; `app/chamados/services/furo_sem_canal.py:114`; `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`. Integração em produção: **[não verificado]**.

Mapa dos campos e consumidores:

| Elemento | Escritor/leitor |
|---|---|
| `fio`, `rota`, `fila`, `verbos` da tarefa | Escritor final único encontrado: `out.update`, `app/chamados/services/tarefa_do_dia.py:630`; projeção em `:640`. |
| `donos` | Variável local do juiz, não campo publicado: `app/chamados/services/tarefa_do_dia.py:608`, `:615`, `:630`. |
| Folha | Consome porta e fio; sem porta devolve `FIO_SEM_PORTA`; lê fila para números de chamados. `app/folha/fora_da_folha.py:67`, `:82`, `:193`, `:238`, `:244`. |
| Retrato | Recebe agregação da Folha, grava fio na chave histórica `dono`, lê esse valor; não lê enumeração `rota`. `app/folha/retrato_folha.py:1158`, `:1190`, `:438`. |
| Links do retrato | Deriva links dos mapas de fio/tipo; admin tem `None` no mapa de fio, e tipos falta/não bate/férias também não possuem link próprio. `app/folha/retrato_folha.py:974`, `:986`, `:1015`; template mostra os links fornecidos em `app/templates/folha/_retrato_o_que_fazer.html:66`. |
| Pauta dos discordantes | Usa o fio projetado e compara com `FIO_SEM_PORTA`. `app/ponto/services/pautas_dos_discordantes.py:64`, `:79`, `:81`. |
| Cadastro × Realidade | Exclui o beco quando existe porta, independentemente da enumeração. `app/escala/services/cadastro_realidade.py:464`, `:469`. |
| Furo sem canal | Presença da porta define canal; enumeração alimenta os quatro contadores. `app/chamados/services/furo_sem_canal.py:97`, `:104`, `:114`. |
| Vigia | Presença da porta define fio vivo; alcance lê rota/fio diretamente, depois aplica os próprios baldes se não houver porta. `app/chamados/services/vigia_fio.py:428`, `:669`, `:673`, `:683`. |
| API do app | Consulta `consulta_do_app`; não consome a tarefa publicada nesses endpoints. `app/api/views.py:993`, `:994`, `:2533`, `:2534`. |
| Diff sombra | Lê fio/fila/rota e presença da porta; conserva C17 como comparação histórica. `app/chamados/management/commands/diff_e3_sombra.py:49`, `:53`, `:56`, `:127`, `:143`. |

Semântica: tarefa `nada`/`nao_sei` publica rota `None`; `porta_da_tarefa` então devolve `None`, e a Folha reduz ambos a `FIO_SEM_PORTA='nada'`, preservando `nao_sei` separadamente. Sistema pode conservar fio `sistema` com rota `None`, sendo igualmente reduzido a sem porta pelos leitores. `app/chamados/services/tarefa_do_dia.py:615`, `:622`, `:629`, `:640`; `app/folha/fora_da_folha.py:85`, `:248`; `app/chamados/juizes.py:1652`.

C17 continua sendo segundo derivador no código, mas seu chamador de produção encontrado é o instrumento de comparação, não os leitores migrados. `app/chamados/juizes.py:1664`, `:1709`; `app/chamados/management/commands/diff_e3_sombra.py:127`. A agregação do retrato é outra unidade — colaborador/competência, com fallback legado — e conserva o agrupamento sem porta→Admin declarado fora deste pouso. `app/folha/retrato_folha.py:192`, `:343`, `:368`, `:441`, `:1190`.

## 14/11 19:50 · sessao 4923414193e6 · - · - · origem=Ronald (tmux)

**PROCEDENTE — `porta_acao` pode nascer sem tela capaz de executar a ação publicada.** Basta pergunta respondida com linha E1, marco vencido apagado, sem `chamado_destino` nem chamado da disputa, em dia da competência atual e resposta fora da classe A: ambos os vínculos aceitam NULL; `_pela_decisao` produz `validar/admin/None`; `_portas_da_tarefa` publica `admin/porta_acao`. (`app/chamados/models.py:1796`, `app/chamados/models.py:3494`, `app/chamados/services/tarefa_do_dia.py:140`, `app/chamados/services/tarefa_do_dia.py:150`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`.)

O modal carrega somente disputas com chamado e descarta perguntas sem âncora; o POST exige chamado real e correspondência da âncora. O detalhe impõe a mesma correspondência. (`app/chamados/services/fio.py:211`, `app/chamados/services/fio.py:271`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`, `app/chamados/views.py:493`, `app/chamados/views.py:499`.)

O lote não salva esse contraexemplo: resposta além do envelope é classe B; o escritor do lote recusa tudo fora de A, e a prévia apresenta as classes externas apenas por contagem. Resolver dia tampouco aparece na competência atual. (`app/chamados/services/validacao.py:902`, `app/chamados/services/validacao.py:770`, `app/templates/chamados/partials/_lote_preview.html:15`, `app/colaboradores/services/calendario.py:738`, `app/colaboradores/services/calendario.py:103`.) Incidência em produção: **[não verificado]**.

**NÃO PROCEDENTE — âncora terminal/silenciada, isoladamente, torna a ação inalcançável.** O fio administrativo carrega todos os chamados da pessoa, sem `qs_na_fila`; o POST de validar não exige fila. Havendo container e pergunta listada, existe botão no modal e execução pelo endpoint. (`app/chamados/services/fio.py:204`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`, `app/templates/chamados/partials/modal_fio.html:279`.)

**PROCEDENTE — a Folha pode dizer “admin agir” sem oferecer link para a ação.** `o_que_segura` conserva fio e frase, mas descarta rota; no retrato, `PORTA_DO_FIO['admin']` é `None`, assim como tipos `nao_bate`, `falta` e `ferias`; a combinação devolve lista vazia, e o template só mostra botões com `retrato_portas`. Não há KeyError nesse caminho: há omissão de link. (`app/folha/fora_da_folha.py:238`, `app/folha/fora_da_folha.py:246`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:990`, `app/folha/retrato_folha.py:1015`, `app/templates/folha/_retrato_o_que_fazer.html:66`.)

Telas existentes para ações legítimas: validar no modal do fio, decidir ausência no painel da ausência, Resolver dia no calendário condicionado ao juiz de oferta e à permissão. Nenhuma delas interpreta a string `porta_acao` para selecionar tela. (`app/templates/chamados/partials/modal_fio.html:279`, `app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344`, `app/templates/colaboradores/partials/_calendario_grade.html:131`, `app/colaboradores/services/calendario.py:97`.)

## 14/11 19:41 · sessao baf83260c678 · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

Cura implementada em [tarefa_do_dia.py:573](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/services/tarefa_do_dia.py:573): LU-02, LU-03 e LU-07 ficam no juiz único. A certificação pela suite/opus permanece pendente **[nao verificado]**. [nao verificado]

A execução local terminou com **76 testes OK**, **34/34 mutantes mortos**, controles verdes e nenhum erro nos alvos. Os dois mutantes legados adaptados também retornaram RC 1. Referências: `chamados/tests/test_e3_degrau_dono_e_porta.py:41`, `chamados/tests/e3_degrau_mutantes.py:127`, `chamados/tests/e3_portas_v6_mutantes.py:90`, `chamados/tests/e3_volta7_mutantes.py:123`.

O diff por arquivo e função ficou assim:

| Arquivo::função — referência | Alteração |
|---|---|
| `chamados/services/tarefa_do_dia.py:573::_portas_da_tarefa` | Exclui residual histórico/None; aceita ação direta administrativa; publica a rota do dono. |
| `chamados/services/tarefa_do_dia.py:633::porta_da_tarefa` | Documenta a projeção vigente; mantém leitura dos cinco campos. |
| `chamados/services/vigia_fio.py:318::censo` | Consome a porta da tarefa antes dos baldes. |
| `chamados/services/vigia_fio.py:553::censo_alcance` | Acrescenta `alcance_porta_acao`; publica o contador no log. |
| `chamados/services/furo_sem_canal.py:43::medir` | Acrescenta `com_porta_acao`. |
| `chamados/management/commands/lavrar_furo_sem_canal.py:29::handle` | Exibe o contador de ação direta. |
| `chamados/management/commands/vigiar_fio_morto.py:180::handle` | Encaminha o mesmo `agora` aos dois censos. |
| `folha/fora_da_folha.py:38::GRUPOS/DESTRAVA` | Rótulo administrativo passa a incluir ação direta. |
| `folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:26::chamadas/violacoes` | Confere chamadas, inclusive aliases; exige chamadas à carga/projeção. |
| `chamados/tests/test_e3_degrau_dono_e_porta.py:41::DegrauDonoEPorta` | Matriz L1–L10, caracterizações invertidas, consumidores e traps de prazo. |
| `chamados/tests/e3_degrau_mutantes.py:17::MUTANTES` | Quatro mutantes pedidos, equivalentes vivos de P1 e cobertura dos leitores alterados. |
| `chamados/tests/test_o261e_f5_alcance.py:122::_soma` | Inclui `porta_acao` na soma L-110; P3 exige uma chamada à partição por rodada. |
| `chamados/tests/test_n1_alimentacao_unica.py:104` | Corrige docstrings para carga em lote, com os IDs LU. |
| `chamados/tests/test_e3_portas_tarefa.py:48` | Ajusta expectativas de rota do colab e residual histórico. |
| `chamados/tests/test_e3_portas_v6_sem_banco.py:45` | Ajusta a prioridade da ação administrativa direta. |
| `chamados/tests/test_o261e_f5b_juiz_porta.py:232` | Preserva fila/verbos e exige a nova porta administrativa. |
| `chamados/tests/test_e3_diff_sombra.py:170` | Exige a diferença de rota A/B prevista por LU-07. |
| `chamados/tests/e3_portas_v6_mutantes.py:63` e `chamados/tests/e3_volta7_mutantes.py:24` | Invertem os mutantes legados para negar a ação direta. |

A matriz foi executada **antes da alteração da regra**, com este comando:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e3_degrau_dono_e_porta.DegrauDonoEPorta -k test_L
```

Trecho da saída RED inicial:

```text
test_L1_historico_sem_porta ... FAIL
test_L2_verbo_nulo_sem_porta ... FAIL
test_L4_admin_direto_tem_porta_e_folha_le_admin ... FAIL
test_L5_admin_direto_com_cobrar ... FAIL
test_L6_rota_colab_com_decidir_na_celula ... FAIL
test_L7_rota_colab_com_acao_admin_na_fila ... FAIL
Ran 10 tests in 1.069s
FAILED (failures=6)
```

Os resultados antes/depois, em `(fio, rota)`, foram:

| Caso e referência no teste | Fase RED | GREEN | [nao verificado]
|---|---|---|
| L1 — `test_e3_degrau_dono_e_porta.py:41` | FAIL: `chamado, porta_fila` | `nada, None`; projeção None |
| L2 — `:47` | FAIL: `chamado, porta_fila` | `nada, None`; projeção None |
| L3 — `:53` | OK: `chamado, porta_fila` | Preservado |
| L4 — `:57` | FAIL: `nada, None` | `admin, porta_acao`; Folha lê admin |
| L5 — `:67` | FAIL: `nada, None` | `admin, porta_acao` |
| L6 — `:72` | FAIL: `colab, porta_fila` | `colab, porta_app` |
| L7 — `:76` | FAIL: `colab, porta_fila` | `colab, porta_app` |
| L8 — `:81` | OK: `admin, porta_fila` | Preservado |
| L9 — `:86` | OK: `admin, porta_fila` | Preservado |
| L10 — `:91` | OK: `nada, None` | Preservado |

L9 conserva `porta_fila`: o residual `validar` fornece uma porta administrativa na célula, que prevalece sobre a rota da ação direta. Já ação administrativa direta com residual `decidir` usa `porta_acao`, sem converter o residual em admin: `chamados/services/tarefa_do_dia.py:619`, `chamados/tests/test_e3_degrau_dono_e_porta.py:96`.

O censo de leitores foi feito antes da primeira gravação, usando `git grep -n` pelos termos solicitados. O tratamento de `porta_acao` é:

| Leitor — referência | Tratamento |
|---|---|
| `folha/fora_da_folha.py:37`, `:82`, `:238` | `FIOS` continua sendo vocabulário de fio; lê `admin` diretamente. Rótulo atualizado em `:45`; chamados continuam vindo de `fila`, em `:244`. |
| `folha/retrato_folha.py:192`, `:974`, `:1015`, `:1069` | Consome fio/grupo e rótulo. Não discrimina a rota da tarefa; links continuam pelo mapa de fio/tipo. `PORTA_DO_FIO['admin']` permanece None. |
| `chamados/services/furo_sem_canal.py:97`, `:117` | Reconhece a presença da porta e conta `com_porta_acao`. |
| `chamados/services/vigia_fio.py:412`, `:428` | `censo()` lê a projeção e conta em `fio_vivo`; não separa por rota. |
| `chamados/services/vigia_fio.py:588`, `:673`, `:740` | `censo_alcance()` inicializa, conta e imprime `alcance_porta_acao`. Soma cobrada em `chamados/tests/test_o261e_f5_alcance.py:124`. |
| `escala/services/cadastro_realidade.py:469` | Testa presença da projeção; qualquer porta publicada exclui o dia do beco. |
| `ponto/services/pautas_dos_discordantes.py:79` | Lê indiretamente o fio da Folha; admin deixa de ser sem fio. |
| `chamados/management/commands/diff_e3_sombra.py:46` | Projeta rota/fio/fila e testa presença, sem enumeração fechada de rotas. |
| `chamados/management/commands/lavrar_furo_sem_canal.py:37` | Exibe o contador novo. |

Os escritores e consumidores dos campos permanecem identificáveis:

| Campo | Escrita/fornecimento | Leitura |
|---|---|---|
| `rota`, `fio` | Seleção e publicação únicas em `chamados/services/tarefa_do_dia.py:615`, `:630` | Projeção `:641`; consumidores no censo acima. |
| `verbos` | Painel/alimentação em `chamados/juizes.py:1570`; seleção na tarefa em `tarefa_do_dia.py:601` | Residual em `:611`; projeção em `:641`; instrumento em `diff_e3_sombra.py:144`. |
| `fila` | Alimentação em `chamados/juizes.py:1573`; seleção publicada em `tarefa_do_dia.py:602` | Folha em `folha/fora_da_folha.py:244`; instrumento em `diff_e3_sombra.py:143`. |
| Projeção C17 histórica | `chamados/juizes.py:1709` | Lado anterior do comparador, `diff_e3_sombra.py:127`; não foi alterada. |

Nos baldes do vigia, os dias com ação administrativa já publicada pela tarefa passam antes dos baldes: `censo()` em `vigia_fio.py:428`; `censo_alcance()` em `:669`. [nao verificado]

- **Decidir ausência:** quando a tarefa expõe a ação, sai de `aguarda_decisao_de_ausencia` e ganha porta administrativa. Fontes: `tarefa_do_dia.py:174`, `:305`, `:606`.
- **A/S validar:** quando a tarefa publica `validar` sem chamado, ganha `porta_acao`. Fonte: `tarefa_do_dia.py:352`.
- **Resolver dia:** dias que também tenham ação administrativa na tarefa saem de `decisao_do_dia_pendente`. A oferta do calendário sozinha continua podendo alcançar o balde; não foi criada ação nova neste lote. Fontes: `vigia_fio.py:306`, `:442`, `:683`.
- **Aguardando documento:** continua sem dono por essa ação, preservando L10/LU-04 fora do lote. Fontes: `tarefa_do_dia.py:299`, `test_e3_degrau_dono_e_porta.py:91`.

Os dois baldes conservam chamadores condicionados à ausência de porta. Quantos dias restam em cada um, ou se algum fica vazio na frota: **[nao verificado]**.

Para reproduzir as verificações:

```bash
# Reproduz os seis REDs contra a base fornecida; RC esperado 1.
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --base

# GREEN final: 76 testes OK. [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -q \
  chamados.tests.test_e3_degrau_dono_e_porta \
  chamados.tests.test_e3_pos_suite_sem_banco \
  chamados.tests.test_e3_portas_v6_sem_banco \
  chamados.tests.test_e3_volta7_sem_banco

# Concluiu: MORTOS=34 TOTAL=34; RC 0.
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
```

O runner imprime o comando individual antes de cada controle verde; falha do controle levanta `AssertionError`: `chamados/tests/e3_degrau_mutantes.py:132`. Os quatro pedidos e os equivalentes vivos de P1 são:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante historico_da_dono
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante acao_direta_sem_dono
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante rota_colab_pela_fila
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante porta_acao_some
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante sem_memo_por_colab
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante carga_sem_relogio

# Legados adaptados; ambos RC 1.
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_portas_v6_mutantes --mutante fio_acao_sem_porta_ignorada
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_volta7_mutantes --mutante fio_v6
```

P2 agora acusa chamadas proibidas inseridas em memória, inclusive por alias: `folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:90`. P4 usa traps de prazo, sem alegar vencimento C9: `test_e3_degrau_dono_e_porta.py:105`. P3 exige `call_count == 1` por rodada, mas sua execução com fixture ORM permanece **[nao verificado]**: `test_o261e_f5_alcance.py:264`.

Ficaram fora LU-01, LU-04, LU-05 e LU-06: residual `decidir` continua `chamado` (`test_e3_degrau_dono_e_porta.py:53`); documento sem porta continua sem dono (`:91`); agrupamento `ninguem` permanece (`folha/retrato_folha.py:192`); universo da carga não foi alterado. Integração ORM, remedida e totais novos de produção: **[nao verificado]**. Registro sem construção: **PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA**, número informado no anexo **2.333**; remedida **[nao verificado]**.

## 14/11 19:27 · sessao c4cb8db0cbc8 · BO-DISPUTA-SEM-PERGUNTA · leitura · origem=Code

Referências relativas a `app/`, salvo indicação contrária. **Cópia `arquivo:+N`** significa a linha do lado novo do `diff_obra.patch` fornecido. “Push sim” significa tentativa de envio; entrega real e execução dos testes citados: **[nao verificado]**. Os testes novos da cópia foram apenas listados no anexo; sua cobertura: **[nao verificado]**. [nao verificado]

Estados: **a** ata lavrada; **b** sem ata; **c** sem marcos/lâmpadas na ata; **d** pergunta respondida; **e** pergunta morta pelo C6. Respondida pode continuar viva, mas deixa de ser pendência do colaborador no app (`chamados/juizes.py:1417`; `chamados/services/pergunta_no_app.py:71`).

**Porta 1 — criar ou atualizar disputa**

`chamados/services/disputa_emissao.py::criar_ou_atualizar_disputa:644` → `_abrir_ou_agregar_disputa:474`. Chamadores: views de batida `api/views.py:764`, `api/views_core.py:758`; triagem `ponto/services/triagem_batida.py:735`; command `marcar_foto_ausente_retro.py:96`; fábrica da célula `chamados/services/fabrica_da_celula.py:172`. Esta última é chamada pelo cartório, reconciliador e backfill (`ponto/services/cartorio.py:1045`; `chamados/reconciliador.py:1387`; `chamados/management/commands/backfill_fio_morto_e4b.py:143`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| N0 — motivos vazios; qualquer estado | Nenhum; guarda local | Nada | Não | `disputa_emissao.py:656` | Guarda preservada. |
| N1 — a; DS inexistente ou somente fechadas | Nenhum para nascer; procura apenas DS aberta | Cria/reusa chamado-pai; cria `DS.motivos`, `contexto`, `chamado` **antes** da geração | Sim, ao colaborador, mesmo se o gerador não entregar pergunta local | `disputa_emissao.py:531`, `:568`, `:587`, `:669` | Nascimento passa por ensaio; sem pergunta local admitida, desfaz o savepoint. Reuso estrangeiro pode ser repetido na dona (`disputa_emissao.py:+435`). |
| N2 — b/c; DS inexistente ou somente fechadas | Nascimento não consulta ata; geração usa contexto/grade conforme insumo disponível | DS/pai já persistem. Pode criar pergunta com data do contexto; pode terminar sem pergunta | Sim, ao colaborador | `disputa_emissao.py:345`, `:350`, `:360`, `:371`, `:425`, `:669` | Resolvedor comum e ensaio evitam parte dos nascimentos vazios. **“Pergunta local admitida” não significa C6 viva**: `locais()` exclui vias, sem consultar C6 (`disputa_emissao.py:+435`). |
| N3 — a/b/c; DS aberta | Nenhum para agregar; união e merge locais | Atualiza `DS.motivos/contexto`; contexto registra `qtd/ultima_em` | Ao colaborador somente com motivo novo; outro dia do mesmo motivo pode gerar pergunta sem push | `disputa_emissao.py:535`, `:548`, `:565`, `:682`, `:685`, `:686` | Agrega motivos locais; encaminha estrangeiros à dona. Contexto continua podendo ser salvo antes da geração (`disputa_emissao.py:+387`, `:+408`, `:+435`). | [nao verificado]
| N4 — d/e na própria DS | Dedup local por existência; nenhum C6 nesse predicado | Pode atualizar DS como N3, sem criar pergunta nova | Regra N1/N3; resposta ou morte não são guarda desse push | `disputa_emissao.py:325`, `:398`, `:417`, `:669`, `:686` | Dedup por existência permanece; efeito passa pela pendência do app (`disputa_emissao.py:+213`, `:+1828`; `pergunta_no_app.py:+84`). | [nao verificado]
| N5 — pergunta canônica pertence a outra DS aberta/fechada | Identidade do fato; juiz da resolvedora e guarda da costura | Pode reutilizar/atualizar pergunta estrangeira e deixar a DS nova sem filhos; `_criada` não governa o retorno do gerador | Pode enviar ao colaborador mesmo com DS local vazia | `disputa_emissao.py:444`, `:470`; `chamados/models.py:2977`, `:3035`, `:3064` | Distingue `ReusadaDeOutraDS`; desfaz nascimento sem pergunta local; agrega na dona aberta. Na fechada, não salva motivo: registra descarte após commit (`disputa_emissao.py:+435`; `descarte_disputa.py:+5`). |

**Porta 2 — gerar/reaproveitar perguntas e entradas pendentes**

`chamados/services/disputa_emissao.py::gerar_perguntas_disputa:300`; `::perguntar_entradas_pendentes:257`. Chamadores internos: wrapper e agrupador (`disputa_emissao.py:670`, `:682`, `:685`, `:1270`). Há geração adicional pela fábrica e pelo command smoke (`chamados/services/fabrica_da_celula.py:174`; `chamados/management/commands/smoke_chamados.py:103`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| G1 — a/b/c; texto/data resolvíveis | GEOMETRIA DE TURNO para batida; dados da célula/contexto; destino derivado separadamente | `PerguntaDisputa.abrir` cria ou retorna canônica; grava dia/destino/célula/lavra conforme nascimento ou reuso | Nenhum dentro do gerador; wrapper decide | `disputa_emissao.py:345`, `:350`, `:371`, `:391`, `:411`, `:444` | `_resolver_pergunta_por_motivo` resolve dia antes de texto/dedup/destino (`disputa_emissao.py:+213`). |
| G2 — sem dia para pergunta de hora | Nenhum; ausência local de insumo | Nenhuma pergunta; não desfaz DS já criada pelo wrapper | Wrapper ainda pode enviar | `disputa_emissao.py:424`, `:425`, `:430` | Causa `InsumoAusente`; núcleo pode desfazer o nascimento (`disputa_emissao.py:+213`, `:+435`). |
| G3 — d/e local, mesma identidade, fora das vias excluídas | Nenhum; existência local | Não cria; dedup não lê resposta nem C6 | Nenhum próprio | `disputa_emissao.py:325`, `:398`, `:417` | Predicado permanece no resolvedor (`disputa_emissao.py:+213`). |
| G4 — canônica de outra DS, aberta ou fechada | Porta do fato `PerguntaDisputa.abrir` | Retorna a pergunta estrangeira; não transfere sua `disputa` | Nenhum próprio | `chamados/models.py:2833`, `:2977`, `:3064`; `disputa_emissao.py:470` | Retorno recebe causa própria e argumentos para replay; wrapper trata propriedade da pergunta (`disputa_emissao.py:+369`, `:+435`). |
| G5 — lista de entradas pendentes | GEOMETRIA DE TURNO / `dia_do_turno_da_batida`; FURO DO DIA / `classificar_falta` | Injeta contexto por dia, chama gerador; salva `DS.contexto` quando há retorno | Nenhum próprio | `disputa_emissao.py:282`, `:288`, `:291`, `:295`, `:296` | Restaura contexto em memória, agrega conforme dona local/estrangeira e coleta criadas (`disputa_emissao.py:+136`). |
| G6 — fábrica da célula | CÉLULA/PRECEDÊNCIA; motivos pela ata | Desfaz carimbo sem lastro; chama wrapper e depois chama gerador novamente se recebeu DS | Silenciado no julgamento da célula | `chamados/services/fabrica_da_celula.py:139`, `:169`, `:172`, `:174`, `:218` | Arquivo não alterado no diff; chamada adicional permanece. |

O contrato final do dia já existe no hash: `_sob_contrato` exige saída com dia do juiz e desfaz a chamada se o contrato falhar; isso não uniformiza as decisões anteriores do gerador (`chamados/models.py:2862`; `chamados/services/fio.py:1140`). Testes dessa porta: `chamados/tests/test_e1_v15e_contrato_de_saida.py`, referenciado em `chamados/models.py:2872`; teste de derivação pelo dia da pergunta em `chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:80`.

**Porta 3 — órfãos, `orfao_14h` e complementação**

`chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`; auxiliares `::_criar_pergunta_se_couber:823`, `::_complementar_marcos_faltantes:850`, `::_ata_do_dia:887`.

Chamadores: commands `processar_alertas_turno.py:385`, `processar_alertas_avancados.py:159`, `detectar_ausencias.py:803`, agendados respectivamente em `*/5`, `*/30`, `*/5` (`config/crons.py:362`, `:363`, `:364`); ação de view `chamados/services/acoes_disputa.py:222`; cobrar-dia `chamados/services/cobrar_dia.py:49`; passivo `chamados/services/pergunta_no_app.py:246`, acionado pelo command `perguntar_chamados_sem_pergunta.py:20`.

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| O1 — elegível; DS inexistente/somente fechadas | Nenhum para nascer; busca DS aberta | Cria pai e DS antes das guardas; motivo inicial `orfao_14h` | Depende dos ramos seguintes | `disputa_emissao.py:1096`, `:1101`, `:1123` | Nascimento antecipado permanece. Rollback específico: nova + retorno estrangeiro + nenhuma pergunta local (`disputa_emissao.py:+1146`). |
| O2 — a, marco apagado, rota por ata; DS aberta/nova | DIA DO CHAMADO / `data_do_chamado`; leitura dos marcos/lâmpadas | Cria/reusa pergunta por marco; agrega `DS.motivos`; chamado vai a análise | Sim ao colaborador, se não fechar e `notificar` | `disputa_emissao.py:1166`, `:1170`, `:1175`, `:873`, `:837`, `:1289` | Encaminha estrangeira à dona; push exige pendentes da DS e DS ainda aberta (`disputa_emissao.py:+1146`, `:+1363`). | [nao verificado]
| O3 — b, com escala vigente; DS aberta/nova | `_marcos_faltantes`; guarda local de `sem_celula` e escala vigente | DS/pai ficam; nenhuma pergunta; abstenção impede auto-fecho | **Sim ao colaborador no hash**, se `notificar` | `disputa_emissao.py:902`, `:908`, `:911`, `:1279`, `:1289`; teste espera DS aberta com zero perguntas: `chamados/tests/test_hx_disputa_muda_sem_ata.py:51` | **DS aberta vazia permanece.** Novo filtro suprime push sem pendentes (`disputa_emissao.py:+1363`). |
| O4 — c: ata de trabalho sem pares de marco/lâmpada | Leitor devolve `sem_celula` | Mesmo resultado de O3, havendo escala vigente | Mesmo resultado de O3 | `chamados/services/declaracao_texto.py:121`, `:126`; `disputa_emissao.py:902` | Predicado preservado; efeito filtrado como O3. |
| O5 — b, sem escala vigente | FURO DO DIA; exceção local da guarda de ata | Permite pergunta por batidas; não exige ata impossível dessa escala | Sim se permanece aberta | `disputa_emissao.py:908`, `:910`, `:1200`, `:1205`, `:1213` | Exceção preservada no hunk do agrupador (`disputa_emissao.py:+1146`). |
| O6 — falha ao ler ata | Nenhum; `except` local | `_ata_do_dia` devolve `True, None`; não bloqueia pergunta | Pode enviar se permanece aberta | `disputa_emissao.py:897`, `:901` | Ramo não alterado. |
| O7 — a, sem falta acusada; DS aberta/nova | FURO DO DIA; depois DISPUTA FECHA / C7 | Pode ter criado DS/pai; fecha DS se C7 permite e não houve abstenção | Não depois do auto-fecho | `disputa_emissao.py:1201`, `:1238`, `:1278`, `:1282`, `:1284`; `chamados/tests/test_hx_disputa_muda_sem_ata.py:63` | Conserva auto-fecho e exceção por falta de ata (`disputa_emissao.py:+1146`). |
| O8 — d, pergunta ainda viva; DS aberta | C6 por `fio_vivo` | `_ja_perguntou` bloqueia nova; resposta permanece | Hash não exige que o colaborador ainda deva resposta | `disputa_emissao.py:1415`, `:1422`, `:832`, `:1289` | Push exige pendência do app, que exclui respondida (`disputa_emissao.py:+1363`; `pergunta_no_app.py:71`). | [nao verificado]
| O9 — e, colaborador ativo | C6 por `fio_vivo` | Permite tentar `abrir`; canônica pode ser reutilizada em vez de nascer outra | Depende de permanecer aberta no hash | `disputa_emissao.py:1415`, `:1422`, `:837`; `chamados/models.py:2977` | Trata retorno estrangeiro e filtra push (`disputa_emissao.py:+840`, `:+1363`). |
| O10 — e, colaborador não ativo | Nenhum; guarda local de existência | Linha existente bloqueia geração antes de consultar C6 | Depende das demais perguntas/fluxo | `disputa_emissao.py:1416`, `:1420` | Predicado não alterado. |
| O11 — intervalo explícito; a/b/c | Nenhum juiz de ata nesse ramo; lê `contexto.marcos_faltantes` | Pergunta por marco com data do contexto e destino `_chi`; não passa por `_ata_do_dia` | Sim se permanece aberta | `disputa_emissao.py:1138`, `:1155`, `:1289` | Fotografa fatos em `_marcos_de_intervalo`; destino passa pelo eleitor comum (`disputa_emissao.py:+1146`; `fio.py:+752`). |
| O12 — chamado já tem pergunta não materializada; dona aberta **ou fechada** | Proxy SQL `materializada_em IS NULL`; depois leitor da ata | Complementa e pode salvar `DS.motivos` da dona fechada | Pode retornar antes do push principal | `disputa_emissao.py:1070`, `:1073`, `:1081`, `:1084`, `:1093` | Agregador não salva dona fechada; registra descarte. Complementos só notificam dona aberta (`disputa_emissao.py:+1092`, `:+408`; `descarte_disputa.py:+5`). | [nao verificado]
| O13 — sem origem e sem data/base | DIA DO CONTEXTO; GEOMETRIA DE TURNO ou DIA DE TRABALHO | Pergunta não nasce; DS/pai já existem; C7 pode fechar | Não se auto-fechar | `disputa_emissao.py:1232`, `:1235`, `:816`, `:820`, `:1282` | Guardas preservadas no agrupador (`disputa_emissao.py:+1146`). |

**Porta 4 — âncora, eleição e escrita do destino**

`chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1302`, chamada pela ação de view `chamados/services/acoes_disputa.py::nucleo_pedir_confirmacao_ancora:178`, em `:189`.

| ID — entrada/porta | Juiz consultado | O que persiste no hash | Push / destinatário | Prova | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| A1 — chamado não é `ancora_limbo` ou está encerrado | Nenhum; predicado local | Nada | Não | `disputa_emissao.py:1316`, `:1318` | Guarda preservada. |
| A2 — d/e, pergunta de âncora não materializada já existe | Nenhum; proxy SQL, sem C6 | Nada nessa chamada | Não | `disputa_emissao.py:1323` | Guarda preservada. |
| A3 — a/b/c; sem pergunta bloqueadora; DS aberta/nova | Nenhuma consulta à ata/marcos; busca DS aberta | Cria/agrega DS; cria **conversa**, destino `chamado_limbo`; chamado vai a análise | Sim ao colaborador | `disputa_emissao.py:1332`, `:1358`, `:1367`, `:1371`, `:1377`, `:1381` | Agregação central; push filtrado após commit. Conversa continua recebendo limbo diretamente (`disputa_emissao.py:+1411`, `:+1828`). | [nao verificado]
| A4 — eleição na emissão datada | CHAMADO: dia/motivos atendidos; desempate local por turno | Escolhe chamado vivo; exclui candidato já com pergunta não materializada do mesmo motivo; pode devolver nenhum | Nenhum próprio | `disputa_emissao.py:156`, `:202`, `:227`, `:241` | `_derivar_chamado_destino` delega ao novo eleitor; intervalo explícito precede chamado genérico; empate usa menor PK (`disputa_emissao.py:+98`; `fio.py:+752`). |
| A5 — eleição em outros caminhos | Órfãos escolhem primeiro do grupo; declaração pede seu chamado; âncora pede limbo | Destino recebido pela pergunta pode depender da porta de entrada | Nenhum próprio | `disputa_emissao.py:1197`, `:1377`; `chamados/services/declaracao_texto.py:376` | Emissores datados passam pelo eleitor; outros caminhos abaixo continuam escolhendo destino. |
| A6 — reatar histórico / substituir chamado terminal | CHAMADO: resolvedora válida; C8 para sobrevivente | Reaponta pergunta ao histórico único ou sobrevivente escolhido fora do emissor | Não próprio | `chamados/reconciliador.py:1586`, `:1624`, `:1643`; `chamados/models.py:374`, `:390`, `:437` | Esses eleitores permanecem; guarda nova preserva intervalo já eleito, sem substituir todas as eleições (`fio.py:+808`). |
| A7 — costura existente | `resolvedora_valida` + `_costura_nao_mata_o_fio` | Atualiza `PerguntaDisputa.chamado_destino`, eventualmente dia, e relavra | Não próprio | `chamados/services/fio.py::reapontar_resolvedora:804`, `:906`, `:926` | Atribuição/persistência passam por `gravar_destino_da_pergunta`; guarda protege intervalo eleito (`fio.py:+785`, `:+886`). |

O pai da DS também é escolhido por três nascimentos com argumentos distintos de `ChamadoColaborador.abrir`: núcleo comum, agrupador manual e âncora (`disputa_emissao.py:568`, `:1104`, `:1339`). O diff não os transforma em um único nascimento (`disputa_emissao.py:+435`, `:+1146`, `:+1411`).

**Porta 5 — declaração de horário**

`chamados/services/declaracao_texto.py::declarar_hora_colab:307`, `::aplicar_declaracao:631` → `disputa_emissao.py::disputa_para_declaracao:610`. São caminhos do colaborador e da aplicação administrativa da proposta, respectivamente (`declaracao_texto.py:313`, `:632`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| D1 — sem dia; b/c; ou hora não casa com marco | DIA DO CHAMADO; leitor da ata; casamento hora→marco | Recusa antes de criar DS/pergunta | Não | `declaracao_texto.py:330`, `:334`, `:341` | Guardas anteriores preservadas. |
| D2 — a, marco faltante; DS aberta/nova | Núcleo da DS; canônica do fato; depois resposta/validação | Cria/agrega DS; cria/reusa pergunta com dia da célula e destino solicitado; grava resposta; admin valida | Não pela declaração | `declaracao_texto.py:360`, `:363`, `:376`, `:659`, `:666`, `:696`; ausência de push coberta em `chamados/tests/test_porta_disputa_emissao.py:223` | DS/pergunta ficam no mesmo ensaio; retorno estrangeiro desfaz ensaio local e repete na dona (`disputa_emissao.py:+614`). |
| D3 — d, mesma resposta; ou e por validação sem efeito no marco | Comparação local / `_resposta_validada` | Pode ter agregado/criado DS antes de retornar `ja_declarada` ou `validada_sem_registro` | Não | `declaracao_texto.py:378`, `:412`, `:417` | Recusas permanecem depois do novo ato conjunto (`declaracao_texto.py:+357`; `disputa_emissao.py:+614`). |
| D4 — canônica de outra DS aberta/fechada | Identidade do fato | Reuso conserva dona original; DS local pode ter sido criada anteriormente | Não | `chamados/models.py:2833`; `declaracao_texto.py:360`, `:376` | Replay na dona; fechada recebe descarte de motivo, sem reabertura pelo agregador (`disputa_emissao.py:+614`, `:+408`). |

**Porta 6 — demais atualizações de DisputaSupervisao**

O censo atribui os escritores a `chamados/models.py` e `chamados/services/disputa_emissao.py`; os atos são declarados nos selos (`chamados/tests/test_porta_disputa_ciclo.py:81`, `:86`; `chamados/tests/test_porta_disputa_emissao.py:84`). As portas abaixo não distinguem a/b/c salvo pelas perguntas/guardas indicadas.

| ID — porta / entrada | Juiz ou predicado | Persistência | Chamadores; push | Na cópia | [nao verificado]
|---|---|---|---|---|
| U1 — `DisputaSupervisao.fechar:1911`; aberta, inclusive d/e | C7: nenhuma viva e nenhuma resposta sem veredito; exceções nomeadas não dispensam resposta sem veredito | `fechada_em/por`, `resolucao`, `motivo_fechamento`; exceções podem encerrar perguntas mudas | Escrita `models.py:1992`; reconciliador `:413`, `:613`, `:1172`; materialização `materializacao.py:592`, `:1339`; tranca `competencia_trancada.py:229`; desligamento `colaboradores/services/desligamento.py:167`. Signal tenta resolver pai, sem push próprio (`chamados/signals.py:20`, `:43`). | C7 e signal não alterados no diff. |
| U2 — `aparar_motivo_respondido:2008`; motivo presente | Nenhum; fato respondido por outro caminho | Remove motivo e contexto; não fecha sozinha (`models.py:2024`) | Upload de foto (`api/foto_recebida.py:72`); sem push nessa porta | Sem mudança. |
| U3 — `remover_motivo:2027`; aberta e motivo presente | Predicados locais; C7 no eventual fechamento | Retrata perguntas; atualiza motivos/contexto; tenta fechar se não sobrou motivo (`models.py:2121`, `:2128`) | Signals de posto e âncora (`colaboradores/signals.py:77`; `escala/signals.py:174`); sem push próprio | Sem mudança. |
| U4 — `anotar_contexto`; a/b/c/d/e | Nenhum; primeira presença da chave | Atualiza contexto uma vez (`models.py:2157`, `:2161`) | View `chamados/views.py:1343`; command escalonamento `escalonar_chamados_supervisao.py:159`; efeitos ficam nos chamadores | Sem mudança. |
| U5 — `renovar_contexto:2164`; a/b/c/d/e | Nenhum; valor diferente | Atualiza contexto (`models.py:2175`, `:2179`) | Cron diário (`alertar_disputas_pendentes.py:48`), detalhado abaixo | Cópia desloca chamada do cron para callback condicionado a pendência (`alertar_disputas_pendentes.py:+41`). |
| U6 — `reabrir_admin:2182`; fechada | Nenhum; exige usuário e teto de duas | Limpa fechamento/resolução; incrementa contador; motivo opcional (`models.py:2194`, `:2217`) | Ação de view `acoes_disputa.py:259`, delegada pelo fio `fio.py:583`; depois cria conversa em `acoes_disputa.py:263`; push supervisão/colaborador em `:277`, `:288` | Só aviso ao cola

[truncado em 20000 de 34735 caracteres]

## 14/11 19:23 · sessao c50cfa1332dd · - · - · origem=Ronald (tmux)

Censo de escrita DS executado **sem Django, banco ou logs**, com `python3 -B`, usando o varredor puro existente; ele classifica construções, saves, gerentes e inferências, sem garantir resolução de todo alias dinâmico (`app/core/censo_escritas.py:1`, `app/core/censo_escritas.py:451`, `app/core/censo_escritas.py:469`).

**19 sítios atribuídos de escrita DS no hash**, todos nestes dois módulos (`app/core/portas.py:443`):

| Porta | Persistência | Chamadores identificados |
|---|---|---|
| `DisputaSupervisao.fechar` | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; save `app/chamados/models.py:1992` | Reconciliador `app/chamados/reconciliador.py:413`, `:613`, `:1172`; materialização `app/chamados/services/materializacao.py:592`, `:1339`; tranca `app/chamados/services/competencia_trancada.py:229`; becos `app/chamados/services/becos.py:57`; upload foto `app/api/foto_recebida.py:80`; entrada sincronizada `app/ponto/services/entrada_sincronizada.py:60`; emissão `app/chamados/services/disputa_emissao.py:1284`, `:2092`; desligamento `app/colaboradores/services/desligamento.py:167`; comandos `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`. |
| `aparar_motivo_respondido` | `motivos`, `contexto`; save `app/chamados/models.py:2024` | Upload de foto, `app/api/foto_recebida.py:72`. |
| `remover_motivo` | retrata perguntas; `motivos`, `contexto`; save `app/chamados/models.py:2121`; chama `fechar` se nenhum motivo, `:2128` | Signals de posto `app/colaboradores/signals.py:77` e âncora `app/escala/signals.py:174`. |
| `anotar_contexto` | primeira anotação apenas; save `app/chamados/models.py:2161` | View `app/chamados/views.py:1343`; comando escalonamento `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`. |
| `renovar_contexto` | se valor mudou; save `app/chamados/models.py:2179` | Alerta diário `app/ponto/management/commands/alertar_disputas_pendentes.py:48`. |
| `reabrir_admin` | limpa fechamento/resolução, incrementa teto administrativo, agrega motivo opcional; save `app/chamados/models.py:2217` | Ação de view `app/chamados/services/acoes_disputa.py:259`, delegada pelo fio `app/chamados/services/fio.py:583`. |
| `reabrir_sistema` | limpa fechamento/resolução; save `app/chamados/models.py:2233` | Ressurreição no chamado `app/chamados/models.py:1622`; relavra `app/chamados/services/relavra_via.py:261`; validação recusada `app/chamados/services/validacao.py:241`; materialização de ausência desfeita `app/chamados/services/materializacao.py:692`; comando `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`. |
| `reabrir_por_colab` | questionário volta a pendente e incrementa `reaberturas_colab`; save `app/chamados/models.py:2296` | Views HTML `app/chamados/views.py:1334` e API `app/api/views.py:2679`. |
| `limpar_resolucao_residual` | apenas `resolucao` de aberta; save `app/chamados/models.py:2322` | Nenhum chamador produtivo encontrado; contrato declara a porta (`app/core/portas.py:405`); alcance de invocação externa [nao verificado]. |
| `perguntar_entradas_pendentes` | `contexto`; save `app/chamados/services/disputa_emissao.py:296` | Wrapper `app/chamados/services/disputa_emissao.py:685`; órfãos `:1270`. |
| `_abrir_ou_agregar_disputa` | atualiza motivos/contexto `app/chamados/services/disputa_emissao.py:565`; cria DS `:587` | Wrapper `:662`; declaração `:628`; comando smoke `app/chamados/management/commands/smoke_chamados.py:100`; helper sem chamador `_disputa` `app/ponto/management/commands/reconciliar_grade.py:424`. |
| Agrupador de órfãos | saves/criação `app/chamados/services/disputa_emissao.py:1084`, `:1123`, `:1132`, `:1161`, `:1256` | Fluxo auditado pelo outro explorer; cinco sítios também declarados em `app/chamados/tests/test_porta_disputa_emissao.py:86`. |
| Âncora | cria DS `app/chamados/services/disputa_emissao.py:1358`; atualiza motivos `:1367` | `nucleo_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:189`; cascas de detalhe e fio declaradas em `:179`. |

O comentário antigo que diz desligamento por `.update()` está ultrapassado: o código atual percorre as abertas e chama `fechar`, retendo resposta sem veredito (`app/colaboradores/services/desligamento.py:157`, `:164`, `:167`). Smoke e `_disputa` são **chamadores do núcleo**, não criações DS adicionais; smoke chama também o gerador, e `_disputa` é removido pelo diff (`app/chamados/management/commands/smoke_chamados.py:100`, `:103`; `app/ponto/management/commands/reconciliar_grade.py:424`; `diff_obra.patch`, hunk `@@ -409,23 +409,7 @@`).

**Portas adicionais de pergunta/âncora**

| Estado/porta | Juiz/predicado | Persistência e push | Cópia | [nao verificado]
|---|---|---|---|
| Declaração do colaborador sem dia | `dia_do_fato`; sem dia recusa | Nada, retorna `sem_data` (`app/chamados/services/declaracao_texto.py:330`). | Preserva guarda; altera operação posterior (`diff_obra.patch`, declaração, hunk `@@ -357,24 +357,13 @@`). |
| Sem ata | `_marcos_faltantes`; `sem_celula` | Recusa antes DS/pergunta; acende falta de ata (`app/chamados/services/declaracao_texto.py:334`). | Guarda anterior preservada pelo diff. |
| Com ata, sem marco que case | `_casar_hora` | Nada; `nao_casa` (`app/chamados/services/declaracao_texto.py:341`). | Guarda anterior preservada pelo diff. |
| Com marco faltante; DS aberta / inexistente | Núcleo DS; depois canônica do universo do fato, `fio.canonica_do_grupo` | DS criada/agregada; pergunta reusada localmente ou `abrir` devolve canônica até de outra DS; destino pedido é `ch`, dia é célula (`app/chamados/services/declaracao_texto.py:360`, `:363`, `:376`; `app/chamados/models.py:2833`). Declaração não manda push (`app/chamados/tests/test_porta_disputa_emissao.py:223`). | DS e pergunta entram no mesmo ensaio; retorno estrangeiro desfaz ensaio e repete na dona; destino passa `_destino_do_intervalo` (`diff_obra.patch`, `_pergunta_para_declaracao`, novas linhas a partir de `:614`). |
| Pergunta validada embora marco siga faltante | `_resposta_validada` | Retorna `validada_sem_registro`; **DS já foi criada/agregada antes** (`app/chamados/services/declaracao_texto.py:378`, `:412`). | Mantém a recusa, reorganiza nascimento (`diff_obra.patch`, declaração, hunk `@@ -357,24 +357,13 @@`). |
| Pergunta já respondida igual à hora | comparação local | `ja_declarada`; sem nova resposta (`app/chamados/services/declaracao_texto.py:417`). | Comparação posterior preservada pelo diff. |
| Declaração administrativa | proposta válida, mesma canônica; depois `validar_pergunta` | DS/pergunta como acima; responde se necessário e valida; sem proposta não escreve (`app/chamados/services/declaracao_texto.py:649`, `:659`, `:666`, `:687`, `:696`). | Passa argumentos da pergunta ao ato DS; replay em dona estrangeira (`diff_obra.patch`, hunk `@@ -656,15 +645,13 @@`). |
| Âncora: categoria diferente/encerrada | nenhum: predicados locais | Nada (`app/chamados/services/disputa_emissao.py:1316`). | Sem alteração desses predicados no diff. |
| Âncora: pergunta não materializada existente, respondida ou C6 morta | nenhum: `materializada_em IS NULL` | Nada, **não consulta C6** (`app/chamados/services/disputa_emissao.py:1323`). | Guarda preservada no diff. |
| Âncora sem tal pergunta; DS aberta / inexistente ou só fechada | nenhuma consulta à ata/marcos; busca DS aberta | Agrega ou cria DS; cria conversa `ancora_ausente`, destino literal `chamado_limbo`; chamado vai a análise; solicita push ao colab (`app/chamados/services/disputa_emissao.py:1332`, `:1358`, `:1371`, `:1377`, `:1379`, `:1381`). | Agregação passa `_agregar_motivos_na_dona`; push herda filtro de pendência pós-commit; eleição datada não entra porque conversa (`diff_obra.patch`, hunk `@@ -1332,7 +1411,7 @@`; `@@ -1755,38 +1828,36 @@`). |
| Reabertura administrativa de DS fechada | predicados locais: staff/grupo, motivo válido, justificativa, teto dois | Reabre DS primeiro; depois cria conversa nova, sem destino explícito; reabre chamado se encerrado; push supervisão e colab (`app/chamados/services/acoes_disputa.py:245`, `:255`, `:259`, `:263`, `:269`, `:276`, `:288`). | Filtro de pendência pós-commit no push ao colab; ordem DS→pergunta permanece (`diff_obra.patch`, hunk `@@ -285,18 +293,18 @@`). |
| Reabertura de questionário | `perguntas_corrigiveis` | Limpa texto/resposta/validação via `reabrir_para_correcao`; guarda foto na sequência; não gera pergunta nova (`app/chamados/services/disputa_emissao.py:1582`, `:1593`, `:1615`, `:1632`). View própria e ação fio mandam push (`app/chamados/views.py:1435`, `:1444`; `app/chamados/services/acoes_disputa.py:102`, `:109`). | Push ao colab filtrado depois do commit (`diff_obra.patch`, ações hunk `@@ -90,6 +90,7 @@`; views hunk `@@ -1444,12 +1444,13 @@`). |
| Fábrica de célula | motivos emitidos pela ata; remove carimbo sem lastro | Chama wrapper e **chama gerador novamente se DS retornou** (`app/chamados/services/fabrica_da_celula.py:160`, `:169`, `:172`, `:174`). Julgamento silencia push (`:218`). | Arquivo fora do diff; chamada duplicada preservada. |

Testes de existência/contrato, sem executá-los: declaração e ausência de push `app/chamados/tests/test_porta_disputa_emissao.py:185`, `:199`, `:211`, `:223`; reabertura colab e teto `app/chamados/tests/test_porta_disputa_ciclo.py:283`, `:297`, `:311`, `:322`; porta de dia `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:165`; cobertura dos novos testes listados no anexo [nao verificado].

**I3: NÃO FECHA globalmente, hash e cópia.**

No hash, eleição da emissão é `_derivar_chamado_destino`; órfãos escolhem primeiro chamado da ordem recebida; declaração pede seu chamado; âncora pede limbo (`app/chamados/services/disputa_emissao.py:156`, `:1197`, `:1377`; `app/chamados/services/declaracao_texto.py:376`). Escrita persistente está em dois corpos: `models._nascer` e `fio.reapontar_resolvedora`, explicitamente reconhecido (`app/chamados/services/fio.py:808`, `app/chamados/models.py:3038`, `:3070`, `app/chamados/services/fio.py:926`).

A cópia cria um eleitor comum aos emissores datados e um escritor de destino, mas conserva outros escolhendo destino: C8 escolhe sobrevivente no gate do chamado; reconciliador escolhe histórico único; limbo recebe pai imposto; migração recebe novo chamado (`app/chamados/models.py:374`, `:390`, `:437`; `app/chamados/reconciliador.py:1586`, `:1624`, `:1643`; `app/chamados/services/limbo_folgas.py:226`, `:245`; `app/chamados/services/fio.py:2175`). Esses sítios não são alterados no diff; guarda nova só preserva intervalo já eleito, não elege universalmente (`diff_obra.patch`, fio hunk `@@ -752,7 +808,32 @@`).

**I4: NÃO FECHA no ciclo inteiro, hash e cópia; o escritor final do campo já é único.**

`gravar_dia_do_fato` já é o ponto único de atribuição, e o contrato final exige dia do juiz para fato/conversa civil separadamente (`app/chamados/services/fio.py:791`, `:801`; `app/chamados/models.py:2862`, `:2868`, `:2875`). Isso não unifica as decisões anteriores: órfão com origem classifica o dia **civil** e pode sair antes de resolver dia **do turno** e ler sua ata (`app/chamados/services/disputa_emissao.py:1199`, `:1200`, `:1201`, `:1205`, `:1209`). A sequência permanece na cópia (`diff_obra.patch`, órfãos hunk `@@ -1098,176 +1146,202 @@`). Ocorrência/frequência em produção [nao verificado].

O gerador do hash também monta texto/dedup antes do dia final do modelo; a cópia resolve esses três insumos por `_resolver_pergunta_por_motivo`, melhoria local que não elimina a sequência civil→turno dos órfãos (`app/chamados/services/disputa_emissao.py:345`, `:372`, `:399`; `app/chamados/models.py:2931`; `diff_obra.patch`, `_resolver_pergunta_por_motivo`, novas linhas `:209` seguintes).

## 14/11 19:22 · sessao b468514477ca · E3-V10-DEGRAU-DONO-E-PORTA · verificador · origem=Code

**Veredito: REPROVADO nesta volta.** Há contraexemplo à L-129, divergência entre os universos da carga e do app, dois mutantes obsoletos e insuficiência do selo AST para provar consumo da tarefa. Referências: `app/chamados/services/tarefa_do_dia.py:277`, `app/chamados/services/pergunta_no_app.py:52`, `app/chamados/tests/e3_degrau_mutantes.py:41`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:57`.

**1. Escritores e leitores**

| Campo/valor | Quem fornece/escreve | Quem lê |
|---|---|---|
| `fio` | Seleção em `app/chamados/services/tarefa_do_dia.py:622`; publicação em `:635` | Projeção em `app/chamados/services/tarefa_do_dia.py:645`; Folha em `app/folha/fora_da_folha.py:85`; vigia em `app/chamados/services/vigia_fio.py:663`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:54` |
| `rota` | `app/chamados/services/tarefa_do_dia.py:629`; publicação em `:635` | Projeção em `app/chamados/services/tarefa_do_dia.py:645`; furo em `app/chamados/services/furo_sem_canal.py:99`; vigia em `app/chamados/services/vigia_fio.py:662`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:53` |
| `fila` | Calculada em `app/chamados/services/tarefa_do_dia.py:609`, a partir da alimentação de `app/chamados/juizes.py:1589` | Folha nomeia chamados por ela em `app/folha/fora_da_folha.py:244`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:143` |
| `verbos` | Painel fornece por `app/chamados/juizes.py:1570`; tarefa seleciona em `app/chamados/services/tarefa_do_dia.py:608` | Residual do próprio juiz em `app/chamados/services/tarefa_do_dia.py:618`; projeção em `:645`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:144` |
| `donos` | **Conjunto local, não campo publicado**: `app/chamados/services/tarefa_do_dia.py:615` | Seleção do fio em `app/chamados/services/tarefa_do_dia.py:622`; publicação não inclui `donos`, em `:635` |
| Retorno de `porta_da_tarefa` | Cópia dos cinco campos ou `None`: `app/chamados/services/tarefa_do_dia.py:645` | Folha `app/folha/fora_da_folha.py:70`, `:238`; vigia `app/chamados/services/vigia_fio.py:649`; furo `app/chamados/services/furo_sem_canal.py:97`; beco `app/escala/services/cadastro_realidade.py:469`; sombra `app/chamados/management/commands/diff_e3_sombra.py:43` |
| Pendentes/histórico | `particao_do_app`, em `app/chamados/services/pergunta_no_app.py:76`; consulta compõe em `:73` | API `app/api/views.py:994`, `:2534`; dashboard `app/core/views.py:253`; aviso `app/chamados/services/aviso_pendencia.py:9`; cobrança `app/chamados/services/cobranca.py:48`; projeção/badge `app/chamados/services/pergunta_no_app.py:100`; carga `app/chamados/services/tarefa_do_dia.py:514` |
| Vivas do app | Consulta ORM e C6 em `app/chamados/services/pergunta_no_app.py:52`, `:61` | Consulta em `app/chamados/services/pergunta_no_app.py:73`; ficha `app/colaboradores/services/ficha.py:166`; C17 `app/chamados/juizes.py:1642`; diagnósticos `app/chamados/services/pergunta_no_app.py:208`, `:231` |
| C6 na carga | Corpo único em `app/chamados/juizes.py:1399`; fachada em `:862`; alimentação em `app/chamados/services/carga_pergunta_viva.py:69` | Carga chama em `app/chamados/services/tarefa_do_dia.py:521`, `:525`; `viva_c6` alimenta exposição administrativa em `:260`, `:347`; `_pendentes_app` alimenta publicação em `:568`, `:610` |

A pauta lê indiretamente o fio da tarefa e compara com `FIO_SEM_PORTA`; o E4b lê instâncias e ações para escolher atos. Referências: `app/ponto/services/pautas_dos_discordantes.py:79`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`.

O retrato grava outro campo, `dono`, a partir do grupo agregado da Folha; depois prefere esse valor gravado. Templates renderizam o resultado e o PDF usa o rótulo do LED. Referências: `app/folha/retrato_folha.py:1158`, `:1190`, `:438`; `app/templates/folha/_retrato_lista.html:47`; `app/templates/folha/_retrato_o_que_fazer.html:33`; `app/folha/views_retrato.py:413`.

**Achado de segundo escritor dos quatro campos publicados: NÃO PROCEDENTE nos caminhos rastreados.** A montagem fica em `_portas_da_tarefa`; os leitores citados projetam, contam ou agregam sua saída. Referências: `app/chamados/services/tarefa_do_dia.py:635`, `app/folha/fora_da_folha.py:85`, `app/chamados/services/vigia_fio.py:662`.

**2. Uma definição de pendência, mas conjuntos diferentes — PROCEDENTE**

O predicado local é compartilhado: `respondida_em is None` e disputa aberta. Não há segunda cópia desse predicado na carga. Referências: `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/tarefa_do_dia.py:514`.

O universo, porém, difere:

- A carga seleciona `colaborador_id`, `conversa=False` e `data_evento` dentro da janela. O app seleciona `disputa__colaborador` e `q_vivas`, sem esses recortes de conversa/data. Referências: `app/chamados/services/tarefa_do_dia.py:395`, `app/chamados/services/pergunta_no_app.py:52`.
- Mesmo dentro da janela, um `agora` histórico pode divergir: o app elimina previamente perguntas com `validada_em` preenchido; a carga admite a pergunta e C6 desconsidera validação posterior ao instante consultado. Referências: `app/chamados/models.py:3468`, `app/chamados/juizes.py:1387`, `app/chamados/services/tarefa_do_dia.py:395`.
- Resposta e fechamento da disputa são lidos **no estado atual** pela partição. C6 projeta resposta futura para inexistente, mas a partição continua vendo seu carimbo preenchido. Isso afeta ambas as composições. Referências: `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/juizes.py:1390`.
- Os dois campos de colaborador devem coincidir pela porta de identidade. Divergência efetiva de colaborador/empresa em dados válidos: **[nao verificado]**. Referências: `app/chamados/models.py:3573`, `app/chamados/services/tarefa_do_dia.py:390`, `app/chamados/services/pergunta_no_app.py:59`.

A igualdade alegada nos comentários, inclusive os números de frota, não constitui prova disponível de equivalência: **[nao verificado]**. Referência da alegação: `app/chamados/services/tarefa_do_dia.py:44`.

**3. Juízes, escritores e relógios**

- **C6 duplicado na carga: NÃO PROCEDENTE.** A carga chama o corpo registrado e reaproveita o resultado quando a pergunta já foi julgada como candidata. Referências: `app/chamados/services/tarefa_do_dia.py:518`, `:521`, `:523`; `app/chamados/juizes.py:888`.
- **C17 paralelo nos leitores migrados: NÃO PROCEDENTE.** O chamado remanescente encontrado é o lado anterior do instrumento de comparação. Referências: `app/chamados/management/commands/diff_e3_sombra.py:127`, `app/core/juizes.py:136`.
- **Tabela paralela de verbo no novo juiz: NÃO PROCEDENTE.** Ele reutiliza `fio_pelos_verbos`, excluindo `cobrar` antes. Referências: `app/chamados/services/tarefa_do_dia.py:618`, `:620`; `app/chamados/juizes.py:1650`.
- **C9 substituído por `sla.vencido` na escolha do fio: NÃO PROCEDENTE.** A escolha não consulta prazo. Entretanto, o painel continua exibindo outro SLA, calculado por referência/urgência; esse valor não comprova vencimento C9. Referências: `app/chamados/services/tarefa_do_dia.py:610`, `app/chamados/catalogo/motor.py:54`, `app/chamados/services/painel.py:378`, `app/chamados/services/sla.py:26`.
- **Relógio novo dentro da tarefa: NÃO PROCEDENTE.** O instante segue à carga, ao C6 e à alimentação; a casca rejeita instante nulo ou carga incompatível. A API atual não fornece esse instante, e C6 conserva fallback de calendário. Referências: `app/chamados/services/tarefa_do_dia.py:519`, `:565`, `:657`; `app/api/views.py:2534`; `app/chamados/juizes.py:592`.

Há derivações de “dono” com **outra unidade**: a ficha traduz verbo do chamado; o relatório classifica disputa por `aguarda_o_colaborador`. Uma resposta vazia carimbada pode estar no histórico do app e ainda sustentar `AGUARDA_COLAB` no relatório. Isso é diferença semântica, não prova de outro juiz do colab-dia. Referências: `app/colaboradores/services/ficha.py:159`, `app/chamados/relatorios_disputa.py:26`, `app/chamados/services/fio.py:187`, `app/chamados/services/pergunta_no_app.py:81`.

**4. Residual `cobrar` e preservação do admin**

**Residual sozinho dar dono: NÃO PROCEDENTE.** O filtro exclui `cobrar`; sem outro dono, não publica rota. A porta `validar` do chamado ligado à ação do colab continua residual quando falta pendência no app, preservando o admin. Referências: `app/chamados/services/tarefa_do_dia.py:613`, `:618`, `:629`; testes em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:58`, `:105`, `:113`.

**E4b esconder classe e como resolvida: NÃO PROCEDENTE por leitura.** O teste exige `segue_sem_porta`, reabertura, nota única, idempotência e manutenção de um dia sem porta no universo. Execução ORM: **[nao verificado]**. Referências: `app/chamados/tests/test_e4_passivo.py:76`, `:81`, `:86`, `:89`, `:117`, `:125`.

A cobertura denominada “B” exige cuidado: o agrupador produz grupo B a partir de fio colab; já `RedBRespostaTextual` usa perguntas respondidas e hipótese de resposta E1, esperando `nada`. Esse teste não certifica a classe B pendente da lei fornecida. Referências: `app/folha/fora_da_folha.py:97`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:108`, `:120`, `:128`.

**Há caminho que perde o colab apesar da pendência: PROCEDENTE**, detalhado no item 8. Referências: `app/chamados/services/tarefa_do_dia.py:277`, `:610`.

**5. Mordida dos testes e mutantes** [nao verificado]

**PROCEDENTE — `--todos` não conclui.** `sem_memo_por_colab` e `carga_sem_relogio` ainda apontam para implementação/teste removidos na passagem para lote. A validação textual levanta antes do controle verde. Referências: `app/chamados/tests/e3_degrau_mutantes.py:41`, `:43`, `:87`, `:92`, `:112`.

**PROCEDENTE — selo AST insuficiente.** Proíbe imports/nomes, mas a anti-vacuidade exige apenas presença dos imports permitidos. Não exige chamadas nem consumo da resposta; preservar esses imports permite passar sem ler a tarefa. O degrau apenas executa esse selo. Referências: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:24`, `:57`, `:61`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:293`.

| Teste alterado | Veredito sobre a mordida | [nao verificado]
|---|---|
| `n1` | **NÃO PROCEDENTE** a vacuidade “ninguém foi chamado”: junto ao mapa vazio exige uma chamada à partição. Não prova equivalência dos conjuntos. `app/chamados/tests/test_n1_alimentacao_unica.py:114`, `:115`, `:164` |
| `portas_tarefa` | **NÃO PROCEDENTE** ficar vazio: exige C6 exatamente para `[p.pk]`, alimentação única e repetição sem consultas. `app/chamados/tests/test_e3_portas_tarefa.py:139`, `:144`, `:146`, `:152` |
| `pos_suite` | **NÃO PROCEDENTE** perder toda mordida: agora protege a grade respondida, fora dos pendentes. O contrato ficou mais estreito. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:50`, `:65`, `:67` |
| `e4_passivo` | **NÃO PROCEDENTE** mascarar ausência de efeito: exige que a classe e continue contada sem porta. `app/chamados/tests/test_e4_passivo.py:81`, `:122`, `:125` |
| `o261e_f5_alcance` | **PROCEDENTE, limitado:** prova influência da partição, mas deixou de provar ausência de consulta paralela pelo vigia; um leitor paralelo da mesma partição também poderia acompanhar o mock. `app/chamados/tests/test_o261e_f5_alcance.py:273`, `:274`, `:278`, `:280` |

Os testes C1/C2 fornecem um campo sintético `prazo_c9_estourou` que o juiz não lê; demonstram independência desse campo, não integração com C9 real. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:44`, `:48`, `:300`.

Na execução sem banco, degrau+pós-suite+portas-v6 deram **50 testes OK**; o runner completo abortou no alvo obsoleto. Isoladamente, 19 mutantes tiveram controle verde e morreram; dois falharam por alvo inexistente. Fontes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:19`, `app/chamados/tests/test_e3_portas_v6_sem_banco.py:15`, `app/chamados/tests/e3_degrau_mutantes.py:84`. Registro persistente desses resultados: **[nao verificado]**.

**6. Semântica**

`rota` é classificação da composição de portas do dia; não identifica necessariamente a porta particular do dono vencedor. O fio é escolhido antes da rota. `porta_app` também não implica fila vazia: `cobrar` pode permanecer em `fila/verbos`. Referências: `app/chamados/services/tarefa_do_dia.py:608`, `:622`, `:632`, `:634`.

`nada` e `nao_sei` são distintos na tarefa, mas ambos ficam sem rota. A Folha os projeta para `FIO_SEM_PORTA='nada'`, preservando separadamente a flag de desconhecimento. Há ainda `sistema` sem rota. Referências: `app/chamados/services/tarefa_do_dia.py:623`, `:629`, `:634`; `app/chamados/juizes.py:1652`; `app/folha/fora_da_folha.py:85`, `:248`.

Os leitores diretos aceitam a presença da projeção: Folha lê fio, beco testa presença, furo reconhece as três rotas e vigia conta pela chave publicada. Referências: `app/folha/fora_da_folha.py:85`, `app/escala/services/cadastro_realidade.py:469`, `app/chamados/services/furo_sem_canal.py:114`, `app/chamados/services/vigia_fio.py:662`.

**PERGUNTA:** “sem porta explícito” também substitui o agrupamento do retrato/PDF? O código mantém `ninguem → Admin/Hoje`; “SEM PORTA” aparece no drawer, enquanto o PDF imprime o LED. Referências: `app/folha/retrato_folha.py:192`, `:477`, `:1069`; `app/folha/views_retrato.py:413`.

**7. Comentários/docstrings incompatíveis — PROCEDENTE**

- “Uma consulta do app por colab, memo da carga” descreve implementação abandonada; o corpo faz partição em lote. Referências: `app/chamados/services/tarefa_do_dia.py:362`, `:514`.
- “TODO chamado vivo na fila” publica porta e “CHAMADO VIVO NA FILA TEM PORTA” não ressalvam o residual `cobrar` agora excluído. Referências: `app/chamados/services/tarefa_do_dia.py:28`, `:580`, `:618`, `:629`.
- A promessa de conservar portas de chamados vivos também necessita dessa ressalva. Referências: `app/chamados/services/tarefa_do_dia.py:641`, `:618`.
- Docstrings dos testes `n1` ainda descrevem consulta/memo por colaborador, contrariando seus próprios comentários e asserções de lote. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:106`, `:112`, `:143`, `:163`.

**8. Contraexemplo à lei — PROCEDENTE**

Caso **sintético**, executado em memória com corpos reais: pergunta 303 de `orfao_14h`, sem resposta, validação ou materialização; disputa aberta; célula `furo`; marco `fim` aceso e vencido; sem tranca e fila vazia. O SQL admite a pergunta e C6 retorna viva porque a célula acusa. Referências: `app/chamados/models.py:3467`, `app/chamados/juizes.py:1444`, `:1450`; mecanismo de execução em `app/chamados/tests/e4_apoio.py:8`.

Resultado: pendentes `[303]`, mas tarefa com **`fio='nada', rota=None`**. A tarefa pula a lâmpada acesa antes de associar a pergunta; depois exige simultaneamente pendência e ação `RESPONDER` na instância. Referências: `app/chamados/services/pergunta_no_app.py:73`, `:81`; `app/chamados/services/tarefa_do_dia.py:277`, `:321`, `:610`, `:622`.

A API serve as pendentes e sua porta de resposta consulta C6, sem acrescentar guarda de lâmpada acesa. Portanto não basta alegar que essa pergunta estaria inacessível por estar acesa. Referências: `app/api/views.py:2535`, `:2572`, `:2608`, `:2611`.

Também existe contraexemplo histórico na direção inversa: validação posterior a `agora` exclui a pergunta do app pelo pré-filtro atual, mas C6 da carga pode considerá-la pendente e permitir fio colab. Referências: `app/chamados/models.py:3468`, `app/chamados/juizes.py:1387`, `app/chamados/services/tarefa_do_dia.py:523`.

Incidência desses casos em produção: **[nao verificado]**.

**9. Pouso** [nao verificado]

**Dependência nova de migration ou cron: NÃO PROCEDENTE nos caminhos alterados examinados.** Os novos valores são montados em memória; a saída do comando acrescenta o contador app ao fluxo existente. Referências: `app/chamados/services/tarefa_do_dia.py:391`, `:635`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`.

Estado real de migrations, implantação conjunta, outras raias e cumprimento das condições de produção/exportação: **[nao verificado]**. A reprodução `--base` também permanece **[nao verificado]**, pois depende de `git show` da base. Referência: `app/chamados/tests/e3_degrau_mutantes.py:107`.

**PERGUNTA:** conversas pendentes do app pertencem a algum colab-dia para esta lei? O app as admite; a projeção declara que não têm dia de fato, e a carga as exclui. Referências: `app/chamados/services/pergunta_no_app.py:52`, `:114`, `app/chamados/services/tarefa_do_dia.py:396`.

## 14/11 19:22 · sessao 88570989b070 · - · - · origem=Ronald (tmux)

Referências abaixo relativas à raiz; nada foi executado contra banco/log [nao verificado].

**Portas e chamadores**

| Porta | Chamador comprovado |
|---|---|
| `app/chamados/views_cobrar.py::cobrar_chamado:24` | View POST, URL `app/chamados/urls.py:25`; permissão/área em `views_cobrar.py:25` e `:31`. |
| `app/chamados/views_cobrar.py::cobrar_massa:61` | View POST, URL `app/chamados/urls.py:31`; seleção VIVOS/verbo/folha em `views_cobrar.py:76`, `:78`, `:95`. |
| `app/chamados/services/cobranca.py::cobrar_colaborador:140` | View IA `_aplicar_cobranca:834`, chamada indireta `app/core/views_ia.py:850`, escritor registrado `app/core/intake.py:67`. |
| `app/chamados/services/disputa_emissao.py::recobrar_questionario:1690` | View `reabrir_disputa_supervisao`, chamada `app/chamados/views.py:1425`, somente após guarda `pend_vivas` em `:1417` e ausência de corrigíveis `:1421`. |
| `app/ponto/management/commands/alertar_disputas_pendentes.py::Command.handle:30` | Cron 08:00, `app/config/crons.py:745`. |
| `app/chamados/services/disputa_emissao.py::_enviar_push_disputa_colab:1757` | Emissor central; não consulta C6, somente silenciamento em `:1759`; envia webpush `:1777` e FCM `:1785`. |

**Matriz — cobrar individual**

| Estado | Juiz | Persiste | Push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|---|
| a ata; b sem ata; c sem marco; d respondida; e morta; DS aberta/fechada/inexistente | Nenhum juiz de pergunta/DS: recebe qualquer chamado existente `app/chamados/views_cobrar.py:29`; guarda é somente permissão, área e throttle `:25`, `:31`, `:36`. | Nenhuma escrita DS/P; sucesso cria `FollowupChamado` em `:55`, pelo escritor `app/chamados/services/nota.py:103`. | Tenta FCM ao `ch.colaborador`, todos os estados, em `views_cobrar.py:44`; entrega depende driver `app/chamados/fcm_utils.py:104`. | `app/chamados/tests/test_e5_cobrar.py:34` cobre envio/trilha/throttle; `:48` falha sem token. | Novo `_enviar_cobranca` consulta `_disputa_do_chamado` e app quando encontra DS aberta; **se não encontra DS e categoria do chamado não é `disputa_supervisao`, envia diretamente** (anexo diff, `app/chamados/views_cobrar.py`, hunk `+20`). |

**Contraexemplo importante da cópia para I2:** chamado de furo, categoria diferente de `disputa_supervisao`, somente dona FECHADA, pergunta respondida ou morta. `_disputa_do_chamado` só busca dona ABERTA (`app/chamados/services/fio.py:131`, `:135`); retorna `None`; novo `elif ch.categoria != 'disputa_supervisao': enviar()` manda FCM sem pergunta pendente. Individual não exige VIVOS (`app/chamados/views_cobrar.py:29`). Portanto **I2 NÃO FECHA na cópia** mesmo restringindo a cobrança ao colaborador (anexo diff, `views_cobrar.py`, hunk `+20`; hash `fio.py:131`).

**Matriz — massa**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c, DS aberta/fechada/inexistente, desde que crédito da consulta produza verbo cobrar | Nenhum C6 atual no caminho quente: `particionar_verbos` lê créditos (`app/chamados/services/verbos.py:34`); crédito usa `q_vivas` (`app/chamados/services/raiox.py:74`) e exclui materializada+VL_SIM `:135`; não filtra DS fechada (`:56`). | FCM ao colab `views_cobrar.py:123`, uma seleção por pessoa `:97`; sucesso Followup `:132`. DS/P sem escrita nessa porta. | `app/chamados/tests/test_e5b_cobrar_massa.py:52` dedup/placar; `:74` prévia sem enviar. | Acrescenta `pendentes_da_disputa` se `_disputa_do_chamado` encontra DS aberta; preserva envio direto não-pai sem DS aberta (diff `views_cobrar.py`, hunks `+118`, `+20`). |
| d respondida creditada | Resposta ganha verbo validar: `raiox.py:139`; `catalogo/natureza.py:48` e `:59`. | Não entra no conjunto cobrar `views_cobrar.py:78`, salvo outra pergunta que produza crédito cobrar sob outro chamado [nao verificado]. | Predicados acima. | Gate app adicional no recorte DS aberta (diff `views_cobrar.py`, `+118`). |
| e morta C6 | A consulta de crédito pode ficar stale: código declara que conta o campo como está até próximo ciclo (`raiox.py:117`) e não consulta juiz no caminho quente (`:120`). | Possível crédito SQL divergente de C6 atual; envio permanece possível se verbo cobrar (`views_cobrar.py:78`, `:123`). | `raiox.py:117`. | C6 reaplicado se encontra DS aberta; bypass de dona fechada permanece (diff `views_cobrar.py`, `+20`, `+118`). |

**Matriz — cobrança por pessoa**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c, pergunta viva sem resposta, DS aberta | C6 através `_pendentes`→`consulta_do_app` (`cobranca.py:45`; `pergunta_no_app.py:70`). Ata/marco não são portas diretas desta cobrança. | `ChamadoColaborador.prazo_sla` dos pais via `definir_prazo_sla`, `cobranca.py:178`; Followup `:181`; LogAuditoria `:188`; FCM primeiro/webpush fallback ao colab em `:107`, `:112`. | `test_cobranca.py:102` um toque; `:115` prazo; `:139` trilha. | Mesmo prazo/trilha; aviso passa por lista app antes/depois commit (diff `cobranca.py`, `+179`; novo `pergunta_no_app.py:82`). | [nao verificado]
| d respondida, e morta, DS fechada, ou nenhuma pergunta | App exclui morta antes (`pergunta_no_app.py:58`); respondida/DS fechada vão histórico `:71`; zero pendente retorna sem cobrar `cobranca.py:156`. | Nada/push não, se não existe outra pendente da pessoa (`cobranca.py:156`). | `test_cobranca.py:86` sem pendência. | Conserva e acrescenta rechecagem após commit (diff `pergunta_no_app.py:82`). | [nao verificado]
| Prazo futuro em algum pai das pendentes | Predicado local `cobranca.py:86`. | Nada/push não `:159`. | `test_cobranca.py:127`. | Conservado, diff não altera este bloco. | [nao verificado]

A cobrança por pessoa já utiliza a lista correta no hash, mas o envio fica depois de seu atomic interno e pode ainda ocorrer dentro de atomic externo, sem rechecagem imediatamente após commit (`cobranca.py:176`, `:187`). Cópia acrescenta esse controle (diff `cobranca.py`, hunk `+179`). [nao verificado]

**Matriz — recobrar questionário**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c, pendente viva, DS aberta, sem corrigível | A view consulta C6 `app/chamados/views.py:1417`; o serviço conta apenas `respondida_em IS NULL` `disputa_emissao.py:1712`. | Prazo do chamado `:1718`; Followup `:1721`; LogAuditoria `:1734`; FCM ao colab da DS `:1728`. | `test_recobrar_questionario.py:88` prazo; `:96` push. | Serviço usa `pendentes_da_disputa`, retorna imediatamente se zero; usa `_enviar_push_disputa_colab` com gate após commit (diff `disputa_emissao.py`, `+1785`). |
| d respondida corrigível | View segue reabertura, não recobrança `views.py:1435`; FCM após reabertura `:1447`. | Reabertura canônica e Followup `:1440`. | `test_recobrar_questionario.py:112`. | FCM envolve `notificar_pendentes_apos_commit` (diff `views.py`, `+1444`). |
| e morta sem resposta | View não chama serviço se nenhuma pendente viva `views.py:1431`; **serviço avulso não possui essa guarda**, conta cru e sempre tenta FCM `disputa_emissao.py:1712`, `:1728`. | Pela view: nada/push não; serviço direto: prazo/trilha/push mesmo zero pendentes, `:1716`, `:1728`. | `test_recobrar_questionario.py:122` cobre não apagar resposta, não cobre morto/zero. | Serviço avulso também protegido por app/gate (diff `disputa_emissao.py`, `+1785`). | [nao verificado]
| DS fechada/inexistente | `_disputa_do_chamado` exclui fechada `fio.py:131`, `:135`; view retorna `views.py:1411`. | Nada/push não pela view. | `test_recobrar_questionario.py:102`. | Conservado. | [nao verificado]

**Matriz — cron 08:00**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c/d/e, inclusive zero perguntas, DS aberta | Nenhum C6/ata/marco: universo é só `fechada_em IS NULL` (`alertar_disputas_pendentes.py:33`). | `DisputaSupervisao.contexto.ultimo_alerta_diario` por `renovar_contexto` `:48`, escritor `models.py:2177`; webpush colab `:59`. Empresa agrupa todas as abertas `:70`; webpush supervisão `:103`; LogAuditoria `:109`; >7 dias log `:75`. | `test_porta_anotacao_contexto.py:214` mudança de data; `:219` idempotência; não verificado teste C6 deste cron. | Somente aviso colab/carimbo passam app antes/depois commit (diff `alertar_disputas_pendentes.py`, `+41`); supervisão/agrupamento/log não alterados no diff. | [nao verificado]
| DS fechada/inexistente | Filtro inicial `alertar_disputas_pendentes.py:33`. | Nada/push não. | Mesmo filtro. | Conservado. | [nao verificado]

**C6 e app — fonte comum da cópia**

C6 mata validada, dia trancado e vias terminais (`app/chamados/juizes.py:1401`, `:1405`, `:1412`); resposta com texto pode continuar viva para decisão (`:1417`), porém app a coloca em histórico (`pergunta_no_app.py:71`). Sem ata/veredito, pergunta não materializada pode permanecer viva (`juizes.py:1476`). Portanto “sem ata” e “sem marco” isoladamente **não determinam** push; é preciso estado integral C6 e pendência app (`pergunta_no_app.py:58`, `:71`).

**Outros pushes relacionados** [nao verificado]

| Porta | Hash | Cópia |
|---|---|---|
| `acoes_disputa::acao_reabrir_disputa:90` | Corrigíveis `:97`, reabre `:103`, FCM colab `:109`; sem rechecagem C6 depois. | Gate app/commit (diff `acoes_disputa.py`, `+90`). | [nao verificado]
| `acoes_disputa::acao_regularizar_disputa:118` | Bloqueia viva sem resposta `:130`; materializa `:139`; mesmo após fechar tenta webpush/FCM “regularizada” `:150`, `:156`. | Ambos envolvidos no gate pendência, portanto fechamento/zero pendente suprime esses avisos (diff `+149`). |
| `fio::processar_post_fio:485`, regularizar | Guarda viva sem resposta `:660`; webpush/FCM regularização `:681`, `:688`. | Gate app/commit (diff `fio.py`, `+678`). | [nao verificado]
| `acoes_disputa::acao_reabrir_disputa_admin:235` | DS fechada `:255`, reabre `:259`, conversa `:263`, push supervisão `:277`, FCM colab `:288`. | Só FCM colab passa gate; supervisão permanece (diff `+293`). | [nao verificado]
| `validacao::recusar_pergunta:573` | Recusa somente respondida não validada `:600`, `:603`; recusa+reabertura atomic `:615`; FCM colab `:640`. | Gate app/commit (diff `validacao.py`, `+638`). | [nao verificado]
| `fio::processar_post_fio`, mensagem DP | Nota `:718`, webpush `:726`, FCM `:734`, sem condição C6. | Sem mudança nesse bloco no diff fornecido. |

Driver FCM verifica somente token, não pergunta (`app/chamados/fcm_utils.py:101`, `:104`); envio efetivo chama `messaging.send` em `:85`. Driver webpush verifica usuário/assinatura, não pergunta (`app/chamados/push_utils.py:114`, `:24`); supervisão recebe por setores ativos da empresa `:98`, `:100`, `:106`.

**Para invariantes:** I2 hash **NÃO FECHA**, contraexemplos individual/cron/serviço recobrar avulso (`views_cobrar.py:44`; `alertar_disputas_pendentes.py:59`; `disputa_emissao.py:1728`). Cópia **NÃO FECHA** pelo bypass do filho com dona fechada (diff `views_cobrar.py`, hunk `+20`, cruzado com `fio.py:131`). Se I2 se aplicar literalmente a todo push, mensagem do DP e supervisão também ficam fora dessa restrição (`fio.py:734`; `alertar_disputas_pendentes.py:103`). [nao verificado]

## 14/11 19:21 · sessao 2a4e6c031622 · - · - · origem=Ronald (tmux)

Escopo: `E = app/chamados/services/disputa_emissao.py`; `D = app/chamados/services/declaracao_texto.py`. Referências “cópia” abaixo são linhas novas dos hunks fornecidos, não código executado. Testes da cópia só constam na lista; cobertura e resultados deles são [nao verificado].

| ID / estado | Juiz / predicado | Persistência no hash | Push | Prova / cópia | [nao verificado]
|---|---|---|---|---|
| N1 — motivos vazios, qualquer DS | nenhum: guarda local | nada | não | E:656. Sem mudança no diff. |
| N2 — DS inexistente ou só fechadas; com ata e dia resolvível | nenhum para nascer; busca somente DS aberta | cria/reusa `ChamadoColaborador`; cria `DisputaSupervisao(motivos, contexto, chamado)` antes de perguntar | sim, para colab, independentemente do resultado do gerador | E:531,568,587,669. Cópia E::_abrir_ou_agregar_disputa novo :474: ensaio/savepoint; rollback sem motivo local, replay estrangeiro. |
| N3 — mesma entrada, sem ata ou sem marco | nascimento não consulta ata; gerador só consulta grade se texto não pode ser montado | DS/pai já persistidos; pergunta pode nascer se contexto tem data; sem dia/texto, zero perguntas | sim, para colab | E:345,350,360,371,425,669. Cópia passa pelo resolvedor novo :213 e ensaio; não equivale a exigir pergunta **viva**, pois `locais()` só exclui `VIAS_FORA_DO_FATO`. |
| N4 — DS aberta | nenhum para agregar: união local de motivos + merge contexto | atualiza `DS.motivos`, `DS.contexto`; incremento `qtd`, `ultima_em` | só se motivo novo; repetição do motivo pode produzir pergunta de outro dia sem push | E:535,548,565,682,685,686. Cópia `_contexto_do_ato` novo :387 e agregador novo :408; motivos admitidos são os das perguntas locais; contexto é salvo antes do gerador. | [nao verificado]
| G1 — pergunta do dia respondida, viva ou morta, na própria DS | nenhum/C6 ausente do dedup do gerador | dedup por existência, sem ler `respondida_em` nem `pergunta_viva`; zero nova se identidade e via correspondem | gerador não envia; wrapper segue N2/N4 | E:325,398,417. Cópia `_resolver_pergunta_por_motivo` novo :213 mantém dedup por existência. |
| G2 — pergunta canônica pertence a outra DS aberta/fechada | porta da identidade dos fatos; resolvedora + costura no reuso | `PerguntaDisputa.abrir` pode devolver pergunta estrangeira e atualizar célula/destino/lavra; emissor ignora `_criada` e sempre inclui retorno; DS recém-criada pode ficar vazia | wrapper envia mesmo assim no hash | E:444,470; models.py:2977,3020,3024,3053,3064. Cópia distingue `ReusadaDeOutraDS`, desfaz nascimento sem pergunta local e agrega à dona; dona fechada não é salva, descarte em trilha pós-commit (diff descarte_disputa.py:5). | [nao verificado]
| G3 — nenhum dia para pergunta de hora | nenhum: predicado local de ausência de dia | nenhuma pergunta; DS já nasceu no wrapper | wrapper envia no hash | E:424–430; teste existente `test_3b_reuso_746.py:23` chama com contexto vazio e espera DS. Cópia recusa `InsumoAusente` e rollback do nascimento. | [nao verificado]
| P1 — entradas pendentes | GEOMETRIA DE TURNO / `dia_do_turno_da_batida`; FURO DO DIA / `classificar_falta` | para cada dia acusado, injeta contexto e chama gerador; salva `DS.contexto` se lista retornada não vazia | nenhum nesta função | E:257,282,288,291,295. Cópia novo :136 conserva contexto anterior, encaminha motivo de estrangeira à dona e coleta criadas. |

| ID / órfãos | Juiz / predicado | Persistência no hash | Push | Prova / cópia | [nao verificado]
|---|---|---|---|---|
| O0 — parcial na lista do admin | `cartorio.parcial_so_lista` (não registrado com esse nome em JUIZES fornecido) | nada; filtro antes de DS/pai | não | E:916,940,1047. Sem mudança. |
| O1 — elegível, DS inexistente/só fechadas | nenhum para nascer: filtro aberto | cria pai e DS antes das guardas de ata/falta; seed `orfao_14h` | depende das próximas linhas | E:1096,1101,1123. Cópia mantém nascimento antecipado no agrupador; rollback específico apenas nova + estrangeiras + zero perguntas. |
| O2 — com ata, marco apagado, rota “por ata” | DIA DO CHAMADO (`data_do_chamado`); MARCOS / leitor da célula + lâmpadas | uma pergunta por marco, via `_complementar_marcos_faltantes` → `_criar_pergunta_se_couber`; agrega motivos e `ch.analisar()` | sim se não fechar e `notificar` | E:1166,1170,1171,1175,873,837,1289. Cópia admite local e encaminha estrangeira à dona; somente push com pendentes da DS nova :1365. | [nao verificado]
| O3 — sem ata, com escala vigente | `_marcos_faltantes` retorna `sem_celula`; guarda local + escala vigente | DS/pai permanecem; nenhuma pergunta; `_absteve_por_ata` impede auto-fecho | **sim no hash**, se `notificar` | E:902,908,911,1172,1279,1289. Teste prova explicitamente DS aberta com zero perguntas: `test_hx_disputa_muda_sem_ata.py:51–61`. **Cópia mantém essa DS aberta**, mas não envia push sem pendentes (novo :1365). I1 não fecha na cópia por esta linha. | [nao verificado]
| O4 — ata de trabalho sem marcos/lâmpadas | `_marcos_faltantes` trata como `sem_celula` | igual O3 | igual O3 | D:121–126; E:902. Diff não muda esse predicado. |
| O5 — sem ata **e sem escala vigente** | FURO DO DIA; exceção local `_ata_do_dia` | permite perguntar só por batidas; sem irmãs da ata | sim se sobra aberta | E:908–910; E:1200,1205,1213. Sem alteração dessa exceção. |
| O6 — falha ao ler ata | nenhum: `except` local | `return True,None`; mantém comportamento de perguntar sem bloqueio da ata | sim se sobra aberta | E:897–901. Sem alteração no diff. |
| O7 — com ata, nenhum marco faltante / classificador não acusa | `classificar_falta`, depois DISPUTA FECHA / `DS.pode_fechar` | DS/pai podem ter nascido; auto-fecha DS se C7 permite e não houve abstenção | não após auto-fecho | E:1201,1238,1278,1282,1284,1288; `test_hx_disputa_muda.py:24,44`; `test_hx_disputa_muda_sem_ata.py:63`. Cópia conserva. |
| O8 — pergunta respondida mas viva | FIO VIVO / `fio_vivo` → C6 | `_ja_perguntou` bloqueia nova; resposta não basta para decidir | push do agrupador não distingue resposta no hash | E:1415–1422; `_criar_pergunta_se_couber`:832; push:1289. Cópia usa pendência efetiva no app para push. | [nao verificado]
| O9 — pergunta morta C6=false, colaborador ativo | FIO VIVO / C6 | `_ja_perguntou` devolve falso, permite chamar `abrir`; resultado final sujeito à identidade canônica, não necessariamente nova pergunta | hash pode enviar se DS não fecha | E:1415–1422,837; models.py:2977,3064. Cópia encaminha retorno estrangeiro e restringe push; dedup do gerador direto continua G1. | [nao verificado]
| O10 — pergunta morta, colaborador não ativo | nenhum: guarda local | existência da linha bloqueia nova pergunta antes de C6 | nenhum garantido; resto da DS determina | E:1416–1420. Sem mudança. |
| O11 — intervalo explícito | nenhum juiz de ata nesta rota; contexto `marcos_faltantes` → motivo/data | cria pergunta com data do marco e destino próprio `_chi`; não chama `_ata_do_dia` nessa rota | sim se permanece aberta | E:1138–1155. Cópia fotografa fatos em `_marcos_de_intervalo`, depois passa pelo eleitor central. |
| O12 — pergunta não materializada já ligada ao chamado; dona inclusive fechada | **proxy SQL**, não C6: `materializada_em is null`; depois leitor de ata | complemento pode salvar `DS.motivos` da dona fechada | fluxo pode retornar antes do push principal | E:1070,1073,1081,1084,1093. Cópia `_agregar_motivos_na_dona` recusa salvar dona fechada e registra descarte; notifica complementos somente dona aberta. | [nao verificado]
| O13 — sem origem e sem base/data | DIA DO CONTEXTO; base por batidas do turno ou `eh_dia_trabalho` | DS/pai já nascidos, pergunta não nasce; C7 pode fechar | não se auto-fecha | E:1232,1235; `_tem_base`:816–820; E:1282. Cópia conserva. |

Chamadores:

- Nascimento: view `api/views.py::api_bater_ponto:764`; duplicata `api/views_core.py::api_bater_ponto:758`; `ponto/services/triagem_batida.py::ramo_registrar_es:735`; command `marcar_foto_ausente_retro.py:96`; `fabrica_da_celula.py::perguntar:172` e gerador novamente `:174`; este serviço é alcançado por `ponto/services/cartorio.py:1045`, `chamados/reconciliador.py:1387` e command `backfill_fio_morto_e4b.py:143`.
- Órfãos: command `processar_alertas_turno.py::_abrir_chamado_critico:385` (cron */5, `config/crons.py:362`); command `processar_alertas_avancados.py::handle:159` (*/30, `:363`); command `detectar_ausencias.py::_abrir_chamado:803` (*/5, `:364`); ação de view `acoes_disputa.py::acao_iniciar_disputa_orfaos:222`; serviço/view `cobrar_dia.py::cobrar_dia:49`; passivo `pergunta_no_app.py::perguntar:246`, alcançado pelo command `perguntar_chamados_sem_pergunta.py:20`.
- Gerador direto adicional: command `smoke_chamados.py:103`.
- `DisputaSupervisao` fechada não é selecionada como recipiente nos wrappers de nascimento (`E:531,1096`); pode ser dona de pergunta canônica estrangeira (`models.py:2977,3064`) ou recipiente do complemento escolhido por FK (`E:1073`).

F1/F2+: o diff implementa taxonomia/contrato de saída em `_gerar_perguntas_com_causas` (`JaExisteLocal`, `InsumoAusente`, `ReusadaDeOutraDS`, novo E:198), resolvedor compartilhado (`novo E:213`), eleição explícita “RAIZ-F1” em `fio.py::eleger_destino_da_pergunta` novo :752 e tratamento observado de motivo em dona fechada (`descarte_disputa.py:5`; contador I23 novo auditor :78). Mas ensaio+rollback+replay do nascimento, atomic unindo DS/pergunta em declarações e órfãos são implementação F2; writer central de destino/selos aproximam F5; contadores I7a/I7b/I23 aproximam F7. Uma matriz F1 documental completa e aprovação de contrato no anexo não são demonstradas [nao verificado].

Pergunta de lei candidata: “Quando o marco ainda exige resposta, mas a pergunta canônica pertence a uma DS fechada e o novo motivo não consta nela, qual deve ser a transição do ciclo?” O código da cópia publica I23 como “decisão na RAIZ-DA-DISPUTA F1” e só registra descarte (diff auditor novo :79; descarte_disputa.py:5); hash reusa sem mudar dona (`models.py:3064`). Não propor resposta.

## 14/11 19:19 · sessao e4253e35603f · - · - · origem=Ronald (tmux)

Achados para perguntas 2/3/8:

1. **PROCEDENTE — carga e consulta não têm o mesmo universo.** A carga lê `colaborador_id`, `conversa=False` e `data_evento` na janela (`app/chamados/services/tarefa_do_dia.py:395`); o app lê `disputa__colaborador` e `q_vivas`, sem recorte de data/conversa (`app/chamados/services/pergunta_no_app.py:52`). A identidade dos dois colaboradores é exigida na porta de escrita (`app/chamados/models.py:3573`), portanto divergência nesse campo em dados válidos não está demonstrada [nao verificado].

2. **PROCEDENTE — mesmo recorte e mesmo `agora` histórico também podem divergir.** `q_vivas` exige `validada_em IS NULL` no estado atual (`app/chamados/models.py:3468`), enquanto C6 considera inexistente a validação posterior a `agora` (`app/chamados/juizes.py:1387`). A carga não aplica `q_vivas` (`app/chamados/services/tarefa_do_dia.py:395`). Pergunta sem resposta, disputa aberta e célula acusadora, validada depois do instante consultado, pode estar nas pendentes da carga e ausente de `consulta_do_app(..., agora=agora)` (`app/chamados/services/tarefa_do_dia.py:514`; `app/chamados/juizes.py:1450`; `app/chamados/models.py:3468`).

3. **NÃO PROCEDENTE — predicado local duplicado de partição.** Ambos usam `particao_do_app`: sem resposta e disputa aberta (`app/chamados/services/pergunta_no_app.py:73`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/tarefa_do_dia.py:514`). Contudo, a partição lê `respondida_em` e `fechada_em` atuais, sem projeção temporal; resposta futura carimbada já manda a pergunta ao histórico em ambos (`app/chamados/services/pergunta_no_app.py:81`). C6 projeta resposta futura para inexistente, criando duas semânticas temporais dentro da composição (`app/chamados/juizes.py:1390`).

4. **PROCEDENTE — contraexemplo sintético reproduzido sem banco e sem arquivos.** Executei corpos reais por AST usando os helpers existentes (`app/chamados/tests/e4_apoio.py:8`; `app/chamados/tests/e3_portas_apoio.py:18`). Hipótese: pergunta 303 de `orfao_14h`, sem resposta/validação/materialização, disputa aberta, célula `furo`, marco `fim` aceso e vencido, sem tranca, fila vazia. C6 retornou viva e a consulta/partição retornou pendente 303; a tarefa publicou `fio='nada', rota=None`, com instância `razao='aceso', pergunta_id=None, acoes=[]`. C6 mantém viva a pergunta sem materialização sobre célula acusadora (`app/chamados/juizes.py:1444`; `app/chamados/juizes.py:1450`); a tarefa pula a lâmpada acesa antes de associar a pergunta (`app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:321`); `_portas` exige simultaneamente pendência e instância RESPONDER (`app/chamados/services/tarefa_do_dia.py:610`). Incidência em produção [nao verificado].

   O mesmo bloqueio estrutural pode ocorrer em isento, sem célula/ata, sem trabalho, marco sem luz/teto, ausência ou motivo fora do mapa: esses ramos removem/excluem RESPONDER antes da publicação (`app/chamados/services/tarefa_do_dia.py:203`; `app/chamados/services/tarefa_do_dia.py:219`; `app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:287`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:333`). Cada classe exige confirmar C6 vivo; não são ocorrências de produção demonstradas [nao verificado].

5. **NÃO PROCEDENTE — cópia do corpo C6 na carga.** A carga chama o juiz registrado com `hoje`, `agora` e carregador, reutilizando resultado para perguntas candidatas já julgadas (`app/chamados/services/tarefa_do_dia.py:518`; `app/chamados/services/tarefa_do_dia.py:521`; `app/chamados/services/tarefa_do_dia.py:523`). C6 encaminha carga ao corpo único (`app/chamados/juizes.py:888`), cuja tranca lê a alimentação da carga (`app/chamados/juizes.py:1405`; `app/chamados/services/carga_pergunta_viva.py:146`).

6. **NÃO PROCEDENTE — C17 ativo como juiz paralelo nos chamadores de produção rastreados.** A busca de chamadas encontrou a definição e somente o instrumento de comparação `diff_e3_sombra`, que o lê como “antes” (`app/chamados/juizes.py:1664`; `app/chamados/management/commands/diff_e3_sombra.py:127`). Seu corpo antigo continua aplicando `cobrar -> colab` (`app/chamados/juizes.py:1650`; `app/chamados/juizes.py:1709`); `_portas` exclui cobrar antes de consultar essa tabela (`app/chamados/services/tarefa_do_dia.py:618`).

7. **NÃO PROCEDENTE — relógio novo interno no caminho tarefa/carga examinado.** O instante da carga segue ao C6 e ao alimentador (`app/chamados/services/tarefa_do_dia.py:519`; `app/chamados/services/tarefa_do_dia.py:521`; `app/chamados/services/tarefa_do_dia.py:565`), e a casca recusa instante nulo ou carga de instante diferente (`app/chamados/services/tarefa_do_dia.py:657`). A API chama a consulta sem instante, conservando C6 atual e seu fallback de calendário (`app/api/views.py:994`; `app/api/views.py:2534`; `app/chamados/juizes.py:592`). Igualdade de relógio com uma carga externa, portanto, não é garantida nesses leitores (`app/api/views.py:2534`).

8. **PROCEDENTE — docstring da carga contradiz implementação.** Promete “uma consulta do app por colab (memo da carga)” (`app/chamados/services/tarefa_do_dia.py:362`), mas implementa uma partição em lote seguida do carregador C6 (`app/chamados/services/tarefa_do_dia.py:514`; `app/chamados/services/tarefa_do_dia.py:518`). A afirmação de igualdade integral com o universo do app também não vale sem as ressalvas acima (`app/chamados/services/tarefa_do_dia.py:512`).

**PERGUNTA:** a conversa pendente do app participa de algum colab-dia? O app admite conversa, mas o juiz de dia dela devolve ausência de dia de turno e a carga exclui conversa (`app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:114`; `app/chamados/services/tarefa_do_dia.py:396`). Não atribuí esse caso a um dia sem corte adicional [nao verificado].

## 14/11 19:19 · sessao 9a633ee7c38b · - · - · origem=Ronald (tmux)

**Pergunta 1 — mapa dos campos e leitores.**

| Valor | Escritor/fornecedor | Leitores de produção |
|---|---|---|
| `fio`, `rota`, `fila`, `verbos` publicados | `_portas_da_tarefa`: computa `verbos`/`fila` em `app/chamados/services/tarefa_do_dia.py:608`, donos em `:615`, fio em `:622`, rota em `:629`, publica todos em `:635`. | Projeção `porta_da_tarefa` em `app/chamados/services/tarefa_do_dia.py:645`; leitores abaixo. |
| `donos` | **Não é campo da tarefa**: conjunto local, criado em `app/chamados/services/tarefa_do_dia.py:615`, acrescido em `:617` e `:621`, consumido para `fio` em `:622`; não aparece no `out.update` de `:635`. | Apenas a seleção de fio do próprio juiz, `app/chamados/services/tarefa_do_dia.py:622`. |
| `porta_da_tarefa` | Copia `rota`, `fila`, `verbos`, `fio`, `nao_sei`, ou retorna `None` quando `rota` é falsa, `app/chamados/services/tarefa_do_dia.py:645`. | `fora_da_folha`, `vigia_fio`, `furo_sem_canal`, `beco_em_lote` e instrumento sombra, endereços abaixo. |
| Pendentes/histórico | `particao_do_app`, `app/chamados/services/pergunta_no_app.py:79`; predicado único sem resposta e disputa aberta, `:81`; `consulta_do_app` fornece vivas em `:73`. | API `app/api/views.py:994`, `:2534`; dashboard `app/core/views.py:253`; aviso `app/chamados/services/aviso_pendencia.py:9`; cobrança `app/chamados/services/cobranca.py:48`; projeção/badge `app/chamados/services/pergunta_no_app.py:100`; carga `app/chamados/services/tarefa_do_dia.py:514`. |
| Vivas do app | `perguntas_do_app` seleciona pelo colaborador e `q_vivas`, `app/chamados/services/pergunta_no_app.py:52`; C6 em `:61`. | Consulta `:73`; C17 `_no_app`, `app/chamados/juizes.py:1642`; ficha, `app/colaboradores/services/ficha.py:166`; diagnóstico `pergunta_no_app.py:208`, `:231`. |

**Leitores diretos da porta, com o que efetivamente usam:**

- Folha `_porta` chama projeção em `app/folha/fora_da_folha.py:70`; `_fio_da_porta` usa `porta['fio']` ou `FIO_SEM_PORTA` em `:85`; o agregado lê esse fio em `:193`; `o_que_segura` lê fio, fila e `nao_sei` em `:238`–`:248`.
- Vigia monta portas em `app/chamados/services/vigia_fio.py:649`, preserva `nao_sei` em `:656`, conta pela rota em `:662`, admin pelo fio em `:663`, publica rota/fio em `:664`.
- Furo lê presença e rota em `app/chamados/services/furo_sem_canal.py:97`–`:99`, preserva desconhecimento em `:100`–`:102`, conta ausências em `:104`, divide rotas em `:114`–`:116`.
- Beco exclui dia com qualquer porta em `app/escala/services/cadastro_realidade.py:469`; não traduz rota/fio.
- Pauta usa indiretamente `_fio_do_dia` e compara com `FIO_SEM_PORTA`, `app/ponto/services/pautas_dos_discordantes.py:79`–`:82`.
- Sombra projeta fio/fila/rota em `app/chamados/management/commands/diff_e3_sombra.py:49`–`:55`, imprime rota/fio/fila/verbos em `:143`–`:144`.
- E4b lê **instâncias e ações**, não os campos publicados, em `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`–`:107`.

**Pergunta 3 — segundo escritor publicado: NÃO PROCEDENTE nos leitores acima.** A publicação fica em `app/chamados/services/tarefa_do_dia.py:635`, e a projeção apenas copia em `:645`; folha lê em `app/folha/fora_da_folha.py:85`, vigia lê em `app/chamados/services/vigia_fio.py:662`, furo lê em `app/chamados/services/furo_sem_canal.py:99`, beco testa presença em `app/escala/services/cadastro_realidade.py:469`.

**C17 ainda existe, mas o chamado de produção encontrado é o instrumento sombra:** `app/chamados/management/commands/diff_e3_sombra.py:127`; sua definição e escrita antigas estão em `app/chamados/juizes.py:1664` e `:1709`. A tarefa reutiliza a autoridade `fio_pelos_verbos` em `app/chamados/services/tarefa_do_dia.py:620`–`:621`, excluindo residual cobrar antes em `:618`; tabela única em `app/chamados/juizes.py:1650`–`:1661`.

**Há outros “donos” derivados, com outra unidade — PROCEDENTE como diferença semântica, não prova de segundo juiz do colab-dia:**

- Ficha deriva dono **do chamado** diretamente do verbo: cobrar → colaborador, `app/colaboradores/services/ficha.py:159`–`:162`.
- Relatório classifica **disputa** como `AGUARDA_COLAB` por `aguarda_o_colaborador`, `app/chamados/relatorios_disputa.py:24`–`:28`; essa função usa C6 e `espera_o_texto_do_colaborador`, `app/chamados/services/fio.py:172`–`:174`; respondida sem texto ainda espera colaborador, `:187`.
- Portanto uma pergunta com `respondida_em` e texto vazio está no histórico da consulta (`app/chamados/services/pergunta_no_app.py:81`–`:84`), mas pode sustentar `AGUARDA_COLAB` no relatório (`app/chamados/services/fio.py:187`, `app/chamados/relatorios_disputa.py:26`). Não chamar esses rótulos de “fio da tarefa”.

**Pergunta 6 — semântica dos leitores.**

- `rota=None` implica `porta_da_tarefa=None`, independentemente de `fio`, por `app/chamados/services/tarefa_do_dia.py:645`; há exceção explícita `fio='sistema', rota=None` em `:634`.
- `porta_app` nasce quando colab tem porta e não há `portas_vivas`, `app/chamados/services/tarefa_do_dia.py:628`–`:634`; por isso não significa necessariamente fila vazia: o cobrar residual pode continuar em `fila`/`verbos`, calculados em `:608`–`:609`.
- `porta_fila`/`porta_pergunta` são escolhidas conforme a composição de portas, `app/chamados/services/tarefa_do_dia.py:632`; não identificam necessariamente a porta particular do dono vencedor: fio é escolhido antes em `:622`.
- Folha não restringe quais strings de rota aceita: só recebe a projeção e lê fio, `app/folha/fora_da_folha.py:70`, `:85`; beco só testa presença, `app/escala/services/cadastro_realidade.py:469`; furo reconhece as três rotas no contador, `app/chamados/services/furo_sem_canal.py:114`–`:116`; vigia conta dinamicamente a chave da rota em `app/chamados/services/vigia_fio.py:662` e inicializa app em `:577`.
- `nada` e `nao_sei` são distintos na tarefa, `app/chamados/services/tarefa_do_dia.py:623`; ambos ficam sem rota, `:629`–`:630`. Folha os colapsa para `FIO_SEM_PORTA='nada'` pela ausência de porta (`app/folha/fora_da_folha.py:85`, `app/chamados/juizes.py:1652`), mas `o_que_segura` preserva a flag `nao_sei` separadamente, `fora_da_folha.py:248`. Vigia e furo também preservam a flag, `vigia_fio.py:656`, `furo_sem_canal.py:100`–`:102`.

**Retrato, templates e PDF — detalhe importante para “sem porta explícito”.**

- Retrato é agregado por pessoa: lê `grupo` do `fora_da_folha` e grava `dono` por `_fio_cru`, `app/folha/retrato_folha.py:1158`–`:1190`; grupo D torna-se `ninguem`, `:177`–`:178`, `:368`. Leitor prefere o gravado, mas tem derivação legada por status/motivo sem grupo, `:428`–`:441`.
- `ninguem` vira **balde Admin**, `app/folha/retrato_folha.py:192`; se todos os dias forem atuais/futuros, pode virar **Hoje**, `:477`–`:481`. A UI rotula esses baldes em `:197`.
- Drawer mostra a frase SEM PORTA por `FIO_DO_CARTORIO` (`app/folha/retrato_folha.py:183`) e `DESTRAVA` (`:1068`–`:1069`; origem `app/folha/fora_da_folha.py:54`); template apenas renderiza os valores em `app/templates/folha/_retrato_o_que_fazer.html:33`–`:40`.
- Lista mostra LED já montado, `app/templates/folha/_retrato_lista.html:47`; PDF mostra o mesmo rótulo do LED em `app/folha/views_retrato.py:411`–`:413`. O PDF **não imprime a frase SEM PORTA** nesse registro: colunas em `:407`–`:418`.
- Vigia de retrato conta `ninguem` pelo gravado, `app/folha/vigia_retido_calado.py:46`–`:49`.

**PERGUNTA de lei:** a exigência nova “sem porta explícito” substitui também o agrupamento legado `ninguem → Admin/Hoje` da barra/PDF, ou admite o balde com identificação no drawer? O código mantém agrupamento em `app/folha/retrato_folha.py:192`, `:477`–`:481`, rótulo Admin em `:197`, PDF pelo LED em `app/folha/views_retrato.py:413`, e SEM PORTA apenas na frase do drawer em `app/folha/retrato_folha.py:1069`.

**Pergunta 7 — comentários que permanecem incompatíveis/incompletos.**

- **PROCEDENTE:** “TODO chamado VIVO na fila ... são publicadas” no cabeçalho, `app/chamados/services/tarefa_do_dia.py:27`–`:29`, e “CHAMADO VIVO NA FILA TEM PORTA”, `:580`; residual cobrar perde dono e rota em `:618`, `:629`–`:630`.
- **PROCEDENTE:** “ações exigíveis e chamados vivos ligados ao dia conservam suas portas”, `app/chamados/services/tarefa_do_dia.py:641`, não ressalva residual cobrar que não publica rota, `:618`, `:629`–`:630`.
- **PROCEDENTE:** `_fio_do_dia` documenta somente admin/colab/nada, `app/folha/fora_da_folha.py:74`, mas retorna qualquer fio publicado, `:85`, inclusive chamado do juiz, `app/chamados/services/tarefa_do_dia.py:622`.
- **PROCEDENTE:** retrato ainda descreve grupo D como “ninguém está esperando ninguém”, `app/folha/retrato_folha.py:361`–`:362`; nova frase diz ausência de ação alcançável, `app/folha/fora_da_folha.py:54`. Há chamado cobrar e ação interna possíveis sem porta, `app/chamados/services/tarefa_do_dia.py:618`–`:635`.
- **PROCEDENTE:** doc do beco diz `None = nenhuma ação ... por porta da fila`, `app/escala/services/cadastro_realidade.py:419`–`:420`; a projeção agora também publica app sem fila, `app/chamados/services/tarefa_do_dia.py:634`, e None também cobre sistema sem rota, `:614`, `:634`.

## 14/11 19:19 · sessao 42afa79ed43b · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **PROCEDENTE — bateria de mutantes incompleta.** `--todos` aborta no 12º registro, `sem_memo_por_colab`, porque o texto alvo já não existe; `carga_sem_relogio` também está obsoleto. Ambos ainda apontam para um teste removido. A validação do alvo ocorre antes do controle verde, portanto esses dois nem chegam a executar teste. Referências: `app/chamados/tests/e3_degrau_mutantes.py:41`, `:43`, `:87`, `:92`, `:112`; implementação atual em `app/chamados/services/tarefa_do_dia.py:514`. Execução nesta sessão: 19 registros morreram por assertion após controle verde; dois produziram `ValueError`. Resultado sem artefato persistido: [nao verificado em arquivo].

2. **PROCEDENTE — selo L129 não prova leitura efetiva da tarefa.** Ele proíbe imports/nomes e exige a presença dos imports autorizados, mas não exige chamadas nem consumo da resposta. Em mutação somente em memória, substituí todas as chamadas `carga_da_tarefa(...)` e `porta_da_tarefa(...)` por `None`, preservando imports: os três testes do selo passaram. Referências: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:24`, `:57`, `:61`; o degrau apenas executa esse selo em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:293`. Resultado experimental sem artefato persistido: [nao verificado em arquivo].

3. **NÃO PROCEDENTE — vacuidade específica de `assertEqual({}, por_colab)` por ninguém ser chamado.** A mesma execução exige `particao_do_app` exatamente uma vez. O teste de `o_que_segura` exige também um dia produzido. Ainda assim, esses testes medem chamadas; não demonstram equivalência do conjunto ao app nem consumo das pendentes retornadas. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:110`, `:114`, `:115`, `:161`, `:164`.

4. **NÃO PROCEDENTE — nova asserção de `test_e3_portas_tarefa` ficou vazia.** Exige exatamente `[p.pk]` no C6, uma alimentação do painel, verbos específicos e repetição sem queries; C6 ausente ou adicional nesta fixture falha. Referências: `app/chamados/tests/test_e3_portas_tarefa.py:139`, `:144`, `:146`, `:148`, `:152`.

5. **NÃO PROCEDENTE — `pos_suite` perdeu toda mordida contra C6 redundante.** O contrato ficou mais estreito: pergunta respondida na grade, fora dos pendentes. O mutante registrado que remove a exclusão das perguntas da grade continua produzindo assertion após controle verde. Referências: `app/chamados/tests/test_e3_pos_suite_sem_banco.py:50`, `:52`, `:65`, `:67`; mutante em `app/chamados/tests/e3_pos_suite_sem_banco.py:31`. Resultado experimental sem artefato persistido: [nao verificado em arquivo].

6. **NÃO PROCEDENTE — E4b mascara residual `cobrar` como resolvido.** A fixture mudou para disputa fechada; o teste agora exige `segue_sem_porta`, chamado reaberto, nota única e idempotência. O teste do universo exige dois dias antes e um sem porta depois. Referências: `app/chamados/tests/test_e4_passivo.py:42`, `:76`, `:81`, `:83`, `:86`, `:89`, `:117`, `:122`, `:125`.

7. **PROCEDENTE, alcance limitado — teste `o261e` deixou de demonstrar ausência de consulta paralela pelo vigia.** Antes o mock de app devia receber zero chamadas; agora a partição deve receber chamadas e a saída deve mudar. Isso demonstra influência do insumo sobre tarefa e vigia, mas um vigia com consulta paralela à mesma partição também poderia mudar e satisfazer esse teste. Referências: `app/chamados/tests/test_o261e_f5_alcance.py:264`, `:273`, `:274`, `:278`, `:280`.

8. **Item 3 coberto nos corpos testados.** C4 elimina o dono por `cobrar`; C14 conserva a porta `validar` do admin quando o colab não tem pendência; C15 publica app quando o único residual é `cobrar`. Os respectivos mutantes morreram com controle verde nesta sessão. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:58`, `:105`, `:113`; implementação em `app/chamados/services/tarefa_do_dia.py:610`, `:618`, `:629`. Equivalência real de universo/C6 não é demonstrada pela matriz: o teste da carga usa C6 mockado e `_perg` já pronto, em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:229`, `:236`, `:246`.

Executados juntos, sem banco: degrau 33, pós-suite 7 e portas-v6 10, total 50 testes verdes, processo rc 0. Referências das suítes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:19`, `app/chamados/tests/test_e3_portas_v6_sem_banco.py:15`; resultado sem artefato persistido: [nao verificado em arquivo]. Os testes ORM n1/portas/E4/o261 foram avaliados por leitura; execução: [nao verificado].

## 14/11 19:04 · sessao 062f239e93bc · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura local implementada para revisão. **18 testes sem banco passaram; RED contra a base: 19 assertions, zero errors; 15 mutantes mortos.** Runner e catálogo: [test_bo_disputa_c25_sem_banco.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/test_bo_disputa_c25_sem_banco.py:410), [bo_disputa_c25_mutacoes.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/bo_disputa_c25_mutacoes.py:2). [nao verificado]

**O selo real permanece VERMELHO em models.py**, que ficou intocado. O hunk abaixo zera essas violações quando aplicado somente em memória. Há também uma expectativa contraditória no terceiro teste HX: ele exige aberta com o mesmo classificador silencioso dos dois testes restaurados; permaneceu intocado (`chamados/tests/test_hx_disputa_muda.py:44`, `:64`, `:72`). Suíte ORM e certificação: [nao verificado]. Ruff não executou: módulo indisponível; lint: [nao verificado].

Arquivos tocados por item:

| Item | Arquivos |
|---|---|
| 1 — eleitor/escritor | `chamados/services/fio.py:752`, `:784`; `chamados/services/disputa_emissao.py:97`; `chamados/services/declaracao_texto.py:412` |
| 2 — push | `chamados/services/pergunta_no_app.py:84`; `disputa_emissao.py:1830`; `acoes_disputa.py:110`; `cobranca.py:197`; `validacao.py:641`; `chamados/views.py:1448`; `chamados/views_cobrar.py:23`; `ponto/management/commands/alertar_disputas_pendentes.py:56`; `fio.py:681` |
| 3 — restaurar os dois protegidos | `chamados/tests/test_hx_disputa_muda.py:44`, `:64`: removidas somente as duas chamadas de `_lavrar_ata_do_dia` |
| 4 — autoridade do dia | `chamados/services/disputa_emissao.py:212` |
| RED, mutantes e censos | `chamados/tests/test_bo_disputa_c25_sem_banco.py:89`; `bo_disputa_c25_mutacoes.py:2`; `bo_disputa_c25_censos.py:159` |

O eleitor único considera o fato explícito do intervalo antes do chamado genérico do dia; empate dentro do fato usa menor PK. Pergunta existente conserva seu candidato. `_destino_do_intervalo` e S160 agora delegam a ele (`chamados/services/fio.py:747`, `:752`; `chamados/services/disputa_emissao.py:97`). O escritor recebe o destino decidido, inclusive `None`, sem reeleger (`chamados/services/fio.py:784`).

Na base, S160 filtrava chamados vivos por `motivos_atendidos`, excluía candidato com pergunta não materializada, aceitava comparação de dia parcialmente desconhecida e terminava com `None` na ambiguidade após o desempate por turno/estágio (`0c8adfe5b:app/chamados/services/disputa_emissao.py:167`). Dependiam desse `None` o HIST/c1/c2 do reatamento e o reatamento imediato da pergunta recém-criada (`chamados/reconciliador.py:1559`; `chamados/services/disputa_emissao.py:361`). Agora:

- Ambiguidade do mesmo fato resolve por menor PK; esses caminhos deixam de receber esse caso (`chamados/services/fio.py:773`).
- Zero candidatos continua chegando ao caminho de órfã; candidato sem dia forte não entra na eleição (`chamados/services/fio.py:774`; `chamados/services/disputa_emissao.py:109`).
- Pergunta sem dia e sem chave deixa de obter candidato/data pela existência de um único chamado; continua sem destino derivado (`chamados/services/fio.py:767`; `chamados/reconciliador.py:1574`).
- `ESTAGIOS_TURNO` foi preservado para os outros leitores C8/I4b (`chamados/services/disputa_emissao.py:94`; `chamados/juizes.py:2212`; `chamados/management/commands/auditar_invariantes_chamados.py:698`).

A autoridade do dia é **`fio.dia_do_turno_da_linha`**, já chamada pelo nascimento: episódio passa pelo turno da batida; marco passa pelo dia da célula (`chamados/services/fio.py:1675`; `chamados/models.py:2936`; `ponto/turnos.py:2927`, `:2939`). O contexto fornece somente a chave da consulta. Texto e dedup usam a resposta da autoridade; sem resposta, o motivo é recusado como `InsumoAusente`, com `motivo_sem_dia_do_fato`, sem cascata grade/contexto/batida (`chamados/services/disputa_emissao.py:212`, `:239`).

O caminho genérico de órfãos voltou a ouvir o classificador antes de aplicar a espera por ata. Silêncio fecha a disputa muda; motivo efetivamente solicitado sem ata continua aberto, sem pergunta e sem push, contado em I7b (`chamados/services/disputa_emissao.py:1278`, `:1353`; `chamados/tests/test_bo_disputa_c25_sem_banco.py:332`, `:344`; `chamados/management/commands/auditar_invariantes_chamados.py:74`). Nos anexos fornecidos não há aval literal que autorize estender a espera ao primeiro caminho; a extensão saiu (`chamados/services/disputa_emissao.py:1278`).

Todos os envios de disputa abaixo passam pela lista **`consulta_do_app()['pendentes']`**: C6 `pergunta_viva`, resposta nula e disputa aberta. O callback relê essa lista após o commit; rollback descarta o envio (`chamados/services/pergunta_no_app.py:45`, `:61`, `:84`).

| Sítio de envio | Predicado adicional conservado |
|---|---|
| `chamados/services/disputa_emissao.py:706` | motivos novos e disputa aberta |
| `chamados/services/disputa_emissao.py:1104` | `notificar`, complemento, dona diferente da principal |
| `chamados/services/disputa_emissao.py:1369` | `notificar`, motivos e pendentes da disputa |
| `chamados/services/disputa_emissao.py:1459` | emissão da âncora; gate comum decide o aviso |
| `chamados/services/disputa_emissao.py:1804` | recobrança com pendentes; zero retorna sem cobrar |
| `chamados/services/disputa_emissao.py:1830` | silêncio por ContextVar; WebPush/FCM dentro do callback |
| `ponto/management/commands/alertar_disputas_pendentes.py:56` | carimbo diário; disputa muda nem recebe carimbo |
| `chamados/services/acoes_disputa.py:110` | reabertura de perguntas corrigíveis |
| `chamados/services/acoes_disputa.py:296` | reabertura administrativa autorizada |
| `chamados/views.py:1448` | reabertura pela supervisão |
| `chamados/services/validacao.py:641` | recusa da resposta |
| `chamados/services/cobranca.py:197` | pendências e prazo; driver compartilhado permanece em `:107`, `:112` |
| `chamados/views_cobrar.py:47` | cobrança individual/massa, permissões e throttle dos chamadores |
| `chamados/services/acoes_disputa.py:152`, `:158`; `chamados/services/fio.py:681`, `:688` | avisos de regularização também passam pelo gate literal; sem pendente são suprimidos |

“Colaborador notificado” depende agora de entrega confirmada por algum canal; a quantidade vem das perguntas pendentes, sem usar `len(orfaos)` (`chamados/services/acoes_disputa.py:233`). Agendamento e entrega têm campos separados no recibo (`chamados/services/pergunta_no_app.py:98`).

O censo completo, com **arquivo:linha, função, campo e forma de acesso**, é reproduzível pelo [censo AST](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/bo_disputa_c25_censos.py:170):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/bo_disputa_c25_censos.py --censo
```

Ele inclui atributos, filtros, argumentos, literais de `values`/`only`/`save` e templates; a classificação do modelo exige ler o sítio indicado (`chamados/tests/bo_disputa_c25_censos.py:170`). Os escritores de interesse depois da mudança são:

| Campo | Escritores |
|---|---|
| `PerguntaDisputa.chamado_destino` | `chamados/services/fio.py:792`, `:794`, `:797`; pendências reais em `chamados/models.py:367`, `:3043`, `:3067`, `:3075` |
| `DS.fechada_em` | `chamados/models.py:1988` (`fechar`), `:2205` (`reabrir_admin`), `:2229` (`reabrir_sistema`) |
| `DS.motivos` | `chamados/models.py:2022`, `:2119`, `:2215`; `chamados/services/disputa_emissao.py:422`; nascimentos em `:533`, `:1175`, `:1439` |
| `PerguntaDisputa.respondida_em` | `chamados/models.py:1555` (simulação), `:1613` (ressuscitação delegada), `:3193`, `:3224`, `:3323`; `chamados/services/disputa_emissao.py:1546`, `:1671`/`:1710`, `:1731`, `:1752` |
| push | drivers dos callbacks inventariados na tabela anterior; porta comum em `chamados/services/pergunta_no_app.py:84` |

Os principais consumidores de `chamado_destino`, incluindo FK, consultas e projeções, ficam nos sítios abaixo; o comando de censo discrimina cada ocorrência por função (`chamados/tests/bo_disputa_c25_censos.py:170`):

```text
api/views.py:1031,2548,2567,2568
chamados/models.py:304,310,311,345,361,362,366,436,443,444,1967,2083,3010,3029,3051,3057
chamados/juizes.py:779,843,857,1860,1987,2034,2346
chamados/reconciliador.py:652,1052,1502
chamados/relatorios_disputa.py:43
chamados/views_cobrar.py:188
chamados/services/acoes_chamado.py:367
chamados/services/acoes_disputa.py:362
chamados/services/becos.py:30,46,47
chamados/services/competencia_trancada.py:121,132,165
chamados/services/declaracao_texto.py:302,365,412,653
chamados/services/disputa_emissao.py:345,350,361,630,631,633,635,867,878,1004,1005,1116,1137,1333,1403,1455,1500,1870,1978,2107
chamados/services/explicador.py:122
chamados/services/fio.py:79,80,116,134,225,226,330,395,396,816,817,819,827,833,860,1079,1106,1262,1263,1264,1269,2113
chamados/services/foto_e4b.py:259
chamados/services/historico.py:96
chamados/services/janela_colab.py:64,65
chamados/services/limbo_folgas.py:205,240,241
chamados/services/materializacao.py:342,533,587,588,717,800,1000,1082,1111,1113,1248,1269,1271
chamados/services/pergunta_no_app.py:240,263,266
chamados/services/raiox.py:57,62,137
chamados/services/relavra_via.py:101,103,156
chamados/services/tarefa_do_dia.py:377
chamados/services/validacao.py:309,341,455,870
chamados/services/vigia_carimbo.py:103
chamados/management/commands/auditar_disputas.py:151
chamados/management/commands/auditar_invariantes_chamados.py:610,616,621,672,673,679,942,978,979,982,1247,1662
chamados/management/commands/backfill_veredito_lavrado.py:101
chamados/management/commands/lavrar_perguntas_stale.py:58,88
chamados/management/commands/migrar_data_evento_dia_do_turno.py:377,472
chamados/management/commands/reabrir_orfas_enterradas.py:69
chamados/management/commands/reatar_beco_folga_ao_limbo.py:87,122,124
chamados/management/commands/relavrar_via_i6.py:56
colaboradores/services/calendario.py:474
ponto/services/veredito_celula.py:187
ponto/management/commands/reconciliar_fantasmas.py:212
ponto/management/commands/reconciliar_grade.py:444
```

Leitores de `DS.fechada_em`:

```text
api/foto_recebida.py:62
api/views.py:1013,1029,2671
chamados/models.py:190,1621,1871,1935,2087,2194,2223,2280,2318,2729
chamados/juizes.py:2396
chamados/reconciliador.py:412,551,600,611,612,1050,1161,1162,1504
chamados/relatorios_disputa.py:41
chamados/signals.py:29,131,155
chamados/views.py:471,495,512,530,911,1329
chamados/views_cobrar.py:184,187
chamados/services/acoes_chamado.py:346
chamados/services/acoes_disputa.py:263,355
chamados/services/becos.py:27,56,59
chamados/services/cobranca.py:238
chamados/services/competencia_trancada.py:176,221
chamados/services/descarte_disputa.py:9
chamados/services/disputa_emissao.py:42,414,468,492,695,705,1013,1103,1145,1368,1412,1600,1980,2075,2099,2101,2160,2162
chamados/services/fila_por_causa.py:225,259,262
chamados/services/fio.py:130,134,600,633
chamados/services/foto_e4b.py:182
chamados/services/materializacao.py:591,646,1322
chamados/services/pergunta_no_app.py:71
chamados/services/regularizacao_ext.py:60
chamados/services/relavra_via.py:183,260
chamados/services/validacao.py:171,237,240,242
chamados/services/vigia_fio.py:753
chamados/management/commands/auditar_chamados_orfaos.py:41
chamados/management/commands/auditar_disputas.py:72,87,136,152,167
chamados/management/commands/auditar_invariantes_chamados.py:63,620,676,890,1851
chamados/management/commands/encerrar_perguntas_com_fato.py:82
chamados/management/commands/fechar_becos_disputa.py:32
chamados/management/commands/reabrir_orfas_enterradas.py:96,131,164,165
colaboradores/painel_cell.py:32
colaboradores/services/desligamento.py:164
colaboradores/services/drawer.py:70
colaboradores/services/situacional.py:117
colaboradores/signals.py:73
escala/signals.py:170
ponto/services/entrada_sincronizada.py:57
ponto/management/commands/alertar_disputas_pendentes.py:35
ponto/management/commands/escalonar_chamados_supervisao.py:58,149
ponto/management/commands/reconciliar_fantasmas.py:214
templates/chamados/partials/modal_fio.html:379
```

Leitores de `PerguntaDisputa.respondida_em`:

```text
api/views.py:1160,1172,2556,2570,2613,2630
api/views_mensageria.py:569,570
chamados/models.py:1900,3106,3109,3188,3192,3409,3559,3985,3992,3993,3994
chamados/juizes.py:348,852,1385,1528,1673,1680,1683
chamados/relatorios_disputa.py:21
chamados/veredito_lavrado.py:207,208
chamados/views.py:1280,1416
chamados/views_cobrar.py:192,199
chamados/services/acoes_disputa.py:96,132
chamados/services/becos.py:30
chamados/services/cobranca.py:237
chamados/services/declaracao_texto.py:406,408,674
chamados/services/disputa_emissao.py:1697,1748,1892,1910,2050,2123,2140
chamados/services/fio.py:339,660
chamados/services/foto_e4b.py:180
chamados/services/furo_sem_canal.py:81
chamados/services/historico.py:99
chamados/services/materializacao.py:675,1376
chamados/services/painel.py:67
chamados/services/passivo_validado.py:66
chamados/services/pergunta_no_app.py:71
chamados/services/raiox.py:138
chamados/services/validacao.py:416,604,609,633,952
chamados/services/verbos.py:335,336,342,345
chamados/services/vigia_fio.py:680,788
chamados/management/commands/auditar_invariantes_chamados.py:261,676,1191,1658,1689
chamados/management/commands/backfill_decisoes_pergunta.py:56,60
chamados/management/commands/curar_perguntas_em_limbo.py:46,69
ponto/management/commands/reconciliar_grade.py:445
templates/chamados/partials/modal_fio.html:185,217,220
templates/chamados/partials/_card_atendimento.html:94,100,137,155,384,385
templates/chamados/partials/_pergunta_card.html:24,29,32
```

Leitores de `DS.motivos`, somente respondendo à fila:

| Leitor | Uso / pressuposto |
|---|---|
| `chamados/models.py:2017`, `:2088`, `:2212` | lista restante, remoção/fecho e acréscimo; pressupõem agregado completo |
| `chamados/services/disputa_emissao.py:400`, `:418`, `:1127`, `:1148`, `:1804` | recorrência, união, complemento, seed e mensagem |
| `chamados/services/descarte_disputa.py:9`, `:10` | fotografia e diferença do agregado fechado |
| `chamados/services/fio.py:680`; `acoes_disputa.py:151` | corpo da notificação |
| `chamados/relatorios_disputa.py:52` | exibição |
| `chamados/management/commands/relatorio_pendencias_disputa.py:39` | exibição dos labels recebidos |
| `chamados/management/commands/auditar_disputas.py:88`, `:142`, `:169`, `:257` | contexto, relatório e comparação com perguntas; `:169` pressupõe todos agregados |
| `chamados/management/commands/auditar_invariantes_chamados.py:50` | universo dos dias; pressupõe todos agregados |
| `ponto/management/commands/alertar_disputas_pendentes.py:74` | log |
| `api/foto_recebida.py:63`, `:70` | descoberta por foto e restantes para aparar/fechar; pressupõe agregado |
| `escala/signals.py:171` | descoberta exige âncora agregada |
| `colaboradores/signals.py:74` | descoberta exige posto agregado |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | descoberta exige device agregado |
| `templates/chamados/partials/modal_fio.html:327` | habilita fluxo da âncora pelo agregado |
| `templates/chamados/detalhe_local.html:301` | exibição |

Portanto, omitir motivo sem pergunta local pode excluir a disputa dos filtros de foto, âncora, posto e aparelhos; esse comportamento ficou na fila (`api/foto_recebida.py:63`; `escala/signals.py:171`; `colaboradores/signals.py:74`; `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49`).

O selo por função detecta atribuição, `setattr`, constructor/create/update, dict local, `save(update_fields)` e apelidos de import. Hoje acusa os quatro sítios abaixo; os encaminhamentos de `models.py:2846` e `:2858` também estão no hunk, embora sejam passagem de argumento (`chamados/tests/bo_disputa_c25_censos.py:58`):

```text
chamados/models.py:367   ChamadoColaborador._vivas_entre: atributo
chamados/models.py:3043  PerguntaDisputa._nascer: atributo
chamados/models.py:3067  PerguntaDisputa._nascer: save(update_fields)
chamados/models.py:3075  PerguntaDisputa._nascer: create/update
```

Hunk exato para a sessão, com âncoras únicas verificadas pelo gerador; predicados e campos de persistência preservados (`chamados/tests/bo_disputa_c25_censos.py:17`; `chamados/tests/test_bo_disputa_c25_sem_banco.py:398`):

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -364,7 +364,8 @@
                 if competencia_do_dia_entregue(_p.data_evento, _p.disputa.colaborador.empresa, _memo) is not False:
                     continue
                 if _p.chamado_destino_id == self.pk:
-                    _p.chamado_destino = self
+                    from chamados.services.fio import gravar_destino_da_pergunta
+                    gravar_destino_da_pergunta(_p, self)
                 if pergunta_viva(_p):
                     _vivas.append(_p)
         finally:
@@ -2842,8 +2843,9 @@
         PROPRIA, `abrir_conversa`. Selo: chamados/tests/test_selo_p13_data_evento_porta_unica.py."""
         if data_evento is None:
             raise ValueError('PerguntaDisputa.abrir exige data_evento (dia do turno); conversa e abrir_conversa')
+        from chamados.services.fio import gravar_destino_da_pergunta
         p, criada, _dia = cls._sob_contrato(disputa=disputa, motivo=motivo, data_evento=data_evento, conversa=False,
-                                            chamado_destino=chamado_destino, **campos)
+                                            **gravar_destino_da_pergunta(campos, chamado_destino))
         return p, criada
 
     @classmethod
@@ -2854,8 +2856,9 @@
         de plantio nao a le como dia do turno; `ancora_ausente` e `escolha` nao plantam por hora do dia).
         Devolve (pergunta, criada)."""
         from django.utils import timezone as _tz_b2
+        from chamados.services.fio import gravar_destino_da_pergunta
         p, criada, _dia = cls._sob_contrato(disputa=disputa, motivo=motivo, data_evento=_tz_b2.localdate(), conversa=True,
-                                            chamado_destino=chamado_destino, **campos)
+                                            **gravar_destino_da_pergunta(campos, chamado_destino))
         return p, criada
 
     @classmethod
@@ -3040,8 +3043,8 @@
                         from chamados.services.fio import _costura_nao_mata_o_fio
                         _ok_f, _mot_f = _costura_nao_mata_o_fio(ex, chamado_destino)
                         if _ok_f:
-                            ex.chamado_destino = chamado_destino
-                            upd.append('chamado_destino')
+                            from chamados.services.fio import gravar_destino_da_pergunta
+                            upd.append(gravar_destino_da_pergunta(ex, chamado_destino))
                         else:
                             from core.observ import engolir as _eng_cr
                             with _eng_cr('chamados.abrir', 'trilha_costura_recusada', pergunta=ex.pk):
@@ -3064,7 +3067,8 @@
                         from chamados.veredito_lavrado import lavrar as _lv_s2
                         _lv_s2(ex, salvar=False)
                         upd += ['veredito_lavrado', 'julgado_em', 'janela_expira_em']
-                        ex.save(update_fields=upd)
+                        from chamados.services.fio import gravar_destino_da_pergunta
+                        gravar_destino_da_pergunta(ex, ex.chamado_destino, salvar=True, campos=upd)
                         cls.acordar_celulas(_linhas_antes_reuso)
                     return ex, False, _dia_fato
             # FIO-E0-V4-E1 (b): `colaborador` e `instancia_marco` da identidade sao derivados no `save` de insercao
@@ -3072,10 +3076,10 @@
             # volta 15d2 (aval E1-VOLTA-15D-UM-PONTO-DE-ESCRITA): o dia do juiz entra pelo UNICO ponto de escrita
             # (`fio.gravar_dia_do_fato`); a insercao e a mesma do `create` (instancia + `save(force_insert=True)`).
             from chamados.services.fio import gravar_dia_do_fato
-            p = cls(disputa=disputa, motivo=motivo, chamad

[truncado em 20000 de 24327 caracteres]

## 14/11 19:00 · sessao 0c7544dcf12d · PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA · censo · origem=Code

# CENSO — PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA

**Fechamento global: `[nao verificado]`.** A leitura identificou portas administrativas que corrigem resposta, decidem o dia, renovam cobrança e encerram perguntas; portanto, a validação não é a única intervenção possível. A correção pode partir de `'(sem resposta)'`, enquanto “Resolver dia” pode registrar fatos e encerrar perguntas com decisão de silêncio. (`app/chamados/services/acoes_disputa.py:353`; `app/chamados/services/acoes_disputa.py:371`; `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:323`)

**Alcances reais nos 2.381 dias: `[nao verificado]`.** Não houve execução contra banco. A sonda abaixo distingue guardas chamáveis de corpos que misturam guarda, gravação, sinais e notificações; estes últimos não são executados. (`app/chamados/services/validacao.py:398`; `app/chamados/services/cobrar_dia.py:39`; `app/chamados/services/disputa_emissao.py:1733`)

**Denominador do anexo não reconciliado:** a transcrição conferida por grupos somou **2.357 dias**, diferença de **24** para os 2.381 declarados. Não atribuí colaborador, prazo ou comportamento aos 24 não contabilizados. A contagem não equivale a provar que faltam linhas no original. `[nao verificado]`

Árvore/hash identificado pelo pedido: `3a95114a5e25f0212bff04e861bdc62960d5c042`; correspondência independente entre conteúdo e hash `[nao verificado]`. Nenhum arquivo foi criado, editado ou apagado; este Markdown é a entrega textual para gravação pela sessão. `[nao verificado]`

## (a) Método e inventário das portas

A varredura executou `ast.parse` nos Python de produção sob `app`, inspecionou `Import`, `ImportFrom`, `Call`, funções, classes, registros de admin e declarações de URLs. Foram parseados 765 arquivos, sem erro de sintaxe nesse recorte. Esse resultado não demonstra sozinho fechamento de chamadas dinâmicas, herança externa e todas as ações indiretas da árvore. `[nao verificado]`

O censo legado não foi usado como prova de fechamento: ele exclui `api/` e `admin/` e identifica POST/guardas por texto e regex. (`app/core/portas_da_tela.py:20`; `app/core/portas_da_tela.py:53`; `app/core/portas_da_tela.py:58`)

### Apelidos de import resolvidos

| Apelido | Destino | Evidência |
|---|---|---|
| `vreg` | `chamados.views_regularizacao` | `app/chamados/urls.py:6` |
| `_ac` | `chamados.services.acoes_chamado` | `app/chamados/views.py:423` |
| `_ad` | `chamados.services.acoes_disputa` | `app/chamados/services/fio.py:665` |
| `_ac_geo` | `chamados.services.acoes_chamado` | `app/chamados/services/fio.py:572` |
| `_corrig` | `chamados.models.perguntas_corrigiveis` | `app/chamados/views.py:1419` |
| `_reabrir2` | `chamados.models.reabrir_para_correcao` | `app/chamados/views.py:1435` |
| `_muda_a_folha` | `chamados.services.cobranca_folha.muda_a_folha` | `app/chamados/views_cobrar.py:88` |
| `_vc` | `ponto.services.veredito_celula.veredito_celula` | `app/ponto/views.py:3354` |

### Autoridade e significado de “sem resposta”

`is_staff` permite entrar no fio como gestor, mas várias ações exigem também permissão nominal e autoridade sobre o departamento do chamado; staff, permissão de ação e autoridade contextual são guardas distintas. (`app/chamados/views.py:35`; `app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`)

O universo de pergunta devida não pode ser convertido em `respondida_em IS NULL`: a tarefa mantém `responder/colaborador` quando a última decisão de resposta tem texto vazio. Já a validação examina o carimbo `respondida_em`. Logo, **validar = 0 nos 2.381** não está demonstrado pelo anexo. (`app/chamados/services/tarefa_do_dia.py:145`; `app/chamados/services/validacao.py:418`)

Os três relógios também não são equivalentes: C9 compara `prazo_sla`; o prazo de cobrança admite derivação pelo nascimento; `sla.vencido` usa referência de followup/nascimento e urgência. (`app/chamados/catalogo/motor.py:54`; `app/chamados/catalogo/motor.py:59`; `app/chamados/services/sla.py:17`; `app/chamados/services/sla.py:33`)

### Pergunta, disputa, fio e painel

As rotas abaixo partem de `/chamados/`, prefixo publicado no projeto. Alcance quantitativo de cada linha: `[nao verificado]`. (`app/config/urls.py:28`)

| Ação e porta | Guarda que decide | O que grava / trilha |
|---|---|---|
| **Validar pergunta** — detalhe `local/<id>/`; fio `colaborador/<id>/`; modal `modal/<id>/` | Contexto por chamado; pergunta deve estar listada. Serviço exige resposta carimbada, não validada e confirmação quando implausível. Detalhe e fio diferem quanto à disputa fechada. (`app/chamados/services/fio.py:604`; `:636`; `app/chamados/views.py:491`; `app/chamados/services/validacao.py:418`; `:429`) | `PerguntaDisputa.validada_em/validada_por`; `DecisaoPergunta.VALIDACAO`; materialização; logs. A recusa da materialização pode desfazer a validação. (`app/chamados/services/validacao.py:159`; `:164`; `:488`; `:489`; `:504`) |
| **Recusar pergunta** — mesmas telas | Motivo; não validada; respondida; não fora do fato. No fio, pergunta de disputa aberta e listada pelo chamado. (`app/chamados/services/validacao.py:591`; `:602`; `:605`; `:609`; `app/chamados/services/fio.py:671`) | `recusa_motivo`; reabertura com decisão `recusa`; nota, evento, log e push. (`app/chamados/services/validacao.py:619`; `:628`; `:633`; `:637`; `:643`; `:647`) |
| **Corrigir resposta** — mesmas telas | HH:MM, motivo, pergunta existente, não validada, disputa aberta e tipo `hora`. **Não exige resposta anterior.** (`app/chamados/services/acoes_disputa.py:336`; `:345`; `:347`; `:349`; `:353`) | Nota com conteúdo anterior; core escreve `resposta_colab/respondida_em` e decisão `RESPOSTA`; log `corrigir_resposta_admin`; depois valida/materializa. Pode ficar resposta corrigida com materialização recusada. (`app/chamados/services/acoes_disputa.py:359`; `:371`; `:375`; `:379`; `:382`; `app/chamados/services/disputa_emissao.py:1508`; `:1511`) |
| **Regularizar disputa — detalhe** | Resolução e nenhuma pergunta viva sem carimbo de resposta; guarda dentro da ação. (`app/chamados/services/acoes_disputa.py:124`; `:130`) | Materialização granular; fechamento conforme resultado; log. (`app/chamados/services/acoes_disputa.py:139`; `:144`) |
| **Regularizar disputa — fio/modal** | Possui guarda própria inline: resolução e nenhuma viva sem resposta. Não delega a guarda do detalhe. (`app/chamados/services/fio.py:684`; `:686`; `:698`) | Materialização granular; log; notificações. (`app/chamados/services/fio.py:708`; `:713`; `:719`) |
| **Reabrir disputa — fio/modal** | Usa `perguntas_corrigiveis`; sem corrigíveis recusa. Este ramo não executa a alternativa de recobrança do endpoint específico. (`app/chamados/services/acoes_disputa.py:95`; `:97`; `:98`) | `reabrir_para_correcao`; nota, push e log. (`app/chamados/services/acoes_disputa.py:103`; `:106`; `:108`; `:113`) |
| **Reabrir ou recobrar** — `local/<id>/reabrir-disputa/` | Gate condicional: só consulta `pode_agir_chamado` se não tiver `gerir_chamados`. Resolve disputa, corrigíveis e pendentes vivas dentro da view. (`app/chamados/urls.py:20`; `app/chamados/views.py:1404`; `:1410`; `:1417`; `:1420`) | Corrigíveis: reabre perguntas, nota e log. Sem corrigíveis, com pendente viva: recobra. (`app/chamados/views.py:1425`; `:1436`; `:1440`; `:1456`) |
| **Recobrança efetiva** — ramo anterior | Seleção fica inline na view; serviço não é guarda pura. (`app/chamados/views.py:1421`; `app/chamados/services/disputa_emissao.py:1697`) | Renova `ChamadoColaborador.prazo_sla`; nota, push e log `recobrar_questionario`. (`app/chamados/services/disputa_emissao.py:1725`; `:1728`; `:1735`; `:1741`) |
| **Reabertura administrativa** — fio/modal | Staff/admin_ti; motivo; justificativa mínima; disputa fechada. (`app/chamados/services/acoes_disputa.py:245`; `:251`; `:253`; `:255`) | Reabre disputa; cria pergunta de conversa; reabre chamado; nota, pushes e log. (`app/chamados/services/acoes_disputa.py:259`; `:263`; `:270`; `:274`; `:297`) |
| **Conferir data** — `disputa/pergunta/<id>/conferir-data/` | POST e `gerir_chamados`; serviço exige pergunta, justificativa, divergência pelo juiz e ausência de conferência anterior. (`app/chamados/urls.py:18`; `app/chamados/views.py:1307`; `:1309`; `app/chamados/services/validacao.py:686`; `:690`; `:693`; `:697`) | `data_conferida_em/por`; log com antes/depois e justificativa. (`app/chamados/services/validacao.py:699`; `:701`; `:702`) |
| **Aplicar declaração de texto — detalhe** | Consulta propostas pelo followup; guardas adicionais no aplicador. (`app/chamados/views.py:478`; `app/chamados/services/declaracao_texto.py:649`; `:668`) | Emite/reusa disputa/pergunta; responde pelo core; valida pelo serviço canônico. (`app/chamados/services/declaracao_texto.py:659`; `:666`; `:693`; `:696`) |
| **Cobrar por push** — `cobrar/<id>/` | `gerir_chamados`, POST, departamento, throttle por colaborador/dia; envio precisa ter sucesso. (`app/chamados/urls.py:25`; `app/chamados/views_cobrar.py:25`; `:31`; `:36`; `:51`) | Push e nota `[COBRANCA]`; esta porta não chama o renovador de prazo. (`app/chamados/views_cobrar.py:44`; `:55`) |
| **Cobrar em massa** — `cobrar/massa/` | Fila/verbo, contexto, `muda_a_folha`, throttle; `previa=1` retorna a prévia antes do envio. (`app/chamados/urls.py:31`; `app/chamados/views_cobrar.py:78`; `:92`; `:95`; `:102`; `:117`) | Push e nota por colaborador alcançado. (`app/chamados/views_cobrar.py:123`; `:132`) |
| **Cobrar colaborador pelo copiloto** — `/ia/perguntar/` | `tem_acao` e guarda real `checar_cobranca`: pergunta pendente no app e ausência de prazo futuro entre os pais. (`app/core/urls.py:15`; `app/core/views_ia.py:841`; `app/chamados/services/cobranca.py:75`; `:88`) | Prazo nos pais, notas, push e log `cobrar_colaborador`. (`app/chamados/services/cobranca.py:178`; `:181`; `:187`; `:188`) |
| **Cobrar dia** — `cobrar/dia/` | POST, `regularizar_batida`, departamento/data; serviço recusa dia ainda não terminado. Guardas posteriores estão misturadas à execução. (`app/chamados/urls.py:32`; `app/chamados/views_cobrar.py:314`; `:317`; `app/chamados/services/cobrar_dia.py:28`) | **Log antes do resultado final**; cartório com `toque=True`; possível emissão de disputa/perguntas. (`app/chamados/services/cobrar_dia.py:39`; `:41`; `:49`) |
| **Validar inline** — `validar-inline/<id>/` | Permissão, área, disputa e seleção de perguntas vivas respondidas. (`app/chamados/urls.py:33`; `app/chamados/views_cobrar.py:149`; `:158`; `:162`; `:169`) | Delega à validação canônica. (`app/chamados/views_cobrar.py:208`) |
| **Validar lote JSON/tela** — `validar-lote/`, `validar-lote/tela/` | Permissão/contexto, pergunta elegível e classe real do lote; guarda integral não isolada. (`app/chamados/urls.py:34`; `:37`; `app/chamados/views_cobrar.py:249`; `app/chamados/services/validacao.py:767`; `:770`) | Validação canônica; tela acrescenta log do lote. (`app/chamados/services/validacao.py:781`; `app/chamados/views_cobrar.py:383`) |
| **Arquivar individual** — `arquivar/<id>/` | Permissão, POST e área; endpoint individual não exige idade. (`app/chamados/urls.py:26`; `app/chamados/views_arquivar.py:26`; `:28`; `:32`) | `silenciado_ate/por/motivo/atualizado_em`, evento e nota. (`app/chamados/services/arquivo.py:166`; `:171`; `:173`; `app/chamados/views_arquivar.py:44`) |
| **Arquivar lote** — `arquivar/lote/` | Corte positivo; filtro, verbo, idade, silêncio e contexto. (`app/chamados/urls.py:28`; `app/chamados/views_arquivar.py:105`; `:125`; `:135`; `:137`) | Mesmo escritor do véu; evento por chamado. (`app/chamados/views_arquivar.py:140`; `app/chamados/services/arquivo.py:173`) |
| **Desarquivar** — `desarquivar/<id>/` | Permissão/POST/área; via de tranca tenta reabertura que pode recusar. (`app/chamados/urls.py:27`; `app/chamados/views_arquivar.py:54`; `:60`; `:70`) | Limpa véu, carimba `resgatado_em`; pode ressuscitar perguntas; nota. (`app/chamados/services/arquivo.py:202`; `:206`; `app/chamados/views_arquivar.py:86`; `:88`) |
| **Assumir/liberar responsável** — `local/<id>/atribuir/` | Gestor e contexto do chamado. (`app/chamados/urls.py:41`; `app/chamados/views.py:1010`; `:1015`) | `atribuido_a/em/atualizado_em`; nota e log quando muda. (`app/chamados/services/vinculos.py:132`; `app/chamados/views.py:1027`; `:1034`) |
| **Mensagem rápida** — `colaborador/<id>/mensagem-rapida/` | Permissão e mensagem não vazia. (`app/chamados/urls.py:40`; `app/chamados/views.py:967`; `:971`) | Abre chamado `mensagem_dp`, nota e push. (`app/chamados/views.py:973`; `:986`; `:989`) |
| **Resposta livre/status** — detalhe/fio | Contexto; conteúdo/status; transição tem guardas posteriores próprias. (`app/chamados/services/fio.py:747`; `:752`; `app/chamados/models.py:128`) | Nota pode preceder transição recusada; transição e pauta pelo executor; logs. (`app/chamados/services/fio.py:756`; `:760`; `app/chamados/services/acoes_chamado.py:304`; `:328`) |
| **Abrir modal / marcar lidas** | Gestor. **GET tem escrita.** (`app/chamados/views.py:872`; `:877`) | `FollowupChamado.lido_dp_em`, via `QuerySet.update`. (`app/chamados/views.py:880`; `app/chamados/services/nota.py:126`; `:132`) |
| **Regularização externa: criar** | Gestor/departamento e motivo. (`app/chamados/urls.py:49`; `app/chamados/views_regularizacao.py:19`; `:49`) | `RegularizacaoExterna`, chamado de rastro e vínculo. (`app/chamados/services/regularizacao_ext.py:237`; `:247`; `:257`; `:310`) |
| **Regularização externa: adicionar item** | Estado do lote, data/horários, alvo pertencente ao colaborador e consistência dos horários. (`app/chamados/urls.py:51`; `app/chamados/views_regularizacao.py:80`; `:87`; `:109`; `:120`) | `RegularizacaoItemBatida`: data, horas, ação, alvo e intervalo. (`app/chamados/services/regularizacao_ext.py:326`) |
| **Regularização externa: excluir item** | Estado do lote. (`app/chamados/urls.py:52`; `app/chamados/views_regularizacao.py:143`) | Exclui o item. (`app/chamados/services/regularizacao_ext.py:336`) |
| **Regularização externa: enviar** | Estado e existência de itens. (`app/chamados/urls.py:53`; `app/chamados/views_regularizacao.py:160`; `:163`) | Estado `aguardando` e notificações. A materialização exige confirmação e recusa qualquer disputa aberta do colaborador. (`app/chamados/services/regularizacao_ext.py:343`; `app/chamados/views_regularizacao.py:170`; `app/chamados/services/regularizacao_ext.py:52`; `:59`) |

### Outros verbos dos roteadores

São ramos reais, e sua aplicabilidade aos dias do universo depende de dados não presentes no anexo. Alcances: `[nao verificado]`.

| Ação | Porta / guarda | Gravação / trilha |
|---|---|---|
| `aprovar_excecao` | Detalhe; `_pode_aprovar`; serviço recusa encerrado, sem exigir categoria nesse ponto. (`app/chamados/views.py:426`; `app/chamados/services/acoes_chamado.py:25`) | Batida, foto, nota, transição e log. (`app/chamados/services/acoes_chamado.py:44`; `:67`; `:69`; `:71`; `:77`) |
| `autorizar_geofence` / `_advertir` | Detalhe e fio; contexto e estado. (`app/chamados/views.py:537`; `app/chamados/services/fio.py:571`; `app/chamados/services/acoes_chamado.py:182`) | Pode criar `Advertencia`; nota, transição e log. (`app/chamados/services/acoes_chamado.py:199`; `:224`; `:229`; `:230`) |
| `recusar_geofence` | Estado, motivo e batida vinculada ao colaborador. (`app/chamados/services/acoes_chamado.py:260`; `:263`; `:265`) | Retratação da batida; nota, transição e log. (`app/chamados/services/acoes_chamado.py:277`; `:281`; `:286`; `:287`) |
| `aceitar_folga_limbo` | Sugestão e template existentes. (`app/chamados/views.py:432`; `app/chamados/services/limbo_folgas.py:85`; `:88`) | Altera folgas de `TipoEscala`; log e reconciliação. (`app/chamados/services/limbo_folgas.py:92`; `:95`; `:99`) |
| `corroborar_regua` | Vínculo existente e ainda não corroborado. (`app/chamados/views.py:441`; `app/escala/regua_defesa.py:655`; `:657`) | Régua, confiança e carimbos de corroboração; nota. (`app/escala/regua_defesa.py:662`; `:666`; `:672`) |
| `realocar_regua` | Destino/template, restrição de ciclo e `julgar_vinculo`. (`app/chamados/views.py:452`; `app/escala/regua_defesa.py:305`; `:310`; `:316`) | `executar_vinculo`; nota. (`app/escala/regua_defesa.py:321`; `:333`) |
| `pedir_confirmacao_ancora` | Categoria, área e resultado da fábrica. (`app/chamados/services/acoes_disputa.py:185`; `:187`; `:190`) | Cria disputa de âncora; log. (`app/chamados/services/acoes_disputa.py:189`; `:192`) |
| `iniciar_disputa_orfaos` | Guarda real `chamado_orfao_e_regularizavel`; recusa sem alvos ou repasse recusado. (`app/chamados/services/acoes_disputa.py:219`; `:220`; `:225`) | Fábrica de disputa/perguntas; log. (`app/chamados/services/acoes_disputa.py:222`; `:229`) |
| `aplicar_proposta_escala` | Gate do registro de intenções, assinatura/vínculo recalculados e restrições da proposta. (`app/chamados/services/proposta_no_fio.py:136`; `:138`; `:141`) | Despacha ao escritor `escala.services.propositor.aplicar`; campos finais completos `[nao verificado]`. (`app/chamados/services/proposta_no_fio.py:151`; `app/core/intake.py:51`) |
| `recusar_proposta_escala` | Gate, motivo e proposta recalculada. (`app/chamados/services/proposta_no_fio.py:168`; `:170`; `:173`) | `propositor.recusar`; campos finais completos `[nao verificado]`. (`app/chamados/services/proposta_no_fio.py:176`) |

### Formulários anunciados sem ramo correspondente

| Form do modal | Resultado do rastreio |
|---|---|
| `aplicar_declaracao_texto` | Form em `app/templates/chamados/partials/modal_fio.html:140`; handler encontrado no detalhe, `app/chamados/views.py:478`, mas não no roteador do fio. |
| `confirmar_correcao_pergunta` | Form em `app/templates/chamados/partials/modal_fio.html:190`; handler de produção correspondente `[nao verificado]`. |
| `apagar_pergunta_cega` | Form em `app/templates/chamados/partials/modal_fio.html:197`; handler de produção correspondente `[nao verificado]`. |

O POST desses forms, sem conteúdo que acione o ramo de mensagem, chega ao retorno “Nada a fazer” do roteador; não foi contado como operação funcional sobre a pergunta. (`app/chamados/services/fio.py:746`; `:781`)

### Portas sobre o dia fora de chamados

| Ação / URL | Guarda | Gravação / trilha |
|---|---|---|
| **Resolver dia:** `/ponto/celula/veredito/` — trabalhou/faltou/justificado | POST e `regularizar_batida`; guardas de negócio inline no serviço. (`app/ponto/urls.py:58`; `app/ponto/views.py:3355`; `app/ponto/services/veredito_celula.py:221`; `:243`; `:249`; `:309`; `:334`) | Batidas ou ausência aprovada; `fechar_por_admin`; decisão `SILENCIO`; chamados; lavra da célula; log. (`app/ponto/services/veredito_celula.py:323`; `:337`; `:190`; `:195`; `:203`; `:352`; `:356`) |
| **Declarar folga/falta pelo copiloto** — `/ia/perguntar/` | Gates diferentes: `editar_escalas`/`regularizar_batida`; juiz real `checar_declaracao`. (`app/core/intake.py:76`; `:80`; `:90`; `:94`; `app/ponto/services/declarar_dia.py:126`) | Folga pelo escritor de folgas e fechamento do fio; falta pelo veredito da célula. A escrita pode recusar depois da prévia. (`app/ponto/services/declarar_dia.py:167`; `:169`; `:191`; `:206`; `:215`) |
| **Inverter batida** — `/ponto/batida/<id>/flip/` | Retratação, autoria, competência e tipo dentro do executor. (`app/ponto/urls.py:59`; `app/ponto/services/flip_batida.py:15`; `:19`; `:22`; `:26`) | `Batida.tipo`, trilha e recomputação. (`app/ponto/services/flip_batida.py:30`; `:39`) |
| **Retratar batida** — `/ponto/batida/<id>/retratar/` | `_pode` chamável; motivo e estado inline. (`app/ponto/urls.py:61`; `app/ponto/services/retratar_humano.py:131`; `:166`; `:170`) | Retratação e motivo estruturado pela porta canônica. (`app/ponto/services/retratar_humano.py:179`; `:181`) |
| **Desfazer retratação** — `/ponto/batida/<id>/desfazer-retratacao/` | Autoria/permissão, estado, origem e janela; `pode_desfazer` sozinho não cobre tudo. (`app/ponto/urls.py:62`; `app/ponto/services/retratar_humano.py:194`; `:197`; `:199`; `:203`) | Aprovação e limpeza da retratação, com trilha.

[truncado em 20000 de 49883 caracteres]

_5250 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
