# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 17:51** · publicado em **06/11/2026 17:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 17:51 · sessao 911185810e67 · - · - · origem=Ronald (tmux)

6. Dinheiro, decisão e porta

- O dado persistido por dia é `ponto.models.DiaPago`: chave colaborador/competência/data/versão, com `data` do dia da entrada; rubricas `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, variantes noturnas e `horas_folga_trabalhada`. A restrição única inclui `versao`, portanto a leitura deve escolher a versão; a porta de leitura usa `versao='motor'` por padrão. (`app/ponto/models.py:315`, `app/ponto/models.py:328`, `app/ponto/models.py:386`; `app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:853`)
- Função pronta: `linhas_do_periodo(colab, ini, fim, *, versao='motor') -> {date: DiaPago}`. Filtra `tipo=TIPO_DIA`, versão e intervalo inclusivo; não roda motor. (`app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:852`)
- `rubricas_do_dia(linha) -> dict`: retorna `lavrada`, `extras_50`, `extras_100`, `atraso`, `antecipada`, `noturnas`, `intra`, `trabalhadas`, `minutos_extra`; ausência de linha dá zeros **com `lavrada=False`**. As variantes feriado/noturna são subconjuntos e não devem ser somadas novamente aos totais. Para distinguir a rubrica de feriado e folga, ler também os campos específicos do `DiaPago`. (`app/ponto/services/dia_pago.py:899`, `app/ponto/services/dia_pago.py:908`, `app/ponto/services/dia_pago.py:912`)
- `pago_do_dia(linha) -> float | None`: `None` se não há linha ou se todas as rubricas de dinheiro estão zero; caso contrário soma `horas_trabalhadas + horas_folga_trabalhada + detalhe['folga_feriado_min']/60`. Logo, **HE zero isolada não prova dia sem dinheiro**. (`app/ponto/services/dia_pago.py:859`, `app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`)
- A escrita da folga certa vai para `horas_folga_trabalhada`; a folga-feriado vai para HE100 e HE100-feriado, com `detalhe['folga_feriado_min']`; folga sem escala certa vai para `horas_trabalhadas`, com `detalhe['folga_sem_escala_min']`. (`app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`, `app/ponto/services/dia_pago.py:257`)
- A decisão de HE está em `DecisaoHE`, estados `'autorizado'` e `'nao'`; `'nao'` é decisão, não ausência de decisão. Só as decisões `objects.vigentes()` valem: filtro `revogada_em__isnull=True`. (`app/ponto/models.py:1255`, `app/ponto/models.py:1260`, `app/ponto/models.py:1273`, `app/ponto/models.py:1277`)
- Função pronta para decisão vigente: `estado_por_dia(ids, ini, fim) -> {(colaborador_id, 'YYYY-MM-DD'): estado}`; lê uma consulta em `DecisaoHE.objects.vigentes()`. **Ausência desse mapa só prova falta de decisão; a existência de pendência exige a fonte de pendência.** (`app/ponto/services/gestao_he.py:63`, `app/ponto/services/gestao_he.py:81`)
- Pendência de HE é `dia['he_fora_da_janela']`, montada pelo espelho; o caso de dia sem previsto é lido de `resultado_v2.dias_sem_previsto_fora_da_janela`, com `sentido='dia'`, `marco=None` e minutos publicados pelo motor. (`app/ponto/services/he_pendente_lavrado.py:19`; `app/ponto/services/espelho.py:743`)
- Para pendência já lavrada, `he_pendente_lavrado.ler(empresa, mes, ano) -> (detalhe, calculado_em) | (None, None)` lê `MetricaSnapshot`, sem motor, e informa a idade da foto; para medir atual, `apurar(empresa, mes, ano, *, decisoes=None, revogar=None)` chama `espelho_do_colab`, portanto passa pelo motor. Sem callback `revogar`, esse apurador declara-se leitor puro; não chamar `lavrar`, que grava. (`app/ponto/services/he_pendente_lavrado.py:71`, `app/ponto/services/he_pendente_lavrado.py:77`, `app/ponto/services/he_pendente_lavrado.py:94`, `app/ponto/services/he_pendente_lavrado.py:177`, `app/ponto/services/he_pendente_lavrado.py:193`)
- Para saber se pendência exige humano, já existe `acima_do_limite(minutos, limite) -> bool`, comparando o total do dia com `Empresa.limite_decisao_he_min`. O consumidor pronto `dias_acima_do_limite_sem_decisao(pendencias, limite, estados) -> [(colaborador_id, 'YYYY-MM-DD')]`, sem duplicatas, usa esse juiz e o mapa de decisão; `'nao'` também libera. (`app/ponto/portas/he.py:249`, `app/ponto/portas/he.py:265`; `app/folha/porta_export.py:199`, `app/folha/porta_export.py:223`)
- `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` está registrada em `core/juizes.py`. Sem ID ou sem verbo de porta alcançável retorna `None`; com porta retorna `{'rota', 'fila', 'verbos', 'fio'}`. (`app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1373`, `app/chamados/juizes.py:1382`, `app/chamados/juizes.py:1386`; `app/core/juizes.py:277`)
- `rota='porta_fila'`: existe chamado da própria célula na fila; senão, `rota='porta_pergunta'`: pergunta viva da célula, presente no app, não respondida sem texto, cuja âncora está na fila. `fila` só enumera os chamados da célula visíveis na lista padrão; `verbos` pode conter também âncoras de pergunta. (`app/chamados/juizes.py:1344`, `app/chamados/juizes.py:1353`, `app/chamados/juizes.py:1375`)
- `fio`: qualquer verbo `cobrar` vence e dá `'colab'`; depois `validar` dá `'admin'`; os demais dão `'chamado'`. Para “há ato pendente do admin?”, consultar os verbos da porta contra **`VERBOS_DO_ADMIN=('validar','decidir')`**, porque `fio='chamado'` pode conter `decidir`. (`app/chamados/juizes.py:1327`, `app/chamados/juizes.py:1332`; `app/chamados/services/verbos.py:22`)
- A porta não precisa rodar o motor de dinheiro no seu corpo e alimentação; usa fila, partição e perguntas vivas. **Não certificaria ausência de escrita transitiva:** `alimentar_portas -> pergunta_viva -> dia_da_pergunta_trancado -> _admin_tocou_o_dia_depois -> _toque_humano_no_dia -> get_user_sistema`, e esse último usa `User.objects.get_or_create`. É escrita potencial se o usuário estiver ausente, apesar das docstrings de “SO LEITURA”; futura execução deve respeitar o envelope somente leitura. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1302`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:954`, `app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1069`; `app/core/utils.py:18`)
- Dinheiro atual sem lavra exige `autoridade_do_periodo(colaborador, ini, fim)`/`espelho_do_colab(...)`, que constroem e executam motor; pelo escopo do aval, essa etapa fica na sombra. A simples leitura de `DiaPago`, da decisão vigente, da foto de pendências e da porta não exige nova apuração monetária. (`app/ponto/services/espelho.py:971`, `app/ponto/services/espelho.py:1111`, `app/ponto/services/espelho.py:1164`, `app/ponto/services/espelho.py:1179`; `app/ponto/services/dia_pago.py:852`; restrição sombra: dado fornecido [nao verificado no repositório])

7. Cadastro para comparar os dois dias de 02/11

- O código de importação reconhece `(11,2)` como **Finados**, na allowlist de nacionais; isso não prova que a linha de 02/11/2026 está cadastrada na produção. (`app/core/views_config.py:111`, `app/core/views_config.py:117`; cadastro PROD [nao verificado])
- A autoridade runtime é `Feriado(data, abrangencia, uf, municipio)`, alcançada por `core.feriados.eh_feriado(data, municipio)` ou `feriados_do_colab(colaborador, ini, fim, ...)`. Nacional alcança todos; estadual exige UF; municipal exige UF e município normalizado. (`app/core/models.py:5`, `app/core/models.py:12`; `app/core/feriados.py:57`, `app/core/feriados.py:91`, `app/core/feriados.py:125`)
- O lugar do feriado vem do **`posto.municipio` do vínculo daquele dia**, não do texto livre da praça nem do posto atual de `Colaborador`. `lugar_do_vinculo(vinculo) -> Municipio|None`; `feriados_do_colab` consulta `vinculo_do_dia`. (`app/core/feriados.py:114`, `app/core/feriados.py:121`, `app/core/feriados.py:150`)
- O vínculo é `EscalaColaborador`, com `posto`, `tipo_escala`, datas início/fim, e `trabalha_em_feriado`. O campo só transforma feriado em folga nos ciclos 6x1, 5x2 e semanal; a função pronta é `ec.feriado_vira_folga(data, escala=None) -> bool`, que junta esse cadastro, ciclo e feriado pelo município do posto do vínculo. (`app/escala/models.py:871`, `app/escala/models.py:896`, `app/escala/models.py:32`, `app/escala/models.py:1373`, `app/escala/models.py:1389`)
- Para vínculo novo, `padrao_trabalha_em_feriado(tipo_escala, posto=None) -> bool` dá precedência à declaração `Posto.opera_em_feriado`; se não declarada, usa o tipo. Isso define o nascimento, não modifica vínculo existente. (`app/escala/models.py:905`, `app/escala/models.py:908`, `app/escala/models.py:911`)
- A dobra normal de feriado é decidida por `MotorBase._feriado_dobra_no_dia(dia) -> bool(self.feriado_em_dobra) and self.eh_feriado(dia)`, com dia de início do plantão. A política padrão sai de `TipoEscala.tipo_ciclo`, não de `tipo_base`: 12x36 está em `CICLOS_FERIADO_SIMPLES`; os demais ciclos recebem dobra por padrão. (`app/ponto/motor_calculo_v2.py:914`, `app/ponto/motor_calculo_v2.py:928`, `app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:3584`, `app/ponto/motor_calculo_v2.py:3599`, `app/ponto/motor_calculo_v2.py:3669`)
- A CCT pode restaurar dobra do 12x36 via `Sindicato.feriado_12x36_em_dobra`; `get_motor_cct` aplica esse campo quando o switch global `ParametroSistema(chave='regua_cct_ativa', empresa=None)` está ligado. (`app/core/models.py:56`; `app/core/regua_cct.py:428`, `app/core/regua_cct.py:443`, `app/core/regua_cct.py:449`, `app/core/regua_cct.py:477`)
- `regua_para(colaborador, competencia, tipo_ciclo=None)` resolve o vínculo da data, sua praça e ciclo; lê `Empresa.regime_trabalhista`. Em `'clt'` devolve régua legal; em `'cct'` busca aplicação vigente. `AplicacaoConvencao(empresa, sindicato, praca opcional, ativo)` é resolvida por `_aplicacao_vigente`: praça específica vence empresa inteira; empate, falta ou sindicato fora da vigência resultam em piso legal com motivo. (`app/core/regua_cct.py:297`, `app/core/regua_cct.py:328`, `app/core/regua_cct.py:352`, `app/core/regua_cct.py:363`, `app/core/regua_cct.py:231`, `app/core/regua_cct.py:253`, `app/core/regua_cct.py:261`, `app/core/regua_cct.py:265`)
- Para folga trabalhada, a apuração do adicional passa por `escala_certa_no_dia(colab.id, data, cache)`. Com escala certa e `feriado_vira_folga=True`, o fechamento separa o período para HE100-feriado; sem escala certa, a hora vai a trabalhadas sem adicional. (`app/ponto/services/fechamento.py:919`, `app/ponto/services/fechamento.py:925`, `app/ponto/services/fechamento.py:935`, `app/ponto/services/fechamento.py:946`, `app/ponto/services/fechamento.py:951`)
- Há outro cadastro decisivo para HE zero: janela de HE da empresa, sua vigência e decisão autorizada. O motor bloqueia integralmente o par de folga/feriado sem previsto quando a janela se aplica, salvo autorização; folga sem escala certa fica fora desse bloqueio específico. A pendência recebe os minutos que o par pagaria. (`app/colaboradores/models.py:58`, `app/colaboradores/models.py:117`; `app/ponto/motor_calculo_v2.py:1422`, `app/ponto/motor_calculo_v2.py:1448`, `app/ponto/motor_calculo_v2.py:1451`, `app/ponto/motor_calculo_v2.py:1455`, `app/ponto/motor_calculo_v2.py:1465`, `app/ponto/motor_calculo_v2.py:1471`)
- O dado fornecido não contém os vínculos, municípios, ciclos, réguas, decisões ou linhas `DiaPago` dos dois dias; portanto qual desses cadastros efetivamente explica **0 versus 2,31** é [nao verificado].

Regras não encontradas, neste recorte de busca

- Não encontrei função única que, recebendo um dia órfão, devolva ao mesmo tempo dinheiro, pendência de HE e porta; as autoridades estão separadas acima. Busquei `DiaPago`, `he_pendente`, `decisao.*pendente`, `pendente.*decisao`, `porta_alcancavel_do_dia`, `VERBOS_DO_ADMIN`. [nao verificado: ausência global; arquivos principais inspecionados acima]
- Não encontrei regra runtime que faça “02/11 é feriado” sem consultar o cadastro; encontrei a allowlist de importação e o juiz que lê `Feriado`. Busquei `11, 2`, `11-02`, `Finados`, `finados`, `02/11`. (`app/core/views_config.py:117`; `app/core/feriados.py:27`, `app/core/feriados.py:91`; ausência global [nao verificado])
- Não encontrei regra de que `trabalha_em_feriado=True` sozinho garante pagamento: há seleção por ciclo/CCT, escala certa, previsto e janela/autorização. (`app/escala/models.py:1390`; `app/core/regua_cct.py:477`; `app/ponto/services/fechamento.py:925`; `app/ponto/motor_calculo_v2.py:1453`)
- Não encontrei garantia transitiva de somente leitura da porta; encontrei o `get_or_create` citado. (`app/core/utils.py:18`)

## 06/11 17:51 · sessao 851264ea97d5 · - · - · origem=Ronald (tmux)

Itens 3 e complemento 4, somente código lido; nenhuma função executada.

**Predicados e status**

| Função/campo real | Entrada e saída | Batidas/registro |
|---|---|---|
| `chamados.juizes.dia_sem_trabalho(cel)` | `CelulaDia` → `bool`: `cel.trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`; na ausência parcial, só `trabalha is False`. Não tratar `None` como folga. | Lê célula/ata, sem ler batidas diretamente; implementação `app/chamados/juizes.py:413`, retorno `:426-428`; registrado `app/core/juizes.py:278`. |
| `ponto.status_dia.status_do_dia_lavrado(colaborador, data, *, agora=None)` | → `{'status','rotulo','fonte'}`; fonte `lavrado`, `relogio`, `sem_celula` ou `nao_lavrado`. | Delegação ao lote em `app/ponto/status_dia.py:201-218`; registrado `app/core/juizes.py:187`. |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` | Pares `(colaborador,data)` → `{(colaborador_id,data): dict}`; é o mesmo corpo do juiz singular. | Carga dos turnos/células e fatos; contrato `app/ponto/status_dia.py:221-236`; calendário importa/chama `app/colaboradores/services/calendario.py:602-607` e lê `status` `:663-664`. |
| `resolver_dia_oferecido(dia, hoje)` | Item de `contexto_calendario()['dias']` → `bool`: `pode_ultimo_recurso` + dia passado + status membro da constante. | Não lê batidas diretamente; `app/colaboradores/services/calendario.py:97-104`; registrado `app/core/juizes.py:718`. |
| `STATUS_QUE_PEDEM_DECISAO` | `('falta','irregular','aberto','sem_certidao')`; **não contém `folga_trabalhada`**. | `app/colaboradores/services/calendario.py:94`; vocabulário próprio `folga_trabalhada` em `app/ponto/status_dia.py:36`. |
| `EscalaColaborador.trabalha_em_feriado` | Booleano do **vínculo**, default model `True`; help diz afetar ciclos weekday `6x1/5x2/semanal`. | `app/escala/models.py:871`, `:896-899`; não é um classificador de batida. |
| `EscalaColaborador.feriado_vira_folga(self,data,escala=None)` | → `bool`: `trabalha_em_feriado=False`, ciclo em `TipoEscala.CICLOS_FERIADO_VIRA_FOLGA` e feriado pelo município do posto **desse vínculo**. | Não lê batidas; `app/escala/models.py:1373-1395`; função não localizada como entrada própria no registro `[nao verificado]`; o registro aponta `fatos_do_dia` para “feriado suprime” (`app/core/juizes.py:281`). |
| `core.feriados.eh_feriado(data,municipio=None)` | → `bool`; sem município, só nacional. Para vínculo: `eh_feriado(data,lugar_do_vinculo(ec))`; para janela pessoal: `feriados_do_colab(colaborador,ini,fim,*,carregados=None,vinculos=None,celulas=None)` → `set[date]`, usando `vinculo_do_dia` por data. | Implementações `app/core/feriados.py:91-95`, `:114-157`; registrado `app/core/juizes.py:176`. |

`folga_trabalhada` é decidido pela lavra quando há batida e `tipo_dia=='folga'` ou `veredito=='fato_sem_previsao'`; `status_da_lavra(...):113` retorna `(status,rotulo)` em `app/ponto/status_dia.py:157-158`. Portanto a constante de “Resolver dia” não serve para identificar todos os dias de folga trabalhada (`app/colaboradores/services/calendario.py:94`).

**Censo de “qual escala casa com as batidas”**

| Sítio | Contrato real e limites | Leitura/escrita/registro |
|---|---|---|
| `ponto.propositor.dia_degenerado(dia)` | Dict-dia da grade → `(bool,motivo)`; motivos cluster, contagem, órfã, tipo incoerente, delta crítico; horas ilegíveis → `SEM_HORA_NA_ATA`. **Não escolhe outra escala.** | Lê dict, não consulta Batida nem grava; `app/ponto/propositor.py:34-73`. Registro próprio não encontrado `[nao verificado]`. |
| `ponto.propositor.atribuir_global(slots,marcas)` / `atribuir_global_turno(slots,marcas)` | Slots `(tipo,'HH:MM')` + horas das marcas → dict `atrib,furos,sobras,custo,desloc,ambiguo`; a variante turno normaliza travessia da meia-noite. **Casa marcas a slots fornecidos**, não busca catálogo. | Puras; `app/ponto/propositor.py:87-117`, `:153-168`. Registro próprio não encontrado `[nao verificado]`; juiz de ocupação registrado é `_match_marcos`, `app/core/juizes.py:200`. |
| `ponto.propositor.template_suspeito(dias)` | Janela de dict-dias → `(suspeito,mediana,n_dias)` pelos deltas absolutos; mínimo três dias; suspeita com mediana >45 min. Não escolhe template. | Pura; `app/ponto/propositor.py:197-221`. Registro próprio não encontrado `[nao verificado]`. |
| `escala.services.cadastro_tipo` | `descreve_turno_partido(payload,tipo=None,pai=None)` → bool por `eh_turno_partido`; `tipo_em_memoria(**campos)` → TipoEscala sem save. **Não encontrei nesse módulo um matcher batidas→escala** `[nao verificado]`. | `app/escala/services/cadastro_tipo.py:86-113`, `:396-398`; `salvar_tipo` cria/edita e pode regenerar vínculos, logo não chamar na sonda (`:140-142`, `:320`, `:327-359`); `derivar_tipo` grava cópia e blocos (`:401-425`). Juiz delegado `eh_turno_partido` registrado `app/core/juizes.py:208`. |
| `escala.services.propositor.propostas(hoje=None,janela=28,colaborador_id=None,so_existencia=False)` | → dict `fonte,n,propostas,universo,janela_dias,...`; uma proposta por vínculo candidato: âncora, template existente ou grade por dia. | Consulta Batida não retratada (`app/escala/services/propositor.py:477-483`), células e templates; dias reais por `turnos_de_batidas` (`:106-118`); saída `:585-589`. Não escreve na coleta (`:42`); **não chamar `aplicar`/`recusar`**, que escrevem (`:38`, `:42`). Registro próprio não encontrado `[nao verificado]`. |
| `escala.services.propositor.propor(ec,dias_bat,dias_janela,templates,previsto_atual,forcar='',ritmo='',horario=(None,None),pausa=None)` | → dict ou None; recebe dias/horários já medidos; cobertura mínima 90%, sobra máxima 30%; retorna `ancora/template/grade_por_dia`. | Sem consulta direta de Batida; `app/escala/services/propositor.py:308-375`, limites `:68-71`. Cuidado: caminho comum compara **datas**, por `_casamento` e `_sobra` (`:235-261`); filtro de horário existe no caminho `_propor_pelo_ritmo` (`:162-197`), e `_horario_mediano` só é passado quando há `_ritmo` (`:526-530`). Portanto não afirmar que toda proposta valida horário de todos os marcos. Registro próprio não encontrado `[nao verificado]`. |
| `escala.services.fase_12x36.ancora_sugerida(dias_batidos,ancora_atual,piso=6,ganho_min=40)` | → dict com `ancora` ISO ou None, motivo, batidas e porcentagens; sugere paridade, não horário/template. | Pura, recebe datas; `app/escala/services/fase_12x36.py:23-65`. O módulo declara quatro sítios de decisão de âncora ainda existentes (`:11-12`). Registro próprio não encontrado `[nao verificado]`. |
| `fase_12x36.dias_de_fase_as_of(colab_id,empresa_id,ini,fim,ate=None)` | → conjunto de datas ISO por lavra mais recente que cobriu cada dia; não recalcula batidas. | Consulta snapshots; `app/escala/services/fase_12x36.py:117-148`; `ultimo_lavrado(ate=None)` → dict ou `sem_medida=True`, `:197-214`. Detector anterior morreu, autoridade declarada é censo `:68-75`; registro próprio não encontrado `[nao verificado]`. |
| `censo_fase_12x36.Command.handle(...)` | Mede paridade real por competência/vínculo; desvio `<90%` com `>=6` plantões. Não há função curta nesse arquivo para devolver “escala X casa”. | Lê turnos reais `app/ponto/management/commands/censo_fase_12x36.py:180`, critério `:256`; opções `--lavrar` e `--curar` escrevem (`:36-42`, `:274-279`, `:339`). Não executar para mero censo sem controlar flags. |
| **Outro matcher:** `escala.detector_proposta.propor_escala(colab,ini,fim)` | → `{'padrao','proposta','confianca','porque'}`; `proposta` inclui template id/código/descrição/ciclo/horário. Casa entrada, saída e número de marcos, raio 30min; mínimo 5 dias; confiança alta exige 15 dias, coerência e estabilidade. | Lê `batidas_apuraveis` em `medir_padrao` (`app/escala/detector_proposta.py:49-58`); matcher `_casa_template` `:95-115`; principal `:118-156`; não escreve `:7`. **Limite declarado:** agrupa LOCALDATE, não dia-turno; noturno pode inflar dias/deslocar entrada (`:18-25`). Registro próprio não encontrado `[nao verificado]`. |
| **Outro:** `escala.regua_defesa.diagnostico_regua(ec,ref=None,dias=30)` + `sugerir_destino(ec,diag)` | Diagnóstico → dict `veredito,classe,evidencia,data_exemplo,declarado,real`, medianas em `med`; destino → `{tipo_escala_id,codigo,n_colabs,dist}` ou None, comparando entrada/intervalo/gap com templates usados por vínculos ativos da mesma empresa. | Diagnóstico lê batidas apuráveis `app/escala/regua_defesa.py:118-174`; destino consulta catálogo/vínculos `:246-288`, sem gravar. Registro próprio desses nomes não encontrado `[nao verificado]`. |
| `fase_na_tela.fase_sugerida(c,te,hoje=None,ec_atual=None)` | → `(ancora|None,desde|None,motivo)`; lê dias trabalhados e troca de ritmo/últimos plantões. | `_trabalhados` usa batidas apuráveis `app/escala/services/fase_na_tela.py:40-47`; principal `:50-95`; não grava. Registro próprio não encontrado `[nao verificado]`. |
| `pausa_nao_cadastrada(te,batidas_por_dia)` | → dict com pausa sugerida, evidência, segunda metade ou None; não escolhe toda escala. | Pura sobre timestamps recebidos; `app/escala/services/pausa.py:29-49`; coletor turnos→timestamps `:52-58`. Registro próprio não encontrado `[nao verificado]`. |

