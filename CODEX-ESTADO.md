# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 22:44** · publicado em **31/10/2026 22:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 22:44 · sessao 36fc57470bf3 · O261-E-F1C4-LEN-VIVOS · verificador · origem=Code

**Parecer: não aprovar ainda.** A decisão de retratar passou a consumir a lavra, mas o requisito **“lavra ausente em qualquer dia do universo = PAREI com o número”** não está atendido: o command conta ausência somente quando há chamados vivos, continua executando e não inclui essa ocorrência no alarme. O número de lavras ausentes em produção é **[nao verificado]**. (`app/ponto/management/commands/supra_juiz.py:245–252,278–305`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:3–5`)

A execução da suite e a confirmação do histórico Git `3e5ea58a..1594b9f2` são **[nao verificado]**; as conclusões abaixo são de leitura estática da cópia e do diff fornecido.

1. **Quem escreve e quem lê**

| Campo/valor | Escritor | Leitor no caminho afetado |
|---|---|---|
| `CelulaDia.veredito` | A porta atribui e persiste; o método do modelo delega. Chamadores produtivos: três pontos do cartório e a decisão administrativa. | O command carrega a lavra e exige `cobranca_indevida` para entrar na retratação. |
| `CelulaDia.protestos` | A mesma porta grava os códigos recebidos do cartório; a decisão administrativa preserva os anteriores. | O command procura um código pertencente à lâmpada e o passa ao texto da retratação. |
| `ChamadoColaborador.status_local` | `retratar → _retratar → reconciliar → _gravar_estado`; a transição `RECONCILIAR` leva os três estados vivos a `resolvido`, quando as guardas permitem. | O command seleciona `VIVOS`; a porta relê o estado e consulta transição, premissa e pergunta viva antes de fechar. |
| `CSF_SEM_LAVRA` | Incrementado uma vez por dia percorrido com vivos e `veredito is None`. | Impressão genérica do `Counter` e asserção do novo teste. | [nao verificado]
| `CSF_LAVRA_SEM_CODIGO` | Incrementado uma vez por dia com vivos, `cobranca_indevida` e nenhum código reconhecido nos protestos. | Impressão genérica do `Counter`; consumidor específico adicional: **[nao verificado]**. |

Fontes da tabela: `app/ponto/portas/celula.py:737–775`; `app/escala/models.py:1787–1796`; `app/ponto/services/cartorio.py:734,1044,1565`; `app/ponto/services/veredito_celula.py:330`; `app/ponto/management/commands/supra_juiz.py:164–165,211,245–252,283–295`; `app/chamados/reconciliador.py:505–548`; `app/chamados/models.py:222,279,605–657`; `app/chamados/catalogo/motor.py:24,111`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:140`.

O vocabulário vem de `_MAPA_VEREDITO`: `cobranca_indevida` corresponde a `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_SEM_FURO`. O helper novo apenas extrai esse conjunto. O administrador só pode lavrar `trabalhou`, `faltou` ou `justificado`, e sua decisão prevalece sobre o cartório. (`app/escala/models.py:1579–1599,1727`; `app/ponto/management/commands/supra_juiz.py:48–53`; `app/ponto/portas/celula.py:655–664,748–751`)

No código produtivo, a atribuição persistente de `veredito/protestos` está na porta; há também atribuição de `veredito` numa cópia exclusivamente em memória. Quando o cartório usa `save=False`, `carimbar` persiste os campos lavrados pela mesma porta. A migration citada apenas adiciona campos nulos. (`app/ponto/portas/celula.py:667–682,769–775,787–807`; `app/escala/migrations/0036_b5_celula_veredito.py:13–21`)

Para não esconder escritores de fixtures, estes são os sítios adicionais de escrita direta identificados, incluindo criação, `update`, atribuição em memória e helpers com `**kwargs`. Os diretórios da primeira coluna prefixam os arquivos da segunda; exaustividade contra escrita dinâmica arbitrária é **[nao verificado]**.

| Diretório | Arquivos:linhas |
|---|---|
| `app/api/tests/` | `test_diagnostico_escala.py:65` |
| `app/chamados/tests/` | `_celula_absolve.py:21`; `test_adesao_agrupa_so_vivo.py:45`; `test_b2d_resposta_visivel.py:97`; `test_bug117_marco_no_espelho.py:60`; `test_bug125_cobranca_cega.py:71,74,153`; `test_c2_le_o_protesto_que_cobra.py:40`; `test_c5_canal.py:28`; `test_c5_regeneracao.py:24`; `test_colab_nao_vaza_fio_mudo.py:32`; `test_e2_pergunta_so_ao_c6.py:51`; `test_emissao_reata_orfa.py:37,91`; `test_emissao_reemite_c2.py:48,61`; `test_f1c4_3_emissores.py:71`; `test_fabrica_pela_ata.py:29`; `test_fabrica_por_evento.py:27,51`; `test_fio_disputa_resolvido.py:48`; `test_fio_le_o_juiz.py:52`; `test_limbo_nao_fecha_com_filho_no_defeito.py:68`; `test_o261_c_viva_sem_chamado_do_dia.py:99`; `test_o261_resposta_vazia.py:46`; `test_o261e_f1_porta_encerramento.py:183`; `test_orfa_sem_candidato.py:41`; `test_orfas_nao_morre_na_guarda_da_porta.py:65,97`; `test_orfas_pelo_evento.py:63,306,310,369`; `test_pergunta_do_dia_em_curso.py:46,49,179,194,208,226,336`; `test_pergunta_respondida_e_marco_aceso.py:44`; `test_reabrir_orfas_enterradas.py:58,80`; `test_reconciliar_pergunta_a_premissa.py:171,223,228,240,250`; `test_reemissao_c2.py:53`; `test_resposta_tardia.py:40`; `test_selo_celula_absolveu.py:53`; `test_selo_disparo_nao_cobra_desligado.py:71`; `test_selo_emissor_fio_mudo.py:70,262`; `test_selo_f1_cego.py:90,136`; `test_selo_memo_pergunta_viva.py:130,135,144`; `test_selo_piso_i6_da_lei.py:72`; `test_tranca_de_verdade.py:60` |
| `app/colaboradores/tests/` | `test_calendario_hoje_nao_e_falta.py:74`; `test_calendario_um_juiz.py:146`; `test_tela_folga_rotulo.py:51` |
| `app/escala/tests/` | `test_cobertura_parcial_periodo.py:280`; `test_propositor_hora_errada.py:83`; `test_semana_do_posto.py:76` |
| `app/folha/tests/` | `test_furo_so_intervalo_nao_retem.py:74`; `test_haiku_fora_da_folha.py:51,54,376`; `test_o259_vigia_pausa.py:54`; `test_o48_passo4_simular_folha_lido.py:108,110`; `test_previa_cobre_pelo_juiz.py:59,63,73,80,87,94,100,105`; `test_txt_le_a_celula_e_o_juiz.py:136`; `test_vigia_retido_calado.py:79,81` |
| `app/pautas/tests/` | `test_pauta_do_dia.py:28` |
| `app/ponto/tests/` | `test_a2_precedencia.py:200`; `test_ausencias_sem_efeito.py:32,41`; `test_ausencias_sem_efeito_pelo_juiz.py:36`; `test_bateria_lavratura.py:128,292,1162`; `test_cartorio_so_chamado_de_batida.py:87`; `test_fechar3_troca_escala_gate.py:142,186,232,269`; `test_furo_cobranca_morta_reabre.py:114`; `test_furo_parcial_sem_cobranca.py:223`; `test_furos_sem_cobranca_portas.py:31`; `test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:52`; `test_o320_pauta_dos_discordantes.py:40,183,206`; `test_o322_afastado_nao_se_cobra.py:119`; `test_ordem_regen_julga_antes_do_dinheiro.py:121`; `test_orfas_vigia.py:55`; `test_selo_b5u_nunca_bateu_ve_ausencia.py:111`; `test_t31_gate_celula.py:22`; `test_varredura_classe_b_ausencia.py:80`; `test_vigia_ausencia_veredito.py:28` |
| `app/relatorios/tests/` | `test_cauda_trilha_do_cartao.py:104`; `test_dia_da_frota.py:33,103` |

Os testes que chamam `lavrar_veredito`, inclusive o novo, compartilham o escritor canônico; o selo de escritores exclui deliberadamente `tests` e `migrations` da varredura produtiva. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:83–87`; `app/ponto/tests/test_selo_veredito_so_pela_porta.py:171–176`)

Os consumidores de placar/stdout identificados são:

- **Dentro do command:** impressão de todas as chaves; TOP, exemplos e dump; alarme A6. As chaves novas aparecem no placar, mas não alimentam TOP/exemplos/dump nem o A6. (`app/ponto/management/commands/supra_juiz.py:234–252,292–314`)
- **Teste que captura stdout:** `_executa` e `_codigos_do_command`; asserções dos códigos diagnósticos, `CSF_RETRATADO` e `CSF_SEM_LAVRA`. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:89–104,110,122,128,140`)
- **Crons:** executor e dois alarmes; stdout/stderr são redirecionados aos respectivos logs. (`app/config/crons.py:580–585,1801–1802`)
- **Scripts:** `sombra.sh` lê saída para reconhecer erro de argumentos e classifica o exit code; `cron_run.sh` registra nome, empresa, duração e saída, sem interpretar o placar. (`bin/sombra.sh:381–399`; `bin/cron_run.sh:11–13,22–35`)
- **Leitores da telemetria:** pipeline, detector de crons quebrados e relatório de duração. O teste de crons quebrados aceita o exit 2 do supra como alarme de negócio. (`app/core/management/commands/pipeline_placar.py:60–109`; `app/core/services/crons_quebrados.py:24–47`; `bin/crons_duracao.py:61–76`; `app/core/tests/test_crons_quebrados.py:45`)
- **Contratos documentais:** referências a logs e à medição de `CSF_RETRATADO`. (`app/docs/CRONS.md:64–73`; `app/docs/BACKLOG.md:1297–1299`; `app/docs/PROMPTS.md:832–836`)

Outro parser específico dos logs do supra ou enumeração externa fechada de suas chaves: **[nao verificado]**.

2. **Segundo juiz e pergunta respondida**

**Não nasce outro juiz de mérito.** O helper lê o mapa existente, e o retratador passa a perguntar: **“a célula deste dia foi lavrada como `cobranca_indevida`, com qual código?”** A autoridade é o julgamento lavrado pelo cartório, pela porta da célula; isso corresponde à L-061. (`app/ponto/management/commands/supra_juiz.py:48–53,245–252`; `app/ponto/services/cartorio.py:1212–1223,1565`; `app/ponto/portas/celula.py:745–775`; `app/docs/LEIS.md:95`)

Vida do chamado, transição, premissa, pergunta viva, cobrança órfã e dia sem trabalho continuam com suas autoridades registradas. A leitura da lâmpada não substitui essas guardas. (`app/core/juizes.py:256–273`; `app/chamados/models.py:635–653`; `app/docs/JUIZES.md:40–60`)

O command **continua recalculando contradições para placar/alarme**, inclusive com `len(vivos)`. Isso permanece uma avaliação diagnóstica paralela à lavra; o diff retira dela a autorização de escrita. (`app/ponto/management/commands/supra_juiz.py:222–248`)

3. **Fonte única, relógio, porta e mordida dos testes** [nao verificado]

A decisão nova consome a fonte lavrada e extrai seu vocabulário do mapa existente. O bloco não acrescenta leitura de relógio nem atribuição direta a estado; a escrita continua delegada ao retratador canônico. Os testes congelam o relógio. (`app/ponto/management/commands/supra_juiz.py:48–53,245–285`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:33`)

`vivos` permanece baseado no conjunto completo `VIVOS`, inclusive `registrado`; `_com_fio` continua separado. Não houve estreitamento desse conjunto no novo caminho. (`app/chamados/catalogo/motor.py:24`; `app/ponto/management/commands/supra_juiz.py:211–214,259`; `app/docs/CORTES.md:255`)

A previsão estática para restaurar a costura anterior é:

| Caso novo | Falharia anteriormente? | O que efetivamente mede |
|---|---|---|
| RED: lavra `concorde`, command deriva CSF | **Sim** | Impede a classificação diagnóstica de autorizar retratação contra a lavra. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:106–114`) |
| Golden: lavra indevida, dia completo | **Não** | É controle positivo; a classificação anterior também produziria CSF. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:116–122`; `app/ponto/supra_juiz.py:443–447`) |
| MORDE: lavra indevida, command sem CSF | **Sim** | Este é o caso positivo que satisfaz o RED literal e distingue a nova autorização. A lavra é forçada pela porta na fixture. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:83–87,124–132`) |
| Sem lavra | **Sim, pela chave ausente** | Não prova que o código anterior retrataria: sem julgamento, a fixture fica sem ata e o leitor não reconstrói suas batidas. A asserção de status já poderia passar; a chave nova é a mordida. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:64–79,134–140`; `app/escala/services/leitor_celula.py:146–148,332–365`) |

Portanto, o teste chamado RED usa `concorde`, mas o caso MORDE cobre **vivo + `cobranca_indevida` lavrada**. Não falta totalmente o caso positivo; ele está sob outro nome. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:4,106–132`)

Faltam casos para ausência de lavra **sem vivos**, parada antes de outras escritas, `CSF_LAVRA_SEM_CODIGO`, recusa do retratador e verificação da nota produzida. Os quatro casos existentes não afirmam esses comportamentos. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:106–140`; `app/ponto/management/commands/supra_juiz.py:246–252,283–285`)

4. **Quando a lavra pode estar desatualizada**

| Etapa declarada | Horário/ordem |
|---|---|
| `processar_cartorio --apply` | Empresas 2/3/4: **06:27/29/31**. |
| Corrente após o cartório | `apurar_furos_diarios → disparar_perguntas_competencia → reconciliar_perguntas_orfas`, por encadeamento. |
| `supra_juiz --executar` | Empresas 2/3/4: **06:54/56/58**. |
| A6 encerrada / corrente | **07:25/27/29** e **07:33/35/37**. |

A expansão e a ordem acima vêm de `app/config/crons.py:17,29–33,580–597,1444–1469`.

O executor permanece agendado por horário. Seu `depende` não vira espera no wrapper; diferentemente da corrente encadeada, ele pode iniciar sem comprovar o término do cartório. O que efetivamente ocorreu em produção é **[nao verificado]**. (`app/config/crons.py:580,1794–1802`; `bin/cron_run.sh:11`; `app/config/crons.py:1444–1452`)

As janelas de desatualização identificáveis são:

- Alteração posterior à passada do cartório sem relavra bem-sucedida: os signals normalmente julgam, mas há saídas por `raw`, anti-recursão e tratamento de exceções; bulk/update não disparam esses signals. (`app/ponto/signals.py:78–93,131–140`; `app/chamados/signals.py:164–180`)
- Escrita sob `calado()`, situação que a fixture cria deliberadamente. (`app/ponto/services/cartorio.py:70–79`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:64–79`)
- Dia corrente: o batch considera dias anteriores a hoje, e o julgamento por evento pode retornar antes do fim do turno. (`app/ponto/management/commands/processar_cartorio.py:49–62`; `app/ponto/services/cartorio.py:1714–1719`)
- Concorrência durante o próprio executor: grade, lavra e chamados são lidos separadamente; o ramo novo não revalida impressão/versão antes da tentativa de escrita. (`app/ponto/management/commands/supra_juiz.py:128,164–165,189–211,245–285`)

**Nos dois sentidos:** lavra indevida antiga pode autorizar tentativa de retratar após surgir lastro novo; lavra antiga diferente de indevida pode impedir uma tentativa que a relavra atual autorizaria. O fechamento efetivo continua condicionado às guardas da porta — não é correto afirmar retratação indevida consumada apenas pelo primeiro cenário. (`app/ponto/management/commands/supra_juiz.py:248–285`; `app/chamados/reconciliador.py:520–526`; `app/chamados/models.py:646–653`)

5. **`emitir_furo_retroativo`: não é o mesmo efeito**

Ele passa `chamados_vivos=len(vivos)`, mas só age quando recebe **`FURO_SEM_COBRANCA`**. Esse ramo usa `cobrancas_vivas` e `chamados_total`, não `chamados_vivos`. (`app/ponto/management/commands/emitir_furo_retroativo.py:99–110`; `app/ponto/supra_juiz.py:305–362`)

O parâmetro influencia `COBRANCA_SEM_FURO`, `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_EM_DIA_COBERTO`, códigos ignorados pelo retroativo. Portanto, passar esse parâmetro ali não reproduz a autorização anterior de retratar. (`app/ponto/supra_juiz.py:443–475,501–504`; `app/ponto/management/commands/emitir_furo_retroativo.py:109–142`)

6. **Contagens, selos e literais**

- **`CSF_RETRATADO` pode ser falso.** O command incrementa sem conferir o retorno de `_retr`, que pode devolver `False` por retenção ou recusa de fechamento. É defeito herdado do bloco deslocado; o novo conjunto de candidatos continua exposto a ele. (`app/ponto/management/commands/supra_juiz.py:283–285`; `app/chamados/reconciliador.py:520–526`)
- **A6 pode dizer OK apesar de lavra ausente ou incompleta.** Sua enumeração fechada contém apenas os dois códigos antigos. (`app/ponto/management/commands/supra_juiz.py:299–305`)
- **T61 conserva os literais exigidos**, mas seu teste da guarda só compara posições textuais, não aninhamento. Ele não prova a nova autorização pela lavra. (`app/ponto/tests/test_t61_retratacao_em_seco.py:29–82`; `app/ponto/management/commands/supra_juiz.py:44,278–290`)
- **F1C4 continua aplicável ao classificador diagnóstico.** Seus testes com nomes “retrata” verificam códigos, não o estado após `supra_juiz --executar`; os três produtores declarados continuam presentes. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:30–74,163–167`; `app/ponto/management/commands/supra_juiz.py:222`)
- **O tripwire de pendência não perde sua ocorrência:** `Command.handle` continua contando/passando vivos ao classificador. A declaração e o teto não devem ser baixados por esta mudança. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:135–148,371–384`; `app/ponto/management/commands/supra_juiz.py:223–224`)
- **O selo de cobrança órfã permanece textual**, verificando presença/ordem da guarda e seu literal de placar. (`app/escala/tests/test_cobranca_orfa_regeneracao.py:181–194`)
- **O docstring ficou desatualizado:** “mudar a classificação muda o que é retratado” deixa de descrever a autorização direta, agora baseada na lavra. (`app/ponto/management/commands/supra_juiz.py:20,238–248`)

Suite verde e ausência de outros consumidores externos quebrados: **[nao verificado]**. [nao verificado]

7. **Contraexemplos e limites da conclusão**

- **Dia A sem lavra e sem chamados vivos; dia B com lavra indevida e chamado elegível.** A não é contado em `CSF_SEM_LAVRA`, e B pode ser processado normalmente. Isso contraria a exigência de parar por ausência em qualquer dia do universo. (`app/ponto/management/commands/supra_juiz.py:245–247,278–285`; `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:4–5`)
- **Dia A sem lavra e com vivo; dia B elegível.** A é contado, mas não impede B nem provoca parada ao final. Contar não satisfaz, sozinho, o “PAREI”. (`app/ponto/management/commands/supra_juiz.py:246–247,278–305`)
- **Lavra indevida sem código reconhecido.** O command preserva o chamado e conta a inconsistência, mas pode terminar normalmente. A porta aceita veredito explícito e códigos vazios, portanto essa combinação é representável pelo código. (`app/ponto/portas/celula.py:658–661,745–746`; `app/ponto/management/commands/supra_juiz.py:248–252,299–305`)
- **Lavra indevida com fechamento recusado pela porta.** O status continua vivo, enquanto o placar diz `CSF_RETRATADO`. Aqui o erro demonstrável é o contador; a recusa da porta pode estar correta. (`app/chamados/models.py:615–619,646–653`; `app/ponto/management/commands/supra_juiz.py:283–285`)

Já **dia completo/folga com chamado vivo e lavra atual sem `cobranca_indevida` não é, por si, contraexemplo à mudança**: os cortes mandam excluir o próprio chamado da formação dessa lâmpada, preservando `vivos` para outras decisões. A cura autorizada é consumir a lavra; recalcular para recuperar a retratação anterior contrariaria esse desenho. (`app/docs/CORTES.md:255,405,409,412,415`; `app/ponto/services/cartorio.py:1206–1214`; `app/docs/LEIS.md:95`)

Ocorrência e quantidade desses estados na produção: **[nao verificado]**.

## 31/10 22:41 · sessao 528fbd488e58 · - · - · origem=Ronald (tmux)

Escrita produtiva de `CelulaDia.veredito/protestos`: porta única em `app/ponto/portas/celula.py:769-775`; cópia exclusivamente em memória em `app/ponto/portas/celula.py:682`. Modelo delega em `app/escala/models.py:1787-1796`. Chamadores produtivos: cartório em `app/ponto/services/cartorio.py:734`, `:1044`, `:1565`; decisão administrativa em `app/ponto/services/veredito_celula.py:330`. Persistência conjunta após `save=False`: `carimbar` agrega `lavrados` e salva pela porta em `app/ponto/portas/celula.py:787-807`.

Valores: `derivar_veredito` usa mapa fechado, com `cobranca_indevida` associada aos dois códigos em `app/escala/models.py:1579-1599`; administrador pode escrever apenas `trabalhou/faltou/justificado`, preservando protestos anteriores, em `app/escala/models.py:1727` e `app/ponto/portas/celula.py:748-751`. Administrador vence cartório em `app/ponto/portas/celula.py:655-657`.

