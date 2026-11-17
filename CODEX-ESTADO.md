# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 14:13** · publicado em **17/11/2026 14:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 14:13 · sessao 643e42dfd211 · CONFRONTO-PILOTO · sonda · origem=Code

```text
SAIDA_BEGIN
sonda_leitura: sonda_mapa.py [91337151eae2675b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TOTAL_REGISTROS 2063 ESPELHO 2030
SEM C001 prefixo16_casa=0
SEM C002 prefixo16_casa=0
SEM C003 prefixo16_casa=0
SEM C004 prefixo16_casa=0
SEM C005 prefixo16_casa=0
SEM C006 prefixo16_casa=0
SEM C007 prefixo16_casa=0
SEM C008 prefixo16_casa=0
SEM C009 prefixo16_casa=0
SEM C010 prefixo16_casa=0
SEM C011 prefixo16_casa=0
SEM C012 prefixo16_casa=0
SEM C013 prefixo16_casa=0
SEM C014 prefixo16_casa=0
SEM C015 prefixo16_casa=0
SEM C016 prefixo16_casa=0
SEM C017 prefixo16_casa=0
SEM C018 prefixo16_casa=0
SEM C019 prefixo16_casa=0
SEM C020 prefixo16_casa=0
ACHOU 0 de 20
ESPELHOS_ULT3D 1 COLABS_DISTINTOS 1
SAIDA_END
```

Saída reproduzida do dado fornecido; nova execução em produção: [nao verificado].
Segundo esse dado, casaram **0/20**, inclusive por prefixo de 16 caracteres; confirmação independente: [nao verificado].
O espelho chama o escritor compartilhado em `app/relatorios/pdf_espelho.py:1103`; ele calcula SHA-256 do PDF da primeira passagem, sem hash no rodapé, e persiste esse valor (`app/relatorios/pdf_base.py:139`, `app/relatorios/pdf_base.py:149`, `app/relatorios/pdf_base.py:152`).
O hash gravado corresponde à primeira passagem; o arquivo final entregue é reconstruído com o hash visível na segunda passagem (`app/relatorios/pdf_base.py:170`, `app/relatorios/pdf_base.py:178`, `app/relatorios/pdf_base.py:180`).
A origem e o significado dos valores da coluna Hash do auditor, e a causa das divergências, permanecem [nao verificado].

## 17/11 14:12 · sessao 2130aa0d2665 · - · - · origem=Ronald (tmux)

Censo das entradas e origens, somente pela cópia.

| Entrada | Origem e cadeia comprovada | Cerca antes do resolver |
|---|---|---|
| `app/ponto/management/commands/reconciliar_fantasmas.py:288` | Comando com `--apply` e sem `--alarme` (`:153`, `:269`). Primeiro tenta `retratar_com_motivo` (`:281`); pula fallback exclusivamente para `RECUSA_COMPETENCIA_ENTREGUE` (`:284`). Cron configurado usa `--alarme` (`app/config/crons.py:392`). | A tentativa consulta a cerca; o fallback pode ser chamado quando `rec=False` por outro motivo (`:281`, `:284`, `:288`). |
| `app/chamados/services/becos.py:55` | Signal de pergunta `app/chamados/signals.py:236` → `on_commit` (`:247`) → `fechar_beco_da_disputa` (`:246`) → `fechar_beco` (`app/chamados/services/becos.py:79`). Também comando `fechar_becos_disputa --apply` (`app/chamados/management/commands/fechar_becos_disputa.py:30`, `:57`); cron usa `--alarme` (`app/config/crons.py:663`). | Não consulta `_retratacao_toca_entregue`: apenas trava e chama resolver (`app/chamados/services/becos.py:52`, `:55`). |
| `app/chamados/signals.py:124` | S143: `post_save` de `ChamadoColaborador` (`:98`), status em ENCERRADOS (`:104`) e gravação do status reconhecida (`:112`). | Não consulta cerca; só entra em contexto de recusas e atomic (`:123`, `:124`). |
| `app/ponto/services/geofence.py:444` | Signal de Posto (`app/ponto/signals.py:291`) → `on_commit` (`:307`) → reavaliação aplicadora (`:306`). Também comando `reconciliar_geofence --apply` (`app/ponto/management/commands/reconciliar_geofence.py:26`, `:34`); cron usa `--alarme` (`app/config/crons.py:486`). | Passa por `retratar` (`app/ponto/services/geofence.py:444`) → `_retratar` (`app/chamados/reconciliador.py:603`) → `retratar_com_motivo` (`:543`) → cerca (`:576`). |
| `app/chamados/services/disputa_emissao.py:1862` | Nesta cópia é condição `_credor_vivo(d.chamado)` dentro do seletor `_abertas_da_disputa_sem_credor` (`:1848`, `:1859`). | Seletor puro, sem escrita; o resolver o chama em `:2173`. |
| `app/chamados/services/disputa_emissao.py:1891` | Nesta cópia constrói `resta` dentro de `_disputa_fecha_no_ato` (`:1881`), depois consulta `pode_fechar` (`:1892`). | Seletor puro, usado para enumerar cascata pela cerca (`:1911`). |

O resolvedor público somente trava e chama o corpo; não contém veto K6 (`app/chamados/services/disputa_emissao.py:2012`, `:2026`, `:2027`, `:2028`). Sua exceção `VIA_COMPETENCIA_TRANCADA` retorna zero no corpo (`:2048`), diferente de competência entregue, cujo juiz é consultado em `app/chamados/models.py:364`.

**Origens adicionais de S143 e da relavra S2.** `transicionar_com_pauta` chama o callback em `app/chamados/services/fecho_runtime.py:160`/`:165`, ou `chamado.transicionar` em `:175`/`:181`. A transição humana chega a `_aplicar` (`app/chamados/models.py:199`) e `_gravar_estado` (`:450`); reconciliar chega a `_gravar_estado` em `:655`; encerrar em `:1693`. O save acontece em `:296`, acordando S143; a relavra S2 pode acontecer depois, independentemente do resolver, em `:308`/`:312`.

Sites runtime encontrados de `transicionar_com_pauta(... ato=*.reconciliar)`, além de retratação/S88:

- `app/colaboradores/services/pos_vinculo.py:43`.
- `app/colaboradores/management/commands/marcar_feriado_do_posto.py:141`.
- `app/ponto/services/lastro.py:401`.
- `app/ponto/services/cobertura.py:123`, `:319`.
- `app/ponto/services/cartorio.py:1011`, `:1539`, `:1552`, `:1573`, `:1652`, `:1667`.
- `app/ponto/services/entrada_sincronizada.py:32`.
- `app/ponto/services/cluster_espurio.py:190`.
- `app/ponto/services/ausencia.py:841`.
- `app/ponto/services/veredito_celula.py:203`.
- `app/ponto/services/triagem_batida.py:598`.
- `app/ponto/management/commands/detectar_ausencias.py:159`, `:826`, `:837`, `:849`.
- `app/ponto/management/commands/curar_residuo_bug115.py:155`.
- `app/ponto/management/commands/detectar_intervalo_ausente.py:139`.
- `app/chamados/services/silencio_ausencia.py:102`.
- `app/chamados/services/materializacao.py:1067`, `:1256`.
- `app/chamados/services/limbo_folgas.py:102`, `:284`, `:287`.
- `app/chamados/services/feriado_reavaliar.py:48`.
- `app/chamados/services/adesao.py:116`, `:151`.
- `app/chamados/management/commands/auditar_chamados_orfaos.py:68`.

Sites usando `ato=*.encerrar`: `app/chamados/reconciliador.py:505`; `app/colaboradores/services/vinculo.py:404`; `app/colaboradores/services/desligamento.py:187`; `app/chamados/services/regularizacao_ext.py:206`.

Sites de transição humana que podem encerrar: `app/colaboradores/views.py:2112`; `app/chamados/services/acoes_chamado.py:71`, `:110`, `:155`, `:229`, `:286`, `:325`; `app/chamados/services/fio.py:769`; wrapper `app/chamados/models.py:455`; comandos `app/chamados/management/commands/smoke_chamados.py:135`, `:148`, `:164`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`.

Não incluir `acoes_chamado.py:241` como S143: pede `em_analise`. Não incluir CANCELAR: `ENCERRADOS=('resolvido','fechado')`, enquanto cancelado está apenas em TERMINAIS (`app/chamados/catalogo/motor.py:30`, `:31`). O gate analisar em `app/chamados/models.py:853` também não chega a S143. Reabrir em `:1489` pode atingir S2 pela mudança de fronteira (`:273`), mas S143 recusa seu status vivo (`app/chamados/signals.py:104`).

**Fechos externos de disputa que chegam à Defesa 1/S143/S144.** Cada `DisputaSupervisao.fechar` grava o fechamento em `app/chamados/models.py:2027`; Defesa 1 tenta reconciliar o pai em `app/chamados/signals.py:50`; S144 chama `materializar_pendentes_aprovadas` em `:172`. Sites encontrados:

- `app/api/foto_recebida.py:84`.
- `app/chamados/reconciliador.py:414`, `:669`, `:1229`.
- `app/chamados/models.py:2041` (`admin_encerra`, sem caller runtime encontrado), `:2163` (`remover_motivo`, chamado por `app/escala/signals.py:174` e `app/colaboradores/signals.py:77`).
- `app/colaboradores/services/desligamento.py:167`.
- `app/ponto/services/entrada_sincronizada.py:60`.
- `app/chamados/services/materializacao.py:1071`, `:2030`.
- `app/chamados/services/disputa_emissao.py:1298`, `:2226`.
- `app/chamados/services/competencia_trancada.py:229`.
- `app/chamados/services/becos.py:58`.
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`.

A guarda de perguntas vivas **não é veto de entrega**: `_vivas_entre` exclui perguntas entregues com `continue` (`app/chamados/models.py:364`, `:365`); `_ancoradas` inclui qualquer competência (`:344`), e a relavra S2 busca todas as não validadas sem filtro K6 (`:309`, `:310`, `:312`).

**Reprodução do censo:** executei `rg` dos nomes `resolver_perguntas_por_chamado_resolvido`, `_resolver_perguntas_por_chamado_resolvido`, `retratar`, `_retratar`, `retratar_com_motivo`, `reavaliar_chamados_geofence`, `fechar_beco`, `fechar_beco_da_disputa`; depois AST de todos `.py` fora de `tests`/`migrations`, coletando imports com `asname`, `Call(Name/Attribute)` e referências `Attribute` usadas como callback. Na expansão S143 acrescentei `transicionar_com_pauta`, `transicionar`, `reconciliar`, `_gravar_estado`, `fechar`, `encerrar`, `cancelar`, `superar`, `encerrar_por_competencia_trancada`, `admin_encerra`. AST é necessária porque `ato=ch.reconciliar` não é `Call`; o próprio código documenta falsos positivos de grep e alias em `app/core/management/commands/gerar_diagrama.py:69`, `:87`, `:88`, e a limitação de `getattr` em `:90`. Fechamento diante de referências dinâmicas arbitrárias: [nao verificado].

## 17/11 14:12 · sessao 4a67d85ceaa0 · RESPOSTAS-1410 · verificador · origem=Code

**Item 2**

1. **Premissas: parcialmente conferem.** A recusa do POST consulta a porta e retorna sem lavrar medição (`app/folha/views.py:124-133`). Porém, o comando só aceita `--mes` e `--ano`; não há `--tentativa` nessa interface (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`). O registro em crons declara execução sob demanda, sem agendamento (`app/config/crons.py:887-897`). Hoje se sobrescreve um carimbo por empresa/dia; exceção em `medir` termina antes de lavrá-lo (`app/folha/porta_export.py:772-796`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`).
2. **Lei:** escritor único concorda com L-007 (`app/docs/LEIS.md:65`). A existência do cron pressuposto por R9 não está demonstrada pelo registro fornecido, que declara justamente não agendar (`app/config/crons.py:887-897`). Selagem pelos commits: **[nao verificado]**.
3. **Literal incompleto:** “toda execução” precisa definir tentativa por invocação ou por empresa: o comando percorre empresas e aborta na primeira exceção, podendo já ter lavrado empresas anteriores (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`). O carimbo existente tampouco preserva tentativas separadas ou competências distintas no mesmo dia (`app/folha/porta_export.py:772-776`).
4. **Dinheiro/!：** a escrita examinada é de `MetricaSnapshot`; não altera o gravado da folha (`app/folha/porta_export.py:768-796`). Alterar `FechamentoMensal` exportado continuaria exigindo `!` e a porta própria (`CLAUDE.md:411-417`).

**ITEM 2: PROCEDE — AVAL-DEVOLVIDO: identificar o cron de R9 e definir a unidade da tentativa; a interface e o registro atuais não correspondem às premissas descritas.** (`app/config/crons.py:887-897`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`, `86-95`)

**Item 3**

1. **Premissas conferem como possibilidade de código.** `data_evento` admite NULL; C6 pode manter viva uma pergunta sem dia e sem materialização (`app/chamados/models.py:2528-2530`; `app/chamados/juizes.py:1470-1483`). O cálculo de C6 não condiciona a vida ao estado aberto/fechado da disputa (`app/chamados/juizes.py:1403-1495`). Estoque real nesse estado: **[nao verificado]**.
2. **Não demonstrei colisão do “esperado 0”.** O nascimento exige dia e já existe contador esperado zero para linhas que violam esse contrato (`app/chamados/models.py:2892-2895`; `app/chamados/services/fio.py:1199-1210`). Isso não demonstra quantidade zero em produção; número medido exige a fonte real (`CLAUDE.md:35-36`).
3. **O literal fecha usando** pergunta de fato (`conversa=False`), `data_evento is None` e vida respondida por C6; conversa tem outra semântica de data (`app/chamados/models.py:2532-2540`; `app/chamados/juizes.py:338-342`). O contador deve declarar fonte, unidade, universo e exclusões (`CLAUDE.md:192`).
4. **Dinheiro/!：** contar essas perguntas não autoriza datá-las, ressuscitá-las ou corrigir gravado exportado; esse limite permanece literal (`CLAUDE.md:265-268`). A retenção existente por célula é uma decisão própria do retentor (`app/folha/export.py:1270-1296`).

**ITEM 3: NAO PROCEDE — não há bloqueante demonstrado; “esperado 0” é compatível com o contrato existente, sem afirmar estoque zerado.** (`app/chamados/models.py:2892-2895`; `app/chamados/services/fio.py:1199-1210`)

**Item 4**

1. **A distinção de donos existe**, com ações de colaborador, admin e sistema (`app/chamados/services/tarefa_do_dia.py:157-186`). Porém, `nao_sei` não é um estado exclusivo: absorção pendente conserva ação administrativa e marca desconhecimento simultaneamente (`app/chamados/services/tarefa_do_dia.py:186`, `334-341`).
2. **4(d), isoladamente, preserva a retenção atual**, inclusive `sem_celula` independentemente de ação humana (`app/folha/export.py:1270-1296`). Mas preservar retenções não preserva necessariamente as **isenções**: L-033 manda manter aberta a pergunta de intervalo sem bloquear a folha, e o retentor aplica essa exceção (`app/docs/LEIS.md:81`; `app/folha/export.py:1287-1294`). O novo ramo precisa respeitá-la explicitamente.
3. **Há interseção sem precedência definida:** ação humana + `nao_sei=True` satisfaz 4(a) e 4(c) com resultados opostos. A porta publicada confirma que desconhecimento pode coexistir com porta humana (`app/chamados/services/tarefa_do_dia.py:669-670`, `719-722`). Também é preciso distinguir ações das instâncias da ação efetivamente alcançável: a tarefa filtra alcance antes de publicar o dono (`app/chamados/services/tarefa_do_dia.py:637-669`).
4. **Dinheiro/!：** retenção participa diretamente da decisão de entrada no TXT (`app/folha/export.py:968-973`). Isso não autoriza modificar gravado exportado; esse ato exige `!` (`CLAUDE.md:411-417`).

**ITEM 4: PROCEDE — AVAL-DEVOLVIDO: resolver ação humana junto de `nao_sei` e conservar a isenção L-033 no novo ramo.** (`app/chamados/services/tarefa_do_dia.py:334-341`; `app/docs/LEIS.md:81`; `app/folha/export.py:1287-1294`)

**Item 5**

1. **Premissa confere.** K6 está registrado como `folha/export.py::competencia_entregue`; responde pela exportação vigente não invalidada, sem consultar aprovação ou tranca (`app/core/juizes.py:343`; `app/folha/export.py:580-594`). Invalidar a última exportação não ressuscita automaticamente a anterior (`app/folha/models.py:217-225`; `app/folha/export.py:591`).
2. **Não demonstrei colisão.** TXT é fotografia, não pagamento (`CLAUDE.md:418-422`). O tratamento de competência paga fora está registrado como construção condicionada a cadastro pela tela e medida antes de valer em produção (`app/docs/CORTES.md:496`). Implantação efetiva: **[nao verificado]**.
3. **Os ramos fecham como delimitação do universo.** Trancada sem TXT passa pelo critério K6, mas isso não obriga retenção: C6 pode calar a pergunta e a tarefa pode retornar sem ações pela tranca (`app/folha/export.py:588-594`; `app/chamados/juizes.py:1407-1410`; `app/chamados/services/tarefa_do_dia.py:221-229`).
4. **Dinheiro/!：** K6 consulta exportações; não reescreve dinheiro (`app/folha/export.py:580-594`). “Sem TXT” não dispensa as condições próprias de eventual apply em competência não exportada (`CLAUDE.md:406-410`).

