# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 22:17** · publicado em **05/11/2026 22:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 22:17 · sessao 916f5668e728 · - · - · origem=Ronald (tmux)

Leitores de `Ausencia.minutos` e impacto da troca, por leitura estática:

| Leitor | Regra decisiva | Quando 720 vira o previsto |
|---|---|---|
| **Fechamento: falta** | `minutos_de_falta_por_dia` lê ausências aprovadas dos tipos que descontam e rateia `(a.minutos or 0) / n`, usando a expansão inteira do registro como denominador; fechamento soma o mapa e divide por 60. `app/folha/export.py:87-92,113-122,138-143`; `app/ponto/services/fechamento.py:864-866,987` | **Sim**, muda `horas_falta` e o mapa por dia. Num registro de um dia, 720→420 equivale a 12h→7h; num range, muda cada parcela proporcionalmente. `app/folha/export.py:118-122`; `app/ponto/services/fechamento.py:864-866` |
| **Fechamento: ausência paga** | `if _a.tipo in Ausencia.TIPOS_PAGOS and _a.minutos:` soma crédito; mesmo predicado dá `continue` na aplicação de abono/supressão por cobertura. Não exige `<480`. `app/ponto/services/fechamento.py:665-669,684-692` | Positivo→positivo mantém o ramo “parcial”, mas reduz crédito. Atenção: `_min_parcial_pagos` só é acumulado nesse arquivo; o desconto efetivo de atraso usa `creditos_por_dia`. `app/ponto/services/fechamento.py:663-669,978-985` |
| **`efeitos_da_ausencia`** | Pago com minutos truthy: soma minutos se a data inicial estiver na janela; na cobertura, pula o abono/supressão do dia inteiro. Retorna também `dias_com_ausencia_de_dia_inteiro`. `app/ponto/services/efeito_ausencia.py:43-61` | **Sim**, diminui `min_parcial_pagos`; positivo→positivo mantém o `continue`. A classificação `dia_inteiro` pode mudar ao cruzar 480, pois vem de outro juiz. `app/ponto/services/efeito_ausencia.py:44-61`; `app/ponto/turnos.py:2580,2598-2600,2684-2690` |
| **`credito_parcial`** | Pago com minutos truthy e data inicial dentro da janela: soma integralmente em `out[a.data]`; aplica `usa=min(atraso_do_dia, crédito_do_dia)`, sem transportar sobra. `app/ponto/services/credito_parcial.py:23-26,47-58` | Crédito bruto **muda**. Atraso líquido só muda se o crédito novo deixar de cobrir o atraso daquele dia. Fechamento usa esse resultado; `DiaPago` aplica o mesmo cap. `app/ponto/services/fechamento.py:982-985`; `app/ponto/services/dia_pago.py:188-195` |
| **`turnos.cobre_parte_do_dia`** | Tipo em `parciais_vigentes`, um dia (`data_fim` nula ou igual à inicial), e **`0 < minutos < 480`**. O 480 é fixo, não vem da grade. `app/ponto/turnos.py:2580,2595-2600` | 720→420: **muda para parcial**, se o cadastro do tipo permitir e for um dia. 720→480/660: continua inteiro; 720→0: continua inteiro. Range maior que um dia continua inteiro. O catálogo é vivo. `app/ponto/turnos.py:2595-2600`; `app/ponto/catalogo/ausencias.py:336-358` |
| **Dia inteiro / pontualidade** | `ausencia_explica_o_dia_inteiro = tipo != 'falta' and not cobre_parte_do_dia`; mapa para motor só inclui efeito `ABONA`. `app/ponto/turnos.py:2668-2690` | Atestado de um dia 720→420 pode sair do mapa que explica o dia inteiro e, portanto, alterar o teto de pontualidade; fechamento entrega esse mapa ao fluxo. `app/ponto/turnos.py:2681-2690`; `app/ponto/services/fechamento.py:680-683` |
| **Grade / cobrança** | Grade pergunta `cobre_parte_do_dia`, grava `cobertura_parcial`; inteiro apaga células/orfãs, parcial preserva a pergunta e dispensa só marcos dentro do período declarado. Serviço de jornada retorna sem cobrança quando a ausência não é parcial. `app/escala/utils.py:1195-1212,1251-1274`; `app/escala/servico_jornada.py:340-345` | Cruzar de 720 para 1..479 pode **reativar cobrança do resto do dia**; sem `hora_ini/hora_fim`, período responde `sem_periodo`. `app/escala/utils.py:1251-1274`; `app/ponto/turnos.py:2632-2641`; `app/escala/servico_jornada.py:342-345` |
| **Abono materializado da grade** | `por_dia_da_grade` usa dias trabalho/ausência; `_pv` vem de `minutos_previstos`, `_coberto=min(realizado,_pv)`, abono da ausência=`_pv-_coberto`. **Não lê diretamente `Ausencia.minutos`.** `app/ponto/services/dia_pago.py:384-400` | A simples substituição do campo não troca diretamente esse número por novo valor; ele continua vindo do previsto e realizado da grade. Efeitos indiretos por mudança de classificação/julgamento dependem dos dados. `app/ponto/services/dia_pago.py:385-399`; `app/escala/utils.py:1251-1274` |
| **TXT: 8792** | Obtém datas de falta pelo catálogo e expansão; `montar_bloco_falta` emite **`len(datas)`**, com um registro 11 por data. `app/folha/export.py:157-185,215-221,732-736` | **Não muda diretamente** por 720→previsto, mantendo tipo/status/datas. A rubrica de falta aqui conta dias, não horas do campo. `app/folha/export.py:157-185,218-220` |
| **8932 / dias abonados** | `datas_abonadas` expande datas dos tipos abonados, “contagem crua”, sem ler minutos. A busca no arquivo encontrou definição e nenhum chamador vivo de `datas_abonadas` dentro dele. `app/folha/export.py:193-206` | Datas/contagem não mudam com minutos. Uso efetivo da função fora desse arquivo: **[não verificado]**. `app/folha/export.py:201-206` |
| **TXT: 8069** | Eventos usam atraso+saída do fechamento em `rubrica_atraso_parcial`; atraso do fechamento já recebe abatimento do crédito. `app/folha/export.py:449-452`; `app/ponto/services/fechamento.py:982-986` | **Pode mudar indiretamente**, quando a redução do crédito aumentar atraso líquido após apuração. `app/ponto/services/credito_parcial.py:54-58`; `app/ponto/services/fechamento.py:985`; `app/folha/export.py:451-452` |
| **Rótulos do espelho / calendário / histórico** | `rotulo_de` passa minutos a `palavra_do_dia`; desconto imprime `Falta (desconta Xh)`; abono imprime `Atestado Xh (abonado)`; rejeitada/suprimida/pendente não usam o número no rótulo. `app/ponto/services/dia_decidido.py:148-155,217-235,484-489` | **Sim** no rótulo aprovado de desconto/abono: 12h→horas do previsto. Zero elimina o número. `app/ponto/services/dia_decidido.py:219-235` |
| **Espelho: totais** | Tela e PDF aplicam `folha_manda`; por competência, lê fechamento ou lavratura. Lavratura fornece `horas_falta`, `horas_abono=minutos_abonados/60`, `dias_falta=len(datas)`; não é simples soma do campo na apresentação. `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/pdf_espelho.py:628`; `app/relatorios/cartao_pela_celula.py:167-190,320-346` | Totais acompanham o fechamento/lavratura que forem recalculados; contagem de dias de falta não muda só por minutos. PDF ainda acumula bruto `min_parcial_pagos` em `horas_abono` antes da substituição pelo resumo da folha quando aplicável. `app/relatorios/pdf_espelho.py:332-336,478-479,628`; `app/relatorios/cartao_pela_celula.py:320-346` |
| **API GET ausências** | Serializa diretamente `'minutos': a.minutos` nas últimas 30 ausências. `app/api/views.py:1861-1863,1869-1880` | **Sim**, o JSON apresenta o novo número. `app/api/views.py:1878` |
| **Lista web de ausências** | View calcula `a.cobre_parte` pelo juiz de 480; template mostra dias_corridos>1 primeiro, depois minutos se parcial, senão “1 dia” ou fim aberto. `app/ponto/views.py:2233-2243`; `app/templates/ponto/ausencias.html:159-165` | 720→420 pode mudar “1 dia” para “420min” em tipo parcial de um dia. Para falta, o catálogo congelado declara parcial=False, mas o juiz consulta cadastro vivo. `app/ponto/catalogo/ausencias.py:88,336-358`; `app/ponto/turnos.py:2595-2600`; `app/templates/ponto/ausencias.html:163-165` |
| **Drawer / formulário** | Mostra minutos se truthy; input recebe valor; bloco período parcial inclui `a.minutos`. `app/templates/ponto/partials/painel_ausencia.html:125,149,296-313` | **Sim**, número exibido/preenchido muda; pode aparecer bloco parcial se cruzar o critério de 480. `app/templates/ponto/partials/painel_ausencia.html:125,149,296-313`; `app/ponto/turnos.py:2598-2600` |
| **Admin / propriedade horas / CSV lote** | Admin lista `minutos`; `Ausencia.horas=round(minutos/60,2)`; histórico transporta minutos no cartão, e CSV lote imprime `x['minutos']`. `app/ponto/admin.py:44`; `app/ponto/models.py:864-866`; `app/ponto/services/dia_decidido.py:504-512,590-613`; `app/ponto/services/atestados_lote.py:118-131` | **Sim** nos números. Dias do histórico vêm das datas ou `dias_corridos`, portanto não mudam por isso. `app/ponto/services/dia_decidido.py:608-614` |

**Ponto central:** não há uma única definição de parcial: crédito/efeito usam **pago + minutos truthy**, enquanto cobertura usa **tipo parcial vivo + um dia + 0<minutos<480**. Um atestado 720→420 altera ambos os valores e a classificação de cobertura; 720→480 altera o crédito, mas mantém cobertura de dia inteiro. `app/ponto/services/credito_parcial.py:24-25`; `app/ponto/services/efeito_ausencia.py:44-52`; `app/ponto/turnos.py:2580,2598-2600`.

**`>=720`:** no caminho de apresentação de `Ausencia`, a comparação foi removida: comentário registra a regra antiga e a view atual delega ao juiz. A comparação viva `duracao_minutos >=720` encontrada em `justificativa.py` é da duração de uma **Justificativa**, não leitura de `Ausencia.minutos`. `app/ponto/views.py:2233-2240`; `app/ponto/services/justificativa.py:156-162`.

## 05/11 22:17 · sessao 721fa8d91626 · - · - · origem=Ronald (tmux)

5. **Molde existente, mas sem backfill específico de `minutos>0` identificado.**

- `curar_ausencia_sem_fim` seleciona a anomalia (`dias_corridos>1`, fim nulo, tipo não aberto), apresenta dry-run e, no apply, passa cada ausência pelo serviço e registra antes/depois: `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-53`.
- O serviço `rederivar_fim` apenas chama `ausencia.save()`, `refresh_from_db()` e retorna; portanto não corrige os 720 positivos, pois o modelo só deriva falta aprovada com minutos falsos/zero: `app/ponto/services/ausencia.py:1277-1282`; `app/ponto/models.py:877-887`.
- Há backfill histórico de **estado**, com registro por ausência em `LogAuditoria` e função reversa: `app/ponto/migrations/0057_a4_backfill_pendente.py:35-52`, `:58-65`. A reversa volta estados para `pendente`; não é snapshot/restauração exata de minutos: `app/ponto/migrations/0057_a4_backfill_pendente.py:58-65`.
- Há snapshot/restauração executável de **FechamentoMensal e DiaPago**, útil como molde de inventário/serialização/restore, mas não de Ausencia: `bin/snapshot_relavratura_10_2026.py:20-26`, `:28-64`; `bin/restore_relavratura_10_2026.py:69-86`.
- Há backfill de insumos de ausência **na CelulaDia**, dry por padrão, que chama o juiz vivo e a porta de carimbo; não altera `Ausencia.minutos`: `app/ponto/management/commands/backfill_b5_insumos.py:51-84`.
- `LogAuditoria` dispõe de JSON antes/depois e ator do sistema; `evento` grava usando atomic próprio e é best-effort, podendo retornar `None` após falha: `app/core/models.py:295-307`; `app/core/observ.py:94-105`, `:124-139`. Logo, log best-effort não é garantia de snapshot restaurável — inferência desses trechos.
- Não localizei comando/serviço que já faça exatamente a troca dos 720 positivos pelo previsto nem snapshot/restore específico de Ausencia; inexistência absoluta: **[não verificado]**.

6. **Zeros, `None` e contraexemplos de segunda execução.**

- O helper `minutos_previstos_do_dia` retorna **0**, e não `None`, quando recebe lista vazia de escalas, não encontra vínculo, falta tipo, `eh_dia_trabalho(...) is not True` ou faltam entrada/saída; no caminho restante retorna `span`: `app/escala/utils.py:2277-2295`, `:2341`.
- `eh_dia_trabalho` pode retornar `None`: a célula existente manda por `cel.trabalha`; sem célula, usa cálculo: `app/escala/models.py:1087-1096`. O helper converte também esse `None` em 0 pela condição `is not True`: `app/escala/utils.py:2287-2288`.
- **Sem escala:** `_esc_vigente_do_dia` retorna `None` sem vínculos; o helper devolve 0: `app/escala/utils.py:2208-2209`, `:2281-2283`.
- **Folga:** célula existente com `trabalha=False` vence; sem célula, foto do mês contendo a data dá `base=False`: `app/escala/models.py:1094-1096`, `:1309-1317`; helper retorna 0: `app/escala/utils.py:2287-2288`.
- **Feriado:** no cálculo sem célula, só vira folga se `base is True` e `feriado_vira_folga` disser sim; esse predicado exige ciclo weekday autorizado, `trabalha_em_feriado=False` e feriado no lugar do vínculo. Não transforma automaticamente o plantão 12x36 em folga: `app/escala/models.py:1367-1371`, `:1389-1395`.
- **12x36:** com âncora individual posterior a 1940 e sem foto do mês, trabalha quando `delta % 2 == 0`; outro dia dá False: `app/escala/models.py:1289-1292`. O template 12x36 sozinho retorna `None`, pois não fornece fase: `app/escala/models.py:824-829`. Com foto, uma folga explícita dá False; fora dela a paridade da folga mais próxima pode decidir: `app/escala/models.py:1309-1317`, `:1351-1364`.
- **Template incompleto:** sem `hi` ou `hf`, previsto=0. Com ambos, span cruza meia-noite somando 1440 se diferença ≤0; desconta pausas declaradas, com fallback ao par `hii/hfi`: `app/escala/utils.py:2290-2295`, `:2312`, `:2326-2341`. Igualdade hi==hf significa 1440 antes das pausas, não 0: inferência de `:2293-2295`.
- **Salvar igual continua sendo escrita:** `Ausencia.atualizado_em` tem `auto_now=True`; `save()` termina incondicionalmente em `super().save`; o post-save chama o cartório mesmo sem comparar alterações: `app/ponto/models.py:813-814`, `:908`; `app/ponto/signals.py:143-147`. Sem guarda de igualdade, segunda execução pode atualizar timestamp e repetir efeitos — inferência desses trechos.
- **Filtro só `minutos=720` não prova pendência:** uma jornada legítima pode resultar em 720 e continuar elegível após a primeira execução; é necessário comparar atual com previsto antes de gravar — inferência do cálculo de span em `app/escala/utils.py:2293-2341`.
- **Insumos mutáveis:** célula pode mudar `trabalha`, DNA, geradora; sem DNA de marcos, usa template vivo; vínculo e folgas também participam. Outra execução pode calcular outro previsto: `app/escala/utils.py:2280-2290`; `app/escala/models.py:1094-1096`, `:1124-1133`, `:1280-1288`. Não há garantia de estabilidade entre execuções — inferência desses leitores.
- **Zero calculado antes pode ser recalculado dentro do save:** falta aprovada com `minutos=0` entra novamente no helper, em vez de preservar necessariamente o zero previamente calculado; insumos concorrentes podem produzir outro resultado: `app/ponto/models.py:877-887`.
- **Ordem interna pode mudar o alvo na segunda execução:** o save calcula minutos usando `data_fim or data` **antes** de derivar `data_fim` a partir de `dias_corridos`. Legado com `dias_corridos>1` e fim nulo pode somar só um dia na primeira passada e passar a ter range na segunda: `app/ponto/models.py:882-887`, `:893-907`. Isso importa especialmente se cada execução zerar minutos para forçar derivação — inferência desses trechos.

## 05/11 22:17 · sessao 973d2c701257 · - · - · origem=Ronald (tmux)

1. **Escritores e possibilidade de substituir `720`**

| Caminho | Comportamento na linha existente |
|---|---|
| `Ausencia.save` | Deriva **somente falta aprovada com minutos falsy**; positivo informado é preservado e pode ser salvo diretamente. Não deriva atestado. `app/ponto/models.py:872-887,908`. |
| `criar_ausencia` | Entrada `minutos>0` recusa tipo não parcial, salvo `checar_parcial=False`; cria objeto novo com o valor e chama save. Existente pode simplesmente retornar, sem atualização. `app/ponto/services/ausencia.py:206-209,246-252,294-301,311-314,341-345,385,417-418`. |
| `garantir_ausencia` | Encaminha `se_existir='retornar'`; existente volta intacta. `app/ponto/services/ausencia.py:453-462,311-314`. |
| `editar_ausencia_registro` | Pode sobrescrever positivo, mas **valor positivo recusa tipo não parcial**; `minutos=None/0` zera e deixa save rederivar apenas falta aprovada. Ainda revalida tipo/sobreposição e reescreve datas/range. `app/ponto/services/ausencia.py:473-494,496-508`. |
| `mudar_estado` | Estado igual retorna sem gravar. Alteração inclui minutos no `update_fields`, porém não recebe valor nem zera o anterior; positivo fica. `app/ponto/services/ausencia.py:801-819`. |
| `decidir` | Chama mudar_estado e grava carimbos/descrição/período; não recebe minutos. `app/ponto/services/ausencia.py:881-911`. |
| `rederivar_fim` | Apenas `save()` + refresh; positivo anterior permanece. `app/ponto/services/ausencia.py:1277-1282`. |
| Materialização | Agora omite minutos: falta aprovada deriva da grade; atestado fica **0 = dia inteiro**. Se já existe mesmo tipo/dia, porta retorna sem atualizar. `app/chamados/services/materializacao.py:484-492,519-528`; `app/ponto/services/ausencia.py:294-301`. |
| Férias | Só cria dia ausente com minutos=0; dia existente é ignorado. `app/ponto/services/ausencia.py:1182-1191`. |
| Django admin | Somente leitura: AusenciaAdmin herda `SoLeitura`; add/change/delete False e campos readonly. `app/ponto/admin.py:37-48`; `app/core/admin.py:59-75`. |

Trechos decisivos:

```python
if self.status == 'aprovada' and self.tipo == 'falta' and not self.minutos:
    ...
    self.minutos = _tot
super().save(*args, **kwargs)
```

`app/ponto/models.py:877-908`.

```python
if minutos and tipo not in _CATV.parciais_vigentes():
    raise AusenciaInvalida(..., codigo='tipo_nao_parcial')
...
ausencia.minutos = int(minutos or 0)
ausencia.save()
```

`app/ponto/services/ausencia.py:480-508`.