**Cadastro x realidade:** `lista(hoje=None,empresas=None,*,agora=None)` retorna dict ou None; lê a lavra do esmeril e `destino`, sem matcher próprio nesse ponto (`app/escala/services/cadastro_realidade.py:710-717`, `:802-817`). O destino foi calculado por `esmeril_espelho.anomalias_da_frota`, que chama `propor_escala(c,hoje-60d,hoje-1d)` (`app/ponto/services/esmeril_espelho.py:206-223`); `_destino` formata (`:234`), `gravar_lavra` escreve snapshot (`:317-330`) e `ler_lavra` lê banco (`:334-350`).

**Complemento regeneração:** campos `regenerada_em`, `regeneracoes`, `dna_anterior` em `app/escala/models.py:1718-1729`; `dna_anterior` só conserva imediatamente anterior (`:1714-1717`). Escritor canônico `ponto.portas.celula.regenerar_celulas_vinculo(...)`:110-112 → número de células alteradas (`:118`); salva DNA anterior, timestamp e incrementa contador (`:263-277`). A função antiga em `gerar_celulas.py` é import/reexport dessa porta (`app/ponto/management/commands/gerar_celulas.py:13`). Isso prova **reescrita**, não prova que qualquer órfã de ata foi causada pela reescrita; atribuição dos 40 dias requer detalhe por célula/DNA/ata `[nao verificado]`.

## 06/11 17:51 · sessao 67ab93825034 · - · - · origem=Ronald (tmux)

1. **Ata e consulta.** A ata é `escala.models.CelulaDia.ata`, JSON do julgamento (`app/escala/models.py:1774-1778`), com uma única célula por `(colaborador,data)` (`app/escala/models.py:1833`). `ata_do_dia(dia, marcos=None, dna=None)` copia `dia['orfas']` em `ata['orfas']` e grava a contagem em `ata['n_orfas']` (`app/ponto/services/cartorio.py:696-711`). A porta `lavrar_veredito` persiste a ata inclusive quando um veredito administrativo impede substituir o veredito (`app/ponto/portas/celula.py:995-1004`, `:1027-1035`).

   A consulta para a **última ata persistida**, por empresa, é:

   ```python
   ini, fim = janela_fechamento(11, 2026, empresa)
   qs = CelulaDia.objects.filter(
       colaborador__empresa=empresa,
       data__range=(ini, fim),
       ata__n_orfas__gt=0,
   )
   ```

   A janela vem de `janela_fechamento` e `Empresa.dia_inicio_competencia`, não de uma constante da sonda (`app/ponto/janelas.py:36-57`; `app/colaboradores/models.py:46`). Não há campo de vigência/retratação da ata nem versões concorrentes nessa entidade; a célula carrega a ata corrente (`app/escala/models.py:1681-1817`, `:1833`). A consulta **não certifica que ela já incorporou todos os insumos atuais**: o leitor `grade_da_celula(colab,ini,fim)` lê `cel.ata` diretamente (`app/escala/services/leitor_celula.py:345-362`). O cartório testa atualização internamente pela impressão dos insumos (`app/ponto/services/cartorio.py:1348-1358`); `houve_fato_novo(...)` deliberadamente exclui chamado/fio e não equivale a esse teste integral (`app/ponto/services/cartorio.py:329-367`).

   Retratação é filtro da **batida**, pela autoridade `batidas_apuraveis(colab,ini=None,fim=None)` — `retratada_em IS NULL` (`app/ponto/turnos.py:2844-2871`). Não deve ser inventado filtro homônimo na célula (`app/escala/models.py:1681-1817`).

   Leitores concretos: `_orfas_da_ata` repassa horas e usa `'?'` somente em ata antiga com contagem sem lista (`app/escala/services/leitor_celula.py:175-185`); `grade_da_celula` transporta isso ao dict-dia (`:401-411`); `periodos_do_dia` lê a lista da ata (`app/ponto/juiz_batida.py:149-164`).

2. **Texto “espúria”.** A busca em todos os templates encontrou o literal somente no calendário: `title="Batida fora do turno previsto (espúria) — não casa nenhum marco; conferir."`, condicionado a `b.orfa` (`app/templates/colaboradores/partials/_calendario_grade.html:121`). O calendário obtém órfãs por `grade_da_celula` (`app/colaboradores/services/calendario.py:332-381`) e marca `b.orfa=True` quando a hora local da batida pertence à lista lida (`:621-627`); esse caminho lê a ata (`app/escala/services/leitor_celula.py:401-403`).

   No template efetivamente chamado `espelho.html`, a órfã aparece como `• HH:MM`, sem a palavra “espúria” (`app/templates/ponto/espelho.html:240-241`). O serviço normalmente lê `grade_da_celula` na competência (`app/ponto/services/espelho.py:518-527`), mas possui caminho pelo builder antes da competência e degradação por ata agregada (`:528-550`). `montar_realizado_grade` apenas transporta a lista de órfãs (`app/escala/utils.py:1099-1103`, `:1131`). Logo, “o ESPELHO escreve literalmente espúria” não foi confirmado nesta cópia [nao verificado].

4. **Célula refeita.** Os campos são `regenerada_em`, `regeneracoes` e `dna_anterior`; este último guarda somente o DNA imediatamente anterior (`app/escala/models.py:1704-1729`). A regeneração reescreve `trabalha`, vínculo gerador e DNA quando há diferença, guarda o DNA antigo e incrementa o contador (`app/ponto/portas/celula.py:255-277`). Ela depois dispara o julgamento do chão alterado (`:352-374`).

   Há dois significados diferentes de órfã: batida sem marco na ata, produzida pelo alinhamento (`app/escala/utils.py:585-665`), e **cobrança órfã de regeneração**, cujo marco foi cobrado antes da reescrita (`app/ponto/portas/celula.py:282-289`). Para a segunda existe `cobranca_orfa(janelas,chamado,regenerada_em=None,editada_em=None) -> instante | None`, dependente de `marcos_faltantes` e reescrita posterior à cobrança (`app/escala/services/regeneracao.py:141-155`). Regeneração não implica automaticamente batida órfã; os “40” do dado não permitem comprovar essa causalidade individual [nao verificado].

5. **Marco versus tipo real.** A ata guarda em cada elemento de `ata['lampadas']` as chaves `tipo` (marco), `hora` (prevista), `acesa`, `luz` (hora real) e `tipo_real` (tipo gravado da batida) (`app/ponto/services/cartorio.py:677-690`). O casamento por proximidade no ramo sem coluna grava `tipo_real=None`, explicitamente desconhecido (`:624-640`).

   Funções existentes que comparam:

   - `periodos_do_dia(celula,tipo_escala) -> dict`: retorna `papeis[].tipo_divergente`, calculado para lâmpada acesa com tipo real conhecido diferente do tipo esperado no DNA (`app/ponto/juiz_batida.py:149-164`, `:238-247`). É juiz registrado (`app/core/juizes.py:1173-1175`), somente leitura, sem motor (`app/ponto/juiz_batida.py:153`). Para contar dias pela função real, contar uma vez cada célula com algum `p['tipo_divergente']` no retorno; separar `desalinhado` e tipos desconhecidos (`:195-204`, `:228-247`). Atenção: a comparação usa o tipo esperado do **DNA**; a validação com a ata confere hora, não explicitamente o tipo (`:228-247`).
   - `montar_realizado_grade(gd,marcos,chamados_dia=None) -> (real,orfas)`: devolve `real[].divergente` comparando tipo real ao tipo do marco; desconhecido não acusa (`app/escala/utils.py:1099-1103`, `:1123-1131`).
   - `dia_degenerado(dia) -> (bool,motivo|None)` acusa `TIPO_INCOERENTE`, mas sua precedência retorna antes para cluster, órfãs e contagem incompatível; portanto não serve para contar **todas** as divergências de tipo (`app/ponto/propositor.py:34-73`).

   Uma batida casada ocupa marco e deixa de ser órfã pelo próprio alinhamento (`app/escala/utils.py:245-247`, `:656-662`); assim o censo de órfãs, isoladamente, não mede tipos divergentes nas lâmpadas acesas (`app/ponto/services/cartorio.py:677-711`). Quantidade real de dias nessa condição não consta do dado [nao verificado].

**Não encontrei escrito:** função pública pura que certifique “ata vigente e atualizada” integralmente; o teste `_em_dia` está dentro do julgamento (`app/ponto/services/cartorio.py:1199`, `:1348-1358`). Também não encontrei o literal “espúria” no template do espelho; encontrei somente a apresentação com ponto e a frase do calendário (`app/templates/ponto/espelho.html:240-241`; `app/templates/colaboradores/partials/_calendario_grade.html:121`).

## 06/11 17:49 · sessao be1b981c644a · O10-C · leitura · origem=Code

HASH LIDO = 40019f43b68afe3e4270634d6f251eb3af9440f0 [nao verificado]

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| C01 | `app/ponto/management/commands/trocas_plantao_lote_o10c.py:37`: “DRY (sem flag): so le. A porta NAO e chamada”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`), `documento='cedente.pdf'` (`app/ponto/models.py:777`). Sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`) e sem `--aplicar` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`). | Zero escritas de células, trilhas e lista administrativa; nenhuma chamada a `trocar_plantao`. | Depositar caso administrativo durante o DRY ou chamar a porta para depois desfazer a transação. | golden |
| C02 | Aval fornecido: “cada um vai a lista do admin COM a porta da troca ja aberta no dia (quem cobriu + os 2 anexos obrigatorios), e o apply do lote aplica 0 pares sem erro”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`), `documento='cedente.pdf'` (`app/ponto/models.py:777`). Sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`); `--aplicar --por=ator` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:175`). | Zero pares; rc 0, sem erro; zero escritas de `CelulaDia` e zero trilhas de edição/troca; uma linha persistida na lista do admin por SOLO. Deve haver acesso à troca daquele dia, com indicação de quem cobriu e duas cartas. URL, status HTTP, controles e critério executável de “alcançável”: **regra nao decide**. | Apenas imprimir o SOLO no terminal; tratar ausência de substituto como erro; aplicar uma edição unilateral. | golden |
| C03 | Corte fornecido, mínimo (3): “rodar 2x -> ainda 1 linha por SOLO (idempotencia)”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`). Executar duas vezes `--aplicar --por=ator` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:175`), sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Após a segunda execução, ainda exatamente uma linha administrativa para esse SOLO; zero pares e zero escritas de células nas duas execuções. | Criar uma linha administrativa nova incondicionalmente em cada execução. | golden |
| C04 | Aval fornecido: “A comp 09 e exportada: nada se grava nela sem o ! proprio, depois do DRY publicado”. `app/ponto/portas/celula.py:548`: dia com TXT emitido não se edita. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-09-10` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); `ExportacaoDominio.empresa=E` (`app/folha/models.py:77`), `mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`). Apply sem par e sem `!`. | Zero pares; nenhuma edição de célula na competência 09. O alcance integral de “gravar na competência”, incluindo se o depósito administrativo é abrangido pela proibição: **regra nao decide**. O aval exige também uma linha por SOLO, mas não explicita essa conciliação. | Considerar o depósito SOLO autorização suficiente para editar o dia exportado ou abrir a guarda automaticamente. | borda |
| C05 | Aval fornecido: “o lado do substituto vem do ADMIN pela porta da troca (O10-B), nunca deduzido”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`). Existe somente um colega candidato: `EscalaColaborador.colaborador=col02` (`app/escala/models.py:872`), `posto=P` (`app/escala/models.py:873`), `data_inicio=2026-10-01` (`app/escala/models.py:875`), `data_fim=2026-10-31` (`app/escala/models.py:876`); o vínculo de col01 tem o mesmo `posto=P` (`app/escala/models.py:873`). Apply sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Ainda zero pares e zero escritas de células; uma linha administrativa para o SOLO. A unicidade do candidato não fornece o substituto. | Escolher automaticamente o único colega do mesmo posto/dia. | adversarial |
| C06 | `app/ponto/tests/test_o10c_lote_trocas.py:6`: aprovado, par declarado, duas cartas e dia fora de lavrada vira par pela porta. `app/ponto/portas/celula.py:635`: cada carta determina o dia de seu lado. | Fixture sintética: duas `Ausencia`, `colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento='cedente.pdf'/'substituto.pdf'` (`app/ponto/models.py:777`). Cedente: `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `status='aprovada'` (`app/ponto/models.py:799`). Vínculos válidos; competência não exportada. `--par=C:S` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`; identificadores: [nao verificado]), com apply. | Uma troca confirmada: retorno da porta `trocada`; dia do cedente com `trabalha=False`, dia do substituto com `trabalha=True`, ambos com origem de edição humana e trilha. Substituto recebe dia normal do próprio template; nenhum desconto novo pela troca. | Fazer o tratamento SOLO interceptar também o par declarado; inverter os lados; tornar o dia assumido folga trabalhada. | golden |
| C07 | `app/ponto/janelas.py:11`: competência termina no mês indicado; `app/ponto/janelas.py:13`: corte 21 abrange 21 do mês anterior até 20 do mês. Aval: “A comp 09 e exportada”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-08-21` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); exportação de setembro: `ExportacaoDominio.mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`), `empresa=E` (`app/folha/models.py:77`). Apply sem par. | O dia pertence à competência 09 e não pode ter sua célula editada sem o ato próprio. Depósito administrativo dentro do alcance de “nada se grava nela”: **regra nao decide**. | Consultar exportação pelo mês civil de agosto ou excluir o primeiro dia da janela exportada. | borda |
| C08 | `app/ponto/janelas.py:13`: competência com corte 21 termina no dia 20. Aval: “A comp 09 e exportada”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-09-20` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); `ExportacaoDominio.empresa=E` (`app/folha/models.py:77`), `mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`). Apply sem par. | O último dia ainda pertence à competência 09; zero edições de célula sem o ato próprio. Alcance da proibição sobre a linha administrativa: **regra nao decide**. | Usar limite final exclusivo e liberar indevidamente o dia 20. | borda |
| C09 | `app/ponto/janelas.py:12`: janela começa no corte do mês anterior. Aval: “cada um vai a lista do admin”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-09-21` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); somente a competência 09 está exportada na fixture: `ExportacaoDominio.mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`). Apply sem par. | O dia pertence à competência 10; a exportação da 09 não o inclui. Zero pares, zero edições de células e uma linha administrativa. | Bloquear todo setembro por mês civil ou incluir o dia 21 na competência anterior. | borda |
| C10 | `app/ponto/tests/test_o10b_editar_dia_troca.py:252`: turno que cruza meia-noite fica no dia de início. `app/ponto/tests/test_o10b_editar_dia_troca.py:7`: dia assumido normal, sem HE100 por ter sido folga. | Fixture sintética com par declarado: duas `Ausencia.colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-20` (`app/ponto/models.py:750`), `documento='cedente.pdf'/'substituto.pdf'` (`app/ponto/models.py:777`). Template do substituto: `TipoEscala.hora_inicio=19:00` (`app/escala/models.py:50`), `hora_fim=07:00` (`app/escala/models.py:51`). Competências não exportadas; confirmação pela porta (`app/ponto/portas/celula.py:632`). | A troca edita os dias de 20/10 indicados nas cartas. O turno assumido permanece no dia 20, com marcos 19:00→07:00; terminar em 21/10 não cria outra troca nem converte o turno em folga trabalhada/HE100. | Deslocar o dia assumido para a data da saída ou criar uma segunda edição no dia 21. | borda |
| C11 | `app/ponto/janelas.py:11`: competência pelo mês/ano do fim. `app/ponto/portas/celula.py:635`: cada carta determina seu próprio dia. | Fixture sintética: `Ausencia` cedente com `colaborador=col01` (`app/ponto/models.py:743`), `data=2026-12-20` (`app/ponto/models.py:750`); substituto com `colaborador=col02` (`app/ponto/models.py:743`), `data=2026-12-21` (`app/ponto/models.py:750`); ambas com `documento` preenchido (`app/ponto/models.py:777`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`). Par declarado, vínculos válidos, competências não exportadas. | Cedente editado em 20/12/2026, competência 12/2026; substituto editado em 21/12/2026, competência 01/2027. Os dois lados mantêm suas datas distintas. | Copiar a data do cedente para o substituto ou calcular a competência seguinte sem mudar o ano. | borda |
| C12 | `app/ponto/portas/celula.py:510`: DNA e marcos vêm do vínculo que responde pela data. `app/escala/alimentacao.py:232`: a geradora da célula é a autoridade. | Fixture sintética com par confirmado em 15/10: `Ausencia.data=2026-10-15` (`app/ponto/models.py:750`). Substituto: `EscalaColaborador` antigo com `colaborador=col02` (`app/escala/models.py:872`), `data_inicio=2026-10-01` (`app/escala/models.py:875`), `data_fim=2026-10-20` (`app/escala/models.py:876`), `tipo_escala=T1` (`app/escala/models.py:874`); novo vínculo desde 21/10 com `tipo_escala=T2` nos mesmos campos. `CelulaDia.colaborador=col02` (`app/escala/models.py:1681`), `data=2026-10-15` (`app/escala/models.py:1685`), `escala_geradora=antigo` (`app/escala/models.py:1692`). | A célula assumida usa o vínculo antigo e os marcos de T1. A existência de T2 como vínculo atual não substitui a autoridade daquele dia. | Usar o vínculo mais recente/ativo no lugar do vínculo do dia. | borda |
| C13 | `app/ponto/management/commands/trocas_plantao_lote_o10c.py:8`: universo composto pelas `Ausencia` do tipo legado; `app/ponto/management/commands/trocas_plantao_lote_o10c.py:37`: DRY somente lê. | Fixture sintética: consulta sem nenhuma `Ausencia.tipo='troca_de_plantao'` (`app/ponto/models.py:749`). Execução DRY, sem `--aplicar` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`). | Universo vazio: zero pares, zero linhas administrativas novas e zero escritas de células/trilhas. Texto exato da saída e rc para universo vazio: **regra nao decide**. | Criar um caso administrativo genérico mesmo sem lançamento de origem. | borda |
| C14 | `app/ponto/portas/celula.py:639`: falta de qualquer carta exige recusa antes de escrever. | Fixture sintética: duas `Ausencia.colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`). Cedente com `documento='cedente.pdf'`; substituto com `documento=''` (`app/ponto/models.py:777`). Confirmar pela porta, com `autor` e `motivo` (`app/ponto/portas/celula.py:632`). | Retorno `recusada`; zero alterações nos dois dias e zero trilhas de edição/troca. A carta do cedente não supre a do substituto. | Validar apenas o documento do cedente ou aceitar uma única carta para o par. | borda |
| C15 | `app/ponto/tests/test_o10b_editar_dia_troca.py:315`: duas confirmações concorrentes; a segunda espera a primeira e lê seu estado, resultando em no-op. `app/ponto/portas/celula.py:642`: segunda confirmação retorna `igual`, sem escrita. | Fixture sintética: mesmas duas `Ausencia`, `colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento='cedente.pdf'/'substituto.pdf'` (`app/ponto/models.py:777`). Duas confirmações simultâneas do mesmo par pela porta (`app/ponto/portas/celula.py:632`), inicialmente ainda não aplicado. | Uma confirmação retorna `trocada`; a seguinte retorna `igual`. Estado final de um único par; a segunda não altera relógios nem acrescenta trilhas. | Ler o estado antes da sincronização e repetir a escrita/trilha com base nessa leitura antiga. | adversarial |
| C16 | Aval: “o apply do lote aplica 0 pares sem erro”. Corte fornecido, mínimo (3): “rodar 2x -> ainda 1 linha por SOLO (idempotencia)”. | Fixture sintética: uma `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`). Dois applies simultâneos (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`), ambos sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Ambos continuam sem autorização para formar par: zero pares e zero edições de células. O mínimo (3) exige uma linha na repetição, mas a semântica específica de depósito concorrente, espera, conflito e recuperação: **regra nao decide**. | Fazer a segunda execução transformar o SOLO já depositado em par automático. Para duplicação causada exclusivamente pela corrida, falta oráculo concorrente explícito. | adversarial |
| C17 | `app/ponto/portas/celula.py:633`: duas edições no mesmo atomic. `app/ponto/portas/celula.py:640`: “nunca meio par”. | Fixture sintética: duas `Ausencia.colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento` preenchido (`app/ponto/models.py:777`). Células iniciais: `CelulaDia.trabalha=True/False` (`app/escala/models.py:1686`), `origem='gerada'` (`app/escala/models.py:1691`). Provocar falha entre as duas edições durante a confirmação (`app/ponto/portas/celula.py:632`). | Nenhum meio par persistido: os dois estados iniciais permanecem e nenhuma trilha de edição/troca da tentativa fica gravada. Tipo exato do erro ou retorno dessa falha: **regra nao decide**. | Confirmar a transação do primeiro lado antes de editar o segundo. | adversarial |
| C18 | Aval fornecido: “admin so recebe caso com porta”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`). Interrupção entre depósito e disponibilização da porta. Se a lista usar a candidata `Pauta`: `ancora_tipo='dia'` (`app/pautas/models.py:83`), `ancora_id='col01:2026-10-15'` (`app/pautas/models.py:84`). Associação formal SOLO→Pauta: [nao verificado]. | O admin não pode receber como caso pronto uma linha cuja porta inexiste. Como verificar alcance, o que permanece após a interrupção e como a execução retoma: **regra nao decide**. | Expor primeiro uma linha administrativa sem porta e considerá-la concluída para fins de depósito. | adversarial |
| C19 | Aval fornecido: “Os 16 lancamentos SOLO nao viram par por lote: cada um vai a lista do admin”; “substitui qualquer versao anterior”. | Fixture sintética de legado: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='rejeitada'` (`app/ponto/models.py:799`). Apply sem par (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Zero pares, zero escritas de células e uma linha administrativa para esse SOLO. O aval novo não estabelece exceção de depósito por rejeição. | Reutilizar a classificação antiga como filtro que descarta o SOLO rejeitado antes de depositá-lo. | adversarial |
| C20 | Aval: “cada um vai a lista do admin”. `app/ponto/portas/celula.py:635`: as duas cartas chegam com `documento`; `app/ponto/portas/celula.py:639`: faltar carta recusa a confirmação. | Fixture sintética de legado: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento=''` (`app/ponto/models.py:777`), `documento_referencia=outra_ausencia_com_documento` (`app/ponto/models.py:791`). Apply sem par. | Uma linha administrativa e zero pares. Se depois houver confirmação com esse lado ainda sem seu `documento`, a porta recusa antes de escrever. Equivalência entre documento referenciado e carta obrigatória do lado: **regra nao decide**; não há autorização explícita para dispensar a carta própria. | Descartar o SOLO por não possuir carta; ou considerar a mera referência autorização para aplicar a troca sem exigir as duas cartas. | adversarial |
| C21 | Aval fornecido: “Os 16 lancamentos SOLO […] cada um vai a lista do admin”. Corte fornecido: “1 linha […] por SOLO”. | Fixture sintética com dois lançamentos distintos do mesmo dia: ambas `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`); uma com `status='rejeitada'`, outra com `status='aprovada'` (`app/ponto/models.py:799`). Apply sem pares. | Duas linhas administrativas, uma por lançamento SOLO; zero pares. A repetição mantém uma linha por lançamento. O precedente de dedup por dia não altera o quantificador “cada um” do aval. | Deduplicar somente por colaborador/data e fazer um lançamento ocultar o outro. | adversarial |
| C22 | Aval fornecido: “Os 16 lancamentos SOLO nao viram par por lote: cada um vai a lista do admin […] o apply do lote aplica 0 pares sem erro”. | Lote **sintético**, não reconstrução dos registros de produção: 16 `Ausencia`, `colaborador=col01…col16` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`), `documento` preenchido (`app/ponto/models.py:777`). Apply sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | rc 0; zero pares e zero escritas de células; 16 linhas administrativas, uma por SOLO. Na segunda execução, continuam 16. Critério executável para alcance das 16 portas: **regra nao decide**. | Criar uma única linha agregada para o lote; interromper no primeiro SOLO; formar pares entre os próprios lançamentos. | golden |

