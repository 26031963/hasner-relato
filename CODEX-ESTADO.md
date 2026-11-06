# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 02:07** · publicado em **06/11/2026 02:08** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 02:07 · sessao 657abac16215 · H12H06-B · sonda · origem=Code

**Parte A — funções reais e limite da conferência.** O conteúdo de `sonda_h12h06b.py` não foi localizado na cópia nem consta do anexo transcrito; portanto, seus imports, argumentos, filtros e eventuais reconstruções de lógica ficam **[nao verificado]**. A vinculação da cópia ao HEAD informado também fica **[nao verificado]**. O identificador entre colchetes no cabeçalho do envelope é o SHA-256 abreviado **do script**, calculado em `bin/sonda_leitura.sh:219` e impresso em `bin/sonda_leitura.sh:221`.

As quatro funções existem com estas assinaturas:

| Função | Assinatura real | Fonte |
|---|---|---|
| `parciais_vigentes` | `parciais_vigentes()` | `app/ponto/catalogo/ausencias.py:336` |
| `cobre_parte_do_dia` | `cobre_parte_do_dia(a, parciais=None)` | `app/ponto/turnos.py:2583` |
| `janela_atual` | `janela_atual(data, empresa=None)` | `app/ponto/janelas.py:60` |
| `competencia_entregue` | `competencia_entregue(empresa_id, mes, ano, registro=False)` | `app/folha/export.py:556` |

`parciais_vigentes()` seleciona a flag de parcial do catálogo vigente; `cobre_parte_do_dia()` exige tipo nesse conjunto, registro de um único dia e **`0 < minutos < 480`**. `janela_atual()` recebe a data primeiro e aplica o corte da empresa. `competencia_entregue()` consulta a exportação vigente e exige que não esteja invalidada. (`app/ponto/catalogo/ausencias.py:357`; `app/ponto/turnos.py:2580`; `app/ponto/turnos.py:2595`; `app/ponto/turnos.py:2598`; `app/ponto/janelas.py:63`; `app/folha/export.py:580`)

**O filtro `status='aprovada', minutos>0` não é o universo integral dos leitores.** Para folha, o carregador filtra aprovadas, mas não exige minutos positivos; para cobrança, exclui apenas rejeitadas. Os carregadores também consideram interseção de datas e os intervalos abertos permitidos. (`app/ponto/turnos.py:2514`; `app/ponto/turnos.py:2516`; `app/ponto/turnos.py:2529`; `app/ponto/turnos.py:2753`)

Quem lê e como trata minutos:

| Leitor | Leitura efetiva | Fonte |
|---|---|---|
| Fechamento mensal | Carrega ausências para folha; soma minutos de tipos pagos e pula o efeito de dia inteiro quando esses minutos são não zero | `app/ponto/services/fechamento.py:630`, `:665`, `:685` |
| Efeitos da ausência | Usa os carregadores para folha e o mesmo teste de tipo pago com minutos não zero | `app/ponto/services/efeito_ausencia.py:43`, `:44`, `:49`, `:51` |
| Crédito parcial | Acumula minutos de tipos pagos pela data inicial | `app/ponto/services/credito_parcial.py:23`, `:24` |
| Exportação | Filtra aprovadas, tipos solicitados e interseção da janela, sem exigir minutos positivos | `app/folha/export.py:87`, `:88`, `:90` |

Nos três primeiros leitores, esses testes locais usam **tipo pago + verdade lógica de `minutos`**, sem chamar `cobre_parte_do_dia()` nesses pontos; portanto, não aplicam ali o limite de 480 nem a condição de dia único do juiz. São tratamentos locais distintos do predicado real. Reconstruções dentro **da sonda** permanecem indeterminadas. (`app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`; `app/ponto/services/efeito_ausencia.py:44`; `app/ponto/services/efeito_ausencia.py:51`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/turnos.py:2598`) **[nao verificado — conteúdo da sonda]**

**Parte B — saída fornecida.** Nela, **`falta` não está em `parciais_vigentes()`**: o resultado é `false`, e a lista contém `abon_hr`, `atestado`, `atraso`, `declaracao`, `saida_antecipada`. **[nao verificado — dado fornecido, sem nova consulta]**

A tabela abaixo preserva `n` e os identificadores recebidos, inclusive quando há vários registros para um mesmo identificador. **[nao verificado — dado fornecido]**

| Empresa | Competência | Exportada | Tipo | n | Colaboradores | Evidência |
|---:|:---:|:---:|---|---:|---|---|
| 2 | 06/2026 | false | falta | 1 | col43 | [nao verificado] |
| 4 | 06/2026 | true | falta | 3 | col39, col40, col47 | [nao verificado] |
| 3 | 07/2026 | true | abono | 1 | col148 | [nao verificado] |
| 3 | 07/2026 | true | falta | 2 | col64 | [nao verificado] |
| 4 | 07/2026 | true | falta | 1 | col676 | [nao verificado] |
| 2 | 08/2026 | true | abono | 3 | col666, col819, col905 | [nao verificado] |
| 2 | 08/2026 | true | atestado | 7 | col284, col342, col400, col433, col707, col842 | [nao verificado] |
| 2 | 08/2026 | true | falta | 46 | col187, col200, col228, col237, col272, col328, col335, col338, col344, col347, col366, col406, col433, col442, col489, col493, col552, col568, col718, col760, col783, col823, col874 | [nao verificado] |
| 2 | 08/2026 | true | saida_antecipada | 1 | col649 | [nao verificado] |
| 3 | 08/2026 | false | atestado | 1 | col159 | [nao verificado] |
| 3 | 08/2026 | false | falta | 8 | col58, col60, col122, col128, col743, col769 | [nao verificado] |
| 4 | 08/2026 | false | abono | 1 | col712 | [nao verificado] |
| 4 | 08/2026 | false | atestado_inss | 1 | col42 | [nao verificado] |
| 4 | 08/2026 | false | falta | 3 | col46, col50 | [nao verificado] |
| 2 | 09/2026 | true | atestado | 3 | col518, col885, col906 | [nao verificado] |
| 2 | 09/2026 | true | falta | 114 | col174, col200, col268, col304, col325, col327, col331, col338, col370, col406, col417, col437, col443, col492, col493, col558, col566, col572, col576, col584, col594, col622, col659, col666, col667, col673, col697, col704, col726, col735, col782, col786, col814, col823, col824, col855, col877, col894, col905, col906 | [nao verificado] |
| 2 | 09/2026 | true | suspensao | 1 | col417 | [nao verificado] |
| 3 | 09/2026 | true | atestado | 2 | col111, col643 | [nao verificado] |
| 3 | 09/2026 | true | falta | 16 | col92, col154, col161, col173, col502, col515, col643, col746, col876 | [nao verificado] |
| 3 | 09/2026 | true | suspensao | 1 | col90 | [nao verificado] |
| 4 | 09/2026 | true | atestado | 1 | col40 | [nao verificado] |
| 4 | 09/2026 | true | falta | 2 | col50, col510 | [nao verificado] |
| 1 | 10/2026 | false | falta | 1 | col950 | [nao verificado] |
| 2 | 10/2026 | false | atestado | 17 | col204, col252, col354, col389, col446, col511, col518, col558, col566, col618, col626, col889, col905, col920 | [nao verificado] |
| 2 | 10/2026 | false | falta | 83 | col55, col200, col206, col207, col248, col304, col307, col335, col370, col373, col427, col432, col444, col507, col569, col576, col577, col622, col655, col666, col697, col699, col718, col736, col833, col885, col889, col893, col924, col928, col929, col944, col951, col954, col966, col968 | [nao verificado] |
| 3 | 10/2026 | false | falta | 6 | col113, col173, col643, col752, col769 | [nao verificado] |
| 4 | 10/2026 | false | falta | 2 | col510, col857 | [nao verificado] |
| 2 | 11/2026 | false | atestado | 4 | col200, col701, col891, col967 | [nao verificado] |
| 2 | 11/2026 | false | falta | 20 | col298, col304, col364, col444, col566, col570, col723, col823, col869, col885, col905, col924, col928, col966 | [nao verificado] |
| 3 | 11/2026 | false | falta | 2 | col769, col984 | [nao verificado] |
| 2 | 09/2029 | false | falta | 1 | col667 | [nao verificado] |

**As linhas detalhadas somam 355, não 448.** Os 93 de `parcial_pelo_juiz` aparecem somente no agregado; sua distribuição por empresa, competência, exportada, tipo e colaboradores é **indeterminada**. **[nao verificado — cálculo e limites do dado fornecido]**

| Parcela | n | Evidência |
|---|---:|---|
| Linhas detalhadas com `exportada=false` — coincide numericamente com `aberta` | 151 | [nao verificado] |
| Linhas detalhadas com `exportada=true` — coincide com `exportada` | 204 | [nao verificado] |
| Subtotal detalhado | 355 | [nao verificado] |
| `parcial_pelo_juiz`, sem detalhamento fornecido | 93 | [nao verificado] |
| **Universo: 151 + 204 + 93** | **448** | [nao verificado] |

Combinações ausentes não foram preenchidas com zero; a composição individual dos 93 permanece indeterminada. **[nao verificado — ausência de detalhamento no dado]**

**Parte C(A) — competência exportada grava, com exceção explícita para 09/2026.** No modo de escrita, exportação gera aviso; os fechamentos abertos são salvos com `save()`, e os demais recebem `update()` dos campos calculados. Entretanto, `GUARDA_COMPETENCIA` contém **09/2026**, e os colaboradores das empresas exportadas nessa competência são excluídos do processamento, preservando seus fechamentos nessa chamada. (`app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:530`; `app/ponto/services/fechamento.py:532`; `app/ponto/services/fechamento.py:551`; `app/ponto/services/fechamento.py:555`; `app/ponto/services/fechamento.py:1121`; `app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:1144`)

Com `somente_leitura=True`, não grava `FechamentoMensal`; a combinação com `lavrar_dias_pagos=True` pode escrever `DiaPago`. (`app/ponto/services/fechamento.py:1112`; `app/ponto/services/fechamento.py:1175`)

**Parte C(B) — sim, há caminho indireto de cron para competência anterior.** `detectar_par_relampago` está agendado com `--dias 2 --apply --retratar`; a busca retrocede dois dias e pode atravessar a virada da competência. (`app/config/crons.py:605`; `app/ponto/management/commands/detectar_par_relampago.py:83`; `app/ponto/management/commands/detectar_par_relampago.py:335`)

A cadeia é: comando chama `_retratar(alvo)` → `retratar_batida()` → `_fechamento_por_evento()` com o timestamp da batida → `recalcular_por_evento()` → `recalcular_fechamento_mes()` na competência **do fato**, após commit. (`app/ponto/management/commands/detectar_par_relampago.py:363`; `app/ponto/management/commands/detectar_par_relampago.py:224`; `app/ponto/registro_batida.py:281`; `app/ponto/registro_batida.py:178`; `app/ponto/services/fechamento.py:1793`; `app/ponto/services/fechamento.py:1794`; `app/ponto/services/fechamento.py:1808`)

O comando mensal `recalcular_fechamento` está declarado fora do pipeline; isso não elimina o caminho indireto acima. (`app/config/crons.py:767`; `app/config/crons.py:1107`) Ocorrência efetiva desse recálculo em produção: **[nao verificado]**.

**Parte C(C) — outros caminhos capazes de recalcular o passado, além do botão:**

| Caminho | Chamada e evidência |
|---|---|
| Registro e retratação de batida | `app/ponto/registro_batida.py:155`, `:160`, `:178`, `:281` |
| Decisão de HE referente ao dia informado | `app/ponto/portas/he.py:202` |
| Folga aceita e resposta validada | `app/chamados/services/validacao.py:331`, `:407` |
| Regeneração de células do vínculo, por competência tocada | `app/ponto/portas/celula.py:414`, `:422`, `:427` |
| Regeneração de exportada, chamada pelo passivo validado | `app/chamados/services/passivo_validado.py:429`, `:438`; `app/ponto/portas/regen_exportada.py:179` |
| Comando `recalcular_fechamento --mes --ano --apply` | `app/ponto/management/commands/recalcular_fechamento.py:78`, `:79`, `:110`, `:118` |
| Comando `aplicar_09_corte_b` | `app/ponto/management/commands/aplicar_09_corte_b.py:212` |
| Comando `diff_reclassificar_partido`, com `--aplicar` | `app/ponto/management/commands/diff_reclassificar_partido.py:206`, `:213`, `:232` |
| Comando `aplicar_janela_he_total`, delegando ao comando mensal | `app/ponto/management/commands/aplicar_janela_he_total.py:145` |
| Scripts para 10/2026, nos respectivos ramos de aplicação | `bin/aplicar_o263_comp10.py:26`, `:48`, `:51`; `bin/relavrar_o118_col221.py:19`, `:52`, `:53`; `bin/relavrar_o119_col221.py:18`, `:51`, `:52`; `bin/r5_idempotencia_frota.py:26`, `:117` |

Esses caminhos continuam sujeitos à guarda de 09/2026 no escritor. (`app/ponto/services/fechamento.py:551`) Já `fechamento_lido()` também recalcula competências solicitadas, mas usa `somente_leitura=True`, sem persistir `FechamentoMensal`. (`app/ponto/services/fechamento.py:1743`)

`bin/relavrar_o120_col221_09.py:75` passa `permitir_exportada=True`, argumento ausente da assinatura atual; essa chamada é incompatível com a cópia. (`app/ponto/services/fechamento.py:439`)

A execução efetiva desses caminhos sobre os registros do dado fornecido permanece **[nao verificado]**.

## 06/11 02:04 · sessao a2114b554fe1 · - · - · origem=Ronald (tmux)

C(A): **exportada em geral grava `FechamentoMensal`**, mas **09/2026 exportada é preservada pela guarda ativa**: competência declarada em `app/ponto/services/fechamento.py:36`; empresas exportadas detectadas em `:530`; colaboradores dessas empresas excluídos do universo em `:551–555`. Para os demais casos, `status='aberto'` usa `save()` em `:1121–1128`; outros status usam `update()` dos campos calculados em `:1144–1174`. `somente_leitura=True` não grava o fechamento (`:1112–1120`), embora a combinação com `lavrar_dias_pagos=True` possa gravar `DiaPago` (`:1175`).

C(C): além do botão, recalculam competências passadas pelos seguintes caminhos do código:

- **Eventos retroativos:** `recalcular_por_evento` determina a competência pela data do fato e chama o escritor (`app/ponto/services/fechamento.py:1793–1794`), após commit (`:1808`). Chamadores: batida (`app/ponto/registro_batida.py:155`, `:160`, `:178`); decisão HE (`app/ponto/portas/he.py:202`); folga aceita (`app/chamados/services/validacao.py:331`); resposta validada (`:407`); regeneração de vínculo, para cada competência tocada (`app/ponto/portas/celula.py:414–429`).
- **Retificação de exportada:** `regenerar_em_exportada` chama diretamente o escritor (`app/ponto/portas/regen_exportada.py:179`); o serviço de passivo validado chama essa porta quando houve plantio em exportada (`app/chamados/services/passivo_validado.py:429–439`).
- **Comando operacional:** `recalcular_fechamento --mes --ano --apply` (`app/ponto/management/commands/recalcular_fechamento.py:77–81`, `:110–120`).
- **Outros comandos que podem persistir:** `aplicar_09_corte_b.py:212`; `diff_reclassificar_partido.py:206` com commit no ramo `--aplicar` (`:213–214`, `:232–238`); `aplicar_janela_he_total.py:145` delega ao comando operacional. [nao verificado]
- **Scripts existentes:** `bin/aplicar_o263_comp10.py:26`, `:48–52`; `bin/relavrar_o118_col221.py:19`, `:52–53`; `bin/relavrar_o119_col221.py:18`, `:51–52`; `bin/r5_idempotencia_frota.py:26`, `:117`.

`bin/relavrar_o120_col221_09.py:75` passa `permitir_exportada=True`, argumento ausente da assinatura atual (`app/ponto/services/fechamento.py:439–442`); essa chamada está incompatível com a cópia.

Medidores com recálculo seguido de rollback: `desvio_o68b.py:93–95`, `folga_que_sumiu.py:69–71`, `plano_b_no_dinheiro.py:109–112`. `celula_veredito_velho.py` desliga recálculo no apply (`:95–99`); no ensaio, recalcula e reverte (`:146–157`).

Ocorrência efetiva desses caminhos em produção: **[não verificado]**.

## 06/11 02:04 · sessao 573d59eacfa9 · - · - · origem=Ronald (tmux)

**C(B): sim, existe caminho indireto de cron para recálculo de competência anterior.** O cron `detectar_par_relampago` roda com `--dias 2 --apply --retratar`; o recorte usa hoje menos dois dias, sem limitar pela competência corrente. Assim, na virada da janela, pode alcançar a anterior. (`app/config/crons.py:605`; `app/ponto/management/commands/detectar_par_relampago.py:335`; `app/ponto/management/commands/detectar_par_relampago.py:343`)

Cadeia executável:

- O comando chama `_retratar(alvo)` ou `_retratar` nas duas batidas do par. (`app/ponto/management/commands/detectar_par_relampago.py:363`; `app/ponto/management/commands/detectar_par_relampago.py:399`)
- `_retratar` chama `retratar_batida`. (`app/ponto/management/commands/detectar_par_relampago.py:224`)
- `retratar_batida` chama `_fechamento_por_evento` com o **timestamp da batida retratada**. (`app/ponto/registro_batida.py:281`)
- O helper chama `recalcular_por_evento`; este resolve a competência daquele fato e chama `recalcular_fechamento_mes`, após commit. (`app/ponto/registro_batida.py:177`; `app/ponto/registro_batida.py:178`; `app/ponto/services/fechamento.py:1793`; `app/ponto/services/fechamento.py:1794`; `app/ponto/services/fechamento.py:1808`)
- A competência é obtida pela data do fato, através de `competencia_do_fato` → `competencias_do_fato` → `competencias_do_intervalo`. (`app/ponto/services/fechamento.py:1588`; `app/ponto/services/fechamento.py:1578`; `app/ponto/services/fechamento.py:1583`)

A declaração de que `recalcular_fechamento` está fora do pipeline refere-se ao comando mensal direto; ela não elimina essa cadeia indireta. (`app/config/crons.py:767`; `app/config/crons.py:1107`)

O cron `pre_fechamento --apply` escreve pautas e apenas consulta os marcos da competência anterior; seu corpo não chama o recálculo mensal. (`app/config/crons.py:406`; `app/folha/management/commands/pre_fechamento.py:59`; `app/folha/management/commands/pre_fechamento.py:61`; `app/folha/management/commands/pre_fechamento.py:73`; `app/folha/management/commands/pre_fechamento.py:74`)

Ocorrência efetiva desse recálculo indireto em produção, frequência e colaboradores atingidos: **[não verificado]**.

## 06/11 02:04 · sessao 07f1d044bfe0 · - · - · origem=Ronald (tmux)

- `status='aprovada'` é o universo por status da folha: `_pelo_efeito(..., para='folha')` filtra aprovada; cobrança lê todas exceto rejeitada. `minutos>0` não integra esse carregador: `ausencias_do_periodo` e `cobertura_ausencia_periodo` leem interseção de datas, incluindo ranges abertos permitidos, sem filtro de minutos. Portanto aprovada + minutos>0 é recorte dos leitores, não seu universo integral. (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2529`, `app/ponto/turnos.py:2753`)