Portanto, `a.minutos = previsto; a.save(update_fields=['minutos', ...])` é tecnicamente aceito pelo modelo mesmo sobre positivo; **não encontrei serviço dedicado de correção de minutos que preserve todos os outros campos e dispense a restrição de tipo parcial**. A porta de edição é o escritor disponível, mas revalida/regrava o cadastro e pode recusar faltas/atestados conforme catálogo vivo. `app/ponto/models.py:877-908`; `app/ponto/services/ausencia.py:465-515`.

A restrição parcial é de cadastro vivo: a semente declara falta e atestado não parciais, mas não prova o cadastro atual sem banco. `app/ponto/catalogo/ausencias.py:17-23,88,95`; cadastro atual [não verificado].

A view de edição registra minutos antes/depois; o serviço editar por si só não grava essa trilha de minutos. `app/ponto/views.py:2770-2771,2794-2814`; `app/ponto/services/ausencia.py:496-515`.

3. **Competência entregue/exportada**

A autoridade é **`folha.export.competencia_entregue(empresa_id, mes, ano)`**, não `folha/porta_export.py` nem `FechamentoMensal.status`: ela pede `ExportacaoDominio` vigente e exige `invalidada_em is None`. `registro=True` devolve o próprio registro. `app/folha/export.py:556-586`.

Vigente significa mais recente por `(gerado_em, pk)` em empresa/mês/ano; não é simples `ExportacaoDominio.objects.exists()`, porque vigente revogado sem substituta não entrega. `app/folha/models.py:192-225`; `app/folha/export.py:564-586`.

Para **(colaborador, data)** há composição pronta:

```python
_ini, fim = janelas.janela_atual(dia, empresa)
k = (getattr(empresa, 'pk', empresa), fim.month, fim.year)
return bool(competencia_entregue(*k))
```

`chamados.services.vigia_fio.competencia_do_dia_entregue(dia, colaborador.empresa)`; dia None retorna None. `app/chamados/services/vigia_fio.py:822-844`. Corte vem da empresa, com clamp 2..28/default21, e competência é o mês/ano do **fim** da janela. `app/ponto/janelas.py:10-33,36-63`.

Outros escritores usam:

- `competencias_a_lavrar` mantém competência anterior enquanto `competencia_entregue` for falsa. `app/ponto/services/he_pendente_lavrado.py:209-225`.
- `empresas_exportadas_no_escopo` chama `marcos_da_competencia`, cujo campo exportada chama o mesmo juiz. `app/ponto/services/fechamento.py:1480-1499,1513-1524`.
- Regeneração de célula usa autoridade **mais estrita e distinta**: `competencia_fechada_para_celula = entregue OU holerite do próprio colaborador em lote publicado`; `_competencias_exportadas` enumera e pergunta esse juiz. `app/folha/export.py:589-615`; `app/ponto/management/commands/gerar_celulas.py:126-174`.
- `competencia_fechada` lê `FechamentoMensal.status in ('aprovado','exportado')` e responde outra pergunta: guarda de competência fechada para escrita de batida. `app/ponto/janelas.py:75-89`; `app/ponto/registro_batida.py:133-135`.
- `folha/porta_export.py::conferir` confere carimbo de medição de hoje/mesma competência/zero falhas; não é autoridade para saber se já foi entregue. `app/folha/porta_export.py:800-821`.

As portas atuais de criar/editar Ausencia **não barram competência fechada**; a remoção da guarda está explícita. Logo um backfill restrito a aberta precisaria fazer esse recorte explicitamente usando a autoridade apropriada. `app/ponto/services/ausencia.py:226-233,485`.

## 05/11 22:12 · sessao 1597824c0f67 · F-346 · verificador · origem=Code

**Não daria verde nesta volta sem triagem de dois achados: a remarcação passa a poder sobrescrever confirmação concorrente, e duas remarcações com objetos carregados da mesma janela podem deixar ausências órfãs.** O primeiro decorre do `save()` completo; o segundo, da janela antiga capturada do objeto sem recarga ou trava. (`app/ferias/services.py:376`, `app/ferias/services.py:389`, `app/ferias/services.py:496`, `app/ferias/views.py:638`)

A análise abaixo é estática. Execução RED/GREEN HTTP, suite, SQL efetivo, incidência em produção e confirmação independente de hash/pai/base: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

`JUIZES.md` é lookup e manda consultar o registro vivo. Este registra colisão, cobertura, saldo, status do período, fracionamento e aviso; `_trocar_janela` não substitui essas autoridades. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`, `app/core/juizes.py:239`, `app/core/juizes.py:242`, `app/core/juizes.py:252`)

No censo abaixo, AG = `AgendamentoFerias`, PA = `PeriodoAquisitivo`. As portas registradas são `ferias/services.py` para AG/PA e `ponto/services/ausencia.py` para Ausência. (`app/core/portas.py:480`)

| Campo | Escritores de runtime encontrados |
|---|---|
| AG `data_inicio/data_fim/dias`, existente | `_trocar_janela`, com atribuição em `services.py:379` e persistência em `services.py:389`. As duas funções públicas delegam a ele. (`app/ferias/services.py:379`, `app/ferias/services.py:397`, `app/ferias/services.py:404`) |
| AG, criação dos intervalos | Gozo histórico, execução de agendamento e aprovação de solicitação. (`app/ferias/services.py:278`, `app/ferias/services.py:298`, `app/ferias/services.py:321`, `app/ferias/services.py:431`, `app/ferias/services.py:441`) |
| AG `aviso_emitido` | Default falso; execução comum; aprovação; troca de janela. (`app/ferias/models.py:305`, `app/ferias/services.py:314`, `app/ferias/services.py:323`, `app/ferias/services.py:436`, `app/ferias/services.py:380`) |
| AG `status` | Default `agendado`; histórico pelo juiz temporal; cancelamento DP/admin; troca pelo juiz temporal; alinhamento chamado pelo cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:512`, `app/ferias/management/commands/sincronizar_status_ferias.py:48`) |
| PA `dias_concedidos` | Criação com defaults; histórico; execução; cancelamento DP/admin; troca; aprovação. (`app/ferias/services.py:259`, `app/ferias/views.py:184`, `app/ferias/services.py:299`, `app/ferias/services.py:324`, `app/ferias/services.py:333`, `app/ferias/services.py:353`, `app/ferias/services.py:378`, `app/ferias/services.py:442`) |
| PA `status` | `aplicar_status` recebe o veredito de `status_de_periodo`; os atos acima o persistem. Também há sincronização, realinhamento de período existente por `update`, abono e venda. (`app/ferias/models.py:204`, `app/ferias/models.py:212`, `app/ferias/services.py:225`, `app/ferias/services.py:266`, `app/ferias/services.py:165`, `app/ferias/services.py:508`) |

O `ag.save()` completo também alcança estas colunas, além dos cinco campos pretendidos; a PK identifica a linha no UPDATE normal. Detalhe SQL da versão instalada: **[nao verificado]**. (`app/ferias/models.py:299`, `app/ferias/models.py:310`, `app/ferias/models.py:313`, `app/ferias/models.py:319`, `app/ferias/services.py:389`)

| Coluna adicional | Escritores e leitores encontrados |
|---|---|
| `periodo_aquisitivo_id` | Preenchido nas três criações; agora também regravado pela troca. Lido por saldo, guarda, materialização, consultas e apresentação. (`app/ferias/services.py:279`, `app/ferias/services.py:322`, `app/ferias/services.py:432`, `app/ferias/services.py:389`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1163`, `app/templates/ferias/_tabela_ferias.html:36`) |
| `agendado_por_id` | Preenchido nas criações; agora regravado pela troca. Lido para autoria das ausências e filtro do lote. (`app/ferias/services.py:283`, `app/ferias/services.py:323`, `app/ferias/services.py:437`, `app/ferias/services.py:389`, `app/ponto/services/ausencia.py:1189`, `app/ferias/views.py:536`) |
| `obs` | Preenchido nas criações; lido, concatenado e persistido pelo cancelamento admin; agora regravado pela troca sem nova atribuição. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:438`, `app/ferias/services.py:360`, `app/ferias/services.py:389`) |
| `confirmacao/confirmado_em` | Escritores próprios: pedido de confirmação e resposta do colaborador; agora também a troca completa. Leitores: envio, resposta, atendimento e apresentação. (`app/ferias/services.py:481`, `app/ferias/services.py:489`, `app/ferias/services.py:389`, `app/ferias/views.py:838`, `app/ferias/views.py:879`, `app/chamados/views.py:627`, `app/templates/ferias/_secao_colab.html:122`) |
| `criado_em` | Nasce por `auto_now_add`; passa pelo save completo. Não localizei leitor específico de runtime: **[nao verificado]**. (`app/ferias/models.py:319`, `app/ferias/services.py:389`) |

Leitores encontrados dos campos de AG, agrupados por caminho:

| Caminho | Campos e referências |
|---|---|
| Modelo | Datas/status nas propriedades de edição/cancelamento; datas/dias/status no `clean`; datas/status na guarda; dias no saldo para remarcar; datas/dias na representação. (`app/ferias/models.py:219`, `app/ferias/models.py:322`, `app/ferias/models.py:348`, `app/ferias/models.py:394`, `app/ferias/models.py:450`) |
| Serviços | Datas/status nas consultas de janela, gozo, divergência e inícios; datas na duplicata; dias/datas nos cancelamentos e troca. (`app/ferias/services.py:51`, `app/ferias/services.py:63`, `app/ferias/services.py:77`, `app/ferias/services.py:102`, `app/ferias/services.py:316`, `app/ferias/services.py:333`, `app/ferias/services.py:376`) |
| Views | Listagem/KPIs; frações; cancelamento/auditoria; correção/auditoria; lote; remarcação/aviso; exclusão de PA; confirmação/push. (`app/ferias/views.py:21`, `app/ferias/views.py:71`, `app/ferias/views.py:234`, `app/ferias/views.py:362`, `app/ferias/views.py:470`, `app/ferias/views.py:527`, `app/ferias/views.py:649`, `app/ferias/views.py:671`, `app/ferias/views.py:814`, `app/ferias/views.py:838`, `app/ferias/views.py:849`) |
| Situação de férias | Status/dias nas frações; datas/status nos vivos, colisão e batidas; `aviso_emitido` no aviso. (`app/ferias/situacao.py:117`, `app/ferias/situacao.py:164`, `app/ferias/situacao.py:246`, `app/ferias/situacao.py:257`, `app/ferias/situacao.py:270`, `app/ferias/situacao.py:343`) |
| Materialização | Status/datas para dias desejados; autor para criação; datas na pauta de recusa. (`app/ponto/services/ausencia.py:1163`, `app/ponto/services/ausencia.py:1178`, `app/ponto/services/ausencia.py:1189`, `app/ponto/services/ausencia.py:1235`) |
| Folha/API | Intervalo recortado na folha; datas/dias/status na API; também aviso na mensageria. (`app/folha/export.py:480`, `app/api/views.py:2252`, `app/api/views.py:2268`, `app/api/views_mensageria.py:840`) |
| Colaboradores/inteligência | Disponibilidade/retorno; gozo; início/status/dias nas consultas e apresentação. (`app/colaboradores/views.py:2843`, `app/colaboradores/views.py:2895`, `app/inteligencia/views.py:92`, `app/inteligencia/views.py:276`, `app/inteligencia/views.py:285`) |
| Ponto | Gate e mensagem de férias; universo por janela; aviso da batida. (`app/ponto/utils.py:88`, `app/ponto/utils.py:197`, `app/ponto/services/aviso_da_batida.py:73`) |
| Comandos | Cron lê datas/status; alerta de retorno lê fim/status. (`app/ferias/management/commands/sincronizar_status_ferias.py:33`, `app/ferias/management/commands/alertar_retornos.py:20`) |
| Templates | Datas/dias/status/aviso nas tabelas, saldo e seção; datas/dias no atendimento e lote. (`app/templates/ferias/_tabela_ferias.html:39`, `app/templates/ferias/saldo.html:59`, `app/templates/ferias/_secao_colab.html:117`, `app/templates/ferias/_meu_atendimento_ferias.html:6`, `app/templates/ferias/cancelar_admin_massa.html:42`) |

Os leitores encontrados de PA `dias_concedidos/status` são os juízes/properties, sincronização/divergência, operações de saldo, seleção da aprovação, listagens/KPIs, auditoria, proteção do direito, situação, relatório e templates. (`app/ferias/models.py:95`, `app/ferias/models.py:212`, `app/ferias/models.py:217`, `app/ferias/models.py:231`, `app/ferias/models.py:239`, `app/ferias/services.py:90`, `app/ferias/services.py:118`, `app/ferias/services.py:225`, `app/ferias/services.py:378`, `app/ferias/services.py:421`, `app/ferias/views.py:29`, `app/ferias/views.py:55`, `app/ferias/views.py:473`, `app/ferias/views.py:790`, `app/ferias/situacao.py:178`, `app/relatorios/services.py:285`, `app/templates/ferias/saldo.html:20`, `app/templates/ferias/_secao_colab.html:42`)

Para Ausência `ferias`, o encadeamento é:

- AG `save` → `_sync_ausencia` → `espelhar_ferias`; este lê tipo/status/datas, preserva existentes e cria faltantes pela porta genérica. Agendamento inativo apaga as existentes da janela. (`app/ferias/models.py:379`, `app/ferias/models.py:434`, `app/ponto/services/ausencia.py:1164`, `app/ponto/services/ausencia.py:1172`, `app/ponto/services/ausencia.py:1182`)
- A troca e o `delete` individual do AG chamam `apagar_ausencias_de_ferias`; o cancelamento admin primeiro rejeita por `update`. (`app/ferias/services.py:387`, `app/ferias/models.py:439`, `app/ponto/services/ausencia.py:1246`, `app/ponto/services/ausencia.py:1257`)
- Portas genéricas existentes também podem alcançar registros desse tipo: criação, edição, estado/decisão, anexo, reabertura, rederivação e exclusão de ensaio. São caminhos na mesma porta registrada. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:792`, `app/ponto/services/ausencia.py:830`, `app/ponto/services/ausencia.py:881`, `app/ponto/services/ausencia.py:1090`, `app/ponto/services/ausencia.py:1270`, `app/ponto/services/ausencia.py:1277`)
- Leitores centrais: cobertura/universos, precedência/veto, cartório, decisão do dia, espelho, escala e silêncio de chamados. (`app/ponto/turnos.py:2529`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2753`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:461`, `app/ponto/services/cartorio.py:355`, `app/ponto/services/dia_decidido.py:481`, `app/ponto/services/espelho.py:582`, `app/escala/utils.py:1195`, `app/chamados/services/silencio_ausencia.py:65`)
- Outros leitores genéricos: telas/API, detalhe do colaborador, histórico/materialização, folha, relatórios, inteligência, catálogo e vigias. (`app/ponto/views.py:2182`, `app/api/views.py:1861`, `app/colaboradores/services/detalhe.py:104`, `app/chamados/services/historico.py:129`, `app/chamados/services/materializacao.py:599`, `app/folha/export.py:76`, `app/relatorios/views.py:197`, `app/inteligencia/resumo.py:39`, `app/ponto/views_tipos_ausencia.py:24`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia_veredito.py:46`)
- DELETE/INSERT acionam julgamento síncrono pelos signals e consumidores depois do commit; Ausência também tem vínculo `SET_NULL` em `CelulaDia`. (`app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:250`, `app/ponto/signals.py:272`, `app/escala/models.py:1796`)

Migrações/defaults e fixtures são fontes distintas dos escritores de runtime; há inclusive injeção deliberada de legado ultrapassando o override nos testes. (`app/ferias/migrations/0001_initial.py:40`, `app/ferias/migrations/0003_agendamentoferias_confirmacao_and_more.py:13`, `app/ferias/tests.py:96`, `app/ferias/test_editar_agendamento.py:16`, `app/ferias/test_sobreposicao_chokepoint.py:60`)

Exaustividade para SQL externo, reflexão, caminhos dinâmicos e escritores fora da cópia: **[nao verificado]**.

**2. Classes perguntadas**

| Classe | Resultado |
|---|---|
| **(a) Segundo juiz** | **Não identificado no diff.** Status AG vem de `status_por_data`; PA de `aplicar_status/status_de_periodo`; aviso usa a constante registrada; colisão continua no veto registrado. (`app/ferias/services.py:373`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:390`, `app/ferias/models.py:212`, `app/ferias/models.py:428`, `app/core/juizes.py:242`, `app/core/juizes.py:256`) |
| **(b) Segundo escritor fora da porta** | **Não introduzido nos campos perguntados.** As duas entradas delegam ao mesmo escritor; Ausência continua pela porta registrada. O save completo, contudo, amplia o conjunto de campos regravados dentro da porta. (`app/ferias/services.py:387`, `app/ferias/services.py:389`, `app/ferias/services.py:397`, `app/ferias/services.py:404`, `app/core/portas.py:485`) |
| **(c) Relógio fora da borda** | **Presente no corpo resultante:** serviço lê `date.today`, e os juízes recebem `hoje=None` e leem novamente. É padrão preservado pelo diff fornecido, não leitura nova criada pela delegação. O teste congela o relógio, sem verificar um único instante propagado da borda. (`app/ferias/services.py:380`, `app/ferias/models.py:94`, `app/ferias/models.py:280`, `app/ferias/test_remarcar_limpa_janela_f346.py:23`) |
| **(d) Derivação paralela** | **Não identificada nos corpos alterados.** Permanecem os mesmos juízes de status, saldo e aviso. (`app/ferias/services.py:378`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:390`, `app/ferias/models.py:212`) |
| **(e) Selo/RED sem mordida** | **Tem mordida, mas limitada.** Os testes observam a janela órfã e a identidade da interseção; o AST morde duas atribuições explícitas, mas não certifica qualquer segundo caminho. Detalhamento abaixo. (`app/ferias/test_remarcar_limpa_janela_f346.py:52`, `app/ferias/test_remarcar_limpa_janela_f346.py:59`, `app/ferias/test_remarcar_limpa_janela_f346.py:99`, `app/ferias/test_remarcar_limpa_janela_f346.py:130`) |
| **(f) Ordem de trava** | **Muda na remarcação; não aparece par de tabelas inédito no conjunto comparado.** Há ordens opostas preexistentes, detalhadas abaixo. (`app/ferias/services.py:353`, `app/ferias/services.py:356`, `app/ferias/services.py:387`, `app/ferias/services.py:391`) |
| **(g) Pouso antes de pré-requisito** | O corpo usa campos e portas existentes; não evidencia nova dependência de schema. Aplicação das migrações existentes, deploy, suite e estado real do dado: **[nao verificado]**. (`app/ferias/models.py:299`, `app/ferias/migrations/0003_agendamentoferias_confirmacao_and_more.py:13`, `app/ferias/services.py:374`) |
| **(h) Mudança de comportamento** | **Sim na remarcação:** limpeza da janela anterior, respectivos efeitos de DELETE e expansão do save. Na correção, o corpo fornecido foi mantido sob outro nome. Os únicos chamadores `.py` localizados são as duas views. (`app/ferias/services.py:387`, `app/ferias/services.py:389`, `app/ferias/services.py:397`, `app/ferias/services.py:404`, `app/ponto/signals.py:150`, `app/ferias/views.py:490`, `app/ferias/views.py:668`) |

