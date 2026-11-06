# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 18:14** · publicado em **06/11/2026 18:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 18:14 · sessao 40b4c38a7809 · ESPELHO-DECIDIU · sonda · origem=Code

O ponto central é: **o valor de falta gravado soma as ausências aprovadas que descontam, mesmo havendo abono sobreposto**; já a cobertura usada para abono/supressão escolhe uma ausência por dia. São operações diferentes. (`app/folha/export.py:140`, `app/folha/export.py:142`, `app/ponto/services/fechamento.py:674`)

1) Transcrição do dado fornecido. `[nao verificado]` indica ausência de verificação independente na sombra; os números abaixo são os do texto recebido. [nao verificado]

```text
UNIVERSO dias_colab_com_ausencia_aprovada=5278 dias_com_2+=33 colabs=5 [nao verificado]
PARES_DE_TIPO {('falta', 'folga_abonada'): 1, ('falta', 'folga'): 1, ('ferias', 'ferias'): 29, ('atestado', 'falta'): 1, ('falta', 'folga_compensatoria'): 1} [nao verificado]
POR_MES {'2026-10': 3, '2026-09': 30} [nao verificado]
CLASSE (tipos, fechamento) {('so_abono', 'nao_desconta'): 29, ('abono+falta', 'desconta'): 4} [nao verificado]
ABONO+FALTA_QUE_DESCONTA 4 [nao verificado]
ERROS {} [nao verificado]
CONCLUSAO_LIBERADA [nao verificado]
rc_sombra=0 [nao verificado]
```

Os oito `EX` estão abaixo; as tuplas de `aus` foram mantidas sem atribuir nomes aos seus campos. A coluna `tela` resume os campos completos visíveis no trecho fornecido. [nao verificado]

| Colaborador / dia | `aus` | `horas_falta` / `falta_min` | `tela`: tipo / faixa / rótulo; suprimidos; previsto | Fonte |
|---|---|---|---|---|
| col127 / 2026-09-03 | `(4709, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','folga']`; `False` | [nao verificado] |
| col127 / 2026-09-04 | `(4710, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','trabalho']`; `True` | [nao verificado] |
| col127 / 2026-09-05 | `(4711, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','folga']`; `False` | [nao verificado] |
| col127 / 2026-09-06 | `(4712, 'ferias', 0, False, None, '2026-10-05 11:55')`; `(2634, 'ferias', 0, False, None, '2026-08-13 11:44')` | `0.0 / None` | `ferias / ferias / Ferias`; `['ferias','trabalho']`; `True` | [nao verificado] |
| col824 / 2026-09-07 | `(4519, 'folga_compensatoria', 0, False, None, '2026-09-28 16:34')`; `(4421, 'falta', 660, False, None, '2026-09-24 18:04')` | `11.0 / 660.0` | `falta / atestado / Atestado`; `['folga','trabalho']`; `True` | [nao verificado] |
| col643 / 2026-09-17 | `(4431, 'falta', 720, False, None, '2026-09-25 13:04')`; `(4434, 'atestado', 0, False, None, '2026-09-25 13:07')` | `12.0 / 720.0` | `falta / atestado / Atestado`; `['atestado','trabalho']`; `True` | [nao verificado] |
| col736 / 2026-10-04 | `(5146, 'folga', 0, False, None, '2026-10-23 17:14')`; `(5168, 'falta', 720, False, 28450, '2026-10-23 19:05')` | `12.0 / 720.0` | `falta / atestado / Atestado`; `['atestado','trabalho']`; `True` | [nao verificado] |
| col444 / 2026-10-10 | `(4830, 'falta', 720, False, 30118, '2026-10-12 18:33')`; `(5098, 'folga_abonada', 0, False, None, '2026-10-22 18:02')` | `12.0 / 720.0` | `falta / atestado / Atestado`; `['atestado','trabalho']`; `True` | [nao verificado] |

Nos quatro exemplos de férias, `fatos.ausencias=['ferias','ferias']`; nos demais, respectivamente, `['falta','folga_compensatoria']`, `['falta','atestado']`, `['falta','folga']` e `['falta','folga_abonada']`. O feriado de col824 aparece truncado como `'Independência do Bra…'`; os demais exemplos mostram `feriado=None`. Os textos de `tela` recebidos estão truncados. [nao verificado]

| C444: pk | Dia | Tipo | min | Status | pelo_proprio | chamado_origem | horas_falta / falta_min | Fonte |
|---|---|---|---:|---|---|---:|---|---|
| 5198 | 2026-10-24 | falta | 720 | aprovada | False | 34480 | `12.0 / 720.0` | [nao verificado] |
| 5099 | 2026-10-16 | falta | 420 | aprovada | False | None | `7.0 / 420.0` | [nao verificado] |
| 4830 | 2026-10-10 | falta | 720 | aprovada | False | 30118 | `12.0 / 720.0` | [nao verificado] |
| 5098 | 2026-10-10 | folga_abonada | 0 | aprovada | False | None | `12.0 / 720.0` | [nao verificado] |

2) Pelo código, há três operações que precisam ser distinguidas.

- **Cobertura para abono/supressão:** o fechamento chama `cobertura_ausencia_periodo(..., para='folha')`; só entram aprovadas. A consulta ordena por `data, pk` crescentes e sobrescreve o mapa: vence **maior data inicial; empatando, maior pk**. Não usa o instante de criação ou aprovação. (`app/ponto/services/fechamento.py:674`, `app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`)
- **Efeito da vencedora:** no fechamento, ABONA acrescenta o dia às justificadas; DESCONTA não o suprime; os demais efeitos entram nas suprimidas. Ausências pagas parciais são ressalvadas antes. `efeitos_da_ausencia` também consulta a **mesma função de cobertura**, com outro laço para aplicar ABONA/SUPRIME. (`app/ponto/services/fechamento.py:685`, `app/ponto/services/fechamento.py:689`, `app/ponto/services/fechamento.py:691`, `app/ponto/services/efeito_ausencia.py:53`, `app/ponto/services/efeito_ausencia.py:59`)
- **Quantidade de falta gravada:** vem de `minutos_de_falta_por_dia`, que percorre **todas** as aprovadas dos tipos `descontam_vigentes()` e acumula os minutos por dia. Não consulta a vencedora da cobertura nem elimina a falta pela presença de abono. Cada registro é rateado pelos seus dias; `DiaPago` recebe minutos/60 em `horas_falta` e os minutos em `detalhe.falta_min`. (`app/ponto/services/fechamento.py:864`, `app/folha/export.py:88`, `app/folha/export.py:118`, `app/folha/export.py:121`, `app/folha/export.py:140`, `app/folha/export.py:142`, `app/ponto/services/dia_pago.py:272`)

A expansão de dias usa intervalo fechado; sem fim, alcança o fim consultado somente para `TIPOS_EM_ABERTO`; nos demais tipos cobre apenas a data inicial, sempre recortada à janela. (`app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2785`)

**A saída `fatos_do_dia` da sonda usa outra escolha:** delega a `fatos_do_periodo`, inclui não rejeitadas e ordena inicialmente apenas por `data` crescente. Depois escolhe a primeira candidata pela faixa: `ferias`, `afastamento`, `atestado`, `feriado`, `folga`, `trabalho`. Na mesma faixa, a data mais antiga chega primeiro; mesma faixa e mesma data não têm desempate explícito por pk. (`app/ponto/precedencia.py:256`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:279`, `app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:396`)

`falta`, `folga` e `folga_abonada` caem na faixa padrão `atestado`; `folga_compensatoria` tem faixa explícita `folga`. O código devolve o tipo vencedor, mas monta o rótulo pela faixa: isso permite `tipo_do_dia='falta'`, `faixa='atestado'`, `rotulo='Atestado'`. (`app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:79`, `app/ponto/precedencia.py:403`, `app/ponto/precedencia.py:405`)

**A palavra final do espelho tem outra etapa:** o espelho carrega a cobertura de folha pela mesma `cobertura_ausencia_periodo` usada pelo fechamento e a passa a `dia_decidido.do_dia`. Quando existe cobertura, ela tem prioridade sobre os fatos A2, e o rótulo vem daquela ausência. (`app/ponto/services/espelho.py:588`, `app/ponto/services/espelho.py:340`, `app/ponto/services/dia_decidido.py:363`, `app/ponto/services/dia_decidido.py:428`)

Portanto:

| Comparação | Resposta pelo código |
|---|---|
| Cobertura do fechamento × cobertura da palavra final do espelho | **Mesma função:** `cobertura_ausencia_periodo`. (`app/ponto/services/fechamento.py:674`, `app/ponto/services/espelho.py:588`) |
| Cobertura × `fatos_do_dia` mostrado na sonda | **Funções e critérios diferentes:** maior `(data,pk)` versus faixa de precedência e ordem por data. (`app/ponto/turnos.py:2757`, `app/ponto/turnos.py:2769`, `app/ponto/precedencia.py:279`, `app/ponto/precedencia.py:396`) |
| Desconto gravado × essas escolhas | **Soma independente** das aprovadas que descontam. (`app/folha/export.py:140`, `app/folha/export.py:142`) |
| `decididos_do_periodo` | Outra função: inclui rejeitadas e sobrescreve por `(data,pk)`; não substitui a cobertura quando esta existe. (`app/ponto/services/dia_decidido.py:550`, `app/ponto/services/dia_decidido.py:555`, `app/ponto/services/dia_decidido.py:561`, `app/ponto/services/dia_decidido.py:363`) |

O recorte não mostra `data/data_fim` de cada registro nem a palavra final de `dia_decidido`; o pk vencedor da cobertura e essa palavra nos exemplos permanecem [nao verificado].

3) Escritores operacionais de `Ausencia.minutos` encontrados na cópia:

| Sítio | O que escreve ou encaminha |
|---|---|
| Campo do modelo | `default=0`. (`app/ponto/models.py:755`) |
| `Ausencia.save` | Deriva **somente falta aprovada com minutos falsy**; soma `minutos_previstos_do_dia` pelo intervalo e atribui o total. Valores positivos são preservados. (`app/ponto/models.py:877`, `app/ponto/models.py:885`, `app/ponto/models.py:887`) |
| `criar_ausencia` | Argumento com default zero → `kwargs['minutos']` → instância → save. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`) |
| `editar_ausencia_registro` | Atribui `int(minutos or 0)` e salva. (`app/ponto/services/ausencia.py:508`) |
| `mudar_estado` / `decidir` | Persiste `minutos` em `update_fields`, incluindo a derivação do modelo. (`app/ponto/services/ausencia.py:834`, `app/ponto/services/ausencia.py:911`) |
| `rederivar_fim` / comando de cura | Faz save completo, podendo persistir a derivação. (`app/ponto/services/ausencia.py:1296`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`) |
| Lançamento admin | Encaminha minutos recebidos da entrada. (`app/ponto/views.py:2388`, `app/ponto/views.py:2417`) |
| Submissão PWA | Encaminha `_min` recebido da entrada. (`app/ponto/views.py:2497`, `app/ponto/views.py:2539`) |
| API | Converte a entrada e encaminha `_min`. (`app/api/views.py:1815`, `app/api/views.py:1824`) |
| Edição pela tela | Lê POST, converte e passa à edição central. (`app/ponto/views.py:2784`, `app/ponto/views.py:2787`, `app/ponto/views.py:2801`) |
| Copiloto | Omite minutos; recebe default zero. (`app/ponto/services/checar_ausencia.py:46`, `app/ponto/services/ausencia.py:130`) |
| Veredito da célula | Omite minutos; recebe default zero. (`app/ponto/services/veredito_celula.py:315`, `app/ponto/services/ausencia.py:130`) |
| Declaração de pergunta de disputa | Omite minutos; recebe default zero. (`app/chamados/services/materializacao.py:484`, `app/ponto/services/ausencia.py:130`) |
| Materialização da resposta de disputa | Omite minutos; recebe default zero. (`app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`) |
| Sincronização de férias | Passa `minutos=0`. (`app/ponto/services/ausencia.py:1204`) |
| `garantir_ausencia` | Encaminha os argumentos à criação central. (`app/ponto/services/ausencia.py:461`) |
| Regularização | Passa `minutos=0` ao wrapper. (`app/ponto/services/regularizacao.py:119`) |
| Abandono provisório | Passa `minutos=0` ao wrapper. (`app/ponto/management/commands/marcar_abandono_provisorio.py:80`) |
| Comando H06 | Passa `minutos=alvo` à edição: aplicação rederiva; reversão restaura o valor anterior do snapshot. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:127`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`) |

**Nenhum desses produtores operacionais embute atualmente `720` ou outro literal positivo para minutos.** As duas materializações omitem o argumento; os comentários registram a retirada do valor fixo. Entradas explícitas e valores de reversão continuam podendo persistir números positivos, que `save` preserva. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:487`, `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:522`, `app/ponto/services/ausencia.py:508`, `app/ponto/models.py:877`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:170`)

Há literais como `720` e atualizações diretas em fixtures de testes. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:71`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:190`) O formulário é `forms.Form`, e o admin de Ausencia herda de `SoLeitura`. (`app/ponto/forms.py:41`, `app/ponto/admin.py:38`)

Quem escreveu os `720`, `660` e `420` dos registros concretos, e com qual versão do código, permanece [nao verificado].

4) **Existe veto de sobreposição, com uma exceção assimétrica:** `linha_que_veta` percorre as sobrepostas não rejeitadas e retorna a primeira não absolvida. `abona_sobre_falta` absolve somente **tipo novo que abona sobre registro existente de tipo `falta`**. Falta nova sobre abono não recebe essa exceção. (`app/ponto/turnos.py:2815`, `app/ponto/precedencia.py:145`, `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:131`)

| Caminho de criação | Passagem pelo veto |
|---|---|
| Admin, PWA, copiloto e veredito da célula | Usam o modo padrão, com `linha_que_veta` e veto final de precedência. (`app/ponto/views.py:2415`, `app/ponto/views.py:2536`, `app/ponto/services/checar_ausencia.py:46`, `app/ponto/services/veredito_celula.py:315`, `app/ponto/services/ausencia.py:323`, `app/ponto/services/ausencia.py:377`) |
| API | Mesmo fato vivo é devolvido; para novo fato, muda para modo `veto`. (`app/api/views.py:1826`, `app/ponto/services/ausencia.py:246`, `app/ponto/services/ausencia.py:253`) |
| Sincronização de férias | Usa criação padrão e registra as recusas. (`app/ponto/services/ausencia.py:1202`, `app/ponto/services/ausencia.py:1209`) |
| Regularização e abandono | Usam `retornar`: havendo sobreposição, devolvem a existente sem criar outra; não executam o veto final reservado ao modo `veto`. (`app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`, `app/ponto/services/ausencia.py:461`, `app/ponto/services/ausencia.py:312`, `app/ponto/services/ausencia.py:377`) |
| As duas materializações de disputa | **Não passam pelo veto entre tipos diferentes:** usam `retornar_mesmo_tipo`. (`app/chamados/services/materializacao.py:490`, `app/chamados/services/materializacao.py:526`) |

Nesse último modo, o produtor procura somente mesmo colaborador/tipo/data; se não encontrar, define `conflito=None` e sobrepostas vazias. Também pula o veto final, condicionado a `se_existir=='veto'`. (`app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:302`, `app/ponto/services/ausencia.py:377`)

Outros escritores que não passam por `linha_que_veta`:

- **Aprovação posterior:** o gate verifica documento/contradição, e `decidir → mudar_estado` verifica a transição e salva; não reconsulta sobreposição nem substitui a falta anterior. (`app/ponto/services/ausencia.py:619`, `app/ponto/services/ausencia.py:628`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:820`, `app/ponto/services/ausencia.py:835`)
- **Edição:** não usa essa exceção; tem veto próprio mais restrito, recusando qualquer outra sobreposta. (`app/ponto/services/ausencia.py:487`, `app/ponto/services/ausencia.py:491`)
- **Reversão de rejeição:** chama `_reabrir`, que muda o estado sem consultar esse veto. (`app/ponto/services/ausencia.py:1102`, `app/ponto/services/ausencia.py:1141`)
- **Rederivação e modelo:** `rederivar_fim` chama `save`; o `save` deriva carimbo, minutos e intervalo, sem veto de sobreposição. A constraint declarada limita apenas duplicidade viva de **colaborador/tipo/data inicial**, permitindo tipos diferentes e interseções de intervalos com inícios distintos. (`app/ponto/services/ausencia.py:1296`, `app/ponto/models.py:868`, `app/ponto/models.py:877`, `app/ponto/models.py:894`, `app/ponto/models.py:827`)

O dado raspado não identifica qual desses caminhos produziu cada sobreposição; atribuir os casos concretos a um escritor específico permanece [nao verificado].

## 06/11 18:13 · sessao 1875a7bbf072 · - · - · origem=Ronald (tmux)

**MÉDIA — remanescente: vigia perde competência e datas ao decidir roteamento.** `na_lista` conserva somente IDs, e `rot` verifica somente `colab`; assim, C5 positiva na anterior pode retirar da conta de calados a linha da corrente sem nenhum dia corrente na C5. A unidade declarada é pessoa, mas o relatório apresenta o número por competência. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:54`.

Como provar: sonda executada em memória com o corpo real de `censo` extraído por AST, sem Django/DB: dois retratos, competências 10 e 11, mesmo `colab=7`, `dono='ninguem'`; C5 devolvendo somente `05/10`. Resultado observado: `bloqueados=2`, `calados=0`, `roteados=2`, um roteado por competência. O mecanismo responsável são as linhas 43 e 48; ocorrência em produção **[nao verificado]**. `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

