# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 15:11** · publicado em **27/11/2026 15:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 15:11 · sessao 05e8003882ed · INVENTARIO-PELA-REGRA · construtor · origem=Code

Corrigi as cinco medições nos três módulos da bateria. A sintaxe dos módulos foi verificada. O recorte Django e os mutantes na pista do Code ficam [nao verificado].

### Diff por caso

- **ESC-11** cobra 630 min trabalhados e 150 min de extra total, com 120 em HE50 e 30 em HE100 de dia normal. O excedente é `HE total − teto = 30`, conforme o corte HE100-RUBRICA. [test_inventario_escala.py:327](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:327)
- **AUS-05** deixa o modelo derivar `instancia_marco` para `batida_ausente`. A pergunta segue aberta, com 480 min previstos, furo apurado e zero falta. [test_inventario_ausencia.py:103](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:103)
- **FOL-03** mapeia e homologa `9902` em `folga_trabalhada_diurna`. Confere os 240 min no fechamento antes de comparar o delta do TXT, que continua exigindo zero em HE50 e HE100 de feriado. [test_inventario_folha.py:56](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:56) · [test_inventario_folha.py:164](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:164)
- **FOL-13** usa 60 min de HE50 autorizada com rubrica homologada. Faz dois POSTs idênticos, exige dois downloads 200, confere a presença dos 60 min e compara bytes e totais por rubrica. [test_inventario_folha.py:53](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:53) · [test_inventario_folha.py:309](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:309)

**ESC-12, AUS-03 e FOL-14** estão idênticos à base. [test_inventario_escala.py:335](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:335) · [test_inventario_ausencia.py:61](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:61) · [test_inventario_folha.py:330](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:330)

### Recorte para o Code

Da raiz de `app`, na pista de teste do Code. O settings `ci` lê `DB_HOST` e `REGUA_DB` da pista. [config/settings/ci.py:6](/home/ronald/wt-codex-inventario-pela-regra-143921/app/config/settings/ci.py:6) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci --noinput \
  ponto.tests.test_inventario_escala \
  ponto.tests.test_inventario_ausencia \
  folha.tests.test_inventario_folha
```

### RED e mutantes

No RED isolado de **ESC-11**, as asserções aceitaram `120/30`. Transferir os 30 min para HE50 derrubou as duas asserções por faixa. Calcular `HE50 − teto` derrubou a do excedente com `0 != 30`. A execução integrada desses mutantes e dos demais fica [nao verificado]. [test_inventario_escala.py:328](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:328)

Os comandos abaixo aplicam uma substituição em memória por processo e rodam o caso correspondente. [ESC-11:327](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:327) · [AUS-05:103](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:103) · [FOL-03:56](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:56) · [FOL-13:309](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:309)

```bash
mutante() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - "$@" <<'PY'
import importlib
import sys
from pathlib import Path

import django
from django.core.management import execute_from_command_line

rotulo, antes, depois = sys.argv[1:]
django.setup()
modulo = importlib.import_module(rotulo.rsplit('.', 2)[0])
arquivo = Path(modulo.__file__)
fonte = arquivo.read_text(encoding='utf-8')
assert fonte.count(antes) == 1, (arquivo, antes)
exec(compile(fonte.replace(antes, depois, 1), str(arquivo), 'exec'), vars(modulo))
execute_from_command_line(['manage.py', 'test', '--noinput', rotulo])
PY
}

esc11=ponto.tests.test_inventario_escala.InventarioEscalaTest.test_ESC_11_extra_integral_acima_de_duas_horas
aus05=ponto.tests.test_inventario_ausencia.InventarioAusenciaTest.test_AUS_05_uma_batida_com_pergunta_em_aberto
fol03=folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_03_dsr_trabalhado_em_rubrica_propria
fol13=folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_13_gerar_duas_vezes_mesmos_bytes_e_totais

# M1 — ESC-11: deslocar a HE100 para HE50, preservando o total.
mutante "$esc11" '        # HE100-RUBRICA' \
  $'        for periodo in resultado.periodos:\n            periodo.minutos_extra_50 += periodo.minutos_extra_100\n            periodo.minutos_extra_100 = 0\n        # HE100-RUBRICA'

# M2 — ESC-11: medir o excedente só pela HE50.
mutante "$esc11" 'extra_total - teto' 'resultado.horas_extra_50 * 60 - teto'

# M3 — AUS-05: forçar "fim" na pergunta genérica.
mutante "$aus05" $'            data_evento=data,\n        )' \
  $'            data_evento=data, instancia_marco=\'fim\',\n        )'

# M4 — FOL-03: voltar a mapear 9902 para o evento antigo.
mutante "$fol03" "'folga_trabalhada_diurna': DSR_TRABALHADO" \
  "'dsr_trabalhado_diurno': DSR_TRABALHADO"

# M5 — FOL-13: retirar da massa o evento exportável.
mutante "$fol13" \
  $'colab = self.massa_comercial({data: (\'08:00\', \'12:00\', \'13:00\', \'18:00\')})\n        self.autorizar(colab, data, 60)\n        self.fechar(colab, 11)' \
  $'colab = self.massa_comercial({data: COMERCIAL})\n        self.fechar(colab, 11)'

# M6 — FOL-13: alterar os bytes do segundo download após conferir o 200.
mutante "$fol13" '        self.assertEqual(primeiro.content, segundo.content)' \
  $'        segundo.content += b\'\\n\'\n        self.assertEqual(primeiro.content, segundo.content)'
```

## 27/11 15:10 · sessao 2e975728d169 · - · - · origem=Ronald (tmux)

Auditei os 40 itens de `RETRATO-DOIS-RELOGIOS` a `O320` e mais 22 de `EMPRESA-ID-ISDIGIT` ao fim do anexo. Enviei as linhas e os ajustes de consolidação ao agente principal. A cópia já tem o caminho da saída tardia de `O232` e a correção de pontualidade de `O73b`. [app/escala/utils.py:1999] [app/escala/tests/test_saida_valida_cross_meia_noite.py:80] [app/ponto/motor_calculo_v2.py:2822]

Estado de produção e identificação Git da cópia [nao verificado].

## 27/11 15:09 · sessao f553e73e9590 · - · - · origem=Ronald (tmux)

O HEAD de `/home/ronald/saas-hasner` é `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Os arquivos A/B e os dois envelopes na cópia de leitura coincidem byte a byte com os blobs do commit. A resolução do hash pelo wrapper está em `bin/codex_pergunta.sh:116`.

**A.** Para cada `Empresa.objects.get(pk=eid)`, chame `folha.porta_export.medir(empresa, 11, 2026)`. A janela vem de `janela_fechamento(11, 2026, empresa)`, cujo resultado com corte 21 é `21/10/2026–20/11/2026`. O marco pode ser lido com `marco_da_competencia(eid, 11, 2026)`. `app/folha/porta_export.py:319`, `app/folha/porta_export.py:343`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:54`, `app/ponto/services/fechamento.py:1577`.

| Par | Função e resultado |
|---|---|
| Tela × PDF | `medir` compara `espelho_do_colab(c, hoje, ini, fim, agora=instante)` com `_coletar_dados_espelho_mes(c, 11, 2026, agora=instante)`. Chave `tela_x_pdf`. `app/folha/porta_export.py:482`, `app/folha/porta_export.py:502`, `app/folha/porta_export.py:699`. |
| Cartão × TXT | `medir` chama `cartao_x_txt_divergentes`. O comando mede todas as empresas ativas e chama `relatorios.cartao_pela_celula.divergencia(colab, 11, 2026, resumo)`. Para contagem por empresa, rode o comando com `ids=True` e `agora` ISO fixo, capture a saída em `StringIO` e associe os IDs `colN` à empresa pelo ORM. `app/folha/porta_export.py:670`, `app/folha/porta_export.py:732`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:20`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:68`, `app/relatorios/cartao_pela_celula.py:216`. |
| Espelho × DiaPago | `dias_fora_do_dia_pago(colab, 11, 2026, ini, fim, versao='motor', dp_por_dia=None)`, chamada por `medir`; chaves `espelho_x_dia_pago` e `espelho_x_dia_pago_nao_medido`. `app/folha/porta_export.py:26`, `app/folha/porta_export.py:603`, `app/folha/porta_export.py:703`. |
| Fechamento × soma do DiaPago | `_medir_dia_pago(c, fech, badge=None)`, interna a `medir`, usa `soma_da_competencia(c, 11, 2026, versao='motor')`. Chaves `fechamento_x_soma_dias_pagos` e `colabs_sem_lavratura`. `app/folha/porta_export.py:384`, `app/folha/porta_export.py:457`, `app/folha/porta_export.py:664`, `app/ponto/services/dia_pago.py:550`. |
| Topo × soma das linhas | `medir` compara `total_relogio` com a soma de `minutos_realizados` dos dias dentro da janela, chave `topo_x_coluna`. `app/folha/porta_export.py:515`, `app/folha/porta_export.py:699`. |
| App × tela | `AppLeATelaTest.test_MORDE_o_endpoint_nao_remonta_a_palavra_do_dia()` e `test_MORDE_o_endpoint_LE_o_espelho()` verificam a AST de `api_espelho_v2` para todos os recortes. `app/api/tests/test_r4_par6_app_le_a_tela.py:53`, `app/api/views.py:1361`. |
| Linhas × badge | `linha_x_badge(colab, 11, 2026, badge, soma=None)` usa o `resumo` do espelho e a soma das linhas. Chaves `linha_x_badge_divergentes`, `linha_x_badge_na_exportada` e `linha_x_badge_nao_medido`. `app/folha/porta_export.py:258`, `app/folha/porta_export.py:449`, `app/folha/porta_export.py:717`. |

`calendario_x_espelho` é uma chave de compatibilidade vazia, pois a pergunta foi incorporada a `espelho_x_dia_pago`. O selo CLI chama `vigiar_abertos`, `assumir` e `concluir`. `concluir` chama `lavrar`, que persiste `MetricaSnapshot` em `update_or_create`. A sonda usa apenas `medir` e os leitores. `app/folha/porta_export.py:608`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:76`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:197`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:219`, `app/folha/porta_export.py:785`, `app/folha/porta_export.py:1279`.

**B.** R5 so com escrita: fica fora (proibido medida nova). O script está fixado na comp 10. Executa duas passadas de `processar_cartorio(apply=True)` e `recalcular_fechamento_mes()`, com critério `foto3 == foto2`. O cartório carimba `CelulaDia` e pode emitir chamados. O recálculo grava `FechamentoMensal`. O dry do cartório omite `--apply` e lê a fila histórica `data < hoje`. O recálculo dispõe de `somente_leitura=True, lavrar_dias_pagos=False`. Os dois modos de leitura preservam o estado da primeira passagem e, por isso, não verificam o critério do R5. `bin/r5_idempotencia_frota.sh:24`, `bin/r5_idempotencia_frota.py:26`, `bin/r5_idempotencia_frota.py:104`, `bin/r5_idempotencia_frota.py:124`, `app/ponto/management/commands/processar_cartorio.py:25`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1883`, `app/ponto/services/cartorio.py:1905`, `app/ponto/services/fechamento.py:468`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1215`.

**F.** O wrapper chama um executável por `CODEX_SONDA_ENVELOPE`, com o caminho da sonda como único argumento. O executável existente `/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3` usa `exec` com `--sombra` e preserva o RC. `SONDA_ARVORE=<cópia conferida>/app` fixa os blobs montados. O Python deve capturar somente a exceção sentinela de rollback, fora do `atomic()`, pois RC diferente de zero faz o wrapper rejeitar a saída. O envelope faz escritas administrativas de role/GRANT e de arquivos durante a preparação. Eu apenas li o código. `bin/codex_pergunta.sh:627`, `bin/sonda_leitura.sh:66`, `bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:140`, `bin/sonda_leitura.sh:157`, `bin/sonda_leitura.sh:232`, `bin/sonda_leitura.sh:239`.

## 27/11 15:09 · sessao 3656640e4966 · - · - · origem=Ronald (tmux)

