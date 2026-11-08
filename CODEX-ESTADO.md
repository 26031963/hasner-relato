# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 10:20** · publicado em **08/11/2026 10:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 10:20 · sessao d3bca936a7a8 · - · - · origem=Ronald (tmux)

Censo auxiliar, só leitura:

| Sítio executável | Resposta para parcial não-falta sem horas | Efeito |
|---|---|---|
| `app/ponto/turnos.py:2598` | `cobre_parte_do_dia=True` se tipo aceita horas, registro de um dia, `0<minutos<480`; horas não participam. | Predicado de parcial. |
| `app/ponto/turnos.py:2632` | Retorna `sem_periodo`. | Período declarado. |
| `app/ponto/turnos.py:2675` | `ausencia_explica_o_dia_inteiro=False`. | Cobertura inteira. |
| `app/ponto/turnos.py:2690` | Excluída de dias inteiros; além disso exige efeito `ABONA`. | Folha/calculador, zera pontualidade apenas para inteiro. |
| `app/escala/servico_jornada.py:344` | Parcial não cala esse guarda; continua classificação. Varre todas as ausências, não somente vencedora (`:342`). | `classificar_falta`, furo. |
| `app/ponto/services/fechamento.py:694` | Parcial de `TIPOS_PAGOS` pula abono inteiro; dia conta pelas batidas (`:695`). | Folha. |
| `app/ponto/services/efeito_ausencia.py:48` | Soma minutos pagos quando dentro do período. | Crédito financeiro. |
| `app/ponto/services/efeito_ausencia.py:55` | Parcial paga não justifica nem suprime dia inteiro (`:56`). | Folha/espelho. |
| `app/ponto/services/credito_parcial.py:27` | Minutos pagos viram crédito do próprio dia (`:28`). | Folha/espelho. |
| `app/ponto/services/dia_decidido.py:494` | Parcial abonada conserva minutos no rótulo (`:496`). | Tela. |
| `app/ponto/services/ausencia.py:887` | Permite período somente para parcial; quando este validador recebe hora faltante, rejeita (`:883-884`). | Cadastro. |
| `app/ponto/services/vigia_ausencia_veredito.py:54` | Pula parcial no contador de discordância. | Vigia. |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` | Pula parcial no contador. | Diagnóstico. |
| `app/ponto/views.py:2084` | Entrega `True` ao drawer. | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:296` | Mostra edição de período aprovado; sem início mostra “dia está na lista do DP” (`:299`). | Tela. |
| `app/templates/ponto/partials/painel_ausencia.html:311` | Mostra período na aprovação, com aviso “sem hora [...] lista do DP” (`:313`). | Tela. |
| `app/ponto/views.py:2240` | Marca `a.cobre_parte=True`. | Lista de ausências. |
| `app/templates/ponto/ausencias.html:163` | Exibe minutos da parcial. | Tela. |
| `app/escala/services/leitor_celula.py:416` | Transporta `cobertura_parcial`; transporta literalmente `periodo_parcial` da ata (`:418-419`). | Alimentação dos leitores de ata. |
| `app/chamados/services/declaracao_texto.py:98` | Parcial entra mesmo com `tipo_dia='ausencia'`; conserva marcos `missing` (`:110-111`). | Texto/declarativa de cobrança. |
| `app/escala/services/furos_diarios.py:49` | Parcial entra na conta, inclusive sem período. Conta `missing` (`:54`, `:21`). | `FuroDiario`, métricas/ranking. |
| `app/relatorios/furos.py:195` | Parcial entra; marcos apagados vão ao resultado (`:202-209`). | Raio-X da folha/tela. |
| `app/inteligencia/calculadores.py:50` | Parcial entra; marcos apagados classificam completo/parcial/furo (`:64-70`). | Métrica `preenchimento_espelho`. |
| `app/inteligencia/calculadores.py:149` | Parcial entra; conta previstos e batidos (`:160-161`). | Métrica de preenchimento de marcos. |

Os chamadores indiretos de `dias_com_ausencia_de_dia_inteiro` também excluem a parcial: `app/ponto/services/fechamento.py:687`, `app/ponto/services/efeito_ausencia.py:65`, `app/ponto/turnos.py:2695`, `app/ponto/management/commands/diff_calculador.py:412`, `app/ponto/services/esmeril_espelho.py:164`, `app/relatorios/management/commands/diff_extrato_parcial.py:67`.

`supra_juiz` tem três respostas distintas comprovadas:

- Com fato e turno encerrado, lê `periodo_parcial`, produz `PARCIAL_SEM_PERIODO`, cobra cadastro: `app/ponto/supra_juiz.py:381-389`.
- Com batida sem par, realizado zero, lê o mesmo campo e produz a mesma acusação: `app/ponto/supra_juiz.py:437-442`.
- Sem batida, pode produzir `FURO_SEM_COBRANCA`: a parcial é dia de cobrança (`:304`), a ausência de período não barra o portão (`:313-316`), e o furo é emitido no resultado (`:367-370`). O cartório barra depois o nascimento por `parcial_so_lista`, não pelo classificador (`app/ponto/services/cartorio.py:1706`, `:1723-1725`).
- Com chamado vivo, parcial sem período não satisfaz `_resto_cobrado`, portanto produz `COBRANCA_EM_DIA_COBERTO`: `app/ponto/supra_juiz.py:530-534`.
- `PARCIAL_SEM_PERIODO` deriva veredito `indefinida`: `app/escala/models.py:1615`.

Guardas adicionais da lista do administrador:

| Sítio | Efeito comprovado |
|---|---|
| `app/ponto/services/cartorio.py:398` | Sem período + cobertura parcial retorna `True` incondicionalmente (`:399`). |
| `app/ponto/services/cartorio.py:1723` | Conta na lista e bloqueia emissão do furo do dia (`:1725`). |
| `app/ponto/services/cartorio.py:1775` | Conta na lista e bloqueia nascimento do chamado de furo parcial (`:1777`). |
| `app/ponto/services/cartorio.py:1832` | Incrementa `parcial_na_lista_sem_push`, deduplicado por dia-colaborador (`:1829-1831`). |
| `app/chamados/services/fabrica_da_celula.py:102` | Retorna sem motivos/pergunta/pai/push (`:104`). |
| `app/chamados/services/disputa_emissao.py:871` | `dia_na_lista_do_admin` delega à ata e ao predicado do cartório. |
| `app/chamados/services/disputa_emissao.py:894` | Remove chamados desses dias antes da fábrica de disputas; lista vazia retorna `None` (`:895-896`). |
| `app/chamados/services/pergunta_no_app.py:88` | Retenção retorna `parcial_na_lista_do_admin` (`:89`), antes de consultar A (`:90`). |
| `app/chamados/reconciliador.py:872` | Fora de costura em chamado já vivo (`:865`), barra nascer/renascer (`:875`). |
| `app/ponto/services/cluster_espurio.py:103` | Põe dia em `na_lista`; remove-o de `a_abrir` (`:107`); fechamento continua calculado separadamente (`:108`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154` | Incrementa barra `parcial_lista`, não pergunta (`:155-156`). |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121` | Incrementa `na_lista`, não emite (`:122-123`). |

Registro de juízes:

- Há registro expresso para a pergunta **“a ausência do dia suspende, fecha ou levanta a cobrança viva?”**: `_C11`, `app/core/juizes.py:117`, autoridade `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia`, `app/core/juizes.py:271`.
- C11 não chama A nem B: seleciona ausência vencedora para cobrança (`app/chamados/services/silencio_ausencia.py:65`); aprovada só fecha se `_premissa_morre` (`:93-94`), que exige veredito fora de `VEREDITOS_ACUSAM` (`:40-41`), com exceção `justificativa` (`:34-35`); fechamento efetivo passa por `ch.reconciliar()` (`:101`).
- Há C18 para **“a cobrança do dia cala porque a célula lavrada diz que o dia não é de trabalho?”**, `app/core/juizes.py:149`, autoridade `dia_sem_trabalho`, `:278`.
- O registro de ausência contém a pergunta de cobertura de dias (`app/core/juizes.py:241`) e a pergunta de período (`:249`); não há ali registro de A ou B como composição única para toda cobrança.
- `app/docs/JUIZES.md:59-67` documenta a autoridade `dia_sem_trabalho`, a exceção parcial que cobra resto, e a regra especial de A para `sem_periodo`.

Exemplo adicional relevante: para efeito de folha, ausência inteira não `ABONA` é excluída da dispensa de pontualidade por `app/ponto/turnos.py:2690`, embora `ausencia_explica_o_dia_inteiro` não teste efeito (`:2675`). Isso é diferença entre domínios/composições, não prova isolada de divergência A/B.

## 08/11 10:20 · sessao 4d73f0df7961 · - · - · origem=Ronald (tmux)

Grade e ata:

- `app/escala/utils.py:1209` consulta cobertura `para='folha'`; `:1210-1212` calcula a parcialidade por `cobre_parte_do_dia`. Para não-falta marca `tipo_dia='ausencia'` (`:1241`) e `cobertura_parcial=True` (`:1251`); chama `periodo_da_parcial_no_dia` (`:1255`). Se `sem_periodo`, não entra no ramo de dispensa (`:1265`): os missing permanecem. A limpeza de todas as células só ocorre em `not _parcial` (`:1272-1274`).
- `app/ponto/services/cartorio.py:647` faz `_acesa = isinstance(x, dict) and not x.get('missing')`; grava em `:677-679`. Sem DNA correspondente, o fallback também grava `acesa = not missing` (`:687-690`). Portanto os marcos missing conhecidos permanecem `acesa=False`; não significa que todas as lâmpadas do dia ficam falsas, pois batidas presentes acendem as respectivas lâmpadas (`:618-620`, `:647`).
- Há exceção de lâmpada sem coluna atribuível com fato cego: `acesa=None` (`app/ponto/services/cartorio.py:641-644`). Não transformar a afirmação sobre missing conhecidos numa garantia universal sobre todas as lâmpadas.
- `ata_do_dia` transporta tipo do dia e lâmpadas (`app/ponto/services/cartorio.py:701-705`), conta false em `n_missing` (`:712-713`), escreve `cobertura_parcial=True` (`:732-733`) e `periodo_parcial='sem_periodo'` (`:736-737`).
- `dispensar_por_ausencia` só aceita ausência aprovada SEM `dia['cobertura_parcial']` (`app/ponto/services/cartorio.py:766-769`); parcial sem período sai intacta. Se também houver `dia['justificado']`, existe outra fonte de dispensa (`:768`), que acende e marca `dispensada` (`:770-776`).
- Uso da montagem/dispensa de ata em produção: `app/ponto/services/cartorio.py:1210`, `:1386`, `:1800-1802`; o último lavra via `cel.lavrar_veredito`.
- `app/escala/services/leitor_celula.py:416-419` só transporta os dois carimbos da ata para o dia; não altera sem_periodo nem julga novamente.

Portão comum de emissão:

- `app/ponto/services/cartorio.py:370` define `parcial_so_lista`; sem `cobertura_parcial`, False (`:396-397`); com `periodo_parcial == 'sem_periodo'`, True incondicional (`:398-399`). Para parcial com período, True depende de julgamento anterior e ausência de fato novo (`:400`).
- Cartório calcula uma vez em `app/ponto/services/cartorio.py:1706-1707`; furo sem batida só chama emissor quando `apply_ and not _parcial_so_lista` (`:1725`, chamada `:1740-1742`); furo parcial só emite quando `apply_ and not vivos and not _parcial_so_lista` (`:1777-1780`). Logo não cria chamado de furo para sem_período que tenha os carimbos.
- `app/ponto/services/cartorio.py:1723-1724` e `:1775-1776` chamam `_contar_na_lista`. Este apenas incrementa contador em memória `parcial_na_lista_sem_push` e guarda chave em conjunto (`:1827-1832`): não escreve linha de lista.
- Chamado vivo pode continuar e ter marcos espelhados: `app/ponto/services/cartorio.py:1781-1789`, `:1790-1798`. O bloqueio de nascimento não é um fecho universal dos vivos.

Todos chamadores executáveis de `parcial_so_lista`, fora tests:

| Sítio | Efeito para parcial sem período |
|---|---|
| `app/ponto/services/cartorio.py:1706` | Cala os dois portões de novo chamado, `:1725`, `:1777`. |
| `app/chamados/services/fabrica_da_celula.py:102-104` | `motivos_a_perguntar` retorna `[]`: sem pergunta/pai/push; há guardas anteriores por dia/estado em `:91-100`. |
| `app/chamados/services/disputa_emissao.py:871-873` | `dia_na_lista_do_admin` retorna o predicado; sem dia/célula retorna False (`:864-870`). |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:154-156` | Conta `parcial_lista`, pula nova pergunta do chamado mudo. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:121-123` | Conta `na_lista`, pula emissão retroativa. |
| `app/ponto/services/cluster_espurio.py:102-107` | Exclui dias da lista de `a_abrir`; `reconciliar_colab` pula `abrir` em `:127-130`; `a_fechar` é calculado separadamente (`:108`), não pelo predicado. |

Leitores de `dia_na_lista_do_admin`:

- `app/chamados/services/disputa_emissao.py:894-896`: filtra chamados órfãos antes de criar disputa/pergunta; chamado existente não é fechado pela filtragem.
- `app/chamados/services/pergunta_no_app.py:88-91`: retorna causa `parcial_na_lista_do_admin` ANTES da causa `coberto_por_ausencia`. O rótulo e dono `'admin'` ficam em `:26-27`. `medir` transforma retenções de chamados existentes em itens (`:126-144`) e contagens (`:145-149`). O command `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:14-21` imprime as contagens.

Supra:

- `app/ponto/supra_juiz.py:381-389`: com fato no dia encerrado + aprovado + sem_periodo, adiciona `PARCIAL_SEM_PERIODO`, texto “cobrar o CADASTRO”.
- Segundo ramo de batida sem par usa o mesmo código em `app/ponto/supra_juiz.py:437-442`.
- O dia sem fato pode seguir sendo FURO: a parcial sem período não é dia inteiro (`app/ponto/supra_juiz.py:268-269`), não ganha dispensa que satisfaça `_parcial_sem_resto` (`:311-312`), e o ramo de furo o alcança em `:313-316`. O cartório posteriormente cala a emissão por `parcial_so_lista`.
- `app/ponto/supra_juiz.py:530-534`: `_resto_cobrado` exige período diferente de sem_periodo; aprovado + chamado vivo sem esse resto produz `COBRANCA_EM_DIA_COBERTO`.
- `app/escala/models.py:1615` mapeia `PARCIAL_SEM_PERIODO` para `indefinida`; a ordem do mapa dá precedência a furo, cobrança indevida e coberta (`:1612-1615`).

Destino “lista do admin”, distinção comprovada:

- Existe promessa literal nos comentários de `app/ponto/services/cartorio.py:379-382` e texto da tela `app/templates/ponto/partials/painel_ausencia.html:296-299`: se parcial aprovada e sem `hora_ini`, mostra “sem periodo: o dia esta na lista do DP”.
- A lista real é `app/escala/views.py:484-492`, chamando `cadastro_realidade.lista`.
- Ela agrega explicitamente fontes de C1–C12 (`app/escala/services/cadastro_realidade.py:800-879`) e as A lavradas (`:886-887`, apresentação `:936-945`). C11 seleciona `veredito='indefinida'` mas **exige** protesto `PREVISTO_COM_DIA_INDEFINIDO` (`:477-478`), não `PARCIAL_SEM_PERIODO`.
- Não localizei publicador específico que coloque o código/dia `PARCIAL_SEM_PERIODO` nessa lista real. Presença efetiva por essa condição: **[não verificado]**. Bloqueio da emissão e contagem `parcial_na_lista_sem_push` estão comprovados, mas não são publicação de linha. [nao verificado]
- Existe caminho genérico A9 para ausência + pelo menos 2 batidas (`app/ponto/services/esmeril_espelho.py:133-134`), sem distinguir sem_período; é rotulado “juiz” (`:23`). Não garante que cada parcial sem período apareça na lista.

## 08/11 09:49 · sessao a44dd9306e29 · ATA-X-TURNO-PAUSA · verificador · origem=Code

**O delta corrige os achados 1 e 2 no caminho novo, mas não sustenta equivalência universal entre `marcos_do_dia` e `pausas_do_dia`. Há consulta adicional no caminho do motor e lacunas nos testes novos.** A comparação contra o main abaixo usa o diff integral fornecido; checkout histórico, execução dos testes e ocorrência em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1705–1754`; `app/ponto/services/cxr_na_lista.py:32–49`; `app/escala/services/cadastro_realidade.py:397–400`)

**A. Correção e contraexemplos**

- **Achado 1: corrigido.** Cada pausa real precisa estar longe, nas duas pontas, de **todas** as declaradas. Basta uma declarada descrevê-la para não acusar. O relato escolhe a menor soma das distâncias absolutas; o corte permanece estritamente `>180`, nas duas pontas. (`app/ponto/motor_calculo_v2.py:1705–1710`, `:2027–2030`)
- **Achado 2: corrigido para uma nova lavra.** Os quatro campos entram na allowlist e C12 possui ramo próprio. A leitura não recupera campos descartados de snapshots anteriores: `ultimo_lavrado` apenas devolve o detalhe persistido. Nova lavra em produção: [nao verificado]. (`app/ponto/services/cxr_na_lista.py:32–34`, `:91`, `:147–166`; `app/escala/services/cadastro_realidade.py:397–400`)
- **Dono sem `pausas_do_dia`: agora silencia.** Não há fallback para `hii/hfi`. Existem entradas desse formato nos testes: `_TE`, passado como template, e `_Vinculo`, passado como `escala_colaborador`. Logo, existe mudança no contrato aceito pelo motor; dono real de produção sem a irmã: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1733–1735`; `app/ponto/tests/test_atraso_cross_midnight.py:8–26`, `:38–48`; `app/ponto/tests/test_motor_pareia_pelo_dna.py:48–60`, `:74–76`)
- **Template versus célula:** com célula existente e `dna.marcos` vazio/nulo, `marcos_do_dia` pode oferecer pausa do template, mas `pausas_do_dia` responde vazio. O delta deixa de acusar nesse estado; respeita a regra já existente de pausa declarada pela célula. (`app/escala/models.py:1124–1133`, `:1138–1158`, `:1189–1208`; `app/ponto/motor_calculo_v2.py:1742–1747`)
- **Noturno com duas pausas:** no exemplo construído `19–07`, pausas `22–23` e `02–03`, o helper ancora a primeira em D e a segunda em D+1, conservando `dia=D` no relato. Essa construção coincide com a do produtor de lâmpadas; o teste novo de duas pausas não exercita esse caso noturno. (`app/ponto/motor_calculo_v2.py:1749–1754`; `app/ponto/management/commands/gerar_celulas.py:55–69`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:56–88`)
- **Ordem do cadastro:** não altera a existência da acusação, pois o predicado consulta todas. Em empate da distância mínima, vence a primeira declarada encontrada; portanto pode alterar o par publicado. A fonte preserva ordem cadastral, sem ordenar por relógio. (`app/ponto/motor_calculo_v2.py:1708–1710`; `app/escala/models.py:290–307`)
- **Dedupe por dia:** conserva somente a primeira pausa acusável daquele dia. Duas pausas divergentes distintas não produzem duas linhas. Isso é explícito e reinicia a cada cálculo; a lavra não consegue recuperar a segunda. (`app/ponto/motor_calculo_v2.py:1695`, `:1711–1723`, `:2364–2365`)

**B. Autoridade dos métodos e DNA**

O seletor usado pela acusação escolhe `escala_colaborador.marcos_do_dia`; na ausência desse método, escolhe `tipo_escala` ou `self.escala`. Nas duas classes reais examinadas, ambos os métodos existem. (`app/ponto/motor_calculo_v2.py:2768–2772`; `app/escala/models.py:281`, `:326`, `:1098`, `:1166`)

| Dono/estado | Relação entre primeira pausa e marcos |
|---|---|
| `TipoEscala` | A primeira pausa vem literalmente de `marcos_do_dia`. Override impede acrescentar a segunda do modelo; modo duração remove pausa fixa. (`app/escala/models.py:304–308`, `:335–345`) |
| EC com DNA contendo primeiro par completo | Ambos leem `hii/hfi` do mesmo DNA congelado. (`app/escala/models.py:1124–1132`, `:1189–1201`) |
| EC sem célula | Ambos recorrem ao template. (`app/escala/models.py:1133`, `:1208`) |
| EC com célula e marcos vazios/nulos | **Divergem:** marcos cai no template; pausas retorna vazio. (`app/escala/models.py:1124–1133`, `:1202–1208`) |
| EC com primeiro par incompleto/ausente e segundo completo | Marcos devolve o primeiro par literal; pausas elimina pares incompletos e pode devolver o segundo como `pausas[0]`. O produtor normal não gera sozinho essa configuração; ocorrência em produção: [nao verificado]. (`app/escala/models.py:1131–1132`, `:1196–1201`; `app/ponto/management/commands/gerar_celulas.py:81–93`) |

Portanto, **“mesmo dono” não implica “mesma resposta no par 1 em todo estado”**. A divergência da célula sem marcos é anterior ao delta e está explicada no próprio leitor de intervalo. (`app/escala/models.py:1138–1164`)

| Campo do DNA | Quem escreve | Quem lê |
|---|---|---|
| `hii/hfi` | `montar_dna`, a partir de `te.marcos_do_dia`. (`app/ponto/management/commands/gerar_celulas.py:81–84`) | EC.marcos, EC.pausas e mapa de marcos do período. (`app/escala/models.py:1131–1132`, `:1197–1200`; `app/escala/utils.py:103–107`) |
| `hii2/hfi2` | `montar_dna`, a partir de `pausas[1:]`. (`app/ponto/management/commands/gerar_celulas.py:92–93`) | EC.pausas e mapas de pausas do período. (`app/escala/models.py:1197–1200`; `app/escala/utils.py:57–61`, `:141–145`) |
| Todos os pares | Vocabulário e ordem em `CelulaDia.PARES_PAUSA_DNA`/`chaves_do_turno`; lâmpadas derivadas pelo gerador. (`app/escala/models.py:1652–1668`; `app/ponto/management/commands/gerar_celulas.py:60–69`) | Juiz da batida, cartório e papel da ata. (`app/ponto/juiz_batida.py:166–176`; `app/ponto/services/cartorio.py:581–586`; `app/ponto/turnos.py:1586–1587`, `:1711–1712`) |

A persistência do blob passa pela porta: geração/regeneração, edição, enriquecimento/geração e reversão que restaura `dna_anterior`. (`app/ponto/portas/celula.py:244–276`, `:591–609`, `:783–794`, `:848–870`)

**C. Quatro campos, `_txt`, dedupe e leitores**

| Campo | Quem escreve | Quem lê diretamente |
|---|---|---|
| `pausa_real` | Acusador: lista com dois instantes ISO localizados. (`app/ponto/motor_calculo_v2.py:1721`) | Allowlist/`medir`, igualdade do item e C12. (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397–399`) |
| `pausa_cadastrada` | Acusador: lista ISO do par escolhido pela menor distância. (`app/ponto/motor_calculo_v2.py:1710`, `:1722`) | Mesmos leitores. (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397–399`) |
| `delta_ida_min` | `(real−prevista)/60`, arredondado a uma casa. (`app/ponto/motor_calculo_v2.py:1705`, `:1723`) | Allowlist/`medir`, igualdade e texto C12. (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:399`) |
| `delta_volta_min` | Mesmo sentido e arredondamento. (`app/ponto/motor_calculo_v2.py:1706`, `:1723`) | Mesmos leitores. (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:400`) |

`_txt` alcança **todos os 14 campos da allowlist**, não somente as pausas. Agora qualquer lista/tupla vira texto unido por ` -> `; antes virava `str(v)`. Números permanecem números, inclusive `bool`; `None` permanece `None`; os demais tipos viram texto. No fluxo normal, valores `None` já são excluídos antes da chamada. (`app/ponto/services/cxr_na_lista.py:32–49`, `:91`)

Os produtores atuais dos campos antigos escrevem strings/números; os dois pares novos são listas. Assim, a mudança concreta de representação está nos pares: **lista no resultado do motor, string no snapshot**. Os deltas conservam representação numérica. (`app/ponto/motor_calculo_v2.py:1634–1639`, `:1682–1684`, `:1721–1723`, `:2073`; `app/ponto/services/cxr_na_lista.py:46–49`)

O `it in _do_colab` compara o item completo **depois** da conversão e do enriquecimento. Os quatro campos passam a distinguir relatos de pausas diferentes que antes poderiam colapsar. Não é dedupe somente por dia/causa; o dedupe por dia ocorre antes, dentro do motor. (`app/ponto/services/cxr_na_lista.py:89–112`; `app/ponto/motor_calculo_v2.py:1714–1723`)

A cadeia persistente é `medir → lavrar → MetricaSnapshot.detalhe → ultimo_lavrado → C12 → detalhe na lista/HTML/PDF`. A tela e o PDF recebem o texto formatado, sem interpretar os quatro valores separadamente. (`app/ponto/services/cxr_na_lista.py:147–166`; `app/escala/services/cadastro_realidade.py:397–409`, `:875–876`, `:930–932`, `:1110`; `app/templates/escala/cadastro_x_realidade.html:28`)

Outros leitores usam o envelope: fechamento concatena a lista; oráculo lê `dia`; `cxr_lavrado` lê identificação/data; relato de janela e resumo do espelho filtram causas próprias; `diff_janela_he` exclui expressamente `l084_pausa`. (`app/ponto/services/fechamento.py:96–97`, `:294–296`; `app/ponto/management/commands/e6_oraculo.py:170–171`; `app/ponto/services/cxr_na_lista.py:179–189`; `app/ponto/janela_he.py:277–282`; `app/ponto/services/espelho.py:1259–1261`; `app/ponto/management/commands/diff_janela_he.py:72–73`)

**D. Consultas no fechamento/lote**

**Sim: cada nova chamada elegível a `_turnos_do_juiz` acrescenta um SELECT de vínculos.** Ela fornece células e batidas, mas omite `escalas=`. O papel então chama `_escalas_da_ata`, com `select_related('tipo_escala')`: uma consulta, mesmo que depois nenhuma ata produza papel. O comentário “ALIMENTACAO, NUNCA CONSULTA” não descreve esse caminho. (`app/ponto/motor_calculo_v2.py:884–893`; `app/ponto/turnos.py:1561–1564`, `:1595–1597`, `:1634`)

O sítio antigo também omite `escalas=` e já pagava uma consulta. Nesta cópia, o nome da função é `turnos_via_autoridade`; `_parear_motor` aparece no comentário. (`app/ponto/motor_calculo_v2.py:453`, `:494–502`, `:885`)

Contando **somente esses SELECTs de escala para o papel**, em cálculo normal com batidas, vínculo e células elegíveis:

| Motor | Main pelo diff fornecido | Obra | Consultas acrescentadas |
|---|---:|---:|---:|
| Base/12x36 | 1 | 2 | +1. (`app/ponto/motor_calculo_v2.py:502`, `:2428`) |
| TurnoPartido | 1 | 3 | +2: chamada no `super` e novamente no override. (`app/ponto/motor_calculo_v2.py:2995`, `:3008`) |
| Comercial | 1 | 3 | +2: chamada no `super` e novamente via `_dia_do_turno`. (`app/ponto/motor_calculo_v2.py:3401`, `:3412`, `:898–899`) |

Sem as guardas `cid`, mapa alimentado e células desse colaborador, o sítio novo acrescenta zero. Isso não representa o total de consultas de `calcular_mes`. (`app/ponto/motor_calculo_v2.py:888–893`)

Os caminhos atingidos são:

- **Fechamento por fatia:** alimentação em `:750`, apuração em `:754`, cálculo por fatia em `:286`. (`app/ponto/services/fechamento.py:750–755`, `:278–291`)
- **Passada órfã:** alimentação em `:825`, cálculo com `ec=None` e `colaborador_id` em `:827–829`. O sítio novo pode consultar; o antigo depende de `_ec.colaborador_id` e não entra nesse ramo. (`app/ponto/services/fechamento.py:825–829`; `app/ponto/motor_calculo_v2.py:469`, `:496`, `:876`)
- **Espelho:** alimentação, fatias ou cálculo direto; **PDF:** chama o espelho. (`app/ponto/services/espelho.py:1131`, `:1164`, `:1179`, `:1227`; `app/relatorios/pdf_espelho.py:596–598`)

Há ainda custo específico do **delta v2**: fora do contexto de alimentação, cada turno passa a consultar célula duas vezes — marcos, já existente na v1, e pausas, acrescentada agora. Dentro do contexto e da janela, ambos usam o mapa. Turnos do pad fora da janela podem consultar, pois o helper não filtra `data_turno` pelo período pedido. (`app/escala/models.py:1048–1067`, `:1117–1123`, `:1182–1188`; `app/ponto/services/fechamento.py:278–291`; `app/ponto/motor_calculo_v2.py:1738–1754`)

Contagem executada e impacto de latência no lote: [nao verificado].

**E. Universo deduplicado: anterior à obra**

**Sim.** O fluxo normal limpa as batidas antes de `montar_periodos`; as implementações chegam a `_periodos_via_autoridade`, que chama o sítio antigo com essas batidas. Portanto, o papel do pareamento monetário já recebia universo deduplicado. (`app/ponto/motor_calculo_v2.py:2433–2436`, `:2835`, `:2962`, `:3300`, `:3619`, `:530`, `:502`)

`turnos_do_colab` carrega batidas vivas cruas e entrega essa lista ao papel. O papel conta por minuto e exige exatamente uma batida em cada luz. (`app/ponto/turnos.py:1866–1869`, `:1918`, `:1942`, `:1615–1620`, `:1659`)

Contraexemplo estático: duas entradas consecutivas `08:00:01 E` e `08:00:20 E`, no minuto de uma luz. O motor elimina a segunda; o papel sobre o universo cru encontra duas e rejeita a emissão daquele dia. Ocorrência desse exemplo em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:736`, `:914–923`; `app/ponto/turnos.py:1659`)