Cenário real plausível: anterior retida por `nunca_bateu`; corrente retida por `sem_celula`, sem porta. O retentor produz dias sem célula, cuja ausência de porta conduz a grupo D; entretanto, C5 consulta somente células `nunca_bateu`. `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:86`, `app/escala/services/cadastro_realidade.py:175`.

**MÉDIA — remanescente: “roteado à lista” pode apontar empresa que a tela exclui.** O contador lê retratos históricos independentemente da atividade da empresa; C5 também consulta todas as empresas ou os IDs informados. A tela e o lavrador usam `empresas_visiveis`, que exige `ativa=True` e exclui CNPJ diagnóstico. `app/folha/vigia_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:38`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/colaboradores/queries.py:85`.

Como provar: lavrar retrato com C5 positiva, depois tornar a empresa inativa e consultar `censo([id])`; o retrato continua no universo do contador, mas a empresa sai da tela e das próximas lavras. Sonda com o corpo real de `censo` em memória, retrato associado a `empresa.ativa=False` e C5 positiva retornou `bloqueados=1`, `calados=0`, `roteados=1`, `cego=0`. A sonda comprova a ausência de filtro no contador; ocorrência real em produção **[nao verificado]**. `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:48`, `app/escala/services/cadastro_realidade.py:185`, `app/escala/views.py:487`.

**Mapa dos campos da lavra consumidos pelo diff:**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `status='fora'`, `motivo='furo_espelho'` | `classificar_export`, sobre fechamentos da empresa/competência. `app/folha/export.py:775`, `app/folha/export.py:962` | `montar_linhas` copia; `_balde` interpreta; C5 exige motivo; vigia lê balde. `app/folha/retrato_folha.py:1164`, `app/folha/retrato_folha.py:1180`, `app/folha/retrato_folha.py:89`, `app/escala/services/cadastro_realidade.py:189`, `app/folha/vigia_retido_calado.py:46` |
| `datas` de furo | Retentor → `dias_que_seguram`, somente para itens de furo; `fora_da_folha` converte datas ISO. `app/folha/fora_da_folha.py:95`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:192` | `montar_linhas` copia; C5 une e compara ISO. `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1183`, `app/escala/services/cadastro_realidade.py:190`, `app/escala/services/cadastro_realidade.py:192` |
| `colab` | ID vindo do item do export, copiado no JSON. `app/folha/retrato_folha.py:1163`, `app/folha/retrato_folha.py:1178` | C5 e vigia. `app/escala/services/cadastro_realidade.py:189`, `app/folha/vigia_retido_calado.py:48` |
| `dono` contendo fio | `_fio_cru`, chamado pelo montador. `app/folha/retrato_folha.py:364`, `app/folha/retrato_folha.py:1186` | `fio_da_linha`, usado pelo vigia para selecionar `ninguem`. `app/folha/retrato_folha.py:428`, `app/folha/retrato_folha.py:438`, `app/folha/vigia_retido_calado.py:47` |
| `empresa`, `mes`, `ano`, `linhas`, `lavrado_em` | Command `lavrar_retrato_folha --apply`. `app/folha/management/commands/lavrar_retrato_folha.py:74` | C5 seleciona empresa/competências; vigia seleciona empresa e último dia de lavra, ordena e relata competência; porta lê `linhas`. `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:45`, `app/folha/retrato_folha.py:110` |
| Retorno C5 `{cid:[datas]}` | Função C5, apenas em memória. `app/escala/services/cadastro_realidade.py:194` | `_nunca_bateu` formata assinatura; `lista` incorpora; `acao` fornece porta; vigia reduz a IDs. `app/escala/services/cadastro_realidade.py:201`, `app/escala/services/cadastro_realidade.py:833`, `app/escala/services/cadastro_realidade.py:657`, `app/folha/vigia_retido_calado.py:43` |
| `hoje` do vigia | Command lê `timezone.localdate()`. `app/folha/management/commands/vigiar_retido_calado.py:31` | `censo` repassa à C5, que calcula ontem e janelas. `app/folha/vigia_retido_calado.py:43`, `app/escala/services/cadastro_realidade.py:170`, `app/escala/services/cadastro_realidade.py:174` |
| Contadores e `cego` | `censo`, em memória. `app/folha/vigia_retido_calado.py:44`, `app/folha/vigia_retido_calado.py:49`, `app/folha/vigia_retido_calado.py:56` | `linhas` imprime; command avalia alarme. `app/folha/vigia_retido_calado.py:60`, `app/folha/management/commands/vigiar_retido_calado.py:32`, `app/folha/management/commands/vigiar_retido_calado.py:36` |

Nenhum defeito adicional de execução identificado nesta revisão do delta; execução da suíte Django **[nao verificado]**. [nao verificado]

## 06/11 18:12 · sessao 0307bae4437f · - · - · origem=Ronald (tmux)

**Sem divergência aritmética entre lavrador e leitor para o mesmo `hoje` e corte.** Ambos derivam `(mes, ano)` do fim das mesmas duas janelas; C5 usa o início da anterior como limite inferior. `app/folha/management/commands/lavrar_retrato_folha.py:60`, `app/escala/services/cadastro_realidade.py:174`, `app/escala/services/cadastro_realidade.py:176`, `app/escala/services/cadastro_realidade.py:183`.

Sonda em memória, com as funções reais, conferiu 49.329 combinações: todos os dias de 2024–2028, cortes 2–28; nenhuma divergência de contenção, continuidade ou reconstrução por competência. A implementação inclui `dia >= corte`, dezembro→janeiro e anterior por dia imediatamente antes do início corrente. `app/ponto/janelas.py:15`, `app/ponto/janelas.py:27`, `app/ponto/janelas.py:30`, `app/ponto/janelas.py:71`.

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `dia_inicio_competencia` | Formulário valida/clampa e chama a porta; porta persiste. `app/colaboradores/views.py:495`, `app/colaboradores/views.py:523`, `app/colaboradores/views.py:538`, `app/colaboradores/services/estrutura.py:12`, `app/colaboradores/services/estrutura.py:17`. | `corte_da_empresa`; default 21 e clamp 2–28. `app/ponto/janelas.py:50`. |
| Janela corrente | `periodo_apuracao_atual` → `periodo_apuracao`. `app/ponto/janelas.py:24`. | `janela_atual`; C5 e lavrador. `app/ponto/janelas.py:60`, `app/escala/services/cadastro_realidade.py:174`, `app/folha/management/commands/lavrar_retrato_folha.py:60`. |
| Janela anterior | `janela_anterior` chama corrente no dia anterior ao início. `app/ponto/janelas.py:71`. | C5 lê início; C5/lavrador leem fim para competência. `app/escala/services/cadastro_realidade.py:176`, `app/escala/services/cadastro_realidade.py:183`, `app/folha/management/commands/lavrar_retrato_folha.py:61`. |
| Competência de `RetratoFolha` | Lavrador grava `(empresa, mes, ano)` do fim. `app/folha/management/commands/lavrar_retrato_folha.py:74`. | C5 consulta mesmo par. `app/escala/services/cadastro_realidade.py:185`. |
| Janela do conteúdo lavrado | `montar_linhas` → `fora_da_folha` → `dias_que_seguram`, que chama `janela_fechamento` e export. `app/folha/retrato_folha.py:1157`, `app/folha/fora_da_folha.py:118`. | Export delega ao mesmo juiz; retentor recebe essas bordas. `app/folha/export.py:47`, `app/folha/export.py:807`, `app/folha/fora_da_folha.py:137`. |
| Janela do fechamento | Juiz `janela_fechamento`. `app/ponto/janelas.py:54`. | Universo e recalculador. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:545`, `app/ponto/services/fechamento.py:612`. |

Exemplos corte 21, calculados pelas funções reais: em 20/10/2026, competências 09/10; em 21/10, 10/11; em 21/12, 12/2026 e 01/2027; em 01/01/2027, mesmo par; em 21/01, 01/02 de 2027. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:66`.

**Limitação — concordância de calendário não certifica contemporaneidade.** Lavrador fixa a data real, aceita competências explícitas e sobrescreve o único retrato da chave; C5 aceita `hoje` arbitrário e consulta o retrato existente sem seleção por `lavrado_em`. Portanto, `hoje` retroativo não recupera uma fotografia histórica. `app/folha/management/commands/lavrar_retrato_folha.py:48`, `app/folha/management/commands/lavrar_retrato_folha.py:55`, `app/folha/management/commands/lavrar_retrato_folha.py:74`, `app/folha/models.py:363`, `app/escala/services/cadastro_realidade.py:169`, `app/escala/services/cadastro_realidade.py:185`.

O vigia seleciona retratos pela última **data de lavra**, enquanto C5 seleciona competências pelo `hoje` recebido; esses universos podem divergir, inclusive antes da nova lavra no dia do corte. A C5 então deixa o desligado sem dias da competência ainda não lavrada. `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:43`, `app/escala/services/cadastro_realidade.py:181`, `app/escala/services/cadastro_realidade.py:190`. Ocorrência em produção: **[nao verificado]**.

## 06/11 18:12 · sessao 800b74a61b49 · - · - · origem=Ronald (tmux)

**Q1 — cura comprovada por sonda em memória; execução Django [nao verificado].** O filtro está em `app/escala/services/cadastro_realidade.py:176`; o teste novo está em `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:278`.

| Mutante simples | Teste novo isolado | Prova | [nao verificado]
|---|---|---|
| Remover `data__gte` | Distingue | Retorna 20/09 e 21/09, exigia só 21/09. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279` |
| `gte` → `gt` | Distingue | Retorna vazio. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:280` |
| Usar início de `janela_atual` | Distingue | Retorna vazio; início atual 21/10. `app/escala/services/cadastro_realidade.py:174` |
| Remover `data__lte` | Sobrevive | Ambas as datas do fixture são anteriores a ontem. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279` |
| `lte` → `lt` | Sobrevive | Fixture não contém ontem. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:279` |

Os dois mutantes superiores são distinguidos pelos testes ativos já existentes: `_ativo` cria HOJE e ontem=24/10, e `ESPERADO` inclui 24/10 e exclui HOJE. Logo os testes desse arquivo, em conjunto, distinguem todos os mutantes simples listados; sobrevivência à suíte integral [nao verificado]. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:261`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:269`.

**Q2 — BAIXA, lacunas remanescentes do selo; nenhum defeito funcional atual demonstrado.** Extraí e executei `_veredito` em memória, sem importar Django; ele percorre somente a função localizada por `_fn` e reconhece strings posicionais apenas para os nomes enumerados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:296`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:328`.

| Forma sondada | Resultado do detector |
|---|---|
| `Q(data_demissao__isnull=False)` | Detecta `data_demissao`; kwargs de qualquer chamada são examinados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332` |
| `Q(**{"data_demissao__isnull": False})` | Detecta; cura do delta funciona. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334` |
| `Q(("data_demissao__isnull", False))` | Não detecta; não percorre strings de tuplas posicionais. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:329` |
| `kw={"data_demissao__isnull":False}; Q(**kw)` | Não detecta; exige `ast.Dict` diretamente no kwarg. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:334` |
| `F("data_demissao")` | Não detecta; `F` não pertence à lista. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:296` |
| `values(d=F("data_demissao"))` | Não detecta; inspeciona o nome `d`, não o campo dentro de `F`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332` |
| `values("colaborador__data_demissao")` | Não detecta; string posicional comparada inteira, sem dividir `__`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330` |
| `from operator import attrgetter as ag; ag("data_demissao")(c)` | Não detecta campo; usa nome sintático `ag`, sem resolver o apelido para o detector de campos. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:315`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:328` |
| `from folha import export as e; e.classificar_export(...)` | Não detecta proibida nem módulo proibido: registra `folha`, e chamadas por atributo não entram em `chamadas`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:313`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322` |
| `import folha.export as e; e.classificar_export(...)` | Bloqueia pelo módulo, mesmo sem reconhecer chamada. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:317`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:343` |
| `from folha.export import classificar_export as e; e(...)` | Detecta pelo mapa de apelidos. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:315`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:322` |
| Helper externo que consulta campo/juiz | Não segue corpo externo: `_fn` entrega somente o AST de `colabs_nunca_bateu`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:311` |

**Relevância:** esses pontos cegos permitem introduzir futuramente predicado paralelo na consulta de desligados ou mover a recomposição para helper mantendo o selo verde; não provam que isso ocorre no corpo atual, que consulta somente situação e consome dias do retrato. O teste de cadastro torto combate especificamente a exclusão por demissão nula, e os testes sem retrato/divergência combatem recomposição simples; o selo continua sem provar fluxo dos dados da chamada até o retorno. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:192`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:154`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:341`.

## 06/11 18:12 · sessao 6edb84ac9a89 · - · - · origem=Ronald (tmux)

Inventário operacional de `Ausencia.minutos` na cópia:

| Sítio | Valor / efeito |
|---|---|
| `app/ponto/models.py:755` | Campo com `default=0`. |
| `app/ponto/models.py:877` | `save` deriva **somente** se `status=='aprovada'`, `tipo=='falta'` e minutos falsy; soma `minutos_previstos_do_dia` no range e atribui em `:887`. Minutos positivos permanecem. |
| `app/ponto/services/ausencia.py:130` | `criar_ausencia(..., minutos=0)`. Repassa o argumento ao `kwargs` em `:343`, instancia em `:385` e grava em `:418`. |
| `app/ponto/services/ausencia.py:508` | `editar_ausencia_registro`: `ausencia.minutos=int(minutos or 0)`; `save()` em `:509`. |
| `app/ponto/services/ausencia.py:834` | `mudar_estado` inclui `minutos` em `update_fields`; grava em `:835`, permitindo persistir a derivação do modelo. `decidir` chama essa porta em `:911`. |
| `app/ponto/services/ausencia.py:1296` | `rederivar_fim` faz `save()` completo e pode, portanto, persistir a derivação do modelo. Seu chamador operacional é `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`. |
| `app/ponto/views.py:2415` | Lançamento admin: `minutos=minutos` em `:2417`, obtido do POST pelo parser em `:2388`; inválido degrada a zero em `:2390`. |
| `app/ponto/views.py:2536` | Submissão PWA: `minutos=_min` em `:2539`, obtido do POST em `:2497`. |
| `app/api/views.py:1820` | API: `minutos=_min` em `:1824`, obtido de `request.data` em `:1815`. |
| `app/ponto/views.py:2794` | Edição da tela passa `minutos=minutos` em `:2801`, lendo POST em `:2784` e convertendo em `:2787`. |
| `app/ponto/services/checar_ausencia.py:46` | Copiloto chama criação aprovada sem argumento minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/ponto/services/veredito_celula.py:315` | Veredito cria aprovada sem argumento minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/chamados/services/materializacao.py:484` | Declaração de pergunta: omite minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/chamados/services/materializacao.py:520` | Ausência da resposta: omite minutos; recebe default zero de `app/ponto/services/ausencia.py:130`. |
| `app/ponto/services/ausencia.py:1202` | Sincronização de férias passa `minutos=0` em `:1204`. |
| `app/ponto/services/ausencia.py:453` | Wrapper `garantir_ausencia(**kw)` repassa todos os argumentos a `criar_ausencia` em `:461`. |
| `app/ponto/services/regularizacao.py:118` | Chama esse wrapper com `minutos=0` em `:119`. |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | Chama o wrapper com `minutos=0` em `:80`. |
| `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169` | Chama edição com `minutos=alvo` em `:170`; aplicação usa `alvo=None` no plano em `:244`; reversão usa os minutos anteriores do snapshot em `:241`. |

**Nenhum desses escritores operacionais conserva literal não zero, como `720`.** Os dois sítios de materialização atuais omitem minutos, com a retirada do literal documentada em `app/chamados/services/materializacao.py:487` e `:522`; os demais passam zero, argumento do usuário, valor encaminhado ou snapshot, conforme tabela. A varredura AST de produção desta leitura percorreu a raiz, inclusive arquivos fora de `app`, sem outro construtor/escritor operacional encontrado; essa conclusão de completude é `[nao verificado]` quanto a mecanismos dinâmicos que a busca estática não revela.

**Testes ainda contêm números fixos**, por exemplo `720` em `app/ponto/tests/test_h12h06b_rederivar_minutos.py:71`, `:73`, `:337`, `:349`; também há atualizações diretas de fixture em `:190`, `:204`, `:301`. O selo que procura literais não zero na criação operacional está em `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:34`, com exclusão explícita de testes e migrations em `:55`; não o executei.

**Formulário/admin:** `AusenciaForm` herda de `forms.Form`, em `app/ponto/forms.py:41`, e não grava como `ModelForm`. O admin herda de `SoLeitura`, em `app/ponto/admin.py:38`; o mixin recusa adicionar/alterar/apagar em `app/core/admin.py:59`, `:62`, `:65` e torna os campos somente leitura em `:68`.