**ITEM 5: NAO PROCEDE — K6 responde ao critério declarado, inclusive TXT invalidado sem substituto; tranca continua sendo julgada pelas autoridades existentes.** (`app/folha/export.py:564-594`; `app/chamados/juizes.py:1407-1410`)

**Item 7**

1. **Premissas não integralmente verificadas.** O hook recebe JSON, mas só consome `transcript_path`; o fornecimento real de `stop_hook_active` é **[nao verificado]** (`bin/hook_stop_fila1.py:1339-1347`). O formato sem PID/arquivo é recusado atualmente (`bin/hook_stop_fila1.py:814-830`). Implementação e comportamento do RETOMA-AUTO: **[nao verificado]**.
2. **Há conflito a esclarecer com a regra vigente:** ESPERANDO libera com produtor vivo provado; aceitar somente um nome não apresenta essa prova (`CLAUDE.md:262-264`; `bin/hook_stop_fila1.py:527-541`). Além disso, a prova do RELATO precede qualquer liberação (`bin/hook_stop_fila1.py:1349-1358`).
3. **O literal não fecha na interseção** `stop_hook_active=true` + ESPERANDO com PID morto: 7(a) manda liberar, enquanto o RED manda continuar recusando. A recusa do PID morto existe e tem caso nominal (`bin/hook_stop_fila1.py:832-836`; `bin/tests/test_hook_esperando_com_prova.sh:95-96`). Falta definir precedência também perante RELATO sem prova (`bin/tests/test_hook_esperando_com_prova.sh:186-189`).
4. **Dinheiro/!：** a alteração proposta alcança a autorização de encerrar o turno; liberar antes da prova contornaria uma guarda vigente, e atalhos permanecem fora da pré-aprovação (`bin/hook_stop_fila1.py:1349-1358`; `CLAUDE.md:265-268`). Efeito sobre exportada ou dinheiro: **[nao verificado]**.

**ITEM 7: PROCEDE — AVAL-DEVOLVIDO: definir precedências, comprovação do subagente e localizar o RETOMA-AUTO antes de certificar 7(c).** (`bin/hook_stop_fila1.py:527-541`, `832-836`, `1349-1358`; RETOMA-AUTO **[nao verificado]**)

## 17/11 14:11 · sessao 562028e7c1f7 · - · - · origem=Ronald (tmux)

Submatriz do resolvedor e efeitos, somente código da cópia:

| Entrada / subramo | Cerca antes da escrita? | Escrita e prova |
|---|---|---|
| `resolver_perguntas_por_chamado_resolvido` | **Não**: wrapper só trata via trancada e trava; não consulta `_retratacao_toca_entregue` nem `veto_de_competencia` (`app/chamados/services/disputa_emissao.py:2021`, `:2027`, `:2028`). | Encaminha todos os ramos abaixo (`app/chamados/services/disputa_emissao.py:2031`). |
| S128 direto | **Não** no resolvedor; C6/tranca/resposta são outras guardas (`app/chamados/services/disputa_emissao.py:2103`, `:2109`, `:2113`, `:2123`). | `PerguntaDisputa.materializada_em/materializada_por/via_resolucao`, eventualmente `respondida_em/validada_em/validada_por`, e S2 `veredito_lavrado/julgado_em/janela_expira_em`: chamada `:2129`; atribuições em `app/chamados/models.py:3220`, `:3253`, `:3256`; lavra `:3271`; save `:3273`. |
| HX sem credor | **Não**: seleção por credores/vitalidade, sem competência (`app/chamados/services/disputa_emissao.py:2173`, `:2182`). | Carimbo e eventual `respondida_em`, sem S2 explícita: `app/chamados/services/disputa_emissao.py:2194` → `app/chamados/models.py:3250`, `:3253`, `:3273`. Também evento `fecho_sem_credor` (`app/chamados/services/disputa_emissao.py:2197`). |
| Reabrir credor de pergunta viva, antes de carimbar | **Sim, parcial**: guarda local de competência na carga, mas ignora dia `None` (`app/chamados/services/disputa_emissao.py:1976`, `:1982`, `:1983`, `:1992`, `:1998`). Não é a cerca `_retratacao_toca_entregue`. | `credor.reabrir` (`:1998`) pode passar pela transição/S2/ressuscitação ou migração abaixo (`app/chamados/models.py:1462`, `:1489`, `:1498`). |
| HX resposta visível | **Não**: chama `_pai.reabrir()` diretamente, sem helper acima (`app/chamados/services/disputa_emissao.py:2146`, `:2150`, `:2152`, `:2154`). | Transição/S2/ressuscitação ou migração abaixo; depois nota (`app/chamados/services/disputa_emissao.py:2156`). |
| Reabrir normal | **Não** em `reabrir`: decide recusas e chama escritor (`app/chamados/models.py:1437`, `:1489`). | `ChamadoColaborador.status_local/resolvido_em/via_encerramento/atualizado_em`, urgência conforme estado (`app/chamados/models.py:228`, `:229`, `:248`, `:251`, save `:296`); antes há evento `estado_chamado` (`:292`). |
| S2 pela transição | **Não**: ocorre após save do chamado, sem veto (`app/chamados/models.py:296`, `:305`, `:308`, `:312`). | `PerguntaDisputa.veredito_lavrado/julgado_em/janela_expira_em` (`app/chamados/veredito_lavrado.py:270`, `:272`, `:273`, save `:275`). |
| Ressuscitar após reabrir | **Não** dentro de `ressuscitar_perguntas` (`app/chamados/models.py:1601`, `:1615`, `:1639`). | Primeiro registra decisões antigas (`:1621`); limpa carimbo, validação e resposta inteira (`:1639` → `:3402`–`:3404`); acorda células (`:1642`); reabre disputa (`:1649`); registra auditoria (`:1663`). |
| Reabrir redirecionado ao irmão | **Não**: ramo `irmao_vivo` chama migração (`app/chamados/models.py:1458`, `:1462`). O plano de retratação avalia mérito/episódios, sem competência (`app/chamados/services/fio.py:1330`–`:1349`). | Migra destino e S2 (`app/chamados/services/fio.py:2246` → `:973`–`:976`); pode retratar perguntas conflitantes do novo destino (`:2225`, `:2252` → `:1392` → `:1410` → `app/chamados/models.py:3311`). |
| Fecho final do resolvedor | **Não**: C7/tranca, sem competência entregue (`app/chamados/services/disputa_emissao.py:2215`, `:2225`, `:2226`). | `DisputaSupervisao.fechada_em/fechada_por/resolucao/motivo_fechamento` (`app/chamados/models.py:2023`–`:2027`); dispara Defesa 1 e S144 (`app/chamados/signals.py:26`, `:156`). |
| Defesa 1 após fecho | **Não**: usa `ch.reconciliar`, sem passar por `retratar_com_motivo` (`app/chamados/signals.py:50`). | Pode reapontar pergunta **antes** do estado (`app/chamados/models.py:689` → `:437`), depois transição/S143/S2 (`:655`, `:296`, `app/chamados/signals.py:124`, `app/chamados/models.py:312`); nota (`app/chamados/signals.py:53`). |
| S144 após fecho | **Não**: testa pergunta em voo e lastro, sem competência (`app/chamados/services/materializacao.py:2161`, `:2163`, `:2167`). | Carimbo pela porta `carimbar`, save `app/chamados/models.py:3273`; disparador em `app/chamados/signals.py:172`. |
| `post_save` da pergunta | Cerca anterior depende do caminho; receiver não aplica veto (`app/chamados/signals.py:260`–`:266`). | Carimbo/resposta/destino acordam célula (`:266` → `app/chamados/models.py:3369` → `app/ponto/portas/celula.py:954`, `EventoCelula.objects.create`); agenda beco em `on_commit` (`app/chamados/signals.py:246`, `:247`). Save só de S2 não acorda outbox (`app/chamados/signals.py:264`, `app/chamados/models.py:3341`). |

**As linhas obrigatórias `disputa_emissao.py:1862` e `:1891` são seleção, não escritores.** A primeira verifica pai vivo dentro de `_abertas_da_disputa_sem_credor`; a segunda calcula `resta` em `_disputa_fecha_no_ato` (`app/chamados/services/disputa_emissao.py:1848`, `:1862`, `:1881`, `:1891`). O resolvedor chama a seleção HX em `:2173`; a cauda real usa `d.pode_fechar` e `d.fechar` diretamente (`:2225`, `:2226`).

**Há também incompletude independente na enumeração da cerca.** `perguntas_alcancadas_pela_retratacao` enumera perguntas dos credores `[chamado] + encerrados_pela_cascata` (`app/chamados/services/disputa_emissao.py:1937`, `:1938`, `:1942`, `:1943`). A cascata só acrescenta pai que `defesa_1_reconcilia` aceite (`:1909`, `:1911`), cujo predicado exclui pai já `ENCERRADO` (`app/chamados/signals.py:23`). Porém HX pode reabrir precisamente pai já encerrado: pula somente pai em `VIVOS` (`app/chamados/services/disputa_emissao.py:2150`) e chama `reabrir` (`:2154`). Esse pai, suas perguntas ressuscitáveis e eventuais conflitantes do irmão não são necessariamente enumerados (`app/chamados/services/disputa_emissao.py:1942`, `:1943`; `app/chamados/services/fio.py:2225`, `:2252`). Existência desses casos em produção: **[nao verificado]**.

**Conclusão de desenho:** colocar cerca somente em `carimbar`, `lavrar` ou no resolvedor chega depois de outras escritas: transição grava antes de S143/S2 (`app/chamados/models.py:292`, `:296`, `:312`), e `reconciliar` pode reapontar antes de `_gravar_estado` (`:689`, `:437`, `:655`). Portanto, essas portas isoladas não constituem ponto único antes da primeira escrita da unidade. A barreira precisa anteceder a aplicação das operações de transição/reatamento, além das entradas autônomas do resolvedor e dos escritores diretos que a matriz principal identificar; chamar isso de lista mínima global sem esse restante seria **[nao verificado]**.

Censo executado em memória com `python3`/AST, sem importar Django: arquivos de `app/**/*.py`, excluindo `tests` e `migrations`; `ast.Call` por `Name`/`Attribute`, resolvendo `ImportFrom.asname` para `lavrar`, e inspeção dos corpos encontrados. Nomes: `resolver_perguntas_por_chamado_resolvido`, `_resolver_perguntas_por_chamado_resolvido`, `reabrir_credor_de_pergunta_viva`, `_perguntas_do_fecho_direto`, `_abertas_da_disputa_sem_credor`, `_disputa_fecha_no_ato`, `perguntas_alcancadas_pela_retratacao`, `carimbar`, `lavrar`, `materializar_pendentes_aprovadas`, `migrar_resolvedoras_vivas`, `reapontar_resolvedora`, `retratar_para_canonica`, `carimbar_identidade`, `descarimbar_em_lote`. Censo de chamadas do resolvedor: `signals.py:124`, `becos.py:55`, `reconciliador.py:593/:1706`, `reconciliar_fantasmas.py:288`; alias reexportado em `models.py:4274`.

## 17/11 14:11 · sessao d10afe6d2b0e · - · - · origem=Ronald (tmux)

A cerca de S128 existe em `app/chamados/reconciliador.py:576`, antes da transição em `:580`, mas E2, HIST e c1 escrevem sem atravessá-la (`app/chamados/reconciliador.py:652`, `:1700`, `:407`).