Pelo diff integral fornecido, **essa divergência entre motor monetário e espelho é anterior**; a obra a estende ao novo consumo de papel em `_turnos_do_juiz`. (`app/ponto/motor_calculo_v2.py:502`, `:882–893`; `app/ponto/turnos.py:1942`)

**F. Checklist e força dos testes** [nao verificado]

| Classe | Resultado da leitura |
|---|---|
| Segundo juiz registrado | O par real continua vindo de `intervalos_do_turno`, e o corte reutiliza `_cadastro_nao_descreve`; são autoridades já registradas. O delta não repareia pausas reais. (`app/core/juizes.py:202`; `app/ponto/motor_calculo_v2.py:1708`, `:1753`; `app/ponto/turnos.py:540–544`) |
| Segundo escritor fora da porta | A acusação escreve memória; CXR continua persistindo pelo `lavrar` existente. A reconciliação da grade altera células/orfas em memória; o cartório entrega a ata à porta de lavra. (`app/ponto/motor_calculo_v2.py:1720–1723`; `app/ponto/services/cxr_na_lista.py:147–153`; `app/escala/utils.py:2131–2133`; `app/ponto/services/cartorio.py:1800–1802`) |
| Relógio fora da borda | O delta usa datas/instantes recebidos. A lavra continua recebendo `agora` do command, que lê o relógio na borda. (`app/ponto/motor_calculo_v2.py:1749–1754`; `app/ponto/services/cxr_na_lista.py:52–65`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30–40`) |
| Derivação paralela | Pausas declaradas vêm da irmã do dono escolhido. A conversão hora→instante permanece local e reproduz a regra do produtor; para duas pausas noturnas bem formadas, coincide. A equivalência universal das fontes, porém, é falsa pelos estados de B. (`app/ponto/motor_calculo_v2.py:1733–1752`; `app/ponto/management/commands/gerar_celulas.py:55–69`; `app/escala/models.py:1202–1208`) |
| Ordem de trava | Os trechos novos não introduzem transação nem aquisição explícita de trava; a ordem de publicação continua acusação → teto de pontualidade → resultado. Verificação concorrente em execução: [nao verificado]. (`app/ponto/motor_calculo_v2.py:1720–1754`, `:2802–2806`; `app/ponto/services/cxr_na_lista.py:147–153`) |
| Campo com dois significados | `dias_cadastro_x_realidade` inclui pausa, que não significa dia retirado da janela. O instrumento foi ajustado para essa diferença; os pares também mudam de lista para string ao atravessar a lavra. (`app/ponto/management/commands/diff_janela_he.py:70–73`; `app/ponto/motor_calculo_v2.py:1721–1723`; `app/ponto/services/cxr_na_lista.py:46–49`) |

Os testes novos **mordem a volta do comparador contra apenas a primeira pausa**, a retirada dos campos da allowlist e a retirada do ramo C12. Entretanto, o caso de duas pausas usa dono fake e intervalos mockados; o teste C12 monta o item manualmente e mocka `ultimo_lavrado`. (`app/ponto/tests/test_ata_pausa_l084_cxr.py:56–88`, `:96–117`)

Eles **não mordem retirar o ramo lista/tupla de `_txt`**: `str(lista_de_ISO)` ainda contém todos os horários exigidos. Tampouco exigem valores/sinais dos deltas ou atravessam `medir → lavrar → snapshot`; não cobrem o helper com EC real, célula vazia, dono sem irmã ou duas pausas noturnas. (`app/ponto/tests/test_ata_pausa_l084_cxr.py:72–88`, `:106–117`)

O teste de reset morde o reaproveitamento do dedupe, mas o teste de dedupe repete a mesma pausa; não demonstra qual relato sobrevive quando há duas divergências distintas no mesmo dia. (`app/escala/tests/test_ata_x_turno_pausa.py:332–350`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:48–53`)

Finalmente, **“só acusa” vale para o acusador, não prova imobilidade financeira da obra inteira**: a nova alimentação de papel altera o caminho que produz o mapa do dia antes de `montar_periodos`. Ausência de diferença monetária nesta cópia: [nao verificado]. (`app/ponto/motor_calculo_v2.py:893–894`, `:2428–2436`, `:1720–1724`)

O restante do estado lido/escrito pelo delta fica assim:

| Valor/estado | Quem escreve | Quem lê |
|---|---|---|
| `_turnos_juiz_calc`, `entrada`, `data_turno` | Pareador produz turnos; `calcular_mes` guarda a resposta. (`app/ponto/turnos.py:321`; `app/ponto/motor_calculo_v2.py:2428`, `:2478`) | Helper filtra turnos e usa o dia explícito. (`app/ponto/motor_calculo_v2.py:1738–1754`) |
| `ida/volta` reais | `intervalos_do_turno`. (`app/ponto/turnos.py:543–544`) | Helper e acusador. (`app/ponto/motor_calculo_v2.py:1753–1754`, `:1705–1706`) |
| `hi/hf`, pausas declaradas | Métodos do dono descritos em B. (`app/escala/models.py:304–345`, `:1124–1133`, `:1189–1208`) | Helper ancora `previstas`; acusador compara e escolhe o par. (`app/ponto/motor_calculo_v2.py:1742–1752`, `:1705–1710`) |
| `dia`, `causa='l084_pausa'` | Acusador. (`app/ponto/motor_calculo_v2.py:1720`) | Lavra, C12, oráculo e filtro do instrumento. (`app/ponto/services/cxr_na_lista.py:91–107`; `app/escala/services/cadastro_realidade.py:399–409`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/management/commands/diff_janela_he.py:73`) |
| `_l084_pausa_acusada` | Reset no cálculo; inclusão no acusador. (`app/ponto/motor_calculo_v2.py:2365`, `:1716`) | Guarda por dia. (`app/ponto/motor_calculo_v2.py:1714`) |
| `_cadastro_x_realidade` | Reset e append do acusador. (`app/ponto/motor_calculo_v2.py:2364`, `:1720–1723`) | Publicação no resultado e leitores de C. (`app/ponto/motor_calculo_v2.py:2806`; `app/ponto/services/cxr_na_lista.py:84–91`) |
| Campos antigos atravessados por `_txt` | Produtores de janela e L-084 do dia. (`app/ponto/motor_calculo_v2.py:1634–1639`, `:1682–1684`, `:2073`) | Allowlist, conversão, igualdade e ramos antigos de C12. (`app/ponto/services/cxr_na_lista.py:32–49`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:394–404`) |

## 08/11 09:46 · sessao 64acf49b617b · - · - · origem=Ronald (tmux)

A/B — conclusão sustentada pelo código:

- O delta cura o falso positivo da segunda pausa: cada pausa real agora precisa divergir de **todas** as declaradas; uma declarada que descreva ambas as pontas impede a acusação. A linha escolhe menor soma das distâncias absolutas. `app/ponto/motor_calculo_v2.py:1705-1710`.
- Dia obrigatório elimina fallback pelo instante da saída; grava diretamente o dia do turno recebido. `app/ponto/motor_calculo_v2.py:1688`, `:1720`, `:1754`.
- EC, tipo_escala e self.escala são as três opções do seletor; a prioridade é EC, depois tipo_escala ou self.escala. `app/ponto/motor_calculo_v2.py:2768-2772`.
- Ambas classes reais têm os dois métodos. No template, a primeira pausa vem literalmente de `marcos_do_dia`; override impede soma da segunda pausa e modo duração impede pausa fixa. `app/escala/models.py:281-308`, `:326-345`.
- **A afirmação de mesma autoridade em todo estado é falsa:** EC.marcos cai no template se célula existe com DNA/marcos nulo/vazio; EC.pausas retorna `()` nessa célula. Trata-se de diferença intencional anterior, documentada em `intervalo_do_dia`, e o delta passa a silenciar a acusação nesse estado. `app/escala/models.py:1124-1133`, `:1189-1208`, `:1138-1158`; `app/ponto/motor_calculo_v2.py:1742-1747`.
- Outra diferença possível: DNA com primeiro par ausente/incompleto e segundo completo faz EC.marcos devolver `(None,None)` ou meio par no primeiro, mas EC.pausas comprime completos e devolve a segunda como `pausas[0]`. `app/escala/models.py:1131-1132`, `:1196-1201`. A ocorrência em produção é [nao verificado].
- O produtor normal não cria sozinho essa segunda sem primeira: segundo par é escrito a partir de `pausas[1:]`, e TipoEscala só estende pares extras se o primeiro existir. `app/ponto/management/commands/gerar_celulas.py:81-93`; `app/escala/models.py:304-307`.
- **Dono sem irmã existe na interface exercitada pelos testes:** `_TE` tem só marcos e é passado ao motor/calcular_mes, e `_Vinculo` também tem só marcos e é passado como escala_colaborador. Com o delta esses donos ficam silenciosos; a volta1 ainda compararia hii/hfi. `app/ponto/tests/test_atraso_cross_midnight.py:8-26`, `:38-48`; `app/ponto/tests/test_motor_pareia_pelo_dna.py:48-60`, `:74-76`; guarda nova `app/ponto/motor_calculo_v2.py:1733-1735`. Dono real de produção sem irmã é [nao verificado].
- Noturno com duas pausas 19–07 / 22–23 e 02–03 recebe corretamente D e D+1 pelo `< hi`, mesma construção existente do produtor de lâmpadas. Não achei contraprova para esse caso bem formado. `app/ponto/motor_calculo_v2.py:1749-1752`; `app/ponto/management/commands/gerar_celulas.py:55-69`.
- Ordem cadastral é preservada, não ordenada por relógio; isso mantém identidade do primeiro par. No acusador, ordem só interfere em empate da soma de distâncias, porque `min` conserva primeiro candidato; existência da acusação independe da ordem. `app/escala/models.py:259-260`, `:290-307`; `app/ponto/motor_calculo_v2.py:1708-1710`.
- Dedupe é por dia e guarda apenas a primeira pausa divergente desse dia; duas divergências distintas não viram duas linhas. Reinicia em cada cálculo. `app/ponto/motor_calculo_v2.py:1711-1723`, `:2364-2365`. É redução explícita, não prova de cobertura de todas as pausas divergentes. `app/ponto/motor_calculo_v2.py:1695`.

Custo adicional da autoridade:

- Dentro de `celulas_do_ato`, tanto marcos como pausas leem o mapa, zero queries de célula; contexto cobre os cálculos das fatias. `app/escala/alimentacao.py:24-51`; `app/ponto/services/fechamento.py:277-291`; `app/escala/models.py:1117-1123`, `:1182-1188`.
- Fora do contexto ou fora da janela alimentada, o helper faz duas consultas de célula por turno: uma marcos já existente na v1 e **uma adicional pausas** no delta. Não há memoização por dia. `app/escala/models.py:1048-1067`, `:1122-1123`, `:1187-1188`; `app/ponto/motor_calculo_v2.py:1738-1744`.
- Turnos do pad podem cair fora da janela: helper varre todos sem filtrar data_turno e identidade só cobre ini..fim. `app/ponto/motor_calculo_v2.py:1738-1754`; `app/escala/models.py:1066-1067`.

DNA — escritores/leitores:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| hii/hfi | `montar_dna`, via te.marcos: `app/ponto/management/commands/gerar_celulas.py:81-84` | EC.marcos `app/escala/models.py:1131-1132`; EC.pausas `:1196-1201`; mapas 4-tupla `app/escala/utils.py:103-107` |
| hii2/hfi2 | `montar_dna`, via pausas[1:]: `app/ponto/management/commands/gerar_celulas.py:92-93` | EC.pausas `app/escala/models.py:1196-1201`; mapas de pausas `app/escala/utils.py:57-61`, `:141-145` |
| Todos pares | Vocabulário central: `app/escala/models.py:1652-1668`; lâmpadas derivadas `app/ponto/management/commands/gerar_celulas.py:60-69` | juiz `app/ponto/juiz_batida.py:166-176`; cartório `app/ponto/services/cartorio.py:581-586`; veredito `app/ponto/services/veredito_celula.py:31-38`; papel `app/ponto/turnos.py:1586-1587`, `:1711-1712` |
| Persistência do DNA | Porta gera/regenera `app/ponto/portas/celula.py:244-276`; edição `:591-609`; reversão restaura blob anterior `:783-794`; geração `:852-870` | Métodos/fontes acima |

Cobertura:

- O novo teste de duas pausas morde a volta do comparador contra a primeira, mas usa dono fake e pausa do juiz mockada; não exercita EC/DNA, template real, noturno duplo, dono sem irmã ou célula sem marcos. `app/ponto/tests/test_ata_pausa_l084_cxr.py:56-88`.
- Há selo antigo do produtor para noite com dois pares, mas não é teste do novo helper. `app/escala/tests/test_celula_segundo_intervalo.py:159-170`.
- Não executei testes; aprovação/RED efetivo desta cópia é [nao verificado]. [nao verificado]

## 08/11 09:46 · sessao d2f0642fdd2e · - · - · origem=Ronald (tmux)