Leitura introduzida: command busca `data/veredito/protestos` em `app/ponto/management/commands/supra_juiz.py:164-166`; decide por lavra em `:245-252`; enumera todos os chamados em `VIVOS` em `:211` e `:259`. `VIVOS` inclui `registrado`, além de aberto/em análise: `app/chamados/catalogo/motor.py:24`.

Cadeia do status retratado:

- `retratar → _retratar`: `app/chamados/reconciliador.py:546-548`.
- Retenção pode devolver `False`; `reconciliar` pode devolver `False`: `app/chamados/reconciliador.py:520-526`.
- `reconciliar` consulta decisão e passa por núcleo de escrita: `app/chamados/models.py:605-619`.
- Autoridade da transição: `decidir`; `RECONCILIAR` leva cada estado vivo a `resolvido`: `app/chamados/catalogo/motor.py:111`; consulta em `app/chamados/models.py:637`.
- Guardas de premissa e pergunta viva permanecem: `app/chamados/models.py:646-653`.
- Núcleo atribui `status_local` em `app/chamados/models.py:222`, grava em `:279`, com tripwire de pergunta viva em `:215-221`.

Achado relevante: `CSF_RETRATADO` incrementa mesmo quando `retratar()` devolve `False`, porque retorno é ignorado em `app/ponto/management/commands/supra_juiz.py:283-285`. Isso já existia, mas a lavra passa a fornecer novos candidatos às guardas; o placar não comprova mudança de estado. `app/chamados/reconciliador.py:520-526`; `app/ponto/management/commands/supra_juiz.py:283-285`.

Não nasce segunda autoridade produtiva: leitura de `cobranca_indevida` responde “o cartório lavrou cobrança indevida neste dia?”; autoridade continua cartório/porta/mapa, conforme L-061 em `app/docs/LEIS.md:95`. Perguntas de vida, transição, premissa, regeneração e dia sem trabalho continuam registradas com seus donos em `app/core/juizes.py:256-273`. Helper do diff só extrai códigos do mapa existente, sem recalcular dia: `app/ponto/management/commands/supra_juiz.py:48-54`.

Censo extra: identifiquei 72 arquivos de testes com escrita direta, incluindo criação, update, memória e helpers por `**kw/**cel`; lista integral enviada nas mensagens anteriores. O selo produtivo deliberadamente exclui tests e migrations, percorre apenas APP, não bin: `app/ponto/tests/test_selo_veredito_so_pela_porta.py:171-176`. `regua_defesa.py` aparece no contrato regex por escrever um dict, não CelulaDia: `app/core/tests/test_contract_escritores_estado.py:39-47`. Migration correspondente apenas adiciona campos nulos: `app/escala/migrations/0036_b5_celula_veredito.py:13-23`. Exaustividade de escritas dinâmicas arbitrárias: [não verificado].

## 31/10 22:41 · sessao d002f018b61b · - · - · origem=Ronald (tmux)

Censo de consumidores, somente leitura:

- **Placar interno:** escritores `classificar_dia` → `placar[cod]`, `por_colab`, `exemplos` em `app/ponto/management/commands/supra_juiz.py:222-237`; novas chaves escritas em `:246-252`; contador completo impresso dinamicamente em `:292-295`. As chaves novas não entram em `por_colab`/exemplos/dump, alimentados apenas dentro do `for classificar_dia`, em `:234-237` e lidos em `:297-298`, `:306-314`.
- **Alarme fechado:** enumera exclusivamente `FURO_SEM_COBRANCA`, `COBRANCA_SEM_FURO`; `CSF_SEM_LAVRA`/`CSF_LAVRA_SEM_CODIGO` não causam exit 2 nem PAREI — `app/ponto/management/commands/supra_juiz.py:299-305`.
- **Único teste que captura stdout do command encontrado:** `_executa`/`_codigos_do_command`, `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:89-104`; lê código diagnóstico em `:110`, `:128`, `CSF_RETRATADO` em `:122` e `CSF_SEM_LAVRA` em `:140`. Nenhum consumidor específico de `CSF_LAVRA_SEM_CODIGO` encontrado; ocorrência consumidora externa não demonstrada [não verificado].
- **Agendas:** execução `06:54 --executar`, alarme encerrada `07:25`, corrente `07:33`, `app/config/crons.py:580-585`; stdout/stderr vai ao log em `:1801-1802`. Nenhum parser específico desses logs encontrado na cópia [não verificado].
- **Envelope e telemetria:** `bin/cron_run.sh:11-13` executa e grava nome/emp/ini/fim/dur/exit, sem placar interno; `:22-25` carimba 0/2, `:32-35` avisa outros códigos. Leitores: `app/core/management/commands/pipeline_placar.py:60-80`, `:84-109`; `app/core/services/crons_quebrados.py:24-47`; `bin/crons_duracao.py:61-76`.
- **Ensaio:** `bin/sombra.sh:381-385` captura stdout/rc e reconhece mensagens argparse no log; `:391-399` classifica rc=2 como alarme. Nenhum reconhecimento de chaves do placar nesse caminho — `bin/sombra.sh:383-399`.
- **Documentação que nomeia logs/contadores:** `app/docs/CRONS.md:64-73`; contrato de retirar `--executar` somente após `CSF_RETRATADO=0`, `app/docs/BACKLOG.md:1297-1299`, `app/docs/PROMPTS.md:832-836`. A mudança não demonstra essa medição em produção [não verificado].

**Selos citados:** conferência estática em memória, com Python `-B`, sem importar Django nem criar arquivos:

- T61 permanece satisfeito nos literais que verifica: uma declaração da constante, `False`, guarda textual presente antes de `_retr`, marcador travado e `motivo_retratacao` preservados — `app/ponto/tests/test_t61_retratacao_em_seco.py:29-82`; implementação `app/ponto/management/commands/supra_juiz.py:44`, `:278-290`. Limite do selo: `:53-55` comprova posição textual `i<j`, não aninhamento AST.
- F1C4 continua com exatamente os três produtores declarados: cartório (`1028`, `1212`), command supra (`222`), retroativo (`99`); declaração e catraca em `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:163-167`, `:200-230`. A migração não removeu a classificação diagnóstica.
- Tripwire continua encontrando dois terminais em `Command.handle`, linhas `223-224`; a função continua declarada, e `26` declarados = teto `26` — `app/core/tests/test_tripwire_contagem_de_pendencia.py:135-148`, `:371-384`. Portanto a mudança não exige baixar esse teto.
- Selo de órfã também permanece, baseado em existência/ordem textual de `cobranca_orfa` antes de `_retr` e existência do literal — `app/escala/tests/test_cobranca_orfa_regeneracao.py:181-194`. Não mede a costura lavrada.
- Não localizei enumeração fechada externa das chaves novas que necessariamente quebre [não verificado]. A enumeração fechada relevante continua sendo a do próprio A6 — `app/ponto/management/commands/supra_juiz.py:300`.

**`emitir_furo_retroativo`:** passa `chamados_vivos=len(vivos)` em `app/ponto/management/commands/emitir_furo_retroativo.py:99-108`, mas só consome `FURO_SEM_COBRANCA`, em `:109-110`. Esse código depende de `cobrancas_vivas` e `chamados_total`, não de `chamados_vivos` — `app/ponto/supra_juiz.py:305-362`. O parâmetro influencia somente os ramos `COBRANCA_SEM_FURO`/`COBRANCA_EM_DIA_SEM_TRABALHO`, em `:443-475`, ignorados pelo retroativo. Assim, não reproduz a decisão de retratar do command antigo — `app/ponto/management/commands/emitir_furo_retroativo.py:109-142`.

## 31/10 22:41 · sessao 394eb64b1e37 · - · - · origem=Ronald (tmux)

Ordem/frescor:

- O cartório está agendado às 06:27/29/31 para empresas 2/3/4; o executor às 06:54/56/58. A expansão usa `EMPRESAS=(2,3,4)` e passo padrão 2. `depende=('processar_cartorio',)` é metadado; o executor permanece cron de horário e seu wrapper executa imediatamente, sem esperar carimbo. (`app/config/crons.py:17`, `app/config/crons.py:29`, `app/config/crons.py:580`, `app/config/crons.py:597`, `app/config/crons.py:1793`, `app/config/crons.py:1801`, `bin/cron_run.sh:11`)
- Há encadeamento real cartório → apurar furos → disparar perguntas → reconciliar perguntas órfãs; esses elos esperam arquivos. O executor não pertence a essa corrente. (`app/config/crons.py:1444`, `app/config/crons.py:1452`, `app/config/crons.py:1464`, `app/config/crons.py:1469`, `app/config/crons.py:580`)
- Batch só julga `data<hoje`; a lavra de hoje depende do evento e `julgar_celula` normalmente retorna antes do fim do turno. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/services/cartorio.py:1714`)
- Batidas e criação/mudança de status do chamado normalmente relavram pelo signal. Entretanto `raw`, bulk/update, exceção engolida e anti-recursão podem impedir atualização. `calado()` também suprime; os testes deliberadamente criam esse estado. (`app/ponto/signals.py:82`, `app/ponto/signals.py:88`, `app/ponto/signals.py:131`, `app/chamados/signals.py:164`, `app/chamados/signals.py:172`, `app/ponto/services/cartorio.py:70`, `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:64`)
- O próprio executor lê três fotografias em momentos separados: grade, lavra e chamados; o ramo novo não confere impressão, versão ou atualidade antes de escrever. Assim, também existe janela de concorrência durante a execução. (`app/ponto/management/commands/supra_juiz.py:128`, `app/ponto/management/commands/supra_juiz.py:164`, `app/ponto/management/commands/supra_juiz.py:189`, `app/ponto/management/commands/supra_juiz.py:245`)
- Lavra velha `cobranca_indevida` após surgimento de furo pode autorizar tentativa indevida; lavra velha `concorde` após alteração que deveria absolver mantém chamado vivo. O efeito final depende das guardas canônicas; ocorrência concreta em produção é [não verificado]. (`app/ponto/management/commands/supra_juiz.py:248`, `app/ponto/management/commands/supra_juiz.py:260`, `app/chamados/reconciliador.py:520`, `app/chamados/reconciliador.py:525`)

Testes e lei: [nao verificado]

| Caso | Se restaurar código anterior | Observação |
|---|---|---|
| RED concorde | Falharia: antigo deriva CSF e retrata | A própria fixture exige CSF no stdout; porém o RED chamado assim não satisfaz o literal “vivo + **cobranca_indevida** lavrada”. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:4`, `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:106`) |
| Golden cobrança indevida + completo | Passaria também anteriormente | Mesmo código CSF estaria disponível no classificador; é controle positivo, sem mordida contra reversão. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:116`, `app/ponto/supra_juiz.py:443`) |
| MORDE cobrança indevida + incompleto | Falharia: antigo não derivaria CSF | Esse é o caso que satisfaz o RED literal e distingue o novo caminho positivo. A lavra é forçada pela porta, não produzida pelo julgamento corrente. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:83`, `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:124`) |
| Sem lavra | Falharia: antigo retrataria e não emitiria chave nova | Só cobre ausência com chamado vivo. Não cobre ausência em qualquer dia nem parada efetiva. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:134`, `app/ponto/management/commands/supra_juiz.py:246`) |

- Os testes usam relógio congelado; o diff não introduz nova leitura do relógio. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:33`, `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:51`, `app/ponto/management/commands/supra_juiz.py:231`)
- O requisito “lavra ausente em qualquer dia do universo = PAREI com número” não está implementado: ausência só conta se `vivos`; colaborador sem batida é pulado; não há parada por essas chaves e o alarme só enumera dois códigos anteriores. (`app/ponto/management/commands/supra_juiz.py:116`, `app/ponto/management/commands/supra_juiz.py:246`, `app/ponto/management/commands/supra_juiz.py:300`)
- A fonte canônica atual deliberadamente exclui da lavra os chamados cujos marcos acenderam ou cujo dia é folga; passa `len(_protestam)`, preservando `vivos` para emissão. A distinção decorre dos cortes citados, não deve ser resolvida estreitando `vivos`. (`app/docs/CORTES.md:255`, `app/docs/CORTES.md:405`, `app/docs/CORTES.md:409`, `app/docs/CORTES.md:412`, `app/ponto/services/cartorio.py:1206`, `app/ponto/services/cartorio.py:1214`)
- L-061 estabelece consumidor lendo lampada; a tabela registra a lavra no cartório e distingue-a da transição de estado. (`app/docs/LEIS.md:95`)

Contraexemplo adicional: o próprio MORDE cria batidas 09–16, lavra artificialmente `COBRANCA_SEM_FURO` e só verifica status. A nota produzida usa o fallback “fechou completo (todas as celulas casadas)”, que contradiz a fixture incompleta; o teste não lê essa nota. (`app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:73`, `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:85`, `app/ponto/tests/test_lenvivos_retratador_le_a_lavra.py:132`, `app/ponto/supra_juiz.py:139`)

Há ainda contador potencialmente falso: o executor incrementa `CSF_RETRATADO` sem conferir o retorno da porta, embora ela possa retornar `False` por retenção ou recusa de `reconciliar`. É problema preexistente no bloco deslocado. (`app/ponto/management/commands/supra_juiz.py:283`, `app/ponto/management/commands/supra_juiz.py:285`, `app/chamados/reconciliador.py:520`, `app/chamados/reconciliador.py:525`)

## 31/10 22:27 · sessao 74e09ba720dc · TRAVA-COBERTURA · verificador · origem=Code

**A volta 3 não fecha o contrato.** Os principais bloqueios são: rc 0 antes do término da suite com `docker run -d`; perda da proteção contra órfãos quando Docker vem envolvido por shell; registro ilegível aceito; ciclo de vida da sombra diferente do prometido; e coleta de rc defeituosa no selo novo. (`bin/trava_teste.sh:188`, `bin/trava_teste.sh:248`, `bin/trava_teste.sh:250`; `bin/trava_fila.sh:128`; `bin/sombra_trava.sh:38`; `bin/tests/test_trava_dono_do_job.sh:56`)

Execução integral dos selos, mutantes, concorrência efetiva e estado de produção: **[nao verificado]**. As mutações abaixo são previsões pela leitura, sem alteração de arquivos.

**1. Quem escreve e quem lê cada estado**

| Estado | Escritores e remoções | Leitores |
|---|---|---|
| `<trava>.fila/<pid>-<starttime>` | `_tf_grava` publica por temporário + `mv`; chamado por entrada e mudança de estado. Removem: poda, superação, saída e cancelamento. **Poda e saída não tomam `.fila.lk`.** (`bin/trava_fila.sh:58`, `bin/trava_fila.sh:67`, `bin/trava_fila.sh:88`, `bin/trava_fila.sh:119`, `bin/trava_fila.sh:129`, `bin/trava_fila.sh:133`, `bin/trava_fila.sh:160`) | `_tf_campo`, `tf_entradas`, `tf_estado`, `tf_sai`; listagem e decisões de superação/cancelamento consomem esses leitores. (`bin/trava_fila.sh:49`, `bin/trava_fila.sh:52`, `bin/trava_fila.sh:84`, `bin/trava_fila.sh:127`, `bin/trava_fila.sh:136`, `bin/trava_fila.sh:158`, `bin/trava_fila.sh:170`) |
| `.fila/voltas/<obra>` | `_tf_abre`, alcançado tanto por `tf_entra` quanto por `--abre-volta`; a gravação não tem falha conferida. (`bin/trava_fila.sh:100`, `bin/trava_fila.sh:117`, `bin/trava_fila.sh:146`) | `_tf_volta_max`, usado na abertura e na admissão. Erro de leitura vira vazio. (`bin/trava_fila.sh:95`, `bin/trava_fila.sh:98`, `bin/trava_fila.sh:111`) |
| `.fila.jsonl` | Uma função, `tf_evento`, chamada por vários processos e caminhos; `tf_sai` pode chamar fora da trava da fila. Falha de append termina em `echo`, sem impedir a operação. (`bin/trava_fila.sh:41`, `bin/trava_fila.sh:46`, `bin/trava_fila.sh:136`) | Os selos verificam `superado` e quantidade de `cancelado`; não há replay desse arquivo para decidir a fila. (`bin/tests/test_trava_dono_do_job.sh:85`, `bin/tests/test_trava_dono_do_job.sh:150`; `bin/trava_fila.sh:52`) |
| `.fila.lk` | Entrada, transição, abertura e cancelamento abrem fd 6 com `>>` e disputam `flock`; seu conteúdo não é o estado consultado. (`bin/trava_fila.sh:109`, `bin/trava_fila.sh:121`, `bin/trava_fila.sh:126`, `bin/trava_fila.sh:145`, `bin/trava_fila.sh:156`) | O leitor operacional é o próprio `flock`; `tf_lista` e `tf_sai` não o adquirem. (`bin/trava_fila.sh:133`, `bin/trava_fila.sh:168`) |
| `$TRAVA.jobs/<id>.pid` | O job publica identidade por `.pid.novo` + `mv`; não confere o sucesso antes de continuar. O esperador remove ao terminar; a coleta por idade também remove. (`bin/trava_teste.sh:123`, `bin/trava_teste.sh:149`, `bin/trava_teste.sh:164`) | Esperador aguarda tamanho positivo e lê identidade; limpeza do selo também lê. (`bin/trava_teste.sh:128`, `bin/trava_teste.sh:133`; `bin/tests/test_trava_dono_do_job.sh:37`) |
| `.out` / `.err` | Esperador cria/trunca; redirecionamento do lançamento abre novamente; job e descendentes produzem saída. Esperador e coleta por idade removem. (`bin/trava_teste.sh:123`, `bin/trava_teste.sh:125`, `bin/trava_teste.sh:127`, `bin/trava_teste.sh:149`) | Dois `tail`s; caso (d) procura marcador em `.out`. (`bin/trava_teste.sh:142`, `bin/trava_teste.sh:143`; `bin/tests/test_trava_dono_do_job.sh:123`) |
| `.rc` | `_fim` do job publica por `.rc.novo` + `mv`; esperador e coleta por idade removem. (`bin/trava_teste.sh:123`, `bin/trava_teste.sh:149`, `bin/trava_teste.sh:160`) | Esperador; selo (d); selo Docker (i). (`bin/trava_teste.sh:147`; `bin/tests/test_trava_dono_do_job.sh:127`; `bin/tests/test_trava_dono_para_filho.sh:78`) |
| `.quem` | Job escreve ocupação e, no EXIT após aquisição, “livre”. O selo antigo escreve uma mentira deliberada na cópia isolada. (`bin/trava_teste.sh:159`, `bin/trava_teste.sh:182`; `bin/tests/test_trava_teste.sh:245`) | `_quem`, depois de consultar o flock, e mensagem de timeout. **Não governa a exclusão.** (`bin/trava_teste.sh:80`, `bin/trava_teste.sh:82`, `bin/trava_teste.sh:175`) |
| Rótulo `hasner.trava.*` | A trava acrescenta `--label` somente quando os dois primeiros argumentos são literalmente `docker run`; Docker materializa o rótulo. (`bin/trava_teste.sh:50`, `bin/trava_teste.sh:188`, `bin/trava_teste.sh:235`) | `_rotulados`, `_quem`, tratamento de órfão e `_parar`. (`bin/trava_teste.sh:64`, `bin/trava_teste.sh:90`, `bin/trava_teste.sh:194`, `bin/trava_teste.sh:241`) |
| fd 9, `juliani_db_test.lock` | Job abre e adquire; o filho executado fecha fd 9. O esperador não adquire esse lock. (`bin/trava_teste.sh:120`, `bin/trava_teste.sh:173`, `bin/trava_teste.sh:174`, `bin/trava_teste.sh:248`) | Aquisição operacional; `--quem` consulta o mesmo arquivo usando **fd 8**. (`bin/trava_teste.sh:79`, `bin/trava_teste.sh:80`) |
| fd 8, `sombra.lock` | `trancar_sombra` abre/adquire no próprio chamador e exporta herança; fecha explicitamente em erros posteriores à abertura. (`bin/sombra_trava.sh:46`, `bin/sombra_trava.sh:48`, `bin/sombra_trava.sh:50`, `bin/sombra_trava.sh:59`, `bin/sombra_trava.sh:64`) | A decisão de herança consulta apenas `SOMBRA_TRAVA_HERDADA`; não comprova o descritor recebido. (`bin/sombra_trava.sh:30`) |

**Há múltiplos escritores/modificadores.** A biblioteca centraliza o formato, mas não todas as modificações sob uma porta serializada: entradas têm diversos removedores; eventos recebem múltiplos produtores; `.out/.err` recebem inicialização e produção; jobs distintos escrevem `.quem`, normalmente serializados pelo flock. `.pid/.rc` têm o job como publicador normal, mas também possuem removedores externos. (`bin/trava_fila.sh:58`, `bin/trava_fila.sh:137`, `bin/trava_teste.sh:123`, `bin/trava_teste.sh:125`, `bin/trava_teste.sh:159`, `bin/trava_teste.sh:164`)

Os códigos têm estes significados concretos:

| Código | Caminhos |
|---|---|
| **143** | Esperador sinalizado após instalar `_solta`; job recebe TERM/INT/HUP; volta recusada por `tf_entra`; entrada desaparecida antes de `rodando`; `.rc` ausente/vazio/não numérico; cancelamento normal pelo trap do job. (`bin/trava_teste.sh:139`, `bin/trava_teste.sh:148`, `bin/trava_teste.sh:163`, `bin/trava_teste.sh:168`, `bin/trava_teste.sh:180`, `bin/trava_teste.sh:245`) |
| **75** | Falha de registro inicial; timeout do flock; Docker não responde; dono rotulado ainda vivo; stop de órfão falha; órfãos persistem; suite sem rótulo não libera a pista. Portanto **não significa exclusivamente timeout** nesta implementação. (`bin/trava_teste.sh:169`, `bin/trava_teste.sh:176`, `bin/trava_teste.sh:196`, `bin/trava_teste.sh:203`, `bin/trava_teste.sh:210`, `bin/trava_teste.sh:221`, `bin/trava_teste.sh:228`) |
| **3** | `tf_entra` recusa volta antiga; a interface de teste converte para 143, e a sombra para 1. Separadamente, régua usa 3 para raia ocupada ou ordem inválida. Um comando executado que termine 3 também pode ter seu rc retransmitido. (`bin/trava_fila.sh:115`; `bin/trava_teste.sh:168`, `bin/trava_teste.sh:150`; `bin/sombra_trava.sh:43`; `bin/regua.sh:103`, `bin/regua.sh:122`) |

**2. Sinais, concorrência e falhas**

| Caso | Comportamento e rc observado pelo chamador |
|---|---|
| TERM/INT no esperador | Depois de instalar o trap: mata os tails, deixa o job e sai **143**. Nos 30 segundos iniciais ainda não há esse trap; INT herdado como ignorado também exige distinguir o ambiente de lançamento. Resultado concreto dessas variantes: [nao verificado]. (`bin/trava_teste.sh:128`, `bin/trava_teste.sh:135`, `bin/trava_teste.sh:141`) |
| KILL no esperador | Sem limpeza; se já lançado, o job continua na sessão criada por `setsid`. Quem aguarda o esperador vê término por KILL, normalmente **137**; os arquivos ficam para acompanhamento/coleta posterior. (`bin/trava_teste.sh:127`, `bin/trava_teste.sh:149`) |
| KILL no job antes de publicar `.pid` | Esperador não recebe identidade e sai **1** após sua espera de aproximadamente 30 s. (`bin/trava_teste.sh:128`, `bin/trava_teste.sh:131`, `bin/trava_teste.sh:164`) |
| KILL depois de `.pid`, antes do flock | Sem EXIT, não publica `.rc`; esperador devolve **143** quando os tails detectarem a morte. Um processo `flock` sobrevivente pode ainda esperar/adquirir e sair; ele não executa a suite. (`bin/trava_teste.sh:147`, `bin/trava_teste.sh:162`, `bin/trava_teste.sh:174`) |
| KILL depois do flock | Sem `.rc`, esperador devolve **143**. Depois do lançamento, filho/container pode sobreviver sem fd 9; a aquisição seguinte tenta reconhecer e parar o órfão rotulado. (`bin/trava_teste.sh:148`, `bin/trava_teste.sh:194`, `bin/trava_teste.sh:208`, `bin/trava_teste.sh:248`) |
| Superado esperando flock | Superação envia TERM ao grupo, ou aos filhos diretos e PID; remove entrada e registra `superado`/RELATO. Caminho normal do job: **143**, sem executar comando. A checagem após adquirir o flock também impede executar uma entrada já removida. (`bin/trava_fila.sh:73`, `bin/trava_fila.sh:87`, `bin/trava_fila.sh:89`; `bin/trava_teste.sh:163`, `bin/trava_teste.sh:180`) |
| `--abre-volta` concorrente com `tf_entra` | Ambos disputam fd 6. Abertura primeiro: velha é recusada, **3 interno → 143 externo**. Entrada primeiro: abertura remove a velha se ainda `fila`. Se `tf_estado(..., rodando)` ganhar antes, ela permanece. Não há essa corrida sem serialização entre esses três caminhos. (`bin/trava_fila.sh:85`, `bin/trava_fila.sh:109`, `bin/trava_fila.sh:126`, `bin/trava_fila.sh:145`) |
| `--cancela-obra`, rodando e fila | Na fila sinaliza grupo/filhos; rodando sinaliza somente dono. Job de teste executa `_parar` e normalmente publica **143**. **O cancelador retorna 0 contando solicitações, sem aguardar nem comprovar morte; falha de `kill` é ignorada.** (`bin/trava_fila.sh:160`, `bin/trava_fila.sh:165`; `bin/trava_teste.sh:238`) |
| Reuso de PID | Fila e rótulo comparam starttime. Esperador descarta starttime ao passar somente PID para `tail --pid`, podendo esperar um processo reciclado. Há também intervalo entre validar identidade na enumeração e sinalizar. (`bin/trava_fila.sh:36`, `bin/trava_fila.sh:58`, `bin/trava_fila.sh:87`; `bin/trava_teste.sh:62`, `bin/trava_teste.sh:133`, `bin/trava_teste.sh:142`) |
| `.rc` ausente/truncado | Ausente, vazio ou não numérico: **143**. Prefixo truncado ainda numérico é aceito. Não há limite 0–255: conteúdo `256` chega a `exit 256`, que resulta em **0**. Publicação normal por rename evita observar uma escrita parcial normal, mas não valida corrupção. (`bin/trava_teste.sh:147`, `bin/trava_teste.sh:150`, `bin/trava_teste.sh:160`) |
| Job nunca escreve `.pid` em 30 s | Esperador sai **1**, sem cancelar o lançamento. Um job atrasado ou com publicação de pidfile falhada pode continuar e executar depois dessa saída. (`bin/trava_teste.sh:127`, `bin/trava_teste.sh:131`, `bin/trava_teste.sh:164`, `bin/trava_teste.sh:165`) |

Na **sombra**, cancelamento rodando não possui `_parar` equivalente: envia TERM ao chamador registrado, enquanto filhos podem continuar com fd 8 herdado. Tampouco há `tf_sai` no encerramento normal; a entrada morta é posteriormente podada, sem evento `fim`. (`bin/trava_fila.sh:160`, `bin/trava_fila.sh:58`; `bin/sombra_trava.sh:6`, `bin/sombra_trava.sh:38`, `bin/sombra_trava.sh:63`)

**3. Suite sem trava, duas juntas e falso verde** [nao verificado]

- **Rc 0 sem término da suite:** `trava_teste.sh -- docker run -d ... python manage.py test ...` é aceito. A trava aguarda o cliente Docker, que pode terminar 0 depois de lançar o container; não aguarda o veredito da suite. A próxima aquisição pode considerar aquele container órfão e pará-lo. (`bin/trava_teste.sh:104`, `bin/trava_teste.sh:188`, `bin/trava_teste.sh:235`, `bin/trava_teste.sh:250`, `bin/trava_teste.sh:201`)
- **Sobreposição por wrapper:** `trava_teste.sh -- bash -c 'docker run ... manage.py test ...'` executa inicialmente sob flock, mas não recebe rótulo nem ativa a consulta de suites sem rótulo. Após KILL do job, um container sobrevivente fica fora da trava; outro wrapper pode iniciar uma segunda suite. O mesmo reconhecimento restrito afeta um caminho absoluto para Docker. (`bin/trava_teste.sh:188`, `bin/trava_teste.sh:194`, `bin/trava_teste.sh:248`)
- **Janela mesmo com Docker direto:** KILL entre lançar o cliente e materializar o container permite que a próxima aquisição consulte uma pista ainda vazia; o cliente antigo pode criar seu container depois. É uma possibilidade pela ordem do código; reprodução concorrente **[nao verificado]**. (`bin/trava_teste.sh:194`, `bin/trava_teste.sh:248`)
- **Rc 0 fabricado por arquivo inválido:** `.rc=256` passa pela validação exclusivamente numérica. Não é a publicação normal do job, mas é um caminho concreto de aceitação de estado corrompido. (`bin/trava_teste.sh:148`, `bin/trava_teste.sh:150`)
- **L-074 ainda incompleta no contexto v2:** isolamento e moldes preservam somente a distinção especial de 75 e continuam aceitando `OK` pelo grep, mesmo com processo terminado 143/137. Isso pode produzir “verde” de isolamento ou aceitar GREEN sem término coerente. (`bin/isolamento.sh:37`, `bin/isolamento.sh:38`; `bin/molde_fatia/rodar.sh:34`, `bin/molde_fatia/rodar.sh:55`, `bin/molde_fatia/rodar.sh:56`)

**4. PreToolUse: escapes, falsas recusas e ordem**

| Suite real que escapa | Falha do reconhecimento | [nao verificado]
|---|---|
| `echo inicio` seguido de newline e `docker run ... manage.py test` | Newline é consumido como whitespace; o segmento começa em `echo`. (`bin/disjuntor.py:167`, `bin/disjuntor.py:174`) |
| `if true; then docker run ... manage.py test; fi` | O segmento começa em `then`, sem tratamento da gramática do shell. (`bin/disjuntor.py:197`, `bin/disjuntor.py:216`) |
| `docker --context default run ... manage.py test` | Procura `run` somente em `resto[1:3]`. (`bin/disjuntor.py:216`) |
| `sudo -u ronald docker run ...`; `env -C /tmp docker run ...` | Não consome corretamente argumentos textuais das opções dos prefixos. (`bin/disjuntor.py:199`) |
| `docker run > saida --rm img python manage.py test` | Redirecionamento divide o comando antes dos argumentos identificadores. (`bin/disjuntor.py:175`) |
| `docker run ... python manage.py --settings=config.settings.ci test ponto` | Exige `manage.py` imediatamente seguido de `test`. (`bin/disjuntor.py:160`) |
| `D=docker; "$D" run ...`; `eval 'docker run ...'`; `xargs docker run ...` | Não resolve expansão ou esses executores. Scripts chamados também não são abertos por esse predicado. (`bin/disjuntor.py:188`, `bin/disjuntor.py:207`, `bin/disjuntor.py:208`, `bin/disjuntor.py:216`) |

| Comando legítimo recusado | Motivo |
|---|---|
| `bash bin/trava_teste.sh -- bash -c 'docker run ... manage.py test'` | Ao ver `bash`, procura `-c` em todos os argumentos, inclusive argumentos do script executado; perde o fato de que a trava envolve a execução. (`bin/disjuntor.py:208`, `bin/disjuntor.py:210`; `bin/trava_teste.sh:248`) |
| `docker run --rm alpine echo 'python manage.py test'` | Procura o texto no conjunto dos argumentos, sem identificar o programa de teste. (`bin/disjuntor.py:216`) |
| `docker run --rm -e NOTE='manage.py test' alpine sleep 30` | Mesmo problema: valor de ambiente vira execução presumida. (`bin/disjuntor.py:216`) |
| `grep -F ';' docker run --rm img python manage.py test` ou `printf '%s\n' ';' docker run ...` | Após retirar aspas, trata o token **citado** `;` como operador e inventa um segmento Docker executável. (`bin/disjuntor.py:167`, `bin/disjuntor.py:175`) |
| Here-doc contendo `; docker run ... manage.py test` | Não distingue corpo de here-doc de comandos executados. (`bin/disjuntor.py:174`, `bin/disjuntor.py:175`) |

As chamadas canônicas atuais passam: `suite.sh` usa `exec env ... bash trava -- docker run`; régua e pre-push usam `bash trava -- docker run`. Um `grep -n "docker run ... manage.py test" arquivo` simples também passa. Isso não prova todas as combinações possíveis dos argumentos adicionais. (`bin/suite.sh:96`; `bin/regua.sh:165`; `bin/pre-push.sh:142`; `bin/disjuntor.py:207`)