- Quem lê esse universo: fechamento carrega `ausencias_do_periodo(..., para='folha')`; efeitos da ausência e crédito parcial fazem a mesma chamada. (`app/ponto/services/fechamento.py:629`, `app/ponto/services/fechamento.py:630`, `app/ponto/services/efeito_ausencia.py:43`, `app/ponto/services/credito_parcial.py:23`)

- As três implementações de dinheiro usam **tipo pago e minutos não zero**, sem perguntar `cobre_parte_do_dia`: fechamento soma minutos e pula o efeito de dia inteiro; efeitos da ausência faz o mesmo; crédito parcial acumula por data. Isso difere do juiz, que exige tipo em parciais, registro de um só dia e `0 < minutos < 480`. Ressalva: o código usa verdade lógica de `minutos`, não comparação literal `>0`; o campo é `IntegerField`. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2595`, `app/ponto/turnos.py:2598`, `app/ponto/models.py:755`)

- A tela do espelho e o PDF leem `efeitos_da_ausencia`; o fechamento também usa `creditos_por_dia`. (`app/ponto/services/espelho.py:1048`, `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332`, `app/relatorios/pdf_espelho.py:333`, `app/ponto/services/fechamento.py:978`, `app/ponto/services/fechamento.py:983`)

- Exportação tem outro leitor: `ausencias_na_janela` filtra aprovadas, tipos pedidos e interseção da janela, sem filtro de minutos; `minutos_de_falta_por_dia` usa tipos que descontam e rateia minutos pelos dias. (`app/folha/export.py:87`, `app/folha/export.py:88`, `app/folha/export.py:90`, `app/folha/export.py:138`, `app/folha/export.py:140`, `app/folha/export.py:141`)

- Não localizei `sonda_h12h06b.py` nem com `rg --files -uuu`; ausência do script impede conferir as chamadas que ele efetivamente fez ou identificar reconstruções dentro dele. [não verificado]

## 06/11 01:57 · sessao 43ff11e3f048 · H12H06-B · verificador · origem=Code

**Alta — a cura de #2 não fecha.** `mudar_estado` grava status, minutos e range sem mover `atualizado_em`; depois tenta registrar a trilha. Essa tentativa pode falhar sem desfazer a ausência, e `registrar_log` não verifica o retorno. (`app/ponto/services/ausencia.py:818`, `app/ponto/services/ausencia.py:820`, `app/core/auditoria.py:22`, `app/core/observ.py:105`, `app/core/observ.py:139`)

Contraexemplo: depois do apply, rejeitar e reabrir um atestado, com falha nas duas trilhas, devolve status/minutos/identidade/carimbo aos valores esperados. A reversão não encontra log posterior e restaura 720 sobre o ato posterior. Há também uma janela sem falha definitiva: duas chamadas podem concluir os saves da ida/volta e ficar suspensas antes dos respectivos logs. (`app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:1125`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`) Ocorrência em produção: [não verificado].

**Média — #3 fecha o relato da linha, mas permanece parcialmente aberto para recuperação.** O resultado recebe a linha antes da confirmação, conforme pretendido. Porém, se uma falha de escrita deixar a confirmação final truncada, o leitor executa `json.loads` sobre todas as linhas e aborta antes de montar qualquer reversão, inclusive das linhas anteriores corretamente confirmadas. Falta tolerância ao fragmento final do diário. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:270`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:254`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:222`) Reprodução dessa falha: [não verificado].

**Alta — contesto o descarte categórico da corrida com exportação.** O comando trava somente a ausência, consulta a cerca e depois escreve. A consulta de competência entregue não estabelece trava compartilhada com o exportador, que registra `ExportacaoDominio` independentemente. Portanto, um export concorrente pode entregar a competência entre a conferência e a alteração. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:160`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`, `app/folha/export.py:581`, `app/folha/models.py:182`, `app/folha/views.py:297`) Ocorrência e fundamento do descarte anterior: [não verificado].

Quanto às demais curas e hipóteses:

- **#1 fecha o caso descrito:** o objeto é recarregado e o carimbo retornado dentro da transação que mantém a trava. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:173`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:189`)
- **PK repetido:** a confirmação consome a última foto pendente daquele PK; não reutiliza indiscriminadamente todas as fotos anteriores. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:233`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:235`)
- **PK de log versus commit:** o filtro compara um identificador automático, não uma sequência de commits. Não identifiquei escritor concreto com log menor e alteração posterior da mesma ausência que atravesse todas as demais guardas; esse contraexemplo permanece [não verificado]. (`app/core/migrations/0002_log_auditoria.py:19`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- **Trilha própria da reversão:** ela também grava evento e confirma seu próprio PK; o filtro estritamente `pk__gt` não conta essa mesma trilha como posterior. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- **Push do apply desfeito:** existe caminho síncrono entre save, julgamento e envio antes das verificações que podem provocar rollback. Entretanto, alcançar esse envio nas 30 linhas depende de condições não fornecidas; não o promovo a achado específico desta obra. (`app/ponto/services/ausencia.py:508`, `app/ponto/signals.py:147`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:778`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175`) Alcance nas 30: [não verificado].

**Quem escreve LogAuditoria com `modelo='Ausencia'`:**

| Origem | Arquivo:linha |
|---|---|
| Criação, estado, documento e período | `app/ponto/services/ausencia.py:441`, `:820`, `:854`, `:915` |
| Cancelamento em lote, aprovação apesar do anexo, cancelamento individual e edição | `app/ponto/views.py:2357`, `:2614`, `:2739`, `:2808` |
| Lançamento pela IA | `app/core/views_ia.py:991` |
| Devolução de perguntas absorvidas | `app/chamados/services/materializacao.py:663` |
| Cura do fim | `app/ponto/management/commands/curar_ausencia_sem_fim.py:47` |
| Apply e reversão desta obra | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182` |
| Migrações históricas, diretamente | `app/ponto/migrations/0057_a4_backfill_pendente.py:44`, `app/ponto/migrations/0058_a4_relogio_recomeca.py:43`, `app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:37` |

O escritor físico atual é `evento`; `registrar_log` é adaptador. (`app/core/observ.py:134`, `app/core/auditoria.py:22`)

**Quem escreve Ausencia sem trilha própria:**

- `editar_ausencia_registro` e `rederivar_fim` fazem save completo; os chamadores identificados registram a trilha. (`app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:1280`, `app/ponto/views.py:2808`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`)
- A rejeição de férias não registra trilha da ausência, mas move explicitamente `atualizado_em`. (`app/ponto/services/ausencia.py:1267`)
- Há exclusões sem trilha própria; a reversão detecta que a linha desapareceu. (`app/ponto/services/ausencia.py:1174`, `app/ponto/services/ausencia.py:1197`, `app/ponto/services/ausencia.py:1254`, `app/ponto/services/ausencia.py:1274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:140`)
- Há saves de metadados sem carimbo/log próprio: prazo; decisão sem mudança de estado e sem período; documento/descrição/prazo na reabertura. Eles não demonstram, isoladamente, alteração invisível de minutos/identidade/status. (`app/ponto/services/ausencia.py:536`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:1114`, `app/ponto/services/ausencia.py:1124`, `app/ponto/services/ausencia.py:1132`)

**Modelo/objeto diferentes:** o cancelamento de férias altera ausências e registra o ato em `AgendamentoFerias`, com o PK do agendamento; a alteração das ausências carimba `atualizado_em`. O aviso de recusa registra `Colaborador`, mas não altera uma ausência. Nenhum desses caminhos fornece o contraexemplo invisível procurado. (`app/ferias/services.py:356`, `app/ferias/views.py:410`, `app/ponto/services/ausencia.py:1267`, `app/api/views.py:1937`)

## 06/11 01:57 · sessao d295a10a7b09 · - · - · origem=Ronald (tmux)

**Achado adicional sólido, média — #3 continua parcial se a confirmação ficar truncada.** O diário faz `write`, `flush` e `fsync`; uma falha que deixe um fragmento JSON final não é tratada ([comando:254](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:254)). A reversão interpreta todas as linhas com `json.loads` antes de montar o plano; o fragmento aborta também a reversão das linhas anteriores confirmadas ([comando:222](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:222)). Ocorrência real: [não verificado].

Se a última confirmação falhar e o `finally` conseguir gravar o resultado, `completo` será `true`: a última linha já entrou em `saida` antes da confirmação ([comando:270](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:270), [comando:285](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:285)). **Não elevar isoladamente:** o próprio predicado define completude como quantidade de linhas processadas, que pode estar completa apesar da confirmação ausente ([comando:285](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:285)).

**Sem novo achado de `on_commit`:** o consumidor de Ausencia envolve o trabalho em `engolir`; não há nesse callback um caminho útil desprotegido demonstrado que impeça o retorno após commit ([signals:254](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/signals.py:254), [signals:260](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/signals.py:260), [signals:263](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/signals.py:263)). A trilha auxiliar `perguntas_devolvidas_ausencia_rejeitada` exige status rejeitada, ausente da LISTA ([materializacao:638](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/chamados/services/materializacao.py:638), [comando:51](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:51)).

**PK versus commit:** escritor concreto com log anterior à escrita, pk menor e alteração pós-apply capaz de passar todas as guardas: [não verificado]. Não promover esse risco genérico. [nao verificado]

## 06/11 01:55 · sessao d295a10a7b09 · - · - · origem=Ronald (tmux)

- **Alta — cura #2 não fecha quando a trilha falha.** `mudar_estado` salva `status`, `minutos` e `data_fim` sem `atualizado_em`, depois chama a auditoria; não exige sucesso dela ([ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:807), [ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:818)). `evento` engole a falha e retorna `None`, e `registrar_log` descarta esse retorno ([observ.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/observ.py:105), [observ.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/observ.py:139), [auditoria.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/auditoria.py:22)). Contraexemplo: após apply, rejeitar e reabrir o atestado com falha nos dois logs deixa identidade, minutos e carimbo iguais; a reversão aceita e restaura 720, porque só procura logs existentes com pk maior ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152)). Ocorrência em produção: [não verificado].

- **Alta — mesma cura possui janela mesmo sem falha definitiva.** A escrita de `mudar_estado` termina antes da chamada de auditoria, sem transação englobando ambas ([ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:792), [ausencia.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/services/ausencia.py:819)). Duas chamadas intercaladas podem completar a ida/volta de estado e parar antes dos respectivos logs; nesse intervalo a reversão não encontra trilha posterior ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157)). Interleaving executado: [não verificado].

- **Cura #1 fecha o caso descrito:** o objeto é recarregado e o carimbo devolvido dentro do `atomic` que mantém a trava ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:173), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:189)).

- **Trilha própria da reversão:** ela grava evento com o próprio pk e confirma esse pk; o filtro usa `pk__gt`, portanto não conta essa mesma trilha como ato posterior ([comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274), [comando](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157)).

- **PK versus commit:** o modelo não contém campo de ordenação de commits; `pk__gt` mede pk, e a criação acontece dentro da transação do chamador ([models.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/models.py:285), [observ.py](/home/ronald/codex_ws/4e9380e52b1a12a052e62a64b664c8c316ab414c/app/core/observ.py:133)). Um escritor concreto com log menor e alteração da mesma ausência depois do apply: [não verificado]; não apresento a hipótese genérica como achado.

## 06/11 01:55 · sessao b77f0a7fd3d0 · - · - · origem=Ronald (tmux)

- **Exportação — corrida estrutural, gravidade alta se descarte negava a possibilidade.** A trava cobre somente Ausencia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`); a cerca consulta entrega antes da escrita (`:160`, `:169`), enquanto `competencia_entregue` apenas consulta (`app/folha/export.py:581`) e o export registra sem trava compartilhada (`app/folha/models.py:182`, chamado em `app/folha/views.py:297`). Um export pode registrar entre a cerca e a alteração; ocorrência ou disponibilidade operacional na produção: [não verificado].

- **Push do apply desfeito — caminho síncrono existe, mas alcance específico não demonstrado.** O save (`app/ponto/services/ausencia.py:508`) dispara julgamento síncrono (`app/ponto/signals.py:143`, `:147`, `:93`), que pode emitir (`app/ponto/services/cartorio.py:1740`, `:1778`) e enviar push (`app/ponto/management/commands/detectar_ausencias.py:778`) antes dos checks que provocam rollback (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175`, `:178`, `:186`). Gravidade média condicional; células, vínculos, chamados e gates necessários para alcançar essas emissões nas 30 linhas: [não verificado]. O ramo flexível cala o push (`app/ponto/management/commands/detectar_ausencias.py:777`), e a cobertura parcial depende de tipo cadastrado e minutos abaixo de 480 (`app/ponto/turnos.py:2595`).

Autores explícitos de LogAuditoria com modelo `Ausencia` fora de `services/ausencia.py`:

- `app/core/views_ia.py:991`: lançamento por IA.
- `app/ponto/views.py:2357`: cancelamento em lote.
- `app/ponto/views.py:2614`: confirmação de aprovação apesar do anexo.
- `app/ponto/views.py:2739`: cancelamento individual.
- `app/ponto/views.py:2808`: edição.
- `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`: apply e reversão.
- `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`: cura de fim.
- `app/chamados/services/materializacao.py:663`: devolução de perguntas absorvidas após rejeição; grava no pk da ausência, embora altere perguntas/disputas (`:657`–`:665`).

Todos esses chamadores usam `evento` direta ou indiretamente: `app/core/auditoria.py:22`, escritor físico em `app/core/observ.py:134`.

Os anexos originais de triagem/opus mencionados não foram disponibilizados no dado: [não verificado].

## 06/11 01:55 · sessao 674011c3259a · - · - · origem=Ronald (tmux)

**Alta — #2 permanece aberto quando a trilha falha.** `mudar_estado` persiste `status`, `minutos` e `data_fim` sem `atualizado_em`, depois tenta auditoria (`app/ponto/services/ausencia.py:807`, `:818`, `:819`, `:820`). `evento` admite falha e retorna `None`, preservando a alteração de negócio; `registrar_log` ignora o retorno (`app/core/observ.py:105`, `:133`, `:139`; `app/core/auditoria.py:22`). Assim, ida e volta de status com falhas nas duas trilhas pode deixar identidade/minutos/carimbo iguais e passar pela reversão (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142`, `:154`, `:157`). Ocorrência em produção: [não verificado].

**Quem registra `modelo='Ausencia'`:**

- `app/ponto/services/ausencia.py:441`: criação.
- `app/ponto/services/ausencia.py:820`: mudança de estado.
- `app/ponto/services/ausencia.py:854`: documento anexado.
- `app/ponto/services/ausencia.py:915`: período parcial.
- `app/ponto/views.py:2357`: cancelamento em lote.
- `app/ponto/views.py:2614`: aprovação apesar do anexo.
- `app/ponto/views.py:2739`: cancelamento individual.
- `app/ponto/views.py:2808`: edição.
- `app/core/views_ia.py:991`: criação pela IA.
- `app/chamados/services/materializacao.py:663`: devolução das perguntas absorvidas.
- `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`: cura do fim.
- `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`: apply **e reversão**.
- Migrações históricas: `app/ponto/migrations/0057_a4_backfill_pendente.py:44`, `app/ponto/migrations/0058_a4_relogio_recomeca.py:43`, `app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:37`.

Escritor físico atual: `app/core/observ.py:134`; adaptador: `app/core/auditoria.py:22`.

**Quem escreve Ausencia sem trilha própria:**

- `editar_ausencia_registro`: `app/ponto/services/ausencia.py:508`; save completo, com log pelos chamadores conhecidos `app/ponto/views.py:2808` e comando `:182`.
- `rederivar_fim`: `app/ponto/services/ausencia.py:1280`; log no chamador `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`.
- `rejeitar_ausencias_de_ferias`: `app/ponto/services/ausencia.py:1265`; sem log, mas move explicitamente `atualizado_em` em `:1267`.
- Exclusões sem trilha da Ausencia: `app/ponto/services/ausencia.py:1174`, `:1197`, `:1254`, `:1274`; a reversão recusa linha ausente (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:140`).
- Escritas de metadados sem carimbo/log próprio: `app/ponto/services/ausencia.py:536`, `:911` sem período e estado inalterado, `:1114`, `:1124`, `:1132`. Esses caminhos isolados não demonstram mudança de minutos/identidade/status: [não verificado].

Log com outro modelo encontrado: aviso de recusa em `Colaborador`, sem criação da ausência (`app/api/views.py:1937`); correção de férias em `AgendamentoFerias`, com exclusão/recriação do espelho (`app/ferias/views.py:497`; `app/ferias/services.py:385`, `:387`). Escritor invisível que altere a mesma ausência preservada nesses caminhos: [não verificado].

## 06/11 01:16 · sessao 9d598bca1a77 · C20-FIO-MUDO · verificador · origem=Code

**Não certificaria a obra completa.** O delta corrige captura do callback, cerca de competência e savepoint, mas continuam sem cobertura quatro atos de perda; há também uma janela concorrente na adoção. A linha permanece **ABERTA**. (`app/chamados/models.py:468`, `app/chamados/models.py:521`, `app/docs/PROMPTS.md:1882`, `app/docs/BACKLOG.md:1764`; janela detalhada no item 3.)

Execução dos testes, RED/GREEN, mutantes e efeitos atuais em produção: **[nao verificado]**. [nao verificado]

1. **Achados do opus final**

   | Achado | Avaliação do delta |
   |---|---|
   | **(1) Callback sem captura** | **Resolve.** O wrapper agora captura a exceção com `engolir`; este registra ERROR e incrementa o contador de engolidos. R20a exige execução do callback seguinte. (`app/chamados/models.py:521`, `app/core/observ.py:73`, `app/core/observ.py:86`, `app/chamados/tests/test_c20_fio_mudo.py:337`) |
   | **(2) Cerca contra L-050 e sem teste que morde** | **Resolve o achado específico.** A adoção deixou de consultar competência entregue; R20c cria exportação vigente e exige reapontamento. A prova não cobre o fluxo posterior inteiro. (`app/chamados/models.py:475`, `app/chamados/models.py:479`, `app/chamados/tests/test_c20_fio_mudo.py:385`, `app/chamados/tests/test_c20_fio_mudo.py:397`, `app/docs/LEIS.md:91`) |
   | **(3) Cinco atos/estoque incompletos** | **Não resolve.** Os pontos de entrada continuam sendo nascimento efetivo e reabertura; o dedup de chamado já vivo retorna antes da adoção. A obra segue nomeada ABERTA. (`app/chamados/models.py:965`, `app/chamados/models.py:983`, `app/chamados/models.py:1033`, `app/chamados/models.py:1502`, `app/docs/BACKLOG.md:1764`) |
   | **(4) M3 sobrevivente sem equivalência declarada** | **Não demonstrado como resolvido.** Definição do M3, nova execução e justificativa de equivalência: **[nao verificado]**. |
   | **(5) `savepoint=False` tornando emissão refém** | **Resolve a estrutura transacional apontada.** Agora existe savepoint próprio, com captura por fora; R20b alcança erro SQL e consulta depois dele. Falta provar rollback após escrita parcial. (`app/chamados/models.py:468`, `app/chamados/models.py:526`, `app/chamados/tests/test_c20_fio_mudo.py:362`) |

   Os achados **(3) e (4) continuam pendentes**; o primeiro está nomeado no BACKLOG, enquanto a resolução documental do M3 permanece **[nao verificado]**. (`app/docs/BACKLOG.md:1764`.)

2. **Quem escreve e quem lê**

   | Campo/ato | Escritores | Leitores relevantes |
   |---|---|---|
   | `PerguntaDisputa.chamado_destino` | `fio.reapontar_resolvedora`, com julgamento e lavra no mesmo save; nascimento em `PerguntaDisputa.abrir`. **O ramo de identidade de `abrir` também reaponta pergunta existente diretamente.** (`app/chamados/services/fio.py:829`, `app/chamados/services/fio.py:845`, `app/chamados/models.py:2550`, `app/chamados/models.py:2574`, `app/chamados/models.py:2576`) | Âncora, resolução da disputa do chamado, API e fila de validação. (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:131`, `app/api/views.py:1007`, `app/chamados/services/validacao.py:793`) |
   | `PerguntaDisputa.via_resolucao` | Núcleo `_escrever_carimbo`, portas de carimbar/identidade/descarimbar e limpeza em lote; atos próprios em `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. (`app/chamados/models.py:2656`, `app/chamados/models.py:2662`, `app/chamados/models.py:2725`, `app/chamados/models.py:2746`, `app/chamados/models.py:2763`, `app/chamados/models.py:2773`, `app/chamados/services/relavra_via.py:123`) | C6, exclusão de duplicatas na adoção e idempotência da retratação. (`app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/models.py:481`, `app/chamados/services/fio.py:882`) |
   | Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `fechar` grava os quatro; `reabrir_admin` e `reabrir_sistema` limpam os quatro. `limpar_resolucao_residual` é porta estreita para somente resolução. (`app/chamados/models.py:1961`, `app/chamados/models.py:2163`, `app/chamados/models.py:2187`, `app/chamados/models.py:2270`) | Propriedade `aberta`, idempotência do fechamento, resolução do fio e API de disputas abertas. (`app/chamados/models.py:1864`, `app/chamados/models.py:1920`, `app/chamados/services/fio.py:131`, `app/api/views.py:1028`) |
   | `ChamadoColaborador.status_local` | Transições por `_gravar_estado`; nascimento pelo estado decidido pelo motor nas portas de criação. (`app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/models.py:994`, `app/chamados/models.py:1030`, `app/chamados/models.py:1155`) | Adoção, guarda da costura, fila e painel. (`app/chamados/models.py:469`, `app/chamados/models.py:486`, `app/chamados/services/fio.py:764`, `app/chamados/services/fila.py:126`, `app/chamados/services/painel.py:211`) |
   | Evento `pergunta_reatada` | Gate de fechamento e nova adoção; gravação física por `core.observ.evento`. (`app/chamados/models.py:423`, `app/chamados/models.py:508`, `app/core/observ.py:94`) | Auditoria genérica consulta `LogAuditoria`; consumidor específico dessa ação: **[nao verificado]**. (`app/core/views.py:705`) |
   | Ocorrência `disputa_reaberta_sistema` | `reabrir_sistema`, pela porta de nota, **no chamado-pai da disputa**, quando existe. (`app/chamados/models.py:2192`, `app/chamados/services/nota.py:102`) | Followups do fio e histórico. (`app/chamados/services/fio.py:171`, `app/chamados/services/historico.py:80`) |
   | `FechamentoMensal` | Porta de fechamento: criação/cálculo, atualização, aprovação, reabertura e restauração. (`app/ponto/services/fechamento.py:889`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1307`, `app/ponto/services/fechamento.py:1342`, `app/ponto/services/fechamento.py:1433`) | Montagem do TXT lê fechamentos e classificação de entrada. (`app/folha/export.py:692`, `app/folha/export.py:695`, `app/folha/export.py:698`) |
   | `ExportacaoDominio` | `registrar` cria conteúdo/hash históricos; `invalidar` altera invalidadores preservando conteúdo/hash. (`app/folha/models.py:143`, `app/folha/models.py:174`, `app/folha/models.py:182`) | Competência entregue, vigência, histórico e download da cópia guardada. (`app/folha/export.py:569`, `app/folha/models.py:121`, `app/folha/views.py:554`) |

   **Existe segundo sítio de reapontamento fora de `reapontar_resolvedora`, preexistente ao delta.** Ele consulta os mesmos juízes, mas repete a escrita no ramo de identidade de `PerguntaDisputa.abrir`; o selo admite ambos os arquivos e não impede essa duplicação de sítio. (`app/chamados/models.py:2538`, `app/chamados/models.py:2550`, `app/chamados/models.py:2574`, `app/core/tests/test_contract_escritores_estado.py:61`.)

   **Existe estado intermediário observável:** pergunta já reatada, disputa ainda fechada. `_disputa_do_chamado` e a API que seleciona disputas abertas não a encontram nesse intervalo; o fio do admin admite vivas em disputas fechadas. Não cabe afirmar invisibilidade universal ao colaborador, porque `perguntas_do_app` consulta C6 sem excluir disputa fechada. (`app/chamados/models.py:524`, `app/chamados/services/fio.py:131`, `app/api/views.py:1028`, `app/chamados/services/fio.py:209`, `app/chamados/services/pergunta_no_app.py:46`.)

