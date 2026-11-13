# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 01:31** · publicado em **13/11/2026 01:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 01:31 · sessao b845571b3f94 · - · - · origem=Ronald (tmux)

D. O corpo de `pode_executar_csf` continua sendo o predicado “existe `respondida_em` na origem OU nas perguntas que resolvem”; não exige texto, disputa aberta, pergunta viva ou categoria. Há dois chamadores de produção na cópia: command e gate. O registro diz um antes e dois depois; a alegação de três chamadores no aval fornecido não confere com a cópia. O histórico Git da base não está disponível para confirmação independente. (`app/ponto/supra_juiz.py:551`, `app/ponto/management/commands/supra_juiz.py:261`, `app/chamados/models.py:716`, `app/docs/CORTES.md:448`; histórico independente [não verificado].)

O universo **das invocações** aumentou: o command pergunta pelos `vivos`; o novo gate pergunta por chamados com categoria `disputa_supervisao`. A função não mudou, mas “mesmo universo antes e depois” não descreve seus chamadores. (`app/ponto/management/commands/supra_juiz.py:260`, `app/chamados/models.py:713`.)

Existe outro predicado que preserva resposta do colaborador contra reconciliação automática: `_tem_resposta_do_colab` usa **texto não vazio**, apenas nas perguntas que resolvem o chamado; seus dois leitores evitam reconciliar o chamado. Isso diverge do CSF em texto sem carimbo, carimbo sem texto e conversa apenas na origem. A declaração de autoridade única baseada somente no grep de `disputa_origem` não cobre esse predicado. Não encontrei função literal `tem_conversa`; o candidato concreto é este. (`app/ponto/services/cartorio.py:1637`, `app/ponto/services/cartorio.py:1648`, `app/ponto/services/cartorio.py:1665`, `app/ponto/supra_juiz.py:555`, `app/docs/CORTES.md:448`.)

Há ainda uma divergência entre comentário e execução: o gate diz que a metade DESTINO do juiz “não se aplica”, mas chama a função inteira. Um chamado da categoria `disputa_supervisao`, sem disputa de origem e com pergunta respondida apontando para ele como destino, também será recusado como pai. A existência desse arranjo na produção é [não verificado]. (`app/chamados/models.py:704`, `app/chamados/models.py:713`, `app/chamados/models.py:716`, `app/ponto/supra_juiz.py:559`.)

E. O contador lê a trilha e não rejulga. Conta chamados atualmente VIVOS que tenham **qualquer** recusa histórica com aquele motivo; não exige última recusa, nova recusa após reabertura, categoria ou disputa atual. Assim, é exatamente “vivos com trilha histórica”, não necessariamente “recusados no ciclo atual”. Não tem leitor de produção; o teste é seu consumidor. A documentação aponta o módulo errado (`models.py`). (`app/chamados/juizes.py:2510`, `app/chamados/juizes.py:2519`, `app/chamados/juizes.py:2522`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:59`, `app/docs/JUIZES.md:92`.)

H. Mapa de escritores e leitores relevantes:

| Campo/valor | Quem escreve | Quem lê / significado |
|---|---|---|
| `status_local` | `_gravar_estado`; `_vivas_entre` apenas simula e restaura memória. (`app/chamados/models.py:228`, `app/chamados/models.py:356`, `app/chamados/models.py:371`.) | Catálogo define VIVOS/ENCERRADOS/TERMINAIS; contador usa VIVOS; gate restringe categoria. (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/juizes.py:2522`, `app/chamados/models.py:713`.) |
| `resolvido_em` | `_gravar_estado`: carimba terminal, preserva resolvido→fechado e limpa ao voltar vivo. (`app/chamados/models.py:242`, `app/chamados/models.py:248`.) | Painel conta histórico pelo carimbo, separando autoria por `respondido_por`. (`app/chamados/services/painel.py:241`, `app/chamados/services/painel.py:244`, `app/chamados/services/painel.py:249`.) |
| Recusa `conversa_exige_decisao_humana` | Gate entrega payload a `_registrar_recusa`; este compara última recusa e mudança posterior de estado; `evento` persiste. (`app/chamados/models.py:719`, `app/chamados/models.py:403`, `app/core/observ.py:134`.) | Contador lê toda a história daquele motivo, não só última linha. (`app/chamados/juizes.py:2519`.) |
| `respondida_em` | Resposta real, resposta automática de fato entregue por outro caminho, recuperação pela trilha e simetria de `carimbar`; reabertura/ressuscitação zeram. (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3162`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1596`.) | CSF aceita só timestamp; C6 exige timestamp **e texto** no degrau da resposta; cartório aceita texto. (`app/ponto/supra_juiz.py:557`, `app/chamados/juizes.py:1417`, `app/ponto/services/cartorio.py:1641`.) |
| `validada_em` | Decisão de validar; simetria de carimbo também preenche. Nova resposta, devolução, reabertura e ressuscitação podem limpar. (`app/chamados/services/validacao.py:134`, `app/chamados/models.py:3165`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/validacao.py:199`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1596`.) | C6 mata no primeiro degrau; estado exibe VALIDADA/RESOLVIDA; validação evita segundo ato. (`app/chamados/juizes.py:1401`, `app/chamados/models.py:3071`, `app/chamados/services/validacao.py:396`.) |

Não há equivalência universal de significado: `validada_em` também é preenchido por simetria, e `respondida_em` sozinho não prova fala com texto. A documentação já admite ambas as condições; são anteriores ao novo gate. (`app/chamados/services/validacao.py:19`, `app/chamados/services/materializacao.py:830`, `app/chamados/services/disputa_emissao.py:1983`.)