A ordem é **chave → suite → disjuntor existente**. A chave conserva prioridade; a recusa de suite alcança sessão principal e disjuntor desligado. Porém, quando recusa, impede executar B1/B3/B4 naquela chamada; a ordem relativa dessas guardas dentro de `pretool` permanece. (`bin/disjuntor.py:1568`, `bin/disjuntor.py:1569`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:1517`, `bin/disjuntor.py:1522`, `bin/disjuntor.py:1533`)

**5. Mordida dos selos e isolamento**

Antes dos mutantes há um defeito no próprio selo: `_rc()` executa `wait`, mas seus chamadores usam `$(_rc ...)`, portanto o `wait` ocorre num subshell que não é pai dos processos lançados pelo teste. As comparações de rc positivo em **(b), (e), (f)** não recuperam corretamente o resultado desses filhos; a comparação negativa de **(a)** também fica enfraquecida. (`bin/tests/test_trava_dono_do_job.sh:56`, `bin/tests/test_trava_dono_do_job.sh:75`, `bin/tests/test_trava_dono_do_job.sh:90`, `bin/tests/test_trava_dono_do_job.sh:99`, `bin/tests/test_trava_dono_do_job.sh:147`, `bin/tests/test_trava_dono_do_job.sh:175`)

Nesta tabela, “não percebe” refere-se **às asserções do caso**, não à passagem garantida do selo inteiro; para casos afetados, pressupõe corrigir a coleta de rc. Resultado executado dos mutantes: **[nao verificado]**.

| Caso | Mutação mínima que suas asserções reprovam | Mutação que o caso não percebe |
|---|---|---|
| **(a)** | Retirar o evento `superado` ou o RELATO. (`bin/trava_fila.sh:89`, `bin/trava_fila.sh:90`; selo: `bin/tests/test_trava_dono_do_job.sh:85`, `bin/tests/test_trava_dono_do_job.sh:87`) | Trocar o rc de superação para outro não zero: só exige não zero. (`bin/trava_teste.sh:163`; selo: `bin/tests/test_trava_dono_do_job.sh:76`) |
| **(b)** | Remover a condição `estado=fila` da superação, atingindo também quem roda. (`bin/trava_fila.sh:85`; selo: `bin/tests/test_trava_dono_do_job.sh:91`) | Publicar sempre rc 0: este caso só exercita comandos bem-sucedidos. Também não compara horários para provar “a volta 3 rodou depois”. (`bin/trava_teste.sh:160`; selo: `bin/tests/test_trava_dono_do_job.sh:91`, `bin/tests/test_trava_dono_do_job.sh:99`) |
| **(c)** | Retirar o `exit 3` da recusa: o atrasado pode executar. (`bin/trava_fila.sh:115`; selo: `bin/tests/test_trava_dono_do_job.sh:107`) | Trocar 3 por outro erro: não exige código exato nem evento `recusado`. (`bin/trava_fila.sh:115`; selo: `bin/tests/test_trava_dono_do_job.sh:107`) |
| **(d)** | Fazer `_solta` sinalizar o job: perde a conclusão com rc 0. (`bin/trava_teste.sh:135`; selo: `bin/tests/test_trava_dono_do_job.sh:127`) | Retirar `setsid -f`, mantendo filho comum: o teste mata somente o PID do esperador, não sua árvore; não prova sobrevivência ao TaskStop descrito no aval. (`bin/trava_teste.sh:127`; selo: `bin/tests/test_trava_dono_do_job.sh:119`) |
| **(e)** | Retirar TERM do dono rodando: trabalho de 60 s permanece além da espera do selo. (`bin/trava_fila.sh:160`; selo: `bin/tests/test_trava_dono_do_job.sh:135`, `bin/tests/test_trava_dono_do_job.sh:143`) | Gravar motivo/quem incorretos: confere quantidade, não conteúdo desses campos. (`bin/trava_fila.sh:161`; selo: `bin/tests/test_trava_dono_do_job.sh:150`) |
| **(f)** | Superar também `rodando`: mata `S2_RODANDO`. (`bin/trava_fila.sh:85`; selo: `bin/tests/test_trava_dono_do_job.sh:175`) | Retirar o fechamento de fd 8 quando a entrada desaparece: o wrapper do selo sai imediatamente e fecha o fd de qualquer maneira. (`bin/sombra_trava.sh:50`; selo: `bin/tests/test_trava_dono_do_job.sh:160`) |
| **(g)** | Retirar `nega_suite_fora_da_trava` de `main_pretool`. (`bin/disjuntor.py:1569`; selo: `bin/tests/test_trava_dono_do_job.sh:193`) | Condicionar a guarda a `not desligado()`, ou excluir agentes. O selo usa `DISJUNTOR=0`, mas a chave real é arquivo; nenhum payload leva `agent

[truncado em 20000 de 27400 caracteres]

## 31/10 22:25 · sessao 10a894de865c · - · - · origem=Ronald (tmux)

Achados por leitura estática; reprodução dos cenários e resultados dos selos [nao verificado].

1. **Registro ilegível não fecha a porta.** `tf_estado` só exige arquivo existente; lê campos sem validar e regrava `rodando` mesmo com obra/volta ausentes. O job aceita esse sucesso e executa. A leitura da maior volta trata erro como ausência, e sua escrita não tem retorno conferido. (`bin/trava_fila.sh:49`, `bin/trava_fila.sh:95`, `bin/trava_fila.sh:100`, `bin/trava_fila.sh:128`, `bin/trava_fila.sh:129`, `bin/trava_teste.sh:180`)

2. **Mais de um caminho modifica os registros.** `_tf_grava` cria/substitui entradas; `_tf_supera` e `tf_cancela_obra` removem sob fd6; `tf_sai` e a poda de `tf_entradas` removem sem fd6. Eventos têm uma função escritora, mas vários processos a chamam, inclusive fora do lock da fila. (`bin/trava_fila.sh:58`, `bin/trava_fila.sh:67`, `bin/trava_fila.sh:88`, `bin/trava_fila.sh:133`, `bin/trava_fila.sh:160`, `bin/trava_fila.sh:46`)

3. **Falha de trilha vira sucesso.** `tf_evento` responde à falha de append com `echo`; não propaga a falha, e as operações podem continuar sem evento persistido. (`bin/trava_fila.sh:46`, `bin/trava_fila.sh:47`, `bin/trava_fila.sh:120`, `bin/trava_fila.sh:130`, `bin/trava_fila.sh:147`)

4. **Sinais do esperador.** Depois de instalar `_solta`, TERM/INT/HUP param apenas os tails e saem 143; o job destacado continua. Antes disso, inclusive nos 30 segundos aguardando `.pid`, não há esse trap. KILL não executa limpeza; o job foi lançado em outra sessão. INT pode estar herdado como ignorado quando a chamada veio de comando assíncrono; efeito concreto [nao verificado]. (`bin/trava_teste.sh:127`, `bin/trava_teste.sh:128`, `bin/trava_teste.sh:135`, `bin/trava_teste.sh:141`)

5. **KILL no job.** Não escreve `.rc`; o esperador usa 143. Antes da aquisição, um filho `flock` sobrevivente pode continuar esperando/adquirir e sair, mas não executa suite. Depois de lançar container direto, o container pode sobreviver sem fd9; a próxima aquisição procura seu rótulo e para o órfão. Existe janela entre lançar o cliente Docker e ele criar o container: outro job pode consultar pista vazia antes dessa criação; concorrência efetiva nessa janela [nao verificado]. (`bin/trava_teste.sh:147`, `bin/trava_teste.sh:173`, `bin/trava_teste.sh:194`, `bin/trava_teste.sh:201`, `bin/trava_teste.sh:208`, `bin/trava_teste.sh:248`)

6. **Superação e abertura concorrente.** Entrada, abertura e transição para `rodando` disputam o mesmo fd6. Se a abertura ganhar primeiro, entrada velha retorna 3, convertido em 143 pelo job; se a entrada velha ganhar, a abertura subsequente mata/remove sua entrada enquanto `estado=fila`; se a transição para `rodando` ganhar, não é superada. (`bin/trava_fila.sh:109`, `bin/trava_fila.sh:112`, `bin/trava_fila.sh:115`, `bin/trava_fila.sh:126`, `bin/trava_fila.sh:145`, `bin/trava_fila.sh:85`, `bin/trava_teste.sh:168`)

7. **Cancelamento.** Na fila, envia TERM ao grupo se pgid=pid; senão, aos filhos diretos e ao pid. Rodando, envia TERM somente ao dono; no job de teste, `_parar` busca containers por rótulo, tenta `docker stop`, mata/espera filho e sai 143. O comando de cancelamento conta pedidos e retorna sucesso sem comprovar morte; falha de `kill` é ignorada. (`bin/trava_fila.sh:73`, `bin/trava_fila.sh:160`, `bin/trava_fila.sh:165`, `bin/trava_teste.sh:238`)

8. **Sombra não tem a mesma propriedade de vida.** Registra o próprio BASHPID, não destaca job, não instala trap para parar filhos e não chama `tf_sai` no encerramento normal. Cancelamento de entrada `rodando` mata só esse pid; filhos herdando fd8 podem continuar e manter a trava, enquanto a entrada do dono morto é posteriormente podada. (`bin/sombra_trava.sh:6`, `bin/sombra_trava.sh:38`, `bin/sombra_trava.sh:46`, `bin/sombra_trava.sh:63`, `bin/trava_fila.sh:160`, `bin/trava_fila.sh:58`)

9. **PID reutilizado.** Identidade da fila e rótulos compara starttime; o esperador extrai só o número de PID para `tail --pid`, ignorando starttime, podendo prolongar a espera pelo processo reciclado. Há ainda intervalo entre verificar identidade e enviar sinal nas operações da fila. (`bin/trava_fila.sh:34`, `bin/trava_teste.sh:60`, `bin/trava_teste.sh:133`, `bin/trava_teste.sh:142`, `bin/trava_fila.sh:58`, `bin/trava_fila.sh:87`, `bin/trava_fila.sh:160`)

10. **`.rc` ausente/truncado.** Ausente, vazio ou não numérico vira 143; truncamento que ainda deixa dígitos é aceito como outro rc. Não há validação de faixa: conteúdo corrompido `256` passa pela regex e `exit 256` corresponde a 0; experimento [nao verificado]. A escrita normal usa arquivo temporário e `mv`, evitando leitura de prefixo durante publicação normal. (`bin/trava_teste.sh:147`, `bin/trava_teste.sh:148`, `bin/trava_teste.sh:150`, `bin/trava_teste.sh:160`)

11. **Sem `.pid` em 30 segundos.** Esperador sai 1, não cancela o lançamento nem verifica o rc de `setsid`; um job atrasado ou cujo pidfile falhou pode continuar, pois a publicação de `.pid` também não é checada antes de entrar na fila. (`bin/trava_teste.sh:127`, `bin/trava_teste.sh:128`, `bin/trava_teste.sh:131`, `bin/trava_teste.sh:164`, `bin/trava_teste.sh:165`)

12. **Outros caminhos de rc 0 sem término da suite.** A API aceita `docker run -d ... manage.py test`: aguarda o cliente Docker, não o container, e pode devolver 0 com a suite em execução. A próxima aquisição encontra dono morto e pode parar esse container. (`bin/trava_teste.sh:104`, `bin/trava_teste.sh:188`, `bin/trava_teste.sh:235`, `bin/trava_teste.sh:248`, `bin/trava_teste.sh:250`, `bin/trava_teste.sh:201`)

13. **Wrapper permite perder a proteção contra órfão.** `-- bash -c 'docker run ... manage.py test'` não recebe rótulo nem ativa busca de suite sem rótulo, pois o teste exige literalmente primeiros argumentos `docker run`. Após KILL do job, novo wrapper pode executar por cima do container sobrevivente. (`bin/trava_teste.sh:188`, `bin/trava_teste.sh:194`, `bin/trava_teste.sh:248`)

14. **Coleta pode apagar arquivos de job vivo.** O `find -mtime +2 -delete` não consulta identidade, estado ou término; outro esperador pode remover `.pid/.out/.err` de job antigo ainda vivo, prejudicando acompanhamento e futura leitura. (`bin/trava_teste.sh:123`, `bin/trava_teste.sh:128`, `bin/trava_teste.sh:142`, `bin/trava_teste.sh:147`)

15. **Colisões de obra.** O saneamento substitui caracteres e corta em 100 bytes: `A B` e `A_B`, ou nomes com mesmo prefixo de 100 caracteres, viram a mesma obra e podem cancelar/superar uma à outra. `.` e `..` também permanecem permitidos como nome do arquivo de maior volta. (`bin/trava_fila.sh:39`, `bin/trava_fila.sh:85`, `bin/trava_fila.sh:95`, `bin/trava_fila.sh:159`)

16. **Descritores herdados.** `setsid` não fecha fd6/7/8; o job substitui fd9 e o filho fecha apenas fd9. O fd7 da régua externa do integrador pode continuar herdado pelo job/filho. Uma rota concreta dos chamadores de sombra até suite que produza inversão de fd8/fd9 não foi encontrada nesta leitura [nao verificado]. (`bin/trava_teste.sh:127`, `bin/trava_teste.sh:173`, `bin/trava_teste.sh:248`, `bin/integrador.sh:51`)

## 31/10 22:24 · sessao be3b32fd55a2 · - · - · origem=Ronald (tmux)

O selo `test_trava_dono_do_job.sh` tem um defeito anterior à análise de mutantes: `_rc()` usa `wait`, mas é invocado dentro de substituição de comando, em subshell, que não é pai dos processos lançados pelo selo; portanto as comparações positivas com `0`/`143` não recuperam o rc daqueles filhos. Isso afeta (b), (e) e (f), e torna os resultados negativos de (a) menos informativos. A reprodução mínima, sem arquivos, retornou `subshell_wait_rc=-1` e `parent_wait_rc=0` nesta execução; o valor específico em outros Bash é [nao verificado]. (`bin/tests/test_trava_dono_do_job.sh:56`, `:75`, `:90`, `:99`, `:147`, `:170`, `:175`, `:180`)

As mutações abaixo são previsões da leitura, sem mutar arquivos nem rodar selos; “não percebe” refere-se às asserções daquele caso, supondo corrigida a leitura de rc acima. Resultado experimental dos mutantes: [nao verificado].

| Caso | Mutação de produção que a asserção reprova | Mutação que a asserção não percebe |
|---|---|---|
| (a) | Retirar o evento `superado` ou a linha `RELATO`. (`bin/trava_fila.sh:89`, `:90`; asserções `bin/tests/test_trava_dono_do_job.sh:85`, `:87`) | Trocar o rc da superação por outro não zero: o caso exige apenas `!=0`, ausência de marcador e trilhas. (`bin/trava_teste.sh:163`; `bin/tests/test_trava_dono_do_job.sh:76`) |
| (b) | Permitir que `_tf_supera` mate também `estado=rodando`: perde-se `X2_RODANDO_FIM`. (`bin/trava_fila.sh:85`; `bin/tests/test_trava_dono_do_job.sh:91`) | Retirar a exclusão pelo flock pode produzir os dois marcadores e rc zero: o caso não compara ordem de início/fim. Outros casos podem perceber a mudança. (`bin/trava_teste.sh:174`; `bin/tests/test_trava_dono_do_job.sh:89`, `:99`) |
| (c) | Retirar `exit 3` da recusa permite executar o marcador atrasado. (`bin/trava_fila.sh:115`; `bin/tests/test_trava_dono_do_job.sh:104`, `:107`) | Trocar `3` por outro rc não zero: não confere código exato, evento `recusado` nem ausência de entrada transitória. (`bin/trava_fila.sh:115`; `bin/tests/test_trava_dono_do_job.sh:107`) |
| (d) | Acrescentar TERM ao job em `_solta`: desaparece a conclusão com rc zero. (`bin/trava_teste.sh:135`; `bin/tests/test_trava_dono_do_job.sh:123`, `:127`) | Substituir `setsid -f bash` por `bash`: o caso mata apenas o PID do esperador, não sua árvore; um filho comum também pode sobreviver a esse sinal individual. A garantia contra TaskStop por árvore não é testada. (`bin/trava_teste.sh:127`; `bin/tests/test_trava_dono_do_job.sh:119`) |
| (e) | Retirar o TERM do dono rodando deixa o trabalho de 60 segundos vivo além do prazo observado. (`bin/trava_fila.sh:160`; `bin/tests/test_trava_dono_do_job.sh:135`, `:143`) | Gravar motivo/quem constantes ou incorretos: confere quantidade de eventos e texto `cancelados=2`, sem conferir esses campos. (`bin/trava_fila.sh:161`; `bin/tests/test_trava_dono_do_job.sh:150`, `:152`) |
| (f) | Permitir superação de rodando mata `S2_RODANDO`; retirar a integração com a fila impede a entrada esperada. (`bin/trava_fila.sh:85`; `bin/sombra_trava.sh:40`; `bin/tests/test_trava_dono_do_job.sh:168`, `:175`) | Retirar `exec 8>&-` no caminho de entrada desaparecida: o wrapper do teste imediatamente executa `exit 1`, fechando o descritor de qualquer maneira. Um chamador que tratasse o erro continuando não está coberto. (`bin/sombra_trava.sh:50`; `bin/tests/test_trava_dono_do_job.sh:160`) |
| (g) | Retirar a chamada a `nega_suite_fora_da_trava`. (`bin/disjuntor.py:1569`; `bin/tests/test_trava_dono_do_job.sh:193`) | Mover a guarda para dentro de `if not desligado()` passa os exemplos: `DISJUNTOR=0` não desliga nada; a chave real é arquivo. Excluir agentes também passa: todos os payloads omitem `agent_id`. (`bin/disjuntor.py:144`, `:146`, `:1570`; `bin/tests/test_trava_dono_do_job.sh:185`, `:192`) |
| (i) Docker | Retirar o `docker stop` de `_parar` deixa o container vivo após cancelamento. (`bin/trava_teste.sh:242`; `bin/tests/test_trava_dono_para_filho.sh:75`) | Retirar o destacamento preservando um filho normal pode sobreviver ao TERM enviado só ao PID do esperador; o teste não mata a árvore. (`bin/trava_teste.sh:127`; `bin/tests/test_trava_dono_para_filho.sh:68`) |
| (ii) Docker | Fazer `_quem` ignorar containers rotulados e dizer livre reprova a expectativa `ORFAO`. (`bin/trava_teste.sh:94`; `bin/tests/test_trava_dono_para_filho.sh:97`, `:99`) | Fazer o esperador retornar zero quando falta `.rc`: (ii) executa `wait` mas não confere seu status. (`bin/trava_teste.sh:148`; `bin/tests/test_trava_dono_para_filho.sh:95`) |

A sombra continua com ciclo de vida diferente do prometido para o job: registra o `BASHPID` do chamador, toma fd 8 nesse mesmo processo e exporta herança; não cria esperador destacado, arquivo de rc ou trap para parar container. Cancelar rodando envia TERM apenas ao PID registrado, enquanto os filhos herdam o descritor. (`bin/sombra_trava.sh:38`, `:46`, `:48`, `:63`, `:64`; `bin/trava_fila.sh:160`)

Também não há `tf_sai` no término bem-sucedido da sombra: suas chamadas aparecem apenas em erros de abertura/timeout. A entrada morta será podada na leitura, sem evento `fim`. Pedidos superados ou cancelados antes da aquisição também não chegam à escrita de `sombra_espera.jsonl`. (`bin/sombra_trava.sh:46`, `:54`, `:57`, `:65`; `bin/trava_fila.sh:58`)

Os selos reapontam lock/fila/jobs e sombra para diretórios temporários e conferem que a substituição do lock ocorreu; o hook recebe `HOOK_STOP_RAIZ` próprio. O selo Docker apenas sourceia recursos, sem chamar `teste_envfile`, e cria/remove containers por nomes próprios. (`bin/tests/test_trava_dono_do_job.sh:24`, `:27`, `:29`, `:157`, `:159`, `:186`; `bin/tests/test_trava_dono_para_filho.sh:28`, `:35`, `:38`, `:40`, `:44`, `:45`)

Não cabe afirmar isolamento absoluto: a limpeza de `dono_do_job` mata PID nu vindo dos arquivos, sem conferir starttime, e usa `pkill -f`; PID reciclado pode atingir processo alheio. As consultas Docker são globais, e suites alheias sem rótulo podem interferir nos casos Docker; a trava as espera em vez de matá-las. (`bin/tests/test_trava_dono_do_job.sh:35`, `:38`, `:40`; `bin/trava_teste.sh:68`, `:194`, `:225`, `:230`)

## 31/10 22:24 · sessao 35b88c87e455 · - · - · origem=Ronald (tmux)

Achados do predicado, sem executar suites nem escrever arquivos:

1. **Escapes reproduzidos com o predicado real compilado em memória**:

| Comando de suite | Por que passa | [nao verificado]
|---|---|
| `echo inicio` + newline + `docker run --rm img python manage.py test ponto` | Newline é consumido como whitespace; o segmento começa em `echo`. `bin/disjuntor.py:167`, `bin/disjuntor.py:174`. |
| `if true; then docker run --rm img python manage.py test ponto; fi` | O segmento começa por `then`, que não é prefixo conhecido. `bin/disjuntor.py:157`, `bin/disjuntor.py:197`, `bin/disjuntor.py:216`. |
| `docker --context default run --rm img python manage.py test ponto` | `run` só é procurado nos índices 1 e 2. `bin/disjuntor.py:216`. |
| `sudo -u ronald docker run ...` / `env -C /tmp docker run ...` / `stdbuf -o L docker run ...` | O argumento textual da opção interrompe a remoção dos prefixos. `bin/disjuntor.py:199`. |
| `docker run > out --rm img python manage.py test ponto` | O redirecionamento separa `docker run` dos argumentos da suite. `bin/disjuntor.py:175`. |
| `docker run --rm img python manage.py --settings=config.settings.ci test ponto` | A regex exige `manage.py` imediatamente seguido de whitespace e `test`. `bin/disjuntor.py:160`. |
| `D=docker; "$D" run ...`, `eval 'docker run ...'`, `printf x \| xargs docker run ...` | Não resolve expansão, eval nem executores adicionais. `bin/disjuntor.py:188`, `bin/disjuntor.py:207`, `bin/disjuntor.py:216`. |

Também não inspeciona o conteúdo de scripts chamados por `bash arquivo.sh`; só recursa no texto após opção curta contendo `c`. Portanto a camada estática é indispensável e também tem heurísticas próprias, distintas. `bin/disjuntor.py:208`, `bin/tests/test_trava_teste.sh:83`, `bin/tests/test_trava_teste.sh:87`, `bin/tests/test_trava_teste.sh:120`.

2. **Falsos positivos reproduzidos**:

- `bash bin/trava_teste.sh -- bash -c 'docker run --rm img python manage.py test ponto'`: nega apesar de o shell interno executar sob o flock; encontra o `-c` em qualquer posição após `bash`, sem distinguir execução de script de execução por `bash -c`. `bin/disjuntor.py:208`, `bin/disjuntor.py:210`; a trava executa o argumento genérico em `bin/trava_teste.sh:248`.
- `docker run --rm alpine echo 'python manage.py test ponto'` e `docker run --rm -e NOTE='manage.py test' alpine sleep 30`: nega por procurar a regex em todos os argumentos unidos, sem identificar o programa executado. `bin/disjuntor.py:216`.
- `printf '%s\n' ';' docker run --rm img python manage.py test` e `grep -F ';' docker run --rm img python manage.py test`: nega embora apenas imprimam/leiam texto; após remover as aspas, trata até o token **citado** `;` como operador. `bin/disjuntor.py:167`, `bin/disjuntor.py:175`.
- `cat <<'EOF'` com corpo `; docker run --rm img python manage.py test` também é negado: não trata here-doc como dados. `bin/disjuntor.py:174`.

As formas canônicas atuais, com trava em primeiro plano, não são negadas nas chamadas de `suite.sh:96`, `regua.sh:165`, `pre-push.sh:142`; `grep -n "docker run ... manage.py test" arquivo` simples também passa. Isso não constitui prova para todas as combinações de argumentos adicionais. `bin/disjuntor.py:207`, `bin/disjuntor.py:216`.

3. **Ordem**:

`guarda_chave` continua primeiro; a nova guarda vem antes de `desligado()` e de `pretool`, logo alcança a sessão principal e a chave desligada, mas antecipa e pode impedir a avaliação de B1/B3/B4. A ordem relativa dessas guardas antigas permanece em `pretool`; a nova recusa vence antes delas. `bin/disjuntor.py:1568`, `bin/disjuntor.py:1569`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:1513`, `bin/disjuntor.py:1517`, `bin/disjuntor.py:1522`.

A guarda continua sujeita ao fail-open geral: exceção antes da resposta, inclusive falha em `trilha('NEGOU', ...)`, libera fora do ramo B3. `bin/disjuntor.py:1575`, `bin/disjuntor.py:1579`, `bin/disjuntor.py:1589`.

4. **Selo (g): dois furos concretos**:

- Não testa desligamento: repete os casos com a variável `DISJUNTOR=0`, mas `desligado()` consulta exclusivamente a existência do arquivo `DISJUNTOR_DESLIGADO`; a variável não é lida. `bin/tests/test_trava_dono_do_job.sh:192`, `bin/disjuntor.py:64`, `bin/disjuntor.py:144`.
- Não testa agente: o payload construído nunca leva `agent_id`. `bin/tests/test_trava_dono_do_job.sh:185`.

Mutação que passa despercebida: trocar `bin/disjuntor.py:1569` por `neg = neg or (not desligado() and nega_suite_fora_da_trava(payload))`; os seis casos permanecem com a chave ligada. Outra: aplicar a recusa apenas quando não há `agent_id`. Mutação mínima que deixa vermelho: remover a chamada à nova guarda em `bin/disjuntor.py:1569`, pois os três comandos positivos precisam produzir recusa. `bin/tests/test_trava_dono_do_job.sh:189`, `bin/tests/test_trava_dono_do_job.sh:193`.

5. **Classes / fonte única**:

- A v2 criou uma implementação Python do juiz log+rc que duplica a decisão do shell; não é idêntica: Python retorna `INDEFINIDO` para ausência de OK/FAILED, enquanto shell admite `VAZIO` para `NO TESTS RAN`/`Ran 0 tests`. Ambas aceitam `OK`+rc0 sem exigir uma linha `Ran`, apesar do comentário do shell prometer essa condição. `app/core/esteira_vigia.py:813`, `app/core/esteira_vigia.py:825`, `bin/vigia_veredito.sh:12`, `bin/vigia_veredito.sh:19`, `bin/vigia_veredito.sh:24`. Não achei autoridade registrada de infra para afirmar “duplica juiz **registrado**”; essa qualificação fica [nao verificado].
- Recursos/envfile continuam derivados de `recursos.sh`; `_recursos_teste` sourceia a fonte, chama `teste_envfile` e lê as flags, sem regravar os valores de CPU/senha. `app/core/esteira_vigia.py:770`, `bin/recursos.sh:81`, `bin/recursos.sh:118`.
- A escrita concorrente de envfile usa temporário único e rename no mesmo diretório. `bin/recursos.sh:139`, `bin/recursos.sh:143`.
- Duplicação nova dos relógios/identidades: `_inicio` da trava e `tf_inicio` da biblioteca implementam independentemente a leitura do mesmo campo 22 de `/proc`; a fila chama uma e o job monta identidade com a outra. `bin/trava_teste.sh:49`, `bin/trava_teste.sh:154`, `bin/trava_fila.sh:18`, `bin/sombra_trava.sh:39`. Não é leitura de relógio de negócio; é identidade de processo.

## 31/10 22:09 · sessao 5b0dcb328d14 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**A cura dos três casos novos procede. O aval irrestrito para “qualquer aware” NÃO procede:** ainda há uma entrada aceita que escolhe o passo de 1 minuto e atravessa uma borda. Ela exige construir o aware incorretamente com `tzinfo=TZ_LOCAL`; seu equivalente UTC funciona corretamente. (`app/ponto/motor_calculo_v2.py:73–79`, `:186–194`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:460–480`)

1. **Cursor, progresso e limites**

   Primeiro, o corpo localiza as pontas, recorta os intervalos ao turno, descarta peças vazias e une sobreposições. Cada união começa com `a = _i`. (`app/ponto/motor_calculo_v2.py:162–178`)

   A cada passagem:

   - Lê a parede de `localizar(a)`, remove fuso, minutos, segundos e microssegundos e soma uma hora. Portanto, busca a próxima hora inteira dessa parede, sem arredondar a duração da pausa. (`app/ponto/motor_calculo_v2.py:186`)
   - Aplica `TZ_LOCAL.localize(hora_cheia)` e depois `localizar`, obtendo `proxima`. (`app/ponto/motor_calculo_v2.py:187`)
   - Se `proxima <= a`, substitui por `a + 1 minuto`; caso contrário, preserva o corte, mesmo que faltem apenas segundos. Limita então por `_f`. (`app/ponto/motor_calculo_v2.py:188–190`)
   - Consulta os dois prefixos da mesma `minutos_noturnos`. Se a diferença superar `1e-9`, soma **toda a duração** de `[a,b]`; depois faz `a = b`. (`app/ponto/motor_calculo_v2.py:191–194`)

   Depois de um passo de 1 minuto, o cursor conserva o `tzinfo` da soma. A passagem seguinte chama novamente `localizar(a)`; isso **não garante normalização quando `a.tzinfo` é o próprio objeto `TZ_LOCAL`**, conforme o contraexemplo do item 2. (`app/ponto/motor_calculo_v2.py:79`, `:186–194`)

   Nas sondas em memória dos corpos da cópia, obtive:

   | Entrada examinada | Resultado atual |
   |---|---:|
   | Pausa 04:59:30–05:01, sem prorrogação | 0,5 min |
   | Mesma pausa, com prorrogação | 1,5 min |
   | Pausa 21:59:30–22:01, ambas as flags | 1 min |
   | Naive inexistente de 04/11/2018, 00:00:30–06h, sem prorrogação | 239,5 min |
   | Mesmo caso, com prorrogação | 299,5 min |

   Os três casos sem prorrogação correspondem às fixtures novas; os valores com prorrogação resultam da consulta aos prefixos com a flag ligada. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:460–480`; `app/ponto/motor_calculo_v2.py:110–116`, `:191–196`)

   No caso naive inexistente, `a` começa como `00:00:30-03`, mas sua parede é relida como `01:00:30-02`; o primeiro corte vira `02:00-02`. Assim, desaparece a sequência anterior de passos mínimos até as 05h. (`app/ponto/motor_calculo_v2.py:78–79`, `:186–190`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:472–480`)

   Também reproduzi **354 minutos em 1914**, **270 na entrada do verão de 2018** e **450 na saída**, conforme as bordas declaradas nos testes. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:389–405`; `app/ponto/motor_calculo_v2.py:186–196`)

   Para a hora ambígua `17/02/2018 23:30:30`, com turno 20h–08h e pausa até 06h, as ocorrências `-02` e `-03` produziram, respectivamente, **389,5 e 329,5 minutos sem prorrogação**, e **449,5 e 389,5 com prorrogação**. A escolha da ocorrência pertence à localização da entrada; o corpo depois trabalha pelos instantes recebidos. (`app/ponto/motor_calculo_v2.py:73–79`, `:100–116`, `:162–196`)

   Quanto às propriedades:

   - **`b <= a` e laço sem progresso: NÃO procedem**, para operações concluídas sem exceção. Dentro do laço, `_f > a`; a guarda garante outro candidato posterior; o `min` continua posterior. O cursor avança estritamente numa representação de precisão finita. (`app/ponto/motor_calculo_v2.py:179`, `:188–194`)
   - **Resultado negativo ou superior ao total devolvido por `minutos_noturnos(entrada,saida)`: NÃO procedem.** As parcelas somadas são positivas, o minuendo acumula durações positivas e o retorno aplica o teto. (`app/ponto/motor_calculo_v2.py:112–118`, `:176–196`)
   - **Excesso material sobre a união: NÃO procede.** Cada trecho pertence à união recortada e é percorrido uma vez. **Excesso literal de `float` existe:** a pausa prorrogada de 60 min e 25 s retorna `60.41666666666667`, contra duração direta `60.416666666666664`, diferença aproximada de `7,1e-15` minuto. (`app/ponto/motor_calculo_v2.py:164–194`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:119–125`, `:185`)
   - Acima de 26h, o teto final pode devolver zero; isso preserva “parte ≤ todo”, sem corrigir o comportamento do minuendo, que está fora deste aval. (`app/ponto/motor_calculo_v2.py:93–95`, `:151–161`, `:196`)