3. **Transação, exceções e travas**

   - **Em `abrir()`:** a transação externa trava Colaborador e cria o chamado antes da adoção. O `engolir` envolve o `atomic()` interno; assim, uma exceção propagada desfaz primeiro o savepoint e só depois é capturada. Para `DatabaseError` recuperável, com rollback bem-sucedido, a estrutura preserva a transação externa. Recuperação observada nesta cópia: **[nao verificado]**. (`app/chamados/models.py:961`, `app/chamados/models.py:964`, `app/chamados/models.py:1030`, `app/chamados/models.py:468`, `app/core/observ.py:86`.)

   - **O `savepoint=False` de `reapontar_resolvedora` continua existindo**, mas fica dentro do novo savepoint da adoção. Erro propagado desse núcleo alcança a proteção externa; isso preserva a emissão, sem garantir a costura. (`app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:468`, `app/chamados/models.py:503`, `app/chamados/models.py:526`.)

   - **Em `reabrir()` direto:** não existe atomic envolvendo o método inteiro. `_gravar_estado` tem atomic próprio, depois vêm ressurreição e adoção. Sem atomic do chamador, falha capturada da adoção não desfaz a reabertura anterior; falha na ressurreição pode ocorrer antes de alcançar a nova proteção. (`app/chamados/models.py:274`, `app/chamados/models.py:1492`, `app/chamados/models.py:1501`, `app/chamados/models.py:1604`, `app/chamados/models.py:1632`.)

   - **Callback levantando:** seu serviço tem atomic próprio, e a captura fica por fora. A falha não desfaz a costura já comprometida; pode deixar a disputa fechada, com ERROR e contador de sobra. O contador declara dono ADMIN e “NAO alarma”; ele não é reparador nem retentativa. (`app/chamados/services/fio.py:912`, `app/chamados/models.py:521`, `app/core/observ.py:79`, `app/chamados/services/vigia_fio.py:740`, `app/chamados/services/vigia_fio.py:748`.)

   - **Ordem explícita:** adoção por `abrir`: Colaborador → chamado adotante → escrita de Pergunta; adoção direta: chamado adotante → escrita de Pergunta. O callback trava Pergunta e Disputa, ordenadas por disputa/pk, e depois julga C6. A relavra existente também toma Pergunta+Disputa. (`app/chamados/models.py:964`, `app/chamados/models.py:469`, `app/chamados/services/fio.py:847`, `app/chamados/services/fio.py:913`, `app/chamados/services/relavra_via.py:221`.)

   - **Lacuna concorrente:** candidatas e status das âncoras antigas são lidos sem trava. Outra costura pode transferir a pergunta a outro chamado vivo e comprometer antes da escrita da adoção; esta usa `_anc` antigo e sobrescreve o destino sem reler a Pergunta. A trava do Colaborador não serializa a porta independente de reapontamento. É um intercalamento permitido pela estrutura; ocorrência em produção: **[nao verificado]**. (`app/chamados/models.py:479`, `app/chamados/models.py:485`, `app/chamados/models.py:491`, `app/chamados/models.py:503`, `app/chamados/services/fio.py:829`, `app/chamados/services/fio.py:845`.)

   - **Não há prova de ausência de deadlock.** O callback não trava explicitamente Chamado/Colaborador, mas insere nota com FK no chamado-pai; locks implícitos e adiamento das constraints efetivas: **[nao verificado]**. Também não verifica que o chamado continua vivo: verifica FK de destino e C6. (`app/chamados/models.py:2194`, `app/chamados/services/nota.py:103`, `app/chamados/services/fio.py:915`, `app/chamados/services/fio.py:917`.)

4. **Retirada da cerca de competência**

   **L-050 governa chamado/pergunta:** a fronteira do exportado foi retirada por desenho. Para gravado, recalculo e TXT, é necessário ler também L-113, que altera L-092/L-052 e preserva TXT anterior, trilha e DIFF antes de ato de frota. CORTES reafirma dia exportado editável. (`app/docs/LEIS.md:91`, `app/docs/LEIS.md:150`, `app/docs/CORTES.md:219`.)

   A adoção escreve pergunta/lavra, identidade das irmãs, auditoria e agenda reabertura; não aparece nela escrita direta de fechamento, exportação ou célula. **Isso não prova imutabilidade do fluxo posterior:** validação chama recalculo do fechamento pela data do fato, e o fechamento tem escritor dos valores gravados. (`app/chamados/models.py:503`, `app/chamados/models.py:516`, `app/chamados/models.py:524`, `app/chamados/services/validacao.py:404`, `app/chamados/services/validacao.py:407`, `app/ponto/services/fechamento.py:1144`.)

   A célula conserva sua autoridade separada, `competencia_fechada_para_celula`, composta de TXT vigente e holerite publicado; retirar a cerca da adoção não remove essa guarda. (`app/folha/export.py:578`, `app/folha/export.py:599`, `app/ponto/management/commands/gerar_celulas.py:146`.)

   Há uma borda adicional sem prova: o callback reabre **o continente inteiro**, embora tenha selecionado uma pergunta reatada. CORTES distingue a apresentação de vivas em disputa fechada, “sem reabrir, sem push”; o efeito sobre outras perguntas de uma disputa mista não é exercitado pelo R20c. Conformidade desse cenário completo: **[nao verificado]**. (`app/chamados/services/fio.py:920`, `app/api/views.py:1043`, `app/docs/CORTES.md:327`, `app/chamados/tests/test_c20_fio_mudo.py:390`.)

5. **Os testes mordem?** [nao verificado]

   - **R20a: sim, captura e continuidade.** Mock no endereço correto, callback executado enquanto o patch está ativo, ERROR e callback seguinte exigidos. Não prova commit real nem liberação de locks, pois usa `TestCase` e captura artificial de callbacks. (`app/chamados/tests/test_c20_fio_mudo.py:32`, `app/chamados/tests/test_c20_fio_mudo.py:337`, `app/chamados/tests/test_c20_fio_mudo.py:346`.)
   - **R20b: sim, erro SQL e transação utilizável nesse cenário; parcialmente quanto ao rollback prometido.** Quebra na eleição, antes da primeira costura. “Destino ficou igual” passaria sem qualquer escrita anterior; falta escrever primeiro e falhar depois. (`app/chamados/tests/test_c20_fio_mudo.py:362`, `app/chamados/tests/test_c20_fio_mudo.py:372`, `app/chamados/models.py:500`, `app/chamados/models.py:503`.)
   - **R20c: sim, antiga cerca.** Exportação vigente impede vacuidade do cenário. Compara todos os campos do fechamento, mas só hash/conteúdo da exportação; não executa callback, não fotografa célula nem percorre resposta/materialização posterior. (`app/chamados/tests/test_c20_fio_mudo.py:385`, `app/chamados/tests/test_c20_fio_mudo.py:398`, `app/chamados/tests/test_c20_fio_mudo.py:401`, `app/folha/export.py:569`.)
   - **R08j:** prova adiamento, respeito ao resultado C6 e idempotência; continuaria passando se C6 fosse avaliado antes da trava. Não demonstra intercalamento concorrente. (`app/chamados/tests/test_c20_fio_mudo.py:246`, `app/chamados/tests/test_c20_fio_mudo.py:254`, `app/chamados/tests/test_c20_fio_mudo.py:262`.)
   - **R08g e R19b:** alcançam os ramos, exigindo chamada efetiva da porta e pergunta viva sob outro chamado após o nascimento inicial. (`app/chamados/tests/test_c20_fio_mudo.py:273`, `app/chamados/tests/test_c20_fio_mudo.py:425`, `app/chamados/tests/test_c20_fio_mudo.py:433`.)
   - **R08i/R19c:** idempotência verifica retorno/quantidade, sem foto integral; o teste de futuro roda o vigia hoje e olha perguntas amanhã, sem tentar abertura futura. (`app/chamados/tests/test_c20_fio_mudo.py:305`, `app/chamados/tests/test_c20_fio_mudo.py:441`.)