D: há consulta nova. Com `cid`, células alimentadas e ao menos uma célula desse colaborador, `_turnos_do_juiz` chama `papel_por_minuto_da_ata` sem `escalas=`; esta chama `_escalas_da_ata`, que executa **um SELECT** com `select_related('tipo_escala')`. O comentário “ALIMENTACAO, NUNCA CONSULTA” não corresponde ao caminho executado. `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:888`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1561`.

A consulta acontece mesmo sem batidas ou com ata ilegível, porque a escala é carregada antes da análise das células. Sem satisfazer as guardas do novo sítio, ele acrescenta zero consultas. `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1616`, `app/ponto/turnos.py:1634`, `app/ponto/motor_calculo_v2.py:888`.

| Caminho elegível | SELECTs novos por `calcular_mes` | Evidência |
|---|---:|---|
| Base/12x36/Plantão | +1 | `app/ponto/motor_calculo_v2.py:2428` |
| TurnoPartido | +2 | `app/ponto/motor_calculo_v2.py:2995`, `app/ponto/motor_calculo_v2.py:3008` |
| Comercial | +2 | `app/ponto/motor_calculo_v2.py:3401`, `app/ponto/motor_calculo_v2.py:3412`, `app/ponto/motor_calculo_v2.py:899` |

O sítio antigo também omite `escalas=` e já pagava um SELECT. O nome real da função nesta cópia é `turnos_via_autoridade`; `_parear_motor` aparece somente no comentário. Num cálculo normal elegível, contando apenas essas consultas de escala da ata, passa de 1 para 2 no Base e de 1 para 3 nos dois overrides acima. `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:502`, `app/ponto/motor_calculo_v2.py:885`.

Os caminhos atingidos são:

- Fechamento por fatia: alimenta células em `app/ponto/services/fechamento.py:750`, chama a apuração em `app/ponto/services/fechamento.py:754` e calcula cada fatia em `app/ponto/services/fechamento.py:286`.
- Passada órfã: alimenta em `app/ponto/services/fechamento.py:825`, calcula com `escala_colaborador=None` e `colaborador_id` em `app/ponto/services/fechamento.py:827`. Nesse ramo, o novo sítio pode consultar; o antigo depende de `_ec.colaborador_id` e fica desabilitado. `app/ponto/motor_calculo_v2.py:469`, `app/ponto/motor_calculo_v2.py:496`.
- Espelho: alimenta em `app/ponto/services/espelho.py:1131`, chama fatias em `app/ponto/services/espelho.py:1164` ou cálculo direto em `app/ponto/services/espelho.py:1179`; a tela pede essa autoridade em `app/ponto/services/espelho.py:1227`.
- PDF: chama o espelho em `app/relatorios/pdf_espelho.py:596`, `app/relatorios/pdf_espelho.py:598`.

`papel_por_minuto_da_ata` diretamente pode executar três consultas sem alimentação: células, escalas e batidas. Com células e batidas fornecidas, sem `escalas=`, executa uma; fornecendo também `escalas={}` ou mapa carregado, essas três consultas desaparecem. `app/ponto/turnos.py:1588`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1602`.

E: **sim, o sítio antigo recebe batidas deduplicadas** no fluxo normal de `calcular_mes`: limpa em `app/ponto/motor_calculo_v2.py:2433`, chama `montar_periodos(batidas_limpas)` em `app/ponto/motor_calculo_v2.py:2436`, as implementações delegam a `_periodos_via_autoridade`, que chama `turnos_via_autoridade`. `app/ponto/motor_calculo_v2.py:2835`, `app/ponto/motor_calculo_v2.py:2962`, `app/ponto/motor_calculo_v2.py:3300`, `app/ponto/motor_calculo_v2.py:530`.

`turnos_do_colab` carrega batidas vivas cruas e passa a lista ao papel sem essa deduplicação. O papel conta batidas por minuto e rejeita luzes com contagem diferente de um; também rejeita minuto ambíguo dentro do vão. `app/ponto/turnos.py:1866`, `app/ponto/turnos.py:1918`, `app/ponto/turnos.py:1942`, `app/ponto/turnos.py:1615`, `app/ponto/turnos.py:1659`, `app/ponto/turnos.py:1741`.

Contraexemplo estático: duas entradas consecutivas `08:00:01 E` e `08:00:20 E`, no minuto de uma luz, viram uma entrada no motor, porque o dedupe elimina o mesmo tipo com diferença menor que três minutos; no universo cru, a luz tem duas batidas e impede emissão do papel daquele dia. Ocorrência desse caso em produção: [nao verificado]. `app/ponto/motor_calculo_v2.py:70`, `app/ponto/motor_calculo_v2.py:736`, `app/ponto/motor_calculo_v2.py:914`, `app/ponto/motor_calculo_v2.py:921`, `app/ponto/turnos.py:1659`.

Pelo diff integral fornecido, essa diferença de universo entre o pareamento monetário do motor e `turnos_do_colab` é anterior à obra; a obra a estende ao novo consumo de papel em `_turnos_do_juiz`. Comparação independente com checkout do main e contagem real de consultas/latência: [nao verificado]. `app/ponto/motor_calculo_v2.py:502`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1942`.

## 08/11 09:46 · sessao 1364412be57e · - · - · origem=Ronald (tmux)

C e F, apenas por leitura do código; execução dos testes e estado de produção [nao verificado]. [nao verificado]

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `pausa_real` | `_acusar_pausa_l084`: lista de dois ISO com fuso (`app/ponto/motor_calculo_v2.py:1721`) | allowlist/`medir` e `_txt` (`app/ponto/services/cxr_na_lista.py:33`, `:91`); igualdade do item (`:112`); C12 (`app/escala/services/cadastro_realidade.py:397`, `:399`) |
| `pausa_cadastrada` | Mesmo produtor, par declarado escolhido por distância mínima (`app/ponto/motor_calculo_v2.py:1710`, `:1722`) | Mesmos leitores (`app/ponto/services/cxr_na_lista.py:33`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:397`, `:399`) |
| `delta_ida_min` | `(real-prevista)/60`, arredondado a uma casa (`app/ponto/motor_calculo_v2.py:1705`, `:1723`) | allowlist, transformação, igualdade e texto C12 (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:399`) |
| `delta_volta_min` | Mesmo sentido `real-prevista`, arredondado (`app/ponto/motor_calculo_v2.py:1706`, `:1723`) | Mesmos leitores (`app/ponto/services/cxr_na_lista.py:34`, `:91`, `:112`; `app/escala/services/cadastro_realidade.py:400`) |

A lavra persiste o dicionário inteiro em `MetricaSnapshot.detalhe`, JSONField; `ultimo_lavrado` devolve esse detalhe sem reconstruir campos. Portanto snapshot anterior que perdeu os campos precisa de nova lavra; a leitura sozinha não o corrige (`app/ponto/services/cxr_na_lista.py:147`, `:150`, `:153`, `:157`, `:164`; `app/inteligencia/models.py:95`). Nova lavra em produção [nao verificado].

`_txt` aplica-se aos **14 campos** da allowlist, antes da deduplicação. `list/tuple` agora vira `' -> '.join(str(x)...)`; antes virava `str(v)`. `None` permanece `None`; `int/float` permanecem números, inclusive `bool` por herança de `int`; os demais valores viram texto. Os produtores atuais escrevem os campos antigos como strings/números e os pares novos como listas ISO, de modo que o efeito concreto novo aparece nos dois pares da pausa (`app/ponto/services/cxr_na_lista.py:32`, `:46`, `:91`; produtores antigos em `app/ponto/motor_calculo_v2.py:1634`, `:1682`, `:2073`; pares em `:1721`).

`it in _do_colab` compara o item **já convertido e enriquecido**. Os quatro campos passam a distinguir itens que antes seriam iguais por terem os dados da pausa descartados. A igualdade é do item completo; não é dedupe apenas por dia/causa. O dedupe por dia do motor continua anterior e elimina outras pausas acusáveis daquele dia; a lavra não recupera o que o produtor suprimiu (`app/ponto/services/cxr_na_lista.py:89`, `:91`, `:107`, `:112`; `app/ponto/motor_calculo_v2.py:1714`, `:1720`).

O texto atravessa C12 → `lista` → HTML/PDF: `_cxr_do_motor` formata os quatro valores em `detalhe`; `lista` incorpora a assinatura; HTML imprime `a.detalhe`; PDF também lê esse campo (`app/escala/services/cadastro_realidade.py:397`, `:405`, `:875`, `:930`, `:1110`; `app/templates/escala/cadastro_x_realidade.html:28`; `app/escala/views.py:487`, `:489`, `:492`).

Outros leitores do envelope não interpretam esses quatro valores: fechamento concatena a lista; oráculo usa apenas `dia`; `cxr_lavrado` usa `data/colab/competencia`; resumo do espelho filtra causa de chegada; `pontas_do_relato` exige “janela de HE”; instrumento `diff_janela_he` exclui `l084_pausa` expressamente (`app/ponto/services/fechamento.py:96`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:183`; `app/ponto/services/espelho.py:1259`; `app/ponto/janela_he.py:277`, `:281`; `app/ponto/management/commands/diff_janela_he.py:72`).

Os novos testes mordem a retirada da allowlist e do ramo C12: usam a saída real do produtor, conferem campos e exigem os horários no texto. Contudo **não mordem retirar o novo ramo list/tuple de `_txt`**: `str(lista_de_ISO)` ainda contém todos os horários exigidos. Também não verificam valores/sinais dos deltas, nem atravessam `medir → lavrar → snapshot`; montam `it` manualmente e mockam `ultimo_lavrado` (`app/ponto/tests/test_ata_pausa_l084_cxr.py:96`, `:102`, `:110`, `:112`, `:113`, `:115`, `:116`).

No trecho da lavra, o delta altera transporte/apresentação; o escritor persistente continua `lavrar`, chamado pelo command com um instante lido na borda. Não introduz relógio nesse serviço nem segundo escritor de snapshot (`app/ponto/services/cxr_na_lista.py:52`, `:65`, `:147`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`, `:32`, `:40`).

## 08/11 09:30 · sessao 1eb7712af56e · ATA-X-TURNO-PAUSA · verificador · origem=Code

**Parecer: bloqueia o pouso.** Há falsa acusação com duas pausas, perda dos dados da pausa na lista do admin e divergências entre os caminhos que deveriam consultar o mesmo juiz (`app/ponto/motor_calculo_v2.py:2780`; `app/ponto/services/cxr_na_lista.py:30`; `app/escala/utils.py:2132`; `app/ponto/turnos.py:1659`).

A execução da suíte integrada, o impacto financeiro em produção e a história dos 821 commits permanecem **[nao verificado]**. As reproduções mencionadas abaixo executaram funções reais extraídas por AST em memória; não equivalem à execução completa com Django/banco **[nao verificado]**. [nao verificado]

**Achados — A, C e E**

1. **BLOQUEIA — duas pausas corretas produzem `l084_pausa` falsa.** O laço obtém apenas `hii/hfi` de `marcos_do_dia` e compara **todos** os intervalos reais com esse primeiro par. O main já possui `pausas_do_dia` para devolver todos os pares declarados (`app/ponto/motor_calculo_v2.py:2771`; `app/ponto/motor_calculo_v2.py:2780`; `app/escala/models.py:1166`).

   Caso concreto: previstas **09:15–09:45 e 14:15–15:40**, reais exatamente iguais. A segunda pausa é comparada com 09:15–09:45 e gera deltas **+300/+355 minutos**, suficientes para acusar cadastro incorreto. A reprodução isolada confirmou essa linha; o predicado exige ambas as distâncias absolutas maiores que 180 (`app/ponto/motor_calculo_v2.py:1699`; `app/ponto/motor_calculo_v2.py:1713`; `app/ponto/motor_calculo_v2.py:1989`).

   Isso também pode mudar o dono do dia para CADASTRO, porque E6 consulta a presença do dia na lista sem distinguir a causa; BATIDA ainda precede essa decisão (`app/ponto/management/commands/e6_oraculo.py:104`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/ponto/management/commands/e6_oraculo.py:170`).

2. **BLOQUEIA — CXR-TELA perde os horários e deltas da pausa.** O produtor escreve `pausa_real`, `pausa_cadastrada`, `delta_ida_min` e `delta_volta_min`; a allowlist da lavra não copia esses campos. O formatador C12 reconhece somente os formatos antigos de janela HE e entrada/saída (`app/ponto/motor_calculo_v2.py:1713`; `app/ponto/services/cxr_na_lista.py:30`; `app/ponto/services/cxr_na_lista.py:86`; `app/escala/services/cadastro_realidade.py:394`).

   Para a linha do caso col841, o detalhe resultante é **“entrada real - x cadastrada - (None min), saida real - x cadastrada - (None min)”**. A causa sobrevive na lavra, mas esse ramo nem a apresenta; o admin recebe a acusação sem sua evidência (`app/escala/services/cadastro_realidade.py:398`; `app/escala/services/cadastro_realidade.py:404`).

3. **BLOQUEIA — a cura continua fora do juiz registrado de ocupação do marco.** `_match_marcos` é a autoridade registrada; o bloco novo muda sua resposta posteriormente, somente na grade. A Zona 5 e `marco_ocupado_pela_batida` continuam consultando a fonte sem essa reconciliação (`app/core/juizes.py:200`; `app/escala/utils.py:427`; `app/escala/utils.py:2132`; `app/ponto/motor_calculo_v2.py:3127`).

   Caso concreto do próprio teste: jornada **07–17**, pausa **12:38–13:38**, batidas **06:54 E / 12:38 S / 12:54 E / 17:05 S**. A grade curada exige **hfi=12:54, delta −44**. Na reprodução isolada, `marco_ocupado_pela_batida` com as quatro batidas devolveu `(None, None)` para 12:54; com somente o segundo período, devolveu `hii,+16`. Esse último é o formato passado por `cumpriu_a_volta_do_intervalo`, que recusa a classificação como volta (`app/escala/tests/test_ata_x_turno_pausa.py:139`; `app/escala/tests/test_ata_x_turno_pausa.py:154`; `app/escala/regua_defesa.py:491`; `app/escala/regua_defesa.py:493`).

   **Não é inteiramente redundante com o main:** o alinhamento por papel já senta os pares, mas seu cluster final ainda pode retirar uma perna; a nova reconciliação volta a acendê-la. Há duas respostas para a ocupação, em vez de uma cura completa na fonte (`app/escala/utils.py:561`; `app/escala/utils.py:577`; `app/escala/utils.py:2132`).

4. **BLOQUEIA — “uma entrada só” ainda recebe universos diferentes.** O motor remove duplicatas **antes** de produzir o papel; `turnos_do_colab` produz o papel sobre as batidas carregadas. Isso altera a guarda que exige exatamente uma batida por minuto de luz (`app/ponto/motor_calculo_v2.py:882`; `app/ponto/motor_calculo_v2.py:893`; `app/ponto/turnos.py:1659`; `app/ponto/turnos.py:1942`).

   Contraexemplo reproduzido isoladamente: DNA **07–17 / pausa 12–13**, ata com quatro luzes, cruas **E07:00:01, E07:00:05, S12:00, E13:00, E17:00**, última E gravada cumprindo a saída na ata. Com cruas, o papel fica vazio e surgem três turnos abertos; após o dedupe do motor, o papel é `E/Xi/Xi/S` e surge um turno fechado. A segunda E é removida pelo predicado de duplicata (`app/ponto/motor_calculo_v2.py:921`; `app/ponto/turnos.py:1659`; `app/ponto/turnos.py:1833`; `app/ponto/turnos.py:1380`).

5. **BLOQUEIA a declaração de neutralidade financeira da mudança (3).** O acusador, isoladamente, apenas acrescenta relato; a alteração do papel alimenta caminhos monetários: substituição de períodos, atribuição do dia, julgamento L084 da janela e crédito dos pares de turnos abertos (`app/ponto/motor_calculo_v2.py:1713`; `app/ponto/motor_calculo_v2.py:685`; `app/ponto/motor_calculo_v2.py:2396`; `app/ponto/motor_calculo_v2.py:2447`; `app/ponto/motor_calculo_v2.py:1594`; `app/ponto/motor_calculo_v2.py:2561`).

   Forma concreta noturna reproduzida: DNA **19–07 / pausa 03–04**, luzes **18:56/23:59/03:00/07:01**, cruas **E18:56:01, E18:56:05, S23:59, E03:00, E07:01**. Sem papel, a cauda das 03:00 fica em D+1; com papel após dedupe, fica em D, dentro do turno fechado. O mapa alterado participa dos filtros e da classificação; comercial/partido também o usam para agrupar e recalcular rubricas (`app/ponto/motor_calculo_v2.py:905`; `app/ponto/motor_calculo_v2.py:2392`; `app/ponto/motor_calculo_v2.py:2986`; `app/ponto/motor_calculo_v2.py:3390`). **Valor líquido e rubricas efetivamente alteradas nesse exemplo: [nao verificado].**

6. **FILA — outro contador continua tratando qualquer CXR como isenção de pontualidade.** `diff_reclassificar_partido` coleta toda a lista, soma `len()` e anuncia dias que “deixam de gerar atraso/saida antecipada”. A nova pausa aumenta esse número, embora apenas acuse; seus exemplos também procuram campos de entrada/saída inexistentes (`app/ponto/management/commands/diff_reclassificar_partido.py:188`; `app/ponto/management/commands/diff_reclassificar_partido.py:260`; `app/ponto/management/commands/diff_reclassificar_partido.py:262`; `app/ponto/management/commands/diff_reclassificar_partido.py:269`).

7. **FILA — a chamada nova consulta vínculo apesar do comentário “ALIMENTAÇÃO, NUNCA CONSULTA”.** A omissão de `escalas=` faz `papel_por_minuto_da_ata` chamar `_escalas_da_ata`, com consulta e escolha por `ativa/data_inicio`, em vez de receber diretamente o vínculo da fatia. A dívida dessa escolha por período já está documentada no carregador (`app/ponto/motor_calculo_v2.py:885`; `app/ponto/motor_calculo_v2.py:893`; `app/ponto/turnos.py:1596`; `app/ponto/turnos.py:1544`; `app/ponto/turnos.py:1561`). Impacto de desempenho: **[nao verificado]**.

**B — quem escreve e quem lê**

| Campo/valor | Escritores | Leitores e significado |
|---|---|---|
| `dias_cadastro_x_realidade` | Nasce no resultado; lista privada reiniciada e publicada: `app/ponto/motor_calculo_v2.py:289`, `:2326`, `:2784`. | Fatias concatenam, sem interpretar causa: `app/ponto/services/fechamento.py:96`, `:294`. |
| Itens `dia/causa` | L084 do dia: `app/ponto/motor_calculo_v2.py:1634`; janela HE: `:1682`; pausa: `:1713`; L084 antigo sem `causa`: `:2035`. | E6 projeta qualquer `dia` não nulo em conjunto de datas: `app/ponto/management/commands/e6_oraculo.py:170`. |
| Linha `l084_pausa` | Laço passa explicitamente **dia do turno**; horários são ISO com deslocamento, deltas são minutos: `app/ponto/motor_calculo_v2.py:2781`, `:1714`, `:1716`. | CXR converte `dia→data`, filtra competência e deduplica itens; perde os campos novos: `app/ponto/services/cxr_na_lista.py:86`, `:88`, `:95`, `:107`. |
| `_l084_pausa_acusada` | Criação/adição no acusador e reset por cálculo: `app/ponto/motor_calculo_v2.py:1706`, `:1709`, `:2327`. | Consulta de dedupe por dia no próprio acusador; nenhum outro consumidor encontrado: `app/ponto/motor_calculo_v2.py:1707`. |
| `_turnos_juiz_calc` | Atribuído por cálculo: `app/ponto/motor_calculo_v2.py:2440`. | Janela L084 `:1595`; crédito de aberto `:2561`; acusador novo `:2767`. |
| `_dia_turno_juiz_calc` | Atribuído por cálculo: `app/ponto/motor_calculo_v2.py:2441`. | Janela `:1594`; fallback da pausa `:1703`; relato L084 `:2040`. |
| `papel_por_minuto` | Produzido de ata/DNA: `app/ponto/turnos.py:1568`, `:1808`, `:1809`, `:1833`; alimentação nova: `app/ponto/motor_calculo_v2.py:893`. | Passado ao pareador em `app/ponto/turnos.py:1538`; consultado/aplicado em `:903`, `:1308`, `:1314`, `:1380`. |
| Órfãs e células do matcher | `app/escala/utils.py:615`, `:621`, `:655`, `:662`. | Alinhamento às colunas e mapas do dia: `app/escala/utils.py:1726`, `:1778`, `:1779`. |
| `dias_out[…]['celulas'/'orfas']` | Criação `app/escala/utils.py:1822`, `:1828`; conservação civil `:1853`, `:1857`, `:1859`; pontas `:2064`; pausa nova `:2131`. | Ata provisória `:1917`; guardas `:1992`, `:2111`; cartão `:1107`; cartório `app/ponto/services/cartorio.py:701`; espelho `app/ponto/services/espelho.py:685`; supra-juiz `app/ponto/supra_juiz.py:514`. |
| Overlays das mesmas células/listas | Isenção `app/escala/utils.py:858`; folga `:1173`; ausência `:1238`, `:1266`, `:1277`. | A ata provisória é construída depois desses overlays: `app/escala/utils.py:1895`, `:1917`. |

Os leitores **não atribuem todos o mesmo efeito à causa**: E6 usa pertencimento ao dia; CXR conta itens; C12 deveria apresentar evidências; os leitores de HE filtram causas específicas. Não encontrei conversão nova de dia do turno para dia civil nesses leitores; encontrei perda do conteúdo da pausa e o contador incorreto descritos acima (`app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:126`; `app/escala/services/cadastro_realidade.py:394`; `app/ponto/janela_he.py:281`).