2. **O passo de 1 minuto pode ser escolhido e pode cruzar borda**

   **SIM**, nesta entrada sintética aceita pelo corpo, usando exatamente o `TZ_LOCAL` do módulo: (`app/ponto/motor_calculo_v2.py:28`, `:73–79`, `:162–164`, `:186–190`)

   ```python
   entrada = TZ_LOCAL.localize(datetime.datetime(2018, 11, 4, 20))
   saida   = TZ_LOCAL.localize(datetime.datetime(2018, 11, 5, 7))
   ini = datetime.datetime(2018, 11, 4, 20, 53, 30, tzinfo=TZ_LOCAL)
   fim = datetime.datetime(2018, 11, 4, 20, 55,  0, tzinfo=TZ_LOCAL)
   ```

   A atribuição direta de `tzinfo` deixa a pausa com LMT `-03:06`. Seus instantes correspondem a **21:59:30–22:01:00 no offset vigente `-02`**. Como `localizar(a)` recebe o mesmo objeto de fuso, mantém essa parede atrasada; calcula `proxima = 21:00-02`, anterior a `a`, e escolhe o piso. (`app/ponto/motor_calculo_v2.py:79`, `:186–189`)

   | Passo | Instante local vigente | Aumento do prefixo | Duração somada |
   |---|---|---:|---:|
   | Primeiro | 21:59:30 → **22:00:30** | 0,5 min | **1 min** |
   | Segundo | 22:00:30 → 22:01:00 | 0,5 min | 0,5 min |
   | Total | Pausa com apenas 1 min dentro da janela | 1 min | **1,5 min** |

   Reproduzido com **e sem prorrogação**: a borda das 22h fica no interior do primeiro trecho, e o corpo transforma presença parcial em duração integral. (`app/ponto/motor_calculo_v2.py:103`, `:188–194`)

   Há equivalente nas 05h: turno canônico **04/11/2018 22h → 05/11 07h**; pausa construída com `tzinfo=TZ_LOCAL` em **05/11 03:53:30–03:55**, equivalente a **04:59:30–05:01 no offset `-02`**. O piso alcança **05:00:30** e devolve **1 contra 0,5 minuto**, sem prorrogação. Com prorrogação, os 1,5 minuto são devidos. (`app/ponto/motor_calculo_v2.py:101`, `:186–194`)

   Essa construção de `pytz` é inadequada, mas o corpo não a rejeita. **Ela não demonstra falha na forma UTC aware declarada para o banco:** convertendo os mesmos instantes para UTC antes da chamada, o desconto das 05h volta a 0,5. Ocorrência produtiva dessa construção: **[nao verificado]**. (`CLAUDE.md:218`; `app/ponto/motor_calculo_v2.py:79`, `:162–196`)

3. **Diferenças para os corpos anteriores e chamadores**

   Comparação em memória com os anteriores representados pelos mutantes da cópia, todos os casos abaixo sem prorrogação: (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:408–431`, `:483–508`)

   | Caso | e4ed207c representado | 14a61ef3 representado | Atual |
   |---|---:|---:|---:|
   | 04:59:30–05:01 | 0,5 | **1** | 0,5 |
   | 21:59:30–22:01 | 1 | **1,5** | 1 |
   | Naive inexistente, 00:00:30–06h | 239,5 | **240** | 239,5 |
   | Histórico de 1914 | **360** | 354 | 354 |
   | Entrada/saída do verão, fixtures existentes | 270 / 450 | 270 / 450 | 270 / 450 |

   As entradas e expectativas estão nas fixtures; os cortes distintos estão nos dois mutantes e no corpo atual. Portanto, **identidade geral com e4ed207c NÃO procede**, enquanto os dois casos ordinários com segundos recuperam os valores daquele corpo. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:396–405`, `:426–429`, `:460–480`, `:502–506`; `app/ponto/motor_calculo_v2.py:186–193`)

   **“Aware UTC com segundos não muda chamador” NÃO procede.** Usei entrada `15/10/2026 01:00Z`, saída `10:00Z`, pausa `07:59:30Z–08:01Z`, `pr=False`: são os instantes do primeiro teste novo. O desconto muda de **1 para 0,5 minuto** frente a 14a61ef3. (`app/ponto/motor_calculo_v2.py:79`, `:186–196`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:460–464`, `:503–508`)

   | Leitor, na mesma entrada | 14a representado → atual |
   |---|---|
   | `MotorBase._noturno_dos_intervalos` e método emprestado ao contexto | 1 → 0,5 min. (`app/ponto/motor_calculo_v2.py:1915–1916`; `app/ponto/calculador/regras.py:60–63`) |
   | `calcular_noturnas`, fator 1 | Relógio/legais: 419 → **419,5 min**. (`app/ponto/motor_calculo_v2.py:971–980`) |
   | `noturnas_do_dia` e `noturno_no_par`, fator 1 | Relógio/legais: 419 → **419,5 min**. (`app/ponto/calculador/regras.py:80–89`; `app/ponto/calculador/periodo.py:508–511`) |
   | `he_noturna`, HE100 = 538,5 min trabalhados e HE50 = 0 | HE100 noturna: 419 → **419,5 min**. (`app/ponto/motor_calculo_v2.py:392–406`, `:440–451`) |

   A HE por período encaminha essas pontas, intervalos e flag à mesma função e escreve seus retornos divididos por 60. Assim, quando essa ponta recebe os 538,5 minutos em HE100, a rubrica passa de `419/60` para `419,5/60` hora. Execução integral de `do_dia` e impacto produtivo persistido: **[nao verificado]**. (`app/ponto/calculador/regras.py:883–898`)

4. **Expectativa, mordida, `subTest` e relógio**

   **Valor copiado do resultado do motor: NÃO procede.** A expectativa principal é a duração absoluta da interseção da pausa com bordas fornecidas literalmente pelos testes. Compartilha `localizar` para converter as pontas; a chamada a `minutos_noturnos` serve à segunda asserção, de teto, e não calcula `esperado`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:452–480`)

   **Os três casos mordem `_piso_incondicional`: SIM.** Na execução em memória, atual/mutante deram, respectivamente, **0,5/1**, **1/1,5** e **239,5/240**; cada chamada ao teste sob o mutante levantou a `AssertionError` esperada. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454–458`, `:460–508`, `:520–522`)

   **O `subTest` do MORDE não esconde a sobrevivência do mutante.** Os testes internos são chamados diretamente dentro de `assertRaises`. Se não levantarem, a falha de `assertRaises` é registrada pelo `subTest` externo no runner. Confirmei substituindo em memória o mutante pelo corpo curado: o runner `unittest` terminou com **três falhas**, uma por nome. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:515–522`)

   **Relógio solto: NÃO procede.** As datas são literais e `_local` apenas constrói/localiza o `datetime`. **Cobertura universal: NÃO procede**, pois os casos novos não exercitam o aware com o objeto base de fuso do item 2. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377–379`, `:460–480`)

   Executei **21 testes selecionados por AST em memória**, incluindo as classes novas, com sucesso. Isso não equivale à suíte Django canônica, que permanece **[nao verificado]**. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:80–130`, `:244–284`, `:382–522`)

5. **Classes reprovadas e escritores/leitores**

   **Segundo juiz, segundo escritor fora da porta e derivação paralela da janela: NÃO procedem neste delta.** A classificação continua vindo dos prefixos da mesma função; as novas atribuições são locais. **Selo sem mordida e relógio solto também NÃO procedem nos testes novos.** O achado residual é a insuficiência da normalização para uma forma aware aceita, não uma dessas duplicações. (`app/ponto/motor_calculo_v2.py:186–196`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:452–522`; `app/docs/JUIZES.md:29`)

   | Valor | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | Fuso | Configuração e inicialização de `TZ_LOCAL`. (`app/config/settings/base.py:93`; `app/ponto/motor_calculo_v2.py:28`) | Conversão, localização do corte e fixtures. (`app/ponto/motor_calculo_v2.py:78–79`, `:187`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:379`) |
   | Pontas e intervalos | Argumentos dos chamadores; carimbo dos efetivos. (`app/ponto/motor_calculo_v2.py:442–443`, `:1828–1830`, `:1915`) | Recorte/união; releitura por período. (`app/ponto/motor_calculo_v2.py:162–172`; `app/ponto/calculador/periodo.py:752`) |
   | Prorrogação | Parâmetro/atributo do motor, contexto e CCT. (`app/ponto/motor_calculo_v2.py:758–762`; `app/ponto/calculador/regras.py:60–61`; `app/ponto/calculador/nucleo.py:348`) | Prefixos/teto e delegações. (`app/ponto/motor_calculo_v2.py:191–196`, `:1916`; `app/ponto/calculador/periodo.py:509`) |
   | `_ent`, `_sai`, `pecas`, `unidas`, `_i`, `_f` | Localização, recorte, ordenação e união. (`app/ponto/motor_calculo_v2.py:162–178`) | Guarda, limite, prefixos e teto. (`app/ponto/motor_calculo_v2.py:179–196`) |
   | `a` | Início da união e avanço `a = b`. (`app/ponto/motor_calculo_v2.py:178`, `:194`) | Guarda, parede, comparação, piso, prefixo e duração. (`app/ponto/motor_calculo_v2.py:179–193`) |
   | `hora_cheia`, `proxima`, `b` | Construção da parede, localização, eventual piso e `min`. (`app/ponto/motor_calculo_v2.py:186–190`) | Localização seguinte, guarda, prefixo, soma e avanço. (`app/ponto/motor_calculo_v2.py:187–194`) |
   | Prefixos e `total` | `minutos_noturnos`; inicialização e acumulação local. (`app/ponto/motor_calculo_v2.py:96–118`, `:176`, `:193`) | Teste de presença e retorno limitado. (`app/ponto/motor_calculo_v2.py:191–196`) |
   | Desconto retornado | Retorno desta função. (`app/ponto/motor_calculo_v2.py:196`) | Subtrações nos leitores do motor e calculador. (`app/ponto/motor_calculo_v2.py:442–443`, `:973`, `:1915`; `app/ponto/calculador/regras.py:86`) |
   | Resultados noturnos | Escritores existentes dos períodos e rubricas. (`app/ponto/motor_calculo_v2.py:991–993`; `app/ponto/calculador/regras.py:896–898`, `:912–914`; `app/ponto/calculador/periodo.py:753–754`) | Consumo posterior ou alteração persistida por esta obra: **[nao verificado]**. |
   | Fixtures, `desc`, bordas, `esperado` | Literais/helper, chamada real e interseção declarada. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:377–379`, `:452–480`) | Asserções de igualdade e teto. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:457–458`) |
   | Mutante, `nome`, `t` e símbolo substituído | Definição do mutante, lista de métodos, instanciação e `mock.patch`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:483–520`) | Import/chamada em `_caso`, `getattr` e `assertRaises`. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:453–454`, `:521–522`) |

   **Ordem de trava, execução histórica do RED, pré-requisitos e pouso efetivo: [nao verificado].** [nao verificado]

## 31/10 22:08 · sessao aa7f9ac23376 · - · - · origem=Ronald (tmux)

Achado: **a alegação “o passo de 1 minuto nunca cruza borda” NÃO procede para todo `datetime` aware aceito pelo corpo**. `datetime(..., tzinfo=TZ_LOCAL)` usa diretamente o objeto base `pytz`, com LMT `-03:06`; `astimezone(TZ_LOCAL)` pode devolver o próprio objeto, de modo que `localizar(a)` não normaliza essa entrada. O corpo aceita essa forma sem validação. (`app/ponto/motor_calculo_v2.py:73–79`, `:162–164`, `:186–189`)

Contraexemplo reproduzido em memória:

```python
ent = TZ_LOCAL.localize(datetime.datetime(2018, 11, 4, 20))
sai = TZ_LOCAL.localize(datetime.datetime(2018, 11, 5, 7))
ini = datetime.datetime(2018, 11, 4, 20, 53, 30, tzinfo=TZ_LOCAL)
fim = datetime.datetime(2018, 11, 4, 20, 55, tzinfo=TZ_LOCAL)
```

A pausa representa **21:59:30→22:01:00 `-02:00`**, duração 1,5 minuto. O candidato calculado é **21:00 `-02:00`**, anterior a `ini`; o piso leva o cursor a **22:00:30 `-02:00`**, atravessando 22h. O prefixo aumenta 0,5 minuto nesse primeiro passo, mas o corpo soma 1 minuto; acrescenta mais 0,5 no restante. **Resultado 1,5 contra 1 minuto**, tanto com `pr=False` quanto com `pr=True`. (`app/ponto/motor_calculo_v2.py:103`, `:186–194`)

Também reproduzi borda das 05h: entrada canônica **04/11/2018 22h**, saída **05/11/2018 07h**, pausa construída com `tzinfo=TZ_LOCAL` **05/11 03:53:30→03:55**, equivalente a **04:59:30→05:01 `-02:00`**. Candidato **04:00 `-02:00`**, piso até **05:00:30**; **desconto 1 contra 0,5 minuto sem prorrogação**. Com prorrogação, retorna corretamente 1,5 minuto. (`app/ponto/motor_calculo_v2.py:101`, `:186–194`)

Depois do piso, `a = b` mantém o mesmo `tzinfo`; a próxima passagem chama novamente `localizar(a)`, que continua sem normalizar quando os objetos de fuso são idênticos. Portanto, o piso pode ser escolhido repetidamente. (`app/ponto/motor_calculo_v2.py:79`, `:186–194`)

Outra divergência dessa forma aware: entrada/saída canônicas **14/10/2026 22h→15/10 07h**, pausa base-LMT **14/10 23:55→23:56**, equivalente a **15/10 00:01→00:02 `-03:00`**. Ambos os prefixos retornam 120 e o desconto é **0 contra 1 minuto**, com ambas as flags. Aqui a origem é a combinação já existente de `localizar` com `fim.date()` no minuendo, que omite o segmento do dia seguinte quando o fim conserva a data de parede anterior. (`app/ponto/motor_calculo_v2.py:79`, `:97–104`, `:191–196`)

Esses exemplos usam construção inadequada de `pytz`, **fora da forma UTC aware declarada para o banco**. O equivalente UTC canônico do contraexemplo das 05h retorna 0,5 no corpo atual; não demonstram defeito nessa forma produtiva. (`CLAUDE.md:218`; `app/ponto/motor_calculo_v2.py:79`, `:186–196`)

**`b <= a`, laço sem progresso, negativo e excesso material sobre união/todo NÃO procedem para operações válidas e intervalos finitos**: a guarda assegura candidato posterior, o fim também é posterior dentro do laço, cada trecho é percorrido uma vez, só se somam durações positivas e existe teto final. Exceções por limites de `datetime` não são laço infinito. (`app/ponto/motor_calculo_v2.py:164–196`)

Sonda adicional em memória: **44.160 combinações** em torno das transições disponíveis, com entradas UTC aware ou naive, ambas as flags, comparadas à união exata dos segmentos existentes de `minutos_noturnos`: nenhuma divergência encontrada. Isso é amostragem, não prova universal; banco, logs e suíte Django permanecem **[nao verificado]**. As fontes executadas foram os corpos da cópia. (`app/ponto/motor_calculo_v2.py:73–118`, `:162–196`)

## 31/10 22:06 · sessao 06d2750e0795 · - · - · origem=Ronald (tmux)

Há mudança sintética em todos os caminhos citados com pontas e pausa **aware UTC com segundos**. Sonda em memória com os corpos reais extraídos por AST: entrada `2026-10-15 01:00Z`, saída `10:00Z`, pausa `07:59:30Z–08:01Z`, `prorrogacao=False`; equivalem a `22:00–07:00`, pausa `04:59:30–05:01` local. A conversão é a de `app/ponto/motor_calculo_v2.py:73-79`; a fixture corresponde a `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:460-464`.

| Caminho | Mutante de 14a61ef3 | Corpo atual | Evidência |
|---|---:|---:|---|
| Desconto | 1 | 0,5 | `app/ponto/motor_calculo_v2.py:186-196`; mutante `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:483-508` |
| `MotorBase._noturno_dos_intervalos` | 1 | 0,5 | `app/ponto/motor_calculo_v2.py:1915-1916` |
| `_ContextoNoturno._noturno_dos_intervalos` | 1 | 0,5 | `app/ponto/calculador/regras.py:60-63` |
| `calcular_noturnas`, fator 1 | `(419,419)` | `(419,5;419,5)` | `app/ponto/motor_calculo_v2.py:971-980` |
| `noturnas_do_dia`, fator 1 | `(419,419)` | `(419,5;419,5)` | `app/ponto/calculador/regras.py:80-89` |
| `noturno_no_par`, fator 1 | relógio/legais 419 | relógio/legais 419,5 | `app/ponto/calculador/periodo.py:508-511` |
| `he_noturna`, HE100=538,5 min e HE50=0 | `(0,419)` | `(0,419,5)` | `app/ponto/motor_calculo_v2.py:440-451` |

O caso HE usa todos os 538,5 minutos trabalhados como extra: `_inicio_do_trecho_extra` recua até a entrada, portanto consulta a mesma janela inteira. O recuo pelo trabalho está em `app/ponto/motor_calculo_v2.py:392-406`; a subtração do desconto está em `:441-443`.

O caminho de **HE por período** importa `he_noturna` em `app/ponto/calculador/regras.py:868`, fornece pontas/intervalos/flag em `:886-888`, soma os retornos em `:889-890` e escreve horas nas rubricas em `:896-898`. Com uma ponta que recebe a jornada inteira em HE100, o mesmo delta chega à rubrica: `419/60 = 6,983333333333333 h` contra `419,5/60 = 6,991666666666666 h`; essa propagação decorre das divisões explícitas em `:896-898`. Execução integral de `do_dia`: **[nao verificado]**.

Escritores e leitores externos relevantes:

| Valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| Flag do motor | `MotorBase.__init__`, recebe parâmetro ou consulta regra legal e atribui `self.prorrogacao_noturna_pos5h`: `app/ponto/motor_calculo_v2.py:758-762` | `calcular_noturnas` `:971-973`, delegador `:1915-1916`, casca de HE `:1002-1006` |
| Flag no contexto | `_ContextoNoturno.__init__`: `app/ponto/calculador/regras.py:60-61` | Método emprestado em `:63`, cujo corpo lê o atributo em `app/ponto/motor_calculo_v2.py:1916` |
| Flag na CCT | `cct_do_motor` constrói `CCT(prorrogacao_pos5h=bool(motor.prorrogacao_noturna_pos5h))`: `app/ponto/calculador/nucleo.py:344-351` | `noturno_no_par`: `app/ponto/calculador/periodo.py:508-510` |
| Pontas efetivas/intervalos carimbados | `_carimbar_segmento`: `app/ponto/motor_calculo_v2.py:1828-1830` | Releitura passa `e`, `s_` e `intervalos_do_motor` para `noturno_no_par`: `app/ponto/calculador/periodo.py:752` |
| Retorno de noturnas do motor | `calcular_noturnas`: `app/ponto/motor_calculo_v2.py:971-975` | `_escrever_noturnas`, que grava `periodo.minutos_noturnos_relogio/legais`: `:991-993` |
| Retorno de HE do motor | `he_noturna`: `app/ponto/motor_calculo_v2.py:448-451` | `_aplicar_he_noturna`, que grava campos do período: `:1002-1010` |
| Retorno de noturnas do calculador | `noturnas_do_dia`: `app/ponto/calculador/regras.py:85-89` | Rubrica/detalhes do dia: `:908-914`; encapsulamento em `NoturnoV3`: `app/ponto/calculador/periodo.py:508-511` |
| `NoturnoV3.relogio_min/legais_min` | Construtor em `app/ponto/calculador/periodo.py:511` | Releitura escreve os campos do período em `:752-754` |
| HE por período | Acumuladores `_s50/_s100`: `app/ponto/calculador/regras.py:877`, `:889-890` | Rubricas `horas_extras_50_noturna/100_noturna`: `:896-898` |

**“Aware UTC com segundos não muda nenhum chamador” NÃO procede**: a sonda acima demonstra mudança numérica nos leitores reais. Impacto em colaborador, rubrica persistida ou dados produtivos: **[nao verificado]**.

A sonda usou `python3 -B` e AST em memória; Django não está instalado neste ambiente. Não houve acesso a banco/log, import de modelos ou gravação de arquivo. Execução Django canônica e integração completa: **[nao verificado]**.

## 31/10 22:03 · sessao 63fba4a5c37a · - · - · origem=Ronald (tmux)