6. **Cinco atos e estoque**

   | Ato | Cobertura final pelo evento |
   |---|---|
   | **Reabrir** | **Coberto pelo novo ponto de chamada**, depois da ressurreição; sujeito às falhas e concorrência acima. (`app/chamados/models.py:1501`, `app/chamados/models.py:1502`) |
   | **Retratar duplicata** | **Sem restauração de cobertura na origem que perdeu a pergunta.** Marca identidade e retorna. (`app/chamados/services/fio.py:878`, `app/chamados/services/fio.py:891`) |
   | **Validação** | **Sem evento C20 de recomposição.** C6 mata pela validação; o fluxo existente materializa/recalcula, sem chamar adoção. (`app/chamados/juizes.py:1135`, `app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:430`) |
   | **Morte por leitura** | **Juiz existente nomeado: C6, `pergunta_viva`/`_pergunta_viva_calc`.** Não há evento novo de recomposição nessa leitura. (`app/chamados/juizes.py:822`, `app/chamados/juizes.py:839`, `app/chamados/juizes.py:1135`) |
   | **Reapontar** | **Sem recomposição da âncora anterior.** A porta salva destino/lavra e retorna. (`app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:849`) |

   **O evento não percorre automaticamente o estoque já vivo:** o dedup retorna antes da adoção. Uma nova abertura pode encontrar perguntas antigas; uma nova reabertura pode adotar órfãs, mas o número efetivamente alcançado dos 494 é **[nao verificado]**. (`app/chamados/models.py:965`, `app/chamados/models.py:983`, `app/chamados/models.py:479`, `app/chamados/models.py:1502`.)

   O contrato declara **494**; o cabeçalho dos testes relata outro retrato de **495**, incluindo 3 excluídos e 34 perguntas com destino morto/sem destino. Esses números não demonstram 34 curas nem reconciliam o universo contratual; conciliação quantitativa: **[nao verificado]**. (`app/docs/PROMPTS.md:1875`, `app/chamados/tests/test_c20_fio_mudo.py:13`.)

**Falta para certificar:** cobrir os quatro atos restantes e demonstrar alcance do estoque; esclarecer M3; fechar a janela de sobrescrita concorrente; provar rollback após escrita parcial e o caso de disputa fechada/mista em competência entregue. Esses pontos não estão provados pelos atuais pontos de chamada e testes. (`app/docs/PROMPTS.md:1882`, `app/chamados/models.py:479`, `app/chamados/models.py:1033`, `app/chamados/models.py:1502`, `app/chamados/tests/test_c20_fio_mudo.py:362`, `app/chamados/tests/test_c20_fio_mudo.py:374`; M3: **[nao verificado]**.)

## 06/11 01:14 · sessao 6def20f82445 · - · - · origem=Ronald (tmux)

- **Adoção levantando dentro de `abrir()`: corrigido quanto ao savepoint.** `abrir()` abre transação e trava o Colaborador antes do dedup (`app/chamados/models.py:961-965`); cria o chamado antes de adotar (`app/chamados/models.py:1030-1033`). A adoção entra em `engolir` **antes** de entrar em `atomic()`, portanto a saída do `atomic` desfaz o savepoint antes de a exceção ser capturada; retorna `[]` depois do ERROR (`app/chamados/models.py:467-468`, `app/chamados/models.py:525-526`; captura e contador em `app/core/observ.py:73-91`). Para erro de banco recuperável com rollback bem sucedido, essa forma preserva a transação externa; funcionamento observado nesta cópia não foi executado [nao verificado].

- **`atomic(savepoint=False)` interno da porta não elimina a proteção nova.** `reapontar_resolvedora` ainda usa `atomic(savepoint=False)` e salva destino/lavra (`app/chamados/services/fio.py:837-848`), mas agora está dentro do savepoint da adoção (`app/chamados/models.py:468`, `app/chamados/models.py:503`). Erro que propague desse núcleo chega ao savepoint e é capturado somente depois de sua saída (`app/chamados/models.py:467-468`). A emissão pode prosseguir sem a costura; não significa fio garantido (`app/chamados/models.py:526`, `app/chamados/models.py:1033-1041`).

- **`reabrir()` direto tem outra unidade transacional.** Não há transação envolvendo o método inteiro: chama `_gravar_estado`, depois `ressuscitar_perguntas`, depois adota (`app/chamados/models.py:1492-1503`). `_gravar_estado` tem seu próprio atomic (`app/chamados/models.py:274-295`); portanto, sem transação externa do chamador, a falha capturada da adoção não desfaz a reabertura já gravada (`app/chamados/models.py:468`, `app/chamados/models.py:1492-1503`). A proteção nova também não cobre falha anterior na ressurreição: ela descarimba perguntas e chama `reabrir_sistema` diretamente (`app/chamados/models.py:1604`, `app/chamados/models.py:1631-1632`).

- **Callback levantando: corrigido quanto à propagação.** O wrapper registra captura própria e chama o serviço dentro dela (`app/chamados/models.py:521-524`); o serviço usa atomic próprio (`app/chamados/services/fio.py:912-922`), portanto uma exceção propagada do serviço passa pela saída do atomic antes do `engolir`. O callback falho não precisa cortar callbacks seguintes; R20a injeta no endereço efetivamente importado, observa ERROR, costura anterior e callback posterior (`app/chamados/tests/test_c20_fio_mudo.py:337-350`). Commit real, recuperação de conexão e rollback fatal não foram exercitados [nao verificado].

- **Ordem explícita da adoção:** `Colaborador → chamado adotante → UPDATE Pergunta`, quando chamada por `abrir`; `chamado adotante → UPDATE Pergunta`, quando chamada diretamente sem outra transação/trava (`app/chamados/models.py:964`, `app/chamados/models.py:469`, `app/chamados/models.py:503`; escrita em `app/chamados/services/fio.py:845-848`). A adoção não executa `select_for_update` das candidatas nem das âncoras anteriores: coleta perguntas e status em consultas comuns (`app/chamados/models.py:479-493`). A trava na porta da costura é da linha **do destino novo**, sem reler a pergunta sob trava (`app/chamados/services/fio.py:758-765`, `app/chamados/services/fio.py:829-848`).

- **Lacuna concorrente concreta por inspeção:** entre a coleta e a escrita, outro escritor pode reapontar a mesma pergunta para um chamado vivo; a adoção continua usando `_anc` e `_grupo` antigos, e `reapontar_resolvedora` escreve por cima sem conferir o destino corrente da pergunta (`app/chamados/models.py:485-503`; `app/chamados/services/fio.py:829-848`). A trava do Colaborador serializa quem passa por `abrir()`, mas a porta de reapontamento não toma essa trava (`app/chamados/models.py:964`; `app/chamados/services/fio.py:836-848`). Execução concorrente e ocorrência no dado de produção: [nao verificado].

- **Ordem explícita do callback:** trava Pergunta e Disputa na consulta, ordenada por disputa/pk; só depois julga C6 e reabre (`app/chamados/services/fio.py:913-920`). `reabrir_sistema` limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`, salva e insere nota no chamado **pai da disputa** (`app/chamados/models.py:2187-2194`). Não chama `ChamadoColaborador.reabrir`; os receivers de fechamento da disputa saem quando `fechada_em` é nulo (`app/chamados/signals.py:28`, `app/chamados/signals.py:106`, `app/chamados/signals.py:130`).

- **Não certificaria “sem qualquer trava de Chamado” só pela docstring.** A nota grava `FollowupChamado` com FK no chamado pai (`app/chamados/models.py:2192-2194`; `app/chamados/services/nota.py:87-103`). Um ciclo envolvendo a trava implícita dessa FK e `Chamado → Pergunta` depende das constraints efetivas, adiamento e modos de trava; deadlock desse cenário não foi demonstrado [nao verificado]. A cadeia proposta `reabrir_sistema → chamado.reabrir → Colaborador` não existe nesse código (`app/chamados/models.py:2177-2195`).

- **Callback não garante destino ainda vivo.** Sua consulta exige o mesmo `chamado_destino_id` e disputa fechada; o predicado seguinte é C6, sem filtro do status do chamado (`app/chamados/services/fio.py:915-920`). Assim, o que ele protege explicitamente é a pergunta ainda apontar para aquele pk e continuar viva, não o chamado ainda estar vivo (`app/chamados/services/fio.py:915-920`). A disputa fechada pode ser observada entre commit e callback e continua assim se ele falhar; o próprio R20a exige essa sobra no contador (`app/chamados/tests/test_c20_fio_mudo.py:345-350`).

- **Limite do R20b:** o erro SQL é real, mas injetado em `canonica_do_grupo`, antes de qualquer costura/retratação; a asserção “destino voltou” também passa se nenhuma escrita chegou a ocorrer (`app/chamados/tests/test_c20_fio_mudo.py:362-372`; eleição antes da escrita em `app/chamados/models.py:500-516`). Ele morde transação envenenada e falha de emissão, mas não comprova rollback de adoção parcialmente escrita (`app/chamados/tests/test_c20_fio_mudo.py:368-372`).

## 06/11 01:14 · sessao 541a416d38e7 · - · - · origem=Ronald (tmux)

Mapa de escritores/leitores e pontos relevantes, por inspeção estática:

| Campo/ato | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.chamado_destino` | `fio.reapontar_resolvedora`, com E4-c, guarda e lavra no mesmo save (`app/chamados/services/fio.py:829-848`). **Há outro sítio preexistente:** `PerguntaDisputa.abrir` reaponta a existente diretamente, com os mesmos juízes e lavra, sem chamar aquela porta (`app/chamados/models.py:2538-2574`). Nascimento grava a FK em `models.py:2576-2577`. | Âncora/listagem: `fio.chamado_que_lista` (`fio.py:97-117`); resolução de disputa aberta (`fio.py:131-137`); lista do app (`app/api/views.py:1007-1010`); validação (`app/chamados/services/validacao.py:793-795`). |
| `PerguntaDisputa.via_resolucao` | Núcleo do carimbo `_escrever_carimbo` (`models.py:2656-2660`), usado pelas portas `carimbar`, `carimbar_identidade`, `descarimbar` (`models.py:2662`, `2725`, `2746`); limpeza em lote (`models.py:2763-2764`). Exceções/atos próprios: `encerrar_por_fato_no_espelho` (`models.py:2773-2774`) e `relavra_via.relavrar` (`app/chamados/services/relavra_via.py:123-126`). | C6 lê validação, tranca, resposta e depois vias de identidade (`app/chamados/juizes.py:1135-1172`); adoção exclui duplicata previamente carimbada (`models.py:481`); retratação testa idempotência (`fio.py:882-883`). |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `DisputaSupervisao.fechar` escreve os quatro juntos (`models.py:1961-1965`); `reabrir_admin` limpa quatro e incrementa seu contador (`models.py:2163-2175`); `reabrir_sistema` limpa quatro sem esse contador (`models.py:2187-2191`). Porta estreita `limpar_resolucao_residual` apaga só resolução de disputa aberta (`models.py:2270-2274`). | `aberta` lê `fechada_em` (`models.py:1864-1865`); fechamento lê estado/autor/resolução/motivo para idempotência (`models.py:1920-1924`); resolução de disputa do fio exige aberta (`fio.py:131-137`); API indexa abertas (`app/api/views.py:1028-1031`); contador lê `motivo_fechamento` (`app/chamados/services/vigia_fio.py:734`). |
| `ChamadoColaborador.status_local` | Transições passam por `_gravar_estado`, atribuição em `models.py:222`, save em `models.py:279`; nascimento calcula estado por motor e insere (`models.py:994-1030`); outra porta de nascimento insere em `models.py:1155`. | Adoção verifica status travado do próprio chamado (`models.py:466-469`) e status das outras âncoras (`models.py:486-492`); guarda da costura lê destino fresco (`fio.py:758-765`); fila e painel filtram estado (`app/chamados/services/fila.py:126`, `app/chamados/services/painel.py:211-222`). |
| Evento `pergunta_reatada` | Gate de fechamento já emitia (`models.py:423-427`); adoção emite (`models.py:508-512`); escrita física de auditoria passa por `core.observ.evento` (`app/core/observ.py:94-138`). | Auditoria genérica consulta e filtra `LogAuditoria` por ação/modelo (`app/core/views.py:705-714`). Consumidor específico dessa ação não localizado; uso adicional [nao verificado]. |
| Ocorrência `disputa_reaberta_sistema` | `reabrir_sistema` escreve **no chamado-pai da disputa**, quando existe (`models.py:2192-2194`), pela porta `nota.anexar_ocorrencia`; inserção física em `app/chamados/services/nota.py:102-109`. | Fio prefetch de followups (`fio.py:171-176`) e histórico (`app/chamados/services/historico.py:80-85`). |

**Segundo escritor relevante:** a reivindicação de exclusividade de `reapontar_resolvedora` (`fio.py:787-796`) não corresponde ao ramo de identidade de `PerguntaDisputa.abrir` (`models.py:2538-2574`). Não é introduzido pelo delta; o selo por arquivo admite `models.py` e `fio.py`, portanto não impede essa duplicação de sítio (`app/core/tests/test_contract_escritores_estado.py:61`).

**Leitores podem observar meio-estado entre commit e callback, ou indefinidamente se o callback falhar:** pergunta reatada a chamado vivo, C6 viva, disputa ainda fechada. O callback é agendado depois da costura (`models.py:508-526`); `_disputa_do_chamado` exclui a disputa fechada (`fio.py:131-137`). O fio do admin admite perguntas vivas em disputas fechadas, enquanto o fio do colaborador usa abertas (`fio.py:209-221`). Porém **não afirmar inacessibilidade universal do colaborador**: `perguntas_do_app` não filtra disputa fechada, só universo/C6 (`app/chamados/services/pergunta_no_app.py:42-53`). A sobra é contada por `viva_em_disputa_fechada`; sua documentação declara dono ADMIN e “NAO alarma” (`app/chamados/services/vigia_fio.py:737-759`).

**Cinco atos:** contrato literal em `app/docs/PROMPTS.md:1875`; enumeração e exigência de nomear o juiz da morte por leitura em `app/docs/PROMPTS.md:1882`; linha da obra segue `ABERTA` (`app/docs/BACKLOG.md:1764`).