**Limite temporal:** os `720` do dado fornecido não identificam quem escreveu cada registro nem demonstram execução do código atual; autoria histórica individual permanece `[nao verificado]`.

## 06/11 18:11 · sessao 6c411e286cf9 · - · - · origem=Ronald (tmux)

A porta existe, mas não é aplicada em todos os caminhos; estar no produtor único não significa passar pelo veto. O construtor está em `app/ponto/services/ausencia.py:385`, e o veto final só roda quando `se_existir == 'veto'`, em `app/ponto/services/ausencia.py:377`.

**Regra exata da porta**

- `ausencias_sobrepostas` consulta ausências **não rejeitadas**, portanto inclui aprovadas e abertas; considera intervalo fechado, início dentro da janela e intervalo aberto somente nos tipos autorizados, ordenando por `data, pk`. `app/ponto/turnos.py:2815`, `app/ponto/turnos.py:2819`.
- `linha_que_veta` percorre todas e devolve a primeira que `abona_sobre_falta` não absolver. `app/ponto/precedencia.py:145`.
- A exceção é assimétrica: só absolve quando a existente tem `tipo == 'falta'` e o tipo novo pertence a `abonam_vigentes()`. Logo, abono novo sobre falta pode entrar; falta nova sobre abono não recebe essa exceção. `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:131`.
- No criar padrão, `linha_que_veta` é consultada em `app/ponto/services/ausencia.py:323`; havendo veto, levanta `AusenciaInvalida` em `app/ponto/services/ausencia.py:326`. O veto final também chama esse juiz, em `app/ponto/precedencia.py:461`.
- A criação pode nascer diretamente aprovada após essas verificações: `status` vai para os argumentos em `app/ponto/services/ausencia.py:344`; carimbos da aprovação são preenchidos em `app/ponto/services/ausencia.py:347`.

**Criadores e comportamento**

| Caminho | Referência | Passa pelo veto de sobreposição? |
|---|---|---|
| Lançamento admin, já aprovado | `app/ponto/views.py:2415`, `app/ponto/views.py:2420` | Sim, modo padrão |
| Submissão PWA | `app/ponto/views.py:2536` | Sim, modo padrão |
| API app | `app/api/views.py:1820`, `app/api/views.py:1826` | Novo fato: sim; mesmo fato vivo é devolvido antes, sem criar (`app/ponto/services/ausencia.py:246`) |
| Copiloto, já aprovado | `app/ponto/services/checar_ausencia.py:46` | Sim, modo padrão |
| Veredito da célula, já aprovado | `app/ponto/services/veredito_celula.py:315` | Sim, modo padrão |
| Espelhamento de férias, já aprovado | `app/ponto/services/ausencia.py:1202` | Sim, modo padrão; recusas são registradas em `app/ponto/services/ausencia.py:1209` e encaminhadas à pauta em `app/ponto/services/ausencia.py:1215` |
| Regularização | `app/ponto/services/regularizacao.py:118` | Usa `garantir_ausencia` → `retornar`; conflito devolve existente e não cria (`app/ponto/services/ausencia.py:461`, `app/ponto/services/ausencia.py:312`) |
| Abandono provisório | `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | Mesmo comportamento `retornar` |
| Declaração validada de disputa | `app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:490` | **Não**: `retornar_mesmo_tipo` |
| Materialização da resposta de disputa | `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:526` | **Não**: `retornar_mesmo_tipo`; recebe `status` em `app/chamados/services/materializacao.py:525` |

O desvio das duas materializações é executável: o modo `retornar_mesmo_tipo` só procura mesmo colaborador/tipo/data; se não achar, define `conflito = None` e `_sobrepostas = none()`, evitando `linha_que_veta`. Depois evita também o veto final porque o modo não é `veto`. `app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:302`, `app/ponto/services/ausencia.py:377`.

**Aprovação posterior não consulta essa porta**

- Aprovação em lote usa `gate_aprovacao_ausencia` e chama `_a4_decidir`. `app/ponto/views.py:2283`, `app/ponto/views.py:2290`.
- Aprovação individual também usa esse gate e aprova os irmãos via `_a4_decidir`. `app/ponto/views.py:2607`, `app/ponto/views.py:2643`.
- Esse gate verifica contradição documento/desconto e aprovação sem documento; não verifica sobreposição. `app/ponto/services/ausencia.py:619`, `app/ponto/services/ausencia.py:628`.
- `_a4_decidir` delega a `decidir`, que chama `mudar_estado`; esta verifica a transição, atribui o status e salva, sem `linha_que_veta` ou `veto_de_lancamento`. `app/ponto/views.py:85`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:820`, `app/ponto/services/ausencia.py:824`, `app/ponto/services/ausencia.py:835`.
- Portanto, o abono que entrou como aberto sobre uma falta pode ser aprovado mantendo ambas as linhas aprovadas: a criação aceita a exceção, e a aprovação não rejeita/substitui a falta nem consulta colisão. `app/ponto/precedencia.py:127`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:824`.

**Outros escritores sem esse juiz**

- `editar_ausencia_registro` não usa `linha_que_veta`; tem veto próprio mais restrito: qualquer `ausencia_sobrepoe`, excluindo a própria, recusa a edição. `app/ponto/services/ausencia.py:487`, `app/ponto/services/ausencia.py:491`.
- `reverter_rejeicao` chama `_reabrir` diretamente; `_reabrir` muda para estado aberto sem consulta de sobreposição. `app/ponto/services/ausencia.py:1102`, `app/ponto/services/ausencia.py:1141`.
- `rederivar_fim` simplesmente chama o `save`; o comando de cura usa esse escritor. `app/ponto/services/ausencia.py:1296`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`.
- `Ausencia.save` deriva carimbo/minutos/range, sem veto de sobreposição. `app/ponto/models.py:868`, `app/ponto/models.py:877`, `app/ponto/models.py:894`.
- A constraint declarada impede apenas duplicidade viva de **colaborador/tipo/data inicial**, não interseção de intervalos nem dois tipos diferentes no mesmo dia. `app/ponto/models.py:827`.
- Admin Django de Ausencia está em modo `SoLeitura`. `app/ponto/admin.py:38`.

O dado raspado não identifica o caminho de escrita que produziu cada registro; atribuir os casos concretos a algum desses escritores é `[nao verificado]`.

## 06/11 18:11 · sessao b8bae2adde14 · - · - · origem=Ronald (tmux)

O fechamento tem dois caminhos distintos:

- **Abono/supressão e motor:** `fechamento.py` chama `cobertura_ausencia_periodo(..., para='folha')` e aplica o efeito da vencedora; ABONA justifica, DESCONTA conserva o previsto, outros efeitos suprimem. Referências: `app/ponto/services/fechamento.py:671-692`. `efeitos_da_ausencia` usa a mesma cobertura para justificadas/suprimidas, embora some créditos parciais num laço sobre todas. Referências: `app/ponto/services/efeito_ausencia.py:47-62`.
- **Horas de falta gravadas:** NÃO usa somente aquela vencedora. `fechamento.py:864-866` chama `folha.export.minutos_de_falta_por_dia`; essa função percorre **todas** as aprovadas dos tipos `descontam_vigentes()` e soma minutos por dia, sem testar se outro abono cobre o mesmo dia. Referências: `app/folha/export.py:87-92`, `app/folha/export.py:138-143`. Rateia cada registro pelo número integral de dias: `app/folha/export.py:113-122`; a expansão delega a `dias_da_ausencia`: `app/folha/export.py:63-70`. O mapa alimenta a lavratura em `app/ponto/services/fechamento.py:1189`, `app/ponto/services/fechamento.py:1265`; `DiaPago.horas_falta = minutos/60` e `detalhe.falta_min = round(minutos,2)` em `app/ponto/services/dia_pago.py:272-275`.

**Critério da cobertura:** para folha só aprovada (`app/ponto/turnos.py:2514-2515`); ordena por `data, pk` crescentes e sobrescreve o mapa, vencendo maior `data`, depois maior `pk`. Referências: `app/ponto/turnos.py:2753-2769`. Isso não é ordem de `criado_em` ou `aprovada_em`; os campos efetivamente usados são `data,pk` (`app/ponto/turnos.py:2757`). A consulta singular `ausencia_cobre` implementa o mesmo critério com `order_by('-data','-pk').first()` (`app/ponto/turnos.py:2552-2557`), mas é outra função.

**Cobertura de dias:** fechado usa `data_fim`; sem fim, tipos `TIPOS_EM_ABERTO` alcançam o fim consultado; demais cobrem apenas `data`, limitado à janela. Referências: `app/ponto/turnos.py:2760-2770`, `app/ponto/turnos.py:2779-2789`.

**A2 / `fatos_do_dia` que aparece em `tela` na sonda:**

- Delega a `fatos_do_periodo`: `app/ponto/precedencia.py:256`.
- Inclui todas não rejeitadas, inclusive pendentes; ordenação da consulta somente por `data` crescente: `app/ponto/precedencia.py:273-279`.
- Monta as candidatas e ordena pela faixa, escolhendo a primeira: `app/ponto/precedencia.py:379-397`. Ordem: férias, afastamento, atestado, feriado, folga, trabalho (`app/ponto/precedencia.py:45-46`).
- Dentro da mesma faixa conserva a ordem recebida; data mais antiga chega antes. Entre candidatas com mesma data e faixa, não há desempate explícito por `pk` (`app/ponto/precedencia.py:279`, `app/ponto/precedencia.py:396-397`); um vencedor exato nesses empates é `[nao verificado]`.
- `falta`, `folga`, `folga_abonada` não estão no mapa especial e caem na faixa padrão `atestado`; `folga_compensatoria` está explicitamente na faixa `folga`. Referências: `app/ponto/precedencia.py:52-64`, `app/ponto/precedencia.py:79`. O `tipo_do_dia` conserva o código, mas `rotulo` vem da faixa; por isso código `falta` pode aparecer com faixa `atestado` e rótulo `Atestado`. Referências: `app/ponto/precedencia.py:66-68`, `app/ponto/precedencia.py:403-405`.

**Espelho completo / `dia_decidido`: cuidado para não igualar à saída A2 da sonda.**

- A montagem efetiva do espelho carrega cobertura de folha pela **mesma função de `turnos`**: `app/ponto/services/espelho.py:588`; carrega A2 e decisões separadamente: `app/ponto/services/espelho.py:646-649`.
- Encaminha a cobertura a `do_dia`: `app/ponto/services/espelho.py:331-342`.
- `dia_decidido.veredito_do_dia` prioriza a cobertura, antes dos fatos A2: `app/ponto/services/dia_decidido.py:363-364`; `do_dia` usa `rotulo_de(cobertura)` quando ela existe: `app/ponto/services/dia_decidido.py:428-429`.
- `rotulo_de` usa o efeito vigente e os minutos daquela ausência: `app/ponto/services/dia_decidido.py:468-481`, `app/ponto/services/dia_decidido.py:491-497`; para desconto monta “... (desconta ...h)” (`app/ponto/services/dia_decidido.py:219-221`).
- `decididos_do_periodo`, outro mapa para histórico/recusas, também ordena `data,pk` e sobrescreve; inclui rejeitadas e usa `data_fim or data`, sem estender tipos abertos até o fim consultado. Referências: `app/ponto/services/dia_decidido.py:547-562`.

Portanto, **A2 e cobertura são funções diferentes com critérios diferentes**; **palavra final do espelho e cobertura usada pelo motor compartilham `cobertura_ausencia_periodo`**; **valor de falta gravado soma todas as aprovadas que descontam**, independentemente das duas escolhas. Referências nos pontos acima. O trecho `tela` fornecido não contém a palavra final de `dia_decidido`; o resultado real dessa palavra nos exemplos é `[nao verificado]`.

As tuplas EX não mostram `data/data_fim` de cada ausência; inferir qual `pk` ganha cobertura usando somente o timestamp exibido seria `[nao verificado]`.

## 06/11 18:11 · sessao 23b0f53d0635 · ESPURIA · sonda · origem=Code

**Não é possível certificar integralmente a sonda nem explicar o cadastro do PAR com este material:** não localizei `sonda_espuria2.py` na cópia, e a saída fornecida contém `PAR_ACHADOS {}`. [nao verificado]

1. **Leituras e eventual repetição de regra**

A correção das chaves está de acordo com a porta real: `rubricas_do_dia` devolve `extras_50` e `extras_100`, lendo os totais do `DiaPago`; o espelho chama essa função. [app/ponto/services/dia_pago.py:912](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:912), [app/ponto/services/espelho.py:813](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/espelho.py:813)

Os pontos que precisam ser conferidos no código da sonda são:

| Número | Fonte real / regra que seria repetida |
|---|---|
| `he` | A porta já fornece `minutos_extra = (extras_50 + extras_100) * 60`. Somar os dois totais novamente repete essa composição, embora não reapure os períodos. [app/ponto/services/dia_pago.py:925](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:925) |
| `he_fer`, `folga_trab` | São campos próprios do `DiaPago`; lê-los diretamente não exige reconstruir o enquadramento. [app/ponto/models.py:331](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/models.py:331), [app/ponto/models.py:333](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/models.py:333) |
| `pago` | A autoridade é `pago_do_dia`: verifica sinal nas rubricas e soma trabalhadas, folga trabalhada e `folga_feriado_min/60`. Copiar essa expressão ou seu teste de ausência seria repetir regra. [app/ponto/services/dia_pago.py:888](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:888) |
| `n_bat` | `batidas_apuraveis` aplica o filtro de retratação, colaborador e janela inclusiva. A atribuição ao dia da jornada é outra porta: `dia_das_batidas`. Agrupar tudo apenas pela data civil pode divergir dela. [app/ponto/turnos.py:2863](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/turnos.py:2863), [app/ponto/turnos.py:2908](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/turnos.py:2908) |
| `min_real` | Há duas origens a distinguir: o campo persistido do `DiaPago`, alimentado pelo mapa da grade, e o relógio do espelho, produzido por `realizado_do_dia` com vínculo e intervalo do dia. [app/ponto/services/dia_pago.py:286](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:286), [app/ponto/services/espelho.py:286](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/espelho.py:286), [app/ponto/services/espelho.py:783](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/espelho.py:783) |

**Arquivo:linha de repetição dentro da própria sonda: [nao verificado].** O cabeçalho e os resultados fornecidos não permitem confirmar quais dessas expressões ela executou. [nao verificado]

`sem_pago` também não significa necessariamente “sem lavratura”: `pago_do_dia` retorna `None` para linha ausente **ou** sem sinal monetário; `rubricas_do_dia` distingue ausência por `lavrada=False`. [app/ponto/services/dia_pago.py:888](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:888), [app/ponto/services/dia_pago.py:908](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/dia_pago.py:908)

2. **Saída fornecida, em tabelas**

Os registros abaixo são transcrição do dado raspado; não houve confirmação independente em banco ou log. [nao verificado]

`S2_DIN sem_trab`:

| pago | HE | fer | FT | dia | batidas | relógio | n | Evidência |
|---|---|---|---|---|---|---|---:|---|
| sem_pago | he0 | fer0 | ft0 | feriado | bat=2+ | real0 | 16 | [nao verificado] |
| sem_pago | he0 | fer0 | ft0 | util | bat=1 | real0 | 22 | [nao verificado] |
| pago | he0 | fer0 | ft0 | util | bat=1 | real0 | 32 | [nao verificado] |
| pago | he0 | fer0 | ft0 | feriado | bat=2+ | real0 | 15 | [nao verificado] |
| pago | he0 | fer0 | ft>0 | util | bat=2+ | real0 | 1 | [nao verificado] |
| sem_pago | he0 | fer0 | ft0 | util | bat=2+ | real0 | 36 | [nao verificado] |
| pago | he0 | fer0 | ft0 | util | bat=2+ | real0 | 43 | [nao verificado] |
| pago | he0 | fer0 | ft0 | feriado | bat=1 | real0 | 2 | [nao verificado] |
| sem_pago | he0 | fer0 | ft0 | feriado | bat=1 | real0 | 2 | [nao verificado] |
| pago | he0 | fer0 | ft0 | util | bat=2+ | real>0 | 1 | [nao verificado] |

`S2_ESTR_DIN n=72 com_2+_batidas=50 com_relogio>0=0`. [nao verificado]

Nos oito `S2_ESTR_DIN_EX`, são comuns: `pago=None`, `trabalhadas=0.0`, `he=0.0`, `he_fer=0.0`, `folga_trab=0.0`, `fer_min=0.0`. [nao verificado]

| col | dia | status | feriado | fer_vira_folga | trab_feriado | n_bat | n_orfas | min_real | lavrada | dono | Evidência |
|---|---|---|---|---|---|---:|---:|---|---|---|---|
| col43 | 2026-11-02 | folga_trabalhada | True | True | False | 2 | 2 | 0.0 | True | CADASTRO | [nao verificado] |
| col97 | 2026-11-02 | folga_trabalhada | False | None | None | 1 | 1 | 0.0 | True | CADASTRO | [nao verificado] |
| col97 | 2026-11-03 | escala_suspeita | False | None | None | 1 | 1 | None | False | CADASTRO | [nao verificado] |
| col97 | 2026-11-05 | escala_suspeita | False | None | None | 1 | 1 | 0.0 | True | CADASTRO | [nao verificado] |
| col207 | 2026-11-02 | folga_trabalhada | True | True | False | 2 | 2 | 0.0 | True | ESTRUTURA | [nao verificado] |
| col227 | 2026-11-03 | folga_trabalhada | False | False | True | 4 | 1 | None | False | BATIDA | [nao verificado] |
| col242 | 2026-11-02 | folga_trabalhada | True | True | False | 4 | 4 | 0.0 | True | ESTRUTURA | [nao verificado] |
| col270 | 2026-10-22 | folga_trabalhada | False | False | True | 4 | 4 | 0.0 | True | ESTRUTURA | [nao verificado] |