A trilha é best-effort: a recusa pode preservar o pai sem conseguir gravar a linha; nesse caso o contador não o verá. Repetições consecutivas com mesmo payload e sem nova escrita de estado deduplicam; alternância de motivos não garante uma única linha histórica. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/chamados/models.py:403`.)

I. Há evidência concreta de predicados paralelos para preservar resposta contra cascata, com critérios distintos; não é sustentada a declaração irrestrita de juiz único. Também há campos com usos distintos já documentados (`validada_em`: decisão/simetria; `respondida_em`: projeção com possível passivo sem texto). A classificação desses achados como bloqueantes da O168 depende da unidade do aval, mas a afirmação de unicidade/mismo universo precisa ser corrigida ou demonstrada com censo abrangente. (`app/ponto/services/cartorio.py:1637`, `app/ponto/supra_juiz.py:551`, `app/docs/CORTES.md:448`, `app/chamados/services/validacao.py:19`, `app/chamados/services/materializacao.py:830`.)

## 13/11 01:31 · sessao a58117558288 · - · - · origem=Ronald (tmux)

**A — árvore atual.** Não achei fechador atual de `ChamadoColaborador` por sistema fora da lista: os atos encontrados estão em `colaboradores/services/desligamento.py:156`, `colaboradores/services/vinculo.py:388`, `chamados/services/competencia_trancada.py:209`, `chamados/services/regularizacao_ext.py:206`, `chamados/reconciliador.py:497` e nos três comandos em `escala/management/commands/fechar_avisos_regua_na_lista.py:48`, `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `ponto/management/commands/fechar_chamados_meio_periodo.py:110`. O censo puro O168 executado em memória produziu 21 sítios/23 chamadas e nenhuma divergência; o censo E4B integral produziu zero escritores fora da porta — mecanismos em `chamados/tests/test_o168_fechadores_fora_do_gate.py:113` e `chamados/tests/e4b_c12_ast.py:221`.

**F — mordida e bloqueante concreto.** O selo detecta o mutante nominal com importação `as`, chamada adicional e remoção de sítio: reconhecimento em `chamados/tests/test_o168_fechadores_fora_do_gate.py:82`, comparação em `:139`, asserções em `:151`, mutantes em `:201` e `:214`. Também reconhece `t = modulo.transicionar_com_pauta`; não reconhece `t = transicionar_com_pauta` nem `getattr(modulo, 'transicionar_com_pauta')` — o reconhecimento de atribuição aceita apenas `ast.Attribute`, em `:87`, e de chamada aceita apenas `ast.Name` registrado ou `ast.Attribute`, em `:100`.

Reproduzi exclusivamente em memória um **sétimo fechador que passa pelos dois censos**:

```python
from chamados.models import ChamadoColaborador as C
from chamados.services.fecho_runtime import transicionar_com_pauta
t = transicionar_com_pauta

def fechar_pelo_sistema(pk):
    ch = C.objects.get(pk=pk)
    t(ch, 'fechado', None, ato=ch.encerrar)
```

A catraca O168 não o conta, pela limitação acima (`chamados/tests/test_o168_fechadores_fora_do_gate.py:87`); o E4B reconhece a porta canônica e autoriza o ato, portanto também não acusa escritor fora da porta (`chamados/tests/e4b_c12_ast.py:246`). Resultado observado: O168 `{}`; E4B `NEW_WRITER_ROWS=[]`, porta sintética reconhecida e zero violações na árvore completa. Trata-se de **mutante sobrevivente**, não de caminho atual de produção; contraria a catraca geral de novos fechadores exigida no aval fornecido. A execução em memória não possui arquivo persistido de resultado [não verificado].

**Cron.** Os três nomes aparecem somente em `FORA_DE_PIPELINE`, cujo dicionário começa em `app/config/crons.py:769`: `fechar_chamados_meio_periodo` em `:1317`, `fechar_avisos_regua_na_lista` em `:1441`, `fechar_chamados_vinculo_na_lista` em `:1443`. Na fonte atual não há outra ocorrência nominal. O teste verifica pertencimento e uma ocorrência textual entre aspas simples (`chamados/tests/test_o168_fechadores_fora_do_gate.py:169`); cron efetivamente instalado [não verificado]. A contagem textual tem fragilidade futura: uma agenda com aspas duplas escaparia desse critério (`:177`).

**G — deslocamento e custo.** `CORPO` continua com nove entradas e `EXCECOES` vazio; o ajuste mantém as identidades nominais dos mesmos escritores (`chamados/tests/e4b_c12_ast.py:18`). O deslocamento de 29 linhas corresponde às três linhas inseridas na decisão e ao método de 26 linhas (`chamados/models.py:686`, `:699`). O censo integral atual sem violações é compatível com deslocamento, não com ampliação de exceções (`chamados/tests/e4b_c12_ast.py:221`).

Os tetos 220 e 280 e a atribuição de +3 são registrados em `ponto/tests/test_selo_chokepoint_escrita.py:196` e `:210`. O juiz realiza as três leituras descritas para um pai com uma disputa e nenhuma resposta (`ponto/supra_juiz.py:555`, `:556`, `:559`). Os valores ORM medidos, o comparativo 1/3 furos e os logs mencionados não foram fornecidos: medição empírica [não verificado].

A redução 2→1 das falhas é estaticamente compatível com o gate: recusa impede `_gravar_estado` (`chamados/models.py:652`) e `_retratar` retorna antes da auto-rejeição da justificativa, resolver e nota (`chamados/reconciliador.py:524`). Contudo, `r['falhas']` **conta lotes de colaborador com alguma falha**, não exceções nem chamadas a `fechar` (`chamados/reconciliador.py:1352`, `:1393`, `:1403`). Duas batidas presentes no mesmo lote não explicam duas unidades de falha; seria necessário outro lote/evento, cuja origem exata neste ensaio é [não verificado]. O comentário “MEDIDO” do teste não substitui essa prova (`chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:155`).

**I.** Classe comprovada nesta fatia: **selo sem mordida completa**, pelo mutante de sétimo fechador com alias por atribuição; recomendação de bloqueante fundamentada no reconhecimento incompleto de `chamados/tests/test_o168_fechadores_fora_do_gate.py:87` e na autorização complementar de `chamados/tests/e4b_c12_ast.py:246`.

## 13/11 01:30 · sessao 7e4311f26616 · - · - · origem=Ronald (tmux)

A/B/C — resultados de leitura estática:

- O pai passa pelo novo gate antes de `_gate_pergunta_viva` e `_gravar_estado`; a disputa conserva seu fechamento independente. Assim o pai comum de categoria `disputa_supervisao` com conversa fica vivo, enquanto `DisputaSupervisao.fechar` grava `fechada_em`, autor e resolução normalmente (`app/chamados/models.py:686`, `app/chamados/models.py:713`, `app/chamados/models.py:1962`). Sem outra disputa aberta, deixa de incidir a trava específica da RegularizacaoExterna; sucesso integral da regularização continua condicionado às demais guardas (`app/chamados/services/regularizacao_ext.py:59`).

- **Falta a nota pedida no GREEN literal do aval.** A recusa grava somente `LogAuditoria`; S88 retorna antes de anexar seu Followup. A timeline do supervisor lê Followups, portanto não apresenta o motivo novo da retenção (`app/chamados/models.py:719`, `app/chamados/models.py:403`, `app/chamados/signals.py:43`, `app/chamados/signals.py:46`, `app/chamados/views.py:372`, `app/templates/chamados/detalhe_local.html:112`). O aval exige “pai aberto com nota”; referência de linha do anexo [não verificado].

- Na recusa nova, `_f1_dedup` já foi retirado da instância e a variável local é descartada; `_f1_ancoradas` vira `None`. O gate da pergunta viva ainda não rodou: não reata perguntas e não grava trilha de dedup. `reconciliar` retorna antes de alterar status, resolução e urgência (`app/chamados/models.py:685`, `app/chamados/models.py:687`, `app/chamados/models.py:689`, `app/chamados/models.py:694`, `app/chamados/models.py:652`).