- **regras que voce NAO encontrou escritas — alcance da porta:** procurei URL, formulário, ação, permissões e resposta observável que comprovem “porta já aberta/alcançável”. O aval exige o resultado, mas não define esse protocolo; a assinatura está em `app/ponto/portas/celula.py:632`. [nao verificado]
- **regras que voce NAO encontrou escritas — escrita na competência:** procurei a enumeração das entidades abrangidas por “nada se grava nela” e a conciliação com o depósito administrativo de SOLO exportado. A guarda escrita trata da edição de células em `app/ponto/portas/celula.py:548`.

[truncado em 20000 de 21876 caracteres]

## 06/11 17:45 · sessao 17c10b2746e0 · - · - · origem=Ronald (tmux)

- **Porta e atomicidade:** `app/ponto/portas/celula.py:633` declara par no mesmo atomic e retornos `trocada|igual|recusada`. `:635` diz que cada carta determina o dia do próprio lado; `:636` determina cedente como folga sem desconto; `:637` determina substituto como dia normal do próprio template. `:639` exige recusa antes de escrever quando falta lado, são o mesmo colaborador ou falta carta. `:640` determina rollback do outro lado se houver competência lavrada ou ausência de vínculo, “nunca meio par”. `:641` determina trilha somente quando algum lado mudou; `:642` determina segunda confirmação `igual`, sem escrita.
- **Estado intermediário:** `app/ponto/portas/celula.py:540` exige trilha com antes real/depois e motivo; `:541` determina que falha de trilha desfaça a edição. Falha entre lados tem caso explícito em `app/ponto/tests/test_o10b_editar_dia_troca.py:325`, com preservação das células e ausência de trilhas observadas em `:338`.
- **Concorrência:** regra explícita em `app/ponto/tests/test_o10b_editar_dia_troca.py:315`: “Duas confirmacoes concorrentes: a segunda espera a primeira e le o estado DELA (no-op).” `app/ponto/portas/celula.py:670` documenta ordem global de trava `(colaborador_id,data)`, independente do papel; o cenário cruzado está descrito em `app/ponto/tests/test_o10b_editar_dia_troca.py:443`.
- **Vínculo:** `app/ponto/portas/celula.py:510` determina vínculo escolhido para a data e marcos do template desse vínculo; `:537` reafirma DNA do vínculo do dia. `:550` determina `sem_vinculo` quando nenhum responde pelo dia.
- **Exportada:** `app/ponto/portas/celula.py:548` determina que dia em competência com TXT emitido não se edita, retorna `barrada` com frase em `barrados`; `:549` permite somente `apesar_da_lavra=<motivo>`. O lote declara nunca passar essa abertura em `app/ponto/management/commands/trocas_plantao_lote_o10c.py:46`.
- **Pauta:** `app/pautas/models.py:9` declara pauta ancorada e abertura do objeto; `:33` define âncora dia `<colab_id>:<AAAA-MM-DD>`. `app/pautas/services.py:156` declara `escrever` como único caminho de escrita. Precedente de dedup por todo histórico, incluindo feitas/canceladas, é específico do limbo, em `app/chamados/services/fabrica_da_celula.py:228`; não constitui regra escrita de dedup de SOLO.
- **DRY:** `app/ponto/management/commands/trocas_plantao_lote_o10c.py:37` determina somente leitura e nenhuma chamada à porta. `app/ponto/tests/test_o10c_lote_trocas.py:14` também determina nenhuma escrita sem `--aplicar`.
- **Par declarado:** regra normativa anterior preservável em `app/ponto/tests/test_o10c_lote_trocas.py:6`: aprovado, colega declarado, duas cartas e fora de lavrada vira par pela porta; cedido folga e assumido normal. `:12` determina segunda rodada sem nova escrita; `:13` proíbe desconto novo.

Definições dos campos:

| Modelo | Campos com definição |
|---|---|
| `Ausencia` | `colaborador` `app/ponto/models.py:743`; `tipo` `:749`; `data` `:750`; `data_fim` `:751`; `hora_ini` `:768`; `hora_fim` `:770`; `descricao` `:776`; `documento` `:777`; `documento_referencia` `:791`; `status` `:799`; `registrado_por` `:801` |
| `CelulaDia` | `colaborador` `app/escala/models.py:1681`; `data` `:1685`; `trabalha` `:1686`; `origem` `:1691`; `escala_geradora` `:1692`; `dna` `:1697`; `editada_em` `:1732`; `editada_por` `:1733` |
| `Pauta` | `destinatario_departamento` `app/pautas/models.py:76`; `destinatario_user` `:78`; `ancora_tipo` `:83`; `ancora_id` `:84`; `texto` `:89`; `feito_em` `:93`; `cancelado_em` `:97`; `pai` `:101` |
| `LogAuditoria` | `usuario` `app/core/models.py:295`; `acao` `:296`; `modelo` `:297`; `objeto_id` `:298`; `descricao` `:299`; `valor_antes` `:300`; `valor_depois` `:301` |

Não encontrei regra escrita específica de SOLO para concorrência de depósito em Pauta, fechamento/cancelamento e reexecução, falha entre depósito e disponibilização da porta, chave por lançamento versus colaborador/dia, nem definição operacional de “porta já aberta/alcancável”; o aval fornecido exige o resultado, mas não detalha essas decisões. `[nao verificado]`

## 06/11 17:45 · sessao 9ad22bc8dbbb · - · - · origem=Ronald (tmux)

Regras escritas e campos conferidos, somente por leitura:

- Competência: `app/ponto/janelas.py:10` define competência pelo mês/ano do fim; `:12` manda começar no corte do mês anterior e terminar em corte−1; `:13` explicita 21→20. Campo `Empresa.dia_inicio_competencia`: `app/colaboradores/models.py:46`.
- Vínculo trocado: `app/ponto/portas/celula.py:507`–`:511` manda produzir DNA e marcos pelo vínculo que responde à data. `app/escala/alimentacao.py:231`–`:233` declara `CelulaDia.escala_geradora` como autoridade; `:244`–`:246` disciplina ausência de célula/geradora; `:249`–`:252` exclui vínculo desativado sem fim declarado.
- Exportada: `app/ponto/portas/celula.py:548`–`:550` estabelece que dia com TXT emitido não se edita, retorna `barrada`, registra frase e só abre por `apesar_da_lavra=<motivo>`. Isso descreve edição da célula; não define integralmente “nada se grava na competência”.
- Troca confirmada: `app/ponto/portas/celula.py:633`–`:642` exige duas edições no mesmo atomic; cada `Ausencia.data` é o dia daquele lado; cedente vira folga e substituto vira trabalho normal; recusa lado/carta ausente; nunca deixa meio par; repetição retorna `igual` sem escrita.
- Cross-meia-noite: caso normativo nomeado `test_B07_turno_que_cruza_a_meia_noite_fica_no_dia_em_que_comeca`, `app/ponto/tests/test_o10b_editar_dia_troca.py:252`; preservação dos marcos 19:00→07:00 em `:257`. O aval literal desse arquivo, `:4`–`:9`, exige dia assumido normal, sem HE100 por ter sido folga.
- Concorrência de confirmação: regra textual no docstring `app/ponto/tests/test_o10b_editar_dia_troca.py:315`: “Duas confirmacoes concorrentes: a segunda espera a primeira e le o estado DELA (no-op).”
- Estado intermediário: regra “nunca meio par” em `app/ponto/portas/celula.py:640`; caso de falha entre lados em `app/ponto/tests/test_o10b_editar_dia_troca.py:325`.
- Vazio: recusa lado ausente escrita em `app/ponto/portas/celula.py:639`; par inteiramente vazio exercitado em `app/ponto/tests/test_o10b_editar_dia_troca.py:304`.

Definições dos campos:

| Model | Campos e definição |
|---|---|
| `Ausencia` | `colaborador` `app/ponto/models.py:743`; `tipo` `:749`; `data` `:750`; `data_fim` `:751`; `documento` `:777` |
| `EscalaColaborador` | `colaborador` `app/escala/models.py:872`; `posto` `:873`; `tipo_escala` `:874`; `data_inicio` `:875`; `data_fim` `:876`; `ativa` `:877`; `data_ancora_colaborador` `:974` |
| `TipoEscala` | `hora_inicio` `app/escala/models.py:50`; `hora_fim` `:51`; `hora_inicio_intervalo` `:52`; `hora_fim_intervalo` `:53` |
| `CelulaDia` | `colaborador` `app/escala/models.py:1681`; `data` `:1685`; `trabalha` `:1686`; `origem` `:1691`; `escala_geradora` `:1692`; `dna` `:1697` |
| `ExportacaoDominio` | `empresa` `app/folha/models.py:77`; `mes` `:78`; `ano` `:79`; `gerado_por` `:81`; `nome_arquivo` `:82`; `hash_sha256` `:83` |

Não encontrei regra de atomicidade/idempotência concorrente do depósito SOLO em lista administrativa; a regra de concorrência localizada é da confirmação da troca. `[nao verificado]`

## 06/11 17:44 · sessao 99a3602610bb · PAUTA-08 · verificador · origem=Code

1. **O texto novo mede o estado atual, mas a afirmação “hash conferido intacto” pode ser persistida antes da conferência.** A cabeça é gravada dentro da transação por colaborador; a comparação final fica depois desses commits ([comando:198](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198), [comando:219](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219)).

| Afirmação de `_cabeca` | O que a sustenta / limite |
|---|---|
| Identidade `colNN` e competência | Montadas com PK, mês e ano recebidos ([comando:54](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:54), [comando:67](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:67)). |
| “EXPORTADA (TXT vigente)” | Consulta `competencia_entregue` por empresa/competência; o juiz verifica registro vigente não invalidado ([comando:124](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124), [export.py:581](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/folha/export.py:581)). |
| “PAGO = o gravado da folha de hoje” | Lê o `FechamentoMensal` atual, recortado pelos campos canônicos. Igualdade com o TXT exportado ou pagamento efetivo no Domínio: **não medida** ([comando:130](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130), [regen_exportada.py:77](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:77)). |
| “hash conferido intacto NESTE ato” | Compara hash inicial e final ([comando:120](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:219](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219)). Se divergir, lança erro **depois de gravar as pautas**; elas podem sobreviver com a afirmação não confirmada ([comando:189](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189), [comando:221](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:221)). |
| “DEVIDO = o calculo de hoje, lido sem gravar” | Chama `somente_leitura=True`; o ramo retorna os campos calculados em vez de salvar o fechamento ([comando:135](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135), [fechamento.py:1112](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/fechamento.py:1112)). |
| “N campo(s) diferem” | Conta os campos selecionados por `_diferente`, excluindo mês/ano. Diferença numérica significa tolerância `> 0.015` ([comando:139](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139), [comando:174](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174), [regen_exportada.py:248](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:248)). |
| “os dois numeros vao abaixo” | Monta `campo: pago X -> devido Y`, distribui em filhas e as escreve ([comando:175](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175), [comando:202](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202)). |
| “A diferenca NAO e atribuida a causa neste ato” | Condiz com a saída: os itens contêm campo e valores, sem atribuição causal ([comando:66](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66), [comando:175](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175)). |
| “Conferir com o pago no Dominio” | Instrução humana; conferência **não medida** pelo comando ([comando:66](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66)). |
| “(L-092)” | Referência normativa, não medição ([comando:65](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:65)). |

“Intacto” significa igualdade do hash nos dois extremos, dentro dos campos cobertos; há exclusões explícitas, e o hash é MD5 truncado em 12 caracteres. Isso não demonstra história desde a exportação nem ausência de alterações intermediárias ([regen_exportada.py:37](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:37), [regen_exportada.py:56](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:56), [regen_exportada.py:74](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:74)).

2. **Sim, restam expressões equivalentes, com escopos distintos.**

- No comando: “sem tocar o gravado” na docstring; “antes e depois […] nao se moveu”; help “Nao toca o gravado”. Referem-se ao ato, sem demonstrar a história anterior ([comando:2](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:2), [comando:14](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:14), [comando:72](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:72)).
- Na docstring do teste: “PAGO […] L-092: nao se toca”; a propriedade descrita é hash antes/depois ([teste:2](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:2), [teste:13](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:13)).
- Em outro comando de `pautas/`: “O TXT exportado NAO foi tocado”, sobre **TXT**, além de “sem tocar o gravado” na docstring e no help ([pauta_folga_nao_definida_exportada.py:33](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:33), [mesmo arquivo:2](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:2), [mesmo arquivo:39](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:39)).
- O literal velho também permanece como texto rejeitado no comentário/assert do teste; não é uma afirmação emitida ao DP ([teste:123](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:123), [teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127)).

3. **O teste rejeita o texto velho literal, mas não qualquer variante histórica.** Ele apenas proíbe `NAO foi tocado` e exige `conferido intacto NESTE ato` ([teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127), [teste:128](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:128)).

Um mutante que passa por essas duas asserções: conservar o texto novo e acrescentar **“Desde a exportacao, o gravado permaneceu inalterado.”** Mantém a substring exigida, evita a proibida e volta a afirmar uma história que a comparação inicial/final não mede ([teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127), [teste:128](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:128), [comando:120](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:219](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219)). Execução da suíte Django: **[nao verificado]**.

4. **Escritores e leitores localizados:**

| Quem | Operação sobre `Pauta.texto` |
|---|---|
| Comando → serviço | Cabeça e filhas passam por `_ps.escrever`; o serviço persiste `texto[:500]` ([comando:198](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198), [comando:202](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202), [services.py:185](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/services.py:185)). |
| Próprio comando | Localiza cabeça por prefixo; seleciona filhas por prefixo e compara textos completos; reversão verifica a marca inicial ([comando:234](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:234), [comando:241](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:241), [comando:268](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:268)). |
| Painel de pautas | Serializa cabeça/respostas; navegador mostra ambas por `textContent` ([views.py:40](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/views.py:40), [views.py:47](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/views.py:47), [_copiloto.html:405](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/templates/core/_copiloto.html:405), [_copiloto.html:424](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/templates/core/_copiloto.html:424)). |
| API/mensageria | API entrega texto literal; cliente busca o JSON e ferramenta repassa os itens ([views_mensageria.py:1334](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/api/views_mensageria.py:1334), [core_client.py:431](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/mensageria/nucleo/core_client.py:431), [ferramentas.py:2241](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/mensageria/nucleo/ferramentas.py:2241)). |
| Testes do comando | Inspecionam prefixo, valores das filhas e substrings da cabeça ([teste:105](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:105), [teste:108](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:108), [teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127)). |

**Não identifiquei leitor que quebre ao parsear a prosa alterada:** os reconhecedores desse comando usam marca/chave preservadas; UI e API exibem ou transportam o texto ([comando:41](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:41), [comando:54](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:54), [comando:234](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:234), [_copiloto.html:405](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/templates/core/_copiloto.html:405)). Existem parsers de outras famílias, protegidos por outros marcadores, como Esmeril e Discordantes ([pautas_do_esmeril.py:63](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/pautas_do_esmeril.py:63), [pautas_dos_discordantes.py:165](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/pautas_dos_discordantes.py:165)). Leitores externos: **[nao verificado]**.

**Cabeças antigas não são corrigidas pelo comando:** encontrar uma existente provoca `continue`; a comparação posterior considera apenas filhas. Assim, uma cabeça antiga pode conservar a frase velha; ocorrência real em produção: **[nao verificado]** ([comando:193](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193), [comando:241](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:241)).

5. **Sim: `LEIS.md:177` ainda contém uma generalização histórica contradita pelos docs.**