Portanto, `com_relogio>0=0` não deve ser reescrito como “todos têm relógio medido em zero”: os exemplos incluem `min_real=None`. [nao verificado]

Para apresentar todas as `S2_D0211` sem repetir campos, uso estas abreviações da transcrição. [nao verificado]

- Perfil **A**: `feriado=True`, `trab_feriado=False`, `fer_vira_folga=True`; **B**: `True, True, False`; **C**: `False, None, None`. [nao verificado]
- `ST` corresponde a `sem_trabalho`; `FT` a `folga_trabalhada`; donos `CAD/EST/BAT` correspondem a `CADASTRO/ESTRUTURA/BATIDA`. [nao verificado]
- Em todas essas linhas: `folga_trab=0.0`, `fer_min=0.0`; `trabalhadas` coincide com `pago` quando este é numérico, e vale `0.0` quando `pago=None`. Na coluna `he_fer`, **=pago** conserva o número integral da coluna `pago`. [nao verificado]

| col | ST | status | perfil | ec | n_bat | min_real | pago | he | he_fer | dono | Evidência |
|---|---|---|---|---:|---:|---:|---|---:|---|---|---|
| col43 | True | FT | A | 1411 | 2 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col97 | True | FT | C | None | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col203 | True | FT | A | 1409 | 4 | 0.0 | 7.069715555555556 | 0.0 | 0.0 | CAD | [nao verificado] |
| col207 | True | FT | A | 1452 | 2 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col227 | False | irregular | B | 195 | 1 | 533.0 | 8.893322777777776 | 0.0 | 0.0 | BAT | [nao verificado] |
| col242 | True | FT | A | 1398 | 4 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col278 | True | FT | A | 1350 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col282 | False | ok | B | 247 | 2 | 659.0 | 10.971358055555555 | 0.0 | 0.0 | EST | [nao verificado] |
| col296 | False | irregular | B | 261 | 0 | 345.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col300 | False | irregular | B | 1459 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col366 | True | FT | B | 1165 | 1 | 0.0 | 10.998848333333335 | 0.0 | 0.0 | EST | [nao verificado] |
| col369 | True | FT | A | 1313 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col371 | False | ok | B | 324 | 4 | 416.0 | 5.010742500000001 | 5.01 | =pago | CAD | [nao verificado] |
| col384 | True | FT | A | 1347 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col451 | False | irregular | B | 1485 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col476 | True | FT | A | 1451 | 4 | 0.0 | 7.448265966111111 | 0.0 | 0.0 | EST | [nao verificado] |
| col569 | False | ok | B | 484 | 4 | 406.0 | 6.652301132499999 | 6.65 | =pago | CAD | [nao verificado] |
| col583 | True | FT | A | 1287 | 4 | 0.0 | 8.078643055555556 | 0.0 | 0.0 | CAD | [nao verificado] |
| col617 | True | FT | A | 1313 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col622 | True | FT | A | 1251 | 4 | 0.0 | 6.974738382777777 | 0.0 | 0.0 | CAD | [nao verificado] |
| col655 | False | irregular | B | 1275 | 2 | 240.0 | 2.305491111111111 | 2.31 | =pago | CAD | [nao verificado] |
| col707 | False | irregular | B | 1465 | 4 | 611.0 | 11.193095 | 0.0 | 0.0 | CAD | [nao verificado] |
| col784 | True | FT | A | 1458 | 2 | 0.0 | 4.133950277777777 | 0.0 | 0.0 | EST | [nao verificado] |
| col788 | True | FT | B | 1301 | 8 | 0.0 | 16.0160475 | 0.0 | 0.0 | BAT | [nao verificado] |
| col789 | True | FT | A | 1472 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col820 | True | FT | A | 1380 | 1 | 0.0 | None | 0.0 | 0.0 | BAT | [nao verificado] |
| col822 | True | FT | A | 1360 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col864 | True | FT | A | 1494 | 4 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col866 | True | aberto | C | None | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col877 | False | irregular | B | 1337 | 4 | 660.0 | 10.165167222222221 | 0.0 | 0.0 | CAD | [nao verificado] |
| col932 | False | irregular | B | 1335 | 2 | 716.0 | 11.936174444444445 | 0.0 | 0.0 | CAD | [nao verificado] |
| col944 | True | FT | A | 1348 | 3 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col961 | True | FT | A | 1348 | 4 | 0.0 | 7.146785555555555 | 0.0 | 0.0 | CAD | [nao verificado] |
| col962 | True | FT | A | 1303 | 2 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col967 | True | FT | A | 1316 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col968 | True | FT | A | 1358 | 4 | 0.0 | 6.9958141666666664 | 0.0 | 0.0 | EST | [nao verificado] |
| col979 | False | irregular | B | 1390 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col982 | False | irregular | B | 1394 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col985 | True | FT | A | 1418 | 2 | 0.0 | 4.017282222222223 | 0.0 | 0.0 | CAD | [nao verificado] |
| col986 | True | FT | B | 1419 | 4 | 0.0 | 11.057805930555556 | 0.0 | 0.0 | CAD | [nao verificado] |
| col988 | True | FT | A | 1422 | 4 | 0.0 | 7.342193096111111 | 0.0 | 0.0 | CAD | [nao verificado] |
| col990 | True | FT | A | 1424 | 4 | 0.0 | 7.317387678888889 | 0.0 | 0.0 | CAD | [nao verificado] |
| col991 | True | FT | A | 1425 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col998 | True | FT | B | 1450 | 4 | 0.0 | 10.450366262777777 | 0.0 | 0.0 | CAD | [nao verificado] |
| col1003 | True | FT | A | 1469 | 2 | 0.0 | None | 0.0 | 0.0 | EST | [nao verificado] |
| col60 | False | irregular | B | 1490 | 4 | 656.0 | 10.932136944444444 | 0.0 | 0.0 | CAD | [nao verificado] |
| col87 | False | ok | B | 802 | 4 | 445.0 | 4.435254722222222 | 4.44 | =pago | CAD | [nao verificado] |
| col114 | False | ok | B | 1036 | 4 | 439.0 | 6.332564722222222 | 6.33 | =pago | CAD | [nao verificado] |
| col129 | False | irregular | B | 853 | 2 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col502 | False | ok | B | 1150 | 4 | 440.0 | 4.812576055277777 | 4.81 | =pago | CAD | [nao verificado] |
| col749 | True | FT | A | 1374 | 4 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col868 | True | FT | B | 1377 | 1 | 0.0 | 11.870235555555555 | 0.0 | 0.0 | CAD | [nao verificado] |
| col900 | True | FT | A | 1341 | 1 | 0.0 | None | 0.0 | 0.0 | CAD | [nao verificado] |
| col996 | True | FT | A | 1446 | 4 | 0.0 | 13.019503333333333 | 0.0 | 0.0 | CAD | [nao verificado] |
| col1001 | True | FT | A | 1467 | 4 | 0.0 | 7.38249611111111 | 0.0 | 0.0 | CAD | [nao verificado] |

`PAR_ACHADOS {}`. [nao verificado]  
`ERROS {}`. [nao verificado]

3. **PAR: diferença cadastral que explica HE feriado 0 × 2,31**

O lado com `he=2.31` aparece como **col655**, com `he_fer=2.305491111111111`, `ec=1275`, `trab_feriado=True` e `fer_vira_folga=False`. O integrante com HE zero não foi identificado por `PAR_ACHADOS`; o anexo tampouco fornece escala efetiva/ciclo ou régua do col655. Assim, a diferença concreta de cadastro entre os dois é **[nao verificado]**.

O código comprova estes caminhos de transformação:

| Cadastro/condição | Transformação em rubrica |
|---|---|
| Vínculo do dia | `escala_vigente` delega a escolha a `vinculo_do_dia`, incluindo a geradora da célula quando existente. [app/escala/servico_jornada.py:38](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/escala/servico_jornada.py:38) |
| `trabalha_em_feriado` e `feriado_vira_folga` | Se trabalha em feriado, o predicado retorna falso; caso contrário, exige ciclo permitido e feriado no lugar do vínculo. [app/escala/models.py:1389](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/escala/models.py:1389) |
| Período de folga com escala certa | O fechamento primeiro filtra `periodos_ft` por `escala_certa_no_dia`; depois, nos elegíveis cujo vínculo declara `feriado_vira_folga`, transfere as horas para HE100 e HE100 feriado. [app/ponto/services/fechamento.py:919](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:919), [app/ponto/services/fechamento.py:935](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:935), [app/ponto/services/fechamento.py:945](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:945) |
| Folga sem escala certa | As horas entram em trabalhadas, sem aquele adicional. [app/ponto/services/fechamento.py:964](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/services/fechamento.py:964) |
| Feriado em período normal | O motor exige `feriado_em_dobra` e feriado no dia; um dos escritores coloca a duração em `minutos_extra_100_feriado`. [app/ponto/motor_calculo_v2.py:934](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:934), [app/ponto/motor_calculo_v2.py:2809](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:2809) |
| Ciclo e régua | O padrão de `feriado_em_dobra` vem do ciclo: `12x36` integra o conjunto de feriado simples; a fábrica CCT pode habilitar sua dobra por cláusula cadastrada. [app/ponto/motor_calculo_v2.py:3599](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:3599), [app/ponto/motor_calculo_v2.py:3669](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/motor_calculo_v2.py:3669), [app/core/regua_cct.py:477](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/core/regua_cct.py:477) |

Para col655, o dado imprime `fer_vira_folga=False`; portanto, ele não fornece evidência de enquadramento no ramo de folga-feriado de `fechamento.py:940`. Atribuir o contraste do PAR a “12x36 versus 6x1”, a uma CCT específica ou a outro vínculo seria **[nao verificado]**.

## 06/11 18:09 · sessao e58a772d7f48 · ESPURIA · sonda · origem=Code

O QUADRO fecha: **161 + 136 + 96 + 30 + 27 + 12 + 4 = 466**, igual a `UNIVERSO dias_com_orfa_na_ata=466` e `LINHAS 466`. Os vetores `sem_trab` somam **136 + 30 + 4 = 170**; o bloco DINHEIRO soma **93 + 72 + 3 + 1 + 1 = 170**. [nao verificado]

Não aparece campo literal `ERRO`; aparece `ERROS {}`. Há `CONCLUSAO_LIBERADA`. [nao verificado]

A linha a conferir como possível problema de instrumento é **`HE nos sem_trabalho {(False, False, False): 170}`**, pelos três indicadores zerados em todos os 170 dias; as AMOSTRA também exibem `he`, `he_fer` e `folga_trab` em `0.0`. Isso não comprova erro de instrumento. [nao verificado] O modelo define rubricas separadas `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado` e `horas_folga_trabalhada`. [app/ponto/models.py:328](/home/ronald/codex_ws/7c140052a45f90bc52eb5d68726782428fe9307c/app/ponto/models.py:328) O mapeamento dessas rubricas para os rótulos da sonda não foi conferido: não localizei `logs/espuria/sonda_espuria.py` nesta cópia. [nao verificado]

As transcrições abaixo são literais do texto fornecido. **Cada linha dos blocos recebe `[nao verificado]`**, indicado aqui fora dos blocos para preservar o conteúdo literal.

1. Agregados, DINHEIRO, ERROS e conclusão — [nao verificado]

```text
UNIVERSO dias_com_orfa_na_ata=466 (aval citou 477 em PROD 17:3x; aqui SOMBRA dump do dia)
ITEM3 {'celulas': 9845, 'dias_lampada_acesa_tipo_real_desconhecido': 578, 'dias_lampada_tipo_divergente': 200, 'dias_lampada_tipo_divergente_COM_orfa': 111, 'dias_lampada_tipo_divergente_SEM_orfa': 89, 'lampadas_tipo_divergente': 303, 'par_E_aceso_por_S': 166, 'par_S_aceso_por_E': 137}
LINHAS 466
QUADRO por vetor (sem_trabalho, lista CxR do motor, regenerada) -- soma=466
  trab/nao_cxr/nao_regen                   161
  sem_trab/nao_cxr/nao_regen               136
  trab/cxr/nao_regen                       96
  sem_trab/nao_cxr/regen                   30
  trab/nao_cxr/regen                       27
  trab/cxr/regen                           12
  sem_trab/cxr/nao_regen                   4
STATUS {'folga_trabalhada': 161, 'escala_suspeita': 6, 'ok': 107, 'irregular': 189, 'aberto': 3}
DONO {'CADASTRO': 279, 'ESTRUTURA': 109, 'BATIDA': 78}
DONO x vetor {('sem_trab/nao_cxr/nao_regen', 'CADASTRO'): 86, ('trab/nao_cxr/nao_regen', 'CADASTRO'): 66, ('sem_trab/nao_cxr/regen', 'ESTRUTURA'): 7, ('trab/nao_cxr/nao_regen', 'ESTRUTURA'): 64, ('trab/nao_cxr/nao_regen', 'BATIDA'): 31, ('trab/cxr/nao_regen', 'CADASTRO'): 88, ('sem_trab/nao_cxr/nao_regen', 'BATIDA'): 19, ('sem_trab/nao_cxr/nao_regen', 'ESTRUTURA'): 31, ('trab/nao_cxr/regen', 'ESTRUTURA'): 7, ('sem_trab/nao_cxr/regen', 'CADASTRO'): 16, ('trab/nao_cxr/regen', 'CADASTRO'): 9, ('trab/nao_cxr/regen', 'BATIDA'): 11, ('sem_trab/nao_cxr/regen', 'BATIDA'): 7, ('trab/cxr/regen', 'CADASTRO'): 12, ('trab/cxr/nao_regen', 'BATIDA'): 8, ('sem_trab/cxr/nao_regen', 'CADASTRO'): 2, ('sem_trab/cxr/nao_regen', 'BATIDA'): 2}
FERIADO {(True, False, True): 30, (False, None, None): 10, (False, True, False): 325, (False, False, False): 78, (True, True, False): 23}
ESCALA_ERRADA {None: 268, 'LIMBO': 66, 'A2': 122, 'A8': 10}
SUGESTAO_PROPOR_ESCALA (por dia) {'media': 191, 'insuficiente': 159, 'alta': 116}
--- DINHEIRO dos dias sem trabalho n=170
  pago / decisao_he=None / porta=None / sem_verbo_admin                            93
  sem_pago / decisao_he=None / porta=None / sem_verbo_admin                        72
  sem_pago / decisao_he=None / porta=porta_pergunta / sem_verbo_admin              3
  pago / decisao_he=autorizado / porta=None / sem_verbo_admin                      1
  sem_pago / decisao_he=nao / porta=None / sem_verbo_admin                         1
ESTRUTURA_DINHEIRO (sem pago E sem decisao E sem porta/verbo admin): 72
HE nos sem_trabalho {(False, False, False): 170}
ERROS {}
CONCLUSAO_LIBERADA
```

2. Todas as linhas ITEM3_EX_SEM_ORFA — [nao verificado]

```text
ITEM3_EX_SEM_ORFA ('col174', '2026-10-25', [('S', '02:00', '02:27', 'E')])
ITEM3_EX_SEM_ORFA ('col191', '2026-10-22', [('S', '16:00', '17:54', 'E'), ('E', '17:00', '18:59', 'S'), ('S', '23:00', '23:01', 'E')])
ITEM3_EX_SEM_ORFA ('col200', '2026-10-29', [('E', '22:00', '02:00', 'S')])
ITEM3_EX_SEM_ORFA ('col200', '2026-11-02', [('E', '22:00', '02:00', 'S'), ('S', '02:00', '02:00', 'E'), ('E', '03:00', '03:02', 'S')])
ITEM3_EX_SEM_ORFA ('col207', '2026-10-24', [('E', '13:00', '13:38', 'S')])
ITEM3_EX_SEM_ORFA ('col247', '2026-10-24', [('E', '23:00', '00:00', 'S')])
```

Todas as linhas AMOSTRA — [nao verificado]