- Para duas recusas consecutivas com mesmo payload e sem mudança de estado, `_registrar_recusa` produz uma linha; dry-run não registra. Isso está afirmado pelo teste, mas execução [não verificado] (`app/chamados/models.py:403`, `app/chamados/models.py:405`, `app/chamados/models.py:718`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:119`).

- Existe escrita anterior condicional: se esse pai também satisfizer `e_chamado_de_furo` e não tiver dia forte, `_gate_premissa_do_fechamento` escreve `fechamento_sem_dia_do_fato` antes de O168 recusar, e esse evento não deduplica. Não demonstrado nos três tipos normais de pais; ocorrência em produção [não verificado] (`app/chamados/models.py:757`, `app/chamados/models.py:762`, `app/chamados/models.py:764`, `app/chamados/models.py:1274`, `app/chamados/services/disputa_emissao.py:568`, `app/chamados/services/disputa_emissao.py:1104`, `app/chamados/services/disputa_emissao.py:1339`).

- **Validação humana:** carimba validação, materializa a pergunta e chama `materializar_perguntas_validadas_da_disputa`; sua cauda fecha a disputa por C7, acionando S88 e O168. Perguntas e seus carimbos persistem; pai fica vivo; O168 não desfaz materialização nem validação. Outro chamador da cauda é o lote de passivo validado (`app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:463`, `app/chamados/services/validacao.py:469`, `app/chamados/services/materializacao.py:1384`, `app/chamados/services/materializacao.py:1322`, `app/chamados/services/materializacao.py:1339`, `app/chamados/services/passivo_validado.py:405`).

- A ação de validação informa “Pergunta validada e ponto gravado no espelho”; regularização informa “Disputa … regularizada totalmente”. Nenhuma traduz o pai retido pelo novo gate. No modal, o supervisor pode ver simultaneamente o status vivo do chamado e “Disputa fechada”; no detalhe, o painel de disputa só carrega disputa aberta (`app/chamados/services/acoes_disputa.py:57`, `app/chamados/services/acoes_disputa.py:162`, `app/templates/chamados/partials/modal_fio.html:102`, `app/templates/chamados/partials/modal_fio.html:379`, `app/chamados/services/acoes_chamado.py:342`).

- **Retratação por premissa morta:** `_retratar` retorna imediatamente se O168 recusar. Permanecem como estavam disputa, perguntas e justificativa automática; justificativa pendente não sofre a auto-rejeição posterior; não nasce nota de retratação (`app/chamados/reconciliador.py:524`, `app/chamados/reconciliador.py:530`, `app/chamados/reconciliador.py:537`, `app/chamados/reconciliador.py:539`). O contador de retratados só aumenta com retorno verdadeiro (`app/chamados/reconciliador.py:1465`).

- **Porta humana real:** detalhe → resposta livre → `transicionar_com_pauta` sem `ato` → `transicionar`, que não lê O168. O dropdown oferece resolvido/fechado. Continuam valendo justificativa pendente, disputa viva, pergunta viva e S122; logo existe porta, mas não promessa de fechamento para qualquer estado (`app/chamados/views.py:546`, `app/chamados/services/acoes_chamado.py:319`, `app/chamados/services/acoes_chamado.py:322`, `app/chamados/services/fecho_runtime.py:181`, `app/chamados/models.py:135`, `app/chamados/models.py:148`, `app/chamados/models.py:181`, `app/templates/chamados/detalhe_local.html:390`).

## 13/11 01:29 · sessao 2e4159f3821a · O168 · sonda · origem=Code

Não executei o envelope: ele cria/reescreve arquivos e acrescenta log, contrariando “só leitura” (bin/sonda_leitura.sh:126–134, :216–218, :248–250).

Transcrição integral do dado fornecido; nova execução [nao verificado]:

```text
sonda_leitura: sonda_cascata.py [468fad0c8bbe7ab8] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

pais_com_nota_defesa1=5027
conversa__defesa1                                2246  amostra=[21443, 2194, 1228, 3992, 1612, 23712, 28085, 16587, 18106, 1305]
conversa__defesa1__ult30d                         150  amostra=[33987, 33994, 34858, 30592, 34861, 1956, 36926, 38028, 28656, 36963]
conversa__defesa1__ult7d                           23  amostra=[38028, 35846, 38503, 37220, 37761, 35138, 37259, 27229, 37424, 37432]
conversa__outra_via                               711  amostra=[9359, 134, 1520, 21447, 1229, 19677, 23623, 18706, 16787, 15221]
conversa__outra_via__ult30d                        54  amostra=[19779, 18213, 29228, 29508, 22837, 8419, 7093, 10504, 18852, 33038]
conversa__outra_via__ult7d                          3  amostra=[22837, 35769, 20312]
sem_conversa__defesa1                            2772  amostra=[16712, 22483, 21446, 24543, 36897, 32600, 33980, 35970, 21457, 25806]
sem_conversa__defesa1__ult30d                     893  amostra=[36897, 32600, 33980, 35970, 33988, 36707, 32601, 32942, 32356, 32602]
sem_conversa__defesa1__ult7d                      144  amostra=[38669, 38102, 38107, 38095, 37644, 37645, 37898, 37650, 38342, 37655]
sem_conversa__outra_via                          1003  amostra=[1230, 18087, 21450, 1231, 19672, 9398, 24546, 21453, 371, 19684]
sem_conversa__outra_via__ult30d                   153  amostra=[29784, 34905, 34050, 33358, 33386, 34668, 34652, 34535, 34031, 34639]
sem_conversa__outra_via__ult7d                     15  amostra=[37207, 37272, 37683, 37420, 37802, 37476, 37481, 37479, 37862, 37870]
total_encerrados                                 6732  amostra=[]
total_erros=0
```

Conferência aritmética do dado: `2246 + 711 + 2772 + 1003 = 6732`; `total_erros=0` informado. Origem em produção [nao verificado].

## 13/11 01:27 · sessao 8f2519eeb11c · O168 · sonda · origem=Code

Não executei o envelope: ele escreve arquivos, contrariando a regra de somente leitura (`bin/sonda_leitura.sh:132`, `bin/sonda_leitura.sh:134`, `bin/sonda_leitura.sh:217`, `bin/sonda_leitura.sh:250`).

No dado fornecido, as classes somam **7058**, igual ao total, e consta **total_erros=0**; execução independente [nao verificado].

Reprodução integral da saída fornecida [nao verificado]:

```text
sonda_leitura: sonda_pais_conversa.py [8e530f330dd1672f] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

aberto__conversa_TODA_validada                               50  amostra=[20239, 19790, 18257, 21649, 20019, 3893, 28437, 4757, 17616, 15394]
aberto__conversa_parcial_validada                            87  amostra=[18157, 18180, 19823, 28962, 21643, 20045, 18566, 30524, 20332, 21946]
aberto__conversa_sem_validacao                               57  amostra=[38217, 25887, 32489, 31901, 20591, 23043, 23034, 38701, 37022, 36871]
aberto__sem_conversa                                         97  amostra=[19962, 17167, 28210, 25895, 22832, 22835, 34689, 18059, 33347, 20584]
cancelado__conversa_TODA_validada                             1  amostra=[2332]
cancelado__sem_conversa                                       1  amostra=[1725]
em_analise__conversa_TODA_validada                            3  amostra=[15622, 22166, 2074]
em_analise__conversa_parcial_validada                        28  amostra=[16640, 17917, 6295, 24943, 10388, 15712, 33492, 17211, 9785, 10155]
em_analise__conversa_sem_validacao                            2  amostra=[23120, 32735]
fechado__conversa_TODA_validada                             562  amostra=[1520, 1229, 3992, 16767, 16782, 14387, 11861, 15967, 15448, 13642]
fechado__conversa_parcial_validada                           55  amostra=[18706, 14017, 13663, 16960, 16989, 19880, 2988, 17165, 18580, 20245]
fechado__conversa_sem_validacao                              30  amostra=[23623, 18713, 17160, 27368, 17839, 29508, 17614, 23060, 23033, 18088]
fechado__sem_conversa                                       447  amostra=[1231, 24546, 21457, 1329, 21470, 1389, 21500, 1234, 15507, 18130]
resolvido__conversa_TODA_validada                          2131  amostra=[21443, 134, 2194, 1228, 19677, 1612, 23712, 28085, 16587, 18106]
resolvido__conversa_parcial_validada                        119  amostra=[9359, 16787, 15221, 14292, 18150, 1577, 19808, 16970, 8353, 19860]
resolvido__conversa_sem_validacao                            60  amostra=[21447, 25811, 20799, 18210, 25862, 19974, 34861, 18484, 17192, 18568]
resolvido__sem_conversa                                    3328  amostra=[16712, 22483, 21446, 24543, 36897, 1230, 18087, 32600, 21450, 33980]
total                                                      7058  amostra=[]
total_erros=0
```

## 13/11 00:49 · sessao eb2adfbaf90a · E3-FORMA-SQL · leitura · origem=Code

```python
# SOMENTE TEXTO; não executado, compilado ou validado em execução. [nao verificado]
# Nenhum arquivo foi criado, editado ou apagado nesta revisão. [nao verificado]
#
# Executar o MESMO texto na BASE e na COPIA, mudando somente PARTE.
# Compatibilidade com a BASE e sombra imutável: [nao verificado].
#
# Particionamento: decisão expressa no enunciado.
# Pergunta tem colaborador próprio: app/chamados/models.py:2454.
# Disputa recebe todas as perguntas: app/chamados/models.py:1859.
#
# Hash novo: Merkle + multiconjunto combinável em faixas fixas de IDs.
# Não é o SHA256 das listas da v1; comparar BASE/COPIA com este mesmo texto.
# Implementação proposta e propriedades em execução: [nao verificado].
#
# O acumulador aceita IDs fora de ordem e repetidos; necessário para:
# app/chamados/relatorios_disputa.py:60;
# app/chamados/services/fio.py:206.
#
# vigia.ramo preserva a guarda da v1: _listar retorna None e não é registrado.
# e3_diff_leitores_v1.py:369; app/chamados/services/vigia_fio.py:629.
#
# PRONTO 7: descrição literal do aval não fornecida. [nao verificado]
# São emitidas categorias reais, respostas sem veredito, booleanos C7,
# classes da tarefa e as aberturas nativas do vigia:
# app/chamados/relatorios_disputa.py:6;
# app/chamados/models.py:1847;
# app/chamados/models.py:1854;
# app/chamados/services/vigia_fio.py:575.
#
# Paridade externa:
# agregado: paridade_face_sql_sem_texto::face
# partes:   paridade_face_sql_sem_texto::juiz
# Confrontar os acumuladores dos mesmos blocos após somar as partes.
# Não se emite um booleano de paridade local sem a face global.
# Decisão expressa no enunciado; juiz/face:
# app/chamados/juizes.py:1515; app/chamados/juizes.py:1531.

PARTE = 0                  # int entre 0 e N_PARTES-1, ou "agregado"
N_PARTES = 12
BLOCO = 1000
TETO_MB = 2048
TETO_DURO_MB = 2560
TRACO = False

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
import os
import re
import signal
import threading
import time
import uuid

from unittest.mock import patch
from zoneinfo import ZoneInfo

FIXO = dt.datetime.fromisoformat("2026-11-12T23:48:00+00:00")
ZONA = ZoneInfo("America/Sao_Paulo")
HOJE = FIXO.astimezone(ZONA).date()
DIA = dt.timedelta(days=1)
DATA_REAL = dt.date
INSTANTE_REAL = dt.datetime

Model = QuerySet = None
transaction = J = None

ASSINATURAS = {}
AUSENTES = set()
REGISTROS = {}
MANIFESTOS = {}
TEMPOS = {}
UNIVERSO = {}
INCANONIZAVEL = collections.Counter()
SQL_ERROS = collections.Counter()
OBS_ERROS = collections.Counter()
SEQUENCIAS = collections.Counter()
ATUAL = None
ALCANCADO_FIM = False
ROLLBACKS = 0

MODULO_HASH = 1 << 256
MASCARA_HASH = MODULO_HASH - 1

MEMORIA = {
    "pico_mb": 0.0,
    "teto_mb": TETO_MB,
    "teto_duro_mb": TETO_DURO_MB,
}
MEMORIA_LOCK = threading.Lock()
PARAR_VIGIA = threading.Event()

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
validada_sem_gravar devolvida_ao_colaborador materializacao_recusada
veredito_sem_lavra_vigente validada_sem_efeito efeito_sem_lampada
encerrada_pelo_juiz silencio_decidido absorvida_aprovada
absorcao_rejeitada absorcao_aguarda_documento absorcao_pendente
perguntar responder validar materializar rejulgar decidir_ausencia
entregar_documento emissor cartorio materializacao
furo cobrado discordante indefinida concordante
entra bloqueado modulo_proprio sim nao
regularizar_batida admin-cadastro admin-batida
sem_pergunta_viva pergunta_viva_sob_chamado_terminal
pergunta_viva_sem_ancora pergunta_viva_fora_do_app outra
dia_na_lista_dono_batida dia_na_lista_dono_cadastro
dia_na_lista_dono_estrutura dia_na_lista_dono_nenhum
reabrir_credor registrar_decisao
""".split())
TOKENS.update({
    "", "nenhuma porta", "fora da lista do admin", "lista sem resposta",
})

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
colabs furo dias_ferias_com_batida
dono_do_dia ec_do_dia posto_do_dia pai_limbo
saldo codigo_tipo chamados disputas fica_resposta_sem_veredito
fica_pergunta_viva_fora fica_resgatado perguntas_com_resposta_sem_veredito
disputas_com_pai_vivo_fora sem_dia fica_fora_da_tranca decidir_pelo_dp
vivas_apesar_da_tranca perguntas_lidas_trancadas aplicadas [nao verificado]
ok pendentes prazo_ate canal erros
alvo abrir fechar mantidas alvo_dias estrutura por_dono
abrir_chaves fechar_chaves n chave
q_respondida_sem_texto q_respondida_com_texto
resultado ordem faltam saida_sha256 bytes
categoria pendentes_com_tranca pendentes_ignorando_tranca
""".split())