Inventário, na cópia atual:

| Arquivo | Escritor → leitor | Excluído |
|---|---|---|
| `logs/sessao/disjuntor.log` | `trilha`, inclusive `_anuncia` de B3/PDC/rearme → `disjuntor.ler`; `bin/disjuntor.py:135`, `bin/disjuntor.py:975`, `bin/disjuntor.py:1051`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:2168` | Sim; `bin/hook_stop_fila1.py:585`. |
| `logs/sessao/disjuntor_estado.json` | `_com_estado` → `_com_estado`, `ler_estado`; `bin/disjuntor.py:267`, `bin/disjuntor.py:278`, `bin/disjuntor.py:299` | Sim; `bin/hook_stop_fila1.py:585`. |
| Seu `.lock` | `_com_estado`: cria/usa `flock`; `bin/disjuntor.py:256`, `bin/disjuntor.py:260` | Sim; `bin/hook_stop_fila1.py:586`. |
| `logs/padrao_de_cima_escopo.json` | `_anuncia` PDC → `_pdc_grava_escopo`; leitor `_pdc_escopo_lido`/`estado_pdc`; `bin/disjuntor.py:1462`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:1526`, `bin/disjuntor.py:1546` | Sim, pelo dono; `bin/hook_stop_fila1.py:593`. |
| `logs/sessao/padrao_de_cima_geracao` | `decide_pdc`/`_pdc_geracao_grava` → `_pdc_geracao_lida`/`estado_pdc`; `bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1497`, `bin/disjuntor.py:1548` | Sim, pelo dono; `bin/hook_stop_fila1.py:593`. |
| `logs/sessao/cota.json` | Status line → `ler_cota`; `bin/statusline_cota.py:49`, `bin/statusline_cota.py:54`, `bin/disjuntor.py:1653` | Sim; `bin/hook_stop_fila1.py:586`. |
| `logs/sessao/pisos.log` | Hook chama `OC.registrar_piso` → `ultimo_piso`, `gatilho_tokens`, `disjuntor.ler`; `bin/hook_stop_fila1.py:911`, `bin/orcamento_contexto.py:198`, `bin/orcamento_contexto.py:210`, `bin/orcamento_contexto.py:227`, `bin/disjuntor.py:2230` | Sim; `bin/hook_stop_fila1.py:585`. |
| `logs/hook_stop_fila1.log` | `_trilha` → `b2_decide`; `bin/hook_stop_fila1.py:48`, `bin/disjuntor.py:1754`, `bin/disjuntor.py:1761` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/hook_stop_fila1.json` | `_zera_teto`, contador principal → contador principal; `bin/hook_stop_fila1.py:1079`, `bin/hook_stop_fila1.py:1267`, `bin/hook_stop_fila1.py:1288` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/hook_stop_espera.json` e `.lock` | `_prova_arquivo`: memória/trava → a própria função; `bin/hook_stop_fila1.py:615`, `bin/hook_stop_fila1.py:625`, `bin/hook_stop_fila1.py:637` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/sessao/orcamento_estado.json` e `.lock` | Hook: `_orc_salva`, `_orcamento` → `_orcamento_travado`, `OC.ler`, `disjuntor.ler`; `bin/hook_stop_fila1.py:764`, `bin/hook_stop_fila1.py:832`, `bin/hook_stop_fila1.py:856`, `bin/orcamento_contexto.py:357`, `bin/disjuntor.py:2210` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/sessao/orcamento.log` | `_orc_trilha` → `OC.ler`, `disjuntor.ler`; `bin/hook_stop_fila1.py:738`, `bin/orcamento_contexto.py:364`, `bin/disjuntor.py:2169` | Sim; `bin/hook_stop_fila1.py:596`. |
| `app/docs/RELATO.md` | `relato_negacao` delega a `esteira_vigia.no_relato` → hook e decisão PDC; `bin/disjuntor.py:234`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:884`, `bin/hook_stop_fila1.py:429`, `bin/disjuntor.py:1376` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/codex/triagem.log` | `--triagem`, via `_triagem_sob_trava` → `_triagens`; `bin/disjuntor.py:837`, `bin/disjuntor.py:839`, `bin/disjuntor.py:861`, `bin/disjuntor.py:2326` | **Não**; lista completa em `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:593`, `bin/hook_stop_fila1.py:596`. |
| `<resposta>.lock` | `triagem` cria e toma `LOCK_SH`; wrapper também usa a trava; `bin/disjuntor.py:806`, `bin/disjuntor.py:813`, `bin/codex_pergunta.sh:157` | **Não**; `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:596`. |

`BACKLOG` também é excluído, embora não haja escrita nele nesses três componentes nos sítios encontrados; sua leitura e exclusão estão em `bin/hook_stop_fila1.py:173`, `bin/hook_stop_fila1.py:596`; ausência absoluta de outro escritor indireto: [nao verificado].

Os temporários também **não estão na lista**: `.disjuntor_estado.*`, `.padrao_de_cima_escopo.*`, `.cota.*`, `.hook_stop_espera.*`, `.orcamento_estado.*`, `.RELATO.*.novo`; criação/substituição em `bin/disjuntor.py:278`, `bin/disjuntor.py:1518`, `bin/statusline_cota.py:49`, `bin/hook_stop_fila1.py:633`, `bin/hook_stop_fila1.py:764`, `app/core/esteira_vigia.py:879`; lista em `bin/hook_stop_fila1.py:596`.

Para temporários, a omissão não demonstra automaticamente uma liberação: cada escrita usa nome/inode novo e depois `os.replace`; a memória exige a mesma chave caminho+dispositivo+inode em duas observações, com mtime crescente; `bin/disjuntor.py:278`, `bin/disjuntor.py:281`, `bin/hook_stop_fila1.py:609`, `bin/hook_stop_fila1.py:629`, `bin/hook_stop_fila1.py:649`. Uma corrida com duas observações durante a escrita não está proibida pela lista; exploração concreta: [nao verificado].

`triagem.log` é o residual estável mais claro: é append no mesmo arquivo e não está excluído, portanto o avanço entre observações satisfaz `_prova_arquivo`; `bin/disjuntor.py:839`, `bin/hook_stop_fila1.py:609`, `bin/hook_stop_fila1.py:649`. Sua escrita requer a operação explícita de triagem e sidecar válido; não há evidência de que seja produzida pelo simples Stop/`--padrao-parada`; `bin/disjuntor.py:822`, `bin/disjuntor.py:827`, `bin/disjuntor.py:2326`. **Fila**, por inventário incompleto; chamar isso de avanço autônomo sem produtor seria [nao verificado].

A trava de `no_relato` é no diretório `app/docs`, sem arquivo `.lock` próprio; diretórios não servem de prova; `app/core/esteira_vigia.py:844`, `app/core/esteira_vigia.py:848`, `bin/hook_stop_fila1.py:581`.

Fragilidades adicionais:

- **Fila:** `_pdc_grava_escopo` e status line podem abandonar temporários quando há exceção após `mkstemp`; seus `except` apenas registram/engolem, sem `unlink`; `bin/disjuntor.py:1518`, `bin/disjuntor.py:1522`, `bin/statusline_cota.py:49`, `bin/statusline_cota.py:55`. Hook, orçamento, estado DJ e `no_relato` possuem limpeza explícita; `bin/hook_stop_fila1.py:638`, `bin/hook_stop_fila1.py:769`, `bin/disjuntor.py:284`, `app/core/esteira_vigia.py:885`.
- **Fila:** a frase do caso 8b sobre o escopo liberar porque o mtime anda omite que o escopo é substituído atomicamente e a chave inclui inode; regravação normalmente cria primeira observação de nova chave, enquanto a geração é reescrita no lugar; `bin/tests/test_hook_esperando_com_prova.sh:124`, `bin/disjuntor.py:1518`, `bin/disjuntor.py:1521`, `bin/hook_stop_fila1.py:609`, `bin/disjuntor.py:1487`. O teste pode morder a ausência de exclusão pelo **motivo esperado**, sem reproduzir um `ALLOW` no escopo; `bin/tests/test_hook_esperando_com_prova.sh:135`, `bin/tests/test_hook_esperando_com_prova.sh:87`.
- A escrita da geração ocorre antes do estado, e o escopo é escrito no anúncio sob a mesma trava; a alteração de comentários não muda essa ordem; `bin/disjuntor.py:1454`, `bin/disjuntor.py:1462`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:275`, `bin/disjuntor.py:292`.

## 31/10 22:03 · sessao 9156207494da · - · - · origem=Ronald (tmux)

- O import é tardio e ocorre só depois de o alvo existir e ser regular; `ImportError`, `SyntaxError`, falha de acesso e ausência das constantes entram em `except Exception` e devolvem motivo não vazio, invalidando a prova por arquivo — inclusive quando acompanhada de PID válido. PID sozinho não executa esse trecho. (`bin/hook_stop_fila1.py:576`, `bin/hook_stop_fila1.py:590`, `bin/hook_stop_fila1.py:685`)

- **Fila — memória congelada em falha transitória.** O retorno de erro antecede aquisição da trava, leitura e gravação de `ESPERA`; conserva a observação anterior, contrariando o “sempre” da docstring. Ao recuperar, um avanço ocorrido durante a indisponibilidade pode liberar na primeira leitura recuperada, sem movimento posterior à recuperação. A decisão ainda mede apenas avanço desde a última observação persistida. (`bin/hook_stop_fila1.py:564`, `bin/hook_stop_fila1.py:594`, `bin/hook_stop_fila1.py:615`, `bin/hook_stop_fila1.py:629`, `bin/hook_stop_fila1.py:649`)

- `HOOK_STOP_RAIZ` troca os caminhos de dados; o código importado é procurado no diretório do `__file__` do hook. As duas constantes relativas são combinadas com `RAIZ` do hook, portanto não é preciso existir `disjuntor.py` na árvore de dados alternativa. (`bin/hook_stop_fila1.py:26`, `bin/hook_stop_fila1.py:33`, `bin/hook_stop_fila1.py:591`, `bin/hook_stop_fila1.py:593`, `bin/disjuntor.py:1101`, `bin/disjuntor.py:1104`)

- No fluxo normal de `main`, o disjuntor já foi importado pela passagem B4 antes da prova de espera. O import novo normalmente reutiliza o módulo carregado; `sys.path.insert` não garante recarregar outra versão já presente em `sys.modules` [nao verificado quanto à ocorrência neste host]. (`bin/hook_stop_fila1.py:1046`, `bin/hook_stop_fila1.py:1215`, `bin/hook_stop_fila1.py:1240`)

- O import novo não executa `decide_pdc`: só acessa constantes. Assim, por si, não regrava geração, escopo nem trilha; a transição que os grava continua em `decide_pdc` → `_com_estado` → `_anuncia`. (`bin/hook_stop_fila1.py:592`, `bin/hook_stop_fila1.py:593`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:1466`)

- Os leitores externos do hook não ganham essa dependência em tempo de importação: handoff pede funções de fila; interrompidas importa três funções de fila; backlog_estado importa o módulo; o disjuntor pede o recorte do topo e `H.RELATO`. O import novo só está no corpo de `_prova_arquivo`. (`bin/handoff_sessao.sh:49`, `bin/handoff_sessao.sh:64`, `bin/handoff_sessao.sh:260`, `bin/handoff_sessao.sh:282`, `bin/interrompidas_estado.py:53`, `bin/backlog_estado.py:20`, `bin/disjuntor.py:689`, `bin/disjuntor.py:703`, `bin/hook_stop_fila1.py:590`)

- Os dois consumidores internos da prova de espera — orçamento e `main` — compartilham `_ESPERA_MEMO`; uma falha fica memorizada durante esse processo e não é reavaliada no segundo consumidor. (`bin/hook_stop_fila1.py:704`, `bin/hook_stop_fila1.py:997`, `bin/hook_stop_fila1.py:1240`)

- **Fila — captura não universal.** `SystemExit`, `KeyboardInterrupt` e encerramento por sinal não são contidos pelos `except Exception` do trecho, de `_esperando_com_prova` nem de `main`; eventual falha dessa natureza não produz necessariamente decisão JSON. Não há `SystemExit` no carregamento normal de `disjuntor.py`; seu `sys.exit(main(...))` está protegido por `__name__ == '__main__'`. (`bin/hook_stop_fila1.py:594`, `bin/hook_stop_fila1.py:690`, `bin/hook_stop_fila1.py:1314`, `bin/disjuntor.py:2342`)

- **Fila — efeitos ambientais do import.** Escrita automática de bytecode, cache existente e configuração `PYTHONDONTWRITEBYTECODE` no host são [nao verificado]. O comando Stop da cópia usa `python3` sem `-B`, e a lista de exclusões não contempla `__pycache__`; não afirmar que houve arquivo `.pyc` escrito sem evidência de ambiente. (`.claude/settings.json:9`, `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:596`)

- Quanto às classes 1–4 e 6, o trecho novo consulta as duas autoridades de caminho, não cria novo cálculo de decisão, escritor explícito, leitura de relógio nem aquisição de trava; a aquisição da trava de mtime continua depois da importação. Classe 5 depende da auditoria do teste; classe 7/pouso real é [nao verificado]. (`bin/hook_stop_fila1.py:590`, `bin/hook_stop_fila1.py:593`, `bin/hook_stop_fila1.py:615`)

## 31/10 22:03 · sessao 71cefccbab80 · - · - · origem=Ronald (tmux)

Auditoria estática; resultados de execução dos selos: [nao verificado].

- **8b morde a exclusão dos dois destinos, por motivo específico.** `_espera_bloqueia` exige simultaneamente `block`, `siga: ITEM-VIVO` e `NOPROVA.*arquivo do proprio hook`; bloquear por primeira observação não basta (`bin/tests/test_hook_esperando_com_prova.sh:80-89`, `:128-134`). Contra o ancestral sem as duas exclusões, os dois casos ficam vermelhos por esse critério; excluindo apenas um destino, o caso do outro continua vermelho (`bin/hook_stop_fila1.py:593-608`; anexo do diff).

- **FILA — a narrativa do caso do escopo exagera o que ele demonstra.** O escopo é escrito por `mkstemp` e `os.replace`; a memória identifica o alvo por caminho, dispositivo e inode. Na sequência normal com inode novo, regravar o escopo reinicia a observação daquele alvo e produz `arquivo nao andou`, em vez de liberar o Stop (`bin/disjuntor.py:1518-1521`; `bin/hook_stop_fila1.py:609-610`, `:629-650`). A geração, ao contrário, é truncada e escrita no mesmo arquivo, portanto oferece o avanço de mtime com identidade estável necessário ao bypass anterior (`bin/disjuntor.py:1478-1491`; `bin/hook_stop_fila1.py:609-650`). Reciclagem efetiva de inode nesta cópia: [nao verificado].

- **FILA — 8b não prova dependência das constantes do dono.** Uma cura com os dois caminhos literais atuais passa os mesmos casos: o teste consulta as constantes, mas não as muda e não verifica de onde a implementação obteve os caminhos (`bin/tests/test_hook_esperando_com_prova.sh:128-134`; `bin/disjuntor.py:1101-1104`; `bin/hook_stop_fila1.py:593`). Seria necessário deslocar uma constante na cópia de teste e testar o novo destino para distinguir essas implementações (`bin/tests/test_hook_esperando_com_prova.sh:128-134`).

- **FILA — uma exclusão ampla por diretório também passa 8b e não é confrontada pelo positivo atual.** Os dois novos destinos ficam em `logs`; o arquivo com escritor que deve liberar fica fora da árvore, em `$_tmp/agente.out`. Não há positivo de produtor legítimo dentro de `logs` nesses casos (`bin/tests/test_hook_esperando_com_prova.sh:38`, `:107-114`, `:128-134`; `bin/disjuntor.py:1101-1104`). A cura por diretório precisa preservar o teste de regularidade anterior para manter o motivo esperado nos casos `arquivo=logs` (`bin/hook_stop_fila1.py:578-582`; `bin/tests/test_hook_esperando_com_prova.sh:122-123`).

- **FILA — 8b não confirma que a regravação ocorreu.** `_pdc` descarta stdout/stderr, seus dois retornos não são conferidos e o único controle posterior é `-f`; não se compara mtime, inode ou conteúdo antes/depois. Com a exclusão presente, o caso pode ficar verde mesmo se a segunda chamada falhar ou parar de regravar (`bin/tests/test_hook_esperando_com_prova.sh:127-134`; `bin/hook_stop_fila1.py:598-608`). A mordida preservada é da exclusão, não da reprodução material do movimento alegado (`bin/tests/test_hook_esperando_com_prova.sh:87`, `:134`).

- **Os novos returns 7/8 corrigem duas ausências reais de observação do caso `desligado`.** O return 7 rejeita saída vazia acompanhada de status diferente de zero; o return 8 exige exatamente uma nova ocorrência da mensagem da chave por chamada. Ambos rodam para o payload cru construtor e o verificador (`bin/tests/test_padrao_de_cima.sh:328-334`). O caminho real captura o JSON não objeto, reconhece a chave, chama `trilha('FALHA', ... DISJUNTOR_DESLIGADO...)` e retorna zero (`bin/disjuntor.py:2078-2081`, `:2094-2101`).

- **Contra b3c0f8ec, os novos returns 7/8 devem passar por inspeção:** o delta de `disjuntor.py` fornecido só troca comentários; a implementação da chave que eles observam permanece a mesma (`bin/disjuntor.py:2099-2101`; anexo do diff). Execução contra esse ancestral: [nao verificado].

- **FILA — não há mutante dedicado aos novos returns 7/8.** O mutante `v9-cru-ignora-chave` remove a guarda da chave, levando à negativa e ao return 6 antes das novas verificações; os outros mutantes de `desligado` removem as portas do wrapper. Portanto os novos asserts são úteis, mas sua mordida específica contra crash mudo ou ausência da trilha não está demonstrada pelo catálogo atual (`bin/tests/test_padrao_de_cima.sh:325-334`, `:711-718`, `:816`). O harness sabe reconhecer `return 7` e `return 8` literais como asserts, caso um mutante dedicado os alcance (`bin/tests/_mordida.sh:139-145`, `:214-225`).

- **FILA menor — return 8 não confere literalmente o evento `FALHA`.** Ele conta apenas a substring `DISJUNTOR_DESLIGADO, ferramenta liberada`, embora o comentário diga “1 linha FALHA”; uma troca do rótulo do evento mantendo a mensagem passaria esse assert (`bin/tests/test_padrao_de_cima.sh:329-334`; `bin/disjuntor.py:2100`).

## 31/10 21:58 · sessao 590560d188d5 · TRAVA-COBERTURA · leitura · origem=Code

**O contrato muda a identidade do dono do trabalho e exige migração dos chamadores, dos recursos e dos selos.** O selo atual exige que matar a chamada mate o container; a proposta exige que ele sobreviva. A sombra, por sua vez, entrega a posse ao chamador por fd 8, sem receber um comando para destacar. (`bin/tests/test_trava_dono_para_filho.sh:54–63`; `bin/sombra_trava.sh:21–39`; `contrato.txt:2`.)

Uso `contrato.txt:1–5` para as cinco cláusulas numeradas do anexo. Os contraexemplos abaixo são sequências condicionais de revisão, não ocorrências medidas em produção; ocorrência real [nao verificado].

**1. O que cada linha encontra hoje e o que muda**

| Linha | Código atual da cópia | Mudança contratada e lacunas |
|---|---|---|
| **1 — quem chama** | A trava define `QUEM` como `ESTEIRA_QUEM` ou basename, acrescentando `$$`; aceita somente `--espera`, `--quem` e `--`. A suite faz `exec` para a trava. (`bin/trava_teste.sh:36`; `bin/trava_teste.sh:85–93`; `bin/suite.sh:96`.) | Introduz obra, volta, fallback de identidade e três verbos. A semântica de `--abre-volta` além da assinatura — persistência, monotonicidade e rejeição de pedidos atrasados — não está especificada. (`contrato.txt:1–2`; detalhes [nao verificado].) |
| **1 — PreToolUse** | `pretool` **já extrai** o comando Bash, mas retorna antes de qualquer fiscalização posterior quando não há `agent_id`, não é `Agent` e não contém `codex_pergunta`. Depois disso não há bloqueio Docker/suite. (`bin/disjuntor.py:1430–1475`.) | A nova fiscalização precisa anteceder esse retorno para alcançar a sessão principal. O matcher já é vazio; ampliar apenas a configuração não resolve. (`bin/disjuntor.py:1431–1432`; `.claude/settings.json:62–68`; `contrato.txt:1,3`.) |
| **2 — quem espera** | A própria trava segura fd 9, espera `flock`, lança o comando em background sem esse fd e faz `wait`. TERM/INT/HUP acionam `_parar`, que procura container pelo rótulo, solicita parada e sai 143. (`bin/trava_teste.sh:96–103`; `bin/trava_teste.sh:159–171`.) | Separa esperador e job, acrescenta registros, arquivo de rc e superação de jobs ainda em fila. Para sombra, destacar apenas `trancar_sombra` não transfere a continuação: a função atualmente retorna para o restante do script executar. (`contrato.txt:2`; `bin/sombra_trava.sh:21–39`; `bin/sombra.sh:438–461`.) |
| **3 — falha/limite** | A trava já retorna 75 por timeout, falha de `docker ps`, dono vivo sem posse, falha ao parar órfão e suite sem rótulo persistente. Já compara PID **e starttime** para validar o dono do container. Não usa pidfile nem arquivo de rc. A sombra retorna **1** quando sua espera falha. (`bin/trava_teste.sh:39–56`; `bin/trava_teste.sh:97–99`; `bin/trava_teste.sh:115–149`; `bin/sombra_trava.sh:28–35`.) | Acrescenta validação/poda dos registros, timeout de nascimento e rc persistido. “Sem pidfile = job não nasceu” não é demonstrado por um timeout; o protocolo de nascimento não foi fornecido. Também falta explicitar se a sombra migra seu timeout de 1 para 75. (`contrato.txt:3`; implementação [nao verificado].) |
| **4 — medida** | A trava de testes escreve `.quem`; a sombra escreve `sombra_espera.jsonl`, com leitor próprio de esperas longas. (`bin/trava_teste.sh:102–103`; `bin/sombra_trava.sh:30–31`; `bin/sombra_trava.sh:42–60`.) | Introduz journal de eventos, listagem e mensagem de RELATO no stderr. Os valores esperados “0” são metas do contrato, sem medição apresentada. Destino da mensagem quando o esperador já morreu [nao verificado]. (`contrato.txt:4`.) |
| **5 — quem desliga** | Hoje sinais comuns na trava param o filho; a aquisição seguinte também para containers rotulados de donos mortos. A sombra não tem mecanismo equivalente de rótulo e `_parar`. (`bin/trava_teste.sh:120–144`; `bin/trava_teste.sh:159–168`; `bin/sombra_trava.sh:21–39`; `bin/sombra.sh:182–192`.) | Introduz cancelamento por obra. “Job só morre por cancela-obra” precisa ser conciliado com o TERM por superação da linha 2 e com a recuperação de órfãos existente. Commit exato de reversão, drenagem dos jobs destacados e tratamento dos novos arquivos no rollback [nao verificado]. (`contrato.txt:2,5`.) |

