# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 12:44** · publicado em **31/10/2026 12:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 12:44 · sessao 03ec68bba43d · - · - · origem=Ronald (tmux)

Leitura sem alterações. O arquivo do fechamento encontrado nesta árvore é `app/ponto/services/fechamento.py`; ele declara o recalculo da folha e implementa o laço por fatias (`app/ponto/services/fechamento.py:2`, `app/ponto/services/fechamento.py:132`, `app/ponto/services/fechamento.py:203`).

**A: resultado da regra de feriado do v2**

- `get_motor` escolhe `MotorIntermitente` quando `tipo_ciclo == 'intermitente'` ou `tipo_base == 'intermitente'`; `CICLOS_FERIADO_SIMPLES` contém apenas `12x36`, portanto o flag calculado do intermitente é `feriado_em_dobra=True` (`app/ponto/motor_calculo_v2.py:3563`, `app/ponto/motor_calculo_v2.py:3537`, `app/ponto/motor_calculo_v2.py:3607`).
- `MotorIntermitente` herda de `Motor12x36`, define somente `__init__`, aumenta `jornada_minutos` e apaga o conjunto de feriados recebido; não sobrescreve `calcular_periodo` nem define flag contrário à dobra (`app/ponto/motor_calculo_v2.py:3483`, `app/ponto/motor_calculo_v2.py:3491`, `app/ponto/motor_calculo_v2.py:3494`, `app/ponto/motor_calculo_v2.py:3495`).
- `MotorBase.eh_feriado` primeiro consulta `self.feriados`; ausente a data, chama `is_feriado(data)` sem passar local (`app/ponto/motor_calculo_v2.py:901`, `app/ponto/motor_calculo_v2.py:903`, `app/ponto/motor_calculo_v2.py:905`).
- `is_feriado(data, praca=None)` chama `core.feriados.eh_feriado(data, praca)` e devolve falso se ocorrer exceção (`app/ponto/motor_calculo_v2.py:30`, `app/ponto/motor_calculo_v2.py:40`, `app/ponto/motor_calculo_v2.py:42`).
- O juiz territorial admite nacional para todos, estadual somente com UF, municipal somente com UF e município correspondentes; sem município o fallback só reconhece nacional (`app/core/feriados.py:57`, `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:91`).
- Assim, **nacional cadastrado e consultável: reconhecido**; `_feriado_dobra_no_dia` resulta verdadeiro pelo flag e calendário e o `calcular_periodo` herdado escreve toda a duração líquida efetiva em `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, marcando `hora_extra_100=True` (`app/ponto/motor_calculo_v2.py:879`, `app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:2748`, `app/ponto/motor_calculo_v2.py:2749`, `app/ponto/motor_calculo_v2.py:2750`, `app/ponto/motor_calculo_v2.py:2751`).
- **Municipal isolado: não reconhecido**, pois o intermitente apagou o conjunto e o fallback não tem município; consequentemente a dobra de feriado é zero (`app/ponto/motor_calculo_v2.py:3495`, `app/ponto/motor_calculo_v2.py:905`, `app/core/feriados.py:64`, `app/core/feriados.py:91`, `app/ponto/motor_calculo_v2.py:879`).
- Não confundir “dobra municipal zero” com uma garantia absoluta de “HE100 total zero”: se existir jornada prevista positiva e excedente superior a duas horas, o ramo normal pode produzir HE100 pelo excedente (`app/ponto/motor_calculo_v2.py:2773`, `app/ponto/motor_calculo_v2.py:2778`, `app/ponto/motor_calculo_v2.py:2785`, `app/ponto/motor_calculo_v2.py:2791`, `app/ponto/motor_calculo_v2.py:2792`).
- No caso comum de intermitente sem previsão, `_prev_dia` falso faz o limite virar a própria duração e não há extra; o aumento de `jornada_minutos` do intermitente só é lido quando `_previsto_do_dia` devolve `None` (`app/ponto/motor_calculo_v2.py:2773`, `app/ponto/motor_calculo_v2.py:2778`, `app/ponto/motor_calculo_v2.py:1200`, `app/ponto/motor_calculo_v2.py:1211`, `app/ponto/motor_calculo_v2.py:1214`).
- O número é duração efetiva depois da janela e dos intervalos, não necessariamente a distância bruta entre batidas; o plantão aberto sem saída retorna antecipadamente (`app/ponto/motor_calculo_v2.py:2690`, `app/ponto/motor_calculo_v2.py:2697`, `app/ponto/motor_calculo_v2.py:2699`, `app/ponto/motor_calculo_v2.py:2702`, `app/ponto/motor_calculo_v2.py:2712`, `app/ponto/motor_calculo_v2.py:2723`).
- O dia consultado no `calcular_periodo` é `e.date()`; a política não exige “dia não previsto”, apesar do comentário envelhecido da linha 2745: o corpo efetivo é somente flag e calendário (`app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:879`).

**Ressalva indispensável se a resposta fala do fechamento completo**

Não se pode afirmar que “trabalho registrado em feriado nacional sempre resulta em W horas pagas” com os insumos fornecidos: após `calcular_periodo`, `calcular_mes` chama `_dia_sem_previsto_fora_da_janela` (`app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:2399`).

Essa função:
- passa quando é folga do par **ou** `self.eh_feriado(dia)`; exige empresa com janela ativa e data alcançada por `janela_he_desde`, previsto exatamente zero e ausência de decisão vigente `autorizado` (`app/ponto/motor_calculo_v2.py:1386`, `app/ponto/motor_calculo_v2.py:1387`, `app/ponto/motor_calculo_v2.py:1389`, `app/ponto/motor_calculo_v2.py:1391`, `app/ponto/motor_calculo_v2.py:1394`, `app/ponto/motor_calculo_v2.py:1327`, `app/ponto/motor_calculo_v2.py:1330`).
- Nesse caso transfere os minutos para fora da janela, zera trabalhadas, extras, inclusive `minutos_extra_100_feriado`, e colapsa o segmento efetivo; gera pendência até autorização (`app/ponto/motor_calculo_v2.py:1409`, `app/ponto/motor_calculo_v2.py:1410`, `app/ponto/motor_calculo_v2.py:1411`, `app/ponto/motor_calculo_v2.py:1414`, `app/ponto/motor_calculo_v2.py:1420`, `app/ponto/motor_calculo_v2.py:1423`).

Portanto a resposta precisa distinguir **regra bruta de dobra nacional = W** de **resultado final eventualmente zero por L-097**, que não implementa exclusão por ciclo intermitente (`app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:1387`, `app/ponto/motor_calculo_v2.py:1411`).

**Por que a fatia está no vínculo certo**

O fechamento usa `vinculo_do_dia` para atribuir o dono e percorre cada escala/fatia; nela usa `esc.tipo_escala`, monta seu motor e chama `calcular_mes` com aquele tipo e vínculo (`app/ponto/services/fechamento.py:188`, `app/ponto/services/fechamento.py:191`, `app/ponto/services/fechamento.py:203`, `app/ponto/services/fechamento.py:262`, `app/ponto/services/fechamento.py:263`, `app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:272`, `app/ponto/services/fechamento.py:273`).

O construtor local do motor chama `get_motor_cct(colab, fatia_ini, tipo_escala=esc.tipo_escala, feriados=feriados, ...)`; o conjunto territorial foi montado com `feriados_de(lugar_do_colab(colab), ...)`, mas o intermitente o apaga depois (`app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:622`, `app/ponto/services/fechamento.py:709`, `app/ponto/services/fechamento.py:711`, `app/ponto/services/fechamento.py:713`, `app/ponto/motor_calculo_v2.py:3495`).

O município usado nesse carregamento é o do `colaborador.posto`, não uma consulta direta ao posto do vínculo da fatia (`app/core/feriados.py:106`, `app/core/feriados.py:113`, `app/core/feriados.py:114`, `app/ponto/services/fechamento.py:622`).

**Escritores/leitores v2**

| Campo | Quem escreve/resolve | Quem lê |
|---|---|---|
| `feriados` | fechamento carrega território e injeta: `app/ponto/services/fechamento.py:622`, `:713`; base converte em `set`: `app/ponto/motor_calculo_v2.py:740`; intermitente sobrescreve vazio: `:3495` | `MotorBase.eh_feriado`: `app/ponto/motor_calculo_v2.py:903`; fallback `:905` |
| `feriado_em_dobra` | `get_motor` deriva do ciclo: `app/ponto/motor_calculo_v2.py:3607`; constante só 12x36: `:3537`; base grava booleano: `:737`; CCT pode conceder de volta para 12x36: `app/core/regua_cct.py:477`, `:478` | predicado efetivo: `app/ponto/motor_calculo_v2.py:879`; ramo de alerta: `:2756`; carimbo: `app/core/regua_cct.py:425`; fronteira v3: `app/ponto/calculador/nucleo.py:351` |
| `prorrogacao_noturna_pos5h` | `get_motor_cct` com switch desligado/CLT: `app/core/regua_cct.py:453`, `:454`; ligado: `:466`; base resolve ausência pela lei do ciclo e grava: `app/ponto/motor_calculo_v2.py:708`, `:710`, `:712`; lei retorna ciclo diferente de 12x36: `app/core/regua_cct.py:182` | noturnas: `app/ponto/motor_calculo_v2.py:922`; desconto de intervalo noturno: `:1866`; split HE noturna: `:955`; carimbo: `app/core/regua_cct.py:423`; fronteira v3 `app/ponto/calculador/nucleo.py:348` |
| `hora_reduzida_afastada` | default da fábrica falso: `app/ponto/motor_calculo_v2.py:3540`; CCT injeta só se cláusula, ciclo 12x36 e trava liberada: `app/core/regua_cct.py:470`; base grava: `app/ponto/motor_calculo_v2.py:719` | fator noturno: `app/ponto/motor_calculo_v2.py:930`; carimbo: `app/core/regua_cct.py:424`; fronteira v3: `app/ponto/calculador/nucleo.py:349` |
| lei da família | tabela nomeia `MotorIntermitente` e `Motor12x36` cru: `app/ponto/calculador/regras.py:487`, `:494`; função lê por identidade exata de classe: `:518` | v2 não chama essa função [não verificado como afirmação negativa absoluta]; concretamente intermitente herda `Motor12x36.calcular_periodo` (`app/ponto/motor_calculo_v2.py:3483`), que não chama `_julgar_pontualidade`; filha com escala chama: `:3461`, `:3471`; escritor comum: `:1950` |

**Dado útil sobre a v3 atual**

A árvore já contém a correção V3-RETOMA: `_cct_de(_te_dia)` usa template do dia, enquanto `_feriados` continua vindo diretamente do chamador; esse arranjo torna esperado que ambas variantes do chamador deem a mesma dobra nacional **e municipal**, porque o motor usado somente para flags do intermitente tem `feriado_em_dobra=True`, e o calendário efetivo da v3 não é o conjunto apagado nele (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:507`, `app/ponto/calculador/alimentacao.py:510`, `app/ponto/calculador/alimentacao.py:524`).

## 31/10 12:44 · sessao c04dc1b063c1 · - · - · origem=Ronald (tmux)

Inventário de decisões de produção sobre dobra, com referências atuais da árvore:

| Sítio | Predicado / consequência |
|---|---|
| `app/ponto/motor_calculo_v2.py:3537` e `:3607-3608` | `CICLOS_FERIADO_SIMPLES={'12x36'}`; se não veio override, `feriado_em_dobra = ciclo not in CICLOS_FERIADO_SIMPLES`. Intermitente recebe `True`. |
| `app/core/regua_cct.py:440`, `:449-458`, `:477-479` | Com switch CCT desligado delega diretamente; ligado, somente ciclo literal `'12x36'` + cláusula `feriado_12x36_em_dobra` faz `setdefault('feriado_em_dobra', True)`. |
| `app/ponto/motor_calculo_v2.py:859-879` | `_feriado_dobra_no_dia(dia)` retorna `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; não testa “previsto”. |
| `app/ponto/motor_calculo_v2.py:2747-2754` | `Motor12x36.calcular_periodo`: duração positiva + helper verdadeiro põe toda duração em `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado`. |
| `app/ponto/motor_calculo_v2.py:3030-3039` | Consolidação diária do TurnoPartido: trabalho positivo + helper verdadeiro põe o total diário em HE100 feriado e dá `continue`. |
| `app/ponto/motor_calculo_v2.py:3193-3200` | `MotorComercial.calcular_periodo`: duração positiva + helper verdadeiro põe toda duração em HE100 feriado. |
| `app/ponto/motor_calculo_v2.py:3275-3290` | Consolidação diária Comercial: trabalho positivo + helper verdadeiro põe total diário em HE100 feriado e dá `continue`. |
| `app/ponto/motor_calculo_v2.py:3483-3495` | `MotorIntermitente(Motor12x36)` conserva decisão herdada; `__init__` só amplia jornada e zera `self.feriados`, embora a docstring afirme “sem feriado-100 automático”. |
| `app/ponto/calculador/regras.py:287-295`, `:344-347` | `_ContextoFeriado` empresta exatamente `MotorBase._feriado_dobra_no_dia` e `eh_feriado`; `dobra_de_feriado_do_dia` retorna zero se trabalho não positivo, senão todo trabalho quando helper verdadeiro. |
| `app/ponto/calculador/regras.py:298-306` | `feriado_em_dobra_do_ciclo` responde `tipo_ciclo not in CICLOS_FERIADO_SIMPLES`. Busca de produção encontrou somente definição; a própria documentação do repo o registra órfão em `app/docs/BACKLOG.md:1018`. |
| `app/ponto/calculador/regras.py:775-789` | `do_dia`: havendo calendário, flag e dia, chama dobra; se positiva, HE100 total e feriado = dobra/60, HE50=0 e retorna antes do excedente. |
| `app/ponto/calculador/periodo.py:817-847` | Usa dia da jornada, soma trabalho dos pares, chama dobra com `cct.feriados` e `cct.feriado_em_dobra`; se positiva grava total em HE100 feriado no último período e HE50=0; caso contrário usa excedente. |
| `app/folha/export.py:330-349`, `:352-369`, `:409-432` | Export usa ciclo do vínculo no fim da janela e cláusula CCT no fim da janela; suprime parcela de feriado se `tipo_ciclo=='12x36' and not feriado_12x36_em_dobra`. Outros ciclos preservam parcela apurada. É decisão mensal de emissão, não consulta diária de calendário. |

Há também decisões sobre **folga causada pelo feriado**, que pagam HE100 feriado por outro caminho:

- `app/escala/models.py:32`, `:1357-1363`: `feriado_vira_folga` só aceita ciclos 6x1/5x2/semanal, vínculo que não trabalha em feriado, e calendário válido na praça do colaborador. Intermitente está fora do conjunto.
- `app/ponto/services/fechamento.py:909-931`: primeiro exige `_ft_certa`; identifica vínculo diário e pergunta `feriado_vira_folga`; soma horas desses períodos a HE100 total e feriado.
- `app/ponto/services/dia_pago.py:244-251`: grava a partição `ft_feriado` em HE100 total/feriado.
- `app/ponto/calculador/alimentacao.py:523`: forma `folga_e_feriado` exigindo vínculo diário, presença no calendário recebido e `feriado_vira_folga`.
- `app/ponto/calculador/regras.py:660-683`: somente `not intermitente`, com escala certa e dia, passa à decisão de folga; `_hora_feriado = bool(folga_e_feriado and _certa)` põe horas em HE100 total/feriado.

Calendário usado nessas decisões:

- `app/core/feriados.py:57-67`: nacional sempre alcança; estadual exige UF; municipal exige UF e município normalizado iguais.
- `app/core/feriados.py:91-95`: sem município, somente nacional.
- `app/core/feriados.py:106-114`: lugar vem de `colaborador.posto.municipio`.

Outro portão pode cancelar o pagamento apurado:

- `app/ponto/motor_calculo_v2.py:1386-1414`: `_dia_sem_previsto_fora_da_janela` exige folga ou feriado, empresa na janela, previsto exatamente zero e ausência de autorização; folga sem escala certa escapa; ao bloquear zera trabalho, noturno e extras inclusive `minutos_extra_100_feriado`. Por isso “há registro” sozinho não garante minutos positivos pagos.

`core/juizes.py`:

- Pergunta F3 é “feriado trabalhado paga dobra ou hora simples?” (`app/core/juizes.py:138`); juiz declarado é `ponto/motor_calculo_v2.py::CICLOS_FERIADO_SIMPLES` (`:260-262`).
- Pendências de F3 são seis entradas (`app/core/juizes.py:437-442`): intermitente zerando calendário e dobrando nacional pelo fallback; TurnoPartido/Comercial supostamente sem olhar flag; Comercial por período supostamente sem flag; supressão literal 12x36 no export; concessão literal 12x36 em CCT; campo manual `Ausencia.hora_extra_100`, descrito como sem leitor.
- As duas descrições de Comercial/TurnoPartido permanecem no catálogo, mas os corpos atuais já consultam `_feriado_dobra_no_dia` em `app/ponto/motor_calculo_v2.py:3030`, `:3193`, `:3275`. Não tratar a descrição histórica como comportamento atual.
- Campo manual recebe checkbox em `app/ponto/views.py:2381`, repassa ao criador em `:2405-2410`; o catálogo diz “gravado e lido por ninguém” (`app/core/juizes.py:442`). Esse campo não é a mesma variável que `PeriodoCalculo.hora_extra_100`.

O277:

- `app/docs/BACKLOG.md:1390` diz literalmente que o corte DP de 14/10 é **“SDF/intermitente em feriado NAO recebe dobra; o cartao nao afirma pagamento em dobro”**; revoga direção anterior da O264, passa a considerar código errado e docstring certa.
- Mesma linha estabelece orientação de 15/10: SDF depende cadastro da O275; intermitente segue docstring **sem entrada nova no frozenset**; cura deve incidir em `feriado_em_dobra`, não retirar `set()` nem alterar `eh_feriado`, e não usar “dia previsto não dobra”; registra “0 prod tocado”.
- O comportamento ainda contradiz essa regra nos caminhos que pagam: constante só exclui 12x36 (`app/ponto/motor_calculo_v2.py:3537`), fábrica dá `True` a intermitente (`:3607-3608`), classe não sobrescreve flag (`:3491-3495`), helper paga quando flag+calendário (`:879`). A v3 empresta esse helper e preserva a decisão (`app/ponto/calculador/regras.py:290-295`, `:344-347`). Zeros acidentais por calendário perdido não constituem implementação de O277.

## 31/10 12:44 · sessao 886c35c942b4 · GRADE-FOLGA · verificador · origem=Code

O diff está **incompleto**: pode esconder uma folga válida ao escolher a candidata, muda a ATA pela ordem dos overlays e não garante corrigir atas já julgadas. Além disso, o teste de calendário alheio não demonstra a regressão específica no legacy. As bases estão em `app/ponto/precedencia.py:234`, `app/escala/utils.py:1198`, `app/ponto/services/cartorio.py:1161` e `app/escala/utils.py:993`.

As referências abaixo são da árvore **ANTES**; referências ao teste novo indicam explicitamente o arquivo proposto no anexo. [nao verificado]

1. **A menor PK pode esconder a folga válida — e o diff leva essa lacuna à grade.**

   A precedência escolhe primeiro **uma única** FolgaDia por pessoa/data, pela menor PK; somente depois descarta calendário de outro vínculo. Entretanto, a unicidade é por **vínculo/data**, permitindo várias candidatas da mesma pessoa no dia. (`app/ponto/precedencia.py:234`, `:298`, `:313`; `app/escala/models.py:1571`.)

   Contraexemplo: dia cuja célula diz trabalho, calendário inválido do vínculo velho com PK menor e folga manual válida com PK maior. O carregador proposto guarda o calendário; o predicado o rejeita; a manual nunca é examinada. Antes, o overlay podia encontrar essa manual; depois, pode deixar o dia em trabalho. A mesma possibilidade existe com calendário válido do vínculo do dia ocultado pela candidata inválida. (`app/ponto/precedencia.py:234`, `:313`, `:316`; `app/escala/models.py:1071`; `app/escala/utils.py:1109`.)

   Portanto, compartilhar o carregador **não basta**: é necessário resolver o conjunto das candidatas válidas, com regra explícita para coexistência. A existência desse contraexemplo em produção é **[nao verificado]**.

2. **A troca de ordem muda resultado observável, inclusive lâmpadas da ATA.**

   Antes há `folga(e1) → ausência(e1) → folga(e2) → ausência(e2)`. A ausência consulta a mesma pessoa, mas pode deixar marcas que uma folga posterior não remove. Os vínculos são percorridos por `-data_inicio`. (`app/escala/utils.py:1785`, `:1128`; `app/escala/models.py:1003`.)

   Exemplo: dia inicialmente de trabalho, justificativa aprovada e folga somente no segundo vínculo percorrido. A primeira ausência marca `justificado=True`; a folga posterior limpa células/órfãs, preservando essa marca. Com todas as folgas primeiro, nenhuma passagem marca a justificativa, pois o dia já é folga. (`app/escala/utils.py:1198`, `:1117`.)

   | Resultado desse cenário | Antes | Depois |
   |---|---|---|
   | `tipo_dia/celulas/orfas` | `folga/[None,None]/[]` | Igual |
   | `justificado` | `True` | Ausente |
   | Duas lâmpadas reconstruídas do DNA | Dispensadas por justificativa | Apagadas |
   | `n_missing` da ATA | 0 | 2 |

   A reconstrução das lâmpadas consulta o DNA mesmo com células vazias; `dispensar_por_ausencia` lê `justificado` para dispensá-las. Esse cenário foi reproduzido em memória com as funções da cópia e consultas dubladas. (`app/ponto/services/cartorio.py:470`, `:509`, `:559`, `:612`.)

   Com falta aprovada também há diferença de dicionário: antes podem sobrar `falta_aprovada=True` e `ausencia_id` após uma folga posterior; depois, a falta já encontra folga e não escreve essas chaves. (`app/escala/utils.py:1170`, `:1174`, `:1117`.)

3. **O vínculo coincide nos dias devolvidos pelos dois builders, com duas ressalvas.**

   No caminho padrão, a precedência carrega células, vínculos da janela e geradoras faltantes; os dois builders carregam todos os vínculos da pessoa e perguntam à mesma `vinculo_do_dia`. Sem geradora, o desempate usa `(ativa, data_inicio)`; não há empate de início para a mesma pessoa pela restrição do modelo. (`app/ponto/precedencia.py:271`, `:275`, `:283`, `:307`; `app/escala/utils.py:964`, `:968`, `:970`, `:1240`, `:1252`, `:1254`; `app/escala/alimentacao.py:253`; `app/escala/models.py:1004`.)

   - **Por turno:** `esc_por_dia[d]` é diretamente a resposta da juíza; inclusive `None`. Geradora fora da vigência produz `None`, sem escolher outro vínculo como reserva. (`app/escala/utils.py:1260`; `app/escala/alimentacao.py:247`.)
   - **Legacy, dias retidos:** o segmento é formado pela juíza; a `lambda: esc` proposta entrega esse mesmo vínculo nos dias conservados. (`app/escala/utils.py:974`, `:983`, `:997`.)
   - **Legacy, dia adicional:** o single recebe também `d0−1`. Nesse dia, a lambda pode responder outro vínculo: no exemplo do anexo, o segmento novo passa **novo em 09/10**, enquanto a juíza responde velho. Esse resultado é descartado pelo recorte. Logo a afirmação literal “mesmo vínculo em todo dia montado” é falsa, embora os dias retidos coincidam. (`app/escala/utils.py:993`, `:995`, `:997`.)
   - **Legacy, vínculo `None`:** fabrica diretamente uma linha folga e não chama o predicado. O builder por turno passa `None` ao predicado proposto. Portanto, não são duas consultas equivalentes nesse ramo; o rótulo legacy já nasce folga. (`app/escala/utils.py:985`, `:987`, `:1260`.)

4. **A mudança de universo atua nas duas direções; o teste não cobre todas.** [nao verificado]

   Antes, por turno, entram as FolgaDia dos vínculos pertencentes a `escs_janela_ids`, aplicadas pela janela inteira. O helper proposto passa a consultar qualquer vínculo da pessoa, reproduzindo o universo atual da precedência. (`app/escala/utils.py:1785`, `:1109`; `app/ponto/precedencia.py:236`.)

   | Situação, mantendo os demais fatos | Direção possível |
   |---|---|
   | Manual de vínculo ausente de `escs_janela_ids` | Trabalho → folga: passa a ser encontrada. |
   | Calendário de outro vínculo presente na janela | Folga → trabalho: deixa de valer pelo filtro. |
   | Calendário do vínculo do dia | Continua elegível. |
   | Candidata inválida de PK menor escondendo manual/calendário válido | Pode perder folga válida, conforme achado 1. |

   Essas direções decorrem do filtro anterior por vínculo, do universo por pessoa e do descarte posterior da candidata; não demonstram quantidade de dias reais atingidos. (`app/escala/utils.py:1109`, `:1116`, `:1786`; `app/ponto/precedencia.py:234`, `:313`.) Quantidades em produção: **[nao verificado]**.

   O teste chamado `test_MORDE_folga_de_calendario_de_outro_vinculo_e_trabalho_na_grade` **já passa no legacy ANTES**: em 15/10, o single do segmento novo consulta somente FolgaDia do novo. É o teste da **manual presa ao velho** que detecta mudança no legacy. Assim, a bateria tem mordida nesse builder, mas não comprova a regressão específica anunciada pelo primeiro teste nos dois caminhos. (`app/escala/utils.py:993`, `:997`, `:1109`; teste proposto `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:62`, `:67`.)

   Faltam casos para candidatas coexistentes, vínculo antigo fora da janela, ausência/justificativa na troca de ordem e ATA/chamado resultantes; os três testes propostos só verificam `tipo_dia` e faixa da precedência. (Teste proposto `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:48`, `:62`, `:67`, `:72`.) RED/verde da suíte nova executada: **[nao verificado]**.

5. **Quem escreve e quem lê os campos envolvidos.**

   | Campo/valor | Escritores | Leitores relevantes |
   |---|---|---|
   | `FolgaDia.calendario_origem_id` não nulo | Gerador grava `ec.folga_calendario`. (`app/escala/folgas.py:157`, `:174`.) | Reconciliação/proteção das manuais, guardas das portas, reparentamento e precedência. (`app/escala/folgas.py:101`, `:165`, `:255`, `:275`, `:405`; `app/ponto/precedencia.py:313`.) |
   | Origem nula/manual | `lancar_folga` e `_foto_do_dia` criam sem calendário. A FK também declara `SET_NULL` na exclusão do calendário. (`app/escala/folgas.py:259`, `:284`; `app/escala/models.py:1557`.) | Os mesmos leitores de proveniência, além do toggle e declaração de dia. (`app/escala/views_plano_folgas.py:112`; `app/ponto/services/declarar_dia.py:108`.) |
   | `FolgaDia.escala_colaborador_id` | Gerador e portas manuais gravam `ec`; reparentamento troca a FK da manual, recusando calendário. (`app/escala/folgas.py:174`, `:259`, `:284`, `:405`, `:435`.) | Alimentação, cálculo da foto, overlay, precedência, plano, diagnóstico e conflito de fase. (`app/escala/alimentacao.py:66`; `app/escala/models.py:1251`, `:1280`, `:1327`; `app/escala/utils.py:1109`; `app/ponto/precedencia.py:236`, `:314`; `app/escala/views_plano_folgas.py:60`; `app/ponto/services/diagnostico_escala.py:102`; `app/escala/services/fase_conflitante.py:74`.) |
   | Vínculo usado no dia | Juíza seleciona; por turno materializa `esc_por_dia`; legacy forma segmentos e passa `esc` ao single. (`app/escala/alimentacao.py:237`; `app/escala/utils.py:1260`, `:974`, `:993`.) | Montagem de marcos/trabalho e, no diff, o predicado de folga. A precedência lê sua própria chamada à mesma juíza. (`app/escala/utils.py:1297`, `:1471`; `app/ponto/precedencia.py:307`.) |
   | `tipo_dia/celulas/orfas` | Builders escrevem a base; folga limpa os três; falta/ausência inteira também; parcial preserva células/órfãs; justificativa limpa missing/órfãs; isenção altera células. (`app/escala/utils.py:727`, `:752`, `:1743`, `:1749`, `:1117`, `:1174`, `:1180`, `:1195`, `:1198`, `:809`.) | Legacy remapeia; conservação e reconciliação alteram células/órfãs; adaptador, cartório e leitor da ATA transportam o resultado. (`app/escala/utils.py:1014`, `:1766`, `:1979`, `:1059`; `app/ponto/services/cartorio.py:564`; `app/escala/services/leitor_celula.py:344`, `:359`.) |

   A porta registrada de FolgaDia é `escala/folgas.py`; o diff altera leitura e dicionários, sem acrescentar escritor persistente dessa entidade. (`app/core/portas.py:466`; trechos propostos sobre `app/escala/utils.py:1101` e `app/ponto/precedencia.py:234`.)

   Para os leitores de `tipo_dia`, abaixo **F→T** significa folga antes e trabalho depois; **T→F** inverte a elegibilidade indicada. Leitores da ATA só recebem a mudança após nova lavratura, pois o leitor recupera o tipo gravado. (`app/escala/services/leitor_celula.py:344`.)

   | Leitor | Comportamento condicionado à mudança |
   |---|---|
   | `app/escala/utils.py:850` | F→T permite marcar isenção do dia, havendo células. |
   | `app/escala/utils.py:1170`, `:1198` | F→T permite falta aprovada e justificativa; ausência não-falta continua podendo cobrir ambos. |
   | `app/escala/utils.py:1016` | Apenas transporta o tipo do segmento. |
   | `app/ponto/services/cartorio.py:567`, `:997` | Grava o tipo na ATA; folga↔trabalho isoladamente não altera o teste de cobertura integral por ausência/falta. |
   | `app/escala/services/leitor_celula.py:344`, `:384` | Transporta o tipo gravado; folga↔trabalho não altera a seleção específica de ausência parcial. |
   | `app/chamados/juizes.py:408` | F→T deixa de declarar “dia sem trabalho” pela ATA; continua declarando se `cel.trabalha is False`. |
   | `app/ponto/supra_juiz.py:258`, `:281`, `:379`, `:396`, `:420`, `:443`, `:448`, `:463` | F→T habilita ramos de cobrança e inconsistência de trabalho; pode desabilitar cobrança em dia sem trabalho. Os demais requisitos permanecem. |
   | `app/ponto/status_dia.py:96`, `:106`, `:115`, `:146` | Muda os fatos usados para lavrar status e os ramos folga/folga trabalhada, conforme batida, previsto e veredito. |
   | `app/ponto/services/espelho.py:690` | F→T desliga `_folga`. |
   | `app/relatorios/furos.py:147`, `:151`, `:218`, `:237` | F→T inclui o dia na apuração de furos, conforme células/âncora; sentinelas não mudam. |
   | `app/inteligencia/calculadores.py:46`, `:141` | F→T inclui o dia nas métricas de trabalho. |
   | `app/escala/services/furos_diarios.py:39` | F→T permite coletar furos desse dia. |
   | `app/folha/causas.py:40`, `:41`, `:100` | F→T inclui em dias previstos e retira de folga com órfãs. |
   | `app/folha/export.py:1071`, `:1075`, `:1080` | F→T inclui no universo de previsto/realizado limitado por dia; não cria abono, que exige ausência. |
   | `app/ponto/services/dia_pago.py:386`, `:393`, `:398` | F→T inclui nos mapas previsto/realizado. T→F exclui mesmo que `minutos_previstos` permaneça positivo. |
   | `app/ponto/services/fechamento.py:1011`, `:1015` | F→T inclui na contagem de dias previstos; não muda a detecção de ausência de células. |
   | `app/escala/orfa_classe.py:65` | F→T deixa a classificação automática de folga e segue a classificação de trabalho, se ainda houver órfãs. |
   | `app/ponto/propositor.py:36`, `:210` | F→T permite avaliar degeneração/deslocamento. |
   | `app/ponto/services/simular_dia.py:12` | F→T permite considerar o dia perfeito, conforme células/órfãs. |
   | `app/ponto/worklist.py:19` | F→T deixa a saída “dia de folga” e permite examinar lâmpadas. |
   | `app/chamados/services/declaracao_texto.py:91`, `:95` | F→T permite oferecer marcos faltantes; sentinelas não mudam. |
   | `app/chamados/services/disputa_emissao.py:68` | F→T permite devolver a data de trabalho para emissão. |
   | `app/chamados/services/materializacao.py:223` | O teste de tipo aceita tanto folga quanto trabalho: não muda por essa troca. |
   | `app/ponto/services/veredito_celula.py:221` | F→T permite materializar trabalho, sujeitos os demais requisitos. |
   | `app/ponto/services/bordas_realizado.py:74` | F→T inclui na auditoria das bordas do realizado. |
   | `app/ponto/management/commands/tripwire_celulas.py:342` | F→T retira da checagem específica de ATA folga sem lâmpadas. |
   | `app/ponto/management/commands/mapa_divergencia.py:131`, `:143`, `:155`, `:228` | A assinatura/comparação passa a refletir o novo tipo. |

6. **Classes de reprovação e outros sítios que ainda discordam.**

   - **Segundo juiz registrado:** a extração proposta do predicado não cria outro vencedor da precedência nem outro seletor de vínculo: essas autoridades continuam sendo `fatos_do_dia` e `vinculo_do_dia`. Há, porém, outros leitores com regras diferentes, descritos abaixo. (`app/core/juizes.py:160`, `:1255`.)
   - **Segundo escritor fora da porta, relógio novo ou inversão de travas:** não aparecem nos trechos propostos; eles consultam dados e modificam a grade em memória. A porta persistente de FolgaDia continua registrada em `escala/folgas.py`. (`app/core/portas.py:466`; alterações propostas sobre `app/escala/utils.py:1101`, `:1785` e `app/ponto/precedencia.py:234`.)
   - **Selo sem mordida suficiente:** ocorre a limitação do teste legacy já descrita; também falta testar a ATA afetada pela ordem. (Teste proposto `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:62`; `app/escala/utils.py:993`; `app/ponto/services/cartorio.py:612`.)
   - **Ativação incompleta:** o diff não altera `REGRA_VERSOES`, e o hash não inclui `tipo_dia/celulas/orfas`. Dia já julgado com impressão igual pode ser pulado, preservando a ATA antiga. É necessário um caminho explícito de atualização — versão/horizonte ou rejulgamento forçado apropriado. (`app/ponto/services/cartorio.py:110`, `:160`, `:324`, `:1148`, `:1157`, `:1161`.) Sua execução ou pouso consta como **[nao verificado]**.

   **Há outro sítio alimentando diretamente o mesmo chamado:** `o_dia_ja_e_folga` busca somente FolgaDia do vínculo do dia. Uma manual presa ao velho vale pelo predicado proposto, mas não é reconhecida ali. Esse booleano decide o caminho que carimba/fecha a pergunta e o fio. (`app/ponto/services/declarar_dia.py:61`, `:90`; `app/chamados/services/validacao.py:261`, `:271`.)

   Se esse reconhecimento falha, a validação vai para `checar_declaracao`; o veto consulta **qualquer FolgaDia da pessoa/data**, sem validar proveniência/vínculo. Assim, calendário antigo que o novo predicado rejeita ainda pode impedir um lançamento por “já tem FOLGA”. É divergência preexistente deixada aberta. (`app/chamados/services/validacao.py:280`; `app/ponto/services/declarar_dia.py:114`; `app/ponto/precedencia.py:161`, `:458`.)

   **Também permanece uma alimentação paralela no calendário:** passa `{data: True}` somente da foto do vínculo carregado. Isso elimina proveniência e vínculo; o novo predicado trata `True` como manual. Se a juíza escolhe outra geradora, esse calendário não é descartado; uma manual em outro vínculo também fica fora dessa alimentação. (`app/colaboradores/services/calendario.py:592`, `:604`, `:607`; `app/ponto/precedencia.py:283`, `:313`.) Esse caminho demonstra divergência no calendário; por si só não demonstra escrita da mesma ATA/chamado.

   A foto por vínculo ainda alimenta o cálculo que gera `trabalha`/DNA; ela não equivale ao universo “manual da pessoa” do predicado proposto. A mesma ATA recebe depois a precedência por outra leitura. Portanto, igualar apenas o overlay não prova convergência de toda a cadeia. (`app/escala/models.py:1251`, `:1280`; `app/ponto/portas/celula.py:243`, `:590`; `app/ponto/services/cartorio.py:1673`.)

Os **11 casos**, sua distribuição e seu desaparecimento após a cura não são comprováveis pelo material fornecido: **[nao verificado]**.

## 31/10 12:43 · sessao 47ffd0481ef0 · - · - · origem=Ronald (tmux)

**Resultado B:** com vínculo do dia resolvido como intermitente, **ambos os chamadores — 12x36 e intermitente — produzem `W/60` em `horas_extras_100` e `horas_extras_100_feriado`**, tanto no nacional quanto no municipal da praça presente no conjunto recebido; `W` são os minutos trabalhados usados pela v3. A escolha do template do dia está em `app/ponto/calculador/alimentacao.py:490–507`; o flag de dobra vem desse template em `:510`; o conjunto de feriados, separadamente, vem do argumento do chamador em `:407/:524`; a escrita das duas rubricas está em `app/ponto/calculador/regras.py:775–789`.

- `_te_dia` começa como template do chamador (`alimentacao.py:487`), mas o vínculo do dia o substitui, junto com `_interm_dia`, em `:490–497`; `_cct_de(_te_dia)` roda em `:507`. Portanto, identificado o vínculo intermitente, a variante do chamador não altera esses flags (`app/ponto/calculador/alimentacao.py:490–510`).
- `_cct_de` constrói motor com `get_motor_cct(c, ini, tipo_escala=_te)` (`app/ponto/calculador/alimentacao.py:387–404`); o ciclo intermitente seleciona `MotorIntermitente` (`app/ponto/motor_calculo_v2.py:3563–3564`); só `12x36` pertence a `CICLOS_FERIADO_SIMPLES` (`:3537`), então o flag é `True` (`:3607–3608`).
- **A diferença decisiva de calendário:** `MotorIntermitente` zera `self.feriados` (`app/ponto/motor_calculo_v2.py:3495`), e `cct_do_motor` copia esse vazio (`app/ponto/calculador/nucleo.py:350`), **mas `rubricas_por_dia` não usa `_cct.feriados` na dobra**: usa `_feriados = set(feriados or ())` do chamador (`app/ponto/calculador/alimentacao.py:407`) e o passa em `:524`.
- `_ContextoFeriado` volta a escrever esse conjunto e o flag em seu próprio contexto (`app/ponto/calculador/regras.py:293–295`), emprestando `MotorBase.eh_feriado` e `_feriado_dobra_no_dia` (`:290–291`). A regra retorna todo `W` se `W>0` e o predicado for verdadeiro (`:344–347`).
- O predicado emprestado é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)` (`app/ponto/motor_calculo_v2.py:879`); `eh_feriado` aceita a data injetada ou consulta `is_feriado(data)` (`:903–905`). O fallback, sem município, só alcança nacional (`app/ponto/motor_calculo_v2.py:30–43`; `app/core/feriados.py:91–95/:57–67`).
- Se o municipal **não** veio no conjunto recebido, não é recuperado pelo fallback sem município (`app/ponto/motor_calculo_v2.py:905/:30–41`; `app/core/feriados.py:57–67/:91–95`). No fechamento o conjunto é montado pelo município do posto e entregue à v3 (`app/ponto/services/fechamento.py:621–623/:1239–1243`; `app/core/feriados.py:106–114`).
- Intermitente não é desviado pelo ramo da folga: mesmo quando `dia_de_trabalho=False`, passa adiante (`app/ponto/calculador/regras.py:646–660/:736–740`); portanto não cai na outra conversão `folga_e_feriado` (`:676–735`).

**Escritores/leitores na v3:**

| Campo | Quem escreve / fornece | Quem lê |
|---|---|---|
| `feriados` | Motor escreve conjunto em `app/ponto/motor_calculo_v2.py:740`, intermitente zera em `:3495`; fronteira copia em `app/ponto/calculador/nucleo.py:350`. Na porta por dia, entretanto, `app/ponto/calculador/alimentacao.py:407/:524` usa o argumento do chamador; contexto o grava em `app/ponto/calculador/regras.py:294`. | Fronteira verifica existência/None em `app/ponto/calculador/nucleo.py:344–350`; regra chama a dobra em `app/ponto/calculador/regras.py:775–777`; método emprestado lê conjunto em `app/ponto/motor_calculo_v2.py:903`. A releitura alternativa usa **`cct.feriados`** em `app/ponto/calculador/periodo.py:830`. |
| `feriado_em_dobra` | Fábrica resolve pelo ciclo em `app/ponto/motor_calculo_v2.py:3607–3608`, motor grava em `:737`; fronteira copia em `app/ponto/calculador/nucleo.py:351`; alimentação lê em `app/ponto/calculador/alimentacao.py:510` e passa em `:524`; contexto grava em `app/ponto/calculador/regras.py:295`. | `app/ponto/motor_calculo_v2.py:879`, emprestado pela v3 em `app/ponto/calculador/regras.py:290`; `do_dia` exige ambos os dados em `:633–638/:775`; releitura lê em `app/ponto/calculador/periodo.py:831`. |
| `prorrogacao_noturna_pos5h` | `get_motor_cct`: `app/core/regua_cct.py:454/:466`; ausência de valor é resolvida no motor em `app/ponto/motor_calculo_v2.py:708–712`. Fronteira renomeia para `prorrogacao_pos5h` em `app/ponto/calculador/nucleo.py:348`; alimentação lê/passa em `app/ponto/calculador/alimentacao.py:508/:530`; contexto noturno grava em `app/ponto/calculador/regras.py:61`. | `app/ponto/calculador/regras.py:609/:627/:85–86/:815–824`; contexto empresta método em `:63`; releitura passa `cct.prorrogacao_pos5h` em `app/ponto/calculador/periodo.py:508`. |
| `hora_reduzida_afastada` | CCT resolve apenas para ciclo 12x36 em `app/core/regua_cct.py:470–472`; motor grava em `app/ponto/motor_calculo_v2.py:719`; fronteira copia em `app/ponto/calculador/nucleo.py:349`; alimentação lê/passa em `app/ponto/calculador/alimentacao.py:509/:530`. | Guarda em `app/ponto/calculador/regras.py:609`; fator noturno em `:88`; releitura passa o flag em `app/ponto/calculador/periodo.py:509`. |
| Lei da família | Declaração nominal das identidades de classe em `app/ponto/calculador/regras.py:487–498`, incluindo `MotorIntermitente` em `:494–497`; leitor único faz lookup exato por `type(motor)` em `:516–518`; alimentação consulta em `app/ponto/calculador/alimentacao.py:403` e passa causa em `:531`. | `do_dia` lê causa e declara atraso/antecipada, descartando pontualidade recebida, em `app/ponto/calculador/regras.py:618–625`; releitura recebe causa e a passa em `app/ponto/calculador/periodo.py:783–789`; outro consumidor é `folha_julga_pontualidade` em `app/chamados/juizes.py:1529/:1545–1551`. |

**Outro sítio relevante:** `periodo.reler` usa `cct.feriados`, e não o calendário do chamador da alimentação (`app/ponto/calculador/periodo.py:829–831`). Com `cct_do_motor(MotorIntermitente)`, recebe o conjunto zerado (`app/ponto/motor_calculo_v2.py:3495`; `app/ponto/calculador/nucleo.py:350`): nacional pode dobrar pelo fallback, municipal sozinho não; quando dobra, grava toda a jornada no último período em `app/ponto/calculador/periodo.py:833–842`, pelo mesmo predicado `app/ponto/motor_calculo_v2.py:879/:903–905`.

## 31/10 12:41 · sessao 8e16097b8d6d · - · - · origem=Ronald (tmux)

**Troca de ordem muda resultado, inclusive ATA.** Provei em memória com `python3 -B`, extraindo por AST os overlays e funções de ATA reais, dublando somente consultas/imports. Nenhum arquivo ou banco foi escrito. Fontes executadas: `app/escala/utils.py:1101`, `app/escala/utils.py:1123`, `app/ponto/services/cartorio.py:428`, `app/ponto/services/cartorio.py:559`, `app/ponto/services/cartorio.py:594`.

Caso: dois vínculos em `rows`, folga somente no segundo, justificativa aprovada para um dia inicialmente de trabalho. Antes: `A1 → B1 → A2 → B2`; B1 marca `justificado=True`, A2 limpa células/órfãs e põe folga, mas preserva a marca; B2 não remove. Depois: folgas primeiro, e nenhuma ausência marca justificativa porque o dia já é folga. Fontes: `app/escala/utils.py:1785`, `app/escala/utils.py:1117`, `app/escala/utils.py:1198`.

Resultado medido no cenário sintético:

| Resultado | Antes | Depois |
|---|---|---|
| `tipo_dia/celulas/orfas` | `folga/[None,None]/[]` | igual |
| `justificado` | `True` | ausente |
| ATA, duas lâmpadas do DNA | acesas, `dispensada='justificativa'` | apagadas |
| `n_missing` da ATA | 0 | 2 |

As lâmpadas são reconstruídas do DNA mesmo com células `None`; depois o cartório lê `justificado` para dispensá-las. Portanto, há efeito além de metadado sem leitor: `app/ponto/services/cartorio.py:448`, `app/ponto/services/cartorio.py:470`, `app/ponto/services/cartorio.py:509`, `app/ponto/services/cartorio.py:612`.

Com falta aprovada também muda o dict: antes B1 grava `falta_aprovada=True/ausencia_id`, A2 mantém essas chaves ao trocar para folga; depois todos os B pulam a falta em folga e as chaves não aparecem. Não localizei leitor vivo de `dia['falta_aprovada']`; a mudança de dict está demonstrada pelo código. Fontes: `app/escala/utils.py:1170`, `app/escala/utils.py:1174`, `app/escala/utils.py:1117`.

**Escritores e transporte de `dias_out`:**