- **CXR muda número legitimamente:** aumenta `total`, `por_causa` e numerador do snapshot; o contrato declara contagem de **itens**, inclusive vários no mesmo dia (`app/ponto/services/cxr_na_lista.py:14`; `:123`; `:126`; `:147`).
- **`diff_janela_he`: não encontrei defeito novo no filtro.** Exclui explicitamente `l084_pausa` (`app/ponto/management/commands/diff_janela_he.py:72`).
- **Espelho/HE:** `pontas_do_relato` aceita causas contendo “janela de HE”; o resumo acima do teto aceita somente a causa de chegada. A pausa não entra nesses subconjuntos (`app/ponto/janela_he.py:281`; `app/ponto/services/espelho.py:1260`).
- **FechamentoMensal/lavra financeira:** não encontrei persistência da lista nem consumo direto dela para rubricas; a composição transporta a lista e o instrumento documenta que o fechamento não a persiste (`app/ponto/services/fechamento.py:97`; `app/ponto/services/fechamento.py:294`; `app/ponto/management/commands/diff_reclassificar_partido.py:169`).
- **Fallback `dia=None`: nota.** O mapa real indexa batidas gravadas E, mas a ida da pausa costuma ser S; o teste direto injeta artificialmente essa S no mapa. O chamador de produção evita essa falha passando `dia=` explicitamente (`app/ponto/motor_calculo_v2.py:905`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:23`; `app/ponto/motor_calculo_v2.py:2781`).

**C — caminhos com e sem células**

| Caminho | Alimentação |
|---|---|
| Espelho | Recebe células: `app/ponto/services/espelho.py:1131`. |
| PDF | Recebe células: `app/relatorios/pdf_espelho.py:405`. |
| Fechamento/lote, fatias normais | Recebe células: `app/ponto/services/fechamento.py:750`. |
| Fechamento, batidas órfãs de vínculo | Também recebe células: `app/ponto/services/fechamento.py:825`. |
| `esmeril_espelho` | Cria/calcula sem alimentar células: `app/ponto/services/esmeril_espelho.py:161`, `:165`. |
| `vigiar_motor_autoridade` | Idem: `app/ponto/management/commands/vigiar_motor_autoridade.py:57`. |
| `diff_extrato_parcial` | Idem: `app/relatorios/management/commands/diff_extrato_parcial.py:61`, `:68`. |

O papel novo permanece `None` sem alimentação, sem células daquele colaborador ou sem identificador; ata desalinhada ou sem bordas suficientes também faz o produtor se abster. Portanto, fechamento/lote **não são genericamente o caminho sem células** no main atual (`app/ponto/motor_calculo_v2.py:886`; `:888`; `:891`; `app/ponto/turnos.py:1639`; `:1685`; `app/ponto/services/fechamento.py:750`).

**D — checklist**

- **Segundo juiz/derivação paralela: encontrado**, no pós-processamento da ocupação e nos universos diferentes do papel, achados 3 e 4 (`app/core/juizes.py:200`; `app/escala/utils.py:2132`; `app/ponto/motor_calculo_v2.py:882`).
- **Segundo escritor persistente fora da porta: não encontrei novo em produção.** O bloco altera a grade em memória; a persistência segue cartório → `lavrar_veredito` → porta (`app/escala/utils.py:2132`; `app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:1028`).
- **Relógio novo fora da borda: não encontrei no diff.** Os acréscimos derivam papel, instantes dos marcos e relato (`app/ponto/motor_calculo_v2.py:882`; `:1699`; `:2777`).
- **Ordem de trava: não encontrei aquisição ou alteração de trava nos trechos novos** (`app/escala/utils.py:2084`; `app/ponto/motor_calculo_v2.py:2764`; `app/ponto/turnos.py:1537`).
- **Pré-requisito “juiz único”: não satisfeito**, pelo contraexemplo de ambiguidade no minuto (`app/ponto/turnos.py:1659`; `app/ponto/motor_calculo_v2.py:893`).
- **ESMERIL-SEMÂNTICA: encontrado**, tanto no contador que chama itens de pausa de dias isentos quanto na comparação da segunda pausa com a primeira (`app/ponto/management/commands/diff_reclassificar_partido.py:262`; `app/ponto/motor_calculo_v2.py:2780`).
- **Testes:** os testes da ata verificam grade final e convergência; os outros dois arquivos verificam predicado/dedupe e presença da acusação no espelho. Não certificam a igualdade com `_match_marcos`, os caminhos sem células, CXR-TELA ou neutralidade das rubricas (`app/escala/tests/test_ata_x_turno_pausa.py:105`; `:193`; `app/ponto/tests/test_ata_pausa_l084_cxr.py:27`; `:56`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:54`). RED por retirada/mutação e suíte completa: **[nao verificado]**.

**E — demais contraexemplos**

- **D+1 civil:** a escolha da lista usa o instante e a origem registrada em `_orfa_civil`; o relato continua com `data_turno`. Não encontrei erro novo nessa seleção para o caso normal de uma pausa noturna (`app/escala/utils.py:1847`; `:2092`; `app/ponto/motor_calculo_v2.py:2781`).
- **Célula/slot ausente:** interrompe a cura; **não encontrei escrita parcial** nessas guardas (`app/escala/utils.py:2116`; `:2125`).
- **Marco ocupado por outra hora:** impede a ascensão das duas pernas. A identificação de “já ocupada por esta batida” continua somente por HH:MM; colisão completa entre datas permanece **[nao verificado]** (`app/escala/utils.py:2119`; `:2125`).
- **Duas pausas na ata:** além da falsa acusação bloqueante, a cura visual pula qualquer caso com dois pares ou seis marcos; não cobre essa classe já suportada pelo main (`app/escala/utils.py:2087`; `app/escala/models.py:1166`).
- **FILA — expectativa reaplicada incompatível com o alinhamento atual:** no cenário com E02:05 extra e par conhecido 21:44–22:41, a fonte senta o par antes das soltas; a reprodução isolada deixa 02:05 órfã e `hii` ausente. O teste exige 02:05 sentada e nenhuma célula ausente. Resultado integrado desse fixture: **[nao verificado]** (`app/escala/utils.py:561`; `:564`; `:577`; `app/escala/tests/test_ata_x_turno_pausa.py:297`).

## 08/11 09:28 · sessao f4c15bc06c56 · - · - · origem=Ronald (tmux)

Achados da escala/ata:

1. **Bloqueia — cura fora do juiz registrado mantém respostas diferentes para o mesmo marco.** O juiz registrado é `_match_marcos` (`app/core/juizes.py:200`); o novo bloco reescreve sua resposta depois, em `app/escala/utils.py:2131`. No fixture 07:00–17:00, pausa 12:38–13:38, batidas 06:54 E / 12:38 S / 12:54 E / 17:05 S, a grade nova acende **hfi=12:54, delta −44** (`app/escala/tests/test_ata_x_turno_pausa.py:139`, `:153`). Executei os ASTs reais em memória: `marco_ocupado_pela_batida` responde **(None,None)** usando as quatro batidas; usando só o segundo período responde **hii,+16**. Esse segundo formato é precisamente o insumo de `cumpriu_a_volta_do_intervalo`, que retorna falso (`app/escala/regua_defesa.py:491`, `:493`). A Zona 5 consulta `_match_marcos` diretamente e seleciona P2 pelo hfi, sem a reconciliação nova (`app/ponto/motor_calculo_v2.py:3122`, `:3127`, `:3129`). Valor financeiro líquido resultante: **[nao verificado]**.

2. **Fila — expectativa antiga permanece em teste novo.** Com o turno conhecido contendo o par 21:44–22:41 e uma E 02:05 solta, o main senta o par antes das soltas (`app/escala/utils.py:561`, `:564`); E 02:05 perde a disputa do hfi já ocupado. O cluster depois deixa hii missing (`:577`). AST real de `_match_marcos` devolveu hi18:46/hii missing/hfi22:41/hf07:04, órfãs21:44 e02:05. O teste exige nenhuma missing, apenas21:44 órfã e02:05 sentada (`app/escala/tests/test_ata_x_turno_pausa.py:294`, `:297`). Pareador completo e suíte: **[nao verificado]**.

3. **Fila — duas pausas ficam fora da cura.** O bloco exige exatamente um par e quatro marcos (`app/escala/utils.py:2087`), embora o main produza a cadeia de todas as pausas e alinhe pares em ordem (`:1630`, `:1636`, `:539`). Exemplo condicionado a dois pares reconhecidos: jornada07–17, pausas10–10:15/14–14:15, batidas E07/S08/E08:30/S15/E15:30/S17. AST de `_alinhar_por_papel` deixa08/08:30 órfãs e primeira pausa missing; o bloco novo necessariamente pula (`:2087`). Reconhecimento desse turno pelo pareador completo: **[nao verificado]**.

4. **Nota — bloco ainda é vivo no golden; não basta removê-lo como redundante.** No golden, AST real deixa hii missing e21:44 órfã; hfi já recebe22:41. A causa é o cluster final (`app/escala/utils.py:577`). A nova escritura cura hii (`:2131`). O teste golden prende esse comportamento (`app/escala/tests/test_ata_x_turno_pausa.py:101`, `:105`).

Censo agrupado:

| Dado | Escritores | Leitores |
|---|---|---|
| Células/orfãs do matcher | `app/escala/utils.py:615`, `:621`, `:655`, `:662` | alinhamento das colunas `:1726`; mapas `:1778` |
| `dias_out[..]['celulas'/'orfas']` | criação `:1822`, `:1828`; conservação civil/I `:1853`, `:1857`, `:1859`; ordenação `:1863`; pontas `:2064`; pausa `:2131` | ata provisória `:1917`; reconciliação `:1992`, `:2111`; cartão `:1107`, `:1131` |
| Overlays das mesmas listas | folga `:1173`; ausência `:1238`, `:1266`, `:1273`, `:1277`; isenção `:858` | ata provisória montada depois, `:1917` |
| Grade final | — | cartório `app/ponto/services/cartorio.py:701`, `:708`, `:1373`; supra-juiz `app/ponto/supra_juiz.py:230`, `:305`, `:514`; espelho `app/ponto/services/espelho.py:361`, `:685` |

**Não encontrei nova escrita persistente de produção fora da porta.** A escrita nova é sobre listas da grade em memória (`app/escala/utils.py:2131`); a persistência permanece cartório→`lavrar_veredito` (`app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:969`, `:1001`). Os testes salvam ata diretamente (`app/escala/tests/test_ata_x_turno_pausa.py:206`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:49`).

Guardas: célula/slot ausente impede cura (`app/escala/utils.py:2116`); marco ocupado por outra hora impede ambas as pernas (`:2119`, `:2125`); D+1 escolhe lista civil usando a origem da órfã e o instante (`:2092`). A comparação da célula já acesa continua só HH:MM (`:2119`); contraexemplo completo com colisão entre datas **[nao verificado]**.

Os testes da ata mordem a retirada do bloco no golden e na volta curta, mas verificam a grade final (`app/escala/tests/test_ata_x_turno_pausa.py:105`, `:146`). A convergência grava e relê a ata sem comparar o juiz de marcos (`:193`). Os outros dois arquivos prendem acusação/ligação no espelho, sem equivalência financeira ou caminhos sem células (`app/ponto/tests/test_ata_pausa_l084_cxr.py:55`; `app/ponto/tests/test_juiz_do_par_uma_entrada.py:53`). Não encontrei comparação nova contra `_match_marcos`, `cumpriu_a_volta` ou Zona 5 nesses três arquivos; execução da suíte **[nao verificado]**.

## 08/11 09:28 · sessao 4a100c439430 · - · - · origem=Ronald (tmux)

Achados da fatia motor/caminhos:

1. **bloqueia — o juiz ainda recebe universos diferentes.** `_turnos_do_juiz` remove duplicatas antes de derivar o papel; `turnos_do_colab` deriva o papel das batidas carregadas, sem esse dedup (`app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1942`). A guarda de ambiguidade exige exatamente uma batida em cada minuto de luz (`app/ponto/turnos.py:1659`).

   Contraexemplo executado em memória com funções reais extraídas por AST: DNA 07–17, pausa 12–13, quatro luzes na ata; batidas `E07:00:01 E07:00:05 S12:00 E13:00 E17:00`, última E gravada cumprindo S na ata. Com cruas: papel `{}` e três turnos abertos; com dedup: mapa `E/Xi/Xi/S` e um turno fechado 07–17. A segunda E é retirada pelo predicado de mesmo tipo e gap inferior a três minutos (`app/ponto/motor_calculo_v2.py:70`, `app/ponto/motor_calculo_v2.py:921`). Portanto “uma entrada só” não está satisfeito nessa forma (`app/ponto/turnos.py:1659`, `app/ponto/motor_calculo_v2.py:893`).

2. **bloqueia a declaração de neutralidade monetária da mudança (3) — seus resultados alimentam dinheiro.** O novo mapa é montado antes de `montar_periodos` e usado para excluir períodos substituídos pela ata; também classifica o dia, decide dispensa da janela L084 e credita períodos abertos (`app/ponto/motor_calculo_v2.py:2392`, `app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:685`, `app/ponto/motor_calculo_v2.py:2449`, `app/ponto/motor_calculo_v2.py:1594`, `app/ponto/motor_calculo_v2.py:2561`). Comercial e partido chamam esse juiz novamente para agrupar períodos e recalcular HE/intrajornada/feriado (`app/ponto/motor_calculo_v2.py:2986`, `app/ponto/motor_calculo_v2.py:3007`, `app/ponto/motor_calculo_v2.py:3030`, `app/ponto/motor_calculo_v2.py:3390`, `app/ponto/motor_calculo_v2.py:3415`).

   Forma concreta também exercitada por AST: DNA 19–07, pausa 03–04; luzes `18:56/23:59/03:00/07:01`; cruas `E18:56:01 E18:56:05 S23:59 E03:00 E07:01`. Sem papel, a cauda E03:00 fica em D+1; com papel após dedup, fica em D, num turno fechado único. Isso altera o insumo usado pelos ramos monetários acima; valor financeiro final desse caso **[nao verificado]**. Não confundir com `_acusar_pausa_l084`, cujo corpo apenas acrescenta relato (`app/ponto/motor_calculo_v2.py:1694`, `app/ponto/motor_calculo_v2.py:1713`).

3. **fila — “ALIMENTAÇÃO, NUNCA CONSULTA” não corresponde à chamada nova.** A chamada nova omite `escalas=`, levando obrigatoriamente a `_escalas_da_ata`, que consulta e escolhe vínculo por `ativa/data_inicio`; não usa diretamente o vínculo da fatia (`app/ponto/motor_calculo_v2.py:893`, `app/ponto/turnos.py:1596`, `app/ponto/turnos.py:1561`, `app/ponto/turnos.py:1563`). A dívida de escolha por período está documentada no próprio carregador (`app/ponto/turnos.py:1544`). Comercial/partido podem repetir essa carga na segunda chamada do juiz (`app/ponto/motor_calculo_v2.py:2986`, `app/ponto/motor_calculo_v2.py:3390`). Impacto de desempenho medido **[nao verificado]**.

4. **fila — os testes novos não certificam dinheiro nem todos os bocais.** O teste da entrada única só exige presença da acusação no espelho após lavar a ata; não afirma minutos, HE, atraso, intra ou noturno, nem fechamento/lote, nem cruas versus dedup (`app/ponto/tests/test_juiz_do_par_uma_entrada.py:54`, `app/ponto/tests/test_juiz_do_par_uma_entrada.py:61`). Os testes do acusador exercitam predicado/dedupe e presença no espelho, sem comparar essas rubricas (`app/ponto/tests/test_ata_pausa_l084_cxr.py:27`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:36`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:43`, `app/ponto/tests/test_ata_pausa_l084_cxr.py:56`). O teste direto injeta artificialmente a S da ida no mapa de dia, enquanto o produtor real só indexa batidas gravadas E; o laço real evita o problema passando `dia=` explicitamente (`app/ponto/tests/test_ata_pausa_l084_cxr.py:23`, `app/ponto/motor_calculo_v2.py:905`, `app/ponto/motor_calculo_v2.py:2781`). Eficácia por mutação/remover cura **[nao verificado]**.

Censo dos campos solicitados:

| Campo | Escritores | Leitores |
|---|---|---|
| `_turnos_juiz_calc` | atribuído em cada cálculo, `app/ponto/motor_calculo_v2.py:2440` | janela L084 do dia `:1595`; crédito aberto `:2561`, `:2566`; novo acusador `:2767` |
| `_dia_turno_juiz_calc` | `app/ponto/motor_calculo_v2.py:2441` | janela `:1594`; fallback acusador `:1703`; relato L084 `:2040` |
| `_dia_juiz_para_marco` | entrega `app/ponto/motor_calculo_v2.py:2396`; limpa em `finally` `:2400` | filtro de períodos/intervalos `:684`–`:687` |
| `papel_por_minuto` | produzido por `app/ponto/turnos.py:1568`, saídas `:1808`, `:1809`, `:1833`; `_papel` novo no motor `app/ponto/motor_calculo_v2.py:886`, `:893` | repassado `app/ponto/motor_calculo_v2.py:894`, `app/ponto/turnos.py:1538`; leitor `_papel_da_ata` `:888`, consulta `:903`; aplicação no pareador `:1308`, `:1314`, `:1380` |

Caminhos:

- **Espelho/PDF/fechamento/lote normal têm alimentação:** `app/ponto/services/espelho.py:1131`; `app/relatorios/pdf_espelho.py:405`; `app/ponto/services/fechamento.py:750`; caminho órfão `:825`. A apuração por fatia entrega colaborador, vínculo e datas ao motor (`app/ponto/services/fechamento.py:286`).
- **Sem alimentação:** `esmeril_espelho` cria motor e calcula sem atribuir células (`app/ponto/services/esmeril_espelho.py:161`, `:165`); `vigiar_motor_autoridade` idem (`app/ponto/management/commands/vigiar_motor_autoridade.py:57`); `diff_extrato_parcial` idem (`app/relatorios/management/commands/diff_extrato_parcial.py:61`, `:68`). Nesses caminhos o novo papel permanece `None`; células ausentes/vazias, cid ausente, ou nenhuma célula do cid também deixam `None` (`app/ponto/motor_calculo_v2.py:886`, `:888`, `:891`). Ata desalinhada ou dia sem bordas legíveis faz o produtor se abster (`app/ponto/turnos.py:1635`, `:1686`).
- **Bocal já existente:** `turnos_via_autoridade` já derivava e passava papel, com limites `AUT_*`; a obra migra o outro caminho do juiz, não cria o primeiro fornecimento de papel ao motor (`app/ponto/motor_calculo_v2.py:494`, `:500`, `:506`). Porém a receita nova não é literalmente a mesma: `turnos_de_batidas` usa os limites padrão do juiz; o outro caminho passa `AUT_INTRA_MAX_S/AUT_CONT_MAX_S` (`app/ponto/turnos.py:1537`, `app/ponto/motor_calculo_v2.py:504`).

Não encontrei relógio novo, aquisição de trava ou escrita persistente nova nessa fatia; o trecho acrescentado deriva papel e relato em memória (`app/ponto/motor_calculo_v2.py:882`, `:1694`, `:2764`, `app/ponto/turnos.py:1537`). A suíte Django não foi executada: o interpretador disponível não possui Django **[nao verificado]**; as sondas AST acima não executaram ORM nem criaram arquivos.

## 08/11 09:26 · sessao f14fc2900b56 · - · - · origem=Ronald (tmux)

Achados B:

1. **Bloqueia — nova acusação chega à C12 sem evidência da pausa.** O produtor grava `pausa_real`, `pausa_cadastrada`, `delta_ida_min` e `delta_volta_min`, mas a allowlist da lavra não inclui esses campos; a cópia conserva apenas `causa` e metadados. A apresentação C12 conhece apenas os formatos janela de HE e entrada/saída. Caso col841: aparece **“entrada real - x cadastrada - (None min), saida real - x cadastrada - (None min)”**, sem horários, deltas ou indicação textual da pausa. Reproduzi isso em memória com AST das funções reais e insumo sintético do caso, sem Django, banco ou arquivo criado. Fontes: `app/ponto/motor_calculo_v2.py:1713`, `app/ponto/services/cxr_na_lista.py:30`, `app/ponto/services/cxr_na_lista.py:86`, `app/escala/services/cadastro_realidade.py:394`.

2. **Fila — outro contador interpreta pausa como dia isento de pontualidade.** `diff_reclassificar_partido` captura toda a lista, soma `len()` e anuncia “dia-colab ... deixam de gerar atraso/saida antecipada”; uma pausa agora aumenta esse contador, embora o acusador não retire pontualidade. O exemplo ainda procura campos de entrada/saída inexistentes na linha da pausa. Fontes: `app/ponto/management/commands/diff_reclassificar_partido.py:188`, `app/ponto/management/commands/diff_reclassificar_partido.py:260`, `app/ponto/management/commands/diff_reclassificar_partido.py:262`, `app/ponto/management/commands/diff_reclassificar_partido.py:269`, `app/ponto/motor_calculo_v2.py:1713`.

3. **Nota — testes novos não cobrem essa integração com CXR-TELA.** O teste do espelho confere apenas `dia` e `causa` da lista em memória; não passa pela lavra C12 nem confere os campos/detalhe. Fonte: `app/ponto/tests/test_ata_pausa_l084_cxr.py:56`.

Mapa dos escritores:

- Campo público nasce no `ResultadoMes`; lista privada é zerada a cada cálculo e publicada no resultado. Fontes: `app/ponto/motor_calculo_v2.py:289`, `app/ponto/motor_calculo_v2.py:2326`, `app/ponto/motor_calculo_v2.py:2784`.
- Escritor L084 do dia: `{dia, causa='l084_do_dia', entrada/saida ISO, deltas em minutos}`. Fonte: `app/ponto/motor_calculo_v2.py:1634`.
- Escritor janela HE: `{dia, causa, real/cadastrado HH:MM, minutos_fora arredondados}`. Fonte: `app/ponto/motor_calculo_v2.py:1682`.
- Escritor pausa: `{dia, causa='l084_pausa', pares ISO, deltas em minutos}`; o laço passa explicitamente `data_turno`, inclusive para pausa civil D+1. Fontes: `app/ponto/motor_calculo_v2.py:1713`, `app/ponto/motor_calculo_v2.py:2781`.
- Escritor pontualidade L084 antigo: `{dia do mapa do juiz, entrada/saida ISO, deltas em minutos}`, sem `causa`. Fonte: `app/ponto/motor_calculo_v2.py:2035`.
- Composição de fatias concatena a lista, sem reclassificar causas. Fontes: `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:294`.
- Lavra CXR converte `dia` em `data`, filtra pela competência, copia campos permitidos, remove itens iguais e grava snapshot. Fontes: `app/ponto/services/cxr_na_lista.py:86`, `app/ponto/services/cxr_na_lista.py:87`, `app/ponto/services/cxr_na_lista.py:95`, `app/ponto/services/cxr_na_lista.py:107`, `app/ponto/services/cxr_na_lista.py:145`.

Mapa dos leitores:

- **E6:** lê qualquer `dia` não nulo, ignora `causa`/instantes/minutos e pergunta por pertença ao conjunto do dia. BATIDA permanece anterior a CADASTRO, portanto estar na lista não garante dono CADASTRO quando há batida ímpar/solta. Fontes: `app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/management/commands/e6_oraculo.py:195`, `app/ponto/management/commands/e6_oraculo.py:104`.
- **CXR-TELA:** conta itens, não dias; nova pausa aumenta `total`, `por_causa` e numerador lavrado legitimamente pelo rótulo declarado. Contudo perde os dados da pausa — achado 1. Fontes: `app/ponto/services/cxr_na_lista.py:14`, `app/ponto/services/cxr_na_lista.py:123`, `app/ponto/services/cxr_na_lista.py:126`, `app/ponto/services/cxr_na_lista.py:147`.
- **CXR lavrado para dono:** projeta os `data` dos itens em conjunto de datas; ignora causa e minutos. Fonte: `app/ponto/services/cxr_na_lista.py:178`.
- **C12:** lê a lavra e apresenta item por item; usa `data`, vínculo/template e dono lavrados. Fontes: `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:401`.
- **Espelho/pontas HE:** passa a lista a `pontas_do_relato`, que aceita apenas causas contendo “janela de HE”; pausa é ignorada. O resumo acima do teto aceita apenas “chegada fora da janela de HE”. Fontes: `app/ponto/services/espelho.py:741`, `app/ponto/janela_he.py:278`, `app/ponto/janela_he.py:281`, `app/ponto/services/espelho.py:1260`.
- **Diff janela:** exclui especificamente `l084_pausa`; não encontrei falha nova nesse filtro. Fonte: `app/ponto/management/commands/diff_janela_he.py:72`.
- **Fechamento/lavra de dinheiro:** o caminho por fatias transporta a lista composta; não encontrei persistência dela em `FechamentoMensal` nem consumo direto para rubricas. O próprio instrumento documenta que o fechamento não persiste a lista. Fontes: `app/ponto/services/fechamento.py:97`, `app/ponto/services/fechamento.py:294`, `app/ponto/management/commands/diff_reclassificar_partido.py:169`.
- **Calculador v3:** a captura de `_cadastro_x_realidade` em contexto de pontualidade vai para seu `Censo.cxr`, campo separado; não é segundo escritor do resultado público de produção. Fontes: `app/ponto/calculador/regras.py:275`, `app/ponto/calculador/periodo.py:283`.

`_l084_pausa_acusada`: escritor/leitor de dedupe no acusador, reinicialização por `calcular_mes`; não encontrei outro consumidor. Fontes: `app/ponto/motor_calculo_v2.py:1704`, `app/ponto/motor_calculo_v2.py:1707`, `app/ponto/motor_calculo_v2.py:1709`, `app/ponto/motor_calculo_v2.py:2327`.

Quanto a dia do turno versus civil, o novo laço passa `data_turno`; CXR e E6 conservam a chave recebida e não redatam a pausa pelo instante civil. Não encontrei conversão nova incorreta de unidade nesses leitores; a falha verificada é o descarte completo dos campos da pausa na C12. Fontes: `app/ponto/motor_calculo_v2.py:2781`, `app/ponto/services/cxr_na_lista.py:88`, `app/ponto/management/commands/e6_oraculo.py:170`.

## 08/11 09:25 · sessao c8d28fdc59f6 · FIO-E0-V4-E1 · verificador · origem=Code

**Parecer: REPROVA.** Há contraexemplo à igualdade entre o universo eleito pela porta e o universo de seus membros; além disso, o reuso admitido pelo juiz continua sujeito a leitores que tratam a data gravada como dia do turno. (`app/chamados/services/fio.py:1391`; `app/chamados/services/declaracao_texto.py:244`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`.)