- **“Até 06/11 […] nenhum código criava”** conflita com o registro de **19/10** de Pauta DP com `pago -> devido` por campo. A cópia contém essa montagem e a escrita de cabeça/filhas em outro fluxo ([LEIS.md:177](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:177), [RELATO-ARQUIVO.md:43468](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/RELATO-ARQUIVO.md:43468), [RELATO-ARQUIVO.md:43472](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/RELATO-ARQUIVO.md:43472), [passivo_validado.py:192](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/chamados/services/passivo_validado.py:192), [passivo_validado.py:229](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/chamados/services/passivo_validado.py:229)). Esse fluxo retifica o gravado; é diferente do comparador somente leitura da PAUTA-08 ([passivo_validado.py:438](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/chamados/services/passivo_validado.py:438), [comando:135](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135)).
- **“Caminho AUTOMATICO segue sem codigo” precisa delimitar a família.** A autorização de HE já gera automaticamente Pauta DP em exportada após autorização, com pago da lavratura e devido do motor. Isso não implementa o comparador genérico da PAUTA-08 ([autorizacao_he_periodo.py:174](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/autorizacao_he_periodo.py:174), [mesmo arquivo:184](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/autorizacao_he_periodo.py:184), [mesmo arquivo:299](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/autorizacao_he_periodo.py:299)).
- **Há diferença de referência entre linhas da própria LEIS:** a linha 125 exige **exportado × motor de hoje**; a 177 descreve **gravado atual × recálculo**. O comando não demonstra equivalência entre gravado atual e exportado ([LEIS.md:125](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:125), [LEIS.md:177](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:177), [comando:130](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130)).
- A pauta manual é compatível com os cortes que encaminham diferenças ao DP e com a revogação da pauta como **pré-condição**: deixar de exigir previamente não impede sua emissão ([CORTES.md:353](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/CORTES.md:353), [LEIS.md:150](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:150), [LEIS.md:198](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:198)).

## 06/11 17:42 · sessao eb897eabe44c · - · - · origem=Ronald (tmux)

2. Restam expressões equivalentes, mas com escopos diferentes:

- `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:2`: “sem tocar o gravado”; `:14`: hash “antes e depois”, prova de que “nao se moveu”; `:72`: help “Nao toca o gravado”. São alegações sobre o ato, não prova da história desde o export (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`).
- `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:2`: docstring “PAGO (... L-092: nao se toca)”. A mesma docstring explicita hash antes/depois em `:13`.
- Outro comando conserva literalmente “O TXT exportado NAO foi tocado”, referente ao TXT: `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:33`. Também conserva “sem tocar o gravado” na docstring `:2`, exclusão de escritas em `:9` e help em `:39`.
- A ocorrência “nao tocada(s)” em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:277` refere-se às pautas alheias puladas na reversão (`:268`), não ao gravado.
- O literal velho aparece no comentário e na asserção negativa do teste, não como afirmação ao DP: `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:123` e `:127`.

5. Há problemas de escopo na linha alterada:

- **Contradição documental concreta:** “até 06/11 [...] nenhum codigo criava” (`app/docs/LEIS.md:177`) colide com o marco de **19/10** que registra Pauta DP com `pago -> devido` por campo (`app/docs/RELATO-ARQUIVO.md:43468`, `:43472`). Outro marco de 19/10 registra cabeça e filhas escritas pela mesma família (`app/docs/RELATO-ARQUIVO.md:43412`, `:43419`). O código da cópia confirma os pares em `app/chamados/services/passivo_validado.py:192` e a escrita em `:229` e `:233`; o chamador associa regeneração e pauta em `:438` e `:443`. Esses caminhos retificam o gravado; distinguem-se do comando PAUTA-08 (`app/chamados/services/passivo_validado.py:423`).
- **“Caminho AUTOMATICO segue sem codigo” precisa restringir a família:** a autorização de HE em exportada já chama a pauta quando houve autorização (`app/ponto/services/autorizacao_he_periodo.py:299`). Ela lê pago da lavratura e devido do motor (`:174`–`:176`), escreve os dois números no texto (`:184`) e grava Pauta DP (`:244`). Essa classe já está descrita em `app/docs/DOSSIES.md:279` e `:287`; não é o comparador genérico FM atual × recálculo somente_leitura.
- **Diferença de referência:** `app/docs/LEIS.md:125` define os dois números como **exportado** × motor de hoje; `:177` descreve **gravado atual** × recálculo. O comando lê FM atual (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`), portanto não demonstra equivalência com o exportado.
- A existência da pauta manual não contradiz a revogação de sua exigência como **pré-condição** em `app/docs/LEIS.md:150` e `:198`. Os cortes que mandam encaminhar diferenças ao DP são compatíveis com essa saída (`app/docs/CORTES.md:353`, `:387`, `:402`).

## 06/11 17:41 · sessao f162274a47d4 · - · - · origem=Ronald (tmux)

Auditoria do contrato, por inspeção estática:

| Afirmação de `_cabeca` | Evidência e limite |
|---|---|
| Identidade `col<pk>` e competência | Montadas por `_chave`, sem medição histórica (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53-54`, `:67`). |
| `EXPORTADA (TXT vigente)` | Consulta `competencia_entregue` por empresa e competência (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:123-125`); juiz verifica registro vigente e não invalidado (`app/folha/export.py:580-586`). Não verifica conteúdo físico do TXT nem pagamento efetivo no Domínio (`app/folha/export.py:580-586`). |
| `PAGO = o gravado da folha de hoje` | Lê `valores_do_gravado` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`), que consulta `FechamentoMensal` e recorta `CAMPOS` (`app/ponto/portas/regen_exportada.py:77-84`). Mede gravado atual; equivalência com dinheiro efetivamente pago é **não medida**. |
| `hash conferido intacto NESTE ato` | Compara hash inicial (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`) com final (`:219-222`). **Ressalva material:** persiste cabeça/filhas e encerra transação antes dessa conferência (`:189-209`); se hash divergir, lança erro depois dos commits, podendo sobreviver pauta com afirmação não confirmada (`:219-222`). |
| `DEVIDO = o calculo de hoje, lido sem gravar` | Chamada com `somente_leitura=True` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`); ramo coleta dicionário em vez de salvar (`app/ponto/services/fechamento.py:1112-1128`), e `lavrar_dias_pagos` tem default falso (`:439-441`, `:1175`). |
| `N campo(s) diferem` | Lista por `_diferente`, excluindo `mes/ano`, e tamanho usado na cabeça (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`, `:174`). “Diferem” usa tolerância numérica `>0.015` (`app/ponto/portas/regen_exportada.py:248-252`). |
| `os dois numeros vao abaixo` | Gera itens pago/devido e filhas (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175-177`); persiste filhas (`:201-204`). |
| `A diferenca NAO e atribuida a causa neste ato` | Texto não atribui causa; itens só contêm campo e valores (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66`, `:175-177`). Não existe medição causal nesse fluxo (`:120-140`). |
| `Conferir com o pago no Dominio` | Instrução humana, **não medida** pelo comando (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66`). |
| `(L-092)` | Referência normativa, não medição (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:65`). |

O hash é MD5 truncado em 12 caracteres e cobre campos concretos menos exclusões explícitas, incluindo carimbos, identidade e aprovação; portanto “intacto” significa igualdade desse hash nos extremos, não ausência de qualquer escrita ou história desde a exportação (`app/ponto/portas/regen_exportada.py:35-38`, `:56-74`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`, `:219`).

O teste novo derruba o texto velho literal: proíbe `NAO foi tocado` e exige `conferido intacto NESTE ato`; não barra qualquer equivalente semântico (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127-128`). Mutante que passaria por essa lógica: conservar o cabeçalho atual e acrescentar **“O gravado permanece identico desde a exportacao.”** A nova frase evita o literal proibido e preserva o exigido, apesar de alegar história não medida (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127-128`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`, `:219`). Execução do mutante: `[nao verificado]`.

## 06/11 17:41 · sessao a00820d944e2 · - · - · origem=Ronald (tmux)

**Quem escreve**

- O comando monta cabeça e filhas em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`, `:175` e `:177`; pede a escrita da cabeça com `texto=cab` em `:198`–`:199` e das filhas com `texto=f` em `:202`–`:204`.
- O autor vem de `autor_sistema(informado='ti')` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:188`. O service efetivamente cria a linha em `app/pautas/services.py:180`, gravando `texto=texto[:500]` em `:185`; o campo tem teto 500 em `app/pautas/models.py:89`.

**Quem lê**

- UI: `app/pautas/views.py:33` consulta as pautas; `:40` serializa a cabeça e `:46`–`:48` serializam as respostas. O navegador exibe os textos diretamente usando `textContent`, em `app/templates/core/_copiloto.html:405` e `:424`.
- API de mensageria: consulta a aba, normalmente DP, em `app/api/views_mensageria.py:1324`–`:1329` e entrega `p.texto` literalmente em `:1334`. `mensageria/nucleo/core_client.py:431`–`:438` obtém o JSON e `mensageria/nucleo/ferramentas.py:2241`–`:2248` repassa seus itens.
- O próprio comando lê o texto para localizar cabeça por `texto__startswith=chave + ' '` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:233`–`:234`; seleciona filhas por prefixo e compara seus textos integralmente em `:241`–`:243`; na reversão exige `p.texto.startswith(MARCA)` em `:268`.

**Risco de parsing**

- Os leitores concretos acima não dependem da prosa alterada: os reconhecedores usam a marca e a chave definidas em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:41` e `:53`–`:54`, enquanto o delta muda o trecho posterior em `:64`–`:66`. UI e API transportam/exibem o texto diretamente (`app/pautas/views.py:40`; `app/templates/core/_copiloto.html:405`; `app/api/views_mensageria.py:1334`).
- Leitor externo ou em produção que aplique regex/split à prosa: **[nao verificado]**.

**Consequência adicional**

- A nova redação não corrige cabeças já existentes: a execução encontra a cabeça e continua sem reescrevê-la em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`–`:197`; a verificação de divergência compara somente filhas em `:241`–`:243`. Portanto uma cabeça antiga pode continuar exibindo a frase velha; existência efetiva em produção **[nao verificado]**.

## 06/11 17:35 · sessao d510127cbf40 · E-SEM-VIVA-B · verificador · origem=Code

**Reprovaria a volta 4.** O desligado sem demissão ainda pode entrar com fechamento preexistente; datas de férias passam como dias do retentor; e o vigia pode rotear outra competência apenas pela presença do colaborador na C5. `app/folha/export.py:775`, `app/escala/services/cadastro_realidade.py:184`, `app/folha/vigia_retido_calado.py:43`.

Os achados abaixo são de código. Ocorrência e quantidades em produção, execução da suíte Django e comparação integral com `24946096`: **[nao verificado]**.

1. **ALTA — a exclusão do desligado sem `data_demissao` depende de nunca ter existido fechamento.**

   O universo do fechamento exclui desligado sem data, mas `_K2` consulta os `FechamentoMensal` existentes diretamente, sem reaplicar esse universo. Com data nula, `sai_pela_rescisao` responde falso e o retentor não recorta o fim da janela. Portanto, um fechamento anterior à alteração cadastral pode produzir uma **lavra nova** com dias aceitos pela C5. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430`, `app/folha/export.py:775`, `app/folha/export.py:60`, `app/folha/export.py:1198`, `app/escala/services/cadastro_realidade.py:186`.

   O teste mascara esse histórico: apaga a data **antes** de criar fechamentos; `_fechar` só faz `get_or_create`. Ele prova ausência de fechamento naquela fixture, não exclusão de um desligado sem data que já possuía fechamento. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:140`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:60`.

   **Como provar:** no caso positivo existente, executar conceitualmente `self._fechar()`, depois retirar `data_demissao`, depois `self._lavrar()`. As células continuam acusadoras, o fechamento continua existente e a C5 aceita suas datas. Esse cenário repete a ampliação reprovada na volta 3; sua execução: **[nao verificado]**. `app/escala/models.py:1767`, `app/folha/export.py:1233`, `app/escala/services/cadastro_realidade.py:183`.

2. **MÉDIA — “datas de qualquer bloqueado” não equivale aos dias de `dias_que_seguram`.**

   A C5 aceita as datas de **todo** item do balde `bloqueado`; a autoridade `dias_que_seguram` só preenche seu mapa de dias para `furo_espelho`. `app/escala/services/cadastro_realidade.py:184`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`.

   O escritor legítimo também grava `datas` para `ferias_com_batida`. Logo, não é necessário fabricar JSON inválido para obter a divergência. `app/folha/export.py:868`, `app/folha/export.py:875`, `app/folha/retrato_folha.py:1172`, `app/folha/models.py:323`.

   **Como provar:** desligado com demissão depois do fim da competência, fechamento, férias com batida e uma célula ainda `nunca_bateu` naquele dia. O lavrador grava a data de férias; a C5 a aceita; `dias_que_seguram()['dias']` não contém o colaborador por esse motivo. A própria casa reconhece o intervalo entre primeira batida e rejulgamento de `nunca_bateu`; ocorrência desse caso específico: **[nao verificado]**. `app/folha/export.py:858`, `app/folha/retrato_folha.py:1175`, `app/escala/services/cadastro_realidade.py:188`, `app/chamados/services/adesao.py:59`.

3. **MÉDIA — o vigia perde competência e datas e pode reduzir `calados` além do aval.**

   `censo` transforma o retorno da C5 em um conjunto de IDs. Depois considera roteada qualquer linha bloqueada com fio `ninguem` daquele colaborador, em qualquer retrato selecionado da última lavra. `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:48`.

   **Como provar com as fontes reais:** colaborador desligado com demissão futura, um `nunca_bateu` retido na anterior e dias sem célula, mas cobertos por vínculo, na corrente. O retentor pode gerar `sem_celula` na corrente; sem célula, o fio resulta sem porta. A presença na C5 pela anterior roteia também a corrente, embora seus dias não estejam na C5. `app/folha/export.py:1227`, `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:78`, `app/folha/vigia_retido_calado.py:48`.

   O resultado condicional é `roteados=2` e `calados=0` para duas linhas antes caladas, apesar de só uma competência ter dias C5. Isso já era possível para ativos; o diff estende o alcance aos desligados. Ocorrência em produção: **[nao verificado]**. `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:53`, `app/escala/services/cadastro_realidade.py:188`.

4. **MÉDIA — o universo de empresas do leitor é maior que o do lavrador.**

   A C5 consulta todas as empresas ou os IDs recebidos. O lavrador parte de `empresas_visiveis()` inclusive quando recebe `--empresa`; essa fonte exclui empresas inativas e CNPJ começando por `11222333`. `app/escala/services/cadastro_realidade.py:169`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:51`, `app/colaboradores/queries.py:85`.

   **Como provar:** numa empresa excluída, manter fechamento e dia que a autoridade viva retém, sem retrato. A autoridade pode responder positivamente, mas C5 devolve ausência. Se houver retrato de quando a empresa era visível, C5 pode continuar usando uma lavra que o command já não atualiza. `app/folha/export.py:778`, `app/escala/services/cadastro_realidade.py:183`, `app/escala/services/cadastro_realidade.py:188`.

   Os CNPJs das fixtures novas não exercitam a exclusão `11222333*`; suas empresas são criadas ativas. A tela também usa `empresas_visiveis`, enquanto o vigia admite IDs diretamente. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:48`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:52`, `app/escala/views.py:487`, `app/folha/vigia_retido_calado.py:34`.

5. **MÉDIA — falta positivo de desligado retido na competência corrente.**

   O mutante plausível **M1** — trocar `for _ini, fim in janelas:` por `for _ini, fim in janelas[:1]:` — deixa de consultar o retrato corrente e preserva, por inspeção, os resultados esperados dos testes novos/alterados. Os positivos de desligados exigem somente dias da anterior; os ativos dispensam a lavra. Execução integral contra o mutante: **[nao verificado]**. `app/escala/services/cadastro_realidade.py:181`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:120`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:223`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:243`.

   **Como provar a lacuna:** desligado com demissão depois de 20/11, célula `nunca_bateu` em 23/10, fechamento e lavra da competência 11; exigir 23/10 na C5. Esse é o ramo que M1 elimina. `app/ponto/janelas.py:24`, `app/escala/services/cadastro_realidade.py:172`, `app/escala/services/cadastro_realidade.py:181`.

6. **MÉDIA — o selo não certifica a promessa que anuncia.**

   A C5 lê `situacao`, assim como `_K2` e o universo do fechamento; o campo foi omitido de `CAMPOS_DA_AUTORIDADE`. Portanto, “nenhum campo que o juiz lê é lido aqui” é falso pelo próprio código. Isso não significa que retirar a distinção ativo/desligado seria correto: ela sustenta a exceção aprovada para ativos. `app/escala/services/cadastro_realidade.py:151`, `app/escala/services/cadastro_realidade.py:178`, `app/folha/export.py:802`, `app/ponto/services/fechamento.py:431`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:260`.

   O detector só examina atributos e argumentos posicionais de algumas chamadas. Uma leitura por `filter(data_demissao__isnull=...)` não é detectada; helpers externos também ficam fora da função inspecionada. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:273`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:294`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:298`.

   Basta ainda existir uma chamada a `linhas_do_retrato`, mesmo com retorno descartado, para cumprir a exigência positiva. O selo não verifica que o resultado dessa chamada governa a seleção; o reconhecimento de código morto limita-se ao `if` com constante falsa. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:289`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:291`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`.

   **Como provar:** inserir uma consulta ORM ao campo proibido, ou manter uma chamada descartada à porta e selecionar por leitura paralela. Ambos satisfazem o detector atual; as sondas AST em memória confirmaram essas brechas. A suíte Django com esses mutantes: **[nao verificado]**. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:277`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:302`.

A equivalência universal perguntada no item 2 é, portanto, **não**. Além dos achados acima, há estes limites específicos:

| Caso | Resposta pelo código |
|---|---|
| Competência e corte da empresa | Leitor e lavrador usam mês/ano do **fim** de `janela_anterior` e `janela_atual`. Para o mesmo dia e empresa, não encontrei divergência de idioma, inclusive no dia do corte. `app/escala/services/cadastro_realidade.py:172`, `app/escala/services/cadastro_realidade.py:182`, `app/folha/management/commands/lavrar_retrato_folha.py:60`, `app/ponto/janelas.py:27`. |
| Lavra versus autoridade viva | A divergência é deliberadamente aceita pelo teste: mudar a demissão depois da lavra mantém os dias antigos até relavrar. Assim, a C5 não garante “antes da demissão atual”. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:169`. |
| Readmissão | A reativação muda para ativo e limpa a demissão; C5 passa a devolver todos os candidatos, dispensando a lavra anterior. Preserva a exceção dos ativos, mas refuta uma descrição irrestrita de “somente a lavra”. `app/colaboradores/services/desligamento.py:210`, `app/colaboradores/services/desligamento.py:211`, `app/escala/services/cadastro_realidade.py:188`. |
| `colab` repetido | C5 une as datas de todas as linhas bloqueadas; `linha_do_colab` responde pela primeira linha encontrada. O JSON não impõe unicidade interna, mas o escritor normal deriva uma linha por fechamento/item. Duplicata real: **[nao verificado]**. `app/escala/services/cadastro_realidade.py:186`, `app/folha/retrato_folha.py:935`, `app/folha/models.py:348`, `app/folha/retrato_folha.py:1162`, `app/ponto/models.py:485`. |
| Datas não ISO | Não casam com `d.isoformat()`. O escritor normal produz `str(date)` e documenta ISO; portanto, não há evidência de formato alternativo legítimo no caminho examinado. Dados assim em produção: **[nao verificado]**. `app/escala/services/cadastro_realidade.py:188`, `app/folha/fora_da_folha.py:192`, `app/folha/retrato_folha.py:1175`, `app/folha/models.py:332`. |
| Bloqueado por outro motivo | Ausência de datas exclui; presença de datas inclui, independentemente de ser `furo_espelho`. É o contraexemplo de férias do achado 2. `app/escala/services/cadastro_realidade.py:184`, `app/escala/services/cadastro_realidade.py:186`. |

Quanto ao item 3, **não encontrei a recomposição paralela da volta 3 no novo corpo de produção**: ele lê `linhas_do_retrato`; não chama rescisão ou retentor. `_K2` continua registrado como `classificar_export`. O defeito está na equivalência assumida para o conteúdo lido e no estado atual usado para escolher quem passa por essa leitura. `app/escala/services/cadastro_realidade.py:162`, `app/escala/services/cadastro_realidade.py:184`, `app/core/juizes.py:299`, `app/docs/JUIZES.md:74`.

Também não há novo escritor persistente no corpo alterado de C5/vigia: são consultas, composição do retorno e contagens. As escritas diretas dos testes são fixtures. `app/escala/services/cadastro_realidade.py:173`, `app/folha/vigia_retido_calado.py:33`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:42`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:195`.

O command agora lê o dia na borda e o repassa. A assinatura permissiva de `censo` ainda permite omissão, que chega ao fallback de relógio de C5; esse fallback não nasceu neste diff. O lavrador e o retentor também conservam suas leituras próprias de relógio. `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/vigia_retido_calado.py:27`, `app/folha/vigia_retido_calado.py:43`, `app/escala/services/cadastro_realidade.py:167`, `app/folha/management/commands/lavrar_retrato_folha.py:48`, `app/folha/export.py:1197`.

**Rastreio dos escritores e leitores solicitados:**

| Campo/valor | Quem produz e escreve | Quem lê |
|---|---|---|
| `RetratoFolha.linhas` | `montar_linhas` monta; `lavrar_retrato_folha --apply` persiste por empresa/competência. `app/folha/retrato_folha.py:1156`, `app/folha/retrato_folha.py:1177`, `app/folha/management/commands/lavrar_retrato_folha.py:74`. | Porta `linhas_do_retrato`; `__str__` lê o tamanho. `app/folha/retrato_folha.py:110`, `app/folha/models.py:368`. |
| Chave `colab` | ID do item devolvido por `_K2`, gravado por `montar_linhas`. `app/folha/export.py:830`, `app/folha/retrato_folha.py:1163`, `app/folha/retrato_folha.py:1178`. | C5; vigia; ordenação; lookup do drawer; portas e tabela. `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:48`, `app/folha/retrato_folha.py:756`, `app/folha/retrato_folha.py:936`, `app/folha/retrato_folha.py:1010`, `app/folha/views_retrato.py:272`. |
| Chaves `status`, `motivo` | `_K2` decide; `montar_linhas` copia. `app/folha/export.py:833`, `app/folha/export.py:859`, `app/folha/export.py:865`, `app/folha/export.py:870`, `app/folha/export.py:960`, `app/folha/retrato_folha.py:1180`. | Balde, fio, tipo, saldo, ação e apresentação. `app/folha/retrato_folha.py:91`, `app/folha/retrato_folha.py:441`, `app/folha/retrato_folha.py:491`, `app/folha/retrato_folha.py:501`, `app/folha/retrato_folha.py:1062`, `app/folha/views_retrato.py:284`. |
| Chave `datas` | Dias do retentor passam por `dias_que_seguram` e `fora_da_folha`; férias vêm do item de `_K2`; `montar_linhas` grava ambas as formas. `app/folha/export.py:1221`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:192`, `app/folha/export.py:875`, `app/folha/retrato_folha.py:1175`, `app/folha/retrato_folha.py:1183`. | C5; teto temporal de dono; recorte por período. `app/escala/services/cadastro_realidade.py:186`, `app/folha/retrato_folha.py:478`, `app/folha/retrato_folha.py:1233`. |
| `CelulaDia.veredito` | Método do modelo delega à porta; porta atribui e persiste. Chamadores: cartório e decisão administrativa. `app/escala/models.py:1819`, `app/ponto/portas/celula.py:814`, `app/ponto/portas/celula.py:819`, `app/ponto/services/cartorio.py:889`, `app/ponto/services/cartorio.py:1242`, `app/ponto/services/cartorio.py:1802`, `app/ponto/services/veredito_celula.py:330`. | Inventário abaixo. A cópia em memória também altera o valor, dentro da própria porta. `app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:726`. |
| `Colaborador.situacao` | Default ativo; operação declarada na porta; desligamento, reativação, afastamento, reversão e admissão determinam os valores. `app/colaboradores/models.py:323`, `app/colaboradores/services/colaborador.py:32`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`, `app/ponto/views.py:2435`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`, `app/colaboradores/views.py:1205`, `app/colaboradores/views.py:2610`. | Inventário abaixo; o admin exibe/filtra, mas declara o campo somente leitura. `app/colaboradores/admin.py:49`, `app/colaboradores/admin.py:71`. |

Os consumidores externos de `RetratoFolha.linhas` passam pela porta na tela do retrato, drawer do colaborador, vigia e nova C5. Internamente, a porta alimenta contadores, filtros, agrupamentos, ordenação, LEDs, ações e recorte temporal. `app/folha/views_retrato.py:196`, `app/colaboradores/views.py:982`, `app/folha/vigia_retido_calado.py:46`, `app/escala/services/cadastro_realidade.py:184`, `app/folha/retrato_folha.py:120`, `app/folha/retrato_folha.py:617`, `app/folha/retrato_folha.py:657`, `app/folha/retrato_folha.py:751`, `app/folha/retrato_folha.py:547`, `app/folha/retrato_folha.py:1028`, `app/folha/retrato_folha.py:1195`.

O retorno C5 tem este encadeamento de leitores, inclusive fora do diff:

| Retorno | Leitores |
|---|---|
| `colabs_nunca_bateu` | `_nunca_bateu` e `censo`. `app/escala/services/cadastro_realidade.py:197`, `app/folha/vigia_retido_calado.py:43`. |
| `_nunca_bateu` | `lista`. `app/escala/services/cadastro_realidade.py:829`. |
| `lista`, contendo C5 | Tela e PDF; command C7; command C8; `ListaDoAdmin` do vigia do fio. `app/escala/views.py:487`, `app/escala/views.py:489`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`, `app/chamados/services/vigia_fio.py:216`. |
| Censo de retidos | Command imprime números e aplica o alarme. `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/management/commands/vigiar_retido_calado.py:35`. |

O inventário estático de acessos de produção a `CelulaDia.veredito`, incluindo leituras pelo próprio escritor e projeções do valor, é o seguinte; exaustividade de acessos dinâmicos: **[nao verificado]**.

```text
app/chamados/juizes.py:816,1132,1148,1181,1183
app/chamados/models.py:1161,1165,1166,1710,2790,2791,2792
app/chamados/reconciliador.py:627,629,631,997,1016
app/chamados/management/commands/auditar_invariantes_chamados.py:198,377,1435,1444,1594,1601,1602,1604,1711,1799
app/chamados/services/adesao.py:34
app/chamados/services/disputa_emissao.py:705
app/chamados/services/fabrica_da_celula.py:99
app/chamados/services/furo_sem_canal.py:72,73,97
app/chamados/services/limbo_folgas.py:194
app/chamados/services/resposta_tardia.py:45,56
app/chamados/services/silencio_ausencia.py:40
app/chamados/services/vigia_fio.py:379
app/colaboradores/management/commands/corrigir_escala_retroativa.py:123
app/colaboradores/services/calendario.py:413,414
app/colaboradores/services/pos_vinculo.py:61
app/escala/regua_defesa.py:688
app/escala/services/cadastro_realidade.py:173,445,581
app/escala/services/leitor_celula.py:407
app/escala/services/propositor.py:474
app/folha/export.py:1222,1233,1254,1255
app/folha/fora_da_folha.py:227
app/folha/previa.py:94
app/folha/services/vigia_pausa.py:157
app/pautas/services.py:422
app/ponto/marcos.py:155
app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58,62
app/ponto/management/commands/celula_veredito_velho.py:88,106,125
app/ponto/management/commands/recusar_ponta_pequena.py:145
app/ponto/management/commands/supra_juiz.py:166
app/ponto/portas/celula.py:397,701,741,766,783,797
app/ponto/services/cartorio.py:889,891,1199,1358
app/ponto/services/diagnostico_escala.py:177
app/ponto/services/furos_sem_cobranca.py:61
app/ponto/services/furos_vetados.py:61
app/ponto/services/pautas_dos_discordantes.py:71
app/ponto/services/veredito_celula.py:109,127
app/ponto/services/vigia_ausencia_veredito.py:42,56
app/relatorios/dia_da_frota.py:51,54,55,60,74
```

O inventário de acessos de produção a `Colaborador.situacao`, incluindo consultas por relações, metadados e projeções, é o seguinte; certificação absoluta de “todos”, incluindo acesso dinâmico, fixtures e migrações: **[nao verificado]**.

```text
app/api/login_helpers.py:141
app/api/views.py:116,355,415,420,421
app/api/views_core.py:173,344,402,407,408
app/api/views_mensageria.py:449,1058,1221,1236,1273
app/chamados/models.py:966
app/chamados/reconciliador.py:810
app/chamados/management/commands/auditar_invariantes_chamados.py:360,653,857,1473,1593,1655,1765,1777
app/chamados/management/commands/disparar_perguntas_competencia.py:169
app/chamados/services/adesao.py:33,66
app/chamados/services/desvio_escala.py:107
app/chamados/services/disputa_emissao.py:1241
app/chamados/services/fabrica_da_celula.py:91
app/chamados/services/pergunta_no_app.py:127
app/colaboradores/admin.py:49,50,71
app/colaboradores/models.py:280,

[truncado em 20000 de 30776 caracteres]

## 06/11 17:32 · sessao 1be5a3e989aa · OTIMIZA-3 · verificador · origem=Code

**REPROVA: o exemplo original foi corrigido, mas a classe “loader de string produz falso zero” continua aberta.** Há chamadas com literais completos que escapam da detecção, e o próprio selo exige zero para uma função chamada por loader. (`bin/quem_chama.py:515–520`, `bin/tests/test_quem_chama.sh:263–265`, `bin/tests/test_quem_chama.sh:508`)

A análise abaixo usa a cópia e reproduções em memória, sem criar arquivos. A cópia não contém `.git`: identidade do commit e execução integral do selo histórico **[não verificado]**. [nao verificado]

1. **BLOQUEIA — P1(a,c): loader obtido por `getattr` deixa falso zero, mesmo com módulo literal completo.**

   Contraexemplo sintético: alvo `app/pkg/mod.py`, contendo `def f(): return 1`; chamador:

   ```python
   import importlib
   m = getattr(importlib, 'import_module')('pkg.mod')
   m.f()
   ```

   A reprodução imprime `simbolo f [simbolo-de-topo]: 0 chamador(es)`. `_loaders_py` aceita somente função representada por `ast.Name` ou `ast.Attribute`; nessa chamada, a função é outro `ast.Call`. A string `'pkg.mod'` ainda conta como citação do arquivo, mas não rebaixa o símbolo. (`bin/quem_chama.py:515–520`, `bin/quem_chama.py:590–601`)

   O comportamento das formas pedidas é:

   | Forma | Resultado para o símbolo |
   |---|---|
   | `import_module('pkg.mod')`, `__import__('pkg.mod')`, `run_module('pkg.mod')` | Não varrido, com sítio. (`bin/quem_chama.py:99`, `bin/quem_chama.py:590–593`) |
   | `__import__('pkg', fromlist=['mod'])` | Não varrido, com sítio; combina constantes de pai e folha. (`bin/quem_chama.py:526–530`, `bin/quem_chama.py:550–552`) |
   | `import_module('.mod', package='pkg')` | Não varrido no AST, quando nome relativo e pacote são constantes. (`bin/quem_chama.py:531–536`) |
   | `from importlib import import_module as im`; `import importlib as il` | Cobertos: apelido do from-import ou último atributo da chamada. (`bin/quem_chama.py:506–520`) |
   | Tupla/lista/dict local, variável ou parâmetro com `'pkg.mod'` no mesmo arquivo | Cobertos lexicalmente; não há acompanhamento do valor até a chamada. (`bin/quem_chama.py:585–593`) |
   | Nome vindo de parâmetro, `getattr`, ambiente ou settings, sem literal completo no arquivo do loader | Pode permanecer zero; não há resolução entre arquivos nem desses valores. (`bin/quem_chama.py:523–525`, `bin/quem_chama.py:590–593`) |
   | `leaf=['mod']; __import__('pkg', fromlist=leaf)` | Pode dar zero nas duas facetas: `fromlist` variável não é combinado. (`bin/quem_chama.py:527–530`) |

2. **BLOQUEIA — P1(b), P2: execução por caminho e parâmetro entre arquivos continuam descobertas.**

   `spec_from_file_location`, `SourceFileLoader`, `SourcelessFileLoader`, `imp.load_source` e `run_path` são reconhecidos pelo nome. O alvo somente é associado quando alguma string do mesmo arquivo contém seu caminho ou basename. Portanto variável local, constante, `os.path.join`, `Path` e f-string funcionam **quando preservam esse literal**, como `'mod.py'`; não existe resolução dessas construções. (`bin/quem_chama.py:97–100`, `bin/quem_chama.py:433`, `bin/quem_chama.py:458–464`, `bin/quem_chama.py:586–589`)

   Contraexemplo sintético entre arquivos: `load.py` define `load(path)` usando `spec_from_file_location`; `caller.py` executa `load('app/pkg/mod.py').f()`. A reprodução mantém o arquivo com citações e o símbolo `f` com zero: nenhum arquivo reúne o loader reconhecido e o literal do alvo. (`bin/quem_chama.py:547`, `bin/quem_chama.py:585–593`)

   Também deixam o símbolo com zero, nos contraexemplos isolados, `exec(open('app/pkg/mod.py').read())`, `exec(compile(...))` e `subprocess.run(['python3', 'app/pkg/mod.py'])`: `exec`, `compile` e execução de Python por subprocesso não pertencem aos conjuntos de loaders. O caminho ainda pode contar na faceta arquivo. (`bin/quem_chama.py:97–99`, `bin/quem_chama.py:600–601`)

   Caminho montado como `'mod' + '.py'`, sem basename completo, pode deixar **arquivo e símbolo** com zero; o aviso FORA não altera os tipos das facetas. (`bin/quem_chama.py:90–92`, `bin/quem_chama.py:462`, `bin/quem_chama.py:911–915`)

3. **BLOQUEIA — P1(d,e): texto embutido tem cobertura diferente do AST e ainda produz zero.**

   Contraexemplo sintético reproduzido em `.sh`:

   ```sh
   python3 -c "import importlib;m=importlib.import_module('.mod',package='pkg');m.f()"
   ```

   Para `app/pkg/mod.py`, arquivo e símbolo imprimem zero. O reconhecimento textual procura nome completo entre aspas, `__import__` com `fromlist=` ou ponto de entrada; não resolve o relativo com `package=` que o AST resolve. (`bin/quem_chama.py:437–441`, `bin/quem_chama.py:467–483`, `bin/quem_chama.py:531–536`)

   Heredoc, `python3 -c`, string de código em `.py` e config são cobertos quando o texto contém um loader reconhecido e caminho/basename ou nome completo reconhecível. `exec(open(...))` embutido continua descoberto pelo mesmo motivo do achado 2. (`bin/quem_chama.py:472–483`, `bin/quem_chama.py:594–596`, `bin/quem_chama.py:685–689`)

   `pkg.mod:attr` simples é reconhecido em strings de `.py`, `.sh` e configurações aceitas, incluindo `.yml/.toml/.cfg/Dockerfile`. Em `.sh/config`, o nome pontilhado também pode causar EMBUTIDO, que prevalece sobre LOADER. Arquivo chamado apenas `gunicorn`, sem extensão reconhecida ou shebang, é ignorado; `gunicorn.conf.py` passa pelo AST. (`bin/quem_chama.py:119–120`, `bin/quem_chama.py:441`, `bin/quem_chama.py:777–792`, `bin/quem_chama.py:876–893`)

4. **BLOQUEIA — P5: `r9-montado-fora` certifica um falso zero concreto.**

   A fixture contém `X = "ld_mont"` e chama efetivamente `importlib.import_module("m.%s" % X).f_ld_mont()`, mas a asserção exige `simbolo f_ld_mont [simbolo-de-topo]: 0 chamador(es)`. Não é um contraexemplo vácuo: existe a ligação e existe a chamada. (`bin/tests/test_quem_chama.sh:263–265`, `bin/tests/test_quem_chama.sh:508`)

   A exclusão “nome montado fica FORA” está somente no texto; o símbolo continua em VARRIDOS e o resumo contabiliza seu zero como afirmação. Isso contraria o aval literal fornecido e a distinção operacional entre contado e não varrido. (`bin/quem_chama.py:10–13`, `bin/quem_chama.py:90–92`, `bin/quem_chama.py:590–593`, `bin/quem_chama.py:911–915`)

5. **FILA — P1/P6: casos reais corrigidos e casos reais ainda não associados ao loader.**

   O caso original está corrigido na cópia: `gerar_estado.py` conserva `'espelho_verdade.py'`, carrega o módulo e lê `mod.placar`; esse basename agora produz LOADER. O BLOQUEIO anterior foi resolvido nesse exemplo, mas não como classe, pelos achados anteriores. (`bin/gerar_estado.py:140–145`, `bin/quem_chama.py:586–589`)

   Permanecem estas formas reais sem reconhecimento específico do loader:

   | Caso real | Efeito demonstrável |
   |---|---|
   | `intake`: strings `modulo.simbolo`, separadas antes de `import_module` + `getattr` | Conta arquivo e símbolo pela regra antiga de citação; não produz LOADER. (`app/core/intake.py:34–35`, `app/core/intake.py:485–494`, `bin/quem_chama.py:606–608`) |
   | `test_marca_cron15`: nomes em tabela externa, seguidos de dois `getattr` | O arquivo do loader não contém os módulos completos; esse sítio não é associado. (`app/core/tests/test_marca_cron15.py:62–65`, `app/core/tests/marca_cron15.py:29–35`) |
   | `test_selo_estrutural`: dado externo, `hasattr(import_module(mod), cls)` | Mesmo problema; o alvo atual está na tabela externa. (`app/core/tests/test_selo_estrutural.py:23–31`, `app/core/estrutural.py:47`) |
   | Settings montado por perfil, depois `getattr(mod, 'MIDDLEWARE')` | Sem associação pelo loader. (`app/core/tests/test_trilha_com_contexto.py:103–108`, `app/core/tests/test_selo_log_nao_e_cego.py:120–125`, `app/core/tests/test_selo_erro_com_mensagem.py:80–85`) |
   | `exec(open(...).read(), ns)` e leitura de `ns['logconfig_dict']` | Execução real por caminho, sem detector correspondente. (`app/core/tests/test_access_log_persistido.py:73–78`, `bin/quem_chama.py:97–99`) |

   Esses exemplos não provam zeros globais atuais: testes e settings já recebem tipos fora de VARRIDOS, e citações existentes podem fornecer achados. A quantidade de falsos zeros atuais nesses alvos é **[não verificado]**. (`bin/quem_chama.py:348–349`, `bin/quem_chama.py:366–373`, `bin/quem_chama.py:606–608`)

   `include('x.urls')`, settings e `apps.get_model` não ganham reconhecimento de loader; `urlpatterns`, modelos e settings têm exclusões próprias, enquanto strings `app.mod.Classe` podem contar o símbolo diretamente. `call_command('nome')` conta o arquivo do command pelo token; `Command` é convencional, fora da contagem. (`app/config/urls.py:26`, `app/config/settings/base.py:53–68`, `bin/sombra_mascara_pessoal.py:19`, `bin/quem_chama.py:81–82`, `bin/quem_chama.py:352–355`, `bin/quem_chama.py:370–373`, `bin/quem_chama.py:817–819`, `bin/quem_chama.py:606–608`)

6. **FILA — P2: o caminho reconhecido protege a faceta arquivo, inclusive no próprio alvo.**

   Quando há marcador LOADER, o sítio é recolhido antes da exclusão de autocitação e depois acrescentado à faceta arquivo. Assim, loader reconhecido como único chamador, inclusive dentro do próprio alvo, não deixa arquivo com zero; os símbolos contáveis são rebaixados. (`bin/quem_chama.py:867–871`, `bin/quem_chama.py:885–894`)

   `app/X.py` corresponde a módulo `X`, sem `app.`; `app/pkg/__init__.py` corresponde a `pkg`. Ambos funcionam com esses nomes. Já `'app.pkg.mod'` não corresponde à chave `pkg.mod` usada pelo detector de nome e pode deixar o símbolo com zero. (`bin/quem_chama.py:171–180`, `bin/quem_chama.py:434–436`, `bin/quem_chama.py:590–593`)

   Basename coincidente associa todos os alvos correspondentes, inclusive pacotes com `__init__.py`; isso produz falsos positivos, não desambiguação. Os loaders não reconhecidos dos achados 1–3 continuam podendo deixar uma ou ambas as facetas com zero. (`bin/quem_chama.py:433`, `bin/quem_chama.py:461–464`)

7. **FILA — P3: falsos positivos podem esconder zero verdadeiro; há dois alvos falsamente associados comprovados, sem contagem comprovada de zeros ocultos.**

   Exemplos reais:

   - `bin/crons_duracao.py` cita `'crons.py'` e carrega `app/config/crons.py`; a regra por basename associa também `app/core/management/commands/crons.py`. Outro sítio equivalente copia `config.crons` para um temporário `crons.py`. **Um alvo falso, por dois sítios comprovados.** (`bin/crons_duracao.py:31–40`, `app/chamados/tests/test_cron_longo_sai_da_medida.py:268–279`, `bin/quem_chama.py:462`)
   - O logger `'core.erros'` é tratado como módulo carregado porque o arquivo contém `__import__`; esse loader percorre uma tupla que não contém `core.erros`. **Segundo alvo falso comprovado.** (`app/core/erros.py:30`, `app/core/erros.py:43–52`, `bin/quem_chama.py:590–593`)

   Um alvo sintético `app/a/common.py::morto`, sem leitores, seria rebaixado por um loader de `app/b/common.py`. Portanto o mecanismo pode retirar do relatório um zero verdadeiro utilizável pela L-111. Esses falsos positivos são expressamente admitidos pelo instrumento; não demonstram, sozinhos, violação do aval. (`bin/quem_chama.py:91–92`, `bin/quem_chama.py:462`, `bin/quem_chama.py:892–893`, `app/docs/LEIS.md:137`)

   **Nenhum zero verdadeiro oculto foi comprovado nesses dois alvos reais; total do repositório [não verificado].** Nos exemplos, há usos dos helpers e do logger. (`app/core/erros.py:71–73`, `app/colaboradores/views.py:437`, `app/core/management/commands/crons.py:57`, `app/core/management/commands/crons.py:99`)

   A regex não casa genericamente `http://` ou `C:\`: exige o módulo-alvo exato e `\w` após os dois-pontos. `10:30` exigiria módulo-alvo chamado `10`; existência desse alvo **[não verificado]**. `x.y:z` em string/config pode rebaixar `x.y`; `.md` e `.txt` comum são excluídos como chamadores. (`bin/quem_chama.py:441`, `bin/quem_chama.py:481–483`, `bin/quem_chama.py:780–792`)

8. **FILA — P4: os leitores de tipos e do resumo estão coerentes; a classificação incompleta é que alimenta zeros indevidos.**

   | Estado | Escritor | Leitor |
   |---|---|---|
   | `info['tipo']` | Classificador `:837`; EMBUTIDO `:883`; LOADER `:893` | Guardas `:882/:892`; render `:936` |
   | `a['tipo']` | Construção `:839`; EMBUTIDO `:881–883` | Render `:934` |
   | `a['embutido']` / `a['loader']` | `:880` / `:889` | Render `:933–936` |
   | Dicts `embutido` / `loader` | Inicialização `:847`; acumulação `:865/:868` | Consultas `:877/:886` |
   | `LOADER` / `EMBUTIDO` | Definições `:95/:94` | Emissão `:552/:588/:592/:595/:688` e `:598/:686`; recolha `:867/:864`; render `:907/:903` |
   | `LOADERS_CAMINHO` / `LOADERS_NOME` | `:97–99` | Regex `:100–101`; aliases `:509`; chamadas `:518/:520` |
   | `VARRIDOS` | `:56–73` | Guardas `:882/:892`; contagem `:911`; cabeçalho `:925` |
   | `FORA` | `:83–92` | Somente cabeçalho `:926` |
   | Parâmetros `emb` / `ldr` | Render `:933–936` | `_faceta`: `:902–909` |

   Todas as linhas da tabela pertencem a `bin/quem_chama.py`.

   EMBUTIDO e LOADER incrementam “não varridas”; somente o ramo VARRIDOS incrementa varridas e zeros. RESUMO e `main` usam esses acumuladores, sem semântica antiga observada. (`bin/quem_chama.py:903–915`, `bin/quem_chama.py:939–941`, `bin/quem_chama.py:968–969`)

   Não encontrei outro consumidor automatizado da saída além do selo; completude diante de referências construídas dinamicamente **[não verificado]**. O selo extrai facetas, e o uso humano pelo opus está documentado. (`bin/tests/test_quem_chama.sh:315–330`, `app/docs/RELATO.md:292`)

9. **FILA — P5: há asserções mascaradas, mas os grupos positivos representados mordem; três formas declaradas não têm fixture.**

   As asserções genéricas de não varrido/ausência de zero para `ld_cfg` e `ld_shn` sobrevivem à remoção do loader porque EMBUTIDO já as protege. Entretanto, as asserções de sítio específico falham: o grupo não passa inteiro por acaso. (`bin/tests/test_quem_chama.sh:499–506`, `bin/quem_chama.py:876–893`)

   Nas mutações em memória, remover os detectores representados altera os grupos correspondentes: spec → `dir/var/cte/par/sh`; SourceFileLoader → `sfl`; run_path → `rp/pyc`; import_module → `im/ima/rel/shn`; `__import__` → `imp`. Relativo, fromlist e apelidos também têm verificações específicas. (`bin/tests/test_quem_chama.sh:166–259`, `bin/tests/test_quem_chama.sh:485–505`)

   Já retirar `SourcelessFileLoader`, `load_source` ou `run_module` dos conjuntos e regex não altera as fixtures r9: faltam casos dessas formas. Execução do selo completo contra esses mutantes **[não verificado]**. (`bin/quem_chama.py:97–101`, `bin/tests/test_quem_chama.sh:164–259`)

   `r9-prosa-nao-carrega` não é vácuo: contém loaders completos em docstring e comentário shell; remover os respectivos filtros faz suas asserções falharem. O real histórico pode ser silenciosamente pulado pelo `if git cat-file`; sua execução nesta cópia é **[não verificado]**. (`bin/tests/test_quem_chama.sh:267–273`, `bin/tests/test_quem_chama.sh:509–510`, `bin/tests/test_quem_chama.sh:561–566`, `bin/quem_chama.py:585`, `bin/quem_chama.py:856`)

## 06/11 17:30 · sessao 80fbe5c760f5 · - · - · origem=Ronald (tmux)

Achados e limites, somente pelo código:

1. **Média — o vigia pode baixar `calados` de outras competências/dias.** `censo` descarta as datas de C5 e guarda apenas IDs (`app/folha/vigia_retido_calado.py:43`); depois marca como roteada toda linha bloqueada com fio `ninguem` desse ID (`app/folha/vigia_retido_calado.py:46`–`49`). Prova: dois retratos lavrados no mesmo dia, um da competência 10 com um dia `nunca_bateu` autorizado, outro da 09 ou 11 com outra retenção sem aquele dia; o colaborador entra em C5 pela primeira, e ambas as linhas entram em `roteados`. A seleção dos retratos inclui todos da data da última lavra (`app/folha/vigia_retido_calado.py:38`–`41`). Isso já existia para ativos, mas os novos desligados ampliam seu alcance. Existência em produção: [nao verificado].

2. **Média — a promessa “nenhum campo que o juiz lê é lido aqui” é falsa.** C5 consulta `situacao='desligado'` (`app/escala/services/cadastro_realidade.py:177`–`178`), campo também lido pelo universo da autoridade (`app/ponto/services/fechamento.py:430`–`432`). O comentário promete independência do estado atual (`app/escala/services/cadastro_realidade.py:150`–`152`). Contraexemplo: reativação altera `situacao` para ativo e limpa demissão (`app/colaboradores/services/desligamento.py:203`–`214`); os mesmos dias deixam de passar pela lavra e entram integralmente, porque o filtro de datas só se aplica a IDs do conjunto `segura` (`app/escala/services/cadastro_realidade.py:187`–`190`). Isso preserva a regra antiga dos ativos; refuta a alegação de que a C5 inteira responde exclusivamente pela lavra. Ocorrência em produção: [nao verificado].

3. **Baixa — relógio ainda pode ser lido no interior da função.** O command agora passa `hoje` (`app/folha/management/commands/vigiar_retido_calado.py:30`–`31`), mas `censo(empresas, hoje=None)` continua permitindo omissão (`app/folha/vigia_retido_calado.py:27`) e encaminha `None` (`app/folha/vigia_retido_calado.py:43`), acionando `timezone.localdate()` em C5 (`app/escala/services/cadastro_realidade.py:167`). O caminho do command está corrigido; não é um contrato obrigatório na assinatura.

4. **Byte igual ao base: [nao verificado].** A cópia não tem repositório Git disponível. Pela expressão fornecida no diff, ativos continuam passando pelos mesmos filtros de empresa, `nunca_bateu`, intervalo e ordenação, sem filtro de lavra (`app/escala/services/cadastro_realidade.py:169`–`190`). Isso sustenta equivalência da regra em estado estável, não uma comparação integral com `24946096`.

5. **Emissão do cartório:** C5 só consulta e monta dicionários (`app/escala/services/cadastro_realidade.py:173`–`190`); o vigia só consulta e conta (`app/folha/vigia_retido_calado.py:33`–`57`); o command imprime e eventualmente retorna erro (`app/folha/management/commands/vigiar_retido_calado.py:31`–`40`). Nenhum desses caminhos chama emissor. O portão de furo continua excluindo desligado (`app/ponto/services/cartorio.py:1696`–`1697`), assim como a parcial (`app/ponto/services/cartorio.py:1762`). O ramo nunca-bateu lavra pela porta (`app/ponto/services/cartorio.py:1241`–`1247`) e retorna antes do fluxo normal conforme estrutura/comentário (`app/ponto/services/cartorio.py:1248`–`1254`). Comparação integral do cartório com o base: [nao verificado].

Mapa dos escritores:

- `CelulaDia.veredito` é campo do modelo (`app/escala/models.py:1770`); método do modelo delega à porta (`app/escala/models.py:1819`–`1828`); gravação do valor ocorre em `app/ponto/portas/celula.py:814`, persistida em `:818`–`819`. Chamadores de produção encontrados: cartório `app/ponto/services/cartorio.py:889`, `:1242`, `:1802`; decisão administrativa `app/ponto/services/veredito_celula.py:330`. Há escrita em cópia apenas em memória na própria porta (`app/ponto/portas/celula.py:711`–`726`).
- `Colaborador.situacao` tem default ativo (`app/colaboradores/models.py:323`); porta declara a operação (`app/colaboradores/services/colaborador.py:32`), atribui/salva (`:55`–`61`), salva objeto completo (`:73`–`76`), atualiza lote (`:80`–`88`) e cria (`:91`–`93`). Decisores encontrados: desligamento `app/colaboradores/services/desligamento.py:104`–`108`; reativação `:210`–`214`; afastamento `app/ponto/views.py:2433`–`2435`; reversão `app/ponto/management/commands/reverter_situacao_afastado.py:67`–`73`; admissão ativa `app/colaboradores/views.py:1205`, `:2610`. C5 não adiciona escrita desses campos (`app/escala/services/cadastro_realidade.py:173`–`190`).

Mapa completo das chamadas encontradas do retorno C5:

- `colabs_nunca_bateu` → formatador `_nunca_bateu` (`app/escala/services/cadastro_realidade.py:197`) e vigia (`app/folha/vigia_retido_calado.py:43`).
- `_nunca_bateu` → `lista` (`app/escala/services/cadastro_realidade.py:829`).
- `lista` → tela (`app/escala/views.py:487`), PDF (`:489`), render (`:492`); command C7 (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, filtro C7 em `:32`); command C8 (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`, filtro C8 em `:40`); vigia do fio (`app/chamados/services/vigia_fio.py:216`).
- O vigia do fio guarda presença do colaborador (`app/chamados/services/vigia_fio.py:218`, `:248`–`251`), mas sua visão por dia ignora C5: exige chave `data` (`app/escala/services/cadastro_realidade.py:1002`), enquanto C5 só fornece código e detalhe (`:198`–`199`).
- Contadores da tela que podem subir: `por_assinatura.C5` (`app/escala/services/cadastro_realidade.py:880`–`882`, `:907`–`911`), `colabs_do_cadastro` e `colabs_listados` (`:914`–`920`). Os commands C7/C8 filtram suas próprias assinaturas e não passam a fechar por C5 (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:32`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:40`).

Inventário dos leitores candidatos de produção de `CelulaDia.veredito`, após busca de atributos, keywords ORM e nomes literais; homônimos sem relação foram retirados quando identificados:

- `app/chamados/juizes.py:816,1132,1148,1181,1183`
- `app/chamados/management/commands/auditar_invariantes_chamados.py:198,377,1435,1444,1594,1601,1602,1604,1711,1799`
- `app/chamados/models.py:1161,1165,1166,1710,2790,2791,2792`
- `app/chamados/reconciliador.py:627,629,631,997,1016`
- `app/chamados/services/adesao.py:34`; `disputa_emissao.py:705`; `fabrica_da_celula.py:99`; `furo_sem_canal.py:72,73,97`; `limbo_folgas.py:194`; `resposta_tardia.py:45,56`; `silencio_ausencia.py:40`; `vigia_fio.py:379`
- `app/colaboradores/management/commands/corrigir_escala_retroativa.py:123`
- `app/colaboradores/services/calendario.py:413,414`; `pos_vinculo.py:61`
- `app/escala/services/cadastro_realidade.py:173,445,581`; `leitor_celula.py:407`; `propositor.py:474`
- `app/folha/export.py:1222,1233,1254,1255`; `fora_da_folha.py:227`; `previa.py:94`; `services/vigia_pausa.py:157`
- `app/pautas/services.py:422`
- `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58,62`; `celula_veredito_velho.py:88,106,125`; `recusar_ponta_pequena.py:145`
- `app/ponto/portas/celula.py:397,701,741,766,783,797`
- `app/ponto/services/cartorio.py:889,891,1199,1358`; `diagnostico_escala.py:177`; `furos_sem_cobranca.py:61`; `furos_vetados.py:61`; `pautas_dos_discordantes.py:71`; `veredito_celula.py:109,127`; `vigia_ausencia_veredito.py:42,56`
- `app/relatorios/dia_da_frota.py:51,54,55,60`

Inventário de candidatos de produção de `Colaborador.situacao` — os pontos incluem leitura, metadados e escrita; resolução exaustiva de acesso dinâmico: [nao verificado]:

- `app/api/login_helpers.py:141`
- `app/api/views.py:116,355,415,420,421`; `views_core.py:173,344,402,407,408`; `views_mensageria.py:449,1058,1221,1236,1273`
- `app/chamados/management/commands/auditar_invariantes_chamados.py:360,653,857,1473,1593,1655,1765,1777`; `disparar_perguntas_competencia.py:169`
- `app/chamados/models.py:966`; `reconciliador.py:810`
- `app/chamados/services/adesao.py:33,66`; `desvio_escala.py:107`; `disputa_emissao.py:1241`; `fabrica_da_celula.py:91`; `pergunta_no_app.py:127`
- `app/colaboradores/admin.py:49,50,71`; `models.py:280`; `queries.py:26`
- `app/colaboradores/management/commands/diagnostico_login.py:36`; `efetivar_desligamentos_agendados.py:40,45`; `fechar_vinculo_de_desligado.py:35`; `tripwire_vinculo_orfao.py:46,47,48`
- `app/colaboradores/services/cadastro.py:105`; `colaborador.py:32`; `desligamento.py:17,53,104,199,200,210`; `ficha.py:234`; `senha_provisoria.py:17`; `situacao_exibida.py:56`; `vinculo.py:89`
- `app/colaboradores/views.py:33,62,66,206,602,619,884,1026,1043,1045,1205,1414,1448,1540,1689,1748,1770,1831,2610,2846,2856,2896,2912`
- `app/comunicados/services.py:57`; `views.py:125`
- `app/core/busca.py:89,104`; `canal.py:36`; `reexibir.py:18`; `regua_cct.py:292`; `services/painel_op.py:68`; `views.py:216,352,797,1203`; `views_config.py:526`
- `app/escala/folgas.py:75`; `models.py:929`; `proposta_pdf.py:35`; `regua_defesa.py:260`; `services/cadastro_realidade.py:178,246,445,566,570`; `services/furos_diarios.py:259`; `services/previsto_cego.py:143`; `views.py:242`
- `app/ferias/management/commands/alertar_retornos.py:30`; `pautas_de_ferias.py:66,77`; `app/ferias/situacao.py:172`; `views.py:586,684,692`
- `app/folha/export.py:802`
- `app/holerite/matriz.py:88`; `motor.py:252,275`; `views.py:1129,1150`
- `app/inteligencia/calculadores.py:38,139,324`; `management/commands/gerar_alertas.py:113`; `resumo.py:61`; `views.py:63,76,77,83,84,85,86,87,158,209,220,226,232,260,293,479,512,529,530`
- `app/ponto/management/commands/checklist_golive.py:30`; `detectar_ausencias.py:186`; `detectar_cluster_espurio.py:37`; `detectar_par_relampago.py:337`; `emitir_furo_retroativo.py:52`; `marcar_abandono_provisorio.py:36`; `projecoes_vencidas.py:67`; `propor_reorganizacao.py:35`; `reconciliar_grade.py:106`; `reverter_situacao_afastado.py:67,73,80`; `supra_juiz.py:110`; `tripwire_celulas.py:18,58,89`; `vigiar_motor_autoridade.py:46`
- `app/ponto/services/afastado_avisa.py:41,126,167,171`; `cartorio.py:1697,1762`; `cluster_espurio.py:117`; `dia_do_colab.py:78`; `fechamento.py:431,432`; `furos_sem_cobranca.py:62`; `furos_vetados.py:62`; `geofence.py:46`; `he_pendente_lavrado.py:92`; `pautas_dos_discordantes.py:70`; `triagem_batida.py:114,119`; `vigia_de_hora.py:117`; `vinculo_na_lista.py:70,82,151`
- `app/ponto/utils.py:47,170,172`; `views.py:376,1119,1120,1121,2225,2373,2435`
- `app/relatorios/dia_da_frota.py:34,64`; `furos.py:61,117`; `management/commands/diff_extrato_parcial.py:86`; `services.py:105,343`; `views.py:455,833,848,857,1337,1345,1445,1447,1500,1545`

Testes e migrações não estão nesse inventário de produção; a árvore contém muitos escritores de fixture, inclusive os explícitos do diff (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:42`–`44`, `:147`, `:176`, `:195`, `:206`, `:214`).

## 06/11 17:29 · sessao 92a3e9def14f · - · - · origem=Ronald (tmux)

1. **Bloqueia — loader com nome montado continua sem sítio reconhecido.** Três leitores reais montam `config.settings.<perfil>` e leem `MIDDLEWARE` por `getattr`: [app/core/tests/test_trilha_com_contexto.py:103](app/core/tests/test_trilha_com_contexto.py:103), [app/core/tests/test_selo_log_nao_e_cego.py:120](app/core/tests/test_selo_log_nao_e_cego.py:120), [app/core/tests/test_selo_erro_com_mensagem.py:80](app/core/tests/test_selo_erro_com_mensagem.py:80). A detecção exige string inteira igual ao módulo, sem composição: [bin/quem_chama.py:590](bin/quem_chama.py:590). Sonda em memória de `varrer_py` sobre o primeiro leitor, alvo `app/config/settings/base.py::MIDDLEWARE`, retornou `[]`; o `0` global depende dos outros leitores da árvore e permanece **[não verificado]**. A constante real está em [app/config/settings/base.py:53](app/config/settings/base.py:53).

2. **Bloqueia — dado em outro módulo não rebaixa o alvo carregado.** [app/core/tests/test_marca_cron15.py:62](app/core/tests/test_marca_cron15.py:62) lê `M.MARCADOS`, separa módulo/classe/método e faz `import_module(mod)` seguido de dois `getattr`; as strings estão em [app/core/tests/marca_cron15.py:29](app/core/tests/marca_cron15.py:29). Contraexemplo concreto: `chamados.tests.test_contract_crons.ContratoCronsTest` nas entradas de [app/core/tests/marca_cron15.py:30](app/core/tests/marca_cron15.py:30), classe existente em [app/chamados/tests/test_contract_crons.py:19](app/chamados/tests/test_contract_crons.py:19). A sonda de `varrer_py` desse leitor contra esse alvo retornou `[]`. O algoritmo examina somente as strings do arquivo chamador: [bin/quem_chama.py:585](bin/quem_chama.py:585).

3. **Fila — string `modulo.simbolo` usada para loader é contada como citação antiga, sem rebaixamento.** O dado está em [app/core/intake.py:34](app/core/intake.py:34), e `escritor_de` separa o caminho com `rpartition('.')` antes de importar e ler por `getattr`: [app/core/intake.py:485](app/core/intake.py:485). Contraexemplo: `ferias.services.agendar_ferias_porta`. A sonda encontrou citação de arquivo e símbolo em `:34`, nenhum marcador `LOADER`; a citação nasce de [bin/quem_chama.py:606](bin/quem_chama.py:606), enquanto o rebaixamento exige igualdade com o nome do módulo em [bin/quem_chama.py:591](bin/quem_chama.py:591). Portanto esse caso tem chamador contado, mas não recebe a classificação pedida pelo aval literal.

4. **Fila — leitores de tabelas estruturais também ficam invisíveis no sítio real do loader.** [app/core/tests/test_selo_estrutural.py:27](app/core/tests/test_selo_estrutural.py:27) deriva `mod` de dado externo e faz `hasattr(import_module(mod), cls)`; o alvo atual vem de [app/core/estrutural.py:47](app/core/estrutural.py:47), classe existente em [app/core/tests/test_contract_reciclagem.py:43](app/core/tests/test_contract_reciclagem.py:43). Outro leitor equivalente está em [app/core/tests/test_selo_contratos_estruturais.py:75](app/core/tests/test_selo_contratos_estruturais.py:75), com dados em [app/core/contratos_estruturais.py:82](app/core/contratos_estruturais.py:82). A string `mod.Classe` da tabela ainda pode contar o símbolo pelo mecanismo antigo de [bin/quem_chama.py:606](bin/quem_chama.py:606); não prova reconhecimento do loader.

5. **Sem defeito demonstrado nas formas reais antigas de caminho/nome literal.** `gerar_estado` cita `espelho_verdade.py` em [bin/gerar_estado.py:140](bin/gerar_estado.py:140), carrega em `:142` e chama `mod.placar` em `:145`; o reconhecimento por basename cobre esse caminho por [bin/quem_chama.py:458](bin/quem_chama.py:458). O mesmo mecanismo alcança `medida_leitor_do_dia.py` em [bin/leitor_dia_estado.py:25](bin/leitor_dia_estado.py:25), `crons.py` em [bin/crons_duracao.py:31](bin/crons_duracao.py:31) e `esteira_vigia.py` em [bin/disjuntor.py:184](bin/disjuntor.py:184). `__import__('colaboradores.queries', fromlist=[...]).empresas_visiveis()` está em [app/comunicados/views.py:122](app/comunicados/views.py:122), coberto pelo literal inteiro e loader de nome de [bin/quem_chama.py:590](bin/quem_chama.py:590).

6. **Tupla/lista/dict local com módulo inteiro é coberta, mesmo por parâmetro local; framework sem loader nomeado não é rebaixado.** A tupla de módulos e exceções de [app/core/erros.py:43](app/core/erros.py:43), usada por `__import__(caminho)` e `getattr` em `:51-52`, recebe marcador `LOADER` para `pautas.services` em `:48` pela sonda. Já `include('x.urls')` real em [app/config/urls.py:26](app/config/urls.py:26) não está nos nomes reconhecidos de [bin/quem_chama.py:97](bin/quem_chama.py:97); `urlpatterns` é convencional e já fica fora da contagem por [bin/quem_chama.py:81](bin/quem_chama.py:81). Settings de classe, como [app/config/settings/base.py:60](app/config/settings/base.py:60), continuam sujeitos à citação pontilhada antiga de [bin/quem_chama.py:606](bin/quem_chama.py:606), sem reconhecimento específico do framework.

## 06/11 17:29 · sessao ac52abb7ece6 · - · - · origem=Ronald (tmux)

P3 — fila: o escopo lexical do arquivo produz falsos positivos reais. `bin/crons_duracao.py:31` cita `crons.py`; o loader de `bin/crons_duracao.py:37` carrega `app/config/crons.py`, mas a busca pelo basename também associa `app/core/management/commands/crons.py`, por `bin/quem_chama.py:433` e `bin/quem_chama.py:462`. O mesmo falso alvo vem de `app/chamados/tests/test_cron_longo_sai_da_medida.py:271`/`:277` (esse teste copia `config.crons`, conforme `:268`/`:272`). Portanto há **um alvo falso comprovado por dois sítios** nesse caso; total do repositório [não verificado].

P3 — fila: outro falso positivo comprovado é `app/core/erros.py:30`: a string do logger `'core.erros'` vira associação de loader porque há um `__import__` em `app/core/erros.py:51`; entretanto os valores realmente carregados vêm da tupla de `app/core/erros.py:43`–`:48`, que não contém `core.erros`. A causa é a comparação de todas as strings do arquivo em `bin/quem_chama.py:590`–`:593`.

Assim, identifiquei **dois alvos falsamente associados**, mas **nenhum zero verdadeiro oculto comprovado**: `_negocio` é usado em `app/core/erros.py:71`, o logger em `:73`, e `so_recusa_de_negocio` é importado em `app/colaboradores/views.py:437`; os helpers do comando `crons` são usados em `app/core/management/commands/crons.py:57` e `:99`. Total de zeros verdadeiros ocultos [não verificado]. O mecanismo pode esconder um zero verdadeiro: alvo `app/a/common.py` contendo somente `def morto(): return 1`, junto de um chamador que executa `sf('x', 'app/b/common.py')`; o basename rebaixa `morto` por `bin/quem_chama.py:462`, `:586`–`:589` e `:892`–`:893`. Isso perde evidência utilizável pela L-111 (`app/docs/LEIS.md:137`), mas é o falso positivo expressamente admitido em `bin/quem_chama.py:91`–`:92`.

As suspeitas de regex devem ser delimitadas: o padrão é específico ao **nome do alvo**, `bin/quem_chama.py:441`; `http://` e `C:\` não têm `\w` depois de `:`, portanto não casam. Uma hora `10:30` só casaria com alvo cujo módulo fosse literalmente `10`; arquivo correspondente não encontrado [não verificado]. `x.y:z` em string de código/config pode rebaixar `x.y`, independentemente de execução (`bin/quem_chama.py:481`–`:483`); `.md` é excluído em `bin/quem_chama.py:780`, `.txt` comum cai em `None` em `:792`, enquanto `.txt` sob templates segue o ramo template `:782`–`:783` (que não chama `loader_texto`, `:704`–`:744`).

P4 — mapa completo:

| Dado | Escritor | Leitor |
|---|---|---|
| `info['tipo']` | classificador `bin/quem_chama.py:837`; EMBUTIDO `:883`; LOADER `:893` | guardas `:882`, `:892`; `render` `:936` |
| `a['tipo']` | construção `:839`; loop EMBUTIDO inclui `(None,a)` `:881`–`:883` | `render` `:934` |
| `a['embutido']` | `:880` | `:933`, passado `:934`/`:936` |
| `a['loader']` | `:889` | `:933`, passado `:934`/`:936` |
| dict `embutido` | inicializa `:847`, acumula `:865` | consulta `:877` |
| dict `loader` | inicializa `:847`, acumula `:868` | consulta `:886` |
| `LOADERS_CAMINHO` | definição `:97`–`:98` | regex `:100`; aliases `:509`; calls `:518` |
| `LOADERS_NOME` | definição `:99` | regex `:101`; aliases `:509`; calls `:520` |
| `LOADER` | definição `:95` | emissão `:552`, `:588`, `:592`, `:595`, `:688`; recolha `:867`; tipo `:893`; saída `:907` |
| `EMBUTIDO` | definição `:94` | emissão `:598`, `:686`; recolha `:864`; tipo `:883`; saída `:903` |
| `VARRIDOS` | definição `:56`–`:73` | guardas `:882`/`:892`; contagem `:911`; cabeçalho `:925` |
| `FORA` | definição `:83`–`:92` | cabeçalho `:926` |
| `emb`/`ldr` | `render` lê metadados `:933` | passa `:934`/`:936`; `_faceta` usa `:905`/`:909` |

Não encontrei leitor mantendo contagem antiga: os ramos EMBUTIDO/LOADER incrementam `conta['nao']` em `bin/quem_chama.py:906`/`:910`; somente o ramo `tipo in VARRIDOS` incrementa varridas/zeros em `:914`–`:915`; RESUMO usa esses mesmos acumuladores em `:939`–`:941`; `main` também usa `conta['nao']` em `:968`–`:969`.

Busca `rg --hidden` dos identificadores exatos `quem_chama.py`, `QUEM_CHAMA.txt` e `QUEM_CHAMA_PY` não encontrou outro consumidor automatizado fora do próprio instrumento e `bin/tests/test_quem_chama.sh:315`–`:330`; completude frente a referências construídas dinamicamente [não verificado]. O consumidor humano opus consta expressamente em `app/docs/RELATO.md:292`; a afirmação histórica `quem-mais-le=0` está em `app/docs/RELATO.md:287`.

## 06/11 17:29 · sessao 5d1aca6012ca · - · - · origem=Ronald (tmux)

1. **Fila — algumas asserções isoladas passam por EMBUTIDO, mas o grupo acusa a perda do loader.** `ld_cfg` e `ld_shn` já recebem EMBUTIDO pelo nome pontilhado solto, antes do processamento de LOADER; a segunda passagem preserva tipos já fora de VARRIDOS (`bin/quem_chama.py:114`, `bin/quem_chama.py:876`, `bin/quem_chama.py:892`). Retirar LOADER de `varrer_linhas()` mantém verdes `r9-loader-ld_cfg`, `r9-loader-zero-ld_cfg`, `r9-loader-ld_shn`, `r9-loader-zero-ld_shn`, mas quebra os sítios específicos em `bin/tests/test_quem_chama.sh:501` e `bin/tests/test_quem_chama.sh:505`; as asserções genéricas ficam em `bin/tests/test_quem_chama.sh:500`, `bin/tests/test_quem_chama.sh:502`, `bin/tests/test_quem_chama.sh:504`, `bin/tests/test_quem_chama.sh:506`.

2. **Fila — três nomes declarados não têm fixture r9 e os mutantes sobrevivem nesse recorte.** Retirar `SourcelessFileLoader`, `load_source` ou `run_module` simultaneamente dos conjuntos e regex não altera qualquer resultado r9; os nomes estão declarados em `bin/quem_chama.py:97`–`bin/quem_chama.py:101`, mas as fixtures exercitam somente spec/SourceFileLoader/run_path/import_module/__import__/entrypoint/embutido (`bin/tests/test_quem_chama.sh:164`–`bin/tests/test_quem_chama.sh:259`). O selo completo e os testes históricos não foram executados: [não verificado].

3. **Bloqueia, pela exigência literal do usuário — `r9-montado-fora` certifica um falso zero real.** A fixture efetivamente monta `m.ld_mont` e chama `f_ld_mont()` por `import_module("m.%s" % X)` (`bin/tests/test_quem_chama.sh:263`–`bin/tests/test_quem_chama.sh:265`), mas exige símbolo contado com zero (`bin/tests/test_quem_chama.sh:508`). A detecção só aceita uma constante que corresponda inteira ao módulo (`bin/quem_chama.py:590`–`bin/quem_chama.py:593`); o rótulo textual FORA não modifica o tipo dessa faceta (`bin/quem_chama.py:90`–`bin/quem_chama.py:92`, `bin/quem_chama.py:911`–`bin/quem_chama.py:915`). Na reprodução em memória, arquivo e símbolo de `ld_mont` imprimem zero; não é contra-exemplo vácuo.

4. **Fila — `r9-prosa-nao-carrega` não é vácuo.** Há loader completo numa docstring e num comentário shell (`bin/tests/test_quem_chama.sh:267`–`bin/tests/test_quem_chama.sh:273`). Remover o filtro de prosa de `varrer_py()` ou trocar o shell limpo pelo texto cru faz `ld_doc` receber LOADER e quebra as asserções em `bin/tests/test_quem_chama.sh:509`–`bin/tests/test_quem_chama.sh:510`; os filtros reais ficam em `bin/quem_chama.py:386`–`bin/quem_chama.py:390`, `bin/quem_chama.py:585`, `bin/quem_chama.py:649`–`bin/quem_chama.py:658`, `bin/quem_chama.py:856`.

5. **Verificação do recorte positivo:** removendo cada detector representado, o grupo correspondente muda e suas asserções específicas mordem: spec → `ld_dir/var/cte/par/sh`; SourceFileLoader → `ld_sfl`; run_path → `ld_rp/pyc`; import_module → `ld_im/ima/rel/shn`; __import__ → `ld_imp`; relativo, fromlist e apelidos também são exercitados especificamente (`bin/tests/test_quem_chama.sh:166`–`bin/tests/test_quem_chama.sh:259`, `bin/tests/test_quem_chama.sh:485`–`bin/tests/test_quem_chama.sh:505`). Os resultados foram obtidos executando `quem_chama()` e `render()` originais em memória, com os heredocs extraídos do selo e as leituras git substituídas por mapas; execução integral do shell, que cria arquivos, permanece [não verificado].

## 06/11 17:29 · sessao 336c741a6680 · - · - · origem=Ronald (tmux)

Auditoria dos testes, sem executar Django/DB nem gravar arquivos. [nao verificado]

**Médio — falta um caso positivo da competência corrente.** O mutante `for _ini, fim in janelas[:1]:` em `app/escala/services/cadastro_realidade.py:181` deixa de consultar o retrato corrente e sobrevive, por inspeção, a todos os testes novos/alterados: os desligados positivos só têm dias aceitos na anterior; na corrente o caso A sai pela rescisão (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:32`, `:120`), e o teste corte 10 também só espera dia da anterior (`:223`). Prova a acrescentar: desligado com demissão depois de 20/11, dia `nunca_bateu` em 23/10, FM e lavra corrente; exigir o dia na C5 (`app/escala/services/cadastro_realidade.py:172`, `:181`).

**Médio — selo deixa passar leitura de campos por kwargs ORM e chamadas de porta descartadas.** O detector só procura atributos e argumentos posicionais constantes, sem examinar palavras-chave ORM (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:294`, `:298`). Confirmei em execução AST pura que injetar `Colaborador.objects.filter(pk__in=segura, data_demissao__isnull=True).exists()` passa o selo com `campos=set()`; a restrição pretendida está em `:260` e `:307`. Também confirmei que uma chamada descartada `linhas_do_retrato(None, 'bloqueado')`, seguida de leitura paralela de `retrato.linhas`, passa: presença da chamada basta (`:291`, `:304`); a proteção contra código morto reconhece somente `if` com constante falsa (`:289`). Helpers externos à função também não são inspecionados (`:273`, `:280`).

**Médio — “nenhum campo que o juiz lê” não corresponde ao selo.** A C5 consulta `situacao='desligado'` (`app/escala/services/cadastro_realidade.py:177`), assim como a autoridade de universo (`app/ponto/services/fechamento.py:431`); `situacao` está ausente de `CAMPOS_DA_AUTORIDADE` (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:260`). Isso não prova que o filtro original de situação deva desaparecer; prova que a afirmação/selo amplo não cobre o campo já utilizado (`app/escala/services/cadastro_realidade.py:151`).

A testemunha foi melhorada: `_segura()` chama diretamente a autoridade, sem recompor rescisão e retentor (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:79`). É independente do leitor C5, que consulta o retrato (`app/escala/services/cadastro_realidade.py:183`), e compartilha a fonte do lavrador por desenho (`app/folha/retrato_folha.py:1156`; `app/folha/fora_da_folha.py:161`). O teste da divergência entre estado vivo e lavra tem mordida real contra recomposição simples (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`).

Mapa completo: em cada linha abaixo, o mutante **M1 = ignorar competência corrente**, descrito acima, sobrevive por inspeção. Execução integral dos testes: **[nao verificado]**. [nao verificado]

| Teste novo/alterado — arquivo:linha | O que verifica / limite | [nao verificado]
|---|---|
| `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:97` | Fixture positivo real; não chama C5. |
| mesmo arquivo `:109` | Presença e porta C5; não exige os dias exatos. |
| mesmo arquivo `:120` | Dias exatos da anterior; sem positivo corrente. |
| mesmo arquivo `:124` | Exclusão de rescisão anterior. |
| mesmo arquivo `:129` | Igualdade com autoridade em dois casos restritos. |
| mesmo arquivo `:138` | Sem data de demissão fica fora. |
| mesmo arquivo `:148` | Sem FM fica fora. |
| mesmo arquivo `:154` | Sem retrato fica fora mesmo com autoridade viva positiva. |
| mesmo arquivo `:160` | Snapshot e autoridade viva divergem; impede recomposição simples. |
| mesmo arquivo `:174` | Demissão na borda da anterior. |
| mesmo arquivo `:183` | Retenção sem código, sem datas. |
| mesmo arquivo `:194` | Retrato legado sem datas. |
| mesmo arquivo `:201` | Exclui entra e rescisão; não testa outro motivo bloqueado com datas. |
| mesmo arquivo `:209` | Honra `hoje` passado; não testa positivo corrente. |
| mesmo arquivo `:223` | Corte 10; só positivo anterior. |
| mesmo arquivo `:243` | Ativo sem FM/retrato; ignora caminho desligado. |
| mesmo arquivo `:246` | Ativo com FM/retrato; ignora caminho desligado. |
| mesmo arquivo `:251` | Ativo com campos do retentor; ignora caminho desligado. |
| mesmo arquivo `:302` | Selo sobre produção; M1 passa, confirmado por AST em memória. |
| mesmo arquivo `:309` | Exercita exemplo sintético, sem ler produção. |
| mesmo arquivo `:320` | Exercita exemplo sintético, sem ler produção. |
| mesmo arquivo `:329` | Exercita `if False` sintético, sem ler produção. |
| `app/folha/tests/test_vigia_retido_calado.py:98` | Desligado anterior roteado; só lavra explícita da 10. |
| mesmo arquivo `:128` | Data injetada chega à lista, para ativo. |
| mesmo arquivo `:135` | Command passa data ao censo; não conta leituras do relógio. |

Os testes comportamentais novos ESEM estão congelados por classe (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:90`, `:217`, `:230`); os três testes novos/alterados do vigia também (`app/folha/tests/test_vigia_retido_calado.py:69`). Os testes AST não usam o relógio (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:267`). Portanto, não encontrei dependência nova de relógio solto nesses testes; continuam sem cobrir a borda de meia-noite local e a garantia de leitura única no command (`app/folha/tests/test_vigia_retido_calado.py:140`, `:143`).

## 06/11 17:29 · sessao 447469798dd3 · - · - · origem=Ronald (tmux)

Achados e rastreio, somente por código:

1. **P1 — desligado sem data volta a entrar quando já existe FechamentoMensal.** O teste retira `data_demissao` antes de criar fechamentos, logo não cobre histórico existente (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:138-145`). O universo exclui desligado sem data (`app/ponto/services/fechamento.py:423-432`), porém `_K2` consulta todos os FM existentes, sem consultar esse universo (`app/folha/export.py:775-780`); `sai_pela_rescisao` retorna falso para data nula (`app/folha/export.py:54-60`), e o retentor não limita o fim sem data (`app/folha/export.py:1198-1201`). Havendo célula acusadora, a lavra fresca pode produzir datas que C5 aceita (`app/folha/export.py:1233`; `app/escala/services/cadastro_realidade.py:184-190`). **Prova:** inverter o preparo do teste: `self._fechar()`; depois `update(data_demissao=None)`; depois `self._lavrar()`. `_fechar` usa apenas `get_or_create`, preservando FM (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:60-66`). Isso repete a ampliação explicitamente reprovada, sem exigir retrato velho. Execução desse cenário: [nao verificado].

2. **P2 — “datas de bloqueado” não equivale às datas de `dias_que_seguram`.** C5 aceita datas de qualquer bloqueado (`app/escala/services/cadastro_realidade.py:184-188`), enquanto a testemunha registrada só povoa `dias` para `motivo == 'furo_espelho'` (`app/folha/fora_da_folha.py:128-137`). O escritor real também grava datas de `ferias_com_batida` (`app/folha/export.py:868-876`; `app/folha/retrato_folha.py:1172-1183`). **Prova:** desligado com demissão após fim, FM, férias com batida apurável em dia que ainda tenha célula `nunca_bateu`; a lavra real grava esse dia de férias e C5 aceita, mas `dias_que_seguram()['dias']` não o contém. A coexistência desse estado em produção: [nao verificado]. A semântica dupla é documentada no modelo (`app/folha/models.py:323-334`).

3. **P2 — universo de empresas não acompanha o escritor.** C5 lê todas empresas ou os pks recebidos (`app/escala/services/cadastro_realidade.py:169`), mas o lavrador filtra `empresas_visiveis` inclusive com `--empresa` (`app/folha/management/commands/lavrar_retrato_folha.py:49-51`), excluindo inativas e CNPJ `11222333*` (`app/colaboradores/queries.py:78-87`). Sem retrato anterior, desligado dessas empresas desaparece mesmo quando a autoridade viva o retém; com retrato anterior, C5 pode continuar lendo uma lavra que o lavrador já não atualiza (`app/escala/services/cadastro_realidade.py:183-188`). Ocorrência real: [nao verificado].

4. **P2 — selo omite campo de estado efetivamente compartilhado.** C5 consulta `situacao` (`app/escala/services/cadastro_realidade.py:177-178`); `_K2` também consulta `situacao` para travas (`app/folha/export.py:797-804`), e o universo usa o campo (`app/ponto/services/fechamento.py:430-432`). `CAMPOS_DA_AUTORIDADE` não inclui `situacao` (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:273`). A afirmação “nenhum campo que o juiz lê” é falsa pelo próprio código (`app/escala/services/cadastro_realidade.py:151-152`).

Limites solicitados:

- **Competência:** consulta e lavrador usam exatamente mês/ano do fim de `janela_anterior` e `janela_atual` por empresa; não achei divergência estática desse idioma, inclusive corte e virada (`app/escala/services/cadastro_realidade.py:172-183`; `app/folha/management/commands/lavrar_retrato_folha.py:58-65`; `app/ponto/janelas.py:24-33,60-72`).
- **`colab` repetido:** C5 faz união das datas de todas as linhas (`app/escala/services/cadastro_realidade.py:185-186`); o lookup oficial existente responde pela primeira (`app/folha/retrato_folha.py:931-937`). JSON não impõe unicidade interna (`app/folha/models.py:348-352`), mas FM tem unicidade colab/mês/ano (`app/ponto/models.py:485`), e o lavrador emite uma linha por item (`app/folha/retrato_folha.py:1162-1189`). Duplicatas reais: [nao verificado].
- **Data não ISO:** falha em casar com `d.isoformat()` (`app/escala/services/cadastro_realidade.py:188`); escritor real produz `str(date)` (`app/folha/fora_da_folha.py:192`; `app/folha/retrato_folha.py:1175`). Formatos diferentes na produção: [nao verificado].
- **Readmissão:** serviço real volta a ativo e limpa demissão (`app/colaboradores/services/desligamento.py:203-214`). C5 então ignora a lavra e retorna todos os dias candidatos (`app/escala/services/cadastro_realidade.py:177-190`). Pode divergir do retrato antigo; isso também é consequência da preservação do comportamento ativo, não regressão isolada (`app/escala/services/cadastro_realidade.py:153-154`).

Mapa de RetratoFolha:

- `_K2` registrado em `app/core/juizes.py:299` e `app/docs/JUIZES.md:74`; escreve valores do item em `app/folha/export.py:829-875,960-972`.
- Retentor produz dias em `app/folha/export.py:1218-1222`; `dias_que_seguram` seleciona universo de furo (`app/folha/fora_da_folha.py:118-139`); `fora_da_folha` transforma em datas ISO (`app/folha/fora_da_folha.py:184-193`).
- Escritor único persistente: `montar_linhas` monta chaves (`app/folha/retrato_folha.py:1156-1189`); command grava `linhas` por `update_or_create` (`app/folha/management/commands/lavrar_retrato_folha.py:74-76`).
- Leitor direto da lista: `linhas_do_retrato` (`app/folha/retrato_folha.py:108-111`); balde lê `status/motivo` (`app/folha/retrato_folha.py:89-95`).
- Leitores externos de linhas: tela/pipeline do retrato (`app/folha/views_retrato.py:190-197,226,272-286`), drawer do colaborador (`app/colaboradores/views.py:975-985`), vigia (`app/folha/vigia_retido_calado.py:33-54`), nova C5 (`app/escala/services/cadastro_realidade.py:183-188`).
- Leitores internos: contadores (`app/folha/retrato_folha.py:120-121`), fio (`:428-441`), dono/datas (`:474-480`), tipo (`:484-491`), saldo (`:494-501`), recorte (`:617-645`), lookup por colab (`:935-937`), portas (`:1010-1015`), ações/motivo (`:1058-1065`), recorte de datas (`:1233-1245`). Frescor lê metadados do retrato, não julga `linhas` (`app/folha/retrato_folha.py:1295-1306`).

## 06/11 17:23 · sessao 89a375b18f4a · O10-C · censo · origem=Code

O O10-C atual **não deposita SOLO numa lista persistida do admin**: classifica e imprime a linha; a lista `!` também é saída de terminal. A aplicação fica restrita a `trocaria`, obtida com par declarado. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:90`, `:233`, `:237`, `:247`, `:254`)

**1) As listas e superfícies administrativas encontradas são estas:**