- Builder simples escreve folga/trabalho, células e órfãs; depois conserva sobras e aplica overlays: `app/escala/utils.py:725`, `app/escala/utils.py:727`, `app/escala/utils.py:741`, `app/escala/utils.py:752`, `app/escala/utils.py:763`, `app/escala/utils.py:768`.
- Legacy fabrica folga para segmento sem vínculo; incorpora o builder simples e remapeia os três campos à régua final: `app/escala/utils.py:985`, `app/escala/utils.py:993`, `app/escala/utils.py:1014`, `app/escala/utils.py:1016`.
- Por turno escreve folga/trabalho, células, órfãs, recolhe sobras/intervalos; depois overlays e reconciliação de pontas alteram células/órfãs: `app/escala/utils.py:1743`, `app/escala/utils.py:1749`, `app/escala/utils.py:1766`, `app/escala/utils.py:1785`, `app/escala/utils.py:1979`.
- Folga escreve os três; ausência inteira/falta também; ausência parcial mantém células/órfãs; justificativa limpa missing/órfãs sem mudar tipo; isenção modifica células e usa trabalho para marcar o dia: `app/escala/utils.py:1117`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`, `app/escala/utils.py:1195`, `app/escala/utils.py:1198`, `app/escala/utils.py:809`, `app/escala/utils.py:850`.
- Cartório lê os três e grava tipo, lâmpadas, contagens e órfãs na ATA; leitor da célula recupera a ATA e entrega nova grade aos consumidores: `app/ponto/services/cartorio.py:564`, `app/ponto/services/cartorio.py:567`, `app/ponto/services/cartorio.py:574`, `app/escala/services/leitor_celula.py:344`, `app/escala/services/leitor_celula.py:359`.

**Censo dos leitores de `tipo_dia` e efeito condicional do diff.** Considere F→T = folga antes, trabalho depois; T→F = inverso. Nos leitores da ATA, o efeito depende de nova lavratura; a leitura não remonta automaticamente o dia: `app/escala/services/leitor_celula.py:331`, `app/escala/services/leitor_celula.py:344`.

| Leitor | Efeito de F→T; T→F faz o inverso salvo observação |
|---|---|
| `app/escala/utils.py:850` | Passa a permitir marca de isenção do dia, havendo células dict. |
| `app/escala/utils.py:1170` | Falta aprovada passa a poder virar `falta`; não-falta vence ambas. |
| `app/escala/utils.py:1198` | Justificativa passa a ser aplicada. |
| `app/escala/utils.py:1016` | Só propaga tipo do segmento legacy. |
| `app/ponto/services/cartorio.py:567` | Lavra o novo tipo. |
| `app/ponto/services/cartorio.py:997` | F→T isolado não muda o teste ausência/falta; eventual conversão subsequente em falta muda cobertura integral. |
| `app/escala/services/leitor_celula.py:344` | Só propaga tipo da ATA; fallback `cel.trabalha` fica igual. |
| `app/escala/services/leitor_celula.py:384` | Folga↔trabalho isoladamente não altera alvo de cobertura parcial, que exige ausência. |
| `app/chamados/juizes.py:408` | Deixa de declarar dia sem trabalho **se** `cel.trabalha is not False`; se for False continua sem trabalho independentemente da ATA. |
| `app/ponto/supra_juiz.py:258`, `:281`, `:379`, `:396`, `:420`, `:443`, `:448`, `:463` | Reabre ramos de cobrança/realizado/orfandade de trabalho conforme demais gates; deixa o ramo de cobrança em dia sem trabalho, que também exige previsto zero. |
| `app/ponto/status_dia.py:96`, `:106`, `:115`, `:146` | Deixa os rótulos folga/folga trabalhada condicionados ao tipo, com demais gates preservados; tipo vai aos fatos do status. |
| `app/ponto/services/espelho.py:690` | Flag visual `_folga` fica False. |
| `app/relatorios/furos.py:147`, `:151`, `:218`, `:237` | F→T entra na apuração de furos, sujeito a sem_ancora e células; sentinelas fora_vinculo/sem_celula não mudam. |
| `app/inteligencia/calculadores.py:46`, `:141` | Entra em numerador/denominador das métricas conforme células/limbo. |
| `app/escala/services/furos_diarios.py:39` | Entra na coleta de furos diários, sujeito a previsto e janela. |
| `app/folha/causas.py:40`, `:41`, `:100` | Entra em dias previstos/furados e deixa conjunto folga com órfãs; diagnóstico pode mudar. |
| `app/folha/export.py:1071`, `:1075`, `:1080` | Entra no universo trabalho+ausência, previsto e realizado com cap passam a contribuir; não vira abono porque exige ausência. |
| `app/ponto/services/dia_pago.py:386`, `:393`, `:398` | Entra nos mapas previsto/realizado com cap; não vira abono porque exige ausência. **T→F exclui o dia mesmo se `minutos_previstos` continuar positivo.** |
| `app/ponto/services/fechamento.py:1011`, `:1015` | Folga↔trabalho não altera abstinência sem_celula; F→T entra na contagem/soma dos dias previstos. |
| `app/escala/orfa_classe.py:65` | Órfãs deixam a classificação automática `FOLGA_DESMENTIDA` e seguem heurística de trabalho. Overlay que apague órfãs torna retorno vazio antes deste teste. |
| `app/ponto/propositor.py:36`, `:210` | Dia passa a poder ser avaliado como degenerado e a contribuir para suspeita de deslocamento. |
| `app/ponto/services/simular_dia.py:12` | Dia passa a poder ser considerado perfeito, respeitando órfãs/missing/tipos. |
| `app/ponto/worklist.py:19` | Sai de “dia de folga”/sem classificação e passa a verificar lâmpadas. |
| `app/chamados/services/declaracao_texto.py:91`, `:95` | Passa a oferecer marcos missing cobrados; sentinelas não mudam. |
| `app/chamados/services/disputa_emissao.py:68` | Passa a devolver a data de trabalho para emissão da disputa. |
| `app/chamados/services/materializacao.py:223` | Teste do tipo não muda: aceita tanto folga quanto trabalho; somente sentinelas são barradas. Marcos podem mudar via células/ATA. |
| `app/ponto/services/veredito_celula.py:221` | Passa a permitir materializar trabalho, sujeito aos demais requisitos. |
| `app/ponto/services/bordas_realizado.py:74` | Entra na auditoria dos limites do realizado. |
| `app/ponto/management/commands/tripwire_celulas.py:342` | Sai da checagem específica de ATA folga sem lâmpadas. |
| `app/ponto/management/commands/mapa_divergencia.py:131`, `:143`, `:155`, `:228` | Assinatura e causa da divergência passam a refletir novo tipo. |

`app/colaboradores/services/calendario.py:794` lê `tipo_do_dia` da precedência, não `tipo_dia` da grade; `e6_piso_autopsia` apenas menciona o campo em comentário e usa a autoridade do previsto: `app/ponto/management/commands/e6_piso_autopsia.py:99`, `app/ponto/management/commands/e6_piso_autopsia.py:104`.

## 31/10 12:41 · sessao 516c9bd1170f · REPROVA-E-DESENHO-B3 · verificador · origem=Code

**REPROVA como “muda só o limiar”.** Trocar `3 → 1` antecipa o disparo, mas não implementa liberação pela linha, exceção por erro bobo ou a medição prometida. O Stop também tem implementação própria fora de `disjuntor.py`. (`bin/disjuntor.py:574`, `bin/disjuntor.py:950`, `bin/hook_stop_fila1.py:1050`)

1. **Escrever a linha não libera o próximo verificador.** O Stop deixa de cobrar quando reconhece a pergunta no topo; o PreToolUse não consulta o RELATO: continua negando enquanto `disparado` estiver ativo. Quem limpa esse estado hoje é `rearma_b3`. Portanto “até a linha existir” muda o mecanismo de destravamento. (`bin/hook_stop_fila1.py:1051`, `bin/disjuntor.py:947–954`, `bin/disjuntor.py:592–593`)

2. **O reconhecedor não exige o conteúdo proposto.** Basta `DESENHO` em maiúsculas e a identidade da obra na mesma linha. Não verifica juiz único, escritor único, célula soberana, lâmpada 4a, pergunta sem juiz ou `arquivo:linha`. Só considera as duas primeiras linhas úteis; o selo aceita até `DESENHO OBRA-X (C)`. (`bin/hook_stop_fila1.py:417`, `bin/hook_stop_fila1.py:420–440`, `bin/tests/test_disjuntor.sh:654–655`)
   
   Também não há vínculo com o disparo atual, data ou consumo da declaração. Uma linha pode nomear várias obras; o problema de citar outra obra na mesma linha e calá-la já está registrado. (`bin/hook_stop_fila1.py:417`, `bin/disjuntor.py:507–517`, `app/docs/BACKLOG.md:811`)

3. **“Erro bobo, declara por quê e libera” não existe.** A máquina atual acumula agentes reprovados e mantém o disparo até rearme; não tem transição de dispensa por declaração nem registro específico dessa classificação. O rearme registra usuário e motivo genéricos. Usá-lo como dispensa confundiria dois atos diferentes. (`bin/disjuntor.py:565–580`, `bin/disjuntor.py:585–595`)

4. **O Stop não garante pedir isso na primeira reprovação.** A varredura B3 ocorre no final do caminho normal. Antes dela há saídas por orçamento, PAREI, ESPERANDO provado, fila vazia, teto e B2. O próprio selo comenta que o teto libera antes da varredura e cala o recado. O PreToolUse varre independentemente e pode negar sem esse aviso anterior. (`bin/hook_stop_fila1.py:1219–1248`, `bin/hook_stop_fila1.py:1286–1302`, `bin/tests/test_disjuntor.sh:330–332`, `bin/disjuntor.py:949–950`)

5. **Os contadores têm unidades diferentes das prometidas.**
   
   - B3 conta **IDs distintos de agentes reprovados por obra**, acumulados no JSON, sem janela de 24 horas. Não conta cada mensagem de reprovação. (`bin/disjuntor.py:556–570`)
   - O log recebe **um disparo por ciclo armado**, não uma linha por Stop ou por tentativa negada. (`bin/disjuntor.py:574–579`)
   - O ESTADO conta eventos `DISJUNTOR` das últimas 24 horas pelo identificador `B3`; não lê obra, classificação ou motivo. `REARMA` não entra nessa leitura. Logo não entrega “liberadas por erro bobo, com o porquê”. (`bin/disjuntor.py:1032–1055`)
   - `gerar_estado.py` delega a `disjuntor.ler/linha`; a apresentação atual diz “disparos 24 h” e soma negações gerais. Não há outro limiar B3 nesse gerador. (`bin/gerar_estado.py:277–283`, `bin/disjuntor.py:1147–1160`)

6. **A troca alcança contagem antiga e não garante log literal com `1`.** A varredura reaplica o limiar a todas as obras persistidas: uma obra antiga com uma ou duas reprovações dispara na próxima avaliação. O log usa o `n` real, que pode ser maior que um; as mensagens de Stop/PreToolUse usam a constante. Assim podem anunciar `1` enquanto o disparo registra `2` ou mais. (`bin/disjuntor.py:559–578`, `bin/disjuntor.py:954`, `bin/hook_stop_fila1.py:1054`)

7. **“Rearme zera, como hoje” precisa delimitar o que zera.** Ele esvazia `reprovados`, baixa `disparado` e guarda os IDs anteriores em `rearmado`, para não recontá-los. Não apaga disparos do log nem zera o placar das últimas 24 horas. (`bin/disjuntor.py:567–568`, `bin/disjuntor.py:590–593`, `bin/disjuntor.py:1036–1055`)
   
   “Do Ronald” também não é autenticação implementada: a CLI aceita obra/motivo e registra `$USER`; o selo a executa diretamente. A guarda atual protege a chave/settings, não esse comando de rearme. (`bin/disjuntor.py:136–158`, `bin/disjuntor.py:590–591`, `bin/disjuntor.py:1170–1173`, `bin/tests/test_disjuntor.sh:363–365`)

8. **Há mensagens e selos presos ao desenho antigo.**
   
   | Sítio | Suposição atual |
   |---|---|
   | `bin/disjuntor.py:11–12`, `:68`, `:94`, `:949` | Terceira reprovação/quarto verificador, em documentação e comentários. |
   | `bin/disjuntor.py:576–578` | Ação do log cravada em **“4o verificador final negado”**; plural fixo `reprovacoes`. |
   | `bin/hook_stop_fila1.py:1052–1054` | Stop também crava **“4o”**; com constante `1`, fica “1 reprovacoes” e “falhou 1 vezes”. |
   | `bin/disjuntor.py:951–954` | PreToolUse mantém “QUAL PADRAO”/“POR QUE falhou”, sem os campos novos. |
   | `bin/tests/test_disjuntor.sh:320–329`, `:376–382` | Duas reprovações **devem passar**; terceira dispara; mensagens/log exigem literalmente `3`. |
   | `bin/tests/test_disjuntor.sh:366–370`, `:384–387` | Casos sem Stop, falha de gravação e identidade ainda semeiam três reprovações. |
   | `bin/tests/test_disjuntor.sh:759–760` | Mutantes ancoram frases antigas, inclusive **“o 4o”**; âncora ausente já causa falha. (`:712–717`) |

   Os selos de topo provam que o recado do Stop cala, mantendo `decision=block` pelo `siga`; não provam que a linha libera PreToolUse. (`bin/tests/test_disjuntor.sh:335–348`)

9. **Preservar “o mesmo de hoje” exige preservar o alcance exato.** A leitura conta apenas agentes com metadado `verificador`, prompt reconhecido como FINAL e veredito no `SubagentHandback`; reconhece o prefixo da primeira linha, não qualquer texto de reprovação. A negativa B3 intercepta lançamento `Agent/verificador` FINAL, não todo caminho possível de revisão. (`bin/disjuntor.py:529–541`, `bin/disjuntor.py:550–557`, `bin/disjuntor.py:944–950`)
   
   A chave pula os disjuntores no Stop e no PreToolUse, mas a guarda da própria chave/settings continua ativa mesmo desligada: essa é uma exceção atual ao “desliga tudo”. (`bin/hook_stop_fila1.py:1040–1041`, `bin/disjuntor.py:979–981`)

Os registros anteriores também dizem terceira reprovação: devem ser distinguidos do novo corte, preservando sua natureza histórica. O `3` do B2 pertence a outro limiar. (`app/docs/CORTES.md:413`, `app/docs/BACKLOG.md:876`, `app/docs/PROMPTS.md:1695`, `bin/disjuntor.py:62`)

Análise estática; não executei o selo porque ele cria arquivos temporários e remove árvores. (`bin/tests/test_disjuntor.sh:37–49`)

## 31/10 12:40 · sessao a3f1a051fdbb · - · - · origem=Ronald (tmux)

O vínculo coincide nos dias efetivamente devolvidos, no caminho padrão de `fatos_do_periodo`: ambos os builders usam `vinculo_do_dia` com células completas; a precedência carrega os candidatos da janela e acrescenta as geradoras faltantes. A ausência de empate por `data_inicio` é garantida por unicidade por colaborador. Referências: `app/escala/utils.py:964`, `:968`, `:971`, `:1240`, `:1252`, `:1255`, `:1260`; `app/ponto/precedencia.py:271`, `:275`, `:283`, `:307`; `app/escala/models.py:1004`.

Há duas ressalvas:

- O legacy chama o single com um dia adicional `d0−1`, portanto a `lambda: esc` proposta pode devolver outro vínculo nesse padding: no exemplo, novo em 09/10, quando a juíza devolveria velho. O resultado desse dia é descartado, sem alterar o dia retido. `app/escala/utils.py:993`, `:995`, `:997`.
- Quando a juíza devolve `None`, o legacy cria diretamente o dia folga e não chama o predicado; por turno, o mapa contém `None`. Geradora fora da vigência resulta em `None` nas duas consultas à juíza. `app/escala/utils.py:985`, `:987`, `:1260`; `app/escala/alimentacao.py:247`, `:250`.

O teste chamado `test_MORDE_folga_de_calendario_de_outro_vinculo_e_trabalho_na_grade` **já passa no legacy antes do diff**: a FolgaDia velha de 15/10 não é consultada pelo single do segmento novo. A mordida legacy do conjunto é o caso manual preso ao vínculo velho, que antes não aparece no segmento novo. Logo o nome/comentário promete uma regressão específica nos dois builders que o teste não demonstra. `app/escala/utils.py:993`, `:997`, `:1109`; anexo `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:63`, `:68`.

O atalho do calendário continua escolhendo outra representação: passa `{data: True}` somente da foto do vínculo carregado, e não a candidata por menor pk da pessoa; o predicado proposto aceita `True` como manual e perde a origem de calendário. A precedência pode completar outra geradora, sem reparar esse booleano. `app/colaboradores/services/calendario.py:593`, `:604`, `:607`; `app/ponto/precedencia.py:283`.

O registro não aponta um segundo juiz de vínculo introduzido pelo diff: a autoridade é `escala/alimentacao.py::vinculo_do_dia`; o novo predicado apenas recebe sua resposta. O registro também distingue a pergunta de vínculo da pergunta de foto/fase. `app/core/juizes.py:1255`, `:1299`.

A ativação está incompleta para atas existentes com impressão idêntica: o cartório pula o dia já julgado se `forcar=False`; o diff não altera a versão e a grade não integra essa impressão. Uma nova versão com horizonte ou rejulgamento forçado resolveria essa perna; sua execução não consta do dado `[não verificado]`. Evitar afirmar “nenhuma ata existente muda”, porque mudanças concomitantes de insumos também invalidam a impressão. `app/ponto/services/cartorio.py:110`, `:160`, `:1148`, `:1157`, `:1161`.

## 31/10 12:40 · sessao e5bf9903abb4 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **Candidata inválida pode esconder folga válida.** A seleção atual guarda somente a menor PK por pessoa/data (`app/ponto/precedencia.py:234-238`), embora o banco permita uma FolgaDia por vínculo/data (`app/escala/models.py:1571`). Havendo calendário antigo inválido de PK menor e manual válida de PK maior, o predicado descarta a primeira e nunca vê a segunda; a extração proposta preserva essa lacuna e a leva à grade. Ocorrência concreta no dado: **[não verificado]**.

- **Calendário segue com alimentação incompatível com o predicado.** Passa `{data: True}` derivado apenas das folgas do vínculo ativo (`app/colaboradores/services/calendario.py:592-608`). Isso perde `calendario_origem_id`/`escala_colaborador_id`; o predicado proposto aceita `True` como manual, inclusive quando a geradora do dia é outro vínculo. A precedência já completa os vínculos faltantes das geradoras (`app/ponto/precedencia.py:279-287`), mas não recupera a proveniência desse booleano. Manual de outro vínculo também fica fora dessa alimentação (`app/colaboradores/services/calendario.py:607`).

- **Outra resposta viva alimenta o mesmo chamado:** `o_dia_ja_e_folga` procura somente FolgaDia do vínculo do dia (`app/ponto/services/declarar_dia.py:61-70`, `:90-91`); o aceite do admin usa esse resultado para carimbar/fechar pergunta e fio (`app/chamados/services/validacao.py:247-271`). Manual presa em vínculo velho vale pelo predicado proposto, mas aqui não é reconhecida como já declarada. A alternativa passa por `checar_declaracao` (`app/chamados/services/validacao.py:280`), cujo veto encontra qualquer FolgaDia da pessoa/data sem conferir validade (`app/ponto/precedencia.py:161-166`, `:458-466`). Assim tanto a manual velha válida quanto o calendário velho inválido podem gerar recusa por folga existente. É lacuna preexistente deixada fora do diff, não regressão criada nele.

Escritores e portas de FolgaDia:

| Campo/valor | Escritor |
|---|---|
| FK vínculo + calendário não nulo | `gerar_foto_folgas`: `app/escala/folgas.py:157-175` |
| FK vínculo + calendário nulo/manual | `lancar_folga`: `app/escala/folgas.py:254-260`; `_foto_do_dia`: `:274-284`; nulabilidade/default: `app/escala/models.py:1557-1559` |
| Troca FK vínculo da manual | `reparentar_folga`: `app/escala/folgas.py:435-436`; calendário é excluído desse ato em `:405-406` |
| Exclusão indireta do calendário pode nulificar origem | `SET_NULL`: `app/escala/models.py:1557-1559`; rota concreta de exclusão: **[não verificado]** |
| Exclusão indireta do vínculo apaga FolgaDia | `CASCADE`: `app/escala/models.py:1553-1554` |

A porta declarada é exclusivamente `escala/folgas.py` (`app/core/portas.py:466`). O diff fornecido não escreve FolgaDia, não lê relógio novo nem adiciona trava: modifica leitura e dicionários da grade (`cura.diff`, trechos de `_aplicar_folgas_foto`; linhas posteriores propostas **[não verificado]**).

Leitores adicionais dos campos:

- Proveniência calendário/manual: divergência da foto (`app/escala/folgas.py:101-104`), proteção/reconciliação da foto (`:165-175`), guardas das portas manuais (`:254-256`, `:274-276`), reparentamento (`:405-418`, `:453-457`), toggle (`app/escala/views_plano_folgas.py:112-114`), declaração de dia (`app/ponto/services/declarar_dia.py:108-112`), filtro atual da precedência (`app/ponto/precedencia.py:313-315`).
- FK vínculo como universo de folga: alimentação (`app/escala/alimentacao.py:66-69`), cálculo aritmético (`app/escala/models.py:1251-1259`, `:1280-1282`, `:1327-1334`), defeito mensal (`:1220-1221`), plano (`app/escala/views_plano_folgas.py:60-70`), diagnóstico (`app/ponto/services/diagnostico_escala.py:102-104`), conflito de fase (`app/escala/services/fase_conflitante.py:74-80`), overlay atual (`app/escala/utils.py:1109-1111`).

Essas consultas mensais/fase não são todas a pergunta de validade da folga lançada: o gerador lê a foto por vínculo e grava `trabalha`/DNA (`app/ponto/portas/celula.py:243-248`, `:590-596`); o cartório grava separadamente a precedência na mesma ata (`app/ponto/services/cartorio.py:1673-1682`). A auditoria C5 também aceita folga pela cobertura de algum vínculo sem provenance (`app/chamados/management/commands/auditar_invariantes_chamados.py:1530-1549`), mas é alarme, não escritor de ata/chamado.

O comando de rejulgamento também seleciona qualquer FolgaDia pessoa/data (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:55-60`), porém o resultado efetivo é delegado a `julgar_celula` (`:69-71`); não impõe por si só a validade final.

## 31/10 12:40 · sessao b8196e590e3e · - · - · origem=Ronald (tmux)

O contrato vai além de mudar a constante: o destravamento por linha escrita e o desvio “erro bobo” não existem na máquina atual. `bin/disjuntor.py:947`, `bin/disjuntor.py:585`.

1. **Linha no RELATO cala o Stop, mas não libera verificador.** O Stop consulta `_pergunta_b3_escrita`; o PreToolUse consulta somente `varrer_b3` e nega enquanto `disparado` estiver ativo. Apenas `rearma_b3` limpa esse estado. `bin/hook_stop_fila1.py:1050`, `bin/disjuntor.py:950`, `bin/disjuntor.py:593`.

2. **O conteúdo exigido não é validado.** O reconhecedor atual aceita qualquer linha que contenha `DESENHO` e a identidade da obra; não exige uma das quatro leis, pergunta sem juiz, `arquivo:linha` ou motivo. Ele considera somente as duas primeiras linhas úteis. `bin/hook_stop_fila1.py:411`, `bin/hook_stop_fila1.py:417`, `bin/hook_stop_fila1.py:420`, `bin/hook_stop_fila1.py:440`.

3. **O Stop não garante pedir isso na primeira reprovação.** A varredura/recado B3 acontece no final do caminho normal; antes existem saídas por B4, orçamento, PAREI, ESPERANDO provado, fila vazia, teto e B2. O teste reconhece explicitamente que o teto pode calar o recado. `bin/hook_stop_fila1.py:1208`, `bin/hook_stop_fila1.py:1219`, `bin/hook_stop_fila1.py:1228`, `bin/hook_stop_fila1.py:1233`, `bin/hook_stop_fila1.py:1247`, `bin/hook_stop_fila1.py:1286`, `bin/hook_stop_fila1.py:1297`, `bin/hook_stop_fila1.py:1302`, `bin/tests/test_disjuntor.sh:330`.

4. **“Erro bobo” não tem transição nem registro.** O estado B3 guarda `reprovados`, `disparado` e `rearmado`; o rearme registra motivo genérico e usuário, sem classificação de liberação. A CLI não autentica Ronald: apenas chama a função com os argumentos. `bin/disjuntor.py:566`, `bin/disjuntor.py:585`, `bin/disjuntor.py:590`, `bin/disjuntor.py:1170`.

5. **A medição prometida não corresponde ao ESTADO atual.** O leitor conta eventos `DISJUNTOR` nas últimas 24 h por identificador (`B3`), sem ler motivo, conteúdo da pergunta ou liberação. `REARMA` não participa dos eventos lidos. A frase é “disparos 24 h”, não “paradas de desenho” nem liberações por erro bobo. `bin/disjuntor.py:1032`, `bin/disjuntor.py:1036`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:1147`, `bin/disjuntor.py:1156`.

6. **O gerador ESTADO delega a esse leitor; não possui outro limiar.** Ele importa `disjuntor.ler/linha` e insere a frase na seção Disjuntores. Alterar apenas a seção visual não criaria a métrica ausente. `bin/gerar_estado.py:277`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:336`.

7. **Baixar o limiar também alcança reprovações antigas.** O contador persiste IDs por obra, sem janela temporal; a varredura verifica todas as obras guardadas. Obras antigas com uma ou duas reprovações passam a disparar no próximo scan. O log usa `n` real, que pode ser maior que um, e o texto mantém `reprovacoes` no plural. Rearmar zera a contagem da obra, não apaga disparos anteriores do log/ESTADO 24 h. `bin/disjuntor.py:559`, `bin/disjuntor.py:565`, `bin/disjuntor.py:572`, `bin/disjuntor.py:576`, `bin/disjuntor.py:590`, `bin/disjuntor.py:1036`.

8. **Sítios que conservam a regra antiga:** docstring 3ª/4º; comentário do limiar e do reconhecedor FINAL; ação de log “4o verificador”; comentário PreToolUse “3a”; mensagem Stop “4o verificador”. As mensagens parametrizadas passam a dizer “1 reprovacoes”/“falhou 1 vezes”. `bin/disjuntor.py:11`, `bin/disjuntor.py:68`, `bin/disjuntor.py:94`, `bin/disjuntor.py:578`, `bin/disjuntor.py:949`, `bin/disjuntor.py:951`, `bin/hook_stop_fila1.py:1052`.

Referências documentais ao contrato antigo aparecem em `app/docs/CORTES.md:413`, `app/docs/BACKLOG.md:876`, `app/docs/PROMPTS.md:1695`, `app/docs/PROMPTS.md:1700` e `app/docs/PROMPTS.md:1707`; são registros de avais anteriores, cuja relação com o novo corte precisa ficar explícita, sem tratá-los automaticamente como código a substituir. `app/docs/CORTES.md:413`, `app/docs/PROMPTS.md:1707`.

A chave realmente pula os disjuntores no Stop e no PreToolUse; a guarda da própria chave/settings continua ativa mesmo desligada, portanto “desliga tudo” tem essa exceção atual. `bin/hook_stop_fila1.py:1040`, `bin/disjuntor.py:979`.

## 31/10 12:39 · sessao 51f85d210cb4 · - · - · origem=Ronald (tmux)

Achados dos selos, por leitura estática:

- **Trocar só a constante quebra o selo atual.** Duas reprovações finais devem deixar Stop sem B3 e permitir PreToolUse; só a terceira dispara. Isso está codificado em `bin/tests/test_disjuntor.sh:320`, `bin/tests/test_disjuntor.sh:323`, `bin/tests/test_disjuntor.sh:324` e novamente em `bin/tests/test_disjuntor.sh:376`, `bin/tests/test_disjuntor.sh:380`.
- **Mensagens/log exigem literalmente 3.** Stop exige `3 reprovacoes` e `falhou 3 vezes`; PreToolUse exige `falhou 3 vezes`; trilha exige `3 reprovacoes`. Referências: `bin/tests/test_disjuntor.sh:327`, `bin/tests/test_disjuntor.sh:328`, `bin/tests/test_disjuntor.sh:329`. A descrição do selo ainda promete terceira reprovação/quarto verificador: `bin/tests/test_disjuntor.sh:13`.
- **Os casos de varredura sem Stop e de falha de gravação continuam semeando três reprovações**, portanto não certificam primeira reprovação: `bin/tests/test_disjuntor.sh:366`, `bin/tests/test_disjuntor.sh:367`, `bin/tests/test_disjuntor.sh:370`. A identidade O9 também só é exercitada com três: `bin/tests/test_disjuntor.sh:384`.
- **Linha no topo hoje só cala recado do Stop.** `_b3_cala` exige `decision=block` com o `siga` da fila, sem mensagem B3 e sem FALHA; não prova liberar PreToolUse. Referências: `bin/tests/test_disjuntor.sh:335`, `bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:339`. As linhas aceitas não contêm lei concreta nem pergunta com arquivo:linha: `bin/tests/test_disjuntor.sh:343`, `bin/tests/test_disjuntor.sh:345`. O teste unitário aceita até `DESENHO OBRA-X (C)`: `bin/tests/test_disjuntor.sh:654`.
- **O selo atual exige topo entre as primeiras duas linhas úteis**, inclusive separadas por branco; terceira linha não cala. Referências: `bin/tests/test_disjuntor.sh:343`, `bin/tests/test_disjuntor.sh:347`, `bin/tests/test_disjuntor.sh:353`. Mutantes preservam essa política em `bin/tests/test_disjuntor.sh:786`, `bin/tests/test_disjuntor.sh:797`, `bin/tests/test_disjuntor.sh:798`, `bin/tests/test_disjuntor.sh:810`.
- **Rearme é testado como CLI invocável pelo próprio agente**, com motivo livre `Ronald respondeu o desenho`; a prova é somente liberação imediata e persistência após novo Stop. Referências: `bin/tests/test_disjuntor.sh:363`, `bin/tests/test_disjuntor.sh:364`, `bin/tests/test_disjuntor.sh:365`. O código correspondente só registra `$USER` e motivo, guarda os IDs antigos em `rearmado` e zera o ciclo: `bin/disjuntor.py:590`, `bin/disjuntor.py:592`, `bin/disjuntor.py:593`.
- **Mudar mensagens exige atualizar mutantes textuais.** Há âncoras literais para `QUAL PADRAO quebra` e `o 4o`: `bin/tests/test_disjuntor.sh:759`, `bin/tests/test_disjuntor.sh:760`. O executor acusa falha quando a âncora desaparece ou deixa de ser única: `bin/tests/test_disjuntor.sh:712`, `bin/tests/test_disjuntor.sh:715`.
- **Não há cobertura do novo contador de paradas/dispensas por erro bobo nos casos B3 existentes.** O bloco verifica a trilha antiga e somente `b3_sem_obra` na leitura do ESTADO; a outra asserção do leitor cobre negações gerais/RELATO ausente. Referências: `bin/tests/test_disjuntor.sh:329`, `bin/tests/test_disjuntor.sh:388`, `bin/tests/test_disjuntor.sh:391`, `bin/tests/test_disjuntor.sh:626`.
- **Rodar este selo violaria só leitura:** cria diretório temporário, arquivos e remove árvores. Referências: `bin/tests/test_disjuntor.sh:37`, `bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:49`.

## 31/10 12:36 · sessao 0819a0bb99c3 · INTERMITENTE-EM-FERIADO · leitura · origem=Code

**1. Fontes existentes e limites da leitura**

**Texto integral da CLT, do art. 452-A/§6 e da Lei 605/49/art. 9:** sem fonte no repo. Foram encontradas referências a esses dispositivos, sem sua transcrição integral. [nao verificado]

**Conteúdo cadastrado de `Sindicato.obs`:** sem fonte no repo. [nao verificado] O modelo contém apenas a declaração `obs = models.TextField(blank=True)`. (`app/core/models.py:40`)

O repositório aponta onde estaria esse conteúdo:

> “A fundamentacao INTEIRA (clausulas literais + parecer) esta em Sindicato.obs e em core/regua_cct.py”  
> “A CCT em PDF **nao esta no sistema** […] Enquanto nao tiver, o lastro e o texto em `obs`.”

(`CLAUDE.md:442-443`, `CLAUDE.md:455-456`)

A CCT identificada é “Vigilantes de Londrina/PR”, registro “MTE PR000251/2026”, vigência “01/02/2026-31/01/2028”. (`CLAUDE.md:430-432`)

Existe esta transcrição literal da cláusula 38-d:

> “no regime de 12x36 - ainda que cumprido em horario noturno -, a hora sera considerada normal de 60 (sessenta) minutos, garantido, sempre, o adicional noturno respectivo.”

O trecho trata de hora noturna no 12x36; não menciona feriado ou intermitente. (`app/core/regua_cct.py:127-138`)

**2. O que está escrito sobre intermitente, dobra e §6**

| Fonte | Trecho literal e alcance |
|---|---|
| Docstring de `MotorIntermitente` | “Intermitente-horista (Art. 452-A) — sem escala, sem marcos, sem jornada prevista. Apura SOMENTE horas cumpridas do par E/S (+ noturno legal). **Sem HE por estouro de jornada, sem feriado-100 automatico, sem atraso.**” (`app/ponto/motor_calculo_v2.py:3485-3488`) |
| Comentário do cálculo geral de feriado | “Feriado: Sumula 146 TST paga **TODAS as horas em dobro (nao so excedente)**.” (`app/ponto/motor_calculo_v2.py:2731-2733`) |
| Docstring da regra compartilhada | “o dia de feriado que dobra poe a jornada **INTEIRA em HE 100** […] e nao so o excedente.” (`app/ponto/calculador/regras.py:330-332`) |
| Régua CCT: piso declarado para 12x36 | `'feriado_12x36_em_dobra': False, # BUG 91: piso legal = hora simples (Art.59-A par.2)` (`app/core/regua_cct.py:44`) |
| Régua CCT: exceção cadastrada | “a dobra do feriado no 12x36 so volta por **CLAUSULA CADASTRADA**”; “Fora do 12x36 […] a Sumula 146 e o piso”. (`app/core/regua_cct.py:473-476`) |
| Comentário do modelo `Sindicato` | “o piso LEGAL do feriado trabalhado em 12x36 e HORA SIMPLES”; “a convencao coletiva PODE ser mais benefica e devolver a dobra; nenhuma das nossas diz isso hoje”. (`app/core/models.py:49-52`) |
| Comentário do espelho | “100% = descanso trabalhado (Lei 605/49 art.9 + Sum.146 TST) + excedente por CCT.” Em seguida: “Quantidade CRUA: majoracao e' da folha, nunca do ponto.” (`app/relatorios/pdf_espelho.py:764-766`) |

Há **cortes internos em sentidos diferentes**, com a inversão expressamente registrada:

- **13/10, aproximadamente 17h:** “intermitente que trabalha em feriado recebe a dobra (Lei 605 art.9 / Sumula 146 / art.452-A par.6) -- a docstring do MotorIntermitente se corrige para o codigo”. (`app/docs/PROMPTS.md:1204`)
- **13/10, aproximadamente 18h:** “intermitente em feriado recebe dobra (452-A §6, Lei 605 art.9, Sum.146)”; acrescenta: “**leitura passa pelo DP antes de virar lei**”. (`app/docs/PROMPTS.md:1213`)
- **14/10, 21:4x:** “**SDF/intermitente em feriado NAO recebe dobra (DP 14/10); o cartao nao afirma pagamento em dobro**”. A própria linha identifica esse corte como inversão da O264. (`app/docs/PROMPTS.md:1231`)
- **15/10, 11:5x:** “**intermitente segue pela docstring do MotorIntermitente (sem feriado-100 automatico)**”. O registro complementar diz “sem entrada nova no frozenset”. (`app/docs/PROMPTS.md:1239`)

O BACKLOG também registra a inversão:

> “A LEITURA DO DP CHEGOU E INVERTEU ESTA OBRA”; “a docstring do `MotorIntermitente` **nao se corrige**”; a cura dos dois zeros de 12/10 está “**CANCELADA** -- aqueles zeros sao o numero CERTO”.

(`app/docs/BACKLOG.md:1377`; orientação complementar em `app/docs/BACKLOG.md:1390`)

**Art. 452-A, §6:** os registros de 13/10 acima citam o dispositivo para sustentar a dobra, mas não transcrevem seu conteúdo. (`app/docs/PROMPTS.md:1204`, `app/docs/PROMPTS.md:1213`) Para a enumeração literal solicitada — remuneração, férias + 1/3, 13º, RSR e adicionais legais, pagos ao fim de cada período — **sem fonte no repo**. [nao verificado]

**3. Tratamento atual no código**

**Motor v2**

A limpeza está em [app/ponto/motor_calculo_v2.py:3491](/home/ronald/codex_ws/c7ea207a3f1e8ea911a0265b8e88991b4d91c1e4/app/ponto/motor_calculo_v2.py:3491), com estas atribuições:

```python
super().__init__(*args, **kwargs)
# neutraliza gatilhos de extra: jornada "infinita" e sem feriados
self.jornada_minutos = 24 * 60 * 365
self.feriados = set()
```

(`app/ponto/motor_calculo_v2.py:3491-3495`)

**As linhas atuais 903–905 não limpam o calendário:** elas consultam o conjunto e depois chamam o fallback. (`app/ponto/motor_calculo_v2.py:901-905`)

| Decisão | Código literal / valor produzido |
|---|---|
| Escolha do motor | `if ciclo == 'intermitente' or tipo_base == 'intermitente': cls = MotorIntermitente`. (`app/ponto/motor_calculo_v2.py:3563-3564`) |
| Flag de dobra pelo ciclo | `CICLOS_FERIADO_SIMPLES = frozenset({'12x36'})`; se o argumento não veio: `feriado_em_dobra = ciclo not in CICLOS_FERIADO_SIMPLES`. Para o ciclo literal `intermitente`, esse ramo produz **`True`**. (`app/ponto/motor_calculo_v2.py:3537`, `:3607-3608`) |
| Limpeza no intermitente | `self.feriados = set()`: conjunto vazio. (`app/ponto/motor_calculo_v2.py:3495`) |
| Reconhecimento do feriado | `if data in self.feriados: return True`; caso contrário: `return is_feriado(data)`. O conjunto vazio mantém essa segunda chamada. (`app/ponto/motor_calculo_v2.py:901-905`) |
| Fallback | `return eh_feriado(data, praca)`; em exceção, `return False`. O argumento `praca` tem padrão `None`. (`app/ponto/motor_calculo_v2.py:30-43`) |
| Alcance sem município | “SEM MUNICIPIO […] nem municipal nem estadual valem, so o nacional.” (`app/core/feriados.py:91-95`) |
| Decisão de dobra | `return bool(self.feriado_em_dobra) and self.eh_feriado(dia)`. (`app/ponto/motor_calculo_v2.py:879`) |

`MotorIntermitente` herda de `Motor12x36` e seu corpo contém somente o `__init__` mostrado acima. (`app/ponto/motor_calculo_v2.py:3483-3495`) O `calcular_periodo` herdado decide assim:

- **Sem saída:** marca `turno_aberto = True` e retorna; os campos de extra nascem em zero. (`app/ponto/motor_calculo_v2.py:2689-2693`, `:166-169`)
- **Com duração positiva e dobra reconhecida:** atribui a duração inteira a `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, e marca `hora_extra_100 = True`. (`app/ponto/motor_calculo_v2.py:2747-2754`)
- **Ramo seguinte:** `elif feriado and duracao > 0 and self.feriado_em_dobra` apenas acrescenta o alerta “PREVISTO na escala — horas SIMPLES”. O helper anterior já usa o mesmo flag e a consulta de feriado. (`app/ponto/motor_calculo_v2.py:2756-2760`, `:879`)
- **Demais situações:** calcula excedente contra `_jornada_do_dia(e)`; sem previsto, subtrai a própria duração, produzindo **extra zero**. Havendo extra, aplica a tolerância e reparte em até 120 minutos a 50%, com o restante a 100%. (`app/ponto/motor_calculo_v2.py:2773-2792`)

Assim, **o corpo não estabelece “HE100 feriado sempre zero para intermitente”**: o calendário injetado é apagado, mas permanecem o fallback e o flag derivado do ciclo. (`app/ponto/motor_calculo_v2.py:3495`, `:901-905`, `:3537`, `:3607-3608`)

Há teste que injeta `12/06/2026` como feriado e exige `minutos_extra_100 == 0` e `horas_extra == 0`, com as mensagens “Horista nao tem feriado-100 automatico” e “Horista nao tem HE por estouro de jornada”. (`app/ponto/tests/test_intermitente.py:54-68`)

**Calculador em `ponto/calculador/`**

Na alimentação de `regras.do_dia`, o calendário vem diretamente do argumento recebido:

```python
_feriados = set(feriados or ())
```

O flag de dobra vem do motor CCT; a identificação de intermitente é atualizada pelo vínculo do dia e enviada separadamente à regra. (`app/ponto/calculador/alimentacao.py:378-396`, `:479-485`, `:509-510`)

| Ramo | Valor produzido |
|---|---|
| Folga trabalhada do intermitente | `if _trab <= 0 or intermitente: return 0.0, 0.0`. (`app/ponto/calculador/regras.py:386-388`) |
| Dia sem trabalho previsto, mas intermitente | Não retorna pelo ramo dos demais regimes; grava `intermitente_segue_como_trabalho = True` e `horas_trabalhadas = trab / 60`. (`app/ponto/calculador/regras.py:736-740`) |
| Excedente sem previsto | `extra = trab - _limite if previsto_min else 0.0`: **zero** quando não há previsto. (`app/ponto/calculador/regras.py:767-771`) |
| Regra de feriado | Usa os mesmos métodos `MotorBase._feriado_dobra_no_dia` e `MotorBase.eh_feriado`. Retorna o trabalhado inteiro quando o juiz reconhece dobra; caso contrário, **`0.0`**. (`app/ponto/calculador/regras.py:287-295`, `:344-347`) |
| Dobra positiva | `horas_extras_100 = _dobra / 60`, `horas_extras_100_feriado = _dobra / 60`, `horas_extras_50 = 0.0`; retorna sem somar o excedente comum. (`app/ponto/calculador/regras.py:774-789`) |
| Calendário ou flag ausentes | Declara `horas_extras_100` e `horas_extras_100_feriado` em `SEM_ENTRADA`. (`app/ponto/calculador/regras.py:633-638`) |

Na releitura v3 de `periodo.py`, `reler` é descrito como “**LEITURA DE ENSAIO, nao caminho de producao**”. (`app/ponto/calculador/periodo.py:540-545`) Sem previsto, o excesso retorna com os campos numéricos zerados; a dobra é julgada depois, separadamente. (`app/ponto/calculador/periodo.py:367-371`, `:410-413`, `:819-831`) Quando positiva, grava toda a dobra em `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, com `minutos_extra_50 = 0.0`; caso contrário, grava os valores do excesso. (`app/ponto/calculador/periodo.py:833-847`)

A integração do fechamento chama `rubricas_por_dia` e grava `versao='oraculo'`; o comentário registra que essas linhas nascem ao lado de `versao='motor'` e que “Nenhum leitor le 'oraculo' hoje”. (`app/ponto/services/fechamento.py:1214-1217`, `:1233-1262`)

O relatório documenta um caso específico como “v2 paga 0 de HE100 feriado […] v3 paga em dobra”, mas o mantém como **pergunta**. Sua referência à limpeza em `903-905` não corresponde às linhas atuais do código. (`app/docs/RELATO.md:12`; `app/ponto/motor_calculo_v2.py:901-905`, `:3493-3495`)

**4. Leis L-NNN, cortes e casos E1**

| Registro | Trecho literal |
|---|---|
| **L-085** | “A JORNADA PERTENCE AO DIA DE INICIO, inclusive para domingo, feriado, contagem de semana e DSR”. (`app/docs/LEIS.md:117`) |
| **L-086** | Texto original: “domingo ou feriado so pagam dobra em dia NAO previsto”. A mesma linha registra: “a clausula de FERIADO do texto esta **REVOGADA** -- feriado previsto DOBRA conforme feriado_em_dobra do ciclo (12x36 nao dobra)”. (`app/docs/LEIS.md:118`) |
| Corte corretivo de 27/09 | “FERIADO NAO entra na L-086: segue a regra de feriado que ja existe no cadastro do ciclo”. (`app/ponto/motor_calculo_v2.py:862-865`) |
| **L-095** | Inclui “HE100 feriado” entre os valores lavrados uma vez pelo motor v2. (`app/docs/LEIS.md:113`) |
| Menção a intermitente na auditoria da **L-091** | “tipo_base ainda escolhe o motor […] 12x36 e intermitente pelo rotulo”. (`app/docs/LEIS.md:174`) |
| **C05** | Descrito como “lote 1 contra a v2, rubrica extra”, “v3 verde, v2 vermelha”. (`app/docs/RELATO.md:86`) |
| **C06** | “CCT Vigilantes x prorrogacao noturna”. (`app/docs/BACKLOG.md:782`) |
| **C16** | “intervalo 30 min batido: suprimido=30 pelo art.71 par.4 pos-2017?”. (`app/docs/BACKLOG.md:782`) |
| **C17** | “comercial 08-17 sem intervalo”; resposta registrada: “C17 HE 0 intra 60”. (`app/docs/RELATO.md:91`) |
| Corte **V3-E1-C17** | “L-031+L-032: sem intervalo batido indeniza so o suprimido, HE 0; C17/C16/C06 PENDENTE ate o DP/contador”. (`app/docs/CORTES.md:410`) |
| Corte **E1-VALIDACAO** | “lote1 todos menos C05/C17 (DP)”. (`app/docs/CORTES.md:411`) |
| Estado posteriormente relatado | “PENDENTE_DP C05/C06/C16/C17 em skip”. (`app/docs/RELATO.md:82`) |

**Arquivos `docs/E1-*` e texto integral desses quatro casos:** sem fonte no repo. [nao verificado] O relatório referencia `docs/E1-PERGUNTA-AO-DP-CONTADOR.md` expressamente como documento “da copia” e identifica outra cópia de trabalho para o E1. (`app/docs/RELATO.md:88`, `:92`)

**C05, C06, C16 ou C17 como caso específico de intermitente trabalhando em feriado:** sem fonte no repo. [nao verificado]

## 31/10 12:35 · sessao d4425bae0753 · - · - · origem=Ronald (tmux)

Fontes encontradas, todas dentro do snapshot:

- **`app/docs/LEIS.md:117` — L-085:** “**A JORNADA PERTENCE AO DIA DE INICIO**, inclusive para domingo, feriado, contagem de semana e DSR: hora depois da meia-noite herda o dia em que o plantao COMECOU, e nunca cria trabalho no dia seguinte”.
- **`app/docs/LEIS.md:118` — L-086**, texto original: “**PLANTAO PREVISTO NA ESCALA NUNCA E HE 100%**: domingo ou feriado so pagam dobra em dia NAO previsto (folga, ou feriado trabalhado fora da escala)”. **A mesma linha registra a revogação parcial:** “a clausula de FERIADO do texto esta REVOGADA -- feriado previsto DOBRA conforme feriado_em_dobra do ciclo (12x36 nao dobra), e ha selo que exige isso.”
- **`app/docs/LEIS.md:113` — L-095:** “o valor de um dia (normais, HE50, HE100, HE100 feriado, noturno, intra indenizada, atraso, saida antecipada, folga trabalhada, falta) e lavrado **uma vez**, pelo motor v2, no evento”.
- **`app/docs/LEIS.md:124` — L-091:** “o motor e a geometria saem do DESENHO […] `TipoEscala.tipo_base` e rotulo de tela e nao manda no calculo”. A auditoria **`app/docs/LEIS.md:174`** registra literalmente: “12x36 e intermitente pelo rotulo”. Não é regra específica de remuneração de feriado.

**Histórico explícito sobre INTERMITENTE-EM-FERIADO — indispensável preservar as mudanças:**

1. **`app/docs/PROMPTS.md:1204`**, corte 13/10 14:5x, ainda alternativa:  
   “corte Ronald: intermitente que trabalha em feriado (recebe dobra pela Lei 605 art.9 OU nao recebe, como o MotorIntermitente declara). Segue a fila; PAREI so em lei ou !”

   A mesma linha contém DELTA 13/10 16:0x:  
   “intermitente que trabalha em feriado **RECEBE** a dobra (Lei 605 art.9); o override morto do `MotorIntermitente` sai pela **L-111**.”

   E DELTA ~17:0x:  
   “intermitente que trabalha em feriado recebe a dobra (Lei 605 art.9 / Sumula 146 / art.452-A par.6) -- a docstring do MotorIntermitente se corrige para o codigo, e os 2 zeros de 12/10 (23,74 h, mesma empresa, mesmo ciclo, trabalha_em_feriado=True) sao bug de ESTRUTURA a curar na comp 10 com DIFF antes”.

2. **`app/docs/PROMPTS.md:1213`**, aval 13/10 ~18:1x:  
   “aval Ronald: pergunta 2 — intermitente em feriado recebe dobra (452-A §6, Lei 605 art.9, Sum.146); docstring errada, os 2 zeros de 12/10 sao bug; **leitura passa pelo DP antes de virar lei**.”

3. **`app/docs/PROMPTS.md:1231`**, corte 14/10 21:4x, rótulo do próprio documento “lei dele, e INVERTE a da O264”:  
   “corte Ronald: **SDF/intermitente em feriado NAO recebe dobra (DP 14/10); o cartao nao afirma pagamento em dobro**. Segue a fila; PAREI so em lei ou !”

   A mesma linha registra: “a O264 ganha DELTA (docstring NAO se corrige; a cura dos 2 zeros de 12/10 esta CANCELADA) e o resto migra para a O277”.

4. **`app/docs/PROMPTS.md:1239`**, corte 15/10 11:5x:  
   “corte Ronald O277: quem carrega o lado SDF e o cadastro da O275 ('feriado e dia de trabalho' no TipoEscala) -- tudo tem cadastro, nada de lista paralela; **intermitente segue pela docstring do MotorIntermitente (sem feriado-100 automatico)**. Ordem: O275 pousa, depois O277 le o flag. DIFF da dobra ja paga na 10 publicado antes; 09 exportada nao se toca.” [nao verificado]

   O comentário da mesma linha explicita: “o intermitente ficando pela docstring, **sem entrada nova no frozenset**”.

5. **`app/docs/BACKLOG.md:227-229`**, repetição do corte de 15/10:  
   “O277: quem carrega o lado SDF e o CADASTRO DA O275 […] ‘O275 pousa, depois O277 le o flag’ -- intermitente segue pela docstring do MotorIntermitente, **sem feriado-100 automatico**.”

6. **`app/docs/RELATO-ARQUIVO.md:878-880`**, histórico confirma a inversão:  
   “A SUA PERGUNTA DE 13/10 14:5x FOI RESPONDIDA PELO DP, E NA DIRECAO CONTRARIA […]”  
   “a leitura chegou e diz **NAO recebe** […] a cura dos **2 zeros de 12/10** […] esta **CANCELADA**: aqueles zeros eram o numero certo.”

7. **`app/docs/RELATO.md:12`**, pergunta atual, ainda pergunta:  
   “**lei: INTERMITENTE-EM-FERIADO (V3-RETOMA, 11:1x).** 1 colab intermitente convocado em feriado, 2 dias: 12,16 h (09) e 12,15 h (10). v2 paga 0 de HE100 feriado nesses dias (o motor intermitente limpa o calendario injetado, motor_calculo_v2.py:903-905), v3 paga em dobra. **PERGUNTA:** intermitente convocado em feriado recebe dobra (HE100) ou hora simples (CLT 452-A / CCT)? Ate a resposta: o caso fica de fora da cura a7088de8 (que nao o toca); a fila segue.”

**CORTES.md e casos E1:**

- **`app/docs/CORTES.md:410`**, 31/10 00:0x, V3-E1-C17:  
  “(b) L-031+L-032: **sem intervalo batido indeniza so o suprimido, HE 0; C17/C16/C06 PENDENTE ate o DP/contador**; DEGRAU_HE_100_MIN vira cadastro por convencao (fila)”.
- **`app/docs/CORTES.md:411`**, 31/10 00:4x, E1-VALIDACAO:  
  “**lote1 todos menos C05/C17 (DP)**; lote2 C02 RETEM (L-039), C12 RETEM por furo pos-encerramento (ADR D4, L-033), C13 nao retem (L-084), C14 -> L3-C01, C18 ausencia nao retem, C19 nao retem rotulo 'isento de ponto'; lote3 C02 escala_suspeita NAO retem (L-084, L-099/R2); 24x48 (L3 C11/C12) fora sem norma; sem tabela de traducao de vocabulario”.
- **`app/docs/BACKLOG.md:782`** descreve os assuntos: “C17 (sem intervalo: HE?), **C16 (intervalo 30 min batido: suprimido=30 pelo art.71 par.4 pos-2017?) e C06 (CCT Vigilantes x prorrogacao noturna)**; os 3 ficam PENDENTE no E1 ate a resposta”.
- **`app/docs/RELATO.md:82`** registra: “PENDENTE_DP **C05/C06/C16/C17** em skip com valores conferidos”.
- **`app/docs/RELATO.md:86`** descreve C05 apenas como “lote 1 contra a v2, **rubrica extra**”, “v3 verde, v2 vermelha”.
- **`app/docs/RELATO.md:91`** descreve C17 como “**comercial 08-17 sem intervalo**”, e registra resposta “C17 HE 0 intra 60”.
- **`app/docs/RELATO.md:88`** cita `docs/E1-PERGUNTA-AO-DP-CONTADOR.md` **“da copia”**.
- **`app/docs/RELATO.md:92`** cita a cópia `wt-v3e1-3010-2336` e o harness `ponto/tests/test_v3_e1_valor.py`.

**Limite importante:** não existem arquivos `docs/E1-*` nem `app/docs/E1-*` neste snapshot; `find .`, `rg --files --hidden` e listagens de `docs`/`app/docs` não os localizaram. Não existe aqui o texto integral dos casos C05/C06/C16/C17. Portanto **C05 ser feriado ou intermitente: sem fonte no repo**; os textos acima são as referências efetivamente disponíveis. Não extrapolar “rubrica extra” para feriado.