O save antigo com `update_fields` **já disparava guarda e sync**: ambos são incondicionais no override. O ganho não depende de usar save completo; a mudança relevante é chamar a limpeza e ampliar as colunas persistidas. (`app/ferias/models.py:434`, `app/ferias/models.py:437`, `app/ferias/services.py:387`, `app/ferias/services.py:389`)

O AST só lê `services.py`, reconhece `Assign/AugAssign` em atributos e exclui receptores chamados literalmente `self` ou `periodo`. Extraindo seus helpers em memória, um segundo escritor por atribuição explícita foi detectado; `setattr`, `QuerySet.update` e `AnnAssign` passaram despercebidos. Escritor em outro arquivo também fica fora do universo. (`app/ferias/test_remarcar_limpa_janela_f346.py:87`, `app/ferias/test_remarcar_limpa_janela_f346.py:95`, `app/ferias/test_remarcar_limpa_janela_f346.py:99`, `app/ferias/test_remarcar_limpa_janela_f346.py:104`)

Mutante plausível não distinguido pelos testes novos: **retirar o `atomic`, mantendo limpeza e saves**. Todos os cenários funcionais novos são de sucesso; o AST não cobra transação. Um veto A2 depois do DELETE distinguiria esse mutante, verificando restauração das ausências e do PA. A versão atual contém essa proteção; passagem efetiva do mutante pela suite: **[nao verificado]**. (`app/ferias/services.py:377`, `app/ferias/services.py:387`, `app/ferias/test_remarcar_limpa_janela_f346.py:52`, `app/ferias/test_remarcar_limpa_janela_f346.py:120`)

As ordens de escrita das tabelas do domínio são:

| Caminho | Ordem |
|---|---|
| Remarcar antes | AG U → Ausência I pelo sync → PA U, conforme diff fornecido; arquivo anterior independente: **[nao verificado]**. O encadeamento save/sync está em `app/ferias/models.py:434`. |
| Remarcar depois / corrigir agora | Ausência D → AG U → Ausência I → PA U. (`app/ferias/services.py:387`, `app/ferias/services.py:389`, `app/ferias/services.py:391`, `app/ponto/services/ausencia.py:1186`) |
| Corrigir antes | Mesma sequência, conforme corpo renomeado no diff fornecido; histórico independente: **[nao verificado]**. |
| Cancelar admin | PA U → Ausência U rejeitada → AG U → Ausência D. (`app/ferias/services.py:355`, `app/ferias/services.py:356`, `app/ferias/services.py:362`, `app/ponto/services/ausencia.py:1172`) |
| Gozo histórico | AG I → Ausência I → PA U. (`app/ferias/services.py:298`, `app/ferias/services.py:302`, `app/ferias/models.py:437`) |
| Executar | AG I → Ausência I → PA U. (`app/ferias/services.py:321`, `app/ferias/services.py:326`, `app/ferias/models.py:437`) |
| Avaliar/aprovar solicitação | Trava Solicitação → Solicitação U → AG I → Ausência I → PA U. (`app/ferias/views.py:717`, `app/ferias/views.py:718`, `app/ferias/services.py:416`, `app/ferias/services.py:441`, `app/ferias/services.py:444`) |
| Excluir período | DELETE do queryset de AG → DELETE do PA; esse corpo não chama explicitamente a limpeza individual. (`app/ferias/services.py:476`) |

Portanto, Ausência → AG é nova **nessa remarcação**, mas já ocorre no cancelamento admin e no corpo anterior da correção fornecido. Ausência → PA e PA → Ausência coexistem; não há ordem global uniforme. Deadlock concreto, linhas compartilhadas e ordem SQL dos efeitos indiretos: **[nao verificado]**. (`app/ferias/services.py:355`, `app/ferias/services.py:356`, `app/ferias/services.py:362`, `app/ferias/services.py:387`, `app/ferias/services.py:391`)

Os signals de Ausência acrescentam julgamento síncrono de células; os consumidores por `on_commit` ficam depois do commit. A sequência simplificada acima não é uma enumeração de todas as travas SQL. (`app/ponto/signals.py:85`, `app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:272`)

Os testes existentes mantêm expectativas de datas, saldo, recusas de status, permissões, justificativa, limpeza na correção e status rederivado. Não localizei expectativa que exija preservar as órfãs antigas; resultado real desses testes: **[nao verificado]**. (`app/ferias/test_editar_agendamento.py:25`, `app/ferias/test_editar_agendamento.py:35`, `app/ferias/test_editar_agendamento.py:52`, `app/ferias/test_editar_datas_admin.py:50`, `app/ferias/test_editar_datas_admin.py:96`, `app/ferias/test_a3_situacao_de_ferias.py:549`, `app/ferias/test_a3_situacao_de_ferias.py:571`)

**3. Contraexemplos e lacunas**

**Confirmação perdida — regressão permitida pelo save ampliado.** Intercalação: a remarcação carrega AG com `aguardando/None`; o colaborador confirma e grava `confirmado/instante`; a remarcação salva seu objeto antigo integralmente e pode restaurar `aguardando/None`. O teste novo não observa esses campos. É contraexemplo deduzido do código; reprodução em banco e ocorrência em produção: **[nao verificado]**. (`app/ferias/views.py:638`, `app/ferias/views.py:879`, `app/ferias/views.py:899`, `app/ferias/services.py:496`, `app/ferias/services.py:498`, `app/ferias/services.py:389`, `app/ferias/test_remarcar_limpa_janela_f346.py:52`)

**Duas remarcações deixam órfã — cura ainda vulne

[truncado em 20000 de 24159 caracteres]

## 05/11 22:08 · sessao c88468a062ea · - · - · origem=Ronald (tmux)

Censo estático da cópia, sem banco/log e sem execução de testes; cobertura de caminhos dinâmicos ou SQL externo: [nao verificado]. O lookup remete ao registro vivo (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`); saldo, status do PA, fracionamento, aviso e colisão têm autoridades registradas (`app/core/juizes.py:239`–`257`).

**Escritores de runtime**

| Campo | Escritores e persistência |
|---|---|
| AG `data_inicio/data_fim/dias` existente | `_trocar_janela`, atribuição e `save()` (`app/ferias/services.py:376`–`389`). Não encontrei outro escritor do intervalo por atribuição AST no código de runtime; inexistência de escritor dinâmico: [nao verificado]. |
| AG criação | `registrar_gozo_historico` (`app/ferias/services.py:278`–`298`), `executar_agendamento` (`app/ferias/services.py:321`–`323`), `aprovar_solicitacao` (`app/ferias/services.py:431`–`441`). |
| AG `aviso_emitido` | Criação comum (`app/ferias/services.py:314`, `323`); troca (`app/ferias/services.py:380`); aprovação (`app/ferias/services.py:436`). Default falso (`app/ferias/models.py:305`). |
| AG `status` | Criação histórica pelo juiz (`app/ferias/services.py:282`); default agendado (`app/ferias/models.py:309`); cancelamento DP (`app/ferias/services.py:336`–`337`), admin (`app/ferias/services.py:361`–`362`); troca pelo juiz (`app/ferias/services.py:384`); cron delega a `alinhar_status_por_data` (`app/ferias/management/commands/sincronizar_status_ferias.py:38`–`49`; `app/ferias/services.py:512`–`516`). |
| PA `dias_concedidos` | Histórico (`app/ferias/services.py:299`–`302`), criação (`app/ferias/services.py:324`–`326`), cancelamento DP (`app/ferias/services.py:333`–`335`), admin (`app/ferias/services.py:353`–`355`), troca (`app/ferias/services.py:378`, `391`), aprovação (`app/ferias/services.py:442`–`444`); defaults/criação via `garantir_periodo_aquisitivo` (`app/ferias/services.py:259`–`260`; `app/ferias/views.py:174`–`184`). |
| PA `status` | `aplicar_status` atribui a decisão de `status_de_periodo` (`app/ferias/models.py:204`–`212`); os escritores acima a persistem. Também sincronização (`app/ferias/services.py:225`–`239`), período existente por `QuerySet.update` (`app/ferias/services.py:263`–`266`), concessão com abono (`app/ferias/services.py:163`–`165`), venda (`app/ferias/services.py:506`–`508`). |

**Colunas adicionais alcançadas pelo `ag.save()` completo**

O modelo declara `periodo_aquisitivo`, `agendado_por`, `obs`, `confirmacao`, `confirmado_em`, `criado_em`, além dos cinco campos pretendidos (`app/ferias/models.py:299`–`319`). O PK é usado para identificar a linha; não deve ser contado como coluna regravada no UPDATE normal. Detalhe SQL da versão instalada: [nao verificado].

- `periodo_aquisitivo` e `agendado_por`: preenchidos nas três criações (`app/ferias/services.py:278`–`284`, `321`–`323`, `431`–`438`). São lidos na materialização, guardas e apresentação (`app/ponto/services/ausencia.py:1163`, `1189`; `app/ferias/models.py:424`, `428`; `app/templates/ferias/_tabela_ferias.html:36`–`38`).
- `obs`: criado nas mesmas portas (`app/ferias/services.py:284`, `323`, `438`); cancelamento admin concatena e persiste (`app/ferias/services.py:360`–`362`). `_trocar_janela` passa a regravar o snapshot sem atribuir novo valor (`app/ferias/services.py:379`–`389`).
- `confirmacao/confirmado_em`: `pedir_confirmacao` e `registrar_resposta_do_colab` (`app/ferias/services.py:481`–`498`). Leitores: envio (`app/ferias/views.py:838`), resposta (`app/ferias/views.py:879`), atendimento (`app/chamados/views.py:627`–`629`), apresentação (`app/templates/ferias/_secao_colab.html:122`–`124`).
- `criado_em`: `auto_now_add`, sem escritor explícito adicional localizado (`app/ferias/models.py:319`); leitor específico de runtime não localizado: [nao verificado].
- Exemplo de alteração concorrente exposta: depois de a view carregar AG, uma confirmação grava `confirmado/confirmado_em`; o `save()` completo da remarcação pode regravar o snapshot anterior. A view carrega antes do serviço (`app/ferias/views.py:638`, `668`), a confirmação grava separadamente (`app/ferias/services.py:496`–`498`) e a troca salva todos os campos (`app/ferias/services.py:389`). Ocorrência real: [nao verificado].

**Leitores de AG, agrupados**

- Núcleo: propriedades de estado (`app/ferias/models.py:322`–`346`); `clean` lê status/dias/datas (`app/ferias/models.py:350`–`375`); guarda lê datas/status e consulta janela persistida (`app/ferias/models.py:394`–`430`); texto e ordenação (`app/ferias/models.py:448`–`451`); saldo para remarcar lê `dias` (`app/ferias/models.py:219`–`222`).
- Serviços: intervalo e status (`app/ferias/services.py:51`, `63`, `77`–`82`, `102`); duplicata (`app/ferias/services.py:316`–`318`); cancelamentos e troca (`app/ferias/services.py:333`–`389`).
- Views: KPIs/listagem (`app/ferias/views.py:25`–`31`, `71`–`89`); validação/fracionamento (`app/ferias/views.py:234`); cancelamento/auditoria (`app/ferias/views.py:325`, `362`–`375`, `404`–`414`); correção/auditoria (`app/ferias/views.py:452`, `470`–`503`); lote (`app/ferias/views.py:527`, `561`); remarcação (`app/ferias/views.py:649`, `671`); exclusão de PA (`app/ferias/views.py:814`); confirmação/push (`app/ferias/views.py:838`, `849`, `856`, `879`, `891`).
- Situação: frações (`app/ferias/situacao.py:117`–`122`), vivos (`app/ferias/situacao.py:164`–`168`), aviso (`app/ferias/situacao.py:246`–`262`), batida dentro das férias (`app/ferias/situacao.py:270`–`289`), aviso de colisão (`app/ferias/situacao.py:343`–`352`).
- Espelho Ausência: `status/data_inicio/data_fim/agendado_por` (`app/ponto/services/ausencia.py:1163`–`1190`); pauta de recusa lê datas (`app/ponto/services/ausencia.py:1235`–`1236`).
- Folha: lê intervalo pelo serviço e recorta (`app/folha/export.py:473`–`482`).
- API: datas/dias/status (`app/api/views.py:2252`–`2268`); mensageria também aviso (`app/api/views_mensageria.py:840`–`843`).
- Disponibilidade: consulta status/fim e retorno (`app/colaboradores/views.py:2843`–`2853`), em gozo pelo juiz e apresentação (`app/colaboradores/views.py:2895`–`2904`).
- Inteligência: consulta início/status e apresentação início/dias (`app/inteligencia/views.py:92`–`94`, `276`–`289`).
- Ponto: gate/apresentação (`app/ponto/utils.py:88`–`131`); universo por janela (`app/ponto/utils.py:197`–`198`); aviso da batida pelo juiz (`app/ponto/services/aviso_da_batida.py:73`–`74`).
- Comandos: sincronização (`app/ferias/management/commands/sincronizar_status_ferias.py:33`–`43`); retornos (`app/ferias/management/commands/alertar_retornos.py:20`–`24`).
- Templates: datas/dias/status/aviso (`app/templates/ferias/_tabela_ferias.html:39`–`43`, `app/templates/ferias/saldo.html:59`–`70`, `app/templates/ferias/_secao_colab.html:117`–`152`); lote (`app/templates/ferias/cancelar_admin_massa.html:42`–`45`); atendimento (`app/templates/ferias/_meu_atendimento_ferias.html:6`).

**Leitores de PA `dias_concedidos/status`**

Saldo/status canônicos (`app/ferias/models.py:95`, `212`, `217`); sincronização e divergência (`app/ferias/services.py:90`, `225`–`239`), períodos vencendo (`app/ferias/services.py:118`), todos os incrementos/decrementos acima; seleção da aprovação (`app/ferias/services.py:421`–`423`); listagem e KPIs (`app/ferias/views.py:29`–`30`, `55`–`59`); auditoria (`app/ferias/views.py:364`, `373`, `406`, `414`, `473`, `503`); proteção de direito (`app/ferias/views.py:790`–`791`); situação de férias (`app/ferias/situacao.py:178`–`183`); relatório (`app/relatorios/services.py:285`–`290`); templates (`app/templates/ferias/saldo.html:20`–`46`, `app/templates/ferias/_secao_colab.html:42`–`49`, `app/templates/ferias/_tabela_ferias.html:29`).

**Ausencia `ferias`: escritores**

A materialização lê todas as linhas do tipo na janela, sem filtro de status (`app/ponto/services/ausencia.py:1165`–`1169`); cria os dias faltantes pela porta genérica (`app/ponto/services/ausencia.py:1182`–`1191`) e apaga quando inativa (`app/ponto/services/ausencia.py:1172`–`1175`). Limpeza inclusiva com exceção inclusiva (`app/ponto/services/ausencia.py:1250`–`1254`); rejeição admin (`app/ponto/services/ausencia.py:1265`–`1267`); AG `delete()` chama limpeza (`app/ferias/models.py:439`–`443`).

Portas genéricas existentes também alcançam registros de férias: criação (`app/ponto/services/ausencia.py:130`, `341`–`349`, `417`), edição (`app/ponto/services/ausencia.py:465`, `496`–`508`), estado (`app/ponto/services/ausencia.py:792`, `808`–`819`), decisão (`app/ponto/services/ausencia.py:895`–`911`), reabertura (`app/ponto/services/ausencia.py:1090`–`1132`), rederivação (`app/ponto/services/ausencia.py:1277`–`1280`), limpeza exclusiva de ensaio (`app/ponto/services/ausencia.py:1270`–`1274`). Todas ficam na mesma porta; não constituem automaticamente escritor externo. Admin de Ausencia é `SoLeitura` (`app/ponto/admin.py:37`–`43`); admin de férias registra apenas Afastamento (`app/ferias/admin.py:2`–`5`).

**Ausencia: leitores e efeitos relevantes**

- Cobertura e universos: `app/ponto/turnos.py:2529`, `2552`, `2753`, `2815`, `3087`; precedência/veto: `app/ponto/precedencia.py:273`–`279`; cartório: `app/ponto/services/cartorio.py:355`; decisão do dia: `app/ponto/services/dia_decidido.py:481`–`487`, `542`, `579`.
- DELETE/INSERT dispara leitura dos dias e rejulgamento síncrono (`app/ponto/signals.py:115`–`128`, `143`–`152`) e reconciliação após commit (`app/ponto/signals.py:250`–`284`).
- Consumidores genéricos: ponto/telas (`app/ponto/views.py:1002`, `2182`, `2222`, `2335`, `2573`, `2718`, `2751`); API (`app/api/views.py:1861`, `3092`); detalhe do colaborador (`app/colaboradores/services/detalhe.py:104`); histórico (`app/chamados/services/historico.py:129`); texto/absorção (`app/chamados/services/materializacao.py:599`–`600`, `635`–`653`); folha (`app/folha/export.py:76`–`81`); relatórios (`app/relatorios/views.py:197`, `277`); inteligência (`app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `239`, `516`); vigias (`app/ponto/services/vigia_ausencia.py:59`, `108`–`120`, `215`–`219`; `app/ponto/services/vigia_ausencia_veredito.py:46`).
- Catálogo/contagem lê qualquer tipo (`app/ponto/views_tipos_ausencia.py:24`). Comandos genéricos de inspeção/rejulgamento: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `ausencias_sem_efeito_na_celula.py:41`, `ausencia_bloqueante_sem_fim.py:27`, `curar_ausencia_sem_fim.py:29`, `rejulgar_status_por_tipo.py:23`, `reabrir_chamados_ausencia_aberta.py:33`.

**Fontes distintas de runtime**

Schema/defaults iniciais (`app/ferias/migrations/0001_initial.py:40`–`72`), confirmação (`app/ferias/migrations/0003_agendamentoferias_confirmacao_and_more.py:13`–`21`). Fixtures de teste criam AG/PA diretamente, p.ex. `app/ferias/tests.py:96`, `app/ferias/test_editar_agendamento.py:16`–`22`, `app/ferias/test_editar_datas_admin.py:31`–`42`, teste novo `app/ferias/test_remarcar_limpa_janela_f346.py:29`–`32`. Há injeção intencional de legado ultrapassando o override (`app/ferias/test_sobreposicao_chokepoint.py:60`–`63`). Não se deve converter isso em escritor de produção.

Outro sítio de troca do intervalo AG sem limpeza não foi encontrado na busca estática; afirmação absoluta sobre caminhos dinâmicos, importação externa ou dado de produção: [nao verificado].

## 05/11 22:08 · sessao 5342cbf7a899 · - · - · origem=Ronald (tmux)

A cura possui mordida funcional e estrutural, com lacunas de cobertura; não encontrei contraexemplo atual à diferença inclusiva entre janelas. (app/ferias/test_remarcar_limpa_janela_f346.py:52; app/ponto/services/ausencia.py:1246)