| Entrada | Chamador | Cerca antes da escrita? | Escrita/prova |
|---|---|---|---|
| `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` | comando `handle`, `--apply` (`:26`, `:54`) | Sim, S128 `app/chamados/reconciliador.py:576` | Transição `:580`, justificativa `:592`, resolver `:593`, ocorrência `:595` |
| `app/ponto/management/commands/processar_alertas_turno.py:216` | comando, `_resolver_chamado_do_turno` (`:153`), chamado por `handle` em `:89` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`app/chamados/reconciliador.py:580`, `:592`, `:593`, `:595`) |
| `app/ponto/management/commands/reconciliar_grade.py:309` | comando sob `apply` (`:305`) | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/management/commands/reconciliar_vinculo.py:68` | comando sob `apply` (`:58`) | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/management/commands/supra_juiz.py:286` | comando, alias `_retr` (`:258`), `--executar` e trava T6 (`:279`) | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/services/geofence.py:444` | comando `app/ponto/management/commands/reconciliar_geofence.py:34`; signal do Posto `app/ponto/signals.py:306` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/services/justificativa.py:46` | `encerrar_chamado_de_origem`, chamado em `app/ponto/views.py:1840` e `app/ponto/services/justificativa.py:217`, `:408` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`); aprovação já escreveu Justificativa antes (`app/ponto/services/justificativa.py:208`, `:386`) |
| `app/ponto/management/commands/reconciliar_fantasmas.py:281` | comando | Sim, S128 `app/chamados/reconciliador.py:576`; recusa de competência impede fallback (`app/ponto/management/commands/reconciliar_fantasmas.py:284`) | Fallback resolver `:288` ainda acontece para outras recusas, inclusive retenção anterior à cerca (`app/chamados/reconciliador.py:564`) |
| `app/chamados/reconciliador.py:1061` | consumidor outbox em `:1426`; comando `app/chamados/management/commands/consumir_eventos_celula.py:31` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/chamados/reconciliador.py:1522` | consumidor outbox `:1430`; reconciliador por colaborador `:1499`, signals `app/ponto/signals.py:215` e `app/chamados/signals.py:377` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/chamados/reconciliador.py:652` | E2: outbox `:1423`; reconciliação por colaborador `:1490`; cartório `app/ponto/services/cartorio.py:1007` | **Não**: filtro verifica C6 e tranca, sem K6 (`app/chamados/reconciliador.py:718`, `:724`) | `PerguntaDisputa.materializada_em/materializada_por/via_resolucao`, save em `app/chamados/models.py:3273`; pode fechar disputa depois (`app/chamados/reconciliador.py:668`) |
| `app/chamados/reconciliador.py:1677` | `reatar_pergunta_orfa`; callers `:1584` e `app/chamados/services/disputa_emissao.py:473` | **Não**: c1 verifica motivo, resposta, materialização, turno e C6 (`app/chamados/reconciliador.py:363`, `:366`, `:368`, `:373`, `:400`) | carimbo em `:407` → save `app/chamados/models.py:3273`; fecho disputa em `app/chamados/reconciliador.py:414` |
| `app/chamados/reconciliador.py:1700` → `:1706` | HIST de `reatar_pergunta_orfa`; callers `:1584` e `app/chamados/services/disputa_emissao.py:473` | **Não**: entregue permanece no caminho histórico (`app/chamados/reconciliador.py:1650`); costura libera entregue em `app/chamados/services/fio.py:824` | Primeiro `PerguntaDisputa.chamado_destino`, eventualmente `data_evento`, `veredito_lavrado/julgado_em/janela_expira_em` em `app/chamados/services/fio.py:975`; só depois resolver em `app/chamados/reconciliador.py:1706` |
| `app/chamados/reconciliador.py:1713` | ramo derivável de `reatar_pergunta_orfa`; mesmos callers `:1584` / `app/chamados/services/disputa_emissao.py:473` | **Não**, costura sem veto geral K6 (`app/chamados/services/fio.py:955`, `:957`, `:974`) | Mesmos campos da costura em `app/chamados/services/fio.py:975` |
| `app/chamados/reconciliador.py:1679` → `:1011` | c2 de `reatar_pergunta_orfa` | **Parcial**: K6 somente para motivos fora `MOTIVOS_C1` (`:867`, `:965`); `orfao_14h/saida_sem_entrada` são `MOTIVOS_C1` (`:333`) | Antes da costura pode julgar célula por `_ctx_c2` (`:977` → `:759`) e abrir/renascer chamado (`:967`); costura grava em `app/chamados/services/fio.py:975` |

O wrapper público `retratar` encaminha a `_retratar` e este a `retratar_com_motivo` (`app/chamados/reconciliador.py:603`, `:543`). A primeira escrita de estado do Chamado fica em `app/chamados/models.py:296`, precedida da trilha em `:292`; pode haver reaponte antes, pois `reconciliar` chama `_decisao_do_fechamento` em `:652`, esta chama `_gate_pergunta_viva` em `:689`, e o gate costura em `:437`, antes de `_gravar_estado` em `:655`. Os candidatos desse gate excluem competência entregue (`app/chamados/models.py:364`).

O cartório também chama diretamente `reconciliar`, sem S128, em `app/ponto/services/cartorio.py:1011`; portanto `_gravar_estado` recebe caminhos que não passaram pela cerca `app/chamados/reconciliador.py:576`.

Para a lista fechada operacional, precisam ser contemplados pelo menos: unidade S128 antes da transição (`app/chamados/reconciliador.py:580`), resolver direto, primeira escrita da costura HIST/derivável (`app/chamados/services/fio.py:975`), carimbos E2/c1 (`app/chamados/reconciliador.py:652`, `:407`), e transições diretas com S143/relavra (`app/chamados/models.py:296`, `:312`). Somente cercar carimbo é tardio para a transição e insuficiente para a costura; somente cercar `_gravar_estado` não alcança resolver direto, E2 ou HIST (`app/chamados/reconciliador.py:1700`, `:1706`, `:652`).

Censo executado em leitura: `rg` dos nomes e imports, seguido de `python3 -B` com `ast.parse` sobre `app/**/*.py`, excluindo `tests/migrations/__pycache__`, coletando `ImportFrom` com alias e `Call(Name|Attribute)`. Nomes: `retratar`, `_retratar`, `retratar_com_motivo`, `retratar_perguntas_absolvidas_por_celula`, `retratar_chamados_absolvidos_por_celula`, `retratar_premissa_morta_colab`, `reconciliar_chamados_colab`, `reatar_pergunta_orfa`, `reconciliar_perguntas_orfas_colab`, `lavrar_sem_turno_aberto`, `reemitir_chamado_do_dia` [nao verificado: comando executado, sem arquivo de saída por regra de leitura]. Homônimos excluídos: `_retratar` de Batida em `app/ponto/management/commands/detectar_par_relampago.py:217` e `retratar_humano` importado em `app/ponto/views.py:3355`.

## 17/11 14:10 · sessao 65a6af02077b · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Item 3(a): não confirmei colisão do “esperado 0”.** `data_evento` permite NULL (`app/chamados/models.py:2528`), mas isso sozinho não prova legitimidade do estado. Já existe contador esperado 0 para dia nulo/divergente, com universo e exclusões declarados (`app/chamados/services/fio.py:1199`, `:1203`, `:1209`). O contador novo precisa declarar universo, unidade e exclusões (`CLAUDE.md:192`); não pode apresentar meta como número medido (`CLAUDE.md:35`). Quantidade real de perguntas vivas sem dia: **[não verificado]**. O acervo anterior ao horizonte é explicitamente congelado (`app/chamados/models.py:2500`, `:2505`; `app/docs/LEIS.md:145`).

- **Item 4(d): preservar retenções existentes é compatível com o retentor atual**, que continua retendo `sem_celula` e vereditos acusadores independentemente de tarefa humana (`app/folha/export.py:1270`, `:1273`, `:1293`, `:1295`). Portanto, “ninguém age” não libera automaticamente retenção antiga.

- **Item 4: colisão adicional com L-033.** A lei mantém pergunta de intervalo aberta **sem bloquear folha** (`app/docs/LEIS.md:81`). O retentor aplica expressamente essa isenção (`app/folha/export.py:1173`, `:1287`, `:1293`); o teste fixa `veredito='furo'`, protesto preservado e retenção vazia (`app/folha/tests/test_furo_so_intervalo_nao_retem.py:169`). Um novo ramo que retém por qualquer pergunta viva com ação humana precisa conservar essa exceção. “Só acrescenta retenção” não resolve essa precedência. **ITEM 4: PROCEDE**, além da coexistência `nao_sei` + ação humana já encontrada pelo principal.

- **Item 5: premissa K6 confere.** Autoridade registrada em `app/core/juizes.py:343`; implementação só consulta a tripla empresa/competência e exige exportação vigente não invalidada (`app/folha/export.py:580`, `:588`, `:591`). Não consulta aprovação nem tranca. O desempate pelo último `(gerado_em, pk)` fica em `app/folha/models.py:217`; invalidar o último não ressuscita o anterior (`:224`; `app/folha/export.py:591`). A competência paga fora está registrada como construção (`app/docs/CORTES.md:232`, `:496`); implantação ou cadastro real: **[não verificado]**. **ITEM 5: NAO PROCEDE** nesta revisão.

- **Limite de autorização dos itens 3–5:** mudar retenção altera quem entra no TXT, cuja decisão é `classificar_export` (`app/folha/export.py:765`, `:768`). Não autoriza tocar gravado exportado: exige `!` (`CLAUDE.md:265`); escopo literal não se amplia por alegar que dinheiro não mudou (`:268`). Apply em não exportada segue a autorização própria de `CLAUDE.md:286`.

## 17/11 14:09 · sessao f1ac47d7d425 · - · - · origem=Ronald (tmux)

Item 7 — achados para consolidação:

1. **Premissas:** o hook recebe JSON de stdin, mas só lê `transcript_path`; a presença real de `stop_hook_active` é **[nao verificado]** (`bin/hook_stop_fila1.py:1339-1347`). O registro local apenas aponta o comando de Stop, sem schema do payload (`.claude/settings.json:3-10`). O formato novo `ESPERANDO: subagente <nome>` atualmente cai em “produtor nao declarado em 1 palavra”; essa é a mudança pedida, não um defeito da implementação atual (`bin/hook_stop_fila1.py:814-830`). A implementação do despertador RETOMA-AUTO é **[nao verificado]**; há registros históricos de sua invocação (`app/docs/PROMPTS.md:2092`, `app/docs/PROMPTS.md:2102`, `app/docs/PROMPTS.md:2135`, `app/docs/PROMPTS.md:2341`).

2. **Lei vigente:** aceitar a declaração sem comprovar subagente vivo exige esclarecer a relação com a regra de produtor vivo provado (`CLAUDE.md:262-264`; `app/docs/LEIS.md:66`). Há também uma precedência vigente que o item precisa preservar ou excepcionar expressamente: afirmação sem prova impede fechar o turno, antes de qualquer liberação (`bin/hook_stop_fila1.py:1349-1358`), inclusive ESPERANDO válido (`bin/tests/test_hook_esperando_com_prova.sh:186-189`).

3. **Literal não fecha:** para a combinação `stop_hook_active=true` + `ESPERANDO` com PID morto, (a) manda liberar e o RED manda continuar recusando; falta precedência. A recusa do PID morto está implementada e possui caso nominal (`bin/hook_stop_fila1.py:621-628`, `bin/hook_stop_fila1.py:832-836`; `bin/tests/test_hook_esperando_com_prova.sh:95-96`). Também falta dizer qual prova distingue `<nome>` vivo de nome inexistente: a implementação atual exige PID/arquivo e não observa agentes diretamente (`bin/hook_stop_fila1.py:527-541`, `bin/hook_stop_fila1.py:829-836`).

4. **Exportada/dinheiro/!:** os ramos examinados operam o encerramento e arquivos de trilha/contador (`bin/hook_stop_fila1.py:43-56`, `bin/hook_stop_fila1.py:1433-1435`, `bin/hook_stop_fila1.py:1453-1459`). Não demonstram escrita de competência/dinheiro. Porém, se a liberação de (a) contornar a prova obrigatória, cai na vedação de atalho; a exceção precisa ser literal (`CLAUDE.md:265-268`; `bin/hook_stop_fila1.py:1349-1358`).

**ITEM 7: PROCEDE — devolver para definir precedência entre liberação por entrada ativa, recusa de PID morto e prova obrigatória, além de localizar o RETOMA-AUTO e definir a prova de vida do subagente (`bin/hook_stop_fila1.py:621-628`, `bin/hook_stop_fila1.py:829-836`, `bin/hook_stop_fila1.py:1349-1358`; RETOMA-AUTO [nao verificado]).**

## 17/11 14:09 · sessao 56328c467ec4 · - · - · origem=Ronald (tmux)

ITEM 2 — AVAL-DEVOLVIDO

1. Premissas: parcialmente. O POST só consulta a porta e devolve recusa (`app/folha/views.py:124-133`); o escritor existente lavra `MetricaSnapshot` por empresa/dia via `update_or_create`, sem tentativa individual (`app/folha/porta_export.py:768-796`). O comando aceita somente `--mes` e `--ano`, portanto a distinção “manual sem `--tentativa`” não existe nesta cópia (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`). A medição está explicitamente declarada como não agendada, contrariando a premissa de cron existente (`app/config/crons.py:887-897`). A exceção de `medir` imprime motivo e termina antes de `lavrar`; não deixa registro FALHOU (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`).

2. Lei vigente: escritor único está alinhado à L-007 e ao contrato estrutural (`app/docs/LEIS.md:65`; `CLAUDE.md:178`). A declaração atual de cron é “não agendar”; o aval precisa reconciliar isso para tornar R9 executável (`app/config/crons.py:887-897`). Não há estados FALHOU/DIVERGENTE demonstrados no escritor atual, cujo valor é número de falhas ou -1 (`app/folha/porta_export.py:772-776`).

3. Literal: “toda execução cria a tentativa” não define se a unidade é invocação ou empresa. O comando percorre empresas, lavra cada uma e aborta na primeira exceção; uma invocação pode ter empresas já lavradas e outras não medidas (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`). O carimbo atual sobrescreve a mesma empresa/dia inclusive quando muda competência, pois competência só está em `defaults` (`app/folha/porta_export.py:772-776`). A unidade do registro e o cron de R9 precisam ser identificados antes da construção.

4. Dinheiro/exportada/!: a escrita examinada é de métrica, sem alteração de `FechamentoMensal` (`app/folha/porta_export.py:768-796`). Tocar gravado exportado exigiria `!` e a porta específica, fora dessa escrita (`CLAUDE.md:411-417`). Impacto real em produção: [não verificado].

ITEM 2: PROCEDE — premissas de cron e `--tentativa` não correspondem à cópia, e “toda execução” deixa a unidade do registro indefinida (`app/config/crons.py:887-897`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`, `:86-95`).

## 17/11 14:00 · sessao 3f14516fb790 · PORTA-EXPORT · verificador · origem=Code

**G ainda tem uma regressão no reinício: a limpeza de `detalhe` feita por `pedir` deixou de valer. Também há lacunas nos testes novos e falhas de `assumir` sem registro persistido da falha original.** (`app/folha/porta_export.py:1105`, `app/folha/porta_export.py:1049`, `app/folha/tests/test_porta_sem_beco_item2.py:459`, `app/folha/tests/test_porta_sem_beco_item2.py:477`, `app/folha/porta_export.py:1209`)

1. **Chamadores, transações e travas**

   Há quatro chamadores diretos em produção e um em teste. O auxiliar abre `atomic`, busca com `select_for_update`, filtra estado/tentativa e deriva o detalhe da linha travada. (`app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1049`)

   | Chamador direto | Contexto |
   |---|---|
   | `_falhar` | Sem `atomic` próprio; usa o do auxiliar e depois registra a recusa. (`app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1076`) |
   | `pedir`, ao refazer | O `atomic` da tentativa de criação já terminou; a retomada usa o `atomic` do auxiliar, sem advisory local. (`app/folha/porta_export.py:1093`, `app/folha/porta_export.py:1098`, `app/folha/porta_export.py:1106`) |
   | `assumir`, com tentativa | Leitura inicial comum; no comando, entra sob a advisory já adquirida. (`app/folha/porta_export.py:1129`, `app/folha/porta_export.py:1131`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`) |
   | `concluir` | Tem `atomic` externo, já travou a mesma linha e já chamou `lavrar`; o auxiliar abre `atomic` aninhado e pede novamente a trava dessa linha. (`app/folha/porta_export.py:1179`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1185`, `app/folha/porta_export.py:1187`) |
   | Teste direto | Ignora o retorno, em classe `TestCase`, sem advisory explícita nesse teste. (`app/folha/tests/test_porta_sem_beco_item2.py:34`, `app/folha/tests/test_porta_sem_beco_item2.py:102`) |

   `_falhar` recebe chamadas de `encerrar` e dos dois ramos de `vigiar`. `encerrar` vem da recuperação do comando, de `falhar_ao_assumir` e da falha de disparo do subprocesso. Os dois primeiros caminhos do comando permanecem dentro da advisory; `disparar` não adquire essa trava. (`app/folha/porta_export.py:1199`, `app/folha/porta_export.py:1229`, `app/folha/porta_export.py:1238`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/folha/porta_export.py:1215`, `app/folha/porta_export.py:1340`)

   `vigiar` também roda pela leitura da porta, pelo censo global anterior à advisory e por `assumir` sem tentativa, este último sob a advisory do comando. O helper de testes chama `assumir`/`concluir` sem advisory local. (`app/folha/porta_export.py:1272`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:73`, `app/folha/porta_export.py:1252`, `app/folha/porta_export.py:1137`, `app/folha/porta_export.py:887`)

   **Não identifiquei inversão dessas travas formando ciclo nos caminhos de produção examinados.** A ordem do comando é advisory → pedido → carimbo; a segunda aquisição do pedido em `concluir` é da linha que a própria transação já travou. O censo global encerra cada `atomic` antes de chegar à advisory; esta usa `pg_try_advisory_lock`, e `medicao_viva` somente consulta `pg_locks`. Ausência de deadlock em execução real: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1185`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:982`, `app/folha/porta_export.py:1002`)

2. **Dependência da cópia do chamador: sim, em `pedir`**

   `pedir` fazia `linha.detalhe = {}` para reiniciar o registro. Agora essa limpeza é ignorada: `_mudar_estado` começa com `atual.detalhe`, vindo do banco. (`app/folha/porta_export.py:1105`, `app/folha/porta_export.py:1049`)

   Os extras da retomada substituem competência, origem, ator e tentativa, mas deixam sobreviver, quando existentes, **`resultado`, `n`, `motivo`, `pid`, `backend_pid`, `em_curso_em`, `concluido_em` e `falhou_em` da tentativa anterior**. Seus escritores são `assumir`, `_falhar`, `concluir` e o timestamp automático da transição. (`app/folha/porta_export.py:1106`, `app/folha/porta_export.py:1132`, `app/folha/porta_export.py:1141`, `app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1052`)

   **`pedido_em` é renovado**, porque `para=PEDIDO` o regrava dinamicamente; não procede concluir timeout imediato por timestamp antigo. A regressão comprovada é o registro da tentativa nova carregar campos da anterior. Liberação indevida do TXT por isso: **[nao verificado]**; o leitor decide por estado, identidade da tentativa e carimbo, e a adoção sobrescreve a sessão antes de entrar em curso. (`app/folha/porta_export.py:1052`, `app/folha/porta_export.py:1106`, `app/folha/porta_export.py:1275`, `app/folha/porta_export.py:1283`, `app/folha/porta_export.py:1131`)

3. **Mudança de retorno: os usos encontrados continuam corretos**

   | Uso | Avaliação |
   |---|---|
   | `_falhar` | Usa o estado anterior na trilha e devolve `bool(antes)`. (`app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1077`) |
   | `pedir` | Converte explicitamente para bool; `abrir_a_porta` testa `if criado`. (`app/folha/porta_export.py:1106`, `app/folha/porta_export.py:1308`) |
   | `assumir` | Usa `not`; sucesso retorna `'pedido'`, string não vazia, e derrota retorna `None`. Preserva seu contrato linha/`None`. (`app/folha/porta_export.py:913`, `app/folha/porta_export.py:1047`, `app/folha/porta_export.py:1055`, `app/folha/porta_export.py:1131`) |
   | `concluir` | Ignora o retorno, mas já protege a mesma linha/estado/tentativa na transação externa. Seu retorno continua dict/`None`. (`app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1192`) |
   | Teste direto | Ignora o retorno. (`app/folha/tests/test_porta_sem_beco_item2.py:102`) |

   `encerrar` e `falhar_ao_assumir` continuam repassando bool; `_fechar` descarta esse retorno. O comando testa `linha is None` e `res is None`, não o estado anterior contra `True`. Não apareceu comparação incompatível nos consumidores encontrados. (`app/folha/porta_export.py:1199`, `app/folha/porta_export.py:1215`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:156`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:172`)

4. **Fechamento, leitores e falhas sem registro**

   `falhar_ao_assumir` delega a `encerrar` → `_falhar` → `_mudar_estado`: grava estado, resultado, motivo, instante da falha e `calculado_em`, condicionado à tentativa. Depois `_recusa` escreve a trilha pelo mecanismo existente. `_fechar` somente executa o callback e captura exceções com saída em stdout; não é outro escritor de estado. (`app/folha/porta_export.py:1215`, `app/folha/porta_export.py:1199`, `app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1066`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:59`)

   Os leitores relevantes são: `vigiar`, para estado/sessão/prazo; `estado_da_porta`, para estado/tentativa/carimbo; `conferir`, para o texto; `_recusa`, para identificação e autoria; e o contador de recusas, que lê a linha crítica. O stdout de `_fechar` não entra nesse contador. (`app/folha/porta_export.py:1225`, `app/folha/porta_export.py:1275`, `app/folha/porta_export.py:820`, `app/folha/porta_export.py:1063`, `app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `bin/recusa_estado.py:68`)

   **Nos dois callsites novos, outro detentor vivo da mesma advisory impede a entrada no ramo de recuperação.** O CAS também impede encerrar uma tentativa sucessora com outro UUID. Essa proteção depende de conservar a sessão que adquiriu a trava; perda/substituição da conexão durante a recuperação: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:139`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/folha/porta_export.py:1042`)

   Há uma ressalva anterior a G, dentro do escopo dos chamadores: `disparar` inclui `Popen` e a saída do contexto do arquivo no mesmo `try`. **Se o fechamento do arquivo levantar `OSError` depois de o filho nascer**, chama `encerrar`, que aceita também `EM_CURSO`, sem consultar vivacidade. Esse caminho pode fechar o estado da mesma tentativa já adotada pelo filho vivo; ocorrência real: **[nao verificado]**. (`app/folha/porta_export.py:1335`, `app/folha/porta_export.py:1337`, `app/folha/porta_export.py:1339`, `app/folha/porta_export.py:1199`)

   **Sim, `assumir` pode levantar sem deixar a falha original no registro do job nem na recusa crítica:** cron/manual não passa tentativa, e `falhar_ao_assumir` retorna `False` imediatamente. Se a própria recuperação levantar, `_fechar` também fica apenas no stdout; uma tentativa aberta poderá receber depois o motivo genérico do vigia. Isso não prova ausência de toda saída: o comando imprime o erro; retenção dessa saída em produção é **[nao verificado]**. (`app/folha/porta_export.py:1125`, `app/folha/porta_export.py:1209`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `app/folha/porta_export.py:1229`, `app/folha/porta_export.py:1238`)

   A mensagem final “cada uma FALHOU com o seu motivo” não é garantida: `_fechar` não verifica sucesso e pode capturar a falha da gravação. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:270`)