| Lista / entidade | Quem escreve a linha | Quem lê | Link ou ação existente |
|---|---|---|---|
| **Ausências — `Ausencia`**, com colaborador e data/período. (`app/ponto/models.py:743`, `:750`) | `ponto.services.ausencia.criar_ausencia`; o lançamento administrativo chama essa função. (`app/ponto/services/ausencia.py:130`, `:385`; `app/ponto/views.py:2415`) | `lista_ausencias` → `ponto/ausencias.html`, com filtros por colaborador, tipo e status. (`app/ponto/views.py:2155`, `:2183`, `:2202`, `:2244`) | Linha abre painel; há documento, aprovação/cancelamento e upload individual. Esses controles não recebem colega nem duas cartas. (`app/templates/ponto/ausencias.html:113`, `:142`, `:167`; `app/templates/ponto/partials/painel_ausencia.html:90`, `:134`) |
| **Central de Justificativas — `Justificativa`**, por colaborador/data do turno. (`app/ponto/models.py:136`, `:144`) | Há vários escritores: `_abrir_justificativa_fluxo`, API, `regularizar_turno_orfao` e `_abrir_justificativa_automatica`. (`app/ponto/views.py:1621`, `:1635`; `app/api/views.py:1997`, `:2007`; `app/ponto/services/regularizacao.py:91`; `app/ponto/management/commands/processar_alertas_turno.py:389`) | `fila_justificativas` → `ponto/fila_justificativas.html`, por padrão pendentes. (`app/ponto/views.py:1695`, `:1704`, `:1709`, `:1749`) | Abre painel da justificativa; este oferece POSTs de aprovação e rejeição. (`app/templates/ponto/fila_justificativas.html:148`; `app/templates/ponto/partials/painel_justificativa.html:51`, `:58`) |
| **Central de Chamados — `ChamadoColaborador`**, com colaborador e FK opcional de célula. (`app/chamados/models.py:32`, `:44`) | Portas de nascimento `ChamadoColaborador.abrir` — cria ou pega — e `criar`, sem deduplicação. (`app/chamados/models.py:796`, `:867`, `:1015`) | `painel_gestao` → `montar_contexto_painel` → listas resumida/detalhada. (`app/chamados/views.py:721`, `:726`, `:740`, `:767`) | Linha abre o fio; existem ações conforme o verbo, como validar e cobrar. (`app/templates/chamados/partials/_lista_chamados.html:42`, `:82`) |
| **Pendência administrativa da pergunta — `PerguntaDisputa.pendencia_admin`**, junto de `recusa_motivo`; não é uma tabela genérica de casos. (`app/chamados/models.py:2317`, `:2321`) | `_materializar_resposta_hora` grava os dois campos juntos. (`app/chamados/services/materializacao.py:759`, `:821`, `:825`) | `modal_fio_colaborador` → `chamados/partials/modal_fio.html`. (`app/chamados/views.py:869`, `:927`) | O bloco mostra aviso e motivo; a orientação de correção é textual, sem link próprio nesse bloco. (`app/templates/chamados/partials/modal_fio.html:217`; `app/chamados/services/explicador.py:85`) |
| **Pautas — `Pauta`**, destinadas a departamento/usuário; aceitam âncora de dia `<colab>:<AAAA-MM-DD>`. (`app/pautas/models.py:74`, `:83`) | Escritor canônico `pautas.services.escrever`, que cria pauta, trilha e contador. (`app/pautas/services.py:153`, `:180`, `:186`) | `pautas.views.aba` → painel `_copiloto.html`. (`app/pautas/views.py:29`, `:33`, `:61`; `app/templates/core/_copiloto.html:404`) | Há responder/feita/cancelar. A navegação por âncora atende `colab`, `chamado`, `posto`; **`dia` e `competencia` caem no aviso “ainda não tem tela própria”**. (`app/templates/core/_copiloto.html:432`, `:443`, `:452`, `:564`, `:570`) |
| **Fila humana do flip**, por batida/colaborador/dia. (`app/ponto/services/flip_auto.py:248`) | Não tem depósito próprio nesses caminhos: `avaliar_fila` deriva os itens e deduplica por ID da batida. (`app/ponto/services/flip_auto.py:266`, `:268`) | `flip_fila_view` seleciona `veredito='humano'` → `ponto/flip_fila.html`. (`app/ponto/views.py:3299`, `:3305`) | Link para espelho e POST `flip_batida`. (`app/templates/ponto/flip_fila.html:11`, `:16`) |
| **Pendências de cadastro / Furos de espelho** — agregadas por colaborador/período. (`app/relatorios/views_pend.py:21`, `:24`; `app/templates/relatorios/pendencias_espelho.html:42`) | São derivadas de `ranking_colabs` + `universo_empresa`; essas funções de montagem não depositam um caso próprio. (`app/relatorios/views_pend.py:23`; `app/relatorios/pendencias.py:120`, `:135`) | `pendencias_cadastro_view` / `pendencias_espelho_view` → templates homônimos. (`app/relatorios/views_pend.py:74`, `:84`, `:87`, `:95`) | Linhas exibem nome, contato, natureza, descrição, recomendação e dias; não têm ação/link na linha. (`app/templates/relatorios/pendencias_cadastro.html:42`; `app/templates/relatorios/pendencias_espelho.html:42`) |
| **Gestão de HE**, com pendências por colaborador/dia. (`app/ponto/views.py:884`) | `he_pendente_lavrado.lavrar` grava o retrato em `MetricaSnapshot`; decisões vigentes são lidas de `DecisaoHE`. (`app/ponto/services/he_pendente_lavrado.py:177`, `:182`; `app/ponto/services/gestao_he.py:81`) | `gestao_he` lê o retrato e renderiza `ponto/gestao_he.html`. (`app/ponto/views.py:931`, `:942`, `:951`) | A célula do dia posta para `decidir_he`; a view chama a porta de HE. (`app/templates/ponto/gestao_he.html:256`; `app/ponto/views.py:517`, `:529`) |