```text
AMOSTRA sem_trab/nao_cxr/nao_regen {'col': 'col43', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 421, 'codigo': '34', 'descricao': '6x1milano', 'ciclo': '6x1', 'horario': '07:30-16:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1411, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/nao_regen {'col': 'col97', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': None, 'fer_vira_folga': None, 'ec': None, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/nao_regen {'col': 'col97', 'dia': '2026-11-03', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'escala_suspeita', 'tipo_dia': 'folga', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': None, 'fer_vira_folga': None, 'ec': None, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': False, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/nao_regen {'col': 'col174', 'dia': '2026-10-23', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1156, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 6.782049166666667, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/nao_regen {'col': 'col174', 'dia': '2026-10-24', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1156, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/nao_regen {'col': 'col174', 'dia': '2026-10-26', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1156, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/regen {'col': 'col207', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 573, 'codigo': '142', 'descricao': '[nome]', 'ciclo': '6x1', 'horario': '06:30-12:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1452, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/cxr/nao_regen {'col': 'col214', 'dia': '2026-11-01', 'emp': 2, 'sug': ("{'tipo_escala_id': 175, 'codigo': 'PAI-COMERCIAL.1', 'descricao': '6x1 - 12:40 - 17:00/18:00 - 21:00', 'ciclo': '6x1', 'horario': '12:40-21:00'}", 'alta'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 182, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.382154238055556, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/regen {'col': 'col270', 'dia': '2026-10-22', 'emp': 2, 'sug': ("{'tipo_escala_id': 252, 'codigo': 'PAI-12x36.55', 'descricao': '15:00 19:00 20:00 23:00 FOLGA QUINTA', 'ciclo': '12x36', 'horario': '15:00-23:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1449, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/nao_cxr/regen {'col': 'col270', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 252, 'codigo': 'PAI-12x36.55', 'descricao': '15:00 19:00 20:00 23:00 FOLGA QUINTA', 'ciclo': '12x36', 'horario': '15:00-23:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1449, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/nao_cxr/regen {'col': 'col278', 'dia': '2026-10-26', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': False, 'fer_vira_folga': False, 'ec': 1350, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': 6.836554444444444, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/nao_cxr/regen {'col': 'col278', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': False, 'fer_vira_folga': False, 'ec': 1350, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': 6.844773611111111, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/nao_cxr/regen {'col': 'col278', 'dia': '2026-10-30', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': False, 'fer_vira_folga': False, 'ec': 1350, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': 6.834293055555556, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/cxr/nao_regen {'col': 'col371', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 277, 'codigo': 'PAI-12x36.74', 'descricao': '12:00 16:00 17:00 20:00 folga segunda', 'ciclo': '12x36', 'horario': '12:00-20:00'}", 'alta'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': True, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 324, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 5.010742500000001, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/cxr/regen {'col': 'col373', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 382, 'codigo': 'PAI-6X1.ALPHAT', 'descricao': '6x1 - 12:50 - 14:40 - 15:50 - 21:20', 'ciclo': '6x1', 'horario': '14:00-22:20'}", 'media'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1497, 'escala_errada': 'LIMBO', 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 5.77680861111111, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA trab/cxr/regen {'col': 'col373', 'dia': '2026-10-30', 'emp': 2, 'sug': ("{'tipo_escala_id': 382, 'codigo': 'PAI-6X1.ALPHAT', 'descricao': '6x1 - 12:50 - 14:40 - 15:50 - 21:20', 'ciclo': '6x1', 'horario': '14:00-22:20'}", 'media'), 'sem_trabalho': False, 'status': 'ok', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1497, 'escala_errada': 'LIMBO', 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 6.099447777777778, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': 'nao', 'porta': None, 'verbo_admin': []}
AMOSTRA trab/cxr/nao_regen {'col': 'col382', 'dia': '2026-10-23', 'emp': 2, 'sug': ("{'tipo_escala_id': 547, 'codigo': '120', 'descricao': 'pgconcentrix', 'ciclo': '6x1', 'horario': '15:00-23:30'}", 'media'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1269, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.195496111111111, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA trab/cxr/regen {'col': 'col707', 'dia': '2026-10-29', 'emp': 2, 'sug': ("{'tipo_escala_id': 381, 'codigo': '12x36_DG.2', 'descricao': '12X36 SUPERVISAO CURITIBA  DAS 11:00 AS 23:00', 'ciclo': '12x36', 'horario': '11:00-23:00'}", 'media'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': True, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1465, 'escala_errada': None, 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 11.165899238888887, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
AMOSTRA sem_trab/cxr/nao_regen {'col': 'col788', 'dia': '2026-10-23', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1301, 'escala_errada': 'A2', 'cxr': True, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.057353611111112, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/cxr/nao_regen {'col': 'col788', 'dia': '2026-10-29', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1301, 'escala_errada': 'A2', 'cxr': True, 'dono': 'BATIDA', 'lavrada': True, 'pago': 0.8538388888888889, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
AMOSTRA sem_trab/cxr/nao_regen {'col': 'col788', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 1301, 'escala_errada': 'A2', 'cxr': True, 'dono': 'BATIDA', 'lavrada': True, 'pago': 16.0160475, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
```

Todas as linhas D0211 — [nao verificado]

```text
D0211 {'col': 'col43', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 421, 'codigo': '34', 'descricao': '6x1milano', 'ciclo': '6x1', 'horario': '07:30-16:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1411, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col97', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': False, 'trab_feriado': None, 'fer_vira_folga': None, 'ec': None, 'escala_errada': None, 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col203', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 305, 'codigo': 'PAI-12x36.92', 'descricao': '07:00 - 12:00/13:00 - 15:00', 'ciclo': '12x36', 'horario': '07:00-15:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1409, 'escala_errada': 'LIMBO', 'cxr': False, 'dono': 'CADASTRO', 'lavrada': True, 'pago': 7.069715555555556, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col207', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 573, 'codigo': '142', 'descricao': '[nome]', 'ciclo': '6x1', 'horario': '06:30-12:30'}", 'media'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1452, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col227', 'dia': '2026-11-02', 'emp': 2, 'sug': (None, 'insuficiente'), 'sem_trabalho': False, 'status': 'irregular', 'tipo_dia': 'trabalho', 'trabalha': True, 'regen': False, 'feriado': True, 'trab_feriado': True, 'fer_vira_folga': False, 'ec': 195, 'escala_errada': None, 'cxr': False, 'dono': 'BATIDA', 'lavrada': True, 'pago': 8.893322777777776, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': 'porta_fila', 'verbo_admin': []}
D0211 {'col': 'col242', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 277, 'codigo': 'PAI-12x36.74', 'descricao': '12:00 16:00 17:00 20:00 folga segunda', 'ciclo': '12x36', 'horario': '12:00-20:00'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': False, 'feriado': True, 'trab_feriado': False, 'fer_vira_folga': True, 'ec': 1398, 'escala_errada': None, 'cxr': False, 'dono': 'ESTRUTURA', 'lavrada': True, 'pago': None, 'he': 0.0, 'he_fer': 0.0, 'folga_trab': 0.0, 'decisao_he': None, 'porta': None, 'verbo_admin': []}
D0211 {'col': 'col278', 'dia': '2026-11-02', 'emp': 2, 'sug': ("{'tipo_escala_id': 457, 'codigo': '44x1', 'descricao': '6x1 - 07:50 - 11:10 - 12:10 - 15:50', 'ciclo': '6x1', 'horario': '07:50-15:50'}", 'alta'), 'sem_trabalho': True, 'status': 'folga_trabalhada', 'tipo_dia': 'folga', 'trabalha': False, 'regen': True, 'feriado': True, 'tra

[truncado em 20000 de 47416 caracteres]

## 06/11 18:08 · sessao c956e686389f · - · - · origem=Ronald (tmux)

Não é possível certificar a implementação da sonda: `sonda_espuria2.py` e seus emissores `S2_DIN`/`PAR_ACHADOS` não foram localizados na cópia pesquisada. Linha da própria sonda que replique regra: **[não verificado]**.

As fontes reais comprovadas:

- **Rubricas:** `rubricas_do_dia` lê `DiaPago.horas_extras_50` e `.horas_extras_100`, devolvendo `extras_50`/`extras_100`; `minutos_extra` é a soma desses totais ×60. Não devolve chaves `horas_extras_*`. `app/ponto/services/dia_pago.py:908-926`.
- **Uso em produção pelo código:** espelho importa e chama `linhas_do_periodo`, `pago_do_dia`, `rubricas_do_dia`; calendário também chama `rubricas_do_dia`. `app/ponto/services/espelho.py:406-410`, `app/ponto/services/espelho.py:811-813`, `app/colaboradores/services/calendario.py:655-656`.
- **HE feriado e folga trabalhada:** campos diretos do modelo. `app/ponto/models.py:331-333`.
- **Pago:** a porta `pago_do_dia` primeiro retorna `None` se não houver sinal em nenhuma rubrica monetária; senão soma trabalhadas + folga trabalhada + `detalhe['folga_feriado_min']/60`. Reproduzir esse critério na sonda seria duplicação; se isso ocorre nela é **[não verificado]**. `app/ponto/services/dia_pago.py:888-896`.
- **Batidas apuráveis:** porta aplica `retratada_em IS NULL`, os limites opcionais inclusivos e colaborador, ordenando por timestamp. Não aplica regra de dia da jornada. `app/ponto/turnos.py:2863-2871`.
- **Dia de cada batida:** existe porta própria `dia_das_batidas`; atribui batidas reclamadas ao `data_turno` e as restantes à data local. Agrupar manualmente todas por data civil não reproduziria essa atribuição; implementação da sonda **[não verificado]**. `app/ponto/turnos.py:2874-2905`, `app/ponto/turnos.py:2908-2919`.
- **Atenção a `min_real`:** o campo persistido `DiaPago.minutos_realizados` existe e é alimentado pelo mapa da grade; o relógio publicado no espelho vem de `realizado_do_dia`, passando pelo vínculo e intervalo do dia, e aparece como `minutos_realizados` no dicionário da tela. São origens que precisam ser distinguidas; a origem exata do `min_real` da sonda é **[não verificado]**. `app/ponto/models.py:351`, `app/ponto/services/dia_pago.py:283-290`, `app/ponto/services/espelho.py:286-294`, `app/ponto/services/espelho.py:675-676`, `app/ponto/services/espelho.py:783`, `app/ponto/turnos.py:772-793`.

As ausências são diferentes: `rubricas_do_dia(None)` entrega zeros com `lavrada=False`; `pago_do_dia` entrega `None` tanto para linha ausente quanto para linha com todas as rubricas monetárias zeradas. Portanto `sem_pago` sozinho não prova falta de lavratura. `app/ponto/services/dia_pago.py:888-891`, `app/ponto/services/dia_pago.py:908-914`.

## 06/11 18:08 · sessao 8f98d7cb6b29 · - · - · origem=Ronald (tmux)

Não é possível explicar o cadastro concreto do PAR: o dado termina com `PAR_ACHADOS {}`, não identifica os dois participantes e não fornece ciclo/tipo de escala nem régua de ambos. Atribuir a diferença a 12x36 versus 6x1 seria [não verificado].

O dado mostra col655 em 02/11 com `ec=1275`, `trab_feriado=True`, `fer_vira_folga=False`, `min_real=240`, `pago=2.305491111111111`, `he=2.31`, `he_fer=2.305491111111111`; a identidade do outro participante é [não verificado].

Caminhos comprovados no código:

- Vínculo do dia: `app/escala/servico_jornada.py:38-58` carrega célula e vínculos e delega a `vinculo_do_dia`; havendo geradora na célula, ela alimenta a decisão.
- `feriado_vira_folga`: `app/escala/models.py:1389-1395` retorna falso se `trabalha_em_feriado=True` ou escala ausente; exige ciclo permitido e feriado no lugar do vínculo. Ciclos permitidos são 6x1, 5x2 e semanal, em `app/escala/models.py:32`. Em `app/escala/models.py:1369-1371`, o predicado pode converter trabalho previsto em folga.
- Folga com adicional: `app/ponto/services/fechamento.py:919-926` filtra `resultado.periodos_ft` por `escala_certa_no_dia`. O juiz recusa LIMBO em `app/escala/services/escala_certa.py:31-33` e assinaturas A2/A8 em `app/escala/services/escala_certa.py:38-51`.
- Rubrica de feriado em folga: `app/ponto/services/fechamento.py:935-950` resolve o vínculo do dia, pergunta `feriado_vira_folga`, separa esses períodos e soma suas horas a HE100 e HE100 feriado. O resto elegível vai a folga trabalhada. O mesmo conjunto é entregue à lavratura de `DiaPago` em `app/ponto/services/fechamento.py:1184-1188`; `app/ponto/services/dia_pago.py:244-256` escreve HE100, HE100 feriado e `detalhe['folga_feriado_min']`.
- Folga sem escala certa: fica fora dos adicionais e entra em trabalhadas por `app/ponto/services/fechamento.py:964-969` e `app/ponto/services/dia_pago.py:257-265`.
- Feriado dos períodos normais: `app/ponto/motor_calculo_v2.py:934` exige `feriado_em_dobra` e feriado; um dos escritores da rubrica está em `app/ponto/motor_calculo_v2.py:2809-2812`, outro em `app/ponto/motor_calculo_v2.py:3092-3095`.
- O padrão `feriado_em_dobra` é decidido pelo **tipo_ciclo**, não pelo `tipo_base` ou nome da classe: `app/ponto/motor_calculo_v2.py:3623-3624,3669-3680`. Apenas `'12x36'` pertence ao conjunto padrão de feriado simples (`app/ponto/motor_calculo_v2.py:3599`). CCT pode habilitar dobra do 12x36 em `app/core/regua_cct.py:477-478`.
- Régua do motor: fechamento chama `get_motor_cct` com tipo da escala e início da fatia em `app/ponto/services/fechamento.py:725-732`. `regua_para` lê vínculo e praça do dia em `app/core/regua_cct.py:328-339`, regime da empresa em `app/core/regua_cct.py:352-366` e aplicação/CCT em `app/core/regua_cct.py:366-379`; `get_motor_cct` injeta os parâmetros em `app/core/regua_cct.py:458-478`.

Cuidado ao citar comentários como regra executável: há comentários antigos dizendo “feriado previsto não dobra” em `app/ponto/motor_calculo_v2.py:2807` e `app/ponto/motor_calculo_v2.py:3088-3091`, mas o predicado efetivamente chamado em `app/ponto/motor_calculo_v2.py:934` não consulta previsto; consulta apenas o flag de dobra e o feriado.

## 06/11 17:59 · sessao 837afbd0fb35 · E-SEM-VIVA-B · verificador · origem=Code

**As curas de produção estão corretas para os alvos apontados; a certificação continua parcial.** O filtro exige `furo_espelho`, o positivo corrente cobre a competência antes ausente e o corpo da C5 continua lendo a lavra, sem consultar demissão, admissão ou retentor. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:185`, `app/escala/services/cadastro_realidade.py:189`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:217`.

Execução da suíte Django, sobrevivência dos mutantes à suíte integral e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **MÉDIA — falta uma prova da fronteira inferior, inclusive para “ativo nunca-bateu igual”.**

   O mutante plausível que remove `data__gte=janelas[0][0]` deixa passar dias anteriores à janela. Por inspeção, os testes novos não o distinguem: suas células começam depois dessa fronteira, inclusive no corte 10. O teste do vigia com `hoje=01/09` continua excluindo 01/10 pelo limite **superior**. `app/escala/services/cadastro_realidade.py:176`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:33`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:250`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`, `app/folha/tests/test_vigia_retido_calado.py:133`.

   **Como provar:** ativo, corte 21, `hoje=25/10`, células `nunca_bateu` em 20/09 e 21/09; exigir somente 21/09. Remover o filtro inclui ambas; trocar `gte` por `gt` exclui ambas. Esse cenário distingue duas regressões na regra dos ativos. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:66`, `app/escala/services/cadastro_realidade.py:175`.

2. **BAIXA — a cura do selo ORM é correta, mas não completa.**

   O detector agora encontra `filter(data_demissao__isnull=False)`. Ainda ignora `filter(**{'data_demissao__isnull': False})`, porque a expansão tem `k.arg=None`, descartado pelo próprio filtro do AST. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:327`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:357`.

   **Como provar:** inserir essa expansão na consulta de desligados e executar `_veredito`: o campo fica fora de `campos`, satisfazendo indevidamente o selo. A sonda AST em memória confirmou isso; não significa sobrevivência comportamental, pois o novo teste de cadastro torto combate essa alteração específica. `app/escala/services/cadastro_realidade.py:179`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:335`.

   A chamada descartada à porta também continua suficiente para a exigência positiva do AST. Os testes de ausência de retrato e divergência lavra/vivo combatem a recomposição simples, mas o selo não certifica fluxo de dados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:154`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:317`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332`.

3. **BAIXA — descarte 1 defensável como CADASTRO; documentação ainda contraditória.**

   Com fechamento preexistente, retirar `data_demissao` não impede a nova lavra: `_K2` lê os fechamentos existentes, rescisão responde falso para data nula e o retentor não recorta o fim. A C5 acompanha essa resposta. O teste novo reproduz corretamente o histórico e exige igualdade com a autoridade. Isso sustenta retirar o caso da correção local da C5, sem criar predicado paralelo. `app/folha/export.py:775`, `app/folha/export.py:60`, `app/folha/export.py:1200`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/services/cadastro_realidade.py:189`.

   Entretanto, “sem `data_demissao` = fora” permanece escrito antes da ressalva sobre fechamento antigo. O primeiro enunciado precisa da condição de ausência de fechamento preexistente. `app/escala/services/cadastro_realidade.py:148`, `app/escala/services/cadastro_realidade.py:151`.

   **Como provar:** `_fechar()`, apagar a demissão, `_lavrar()`, consultar C5 — exatamente o histórico agora exercitado pelo teste. Uma obrigação adicional de excluir data nula pelo aval literal fornecido é **[nao verificado]**; o código atual não faz essa exclusão. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:228`, `app/escala/services/cadastro_realidade.py:179`.

4. **MÉDIA — descarte 3 sustenta a separação de escopo, mas a perda de competência permanece.**

   O vigia declara unidade **pessoa** e transforma C5 em conjunto de IDs. Portanto, a triagem pode tratar isso como granularidade preexistente; não pode concluir que o roteamento respeita a competência de cada linha. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

   **Como provar:** hoje 25/10, corte 21, desligado com demissão 25/11, fechamentos 10 e 11, `nunca_bateu` em 05/10 e vínculo cobrindo 21–24/10 sem células. A anterior pode ser retida pelo `nunca_bateu`; a corrente, por `sem_celula`, sem porta e com fio `ninguem`. C5 contém somente 05/10, mas o vigia roteia ambas as linhas pelo mesmo ID. O aval por dias/competência não fica demonstrado por esse contador. `app/folha/export.py:1231`, `app/folha/export.py:1233`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:86`, `app/folha/retrato_folha.py:1186`, `app/escala/services/cadastro_realidade.py:175`, `app/folha/vigia_retido_calado.py:48`.

