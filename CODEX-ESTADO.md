# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 20:31** · publicado em **15/11/2026 20:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 20:31 · sessao 7690ac4ac86d · E4C-F1-NOVA · verificador · origem=Code

1) **NAO PROCEDE — as premissas de código conferem.**

- `_recusa_par_ilegal` compara o par atual com o carregado e permite o legado inalterado: `if not self._state.adding and _par == getattr(self, '_par_carregado', None): return` (`app/chamados/models.py:3562-3564`). O teste também permite `passivo.save()` e alteração de outro campo (`app/chamados/tests/test_e3_10_cura_no_escritor.py:190-196`).
- Os três predicados existem: retenção usa `I OU T` (`app/chamados/models.py:1905-1909`); fila usa `q_respondida_com_texto()` (`app/chamados/juizes.py:1725`), cuja expressão é `I E T` (`app/chamados/juizes.py:1559`); property usa somente `respondida_em is not None` (`app/chamados/models.py:3641`).
- Ressuscitação e simulação limpam somente instante: `respondida_em=None` e `_q.respondida_em = None` (`app/chamados/models.py:1613`, `app/chamados/models.py:1555`). O lote realmente dispensa conferir texto quando recebe instante nulo (`app/chamados/models.py:3349-3352`).
- O contrato atual é unilateral e absolve explicitamente só instante: `self.assertNotIn(GATILHO, e3, 'acusaria carimbo-sem-texto, que e legitimo')` (`app/chamados/tests/test_contract_par_resposta.py:113`).

Universo, contagens e composição 141 = 111 + 30 provêm exclusivamente do dado raspado; confirmação independente em produção, pks efetivos e conteúdo da c9: **[nao verificado]**.

2) **NAO PROCEDE — não identifiquei colisão demonstrada.**

A L-130 já estabelece unidade pergunta, juiz único, recusa dos dois pares e preservação das materializadas: “validadas/materializadas ficam” (`app/docs/LEIS.md:151`). Portanto, excluir as 30 materializadas da futura zeragem respeita esse literal; sua quantidade vem somente do anexo **[nao verificado]**.

A exceção de competência confere: “volta para a fila, com a resposta que tinha” (`app/chamados/models.py:3417-3418`). Fantasma apenas executa `self.descarimbar()` (`app/chamados/models.py:3395-3397`); essa porta salva somente via, instante e autor da materialização (`app/chamados/models.py:3291-3295`).

Registrar a frase em CORTES antes do pouso atende à trava que a exige (`CLAUDE.md:74-76`). Quanto ao item A, relógio e seleção de respondidas ocupam trechos distintos de `placar_aceite`: `agora = _tz.now()` versus filtro por `respondida_em` (`app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:333-345`). Sobreposição concreta dos futuros patches: **[nao verificado]**.

3) **NAO PROCEDE — não encontrei condição literalmente inexequível.**

Já existe `REABERTURA`, incluída em `ATOS_COM_FOTO` (`app/chamados/models.py:4001`, `app/chamados/models.py:4012`). O escritor existente prepara zeragem dos dois campos e registra texto/instante anteriores dentro do mesmo atomic, antes do lote (`app/chamados/services/disputa_emissao.py:1609`, `app/chamados/services/disputa_emissao.py:1621-1648`).

A simulação pode acompanhar essa zeragem preservando seu universo atual, que já exige texto NULL ou vazio (`app/chamados/models.py:1530-1533`). As provas futuras de zero mudança em B1 e zero efeito entregue em B2 ainda são **[nao verificado]**; condição que falhar não autoriza pouso: “aval condicional que nao fecha na condicao = PAROU” (`CLAUDE.md:37-38`).

4) **NAO PROCEDE — nenhum alcance adicional que exija `!` ficou demonstrado.**

Há efeito indireto possível sobre encerramento: o CSF consulta `sem_veredito_entre` e retorna `not perguntas_que_seguram_csf(ch)` (`app/ponto/supra_juiz.py:573-588`). Isso justifica medir os efeitos de escritores e crons; o resultado efetivo é **[nao verificado]**.

O gravado exportado e `ponto/motor_calculo_v2.py` continuam exigindo autorização própria (`CLAUDE.md:265-268`). Se surgir dinheiro em aberta, a tabela não substitui reversão anterior, exportadas intactas com hash e prova de frota posterior (`CLAUDE.md:406-410`). Movimento financeiro adicional causado por B: **[nao verificado]**.

Correspondência desta cópia com HEAD/main `e84a167f4`: **[nao verificado]**.

VEREDITO: TUDO NAO PROCEDE

## 15/11 20:31 · sessao 53a8218927c2 · E3-RESTO-R3-C17 · verificador · origem=Code

AVAL-DEVOLVIDO — ITEM A, por inexatidão de premissa na pergunta 1.

1) **PROCEDE — a descrição do substituto c14 não confere.** A premissa “dict de 3 chaves” vale para `app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`: `Mock(return_value=dict(cego=0, calados=0, dias_sem_leitura_sistema=n))`. Entretanto, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:315-317` faz `d, _txt = vigia_censo(self.MISTO)` e `censo=lambda emps, hoje: d`. Esse helper executa o censo real e devolve seu resultado integral: `app/chamados/tests/test_e3_r3_c6_sem_banco.py:253-254`, `d = funcao(..., 'censo')(...)` / `return d, ...`. As nove chaves vêm de `app/folha/vigia_retido_calado.py:61-62` e `:89`. A descrição precisa distinguir os dois substitutos.

   As premissas centrais conferem: universo iniciado em retratos, `qs = RetratoFolha.objects.all()` (`app/folha/vigia_retido_calado.py:46`); C5 recebendo o parâmetro original, `colabs_nunca_bateu(empresas, hoje)` (`:60`); `None` abrangendo todas as empresas, `Empresa.objects.all() if empresas is None` (`app/escala/services/cadastro_realidade.py:163`); alarme estrutural atual, `ALARMA_SEMPRE = ('cego',)` (`app/folha/management/commands/vigiar_retido_calado.py:16`). Os relógios excluídos também existem: `gerado_em = timezone.now()` (`app/relatorios/pdf_base.py:133`) e `agora = _tz.now()` / `hoje = _tz.localdate()` (`app/chamados/services/verbos.py:286-287`).

2) **NAO PROCEDE.** O papel proposto permanece compatível com “CRON DE VARREDURA E VIGIA, NUNCA JUIZ” (`app/docs/LEIS.md:94`, L-060) e “so alarma, NUNCA juiz” (`CLAUDE.md:172`). O balde vigente está registrado como “sem acao disponivel” (`app/docs/CORTES.md:437`, LU-05), correspondendo ao filtro `dono_da_linha(l, hoje=hoje) == 'sem_acao_disponivel'` (`app/folha/vigia_retido_calado.py:66`). Os textos anteriores completos de 4.2/4.3 e ORDEM-VIVA-1511-NOITE: **[nao verificado]**.

3) **NAO PROCEDE.** Os comportamentos novos são construção autorizada, não requisitos já satisfeitos: hoje `dataset_furos(..., agora=None)` permite omissão (`app/relatorios/furos.py:103`) e transporta `agora` ao D4 (`:123-124`). Também existe o encaixe para eliminar o relógio interno do limbo: a borda captura `_agora` e deriva `hoje` (`app/relatorios/views.py:1519-1520`), mas ainda chama `escalas_em_limbo(empresa)` (`:1554`). A realização futura da medição de produção e sua publicação: **[nao verificado]**.

4) **NAO PROCEDE.** O caminho examinado consulta e conta, terminando em `return out` (`app/folha/vigia_retido_calado.py:44-90`); o comando declara “NAO TEM `--apply`” (`app/folha/management/commands/vigiar_retido_calado.py:9`) e alarma por `raise SystemExit(2)` (`:41-42`). Isso não demonstra escrita no gravado exportado nem intervenção no motor, ações que exigem autorização específica (`CLAUDE.md:142`, `:265-268`). Pares efetivamente ausentes e efeitos concretos em produção: **[nao verificado]**.

VEREDITO: PROCEDE em 1

## 15/11 20:30 · sessao b0949139420b · - · - · origem=Ronald (tmux)

Não identifiquei procedente concreto nas premissas dos escritores ou na executabilidade de B1.

- **Comparação com o par carregado confere:** `app/chamados/models.py:3563-3564`: `if not self._state.adding and _par == getattr(self, '_par_carregado', None): return`. O `save` chama a guarda em `:3529`; `from_db` guarda a foto em `:3546`. O teste mantém explicitamente o legado intocado: `app/chamados/tests/test_e3_10_cura_no_escritor.py:192-193`: `passivo.save()` e `passivo.save(update_fields=['pergunta'])`.
- **Guarda atual é unilateral, como descrito:** nascimento chama `recusa_respondida_sem_texto` em `app/chamados/models.py:2921-2923`; automática em `app/chamados/services/disputa_emissao.py:1671`. A porta verifica apenas `respondida_sem_texto` em `app/chamados/services/fio.py:1601-1605`. O lote só consulta texto quando o novo instante é não nulo: `app/chamados/models.py:3349-3352`: `if outros['respondida_em'] is not None:`. Portanto a passagem `respondida_em=None` sem conferir texto existe.
- **Ressuscitação e simulação limpam só instante:** `app/chamados/models.py:1613`: `descarimbar_em_lote(... respondida_em=None)`; simulação `:1555`: `_q.respondida_em = None`. O universo atual já restringe texto a NULL/`''` em `:1533`, permitindo preservar o comportamento de seleção ao acrescentar zeragem do texto.
- **Exceções de reabertura conferem:** `app/chamados/models.py:3417-3421`: “com a resposta que tinha” e `self.descarimbar()`; fantasma faz apenas `self.descarimbar()` em `:3395-3397`; reabertura por resposta retorna `self.descarimbar(salvar=False)` em `:3412-3414`. `descarimbar` grava somente `via_resolucao`, `materializada_em`, `materializada_por` em `:3291-3295`.
- **Ato e escritor existentes comportam o pedido:** `app/chamados/models.py:4001` já define `REABERTURA`; `:4012` inclui esse ato entre os que admitem foto, com constraint em `:4040-4043`. `app/chamados/services/disputa_emissao.py:1609` zera ambos os campos; dentro do atomic de `:1621`, registra texto e instante anteriores em `:1631-1637`, antes do lote em `:1648`. Não precisa criar tipo de ato para cumprir esse pedido.
- **Contrato estático atual é unilateral:** `app/chamados/tests/test_contract_par_resposta.py:81`: `if GATILHO in escritos and EXIGIDO not in escritos`; `:113` absolve explicitamente escrita só de instante. A mudança solicitada deve adaptar esse contrato para as duas direções, sem confundir escrituras legítimas que completam o par com transições ilegais; por exemplo, `carimbar_resposta_pela_trilha` exige texto já existente em `app/chamados/services/disputa_emissao.py:1691` antes de escrever só instante em `:1695-1697`.

## 15/11 20:30 · sessao d94200c4f926 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- Premissas principais conferem: [app/relatorios/pdf_base.py:133] `gerado_em = timezone.now()`; [app/chamados/services/verbos.py:286] `agora = _tz.now()` e [:287] `hoje = _tz.localdate()`. As exclusões nomeadas do aval correspondem a relógios existentes.
- [app/relatorios/furos.py:103] tem `dataset_furos(... agora=None)`; [:123-124] transporta `agora` ao D4 e [:160-164] ao calendário. Exigir instante válido é mudança futura expressamente autorizada; não é premissa de comportamento já existente.
- [app/relatorios/furos.py:59] lê `timezone.localdate()` em `escalas_em_limbo`; [app/relatorios/views.py:1519-1520] já captura `_agora` e deriva `hoje`, mas [:1554] chama `escalas_em_limbo(empresa)` sem dia. Item 6 corresponde ao código.
- O selo perde multiplicidade: [app/chamados/tests/test_e3_r3_c15_sem_banco.py:162-163] retorna `sorted({n.func.attr ...})`, conjunto de nomes. [:182-184] reconhece somente atribuições cujo chamado seja atributo `now`. Item 8 cobre defeitos efetivos.
- Casos c16 citados existem: [app/chamados/tests/test_e3_r3_c16_sem_banco.py:78-113] cobre meia-noite, competência explícita velha, relavra parcial, falta parcial e vazio. [app/chamados/tests/e3_r3_c16_mutantes.py:26-29] contém alvos literais do laço atual; renovar alvo após refatorar é coerente.
- Substitutos sem banco citados conferem: c4:149-157, c5:96-103, c6:246-253, c14:341-350, c15:77-85 e c16:63-74 não fornecem consulta de `empresas_visiveis`. Negativos c5:243-245 são falhas de assinatura antes do corpo.

Única inexatidão literal encontrada no anexo (a): a descrição conjunta “substituto do command c11 ... e c14 ... (dict de 3 chaves)” só vale para c11. [app/chamados/tests/test_e3_r3_c11_sem_banco.py:275] contém `Mock(return_value=dict(cego=0, calados=0, dias_sem_leitura_sistema=n))`. Em c14, [:315] `d, _txt = vigia_censo(self.MISTO)` e [:317] `censo=lambda emps, hoje: d` devolvem o dict integral. A origem está em [app/chamados/tests/test_e3_r3_c14_sem_banco.py:23] (helper importado do c6) e [app/chamados/tests/test_e3_r3_c6_sem_banco.py:253-254] (executa `censo` real). As nove chaves estão em [app/folha/vigia_retido_calado.py:61-62] e [:89]. Essa inexatidão não impede construir, mas contraria a descrição literal do anexo se a pergunta 1 exigir conferir também essa premissa auxiliar.

Não identifiquei outra condição técnica inexequível na ordem da construção nos trechos examinados; execução efetiva futura [nao verificado].

## 15/11 20:29 · sessao cabd9c751e18 · - · - · origem=Ronald (tmux)

Q2: **NAO PROCEDE**, quanto às leis e exceções examinadas.

- A L-130 já fixa `I ∧ T`, unidade pergunta, recusa das duas metades ilegais e preservação das validadas/materializadas: `app/docs/LEIS.md:151`, trecho: “validadas/materializadas ficam”.
- A exceção de competência confere: `app/chamados/models.py:3417-3421`, trecho: “volta para a fila, com a resposta que tinha”; executa `self.descarimbar()`.
- A fantasma só desfaz o carimbo: `app/chamados/models.py:3392-3397`, trecho: “So desfaz a via `fantasma` […] Gemea de `reabrir_por_competencia`”.
- Registrar a frase antes de pousar satisfaz a trava: `CLAUDE.md:74-76`, trecho: “exige a frase `corte Ronald: juiz <nome> nasce` no CORTES.md”.

Q4: **NAO PROCEDE** como acusação demonstrada pelos caminhos examinados; efeito financeiro real da mudança permanece **[nao verificado]**.

- O gate do CSF recebe a retenção que B2 pretende trocar: `app/ponto/supra_juiz.py:573-588`, trechos: `DisputaSupervisao.sem_veredito_entre(...)` e `return not perguntas_que_seguram_csf(ch)`. Portanto a tabela de efeitos secundários exigida pelo aval é necessária.
- `!` continua obrigatório para gravado exportado e motor: `CLAUDE.md:265-268`, trecho: “tocar o GRAVADO de competencia EXPORTADA […] `ponto/motor_calculo_v2.py`”.
- Caso apareça dinheiro em aberta, publicar a tabela não dispensa as condições gerais: `CLAUDE.md:406-410`, trecho: “PRE-APROVADO com os TRES juntos”: reversão antes, exportadas intactas com hash e prova de frota depois.

A L-126 proíbe **construtor Codex** de tocar `core/juizes.py`: `app/docs/LEIS.md:147`, trecho: “PROIBIDO ao construtor […] `core/juizes.py`”. O item B não identifica o construtor, portanto isso não demonstra colisão do aval; atribuição efetiva permanece **[nao verificado]**.

## 15/11 20:29 · sessao 3c54cafb2d59 · - · - · origem=Ronald (tmux)

2) **NAO PROCEDE** quanto a colisão identificável nas leis lidas. O alarme permanece vigia: `CLAUDE.md:172`, “contador ‘esperado 0’, so alarma, NUNCA juiz”; `app/docs/LEIS.md:94` (L-060), “CRON DE VARREDURA E VIGIA, NUNCA JUIZ”. A medição pela função real e a publicação exigidas pelo item são compatíveis com `CLAUDE.md:35-38`, “Medir chamando a funcao REAL” e “ESCOPO DO AVAL E’ LITERAL”. Os textos completos de 4.2/4.3 e ORDEM-VIVA-1511 não foram localizados na árvore: **[nao verificado]**.

4) **NAO PROCEDE** quanto a exigência adicional de `!` demonstrada pelo escopo A. `CLAUDE.md:265-268` exige `!` para “tocar o GRAVADO de competencia EXPORTADA”, escala/vínculo, regra fora do pedido, atalhos e motor. Nos caminhos examinados:

- `app/folha/vigia_retido_calado.py:44-90`: consultas, contagens e `return out`.
- `app/folha/management/commands/vigiar_retido_calado.py:9`: “NAO TEM `--apply`”; `:37-42`: seleção dos alarmes e `raise SystemExit(2)`.
- `app/escala/services/cadastro_realidade.py:150`: “Nenhum push: a lista nao emite”; `:163-173`: consultas e `return out`.
- `app/relatorios/furos.py:52-100`: consultas e montagem da lista de limbo; `:123-124`: `status_dos_dias_lavrados(..., agora=agora)`; `:160-167`: calendário e retorno dos cards.

Efeito concreto em produção, pares ausentes e motivos prováveis: **[nao verificado]**.

## 15/11 20:25 · sessao 540be23be5a7 · BUG-ENC-CELULAS-FORMA · verificador · origem=Code

O diff remove os dois repasses incompatíveis. Não encontrei chamador de produção residual que entregue chave composta ao tripwire. Permanecem dois problemas anteriores ao diff: alimentação potencialmente sem pad no fechamento e diferença entre os carregadores singular e plural. (`app/ponto/turnos.py:2162`; `app/ponto/services/vigia_de_hora.py:61`; `app/ponto/services/fechamento.py:605`; `app/ponto/turnos.py:2105`; `app/ponto/turnos.py:2154`.)

Execução dos testes, RED anterior, GREEN, cumprimento dos tetos e alcance em produção: **[nao verificado]**. [nao verificado]

**1. Quem produz, quem lê e todos os chamadores de produção**

Os contratos são diferentes:

| Função | Forma de `celulas` e leitores |
|---|---|
| `turnos_do_colab` | `{data: CelulaDia}` da janela com pad; alimenta candidatos por geradora, papel da ata e segmentação. Converte internamente para composta ao perguntar a `vinculo_do_dia`. (`app/ponto/turnos.py:1854`; `app/ponto/turnos.py:1893`; `app/ponto/turnos.py:1944`; `app/ponto/turnos.py:1961`; `app/ponto/turnos.py:1965`.) |
| `dia_encerrado` | Repassa o argumento a `dia_encerrado_motivo`. (`app/ponto/turnos.py:2083`.) |
| `dia_encerrado_motivo` | Repassa `{data: CelulaDia}` somente ao pareador; não fornece essa alimentação a `_escala_vigente` nem ao julgamento do fim previsto. (`app/ponto/turnos.py:2104`; `app/ponto/turnos.py:2105`.) |
| `dias_encerrados` | `{(colaborador_id,data): CelulaDia}`; alimenta vínculo e DNA do encerramento. Depois do diff, não alimenta o pareador com esse mapa. (`app/ponto/turnos.py:2138`; `app/ponto/turnos.py:2153`; `app/ponto/turnos.py:2162`; `app/ponto/turnos.py:2178`; `app/ponto/turnos.py:2179`.) |

O produtor canônico `celulas_do_periodo` constrói chaves compostas; o pareador, quando recebe `None`, carrega o pad e projeta para data. O leitor final do DNA procura `(self.colaborador_id,data)`. (`app/escala/alimentacao.py:73`; `app/ponto/turnos.py:1889`; `app/ponto/turnos.py:1891`; `app/escala/models.py:1120`.)

**Chamadores que fornecem `celulas`, incluindo repasses internos:**

| Chamador | Destino | Forma e alcance |
|---|---|---|
| `app/escala/utils.py:1581` | `turnos_do_colab` | Data, projetada da carga feita com `janela_com_pad`. (`app/escala/utils.py:1307`; `app/escala/utils.py:1308`.) |
| `app/escala/utils.py:1898` | `turnos_do_colab` | Data; conserva células do pad e substitui a ata dos dias montados por cópias em memória. (`app/escala/utils.py:1881`; `app/escala/utils.py:1884`; `app/escala/utils.py:1886`; `app/escala/utils.py:1897`.) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` | `turnos_do_colab` | Data; carga `[ini−2,fim+2]`, que contém o pad atual `[ini−2,fim+1]`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:87`; `app/ponto/turnos.py:1847`.) |
| `app/ponto/turnos.py:2104` | `turnos_do_colab` | Repassa o parâmetro de `dia_encerrado_motivo`. |
| `app/ponto/services/fechamento.py:377` | `dia_encerrado` | Data; recorte do mapa recebido, com risco de pad incompleto explicado abaixo. (`app/ponto/services/fechamento.py:370`; `app/ponto/services/fechamento.py:375`.) |
| `app/ponto/turnos.py:2083` | `dia_encerrado_motivo` | Repassa o parâmetro de `dia_encerrado`. |
| `app/ponto/services/cartorio.py:1154` | `dias_encerrados` | Composta `(c.pk,x.data)`, sem `turnos`; agora não alcança o tripwire. (`app/ponto/turnos.py:2162`.) |

**Demais chamadas de `turnos_do_colab`: todas omitem `celulas`, portanto recebem o default `None`.** (`app/ponto/turnos.py:1850`.)

| Chamadas |
|---|
| `app/ponto/turnos.py:780`; `app/ponto/turnos.py:2162`; `app/ponto/turnos.py:2193`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:3116` |
| `app/ponto/turno_leitura.py:89`; `app/ponto/turno_leitura.py:95` |
| `app/ponto/nucleo.py:47` |
| `app/ponto/views.py:380`; `app/ponto/views.py:1588`; `app/ponto/views.py:3217` |
| `app/relatorios/pdf_espelho.py:213`; `app/relatorios/pdf_espelho.py:451` |
| `app/escala/regua_defesa.py:358` |
| `app/escala/servico_jornada.py:249`; `app/escala/servico_jornada.py:403` |
| `app/api/views.py:1441` |
| `app/chamados/models.py:1359` |
| `app/chamados/juizes.py:401`; `app/chamados/juizes.py:2168` |
| `app/chamados/detector_anomalias.py:157` |
| `app/colaboradores/services/drawer.py:45` |
| `app/ponto/services/espelho.py:614` |
| `app/ponto/services/bordas_realizado.py:66` |
| `app/ponto/services/teto_na_lista.py:68` |
| `app/ponto/services/vigia_pareador_celula.py:97` |
| `app/ponto/services/esmeril_espelho.py:98` |
| `app/ponto/services/triagem_batida.py:553` |
| `app/ponto/services/regularizacao.py:61` |
| `app/ponto/services/passivo_s84.py:82` |
| `app/ponto/management/commands/detectar_par_relampago.py:101` |
| `app/ponto/management/commands/dna_x_batida_real.py:69` |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:46` |
| `app/ponto/management/commands/detectar_ausencias.py:337` |
| `app/ponto/management/commands/vigia_plantio_overnight.py:38` |
| `app/ponto/management/commands/verificar_nucleo.py:29` |
| `app/ponto/management/commands/e6_oraculo.py:159` |
| `app/ponto/management/commands/censo_fase_12x36.py:180` |
| `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124` |
| `app/escala/services/escala_auto.py:87` |
| `app/escala/services/impacto_troca.py:18` |
| `app/chamados/services/materializacao.py:124`; `app/chamados/services/materializacao.py:221` |
| `app/chamados/services/disputa_emissao.py:722` |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:145` |