O bloqueio novo também precisa definir sua relação com o disjuntor desligado e com exceções: hoje a chave pula `pretool`, e exceções fora de B3 liberam a ferramenta. (`bin/disjuntor.py:1486–1488`; `bin/disjuntor.py:1496–1506`; política futura [nao verificado].)

**2. Quem escreve e quem lê**

| Campo/arquivo | Escritores e leitores atuais | Responsabilidade nova |
|---|---|---|
| **`<trava>.fila/`** | Não há acesso a esse registro nos fluxos atuais de aquisição de testes ou sombra. (`bin/trava_teste.sh:85–171`; `bin/sombra_trava.sh:21–39`.) | Job registra identidade/estado; entrada nova consulta e supera; cancelador consulta; `--fila` lista e leitores podam mortos. Nomes dos arquivos, publicação atômica, sincronização e remoção terminal [nao verificado]. (`contrato.txt:1–3`.) |
| **Arquivo de rc** | O rc vem do `wait`; não há arquivo intermediário. (`bin/trava_teste.sh:169–171`.) | Job escreve, esperador lê e devolve. Caminho, exclusividade por job, publicação atômica e ordenação entre rc, saída final e remoção do registro [nao verificado]. (`contrato.txt:2–3`.) |
| **`<trava>.fila.jsonl`** | Os fluxos atuais não escrevem esse journal. (`bin/trava_teste.sh:96–171`; `bin/sombra_trava.sh:21–39`.) | Eventos são escritos pelo mecanismo novo; cancelamento inclui quem/motivo. Não há leitor do journal nomeado no contrato — `--fila` não é explicitamente definido como leitor dele. Escritor de cada transição e garantia contra eventos duplicados/perdidos [nao verificado]. (`contrato.txt:4–5`.) |
| **`.quem`** | Escrito após aquisição e no EXIT. Lido por `_quem` e pela mensagem de timeout. `_quem` consulta primeiro o flock: se estiver preso e o texto disser livre, responde “não sei”. (`bin/trava_teste.sh:59–81`; `bin/trava_teste.sh:98`; `bin/trava_teste.sh:102–103`.) | O contrato não define seu novo escritor/formato. Se ficar com o esperador, o EXIT dele poderá publicar “livre” com job vivo; a defesa atual de `_quem` evita aceitar esse texto enquanto o flock continuar preso. (`contrato.txt:2`; `bin/trava_teste.sh:61–66`; `bin/trava_teste.sh:103`; migração [nao verificado].) |
| **Rótulo do container** | Chave derivada do **basename da trava**; valor `$$-starttime`. Inserido apenas quando o comando começa por `docker run`. Lido por `_rotulados`, `_quem`, recuperação de órfãos e `_parar`; `_sem_rotulo` procura suites sem rótulo de trava alguma. (`bin/trava_teste.sh:37`; `bin/trava_teste.sh:51–56`; `bin/trava_teste.sh:71–78`; `bin/trava_teste.sh:101`; `bin/trava_teste.sh:109–156`; `bin/trava_teste.sh:162–163`.) | A identidade precisa representar o **job destacado**. Se conservar a identidade do esperador, sua morte fará o dono parecer morto. Para sombra, o vínculo precisa ser criado nos containers hoje sem esse rótulo. (`contrato.txt:2,5`; `bin/trava_teste.sh:47–49`; `bin/sombra.sh:183–191`; implementação [nao verificado].) |
| **rc e stdout/stderr do chamador** | Saída do filho segue pelos descritores herdados; o processo espera seu rc. (`bin/trava_teste.sh:169–171`.) | O esperador passa a retransmitir. Precisa entregar toda a saída antes de devolver o rc e suportar desaparecimento do consumidor sem matar o job. Esse protocolo [nao verificado]. (`contrato.txt:2`.) |

Os consumidores concretos de saída e rc são:

| Consumidor | Leitura e dependência |
|---|---|
| **Suite** | `exec` preserva a interface de processo, saída e rc da trava. (`bin/suite.sh:96–103`.) |
| **Régua** | Grava log, captura rc, rejeita códigos fora de 0/1 sem carimbo e julga log+rc. (`bin/regua.sh:165–180`.) |
| **Pre-push** | Captura ambos os fluxos em arquivo, filtra **depois** da chamada e devolve `_rc`; esse trecho não exige retransmissão ao vivo. (`bin/pre-push.sh:142–157`.) |
| **Vigia da árvore** | Nos três runs, usa arquivo de saída mais rc para decidir publicação. (`bin/vigia_arvore.sh:83–100`; `bin/vigia_arvore.sh:113–119`; `bin/vigia_arvore.sh:132–138`.) |
| **Régua relógio** | Consome fluxo por `tee`/`grep`, captura `PIPESTATUS[0]` e depois julga log+rc. Latência máxima exigida [nao verificado]. (`bin/regua_relogio.sh:51–62`.) |
| **Régua calendário** | Captura arquivo e rc por rodada; só compara resultados após veredito válido. (`bin/regua_calendario.sh:46–62`.) |
| **Isolamento** | Preserva somente 75; para os demais códigos decide pelo último `OK`, podendo perder um 143. (`bin/isolamento.sh:33–38`.) |
| **Placar** | Extrai contador por `sed \| head -1`, sem usar o rc; ausência vira `nao_medido`. O consumidor pode fechar antes do fim do produtor. (`bin/placar_code.sh:125–133`.) |
| **Molde fatia** | Distingue 75 e depois consulta `OK` nos arquivos. (`bin/molde_fatia/rodar.sh:30–36`; `bin/molde_fatia/rodar.sh:52–56`.) |
| **Molde autocura** | Distingue 75 e publica fim para os demais retornos. (`bin/molde_autocura/rodar.sh:22–33`.) |
| **`_rodar_testes`** | `subprocess.run` espera, grava saída e julga conteúdo+rc; 75 e resultado indefinido viram exceções próprias. (`app/core/esteira_vigia.py:794–809`.) |
| **Integrador** | Espera a régua; procura, no stdout daquela chamada, o caminho anunciado do log e julga aquele arquivo com o rc. (`app/core/integrador_lote.py:379–392`.) |

Há ainda leitores indiretos que não consultarão automaticamente a fila nova:

- **Posse:** handoff consulta diretamente o flock das duas travas; o disjuntor consulta o flock da sombra; o hook de Stop usa essa decisão para impedir `/clear` durante DIFF/apply. Soltar o fd antes do término altera essas leituras. (`bin/handoff_sessao.sh:223–234`; `bin/disjuntor.py:307–326`; `bin/hook_stop_fila1.py:785–795`.)
- **Métrica antiga da sombra:** `--esperas-longas` lê `sombra_espera.jsonl`; o placar consome a saída e converte `?` em problema. O journal novo não substitui esse formato sem migração. (`bin/sombra_trava.sh:42–60`; `bin/placar_code.sh:155–160`.)
- **Atividade durante execução:** o vigia lê conteúdo e mtime de arquivos de saída para classificar atividade e decidir ações. Acumular a saída até o fim pode alterar essas decisões quando o fluxo passar por esses arquivos; uma exigência universal de stdout em tempo real [nao verificado]. (`app/core/esteira_vigia.py:710–723`; `app/core/esteira_vigia.py:1552–1575`.)

**3. Contraexemplos que o texto ainda permite**

| Sequência | Consequência e fundamento |
|---|---|
| **Mesma obra, mesma volta:** A entra; B entra com o mesmo N. | A regra só elimina voltas **menores**. Ambos podem executar sequencialmente; não existe deduplicação por igualdade prevista. Se executar duas vezes é indesejado, falta uma regra. (`contrato.txt:2`.) |
| **Pedido antigo chega atrasado:** B/X/2 entra e varre a fila; depois A/X/1 finalmente se registra. | A varredura passada não encontra A. Sem máximo persistente por obra e validação na admissão, A ainda pode executar. O comportamento de `--abre-volta` necessário para impedir isso [nao verificado]. (`contrato.txt:1–2`.) |
| **Volta vazia:** A/X/vazia espera; B/X/2 chega. | A é imune à superação por definição. Logo, a proposta não promete limpar todos os pedidos antigos da obra. (`contrato.txt:1–2`.) |
| **PID reciclado:** A morre; seu PID passa a pertencer a outro processo; um leitor encontra o registro antigo. | Conferir starttime deve podar A, sem sinalizar o novo processo. Porém o texto não define nova validação imediatamente antes de TERM ao PGID: conferir, perder o processo e depois sinalizar ainda deixa uma janela. O código atual valida identidade do dono do container, sem protocolo novo de grupos. (`contrato.txt:2–3`; `bin/trava_teste.sh:39–49`; proteção futura [nao verificado].) |
| **Esperador morre antes do pidfile:** lançamento foi iniciado, mas registro/pidfile ainda não foi publicado; esperador morre e há nova tentativa. | O primeiro job pode continuar e a segunda tentativa criar outro. Mesmo com esperador vivo, ultrapassar 30 s sem pidfile não prova ausência do job. Falta protocolo para localizar/conciliar nascimento tardio. (`contrato.txt:2–3`; implementação [nao verificado].) | [nao verificado]
| **Superação no instante do flock:** A consta `fila`; A adquire flock; antes de publicar `rodando`, B decide superá-lo; A inicia Docker. | B pode matar um job já adquirido ou presumir “ainda sem container” quando isso deixou de valer. É necessário ordenar aquisição, estado e autorização de lançamento com a decisão de superação. Hoje há trabalho entre adquirir e lançar, mas nenhum estado de fila. (`contrato.txt:2`; `bin/trava_teste.sh:97–101`; `bin/trava_teste.sh:113–169`; atomicidade futura [nao verificado].) |
| **Duas travas distintas, basenames distintos:** cada uma protege uma suite no mesmo recurso. | Cada trava consulta seus rótulos; a detecção de suites sem rótulo ignora containers rotulados por **qualquer** trava. As duas podem deixar de enxergar a concorrência entre si. Na cópia normal o caminho é fixo; o cenário exige cópias/reapontamento. (`bin/trava_teste.sh:34–37`; `bin/trava_teste.sh:51–56`; `bin/tests/test_trava_teste.sh:202`.) |
| **Duas travas distintas, mesmo basename:** caminhos diferentes produzem a mesma chave. | Seus containers passam a compartilhar o espaço de identificação, podendo gerar 75 por dono vivo de outra trava ou recuperação cruzada de órfão. As filas, derivadas dos caminhos completos, continuam distintas. (`bin/trava_teste.sh:37`; `bin/trava_teste.sh:120–133`; `contrato.txt:2`.) |
| **Ausência transitória de rc:** job está vivo, ainda não publicou rc, e o esperador interpreta ausência como 143. | Produz rc falso e permite ao chamador limpar recursos enquanto o job trabalha. “Sem rc = 143” precisa ser aplicado após constatação de término/morte, não durante polling. Essa ordenação [nao verificado]. (`contrato.txt:2–3`.) | [nao verificado]
| **143 correto, leitor incorreto:** log já contém `OK`; job é interrompido e o esperador devolve 143. | Isolamento pode devolver sucesso, pois só preserva 75. As cadeias dos moldes também ignoram rc da régua e escolhem o `/tmp/regua_*.log` mais novo, podendo aproveitar resultado alheio. (`bin/isolamento.sh:37–38`; `bin/molde_fatia/cadeia.sh:51–55`; `bin/molde_autocura/cadeia.sh:48–50`.) |
| **Registro ilegível ou morto sem nova leitura:** gravação interrompida deixa registro ilegível; ou job morre e ninguém consulta depois. | O ilegível manda fechar em 75, sem recuperação definida; a poda de mortos ocorre apenas “na leitura”, sem garantia de quando acontecerá. Limpeza eventual automática [nao verificado]. (`contrato.txt:3`.) |
| **Cancelamento pede parada, mas Docker falha:** TERM chega; consulta/parada do container falha. | O `_parar` atual ainda sai 143 sem confirmar pista vazia. Portanto 143 não prova que o container terminou; a recuperação seguinte tem verificação mais forte e retorna 75 se o stop falhar. (`bin/trava_teste.sh:159–166`; `bin/trava_teste.sh:129–144`; `contrato.txt:5`.) |

Sobreviver ao esperador, por si só, é o comportamento pretendido; o problema dos cenários acima é sobreviver sem identidade recuperável, sem recursos ou sem consumidor do resultado. (`contrato.txt:2–5`; garantias de recuperação [nao verificado].)

**4. Chamadores que precisam mudar junto**

- **Sombra com fd 8 herdado: quebra se a migração destacar apenas a aquisição.** A variável herdada faz a função retornar imediatamente, sem conferir o descritor. Preservar a variável e perder fd 8 deixa a continuação sem adquirir a trava; remover a herança enquanto o pai segura a trava faz o filho disputar com o pai. (`bin/sombra_trava.sh:22–28`.)

  O par de simulação precisa conservar uma posse e uma identidade ao longo das duas fotos e da comparação: fotos gravam `SOMBRA_TRAVA_ID`; comparação rejeita identidades diferentes. Destacar cada foto com identidade nova também quebra o par. (`bin/simular_folha.sh:76`; `bin/simular_folha.sh:82–85`; `bin/simular_folha.sh:96–101`.)

- **Pre-push: o caminho normal pode manter sua interface, mas a morte do esperador expõe recursos temporários.** O hook apaga o archive no EXIT, enquanto o job monta esse diretório. Se o hook sair enquanto o job ainda espera, o archive pode desaparecer antes do Docker nascer. Relógio e calendário têm o mesmo vínculo entre temporário montado e limpeza no EXIT. (`bin/pre-push.sh:39–40`; `bin/pre-push.sh:144`; `bin/regua_relogio.sh:31–32`; `bin/regua_relogio.sh:54`; `bin/regua_calendario.sh:26–27`; `bin/regua_calendario.sh:48`.)

- **Cron: esperar até o resultado preserva o encadeamento; sobreviver ao shell muda o significado da medição.** O envelope mede fim/rc do comando, remove sua marca no EXIT e publica fim somente em 0/2. A sombra é encadeada por `--refazer && --bloco`: se esse shell morrer durante refazer, o job sobrevivente não executa sozinho a continuação `&& --bloco`. (`bin/cron_run.sh:9–25`; `app/config/crons.py:331–333`.) Cron efetivamente instalado em produção [nao verificado].

- **Integrador e cadeias: vida do teste e posse da árvore precisam continuar coordenadas.** O integrador segura locks externos; `pegar_arvore` usa fd 6/8. Herdá-los pode mantê-los presos depois da morte do chamador; fechá-los pode liberar a árvore enquanto o job ainda a lê. Além disso, a cadeia desfaz arquivos no EXIT e o integrador desfaz aplicações quando recebe `TesteNaoRodou`. (`bin/integrador.sh:32`; `bin/integrador.sh:41–51`; `bin/trava.sh:60–68`; `bin/molde_fatia/cadeia.sh:25–35`; `app/core/integrador_lote.py:653–656`.) Política dos descritores no job destacado [nao verificado].

- **Desligamento da esteira e selos: exigem revisão explícita.** O matador atual sinaliza grupos encontrados pelos detentores do lock da esteira; a nova sessão pode escapar ou continuar descobrível conforme os descritores herdados. O controlador também identifica trabalho pelo grupo. (`app/core/esteira_vigia.py:1026–1044`; `bin/esteira_slots.sh:34–47`; herança futura [nao verificado].) Já o selo de dono contradiz diretamente a sobrevivência proposta, tanto no TERM quanto no KILL; a régua executa esse selo antes da suite. (`bin/tests/test_trava_dono_para_filho.sh:54–80`; `bin/regua.sh:138–143`.)

O selo estático existente continua útil, mas reconhece texto/composição sintática: função Shell contendo `trava_teste.sh` e unidade Python contendo a constante correspondente. Isso não demonstra, sozinho, passagem efetiva pela trava em todos os caminhos; a implementação do novo gate também precisará distinguir execução de mera menção no comando. (`bin/tests/test_trava_teste.sh:83–91`; `bin/tests/test_trava_teste.sh:114–122`; gate futuro [nao verificado].)

## 31/10 21:57 · sessao d596822205ca · F1C4-3-RG-SEM-CELULA-CASO · leitura · origem=Code

**1. O dia entra em `mortos`?**

- **Na cópia atual, não entra por `_indef`.** A ausência da chave faz `_cels_rg_m.get((c.pk, _d))` devolver `None`; o teste de existência falha antes da chamada a `dia_sem_trabalho`. Portanto `_indef` fica vazio para esse chamado de um único dia. Isso não impede entrada pelos outros caminhos abaixo. (`app/ponto/management/commands/reconciliar_grade.py:281-291`)
- **Quanto ao pai de `9651ef38`: o trecho exato e suas linhas são `[nao verificado]`**, pois a cópia não disponibiliza esse histórico. A documentação local descreve a regra anterior como `sem_ancora → mortos → RETRACT_INDEFINIDO`, e o backlog registra especificamente que o dia sem âncora e sem célula “deixou de ser morto”. Isso sustenta o comportamento anterior descrito, mas não comprova sua atribuição à revisão solicitada. (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:4-7`; `app/docs/BACKLOG.md:832`)

**2. Outros caminhos e o retorno de `classificar_falta`**

Há três caminhos além de `_indef`:

- **`avaliado_limpo`:** recebe o dia quando não há `miss`. Porém, no processamento informado, `sem_ancora` adiciona o dia a `indefinidos` e executa `continue` antes dessa avaliação. Logo esse dia não entra por `avaliado_limpo`. (`app/ponto/management/commands/reconciliar_grade.py:139-156`)
- **`_coberto`:** pode incluir o dia diretamente em `mortos`. Exige vínculo `ec`; sem vínculo devolve `False`. Com vínculo, aceita a ausência selecionada por `ausencia_cobre(..., para='cobranca')` se seu tipo não for `falta`, ou uma substituição do titular naquele posto, sobreposta ao dia, em estado `designada`, `em_curso` ou `concluida`. Não exige `CelulaDia`. (`app/ponto/management/commands/reconciliar_grade.py:266-267`; `app/ponto/management/commands/reconciliar_grade.py:391-410`)
- **`_lei_cala_no_dia`:** se a lei calar em todas as datas de `dts`, une `dts` a `mortos`. Para um único dia basta essa consulta devolver `True`. O auxiliar testa `classificar_falta(...)[1] is None`; se a chamada lançar exceção, devolve `False`. (`app/ponto/management/commands/reconciliar_grade.py:290-291`; `app/ponto/marcos.py:166-176`)

**Sem `CelulaDia`, `classificar_falta` não tem retorno único.** A consulta devolve `None` e apenas deixa de disparar a guarda de célula com `trabalha is False`; o restante continua. (`app/escala/servico_jornada.py:262-271`; `app/escala/servico_jornada.py:302-304`)

O resultado depende destes insumos:

- **Ausência:** uma ausência que alcance o dia, não rejeitada e que não cubra apenas parte dele faz retornar `(None, None, None)`. Esse caminho não exige vínculo. (`app/escala/servico_jornada.py:340-345`; `app/ponto/turnos.py:2516-2533`; `app/ponto/turnos.py:2583-2600`)
- **Vínculo/escala:** sem célula, `escala_vigente` consulta os vínculos; `vinculo_do_dia` escolhe entre os que cobrem a data, priorizando `ativa` e `data_inicio`, ou devolve `None`. Ausência de célula não implica ausência de vínculo. (`app/escala/servico_jornada.py:46-58`; `app/escala/alimentacao.py:173-184`; `app/escala/alimentacao.py:253-254`)
- **Turno completo e teto temporal:** turno noturno completo faz calar. Havendo vínculo e tipo de escala, também cala quando existe fim esperado e `now() < fim_turno(...)`. Nessa chamada, `fim_turno` recebe o tipo, sem `ec`; os marcos vêm do template e são encadeados através da meia-noite. (`app/escala/servico_jornada.py:354-378`; `app/escala/servico_jornada.py:89-93`; `app/escala/servico_jornada.py:68-84`)
- **Batidas e marcos esperados:** ultrapassadas as guardas, usa os turnos do núcleo. Sem célula, `ec.marcos_do_dia` recorre ao template; esse método do template não decide trabalho/folga. (`app/escala/servico_jornada.py:390-435`; `app/escala/models.py:1094-1108`; `app/escala/models.py:326-345`)

Exemplos condicionais: com universo de batidas vazio e marcos esperados de entrada/saída, retorna `('E', 'saida_sem_entrada', 'ENTROU')`; sem marcos esperados, retorna `('S', 'orfao_14h', 'SAIU')`. Dia completo pode devolver `(None, None, None)`, e falha ao obter o universo do núcleo também faz calar. Portanto **“sem célula” sozinho não prova nem retratação nem manutenção**. (`app/escala/servico_jornada.py:390-402`; `app/escala/servico_jornada.py:440-480`)

**3. Quem consome o resultado**

| Consumidor | Efeito |
|---|---|
| `RETRACT_INDEFINIDO` | Incrementa se `dts` não vazio estiver contido em `mortos` e `_indef` não vazio, **antes** de consultar `premissa_morta`. No caso sem célula, esse ramo não incrementa. (`app/ponto/management/commands/reconciliar_grade.py:281-298`) |
| `RETRACT` e `retracts` | Só incrementa/adiciona depois de `premissa_morta` autorizar. (`app/ponto/management/commands/reconciliar_grade.py:298-304`) |
| `--apply` | Chama `chamados.reconciliador.retratar`; o conteúdo depende de `_indef`. A chamada ocorre depois da contagem. (`app/ponto/management/commands/reconciliar_grade.py:303-316`) |
| Stdout | Publica os contadores no `PLANO`, o tamanho de `retracts` e até 40 candidatos. (`app/ponto/management/commands/reconciliar_grade.py:318-325`) |
| Tripwire | `divergencia_grade_cartorio = len(creates) + len(retracts)`; não soma `RETRACT_INDEFINIDO` separadamente. (`app/ponto/management/commands/reconciliar_grade.py:337-342`) |

O contador representa candidatos: `retratar` pode devolver `False`, e esse retorno não é usado pelo comando para desfazer a contagem. (`app/chamados/reconciliador.py:520-526`; `app/chamados/reconciliador.py:546-548`; `app/ponto/management/commands/reconciliar_grade.py:303-316`)

Leitores/configuração encontrados:

- **Cron:** configuração às `06:20`, empresas `2,3,4`, sem `--apply`; stdout/stderr vão para `logs/reconciliar_grade.log`. (`app/config/crons.py:443`; `app/config/crons.py:32`; `app/config/crons.py:1801-1802`)
- **Vigia/placar:** extrai o último `divergencia_grade_cartorio` desse log, exige arquivo modificado hoje e publica `divergencia_grade_x_cartorio`. Esse valor não integra o `TOTAL` do Code. (`bin/placar_code.sh:528-532`)
- **Vigia de execução:** o envelope acompanha o código de saída e aciona `cron_quebrado` nas falhas; não interpreta esses contadores. (`bin/cron_run.sh:11-13`; `bin/cron_run.sh:27-38`)
- **Selos:** validam o contrato do tripwire, presença/ausência de `RETRACT_INDEFINIDO` e ausência de `--apply` no cron. (`app/ponto/tests/test_reconciliar_grade_sombra.py:71-88`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:61-74`; `app/chamados/tests/test_hx_um_emissor.py:5-9`)