PRIVADOS = {
    "nome", "nome_completo", "cpf", "username", "email",
    "codigo", "codigo_dominio", "telefone", "matricula",
}

CAMPOS_CLASSE = (
    "rota", "fio", "razao", "estado", "status", "grupo", "motivo",
    "causa", "detalhe", "papel", "dono", "acao", "de", "categoria",
    "pode_fechar", "todas_respondidas", "pode_solicitar_correcao",
    "resposta_enviada", "tem_pergunta_viva", "nao_sei",
    "sem_canal", "vencido_cobranca", "sem_canal_push",
    "candidato", "creditada_respondida", "creditada_pendente",
    "pertence",
)


class DesfazerSonda(Exception):
    pass


class FalhaDaSonda(BaseException):
    pass


class EscritaBloqueada(FalhaDaSonda):
    pass


class TetoDeMemoria(FalhaDaSonda):
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


def rss_mb():
    with open("/proc/self/statm", "r") as arquivo:
        paginas = int(arquivo.read().split()[1])
    return paginas * os.sysconf("SC_PAGE_SIZE") / 1048576.0


def atualizar_pico(atual):
    with MEMORIA_LOCK:
        MEMORIA["pico_mb"] = max(MEMORIA["pico_mb"], atual)


def memoria_saida():
    with MEMORIA_LOCK:
        return {
            **MEMORIA,
            "pico_mb": round(MEMORIA["pico_mb"], 1),
        }


def medir_memoria():
    atual = rss_mb()
    atualizar_pico(atual)
    if atual > TETO_MB:
        raise TetoDeMemoria()
    return atual


def traco(nome, unidade, etapa):
    if TRACO:
        os.write(2, (
            "TRACO %s %s rss=%.1f etapa=%s\n"
            % (nome, serial(unidade), rss_mb(), etapa)
        ).encode("ascii"))


def iniciar_vigia_duro():
    def laco():
        while not PARAR_VIGIA.is_set():
            try:
                atual = rss_mb()
                atualizar_pico(atual)
                if atual > TETO_DURO_MB:
                    os.write(1, (
                        "===TETO_DURO_MB_EXCEDIDO===\n"
                        + serial({
                            "fatal": "TetoDuroDeMemoria",
                            "execucao_completa": False,
                            "rss_mb": round(atual, 1),
                            **memoria_saida(),
                        })
                        + "\n"
                    ).encode("ascii"))
                    os.kill(os.getpid(), signal.SIGKILL)
            except Exception:
                os.write(1, b"===FALHA_VIGIA_DURO===\n")
                os.kill(os.getpid(), signal.SIGKILL)
            PARAR_VIGIA.wait(0.25)

    threading.Thread(
        target=laco, name="e3-teto-rss", daemon=True,
    ).start()