Os cenários abaixo são contraprovas construídas sobre o código. Sua ocorrência em produção, execução integrada dos testes e números SQL observados estão **[nao verificado]**. [nao verificado]

1. **Campos e memo — procedente, gravidade alta quanto ao significado de `data_evento`.**

   | Campo/valor | Quem escreve | Quem lê e qual significado usa |
   |---|---|---|
   | `data_evento` | `_nascer` grava o parâmetro; `abrir_conversa` fornece dia civil; `reapontar_resolvedora` data a existente; `rechavear_data_evento` troca a data. (`app/chamados/models.py:2707`; `app/chamados/models.py:2824`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:959`.) | Busca/índice usam a data gravada; o fecho passa a derivar a identidade pelo juiz. C6, validação de destino, plantio e expiração continuam lendo a gravada como dia relevante. (`app/chamados/models.py:2663`; `app/chamados/models.py:3298`; `app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:336`; `app/chamados/juizes.py:1988`; `app/chamados/services/materializacao.py:344`; `app/chamados/veredito_lavrado.py:229`.) |
   | `batida_origem_id` | `_nascer` aceita explícita ou deriva do contexto; o `save` de inserção deriva quando ausente. (`app/chamados/models.py:2751`; `app/chamados/models.py:3254`.) | Identifica episódio no índice, busca, plano e agrupamento; `_dia_do_fato` carrega a Batida para perguntar seu dia ao juiz. É FK do episódio, não uma data. (`app/chamados/models.py:2632`; `app/chamados/models.py:3295`; `app/chamados/services/fio.py:1020`; `app/chamados/services/fio.py:1122`; `app/chamados/services/fio.py:1234`.) |
   | `instancia_marco` | `_chave_de_identidade` deriva do motivo e recusa explícita divergente; há backfill histórico. (`app/chamados/models.py:3243`; `app/chamados/models.py:3252`; `app/chamados/migrations/0051_decisao_pergunta.py:27`.) | Busca/índice/base do fecho usam posição do marco; `_dia_do_fato` usa seu preenchimento para reconhecer linha de marco. (`app/chamados/models.py:2663`; `app/chamados/models.py:3289`; `app/chamados/services/fio.py:1205`; `app/chamados/services/fio.py:1237`.) |
   | `celula_id` | Nascimento e reuso escolhem célula pela **data pedida**; rechaveamento escolhe pela nova data. (`app/chamados/models.py:2748`; `app/chamados/models.py:2782`; `app/chamados/models.py:2825`; `app/chamados/services/fio.py:956`; `app/chamados/services/fio.py:960`.) | C6 só aceita essa alimentação se `celula.data == data_evento`; outbox prefere a FK. `_dia_do_fato` não lê `p.celula.data`: passa `p.data_evento` ao conversor da célula. (`app/chamados/juizes.py:638`; `app/chamados/models.py:3047`; `app/chamados/services/fio.py:1239`.) |
   | `chamado_destino_id` | Nascimento/reuso em `_nascer`; reapontamento em `reapontar_resolvedora`. (`app/chamados/models.py:2825`; `app/chamados/models.py:2796`; `app/chamados/services/fio.py:868`.) | Âncora da listagem, validação da costura e julgamento do destino. O significado de FK de destino é consistente nesses caminhos. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:2786`; `app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:761`.) |

   **Não existe acordo global sobre `data_evento`.** O comentário novo declara chave gravada versus identidade derivada, mas `chave_do_dia` ainda se documenta como identidade, `_dia_de_turno_da_pergunta` devolve a gravada e o plantio combina a hora com ela. A declaração local não migrou esses leitores. (`app/chamados/services/fio.py:1219`; `app/chamados/services/fio.py:1198`; `app/chamados/juizes.py:332`; `app/chamados/services/materializacao.py:414`.)

   **Memo misto — não procedente a suspeita de colisão ou iteração incompatível.** Vizinhos usam chave de três elementos `(pk, chave_do_dia, bo)`; dia e colaborador usam chaves de dois elementos `('dia_do_turno', bo)` e `('colab', cid)`. Os consumidores acessam chaves específicas; não encontrei consumidor que percorra esse dicionário supondo valores exclusivamente de vizinhança. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1246`; `app/chamados/services/fio.py:1258`; `app/chamados/services/fio.py:1264`; `app/chamados/services/fio.py:1278`.)

2. **Propriedade fixa entre porta e membros — procedente, gravidade alta.**

   Contraexemplo: mesmo colaborador, motivo `gps_ausente`, nenhuma linha de acervo; alfa e beta são episódios distintos cujo juiz responde **18/10**. Cada episódio tem uma linha gravada em **10/09** e outra em **18/10**. Essa disposição cabe no índice: o horizonte é 21/09 e a unicidade do episódio só inclui linhas a partir dele. A premissa de duas batidas julgadas no mesmo dia está no teste novo. (`app/chamados/models.py:2313`; `app/chamados/models.py:3294`; `app/chamados/tests/test_e1_v13.py:59`.)

   | Consulta | Universo resultante |
   |---|---|
   | `universo_da_linha(A0)` ou `A1` | `{A0, A1}` |
   | `universo_da_linha(B0)` ou `B1` | `{B0, B1}` |
   | `universo_do_fato(colab, gps_ausente, 18/10, bo=None)` | `{A0, A1, B0, B1}` |

   A causa é concreta: sem episódio, `do_fato` traz as duas linhas gravadas em 18/10; ambas entram em `datadas`; o fecho preserva todas as sementes, sem exigir conectividade entre elas. Com episódio, a busca só liga o próprio episódio ao acervo sem episódio, inexistente neste cenário. Os resultados foram reproduzidos em memória com funções extraídas do código e ORM/juiz substituídos por stubs; integração e produção **[nao verificado]**. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1320`; `app/chamados/services/fio.py:1381`; `app/chamados/services/fio.py:1391`.)

   **Os consumidores realmente divergem:** `_candidata_reusavel` chama sem episódio; `grupos_de_colisao` detecta as duas chaves de episódio e expande cada uma por fecho individual, produzindo dois grupos. O dedup consome esses grupos; a migração e `_colisao_ao_datar` começam pelo universo da linha. (`app/chamados/services/declaracao_texto.py:244`; `app/chamados/services/fio.py:1479`; `app/chamados/services/fio.py:1507`; `app/chamados/management/commands/dedup_perguntas_fato.py:58`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:891`.)

   Há caminho de emissão sem episódio inferido: `gerar_perguntas_disputa` admite contexto sem `ultima_batida`, aplica seu dedup local e pode chegar a `abrir` com dia do contexto. Uma disputa nova não fica impedida pelas perguntas de outras disputas. A ocorrência desse cenário em produção está **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/services/disputa_emissao.py:327`; `app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:444`.)

   **Assert de dois dias por chamador real — não demonstrado.** O agrupamento expande sementes individuais; `_colisao_ao_datar` começa por uma linha; a migração entrega ao plano membros do universo encontrado. Não obtive contraprova desse assert nesses caminhos. (`app/chamados/services/fio.py:1507`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:1634`; `app/chamados/services/fio.py:1641`.)

3. **Porta e célula — dois achados procedentes, gravidade alta.**

   **Nascimento sem linha anterior não verifica o dia da batida pedida.** Se o episódio ainda não possui perguntas, `universo_do_fato` não encontra linha para julgar. A recusa exige uma linha recusada daquele episódio; ausente essa linha, nasce pergunta com a data pedida e célula dessa data. Portanto `abrir(D0, batida cujo juiz diz DIA)` pode criar em D0; uma chamada direta posterior igual pode recusar a própria linha pelo juiz. (`app/chamados/services/fio.py:1362`; `app/chamados/services/fio.py:1376`; `app/chamados/models.py:2766`; `app/chamados/models.py:2748`; `app/chamados/models.py:2824`.)

   O caller de emissão não garante igualdade: grade/contexto têm precedência, e o juiz da última batida só é consultado quando falta a data anterior. Envolver `_dia_p` em `dia_do_turno_da_celula` não confere essa igualdade: o conversor devolve o valor recebido. (`app/chamados/services/disputa_emissao.py:371`; `app/chamados/services/disputa_emissao.py:378`; `app/chamados/services/disputa_emissao.py:424`; `app/chamados/services/disputa_emissao.py:451`; `app/ponto/turnos.py:2933`.)

   **Reuso correto pelo juiz pode falhar na costura.** Tome alfa gravada em 10/09, juiz em 18/10, como no teste novo; acrescente destino de escopo DIA declarado em 18/10. O proxy do pedido passa. A porta elege alfa e prepara célula de 18/10, mas conserva `ex.data_evento=10/09`. A segunda validação lê essa data antiga e lança `ResolvedoraCruzada`, pois a diferença supera um dia. O teste de reuso não fornece destino. (`app/chamados/tests/test_e1_v13.py:200`; `app/chamados/tests/test_e1_v13.py:203`; `app/chamados/models.py:2737`; `app/chamados/models.py:2782`; `app/chamados/models.py:2786`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:2016`; `app/chamados/juizes.py:2023`.)

   Sem essa recusa, o estado persistido pode ficar com célula DIA e data gravada D0; C6 descarta a célula como alimentação e continua julgando pelo dia gravado. Logo atribuir a célula certa não reconcilia os leitores. (`app/chamados/models.py:2820`; `app/chamados/juizes.py:638`; `app/chamados/juizes.py:671`; `app/chamados/juizes.py:679`.)

   **Correção do inventário de callers:** os sítios de produção de `abrir` não passam `batida_origem_id` explicitamente; `_nascer` a infere. `reapontar_resolvedora` e `_colisao_ao_datar` não chamam `abrir`: usam a porta de existente. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:834`; `app/chamados/services/declaracao_texto.py:348`; `app/chamados/services/declaracao_texto.py:636`; `app/chamados/models.py:2751`; `app/chamados/services/fio.py:849`; `app/chamados/services/fio.py:868`.)

   **Não procede acusar automaticamente o agrupador de passar dia civil:** seu `localtime(...).date()` alimenta a classificação; a criação recebe `resolver_data_evento(...batida_origem=...)`, que consulta o juiz. (`app/chamados/services/disputa_emissao.py:1108`; `app/chamados/services/disputa_emissao.py:1114`; `app/chamados/services/disputa_emissao.py:1123`; `app/escala/servico_jornada.py:488`.)

4. **Juiz, derivação e relógio — não procedente como clonagem nova; inconsistência de leitores permanece.**

   `_dia_do_fato` delega a batida ao recorte do juiz registrado: `dia_do_turno_da_batida → dia_das_batidas → turnos_do_colab`. Não reproduz pareamento nem deriva dia do timestamp por conta própria. A ponte é a implementação expressamente descrita para linhas sem dia próprio. (`app/core/juizes.py:196`; `app/chamados/services/fio.py:1235`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/chamados/services/fio.py:1227`.)

   Os acréscimos de identidade/memo/contador não introduzem leitura direta de relógio. A catraca de calendário cobre `.date()` sobre timestamp e lookups `timestamp__date`; isso não detecta o conflito semântico entre o novo juiz e leitores antigos de `data_evento`. A correspondência histórica dessa catraca com `935c8d96` está **[nao verificado]**. (`app/chamados/services/fio.py:1229`; `app/chamados/services/fio.py:1268`; `app/chamados/services/fio.py:1410`; `app/ponto/tests/test_contract_no_batida_date.py:175`; `app/ponto/tests/test_contract_no_batida_date.py:203`; `app/chamados/juizes.py:336`.)

   Para marco, o código declara `p.data_evento` como dia de célula sem consultar a FK. Erro concreto adicional dependeria de marco com data gravada divergente da célula soberana; presença desse caso está **[nao verificado]**. (`app/chamados/services/fio.py:1237`; `app/chamados/services/fio.py:1239`.)

5. **Custo — procedente como amplificação, gravidade média; estouro de teto [nao verificado].**

   Considere **B** batidas existentes distintas examinadas, incluindo as alcançadas na ponte e recusadas:

   | Operação | Chamadas ao juiz pesado |
   |---|---|
   | Um fecho com memo novo | Uma por batida distinta examinada. |
   | Um `abrir` | Zero com busca vazia; caso contrário, B no universo/ponte/fecho compartilhado. |
   | Recusa `FatoEmOutroDia` | Mais uma para o episódio, pois o rótulo abre outro memo. |
   | `fecho_cruza_dias` | Soma dos B de cada fecho executado; não compartilha memo entre componentes. |

   Essas contagens decorrem das chaves por batida, do compartilhamento local e da criação de `_memo_t` separado. Batida inexistente custa a busca, mas não chama o juiz. (`app/chamados/services/fio.py:1230`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1339`; `app/chamados/services/fio.py:1392`; `app/chamados/models.py:2772`; `app/chamados/services/fio.py:1428`.)

   **Memo por batida não basta para compartilhar trabalho por turno/janela:** cada batida distinta reconstrói `dia_das_batidas(d−1,d+1)`, carregando turnos e batidas novamente, mesmo com janelas sobrepostas. (`app/ponto/turnos.py:2925`; `app/ponto/turnos.py:2903`; `app/ponto/turnos.py:2905`; `app/ponto/turnos.py:1863`; `app/ponto/turnos.py:1884`.)

   Os selos citados têm coberturas diferentes: chokepoint impõe tetos SQL; modal monta 330 perguntas sem episódio; `test_porta_chamado_e1` mede atos/no-ops de Chamado; cadeia/ordem verificam transação e precedência de travas, sem impor teto de consultas. Não são prova conjunta do novo custo da porta por episódio. (`app/ponto/tests/test_selo_chokepoint_escrita.py:397`; `app/chamados/tests/test_selo_teto_modal.py:68`; `app/chamados/tests/test_porta_chamado_e1.py:57`; `app/chamados/tests/test_e1_v13_cadeia.py:32`; `app/chamados/tests/test_e1_v13_ordem.py:59`.)

6. **Candidatas de `fecho_cruza_dias` — não procedente a falta de cobertura, sob as invariantes de nascimento.**

   Todo passo entre linhas distintas ocorre por chave gravada ou por episódio/motivo. O contador seleciona chave com múltiplas linhas e episódio com múltiplas chaves; assim cobre ambas as formas de aresta. Colaborador e instância são normalizados no nascimento. (`app/chamados/models.py:2630`; `app/chamados/models.py:2632`; `app/chamados/models.py:2636`; `app/chamados/services/fio.py:1416`; `app/chamados/services/fio.py:1421`; `app/chamados/models.py:3236`; `app/chamados/models.py:3243`.)

   Dois episódios distintos na mesma data, sem acervo, são candidatas excedentes, mas seus fechos não recusam vizinhos e não entram no resultado. A confirmação usa o fecho real. (`app/chamados/models.py:2632`; `app/chamados/services/fio.py:1428`; `app/chamados/services/fio.py:1431`.)

   O contador, porém, acrescenta membros quando existe **qualquer** recusa; não verifica novamente se sua causa foi outro dia, outra base ou ponte ambígua. Garantia irrestrita para acervo que viole as invariantes, ou ocorrência de falso positivo nesse acervo: **[nao verificado]**. (`app/chamados/services/fio.py:1340`; `app/chamados/services/fio.py:1431`.)

7. **Classes de reprovação e selos.**

   - **Segundo escritor fora da porta — não procedente no censo AST de `data_evento`.** Executei em memória o scanner existente: retornou somente `_nascer`, `reapontar_resolvedora` e `rechavear_data_evento`, sem sítio fora das portas. Isso verifica localização da escrita, não correção do significado. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:122`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:143`.)
   - **Ordem de trava — nenhuma inversão nova demonstrada.** `_nascer` trava Colaborador antes do universo; as leituras novas de Batida/Colaborador não pedem `FOR UPDATE`. Execução dos selos de ordem e concorrência: **[nao verificado]**. (`app/chamados/models.py:2724`; `app/chamados/models.py:2764`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1252`.)
   - **RED v13 sem mordida — não procedente contra a variante simples por data gravada.** O teste exige B0, gravada D0, dentro do fato DIA; voltar a decidir pela gravada a exclui. Também exige reuso de A0 em outra data e recusa de beta pedido em D0. (`app/chamados/tests/test_e1_v13.py:108`; `app/chamados/tests/test_e1_v13.py:121`; `app/chamados/tests/test_e1_v13.py:157`; `app/chamados/tests/test_e1_v13.py:198`.)
   - **Lacunas dos novos selos — procedente, gravidade média.** O cenário de união de episódios usa acervo como ponte; o teste de reuso omite destino. Não cobrem as contraprovas dos itens 2 e 3. (`app/chamados/tests/test_e1_v13.py:78`; `app/chamados/tests/test_e1_v13.py:203`.)
   - **Preservação byte a byte dos v5/v8/v10/v11, RED executado, mutante executado e passagem dos tetos:** **[nao verificado]**.
   - **Campo com dois significados não reconciliados — procedente, gravidade alta:** a identidade nova usa o juiz, enquanto destino, C6 e plantio continuam usando a data gravada como dia do fato. (`app/chamados/services/fio.py:1235`; `app/chamados/juizes.py:1988`; `app/chamados/juizes.py:336`; `app/chamados/services/materializacao.py:414`.)

## 08/11 09:21 · sessao 7f8fd01fd110 · - · - · origem=Ronald (tmux)

Procedente, alta: a porta ainda une componentes desconectados quando chamada sem episódio. Para `gps_ausente`, use A0/A1 do episódio alfa, gravadas respectivamente em 10/09 e 18/10, B0/B1 do episódio beta nas mesmas datas, ambos julgados como 18/10, sem linha de acervo. Por linha, os universos são `{A0,A1}` e `{B0,B1}`; `universo_do_fato(...,18/10,batida_origem_id=None)` retorna as quatro. A seleção sem episódio traz ambas as linhas de 18/10; `datadas` conserva ambas e `fecho_do_fato` retém todas as sementes independentemente da conectividade. `app/chamados/models.py:2636`, `app/chamados/models.py:2663`, `app/chamados/services/fio.py:1320`, `app/chamados/services/fio.py:1381`, `app/chamados/services/fio.py:1391`.