**Demais chamadas das outras três funções: todas omitem `celulas`.**

| Destino | Chamadas |
|---|---|
| `dia_encerrado` | `app/ponto/services/vigia_de_hora.py:61`; `app/ponto/services/flip_auto.py:285`; `app/ponto/management/commands/detectar_ausencias.py:474` |
| `dia_encerrado_motivo` | `app/chamados/reconciliador.py:1091` |
| `dias_encerrados` | `app/ponto/turnos.py:2191`; `app/ponto/management/commands/supra_juiz.py:121`; `app/ponto/management/commands/emitir_furo_retroativo.py:63` |

Os métodos homônimos chamados em `app/ponto/status_dia.py:274` e `app/core/services/painel_op.py:334` são de `CargaDaJanela`; chamam diretamente o corpo puro, sem passar pelas quatro funções públicas. (`app/ponto/turnos.py:3057`.)

**Tupla residual em produção:** nenhum fornecedor acima entrega composta ao pareador depois do diff; a composta do cartório permanece no encerramento. Chamadores externos à cópia ou construídos dinamicamente: **[nao verificado]**. (`app/escala/utils.py:1581`; `app/escala/utils.py:1898`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`; `app/ponto/services/fechamento.py:375`; `app/ponto/turnos.py:2162`.)

**Data sem pad: sim, há caminho no fechamento.** O envelope começa no início do corte 2 e termina no fim do corte 28; as células são carregadas nesse envelope sem ampliação. `_furos_encerrados` calcula o pad de cada dia, mas apenas copia o que encontra e sempre passa um dict, inclusive vazio. (`app/ponto/services/fechamento.py:584`; `app/ponto/services/fechamento.py:605`; `app/ponto/services/fechamento.py:606`; `app/ponto/services/fechamento.py:370`; `app/ponto/services/fechamento.py:377`.)

Consequentemente, cortes **2 e 3** podem ultrapassar o envelope no início, e corte **28**, no fim. O dict não-`None` suprime a carga própria; o tripwire não verifica completude. É problema anterior ao diff; clientes nesses cortes e células efetivamente omitidas em produção são **[nao verificado]**. (`app/ponto/janelas.py:15`; `app/ponto/janelas.py:20`; `app/ponto/turnos.py:1847`; `app/ponto/turnos.py:1865`; `app/ponto/turnos.py:1889`.)

**2. Testes existentes que quebrariam por chave composta** [nao verificado]

Não encontrei teste existente passando composta diretamente ao pareador ou aos dois singulares. As compostas encontradas no teste existente são destinadas a `dias_encerrados`, cujo contrato continua válido. (`app/ponto/tests/test_c1_dias_encerrados.py:64`; `app/ponto/tests/test_c1_dias_encerrados.py:66`; `app/ponto/tests/test_c1_dias_encerrados.py:77`; `app/ponto/turnos.py:2162`.)

O novo teste que passa composta ao pareador **espera `TypeError`**; o novo teste do cartório passa composta corretamente à casca plural. (`app/ponto/tests/test_bug_enc_celulas_forma.py:87`; `app/ponto/tests/test_bug_enc_celulas_forma.py:88`; `app/ponto/tests/test_bug_enc_celulas_forma.py:33`.)

Resultado efetivo da suíte: **[nao verificado]**. [nao verificado]

**3. Custo novo e selos de queries**

**Sim: há uma nova consulta direta de células por execução alcançada do memo `_enc` do cartório.** Antes a alimentação não-`None` suprimia essa carga; agora a chamada sem `celulas` aciona `celulas_do_periodo` do pad, que executa um SELECT. Não é uma consulta por dia: o mapa de encerramento fica memoizado por corpo/janela. (`app/ponto/services/cartorio.py:1152`; `app/ponto/services/cartorio.py:1154`; `app/ponto/turnos.py:2162`; `app/ponto/turnos.py:1889`; `app/ponto/turnos.py:1891`; `app/escala/alimentacao.py:73`.)

O cron chama o julgamento por colaborador; segunda passada e releitura podem abrir outro corpo e outro memo. O delta líquido total pode também mudar pela consulta complementar de geradoras. Volume, tempo e delta líquido de produção: **[nao verificado]**. (`app/ponto/management/commands/processar_cartorio.py:59`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1021`; `app/ponto/services/cartorio.py:1086`; `app/escala/alimentacao.py:155`; `app/escala/alimentacao.py:157`.)

**Existe cobertura indireta com teto:** o selo de chokepoint registra batida histórica com células e furos vivos, captura SQL, exige o fechamento dos chamados e cobra o teto. O outro ramo mede chamados que permanecem vivos. A cobrança usa `assertLessEqual`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:281`; `app/ponto/tests/test_selo_chokepoint_escrita.py:428`; `app/ponto/tests/test_selo_chokepoint_escrita.py:437`; `app/ponto/tests/test_selo_chokepoint_escrita.py:443`; `app/ponto/tests/test_selo_chokepoint_escrita.py:464`; `app/ponto/tests/test_selo_chokepoint_escrita.py:478`; `app/ponto/tests/test_selo_chokepoint_escrita.py:293`.)

O caminho é signal → `julgar_celula` → `julgar_colab` → `_turno_encerrado` para dia passado. (`app/ponto/signals.py:135`; `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:2040`; `app/ponto/services/cartorio.py:1397`.)

Os testes novos verificam respostas e paridade, sem contagem de queries; o selo do D4 mede o painel com lote. Teto específico do cron e aprovação dinâmica desses tetos nesta obra: **[nao verificado]**. (`app/ponto/tests/test_bug_enc_celulas_forma.py:43`; `app/ponto/tests/test_e4_carga_paridade.py:148`; `app/core/tests/test_e4_7iii_lote_d4.py:43`; `app/core/tests/test_e4_7iii_lote_d4.py:60`.)

**4. Lista, queryset e outros tipos**

O tripwire verifica elementos iterados que sejam tuplas; **não valida que o argumento seja um mapa nem que suas chaves sejam datas**. Depois, o código exige `.values()` e `.items()`. (`app/ponto/turnos.py:1865`; `app/ponto/turnos.py:1893`; `app/ponto/turnos.py:1631`; `app/ponto/turnos.py:1961`.)

Por leitura do código: lista não vazia contendo tupla dispara o `TypeError`; lista sem tuplas, inclusive vazia, chega a `.values()` e falha; queryset pode ser avaliado pela condição booleana e depois falhar na exigência de `.items()`. Execução desses casos: **[nao verificado]**. (`app/ponto/turnos.py:1865`; `app/ponto/turnos.py:1893`; `app/ponto/turnos.py:1631`.)

Não encontrei fornecedor de produção usando lista/queryset nos quatro parâmetros; os fornecedores explícitos estão na tabela da pergunta 1. Há teste usando **`MappingProxyType({})`**, que é mapa e não `dict`, como alimentação válida. (`app/escala/utils.py:1581`; `app/escala/utils.py:1898`; `app/ponto/services/fechamento.py:377`; `app/ponto/services/cartorio.py:1154`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:54`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169`.)

**5. Classes que já reprovaram obra**

| Classe | Parecer |
|---|---|
| Segundo juiz | Não identificado no diff: continuam as autoridades registradas e os mesmos corpos puros. (`app/core/juizes.py:212`; `app/core/juizes.py:239`; `app/ponto/turnos.py:1920`; `app/ponto/turnos.py:2161`; `app/ponto/turnos.py:3048`; `app/ponto/turnos.py:3057`.) |
| Segundo escritor | Não identificado nas alterações: validação, carga e delegação, sem novo escritor de estado. (`app/ponto/turnos.py:1865`; `app/ponto/turnos.py:2153`; `app/ponto/services/vigia_de_hora.py:61`.) |
| Relógio fora da borda | Não há nova leitura: o vigia fornece o instante deslocado; os defaults existentes só leem o relógio quando falta `agora`. (`app/ponto/services/vigia_de_hora.py:61`; `app/ponto/turnos.py:2102`; `app/ponto/turnos.py:2148`.) |
| Derivação paralela | A recarga não cria regra concorrente: vínculo e pareamento continuam nas mesmas autoridades. (`app/ponto/turnos.py:1965`; `app/ponto/turnos.py:1997`; `app/ponto/turnos.py:2178`.) |
| Selo/RED sem mordida | Há expectativas concretas para cartório/vigia e mutante que remove `mais=` do lote. Execução RED/GREEN/mutante: **[nao verificado]**. (`app/ponto/tests/test_bug_enc_celulas_forma.py:49`; `app/ponto/tests/test_bug_enc_celulas_forma.py:50`; `app/ponto/tests/test_bug_enc_celulas_forma.py:72`; `app/ponto/tests/test_bug_enc_celulas_forma.py:75`; `app/ponto/tests/test_e4_carga_paridade.py:159`; `app/ponto/tests/test_e4_carga_paridade.py:165`.) |
| **ESMERIL-SEMANTICA** | **Permanece:** o mesmo nome representa dois formatos e alimenta leitores diferentes. O diff documenta e interrompe a passagem incompatível; não uniformiza o contrato. (`app/ponto/turnos.py:2076`; `app/ponto/turnos.py:2104`; `app/ponto/turnos.py:2138`; `app/ponto/turnos.py:2155`; `app/ponto/turnos.py:2178`.) |

**6. Paridade entre lote, casca, cartório e vigia**

**Lote versus casca plural:** para mesma janela, snapshot e alimentação completa, não encontrei divergência estática: ambos usam `_dias_encerrados_puro` e obtêm turnos pelo mesmo `_turnos_do_colab_puro`, com pad. Os testes disponíveis cruzam X/Y/Z, duas janelas e três instantes; paridade universal executada é **[nao verificado]**. (`app/ponto/turnos.py:2161`; `app/ponto/turnos.py:1920`; `app/ponto/turnos.py:2989`; `app/ponto/turnos.py:3048`; `app/ponto/turnos.py:3057`; `app/ponto/tests/test_e4_carga_paridade.py:47`; `app/ponto/tests/test_e4_carga_paridade.py:48`; `app/ponto/tests/test_e4_carga_paridade.py:145`.)

**Plural/cartório versus singular/vigia: não há garantia universal.** Há contraexemplo além de X/Y, inferido do código:

- Célula de `c1` aponta para vínculo vigente de `c2`. Isso já existe como cenário de teste: grava-se a geradora alheia e exige-se que `escala_vigente(c1,D)` a devolva. (`app/chamados/tests/test_e3_c8c_orm.py:68`; `app/chamados/tests/test_e3_c8c_orm.py:74`; `app/chamados/tests/test_e3_c8c_orm.py:78`.)
- O singular entrega diretamente essa geradora à juíza; o plural e o lote carregam somente vínculos de `c1`. Assim, no plural a geradora falta, a juíza devolve `None` e o encerramento resulta em `sem_previsto`. (`app/escala/servico_jornada.py:61`; `app/escala/servico_jornada.py:63`; `app/ponto/turnos.py:2154`; `app/ponto/turnos.py:3002`; `app/escala/alimentacao.py:279`; `app/escala/alimentacao.py:281`; `app/ponto/turnos.py:2127`; `app/ponto/turnos.py:2129`.)
- Com template noturno 19–07, nenhuma batida, nenhuma célula de `c2` nesse dia e consulta às 08h de D+1, o singular pode encerrar pelo template, enquanto plural/lote permanecem em `sem_previsto`. (`app/escala/models.py:1122`; `app/escala/models.py:1133`; `app/ponto/turnos.py:2122`; `app/ponto/turnos.py:2130`.)
- No vigia às 09h, teto de 30 minutos desloca a pergunta para 08h; vencidas as guardas, ele pode soltar a célula, embora plural/lote não afirmem encerramento. (`app/ponto/services/cartorio.py:862`; `app/ponto/services/vigia_de_hora.py:47`; `app/ponto/services/vigia_de_hora.py:53`; `app/ponto/services/vigia_de_hora.py:55`; `app/ponto/services/vigia_de_hora.py:61`.)

A assimetria é anterior às três mudanças; execução dessa contraprova e ocorrência em produção são **[nao verificado]**. O teste de candidatos, inclusive, exige que vínculo alheio não entre no balde do colaborador solicitado. (`app/escala/tests/test_o131_um_carregador_de_candidatos.py:150`; `app/escala/tests/test_o131_um_carregador_de_candidatos.py:155`.)

**7. Leis dos sítios**

Não identifiquei violação direta **introduzida pelo diff** das leis de domínio listadas:

| Lei | Conferência |
|---|---|
| L-025 | O pareador continua excluindo retratadas. (`app/docs/LEIS.md:76`; `app/ponto/turnos.py:1869`.) |
| L-032, L-085, L-087 | Continuam ata, seleção por geradora e pareador; não nasce regra própria de intervalo, tipo gravado ou dia civil. (`app/docs/LEIS.md:80`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:119`; `app/ponto/turnos.py:1944`; `app/ponto/turnos.py:1965`; `app/ponto/turnos.py:1997`.) |
| L-102, L-103, L-115 | As alterações tratam entrada/carga, sem redefinir palavra, zero ou saída posicional. Regressão comportamental completa: **[nao verificado]**. (`app/docs/LEIS.md:128`; `app/docs/LEIS.md:129`; `app/docs/LEIS.md:153`; `app/ponto/turnos.py:1865`; `app/ponto/turnos.py:2161`.) |
| L-073 | Permanece o julgamento por aberta viva e fim previsto, com instante fornecido pelo vigia. (`app/docs/LEIS.md:100`; `app/ponto/turnos.py:2122`; `app/ponto/turnos.py:2127`; `app/ponto/services/vigia_de_hora.py:61`.) |

O corte que revogava o nascimento de `dia_encerrado` foi corrigido; o registro vivo mantém essa autoridade. Portanto, a existência desse juiz não reprova o diff. (`app/docs/CORTES.md:302`; `app/core/juizes.py:239`.)

Entretanto, **não considero demonstrado o fechamento integral da classe**: o pad incompleto permanece em leitor alcançado, e a contraprova singular/plural limita a coerência universal exigida pela L-099. RED evidenciado, GREEN e suíte continuam **[nao verificado]**, requisitos que o código dos testes sozinho não comprova. (`app/ponto/services/fechamento.py:605`; `app/ponto/services/fechamento.py:377`; `app/ponto/turnos.py:2105`; `app/ponto/turnos.py:2154`; `app/docs/LEIS.md:122`; `CLAUDE.md:28`; `CLAUDE.md:193`.)

## 15/11 20:23 · sessao 7a047bcec0f8 · - · - · origem=Ronald (tmux)

**5. Classes de reprovação**

- **Segundo juiz:** o diff conserva o juiz registrado `dia_encerrado` e o pareador registrado `turnos_do_colab`; casca e lote chamam os mesmos corpos `_dias_encerrados_puro` e `_turnos_do_colab_puro`. Não aparece regra concorrente nas três alterações fornecidas. (`app/core/juizes.py:212`, `app/core/juizes.py:239`, `app/ponto/turnos.py:1920`, `app/ponto/turnos.py:2161`, `app/ponto/turnos.py:3048`, `app/ponto/turnos.py:3057`.)
- **Segundo escritor:** os trechos alterados fazem validação, carga e delegação; o escritor do veredito continua sendo a porta `ponto.portas.celula.lavrar_veredito`, acessada pelo método do modelo. (`app/ponto/turnos.py:1865`, `app/ponto/turnos.py:2153`, `app/ponto/services/vigia_de_hora.py:61`, `app/escala/models.py:1819`.)
- **Relógio fora da borda:** não há nova leitura do relógio nas alterações; o vigia entrega `agora - 2*teto`; os defaults existentes das cascas leem `timezone.now()` somente quando `agora is None`. (`app/ponto/services/vigia_de_hora.py:61`, `app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2148`.)
- **Derivação paralela:** o diff passa a carregar novamente células para o pareamento, mas continua perguntando às mesmas autoridades de vínculo e geometria; duplicação de leitura não constitui segundo cálculo da regra. (`app/ponto/turnos.py:1891`, `app/ponto/turnos.py:1965`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2178`.)
- **Selo sem mordida:** os testes novos têm expectativas independentes para X/Y e para o vigia; o teste de paridade também exige que retirar `mais=` produza divergência. A existência dessas asserções é verificável; RED anterior, GREEN e execução dos mutantes nesta obra são **[nao verificado]**. (`app/ponto/tests/test_bug_enc_celulas_forma.py:46`, `app/ponto/tests/test_bug_enc_celulas_forma.py:71`, `app/ponto/tests/test_e4_carga_paridade.py:127`, `app/ponto/tests/test_e4_carga_paridade.py:150`.)
- **ESMERIL-SEMANTICA: permanece.** Singular `celulas` é mapa `{data}` destinado somente ao pareador; plural é mapa `{(pk,data)}` destinado ao vínculo e ao DNA do encerramento. O singular nem repassa esse argumento à seleção de escala/fim previsto. O diff documenta a assimetria, mas não unifica o contrato. (`app/ponto/turnos.py:2076`, `app/ponto/turnos.py:2104`, `app/ponto/turnos.py:2138`, `app/ponto/turnos.py:2155`, `app/ponto/turnos.py:2178`.)

**6. Paridade e contraexemplo além de X/Y**

**CargaDaJanela versus casca plural:** para mesmos colaboradores, janela, snapshot, células completas e turnos da mesma janela, o encadeamento agora é equivalente: ambas entregam vínculos/células ao mesmo julgamento; ambas obtêm os turnos pelo mesmo corpo e pad. Não encontrei divergência estática nessas condições. A prova executável disponível cobre X/Y/Z, duas janelas e três instantes; paridade universal executada é **[nao verificado]**. (`app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1920`, `app/ponto/turnos.py:2161`, `app/ponto/turnos.py:2989`, `app/ponto/turnos.py:3048`, `app/ponto/turnos.py:3057`, `app/ponto/tests/test_e4_carga_paridade.py:47`, `app/ponto/tests/test_e4_carga_paridade.py:145`.)

**Cartório/plural versus vigia/singular: não há garantia universal. Contraexemplo inferido do código, não executado:**

1. Célula de `c1` em D aponta para vínculo vigente de `c2`, noturno 19–07; nenhuma batida; hoje D+1, agora 09:00. O modelo declara FK simples para a geradora, com unicidade apenas `(colaborador,data)`; não declara igualdade entre os donos. A classe `CelulaDia` não contém `save`/`clean` próprios. (`app/escala/models.py:1635`, `app/escala/models.py:1692`, `app/escala/models.py:1830`.)
2. Isso já é uma fixture aceita por teste existente: ele grava geradora de outro colaborador e exige que `escala_vigente(c1,D)` devolva o vínculo de `c2`; a base confirma que `ec2` pertence a `c2`. (`app/chamados/tests/test_e3_c8c_orm.py:68`, `app/chamados/tests/test_e3_c8c_orm.py:74`, `app/chamados/tests/test_e3_c8c_orm.py:78`, `app/chamados/tests/test_e2_tarefa_do_dia.py:80`, `app/chamados/tests/test_e2_tarefa_do_dia.py:88`.)
3. Singular obtém a geradora diretamente e passa `[_ger]` à juíza; a juíza valida pk/vigência e não valida o dono. Sem célula de `c2`, os marcos caem no template 19–07. Sem turno aberto, às 08:00 o singular responde encerrado. (`app/escala/servico_jornada.py:49`, `app/escala/servico_jornada.py:61`, `app/escala/alimentacao.py:279`, `app/escala/models.py:1122`, `app/escala/models.py:1133`, `app/ponto/turnos.py:2104`, `app/ponto/turnos.py:2122`, `app/ponto/turnos.py:2127`, `app/ponto/turnos.py:2130`.)
4. Plural e lote carregam somente vínculos de `c1`; a geradora `c2` não está na lista, logo a juíza retorna `None` e o motivo é `sem_previsto`, dia não encerrado. (`app/ponto/turnos.py:2154`, `app/ponto/turnos.py:3002`, `app/ponto/turnos.py:3057`, `app/escala/alimentacao.py:279`, `app/ponto/turnos.py:2127`.)
5. No vigia, fim07 passa pelas guardas de ontem; teto30min desloca a pergunta das09 para08, e o singular encerra, fazendo `_ontem_em_janela` retornar `False`. A leitura equivalente do fechamento plural/lote ainda seria não encerrado. (`app/ponto/services/cartorio.py:862`, `app/ponto/services/vigia_de_hora.py:47`, `app/ponto/services/vigia_de_hora.py:53`, `app/ponto/services/vigia_de_hora.py:55`, `app/ponto/services/vigia_de_hora.py:61`.)

Essa diferença decorre dos carregadores que o diff fornecido não altera; existência desse cadastro em produção e execução da contraprova são **[nao verificado]**. O teste antigo de candidatos, inclusive, exige que `mais=` de outro colaborador não entre no balde do solicitado, em contraste com o carregador singular direto. (`app/escala/tests/test_o131_um_carregador_de_candidatos.py:142`, `app/escala/alimentacao.py:155`, `app/escala/servico_jornada.py:61`.)

**7. Leis**

- **L-025:** o pareador continua excluindo batidas retratadas; a mudança não remove o filtro. (`app/docs/LEIS.md:76`, `app/ponto/turnos.py:1868`, `app/ponto/turnos.py:3029`.)
- **L-032, L-085, L-087:** a correção conserva o pareador e sua alimentação por ata/DNA, a seleção por geradora e a data do turno; não introduz ramo novo para intervalo, tipo gravado ou dia civil. (`app/docs/LEIS.md:80`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`, `app/ponto/turnos.py:1944`, `app/ponto/turnos.py:1965`, `app/ponto/turnos.py:1997`.)
- **L-102/L-103/L-115:** os trechos alterados não mudam os contratos de palavra/zero/saída posicional descritos nessas leis; a revisão estática não identifica violação direta delas. Regressão comportamental completa é **[nao verificado]**. (`app/docs/LEIS.md:128`, `app/docs/LEIS.md:129`, `app/docs/LEIS.md:153`, `app/ponto/turnos.py:1865`, `app/ponto/turnos.py:2161`.)
- **Teto temporal/L-073:** permanece a conjunção “aberta viva” + “fim previsto”, consultando as autoridades existentes, e o vigia continua perguntando por instante. (`app/docs/LEIS.md:100`, `app/ponto/turnos.py:2122`, `app/ponto/turnos.py:2127`, `app/ponto/services/vigia_de_hora.py:61`.)
- **Cortes de juízes:** o registro vivo mantém geometria em `turnos_do_colab` e encerramento em `dia_encerrado`; os cortes de `turno_aberto_ha`, cobertura e recusa do admin tratam outras perguntas e não exigem nova autoridade para este diff. (`app/core/juizes.py:212`, `app/core/juizes.py:239`, `app/docs/CORTES.md:303`, `app/docs/CORTES.md:336`, `app/docs/CORTES.md:371`, `app/docs/CORTES.md:381`, `app/docs/CORTES.md:389`.)
- **LEI-AKITA5/RED e anti-vacuidade:** há testes com expectativas concretas e mutante explícito, mas o requisito histórico “RED evidenciado → GREEN → suite” não pode ser certificado pelo material lido; **[nao verificado]**. (`CLAUDE.md:28`, `CLAUDE.md:193`, `CLAUDE.md:194`, `app/ponto/tests/test_bug_enc_celulas_forma.py:49`, `app/ponto/tests/test_e4_carga_paridade.py:150`.)