5. **Os três testes de `VoltaTresTest`** [nao verificado]

   - **Primeiro: morde a regressão da cópia velha.** Lê PEDIDO, adota, encerra pela cópia antiga e exige trilha `antes=em_curso`, ator e metadados presentes. É sequencial; não prova concorrência/travas e não compara os valores exatos da sessão e do instante preservados. (`app/folha/tests/test_porta_sem_beco_item2.py:430`, `app/folha/tests/test_porta_sem_beco_item2.py:432`, `app/folha/tests/test_porta_sem_beco_item2.py:438`, `app/folha/tests/test_porta_sem_beco_item2.py:442`)
   - **Segundo: morde a persistência da falha do clique, mas não A→B no mesmo disparo.** Executa um comando exclusivo para A e outro para B. O mock também levanta antes de qualquer escrita real de `assumir`. (`app/folha/tests/test_porta_sem_beco_item2.py:453`, `app/folha/tests/test_porta_sem_beco_item2.py:459`, `app/folha/tests/test_porta_sem_beco_item2.py:460`, `app/folha/tests/test_porta_sem_beco_item2.py:462`)
   - **Terceiro: pode passar vazio quanto a “encerrar levantou”.** Não exige que o mock de `encerrar` seja chamado. Removendo a recuperação do handler, A ainda entra em `falharam`, B continua medindo e o comando termina com `SystemExit`; suas assertivas continuam satisfeitas. (`app/folha/tests/test_porta_sem_beco_item2.py:477`, `app/folha/tests/test_porta_sem_beco_item2.py:478`, `app/folha/tests/test_porta_sem_beco_item2.py:480`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:170`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:275`)

6. **Classes de reprovação**

   **Presentes:** selo sem mordida no terceiro teste; cobertura insuficiente do “mesmo disparo” no segundo; e campos misturando tentativas após o reinício — por exemplo, UUID novo com resultado/sessão/instantes anteriores. (`app/folha/tests/test_porta_sem_beco_item2.py:477`, `app/folha/tests/test_porta_sem_beco_item2.py:459`, `app/folha/porta_export.py:1105`, `app/folha/porta_export.py:1049`)

   **Não identifiquei introdução de segundo juiz, segundo escritor fora da porta ou derivação paralela:** os helpers novos delegam às funções existentes. Tampouco identifiquei inversão das travas examinadas. As leituras de relógio adicionadas são timestamps da transição; não há demonstração de divergência de dia causada por G. (`app/folha/porta_export.py:1215`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1052`)

   Resultado de execução dos testes, concorrência real e ocorrência desses caminhos em produção: **[nao verificado]**. [nao verificado]

## 17/11 13:58 · sessao c06c9acddf9f · - · - · origem=Ronald (tmux)

**Q3 — retorno compatível nos usos encontrados.**

- `_falhar` usa o estado anterior na trilha e devolve `bool(antes)`; `encerrar` e `falhar_ao_assumir` repassam esse bool (`app/folha/porta_export.py:1073`, `:1075`, `:1077`, `:1199`, `:1215`).
- `pedir` converte explicitamente para bool; `abrir_a_porta` testa `if criado` (`app/folha/porta_export.py:1106`, `:1307`, `:1308`).
- `assumir` testa `not _mudar_estado`; o sucesso devolve `'pedido'`, string não vazia, e o fracasso devolve `None`. Depois retorna linha ou `None`, que o comando testa com `is None` (`app/folha/porta_export.py:913`, `:1047`, `:1055`, `:1131`, `:1134`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:156`).
- `concluir` ignora esse retorno, mantendo o contrato dict ou `None`; já travou a mesma linha com estado e tentativa antes da chamada interna. O comando testa `res is None` e depois `res['resultado']` (`app/folha/porta_export.py:1179`, `:1180`, `:1187`, `:1192`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:172`, `:179`).
- A quinta chamada direta fica no teste e ignora o retorno (`app/folha/tests/test_porta_sem_beco_item2.py:102`). Comparações desse retorno com `True`/`False` não foram encontradas na cópia [nao verificado fora da cópia].

**Q5 — duas limitações concretas nos testes novos.** [nao verificado]

1. O primeiro testa efetivamente cópia antiga, trilha `antes=em_curso`, ator e presença dos metadados da adoção. Porém é sequencial; chama `assumir` diretamente, sem advisory lock, e só exige que sessão e instante sejam verdadeiros, sem comparar os valores exatos (`app/folha/tests/test_porta_sem_beco_item2.py:428`, `:430`, `:432`, `:438`, `:439`, `:442`, `:443`).

2. O segundo testa que falha de adoção do clique persiste estado, motivo e uma recusa. **Não prova continuidade A→B no mesmo disparo:** executa um comando exclusivo para A e outro exclusivo para B. Além disso, o mock levanta antes de qualquer escrita real de `assumir`, cobrindo apenas essa posição da falha (`app/folha/tests/test_porta_sem_beco_item2.py:451`, `:453`, `:459`, `:460`, `:462`, `:463`, `:464`, `:465`).

3. O terceiro **pode passar vazio quanto a “encerrar levantou”**: instala o mock, mas não verifica que ele foi chamado. Se a chamada de recuperação for removida do comando, o handler ainda acrescenta A a `falharam`, continua para B e levanta `SystemExit` no final; as duas assertivas permanecem satisfeitas. Portanto ele prova B pronta após falha de concluir, mas não exige que a exceção de encerrar tenha ocorrido (`app/folha/tests/test_porta_sem_beco_item2.py:471`, `:477`, `:478`, `:480`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `:170`, `:171`, `:275`, `:276`).

Observação corrigida: **não há evidência de `pedido_em` velho no retry**, pois `_mudar_estado(..., para=PEDIDO)` regrava `pedido_em` dinamicamente. Os outros campos antigos continuam presentes porque a limpeza da cópia em memória deixou de ser a base da persistência (`app/folha/porta_export.py:1049`, `:1052`, `:1105`, `:1106`).

Execução dos testes e comportamento em produção: [nao verificado]. [nao verificado]

## 17/11 13:57 · sessao 0721ab2ad95a · - · - · origem=Ronald (tmux)

Q4: há caminho sem registro da falha original: cron/manual chama `assumir` sem tentativa; se levantar, `falhar_ao_assumir` retorna `False` sem gravar. O comando só imprime e acrescenta a empresa a `falharam`; depois afirma “cada uma FALHOU com seu motivo”, mesmo sem essa garantia. (`app/folha/porta_export.py:1125`, `app/folha/porta_export.py:1209`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:269`.)

`_fechar` não é escritor alternativo: executa o callback e captura qualquer `Exception`, imprimindo em stdout. Não verifica sucesso nem preserva o erro em outro registro. Se a recuperação falhar, a tentativa pode continuar aberta e o vigia posteriormente gravar apenas o motivo genérico de prazo/processo morto. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:59`, `app/folha/porta_export.py:1226`, `app/folha/porta_export.py:1237`.)

Nos dois callsites atuais, `_fechar` executa dentro da advisory lock adquirida para empresa/competência; outro detentor dessa mesma trava impede a entrada no ramo. A tentativa também é conferida novamente pelo CAS. Não encontrei nesse caminho fechamento de tentativa de outro detentor vivo da mesma trava; comportamento com perda/substituição de sessão de conexão [nao verificado]. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/folha/porta_export.py:982`, `app/folha/porta_export.py:1042`.)

Existe caminho anterior ao diff que merece ressalva: `disparar` põe `Popen` e saída do contexto de arquivo no mesmo `try`; um `OSError` no fechamento do arquivo depois de o filho nascer chama `encerrar`, que aceita também `EM_CURSO` e não consulta vivacidade. Pode, portanto, fechar o estado da tentativa já adotada pelo filho vivo; ocorrência real [nao verificado]. (`app/folha/porta_export.py:1335`, `app/folha/porta_export.py:1337`, `app/folha/porta_export.py:1339`, `app/folha/porta_export.py:1199`.)

Os campos gravados seguem pela porta existente: `_falhar` fornece `resultado` e `motivo`; `_mudar_estado` grava `estado`, `falhou_em` e `calculado_em`; `_recusa` escreve a trilha por `recusa_critica.registrar`. (`app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1063`, `app/core/recusa_critica.py:70`.) `vigiar` lê estado/sessão/prazo; `estado_da_porta` lê estado/tentativa; `conferir` transforma FALHA em texto fixo, sem mostrar o motivo. (`app/folha/porta_export.py:1225`, `app/folha/porta_export.py:1275`, `app/folha/porta_export.py:823`.) O contador lê exclusivamente as linhas de recusa; stdout de `_fechar` não alimenta esse mecanismo. (`app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `bin/recusa_estado.py:68`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`.)

Q6: os helpers novos delegam ao escritor existente, sem introduzir segundo juiz ou segundo escritor de estado; permanece a lacuna de observabilidade acima. (`app/folha/porta_export.py:1215`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`.) Não executei testes nem acessei banco/log; efeitos reais [nao verificado].

## 17/11 13:57 · sessao e84a60e96239 · - · - · origem=Ronald (tmux)

Censo completo de `_mudar_estado`: quatro chamadores em produção e um direto em teste; o auxiliar sempre abre `atomic`, trava a linha filtrada por estado/ficha, deriva `detalhe` da linha travada e retorna estado anterior ou `None` (`app/folha/porta_export.py:1040–1055`).

| Chamador direto | Contexto e travas |
|---|---|
| `_falhar` | Sem `atomic` próprio; delega ao auxiliar e depois registra recusa (`app/folha/porta_export.py:1072–1077`). Vem de `encerrar` (`:1199`), de `vigiar` para EM_CURSO (`:1229`) ou PEDIDO (`:1238`). |
| `pedir`, retomada | O `atomic` da tentativa de criação já terminou antes de chamar o auxiliar; não retém trava explícita anterior nesse caminho (`app/folha/porta_export.py:1093–1107`). Vem de `abrir_a_porta` (`:1307`), acionada pela view sem `atomic` local (`app/folha/views.py:101–131`). |
| `assumir`, com tentativa | Sem `atomic` próprio; faz leitura comum e chama o auxiliar (`app/folha/porta_export.py:1129–1134`). No comando, executa dentro da advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138–150`). |
| `concluir` | Já está dentro de `atomic` externo e já travou a MESMA linha; chama `lavrar` antes do auxiliar, que abre `atomic` aninhado e pede novamente a mesma trava (`app/folha/porta_export.py:1179–1188`). No comando, permanece sob advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138–168`). |
| Teste `test_CONCLUIDO_sem_o_carimbo_DESTA_tentativa_nao_le_o_verde_antigo` | Chamada direta, sem uso do retorno, dentro de classe `TestCase`; nenhum advisory lock explícito nesse teste (`app/folha/tests/test_porta_sem_beco_item2.py:34`, `:96–104`). |

Entradas indiretas relevantes: `encerrar` é chamado pela recuperação do comando, ainda dentro da advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `:168`), por `falhar_ao_assumir` nesse mesmo contexto (`:152`; `app/folha/porta_export.py:1215`), e por `disparar` quando o subprocesso não nasce, sem advisory lock local (`app/folha/porta_export.py:1331–1340`). `vigiar` vem da leitura da porta (`:1272`), do censo global antes da aquisição da advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:73`, `:138`; `app/folha/porta_export.py:1252`) e de `assumir` sem tentativa, sob a advisory do comando (`app/folha/porta_export.py:1135–1138`). O helper de testes chama `assumir` e `concluir` sem advisory lock local (`app/folha/porta_export.py:887–890`).

Não encontrei ciclo de espera introduzido: comando adquire advisory → linha do pedido; `concluir` adquire pedido → carimbo → novamente o próprio pedido; a advisory usa `pg_try_advisory_lock`, sem espera, e `medicao_viva` apenas consulta `pg_locks` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`; `app/folha/porta_export.py:982`, `:1002–1006`, `:1179–1188`, `:779`). O censo anterior ao comando encerra cada chamada do auxiliar antes de chegar à advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:73`, `:138`; `app/folha/porta_export.py:1040–1055`, `:1249–1253`). Ausência de deadlock em execução real: [nao verificado].

Q2: **há dependência da cópia em memória**. `pedir` faz explicitamente `linha.detalhe = {}` antes de chamar o auxiliar; essa limpeza agora é ignorada porque a base vem de `atual.detalhe` (`app/folha/porta_export.py:1105`, `:1049`). Assim, retomada ganha nova `tentativa`, `pedido_em`, origem/ator/competência, mas conserva `resultado`, `n`, `motivo`, `pid`, `backend_pid`, `em_curso_em`, `concluido_em` e `falhou_em` anteriormente existentes, pois os extras não os removem (`app/folha/porta_export.py:1106–1107`, `:1050–1052`). Escritores desses campos: `assumir` (`:1132`, `:1141–1143`), `_falhar` (`:1073–1074`), `concluir` (`:1187–1188`) e carimbos automáticos de transição (`:1052`).

**Correção de hipótese preliminar:** `pedido_em` não permanece velho: `para=PEDIDO` faz `'%s_em' % para` renová-lo (`app/folha/porta_export.py:1052`, `:1106`). Portanto não há base para afirmar timeout imediato por esse campo. A mistura de campos de tentativas está demonstrada; liberação indevida do TXT por ela não está demonstrada, pois a porta decide por estado/tentativa e carimbo, e `assumir` substitui o `backend_pid` antes de EM_CURSO (`app/folha/porta_export.py:1275–1291`, `:1131–1132`). Os leitores encontrados de `resultado/n/motivo` do registro são sobretudo asserts dos testes (`app/folha/tests/test_porta_sem_beco_item2.py:121–130`); `vigiar` só consulta `backend_pid` quando estado é EM_CURSO (`app/folha/porta_export.py:1226–1228`).

## 17/11 13:38 · sessao 2977a8187bea · PORTA-EXPORT · verificador · origem=Code

**Não considero a obra fechada:** a exceção em `assumir` ainda não grava FALHOU/motivo/recusa; e o `antes` de `encerrar` pode diferir do estado vencido pelo CAS. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`; `app/folha/porta_export.py:1065`; `app/folha/porta_export.py:1192`)

Execução dos testes, reprodução das corridas e ocorrência em produção: **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê**

   | Campo | Escritores e leitores encontrados |
   |---|---|
   | Pedido: `detalhe.tentativa` | `pedir` cria/renova UUID; `assumir` adota a recebida ou cria outra. `_mudar_estado` transporta o detalhe. Leem `_ficha`/CAS, adoção, conclusão, recusa, estado da porta e argv do subprocesso. (`app/folha/porta_export.py:1083`; `app/folha/porta_export.py:1100`; `app/folha/porta_export.py:1123`; `app/folha/porta_export.py:1135`; `app/folha/porta_export.py:1022`; `app/folha/porta_export.py:1040`; `app/folha/porta_export.py:1170`; `app/folha/porta_export.py:1060`; `app/folha/porta_export.py:1260`; `app/folha/porta_export.py:1307`) |
   | Pedido: `detalhe.ator` | `pedir` grava `getattr(ator, 'pk', None)`, recebendo `request.user` da view. A adoção preserva o campo; uma tentativa nova cron/manual substitui o detalhe sem ator. `_recusa` o lê. (`app/folha/views.py:131`; `app/folha/porta_export.py:1082`; `app/folha/porta_export.py:1100`; `app/folha/porta_export.py:1124`; `app/folha/porta_export.py:1134`; `app/folha/porta_export.py:1057`) |
   | Carimbo: `detalhe.tentativa` | `lavrar` grava o argumento, inclusive `None`; `concluir` fornece a tentativa da linha validada. O leitor decisório é `estado_da_porta`. (`app/folha/porta_export.py:768`; `app/folha/porta_export.py:784`; `app/folha/porta_export.py:1178`; `app/folha/porta_export.py:1260`) |
   | Motivo | `_falhar` persiste até 500 caracteres; `concluir` persiste `res['motivo']`. `_recusa` acrescenta a trilha; divergência recebe texto explicativo somente na linha. `registrar` normaliza espaços; o parser recupera o motivo, mas o contador usa data/porta, sem interpretar seu conteúdo. (`app/folha/porta_export.py:1067`; `app/folha/porta_export.py:1181`; `app/folha/porta_export.py:1056`; `app/folha/porta_export.py:1184`; `app/core/recusa_critica.py:84`; `app/core/recusa_critica.py:92`; `app/core/recusa_critica.py:117`) |

   **Há escrita fora de `concluir`:** `lavrar` continua escrevendo diretamente; os dois testes negativos novos a chamam; a fixture `_carimbo` escreve pedido e carimbo diretamente, com tentativa constante `fixture`. Portanto, exclusividade literal de `concluir` não existe. (`app/folha/porta_export.py:779`; `app/folha/tests/test_porta_sem_beco_item2.py:305`; `app/folha/tests/test_porta_sem_beco_item2.py:317`; `app/folha/tests/test_porta_do_export.py:42`; `app/folha/tests/test_porta_do_export.py:46`)

   Nos caminhos produtivos encontrados, o comando chama `concluir`; o helper corrigido usa `assumir → concluir`, inclusive para o teste de relatórios. Não encontrei outro chamador produtivo de escrita desse carimbo. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:157`; `app/folha/porta_export.py:887`; `app/relatorios/tests/test_cauda_trilha_do_cartao.py:109`)

   Não encontrei leitor decisório alternativo aceitando carimbo sozinho: `conferir`, `abrir_a_porta` e a view delegam à mesma porta. (`app/folha/porta_export.py:820`; `app/folha/porta_export.py:1282`; `app/folha/views.py:130`)