## 31/10 12:35 · sessao 15f636befbee · - · - · origem=Ronald (tmux)

Levantamento somente leitura.

**V3: `regras.do_dia` mantém o intermitente no caminho de trabalho comum e aplica a regra geral de feriado.**

- Identificação do regime: `app/ponto/calculador/alimentacao.py:406`:
  > `_interm = getattr(te, 'tipo_ciclo', None) == 'intermitente'`

  O vínculo do dia atualiza essa identificação em `app/ponto/calculador/alimentacao.py:479-485`:
  > `_interm_dia = getattr(_te_d, 'tipo_ciclo', None) == 'intermitente'`

  A chamada entrega calendário, flag e regime em `app/ponto/calculador/alimentacao.py:509-510`:
  > `feriados=_feriados, feriado_em_dobra=_fer_dobra,`
  > `intermitente=_interm_dia, dias_folga=_folga_dia,`

- O calendário passado à regra é a lista recebida pelo alimentador, `app/ponto/calculador/alimentacao.py:396`:
  > `_feriados = set(feriados or ())`

  O flag vem do motor obtido pela fábrica CCT, `app/ponto/calculador/alimentacao.py:378,391-394`:
  > `_mcct = get_motor_cct(c, ini, tipo_escala=te)`
  > `_cct = _cct_do_motor(_mcct) if _mcct is not None else None`
  > `_fer_dobra = None if _cct is None else _cct.feriado_em_dobra`

- Intermitente não recebe classificação de folga trabalhada. `app/ponto/calculador/regras.py:369-372`:
  > `**INTERMITENTE NUNCA E FOLGA TRABALHADA**`
  > `No intermitente a celula diz trabalha=False em TODO dia -- e o normal do regime, nao uma folga violada.`

  Retorno literal da função de folga, `app/ponto/calculador/regras.py:386-388`:
  > `_trab = float(trabalhado_min or 0)`
  > `if _trab <= 0 or intermitente:`
  > `    return 0.0, 0.0`

- Mesmo com `dia_de_trabalho=False`, o intermitente segue para o trabalho comum. `app/ponto/calculador/regras.py:657-660`:
  > `O INTERMITENTE CAI PARA O CAMINHO NORMAL [...] Entao aqui ele NAO retorna -- segue adiante.`
  > `if not intermitente and escala_certa is not None and dia is not None:`

  `app/ponto/calculador/regras.py:736-740`:
  > `if not intermitente:`
  > `    return Rubricas({}, _nd, det)`
  > `det['intermitente_segue_como_trabalho'] = True`
  > `val['horas_trabalhadas'] = trab / 60`

- Sem previsto, o excedente comum é zero, `app/ponto/calculador/regras.py:767-771`:
  > `_limite = float(previsto_min or 0) + min(suprimido, float(intervalo_declarado_min or 0))`
  > `extra = trab - _limite if previsto_min else 0.0`
  > `if extra <= TOLERANCIA_HE_MIN_DIA:`
  > `    extra = 0.0`

- **A dobra de feriado vem depois disso e não contém exceção `intermitente`.** `app/ponto/calculador/regras.py:774-789`:
  > `_dobra = 0.0`
  > `if feriados is not None and feriado_em_dobra is not None and dia is not None:`
  > `    _dobra = dobra_de_feriado_do_dia(dia, trab, feriados=feriados,`
  > `                                     feriado_em_dobra=feriado_em_dobra)`
  > `if _dobra > 0:`
  > `    val['horas_extras_100'] = _dobra / 60`
  > `    if 'horas_extras_100_feriado' not in _nd:`
  > `        val['horas_extras_100_feriado'] = _dobra / 60`
  > `    val['horas_extras_50'] = 0.0`
  > `    [...]`
  > `    return Rubricas(val, _nd, det)`

  Portanto as atribuições desse ramo são `horas_trabalhadas=trab/60`, `horas_extras_100=trab/60`, `horas_extras_100_feriado=trab/60`, `horas_extras_50=0`, quando o juiz devolve dobra; são rubricas/horas, não um valor monetário final. As atribuições estão em `app/ponto/calculador/regras.py:740,779-789`.

- Juiz compartilhado com v2: `app/ponto/calculador/regras.py:287-295`:
  > `_feriado_dobra_no_dia = MotorBase._feriado_dobra_no_dia`
  > `eh_feriado = MotorBase.eh_feriado`
  > `self.feriados = set(feriados or ())`
  > `self.feriado_em_dobra = bool(feriado_em_dobra)`

  `app/ponto/calculador/regras.py:344-347`:
  > `_ctx = _ContextoFeriado(feriados, feriado_em_dobra)`
  > `if float(trabalhado_min or 0) <= 0:`
  > `    return 0.0`
  > `return float(trabalhado_min) if _ctx._feriado_dobra_no_dia(dia) else 0.0`

  O juiz v2, `app/ponto/motor_calculo_v2.py:879`:
  > `return bool(self.feriado_em_dobra) and self.eh_feriado(dia)`

- Se faltarem calendário ou flag, HE100 e HE100 feriado ficam declaradas sem entrada, `app/ponto/calculador/regras.py:633-638`:
  > `if feriados is None or feriado_em_dobra is None:`
  > `    _nd['horas_extras_100'] = SEM_ENTRADA['horas_extras_100']`
  > `    [...]`
  > `    _nd['horas_extras_100_feriado'] = SEM_ENTRADA['horas_extras_100_feriado']`

  A causa literal está em `app/ponto/calculador/regras.py:529-531`:
  > `a regra existe (dobra_de_feriado_do_dia, o juiz do motor) e esta chamada nao recebeu o calendario de feriados nem o cadastro do ciclo -- passe feriados=/feriado_em_dobra=`

**Ponto crítico do v2 que repercute no v3: esvaziar `self.feriados` não desativa a consulta de feriado no banco.**

- Neste hash a classe fica em `app/ponto/motor_calculo_v2.py:3483-3495`; as linhas 903-905 são o corpo de `eh_feriado`, não a limpeza do calendário. Docstring, `app/ponto/motor_calculo_v2.py:3485-3488`:
  > `Intermitente-horista (Art. 452-A) — sem escala, sem marcos, sem jornada prevista. Apura SOMENTE horas cumpridas do par E/S (+ noturno legal).`
  > `Sem HE por estouro de jornada, sem feriado-100 automatico, sem atraso.`
  > `Herda de Motor12x36 apenas o pareamento E/S cruza-meia-noite.`

  Implementação, `app/ponto/motor_calculo_v2.py:3491-3495`:
  > `super().__init__(*args, **kwargs)`
  > `# neutraliza gatilhos de extra: jornada "infinita" e sem feriados`
  > `self.jornada_minutos = 24 * 60 * 365`
  > `self.feriados = set()`

  Consulta, `app/ponto/motor_calculo_v2.py:901-905`:
  > `"""Consulta feriado por contexto injetado; fallback mantém compatibilidade com banco."""`
  > `if data in self.feriados:`
  > `    return True`
  > `return is_feriado(data)`

- O único ciclo da lista simples é 12x36, `app/ponto/motor_calculo_v2.py:3537`:
  > `CICLOS_FERIADO_SIMPLES = frozenset({'12x36'})`

  Fábrica, `app/ponto/motor_calculo_v2.py:3607-3608`:
  > `if feriado_em_dobra is None:`
  > `    feriado_em_dobra = ciclo not in CICLOS_FERIADO_SIMPLES`

  Assim o ciclo literal `'intermitente'` recebe `True` nesse ramo, e a limpeza do conjunto não remove o fallback do banco; essas duas decisões estão em `app/ponto/motor_calculo_v2.py:3537,3607-3608,901-905`.

**V3 possui também uma releitura de ensaio por período.**

- `app/ponto/calculador/periodo.py:540-545`:
  > `def reler(resultado, regua_de, marcos=None, fora_por_lei=None, cct=None):`
  > `LEITURA DE ENSAIO, nao caminho de producao: serve a bateria e ao DIFF do passo 4.`

- Sem previsto, `app/ponto/calculador/periodo.py:410-413`:
  > `_prev = float(getattr(regua, 'previsto_min', 0) or 0)`
  > `if not _prev:`
  > `    return ExcessoV3(sem_previsto=True)`

  Os campos de excesso nascem zero, `app/ponto/calculador/periodo.py:367-371`:
  > `extra_min: float = 0.0`
  > `extra_50_min: float = 0.0`
  > `extra_100_min: float = 0.0`

- A dobra é julgada independentemente desse zero, `app/ponto/calculador/periodo.py:828-843`:
  > `_dobra = 0.0`
  > `if cct is not None:`
  > `    _dobra = dobra_de_feriado_do_dia(dia, _trab, feriados=cct.feriados,`
  > `                                     feriado_em_dobra=cct.feriado_em_dobra)`
  > `if _dobra > 0:`
  > `    _ult.minutos_extra = _dobra`
  > `    _ult.minutos_extra_50 = 0.0`
  > `    _ult.minutos_extra_100 = _dobra`
  > `    _ult.minutos_extra_100_feriado = _dobra`
  > `    _ult.hora_extra_100 = True`

**Integração atual: oráculo paralelo.**

- `app/ponto/services/fechamento.py:1233-1243` chama `rubricas_por_dia` a partir da competência `(2026, 10)` e passa `feriados=feriados`; `app/ponto/services/fechamento.py:1245-1263` lavra com `rubricas=_rub_o` e `versao='oraculo'`.
- Comentário literal, `app/ponto/services/fechamento.py:1214-1217`:
  > `a chave do DiaPago inclui versao, entao estas linhas NASCEM AO LADO das de versao='motor' [...] Nenhum leitor le 'oraculo' hoje -- colaboradores/services/calendario.py e folha/porta_export.py filtram versao='motor'`

**Teste localizado: intermitente comum, sem prova específica de feriado.** [nao verificado]

`app/ponto/tests/test_s5b_regra_pontualidade.py:347-354` passa 480 minutos, sem previsto, `dia_de_trabalho=False`, `intermitente=True`, e exige `horas_trabalhadas=8`, folga trabalhada zero e `intermitente_segue_como_trabalho=True`. A chamada não passa `feriados`/`feriado_em_dobra`; portanto esse teste não verifica a dobra do intermitente em feriado (`app/ponto/tests/test_s5b_regra_pontualidade.py:348-350`).

## 31/10 12:35 · sessao 25c8697c4180 · - · - · origem=Ronald (tmux)

Fontes encontradas, todas somente lidas:

- **Texto primário integral CLT art.452-A/§6 ou Lei605/49 art.9: sem fonte no repo.** As buscas por `adicionais legais`, `pagamento imediato`, `Ao final de cada`, `452-A`, `605` não localizaram a transcrição legal nem a lista de parcelas do §6. Há citações aos artigos em documentação interna, abaixo.
- **Conteúdo cadastrado de Sindicato.obs: sem fonte no repo.** Existe o campo `obs = models.TextField(blank=True)` em `app/core/models.py:40`, mas nenhum dump/fixture com seu valor foi localizado.
- `CLAUDE.md:442-443`: “A fundamentacao INTEIRA (clausulas literais + parecer) esta em Sindicato.obs e / em core/regua_cct.py; aqui fica so o que evita redescobrir do zero:”
- `CLAUDE.md:455-456`: “A CCT em PDF **nao esta no sistema** e o modelo Sindicato nao tem campo de / anexo. Enquanto nao tiver, o lastro e o texto em `obs`.”
- `CLAUDE.md:430-432` identifica a CCT: “CCT VIGILANTES LONDRINA”; “Sindicato #2 (Vigilantes de Londrina/PR), MTE PR000251/2026, vigencia / 01/02/2026-31/01/2028”. As cláusulas resumidas em `444-454` tratam hora noturna, adicional e janela 22h–05h; não mencionam intermitente/feriado.

Regra CCT e modelos:

- `app/core/regua_cct.py:44`: `'feriado_12x36_em_dobra': False,   # BUG 91: piso legal = hora simples (Art.59-A par.2)`
- `app/core/regua_cct.py:473-478`: “a dobra do feriado no 12x36 so volta por CLAUSULA CADASTRADA”; “Fora do 12x36 nao ha o que decidir aqui -- a Sumula 146 e o piso e o / `get_motor` ja resolve pelo ciclo; passar False para um 6x1 seria tirar / dele um direito que nenhuma CCT precisou conceder.” Ramo: `if _ciclo == '12x36' and r.get('feriado_12x36_em_dobra'):` → `kwargs.setdefault('feriado_em_dobra', True)`.
- `app/core/models.py:49-58`: “o piso LEGAL do feriado trabalhado / em 12x36 e HORA SIMPLES -- Art.59-A par.2 CLT, Sumula 444 cancelada”; “a / convencao coletiva PODE ser mais benefica e devolver a dobra; nenhuma das / nossas diz isso hoje”. Help text: “Fora isso, o 12x36 paga hora simples e a dobra vale so para 6x1/5x2/comercial.”
- `app/relatorios/pdf_espelho.py:764-766`: “50% = CLT art.59 §1 / CF art.7 XVI. 100% = descanso trabalhado / (Lei 605/49 art.9 + Sum.146 TST) + excedente por CCT. / Quantidade CRUA: majoracao e' da folha, nunca do ponto.”

Docstrings atuais:

- `app/ponto/motor_calculo_v2.py:3485-3488`: “Intermitente-horista (Art. 452-A) — sem escala, sem marcos, sem jornada / prevista. Apura SOMENTE horas cumpridas do par E/S (+ noturno legal). / Sem HE por estouro de jornada, sem feriado-100 automatico, sem atraso. / Herda de Motor12x36 apenas o pareamento E/S cruza-meia-noite.”
- Limpeza real em `app/ponto/motor_calculo_v2.py:3493-3495`: “neutraliza gatilhos de extra: jornada "infinita" e sem feriados”; `self.jornada_minutos = 24 * 60 * 365`; `self.feriados = set()`.
- **Atenção ao desencontro de linhas:** `app/ponto/motor_calculo_v2.py:901-905` contém `eh_feriado`, com `if data in self.feriados: return True; return is_feriado(data)`. A referência à limpeza nas linhas903–905 em `app/docs/RELATO.md:12` está desatualizada em relação ao arquivo atual.
- `app/ponto/motor_calculo_v2.py:862-865` corte literal: “FERIADO NAO entra / na L-086: segue a regra de feriado que ja existe no cadastro do ciclo (`feriado_em_dobra`; / 12x36 nao dobra pelo parecer do Art.59-A), com o dia do feriado sendo o dia de INICIO do / plantao.”
- `app/ponto/motor_calculo_v2.py:873-875`: “Plantao que comeca no feriado dobra; plantao que comeca na vespera e ENTRA pelo / feriado nao dobra, porque a hora pertence ao dia em que ele comecou.”

Histórico indispensável de cortes, embora fora de CORTES.md:

1. `app/docs/PROMPTS.md:1204`, delta13/10~17:0x: “intermitente que trabalha em feriado recebe a dobra (Lei 605 art.9 / Sumula 146 / art.452-A par.6) -- a docstring do MotorIntermitente se corrige para o codigo, e os 2 zeros de 12/10 (23,74 h, mesma empresa, mesmo ciclo, trabalha_em_feriado=True) sao bug de ESTRUTURA a curar na comp 10 com DIFF antes”.
2. `app/docs/PROMPTS.md:1213`, aval13/10~18:1x: “intermitente em feriado recebe dobra (452-A §6, Lei 605 art.9, Sum.146); docstring errada, os 2 zeros de 12/10 sao bug; leitura passa pelo DP antes de virar lei.”
3. **Corte posterior contrário**, `app/docs/PROMPTS.md:1231`,14/10~21:4x: “corte Ronald: SDF/intermitente em feriado NAO recebe dobra (DP 14/10); o cartao nao afirma pagamento em dobro.”
4. `app/docs/PROMPTS.md:1239`,15/10~11:5x: “intermitente segue pela docstring do MotorIntermitente (sem feriado-100 automatico). Ordem: O275 pousa, depois O277 le o flag. DIFF da dobra ja paga na 10 publicado antes; 09 exportada nao se toca.”
5. `app/docs/BACKLOG.md:1377`, O264 registra a inversão: “A LEITURA DO DP CHEGOU E INVERTEU ESTA OBRA”; “a docstring do `MotorIntermitente` **nao se corrige**”; “a cura dos **2 zeros de 12/10** [...] esta **CANCELADA** -- aqueles zeros sao o numero CERTO.”
6. `app/docs/BACKLOG.md:1390`, O277 registra a mesma inversão e orientação “intermitente pela docstring, **sem entrada nova no frozenset**”.
7. `app/docs/RELATO.md:12` reabre pergunta sob V3-RETOMA: “1 colab intermitente convocado em feriado, 2 dias: 12,16 h (09) e 12,15 h (10). v2 paga 0 de HE100 feriado nesses dias [...] v3 paga em dobra. PERGUNTA: intermitente convocado em feriado recebe dobra (HE100) ou hora simples (CLT 452-A / CCT)?”

As referências ao §6 acima **não transcrevem** as parcelas remuneração, férias+1/3,13º,RSR,adicionais legais. Para a enumeração literal pedida: **sem fonte no repo**.

## 31/10 12:35 · sessao 175c89d8e41c · GRADE-FOLGA · censo · origem=Code

1. **Chamadores no código de produção**

| Função chamada | Todos os chamadores encontrados fora dos testes | [nao verificado]
|---|---|
| `_aplicar_folgas_foto` | `montar_grade_prevista` — `app/escala/utils.py:768`; `montar_grade_prevista_periodo_por_turno` — `app/escala/utils.py:1787`. |
| `montar_grade_prevista` | `_montar_grade_prevista_periodo_legacy` — `app/escala/utils.py:993`. |
| `_montar_grade_prevista_periodo_legacy` | `montar_grade_prevista_periodo` — `app/escala/utils.py:2029`. |
| `montar_grade_prevista_periodo_por_turno` | `montar_grade_prevista_periodo` — `app/escala/utils.py:2027`; `_score_geometria` — `app/ponto/management/commands/detectar_par_relampago.py:136`. |
| `montar_grade_prevista_periodo` | `_julgar_colab_corpo` — `app/ponto/services/cartorio.py:931`; `Command.handle` — `app/ponto/management/commands/reconciliar_grade.py:118`; `grade_espelho_janela` — `app/escala/utils.py:2277`. |

Duas referências executáveis **inspecionam a assinatura, sem executar o builder**: `censo_col900` lê o default de `tol_min` do single; `tripwire_celulas` lê o mesmo parâmetro do dispatcher. — `app/ponto/management/commands/censo_col900.py:53`; `app/ponto/management/commands/tripwire_celulas.py:124`.

O dispatcher escolhe o builder por turno quando `GRADE_POR_TURNO` é verdadeiro; caso contrário, chama o legacy. — `app/escala/utils.py:2026`; `app/escala/utils.py:2029`.

2. **Quem lê o resultado de `montar_grade_prevista_periodo`**

O overlay escreve `tipo_dia='folga'`, substitui `celulas` por uma lista de `None` e apaga `orfas`. **Ele não zera os campos de minutos nesse trecho.** — `app/escala/utils.py:1117`; `app/escala/utils.py:1118`; `app/escala/utils.py:1119`; `app/escala/utils.py:1120`.

| Chamador direto | Lê `tipo_dia`? | Lê as células? Para decidir o quê? |
|---|---|---|
| Cartório | **Sim.** No ramo nunca-bateu, distingue cobertura integral de ausência/falta. O classificador também recebe o dia e lê seu tipo. — `app/ponto/services/cartorio.py:997`; `app/ponto/services/cartorio.py:1207`; `app/ponto/supra_juiz.py:199`. | **Sim.** Conta batidas casadas, calcula lâmpadas/missing, classifica o dia e lavra ata/veredito/status. — `app/ponto/services/cartorio.py:1170`; `app/ponto/services/cartorio.py:564`; `app/ponto/services/cartorio.py:575`; `app/ponto/services/cartorio.py:1557`. |
| `reconciliar_grade` | **Não lê diretamente o `tipo_dia` dos dias retornados.** O processamento extrai missing das células; sua leitura de “dia sem trabalho” vem separadamente da `CelulaDia`, pelo juiz. — `app/ponto/management/commands/reconciliar_grade.py:151`; `app/ponto/management/commands/reconciliar_grade.py:280`. | **Sim.** Missing vencidos compõem os furos; nenhum missing põe o dia em `avaliado_limpo`. Isso alimenta planos de criação e retratação de chamados. — `app/ponto/management/commands/reconciliar_grade.py:154`; `app/ponto/management/commands/reconciliar_grade.py:169`; `app/ponto/management/commands/reconciliar_grade.py:179`; `app/ponto/management/commands/reconciliar_grade.py:258`; `app/ponto/management/commands/reconciliar_grade.py:266`. |
| `grade_espelho_janela` | **Repassa o campo.** Chama o dispatcher, aplica clipping e retorna a grade. — `app/escala/utils.py:2277`; `app/escala/utils.py:2278`; `app/escala/utils.py:2279`. | O clipping substitui células por `None` e apaga órfãs em dias futuros ou anteriores ao piso; os consumidores abaixo fazem as demais leituras. — `app/escala/utils.py:942`; `app/escala/utils.py:944`; `app/escala/utils.py:946`; `app/escala/utils.py:948`. |

No cartório, essas leituras têm consequências além da apresentação:

- **Julgamento:** tipo e células participam das condições de cobrança, furo parcial e cobrança em dia sem trabalho. — `app/ponto/supra_juiz.py:281`; `app/ponto/supra_juiz.py:348`; `app/ponto/supra_juiz.py:379`; `app/ponto/supra_juiz.py:443`.
- **Ata:** `ata_do_dia` grava o tipo, os minutos e as lâmpadas derivadas das células; a lavra entrega essa ata à `CelulaDia`. — `app/ponto/services/cartorio.py:565`; `app/ponto/services/cartorio.py:567`; `app/ponto/services/cartorio.py:568`; `app/ponto/services/cartorio.py:1559`.
- **Chamados:** os códigos controlam emissão de furo inteiro/parcial; a ata deste julgamento também participa da reconciliação por marcos. — `app/ponto/services/cartorio.py:1489`; `app/ponto/services/cartorio.py:1508`; `app/ponto/services/cartorio.py:1531`; `app/ponto/services/cartorio.py:1544`; `app/ponto/services/cartorio.py:1182`; `app/ponto/services/cartorio.py:1281`.
- **Status:** `fatos_do_status` lê tipo e células da grade julgada, e esses fatos são passados à lavra. — `app/ponto/status_dia.py:143`; `app/ponto/status_dia.py:146`; `app/ponto/services/cartorio.py:1563`.

Os consumidores de `grade_espelho_janela` são:

| Consumidor | Leitura e destino |
|---|---|
| Espelho | Chama o builder em `_pela_grade`; usa esse caminho para o trecho anterior à competência ou como fallback de ata agregada. Lê tipo para `folga`, células não nulas para `previsto_canon` e células para realizado/missing. — `app/ponto/services/espelho.py:503`; `app/ponto/services/espelho.py:541`; `app/ponto/services/espelho.py:571`; `app/ponto/services/espelho.py:690`; `app/ponto/services/espelho.py:695`; `app/ponto/services/espelho.py:700`. |
| Tela do espelho | Recebe `folga`, `previsto_canon` e `realizado_grade`; o template usa esses campos para desenhar o dia. — `app/ponto/services/espelho.py:781`; `app/ponto/services/espelho.py:782`; `app/ponto/services/espelho.py:783`; `app/templates/ponto/espelho.html:166`; `app/templates/ponto/espelho.html:213`; `app/templates/ponto/espelho.html:226`. |
| Simulação | `grade_dia` retorna o dia do builder. `dia_perfeito` exige trabalho, ausência de órfãs/missing e compatibilidade dos tipos das células. Isso participa da recusa de materialização e da decisão de flip automático/humano. — `app/ponto/services/simular_dia.py:7`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/simular_dia.py:18`; `app/ponto/services/simular_dia.py:21`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/flip_auto.py:303`; `app/ponto/services/flip_auto.py:305`. |
| `mapa_divergencia` | Faz duas chamadas, inclui tipo e status/missing das células nas assinaturas e compara divergências. — `app/ponto/management/commands/mapa_divergencia.py:104`; `app/ponto/management/commands/mapa_divergencia.py:105`; `app/ponto/management/commands/mapa_divergencia.py:120`; `app/ponto/management/commands/mapa_divergencia.py:131`; `app/ponto/management/commands/mapa_divergencia.py:208`. |
| PDF do espelho | Nas chamadas próprias, lê órfãs e células marcadas como isenção; não decide diretamente pelo `tipo_dia`. As órfãs afetam a identificação de entrada espúria/turno aberto e a marca de batida órfã. Os dias desenhados vêm do serviço de espelho. — `app/relatorios/pdf_espelho.py:471`; `app/relatorios/pdf_espelho.py:509`; `app/relatorios/pdf_espelho.py:539`; `app/relatorios/pdf_espelho.py:548`; `app/relatorios/pdf_espelho.py:551`; `app/relatorios/pdf_espelho.py:594`; `app/relatorios/pdf_espelho.py:598`; `app/relatorios/pdf_espelho.py:605`. |