## 15/11 20:21 · sessao c50ef8d77bea · - · - · origem=Ronald (tmux)

**Censo estático completo:** 49 chamadas de `turnos_do_colab`, 4 de `dia_encerrado`, 2 de `dia_encerrado_motivo`, 4 da função `dias_encerrados`; varredura AST de todos os `.py`, com aliases de importação, fora diretórios `tests`. Nenhuma chamada executável encontrada fora `app`; chamadas dinâmicas construídas em runtime [nao verificado].

**Quem produz/consome o mapa**

- `celulas_do_periodo` produz `{(colaborador_id,data): CelulaDia}` por compreensão; `app/escala/alimentacao.py:54`, `:73`.
- `turnos_do_colab` espera `{data: CelulaDia}`; quando `None`, carrega o pad e projeta as chaves compostas para data; `app/ponto/turnos.py:1854`, `:1889`, `:1891`.
- O mapa alimenta candidatos via `.values()` e geradora; alimenta `papel_por_minuto_da_ata`; depois é reconvertido para composta para `vinculo_do_dia`; `app/ponto/turnos.py:1893`, `:1944`, `:1961`, `:1965`.
- `papel_por_minuto_da_ata` itera `.items()` como `(data,celula)`; `app/ponto/turnos.py:1631`.
- `dia_encerrado` apenas repassa `celulas` a `dia_encerrado_motivo`; este repassa ao pareador, mas **não** ao carregador `_escala_vigente` nem ao julgamento puro; `app/ponto/turnos.py:2083`, `:2104`, `:2105`.
- `dias_encerrados` espera mapa composto; carrega-o somente se `None`; agora não passa esse mapa a `turnos_do_colab`; `app/ponto/turnos.py:2138`, `:2153`, `:2162`.
- O julgamento em lote lê composta por `vinculo_do_dia` e pelo fim previsto; `app/ponto/turnos.py:2178`, `:2179`, `:2127`; o leitor final busca `.get((self.colaborador_id,data))`; `app/escala/models.py:1120`.
- `CargaDaJanela` carrega composta com pad, armazena por colaborador/data, fornece projeção data ao pareador e composta ao encerramento; `app/ponto/turnos.py:2992`, `:2995`, `:2996`, `:3017`, `:3050`, `:3057`.

**Todos os chamadores que fornecem `celulas`**

| Chamador | Destino e forma |
|---|---|
| `app/escala/utils.py:1581` | `turnos_do_colab`: `{data:cel}` por projeção; carga exatamente com `janela_com_pad` em `:1306`–`:1308`. |
| `app/escala/utils.py:1898` | `turnos_do_colab`: `_cels_juiz`, chave data; percorre a mesma carga com pad e preserva células do pad; `:1881`, `:1883`, `:1884`, `:1897`. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` | `turnos_do_colab`: `{x.data:x}`; carrega `[ini-2,fim+2]`, portanto inclui o pad, com um dia extra no fim; `:86`, `:87`. |
| `app/ponto/turnos.py:2104` | `turnos_do_colab`: parâmetro `celulas` repassado por `dia_encerrado_motivo`; forma herdada do chamador. |
| `app/ponto/services/fechamento.py:377` | `dia_encerrado`: `_cel_do_dia`, chave data, construído em `:370`–`:375`; **há pad incompleto possível nas bordas**, descrito abaixo. |
| `app/ponto/turnos.py:2083` | `dia_encerrado_motivo`: parâmetro `celulas` recebido por `dia_encerrado`. |
| `app/ponto/services/cartorio.py:1154` | `dias_encerrados`: `{(c.pk,x.data):x}`; após o diff esse mapa fica apenas no encerramento, sem alcançar o tripwire; `app/ponto/turnos.py:2162`. |

**Todos os demais chamadores: `celulas` omitido, portanto `None`**

`turnos_do_colab`:

- `app/ponto/turnos.py:780`, `:2162`, `:2193`, `:2925`, `:3116`.
- `app/ponto/turno_leitura.py:89`, `:95`.
- `app/ponto/nucleo.py:47`.
- `app/ponto/views.py:380`, `:1588`, `:3217`.
- `app/relatorios/pdf_espelho.py:213`, `:451`.
- `app/escala/regua_defesa.py:358`.
- `app/escala/servico_jornada.py:249`, `:403`.
- `app/api/views.py:1441`.
- `app/chamados/models.py:1359`.
- `app/chamados/juizes.py:401`, `:2168`.
- `app/chamados/detector_anomalias.py:157`.
- `app/colaboradores/services/drawer.py:45`.
- `app/ponto/services/espelho.py:614`.
- `app/ponto/services/bordas_realizado.py:66`.
- `app/ponto/services/teto_na_lista.py:68`.
- `app/ponto/services/vigia_pareador_celula.py:97`.
- `app/ponto/services/esmeril_espelho.py:98`.
- `app/ponto/services/triagem_batida.py:553`.
- `app/ponto/services/regularizacao.py:61`.
- `app/ponto/services/passivo_s84.py:82`.
- `app/ponto/management/commands/detectar_par_relampago.py:101`.
- `app/ponto/management/commands/dna_x_batida_real.py:69`.
- `app/ponto/management/commands/marcar_abandono_provisorio.py:46`.
- `app/ponto/management/commands/detectar_ausencias.py:337`.
- `app/ponto/management/commands/vigia_plantio_overnight.py:38`.
- `app/ponto/management/commands/verificar_nucleo.py:29`.
- `app/ponto/management/commands/e6_oraculo.py:159`.
- `app/ponto/management/commands/censo_fase_12x36.py:180`.
- `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`.
- `app/escala/services/escala_auto.py:87`.
- `app/escala/services/impacto_troca.py:18`.
- `app/chamados/services/materializacao.py:124`, `:221`.
- `app/chamados/services/disputa_emissao.py:722`.
- `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:145`.

`dia_encerrado`:

- `app/ponto/services/vigia_de_hora.py:61`.
- `app/ponto/services/flip_auto.py:285`.
- `app/ponto/management/commands/detectar_ausencias.py:474`.

`dia_encerrado_motivo`:

- `app/chamados/reconciliador.py:1091`.

Função `dias_encerrados`:

- `app/ponto/turnos.py:2191`.
- `app/ponto/management/commands/supra_juiz.py:121`.
- `app/ponto/management/commands/emitir_furo_retroativo.py:63`.

**Métodos homônimos, separados das quatro funções:** `_carga.dias_encerrados` em `app/ponto/status_dia.py:274`; `carga.dias_encerrados` em `app/core/services/painel_op.py:334`; o método chama diretamente `_dias_encerrados_puro` em `app/ponto/turnos.py:3057`.

**Risco de tupla em produção:** nenhum dos chamadores de produção encontrados fornece tupla ao pareador após o diff; os fornecedores diretos produzem data, e a composta do cartório termina no encerramento. Referências: `app/escala/utils.py:1581`, `:1898`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`, `:88`; `app/ponto/services/fechamento.py:375`, `:377`; `app/ponto/services/cartorio.py:1154`; `app/ponto/turnos.py:2162`. Presença de chamador externo à cópia [nao verificado].

**Achei um fornecedor data com pad potencialmente incompleto, preexistente:** o fechamento carrega células entre o início do corte 2 e o fim do corte 28, sem pad (`app/ponto/services/fechamento.py:584`, `:605`, `:606`), indexa por colaborador/data (`:611`–`:613`) e passa esse mapa a `_furos_encerrados` (`:1115`–`:1117`). Este calcula o pad, mas copia apenas células presentes e sempre passa um dict (`:370`–`:377`). Como `janela_com_pad` é `ini-2,fim+1` (`app/ponto/turnos.py:1847`) e o período começa no dia do corte do mês anterior e termina no corte-1 do mês atual (`app/ponto/janelas.py:15`–`:20`), cortes **2 ou 3** podem faltar no início e corte **28** pode faltar no fim. Dict não-None suprime a carga própria (`app/ponto/turnos.py:1889`). Clientes usando esses cortes e células reais fora do envelope [nao verificado].

**Não dict:** nenhum fornecedor de produção encontrado passa lista/queryset; os fornecedores são os dicts citados acima. O tripwire não valida o tipo: itera o argumento (`app/ponto/turnos.py:1865`) e depois o corpo exige `.values()` e `.items()` (`:1893`, `:1961`). Lista, inclusive vazia, chega a `AttributeError` em `.values()`; queryset pode ser avaliado pela condição booleana e não recebe diagnóstico contratual específico. Comportamento executado desses tipos [nao verificado]; inferência do corpo em `app/ponto/turnos.py:1865`, `:1889`, `:1893`, `:1961`.

## 15/11 20:21 · sessao 3c2a8188a83e · - · - · origem=Ronald (tmux)

2. **Não encontrei teste existente que passe chave composta diretamente a `turnos_do_colab`, `dia_encerrado` ou `dia_encerrado_motivo` e passe a quebrar pelo tripwire.** As chamadas compostas existentes são à casca plural, contrato preservado: `app/ponto/tests/test_c1_dias_encerrados.py:64`, `:66`, `:77`; o repasse ao pareador foi removido em `app/ponto/turnos.py:2162`. [nao verificado: execução dos testes]

O novo teste que passa composta diretamente ao pareador **espera** o erro: `app/ponto/tests/test_bug_enc_celulas_forma.py:87`, `:88`. O novo teste de cartório usa composta na casca plural: `app/ponto/tests/test_bug_enc_celulas_forma.py:33`.

Demais alimentações encontradas nos testes: chave data em `app/escala/tests/test_saida_valida_vira_orfa.py:158`; conversão para data em `app/ponto/tests/test_c1_dias_encerrados.py:73`, chamada singular em `:74`; `{}` em `app/ponto/tests/test_o142_turno_invariante_na_janela.py:203`, `:204`, `:214`; `MappingProxyType({})` em `:54`, usado como default do helper em `:161` e repassado em `:169`, `:172`, `:176`. Não encontrei lista/queryset alimentando os quatro em testes; censo estático não é execução [nao verificado].

3. **Sim: uma nova consulta de células por execução do memo `_enc` alcançada pelo julgamento alimentado.** O cartório passa composta sem `turnos` em `app/ponto/services/cartorio.py:1154`; `_enc` só carrega na primeira pergunta por corpo em `:1152`. Agora a casca chama o pareador sem alimentação em `app/ponto/turnos.py:2162`, acionando a carga própria em `:1889`, `:1891`, cujo dict comprehension executa um SELECT em `app/escala/alimentacao.py:73`, `:75`.

É custo **por corpo/janela atingido**, não por dia: memo em `app/ponto/services/cartorio.py:1152`, reutilização em `:1155`. O cron chama `julgar_colab` por colaborador em `app/ponto/management/commands/processar_cartorio.py:59`, `:65`. A releitura pode executar outro corpo e novo memo: `app/ponto/services/cartorio.py:1086`. O número líquido de queries e o volume de produção são [nao verificado]; novas geradoras alcançadas pelo pad podem acionar carga complementar em `app/escala/alimentacao.py:155`, `:157`.

Não generalizar o +1 a chamadas originalmente sem alimentação: pelo diff fornecido, essas já passavam `celulas=None` ao pareador; a carga própria continua em `app/ponto/turnos.py:1889`.

**Há teto indireto sobre o caminho real do cartório.** `app/ponto/tests/test_selo_chokepoint_escrita.py:406` monta batida histórica com células e furos; captura SQL em `:428`, `:429`; exige que os chamados fechem em `:437`, `:442`; cobra tetos em `:438`, `:443`. O outro ramo captura em `:464`, `:465`, cobra em `:473`, `:478`. A função de cobrança realmente compara com teto em `:292`, `:293`; tetos respectivos em `:196`, `:210`, `:228`, `:235`.

A ligação ao corpo é explícita no código: signal da batida em `app/ponto/signals.py:131`, `:135`; julgamento em `:93`; `julgar_celula` chama `julgar_colab` em `app/ponto/services/cartorio.py:2040`; dia passado alcança `_turno_encerrado` em `:1397`. O cenário do selo usa dia de dois dias atrás em `app/ponto/tests/test_selo_chokepoint_escrita.py:281`.

Não encontrei teto **direto do cron** nem dos dois testes novos. Esses últimos fazem verificações de respostas e paridade em `app/ponto/tests/test_bug_enc_celulas_forma.py:43`, `:44`, `:49`, `:50`, `:72`, `:75` e `app/ponto/tests/test_e4_carga_paridade.py:143`, `:148`, `:165`; execução, aprovação dos tetos e RED anterior são [nao verificado].

Os selos do D4 medem outro caminho: painel com lote em `app/core/tests/test_e4_7iii_lote_d4.py:43`, `:47`, `:60`, `:61`; não certificam especificamente a nova carga da casca no cron [nao verificado: cobertura dinâmica].

5. **Os testes novos têm mordida estática além de paridade:** esperados explícitos X/Y em `app/ponto/tests/test_bug_enc_celulas_forma.py:49`, `:50`; vigia False/True em `:72`, `:75`; tripwire com `assertRaises` em `:87`; regra explícita X/Y/Z em `app/ponto/tests/test_e4_carga_paridade.py:129`, `:143`. O mutante remove `mais=` somente do lote em `:159`, `:160`, exige patch chamado em `:162` e divergência de Y em `:165`. Evidência de execução RED/GREEN não foi fornecida [nao verificado].

## 15/11 20:15 · sessao 968c3403316b · CORTE-JUIZ-RESPONDIDA · sonda · origem=Code

universo=50359; total_erros=0; classificadas=50359; fatal=null. (`saída:1`)

Referências `saída:N`: numeração dos objetos JSON fornecidos, começando pelo resumo como linha 1.

**2. Divergências por família**

| Família | Total | Mudança ao adotar `I E T` |
|---|---:|---|
| A | 0 | Nenhuma neste universo. (`saída:2`) |
| B | 9626 | **SIM → NÃO** nos pares `I_sem_T`. (`saída:2,53–64`) |
| C | 9626 | **SIM → NÃO** nos pares `I_sem_T`. (`saída:2,65–76`) |
| D_strip | 0 | Nenhuma neste universo. (`saída:2`) |
| D_cru | 0 | Nenhuma neste universo. (`saída:2`) |
| D_SQL_sem_trim | 0 | Nenhuma neste universo. (`saída:2`) |
| A_sem_trim | 0 | Nenhuma neste universo. (`saída:2`) |

A exige instante e texto após trim; B aceita instante **ou** texto; C aceita somente instante. Esses predicados estão, respectivamente, em `app/chamados/juizes.py:1559`, `app/chamados/models.py:1907–1908` e `app/chamados/models.py:3641`.

As divergências de B e C recaem sobre os mesmos **9626** pares; não são estoques distintos a somar. (`saída:53–76`)

**3. Estoque vivo “instante sem texto”**

| Competência | Quantidade | Estado |
|---|---:|---|
| ABERTA | **2** | Vivas, não validadas, não materializadas; disputa aberta. (`saída:18`) |
| ENTREGUE | **141 = 111 + 29 + 1** | Todas vivas e não validadas; 111 não materializadas, 30 materializadas. (`saída:43,47,50`) |
| Indecidível | **0** | O único grupo de competência indecidível tem `I_T`, portanto não pertence ao estoque. (`saída:52`; quadro completo: `saída:3–52`) |

Total vivo: **143 = 2 + 141**. As contagens batem com o aval registrado: diferença **0 abertas, 0 entregues, 0 no total**. (`saída:18,43,47,50`; `app/docs/LEIS.md:151`)

Os PKs **38251/38252** constam do aval registrado, mas `saída:18` não identifica as perguntas: confirmação desses PKs **nesta execução** é **[nao verificado]**. (`app/docs/LEIS.md:151`)

**4. Pares ilegais “texto sem instante”**

**0** no quadro de estados. Não há ocorrência para discriminar por vida, validação, materialização ou competência: os grupos de `saída:3–52` somam **50359**, fechando o universo de `saída:1`, e nenhum apresenta `sem_I_T`. (`saída:1,3–52`)

## 15/11 20:14 · sessao 43d414010fcc · BUG-ENC-CELULAS-FORMA · censo · origem=Code

**1. O defeito ainda existe, mas a descrição do BACKLOG mistura dois efeitos.** `dias_encerrados` recebe `{(pk, data): célula}` e repassa o argumento original `celulas` ao pareador quando `turnos` não foi fornecido. O pareador espera `{data: célula}`. ([turnos.py:2132](/home/ronald/codex_ws/e84a167f4d704612866e392da6078adaed18f9d0/app/ponto/turnos.py:2132), `app/ponto/turnos.py:2150`, `app/ponto/turnos.py:1854`)

Na segmentação, uma chave `(pk, D)` recebida indevidamente vira `(pk, (pk, D))`; `vinculo_do_dia` procura `(pk, D)`, perde a célula e, portanto, sua geradora. Essa incompatibilidade permanece no HEAD da cópia. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1960`, `app/escala/alimentacao.py:269`)

O sítio antigo `cartorio.py:910` corresponde atualmente a **`:1154`**: continua passando células compostas de `d0..d1`, **sem `turnos`**. Os limites vêm das células do julgamento. ([cartorio.py:1154](/home/ronald/codex_ws/e84a167f4d704612866e392da6078adaed18f9d0/app/ponto/services/cartorio.py:1154), `app/ponto/services/cartorio.py:1131`)

A afirmação “o papel da ata não vê as células por causa da chave composta” não se sustenta: o papel percorre os objetos e entrega cada célula a `periodos_do_dia`, que lê `ata`, `dna` e `celula.data`. Há, entretanto, falta das células externas ao julgamento: o pareador lê `ini−2..fim+1`, e um mapa fornecido suprime sua carga automática. São efeitos distintos. (`app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1633`, `app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:175`, `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1884`)

**2. Escritores, leitores e semântica das formas.**

| Sítio | Produção/leitura e forma |
|---|---|
| `celulas_do_periodo` | Produz `{(pk,data): célula}`, com intervalo inclusivo. (`app/escala/alimentacao.py:55`, `app/escala/alimentacao.py:73`) |
| `dias_encerrados` | Recebe composto; mantém esse mapa para o encerramento, mas o repassa indevidamente ao pareador. Retorna `{data: bool}` ou motivos. (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2134`, `app/ponto/turnos.py:2147`, `app/ponto/turnos.py:2150`) |
| `_dias_encerrados_puro` | Agrupa turnos por `data_turno`; entrega composto corretamente ao vínculo e ao fim previsto. (`app/ponto/turnos.py:2162`, `app/ponto/turnos.py:2166`) |
| `turnos_do_colab` | Espera `{data: célula}` da janela com pad. Quando carrega sozinho, converte composto para data corretamente. (`app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1886`) |
| Candidatura de vínculos | Lê `celulas.values()` para obter geradoras; esse leitor independe da chave. (`app/ponto/turnos.py:1888`) |
| Segmentação | Supõe chaves data, acrescenta `pk` e consulta o vínculo diário; é onde o composto indevido perde a geradora. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1960`) |
| `vinculo_do_dia` | Espera composto. A geradora nomeada vence dentro da vigência, inclusive inativa; sem célula encontrada, usa os vínculos que cobrem a data. (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`) |
| Papel da ata | Percorre células, lê seus objetos e produz mapa por instante local truncado no minuto. Não faz busca por `celulas.get(data)`. (`app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1633`, `app/ponto/turnos.py:904`, `app/ponto/turnos.py:905`) |
| DNA/fim previsto | `marcos_dna_periodo` e `EscalaColaborador.marcos_do_dia` esperam composto. O primeiro produz mapa por data com os marcos. (`app/escala/utils.py:90`, `app/escala/utils.py:97`, `app/escala/models.py:1120`) |
| Cartório | Constrói composto tanto para cadastro quanto para encerramento. O cadastro direto alimenta corretamente `vinculo_do_dia`; o encerramento alcança o repasse defeituoso. (`app/ponto/services/cartorio.py:1143`, `app/ponto/services/cartorio.py:1154`, `app/ponto/supra_juiz.py:174`) |
| Singular | `dia_encerrado` delega a `dia_encerrado_motivo`; este passa `celulas` somente ao pareador. Vínculo e fim previsto não recebem essa alimentação e seguem sua leitura própria. (`app/ponto/turnos.py:2077`, `app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2099`) |
| Vigia | Produz composto corretamente para `escala_vigente`, mas entrega composto indevidamente à singular, cujo destino é o pareador por data. (`app/ponto/services/vigia_de_hora.py:47`, `app/ponto/services/vigia_de_hora.py:58`, `app/ponto/turnos.py:2098`) |
| Fechamento | Cliente singular correto: constrói `{data: célula}` recortado pelo pad de cada dia. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:377`) |
| `CargaDaJanela` | Agrupa por colaborador/data; fornece `{data}` ao pareador e composto ao encerramento, usando seus turnos já calculados. (`app/ponto/turnos.py:2983`, `app/ponto/turnos.py:3038`, `app/ponto/turnos.py:3045`) |
| Motor | Recebe composto; conserva-o para DNA e períodos, convertendo para data ao alimentar o papel. (`app/ponto/motor_calculo_v2.py:473`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:612`) |