def sem_forma(valor, prefixo="sem_forma"):
    tipo = type(valor)
    chave = tipo.__module__ + "." + tipo.__qualname__
    INCANONIZAVEL[prefixo + ":" + chave] += 1
    return ("incanonizavel", tipo.__module__, tipo.__qualname__)


def canonico(valor, ativos=None, memo=None):
    # Memo com referência forte ao original: cura do DAG do anexo.
    # Forma inteira usa tuplas; digest_forma nunca serializa o DAG expandido.
    # Implementação proposta: [nao verificado].
    if ativos is None:
        ativos = set()
    if memo is None:
        memo = {}

    if valor is None or isinstance(valor, (bool, int)):
        return valor
    if isinstance(valor, str):
        if valor in TOKENS or re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return valor
        return ("str_sha256", len(valor), sha(valor))
    if isinstance(valor, float):
        return ("float", valor.hex())

    identidade = id(valor)
    if identidade in ativos:
        return sem_forma(valor, "ciclo")
    if identidade in memo:
        return memo[identidade][1]

    ativos.add(identidade)
    try:
        try:
            if isinstance(valor, enum.Enum):
                forma = canonico(valor.value, ativos, memo)
            elif isinstance(valor, decimal.Decimal):
                forma = ("decimal", str(valor))
            elif isinstance(valor, (INSTANTE_REAL, DATA_REAL, dt.time)):
                forma = ("iso", valor.isoformat())
            elif isinstance(valor, dt.timedelta):
                forma = (
                    "duracao", valor.days, valor.seconds, valor.microseconds,
                )
            elif isinstance(valor, uuid.UUID):
                forma = ("uuid", str(valor))
            elif isinstance(valor, bytes):
                forma = (
                    "bytes_sha256", len(valor),
                    hashlib.sha256(valor).hexdigest(),
                )
            elif Model is not None and isinstance(valor, Model):
                # Mesma projeção de modelo da E3 v1: somente identidade.
                # e3_diff_leitores_v1.py:265.
                forma = ("model", valor._meta.label_lower, valor.pk)
            elif QuerySet is not None and isinstance(valor, QuerySet):
                itens = [canonico(x, ativos, memo) for x in valor]
                ordem = {}
                forma = (
                    "queryset",
                    tuple(sorted(
                        itens, key=lambda x: digest_forma(x, ordem),
                    )),
                )
            elif isinstance(valor, collections.abc.Mapping):
                itens = []
                for chave, conteudo in valor.items():
                    if isinstance(chave, str) and chave.lower() in PRIVADOS:
                        continue
                    k = (
                        chave
                        if isinstance(chave, str) and chave in CHAVES
                        else canonico(chave, ativos, memo)
                    )
                    itens.append((k, canonico(conteudo, ativos, memo)))
                ordem = {}
                forma = (
                    "dict",
                    tuple(sorted(
                        itens,
                        key=lambda x: digest_forma(x[0], ordem),
                    )),
                )
            elif isinstance(valor, (set, frozenset)):
                itens = [canonico(x, ativos, memo) for x in valor]
                ordem = {}
                forma = (
                    "set",
                    tuple(sorted(
                        itens, key=lambda x: digest_forma(x, ordem),
                    )),
                )
            elif isinstance(valor, (list, tuple)):
                forma = (
                    "list" if isinstance(valor, list) else "tuple",
                    tuple(canonico(x, ativos, memo) for x in valor),
                )
            elif dataclasses.is_dataclass(valor) and not isinstance(valor, type):
                forma = canonico({
                    f.name: getattr(valor, f.name)
                    for f in dataclasses.fields(valor)
                    if f.name.lower() not in PRIVADOS
                }, ativos, memo)
            else:
                forma = sem_forma(valor)
        except Exception:
            forma = sem_forma(valor, "erro_canonizador")
    finally:
        ativos.remove(identidade)

    memo[identidade] = (valor, forma)
    return forma


def digest_forma(forma, memo=None):
    if memo is None:
        memo = {}
    if isinstance(forma, tuple):
        chave = id(forma)
        if chave in memo:
            return memo[chave][1]
        h = hashlib.sha256(b"T%d(" % len(forma))
        for item in forma:
            h.update(digest_forma(item, memo).encode("ascii"))
            h.update(b",")
        h.update(b")")
        digest = h.hexdigest()
        memo[chave] = (forma, digest)
        return digest
    return hashlib.sha256(
        ("P" + repr(forma)).encode("utf-8")
    ).hexdigest()


def bytes_forma(forma, memo=None):
    # Comprimento JSON expandido calculado sem construir esse JSON.
    # Implementação proposta: [nao verificado].
    if memo is None:
        memo = {}
    if not isinstance(forma, tuple):
        return len(serial(forma).encode("utf-8"))
    chave = id(forma)
    if chave in memo:
        return memo[chave][1]
    tamanho = (
        2 + max(0, len(forma) - 1)
        + sum(bytes_forma(x, memo) for x in forma)
    )
    memo[chave] = (forma, tamanho)
    return tamanho


def primeiro_id(unidade):
    if type(unidade) is int:
        return unidade
    if isinstance(unidade, (list, tuple)):
        for item in unidade:
            ident = primeiro_id(item)
            if ident is not None:
                return ident
    return None


def rotulo_classe(valor):
    if valor is None:
        return "None"
    if type(valor) is bool:
        return "True" if valor else "False"
    if isinstance(valor, str):
        return valor if valor in TOKENS else "FORA_DO_VOCABULARIO"
    if isinstance(valor, enum.Enum):
        return rotulo_classe(valor.value)
    return "TIPO:" + type(valor).__name__