2. **PRONTA e caminhos legítimos**

   Nos objetos lidos pela função, **PRONTA exige pedido CONCLUIDO, tentativa não vazia e igual à do carimbo**, ambos consultados pelo mesmo dia. Os dois escapes da volta 2 estão fechados. (`app/folha/porta_export.py:1248`; `app/folha/porta_export.py:1258`; `app/folha/porta_export.py:1260`; `app/folha/porta_export.py:1268`)

   Isso não garante que o banco permaneça naquele estado até a resposta: entre as duas consultas, um cron pode substituir o pedido concluído por outra tentativa EM_CURSO, enquanto o leitor conserva o pedido antigo e encontra o carimbo antigo correspondente. É uma leitura válida do par anterior; não demonstra liberação sem jamais ter existido o par exigido. Corrida executada: **[nao verificado]**. (`app/folha/porta_export.py:1249`; `app/folha/porta_export.py:1250`; `app/folha/porta_export.py:1131`)

   Não encontrei beco estrutural novo para cron/manual/rollout: sem clique, `assumir` cria pedido EM_CURSO com UUID, e `concluir` grava o par. O rollout seleciona empresas ativas em rollout e a competência corrente; o cron está declarado com `--rollout --origem cron`. Instalação e funcionamento reais: **[nao verificado]**. (`app/folha/porta_export.py:1131`; `app/folha/porta_export.py:1178`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:46`; `app/config/crons.py:1803`)

3. **R8 alargado**

   **`assumir` continua incompleto.** Seu `except` apenas imprime, acrescenta a empresa a `falharam` e continua. Não persiste FALHOU, motivo nem linha de recusa. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`)

   Se a exceção ocorrer antes da criação e não houver pedido anterior, não nasce registro. Se houver pedido do clique e a adoção ainda não ocorreu, ele pode permanecer PEDIDO até o vigia aplicar o prazo de 300 segundos; o motivo posterior será o prazo, não a exceção original. (`app/folha/porta_export.py:1118`; `app/folha/porta_export.py:1122`; `app/folha/porta_export.py:920`; `app/folha/porta_export.py:1214`)

   **`concluir` entrou no `try` corretamente.** Exceções dentro do bloco que lavra e muda o estado desfazem ambas as escritas; depois o comando tenta `encerrar` a tentativa como FALHOU. Essa chamada de recuperação não tem proteção adicional: se ela levantar, a seguinte empresa não será alcançada. (`app/folha/porta_export.py:1172`; `app/folha/porta_export.py:1178`; `app/folha/porta_export.py:1180`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:155`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:159`)

   A frase “`concluir` levantou, nada ficou” é ampla demais: `_recusa` ocorre depois do `atomic`. Uma exceção ali não desfaz o par já confirmado. É o limite do log não transacional já descartado na triagem, não outro bloqueador por queda CAS/log. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`; `app/folha/porta_export.py:1182`; `app/folha/porta_export.py:1184`)

4. **Verdade da trilha**

   **Contraexemplo do `antes`:** `encerrar` conserva um objeto PEDIDO(t); outro chamador adota a mesma tentativa, mudando o banco para EM_CURSO(t); o encerramento aceita ambos os estados e vence de EM_CURSO, mas imprime `antes=pedido`. Além disso, escreve o JSON antigo inteiro, podendo apagar `pid`, `backend_pid` e o instante da adoção. Esse interleaving é permitido pelo código; reprodução: **[nao verificado]**. (`app/folha/porta_export.py:1065`; `app/folha/porta_export.py:1192`; `app/folha/porta_export.py:1124`; `app/folha/porta_export.py:1035`; `app/folha/porta_export.py:1039`; `app/folha/porta_export.py:1042`)

   Nos caminhos do vigia, o CAS aceita um único estado; em `concluir`, EM_CURSO é conferido sob trava da linha. Nesses sítios, o problema acima não se aplica. (`app/folha/porta_export.py:1206`; `app/folha/porta_export.py:1215`; `app/folha/porta_export.py:1173`)

   Há também uma ambiguidade em **`origem`**: nasce como origem do pedido e é sobrescrita pela origem do executor durante a adoção. O CLI aceita `--tentativa` com `--origem manual`; nessa combinação, a linha deixa de preservar literalmente a origem do pedido. (`app/folha/porta_export.py:1083`; `app/folha/porta_export.py:1125`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:39`)

   **Ator é pk nesse caminho**, nunca username: a porta grava `.pk` e o imprime no motivo. Cron/manual sem ator saem `ator=-`; o campo principal `usuario` também fica `-`, pois `_recusa` passa `request=None`. (`app/folha/porta_export.py:1082`; `app/folha/porta_export.py:1057`; `app/folha/porta_export.py:1058`; `app/core/recusa_critica.py:77`)

5. **Mordida dos testes** [nao verificado]

   - Os freezes existem nas duas classes; os negativos exercitam a porta real contra carimbo sozinho e tentativa ausente dos dois lados. (`app/folha/tests/test_porta_sem_beco_item2.py:19`; `:292`; `:302`; `:310`; `:387`)
   - M9 verifica retorno, ausência de carimbo/recusa e preservação da sucessora. O marcador de log não esvazia a verificação: o filtro seleciona a porta. (`app/folha/tests/test_porta_sem_beco_item2.py:328`; `:341`; `:29`)
   - M12 verifica o perdedor do CAS e uma linha, mas ambos os objetos começam EM_CURSO. Não cobre a adoção PEDIDO→EM_CURSO entre leitura e encerramento, nem verifica ator. (`app/folha/tests/test_porta_sem_beco_item2.py:350`; `:354`; `:358`)
   - O relógio simulado hoje/amanhã morde a antiga leitura dupla; o reclique exige um único subprocesso, impedindo que todos os POSTs passem inertes. (`app/folha/tests/test_porta_sem_beco_item2.py:366`; `:368`; `:384`)
   - **R8ConcluirTest prova tratamento na borda e continuação para B.** Para A, lança antes do corpo real; não prova rollback após lavrar, ausência de carimbo parcial, uma recusa ou tratamento de `assumir`. (`app/folha/tests/test_porta_sem_beco_item2.py:400`; `:405`; `:408`)
   - O acréscimo do shell exige ausência de arquivo também para a porta e verifica sua frase, com datas explícitas. (`bin/tests/test_recusa_estado.sh:30`; `:44`; `:48`)

6. **Classes históricas**

   Não encontrei segundo juiz nem derivação paralela nova: texto e clique continuam consultando `estado_da_porta`; a dupla leitura do dia foi removida. Não identifiquei inversão nova de travas no trecho: o comando mantém advisory lock antes da trava da linha em `concluir`. (`app/folha/porta_export.py:820`; `app/folha/porta_export.py:1282`; `app/folha/porta_export.py:1248`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:130`; `app/folha/porta_export.py:1173`)

   Permanecem a escrita direta de fixtures fora de `concluir`, a cobertura insuficiente do selo para rollback/R8 completo e o campo `origem` servindo ao pedido e ao executor. Os bloqueadores concretos desta revisão são a falha de `assumir` sem registro e a trilha `antes` potencialmente falsa. (`app/folha/tests/test_porta_do_export.py:42`; `app/folha/tests/test_porta_sem_beco_item2.py:400`; `app/folha/porta_export.py:1125`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`; `app/folha/porta_export.py:1065`)

## 17/11 13:36 · sessao d53c28693167 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **R8 não cumpre “assumir levantou → FALHOU com motivo e uma recusa”.** O `except` de `assumir` apenas imprime, acrescenta a empresa a `falharam` e segue; não chama `encerrar`, não persiste motivo nem emite `_recusa`. Se falhar antes da criação, não nasce registro; se já existia pedido do clique, ele pode continuar `PEDIDO` até o vigia expirar os 300 segundos. A seguinte empresa é tentada, supondo que a saída e a liberação da trava não levantem. Fontes: `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`, `app/folha/porta_export.py:1118`, `app/folha/porta_export.py:1122`, `app/folha/porta_export.py:1214`, `app/folha/porta_export.py:920`. Ocorrência em produção: [nao verificado].

2. **`antes` não é necessariamente o estado vencido pelo CAS amplo de `encerrar`.** `_falhar` guarda `antes` do objeto em memória; `encerrar` aceita banco em `PEDIDO` **ou** `EM_CURSO`, com filtro somente da tentativa. Se outra chamada adotar o mesmo pedido entre essa leitura e o UPDATE, o banco pode transitar `PEDIDO→EM_CURSO` preservando a tentativa; o encerramento então vence de `EM_CURSO`, mas escreve `antes=pedido`. Como o JSON inteiro sai do objeto antigo, também pode apagar `pid`, `backend_pid` e o instante da adoção. Nos caminhos de `vigiar`, o estado permitido é unitário; no `concluir`, `EM_CURSO` está protegido pela linha travada. Fontes: `app/folha/porta_export.py:1065`, `app/folha/porta_export.py:1192`, `app/folha/porta_export.py:1124`, `app/folha/porta_export.py:1035`, `app/folha/porta_export.py:1039`, `app/folha/porta_export.py:1042`, `app/folha/porta_export.py:1206`, `app/folha/porta_export.py:1215`, `app/folha/porta_export.py:1173`. Interleaving reproduzido: [nao verificado].

3. **Rollback de concluir:** falhas dentro do bloco que trava o pedido, lavra e muda o estado ficam no mesmo `atomic`; o comando captura a exceção e chama `encerrar` sobre a tentativa. Entretanto, a afirmação comentada “`concluir` levantou, nada ficou” não cobre literalmente toda a função: a cópia do detalhe e `_recusa` são posteriores ao `atomic`. Isso é o limite já associado ao log não transacional, não proponho reabrir a queda CAS/log descartada. Fontes: `app/folha/porta_export.py:1172`, `app/folha/porta_export.py:1178`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1182`, `app/folha/porta_export.py:1184`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:158`.

4. **Limite de R8 se o próprio encerramento levantar:** a chamada a `encerrar` no tratamento da exceção não tem proteção adicional; uma exceção nela interrompe o laço, logo a continuação não é incondicional. Fonte: `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:159`. Ocorrência concreta: [nao verificado].

5. **R8ConcluirTest cobre a borda, não o rollback interno.** Há freeze na classe. Para A, o mock lança antes de executar qualquer linha do `concluir` real; somente B usa a implementação real. O teste verifica FALHA, motivo e B pronta, mas não verifica ausência de carimbo parcial nem quantidade de recusas. Fontes: `app/folha/tests/test_porta_sem_beco_item2.py:387`, `app/folha/tests/test_porta_sem_beco_item2.py:400`, `app/folha/tests/test_porta_sem_beco_item2.py:405`, `app/folha/tests/test_porta_sem_beco_item2.py:408`.

6. **M12 morde perdedor do CAS, mas não o `antes` concorrente acima.** Ambos os objetos começam `EM_CURSO`, o primeiro encerra e o segundo perde; não há adoção `PEDIDO→EM_CURSO` entre leitura e CAS. Fontes: `app/folha/tests/test_porta_sem_beco_item2.py:350`, `app/folha/tests/test_porta_sem_beco_item2.py:354`, `app/folha/tests/test_porta_sem_beco_item2.py:358`.

7. **PRONTA local está protegida; fotografia atual do banco não está garantida.** A função exige pedido local `CONCLUIDO`, tentativa verdadeira e igualdade com carimbo. Ela pode ler o pedido concluído antigo, um cron substituí-lo por nova tentativa em curso, e ainda ler o carimbo antigo correspondente. Isso continua linearizável ao instante inicial em que aquele par era válido; não demonstra liberação sem jamais ter existido o par exigido. Fontes: `app/folha/porta_export.py:1249`, `app/folha/porta_export.py:1250`, `app/folha/porta_export.py:1258`, `app/folha/porta_export.py:1260`, `app/folha/porta_export.py:1268`, `app/folha/porta_export.py:1131`. Corrida executada: [nao verificado].

8. **Ator:** `pedir` grava `getattr(ator, 'pk', None)`, a adoção preserva o campo e a recusa o imprime no motivo. Cron/manual criam detalhe sem `ator`, saindo `ator=-`; o campo principal `usuario` da linha também sai `-`, porque `_recusa` chama `registrar(None, ...)`. Nome de usuário não é introduzido por esse caminho da porta. Fontes: `app/folha/porta_export.py:1082`, `app/folha/porta_export.py:1083`, `app/folha/porta_export.py:1124`, `app/folha/porta_export.py:1134`, `app/folha/porta_export.py:1056`, `app/folha/porta_export.py:1058`, `app/core/recusa_critica.py:77`.

Nenhum teste foi executado; resultados de execução: [nao verificado]. [nao verificado]

## 17/11 13:35 · sessao 7dede955e4c0 · - · - · origem=Ronald (tmux)

- **Pedido — tentativa:** `pedir` cria UUID e o renova ao refazer (`app/folha/porta_export.py:1083`, `app/folha/porta_export.py:1100`); `assumir` adota a tentativa recebida ou cria outra para cron/manual (`app/folha/porta_export.py:1122`, `app/folha/porta_export.py:1135`). Leitores: `_ficha`/CAS, `assumir`, `concluir`, `_recusa`, `estado_da_porta` e argv do subprocesso (`app/folha/porta_export.py:1022`, `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1123`, `app/folha/porta_export.py:1170`, `app/folha/porta_export.py:1060`, `app/folha/porta_export.py:1260`, `app/folha/porta_export.py:1307`).

- **Pedido — ator:** escritor explícito é `pedir`, via `getattr(ator, 'pk', None)`; a view fornece `request.user`, e a recusa imprime esse pk (`app/folha/porta_export.py:1082`, `app/folha/porta_export.py:1100`, `app/folha/views.py:131`, `app/folha/porta_export.py:1057`). `assumir` com tentativa conserva ator mas sobrescreve origem; sem tentativa substitui todo detalhe e não inclui ator, como esperado para nova tentativa cron/manual (`app/folha/porta_export.py:1124`, `app/folha/porta_export.py:1131`).

- **Carimbo — tentativa:** `lavrar` grava o argumento recebido, incluindo `None`; `concluir` passa a tentativa validada da linha atual (`app/folha/porta_export.py:768`, `app/folha/porta_export.py:784`, `app/folha/porta_export.py:1178`). O leitor decisório exige pedido `CONCLUIDO`, tentativa verdadeira e igualdade com o carimbo (`app/folha/porta_export.py:1258`).