Os outros produtores explícitos para o pareador são a montagem da grade, que entrega `{data}` com ata em memória, e o comando retroativo, que entrega `{data}` carregado de `ini−2..fim+2`. (`app/escala/utils.py:1581`, `app/escala/utils.py:1880`, `app/escala/utils.py:1898`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`)

**Data da chave e significado do dia são questões diferentes:** a célula representa o dia do turno; marcos podem alcançar D+1. O pareador consulta o mapa de DNA pela **data civil da batida** para medir distâncias, mas filtra e entrega resultados por **`data_turno`**; o encerramento também agrupa por esse dia. (`app/ponto/turnos.py:2940`, `app/escala/servico_jornada.py:89`, `app/ponto/turnos.py:1231`, `app/ponto/turnos.py:1239`, `app/ponto/turnos.py:1995`, `app/ponto/turnos.py:2162`)

Quanto aos dados persistidos, geração/regeneração e edição manual escrevem trabalho, geradora e DNA; a porta de lavra escreve ata, protestos, veredito, via, instante e status. (`app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:306`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1096`)

**3. Ref, hunks e histórico.**

A cópia disponível não fornece história Git consultável. Existência dos objetos da ref, os 30 commits mencionados, aplicação limpa dos commits e equivalência por outro hash no main: **[nao verificado]**. O diff cumulativo fornecido também não permite atribuir cada hunk individualmente a `6bedd35d4` ou `a2cd7981d`: **[nao verificado]**.

| Hunk fornecido | Situação frente à cópia |
|---|---|
| Alteração de `dias_encerrados` | **Ainda casa textualmente**, deslocada duas linhas: contexto e chamada a remover continuam presentes. Não ficou obsoleto nesse sítio. Aplicação Git efetiva: **[nao verificado]**. (`app/ponto/turnos.py:2147`, `app/ponto/turnos.py:2150`) |
| Novo `test_bug_enc_celulas_forma.py` | Destino não localizado na cópia; conflito ou aplicação Git: **[nao verificado]**. |
| Novo `test_e4_carga_paridade.py` | Destino não localizado na cópia; conflito ou aplicação Git: **[nao verificado]**. |

Documentalmente, **E4-CARGA-PARIDADE é obra separada, anterior ao BUG-ENC**; ambas continuam registradas em linhas próprias. Que `78403ab2b` já esteja no main sob outro hash: **[nao verificado]**. (`app/docs/BACKLOG.md:988`, `app/docs/BACKLOG.md:989`)

Há registro posterior confirmando novamente os dois repasses compostos, plural e singular; isso confirma o recenso documental, sem identificar um commit posterior responsável. (`app/docs/RELATO.md:578`)

**4. Conjunto de arquivos, preservados os contratos atuais.**

| Arquivo | Papel no perímetro mínimo |
|---|---|
| `app/ponto/turnos.py` | Contém o repasse plural incompatível e a fronteira do pareador onde o tripwire proposto se aplica. (`app/ponto/turnos.py:2150`, `app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1956`) |
| `app/ponto/services/vigia_de_hora.py` | Contém o produtor composto indevido da via singular; o hunk plural fornecido não o alcança. (`app/ponto/services/vigia_de_hora.py:58`, `app/ponto/turnos.py:2098`) |
| `app/ponto/tests/test_bug_enc_celulas_forma.py` | Testes propostos de plural, vigia e contrato; conteúdo fornecido no anexo, presença na cópia **[nao verificado]**. |
| `app/ponto/tests/test_e4_carga_paridade.py` | Dependência da fixture importada pelo teste BUG; obra separada no BACKLOG. Presença na cópia **[nao verificado]**. (`app/docs/BACKLOG.md:989`) |

**O defeito de forma não exige alteração de `cartorio.py`: ele fornece exatamente o composto documentado pela porta plural.** Tampouco foi demonstrada necessidade de alterar `chamados/*` para corrigir esse contrato. Uma exigência adicional de versionamento/alcance no cartório para esta obra é **[nao verificado]**. (`app/ponto/services/cartorio.py:1154`, `app/ponto/turnos.py:2132`)

O teste existente de C1 precisa permanecer como proteção do contrato singular por data e do DNA divergente; ele os documenta e compara explicitamente. Necessidade de editá-lo: **[nao verificado]**. (`app/ponto/tests/test_c1_dias_encerrados.py:54`, `app/ponto/tests/test_c1_dias_encerrados.py:73`)

**5. Consumidores e alcance até dinheiro/exportação.**

| Resultado | Caminho comprovado |
|---|---|
| Encerramento → julgamento | Alimenta a impressão `enc=0` e `classificar_dia(turno_encerrado=...)`. (`app/ponto/services/cartorio.py:1397`, `app/ponto/services/cartorio.py:1474`) |
| Julgamento → persistência | Cartório entrega veredito e ata à mesma lavra; a porta grava ambos e aplica o status. (`app/ponto/services/cartorio.py:1851`, `app/ponto/services/cartorio.py:1853`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1096`) |
| Ata/veredito → leitores | Grade reconstrói minutos, órfãs e veredito; prévia lê ata e lâmpadas. (`app/escala/services/leitor_celula.py:413`, `app/escala/services/leitor_celula.py:423`, `app/escala/services/leitor_celula.py:433`, `app/folha/previa.py:83`) |
| Ata/papel → motor | Fechamento alimenta as células; motor deriva papel e o fornece ao pareador. Outro ramo consulta diretamente `periodos_do_dia`. **Sim, `motor_calculo_v2` consome.** (`app/ponto/services/fechamento.py:750`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:506`, `app/ponto/motor_calculo_v2.py:616`) |
| Veredito → TXT | Retenção lê vereditos e protestos; `classificar_export` pode excluir pelo motivo `furo_espelho`. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/folha/export.py:1293`, `app/folha/export.py:968`) |
| Encerramento → furo financeiro | Fechamento filtra os furos produzidos pelo motor antes da lavra de `DiaPago`; esse cliente fornece corretamente `{data}` com pad. (`app/ponto/services/fechamento.py:377`, `app/ponto/services/fechamento.py:1115`, `app/ponto/services/fechamento.py:1293`) |

Portanto, **há consumidores até cálculo e elegibilidade do TXT**; isso não demonstra alteração monetária causada por este bug em produção. Pessoas atingidas, valores e competência exportada efetivamente alterada: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:506`, `app/folha/export.py:970`)

“Já exportada” também não constitui bloqueio absoluto no código atual: o recálculo documenta a retirada dessa recusa, preservando trilha e versão anterior do TXT. A lei aplicável inclui L-113, além de L-092 e do horizonte L-124. (`app/ponto/services/fechamento.py:485`, `app/ponto/services/fechamento.py:493`, `app/docs/LEIS.md:155`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:145`)

**6. Leis e cortes encontrados.**

| Proteção | Referência |
|---|---|
| **L-002** — uma autoridade; testemunha lê | `app/docs/LEIS.md:60` |
| **L-020** — célula e DNA congelado | `app/docs/LEIS.md:71` |
| **L-025** — universo de batidas apuráveis | `app/docs/LEIS.md:76` |
| **L-032** — intervalo cadastrado | `app/docs/LEIS.md:80` |
| **L-061** — consumidor lê célula/ata | `app/docs/LEIS.md:95` |
| **L-073** — teto por instante/fato encerrado | `app/docs/LEIS.md:100` |
| **L-085** — jornada pertence ao dia de início | `app/docs/LEIS.md:117` |
| **L-087** — papel pela ata contra DNA | `app/docs/LEIS.md:119` |
| **L-102/L-103** — sem par: apresentação e zero declarado no fechamento | `app/docs/LEIS.md:128`, `app/docs/LEIS.md:129` |
| **L-110** — teste pela regra, sem código antigo como gabarito | `app/docs/LEIS.md:136` |
| **L-113/L-124** — exportada e horizonte | `app/docs/LEIS.md:155`, `app/docs/LEIS.md:145` |

Os cortes de nascimento/correção de `dia_encerrado` exigem fato encerrado, nunca simplesmente `data < hoje`. O corte do juiz do cálculo protege o motor; o de LAUDOS-[nome] exige ata e motor lendo o mesmo turno, mas pertence a outra obra. (`app/docs/CORTES.md:55`, `app/docs/CORTES.md:62`, `app/docs/CORTES.md:63`, `app/docs/CORTES.md:405`)

Corte nominal específico de BUG-ENC-CELULAS-FORMA ou E4-CARGA-PARIDADE: **[nao verificado]**.

**7. O RED continua discriminante por leitura, com ressalva sobre o artefato raspado.**

Para a fixture lógica X/Y descrita no anexo, a chave aninhada ainda provoca seleção dos marcos do ativo. Em X, a pausa desse ativo absorve S07; em Y, o ativo sem pausa permite fechar na S00. O lote recebe a forma correta e não sofre essa perda da geradora. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1984`, `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:3038`)

| Caso às D+1 08h | Previsão por leitura |
|---|---|
| X, plural alimentado como cartório | Continua aberto até a expiração: `turno_aberto_relogio`; pelo vínculo correto, já estaria encerrado. (`app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1171`, `app/ponto/turnos.py:2030`, `app/ponto/turnos.py:2116`) |
| Y, plural alimentado como cartório | Fecha indevidamente na S00; pelo vínculo correto, a saída é de intervalo e a aberta permanece viva até 11h. (`app/ponto/turnos.py:1984`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:2030`) |
| Vigia X/Y | Continua alcançando o mesmo repasse incompatível pela singular. (`app/ponto/services/vigia_de_hora.py:58`, `app/ponto/turnos.py:2098`) |
| `assertRaises(TypeError)` | Continua discriminante: o contrato é documentado, mas não há validação de chaves; elas seguem até o embrulho silencioso. Exceção observada em execução: **[nao verificado]**. (`app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1956`) |

O teste C1 existente não substitui esse cenário: sua célula tem DNA divergente, sem nomear geradora, e compara o plural composto com a singular por data. (`app/ponto/tests/test_c1_dias_encerrados.py:62`, `app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:73`)

Execução literal do anexo, importação da fixture e resultados efetivos permanecem **[nao verificado]**. Os CPFs raspados não podem ser tratados como os dados originais da fixture: o modelo exige unicidade e normaliza CPF no `save`. (`app/colaboradores/models.py:319`, `app/colaboradores/models.py:418`)

## 15/11 20:12 · sessao fb73725634cf · - · - · origem=Ronald (tmux)

O defeito permanece: `dias_encerrados` documenta mapa `{(pk,data): célula}` e o repassa sem conversão ao pareador, que documenta `{data: célula}`; a segmentação acrescenta outro `pk`, impedindo a busca composta da geradora. (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2150`, `app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1956`, `app/escala/alimentacao.py:269`)

| Produtor/leitor | Forma e semântica |
|---|---|
| `celulas_do_periodo` | Produz composto, intervalo inclusivo. (`app/escala/alimentacao.py:55`, `app/escala/alimentacao.py:73`) |
| `dias_encerrados` | Recebe composto; reaproveita-o no encerramento e passa-o incorretamente ao pareador quando `turnos=None`. (`app/ponto/turnos.py:2147`, `app/ponto/turnos.py:2150`) |
| `_dias_encerrados_puro` | Agrupa por `data_turno`; fornece composto ao vínculo e ao fim previsto. (`app/ponto/turnos.py:2162`, `app/ponto/turnos.py:2166`) |
| `turnos_do_colab` | Contrato `{data}` na janela `ini−2..fim+1`; converte corretamente quando carrega sozinho. (`app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1884`) |
| Candidatos | Lê valores/geradoras, independente da chave. (`app/ponto/turnos.py:1888`) |
| Segmentação | Espera chave data, transforma para composto e pergunta ao vínculo. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1960`) |
| Papel da ata | Itera objetos, entrega cada célula a `periodos_do_dia`; composto não o cega. A chave apenas endereça balde interno. (`app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1633`, `app/ponto/turnos.py:1704`, `app/ponto/turnos.py:1731`) |
| Juiz do papel | Lê ata, DNA e **`celula.data`**; produz instantes dos marcos, com saída possivelmente em D+1. (`app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:175`, `app/escala/servico_jornada.py:89`) |
| Leitor do papel | Busca instante local truncado no minuto, não data isolada. (`app/ponto/turnos.py:904`, `app/ponto/turnos.py:905`) |
| `vinculo_do_dia` | Composto; geradora vence dentro da vigência, inclusive inativa; sem célula, recorre ao vínculo que cobre a data. (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`) |
| Singular | `dia_encerrado_motivo` passa `celulas` somente ao pareador; vínculo e fim previsto não recebem essa alimentação. (`app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2099`) |
| Vigia | Constrói composto corretamente para `escala_vigente`, mas entrega composto incorretamente à singular. (`app/ponto/services/vigia_de_hora.py:47`, `app/ponto/services/vigia_de_hora.py:58`) |
| Fechamento | Contrato singular `{data}`, recortado para o pad de cada dia: cliente correto atual. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:377`) |
| Carga | Agrupa células por colaborador/data; entrega `{data}` ao pareador e composto ao encerramento. (`app/ponto/turnos.py:2983`, `app/ponto/turnos.py:3038`, `app/ponto/turnos.py:3045`) |
| Motor | Recebe composto; converte para data antes do papel; mantém composto para DNA e juiz dos períodos. (`app/ponto/motor_calculo_v2.py:473`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:612`) |

No pareador, o mapa DNA é consultado pela **data civil da batida** para distâncias; a atribuição final e o encerramento usam **`data_turno`**. Os marcos do turno são ancorados no dia de entrada e encadeados, avançando o dia quando necessário. (`app/ponto/turnos.py:1231`, `app/ponto/turnos.py:1239`, `app/ponto/turnos.py:1995`, `app/ponto/turnos.py:2162`, `app/escala/servico_jornada.py:89`, `app/escala/servico_jornada.py:97`)

Inventário dos clientes explícitos atuais:

- Pareador em produção: singular e plural em `turnos.py`; montagem da grade com `{data}` e ata em memória; comando retroativo com `{data}` e intervalo `ini−2..fim+2`. (`app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2150`, `app/escala/utils.py:1880`, `app/escala/utils.py:1898`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`)
- Singular: fechamento correto `{data}` com pad; vigia incorreto composto. (`app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:377`, `app/ponto/services/vigia_de_hora.py:58`)
- Plural: cartório fornece composto e omite `turnos`. (`app/ponto/services/cartorio.py:1154`)
- Testes explícitos: C1 plural composto e singular por data; O142 repassa argumento do helper, incluindo `None`/`{}` conforme o caso; saída órfã passa `{DIA:cel}`. (`app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:73`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:161`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169`, `app/ponto/tests/test_o142_turno_invariante_na_janela.py:203`, `app/escala/tests/test_saida_valida_vira_orfa.py:158`)

Os dados persistidos têm escritores concretos: geração/regeneração monta DNA e grava trabalho/geradora; edição manual também grava esses campos. A lavra escreve ata, protestos, veredito, via, instante e status; `carimbar` persiste os insumos junto dos campos lavrados. (`app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:306`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1096`, `app/ponto/portas/celula.py:1199`)