- O teste funcional percorre a porta HTTP real e verifica janela disjunta, sobreposição com preservação dos PKs, borda comum inclusiva, redução e repetição; R01 também confere as datas persistidas do agendamento. A aceitação HTTP 200/302 isolada é fraca, mas as asserções posteriores impedem que uma simples recusa da remarcação faça esses cenários passarem. (app/ferias/test_remarcar_limpa_janela_f346.py:43; app/ferias/test_remarcar_limpa_janela_f346.py:52)
- Pela leitura do diff fornecido, a implementação anterior manteria as ausências antigas em R01/R02/R03/R04; execução RED contra base/pai não foi feita nem evidenciada nesta auditoria. [nao verificado]
- O detector AST lê somente `services.py`, reconhece apenas `Assign`/`AugAssign` para atributos chamados `data_inicio`, `data_fim` ou `dias`, e exclui receptores literalmente chamados `periodo` e `self`. A contraprova embutida morde dois escritores com atribuição explícita; não certifica o censo geral. (app/ferias/test_remarcar_limpa_janela_f346.py:87; app/ferias/test_remarcar_limpa_janela_f346.py:91; app/ferias/test_remarcar_limpa_janela_f346.py:130)
- Extraí e executei apenas os helpers AST em memória, com Python3 `-B`: a cópia retorna `['_trocar_janela']`; acrescentar segundo escritor via `setattr`, `QuerySet.update` ou `AnnAssign` mantém esse resultado, enquanto atribuição explícita acrescenta `b`. Esses escapes decorrem das formas selecionadas pelo detector. (app/ferias/test_remarcar_limpa_janela_f346.py:99)
- Mutante plausível: retirar o `atomic` e manter a mesma limpeza e demais instruções passa pelos cenários novos de sucesso e pelo AST; faltaria teste que provoque A2 depois do DELETE e confira restauração das ausências antigas e do PA. A implementação atual protege essas operações com o mesmo `atomic`; não é defeito atual. (app/ferias/services.py:377; app/ferias/services.py:387; app/ferias/test_remarcar_limpa_janela_f346.py:52; app/ferias/test_remarcar_limpa_janela_f346.py:120)

Os únicos chamadores `.py` encontrados das duas funções são `editar_datas_admin` e `editar_agendamento`; a correção captura `ValidationError`, enquanto a remarcação a deixa propagar. O veto continua acontecendo no `save`, logo essa diferença entre as portas já existia antes da cura. (app/ferias/views.py:489; app/ferias/views.py:491; app/ferias/views.py:667; app/ferias/models.py:427; app/ferias/models.py:434)

O `save(update_fields=[datas…])` anterior já disparava `_guard_sobreposicao` e `_sync_ausencia`; `save()` completo amplia as colunas persistidas, não cria esses disparos. Além dos cinco campos tratados, passa a persistir `periodo_aquisitivo_id`, `agendado_por_id`, `obs`, `confirmacao`, `confirmado_em` e `criado_em`; a confirmação possui escritor concorrente possível em `registrar_resposta_do_colab`, portanto uma instância carregada antes dessa resposta pode sobrescrevê-la com valores antigos. Ocorrência efetiva em produção: [nao verificado]. (app/ferias/models.py:299; app/ferias/models.py:310; app/ferias/models.py:313; app/ferias/models.py:434; app/ferias/services.py:389; app/ferias/services.py:489)

Testes existentes relevantes preservam suas expectativas: [nao verificado]

- Remarcação: datas novas, recusa de `em_gozo`/`concluido`, saldo e dias zero. A mudança das ausências não era observada nesses testes. (app/ferias/test_editar_agendamento.py:25; app/ferias/test_editar_agendamento.py:35; app/ferias/test_editar_agendamento.py:52; app/ferias/test_editar_agendamento.py:62)
- Correção admin: permissão, justificativa, cancelado, edição de `em_gozo`/`concluido`, saldo, migração das ausências e manutenção da porta antiga estreita. (app/ferias/test_editar_datas_admin.py:50; app/ferias/test_editar_datas_admin.py:71; app/ferias/test_editar_datas_admin.py:88; app/ferias/test_editar_datas_admin.py:96; app/ferias/test_editar_datas_admin.py:108)
- Correção A3: fração curta permitida, colisão recusada, saldo recusado e status rederivado no mesmo ato. O teste de colisão verifica somente a data antiga do agendamento, sem certificar as ausências restauradas. (app/ferias/test_a3_situacao_de_ferias.py:538; app/ferias/test_a3_situacao_de_ferias.py:549; app/ferias/test_a3_situacao_de_ferias.py:561; app/ferias/test_a3_situacao_de_ferias.py:571)

Sobre os casos pedidos: borda e sobreposição à direita estão cobertas; à esquerda, expansão, contenção deslocada, A2 e duas janelas vizinhas não estão no teste novo. O helper atual opera `antiga − nova` por limites inclusivos e, para vizinhas sem sobreposição, não alcança os dias do outro agendamento. A2 restaura o DELETE pelo `atomic`, embora a porta HTTP de remarcação não trate sua exceção. Um status diferente de `agendado` chegando diretamente ao serviço é rederivado sem guarda adicional; a guarda está explicitamente na view, e acrescentá-la ao serviço seria a guarda copiada proibida pelo aval. (app/ferias/test_remarcar_limpa_janela_f346.py:52; app/ponto/services/ausencia.py:1250; app/ferias/services.py:377; app/ferias/services.py:384; app/ferias/services.py:400; app/ferias/views.py:649; app/ferias/views.py:667)

## 05/11 22:07 · sessao 3acab27597a4 · - · - · origem=Ronald (tmux)

Auditoria de ordem/autoridades concluída por leitura; ocorrência real de deadlock ou resultados de execução permanecem [nao verificado].

**Ordens das três tabelas do domínio**

| Caminho | Ordem de escrita comprovável |
|---|---|
| Remarcar antes | AG UPDATE → Ausencia INSERT pelo sync → PA UPDATE; o corpo anterior está no diff fornecido, e o encadeamento de `save` para sync está em `app/ferias/models.py:434-437`. Referência de linha para o corpo anterior: [nao verificado]. |
| Remarcar depois / corrigir agora | Ausencia DELETE → AG UPDATE → Ausencia INSERT pelo sync → PA UPDATE (`app/ferias/services.py:377-391`, `app/ferias/models.py:434-437`, `app/ponto/services/ausencia.py:1182-1194`). |
| Corrigir antes | Mesma ordem atual, pois o diff fornecido apenas renomeia seu corpo para `_trocar_janela`; referência de linha do arquivo anterior: [nao verificado]. |
| Cancelar admin | PA UPDATE → Ausencia UPDATE rejeitada → AG UPDATE → Ausencia DELETE pelo sync (`app/ferias/services.py:352-362`, `app/ponto/services/ausencia.py:1265-1267`, `app/ponto/services/ausencia.py:1172-1176`). |
| Gozo histórico | AG INSERT → Ausencia INSERT → PA UPDATE (`app/ferias/services.py:277-302`, `app/ferias/models.py:434-437`). |
| Executar agendamento | AG INSERT → Ausencia INSERT → PA UPDATE (`app/ferias/services.py:315-326`, `app/ferias/models.py:434-437`). |
| Avaliar solicitação / aprovar | SELECT FOR UPDATE Solicitação → Solicitação UPDATE → AG INSERT → Ausencia INSERT → PA UPDATE (`app/ferias/views.py:717-731`, `app/ferias/services.py:413-416`, `app/ferias/services.py:431-444`). |
| Excluir período | DELETE em queryset dos agendamentos → DELETE do período; não há chamada explícita à porta de limpeza nesse corpo (`app/ferias/services.py:476-478`). |

- A ordem **A→AG é nova na remarcação**, mas não é par novo no conjunto analisado: já era a ordem da correção e cancelamento admin também acessa A antes de AG (`app/ferias/services.py:356-362`, `app/ferias/services.py:387-389`; histórico da correção pelo diff: [nao verificado]).
- Existem ordens opostas **A→PA** e **PA→A** entre troca/criação e cancelamento admin; portanto não cabe afirmar ordem global consistente. O diff amplia para remarcação uma ordem já existente na correção, sem demonstrar introdução de um novo par global (`app/ferias/services.py:387-391`, `app/ferias/services.py:353-356`, `app/ferias/services.py:321-326`).
- Deadlock concreto exige linhas concorrentes realmente compartilhadas e execução sobre banco; ocorrência, frequência e planos de trava são [nao verificado]. Não confundir a atribuição Python `periodo.dias_concedidos = ...` com UPDATE: a escrita do PA só ocorre no `save` posterior (`app/ferias/services.py:378`, `app/ferias/services.py:391`).

**Efeitos indiretos que a sequência simplificada omite**

- Ausencia tem consumidores síncronos de `post_save` e `post_delete` que chamam julgamento de células; portanto DELETE/INSERT não são operações isoladas das demais tabelas (`app/ponto/signals.py:143-152`, `app/ponto/signals.py:85-93`).
- CelulaDia referencia Ausencia por `SET_NULL`, logo apagar uma ausência também afeta esse vínculo quando existente (`app/escala/models.py:1796-1798`). A ordem SQL exata do coletor Django não foi disponível na cópia: [nao verificado].
- O consumidor de chamados agenda execução por `transaction.on_commit`, portanto esse consumidor não deve ser colocado entre DELETE e `ag.save()` na ordem interna do atomic (`app/ponto/signals.py:250-272`, `app/ponto/signals.py:282-284`).
- O julgamento síncrono retorna para datas futuras; os cenários novos, todos futuros ao relógio congelado, não exercitam lavra de células passadas (`app/ponto/services/cartorio.py:1965-1978`, `app/ferias/test_remarcar_limpa_janela_f346.py:19-30`).
- Em veto A2, a exceção sobe de `AgendamentoFerias.save` antes do `super().save`; o DELETE anterior está dentro do mesmo atomic. O código promete rollback transacional, mas execução do rollback nesta cópia é [nao verificado] (`app/ferias/models.py:428-436`, `app/ferias/services.py:377-389`).

**Juiz, derivação e relógio**

- JUIZES é lookup; o registro vivo prevalece (`app/docs/JUIZES.md:10-14`). Autoridades relevantes: veto, saldo, status de PA, fracionamento e aviso (`app/core/juizes.py:242`, `app/core/juizes.py:252-256`).
- Não identifiquei novo juiz nesses corpos: `_trocar_janela` chama `status_por_data`, `periodo.aplicar_status` e a porta de ausência; o save chama o veto registrado (`app/ferias/services.py:373-390`, `app/ferias/models.py:212`, `app/ferias/models.py:428-432`).
- Não há nova derivação paralela nesses corpos: o status de AG vem de `status_por_data`, o de PA de `status_de_periodo`, o aviso usa a constante registrada (`app/ferias/services.py:380-390`, `app/ferias/models.py:212`, `app/core/juizes.py:256`).
- Há relógio lido dentro do serviço, em vez de recebido da borda, além dos defaults dos juízes: `date.today()` no aviso e nos juízes (`app/ferias/services.py:380`, `app/ferias/models.py:280`, `app/ferias/models.py:94`). O diff preserva leituras já existentes em ambas as funções anteriores; não inaugura esse padrão [nao verificado quanto às linhas do commit anterior].
- Chamadas distintas ao relógio podem teoricamente atravessar meia-noite; a execução dessa divergência e sua presença em produção são [nao verificado]. O teste congela o relógio, não verifica propagação de um único instante capturado pela borda (`app/ferias/test_remarcar_limpa_janela_f346.py:23`).

**Grep pedido**

Nos termos literais `ferias/services`, `apagar_ausencias_de_ferias`, `remarcar`, `_sync_ausencia`, `espelhar_ferias`, a única ocorrência encontrada em LEIS/CORTES/DOSSIES foi `app/docs/LEIS.md:174`, L-089, citando `aplicar_art1`. Ausência de outra regra sob vocabulário diferente: [nao verificado].

**Outros pontos úteis**

- A porta admin captura `ValidationError`; a porta remarcação chama o serviço sem esse tratamento, podendo propagar o veto como erro HTTP. Essa diferença já existia antes segundo o diff, não nasce da delegação (`app/ferias/views.py:488-494`, `app/ferias/views.py:667-668`).
- A guarda HTTP da remarcação só testa `status == 'agendado'`; não consulta `ja_iniciou` nem compara início com hoje. Assim um agendamento atrasado cujo cron ainda não atualizou o status passa pela guarda (`app/ferias/views.py:648-650`, `app/ferias/models.py:322-328`).
- O serviço compartilhado não guarda status: chama o juiz temporal e pode reativar um cancelado se chamado diretamente; as duas portas HTTP possuem guardas diferentes que o impedem nos caminhos usuais (`app/ferias/services.py:378-389`, `app/ferias/views.py:452-453`, `app/ferias/views.py:648-650`). Isso também existia nos dois corpos anteriores segundo o diff; ocorrência real é [nao verificado].

## 05/11 22:05 · sessao 096a16709250 · C20-FIO-MUDO · verificador · origem=Code

**Parecer: não certificaria a obra como completa.** A escrita continua pelas portas existentes, mas há eleição paralela da canônica, retratação antes da aceitação da costura e falta de garantia transacional na reabertura direta. (`app/chamados/models.py:484`, `app/chamados/models.py:487`, `app/chamados/models.py:492`, `app/chamados/models.py:1408`, `app/chamados/models.py:1478`)

Execução dos testes, reprodução das corridas e resultado real em produção: **[não verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

| Campo/ato | Escritores | Leitores relevantes |
|---|---|---|
| `PerguntaDisputa.chamado_destino` | `PerguntaDisputa.abrir`, tanto nascimento quanto reapontamento por identidade; `fio.reapontar_resolvedora`. O método novo chama essa porta. (`app/chamados/models.py:2526`, `app/chamados/models.py:2552`, `app/chamados/services/fio.py:845`, `app/chamados/models.py:492`) | Âncora, C6/materialização, plano e reconciliador. (`app/chamados/services/fio.py:117`, `app/chamados/juizes.py:817`, `app/chamados/juizes.py:744`, `app/chamados/models.py:344`, `app/chamados/reconciliador.py:1480`) |
| `via_resolucao` | Núcleo `_escrever_carimbo`; portas de carimbar/descarimbar; também os sítios existentes de relavra e encerramento por fato. O novo caminho passa por `retratar_duplicata → carimbar_identidade`. (`app/chamados/models.py:2632`, `app/chamados/models.py:2701`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/fio.py:891`) | C6; migrador, por C6; ressuscitação. (`app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/services/fio.py:935`, `app/chamados/models.py:1531`) |
| `validada_em` | Validação/devolução administrativa; simetria em `carimbar`; desfazimento por correção e ressuscitação. Não recebe escritor novo neste diff. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/models.py:2668`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1580`) | C6, estado, seleção de ancoradas e reconciliação. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:2575`, `app/chamados/models.py:327`, `app/chamados/reconciliador.py:620`) |
| `status_local` | `_gravar_estado` e nascimento pelas portas existentes. O helper novo somente lê. (`app/chamados/models.py:222`, `app/chamados/models.py:1006`, `app/chamados/models.py:452`, `app/chamados/models.py:469`) | Vigia, gates, guarda da costura e juiz de materialização. (`app/chamados/services/vigia_fio.py:335`, `app/chamados/models.py:400`, `app/chamados/services/fio.py:764`, `app/chamados/juizes.py:744`) |
| Evento `pergunta_reatada` | Gate anterior e helper novo chamam `core.observ.evento`. Antes, `objeto_id` identifica a âncora que fecha; agora também pode identificar o destino que abre. (`app/chamados/models.py:423`, `app/chamados/models.py:496`) | Teste específico e auditoria genérica. Consumidor de produção específico por esse nome: **[não verificado]**. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:302`, `app/core/views.py:705`) |
| Lavra do reapontamento | `veredito_lavrado.lavrar` calcula os três campos; `reapontar_resolvedora` grava destino e lavra no mesmo `save`. (`app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:845`) | Leitura do veredito e controle de envelhecimento do cache. (`app/chamados/juizes.py:632`, `app/chamados/veredito_lavrado.py:173`) |

**Não há segundo escritor direto novo fora da porta.** A autorização do destino já contempla `models.py` e `fio.py`; o diff acrescenta uma chamada, não uma atribuição paralela. (`app/core/tests/test_contract_escritores_estado.py:60`, `app/chamados/models.py:492`)

Há, porém, resultados diferentes entre leitores:

- `fio_vivo` reduz C6; o vigia delega à mesma redução. Reatar muda a âncora, sem criar outra definição de vivacidade. (`app/chamados/juizes.py:1233`, `app/chamados/services/vigia_fio.py:156`)
- O helper aceita pergunta de disputa fechada e não reabre seu continente. O admin deliberadamente mostra essas vivas, mas `_disputa_do_chamado` exige disputa aberta e o caminho do colaborador também preserva esse recorte. Portanto, “reatada e viva” não prova “questionário novamente acessível ao colaborador”. (`app/chamados/models.py:464`, `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:209`, `app/api/views.py:1008`)
- A ressuscitação existente reabre a disputa quando efetivamente devolve perguntas; a adoção não executa esse efeito. Isso é uma diferença de cobertura, sem autorização para simplesmente reabrir tudo. (`app/chamados/models.py:1580`, `app/chamados/models.py:1607`, `app/chamados/services/fio.py:211`)

**2. Juiz novo ou derivação paralela**

Dia, motivos, competência, vivacidade e âncora são consultados nas funções existentes; não identifiquei cópia nova dessas cinco regras. O registro mantém C6 e a redução do fio nas autoridades já declaradas. (`app/chamados/models.py:454`, `app/chamados/models.py:459`, `app/chamados/models.py:468`, `app/chamados/models.py:475`, `app/core/juizes.py:260`)

A diferença para `_plano_pergunta_viva` é o sentido da busca: o plano parte da âncora e procura uma cobrança sobrevivente, conferida por E4-c; o novo método parte da cobrança e procura perguntas do colaborador/dia/motivo. (`app/chamados/models.py:364`, `app/chamados/models.py:373`, `app/chamados/models.py:464`)

**Existe uma segunda regra de eleição:** `_ja` determina que “a que já está aqui vence”, sem consultar `canonica_do_grupo` sobre o grupo completo. Exemplo: pergunta local sem resposta e órfã respondida; a lei da canônica favorece a respondida, mas o helper ignora a órfã inteira. A migração existente compara origem e destino conjuntamente. (`app/chamados/models.py:477`, `app/chamados/models.py:484`, `app/chamados/services/fio.py:873`, `app/chamados/services/fio.py:937`)

**3. Ordem de trava**

- `abrir()` envolve a seleção e criação em `atomic`, adquirindo primeiro a trava do colaborador. (`app/chamados/models.py:937`, `app/chamados/models.py:940`)
- **`reabrir()` diretamente não faz o mesmo:** consulta estado sem trava, chama `_gravar_estado`, depois ressuscita e adota. O `atomic` interno de `_gravar_estado` não envolve toda essa sequência. Quando chamado por `abrir`, herda a transação externa; fora disso, não há a mesma garantia. (`app/chamados/models.py:1408`, `app/chamados/models.py:274`, `app/chamados/models.py:1468`, `app/chamados/models.py:1477`)
- `canonica_do_grupo` somente ordena objetos em memória; competência consulta janela e entrega, sem trava explícita nesses helpers. (`app/chamados/services/fio.py:873`, `app/chamados/services/vigia_fio.py:834`)
- `retratar_duplicata` grava a pergunta pela porta de identidade; `reapontar_resolvedora` abre `atomic`, trava o chamado de destino e depois grava a pergunta canônica. Não trava nem relê a pergunta antes da decisão. (`app/chamados/services/fio.py:891`, `app/chamados/models.py:2720`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:762`, `app/chamados/services/fio.py:847`)