- **Becos de disputa.** O contador usa `chamados.services.becos.classificar(disputa)` e retorna `FECHAVEL`, `BARRADA` ou `None`. O comando chama `get_user_sistema()` mesmo em dry ou `--alarme`, e esse helper usa `get_or_create`. A sonda deve usar o classificador diretamente. O `--alarme` do cron sai com RC 2 para qualquer `FECHAVEL`. `--apply` chama a porta de escrita `fechar_beco`. [app/chamados/services/becos.py:25](app/chamados/services/becos.py#L25), [app/chamados/management/commands/fechar_becos_disputa.py:27](app/chamados/management/commands/fechar_becos_disputa.py#L27), [app/core/utils.py:18](app/core/utils.py#L18), [app/config/crons.py:663](app/config/crons.py#L663)
- **Órfãs da lâmpada.** `chamados.reconciliador.orfas_pela_lampada_sem_evento(agora=REF, colaborador_ids=ids)` devolve os IDs completos. O recorte da competência pode filtrar esses IDs por `PerguntaDisputa.data_evento`. O comando sem argumentos de alarme conta em dry e `--alarme` sai com RC 2 se houver achados. `perguntas_orfas_reatavies(colab)` mede separadamente o universo elegível e pode receber o filtro de data no queryset. [app/chamados/reconciliador.py:1325](app/chamados/reconciliador.py#L1325), [app/ponto/management/commands/reconciliar_perguntas_orfas.py:24](app/ponto/management/commands/reconciliar_perguntas_orfas.py#L24), [app/chamados/reconciliador.py:1640](app/chamados/reconciliador.py#L1640)
- **Fio sem caminho.** `censo_alcance(empresas=[empresa], hoje=HOJE, agora=REF, limite=0)` usa a tarefa publicada e retorna todos os `sem_porta` mesmo com limite zero, com competência, causa e detalhe em cada linha. A sonda pode contar os `sem_porta` da `11/2026` por causa e detalhe. O vigia mede empresas visíveis em operação e competências que `competencias_a_lavrar` seleciona. O comando de frota grava carimbo em disco. `contador_fio_mudo(hoje=HOJE, agora=REF, ini=INI, fim=FIM, empresas=[empresa.pk])` mede os motivos devidos sem pergunta viva. `motivos_que_o_evento_deixou(cel, HOJE, REF, memo=memo)` mede o que o julgamento da célula deixou de emitir. [app/chamados/services/vigia_fio.py:524](app/chamados/services/vigia_fio.py#L524), [app/chamados/services/vigia_fio.py:622](app/chamados/services/vigia_fio.py#L622), [app/chamados/management/commands/vigiar_fio_morto.py:131](app/chamados/management/commands/vigiar_fio_morto.py#L131), [app/chamados/management/commands/disparar_perguntas_competencia.py:77](app/chamados/management/commands/disparar_perguntas_competencia.py#L77), [app/chamados/services/fabrica_da_celula.py:234](app/chamados/services/fabrica_da_celula.py#L234)
- **E6.** O comando `e6_oraculo --mes 11 --ano 2026 --hoje 2026-11-27`, sem CSV, certifica o espelho da frota e exige `FechamentoMensal` para selecionar colaboradores. O comando captura falha do espelho e segue. Para apor a classe a um dia já achado pelo vigia, existe precedente em `RELATO`, com `InsumosDoDono(colab, INI, FIM, HOJE, agora=REF).dono(dia)` como leitor por colaborador. [app/ponto/management/commands/e6_oraculo.py:128](app/ponto/management/commands/e6_oraculo.py#L128), [app/ponto/management/commands/e6_oraculo.py:182](app/ponto/management/commands/e6_oraculo.py#L182), [app/ponto/management/commands/e6_oraculo.py:276](app/ponto/management/commands/e6_oraculo.py#L276), [app/docs/RELATO.md:5549](app/docs/RELATO.md#L5549)
- **RC de órfãs de obra.** `bin/rc_sem_colher.py::sem_colher(raiz, relato_txt, agora=None, min_idade=0)` lê logs de obras das últimas 24 h. RC 0 indica lista vazia, RC 3 indica achados e RC 1 indica falha de leitura. Sua unidade é obra e arquivo `.rc`. [bin/rc_sem_colher.py:2](bin/rc_sem_colher.py#L2), [bin/rc_sem_colher.py:50](bin/rc_sem_colher.py#L50)

O envelope padrão de `codex_pergunta.sh --sonda` chama `sonda_leitura.sh` sem `--sombra`. Há um `CODEX_SONDA_ENVELOPE` de sombra documentado no `RELATO`. A saída só segue se o envelope retorna zero, logo a sonda precisa suprimir exclusivamente a exceção deliberada de rollback **fora** do `atomic`. `SET TRANSACTION READ ONLY` antes de consultar e a pré-carga `tranca_em_lote(sistema_pk=User.objects.values_list('pk', flat=True).get(username='sistema'))` evitam o `get_or_create` nas leituras da tranca. [bin/codex_pergunta.sh:623](bin/codex_pergunta.sh#L623), [bin/sonda_leitura.sh:93](bin/sonda_leitura.sh#L93), [app/docs/RELATO.md:2735](app/docs/RELATO.md#L2735), [app/chamados/juizes.py:1254](app/chamados/juizes.py#L1254), [app/chamados/juizes.py:1321](app/chamados/juizes.py#L1321)

## 27/11 15:08 · sessao 83229a591dba · - · - · origem=Ronald (tmux)

Auditei as 25 obras do bloco inicial e as cinco verificações adicionais. Enviei ao agente principal as classificações, os sítios de código, os contratos e as ressalvas de acervo. A confirmação do hash e a situação atual em produção ficam [nao verificado].

## 27/11 15:08 · sessao 9e7bed9a78ac · INVENTARIO-PELA-REGRA · leitura · origem=Code

# INVENTÁRIO-PELA-REGRA

Gabarito de **12 casos CCT e 5 casos intermitentes**, pela L-110. Execução contra o HEAD e placar de vermelhos [nao verificado]. `app/docs/LEIS.md:136`

Os cadastros, vínculos, convocações e batidas abaixo são dados de teste [nao verificado]. As batidas estão em UTC−3. Nos plantões que cruzam a meia-noite, os minutos pertencem ao dia de início. `app/docs/LEIS.md:107` · `app/docs/LEIS.md:117`

A extração inicial exibiu comentários técnicos junto dos jurídicos em `app/core/regua_cct.py:56`. As referências dos casos apontam para as regras, cláusulas e pareceres usados no gabarito. O corte de 17/11 sobre intermitentes e seu texto em `logs/AVAIS-17*` ficaram [nao verificado]. Os casos INT usam a CLT e as leis da casa citadas em cada linha.

## CCT dos Vigilantes de Londrina

`Vigilância A` é a empresa fictícia. Em todos os casos CCT, a linha de `AplicacaoConvencao` está ativa e é única no nível empresa + praça, para o Sindicato nº 2. O vínculo 12x36 está vigente desde 01/11/2026. A convenção PR000251/2026 tem vigência documentada de 01/02/2026 a 31/01/2028. `CLAUDE.md:214` · `CLAUDE.md:218` · `app/core/regua_cct.py:234`

| ID | Família | Escala | CADASTRO | BATIDAS (UTC−3) | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| CCT-01 | CCT · `regua_excedente` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026 | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | `lei: cláusula ou corte que defina se a régua do escrito e a 9ª diária se aplicam ao plantão regular 12x36 e quais minutos entram como HE nesse ciclo.` | `SERIE_T.md:1072` · parecer e decisão de 29–30/07, `app/core/regua_cct.py:376` e `app/core/regua_cct.py:378` |
| CCT-02 | CCT · `prorrogacao_noturna_pos5h` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Leitura restritiva registrada no cadastro | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | `lei: parecer ou corte que confirme se a cl. 10 exclui os minutos trabalhados entre 05h e 07h. O texto referido fecha a janela 22h–05h e é silente sobre a prorrogação.` | L-035, `app/docs/LEIS.md:83` · cl. 10, `CLAUDE.md:236` · registro da interpretação, `SERIE_T.md:1326` |
| CCT-03 | CCT · `hora_reduzida_afastada_12x36` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Janela 22h–05h fixada no caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Noturno 0025 **420 min de base**, com conversão **1:1** entre relógio e folha, a 20%. | Cl. 38-d, `app/core/regua_cct.py:129` e `app/core/regua_cct.py:136` · L-036, `app/docs/LEIS.md:84` · `SERIE_T.md:1074` |
| CCT-04 | CCT · `adicional_noturno_pct` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Janela 22h–05h fixada no caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Rubrica 0025 **420 min de base a 20%**, com adicional devido além do piso salarial. | Cl. 3 e 38-d, `app/core/regua_cct.py:136` e `app/core/regua_cct.py:146` · `SERIE_T.md:1075` |
| CCT-05 | CCT · `tolerancia_marcacao_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 | 18/11 E 19:06, S 20:30, E 21:30. 19/11 S 07:00 | `lei: cláusula que fixe a tolerância CCT por marcação, ou corte que mande usar o piso legal neste cadastro. A entrada isolada tem desvio de 6 min.` | `SERIE_T.md:1076` · corte sobre o piso da CLT, art. 58 § 1º, `app/docs/CORTES.md:521` |
| CCT-06 | CCT · `tolerancia_dia_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 | 18/11 E 19:04, S 20:30, E 21:34. 19/11 S 06:56 | `lei: cláusula que fixe o teto diário CCT, ou corte que mande usar o piso legal neste cadastro. Os três desvios de 4 min somam 12 min no mesmo plantão.` | `SERIE_T.md:1077` · corte sobre o piso da CLT, art. 58 § 1º, `app/docs/CORTES.md:521` · L-085, `app/docs/LEIS.md:117` |
| CCT-07 | CCT · `intrajornada_minima_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 e `intervalo_indenizavel=True` | 18/11 E 19:00, S 20:30, E 21:00. 19/11 S 07:00 | `lei: cláusula que fixe o mínimo convencional aplicável. Com 30 min de pausa gozada, esse mínimo decide quantos minutos suprimidos entram na rubrica de intrajornada indenizada.` | `SERIE_T.md:1078` · L-031, `app/docs/LEIS.md:79` · [CLT, art. 71, del5452compilado.htm:373 e :379](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art71) |
| CCT-08 | CCT · `compensacao_autorizada` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantões 18 e 26/11/2026. Prorrogação autorizada e compensação solicitada no segundo plantão | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:30. 26/11 E 19:00, S 20:30, E 21:30. 27/11 S 06:30 | `lei: cláusula que autorize compensar a prorrogação no 12x36 e defina se os 30 min podem ser quitados pela redução do segundo plantão.` | `SERIE_T.md:1079` · [CLT, art. 59 §§ 2º, 5º e 6º, del5452compilado.htm:326, :330 e :332](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-09 | CCT · `compensacao_janela` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantões 18/11/2026 e 18/01/2027. Compensação admitida como premissa para testar só o prazo | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:30. 18/01 E 19:00, S 20:30, E 21:30. 19/01 S 06:30 | `lei: cláusula que fixe a janela, o marco inicial e o vencimento do saldo. Isso decide se a redução em janeiro quita os 30 min de novembro ou se o saldo já virou HE a pagar.` | `SERIE_T.md:1080` · [CLT, art. 59 §§ 2º, 5º e 6º, del5452compilado.htm:326, :330 e :332](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-10 | CCT · `teto_prorrogacao_dia_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Prorrogação de 90 min determinada e confirmada pelo empregador | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 08:30 | `lei: cláusula que fixe o teto diário convencional no 12x36 e diga em que rubrica pagar o trabalho efetivo acima do teto, se houver.` | `SERIE_T.md:1081` · L-097, `app/docs/LEIS.md:115` · [CLT, arts. 59 e 59-A, del5452compilado.htm:323 e :333](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-11 | CCT · cl. 10, janela estrita | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Sem prorrogação após 05h por premissa expressa do caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Noturno 0025 **420 min de base a 20%**, sendo 120 min em 18/11 e 300 min em 19/11. De 05h a 07h, **0 min de adicional noturno**. O plantão é atribuído a 18/11. | Cl. 10, `CLAUDE.md:236` · cl. 38-d, `app/core/regua_cct.py:136` · L-085, `app/docs/LEIS.md:117` |
| CCT-12 | CCT · cl. 38-d, hora de 60 min | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa 01h–02h e janela 22h–05h fixada no caso | 18/11 E 19:00. 19/11 S 01:00, E 02:00, S 07:00 | Normais **660 min**. Noturno 0025 **360 min de base a 20%**. Dos 420 min da janela, 60 min são pausa. Cada 60 min de trabalho noturno gera 60 min de base. | Cl. 38-d e parecer de 01/09, `app/core/regua_cct.py:129`, `app/core/regua_cct.py:136` e `app/core/regua_cct.py:141` · L-036, `app/docs/LEIS.md:84` |

## Intermitente

`Serviços B` é a empresa fictícia. Os casos usam regime `clt`, praça Londrina, sem CCT ou `AplicacaoConvencao` aplicável, vínculo intermitente escrito desde 01/11/2026 e salário-hora pactuado conforme o piso aplicável. As convocações indicadas foram recebidas e aceitas na data registrada, às 08h. [CLT, art. 452-A, del5452compilado.htm:2116, :2117 e :2119](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a)

| ID | Família | Escala | CADASTRO | BATIDAS (UTC−3) | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| INT-01 | Intermitente · convocação aceita e trabalhada | Personalizado sem intervalo, 08h–12h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada aceita 19/11 para 23/11/2026, 08h–12h | 23/11 E 08:00, S 12:00 | Normais **240 min**, HE **0 min**, falta **0 min**. Ao fim da prestação, recibo com remuneração, férias proporcionais + 1/3, 13º proporcional e DSR discriminados. | [CLT, arts. 71 § 1º e 452-A §§ 1º, 2º, 6º e 7º, del5452compilado.htm:375, :2117, :2119, :2125 e :2135](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-02 | Intermitente · dia sem convocação | Personalizado sem intervalo, sem jornada chamada no dia | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / 24/11/2026 sem convocação | 24/11 sem batidas | Dia de inatividade. Previsto **0 min**, normais **0 min**, HE **0 min**, falta **0 min**. Sem desconto de DSR por falta nesse dia. | L-088, `app/docs/LEIS.md:120` · [CLT, art. 452-A § 5º, del5452compilado.htm:2123](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-03 | Intermitente · convocação em feriado | Personalizado sem intervalo, 09h–13h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada avulsa aceita 17/11 para o feriado de teste de 20/11/2026, sem plantão regular na escala e sem folga compensatória | 20/11 E 09:00, S 13:00 | Feriado em dobro **240 min**, pagos a **2×** o valor da hora normal. DSR em parcela própria. | [Lei 605/49, art. 9º, l0605.htm:67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art9) · [Súmula 146, TST, IRR-308, p. 4](https://www.tst.jus.br/documents/d/guest/irr-308-_acordao-pdf) · L-086, `app/docs/LEIS.md:118` |
| INT-04 | Intermitente · HE acima do convocado | Personalizado sem intervalo, 08h–11h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada aceita 19/11 para 25/11/2026, 08h–11h. HE até 12h acordada e autorizada com registro | 25/11 E 08:00, S 12:00 | Normais **180 min**, HE50 **60 min**, falta **0 min**. | L-097, `app/docs/LEIS.md:115` · [CLT, arts. 59 § 1º e 452-A § 6º I e V, del5452compilado.htm:325, :2127 e :2134](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-05 | Intermitente · DSR proporcional | Personalizado sem intervalo, 08h–11h em dois dias | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / uma chamada aceita 19/11 para 23 e 24/11/2026. Ambos os dias cumpridos integralmente | 23/11 E 08:00, S 11:00. 24/11 E 08:00, S 11:00 | `lei: fórmula do DSR proporcional para uma convocação de dois dias, com base, divisor e arredondamento em minutos. O DSR é devido ao fim da prestação e vem discriminado no recibo.` | [CLT, art. 452-A §§ 6º IV e 7º, del5452compilado.htm:2125, :2132 e :2135](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) · [Lei 605/49, art. 7º b, l0605.htm:55](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art7) |

## Perguntas de lei

1. **CCT-01.** Qual cláusula ou corte define a incidência da régua do escrito e da 9ª diária sobre o 12x36 regular? `app/core/regua_cct.py:378`
2. **CCT-02.** Qual parecer ou corte confirma o tratamento de 05h–07h pela cl. 10? `CLAUDE.md:236` · `SERIE_T.md:1326`
3. **CCT-05 e CCT-06.** Quais são as tolerâncias convencionais por marcação e por plantão? Na ausência de cláusula própria, vale neste cadastro o par legal de 5 min por marcação e 10 min por dia? `SERIE_T.md:1076` · `SERIE_T.md:1077` · `app/docs/CORTES.md:521`
4. **CCT-07.** Qual mínimo de intrajornada da CCT rege a indenização do intervalo parcialmente gozado no 12x36? `SERIE_T.md:1078` · `app/docs/LEIS.md:79`
5. **CCT-08 e CCT-09.** Qual cláusula autoriza compensar prorrogação no 12x36 e qual é o prazo para quitar o saldo? `SERIE_T.md:1079` · `SERIE_T.md:1080`
6. **CCT-10.** Qual teto convencional rege a prorrogação diária no 12x36 e como se classifica, para pagamento, o trabalho efetivo acima dele? `SERIE_T.md:1081` · `app/docs/LEIS.md:115`
7. **INT-05.** Qual é a base, o divisor e o arredondamento do DSR proporcional em minutos por período de prestação? [CLT, art. 452-A § 6º IV, del5452compilado.htm:2132](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) · [Lei 605/49, art. 7º b, l0605.htm:55](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art7)
8. **INT-01 a INT-05.** Qual é o texto do corte de 17/11 sobre intermitentes e quais destes casos ele alcança? [nao verificado]

## 27/11 15:08 · sessao b24bb17059ed · - · - · origem=Ronald (tmux)

Auditei as 54 obras entre `FERIADO-ESCALA` e `O-JANELA-HE` e encaminhei as linhas ao agente principal, com sítios de código, estado no recorte e contrato aplicável. [GRADE-MESTRA.md](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/GRADE-MESTRA.md:140)

No agregado, `GRADE-FOLGA-C5` e `GRADE-FOLGA-GERADOR` compartilham a leitura de folga fora do juiz. `PROPOSITOR-JANELA-LONGA` e `FILA-POR-CAUSA-AS-OF` compartilham o uso da última lavra onde a consulta precisa da decisão por dia. `TROCA-DE-POSTO-SEM-VINCULO` e o item 01 de `BO2910-ESTRUTURA` compartilham a troca de posto sem uma porta única para o ato. `app/ponto/precedencia.py:212`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1561`, `app/escala/models.py:1321`, `app/escala/services/propositor.py:493`, `app/chamados/services/fila_por_causa.py:184`, `app/colaboradores/views.py:337` e `app/colaboradores/views.py:1415`.

## 27/11 15:08 · sessao 903d81c0f68d · INVENTARIO-PELA-REGRA · leitura · origem=Code

# INVENTARIO-PELA-REGRA

As competências usadas são **11/2026** de 21/10 a 20/11, **12/2026** de 21/11 a 20/12, **01/2027** de 21/12 a 20/01 e **02/2027** de 21/01 a 20/02. Os 15 casos são gabaritos de leitura, com asserções limitadas aos dias informados. A execução na pista e o HEAD estão [nao verificado]. [app/docs/LEIS.md:77](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:77) · [app/docs/LEIS.md:136](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:136)

**Dados comuns.** Cadastro e batidas são entradas sintéticas, independentes por caso. Onde constar 6×1 comercial, usei 08:00–17:00 com pausa 12:00–13:00 de segunda a sexta, sábado 08:00–12:00 e DSR domingo. Usei essa escala nos recortes anteriores à transição de dezembro da EC 141/2026. Nos plantões de dezembro e janeiro, o acordo coletivo de jornada e repouso regular é dado do teste. Os feriados locais são entradas de cadastro da praça Londrina/PR, cuja situação real está [nao verificado]. [app/docs/LEIS.md:64](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:64) · [EC 141/2026, arts. 6º e 10, linhas 34–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)

**CMP-01**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo, mensalista. Feriado nacional de 20/11/2026 cadastrado. Sexta seria dia de trabalho e o posto dispensa o expediente no feriado.  
**BATIDAS (UTC−3)** 19/11 08:00 E, 12:00 S, 13:00 E, 17:00 S. 20/11 sem batidas. 21/11 08:00 E, 12:00 S.  
**RESPOSTA DA REGRA** 20/11 é feriado nacional e fecha a comp. **11/2026**. No dia, 0 min de presença exigida, 0 trabalhados, 0 de HE de feriado e 0 de falta. Remuneração do feriado preservada. O sábado 21/11 tem 240 min normais na comp. **12/2026**.  
**FONTE** `app/docs/LEIS.md:77` · [Lei 14.759/2023, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14759.htm) · [Lei 605/1949, art. 8º, linha 64](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-02**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Cadastro dado de feriado municipal em 10/12/2026, praça Londrina/PR. Trabalho autorizado em dia ordinariamente útil, sem folga compensatória. DSR em 13/12.  
**BATIDAS (UTC−3)** 10/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 10/12, 480 min trabalhados na base da rubrica **feriado em dobro**, remunerada a 200%. Em 11/12, 480 min normais. Em 12/12, 240 min normais. DSR de 13/12 preservado.  
**FONTE** `app/docs/LEIS.md:64` · `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [Lei 605/1949, art. 9º, linha 67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [Súmula 146 do TST, linhas 1498–1502](https://www.tst.jus.br/documents/10157/cc23ec94-9e08-47e3-8ed6-2e662cee7690)

**CMP-03**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Mesmo feriado municipal dado de 10/12/2026. Trabalho autorizado, com folga compensatória integral formalizada e gozada em 11/12. DSR em 13/12.  
**BATIDAS (UTC−3)** 10/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11/12 sem batidas. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 10/12, 480 min trabalhados e 0 min de dobra de feriado. Em 11/12, folga compensatória com 0 min trabalhados e 0 de falta. Em 12/12, 240 min normais. DSR de 13/12 preservado.  
**FONTE** `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 6º e 9º, linhas 28 e 67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [Súmula 146 do TST, linhas 1498–1502](https://www.tst.jus.br/documents/10157/cc23ec94-9e08-47e3-8ed6-2e662cee7690)

**CMP-04**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo, mensalista. Cadastro dado de feriado estadual em 13/12/2026, aplicável à praça Londrina/PR. A data coincide com o DSR da escala. Semana cumprida.  
**BATIDAS (UTC−3)** 07, 08, 09, 10 e 11/12, cada dia 08:00 E, 12:00 S, 13:00 E, 17:00 S. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 13/12, 0 min previstos de presença, 0 trabalhados, 0 de HE de feriado e 0 de falta. Um DSR preservado no salário mensal, com 0 min de dobra pelo encontro das duas datas.  
**FONTE** `app/docs/LEIS.md:64` · `app/docs/LEIS.md:74` · `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 1º e 6º–9º, linhas 15–67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-05**  
**Família** escala  
**Escala** 12×36 diurno, 07:00–19:00  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Feriado nacional de 25/12/2026 cadastrado. Foto escala 25/12 como plantão. Acordo coletivo válido e sem cláusula de dobra. Pausa 12:00–13:00 e limites de jornada cumpridos.  
**BATIDAS (UTC−3)** 24/12 sem batidas. 25/12 07:00 E, 12:00 S, 13:00 E, 19:00 S. 26/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **01/2027**. Em 25/12, 660 min previstos e normais trabalhados, 0 de HE50, 0 de dobra de feriado, 0 de adicional noturno, 0 de intrajornada indenizada. DSR e feriado abrangidos pela remuneração mensal pactuada. A Resolução TST 225/2025 cancelou a Súmula 444 por perda de eficácia a partir de 11/11/2017.  
**FONTE** `app/docs/LEIS.md:73` · `app/docs/LEIS.md:77` · `app/docs/LEIS.md:118` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm) · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Resolução TST 225/2025, art. 1º, XXIV, p. 3](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/92286/Resolu%C3%A7%C3%A3o%20n.%20225%2C%20de%2030%20de%20junho%20de%202025.pdf?sequence=3#page=3)

**CMP-06**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Feriado nacional de 01/01/2027 cadastrado. Foto escala plantão iniciado em 01/01. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 31/12 sem batidas. 01/01 19:00 E, 21:00 S, 22:00 E. 02/01 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula vigente que conceda dobra ao 12×36 dos vigilantes e defina sua base`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%, com hora de 60 min. São 240 min trabalhados na data civil do feriado. Para atribuição da jornada, a L-085 usa 01/01, dia de início.  
**FONTE** `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:227` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm) · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm)

**CMP-07**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Natal de 25/12/2026 cadastrado. Foto escala plantão iniciado em 24/12. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 24/12 19:00 E, 21:00 S, 22:00 E. 25/12 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula de dobra e corte de incidência por data civil que resolva a divergência com a L-085`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%. O Natal toca fisicamente 25/12 das 00:00 às 07:00, 420 min, dos quais 300 dentro da janela noturna. A L-085 atribui o plantão a 24/12.  
**FONTE** `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

**CMP-08**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Feriado de 01/01/2027 cadastrado. Foto escala plantão iniciado em 31/12. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 31/12 19:00 E, 21:00 S, 22:00 E. 01/01 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula de dobra e corte de incidência por data civil que resolva a divergência com a L-085`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%. O Ano-Novo toca fisicamente 01/01 das 00:00 às 07:00, 420 min, dos quais 300 dentro da janela noturna. A L-085 atribui o plantão a 31/12. A virada do ano civil permanece na mesma competência.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

**CMP-09**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão e início de vínculo em 05/12/2026. Horista. DSR em 06/12. Sem feriado cadastrado no recorte.  
**BATIDAS (UTC−3)** 04/12 sem batidas. 05/12 08:00 E, 12:00 S. 06/12 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento do DSR proporcional da primeira semana do horista admitido no sábado`. Na comp. **12/2026**, antes de 05/12 são 0 min previstos, 0 de falta e 0 de DSR creditável. Em 05/12 são 240 min previstos e normais trabalhados. O primeiro DSR a apurar é 06/12, na mesma competência.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [Lei 605/1949, arts. 6º e 7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-10**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão e início de vínculo em 21/11/2026. Horista. Feriado nacional de 20/11 cadastrado. DSR em 22/11.  
**BATIDAS (UTC−3)** 20/11 sem batidas. 21/11 08:00 E, 12:00 S. 22/11 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento do DSR proporcional do horista admitido no primeiro dia da competência`. Em **11/2026**, 0 min previstos, trabalhados, de falta ou de DSR e nenhum crédito do feriado de 20/11 para este vínculo. Em **12/2026**, 240 min previstos e normais trabalhados em 21/11. O primeiro DSR a apurar é 22/11.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [Lei 14.759/2023, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14759.htm) · [Lei 605/1949, arts. 6º–8º, linhas 28–64](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-11**  
**Família** escala  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão e início de vínculo em 20/01/2027. Horista. Repousos remunerados em 23 e 24/01. Sem feriado cadastrado no recorte.  
**BATIDAS (UTC−3)** 19/01 sem batidas. 20, 21 e 22/01, cada dia 08:00 E, 12:00 S. 23 e 24/01 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento dos dois DSR proporcionais quando a primeira semana atravessa o corte`. São 0 min previstos e de falta antes da admissão, 240 min previstos e normais em **01/2027** no dia 20, e 480 min previstos e normais em **02/2027** nos dias 21 e 22. Os dois repousos a apurar caem em **02/2027**.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [CLT, art. 71, § 1º](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Lei 605/1949, arts. 6º–7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [EC 141/2026, arts. 1º e 10, linhas 9–26 e 49–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)

**CMP-12**  
**Família** folha/export  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 20/01/2027, último dia do vínculo. Sem feriado no recorte. Há pendência de célula que normalmente reteria o TXT. Os demais requisitos da emissão estão satisfeitos.  
**BATIDAS (UTC−3)** 18, 19 e 20/01, cada dia 08:00 E, 12:00 S. 21/01 sem batidas.  
**RESPOSTA DA REGRA** **01/2027** inclui o dia 20. São 720 min previstos e normais no recorte de 18 a 20, dos quais 240 no último dia. O movimento entra no TXT de **01/2027** e a pendência do desligado gera zero motivos de retenção. Em 21/01, já em **02/2027**, são 0 min previstos, 0 de falta e nenhum movimento para este vínculo.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-13**  
**Família** folha/export  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 21/12/2026, último dia do vínculo. Descansos em 19 e 20/12. Sem feriado no recorte. Demais requisitos de emissão satisfeitos.  
**BATIDAS (UTC−3)** 18/12 08:00 E, 12:00 S. 19 e 20/12 sem batidas. 21/12 08:00 E, 12:00 S. 22/12 sem batidas.  
**RESPOSTA DA REGRA** 240 min normais de 18/12 no TXT de **12/2026** e 240 min normais de 21/12 no TXT de **01/2027**. Zero motivos de retenção do desligado em ambas. A partir de 22/12, 0 min previstos e 0 de falta.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-14**  
**Família** folha/export  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 10/12/2026, último dia do vínculo. Sem feriado cadastrado neste recorte. Demais requisitos de emissão satisfeitos.  
**BATIDAS (UTC−3)** 07, 08, 09 e 10/12, cada dia 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11, 12 e 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. São 1.920 min previstos e normais no recorte de 07 a 10/12, dos quais 480 no último dia. O movimento entra no TXT, com zero motivos de retenção do desligado. Em 11, 12 e 13/12, 0 min previstos, 0 de falta e nenhum DSR por data posterior ao fim do vínculo.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517` · `app/docs/DOSSIES.md:135`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-15**  
**Família** ausência/férias  
**Escala** 6×1 comercial nos dias do recorte  
**CADASTRO** Admissão 01/10/2025, vínculo ativo, mensalista. Férias já concedidas de 15/11 a 14/12/2026, inclusive. Feriados nacionais de 15 e 20/11 cadastrados.  
**BATIDAS (UTC−3)** Cada dia de 15/11 a 14/12, sem batidas. 15/12 08:00 E, 12:00 S, 13:00 E, 17:00 S.  
**RESPOSTA DA REGRA** **11/2026** recebe 6 dias corridos de gozo, de 15 a 20/11. **12/2026** recebe 24, de 21/11 a 14/12. O dia 21/11 está em férias. Os DSR de 15, 22 e 29/11 e de 06 e 13/12 estão dentro do gozo. Durante o gozo são 0 min de trabalho, falta, HE ou DSR pago à parte. O retorno em 15/12 dá 480 min normais em **12/2026**. O terço mínimo incide sobre a remuneração dos 30 dias e é pago com ela até 13/11, data que cai em **11/2026**. `lei: competência e formato da rubrica de férias e terço no TXT`.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [CLT, arts. 130, 142 e 145](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Constituição, art. 7º, XVII, linha 295](https://www.planalto.gov.br/ccivil_03/constituicao/constituicao.htm) · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

## Perguntas de lei

1. **Dobra do 12×36, CMP-05 a CMP-08.** Qual cláusula ou corte vigente concede dobra ao plantão previsto? Para o cadastro sem cláusula favorável, o aval confirma o piso do art. 59-A? `app/docs/LEIS.md:118` · `app/core/regua_cct.py:473` · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Resolução TST 225/2025, art. 1º, XXIV, p. 3](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/92286/Resolu%C3%A7%C3%A3o%20n.%20225%2C%20de%2030%20de%20junho%20de%202025.pdf?sequence=3#page=3)
2. **Virada do feriado, CMP-06 a CMP-08.** Havendo dobra, qual corte autoriza calcular a rubrica pelas horas da data civil, inclusive a sobreposição com a 0025, diante da atribuição pelo início na L-085? `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `CLAUDE.md:236`
3. **DSR da primeira semana, CMP-09 a CMP-11.** Qual base, divisor e arredondamento convertem o DSR proporcional do horista em minutos? Como aplicar a fórmula aos dois repousos quando a semana cruza o dia 20/21? `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 6º–7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [EC 141/2026, arts. 1º e 10, linhas 9–26 e 49–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)
4. **Férias e terço no TXT, CMP-15.** Qual regra atribui o pagamento no TXT quando o gozo atravessa o corte? Qual unidade a rubrica deve carregar? `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [CLT, arts. 142 e 145](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm)

## 27/11 15:08 · sessao 9139767f560b · - · - · origem=Ronald (tmux)

Auditei as 50 obras de `PERGUNTA-VIVA-COM-CELULA-ABSOLVEU` a `F-346-FILA-OPUS` e enviei a tabela em três blocos para consolidação. Os endereços do anexo divergem da cópia em várias obras. Por exemplo, a primeira consta em `app/docs/BACKLOG.md:2451` e a última em `app/docs/BACKLOG.md:2508`. Estado de produção e confirmação do hash Git ficam como [nao verificado].

## 27/11 15:08 · sessao 39cd61c86174 · - · - · origem=Ronald (tmux)

**D.** A view chama `recalcular_fechamento_mes(11, 2026, empresa_id=e.pk, somente_leitura=True, retornar_objeto=True)`, usa os valores do dicionário como `fechamentos`, chama `classificar_export(e, 11, 2026, fechamentos=fechamentos)` e passa os mesmos objetos com `itens=itens` a `montar_linhas_export`. Em seguida chama `validar_layout_dominio(linhas)`. A montagem devolve `(linhas, entra_ids)` em memória. A aprovação e o registro do TXT ficam na view. [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:258](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:258) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282) [app/folha/views.py:304](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:304) [app/folha/export.py:699](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:699)

Para atribuir os erros, monte novamente com `[fechamento]` e `[item]` na ordem original e use `fechamento.colaborador_id`. Preserve cada bloco inteiro, pois o validador conta os registros 11 depois da rubrica 8792 conjuntamente. Confronte a concatenação das linhas e dos erros com a rodada da empresa. Publique somente a classificação da mensagem e seu campo, pois algumas mensagens embutem o registro. [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725) [app/folha/export.py:734](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:734)

Fixe o relógio da medição com `freeze_time` e capture a hora real do cabeçalho antes do congelamento. O fechamento lê o relógio internamente e captura exceções por colaborador. Compare as chaves retornadas com `universo_da_competencia` e falhe com os IDs ausentes, ou propague a exceção original no ponto de log. [app/requirements.txt:39](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/requirements.txt:39) [app/ponto/services/fechamento.py:574](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:574) [app/ponto/services/fechamento.py:636](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:636) [app/ponto/services/fechamento.py:1327](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1327)

**E.** O cadastro é `core.models.Feriado`, tabela `core_feriado`. Use `Feriado.objects.filter(abrangencia='municipal', data__range=(date(2026, 11, 21), date(2026, 12, 20))).order_by('uf', 'municipio', 'data', 'pk')`. Para cruzar por empresa e praça, leia os pares `Posto.praca` e `Posto.municipio` da empresa e use `core.feriados.alcanca(registro, posto.municipio)`, que normaliza UF e nome do município. [app/core/models.py:5](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/models.py:5) [bin/sombra_regras_pessoais.py:194](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sombra_regras_pessoais.py:194) [app/colaboradores/models.py:219](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:219) [app/core/feriados.py:57](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:57)

O cenário de teste identifica **10/12/2026, Aniversário de Londrina/PR**. A presença do registro na Sombra permanece [nao verificado]. [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:8](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:8) [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62)

**Envelope.** O adaptador existente encaminha a `--sombra`. Configure-o via `CODEX_SONDA_ENVELOPE` e a árvore fixada via `SONDA_ARVORE`. Capture exclusivamente a exceção sentinela após o `atomic` reverter, pois o wrapper descarta toda a saída quando o envelope termina com código diferente de zero. [Adaptador da Sombra](/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3) [bin/sonda_leitura.sh:226](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:226) [bin/codex_pergunta.sh:628](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:628)

## 27/11 15:07 · sessao d1951f170791 · - · - · origem=Ronald (tmux)

Auditei os 40 IDs atribuídos e encaminhei as linhas e os cruzamentos com o placar. Os sítios, a classificação e as ressalvas de cada obra foram enviados nos três blocos. A confirmação do hash da cópia e a incidência atual em produção permanecem [nao verificado].

## 27/11 15:07 · sessao db709b7df738 · - · - · origem=Ronald (tmux)

Auditei os 48 IDs atribuídos e enviei as linhas ao agente principal em cinco blocos. A atualização final inclui o exemplo vivo de carimbo desatualizado em `app/ponto/models.py:180` e `app/ponto/services/justificativa.py:351–353`. A prevalência em produção ficou `[nao verificado]`.

## 27/11 15:06 · sessao 313d857a7dc2 · INVENTARIO-PELA-REGRA · leitura · origem=Code

## INVENTARIO-PELA-REGRA — BATIDA/ESPURIA

15 casos escritos pela regra, com cadastros e batidas sintéticos. O diretório fornecido veio sem metadados Git. A correspondência ao hash `59600163ccc9cffb8f3ae148beede7cd7866ae86` e o resultado na pista ficam [nao verificado]. `app/docs/LEIS.md:136` · `app/docs/LEIS.md:154`

As batidas estão em horário local UTC−3. As datas são de 2026 e cada caso é independente. No 6x1 comercial, o cadastro-base é de 44 h semanais, com 08:00–17:00 e pausa de 12:00–13:00 de segunda a sexta, sábado 08:00–12:00 e DSR no domingo. Esses horários são entradas da bateria. `app/docs/LEIS.md:75` · `app/docs/LEIS.md:107` · [nao verificado]

| ID | Família | Escala | CADASTRO | BATIDAS | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| BAT-01 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base. O primeiro evento é retransmitido com a mesma identidade. | 23/11: 08:00:00 duas vezes, 12:00, 13:00, 17:00. | Normais **480**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. A retransmissão vale uma entrada. | L-087 `app/docs/LEIS.md:119`, L-099 `app/docs/LEIS.md:122`, L-110 `app/docs/LEIS.md:136`. |
| BAT-02 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. As duas candidatas à entrada têm IDs distintos, sem confirmação de duplicidade. | 23/11: 08:04:58, 08:05:03, 12:00. | `lei: qual critério confirma duplicidade entre registros distintos, qual instante prevalece e como os segundos são comparados ao limite de tolerância?` **Pergunta 1.** | L-087 `app/docs/LEIS.md:119`, L-099 `app/docs/LEIS.md:122`. CLT, art. 58, § 1º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art58) |
| BAT-03 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base. A batida isolada das 10:00 é suspeita de espúria, sem decisão ou retratação. | 23/11: 08:00, 10:00, 12:00, 13:00, 17:00. | `lei: qual evidência permite desconsiderar a marcação isolada na apuração e quais minutos são liberados enquanto a decisão está em aberto?` **Pergunta 2.** | L-025 `app/docs/LEIS.md:76`, L-087 `app/docs/LEIS.md:119`, L-088 `app/docs/LEIS.md:120`, L-125 `app/docs/LEIS.md:146`. |
| BAT-04 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. Falta a saída. Colaborador sem resposta e sem outra evidência da saída. | 24/11: 08:00. | `lei: qual tratamento de pagamento e fechamento se aplica ao furo de saída sem resposta?` O marco segue em aberto. **Pergunta 3.** | L-030 `app/docs/LEIS.md:78`, L-039 `app/docs/LEIS.md:87`, L-088 `app/docs/LEIS.md:120`, L-103 `app/docs/LEIS.md:129`. |
| BAT-05 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. Falta a entrada. Colaborador sem resposta e sem outra evidência da entrada. | 25/11: 12:00. | `lei: qual tratamento de pagamento e fechamento se aplica ao furo de entrada sem resposta?` O marco segue em aberto. **Pergunta 3.** | L-030 `app/docs/LEIS.md:78`, L-039 `app/docs/LEIS.md:87`, L-088 `app/docs/LEIS.md:120`, L-103 `app/docs/LEIS.md:129`. |
| BAT-06 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. A marcação das 08:00 foi formalmente retratada. A entrada efetiva foi às 08:20. | 26/11: 08:00 retratada, 08:20, 12:00. | Normais **220**, HE50 0, HE100 0, noturno 0, falta 0, atraso **20**. O par apurável é 08:20–12:00. | L-025 `app/docs/LEIS.md:76`, L-084 `app/docs/LEIS.md:116`, L-087 `app/docs/LEIS.md:119`, L-093 `app/docs/LEIS.md:111`, `app/docs/GLOSSARIO.md:28`. |
| BAT-07 | BATIDA/ESPURIA | 12x36 noturno, 19:00–07:00 | CCT Vigilantes Londrina vigente e aplicada à empresa. Plantão previsto no dia 20, pausa de 21:00–22:00, folga no dia 21 e corte de competência no dia 21. | 20/10: 19:00, 21:00, 22:00. 21/10: 07:00. | No plantão de **20/10**, normais **660**, HE50 0, HE100 0, noturno **420** à alíquota de 20%, falta 0, atraso 0. A saída fecha o plantão na competência que termina no dia 20. O dia 21 recebe **0** minuto desse plantão. | L-026 `app/docs/LEIS.md:77`, L-085 `app/docs/LEIS.md:117`, CCT cl. 38-d e cl. 10 `CLAUDE.md:227` e `CLAUDE.md:236`, aplicação `CLAUDE.md:217`, parecer `app/core/regua_cct.py:142`. |
| BAT-08 | BATIDA/ESPURIA | 12x36 diurno, 07:00–19:00 | Plantão previsto, pausa de 12:00–13:00. A entrada de chão chega sem coordenadas. | 26/11: 07:00 sem GPS, 12:00, 13:00, 19:00. | A entrada é registrada e apurada. Normais **660**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. | L-030 `app/docs/LEIS.md:78`, L-087 `app/docs/LEIS.md:119`, L-032 `app/docs/LEIS.md:80`. |
| BAT-09 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base. Entrada efetiva seis minutos após o marco. | 26/11: 08:06, 12:00, 13:00, 17:00. | Normais **474**, HE50 0, HE100 0, noturno 0, falta 0, atraso **6**. O limite por marcação foi excedido. | L-084 `app/docs/LEIS.md:116`, L-093 `app/docs/LEIS.md:111`. CLT, art. 58, § 1º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art58) |
| BAT-10 | BATIDA/ESPURIA | 6x1 comercial | Domingo de DSR previsto como folga. Trabalho autorizado e efetivo, sem folga compensatória ou banco de horas. | 29/11: 08:00, 12:00. | Normais 0, HE50 0, HE100 **240**, noturno 0, falta 0, atraso 0. O trabalho de domingo é pago em dobro e o DSR é preservado. | L-086 `app/docs/LEIS.md:118`, L-093 `app/docs/LEIS.md:111`, CLT, art. 67 e Súmula 146 do TST. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art67) · [TST](https://jurisprudencia-backend.tst.jus.br/rest/documentos/58ebcbe1221a82779995e18d8a4ba19d) |
| BAT-11 | BATIDA/ESPURIA | Personalizado sem intervalo em cada bloco, dupla pegada | Dois blocos previstos no mesmo dia, 08:00–12:00 e 16:00–20:00. Acordo escrito para o vão de quatro horas, integralmente de descanso. | 23/11: 08:00, 12:00, 16:00, 20:00. | Normais **480** = 240 + 240, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. | L-024 `app/docs/LEIS.md:75`, L-087 `app/docs/LEIS.md:119`, L-091 `app/docs/LEIS.md:124`. CLT, art. 71, caput e § 2º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art71) |
| BAT-12 | BATIDA/ESPURIA | Personalizado sem intervalo | Empresa com corte no dia 21. Dia 20 de folga, dia 21 com jornada prevista de 00:00–04:00. Dois cenários independentes, cada um com uma marcação avulsa e sem resposta do colaborador. | A: 20/10 às 23:58, nenhuma marcação em 21/10. B: 21/10 às 00:02, nenhuma marcação em 20/10. | `lei: enquanto a avulsa está sem par, qual competência assume a pendência em cada cenário e qual efeito ela tem no pagamento ou fechamento de cada competência?` **Pergunta 4.** | L-026 `app/docs/LEIS.md:77`, L-073 `app/docs/LEIS.md:100`, L-085 `app/docs/LEIS.md:117`, L-088 `app/docs/LEIS.md:120`, L-103 `app/docs/LEIS.md:129`. |
| BAT-13 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. A saída das 11:40 foi formalmente retratada. Saída efetiva às 12:00. | 27/11: 08:00, 11:40 retratada, 12:00. | A exibição mostra **as três marcas cruas**. A apuração usa 08:00–12:00. Normais **240**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. | Portaria 671 conforme L-025 `app/docs/LEIS.md:76`, L-087 `app/docs/LEIS.md:119`, `app/docs/GLOSSARIO.md:28`. |
| BAT-14 | BATIDA/ESPURIA | Personalizado sem intervalo em cada bloco, dupla pegada | 08:00–12:00 e 16:00–20:00, com acordo escrito para o vão de quatro horas. Teste do limite diário com variações de até cinco minutos por marcação. | 24/11: 08:05, 12:00, 16:05, 20:00. 25/11: 08:05, 12:00, 16:05, 19:59. | **24/11**, soma de desvios **10**: normais **480**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0, saída antecipada 0. **25/11**, soma **11**: normais **469**, HE50 0, HE100 0, noturno 0, falta 0, atraso **10**, saída antecipada **1**. | CLT, art. 58, § 1º e art. 71, caput. L-093 `app/docs/LEIS.md:111`. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art58) |
| BAT-15 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base, pausa fechada. Trabalho até 18:31 comprovado e os 91 minutos após o marco expressamente autorizados para pagamento, sem compensação. | 26/11: 08:00, 12:00, 13:00, 18:31, com a última marcação gravada como `E`. | A última marcação é a saída pela posição, **91 min** após o marco de 17:00. Normais **480**, HE50 **91**, HE100 0, noturno 0, falta 0, atraso 0. | L-087 `app/docs/LEIS.md:119`, L-097 `app/docs/LEIS.md:115`, L-115 `app/docs/LEIS.md:153`. CLT, art. 59, § 1º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |

### Propriedades fixas da família

- **Registro e apuração.** A batida de chão entra. A exibição preserva o cru. A apuração exclui a marca retratada e atribui o papel pelo instante contra os marcos cadastrados. `app/docs/LEIS.md:76` · `app/docs/LEIS.md:78` · `app/docs/LEIS.md:119`
- **Furo e falta.** Marco faltante permanece em aberto. A falta exige decisão. O zero de realizado para o fechamento sem par tem o alcance da L-103. A apresentação `Sem turno pareado` segue a L-102. `app/docs/LEIS.md:87` · `app/docs/LEIS.md:120` · `app/docs/LEIS.md:128` · `app/docs/LEIS.md:129`
- **Virada do dia.** A jornada pertence ao dia de início. A competência usa o corte da empresa. A saída após a meia-noite fecha o plantão de origem. `app/docs/LEIS.md:77` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:123`
- **Coerência dos minutos.** Repetir o mesmo fato dá o mesmo resultado, o previsto vem do cadastro, as parcelas de trabalho somam o total e os leitores usam o mesmo valor. O noturno é a base do adicional sobre os minutos elegíveis. `app/docs/LEIS.md:60` · `app/docs/LEIS.md:75` · `app/docs/LEIS.md:113` · `app/docs/LEIS.md:122` · `app/docs/LEIS.md:136` · `app/core/regua_cct.py:136`

### Perguntas de lei

1. **BAT-02.** Quais critérios confirmam que duas marcas com IDs distintos e poucos segundos de diferença representam uma só batida? Qual instante prevalece e qual precisão se aplica ao limite de cinco minutos? `app/docs/LEIS.md:119` · `app/docs/LEIS.md:122` · [nao verificado]
2. **BAT-03.** Qual evidência permite excluir da apuração uma candidata a espúria isolada, sem retratação, e quais minutos podem ser pagos enquanto ela aguarda decisão? `app/docs/LEIS.md:76` · `app/docs/LEIS.md:120` · `app/docs/LEIS.md:146` · [nao verificado]
3. **BAT-04 e BAT-05.** Qual pagamento ou retenção se aplica ao furo de entrada ou saída sem resposta do colaborador no fechamento? Que evidência pode resolver cada ponta? `app/docs/LEIS.md:78` · `app/docs/LEIS.md:120` · `app/docs/LEIS.md:129` · [nao verificado]
4. **BAT-12.** A qual competência pertence a pendência de uma avulsa junto ao corte 20/21 antes de haver par, e qual é seu efeito em cada fechamento? `app/docs/LEIS.md:77` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:129` · [nao verificado]

## 27/11 15:05 · sessao 3acafa1bd4ec · INVENTARIO-PELA-REGRA · leitura · origem=Code

## Vermelhos

Os valores observados vêm do `bateria.log` fornecido. As linhas abaixo identificam as asserções e os caminhos de produção na cópia. Usei a L-099 para atribuir o dono ao dado, ao código de produção ou, quando a asserção mede outra coisa, ao instrumento. [app/docs/LEIS.md:122] [app/docs/LEIS.md:136]

| Caso | Regra | Sistema | Dono | Sítio | Nota |
|---|---|---|---|---|---|
| **ESC-11** · `campo='horas_extra_50'` | CLT art. 59 e L-097/L-115. Os **150 min autorizados** contam integralmente após a saída posicional. O teste pede **150 em HE50**. O corte HE100-RUBRICA do projeto destina o excedente diário acima de 120 min à HE100 de dia normal. [app/docs/LEIS.md:115] [app/docs/LEIS.md:153] [app/docs/DOSSIES.md:189] [app/docs/RELATO-ARQUIVO.md:16815] [app/ponto/tests/test_inventario_escala.py:327] | **120 min em HE50**, conforme o log. A divisão do motor destina os outros **30 min à HE100**. Essa segunda parcela é inferida do código. [app/ponto/tests/test_inventario_escala.py:171] [app/ponto/motor_calculo_v2.py:3526–3540] | **INSTRUMENTO** | `MotorComercial.calcular_mes` divide a extra diária entre HE50 e HE100. [app/ponto/motor_calculo_v2.py:3533–3540] | Medir 630 trabalhados, 150 de extra total, 120 em HE50 e 30 em HE100 de dia normal. Assim a asserção cobra o reconhecimento integral da autorização e a rubrica definida pelo corte. [app/ponto/tests/test_inventario_escala.py:321–327] [app/docs/RELATO-ARQUIVO.md:16815] | [nao verificado]
| **ESC-11** · asserção do excedente | O trabalho autorizado excede o limite ordinário de 120 min em **30 min**. O teste procura esse excedente em `HE50 − teto`. [app/core/regua_cct.py:50] [app/ponto/tests/test_inventario_escala.py:328–330] | **0 min** na expressão testada, pois ela subtrai 120 dos 120 classificados como HE50. [app/ponto/tests/test_inventario_escala.py:330] [app/ponto/motor_calculo_v2.py:3533–3539] | **INSTRUMENTO** | `MotorComercial.calcular_mes` e os agregadores de HE50 e HE100. [app/ponto/motor_calculo_v2.py:323–336] [app/ponto/motor_calculo_v2.py:3533–3539] | Comparar **HE total − teto = 30** e conferir os 30 na HE100 de dia normal. [app/docs/RELATO-ARQUIVO.md:16815] [app/ponto/motor_calculo_v2.py:323–336] | [nao verificado]
| **ESC-12** | CLT art. 67, Lei 605/49, Súmula 146 e L-086. No domingo de DSR trabalhado e sem compensação, **0 previsto, 240 trabalhados como base da dobra, 0 normais e 0 HE50**. [app/docs/LEIS.md:118] [app/ponto/tests/test_inventario_escala.py:332–349] | O previsto **0** passou. A soma `horas_trabalhadas + horas_ft` deu **0 min**. A rubrica na lavra ficou [nao verificado], pois a falha ocorre antes de `fechar()`. [app/ponto/tests/test_inventario_escala.py:344–349] | **ESTRUTURA** | `parear_turnos` usa a pausa do template quando o domingo vem sem marcos no DNA. O ramo de absorção toma `12:00 S` como pausa e o fecho posterior exige uma batida depois dela. [app/ponto/management/commands/gerar_celulas.py:73–93] [app/ponto/turnos.py:1224–1244] [app/ponto/turnos.py:1356–1376] [app/ponto/turnos.py:1166–1175] | A leitura do código explica o zero no recorte. Há um par `08:00 E / 12:00 S` na massa e o motor comercial devolve zero trabalhado para o período que recebe sem saída. **Single Source of Truth / Aggregate Root**. O pareamento usa marcos do template para uma folga que a célula declara sem marcos. [app/ponto/tests/test_inventario_escala.py:123–135] [app/ponto/tests/test_inventario_escala.py:334–346] [app/ponto/motor_calculo_v2.py:3331–3336] [app/docs/GLOSSARIO.md:11] | [nao verificado]
| **AUS-03** | L-022/L-089. O atestado cobre só o dia de folga do 12×36. O dia mantém **`folga`, 0 previsto, 0 abono, 0 falta e 0 consumo de saldo**. [app/docs/LEIS.md:73] [app/docs/LEIS.md:121] [app/ponto/tests/test_inventario_ausencia.py:61–75] | **`('ausencia', 0)`**. Abono, falta e saldo nesta rodada [nao verificado]. A asserção de classificação interrompe as duas conferências seguintes, e a de saldo é um skip. [app/ponto/tests/test_inventario_ausencia.py:70–75] | **ESTRUTURA** | `_marcar_ausencias` preserva a folga para `falta`, mas aplica o rótulo `ausencia` ao atestado. O cartório copia esse tipo para a ata, e `grade_da_celula` o lê. [app/escala/utils.py:1248–1265] [app/ponto/services/cartorio.py:749–759] [app/escala/services/leitor_celula.py:397–422] | É um erro de precedência na escrita da projeção da grade, documentado na dívida da L-089. Quebra adicional de padrão arquitetural [nao verificado]. [app/docs/LEIS.md:180] [app/docs/GLOSSARIO.md:25] |
| **AUS-05** · `ERROR` | L-039/L-088. Uma entrada às 08:00 com pergunta em aberto preserva **480 previstos** e o **furo em aberto**, sem falta decidida nem desconto de DSR por esse evento. [app/docs/LEIS.md:87] [app/docs/LEIS.md:120] [app/ponto/tests/test_inventario_ausencia.py:90–118] | A criação da pergunta levantou `ValueError`: a fixture passou `instancia_marco='fim'`, e `batida_ausente` deriva `''`. Resultado da apuração [nao verificado]. [app/ponto/tests/test_inventario_ausencia.py:103–111] [app/chamados/models.py:3898–3909] | **INSTRUMENTO** | `PerguntaDisputa.save` valida a identidade no nascimento. `instancia_do_motivo` deriva a posição pela tabela de motivos. [app/chamados/models.py:3852–3856] [app/chamados/services/declaracao_texto.py:43–58] | Para a pergunta genérica `batida_ausente`, omitir `instancia_marco` e deixar o modelo derivá-la. Para uma pergunta específica sobre a saída, usar o motivo de marco `orfao_14h`, que deriva `fim`. Manter a pergunta aberta e as asserções de previsto, furo e falta. [app/chamados/models.py:3898–3909] [app/chamados/services/declaracao_texto.py:43–58] [app/ponto/tests/test_inventario_ausencia.py:113–118] | [nao verificado]
| **FOL-03** · `rubrica='9902'` | Lei 605/49, Súmula 146 e L-086/L-095. O DSR trabalhado deve contribuir **240 min à base da dobra**, com **0 HE50** e **0 HE100 de feriado** no delta. `9902` é o código escolhido pela fixture para observar essa parcela. [app/docs/LEIS.md:113] [app/docs/LEIS.md:118] [app/folha/tests/test_inventario_folha.py:33–39] [app/folha/tests/test_inventario_folha.py:155–165] | O delta lido em **`9902` foi 0**. O valor efetivamente apurado em `horas_folga_trabalhada` nessa rodada fica [nao verificado]. [app/folha/tests/test_inventario_folha.py:95–104] [app/folha/tests/test_inventario_folha.py:164–165] | **INSTRUMENTO** | A fixture vincula `9902` a `dsr_trabalhado_diurno`, uma opção do catálogo. O emissor lê `horas_folga_trabalhada` e emite **`folga_trabalhada_diurna`**. [app/folha/tests/test_inventario_folha.py:52–68] [app/folha/models.py:32–34] [app/folha/export.py:439–447] [app/folha/export.py:739–748] | Mapear e homologar `9902` para o evento efetivamente emitido. Conferir também os 240 min de folga trabalhada no fechamento antes de comparar cenário e controle no TXT. Isso verifica se a apuração entregou a base ao emissor. [app/ponto/services/fechamento.py:953–990] [app/folha/export.py:993–1000] [app/folha/tests/test_inventario_folha.py:160–165] |
| **FOL-13** | L-099/L-114. Duas gerações com as mesmas entradas devem produzir **os mesmos bytes e totais por rubrica**. [app/docs/LEIS.md:122] [app/docs/LEIS.md:154] [app/folha/tests/test_inventario_folha.py:307–321] | O primeiro download esperado deu **204**. O log registra duas recusas por falta de eventos. A comparação de bytes e totais ficou [nao verificado]. [app/folha/tests/test_inventario_folha.py:117–128] [app/folha/tests/test_inventario_folha.py:314–321] [app/folha/views.py:275–280] | **INSTRUMENTO** | O POST chama a rota de geração. A massa de jornada ordinária depende de um evento de normais que o emissor não oferece. Sem linhas, a resposta HTMX é `204` com mensagem de erro. [app/folha/urls.py:14] [app/folha/export.py:439–461] [app/folha/export.py:739–755] [app/core/respostas.py:63–66] | Incluir na massa um evento não zero que a rota emita, por exemplo **60 min de HE autorizada e homologada**. Manter as duas chamadas idênticas, exigir dois downloads e comparar bytes e agregados. [app/folha/tests/test_inventario_folha.py:52–68] [app/folha/tests/test_inventario_folha.py:144–153] [app/folha/tests/test_inventario_folha.py:314–321] |
| **FOL-14** · `etapa='reemissão'` | L-093/L-113. Fora da guarda, a correção leva o recorte de **450 para 480 min trabalhados** e de **30 para 0 de atraso**. A emissão corrigida deve substituir a anterior. [app/docs/LEIS.md:111] [app/docs/LEIS.md:155–156] [app/ponto/services/fechamento.py:36–50] [app/folha/tests/test_inventario_folha.py:343–373] | A primeira emissão levou **30 min de atraso**. A lavra corrigida passou com **480 trabalhados e 0 atraso**, mas o segundo POST devolveu **204, “Nenhum evento a exportar”**. [app/folha/tests/test_inventario_folha.py:343–348] [app/folha/tests/test_inventario_folha.py:368–373] [app/folha/views.py:275–280] | **ESTRUTURA** | O emissor não oferece evento de normais, descarta eventos de valor zero e recusa a geração sem linhas antes de registrar o TXT. [app/folha/export.py:439–461] [app/folha/export.py:739–755] [app/folha/views.py:275–307] | Neste recorte, corrigir o atraso remove a única linha exportável da massa. **CQRS com projeções reconstruíveis**. A lavra incorpora a correção, mas a emissão fica sem uma versão que a represente. O leiaute para esse resultado fica [nao verificado]. [app/folha/tests/test_inventario_folha.py:325–326] [app/folha/tests/test_inventario_folha.py:341–373] [app/docs/GLOSSARIO.md:25] | [nao verificado]
| **FOL-14** · `etapa='vigência'` | L-113. Um TXT vigente para a empresa e competência corrigidas, com a versão anterior intacta no histórico e preservação dos dois controles. [app/docs/LEIS.md:155–156] [app/folha/tests/test_inventario_folha.py:331–339] [app/folha/tests/test_inventario_folha.py:375–384] | **`vigente.pk == antigo.pk == 3`**. As conferências posteriores de conteúdo e hash do antigo e dos controles passaram até a asserção do aviso. [app/folha/tests/test_inventario_folha.py:375–385] | **ESTRUTURA** | A recusa ocorre antes de `ExportacaoDominio.registrar`. A vigência lê o registro mais recente da mesma empresa, mês e ano. [app/folha/views.py:277–280] [app/folha/views.py:302–307] [app/folha/models.py:174–188] [app/folha/export.py:589–603] | Mesmo desvio da reemissão, observado na vigência. **CQRS com projeções reconstruíveis**. O leitor de vigência devolve o único resultado que o fluxo deixou disponível para a tripla. [app/docs/GLOSSARIO.md:25] [app/folha/export.py:589–603] |
| **FOL-14** · aviso ao DP | L-113 × DINHEIRO 2. Uma exportada que muda fora da guarda gera **aviso ao DP com os dois números**. [app/docs/LEIS.md:156] [app/folha/tests/test_inventario_folha.py:350–369] | A consulta de novas pautas para o DP devolveu **`False`**. O recálculo produziu o aviso de log sobre a competência exportada. [app/folha/tests/test_inventario_folha.py:350–355] [app/folha/tests/test_inventario_folha.py:385] [app/ponto/services/fechamento.py:559–567] | **ESTRUTURA** | O caminho genérico agenda e executa o recálculo e registra logs. O ramo da exportada registra o `warning` sem entregar os dois valores ao escritor de Pauta. A âncora de competência buscada pelo teste coincide com a forma do modelo. [app/ponto/services/fechamento.py:1822–1848] [app/ponto/services/fechamento.py:559–567] [app/pautas/models.py:74–89] | **Event-driven architecture**. O evento chega ao recálculo, e o aviso ao DP exigido para a alteração fica ausente nesse caminho. [app/docs/GLOSSARIO.md:16] [app/docs/LEIS.md:156] [app/ponto/services/fechamento.py:559–567] | [nao verificado]

## 38 skips

A cópia declara **39** chamadas a `sem_campo`. A de **AUS-05** fica depois do `ValueError`, o que deixa **38 skips executados**: 13 de escala, 9 de ausência e 16 de folha. O texto completo tem a forma `sem campo: <campo>. <motivo>`. A tabela agrupa pelo `<motivo>` e identifica cada ocorrência pela chamada. [app/ponto/tests/test_inventario_escala.py:173–175] [app/ponto/tests/test_inventario_ausencia.py:103–110]

As referências **ESC**, **AUS** e **FOL** na tabela apontam, respectivamente, para `app/ponto/tests/test_inventario_escala.py`, `app/ponto/tests/test_inventario_ausencia.py` e `app/folha/tests/test_inventario_folha.py`. [app/ponto/tests/test_inventario_escala.py:178–180] [app/ponto/tests/test_inventario_ausencia.py:31] [app/folha/tests/test_inventario_folha.py:47–48]

| Motivo literal do skip | Ocorrências | Classe |
|---|---|---|
| A apuração não tem parcela exclusiva de normais. | **9** · ESC-01:190, ESC-02:207, ESC-04:244, ESC-06:283, ESC-07:297, ESC-10:311, ESC-11:324, ESC-15:375, AUS-08:162 | ESTRUTURA-sem-campo |
| DiaPago não tem parcela exclusiva de normais. | **4** · ESC-03:226, ESC-13:360, AUS-04:84, AUS-10:187 | ESTRUTURA-sem-campo |
| O emissor não tem evento de normais. | **8** · FOL-01:136, FOL-02:151, FOL-04:175, FOL-05:186, FOL-09:242, FOL-11:281, FOL-12:298, FOL-14:341 | ESTRUTURA-sem-campo |
| O vínculo de quatro horas não declara regime de remuneração. | **4** · AUS-01:40, FOL-06:196, FOL-07:210, FOL-08:227 | ESTRUTURA-sem-campo |
| A lavra diária não contém desconto de DSR. | **2** · AUS-02:55, AUS-10:186 | ESTRUTURA-sem-campo |
| A grade classifica o domingo como folga. | **1** · ESC-01:189 | ESTRUTURA-sem-campo |
| O período expõe trabalhado total e noturno de relógio. | **1** · ESC-05:262 | ESTRUTURA-sem-campo |
| DiaPago não distingue essas parcelas. | **1** · ESC-12:343, para normais e HE100 específica de DSR | ESTRUTURA-sem-campo |
| A apuração expõe semanas com DSR e reflexo, sem essas duas bases. | **1** · AUS-01:41 | ESTRUTURA-sem-campo |
| O atestado não expõe consumo de saldo. | **1** · AUS-03:70 | ESTRUTURA-sem-campo |
| O agendamento expõe gozo e dias. A apuração de ponto não expõe verbas nem pagamento de férias. | **1** · AUS-06:131–132 | ESTRUTURA-sem-campo |
| O emissor não publica normais nem as duas bases de DSR. | **1** · FOL-06:197–198 | ESTRUTURA-sem-campo |
| O TXT registra faltas em dias, com as datas em registros 11. | **1** · FOL-07:211. Os minutos são consultados separadamente. [app/folha/tests/test_inventario_folha.py:213–215] [app/folha/export.py:215–221] | ESTRUTURA-sem-campo |
| O emissor não publica essas duas bases em minutos. | **1** · FOL-08:228, para DSR devido e descontado | ESTRUTURA-sem-campo |
| O emissor não tem evento de normais e a lavra não tem essa parcela exclusiva. | **1** · FOL-10:260–261 | ESTRUTURA-sem-campo |
| Pauta expõe texto livre, sem campos de valores por rubrica. | **1** · FOL-14:342. É o skip do **formato estruturado**. A existência do aviso foi testada e falhou. [app/pautas/models.py:83–89] [app/folha/tests/test_inventario_folha.py:385] | ESTRUTURA-sem-campo |

As sete **perguntas de lei** estão no `bateria.md` fornecido e ficaram fora dos 35 métodos e dos 38 skips. São **ESC-08/Q1** rubrica e percentual da folga comum trabalhada, **ESC-09/Q2** tratamento dos 60 min de interjornada, **ESC-14/Q3** vigência e rubrica da dobra histórica, **AUS-09/Q4** efeitos do afastamento previdenciário, **AUS-11/Q5** remuneração e DSR na licença particular, **AUS-12/Q6** férias e terço atravessando o corte, e **FOL-15/Q7** contrato de leiaute do TXT. [app/ponto/tests/test_inventario_escala.py:297–305] [app/ponto/tests/test_inventario_escala.py:351–369] [app/ponto/tests/test_inventario_ausencia.py:153–172] [app/ponto/tests/test_inventario_ausencia.py:194] [app/folha/tests/test_inventario_folha.py:385]

## 27/11 14:57 · sessao 1ba509958f06 · INVENTARIO-PELA-REGRA · construtor · origem=Code

Implementei os **35 casos de medida** em três arquivos novos sobre o HEAD `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Conferi a sintaxe, os IDs e as docstrings de fonte. O SHA é o da base. A proibição de escrita em `.git` impediu gerar um SHA de commit para a entrega. [Escala](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:180) · [Ausência](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:31) · [Folha](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:130)

**Placar Django [nao verificado].** O Python local não dispõe de `django.test` nem de `freezegun`, e o recorte da suíte usa SSH e referências Git. O candidato a RED é **FOL-03**. Ele cobra 240 minutos no evento cadastrado `dsr_trabalhado_diurno`. A lista do emissor passa diretamente de feriado para folga trabalhada. [FOL-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:155) · [Emissor](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:439) · [Rota da suíte](/home/ronald/wt-codex-inventario-pela-regra-143921/bin/suite.sh:320) [nao verificado]

### Recibo por caso

**Grade** é `grade_da_celula`, **motor** é `autoridade_do_periodo`, **fechamento** é `recalcular_fechamento_mes`, **TXT** é `montar_linhas_export` sobre `fechamento_lido`, e **POST** é `gerar_dominio_txt`. Os números abaixo são gabaritos em minutos, salvo a unidade indicada. [Grade](/home/ronald/wt-codex-inventario-pela-regra-143921/app/escala/services/leitor_celula.py:350) · [Motor](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/services/espelho.py:1007) · [Fechamento](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/services/fechamento.py:468) · [TXT](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:86) · [POST](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/views.py:104)

| Caso | Porta | Campo lido e gabarito |
|---|---|---|
| [ESC-01](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:180) | Grade, motor | Previsto `480 × 5 / 240 / 0`, total previsto e trabalhado `2.640`, domingo `folga`, HE50 e HE100 `0` |
| [ESC-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:199) | Grade, motor | Previsto `660 / 0 / 660`, trabalhado nos dois plantões `660 / 660`, descanso entre saída e entrada `2.160` |
| [ESC-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:215) | Grade, fechamento | Previsto `480 / 660 / 0`, vínculo gerador anterior em 03/11 e atual em 04–05/11, trabalhado `480 / 660`, 05/11 `folga` |
| [ESC-04](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:237) | Grade, motor, `regua_para` | Previsto `660 / 0`, um turno de `10/11 19:00` a `11/11 07:00`, trabalhado `660`, base noturna `420` a `20%` |
| [ESC-05](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:255) | Grade, motor, `regua_para` | Previsto e trabalhado `660`, noturno de relógio `420`, base legal `480` a `20%`, adicional após 05:00 `0` |
| [ESC-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:275) | Grade, motor | Previsto e trabalhado `660`, HE100 feriado `0` |
| [ESC-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:288) | Grade, motor | 20/11 `folga`, previsto, trabalhado, falta e HE100 feriado `0` |
| [ESC-10](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:303) | Grade, motor | Previsto `480`, trabalhado `540`, HE50 `60`, HE100 `0` |
| [ESC-11](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:316) | Grade, motor, `regua_para` | Previsto `480`, trabalhado `630`, HE50 `150`, limite ordinário `120`, excesso `30` |
| [ESC-12](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:332) | Grade, motor, fechamento | Previsto `0`, trabalhado `240`, HE50 `0`, HE100 total `240` |
| [ESC-13](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:351) | Grade, motor, fechamento | Previsto `0`, trabalhado `480`, HE50 `0`, HE100 feriado `480` |
| [ESC-15](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:367) | Grade, motor, `piso_intervalo_art71` | Previsto `240 / 240`, trabalhado `240 / 210`, antecipada `0 / 30`, HE e piso de intervalo `0` |
| [AUS-01](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:31) | Fechamento, motor | Falta decidida `240` em 29/10, semanas com DSR `0` |
| [AUS-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:48) | Fechamento | Previsto e abono `480`, trabalhado e falta `0` |
| [AUS-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:61) | Grade, fechamento | 10/11 `folga`, previsto, abono e falta `0` |
| [AUS-04](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:77) | Fechamento | Previsto `480`, trabalhado `450`, atraso `30`, antecipada e falta integral `0` |
| [AUS-05](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:90) | Fechamento, pergunta persistida | Previsto `480`, `furo_apurado=True`, pergunta sem resposta ou validação, falta `0` |
| [AUS-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:120) | `registrar_gozo_historico`, motor | Agendamento de `30 dias`, falta e atraso `0` |
| [AUS-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:137) | `registrar_gozo_historico`, `ausencias_na_janela`, fechamento | Um agendamento de `30 dias`, `12 / 18 dias` de gozo nos recortes, falta `0 / 0` |
| [AUS-08](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:153) | `em_gozo_hoje`, motor | `30 dias`, 12/10 dentro do gozo, 28/10 fora, trabalhado no retorno `480`, HE100 feriado no gozo `0` |
| [AUS-10](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:171) | Fechamento | Abono `480 / 480 / 480`, total `1.440`, falta `0`, trabalhado no retorno `480` |
| [FOL-01](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:130) | TXT, fechamento | Trabalhado `480`; HE50, HE100, noturno, falta e atraso `0` |
| [FOL-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:144) | TXT | HE50 `60`, HE100 `0` |
| [FOL-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:155) | TXT | HE100 de DSR `240`, HE50 e HE100 feriado `0` |
| [FOL-04](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:167) | TXT | HE100 feriado `480`, HE100 de DSR e HE50 `0` |
| [FOL-05](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:179) | TXT, fechamento, `regua_para` | Trabalhado `660`, `0025` com base `420`, alíquota `20%` no cadastro |
| [FOL-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:192) | TXT, motor | Trabalhado na semana `1.440`, uma semana com direito a DSR |
| [FOL-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:204) | TXT, `minutos_de_falta_por_dia` | Falta `240` na fonte, `1 dia` em `8792` com data `20261029`, atraso `0` |
| [FOL-08](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:223) | TXT, motor, `minutos_de_falta_por_dia` | Falta `240` na fonte e `1 dia` na rubrica de 29/10, semanas com direito a DSR `0` |
| [FOL-09](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:236) | TXT, fechamento | Atraso `30`, trabalhado `450`, falta integral e antecipada `0` |
| [FOL-10](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:248) | TXT, fechamento, `pago_do_dia` | HE50 `60`, HE100 feriado `480`, atraso `30`; trabalhado `480 + 540 + 450 + 480 = 1.950` |
| [FOL-11](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:270) | TXT, `janela_competencia`, fechamento | Janela `21/10–20/11`, HE100 feriado `480` em novembro, 20/10 e 21/11 nas competências vizinhas |
| [FOL-12](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:291) | TXT, fechamento | Outubro recebe trabalhado `660` e `0025` de `420`. Novembro recebe `0` desse plantão |
| [FOL-13](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:307) | POST, `agregar_export_dominio` | Dois downloads com bytes e totais por rubrica iguais |
| [FOL-14](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:323) | POST, fechamento, `competencia_entregue` | Trabalhado `450 → 480`, atraso `30 → 0`, um TXT vigente, bytes e hash anteriores preservados, dois controles estáveis e nova pauta ao DP |

### `lei:` pendentes

`ESC-08` Q1, folga comum trabalhada. `ESC-09` Q2, interjornada. `ESC-14` Q3, feriado histórico. `AUS-09` Q4, afastamento previdenciário. `AUS-11` Q5, licença particular. `AUS-12` Q6, apropriação de férias no corte. `FOL-15` Q7, contrato do leiaute. [Bateria anexa, seção «Perguntas de lei», Q1–Q7]

### Sem campo

Os skips são explícitos por subteste. As demais verificações do caso continuam executáveis. Os códigos `9901` e `9902` são exclusivos da fixture para distinguir feriado de DSR trabalhado. [Skip](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:173) · [Códigos da fixture](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:32)

- **sem campo: normais como parcela exclusiva** em ESC-01–04, 06–07, 10–13 e 15, AUS-04, 08 e 10, e FOL-01, 02, 04–06, 09–12 e 14. [Campos da lavra](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/models.py:324) · [Eventos do TXT](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:439)
- **sem campo: classificação DSR** em ESC-01, **HE100 diária específica de DSR** em ESC-12 e **240 minutos diurnos de relógio** em ESC-05. [Grade](/home/ronald/wt-codex-inventario-pela-regra-143921/app/escala/services/leitor_celula.py:397) · [Lavra](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/models.py:324) · [Resultado do período](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/motor_calculo_v2.py:205)
- **sem campo: remuneração horista em vínculo 6×1** em AUS-01 e FOL-06–08. **sem campo: DSR devido e descontado em minutos** em AUS-01, 02, 05 e 10 e FOL-06 e 08. [Cadastro da escala](/home/ronald/wt-codex-inventario-pela-regra-143921/app/escala/models.py:816) · [Resultado mensal](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/motor_calculo_v2.py:270) · [Emissor](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:453)
- **sem campo: consumo de saldo por atestado**, AUS-03. **sem campo: férias brutas R$ 3.000, terço R$ 1.000, total R$ 4.000 e pagamento até 28/02**, AUS-06. [AUS-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:70) · [AUS-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:131)
- **sem campo: 240 minutos na linha TXT de falta**, FOL-07. A fonte informa minutos e o TXT informa dias e datas. **sem campo: valores estruturados de antes/depois no aviso ao DP**, FOL-14. A pauta tem texto livre. [Falta no TXT](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:215) · [Pauta](/home/ronald/wt-codex-inventario-pela-regra-143921/app/pautas/models.py:86)

O instrumento escrito do 12×36 CLT ficou como premissa da massa [nao verificado]. A fixture cadastra a fase e o horário. A empresa B também tem aplicação de CCT vinculada à praça. [Fixtures](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:47)

### Comandos para o Code

```bash
PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh \
  --dir /home/ronald/wt-codex-inventario-pela-regra-143921 \
  --only "ponto.tests.test_inventario_escala ponto.tests.test_inventario_ausencia folha.tests.test_inventario_folha" \
  --parallel 1
```

Os três comandos de mutação abaixo partem de `app/` no ambiente de teste. Cada um altera uma resposta em memória e executa só o caso correspondente. Resultado dos mutantes [nao verificado]. [ESC-15](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:367) · [AUS-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:137) · [FOL-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:144) [nao verificado]

```bash
mutante_inventario() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python - "$1" <<'PY'
import sys
from datetime import date
from importlib import import_module
from unittest.mock import patch

import django
from django.core.management import call_command

django.setup()

modo = sys.argv[1]
alvos = {
    'intervalo': (
        'ponto.tests.test_inventario_escala.piso_intervalo_art71',
        'ponto.tests.test_inventario_escala.InventarioEscalaTest.test_ESC_15_quatro_horas_e_saida_antecipada',
    ),
    'gozo': (
        'ponto.tests.test_inventario_ausencia.ausencias_na_janela',
        'ponto.tests.test_inventario_ausencia.InventarioAusenciaTest.test_AUS_07_gozo_continuo_no_corte_do_dia_21',
    ),
    'he50': (
        'folha.export.eventos_do_fechamento',
        'folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_02_extra_autorizada_no_txt',
    ),
}
alvo, teste = alvos[modo] [nao verificado]
modulo, nome = alvo.rsplit('.', 1)
original = getattr(import_module(modulo), nome)

def mutado(*args, **kwargs):
    if modo == 'intervalo':
        return 15
    resultado = original(*args, **kwargs)
    if modo == 'gozo':
        return resultado.exclude(data=date(2026, 11, 20))
    return [(evento, valor) for evento, valor in resultado if evento != 'hora_extra_diurna']

with patch(alvo, side_effect=mutado):
    call_command('test', teste, interactive=False, parallel=1) [nao verificado]
PY
}

mutante_inventario intervalo
mutante_inventario gozo
mutante_inventario he50
```

## 27/11 14:48 · sessao 7b6055d82c76 · PAR-PELO-TURNO · construtor · origem=Code

**PAR-PELO-TURNO c1v13** está na cópia sobre o HEAD `74b4f07168adf5a27e35266e7175cc31bf952834`. A proibição de `git commit` e de escrita em `.git*` prevaleceu. O heredoc com `OBRA` e `LEI-AKITA` está no recibo. [SHA e comando de commit](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:181)

**L-073, contestável no RELATO.** Às 02:00, espelho e cartão exibem as duas marcas, com **0 minutos fechados, 0 h de relógio e 1 turno aberto**, sem publicar “Em aberto”. Às 14:00, a saída entra no corte inclusivo e os dois registram **960 minutos e geometria fechada**. A folha usa o instante da borda, e `montar_dias` é o escritor de `datas_saida_orfa` para a janela com linhas, inclusive quando o PDF recebe `_turnos_juiz=[]`. [RELATO](/home/ronald/wt-codex-par-pelo-turno-111717/app/docs/RELATO.md:3) · [REDs V1–V3](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_pelo_turno_c1v13.py:47) · [Relação `arquivo::função`](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:140)

Os REDs deram **5 falhas de asserção antes da cura**. Depois, **73 testes passaram em memória** e cada um dos três mutantes falhou na asserção esperada. A contagem de `except` adicionado foi **0**. [Log anterior à cura](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:5) · [Comandos e resultados dos mutantes](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:111) · [Contagem](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:180) [nao verificado]

### Chamadores com `ate=None`

Na segunda virada, a autoridade passou de `[1]`, 0 h e 1 aberto no main para `[1,2]`, 26 h e 0 abertos na obra. O main do `porta_export` já alcançava o par pedindo `fim + 1 dia`. [Comparação reproduzida](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:149) [nao verificado]

| Chamador | Classificação no caso |
|---|---|
| `calendario` | Cura pretendida na carga. A lista de faltas consumida ficou `[]` nas duas versões. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/colaboradores/services/calendario.py:602) |
| `porta_export` | Cura pretendida no recorte. As duas janelas deram 26 h para o turno. [Comparação](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:159) |
| `aplicar_09_he_intra` | Cura pretendida na carga dos dois lados da medição. Efeito financeiro [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/management/commands/aplicar_09_he_intra.py:86) |
| `autorizacao_he_periodo` | Cura pretendida na carga da medição. Efeito na autorização [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/autorizacao_he_periodo.py:88) |
| `diff_janela_he` | Cura pretendida na carga. Delta das rubricas [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/management/commands/diff_janela_he.py:58) |
| `diff_calculador` | Cura pretendida na carga do lado da autoridade. Delta agregado [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/management/commands/diff_calculador.py:145) |

**Pendente para o Code e o opus final.** O comando com todos os labels está no recibo. O `suite.sh` usa SSH ou Docker nesse recorte, ambos vedados nesta volta. Suite de integração, PDF binário, banco e produção [nao verificado]. Ficaram fora a exibição de retratadas pela L-025 e a apuração própria dos cards do app. O selo vizinho `test_falta_um_significado.py:177` falhou em memória porque procura a chamada literal com quatro argumentos. A asserção foi preservada. [Comando da suite e fila](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:166) · [Selo vizinho](/home/ronald/wt-codex-par-pelo-turno-111717/app/relatorios/tests/test_falta_um_significado.py:177) [nao verificado]

## 27/11 14:46 · sessao 2659bd4c88e1 · E4C-F5-INSTANTE · sonda · origem=Code

## E4C-F5-INSTANTE — sonda na Sombra

**Corte (`transaction_timestamp()`)** [nao verificado]. Pela janela de 30 dias do anexo `r.md`, §4, o corte cai em **2026-11-27**, no fuso `America/Sao_Paulo`. A saída contém as contagens diárias, sem o horário do corte. `sonda.stdout:3–32` · [app/config/settings/base.py:93](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/settings/base.py:93)

A execução fornecida registra leitura na Sombra. Pelo contexto informado, a Sombra é o dump do dia e os números medem o código do `main` de hoje. A revisão executada e uma nova execução ficam [nao verificado]. `sonda.stdout:1`

As referências `sonda.stdout` numeram a saída raspada fornecida a partir de `sonda_leitura`. Os totais são as somas das 30 linhas. `sonda.stdout:1–32`

| dia | respostas | com_2_ou_mais_eventos_depois | sem_evento_depois | sem_celula_resolvida | anteriores_ao_horizonte | fonte |
|---|---:|---:|---:|---:|---:|---|
| 2026-10-29 | 86 | 19 | 67 | 0 | 86 | `sonda.stdout:3` |
| 2026-10-30 | 39 | 0 | 39 | 0 | 39 | `sonda.stdout:4` |
| 2026-10-31 | 44 | 4 | 40 | 0 | 44 | `sonda.stdout:5` |
| 2026-11-01 | 62 | 1 | 60 | 0 | 62 | `sonda.stdout:6` |
| 2026-11-02 | 95 | 5 | 90 | 0 | 95 | `sonda.stdout:7` |
| 2026-11-03 | 125 | 1 | 122 | 0 | 125 | `sonda.stdout:8` |
| 2026-11-04 | 34 | 1 | 32 | 0 | 34 | `sonda.stdout:9` |
| 2026-11-05 | 76 | 12 | 62 | 0 | 76 | `sonda.stdout:10` |
| 2026-11-06 | 90 | 2 | 88 | 0 | 90 | `sonda.stdout:11` |
| 2026-11-07 | 32 | 1 | 29 | 0 | 32 | `sonda.stdout:12` |
| 2026-11-08 | 45 | 19 | 26 | 0 | 45 | `sonda.stdout:13` |
| 2026-11-09 | 161 | 19 | 138 | 0 | 161 | `sonda.stdout:14` |
| 2026-11-10 | 93 | 10 | 83 | 0 | 93 | `sonda.stdout:15` |
| 2026-11-11 | 178 | 19 | 158 | 0 | 178 | `sonda.stdout:16` |
| 2026-11-12 | 163 | 26 | 136 | 1 | 163 | `sonda.stdout:17` |
| 2026-11-13 | 102 | 4 | 98 | 0 | 102 | `sonda.stdout:18` |
| 2026-11-14 | 129 | 17 | 112 | 0 | 129 | `sonda.stdout:19` |
| 2026-11-15 | 57 | 7 | 50 | 0 | 57 | `sonda.stdout:20` |
| 2026-11-16 | 142 | 13 | 129 | 0 | 142 | `sonda.stdout:21` |
| 2026-11-17 | 84 | 13 | 71 | 0 | 84 | `sonda.stdout:22` |
| 2026-11-18 | 104 | 41 | 57 | 0 | 104 | `sonda.stdout:23` |
| 2026-11-19 | 157 | 89 | 65 | 0 | 157 | `sonda.stdout:24` |
| 2026-11-20 | 133 | 133 | 0 | 0 | 77 | `sonda.stdout:25` |
| 2026-11-21 | 77 | 77 | 0 | 0 | 0 | `sonda.stdout:26` |
| 2026-11-22 | 64 | 64 | 0 | 0 | 0 | `sonda.stdout:27` |
| 2026-11-23 | 122 | 122 | 0 | 0 | 0 | `sonda.stdout:28` |
| 2026-11-24 | 123 | 123 | 0 | 0 | 0 | `sonda.stdout:29` |
| 2026-11-25 | 76 | 76 | 0 | 0 | 0 | `sonda.stdout:30` |
| 2026-11-26 | 73 | 73 | 0 | 0 | 0 | `sonda.stdout:31` |
| 2026-11-27 | 0 | 0 | 0 | 0 | 0 | `sonda.stdout:32` |
| **Total** | **2.766** | **991** | **1.752** | **1** | **2.175** | `sonda.stdout:3–32` |

## 27/11 14:44 · sessao dbed3086e52c · E4C-F5-INSTANTE · leitura · origem=Code

# E4C-F5-INSTANTE — mapa da volta

**Na cura5, o ato de resposta comum insere 1 evento pelo `registrar`.** Uma resposta pública de falta ou atestado que também absorva a própria pergunta pode inserir **3 eventos na mesma célula** pelas três passagens identificadas. A frequência em produção fica [nao verificado]. `app/chamados/services/disputa_emissao.py:1565–1568,1593–1597` · `app/chamados/services/materializacao.py:1065–1073,1123–1153,1190–1196` · `app/chamados/models.py:4797–4803,4821–4843`

Os blobs conferidos da cópia batem com o lado `+` do delta. O recibo identifica o lado anterior como **cura4 `8529b6003`**, também identificado no teste de autoria. A cópia veio sem metadados Git. A identidade do commit da cópia e a contagem no **`main 59600163c`** ficam [nao verificado]. A coluna da cura4 abaixo usa os hunks removidos do delta fornecido. `recibo.md:3–5` · `app/chamados/tests/test_e4c_f5_c4_red.py:701–708` · `app/chamados/models.py:4760–4768`

## 1. Quem escreve, quem lê e quantas vezes acorda

**Unidade da conta:** inserts sobreviventes de `EventoCelula`, por célula, no caminho indicado. A emissão lê `PerguntaDisputa.celula_id`, o dono pela disputa e `data_evento`. Resolve pela FK, com fallback por dono e dia, e conta uma vez por célula **dentro de cada chamada**. Endereço sem célula resolvida dá **0**. `app/chamados/models.py:3609–3634` · `app/ponto/portas/celula.py:981–991`

| Caminho | Quem escreve e quem lê | `main 59600163c` | Cura5 | Cura4, lado `−` do delta |
|---|---|---|---|---|
| Resposta humana comum, automática ou pela trilha | A porta de resposta prepara texto e instante, ou só o instante no carimbo pela trilha. Na cura5, `registrar` lê o endereço e confere a trava, grava a projeção, insere a linha `RESPOSTA` e chama o outbox. O receiver lê a marca e sai. `app/chamados/services/disputa_emissao.py:1529–1568,1758–1765,1793–1800` · `app/chamados/models.py:4774–4786,4815–4843` · `app/chamados/signals.py:255–263` | [nao verificado] | **1**, `registrar`. | **1**, `post_save`, no caso pareado com a mesma instância e o evento ainda pendente. O `registrar` lê o recibo e consulta a fila. `app/chamados/models.py:3637–3669,4829–4838` e `app/chamados/signals.py:254–268` no lado `−`. |
| Save avulso, seguido ou não de um `registrar` separado | `PerguntaDisputa.save` grava a coluna dentro de `atomic`. O receiver relê a pergunta pelo PK após o save. Save inteiro ou parcial elegível emite. `raw`, `created` e parcial sem campos elegíveis dão **0** nesse receiver. Um registro comum subsequente grava sua própria linha. `app/chamados/models.py:3600–3613,3856–3872,4821–4843` · `app/chamados/signals.py:254–263` | [nao verificado] | **1** avulso. **2** se seguido de `registrar` comum, inclusive na mesma instância e no mesmo `atomic`. | **1** avulso. Com o registro separado, **1** se este consumir o recibo vivo da mesma instância dentro do `atomic`, **2** se o recibo estiver ausente, já consumido ou o evento já tiver sido processado. `app/chamados/models.py:3637–3669,4829–4838` no lado `−`. |
| `registrar` sem save ou dois `registrar` | Cada lote comum lê os endereços, insere as linhas e emite por célula. Estoque grava sua linha e emite **0**. Lote vazio sem projeção emite **0**. Na cura5, lote vazio com projeção usa save comum, com **1** se elegível. `app/chamados/models.py:4796–4808,4815–4843` | [nao verificado] | **1** por lote comum, **2** para dois lotes que cubram a mesma célula. | **1** por lote comum sem recibo anterior, **2** para dois lotes sem save anterior. Estoque e lote vazio emitem **0** pela porta. `app/chamados/models.py:4796–4817,4829–4838` no lado `−`. |
| `transaction.on_commit` | O receiver de beco agenda por ID de disputa. O callback relê a disputa, perguntas e decisões e pode fazer novos carimbos ou fechar o beco, em outro ato. O `on_commit` da trava mantém o registro da trava e também emite **0** diretamente. `app/chamados/signals.py:229–240` · `app/chamados/services/becos.py:25–35,38–73,80–94` · `app/chamados/services/disputa_emissao.py:2115–2125` · `app/chamados/services/trava_do_colaborador.py:65–68` | [nao verificado] | **0** diretamente pelo agendamento. Um `registrar` chamado no callback, com sua trava, emite **1**. Save avulso confirmado + esse callback dão **2**. O teste executa callbacks capturados. `app/chamados/tests/test_e4c_f5_c4_red.py:737–753` | O callback de limpeza da marca emite **0**. Em commit real, com o evento do save preservado e um novo registro comum, são **2** no total: o recibo é consumido pelo registro ou limpo ao fim do commit, e a marca do save só é criada dentro do `atomic`. A ordem dos callbacks pode afetar a simulação que os executa ainda dentro de um `atomic`. `app/chamados/models.py:3649–3669,4829–4838` no lado `−` · `app/chamados/tests/test_e4c_f5_c4_red.py:737–753` |
| Rollback do `atomic` ou do savepoint | O rollback do ato pareado leva coluna, linha e outbox, e descarta os callbacks agendados no trecho desfeito. Se só o save avulso é desfeito e o `registrar` seguinte confirma, a emissão deste sobrevive. `app/chamados/models.py:3856–3872,4815–4843` · `app/chamados/signals.py:229–240` · `app/chamados/tests/test_e4c_f5_c4_red.py:848–855` | [nao verificado] | **0** no rollback completo. **1** se só o save anterior for desfeito e o registro seguinte confirmar. | **0** no rollback completo. **1** no segundo caso: a consulta de pendência deixa de encontrar a linha do recibo. `app/chamados/models.py:3658–3669` no lado `−`. |
| Exceção entre save e emissão | No pareado, o `finally` limpa a marca e o erro que sai do `atomic` desfaz o que já foi gravado. No save avulso, falha do insert do receiver também desfaz a coluna no `atomic` do `save`. Se um save avulso já confirmou e a execução falha antes de um registro separado, a emissão do save permanece. `app/chamados/models.py:3856–3872,4780–4786,4815–4844` · `app/chamados/signals.py:247–263` | [nao verificado] | **0** no pareado abortado ou no save cujo receiver falha. **1** se o save avulso já confirmou. Uma falha posterior à saída do `atomic` da resposta, sem um `atomic` externo, também pode deixar **1** da resposta já gravada. `app/chamados/services/disputa_emissao.py:1487–1494,1565–1597` | **0** se a exceção sair do `atomic` que inclui o save e a emissão do receiver. **1** se o save avulso já confirmou. `app/chamados/services/disputa_emissao.py:1487–1494,1559–1568` e `app/chamados/signals.py:254–268` no lado `−`. |

A linha de decisão tem como leitor `DecisaoPergunta.leitura`. `composta` entrega aos juízes o instante e a validação da sequência, o texto e a materialização da coluna. **O insert de `EventoCelula` só lê a célula para obter ID, dono e dia.** O consumidor lê os eventos pendentes, relê o estado dos chamados e da célula e marca `processado_em` em lote. Quando a própria lavra escreve veredito ou ata da célula, `salvar_e_emitir` grava a célula e o evento juntos. `app/chamados/models.py:4550–4574,4660–4669` · `app/core/juizes.py:298,313` · `app/ponto/portas/celula.py:975–1002` · `app/chamados/reconciliador.py:1465–1477,1492–1537,1546–1556`

**Outros caminhos que alteram a contagem na cura5.** O equivalente em `main 59600163c` fica [nao verificado].

| Caminho | Escrita, leitura e contagem na cura5 |
|---|---|
| Carimbo e fechos com `SILENCIO` | **1** por pergunta carimbada: `registrar` no lote com ato, `post_save` no carimbo sem ato. Os dois fechos com `SILENCIO` chamam por pergunta, então perguntas diferentes podem produzir vários eventos na mesma célula. `app/chamados/models.py:2175–2189,3515–3524,3561–3577,4797–4803` · `app/ponto/services/veredito_celula.py:192–203` |
| Nascimento e reuso | Nascimento: **0** pelo receiver de outbox e **0 ou 1** pelo registro das assinaturas. Reuso: o save de vínculo emite pela célula atual e a porta usa o endereço anterior capturado. São **2 na mesma célula** se só mudou o chamado-destino. `app/chamados/models.py:3271–3292,3306–3328,4505–4524` · `app/chamados/signals.py:254–263` |
| `reabrir_para_correcao`, inclusive `RECUSA`, e L-130 | O lote grava coluna por `update`, sem passar pelo receiver. `registrar` emite pelas linhas de decisão e a porta emite pela captura anterior. Na reabertura são **2** na interseção e **1** onde só há captura. Na L-130 são **2** no sucesso e **0** se o `update` vazio levantar e desfizer o ato. `app/chamados/services/disputa_emissao.py:1671–1717` · `app/chamados/services/validacao.py:471–496,1013–1021` |
| Ressuscitação | **2** por célula coberta pela compensação e pela emissão após o `update`, **3** se o lote anterior de passivo também a cobrir, **0** sem alvos. `app/chamados/models.py:1635–1642,1675–1686,1720–1733` |
| Retratação de motivo | **1** pelo `registrar(RETRATACAO)` e mais **1** pelo receiver se o carimbo posterior gravar. `app/chamados/models.py:2316–2324,3561–3577,4797–4803` · `app/chamados/signals.py:254–263` |
| Absorção depois de `FALTA_DIA` ou `ATESTADO` | **2 adicionais** por pergunta absorvida com sucesso: um do carimbo, pelo `registrar` ou pelo receiver conforme haja ato no carimbo, e um do `registrar(ABSORCAO)`. Se a absorvida é a própria âncora, são **3** nessa célula contando a emissão do ato de resposta. O teste de `FALTA_DIA` verifica o vínculo de absorção da âncora, sem contar eventos. `app/chamados/services/disputa_emissao.py:1565–1568,1593–1597` · `app/chamados/services/materializacao.py:1065–1073,1123–1153,1190–1196` · `app/chamados/models.py:3478–3496,3522–3524,4797–4803` · `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:62–76` |

## 2. Falhas apontadas no ver5

**Conto 11 famílias.** Agrupei as portas com chamada explícita redundante, os dois selos que compartilham a dispensa e as cinco âncoras ausentes. Separei a projeção por variável do acesso direto ao helper, pois são dois pontos de entrada distintos para a escrita. A correspondência com a numeração do aval fica [nao verificado]. `app/chamados/services/disputa_emissao.py:1696–1717` · `app/chamados/models.py:4780–4803` · `app/core/censo_escritas.py:407–423` · `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:31–32,65–67,95–108` · `app/chamados/tests/e4_verificar.py:21–37`

| # | Caminho do mapa | Classe e falha |
|---|---|---|
| 1 | Save avulso + `registrar`; retratação | **Duplicado.** As duas chamadas podem emitir para a mesma célula, inclusive no mesmo `atomic`. Na retratação, o registro vem antes do carimbo. `app/chamados/models.py:2316–2324,3561–3577,4821–4843` · `app/chamados/signals.py:254–263` |
| 2 | Reuso e portas de `update` | **Duplicado.** Reuso pode emitir duas vezes para o mesmo endereço. Reabertura, ressuscitação e L-130 sobrepõem a emissão da porta à do registro onde seus conjuntos de células coincidem. `app/chamados/models.py:1681–1682,1723–1733,3306–3316` · `app/chamados/services/disputa_emissao.py:1680–1683,1696–1717` · `app/chamados/services/validacao.py:483–496` |
| 3 | Resposta pública + absorção | **Duplicado.** Carimbo e ato de absorção emitem separadamente, além da resposta da âncora quando é a mesma pergunta. `app/chamados/services/disputa_emissao.py:1565–1568,1593–1597` · `app/chamados/services/materializacao.py:1123–1153` |
| 4 | `post_save`, reentrada e erro | **Sem acordar.** A marca cobre qualquer save da mesma instância durante o escopo. Outro objeto com o mesmo PK tem seu próprio `__dict__`. No cenário do laudo, o save independente confirma com **0** se enxerga a marca e a transação que a colocou falha. Numa projeção reentrante, o `finally` interno ainda pode tirar a marca do escopo externo e reabilitar a emissão cedo demais. Ocorrência em produção [nao verificado]. `app/chamados/models.py:3856–3872,4774–4786,4815–4843` · `app/chamados/signals.py:229–240,254–263` |
| 5 | `registrar(projecao=...)` | **Censo cego.** A projeção literal é reconhecida. A mesma projeção passada por variável escapa, inclusive no ramo vazio que executa o save comum. `app/core/censo_escritas.py:407–423` · `app/chamados/models.py:4797–4803` |
| 6 | Save físico da projeção | **Censo cego.** A chamada direta a `_salvar_a_projecao` escapa e tem capacidade de gravar com o receiver calado, sem linha de decisão nem evento. Os dois saves físicos ficam visíveis no balde sem entidade, fora da contagem por porta. Chamada externa em produção [nao verificado]. `app/core/censo_escritas.py:407–454,529–547` · `app/chamados/models.py:4780–4786,4797–4803,4821–4828` · `app/chamados/tests/test_e4c_f5_c4_red.py:1576–1591` |
| 7 | `registrar` e leitura posterior | **Outra — cache velho.** A conferência aceita outra instância do mesmo PK ou um lote por `pergunta_id`. A invalidação só alcança objetos em `ln['pergunta']`, e a leitura aceita prefetch. A instância projetada pode conservar a leitura anterior. `app/chamados/models.py:4558–4574,4804–4808,4834–4840` |
| 8 | `registrar`, endereço e lote vazio | **Outra — contrato de projeção.** A lista de campos proibidos repete o endereço lido antes do save. O lote vazio passa antes da guarda e o receiver lê o endereço depois do save. Em lote não vazio, a guarda pode consumir um iterador de campos antes de repassá-lo ao save. Os chamadores mostrados passam listas e a recusa da guarda nesses chamadores não se confirma. `app/chamados/models.py:3609–3613,4770–4771,4796–4828` · `app/chamados/services/disputa_emissao.py:1565–1568,1762–1765,1797–1800` · `app/chamados/services/validacao.py:176–179,315–319,393–398` |
| 9 | Trilha de auditoria e escrita de coluna | **Selo fraco.** `_dicts_da_trilha` dispensa pelo nome `dict` sem conferir sua ligação e agrega imports de `evento` sem resolver a ligação no ponto da chamada. Isso afeta os selos de carimbo e do par. O K6 real lê a pergunta e grava `LogAuditoria`. O contraexemplo escreve ao avaliar o argumento antes de entrar na trilha. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,213–232` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:135–162` · `app/chamados/services/materializacao.py:846–859` · `app/core/observ.py:138–153` |
| 10 | Absorção, carimbo, vigia, rollback do registro e reversão da célula | **Selo fraco.** Conferi zero ocorrências para `absorcao_sem_comparador`, `tirar_guarda`, `contador_ignora_b`, `M-H2-evento` e `M-C11-reversao`. Quando selecionados, param na âncora antes da asserção de negócio. Presença nas rodadas do recibo [nao verificado]. `app/chamados/tests/e4c_f5_volta3_mutantes.py:31–32,65–67,95–108` · `app/chamados/tests/e4_verificar.py:21–37,58–64` |
| 11 | Save pareado, erro e `on_commit` | **Selo fraco — alcance da prova.** O teste do lote manteve `(1, 1)` com projeção pareada, deixando de exigir dedupe entre chamadas separadas. O mock do limbo simula o save. H2 instrumenta rollback sem projeção e o teste do callback roda sem commit real. M58 verifica a recusa dos campos com insert simulado. `app/chamados/tests/test_e4c_f5_c4_red.py:737–768,1541–1548` · `app/chamados/tests/test_curar_perguntas_em_limbo.py:29–58` · `app/chamados/tests/test_e4_expand.py:396–431` · `app/chamados/tests/e4c_f5_c4red2_mutantes.py:807–815` |

**Trilha dupla como falha separada** fica [nao verificado]. O código mostra uma porta física para a sequência, uma para o outbox e uma para a auditoria. A duplicação demonstrada acima consiste em múltiplas chamadas que inserem na mesma fila. C6 e C21 continuam nas autoridades registradas. `app/chamados/models.py:4821–4843` · `app/ponto/portas/celula.py:981–991` · `app/core/observ.py:94–106,138–153` · `app/core/juizes.py:298,313`

## 3. Desenho

O glossário nomeia **Transactional Outbox / O-OUTBOX** e **Single Writer Principle / escritor único**. A implementação existente é `salvar_e_emitir` → `emitir_evento_celula` → `consumir_eventos_celula`. A fila aceita eventos distintos ainda pendentes. A idempotência é do processamento que relê o estado. `JUIZES.md` localiza as autoridades de validação e de pergunta respondida. `app/docs/GLOSSARIO.md:9–17` · `app/docs/PLANO-PADROES.md:8–22,53–59` · `app/docs/JUIZES.md:38–39,102–109` · `app/ponto/portas/celula.py:981–1002` · `app/escala/models.py:1858–1875`

| Situação | (a) Padrão da casa, aplicado à unidade do ato da pergunta | (b) Cura5, silêncio no receiver | [nao verificado]
|---|---|---|
| Escritores do acordar | **1 dono da emissão por ato**, usando o único insert físico já existente. A unidade documentada na célula é escrita + evento no mesmo `atomic`, com um consumidor. `app/docs/PLANO-PADROES.md:8–10` · `app/ponto/portas/celula.py:981–1002` | **1 insert físico**, com **2 gatilhos** no par básico (`registrar` e receiver) e emissões explícitas em outras portas. A marca seleciona o `registrar` durante a projeção pareada. `app/chamados/models.py:1733,3316,4774–4786,4841–4843` · `app/chamados/signals.py:254–263` · `app/chamados/services/disputa_emissao.py:1714–1717` |
| Rollback e erro no meio | Escrita e evento voltam juntos. Falha posterior do consumidor deixa evento pendente ou registra tentativa. `app/ponto/portas/celula.py:994–1002` · `app/chamados/reconciliador.py:1543–1563` | Mesma garantia no pareado e no save avulso elegível comum. Num save independente que confirme sob a marca da mesma instância, pode sobrar coluna com **0** eventos se o ato que colocou a marca falhar. `app/chamados/models.py:3856–3872,4780–4786,4815–4843` · `app/chamados/signals.py:254–263` |
| Save avulso | A unidade demonstrada na porta da célula emite **1** para campos elegíveis persistidos pela porta. Essa é a referência para a responsabilidade pelo save avulso da pergunta. `app/ponto/portas/celula.py:975–1002` | **1** pelo receiver quando elegível e fora do escopo. Lote vazio com projeção usa essa mesma rota. `app/chamados/models.py:4797–4803` · `app/chamados/signals.py:254–263` |
| Dois `registrar` | Dois atos independentes que cobrem a célula dão **2** emissões. A fila admite ambas. O padrão `evento(unica=True)` da auditoria tem outra chave, baseada na igualdade do registro sob a trava do dono. `app/escala/models.py:1858–1875` · `app/core/observ.py:94–106,138–153` | **2** para dois registros comuns bem-sucedidos. O conjunto de células vistas dura uma chamada de `acordar_celulas`. `app/chamados/models.py:3621–3634,4841–4843` · `app/chamados/tests/test_e4c_f5_c4_red.py:833–839` |

**Sítios em que a mudança de responsabilidade incidiria.**

- **A cura5 já pareia** os três escritores de resposta, os três de validação/devolução, os dois métodos de carimbo e os dois chamadores de `SILENCIO`. São as entradas que passam a gravação para o `registrar`; o receiver continua como entrada dos saves avulsos. `app/chamados/services/disputa_emissao.py:1565–1568,1762–1765,1797–1800` · `app/chamados/services/validacao.py:176–179,315–319,393–398` · `app/chamados/models.py:2182–2189,3522–3524,3571–3577` · `app/ponto/services/veredito_celula.py:194–203` · `app/chamados/signals.py:254–263`
- **O alcance do escritor único por ato** inclui o receiver avulso e os caminhos que combinam carimbo, save ou `update` com outra emissão: retratação, absorção, reabertura/recusa, ressuscitação, L-130 e nascimento/reuso. Reapontamento e rechaveamento também entram no mapa porque podem precisar acordar a célula anterior e a atual. `app/chamados/signals.py:254–263` · `app/chamados/models.py:1681–1682,1723–1733,2316–2324,3271–3328` · `app/chamados/services/materializacao.py:1123–1153` · `app/chamados/services/disputa_emissao.py:1680–1717` · `app/chamados/services/validacao.py:483–496,1013–1021` · `app/chamados/services/fio.py:1083–1094,1195–1208`

**Recomendação — O-OUTBOX (*Transactional Outbox*) com escritor único da emissão por ato, na mesma transação da escrita.** `app/docs/GLOSSARIO.md:12–17` · `app/docs/PLANO-PADROES.md:8–10,53–59` · `app/ponto/portas/celula.py:981–1002`

## 4. Sonda de frota

**A prova disponível é a linha `escala.EventoCelula` (`escala_eventocelula`).** `celula_id` e `criado_em` identificam a célula e o instante da emissão sobrevivente. `processado_em` é a marca que o consumidor aplica em lote. O evento contém também dono e dia, mas carece de `pergunta_id`, `deci

[truncado em 20000 de 26910 caracteres]

## 27/11 14:37 · sessao 61335083ba97 · E1C-LIMITE-COMERCIAL · sonda · origem=Code

**SOMBRA · competência 11/2026.** Consolidei a saída raspada fornecida. Nova execução na SOMBRA nesta sessão [nao verificado]. As referências à saída contam `sonda_leitura` como linha 1. `r.md:§6`

```text
dias_sem_marcos_trabalhados=102 colabs=38
dias_HE_diferente=0 colabs_com_diff_mensal=0 dias_sem_resposta=0
```

A contagem confere com 102 pares distintos de `colab_id` e dia, distribuídos entre 38 `colab_id`. `saída raspada:2` `saída raspada:4–105` `saída raspada:106`

| Empresa | Dias | Colabs | Σ HE atual (h) | Σ HE literal-zero (h) |
|---|---:|---:|---:|---:|
| 2 | [nao verificado] | [nao verificado] | 0 | 0 |
| 3 | [nao verificado] | [nao verificado] | 0 | 0 |
| 4 | [nao verificado] | [nao verificado] | 0 | 0 |
| **Total** | **102** | **38** | **0** | **0** |

O raspado omite a associação de `colab_id` com `empresa_id`. As somas de HE por empresa são dedutíveis porque cada linha tem `(0, 0, 0, 0)` nas duas passadas, na ordem HE50, HE100, HE50 noturna e HE100 noturna. `saída raspada:3–105` `app/ponto/services/fechamento.py:639` `app/ponto/motor_calculo_v2.py:324`

**DIFF_MENSAL — soma de `literal-zero − atual` por campo.** A saída registra zero colaboradores com diferença e nenhuma linha `DIFF_MENSAL`. `r.md:§6` `saída raspada:4–106`

| Campo | Σ diferença (h) |
|---|---:|
| `horas_extras_50` | 0 |
| `horas_extras_100` | 0 |
| `horas_extras_100_feriado` | 0 |
| `horas_extras_50_noturna` | 0 |
| `horas_extras_100_noturna` | 0 |
| `horas_folga_trabalhada` | 0 |
| `horas_reflexo_dsr` | 0 |
| `saldo_banco_horas` | 0 |

Todos os 102 dias têm `FT_h > 0` e `limites_min_atual_grade=[]`. O ponto de troca do limite diário ficou sem chamadas nesses dias. O contador `dias_sem_resposta` só aumenta quando há uma chamada registrada com retorno `None`. `saída raspada:4–106` `r.md:§6` `app/ponto/motor_calculo_v2.py:3349` `app/ponto/motor_calculo_v2.py:3429`

## 27/11 14:37 · sessao be5d5a6ad6e4 · INVENTARIO-PELA-REGRA · sonda · origem=Code

**Sombra · INVENTARIO-PELA-REGRA**  
`MEDIDO_EM|2026-11-27T14:36:36.953833-03:00` (`saída fornecida:50`)

A tabela transcreve o bloco `DADO RASPADO`. As referências à saída contam a partir da linha `sonda_leitura`. Nova execução nesta sessão [nao verificado].

| Empresa | Competência | Lotes | Detalhe por lote (`id`, `publicado`, `primeiro_job_em`) | Colabs com `Holerite` | Pautas DP (n, IDs) | Nossos TXT (n, IDs) | `no_storage` | Fonte | [nao verificado]
|---|---|---:|---|---:|---|---|---|---|
| 2 | 10/2026 | 1 | `19`, `1`, `2026-11-11T19:22:58.227461+00:00` | 418 | `0, []` | `0, []` | nao medivel aqui | `saída fornecida:3–9` |
| 3 | 10/2026 | 2 | `20`, `0`, `2026-11-11T19:34:08.870092+00:00`<br>`21`, `1`, `2026-11-16T15:28:40.863399+00:00` | 111 | `0, []` | `0, []` | nao medivel aqui | `saída fornecida:10–17` |
| 4 | 10/2026 | 0 | — | 0 | `0, []` | `0, []` | nao medivel aqui | `saída fornecida:18–23` |
| `sem_empresa_no_recorte` | 10/2026 | 0 | — | 0 | [nao verificado] | [nao verificado] | nao medivel aqui | `saída fornecida:24–27` |
| 2 | 11/2026 | 0 | — | 0 | `0, []` | `0, []` | nao medivel aqui | `saída fornecida:28–33` |
| 3 | 11/2026 | 0 | — | 0 | `0, []` | `0, []` | nao medivel aqui | `saída fornecida:34–39` |
| 4 | 11/2026 | 0 | — | 0 | `0, []` | `0, []` | nao medivel aqui | `saída fornecida:40–45` |
| `sem_empresa_no_recorte` | 11/2026 | 0 | — | 0 | [nao verificado] | [nao verificado] | nao medivel aqui | `saída fornecida:46–49` |

Nos três lotes, `upload_em=nao_registrado`. `primeiro_job_em` é o horário do primeiro disparo registrado na auditoria. `saída fornecida:4,11–12` · [app/holerite/models.py:28](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/holerite/models.py:28) · [app/holerite/views.py:1712](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/holerite/views.py:1712)

`Holerite` conta colaboradores distintos com registro, incluindo versões substituídas. O runner monta `/app/media` em `tmpfs`, por isso `Holerite_vigente_no_storage` e `Holerite_storage_indeterminado` também ficam como **nao medivel aqui**. Presença de PDFs em produção [nao verificado]. `r.md:seção 3` · [bin/sombra.sh:189](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sombra.sh:189)

_8710 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