Por leitura, o RED conceitual ainda distingue X e Y: com chave composta a segmentação escolhe o ativo; em X, intervalo ativo absorve S07 e permanece aberto porque não existe batida posterior; em Y, ativo sem intervalo permite fechar na S00. Pelo vínculo correto, X fecha e Y fica aberto até fim+4h. (`app/ponto/turnos.py:1956`, `app/ponto/turnos.py:1984`, `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1171`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:2030`, `app/ponto/turnos.py:2116`)

O tripwire de `TypeError` também permanece pertinente: a casca não valida o formato antes de carregar e executar; composto segue silenciosamente até a transformação defeituosa. (`app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1915`, `app/ponto/turnos.py:1956`)

Execução e resultados efetivos dos testes anexos: [nao verificado]. [nao verificado]

## 15/11 20:11 · sessao 99b31c068b65 · - · - · origem=Ronald (tmux)

O cartório ainda chama a porta vulnerável sem `turnos`: `_julgar_colab_corpo` extrai `d0,d1` das células em `app/ponto/services/cartorio.py:1131`; `_turno_encerrado` passa o mapa composto `{(c.pk,x.data):x}` em `app/ponto/services/cartorio.py:1154`, sem expandir o pad nem fornecer turnos. A antiga referência `:910` está deslocada; o sítio atual é `:1154`. (`app/ponto/services/cartorio.py:1151`)

O resultado chega a dois usos distintos: marca `enc=0` na impressão do dia passado ainda em curso e alimenta `classificar_dia(turno_encerrado=...)`; os códigos derivados chegam à lavra de veredito e ata. (`app/ponto/services/cartorio.py:1397`, `app/ponto/services/cartorio.py:1399`, `app/ponto/services/cartorio.py:1464`, `app/ponto/services/cartorio.py:1474`, `app/ponto/services/cartorio.py:1853`) O cadastro direto continua correto: carrega todos os vínculos e entrega mapa composto a `cadastro_do_dia`, cujo corpo chama `vinculo_do_dia` com essa alimentação. (`app/ponto/services/cartorio.py:1143`, `app/ponto/services/cartorio.py:1145`, `app/ponto/supra_juiz.py:174`)

Consumidores e alcance:

| Dado | Produção e consumo |
|---|---|
| Ata/veredito | Cartório monta ata e pede lavra conjunta; a porta atribui `cel.ata`, `protestos`, `veredito`, via e instante. (`app/ponto/services/cartorio.py:1851`, `app/ponto/services/cartorio.py:1853`, `app/ponto/portas/celula.py:1091`, `app/ponto/portas/celula.py:1092`, `app/ponto/portas/celula.py:1093`) |
| Status do dia | A porta aplica o status com o mesmo veredito; D4 lê a coluna após o encerramento. (`app/ponto/portas/celula.py:1016`, `app/ponto/portas/celula.py:1021`, `app/ponto/status_dia.py:205`, `app/ponto/status_dia.py:211`) |
| Ata → grade | O leitor reconstrói células/marcos, órfãs, minutos realizados e veredito, transportando cobertura parcial. (`app/escala/services/leitor_celula.py:413`, `app/escala/services/leitor_celula.py:420`, `app/escala/services/leitor_celula.py:423`, `app/escala/services/leitor_celula.py:424`, `app/escala/services/leitor_celula.py:433`) |
| Papel | `periodos_do_dia` lê o objeto `celula.ata`, `celula.dna` e ancora os marcos em `celula.data`; não depende de buscar a célula por chave no mapa recebido pelo papel. (`app/ponto/juiz_batida.py:157`, `app/ponto/juiz_batida.py:158`, `app/ponto/juiz_batida.py:175`) |
| Motor | Fechamento fornece `motor.celulas_alimentadas`; o motor espera composto, converte para `{data:cel}` ao alimentar o papel e passa o papel a `parear_turnos`. Outro ramo busca `cels.get((cid,dia))` e consulta `periodos_do_dia`. (`app/ponto/services/fechamento.py:750`, `app/ponto/motor_calculo_v2.py:498`, `app/ponto/motor_calculo_v2.py:500`, `app/ponto/motor_calculo_v2.py:506`, `app/ponto/motor_calculo_v2.py:612`, `app/ponto/motor_calculo_v2.py:616`) |
| Prévia | Lê `cel.ata`, percorre lâmpadas e apresenta o veredito. (`app/folha/previa.py:83`, `app/folha/previa.py:84`, `app/folha/previa.py:94`) |
| Exportação | Retenção lê `CelulaDia.veredito` e protestos; `classificar_export` coloca o colaborador fora pelo motivo `furo_espelho`. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/folha/export.py:1293`, `app/folha/export.py:1294`, `app/folha/export.py:968`, `app/folha/export.py:970`) |
| Furo no dinheiro | Fechamento usa `dia_encerrado` para filtrar os furos saídos do motor; aqui fornece corretamente mapa por data recortado com pad. A lista filtrada é entregue à lavra de `DiaPago`. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:370`, `app/ponto/services/fechamento.py:373`, `app/ponto/services/fechamento.py:377`, `app/ponto/services/fechamento.py:1115`, `app/ponto/services/fechamento.py:1293`) |

Assim, há caminhos de consumo até cálculo monetário e elegibilidade do TXT; ocorrência concreta, valores, pessoas e alteração de competência exportada por este defeito: **[nao verificado]**. O caminho do fechamento acima não reproduz a forma composta defeituosa: ele documenta e constrói `{data:cel}` com pad. (`app/ponto/services/fechamento.py:350`, `app/ponto/services/fechamento.py:371`)

A regra atual do cartório distingue alcance por horizonte: `REGRA_VERSOES` declara versões/horizontes; `versao_para` usa a competência do juiz `janela_atual` e devolve a maior versão aplicável. Ela não consulta estado exportado ou trancado. Isso é relevante para qualquer futura discussão de alcance, sem afirmar que esta obra precise alterar essa declaração. (`app/ponto/services/cartorio.py:142`, `app/ponto/services/cartorio.py:258`, `app/ponto/services/cartorio.py:262`, `app/ponto/services/cartorio.py:273`, `app/ponto/services/cartorio.py:276`)

Leis e cortes encontrados:

- **L-025**: universo de apuração em `batidas_apuraveis`; **L-032**: intervalo cadastrado; **L-061**: consumidor lê a lâmpada; **L-073**: teto por instante ou fato encerrado. (`app/docs/LEIS.md:76`, `app/docs/LEIS.md:80`, `app/docs/LEIS.md:95`, `app/docs/LEIS.md:100`)
- **L-085**: jornada pertence ao dia de início; **L-087**: papel vem da ata contra DNA, instante vem da batida. (`app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`)
- **L-102/L-103**: sem turno pareado, palavra sem número na apresentação e zero declarado no fechamento. (`app/docs/LEIS.md:128`, `app/docs/LEIS.md:129`)
- **L-110**: regra é referência, código antigo não é gabarito; **L-124**: horizonte para frente. (`app/docs/LEIS.md:136`, `app/docs/LEIS.md:145`)
- Exportadas exigem ler a alteração normativa: **L-113** revoga a recusa de recálculo e pauta como pré-condição; o texto antigo de **L-092** sozinho é insuficiente. (`app/docs/LEIS.md:125`, `app/docs/LEIS.md:155`)
- `JUIZ-DIA-ENCERRADO-NASCE` e `CHAMADO-X-JUIZ-CORRIGIDO`: nome autorizado e critério corrigido para fato encerrado, nunca `data < hoje`; registro exige diff do universo dos consumidores que retratam. (`app/docs/CORTES.md:55`, `app/docs/CORTES.md:62`)
- `JUIZ-CALCULAR-PERIODO-NASCE`: declara autoridade pelo par e proíbe tocar o motor nessa fatia. (`app/docs/CORTES.md:63`)
- `LAUDOS-[nome]-2810`: um juiz do par e ata lendo o mesmo turno do motor; é corte de outra obra, não autorização nominal desta. (`app/docs/CORTES.md:405`)

Corte nominal `BUG-ENC-CELULAS-FORMA` ou `E4-CARGA-PARIDADE` em `CORTES.md`: **[nao verificado]**. Não localizei `AGENTS.md` no inventário `rg --files` da cópia; instruções adicionais fora da cópia: **[nao verificado]**.

## 15/11 20:11 · sessao 3cf37e6a7370 · - · - · origem=Ronald (tmux)

1. **História Git e atribuição dos hunks:** a cópia não contém `.git`; o comando Git retorna “not a git repository”. Aplicação limpa da ref, existência dos três commits, equivalência por patch-id/cherry-pick e commits posteriores responsáveis por mudanças: **[nao verificado]**. O diff fornecido é cumulativo; atribuir cada hunk especificamente a `6bedd35d4` ou `a2cd7981d`: **[nao verificado]**.

2. **Hunks fornecidos contra a cópia:**

| Hunk | Resultado por leitura |
|---|---|
| `turnos.py`, alteração de `dias_encerrados` | **Ainda casa textualmente**, deslocado +2 linhas: `cels`, consulta de escalas e retorno permanecem em `app/ponto/turnos.py:2147`; a linha a remover permanece em `app/ponto/turnos.py:2150`. Contexto não ficou obsoleto. Aplicação Git efetiva: **[nao verificado]**. |
| Novo `test_bug_enc_celulas_forma.py` | Arquivo não localizado na cópia; nenhum hunk preexistente para conflitar. Aplicação Git/estado na ref: **[nao verificado]**. |
| Novo `test_e4_carga_paridade.py` | Arquivo não localizado na cópia; nenhum hunk preexistente para conflitar. Aplicação Git/estado na ref: **[nao verificado]**. |

3. **E4-CARGA-PARIDADE:** documentalmente é obra separada e anterior ao BUG-ENC, ainda registrada como fila própria (`app/docs/BACKLOG.md:989`); BUG-ENC segue interrompida com a ref guardada (`app/docs/BACKLOG.md:988`). Que `78403ab2b` já tenha pousado sob outro hash: **[nao verificado]**. A anotação posterior do recenso ainda confirma as formas compostas nos dois repasses (`app/docs/RELATO.md:578`).

4. **RED lógico ainda discriminante, sem executar:**

- Plural recebe composto e o repassa ao pareador (`app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2150`); o corpo embrulha cada chave novamente (`app/ponto/turnos.py:1956`). Com a fixture lógica X/Y descrita no anexo, a perda da geradora continua alcançando a escolha dos marcos (`app/ponto/turnos.py:1960`, `app/ponto/turnos.py:1984`).
- X: saída das 07h pode ser absorvida pela pausa do vínculo ativo, mantendo o turno aberto (`app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1370`). Y: vínculo ativo sem pausa permite o fechamento na saída das 00h (`app/ponto/turnos.py:1421`, `app/ponto/turnos.py:1492`). O julgamento conserva o motivo `turno_aberto_relogio` enquanto a aberta estiver viva (`app/ponto/turnos.py:2116`).
- O lote converte corretamente composto → data e entrega seus próprios turnos ao encerramento (`app/ponto/turnos.py:3038`, `app/ponto/turnos.py:3045`).
- O vigia continua entregando composto à porta singular (`app/ponto/services/vigia_de_hora.py:58`), que o repassa ao pareador (`app/ponto/turnos.py:2098`). Portanto os dois casos lógicos do vigia continuam relevantes.
- O teste proposto `assertRaises(TypeError)` também distingue o HEAD: a casca documenta `{data}` mas não valida as chaves (`app/ponto/turnos.py:1854`, `app/ponto/turnos.py:1884`); o corpo simplesmente produz a chave aninhada (`app/ponto/turnos.py:1956`). Exceção efetivamente observada: **[nao verificado]**.

5. **Limite do artefato raspado:** o arquivo novo depende da fixture nova E4, ausente da cópia; executar apenas o arquivo BUG isolado não constitui o RED demonstrado **[nao verificado]**. Não interpretar os três valores literais `[cpf]` do anexo como fixture original: CPF é único e normalizado no `save` (`app/colaboradores/models.py:319`, `app/colaboradores/models.py:418`). Passagem/importação/resultado da fixture original da ref: **[nao verificado]**.

6. **Teste atual não substitui esse RED:** o existente documenta expressamente o contrato singular por data e o plural composto (`app/ponto/tests/test_c1_dias_encerrados.py:54`), cria célula com DNA divergente sem nomear geradora (`app/ponto/tests/test_c1_dias_encerrados.py:62`) e testa a alimentação plural e singular convertida (`app/ponto/tests/test_c1_dias_encerrados.py:66`, `app/ponto/tests/test_c1_dias_encerrados.py:73`). Ele não reúne, nesse caso, a fixture multivínculo/noturna X/Y do anexo (`app/ponto/tests/test_c1_dias_encerrados.py:53`).

## 15/11 20:10 · sessao ae12a531a8b9 · VINCULO-LINHA-DO-TEMPO · censo · origem=Code

**1. Escritor e portas**

Na cópia, a porta única declarada é o **módulo** `colaboradores/services/vinculo.py`, não exclusivamente `executar_vinculo`. O gate exige ausência de escritores externos e tem allowlist e pendências vazias. (`app/core/portas.py:470`; `app/escala/tests/test_chokepoint_escala_gate.py:27`; `app/escala/tests/test_chokepoint_escala_gate.py:55`)

O executor **já absorve posteriores**: fecha anteriores, fecha inativas sobrepostas e chama `absorver_vigencias_posteriores`. Portanto, “hoje fecha só o ANTERIOR; o POSTERIOR sobrevive” está defasado em relação ao código fornecido. (`app/colaboradores/services/vinculo.py:214`; `app/colaboradores/services/vinculo.py:241`; `app/colaboradores/services/vinculo.py:253`; `app/docs/BACKLOG.md:1182`)

| Porta | Caminho constatado |
|---|---|
| Wizard de fase | Preenche campos; o formulário envia para `vincular_escala`, que chama juiz/executor. (`app/colaboradores/views_fase.py:5`; `app/templates/colaboradores/partials/painel_vinculo.html:136`; `app/colaboradores/views.py:411`; `app/colaboradores/views.py:423`) |
| Cadastro de escala/painel | Mesmo formulário e executor. (`app/templates/colaboradores/partials/painel_vinculo.html:136`; `app/colaboradores/views.py:423`) |
| Admissão | **Não cria EC nem chama o executor nessa operação**: cria `Colaborador` e registra `HistoricoVinculo`. (`app/colaboradores/views.py:1184`; `app/colaboradores/services/colaborador.py:91`; `app/colaboradores/views.py:1220`) |
| Edição cadastral | Salva `Colaborador`, não EC. (`app/colaboradores/views.py:1344`) |
| Django admin de EC | Somente leitura; adição, alteração e exclusão negadas. (`app/escala/admin.py:19`; `app/core/admin.py:59`; `app/core/admin.py:62`; `app/core/admin.py:65`) |
| Desvinculação | Chama `encerrar_vinculo` do módulo. (`app/escala/views.py:267`) |
| Desligamento | Chama `fechar_vinculos_ativos` do módulo. (`app/colaboradores/services/desligamento.py:147`) |
| Troca pelo chat | Chama juiz/executor. (`app/colaboradores/services/troca_pelo_chat.py:72`; `app/colaboradores/services/troca_pelo_chat.py:75`) |
| Régua/transferência | Chama juiz/executor. (`app/escala/regua_defesa.py:316`; `app/escala/regua_defesa.py:321`) |
| Command de fase | Chama juiz/executor. (`app/ponto/management/commands/censo_fase_12x36.py:422`) |
| Command corrigir vigência | Usa reabertura, fechamento e absorção do módulo. (`app/escala/management/commands/corrigir_vinculo_vigencia.py:86`; `app/escala/management/commands/corrigir_vinculo_vigencia.py:107`; `app/escala/management/commands/corrigir_vinculo_vigencia.py:120`) |
| Command corrigir retroativa | Usa criação de trecho e correção do módulo. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:195`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211`) |
| Command fechar desligados | Usa encerramento do módulo. (`app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:55`) |
| Recusa de aceite | Restaura pelo módulo. (`app/chamados/services/acoes_chamado.py:147`) |
| Ajuste automático/desfazer | Usa funções do módulo. (`app/escala/services/escala_auto_executor.py:94`; `app/escala/services/escala_auto_executor.py:124`) |
| Signals | Leem alterações e acionam regeneração/invalidação/revalidação/reparentamento; não escrevem os três campos investigados. (`app/escala/signals.py:25`; `app/escala/signals.py:73`; `app/escala/signals.py:125`; `app/escala/signals.py:145`; `app/escala/signals.py:174`; `app/escala/signals.py:203`) |

**Escritores de produção externos encontrados: nenhum no censo estático.** Isso não equivale a uma trava de runtime contra qualquer escrita dinâmica; o próprio detector documenta operações não atribuíveis e exclui testes/migrations. (`app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:54`; `app/escala/tests/test_chokepoint_escala_gate.py:55`)

Há várias funções escritoras **dentro** do módulo:

| Escrita | Arquivo:linha |
|---|---|
| Reativar/criar no executor | `app/colaboradores/services/vinculo.py:276`, `app/colaboradores/services/vinculo.py:287` |
| Fechar `(ativa=False, data_fim)` | `app/colaboradores/services/vinculo.py:641` |
| Reabrir `(ativa=True, data_fim)` | `app/colaboradores/services/vinculo.py:730` |
| Absorver, preservando fim | `app/colaboradores/services/vinculo.py:799` |
| Encerrar por `save` | `app/colaboradores/services/vinculo.py:843` |
| Criar trecho retroativo | `app/colaboradores/services/vinculo.py:970` |
| Corrigir fim retroativo | `app/colaboradores/services/vinculo.py:982` |
| Restaurar início/fim/ativa de snapshot | `app/colaboradores/services/vinculo.py:1031` |
| Ajustar automaticamente | `app/colaboradores/services/vinculo.py:1087`, `app/colaboradores/services/vinculo.py:1089` |
| Desfazer ajuste | `app/colaboradores/services/vinculo.py:1106`, `app/colaboradores/services/vinculo.py:1110` |

A correspondência exata da cópia com o HEAD/hash informado é **[nao verificado]**. O passivo atualmente zero em produção é **[nao verificado]**; zero em 30/09 é registro histórico. (`app/docs/RELATO-ARQUIVO.md:3097`)

**2. Censo dos leitores**

As autoridades registradas são `vinculo_do_dia` e `vinculo_cobre_a_data`; a forma ORM é `q_vinculo_cobre_a_data`. A cobertura considera intervalo e `ativa or data_fim`; a escolha diária considera a geradora da célula e, sem geradora, os candidatos que cobrem a data. (`app/core/juizes.py:1314`; `app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:219`; `app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:285`)

Convenção do inventário: **A** consulta essa autoridade/carregador; **B** escolhe vínculo/tipo/fase/posto por `ativa`, sem cobertura da data; **C** usa estado para outra pergunta, descrita abaixo. Chamar depois `eh_dia_trabalho` ou marcos não transforma a seleção do vínculo em A. (`app/chamados/juizes.py:1852`; `app/chamados/juizes.py:1857`; `app/escala/services/furos_diarios.py:301`; `app/escala/services/furos_diarios.py:316`)

| Sítio | Classe e leitura |
|---|---|
| `app/escala/folgas.py:74`, `app/escala/folgas.py:79` | **A** — ativa delimita universo; cobertura mensal vem do carregador em `app/escala/folgas.py:78`. |
| `app/escala/folgas.py:459` | **A** — desempata ativos entre candidatos obtidos por `q_vinculo_cobre_a_data`. (`app/escala/folgas.py:451`) |
| `app/ponto/portas/celula.py:908` | **A** — guarda de ativa depois de cobertura e junto da escolha pelo juiz. (`app/ponto/portas/celula.py:906`; `app/ponto/portas/celula.py:909`) |
| `app/ponto/nucleo.py:15` | **A** — ordena candidatos do carregador; consulta cobertura por dia em `app/ponto/nucleo.py:26`. |

Os seguintes sítios são **B**, com a escolha descrita na última coluna:

| Arquivo:linha | Escolha por ativa sem cobertura da data |
|---|---|
| `app/colaboradores/views_fase.py:45` | Tipo/âncora do EC mais recente. O início que cobre o dia contraditório é consultado separadamente em `app/colaboradores/views_fase.py:73`. |
| `app/colaboradores/views.py:48`, `app/colaboradores/views.py:226`, `app/colaboradores/views.py:268`, `app/colaboradores/views.py:336`, `app/colaboradores/views.py:355`, `app/colaboradores/views.py:394`, `app/colaboradores/views.py:927`, `app/colaboradores/views.py:2206`, `app/colaboradores/views.py:2864` | Prefetch/listas, folga, painéis, posto, agenda e disponibilidade. |
| `app/colaboradores/services/calendario.py:258` | Contexto do calendário. |
| `app/colaboradores/services/drawer.py:22` | Contexto do drawer. |
| `app/colaboradores/services/detalhe.py:73` | Contexto do detalhe. |
| `app/colaboradores/services/troca_pelo_chat.py:54` | Rótulo do vínculo anterior. |
| `app/ponto/selecao_periodo.py:278` | Jornada usada como teto do turno. |
| `app/ponto/turnos.py:2464`, `app/ponto/turnos.py:2483` | Tipo para próxima batida/intermitente/intervalo. |
| `app/ponto/turnos.py:1560` | Ordena por `ativa,data_inicio` e conserva o último; dívida explicitada em `app/ponto/turnos.py:1541`. |
| `app/ponto/worklist.py:49` | Posto da fila DP. |
| `app/ponto/views.py:2064` | Rótulo da escala. |
| `app/ponto/services/faltas_de_hoje.py:35` | EC usado para perguntar se trabalha. |
| `app/ponto/services/abriu_nao_bateu.py:194` | EC usado para procurar marco do dia. |
| `app/ponto/services/piso_intervalo.py:28` | Template para batidas do período. |
| `app/ponto/services/espelho.py:42`, `app/ponto/services/espelho.py:201` | Piso visual e resumo da escala. |
| `app/ponto/services/bordas_realizado.py:62` | Intervalo livre do tipo ativo. |
| `app/ponto/services/fechamento_lista.py:59` | Escala do resumo. |
| `app/ponto/services/triagem_batida.py:92` | “Sem escala” pela inexistência de ativa. |
| `app/ponto/services/triagem_batida.py:332` | Rótulo de anuência por tipo ativo. |
| `app/inteligencia/calculadores.py:327` | Cálculo de desvio de escala. |
| `app/relatorios/furos.py:60` | EC ativo sem fim para limbo de hoje/amanhã. |
| `app/relatorios/views.py:492` | Extrato parcial. |
| `app/relatorios/views.py:1320` | Primeiro EC com `ec.ativa`. |
| `app/relatorios/pendencias.py:74` | Tipo exibido para direção. |
| `app/core/views.py:401`, `app/core/views.py:454`, `app/core/views.py:789` | Supervisor, onboarding e painel TV. |
| `app/core/services/painel_op.py:109` | Mapa de tipo ativo. |
| `app/holerite/views.py:282` | Escala na lista de pessoas. |
| `app/escala/views_plano_folgas.py:55` | ECs da grade mensal. |
| `app/escala/views.py:17` | Minha escala. |
| `app/escala/services/escala_auto.py:172` | EC ativo sem fim mais recente para proposta. |
| `app/escala/services/fase_conflitante.py:71` | Foto/fase do próprio vínculo ativo. |
| `app/escala/services/impacto_troca.py:34` | EC do “antes” da troca. |
| `app/escala/services/meio_periodo.py:152` | Observado por tipo ativo. |
| `app/escala/services/cadastro_realidade.py:228` | Mapa de tipo ativo comparado com células do período. |
| `app/escala/services/furos_diarios.py:35`, `app/escala/services/furos_diarios.py:301` | Apuração e classificação sem escala/limbo/sem dia. |
| `app/api/views_core.py:299` | `api_me`. |
| `app/api/views.py:311`, `app/api/views.py:908` | `api_me` e `api_escala_v2`. |
| `app/chamados/juizes.py:1852` | EC para marco esperado da pergunta. |
| `app/chamados/views.py:442` | EC ativo mais recente do tipo. |
| `app/chamados/services/disputa_emissao.py:349` | Opções de escala por ativa/sem fim. |
| `app/chamados/services/desvio_escala.py:44` | EC ativo mais recente. |
| `app/folha/views_retrato.py:107` | Cadastro do retrato. |
| `app/folha/services/ranking_he.py:132` | Tipo usado no ranking. |
| `app/ferias/management/commands/alertar_retornos.py:28` | Tipo ativo. |
| `app/relatorios/management/commands/diff_extrato_parcial.py:105` | Mapa de tipo ativo. |
| `app/escala/management/commands/termometro_regua.py:35` | EC para régua. |
| `app/escala/management/commands/celulas_contra_o_template.py:28` | EC ativo comparado com células. |
| `app/ponto/management/commands/diff_espelho_alimentacao.py:94` | Caminho legado do diff. |
| `app/ponto/management/commands/tripwire_celulas.py:17` | Mapa de EC ativo comparado com células. |
| `app/ponto/management/commands/reconciliar_fantasmas.py:173` | EC do diagnóstico. |
| `app/ponto/management/commands/fechar_chamados_meio_periodo.py:60` | Tipo ativo. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:115` | Tipo ativo. |
| `app/ponto/management/commands/censo_fase_12x36.py:403` | EC ativo mais recente na operação `_curar`. |

Os seguintes sítios são **C** — não consultam o juiz nessa leitura, mas respondem outra pergunta:

| Arquivo:linha | Outro uso |
|---|---|
| `app/folha/export.py:806` | Guarda de vínculo sem âncora para exportação. |
| `app/colaboradores/services/geofence_raios.py:63` | Quantidade de vínculos ativos por posto. |
| `app/ponto/services/cluster_espurio.py:185` | Universo do reconciliador: existência de ativo sem fim. |
| `app/relatorios/views.py:830`, `app/relatorios/views.py:856` | Universo dos filtros de busca por tipo/fase. |
| `app/escala/views_plano_folgas.py:43` | Postos com vínculo ativo. |
| `app/escala/views_plano_folgas.py:109`, `app/escala/views_plano_folgas.py:176` | Guarda por PK para editar/aplicar folgas. |
| `app/escala/utils.py:2115` | Existência de âncora; função retorna zero em `app/escala/utils.py:2123`. |
| `app/escala/regua_defesa.py:259` | Universo/contagem de templates para sugerir destino. |
| `app/escala/views.py:86`, `app/escala/views.py:124`, `app/escala/views.py:165`, `app/escala/views.py:171`, `app/escala/views.py:240`, `app/escala/views.py:262`, `app/escala/views.py:392` | Listas/contagens, proteção de exclusão, não vinculados, alvo do desvínculo e aplicação de calendário; inclui consulta `ativa=False`. |
| `app/escala/services/escala_auto.py:186` | Contagem de compartilhamento do template. |
| `app/escala/services/meio_periodo.py:80` | Contagem por template. |
| `app/escala/services/furos_diarios.py:158` | Universo do ranking por join de ativa. |
| `app/escala/services/cadastro_tipo.py:339` | Universo de propagação do template; recorte temporal próprio em `app/escala/services/cadastro_tipo.py:342`. |
| `app/api/views_mensageria.py:1222` | Pendência cadastral de folga em 6x1. |
| `app/chamados/reconciliador.py:233` | Premissa “existe ativa sem âncora”. |
| `app/chamados/detector_anomalias.py:58` | Colisão por quantidade de ativas sem fim; não é censo geral de sobreposição temporal. |
| `app/chamados/services/limbo_folgas.py:65` | Quantidade de usuários do template. |
| `app/chamados/services/desvio_escala.py:106` | Outros usuários ativos do template. |
| `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:45` | Estado ativo de colaborador fora da situação ativa. |
| `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:34` | Alvos de fechamento. |
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42` | Universo de atualização cadastral. |
| `app/ponto/management/commands/detectar_par_relampago.py:336` | Universo do detector. |
| `app/ponto/management/commands/detectar_cluster_espurio.py:71` | Universo do detector. |
| `app/ponto/management/commands/tripwire_celulas.py:57` | Universo ativo e regra temporal própria, sem juiz, em `app/ponto/management/commands/tripwire_celulas.py:67`. |
| `app/ponto/management/commands/checklist_golive.py:70` | Contagem de pessoas sem join de ativa. |
| `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73` | Universo smoke com limite próprio de início. |
| `app/ponto/management/commands/detectar_ausencias.py:185` | Colegas usuários do template. |
| `app/ponto/management/commands/propor_reorganizacao.py:42` | Universo ativo intermitente. |
| `app/escala/management/commands/corrigir_vinculo_vigencia.py:100`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:124` | Filtros Python por ativa e início para planejar correção. |
| `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:118` | Comparador temporal legado, confrontado com juiz em `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:115`. |
| `app/chamados/services/carga_pergunta_viva.py:97` | Ordenação da carga para leitores C6. |
| `app/ponto/portas/regen_exportada.py:180` | Ativa decide extensão do horizonte. |
| `app/escala/admin.py:28` | Filtro administrativo por estado. |
| `app/colaboradores/services/ficha.py:94`, `app/escala/services/vigencia_sem_trilha.py:78` | Serialização de estado. |
| `bin/censo_folha.py:25` | Contagem diagnóstica de ativas; zero recebe rótulo `SEM_ESCALA` em `bin/censo_folha.py:33`. |