**Folha/dinheiro:** `grade_do_fechamento` consulta `grade_da_celula`, que lê a ata gravada. A ligação com o dispatcher passa pela ata lavrada pelo cartório; o caminho financeiro está detalhado no item 4. — `app/folha/export.py:324`; `app/folha/export.py:327`; `app/escala/services/leitor_celula.py:331`; `app/ponto/services/cartorio.py:1559`.

Há também leitura das células **dentro do builder por turno, depois do overlay**: ele monta atas em memória com suas lâmpadas, entrega essas cópias ao juiz de turnos e obtém o realizado final desse juiz. — `app/escala/utils.py:1832`; `app/escala/utils.py:1838`; `app/escala/utils.py:1985`; `app/escala/utils.py:1998`.

3. **Onde se decide se uma `FolgaDia` vale**

Na tabela, “outro vínculo” significa diferente do vínculo escolhido para aquele dia; o resultado indicado é o tratamento da foto naquele caminho, sujeito às demais condições do próprio código.

| Sítio | Calendário em outro vínculo | Manual em outro vínculo do mesmo colaborador |
|---|---|---|
| `_aplicar_folgas_foto` | Busca pelo `esc` recebido, sem verificar origem ou vigência. Se receber o vínculo da foto, aplica-a mesmo sem conferir se ele é o vínculo daquele dia. — `app/escala/utils.py:1109`; `app/escala/utils.py:1116`. | Mesmo comportamento: não distingue origem. — `app/escala/utils.py:1109`; `app/escala/utils.py:1118`. |
| Integração do overlay no builder por turno | **Pode aplicar a foto ao dia de outro vínculo:** basta o vínculo da foto responder por algum dia da janela; o overlay recebe todo `dias_out`. — `app/escala/utils.py:1260`; `app/escala/utils.py:1261`; `app/escala/utils.py:1785`; `app/escala/utils.py:1787`. | Exatamente o mesmo alcance. — `app/escala/utils.py:1109`; `app/escala/utils.py:1787`. |
| Builder legacy | Escolhe vínculos por dia, constrói segmentos, chama o single por segmento e conserva apenas os dias daquele segmento. — `app/escala/utils.py:970`; `app/escala/utils.py:975`; `app/escala/utils.py:993`; `app/escala/utils.py:997`. | Mesmo tratamento; o overlay do single filtra pelo vínculo recebido. — `app/escala/utils.py:768`; `app/escala/utils.py:1110`. |
| `EscalaColaborador.eh_dia_trabalho_calculado` | Consulta somente a foto do `self`, por `folgas[self.pk]` ou filtro equivalente; foto de outro vínculo não entra. Não distingue origem nem testa vigência do `self` nesse cálculo. — `app/escala/models.py:1252`; `app/escala/models.py:1257`; `app/escala/models.py:1281`. | Também não entra enquanto estiver presa ao outro vínculo. — `app/escala/models.py:1252`; `app/escala/models.py:1281`. |
| `EscalaColaborador.eh_dia_trabalho` | Havendo célula do colaborador/data, retorna `cel.trabalha`, sem consultar a foto; só sem célula cai no cálculo acima. — `app/escala/models.py:1067`; `app/escala/models.py:1071`; `app/escala/models.py:1073`. | Mesmo comportamento. — `app/escala/models.py:1072`; `app/escala/models.py:1073`. |
| `fatos_do_periodo` / `fatos_do_dia` | Carrega fotos por pessoa/data, mas **descarta a selecionada** se tiver origem de calendário e seu vínculo diferir do vínculo diário, inclusive quando não há vínculo diário. — `app/ponto/precedencia.py:236`; `app/ponto/precedencia.py:307`; `app/ponto/precedencia.py:313`; `app/ponto/precedencia.py:315`. | **Conserva a foto**, pois a guarda exige origem preenchida; ela entra como fato de faixa `folga`, sujeito à precedência das demais faixas. — `app/ponto/precedencia.py:313`; `app/ponto/precedencia.py:337`; `app/ponto/precedencia.py:338`. |
| Alimentação especial do calendário | Quando `_cobre=True`, carrega apenas fotos do `ec` fornecido e passa `{data: True}` à precedência. Foto de outro vínculo não entra nesse mapa; origem e id não são preservados nele. Quando `_cobre=False`, deixa a precedência carregar normalmente. — `app/colaboradores/services/calendario.py:593`; `app/colaboradores/services/calendario.py:602`; `app/colaboradores/services/calendario.py:606`; `app/colaboradores/services/calendario.py:607`. | Também fica fora no caminho `_cobre=True`; no outro caminho vale a carga por pessoa descrita acima. — `app/colaboradores/services/calendario.py:607`; `app/colaboradores/services/calendario.py:608`. |
| `_folga_avulsa` → `veto_de_lancamento` | **Conta por colaborador/data**, sem filtro de vínculo, origem ou vigência. Pode vetar nova folga e outros lançamentos; férias atravessa a folga. — `app/ponto/precedencia.py:165`; `app/ponto/precedencia.py:449`; `app/ponto/precedencia.py:458`; `app/ponto/precedencia.py:462`. | Mesmo tratamento: conta e pode vetar. — `app/ponto/precedencia.py:166`; `app/ponto/precedencia.py:459`; `app/ponto/precedencia.py:462`. |
| `declarar_dia`: `_folga_do_dia`, `o_dia_ja_e_folga`, `_recusa_do_dia` | A consulta local usa o vínculo diário e não encontra foto de outro vínculo. Se própria, calendário recusa com “edite o calendário”. Depois existe o veto por pessoa, que pode encontrar a foto do outro vínculo. — `app/ponto/services/declarar_dia.py:58`; `app/ponto/services/declarar_dia.py:70`; `app/ponto/services/declarar_dia.py:91`; `app/ponto/services/declarar_dia.py:109`; `app/ponto/services/declarar_dia.py:114`. | Não conta localmente como “já é folga” se estiver em outro vínculo; ainda pode ser encontrada pelo veto subsequente por pessoa. — `app/ponto/services/declarar_dia.py:70`; `app/ponto/services/declarar_dia.py:111`; `app/ponto/services/declarar_dia.py:114`. |

Em `fatos_do_periodo`, havendo várias fotos na mesma data, fica a primeira por `pk`. Se essa primeira for calendário de outro vínculo e for descartada, o código não procura a próxima foto nessa seleção. — `app/ponto/precedencia.py:237`; `app/ponto/precedencia.py:238`; `app/ponto/precedencia.py:298`; `app/ponto/precedencia.py:313`.

No cálculo do vínculo, uma foto no mês impede o ramo de âncora; foto no próprio dia produz `False`. Fora da foto, usa o ciclo ou, no ramo de ciclo desconhecido, a paridade da foto mais próxima daquele vínculo. — `app/escala/models.py:1260`; `app/escala/models.py:1267`; `app/escala/models.py:1288`; `app/escala/models.py:1290`; `app/escala/models.py:1327`; `app/escala/models.py:1335`.

Os sítios relacionados à vigência e ao dono da foto são:

| Sítio | Regra |
|---|---|
| `folgas_do_periodo` | Carrega por ids de vínculo, amplia o intervalo para meses inteiros e devolve datas separadas por vínculo; não conserva origem nem aplica vigência. Ambas as origens permanecem no conjunto do vínculo original. — `app/escala/alimentacao.py:60`; `app/escala/alimentacao.py:66`; `app/escala/alimentacao.py:70`. |
| `vinculo_cobre_a_data` | Exige início até o dia, fim nulo ou posterior/igual e vigência declarada: ativo ou fim preenchido. — `app/escala/alimentacao.py:164`; `app/escala/alimentacao.py:182`; `app/escala/alimentacao.py:184`. |
| `vinculo_do_dia` | Com geradora na célula, procura esse vínculo e confere limites de início/fim; sem geradora, escolhe entre os que cobrem o dia por máximo `(ativa, data_inicio)`. — `app/escala/alimentacao.py:237`; `app/escala/alimentacao.py:247`; `app/escala/alimentacao.py:250`; `app/escala/alimentacao.py:253`; `app/escala/alimentacao.py:254`. |
| `reparentar_folga` | Calendário retorna `'calendario'`, sem migrar. Manual permanece se o vínculo original ainda cobre a data, mesmo que outro seja escolhido para o dia. Se não cobre, procura o vínculo diário; sem alvo ou com foto no alvo, não migra. Com alvo disponível e aplicação habilitada, muda o vínculo. — `app/escala/folgas.py:405`; `app/escala/folgas.py:408`; `app/escala/folgas.py:414`; `app/escala/folgas.py:418`; `app/escala/folgas.py:420`; `app/escala/folgas.py:435`. |
| Reparentamento em lote/comando/signal | O lote e o comando selecionam somente manuais cujo vínculo não cobre o dia. O signal aciona esse processo na criação/mudança de vigência. — `app/escala/folgas.py:453`; `app/escala/folgas.py:457`; `app/escala/management/commands/reparentar_folgas.py:28`; `app/escala/management/commands/reparentar_folgas.py:34`; `app/escala/signals.py:185`; `app/escala/signals.py:191`. |

Outros leitores diretos, de tela, edição ou diagnóstico:

| Sítio | Tratamento das duas origens e de outro vínculo |
|---|---|
| Plano de folgas, `_grid_ctx` | Indexa por vínculo/dia; foto gravada aparece antes do cálculo. Ambas as origens presas em outro vínculo ficam na linha desse outro vínculo, se ele estiver entre os ativos carregados. — `app/escala/views_plano_folgas.py:55`; `app/escala/views_plano_folgas.py:63`; `app/escala/views_plano_folgas.py:70`; `app/escala/views_plano_folgas.py:72`. |
| Toggle do plano | Consulta vínculo ativo + data; foto de outro vínculo não é encontrada localmente. Calendário próprio bloqueia edição. — `app/escala/views_plano_folgas.py:109`; `app/escala/views_plano_folgas.py:112`; `app/escala/views_plano_folgas.py:113`. |
| `lancar_folga` / `_foto_do_dia` | Consultam vínculo + data; calendário próprio bloqueia alteração. Outra foto fica fora da consulta local, mas a criação chama o veto por pessoa. — `app/escala/folgas.py:254`; `app/escala/folgas.py:255`; `app/escala/folgas.py:258`; `app/escala/folgas.py:274`; `app/escala/folgas.py:275`; `app/escala/folgas.py:283`. |
| `defeito_folga_nao_definida` | Em 6x1 sem folga semanal, qualquer foto do próprio vínculo no mês afasta o defeito; ambas as origens em outro vínculo não contam. — `app/escala/models.py:1218`; `app/escala/models.py:1220`. |
| Diagnóstico de escala / relatório de furos | Contam ou verificam foto do vínculo no mês, sem distinguir origem; foto de outro vínculo não satisfaz a consulta. — `app/ponto/services/diagnostico_escala.py:102`; `app/relatorios/furos.py:112`. |
| `fase_conflitante` | Lê fotos de cada vínculo ativo, sem filtro de origem/vigência por data; compara foto exata ou paridade da mais próxima com a âncora. Fotos de outro vínculo não entram no conjunto daquele vínculo. — `app/escala/services/fase_conflitante.py:55`; `app/escala/services/fase_conflitante.py:57`; `app/escala/services/fase_conflitante.py:71`; `app/escala/services/fase_conflitante.py:74`; `app/escala/services/fase_conflitante.py:80`. |
| `rejulgar_ausencia_em_folga` | Seleciona pares pessoa/data por foto da empresa/janela, sem distinguir origem ou vínculo diário. Portanto ambas as fotos em outro vínculo podem selecionar o dia para o cruzamento com ausência e eventual rejulgamento. — `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:55`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:60`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:63`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:72`. |
| Auditoria C5 | Considera a foto se o vínculo original cobre a data **ou** outro candidato não resulta em `'nenhum'`; não distingue origem. Assim ambas em vínculo diferente podem contar como contradição se a célula trabalha e a competência não foi exportada. — `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1535`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1542`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1546`. |
| `vinculo_que_cobre`, usado pelo C5 | Filtra vigência, exclui o vínculo indicado e pode responder único, nenhum ou ambíguo. O C5 só exclui `'nenhum'`, aceitando também `'ambiguo'`. — `app/escala/folgas.py:369`; `app/escala/folgas.py:383`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1536`. |
| `divergencia_foto_x_calendario` | Compara fotos do vínculo/mês e separa calendário/manual para calcular falta/sobra; fotos em outro vínculo não entram. — `app/escala/folgas.py:101`; `app/escala/folgas.py:104`; `app/escala/folgas.py:107`; `app/escala/folgas.py:108`. |
| `gerar_foto_folgas` | No vínculo/mês recebido, preserva manuais, remove fotos de calendário e recria as datas do calendário sem sobrepor manuais. Fotos em outro vínculo ficam fora desses filtros. — `app/escala/folgas.py:165`; `app/escala/folgas.py:169`; `app/escala/folgas.py:174`; `app/escala/folgas.py:175`. |

4. **Quem lê `ata['tipo_dia']` da `CelulaDia`**

As leituras diretas da chave são:

| Leitor direto | Para quê |
|---|---|
| `grade_da_celula` | Usa `ata.get('tipo_dia')` como tipo do dia; somente se ausente/falso usa `cel.trabalha` como fallback. — `app/escala/services/leitor_celula.py:331`; `app/escala/services/leitor_celula.py:344`; `app/escala/services/leitor_celula.py:345`; `app/escala/services/leitor_celula.py:359`. |
| `dia_sem_trabalho` | Retorna verdadeiro se `cel.trabalha is False` **ou** se a ata tem tipo preenchido diferente de `'trabalho'`. — `app/chamados/juizes.py:407`; `app/chamados/juizes.py:408`. |
| `medir_ata_fantasma` | Seleciona tipo folga sem lâmpadas; conta atas com `n_celulas` preenchido e separa as exportadas. — `app/ponto/management/commands/tripwire_celulas.py:342`; `app/ponto/management/commands/tripwire_celulas.py:344`; `app/ponto/management/commands/

[truncado em 20000 de 26092 caracteres]

## 31/10 12:32 · sessao 82a08d920812 · - · - · origem=Ronald (tmux)

Censo do item 4 — leituras diretas da chave na ATA:

| Leitor | Uso |
|---|---|
| `app/escala/services/leitor_celula.py:344` | `grade_da_celula` usa `ata.get('tipo_dia')` como `dias[].tipo_dia`; só quando ausente/falso usa `cel.trabalha` como fallback (`:345`). Consulta `CelulaDia` em `:325`, obtém ATA em `:331`, devolve tipo em `:359`. |
| `app/chamados/juizes.py:407` | `dia_sem_trabalho`: verdadeiro se `cel.trabalha is False` **ou** tipo presente na ATA diferente de `trabalho` (`:408`). |
| `app/ponto/management/commands/tripwire_celulas.py:341` | `medir_ata_fantasma` seleciona ATA `tipo_dia == folga` sem lâmpadas (`:342`), conta `n_celulas > 0` como fantasma e separa competências exportadas (`:344-349`). |

Consumidores indiretos do tipo devolvido por `grade_da_celula`:

| Consumidor | Decisão sustentada pelo código |
|---|---|
| `app/escala/services/leitor_celula.py:384` | Só dias de `ausencia` recebem apuração de cobertura parcial (`:389-397`). |
| `app/escala/services/leitor_celula.py:366` → `:188` | Isenção chama `aplicar_isencao_no_dia`; este usa tipo `trabalho` para marcar `dia['isencao']` (`app/escala/utils.py:850-851`). |
| `app/folha/export.py:324-327` | `grade_do_fechamento` lê a ATA via `grade_da_celula`, para janela da competência. |
| `app/ponto/services/fechamento.py:981-982` | Obtém a grade da ATA pelo leitor acima. Distingue ausência de célula dentro do vínculo (`:1011-1013`), conta previsto somente de `trabalho`/`ausencia` (`:1014-1018`), deriva mapas de abono/previsto/realizado (`:1028-1032`), grava previsto/dias (`:1051-1054`). |
| `app/ponto/services/dia_pago.py:385-399` | `por_dia_da_grade`: universo `trabalho`/`ausencia`; ausência abona `previsto - min(realizado, previsto)`; previsto e realizado são mapas por dia. É chamado pelo fechamento (`app/ponto/services/fechamento.py:1028-1032`) e por `retratar_exportada` (`app/ponto/services/dia_pago.py:462`), que escreve os três campos de `DiaPago` (`:474-481`). |
| `app/folha/export.py:1069-1086` | Fallback de prontidão: filtra `trabalho`/`ausencia`, soma previsto, abona ausência e soma realizado com teto diário somente de trabalho. Caminho de snapshot lê os campos materializados em `:1019-1024`, `:1050-1053`. |
| `app/folha/causas.py:40-42` | `classificar_causa` conta trabalho e furos; folga com órfãs pode classificar `dias_trocados` (`:100-106`). Chamado pelo fechamento com a grade da ATA (`app/ponto/services/fechamento.py:1045-1048`). |
| `app/folha/porta_export.py:133-145` | Auditoria `_gravado_x_grade`: deriva os mesmos mapas da ATA e compara a soma com lavratura e escalar do fechamento. |
| `app/ponto/services/espelho.py:530` | Lê e remapeia ATA; tipo `folga` vira booleano em `:690`, devolvido em `:781`. Template imprime “Folga” (`app/templates/ponto/espelho.html:166`). |
| `app/relatorios/furos.py:141-153` | Relatório ignora `fora_vinculo`/`sem_celula`; qualquer tipo diferente de trabalho vira estado `folga`, sem procurar furos. |
| `app/relatorios/furos.py:217-225` | `dias_100_em_branco` só examina trabalho sem `sem_ancora`, para retornar dias totalmente furados. |
| `app/relatorios/furos.py:236-250` | `furos_por_dia` só examina trabalho sem `sem_ancora`, retorna horários faltantes. |
| `app/inteligencia/calculadores.py:39-67` | Métrica de preenchimento só conta trabalho, com julgamento das células. |
| `app/inteligencia/calculadores.py:136-153` | Métrica de marcas previstas/batidas só conta trabalho. |
| `app/inteligencia/calculadores.py:324-325` | Passa dias da ATA a `template_suspeito`, que só usa trabalho para calcular desvio (`app/ponto/propositor.py:209-216`). |
| `app/ponto/management/commands/propor_reorganizacao.py:65-71` | Passa ATA a `template_suspeito` e `dia_degenerado`; este recusa tipos diferentes de trabalho (`app/ponto/propositor.py:36-37`). |
| `app/escala/services/furos_diarios.py:36-47` | Só trabalho entra na apuração de previstos, furos e órfãs. |
| `app/ponto/services/bordas_realizado.py:68-88` | Só trabalho entra nos contadores de divergência ATA × juiz do realizado e bordas. |
| `app/ponto/worklist.py:18-26` | Tipo diferente de trabalho retorna gap desconhecido com motivo “dia de … pela celula”; trabalho examina lâmpadas apagadas. |
| `app/chamados/services/disputa_emissao.py:66-70` | `_data_furo_da_grade` só confirma data do turno se o tipo for trabalho. |
| `app/chamados/services/declaracao_texto.py:82-98` | `_marcos_faltantes` distingue sem célula/fora do vínculo e só cobra marcos em trabalho. |
| `app/chamados/services/materializacao.py:217-225` | `_marcos_da_regua` retorna vazio em sem célula/fora do vínculo; demais tipos devolvem horas dos marcos. |
| `app/ponto/management/commands/supra_juiz.py:116` e `app/ponto/management/commands/emitir_furo_retroativo.py:59,99-109` | Leem ATA e a passam ao classificador. `app/ponto/supra_juiz.py:199` lê o tipo; usa trabalho para realizado zero (`:258`), trabalho ou ausência parcial para cobrança (`:281`), tipo sem trabalho para cobrança indevida (`:443-447`), trabalho para realizado inflado (`:448-451`) e órfãs (`:463-472`). |
| `app/ponto/management/commands/mapa_divergencia.py:98,108` | Usa grade da ATA como lado folha; inclui tipo nas assinaturas (`:131,143,155`) e conta divergência por tipo (`:228-229`). |

Consumidores do juiz direto `dia_sem_trabalho`:

- `_celula_furada` consulta esse juiz e, em dia sem trabalho, consulta a véspera antes de retornar falso: `app/chamados/juizes.py:443-449`.
- A premissa do chamado considera o dia regularizado quando esse juiz diz sem trabalho, exceto módulos `turno_aberto*`: `app/chamados/models.py:1227-1233`.
- `reconciliar_grade` usa esse juiz para pôr dias indefinidos no conjunto `mortos`: `app/ponto/management/commands/reconciliar_grade.py:280-283`; retratação exige todas as datas mortas (`:292`).

Separação importante para a consolidação: `status_dia.fatos_do_status` lê o dict da **grade do julgamento**, explicitamente `montar_grade_prevista_periodo`, não o leitor da ATA (`app/ponto/status_dia.py:124-149`). `simular_dia.grade_dia` usa `grade_espelho_janela` (`app/ponto/services/simular_dia.py:6-8`), portanto `simular_dia:12` e `veredito_celula:221` não devem ser atribuídos automaticamente à ATA.

Não encontrei leituras adicionais de `tipo_dia` por SQL/`ata__tipo_dia`, JavaScript ou templates literais; busca estática, sem prova de ausência dinâmica. [não verificado]

## 31/10 12:31 · sessao 42ef32b09afc · - · - · origem=Ronald (tmux)

Inventário do item 3, sem alterações.

**Decisão operacional sobre o dia**

| Sítio | Critério e efeito | Calendário em outro vínculo | Manual em outro vínculo |
|---|---|---|---|
| `app/escala/utils.py:1101` `_aplicar_folgas_foto` | Busca `FolgaDia` por **vínculo passado + intervalo**, sem testar origem/vigência; muda `tipo_dia='folga'`, apaga células e órfãs (`:1109-1120`). | Ignorada se outro que o `esc` passado; aceita se esse vínculo for passado, mesmo fora da vigência naquele dia (`:1109-1119`). | Exatamente igual: nenhuma distinção por origem (`:1109-1119`). |
| `app/escala/utils.py:1785` integração do overlay no builder por turno | Aplica o overlay sobre **todo `dias_out`** para cada vínculo que responde por **algum dia** da janela; o conjunto vem do mapa diário (`:1254-1261`, `:1785-1788`). | Pode virar folga em dia de outro vínculo se o vínculo da foto responder por qualquer outro dia da janela; não existe guarda por vínculo daquele dia dentro do overlay (`:1261`, `:1785-1787`, `:1114-1119`). | Mesmo comportamento (`:1109-1119`, `:1785-1787`). |
| `app/escala/models.py:1223` `EscalaColaborador.eh_dia_trabalho_calculado` | Foto pertence ao **self**: pré-carga usa `folgas[self.pk]`; consultas usam `escala_colaborador=self`. Data na foto faz `base=False` (`:1251-1259`, `:1280-1288`). Foto do mês vence a âncora; fora da foto usa ciclo ou, se ciclo desconhecido, paridade da foto mais próxima desse mesmo vínculo (`:1260-1267`, `:1290-1292`, `:1322-1335`). | Ignora foto de outro vínculo; não testa origem nem vigência do `self` dentro desta função (`:1251-1259`, `:1280-1281`, `:1327-1332`). | Ignora também enquanto presa em outro vínculo (`mesmas linhas`). |
| `app/escala/models.py:1048` `EscalaColaborador.eh_dia_trabalho` | Se há célula do colaborador/dia, retorna diretamente `cel.trabalha`; só sem célula consulta cálculo acima (`:1066-1073`). | Não consulta foto quando há célula; sem célula vale a restrição ao `self` do cálculo (`:1071-1073`, `:1257-1259`). | Igual (`mesmas linhas`). |
| `app/ponto/precedencia.py:212` `fatos_do_periodo` / `fatos_do_dia` | Por padrão carrega fotos de **todos os vínculos do colaborador**, por data; em colisão fica a primeira por pk (`:234-238`). Escolhe vínculo diário via `vinculo_do_dia` (`:307`), descarta foto de calendário cujo vínculo diverge (`:313-315`), mantém demais e acrescenta faixa folga (`:337-338`). `fatos_do_dia` delega ao período (`:209`). | Descarta **a foto selecionada** quando `calendario_origem_id != None` e `escala_colaborador_id != ec.pk`; também descarta se `ec=None` (`:313-315`). Como só a primeira por pk foi selecionada, não procura segunda foto depois do descarte (`:236-238`, `:298`, `:313-315`). | Mantém mesmo presa a outro vínculo ou sem vínculo diário, pois a guarda exige calendário preenchido; entra entre fatos presentes como folga, sujeita às outras faixas de precedência (`:313-315`, `:332-350`). |
| `app/colaboradores/services/calendario.py:592` alimentação especial de `fatos_do_periodo` | Carrega fotos apenas do `ec` já carregado. Se ele cobre ambas pontas da janela, passa `vinculos=[ec]`, foto desse ec e `folgas_lancadas={data: True}`; caso contrário passa `None` e deixa a precedência carregar (`:592-608`). | Foto de outro ec não entra no mapa quando `_cobre=True`; o mapa de `True` também não conserva origem/id para a guarda da precedência (`:593-608`, `app/ponto/precedencia.py:313`). | Também não entra enquanto presa em outro ec no caminho `_cobre=True`; no outro caminho vale carga por pessoa da precedência (`:607-608`, `app/ponto/precedencia.py:234-238`). |
| `app/ponto/precedencia.py:161` `_folga_avulsa`, usada por `veto_de_lancamento` | Busca por **colaborador + data**, sem vínculo/origem/vigência (`:165-166`). O veto recusa nova folga e outros lançamentos perante foto existente; férias atravessa a folga (`:449-466`). | Conta e pode vetar: nenhuma guarda para calendário de outro vínculo neste caminho (`:165-166`, `:458-466`). | Conta e pode vetar, igualmente (`mesmas linhas`). |
| `app/ponto/services/declarar_dia.py:61` `_folga_do_dia`, `o_dia_ja_e_folga`, `_recusa_do_dia` | Resolve vínculo diário por `escala_vigente`, depois busca foto **desse ec e data** (`:56-70`, `:90-91`, `:105-108`). Foto de calendário recusa com “edite o calendário”; manual recusa com “já é folga” (`:109-112`). | Foto de outro ec não é encontrada localmente (`:70`). Ainda pode ser apanhada pelo veto subsequente, que consulta por pessoa (`:113-116`, `app/ponto/precedencia.py:458-466`). | Mesma diferença: não conta como “já é folga” local se presa a outro ec, mas veto subsequente consulta pessoa (`mesmas linhas`). |

**Vigência e mudança de dono da foto**

- `app/escala/alimentacao.py:49-70` `folgas_do_periodo` carrega datas por ids dos vínculos pedidos, arredondando a janela para meses inteiros; conserva a separação por vínculo e **não conserva origem**, nem aplica vigência da foto (`:60-70`). Foto manual e calendário em outro vínculo ficam no balde desse outro vínculo (`:66-70`).
- `app/escala/alimentacao.py:173-184` `vinculo_cobre_a_data` exige início não nulo e anterior/igual ao dia, fim nulo ou posterior/igual e vigência declarada; esta significa ativo ou fim preenchido (`:157-164`). A forma SQL está em `:187-193`.
- `app/escala/alimentacao.py:196-254` `vinculo_do_dia` escolhe a geradora da célula se encontrada e dentro dos limites de início/fim; fora retorna `None` (`:237-252`). Sem geradora escolhe entre vínculos que cobrem o dia pelo máximo `(ativa, data_inicio)` (`:253-254`). O ramo da geradora **não chama** `tem_vigencia_declarada`, ao contrário do ramo sem geradora (`:250-254`).
- `app/escala/folgas.py:392-444` `reparentar_folga`: calendário retorna `'calendario'` e não muda de dono (`:405-406`); manual cujo vínculo ainda cobre o dia retorna `'ja_cobre'` **mesmo se outro for escolhido pela juíza para o dia** (`:407-409`). Só quando o vínculo não cobre busca o alvo via `vinculo_do_dia_carregado` (`:414-416`). Alvo com foto já existente resulta `'ocupado'`; sem alvo resulta `'sem_vinculo'`; com alvo migra e relavra a partir da competência corrente (`:418-443`).
- `app/escala/folgas.py:447-459` `reparentar_folgas_do_colaborador` seleciona só fotos manuais e só chama reparentamento quando o vínculo original não cobre a data; calendário nunca entra (`:453-459`). A mesma seleção está no comando `app/escala/management/commands/reparentar_folgas.py:28-34`.
- `app/escala/signals.py:172-191` aciona reparentamento ao criar vínculo ou mudar a vigência; delega à função acima (`:185-191`).

**Outros leitores diretos que classificam/exibem/censam a foto**

| Sítio | O que faz e tratamento de fotos em outro vínculo |
|---|---|
| `app/escala/views_plano_folgas.py:51-82` `_grid_ctx` | Carrega fotos dos vínculos ativos e indexa por **ec + dia**; foto do próprio ec é exibida como gravada antes de qualquer cálculo (`:55-73`). Foto presa a outro ec não aparece na linha do ec diário; não distingue origem ou testa vigência da data na leitura (`:60-73`). |
| `app/escala/views_plano_folgas.py:109-114` toggle | Busca ec ativo e foto desse ec/data; calendário recusa edição. Foto em outro ec não é encontrada, seja calendário/manual (`:109-114`); a escrita delega a `lancar_folga`/`alterar_folga` (`:124-137`). |
| `app/escala/folgas.py:254-260` `lancar_folga` e `:274-289` `_foto_do_dia` | Consultam por ec/data; foto própria de calendário impede edição, manual pode ser criada/alterada/removida. Foto em outro ec não é encontrada localmente; veto por pessoa é chamado para criação (`:258-260`, `:282-284`). |
| `app/escala/models.py:1212-1221` `defeito_folga_nao_definida` | Para 6x1 sem folga semanal, basta qualquer foto **do self no mês** para afastar defeito. Outro vínculo não conta; origem não diferencia (`:1217-1221`). |
| `app/ponto/services/diagnostico_escala.py:102-104` | Conta fotos do ec/mês e lê defeito acima; calendário/manual de outro ec não contam (`:102-104`). |
| `app/relatorios/furos.py:112-115` | Diagnóstico acrescenta “sem foto FolgaDia do mês” se ec não tem foto; outro vínculo não conta e origem não diferencia (`:112-115`). |
| `app/escala/services/fase_conflitante.py:52-58`, `:71-83` | Para ec ativo, lê suas fotos de qualquer data, calcula folga exata/paridade pela foto mais próxima e compara com âncora. Foto de outro ec não entra; origem/vigência de cada data não são filtradas (`:71-80`). |
| `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:55-60` | Monta `(colaborador_id, data)` de fotos da empresa na janela, **sem vínculo diário, origem ou vigência**. Tanto calendário errado quanto manual errado podem selecionar dias para rejulgamento; cruza com ausência não-falta (`:24-35`, `:55-60`) e, com `--apply` e universo esperado, chama cartório (`:63-72`). |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1532-1549` C5 | Foto entra se o vínculo original cobre o dia **ou** `vinculo_que_cobre(..., exceto=original)` não devolve `'nenhum'`; origem não distingue (`:1532-1536`). Assim calendário/manual em outro vínculo podem contar; cobra conflito se célula trabalha e competência não foi exportada (`:1540-1549`). |
| `app/escala/folgas.py:363-383` `vinculo_que_cobre` | Usado pelo C5: filtra candidatos pela vigência SQL, exclui vínculo opcional; um candidato vence, vários com único ativo vence, senão `'ambiguo'` (`:369-383`). C5 aceita inclusive `'ambiguo'`, pois só exclui `'nenhum'` (`app/chamados/management/commands/auditar_invariantes_chamados.py:1535-1536`). |
| `app/escala/folgas.py:83-108` `divergencia_foto_x_calendario` | Compara fotos **do ec/mês** com calendário atual; separa calendário/manual pela origem e calcula falta/sobra (`:99-108`). Fotos em outro ec não entram, qualquer que seja origem (`:101-104`). |
| `app/escala/folgas.py:150-175` `gerar_foto_folgas` | No ec/mês, preserva fotos manuais, apaga fotos de calendário e recria datas mandadas, pulando dias com manual (`:165-175`). Outro vínculo não é consultado/tocado por esses filtros (`:165-174`). |