Não identifiquei inversão **direta nova** de Disputa→Colaborador nesses helpers: eles não adquirem `select_for_update` de Disputa. O callback da pergunta agenda becos em `on_commit`. Isso não constitui prova geral contra deadlock; o próprio selo declara que não analisa chamadas entre funções. (`app/chamados/services/fio.py:742`, `app/chamados/services/fio.py:878`, `app/chamados/signals.py:191`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`)

**4. Relógio e orçamento de queries**

O método novo não lê relógio diretamente nem recebe instante. Entretanto, chama C6, que pode chegar ao `date.today()` decisório preexistente de `_dentro_da_janela_viva`. Portanto, não introduz essa leitura, mas amplia seu alcance dentro de `abrir` sem injetar relógio da borda. (`app/chamados/models.py:434`, `app/chamados/models.py:475`, `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:562`)

Os `now()` de retratação e lavra são carimbos de escrita preexistentes; devem ser distinguidos daquele relógio decisório. A composição da competência recebe o dia explícito. (`app/chamados/services/fio.py:884`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/services/vigia_fio.py:838`)

**Há custo por candidata:** a busca carrega somente `disputa`, enquanto C6 acessa célula, colaborador, empresa e tranca. O helper não aplica o mapa de identidade usado pela migração; `_st` resolve em lote apenas os estados das âncoras. (`app/chamados/models.py:464`, `app/chamados/models.py:469`, `app/chamados/juizes.py:801`, `app/chamados/juizes.py:932`, `app/chamados/services/fio.py:927`)

Os selos citados não certificam esse orçamento:

- `test_c2_corrida_do_abrir` usa módulo `sem_previsao_escala`, que sai na primeira guarda C20; mede deduplicação concorrente, não queries da adoção. (`app/chamados/tests/test_c2_corrida_do_abrir.py:56`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `app/chamados/models.py:452`)
- O chokepoint mede batida/reconciliação/estado; seu `abrir` de preparação está fora da captura correspondente. Quebra efetiva de teto: **[não verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:110`, `app/ponto/tests/test_selo_chokepoint_escrita.py:312`)

**5. Idempotência**

No sucesso sequencial normal, a segunda adoção encontra a pergunta em `self`, inclui seu motivo em `_ja` e não reaponta nem repete `pergunta_reatada`. (`app/chamados/models.py:477`, `app/chamados/models.py:484`)

Isso **não garante zero duplicata**:

- Se já houver uma viva local, a órfã equivalente permanece intocada. (`app/chamados/models.py:484`)
- Uma duplicata respondida pode continuar viva: C6 examina a resposta antes da via de identidade, e a porta documenta explicitamente essa exceção. A frase nova “duplicata já é morta pelo C6” é falsa como afirmação geral. (`app/chamados/models.py:446`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`, `app/chamados/models.py:2702`)

Dois `abrir` da mesma chave serializam pelo colaborador; o segundo retorna o existente sem nova adoção. Dois `reabrir` diretos não têm essa proteção integral. Ambas as execuções podem selecionar a mesma órfã antes da costura; a porta trava o destino, mas salva a pergunta recebida sem refazer a seleção. Duplicação de evento em corrida é um contraexemplo estático possível; reprodução: **[não verificado]**. (`app/chamados/models.py:940`, `app/chamados/models.py:959`, `app/chamados/models.py:1408`, `app/chamados/models.py:464`, `app/chamados/services/fio.py:845`)

“Uma linha” também depende do sucesso da auditoria: `evento` é best-effort e pode retornar sem gravá-la. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`)

**6. Contraexemplos e fronteiras**

| Caso | Resultado demonstrável |
|---|---|
| Outro dia ou motivo | Fica fora do filtro. A busca também não alcança D±1 que E4-c admite como mesma jornada overnight; conformidade desse alcance com o aval: **[não verificado]**. (`app/chamados/models.py:464`, `app/chamados/juizes.py:1991`) |
| Outro módulo | Outro módulo de **destino novo** sai pela constante; módulo diferente da **âncora antiga terminal** não impede adoção. (`app/chamados/models.py:432`, `app/chamados/models.py:452`, `app/chamados/models.py:473`) |
| Morta por C6 ou validada | Não é adotada. A exceção de duplicata respondida segue a precedência do próprio C6. (`app/chamados/models.py:475`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`) |
| Competência entregue | Sai antes da busca. O renascimento admite competência exportada, portanto pode reabrir sem recuperar a órfã por esta cura. A exclusão copia a cerca existente; sua conciliação com L-113 permanece **[não verificado]**. (`app/chamados/models.py:460`, `app/chamados/models.py:347`, `app/chamados/models.py:1030`, `app/docs/LEIS.md:150`) |
| Agrupado/limbo | As entradas normais protegem agrupado sob pai vivo; o helper isolado não consulta agrupamento. Chamado de outro módulo, como destino de limbo, sai pela guarda inicial. (`app/chamados/models.py:960`, `app/chamados/models.py:1442`, `app/chamados/models.py:452`) |
| Pai de adesão/nunca-bateu | O pai de adesão usa outro módulo e não adota. Entretanto, uma órfã preexistente de um `batida_ausente` nunca-bateu pode passar: `nunca_bateu` acusa no C6. R19d não cobre esse estoque. (`app/chamados/services/adesao.py:74`, `app/escala/models.py:1767`, `app/chamados/juizes.py:1181`, `app/chamados/models.py:464`) |
| Destino terminal ou nenhum | Coberto quando C6 permite. “Sem destino” com pai vivo não é órfã: a âncora efetiva é o pai, preservado pela guarda. (`app/chamados/services/fio.py:117`, `app/chamados/models.py:473`, `app/chamados/models.py:492`) |
| Renascer por premissa | Alcança a adoção se efetivamente passar por `reabrir`; existente já vivo retorna sem reparar órfãs. (`app/chamados/models.py:1055`, `app/chamados/models.py:1478`, `app/chamados/models.py:959`) |

**Pouso antes do pré-requisito:** o método retrata irmãs antes de saber se a canônica será aceita. Exemplo estático: célula em D e contexto em D+2; a seleção usa D pela célula, E4-c compara o contexto e recusa, mas as retratações anteriores permanecem. A abertura continua. Execução desse cenário: **[não verificado]**. (`app/chamados/juizes.py:90`, `app/chamados/juizes.py:1973`, `app/chamados/juizes.py:1998`, `app/chamados/models.py:487`, `app/chamados/models.py:493`)

**7. Os cinco atos de perda**

| Ato | Cobertura |
|---|---|
| **Reabrir** | Novo gancho cobre reabertura efetiva de Chamado, após ressuscitar as próprias. `Disputa.reabrir_admin/sistema/por_colab` não chama diretamente a adoção; a casca administrativa chama `ch.reabrir` somente se encerrado. (`app/chamados/models.py:1477`, `app/chamados/models.py:2116`, `app/chamados/models.py:2153`, `app/chamados/models.py:2222`, `app/chamados/services/acoes_disputa.py:265`) |
| **Retratar duplicata** | A porta carimba a duplicata, mas não reata a canônica. Pode retirar a última viva de um chamado enquanto a canônica permanece num terminal; o diff não acrescenta evento nessa operação. (`app/chamados/services/fio.py:878`, `app/chamados/models.py:1009`, `app/chamados/models.py:1478`) |
| **Validação** | Já possui transação para carimbo/materialização/desfecho e recalcula por evento. A morte por validação é legítima; não exige irmã nova. O diff não acrescenta gatilho C20 aqui. (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:405`, `app/chamados/juizes.py:1136`) |
| **Morte por leitura** | C6 já resolve tranca, marco e célula; E2 carimba absolvidas pelo julgamento existente. Não há nova adoção nessa leitura. Morte legítima não demonstra perda indevida de cobertura. (`app/chamados/juizes.py:1138`, `app/chamados/juizes.py:1177`, `app/chamados/reconciliador.py:651`, `app/ponto/services/cartorio.py:958`) |
| **Reapontar** | A porta existente protege o **destino**, valida e lavra no mesmo save. Não verifica o chamado de origem que perde sua última pergunta. A legitimidade de deixar essa origem sem fio precisa ser demonstrada por caso. (`app/chamados/services/fio.py:784`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`) |

Assim, os dois ganchos novos não demonstram, sozinhos, cobertura dos cinco atos. (`app/chamados/models.py:1009`, `app/chamados/models.py:1478`)

**8. Mordida dos testes e R19d** [nao verificado]

R08/R08b exigem cobrança, destino correto, fio vivo e nenhuma irmã; R08r e R19b incluem guardas explícitas de vivacidade. São as verificações mais fortes do arquivo, mas execução RED e eficácia por mutação permanecem **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:125`, `app/chamados/tests/test_c20_fio_mudo.py:164`, `app/chamados/tests/test_c20_fio_mudo.py:204`)

Há lacunas concretas:

- **R19a pode passar sem nascer cobrança:** não exige cobrança não nula e compara contra `getattr(..., None)`. (`app/chamados/tests/test_c20_fio_mudo.py:174`)
- **R19c não exerce adoção futura:** roda o vigia de hoje e consulta ausência amanhã, sem semear órfã futura nem abrir cobrança futura. (`app/chamados/tests/test_c20_fio_mudo.py:210`)
- O controle de tranca é anunciado, mas não há caso executável correspondente; também faltam segunda adoção após sucesso, trilha única, corrida C20, grupo respondido/duplicado e orçamento de queries. (`app/chamados/tests/test_c20_fio_mudo.py:11`, `app/chamados/tests/test_c20_fio_mudo.py:91`)

**R19d procede para dispensar questionário de quem nunca bateu:** o cartório carimba sem emitir, e adesão declara que a ação pertence ao supervisor/RH. Porém, a mesma lei manda agrupar os furos no pai próprio; ela não comprova legitimidade indefinida de qualquer filho diário vivo. R19d não verifica pai nem agrupamento. (`app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1268`, `app/chamados/services/adesao.py:19`, `app/chamados/services/adesao.py:102`, `app/chamados/tests/test_c20_fio_mudo.py:106`)

Por fim, os números reproduzidos no teste somam **495**; retirados os três nunca-bateu, restam **492**, não o universo literal **494**. Reconciliação dos universos e comprovação individual das exclusões: **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:1`, `app/chamados/tests/test_c20_fio_mudo.py:13`)

## 05/11 22:03 · sessao 107544328c40 · - · - · origem=Ronald (tmux)

Achados principais:

- **`reabrir()` não tem a mesma transação/trava de `abrir()`.** Lê estado sem `select_for_update`, grava estado em transação interna e só depois ressuscita/adota. A unidade estado+adoção depende de transação externa do chamador. `abrir()` garante `atomic` e trava do colaborador. (`app/chamados/models.py:937`, `app/chamados/models.py:940`, `app/chamados/models.py:1408`, `app/chamados/models.py:274`, `app/chamados/models.py:1477`)

- **Corrida/idempotência concorrente não garantida.** O helper coleta perguntas/âncoras antes de costurar; a porta trava o destino, mas não relê nem trava a pergunta antes de decidir. Duas execuções podem selecionar a mesma órfã e ambas salvar/registrar o evento; reprodução concorrente [não verificado]. (`app/chamados/models.py:464`, `app/chamados/models.py:473`, `app/chamados/models.py:496`, `app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`)

- **Continente fechado continua fechado.** O helper admite perguntas de qualquer disputa e não reabre a disputa; C6 pode considerá-las vivas, mas `_disputa_do_chamado` só retorna disputa aberta. Assim, existe contracaso estático de destino vivo com fio vivo, mas lookup administrativo ausente; ocorrência em produção [não verificado]. (`app/chamados/models.py:464`, `app/chamados/juizes.py:1135`, `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:135`) A ressuscitação existente reabre o continente quando altera perguntas; a adoção não executa isso. (`app/chamados/models.py:1580`, `app/chamados/models.py:1607`)

Inventário:

| Campo/ato | Escritores existentes e utilização nova | Leitores relevantes |
|---|---|---|
| `chamado_destino` | `PerguntaDisputa.abrir` cria e reaponta identidade; `fio.reapontar_resolvedora` costura. Helper novo apenas chama a porta. (`models.py:2526`, `models.py:2552`, `services/fio.py:845`, `models.py:492`, todos em `app/chamados/`) | Âncora: `services/fio.py:116`; C6/memo: `juizes.py:817`; materialização: `juizes.py:743`; plano/gate: `models.py:344`; órfãs: `reconciliador.py:1480`. |
| `via_resolucao` | Núcleo `_escrever_carimbo`, portas `carimbar_identidade`/`descarimbar`/lote; exceções existentes `encerrar_por_fato_no_espelho` e `relavrar`. Novo dedup chama `retratar_duplicata` → `carimbar_identidade`. (`app/chamados/models.py:2632`, `app/chamados/models.py:2701`, `app/chamados/models.py:2722`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/fio.py:891`) | C6: `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`; migrador: `app/chamados/services/fio.py:935`; ressuscitação: `app/chamados/models.py:1531`. |
| `validada_em` | Validação administrativa e devolução; simetria pela porta `carimbar`; desfazimento por resposta/correção/ressuscitação. Nenhum escritor direto novo. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/models.py:2668`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1580`) | C6: `app/chamados/juizes.py:1136`; estado: `app/chamados/models.py:2575`; ancoradas: `app/chamados/models.py:327`; reconciliação: `app/chamados/reconciliador.py:620`. |
| `status_local` | Núcleo `_gravar_estado`; nascimento pelas portas `abrir`/`criar`. Helper novo somente lê. (`app/chamados/models.py:222`, `app/chamados/models.py:1006`, `app/chamados/models.py:1131`, `app/chamados/models.py:452`, `app/chamados/models.py:469`) | Vigia: `app/chamados/services/vigia_fio.py:335`; gate: `app/chamados/models.py:400`; costura: `app/chamados/services/fio.py:764`; C6/materialização: `app/chamados/juizes.py:744`. |
| `pergunta_reatada` | Gate antigo e helper novo chamam `core.observ.evento`; antigo associa à âncora que fecha, novo ao destino que abre. (`app/chamados/models.py:423`, `app/chamados/models.py:496`) | Teste específico: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:302`; auditoria genérica: `app/core/views.py:705`. Consumidor de produção específico por nome [não verificado]. |
| Lavra | `veredito_lavrado.lavrar` atribui os três campos; `reapontar_resolvedora` salva destino e lavra juntos. (`app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:845`) | Cache/staleness: `app/chamados/veredito_lavrado.py:173`; C6 consulta o juiz de materialização: `app/chamados/juizes.py:1214`. |

**Não identifiquei segundo escritor direto introduzido pelo diff:** a lista autorizada do destino já contém `models.py` e `fio.py`, e a nova chamada usa `reapontar_resolvedora`. (`app/core/tests/test_contract_escritores_estado.py:60`, `app/chamados/models.py:492`)

**Travas:** `canonica_do_grupo` é cálculo em memória; competência compõe consultas; `retratar_duplicata` atualiza pergunta pela porta; `reapontar_resolvedora` trava chamado destino antes de atualizar a canônica. Não há nova trava explícita de `DisputaSupervisao` nesses helpers, portanto não há inversão direta nova de Disputa→Colaborador. (`app/chamados/services/fio.py:873`, `app/chamados/services/vigia_fio.py:834`, `app/chamados/services/fio.py:891`, `app/chamados/models.py:2720`, `app/chamados/services/fio.py:762`, `app/chamados/services/fio.py:847`) O hook da pergunta agenda becos somente em `on_commit`; o selo da ordem admite não enxergar inversões entre funções. (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`)

**Idempotência sequencial:** sucesso move a pergunta para `self`; segunda chamada marca o motivo em `_ja` e pula escrita/evento. Isso não prova “zero duplicata” global: uma órfã do mesmo motivo é ignorada quando já existe viva em `self`, sem retratá-la. (`app/chamados/models.py:477`, `app/chamados/models.py:484`) Duplicata respondida também pode permanecer viva pela precedência da resposta em C6. (`app/chamados/models.py:2704`, `app/chamados/juizes.py:1150`)

**“Uma linha” não é garantia absoluta:** `evento` é best-effort e pode retornar sem linha após falha; o teste novo não conta `LogAuditoria`. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/chamados/tests/test_c20_fio_mudo.py:1`)

## 05/11 22:03 · sessao 7fd15dd1c688 · - · - · origem=Ronald (tmux)

Achados estáticos principais:

- **Pouso antes do pré-requisito:** o método retrata as duplicatas antes de saber se a canônica pode ser costurada; se a porta recusar, mantém as retratações e segue a abertura (`app/chamados/models.py:487-495`; `app/chamados/services/fio.py:837-849`). Contraexemplo possível: célula em D, contexto em D+2; `dia_do_fato` escolhe D pela célula, mas E4-c compara o contexto e recusa D×D+2 (`app/chamados/juizes.py:90-92`, `:1973-1999`). Execução desse exemplo: [não verificado].
- **Dedup incompleto:** havendo viva já ancorada no destino, `_ja` pula todo o grupo órfão; não aplica canônica nem retrata a irmã (`app/chamados/models.py:477-490`). A migração existente compara origem+destino e aplica a canônica ao grupo inteiro (`app/chamados/services/fio.py:937-945`).
- **Nenhuma cópia evidente de juiz:** dia, motivo, competência, viva e âncora são delegados às funções existentes (`app/chamados/models.py:454-475`); registro C1/C5/C6/C8/C14/C15 em `app/core/juizes.py:260-277`. `_plano_pergunta_viva` parte das perguntas ancoradas e procura sobrevivente pelo C8/E4-c; o método novo parte da cobrança e procura perguntas do colab-dia-motivo (`app/chamados/models.py:364-377`, `:464-475`). Igualdade de data limita a busca, enquanto E4-c permite D±1 da mesma jornada overnight (`app/chamados/juizes.py:1957-1959`, `:1991-1997`); conformidade dessa limitação com o aval: [não verificado].

Contraexemplos e controles:

- Outro módulo de **destino novo** é recusado pela constante; outro módulo da **âncora antiga** não é recusado, desde que terminal (`app/chamados/models.py:432`, `:452`, `:473`).
- Outro dia/motivo fica fora do filtro; morta pelo C6 fica fora; competência entregue fica fora (`app/chamados/models.py:460-476`).
- Agrupado sob pai vivo é recusado antes da reabertura, e o `abrir` também deduplica filho agrupado (`app/chamados/models.py:1387-1388`, `:1442-1443`, `:960-967`). O helper isolado não pergunta por agrupamento (`app/chamados/models.py:452-492`).
- `nunca_bateu` pertence aos vereditos que acusam e pode manter uma pergunta existente viva pelo C6 (`app/escala/models.py:1767-1768`; `app/chamados/juizes.py:1181-1184`). Assim, órfã prévia pode ser adotada nesse cenário; o método não tem exclusão própria de adesão (`app/chamados/models.py:464-492`). Isso tensiona “não há o que responder pelo questionário” de `app/chamados/services/adesao.py:19-21`, mas proibir criar pergunta nova não implica automaticamente proibir reatar uma existente: [não verificado].
- Pergunta viva em disputa fechada pode ser adotada porque o filtro não exige disputa aberta (`app/chamados/models.py:464-465`); o app continua filtrando disputas abertas (`app/api/views.py:1008`), enquanto o fio do admin deliberadamente mostra vivas em disputas fechadas (`app/chamados/services/fio.py:209-221`). Portanto “reatada” não prova retorno ao questionário do colaborador; reabrir essa disputa automaticamente contrariaria o precedente documentado em `app/chamados/services/fio.py:211-212`.
- Competência entregue: exclusão herdada de `_vivas_entre`, não novo juiz (`app/chamados/models.py:347`, `:460`). Porém o renascimento declara expressamente permitido em competência exportada e chama `reabrir` (`app/chamados/models.py:1030-1035`, `:1055`): pode renascer sem adotar órfã. A resolução da tensão com L-113/L-092 exige distinguir escopo de cura e lei; conformidade final: [não verificado].

Cinco atos:

1. **Reabrir:** coberto para Chamado pelo novo gancho, após ressuscitar próprias (`app/chamados/models.py:1477-1478`); renascimento passa por ele (`:1055`). Reabrir Disputa diretamente não chama adoção (`:2116-2171`, `:2218-2224`); a casca administrativa chama `ch.reabrir` somente se encerrado (`app/chamados/services/acoes_disputa.py:265-266`).
2. **Retratar duplicata:** porta preserva identidade/canônica, mas não reata a canônica (`app/chamados/services/fio.py:878-895`). Pode matar a última pergunta de um chamado vivo enquanto a canônica continua num terminal; o gancho novo não roda nessa operação (`app/chamados/models.py:1007-1009`, `:1478`). `abrir` sobre existente vivo retorna sem adoção (`:956-959`).
3. **Validação:** já atomiza carimbo, materialização e devolução de recusa e recalcula por evento (`app/chamados/services/validacao.py:395-427`). Validada morre legitimamente pelo C6 (`app/chamados/juizes.py:1136-1137`); essa morte não exige pergunta nova.
4. **Morte por leitura:** C6 já decide tranca, marco e célula (`app/chamados/juizes.py:1138-1146`, `:1177-1196`). E2 carimba absolvidas, consulta C6 e poupa trancadas (`app/chamados/reconciliador.py:582-604`, `:651-658`), acionado pelo cartório (`app/ponto/services/cartorio.py:958`). Morte legítima sai do universo; não demonstra lacuna por si.
5. **Reapontar:** já protege destino pela validação E4-c, trava, lavra e save conjunto (`app/chamados/services/fio.py:837-849`). Não verifica o chamado de origem que perdeu a última pergunta; preservar o fio da origem não é efeito garantido dessa porta (`:784`, `:845-848`). Se a transferência torna a origem legítima sem pergunta, essa exclusão precisa de prova própria: [não verificado].

## 05/11 22:02 · sessao 7de248e93add · - · - · origem=Ronald (tmux)

4. **Custo e relógio**

- O método novo não chama `now()`/`today()` diretamente, mas não recebe nem repassa instante: `app/chamados/models.py:434-502`. A composição de competência usa o dia explícito: `app/chamados/services/vigia_fio.py:838`; `app/ponto/janelas.py:60-63`.
- Há relógio decisório indireto preexistente: `pergunta_viva` pode alcançar `date.today()` através de `_dentro_da_janela_viva`: `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:562`. A regra de relógio injetado consta em `app/docs/PROMPTS.md:1624`. O diff amplia o alcance desse caminho no `abrir`, sem criar essa leitura: `app/chamados/models.py:475`, `app/chamados/models.py:1009`.
- Há timestamps preexistentes dentro do laço: `retratar_duplicata` usa `timezone.now()` quando não recebe `agora`, e a lavra usa `timezone.now()`: `app/chamados/services/fio.py:884-891`; `app/chamados/veredito_lavrado.py:228`. São carimbos de escrita; não confundir com o relógio decisório anterior: mesmos locais.
- Há N+1 potencial concreto: candidatos carregam apenas `disputa`, enquanto C6 acessa `celula`, `disputa.colaborador`, empresa e tranca: `app/chamados/models.py:464-465`; `app/chamados/juizes.py:801`, `app/chamados/juizes.py:932-947`. Fora de request, o memo pertence à instância do colaborador: `app/chamados/juizes.py:966-979`. A adoção não aplica o mapa de identidade que a migração já aplica: `app/chamados/services/fio.py:927-930`.
- O lote `_st` evita consultas individuais de status das âncoras, mas não elimina as consultas de C6: `app/chamados/models.py:469`, `app/chamados/models.py:475`.
- `test_c2_corrida_do_abrir` não mede queries; seu módulo é `sem_previsao_escala`, portanto sai antes do ramo C20: `app/chamados/tests/test_c2_corrida_do_abrir.py:56`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127-150`; `app/chamados/models.py:452`.
- `test_selo_chokepoint_escrita` mede batida/reconciliação/estado; seu `abrir` de preparação fica fora de `CaptureQueriesContext`: `app/ponto/tests/test_selo_chokepoint_escrita.py:110-119`, `app/ponto/tests/test_selo_chokepoint_escrita.py:312-326`. Não certifica orçamento de adoção na abertura; quebra efetiva de teto: **[não verificado]**.

8. **Mordida e R19d**

- R08/R08b cobram nascimento, destino, fio vivo e nenhuma irmã: `app/chamados/tests/test_c20_fio_mudo.py:125-148`. R08r e R19b possuem guardas explícitas de pergunta viva antes da operação: `app/chamados/tests/test_c20_fio_mudo.py:164`, `app/chamados/tests/test_c20_fio_mudo.py:204`.
- R19a pode passar sem cobrança nova: não exige `_cobranca_do_dia()` não nula e compara contra `getattr(..., None)`: `app/chamados/tests/test_c20_fio_mudo.py:174-185`.
- R19c não testa adoção futura: executa vigia de hoje e apenas consulta ausência amanhã; não semeia pergunta futura nem chama abertura/adoção futura: `app/chamados/tests/test_c20_fio_mudo.py:210-213`.
- O anunciado controle de tranca não existe como teste: aparece na descrição, mas os controles executáveis são validada, outro vivo e amanhã: `app/chamados/tests/test_c20_fio_mudo.py:11`, `app/chamados/tests/test_c20_fio_mudo.py:173-213`.
- Não há teste de trilha única, segunda adoção após sucesso, corrida C20, grupo duplicado/respondido ou queries no arquivo novo: `app/chamados/tests/test_c20_fio_mudo.py:91-213`.
- R19d tem suporte literal na lei operacional: zero batida viva → dívida de adesão, cartório carimba e retorna sem emitir; o texto da adesão declara ausência de questionário: `app/ponto/services/cartorio.py:1154-1162`, `app/ponto/services/cartorio.py:1268`; `app/chamados/services/adesao.py:19-21`.
- Essa leitura é válida para dispensar **questionário** do nunca-bateu; não comprova legitimidade indefinida de todo filho diário vivo: adesão prevê pai próprio e agrupamento/reconciliação dos furos: `app/chamados/services/adesao.py:74-86`, `app/chamados/services/adesao.py:102-118`. O R19d não verifica pai nem agrupamento: `app/chamados/tests/test_c20_fio_mudo.py:106-122`.
- O dado reproduzido no teste soma 495 e, retirando os três nunca-bateu, resulta em 492; não demonstra o universo literal 494: `app/chamados/tests/test_c20_fio_mudo.py:13-15`. Reconciliação dos universos: **[não verificado]**.

**Achado adicional:** a afirmação nova “duplicata já é morta por C6” não vale para toda pergunta respondida: C6 pode retornar viva antes de examinar a via de identidade, e a própria porta documenta essa exceção: `app/chamados/models.py:446`; `app/chamados/juizes.py:1150-1172`; `app/chamados/models.py:2702-2708`. Duas candidatas respondidas podem permanecer vivas após retratar a não canônica; o método reaponta somente a canônica: `app/chamados/models.py:487-492`. Esse caso não tem teste C20: `app/chamados/tests/test_c20_fio_mudo.py:91-213`.

## 05/11 21:20 · sessao d0c9cbba8c22 · H12H06-A · verificador · origem=Code

**O delta corrige o caso do teste, mas a correção de duração continua incompleta:** o template ainda decide primeiro por `dias_corridos`, antes de consultar `fim_aberto`. (`app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`)

O material recebido não traz linhas da sonda identificando registros dessa rodada; apontar qual `colNN`/ID permanece errado em produção: **[não verificado]**.

**1. Quais casos ficam corrigidos ou errados**

As conclusões abaixo são por inspeção, sem execução: **[não verificado: resultado executado]**.

| Registro/condição | Resultado na célula após o delta |
|---|---|
| Tipo aberto, `data_fim=None`, `dias_corridos<=1`, sem parcialidade | Exibe “—”: corrige o caso denunciado. (`app/ponto/views.py:2243`, `app/templates/ponto/ausencias.html:164`) |
| Tipo aberto, `data_fim=None`, `dias_corridos=N>1` | **Continua exibindo “N dias”**, embora a cobertura prossiga até o fim da janela consultada. O primeiro ramo impede alcançar `fim_aberto`. (`app/templates/ponto/ausencias.html:160`, `app/ponto/turnos.py:2781`) |
| Range fechado de vários dias, mas `dias_corridos<=1` | **Continua exibindo “1 dia”**: `fim_aberto=False`, e a parcialidade também é falsa quando o fim difere do início. (`app/ponto/views.py:2243`, `app/ponto/turnos.py:2599`, `app/templates/ponto/ausencias.html:165`) |
| Range fechado com `dias_corridos=N>1` divergente das datas | **Continua exibindo N**, sem calcular a extensão inclusiva do range. A cobertura usa `data_fim`. (`app/templates/ponto/ausencias.html:161`, `app/ponto/turnos.py:2779`) |
| Tipo fora do catálogo, sem fim e `dias_corridos>1` | Exibe “N dias”, mas o juiz expande somente o dia inicial. Com `dias_corridos<=1`, “1 dia” coincide com essa expansão. (`app/templates/ponto/ausencias.html:160`, `app/ponto/catalogo/ausencias.py:194`, `app/ponto/turnos.py:2783`) |
| Tipo simultaneamente aberto e lançável por minutos, sem fim, com `0<minutos<480` e `dias_corridos<=1` | Exibe minutos antes de alcançar `fim_aberto`; a abertura continua sem representação na célula. Essa combinação de cadastro é oferecida sem exclusão entre os campos. (`app/ponto/views_tipos_ausencia.py:76`, `app/ponto/views_tipos_ausencia.py:78`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:163`) |

**Tipo inexistente não recebe permissão de abertura:** `fim_de` devolve `POR_LEI`, portanto `em_aberto_permitido` devolve falso. Ausência no catálogo atual também pode decorrer de inativação, pois a carga considera somente tipos ativos. (`app/ponto/catalogo/ausencias.py:194`, `app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:249`)

A divergência entre datas e `dias_corridos` é normalizada no `save`, mas a lista apenas lê o registro: o delta não normaliza nem salva os valores carregados. A existência desses registros em produção permanece **[não verificado]**. (`app/ponto/models.py:894`, `app/ponto/models.py:900`, `app/ponto/views.py:2238`, `app/ponto/views.py:2244`)

**2. Quem escreve e quem lê**

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `fim_aberto` | Atribuição transitória em cada objeto da lista; não é gravação no banco. (`app/ponto/views.py:2243`) | Ramo da duração no template. (`app/templates/ponto/ausencias.html:164`) |
| `Ausencia.data_fim` | Criação: `derivar_data_fim` → kwargs → `save`. Edição: rederiva, atribui e salva. O modelo também deriva fim quando há dias acima de 1 em tipo não aberto; transição de estado inclui o campo em `update_fields`; `rederivar_fim` reaplica o `save`. (`app/ponto/services/ausencia.py:197`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:499`, `app/ponto/services/ausencia.py:508`, `app/ponto/models.py:907`, `app/ponto/services/ausencia.py:818`, `app/ponto/services/ausencia.py:1280`) | No caminho da lista: seleção por vigência, juiz de parcialidade e cálculo de `fim_aberto`. Na cobertura: `dias_da_ausencia` e `cobertura_ausencia_periodo`. Há também consumidores no export, relatórios e drawer. (`app/ponto/turnos.py:3087`, `app/ponto/turnos.py:2599`, `app/ponto/views.py:2243`, `app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2760`, `app/folha/export.py:70`, `app/relatorios/views.py:205`, `app/templates/ponto/partials/painel_ausencia.html:124`) |
| Retorno de `em_aberto_permitido(tipo)` | A função calcula `fim_de(tipo) == INDETERMINADO`; não persiste o booleano. A alimentação vem do catálogo vigente, carregado do cadastro ativo ou do fallback. (`app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:192`, `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:278`) | A view nova, a validação de `indeterminado` no formulário e o comando que procura bloqueantes sem fim. (`app/ponto/views.py:2243`, `app/ponto/forms.py:85`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:24`) |

Quem alimenta `TipoAusencia.fim` pela tela é `salvar` do cadastro, passando os campos para `gravar_tipo_ausencia`, que cria ou altera o modelo. (`app/ponto/views_tipos_ausencia.py:76`, `app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1289`)

**Sim: para o mesmo catálogo vigente, `em_aberto_permitido(tipo)` e a pertinência a `Ausencia.TIPOS_EM_ABERTO` respondem à mesma pergunta com o mesmo conjunto:** tipos cujo `fim` é `indeterminado`. O segundo é alias de um conjunto vivo, não uma lista congelada diferente. (`app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:434`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/catalogo/ausencias.py:403`, `app/ponto/models.py:689`)

Isso responde à **permissão do tipo**. A duração do registro ainda depende de existir `data_fim`; `dias_da_ausencia` prioriza esse fim explícito. (`app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2781`)

**3. Mordida do teste** [nao verificado]

**Por inspeção, morde o comportamento anterior descrito no patch:** cria `afastamento_inss` sem fim, consulta a lista, isola a célula e rejeita “1 dia”, que era o fallback anterior. RED efetivamente executado em `58ac8543` e GREEN nesta cópia: **[não verificado]**. (`app/chamados/tests/test_h06_falta_pela_grade.py:168`, `app/chamados/tests/test_h06_falta_pela_grade.py:171`, `app/chamados/tests/test_h06_falta_pela_grade.py:177`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`; comportamento anterior no patch fornecido)

**Passaria com outra regressão:** célula vazia, “0 dias” ou “2 dias” satisfazem a única asserção sobre o conteúdo. O teste exige uma célula, mas não exige “—” nem prova a extensão da cobertura. Também não exercita os casos divergentes da tabela. (`app/chamados/tests/test_h06_falta_pela_grade.py:168`, `app/chamados/tests/test_h06_falta_pela_grade.py:178`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`)

**4. Outro defeito no delta**

O defeito funcional demonstrável é a **correção incompleta pela ordem dos ramos**, acompanhado da asserção insuficiente do teste; outro defeito introduzido pelo delta: **[não verificado]**. (`app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`)

A célula aberta também mantém `data-value` baseado em `dias_corridos`, mesmo exibindo “—”; esse atributo já está fora do hunk alterado, portanto não o atribuo como regressão nova deste delta. (`app/templates/ponto/ausencias.html:159`, `app/templates/ponto/ausencias.html:164`; patch fornecido)

## 05/11 21:12 · sessao a965ac098cf9 · H12H06-A · verificador · origem=Code

**Retifico a v1: a contagem do TXT, isoladamente, não demonstra regressão de A.** Para a mesma falta aprovada e as mesmas datas, o 8792 recebe a mesma quantidade com 720 ou zero minutos. A troca altera as horas calculadas, mas não introduziu a contagem por calendário. (`app/folha/export.py:70`, `app/folha/export.py:121`, `app/folha/export.py:218`, `app/folha/export.py:732`)

**1. Condição para falta aprovada com zero**

No escape da resposta, `FALTA_DIA` solicita `tipo='falta', status='aprovada'`. O materializador omite minutos; a porta fornece zero e `Ausencia.save` substitui esse valor pelo previsto do dia. **Uma falta nova permanece aprovada com zero se o previsto consultado nessa gravação resultar em zero.** (`app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:580`, `app/ponto/services/ausencia.py:130`, `app/ponto/models.py:877`, `app/ponto/models.py:885`)

O sítio `declara_falta` não faz a falta nascer aprovada: omite status, a porta aplica `estado_inicial` e a aprovação posterior passa novamente pelo `save`, incluindo a persistência de minutos derivados. (`app/chamados/services/materializacao.py:484`, `app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:818`)

O juiz devolve zero sem vínculo, sem tipo, quando `eh_dia_trabalho(...) is not True` ou quando faltam início/fim. **Com previsto positivo preservado até a gravação, esses sítios não produzem falta nova aprovada com zero.** (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/ponto/models.py:885`)

Antes disso, os emissores de falta impõem guardas que a v1 deixou de considerar:

- O detector exige vínculo, resultado de dia de trabalho e início previsto; seu funil exige célula, com exceção explícita para limbo. (`app/ponto/management/commands/detectar_ausencias.py:139`, `app/ponto/management/commands/detectar_ausencias.py:305`, `app/ponto/management/commands/detectar_ausencias.py:312`, `app/ponto/management/commands/detectar_ausencias.py:656`)
- A emissão retroativa exige `FURO_SEM_COBRANCA`. Esse ramo exige dia de cobrança, previsto não zero, realizado zero, nenhuma batida, ausência de cobertura integral e dia encerrado; depois o emissor exige vínculo e `eh_dia_trabalho is True`. (`app/ponto/management/commands/emitir_furo_retroativo.py:111`, `app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:313`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:139`)
- A porta de nascimento também veta chamados dos módulos abrangidos quando o dia está identificado e falta vínculo ou a régua é degenerada. (`app/chamados/models.py:928`, `app/chamados/models.py:1097`, `app/ponto/services/furos_vetados.py:40`)

Essas condições não constituem uma guarda universal `previsto > 0` para toda pergunta: a fábrica da célula consulta `dia_sem_trabalho`, situação, data, veredito e marcos; a fábrica de órfãos existentes admite explicitamente continuar sem vínculo. **Isso não prova nascimento atual de chamado sem escala através da porta de emissão.** (`app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:840`, `app/chamados/models.py:1105`)

Há uma possibilidade condicional no código de produção: pergunta já existente continua aceita, mas o previsto na materialização passa a zero. Por exemplo, uma pergunta anteriormente respondida, sem validação, sem tranca e sem decisão administrativa pode continuar viva pelo texto; os materializadores não revalidam previsto positivo. Ocorrência desse cenário no dado fornecido: **[não verificado]**. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`, `app/api/views.py:2631`, `app/chamados/services/materializacao.py:516`)

O teste sem escala cria chamado e pergunta diretamente; portanto não demonstra que os emissores atuais produziriam essa situação. (`app/api/tests/test_falta_na_resposta.py:26`, `app/api/tests/test_falta_na_resposta.py:30`, `app/api/tests/test_falta_na_resposta.py:34`)

**2. T1–T4 e alterações numéricas**