- **Ainda há escrita de carimbo fora de `concluir`:** `lavrar` continua função pública capaz de escrever diretamente; dois testes novos a chamam sem pedido ou sem tentativa (`app/folha/porta_export.py:779`, `app/folha/tests/test_porta_sem_beco_item2.py:305`, `app/folha/tests/test_porta_sem_beco_item2.py:317`). A fixture escreve diretamente tanto pedido quanto carimbo (`app/folha/tests/test_porta_do_export.py:42`, `app/folha/tests/test_porta_do_export.py:46`). Não encontrei outro chamador produtivo que escreva esse carimbo; o comando usa `concluir` e o helper passou a usar `assumir → concluir` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:157`, `app/folha/porta_export.py:885`).

- **Testes de outros apps:** o uso encontrado em relatórios chama o helper corrigido (`app/relatorios/tests/test_cauda_trilha_do_cartao.py:109`). Os demais usos do helper estão em integração/fixtures de folha (`app/folha/tests/test_integracao.py:160`, `app/folha/tests/test_txt_le_a_celula_e_o_juiz.py:155`, `app/folha/tests/test_sm1_monster_export.py:124`).

- **Nenhum leitor decisório encontrado aceita carimbo sozinho:** `conferir` delega ao estado, o clique delega ao estado e a view delega ao clique; o script externo encontrado chama `conferir` (`app/folha/porta_export.py:820`, `app/folha/porta_export.py:1282`, `app/folha/views.py:130`, `bin/txt_antes_badge_linha.py:60`). PRONTA sem as condições do par não atravessa a cadeia de `elif` (`app/folha/porta_export.py:1254`).

- **Motivo da recusa:** `_falhar` grava no pedido o motivo truncado a 500 caracteres e envia o original à recusa; `concluir` grava `res['motivo']`, mas divergência grava vazio e usa fallback apenas na linha (`app/folha/porta_export.py:1067`, `app/folha/porta_export.py:1069`, `app/folha/porta_export.py:1181`, `app/folha/porta_export.py:1184`). `_recusa` acrescenta antes/depois/origem/ator; `registrar` normaliza espaços e grava WARNING (`app/folha/porta_export.py:1056`, `app/core/recusa_critica.py:80`, `app/core/recusa_critica.py:85`).

- **Leitores do motivo:** o parser o devolve como campo, mas o contador da porta ignora seu conteúdo e conta por data local da linha e porta; o wrapper de estado usa esse contador (`app/core/recusa_critica.py:49`, `app/core/recusa_critica.py:92`, `app/core/recusa_critica.py:113`, `app/core/recusa_critica.py:136`, `bin/recusa_estado.py:68`). A mensagem de FALHA na tela não lê o motivo persistido (`app/folha/porta_export.py:823`, `app/folha/porta_export.py:933`).

## 17/11 13:35 · sessao 267c99086cf9 · - · - · origem=Ronald (tmux)

Revisão estática; resultado da execução dos testes = [nao verificado]. [nao verificado]

- **Freeze existe e está importado:** ambas as classes novas têm `@freeze_time`, com import de `freezegun`; não encontrei ausência de congelamento nesses testes. `app/folha/tests/test_porta_sem_beco_item2.py:19`, `:292`, `:387`.
- **Os negativos da porta mordem os dois bugs declarados:** criam carimbo sem pedido e par sem tentativa, chamam `estado_da_porta` real e exigem recusa/FALHA. `app/folha/tests/test_porta_sem_beco_item2.py:302`, `:310`.
- **Helper verde:** exercita o helper real e exige PRONTA, pedido CONCLUIDO e tentativa preenchida; não espiona `assumir`/`concluir`, portanto prova o resultado, não a exclusividade desse caminho. `app/folha/tests/test_porta_sem_beco_item2.py:320`; implementação efetivamente usa ambos em `app/folha/porta_export.py:887`.
- **M9:** cria sucessora real, conserva detalhe da velha e verifica retorno `None`, ausência de carimbo, ausência de recusa e sucessora EM_CURSO; o marcador de log não torna a asserção vazia, pois `_linhas_da_porta` filtra só a porta. `app/folha/tests/test_porta_sem_beco_item2.py:328`, `:29`.
- **M12:** verifica CAS perdedor mediante duas cópias lidas antes do primeiro encerramento, contando exatamente uma recusa; porém é sequencial e cobre somente EM_CURSO→FALHOU, sem testar `PEDIDO→EM_CURSO` entre leitura e CAS de `encerrar`, nem ator. `app/folha/tests/test_porta_sem_beco_item2.py:346`.
- **Relógio:** o mock retorna hoje na primeira chamada e amanhã nas seguintes; ao exigir PRONTA com os registros de hoje, morde a antiga leitura dupla entre pedido/carimbo. Não é um contador literal de chamadas ignoradas, mas cobre o efeito relevante. `app/folha/tests/test_porta_sem_beco_item2.py:361`.
- **Reclique:** exercita POSTs e CAS reais; substitui apenas subprocesso e medição indevida no request, exigindo uma linha e um disparo. Não verifica respostas HTTP, mas `popen.call_count == 1` impede que todos os POSTs sejam inertes. `app/folha/tests/test_porta_sem_beco_item2.py:371`.
- **Lacuna principal: R8ConcluirTest não prova atomicidade de concluir.** Para A, o mock lança ANTES de chamar a implementação real; para B, chama a real. Isso morde tratamento da exceção no comando, motivo de A e continuação de B, mas não falha após lavrar, rollback do carimbo/estado, exceção pós-commit, nem uma linha de recusa. `app/folha/tests/test_porta_sem_beco_item2.py:398`, `:400`, `:408`.
- **Essa lacuna importa:** a `_recusa` real ocorre depois do `transaction.atomic`; uma exceção ali não é revertida pelo bloco, ao contrário do comentário do comando. `app/folha/porta_export.py:1172`, `:1182`, `:1184`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`.
- **Nenhum teste novo injeta falha em `assumir`**; o R8 novo injeta apenas `concluir`. `app/folha/tests/test_porta_sem_beco_item2.py:396`.
- **Fixtures alteradas:** `_carimbo` ainda escreve diretamente pedido+carimbo, com tentativa constante `fixture`, fora de `concluir`; assim continua adequada para testar o leitor/texto, mas não certifica escritor único nem protocolo do job. `app/folha/tests/test_porta_do_export.py:33`, `:42`, `:46`. Já `_pela_porta` passou a usar `assumir→concluir`. `app/folha/tests/test_porta_sem_beco.py:34`.
- **Shell:** o acréscimo realmente torna obrigatórios `sem_arquivo` da porta e texto “sem arquivo”; datas são explícitas, sem relógio solto nesse trecho. `bin/tests/test_recusa_estado.sh:29`, `:44`, `:48`.

## 17/11 12:42 · sessao 04f310b9fdf3 · PORTA-EXPORT · verificador · origem=Code

**Item 2 ainda não fecha integralmente.** Há liberação sem pedido/tentativa concluída, janela para término ruim ficar sem recusa e reemissão de recusa pelo POST. A proteção contra conclusão atrasada e contra FALHOU encoberto por verde antigo está implementada no caminho normal. (`app/folha/porta_export.py:1238–1257`, `:1056–1060`, `:1163–1174`; `app/folha/views.py:130–135`; `app/core/respostas.py:50–56`.)

Execução dos testes, comportamento em produção, banco, logs e correspondência histórica dos commits com o hash informado: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

As identidades de pedido e carimbo são empresa × competência × dia: competência na chave, empresa no escopo e dia em `data_ref`; a constraint inclui esses quatro campos. (`app/folha/porta_export.py:779–784`, `:933–941`, `:1077–1079`; `app/inteligencia/models.py:103–105`.)

| Campo | Escritor | Leitor |
|---|---|---|
| Pedido: `estado` | `pedir` cria PEDIDO; `assumir` cria EM_CURSO; `_mudar_estado` faz as demais transições, chamado por `pedir`, `assumir`, `_falhar` e `concluir`. (`app/folha/porta_export.py:1030–1039`, `:1056–1058`, `:1073–1079`, `:1089–1090`, `:1114–1126`, `:1170–1171`.) | CAS, repetição, adoção, conclusão, vigia, varredura e porta. (`app/folha/porta_export.py:1034`, `:1086`, `:1114–1119`, `:1164`, `:1193–1198`, `:1217`, `:1242–1249`.) |
| Pedido: `tentativa` | UUID novo em `pedir` e em `assumir` sem tentativa; adoção conserva o UUID recebido. (`app/folha/porta_export.py:1010–1012`, `:1073`, `:1090`, `:1112–1125`.) | Ficha do CAS, adoção, conclusão, objeto da recusa, argv do job e comparação com carimbo. (`app/folha/porta_export.py:1015–1017`, `:1035–1036`, `:1051–1052`, `:1113–1114`, `:1160–1168`, `:1248`, `:1296`.) |
| Pedido: `pid` | `assumir`, com `os.getpid()`. O PID retornado por `Popen` vai à auditoria, sem atualização do pedido naquele ponto. (`app/folha/porta_export.py:1115`, `:1125`, `:1303–1315`.) | O teste exige presença; o vigia operacional consulta `backend_pid`, não `pid`. (`app/folha/tests/test_porta_sem_beco_item2.py:60–64`; `app/folha/porta_export.py:1193–1196`.) |
| Pedido: `backend_pid` | `assumir` consulta `pg_backend_pid()` e grava a sessão. (`app/folha/porta_export.py:1108–1115`, `:1125`.) | `vigiar → medicao_viva`, comparando-o com `pg_locks.pid`. (`app/folha/porta_export.py:997–1001`, `:1194`.) |
| Pedido: `resultado`, `n` | `resultado_de` produz; `concluir` persiste ambos. `_falhar` grava `resultado='falhou'`, sem acrescentar `n`. (`app/folha/porta_export.py:1056–1058`, `:1136–1147`, `:1170–1171`.) | Os testes leem o registro; o command lê o retorno de `concluir`. A porta decide pelo estado e pelo carimbo, sem consultar esses dois campos persistidos. (`app/folha/tests/test_porta_sem_beco_item2.py:121–129`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:154–164`; `app/folha/porta_export.py:1241–1257`.) |
| Pedido: `motivo` | `_falhar`, limitado a 500 caracteres; `concluir`, com o motivo de `resultado_de`. (`app/folha/porta_export.py:1058`, `:1136–1144`, `:1171`.) | Testes conferem o motivo persistido. A mensagem pública deixou de lê-lo; a recusa recebe o motivo como argumento. (`app/folha/tests/test_porta_sem_beco_item2.py:56`, `:130`; `app/folha/porta_export.py:1050`, `:1174`, `:1261–1262`.) |
| Pedido: `pedido_em` | Criação em `pedir`; repetição por `_mudar_estado`, que gera `pedido_em` a partir do nome do estado. (`app/folha/porta_export.py:1033`, `:1074`, `:1089–1090`.) | `vigiar`, para os 300 segundos sem adoção. (`app/folha/porta_export.py:1200–1205`.) |
| Pedido: `em_curso_em`, `concluido_em`, `falhou_em` | `_mudar_estado`; `assumir` sem tentativa escreve diretamente `em_curso_em`. Esses timestamps não participam da decisão de vida atual, que usa sessão/trava ou `pedido_em`. (`app/folha/porta_export.py:1033`, `:1126`, `:1192–1205`.) | Leitor operacional específico desses timestamps: **[nao verificado]**. |
| Pedido/carimbo: `calculado_em` | `auto_now` do modelo; `_mudar_estado` também o atualiza explicitamente. (`app/inteligencia/models.py:96`; `app/folha/porta_export.py:1037`.) | Uso específico para liberar ou vigiar a porta: não aparece nos respectivos julgamentos. (`app/folha/porta_export.py:1192–1205`, `:1238–1257`.) |
| Carimbo: `tentativa` | Somente `lavrar`; `concluir` fornece a tentativa corrente. (`app/folha/porta_export.py:784`, `:1168`.) | `estado_da_porta`, quando o pedido está CONCLUIDO; testes conferem o vínculo. (`app/folha/porta_export.py:1247–1248`; `app/folha/tests/test_porta_sem_beco_item2.py:92`, `:268`.) |
| Carimbo: `erro_cartao_x_txt` | `medir` produz; `lavrar` persiste. (`app/folha/porta_export.py:671–675`, `:737`, `:790`.) | Teste verifica a persistência. O texto público não lê mais esse campo. (`app/folha/tests/test_porta_sem_beco_item2.py:132–133`; `app/folha/porta_export.py:1261–1262`.) |
| Carimbo: `valor` | `lavrar`: `falhas`, ou `-1` quando ausente. (`app/folha/porta_export.py:779–781`.) | `estado_da_porta` classifica o escalar; `conferir` imprime a quantidade divergente. (`app/folha/porta_export.py:871–872`, `:1252–1257`.) |

**Escritores externos existem nos testes:** `_carimbo` escreve diretamente o snapshot; o teste R7 altera diretamente `backend_pid`. Não são escritores operacionais novos. O helper verde chama o mesmo `lavrar`. (`app/folha/tests/test_porta_do_export.py:33–42`; `app/folha/tests/test_porta_sem_beco_item2.py:50–52`; `app/folha/porta_export.py:875–887`.)

O escritor genérico `inteligencia.metricas.calcular` exige chave registrada; os calculadores declarados usam outras chaves. Não há nesses registros um segundo escritor identificado para as chaves da porta. (`app/inteligencia/metricas.py:36–56`; `app/inteligencia/calculadores.py:9`, `:112`, `:183`, `:240`, `:302`.)

**Os dois PIDs têm significados separados**, mas “PID gravado e conferido” precisa dessa ressalva: o `pid` do SO fica como trilha e só nasce no pedido ao assumir; quem é conferido é o PID do backend. (`app/folha/porta_export.py:899–904`, `:1073–1079`, `:1115`, `:1194`.)

**2. Liberação e tentativa atrasada**

**Sim, há caminho para `conferir == ''` sem tentativa concluída:** pedido ausente + carimbo do dia com zero chega a PRONTA. `lavrar` aceita `tentativa=None`, e o helper verde produz exatamente esse estado. A exceção é executável e não está condicionada a ambiente de teste; utilização desse caminho em produção: **[nao verificado]**. (`app/folha/porta_export.py:768`, `:875–887`, `:1238–1257`, `:820–822`.)

Também não se exige tentativa **preenchida**: pedido CONCLUIDO e carimbo com tentativa ausente passam pela comparação `None == None`. Existência desses registros em produção: **[nao verificado]**. (`app/folha/porta_export.py:1247–1248`.)

**FALHOU de hoje com verde antigo não libera:** FALHOU prevalece antes da inspeção do escalar. CONCLUIDO com carimbo de outra tentativa também falha. (`app/folha/porta_export.py:1245–1249`.)

**No caminho do command, a tentativa atrasada é barrada:** adoção exige PEDIDO com o UUID recebido; `encerrar` usa CAS por tentativa; `concluir` trava e filtra a linha pela tentativa EM_CURSO antes de carimbar. (`app/folha/porta_export.py:1112–1116`, `:1056–1058`, `:1160–1168`, `:1178–1182`.)

Essa proteção pertence a `concluir`; uma chamada direta a `lavrar` continua sem verificar posse ou estado. Os chamadores diretos encontrados incluem o helper e testes. (`app/folha/porta_export.py:768–807`, `:885–887`; `app/folha/tests/test_porta_sem_beco.py:157–162`.)

**3. Pedido preso, morte e R7**

O vigia consulta a trava correta de dois inteiros: banco, `classid`, `objid`, `objsubid=2`, `granted` e o `pid` gravado do backend. Não basta outra sessão segurar a trava. (`app/folha/porta_export.py:995–1001`.)

Entretanto, a pergunta respondida é **“essa sessão segura essa trava?”**. Perder a sessão enquanto o processo Python continua vivo gera FALHOU; continuar segurando a trava enquanto o processo fica travado conserva EM_CURSO, sem prazo máximo nessa fase. (`app/folha/porta_export.py:1193–1205`.)

**Reuso de PID não é distinguido pela identidade da sessão:** não há `backend_start` nem outro identificador de encarnação no registro/consulta. Um backend reutilizado com o mesmo PID e a mesma trava satisfaria o predicado; ocorrência real dessa combinação: **[nao verificado]**. (`app/folha/porta_export.py:997–1000`, `:1108–1115`, `:1124–1126`.)

**R7 ganhou observador sem tela:** o command varre todos os pedidos abertos do schema, inclusive de dias anteriores, antes de selecionar os alvos. Isso fecha a dependência exclusiva da leitura humana. (`app/folha/porta_export.py:1209–1221`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63–66`.)

**Não garante observação independente da execução seguinte:** o selo está no fim da corrente; o encadeador pode não executá-lo quando faltar o carimbo do predecessor. Uma morte depois da varredura inicial espera outra chamada. Portanto, basta para observação eventual com a corrente funcionando; prazo garantido e funcionamento instalado em produção: **[nao verificado]**. (`app/config/crons.py:1793–1805`; `bin/encadeado.sh:81–95`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`.)

FALHOU também não é histórico imutável no registro: `pedir` e `assumir` reutilizam a linha para outra tentativa. A memória diária da falha depende da recusa emitida antes dessa substituição. (`app/folha/porta_export.py:1085–1090`, `:1118–1126`, `:1056–1060`.)

**4. Uma recusa por tentativa e contador**

**Zero linhas é possível por interrupção:** `_falhar` persiste primeiro e registra depois; `concluir` termina a transação antes de registrar. Morte nesse intervalo deixa a tentativa terminal fora da varredura dos abertos, sem mecanismo mostrado de recuperação da recusa. (`app/folha/porta_export.py:1056–1060`, `:1162–1174`, `:1216–1217`.)

**A corrida normal entre vigia, encerrar e concluir não demonstra duplicação da linha `porta_export`:** o CAS só deixa o vencedor de `_falhar` emitir, e `concluir` filtra/trava a mesma tentativa. (`app/folha/porta_export.py:1034–1042`, `:1056–1060`, `:1163–1174`.)

**Mas há 2+ recusas críticas associadas ao fluxo da tentativa:** além da emissão do término sob `porta_export`, cada POST recusado passa pelo envelope sem `recusas=()`, que registra outra linha sob a porta da rota. O contador do export exclui essas linhas; isso evita inflação daquele N, mas não cumpre literalmente ausência de reemissão pelo clique. (`app/folha/porta_export.py:1050–1053`; `app/folha/views.py:130–135`; `app/core/respostas.py:45`, `:50–56`; `app/core/recusa_critica.py:119–121`.)

**O contador usa o leitor único e está integrado ao ESTADO**, com esperado zero e distinção explícita entre arquivo ausente e zero. (`app/core/recusa_critica.py:133–139`; `bin/recusa_estado.py:60–77`; `bin/gerar_estado.py:267–270`.)

**O dia contado é o prefixo temporal da linha**, não o campo `dia=` — este recebe o dia da tentativa. Assim, uma falha observada hoje para pedido de ontem conta pela data de emissão do log. (`app/folha/porta_export.py:1050`; `app/core/recusa_critica.py:46–50`, `:113–121`.)

A equivalência com o dia civil da aplicação continua **[nao verificado]**: a aplicação declara São Paulo; o ESTADO usa `datetime.date.today()` do host, e o formatter usa `asctime` sem conversão explícita de fuso nesses pontos. (`app/config/settings/base.py:93–95`; `bin/recusa_estado.py:64`; `app/config/settings/saas.py:122`.)

Há outra limitação de ausência de sinal: com o arquivo existente, erros de abertura são ignorados pelo leitor; isso pode terminar em zero, sem a distinção `sem_arquivo` feita anteriormente pelo wrapper. (`app/core/recusa_critica.py:95–105`, `:114–123`; `bin/recusa_estado.py:65–68`.)

**5. R8 e rc**

**Exceção em `medir` e retorno classificado como falhou deixam as seguintes medirem**, desde que persistência e emissão da falha terminem normalmente: ambos os ramos acrescentam a empresa a `falharam` e fazem `continue`. O fechamento retorna rc 2 quando houve essas falhas ou divergências acumuladas. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:147–165`, `:251–259`.)