O chamador real `_candidata_reusavel` omite episódio. O agrupamento real detecta duas chaves de episódio e chama o universo de cada uma separadamente, produzindo dois grupos; portanto porta e backfill/dedup não compartilham o universo anunciado. `app/chamados/services/declaracao_texto.py:244`, `app/chamados/services/fio.py:1122`, `app/chamados/services/fio.py:1479`, `app/chamados/services/fio.py:1501`, `app/chamados/services/fio.py:1507`.

O cenário admite índice ativo: o horizonte é 21/09 e a unicidade de episódio só inclui linhas gravadas a partir dele; cada episódio possui uma linha dentro e outra fora. A premissa de dois episódios no mesmo dia do juiz existe no teste novo. `app/chamados/models.py:2313`, `app/chamados/models.py:3294`, `app/chamados/tests/test_e1_v13.py:59`.

Confirmei os resultados executando somente em memória as funções reais extraídas por AST, com ORM e juiz substituídos por stubs explícitos; execução integrada e presença desse cenário em produção são [nao verificado].

Não procedente para linhas conformes: o universo candidato do contador cobre toda aresta possível do `do_fato`: vizinhança pela chave gravada ou pelo mesmo episódio/motivo. Candidatas excedentes são confirmadas pelo fecho real, evitando contar dois episódios na mesma data sem acervo. `app/chamados/models.py:2630`, `app/chamados/models.py:2632`, `app/chamados/models.py:2636`, `app/chamados/services/fio.py:1415`, `app/chamados/services/fio.py:1421`, `app/chamados/services/fio.py:1428`.

Não encontrei sementes de dois dias vindas dos consumidores internos com grupos normalmente produzidos: o agrupamento parte pelos fechos individuais; `_colisao_ao_datar` começa por uma linha; a migração entrega ao plano membros do universo dessa linha. Outros estados ou concorrência que produzam o assert são [nao verificado]. `app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:1504`, `app/chamados/services/fio.py:1634`, `app/chamados/services/fio.py:1641`.

## 08/11 09:20 · sessao fb39c0000d12 · - · - · origem=Ronald (tmux)

Inventário por campo e achados:

| Campo | Escritores | Leitores / significado |
|---|---|---|
| `data_evento` | Nascimento datado e conversa: `app/chamados/models.py:2696`, `:2707`, `:2824`. Datar existente: `app/chamados/services/fio.py:838`, `:840`, `:870`. Rechaveamento: `app/chamados/services/fio.py:959`, `:965`. Proxy e restauração apenas em memória: `app/chamados/models.py:2737`; `app/chamados/services/fio.py:852`, `:861`. | Chave gravada de busca/índice: `app/chamados/models.py:2630`, `:2663`, `:3289`; `app/chamados/services/fio.py:1193`, `:1205`. Identidade agora derivada pelo juiz: `app/chamados/services/fio.py:1229`, `:1235`, `:1330`, `:1375`. Outros leitores ainda a tratam como dia do turno: `app/chamados/juizes.py:332`, `:336`, `:1988`; `app/chamados/services/materializacao.py:344`, `:414`; `app/chamados/services/pergunta_no_app.py:93`; `app/chamados/services/validacao.py:269`, `:446`; janela: `app/chamados/veredito_lavrado.py:229`; apresentação/divergência: `app/chamados/services/fio.py:87`. |
| `batida_origem_id` | `_nascer` aceita valor explícito ou deriva: `app/chamados/models.py:2751`, `:2753`. `save` de inserção deriva se ausente: `app/chamados/models.py:3226`, `:3254`. Derivador lê foto do nascimento no contexto e valida dono: `app/chamados/models.py:3269`, `:3273`. | Identifica episódio: `app/chamados/models.py:2632`, `:3295`; `app/chamados/services/fio.py:1020`, `:1122`. Alimenta juiz do dia: `app/chamados/services/fio.py:1229`, `:1234`. Forma chave do cache: `app/chamados/services/fio.py:1258`; candidatos do contador: `app/chamados/services/fio.py:1413`, `:1418`. Não representa data nem instante: é FK da batida cujo instante o juiz consulta (`app/chamados/models.py:2350`; `app/chamados/services/fio.py:1234`). |
| `instancia_marco` | Derivação/validação no nascimento: `app/chamados/models.py:3243`, `:3246`, `:3252`. Backfill histórico: `app/chamados/migrations/0051_decisao_pergunta.py:27`. | Posição do marco, derivada do motivo: `app/chamados/models.py:3234`. Chave de busca/índice: `app/chamados/models.py:2663`, `:3289`; `app/chamados/services/fio.py:1205`. Discriminador da linha de marco em `_dia_do_fato`: `app/chamados/services/fio.py:1237`. |
| `celula_id` | Nascimento pela data pedida: `app/chamados/models.py:2748`, `:2825`. Reuso pela data pedida: `app/chamados/models.py:2781`, `:2782`, `:2820`. Rechaveamento pela nova data: `app/chamados/services/fio.py:956`, `:960`, `:965`. | FK da célula da pergunta: `app/chamados/models.py:2323`. C6 verifica se célula corresponde ao `data_evento` antes de alimentar julgamento: `app/chamados/juizes.py:634`, `:638`. Outbox prefere FK e só usa `(colaborador,data_evento)` como fallback: `app/chamados/models.py:3034`, `:3047`, `:3050`. `_dia_do_fato` NÃO lê `p.celula.data`; passa a data gravada à conversão de célula (`app/chamados/services/fio.py:1237`, `:1239`). |
| `chamado_destino_id` | Nascimento: `app/chamados/models.py:2825`. Reuso na porta: `app/chamados/models.py:2796`, `:2820`. Porta de reapontamento: `app/chamados/services/fio.py:868`, `:870`. Simulações sem persistência: `app/chamados/models.py:367`; `app/chamados/services/fio.py:779`, `:783`. | Âncora da listagem, com fallback ao chamado da disputa: `app/chamados/services/fio.py:117`. Gate do destino: `app/chamados/models.py:2784`, `:2786`; `app/chamados/services/fio.py:856`. C6/encerramento/outbox: `app/chamados/models.py:310`, `:361`, `:3028`. Mesmo significado de FK de destino nesses leitores (`app/chamados/models.py:3191`, `:3197`; `app/chamados/services/fio.py:100`). |

**Procedente, alta: a identidade aceita pelo fecho continua podendo ser recusada pelo leitor do destino.** Cenário derivado da própria fixture: episódio alfa gravado `D0=2026-09-10`, juiz retorna `DIA=2026-10-18`; `abrir(...DIA, chamado_destino=chDIA)` encontra e elege alfa, mas conserva `ex.data_evento=D0` (`app/chamados/tests/test_e1_v13.py:200`; `app/chamados/tests/test_e1_v9.py:37`; `app/chamados/models.py:2777`, `:2782`, `:2820`). O proxy do pedido passa, depois o gate de `ex` compara a data gravada com o destino e lança `ResolvedoraCruzada` para esse intervalo superior a um dia (`app/chamados/models.py:2737`, `:2741`, `:2786`, `:2788`; `app/chamados/juizes.py:1988`, `:2007`, `:2016`, `:2023`). O teste novo de reaproveitamento não fornece destino (`app/chamados/tests/test_e1_v13.py:203`).

**Procedente, alta: a porta não valida o dia do episódio no nascimento sem linha preexistente.** A recusa requer universo vazio **e** uma recusada do mesmo episódio; sem linhas, chega ao `create` com a data pedida e célula dessa data (`app/chamados/models.py:2748`, `:2766`, `:2824`). Assim, um pedido `D0` para batida cujo juiz diz `DIA` pode nascer `D0`; a emissão seguinte pode recusar a própria linha pelo juiz (`app/chamados/services/fio.py:1375`, `:1380`; `app/chamados/models.py:2766`). Ocorrência em produção: [nao verificado].

**Callers concretos de `abrir`.** Há quatro sítios de produção; nenhum passa explicitamente `batida_origem_id`, que a porta infere do contexto (`app/chamados/services/disputa_emissao.py:444`, `:834`; `app/chamados/services/declaracao_texto.py:348`, `:636`; `app/chamados/models.py:2751`).

- `gerar_perguntas_disputa`: grade/contexto têm precedência; só consulta juiz da última batida se o fallback estiver vazio; o destino ainda pode fornecer `_data_ev` prioritária (`app/chamados/services/disputa_emissao.py:350`, `:371`, `:378`, `:391`, `:424`, `:451`). Contraexemplo de chamada possível: contexto de `gps_ausente` com `data=D0` e `ultima_batida=b1` de `DIA`; nasce com a divergência acima. O merge preserva chaves antigas ausentes no novo valor (`app/chamados/services/disputa_emissao.py:555`, `:556`). Existência desse contexto em produção: [nao verificado].
- Agrupador: o `localtime(...).date()` serve à classificação, mas o dia passado à criação é o retorno de `resolver_data_evento(...batida_origem=...)`; portanto, não procede afirmar que esse caller passa automaticamente dia civil (`app/chamados/services/disputa_emissao.py:1108`, `:1109`, `:1114`, `:1123`; `app/escala/servico_jornada.py:488`).
- Declaração: ambos passam dia da célula ao conversor registrado (`app/chamados/services/declaracao_texto.py:347`, `:349`, `:635`, `:637`).
- `reapontar_resolvedora` e `_colisao_ao_datar` **não chamam `abrir`**: usam a porta de existente, `fato_livre_para`, `universo_da_linha` e plano; a escrita ocorre no save de reapontamento (`app/chamados/services/fio.py:849`, `:850`, `:891`, `:904`, `:868`, `:870`).

**Procedente, média/alta: dois significados não foram reconciliados em todos os leitores.** `_dia_de_turno_da_pergunta` continua retornando `data_evento` cru para perguntas de fato e é consumido pelo julgamento do C6 e tranca (`app/chamados/juizes.py:332`, `:336`, `:375`, `:671`, `:763`, `:948`, `:1216`). A célula nova `DIA` também deixa de servir à dieta do julgamento se a data gravada permanecer `D0` (`app/chamados/models.py:2782`; `app/chamados/juizes.py:638`). A declaração no comentário novo de `_dia_do_fato` não altera esses leitores (`app/chamados/services/fio.py:1218`; `app/chamados/juizes.py:336`, `:1988`).

**Não demonstrado como erro autônomo: marco usar data gravada.** `_dia_do_fato` não lê a FK da célula; passa `p.data_evento` ao juiz da célula (`app/chamados/services/fio.py:1237`, `:1239`). Isso só produz dia errado se uma linha de marco tiver data gravada divergente do dia soberano pretendido; presença desse caso no dado dado: [nao verificado].

## 08/11 09:19 · sessao cc2c08306ed4 · - · - · origem=Ronald (tmux)

Achados de custo/selos, somente por inspeção:

1. **Procedente, média: memo por batida evita repetição dentro de um fecho, mas não evita recomputar a janela inteira para cada batida.** Cada `bo` novo faz `SELECT Batida` e chama o juiz; este executa `dia_das_batidas(d-1,d+1)`, que carrega turnos e batidas da janela. Duas batidas do mesmo turno pagam duas reconstruções sobre janelas sobrepostas. (`app/chamados/services/fio.py:1229-1236`; `app/ponto/turnos.py:2917-2926`; `app/ponto/turnos.py:2901-2907`; `app/ponto/turnos.py:1861-1888`.)

2. **Contagem de chamadas ao juiz:**
   - Por `fecho_do_fato`: uma por `batida_origem_id` distinta examinada nas sementes, vizinhos e explorações da ponte; inclui vizinhos recusados, pois `_dia_do_fato(q)` precede o filtro de base. (`app/chamados/services/fio.py:1326`; `app/chamados/services/fio.py:1339-1342`; `app/chamados/services/fio.py:1278-1286`.)
   - Por `abrir`: zero se `do_fato` vier vazio; caso contrário, uma por batida distinta examinada pelo universo/ponte/fecho, com o mesmo memo. A batida **pedida**, quando ainda não tem linha, não é julgada por esse caminho. (`app/chamados/services/fio.py:1362-1392`; `app/chamados/models.py:2751-2765`.)
   - Na exceção `FatoEmOutroDia`: **mais uma chamada** para o episódio recusado, pois o rótulo abre `_memo_t = {}` em vez de aproveitar o memo anterior. (`app/chamados/models.py:2771-2776`.)
   - Por `fecho_cruza_dias`: soma das batidas distintas de **cada fecho executado**, sem memo global. O contador chama `fecho_do_fato([p])` sem `_viz`; uma batida alcançada como vizinha recusada pode ser julgada de novo no componente seguinte. (`app/chamados/services/fio.py:1423-1432`; `app/chamados/services/fio.py:1317`.)
   - Número SQL efetivamente observado, tempo e estouro dos tetos: **[nao verificado]**.

3. **Não procedente: os selos citados não são todos selos de custo deste caminho.**
   - Chokepoint efetivamente compara consultas com teto, inclusive ramos FICA de 144/217. (`app/ponto/tests/test_selo_chokepoint_escrita.py:180`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`; `app/ponto/tests/test_selo_chokepoint_escrita.py:239-250`; `app/ponto/tests/test_selo_chokepoint_escrita.py:397-430`.)
   - Modal mede 330 perguntas, mas a fixture não fornece `batida_origem`; ela não representa o novo custo por episódio. (`app/chamados/tests/test_selo_teto_modal.py:68-79`; `app/chamados/tests/test_selo_teto_modal.py:81-97`.)
   - `test_porta_chamado_e1` mede no-op de atos de campos de **Chamado**, não `PerguntaDisputa.abrir`. (`app/chamados/tests/test_porta_chamado_e1.py:1-5`; `app/chamados/tests/test_porta_chamado_e1.py:57-62`; `app/chamados/tests/test_porta_chamado_e1.py:117-122`.)
   - `test_e1_v13_cadeia` captura SQL para detectar escrita sem trava na transação; `_ordem` verifica precedência de trava/escrita e mutantes do registro. Nenhum desses impõe teto de consultas. (`app/chamados/tests/test_e1_v13_cadeia.py:32-59`; `app/chamados/tests/test_e1_v13_ordem.py:59-82`.)

4. **Não procedente: RED v13 sem mordida contra a comparação por data gravada.** A fixture coloca B0 com `data_evento=D0` e batida beta; o teste exige B0 dentro do fato DIA. A variante simples que volta a decidir por `p.data_evento` necessariamente exclui B0 e falha esse assert. Também há reutilização de A0 gravada em outra data e recusa de beta pedido em D0. (`app/chamados/tests/test_e1_v13.py:108-124`; `app/chamados/tests/test_e1_v13.py:157-165`; `app/chamados/tests/test_e1_v13.py:198-208`.)

5. **Lacuna, média: execução explícita do mutante exigido pelo aval [nao verificado].** O arquivo possui testes semânticos que distinguem a variante, mas termina sem teste que instale o mutante `data_evento`; a execução RED/mutante não veio no dado. (`app/chamados/tests/test_e1_v13.py:116-124`; `app/chamados/tests/test_e1_v13.py:315-320`; aval anexo **[nao verificado quanto à execução]**.)

6. **Ordem de trava: nenhuma nova aquisição explícita no diff inspecionado.** `_nascer` continua travando o colaborador antes de chamar o universo; os acréscimos usam leituras comuns para Batida/Colaborador. O runtime completo e eventual regressão nos selos de ordem são **[nao verificado]**. (`app/chamados/models.py:2720-2724`; `app/chamados/models.py:2762-2765`; `app/chamados/services/fio.py:1234`; `app/chamados/services/fio.py:1248-1252`.)

7. **Preservação dos selos antigos:** o diff fornecido só declara mudanças em `models.py`, `fio.py` e `test_e1_v13.py`; não exibe alteração dos v5/v8/v10/v11. Identidade byte a byte com os commits e execução desses selos: **[nao verificado]**. [nao verificado]

## 08/11 04:49 · sessao 7108433fc78b · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**A v3 corrige a vacuidade anterior do C11 e unifica o instante usado pelo command e pelo serviço. A prova continua parcial: falta o estado “coberto, com lista vazia”, e o teste de relógio substitui justamente o espelho que precisaria verificar em profundidade.** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:417`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/ponto/services/cxr_na_lista.py:183`)

Suite Django completa e execução dos mutantes nessa suite: **[nao verificado]**. As contraprovas abaixo distinguem leitura estática de ensaio isolado em memória. [nao verificado]

**1) C11 chega ao dono; o mutante anterior agora é atingido.**

O caminho está conectado: `lista()` chama `_folga_nao_definida_sem_porta()`, esta chama `beco_em_lote()`, que constrói `InsumosDoDono` com `sem_motor=True, cxr=...` e chama `.dono(d)`. (`app/escala/services/cadastro_realidade.py:864`, `:573`, `:525`, `:531`)

A fixture verifica pergunta viva e ausência de porta; o teste exige exatamente o C11 daquele dia com dono `CADASTRO`, além de zero chamadas ao espelho e a `MotorBase.calcular_mes`. Uma exceção engolida por `lista()` retiraria o C11 esperado e falharia na asserção; o selo deixou de passar com o beco vazio. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:367`, `:371`, `:391`, `:393`; `app/escala/services/cadastro_realidade.py:866`)

Há uma distinção entre os dois casos:

- **Com lavra:** `_cxr` já contém o dia; `dono()` chega a `cxr()`, que devolve o conjunto carregado. (`app/ponto/management/commands/e6_oraculo.py:147`, `:163`, `:195`)
- **Sem lavra:** o juiz substituto devolve `ESTRUTURA`; `dono()` encontra `sem_motor=True` e devolve `None` antes de chamar `cxr()`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:374`, `:395`; `app/ponto/management/commands/e6_oraculo.py:189`)

Portanto, remover os dois argumentos de `beco_em_lote:526` agora leva ao espelho sob o juiz forçado, contrariando as asserções de conteúdo e chamadas. É conclusão pelo fluxo; execução desse mutante no runner completo: **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:526`; `app/ponto/management/commands/e6_oraculo.py:168`, `:190`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:391`)

**Existe mutação plausível sobrevivente aos dois cenários novos.** Antes da lógica atual de `InsumosDoDono.dono()`, introduzir:

```python
if self._sem_motor and self._cxr == set():
    self._cxr = None
    self._sem_motor = False
```

Isso introduz um fallback para uma lavra válida que respondeu “nenhum dia”. Os testes novos fornecem conjunto não vazio ou `None`; nenhum fornece conjunto vazio. Esse terceiro estado é produzido legitimamente por `cxr_lavrado()`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:386`, `:397`; `app/ponto/services/cxr_na_lista.py:183`)

No ensaio isolado em memória dos métodos atuais e dessa alteração, os dois cenários novos mantiveram `CADASTRO/0 chamadas` e `None/0 chamadas`; o cenário coberto vazio, com juiz real respondendo `ESTRUTURA`, passou de zero para **uma chamada ao espelho**. Isso demonstra a lacuna dos cenários; aprovação do mutante pela classe Django completa: **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:162`, `:182`)

Também falta uma asserção direta de passagem pela autoridade: substituir a expressão `ins[...].dono(d)` por uma decisão local baseada na presença do dia na lavra reproduziria as duas respostas esperadas. Seria um segundo juiz; os dois resultados finais, sozinhos, não distinguem essa implementação. (`app/escala/services/cadastro_realidade.py:531`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:398`)

**2) A fixture serve ao contrato do leitor, mas não representa uma lavra produzida integralmente naquele estado.**

O colaborador do beco é criado separadamente; as batidas da fixture base pertencem aos outros dois colaboradores. O teste injeta cobertura e item para o beco chamando `lavrar()` diretamente. Já `medir()` só percorre colaboradores encontrados em `Batida` dentro da competência. Assim, **esse snapshot não sairia de `medir()` com esse estado da fixture**. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:70`, `:77`, `:342`, `:386`; `app/ponto/services/cxr_na_lista.py:68`, `:71`)

A estrutura necessária ao leitor está correta: `cobertos` contém colaborador, competência e início; os itens contêm colaborador, competência e data. `cxr_lavrado()` usa precisamente esses campos. A escrita passa pela função canônica, embora o conteúdo seja montado pelo teste. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:386`; `app/ponto/services/cxr_na_lista.py:142`, `:176`, `:180`)

Há ainda uma artificialidade deliberada no juiz: o C11 exige `folga_nao_definida`, e o juiz real já responde `CADASTRO` por esse insumo, independentemente da lista do motor. O mock ignora isso para forçar a situação que exigiria consultar CXR. Logo, o teste sem lavra **não demonstra que um beco real dessa fixture receberia `None`**; demonstra o comportamento quando a lista seria necessária à decisão. (`app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:374`)

Isso dá mordida ao teste de proteção contra motor, mas limita seu alcance como prova de integração produtor → lavra → leitor. A correspondência com algum estado real de produção permanece **[nao verificado]**. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:384`)

**3) Relógio: datas de referência alinhadas; carimbo de persistência separado.**

| Trecho | Comportamento |
|---|---|
| Command | Lê `timezone.now()`, deriva `hoje` desse instante, passa `agora` a `medir` e `hoje` a `lavrar`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`, `:40`) |
| `medir` | Deriva `hoje` de `agora`; usa esse dia nas competências e passa os mesmos valores ao espelho e a `InsumosDoDono`. (`app/ponto/services/cxr_na_lista.py:60`, `:63`, `:78`, `:99`) |
| Janelas | `janela_atual/anterior` calculam a partir da data recebida e do corte da empresa; não consultam relógio. (`app/ponto/janelas.py:60`, `:66`) |
| Espelho | Recebe `agora`, passa-o a `montar_dias`; a autoridade do cálculo recebe datas. A chamada a `calcular_mes` não recebe parâmetro `agora`. (`app/ponto/services/espelho.py:1227`, `:1262`; `app/ponto/services/fechamento.py:286`) |
| `InsumosDoDono` | Guarda o instante recebido; uma eventual consulta ao espelho o reutiliza. Na lavra, recebe também o espelho já calculado. (`app/ponto/management/commands/e6_oraculo.py:132`, `:169`; `app/ponto/services/cxr_na_lista.py:99`) |
| Snapshot | `calculado_em` tem `auto_now=True`; `lavrar` não o preenche com o instante da borda. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:145`) |

**Sim:** no command atual, `data_ref`, o dia usado nas competências e o `agora` entregue ao espelho derivam do mesmo instante. **A expressão “agora do motor” é imprecisa:** o cálculo recebe datas; o instante explícito segue pelo espelho para a montagem da grade. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/ponto/services/cxr_na_lista.py:60`; `app/ponto/services/espelho.py:1266`; `app/escala/utils.py:2358`)