- `reabrir`: agora chama adoção depois de ressuscitar suas próprias perguntas (`models.py:1492-1502`).
- `retratar_duplicata`: grava identidade, não chama adoção nem restaura cobertura do chamado que perdeu pergunta (`fio.py:878-895`). Duplicata respondida pode permanecer viva no C6 porque resposta é julgada antes da identidade (`juizes.py:1150-1172`); a adoção deliberadamente a exclui (`models.py:481`).
- `validacao`: carimba e materializa, em transação própria; não chama adoção (`app/chamados/services/validacao.py:395-397`). O carimbo de validação torna C6 falso no primeiro degrau (`juizes.py:1135-1137`).
- `morte por leitura`: autoridade existente é C6, `pergunta_viva` → `_pergunta_viva_calc` (`juizes.py:835-848`), com validação/tranca/identidade/marco/célula/janela (`juizes.py:1135-1208`). A leitura não chama adoção nesse fluxo (`juizes.py:835-848`).
- `reapontar`: porta grava destino/lavra, não chama adoção para a âncora anterior (`fio.py:837-849`).

O evento novo roda apenas no nascimento real (`models.py:1031-1033`) e reabertura (`models.py:1501-1502`); existente vivo no dedup retorna antes da adoção (`models.py:965-983`). Logo, não há varredura/cura automática do estoque já vivo por essa mudança; quantidade efetivamente alcançada em produção [nao verificado].

## 06/11 01:14 · sessao 51d4d44fe872 · H12H06-B · verificador · origem=Code

1. **Alta — a confirmação pode confirmar o ato de outro. Cura (1) incompleta.** A transação devolve seu carimbo, mas o laço o substitui por uma leitura **depois de liberar a trava** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`).

   Contraexemplo: apply grava atestado `720→0`; antes dessa leitura, outra edição salva descrição e reafirma `minutos=0`, preservando identidade. Esse escritor faz `save()` completo, movendo o carimbo (`app/ponto/services/ausencia.py:496`, `app/ponto/services/ausencia.py:501`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:814`). A confirmação captura o carimbo dessa edição; a reversão aceita identidade, minutos e carimbo e tenta devolver `720`, desfazendo a reafirmação alheia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:141`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:163`).

2. **Alta — há novos atos que não movem a testemunha. Cura (1) incompleta também sem corrida.** `mudar_estado` salva status e minutos sem incluir `atualizado_em`; o modelo não acrescenta esse campo ao `update_fields` (`app/ponto/services/ausencia.py:807`, `app/ponto/services/ausencia.py:818`, `app/ponto/models.py:908`).

   Contraexemplo: após apply de atestado aprovado `720→0`, rejeitar e aprovar novamente são transições permitidas. Ao terminar, identidade, minutos e carimbo podem ser iguais aos confirmados; a reversão atravessa as três verificações apesar das novas decisões (`app/ponto/services/ausencia.py:754`, `app/ponto/services/ausencia.py:759`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:141`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:148`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`).

   Nos dois contraexemplos, restaurar minutos positivos depende do cadastro aceitar esse tipo por minutos; os testes contemplam restauração do atestado e recusa da falta. Cadastro efetivo e ocorrência em produção: **[nao verificado]** (`app/ponto/services/ausencia.py:480`, `app/ponto/tests/test_h12h06b_rederivar_minutos.py:286`).

3. **Média — commit pode ficar sem confirmação e sem linha no resultado. Cura (4) parcial.** Depois do commit, a consulta ou a escrita/`flush`/`fsync` da confirmação podem falhar; os contadores e `saida` só são atualizados depois dessas operações (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:238`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:256`).

   Nesse intervalo, a ausência já mudou, mas o resultado parcial pode omiti-la e a reversão excluir sua foto sem confirmação. `completo=False` informa interrupção, porém não identifica essa escrita efetivada (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:225`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:257`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:270`). Falha efetiva em produção: **[nao verificado]**.

Os demais contraexemplos pedidos:

- **PK repetido — fragilidade condicional, gravidade média.** A última confirmação por PK vence, e todas as fotos desse PK recebem seu carimbo; falta vínculo entre tentativa e confirmação. Uma foto anterior malsucedida pode ser habilitada pela confirmação posterior (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:219`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:221`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:225`). A LISTA desta cópia não repete PK; arquivo concatenado ou com tentativas distintas repetidas: **[nao verificado]** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50`).
- **Reversão de reversão — sem defeito isolado demonstrado.** O leitor aceita esse snapshot; a foto troca novamente origem e destino, permitindo `720→0→720→0`, sujeita às mesmas guardas (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:221`).
- **`on_commit` regravando a própria ausência — [nao verificado].** O callback rastreado chama devolução de perguntas, reconciliação e fechamento de chamados. A confirmação posterior lê somente o carimbo, sem repetir a conferência do estado (`app/ponto/signals.py:254`, `app/ponto/signals.py:269`, `app/ponto/signals.py:271`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`).
- **Fuso — sem falha normal demonstrada.** Ambos os lados usam `isoformat()` de leituras do banco, com `USE_TZ=True` na cópia; configuração da conexão de produção: **[nao verificado]** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:255`, `app/config/settings/base.py:95`).
- **Cura (5) — resolve colisão apenas por igualdade do segundo; não garante unicidade.** Mesmo microssegundo ou repetição do relógio ainda pode colidir, mas `open('x')` interrompe antes do laço, sem sobrescrever o snapshot (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:209`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:234`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:247`).

**Quem escreve `Ausencia.atualizado_em`:**

| Caminho | Referência |
|---|---|
| `auto_now` em criação, edição completa e rederivação do fim | `app/ponto/models.py:814`; `app/ponto/services/ausencia.py:418`; `app/ponto/services/ausencia.py:508`; `app/ponto/services/ausencia.py:1280` |
| Declaração/edição de período parcial inclui expressamente o campo | `app/ponto/services/ausencia.py:878`; `app/ponto/services/ausencia.py:911` |
| Rejeição em lote de férias escreve `timezone.now()` explicitamente; tipo fora desta LISTA | `app/ponto/services/ausencia.py:1265`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50` |

**Quem altera a ausência sem mover esse carimbo:** mudança de estado, anexo, prazo, decisão sem período e campos da reabertura (`app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:843`, `app/ponto/services/ausencia.py:536`, `app/ponto/services/ausencia.py:896`, `app/ponto/services/ausencia.py:1114`, `app/ponto/services/ausencia.py:1124`, `app/ponto/services/ausencia.py:1132`).

**Quem lê:** snapshot, comparação antes/depois, guarda da reversão, retorno da transação e confirmação posterior (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:123`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:95`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253`). Há outro leitor funcional: `cartorio.houve_fato_novo`, além da exibição no admin (`app/ponto/services/cartorio.py:355`, `app/ponto/admin.py:47`).

**Sobre os descartes:**

- **(2), exportação:** permanece risco potencial **alto**, fora do escopo conforme o aval informado. A cerca antecede a escrita, enquanto o exportador calcula e só depois registra a entrega; não há serialização entre esses atos nos trechos examinados (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `app/folha/views.py:208`, `app/folha/views.py:297`). Ocorrência: **[nao verificado]**.
- **(3), push:** contesto tratá-lo como tecnicamente resolvido por sair `pulada`. O `save()` dispara cartório síncrono, que pode enviar push antes das verificações que provocam rollback; o resultado não recolhe esse efeito externo. **Gravidade média**, mantendo o aceite residual informado (`app/ponto/services/ausencia.py:508`, `app/ponto/signals.py:147`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:778`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`). Emissão concreta nessas 30 linhas: **[nao verificado]**.

## 06/11 01:14 · sessao 7617c8ab0520 · - · - · origem=Ronald (tmux)

Auditoria por leitura; execução, GREEN/RED e sobrevivência de mutantes: [nao verificado].

- **R20a morde a captura do callback.** O mock está no endereço importado pelo método; a captura executa os callbacks enquanto o patch permanece ativo. Exige ERROR, pergunta adotada, callback seguinte executado e sobra contada. Sem `engolir`, o `RuntimeError` escapa. Limite: usa `TestCase` e `captureOnCommitCallbacks`; não demonstra commit real nem liberação efetiva de travas. `app/chamados/tests/test_c20_fio_mudo.py:32,337-350`; `app/chamados/models.py:518-524`.
- **R20b morde erro SQL e transação utilizável**, porque injeta consulta de tabela inexistente e consulta novamente depois. Porém quebra em `canonica_do_grupo`, antes de `reapontar_resolvedora`: destino inalterado não prova rollback de alteração parcialmente realizada; a alteração sequer começou. Falta caso que escreve primeiro e quebra depois, incluindo descarte de callbacks registrados no savepoint. `app/chamados/tests/test_c20_fio_mudo.py:362-372`; `app/chamados/models.py:500-516`.
- **R20c morde a antiga cerca.** Cria exportação vigente para a competência calculada pela janela e exige adoção; esse registro satisfaz o juiz `competencia_entregue`. Compara todos os campos do fechamento e somente hash/conteúdo da exportação. Não executa callbacks, não fotografa célula e não valida resposta/materialização posterior nem TXT em disco. `app/chamados/tests/test_c20_fio_mudo.py:385-401`; `app/folha/export.py:569-575`.
- **R08j prova adiamento, respeito ao resultado C6 e idempotência; não prova julgamento sob trava.** O teste continuaria passando se a avaliação C6 fosse antecipada antes da trava, pois não há intercalamento concorrente nem inspeção da trava. `app/chamados/tests/test_c20_fio_mudo.py:246-262`.
- **R08g e R19b corrigidos alcançam seus ramos.** R08g exige chamada efetiva da porta com canônica/alvo; R19b cria primeiro a cobrança, depois transfere a pergunta a outro vivo e confirma que a pergunta permanece viva antes de readotar. `app/chamados/tests/test_c20_fio_mudo.py:273-284,425-439`.
- **Outros limites:** R08i verifica retorno vazio e quantidade de perguntas, sem fotografia integral de campos/trilha; R19c só executa vigia hoje e olha ausência de perguntas amanhã, embora seu nome prometa testar também abertura futura. `app/chamados/tests/test_c20_fio_mudo.py:305-318,441-444`.
- **M3 específico:** o anexo não define a mutação e não encontrei definição correspondente nos arquivos da obra examinados; equivalência declarada e morte do mutante: [nao verificado]. O contrato permanece expressamente ABERTO. `app/docs/BACKLOG.md:1764`.

Retirada da cerca:

- A lei diretamente aplicável a chamado/pergunta é **L-050**, que declara ausência de fronteira do exportado; para dinheiro e célula devem ser lidas também **L-113**, que altera L-092/L-052, preservando versão anterior do TXT, trilha e DIFF antes de ato de frota. Não usar texto antigo de L-092 isoladamente como proibição absoluta vigente. `app/docs/LEIS.md:91,93,125,150`.
- A adoção escreve costura/lavra de pergunta, identidade das irmãs, evento e agenda reabertura; nela não aparece escrita de fechamento/exportação/célula. Isso permite afirmar o alcance direto, não garantir efeitos posteriores de resposta/materialização. `app/chamados/models.py:479-524`; `app/chamados/services/fio.py:829-848,878-895`.
- Célula tem pergunta distinta e juiz próprio: `competencia_fechada_para_celula`; a enumeração de competências chama esse juiz, incluindo o fato do holerite publicado. A remoção da cerca da adoção não remove essa guarda. `app/folha/export.py:578`; `app/ponto/management/commands/gerar_celulas.py:146-174`.

Mapa da folha/TXT útil para consolidação:

| Objeto | Escritores | Leitores |
|---|---|---|
| `FechamentoMensal` | Porta `ponto/services/fechamento.py`: cálculo/get-or-create/update, aprovação, reabertura, restauração; registro formal autoriza esse arquivo. `app/ponto/services/fechamento.py:889,1144,1307-1338,1342-1377,1433-1436`; `app/core/portas.py:499` | Montagem TXT consulta os fechamentos gravados e transforma rubricas; aceita objetos derivados fornecidos pelo chamador. `app/folha/export.py:681-713` |
| `ExportacaoDominio` | `registrar` cria histórico com conteúdo/hash; `invalidar` escreve invalidadores e trilha preservando conteúdo/hash. `app/folha/models.py:143-188`; registro da porta: `app/core/portas.py:504` | `competencia_entregue` lê vigente não invalidada; histórico/vigência e download leem cópia armazenada. `app/folha/export.py:569-575`; `app/folha/models.py:121-137,192-200`; `app/folha/views.py:548-562` |
| Célula de competência entregue | Guarda separada de regeneração consulta juiz de fechamento para célula. `app/ponto/management/commands/gerar_celulas.py:146-174` | C6 lê célula/ata e tranca para determinar vida; exportação, por si só, não é esse predicado de vida. `app/chamados/juizes.py:919-954,1135-1208` |

Escopo/estoque:

- O contrato inclui os cinco atos, enquanto a medição estática registrada já dizia que abertura só cobre nascimento e deixa perdas posteriores. O corte nomeia `reabrir`, `retratar_duplicata`, validação, morte por leitura e reapontar. `app/docs/PROMPTS.md:1875,1882`; `app/docs/RELATO.md:31`.
- C20 instala adoção somente no nascimento efetivo e em `reabrir`; cobrança já viva sem nova abertura não recebe esse evento. `app/chamados/models.py:1031-1033,1502`.
- O cron antigo continua examinando chamados vivos sem pergunta viva e recusando fatos já existentes; ele não foi convertido integralmente em vigia pelo delta mostrado. `app/chamados/management/commands/disparar_perguntas_competencia.py:141-175,211-217`.
- As quantidades do cabeçalho do teste são alegações históricas, não nova prova de produção. Alcance quantitativo atual nos 494 e equivalência com a amostra de 495: [nao verificado]. `app/chamados/tests/test_c20_fio_mudo.py:15-18`.

## 06/11 01:13 · sessao 794cf64b33f9 · - · - · origem=Ronald (tmux)

- **Alta — confirmação pode pertencer a outro ato.** `tratar_linha` devolve o carimbo protegido pela transação em `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, mas `handle` o substitui por uma consulta posterior, sem trava, em `:253`. Entre ambos, `editar_ausencia_registro` pode preservar identidade/minutos e salvar outra descrição (`app/ponto/services/ausencia.py:496`, `:501`, `:507`, `:508`), gerando outro carimbo (`app/ponto/models.py:814`). O snapshot confirma esse segundo ato (`…rederivar_minutos_h06_comp11.py:254`); a reversão aceita identidade, minutos e carimbo (`:141`, `:148`, `:151`) e restaura os minutos anteriores sobre ele (`:163`). Cura (1) parcial; ocorrência em produção [nao verificado].

- **Média — linha efetivada pode ficar sem confirmação e desaparecer do resultado parcial.** A transação termina ao retornar de `tratar_linha` (`…rederivar_minutos_h06_comp11.py:136`, `:182`); a consulta e a escrita da confirmação vêm depois (`:253`, `:254`), incluindo `write`, `flush` e `fsync` (`:238`, `:239`, `:240`). Se qualquer operação dessa janela falhar, a linha já foi efetivada, mas ainda não entrou em `conta`/`saida` (`:256`, `:257`). O `finally` grava somente `saida` (`:270`), e a reversão exclui fotos sem confirmação (`:225`). Cura (4) cobre as linhas anteriores, não esta janela; falha efetiva em produção [nao verificado].

- **PK repetido — fragilidade condicional.** A confirmação é indexada somente por PK e a última vence (`…rederivar_minutos_h06_comp11.py:219`); todas as fotos desse PK recebem essa confirmação (`:221`, `:225`). Um arquivo duplicado/concatenado pode vincular confirmação à foto errada; existência desse arquivo [nao verificado]. A LISTA normal não repete PK (`:50` a `:80`).

- **Reversão de reversão — sem defeito isolado demonstrado.** O leitor aceita o snapshot de qualquer ato (`…rederivar_minutos_h06_comp11.py:212`, `:225`); para atestado, a foto da primeira reversão inverte novamente os minutos (`:120`, `:123`, `:163`). Proibição desse uso [nao verificado].

- **Fuso — sem contraexemplo normal demonstrado.** Comparação usa `isoformat()` de leituras do banco em ambos os lados (`…rederivar_minutos_h06_comp11.py:151`, `:253`, `:255`); a configuração da cópia habilita `USE_TZ` (`app/config/settings/base.py:95`). Fuso da conexão de produção [nao verificado].

- **`on_commit` — regravação da própria ausência não demonstrada.** O callback de Ausencia chama devolução de perguntas, reconciliação e fechamento de chamados (`app/ponto/signals.py:254`, `:262`, `:269`, `:271`, `:272`). Uma regravação posterior seria absorvida pela consulta do carimbo sem nova conferência de identidade/minutos (`…rederivar_minutos_h06_comp11.py:253`); caminho normal concreto que faça isso [nao verificado].

## 06/11 01:13 · sessao d1db771d57b5 · - · - · origem=Ronald (tmux)

**Achado — alta: o carimbo não acompanha todos os atos.** `mudar_estado` grava status, minutos e outros campos sem incluir `atualizado_em` (`app/ponto/services/ausencia.py:807-819`); `Ausencia.save` repassa `update_fields` ao Django, sem acrescentar o campo (`app/ponto/models.py:868-908`). Rejeitar e reaprová-la pode devolver a ausência à mesma identidade sem mover a testemunha: ambas as transições existem (`app/ponto/services/ausencia.py:754-759`). A reversão compara apenas identidade, minutos e carimbo (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:141-153`). Para concretizar com restauração positiva, usar o atestado admitido pelo cadastro/testes; falta com alvo positivo enfrenta a validação de tipo parcial (`app/ponto/services/ausencia.py:480-484`).

**Escritores que movem o carimbo:**

- Campo `auto_now`, persistido por `save()` completo: `app/ponto/models.py:814`; criação em `app/ponto/services/ausencia.py:418`, edição em `:508`, rederivação do fim em `:1280`.
- Declaração do período parcial inclui expressamente `atualizado_em`: `app/ponto/services/ausencia.py:875-878`, persistência em `:908-911`.
- Rejeição em lote de férias escreve `timezone.now()` explicitamente: `app/ponto/services/ausencia.py:1265-1267`.
- A tela de edição chama o escritor completo: `app/ponto/views.py:2794-2803`; o comando desta obra também: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:163-164`.

**Atos que não movem o carimbo:**

- Mudança de estado: `app/ponto/services/ausencia.py:807-819`.
- Prazo recalculado após rejulgamento: `app/ponto/services/ausencia.py:536`.
- Anexo: `app/ponto/services/ausencia.py:843`.
- Decisor, data da aprovação e descrição, quando não há declaração do período parcial: `app/ponto/services/ausencia.py:896-911`.
- Reabertura com documento, descrição ou prazo: `app/ponto/services/ausencia.py:1114`, `:1124`, `:1132`.

**Leitores:**

- Snapshot: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:123`.
- Estado completo antes/depois, incluindo o carimbo: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:94-95`, chamadas em `:160`, `:168`.
- Guarda da reversão: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151-153`.
- Retorno da transação: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`.
- Confirmações carregadas do snapshot: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:219`.
- Confirmação pós-transação: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253-255`.
- **Outro leitor funcional:** `houve_fato_novo` pergunta se uma ausência mudou no intervalo usando também `atualizado_em`; portanto não é campo consumido somente pela reversão (`app/ponto/services/cartorio.py:330-357`).
- Exibição no admin somente leitura: `app/ponto/admin.py:37-47`.

## 06/11 01:13 · sessao d90502a41793 · - · - · origem=Ronald (tmux)

- **Corrida com exportação permanece, sem novidade que reabra o escopo excluído.** A cerca consulta a entrega antes do escritor (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `:164`); o juiz lê `ExportacaoDominio` (`app/folha/export.py:580`). O exportador calcula (`app/folha/views.py:207`), monta (`:268`) e registra a entrega depois (`:297`; `app/folha/models.py:178`). A trava de `Ausencia` do comando (`rederivar_minutos_h06_comp11.py:137`) não serializa esse registro. Gravidade potencial **alta**; ocorrência em produção [nao verificado].

- **Push anterior ao rollback continua tecnicamente possível.** O escritor executa `save()` (`app/ponto/services/ausencia.py:508`), cujo receiver chama o cartório sincronamente (`app/ponto/signals.py:143`, `:93`). O cartório pode chamar o emissor (`app/ponto/services/cartorio.py:1740`, `:1778`), que envia push diretamente (`app/ponto/management/commands/detectar_ausencias.py:778`). As conferências que podem desfazer a linha vêm depois (`rederivar_minutos_h06_comp11.py:169`, `:180`). O descarte é aceite residual; o delta não cura o efeito externo. Gravidade **média**; condições concretas para emissão nas 30 linhas [nao verificado].

- **Não encontrei regravação da mesma `Ausencia` pelo `on_commit` rastreado.** O callback chama devolução de perguntas, reconciliação e fechamento de chamados (`app/ponto/signals.py:254`). A devolução só atua para rejeitada (`app/chamados/services/materializacao.py:638`); a cascata de fechamento chama `materializar_pendentes_aprovadas` (`app/chamados/signals.py:139`), que carimba perguntas (`app/chamados/services/materializacao.py:1343`). É diferente de `materializar_perguntas_validadas_da_disputa`, que pode produzir ausência (`:1274`, `:1300`). Um caminho que regrave a mesma ausência nesse callback [nao verificado].

- **Exceções usuais dos serviços do callback direto são engolidas.** Os três serviços estão em blocos `engolir` (`app/ponto/signals.py:260`, `:263`). A janela concreta de exceção pós-commit continua na consulta e escrita da confirmação do comando (`rederivar_minutos_h06_comp11.py:253`, `:254`).

## 06/11 01:02 · sessao 25d780f2d21b · H12H06-B · verificador · origem=Code

**#3 está parcialmente curado; contesto o descarte de #6.** A foto antecede a escrita, mas sobrevive ao rollback; a transação curta não sincroniza com o exportador. ([rederivar_minutos_h06_comp11.py:154](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154), [export.py:580](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/folha/export.py:580))

1. **Alta — snapshot de tentativa pode desfazer ato alheio.** O diário recebe a foto antes do escritor; divergência posterior ou trilha `None` desfaz o banco, preservando a foto. A reversão consome todas as fotos, sem distinguir tentativa de confirmação e sem conferir o `atualizado_em` fotografado. Contraexemplo: atestado `720→0` sofre rollback por falta de trilha; outro ato posteriormente grava `0`; reverter o snapshot da tentativa encontra identidade e minutos esperados e restaura `720`, desfazendo esse outro ato. O RED da trilha verifica o rollback imediato, sem exercitar essa reversão posterior. ([rederivar_minutos_h06_comp11.py:123](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:123), [rederivar_minutos_h06_comp11.py:154](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154), [rederivar_minutos_h06_comp11.py:175](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175), [rederivar_minutos_h06_comp11.py:211](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:211), [test_h12h06b_rederivar_minutos.py:163](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_h12h06b_rederivar_minutos.py:163))

2. **Alta — #6 continua sendo uma corrida possível.** O command trava apenas `Ausencia`; a cerca consulta `ExportacaoDominio` sem trava compartilhada. O exportador calcula antes e registra o TXT depois. Portanto cabe a sequência: cerca responde “aberta” → exportador registra TXT calculado com os minutos anteriores → command confirma a correção. A duração curta reduz a janela, sem eliminá-la. Que 11/12/01 ainda não tenham fechado em produção: **[nao verificado]**. ([rederivar_minutos_h06_comp11.py:134](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:134), [export.py:580](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/folha/export.py:580), [views.py:208](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/folha/views.py:208), [views.py:297](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/folha/views.py:297))

3. **Alta — #5 fecha o dry; o apply ainda pode emitir efeito externo de linha desfeita.** O dry retorna antes do escritor. No apply, `save` dispara o cartório sincronicamente, que pode abrir chamado e enviar push; as conferências capazes de desfazer a linha vêm depois. Isso também limita a cura #4: a ausência volta no banco, mas o push já enviado permanece. Envio efetivo nessa frota: **[nao verificado]**. ([rederivar_minutos_h06_comp11.py:152](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152), [ausencia.py:508](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/ausencia.py:508), [signals.py:147](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/signals.py:147), [cartorio.py:1740](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/cartorio.py:1740), [detectar_ausencias.py:778](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/detectar_ausencias.py:778), [push_utils.py:40](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/chamados/push_utils.py:40), [rederivar_minutos_h06_comp11.py:164](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:164))

4. **Média — exceção no laço deixa execução parcial sem relatório final garantido.** Só `_Desfaz` é capturada. Uma exceção inesperada interrompe o laço antes do `close`, da impressão do caminho/hash e do resultado. Linhas anteriores podem estar confirmadas, pois cada linha tem seu `atomic`. As fotos anteriores receberam `flush`/`fsync`: o achado é a falta de encerramento e prestação de contas garantidos, sem afirmar perda dessas fotos. ([rederivar_minutos_h06_comp11.py:134](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:134), [rederivar_minutos_h06_comp11.py:178](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:178), [rederivar_minutos_h06_comp11.py:227](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:227), [rederivar_minutos_h06_comp11.py:234](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:234), [rederivar_minutos_h06_comp11.py:243](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:243))

5. **Média operacional — reexecução no mesmo segundo pode abortar antes da idempotência.** O nome usa segundos e a abertura exclusiva acontece antes das conferências das linhas. Duas execuções do mesmo ato no mesmo segundo colidem com `FileExistsError`, inclusive duas reversões consecutivas. O arquivo existente fica protegido; o comando deixa de chegar ao “zero mudanças”. O teste de segunda execução desloca explicitamente a hora. ([rederivar_minutos_h06_comp11.py:203](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:203), [rederivar_minutos_h06_comp11.py:222](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:222), [test_h12h06b_rederivar_minutos.py:137](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_h12h06b_rederivar_minutos.py:137))

Quanto às demais curas e aos contraexemplos pedidos:

| Item | Conclusão pela cópia |
|---|---|
| **#1 — reversão da falta** | Continua recusada por `tipo_nao_parcial`; o teste mantém a reversibilidade como `expectedFailure`. A declaração de PARADA não cura a reversão. ([ausencia.py:480](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/ausencia.py:480), [test_h12h06b_rederivar_minutos.py:279](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_h12h06b_rederivar_minutos.py:279)) |
| **#2 — recalcular exportada** | Permanece aberto: os leitores alterados abrangem o período consultado, além da LISTA; recalcular exportada produz aviso e segue. Medida de impacto na frota: **[nao verificado]**. ([efeito_ausencia.py:47](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/efeito_ausencia.py:47), [credito_parcial.py:26](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/credito_parcial.py:26), [fechamento.py:530](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/fechamento.py:530)) |
| **#4 — trilha `None`** | Curado para a persistência no banco: `None` provoca saída excepcional do `atomic`. ([observ.py:133](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/core/observ.py:133), [rederivar_minutos_h06_comp11.py:175](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175)) |
| **Reversão da reversão** | A composição dos valores funciona para atestado confirmado: a foto registra encontrado/alvo, e a leitura inverte esperado/alvo. Continua sujeita aos achados de tentativa desfeita e colisão do nome. ([rederivar_minutos_h06_comp11.py:120](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:120), [rederivar_minutos_h06_comp11.py:211](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:211)) |
| **PK fora da LISTA** | Descartada antes de construir a linha de reversão. ([rederivar_minutos_h06_comp11.py:208](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:208), [rederivar_minutos_h06_comp11.py:213](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:213)) |
| **#7 — palavra** | Curado no ramo alterado: abono com minutos consulta o juiz existente; desconto preserva o número. A ajuda do formulário ainda diz genericamente `>0 = parcial`, contrariando os casos de 720/range. ([dia_decidido.py:494](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/dia_decidido.py:494), [turnos.py:2598](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/turnos.py:2598), [lancar_ausencia.html:70](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/templates/ponto/lancar_ausencia.html:70)) |

**Quem escreve e quem lê:**

| Objeto | Escritor / leitores |
|---|---|
| **Arquivo de snapshot** | O command abre e escreve pelo `diario`; o mesmo command lê em `--reverter`. Os testes também leem o JSONL para conferir fotos. ([rederivar_minutos_h06_comp11.py:206](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:206), [rederivar_minutos_h06_comp11.py:222](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/management/commands/rederivar_minutos_h06_comp11.py:222), [test_h12h06b_rederivar_minutos.py:103](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_h12h06b_rederivar_minutos.py:103)) |
| **`rotulo_de`** | Monta a palavra; os consumidores diretos de produção são `do_dia` e `cartao_do_dia`. ([dia_decidido.py:484](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/dia_decidido.py:484), [dia_decidido.py:429](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/dia_decidido.py:429), [dia_decidido.py:519](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/dia_decidido.py:519)) |
| **`do_dia`** | Chamado pelo espelho e calendário, recebendo a cobertura carregada por essas rotas. ([espelho.py:340](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/espelho.py:340), [calendario.py:715](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/colaboradores/services/calendario.py:715)) |

O objeto **`_Falsa` continua sem datas** e chega a `rotulo_de`/`cartao_do_dia`; seus casos atuais de abono têm minutos zero, enquanto os positivos são desconto/rejeição, evitando a nova consulta. **`_Aus` ganhou datas** e exercita cobertura parcial em `do_dia`. Não demonstram um chamador atual quebrado; um objeto abonado com minutos, de tipo parcial, sem datas falharia no acesso direto do juiz. ([test_rotulo_do_dia_decidido.py:33](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_rotulo_do_dia_decidido.py:33), [test_rotulo_do_dia_decidido.py:47](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_rotulo_do_dia_decidido.py:47), [test_palavra_parcial_e_pendente.py:40](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_palavra_parcial_e_pendente.py:40), [test_palavra_parcial_e_pendente.py:92](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_palavra_parcial_e_pendente.py:92), [turnos.py:2599](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/turnos.py:2599))

A palavra alterada alcança **espelho, calendário, PDF e API do app**, além de histórico e CSV. ([espelho.py:344](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/espelho.py:344), [_calendario_grade.html:97](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/templates/colaboradores/partials/_calendario_grade.html:97), [pdf_espelho.py:923](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/relatorios/pdf_espelho.py:923), [views.py:1550](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/api/views.py:1550), [painel_ausencia.html:184](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/templates/ponto/partials/painel_ausencia.html:184), [atestados_lote.py:128](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/services/atestados_lote.py:128))

Há selos de igualdade **PDF=tela, grade=tela e API=espelho**. Eles podem acompanhar uma palavra igualmente errada nos dois lados; os novos testes diretos de 720/range/270 são os que cobram a semântica dessa cura. ([test_palavra_do_dia.py:213](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/relatorios/tests/test_palavra_do_dia.py:213), [test_palavra_do_dia.py:234](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/relatorios/tests/test_palavra_do_dia.py:234), [test_o21_app_diz_o_que_foi_decidido.py:170](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:170), [test_h12h06b_parcial_pelo_juiz.py:156](/home/ronald/codex_ws/7bffb71f55f2ddfb2b442cdafd3db538809a9f62/app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:156))

Execução do GREEN de 367 testes, mordida dos mutantes M7–M10 e ocorrência desses caminhos em produção: **[nao verificado]**. [nao verificado]

## 06/11 01:00 · sessao 6672b0f6106e · - · - · origem=Ronald (tmux)

- **Alta — contesto descarte #6: transação curta não elimina corrida com exportação.** O command trava apenas `Ausencia` e consulta a cerca antes do escritor (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:134-159`). A cerca faz leitura comum de `ExportacaoDominio`, sem trava compartilhada (`app/chamados/services/vigia_fio.py:838-843`; `app/folha/export.py:580-586`). A exportação calcula primeiro e registra o TXT depois (`app/folha/views.py:207-212`, `:268`, `:297-300`; `app/folha/models.py:178-187`). Contraexemplo: cerca responde aberta → exportador registra TXT calculado antes da correção → command salva e confirma a ausência. Que 11/12/01 estejam abertas em produção é **[nao verificado]** pelo dado fornecido.