Sobre **“PortaFila”**: o correspondente encontrado é o valor `'porta_fila'` retornado por `porta_alcancavel_do_dia`, a partir dos chamados da célula na fila; essa função lê alcance, não cria casos. O próprio contrato exclui “Resolver dia” desse conceito específico de porta. (`app/chamados/juizes.py:1341`, `:1367`, `:1375`, `:1386`)

**2) Não localizei chamador de UI de `trocar_plantao` ou `editar_dia`.** Os chamadores produtivos encontrados são O10-C → `trocar_plantao` e, dentro desta, → `editar_dia`; o cabeçalho do comando declara ser o primeiro chamador de produção da troca. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:4`, `:160`; `app/ponto/portas/celula.py:675`)

A tela mais próxima é **perfil do colaborador → Calendário de Ponto → dia**. O perfil carrega o calendário; a grade inclui “Resolver dia” já com colaborador e data. O drawer de diagnóstico também inclui essa grade. (`app/templates/colaboradores/detalhe.html:215`; `app/colaboradores/views.py:1935`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`)

A cadeia existente é:

`_veredito_dia.html` → URL `celula/veredito/` → `veredito_celula_view` → serviço `veredito_celula`, com `trabalhou`, `faltou`, `justificado`. “Justificado” chama `criar_ausencia`. (`app/templates/ponto/partials/_veredito_dia.html:17`; `app/ponto/urls.py:58`; `app/ponto/views.py:3340`; `app/ponto/services/veredito_celula.py:18`, `:310`)