A distinção material comprovada é: **a precedência padrão exclui calendário de outro vínculo e conserva manual de outro vínculo; o veto consulta ambos por pessoa; cálculo/foto e portas locais consultam por vínculo; o overlay por turno percorre vínculos de toda a janela e aplica cada foto à janela inteira** (`app/ponto/precedencia.py:234-238`, `:313-315`, `:458-466`; `app/escala/models.py:1251-1288`; `app/escala/utils.py:1261`, `:1785-1787`, `:1109-1119`).

## 31/10 12:31 · sessao 4943d5eb6b04 · - · - · origem=Ronald (tmux)

Censo confirmado de chamadas executadas:

| Alvo | Chamadores de produção |
|---|---|
| `_aplicar_folgas_foto` | `montar_grade_prevista`, `app/escala/utils.py:768`; `montar_grade_prevista_periodo_por_turno`, `app/escala/utils.py:1787`. |
| `montar_grade_prevista` | `_montar_grade_prevista_periodo_legacy`, `app/escala/utils.py:993`. |
| `_montar_grade_prevista_periodo_legacy` | Dispatcher `montar_grade_prevista_periodo`, `app/escala/utils.py:2029`. |
| `montar_grade_prevista_periodo_por_turno` | Dispatcher, `app/escala/utils.py:2027`; `_score_geometria`, `app/ponto/management/commands/detectar_par_relampago.py:136`. |
| `montar_grade_prevista_periodo` | `_julgar_colab_corpo`, `app/ponto/services/cartorio.py:931`; `Command.handle`, `app/ponto/management/commands/reconciliar_grade.py:118`; `grade_espelho_janela`, `app/escala/utils.py:2277`. |

Há referências executáveis que **inspecionam assinatura**, sem chamar o builder: `app/ponto/management/commands/censo_col900.py:53` lê default `tol_min` de `montar_grade_prevista`; `app/ponto/management/commands/tripwire_celulas.py:124` lê default `tol_min` do dispatcher.

O comentário que enumera cinco consumidores e diz que `detectar_intervalo_ausente` chama o single builder está em `app/escala/utils.py:2017`, mas não descreve o censo atual acima; chamadas reais estão nos pontos da tabela.

Por chamador do dispatcher:

1. **Cartório: lê ambos; decide e grava.**
   - Indexa `g['dias']` por data: `app/ponto/services/cartorio.py:934`.
   - Ramo nunca bateu lê `tipo_dia in ('ausencia', 'falta')` para cobertura integral: `app/ponto/services/cartorio.py:997`; produz ata: `app/ponto/services/cartorio.py:1011`.
   - Ramo normal conta células não missing + órfãs para `batidas_no_dia`: `app/ponto/services/cartorio.py:1170`.
   - Passa dia e marcos ao classificador: `app/ponto/services/cartorio.py:1207`. O classificador lê `tipo_dia`, previsto, realizado e células: `app/ponto/supra_juiz.py:199`, `app/ponto/supra_juiz.py:207`; define dia de cobrança por tipo: `app/ponto/supra_juiz.py:281`; missing participa do furo parcial: `app/ponto/supra_juiz.py:348`, `app/ponto/supra_juiz.py:379`; tipo diferente de trabalho participa de `COBRANCA_EM_DIA_SEM_TRABALHO`: `app/ponto/supra_juiz.py:443`.
   - `ata_do_dia` transforma células em lâmpadas e grava `tipo_dia`, minutos, contagens: `app/ponto/services/cartorio.py:564`, `app/ponto/services/cartorio.py:567`, `app/ponto/services/cartorio.py:575`.
   - Lavra ata, veredito e fatos do status: `app/ponto/services/cartorio.py:1557`.
   - Códigos resultantes controlam emissão de chamado inteiro: `app/ponto/services/cartorio.py:1489`, `app/ponto/services/cartorio.py:1508`; parcial: `app/ponto/services/cartorio.py:1531`, `app/ponto/services/cartorio.py:1544`.
   - Ata deste julgamento também alimenta `marcos_acesos` para avaliar protesto e reconciliação: `app/ponto/services/cartorio.py:1182`, `app/ponto/services/cartorio.py:1204`, `app/ponto/services/cartorio.py:1275`.

2. **`reconciliar_grade`: não lê `dias[].tipo_dia`; lê células e decide chamados.**
   - Itera dias, pula sem âncora, extrai células missing e marca como limpo quando não há missing: `app/ponto/management/commands/reconciliar_grade.py:130`, `app/ponto/management/commands/reconciliar_grade.py:139`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:154`.
   - Missing vencidos geram `furo[dt]`: `app/ponto/management/commands/reconciliar_grade.py:169`, `app/ponto/management/commands/reconciliar_grade.py:179`.
   - Planeja/emite chamados, sujeito a outros vetos: `app/ponto/management/commands/reconciliar_grade.py:215`, `app/ponto/management/commands/reconciliar_grade.py:243`, `app/ponto/management/commands/reconciliar_grade.py:250`.
   - Dias avaliados limpos entram em `mortos`: `app/ponto/management/commands/reconciliar_grade.py:266`; todos os dias mortos permitem retratar chamado: `app/ponto/management/commands/reconciliar_grade.py:292`, `app/ponto/management/commands/reconciliar_grade.py:305`.

3. **`grade_espelho_janela`: não decide por tipo; clippa e repassa.**
   - Chama dispatcher, clippa, retorna: `app/escala/utils.py:2277`.
   - Clipping troca células por `None` e apaga órfãs fora da janela: `app/escala/utils.py:944`, `app/escala/utils.py:948`.

Consumidores vivos de `grade_espelho_janela`:

| Consumidor | Leitura/decisão |
|---|---|
| Espelho | `_pela_grade` chama em `app/ponto/services/espelho.py:503`; usado para trecho anterior à competência em `app/ponto/services/espelho.py:541` ou fallback de ata agregada em `app/ponto/services/espelho.py:548`, `app/ponto/services/espelho.py:571`. Lê tipo para `folga`: `app/ponto/services/espelho.py:690`; células não nulas para `previsto_canon`: `app/ponto/services/espelho.py:695`; células para apresentação de realizado/missing: `app/ponto/services/espelho.py:700`, `app/escala/utils.py:1059`. Transporta `folga`, `previsto_canon`, `realizado_grade`: `app/ponto/services/espelho.py:781`. Template usa esses campos: `app/templates/ponto/espelho.html:160`, `app/templates/ponto/espelho.html:166`, `app/templates/ponto/espelho.html:213`, `app/templates/ponto/espelho.html:226`. |
| Simulação | Chama em `app/ponto/services/simular_dia.py:7`; `dia_perfeito` exige tipo trabalho e ausência de órfãs e missing/divergência: `app/ponto/services/simular_dia.py:12`, `app/ponto/services/simular_dia.py:14`. `veredito_celula` recusa materializar trabalho se tipo não trabalho: `app/ponto/services/veredito_celula.py:220`. Flip automático usa dia perfeito antes/depois para auto/humano: `app/ponto/services/flip_auto.py:303`. |
| Mapa diagnóstico | Duas chamadas em `app/ponto/management/commands/mapa_divergencia.py:104`; assina tipo e células/status/missing em `app/ponto/management/commands/mapa_divergencia.py:120`, `app/ponto/management/commands/mapa_divergencia.py:131`; compara divergências em `app/ponto/management/commands/mapa_divergencia.py:158`, `app/ponto/management/commands/mapa_divergencia.py:208`. |
| PDF | Chamada em `app/relatorios/pdf_espelho.py:471` lê só órfãs; elas excluem entrada espúria da contagem de turnos abertos em `app/relatorios/pdf_espelho.py:509`. Segunda chamada em `app/relatorios/pdf_espelho.py:539` lê órfãs e células de isenção em `app/relatorios/pdf_espelho.py:548`, `app/relatorios/pdf_espelho.py:551`; não lê `tipo_dia`. Órfãs marcam batidas no papel em `app/relatorios/pdf_espelho.py:603`. Os dias desenhados vêm do espelho em `app/relatorios/pdf_espelho.py:592`. |

**Dinheiro/folha/export:** não há chamada direta do dispatcher em `folha/export.py`; o ponto de entrada `grade_do_fechamento` lê `grade_da_celula`: `app/folha/export.py:318`, `app/folha/export.py:324`, `app/folha/export.py:327`. `por_dia_da_grade` usa o tipo dessa grade para incluir somente trabalho/ausência no previsto, calcular abono e limitar realizado pelo previsto: `app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:392`, `app/ponto/services/dia_pago.py:393`, `app/ponto/services/dia_pago.py:398`. A ponte do dispatcher até esse dinheiro passa pela ata que o cartório lavra, nos pontos acima.

**Leitura interna adicional das células do overlay:** depois dos overlays (`app/escala/utils.py:1787`), o builder por turno monta cópia em memória da ata com lâmpadas das células atuais (`app/escala/utils.py:1826`, `app/escala/utils.py:1832`) e passa essas cópias ao juiz de turnos (`app/escala/utils.py:1838`); realizado final vem desse juiz (`app/escala/utils.py:1985`, `app/escala/utils.py:1998`). Portanto o apagamento de células também alcança a montagem do realizado dentro do próprio builder.

## 31/10 12:21 · sessao 46797b835cdf · E4-1 · leitura · origem=Code

**Não existe um conjunto `S` que torne a coluna equivalente a P-hoje para todo dia julgado.** A precedência consulta fatos atuais; a lavra combina grade, batidas, isenção e veredito. Inclusive `folga` pode resultar com P-hoje verdadeiro ou falso. `app/ponto/precedencia.py:305`, `app/ponto/status_dia.py:96`, `app/chamados/services/feriado_reavaliar.py:34`.

**1(a) — Todos os retornos de `status_da_lavra`**

Na tabela, aplica-se **a primeira linha satisfeita**. Notação: `B=tem_batida`, `G=tipo_dia`, `M=previsto_min`, `I=isento`, `C=cobertura_parcial`, `V=veredito`, `H=folga_humana`. `R={furo, discordante, cobrado, nunca_bateu, fato_em_ausencia}`; `A=R∪{indefinida}`. `app/ponto/status_dia.py:55`, `app/ponto/status_dia.py:70`.

| Ordem | Combinação de entrada | Status | Ramo |
|---|---|---|---|
| 1 | `!B && G=falta` | `falta` | `app/ponto/status_dia.py:96` |
| 2 | `!B && G=ausencia`, salvo `C && !I && V∈A` | `ausencia` | `app/ponto/status_dia.py:98` |
| 3 | `V=indefinida && H && B` | `folga_trabalhada` | `app/ponto/status_dia.py:101` |
| 4 | `V=indefinida && H && !B` | `folga` | `app/ponto/status_dia.py:103` |
| 5 | `V=indefinida && !H` | `escala_suspeita` | `app/ponto/status_dia.py:104` |
| 6 | `!B && (G=folga || !M)` | `folga` | `app/ponto/status_dia.py:105` |
| 7 | `!B && I` | `ok`, rótulo `isento de ponto` | `app/ponto/status_dia.py:108` |
| 8 | `!B && V=coberta` | `ausencia` | `app/ponto/status_dia.py:110` |
| 9 | `!B && V∈R` | `irregular` | `app/ponto/status_dia.py:112` |
| 10 | Demais entradas sem batida | `sem_certidao` | `app/ponto/status_dia.py:114` |
| 11 | `B && (G=folga || V=fato_sem_previsao)` | `folga_trabalhada` | `app/ponto/status_dia.py:115` |
| 12 | `B && V∈R` | `irregular` | `app/ponto/status_dia.py:117` |
| 13 | `B && V∈{concorde,coberta,cobranca_indevida}` | `ok`, rótulo de isento quando `I` | `app/ponto/status_dia.py:119` |
| 14 | Demais entradas com batida | `sem_certidao` | `app/ponto/status_dia.py:121` |

`aberto`, `previsto` e `sem_dados` pertencem a `ROTULO_STATUS`, mas **não são devolvidos por `status_da_lavra`**. No leitor, `sem_dados` vem da ausência de célula; `aberto/previsto` vêm do relógio; célula sem status pode devolver `previsto` ou `sem_certidao`. `app/ponto/status_dia.py:28`, `app/ponto/status_dia.py:173`, `app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:184`.

A coluna também tem uma segunda regra de entrada: decisão administrativa usa `status_do_admin`: `trabalhou→ok`, `faltou→falta`, `justificado→ausencia`; `_status_alvo` preserva essa autoridade mesmo numa passagem do cartório. `app/ponto/status_dia.py:52`, `app/ponto/portas/celula.py:694`.

**1(b) — Composição dos fatos, da grade e do status**

As tabelas seguintes especificam as combinações por etapas: primeiro `faixa/era_dia_previsto`; depois `tipo_dia/minutos_previstos`; finalmente a primeira linha aplicável de 1(a). Essa é também a composição implementada: precedência monta seus candidatos, a grade aplica overlays, e `fatos_do_status` extrai a entrada da lavra. `app/ponto/precedencia.py:332`, `app/escala/utils.py:1785`, `app/ponto/status_dia.py:146`.

A grade analisada abaixo é a implementação **por turno**, padrão da configuração. A flag efetiva do ambiente é [não verificado]. O dispatcher também admite o legacy; esse merge não propaga `minutos_previstos`, que `fatos_do_status` converte em zero quando ausente. `app/config/settings/base.py:165`, `app/escala/utils.py:2024`, `app/escala/utils.py:1016`, `app/ponto/status_dia.py:146`.

**Como a precedência calcula `era_dia_previsto`:**

| Célula/vínculo | `era_dia_previsto` |
|---|---|
| `celula.trabalha=True` | `True`, inclusive sem vínculo válido. `app/ponto/precedencia.py:305`, `app/ponto/precedencia.py:316` |
| `celula.trabalha=False` | `False`, inclusive sem vínculo válido. `app/ponto/precedencia.py:316` |
| Célula inexistente ou `trabalha=None`, com vínculo | Resultado de `eh_dia_trabalho_calculado`; exceção resulta em `None`. `app/ponto/precedencia.py:324` |
| Célula inexistente ou `trabalha=None`, sem vínculo | `None`. `app/ponto/precedencia.py:325` |

A geradora declarada pela célula só vale dentro de sua vigência; fora dela a seleção devolve `None`, sem escolher outro vínculo como reserva. `app/escala/alimentacao.py:239`, `app/escala/alimentacao.py:247`.

**Sem ausência vencedora**, sejam `F=feriado no calendário aplicável`, `T=trabalha_em_feriado do vínculo selecionado`, `L=FolgaDia considerada pela precedência`, `p=era_dia_previsto`:

| Combinação | `faixa` | `era_dia_previsto` | Fonte |
|---|---|---|---|
| `F && p=True && T=True`, sem `L` | `trabalho` | `True` | `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:339` |
| `F && p=True && T=True`, com `L` | `folga` | `True` | `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:337` |
| `F` e qualquer outra combinação de `p/T` | `feriado` | Mantém `p`, inclusive `None` | `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335` |
| Sem feriado candidato; `L` ou `p=False` | `folga` | Mantém `p` | `app/ponto/precedencia.py:337` |
| Sem feriado candidato nem `L`; `p=True` | `trabalho` | `True` | `app/ponto/precedencia.py:339` |
| Sem feriado candidato nem `L`; `p=None` | `None` | `None` | `app/ponto/precedencia.py:342` |

Para `L`, a precedência pega a primeira FolgaDia por `pk` de cada data. Se ela tem `calendario_origem` e pertence a outro vínculo, descarta essa linha; não procura uma segunda FolgaDia. A manual não passa por esse descarte de vínculo. `app/ponto/precedencia.py:234`, `app/ponto/precedencia.py:313`.

**Ausências na precedência:**

| Fato | Efeito em `faixa/era_dia_previsto` |
|---|---|
| Ausência inteira ou parcial, não rejeitada | Entra pela faixa do tipo, sem testar cobertura parcial; não modifica `era_dia_previsto`. `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:332`, `app/ponto/precedencia.py:360` |
| `ferias` | Faixa `ferias`. `app/ponto/precedencia.py:53` |
| Tipos mapeados como afastamento | Faixa `afastamento`. `app/ponto/precedencia.py:54` |
| `atestado/atestado_inss` | Faixa `atestado`. `app/ponto/precedencia.py:58` |
| `folga_compensatoria/feriado_folga` | Faixa `folga`; feriado candidato ainda vence. `app/ponto/precedencia.py:62`, `app/ponto/precedencia.py:46` |
| `falta` ou outro tipo fora do mapa | Faixa `atestado`, portanto vence feriado/folga. `app/ponto/precedencia.py:76`, `app/ponto/precedencia.py:349` |
| Mais de uma ausência | Considera todas e escolhe pela precedência `ferias > afastamento > atestado > feriado > folga > trabalho`. `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:349` |

**Base da grade por turno, antes dos overlays:**

| Combinação | `tipo_dia` | `minutos_previstos` | Fonte |
|---|---|---|---|
| Sem vínculo selecionado ou sem tipo de escala | `folga` | `0` | `app/escala/utils.py:1543`, `app/escala/utils.py:1743` |
| Vínculo não intermitente; `eh_dia_trabalho=False` | `folga` | `0` | `app/escala/utils.py:1548`, `app/escala/utils.py:1743` |
| Vínculo não intermitente; `eh_dia_trabalho=True` | `trabalho` | Soma dos segmentos previstos `E→S` | `app/escala/utils.py:1550`, `app/escala/utils.py:1578`, `app/escala/utils.py:1749` |
| Vínculo não intermitente; `eh_dia_trabalho=None` | `trabalho`, `sem_ancora=True` | Mesma soma dos marcos | `app/escala/utils.py:1548`, `app/escala/utils.py:1701`, `app/escala/utils.py:1749` |
| Intermitente sem batidas disponíveis para convocação | Não monta turno; cai em `folga/0` | `0` | `app/escala/utils.py:1476`, `app/escala/utils.py:1743` |
| Intermitente com convocação por batidas | Monta o dia na pré-passagem própria | Previsto próprio desse ramo | `app/escala/utils.py:1480`, `app/escala/utils.py:1720` |

Logo, os booleanos enumerados na pergunta não fixam um número de minutos: o ramo comum soma a duração dos segmentos dos marcos encadeados. `app/escala/utils.py:1578`.

**Onde entram feriado e `trabalha_em_feriado` na grade:**

A grade chama:

```python
if e.eh_dia_trabalho(d, celulas=_cels_gr, folgas=_folgas_gr) is False:
    continue