class Registro:
    # Não conserva eventos/linhas: somente acumuladores por faixa de IDs,
    # bitmap de IDs da faixa e contadores de classes.
    # Algoritmo proposto; validação em execução: [nao verificado].
    def __init__(self):
        self.chamadas = 0
        self.projecoes = 0
        self.excecoes = collections.Counter()
        self.hist = collections.Counter()
        self.classes = collections.Counter()
        self.metricas = collections.Counter()
        self.blocos = {}

    def contar_classes(self, valor):
        self.hist[rotulo_classe(valor)] += 1
        if not isinstance(valor, collections.abc.Mapping):
            return

        def campos(d, prefixo):
            for campo in CAMPOS_CLASSE:
                if campo in d:
                    v = d[campo]
                    if v is None or isinstance(v, (bool, str, enum.Enum)):
                        self.classes[
                            prefixo + campo + ":" + rotulo_classe(v)
                        ] += 1

        campos(valor, "")
        for instancia in valor.get("instancias") or ():
            if not isinstance(instancia, collections.abc.Mapping):
                continue
            campos(instancia, "instancia.")
            for acao in instancia.get("acoes") or ():
                if isinstance(acao, collections.abc.Mapping):
                    campos(acao, "acao.")

    def adicionar(self, ident, contexto, valor, exc=None, projecao=False):
        if projecao:
            self.projecoes += 1
        else:
            self.chamadas += 1
        if exc:
            self.excecoes[exc] += 1
        self.contar_classes(valor)

        evento = (
            canonico(contexto),
            ("forma_sha256", digest_forma(canonico(valor))),
        )
        digest = digest_forma(evento)
        numero = int(digest, 16)

        if ident is not None and (type(ident) is not int or ident < 0):
            raise FalhaDaSonda()
        faixa = None if ident is None else ident // BLOCO
        bloco = self.blocos.setdefault(faixa, {
            "n": 0, "soma": 0, "quadrados": 0, "xor": 0, "bitmap": 0,
        })
        bloco["n"] += 1
        bloco["soma"] = (bloco["soma"] + numero) & MASCARA_HASH
        bloco["quadrados"] = (
            bloco["quadrados"] + numero * numero
        ) & MASCARA_HASH
        bloco["xor"] ^= numero
        if ident is not None:
            bloco["bitmap"] |= 1 << (ident % BLOCO)

    def resultado(self):
        blocos = []
        for faixa in sorted(
            self.blocos, key=lambda x: (-1 if x is None else x),
        ):
            b = self.blocos[faixa]
            atomos = {
                "n": b["n"],
                "soma256": "%064x" % b["soma"],
                "quadrados256": "%064x" % b["quadrados"],
                "xor256": "%064x" % b["xor"],
            }
            blocos.append({
                "faixa": faixa,
                "id_de": None if faixa is None else faixa * BLOCO,
                "id_ate": (
                    None if faixa is None else (faixa + 1) * BLOCO - 1
                ),
                "ids_distintos": b["bitmap"].bit_count(),
                "ids_bitmap_hex": format(b["bitmap"], "x"),
                **atomos,
                "sha256": sha(atomos),
            })
        return {
            "universo_registros": self.chamadas + self.projecoes,
            "chamadas_ou_observacoes": self.chamadas,
            "projecoes_de_resultado": self.projecoes,
            "excecoes_por_classe": dict(sorted(self.excecoes.items())),
            "histograma": dict(sorted(self.hist.items())),
            "classes": dict(sorted(self.classes.items())),
            "metricas": dict(sorted(self.metricas.items())),
            "blocos": blocos,
        }


def registro(nome):
    return REGISTROS.setdefault(nome, Registro())