O isolamento não cobre qualquer exceção da empresa: `assumir`, `concluir` e a persistência/emissão feita por `encerrar` ficam fora da proteção de `medir`. Uma falha nesses pontos pode abortar as seguintes. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140–154`.)

**O rc não fica vermelho para toda falha descoberta na rodada.** Exemplo estático: `vigiar_abertos` marca FALHOU um PEDIDO expirado; o job com aquele `--tentativa` não consegue adotá-lo, entra em `compartilhadas` e termina pelo retorno normal. Essa falha não entra em `falharam`. (`app/folha/porta_export.py:1200–1205`, `:1112–1116`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `:128`, `:140–146`, `:258–265`.)

A parcela global `cartao_x_txt` foi preservada: continua medida para a competência inteira e integrada às falhas de cada empresa. R8 não a tornou artificialmente empresarial. (`app/folha/porta_export.py:667`, `:723–727`, `:753–764`.)

**6. Classes de reprovação e força dos testes** [nao verificado]

- **Clone de juiz registrado: não demonstrado.** A medição continua chamando `classificar_export`, autoridade registrada; `resultado_de` valida/classifica o retorno da medição. Existe, porém, classificação paralela do resultado na leitura: o registro guarda `resultado/n`, enquanto a porta volta a interpretar o escalar do carimbo. (`app/core/juizes.py:329–331`; `app/folha/porta_export.py:469`, `:1136–1147`, `:1171`, `:1252–1257`.)
- **Fallback presente:** carimbo sozinho responde quando falta pedido. **Segundo escritor operacional do carimbo: não identificado**; `concluir` passa por `lavrar`, e os escritores diretos externos apontados são fixtures. (`app/folha/porta_export.py:1168`, `:1235–1257`; `app/folha/tests/test_porta_do_export.py:33–42`.)
- **Relógio não é capturado uma vez para a leitura inteira:** pedido e carimbo consultam `_hoje()` separadamente. A gravação pelo job adotado conserva o dia do pedido, mas a leitura ainda pode atravessar a meia-noite entre as duas consultas. (`app/folha/porta_export.py:1004–1007`, `:1112–1117`, `:1168`, `:1238–1240`.)
- **Trilha incompleta permanece:** a recusa do término recebe `request=None`, produzindo usuário `-`, e não contém antes/depois. A auditoria do disparo tem ator/PID, mas não equivale à trilha das transições terminais exigida na rev2.r. (`app/folha/porta_export.py:1050–1053`, `:1311–1314`; `app/core/recusa_critica.py:77–85`; `CLAUDE.md:32–34`.)
- **Ordem de trava no caminho normal:** advisory lock → assumir/medir → trava da linha → carimbo/estado. Inversão concorrente demonstrada: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129–154`; `app/folha/porta_export.py:1162–1171`.)

Os testes novos **têm mordida**, mas não provam tudo que seus nomes sugerem. (`app/folha/tests/test_porta_sem_beco_item2.py:42–57`, `:69–134`, `:168–190`.)

| Cobertura | O que efetivamente prova / o que falta |
|---|---|
| Identidade e atraso | Exercita persistência, CAS e carimbo com tentativa; as sucessões são sequenciais, sem corrida entre processos nesses testes. (`app/folha/tests/test_porta_sem_beco_item2.py:60–104`.) |
| R7 | Exercita `pg_locks` com backend gravado artificialmente inexistente e varredura de pedido de ontem; não mata um processo nem executa o encadeador. (`app/folha/tests/test_porta_sem_beco_item2.py:47–57`, `:137–146`.) |
| R8 | Mocka `medir`, mas usa o command e a persistência reais para os estados das duas empresas. É cobertura válida do isolamento dessas respostas; não prova recuperação de indisponibilidade real do banco. (`app/folha/tests/test_porta_sem_beco_item2.py:158–190`.) |
| Re-clique | O teste chamado “nenhuma por clique repetido” chama `conferir` três vezes, sem POST; o filtro ainda descarta linhas de outras portas. Esconde precisamente a costura do envelope. (`app/folha/tests/test_porta_sem_beco_item2.py:29–30`, `:214–223`.) |
| Meia-noite | Confere vínculo e mesmo `data_ref` na gravação; não confere uma leitura da porta atravessando a virada. (`app/folha/tests/test_porta_sem_beco_item2.py:249–268`.) |
| Contador | O teste novo conta linhas sintéticas já formatadas; apesar do nome, não exercita ausência de arquivo. O teste shell verifica ausência para o leitor anterior, sem chamar `ler_porta_export` nessa situação. (`app/folha/tests/test_porta_sem_beco_item2.py:273–289`; `bin/tests/test_recusa_estado.sh:29–43`.) |

O selo de soma em `relatorios` mocka conclusão, vigia, trava e adoção; serve para a agregação, sem comprovar essas costuras do item 2. Evidência de RED anterior e execução verde dos testes: **[nao verificado]**. (`app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:126–136`.)

**7. Quais pontos (ii) da rev2.r continuam abertos**

| Ponto da rev2.r | Situação nesta cópia |
|---|---|
| Posse da tentativa e conclusão atrasada | **Fechado no caminho normal**, pelo UUID/CAS e conclusão transacional; permanece a liberação excepcional sem pedido/tentativa preenchida. (`app/folha/porta_export.py:1056–1058`, `:1163–1171`, `:1247–1257`.) |
| PID gravado e conferido | **Parcial:** ambos gravados ao assumir; somente backend conferido; PID do processo ainda não é persistido no PEDIDO ao disparar. (`app/folha/porta_export.py:1073–1079`, `:1115`, `:1194`, `:1314`.) |
| Observador sem tela | **Implementado na declaração/caminho do cron**, condicionado à próxima execução do elo. Garantia operacional de periodicidade: **[nao verificado]**. (`app/config/crons.py:1803–1805`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`; `bin/encadeado.sh:84–95`.) |
| Resultado empresarial vinculado | **Persistência fechada; leitura parcial:** resultado/N estão no job, mas a porta continua classificando o carimbo. (`app/folha/porta_export.py:1170–1171`, `:1241–1257`.) |
| FALHOU × verde antigo | **Fechado.** (`app/folha/porta_export.py:1245–1249`.) |
| Dia e meia-noite | **Gravação vinculada fechada; leitura sem referência única permanece.** (`app/folha/porta_export.py:1168`, `:1007`, `:1238–1240`.) |
| Textos e lista | **Fechado no código:** literal de FALHA e fecho de DIVERGENTE, conservando a lista. (`app/folha/porta_export.py:828–872`, `:922–929`.) |
| Motivos, recusa única e trilha | **Ainda aberto:** motivo foi persistido, mas faltam garantia da emissão após interrupção, supressão da reemissão pelo POST e trilha terminal com ator/antes/depois. (`app/folha/porta_export.py:790`, `:1050–1060`, `:1170–1174`; `app/folha/views.py:135`; `app/core/respostas.py:54–56`; `CLAUDE.md:32–34`.) |
| Contador | **Integração fechada; dia operacional e completude do sinal não demonstrados integralmente.** (`bin/gerar_estado.py:267–270`; `bin/recusa_estado.py:64–68`; `app/core/recusa_critica.py:95–105`, `:117–121`.) |
| R8 | **Fechado para exceção de `medir` e resultado falhou com registro funcionando.** Não cobre falha na conclusão/persistência, nem torna vermelho o rc por falha descoberta apenas na varredura inicial. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `:140–165`, `:251–265`.) |

## 17/11 12:39 · sessao 5a018d6a0313 · E4C-F1-B2 · verificador · origem=Code

**A cerca ainda permite contorno e também recusa atos que não tocariam perguntas entregues.** Os dois problemas aparecem nos caminhos abaixo. (`app/ponto/management/commands/reconciliar_fantasmas.py:288`; `app/chamados/services/disputa_emissao.py:1891`)

Execução da suíte, RED contra o objeto Git `682ccefa4` e reprodução dinâmica dos contraexemplos: **[não verificado]**.

1. **A enumeração não coincide com a escrita efetiva.**

   Contraexemplo: A vivo; disputa D já fechada, cujo pai é A; pergunta Q entregue, materializada, não validada, **sem destino**, morta pelo C6. `_ancoradas()` inclui Q pelo pai, e a cerca a enumera. (`app/chamados/models.py:344`; `app/chamados/services/disputa_emissao.py:1942`)

   Nesse estado, Q não entra no direto, que exige destino A, materialização nula e disputa aberta; não provoca reabertura, pois está morta; D fica fora da varredura de abertas; a relavra S2 exige destino A. Portanto a enumeração inclui Q embora esses escritores não a escrevam. (`app/chamados/services/disputa_emissao.py:1830`; `app/chamados/services/disputa_emissao.py:1968`; `app/chamados/services/disputa_emissao.py:1839`; `app/chamados/models.py:309`)

   Mesmo assim, o dia entregue de Q produz veto e recusa A. Há outro sobrealcance, específico da previsão do fecho, no item 6. (`app/chamados/reconciliador.py:530`; `app/chamados/reconciliador.py:538`; `app/chamados/reconciliador.py:576`)

2. **A cerca não antecede a primeira escrita em todos os caminhos.**

   | Entrada | Constatação |
   |---|---|
   | `retratar` | Quando chega à consulta, o veto antecede a transição, o Fix A, o resolver explícito e a ocorrência. Porém a retenção anterior pode escrever trilha e retornar sem consultar competência. (`app/chamados/reconciliador.py:564`; `app/chamados/reconciliador.py:576`; `app/chamados/reconciliador.py:580`; `app/chamados/reconciliador.py:592`; `app/chamados/reconciliador.py:593`) |
   | `reconciliar_geofence` | Delega a `retratar`; separa retorno verdadeiro de falso e só registra sucesso quando verdadeiro. (`app/ponto/services/geofence.py:444`; `app/ponto/services/geofence.py:448`; `app/ponto/services/geofence.py:449`) |
   | `reconciliar_fantasmas` | Interrompe somente pelo motivo `competencia_entregue`. Uma recusa por `retencao` continua no resolver direto. (`app/ponto/management/commands/reconciliar_fantasmas.py:284`; `app/ponto/management/commands/reconciliar_fantasmas.py:288`) |
   | Ramo HIST, antigo ponto `1685` | A cópia chama primeiro `reapontar_resolvedora` em `1700`, que grava destino e lavra; depois chama o resolver em `1706`. A guarda da costura permite o destino terminal quando a competência é entregue. (`app/chamados/reconciliador.py:1700`; `app/chamados/services/fio.py:824`; `app/chamados/services/fio.py:973`; `app/chamados/reconciliador.py:1706`) |
   | `becos.py:55` | Chama diretamente o resolver depois das travas, sem consultar a cerca da retratação. O wrapper do resolver também só trata a via de tranca e adquire travas. (`app/chamados/services/becos.py:52`; `app/chamados/services/becos.py:55`; `app/chamados/services/disputa_emissao.py:2021`; `app/chamados/services/disputa_emissao.py:2027`) |
   | S143 | Pode chamar o resolver após qualquer save elegível de encerramento; sua entrada não exige passagem por `retratar_com_motivo`. (`app/chamados/signals.py:104`; `app/chamados/signals.py:112`; `app/chamados/signals.py:124`) |

   **Contorno concreto por retenção:** considere A elegível em fantasmas, com resposta completa sem veredito creditada a ele; e outra disputa aberta do mesmo colaborador, sem credor vivo, contendo Q entregue, morta, não validada nem materializada. A resposta retém A antes da cerca; o comando recebe `'retencao'` e chama o resolver. (`app/chamados/juizes.py:2115`; `app/chamados/reconciliador.py:449`; `app/chamados/reconciliador.py:564`; `app/ponto/management/commands/reconciliar_fantasmas.py:288`)

   O resolver varre todas as disputas abertas do colaborador, seleciona Q, passa pela guarda de vitalidade e a carimba. Esse caminho escreve a pergunta entregue sem consultar o veto; `recusados_competencia_entregue` permanece zero porque só incrementa na recusa explícita da cerca. (`app/chamados/services/disputa_emissao.py:2168`; `app/chamados/services/disputa_emissao.py:1867`; `app/chamados/services/disputa_emissao.py:2182`; `app/chamados/services/disputa_emissao.py:2194`; `app/ponto/management/commands/reconciliar_fantasmas.py:285`)

3. **Há classes reprovadoras presentes.**

   - **Derivação paralela do fecho/alcance:** `_disputa_fecha_no_ato` chama C7 sobre uma lista reduzida, descartando perguntas apenas potencialmente escritas. O escritor consulta C7 sobre o estado real restante. Usar o mesmo juiz com universos diferentes não estabelece equivalência; o contraexemplo do item 6 mostra a divergência. (`app/chamados/services/disputa_emissao.py:1890`; `app/chamados/services/disputa_emissao.py:1892`; `app/chamados/services/disputa_emissao.py:2225`; `app/chamados/models.py:1943`)
   - **Defesa 1:** não houve duplicação literal do predicado extraído: receptor e enumerador usam `defesa_1_reconcilia`. Entretanto, a previsão ignora o gate do pai, enquanto o receptor respeita sua recusa; isso amplia indevidamente o conjunto previsto como encerrado. (`app/chamados/signals.py:23`; `app/chamados/signals.py:41`; `app/chamados/services/disputa_emissao.py:1899`; `app/chamados/services/disputa_emissao.py:1909`; `app/chamados/signals.py:50`)
   - **Escritor sem cerca:** continuam existindo entradas diretas ao resolver compartilhado; o problema demonstrado é ausência da proteção nessas entradas, embora o carimbo ainda passe pela porta `carimbar`. (`app/chamados/services/becos.py:55`; `app/chamados/services/disputa_emissao.py:2028`; `app/chamados/services/disputa_emissao.py:2194`)
   - **Relógio:** a nova previsão consulta C7 sem fornecer um instante comum; C6 pode chegar a `date.today()`. Divergência efetiva numa virada temporal: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1892`; `app/chamados/models.py:1944`; `app/chamados/juizes.py:596`)
   - **Ordem de trava:** no caminho protegido, a cerca usa a porta global antes da transição; essa porta adquire Colaborador → Disputa → Chamado → Pergunta. Inversão nova nesse trecho: **[não verificado]**. (`app/chamados/reconciliador.py:575`; `app/chamados/models.py:2781`; `app/chamados/models.py:2790`; `app/chamados/models.py:2803`; `app/chamados/models.py:2807`)
   - **Valor com descrição incompleta:** `relatorio['recusados']` recebe qualquer retorno falso, incluindo `'transicao'`, embora a saída diga apenas “retenção ou competência ENTREGUE”. (`app/ponto/services/geofence.py:448`; `app/chamados/reconciliador.py:543`; `app/chamados/reconciliador.py:581`; `app/ponto/management/commands/reconciliar_geofence.py:46`)