- **T1:** os leitores citados realmente usam minutos não zero para crédito e para pular a justificação integral. Com os valores novos descritos — falta derivada e atestado zero — não aparece a divergência apontada para atestados antigos de 720. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2598`)
  
  **A ressalva é causal:** A também muda números de **atestados novos desses sítios**, depois de aprovados. Antes entravam com 720; agora entram com zero. Nos leitores, isso troca crédito 720 por zero e permite justificar o dia quando o efeito é ABONA. Não depende de migrar legado. (`app/chamados/services/materializacao.py:522`, `app/ponto/models.py:877`, `app/ponto/services/fechamento.py:669`, `app/ponto/services/fechamento.py:690`)
  
  Exemplo deduzido: dia obrigatório sem trabalho, atestado aprovado ABONA, sem justificativa ou cobertura concorrente. A inclusão nas justificadas remove o dia das faltas injustificadas; se era a única falta injustificada da semana, altera também DSR perdido para DSR OK, contadores persistidos pelo fechamento. **É alteração numérica causada pelo diff; por si só, não demonstra defeito.** (`app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2629`, `app/ponto/motor_calculo_v2.py:2654`, `app/ponto/services/fechamento.py:1085`)

- **T2:** a expansão usa datas e calendário, sem minutos ou grade; o TXT conta `len(datas)`. A divergência entre horas zero e quantidade de dias não foi criada por A. (`app/ponto/turnos.py:2774`, `app/folha/export.py:70`, `app/folha/export.py:121`, `app/folha/export.py:218`)

- **T3:** os sítios citados permanecem fora dos hunks funcionais de A; os dois materializadores alterados criam ausência de um dia, sem solicitar range. Não identifiquei nesses pontos outro caminho numérico introduzido pelo diff. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/folha/export.py:118`, `app/ponto/models.py:902`; hunks fornecidos no anexo)

- **T4:** o teste chama atestado aprovado diretamente; o escape real usa status vazio e deixa `estado_inicial` decidir. Em ambos, atestado permanece com zero porque a derivação exige tipo literal `falta`. (`app/chamados/tests/test_h06_falta_pela_grade.py:101`, `app/chamados/services/materializacao.py:581`, `app/ponto/services/ausencia.py:344`, `app/ponto/models.py:877`)

**3. Achado novo no diff**

**Duração de afastamento aberto passa a ser apresentada como “1 dia”.** Um registro de tipo em `TIPOS_EM_ABERTO`, com `dias_corridos=1`, minutos zero e `data_fim=None`, é permitido pela porta; sua cobertura pode continuar por vários dias. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:70`, `app/ponto/models.py:902`, `app/ponto/turnos.py:2781`)

A view aplica `cobre_parte_do_dia` a todos os registros. Zero retorna falso e o novo `else` imprime “1 dia”; no hunk anterior, esse caso imprimia “—”. O predicado de parcialidade não determina a duração total de um afastamento aberto. **É uma regressão de apresentação causada pelo diff.** Ocorrência em produção e impacto financeiro: **[não verificado]**. (`app/ponto/views.py:2238`, `app/ponto/views.py:2240`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`; comportamento anterior no hunk fornecido)

## 05/11 21:11 · sessao 0a411a29b6d1 · - · - · origem=Ronald (tmux)

Para falta **nova**, o escape `FALTA_DIA` passa `('falta', 'aprovada')`; ambos os sítios omitem minutos, a porta aplica default zero e `save` substitui esse zero pela soma do previsto. Portanto fica aprovada com zero quando o juiz responde zero na ocasião da gravação. O ramo `declara_falta` nasce `aguardando_decisao`; só deriva quando posteriormente aprovado. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:580`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:781`, `app/ponto/models.py:877`, `app/ponto/models.py:885`)

O previsto pode ser zero por ausência de vínculo/tipo, por `eh_dia_trabalho(...) is not True` ou ausência de início/fim. Esses materializadores não perguntam novamente se existe furo, célula de trabalho ou previsto positivo. Em `retornar_mesmo_tipo`, o veto de precedência nem é executado; uma linha existente também pode voltar sem novo `save`. (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/chamados/services/materializacao.py:467`, `app/chamados/services/materializacao.py:516`, `app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:377`)

**A guarda dos emissores de falta não pode ser omitida**, como foi na v1:

- O detector exige vínculo, dia com trabalho e início previsto; o funil ainda exige célula, ressalvada a exceção declarada de limbo. (`app/ponto/management/commands/detectar_ausencias.py:139`, `app/ponto/management/commands/detectar_ausencias.py:305`, `app/ponto/management/commands/detectar_ausencias.py:312`, `app/ponto/management/commands/detectar_ausencias.py:656`)
- A emissão retroativa exige `FURO_SEM_COBRANCA`; esse diagnóstico exige dia de cobrança, previsto não zero, realizado zero, nenhuma batida, ausência de cobertura integral e turno encerrado. Depois, o emissor exige vínculo, `eh_dia_trabalho is True` e início. (`app/ponto/management/commands/emitir_furo_retroativo.py:101`, `app/ponto/management/commands/emitir_furo_retroativo.py:111`, `app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:313`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:139`)
- Os crons de turno aberto também calam sem vínculo; a porta de nascimento do chamado consulta `motivo_do_veto`, que veta vínculo ausente. (`app/ponto/management/commands/processar_alertas_avancados.py:37`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `app/chamados/models.py:1097`, `app/chamados/models.py:1105`, `app/ponto/services/furos_vetados.py:40`)

**As guardas de perguntas são mais amplas que as do detector de falta total.** A fábrica da célula exige ativo, dia que `dia_sem_trabalho` não dispense, data não futura e veredito vazio ou acusador; não consulta diretamente `minutos_previstos > 0`. Dia passado sem marcos pode seguir por `classificar_falta`, e `perguntar` fornece contexto datado para criar disputa/pergunta, inclusive sem destino derivável. (`app/chamados/services/fabrica_da_celula.py:27`, `app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:389`)

Na fábrica de órfãos existentes, `_tem_base` aceita batidas sem escala e `_ata_do_dia` permite explicitamente continuar sem célula quando não há vínculo. `classificar_falta` também tem ramo binário por E/S e fallback sem marcos. Isso demonstra tratamento de **chamado existente**, não prova que um chamado novo ultrapasse o veto atual de nascimento sem vínculo. (`app/chamados/services/disputa_emissao.py:750`, `app/chamados/services/disputa_emissao.py:753`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:840`, `app/escala/servico_jornada.py:453`, `app/escala/servico_jornada.py:478`, `app/chamados/models.py:1105`)

Assim, o caminho defensável para zero é uma pergunta já disponível cujo previsto seja zero quando respondida/aprovada — por exemplo, alteração posterior de vínculo/grade — porque o materializador não revalida as condições de emissão. Ocorrência desse cenário na produção fornecida: **[não verificado]**. O teste sem escala fabrica diretamente chamado e pergunta; não demonstra sua emissão pelos crons. (`app/api/tests/test_falta_na_resposta.py:27`, `app/api/tests/test_falta_na_resposta.py:30`, `app/api/tests/test_falta_na_resposta.py:34`, `app/chamados/services/materializacao.py:516`, `app/ponto/models.py:885`)

Não encontrei emissor atual do motivo `declara_falta` no catálogo de perguntas; o caminho materializador continua presente. A existência/origem de perguntas desse motivo em produção permanece **[não verificado]**. (`app/chamados/catalogo/perguntas.py:34`, `app/chamados/services/materializacao.py:1047`)

## 05/11 21:10 · sessao a8bef660087b · - · - · origem=Ronald (tmux)

Há um achado novo de apresentação: **ausência com término aberto pode aparecer como “1 dia”**. A porta aceita `dias_corridos=1`, `minutos=0` e preserva `data_fim=None` para tipo em `TIPOS_EM_ABERTO`; a cobertura desse registro continua até o limite consultado, podendo abranger vários dias. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:70`, `app/ponto/models.py:902`, `app/ponto/turnos.py:2781`)

A view aplica `cobre_parte_do_dia` a todos os registros; com zero minutos, o predicado retorna falso. O novo `else` do template apresenta então **“1 dia”**, enquanto o hunk anterior fornecido apresentava “—”. O predicado responde parcialidade do dia, sem determinar duração total de um afastamento aberto. (`app/ponto/views.py:2237`, `app/ponto/views.py:2240`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`; comportamento anterior: `diff_A.patch`, hunk de `app/templates/ponto/ausencias.html:160` fornecido pelo usuário)

O caminho HTTP permite esse registro: dias omitidos viram 1; minutos vazios viram 0; o parâmetro indeterminado chega à porta de criação. Ocorrência em produção e impacto financeiro desse achado: **[não verificado]**. (`app/ponto/views.py:2380`, `app/ponto/services/ausencia.py:95`, `app/ponto/views.py:2405`, `app/ponto/views.py:2412`)

Em H12, os sítios inspecionados usam o mesmo predicado e a janela da empresa: classificação, resumos, contador e marcação da linha. Não identifiquei outro defeito causal nesses hunks por inspeção. (`app/folha/export.py:60`, `app/folha/export.py:807`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:104`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`)

## 05/11 21:10 · sessao bbbf9f695f68 · - · - · origem=Ronald (tmux)

T1 está correta sobre a idade dos leitores e a concordância com os valores novos de A: falta deriva minutos, mas não pertence a `TIPOS_PAGOS`; atestado zero não gera crédito e, sendo ABONA, justifica o dia. O juiz de cobertura também considera zero dia inteiro. (`app/ponto/models.py:683`; `app/ponto/catalogo/ausencias.py:95`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`; `app/ponto/turnos.py:2598`)

**Ressalva de causalidade:** isso não significa ausência de delta financeiro em A. O diff troca **os atestados novos desses sítios de 720 para zero**; portanto muda entradas dos leitores antigos mesmo sem migrar as 18 linhas antigas. Os sítios omitem minutos, a porta usa zero e o modelo só deriva tipo `falta`. (`app/chamados/services/materializacao.py:484`; `app/chamados/services/materializacao.py:520`; `app/ponto/services/ausencia.py:130`; `app/ponto/services/ausencia.py:343`; `app/ponto/models.py:877`)

Exemplo deduzido do código, sem afirmar ocorrência em produção: atestado aprovado com efeito ABONA, dia obrigatório sem trabalho, sem Justificativa e sem cobertura concorrente:

- Com 720: crédito de 720 e exclusão da justificação; com zero: crédito zero e inclusão nas justificadas. (`app/ponto/services/efeito_ausencia.py:44`; `app/ponto/services/efeito_ausencia.py:51`; `app/ponto/services/efeito_ausencia.py:55`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`)
- No motor, esse dia sai de `faltas_injustificadas` e passa a `_abono`; mudam contagem e horas de falta/abono. Se era o único dia injustificado da semana, muda também semana DSR perdido para semana DSR OK. (`app/ponto/motor_calculo_v2.py:2600`; `app/ponto/motor_calculo_v2.py:2605`; `app/ponto/motor_calculo_v2.py:2610`; `app/ponto/motor_calculo_v2.py:2620`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/motor_calculo_v2.py:2654`)
- O fechamento persiste os contadores de DSR do motor; a sua `horas_falta` vem separadamente das faltas declaradas, portanto não se deve confundir a alteração de `resultado.horas_falta` com alteração automática desse campo persistido. (`app/ponto/services/fechamento.py:866`; `app/ponto/services/fechamento.py:987`; `app/ponto/services/fechamento.py:1085`)

Esse delta é coerente com atestado de dia inteiro; **não constitui por si só novo defeito**, mas uma descrição de T1 que atribua toda alteração numérica apenas à migração do legado fica incompleta. (`app/ponto/turnos.py:2583`; `app/ponto/services/fechamento.py:685`; `app/ponto/motor_calculo_v2.py:2600`)

T2: os fatos citados estão certos. `datas_no_periodo` usa `dias_da_ausencia`; a expansão usa calendário e limites do registro/janela, sem minutos e sem grade; TXT usa `len(datas)`. Uma mesma falta aprovada de um dia gera a mesma quantidade TXT com 720 ou zero. (`app/folha/export.py:63`; `app/ponto/turnos.py:2774`; `app/folha/export.py:87`; `app/folha/export.py:218`; `app/folha/export.py:732`)

T4: a fraqueza do teste existe, mas não altera o resultado de minutos. O teste usa atestado aprovado diretamente; a tabela real usa status vazio; a porta aplica `estado_inicial`, documento anexado conduz a `aguardando_decisao`, aprovação passa por `mudar_estado`, e nenhuma dessas etapas deriva minutos de atestado. (`app/chamados/tests/test_h06_falta_pela_grade.py:101`; `app/chamados/services/materializacao.py:581`; `app/ponto/services/ausencia.py:344`; `app/ponto/services/ausencia.py:763`; `app/ponto/services/ausencia.py:857`; `app/ponto/services/ausencia.py:808`; `app/ponto/models.py:877`)

A devolução de registro já existente preserva seu valor, sem `save`, de modo que repetir a resposta não converte atestado antigo 720 em zero. (`app/ponto/services/ausencia.py:294`)

## 05/11 21:09 · sessao 052f591a4700 · H12H06-SEPARA-TRAVA · censo · origem=Code

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 05/11 21:05 · sessao a77a07cefa5d · - · - · origem=Ronald (tmux)

Confirmado: o ramo `adesao is None` apenas lavra/carimba `CelulaDia`, condicionado a `apply_`, e retorna antes das emissões: `app/ponto/services/cartorio.py:1162`, `:1207`, `:1242`, `:1264`, `:1268`. A linha `:1223` é comentário.

Esse ramo não preenche `out['cods']`; o ramo normal preenche em `cartorio.py:1426`. Cluster e fábrica posteriores dependem dessas datas (`:986`, `:990`, `:996`). No signal, `julgar_celula` passa somente `[cel]` (`:1989`); assim, o ramo nunca-bateu puro não alcança Colaborador ou Chamado pelo cartório.

FKs confirmadas:

- `CelulaDia.ausencia → Ausencia`, `SET_NULL`: `app/escala/models.py:1796`, `:1797`.
- `Ausencia.documento_referencia → self`, `SET_NULL`: `app/ponto/models.py:789`, `:790`.
- `Ausencia.chamado_origem → ChamadoColaborador`, `SET_NULL`: `app/ponto/models.py:784`, `:785`.

Essas declarações incluem atualizações referenciais no escopo das exclusões; SQL emitido, ordem do Collector, posição desses updates relativamente ao `post_delete` e primeira aquisição efetiva de travas: **[nao verificado]**.

## 05/11 21:05 · sessao b5677bb09b8e · H12H06-A · verificador · origem=Code

**Parecer: reprovo A como concluído.** H12 está coerente por inspeção. Em H06, falta aprovada com zero minutos ainda pode produzir **um dia de falta no TXT**, embora o fechamento compute zero horas; também sobrevivem leitores financeiros com regra própria de parcialidade. (`app/folha/export.py:121`, `app/folha/export.py:218`, `app/folha/export.py:732`, `app/ponto/services/fechamento.py:866`, `app/ponto/services/fechamento.py:665`)

Execução da suíte, RED/GREEN na base e ocorrência/impacto em produção: **[não verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

O censo abaixo distingue quem fornece os valores de quem efetivamente os persiste: as portas recebem tipo/minutos/status; `criar_ausencia`, `editar_ausencia_registro`, `mudar_estado` e `Ausencia.save` fazem a gravação. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:792`, `app/ponto/models.py:868`)

| Campo/valor | Escritores | Leitores |
|---|---|---|
| `Ausencia.minutos` | Default zero; criação recebe `minutos`; edição recebe/zera; `save` deriva falta aprovada zero. (`app/ponto/models.py:755`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:877`) | Rateio da falta, fechamento, efeitos, crédito parcial, cobertura, apresentação/API/CSV/auditoria, discriminados abaixo. (`app/folha/export.py:121`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2600`) |
| `Ausencia.status` | Criação usa status fornecido ou `estado_inicial`; transição por `mudar_estado`; rejeição em lote de férias usa `update`. Migration histórica 0057 também escreve status. (`app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:1265`, `app/ponto/migrations/0057_a4_backfill_pendente.py:43`) | Derivação, cobertura, precedência, decisão visual, cobrança, filtros/telas, relatórios e vigias. (`app/ponto/models.py:877`, `app/ponto/turnos.py:2514`, `app/ponto/precedencia.py:273`, `app/ponto/services/dia_decidido.py:481`, `app/chamados/services/silencio_ausencia.py:77`, `app/ponto/services/vigia_ausencia.py:108`) |
| `Ausencia.tipo` | Criação e edição; os dois materializadores fornecem o tipo. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:497`, `app/chamados/services/materializacao.py:485`, `app/chamados/services/materializacao.py:521`) | Derivação literal de `falta`, catálogo/efeito, cobertura/range, precedência, leitores financeiros, cobrança e apresentação. (`app/ponto/models.py:877`, `app/ponto/models.py:842`, `app/ponto/turnos.py:2532`, `app/ponto/precedencia.py:381`, `app/ponto/services/fechamento.py:685`, `app/chamados/services/materializacao.py:602`) |
| `Colaborador.data_demissao` | Agendamento, desligamento efetivo e reativação, persistidos pela porta de colaborador. (`app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:211`, `app/colaboradores/services/colaborador.py:32`) | Predicado H12, prontidão, recortes de grade/cobrança/universo, ciclo do desligamento e apresentação, discriminados abaixo. (`app/folha/export.py:60`, `app/folha/export.py:1060`, `app/ponto/services/fechamento.py:423`, `app/escala/services/leitor_celula.py:351`, `app/folha/views.py:658`) |
| Chave `rescisao` | Somente `montar_resumos`. (`app/ponto/services/fechamento_lista.py:109`) | Linha/cor/badge e contador da view; o atributo HTML alimenta o filtro. (`app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/views.py:1215`, `app/templates/ponto/fechamento.html:117`) |
| `n_rescisao` | Soma de `r['rescisao']` na view. (`app/ponto/views.py:1215`) | Pílula do fechamento. (`app/templates/ponto/fechamento.html:65`) |
| `_DECLARA_FALTA_MAPA` | Declaração em `chamados/models.py`. (`app/chamados/models.py:3018`) | Materializador da pergunta; o selo inspeciona sua AST. (`app/chamados/services/materializacao.py:468`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:50`) |
| `cobre_parte` | Atributo transitório preenchido na lista pela chamada ao juiz existente. (`app/ponto/views.py:2240`) | Duração apresentada na lista. (`app/templates/ponto/ausencias.html:163`) |
| `sai_pela_rescisao` | Definição única do predicado. (`app/folha/export.py:54`) | `classificar_export` e `montar_resumos`. (`app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`) |
| `janela_competencia` | Wrapper de `janela_fechamento`; corte vem de `Empresa.dia_inicio_competencia`. (`app/folha/export.py:47`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`) | Export, resumos, prontidão, prévia, PDFs, relatórios e comandos, discriminados abaixo. (`app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:104`, `app/folha/export.py:1073`, `app/folha/previa.py:42`, `app/folha/validacao_pdf.py:42`, `app/relatorios/services.py:214`) |

Os fornecedores de `tipo/minutos/status` fora do diff também passam pela porta:

| Origem | Sítio |
|---|---|
| Lançamento administrativo, colaborador e API | `app/ponto/views.py:2412`, `app/ponto/views.py:2533`, `app/api/views.py:1820` |
| Veredito da célula e copiloto de ausência | `app/ponto/services/veredito_celula.py:315`, `app/ponto/services/checar_ausencia.py:46` |
| Regularização e abandono provisório | `app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76` |
| Espelho de férias | `app/ferias/models.py:385`, `app/ponto/services/ausencia.py:1186` |
| Edição de tipo/minutos | `app/ponto/views.py:2791` |
| Decisões individuais/em lote | `app/ponto/views.py:85`, `app/ponto/views.py:2287`, `app/ponto/views.py:2316`, `app/ponto/views.py:2640`, `app/ponto/views.py:2698` |
| Reverter rejeição; rejeitar férias | `app/ponto/views.py:2581`, `app/ferias/services.py:356` |
| Rejulgar status e rederivar fim | `app/ponto/management/commands/rejulgar_status_por_tipo.py:31`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46` |

Os demais leitores de **minutos**, inclusive fora do diff, são:

- Propriedade `horas`, admin e fotografias de auditoria. (`app/ponto/models.py:865`, `app/ponto/admin.py:44`, `app/ponto/services/ausencia.py:446`, `app/ponto/views.py:2347`, `app/ponto/views.py:2730`, `app/ponto/views.py:2768`, `app/ponto/views.py:2811`)
- Palavra/cartão/histórico, payload da API e CSV. (`app/ponto/services/dia_decidido.py:423`, `app/ponto/services/dia_decidido.py:488`, `app/ponto/services/dia_decidido.py:512`, `app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`)
- Juiz de parcialidade, lista e drawer. (`app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:163`, `app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:149`, `app/templates/ponto/partials/painel_ausencia.html:299`)

Os demais leitores de **tipo/status** concentram-se nestes sítios:

| Domínio | Leitores fora do diff |
|---|---|
| Cobertura, sobreposição e afastamento | `app/ponto/turnos.py:2509`, `app/ponto/turnos.py:2521`, `app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2705`, `app/ponto/turnos.py:2733`, `app/ponto/turnos.py:2793`, `app/ponto/turnos.py:3087` |
| Propriedades e apresentação | `app/ponto/models.py:711`, `app/ponto/models.py:723`, `app/ponto/models.py:732`, `app/ponto/services/dia_decidido.py:453`, `app/ponto/services/dia_decidido.py:476`, `app/api/views.py:1873`, `app/ponto/admin.py:44` |
| Precedência, grade, furo e cartório | `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:339`, `app/escala/servico_jornada.py:340`, `app/escala/utils.py:1209`, `app/escala/utils.py:1231`, `app/ponto/services/cartorio.py:355` |
| Chamados e cobrança | `app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:638`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/historico.py:129`, `app/escala/services/furos_diarios.py:223` |
| Relatórios, calendário, inteligência e férias | `app/relatorios/views.py:197`, `app/relatorios/views.py:277`, `app/colaboradores/services/calendario.py:674`, `app/colaboradores/services/detalhe.py:104`, `app/ponto/services/alertas_ausencia.py:48`, `app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `app/inteligencia/views.py:239`, `app/inteligencia/views.py:516`, `app/ferias/models.py:144` |
| Vigias e comandos | `app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:104`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:115`, `app/ponto/services/vigia_ausencia.py:120`, `app/ponto/services/vigia_ausencia_veredito.py:46`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:41`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27`, `app/inteligencia/management/commands/calcular_scores.py:12`, `app/inteligencia/management/commands/gerar_alertas.py:55` |

Para **`data_demissao`**, além dos leitores financeiros já citados, há leitores de disponibilidade, cadastro, julgamento, holerite e ciclo do vínculo. (`app/ponto/utils.py:180`, `app/escala/services/cadastro_realidade.py:444`, `app/ponto/services/cartorio.py:1096`, `app/ponto/supra_juiz.py:177`, `app/ponto/supra_juiz.py:194`, `app/holerite/matriz.py:88`, `app/colaboradores/services/vinculo.py:92`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:41`, `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:55`)

Os demais consumidores de **`janela_competencia`** incluem ausências, grade, férias, retenção, histórico da folha e serviços/comandos auxiliares. (`app/folha/export.py:166`, `app/folha/export.py:185`, `app/folha/export.py:337`, `app/folha/export.py:486`, `app/folha/export.py:1266`, `app/folha/views.py:529`, `app/folha/views.py:588`, `app/folha/services/vigia_pausa.py:123`, `app/folha/services/sem_furo.py:47`, `app/relatorios/cartao_pela_celula.py:24`, `app/ponto/services/vigia_pareador_celula.py:59`, `app/ponto/management/commands/censo_col900.py:89`, `app/ponto/management/commands/gerar_celulas.py:176`, `app/ponto/management/commands/lavrar_status_competencia.py:40`, `app/ponto/management/commands/mapa_divergencia.py:61`, `app/ponto/management/commands/mapa_divergencia.py:93`)

**Sim, sobraram regras próprias contraditórias:** os leitores financeiros de parcialidade; o TXT que conta falta zero; e apresentação que ainda ensina `>0 = parcial`. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/folha/export.py:218`, `app/templates/ponto/lancar_ausencia.html:70`)

**2. Condições exatas de `Ausencia.save` e caminhos de gravação**

A derivação acontece em **qualquer `save`**, sem exigir detectar um flip, quando simultaneamente:

- `status == 'aprovada'`;
- `tipo == 'falta'`;
- `not self.minutos`.

Ela soma o juiz do previsto, inclusivamente, de `data` até `data_fim or data`. Valores não zero são preservados. (`app/ponto/models.py:877`, `app/ponto/models.py:882`, `app/ponto/models.py:885`)

Para uma **falta nova** dos dois materializadores, não encontrei bypass: ambos omitem minutos, a porta aplica zero, instancia `Ausencia` e chama `save()` inteiro. A aprovação posterior inclui `minutos` e demais derivados em `update_fields`. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:818`)

Há duas distinções necessárias:

- **Atestado aprovado zero não deriva**, porque o teste exige tipo literal `falta`. Isso é o comportamento introduzido pelos sítios, não um bypass de criação. (`app/ponto/models.py:877`, `app/chamados/services/materializacao.py:523`)
- **Registro existente pode ser devolvido sem `save`**, inclusive uma falta já aprovada com zero. Portanto repetir a materialização não garante rederivação de legado. (`app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:299`)

O `update` de produção identificado rejeita exclusivamente férias; o admin é somente leitura; os receivers de ausência julgam/reconciliam consumidores. Não encontrei `bulk_create` ou `update` que aprove faltas oriundas desses sítios. Ausência absoluta de SQL dinâmico ou chamadas não atribuíveis: **[não verificado]**. (`app/ponto/services/ausencia.py:1265`, `app/ponto/admin.py:38`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/core/censo_escritas.py:43`)

Para **atestado aprovado com efeito ABONA no catálogo**:

| Leitor | Zero | 720 |
|---|---|---|
| Juiz de cobertura | Dia inteiro | Dia inteiro. (`app/ponto/turnos.py:2598`) |
| Lista | “1 dia” | “1 dia”. (`app/ponto/views.py:2240`, `app/templates/ponto/ausencias.html:163`) |
| Fechamento | Justifica o dia; não soma crédito parcial | Soma 720 como crédito e pula justificação do dia. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`) |
| `efeitos_da_ausencia` | Justifica o dia | Crédito 720; pula justificação. (`app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`) |
| `creditos_por_dia` | Nenhum crédito | Crédito 720 no dia inicial do registro. (`app/ponto/services/credito_parcial.py:24`) |
| Teto de pontualidade | Dia inteiro | Também dia inteiro, pelo juiz — divergindo da regra de justificação acima. (`app/ponto/turnos.py:2668`, `app/ponto/turnos.py:2689`, `app/ponto/services/fechamento.py:683`) |
| Drawer | Oculta detalhe de minutos; não pede período parcial | Mostra 720; também não pede período parcial. (`app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:296`, `app/ponto/views.py:2084`) |
| Palavra do dia | Sem duração numérica | Acrescenta 12h. (`app/ponto/services/dia_decidido.py:230`, `app/ponto/services/dia_decidido.py:488`) |
| API/CSV/admin | Valor cru zero | Valor cru 720. (`app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`, `app/ponto/admin.py:44`) |

O TXT efetivo recebe os eventos do fechamento e as datas de falta; não lê diretamente minutos de atestado nesse laço. Assim, a diferença zero/720 chega a ele pelos resultados anteriores do fechamento. (`app/folha/export.py:722`, `app/folha/export.py:732`)

**3. Folga, sem escala e sem célula**

Sem escala vigente, sem tipo, com `eh_dia_trabalho(...) is not True`, ou sem marcos de início/fim, o juiz devolve zero; a falta aprovada grava esse zero após passar pela derivação. (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/ponto/models.py:887`)

**Sem célula não implica zero:** a autoridade existente pode consultar o cálculo do vínculo/template. O diff reutiliza essa autoridade; não acrescenta fallback próprio. (`app/escala/models.py:1096`, `app/escala/models.py:1133`, `app/escala/utils.py:2290`, `app/ponto/models.py:881`)

O contraexemplo relevante é:

1. Os materializadores pedem `retornar_mesmo_tipo`; o veto de precedência só é executado no modo `veto`. Logo esses sítios não passam por esse veto ao criar a linha. (`app/chamados/services/materializacao.py:490`, `app/chamados/services/materializacao.py:526`, `app/ponto/services/ausencia.py:377`)
2. A falta pode nascer aprovada zero sem escala — situação expressamente aceita pelo teste alterado. (`app/api/tests/test_falta_na_resposta.py:44`, `app/api/tests/test_falta_na_resposta.py:48`)
3. O fechamento soma zero minutos/horas. (`app/folha/export.py:121`, `app/ponto/services/fechamento.py:866`, `app/ponto/services/fechamento.py:987`)
4. O TXT seleciona o registro por tipo/status/data e conta `len(datas)`: **um dia**, se o colaborador entrar no crivo e existir a rubrica. (`app/folha/export.py:87`, `app/folha/export.py:160`, `app/folha/export.py:218`, `app/folha/export.py:732`)
5. Em folga, a grade mantém explicitamente **folga**, apesar da falta aprovada; o seletor de datas do TXT não consulta essa decisão. (`app/escala/utils.py:1227`, `app/escala/utils.py:1231`, `app/folha/export.py:181`)

Portanto, **sim: zero significa nada nas horas, mas pode significar um dia no TXT**. Essa divergência impede certificar a migração dos leitores de H06. (`app/ponto/services/fechamento.py:866`, `app/folha/export.py:218`)

**4. H12: pergunta e janela**

Os quatro sítios pedidos estão alinhados: predicado único `data_demissao is not None and data_demissao <= fim`, classificador, chave dos resumos, linha e contador. (`app/folha/export.py:60`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`, `app/templates/ponto/_fech_linha.html:2`, `app/ponto/views.py:1215`)

A janela também coincide: classificador usa a empresa recebida; resumos usam `colab.empresa`, com cache por empresa; ambos chegam a `janela_fechamento`. Portanto respeitam cortes distintos, sem fixar 21 nesses leitores novos. (`app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:103`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`)

Os baldes de rescisão de `folha/views`, relatórios e retrato acompanham o classificador. (`app/folha/views.py:589`, `app/folha/views.py:594`, `app/relatorios/views.py:1658`, `app/relatorios/views_pend.py:58`, `app/folha/fora_da_folha.py:119`, `app/folha/retrato_folha.py:1157`)

Persistem duas diferenças, com perguntas distintas:

- `prontidao` chama de `"rescisao"` a cegueira de quem saiu **antes do início**; não usa o predicado H12. Isso não decide entrada no TXT, mas o vocabulário não representa a mesma classificação. (`app/folha/export.py:1060`, `app/folha/export.py:1081`, `app/folha/export.py:1083`)
- O raio-X ainda escreve `†deslig.` pela mera presença da data, inclusive futura; seu balde de rescisão, entretanto, lê o juiz. (`app/folha/views.py:658`, `app/folha/views.py:594`)

O corte de aposentadoria continua pendente na cópia: o universo exclui desligados anteriores à janela, mas o classificador ainda consulta seus fechamentos gravados; não existe `encerrado` nas escolhas e o vigia continua incluindo as projeções nulas. Isso é dívida anterior, não regressão do predicado H12 de A. (`app/docs/CORTES.md:270`, `app/ponto/services/fechamento.py:423`, `app/folha/export.py:777`, `app/ponto/models.py:399`, `app/ponto/services/fechamento.py:1680`)

**5. Classes de reprovação**

| Classe | Verificação |
|---|---|
| Segundo juiz | Não identifiquei juiz novo no diff: H12 extrai uma condição compartilhada; H06 chama o previsto e a cobertura existentes. K1/K2 continuam registrados em `classificar_export`. (`app/folha/export.py:54`, `app/folha/export.py:858`, `app/ponto/views.py:2240`, `app/core/juizes.py:298`) |
| Segundo escritor | Os sítios alterados continuam pela porta; aprovação persiste os derivados do modelo. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:818`) |
| Relógio fora da borda | Não aparece leitura nova de relógio nos hunks funcionais: o predicado H12 recebe fim; a derivação usa as datas da ausência. (`app/folha/export.py:54`, `app/ponto/models.py:882`, `app/ponto/services/fechamento_lista.py:104`) |
| Derivação paralela | Os dois literais foram removidos, mas sobreviveram os julgamentos financeiros de parcialidade e o contador de dias independente do valor zero. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/credito_parcial.py:24`, `app/folha/export.py:218`) |


[truncado em 20000 de 24439 caracteres]

## 05/11 21:05 · sessao 2b546d724cfa · - · - · origem=Ronald (tmux)

Consolidação das direções inversas; todas são estáticas, execução/primeira aquisição concreta de linhas **[nao verificado]**.

| Par/direção | Testemunha na mesma transação |
|---|---|
| **Colaborador → Empresa** | `normalizar_identificadores.py:106` atomic → C U `:109` → `colaboradores/services/colaborador.py:88` → E U command `:112` → `colaboradores/services/estrutura.py:27`. |
| **Empresa → Colaborador/Ausencia** | Não encontrei testemunha; zero global **[nao verificado]**. Escritores e exclusões administrativas detalhados abaixo. |
| **Chamado/Colaborador → Ausencia nova** | `regularizacao.py:89` atomic → Justificativa I `:91` ou U `:106` → receiver `ponto/signals.py:162/:165` → cartório K, com possíveis L de Chamado/Colaborador → loop `regularizacao.py:110/:111/:118` → `ausencia.py:461` → I `:418` ou reabertura U via `:291`. |
| **Chamado/Colaborador → Ausencia de dia seguinte** | `marcar_abandono_provisorio.py:22` atomic abrangente → loops pessoas `:43` e dias `:70` → garantir `:76`; K do fato anterior precede Ausencia seguinte. Condicionado ao ramo de K que efetivamente trava e à existência de outra iteração. |
| **Chamado/Colaborador → Ausencia de férias seguinte** | Atomic externo dos serviços de férias → `ausencia.py:1182` loop → criar `:1186`; K do dia anterior precede I/U seguinte. Atomic, override e famílias específicos pertencem ao inventário do agente de escritores. |
| **Chamado/Colaborador → nova Ausencia após D antiga** | `ferias/services.py:375` atomic → apagar `:385` → `ausencia.py:1254` D → K → `ferias/services.py:387` ag.save → `ferias/models.py:437` sync → `ausencia.py:1186` I em data nova. Condicionado ao K e ao dia novo faltante. |
| **Chamado/Colaborador → Ausencia de outra irmã** | `ponto/views.py:2631` atomic → loop `:2632/:2633`; K após primeiro U `ausencia.py:819` pode travar Chamado/Colaborador antes do U da próxima irmã. |
| **FechamentoMensal → Ausencia de outra irmã** | Mesmo atomic/loop `views.py:2631/:2632/:2633`: `ausencia.py:895` → U `:819`, carimbos U `:911`, invalidar `:930` → FM U `fechamento.py:1640` → próxima irmã U `ausencia.py:819`. Requer ≥2 irmãs e FM atingida. |
| **Colaborador → FechamentoMensal** | `justificativa.py:125` atomic → registrar batida `:173` → I `registro_batida.py:152` ou `:157` → onboarding `colaboradores/signals.py:87/:102` → C U `colaboradores/services/colaborador.py:61` → `Colaborador.save`, `models.py:439` → invalidar `justificativa.py:200` → FM U `fechamento.py:1640`. Guards onboarding: batida criada `signals.py:94`, onboarding false `:97`, usuário com last_login `:99`. |
| **FechamentoMensal → Colaborador** | `views.py:1793` atomic → invalidar `:1806` → FM U `fechamento.py:1640` → se saída informada e sem batida gerada `views.py:1809`, registrar `:1816` → I `registro_batida.py:152/:157` → mesmo receiver onboarding e C U acima. O K anterior do `j.save()` em `views.py:1800` pode já ter adquirido C; primeira aquisição C posterior a FM **[nao verificado]**. |
| **Colaborador → CelulaDia** | J1 acima: onboarding C U durante registrar batida `justificativa.py:173`; após retorno, `j.save()` `:193` → `ponto/signals.py:162/:165` → K → lavra `cartorio.py:1822` → `portas/celula.py:682` U. Requer guards onboarding e julgamento que efetivamente lavra. |

**Regravação, separada de primeira aquisição:** no `decidir`, K após U `ausencia.py:819` pode travar C/Chamado antes do segundo U `:911`; a própria Ausencia já estava adquirida. Também `_reabrir` pode fazer saves sucessivos `ausencia.py:1124/:1125/:1132`, cada um com K, mas isso não prova primeira aquisição inversa.

**Empresa — fechamento das entradas encontradas:**

- `atualizar_empresa` efetua `empresa.save()` em `app/colaboradores/services/estrutura.py:20`; normalização efetua queryset update `:27`.
- Formulário chama atualização em `app/colaboradores/views.py:521`, sem atomic próprio abrangente na função `:485`.
- `diff_janela_he` tem atomic `app/ponto/management/commands/diff_janela_he.py:74`, E U `:101/:103`, depois `_tot` `:102/:104` → leitura `autoridade_do_periodo` `:59`. Não demonstra C/A U/L.
- `diff_janela_he_total.py:106` atualiza E pela mutação executada dentro de `app/ponto/services/diff_frota.py:72` atomic, callback `:86`; cálculo posterior recebe `somente_leitura=True` `:87/:88`. Não demonstra C/A U/L.
- `aplicar_janela_he_total.py:119` atualiza E, sem atomic abrangente próprio; delegação de recálculo começa em `:137`. Não uni as transações.
- `EmpresaAdmin` herda `ModelAdmin` padrão em `app/colaboradores/admin.py:7/:8`; implementação SQL/atomic/cascatas do Django **[nao verificado]**. `Colaborador.empresa` usa PROTECT em `app/colaboradores/models.py:321`.
- Admin de Colaborador `app/colaboradores/admin.py:48` e de Ausencia `app/ponto/admin.py:38` usam SoLeitura; delete negado por `app/core/admin.py:65/:66`.

**Caminho excluído da prova de mesma transação:** `responder_pergunta_disputa_core` chama `_ressuscitar_por_resposta` em `app/chamados/services/disputa_emissao.py:1326` → Chamado U `app/chamados/services/arquivo.py:208`, depois materializa Ausencia em `disputa_emissao.py:1330` → `materializacao.py:519`. Não há atomic no core definido em `disputa_emissao.py:1247`; as chamadas web/API `app/chamados/views.py:1283` e `app/api/views.py:2643` não forneceram atomic abrangente. Os wrappers transacionais `declaracao_texto.py:302/:605` aceitam/produzem HH:MM (`:276`, `:368`, `:641`), não os escapes de ausência. Atomic externo efetivo desse caminho **[nao verificado]**.

_2011 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