No próprio escritor, são **C**, como estado/identidade/alvos da operação: no-op (`app/colaboradores/services/vinculo.py:119`, `app/colaboradores/services/vinculo.py:711`); foto anterior (`app/colaboradores/services/vinculo.py:183`); alvos de fechamento (`app/colaboradores/services/vinculo.py:215`, `app/colaboradores/services/vinculo.py:242`, `app/colaboradores/services/vinculo.py:863`); EC recém-escrita/aceite (`app/colaboradores/services/vinculo.py:322`, `app/colaboradores/services/vinculo.py:405`); censo de sobreposição com comparação própria (`app/colaboradores/services/vinculo.py:523`, `app/colaboradores/services/vinculo.py:537`); absorção (`app/colaboradores/services/vinculo.py:788`); invalidação de âncoras (`app/colaboradores/services/vinculo.py:990`); fotos/trilha (`app/colaboradores/services/vinculo.py:37`, `app/colaboradores/services/vinculo.py:111`, `app/colaboradores/services/vinculo.py:497`, `app/colaboradores/services/vinculo.py:639`).

A implementação da própria autoridade/carregador é **C**, não leitor externo: candidatura, ordenação, vigência declarada, negação ORM e desempate. (`app/escala/alimentacao.py:150`; `app/escala/alimentacao.py:153`; `app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:202`; `app/escala/alimentacao.py:286`)

Leituras em testes são **C**, como arranjo/asserção, nos seguintes sítios: [nao verificado]

| Área | Arquivo:linha |
|---|---|
| Colaboradores | `app/colaboradores/tests/test_vincular_escala.py:110`; `app/colaboradores/tests/test_fase_12x36_tela.py:105`; `app/colaboradores/tests/test_ficha_fonte_dos_totais.py:68`; `app/colaboradores/tests/test_bug110_rota_da_ui_vincula.py:83`; `app/colaboradores/tests/test_vincular_escala_selos.py:52`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:88`; `app/colaboradores/tests/test_escala_aceite.py:78` |
| O207/linha do tempo | `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:157`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:231`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:266`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:309`, `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:324`; `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:73`, `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:125` |
| Ponto | `app/ponto/tests/test_alertas_nucleo.py:80`, `app/ponto/tests/test_alertas_nucleo.py:154`, `app/ponto/tests/test_alertas_nucleo.py:198`, `app/ponto/tests/test_alertas_nucleo.py:241`; `app/ponto/tests/test_fluxos.py:129`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:84` |
| Demais | `app/core/tests/test_haiku_cirurgico.py:121`; `app/escala/tests/test_termometro_camada3.py:102`; `app/escala/tests/test_vigencia_um_escritor.py:79`, `app/escala/tests/test_vigencia_um_escritor.py:160`; `app/escala/tests/test_e4_limbo_pergunta_a_juiza.py:41`; `app/chamados/tests/test_e4b_c12_orm.py:82` |

O censo existente de “cópias da pergunta 2” procura comparações de início/fim; ele **não é um censo de todo `ativa=True` sem data**. Portanto sua lista vazia não certifica ausência dos B acima. (`app/core/censo_pergunta2.py:10`; `app/core/censo_pergunta2.py:37`; `app/core/censo_pergunta2.py:115`)

**3. Testes/fixtures e os “19 selos”** [nao verificado]

O registro dos **19 vermelhos é de `UniqueConstraint(colaborador, condition=ativa=True)`**, incluindo vínculos adjacentes. Não demonstra 19 falhas de EXCLUDE. O relato nomeia oito testes; os outros onze e a reprodução da contagem nesta cópia são **[nao verificado]**. (`app/docs/RELATO-ARQUIVO.md:3094`; `app/docs/RELATO-ARQUIVO.md:3099`; `app/docs/RELATO-ARQUIVO.md:3102`)

| Teste nomeado | Construção e dependência | [nao verificado]
|---|---|
| `test_01_colisao_escala_multiplas_ativas` | **Sobreposição proposital**: dois ativos sem fim; exige detectar colisão. (`app/chamados/tests/test_detector_avancado.py:121`; `app/chamados/tests/test_detector_avancado.py:123`; `app/chamados/tests/test_detector_avancado.py:129`) |
| `test_MORDE_a_absorcao_deixa_trilha_nos_DOIS_lugares` | **Sobreposição proposital**: dois ativos abertos para provar absorção com histórico/auditoria. (`app/colaboradores/tests/test_vinculo_linha_do_tempo.py:95`; `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:99`; `app/colaboradores/tests/test_vinculo_linha_do_tempo.py:105`) |
| `test_MORDE_o_censo_acha_a_sobreposicao_e_a_cura_a_zera` | **Sobrepos

[truncado em 20000 de 26324 caracteres]

## 15/11 20:09 · sessao aa99330323f7 · O95 · censo · origem=Code

1. **A absorção ainda existe, mas o desfecho já tem uma correção posterior na cópia.** O ramo absorve a batida quando há turno corrente, distância ao intervalo `<=90` minutos e intervalo tão próximo quanto — ou mais próximo que — a borda. Ele não procura volta `E`, carimba `_intra_dur=True`, acrescenta a batida ao turno e continua. O empate favorece o intervalo. (`app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1364`, `app/ponto/turnos.py:1370`)

Há uma guarda anterior: uma batida ainda não marcada como intervalo, próxima do marco e além de `cont_max_s` desde a entrada, encerra o turno corrente **como aberto** antes dessa absorção. A comparação usa timestamps; não exige volta. (`app/ponto/turnos.py:1349`)

Entretanto, ao final, `parear_turnos` executa `_fechar_aberto_com_saida_seguinte` e depois **`_fechar_aberto_na_pausa_sem_volta`**. Portanto, a linha anexada descreve um mecanismo ainda presente, mas não descreve integralmente o comportamento final desta cópia. (`app/ponto/turnos.py:1499`)

O resgate O95 fecha na última batida do turno quando:

- o turno tem entrada e continua sem saída;
- a última batida é `S`, carimbada `_intra_dur`;
- existe batida posterior;
- a duração passa pelo piso de par e cabe no teto da jornada. (`app/ponto/turnos.py:1166`, `app/ponto/turnos.py:1170`, `app/ponto/turnos.py:1176`)

**Essa prova posterior não é “volta dentro de `intra_max_s`”.** É qualquer batida posterior conhecida: o máximo das batidas carregadas e, quando fornecido, `ultima_batida`. Sem fato posterior, conserva aberto. (`app/ponto/turnos.py:1156`, `app/ponto/turnos.py:1173`)

Existe uma diferença entre portas: `turnos_do_colab` fornece o máximo global das batidas apuráveis; `turnos_de_batidas` e `turnos_via_autoridade` não fornecem esse provedor, ficando com o universo carregado. Isso precisa entrar no DIFF por janela. (`app/ponto/turnos.py:1898`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1535`, `app/ponto/motor_calculo_v2.py:503`)

A comparação com `_marca_intra_duracao` exige três distinções:

| Situação | Prova realmente exigida |
|---|---|
| Sem bordas declaradas | Turno em curso, `S→E` dentro de `intra_max_s` **e `S` seguinte** dentro de `cont_max_s`. (`app/ponto/turnos.py:407`) |
| Com bordas | `S→E` dentro de `intra_max_s`; `S` longe das bordas; `E` também longe, **ou** continuidade provada pela próxima `S`. A saída posterior não é exigida universalmente: ela libera a exceção da volta próxima da borda. (`app/ponto/turnos.py:432`, `app/ponto/turnos.py:446`, `app/ponto/turnos.py:453`) |
| Ramo `S,S` | Pode marcar a primeira `S` sem volta `E`, desde que encontre outra `S` posterior, dentro do envelope, próxima de borda. (`app/ponto/turnos.py:476`) |

O comentário citado de 25/07 existe e corresponde à **exceção de continuidade**; interpretá-lo como exigência universal de saída posterior seria mais forte que o corpo. (`app/ponto/turnos.py:438`, `app/ponto/turnos.py:453`)

2. **O campo compartilhado não garante significado idêntico entre leitores.** `Turno` contém `data_turno`, objetos de batida nas bordas, lista de batidas e estado aberto. A construção calcula o dia pelo juiz; ele pode ser a véspera da data civil da entrada. (`app/ponto/turnos.py:43`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:312`)

As chamadas diretas de produção encontradas são:

| Porta | Local e participação nessa absorção |
|---|---|
| Motor | `turnos_via_autoridade`, com marcos e parâmetros `AUT_*`. (`app/ponto/motor_calculo_v2.py:503`) |
| Batidas já carregadas | `turnos_de_batidas`, com marcos do template/DNA. (`app/ponto/turnos.py:1535`) |
| Geometria por colaborador | `_turnos_do_colab_puro`, com marcos, papel da ata e última batida global. (`app/ponto/turnos.py:1992`) |
| Turno aberto atual | `_turno_aberto_puro`. (`app/ponto/turnos.py:2278`) |
| Reconciliação de fantasmas | Passa marcos de intervalo e borda. (`app/ponto/management/commands/reconciliar_fantasmas.py:99`) |
| Chamadas sem marcos de intervalo | Não ativam esse ramo específico: órfãos, semana do posto, meio período, fila por causa, diagnóstico e lastro. (`app/ponto/utils_turno_orfao.py:31`, `app/escala/services/semana_do_posto.py:120`, `app/escala/services/meio_periodo.py:168`, `app/chamados/services/fila_por_causa.py:197`, `app/ponto/services/diagnostico_escala.py:142`, `app/ponto/services/lastro.py:268`) |
| Piso de intervalo | Passa explicitamente `marcos_intervalo=None`. (`app/ponto/services/piso_intervalo.py:48`) |

O consumo nos domínios pedidos é este:

| Leitor | O que lê; diferença de significado |
|---|---|
| **Motor** | O adaptador transforma bordas em **instantes**, aberto em saída `None` com anomalia, e saída órfã somente em anomalia. Intervalos vêm da autoridade. (`app/ponto/adaptador_motor.py:30`, `app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:44`) |
| **Motor: dia da jornada** | Mantém mapa instante→`data_turno`, mas há duas receitas: períodos com parâmetros `AUT_*` e turnos do juiz para dia/intervalo. O próprio contrato distingue essas respostas. (`app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:884`) |
| **Núcleo** | Copia `data_turno`, entrada, saída, aberto e cruzamento para `TurnoMaterializado`. Conserva o significado, mas publica uma representação persistida. (`app/ponto/nucleo.py:47`, `app/ponto/nucleo.py:50`, `app/ponto/nucleo.py:57`) |
| **Cartório/grade** | O cartório monta a grade; ela consulta turnos por `data_turno`, reapresenta as lâmpadas da montagem ao pareador e calcula realizado pelo juiz. A ata recebe minutos realizados, lâmpadas e órfãs. **Realizado não equivale a rubrica paga.** (`app/ponto/services/cartorio.py:1175`, `app/escala/utils.py:1581`, `app/escala/utils.py:1898`, `app/escala/utils.py:2045`, `app/ponto/services/cartorio.py:736`) |
| **Espelho tela** | Cards usam ponte instante→pk→dia do juiz. Porém o contador de turnos recorta a competência pela **data civil local da entrada**, enquanto `datas_turno_aberto` recorta pelo **dia do juiz**. É uma divergência concreta na borda da competência. (`app/ponto/services/espelho.py:426`, `app/ponto/services/espelho.py:450`, `app/ponto/services/espelho.py:471`) |
| **Espelho PDF** | O contador usa `len(turnos_do_colab(...))`, geometria da janela inteira; o comentário reconhece a diferença perante os períodos do motor contados pela tela. (`app/relatorios/pdf_espelho.py:444`, `app/relatorios/pdf_espelho.py:451`) |
| **Chamados: reconciliador** | Consulta `TurnoMaterializado` com entrada e sem saída, em `{dia−1,dia}`, e aplica veto posicional. Não é leitura direta do resultado vivo nem simples uso do booleano `aberto`. (`app/chamados/reconciliador.py:96`, `app/chamados/reconciliador.py:112`) |
| **Chamados: disputa/detector** | Juiz de disputa exige turnos fechados no `data_turno` pedido; detector procura a batida por timestamp e usa `t.data_turno`. (`app/chamados/juizes.py:401`, `app/chamados/detector_anomalias.py:157`, `app/chamados/detector_anomalias.py:162`) |
| **Chamados: materialização** | Origem procura turno aberto com entrada anterior ao instante de criação do chamado; `turnos_sem_par` inclui também saída órfã. São perguntas diferentes. (`app/chamados/services/materializacao.py:124`, `app/chamados/services/materializacao.py:127`, `app/chamados/services/materializacao.py:221`) |
| **Chamados: cruzamento** | Um juiz lê `cross_meianoite` por `data_turno`; outro considera as **datas civis locais das duas bordas** para resolver órfão. Não usam a mesma definição de dia. (`app/chamados/juizes.py:2168`, `app/chamados/services/disputa_emissao.py:722`, `app/chamados/services/disputa_emissao.py:729`) |
| **Folha** | Consome `FechamentoMensal` e traduz campos em eventos/rubricas; não reinterpreta diretamente `Turno`. Na lavratura diária, usa o mapa de jornada quando disponível e data civil local da entrada quando falta resposta no mapa. (`app/folha/export.py:402`, `app/folha/export.py:714`, `app/ponto/services/dia_pago.py:126`) |

Há ainda uma leitura temporal própria no verificador da folha: agrupa períodos por `_p.entrada.date()`, sem consultar `data_turno` nesse trecho. A ocorrência produtiva de divergência nesse sítio é [nao verificado]. (`app/folha/porta_export.py:535`)

Dois cuidados adicionais para o esmeril:

- O resgate O95 transforma a batida carimbada em saída **sem apagar o carimbo**. `_pares_marcados` exclui as bordas antes de contar pausa; contar somente `_intra_dur` mudaria o significado. O vigia também exclui bordas. (`app/ponto/turnos.py:1186`, `app/ponto/turnos.py:802`, `app/ponto/turnos.py:880`, `app/ponto/management/commands/vigiar_motor_autoridade.py:78`)
- O estado “em intervalo” significa turno aberto cuja última batida é `S`; não exige volta já existente. Esse estado participa da sugestão do próximo tipo. (`app/ponto/turnos.py:2498`, `app/ponto/turnos.py:2503`, `app/ponto/turnos.py:2474`)

3. **Há outras inferências por marco, mas com provas e efeitos diferentes.**

| Classe | Prova e efeito |
|---|---|
| **Eco junto de luz acesa, inclusive bordas** | Batida dentro do vão completo da ata e a até 15 minutos de uma luz recebe `X`, ficando interna. Não exige volta própria, mas exige entrada/saída da ata e luzes sustentadas por batidas reais únicas. (`app/escala/orfa_classe.py:28`, `app/ponto/turnos.py:1655`, `app/ponto/turnos.py:1682`, `app/ponto/turnos.py:1742`, `app/ponto/turnos.py:1804`) |
| **Primeira `S` de uma sequência `S,S`** | Absorvida sem `E` de volta, com prova de saída posterior próxima de borda e dentro do envelope. (`app/ponto/turnos.py:476`) |
| **Alinhamento de batida a borda** | `_match_marcos` atribui uma batida a um marco por distância/custo no envelope; pode acender uma borda sem parceiro. Isso produz atribuição, não prova de turno fechado. (`app/escala/utils.py:229`, `app/escala/utils.py:625`, `app/escala/utils.py:641`) |
| **Reclassificação E/S pelo marco** | `tipo_por_marcos` e `decidir_tipo_estrito` inferem papel sem exigir parceiro. Não equivalem à absorção como pausa. (`app/ponto/turnos.py:2397`, `app/ponto/turnos.py:2424`) |
| **Recorte das bordas para HE** | Entrada/saída efetivas podem cortar minutos conforme janela cadastrada; trabalham sobre pontas reais e autorização, sem provar uma volta. É efeito monetário por marco. (`app/ponto/motor_calculo_v2.py:1691`, `app/ponto/motor_calculo_v2.py:1733`, `app/ponto/janela_he.py:76`, `app/ponto/janela_he.py:117`) |
| **Desconto de intervalo declarado sem par batido** | `realizado_dos_turnos` subtrai a interseção com a janela declarada quando aplicável. É presunção de descanso na valoração, distinta de absorver uma batida. (`app/ponto/turnos.py:756`, `app/ponto/turnos.py:760`) |

A tolerância de pontualidade é outra operação: calcula atraso/antecipada e não altera horas trabalhadas nessa função. Não deve ser equiparada ao raio de casamento de marcos. (`app/ponto/motor_calculo_v2.py:1951`, `app/ponto/motor_calculo_v2.py:1959`, `app/escala/utils.py:653`)

4. **A sonda deve chamar o escritor real em modo leitura nos dois cenários.** A função é:

```python
recalcular_fechamento_mes(
    mes, ano,
    empresa_id=empresa.pk,
    colaborador_ids=ids,
    somente_leitura=True,
    retornar_objeto=True,
    lavrar_dias_pagos=False,
)
```

Ela monta o motor com `get_motor_cct`, apura por fatias e retorna os próprios fechamentos destacados; não é necessário reproduzir pareamento, soma de horas ou montagem do fechamento. (`app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:735`, `app/ponto/services/fechamento.py:754`, `app/ponto/services/fechamento.py:890`, `app/ponto/services/fechamento.py:1128`)

O universo deve vir de `universo_da_competencia`, por empresa e competência, incluindo quem ainda não possui fechamento. A janela é `janela_fechamento`, não mês civil. Excluir entregues por `competencia_entregue`; para exigir também competência aberta, excluir `competencia_trancada`. Esses estados são distintos no código. O intervalo histórico e a quantidade de competências disponíveis são [nao verificado]. (`app/ponto/services/fechamento.py:382`, `app/ponto/janelas.py:54`, `app/folha/export.py:556`, `app/ponto/services/fechamento.py:1473`)

Esboço de orquestração, **não executado** [nao verificado]; os contextos representam interceptações somente em memória:

```python
from contextlib import nullcontext
from unittest.mock import patch
from django.db import transaction

from ponto import turnos
from ponto.services.fechamento import (
    recalcular_fechamento_mes,
    universo_da_competencia,
    competencia_trancada,
)
from folha.export import competencia_entregue

class RollbackSonda(Exception):
    pass

def apurar(empresa, mes, ano, ids):
    objetos = recalcular_fechamento_mes(
        mes, ano,
        empresa_id=empresa.pk,
        colaborador_ids=ids,
        somente_leitura=True,
        retornar_objeto=True,
        lavrar_dias_pagos=False,
    )
    if set(objetos) != set(ids):
        raise RuntimeError("apuracao incompleta")
    return objetos

# Exemplo: isolar apenas o resgate O95 já presente nesta cópia.
def contexto_antes():
    return patch.object(
        turnos, "_fechar_aberto_na_pausa_sem_volta",
        lambda lista, *args, **kwargs: lista,
    )

def contexto_depois():
    return nullcontext()  # executa a implementação real da cópia

for empresa, mes, ano in competencias_no_escopo:
    if competencia_entregue(empresa, mes, ano):
        continue
    if competencia_trancada(empresa.pk, mes, ano):
        continue

    ids = list(universo_da_competencia(
        mes, ano, empresa.pk,
    ).values_list("pk", flat=True))

    try:
        with transaction.atomic():
            with contexto_antes():
                antes = apurar(empresa, mes, ano, ids)
                foto_antes = fotografar_sem_dados_pessoais(antes)

            with contexto_depois():
                depois = apurar(empresa, mes, ano, ids)
                foto_depois = fotografar_sem_dados_pessoais(depois)

            raise RollbackSonda()
    except RollbackSonda:
        acumular_diff(foto_antes, foto_depois)

imprimir_agregados_e_colNNN()
```

`fotografar_sem_dados_pessoais`, `acumular_diff` e `imprimir_agregados_e_colNNN` são apenas o encanamento proposto da sonda, não funções existentes da aplicação. A interceptação acima mede **o resgate atual ligado versus desligado**, não reconstrói integralmente o código histórico de setembro. [nao verificado]

A fotografia deve conter:

- Campos numéricos dos fechamentos, incluindo horas trabalhadas, folga trabalhada, HE, noturnas, intra, descontos, DSR e banco; o conjunto já usado pelo serviço está em `CAMPOS`. Não somar indiscriminadamente parcelas que se sobrepõem. (`app/ponto/management/commands/aplicar_09_corte_b.py:67`, `app/folha/export.py:424`)
- Classificação real por `classificar_export(..., fechamentos=objetos)`, retendo somente identificador `colNNN`, estado e indicadores necessários; o retorno cru contém nome e código. (`app/folha/export.py:765`, `app/folha/export.py:770`)
- Rubricas emitíveis por `montar_linhas_export(..., itens=classificacao, fechamentos=objetos)` e `agregar_export_dominio`. Para preservar atribuição por colaborador mesmo com matrícula repetida, montar por objeto, usando a classificação do lote; descartar matrícula e TXT antes da saída. (`app/folha/export.py:690`, `app/folha/export.py:714`, `app/folha/export.py:725`, `app/folha/export.py:310`)
- Eventos antes da retenção, por `eventos_do_fechamento`, para distinguir alteração de cálculo de alteração de entrada no TXT. Valores das rubricas devem conservar unidade do catálogo ou formato cru; nem todo valor é hora. (`app/folha/export.py:402`, `app/folha/export.py:241`)

Já existe `diff_frota.medir`: compara **gravado / atual / novo**, chama o recálculo real e desfaz a transação. Porém seu universo nasce dos fechamentos existentes; além disso, uma interceptação global mantida pela mutação contaminaria o “atual” da empresa seguinte. Para reutilizá-lo, limitar cada chamada a uma empresa e restaurar a interceptação fora dela. (`app/ponto/services/diff_frota.py:62`, `app/ponto/services/diff_frota.py:72`, `app/ponto/services/diff_frota.py:84`)

A saída deve separar efeito `depois−antes`, deriva `antes−gravado` e diferença `depois−gravado`, com totais e quantidades de aumentos/reduções por campo e rubrica. Essa separação já existe em `diff_frota.somar`. Ausência no retorno deve ser falha de cobertura, pois o recalculador captura erro por colaborador e continua. (`app/ponto/services/diff_frota.py:101`, `app/ponto/services/fechamento.py:1303`)

A futura execução também precisa impedir escrita de handlers de log e emissão de dados pessoais: o ramo de erro registra nome. `atomic+raise` não resolve esse canal; sua configuração efetiva aqui é [nao verificado]. (`app/ponto/services/fechamento.py:1306`)

As comparações candidatas, sem escolher a cura, são:

| Candidata | O que o DIFF mediria |
|---|---|
| **Exigir volta `E` dentro de `intra_max_s` na absorção**, proposta pelo anexo | Mudanças de fechamento, divisão de turnos, pausas, horas e rubricas; também mudanças no almoço ainda em curso. O efeito produtivo da proposta é [nao verificado]. O teste existente exige conservar aberto quando só há entrada e saída para pausa. (`app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:80`) |
| **Conservar absorção e resgatar após fato posterior**, implementação presente | Efeito exclusivo da pós-passagem, diferenças entre janela curta e competência, tetos e preservação do intervalo com volta. Há testes definidos para esses comportamentos; execução nesta análise é [nao verificado]. (`app/ponto/turnos.py:1106`, `app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:58`, `app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:89`, `app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:111`) |

5. **Para reproduzir um total mensal do col727, a função apropriada é `recalcular_fechamento_mes`, com `colaborador_ids=[727]`, no modo leitura.** `parear_turnos` reproduz geometria; sozinho não reproduz horas e rubricas do fechamento. (`app/ponto/services/fechamento.py:448`, `app/ponto/services/fechamento.py:902`, `app/ponto/adaptador_motor.py:13`)

**Os `−38,02 h` históricos não são reproduzíveis com o material fornecido.** Faltam os insumos completos e a identificação dos dois cenários efetivamente comparados. [nao verificado]

Há ainda três números que não devem ser fundidos:

- O anexo informa `135,47 → 159,49 h`: diferença aritmética de **24,02 h**; o referencial dos `38,02 h` não foi fornecido. [nao verificado]
- O comentário do teste informa `135,47 → 163,46 h`: diferença de **27,99 h**, e menciona `38,02 h em 12 turnos`. Isso comprova o conteúdo do comentário, não uma medição reproduzida. (`app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:7`)
- O teste do col727 verifica o fechamento em `15:00` com batida posterior; não verifica o total mensal de `38,02 h`. (`app/ponto/tests/test_o95_pausa_so_absorve_quem_voltou.py:58`)

## 15/11 20:08 · sessao 1ba0d1600695 · CORTE-JUIZ-RESPONDIDA · leitura · origem=Code

O principal desacordo está na retenção: hoje ela aceita **instante OU texto**; a fila global já exige **instante E texto**. A ressuscitação ainda limpa somente o instante. (`app/chamados/models.py:1907`; `app/chamados/juizes.py:1725`; `app/chamados/models.py:1613`.)

As **143 vivas são a previsão do aval**, não uma medição apresentada de todos os resultados que mudarão. O anexo prevê duas abertas, `38251/38252`, e 141 entregues; não fornece a distribuição dos demais estados nem dos leitores. (`aval.txt:1`, anexo fornecido; contagens efetivamente observadas: **[nao verificado]**.)

Entrega abaixo, sem construção. Correspondência desta árvore com o HEAD/hash informado, execução dos testes e execução da sonda em produção: **[nao verificado]**. Nas referências à c9, a linha indicada é do **lado novo do diff fornecido**; os arquivos integrais daquela cópia não foram abertos: **[nao verificado]**. [nao verificado]

**Convenções para (a) e (b)**

`I` = instante presente; `T` = texto não vazio após o tratamento já existente, `(texto or '').strip()`. `J` designa o juiz novo especificado no corte: `I ∧ T`. A face SQL atual `q_respondida_com_texto()` já expressa esse par, incluindo os brancos reconhecidos pelo Python. (`aval.txt:1`, anexo; `app/chamados/juizes.py:1523`; `app/chamados/juizes.py:1539`; `app/chamados/juizes.py:1559`.)

| Marca usada nas tabelas | Estado de divergência do componente “respondida” |
|---|---|
| **Δ0** | Nenhuma diferença direta: já usa `I ∧ T`. (`app/chamados/juizes.py:1559`.) |
| **ΔB** | `I ∧ ¬T` **e** `¬I ∧ T`: o OU atual dá verdadeiro; J dá falso. (`app/chamados/models.py:1907`.) |
| **ΔC** | `I ∧ ¬T`: o instante sozinho dá verdadeiro; J dá falso. (`app/chamados/models.py:3641`.) |
| **ΔD** | `¬I ∧ T`: o texto sozinho dá verdadeiro; J dá falso. (`app/chamados/services/fio.py:1271`; `aval.txt:1`, anexo.) |
| **Δcru** | Texto sem instante e texto composto somente de brancos; depende de o sítio usar verdade crua ou comparação com `''`. (`app/colaboradores/services/calendario.py:474`; `app/templates/chamados/partials/_card_atendimento.html:141`.) |
| **Δestoque** | Depois da correção autorizada do estoque, leitores de dados passam a ver instante nulo e texto vazio nos alvos efetivamente corrigidos. Validadas/materializadas ficam preservadas. (`aval.txt:1`, anexo.) |
| **Outro** | O sítio lê conteúdo, cronologia, integridade, efeito ou trilha; não se substitui esse predicado inteiro por J. Exemplos: parse de hora, fotografia anterior e detecção de par ilegal. (`app/chamados/juizes.py:1900`; `app/chamados/services/disputa_emissao.py:1632`; `app/chamados/juizes.py:1531`.) |

As marcas indicam a diferença **local**. Filtros de vitalidade, validação, materialização, tranca, prazo, identidade e agregação podem impedir uma mudança no resultado externo. Particularmente, `q_vivas()` declara continência do C6, não igualdade. (`app/chamados/models.py:1905`; `app/chamados/models.py:3436`; `app/chamados/models.py:3439`.)

**(a) Usos que migram, por classe do censo**

Os identificadores R abaixo correspondem aos usos enumerados no anexo `censo.md`. Os sítios de código relacionados foram conferidos na cópia; a existência do arquivo físico `logs/JUIZ-RESPONDIDA/censo.md` nesta árvore: **[nao verificado]**.

**Classe A — instante E texto**

| R | Sítio e finalidade | Hoje → leitura da F1; divergência |
|---|---|---|
| 13 | C6, vitalidade. (`app/chamados/juizes.py:1403`; `app/chamados/juizes.py:1420`.) | `I∧T` → J sobre a projeção temporal; **Δ0**. Preserva os demais degraus. |
| 16 | Face SQL do par. (`app/chamados/juizes.py:1554`; `app/chamados/juizes.py:1559`.) | Aproveitar como face SQL de J; **Δ0**. |
| 19 | Fila global a validar. (`app/chamados/juizes.py:1725`; `app/chamados/juizes.py:1728`.) | Já usa face SQL e confirma C6; passa a nomear a autoridade nova; **Δ0**. |
| 29 | Auditor I21. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`.) | `I ∧ texto!=''`, sem trim → J SQL. Difere em **instante + brancos**, dentro dos demais filtros. |
| 52 | Degrau da resposta de `q_vivas`. (`app/chamados/models.py:3454`; `app/chamados/models.py:3468`.) | Já delega à face SQL; **Δ0**. |
| 79 | Resolvedor por chamado: preservação para materialização. (`app/chamados/services/disputa_emissao.py:2000`; `app/chamados/services/disputa_emissao.py:2001`.) | Cópia de `I∧T` → J; mantém a exigência de não materializada de fato; **Δ0**. |
| 96 | Janela visível do colaborador. (`app/chamados/services/janela_colab.py:63`.) | Herda a migração de `q_vivas`; **Δ0** no par. |
| 110 | Perguntas do app. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/services/pergunta_no_app.py:61`.) | Herda J SQL/C6; **Δ0** no par. |
| 112 | Crédito/contador da mesa, lendo tuplas. (`app/chamados/services/raiox.py:138`; `app/chamados/services/raiox.py:143`; `app/chamados/services/raiox.py:144`.) | Cópia de `I∧T` → J mediante adaptação dos valores carregados; **Δ0**. |
| 125 | Vigia de viva em disputa fechada. (`app/chamados/services/vigia_fio.py:769`.) | Herda J SQL no pré-filtro; **Δ0** no par. |
| 141 | Alimentação do fecho do fio. (`app/ponto/services/veredito_celula.py:181`.) | Herda J SQL; **Δ0** no par. |

**Classe B — instante OU texto**

O corpo comum é `¬validada ∧ ¬materializada ∧ (I∨T)`, condicionado também à tranca. A migração troca **somente** `(I∨T)` por J; seus delegados continuam perguntando ao juiz de retenção. Todos os usos abaixo têm **ΔB**, quando satisfeitos seus respectivos universos. (`app/chamados/models.py:1905`; `app/chamados/models.py:1907`; `app/chamados/models.py:1909`.)

| R | Sítio | Finalidade e destino |
|---|---|---|
| 24 | `app/chamados/juizes.py:2079` | Irmãs sem consumo/retenção → corpo comum com J. |
| 27 | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` | Auditor I12 → delegado de irmãs. |
| 38 | `app/chamados/management/commands/validar_classe_a.py:54` | Fila do comando → lista sem veredito. |
| 42 | `app/chamados/models.py:1907` | **Corpo que muda**: `sem_veredito_entre` → J. |
| 43 | `app/chamados/models.py:1889` | Lista sem veredito da disputa → mesmo corpo. |
| 44 | `app/chamados/models.py:1917` | Pode fechar → mesma lista, além da ausência de fio vivo. |
| 45 | `app/chamados/models.py:1950` | Fechamento → mesma lista impeditiva. |
| 60 | `app/chamados/reconciliador.py:448` | Retenção da retratação → irmãs. |
| 62 | `app/chamados/reconciliador.py:1164` | Encerramento de órfãs → lista sem veredito. |
| 69 | `app/chamados/services/competencia_trancada.py:126` | Medida de retenção, ignorando tranca → mesmo corpo. |
| 80 | `app/chamados/services/disputa_emissao.py:2030` | Proteção/reabertura do pai → irmãs. |
| 85 | `app/chamados/services/fila_por_causa.py:226` | Gaveta da fila → lista sem veredito. |
| 104 | `app/chamados/services/materializacao.py:768` | Retenção do destino durante materialização → irmãs. |
| 133 | `app/colaboradores/services/desligamento.py:165` | Desligamento → lista sem veredito. |
| 134 | `app/colaboradores/services/ficha.py:169` | Ficha → lista sem veredito, intersectada com vivas. |
| 142 | `app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574` | Retenção do CSF nos universos pai/destino → mesmo corpo, ignorando tranca. |