5. **MÉDIA — descarte 4 correto para empresa sem lavra na tela; insuficiente para retrato histórico no vigia.**

   A tela e o lavrador usam `empresas_visiveis`, sustentando o descarte no caminho apresentado. Mas “sem lavra não tem folha” não vale para a autoridade viva: `_K2` consulta fechamentos diretamente. `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/export.py:775`.

   **Como provar o caminho residual:** lavrar uma empresa visível com C5 positiva e depois alterar somente `Empresa.ativa=False`. O lavrador deixa de atualizá-la; `censo([id])` ainda lê seu retrato histórico, C5 ainda aceita empresa/retrato e o vigia pode declarar `roteados`. A tela já exclui essa empresa. Logo, pode haver “roteado para a lista” sem presença na lista acessível. `app/colaboradores/queries.py:85`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:38`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:48`, `app/escala/views.py:487`.

6. **SEM DEFEITO NOVO IDENTIFICADO — filtro de motivo e sua origem.**

   O caminho normal atual sustenta a cura: somente IDs classificados como `furo_espelho` alimentam os dias de `dias_que_seguram`; férias terminam em outro ramo e fornecem outra lista. Não identifiquei, nesse caminho, outro motivo com datas dessa fonte nem furo com datas de férias. `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`, `app/folha/export.py:876`, `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1175`.

   | Quem escreve/lê | Caminho |
   |---|---|
   | Decide `motivo='furo_espelho'` | `classificar_export`. `app/folha/export.py:962`. |
   | Produz suas datas | Retentor → `dias_que_seguram` → conversão ISO em `fora_da_folha`. `app/folha/fora_da_folha.py:98`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:192`. |
   | Monta e persiste o JSON | `montar_linhas` copia motivo/datas; command grava. `app/folha/retrato_folha.py:1181`, `app/folha/retrato_folha.py:1183`, `app/folha/management/commands/lavrar_retrato_folha.py:74`. |
   | Compara literalmente furo no retrato | Nova C5; derivações de fio e tipo. `app/escala/services/cadastro_realidade.py:189`, `app/folha/retrato_folha.py:366`, `app/folha/retrato_folha.py:380`. |
   | Outros leitores de motivo | Balde, saldo, ação/drawer, apresentação e recorte temporal. `app/folha/retrato_folha.py:93`, `app/folha/retrato_folha.py:501`, `app/folha/retrato_folha.py:1062`, `app/folha/views_retrato.py:285`, `app/folha/retrato_folha.py:1240`. |
   | Vigia | Lê indiretamente pelo balde e fio. `app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:47`. |

   Essa garantia não certifica qualquer JSON histórico: o modelo prevê retratos sem `datas` e declara `linhas` como JSON sem esquema de conteúdo. Conteúdo divergente em produção: **[nao verificado]**. `app/folha/models.py:332`, `app/folha/models.py:348`.

7. **SEM RELÓGIO SOLTO IDENTIFICADO nos testes novos/alterados; curas específicas com mordida adequada.** [nao verificado]

   Os comportamentais estão congelados por classe; os testes AST analisam texto. O positivo corrente distingue `janelas[:1]`, e o negativo de férias distingue a retirada do filtro de motivo. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:90`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:243`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:256`, `app/folha/tests/test_vigia_retido_calado.py:69`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:217`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:210`.

   O teste do command prova repasse de `hoje`, mas não unicidade da leitura do relógio: uma segunda chamada a `localdate()` não violaria suas asserções. O command atual contém uma leitura. `app/folha/tests/test_vigia_retido_calado.py:140`, `app/folha/tests/test_vigia_retido_calado.py:143`, `app/folha/management/commands/vigiar_retido_calado.py:31`.

   Não identifiquei push novo nos trechos alterados: C5 monta retorno/link; vigia conta; command imprime/alarma. `app/escala/services/cadastro_realidade.py:201`, `app/escala/services/cadastro_realidade.py:657`, `app/folha/vigia_retido_calado.py:44`, `app/folha/management/commands/vigiar_retido_calado.py:31`.

## 06/11 17:59 · sessao a3b5c798fdcd · PAUTA-08 · verificador · origem=Code

1. **Sim, fecha o achado específico da volta 1.** Toda criação de cabeça neste comando passa pelo `atomic`, pelo `select_for_update` do colaborador e pela comparação do hash antes de chamar o escritor. Não identifiquei caminho que grave uma cabeça nova sem essa conferência no mesmo `atomic`; o ramo `dry` só imprime, e o ramo de pauta existente não cria outra. ([comando:182](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182), [comando:189](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189), [comando:195](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:195), [comando:198](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198), [comando:203](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:203)).

   **Isso prova a conferência, não o congelamento do fechamento até o commit.** O lock é sobre `Colaborador`; o hash lê `FechamentoMensal` sem bloqueá-lo explicitamente. A comparação final continua podendo lançar erro depois das gravações, mas essas cabeças agora tiveram a conferência anterior. ([comando:192](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:192), [regen_exportada.py:71](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:71), [comando:224](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224)).

2. **Banco e arquivo têm comportamentos diferentes.** O novo `raise` ocorre antes de criar qualquer pauta daquele colaborador e antes de acrescentar seus PKs à reversão. Portanto, nesse erro específico, não há pauta nova dele para desfazer nem entrada dele em `rev['pautas']`; seu plano já pode constar no JSON inicial. ([comando:163](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:163), [comando:166](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:166), [comando:195](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:195), [comando:212](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:212)).

   Os colaboradores anteriores têm transações próprias já encerradas e seus PKs já registrados: **o novo `raise` preserva essas pautas e mantém a lista de reversão coerente com elas**, supondo gravações e commits anteriores bem-sucedidos. ([comando:169](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:169), [comando:189](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189), [comando:212](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:212)).

   **O `atomic` não desfaz o arquivo.** Um erro após `_grava`, inclusive falha no commit, pode deixar no JSON PKs cujas pautas voltaram no banco. O próprio comentário reconhece isso; `_reverter` pula PK inexistente. Logo, a coerência nesse novo `raise` decorre da posição dele, não de rollback do JSON. ([comando:210](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:210), [comando:270](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:270), [comando:298](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:298)).

3. **O teste morde a ausência da conferência por colaborador, mas não fixa sua posição transacional.** O mock altera o retorno a partir da segunda chamada; as asserções exigem erro identificando o colaborador e ausência de suas pautas. Sem a conferência nova, sobra a comparação final: erro genérico e pautas já escritas, contrariando essas asserções. ([teste:204](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204), [teste:211](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:211), [comando:224](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224)).

   Por análise estática, dois mutantes passariam pelas asserções desse teste: [nao verificado]

   - **Conferir depois de escrever, ainda dentro do `atomic`:** o erro reverte as pautas e satisfaz a ausência exigida. Se a conferência vier depois de `_grava`, pode deixar PKs mortos no JSON, que esse teste não inspeciona. ([comando:203](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:203), [comando:213](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:213), [teste:208](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:208)).
   - **Conferir antes de escrever, fora do `atomic`:** o mock provoca o erro antes das criações; o teste não observa a localização da transação nem o lock. ([teste:204](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204), [teste:209](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:209)).

   Execução do teste e dos mutantes: **[nao verificado]**. [nao verificado]

4. **A falsidade geral apontada anteriormente foi corrigida.** A linha agora restringe a ausência ao comparador **genérico** e reconhece passivo validado e HE autorizada. Esses fluxos efetivamente escrevem pautas com os dois lados. ([LEIS.md:177](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/docs/LEIS.md:177), [passivo_validado.py:192](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/chamados/services/passivo_validado.py:192), [passivo_validado.py:229](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/chamados/services/passivo_validado.py:229), [autorizacao_he_periodo.py:184](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/autorizacao_he_periodo.py:184), [autorizacao_he_periodo.py:244](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/autorizacao_he_periodo.py:244)).

   Resta **ambiguidade** em “caminho AUTOMATICO segue sem codigo”: no escopo genérico da linha, não reproduz a falsidade anterior; como negação global, seria falsa, pois a autorização de HE chama automaticamente o emissor quando exportada e com autorizações. ([LEIS.md:177](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/docs/LEIS.md:177), [autorizacao_he_periodo.py:299](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/autorizacao_he_periodo.py:299)).

5. **Leitores e escritores no caminho tocado:**

   | Objeto | Quem produz/escreve | Quem lê/consome |
   |---|---|---|
   | `hash_do_gravado` | É função, não coluna: calcula MD5 truncado sobre campos do `FechamentoMensal`. ([regen_exportada.py:62](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:62)) | O comando guarda o inicial, compara por colaborador e compara no fim. ([comando:120](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:195](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:195), [comando:224](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224)) |
   | Hash na reversão | O comando grava `hash_gravado` no plano JSON. ([comando:163](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:163)) | `_reverter` carrega o arquivo, mas usa marca, schema e PKs; não valida o hash guardado. ([comando:258](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:258)) |
   | `FechamentoMensal` no PAUTA-08 | O comando solicita recálculo somente leitura; recebe campos de objeto destacado. ([comando:135](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135), [fechamento.py:1112](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:1112)) | Leem a linha: `hash_do_gravado`, `valores_do_gravado` para PAGO e o recálculo para semear o objeto destacado. ([regen_exportada.py:71](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:71), [regen_exportada.py:81](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:81), [fechamento.py:881](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:881)) |
   | `FechamentoMensal` no ramo escritor relacionado | O mesmo serviço usa `get_or_create`, `save` ou `update`; a porta de retificação chama esse ramo. ([fechamento.py:889](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:889), [fechamento.py:1128](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:1128), [fechamento.py:1144](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:1144), [regen_exportada.py:179](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:179)) | A porta lê hash/valores antes e depois, envia hashes à auditoria e retorna `mudou`; o passivo consome `mudou` para decidir a pauta. ([regen_exportada.py:116](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:116), [regen_exportada.py:189](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:189), [regen_exportada.py:206](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:206), [regen_exportada.py:218](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:218), [passivo_validado.py:442](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/chamados/services/passivo_validado.py:442)) |

Ocorrência desses cenários em produção: **[nao verificado]**.

## 06/11 17:58 · sessao bd1f88673f15 · - · - · origem=Ronald (tmux)

1. **MÉDIA — há mutante plausível sobrevivente na fronteira inferior da janela.** Trocar `data__gte=janelas[0][0]` por `data__gt=janelas[0][0]`, ou apagar esse filtro, preserva por inspeção os cenários novos: as células da fixture são posteriores ao início da anterior, inclusive no corte 10. Nenhum caso insere um `nunca_bateu` em 21/09 ou 20/09 para corte 21. `app/escala/services/cadastro_realidade.py:176`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:33`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:36`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:250`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`.

   O teste de `hoje=01/09` do vigia também não distingue a remoção: a célula de 01/10 continua excluída pelo limite superior de 31/08. O teste de `hoje=25/11` usa desligado, cujos dias ainda dependem dos retratos selecionados; não prova limite inferior dos ativos. `app/folha/tests/test_vigia_retido_calado.py:79`; `app/folha/tests/test_vigia_retido_calado.py:133`; `app/escala/services/cadastro_realidade.py:185`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:240`.

   **Como provar:** ativo com dias 20/09 e 21/09, `hoje=25/10`, corte 21; exigir somente 21/09. `janela_anterior` termina em 20/10 e inicia em 21/09 por composição das funções de janela. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:60`; `app/ponto/janelas.py:66`. Suíte Django contra mutante: **[nao verificado]**. O mutante que apaga o filtro passou o selo AST executado em memória; esse selo não verifica os filtros de datas. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`.

2. **BAIXA — cura de kwargs correta, cobertura AST ainda parcial.** O delta detecta `filter(data_demissao__isnull=False)`, mas `filter(**{'data_demissao__isnull': False})` continua invisível: no AST, a expansão tem `k.arg=None`, que o detector descarta. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:327`.

   **Como provar:** substituir em memória a consulta de desligados por `filter(pk__in=..., situacao='desligado', **{'data_demissao__isnull': False})`; `_veredito` retorna `campos=set()` e o selo passa. A versão com kwarg direto retorna `{'data_demissao'}` e falha no selo. Executei essas sondas usando a classe AST extraída, com `unittest.TestCase`, sem Django nem escrita. O ponto de inserção é `app/escala/services/cadastro_realidade.py:179`; a exigência que passa indevidamente é `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:335`.

   Esse mutante comportamental específico seria combatido pelo novo teste de cadastro torto, pois o ID excluído de `segura` passa pelo ramo dos demais colaboradores e recebe todos os dias, inclusive os posteriores à rescisão. Portanto, não alego sobrevivência à suíte inteira. `app/escala/services/cadastro_realidade.py:192`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`. Execução Django: **[nao verificado]**.

3. **Curas dos testes: adequadas aos dois mutantes apontados na rodada anterior.** O novo positivo corrente exige `DIAS_A`, incluindo 21–23/10, e mata a seleção `janelas[:1]`; o novo negativo de férias mata a retirada do filtro `motivo == 'furo_espelho'`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:217`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:221`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:210`; `app/escala/services/cadastro_realidade.py:189`. Execução Django desses casos: **[nao verificado]**.

4. **Relógio: não encontrei dependência solta nos testes novos/alterados.** Os comportamentais ESEM estão congelados por classe; os três testes alterados/adicionados do vigia pertencem à classe congelada. Os testes AST somente analisam texto. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:90`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:243`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:256`; `app/folha/tests/test_vigia_retido_calado.py:69`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:293`.

   O teste do command prova o repasse de `hoje`, mas não conta chamadas ao relógio: adicionar outra leitura de `timezone.localdate()` ao command não o faria falhar, pois verifica apenas chamadas ao censo e o argumento final. Isso é limite da asserção, não defeito observado no command atual. `app/folha/tests/test_vigia_retido_calado.py:140`; `app/folha/tests/test_vigia_retido_calado.py:142`; `app/folha/management/commands/vigiar_retido_calado.py:31`.

## 06/11 17:57 · sessao d31f241278af · - · - · origem=Ronald (tmux)