O que falta **na cadeia atual** para alcançar a troca dali: o formulário não coleta substituto, duas Ausências/cartas ou motivo; o handler não despacha para `trocar_plantao`. Para `editar_dia`, também não há despacho com `trabalha` e motivo. Esses argumentos constam das respectivas portas. (`app/templates/ponto/partials/_veredito_dia.html:19`; `app/ponto/views.py:3318`, `:3340`; `app/ponto/portas/celula.py:534`, `:561`, `:632`, `:646`)

Além disso, o botão não aparece automaticamente para todo SOLO: exige permissão, último recurso, dia passado e status `falta`, `irregular`, `aberto` ou `sem_certidao`. O botão “DP”, que abre pauta com colaborador/data, está sob a mesma condição da grade. (`app/colaboradores/services/calendario.py:94`, `:103`; `app/templates/colaboradores/partials/_calendario_grade.html:125`, `:126`)

**3) Os “dois anexos obrigatórios” são duas cartas no mesmo campo de modelo:**

- Carta do cedente: `ausencia_cedente.documento`.
- Carta do substituto: `ausencia_substituto.documento`.

A assinatura recebe duas `Ausencia`; o campo é `FileField`, com `upload_to="ausencias/documentos/"`, permitindo vazio no modelo. A obrigatoriedade dupla é imposta pela porta. (`app/ponto/portas/celula.py:632`, `:635`, `:664`; `app/ponto/models.py:777`)

A porta recarrega e trava os registros, recusa lado ausente/inexistente ou mesmo colaborador e testa literalmente `if not a.documento` em cada lado. Essa validação não abre o arquivo nem segue `documento_referencia`; a trilha guarda `documento.name`. (`app/ponto/portas/celula.py:653`, `:656`, `:660`, `:662`, `:664`, `:682`)

**4) Sim, existem precedentes de lote que deposita caso para o admin, sem resolver o fato por ele:**

| Precedente | Depósito e escritor | Idempotência / chave |
|---|---|---|
| **`processar_alertas_turno`** | `_abrir_justificativa_automatica` cria `Justificativa(status='pendente', tipo='esqueci_saida')`; a Central de Justificativas lê essas linhas. (`app/ponto/management/commands/processar_alertas_turno.py:389`, `:398`; `app/ponto/views.py:1704`) | `get_or_create(colaborador, data_turno)`. O banco impõe uma pendente por `(colaborador,data_turno)`, além da unicidade geral com `batida_entrada`. (`app/ponto/management/commands/processar_alertas_turno.py:391`; `app/ponto/models.py:187`, `:189`) |
| **`pautas_discordantes`** | Comando chama `sincronizar`; com aplicação, escreve pautas ao DP por `pautas.services.escrever` e fecha as que saíram do alvo. (`app/ponto/management/commands/pautas_discordantes.py:113`, `:118`; `app/ponto/services/pautas_dos_discordantes.py:173`) | Cabeça `[DISCORDANTE] <DONO> col<ID> <AAAA-MM>`, entre pautas vivas do DP, âncora competência. É agrupado por dono/colaborador/competência, contendo os dias. (`app/ponto/services/pautas_dos_discordantes.py:84`, `:143`, `:165`) |
| **`pauta_dia_em_aberto`** | Escreve pauta ao DP e filhas com detalhamento, pela porta `escrever`. (`app/pautas/management/commands/pauta_dia_em_aberto.py:139`) | Cabeça viva por âncora `empresa:AAAA-MM` e prefixo `[DIA EM ABERTO]`; completar filhas também tem guarda própria. (`app/pautas/management/commands/pauta_dia_em_aberto.py:104`, `:107`, `:117`) |

O escritor canônico de **Pauta** é `escrever`, mas ele faz `create`: a chave de idempotência é conferida por cada emissor, não deduplicada genericamente pela porta. (`app/pautas/services.py:153`, `:180`; `app/ponto/services/pautas_dos_discordantes.py:165`)

Há também precedente **por dia**: `pauta_dp_do_limbo_exportado` escreve ao DP com âncora `<colab>:<data>`. Deduplica pelo histórico inteiro — inclusive feita/cancelada — considerando âncora, destinatário DP, cabeça sem pai, prefixo e tag `[motivo:X]`. (`app/chamados/services/fabrica_da_celula.py:208`, `:219`, `:225`, `:236`)

E existe um serviço diretamente relacionado ao nome ausente: `declarar_cobertura_passada`, sem substituto, abre pauta à supervisão perguntando quem cobriu. A chave considera pauta viva, dia, destinatário supervisão e prefixo `[COBERTURA-A-CONFIRMAR]`; é serviço, não comando de lote. (`app/ponto/services/cobertura.py:238`, `:246`, `:329`, `:333`)

**5) O contraexemplo depende de “SOLO” significar ausência de `--par` ou ausência de colega no cadastro legado:**

- **Sem `--par`: não há caminho de aplicação da troca.** O caso retorna `so_um_lado` — ou outra recusa anterior, como pedido/rejeitada/sem carta — e apenas `trocaria` chama `aplicar_par`. Não passa pela escrita de células/trilhas da porta. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:77`, `:90`, `:237`)
- **Mesmo sem par, `--aplicar` escreve arquivo:** cria diretório e abre um snapshot exclusivo antes do laço. Sem aplicação de par, esse arquivo não recebe as fotos de `aplicar_par`. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:209`, `:216`, `:157`, `:164`)
- **Se SOLO significa o lançamento legado sem colega no cadastro, existe caminho de escrita por CLI:** `--par C:S --aplicar --por USER` fornece a segunda Ausência ao classificador e pode chegar à troca. A porta então escreve `CelulaDia` e trilhas `editar_dia`/`trocar_plantao`; o teste existente monta um lançamento legado SOLO, fornece a segunda carta por `--par` e espera duas células editadas e trilha. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`, `:233`, `:238`, `:160`; `app/ponto/portas/celula.py:600`, `:609`, `:611`, `:686`; `app/ponto/tests/test_o10c_lote_trocas.py:54`, `:140`)
- **O comando não cria o lado do substituto:** consulta uma `Ausencia` já existente pelo ID declarado. Também não deposita o SOLO em Pauta, Justificativa ou chamado; a saída do laço é `stdout`, contadores e lista `!`. (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:188`, `:191`, `:233`, `:247`, `:254`)

Execução dos testes e conteúdo atual do banco: **[nao verificado]**. [nao verificado]

## 06/11 17:22 · sessao 24261d69a599 · - · - · origem=Ronald (tmux)

- **Pautas** aceitam âncora de colaborador, chamado, posto, **dia** (`<colab_id>:<AAAA-MM-DD>`) e competência; o destinatário pode ser departamento ou usuário. Escritor canônico: `pautas.services.escrever`, que cria a linha, registra trilha e lavra contador. (`app/pautas/models.py:30`, `app/pautas/models.py:74`, `app/pautas/models.py:83`, `app/pautas/services.py:153`, `app/pautas/services.py:180`).
- Leitor: `pautas.views.aba` chama `listar_por_aba` e devolve as linhas ao painel do copiloto. As ações existentes são marcar feita, responder e cancelar. (`app/pautas/views.py:29`, `app/pautas/views.py:33`, `app/pautas/views.py:61`, `app/templates/core/_copiloto.html:432`, `app/templates/core/_copiloto.html:443`, `app/templates/core/_copiloto.html:452`).
- **A pauta de dia não tem navegação acionável própria no copiloto**: seu texto pode incorporar URL do calendário, mas o painel o renderiza com `textContent`; as rotas clicáveis cobrem somente `colab`, `chamado` e `posto`, e `dia` cai no aviso “ainda não tem tela própria”. (`app/pautas/services.py:428`, `app/pautas/services.py:429`, `app/templates/core/_copiloto.html:404`, `app/templates/core/_copiloto.html:564`, `app/templates/core/_copiloto.html:570`).
- Existe bloco de pautas na ficha, mas ele mostra somente remetente, destinatário, estado e texto truncado em 180 caracteres. O serviço `pautas_do_colab` consulta âncoras de colaborador e seus dias; não encontrei chamador Python desse serviço além da própria definição — alimentação desse contexto **[não verificado]**. (`app/templates/colaboradores/detalhe.html:108`, `app/templates/colaboradores/detalhe.html:115`, `app/templates/colaboradores/detalhe.html:119`, `app/pautas/services.py:433`).

- **Central de chamados**: a linha persistida é `ChamadoColaborador`, com colaborador e FK opcional de `CelulaDia`; não há garantia de uma linha por dia no modelo. O painel usa `montar_contexto_painel`; a linha abre o fio e oferece ações de validar, cobrar e desarquivar conforme o verbo. (`app/chamados/models.py:32`, `app/chamados/models.py:44`, `app/chamados/models.py:101`, `app/chamados/views.py:721`, `app/chamados/views.py:727`, `app/templates/chamados/partials/_lista_chamados.html:42`, `app/templates/chamados/partials/_lista_chamados.html:82`).
- Escritores canônicos de nascimento: `ChamadoColaborador.abrir` faz cria-ou-pega com deduplicação por `colaborador + **lookup`, dentro de transação e trava na linha do colaborador; `criar` cria sem deduplicação. (`app/chamados/models.py:796`, `app/chamados/models.py:863`, `app/chamados/models.py:867`, `app/chamados/models.py:932`, `app/chamados/models.py:1015`, `app/chamados/models.py:1056`).
- **`PortaFila` não é entidade separada nas fontes encontradas**: `porta_fila` é valor do dicionário devolvido por `porta_alcancavel_do_dia`, quando há chamado da célula em `qs_na_fila`; essa função lê, não deposita casos. O conceito exclui explicitamente o botão Resolver dia do calendário. (`app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1352`, `app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1375`, `app/chamados/juizes.py:1386`).
- `qs_na_fila` seleciona chamados em estados de cobrança, excluindo os silenciados no futuro. (`app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:186`).

- **Pendência do admin na pergunta**: é o campo `PerguntaDisputa.pendencia_admin`, acompanhado de `recusa_motivo`, e não uma tabela genérica de pendências. O escritor explícito é `_materializar_resposta_hora`, que grava ambos juntos. (`app/chamados/models.py:2312`, `app/chamados/models.py:2317`, `app/chamados/models.py:2321`, `app/chamados/services/materializacao.py:759`, `app/chamados/services/materializacao.py:821`, `app/chamados/services/materializacao.py:825`).
- O fio do admin carrega perguntas via `montar_chamados_fio`; `modal_fio_colaborador` renderiza o template que exibe o aviso e o motivo dessa pendência. Nesse bloco não há link de correção: o explicador fornece instrução textual para conferir horário e lançar correção com trilha. (`app/chamados/views.py:869`, `app/chamados/views.py:889`, `app/chamados/views.py:927`, `app/templates/chamados/partials/modal_fio.html:217`, `app/chamados/services/explicador.py:85`).

- **Precedente claro de lote que envia à lista do admin**: `pautas_discordantes` apura casos e chama `sincronizar`; com `--apply`, o serviço escreve pautas ao DP e marca feitas as que saíram do alvo. A chave é a cabeça textual `[DISCORDANTE] <DONO> col<ID> <AAAA-MM>`, agrupada por dono, colaborador e competência; a consulta de existentes considera pauta viva, âncora competência, destinatário DP e prefixo do emissor. (`app/ponto/management/commands/pautas_discordantes.py:113`, `app/ponto/management/commands/pautas_discordantes.py:118`, `app/ponto/services/pautas_dos_discordantes.py:84`, `app/ponto/services/pautas_dos_discordantes.py:93`, `app/ponto/services/pautas_dos_discordantes.py:165`, `app/ponto/services/pautas_dos_discordantes.py:173`).
- Outro comando existente, `pauta_dia_em_aberto`, escreve pautas ao DP por competência em vez de decidir os dias; deduplica uma cabeça viva por âncora `empresa:AAAA-MM` e prefixo `[DIA EM ABERTO]`. (`app/pautas/management/commands/pauta_dia_em_aberto.py:104`, `app/pautas/management/commands/pauta_dia_em_aberto.py:107`, `app/pautas/management/commands/pauta_dia_em_aberto.py:139`).
- Existe também emissão **por dia**: `pauta_dp_do_limbo_exportado` usa `escrever`, destinatário DP e âncora `<colab>:<data>`. Sua idempotência considera todo o histórico, inclusive pautas feitas ou canceladas, pela âncora de dia, destinatário, cabeça sem pai, prefixo `LIMBO EM COMPETENCIA EXPORTADA` e tag `[motivo:X]`. (`app/chamados/services/fabrica_da_celula.py:208`, `app/chamados/services/fabrica_da_celula.py:214`, `app/chamados/services/fabrica_da_celula.py:219`, `app/chamados/services/fabrica_da_celula.py:225`, `app/chamados/services/fabrica_da_celula.py:236`).
- Precedente diretamente relacionado ao nome de quem cobriu: `declarar_cobertura_passada` sem substituto abre pauta à supervisão pedindo o nome. Deduplica por pauta viva, âncora do dia, destinatário supervisão e prefixo `[COBERTURA-A-CONFIRMAR]`; trata-se de serviço, não de comando de lote. (`app/ponto/services/cobertura.py:238`, `app/ponto/services/cobertura.py:241`, `app/ponto/services/cobertura.py:246`, `app/ponto/services/cobertura.py:329`, `app/ponto/services/cobertura.py:333`).

_2368 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