**Não há garantia de uma única leitura literal durante toda a persistência:** `calculado_em` é um carimbo automático de salvamento. Além disso, `ultimo_lavrado()` o publica como `medido_em`; esse campo não preserva o instante inicial passado ao espelho. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:160`)

O teste de meia-noite tem mordida contra o defeito anterior: exige que todos os argumentos `agora` observados sejam exatamente `t1`, e compara seus dias com `data_ref`. Uma leitura extra depois da primeira não apaga essa exigência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:412`, `:425`, `:427`)

Mas sua cobertura termina na entrada do espelho: ele substitui a função inteira e devolve lista vazia, impedindo também a criação de `InsumosDoDono` pelo serviço. Uma regressão dentro do espelho ficaria oculta desse teste; a catraca AST examina apenas o command e `cxr_na_lista`. Também não há asserção sobre `ini/fim`, e a virada escolhida é 05→06, sem mudança de competência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:417`, `:421`, `:447`; `app/ponto/services/cxr_na_lista.py:81`, `:98`)

**4) Quem escreve e quem lê.**

| Elemento | Escritor/origem | Leitores/destino |
|---|---|---|
| `medir(agora=)` | O command captura e fornece o instante. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`) | O serviço deriva `hoje` e alimenta espelho e insumos. (`app/ponto/services/cxr_na_lista.py:60`, `:78`, `:99`) |
| `lavrar(d, hoje)` | Recebe a medida e escreve `data_ini`, `data_ref`, contadores e `detalhe` via `update_or_create`. (`app/ponto/services/cxr_na_lista.py:142`) | `ultimo_lavrado()` recupera o snapshot; C12 formata seus itens; `cxr_lavrado()` fornece dias aos leitores do dono. (`app/ponto/services/cxr_na_lista.py:152`, `:174`; `app/escala/services/cadastro_realidade.py:393`, `:523`, `:1021`) |
| `cobertos` | `medir()` acrescenta a chave somente depois de concluir a leitura do colaborador/competência; falhas saem antes. (`app/ponto/services/cxr_na_lista.py:111`, `:118`) | `cxr_lavrado()` distingue ausência de cobertura (`None`) de cobertura sem dias (`set()`). (`app/ponto/services/cxr_na_lista.py:176`, `:183`) |
| `CLASSIFICAR` | Registro estático no teste; a entrada nova pertence a **`B2_menos_B1`**. (`app/core/tests/test_medida_turno_do_par.py:391`, `:452`) | O teste compara esse registro ao censo calculado pelo instrumento. Não é allowlist de execução. (`app/core/tests/test_medida_turno_do_par.py:577`; `app/core/medida_turno_do_par.py:436`) |

No censo textual da cópia, o chamador encontrado de `cxr_na_lista.medir` é o command já atualizado; não encontrei chamada desse serviço com `hoje=`. O `medir(hoje=...)` de `lavrar_teto_do_dia` pertence a outro serviço. Chamadas externas à cópia: **[nao verificado]**. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:17`, `:32`; `app/ponto/management/commands/lavrar_teto_do_dia.py:28`)

**5) Classes de reprovação.**

| Classe | Parecer sobre a cópia |
|---|---|
| Segundo juiz | O código atual continua chamando a autoridade existente. O mock limita a capacidade dos novos testes de detectar sua substituição por decisão local. (`app/escala/services/cadastro_realidade.py:531`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:374`) |
| Segundo escritor fora da porta | O command e o teste novo usam `cxr_na_lista.lavrar`. A escrita direta da célula é preparação de fixture. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:40`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:366`, `:386`) |
| Relógio fora da borda | A dupla leitura command/serviço foi removida. O carimbo automático permanece; os testes não auditam dependências transitivas. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/inteligencia/models.py:96`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:447`) |
| Derivação paralela | A conversão de instante em data usa a mesma entrada; a inclusão em `CLASSIFICAR` é informativa, não cria autoridade. (`app/ponto/services/cxr_na_lista.py:60`; `app/core/tests/test_medida_turno_do_par.py:12`) |
| Selo sem mordida | A vacuidade anterior foi fechada. Continua faltando o cenário de cobertura vazia no request e a verificação do espelho real pelo selo temporal. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:391`, `:417`; `app/ponto/services/cxr_na_lista.py:184`) |
| Campo com dois significados | `None` e conjunto vazio estão separados corretamente no código. `medido_em` representa `calculado_em`, não o instante inicial da medição. (`app/ponto/services/cxr_na_lista.py:160`, `:184`) |

A documentação de L-040 ainda afirma ausência de leitor, embora C12 leia a lavra na implementação. O desenho `escopo_id=None` com a constraint anterior também permanece. Fechamento dos requisitos de produção — col841, divergência zero na frota, idempotência concorrente e conservação das classes do vigia — **[nao verificado]**. (`app/docs/LEIS.md:166`; `app/escala/services/cadastro_realidade.py:871`; `app/ponto/services/cxr_na_lista.py:146`; `app/inteligencia/models.py:103`)

## 08/11 03:36 · sessao 5b1d37290c70 · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**O delta cura (a)–(d) por inspeção. Não encontrei novo defeito bloqueante nos trechos alterados e nos leitores dos campos. O aval completo de produção permanece [não verificado].** Evidências abaixo.

**1) Curas e mapa de leitura/escrita**

- **(a) Chave corrigida:** os dois chamadores passam `ini`; C11 conserva esse início e corta somente `fim`. Isso elimina a escolha equivocada pelo mês do fim cortado. [cadastro_realidade.py:526](app/escala/services/cadastro_realidade.py:526), [cadastro_realidade.py:568](app/escala/services/cadastro_realidade.py:568), [cadastro_realidade.py:1025](app/escala/services/cadastro_realidade.py:1025).
- **(b) Selo fortalecido:** o teste agora exige contagem `(0, 0)` das chamadas ao espelho e ao motor; capturar o `AssertionError` dentro da lista não basta para deixá-lo passar. [test_cxr_tela_le_o_motor.py:132](app/ponto/tests/test_cxr_tela_le_o_motor.py:132), [test_cxr_tela_le_o_motor.py:138](app/ponto/tests/test_cxr_tela_le_o_motor.py:138).
- **(c) Testes passam pela lavra:** “responde como o espelho” chama efetivamente `cxr_lavrado`; o conflito C9/C12 exige `CADASTRO` em `dono_do_dia`, com juiz sensível ao insumo CXR. [test_cxr_tela_le_o_motor.py:252](app/ponto/tests/test_cxr_tela_le_o_motor.py:252), [test_cxr_tela_le_o_motor.py:311](app/ponto/tests/test_cxr_tela_le_o_motor.py:311).
- **(d) Texto corrigido:** a docstring declara filtro, deduplicação e exclusão por erro; o rótulo conta itens. [cxr_na_lista.py:11](app/ponto/services/cxr_na_lista.py:11), [cxr_na_lista.py:131](app/ponto/services/cxr_na_lista.py:131).

| Papel | Escritor/leitor |
|---|---|
| Produz competência e cobertura | `medir`: competência pelo fim integral; cobertura `[pk, comp, início ISO]`. [cxr_na_lista.py:61](app/ponto/services/cxr_na_lista.py:61), [cxr_na_lista.py:115](app/ponto/services/cxr_na_lista.py:115) |
| Persiste e recupera | `lavrar` grava o dicionário em `detalhe`; `ultimo_lavrado` recupera-o. [cxr_na_lista.py:142](app/ponto/services/cxr_na_lista.py:142), [cxr_na_lista.py:156](app/ponto/services/cxr_na_lista.py:156) |
| Interpreta `cobertos` | `cxr_lavrado`: `(colab, início) → competência → conjunto de datas`. [cxr_na_lista.py:173](app/ponto/services/cxr_na_lista.py:173), [cxr_na_lista.py:180](app/ponto/services/cxr_na_lista.py:180) |
| Consome a chave nova | `beco_em_lote` e `um_dono_por_dia`. [cadastro_realidade.py:526](app/escala/services/cadastro_realidade.py:526), [cadastro_realidade.py:1025](app/escala/services/cadastro_realidade.py:1025) |
| Mostra C12 | `_cxr_do_motor` lê `itens`, sem interpretar `cobertos`. [cadastro_realidade.py:393](app/escala/services/cadastro_realidade.py:393) |

**Nenhum leitor produtivo encontrado ainda exige o par `[colab, comp]**`: o leitor estrutural exige três elementos. O par antigo aparece no teste negativo e resulta em `None`; portanto, snapshots antigos perdem cobertura até nova lavra, explicitamente. [cxr_na_lista.py:173](app/ponto/services/cxr_na_lista.py:173), [test_cxr_tela_le_o_motor.py:305](app/ponto/tests/test_cxr_tela_le_o_motor.py:305).

**2) Descartes versus aval**

Não promoveria os descartes funcionais a bloqueadores deste delta. Em particular, erro parcial fica contado, exclui cobertura e o comando publica antes de sair 2; sem cobertura, o ramo dependente de CXR retorna `None`, sem motor. [cxr_na_lista.py:108](app/ponto/services/cxr_na_lista.py:108), [lavrar_cxr_do_motor.py:37](app/ponto/management/commands/lavrar_cxr_do_motor.py:37), [e6_oraculo.py:189](app/ponto/management/commands/e6_oraculo.py:189).

**A pendência `l084_pausa` impede declarar comprovado o PRONTO (1).** O leitor copia a publicação; não consegue trazer uma acusação que o produtor não entregou. Isso não demonstra defeito novo no leitor nem autoriza recalcular na tela. Os sete dias efetivamente publicados e exibidos: [não verificado]. [cxr_na_lista.py:75](app/ponto/services/cxr_na_lista.py:75), [motor_calculo_v2.py:2719](app/ponto/motor_calculo_v2.py:2719).

**3) Outro ponto relevante ao aceite**

O PRONTO (5) exige cuidado: **nem toda saída de `d` vai para `e/dia_na_lista_dono_*`**. Dias reconhecidos como mesa do admin podem entrar antes em `fio_do_admin`; os demais dias presentes na lista chegam ao detalhamento de `e`. Portanto, afirmar fechamento somente em `e` sem comparar os destinos seria incorreto. [vigia_fio.py:654](app/chamados/services/vigia_fio.py:654), [vigia_fio.py:677](app/chamados/services/vigia_fio.py:677), [vigia_fio.py:684](app/chamados/services/vigia_fio.py:684). A comparação nominal antes/depois e sua soma: [não verificado].

Executei sete verificações puras da função de leitura, incluindo competências distintas, cobertura vazia, ausência e formato antigo: passaram. [cxr_na_lista.py:162](app/ponto/services/cxr_na_lista.py:162). Suite Django e selos da frota: [não verificado].

## 08/11 03:35 · sessao aab5013a25cc · - · - · origem=Ronald (tmux)

Não encontrei descarte que deva virar **defeito bloqueante novo** no delta; os pontos abaixo limitam a conclusão de aceite:

- **Erro parcial:** CXR exclui a competência com falha de `cobertos`, registra erro e publica as demais; o command grava e termina com código 2. É a mesma política do C9. Sem cobertura, `sem_motor=True` devolve `None` quando a lista do motor seria necessária, sem chamá-lo. `app/ponto/services/cxr_na_lista.py:108`, `app/ponto/services/cxr_na_lista.py:115`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `app/ponto/management/commands/lavrar_teto_do_dia.py:34`, `app/ponto/management/commands/e6_oraculo.py:189`.

- **Zero/não medido:** `ultimo_lavrado()` distingue ausência de medição, mas C12 só consome `itens`, perdendo essa distinção na apresentação. Isso permanece no molde já escolhido; não encontrei requisito específico novo que transforme o descarte em bloqueio do delta. `app/ponto/services/cxr_na_lista.py:154`, `app/escala/services/cadastro_realidade.py:393`.

- **Item sem dia:** nasce sem dono na lavra, e a ação continuará “a próxima lavra o traz”; próxima lavra não necessariamente resolverá enquanto o motor continuar sem dia. É limitação real já descartada, compartilhada pela apresentação por dono. A afirmação “sem_dia=0 na frota” não pode ser validada pelos anexos fornecidos. `app/ponto/services/cxr_na_lista.py:81`, `app/ponto/services/cxr_na_lista.py:84`, `app/escala/services/cadastro_realidade.py:759`; número de produção [não verificado].

- **Cron:** a lavra está declarada como elo após `vigia_pareador_celula`, com dependência de `processar_cartorio`; a leitura da manhã usa a lavra anterior por decisão documentada no código. Não há evidência nova para promover a enumeração incompleta do placar a bloqueio funcional deste delta. `app/config/crons.py:1757`, `app/config/crons.py:1765`.

- **`l084_pausa`/PRONTO (1):** os escritores examinados publicam `l084_do_dia`, as causas de janela ou item sem `causa`; C12 copia a publicação, portanto não pode produzir por si os sete exemplos ausentes. O atendimento nominal dos sete dias permanece [não verificado], sem caso fornecido que demonstre falha deste leitor. `app/ponto/motor_calculo_v2.py:1623`, `app/ponto/motor_calculo_v2.py:1670`, `app/ponto/motor_calculo_v2.py:1992`, `app/ponto/motor_calculo_v2.py:2719`, `app/ponto/services/cxr_na_lista.py:75`.

**Cuidado com o PRONTO (5):** não declarar genericamente que cada saída de `d` aparecerá em `e/dia_na_lista_dono_*`. O vigia encaminha previamente dias na lista com BATIDA/CADASTRO para `fio_do_admin`; somente os que permanecem sem outro balde podem entrar em `e/dia_na_lista_dono_*`. A nova C12 calcula o dono com o próprio espelho e pode devolver CADASTRO. A soma exigida precisa de comparação nominal antes/depois; ela não está nos anexos. `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:684`, `app/ponto/services/cxr_na_lista.py:96`, `app/ponto/management/commands/e6_oraculo.py:110`; fechamento da soma [não verificado].

## 08/11 03:35 · sessao 8fade55f0993 · - · - · origem=Ronald (tmux)

Não achei regressão concreta na chave nova nem leitor produtivo que ainda desempacote `cobertos` como par; o único leitor estrutural exige três elementos. `app/ponto/services/cxr_na_lista.py:173`.

Mapa dos campos:

- `medir()` produz `comp` pelo fim integral e grava `[pk, comp, ini.isoformat()]`; devolve `cobertos` no resultado. `app/ponto/services/cxr_na_lista.py:61`, `app/ponto/services/cxr_na_lista.py:115`, `app/ponto/services/cxr_na_lista.py:130`.
- `lavrar()` persiste o resultado inteiro em `detalhe`; `ultimo_lavrado()` recupera esse detalhe. `app/ponto/services/cxr_na_lista.py:142`, `app/ponto/services/cxr_na_lista.py:156`.
- `cxr_lavrado()` converte os triplos em `(colab, início) → competência` e busca os dias por `(colab, competência)`. Formatos antigos ficam sem cobertura, retornando `None`. `app/ponto/services/cxr_na_lista.py:173`, `app/ponto/services/cxr_na_lista.py:177`, `app/ponto/services/cxr_na_lista.py:180`.
- Os dois consumidores produtivos passam `ini`: C11 em `beco_em_lote()` e reconciliação em `um_dono_por_dia()`. `app/escala/services/cadastro_realidade.py:526`, `app/escala/services/cadastro_realidade.py:1025`.
- C11 preserva o início da janela e corta somente o fim; a reconciliação deriva a janela da data do item pela autoridade `janela_atual`. `app/escala/services/cadastro_realidade.py:568`, `app/escala/services/cadastro_realidade.py:569`, `app/escala/services/cadastro_realidade.py:1017`.
- `InsumosDoDono` recebe o conjunto ou `None`; sem cobertura e sem motor, o ramo que precisaria do CXR devolve `None`, sem fallback. `app/ponto/management/commands/e6_oraculo.py:147`, `app/ponto/management/commands/e6_oraculo.py:189`.
- C12 lê apenas `itens`, sem interpretar `cobertos`. `app/escala/services/cadastro_realidade.py:393`.
- O único par antigo explícito é a fixture negativa que exige `None`; os testes novos também distinguem os inícios das competências 10 e 11. `app/ponto/tests/test_cxr_tela_le_o_motor.py:302`, `app/ponto/tests/test_cxr_tela_le_o_motor.py:305`.
- O homônimo `cobertos` de `core/views.py` é um contador de postos, independente do snapshot CXR. `app/core/views.py:882`, `app/core/views.py:891`.

Execução dos testes e estado efetivo dos snapshots de produção: [não verificado]. [nao verificado]

## 08/11 03:19 · sessao d8ec781d1406 · CXR-TELA-LE-O-MOTOR · verificador · origem=Code

**Não aprovo a v1 como pronta.** Há publicação de lavra parcial como execução concluída, lacunas nos selos e ausência de prova do caso obrigatório de pausa. O caminho principal da lista, entretanto, passou a usar `sem_motor=True`. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:37`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:130`; `app/escala/services/cadastro_realidade.py:525`; `app/escala/services/cadastro_realidade.py:1024`)

Verificação estática, com contraprova da seleção de competência executada somente em memória. Resultado de suíte, sete dias de col841, equivalência na frota e fechamento da soma d→e: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado dos campos**

O percurso é motor → resultado do espelho → `medir` → `lavrar` → snapshot → formatador C12/portador do dono → lista/PDF/vigia. O resultado entregue pelo espelho vem da janela **visual**, `vis_ini..hoje`, e não exclusivamente de `apur_ini..apur_fim`. (`app/ponto/motor_calculo_v2.py:2719`; `app/ponto/services/espelho.py:1225`; `app/ponto/services/espelho.py:1227`; `app/ponto/services/espelho.py:1294`; `app/ponto/services/cxr_na_lista.py:71`; `app/escala/services/cadastro_realidade.py:393`)

| Campo | Quem escreve; quem lê; significado |
|---|---|
| Snapshot `chave`, `escopo`, `escopo_id` | `lavrar`: `cxr_do_motor`, `frota`, `None`. `ultimo_lavrado` procura chave/escopo, sem filtrar `escopo_id`. (`app/ponto/services/cxr_na_lista.py:137`; `app/ponto/services/cxr_na_lista.py:147`) |
| `data_ini`, `data_ref` | `lavrar` grava **a data da medição** nos dois; não são os limites das competências. `ultimo_lavrado` ordena por `data_ref` e a devolve como texto ISO. (`app/ponto/services/cxr_na_lista.py:138`; `app/ponto/services/cxr_na_lista.py:148`; `app/ponto/services/cxr_na_lista.py:153`) |
| `numerador`, `denominador`, `valor`, `detalhe` | `lavrar` grava respectivamente `total`, `universo`, `None` e o dicionário inteiro. Portanto numerador é quantidade de itens distintos; denominador é quantidade de colab–competência examinados, não dias nem colaboradores únicos. (`app/ponto/services/cxr_na_lista.py:65`; `app/ponto/services/cxr_na_lista.py:118`; `app/ponto/services/cxr_na_lista.py:139`) |
| `calculado_em`, `medido_em` | O modelo atualiza `calculado_em` ao salvar; `ultimo_lavrado` o expõe como `medido_em`. É carimbo da gravação, posterior à captura `_agora_do_ato`. (`app/inteligencia/models.py:96`; `app/ponto/services/cxr_na_lista.py:52`; `app/ponto/services/cxr_na_lista.py:152`) |
| `itens[].colab`, `empresa` | `medir` acrescenta os PKs do colaborador e de sua empresa na seleção. O formatador agrupa por `colab`; a filtragem final da lista usa a empresa **atual**, carregada por `_vinculos`, e não `itens[].empresa`. (`app/ponto/services/cxr_na_lista.py:61`; `app/ponto/services/cxr_na_lista.py:77`; `app/escala/services/cadastro_realidade.py:401`; `app/escala/services/cadastro_realidade.py:769`; `app/escala/services/cadastro_realidade.py:899`) |
| `itens[].competencia` | `medir` escreve `MM/AAAA` do fim da janela canônica. Formatador usa no detalhe; `cxr_lavrado` usa na chave de consulta. O corte vem da empresa; 21→20 é o default, não constante do serviço novo. (`app/ponto/services/cxr_na_lista.py:30`; `app/ponto/services/cxr_na_lista.py:57`; `app/ponto/services/cxr_na_lista.py:170`; `app/ponto/janelas.py:36`; `app/escala/services/cadastro_realidade.py:404`) |
| `itens[].data` | `medir` converte `dia` em ISO e exige intervalo **inclusivo** `ini <= d <= fim`; ausência de `dia` permanece `None`. Lista, reconciliação e vigia usam essa data como identidade do dia. (`app/ponto/services/cxr_na_lista.py:80`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:95`; `app/escala/services/cadastro_realidade.py:1003`; `app/escala/services/cadastro_realidade.py:1061`) |
| `ec`, `template_id` | Acrescentados pela lavra consultando o vínculo **do dia**. O formatador renomeia para `ec_do_dia`/`template_do_dia`; a ação C12 usa o template histórico. O agrupamento e o template mostrado na linha continuam sendo os de hoje. (`app/ponto/services/cxr_na_lista.py:94`; `app/escala/services/cadastro_realidade.py:402`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:782`) |
| `dono` | Acrescentado pela lavra mediante `InsumosDoDono.dono(d)`, usando o espelho já obtido. O formatador o transforma em `dono_do_dia`; a reconciliação pode posteriormente sobrescrevê-lo. (`app/ponto/services/cxr_na_lista.py:92`; `app/ponto/services/cxr_na_lista.py:96`; `app/escala/services/cadastro_realidade.py:403`; `app/escala/services/cadastro_realidade.py:1028`) |
| `causa`, `real`, `cadastrado`, `minutos_fora` | O produtor HE escreve causa, horários locais `HH:MM` e minutos arredondados. `minutos_fora` é a **sobra após extra declarada**, enquanto `cadastrado` continua sendo o marco cru. A lavra copia; o formatador imprime. (`app/ponto/motor_calculo_v2.py:1662`; `app/ponto/motor_calculo_v2.py:1670`; `app/ponto/motor_calculo_v2.py:1702`; `app/ponto/services/cxr_na_lista.py:79`; `app/escala/services/cadastro_realidade.py:394`) |
| `entrada_real`, `entrada_cadastrada`, `saida_real`, `saida_cadastrada` | Os produtores L-084 escrevem instantes ISO com offset, e não apenas `HH:MM`. Lavra copia como texto; formatador imprime sem conversão. (`app/ponto/motor_calculo_v2.py:1624`; `app/ponto/motor_calculo_v2.py:1998`; `app/ponto/services/cxr_na_lista.py:39`; `app/escala/services/cadastro_realidade.py:398`) |
| `delta_entrada_min`, `delta_saida_min` | Minutos com sinal, arredondados a uma decimal: entrada = real − prevista; saída = prevista − real. Logo saída posterior produz delta negativo. Copiados pela lavra e impressos pelo formatador. (`app/ponto/motor_calculo_v2.py:1973`; `app/ponto/motor_calculo_v2.py:1975`; `app/ponto/motor_calculo_v2.py:2002`; `app/escala/services/cadastro_realidade.py:399`) |
| `cobertos` | `medir` escreve `[colab, competencia]` somente após concluir aquela leitura, inclusive quando nenhum item sobra. `cxr_lavrado` distingue coberto vazio (`set()`) de não coberto (`None`). (`app/ponto/services/cxr_na_lista.py:104`; `app/ponto/services/cxr_na_lista.py:110`; `app/ponto/services/cxr_na_lista.py:166`; `app/ponto/services/cxr_na_lista.py:174`) |
| `fora_da_competencia` | Conta ocorrências descartadas pelo filtro, por leitura de competência. O command imprime; o produto C12 não expõe esse contador. Não representa quantidade de dias distintos. (`app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:109`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:33`; `app/escala/services/cadastro_realidade.py:393`) |
| `repetidos_do_motor` | Conta itens iguais **depois da projeção/conversão**, dentro de um colab–competência. O command imprime; a lista recebe somente o item deduplicado. (`app/ponto/services/cxr_na_lista.py:79`; `app/ponto/services/cxr_na_lista.py:100`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:33`) |
| `total`, `colabs`, `universo`, `sem_dia` | `medir` calcula itens distintos, colaboradores com itens, colab–competência selecionados e itens sem data. O command imprime. O contador C12 da tela é recontado sobre assinaturas efetivamente exibidas. (`app/ponto/services/cxr_na_lista.py:118`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:30`; `app/escala/services/cadastro_realidade.py:933`; `app/escala/services/cadastro_realidade.py:960`) |
| `por_competencia`, `por_dono`, `por_causa` | Agregações dos itens sobreviventes, escritas por `medir`; command imprime as duas primeiras. Causa ausente ganha o rótulo `l084_sem_causa` somente na agregação. (`app/ponto/services/cxr_na_lista.py:111`; `app/ponto/services/cxr_na_lista.py:115`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:32`) |
| `janelas`, `competencias`, `erros`, `erros_amostra`, `rotulo`, `onde_olha` | Escritos por `medir`, armazenados no detalhe. Command usa `erros` para retornar 2; formatador C12 lê apenas `itens`, sem levar cobertura, erro ou frescura à saída da lista. (`app/ponto/services/cxr_na_lista.py:58`; `app/ponto/services/cxr_na_lista.py:121`; `app/ponto/services/cxr_na_lista.py:126`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:39`; `app/escala/services/cadastro_realidade.py:393`) |
| `sem_medida` | Produzido por `ultimo_lavrado` quando não há snapshot; ignorado pelo formatador C12. (`app/ponto/services/cxr_na_lista.py:149`; `app/escala/services/cadastro_realidade.py:393`) |