1. **Descarte 1 — sustenta-se como decisão de seguir o juiz; não como demonstração de exclusão de desligado sem data.** O universo não cria novo fechamento para desligado sem `data_demissao`, mas `_K2` consulta fechamentos existentes, `sai_pela_rescisao` devolve falso para data nula e o retentor não recorta o fim. Assim, um fechamento anterior permite nova lavra `furo_espelho`, aceita pela C5. O teste novo reproduz corretamente esse histórico e exige igualdade com a autoridade. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430`, `app/folha/export.py:775`, `app/folha/export.py:60`, `app/folha/export.py:1200`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/services/cadastro_realidade.py:189`.

   **BAIXA — documentação contraditória:** a docstring ainda diz “sem `data_demissao` [...] = fora” e, duas linhas depois, admite que fechamento antigo segue o juiz. O primeiro enunciado precisa da condição “sem fechamento preexistente”. `app/escala/services/cadastro_realidade.py:148`, `app/escala/services/cadastro_realidade.py:151`.

   O aval literal não especifica expressamente o cadastro com data nula; inferir dele uma obrigação inequívoca de excluir esse caso é **[nao verificado]**. A solução não acrescentou predicado próprio de demissão: só consulta `situacao` e lê a lavra. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:185`.

2. **Descarte 3 — sustenta-se para retirar da cura específica da C5; não elimina a divergência entre competências.** O vigia declara explicitamente unidade **PESSOA** e roteia pela presença do ID na lista. A C5 agora inclui desligados; portanto, o mesmo comportamento já existente para ativos passa a alcançá-los. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:20`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`, `app/escala/services/cadastro_realidade.py:189`.

   **Caminho concreto:** hoje 25/10, corte 21, desligado com demissão 25/11, FM de competências 10 e 11, uma célula `nunca_bateu` em 05/10 e vínculo cobrindo 21–24/10 sem células. O juiz retém a anterior por `espelho_nunca_bateu` e a corrente por `sem_celula`; sem célula não há porta, produzindo grupo D e fio `ninguem`. A C5 contém apenas 05/10, mas o vigia roteia ambas as linhas porque descarta as datas e usa somente ID. `app/folha/export.py:960`, `app/folha/export.py:1227`, `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:86`, `app/folha/fora_da_folha.py:175`, `app/folha/retrato_folha.py:1186`, `app/escala/services/cadastro_realidade.py:175`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

   A formulação estrita por competência não é garantida pelo vigia; ocorrência em produção e execução desse cenário: **[nao verificado]**. O aval fornecido limita a inclusão C5, enquanto o vigia documenta sua própria granularidade por pessoa. `app/escala/services/cadastro_realidade.py:138`, `app/folha/vigia_retido_calado.py:12`.

3. **Descarte 4 — sustenta-se no caminho normal da tela; a justificativa “sem lavra não tem folha” é ampla demais.** A tela passa somente `empresas_visiveis`, e o lavrador aplica a mesma seleção. Contudo, a autoridade viva lê FM diretamente, sem exigir empresa visível ou retrato; logo, ausência de lavra não prova ausência de retenção viva. `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:51`, `app/folha/export.py:775`.

   **MÉDIA — caminho residual do vigia:** lavrar uma empresa visível e depois torná-la inativa. O lavrador deixa de atualizá-la; `censo([id])` continua lendo o retrato antigo, C5 aceita empresa e retrato, e o vigia pode marcar `roteados`, embora a tela exclua a empresa inteira. Esse problema já existia para ativos e agora alcança desligados aceitos pela lavra. `app/colaboradores/queries.py:85`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/vigia_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:38`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:48`, `app/escala/views.py:487`.

   **Como provar:** conservar célula e retrato positivos, alterar somente `Empresa.ativa=False`, comparar `censo([id])` com o conjunto de empresas passado pela view à lista. Execução e ocorrência em produção: **[nao verificado]**. `app/colaboradores/queries.py:85`, `app/folha/vigia_retido_calado.py:35`, `app/escala/views.py:487`.

4. **Sem novo caminho de push identificado nesses trechos:** C5 monta assinaturas e sua ação é um link ao colaborador; o vigia conta, e o command imprime ou alarma. Os portões de emissão do cartório continuam excluindo desligados. `app/escala/services/cadastro_realidade.py:201`, `app/escala/services/cadastro_realidade.py:657`, `app/folha/vigia_retido_calado.py:44`, `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/ponto/services/cartorio.py:1696`, `app/ponto/services/cartorio.py:1762`.

## 06/11 17:57 · sessao e2e596e173d3 · - · - · origem=Ronald (tmux)

No caminho tocado, `hash_do_gravado` é **função que lê**, não campo persistido: consulta `FechamentoMensal`, serializa os campos selecionados e retorna MD5 truncado; exclusões em `_FORA_DO_HASH` (`app/ponto/portas/regen_exportada.py:37`, `:56`, `:62`, `:71`, `:74`).

| Objeto | Quem escreve/produz | Quem lê/consome |
|---|---|---|
| Hash no comando | `hash_do_gravado` calcula; `_medir` guarda o inicial em `hashes` e `linhas` (`app/ponto/portas/regen_exportada.py:70`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`, `:140`, `:148`). | Comparação por colab dentro do atomic (`comando:189`, `:195`); comparação final de todo universo (`comando:224`). |
| Hash no JSON | `_escrever` monta `plano[pk].hash_gravado`; `_grava` serializa e substitui o arquivo (`comando:163`, `:164`, `:166`, `:298`, `:299`, `:300`). | `_reverter` carrega o JSON, mas usa marca, schema e lista `pautas`; não confere o hash do plano (`comando:258`, `:262`, `:265`, `:269`, `:279`). |
| Hash na porta de retificação | `regenerar_em_exportada` calcula antes/depois e entrega os hashes à auditoria e ao retorno (`app/ponto/portas/regen_exportada.py:116`, `:189`, `:199`, `:206`, `:208`, `:217`). | A própria porta compara os hashes para `mudou` (`app/ponto/portas/regen_exportada.py:218`); `passivo_validado` usa `regen['mudou']` para decidir a pauta (`app/chamados/services/passivo_validado.py:438`, `:442`, `:443`). |
| `FechamentoMensal` no PAUTA-08 | O comando pede `somente_leitura=True`; o serviço instancia objeto destacado e devolve campos, sem entrar nos ramos de persistência (`comando:135`; `app/ponto/services/fechamento.py:867`, `:881`, `:882`, `:1112`, `:1119`). | `hash_do_gravado` lê a linha para o hash; `valores_do_gravado` lê o PAGO; o recálculo lê a linha para semear o objeto destacado (`app/ponto/portas/regen_exportada.py:71`, `:81`, `:84`; `app/ponto/services/fechamento.py:881`, `:883`). |
| `FechamentoMensal` no ramo de escrita do mesmo serviço | `recalcular_fechamento_mes` usa `get_or_create`, depois `save()` para aberto ou `update()` para o outro status (`app/ponto/services/fechamento.py:889`, `:1121`, `:1128`, `:1144`). A porta `regenerar_em_exportada` chama esse ramo sem `somente_leitura` (`app/ponto/portas/regen_exportada.py:179`). | Depois da retificação, a porta lê novamente hash e valores (`app/ponto/portas/regen_exportada.py:189`, `:190`). |

**Limite do lock:** PAUTA-08 bloqueia `Colaborador`, enquanto o hash consulta `FechamentoMensal` por SELECT comum (`comando:192`; `app/ponto/portas/regen_exportada.py:71`). O recálculo mostrado percorre colaboradores com `select_related` e escreve FM por `save/update`, sem adquirir explicitamente esse mesmo lock da pessoa (`app/ponto/services/fechamento.py:610`, `:1128`, `:1144`). Portanto, o delta prova a conferência no atomic, mas não prova congelamento do FM durante todo o atomic por esse lock (`comando:189`, `:192`, `:195`; `app/ponto/portas/regen_exportada.py:71`).

Outros escritores disponíveis no mesmo módulo, fora da execução normal do comando: aprovação/reabertura (`app/ponto/services/fechamento.py:1307`, `:1316`, `:1324`, `:1330`, `:1338`), restauração de campos (`:1376`), reabertura de período (`:1433`), invalidação de previsto/derivação (`:1594`, `:1597`, `:1636`, `:1640`).

## 06/11 17:57 · sessao 3074e80c895b · - · - · origem=Ronald (tmux)

Rastreio estático concluído; não executei Django nem consultei banco/log. **[nao verificado]** para estado de produção e acessos dinâmicos fora do código identificado. [nao verificado]

- **Quem decide e escreve:** `classificar_export` atribui `status='fora'`, `motivo='furo_espelho'` quando o retentor acusa; `montar_linhas` copia esse motivo para o JSON; o command persiste com `update_or_create`. `app/folha/export.py:960`, `app/folha/retrato_folha.py:1165`, `app/folha/retrato_folha.py:1181`, `app/folha/management/commands/lavrar_retrato_folha.py:74`.

- **Origem das datas:** `dias_que_seguram` inclui no mapa de dias somente IDs classificados como `furo_espelho`, chama `_dias_que_retem` para eles; esta delega a `motivos_retencao_celula(com_dias=True)`; `fora_da_folha` converte `data` para string ISO; `montar_linhas` copia a lista. `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:98`, `app/folha/fora_da_folha.py:192`, `app/folha/retrato_folha.py:1171`.

- **Outra forma de datas:** o ramo `ferias_com_batida` recebe `dias_ferias_com_batida`; o lavrador usa essa lista apenas quando não encontrou `dias` em `colabs`. Essa lista não vem de `dias_que_seguram`. `app/folha/export.py:868`, `app/folha/export.py:875`, `app/folha/retrato_folha.py:1172`, `app/folha/retrato_folha.py:1175`.

- **Resposta à equivalência:** no caminho normal atual do escritor, em estado estável, não identifiquei outro motivo com datas originadas em `dias_que_seguram`, nem `furo_espelho` com datas de férias: `_por_colab` só contém furo e a classificação de férias termina por `continue` antes de furo. `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:171`, `app/folha/retrato_folha.py:1158`, `app/folha/export.py:876`, `app/folha/export.py:960`.

- **Limite dessa equivalência:** `linhas` é JSON sem validação de esquema declarada; retratos antigos sem `datas` são explicitamente previstos, e a porta somente lê e recorta o conteúdo existente. Portanto, a garantia não vale para qualquer JSON persistido arbitrariamente nem certifica dados legados. `app/folha/models.py:348`, `app/folha/models.py:332`, `app/folha/retrato_folha.py:110`. A ocorrência de JSON divergente é **[nao verificado]**.

**Leitores do motivo gravado:**

| Leitor | Uso/evidência |
|---|---|
| C5 | Compara literalmente `motivo == 'furo_espelho'` antes de unir as datas. `app/escala/services/cadastro_realidade.py:189`. |
| `_balde` | Consulta motivo para retirar os módulos próprios; compõe a porta, contadores e filtros. `app/folha/retrato_folha.py:93`, `app/folha/retrato_folha.py:111`, `app/folha/retrato_folha.py:120`. |
| `_fio_cru` / `fio_da_linha` | Distingue literalmente furo dos demais motivos; leitura de legado quando não existe fio válido gravado. `app/folha/retrato_folha.py:366`, `app/folha/retrato_folha.py:441`. |
| `_tipo_cru` / `tipo_da_linha` | Distingue literalmente furo para escolher causa versus mapa de motivo; fallback legado. `app/folha/retrato_folha.py:380`, `app/folha/retrato_folha.py:491`. |
| `_saldo_cru` / `saldo_da_linha` / `no_periodo` | Consulta motivo para módulos/cadastro e recalcula faixa sobre o número de datas recortadas. `app/folha/retrato_folha.py:414`, `app/folha/retrato_folha.py:501`, `app/folha/retrato_folha.py:1240`, `app/folha/retrato_folha.py:1245`. |
| `o_que_fazer` | Apresenta o rótulo do motivo no drawer. `app/folha/retrato_folha.py:1062`, `app/folha/retrato_folha.py:1065`; chamado em `app/colaboradores/views.py:984`. |
| Tela, PDF e grupos do drawer do retrato | Apresentam rótulo de motivo. `app/folha/views_retrato.py:285`, `app/folha/views_retrato.py:417`, `app/folha/views_retrato.py:496`. |
| Vigia | Leitura indireta por porta/balde e fio; não compara diretamente o motivo. `app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:47`. |
| Botões de ação | Leitura indireta por fio e tipo. `app/folha/retrato_folha.py:1015`. |

Os outros consumidores literais encontrados de `furo_espelho` — `dias_que_seguram`, views antigas de folha e `fechamento_lista` — leem **itens do juiz**, não a chave de linhas já gravadas do Retrato. `app/folha/fora_da_folha.py:119`, `app/folha/views.py:259`, `app/folha/views.py:591`, `app/ponto/services/fechamento_lista.py:34`.

Não encontrei defeito novo no filtro de motivo do delta: ele elimina precisamente a segunda forma legítima de datas, preservando o ramo do retentor. `app/escala/services/cadastro_realidade.py:189`, `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1175`.

## 06/11 17:57 · sessao 99686e6c7958 · - · - · origem=Ronald (tmux)

A generalização da volta 1 foi corrigida: `LEIS.md:177` agora delimita a ausência ao comparador **genérico** e reconhece os dois escritores anteriores. O passivo lê `antes/depois`, monta `pago -> devido` e escreve cabeça/filhas (`app/chamados/services/passivo_validado.py:178`, `:192`, `:229`, `:233`); seu chamador retifica pela porta e então gera a pauta (`app/chamados/services/passivo_validado.py:438`, `:443`). HE usa lavratura para pago, motor para devido e escreve a pauta (`app/ponto/services/autorizacao_he_periodo.py:174`, `:176`, `:184`, `:244`).

A cauda **“caminho AUTOMATICO segue sem codigo”** permanece ambígua: é compatível com a linha se conservar o escopo **genérico**; como negação de qualquer geração automática de Pauta DP com dois números, seria falsa, pois `_ato` chama `_linha_para_o_dp` quando a competência está exportada e houve autorização (`app/docs/LEIS.md:177`; `app/ponto/services/autorizacao_he_periodo.py:299`, `:300`). Eu não manteria o achado antigo como falsidade categórica sem explicitar essa interpretação.

O comando PAUTA-08 continua registrado como ato de uma vez por competência, lista fechada e escrita explícita (`app/config/crons.py:781`, `:784`, `:786`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:79`, `:160`). Existência histórica de outros caminhos ausentes desta cópia: **[nao verificado]**.

## 06/11 17:55 · sessao 9a0f294cef8e · ESPURIA · censo · origem=Code

HASH LIDO: 7c140052a45f90bc52eb5d68726782428fe9307c — identificador fornecido da cópia; correspondência com HEAD [nao verificado].

O quadro **477 = 175 + 262 + 40 + 0** é o declarado no dado raspado, sem linhas individuais para reclassificação ou conferência das amostras. A nova distribuição, os destinos dos 175 e a quantidade de dias com tipos divergentes nas lâmpadas são [nao verificado].

**1. “Dia com órfã na ata”: entidade, escritores, leitores e consulta**

A entidade é **`escala.models.CelulaDia`**, campo JSON **`ata`**, com unicidade por `(colaborador, data)`. Não é uma tabela separada de atas versionadas. (`app/escala/models.py:1774`, `app/escala/models.py:1778`, `app/escala/models.py:1833`)

O produtor é `ata_do_dia(dia, marcos=None, dna=None)`: grava `ata['orfas']` com as horas de `dia['orfas']` e `ata['n_orfas']` com sua quantidade. O alinhamento que produz essas órfãs é `_match_marcos(...)`: devolve `(celulas, orfas)`, sendo órfã a marca não atribuída a marco. (`app/ponto/services/cartorio.py:696`, `app/ponto/services/cartorio.py:708`, `app/escala/utils.py:585`, `app/escala/utils.py:621`, `app/escala/utils.py:662`)

Quem persiste é **`ponto.portas.celula.lavrar_veredito`**; ela pode atualizar a ata mesmo quando preserva um veredito administrativo. A chave `precedencia` tem outro escritor declarado, `carimbar_precedencia`, e atravessa a substituição da ata. (`app/ponto/portas/celula.py:85`, `app/ponto/portas/celula.py:104`, `app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1027`)

Leitores existentes:

| Leitor | O que devolve |
|---|---|
| `_orfas_da_ata(ata)` | Horas gravadas; em ata antiga contendo somente contagem, devolve `'?'` para cada órfã. (`app/escala/services/leitor_celula.py:175`) |
| `grade_da_celula(colab, ini, fim)` | Grade por período, transportando órfãs, lâmpadas, veredito e célula; lê a ata persistida. (`app/escala/services/leitor_celula.py:345`, `app/escala/services/leitor_celula.py:401`) |
| `periodos_do_dia(celula, tipo_escala)` | Interpretação do dia pela ata, incluindo órfãs e papéis das lâmpadas. (`app/ponto/juiz_batida.py:149`) |

A consulta abaixo seleciona **dias da última ata persistida com `n_orfas > 0`**, por empresa. A janela vem de `janela_fechamento`, que lê `Empresa.dia_inicio_competencia`; corte 21 produz 21/10–20/11/2026. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/colaboradores/models.py:46`)

```python
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento

ini, fim = janela_fechamento(11, 2026, empresa)

dias = (
    CelulaDia.objects
    .filter(
        colaborador__empresa=empresa,
        data__range=(ini, fim),
        ata__n_orfas__gt=0,
    )
    .order_by('colaborador_id', 'data')
)
```

Para a frota, aplicar a consulta às empresas do universo, respeitando o corte de cada uma. A célula não tem filtro próprio `vigente`/`retratada_em`; a leitura canônica consulta diretamente `(colaborador, intervalo)`, sem exigir vínculo atualmente ativo. (`app/escala/models.py:1681`, `app/escala/models.py:1833`, `app/escala/services/leitor_celula.py:350`)

**Retratação pertence ao universo das batidas:** `batidas_apuraveis(colab, ini=None, fim=None)` aplica `retratada_em__isnull=True`. Não se deve inventar esse filtro na célula nem juntar batidas por data civil para redefinir os dias da ata. (`app/ponto/turnos.py:2844`, `app/ponto/turnos.py:2863`, `app/ponto/services/cartorio.py:785`)

“Persistida” não garante “atualizada com todos os insumos atuais”: `grade_da_celula` não confere impressão; o cartório calcula `_em_dia` internamente. `houve_fato_novo(...)` exclui deliberadamente chamado/fio e não substitui essa conferência integral. (`app/escala/services/leitor_celula.py:362`, `app/ponto/services/cartorio.py:329`, `app/ponto/services/cartorio.py:1348`)

**2. Onde a tela escreve “espúria”**

O literal encontrado está no **calendário do colaborador**:

> “Batida fora do turno previsto (espúria) — não casa nenhum marco; conferir.”

Ele aparece quando `b.orfa` é verdadeiro. (`app/templates/colaboradores/partials/_calendario_grade.html:121`)