Retenção deixar de ocorrer **por resposta** não prova que o fechamento passará: C7 também exige ausência de fio vivo. (`app/chamados/models.py:1917`.)

**Classe C — só instante**

Salvo as exceções históricas expressamente identificadas, o componente `I` migra para J e `¬I` para `¬J`; consultas usam a face SQL, decisões sobre instâncias usam J. A divergência local é **ΔC**. (`app/chamados/models.py:3641`; `app/chamados/juizes.py:1559`.)

| R | Sítio | Predicado atual, finalidade e destino |
|---|---|---|
| 4 | `app/api/views.py:2577` | I seleciona respondidas no histórico → J; partição do app. |
| 5 | `app/api/views.py:2620` | I informa `ja_respondida` → J; correção/reenvio. |
| 18 | `app/chamados/juizes.py:1718` | I + lastro/vitalidade → J SQL + mesmos limites; corrigíveis. |
| 25 | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` | Falta de I integra contraprova I21a → falta de J. |
| 26 | `app/chamados/management/commands/auditar_invariantes_chamados.py:592` | I em disputa aberta define piso do I2 → J. |
| 30 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1603` | ¬I no auditor C2 → ¬J SQL, conservando os demais filtros. |
| 49 | `app/chamados/models.py:3128`; `app/chamados/models.py:3131` | I distingue pendente/respondida → J. As precedências de fase continuam em `app/chamados/models.py:3124`. |
| 56 | `app/chamados/models.py:3641` | Property `respondida=I` → delegação a J. |
| 58 | `app/chamados/models.py:4067`; `app/chamados/models.py:4074`; `app/chamados/models.py:4077` | **Preservação histórica:** I gera ato conservado e copia texto. Não apagar fotografia legada por ela não satisfazer J; separar isso da classificação corrente. |
| 64 | `app/chamados/services/acoes_disputa.py:95` | Conta ¬I para reabertura → conta ¬J SQL. |
| 65 | `app/chamados/services/acoes_disputa.py:130` | Viva ¬I impede regularização → viva ¬J. |
| 67 | `app/chamados/services/becos.py:30` | ¬I com destino não resolvido → ¬J; diagnóstico de órfã. |
| 71 | `app/chamados/services/declaracao_texto.py:419` | I informa reenvio → J. |
| 72 | `app/chamados/services/declaracao_texto.py:687` | ¬I dispara gravação da declaração → ¬J. |
| 76 | `app/chamados/services/disputa_emissao.py:1734` | Conta ¬I para recobrança → ¬J SQL. |
| 77 | `app/chamados/services/disputa_emissao.py:1842` | I exclui porta de reabrir credor → J; conserva C6 e demais cercas. |
| 89 | `app/chamados/services/fio.py:698` | Viva ¬I impede regularização → viva ¬J. |
| 94 | `app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:106` | **Evento histórico:** instante e `desde` selecionam ocorrência. Preservar a cronologia; não transformar evento anterior em classificação corrente nem remover trilha pelo novo J. |
| 107 | `app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1379` | I + validada + não materializada → J SQL + mesmos filtros; entrada de materialização. |
| 111 | `app/chamados/services/pergunta_no_app.py:81` | ¬I + disputa aberta define pendente → ¬J. |
| 116 | `app/chamados/services/validacao.py:418` | ¬I retorna `sem_resposta` → ¬J. |
| 117 | `app/chamados/services/validacao.py:605` | ¬I retorna `ja_pendente` → ¬J. |
| 121 | `app/chamados/services/verbos.py:335`; `app/chamados/services/verbos.py:342`; `app/chamados/services/verbos.py:345` | I recente/posterior à cobrança → J SQL + limites temporais; contador por pessoa. |
| 123 | `app/chamados/services/vigia_fio.py:694` | I sob âncora terminal define causa c → J, mantendo a precedência diagnóstica anterior. |
| 128 | `app/chamados/views.py:1280` | I informa `ja_respondida` → J. |
| 129 | `app/chamados/views.py:1416`; `app/chamados/views.py:1417` | ¬I + C6 define pendentes vivas → ¬J + C6. |
| 130 | `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176` | Vivas I para validar / ¬I para colaborador → J / ¬J. |
| 137 | `app/ponto/management/commands/reconciliar_grade.py:461` | I pré-filtra antes de fio vivo → J SQL. |
| 143 | `bin/censo_folha.py:28`; `bin/censo_folha.py:29` | Contadores I/¬I → J SQL/negação, conservando filtros de disputa/validação. |
| 148 | `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29` | I governa rótulo/caixa → property/contexto que delega a J. |
| 150 | `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217` | I governa correção/caixa → property/contexto que delega a J. |

A proteção do estoque consolidado não torna todos esses leitores invariantes: a property `respondida` lê qualquer fase, e o lote de materialização seleciona explicitamente perguntas validadas. Já `estado` pode conservar o rótulo pela precedência VALIDADA/RESOLVIDA. (`app/chamados/models.py:3641`; `app/chamados/services/materializacao.py:1377`; `app/chamados/models.py:3124`.)

**Classe D — só texto**

Onde T está sendo usado para afirmar “respondida”, passa a J, com **ΔD**. Guardas de conteúdo, preservação e apresentação continuam lendo o texto para responder à sua pergunta própria. (`app/chamados/services/fio.py:1271`; `app/chamados/services/materializacao.py:835`; `app/chamados/templatetags/chamados_tags.py:142`.)

| R | Sítio | Uso atual, finalidade e destino |
|---|---|---|
| 8 | `app/chamados/canal.py:65`; `app/chamados/canal.py:72` | SQL texto=`''` procura resposta no fio. Diagnóstico de conteúdo, não J completo; ampliar para “não respondida” exigiria ¬J e alcançaria também texto sem instante/brancos. Resultado dependente do fio: **[nao verificado]**. |
| 31 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1655` | `any(T)` interrompe auditor F1 → J quando a exceção significa resposta recebida; **ΔD**. |
| 34 | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:506` | Carrega texto para eleição canônica → alimentar também I e delegar a autoridade usada por R90; **ΔD** no critério de resposta. |
| 37 | `app/chamados/management/commands/dedup_perguntas_fato.py:75` | T dá preferência na deduplicação → J no critério “respondida”; **ΔD**. |
| 40 | `app/chamados/models.py:1533` | Texto NULL/`''` restringe universo de ressuscitação. **Seleção de alvos**, não resposta corrente. Zerar integralmente os selecionados não implica ampliar automaticamente esse universo para todo ¬J. |
| 59 | `app/chamados/reconciliador.py:365` | T impede lavratura da órfã → J no impedimento por resposta; **ΔD**. |
| 61 | `app/chamados/reconciliador.py:1083` | T impede veredito da órfã → J no impedimento por resposta; **ΔD**. |
| 74 | `app/chamados/services/disputa_emissao.py:1663` | T impede sobrescrever resposta automática. Guarda de preservação do conteúdo; não autorizar sobrescrita de texto legado apenas porque J é falso. |
| 81 | `app/chamados/services/disputa_emissao.py:2078`; `app/chamados/services/disputa_emissao.py:2086` | T rotula auditoria “com resposta” → J se o rótulo afirma respondida; **ΔD**. O carimbo permanece na porta de `app/chamados/services/disputa_emissao.py:2081`. |
| 90 | `app/chamados/services/fio.py:1271` | T prefere canônica respondida → J; **ΔD**. |
| 91 | `app/chamados/services/fio.py:1283` | T + via admin protege mérito → J para a parcela “respondida”, conservando via/admin; **ΔD**. |
| 99 | `app/chamados/services/materializacao.py:438` | T + tipo hora permite apresentar estado de aplicação → J como entrada; conserva interpretação do conteúdo; **ΔD**. |
| 105 | `app/chamados/services/materializacao.py:835`; `app/chamados/services/materializacao.py:842` | ¬T recusa plantio. Entrada por J passa a recusar também `¬I∧T`; a guarda de conteúdo continua pertinente; **ΔD** na elegibilidade. |
| 126 | `app/chamados/templatetags/chamados_tags.py:142` | ¬T devolve leitura vazia; apresentação do conteúdo. **Outro**, além de **Δestoque**. |
| 132 | `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/calendario.py:476` | SQL texto!=`''` e C6 → J SQL + C6; **Δcru**. |
| 135 | `app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/ficha.py:171` | ¬T e ausência do conjunto `falou` → J/¬J junto da migração B; os dois componentes precisam permanecer coerentes. **ΔB/ΔD**. |
| 136 | `app/folha/previa.py:142` | `any(T)` como evidência de fala. Se usado para afirmar pergunta respondida, J, **ΔD**; o conteúdo anterior continua evidência histórica. |
| 138 | `app/ponto/services/cartorio.py:1641`; `app/ponto/services/cartorio.py:1648` | `any(T)` protege conversa no cartório → J no impedimento por resposta; **ΔD**. |
| 145 | `app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:182` | Verdade crua do texto governa apresentação. Rótulo de resposta recebida usa J; mera exibição conserva conteúdo. **Δcru** no primeiro caso; **Δestoque** no segundo. |

**Classe E — outros predicados e cópias**