`InsumosDoDono._cxr` nasce `None`, recebe conjunto de datas por injeção ou pelo espelho e é consultado por pertinência em `dono`. `_sem_motor` é escrito pelo construtor e lido nas duas guardas. Sem cobertura, outros insumos ainda podem decidir CADASTRO/BATIDA; apenas a resposta residual que dependeria de CXR fica `None`. (`app/ponto/management/commands/e6_oraculo.py:133`; `app/ponto/management/commands/e6_oraculo.py:146`; `app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/management/commands/e6_oraculo.py:189`)

| Assinatura | Escritor original de `dono_do_dia` |
|---|---|
| C9 | Serviço do teto escreve `dono`; formatador renomeia. (`app/ponto/services/teto_na_lista.py:86`; `app/escala/services/cadastro_realidade.py:379`) |
| C10 | Censo pergunta ao mesmo portador com os dias de fase da própria corrida e grava `dono_do_dia`. (`app/ponto/management/commands/censo_fase_12x36.py:287`; `app/ponto/management/commands/censo_fase_12x36.py:300`) |
| C11 | Beco pergunta ao portador durante a leitura; não é dono previamente lavrado diariamente. (`app/escala/services/cadastro_realidade.py:531`) |
| C12 | Lavra pergunta ao portador com o espelho da própria medição; formatador renomeia. (`app/ponto/services/cxr_na_lista.py:92`; `app/ponto/services/cxr_na_lista.py:96`; `app/escala/services/cadastro_realidade.py:403`) |

Todos podem ser sobrescritos por `um_dono_por_dia`: reaproveita C11 ou pergunta novamente ao juiz. Assim, na saída, `dono_do_dia` significa **dono reconciliado**, não necessariamente o dono originalmente gravado naquele snapshot. Ações, contadores e vigia leem esse resultado. (`app/escala/services/cadastro_realidade.py:1009`; `app/escala/services/cadastro_realidade.py:1026`; `app/escala/services/cadastro_realidade.py:1028`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:1063`)

**Não há significado temporal inteiramente uniforme:** HE escreve `entrada.date()`, enquanto L-084 escreve o dia atribuído pelo juiz. Além disso, motor usa `settings.TIME_ZONE`; lavra/portador usam timezone corrente. O default é São Paulo; divergência efetiva de fuso em produção é **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:1528`; `app/ponto/motor_calculo_v2.py:1997`; `app/ponto/motor_calculo_v2.py:73`; `app/ponto/management/commands/e6_oraculo.py:149`; `app/config/settings/base.py:93`)

**2. Caminhos de request e leitores da lista**

**No grafo da lista CxR, não encontrei caminho sobrevivente que execute o espelho/motor.** As duas montagens potencialmente perigosas usam `sem_motor=True`, cuja guarda antecede a chamada ao espelho. (`app/escala/services/cadastro_realidade.py:525`; `app/escala/services/cadastro_realidade.py:1024`; `app/ponto/management/commands/e6_oraculo.py:166`)

Caminhos localizados:

- View HTML/PDF → `lista`; PDF recebe os dados montados e renderiza. (`app/escala/views.py:487`; `app/escala/views.py:489`; `app/escala/services/cadastro_realidade.py:1067`)
- `lista` → C11 → `beco_em_lote` → portador sem motor. (`app/escala/services/cadastro_realidade.py:864`; `app/escala/services/cadastro_realidade.py:573`; `app/escala/services/cadastro_realidade.py:525`)
- `lista` → `um_dono_por_dia` → portador sem motor. (`app/escala/services/cadastro_realidade.py:885`; `app/escala/services/cadastro_realidade.py:1024`)
- `vigiar_fio_morto` → `ListaDoAdmin` → `lista` → `dias_na_lista`. (`app/chamados/management/commands/vigiar_fio_morto.py:167`; `app/chamados/services/vigia_fio.py:216`; `app/chamados/services/vigia_fio.py:217`)
- Commands `fechar_avisos_regua_na_lista` e `fechar_chamados_vinculo_na_lista` → mesma `lista`. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`)
- Consumidores externos do beco: pauta de competência exportada e reatador chegam ao mesmo serviço alterado. (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:105`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`)

Fora desse grafo, as próprias telas de espelho continuam chamando `espelho_do_colab`; portanto “zero motor em qualquer request do sistema” seria falso. (`app/ponto/views.py:158`; `app/api/views.py:1369`)

**3. Classes de reprovação**

- **Segundo juiz de dono:** não identifiquei duplicação da regra CADASTRO/BATIDA/ESTRUTURA; ela continua no juiz existente. Os novos ramos mudam a obtenção do insumo e a resposta desconhecida. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/ponto/management/commands/e6_oraculo.py:195`)
- **Segundo escritor:** o escritor localizado para a nova chave é `cxr_na_lista.lavrar`, chamado pelo command. Ele não escreve a lista interna do motor. A frase do modelo “escrito só por inteligencia.metricas” ficou documentalmente incompatível com esse padrão, que já é usado por C9. (`app/ponto/services/cxr_na_lista.py:134`; `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`; `app/inteligencia/models.py:82`; `app/ponto/services/teto_na_lista.py:113`)
- **Relógio fora da borda:** o command captura `localdate()` e o serviço captura outro `now()`. Caso: atravessar 20→21 entre as chamadas mantém competência/data_ref de ontem com instante de hoje. A cadeia também conserva o relógio próprio de C2 e outro no PDF. (`app/ponto/management/commands/lavrar_cxr_do_motor.py:28`; `app/ponto/services/cxr_na_lista.py:52`; `app/escala/services/cadastro_realidade.py:252`; `app/escala/services/cadastro_realidade.py:1076`)
- **Derivação paralela:** C12 não recalcula distância, mas filtra, projeta e deduplica a publicação. Portanto “item por item, sem filtro e sem conta” não descreve o código. O censo `CLASSIFICAR` acrescentado pelo diff é informativo, não certificado dessa equivalência. (`app/ponto/services/cxr_na_lista.py:11`; `app/ponto/services/cxr_na_lista.py:79`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/services/cxr_na_lista.py:100`; `app/core/tests/test_medida_turno_do_par.py:12`)
- **Ordem de trava:** o elo toma o `flock`, verifica conclusão própria e espera o predecessor antes de executar. Não identifiquei inversão nova nesse caminho; concorrência entre execuções manuais e cron não está provada pelo teste sequencial. (`bin/encadeado.sh:78`; `bin/encadeado.sh:82`; `bin/encadeado.sh:89`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:118`)
- **Pouso antes do pré-requisito:** o corte exige acusador de pausa no motor, e o golden da obra é `l084_pausa`; os produtores localizados nesta cópia são HE, `l084_do_dia` e L-084 sem causa. Os sete dias em produção permanecem **[nao verificado]**. (`app/docs/CORTES.md:304`; `app/docs/BACKLOG.md:775`; `app/ponto/motor_calculo_v2.py:1623`; `app/ponto/motor_calculo_v2.py:1670`; `app/ponto/motor_calculo_v2.py:1992`)
- **Campo com dois significados:** há `dia` civil versus dia do turno; `fim` completo versus fim cortado; dono original versus reconciliado. O segundo caso tem contraprova abaixo. (`app/ponto/motor_calculo_v2.py:1528`; `app/ponto/motor_calculo_v2.py:1997`; `app/escala/services/cadastro_realidade.py:569`; `app/ponto/services/cxr_na_lista.py:173`; `app/escala/services/cadastro_realidade.py:1028`)

Os testes **não mordem tudo o que anunciam**: [nao verificado]

- O selo sem motor arma `AssertionError`, mas a lista captura `Exception`, inclusive em C11. Uma chamada indevida nesse ramo pode ser engolida, C11 desaparecer e o teste continuar verde porque exige apenas C12 e um PDF válido. Não há `assert_not_called`. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:130`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:134`; `app/escala/services/cadastro_realidade.py:866`)
- “`cxr_lavrado` responde como o espelho” **não chama `cxr_lavrado`**: injeta manualmente o mesmo dia nas duas montagens. Não testa `cobertos`, filtro ou competência. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:284`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:286`)
- O teste real cobre HE 18:56×21:00; os demais usam publicações artificiais. Não certifica pausa nem os sete dias obrigatórios. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:92`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:154`)
- Idempotência compara quantidade de snapshots e `itens`; o selo PDF procura cada data esperada no texto, sem excluir datas extras. São verificações parciais das promessas “mesmo estado” e “mesmos dias”. (`app/ponto/tests/test_cxr_tela_le_o_motor.py:122`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:151`)

**4. Filtro, cobertura e equivalência para `dono()`**

**Os conjuntos completos não são iguais.** Para a mesma publicação, o espelho fornece todos os dias; a lavra conserva a interseção com a competência. Entretanto, para um dia consultado **dentro daquela competência**, o filtro sozinho preserva a pertinência. Descartar agosto não é contraexemplo suficiente para `dono(outubro)`. (`app/ponto/management/commands/e6_oraculo.py:170`; `app/ponto/services/cxr_na_lista.py:88`; `app/ponto/management/commands/e6_oraculo.py:195`)

Em `um_dono_por_dia`, a janela vem de `janela_atual(dia)`. Logo há equivalência de pertinência **condicionada à mesma publicação, cobertura e identidade de competência**; o código não prova equivalência entre a lavra diária e um espelho recalculado posteriormente. (`app/escala/services/cadastro_realidade.py:1017`; `app/ponto/services/cxr_na_lista.py:147`; `app/ponto/services/cxr_na_lista.py:174`)

**Contraexemplo efetivo da API:** em 25/10, C11 corta a janela corrente para 21/10–24/10 e passa esse `fim`. `cxr_lavrado` busca `10/2026`, embora a competência corrente termine em 20/11. Com itens 02/10 na competência 10 e 22/10 na 11, a consulta retorna `{02/10}` em vez de `{22/10}`. (`app/escala/services/cadastro_realidade.py:568`; `app/escala/services/cadastro_realidade.py:569`; `app/escala/services/cadastro_realidade.py:526`; `app/ponto/services/cxr_na_lista.py:173`)

**Isso não muda hoje o dono de C11:** o beco exige `folga_nao_definida=True`, e esse insumo já decide CADASTRO antes de CXR. É erro de contrato mascarado pela precedência atual. (`app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`)

Há ainda diferença de universo: cobertura exige batida no intervalo civil `[ini 00h, fim+1 00h)`, enquanto os portadores/autoridade carregam bordas e o juiz pode atribuir batidas à véspera. Assim, cobertura civil não prova cobertura de todos os dias de turno. Um caso real afetado em produção é **[nao verificado]**. (`app/ponto/services/cxr_na_lista.py:59`; `app/ponto/services/cxr_na_lista.py:61`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/espelho.py:1020`; `app/ponto/turnos.py:306`)

**5. Contratos do ENC e registros**

A declaração está integrada: gatilho `vigia_pareador_celula`, dependência `processar_cartorio`, estágio `auditoria`, `--apply` e papel `lavra`. O teto 900 limita a espera pelo predecessor, não a duração do próprio lavrador. (`app/config/crons.py:1765`; `app/config/crons.py:2232`; `bin/encadeado.sh:92`; `bin/encadeado.sh:102`)

Não falta registrar em `FORA_DE_PIPELINE`: `todas_as_entradas()` já inclui `ENCADEADOS`, e o contrato “command tem casa” usa essa fonte. O contrato de duração também alcança encadeados e exige ausência de `sched`. (`app/config/crons.py:1783`; `app/chamados/tests/test_contract_crons.py:220`; `app/core/tests/test_contract_crons_duracao_horario.py:43`)

**Falta cobertura no consumidor `pipeline_placar`:** ele enumera somente `CRONS`. Caso concreto: o novo lavrador nun

[truncado em 20000 de 23391 caracteres]

## 08/11 03:15 · sessao 326fdbbc8265 · - · - · origem=Ronald (tmux)

Mapeamento e achados para consolidação:

- O resultado consumido **não é recortado à competência pedida**: `espelho_do_colab` chama `autoridade_do_periodo(colaborador, vis_ini, hoje)` e devolve seu `resultado`; apenas outras partes visuais/totais recebem `apur_ini/apur_fim`. Portanto o filtro novo é uma transformação efetiva da publicação, não cópia literal. Fontes: `app/ponto/services/espelho.py:1225`, `:1227`, `:1229`, `:1294`; filtro em `app/ponto/services/cxr_na_lista.py:88`.

- **O campo `dia` mistura dia civil e dia do turno**: HE usa `entrada.date()` e publica `j['dia']`; L084 publica o mapa do juiz ou `_dt_b`. A lavra interpreta todos como mesmo `data`, competência e vínculo. Fontes: `app/ponto/motor_calculo_v2.py:1528`, `:1711`, `:1749`, `:1623`, `:1997`; `app/ponto/services/cxr_na_lista.py:80`, `:88`, `:94`. Caso possível: cauda `E03:00→S11:00` em 21/10 de jornada iniciada em 20/10, cadastro 21–09, saída acima do teto; a acusação HE usa 21/10 embora o turno possa pertencer a 20/10. A herança da jornada e o mapa existem no próprio motor: `app/ponto/motor_calculo_v2.py:2403`, `:2407`, `:2455`. Execução desse cenário completo `[nao verificado]`.

- **Não usar a remoção dos dias externos como contraexemplo isolado de `dono(d)`**: para mesma publicação e `d∈[ini,fim]`, o filtro preserva pertinência. `cxr()` retorna os dias de todo o espelho, enquanto a lavra guarda a interseção; a diferença fora da janela não muda a pergunta interna. Fontes: `app/ponto/management/commands/e6_oraculo.py:170`, `:195`; `app/ponto/services/cxr_na_lista.py:88`, `:170`, `:174`.

- **Contraexemplo concreto da API C11**: em 25/10, a janela corrente 21/10–20/11 vira 21/10–24/10; `cxr_do(cid,fim)` procura `10/2026`, embora o dia 22/10 pertença a `11/2026`. Com lavra coberta nas duas competências e item somente 22/10 na 11, a função devolve `[]` para fim24/10 e `['2026-10-22']` para fim20/11. Executei somente a função extraída por AST com `python3 -B`, sem Django ou escrita. Fontes: `app/escala/services/cadastro_realidade.py:569`, `:525`; `app/ponto/services/cxr_na_lista.py:173`. **Impacto no dono C11 é neutralizado hoje**: o chamador exige `folga_nao_definida`, e o juiz retorna CADASTRO antes de ler CXR. Fontes: `app/escala/services/cadastro_realidade.py:518`; `app/ponto/management/commands/e6_oraculo.py:98`.

- **Cobertura também usa outra geometria**: universo da lavra exige qualquer batida no intervalo civil `[ini00h, (fim+1)00h)`, enquanto Insumos e autoridade incluem bordas e atribuem dia pelo turno. Um colaborador com todas as batidas em 21/10, atribuídas ao turno20/10, pode ficar sem cobertura na competência10 apesar de existir dia20 no espelho. A ocorrência desse caso em produção `[nao verificado]`. Fontes: `app/ponto/services/cxr_na_lista.py:59`, `:61`, `:64`, `:110`; `app/ponto/management/commands/e6_oraculo.py:150`; `app/ponto/services/espelho.py:1019`; atribuição à véspera em `app/ponto/turnos.py:299`, `:306`.

- **Unidades e formatos**: HE publica `real/cadastrado` como `HH:MM`, `minutos_fora` inteiro arredondado e já descontada a extra declarada; `cadastrado` continua marco cru. L084 publica timestamps ISO locais com offset e deltas em minutos com uma decimal; entrada é `real-prevista`, saída é `prevista-real`. Fontes: `app/ponto/motor_calculo_v2.py:1670`, `:1662`, `:1702`, `:1742`, `:1624`, `:1974`, `:1976`, `:1998`. Fuso do motor é fixo `settings.TIME_ZONE`, configurado São Paulo; lavra e Insumos usam o timezone corrente. Divergência efetiva de timezone em produção `[nao verificado]`. Fontes: `app/ponto/motor_calculo_v2.py:28`, `:73`; `app/config/settings/base.py:93`; `app/ponto/services/cxr_na_lista.py:59`; `app/ponto/management/commands/e6_oraculo.py:149`.

- **Golden de pausa sem produtor demonstrado**: não há `l084_pausa` nem `acusar_pausa_l084` em Python de produção desta cópia, pela busca completa de `app`. Os escritores efetivos encontrados publicam `l084_do_dia`, HE e L084 sem causa; `resultado` recebe a lista no final. Fontes positivas: `app/ponto/motor_calculo_v2.py:1623`, `:1670`, `:1992`, `:2719`. O prerequisite é nomeado em `app/docs/CORTES.md:304` e o golden em `app/docs/BACKLOG.md:775`; o relato de commit anterior não prova que esse código esteja nesta cópia (`app/docs/RELATO.md:1778`). Existência dos sete dias em produção `[nao verificado]`.

- **Teste real não cobre pausa**: a fixture real e a asserção exercitam HE de entrada18:56 contra21:00; os demais casos publicam dados por stub. Não comprovam o golden `l084_pausa` nem equivalência completa com `cxr_lavrado`: o teste de equivalência passa uma lista manual diretamente a `InsumosDoDono`. Fontes: `app/ponto/tests/test_cxr_tela_le_o_motor.py:42`, `:76`, `:92`, `:154`, `:284`.

- **Falha parcial substitui snapshot completo antes de informar erro**: o comando grava a medida e só depois sai com código2; cada falha de colaborador remove seus itens e sua cobertura. Caso: snapshot bom tem C12 de A e B; rodada seguinte falha apenas B, grava somente A e a tela perde B. Fontes: `app/ponto/management/commands/lavrar_cxr_do_motor.py:37`, `:39`; `app/ponto/services/cxr_na_lista.py:104`, `:106`, `:110`; `ultimo_lavrado` escolhe o mais recente em `:147`.

_2845 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