- **Alta, se “PAREI” exige interrupção do lote — #1 continua sem parada no código.** Reverter falta para 720 é recusado pela porta quando o tipo não aceita parcial (`app/ponto/services/ausencia.py:480-484`). A recusa vira resultado `recusada` (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:160-161`, `:178-179`), mas o laço continua e chega ao resumo sem erro (`:234-250`). Há atestados depois das faltas na LISTA (`:67-80`). A obrigação de o operador parar fora do command é **[nao verificado]** no código.

- **Alta — #5 fecha o dry; apply continua podendo emitir push de ato desfeito.** O dry retorna antes do escritor (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152-159`). No apply, `editar_ausencia_registro` chama `save` (`app/ponto/services/ausencia.py:507-508`), cujo `post_save` julga sincronicamente (`app/ponto/signals.py:143-147`, `:92-93`). O cartório pode emitir chamado (`app/ponto/services/cartorio.py:1725-1742`, `:1777-1780`), e o emissor chama push diretamente (`app/ponto/management/commands/detectar_ausencias.py:774-783`; `app/chamados/push_utils.py:110-111`, `:40-48`). Só depois o command confere campos, minutos e trilha, podendo desfazer a linha (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:162-176`). Push efetivamente enviado nessa frota: **[nao verificado]**.

- **Alta — #2 permanece global, além da LISTA/competência do one-shot.** Os leitores corrigidos percorrem quaisquer ausências do período (`app/ponto/services/efeito_ausencia.py:47-60`; `app/ponto/services/credito_parcial.py:26-28`; `app/ponto/services/fechamento.py:674-690`). Recalcular exportada só gera aviso, sem recusa (`app/ponto/services/fechamento.py:530-538`), e eventos chamam esse recalculador (`:1793-1794`). Gerar TXT também calcula em leitura e usa seus valores (`app/folha/views.py:207-212`, `:268`). Assim, a nova semântica alcança nova exportação mesmo sem executar o command; impacto quantitativo na frota exportada: **[nao verificado]**.

- **#4 curado para a escrita da trilha no banco.** `evento` usa savepoint próprio, devolve o registro criado ou `None` (`app/core/observ.py:133-139`); o command transforma `None` em exceção que sai do `atomic` antes de ser capturada (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:134`, `:170-179`). Isso não resolve o efeito externo de push descrito acima, que antecede essa conferência (`app/ponto/services/ausencia.py:508`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:170-176`).

- **Ordem de trava: não confirmei ciclo inverso.** Existe aquisição posterior de trava em `Colaborador` se o cartório entrar na emissão: command trava `Ausencia` (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:135-136`), emissor chama `ChamadoColaborador.abrir` (`app/ponto/management/commands/detectar_ausencias.py:693-713`), que trava `Colaborador` (`app/chamados/models.py:863-866`). Deadlock demonstrável com caminho inverso: **[nao verificado]**.

## 06/11 00:59 · sessao a58f67d401d6 · - · - · origem=Ronald (tmux)

Não encontrei contraexemplo ativo para a cura de `rotulo_de`: o ramo novo só consulta cobertura quando há `ABONADO` com minutos; preserva desconto/rejeição e remove horas do abono integral segundo o juiz existente. (`app/ponto/services/dia_decidido.py:492`, `app/ponto/turnos.py:2598`)

Mapa de leitura:

- `rotulo_de` é chamado por `do_dia`, com `cobertura`, e por `cartao_do_dia`, com a ausência. (`app/ponto/services/dia_decidido.py:429`, `app/ponto/services/dia_decidido.py:519`)
- `do_dia` recebe cobertura em `espelho.aplicar_palavra_do_dia` e no calendário. (`app/ponto/services/espelho.py:340`, `app/colaboradores/services/calendario.py:715`)
- O espelho transporta `_cobertura_folha`; o calendário carrega cobertura pela autoridade `cobertura_ausencia_periodo`. (`app/ponto/services/espelho.py:872`, `app/colaboradores/services/calendario.py:533`)
- `cartao_do_dia` também alimenta decisões por dia e histórico completo. (`app/ponto/services/dia_decidido.py:561`, `app/ponto/services/dia_decidido.py:599`)

Objetos sem `data`/`data_fim`:

- `_Falsa` continua sem datas e chama `rotulo_de`/`cartao_do_dia`. Seus casos abonados usam minutos zero; os casos com minutos positivos são desconto ou rejeição, portanto não atingem a leitura nova de datas. Não constitui regressão executável nesses casos. (`app/ponto/tests/test_rotulo_do_dia_decidido.py:33`, `app/ponto/tests/test_rotulo_do_dia_decidido.py:47`, `app/ponto/tests/test_rotulo_do_dia_decidido.py:61`, `app/ponto/tests/test_rotulo_do_dia_decidido.py:73`, `app/ponto/tests/test_rotulo_do_dia_decidido.py:92`)
- `_Aus`, usado como cobertura parcial em `do_dia`, ganhou as datas no delta. (`app/ponto/tests/test_palavra_parcial_e_pendente.py:40`, `app/ponto/tests/test_palavra_parcial_e_pendente.py:92`)
- Um futuro stand-in abonado, parcial e com minutos positivos precisará de datas: o juiz acessa diretamente `a.data_fim`/`a.data`. Não encontrei esse chamador quebrado na cópia. (`app/ponto/services/dia_decidido.py:494`, `app/ponto/turnos.py:2599`)

Universo atingido:

- Calendário: palavra montada por `do_dia`, exposta no título do chip e no texto da célula. (`app/colaboradores/services/calendario.py:715`, `app/templates/colaboradores/partials/_calendario_grade.html:97`, `app/templates/colaboradores/partials/_calendario_grade.html:98`)
- Espelho: palavra atribuída ao dicionário do dia. (`app/ponto/services/espelho.py:344`)
- PDF do espelho: imprime `palavra_dia` na observação. (`app/relatorios/pdf_espelho.py:923`)
- API do app: copia a palavra do espelho e a devolve nas linhas. (`app/api/views.py:1402`, `app/api/views.py:1550`, `app/api/views.py:1610`)
- Histórico do drawer: recebe `cartao_do_dia` e imprime `h.rotulo`. (`app/ponto/services/dia_decidido.py:599`, `app/templates/ponto/partials/painel_ausencia.html:184`)
- Relatório em lote e CSV: reaproveitam `linha_do_tempo`; CSV inclui `rotulo`. (`app/ponto/services/atestados_lote.py:57`, `app/ponto/services/atestados_lote.py:128`)

Selos que copiam a palavra:

- Igualdade PDF/tela, grade/tela e API/espelho: conferem a mesma palavra nos consumidores. (`app/relatorios/tests/test_palavra_do_dia.py:213`, `app/relatorios/tests/test_palavra_do_dia.py:234`, `app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:170`)
- Os novos testes exercitam diretamente `rotulo_de` nos casos 720, range com minutos e parcial verdadeira; os selos de igualdade acima usam outros casos, portanto não demonstram especificamente o percurso integral desses três novos exemplos. (`app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:157`, `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:163`, `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:168`, `app/relatorios/tests/test_palavra_do_dia.py:212`, `app/relatorios/tests/test_palavra_do_dia.py:233`)

Nenhum achado bloqueador novo de rótulo identificado por inspeção; execução dos testes e comportamento efetivo em produção: [nao verificado]. [nao verificado]

_2073 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