4. **Escritores e leitores dos valores alterados e dos principais campos usados na composição.**

   | Campo/valor | Quem escreve/produz | Quem lê |
   |---|---|---|
   | `(bool, motivo)`: `'retencao'`, `'competencia_entregue'`, `'transicao'`, `None` | `retratar_com_motivo`, nos quatro retornos. (`app/chamados/reconciliador.py:567`; `:579`; `:581`; `:598`) | `_retratar` descarta o motivo; fantasmas o recebe e distingue somente competência. (`app/chamados/reconciliador.py:543`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `:284`) |
   | Lista de empresas vetadas | Agrupamento por empresa e composição do veto. (`app/chamados/reconciliador.py:533`; `:538`) | Unidade de retratação e escritor da retenção. (`app/chamados/reconciliador.py:576`; `:578`; `:477`) |
   | Trilha `retratacao_retida_competencia_entregue`; `empresas_vetadas`; ação/modelo/objeto/descrição | Payload em `registrar_retencao`; persistência por `evento`. (`app/chamados/reconciliador.py:477`; `:491`; `app/core/observ.py:134`) | Deduplicação lê o último `valor_depois`; teste conta a trilha. (`app/chamados/reconciliador.py:480`; `:487`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:174`) |
   | `recusa_entregue` / `recusados_competencia_entregue` | Inicialização, incremento e publicação no comando. (`app/ponto/management/commands/reconciliar_fantasmas.py:169`; `:285`; `:321`) | Asserção do caso f. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:377`) |
   | `relatorio['retratados'/'recusados']` | Serviço geofence acrescenta o PK conforme `ok`. (`app/ponto/services/geofence.py:448`) | Comando conta cada lista separadamente. (`app/ponto/management/commands/reconciliar_geofence.py:47`) |
   | `perdeu`, `acusam`, total de ocorrências | Reavaliação geofence calcula e acrescenta os chamados sem premissa. (`app/ponto/services/geofence.py:433`; `:437`) | Aplicação e apresentação do comando; continuam representando perda de premissa. (`app/ponto/services/geofence.py:443`; `app/ponto/management/commands/reconciliar_geofence.py:35`) |
   | `Chamado.status_local` | `_gravar_estado` atribui e salva. (`app/chamados/models.py:228`; `:296`) | Seleção HX e predicado da Defesa 1. (`app/chamados/services/disputa_emissao.py:1860`; `app/chamados/signals.py:23`) |
   | `Disputa.fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | Porta `fechar`. (`app/chamados/models.py:2023`; `:2027`) | Seleção HX e receptores do fecho. (`app/chamados/services/disputa_emissao.py:1856`; `app/chamados/signals.py:36`; `:172`) |
   | `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, persistido por `carimbar`; S128, HX e S144 chamam essa porta. (`app/chamados/models.py:3220`; `:3273`; `app/chamados/services/disputa_emissao.py:2129`; `:2194`; `app/chamados/services/materializacao.py:2167`) | Seleções direta/HX/FECHO e C6. (`app/chamados/services/disputa_emissao.py:1832`; `:1845`; `:1948`; `app/chamados/juizes.py:1404`) |
   | `respondida_em`, `resposta_colab`, `validada_em`, `validada_por` | `carimbar` pode completar instante/validação quando há texto; validação humana também grava seu par. (`app/chamados/models.py:3251`; `app/chamados/services/validacao.py:168`) | C21, C6 e seleções HX/âncoras. (`app/chamados/juizes.py:1552`; `:1405`; `app/chamados/services/disputa_emissao.py:1845`; `app/chamados/models.py:344`) |
   | `data_evento`, `conversa` | Dia atribuído por `gravar_dia_do_fato`; nascimento informa conversa. (`app/chamados/services/fio.py:848`; `app/chamados/models.py:3134`) | Juiz do dia distingue conversa; cerca exclui conversa e consulta esse juiz. (`app/chamados/juizes.py:342`; `app/chamados/reconciliador.py:531`; `:534`) |
   | `chamado_destino`, vínculo à disputa/célula | Nascimento da pergunta; costura pode trocar o destino. (`app/chamados/models.py:3134`; `app/chamados/services/fio.py:973`) | Direto, HX, previsão de fecho e âncoras. (`app/chamados/services/disputa_emissao.py:1831`; `:1865`; `:1891`; `app/chamados/models.py:345`) |
   | `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`; S2 chama após mudança de estado, e a costura salva a mesma lavra. (`app/chamados/veredito_lavrado.py:270`; `app/chamados/models.py:312`; `app/chamados/services/fio.py:975`) | Fotografia dos testes observa os campos persistidos da pergunta. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:164`) |
   | Justificativa `status`/`descricao`; ocorrência da retratação | Fix A rejeita justificativa pendente; a ocorrência passa por `nota.anexar_ocorrencia`. (`app/chamados/reconciliador.py:588`; `:592`; `:595`) | Retenção lê o status; teste de controle confere a rejeição. (`app/chamados/reconciliador.py:454`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:300`) |

   Censo global exaustivo de todos os demais escritores desses campos fora dos caminhos examinados: **[não verificado]**.

5. **Os testes têm mordida parcial; não demonstram uniformemente o motivo anunciado.** [nao verificado]

   - **a/b/n:** `_pergunta` verifica C6 somente na instância recém-criada. O resolver recarrega perguntas e pula as vivas. Se a pergunta está morta apenas em memória, isso não invalida testar a recusa preventiva, mas invalida usar o caso como prova de que aquele S128/HX carimbaria a pergunta; a fotografia pode falhar só pela alteração do chamado ou justificativa. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:135`; `:162`; `:196`; `app/chamados/services/disputa_emissao.py:2083`; `:2109`; `:2182`)
   - **b2:** a discrepância não elimina o caminho da relavra: S2 busca destino igual ao chamado e validação nula, sem exigir pergunta morta. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:272`; `app/chamados/models.py:309`)
   - **c:** é controle positivo, esperado verde, não RED. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:288`)
   - **h1/h2:** exercitam os pulos por pai/destino vivo, mas não cobrem a pergunta viva removida da projeção HX no contraexemplo do item 6. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:302`; `:311`; `app/chamados/services/disputa_emissao.py:1890`)
   - **k/s:** exigem igualdade da própria pergunta entregue antes da fotografia geral, uma verificação mais específica. O caso **s**, porém, grava texto vazio com `respondida_em` preenchido: testa uma validada com par ilegal legado, e não uma resposta completa segundo C21. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:205`; `:334`; `:347`; `:357`; `app/chamados/juizes.py:1552`)
   - **f:** corrige a célula no banco e verifica C6 por objeto fresco; **g** verifica especificamente a contagem de sucesso. O caso f não exercita o contorno por `'retencao'` demonstrado no item 2. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:371`; `:372`; `:393`; `app/ponto/management/commands/reconciliar_fantasmas.py:284`)
   - **Anti-vacuidade:** o harness aceita mensagens amplas de `AssertionError`; portanto “morde” não significa necessariamente escrita da pergunta-alvo. As novas allowlists também dispensam a fiscalização dessas funções, sem provar equivalência com os escritores. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:455`; `app/chamados/tests/test_contract_leitores_vitalidade.py:103`; `:356`)

   Pelo diff fornecido, a cerca já existia na base da volta; a/b/b2/n não constituem novos REDs dessa volta pela ausência da cerca. A nova exigência nesses casos inclui trilha/deduplicação. Execução contra a base e causa exata de cada falha: **[não verificado]**. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:203`; `:257`)

6. **Sim: a superaproximação pode recusar retratação legítima em competência não entregue.**

   Caminho específico pelo **destino que encerra**:

   - A é cobrança duplicada de dia aberto; S é o sobrevivente válido do mesmo dia. Q viva aponta para A numa disputa dB cujo pai é B. O gate permite fechar A por dedup, reatando Q para S antes de gravar o encerramento. (`app/chamados/models.py:795`; `app/chamados/models.py:389`; `app/chamados/models.py:435`; `app/chamados/models.py:655`)
   - No ato real, Q continua viva com destino S vivo: HX pula a disputa e C7 impede seu fecho; portanto a Defesa 1 não encerra B por esse caminho. (`app/chamados/juizes.py:1456`; `app/chamados/services/disputa_emissao.py:1865`; `app/chamados/models.py:1944`; `app/chamados/services/disputa_emissao.py:2225`)
   - A previsão remove Q pelo destino **antigo A**, considera dB fechável e acrescenta B à cascata. Uma pergunta R entregue, de destino B, em disputa fechada, entra então no alcance previsto e veta A — embora B não fosse encerrado nem R relavrada pelo ato real. (`app/chamados/services/disputa_emissao.py:1891`; `:1911`; `:1938`; `:1942`; `app/chamados/reconciliador.py:538`)

   Há também a variante HX: pergunta Q viva de escolha em dia aberto, sem credor vivo, e irmã R validada não materializada em dia entregue. A previsão remove Q como candidata HX e prevê fecho; o escritor pula Q viva e não fecha. R é acrescentada indevidamente pelo FECHO. (`app/chamados/services/disputa_emissao.py:1845`; `:1890`; `:1948`; `:2182`; `app/chamados/juizes.py:1405`; `app/chamados/models.py:1944`)

   A recusa nesses exemplos depende de uma pergunta entregue indevidamente incluída. Se **todos** os dias enumerados forem conhecidos e não entregues, o veto retorna falso. (`app/chamados/services/fio.py:1365`)

VEREDITO: REPROVA -- achados: contorno por retenção (`app/ponto/management/commands/reconciliar_fantasmas.py:288`); resolver sem cerca (`app/chamados/services/disputa_emissao.py:2028`); sobrealcance (`app/chamados/services/disputa_emissao.py:1891`); mordida inespecífica (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:455`).

## 17/11 12:36 · sessao 09fa193ca3fa · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Enumeração e escrita efetiva diferem por sobrealcance.** `_ancoradas()` inclui pergunta sem destino ancorada pelo pai, mesmo materializada e em disputa fechada (`app/chamados/models.py:336`, `app/chamados/models.py:344`). O enumerador incorpora esse universo inteiro (`app/chamados/services/disputa_emissao.py:1942`). Contraexemplo: A vivo, d fechada com pai A, q entregue, materializada, não validada, sem destino e morta pelo C6. O resolver não a carimba diretamente, pois exige destino A, materialização nula e disputa aberta (`app/chamados/services/disputa_emissao.py:1830`); a tentativa de reabertura termina sem ato porque q está morta (`app/chamados/services/disputa_emissao.py:1968`); d fechada fica fora da varredura aberta (`app/chamados/services/disputa_emissao.py:1839`); a relavra S2 exige destino A (`app/chamados/models.py:309`). Mesmo assim, q veta A (`app/chamados/reconciliador.py:530`, `app/chamados/reconciliador.py:538`). Reprodução executada: [não verificado].

2. **A projeção do fecho também produz falso positivo.** dX aberta sem pai, q de escolha viva, não validada e sem destino em competência aberta; r validada, não materializada, sem destino em competência entregue. HX seleciona somente q (`app/chamados/services/disputa_emissao.py:1845`, `app/chamados/services/disputa_emissao.py:1867`). O resolver real pula q viva (`app/chamados/services/disputa_emissao.py:2182`) e C7 impede fechar dX (`app/chamados/models.py:1944`). A preditora remove q como se HX a tivesse escrito (`app/chamados/services/disputa_emissao.py:1890`); r validada está morta pelo C6 (`app/chamados/juizes.py:1405`), logo prevê fecho e incorpora r (`app/chamados/services/disputa_emissao.py:1947`). A acaba recusada embora r não fosse tocada (`app/chamados/reconciliador.py:576`). Reprodução executada: [não verificado].

3. **Variante pedida em Q6, pelo destino encerrando.** A é cobrança duplicada em competência aberta, sustentada por sobrevivente S do mesmo dia; q viva aponta inicialmente para A em dB com pai B. O gate permite dedup apoiado em S (`app/chamados/models.py:795`), planeja e aplica reaponte de q para S (`app/chamados/models.py:389`, `app/chamados/models.py:435`) antes de gravar A (`app/chamados/models.py:689`, `app/chamados/models.py:655`). Portanto, q permanece viva, destino S vivo impede HX e C7 segura dB (`app/chamados/juizes.py:1456`, `app/chamados/services/disputa_emissao.py:1865`, `app/chamados/models.py:1944`). A preditora, porém, elimina q pelo destino **antigo** A, presente em `encerrando` (`app/chamados/services/disputa_emissao.py:1891`), pode prever fecho de dB e incluir B na cascata (`app/chamados/services/disputa_emissao.py:1911`). Uma pergunta entregue r de destino B em disputa fechada passa a entrar no alcance projetado, embora B não fosse efetivamente encerrado (`app/chamados/services/disputa_emissao.py:1938`, `app/chamados/services/disputa_emissao.py:1942`). Assim pode recusar a retratação legítima aberta de A (`app/chamados/reconciliador.py:538`). Reprodução executada: [não verificado].

4. **Defesa 1 não ganhou cópia literal do seu predicado de seleção:** ambos usam `defesa_1_reconcilia` (`app/chamados/signals.py:19`, `app/chamados/signals.py:41`, `app/chamados/services/disputa_emissao.py:1909`). Entretanto, a predição ignora deliberadamente o gate real de reconciliação do pai (`app/chamados/services/disputa_emissao.py:1899`), enquanto o receiver respeita sua recusa (`app/chamados/signals.py:50`). Isso acrescenta sobreaproximação ao alcance, além da diferença no universo fornecido a C7 (`app/chamados/services/disputa_emissao.py:1891`, `app/chamados/models.py:1943`).

5. Ressuscitação pode reabrir disputas inicialmente fechadas (`app/chamados/models.py:1648`), enquanto `_disputas_do_resolver` parte das abertas e do direto anterior (`app/chamados/services/disputa_emissao.py:1873`). Um subalcance concreto decorrente dessa mutação não foi fechado nesta leitura: [não verificado].

A recusa de uma retratação aberta nos contraexemplos depende de uma pergunta entregue acrescentada indevidamente ao alcance; se todos os dias enumerados forem conhecidos e não entregues, o veto retorna falso (`app/chamados/services/fio.py:1365`).

**Conclusão do subexame: REPROVA por sobrealcance demonstrável no código; equivalência entre enumeração e escrita efetiva não se sustenta** (`app/chamados/services/disputa_emissao.py:1891`, `app/chamados/services/disputa_emissao.py:1942`, `app/chamados/services/disputa_emissao.py:2182`).

## 17/11 12:36 · sessao 433d50b4830a · - · - · origem=Ronald (tmux)

A validação dinâmica dos testes e do RED na base é **[não verificado]**; a leitura do código permite separar os seguintes pontos. [nao verificado]

| Casos | O que o teste demonstra / limite | [nao verificado]
|---|---|
| **a, b, n** | `_pergunta` consulta C6 somente na instância recém-criada, sem recarregar a célula (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:135-142`). O resolver carrega instâncias novas e pula perguntas VIVAS (`app/chamados/services/disputa_emissao.py:2083-2084`, `:2109-2110`, `:2181-2183`). Assim, a discrepância memória/banco não invalida testar a recusa preventiva, mas invalida afirmar que esses cenários provam o carimbo descrito; a fotografia pode falhar apenas pela alteração do chamado ou da justificativa (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:162-169`, `:196-203`). |
| **b2** | A discrepância não elimina a prova da relavra: `_gravar_estado` busca perguntas de destino igual ao chamado, não validadas, e as relavra ao cruzar ENCERRADOS, sem exigir pergunta MORTA (`app/chamados/models.py:303-311`). O teste coloca a pergunta materializada e a disputa fechada, mantendo-a não validada (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:267-278`). |
| **c** | É controle positivo e deve permanecer verde; não constitui RED da base (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:288-300`). |
| **h1, h2** | Exercitam o sobrealcance: pergunta entregue VIVA de disputa cujo pai ou destino de irmã permanece vivo (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:302-318`). A precondição de vitalidade usa a instância construída com célula explicitamente atualizada para `furo` (`:224-234`). |
| **k** | O teste exige que a própria pergunta entregue permaneça idêntica, além da fotografia geral; portanto o mutante não pode ser considerado morto somente pela alteração do chamado (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:205-211`, `:320-334`). |
| **s** | Também exige identidade da própria pergunta entregue e consulta C6 por `GET` fresco após corrigir a célula (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:347-357`). Porém não é “RESPONDIDA E VALIDADA” como anunciado: grava `resposta_colab=''` com `respondida_em` preenchido (`:347-348`), e C21 exige instante **e texto** (`app/chamados/juizes.py:1544-1552`). Prova S144 sobre pergunta validada com par ilegal legado. |
| **f** | Corrige o banco e confirma C6 com objeto recarregado antes de executar o comando; isso remove o problema memória/banco para demonstrar o fallback (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:366-377`). |
| **g** | Verifica que perder a premissa não equivale a reconciliar: exige zero reconciliados e uma recusa (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:379-394`). |

**Mordida incompleta:** o harness aceita qualquer `AssertionError` que contenha uma das mensagens amplas permitidas; alterar apenas chamado/justificativa já pode produzir “morde”, sem comprovar escrita da pergunta descrita (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:196-203`, `:403-404`, `:445-455`). Apenas **k/s** passam `entregue=p` e verificam primeiro a identidade da pergunta-alvo (`:205-211`, `:334`, `:357`).

**Base da volta:** a cerca já existia no trecho anterior do diff fornecido; logo **a/b/b2/n** não são novos REDs da volta pelo motivo anunciado. A nova exigência observável desses casos inclui a trilha da recusa e sua deduplicação (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:203`, `:245-257`); execução efetiva contra `682ccefa4` **[não verificado]**.

O novo harness de memória incorpora o corpo real do helper HX, sem substituir sua seleção por stub (`app/chamados/tests/test_e4b_classe_e_memoria.py:369-374`). As novas entradas da allowlist, por sua vez, dispensam a fiscalização AST dessas funções; não comprovam equivalência entre enumeração e escrita (`app/chamados/tests/test_contract_leitores_vitalidade.py:103-107`, `:354-358`).

_6569 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