| R | Sítio e finalidade | Encaminhamento e divergência |
|---|---|---|
| 2 | Correção/reenvio pelo dono da espera. (`app/api/views.py:1179`.) | Delegado de `aguarda_o_colaborador`; migra sua dependência para ¬J; **Δ0** na expressão equivalente atual. |
| 9 | Slot, teto temporal e parse. (`app/chamados/juizes.py:351`; `app/chamados/juizes.py:363`.) | J na elegibilidade de resposta; teto/parse continuam próprios. Texto sem instante pode perder elegibilidade; slot final: **[nao verificado]**. |
| 10 | Chave de memo, com os dois valores. (`app/chamados/juizes.py:855`; `app/chamados/juizes.py:856`.) | **Outro**: conservar insumos de invalidação; **Δestoque** na chave. |
| 11 | Concordância da hora declarada com a luz. (`app/chamados/juizes.py:1359`; `app/chamados/juizes.py:1371`; `app/chamados/juizes.py:1376`.) | **Outro**: interpretação permanece; o degrau chamador lê J. |
| 12 | Projeção por horizonte. (`app/chamados/juizes.py:1388`; `app/chamados/juizes.py:1393`.) | **Outro**: conservar projeção temporal; J recebe o par projetado. |
| 14–15 | Integridade `I∧¬T`, Python e SQL. (`app/chamados/juizes.py:1531`; `app/chamados/juizes.py:1551`.) | **Outro**: continuar identificando essa anomalia. `¬J` também inclui a pendente legal e não substitui esse detector. |
| 17 | Exclusão da anomalia na porta C17. (`app/chamados/juizes.py:1706`.) | Não trocar mecanicamente o detector por J/¬J; alcance depende da partição. **Δestoque** elimina a anomalia nos alvos corrigidos. |
| 20 | Reconhece declaração de hora pelo conteúdo. (`app/chamados/juizes.py:1753`; `app/chamados/juizes.py:1756`.) | Conservar interpretação; quando afirma declaração respondida, entrada por J. `¬I∧T` pode deixar de qualificar. |
| 21–23 | Delta, classe e plausibilidade. (`app/chamados/juizes.py:1900`; `app/chamados/juizes.py:2004`; `app/chamados/juizes.py:2026`.) | **Outro**: conservar parsers; J governa a entrada das filas/validação. |
| 28 | Auditor I20: ¬I e texto!=`''`, com exclusões nominais. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`.) | Integridade, não “respondida”. Deve distinguir texto após trim de brancos; não usar ¬J sozinho. |
| 32–33 | Seleção e cópia de projeções para trilha. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:60`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:326`.) | **Outro**: preservar fotografia histórica; não excluir assinatura anterior por J falso. |
| 35 | Busca literal da resposta na nota. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:32`.) | **Outro**: correspondência de conteúdo/trilha. |
| 36 | Seleção de limbo ¬I e texto!=`''`. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.) | Integridade/cura; distinguir `¬I∧T` de brancos. O comando de cura não é leitor de resposta corrente. |
| 41 | Carrega par anterior à ressuscitação. (`app/chamados/models.py:1610`; `app/chamados/models.py:1611`.) | **Outro**: preservar anterior no ato; escrita muda no item (2). |
| 46–47 | Texto em SILENCIO/RETRATACAO. (`app/chamados/models.py:1990`; `app/chamados/models.py:2122`.) | **Outro**: fotografia histórica permanece. |
| 48 | Guarda do nascimento. (`app/chamados/models.py:2922`; `app/chamados/models.py:2923`.) | Expandir recusa de `I∧¬T` para as duas metades ilegais; J sozinho não distingue pendente legal de par ilegal. |
| 50 | Foto e preenchimento de instante pelo carimbo. (`app/chamados/models.py:3210`; `app/chamados/models.py:3213`; 

[truncado em 20000 de 61522 caracteres]

## 15/11 20:08 · sessao 1afa94a6b5f9 · ORDEM-VIVA-1511-NOITE · verificador · origem=Code

**1(a) — PROCEDE: “os cinco estão em B3, estacionamento, !, lei ou desenho” não confere integralmente.** O topo repete essa situação, mas há registros contrários na própria cópia: BO-0611-05 teve B3 liberado e pousou; E4b também pousou. Portanto, “lista sem abertura imediatamente liberada” não equivale à enumeração literal apresentada. (`app/docs/RELATO.md:4`, `app/docs/RELATO.md:1350`, `app/docs/RELATO.md:1351`, `app/docs/BACKLOG.md:2000`)

- BO-0611-05: **PROCEDE**, pois consta **NO AR**, merge `fb9269ea9`; a linha B3 do BACKLOG está vencida. (`app/docs/RELATO.md:1350`, `app/docs/BACKLOG.md:2004`)
- BO-DISPUTA-SEM-PERGUNTA: **CONFERE**, estacionada após 25 voltas, exigindo matriz conferida e aval. (`app/docs/BACKLOG.md:2005`)
- E4b: **CONFERE quanto às pendências**, mas o código já pousou; o apply dos 24 tem dependência própria de COMPETENCIA-PAGA, e o apply das 39 exportadas continua em `!`. (`app/docs/BACKLOG.md:2000`, `app/docs/BACKLOG.md:1942`, `app/docs/RELATO.md:4`)
- E4c: **CONFERE**, F1 nova em pré-desenho, construção somente por aval novo; F2–F9 dependem do pouso da F1. (`app/docs/BACKLOG.md:1945`, `app/docs/RELATO.md:17`)
- E4d: **CONFERE quanto à dependência**, pois Contract sucede a migração dos leitores; não está demonstrada como construção liberada. (`app/docs/BACKLOG.md:1991`)

**1(b) — CONFERE a frase; PROCEDE tomar prioridade como liberação.** “Construtor na 1a vaga após E3, na frente da LASTRO” existe, mas a mesma linha declara **PARADA até corte do desenho**. O pré-desenho foi entregue; a escolha entre selo do valor e acessor tipado continua **LU2-03 ABERTA**. O corte anterior diz “4a volta NAO abre” e mantém a trava atrás de **E3, DESLIGADO, FERIADO, O168 e HJ-090**. (`app/docs/BACKLOG.md:1891`, `app/docs/RELATO.md:716`, `app/docs/RELATO.md:717`, `app/docs/RELATO.md:63`)

O censo de 12/11 registra ATA **12 chaves/95 acessos/27 arquivos**, DNA **9/41/19**. Reexecutando a ferramenta somente em leitura nesta cópia, ATA permanece **12/95/27**, mas DNA resulta **9/51/20**. São eventos reconhecidos por AST, como cota inferior, sem prova de completude; a contagem histórica de DNA não representa mais esta cópia. (`app/docs/RELATO.md:808`; cálculo em `bin/leitores_de_chave.py:817`, `bin/leitores_de_chave.py:852`; limites em `bin/leitores_de_chave.py:4`)

**1(c) — CONFERE a existência da L-119 e da citação da ref; PROCEDE atribuir à lei a retomada dessa ref.** A L-119 diz que obra interrompida por bug P7.1 volta automaticamente ao topo quando o interruptor pousa, com registro no BACKLOG e medição no ESTADO. Não nomeia BUG-ENC nem qualquer ref de rescaldo. (`app/docs/LEIS.md:140`, `app/docs/CORTES.md:319`)

A ref `refs/rescaldo/1211/wt-e4carga-155931 @a2cd7981d` está expressamente citada na linha BUG-ENC, junto da interrupção por F2 e das pendências. O rescaldo registrou as 17 paradas como interrompidas, declarando **“ordem intocada”**. A existência/resolução efetiva da ref no Git é **[nao verificado]**. (`app/docs/BACKLOG.md:988`, `app/docs/RELATO.md:851`)

**1(d) — PROCEDE: a linha ABERTA de 08/11 está vencida.** Há prova documental positiva: **“PRONTO FIO-E0-V4-E1 NO AR 22:46”**, deploy `a8a692f5`; merge7 certificada com suite e sombra; resíduos encaminhados à E1-FILA, depois também pousada e registrada NO AR. (`app/docs/BACKLOG.md:1964`, `app/docs/RELATO.md:1674`, `app/docs/RELATO.md:1706`, `app/docs/RELATO.md:1679`, `app/docs/RELATO.md:1662`)

O código contém a migration E1 e a cura de classe v15g/MATRIZ-TODAS-AS-PORTAS. Isso sustenta entrega com resíduos desdobrados, não uma obra original ainda aguardando construção. Histórico Git independente e estado atual de produção: **[nao verificado]**. (`app/chamados/migrations/0051_decisao_pergunta.py:1`, `app/chamados/models.py:2373`)

**1(e) — CONFERE a natureza de instrumento; PROCEDE o delta literal.** O contador atual é explicitamente **“medida, nao escritor nem novo juiz”**, mas já existe como `cluster_encerrado_sem_lastro` e já alimenta o vigia. (`app/ponto/services/cluster_espurio.py:88`, `app/ponto/services/cluster_espurio.py:91`, `app/ponto/management/commands/detectar_cluster_espurio.py:92`)

O pedido antigo `fechados_por_idade` ficou vencido: o RELATO explica que idade não tinha fato registrado na fonte. Também registra pouso v5, apply dos três casos e DRY posterior vazio. Logo, não está demonstrado um delta novo pendente para construir. (`app/docs/BACKLOG.md:2121`, `app/docs/RELATO.md:1439`, `app/docs/RELATO.md:1164`)

**2 — PROCEDE nos seguintes pontos de compatibilidade literal.**

- **A lista substituída já recebeu substituições posteriores.** PLANO-JANELA-POS-RESET substitui ORDEM-POS-E4A2 do E4b em diante; ORDEM-ATE-O-ZERO substitui a ordem desse plano; ESPINHA-NA-FRENTE estabelece depois as vagas R3/F1. Assim, “substitui só a lista de 10/11” não identifica integralmente a ordem posterior que será deslocada. (`app/docs/BACKLOG.md:1839`, `app/docs/BACKLOG.md:1989`, `app/docs/BACKLOG.md:2216`)
- **F1 nova:** o aval vigente CORTE-JUIZ-RESPONDIDA exige **“construcao so por aval novo citando a entrega”**. Existirem corte e pré-desenho não satisfaz essa exigência. Há colisão se a nova frase for usada como autorização automática de construção. (`app/docs/BACKLOG.md:1945`, `app/docs/RELATO.md:4`)
- **TIPO:** a mudança de ordem não encerra LU2-03 nem libera desenho/B3; a regra vigente manda parar essa obra e seguir as outras. (`app/docs/RELATO.md:63`, `CLAUDE.md:274`, `CLAUDE.md:294`)

**CONFERE** manter leituras no Codex: QUEIMA já foi encerrada por POS-REDEFINICAO-1935. **CONFERE** teto 2, desde que global: instrumento não constitui terceira vaga de construtor. (`app/docs/RELATO.md:25`, `CLAUDE.md:291`)

**3 — PROCEDE nas condições que ainda não fecham.**

- **“Não cruza arquivo com R3 c17 nem com a F1”:** R3 tem pré-desenho entregue, mas c17 ainda exige aval novo; F1 continua com pré-desenho rodando e padrão a definir. Ausência de cruzamento com as construções futuras: **[nao verificado]**. O critério vigente é arquivo em comum. (`app/docs/RELATO.md:22`, `app/docs/BACKLOG.md:1945`, `CLAUDE.md:309`)
- **“O95 DIFF de frota”:** falta identificar qual alteração será comparada. Existe candidata escrita no BACKLOG, revertida; existe outra cura O95 no código, que preserva absorção e fecha pela existência de fato posterior. Portanto, “sem cura escrita” seria incorreto, mas o delta pretendido permanece indeterminado. O motor de DIFF distingue GRAVADO/HOJE/NOVO e exige uma mutação. (`app/docs/BACKLOG.md:1426`, `app/ponto/turnos.py:1119`, `app/ponto/turnos.py:1171`, `app/ponto/turnos.py:1502`, `app/ponto/services/diff_frota.py:7`, `app/ponto/services/diff_frota.py:45`)
- **FERIADO “termina a medida”:** a medida já está entregue e encontrou ENTREGUE_FORA=1; o ramo correspondente está declarado **não construído**, com AVAL-DEVOLVIDO preservado. Não há liberação implícita dessa construção. (`app/docs/RELATO.md:7`, `app/docs/RELATO.md:8`)

**4 — CONFERE para as leituras declaradas; PROCEDE qualquer extensão a apply.**

O95 alcança dinheiro: altera fechamento de turno, e o motor consome `parear_turnos`. O DIFF pode calcular esse efeito em transação desfeita; isso não autoriza persistência. Efeito monetário atual na frota: **[nao verificado]**. (`app/ponto/turnos.py:1186`, `app/ponto/motor_calculo_v2.py:503`, `app/ponto/services/diff_frota.py:84`, `app/ponto/services/diff_frota.py:89`)

O169 somente contador é leitura; `--apply` e `--legado --apply` são caminhos distintos que escrevem. VINCULO somente censo também não executa a autorização histórica de alteração retroativa. (`app/ponto/management/commands/detectar_cluster_espurio.py:55`, `app/ponto/management/commands/detectar_cluster_espurio.py:69`, `app/ponto/management/commands/detectar_cluster_espurio.py:80`, `app/docs/BACKLOG.md:1182`)

BUG-ENC declara mudança de veredito do cartório, e F1 registra **141 entregues com `!` próprio**. A nova ordem não comprova ausência de efeitos nem substitui esses avais; exportada e vínculo/escala continuam sujeitos às restrições vigentes. (`app/docs/BACKLOG.md:988`, `app/docs/BACKLOG.md:1945`, `CLAUDE.md:265`, `CLAUDE.md:411`)

**Marcador — CONFERE: escritor manual, juiz único no hook.** O valor é escrito **à mão**; `gerar_backlog.py` preserva e republica essa janela. O medidor de interrompidas alarma e não move o marcador. (`bin/gerar_backlog.py:113`, `bin/gerar_backlog.py:119`, `bin/gerar_backlog.py:138`, `bin/interrompidas_estado.py:31`)

Quem interpreta diretamente é `hook_stop_fila1.py::_item_da_ordem_viva`. Consomem essa resposta o selo O158, handoff, medidor de interrompidas e `backlog_estado` — este pela função `_proximo_da_fila`. (`bin/hook_stop_fila1.py:209`, `bin/tests/test_hook_nao_cobra_congelado.sh:111`, `bin/handoff_sessao.sh:64`, `bin/interrompidas_estado.py:93`, `bin/backlog_estado.py:56`)

**CONFERE: o id precisa existir no bloco OBRAS e ser aceito como aberto pelo juiz.** A correspondência do id é exata; inexistente retorna `ausente`, fechado/parado retorna `parado`. O selo rejeita ambos. Não exige a palavra literal `ABERTA` nem que seja a primeira linha da tabela. (`bin/hook_stop_fila1.py:213`, `bin/hook_stop_fila1.py:223`, `bin/hook_stop_fila1.py:225`, `bin/tests/test_hook_nao_cobra_congelado.sh:82`, `bin/tests/test_hook_nao_cobra_congelado.sh:122`)

**PROCEDE quanto ao id novo nesta cópia:** o marcador ainda aponta ORDEM-POS-E4A2; não há linha `ORDEM-VIVA-1511-NOITE` no bloco OBRAS inspecionado. Apenas trocar o comentário para esse id produziria `ausente` no juiz e vermelho no selo. Isso não equivale a garantir Stop liberado, pois o hook possui outras condições. (`app/docs/BACKLOG.md:8`, bloco delimitado em `app/docs/BACKLOG.md:713` e `app/docs/BACKLOG.md:2218`; `bin/hook_stop_fila1.py:228`, `bin/tests/test_hook_nao_cobra_congelado.sh:122`, `bin/hook_stop_fila1.py:1394`)

## 15/11 20:07 · sessao fc9b4b42c98c · FIO-E0-V4-E1-ENTRADAS · leitura · origem=Code

**Veredito: ABERTA para baixa integral, com o núcleo já pousado.** A descrição “em construção na volta 15g” ficou superada pelo pouso e pelo “PRONTO / NO AR”; porém a documentação ainda mantém a obra parada e reúne complementos pendentes. (`app/docs/RELATO.md:1674–1675`; `app/docs/GRADE-MESTRA.md:106`, `:683–701`)

A cópia não contém `.git`: `git log --all --grep` não pôde ser executado. Ancestralidade no `main`, hashes completos e datas dos commits **[não verificado]**. Abaixo, hashes e datas são **registros documentais**, sem consulta a banco ou logs. [nao verificado]

**1. Rastro encontrado**

| Data registrada | Hash / etapa | Evidência |
|---|---|---|
| 06/11, aval 22:12; despacho 22:55 | Abertura em `wt-fioe1-225336`, base `19aeede1`; E1 absorve PLANTIO (i) | `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1835–1836` |
| 02:20–02:25; dia do commit **[não verificado]** | `c2833d85`; PLANTIO `742f6e3b` reescrito para `51ea1e37`; árvore anterior `e55c6a37` | Entrega ainda incompleta, expressamente **“NAO POUSA”**. `app/docs/RELATO.md:1813–1815` |
| 07/11, registro da v5 | `12740227` | Desenho v5 respondido; construção retomada. `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1781–1783` |
| 07/11 16:2x | `344a9ddd`, volta 8 | Suite 14.165 OK, mas parada por desenho; respostas posteriores sobre veto e ordem do sinal. `app/docs/BACKLOG.md:1964`; `app/docs/RELATO.md:1769–1770` |
| 08/11, volta 14b | `6695e972` | Suite 14.301 OK; opus final 2 reprovou; nada pousado nessa etapa. `app/docs/RELATO.md:1742`, `:1721` |
| 08/11 11:2x | `d41caaf6`, volta 15 | Entrega e verificação; posteriormente superada pela 15b. `app/docs/RELATO.md:1729`, `:1726` |
| 08/11, volta 15b | `21e7341c` | Suite 14.325 OK; correções adicionais encaminhadas à 15c. `app/docs/RELATO.md:1719`, `:1725` |
| 08/11, volta 15c | `746179d7`; merge `b74da998` | Suite da volta verde; merge apresentou três falhas na catraca e defeito no portão do apply. `app/docs/RELATO.md:1714–1718` |
| 08/11, volta 15c2 | `3d8ebe87`; merge `76644dc8` | Suite 14.404 OK; opus final 3 reprovou o reuso com data antiga. `app/docs/RELATO.md:1710–1713` |
| 08/11, volta 15d2 | `f16d4737`; merge `b0c4297c` | Helper único; suite 14.422 OK; opus final 4 reprovou o caso de data NULL. `app/docs/RELATO.md:1707–1708`, `:1688` |
| 08/11, volta 15e | `06efab7e`; merge `a42d5204` | Contrato de saída e sombra verde; verificador pediu nova volta. `app/docs/RELATO.md:1696–1697` |
| 08/11, volta 15f | `4ecedc95`; merge `e2f865f5` | Suite 14.474 OK; achados g1–g5 encaminhados à 15g. `app/docs/RELATO.md:1698–1700` |
| 08/11 18:1x | `297499b8`, volta 15g | RED 8/20, GREEN 313; aval **E1-MATRIZ-TODAS-AS-PORTAS**. `app/docs/RELATO.md:1701`; `app/docs/PROMPTS.md:1992` |
| 08/11, após esse aval | `5c875ee3` | **Squash de `297499b8`**, ampliado às portas P1–P7; GREEN 5.978. `app/docs/RELATO.md:1702` |
| 08/11, interrompido 19:23 | `eabbfc5a`, merge6 | **Não certificou**; suite interrompida pelo PARE-P4. `app/docs/RELATO.md:1702`, `:1704` |
| 08/11, volta 15h | `6dee2010`; merge7 `b0e948d8` | Recusa de destino de outro colaborador; suite **14.536 OK** e sombra verde. `app/docs/RELATO.md:1705–1706` |
| 08/11 21:15; deploy 22:46 | `a8a692f5` | **Pouso local**, seguido de **PRONTO E1 NO AR**. `app/docs/RELATO.md:1675`, `:1674` |
| 08/11 23:12 | `a5df8ed5` | Reescrita de mensagens: árvore declarada igual a `a8a692f5`; 19 hashes alterados. Mapeamento efetivo **[não verificado]**. `app/docs/RELATO.md:1673` |
| 09/11 00:1x–00:4x | E1-FILA: `602a7d73` → `06ee1af0` → `45cf0e53` | Voltas corretivas; última com opus APROVA e suite 14.570 OK. `app/docs/RELATO.md:1677`, `:1676`, `:1668` |
| 09/11 00:5x; no ar 01:5x | Merge de `45cf0e53`; commit `7aa48401` | Pouso e deploy da **E1-FILA**. `app/docs/RELATO.md:1679–1680`; `app/docs/BACKLOG.md:1965` |
| 09/11 03:3x | Merge de `e9bf5de2`; commit `89df8c52` | **E1-FILA-V2-2-TRAVA também pousou**, com suite 14.622 OK e opus APROVA. `app/docs/RELATO.md:1633`; `app/docs/BACKLOG.md:1972` |

As demais menções exatas no RELATO são despachos, perguntas respondidas ou referências de contexto: `app/docs/RELATO.md:1662–1664`, `:1672`, `:1686–1690`, `:1693`, `:1723`, `:1728`, `:1749`, `:1768`, `:1784`, `:1787–1788`, `:1811–1812`, `:1818–1819`, `:9028`. As menções posteriores no BACKLOG vinculam complementos à E1. (`app/docs/BACKLOG.md:2086–2087`)

Não localizei os termos solicitados em `app/docs/RELATO-ARQUIVO.md` nem em `app/docs/TICKETS.md` **[não verificado]**.

**2. Houve pouso? O que ficou fora?** [nao verificado]

**Sim, documentalmente:** E1 em `a8a692f5`, E1-FILA em `7aa48401` e V2-2 em `89df8c52`. A comprovação por grafo Git de merge no `main` permanece **[não verificado]**. (`app/docs/RELATO.md:1675`, `:1679–1680`, `:1633`)

Ficaram separados:

- **Cinco grupos exportados**, expressamente fora do apply; o aval autorizava pousar com os limpos. (`app/docs/BACKLOG.md:855`; `app/docs/RELATO.md:1731`)
- Recusas sem contador, origem sem dia, conversa/reabertura sobre retratada e duplicatas sobre retratada, em filas próprias. (`app/docs/BACKLOG.md:857–860`)
- Resíduos de leitores, relógios, travas, outras portas e custo. **V2-2-TRAVA não está inteira pendente: ela pousou.** (`app/docs/BACKLOG.md:1970–1974`)
- Complementos posteriores **BUG-PLANTIO-NOTURNO-MAIS-1** e **ABSOLVIDA-PELA-FOLGA**; o segundo consta explicitamente aberto em 12/11. (`app/docs/BACKLOG.md:2086–2087`)

Há registro de quatro migrações aplicadas por Ronald e DRY seguinte zerado; isso é prova documental, sem conferência do banco nesta auditoria. (`app/docs/RELATO.md:1682`)

**3. A implementação existe na cópia?**

| Elemento | Prova |
|---|---|
| Trava das quatro classes | `trava_na_ordem` adquire Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa, por pk. `app/chamados/models.py:2707`, `:2744`, `:2753`, `:2766`, `:2770` |
| Catraca AST | Enumera as quatro classes, fixa o dono autorizado e exige zero trava fora dele. `app/chamados/tests/test_e1_v7_catracas.py:71`, `:85`, `:387` |
| Append-only | `DecisaoPergunta`; bloqueios de update/delete/bulk_update e de sobrescrita por save. `app/chamados/models.py:3969–3979`, `:3982`, `:4046–4052` |
| Escritor e evento da célula | `registrar` insere decisões e acorda células no mesmo atomic; a emissão passa por `emitir_evento_celula`. `app/chamados/models.py:4086–4099`, `:3314–3331` |
| Lâmpada do efeito | O leitor usa `veredito_lavrado` vigente por `esta_stale`; vencido ou ausente resulta em `nao_sei`. `app/chamados/services/tarefa_do_dia.py:16–18`, `:422–425` |
| Dia do fato em ponto único | `gravar_dia_do_fato` concentra a atribuição de `data_evento`. `app/chamados/services/fio.py:829–839` |
| Backfill | Comando com `--aplicar`; registra silêncio, retrata duplicatas pela porta e desfaz o ato em caso de recusa. `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `:331`, `:344`, `:425` |
| Identidade no banco | Migration 0053 recusa colisões e cria unicidade parcial por MARCO e BATIDA. `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36–66` |

**Limite do backfill:** esse comando não aplica as linhas antigas de resposta/validação da projeção; reserva esse passo a outro ato. A E4 registra o backfill posterior. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3–13`; `app/docs/BACKLOG.md:1991`) Resultados atuais dos testes **[não verificado]**.

**4. Decisão para a linha**

**ABERTA para baixa integral.** Há prova documental suficiente de que **o núcleo foi entregue**, mas não de quitação do agrupamento atual: a grade mantém “PARADA” e 16 complementos; há complemento explicitamente aberto. (`app/docs/RELATO.md:1674–1675`; `app/docs/GRADE-MESTRA.md:106`, `:683–701`; `app/docs/BACKLOG.md:2087`)

Também não há prova de a E1 ter sido absorvida por outra obra **[não verificado]**. A absorção documentada ocorre no sentido contrário: **E1 absorve PLANTIO (i)**. (`app/docs/RELATO.md:1835`)

O estado sustentado pela cópia é: **“núcleo pousado em 08/11; correções E1-FILA/V2-2 pousadas em 09/11; acervo exportado e complementos em linhas próprias”**. (`app/docs/RELATO.md:1675`, `:1679`, `:1633`; `app/docs/BACKLOG.md:855`; `app/docs/GRADE-MESTRA.md:683–701`)

_5779 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