Execução efetiva e valores em produção: `[nao verificado]`.

**4. Testes do caso específico** [nao verificado]

**Não localizei teste que execute esse comando com `sem_ancora=True` e ausência de `CelulaDia` para o dia do chamado.** A lacuna está explicitamente registrada como aberta. (`app/docs/BACKLOG.md:832`)

Os testes próximos cobrem situações diferentes: [nao verificado]

- `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py` simula grade indefinida, mas sempre cria `CelulaDia(trabalha=None)`. O caso chamado “sem ata” passa `ata=None`; a célula continua existindo. (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:28-34`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:55-70`)
- `test_a2_marco_morto.py` também cria a célula antes de executar com `apply=True` e verificar que `retratar` não foi chamado. (`app/ponto/tests/test_a2_marco_morto.py:46-55`)
- `test_reconciliar_sem_ancora_visivel.py` só inspeciona o código procurando contador/lista; não monta o cenário. (`app/ponto/tests/test_reconciliar_sem_ancora_visivel.py:11-18`)
- Em `app/chamados/tests`, o selo do comando verifica os argumentos do cron; o teste de recuperação da data cria célula existente e chama `_cham_dts`, sem exercitar esse julgamento em `handle`. (`app/chamados/tests/test_hx_um_emissor.py:5-9`; `app/chamados/tests/test_c5_emissores.py:39-51`)

## 31/10 21:56 · sessao 3ff5b8f0a0eb · - · - · origem=Ronald (tmux)

Não localizei teste que exercite **`sem_ancora=True` com ausência da chave `(colab, dia)` em `celulas_do_periodo`**. A lacuna está registrada explicitamente como obra aberta em `app/docs/BACKLOG.md:832`: “dia sem ancora e sem celula deixou de ser morto sem caso na bateria”.

Os testes próximos usam **célula existente**: [nao verificado]

- `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py` simula grade indefinida (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:28`–34), mas `_rodar` sempre cria `CelulaDia(trabalha=None, ata=ata)` (`:55`) antes de executar `handle` (`:58`). Os testes cobrem ata de trabalho (`:61`–65), `ata=None` (`:67`–70) e ata de folga (`:72`–74). **“Sem ata” não significa “sem CelulaDia”** (`:55`, `:68`).
- `test_cobranca_em_dia_indefinido_fica_viva_limbo_cobra` também simula `sem_ancora=True` (`app/ponto/tests/test_a2_marco_morto.py:26`–32), mas cria expressamente `CelulaDia(trabalha=None)` (`:46`). Com `apply=True` (`:50`), verifica que `retratar` nem foi chamado (`:52`) e que o chamado permanece vivo (`:54`–55).
- `test_plano_declara_sem_ancora` apenas inspeciona o texto do comando e exige `SEM_ANCORA`/`sem_ancora_dias`; não monta um caso com chamado e ausência de célula (`app/ponto/tests/test_reconciliar_sem_ancora_visivel.py:11`–18).

Em `app/chamados/tests`, os casos encontrados ligados ao comando não exercitam esse cenário:

- O teste de `_cham_dts` cria célula com `trabalha=True` e verifica a data recuperada do vínculo do chamado com ela (`app/chamados/tests/test_c5_emissores.py:39`–51).
- O selo do cron verifica ausência de `--apply`, sem executar o julgamento de dia indefinido (`app/chamados/tests/test_hx_um_emissor.py:5`–9).
- Os testes das portas chamam auxiliares `_relavrar_orfa` e `_disputa`, não `Command.handle` (`app/chamados/tests/test_porta_chamado_e2.py:62`–72; `app/chamados/tests/test_porta_chamado_e3.py:92`–94).

Há teste de **ausência de célula em outro escopo**: `GateCelulaTest` deixa `d_sem` sem criação correspondente (`app/ponto/tests/test_t31_gate_celula.py:16`–25) e verifica `celula_veta_cobranca(..., d_sem) is None` (`:32`–35). Ele não executa `reconciliar_grade.handle` nem testa `_indef`/`mortos` (`:32`–35).

O comentário dos testes descreve a regra anterior como `sem_ancora -> mortos -> RETRACT_INDEFINIDO` (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:4`–7); a correspondência byte a byte com o **pai de `9651ef38`** continua `[nao verificado]`, pois não encontrei histórico Git ou reprodução desse pai na cópia.

## 31/10 21:56 · sessao c686af4f9bc0 · - · - · origem=Ronald (tmux)

Não encontrei a versão histórica nem referência a `9651ef38` na cópia; o conteúdo exato e as linhas do pai de `9651ef38` ficam **[nao verificado]**.

Há duas evidências locais sobre a mudança:

- `app/docs/BACKLOG.md:832`: registra explicitamente **“reconciliar_grade.py:281-282, dia sem ancora e sem celula deixou de ser morto sem caso na bateria”**.
- `app/ponto/management/commands/reconciliar_grade.py:277`: comentário histórico informa **“Ate aqui `sem_ancora` ia inteiro para `mortos`”**.

Na cópia atual, `_indef` exige célula existente antes de perguntar ao juiz: `app/ponto/management/commands/reconciliar_grade.py:281-282`. Sem chave no mapa, `.get(...)` retorna `None`, o primeiro operando falha e o dia não entra em `_indef`; portanto não entra em `mortos` **por esse caminho** (`app/ponto/management/commands/reconciliar_grade.py:283`).

O teste citado pelo comentário não cobre ausência de célula: `_rodar` sempre cria uma `CelulaDia`, inclusive quando `ata=None` (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:55`, `:67-68`).

## 31/10 21:56 · sessao 31b27f047daf · - · - · origem=Ronald (tmux)

Leitores e fluxo do resultado na cópia:

- `RETRACT_INDEFINIDO` incrementa quando todas as datas estão em `mortos` e `_indef` é não vazio, **antes** do veto por `premissa_morta`; portanto pode subir sem gerar `RETRACT`. `RETRACT` e `retracts.append` só acontecem após esse veto. (`app/ponto/management/commands/reconciliar_grade.py:292–304`)
- Com `--apply`, chama `chamados.reconciliador.retratar`; a mensagem depende de `_indef`. (`app/ponto/management/commands/reconciliar_grade.py:305–316`)
- O stdout imprime todos os contadores em `PLANO`, o tamanho de `retracts` e até 40 itens da lista. (`app/ponto/management/commands/reconciliar_grade.py:318–325`)
- O tripwire usa **`len(creates) + len(retracts)`**, não soma diretamente `RETRACT_INDEFINIDO`; imprime também `create`, `retract`, `orfa_dry`, esperado e dono. (`app/ponto/management/commands/reconciliar_grade.py:337–342`)
- O cron configurado roda às **06:20**, com `--empresa 2,3,4`, sem `--apply`; o log padrão é `reconciliar_grade.log`, com stdout/stderr redirecionados pelo renderizador. (`app/config/crons.py:443`, `app/config/crons.py:32`, `app/config/crons.py:1801–1802`)
- O leitor do contador é `bin/placar_code.sh`: extrai o último `divergencia_grade_cartorio` do log, exige log modificado hoje e publica `divergencia_grade_x_cartorio`; esse valor não participa do `TOTAL` do Code. (`bin/placar_code.sh:528–532`)
- O envelope do cron acompanha o **exit code**, não interpreta `RETRACT`/tripwire; registra execução e aciona `cron_quebrado` nas falhas. (`bin/cron_run.sh:11–13`, `bin/cron_run.sh:27–38`)
- Selos que leem o stdout: `test_reconciliar_grade_sombra` verifica existência e contrato completo do tripwire; `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz` verifica ausência/presença de `RETRACT_INDEFINIDO` e divergência zero. (`app/ponto/tests/test_reconciliar_grade_sombra.py:71–88`, `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:61–74`)
- O selo do cron cobra ausência de `--apply`. (`app/chamados/tests/test_hx_um_emissor.py:5–9`)

Execução real do cron, conteúdo de log e valor do contador em produção: `[nao verificado]`.

## 31/10 21:56 · sessao 8dc87397ccfa · - · - · origem=Ronald (tmux)

Leitura concluída sem alterações. Não encontrei `AGENTS.md` no resultado de `rg --files -g AGENTS.md`; instruções externas a esse conjunto [não verificado].

**Chamadores e consumidores de rc/stdout**

| Chamador | Fluxo atual e dependência |
|---|---|
| `suite.sh` | Faz `exec env ESTEIRA_QUEM=... bash trava_teste.sh`, herdando stdout/stderr e devolvendo o rc pelo próprio processo: `bin/suite.sh:96`. Não declara obra/volta nesse ponto: `bin/suite.sh:96`. |
| Régua | Suite escreve em `$LOG`; lê `$?`; só permite 0/1 julgar e devolve outros códigos sem carimbo; juiz lê log+rc: `bin/regua.sh:165`, `bin/regua.sh:170`, `bin/regua.sh:175`, `bin/regua.sh:180`. |
| Pre-push | Captura stdout/stderr em arquivo; guarda rc verdadeiro; imprime filtro só depois de terminar; devolve `_rc`: `bin/pre-push.sh:142`, `bin/pre-push.sh:145`, `bin/pre-push.sh:146`, `bin/pre-push.sh:157`. Portanto não exige stdout ao vivo nesse trecho. |
| Vigia da árvore | Captura stdout/stderr em log; lê rc e passa ambos ao juiz nos três runs: `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:86`, `bin/vigia_arvore.sh:99`, `bin/vigia_arvore.sh:113`, `bin/vigia_arvore.sh:132`, `bin/vigia_arvore.sh:137`. |
| Régua relógio | Pipeline `trava ... 2>&1 | tee ... | grep ...`; captura `PIPESTATUS[0]` e julga log+rc: `bin/regua_relogio.sh:51`, `bin/regua_relogio.sh:56`, `bin/regua_relogio.sh:57`, `bin/regua_relogio.sh:62`. É leitor de fluxo incremental via `tee`/filtro; necessidade operacional de latência específica [não verificado]. |
| Régua calendário | Cada run escreve log; lê rc; 75 retorna 75; restantes passam pelo juiz: `bin/regua_calendario.sh:46`, `bin/regua_calendario.sh:50`, `bin/regua_calendario.sh:51`, `bin/regua_calendario.sh:56`. |
| Isolamento | Só preserva 75; para qualquer outro rc decide pelo último `OK` do log: `bin/isolamento.sh:33`, `bin/isolamento.sh:37`, `bin/isolamento.sh:38`. Contraexemplo: log contém `OK`, job morre 143; `_iso_roda` pode devolver 0 e chamador declarar “verdes sem a fatia”: `bin/isolamento.sh:49`, `bin/isolamento.sh:50`. |
| Placar | `trava ... | sed ... | head -1` dentro de substituição; ignora rc e lê contador; ausente vira `nao_medido`: `bin/placar_code.sh:125`, `bin/placar_code.sh:132`, `bin/placar_code.sh:133`. `head` pode encerrar leitura antes do fim; preservação do job exige relay tolerar consumidor fechado [não verificado quanto à implementação proposta]. |
| Molde fatia | Chamadas em `bin/molde_fatia/rodar.sh:30` e `bin/molde_fatia/rodar.sh:44`; só distingue 75, depois julga `OK` no arquivo: `bin/molde_fatia/rodar.sh:34`, `bin/molde_fatia/rodar.sh:35`, `bin/molde_fatia/rodar.sh:53`, `bin/molde_fatia/rodar.sh:55`, `bin/molde_fatia/rodar.sh:56`. |
| Molde autocura | Chamada em `bin/molde_autocura/rodar.sh:22`; só distingue 75; publica fim para outros rc: `bin/molde_autocura/rodar.sh:28`, `bin/molde_autocura/rodar.sh:32`, `bin/molde_autocura/rodar.sh:33`. |
| `_rodar_testes` | `subprocess.run` espera processo, escreve saída em arquivo e usa rc+conteúdo no juiz: `app/core/esteira_vigia.py:794`, `app/core/esteira_vigia.py:802`, `app/core/esteira_vigia.py:803`, `app/core/esteira_vigia.py:805`, `app/core/esteira_vigia.py:807`. |
| Integrador | Wrapper herda locks fd7/fd8 e chama Python: `bin/integrador.sh:41`, `bin/integrador.sh:44`, `bin/integrador.sh:51`. `_suite` espera régua, lê rc, busca caminho anunciado no trecho novo de stdout e julga log específico: `app/core/integrador_lote.py:379`, `app/core/integrador_lote.py:380`, `app/core/integrador_lote.py:385`, `app/core/integrador_lote.py:388`, `app/core/integrador_lote.py:390`. Não usa mais “log mais novo”. |
| Cron | Geração envelopa comando em `cron_run.sh` e redireciona saída para log: `app/config/crons.py:1431`, `app/config/crons.py:1791`. Runner espera `bash -c`, mede duração/rc, grava fim só em 0/2 e alarma outros rc: `bin/cron_run.sh:11`, `bin/cron_run.sh:13`, `bin/cron_run.sh:22`, `bin/cron_run.sh:32`. Régua relógio/calendário no crontab instalado [não verificado]; entrada de vigia existe em `app/config/crons.py:277`. |

**Quebras concretas/condicionais por destaque**

1. **Selo atual quebra diretamente.** Régua executa todos os selos de host antes da suite e aborta se qualquer falhar: `bin/regua.sh:138`, `bin/regua.sh:140`. Selo exige que TERM no PID da chamada mate container e produza rc143: `bin/tests/test_trava_dono_para_filho.sh:54`, `bin/tests/test_trava_dono_para_filho.sh:60`, `bin/tests/test_trava_dono_para_filho.sh:62`. Também exige KILL da chamada deixar ORFAO: `bin/tests/test_trava_dono_para_filho.sh:68`, `bin/tests/test_trava_dono_para_filho.sh:74`, `bin/tests/test_trava_dono_para_filho.sh:77`. Contrato faz esse PID virar esperador cuja morte preserva job; selo precisa mudar junto, ou régua fica vermelha antes de testar.

2. **Recursos temporários seguem pertencendo ao chamador.** Pre-push cria archive e registra `rm -rf` no EXIT: `bin/pre-push.sh:39`, `bin/pre-push.sh:40`; job monta aquele archive: `bin/pre-push.sh:144`. Se esperador morrer antes da aquisição, hook sair pelo rc/sinal e executar EXIT, archive pode sumir antes do `docker run`; execução desse sinal/EXIT específico em produção [não verificado]. Mesmo problema estrutural para calendário com `$TMP/app` (`bin/regua_calendario.sh:26`, `bin/regua_calendario.sh:27`, `bin/regua_calendario.sh:48`), relógio com shim em `$TMP` (`bin/regua_relogio.sh:31`, `bin/regua_relogio.sh:32`, `bin/regua_relogio.sh:54`) e fallback efêmero do vigia (`bin/vigia_arvore.sh:23`, `bin/vigia_arvore.sh:58`, `bin/vigia_arvore.sh:85`).

3. **Locks extras não podem ser herdados sem decisão.** Integrador segura fd7 `/tmp/regua.lock`, fd8 temporário de push e locks da árvore via `trava.sh`: `bin/integrador.sh:32`, `bin/integrador.sh:41`, `bin/integrador.sh:51`; árvore é fd8 e raia fd6: `bin/trava.sh:60`, `bin/trava.sh:64`. Destaque preservando FDs pode manter locks depois do dono morrer; fechando-os pode deixar job lendo árvore já livre para outro escritor. Política do contrato para esses FDs [não verificado].

4. **Restauração da árvore é ligada ao chamador.** Cadeia instala trap EXIT que faz checkout/remove arquivos e recarrega aplicação: `bin/molde_fatia/cadeia.sh:25`, `bin/molde_fatia/cadeia.sh:27`, `bin/molde_fatia/cadeia.sh:29`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:35`; régua é chamada depois: `bin/molde_fatia/cadeia.sh:51`. Se cadeia sai enquanto teste destacado continua, a árvore montada pode mudar sob o job; proteção contratual [não verificado].

5. **Matador da esteira não alcança nova sessão automaticamente.** Vigia descobre grupos dos detentores do lock e manda TERM/KILL para eles: `app/core/esteira_vigia.py:1026`, `app/core/esteira_vigia.py:1031`, `app/core/esteira_vigia.py:1034`, `app/core/esteira_vigia.py:1037`. Job criado em nova sessão/grupo pode sobreviver a esse desligamento; se herdar o lock, o `fuser` pode descobri-lo também. FDs herdados pelo contrato [não verificado].

6. **Unidade do integrador já permite descendentes sobreviverem ao principal.** `KillMode=process`: `bin/hasner-integrador.service:10`. Portanto `setsid` não resolve por si só protocolo de fim, recursos e locks; serviço instalado [não verificado].

7. **Consumidores antigos ainda aceitam resultado textual sem rc.** Cadeias ignoram rc da régua e escolhem `/tmp/regua_*.log` mais novo, verificando `Ran`/`OK`: `bin/molde_fatia/cadeia.sh:51`, `bin/molde_fatia/cadeia.sh:53`, `bin/molde_fatia/cadeia.sh:54`, `bin/molde_fatia/cadeia.sh:55`; autocura idem `bin/molde_autocura/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:50`. Rc143 correto do relay não basta para esses leitores.

**Leitores complementares**

- Handoff mede somente `flock`, não `.quem` nem fila: `bin/handoff_sessao.sh:223`, `bin/handoff_sessao.sh:230`. Fila existente com lock momentaneamente livre pode aparecer LIVRE; adaptação prevista [não verificado].
- Placar lê contador de esperas longas da sombra: `bin/placar_code.sh:155`, `bin/placar_code.sh:158`.
- Leitor direto externo de `.quem` não foi encontrado; na trava ele é lido em `bin/trava_teste.sh:63`, `bin/trava_teste.sh:80`, `bin/trava_teste.sh:98` e escrito em `bin/trava_teste.sh:102`, `bin/trava_teste.sh:103`. Escritor deve passar ao job para o esperador morto não publicar “livre” enquanto ele roda.

_1274 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