def guardar(
    

[truncado em 20000 de 82527 caracteres]

## 13/11 00:47 · sessao fd03853988f3 · BUG-CADASTRO-C5E · censo · origem=Code

As referências aos anexos contam desde a primeira linha de cada bloco, incluindo linhas em branco.

| Achado | Letra | Trecho literal .q | Trecho literal .r | Trecho triagem e motivo |
|---|---|---|---|---|
| **4/6 — duas leituras no signal; ausência no universo** | **b** | “Ache um ato que leia duas vezes ou deixe um destino ler o relogio por conta propria.” (`pv1.q:10–11`) | “Na edição, `existente.save()` pode alcançar os signals: o de vigência lê **duas vezes**; o de âncora lê uma vez.” (`pv1.r:55`) | “FORA (aval: PROIBIDO ampliar aos ~30 interiores): […] escala/signals.py:67/119” (`triagem_v1.txt:11`). **Descarte por escopo do aval.** A dupla leitura foi vista; a omissão específica de `UNIVERSO_RELOGIO` não foi denunciada nesse trecho. (`pv1.r:55`; `app/ponto/tests/test_cadastro_futuro_c5e.py:326–332`) |
| **1 — relógios adicionais no Salvar e destinos** | **b** | “se o MESMO valor desce a todas as portas e a todo efeito do ato” (`pv1.q:9–10`) | “Ele relê `localdate()` e decide os dias julgáveis com esse novo valor”; “`_copiar_folga_padrao` também lê `date.today()` no meio do Salvar.” (`pv1.r:53`; `pv1.r:56`) | “FORA […] rabo _o_chao_do_dia_mudou:464, pos_vinculo:56, escala/signals.py:67/119, _copiar_folga_padrao:163 […] vinculo.py:748 -> RELOGIO-NA-BORDA.” (`triagem_v1.txt:11–12`). **Descarte por proibição de ampliar a cura.** |
| **6 — cron ainda regenera HOJE** | **b** | “Em cada ato que chega a essas portas (view de Salvar, commands […]), onde o relogio e lido” e “Quem LE e quem ESCREVE cada valor que o diff passa a mover” (`pv1.q:8–9`; `pv1.q:14`) | “Uma leitura `date.today()`; mesmo valor para vigia, regeneração de hoje, geração e lavratura.” (`pv1.r:44`) | “DESCARTADO 3 (c5): continua descartado (Codex item 7 concorda).” (`triagem_c5d.txt:6`). O descarte remete à defesa de conservar o alcance de hoje pela juíza, registrada no item 7 de `preverif_c5d.r`; sua pergunta original não foi fornecida **[nao verificado]**. |
| **7 — DNA órfão após exclusão no snapshot** | **e** | O recorte explícito é “o `hoje` e o que ele decide” (`pv1.q:14`). A volta 3 acrescenta “O que mais esta errado na obra inteira (8dc931da9..4b34ed21a)” (`pv3.q:9`), sem nomear exclusão, `SET_NULL` ou integridade do DNA. | “o snapshot mantém vínculos antes de células, com selo exigindo `E → C`” (`pv3.r:67`). Esse trecho examina travas; o problema de DNA após exclusão não é tratado. | Descarte específico desse problema: **[nao verificado]**. Classifico **não perguntado pelo escopo demonstrável**: snapshot aparece como relógio/travas, sem pergunta específica sobre os resíduos do `delete()`. (`pv1.q:14–16`; `pv3.r:67–68`; `app/ponto/tests/test_cadastro_futuro_c5e.py:344`) |
| **8 — comprovação do D7 e reversão em logs** | **c** | “inclusivo/exclusivo em `d > hoje` x `d >= hoje`” (`pv1.q:16`) | “Já a célula ausente de vínculo inativo só nasce em `d > hoje`.” (`pv1.r:95`); “não houve acesso a banco/log nem execução da suíte com banco” (`pv3.r:73`) | Medição anterior/posterior e prova de reversão nas triagens fornecidas: **[nao verificado]**. A comprovação solicitada depende dos dados/logs ausentes. (`pv3.r:73`) | [nao verificado]

A conferência da cópia confirma os sítios citados:

- **4/6 e 1:** leituras em `app/escala/signals.py:67–68` e `:119`; `app/colaboradores/services/vinculo.py:163`, `:194`, `:740` e `:752`; `app/escala/folgas.py:280`; `app/ponto/portas/celula.py:464`. O universo da catraca omite `escala/signals.py`. (`app/ponto/tests/test_cadastro_futuro_c5e.py:326–332`)
- **6:** `_regenerar_hoje_existentes` chama a porta para hoje e é executado pelo command. (`app/ponto/management/commands/gerar_celulas.py:303`, `:323–325`, `:347`)
- **7:** o snapshot executa `remover.delete()` e depois percorre os vínculos do snapshot; a geradora usa `SET_NULL`, e o DNA é JSON separado. (`app/colaboradores/services/vinculo.py:1029`, `:1034–1035`; `app/escala/models.py:1693`, `:1697`). O vigia incrementa o contador de divergência. (`app/ponto/services/vigia_cadastro_futuro.py:60–61`)
- **8:** a seleção D7 existe, com guarda que exclui o inativo em `d <= hoje`. Isso sustenta a leitura estática; resultado em produção e reversão permanecem **[nao verificado]**. (`app/ponto/portas/celula.py:883`, `:908–910`)

Por volta, **pv1** pediu atos, chamadores e leitores/escritores; **pv2** concentrou-se no delta e proibiu ampliar aos interiores; **pv3** declarou delta apenas de teste, acrescentando revisão da obra inteira com o mesmo veto. (`pv1.q:8–16`; `pv2.q:4–8`; `pv3.q:2`, `:9`). A existência exata nos hashes anteriores e o conteúdo dos avais/diffs apenas mencionados nos anexos permanecem **[nao verificado]**. [nao verificado]

**Contagem: a=0 | b=3 | c=1 | d=0 | e=1.**

## 13/11 00:45 · sessao 4ae7d397b4aa · - · - · origem=Ronald (tmux)

Minha classificação independente: **7 → (e); 8 → (c)**, com a ressalva de escopo abaixo.

| Achado | Sustentação literal dos anexos | Conferência na cópia |
|---|---|---|
| **7 — DNA órfão após snapshot → (e)** | Nenhuma pergunta fornecida nomeia exclusão de EC, `SET_NULL` ou DNA órfão. O pedido mais próximo é `pv1.q`, item 4: “cada valor que o diff passa a mover (o `hoje` e o que ele decide: celula futura x passada [...]”. A resposta trata outra propriedade do snapshot: `pv3.r`: “o snapshot mantém vínculos antes de células, com selo exigindo `E → C`”. Nenhuma triagem fornecida descarta especificamente o problema de DNA órfão. A inferência de **(e)** distingue escopo temporal do escopo de integridade do DNA; os diffs e avais originais anexados às perguntas não estão disponíveis no material fornecido, portanto alcance integral histórico **[nao verificado]**. | O snapshot seleciona ECs a remover, recusa referências humanas e chama `remover.delete()` em `app/colaboradores/services/vinculo.py:1022`, `:1027`, `:1029`; depois só percorre ECs remanescentes do snapshot em `:1034`. A FK usa `SET_NULL`, enquanto DNA é JSON independente: `app/escala/models.py:1692`, `:1693`, `:1697`. O vigia conta divergência de ID em `app/ponto/services/vigia_cadastro_futuro.py:60`. A função integra o **censo de relógio**, especificamente, em `app/ponto/tests/test_cadastro_futuro_c5e.py:344`. |
| **8 — prova de não alteração/reversão D7 → (c)** | `pv1.q`, item 4: “inclusivo/exclusivo em `d > hoje` x `d >= hoje`”. `pv1.r`, item 4: “Já a célula ausente de vínculo inativo só nasce em `d > hoje`.” `pv3.r`, limites: “não houve acesso a banco/log nem execução da suíte com banco; implantação e pouso efetivos permanecem [nao verificado]”. Nenhuma triagem fornecida apresenta a comparação de dados ou a reversão solicitada pelo opus. | O D7 e sua mudança de universo estão em `app/ponto/portas/celula.py:877` e `:883`; o filtro impede entrada dos inativos em `d <= hoje` em `:908`. Isso permite conferir a intenção e o comportamento estático dessa guarda; não comprova resultado de produção ou reversão em `logs/` **[nao verificado]**. |

Para **7**, eu evitaria **(a)** sustentado somente por “O que mais esta errado na obra inteira”: esse trecho genérico não demonstra que o tema de DNA órfão estava presente. Também não há base para **(b)**: as triagens descartam relógios interiores, sem descartar esse problema de integridade. **[nao verificado]**

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

_4488 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