Quem atribui esse sinal é `contexto_calendario(...)`: obtém as órfãs pela grade lida da célula e marca `b.orfa=True` quando a hora local da batida pertence ao conjunto de órfãs daquele dia. Portanto, esse caminho **lê a órfã da ata**; não executa novamente o casamento batida–marco. (`app/colaboradores/services/calendario.py:332`, `app/colaboradores/services/calendario.py:381`, `app/colaboradores/services/calendario.py:621`, `app/escala/services/leitor_celula.py:403`)

No arquivo **`templates/ponto/espelho.html`**, a órfã aparece como **`• HH:MM`**, sem esse literal. `montar_realizado_grade(...)` transporta a lista de órfãs para a apresentação. (`app/templates/ponto/espelho.html:240`, `app/escala/utils.py:1099`, `app/escala/utils.py:1131`)

O serviço do espelho usa a célula na competência, mas também possui caminho pelo builder para o trecho anterior à competência e degradação expressa para ata agregada sem marcos. Assim, “todo espelho sempre lê somente a ata” não descreve todos os caminhos existentes. (`app/ponto/services/espelho.py:518`, `app/ponto/services/espelho.py:528`, `app/ponto/services/espelho.py:537`)

**3. Funções para classificar os dias e censo de casamento com escala**

`dia_sem_trabalho(cel) -> bool` recebe uma célula, não uma lista de batidas. Lê `trabalha` e `ata['tipo_dia']`: considera `trabalha is False` ou tipo preenchido diferente de `trabalho`; na ausência parcial, só `trabalha is False`. Está registrado como juiz. (`app/chamados/juizes.py:413`, `app/chamados/juizes.py:426`, `app/core/juizes.py:278`)

**`STATUS_QUE_PEDEM_DECISAO` não é classificador de folga trabalhada.** Seu conteúdo é `('falta', 'irregular', 'aberto', 'sem_certidao')`. A função que entrega o status é `status_do_dia_lavrado(colaborador, data, *, agora=None) -> {'status', 'rotulo', 'fonte'}`, registrada em `core/juizes.py`. (`app/colaboradores/services/calendario.py:94`, `app/ponto/status_dia.py:201`, `app/core/juizes.py:187`)

A forma de lote é `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None) -> {(colaborador_id, data): resposta}`; é o mesmo corpo do juiz singular. O calendário lê essa resposta. (`app/ponto/status_dia.py:218`, `app/ponto/status_dia.py:221`, `app/colaboradores/services/calendario.py:602`, `app/colaboradores/services/calendario.py:663`)

Para identificar **folga trabalhada**, ler o status `folga_trabalhada`. A regra de lavra existente o produz quando há batida e `tipo_dia == 'folga'` ou `veredito == 'fato_sem_previsao'`. Já `resolver_dia_oferecido(dia, hoje) -> bool` responde outra pergunta: último recurso disponível, dia passado e status pertencente à constante. (`app/ponto/status_dia.py:157`, `app/colaboradores/services/calendario.py:97`, `app/core/juizes.py:718`)

**`trabalha_em_feriado` pertence a `EscalaColaborador`, o vínculo.** Não pertence ao template `TipoEscala`. `ec.feriado_vira_folga(data, escala=None) -> bool` reúne flag desligada, ciclo elegível e feriado do município do posto daquele vínculo. (`app/escala/models.py:871`, `app/escala/models.py:896`, `app/escala/models.py:1373`)

Para perguntar o feriado: `eh_feriado(data, lugar_do_vinculo(ec)) -> bool`. Para uma janela pessoal, `feriados_do_colab(colaborador, ini, fim, *, carregados=None, vinculos=None, celulas=None) -> set[date]` resolve o vínculo **de cada dia**. `eh_feriado` está registrado. (`app/core/feriados.py:91`, `app/core/feriados.py:114`, `app/core/feriados.py:125`, `app/core/juizes.py:176`)

O censo de “qual escala casa” encontrou funções com perguntas diferentes:

| Função real | Entrada → saída; batidas e limite |
|---|---|
| `ponto.propositor.dia_degenerado(dia)` | Dict-dia → `(bool, motivo)`: cluster, contagem incompatível, órfã, tipo incoerente ou delta crítico; pode devolver `SEM_HORA_NA_ATA`. Lê marcas já presentes no dict; **não escolhe outra escala**. (`app/ponto/propositor.py:34`) |
| `atribuir_global(slots, marcas)` / `atribuir_global_turno(slots, marcas)` | Slots e horas → `atrib`, `furos`, `sobras`, `custo`, `desloc`, `ambiguo`. Funções puras; casam marcas aos slots recebidos. A variante turno trata meia-noite; não pesquisa catálogo. (`app/ponto/propositor.py:87`, `app/ponto/propositor.py:112`, `app/ponto/propositor.py:165`) |
| `template_suspeito(dias)` | Dict-dias → `(suspeito, mediana, n_dias)`. Lê deltas recebidos; não consulta batidas nem escolhe template. (`app/ponto/propositor.py:201`) |
| `escala.services.cadastro_tipo` | `descreve_turno_partido(payload, tipo=None, pai=None) -> bool` lê a descrição do turno; `tipo_em_memoria(**campos)` instancia template sem salvar. Não foi encontrado matcher batidas→escala nesse módulo [nao verificado]. **`salvar_tipo` e `derivar_tipo` escrevem**. (`app/escala/services/cadastro_tipo.py:86`, `app/escala/services/cadastro_tipo.py:140`, `app/escala/services/cadastro_tipo.py:396`, `app/escala/services/cadastro_tipo.py:401`) |
| `escala.services.propositor.propostas(hoje=None, janela=28, colaborador_id=None, so_existencia=False)` | Coleta → dict com propostas e universo. Lê batidas não retratadas, células e templates; propõe âncora, template ou grade por dia. A coleta não é `aplicar`/`recusar`, que escrevem. (`app/escala/services/propositor.py:38`, `app/escala/services/propositor.py:382`, `app/escala/services/propositor.py:477`, `app/escala/services/propositor.py:585`) |
| `propor(ec, dias_bat, dias_janela, templates, previsto_atual, forcar='', ritmo='', horario=(None,None), pausa=None)` | Insumos já medidos → proposta ou `None`. O casamento comum compara **datas**; há filtro de horário no caminho por ritmo. Não prova, em todo caminho, casamento horário de todos os marcos. (`app/escala/services/propositor.py:162`, `app/escala/services/propositor.py:235`, `app/escala/services/propositor.py:308`, `app/escala/services/propositor.py:526`) |
| `fase_12x36.ancora_sugerida(dias_batidos, ancora_atual, piso=6, ganho_min=40)` | Datas → dict com âncora ISO ou `None`, motivo e porcentagens. Pura; compara **paridade**, não horário nem catálogo de escalas. (`app/escala/services/fase_12x36.py:23`, `app/escala/services/fase_12x36.py:27`) |
| `fase_12x36.dias_de_fase_as_of(colab_id, empresa_id, ini, fim, ate=None)` | Identificação/janela → datas ISO da fase deslocada, pela lavra mais recente que cobriu cada dia. Lê snapshots, não recalcula batidas. O detector anterior foi removido; o censo é a autoridade de fase indicada pelo módulo. (`app/escala/services/fase_12x36.py:68`, `app/escala/services/fase_12x36.py:117`) |
| `censo_fase_12x36.Command` | Competências/vínculos → censo de paridade real; critério `<90%` com `>=6` plantões. Lê turnos; `--lavrar` e `--curar` possuem escrita. (`app/ponto/management/commands/censo_fase_12x36.py:36`, `app/ponto/management/commands/censo_fase_12x36.py:180`, `app/ponto/management/commands/censo_fase_12x36.py:256`, `app/ponto/management/commands/censo_fase_12x36.py:274`) |
| **`escala.detector_proposta.propor_escala(colab, ini, fim)`** | Pessoa/janela → `padrao`, `proposta`, `confianca`, `porque`. Lê `batidas_apuraveis`; procura template por entrada, saída e número de marcos. **Limite declarado: agrupa por LOCALDATE, não dia-turno; noturno pode distorcer a medida.** (`app/escala/detector_proposta.py:18`, `app/escala/detector_proposta.py:49`, `app/escala/detector_proposta.py:95`, `app/escala/detector_proposta.py:118`) |
| `regua_defesa.diagnostico_regua(ec, ref=None, dias=30)` + `sugerir_destino(ec, diag)` | Vínculo/janela → diagnóstico com classe, evidência e medianas; diagnóstico → destino `{tipo_escala_id, codigo, n_colabs, dist}` ou `None`. O primeiro lê batidas apuráveis; o segundo compara com templates usados na mesma empresa. (`app/escala/regua_defesa.py:118`, `app/escala/regua_defesa.py:153`, `app/escala/regua_defesa.py:246`) |
| `fase_na_tela.fase_sugerida(c, te, hoje=None, ec_atual=None)` | Pessoa/template → `(ancora, desde, motivo)`. Lê dias trabalhados e troca de ritmo; o `desde` refere-se à fase/ritmo, não à prova de todos os horários casarem com um novo template. (`app/escala/services/fase_na_tela.py:40`, `app/escala/services/fase_na_tela.py:50`) |
| `pausa_nao_cadastrada(te, batidas_por_dia)` | Template/timestamps → pausa sugerida com evidência ou `None`. Não escolhe a escala inteira. (`app/escala/services/pausa.py:29`) |

**Registro desses candidatos:** não localizei os nomes acima como entradas próprias em `core/juizes.py` [nao verificado quanto à ausência global]. Os registros encontrados respondem **ocupação de marco** por `_match_marcos`, **turno partido** por `eh_turno_partido` e **vínculo que vale no dia/cobertura da data** por `vinculo_do_dia`/`vinculo_cobre_a_data`; essas perguntas não equivalem a escolher “a escala que casa com as batidas”. (`app/core/juizes.py:200`, `app/core/juizes.py:208`, `app/core/juizes.py:1285`)

O destino já mostrado em Cadastro x Realidade vem da lavra do esmeril: `lista(...)` lê essa lavra; `anomalias_da_frota(...)` chama `propor_escala` sobre a janela de 60 dias. Chamar `lista` lê uma foto; chamar `gravar_lavra` escreve. (`app/escala/services/cadastro_realidade.py:710`, `app/escala/services/cadastro_realidade.py:802`, `app/ponto/services/esmeril_espelho.py:206`, `app/ponto/services/esmeril_espelho.py:223`, `app/ponto/services/esmeril_espelho.py:317`)

Para o **dono L-099**, já existe `dono_da_divergencia(...) -> 'CADASTRO'|'BATIDA'|'ESTRUTURA'`, com montagem centralizada em `InsumosDoDono(...).dono(data)`. O carregador pode consultar o espelho/motor para `dias_cadastro_x_realidade`; essa etapa deve seguir a restrição de sombra do dado fornecido [nao verificado no repositório]. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:128`, `app/ponto/management/commands/e6_oraculo.py:154`, `app/ponto/management/commands/e6_oraculo.py:172`)

**4. “Célula refeita” e produção de órfã**

Os sinais de regeneração são **`regenerada_em`**, **`regeneracoes`** e **`dna_anterior`**. O último conserva apenas o DNA imediatamente anterior, não todo o histórico. `editada_em`/`origem='editada'` representam edição humana, outra operação. (`app/escala/models.py:1704`, `app/escala/models.py:1718`, `app/escala/models.py:1726`, `app/ponto/portas/celula.py:534`)

O escritor `regenerar_celulas_vinculo(...) -> número de células alteradas` troca trabalho/vínculo gerador/DNA quando há diferença, guarda o DNA velho e incrementa o contador. (`app/ponto/portas/celula.py:110`, `app/ponto/portas/celula.py:118`, `app/ponto/portas/celula.py:263`)

Há **duas órfãs diferentes**:

- **Batida órfã na ata:** não encontrou marco no alinhamento atual. Uma reescrita pode alterar os marcos comparados, mas o sinal de regeneração sozinho não prova essa causalidade. (`app/escala/utils.py:585`, `app/escala/utils.py:662`; causalidade dos 40 [nao verificado])
- **Cobrança órfã de regeneração:** cobrança emitida contra DNA anterior. A regeneração marca essas cobranças para novo julgamento; existe `cobranca_orfa(janelas, chamado, regenerada_em=None, editada_em=None) -> instante|None`. (`app/ponto/portas/celula.py:282`, `app/escala/services/regeneracao.py:141`)

Portanto, os **40** não podem ser certificados como “órfãs causadas pela célula refeita” apenas por `regenerada_em` preenchido [nao verificado].

**5. Marco de entrada aceso por batida gravada como saída**

Em **`ata['lampadas']`**, cada elemento guarda:

| Chave | Significado |
|---|---|
| `tipo`, `hora` | Tipo e hora do marco previsto. |
| `acesa`, `luz` | Estado da lâmpada e hora real atribuída. |
| `tipo_real` | Tipo gravado da batida atribuída. |

Essas chaves são produzidas em `lampadas_do_dia`; no casamento por proximidade sem coluna correspondente, `tipo_real=None` significa desconhecido. (`app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:688`)

**Já há comparação escrita:**

- `montar_realizado_grade(gd, marcos, chamados_dia=None) -> (real, orfas)` devolve **`real[].divergente`**, comparando literalmente `tipo_real` com o tipo do marco recebido. (`app/escala/utils.py:1099`, `app/escala/utils.py:1123`)
- O juiz registrado `periodos_do_dia(celula, tipo_escala) -> dict` devolve **`papeis[].tipo_divergente`**. Aqui o esperado vem do **DNA**; a conferência com a ata verifica horas e quantidade, podendo retornar `desalinhado` antes da comparação. (`app/ponto/juiz_batida.py:149`, `app/ponto/juiz_batida.py:195`, `app/ponto/juiz_batida.py:228`, `app/ponto/juiz_batida.py:246`, `app/core/juizes.py:1174`)

Para contar **a divergência literal da ata**, usar o adaptador `montar_realizado_grade` sobre a leitura das lâmpadas, contar uma vez cada `(colaborador,data)` com `divergente=True` entre lâmpadas efetivamente acesas e separar tipos desconhecidos. Para contar pela interpretação do juiz registrado, usar `periodos_do_dia` e publicar separadamente os dias `desalinhado`. As duas comparações têm referências esperadas diferentes; não devem receber o mesmo nome sem essa distinção. (`app/escala/services/leitor_celula.py:114`, `app/escala/services/leitor_celula.py:128`, `app/escala/utils.py:1130`, `app/ponto/juiz_batida.py:239`, `app/ponto/juiz_batida.py:246`)

O universo dessa pergunta é **todas as células da janela**, não apenas as que têm órfã: uma marca atribuída deixa de ser órfã mesmo podendo carregar tipo divergente. Assim, a contagem de órfãs não mede esse fenômeno. (`app/escala/utils.py:245`, `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:708`)

`dia_degenerado` também acusa `TIPO_INCOERENTE`, mas retorna antes para cluster/órfãs/contagem incompatível; não serve como contador completo das inversões. Quantidade real da frota [nao verificado]. (`app/ponto/propositor.py:56`, `app/ponto/propositor.py:65`)

**6. Dinheiro, decisão pendente e porta alcançável**

O dinheiro persistido é **`DiaPago`**, com versão escolhida explicitamente. `linhas_do_periodo(colab, ini, fim, *, versao='motor') -> {date: DiaPago}` lê as linhas diárias sem executar motor. A data da linha é a data da entrada do turno. (`app/ponto/models.py:303`, `app/ponto/models.py:319`, `app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:853`)

Para cada dia, os campos relevantes são `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, variantes noturnas e `horas_folga_trabalhada`; são **horas de rubricas**, não valores em reais. (`app/ponto/models.py:325`, `app/ponto/models.py:328`)

As funções de leitura prontas são:

| Função | Resposta |
|---|---|
| `rubricas_do_dia(linha)` | Dict com rubricas e `lavrada`. Linha ausente retorna zeros **com `lavrada=False`**. Variantes feriado/noturna são subconjuntos dos totais. (`app/ponto/services/dia_pago.py:899`) |
| `pago_do_dia(linha)` | Horas de trabalho pagas ou `None`; reúne trabalhadas, folga trabalhada e `detalhe['folga_feriado_min']`. Sem linha ou com todas as rubricas monetárias zeradas, retorna `None`. **HE zero isolada não prova ausência de dinheiro.** (`app/ponto/services/dia_pago.py:859`, `app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`) |
| `estado_por_dia(ids, ini, fim)` | Decisões de HE **vigentes**, por pessoa/data. Estado `'nao'` é decisão; ausência no mapa não prova, sozinha, existência de pendência. (`app/ponto/services/gestao_he.py:63`, `app/ponto/services/gestao_he.py:86`, `app/folha/porta_export.py:218`) |
| `he_pendente_lavrado.ler(empresa, mes, ano)` | `(detalhe, calculado_em)` ou `(None,None)`: foto de pendências, sem nova apuração. (`app/ponto/services/he_pendente_lavrado.py:193`) |
| `he_pendente_lavrado.apurar(empresa, mes, ano, *, decisoes=None, revogar=None)` | Mede a pendência atual pelo espelho/motor. Sem callback `revogar`, não revoga decisões; `lavrar` é a operação que grava a foto. (`app/ponto/services/he_pendente_lavrado.py:71`, `app/ponto/services/he_pendente_lavrado.py:94`, `a

[truncado em 20000 de 29100 caracteres]

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

_2387 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