```

`app/escala/utils.py:1548`.

Esse método devolve a célula existente diretamente:

```python
if cel is not None:
    return cel.trabalha
return self.eh_dia_trabalho_calculado(data, folgas=folgas)
```

`app/escala/models.py:1071`.

Portanto, **havendo célula, a grade não reaplica o feriado nesse gate**. O tratamento está no fallback calculado:

```python
if base is True and self.feriado_vira_folga(data, escala=escala):
    return False
```

`app/escala/models.py:1338`.

O helper recusa a transformação quando `trabalha_em_feriado=True` ou quando o ciclo não pertence a `6x1/5x2/semanal`; depois consulta o feriado pelo lugar do colaborador. `app/escala/models.py:1357`, `app/escala/models.py:32`.

Isso produz, entre outras, estas combinações diretas, antes de overlays:

| Fatos | Precedência | Grade | Status resultante |
|---|---|---|---|
| Célula `True`, vínculo válido, feriado, `T=False` | `feriado/True` | `trabalho/M` | Pela tabela 1(a): sem batida e `V∈R` dá `irregular`; isento dá `ok`; com batida, `concorde` dá `ok`. `app/ponto/precedencia.py:330`, `app/escala/models.py:1071`, `app/ponto/status_dia.py:108`, `app/ponto/status_dia.py:112`, `app/ponto/status_dia.py:119` |
| Mesmos fatos, `T=True` | `trabalho/True` | `trabalho/M` | Mesmos ramos de status; muda P-hoje. `app/ponto/precedencia.py:330`, `app/ponto/status_dia.py:105` |
| Célula `False`, feriado, qualquer `T` | `feriado/False` | `folga/0` | Sem batida: `folga`; com batida: `folga_trabalhada`; `indefinida` também segue `H=True`. `app/ponto/precedencia.py:330`, `app/escala/utils.py:1548`, `app/ponto/status_dia.py:101`, `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:115` |
| Célula `False`, sem feriado/ausência/folga humana | `folga/False` | `folga/0` | Normalmente `folga/folga_trabalhada`; com `indefinida`, `H=False` dá `escala_suspeita`. `app/ponto/precedencia.py:337`, `app/ponto/precedencia.py:191`, `app/ponto/status_dia.py:104` |
| Sem vínculo, célula `True`, sem feriado | `trabalho/True` | `folga/0` | Sem batida e `nunca_bateu`: `folga`; `indefinida`, sem natureza humana: `escala_suspeita`. `app/ponto/precedencia.py:316`, `app/escala/utils.py:1543`, `app/ponto/status_dia.py:104`, `app/ponto/status_dia.py:105` |
| Sem vínculo, célula `True`, com feriado | `feriado/True` | `folga/0` | Sem batida: `folga`, inclusive com `indefinida`, pois `H=True`. `app/ponto/precedencia.py:330`, `app/ponto/status_dia.py:103`, `app/ponto/status_dia.py:105` |
| Sem vínculo, célula inexistente ou `None`, com feriado | `feriado/None` | `folga/0` | Pode resultar `folga`, mas P-hoje pula por previsão `None`. `app/ponto/precedencia.py:325`, `app/ponto/precedencia.py:335`, `app/ponto/status_dia.py:103`, `app/chamados/services/feriado_reavaliar.py:34` |

**Overlays que completam todas essas combinações:**

| Fato adicional | Grade após o overlay | Consequência para o status |
|---|---|---|
| FolgaDia manual, cujo vínculo participa da janela | `folga`, apaga células/órfãs; **mantém os minutos anteriormente calculados** | Sem batida: `folga`; com batida: `folga_trabalhada`; `indefinida` consulta a natureza humana. `app/escala/utils.py:1109`, `app/escala/utils.py:1118`, `app/ponto/status_dia.py:101`, `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:115` |
| FolgaDia de calendário de **outro vínculo** | Só aplica se esse outro vínculo estiver em `escs_janela_ids`; quando aplica, muda todos os dias correspondentes da grade, sem conferir o vínculo de cada data ou `calendario_origem` | Pode produzir `folga/folga_trabalhada` enquanto a precedência descartou essa FolgaDia. `app/escala/utils.py:1260`, `app/escala/utils.py:1109`, `app/escala/utils.py:1785`, `app/ponto/precedencia.py:313` |
| Ausência pendente/não aprovada | Não entra no overlay de ausência da grade; a precedência a considera se não rejeitada | Mantém o status derivado da grade anterior, apesar de poder mudar `faixa`. `app/escala/utils.py:1148`, `app/ponto/turnos.py:2487`, `app/ponto/precedencia.py:227` |
| Ausência aprovada inteira, não `falta` | `ausencia`, mantém minutos, limpa células/órfãs | Sem batida: `ausencia`, inclusive com `indefinida`. Com batida: segue os ramos de veredito de 1(a). `app/escala/utils.py:1180`, `app/escala/utils.py:1195`, `app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:115` |
| Ausência aprovada parcial | `ausencia`, mantém minutos e células, grava `cobertura_parcial=True` | Sem batida, não isento, `V∈R`: `irregular` se há previsto; `indefinida`: `escala_suspeita` salvo `H`; demais: `ausencia`. Se previsto zero, o ramo 6 pode devolver `folga`. `app/escala/utils.py:1190`, `app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:101`, `app/ponto/status_dia.py:106`, `app/ponto/status_dia.py:112` |
| Parcial + isento Art.62, sem batida | Continua `ausencia` | `ausencia`, inclusive `nunca_bateu/indefinida`. `app/ponto/status_dia.py:98` |
| Falta aprovada sobre dia que não está em `folga` | `falta`, mantém minutos, limpa células/órfãs | Sem batida: `falta`, antes de qualquer veredito. `app/escala/utils.py:1170`, `app/escala/utils.py:1173`, `app/ponto/status_dia.py:96` |
| Falta aprovada sobre `folga` | Mantém `folga` | Pode dar `folga/folga_trabalhada`; a precedência, contudo, considera a falta na faixa `atestado`. `app/escala/utils.py:1170`, `app/ponto/precedencia.py:76`, `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:115` |
| Ausência sem nenhum vínculo selecionado na janela | O loop não chama `_marcar_ausencias` | Permanece a classificação base da grade. `app/escala/utils.py:1785` |
| Isento Art.62 | Overlay posterior a folga/ausência; não muda tipo nem minutos, marca células presumidas como `isencao` | Trabalho sem batida, previsto positivo e sem ramo anterior: `ok`; folga/ausência conservam seus ramos anteriores. `app/escala/utils.py:1790`, `app/escala/utils.py:819`, `app/ponto/status_dia.py:96`, `app/ponto/status_dia.py:108` |

A cobertura parcial é determinada pelo registro: tipo admite horas, período de um dia, `0<minutos<480`. A grade escolhe a ausência aprovada mais recente por `(data,pk)`, enquanto a precedência considera todas as não rejeitadas e ordena por faixa. `app/ponto/turnos.py:2571`, `app/ponto/turnos.py:2734`, `app/ponto/turnos.py:2750`, `app/ponto/precedencia.py:349`.

`B` não significa apenas “existe linha de Batida”: `fatos_do_status` conta células não missing/não isenção, órfãs **ou realizado positivo**. Assim, limpar células por folga/ausência inteira não garante `B=False`. `app/ponto/status_dia.py:143`.

`H` só é consultado quando `V=indefinida`: é verdadeiro para feriado vencedor, FolgaDia vencedora ou subtipo de ausência na faixa folga; mera folga da escala não basta. `app/ponto/status_dia.py:135`, `app/ponto/precedencia.py:187`.

**1(c) — Equivalência e diferenças nos dois sentidos**

Para `batida_ausente` com dia encontrado, P-hoje equivale a:

```text
era_dia_previsto != None
e faixa ∈ {feriado, folga}
```

O catálogo associa qualquer subcategoria desse módulo a `falta_descoberta`; passando esse fato como `False`, a premissa está morta. `app/chamados/services/feriado_reavaliar.py:30`, `app/chamados/services/feriado_reavaliar.py:34`, `app/chamados/catalogo/premissa.py:58`, `app/chamados/catalogo/premissa.py:134`.

**Prova de que não existe `S`:** considere o caminho implementado com célula `trabalha=True`, geradora fora da vigência, sem ausência e sem adesão. A seleção não encontra vínculo, a grade produz `folga/0`, e o ramo sem adesão lavra `nunca_bateu`; o status é `folga`. Sem feriado, a precedência devolve `trabalho/True`, e P-hoje rejeita; com feriado, devolve `feriado/True`, e P-hoje aceita. O mesmo valor `folga` exigiria pertencer e não pertencer a `S`. `app/escala/alimentacao.py:250`, `app/escala/utils.py:1543`, `app/escala/utils.py:1743`, `app/ponto/services/cartorio.py:964`, `app/ponto/services/cartorio.py:1041`, `app/ponto/status_dia.py:105`, `app/ponto/precedencia.py:316`, `app/ponto/precedencia.py:335`.

Como `S` não existe, as diferenças dependem do conjunto escolhido. Para o conjunto de rótulos `S₀={folga,folga_trabalhada}`, esta é a **partição completa dos ramos divergentes**, sempre respeitando os retornos anteriores da tabela 1(a):

| Sentido | Combinação | Resultado |
|---|---|---|
| P-hoje aceita, coluna fora de `S₀` | Sem batida, grade `ausencia`, sem a exceção da parcial | `ausencia`. Inclui os subtipos `folga_compensatoria/feriado_folga`, que A2 põe em folga e a grade em ausência. `app/ponto/precedencia.py:62`, `app/escala/utils.py:1180`, `app/ponto/status_dia.py:98` |
| P-hoje aceita, coluna fora de `S₀` | `indefinida && !H`, sem retorno anterior | `escala_suspeita`. O ramo puro cobre folga de escala; sua produção pelo classificador num julgamento consistente é [não verificado]. `app/ponto/status_dia.py:104`, `app/ponto/tests/test_e3_status_lavrado.py:105` |
| P-hoje aceita, coluna fora de `S₀` | Sem batida, grade diferente de folga, previsto positivo, veredito diferente de `indefinida`, sem retorno anterior | Isento: `ok`; `coberta`: `ausencia`; `V∈R`: `irregular`; demais: `sem_certidao`. É a divergência do feriado vivo sobre célula `True`, entre outras entradas. `app/ponto/precedencia.py:330`, `app/ponto/status_dia.py:108` |
| P-hoje aceita, coluna fora de `S₀` | Com batida, grade diferente de folga, `V` diferente de `indefinida/fato_sem_previsao` | `R→irregular`; `concorde/coberta/cobranca_indevida→ok`; demais→`sem_certidao`. `app/ponto/status_dia.py:115` |
| Coluna em `S₀`, P-hoje rejeita | `indefinida && H`, sem retorno anterior, mas `era_dia_previsto=None` | `folga` ou `folga_trabalhada`; P-hoje pula o dia desconhecido. `app/ponto/status_dia.py:101`, `app/chamados/services/feriado_reavaliar.py:34` |
| Coluna em `S₀`, P-hoje rejeita | Sem batida, `V!=indefinida`, `G=folga || !M`, sem retorno anterior, e P-hoje falso | `folga`. Abrange falta sobre folga, ausência pendente que vence A2, overlay de outro vínculo, grade sem vínculo e trabalho com previsto zero. `app/ponto/status_dia.py:105`, `app/escala/utils.py:1170`, `app/ponto/precedencia.py:227`, `app/escala/utils.py:1785`, `app/escala/utils.py:1743` |
| Coluna em `S₀`, P-hoje rejeita | Com batida, `V!=indefinida`, `G=folga || V=fato_sem_previsao`, e P-hoje falso | `folga_trabalhada`. `app/ponto/status_dia.py:115` |

No mesmo estado das fontes, P-hoje verdadeiro com `status_da_lavra=falta` não nasce do pipeline descrito: a grade exige ausência aprovada `tipo=falta`, e essa linha também entra em A2 na faixa `atestado`, que vence feriado/folga. A coluna administrativa ou antiga tem caminhos adicionais, já separados acima. `app/escala/utils.py:1173`, `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:76`, `app/ponto/portas/celula.py:694`.

**1(d) — Quando a coluna pode ficar desatualizada**

`status_do_dia_lavrado` não compara a coluna com os fatos atuais nem relavra. Quando o relógio permite ler a lavra, devolve `cel.status_dia/status_rotulo` existentes. `app/ponto/status_dia.py:171`, `app/ponto/status_dia.py:181`.

| Escrita | Ordem e condição de desatualização |
|---|---|
| **Feriado criado/alterado** | `save/create` agenda somente `reavaliar_dia` em `on_commit`; não relavra antes. Se mudou de data, captura somente a **nova data**. `app/core/views_config.py:58`, `app/core/views_config.py:81`, `app/chamados/signals.py:265` |
| **Feriado apagado** | A tela executa `delete`; não há `post_delete` de reavaliação, explicitamente porque esse serviço apenas fecha. `app/core/views_config.py:198`, `app/chamados/signals.py:260` |
| **FolgaDia manual pelas portas** | Lançar/alterar/lote escreve a foto e chama regeneração. A cura depende de o dia ser efetivamente tocado e elegível para julgamento; não é atualização incondicional do status. `app/escala/folgas.py:257`, `app/escala/folgas.py:301`, `app/escala/folgas.py:328`, `app/ponto/portas/celula.py:360` |
| **Foto de calendário** | Apaga a foto anterior não manual, faz `bulk_create` e chama `relavrar_celulas_da_foto→regenerar_celulas_vinculo`; mesmas condições da re

[truncado em 20000 de 32859 caracteres]

## 31/10 12:17 · sessao 10cecc7608df · - · - · origem=Ronald (tmux)

Leitura somente; não encontrei `AGENTS.md` no resultado de `rg --files -g AGENTS.md`. [não verificado]

**P1(a): tabela exata da função pura**, aplicando os ramos em ordem, isto é, cada linha pressupõe que nenhuma anterior retornou. Vocabulário em `app/ponto/status_dia.py:28`; conjuntos `R={furo,discordante,cobrado,nunca_bateu,fato_em_ausencia}` e `A=R∪{indefinida}` em `app/ponto/status_dia.py:55`.

| Condição | Resultado | Ramo |
|---|---|---|
| Sem batida, `tipo_dia=falta` | `falta` | `app/ponto/status_dia.py:96` |
| Sem batida, `tipo_dia=ausencia`, exceto `cobertura_parcial && !isento && veredito∈A` | `ausencia` | `app/ponto/status_dia.py:98` |
| `veredito=indefinida`, `folga_humana`, com batida | `folga_trabalhada` | `app/ponto/status_dia.py:101` |
| Idem, sem batida | `folga` | `app/ponto/status_dia.py:103` |
| `veredito=indefinida`, sem `folga_humana` | `escala_suspeita` | `app/ponto/status_dia.py:104` |
| Sem batida e (`tipo_dia=folga` ou previsto zero) | `folga` | `app/ponto/status_dia.py:105` |
| Sem batida, previsto positivo, isento | `ok`, rótulo `isento de ponto` | `app/ponto/status_dia.py:108` |
| Sem batida, previsto positivo, não isento, `coberta` | `ausencia` | `app/ponto/status_dia.py:110` |
| Sem batida, previsto positivo, não isento, `veredito∈R` | `irregular` | `app/ponto/status_dia.py:112` |
| Outros sem batida | `sem_certidao` | `app/ponto/status_dia.py:114` |
| Com batida e (`tipo_dia=folga` ou `fato_sem_previsao`) | `folga_trabalhada` | `app/ponto/status_dia.py:115` |
| Com batida, `veredito∈R` | `irregular` | `app/ponto/status_dia.py:117` |
| Com batida, `veredito∈{concorde,coberta,cobranca_indevida}` | `ok`, rótulo de isento se aplicável | `app/ponto/status_dia.py:119` |
| Demais com batida | `sem_certidao` | `app/ponto/status_dia.py:121` |

`aberto`, `previsto` e `sem_dados` estão no vocabulário, mas não em retorno de `status_da_lavra`; são produzidos pelo leitor nos caminhos relógio/sem célula/sem lavra. `app/ponto/status_dia.py:70`, `app/ponto/status_dia.py:173`, `app/ponto/status_dia.py:177`, `app/ponto/status_dia.py:184`.

**P1(b): composição exata das fontes.**

- `fatos_do_dia` delega ao período de um dia. `app/ponto/precedencia.py:195`, `app/ponto/precedencia.py:209`.
- Ausências: todas não rejeitadas que cobrem a data, sem distinguir inteira/parcial; são ordenadas pela faixa `ferias > afastamento > atestado > feriado > folga > trabalho`. Os tipos `folga_compensatoria` e `feriado_folga` têm faixa `folga`; `falta` e tipos fora do mapa têm faixa `atestado`. `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:76`, `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:294`, `app/ponto/precedencia.py:332`, `app/ponto/precedencia.py:349`.
- `era_dia_previsto`: se célula tem `trabalha!=None`, é `bool(cel.trabalha)`, inclusive se não existe vínculo válido; quando célula ausente ou `trabalha=None`, calcula pelo vínculo, ou `None` sem vínculo/exceção. `app/ponto/precedencia.py:305`, `app/ponto/precedencia.py:316`.
- Feriado só é suprimido da precedência quando `feriado && previsto is True && ec.trabalha_em_feriado`; com `previsto=False/None`, feriado continua faixa candidata. `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335`.
- Folga manual permanece válida mesmo pertencendo a outro vínculo. Folga de calendário de outro vínculo é descartada comparando seu vínculo ao retornado por `vinculo_do_dia`. `app/ponto/precedencia.py:313`.
- Sem ausências: feriado candidato vence folga manual ou `previsto=False`; sem feriado candidato, folga manual/`False` dá faixa `folga`; `True` sem folga dá `trabalho`; `None` sem fatos dá faixa `None`. `app/ponto/precedencia.py:335`, `app/ponto/precedencia.py:342`, `app/ponto/precedencia.py:349`.
- A grade por turno seleciona vínculo com a mesma juíza; sem vínculo/tipo de escala, não monta turno. `app/escala/utils.py:1254`, `app/escala/utils.py:1541`.
- **Grade não aplica feriado diretamente.** Chama `e.eh_dia_trabalho`; esta lê `cel.trabalha`, inclusive `None`, sem recalcular quando célula existe. A grade só pula o turno quando o retorno é exatamente `False`; `None` passa pelo gate. `app/escala/utils.py:1548`, `app/escala/models.py:1066`.
- Feriado e `trabalha_em_feriado` são tratados no **fallback aritmético**: apenas `base=True` e `feriado_vira_folga` levam a `False`; o helper exige vínculo sem `trabalha_em_feriado` e ciclo em `CICLOS_FERIADO_VIRA_FOLGA`. Retornos antecipados da âncora 12x36/24x48 precedem essa etapa. `app/escala/models.py:1260`, `app/escala/models.py:1338`, `app/escala/models.py:1357`.
- Grade sem turno: `tipo_dia=folga`, previsto `0`; grade com turno: `trabalho`, previsto soma minutos dos segmentos `E→S` dos marcos, excluindo pausas. `app/escala/utils.py:1578`, `app/escala/utils.py:1743`, `app/escala/utils.py:1749`.
- **Overlay FolgaDia não zera minutos:** muda `tipo_dia=folga`, apaga células e órfãs, preserva o previsto já calculado. `app/escala/utils.py:1109`, `app/escala/utils.py:1118`.
- **Folga de outro vínculo na grade depende da janela:** consulta apenas FolgaDia do vínculo passado ao overlay; porém o builder chama o overlay de **cada vínculo que possui algum dia na janela**, sobre **todos os dias da grade**, sem filtrar pelo vínculo de cada data nem `calendario_origem`. Logo outro vínculo ausente de `escs_janela_ids` não aplica; outro vínculo presente no conjunto aplica a FolgaDia sobre essa data. `app/escala/utils.py:1260`, `app/escala/utils.py:1109`, `app/escala/utils.py:1785`.
- Ausências na grade usam cobertura para folha, portanto só aprovadas; a autoridade escolhe a mais recente por `(data,pk)`, enquanto A2 considera todas e desempata por faixa. `app/escala/utils.py:1148`, `app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2734`, `app/ponto/turnos.py:2750`.
- Falta aprovada: só transforma em `tipo_dia=falta` se dia não estiver em `folga`; limpa células/órfãs, sem alterar minutos. Ausência não falta aprovada: transforma em `ausencia` mesmo sobre folga, mantém minutos, grava `cobertura_parcial`, limpa células/órfãs só quando inteira. `app/escala/utils.py:1170`, `app/escala/utils.py:1190`, `app/escala/utils.py:1195`.
- Ausência pode deixar de ser aplicada se nenhum vínculo figura em `escs_janela_ids`, pois seu overlay está dentro desse loop. `app/escala/utils.py:1785`.
- Art.62 roda depois de folga/ausência e não muda `tipo_dia` ou previsto; preenche as células faltantes com marca `isencao`. `app/escala/utils.py:1787`, `app/escala/utils.py:1790`, `app/escala/utils.py:819`.
- `fatos_do_status` calcula batida com células não missing e não `isencao`, mais órfãs, **ou** realizado positivo; portanto ausência inteira/FolgaDia podem limpar células e ainda `tem_batida=True` pelo realizado. `app/ponto/status_dia.py:143`.
- Apenas `indefinida` faz `fatos_do_status` consultar A2 para `folga_humana`; A2 devolve verdadeiro para feriado vencedor ou folga lançada/subtipo folga vencedor, e falso para mera folga da escala. `app/ponto/status_dia.py:135`, `app/ponto/precedencia.py:187`.
- Flag da implementação da grade: escolhe por turno se `GRADE_POR_TURNO`; default configurado é verdadeiro, variável de ambiente pode alterar. O legacy também existe e, no merge final, não propaga `minutos_previstos`. `app/escala/utils.py:2024`, `app/config/settings/base.py:165`, `app/escala/utils.py:1016`.

**P1(c): não existe S.**

Prova por mesmo status `folga` em lados opostos: célula `trabalha=True` com geradora fora de vigência dá vínculo `None`; a grade devolve `folga/0`; A2 continua `era_dia_previsto=True`, faixa `trabalho` sem feriado/folga e faixa `feriado` com feriado. No ramo real sem adesão, o cartório grava `nunca_bateu` quando não há cobertura inteira; em ambas as entradas o status é `folga`, mas `espurios` só aceita a segunda. `app/escala/alimentacao.py:239`, `app/escala/alimentacao.py:250`, `app/escala/utils.py:1543`, `app/escala/utils.py:1743`, `app/ponto/precedencia.py:316`, `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1041`, `app/ponto/status_dia.py:105`, `app/chamados/services/feriado_reavaliar.py:34`.

Para comparar com `S0={folga,folga_trabalhada}`, a partição por ramos de `status_da_lavra` é mais segura que afirmar alcançabilidade de todas as combinações da função pura. A função pura explicitamente aceita `indefinida` + folga de escala => `escala_suspeita`, com teste; não confirmei execução real do classificador produzindo esse par em um snapshot consistente. `app/ponto/status_dia.py:101`, `app/ponto/tests/test_e3_status_lavrado.py:105`; alcançabilidade desse par em julgamento real [não verificado].

P verdadeiro e `status=falta` **não nasce desse pipeline no mesmo snapshot**: grade falta exige Ausencia aprovada tipo falta; essa mesma ausência está na A2, recebe faixa atestado e vence feriado/folga. `app/escala/utils.py:1148`, `app/escala/utils.py:1173`, `app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:76`, `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:349`.

P verdadeiro e `status=ausencia` pode vir dos subtipos `folga_compensatoria/feriado_folga`: A2 os classifica em folga e a grade rotula todos os não falta em ausência. `app/ponto/precedencia.py:62`, `app/escala/utils.py:1179`, `app/ponto/status_dia.py:98`.

P falso e status folga/folga trabalhada também ocorre nas entradas de: `era_dia_previsto=None` com natureza humana; falta aprovada sobre folga da grade; ausência pendente em folga/feriado (A2 a considera, grade não); overlay de calendário de outro vínculo presente na janela; trabalho com previsto zero; batida com `fato_sem_previsao`. São os ramos/filtros acima, não evidência de ocorrência em dados. `app/chamados/services/feriado_reavaliar.py:34`, `app/ponto/status_dia.py:101`, `app/escala/utils.py:1170`, `app/ponto/turnos.py:2487`, `app/ponto/precedencia.py:227`, `app/escala/utils.py:1785`, `app/ponto/status_dia.py:106`, `app/ponto/status_dia.py:115`.

Testes particularmente úteis para costura da lavra: `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:57` julga de verdade; controle sem natureza humana => `indefinida/escala_suspeita` em `:69`; feriado => `indefinida/folga` em `:74`; FolgaDia manual => `indefinida/folga` em `:82`. Testes puros cobrem parcial, isento parcial e folga humana em `app/ponto/tests/test_e3_status_lavrado.py:78`, `:91`, `:100`, `:118`.

_1009 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
