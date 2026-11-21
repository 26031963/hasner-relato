# CODEX-ESTADO

> Ultima resposta processada: **21/11/2026 17:56** · publicado em **21/11/2026 18:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 21/11 17:56 · sessao 16374add9c62 · FERIADO-POR-POSTO · construtor · origem=Code

F1–F5 estão na cópia em três arquivos. Os módulos estáticos de F1, F2 e F5 passaram isoladamente. Em F3 e F4, as duas asserções do rótulo passaram em memória. O módulo Django parou na importação de `django.core`, então os dois testes completos seguem [nao verificado]. [manage.py:11](/home/ronald/wt-codex-feriado-por-posto-120132/app/manage.py:11) [nao verificado]

| Item | Mudança | Mesmo teste do log | [nao verificado]
|---|---|---|
| F1 | `previa_da_marca` retorna dentro do `except _DesfazAPrevia`, após a sentinela sair do `atomic`. [feriado_do_posto.py:398](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:398) | `test_sem_except_pass_novo` verde, **1/1** no módulo. O mutante que restaura `except: pass` falhou. [test_contract_no_except_pass.py:73](/home/ronald/wt-codex-feriado-por-posto-120132/app/ponto/tests/test_contract_no_except_pass.py:73) | [nao verificado]
| F2 | `aplicar_marca` nomeia `Posto` no `select_for_update`. [feriado_do_posto.py:307](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:307) | `test_zero_trava_fora_de_trava_na_ordem` verde, **22/22** no módulo. O mutante com `type(posto)` voltou a acusar `?type`. [test_e1_v7_catracas.py:387](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:387) | [nao verificado]
| F3 | A asserção de vínculo em P espera o rótulo atual e conserva `1/1`. [test_jv1_volta5_posto_do_dia_nos_vetos.py:94](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:94) | `test_RED_vinculo_em_P_cadastro_em_Maringa_entra_no_universo`. A asserção passou em memória e falhou com o literal antigo. Teste completo [nao verificado]. | [nao verificado]
| F4 | A asserção de cadastro em P espera o rótulo atual e conserva `0/0`. [test_jv1_volta5_posto_do_dia_nos_vetos.py:100](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:100) | `test_RED_cadastro_em_P_vinculo_em_Maringa_fica_fora`. A asserção passou em memória e falhou com o literal antigo. Teste completo [nao verificado]. | [nao verificado]
| F5 | Registrei `Command.handle` em `B2_menos_B1`, imediatamente antes de `LinhaTransporte.custo_mensal`, que permaneceu na lista. [test_medida_turno_do_par.py:389](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:389) | `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` verde, **35/35** no módulo. O mutante que omite só a entrada nova falhou na igualdade exata. [test_medida_turno_do_par.py:565](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:565) | [nao verificado]

### F2 — posição da trava

O modelo é `colaboradores.models.Posto`. Ele é travado primeiro em cada segmento de `aplicar_marca`, inclusive antes de chamar a retratação. A ordem de `PerguntaDisputa.trava_na_ordem` abrange **Colaborador → DisputaSupervisao → ChamadoColaborador → PerguntaDisputa**. A catraca cobra essas quatro classes e raízes ilegíveis. Nomear `Posto` preserva a trava que já existia. [feriado_do_posto.py:307](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:307) · [feriado_do_posto.py:325](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:325) · [chamados/models.py:2816](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/models.py:2816) · [test_e1_v7_catracas.py:71](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:71)

### F5 — fonte do dia e do par

A única entrada nova foi `colaboradores/management/commands/marcar_feriado_do_posto.py::Command.handle`, em **B2−B1**, o critério largo de dia. O detector marca `localdate(agora)`. O comando obtém `agora` de `timezone.now()` e passa o dia civil da execução à porta. O **dia do fato** vem de `dias_do_fato`, o **vínculo desse dia** de `vinculos_dos_dias`, e os **turnos da prévia** de `turnos_do_colab`. A prévia agrupa os turnos pelo `data_turno` já retornado e lê `entrada` e `saida` dos pares entregues. [core/medida_turno_do_par.py:220](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/medida_turno_do_par.py:220) · [marcar_feriado_do_posto.py:46](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/management/commands/marcar_feriado_do_posto.py:46) · [feriado_do_posto.py:248](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:248) · [feriado_do_posto.py:453](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:453)

### Escritores e leitores

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `Posto.pk`, `nome`, `municipio`, `opera_em_feriado` | `criar_posto` insere a linha e `definir_municipio_posto` grava a FK. `marcar_opera_em_feriado` grava a marca. O comando lê o posto pelo `pk` e imprime nome, município e marca anterior. `aplicar_marca` relê a marca sob trava para planejar e para interromper segmentos que tenham sido superados por outra marca. `eh_feriado_do_posto` usa o município ao consultar `feriados_de`. `colaboradores/services/estrutura.py:43`, `:90`, `:99` · `colaboradores/management/commands/marcar_feriado_do_posto.py:111`, `:134` · `colaboradores/services/feriado_do_posto.py:55`, `:309`, `:328`, `:346` |
| `EscalaColaborador.posto`, `trabalha_em_feriado`, `ChamadoColaborador.status_local` e dia do fato | A porta de vínculo grava o posto ao criar ou reaproveitar o vínculo e grava a chave de feriado em `marcar_trabalha_em_feriado`. `_chamados_do_posto` lê o posto do vínculo devolvido para o dia do fato e recebe os vivos de `qs_vivos`, que filtra `status_local`. A fixture grava `aberto` e `contexto_json.data_turno`. `dias_do_fato` lê `celula.data` ou, no degrau seguinte, `contexto_json.data_turno`. No ato de retratar, `reconciliar` delega a escrita do estado a `_gravar_estado`. `colaboradores/services/vinculo.py:274`, `:287`, `:898` · `colaboradores/services/feriado_do_posto.py:244`, `:283`, `:350` · `chamados/services/painel.py:155` · `chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:96` · `chamados/juizes.py:187` · `chamados/models.py:234`, `:667` |
| `plano` e totais da aplicação | `plano_da_marca` produz `candidatos`, `sem_dia`, `vivos`, `alvo`, `propagar`, `marca_antes` e `marca_muda`. `aplicar_marca` consome o plano e devolve `marca_muda`, `retratados`, `propagados` e `plano`. A prévia repassa esses valores, e o comando lê o plano e imprime os totais do `--apply`. `colaboradores/services/feriado_do_posto.py:277`, `:318`, `:325`, `:343`, `:354`, `:406` · `colaboradores/management/commands/marcar_feriado_do_posto.py:132`, `:152` |
| Foto da célula e `a`, `b`, `outras` | A porta da célula grava `colaborador_id`, `data`, `trabalha`, `escala_geradora` e `dna` na geração, `ata`, `veredito` e `veredito_em` na lavra, e `julgada_em` e `impressao` no carimbo. `_foto_das_celulas` lê a foto, `_classificar_previa` produz as três listas, e `_imprimir_previa` as lê. `ponto/portas/celula.py:293`, `:306`, `:1090`, `:1194` · `colaboradores/services/feriado_do_posto.py:403`, `:409`, `:431`, `:462` · `colaboradores/management/commands/marcar_feriado_do_posto.py:78` |
| Rótulo e `CLASSIFICAR` | O comando escreve o rótulo em `stdout`; `_dry` seleciona a linha, e as duas asserções passam a lê-la com o literal atual. O censo publica `a_classificar_pela_regra`; a lista manual `CLASSIFICAR` guarda a expectativa e o teste compara as duas por igualdade. `colaboradores/management/commands/marcar_feriado_do_posto.py:137` · `chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83`, `:94`, `:100` · `core/medida_turno_do_par.py:432` · `core/tests/test_medida_turno_do_par.py:389`, `:566` |

**LEI-AKITA 8.** O rótulo `candidatos a retratacao no feriado` descreve `len(plano['alvo'])`. O plano inclui chamados vivos no posto do dia do fato, alcançados pelo feriado, numa mudança para FECHA ou na retomada explícita. O total efetivamente retratado é contado depois de passar pelo gate de reconciliação. [feriado_do_posto.py:277](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:277) · [feriado_do_posto.py:331](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:331) · [marcar_feriado_do_posto.py:137](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/management/commands/marcar_feriado_do_posto.py:137)

**Vizinho do rótulo antigo.** O `git grep` na wt inteira encontrou somente `chamados/tests/e4b_c12_base.json:13` após a correção. Ali está o **emissor histórico** congelado para os testes c12. O verificador lê a fonte congelada, e o manifesto aponta nela o sítio S19 da retratação. O outro teste de saída já espera o rótulo atual. [e4b_c12_base.json:13](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_base.json:13) · [e4b_c12_verificar.py:18](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_verificar.py:18) · [e4b_c12_sitios.json:148](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/e4b_c12_sitios.json:148) · [test_feriado_por_posto.py:636](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/tests/test_feriado_por_posto.py:636)

### Comandos

De `app/`, a função abaixo reproduz as provas em memória. Nas catracas estáticas, `unittest.TestCase` serve de base para executar os módulos originais. Em F3 e F4, a prova executa a linha de saída e a asserção original com as contagens do anexo. [test_contract_no_except_pass.py:2](/home/ronald/wt-codex-feriado-por-posto-120132/app/ponto/tests/test_contract_no_except_pass.py:2) · [test_e1_v7_catracas.py:51](/home/ronald/wt-codex-feriado-por-posto-120132/app/chamados/tests/test_e1_v7_catracas.py:51) · [test_medida_turno_do_par.py:95](/home/ronald/wt-codex-feriado-por-posto-120132/app/core/tests/test_medida_turno_do_par.py:95)

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
from importlib import import_module
from io import StringIO
from pathlib import Path
import sys
from textwrap import dedent
from types import ModuleType
import unittest
from unittest.mock import patch

case, mode, *targets = sys.argv[1:]
command = 'colaboradores/management/commands/marcar_feriado_do_posto.py'

if case in ('F3', 'F4'):
    line, count = {'F3': (94, 1), 'F4': (100, 0)}[case]
    rows, out = [None] * count, []
    reader = Path('chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py')
    expr = dedent(''.join(reader.read_text().splitlines(True)[line-1:line+1]))
    if mode == 'mutante':
        expr = expr.replace('candidatos a retratacao no feriado', 'em feriado que o alcanca')
    emit = dedent(''.join(Path(command).read_text().splitlines(True)[136:138]))
    exec(emit, dict(w=out.append, plano={'vivos': rows}, alvo=rows))
    check = unittest.TestCase()
    check._dry = lambda: out[0]
    exec(compile('\n' * (line - 1) + expr, str(reader), 'exec'), {'self': check})
    print(out[0])
else:
    django, test = ModuleType('django'), ModuleType('django.test')
    test.SimpleTestCase = unittest.TestCase
    sys.modules.update({'django': django, 'django.test': test})
    path = Path('colaboradores/services/feriado_do_posto.py').resolve()
    source = path.read_text().splitlines(True)
    if mode == 'mutante' and case == 'F1':
        source[405] = '        pass\n' + source[405][4:]
    if mode == 'mutante' and case == 'F2':
        source[306] = source[306].replace('Posto.objects', 'type(posto).objects')
    if mode == 'mutante' and case == 'F5':
        module = import_module('core.tests.test_medida_turno_do_par')
        module.CLASSIFICAR['B2_menos_B1'].remove(command + '::Command.handle')
    real_open = open

    def read(name, *args, **kwargs):
        if isinstance(name, (str, Path)) and Path(name).resolve() == path:
            return StringIO(''.join(source))
        return real_open(name, *args, **kwargs)

    with patch('builtins.open', read):
        unittest.main(module=None, argv=['unittest', *targets], warnings='ignore')
PY
}

prova F1 cura ponto.tests.test_contract_no_except_pass
prova F2 cura chamados.tests.test_e1_v7_catracas
prova F3 cura
prova F4 cura
prova F5 cura core.tests.test_medida_turno_do_par

prova F1 mutante ponto.tests.test_contract_no_except_pass.ContractNoExceptPassTest.test_sem_except_pass_novo
prova F2 mutante chamados.tests.test_e1_v7_catracas.CatracaTravaTest.test_zero_trava_fora_de_trava_na_ordem
prova F3 mutante
prova F4 mutante
prova F5 mutante core.tests.test_medida_turno_do_par.CatracaTurnoDoParTest.test_a_classificar_publicado_sem_entrar_como_criterio_amplo

git grep -ni -F -e 'em feriado que o alcanca' -e 'em feriado que o alcança' -- :/
```

Comando do módulo F3/F4, que parou na importação local. [manage.py:11](/home/ronald/wt-codex-feriado-por-posto-120132/app/manage.py:11)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_jv1_volta5_posto_do_dia_nos_vetos --settings=config.settings_test
```

## 21/11 17:49 · sessao e72315cf8067 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei P1, P3, P5 e P7 na cópia `f3e48a4458e343075cc3873183ddc8c9cf03c533`. Confira o diff abaixo.

- **P1.** A eleição entrega ao plantio todas as candidatas que leu, inclusive as ambíguas e as inelegíveis. A trava existente recebe seus PKs e vínculos. A releitura compara a eleição, o conjunto e os PKs efetivamente travados, com `AtoDoPlantioMudou` em caso de divergência. [materializacao.py:139](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:139) · [materializacao.py:2123](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:2123) · [models.py:2875](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:2875)
- **P3.** O passivo guarda a composta da principal ao montar o grupo e relê as compostas em lote sob a trava, antes do plantio. Alteração recusa o grupo no balde `escopo_do_plantio_mudou`. [passivo_validado.py:360](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:360) · [passivo_validado.py:383](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:383)
- **P5 e P7.** O teste concorrente confere a ordem das quatro tabelas e o `ASC` do SQL enviado, e usa a eleição confirmada no plantio. A barreira rompida chega ao teste como `AssertionError` com `BrokenBarrierError` como causa. Há selos separados para as duas cláusulas da guarda da principal e para o reset em exceção. [test_e4c_f5_plantio_concorrente.py:58](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:58) · [test_e4c_f5_plantio_concorrente.py:230](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:230) [nao verificado]

### REDs na base

| Caso | Asserção que cai por leitura da base |
|---|---|
| P1, ambiguidade | O `assertEqual` dos PKs enviados ao `FOR UPDATE` recebe só a principal. A eleição ambígua retorna `None`, e a base só acrescenta a irmã eleita. `test_e4c_f5_plantio_concorrente.py:202` · `materializacao.py:2128` na base |
| P1, saída de candidata | O `assertRaisesRegex` espera `AtoDoPlantioMudou`. Uma candidata sem resposta sai do conjunto e a irmã eleita continua igual, então a base aceita. `test_e4c_f5_plantio_concorrente.py:215` · `materializacao.py:2148` na base |
| P3, composta | O `assertEqual` espera `{'escopo_do_plantio_mudou': 1}` nos três subcasos, que alteram isoladamente resposta, validação e materialização. A base chama `_trabalhar_o_par` e o balde fica vazio com o dublê do teste. O cenário com devolução real também passa a exigir esse balde e nenhuma tentativa de carimbo. `test_e4c_f5_guarda_plantio_s144.py:114` · `test_e4c_f5_guarda_plantio_s144.py:166` · `test_e4c_f5_guarda_plantio_s144.py:200` | [nao verificado]
| P7, barreira rompida | O `assertRaisesRegex` espera a falha do worker. O `except RuntimeError` da base converte a exceção da barreira em retorno de recusa. `test_e4c_f5_plantio_concorrente.py:101` · `test_e4c_f5_plantio_concorrente.py:78` na base |

### Mutantes para a sessão

Cada `sed` abaixo é independente e usa as linhas do diff entregue. Aplique um por vez em uma cópia descartável. As falhas previstas são o regex de SQL para m7/m7b, as duas asserções de `AtoDoPlantioMudou` para m8 e o `assertIs` do contexto externo para m9. `test_e4c_f5_plantio_concorrente.py:116` · `test_e4c_f5_plantio_concorrente.py:236` · `test_e4c_f5_plantio_concorrente.py:249` · `test_e4c_f5_plantio_concorrente.py:261`

```bash
obra_wt=/caminho/da/worktree-descartavel
obra_mat="$obra_wt/app/chamados/services/materializacao.py"
obra_passivo="$obra_wt/app/chamados/services/passivo_validado.py"
obra_modelos="$obra_wt/app/chamados/models.py"
obra_teste="$obra_wt/app/chamados/tests/test_e4c_f5_plantio_concorrente.py"

# m7 Pergunta
sed -i "2903s/order_by('pk')/order_by('-pk')/" "$obra_modelos"

# m7b Colaborador, Disputa, Chamado, respectivamente
sed -i "2876s/order_by('pk')/order_by('-pk')/" "$obra_modelos"
sed -i "2885s/order_by('pk')/order_by('-pk')/" "$obra_modelos"
sed -i "2898s/order_by('pk')/order_by('-pk')/" "$obra_modelos"

# m8 remove cada cláusula isoladamente
sed -i '2152s/pk not in travadas or //' "$obra_mat"
sed -i '2152s/ or _dados_da_trava_do_plantio(p) != dados_p//' "$obra_mat"

# m9 restaura o contexto somente no caminho normal
sed -i '1839,1842c\    yield\n    _IRMAS_CONFIRMADAS_NO_PLANTIO.reset(tok)' "$obra_mat"

# P1 só registra a eleita / ignora mudança do conjunto / ignora candidata sem trava
sed -i '2126c\    candidatas = {irma.pk: _dados_da_trava_do_plantio(irma)} if irma is not None else {}' "$obra_mat"
sed -i '2157s/candidatas_atuais != candidatas or //' "$obra_mat"
sed -i '2157s/ or candidatas_atuais\.keys() - travadas//' "$obra_mat"

# P3 pula a reconfirmação composta
sed -i '390,393d' "$obra_passivo"

# P7 volta a capturar RuntimeError como recusa
sed -i '81,83c\                    except RuntimeError as exc:' "$obra_teste"
```

### Porta de cada teste [nao verificado]

`--dir` recebe a raiz da worktree, que contém `app/`. Para cada mutante, a sessão usa o mesmo seletor na cópia descartável. `bin/suite.sh:285` · `bin/suite.sh:324`

```bash
obra_wt=/home/ronald/wt-codex-e4c-f5-constroi-160129

bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_duas_materializacoes_inversas_travam_o_mesmo_par_em_ordem
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_guarda_da_principal_sem_pk_travado
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_guarda_da_principal_com_dados_mudados
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_contexto_do_plantio_restaurado_em_excecao
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_trava_todas_as_candidatas_mesmo_sem_irma_eleita
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_conjunto_de_candidatas_muda_com_eleicao_igual
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_MORDE_guarda_recusa_candidata_sem_pk_travado
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_passivo_reconfirma_a_composta_da_principal
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_passivo_foto_velha_recusa_escopo_e_mesa_valida_mesmo_pk
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_passivo_b_persistido_recusa_par_e_mesa_valida_mesmo_pk
bash bin/suite.sh --dir "$obra_wt" --only chamados -- -k test_RED_barreira_rompida_escapa_do_worker
```

`git diff --check` e o parse dos quatro arquivos passaram. A prova isolada da barreira e do `ContextVar`, com dublê de conexão, terminou com `Ran 2 tests`, `OK`, `rc=0`. Suite em PostgreSQL, REDs na base, mutantes e certificação pelo Code e pelo Opus final **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:98` · `test_e4c_f5_plantio_concorrente.py:253` [nao verificado]

**LEI-AKITA** · origem=`materializacao.py:2130` + `passivo_validado.py:383` · testemunha=`PerguntaDisputa.trava_na_ordem` (`models.py:2875`) + `DecisaoPergunta.composta` (`models.py:4382`) · RED=`test_e4c_f5_plantio_concorrente.py:189,204,98` + `test_e4c_f5_guarda_plantio_s144.py:166` · quem-mais-le=laço da disputa (`materializacao.py:2212`) + passivo (`passivo_validado.py:389`) · juízes novos=0 · padrao=Single Writer Principle com ordem canônica de travas.

## 21/11 17:41 · sessao 7598a4119613 · MEDIDA-FOLHA-[nome] · leitura · origem=Code

# MEDIDA-FOLHA-[nome]-2111-1740 · volta 1

Revisei a cópia no diretório `ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5`. A cópia veio sem `.git`, portanto a identidade do commit está [nao verificado]. Conferi a sintaxe da sonda e exercitei a captura do comando E6 com dados simulados. A execução com Django e os resultados de produção estão [nao verificado].

A unidade comum da folha é **colab-competência por empresa**. A janela começa no `dia_inicio_competencia` do mês anterior e termina na véspera desse dia no mês da competência. O juiz usa 21 como padrão e limita o corte a 2–28. A sonda usa o juiz para 10 e 11/2026. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/folha/export.py:47`

## (a) Termômetro R1

| Contrato | Leitura |
|---|---|
| **Fonte** | O ESTADO exibe o R1 declarado em `placar_estrutural.placar()`. A medição vem de `e6_oraculo.Command.handle`, que chama `InsumosDoDono.dono()` → `dono_da_divergencia()`. O comando lê `espelho_do_colab`, a função que `ponto:espelho` chama em `espelho_ponto` antes de renderizar `ponto/espelho.html`. `bin/gerar_estado.py:60`, `bin/gerar_estado.py:105`, `app/core/placar_estrutural.py:123`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:316`, `app/ponto/urls.py:10`, `app/ponto/views.py:161`, `app/ponto/views.py:195` |
| **Unidade** | Um dono por dia-colab divergente. A sonda conta dias e pessoas distintas por dono, a união de pessoas e quantas aparecem em exatamente dois ou três donos. A precedência efetiva é lacuna de cadastro por folga/vínculo, batida aberta com órfã real ou contagem ímpar, fase deslocada ou Cadastro × Realidade, depois o resíduo `ESTRUTURA`. `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:198`, `app/ponto/management/commands/e6_oraculo.py:378` |
| **Universo** | Empresas ativas e colaboradores com `FechamentoMensal` na empresa e competência, sem filtro de aptidão para o TXT. O espelho fornece `minutos_realizados` ou, na sua ausência, `pago_h` em minutos. `app/ponto/management/commands/e6_oraculo.py:263`, `app/ponto/management/commands/e6_oraculo.py:303`, `app/ponto/models.py:485` |
| **Exclusões** | Batidas retratadas ficam fora do insumo. O comando pula dia sem linha, isento ou com ausência, sem valor lavrado, zero dos dois lados ou diferença explicada pela pausa declarada. Diferença absoluta de até 10 min conta como concordância. O comando pula exceções do espelho e soma erros. A sonda devolve o primeiro erro pelo formato pedido para evitar publicar totais parciais. `app/ponto/turnos.py:2872`, `app/ponto/management/commands/e6_oraculo.py:149`, `app/ponto/management/commands/e6_oraculo.py:215`, `app/ponto/management/commands/e6_oraculo.py:315`, `app/ponto/management/commands/e6_oraculo.py:331` |

**R1 de 21/11 por empresa, união e sobreposição** [nao verificado]. O número existente no placar é a medição de **06/11 na sombra**. `app/core/placar_estrutural.py:130`

## (b) Retrato, prévia e ponte com o R1

| Contrato | Leitura |
|---|---|
| **Fonte da prévia** | O seletor em `folha/exportar.html` chama `folha:preview_exportacao`. A view chama `classificar_export`, conta `entra` e `fora` e agrupa os retidos por `motivo`. `app/templates/folha/exportar.html:26`, `app/folha/urls.py:19`, `app/folha/views.py:333`, `app/folha/views.py:350`, `app/folha/views.py:406` |
| **Fonte do Retrato** | `folha:retrato_folha` lê `RetratoFolha` por empresa e competência via `linhas_do_retrato`. A barra usa `quadros_barra_retrato` → `recortar`. O template inclui a barra e a lista. `app/folha/urls.py:24`, `app/folha/views_retrato.py:194`, `app/folha/views_retrato.py:324`, `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:840`, `app/templates/folha/retrato_folha.html:32`, `app/templates/folha/retrato_folha.html:53` |
| **Unidade e universo** | Uma pessoa por fechamento. A prévia consulta os fechamentos gravados da empresa e competência, de qualquer status. Essa base coincide com a do R1 para a mesma empresa ativa no mesmo estado do banco. O Retrato lê a lista guardada na lavratura. A ponte usa a interseção com o R1 e conta também as pessoas presentes em apenas um dos lados. `app/ponto/management/commands/e6_oraculo.py:303`, `app/folha/export.py:782`, `app/folha/retrato_folha.py:110`, `app/folha/retrato_folha.py:1212` |
| **Exclusões e baldes** | A prévia põe todos os `fora` em **Retidos**. O Retrato exibe a rescisão em **Módulo próprio**, separada de **Bloqueados**. `sem_codigo_dominio`, `ferias_com_batida` e `furo_espelho` são motivos atribuídos por precedência, depois da rescisão. A batida em férias usa agendamento na janela e batida apurável. Filtros da barra e o conjunto de empresas do seletor também podem mudar o total exibido. `app/folha/views.py:351`, `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/export.py:464`, `app/folha/export.py:866`, `app/folha/export.py:902`, `app/folha/export.py:968`, `app/folha/views_retrato.py:216`, `app/colaboradores/queries.py:78` |

O **GET do Retrato com `empresa=` é não mensurável sem gravar por essa view**. O recorte chama `PreferenciaUI.set_pref`, que pode criar a preferência ou salvar seu novo valor. Os números são mensuráveis pelo leitor `linhas_do_retrato`, usado na sonda. A ponte cruza os baldes com os donos do **R1**, que classificam divergência de minutos. O campo `dono` do Retrato guarda o fio de resolução. `app/folha/views_retrato.py:155`, `app/colaboradores/models.py:212`, `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:1246`, `app/ponto/management/commands/e6_oraculo.py:378`

| Número citado | Dado recebido | Apuração nesta volta |
|---|---:|---|
| Espelho pendente, 10/2026 | 207 · `aval.txt:1` | Por empresa, motivo e dono [nao verificado] |
| Espelho pendente, 11/2026 | 298 · `aval.txt:1` | Por empresa, motivo e dono [nao verificado] |
| Entram, retidos, sem código, batida em férias | [nao verificado] | Contagens e ponte [nao verificado] |

## (c) Espelho pendente por motivo e porta

**Fonte e unidade.** A prévia exibe `furo_espelho`, cujo `motivos_espelho` sai de `motivos_retencao_celula`. A face `com_dias=True` devolve um motivo por dia retido. A sonda conta dias e colabs distintos por motivo entre os retidos por `furo_espelho` no universo do R1. Uma pessoa pode aparecer em vários motivos. O Retrato guarda o motivo geral, datas, tipo e ações. A quebra por motivo diário vem da leitura atual da autoridade. `app/folha/views.py:409`, `app/folha/export.py:968`, `app/folha/export.py:1176`, `app/folha/export.py:1259`, `app/folha/export.py:1301`, `app/folha/fora_da_folha.py:125`, `app/folha/retrato_folha.py:1237`

**Universo e exclusões.** A retenção por célula dispensa isentos e desligados, respeita admissão/demissão, ignora falta de célula em dia sem vínculo e só acusa ausência de célula ou de veredito nas condições do ramo de dia passado. Um veredito acusador segue seu próprio ramo, com as dispensas de protestos integralmente não cobraveis e de furo só de pausa. `app/folha/export.py:1205`, `app/folha/export.py:1238`, `app/folha/export.py:1264`, `app/folha/export.py:1293`

| Motivo devolvido | O que falta ou precisa ser decidido |
|---|---|
| `sem_celula` | Dia passado com vínculo sem célula, ou célula com previsão definida sem veredito. A tarefa marca falta de célula como `nao_sei`. O motivo tem **sem porta própria** no mapa. Uma oferta efetiva de Resolver dia depende do calendário. `app/folha/export.py:1264`, `app/folha/export.py:1295`, `app/chamados/services/tarefa_do_dia.py:236`, `app/chamados/services/tarefa_do_dia.py:697`, `app/folha/retrato_folha.py:1016` |
| `espelho_furo` | Furo integral ou parcial ainda acusador. O mapa inclui combinações em que falta cobrança, régua válida ou resolução. `app/escala/models.py:1611`, `app/folha/export.py:1293` |
| `espelho_discordante` | Batida órfã fora da tolerância, realizado inflado ou realizado zero com turno. `app/escala/models.py:1616` |
| `espelho_indefinida` | Dia previsto indefinido, fase por declarar ou ausência parcial sem período. `app/escala/models.py:1615` |
| `espelho_cobranca_indevida` | Cobrança em dia sem trabalho ou sem furo, ainda com veredito acusador. `app/escala/models.py:1613`, `app/escala/models.py:1767` |
| `espelho_cobrado` | O dia continua acusado depois de haver cobrança reconhecida no julgamento. `app/ponto/services/cartorio.py:1534`, `app/escala/models.py:1767` |
| `espelho_nunca_bateu` | Falta de adesão ao ponto no ramo de trabalho sem cobertura integral. `app/ponto/services/cartorio.py:1312`, `app/ponto/services/cartorio.py:1318` |
| `espelho_fato_em_ausencia` | Houve trabalho em dia coberto por ausência e o conflito segue acusado. `app/escala/models.py:1617`, `app/escala/models.py:1754`, `app/chamados/services/tarefa_do_dia.py:358` |

**Portas por ocorrência.** Para os motivos da tabela, a rota depende da ação publicada para o dia. O drawer chama `portas_da_linha` para traduzir a ação em link, com alternativas pelo fio e pelo tipo já gravados. A sonda cruza cada motivo diário com essa tradução e conta `sem_porta` e `sem_leitura` separadamente. `app/chamados/services/tarefa_do_dia.py:719`, `app/chamados/services/tarefa_do_dia.py:804`, `app/folha/fora_da_folha.py:278`, `app/folha/retrato_folha.py:1024`, `app/colaboradores/views.py:996`, `app/templates/folha/_retrato_o_que_fazer.html:42`, `app/templates/folha/_retrato_o_que_fazer.html:66`

| Ação ou situação publicada | Porta de tela para o admin | [nao verificado]
|---|---|
| Validar, decidir, abrir chamado ou cobrar pelo fio | `chamados:modal_fio` → `modal_fio_colaborador`. Cobrar deixa a resposta com o colaborador. **Abrir esse GET é não mensurável sem gravar** por essa view, que marca notas do DP como lidas. A sonda resolve apenas o nome da URL. `app/chamados/urls.py:39`, `app/chamados/views.py:869`, `app/chamados/views.py:877`, `app/chamados/services/nota.py:126`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1016` |
| Resolver dia quando oferecido | `colaboradores:calendario_ponto` → `calendario_ponto`. O formulário envia para `ponto:veredito_celula` → `veredito_celula_view`. A oferta exige dia passado, status elegível e último recurso. A tela exige permissão. `app/colaboradores/urls.py:24`, `app/colaboradores/views.py:1976`, `app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:117`, `app/templates/colaboradores/partials/_calendario_grade.html:131`, `app/templates/ponto/partials/_veredito_dia.html:17`, `app/ponto/urls.py:58`, `app/ponto/views.py:3466` |
| Decidir ausência com registro identificado | `ponto:painel_ausencia` → `painel_ausencia`. O drawer recebe o ID da ausência e `somente_decisao=1`. `app/folha/retrato_folha.py:1050`, `app/ponto/urls.py:70`, `app/ponto/views.py:2070` |
| Tipo gravado `escala`, `folga` ou `sem_escala` | Link complementar `colaboradores:painel_vinculo` → `painel_vinculo`. `app/folha/retrato_folha.py:1003`, `app/colaboradores/urls.py:12`, `app/colaboradores/views.py:262` |
| Sem ação ou fio publicado | `nao_sei` e `ninguem` têm **sem porta** pelo fio. `nao_bate`, `falta`, `ferias` e `sem_causa` têm **sem porta** complementar pelo tipo. A rota `chamados:ir_para_ausencia` exige um chamado, e o mapa do tipo não oferece esse link. `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:996`, `app/chamados/urls.py:44`, `app/chamados/views.py:1490` |

**Quantos casos de cada motivo têm rota funcional, estão sem porta ou sem leitura em produção** [nao verificado].

## (d) Central

**Fonte, unidade e universo.** `chamados:painel_gestao` renderiza a Central. As duas pílulas contam **chamados**, uma vez por verbo, sobre os vivos do recorte. `particionar_verbos` usa créditos de perguntas. Suprime a contagem em Validar/Decidir para registros sem verbo e remapeia os silenciados para Arquivado ou Histórico. A competência da folha não entra no queryset dessas pílulas. `app/chamados/urls.py:22`, `app/chamados/views.py:721`, `app/chamados/views.py:767`, `app/chamados/services/painel.py:96`, `app/chamados/services/painel.py:135`, `app/chamados/services/painel.py:227`, `app/chamados/services/verbos.py:30`, `app/chamados/services/raiox.py:51`

| Contador | Valor do anexo | Sítio |
|---|---:|---|
| **Validar** | 707 · `aval.txt:1` | `verbos_contagens.validar.n`. Resposta creditada ou módulo de validação direta, conforme o juiz do verbo. `app/templates/chamados/painel_gestao.html:42`, `app/chamados/services/painel.py:230`, `app/chamados/catalogo/natureza.py:41` |
| **Decidir** | 705 · `aval.txt:1` | `verbos_contagens.decidir.n`. Chamado acionável sem pergunta viva ou de cadastro, depois das regras anteriores do verbo. `app/templates/chamados/painel_gestao.html:79`, `app/chamados/services/painel.py:230`, `app/chamados/catalogo/natureza.py:41` |
| **Pra decidir (validar + decidir)** | 1413 · `aval.txt:1` | `badge_chamados`, lido por `navbar_context` de `PlacarNavbar`. O cron grava a soma dos dois verbos do tenant sem filtros e o payload da mesma rodada. `app/templates/chamados/painel_gestao.html:191`, `app/core/context_processors.py:59`, `app/core/context_processors.py:102`, `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/core/management/commands/lavrar_badge_navbar.py:91` |

| Pílulas | Cabeçalho |
|---|---|
| `particionar_verbos(qs_vivos(_base=montar_qs_filtrado(urgencia, q, colab, empresa, praca)))['contagens']`. No refresh `chamados:placar_json`, sem filtros, pode ler `PlacarNavbar.objects.filter(pk=1).first().payload_placar`. O JavaScript atualiza as pílulas. `app/chamados/services/painel.py:198`, `app/chamados/services/painel.py:230`, `app/chamados/views.py:840`, `app/templates/chamados/painel_gestao.html:302` | `PlacarNavbar.objects.only('badge_chamados', 'payload_placar', 'lavrado_em').first()`. Sem lavra, `exige_o_admin(particionar_verbos(qs_vivos())['contagens'])`, sem filtros. Se essa partição levantar exceção, `qs_na_fila().count()` conta abertos/em análise não silenciados no futuro, incluindo possíveis casos de Cobrar. `app/core/context_processors.py:68`, `app/core/context_processors.py:87`, `app/core/context_processors.py:110`, `app/chamados/services/painel.py:188`, `app/chamados/catalogo/motor.py:29` |

**Contraexemplo de código.** Se o tenant tem um único chamado Validar, da empresa B, a Central filtrada pela empresa A mostra Validar 0 e Decidir 0, com cabeçalho 1. Mesmo sem filtro, um chamado que passou de Decidir para encerrado depois da lavra pode produzir a mesma diferença entre pílulas ao vivo e cabeçalho gravado. Para o mesmo payload gravado, vale a identidade `badge_chamados = Validar + Decidir`. A dissonância de apresentação é demonstrável. No anexo, `707 + 705 = 1412`, um abaixo de 1413. Filtros, carimbo e caminho de leitura que produziram essa diferença específica [nao verificado]. `app/chamados/services/painel.py:127`, `app/chamados/services/painel.py:155`, `app/chamados/services/verbos.py:22`, `app/core/context_processors.py:68`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `aval.txt:1`

## (e) Retrato 11 × prévia 11

| Contrato | Leitura |
|---|---|
| **Fonte e unidade** | Colab-competência por empresa. A comparação usa `linhas_do_retrato` sobre a foto existente e os itens atuais de `classificar_export`, a fonte da prévia. A terceira leitura chama `montar_linhas` em memória, a derivação usada pelo lavrador antes do `update_or_create`. `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:1174`, `app/folha/views.py:350`, `app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/management/commands/lavrar_retrato_folha.py:77` |
| **Universo e exclusões** | Competência 11 por empresa, com comparação de presença, `status` e `motivo`. A foto pode conter uma população anterior. A reconstrução recebe os mesmos itens do juiz da prévia, via `fora_da_folha` → `dias_que_seguram`. Casos sem foto aparecem no indicador `retrato_presente`. `app/folha/views_retrato.py:194`, `app/folha/retrato_folha.py:1212`, `app/folha/retrato_folha.py:1237`, `app/folha/fora_da_folha.py:146`, `app/folha/fora_da_folha.py:200` |

**Diferenças de critério verificadas no código**

- Para `status` e `motivo`, a derivação atual do Retrato copia as saídas do **mesmo** `classificar_export` que alimenta a prévia. A foto materializa essas saídas em uma rodada anterior. A sonda compara também a reconstrução em memória com a prévia na mesma transação, com um relógio comum, incluindo o `localdate()` interno do classificador. `app/folha/views.py:350`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:146`, `app/folha/retrato_folha.py:1222`, `app/folha/retrato_folha.py:1237`, `app/folha/export.py:1238`, `app/folha/management/commands/lavrar_retrato_folha.py:74`
- **Retidos** da prévia inclui a rescisão. **Bloqueados** do Retrato exclui a rescisão e a exibe em módulo próprio. A barra do Retrato também usa o recorte da tela. Para comparar elegibilidade, a sonda usa `fora_txt` dos dois leitores e também imprime `bloqueado` e `modulo_proprio` do Retrato. `app/folha/views.py:351`, `app/folha/retrato_folha.py:27`, `app/folha/retrato_folha.py:89`, `app/folha/views_retrato.py:225`
- Há uma rota distinta, `folha:previa_proposta`, cujo leitor `montar_previa` usa a classificação para montar propostas de marcos. Ela exclui isentos, dias a partir de hoje e dias fora de vínculo, e põe mês sem batida em seção própria. A prévia que contém a mensagem **Gerar TXT para conferir** é `folha:preview_exportacao`. `app/folha/urls.py:19`, `app/folha/urls.py:21`, `app/folha/views.py:386`, `app/folha/views.py:744`, `app/folha/previa.py:41`, `app/folha/previa.py:69`, `app/folha/previa.py:107`

O aval cita **302 × 303**. A atribuição dessa diferença a defasagem, recorte ou baldes está [nao verificado]. O código guarda uma única foto por empresa e competência. O indicador de frescor compara o carimbo dessa foto ao fechamento mais recente, sem reconstituir historicamente a classificação. `aval.txt:1`, `app/folha/models.py:356`, `app/folha/models.py:365`, `app/folha/retrato_folha.py:1413`

## (f) Porta do export às 07:04

**Unidade, universo e exclusões.** O ESTADO conta linhas de recusa por data local do log e porta `porta_export`, consultando `ponto.log` e rotados. O escritor emite a recusa da tentativa que concluiu divergente ou falhou. O anexo curto repete a mesma linha que está no contexto. Há **uma tentativa identificável** no material fornecido. A contagem do arquivo completo está [nao verificado]. `bin/recusa_estado.py:60`, `app/core/recusa_critica.py:95`, `app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `app/folha/porta_export.py:1074`, `f_linhas_export.txt:1`, `f_contexto_0703_0705.txt:9`

```text
2026-11-21 07:04:59,754 WARNING ponto.recusa_critica RECUSA-ACAO-CRITICA porta=porta_export usuario=- colab=- dia=2026-11-21 objeto=emp3:2026-12:e13c73fcee9c44349e48621754bdc6d6 tipo=erro motivo=antes=em_curso depois=concluido origem=cron ator=-: os leitores discordam em 1 ponto(s)
```

- **Quem escreveu.** O cron chama `selo_leitores_no_mesmo_numero --rollout --origem cron` para a competência corrente de cada empresa em rollout. O comando chama `medir` → `concluir`, que grava o carimbo e o estado. `concluir` chama `_recusa`, e `recusa_critica.registrar` emite o `WARNING` pelo logger encaminhado a `ponto.log`. `app/config/crons.py:1809`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:43`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:218`, `app/folha/porta_export.py:1273`, `app/folha/porta_export.py:1074`, `app/core/recusa_critica.py:85`, `app/config/settings/saas.py:160`, `app/config/settings/saas.py:181`
- **Causa confirmada.** Para a empresa 3, competência **12/2026**, a medição terminou `concluido` com resultado `divergente`, `n=1`. A linha traz a soma do ponto divergente. Qual teste compôs esse `1` está [nao verificado].

[truncado em 20000 de 34488 caracteres]

## 21/11 17:40 · sessao 66840d695d92 · E4C-F5-CONSTROI · verificador · origem=Code

# BLOQUEIA o pouso [nao verificado]

1. **Erro de banco virou classe B comum.** A captura é nova, não isola a consulta numa transação e descarta a exceção. Nas rotas verificadas, a classificação ocorre antes da transação de escrita. O risco de continuar numa transação abortada existe se o serviço for chamado dentro de um `atomic()` externo. O teste injeta uma exceção em Python e não exercita esse efeito no Postgres. `app/chamados/services/validacao.py:1082–1096,1230–1237,746–760` · `app/chamados/tests/test_fila_validar_lote.py:110–126` · `CLAUDE.md:17–19`
2. **Faltam mordidas exigidas pelo aval.** Falta o teste do explicador contra texto residual, o controle de retratação que atravesse o novo retorno antecipado e o caso em que `SEM_RESPOSTA_VIGENTE` barra vínculo e destino encerrado. Os dois testes dos atalhos exercitam `VETO_TEMPORAL`. `app/chamados/tests/test_explicador_disputa.py:53–102,143–156` · `app/chamados/tests/test_marco_por_motivo.py:184–241` · `app/chamados/tests/test_juiz_le_retratacao.py:49–61` · `app/chamados/juizes.py:793–795`
3. **O PRONTO está aberto.** A sonda fornecida ficou abaixo do limite. Publicação no RELATO, registro dos dois SHAs, censo **em produção** anterior à cura, integração sobre a volta 3, medição ORM do vigia e suíte inteira verde no hash final ficam [nao verificado]. A ordem exigida para o pouso está em `CLAUDE.md:35–38,372–381` e no anexo `aval.txt:1`. [nao verificado]

## (a) Aval e rascunho

| Item | Conferência |
|---|---|
| Registrar `44f7c468aeb7` e `566c3b225f0c` | Os dois constam de `aval.txt:1`. Registro no RELATO e validação independente dos SHAs [nao verificado]. |
| Escopo nominal | O patch fornecido mostra as cinco funções de produção e o tipo de retorno nos cinco arquivos previstos. Os hashes de conteúdo dos 12 arquivos listados no patch coincidem com os prefixos `index` anexados. A cópia veio sem metadados Git, portanto a completude do delta entre `5cc451ac3` e `93ab82d08` fica [nao verificado]. O aval diz “8”, mas os sete itens de `rascunho.txt:25–31` nomeiam nove caminhos de teste. O patch mostra sete desses caminhos, a cópia contém testes de lote em `app/chamados/tests/test_fila_validar_lote.py:79–126` e falta o caso pedido em `app/chamados/tests/test_explicador_disputa.py:53–102,143–173`. |
| Sonda A→B antes do pouso | `7:2` → **1**, `7:3` → **0**, `7:4` → **0**, total **1**, abaixo de 50. O trecho fornecido mostra as contagens. A função executada, a cobertura da frota e a publicação no RELATO ficam [nao verificado]. `r_sonda.txt:3–7` · `aval.txt:1` | [nao verificado]
| Censo de horizonte antes do `ValueError` | Na cópia, `carga_da_vida` chama só com `carga=self`. O caminho de produção com **ambos** vem da tarefa, que copia `self.agora` para a carga e passa o mesmo valor ao C6. Os três outros chamadores diretos consultam a foto atual sem carga nem horizonte. Divergências em produção e a realização do censo **antes** da cura ficam [nao verificado]. `app/chamados/services/tarefa_do_dia.py:525,630–637` · `app/chamados/services/carga_pergunta_viva.py:88,119` · `app/chamados/juizes.py:793,925–932,1531–1537` · `app/chamados/services/validacao.py:1227` · `app/chamados/services/explicador.py:52` · `app/chamados/services/vigia_carimbo.py:111` |
| Contrato e ordem | O resultado é imutável, sem interface de par, e exige `ts` e `tipo` para `SLOT`. As seis saídas estão implementadas. Horizonte divergente falha antes da composta e do cache. O veto preserva o empate inclusivo. C21 lê o par projetado antes do cache, e `hora` passa uma foto ao derivador existente, com texto vazio se inelegível. O marco fixo continua disponível. `app/chamados/juizes.py:346–415,1427–1441` · `app/chamados/services/materializacao.py:691–710` |
| Materialização e cache | Veto e resposta desfeita em `data_hora` fecham os dois atalhos e conservam a retratação independente. Os demais retornos sem slot mantêm os controles de destino e lei. `SLOT` conserva as provas por vínculo e janela. A carga guarda o resultado ou a própria exceção, amplia os limites apenas para `SLOT` e sempre inclui a busca por origem. O degrau 6 conserva a captura. `app/chamados/juizes.py:793–840,855–858,1531–1538` · `app/chamados/services/carga_pergunta_viva.py:113–145,186–195` |
| Lote, explicador e vigia | O lote preserva as guardas e exige C21, `SLOT` e ausência de conflito. O `except DatabaseError` acrescenta a política de falha descrita em (c). O explicador traduz o motivo e preserva os caminhos de ação e dono. O vigia conserva universo, classes, tripla e soma. O `prefetch` agrupa decisões em blocos de 500, mas continua havendo `.exists()` de plantadas por pergunta no ramo com slot. `app/chamados/services/validacao.py:1204–1237` · `app/chamados/services/explicador.py:48–95,138–150` · `app/chamados/services/vigia_carimbo.py:94–134` |
| Volta 3, REDs e PRONTO | A cópia conserva as cinco expectativas do C8b nos dois caminhos e legaliza a fixture noturna. O caso C8b e o mutante do aval foram executados em memória. O recorte C6 de N=1 e N=4 e as catracas A1 e de respondedores passaram. A identidade da árvore com a entrega da volta 3, RED antes da cura, os demais mutantes isolados no hash final e suíte inteira ficam [nao verificado]. `app/chamados/tests/test_e3_c8b_sem_banco.py:282–299,369–433` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:188–193,251–278` · `app/chamados/tests/test_e3_catraca_respondedores.py:482–556` · `CLAUDE.md:372–381` |

## (b) Escritores, leitores e fonte única

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `slot.motivo`, `slot.ts`, `slot.tipo` | `_slot_esperado` produz. Cinco leitores diretos em produção: juiz de fato, carga, lote, explicador e vigia. Todos migraram do par para o resultado. `models.py` reexporta o símbolo, e `test_p71` chama sem consumir o retorno. `app/chamados/juizes.py:346–415,793–803` · `app/chamados/services/carga_pergunta_viva.py:119–128` · `app/chamados/services/validacao.py:1227–1236` · `app/chamados/services/explicador.py:50–62` · `app/chamados/services/vigia_carimbo.py:94–117` · `app/chamados/models.py:4580` · `app/chamados/tests/test_p71_marco_sem_hora.py:49` |
| `carga.agora`, `carga.slots[pk]` e testemunhas da carga | A tarefa escreve `agora`, a carga copia e escreve o mapa por PK. `_slot_esperado` lê o mapa e relança a mesma instância. O juiz de fato lê as testemunhas carregadas por `plantadas`, `batida_na_janela` e `retratada`. A tarefa e o C6 encaminham o horizonte. `app/chamados/services/tarefa_do_dia.py:520–539,630–637` · `app/chamados/services/carga_pergunta_viva.py:88–95,110–145,186–195` · `app/chamados/juizes.py:362–387,793–803,855–858,1531–1537` |
| Resposta, atos e carimbos | A porta do colaborador escreve `resposta_colab` e `respondida_em`. As portas automática e de reparo pela trilha também escrevem o par ou completam o instante. A reabertura registra o ato e descarimba pela porta do modelo. `_carimbar_validacao` escreve `validada_em` e seu ato. `PerguntaDisputa.carimbar` escreve o carimbo de materialização e a via, com preenchimento condicional dos instantes. `DecisaoPergunta.registrar` persiste os atos e `composta` fornece os instantes da sequência, o texto da coluna e os carimbos físicos. Leitores relevantes incluem C6, C21, lote, vigia, explicador e o materializador pela guarda de anuência. `app/chamados/services/disputa_emissao.py:1499–1514,1664–1684,1707–1737` · `app/chamados/services/validacao.py:161–175,1225` · `app/chamados/models.py:3259–3268,3324–3374,4348–4390,4428–4438` · `app/chamados/juizes.py:369–380,1427–1441,1462–1466,1589–1598` · `app/chamados/services/vigia_carimbo.py:97–107` · `app/chamados/services/explicador.py:98–123` · `app/chamados/services/materializacao.py:1332–1341` |
| Identidade, dia, destino e batidas | As portas de nascimento fornecem `tipo_resposta`, `motivo`, `data_evento`, `disputa` e `chamado_destino`. A costura grava o destino e a porta do fio grava o dia do fato. O slot os lê para a foto; o derivador lê também `disputa.contexto`, o marco do destino e a grade. O plantio escreve batida pela porta `registrar_batida`. O juiz e o vigia leem as vinculadas vivas, e o lote lê o universo de `batidas_apuraveis`. Para a prova histórica `retratar_pergunta_obsoleta`, localizei o leitor avulso e o da carga. Escritor dessa chave literal [nao verificado]. `app/chamados/services/disputa_emissao.py:448–461` · `app/chamados/services/fio.py:843–855,975–981` · `app/chamados/juizes.py:287–298,408–412,798–840` · `app/chamados/services/materializacao.py:556–592,615–645,691–710,1638–1651` · `app/ponto/registro_batida.py:118,170–180,287` · `app/ponto/turnos.py:2872–2897` · `app/chamados/services/carga_pergunta_viva.py:110–112` |
| Classe, detalhe e `porque` do lote | `delta_da_resposta` produz `resp`, `esperada`, `delta_min`, `ampm`, `sinal_min` e `tipo`. A tolerância vem da folha; a janela de conflito vem de `CONFLITO_LOTE_MIN`. `classe_do_lote` produz classe e detalhe. A fila carrega `detalhe`, mas executor e API consomem só a classe. A proposta e a prévia exibem detalhes da A e agregam as exclusões. O motivo novo existe no serviço e não chega à recusa exibida pelas portas verificadas. `app/chamados/juizes.py:1975–2007,2017–2023` · `app/chamados/services/validacao.py:1028–1042,1085–1087,1128,1147–1154,1204–1237,1279–1294` · `app/chamados/views_cobrar.py:289–300,345–370` · `app/api/views_mensageria.py:542–575` · `app/templates/chamados/partials/_lote_preview.html:24` |
| Saídas indiretas do juiz, do explicador e do vigia | O booleano de materialização alimenta C6, lavra, backfill, raio-X, emissão, explicador, vigia e ambos os caminhos do materializador. O explicador passa o texto ao item de `por_que_nao_fecha`. O vigia produz a tripla que o censo, o passivo e o vigia do fio leem. O passivo já exclui `slot_nao_derivavel` antes de usar `ts`. `app/chamados/juizes.py:1531–1538` · `app/chamados/veredito_lavrado.py:263` · `app/chamados/management/commands/backfill_veredito_lavrado.py:105` · `app/chamados/services/raiox.py:194` · `app/chamados/services/disputa_emissao.py:2052` · `app/chamados/services/materializacao.py:1971,2216` · `app/chamados/services/explicador.py:123,144–150,179` · `app/chamados/services/vigia_carimbo.py:107–134` · `app/chamados/services/passivo_validado.py:330–347` · `app/chamados/services/vigia_fio.py:415–418` |

**Regra antiga que sobrevive.** O parser de `data_hora` continua duplicado no materializador, que lê a coluna depois de passar pelo C21. A cópia já existia no lado removido do patch, e o aval proíbe tocar o materializador. Em `hora`, ambos delegam a `_ts_tipo_do_plantio`. As guardas antigas do lote ainda leem texto físico para escape, legibilidade e delta antes da guarda de vigência para A. A API ainda calcula os detalhes pela classificação anterior e substitui só a classe pela do lote. `app/chamados/juizes.py:389–412,1975–1991` · `app/chamados/services/materializacao.py:1395,1624–1639` · `app/chamados/services/validacao.py:1204–1229` · `app/api/views_mensageria.py:544–546,573–575` · `CLAUDE.md:20–22`

**Dois significados.** Nenhum campo persistido ganha uma segunda função no patch. `slots` conserva a união entre resultado e exceção. Há mistura de formatos em `detalhe['porque']`, com frases antigas e códigos novos, e `LOTE_B` passa a agrupar também indisponibilidade técnica. É essa última mistura que faz uma falha de banco parecer uma exclusão comum na porta. `app/chamados/services/carga_pergunta_viva.py:119–128` · `app/chamados/services/validacao.py:1085–1087,1204–1237`

## (c) `DatabaseError` e `atomic()`

Há três chamadores diretos de `classe_do_lote` na cópia:

- `validar_lote_classe_a`, acionado pela rota de lote, pela tela e pela confirmação da IA. A reclassificação precede `acao_validar_pergunta`, que chama o escritor. O `atomic()` e as travas do escritor vêm depois. `app/chamados/services/validacao.py:1074–1096,746–760` · `app/chamados/services/acoes_disputa.py:13–22` · `app/chamados/views_cobrar.py:234–263,373–385` · `app/core/views_ia.py:142–169,859–874`
- `fila_validar_classificada`, chamada pela proposta, contagem e prévia, em caminhos de leitura sem `atomic()` visível. `app/chamados/services/validacao.py:1272–1294` · `app/chamados/views_cobrar.py:277–300,345–370`
- `api_mensageria_validar`, GET que lê a fila e serializa a classe, sem `atomic()` visível. `app/api/views_mensageria.py:523–552`

**A hipótese de transação quebrada no fluxo normal verificado não procede.** O `atomic()` identificado protege a escrita posterior, e os dicionários PostgreSQL de produção e SaaS na cópia não configuram `ATOMIC_REQUESTS`. Configuração efetiva fora da cópia [nao verificado]. **Sob um `atomic()` externo**, se o Postgres abortar a transação durante o SELECT, o `except` não faz rollback nem delimita savepoint. O executor pode fazer `continue` e tentar outra consulta, ou devolver uma recusa genérica com a transação ainda abortada. `app/config/settings/prod.py:9–18` · `app/config/settings/saas.py:83–92` · `app/chamados/services/validacao.py:1082–1096,1230–1237`

**A captura é nova.** O lado removido do patch deixava a exceção subir. A regra de barrar a classe A quando não há confirmação de ausência de conflito é coerente com o aval. O fallback novo é converter falha de infraestrutura em classificação normal, com um motivo que os chamadores verificados descartam. Isso fere a disciplina de tratar na origem da LEI-AKITA 1. O teste só prova que um `DatabaseError` sintético vira B com algum motivo. Até trocar `CONSULTA_CONFLITO_INDISPONIVEL` por qualquer string não vazia deixa essa asserção satisfeita. `diff.patch`, hunk `app/chamados/services/validacao.py:1220` · `app/chamados/services/validacao.py:1085–1087,1230–1237` · `app/api/views_mensageria.py:546` · `app/chamados/tests/test_fila_validar_lote.py:110–126` · `CLAUDE.md:17–19`

## (d) Classes de reprovação

| Classe | Achado |
|---|---|
| Segundo juiz registrado | Zero novos no patch. O lote e o slot chamam o C21 registrado. São duas chamadas, com finalidades diferentes, ao mesmo predicado. `app/core/juizes.py:305` · `app/chamados/services/validacao.py:1225–1229` · `app/chamados/juizes.py:379–382` |
| Segundo escritor fora da porta | Nenhum novo escritor persistente nos hunks. A foto é local, o cache pertence à carga e a escrita do lote continua na porta. `app/chamados/juizes.py:408–415` · `app/chamados/services/carga_pergunta_viva.py:117–128` · `app/chamados/services/validacao.py:1094–1096` |
| Relógio fora da borda | Nenhuma leitura nova de relógio no slot. Permanece o `date.today()` preexistente da janela viva quando `hoje` não é fornecido. `app/chamados/juizes.py:360–415,625–634,828–835` |
| Derivação paralela | Sobrevive o parser de `data_hora` no materializador. A missão de `hora` segue delegada. `app/chamados/juizes.py:389–412` · `app/chamados/services/materializacao.py:1395,1627–1637` |
| Selo ou RED sem mordida | Falta a mordida do explicador e a dos dois atalhos sob `SEM_RESPOSTA_VIGENTE`. O teste de banco simulado deixa sobreviver qualquer motivo não vazio. O C8b morde, conforme (e). `app/chamados/tests/test_explicador_disputa.py:53–102` · `app/chamados/tests/test_marco_por_motivo.py:184–241` · `app/chamados/tests/test_fila_validar_lote.py:124–126` |
| Ordem de trava | O patch não altera a sequência de travas. A leitura de conflito continua anterior à seção que trava e escreve. Isolamento concorrente em produção [nao verificado]. `app/chamados/services/validacao.py:746–760,1085–1096,1230–1236` |
| Pouso antes do pré-requisito | Pouso efetivo [nao verificado]. Os pré-requisitos pendentes constam de (a). `aval.txt:1` · `CLAUDE.md:372–381` | [nao verificado]
| Campo com dois significados | Nenhum novo significado em coluna persistida. A mistura entre classe de negócio e indisponibilidade técnica está em `LOTE_B` e seu detalhe. `app/chamados/services/validacao.py:1128,1225–1237` |

## (e) Testes e mutantes [nao verificado]

| Teste | Mutante de produção e asserção | [nao verificado]
|---|---|
| `test_marco_por_motivo` | Marco errado, veto perdido ou empate vetado → valores de hora, tipo e motivo em `:79–109,126–146`. Reabrir o vínculo → `assertFalse` em `:200`; reabrir o destino → `assertFalse` em `:213`. Usar texto bruto desfeito → motivo e campos em `:227–241`. Antecipar o cache a ponto de relançar erro inelegível, perder o motivo armazenado ou trocar a instância → `:254–265`. Aceitar horizonte divergente ou consultar antes de rejeitá-lo → `:279–283`. Alvos em `app/chamados/juizes.py:362–415,793–840,1435–1436` e `app/chamados/services/carga_pergunta_viva.py:119–128`. Execução ORM desses mutantes [nao verificado]. |
| `test_e3_c8b_sem_banco` e `e3_c8b_apoio` | Confundir vínculo com prova no slot → falhou em `:396,401` com o mutante em `app/chamados/juizes.py:803`. Alterar o tipo armazenado → falhou em `:427` com o mutante em `app/chamados/services/carga_pergunta_viva.py:126`. A mesma asserção fixa D+1 e `S`; `:428–432` fixa veredito e leituras. O apoio carrega o contrato real por AST em `app/chamados/tests/e3_c8b_apoio.py:94–97,116–124`. |
| `test_e3_pos_suite_sem_banco` | Trocar o motivo do veto → falhou em `:115`. Excluir o empate → falhou em `:111`. O cache sintético e a igualdade de motivo e tipo cobrem `:117–120`. Alvos em `app/chamados/juizes.py:374–387,1435–1436`. |
| `test_hx_fallback_sem_lei` | Absolver com lei acusando, perder a absolvição quando a lei cala ou estender a janela ao dia morto → asserções em `:68–85`, sobre `app/chamados/juizes.py:822–840`. Remover o mock do slot fortalece o teste. Falta assertar que a fixture chegou como `PLANTIO_SEM_TS` antes dos controles. `app/chamados/tests/test_hx_fallback_sem_lei.py:40–45,61–85` |
| `test_o306_vigia_carimbo` e `test_s3f4_data_e_ancora_pela_autoridade` | Absolver a vinculada fora do slot ou contá-la na classe errada → `app/chamados/tests/test_o306_vigia_carimbo.py:111–121`; somas em `:149–161`. Divergir do tipo ou instante do plantio → igualdade em `app/chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:96–110`. Alvos em `app/chamados/juizes.py:408–415,798–804` e `app/chamados/services/vigia_carimbo.py:107–117`. Execução ORM [nao verificado]. |
| `test_fila_validar_lote` | Remover C21 com marco fixo → exige B e `SEM_RESPOSTA_VIGENTE` em `:88–99`. Admitir data impossível → exige B e `DATA_HORA_INVALIDA` em `:104–108`. Eliminar a consulta de conflito → `.exists()` uma vez em `:124`; aceitar batida próxima → B e `conflito` em `:70–77`. Alvos em `app/chamados/services/validacao.py:1225–1237`. A seleção de refs em `:128–133` usa uma discrepância de hora como exclusão; falta nela um caso novo sem slot. Execução ORM [nao verificado]. |
| `test_explicador_disputa` | **Falta** um caso que fique vermelho ao voltar a citar `p.resposta_colab` como vigente em `app/chamados/services/explicador.py:52–61`. O teste de resposta sem veredito já presente verifica C7 e fechamento. `app/chamados/tests/test_explicador_disputa.py:65–102,143–156` |

**C8b com `sem_veredito` fica VERMELHO.** Executei o caso original e ele passou. Zerei `celula.veredito` na testemunha em memória. Os dez subcasos falharam por zero chamadas ao juiz de fato, nas asserções de carga e avulso. Sem a decisão admin, o C6 retorna pelo degrau da resposta antes de chegar ao degrau 6. `app/chamados/tests/e3_c8b_apoio.py:190–205` · `app/chamados/tests/test_e3_c8b_sem_banco.py:369–401` · `app/chamados/juizes.py:1462–1478,1531–1534`

**Comparação com a base anexada.** O lado removido já barrava a vinculada no veto explícito e já deixava `data_hora` desfeita sem slot. Contra essa base, nesses casos escurece a asserção do **motivo**, além da cobertura do novo contrato. O destino encerrado tinha o atalho, e `hora` avulsa podia alimentar o plantio com texto cru. Nos hunks exibidos, não identifiquei remoção de asserção nem troca de expectativa fora do aval. A fixture noturna passa de par ilegal para par legal e conserva D+1, tipo e leituras. `diff.patch`, hunks `app/chamados/juizes.py:341,762` e `app/chamados/tests/test_e3_c8b_sem_banco.py:407,424` · `a

[truncado em 20000 de 21471 caracteres]

## 21/11 17:38 · sessao 14c8ec388ea2 · E4C-F5-CONSTROI · verificador · origem=Code

**CORRIGE.** A trava nova ordena por PK a principal e a irmã eleita nos dois laços. A validação pública ainda grava o par por um caminho que contorna essa trava, e a eleição ainda usa candidatas que ficam fora do conjunto travado. O teste concorrente prova o caso de duas chamadas ao helper, com a trava do colaborador serializando as duas. A ordem sob concorrência com a porta de batida continua sem prova. `materializacao.py:2128–2152,2202–2217` · `passivo_validado.py:379–388` · `validacao.py:743–760,786–803` · `test_e4c_f5_plantio_concorrente.py:52–113` [nao verificado]

### (a) Condições do aval

| Condição | Resultado |
|---|---|
| **(i) Ordem canônica, mesma transação, conjunto completo** | **Falta completar o conjunto.** A ordem existente é Colaborador → Disputa → Chamado → Pergunta, cada grupo por PK crescente. Os dois laços chamam a trava e plantam no mesmo `atomic`. A irmã única elegível pela composta entra mesmo com a projeção divergente. Porém, o helper lê todas as candidatas para decidir unicidade e só trava a irmã retornada. Se houver zero elegíveis ou ambiguidade, trava apenas a principal, salvo candidatas que também sejam principais do lote. As linhas de `DecisaoPergunta` são lidas por `prefetch` e ficam fora do `FOR UPDATE OF self`. `models.py:2875–2904` · `materializacao.py:148–154,2128–2152,2202–2217` · `passivo_validado.py:379–388` |
| **(ii) RED concorrente em ordem inversa, sem deadlock e sem dupla gravação** | **O cenário existe.** Dois workers começam por PKs opostos, sincronizam antes de adquirir o colaborador, exigem PIDs distintos e verificam o SQL do par, uma tentativa vencedora, duas batidas e uma trilha por batida. O worker descarta o mapa retornado pela trava e chama `_materializar_uma` diretamente. A passagem pelo fluxo completo, a execução do RED no commit indicado e a ausência de carimbo duplicado ficam **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:52–113` | [nao verificado]
| **(iii) Passivo pelo mesmo caminho e preservação do restante** | O passivo usa o mesmo helper e o mesmo contexto, e contabiliza a mudança detectada como recusa do grupo. Ainda usa `classe`, `ts`, dia, competência, `exportada` e a foto de reversão obtidos antes da trava. A releitura dentro da unidade só se aplica a motivos de intervalo e ocorre depois do agrupamento e da decisão de chamar a unidade. A sequência das quatro travas permanece no helper. `passivo_validado.py:333–393,443–466,537–555` · `materializacao.py:1919–1925` · `models.py:2875–2904` |

### (b) Leituras no ato e TOCTOU

| Sítio | Cobertura |
|---|---|
| `travar_o_ato_do_plantio` e `metade_irma_do_intervalo` | A eleição inicial ocorre antes da primeira trava. Depois, a principal é relida e a eleição é refeita. Mudança no escopo ou irmã eleita ausente dos PKs travados recusa. A segunda eleição ainda consulta candidatas e decisões fora do conjunto. O teste de ambiguidade demonstra esse limite: a terceira pergunta é lida depois da trava e só então recusada pelo mapa. `materializacao.py:139–154,2113–2152` · `test_e4c_f5_plantio_concorrente.py:156–168` | [nao verificado]
| `par_do_intervalo` pela guarda de espera e pelo plantio | O par aplica C21 à principal e às candidatas, confere a eleição, lê a validação composta da irmã e só então deriva e autoriza o par. No laço e no passivo, a trava cobre a principal e a irmã confirmada, com a ressalva sobre as demais candidatas. A guarda de espera chega pela chamada em `935`, e a tentativa de gravar a irmã chega pela chamada em `1773`. A chamada da tela ao mesmo derivador é leitura; a chamada pela guarda de escrita é parte do ato. `materializacao.py:193–245,784–796,876–901,931–950,1442–1447,1773–1785,1972` |
| `validar_pergunta` e `_validar_o_par` | A principal é carregada e julgada pela composta antes do `atomic`, assim como a implausibilidade e `_irma`. Depois da trava, `_irma` antiga ainda alimenta K6, e a principal antiga é carimbada sem repetir os gates. `_validar_o_par` elege uma irmã nova sob a trava do colaborador, mas o `FOR UPDATE` anterior recebeu os destinos/clones da irmã antiga e **nenhum PK de pergunta**. A irmã nova pode ser validada e plantada, e o caminho avulso também chama a unidade sem instalar o contexto. A varredura posterior ocorre depois dessas escritas. `validacao.py:693–744,746–760,786–806,648–659` · `models.py:2900–2903` |
| Anuência, retenção do destino/clones e fechamento | A anuência usa C21 composto na unidade. O fecho de cada destino/clone consulta outras perguntas creditadas ao chamado e lê suas compostas, sem incluir seus PKs no conjunto do plantio. A cauda consulta C6/C7 depois dos `atomic` de plantio. O ramo de ausência é outro ato: faz seleção inicial e releitura por candidata sob a trava do colaborador. `materializacao.py:1296–1319,1339–1347,2084–2105,2219–2220,1043–1075` · `juizes.py:2121–2126` · `models.py:2017–2035` |
| Espelho, escala/célula e competência | As consultas a batida existente, vizinhança e colisão, os insumos da escala/célula e K6/F6 também decidem a gravação. As quatro travas não incluem essas linhas. Para K6 há um escritor independente verificável: a exportação registra a entrega sem tomar a trava do colaborador. A releitura de K6 na validação pública reduz a janela; o laço e o passivo não repetem o K6 do dia do fato depois da escrita. `materializacao.py:109–117,216–244,799–858,1517–1536,1924–1928` · `juizes.py:238–250,2064–2084` · `validacao.py:794–797` · `folha/models.py:174–188` |

**Escopo da validação pública.** A leitura pura para exibir o estado cabe em fila. A leitura usada para escolher as travas e depois efetivamente validar e plantar pertence à condição 1.2, pela redação do aval sobre tudo o que o ato lê para decidir. A pré-checagem pode ficar onde está se a decisão e o conjunto forem confirmados sob a trava antes da escrita. `aval.txt:1` · `validacao.py:743–760,786–803` · `materializacao.py:904–907,931–950`

### (c) `ContextVar`

O único `set` é `usar_irmas_confirmadas_no_plantio`, chamado pelo laço da disputa e pelo passivo. `par_do_intervalo` lê a eleição por PK, e `_materializar_uma` usa a presença do mapa para ativar a releitura da principal. O `finally` restaura o token anterior inclusive em exceção. Nas chamadas síncronas examinadas, a variável é local ao contexto e o mapa fica restrito ao bloco. O helper, por si, aceita instalação fora de `atomic`. `materializacao.py:202–204,1829–1839,1919–1922,2216–2217` · `passivo_validado.py:385–388`

Há dois usos do estado, **eleição confirmada** e **sinal para reler a principal**. O `.get(p.pk)` também dá o mesmo resultado para chave ausente e irmã confirmada como `None`. Isso rejeita uma irmã concreta nos dois casos, mas o mapa sozinho não prova que o PK atual pertence ao ato. Nenhuma coluna persistida foi criada para esse estado. `materializacao.py:199–204,1829–1839,1919–1922,2144–2152`

### (d) Escritores, leitores e regra antiga

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| PK, disputa, colaborador da disputa, motivo, tipo e dia | A emissão cria a disputa com colaborador e pai e abre a pergunta com motivo, tipo e dia. `_nascer` insere a pergunta e o banco atribui o PK. A porta de re-chaveamento muda o dia. A eleição e a foto de trava leem esses valores, e o gate de data e o derivador voltam a usá-los. `disputa_emissao.py:601–606,837–860` · `models.py:3224–3234,3766–3776` · `fio.py:1032–1096` · `materializacao.py:142–154,168–175,631–641,2113–2117` |
| Pai, destino, clones e contexto | A emissão atribui pai, contexto e clones. Nascimento/reuso e `reapontar_resolvedora` atribuem destino. O mapa de trava lê os três vínculos como conjunto de PKs. Derivação, guarda de data, conciliação e trilha voltam a lê-los nos seus papéis próprios. O `disputa` passado pelo chamador pode ter sido carregado antes da trava; reler `p` não atualiza esse argumento independente. `disputa_emissao.py:576–606,848–859` · `models.py:3195–3232` · `fio.py:74–88,975–981` · `materializacao.py:631–639,1311–1319,1597–1608,1919–1925,2216–2217` |
| `respondida_em`, `resposta_colab`, `validada_em/por` e sequência | Resposta humana, automática e pela trilha escrevem a projeção e registram os atos. Validação assina; devolução e reabertura limpam; `carimbar` também pode preencher colunas vazias. `DecisaoPergunta.registrar` escreve a sequência e invalida o cache da instância recebida. A composta entrega instante e validação do ato, texto da projeção e carimbo/via da coluna. C21, eleição, validação da irmã, anuência e retenção consomem essa leitura. `disputa_emissao.py:1499–1537,1630–1684,1707–1737` · `validacao.py:161–175,269–296,656–657` · `models.py:3363–3374,4382–4390,4428–4453,2017–2027` · `materializacao.py:148–154,195–208,1339–1343` |
| `materializada_em/por`, `via_resolucao`, `pendencia_admin`, `recusa_motivo` e conferência de data | O modelo grava/desfaz carimbo e via. O ramo hora grava pendência com motivo, `_carimbar_recusa` grava motivo e anexa nota, e `conferir_data_do_fato` grava a conferência. Eleição, par, filtros do laço, passivo, testemunha e reversão os leem. `models.py:3324–3328,3394–3405,3416–3455` · `disputa_emissao.py:1682–1684` · `materializacao.py:149–153,205–206,886–900,1401–1407,1724–1748,1944–1946,2191–2215` · `validacao.py:1008–1016` · `passivo_validado.py:65–86,541–557` |
| Batidas, trilha e retorno do lote | `registrar_batida` cria ou reutiliza. `anotar` altera `pergunta_origem` e também é usado pelo flip e pela retratação. A unidade escreve a trilha de materialização; juízes, vigia e testes leem o espelho e o vínculo. O laço agrega `erros`/`materializadas` para a validação pública. O passivo escreve o balde `escopo_do_plantio_mudou`, e o comando imprime os baldes no placar. `registro_batida.py:118–181,255–301` · `flip_batida.py:25–33` · `materializacao.py:1583–1612,2204–2228` · `vigia_carimbo.py:97–116` · `validacao.py:803–805` · `passivo_validado.py:389–393` · `materializar_passivo_validado.py:241–249` | [nao verificado]

**Leitores antigos encontrados.** A fila inicial da disputa ainda usa as faces SQL do carimbo físico e `validada_em` físico. A releitura composta posterior não recupera uma principal excluída pela seleção inicial. O caso de carimbo de resposta apagado que passa como **irmã** pode ficar invisível como **principal**. O passivo também seleciona por `validada_em` físico. `_situacao_da_irma` já usa C21 composto, mas escolhe a primeira complementar por PK para diagnóstico, enquanto o plantio recusa ambiguidade. `materializacao.py:148–154,291–303,2186–2215` · `juizes.py:1573–1588` · `vigia_carimbo.py:94–110` · `test_bug_par_projetado.py:1384–1423`

### (e) Classes de reprovação

- **Juiz duplicado, derivação paralela e escritor fora da porta.** A eleição chama o C21 registrado sobre a composta. O par continua usando o derivador de hora existente, e as escritas continuam pelas portas de batida, validação e carimbo. O defeito é a segunda entrada para o mesmo escritor sem a mesma trava. `core/juizes.py:305` · `materializacao.py:145–154,223–231,1528–1536,1785,2039–2040` · `validacao.py:648–659,786–792`
- **Ordem com a batida.** Há um caminho inverso no código: `flip_automatico --apply` abre um `atomic` e faz `UPDATE` de Batida; o `post_save` pode entrar no reconciliador e pedir a trava do Colaborador. O plantio mantém essa trava e pode tentar `UPDATE` da mesma batida reutilizada para vincular `pergunta_origem`. Se o flip já tiver a batida e seu reconciliador precisar do colaborador, há espera circular possível. A materialização dupla do teste serializa no colaborador e não exercita esse cruzamento. Ocorrência em execução **[nao verificado]**. `flip_automatico.py:62–79` · `flip_batida.py:25–33` · `registro_batida.py:264–274` · `ponto/signals.py:206–215` · `reconciliador.py:635–646,1486–1487` · `materializacao.py:1413–1415,1583–1587,2139–2140` · `test_e4c_f5_plantio_concorrente.py:62–82`
- **Relógio e significado de campo.** A eleição e a composta não introduzem relógio. A aprovação e o carimbo ainda leem instantes em pontos distintos da escrita. O `hoje` do passivo vem da borda do comando. O ponto de dupla função introduzido é o mapa do contexto, descrito em (c). `materializacao.py:148–154,1535,1829–1839,1919–1922,2006` · `models.py:4357–4359` · `materializar_passivo_validado.py:85–89,103–114`
- **Pré-requisito de pouso.** O enunciado informa `OK, Ran 8`. A linha terminal e o RC desse GREEN, a suite inteira, os resultados dos REDs nos commits indicados e o hash/base Git estão **[nao verificado]**. A cópia é compatível com o formato de arquivo exportado descrito na pista, e a checagem Git local retornou ausência de repositório. `CLAUDE.md:119–123` · `app/docs/PISTA.md:72–86,102–117`

### (f) Mutantes

- **m1, m2 e m4** alteram produção. O anexo registra falha para a troca pela coluna crua, a omissão da irmã dos PKs e a remoção da guarda de mudança da irmã. m4 elimina a comparação e a checagem de pertencimento juntas; não demonstra a mordida independente de cada cláusula. `mutantes_def.txt:8–11` · `mutantes.txt:1–4` · `materializacao.py:153,2138,2149`
- **m3** altera só o SQL observado pela asserção; o banco recebe o SQL original. Prova a sensibilidade do regex ao texto `DESC`. Uma mutação real de `order_by('pk')` para `order_by('-pk')` **na query de Pergunta** deveria cair nesse mesmo regex, por leitura estática. Um mutante de produção sobrevivente nessa linha está **[nao verificado]**. Nas outras três tabelas, a ordem por PK pode ser retirada sem que a asserção de ordenação a examine. `mutantes_def.txt:10` · `test_e4c_f5_plantio_concorrente.py:62–70,92–98` · `models.py:2875–2903`
- **Faltam provas dirigidas** para o mutante de produção da ordem, para a guarda da principal, para o reset em exceção e para o caminho real do passivo e da validação pública. O teste concorrente principal ignora o valor retornado pela trava; os dois últimos testes exercitam o contexto diretamente. m5/m6 foram definidos, com resultado **[nao verificado]**. `mutantes_def.txt:8–13` · `materializacao.py:1833–1839,2144–2151` · `test_e4c_f5_plantio_concorrente.py:75–82,156–185` · `passivo_validado.py:385–388` · `validacao.py:786–794` [nao verificado]

### (g) Flakiness

A sincronização usa `Barrier` e `Event`, sem `sleep` e sem depender de qual worker vence. Cada worker fecha sua conexão em `finally`, e o executor espera os workers antes do teardown. Há risco de falso vermelho sob carga pelos limites de 4 s de lock, 10 s de sincronização e 25 s por `Future`. O `Future` estourar não cancela o worker, e a conexão principal do teste de reabertura não aplica os limites SQL do worker. O `except RuntimeError` também pode mascarar uma barreira rompida como recusa da aplicação. `test_e4c_f5_plantio_concorrente.py:35–41,52–104,115–144` [nao verificado]

O relógio da fixture é congelado e os instantes de resposta e reabertura são explícitos. A fixture cria seus próprios registros e o runner aponta para um banco de teste configurado. Interferência com outra execução que use o mesmo banco e estabilidade da classe na suite inteira ficam **[nao verificado]**. `test_e4c_f5_plantio_concorrente.py:18–33,133–142,170–176` · `test_bug_par_projetado.py:44–87` · `config/settings/ci.py:9–17` · `bin/suite.sh:129,324`

### Correções para o pouso [nao verificado]

1. **Unificar a trava do caminho que realmente grava.** Fazer a validação pública confirmar principal, irmã e vínculos sob a mesma trava antes de carimbar; cobrir as candidatas que decidem unicidade e explicitar a proteção das demais leituras decisórias. `validacao.py:693–760,786–803` · `materializacao.py:148–154,2113–2152`
2. **Fechar a janela das fotos antigas.** Revalidar sob a trava os dados do passivo que ainda decidem o trabalho e tratar a seleção inicial da principal pela coluna física, preservando a recusa com dono para instante sem texto. `passivo_validado.py:333–387,537–555` · `materializacao.py:2186–2217`
3. **Provar a ordem com a batida e a mordida do RED.** Cobrir o caminho real de plantio contra o flip em transação, executar o mutante de produção da ordem e comprovar os resultados pendentes com veredito terminal, `Ran` e RC. `flip_automatico.py:62–79` · `materializacao.py:1583–1587` · `test_e4c_f5_plantio_concorrente.py:52–113` · `mutantes_def.txt:10–13` · `CLAUDE.md:119–123`

## 21/11 17:30 · sessao 163ca4271953 · E4C-F5-CONSTROI · sonda · origem=Code

| Empresa | Contagem | Fonte |
|---|---:|---|
| `7:2` | 1 | `sonda/anexos:4` |
| `7:3` | 0 | `sonda/anexos:5` |
| `7:4` | 0 | `sonda/anexos:6` |

`SOMA	1	PAREI_SE_MAIOR_QUE	50` (`sonda/anexos:7`)

Erro ou traceback ausentes no trecho. Há três linhas de contagem. (`sonda/anexos:1–7`)

A soma de 1 não passa de 50. (`sonda/anexos:7`)

## 21/11 17:25 · sessao a4e8a30e5634 · E4C-F5-CONSTROI · construtor · origem=Code

**Cura aplicada na cópia E4C-F5-CONSTROI**, sobre `5cc451ac397216a84a70aaebafb326edc2cfbf37`. Aval `44f7c468aeb7` e rascunho `566c3b225f0c`, conforme `aval.txt:1`. Conferência independente dos hashes dos anexos [nao verificado].

Os dez casos do RED mantêm a mesma AST da base. Nas checagens locais, passaram os 28 testes do C8b, os 28 das duas catracas e o controle avulso de veto e empate. O mutante do C8b saiu com `rc 1`, com dez falhas de asserção por zero chamadas ao juiz de fato. O teste vizinho de hash legado teve dois erros de importação de `django.utils`, reproduzidos com a fonte da base. A rodada ORM, a suíte inteira e a certificação pelo Code e pelo opus final [nao verificado]. `red_tail.txt:1–31` · `chamados/tests/test_e3_c8b_sem_banco.py:369–433` · `chamados/tests/test_e3_pos_suite_sem_banco.py:103–120,163–181` · `chamados/tests/test_e4c_f5_selo_chamada.py:188–193,251–278` · `chamados/tests/test_e3_catraca_respondedores.py:544–556`

**Pouso pendente da sonda.** Contagem A → B por empresa e publicação no RELATO [nao verificado]. Se a soma passar de 50, o aval manda parar com a tabela antes do pouso. `aval.txt:1` [nao verificado]

### Diff por arquivo

| Arquivo e linhas | Alteração |
|---|---|
| [chamados/juizes.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/juizes.py:346), 346–415 e 793–858 | Resultado imutável com `motivo`, `ts` e `tipo`. A incompatibilidade de horizonte é recusada antes da composta e do cache. O veto precede o C21 e o cache. `hora` entrega ao plantio uma foto com o texto elegível ou vazio. O juiz de fato usa o motivo para bloquear os dois atalhos e conserva a retratação com carimbo e trilha. |
| [chamados/services/carga_pergunta_viva.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/carga_pergunta_viva.py:113), 113–145 | Guarda por PK o resultado completo ou a própria exceção. Só `SLOT` entra no cálculo dos limites. A busca por origem continua cobrindo as perguntas do lote. |
| [chamados/services/validacao.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/validacao.py:1196), 1196–1237 | Depois das guardas anteriores, A exige C21, `SLOT` e consulta de conflito concluída sem batida próxima. Usei `CONSULTA_CONFLITO_INDISPONIVEL` para `DatabaseError` e `BATIDA_PROXIMA` para o conflito confirmado, conservando `conflito=True`. |
| [chamados/services/explicador.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/explicador.py:48), 48–62 e 138–150 | Traduz o motivo do slot em texto para o admin. O ramo sem slot deixa de exibir a resposta física. Ações e donos continuam definidos em `explicar_pergunta`. |
| [chamados/services/vigia_carimbo.py](/home/ronald/wt-codex-e4c-f5-constroi-141636/app/chamados/services/vigia_carimbo.py:94), 94–133 | Alimenta as decisões por bloco de 500 e consome o motivo na foto atual. A tripla conserva `ts` para os dois resultados sem registro que têm slot. O censo usa as mesmas classes e somas. |

Nos testes existentes, cada ajuste corresponde a um consumidor do contrato anterior. `git diff --relative --unified=0` mostra os hunks da cópia. [nao verificado]

| Arquivo e linhas | Por que o teste precisava do ajuste | [nao verificado]
|---|---|
| `chamados/tests/test_marco_por_motivo.py:79–100` | Desempacotamento, igualdade do cache e comparações com `(None, None)` passam a ler o resultado. As testemunhas de 13h, cache e veto nos dois caminhos permanecem. |
| `chamados/tests/test_marco_por_motivo.py:108–109,126–127,131–142` | Empate, ato desfeito com marco fixo, saída de intervalo e formato legado passam a conferir os campos. Os horários e tipos exigidos foram preservados. |
| `chamados/tests/e3_c8b_apoio.py:6–16,94–97` | O harness já carrega funções reais por AST. Agora carrega também o tipo real que essas funções instanciam. |
| `chamados/tests/test_e3_c8b_sem_banco.py:410–411,427` | O noturno conserva o instante e a decisão admin da fixture para ter resposta legal e chegar ao juiz. O cache exige `SLOT`, D+1 e `S`. As asserções de leituras continuam em `:428–433`. |
| `chamados/tests/test_e3_pos_suite_sem_banco.py:114–120` | A entrada sintética do cache usa o tipo real. As duas consultas anteriores ao ato exigem `VETO_TEMPORAL`, e o empate exige `SLOT` com tipo `E`. |
| `chamados/tests/test_hx_fallback_sem_lei.py:41–45,61–85` | A fixture de `hora` sem texto já produz `PLANTIO_SEM_TS` pelo caminho real. Retirei o stub de par. Os três controles de destino, lei e janela continuam. |
| `chamados/tests/test_o306_vigia_carimbo.py:112–121` | O controle lê `.motivo` e `.ts` no lugar de desempacotar o par. A expectativa de batida fora do slot permanece. |
| `chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:104–110` | A igualdade com o plantio compara `ts` e `tipo`, com `SLOT` explícito. |

Os casos de `test_fila_validar_lote.py:79–126` já exigiam o contrato novo. Os controles de explicador existentes usam respostas válidas e mantêm as mesmas expectativas. A checagem local de texto residual e retratação usou os corpos reais no harness, em memória. `chamados/tests/test_explicador_disputa.py:53–102` · `chamados/tests/e3_c8b_apoio.py:94–124` · `chamados/services/explicador.py:48–62` · `chamados/juizes.py:287–298,793–795`

### REDs e comandos para o Code

| Caso | Sítio que satisfaz a asserção |
|---|---|
| a1 | O veto do ato posterior protege o juiz contra o vínculo das 08h. No empate, a batida ainda precisa comprovar o slot das 13h. `chamados/juizes.py:371–375,793–804` · `chamados/tests/test_marco_por_motivo.py:184–203` |
| a2 | A saída sob veto ocorre antes da leitura do destino e admite a testemunha independente de retratação. `chamados/juizes.py:793–795,822–840` · `chamados/tests/test_marco_por_motivo.py:205–217` |
| b1 | O C21 sobre o par projetado retorna `SEM_RESPOSTA_VIGENTE` antes do parser e do cache, com horizonte e na leitura avulsa. `chamados/juizes.py:368–393` · `chamados/tests/test_marco_por_motivo.py:219–228` |
| b2 | O texto desfeito chega vazio ao plantio. A autoridade existente ainda pode obter a hora do marco fixo. `chamados/juizes.py:379–415` · `chamados/services/materializacao.py:691–710` · `chamados/tests/test_marco_por_motivo.py:230–241` |
| c | A carga retém motivo e identidade da exceção. A guarda de `data_hora` ocorre antes do cache. O ramo que alcança o erro relança aquela instância. `chamados/services/carga_pergunta_viva.py:117–128` · `chamados/juizes.py:379–387,1531–1538` · `chamados/tests/test_marco_por_motivo.py:243–265` |
| d | A comparação de horizontes precede composta e cache. `chamados/juizes.py:360–373` · `chamados/tests/test_marco_por_motivo.py:267–283` |
| e1 | C21 tira de A a resposta desfeita mesmo quando o marco fixo pode fornecer slot. `chamados/services/validacao.py:1225–1229` · `chamados/tests/test_fila_validar_lote.py:79–99` |
| e2 | O parser devolve `DATA_HORA_INVALIDA`, que o lote carrega para B. `chamados/juizes.py:389–402` · `chamados/services/validacao.py:1227–1229` · `chamados/tests/test_fila_validar_lote.py:101–108` |
| e3 | A falha em `.exists()` leva a B com motivo. A consulta bem-sucedida sem conflito permite A. `chamados/services/validacao.py:1230–1237` · `chamados/tests/test_fila_validar_lote.py:110–126` |
| f | O par legal e o veredito admin levam as cinco testemunhas ao juiz real, nos dois caminhos. O RED exige a chamada antes de conferir o resultado. `chamados/tests/e3_c8b_apoio.py:189–208` · `chamados/tests/test_e3_c8b_sem_banco.py:369–402` |

Comandos a partir de `app/`, pela configuração do Code. O selo C6 conta `ausencias`, `trilha` e `batidas` para N=1 e N=4. No ORM, `decisoes` é pré-carregado na tarefa. A medição ORM do vigia está no comando `medir-vigia` abaixo [nao verificado]. `config/settings/ci.py:6–18,38–41` · `chamados/tests/test_e3_c8b_sem_banco.py:282–299` · `chamados/services/tarefa_do_dia.py:532–545` · `chamados/services/vigia_carimbo.py:103–106`

```bash
t() { PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci "$@"; }

t chamados.tests.test_marco_por_motivo chamados.tests.test_fila_validar_lote \
  chamados.tests.test_e3_c8b_sem_banco \
  -k test_RED_ -k test_testemunhas_de_materializacao_com_e_sem_carga

t chamados.tests.test_marco_por_motivo chamados.tests.test_e3_c8b_sem_banco \
  chamados.tests.test_e3_pos_suite_sem_banco chamados.tests.test_hx_fallback_sem_lei \
  chamados.tests.test_o306_vigia_carimbo chamados.tests.test_s3f4_data_e_ancora_pela_autoridade \
  chamados.tests.test_fila_validar_lote chamados.tests.test_explicador_disputa -k test_

t chamados.tests.test_juiz_le_retratacao -k retratada
t chamados -k chamados.tests

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_e4c_f5_selo_chamada chamados.tests.test_e3_catraca_respondedores
```

### Mutantes isolados

A mudança **(3)** zera só o veredito admin em memória. O `rc 1` local veio de `assert_called_once_with` nos dois caminhos, com zero chamadas em cada uma das cinco testemunhas. `chamados/tests/e3_c8b_apoio.py:189–204` · `chamados/tests/test_e3_c8b_sem_banco.py:377–402`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import unittest
from unittest.mock import patch
from chamados.tests import e3_c8b_apoio

original = e3_c8b_apoio.testemunha

def sem_veredito(*args, **kwargs):
    caso = original(*args, **kwargs)
    for pergunta in caso[2]:
        pergunta.celula.veredito = None
    return caso

nome = (
    'chamados.tests.test_e3_c8b_sem_banco.AlimentacaoC6.'
    'test_testemunhas_de_materializacao_com_e_sem_carga'
)
with patch.object(e3_c8b_apoio, 'testemunha', sem_veredito):
    resultado = unittest.TextTestRunner().run(unittest.defaultTestLoader.loadTestsFromName(nome))
raise SystemExit(0 if resultado.wasSuccessful() else 1)
PY
```

Os demais comandos recompilam um corpo por execução na memória do processo de teste. `c-ordem` deve expor o `ValueError` injetado no subteste que espera o motivo. `medir-vigia` verifica um SELECT de decisões por chamada ao censo para N=1 e N=2. `vigia` retira o prefetch sob essa mesma medição. Resultados pela porta [nao verificado]. `chamados/tests/test_marco_por_motivo.py:243–265` · `chamados/tests/test_fila_validar_lote.py:70–126` · `chamados/tests/test_o306_vigia_carimbo.py:100–121,149–162` · `chamados/services/vigia_carimbo.py:103–106`

```bash
mutante() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - "$1" <<'PY'
import inspect
import sys

import django
django.setup()

from django.core.management import execute_from_command_line
from chamados import juizes, models
from chamados.services import carga_pergunta_viva, explicador, validacao, vigia_carimbo

id_mutante = sys.argv[1]
marco = 'test_marco_por_motivo'
fila = 'test_fila_validar_lote'
vigia = 'test_o306_vigia_carimbo'
pm = '_pergunta_materializada_de_fato'
sl = '_slot_esperado'
lo = 'classe_do_lote'
veto = "        if slot.motivo in ('VETO_TEMPORAL', 'SEM_RESPOSTA_VIGENTE'):\n"
guarda = ("    if tipo_resposta == 'data_hora' and not vigente:\n"
          "        return _ResultadoSlot('SEM_RESPOSTA_VIGENTE')\n")
cache = ("    if carga is not None and p.pk in carga.slots:\n"
         "        resultado = carga.slots[p.pk]\n"
         "        if isinstance(resultado, Exception):\n"
         "            raise resultado\n"
         "        return resultado\n")
c21 = ("    if not pergunta_respondida(DecisaoPergunta.composta(perg)):\n"
       "        return LOTE_B, dict(d, porque='SEM_RESPOSTA_VIGENTE')\n")
exp = " % motivo\n"

casos = {
    'a1': (juizes, pm, veto, veto +
           "            if p.batidas_plantadas.filter(retratada_em__isnull=True).exists():\n"
           "                return True\n", marco, 'ato_posterior_nao_absolve'),
    'a2': (juizes, pm, veto, veto +
           "            if getattr(p.chamado_destino, 'status_local', None) in ENCERRADOS:\n"
           "                return True\n", marco, 'veto_temporal_nao_absolve'),
    'b1': (juizes, sl, guarda,
           "    if tipo_resposta == 'data_hora' and not vigente:\n        resposta = p.resposta_colab\n",
           marco, 'data_hora_desfeita_nao_fornece'),
    'b2': (juizes, sl, "resposta_colab=resposta if vigente else ''",
           'resposta_colab=p.resposta_colab', marco, 'hora_desfeita_depende'),
    'c-ordem': (juizes, sl, guarda + cache, cache + guarda, marco, 'nao_relanca_erro_cacheado'),
    'c-motivo': (carga_pergunta_viva, 'carga_da_vida', '                self.slots[p.pk] = slot\n',
                 "                self.slots[p.pk] = (type(slot)('TIPO_SEM_SLOT')\n"
                 "                                    if slot.motivo == 'SEM_RESPOSTA_VIGENTE' else slot)\n",
                 marco, 'nao_relanca_erro_cacheado'),
    'c-identidade': (juizes, sl, '            raise resultado\n',
                     '            raise type(resultado)(*resultado.args)\n',
                     marco, 'nao_relanca_erro_cacheado'),
    'd': (juizes, sl,
          "        elif agora != carga.agora:\n            raise ValueError('agora diverge do horizonte da carga C6')\n",
          '', marco, 'horizonte_divergente_e_recusado'),
    'e1': (validacao, lo, c21, '', fila, 'lote_sem_resposta_vigente'),
    'e2': (validacao, lo, 'return LOTE_B, dict(d, porque=slot.motivo)',
           'return LOTE_A, d', fila, 'lote_com_data_impossivel'),
    'e3': (validacao, lo, "return LOTE_B, dict(d, porque='CONSULTA_CONFLITO_INDISPONIVEL')",
           'return LOTE_A, d', fila, 'lote_sem_consulta_de_conflito'),
    'conflito': (validacao, lo, "return LOTE_B, dict(d, conflito=True, porque='BATIDA_PROXIMA')",
                 'return LOTE_A, d', fila, 'batida_perto_tira_do_lote'),
    'explicador': (explicador, '_explicar_validada', exp,
                   " % (motivo + repr(p.resposta_colab))\n", marco, 'data_hora_desfeita_nao_fornece'),
    'vigia': (vigia_carimbo, 'classificar', ".prefetch_related('decisoes')", '', vigia,
              'test_as_PARTES_SOMAM'),
}

if id_mutante == 'medir-vigia':
    arquivo, seletor = vigia, 'test_o_plantio_NO_slot_e_o_controle_POSITIVO'
else:
    modulo, nome, antes, depois, arquivo, seletor = casos[id_mutante]
    real = getattr(modulo, nome)
    fonte = inspect.getsource(real)
    assert fonte.count(antes) == 1, (id_mutante, nome)
    ns = {}
    exec(compile(fonte.replace(antes, depois, 1), inspect.getfile(real), 'exec'), vars(modulo), ns)
    setattr(modulo, nome, ns[nome])
    if getattr(models, nome, None) is real:
        setattr(models, nome, ns[nome])

if id_mutante == 'explicador':
    from chamados.tests.test_marco_por_motivo import MarcoPorMotivoTest
    reabrir = MarcoPorMotivoTest._registrar_reabertura_sem_limpar_colunas

    def com_explicacao(self, p):
        horizonte = reabrir(self, p)
        mensagem = explicador._explicar_validada(p, [])
        self.assertIn('sem resposta vigente', mensagem)
        self.assertNotIn(p.resposta_colab, mensagem)
        return horizonte

    MarcoPorMotivoTest._registrar_reabertura_sem_limpar_colunas = com_explicacao

args = ['manage.py', 'test', f'chamados.tests.{arquivo}', '-k', seletor]
if id_mutante in {'vigia', 'medir-vigia'}:
    from django.db import connection
    from django.test.utils import CaptureQueriesContext
    from chamados.tests.test_o306_vigia_carimbo import VigiaCarimboSemRegistroTest

    censo = VigiaCarimboSemRegistroTest._censo
    tabela = connection.ops.quote_name(models.DecisaoPergunta._meta.db_table)

    def com_medicao(self, *args, **kwargs):
        with CaptureQueriesContext(connection) as consultas:
            resultado = censo(self, *args, **kwargs)
        n = sum(tabela in q['sql'] and q['sql'].lstrip().upper().startswith('SELECT')
                for q in consultas.captured_queries)
        print(f"vigia N={resultado['universo']} SELECT decisoes={n}")
        self.assertEqual(n, 1)
        return resultado

    VigiaCarimboSemRegistroTest._censo = com_medicao
    if id_mutante == 'medir-vigia':
        args += ['-k', 'test_as_PARTES_SOMAM']

execute_from_command_line(args)
PY
}

mutante a1
mutante a2
mutante b1
mutante b2
mutante c-ordem
mutante c-motivo
mutante c-identidade
mutante d
mutante e1
mutante e2
mutante e3
mutante conflito
mutante explicador
mutante medir-vigia
mutante vigia
```

### Cinco perguntas por campo

| Campo | Fonte | Escritor | Instante ou borda | Leitores, inclusive fora do diff | Significado único |
|---|---|---|---|---|---|
| `motivo` | Composta, projeção e C21 para elegibilidade. Parser existente e plantio para derivação. `chamados/models.py:4382–4390` · `chamados/juizes.py:368–415` | `_slot_esperado` constrói o valor imutável. `chamados/juizes.py:346–357,374–415` | Veto antes de elegibilidade e cache. Com slot elegível, o resultado é emitido ao concluir a derivação. `chamados/juizes.py:360–415` | Juiz de fato, carga, lote, explicador e vigia. A tradução chega a `explicar_pergunta`. `chamados/juizes.py:793–808` · `chamados/services/carga_pergunta_viva.py:119–128` · `chamados/services/validacao.py:1227–1229` · `chamados/services/explicador.py:52–62,144–150` · `chamados/services/vigia_carimbo.py:110–117` | Uma condição do slot na foto consultada. Categoria sem unidade ou fuso. `SLOT` exige `ts` e `tipo`. Os demais motivos têm ambos vazios. `chamados/juizes.py:346–357` |
| `ts` | Data e hora declaradas em `data_hora`. Derivador do plantio em `hora`, com a cadeia do turno podendo chegar a D+1. `chamados/juizes.py:315–335,389–415` · `chamados/services/materializacao.py:691–710` | `_slot_esperado` escreve ou repassa o instante em `SLOT`. `chamados/juizes.py:399–415` | Após as guardas e ao concluir a derivação. Em cache, pertence à mesma foto. `chamados/juizes.py:362–415` · `chamados/services/carga_pergunta_viva.py:88,117–128` | Os cinco leitores diretos. Fora do diff, a busca de janela, a comparação da batida de origem e o passivo que deriva o dia do plantio. `chamados/juizes.py:240–284,796–858` · `chamados/services/carga_pergunta_viva.py:127–144` · `chamados/services/validacao.py:1230–1232` · `chamados/services/explicador.py:62–95` · `chamados/services/vigia_carimbo.py:111–117` · `chamados/services/passivo_validado.py:330–351` | `datetime` com fuso, criado no fuso ativo do Django, com padrão `America/Sao_Paulo`. As janelas pertencem aos leitores. Juiz e carga usam ±20 min inclusivos, conflito usa ±60 min inclusivos, explicador usa ±20 min inclusivos e contexto diário `[−6h, +30h)`. `config/settings/base.py:93–95` · `chamados/juizes.py:240–284,399` · `chamados/services/carga_pergunta_viva.py:141–145` · `chamados/services/validacao.py:1152,1230–1232` · `ponto/turnos.py:2890–2896` · `chamados/services/explicador.py:64–85` |
| `tipo` | `E` no ramo `data_hora`. Em `hora`, o tipo entregue pelo plantio. `chamados/juizes.py:399–415` · `chamados/services/materializacao.py:624–626,672–710` | `_slot_esperado` escreve no resultado, que a carga preserva inteiro. `chamados/juizes.py:402–415` · `chamados/services/carga_pergunta_viva.py:126–128` | Junto de `ts` na conclusão de `SLOT`. `chamados/juizes.py:346–357,399–415` | Juiz de fato, janela de batidas e explicador. `chamados/juizes.py:240–251,796–858` · `chamados/services/explicador.py:62,86–95` | `E` ou `S` da batida. Categoria sem unidade, fuso ou limite temporal. `chamados/juizes.py:240–251,402–415` |
| `carga.agora` | Argumento `agora` de `CargaDaTarefa`. `chamados/services/tarefa_do_dia.py:520–525,826–839` | `CargaDaTarefa` guarda o argumento e `CargaDaVida` o copia. `chamados/services/tarefa_do_dia.py:525` · `chamados/services/carga_pergunta_viva.py:83–88` | Abertura da carga, antes do primeiro slot. `chamados/services/carga_pergunta_viva.py:83–119` | `_slot_esperado`, juiz de fato e `pergunta_viva`. Fora do diff, a tarefa repassa o mesmo atributo a C6. `chamados/juizes.py:362–366,718–719,925–932` · `chamados/services/tarefa_do_dia.py:629–637` | Horizonte da foto em `datetime` com fuso. Ato posterior é excluído e o empate incluído. `chamados/juizes.py:1428–1441` |
| `carga.slots` | Resultado ou falha da leitura antecipada 

[truncado em 20000 de 28245 caracteres]

## 21/11 17:11 · sessao 744498d1387b · E4C-F5-CONSTROI · construtor · origem=Code

**E4C-F5-CONSTROI · 3.3 · volta 3.** A cura Y está na cópia pedida. O RED da base falhou no trio do carimbo e na contagem, conforme `red_tail.txt:7–30`. A sintaxe dos três arquivos, o `git diff --check` e a conferência local por AST passaram. GREEN, A2 em execução, mutante e medição SQL por verbo **[nao verificado]**. A certificação cabe ao Code e ao opus final. `app/chamados/reconciliador.py:644–661` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:200–247` · `app/ponto/tests/test_selo_chokepoint_escrita.py:205–209,232–236` · `bin/suite.sh:316–325`.

### Diff por arquivo

- **Reconciliador.** Depois da trava existente do colaborador, reaplica `qs_perguntas_absolviveis` ao PK e à disputa original, com prefetch de `decisoes`. A candidata que sai do universo é descartada. A que permanece passa pela conferência do dono e pelo veto C20 existente. A porta recebe a instância relida e consulta o C6. Só o carimbo gravado conta e pode enfileirar o fecho. `validada_em__isnull` continua definido uma vez no seletor. [reconciliador.py](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/reconciliador.py:644) · `app/chamados/reconciliador.py:681–703` · `app/chamados/models.py:3375–3383`.
- **A2.** A variante `celula_absolveu` agora exige zero chamadas à porta, trio vazio, contagem zero, `validada_em == AGORA`, ato `VALIDACAO`, par `(True, True)`, `excluida` com par `(False, True)` e evento processado. Parametrizei o helper. A variante invertida chama diretamente o consumidor real para ler `out['perguntas']`. O gêmeo órfão mantém a chamada pelo comando e as asserções originais. [test_e4c_f5_guarda_reconciliador.py](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:200) · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:268–274` · `app/chamados/reconciliador.py:1429,1465–1473`.
- **Selo.** Somente os tetos dos dois cenários de fecho foram ajustados, **234 → 236** e **298 → 300**. Cada chave anota `+1 SELECT` da releitura e `+1 SELECT` do prefetch, com o commit a preencher pela sessão. A projeção assume uma candidata relida e elegível por cenário. A medição pela porta e o delta de `INSERT`/`UPDATE`/`DELETE` estão **[nao verificado]**. Qualquer delta de escrita medido aciona o PAREI com o número. [test_selo_chokepoint_escrita.py](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/ponto/tests/test_selo_chokepoint_escrita.py:205) · `app/ponto/tests/test_selo_chokepoint_escrita.py:232–236,444–481` · `app/chamados/reconciliador.py:647–651` · `f5_chokepoint_teto_medido_2011_0110.txt:4–5`.

### RED, GREEN e mutante

O primeiro comando seleciona o RED e seu controle tanto na base quanto após a cura. O segundo roda os A2, incluindo o gêmeo órfão e as guardas AST. O terceiro roda o selo. `bin/suite.sh:197–209,316–325` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:115–167` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:239–294`.

```bash
WT=/home/ronald/wt-codex-e4c-f5-constroi-165618
BASE=daba956c8e4c33df62acb8de099fb5e09a85d536

PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_f5_e2_absolvicao [nao verificado]
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_A2_ [nao verificado]
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only ponto -- -k test_selo_chokepoint_escrita [nao verificado]
```

**Mutante `instancia_pre_trava`.** O bloco repõe temporariamente só o reconciliador da base, que pula a releitura e entrega `p` à porta. Mantém o A2 invertido e restaura o reconciliador da cura ao sair. O RED 3.3 deve falhar no trio e na contagem. O A2 invertido deve falhar na primeira asserção, que exige zero chamadas à porta. O controle sem intercalação deve passar. Execução **[nao verificado]**. `volta2_parei.txt` **[nao verificado: linha do anexo]** · `app/chamados/reconciliador.py:644–661` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:153–167` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:227–230`.

```bash
(
  set -e
  alvo="$WT/app/chamados/reconciliador.py"
  cura=$(cat "$alvo" && printf '.')
  base=$(git -C "$WT" show "$BASE:app/chamados/reconciliador.py" && printf '.')
  trap 'printf "%s" "${cura%.}" > "$alvo"' EXIT
  printf '%s' "${base%.}" > "$alvo"

  rc_red=0
  rc_a2=0
  PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_f5_e2_absolvicao || rc_red=$? [nao verificado]
  PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_A2_evento_celula_absolveu_exclui_validada_apos_selecao || rc_a2=$? [nao verificado]
  printf 'RED 3.3 rc=%d | A2 invertido rc=%d\n' "$rc_red" "$rc_a2"
)
```

`LEI-AKITA origem=retratar_perguntas_absolvidas_por_celula, testemunha=seletor unico relido sob a trava + C6 da porta sobre a fresca, RED=test_f5_e2_absolvicao_resposta_entre_prefetch_e_trava_preserva_pergunta_viva, quem-mais-le=consumidor do EventoCelula (subtotal) + cartório (carimbo antes do gate) + reconciliação do colaborador (subtotal) + vigia (emissões do gerador), juizes novos=0, padrao=Single Source of Truth`. `app/chamados/reconciliador.py:644–661,681–703,1429,1496–1506` · `app/chamados/models.py:3375–3383` · `app/core/juizes.py:290` · `app/ponto/services/cartorio.py:1018–1033` · `app/chamados/management/commands/reconciliar_chamados.py:89–93`.

### Autoconferência · campos=26

`R` = `app/chamados/reconciliador.py`, `M` = `app/chamados/models.py`, `J` = `app/chamados/juizes.py`, `T` = `app/chamados/tests/test_e2_pergunta_so_ao_c6.py`. Os caminhos `services/` nesta matriz são relativos a `app/chamados/`.

| Campo ou valor | (a) Fonte | (b) Escritor | (c) Instante | (d) Leitores | (e) Significado único |
|---|---|---|---|---|---|
| 1. Candidata e PK | Universo do seletor `R:681–703` | Nascimento `_nascer` `M:3224–3234` | Emissão antes da trava, releitura por PK e disputa sob a trava `R:636,644–651` | E2 e vigia `R:647–661`, `app/chamados/management/commands/reconciliar_chamados.py:91–93` | O PK identifica a pergunta. A releitura confirma a elegibilidade SQL `R:647–651,688–703` |
| 2. Colaborador, `_cid` e dono | Argumento e FK da disputa `R:635,689`, `M:1892–1896` | `_cid` local e vínculo da pergunta no nascimento `R:635`, `M:3229–3232` | Mesmo PK de trava, com conferência do dono na relação relida `R:644–653` | Trava, C20 e C6 `R:645,652–654`, `J:991–997,1478–1482` | A associação em memória usa o colaborador do mesmo PK e seu memo `R:647,652–653,689`, `J:1174–1195` |
| 3. Trava e registro do dono | Linha e registro transacional `M:2830–2848,2874–2878` | `trava_na_ordem` pela porta existente `M:2830–2839` | Colaborador primeiro no `atomic`, antes da releitura `R:644–649` | Guarda E2 e porta da resposta `R:652`, `services/disputa_emissao.py:1478–1485` | O registro identifica o dono já travado `services/trava_do_colaborador.py:29–47,55–63` |
| 4. `decisoes` | Sequência e complemento legado `M:4352–4379` | `DecisaoPergunta.registrar` `M:4428–4453` | Prefetch da instância entregue à porta, sob a trava `R:644–656` | Composta, C6 e C7 `M:4382–4390,2017–2027`, `J:1406–1409` | A sequência relida inclui o ato interposto no RED `T:131–147` |
| 5. `respondida_em` | Instante vigente recomposto do ato `M:4368–4390` | Portas de resposta e reabertura `services/disputa_emissao.py:1499–1537,1630–1737` | Composta lê os atos carregados sob a trava `R:644–656`, `J:1406–1409` | C6, C21 e C7 `J:1426–1428`, `M:2022–2027` | O instante representa a resposta vigente. A reabertura pode limpá-lo `M:4368–4379` |
| 6. `resposta_colab` | Texto da projeção `M:4382–4390` | Portas de resposta e reabertura `services/disputa_emissao.py:1499–1502,1630–1715` | Coluna relida sob a trava `R:644–649` | Composta, C21 e comparação com a luz `M:4388–4390`, `J:1359–1364,1426–1440` | Texto da resposta. No RED, `19:45` discorda da luz de `17:02` `T:137–146` |
| 7. `validada_em`, `validada_por_id` | Ato vigente e colunas físicas `M:3683–3689,4382–4399` | Validação escreve o ato. As colunas também passam pelas portas de devolução, resposta e carimbo no modo validar `services/validacao.py:161–175,284–296`, `services/disputa_emissao.py:1503–1514`, `M:3367–3374` | O seletor reaplica o filtro físico sob a trava. Para quem passa, os atos são carregados nesse ponto `R:644–651,691` | Seletor, C6, C7 e guarda do par `R:691`, `J:1408–1411`, `M:2022–2027,3384–3393` | Coluna nula delimita o universo SQL. A composta responde pela validação vigente. O A2 verifica a preservação de ambas `R:691`, `M:4385–4399`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:227–237` |
| 8. `celula_id` | FK do fato `M:2623–2625` | Nascimento e rechaveamento `M:3229–3232`, `services/fio.py:1087–1092` | FK e relação relidas sob a trava `R:644–649` | Seletor, C6 e outbox `R:692–702`, `J:1423–1425`, `M:3472–3496` | Identifica a célula vinculada. FK nula exclui a candidata `R:692` |
| 9. Veredito e via da célula | Lavra da célula `app/ponto/portas/celula.py:1032–1045` | `lavrar_veredito` `app/ponto/portas/celula.py:1090–1098` | Relação relida sob a trava `R:644–649` | Seletor, C6 e lastro `R:698–702`, `J:1423–1440,1458–1474`, `app/ponto/services/lastro.py:145` | Veredito registra o julgamento e via identifica a origem. O C6 interpreta a pendência `J:1423–1440,1458–1474` |
| 10. DNA, ata e lâmpada | DNA e ata da célula `J:935–953,1351–1360` | Portas da célula `app/ponto/portas/celula.py:294–306,1032–1067,1090–1098` | Célula relida sob a trava `R:644–649` | C6 e juiz do marco `J:935–975,1436–1440,1463–1474` | DNA dá o marco e ata dá a luz. Só `True` confirma o marco `J:935–953,1472–1474` |
| 11. `data_evento`, `conversa` | Dia e discriminador da pergunta `M:2633–2645` | `gravar_dia_do_fato` recebe o dia. O nascimento fixa `conversa` `services/fio.py:846–854`, `M:3004–3013,3224–3232` | Colunas relidas sob a trava `R:644–649` | C20, C6 e outbox `J:337–341,991–997,1475–1482`, `M:3472–3476` | O discriminador identifica quando o dia gravado é civil de conversa ou dia de turno `J:337–341`, `M:3004–3013` |
| 12. `motivo`, `tipo_resposta` | Colunas da pergunta `M:2631,2652` | Porta de nascimento `M:3035–3036,3229–3232` | Colunas relidas sob a trava `R:644–649` | C6, comparação com a luz e porta de resposta `J:927–946,1347–1352`, `services/disputa_emissao.py:1449–1473` | Motivo identifica a cobrança. Tipo identifica o formato da resposta `J:927–946,1347–1352` |
| 13. Destino e `status_local` | FK e estado do chamado `M:3691–3698`, `J:846–864` | FK pelas portas de nascimento/reuso e costura, com `SET_NULL` na exclusão. Estado por `_gravar_estado` `services/fio.py:857–876`, `M:213` | Relação relida sob a trava `R:644–649` | Memo C6 e outbox `J:846–864`, `M:3463–3470` | FK identifica a resolvedora quando presente. Estado pertence ao chamado `M:3691–3698`, `J:862–864` |
| 14. Veto C20 | Tranca e trilha de toque `J:979–997,1067–1085` | Portas de trancar e reabrir para a tranca. Veto calculado por leitura `app/ponto/services/fechamento.py:1400,1432`, `J:979–989` | Veto consultado também sob a trava com a pergunta relida e o memo existente `R:652–656,725–730`, `J:1101–1117` | Gerador, E2, C6 e C7 `R:654,729`, `J:1414`, `M:2022–2027` | Tranca aplicável barra o carimbo definitivo `R:654–655,725–729` |
| 15. C6 e `_pv_memo` | Juiz registrado sobre a composta `app/core/juizes.py:290`, `J:1406–1409` | Memo calculado pelo juiz por instância `J:895–908` | Gerador antes da trava. Porta julga a instância relida com o carimbo em memória `R:644–656,723`, `M:3361–3383` | Gerador, porta e C7 `R:723`, `M:3375–3383,2029–2035` | C6 decide vitalidade. O RED exige `RECUSA_C6` após a resposta interposta `T:137–157` |
| 16. Autor `_user` | Usuário `sistema` `app/core/utils.py:6–18` | Inicialização local pelo helper `R:633,638–639` | Carregado antes da trava `R:638–645` | Carimbo e fecho `R:656,673–675` | Autor das ações automatizadas do E2 `app/core/utils.py:6–18`, `R:656,673` |
| 17. Borda do relógio | `timezone.now()` das portas e horizonte do C6 `M:3361,2118`, `services/disputa_emissao.py:1494–1501`, `J:880–899,1383–1403` | Cada porta atribui o instante do próprio ato `M:3361–3366,2118–2122`, `services/disputa_emissao.py:1499–1501,1535–1537` | Resposta e carimbo sob a trava de seus atos `services/disputa_emissao.py:1478–1499`, `R:644–656` | Sequência, carimbo, janela viva e fecho `M:4363–4374,3361–3366,2118–2122`, `J:1475–1476` | Instante do ato, instante do carimbo e horizonte têm papéis próprios `J:1383–1403`, `M:3361–3366`, `T:137–147` |
| 18. Trio do carimbo | Colunas e via `VIA_CELULA_ABSOLVEU` `M:2694,3324–3328` | `_escrever_carimbo` pela porta `M:3324–3328,3365–3405` | Ausência de `materializada_em` reaplicada sob a trava. A porta salva após o C6 `R:644–656,690`, `M:3375–3405` | Seletor, composta, lastro e outbox `R:690`, `M:4388–4390,3468–3476`, `J:684–696` | Carimbo já existente exclui a candidata. Via e autor identificam o registro `R:690`, `M:3324–3328` |
| 19. `_gravou` e `recusa_do_carimbo` | Resultado da porta `M:3317–3322,3353–3405` | `carimbar` `M:3330–3405` | Julgamento da relida dentro do `atomic` `R:644–657`, `M:3375–3405` | Bool no E2. Motivo na materialização e no RED `R:656–659`, `services/materializacao.py:2222–2228`, `T:126–129,157` | Bool indica gravação. `RECUSA_C6` identifica a recusa por vitalidade `M:3375–3383,3405` |
| 20. Contagem `n` | Carimbos gravados `R:632,656–659,678` | Incremento local `R:659` | Após sair do `atomic` com sucesso da porta `R:644–659` | Evento e reconciliação `R:1429,1496–1506` | Exclusão ou recusa soma zero `R:650–659`, `T:153–163`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:227–230` |
| 21. Disputa, PK e `_disputas` | FK e mapa local `M:2626–2629`, `R:634,660–669` | Laço dos carimbos bem-sucedidos `R:657–661` | A disputa relida entra após a gravação `R:647–661` | Fecho e log `R:669–677` | PK deduplica a fila. Troca de disputa exclui a candidata desta passagem `R:647,660–669` |
| 22. `disputa.fechada_em` | Coluna da disputa `M:2118–2122,2335–2363` | Fecho e portas de reabertura `M:2118–2122,2312–2363` | Triagem pela relação relida sob a trava e nova leitura antes de C7 `R:647–661,669–672` | E2, porta de fecho e devolução `R:660–672`, `M:2061–2065`, `services/validacao.py:298–303` | `None` indica disputa aberta, inclusive após reabertura `M:2335–2363` |
| 23. Veredito C7 | Respostas sem veredito e `fio_vivo` `M:2022–2035` | `pode_fechar` calcula. `fechar` aplica a guarda `M:2029–2048,2071–2088` | Depois do laço de carimbos. A guarda da porta precede a trava do fecho `R:669–675`, `M:2071–2097` | E2 e porta de fecho `R:672–675`, `M:2071–2088` | Bool autoriza o fecho quando ambos os impedimentos estão ausentes `M:2029–2035` |
| 24. `fechada_por`, `resolucao`, `motivo_fechamento` | Autor, texto do E2 e motivo escolhido pela porta `R:673–675`, `M:2088,2118–2122` | `fechar` grava. Reaberturas limpam `M:2118–2122,2335–2363` | Transação própria do fecho, após o laço `R:669–675`, `M:2090–2097,2118–2122` | Sinal do pai e auditoria `M:2122–2126`, `app/chamados/signals.py:42–43` | Campos registram autor, texto e motivo do fecho `M:2119–2126` |
| 25. Foto da lavra | `veredito_lavrado`, `julgado_em`, `janela_expira_em` `app/chamados/veredito_lavrado.py:279–281` | Porta da lavra. O E2 usa o padrão `lavrar_veredito=False` `app/chamados/veredito_lavrado.py:270–275`, `M:3330,3394–3403`, `R:656` | Foto em memória após C6 e antes do save `M:3394–3403` | Restauração em falha e leitura do veredito lavrado `app/chamados/veredito_lavrado.py:284–289`, `M:2776–2805` | Vazio indica ausência de lavra. `indeterminado` é veredito próprio `M:2783–2798` |
| 26. Falhas e PK no log | Exceção por pergunta ou disputa `R:662–663,676–677` | `_log.exception` nos dois tratadores `R:663,677` | Após falha do ato correspondente `R:644–677` | O laço segue e retorna a contagem `R:662–678`. Leitores externos do log **[nao verificado]** | As mensagens distinguem falha de carimbo e de fecho pelo PK `R:663,677` |

## 21/11 17:11 · sessao 1454d3e22315 · FERIADO-POR-POSTO · verificador · origem=Code

**CORRIGE.** Os dois mutantes apontados na 2a agora caem. Ainda passa um atalho que ignora o juiz no renascimento 12x36. As docstrings também precisam explicitar que a herança sem marca vale para cargo não administrativo e que o desfazer só cria vínculo no ramo de renascimento. [test_feriado_admin_pelo_cargo.py:188](/home/ronald/codex_ws/68f08430f7a931da7c718954879dea5a721801c4/app/colaboradores/tests/test_feriado_admin_pelo_cargo.py:188) · [test_feriado_por_posto.py:751](/home/ronald/codex_ws/68f08430f7a931da7c718954879dea5a721801c4/app/colaboradores/tests/test_feriado_por_posto.py:751) · [feriado_do_posto.py:196](/home/ronald/codex_ws/68f08430f7a931da7c718954879dea5a721801c4/app/colaboradores/services/feriado_do_posto.py:196) · [vinculo.py:1135](/home/ronald/codex_ws/68f08430f7a931da7c718954879dea5a721801c4/app/colaboradores/services/vinculo.py:1135)

### (a) O que cada teste pega [nao verificado]

Executei em memória os corpos dos testes e das funções, com dublês para ORM, fechamento de vigência e auditoria. A execução Django, a suite e o resultado em produção ficam **[nao verificado]**. O critério de verde da obra exige veredito final, `Ran` e `rc`. [CLAUDE.md:119](/home/ronald/codex_ws/68f08430f7a931da7c718954879dea5a721801c4/CLAUDE.md:119)

| Caso | Mutante de produção | Resultado na sonda |
|---|---|---|
| **(1) FECHA 12x36** | Em `chave_ao_nascer`, devolver `False` diretamente para FECHA 12x36. `app/colaboradores/services/feriado_do_posto.py:197` | **VERMELHO** em `assertTrue(esp_fe.called)`. Antes do delta, passava pelo `assertIs(..., False)`. `app/colaboradores/tests/test_feriado_admin_pelo_cargo.py:186–189` |
| **(2) Desfazer 12x36** | Manter o ajuste e, só no desfazer 12x36, copiar `antigo.trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:1150–1151` | **VERMELHO** em `assertIs(renasce.trabalha_em_feriado, True)`. A herdada é `False`. `app/colaboradores/tests/test_feriado_por_posto.py:755–762` |
| **(2) Trilha do desfazer** | Manter a chave correta e suprimir `_nasce_pela_marca` para o 12x36. `app/colaboradores/services/vinculo.py:1101–1109` | **VERMELHO** em `assertEqual(len(self._trilha_da_chave(renasce)), 1)`, com `0 != 1`. `app/colaboradores/tests/test_feriado_por_posto.py:763` |
| **(2) Atalho que ainda passa** | Só no renascimento 12x36 não admin em OPERA, gravar `True` diretamente e manter a trilha. `app/colaboradores/services/vinculo.py:1145–1153` | **PASSA no teste novo**. PK, tipo, valor e contagem da trilha ficam corretos. Acrescentar ao desfazer a verificação da chamada ao juiz com o tipo 12x36, o posto e o cargo. `app/colaboradores/tests/test_feriado_por_posto.py:758–763` |

Os dois **renomes A3** corrigem a descrição do resultado, com as mesmas asserções. Ambos derrubam a marca OPERA prevalecendo sobre o cargo, mas passam se o respectivo copiador só herdar `False`, pois o admin da fixture já começa com `False`. Os testes preexistentes com admin e herdada `True` pegam essa regressão no ajuste e no desfazer. `app/colaboradores/tests/test_feriado_por_posto.py:695–701,714–721` · `app/colaboradores/tests/test_feriado_admin_pelo_cargo.py:145–149,160–172`

### (b) Relógio e isolamento

O teste novo tem relógio congelado na classe, usa `HOJE` fixo nas duas portas e calcula a volta a partir de `_INI`. Ele cria o próprio posto e vínculo, e `_te5b` cria o template se estiver ausente. `_Base._mat` é compartilhado, mas só fornece identificadores únicos à fixture. A trilha é filtrada pelo PK do vínculo que acabou de nascer. A leitura do código indica independência de ordem. `app/colaboradores/tests/test_feriado_por_posto.py:44,51–100,117–119,665–687,751–763`

### (c) Docstrings e aval

- **(2a).** O código dá `False` ao admin em qualquer posto e ciclo. Para não admin, marca declarada decide em qualquer ciclo, inclusive FECHA. Sem marca, o copiador herda. Em `padrao_trabalha_em_feriado` e `ajustar_vinculo_pelo_sistema`, explicitar **“sem marca, o não admin herda”**. O contraexemplo à frase sem essa ressalva é admin em posto sem marca, com herdada `True`, que nasce `False`. `app/colaboradores/services/feriado_do_posto.py:188–200` · `app/escala/models.py:913–928` · `app/colaboradores/services/vinculo.py:1113–1126`
- **`segue_a_marca`.** A docstring corrigiu a distinção. O predicado delimita os vínculos gravados elegíveis para propagação, A4 e sincronização. O nascimento chama `chave_ao_nascer` e aceita a marca em qualquer ciclo. `app/colaboradores/services/feriado_do_posto.py:175–200,212–231,343–350` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:50–61`
- **(2b).** `criar_trecho_retroativo` faz cópia fiel da chave, conforme o aval. Na docstring da classe A3, qualificar a frase “criam um vínculo NOVO”. Quando `volta == novo.data_inicio`, o desfazer remove o ajuste e reabre o antigo. O vínculo novo só nasce no outro ramo. `app/colaboradores/services/vinculo.py:978–985,1129–1153` · `app/colaboradores/tests/test_feriado_por_posto.py:667–671`

### (d) Delta e campos

Os cinco arquivos conferem com os identificadores de conteúdo do anexo. Reconstituí em memória os três arquivos de produção anteriores e confirmei os identificadores de ambos os lados. A árvore sintática é idêntica nos três quando se retiram as docstrings. A relação entre os commits `68f08430f` e `82ddde3bb` fica **[nao verificado]**. `app/colaboradores/services/feriado_do_posto.py:175–185` · `app/colaboradores/services/vinculo.py:1113–1126` · `app/escala/models.py:905–928`

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `Posto.opera_em_feriado` | `marcar_opera_em_feriado`, chamado por `aplicar_marca`. `app/colaboradores/services/estrutura.py:90–94` · `app/colaboradores/services/feriado_do_posto.py:306–322` | `chave_ao_nascer`, juiz do padrão, cobrança, presença, A4, plano e execução da marca, sincronização e descrição da trilha de nascimento. `app/colaboradores/services/feriado_do_posto.py:69,128,196,228,272,310–350` · `app/escala/models.py:922` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53` · `app/colaboradores/services/vinculo.py:1106` |
| `EscalaColaborador.trabalha_em_feriado` | `executar_vinculo` no cadastro novo, os dois copiadores do sistema no nascimento, `criar_trecho_retroativo` por cópia fiel e `marcar_trabalha_em_feriado` no vínculo gravado. `app/colaboradores/services/vinculo.py:287–299,896–904,978–985,1120–1126,1145–1153` | Copiadores leem a herdada. Trilha, A4, propagação e sincronização comparam valores. `feriado_vira_folga` lê para a grade e seus consumidores. O contador 5x2/6x1 e a prévia também leem. `app/colaboradores/services/vinculo.py:1101–1109,1125–1126,1150–1152` · `app/colaboradores/services/feriado_do_posto.py:228–231,283,349` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:62–67` · `app/escala/models.py:940,1400–1406` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:146–147` |
| Trilha `LogAuditoria` | `_nasce_pela_marca` envia `acao`, `modelo`, `objeto_id` e antes/depois. `core.observ.evento` persiste. `app/colaboradores/services/vinculo.py:1101–1109` · `app/core/auditoria.py:22–25` · `app/core/observ.py:133–138` | `_trilha_da_chave` filtra por vínculo, `editar` e a chave em `valor_antes`. O vigia de vigência também lê por modelo e ID, com o filtro amplo já registrado na triagem. `app/colaboradores/tests/test_feriado_por_posto.py:117–119` · `app/escala/services/vigencia_sem_trilha.py:43–57` |

Na fixture, `_posto` grava a marca e `_colab` grava cargo, tipo e chave herdada. No teste novo, o `create` do desfazer grava o tipo do antigo no novo vínculo, e as asserções leem `pk` e `tipo_escala_id` para distinguir o renascimento da reabertura. `app/colaboradores/tests/test_feriado_por_posto.py:82–100,760–761` · `app/colaboradores/services/vinculo.py:1135–1147`

## 21/11 16:59 · sessao c9a7fcd0786f · E4C-F5-CONSTROI · construtor · origem=Code

**PAREI na 3.3, com diff vazio.** O teste A2 `test_A2_evento_celula_absolveu_usa_foto_anterior_a_validacao` exige que o E2 carimbe com a foto anterior à trava mesmo depois de a validação ter sido gravada. A cura exige a decisão oposta. Para entregar a cura com a suíte coerente, é preciso ajustar esse teste, que está fora dos arquivos autorizados. [Teste A2](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:236) [nao verificado]

A fixture grava a validação entre a seleção e a trava do E2. A porta de validação preenche `validada_em` e registra o ato. O seletor canônico exige `validada_em IS NULL`, então a releitura sob a trava exclui a candidata. O A2 exige uma chamada bem-sucedida a `carimbar`, com a coluna nula na entrada e na saída. Mesmo se a releitura admitisse essa candidata, a instância fresca teria a coluna preenchida e contrariaria a asserção. [Intercalação](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:169) · [Validação](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/services/validacao.py:161) · [Seletor](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/reconciliador.py:678) · [Asserções do A2](/home/ronald/wt-codex-e4c-f5-constroi-165618/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:213)

O RED fornecido já morde no trio do carimbo e na contagem. Ele também exige `RECUSA_C6` após a resposta `19:45`, e o controle exige um carimbo de `sistema`. GREEN, A2 em execução, mutante e medição do selo **[nao verificado]**. `red_tail.txt:7–30`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:131–167`.

### Quem consome

| Consumidor | Efeito previsto |
|---|---|
| `EventoCelula` | `out['perguntas']` soma zero para a candidata excluída na releitura ou recusada pela porta. `app/chamados/reconciliador.py:643–651,1415–1419` |
| Cartório | Descarta o retorno do E2 e segue para `reconciliar()`. Uma pergunta preservada como viva pode barrar o fecho. `app/ponto/services/cartorio.py:1018–1033`, `app/chamados/models.py:3375–3383` |
| Reconciliação do colaborador | O subtotal do E2 pode cair e reduzir o total. As entradas são a batida e o evento do filho encerrado. `app/chamados/reconciliador.py:1482–1496`, `app/ponto/signals.py:206–215`, `app/chamados/signals.py:351–371` |
| Vigia `reconciliar_chamados` | Para os mesmos dados, segue contando as emissões do gerador para colaboradores sem evento pendente. A releitura proposta ficaria no escritor, depois da emissão. `app/chamados/management/commands/reconciliar_chamados.py:89–93,109`, `app/chamados/reconciliador.py:635–646,696–724` |

### Teto SQL projetado

Somente `registrar_batida (furo vivo, 1 chamado fecha)` subiria de **234 para 236** e `registrar_batida (furo vivo, 3 chamados fecham)` de **298 para 300**. A projeção atribui uma candidata elegível a cada cenário. Os dois `+1` abaixo são `LEITURA`, a anotar em cada chave com o commit `esta cura 3.3` a preencher pela sessão. O delta total por verbo **[nao verificado]**. `volta1_parei.txt`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,438–475`.

| Delta | Consulta proposta |
|---|---|
| `+1 SELECT` | `FROM chamados_perguntadisputa` com `INNER JOIN` em `chamados_disputasupervisao` e `escala_celuladia` e `LEFT OUTER JOIN` em `chamados_chamadocolaborador`. Filtra o PK da candidata, o `disputa_id` original, o dono e o universo do seletor. `app/chamados/reconciliador.py:678–703`, `app/chamados/models.py:2623–2630,3691–3698` |
| `+1 SELECT` | `FROM chamados_decisaopergunta WHERE pergunta_id IN (...) ORDER BY ato_em, id`, pelo prefetch `decisoes`. Ocorre quando a primeira consulta devolve a candidata. `app/chamados/reconciliador.py:702–703`, `app/chamados/models.py:4219–4221,4252–4253,4357–4361` |

### Matriz da cura proposta

As referências apontam para a base. `R` é `app/chamados/reconciliador.py`, `M` é `app/chamados/models.py`, `J` é `app/chamados/juizes.py` e `T` é `app/chamados/tests/test_e2_pergunta_so_ao_c6.py`.

| Campo | Fonte | Escritor | Instante com a cura | Leitores | Significado |
|---|---|---|---|---|---|
| 1. Candidata e PK | Seletor `R:678–693` | Nascimento `M:3016–3036` | Seleção antes da trava, releitura por PK e disputa original sob a trava `R:635–646` | Carimbo, fila, vigia `R:646–651`, `app/chamados/management/commands/reconciliar_chamados.py:91–93` | A primeira emissão indica elegibilidade inicial. A releitura decide se a candidata segue no universo `R:678–693` |
| 2. Dono da disputa | FK e argumento `M:1892–1896`, `R:634` | Vínculo no nascimento e `_cid` local `M:2626–2629,3035–3036`, `R:634` | Mesmo PK de trava. Dono conferido na disputa relida `R:643–646,679` | Guarda da trava, C20, C6 `R:645`, `J:991–997,1475–1482` | O mapa de identidade reutiliza o colaborador pelo mesmo PK `J:1174–1195` |
| 4. `decisoes` | Sequência e complemento legado `M:4352–4379` | `DecisaoPergunta.registrar` `M:4428–4453` | Prefetch da instância fresca sob a trava `R:643–646,702–703` | Composta, C6, C7 `M:4382–4390,2017–2027`, `J:1406–1409` | Inclui o ato interposto após a pré-seleção `T:131–147` |
| 5. `respondida_em` | Instante vigente recomposto `M:4368–4390` | Portas da resposta e reabertura `app/chamados/services/disputa_emissao.py:1499–1537,1630–1737` | Instante recomposto dos atos relidos sob a trava `R:643–646`, `J:1406–1409` | C6, C21, C7 `J:1426–1428`, `M:2022–2027` | Uma resposta desfeita pode resultar em `None` `M:4368–4379` |
| 6. `resposta_colab` | Texto da projeção `M:4382–4390` | Portas da resposta e reabertura `app/chamados/services/disputa_emissao.py:1499–1502,1630–1715` | Coluna relida sob a trava `R:643–646,702` | Composta, C21 e comparação com a luz `M:4388–4390`, `J:1359–1364,1426–1440` | No RED, `19:45` discorda da luz de `17:02` `T:85–87,137–157` |
| 7. `validada_em`, `validada_por_id` | Ato vigente e colunas `M:3683–3689,4382–4399` | Validação, devolução, resposta e carimbo no modo validar `app/chamados/services/validacao.py:161–175,284–296`, `app/chamados/services/disputa_emissao.py:1503–1514`, `M:3367–3374` | Filtro físico reaplicado sob a trava, com atos relidos para a porta `R:643–646,681,702–703` | Seletor, C6, C7 e guarda do par `R:681`, `J:1408–1411`, `M:2022–2027,3384–3393` | A validação interposta exclui a candidata. É a linha em conflito com o A2 `R:681`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:213–238` |
| 8. `celula_id` | FK do fato `M:2623–2625` | Nascimento e rechaveamento `M:3035–3036`, `app/chamados/services/fio.py:1059–1092` | FK e relação relidas sob a trava `R:643–646,682,702` | Seletor, C6, outbox `R:682–692`, `J:1423–1425`, `M:3472–3496` | FK nula exclui a candidata `R:682` |
| 9. Veredito e via da célula | Lavra da célula `app/ponto/portas/celula.py:1032–1045` | `lavrar_veredito` `app/ponto/portas/celula.py:1090–1098` | Relação relida sob a trava `R:643–646,688–703` | Seletor, C6 e lastro `R:688–692`, `J:1423–1440,1458–1474`, `app/ponto/services/lastro.py:145` | SQL delimita o universo e C6 interpreta a via e a pendência `R:688–692`, `J:1423–1440,1458–1474` |
| 10. DNA, ata e lâmpada | Célula `J:935–953,1351–1360` | Portas da célula `app/ponto/portas/celula.py:294–306,1032–1067,1090–1098` | Relação relida sob a trava `R:643–646,702` | C6 e juiz do marco `J:935–975,1436–1440,1463–1474` | Só `True` confirma o marco `J:1472–1474` |
| 11. `data_evento`, `conversa` | Colunas da pergunta `M:2633–2645` | Nascimento e rechaveamento `M:3004–3024`, `app/chamados/services/fio.py:1090` | Colunas relidas sob a trava `R:643–646,702` | C20, C6 e outbox `J:337–341,991–997,1475–1482`, `M:3472–3476` | O discriminador distingue o dia civil da conversa do dia de turno `J:337–341` |
| 12. `motivo`, `tipo_resposta` | Colunas da pergunta `M:2631,2652` | Nascimento `M:3016–3036` | Colunas relidas sob a trava `R:643–646,702` | C6, comparação com a luz e resposta `J:927–946,1347–1352`, `app/chamados/services/disputa_emissao.py:1449–1473` | Motivo identifica a cobrança e tipo identifica o formato da resposta `J:1347–1352` |
| 13. `chamado_destino`, `status_local` | FK e estado do destino `M:3691–3698`, `J:846–864` | Nascimento, recostura e porta de estado `M:213,3035–3036`, `app/chamados/services/fio.py:857–876` | FK e estado relidos sob a trava `R:643–646,702` | Memo do C6 e outbox `J:846–864`, `M:3463–3470` | A FK admite ausência de destino `M:3691–3698` |
| 14. Veto C20 | Tranca e trilha de toque `J:979–997,1067–1085` | Portas de trancar e reabrir `app/ponto/services/fechamento.py:1400,1432` | Veto consultado sob a trava com a pergunta fresca e o memo existente `R:643–646,715–720`, `J:1101–1117` | Gerador, C6 e C7 `R:719`, `J:1414`, `M:2022–2027` | Morte pela tranca veta o carimbo definitivo `R:715–720` |
| 15. C6 e `_pv_memo` | C6 registrado e composta `app/core/juizes.py:290`, `J:1406–1409` | Juiz calcula e guarda memo por instância `J:895–908` | C6 da porta recebe a instância fresca sob a trava com o carimbo em memória `R:643–646`, `M:3361–3383` | Gerador, porta e C7 `R:713`, `M:3375–3383,2029–2035` | O RED exige `RECUSA_C6` após a resposta interposta `T:137–157` |
| 18. Trio do carimbo | Colunas e via `M:3324–3328` | `_escrever_carimbo` pela porta `M:3324–3328,3365–3405` | Ausência conferida sob a trava antes da chamada `R:643–646,680` | Seletor, composta e outbox `R:680`, `M:4388–4390,3468–3476` | Carimbo já existente exclui a candidata `R:680` |
| 20. Contagem `n` | Carimbos gravados `R:646–649,668` | Incremento local `R:649` | Incremento após o `atomic`, condicionado ao sucesso da porta sobre a instância fresca `R:643–649` | Evento e reconciliação `R:1419,1486–1496` | Exclusão na releitura ou recusa soma zero `R:647–649,678–693`, `T:153–163` |
| 21. Disputa e fila `_disputas` | FK e mapa por PK `M:2626–2629`, `R:633,650–659` | Laço dos carimbos gravados `R:647–651` | Usa a disputa relida sob a trava, após sucesso `R:643–651` | Fecho e log `R:659–667` | PK deduplica. Troca de disputa exclui da passagem `R:635–651,678–679` |
| 22. `disputa.fechada_em` | Coluna da disputa `M:2118–2122,2335–2363` | Fecho e reabertura `M:2118–2122,2312–2363` | Triagem da fila pela disputa relida sob a trava, seguida da leitura antes de C7 `R:643–651,659–663` | E2, porta de fecho e devolução `R:650–663`, `M:2061–2065`, `app/chamados/services/validacao.py:298–303` | `None` indica disputa aberta, inclusive após reabertura `M:2335–2363` |

### Comandos para o Code

O primeiro seleciona o RED na base e o GREEN após a cura. O segundo seleciona o selo. O terceiro isola o A2 conflitante. `bin/suite.sh:197–209,316–325`.

```bash
WT=/home/ronald/wt-codex-e4c-f5-constroi-165618
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_e2_pergunta_so_ao_c6 [nao verificado]
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only ponto -- -k test_selo_chokepoint_escrita [nao verificado]
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_A2_evento_celula_absolveu_usa_foto_anterior_a_validacao [nao verificado]
```

**Mutante `instancia_pre_trava`, após a cura.** Com `fresca` como receptora nova e `p` como candidata original, trocar só o receptor e rodar o RED. A falha esperada é na asserção do trio e da contagem. Execução do mutante **[nao verificado]**. `app/chamados/reconciliador.py:635–646`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149–157`, `red_tail.txt:7–30`.

```bash
sed -i 's/_gravou = fresca\.carimbar(/_gravou = p.carimbar(/' "$WT/app/chamados/reconciliador.py"
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_e2_pergunta_so_ao_c6 [nao verificado]
sed -i 's/_gravou = p\.carimbar(/_gravou = fresca.carimbar(/' "$WT/app/chamados/reconciliador.py"
```

`LEI-AKITA origem=chamados/reconciliador.py::retratar_perguntas_absolvidas_por_celula, testemunha=C6 da porta de carimbo sobre a leitura fresca [nao verificado], RED=test_e2_pergunta_so_ao_c6, quem-mais-le=EventoCelula + cartório + reconciliação do colaborador + vigia do gerador, juizes novos=0, padrao=Single Source of Truth`. `app/chamados/reconciliador.py:635–651,1419,1486`, `app/chamados/models.py:3375–3383`, `app/core/juizes.py:290`, `app/ponto/services/cartorio.py:1025–1026`, `app/chamados/management/commands/reconciliar_chamados.py:91–93`.

## 21/11 16:57 · sessao b161a88f927a · FERIADO-POR-POSTO · verificador · origem=Code

**O nascimento nos dois copiadores atende ao item (2a) para os insumos que recebe.** Administrativo nasce `False`. Não administrativo em posto FECHA nasce `False`, e em posto OPERA nasce `True`, inclusive em 12x36. Os dois copiadores delegam esses casos ao juiz de `app/escala/models.py:905`. O trecho retroativo continua copiando a chave de origem, conforme (2b). `app/colaboradores/services/feriado_do_posto.py:187–199` · `app/colaboradores/services/vinculo.py:978–985,1124,1149`

Os hashes dos quatro arquivos locais batem com os quatro blobs de destino do anexo. Conferi a matriz em memória com as funções da cópia e o ramo anterior do patch. Ancestralidade dos três commits, execução Django, `green_v2` com veredito final, `Ran` e rc, e efeito observado em produção [nao verificado]. `app/colaboradores/services/feriado_do_posto.py:175–199` · `app/escala/models.py:905–928` · `CLAUDE.md:119–123`

### 1. Escritores e leitores

| Símbolo | Escrita ou definição e leitores fora de testes e migrations | [nao verificado]
|---|---|
| `EscalaColaborador.trabalha_em_feriado` | Gravações no item 5. Leituras diretas em `feriado_vira_folga` e no contador 5x2/6x1 (`app/escala/models.py:940,1401`), no A4 e no plano e rechecagem da propagação (`app/colaboradores/services/feriado_do_posto.py:227–231,282,348–349`), na sincronização administrativa e na saída do comando de marcação (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:62–71` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:144–147`), e na troca, cópia retroativa, cópias ESCALA-AUTO e trilha de nascimento (`app/colaboradores/services/vinculo.py:896–904,985,1101–1109,1124–1125,1149–1151`). `feriado_vira_folga` alimenta a escala calculada, a precedência do dia, o calculador e o fechamento (`app/escala/models.py:1380` · `app/ponto/precedencia.py:376–383` · `app/ponto/calculador/alimentacao.py:521` · `app/ponto/services/fechamento.py:949`). |
| `Posto.opera_em_feriado` | A marca explícita é gravada por `marcar_opera_em_feriado`, chamado por `aplicar_marca`. O cadastro de posto novo omite o campo nullable, e a edição genérica recusa a chave no formulário de dados (`app/colaboradores/services/estrutura.py:43–45,67–94` · `app/colaboradores/views.py:851–884` · `app/colaboradores/models.py:256–260`). Leitores da marca: veto de cobrança, presença/sugestões, seleção no nascimento, A4, plano, reaplicação e guardas de propagação/retratação (`app/colaboradores/services/feriado_do_posto.py:60–71,128,139,195,227,271,309,317,327,345–349` · `app/ponto/management/commands/detectar_ausencias.py:628–630`), juiz e contador 5x2/6x1 (`app/escala/models.py:922,943`), sincronizador (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:53`), trilha de nascimento (`app/colaboradores/services/vinculo.py:1106`) e saída do comando via plano (`app/colaboradores/management/commands/marcar_feriado_do_posto.py:132–135`). O Django admin de Posto também expõe o campo para leitura (`app/colaboradores/admin.py:32–44` · `app/core/admin.py:53–75`). |
| `chave_ao_nascer` | Definida em `app/colaboradores/services/feriado_do_posto.py:187`. Os dois chamadores são `ajustar_vinculo_pelo_sistema` e o ramo que insere um vínculo em `desfazer_ajuste_do_sistema`. Ambos persistem o retorno no novo PK. `app/colaboradores/services/vinculo.py:1119–1125,1144–1152` |
| `segue_a_marca` | Definida em `app/colaboradores/services/feriado_do_posto.py:175–184`. Chamadores atuais são `vinculos_que_seguem_a_marca`, que alimenta A4 e propagação, e `sincronizar_feriado_admin`. `app/colaboradores/services/feriado_do_posto.py:218,226,282,347` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:50–56` |
| `cargo_e_administrativo` | Definida em `app/colaboradores/management/commands/sincronizar_feriado_admin.py:4–21`. Chamadores são o próprio comando, `segue_a_marca`, `chave_ao_nascer` e o juiz. `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61` · `app/colaboradores/services/feriado_do_posto.py:184,196` · `app/escala/models.py:920` |
| `padrao_trabalha_em_feriado` | Definida em `app/escala/models.py:905–928`. Chamadores são a criação em `executar_vinculo`, `chave_ao_nascer` e o ramo administrativo do sincronizador. A função retorna o booleano. `app/colaboradores/services/vinculo.py:287–298` · `app/colaboradores/services/feriado_do_posto.py:196–199` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:59–61` |

Os chamadores externos dos copiadores estão no executor ESCALA-AUTO. O ajuste passa o tipo derivado. O desfazer que insere passa o tipo e a chave do vínculo antigo. Ambos recebem um vínculo com a chave conforme (2a). Quando `volta == novo.data_inicio`, o desfazer apaga o ajuste e reabre o PK antigo com a chave que já tinha, que pode divergir da marca. Esse ramo é anterior à cura e é uma reabertura. `app/escala/services/escala_auto_executor.py:101–108,143–148` · `app/colaboradores/services/vinculo.py:1124,1134–1152` · `app/docs/BACKLOG.md:1986`

`criar_trecho_retroativo` usa diretamente `ec.trabalha_em_feriado`, recebe os mesmos argumentos do comando e, para a mesma origem, grava o mesmo valor. Se a origem futura tiver nascido por um dos copiadores curados, o retroativo copiará fielmente a chave que encontrar nela. A cura prescinde de tocar esse sítio. `app/colaboradores/services/vinculo.py:978–985,1124,1149` · `app/colaboradores/management/commands/corrigir_escala_retroativa.py:194–197`

### 2. Antes e depois de `chave_ao_nascer`

Cada célula mostra **antes → depois**. A tabela usa o ramo anterior do anexo e o código atual. `5x2` representa os ciclos em `CICLOS_FERIADO_VIRA_FOLGA` e `12x36` os ciclos fora. `app/escala/models.py:26–34,919–928` · `app/colaboradores/services/feriado_do_posto.py:175–199`

| Cargo | Marca | Ciclo | Herdada `True` | Herdada `False` |
|---|---|---|---|---|
| Admin | `None` | 5x2 | F → F | F → F |
| Admin | `None` | 12x36 | F → F | F → F |
| Admin | `True` | 5x2 | F → F | F → F |
| Admin | `True` | 12x36 | F → F | F → F |
| Admin | `False` | 5x2 | F → F | F → F |
| Admin | `False` | 12x36 | F → F | F → F |
| Não admin | `None` | 5x2 | T → T | F → F |
| Não admin | `None` | 12x36 | T → T | F → F |
| Não admin | `True` | 5x2 | T → T | F → T |
| Não admin | `True` | 12x36 | T → T | **F → T** |
| Não admin | `False` | 5x2 | T → F | F → F |
| Não admin | `False` | 12x36 | **T → F** | F → F |

**Correção de leitura da tabela para 5x2:** em posto marcado o ramo anterior já usava a marca. Portanto, em OPERA com herdada `False` é **T → T**, e em FECHA com herdada `True` é **F → F**. Os únicos deltas são os dois em negrito do 12x36. Nenhuma saída atual contradiz (2a). FECHA e OPERA em 12x36 devolvem exatamente `False` e `True` do juiz de nascimento. No julgamento do dia, um plantão previsto 12x36 segue previsto quanto ao feriado com ambas as chaves, pois `feriado_vira_folga` exclui esse ciclo. O veto de cobrança do posto FECHA tem caminho próprio. `app/colaboradores/services/feriado_do_posto.py:175–199,60–71` · `app/escala/models.py:919–928,1380–1406` · `app/ponto/precedencia.py:376–387` · `app/ponto/management/commands/detectar_ausencias.py:616–630`

### 3. Trilha da troca

**A causa continua correta para a marca lida pelo copiador.** Quando o booleano muda, `_nasce_pela_marca` emite `editar` no PK novo, com antes/depois, cargo, marca, posto e origem. Isso passa a incluir 12x36 OPERA `False → True` e FECHA `True → False`. Se a chave coincide com a herdada, o nascimento pela marca fica sem linha de troca. A descrição registra os insumos e a chamada, e o escritor da auditoria é *best-effort*. Persistência de linhas concretas em produção [nao verificado]. `app/colaboradores/services/vinculo.py:1095–1109,1124–1125,1149–1152` · `app/core/auditoria.py:17–30` · `app/core/observ.py:94–105,133–139`

**Há leitores que podem ver linhas a mais.** A tela de auditoria lista e busca a descrição. O vigia `vigencia_sem_trilha` aceita qualquer linha com `modelo='EscalaColaborador'` por PK de vínculo ou de colaborador, então uma nova linha pode satisfazer o vigia mesmo sendo trilha da chave. Esse critério já existia, e a variação do contador em produção é [nao verificado]. O leitor de janelas de regeneração exige `acao='regenerar_celulas_vinculo'`, que exclui essas linhas `editar`. `app/core/views.py:705–728,733` · `app/templates/core/log_auditoria.html:56,73` · `app/escala/services/vigencia_sem_trilha.py:43–59,69–75` · `app/docs/BACKLOG.md:1986` · `app/escala/services/regeneracao.py:58–67`

### 4. Classes de reprovação

| Classe | Resultado |
|---|---|
| **(a) Segundo juiz registrado** | **Ausente na cura.** `chave_ao_nascer` delega a decisão. O registro de feriado/prazo e o lookup listam `fatos_do_dia` para supressão do trabalho do dia e `CICLOS_FERIADO_SIMPLES` para dobra. A decisão de nascimento de `:905` não tem entrada nesse bloco. `app/colaboradores/services/feriado_do_posto.py:194–199` · `app/escala/models.py:905–928` · `app/core/juizes.py:171–178,310–318` · `app/docs/JUIZES.md:114–118` |
| **(b) Segundo escritor fora das portas** | **Ausente no diff.** Os dois `INSERT`s seguem nas mesmas portas. Há uma regravação implícita preexistente em `executar_vinculo`, onde o `save()` integral do existente pode incluir a chave carregada. A alteração explícita do campo existente está em `marcar_trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:259–283,896–904,1119–1125,1144–1152` |
| **(c) Relógio fora da borda** | **Ausente na decisão nova.** O seletor e o juiz não leem o relógio, e o executor passa `hoje`. **Preexistente:** `sincronizar_feriado_admin --apply` chama a porta por vínculo sem `hoje`, e ela lê `localdate()` depois do `save`. Os signals de vigência e âncora também têm relógio próprio nos seus ramos de atualização, que pulam o `created`. `app/colaboradores/services/feriado_do_posto.py:194–199` · `app/escala/models.py:919–928` · `app/escala/services/escala_auto_executor.py:98–108,127–148` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:68–71` · `app/colaboradores/services/vinculo.py:899–908` · `app/escala/signals.py:54–74,111–126` |
| **(d) Derivação paralela** | **Ausente no nascimento curado.** Fora de `:905`, a marca é lida para detectar se foi declarada, sem predicado de ciclo nem cálculo da resposta. **Presente no caminho preexistente de manutenção:** A1/A2/A4 aplicam o recorte weekday, e o sincronizador ainda pode propor e gravar `True` para um não admin 12x36 ativo em FECHA quando alguém roda `--apply`. `app/colaboradores/services/feriado_do_posto.py:194–199,202–231,345–350` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:43–71` |
| **(f) Campo com dois significados** | **Ausente como sobrecarga nova.** `None` é marca ausente, `False` é FECHA, e a marcação recusa voltar a `None`. A chave gravada é um booleano sem proveniência embutida. Herdada e nascida são valores comparados na trilha. Uma leitura isolada da chave não distingue a origem, e se os valores coincidirem a trilha também não distingue. `app/colaboradores/models.py:256–260` · `app/colaboradores/services/feriado_do_posto.py:195–199,264–265` · `app/escala/models.py:896–899` · `app/colaboradores/services/vinculo.py:1095–1109` |

**(e) Mordida por teste.** Pela leitura, o RED novo reprova o retorno anterior em OPERA e FECHA 12x36 com chaves divergentes. O A3 invertido reprova o retorno anterior no ajuste para 12x36 OPERA. Execução dos mutantes e RED/GREEN Django [nao verificado]. `app/colaboradores/tests/test_feriado_admin_pelo_cargo.py:174–188` · `app/colaboradores/tests/test_feriado_por_posto.py:733–749`

| Teste novo ou alterado | Mutante que esse teste deixaria passar | [nao verificado]
|---|---|
| RED do não admin com marca | Devolver `False` diretamente para FECHA 12x36, sem consultar o juiz. O espião envolve só OPERA. `app/colaboradores/tests/test_feriado_admin_pelo_cargo.py:182–188` |
| Guarda do não admin sem marca | Ignorar a herdada e devolver `True` para 5x2 e `False` para 12x36. São os únicos pares exercitados ali. `app/colaboradores/tests/test_feriado_admin_pelo_cargo.py:190–199` |
| A3 12x36 com marca e desfazer sem marca | Corrigir o ajuste e deixar o desfazer herdar quando renasce um 12x36 marcado. O braço de desfazer do teste usa 5x2 sem marca. Também não há asserção de trilha no braço 12x36 marcado. `app/colaboradores/tests/test_feriado_por_posto.py:733–749` |
| A3 da elegibilidade do vínculo que nasce, com comentário alterado | Responder `True` diretamente para OPERA/5x2 sem chamar o juiz. O teste só compara o booleano do novo vínculo. `app/colaboradores/tests/test_feriado_por_posto.py:764–771` |

### 5. Vínculos já gravados

**Pelos caminhos da cópia, o diff não muda sozinho o booleano de um PK existente.** Só os dois caminhos de inserção do ESCALA-AUTO passam a gravar valores diferentes. Fechar ou reabrir o PK anterior escreve vigência. O comando que aciona os copiadores tem `--apply` com aval ou `--desfazer` explícito, e consta como fora de cron. `app/colaboradores/services/vinculo.py:643,1117–1125,1134–1152` · `app/escala/management/commands/ajustar_escalas_auto.py:31–48,93–95` · `app/config/crons.py:1194–1196`

| Caminho que grava a chave | Efeito do diff |
|---|---|
| `executar_vinculo` cria pelo juiz ou salva o existente com seu valor carregado. | Mesmo comportamento. `app/colaboradores/services/vinculo.py:259–298` |
| `criar_trecho_retroativo` insere inativo com a chave da origem. | Mesmo comportamento. `app/colaboradores/services/vinculo.py:978–985` |
| Ajuste e desfazer com nascimento inserem pelo seletor. | Mudam nas duas combinações destacadas de ciclo fora. `app/colaboradores/services/vinculo.py:1119–1125,1144–1152` · `app/colaboradores/services/feriado_do_posto.py:194–199` |
| `marcar_trabalha_em_feriado` atualiza existente, chamado por aplicar/reaplicar marca ou pelo sincronizador com `--apply`. O comando de marcação exige `--apply` e motivo para persistir. A prévia usa rollback na sombra. | Mesmo comportamento. `app/colaboradores/services/vinculo.py:896–904` · `app/colaboradores/services/feriado_do_posto.py:341–357,374–404` · `app/colaboradores/management/commands/marcar_feriado_do_posto.py:119–126` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:68–71` |

Os signals tratam vigência, âncora, invalidação de previsto, disputas e folgas. A regeneração e o gerador escrevem células, e o cron de feriado roda em `--alarme`. Nenhum desses caminhos recalcula ou grava a chave do vínculo existente. Configuração e execução efetivas em produção [nao verificado]. `app/escala/signals.py:18–35,67–83,111–146,154–205` · `app/ponto/portas/celula.py:289–306,911` · `app/config/crons.py:426,608` · `app/ponto/management/commands/reavaliar_ausencias_feriado.py:35–46`

### 6. Órfãos

- **Docstring do juiz.** Ainda diz que o copiador pergunta no ramo administrativo e resolve o restante fora. Com marca `True` ou `False`, o copiador agora também pergunta. `app/escala/models.py:915–918` · `app/colaboradores/services/feriado_do_posto.py:194–199`
- **Docstring de `segue_a_marca`.** Ainda inclui vínculo “a nascer” no predicado weekday. O recorte continua correto para propagação, A4 e sincronização, mas a menção ao nascimento ficou órfã. `app/colaboradores/services/feriado_do_posto.py:175–199,202–218` · `app/colaboradores/management/commands/sincronizar_feriado_admin.py:50–56`
- **Docstring de `ajustar_vinculo_pelo_sistema`.** “Mesmo […] feriado” é enganoso se lido como a mesma chave. A chave do novo PK pode mudar pela marca ou pelo cargo. `app/colaboradores/services/vinculo.py:1113–1125`
- **A3.** A abertura da classe ainda diz que os dois copiadores criam copiando a chave do velho. Os nomes `test_A3_ajustar_admin_e_sem_marca_copiam` e `test_A3_desfazer_admin_copia` ainda chamam o `False` administrativo de cópia. As fixtures têm herdada `False`, por isso o resultado coincide. O título e a mensagem do teste de “elegibilidade” também perderam a distinção por ciclo que pretendiam testar. `app/colaboradores/tests/test_feriado_por_posto.py:667–673,695–701,714–721,764–771`
- **Help e ocorrências restantes.** O help do campo resume `False` como folga sem falta, embora a própria frase limite o efeito aos ciclos weekday. O help do A4 omite esse recorte e a vigência, que o corpo aplica. São imprecisões preexistentes. Os usos de `CICLOS_FERIADO_VIRA_FOLGA` no julgamento diário e na propagação continuam corretos. “A cópia segue” em posto sem marca e a citação da caracterização antiga no A3 também continuam corretas nos respectivos contextos. `app/escala/models.py:896–899,1393–1406` · `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:1–4,12` · `app/colaboradores/services/feriado_do_posto.py:202–218` · `app/colaboradores/tests/test_feriado_admin_pelo_cargo.py:190–199` · `app/colaboradores/tests/test_feriado_por_posto.py:733–735`

## 21/11 16:53 · sessao b6538cac24a1 · E4C-F5-CONSTROI · construtor · origem=Code

**PAREI no teto SQL da 3.3. Diff vazio nesta volta.** A releitura proposta acrescenta **2 `SELECT`s por candidata que segue elegível**: um para a pergunta com as relações e outro para `decisoes`. Se a candidata saiu do universo, custa **1 `SELECT`**, sem carregar os atos. O cartório chama o E2 dentro da batida medida. O selo documenta um carimbo E2 em cada cenário de fechamento e fixa os tetos na contagem medida. Com a releitura, a projeção é **234 → 236** para um furo e **298 → 300** para três. A execução do selo nesta base e o GREEN pós-cura estão **[nao verificado]**. `app/chamados/reconciliador.py:643–651,678–703`, `app/ponto/services/cartorio.py:1018–1026`, `app/ponto/tests/test_selo_chokepoint_escrita.py:196–206,220–230,319–330,438–475`.

### Diff previsto e leitores

Logo após a trava existente, a cura reaplicaria `qs_perguntas_absolviveis` ao PK da candidata e ao `disputa_id` original. Se a linha não casar, o laço segue sem carimbar, contar ou enfileirar. A instância fresca traz `disputa`, `celula`, `chamado_destino` e `decisoes`. O dono é conferido nessa instância, o veto C20 existente é consultado e `fresca.carimbar(...)` entrega o julgamento ao C6. A fila de fecho recebe a disputa fresca dos carimbos gravados. `app/chamados/reconciliador.py:634–668,671–724`, `app/chamados/models.py:2830–2848,3344–3350,3375–3405`.

| Leitor | Efeito previsto com a cura |
|---|---|
| Consumidor de `EventoCelula` | Soma um carimbo a menos em `out['perguntas']` se a candidata saiu do universo ou se a porta recusou. `app/chamados/reconciliador.py:643–651,1416–1419` |
| Cartório | Descarta o retorno do E2. O gate que roda em seguida pode encontrar a pergunta ainda viva quando o C6 recusar o carimbo. `app/ponto/services/cartorio.py:1018–1033,1556–1562`, `app/chamados/models.py:3375–3383` |
| Reconciliação do colaborador | O subtotal e o total podem cair pelo mesmo motivo. As entradas incluem o sinal da batida e o evento do filho encerrado. `app/chamados/reconciliador.py:1482–1496`, `app/ponto/signals.py:206–215`, `app/chamados/signals.py:354–371` |
| Vigia `reconciliar_chamados` | Conta as emissões do gerador para colaboradores sem evento pendente. A mudança proposta fica no escritor após a emissão, então o valor do vigia para os mesmos dados segue igual. `app/chamados/management/commands/reconciliar_chamados.py:89–93,109`, `app/chamados/reconciliador.py:635–646,696–724` |

### RED, comandos e mutante

O RED fornecido falhou no trio do carimbo e na contagem. O teste também exige a passagem pela porta real e `RECUSA_C6` depois da resposta `19:45`. O controle exige um carimbo de `sistema`. `red_tail.txt:7–30`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:115–167`.

Comandos para o Code. O primeiro roda o RED na base atual e é o comando do GREEN após a cura. O segundo mede o selo. `bin/suite.sh:197–209,316–325`.

```bash
WT=/home/ronald/wt-codex-e4c-f5-constroi-161213
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_e2_pergunta_so_ao_c6 [nao verificado]
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only ponto -- -k test_selo_chokepoint_escrita [nao verificado]
```

**Mutante `instancia_pre_trava`.** Quando a cura existir com o receptor `fresca`, trocar só esse receptor por `p`. A base já chama `p.carimbar`, e o RED fornecido mordeu essa forma na asserção da linha 155. A rodada do mutante após a cura está **[nao verificado]**. `app/chamados/reconciliador.py:635,643–646`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149–167`, `red_tail.txt:7–30`.

```bash
sed -i 's/_gravou = fresca\.carimbar(/_gravou = p.carimbar(/' "$WT/app/chamados/reconciliador.py"
PYTHONDONTWRITEBYTECODE=1 "$WT/bin/suite.sh" --dir "$WT" --only chamados -- -k test_e2_pergunta_so_ao_c6 [nao verificado]
```

`LEI-AKITA origem=chamados/reconciliador.py::retratar_perguntas_absolvidas_por_celula, testemunha=C6 da porta de carimbo sobre a leitura fresca, RED=test_e2_pergunta_so_ao_c6, quem-mais-le=EventoCelula + cartório + reconciliação do colaborador + vigia do gerador, juizes novos=0, padrao=Single Source of Truth`. `app/chamados/reconciliador.py:635–651,1419,1486`, `app/chamados/models.py:3375–3383`, `app/core/juizes.py:290`, `app/ponto/services/cartorio.py:1025–1026`, `app/chamados/management/commands/reconciliar_chamados.py:91–93`.

### Matriz da cura proposta

`R` = `app/chamados/reconciliador.py`, `M` = `app/chamados/models.py`, `J` = `app/chamados/juizes.py`, `T` = `app/chamados/tests/test_e2_pergunta_so_ao_c6.py`. Os instantes abaixo descrevem a proposta parada no teto. As referências apontam para o código da base.

| Campo | Fonte | Escritor | Instante proposto | Leitores | Significado |
|---|---|---|---|---|---|
| 1. Candidata e PK | Linha do seletor `R:678–693` | Nascimento `M:3016–3036` | Pré-seleção antes da trava. Releitura por PK e disputa original sob a trava `R:635–646` | Carimbo, fila e vigia `R:646–651`, `reconciliar_chamados.py:91–93` | Emissão indica elegibilidade inicial. A releitura decide se a linha ainda pertence ao universo `R:678–693,713–724` |
| 2. Dono da disputa | Argumento e FK `R:634,679`, `M:1892` | `_cid` local e vínculo ao nascer `R:634`, `M:2626–2629,3035–3036` | Mesmo PK de trava. Conferência na disputa relida sob a trava `R:643–646` | Exigência, C20, C6 `R:645`, `J:996–997,1475–1482` | O mapa de identidade reutiliza o colaborador do mesmo PK `J:1174–1195` |
| 4. `decisoes` | Sequência e complemento legado `M:4355–4367` | `DecisaoPergunta.registrar` `M:4428–4453` | Segundo prefetch sob a trava, na instância entregue `R:643–646,702–703` | Composta, C6 e fecho `M:4382–4390,2017–2027`, `J:1406–1409` | A sequência pode conter o ato gravado depois da pré-seleção `T:131–147` |
| 5. `respondida_em` | Instante vigente do ato `M:4368–4390` | Portas de resposta e reabertura `services/disputa_emissao.py:1499–1537,1630–1737` | C6 recebe o instante recomposto dos atos lidos sob a trava `R:643–646`, `J:1406–1409` | C6/C21 e C7 `J:1426–1428`, `M:2022–2027` | Pode estar vazio se a resposta foi desfeita `M:4368–4379` |
| 6. `resposta_colab` | Texto da projeção `M:4382–4390` | Portas de resposta e reabertura `services/disputa_emissao.py:1499–1502,1630–1715` | Coluna relida sob a trava `R:643–646,702–703` | Composta, C21 e comparação com a luz `M:4388–4390`, `J:1359–1364,1426–1440` | No RED, `19:45` discorda da luz de `17:02` `T:85–87,137–157` |
| 7. `validada_em`, `validada_por_id` | Ato vigente e colunas `M:3683–3689,4382–4399` | Validação, devolução, resposta e modo validar do carimbo `services/validacao.py:161–175,284–296`, `services/disputa_emissao.py:1503–1514`, `M:3367–3374` | Filtro físico repetido sob a trava, com os atos relidos para a porta `R:643–646,681,702–703` | Seletor, C6/C7 e guarda do par `R:681`, `J:1408–1411`, `M:2022–2027,3384–3393` | Coluna nula é o filtro SQL. Validação vigente vem da composta `R:681`, `M:4385–4399` |
| 8. `celula_id` | FK do fato `M:2623–2625` | Nascimento e rechaveamento `M:3035–3036`, `services/fio.py:1059–1092` | FK e relação relidas sob a trava `R:643–646,682,702–703` | Seletor, C6 e outbox `R:682–692`, `J:1423–1425`, `M:3472–3496` | FK nula na releitura exclui a candidata `R:682` |
| 9. Veredito e via da célula | Lavra da célula `ponto/portas/celula.py:1032–1045` | `lavrar_veredito` `ponto/portas/celula.py:1090–1098` | Veredito e via relidos sob a trava `R:643–646,688–703` | Seletor, C6 e lastro `R:688–692`, `J:1423–1440,1458–1474`, `ponto/services/lastro.py:145` | SQL exige veredito elegível. C6 interpreta a via admin e a pendência `J:1423–1440,1458–1474` |
| 10. DNA, ata e lâmpada | DNA e ata da célula `J:935–953,1351–1360` | Portas da célula `ponto/portas/celula.py:294–306,1032–1067,1090–1098` | Relação `celula` relida sob a trava `R:643–646,702–703` | C6 e juiz do marco `J:935–975,1436–1440,1463–1474` | Só `True` confirma o marco `J:1472–1474` |
| 11. `data_evento`, `conversa` | Colunas da pergunta `M:2633–2645` | Nascimento e rechaveamento do dia `M:3004–3024`, `services/fio.py:1090` | Colunas relidas sob a trava `R:643–646,702–703` | C20, C6 e outbox `J:337–341,991–997,1475–1482`, `M:3472–3476` | A conversa usa dia civil. O juiz usa o discriminador para obter o dia de turno `J:337–341` |
| 12. `motivo`, `tipo_resposta` | Colunas da pergunta `M:2631,2652` | Nascimento `M:3016–3036` | Colunas relidas sob a trava `R:643–646,702–703` | C6, comparação com a luz e resposta `J:927–946,1347–1352`, `services/disputa_emissao.py:1449–1473` | Motivo identifica a cobrança. Tipo identifica o formato da resposta `J:1347–1352` |
| 13. `chamado_destino`, `status_local` | FK e estado do destino `M:3691–3698`, `J:855–864` | Nascimento, recostura e porta de estado `M:213,3035–3036`, `services/fio.py:857–876` | FK e estado relidos sob a trava `R:643–646,702–703` | Chave do memo C6 e outbox `J:855–864`, `M:3463–3470` | A FK pode estar vazia ou ter sido zerada na exclusão do destino `M:3691–3698` |
| 14. Veto C20 | Tranca vigente e trilha de toque `J:979–997,1067–1085` | Portas de trancar e reabrir `ponto/services/fechamento.py:1400,1432` | Veto consultado com a pergunta fresca sob a trava, pelo C20 existente e seu memo `R:643–646,713–720`, `J:1101–1117` | Gerador, C6 e C7 `R:719`, `J:1414`, `M:2022–2027` | Morte por tranca não autoriza carimbo definitivo `R:715–720` |
| 15. C6 e `_pv_memo` | C6 registrado e composta `app/core/juizes.py:290`, `J:1406–1409` | C6 calcula e guarda memo por instância `J:895–908` | C6 da porta recebe instância fresca sob a trava, já com carimbo em memória `R:643–646`, `M:3361–3383` | Gerador, porta e C7 `R:713`, `M:3375–3383,2029–2035` | A resposta interposta chega ao C6 pela composta e a porta devolve `RECUSA_C6` no caso do RED `T:137–157` |
| 18. Trio do carimbo | Colunas e via `M:2694,3324–3328` | `_escrever_carimbo` pela porta `M:3324–3328,3365–3405` | Ausência conferida sob a trava antes do carimbo, que só é salvo se aceito `R:643–646,680`, `M:3375–3405` | Seletor, composta e outbox `R:680`, `M:4388–4390,3468–3476` | Carimbo encontrado na releitura tira a candidata do universo `R:680` |
| 20. Contagem `n` | Carimbos gravados `R:646–649,668` | Incremento local `R:649` | Incremento após sair do `atomic`, se a porta gravou `R:643–649` | Evento e reconciliação `R:1419,1486–1496` | Candidata que saiu do universo soma zero, assim como a recusada pelo C6 `R:647–649,678–693`, `T:153–163` |
| 21. Disputa e fila `_disputas` | FK e mapa por PK `M:2626–2629`, `R:633,650–659` | Laço dos carimbos bem-sucedidos `R:647–651` | Entra a disputa relida sob a trava, após sucesso do carimbo `R:643–651` | Fecho e log `R:659–667` | PK deduplica. Mudança para outra disputa exclui a candidata desta passagem `R:635–651,678–679` |
| 22. `disputa.fechada_em` | Coluna da disputa `M:2118–2122,2335–2363` | Fecho e reabertura `M:2118–2122,2312–2363` | Entrada na fila usa a disputa relida sob a trava. Há nova leitura antes de C7 `R:643–651,659–663` | E2, porta de fecho e devolução `R:650–663`, `M:2061–2065`, `services/validacao.py:298–303` | `None` indica disputa aberta, inclusive após reabertura `M:2335–2363` |

## 21/11 16:48 · sessao 1a64a28f04be · E4C-F5-CONSTROI · construtor · origem=Code

PAREI no RED

Obra **E4C-F5-CONSTROI**, base `41d21a249017e328617fbbd0dc4841f0794c78c8`. Aval `44f7c468aeb7` e rascunho `566c3b225f0c`, conforme `aval.txt:1`. Conferência independente dos hashes dos anexos [nao verificado].

Editei três arquivos de teste e conferi a sintaxe por AST. A execução dos REDs, dos mutantes e da suíte fica com o Code e o opus final [nao verificado]. `chamados/tests/test_marco_por_motivo.py:168–277` · `chamados/tests/test_fila_validar_lote.py:79–126` · `chamados/tests/test_e3_c8b_sem_banco.py:369–402`

### REDs e mutantes

| Caso | Arquivo::teste | Asserção e sítio mordido | Mutante isolado | [nao verificado]
|---|---|---|---|
| a1 | `test_marco_por_motivo.py::test_RED_ato_posterior_nao_absolve_por_batida_vinculada_fora_do_marco` | Batida vinculada às 08h para marco às 13h. O juiz de fato reprova e o slot devolve `VETO_TEMPORAL` com `ts` e `tipo` vazios. Preserva o controle no empate. `chamados/tests/test_marco_por_motivo.py:178–197` · `chamados/juizes.py:349–356,765–782` | Deixar o vínculo sozinho absolver sob veto. |
| a2 | `test_marco_por_motivo.py::test_RED_veto_temporal_nao_absolve_so_pelo_destino_encerrado` | Destino resolvido, sem batida vinculada nem carimbo. O juiz de fato reprova e o slot devolve `VETO_TEMPORAL`. A fixture atribui o destino em memória. `chamados/tests/test_marco_por_motivo.py:199–211` · `chamados/juizes.py:337–341,786–818` | Deixar o destino encerrado absolver sob veto. |
| b1 | `test_marco_por_motivo.py::test_RED_data_hora_desfeita_nao_fornece_slot_pelo_texto_fisico` | Após reabertura com texto físico residual, exige `SEM_RESPOSTA_VIGENTE` e campos vazios, com horizonte explícito e em leitura avulsa. `chamados/tests/test_marco_por_motivo.py:149–158,213–222` · `chamados/juizes.py:349–379` | Voltar a entregar o texto físico residual ao parser. |
| b2 | `test_marco_por_motivo.py::test_RED_hora_desfeita_depende_do_marco_fixo` | A resposta desfeita das 08h deixa `PLANTIO_SEM_TS` sem marco fixo. Com marco fixo, preserva `SLOT` às 13h, tipo `E`. Exercita os dois caminhos de leitura. `chamados/tests/test_marco_por_motivo.py:224–235` · `chamados/juizes.py:382–387` · `chamados/services/materializacao.py:691–704` | Voltar a passar o texto físico ao derivador de `hora`. |
| c | `test_marco_por_motivo.py::test_RED_data_hora_desfeita_nao_relanca_erro_cacheado_do_slot` | A carga real armazena o motivo da `data_hora` inelegível e a própria exceção injetada na derivação da hora. A guarda da `data_hora` prevalece sobre uma entrada de erro. A hora elegível relança a mesma instância. `chamados/tests/test_marco_por_motivo.py:237–259` · `chamados/services/carga_pergunta_viva.py:117–127` · `chamados/juizes.py:361–367` | Isoladamente, perder o motivo ao armazenar, relançar o erro antes da guarda ou recriar a exceção com a mesma mensagem. |
| d | `test_marco_por_motivo.py::test_RED_horizonte_divergente_e_recusado_antes_da_leitura` | Exige `ValueError` para horizontes separados por um segundo, antes de chamar a composta ou consultar o cache. `chamados/tests/test_marco_por_motivo.py:261–277` · `chamados/juizes.py:344–367` | Aceitar a carga de outro horizonte ou rejeitá-la só depois de ler a composta ou o cache. |
| e1 | `test_fila_validar_lote.py::test_RED_lote_sem_resposta_vigente_vai_para_b_mesmo_com_marco_fixo` | A pergunta é A antes da reabertura. Depois, com texto residual e marco fixo, exige B com `SEM_RESPOSTA_VIGENTE`. `chamados/tests/test_fila_validar_lote.py:79–99` · `chamados/services/validacao.py:1213–1228` | Dispensar o C21 quando o marco fixo fornece slot. |
| e2 | `test_fila_validar_lote.py::test_RED_lote_com_data_impossivel_vai_para_b_com_motivo` | O par passa no C21, mas `2026-09-31 15:08` exige B com `DATA_HORA_INVALIDA`. `chamados/tests/test_fila_validar_lote.py:101–108` · `chamados/juizes.py:370–381` · `chamados/services/validacao.py:1213–1228` | Deixar a ausência de slot cair em A. |
| e3 | `test_fila_validar_lote.py::test_RED_lote_sem_consulta_de_conflito_vai_para_b_com_motivo` | Com consulta disponível e sem batida próxima, o controle é A. Se `.exists()` levanta `DatabaseError`, exige B com `porque` preenchido. Usei a chave `porque` já presente no classificador e deixei livre o código para falha da consulta. `chamados/tests/test_fila_validar_lote.py:110–126` · `chamados/services/validacao.py:1202–1211,1223–1228` | Propagar o erro ou tratar a consulta indisponível como ausência de conflito. |
| f | `test_e3_c8b_sem_banco.py::test_testemunhas_de_materializacao_com_e_sem_carga` | Exige uma chamada ao juiz real em cada caminho, avulso e carregado, nas cinco expectativas originais. A asserção da chamada precede a do resultado. `chamados/tests/test_e3_c8b_sem_banco.py:369–402` · `chamados/juizes.py:1439–1456,1503–1512` | Zerar `celula.veredito` na fixture, fazendo a resposta vigente sair pelo degrau anterior ao juiz de fato. `chamados/tests/e3_c8b_apoio.py:183–198` |

### Comandos para o Code

Cada seletor também roda o mutante da linha correspondente, aplicado isoladamente sobre a cura. Os comandos da parte ORM usam a configuração de teste do repositório. `config/settings/ci.py:6–18,38–41`

```bash
red() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    --settings=config.settings.ci "chamados.tests.$1" -k "$2"
}

red test_marco_por_motivo test_RED_ato_posterior_nao_absolve_por_batida_vinculada_fora_do_marco
red test_marco_por_motivo test_RED_veto_temporal_nao_absolve_so_pelo_destino_encerrado
red test_marco_por_motivo test_RED_data_hora_desfeita_nao_fornece_slot_pelo_texto_fisico
red test_marco_por_motivo test_RED_hora_desfeita_depende_do_marco_fixo
red test_marco_por_motivo test_RED_data_hora_desfeita_nao_relanca_erro_cacheado_do_slot
red test_marco_por_motivo test_RED_horizonte_divergente_e_recusado_antes_da_leitura
red test_fila_validar_lote test_RED_lote_sem_resposta_vigente_vai_para_b_mesmo_com_marco_fixo
red test_fila_validar_lote test_RED_lote_com_data_impossivel_vai_para_b_com_motivo
red test_fila_validar_lote test_RED_lote_sem_consulta_de_conflito_vai_para_b_com_motivo

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  -k test_testemunhas_de_materializacao_com_e_sem_carga \
  chamados.tests.test_e3_c8b_sem_banco
```

O mutante do C8b pode ser aplicado em memória. A falha esperada é a ausência da chamada ao juiz de fato [nao verificado]. `chamados/tests/test_e3_c8b_sem_banco.py:377–401` · `chamados/juizes.py:1439–1456`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import unittest
from unittest.mock import patch

from chamados.tests import e3_c8b_apoio

testemunha_original = e3_c8b_apoio.testemunha

def sem_veredito(*args, **kwargs):
    caso = testemunha_original(*args, **kwargs)
    for pergunta in caso[2]:
        pergunta.celula.veredito = None
    return caso

nome = (
    'chamados.tests.test_e3_c8b_sem_banco.AlimentacaoC6.'
    'test_testemunhas_de_materializacao_com_e_sem_carga'
)
with patch.object(e3_c8b_apoio, 'testemunha', sem_veredito):
    suite = unittest.defaultTestLoader.loadTestsFromName(nome) [nao verificado]
    resultado = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(0 if resultado.wasSuccessful() else 1)
PY
```

### Escritores, leitores e significado

| Campo | Quem escreve e quem lê | Significado |
|---|---|---|
| `motivo` | O contrato atribui a escrita a `_slot_esperado`. Os cinco leitores são `_pergunta_materializada_de_fato`, `carga_da_vida`, `classe_do_lote`, `_explicar_validada` e `classificar`. A base usa o par e, no juiz de fato, recompõe o veto. `rascunho.txt:7–21` · `chamados/juizes.py:344–387,765–774` · `chamados/services/carga_pergunta_viva.py:117–127` · `chamados/services/validacao.py:1223–1228` · `chamados/services/explicador.py:48–55` · `chamados/services/vigia_carimbo.py:105–116` | Uma causa explícita por resultado, com a mesma semântica nos cinco leitores. Unidade, fuso e limites temporais ficam nos campos de instante. `rascunho.txt:7–21` |
| `ts` | `_slot_esperado` deriva `data_hora` e repassa o instante da autoridade de plantio em `hora`. Os cinco leitores consomem o instante. O vigia o repassa a `passivo_validado`, que deriva o dia do plantio. `chamados/juizes.py:370–387,765–781,833–835` · `chamados/services/materializacao.py:691–710` · `chamados/services/carga_pergunta_viva.py:126–143` · `chamados/services/validacao.py:1223–1227` · `chamados/services/explicador.py:52–78` · `chamados/services/vigia_carimbo.py:110–116` · `chamados/services/passivo_validado.py:330–348` | Mesmo significado de `datetime` com fuso. `data_hora` usa a data declarada, e `hora` pode chegar a D+1 pela cadeia do turno. As janelas são próprias de cada leitor. O juiz e a carga usam ±20 min inclusivos, a consulta de conflito usa ±60 min inclusivos e o explicador usa ±20 min inclusivos para a vizinha e `[−6h, +30h)` para o contexto do dia. `chamados/juizes.py:238–282,313–333,370–387` · `chamados/services/carga_pergunta_viva.py:126–143` · `chamados/services/validacao.py:1152,1223–1227` · `ponto/turnos.py:2891–2896` · `chamados/services/explicador.py:57–78` |
| `tipo` | `_slot_esperado` fornece `E` em `data_hora` e repassa o tipo do plantio em `hora`. O juiz de fato usa o valor na busca por janela e o explicador usa na descrição e comparação. Carga e vigia descartam o tipo na leitura atual. O lote lê só `ts`. `chamados/juizes.py:379–387,833–835` · `chamados/services/materializacao.py:624–626,672–689,710` · `chamados/services/explicador.py:52,80–87` · `chamados/services/carga_pergunta_viva.py:126` · `chamados/services/vigia_carimbo.py:110` · `chamados/services/validacao.py:1223` | `E` ou `S` designa o tipo da batida. Os consumidores que usam o valor compartilham esse significado. `chamados/juizes.py:238–248,379–387` · `chamados/services/explicador.py:80–87` |
| `carga.agora` | `CargaDaTarefa` guarda o argumento e `carga_da_vida` o copia. `_slot_esperado`, `_pergunta_materializada_de_fato` e `pergunta_viva` leem o atributo quando o horizonte explícito está ausente. `chamados/services/tarefa_do_dia.py:520–525,629–637` · `chamados/services/carga_pergunta_viva.py:83–88` · `chamados/juizes.py:344–354,690–691,903–908` | Mesmo horizonte em `datetime` com fuso. A projeção exclui atos estritamente posteriores, preservando o empate. `_hoje` converte o instante para a data civil local quando necessário. `chamados/juizes.py:1399–1419` · `chamados/veredito_lavrado.py:95–104` · `config/settings/base.py:93–95` |
| `carga.slots` | `carga_da_vida` inicializa e grava por `p.pk`, além de ler a entrada para montar limites. `_slot_esperado` consulta o cache e relança a exceção alcançada. O contrato troca o par pelo resultado completo e conserva a própria exceção. `chamados/services/carga_pergunta_viva.py:95,113–144` · `chamados/juizes.py:361–367` · `rascunho.txt:13–15` | Um mapa por consulta, com PK como chave e horizonte em `carga.agora`. A entrada preserva o significado de `motivo`, `ts` e `tipo`, ou a identidade do erro. Os limites de batida usam inclusividade no filtro, fora do valor guardado. `chamados/services/carga_pergunta_viva.py:69–75,88,113–144` · `rascunho.txt:13–15` |

Na cópia, o único chamador direto que pode passar `agora` e `carga` juntos é `_pergunta_materializada_de_fato`. No caminho carregado da tarefa, ambos saem de `self.agora`. O aquecimento passa só a carga, e lote, explicador e vigia passam só a pergunta. Divergência na frota e contagem A → B por empresa [nao verificado]. `chamados/services/tarefa_do_dia.py:629–637` · `chamados/services/carga_pergunta_viva.py:88,119` · `chamados/juizes.py:765–766,1510–1512` · `chamados/services/validacao.py:1223` · `chamados/services/explicador.py:52` · `chamados/services/vigia_carimbo.py:110`

**LEI-AKITA.** Origem em `_slot_esperado`, testemunha no motivo explícito, REDs de veto temporal, resposta desfeita e identidade da exceção. Cinco chamadores diretos, zero juízes novos. Padrão Single Writer Principle. `chamados/juizes.py:344–387,765–766` · `chamados/tests/test_marco_por_motivo.py:178–259` · `rascunho.txt:47`

## 21/11 16:40 · sessao 332160fd17f8 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei a cura do item 1.2 na cópia `548cc20fac7de3b283e0c87196393f87b2fd4418`. A irmã é eleita por C21 sobre `DecisaoPergunta.composta`. O materializador trava a principal e a irmã eleita, relê a eleição após a quarta trava e devolve uma recusa com motivo se a eleição ou os vínculos que determinam o conjunto mudarem. [materializacao.py](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:139) · [montagem e releitura](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/materializacao.py:2120)

A porta existente adquire **Colaborador → Disputas → Chamados → Perguntas**, em PK crescente em cada grupo. Confirmei por leitura que o passivo já entrava pela mesma função. A alteração no passivo consome a eleição confirmada dentro do mesmo `atomic` e registra a recusa do lote se a releitura mudar o escopo. O conjunto é montado em `travar_o_ato_do_plantio`. [models.py](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/models.py:2875) · [passivo_validado.py](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/services/passivo_validado.py:379)

**Verificação desta volta.** Sintaxe dos três arquivos conferida em memória e `git diff --check` com rc 0. RED, GREEN, mordida no PostgreSQL e incidência em produção [nao verificado]. A certificação cabe ao Code e ao opus final. A porta da suite usa Docker, rede e banco de teste. `bin/suite.sh:316–325`

### RED de concorrência

**Em `548cc20fa`, FALHA por leitura do código.** A fixture retira o instante físico da volta e preserva o ato. Na direção saída → volta, o filtro SQL da base perde a volta e envia apenas `{saída}` ao `FOR UPDATE`. Na direção volta → saída, envia `{saída, volta}`. A asserção exige os dois PKs em ambas as direções, qualquer que seja a thread que chegue primeiro ao banco. `chamados/services/materializacao.py:139–150,2085–2103` na base · [teste novo](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_e4c_f5_plantio_concorrente.py:52)

O `TransactionTestCase` sincroniza as duas eleições antes da primeira trava, confere PIDs PostgreSQL distintos e fecha cada conexão no `finally`. O `SET LOCAL lock_timeout` de 4 s limita a espera. O teste confere o `ORDER BY` do SQL real, uma tentativa que grava, uma recusa motivada e exatamente uma batida e uma trilha `materializar_batida` por metade. A trava anterior do colaborador serializa a disputa pelo par. A inversão dos PKs é detectada pela asserção do SQL, pois essa serialização pode evitar um deadlock mesmo com a ordem invertida. `chamados/models.py:2875–2904` · `chamados/tests/test_e4c_f5_plantio_concorrente.py:35–113`

No RED, use a base com o arquivo de teste novo. Para os dois GREENs, use a cópia com a cura. `bin/suite.sh:192–208,316–325`

```bash
wt=/home/ronald/wt-codex-e4c-f5-constroi-160129

suite() { [nao verificado]
  PYTHONDONTWRITEBYTECODE=1 "$wt/bin/suite.sh" --dir "$wt" --only chamados -- -k "$1" [nao verificado]
}

# RED de concorrência na base com o teste novo [nao verificado]
suite test_duas_materializacoes_inversas_travam_o_mesmo_par_em_ordem [nao verificado]

# GREENs na cópia com a cura
suite BugParIrmaCompostaTest [nao verificado]
suite BugParPlantioConcorrenteTest [nao verificado]
```

### Mutantes para o Code

Cada bloco abaixo é independente. Use `wt` apontando para uma cópia descartável com a cura e a função `suite` acima. O mutante de ordem inverte o SQL entregue ao PostgreSQL pelo observador do teste, preservando o arquivo protegido. `chamados/models.py:2900–2904` · `chamados/tests/test_e4c_f5_plantio_concorrente.py:62–70,98`

```bash
m="$wt/app/chamados/services/materializacao.py"
t="$wt/app/chamados/tests/test_e4c_f5_plantio_concorrente.py"

# 1. C21 sobre a coluna física
sed -i 's/and pergunta_respondida(DecisaoPergunta.composta(q))]/and pergunta_respondida(q)]/' "$m"
suite BugParIrmaCompostaTest [nao verificado]

# 2. Só a principal no conjunto de perguntas
sed -i 's/pergs.add(pk)/pergs.add(p.pk)/' "$m"
suite test_duas_materializacoes_inversas_travam_o_mesmo_par_em_ordem [nao verificado]

# 3. Ordem invertida no SQL real do FOR UPDATE
sed -i '/travas_de_perguntas.append/i\                    sql = sql.replace(" ASC FOR UPDATE", " DESC FOR UPDATE")' "$t"
suite test_duas_materializacoes_inversas_travam_o_mesmo_par_em_ordem [nao verificado]

# 4. Aceitar a eleição anterior apesar da releitura
sed -i 's/if _dados_da_trava_do_plantio(q) != dados_q or (q is not None and q.pk not in travadas):/if False:/' "$m"
suite test_reabertura_entre_eleicao_e_trava_recusa_a_materializacao [nao verificado]

# 5. Admitir uma irmã que só ficou única durante o ato
sed -i 's/if irmas_confirmadas is not None and _dados_da_trava_do_plantio(q) != irmas_confirmadas.get(p.pk):/if False:/' "$m"
suite test_ato_nao_adota_irma_que_so_ficou_unica_depois_da_trava [nao verificado]

# 6. Usar a principal que veio da classificação
sed -i 's/if _IRMAS_CONFIRMADAS_NO_PLANTIO.get() is not None and p.motivo in _PAR_DO_INTERVALO:/if False:/' "$m"
suite test_plantio_rele_principal_trazida_da_classificacao [nao verificado]
```

As mordidas esperadas são o PK/slot ou a ambiguidade no mutante 1, os PKs enviados no 2, a ordem enviada no 3, o motivo exato da recusa no 4, a adoção da terceira pergunta no 5 e a perda da recusa por falta de anuência no 6. `chamados/tests/test_bug_par_projetado.py:1379–1423` · `chamados/tests/test_e4c_f5_plantio_concorrente.py:92–113,146–186`

### LEI-AKITA

`origem=chamados/services/materializacao.py::metade_irma_do_intervalo + travar_o_ato_do_plantio`, `testemunha=C21 sobre a composta relido sob a trava`, `RED=BugParIrmaCompostaTest + BugParPlantioConcorrenteTest`, `quem-mais-le=par_do_intervalo, irma_da_validacao, tela, laço da disputa e passivo`, `juizes novos=0`, `padrao=Single Source of Truth`. `chamados/juizes.py:1551–1560` · `chamados/services/materializacao.py:139–154,193–209,935,2120–2152,2202–2218` · `chamados/services/validacao.py:206–212` · `chamados/services/passivo_validado.py:379–393`

### Autoconferência · campos=24

`M` = `chamados/services/materializacao.py`, `P` = `chamados/services/passivo_validado.py`, `D` = `chamados/models.py`, `V` = `chamados/services/validacao.py`, `E` = `chamados/services/disputa_emissao.py`, `J` = `chamados/juizes.py`.

| Campo ou valor | (a) Fonte | (b) Escritor pela porta | (c) Instante e trava | (d) Leitores | (e) Significado |
|---|---|---|---|---|---|
| Principal e PK | Fila da disputa ou do passivo. `M:2188–2195`, `P:330–359` | PK nasce com a pergunta. `D:3228–3232` | Fila anterior à trava, identidade confirmada após a quarta e instância de intervalo relida no plantio. `M:2142–2147,1919–1922` | Materializador, passivo e vínculo da batida. `M:2202–2218,1583–1587`, `P:541–549` | Pergunta que iniciou o ato. `M:2128–2131` |
| Disputa e colaborador | FK da pergunta e dono da disputa. `D:2626–2643` | Nascimento da pergunta atribui a FK e deriva o dono. `D:3228–3232,3766–3775` | Vínculos comparados após a quarta trava. `M:2113–2117,2142–2149` | Porta de travas, K6 e criação da batida. `M:2139–2141,1924–1928,1528–1536` | Disputa e dono do fato. `D:2637–2643` |
| Pai, destino e clones | FK do pai, destino e IDs dos clones. `M:2113–2117`, `D:3710–3717` | **Achado.** Destino tem portas de nascimento/reuso e de costura. `chamados/services/fio.py:857–867` | União calculada antes da primeira trava e comparada depois da quarta. `M:2126–2149` | Trava, reconciliação e auditoria. `M:2137–2141,1311–1319,1603–1606` | Pai da disputa, destino canônico e clones do episódio. `D:3710–3717` |
| Motivo e complementar | Motivo persistido e mapa do intervalo. `D:2631`, `M:136–143` | Motivo pelo nascimento. Mapa constante. `D:3228–3232`, `M:136` | Filtro da irmã antes e depois das travas. `M:142–151,2128–2149` | Seletor, slot e K6. `M:142–151,631–635,815–819` | Marco cobrado e seu complementar. `M:136–140` |
| `data_evento` | Dia do fato persistido. `D:2633–2636` | `gravar_dia_do_fato` concentra a atribuição. `chamados/services/fio.py:845–854` | Reconfirmado sob as quatro travas. `M:2113–2117,2142–2149` | Seletor, slot, tela e competência. `M:148–151,634–641,799–812`, `chamados/services/fio.py:40–59` | Dia do turno. Uma batida overnight pode cair em D+1. `M:712–717`, `P:101–116` |
| `tipo_resposta` | Campo persistido. `D:2652–2656` | Fornecido à porta de nascimento. `E:449–457`, `D:3228–3232` | Filtro antes da trava e identidade conferida depois. `M:143–151,2113–2117,2142–2149` | Seletor, validação e despacho do materializador. `M:143–151,1983–1989`, `V:199–200` | Formato `hora`. O tipo da batida é derivado no slot. `M:143,631–633,679–696` |
| `materializada_em` | Coluna do carimbo. `D:4382–4390` | `carimbar`, `carimbar_identidade` e portas de descarimbo, conforme o evento. `D:3363–3374,3400,3416–3453` | Filtro prévio e releitura após a trava. `M:148–151,2148–2149,2208–2213` | Seletor, fila e confirmação da gravação. `M:148–151,2188–2195`, `V:659` | Presença física do carimbo. O lastro é julgado à parte. `D:4388–4390`, `J:728–760` |
| `via_resolucao` e fora do fato | Coluna e `fora_do_fato()`. `D:3441–3449` | Portas de carimbo e descarimbo. `D:3324–3328,3416–3453` | Avaliados sobre candidatas na eleição e na releitura. `M:148–154,2148–2149` | Seletor, identidade e passivo. `M:152`, `D:3441–3449`, `P:541–548` | A via nomeia o desfecho. Duplicata e motivo retratado saem da identidade. `D:2599–2603,3441–3449` |
| Pergunta em voo | Contexto do plantio e transação ativa. `M:1826–1827,1894–1896` | `materializacao_em_voo` publica e restaura o PK. `M:1847–1857` | Durante o `atomic` da unidade. `M:1983–1989` | Seletor e carimbo de fechamento. `M:152,2267–2272` | PK materializado no contexto corrente. `M:1894–1896` |
| Sequência de atos | `decisoes`, FK, tipo, instante, PK, texto e efeito na validação. `D:4333–4367` | Porta `DecisaoPergunta.registrar`. `D:4428–4448` | Carga em lote refeita na eleição pós-trava. `M:148–153,2148–2149` | Composta, histórico e C21. `D:4382–4390`, `chamados/services/historico.py:97`, `M:153` | `ato_em` ordena, PK desempata, e o efeito de cada ato determina a vigência. `D:4361–4379` |
| `respondida_em` físico | Coluna e entrada do passivo legado. `D:2658,4312–4317` | **Achado.** Resposta, carimbo e reabertura têm escritores por evento. `E:1499–1537,1682–1684`, `D:3367–3370` | Resposta humana grava após travar o dono. A fila da disputa lê a face SQL antes da trava. `E:1478–1502`, `M:2185–2195` | Fila, face SQL e compositor legado. `J:1583–1588`, `M:2185–2195`, `D:4312–4317` | Carimbo físico, que pode divergir da vigência. `chamados/tests/test_bug_par_projetado.py:1384–1410` |
| `respondida_em` composto | Sequência com a projeção legada. `D:4357–4389` | Derivado pela composta dos atos registrados. `D:4361–4389,4428–4438` | Recalculado na releitura da irmã e lido novamente no ato. `M:148–153,2148–2149,1339–1343` | C21, propriedade `respondida` e anuência. `M:153,197,1341`, `D:3838–3844` | `None` indica ausência de instante vigente, inclusive após reabertura. `D:4368–4379` |
| `resposta_colab` | A composta publica o texto da coluna. `D:4382–4390` | Portas de resposta e reabertura. `E:1499–1537,1630,1682–1684` | C21 relê a candidata sob trava. Principal de intervalo é relida no ato. `M:148–153,2148–2149,1919–1922` | C21, slot, implausibilidade e trilha. `M:153,648,701,1608`, `J:2064–2084` | **Achado.** Slot e implausibilidade leem a coluna crua, que é a fonte de texto da composta. `M:648,701`, `J:2073`, `D:4388–4390` |
| Validação física e composta | Coluna para a assinatura física. Sequência para a vigência. `D:4318–4322,4368–4390` | A assinatura admin passa por `_carimbar_validacao`. Outras portas também alteram a coluna. `V:161–175`, `E:1630,1682–1684`, `D:3371–3374` | A fila lê a coluna antes. O laço julga a principal pela composta depois. `M:2188–2195,2208–2217` | Par, fila, porta do carimbo e vigia. `M:207–211,2188–2195`, `D:3384–3392`, `chamados/services/vigia_ato_x_coluna.py:12` | Assinatura física e validação vigente têm leituras nomeadas. `D:4382–4399` |
| Veredito C21 | `pergunta_respondida(DecisaoPergunta.composta(q))`. `M:145–153` | O juiz existente calcula o booleano. `J:1551–1560` | Duas eleições com leitura nova, antes da primeira trava e depois da quarta. `M:2128–2129,2139–2149` | Seletor, par e anuência. `M:153,197,1341` | Instante vigente e texto não vazio. `J:1551–1560` |
| Irmã decidida e cardinalidade | Uma candidata após os cortes de identidade, carimbo, voo e C21. `M:139–154` | Resultado derivado pelo seletor. `M:154` | Reeleita sob a quarta trava. `M:2148–2152` | Par, trava e validação pública. `M:199–204,2129,2148`, `V:206–212` | **Achado.** `None` cobre entrada inaplicável, zero e múltiplas candidatas. Para a trava significa ausência de irmã eleita. `M:142–154,199–201,2132–2138` |
| Conjunto de PKs | União das principais com as irmãs eleitas. `M:2126–2138` | Montado no ato. Trava efetiva pela porta global. `M:2139–2141`, `D:2900–2904` | Montado antes. PKs efetivamente travados conferidos após a quarta. `M:2126–2149` | Laço da disputa e passivo. `M:2202–2218`, `P:379–393` | Perguntas que o ato pode atingir. `M:202–204,2132–2149` |
| Ordem das quatro travas | `PerguntaDisputa.trava_na_ordem`. `D:2839–2859` | Única porta de aquisição no sítio. `D:2875–2904` | Colaborador, disputas, chamados e perguntas por PK crescente. `D:2875–2904` | Materializador, passivo e validação pública. `M:2139–2140`, `P:385`, `V:752–756` | Ordem de aquisição. `D:2839–2859` |
| Eleição confirmada e motivo da recusa | Comparação das fotos e dos PKs travados. `M:2141–2152` | `travar_o_ato_do_plantio` devolve a eleição ou levanta `AtoDoPlantioMudou`. `M:2146–2152` | Após as quatro travas. Contexto restaurado na saída do ato. `M:2139–2152,1832–1839` | Par, laço da disputa e passivo. `M:202–204,2203–2218`, `P:385–393` | Fotografia confirmada do ato ou motivo de recusa da tentativa. `M:2146–2152` |
| Pendência e data conferida | `pendencia_admin`, `data_conferida_em` e `data_do_fato_de`. `M:168–176,205–206` | Plantio grava pendência. Conferência admin assina a data. `M:1404–1407`, `V:1014` | Lidos no julgamento do par, após a confirmação no caminho do plantio. `M:202–219,2216–2218` | Par, derivador de tela e materializador. `M:205–219,917–924,1938–1963` | Pendência exige ação do admin. Conferência resolve a divergência de data para fins do plantio. `M:168–176,205–206` |
| Slot `(timestamp, tipo)` | `_ts_tipo_do_plantio`, marco e ajuste overnight. `M:622–641,679–717` | Derivador compartilhado. A batida é gravada por `registrar_batida`. `M:622–628,1528–1536` | Derivado no julgamento do par e no plantio. `M:220–238,1401–1409` | Tela, par, plantio e vigia. `M:935–955,1773`, `chamados/services/vigia_carimbo.py:109–114` | Instante civil e espécie `E` ou `S` da batida. `M:233–238,698–717` |
| Batida e `pergunta_origem_id` | Slot confirmado e PK da pergunta. `M:1528–1536,1583–1587` | `registrar_batida` cria. `anotar` vincula a origem. `M:1528–1536,1583–1587`, `ponto/registro_batida.py:255` | No `atomic` do materializador, antes do carimbo. `M:1983–2005,2039–2046` | Juiz do lastro, vigia e espelho. `J:728–760`, `chamados/services/vigia_carimbo.py:105–114` | Batida que materializa a metade, com vínculo explícito à pergunta. `M:1580–1587` |
| Trilha `materializar_batida` | PK da batida e dados do toque. `M:1588–1609` | Escrita passa por `core.observ.evento`. `M:1263–1269`, `core/observ.py:94–98,133–138` | No mesmo ato, antes do carimbo da pergunta. `M:1588–1611,1983–2005,2039–2046` | Auditoria e teste que conta por batida e pergunta. `core/models.py:285–307`, `chamados/tests/test_e4c_f5_plantio_concorrente.py:106–113` | Uma linha de materialização por batida da fixture. `M:1588–1609` |
| Competência e K6 | Janela da empresa pelo dia do fato e TXT vigente. `M:799–812`, `P:89–98`, `folha/export.py:556–594` | Janela e K6 são leituras. O juiz lê a `ExportacaoDominio` vigente. `ponto/janelas.py:60`, `folha/export.py:580–594` | K6 antes da escrita do plantio, após entrar no ato travado. `M:1919–1928,2202–2218` | Tela, validação e materializador. `M:906–916,1924–1928`, `V:215–218,757–759` | Entregue pelo TXT vigente. Falta de dia do fato tem retorno próprio. `M:822–839`, `folha/export.py:588–594` |
| Agrupamento e foto de exportação do passivo | Classificador e chave colaborador × competência. `P:330–364` | `reparar` monta o grupo e consulta a exportação. `P:350–364` | **Achado.** A foto `exportada` é lida antes da trava do plantio e segue para a regeneração. `P:361–364,379–388,454–468` | Placar do passivo e porta de regeneração. `P:365–367,454–468` | A foto orienta o ramo de regeneração. A guarda K6 da escrita é consultada no plantio. `P:454–468`, `M:1924–1928` |

**Outro ponto do censo fora do sítio 1.2.** A validação pública lê `_irma` antes do seu `atomic` para montar chamados e consultar K6. Dentro do ato ela chama `par_do_intervalo` por `_validar_o_par`. Esse caminho direto tem sua própria aquisição de travas e fica fora da prova de concorrência de `travar_o_ato_do_plantio`. `chamados/services/validacao.py:643–659,741–760,786–803`

## 21/11 16:36 · sessao 0715cead0a23 · SENTINELA-DO-SLOT · censo · origem=Code

**A divergência é impossível por construção nos caminhos de produção da cópia.** Há cinco sítios de chamada direta. O único que pode entregar `agora` e `carga` juntos recebe ambos do C6. Nesse caminho, o `agora` explícito é o mesmo `self.agora` que a carga acabou de copiar. O aquecimento do cache passa somente a carga. A condição para a sonda do item 5 não se aplica. `app/chamados/juizes.py:681–682,756–757,887–898,1488–1495` · `app/chamados/services/carga_pergunta_viva.py:88,117–119` · `app/chamados/services/tarefa_do_dia.py:518–522,624–632`

**Base do censo.** O aval fornecido declara `44f7c468aeb7` para `logs/SENTINELA-DO-SLOT/r.txt` e `566c3b225f0c` para o rascunho extraído. Conferência independente desses hashes, do HEAD Git `ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5` e da execução na frota [nao verificado]. Usei as linhas da cópia disponível. `aval.txt:1`

## 1–2. Produção

| Chamador direto | `agora` explícito | `carga` | Origem e possibilidade de divergência |
|---|---|---|---|
| `CargaDaVida.__init__` | Não | Sim, `self` | O construtor escreve `self.agora = tarefa.agora` antes do laço. `_slot_esperado` adota esse valor. Há um único horizonte. `app/chamados/services/carga_pergunta_viva.py:83–95,117–119` · `app/chamados/juizes.py:345–351` |
| `_pergunta_materializada_de_fato` | Quando definido | Quando fornecida | Repassa os dois argumentos por `**`. Se receber só a carga, preenche `agora` com `carga.agora`. Nos dois ramos carregados da tarefa, o escritor é `CargaDaTarefa.__init__`, que grava seu argumento em `self.agora`. O C6 recebe esse valor e a carga que o copiou. `app/chamados/juizes.py:666,681–682,756–757,887–898,1488–1491` · `app/chamados/services/tarefa_do_dia.py:518–522,624–632` · `app/chamados/services/carga_pergunta_viva.py:88` |
| `classe_do_lote` | Não | Não | Passa só `perg`. O relógio de `fila_validar_classificada` é usado para calcular a idade da resposta. `app/chamados/services/validacao.py:1177–1182,1196–1209,1244–1256` |
| `_explicar_validada` | Não | Não | Importa pelo reexport de `chamados.models` e passa só `p`. A lista `_bts` é consultada pelo explicador depois da chamada ao slot. `app/chamados/models.py:4475` · `app/chamados/services/explicador.py:48–64,137,165–171` |
| `vigia_carimbo.classificar` | Não | Não | Passa só `p`, tanto ao juiz de materialização quanto, se ele reprovar, ao slot. `censo`, `vigia_fio` e `passivo_validado` entram pela mesma função `classificar`. `app/chamados/services/vigia_carimbo.py:94–110,119–124` · `app/chamados/services/vigia_fio.py:415` · `app/chamados/services/passivo_validado.py:327` |

**Repasses indiretos de horizonte e carga**

- **Os dois valores.** `tarefa_do_dia` cria `CargaDaTarefa` com o `agora` recebido ou verifica a igualdade com a carga fornecida. `.tarefa()` entra em `_ler_vida`, que cria `carga_da_vida(self, ...)` e chama `pergunta_viva` para as candidatas e para as pendentes do app ainda sem resultado. A sequência é `pergunta_viva → _pergunta_viva_calc → _pergunta_materializada_de_fato → _slot_esperado`, quando o degrau 6 chega ao ramo `hora` ou `data_hora`. Mesmo um instante vindo de argumento externo, como `--agora`, entra nessa sequência por um único atributo. `app/chamados/services/tarefa_do_dia.py:518–522,624–632,656–665,820–835` · `app/chamados/juizes.py:712,756–757,887–898,1482–1495` · `app/chamados/management/commands/diff_e3_sombra.py:95–108,127–145`
- **Só `agora`.** `alimentar_portas` repassa o argumento que recebeu e também o disponibiliza a `_no_app`. `consulta_do_app` repassa seu argumento a `perguntas_do_app`, que chama o C6 sem carga. A rota do app escreve o valor com `_tz_borda.now()`. `_calados` usa o argumento ou `timezone.now()`. `contexto_calendario` faz o mesmo e chama o C6 pelo alias `_pv`. Até quando a própria tarefa abre o calendário com `self.agora`, essa chamada ao C6 recebe somente o horizonte. `app/chamados/juizes.py:1627,1656–1677` · `app/chamados/services/pergunta_no_app.py:45–77` · `app/api/views.py:2549–2553` · `app/chamados/services/cobranca.py:221–241` · `app/colaboradores/services/calendario.py:157–159,188–193,498–507` · `app/chamados/services/tarefa_do_dia.py:703–706`
- **Chamadas avulsas ao juiz de materialização.** `explicador`, `vigia_carimbo` pelo alias `_juiz`, `raiox`, `disputa_emissao`, `materializacao`, `veredito_lavrado`, o backfill e o auditor pelos aliases `PMF` e `_juiz_s6` passam só a pergunta. `app/chamados/services/explicador.py:91–116` · `app/chamados/services/vigia_carimbo.py:95–106` · `app/chamados/services/raiox.py:193–194` · `app/chamados/services/disputa_emissao.py:2023` · `app/chamados/services/materializacao.py:1937,2176` · `app/chamados/veredito_lavrado.py:262–263` · `app/chamados/management/commands/backfill_veredito_lavrado.py:88,105` · `app/chamados/management/commands/auditar_invariantes_chamados.py:509,541,1165,1181`

A API do juiz aceita em tese dois valores independentes. Hoje, um `agora` explícito prevalece na projeção temporal, e o cache ainda pode devolver o resultado da carga. Isso descreve o que aconteceria se um chamador cruzasse dois horizontes. Nos repasses inventariados, o par carregado deriva do mesmo atributo. `app/chamados/juizes.py:345–358,887–898` · `app/chamados/services/carga_pergunta_viva.py:88` · `app/chamados/services/tarefa_do_dia.py:624–632`

## 1 e 3. Testes [nao verificado]

| Acesso ao slot | Argumentos observados | Sítios |
|---|---|---|
| Import pelo reexport de `models` | Nenhum dos dois | `app/chamados/tests/test_marco_por_motivo.py:18–19,67,73,81` |
| Atributo de módulo `juizes` ou `J`, ou import direto | Nenhum dos dois | `app/chamados/tests/test_p71_marco_sem_hora.py:49` · `app/chamados/tests/test_s3f4_data_e_ancora_pela_autoridade.py:108` · `app/chamados/tests/test_o306_vigia_carimbo.py:108–112` |
| Alias local `slot = env.js['_slot_esperado']` | Só `agora`, só `carga`, só `agora`, respectivamente | `app/chamados/tests/test_e3_pos_suite_sem_banco.py:112–117` |
| C8b, por `carga_da_vida` e pelo C6 real do harness | Só carga no aquecimento. No degrau 6 dos controles `data_hora` e `hora` noturna, ambos com o mesmo horizonte. | `app/chamados/tests/test_e3_c8b_sem_banco.py:271–441` · `app/chamados/tests/e3_c8b_apoio.py:109–117,165,183–202` · `app/chamados/juizes.py:887–893,1488–1491,756–757` |

São **nove chamadas diretas escritas em testes**, incluindo as três pelo alias `slot`. A busca textual e por AST em `app/` encontrou o reexport, o alias de dicionário e o carregamento do corpo real pelo harness. Nenhum sítio adicional apareceu por `getattr` ou por passagem de `_slot_esperado` como callback. O mock de HX substitui a função e depois chama o juiz de materialização. As referências da catraca e de O317 inspecionam código. `app/chamados/models.py:4475` · `app/chamados/tests/e4_apoio.py:8–16` · `app/chamados/tests/e3_c8b_apoio.py:109–117` · `app/chamados/tests/test_hx_fallback_sem_lei.py:62–65` · `app/chamados/tests/test_e3_catraca_respondedores.py:588–595,790–798` · `app/chamados/tests/test_o317_passivo_pela_porta.py:554–571`

**Nenhum caso identificado levantaria o novo `ValueError` por divergência.** Executei 11 testes sem banco, observando os argumentos na entrada da função real. Foram 48 chamadas, com 34 só com carga, 2 só com `agora`, 6 sem ambos e 6 com ambos iguais. As seis com ambos foram as cinco testemunhas `data_hora` e o controle de hora noturna. A fixture escreve `tarefa.agora = AGORA`, e `pergunta_viva` adota `carga.agora` antes de repassá-lo. A execução da suíte ORM [nao verificado]. `app/chamados/tests/test_e3_c8b_sem_banco.py:198–443` · `app/chamados/tests/test_e3_pos_suite_sem_banco.py:102–117` · `app/chamados/tests/e3_c8b_apoio.py:183–202` · `app/chamados/juizes.py:887–893,1488–1491,756–757`

## 4. Forma do horizonte

O parâmetro é **`agora`**, nomeado e opcional em `_slot_esperado(p, *, carga=None, agora=None)`. O valor esperado é um `datetime.datetime` com fuso, ou `None` para ausência de horizonte explícito. A carga guarda o instante no atributo **`carga.agora`**, separado de `carga.slots[p.pk]`. O construtor copia o valor sem conversão nem validação de tipo. O comando com `--agora` valida o fuso na borda. `app/chamados/juizes.py:345–358,1382–1400` · `app/chamados/services/carga_pergunta_viva.py:83–95,117–126` · `app/chamados/management/commands/diff_e3_sombra.py:99–108`

`CargaDaTarefa.__init__` escreve o primeiro atributo a partir do argumento `agora`. `CargaDaVida.__init__` lê esse atributo e escreve o da carga do C6. Os leitores diretos de `CargaDaVida.agora` são `pergunta_viva`, `_pergunta_materializada_de_fato` e `_slot_esperado`, cada um quando seu `agora` é `None`. A comparação já existente em `tarefa_do_dia` lê o `agora` de **`CargaDaTarefa`**. `app/chamados/services/tarefa_do_dia.py:518–522,825–835` · `app/chamados/services/carga_pergunta_viva.py:69,83–88` · `app/chamados/juizes.py:345–351,681–682,887–893`

## 21/11 16:35 · sessao ce01571cc26e · E4C-F5-CONSTROI · verificador · origem=Code

**Veredito geral — BLOQUEIA.** A leitura de `_agora_sonda` passou para depois do prefetch, mas continua antes da trava. Se o relógio recuar entre essa leitura e `_agora_r`, a simulação pode descartar uma `VALIDACAO` que a `REABERTURA` gravada deixa vigente. O resultado é uma pergunta descarimbada, com `validada_em` nula na coluna e preenchida na leitura do ato. `app/chamados/models.py:1565–1567, :1597–1601, :1657, :1707–1728, :4367–4385`.

Conferi a cópia contra o diff fornecido e executei, em memória e com dublês de ORM, o universo, o C6 e a leitura dos atos nos casos de avanço, empate e recuo. A identidade Git da cópia, a execução da suíte Django e a ocorrência desses casos em produção ficam **[nao verificado]**. A comparação com o pai usa a versão anterior mostrada no `verif1.diff`. [nao verificado]

## 1. Fonte, escritores, instante, leitores e significado — RESSALVA

| Valor | Fonte, escritor e instante | Leitores e significado |
|---|---|---|
| `_agora_sonda` | Local de `_universo_da_ressuscitacao`. Começa em `None` e recebe `timezone.now()` uma vez, na entrada da primeira iteração, antes de qualquer `continue`. `app/chamados/models.py:1557–1574`. | Apenas o filtro do cache lê o valor. É um `datetime` com fuso, compartilhado pelo lote. O filtro retém `ato_em > _agora_sonda`, portanto a limpeza simulada inclui a igualdade. Esse instante delimita a simulação. `app/config/settings/base.py:93–95`, `app/chamados/models.py:1591–1601`. |
| `DecisaoPergunta.ato_em` | Fonte persistida em `DateTimeField`. Cada porta fornece o instante do ato a `registrar`, que insere a linha. O prefetch hidrata o campo, e o filtro o compara em memória para cada pergunta que chega ao C6. `app/chamados/models.py:1555, :1597–1601, :4225–4227, :4433–4444`. | É o instante atribuído ao ato, com fuso, inclusive quando recuperado de um carimbo histórico. `leitura` e a face SQL da validação ordenam por `(ato_em, pk)`. A tarefa inclui `ato_em <= agora`. A foto da reabertura busca a validação por igualdade de instante, e a absorção usa igualdade com o carimbo, ordenando a vigência por PK. Portanto o campo tem unidade comum, mas cada leitor tem sua própria regra de inclusão e desempate. `app/chamados/models.py:1713–1718, :3609–3619, :4319–4328, :4367–4385, :4423–4431`, `app/chamados/services/tarefa_do_dia.py:388–403, :430–434`. |
| Cache `decisoes` de `_q` | O ORM carrega o cache de `p` pelo prefetch. Depois das guardas de texto e marco aceso, o método clona o queryset relacionado, instala `_result_cache` filtrado e `_prefetch_done=True`, e coloca a entrada num dicionário próprio da cópia `_q`. `app/chamados/models.py:1555, :1573–1586, :1597–1600`. | `pergunta_viva(_q)` chega ao cache por `DecisaoPergunta.composta` → `leitura` → `p.decisoes.all()`. Para essa cópia, o cache representa somente as linhas prefetchadas estritamente posteriores ao corte, sobre colunas já zeradas. O cache original de `p` continua com a sequência carregada pelo ORM. `app/chamados/models.py:1587–1601, :4367–4373, :4387–4396`, `app/chamados/juizes.py:1406–1411`. |
| `_agora_r` | Local de `ressuscitar_perguntas`. A porta lê `timezone.now()` uma vez, dentro do `atomic`, após adquirir as travas, reler os PKs graváveis e registrar as fotos legadas. A leitura precede a consulta das fotos para os atos compensadores. `app/chamados/models.py:1652–1657, :1675–1680, :1707–1713`. | A montagem de `_compensa` usa esse `datetime` com fuso como `ato_em` de todas as `REABERTURA` do lote e o persiste por `registrar`. Uma validação com timestamp igual é limpa se sua linha preceder a reabertura por PK. Uma validação com timestamp maior continua vigente. `app/config/settings/base.py:93–95`, `app/chamados/models.py:1719–1725, :4336–4337, :4367–4385`. |

Os fornecedores de **`ato_em`** são a resposta humana, que lê o relógio após a trava, a resposta automática, que recebe o instante do processamento, e a cura pela trilha, que recebe o `criado_em` da nota. A validação usa o próprio `validada_em`; as devoluções leem `now()`; recusa e reabertura por correção usam o instante lido sob a trava. `app/chamados/services/disputa_emissao.py:1480–1501, :1535–1537, :1642–1668, :1705–1715, :1719–1737`, `app/api/foto_recebida.py:38–48`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:65–69`, `app/chamados/services/validacao.py:161–175, :284–295, :365–374`.

O silêncio usa `materializada_em`; a retratação usa `_agora_ret`, capturado antes da sua transação; a absorção usa o relógio da passagem; a ressuscitação usa `_agora_r`. A promoção do legado copia `respondida_em` ou `validada_em`, e a migração herdou `versao_reaberta_em`. Assim, `ato_em` tem um significado de domínio, **instante atribuído ao ato**. Inferir dele a hora do commit ou uma ordem monotônica de inserção extrapola o contrato. `app/chamados/models.py:1720, :2112–2118, :2216–2218, :2246–2253, :4319–4328`, `app/ponto/services/veredito_celula.py:190–197`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:350–354`, `app/chamados/services/materializacao.py:1036, :1056–1058, :1073–1090`, `app/chamados/migrations/0051_decisao_pergunta.py:39–61`.

## 2. Chamadores, elegibilidade e travas — RESSALVA

- **`ressuscitar_perguntas`** usa `ids` para selecionar as disputas e pedir as travas. Se `ids` vier vazio, devolve zero. Sob a trava, restringe a escrita aos PKs ainda materializados e fora das vias terminais. Havendo escrita, a auditoria leva os `ids` relidos e as listas originais `pulados` e `mortas_c6`. Chegam à porta `reabrir`, o cartório e o desarquivamento por competência trancada. `app/chamados/models.py:1517, :1638–1657, :1675–1678, :1725–1728, :1741–1765`, `app/ponto/services/cartorio.py:1721`, `app/chamados/views_arquivar.py:70–86`.
- **`perguntas_a_ressuscitar`** devolve só `ids`. `disputa_emissao` une esses PKs às ancoradas na carga de reabertura do credor, que alimenta as guardas de competência exportada e limbo. **`perguntas_puladas_na_ressuscitacao`** devolve `marco_aceso=pulados` e `c6_morta=mortas_c6`. Quando a escrita devolve zero, o cartório roda essa prévia para contabilizar e nomear os motivos. `app/chamados/models.py:1520–1532`, `app/chamados/services/disputa_emissao.py:1851–1861, :1889–1911`, `app/ponto/services/cartorio.py:1721–1739`.

**Caso concreto.** Tome uma pergunta materializada, sem resposta, de marco apagado, cujo C6 daria viva sem validação. O pai lê o relógio em `T0`. Uma `VALIDACAO` é registrada em `T1 > T0` e fica visível no prefetch. No pai, ela sobrevive ao filtro e o PK cai em `mortas_c6`. Na cópia, a sonda lê `T1` ou depois, descarta a linha e o PK entra em `ids`. Isso muda o retorno de ambas as prévias se cada uma atravessar essa mesma intercalação. Com uma única candidata, a porta passa de zero, antes da trava, para pedir as travas do colaborador, da disputa, do chamado e da pergunta. A ordem do protocolo é **Colaborador → Disputa → Chamado → Pergunta**, por PK crescente em cada grupo. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:320–345, :364–384`, `app/chamados/models.py:1526–1532, :1583–1602, :1638–1657, :2881–2909`, `app/chamados/juizes.py:1406–1415, :1458–1462`.

## 3. Posição real do relógio — OK

No caminho normal e com lote não vazio, **sim, o relógio é lido depois dos dois SELECTs**. O método constrói um queryset novo e entrega o próprio queryset ao gerador. O `for p in perguntas` usa a iteração normal do QuerySet, que materializa o resultado e roda o prefetch antes de entregar a primeira pergunta ao gerador. O `yield p` vem depois disso, e só então o corpo do laço externo chama `now()`. O teste de custo também exige um lote de decisões para uma ou cinco perguntas. `app/requirements.txt:13`, `app/chamados/models.py:1549–1567`, `app/chamados/juizes.py:1187–1195`, `app/chamados/tests/test_e4c_f5_opus_models.py:132–147`.

O queryset local chega à iteração sem avaliação anterior no método. Se já estivesse materializado, o acesso ao cache ocorreria após a avaliação anterior. O gerador aceita `.iterator()` de outros chamadores, mas este passa `qs` diretamente. Lote vazio conserva `None` e retorna as três listas vazias. Num lote não vazio, até a primeira pergunta que seria pulada por texto legado ou marco aceso já fixou o instante antes do `continue`. `app/chamados/models.py:1549–1574, :1582–1586, :1597–1602`, `app/chamados/juizes.py:1183–1195`.

Essa ordem estabelece o fim do carregamento em memória. Os SELECTs ainda são distintos. Há teste vizinho que intercala uma escrita depois de carregar as colunas e antes do prefetch das decisões. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:112–137`.

## 4. Classes de reprovação — BLOQUEIA

| Classe | Veredito |
|---|---|
| **(a) Segundo juiz** | **OK.** A cópia continua chamando o C6 registrado. Os filtros de resposta e de marco continuam delegados aos juízes existentes. `app/docs/JUIZES.md:10–15, :102–109`, `app/core/juizes.py:290, :305`, `app/chamados/models.py:1546–1555, :1582–1585, :1601`. |
| **(b) Segundo escritor** | **OK.** O filtro escreve no cache da cópia. A linha compensadora persistida passa por `registrar`, que faz o `bulk_create`. O selo de escritor único verifica esse caminho por AST. `app/chamados/models.py:1597–1600, :1719–1725, :4433–4444`, `app/chamados/tests/test_e1_entradas.py:185–222`. |
| **(c) Relógio fora da borda** | **BLOQUEIA.** A sonda está na borda do lote lido. O ato usa outra amostra, sob a trava e depois da releitura. O universo entra na trava já decidido e a releitura filtra materialização e via, sem refazer o C6. Isso permite a divergência do item 5. `app/chamados/models.py:1565–1567, :1598–1601, :1638–1657, :1675–1680, :1707–1725`. |
| **(d) Derivação paralela** | **RESSALVA.** O diff desloca o relógio de um filtro que já existia no pai. O C6 e a leitura dos atos continuam únicos, mas a simulação representa o efeito de uma reabertura por um corte no cache, e a escrita produz uma linha que a autoridade ordena por `(ato_em, pk)`. A equivalência entre esses dois caminhos tem a falha concreta do item 5. `verif1.diff`, `app/chamados/models.py:1587–1601, :1719–1725, :4367–4385`. |
| **(e) RED sem mordida** | **OK para o mutante pedido.** Ao mover `now()` para antes do laço, a sonda pega `T0`. O wrapper avança a `T0 + 500 ms`, registra a validação e entrega o gerador real. A validação fica no cache simulado, o C6 exclui o único PK e a primeira chamada retorna `0`. `_alvo` exige `n == 1`. Isso vale para os dois REDs com avanço na leitura. O controle sem avanço conserva o empate. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:323–345, :364–389`, `app/chamados/models.py:1598–1601, :1638–1644`, `app/chamados/juizes.py:1406–1411`. |
| **(f) Ordem e conjunto de travas** | **RESSALVA.** O protocolo de ordem é o mesmo. O conjunto muda porque `ids` muda antes da trava, inclusive de nenhuma aquisição nessa porta para a aquisição dos quatro grupos no caso do item 2. `app/chamados/models.py:1638–1657, :2881–2909`. |
| **(g) Campo com dois significados** | **OK quanto ao campo.** `ato_em` continua sendo o instante atribuído ao ato. O problema é tomar dois instantes locais de finalidades diferentes como cortes equivalentes. A própria sequência admite ato inserido depois com timestamp anterior, como no caso de absorção já testado. `app/chamados/models.py:1565–1567, :1707–1725, :4225–4227, :4411–4429`, `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:368–377`. |

O RED **morde a antecipação pedida**, mas deixa uma lacuna. Se o mutante chamar primeiro o wrapper, ler o relógio e só depois consumir o gerador retornado, a validação do teste já terá sido injetada e o relógio já terá avançado, embora a iteração real ainda esteja por começar. As prévias também são coletadas após a injeção e `_alvo` deixa esses retornos sem asserção. `app/chamados/juizes.py:1187–1193`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:332–339, :357–372`.

## 5. Regressões frente ao pai — BLOQUEIA

1. **Recuo entre a sonda e a trava.** Use a intercalação dos REDs, com `T0 = 12:00:00.000` e `T1 = 12:00:00.500`. A validação nasce em `T1`, entra no prefetch e a sonda nova, em `T1`, a descarta por igualdade. Na entrada da trava, faça o relógio voltar a `T0`. O pai teria mantido a validação (`T1 > T0`) e pulado a pergunta. A cópia trava o PK, grava `REABERTURA` em `T0` e limpa a coluna. Na leitura posterior, a validação em `T1` vem depois da reabertura em `T0`, mesmo com PK menor. A coluna fica nula e o ato continua validado. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:323–345, :364–384`, `app/chamados/models.py:1598–1601, :1675–1680, :1707–1728, :4367–4385`.
2. **Recuo durante o carregamento.** Suponha uma validação já gravada às `12:00:00.500`. O pai lê `12:00:01.000` antes do lote. O relógio volta a `12:00:00.000` antes do primeiro `yield` e volta a `12:00:01.000` antes da eventual escrita. O pai descarta a validação, inclui o PK e a sua reabertura a vence. A cópia conserva a validação, põe o PK em `mortas_c6` e retorna zero se for a única candidata. É a mudança de elegibilidade no sentido inverso ao do RED. `app/chamados/models.py:1565–1567, :1598–1602, :1638–1644, :1707–1725, :4367–4385`, `app/chamados/juizes.py:1406–1411`.

Os casos existentes de borda cobrem avanço dentro do wrapper, avanço na entrada da trava e empate com o relógio congelado. O `RED_10` avança o relógio enquanto a **resposta humana** espera sua trava. Há teste vizinho que usa `move_to` para intercalar uma devolução antes da trava do **fechamento da disputa**. Nenhum desses cenários citados exerce o recuo entre `_agora_sonda` e `_agora_r` na ressuscitação. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:244–274, :323–345, :374–389`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:224–245`.

## 21/11 16:19 · sessao bba814d3db61 · E4C-F5-CONSTROI · construtor · origem=Code

**PAREI no RED do item 3.3.** Escrevi a regressão e o controle. O aval exige a mordida pareada antes da cura de produção, e a execução da porta usa Docker e PostgreSQL, fora da autorização deste papel. A sintaxe, o `diff --check` e o seletor em `--seco` passaram. RED, GREEN, mutante executado e comparação com main [nao verificado]. `r.txt:39,74`, `bin/suite.sh:265–276,316–325`, `app/config/settings/ci.py:6–18`, [test_e2_pergunta_so_ao_c6.py](/home/ronald/wt-codex-e4c-f5-constroi-161213/app/chamados/tests/test_e2_pergunta_so_ao_c6.py:115).

O teste intercala a resposta real `19:45` depois que o gerador seleciona a pergunta com os atos carregados e antes de o reconciliador pedir a trava. A lâmpada da saída registra `17:02`. A prova exige o ato persistido, C6 vivo na releitura, `RECUSA_C6` na porta real, o trio do carimbo vazio e contagem zero. O controle usa a mesma fixture sem intercalar resposta e exige carimbo de absolvição assinado por `sistema` e contagem um. A intercalação ocorre no `TestCase`, em uma conexão. `app/chamados/reconciliador.py:635–646,702–724`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:85–87,115–167`, `app/chamados/services/disputa_emissao.py:1478–1537`.

### Comando e mutante

O Code usa o mesmo seletor para RED na cópia atual, GREEN após a cura e RED com o mutante. A falha-alvo da corrida está na asserção do carimbo e da contagem. O controle deve passar nas três rodadas. `bin/suite.sh:197–207,316–325`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:153–167`.

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-constroi-161213 --only chamados -- -k test_f5_e2_absolvicao
```

**Cura proposta.** Após a trava existente, reler o PK pelo mesmo seletor de candidatas, com as relações e os atos carregados em consulta comum. Conferir o dono na instância relida, consultar o veto C20 existente e entregar a instância fresca a `carimbar`. Usar a disputa dessa instância para enfileirar o fecho. **Mutante `instancia_pre_trava`.** Na cura, trocar apenas o receptor de `fresca.carimbar(...)` pela candidata original do gerador e rodar o comando acima. A cópia atual já contém essa chamada sobre a instância original. `app/chamados/reconciliador.py:634–667,671–724`, `app/chamados/models.py:2830–2848,3330–3405`.

### Quem mais lê

- Há **três entradas diretas** do escritor. O consumidor `EventoCelula` soma o retorno em `out['perguntas']`. A reconciliação do colaborador soma o retorno ao total, com entradas pelos sinais de chamado e batida. O cartório também chama o escritor após a primeira lavra e segue para o gate de fechamento, descartando o retorno. `app/chamados/reconciliador.py:1410–1422,1482–1496`, `app/chamados/signals.py:370`, `app/ponto/signals.py:215`, `app/ponto/services/cartorio.py:1018–1033`.
- O **vigia** consome o mesmo gerador e conta os itens emitidos para colaboradores sem evento. O escritor entrega a instância à porta de carimbo e usa sua disputa na fila de fecho. A docstring do seletor também cita a semeadura. O corpo da semeadura usa chamados vivos e `_orfas_abertas` com `fantasma_pela_lampada`. `app/chamados/management/commands/reconciliar_chamados.py:89–93,109`, `app/chamados/reconciliador.py:645–667,671–675,1332–1366`.

`LEI-AKITA: origem=retratar_perguntas_absolvidas_por_celula, testemunha=C6 da porta de carimbo sobre a leitura fresca, RED=test_f5_e2_absolvicao [nao verificado], quem-mais-le=EventoCelula + reconciliação do colaborador + cartório + vigia do gerador, juizes novos=0, padrao=Single Source of Truth`. `app/chamados/reconciliador.py:635–646,702–724,1419,1486`, `app/chamados/models.py:3375–3383`, `app/ponto/services/cartorio.py:1025–1026`, `app/chamados/management/commands/reconciliar_chamados.py:91–92`.

### Autoconferência · campos=26

Os nomes curtos da tabela apontam para `app/chamados/reconciliador.py` (`R`), `app/chamados/models.py` (`M`), `app/chamados/juizes.py` (`J`) e `app/chamados/tests/test_e2_pergunta_so_ao_c6.py` (`T`). Os campos do mesmo ato aparecem juntos.

| Campo ou valor | (a) Fonte | (b) Escritor | (c) Instante | (d) Leitores | (e) Significado único |
|---|---|---|---|---|---|
| 1. Candidata e `p.pk` | Linha ORM, universo de `R:671–693` | Nascimento pela porta `M:3016–3036` | Linha e relações antes da trava `R:635–646,702–706` | Escritor e vigia `R:635–651`, `management/commands/reconciliar_chamados.py:91–92` | PK identifica a linha. A emissão pelo gerador indica elegibilidade na leitura inicial. `R:702–724` |
| 2. Colaborador, `_cid` e dono da disputa | Argumento e FK `M:1892`, `R:634–645` | `_cid` local. O nascimento vincula a pergunta à disputa. `R:634`, `M:2626–2629,3035–3036` | PK antes da trava. A exigência atual usa a disputa carregada antes dela. `R:635–645` | Trava, C20 e C6 `R:644–645`, `J:996–997,1478–1482` | O mapa troca instâncias do mesmo PK, sem decidir elegibilidade. `J:1174–1195` |
| 3. Trava de linha e registro do dono | `SELECT FOR UPDATE` e registro da transação `M:2874–2878` | `trava_na_ordem` pela abreviação `trava_do_colaborador` `M:2830–2848` | Primeiro bloqueio no `atomic`, sobre o PK do dono `R:643–645` | Escritor de resposta e exigência do E2 `services/disputa_emissao.py:1478–1485`, `R:644–645` | O registro comprova o dono travado. A exigência detecta sua falta. `services/trava_do_colaborador.py:29–47,55–63` |
| 4. Atos `decisoes` | Sequência real e complemento legado `M:4311–4323,4355–4367` | `DecisaoPergunta.registrar` `M:4428–4453` | Prefetch antes da trava. A resposta invalida o cache da própria instância. `R:702–703`, `M:4438–4450` | Composta, C6 e fecho `M:4382–4390,2017–2027`, `J:1406–1409` | `[]` no cache antigo e `[]` relido podem representar instantes distintos. `T:131–147`, `M:4355–4367` |
| 5. `respondida_em` | Instante vigente do ato na composta `M:4368–4390` | Resposta humana, automática, recuperação da trilha e reabertura têm portas próprias. `services/disputa_emissao.py:1499–1537,1630–1683,1689–1737` | Resposta humana lê o relógio sob a trava. E2 recebe a foto pré-trava. `services/disputa_emissao.py:1478–1499`, `R:635–646` | C6/C21 e C7 `J:1408–1428,1551–1560`, `M:2022–2027` | `None` da leitura vigente cobre falta de resposta e resposta desfeita. O horizonte também pode ocultar a resposta. `M:4368–4379`, `J:1394–1398` |
| 6. `resposta_colab` | Texto da projeção na composta `M:4382–4390` | Escritas humana, automática e de reabertura `services/disputa_emissao.py:1499–1502,1630–1683,1707–1715` | Coluna da candidata lida antes da trava. `R:635–646,702–703` | C21 e comparação com a luz `J:1359–1379,1426–1440` | A composta usa o texto da projeção. A comparação com a luz lê a coluna diretamente. `M:4385–4390`, `J:1361` |
| 7. `validada_em` e `validada_por_id` | Instante vigente pelo ato. Coluna e autor físicos na pergunta. `M:2659,3683–3689,4382–4399` | Validação, devolução, resposta e carimbo com `resposta='validar'` têm caminhos próprios. `services/validacao.py:161–175,284–296`, `services/disputa_emissao.py:1503–1514`, `M:3367–3374` | SQL inicial lê a coluna antes da trava. A porta compara ato e coluna no `atomic`. `R:643–646,678–682`, `M:3384–3393` | Filtro E2, C6, C7 e guarda do par `R:681`, `J:1408–1411`, `M:2022–2027,4393–4399` | `NULL` físico e ausência de validação vigente têm significados distintos. O SQL usa o primeiro. `R:681`, `M:4385–4399` |
| 8. `celula_id` | FK do fato `M:2623–2625` | Nascimento e rechaveamento da pergunta `M:3035–3036`, `services/fio.py:1059–1092` | Relação carregada antes da trava. `R:635–646,702–703` | Seletor, C6 e outbox `R:682–692`, `J:1423–1425`, `M:3472–3496` | FK nula exclui do universo inicial. A célula vinculada ainda precisa de veredito elegível. `R:682–692` |
| 9. `celula.veredito` e `veredito_via` | Julgamento lavrado na célula `ponto/portas/celula.py:1032–1045` | `lavrar_veredito` `ponto/portas/celula.py:1032–1041,1090–1094` | O gerador lê antes da trava. O cartório chama E2 depois da primeira lavra. `R:702–719`, `ponto/services/cartorio.py:1015–1026` | SQL, C6 e lastro `R:688–692`, `J:1423–1440,1458–1474`, `ponto/services/lastro.py:145` | SQL exclui nulo, vazio e veredito acusador. C6 distingue a via admin e o julgamento provisório. `R:688–692`, `J:1423–1440,1458–1474` |
| 10. DNA, ata e lâmpada | `CelulaDia.dna` dá o marco. A ata dá a luz e a pendência. `J:935–953,969–975,1351–1360` | Operações da porta de célula gravam DNA e ata. `ponto/portas/celula.py:294–306,1032–1067,1090–1094` | Dados carregados no `select_related` pré-trava. `R:702–703` | C6 e juiz do marco `J:935–975,1436–1440,1463–1474` | `None` agrega causas de falta de prova. Só `True` confirma o marco. `J:935–945,1472–1474` |
| 11. `data_evento` e `conversa` | Dia da pergunta e discriminador do nascimento `M:2633–2645` | Nascimento e `gravar_dia_do_fato` no rechaveamento `M:3004–3024`, `services/fio.py:850–854,1090` | Colunas da candidata pré-trava. `R:635–646,702–703` | C20, C6 e outbox `J:337–341,991–997,1475–1482`, `M:3472–3476` | `data_evento` contém dia civil de conversa ou dia do turno. O juiz resolve pelo discriminador. `J:337–341` |
| 12. `motivo` e `tipo_resposta` | Campos da pergunta `M:2631,2652` | Porta de nascimento `M:3016–3036` | Colunas carregadas antes da trava. `R:635–646,702–703` | C6, comparação com a luz e resposta `J:927–946,1347–1352`, `services/disputa_emissao.py:1449–1473` | O motivo identifica a cobrança. O tipo identifica o formato de resposta. `J:927–939`, `services/disputa_emissao.py:1449–1473` |
| 13. `chamado_destino` e `status_local` | FK e estado do chamado `M:3691–3698`, `J:855–864` | Nascimento e recostura escrevem a FK. `_gravar_estado` é a porta de estado. `services/fio.py:857–876`, `M:213` | Relação carregada antes da trava. `R:702–703` | Chave do memo C6 e outbox `J:855–864`, `M:3463–3470` | FK ausente admite ausência de resolvedora ou FK zerada por exclusão. `M:3691–3698`, `services/fio.py:860–867` |
| 14. Veto C20 | Tranca vigente e trilha de toque `J:979–997,1067–1085`, `ponto/services/fechamento.py:1464–1470` | Trancar e reabrir têm portas próprias. C20 calcula por leitura. `ponto/services/fechamento.py:1400,1432`, `J:979–989` | Veto explícito pré-trava. C6 o lê novamente e o memo pode vir do request ou colaborador. `R:713–720`, `J:1101–1117,1414–1415` | Gerador, C6 e C7 `R:719`, `J:1414`, `M:2022–2027` | `False` cobre dia sem tranca aplicável e dia liberado por toque. C6 morto pela tranca não autoriza absolvição. `J:1027–1032,1073–1085`, `R:715–720` |
| 15. Veredito C6 e `_pv_memo` | Juiz registrado, sobre a composta `core/juizes.py:290`, `J:1406–1409` | Juiz calcula e guarda memo por instância. `J:895–908` | Gerador antes da trava. Porta sob a trava, depois de aplicar o carimbo em memória. `R:643–646,713–714`, `M:3361–3383` | Gerador, carimbo e C7 `R:713`, `M:3376,2029–2035` | `False` significa que o C6 dá por morta, com vários fundamentos. O cache do ato e o memo da instância antiga podem sobreviver à escrita em outra instância. `J:1406–1474`, `M:4438–4450`, `T:131–157` |
| 16. Autor `_user` | Usuário `sistema` `core/utils.py:6–18` | Inicialização local com `get_user_sistema` `R:632,637–638` | Antes da trava. `R:637–644` | Porta de carimbo e fecho `R:646,663–665` | `None` local sinaliza carregamento pendente. O helper retorna um usuário. `R:632–638`, `core/utils.py:17–18` |
| 17. Borda do relógio | `timezone.now()` na resposta e no carimbo. C6 avulso usa leitura corrente. `services/disputa_emissao.py:1494–1501`, `M:3361`, `J:880–899` | Cada porta é dona do instante do próprio ato. `services/disputa_emissao.py:1499–1501,1535–1537`, `M:3361–3366,2118–2122` | Resposta e carimbo sob a trava. Sem horizonte explícito, o fallback do C6 pode ler `date.today()`. `services/disputa_emissao.py:1478–1499`, `R:643–646`, `J:589–598` | Sequência, carimbo, janela viva e fecho `M:4363–4372,3361–3366,2118–2122`, `J:1475–1476` | Instante do ato, carimbo e horizonte têm funções próprias. A fixture avança o relógio na resposta. `J:1383–1403`, `T:137–147` |
| 18. `materializada_em`, `materializada_por_id`, `via_resolucao` | Trio do carimbo e via `VIA_CELULA_ABSOLVEU` `M:2694,3293–3328` | Atribuição centralizada em `_escrever_carimbo`. E2 usa `carimbar`. `M:3324–3328,3365–3405`, `R:646` | SQL inicial lê ausência antes da trava. A porta aplica em memória e salva sob a trava após C6. `R:643–646,680`, `M:3363–3405` | SQL, C6, lastro e outbox `R:680`, `J:684–696,1408–1409`, `M:3468–3476` | `via_resolucao=''` também representa legado. O autor pode ficar nulo via `SET_NULL`. `M:2771–2773,3699–3708` |
| 19. `_gravou` e `recusa_do_carimbo` | Retorno e motivo da porta `M:3317–3322,3353–3405` | `carimbar` `M:3330–3405` | Depois do julgamento em memória, no `atomic`. `R:643–647`, `M:3375–3405` | E2 lê o bool. Materialização também lê motivo. `R:646–649`, `services/materializacao.py:1074,2017,2222–2228` | `False` agrega três recusas. `RECUSA_C6` distingue a corrida no teste. `M:3317–3322,3353–3393`, `T:157` |
| 20. Contagem `n` | Número de chamadas que gravaram `R:631,646–649,668` | Incremento local único `R:649` | Incremento após a saída do `atomic` da candidata. `R:643–649` | Evento e reconciliação do colaborador `R:1419,1486–1496` | Zero cobre universo vazio, recusas e falhas capturadas. O vigia conta candidatas por outro contrato. `R:647–653`, `management/commands/reconciliar_chamados.py:91–93`, `T:155–163` |
| 21. Disputa, PK e `_disputas` | FK e mapa local por disputa `M:2626–2629`, `R:633,650–659` | O laço registra as disputas dos carimbos bem-sucedidos. `R:647–651` | Conjunto montado após cada carimbo. Consumido após o laço. `R:647–663` | Fila de fecho e log do fecho `R:659–667` | Repetir o PK deduplica a tentativa de fecho. `R:650–659` |
| 22. `disputa.fechada_em` | Coluna da disputa `M:2118–2122,2335–2347,2359–2363` | Fecho grava. Portas admin e sistema limpam. `M:2118–2122,2312–2363` | Entrada na fila usa relação pré-carregada. Antes de C7 há refresh fora do `atomic` da pergunta. `R:650–663` | E2, porta de fecho e devolução `R:650–663`, `M:2061–2065`, `services/validacao.py:298–303` | `None` significa aberta, inclusive após reabertura. Uma foto velha pode impedir a entrada na fila. `M:2335–2363`, `R:650–661` |
| 23. Veredito C7 | Respostas sem veredito e `fio_vivo` `M:2022–2035` | `pode_fechar` calcula. `fechar` aplica a guarda na porta. `M:2029–2048,2071–2088` | Depois do laço. A guarda interna do fecho também precede a própria trava. `R:659–665`, `M:2071–2097` | E2 e porta de fecho `R:662–665`, `M:2071–2088` | `False` agrega resposta sem veredito e pergunta viva. O E2 usa só a autorização booleana. `M:2029–2035`, `R:662` |
| 24. `fechada_por`, `resolucao` e `motivo_fechamento` | Autor, texto do E2 e motivo escolhido pela porta `R:663–665`, `M:2088,2118–2122` | `fechar` grava o conjunto. Reaberturas limpam. `M:2118–2122,2335–2363` | Fecho usa seu próprio `atomic`, após o laço. `R:659–665`, `M:2090–2097,2118–2122` | Sinal do pai e auditoria `M:2053,2123–2126`, `app/chamados/signals.py:42–43` | Texto de resolução e motivo de fechamento são campos distintos. `M:2120–2126` |
| 25. Foto da lavra | `veredito_lavrado`, `julgado_em`, `janela_expira_em` `veredito_lavrado.py:279–281` | A porta da lavra atribui os três. A chamada E2 usa o padrão `lavrar_veredito=False`. `veredito_lavrado.py:270–275`, `M:3330,3394–3403` | Fotografia em memória após C6, antes do save. `M:3394–3403` | Restauração em caso de falha e leitor do cache lavrado `veredito_lavrado.py:284–289`, `M:2776–2805` | Vazio significa nunca lavrado. `indeterminado` é um veredito próprio. `M:2783–2798` |
| 26. Falhas e PK no log | Exceções por pergunta ou por disputa `R:652–653,666–667` | `_log.exception` nos dois tratadores `R:652–653,666–667` | Após falha do ato correspondente. `R:643–667` | O laço continua. O retorno carrega só a contagem. Leitor externo dos logs [nao verificado]. `R:652–668` | Falha da candidata e falha do fecho têm mensagens distintas. Nenhuma tem código próprio no retorno. `R:652–668` |

O teto SQL é uma pendência da cura. O selo mede o caminho da batida com um e três furos que fecham, e o cartório é uma entrada direta do E2. O impacto da releitura nesse teto [nao verificado]. `app/ponto/tests/test_selo_chokepoint_escrita.py:324–325,467–475`, `app/ponto/services/cartorio.py:1018–1026`.

## 21/11 16:10 · sessao 2f41c9f70b86 · E4C-F5-CONSTROI · construtor · origem=Code

**PAREI no RED do item 1.2.** `travar_o_ato_do_plantio` chama `metade_irma_do_intervalo` antes de tomar a trava e inclui o PK da irmã em `perguntas`. No cenário da irmã vigente sem carimbo, o filtro atual envia `{principal}` à trava. C21 sobre a composta enviaria `{principal, irmã}`. Na divergência inversa, a irmã sai do conjunto. A ordem das quatro travas pode continuar idêntica e ainda assim mudar **quais PKs são travados**. O laço de materialização e o passivo usam esse mesmo caminho. `chamados/services/materializacao.py:139–150,2085–2103,2149–2163`, `chamados/models.py:2900–2904`, `chamados/services/passivo_validado.py:376–384`

O diff ficou em [test_bug_par_projetado.py](/home/ronald/wt-codex-e4c-f5-constroi-160129/app/chamados/tests/test_bug_par_projetado.py:1354). A fixture usa um dia anterior ao índice por marco para representar duas irmãs elegíveis. A resposta e a reabertura passam pelas portas reais. Os `update`s da fixture introduzem a divergência física. `chamados/tests/test_bug_par_projetado.py:90–97,1354–1408`, `chamados/models.py:2605–2613,3823–3828`, `chamados/services/disputa_emissao.py:1532–1537,1664–1685`

| Caso | Asserção |
|---|---|
| Irmã vigente excluída pela coluna | O par devolve o PK e o slot `D+1 02:00 E`. A validação pública devolve o PK da irmã e a batida gravada é conferida por tipo, instante e `pergunta_origem_id`. `chamados/tests/test_bug_par_projetado.py:102–115,1379–1393` |
| Irmã desfeita admitida pela coluna | Após a reabertura real, a projeção obsoleta volta a ter instante e texto. A composta segue sem resposta vigente e a porta recusa o par. `chamados/tests/test_bug_par_projetado.py:1395–1410` |
| Duas irmãs elegíveis | Ambas são respondidas pela composta e só uma tem carimbo físico. A porta deve recusar por falta de irmã única. `chamados/tests/test_bug_par_projetado.py:1412–1423` |
| Competência entregue | O TXT vigente é semeado no modelo que K6 consulta. Tela e validação recusam, com comparação das perguntas, dos atos e das batidas antes e depois. `chamados/tests/test_bug_par_projetado.py:1425–1444`, `folha/export.py:580–594` |

**Base** `b0935c1f106d232dc39b263676a588d66eddff54`, conferida com `git rev-parse HEAD`. **Commit RED** ausente pelo veto a `git commit` no brief. **Commit cura** ausente pela parada no conjunto de PKs. SHA-256 do diff RED `6545ec357dfbfeda0360175cbbab78c622f7f14ad026dabc8b828fa6057c5bdc`. `chamados/tests/test_bug_par_projetado.py:1354–1444`, `chamados/services/materializacao.py:2085–2103`

Conferi a sintaxe com `compile()` em memória e o diff com `git diff --check`. RED executado, GREEN, mordida executada, pareamento main × F5, incidência em produção e resultado dos controles vizinhos ficam [nao verificado]. A suite usa Docker, rede e PostgreSQL de teste. Os controles vizinhos estão em `chamados/tests/test_bug_par_projetado.py:136–171,423,691,751`. `bin/suite.sh:316–325`, `config/settings/ci.py:6–18`

**Comando RED e GREEN para o Code** na base com o teste e, futuramente, na cópia com a cura autorizada. `bin/suite.sh:192–208,316–325`

```bash
PYTHONDONTWRITEBYTECODE=1 /home/ronald/wt-codex-e4c-f5-constroi-160129/bin/suite.sh \ [nao verificado]
  --dir /home/ronald/wt-codex-e4c-f5-constroi-160129 \
  --only chamados -- -k BugParIrmaCompostaTest
```

**Mutante `irma_pelo_carimbo`** restaura a seleção física antes da unicidade. Esse é o corpo presente na base, portanto o comando acima já exercita o alvo nesta entrega. Depois de uma cura autorizada, numa cópia de teste em que o único delta de `materializacao.py` seja o seletor, o Code pode aplicar a reversão abaixo e repetir o comando. As asserções-alvo são a do PK e slot, a da irmã reaberta e a das duas elegíveis. `chamados/services/materializacao.py:139–150`, `chamados/juizes.py:1583–1588`, `chamados/tests/test_bug_par_projetado.py:1382,1410,1423`

```bash
wt=/home/ronald/wt-codex-e4c-f5-constroi-160129
git -C "$wt" diff b0935c1f106d232dc39b263676a588d66eddff54 -- app/chamados/services/materializacao.py \
  | git -C "$wt" apply -R
```

**LEI-AKITA:** origem=`metade_irma_do_intervalo`, testemunha=C21 sobre a composta, RED=`BugParIrmaCompostaTest` [nao verificado], quem-mais-le=`par_do_intervalo`, `travar_o_ato_do_plantio` e `irma_da_validacao`, com tela, admin, materializador e passivo a jusante, juizes novos=0, padrao=Single Source of Truth. `chamados/juizes.py:1551–1560`, `chamados/services/materializacao.py:195,928,1766,2093,2154`, `chamados/services/validacao.py:212,648,743`, `chamados/services/passivo_validado.py:382`

### Autoconferência · campos=19

As referências abreviadas na matriz são de `app/chamados/`. Os três chamadores diretos são `materializacao.py:195,2093` e `validacao.py:212`. A tela chega pelo derivador, pelo tag e pelo explicador. O admin chega pela validação pública e pelo serviço de ações. O plantio e o passivo chegam pela trava comum. `services/materializacao.py:894–928,1766–1778,2149–2163`, `templatetags/chamados_tags.py:136`, `services/explicador.py:124`, `services/acoes_disputa.py:13–23,143`, `services/passivo_validado.py:376–384`

| Campo ou valor | (a) Fonte | (b) Escritor | (c) Instante e trava | (d) Leitores | (e) Significado único |
|---|---|---|---|---|---|
| `disputa` / `disputa_id` | FK da pergunta e disputa recebida. `models.py:2626–2630`, `materializacao.py:139–148` | Porta de nascimento atribui a FK. `models.py:3035–3061,3228–3232` | Identidade lida antes da trava do plantio e do admin. `materializacao.py:2091–2103`, `validacao.py:743–756` | Par, trava e validação usam a mesma disputa. `materializacao.py:195,2093–2103`, `validacao.py:212` | `disputa=None` faz o seletor abster-se. `materializacao.py:143–144` |
| `motivo` e complementar | Motivo persistido e mapa de ida e volta. `models.py:2631`, `materializacao.py:136,142` | Nascimento grava o motivo. O mapa é constante. `models.py:3228–3232`, `materializacao.py:136` | Motivo de `p` da instância recebida, da irmã no filtro SQL. `materializacao.py:142–149` | Par, derivador do tipo e K6. `materializacao.py:189,624–626,808–812` | `outro=None` significa motivo fora do mapa. A saída do seletor compartilha o `None`. `materializacao.py:142–150` |
| `data_evento` | Dia do turno gravado na pergunta. `models.py:2633–2636`, `services/fio.py:845–854` | `gravar_dia_do_fato` é o ponto de atribuição de nascimento, costura e re-chaveamento. `services/fio.py:845–854,1084–1096` | Instância de `p` e filtro da irmã antes da trava inicial. Re-chaveamento relê sob trava. `materializacao.py:143–148,2091–2103`, `services/fio.py:1059–1066` | Identidade, plantio e K6 pelo dia do fato. `materializacao.py:147,624–635,792–805` | O dia civil do slot pode cair em D+1. O dia do fato continua sendo o dia do turno. `juizes.py:299–312,325–334` |
| `tipo_resposta` | Campo da pergunta. `models.py:2652–2656` | Tipo fornecido pelo emissor, persistido pela porta de nascimento. `disputa_emissao.py:449–457`, `models.py:3228–3232` | Instância da principal e filtro SQL da candidata. `materializacao.py:143–148` | Escritor da resposta, admin e despacho do materializador. `disputa_emissao.py:1449–1467`, `validacao.py:199–200`, `materializacao.py:1957–1964` | `'hora'` identifica o formato da pergunta. O tipo `E`/`S` é derivado à parte. `materializacao.py:143,624–626,672–689` |
| `materializada_em` | Coluna usada no corte SQL e publicada na composta. `materializacao.py:148`, `models.py:4382–4390` | Porta de carimbo e portas de descarimbo. `models.py:3324–3328,3400,3451–3461`, `disputa_emissao.py:1682–1684` | Corte inicial antes da trava. O laço relê a principal sob a trava. `materializacao.py:2091–2103,2153–2163` | Seletor, fila e confirmação do par gravado. `materializacao.py:148,2142–2163`, `validacao.py:659` | `NULL` significa sem carimbo físico. O lastro tem juiz próprio. `materializacao.py:148,1970–1972`, `juizes.py:667–674` | [nao verificado]
| `via_resolucao` / fora do fato | Coluna e predicado `fora_do_fato`. `models.py:2603,3441–3449` | `_escrever_carimbo` recebe o valor pelas portas de carimbo. `models.py:3324–3328,3416–3438` | Avaliado sobre cada candidata da consulta, antes da trava inicial. `materializacao.py:146–149,2091–2103` | Seletor, índice de identidade e descarimbo. `materializacao.py:149`, `models.py:3451–3461,3823–3833` | `fora_do_fato` agrupa duplicata e motivo retratado pelo efeito comum de saída da identidade. `models.py:2599–2603,3441–3449` |
| Em voo | PK no `ContextVar` e transação ativa. `materializacao.py:1819–1825,1875–1877` | `materializacao_em_voo` põe e retira o PK. `materializacao.py:1828–1838` | Durante o bloco atômico do plantio. `materializacao.py:1956–1964` | Seletor e proteção contra reentrada pelo fecho. `materializacao.py:149`, `disputa_emissao.py:2016,2032` | `True` indica materialização neste contexto transacional. `materializacao.py:1875–1877` |
| Atos da irmã, FK, `ato`, `ato_em`, PK, `ato_texto`, `resposta_mantem_validacao` | Relação `decisoes`. `leitura` ordena linhas reais e passivo. `models.py:4219–4223,4333–4367` | `DecisaoPergunta.registrar` é a porta única e invalida o cache da instância recebida. `models.py:4428–4453` | Hoje a consulta da irmã vem depois da seleção física. A leitura do par fica antes da trava na tela, e dentro do ato no admin e no plantio. Carga em lote no seletor é parte da cura pendente. `materializacao.py:145–150,195–205,928,1765–1778`, `validacao.py:746–788` | `composta`, histórico e consumidores do par. `models.py:3846–3858,4382–4390`, `materializacao.py:200–205` | `ato_em` data o ato, PK desempata e `resposta_mantem_validacao` decide o efeito da resposta sobre a assinatura. C21 recebe texto da projeção. `models.py:4333–4378,4388–4390` |
| `respondida_em` físico | Coluna da projeção. `models.py:2657–2659` | Resposta humana, automática, reparo pela trilha, carimbo e limpeza têm portas por evento. Não há um único escritor físico. `disputa_emissao.py:1499–1537,1682–1684,1707–1737`, `models.py:3367–3374` | A resposta humana toma a trava do dono antes de reler e ler `now()`. O seletor hoje lê a coluna antes da trava. `disputa_emissao.py:1478–1502`, `materializacao.py:145–150,2091–2103` | Face SQL, fila do materializador e composição do passivo. `juizes.py:1583–1588,1770–1775`, `materializacao.py:2140–2146`, `models.py:4312–4317` | **Achado.** O seletor trata o carimbo como resposta vigente. Os dois contracasos distinguem o carimbo do ato. `test_bug_par_projetado.py:1384–1410` |
| `respondida_em` composto | `DecisaoPergunta.leitura` e o passivo sem linha correspondente. `models.py:4312–4317,4348–4389` | Derivado pela composta dos atos registrados e da projeção legada. `models.py:4361–4379,4382–4390,4428–4438` | A composição usa a sequência carregada e não lê relógio. A seleção que alimenta a trava seria anterior a ela. `models.py:4357–4367`, `materializacao.py:2091–2103` | C21 da principal, propriedade `respondida` e anuência do plantio. `materializacao.py:191–194,1332–1336`, `models.py:3838–3844` | `None` é ausência de instante vigente. Nunca respondida e resposta desfeita convergem para esse valor. `models.py:4368–4379` |
| `resposta_colab` | A composta publica o texto da projeção. `models.py:4382–4390` | As portas de resposta e reabertura escrevem ou limpam o texto. `disputa_emissao.py:1499–1537,1630,1682–1684,1707–1715` | Na resposta humana, texto e instante são gravados juntos sob a trava do dono. Na seleção atual o texto é lido no SQL. `disputa_emissao.py:1478–1537`, `materializacao.py:145–149`, `juizes.py:1583–1588` | C21, implausibilidade, declaração de hora e derivador do slot. `juizes.py:1551–1560,1785–1803,2064–2084`, `materializacao.py:641–655,694–704` | **Acesso cru encontrado.** O derivador e a implausibilidade leem a coluna diretamente. É a mesma fonte de texto que a composta publica. O `ato_texto` tem papel histórico distinto. `materializacao.py:641,694`, `juizes.py:2073`, `models.py:4199–4202,4382–4390` |
| `validada_em` / `validada_por_id` físicos | Projeção usada para sintetizar validação legada sem linha. `models.py:4318–4322` | A assinatura admin passa por `_carimbar_validacao`. Há também carimbo e limpeza por outras portas. `validacao.py:161–175`, `models.py:3371–3374`, `disputa_emissao.py:1503–1514,1682–1684` | Admin grava depois da trava da porta. A foto legada vem da instância da leitura. `validacao.py:746–760`, `models.py:4294–4303` | Compositor, fila física e vigia de paridade ato × coluna. `models.py:4318–4322,4393–4399`, `materializacao.py:2142–2146` | Coluna preenchida é presença física. A vigência tem leitura própria. `models.py:4393–4399` |
| `validada_em` composto | Sequência processada pela composta. `models.py:4368–4390` | Ato de validação pela porta e atos posteriores com efeito de desfazer. `validacao.py:161–175`, `models.py:4325–4345,4428–4438` | No par, depois da seleção da irmã. No materializador, depois da releitura da principal sob a trava. `materializacao.py:195–205,2153–2163` | Guarda da irmã validada, admin e materializador. `materializacao.py:200–205,2161`, `validacao.py:655–658` | `None` significa ausência de validação vigente, inclusive após devolução, recusa ou reabertura. `models.py:4325–4331,4368–4379` |
| Veredito C21 | `pergunta_respondida`. Sua face SQL atual recebe a projeção física. `juizes.py:1551–1560,1583–1588` | O juiz calcula o booleano. O seletor atual usa a face SQL para a irmã. `materializacao.py:145–150,191–195` | Principal julgada pela composta na entrada do par. Irmã cortada no SQL antes da releitura. `materializacao.py:145–150,191–205` | Par, propriedade `respondida` e anuência do plantio. `materializacao.py:193,1332–1336`, `models.py:3838–3844` | `False` significa ausência do par válido instante + texto. A divergência entre as duas fontes está no RED. `juizes.py:1551–1560`, `test_bug_par_projetado.py:1384–1423` |
| Cardinalidade e retorno `None` | Tamanho da lista após os cortes atuais. `materializacao.py:143–150` | Retorno calculado pelo seletor. `materializacao.py:150` | Após a consulta e os cortes fora do fato e em voo. Na trava do plantio, ainda antes de `trava_na_ordem`. `materializacao.py:145–150,2091–2103` | Par, tráfego de trava e irmã da validação. `materializacao.py:195–197,2093–2103`, `validacao.py:212,743–756` | **Achado.** `None` cobre entrada inaplicável, zero e múltiplas candidatas. O par traduz zero e ambiguidade para a mesma mensagem `FALTA_A_OUTRA_METADE`. `materializacao.py:143–153,195–197` |
| PK de `p` e da irmã | PK da linha persistida. O seletor exclui o da principal e devolve a única irmã. `models.py:3228–3232`, `materializacao.py:148–150` | Inserção pela porta de nascimento. O seletor entrega a instância. `models.py:3228–3232`, `materializacao.py:150` | Identidade calculada inicialmente antes da trava, e seleção refeita no par que planta. `materializacao.py:1765–1778,2091–2103` | Trava, validação, retorno público e vínculo na batida. `materializacao.py:1576–1579,2093–2103`, `validacao.py:659,849–852` | Um PK aponta uma única pergunta. O RED confere o mesmo PK no par, no recibo e na batida. `test_bug_par_projetado.py:102–115,1379–1393` |
| Slot `(ts_q, tipo_q)` | `_ts_tipo_do_plantio`, marco e ajuste do dia do turno. `materializacao.py:224–238,615–626,691–710`, `juizes.py:299–334` | Derivador único. Escrita da batida pela porta `registrar_batida`. `materializacao.py:615–621,1521–1529` | Derivado após escolher a irmã. O plantio rederiva o slot ao chamar a unidade real. `materializacao.py:224,1394–1395,1766–1778` | Tela usa o slot do par. Validação e plantio usam o PK e recalculam pelo mesmo derivador. `materializacao.py:928–948,1766–1778`, `validacao.py:648–659` | `None` no slot do par vem com impedimento. A falta de irmã única conserva a ambiguidade acima. `materializacao.py:175–179,195–197,224–238` |
| IDs de disputa, destino, clones e pai reunidos para a trava | FKs e `chamados_relacionados_ids` da principal e da irmã. `materializacao.py:2093–2101`, `models.py:3713–3717` | Destino escrito por nascimento/reuso e costura. Clones são fornecidos pelo emissor. `models.py:3194–3198,3228–3232`, `services/fio.py:857–867`, `disputa_emissao.py:850` | Coletados antes de `trava_na_ordem`, também na validação. `materializacao.py:2091–2103`, `validacao.py:743–756` | Trava global e reconciliação do destino e dos clones no plantio. `models.py:2880–2904`, `materializacao.py:1265–1312` | IDs identificam linhas. `None` e duplicados são descartados pela normalização da trava. `models.py:2845–2848,2862–2863` |
| Trava | `PerguntaDisputa.trava_na_ordem`. `models.py:2839–2859` | Esse método adquire as quatro famílias de travas. O seletor fornece parte do conjunto. `models.py:2875–2904`, `materializacao.py:2091–2103` | Colaborador → Disputa → Chamado → Pergunta. A eleição da irmã antecede a primeira trava do laço. `models.py:2841–2848`, `materializacao.py:2091–2103,2153–2155` | Laço do admin, passivo e validação pública. `materializacao.py:2154`, `passivo_validado.py:382`, `validacao.py:743–756` | **Bloqueio do sítio.** A mesma ordem recebe outro conjunto de PKs quando a irmã muda de elegibilidade. `materializacao.py:145–150,2093–2103`, `test_bug_par_projetado.py:1384–1423` |

## 21/11 16:09 · sessao 61c4c68ce30e · TRAVAS-SEM-MEMORIA · censo · origem=Code

# Censo M8 — TRAVAS-SEM-MEMORIA

A identificação `a9e3e967859085765806684361ab1c129a2f3779` vem do pedido e do laudo. O HEAD pelo Git e as capturas brutas estão [nao verificado]. A cópia disponível veio sem `.git` e sem `logs/`. A casa documenta a cópia de leitura por `git archive` e exclui `logs/` do Git. `CLAUDE.md:347`, `.gitignore:53`

## 1. Formato do `pede` e do `espera`

- **Sítios da cura.** A captura de `pede`, a extração de `_pl_id` e o ramo de admissão ficam em `bin/suite.sh:357`, `bin/suite.sh:359`, `bin/suite.sh:360`. O comando canônico chega a esse ramo depois da conferência de `PRONTO` e da publicação da ref. Com `rc=0` e ID vazio, a inteira sai 71 e o recorte segue para a VM. O selo fabrica `id=<id> sha=<sha> enfileirado` e mede se o ID correto chegou ao `espera`. `CLAUDE.md:96`, `bin/suite.sh:340`, `bin/suite.sh:354`, `bin/suite.sh:389`, `bin/suite.sh:393`, `bin/tests/test_pista_lateral.sh:32`, `bin/tests/test_pista_lateral.sh:174`, `bin/tests/test_pista_lateral.sh:308`, `bin/tests/test_pista_lateral.sh:453`
- **Formato real de `pede`, conforme o anexo.** O laudo relata **ID nu em 12 de 12 capturas** e nomeia `logs/COMBOIO-1911/c/pede.txt`, `logs/E4C-F3-LAVRA-FOTO/preopus/pede.txt` e `logs/ESCALA-NAO-DESCREVE/c7/pede.txt`. Transcreve `1119021427_2fd98211` para a terceira. O RELATO associa os três pedidos aos IDs `1119013536_84786d94`, `1119020736_88f67e29` e `1119021427_2fd98211`, respectivamente. A linha exata de cada captura e o stderr do servidor estão [nao verificado]. `laudo_opus.txt:2` (anexo), `app/docs/RELATO.md:1553`, `app/docs/RELATO.md:1525`, `app/docs/RELATO.md:1526`
- **Formato de `espera`.** O contrato documenta que o status do SSH é o rc da suite. O executor procura `id=<job> sha=<sha> rc=<n>`, compara o rc com o status do SSH e, para rc 0, exige `Ran N tests` positivo depois do cabeçalho. O dublê emite esse cabeçalho e o veredito. O RELATO registra `rc=0`, `Ran` e `OK` para `logs/COMBOIO-1911/c/espera.out` (16932), `logs/E4C-F3-LAVRA-FOTO/opus_final/suite_pista.log` (16938) e `logs/ESCALA-NAO-DESCREVE/c7/opus_delta/suite_pista.log` (16952). O anexo menciona três capturas de espera sem transcrevê-las. O cabeçalho real, a ordem das linhas e três provas diretas em `espera*.txt` ficam [nao verificado]. `app/docs/BACKLOG.md:2227`, `bin/suite.sh:368`, `bin/suite.sh:369`, `bin/suite.sh:372`, `bin/suite.sh:374`, `bin/tests/test_pista_lateral.sh:38`, `bin/tests/test_pista_lateral.sh:50`, `app/docs/RELATO.md:1563`, `app/docs/RELATO.md:1536`, `app/docs/RELATO.md:1541`, `laudo_opus.txt:14` (anexo)
- **Escritores e leitores.** A sessão fornece obra e cópia. A suite resolve `_pl_sha`, monta `_pl_args`, escreve `refs/pista/<sha>` e captura resposta e status em `_pl_out` e `_pl_rc`. A pista recebe a ref pelo contrato. `_pl_id` nasce no parser e alimenta o diagnóstico, o `espera`, a validação da identidade e a medição. O segundo SSH escreve `_pl_esp` e `_pl_erc`. O parser produz `_pl_cab`, `_pl_jrc`, `_pl_ran` e `_pl_falta`, que determinam a saída, a causa e o registro em `pista_lateral.log`. O selo lê a ref, o argv gravado em `ssh.log` e a medição. `CLAUDE.md:96`, `bin/suite.sh:324`, `bin/suite.sh:343`, `bin/suite.sh:354`, `bin/suite.sh:357`, `bin/suite.sh:359`, `bin/suite.sh:363`, `bin/suite.sh:368`, `bin/suite.sh:369`, `bin/suite.sh:374`, `bin/suite.sh:379`, `bin/suite.sh:383`, `bin/suite.sh:315`, `app/docs/BACKLOG.md:2227`, `bin/tests/test_pista_lateral.sh:28`, `bin/tests/test_pista_lateral.sh:174`, `bin/tests/test_pista_lateral.sh:177`
- **Quem mais lê.** `suite.sh` é o único parser dos dois retornos localizado no código entregue. O PreToolUse registrado lê a *invocação* da suite. A seção 3 prescreve ler `Ran`, o veredito e o rc de processo no log. O RELATO registra a sessão como leitora operacional e cita o esperador externo `logs/ORDEM-1811/p2/corrida.sh`. O parser desse script e outros parsers em `logs/` estão [nao verificado]. `bin/suite.sh:359`, `bin/suite.sh:369`, `.claude/settings.json:73`, `bin/hook_pista_pronto.py:146`, `CLAUDE.md:122`, `app/docs/RELATO.md:773`, `app/docs/RELATO.md:1847`

## 2. Leitor da invocação de `codex_pergunta.sh`

- **Sítios da cura.** O ramo de ficha de `decide` e `nega_ficha` em `bin/hook_pista_pronto.py:256`, `bin/hook_pista_pronto.py:291`, `bin/hook_pista_pronto.py:307`, a interface do reconhecedor em `bin/disjuntor.py:1448` e os casos pelo registro em `bin/tests/test_pista_lateral.sh:125`, `bin/tests/test_pista_lateral.sh:272`, `bin/tests/test_pista_lateral.sh:295`. O hook já compartilha `_trechos`, mas faz seu próprio reconhecimento por `comandos`, `_comando` e `_chama`. `_PALAVRA_CHAVE` remove as palavras de controle antes do reconhecimento no disjuntor. O exemplo do anexo usa obra sem `-RASCUNHO`, portanto mostra a divergência de reconhecimento. A recusa M9a depende do sufixo contratual. `bin/hook_pista_pronto.py:54`, `bin/hook_pista_pronto.py:58`, `bin/hook_pista_pronto.py:65`, `bin/hook_pista_pronto.py:108`, `bin/hook_pista_pronto.py:130`, `bin/hook_pista_pronto.py:260`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1454`, `app/docs/RELATO.md:1094`, `laudo_opus.txt:3` (anexo)
- **Escritores e leitores do texto Bash.** A sessão fornece `CODEX_OBRA`, `CODEX_PAPEL` e os anexos na chamada. Os dois PreToolUse recebem `tool_input.command`. **O reconhecedor canônico da invocação do wrapper é `disjuntor._codex_cmds`.** Ele produz `papel`, `volta`, `obra` e `anexos`. Seus consumidores são `_codex_cmd` (auxiliar sem chamada localizada), `codex_decide` para B4 e ANEXO-ENXUTO, `_ramo_b3` no caminho de exceção e `_pretool` no caminho normal. O segundo leitor em M7b é `hook_pista_pronto`, que usa `papel`, `obra` e o primeiro anexo para decidir M9a. `CLAUDE.md:347`, `app/docs/RELATO.md:1094`, `.claude/settings.json:62`, `.claude/settings.json:77`, `bin/disjuntor.py:1841`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:1477`, `bin/disjuntor.py:1486`, `bin/disjuntor.py:1819`, `bin/disjuntor.py:1853`, `bin/disjuntor.py:1947`, `bin/hook_pista_pronto.py:260`, `bin/hook_pista_pronto.py:295`
- **Interfaces que a cura atravessa.** O hook hoje lê `export` anterior, opções de shell e palavras por `shlex`. `_codex_cmds` lê atribuições prefixadas e extrai anexos por regex. O wrapper recebe o argv e o ambiente efetivos, aceita `--anexo <arquivo>` e valida obra e papel. O próprio wrapper aciona o censo do construtor antes da vaga. A ficha vem do chat e a sessão a anexa. O hook confere cabeçalho, linhas `DECISAO` e referências. O wrapper escreve o `.id`, lido pelo disjuntor na triagem e pelo hook na conferência da ficha, que também admite o registro de entrega no RELATO. `bin/hook_pista_pronto.py:67`, `bin/hook_pista_pronto.py:79`, `bin/hook_pista_pronto.py:123`, `bin/disjuntor.py:1449`, `bin/disjuntor.py:1462`, `bin/disjuntor.py:1857`, `bin/codex_pergunta.sh:61`, `bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:247`, `bin/codex_pergunta.sh:464`, `bin/disjuntor.py:855`, `bin/disjuntor.py:897`, `bin/hook_pista_pronto.py:190`, `bin/hook_pista_pronto.py:211`, `bin/hook_pista_pronto.py:270`, `app/docs/RELATO.md:1093`
- **Outros leitores.** `hook_stop_fila1` lê argv e ambiente do processo já vivo em `/proc` para provar a espera do censo. Stop, handoff e alarme consomem essa prova. Os selos exercitam, separadamente, reconhecimento Bash, chegada ao wrapper, prova do processo e dono do censo. Invocações e decisões nos logs da obra estão [nao verificado]. `.claude/settings.json:9`, `bin/hook_stop_fila1.py:730`, `bin/hook_stop_fila1.py:754`, `bin/hook_stop_fila1.py:773`, `bin/hook_stop_fila1.py:804`, `bin/handoff_sessao.sh:130`, `bin/alarme_sessao_ociosa.py:137`, `bin/tests/test_disjuntor.sh:575`, `bin/tests/test_disjuntor.sh:591`, `bin/tests/test_pista_lateral.sh:131`, `bin/tests/test_pista_lateral.sh:276`, `bin/tests/test_hook_esperando_com_prova.sh:201`, `bin/tests/test_censo_antes_da_vaga.sh:258`

## 3. Árvore usada para escolher a volta

- **Sítios da cura.** A construção de `ONLY`, a ordem da guarda de inteira e da resolução do SHA, o comentário sobre o instante e o leitor da árvore no helper. Hoje o helper já escolheu as labels antes de `_pl_sha` nascer. Ele calcula `base..HEAD` e usa o disco para descobrir os testes, conferir existência e ler seu conteúdo. `_suite_inteira` é consultada primeiro pelo disco e novamente com o SHA. `bin/suite.sh:181`, `bin/suite.sh:187`, `bin/suite.sh:254`, `bin/suite.sh:283`, `bin/suite.sh:309`, `bin/suite.sh:322`, `bin/suite.sh:324`, `bin/suite.sh:326`, `bin/vizinhos_da_volta.py:28`, `bin/vizinhos_da_volta.py:44`, `bin/vizinhos_da_volta.py:93`, `bin/vizinhos_da_volta.py:117`
- **Escritores e leitores da seleção.** O chamador fornece `DIR` e `VOLTA`. `ONLY` vem do argv ou do stdout do helper e passa pelo filtro de `LABELS`. A guarda, o `--only` remoto, o `--seco` e o Django da VM leem a seleção. O helper lê o manifesto de selos globais e emite labels em stdout e motivos em stderr. `LABELS` tem fonte na régua e leitores na própria régua, na suite, no pre-push e no selo de fonte única. `bin/suite.sh:153`, `bin/suite.sh:155`, `bin/suite.sh:161`, `bin/suite.sh:176`, `bin/suite.sh:187`, `bin/suite.sh:194`, `bin/suite.sh:206`, `bin/suite.sh:256`, `bin/suite.sh:344`, `bin/suite.sh:402`, `bin/suite.sh:459`, `bin/vizinhos_da_volta.py:126`, `bin/vizinhos_da_volta.py:129`, `bin/selos_globais_da_volta.txt:1`, `bin/regua.sh:23`, `bin/regua.sh:168`, `bin/pre-push.sh:184`, `bin/tests/test_labels_fonte_unica.sh:20`, `CLAUDE.md:117`
- **SHA, diagnóstico e chamadores.** A rota lê `_pl_sha`, reconfere o HEAD e usa o SHA para julgar inteira, conferir `PRONTO`, criar a ref, pedir e validar o job e vincular a contingência à cópia. `volta_intermediaria.sh` fornece `VOLTA_PORQUE`, conta os motivos emitidos pelo helper e publica o resultado. O ruff do mesmo wrapper faz sua própria leitura anterior de HEAD e disco. Os selos substituem o helper por labels prontas. A5 exercita avanço entre SHA e pedido, enquanto V1/V2 exercitam a seleção devolvida pelo dublê. `bin/suite.sh:186`, `bin/suite.sh:189`, `bin/suite.sh:324`, `bin/suite.sh:335`, `bin/suite.sh:340`, `bin/suite.sh:343`, `bin/suite.sh:354`, `bin/suite.sh:369`, `bin/suite.sh:394`, `bin/volta_intermediaria.sh:18`, `bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:34`, `bin/volta_intermediaria.sh:42`, `CLAUDE.md:104`, `bin/tests/test_pista_lateral.sh:105`, `bin/tests/test_pista_lateral.sh:199`, `bin/tests/test_pista_lateral.sh:316`, `bin/tests/test_suite_motivo.sh:32`

## 4. Falha de `git status` dentro da vez

- **Sítios da cura.** `_nv_confere` antes e depois do run, em paridade com a admissão da rota, e o selo das duas bordas. Há `pipefail`, mas o `-z` externo decide pelo texto da substituição e ignora seu status. A admissão consulta o status da atribuição antes de truncar a saída. `bin/suite.sh:34`, `bin/suite.sh:118`, `bin/suite.sh:122`, `bin/suite.sh:125`, `bin/suite.sh:128`, `bin/suite.sh:334`, `bin/suite.sh:337`
- **Escritores e leitores.** Git produz o HEAD e o par saída/status. Na admissão, `_pl_suja` recebe essa saída e alimenta a recusa, com reconferência de `_pl_head`. Na VM, `DIR` atravessa a trava até `_NV_DIR`; a contingência exporta `SUITE_NA_VEZ_SHA` e a entrada normal remove valor herdado. `_nv_confere` lê ambos. O runner produz `_NV_RC`, que a reconferência posterior pode trocar por 2, preservando rc de sinal. O rc final também alimenta a medição de contingência. `bin/suite.sh:109`, `bin/suite.sh:119`, `bin/suite.sh:126`, `bin/suite.sh:128`, `bin/suite.sh:133`, `bin/suite.sh:137`, `bin/suite.sh:154`, `bin/suite.sh:334`, `bin/suite.sh:338`, `bin/suite.sh:394`, `bin/suite.sh:452`, `bin/trava_teste.sh:42`, `bin/suite.sh:509`, `CLAUDE.md:122`
- **Cobertura existente.** O Git falso consegue falhar silenciosamente. A8 cobra essa falha antes do SSH. A3/A4 cobram avanço de HEAD na VM. A9 e A10 cobram ambiente herdado e preservação do sinal. A mesma falha silenciosa de status dentro da vez fica fora desses casos. `bin/tests/test_pista_lateral.sh:77`, `bin/tests/test_pista_lateral.sh:220`, `bin/tests/test_pista_lateral.sh:223`, `bin/tests/test_pista_lateral.sh:327`, `bin/tests/test_pista_lateral.sh:329`, `bin/tests/test_pista_lateral.sh:332`, `bin/tests/test_pista_lateral.sh:445`

## 5. Visibilidade de `declaracoes_ambiguas`

- **Sítios da cura.** O renderizador do ESTADO, a saída `--linhas` usada por `raia_agente.sh listar` e o selo de publicação. Duas cópias limpas sem commits à frente podem produzir uma ambiguidade com `raias` e `sujas` vazias. Nesse caso o ESTADO só imprime a mensagem de nenhuma raia viva. `--linhas` imprime raias, sujas e contadores. `bin/raias_estado.py:143`, `bin/raias_estado.py:157`, `bin/raias_estado.py:159`, `bin/raias_estado.py:230`, `bin/gerar_estado.py:217`, `bin/gerar_estado.py:228`, `bin/gerar_estado.py:232`, `bin/raia_agente.sh:81`
- **Escritores e leitores.** `raia_agente.sh` cria, atualiza, relê e baixa a declaração `logs/raias/<basename>.agente`. `_declaradas` lê o nome e os campos. Git fornece caminhos, HEAD, avanço e sujeira. `ler` calcula `ambiguas`, usa o valor para suspender a atribuição às duas cópias e publica `declaracoes_ambiguas` junto de `raias`, `sujas` e contadores. O JSON expõe o campo. O gerador é o escritor atual da frase do aviso. `bin/raia_agente.sh:49`, `bin/raia_agente.sh:54`, `bin/raia_agente.sh:66`, `bin/raia_agente.sh:73`, `bin/raias_estado.py:51`, `bin/raias_estado.py:69`, `bin/raias_estado.py:77`, `bin/raias_estado.py:108`, `bin/raias_estado.py:142`, `bin/raias_estado.py:151`, `bin/raias_estado.py:157`, `bin/raias_estado.py:183`, `bin/raias_estado.py:189`, `bin/raias_estado.py:238`, `bin/gerar_estado.py:229`
- **Publicação e selos.** O gerador grava o ESTADO. `relato.sh` o entrega ao raspador e publica o destino. E6 lê o campo do JSON com gêmeas que têm commits à frente. O selo de E1 cobre ESTADO gerado, publicado e `listar` para os tokens de cópia, sem cobrir o aviso isolado nessas saídas. Declarações reais e a publicação resultante estão [nao verificado]. `bin/gerar_estado.py:383`, `bin/gerar_estado.py:401`, `bin/relato.sh:38`, `bin/relato.sh:117`, `bin/relato.sh:147`, `bin/raspar_publico.py:28`, `bin/raspar_publico.py:40`, `bin/tests/test_travas_sem_memoria.sh:72`, `bin/tests/test_travas_sem_memoria.sh:110`, `bin/tests/test_travas_sem_memoria.sh:127`, `bin/tests/test_travas_sem_memoria.sh:159`, `bin/tests/test_raia_declarada_no_estado.sh:28`

## 6. Recorte aceito, espera inconclusiva e VM

- **Sítios da cura.** O ramo que já tem `_pl_id` e `_pl_falta` e entrega `_pl_causa` à contingência. V3 cobre falha no pedido. V5 cobre pedido aceito com espera inconclusiva e hoje exige VM após dois SSHs. V6 lê o rc final da VM no log. O dublê e essas expectativas são parte do sítio de prova. `bin/suite.sh:368`, `bin/suite.sh:374`, `bin/suite.sh:382`, `bin/suite.sh:386`, `bin/suite.sh:388`, `bin/tests/test_pista_lateral.sh:31`, `bin/tests/test_pista_lateral.sh:41`, `bin/tests/test_pista_lateral.sh:204`, `bin/tests/test_pista_lateral.sh:311`, `bin/tests/test_pista_lateral.sh:313`, `bin/tests/test_pista_lateral.sh:449`, `bin/tests/test_pista_lateral.sh:459`
- **Escritores e leitores.** A sessão fornece `PISTA_CONTINGENCIA` para a inteira. O bloco escreve `_pl_decl` e `_pl_causa` para declaração, falha de ref, falha de `pede` ou falha de `espera`. O consumidor comum escreve a abertura da VM, exporta `SUITE_NA_VEZ_SHA` e prepara `_PL_VM_FIM`. A trava recebe a chamada, o fim lê `_PL_VM_FIM` e o rc do processo, e `_pl_mede` escreve a trilha que V6 lê. `bin/suite.sh:315`, `bin/suite.sh:350`, `bin/suite.sh:354`, `bin/suite.sh:360`, `bin/suite.sh:386`, `bin/suite.sh:389`, `bin/suite.sh:393`, `bin/suite.sh:396`, `bin/suite.sh:452`, `bin/suite.sh:509`, `bin/tests/test_pista_lateral.sh:313`
- **Autoridade de cancelamento.** O contrato atribui `cancela <id>` à sessão, para volta superada da mesma obra e com trilha. Define confirmação via `estado` e `espera` com rc 130. Há relato de uso em uma corrida pista/VM e registros históricos divergentes sobre a disponibilidade do verbo. Implementação, disponibilidade e resposta atuais do servidor [nao verificado]. `CLAUDE.md:291`, `CLAUDE.md:292`, `app/docs/RELATO.md:2644`, `app/docs/RELATO.md:1847`, `app/docs/RELATO.md:2207`, `app/docs/RELATO.md:2564`, `app/docs/BACKLOG.md:2227`

## 7. Prova de morte dos mutantes do selo

- **Sítios da cura.** Seleção da árvore sob teste, distinção entre controle e rodada filha, contabilização da morte e resumo final. Os sete mutantes estão declarados no bloco. O resumo que pode mostrar `0 mutantes mortos` fica na linha 196 desta cópia. `bin/tests/test_travas_sem_memoria.sh:35`, `bin/tests/test_travas_sem_memoria.sh:170`, `bin/tests/test_travas_sem_memoria.sh:180`, `bin/tests/test_travas_sem_memoria.sh:186`, `bin/tests/test_travas_sem_memoria.sh:193`, `bin/tests/test_travas_sem_memoria.sh:196`
- **Escritores e leitores.** A chamada filha é o escritor localizado de `SELO_TRAVAS_BIN`. O selo lê a variável para escolher `BSOB` e decidir se roda o lote. Valor herdado não vazio também ativa esses dois efeitos. `_erro` escreve `falhou`, lido pelo pré-check, pelo lote e pelo `exit`. `mortos` nasce em zero dentro do lote e é incrementado por qualquer rc não zero do filho. O filho escreve `$T/mut_$1.out`, e a decisão usa apenas o rc. A trap remove o diretório. A régua chama o selo pelo glob, lê o rc e exibe a saída quando há falha. `bin/tests/test_travas_sem_memoria.sh:19`, `bin/tests/test_travas_sem_memoria.sh:25`, `bin/tests/test_travas_sem_memoria.sh:27`, `bin/tests/test_travas_sem_memoria.sh:38`, `bin/tests/test_travas_sem_memoria.sh:170`, `bin/tests/test_travas_sem_memoria.sh:180`, `bin/tests/test_travas_sem_memoria.sh:183`, `bin/tests/test_travas_sem_memoria.sh:196`, `bin/tests/test_travas_sem_memoria.sh:197`, `bin/regua.sh:138`, `bin/regua.sh:140`, `CLAUDE.md:131`
- **Casos a que o rc precisa corresponder.** E1/E2 verificam HEAD e tokens gerados/publicados. A fixture de gêmeas e pasta recriada verifica identidade e metadado velho. E4 confere avanço da cópia, E5 a pasta que some e E6 a atribuição ambígua. Hoje uma falha de preparação também conta, e E5 pode receber a mesma mensagem de falha após uma exceção. O selo vizinho já lê o motivo por caso. Resultado efetivo dos sete mutantes [nao verificado]. `bin/tests/test_travas_sem_memoria.sh:66`, `bin/tests/test_travas_sem_memoria.sh:89`, `bin/tests/test_travas_sem_memoria.sh:114`, `bin/tests/test_travas_sem_memoria.sh:139`, `bin/tests/test_travas_sem_memoria.sh:148`, `bin/tests/test_travas_sem_memoria.sh:153`, `bin/tests/test_travas_sem_memoria.sh:159`, `bin/tests/test_pista_lateral.sh:342`, `bin/tests/test_pista_lateral.sh:356`

## 8. `PRONTO` em bloco cercado

- **Sítios da cura.** A varredura de `tem_pronto` e as provas do mesmo juiz no executor e no hook registrado. O contrato trata citação como ausente. A varredura só tira CR/LF e compara a linha ao marcador ou ao marcador seguido de espaço. Uma linha literal dentro de bloco cercado na coluna 1 satisfaz essa comparação. Os selos existentes incluem citação com `>` e o marcador arquivado. `bin/pronto_item_a_item.py:8`, `bin/pronto_item_a_item.py:35`, `bin/pronto_item_a_item.py:39`, `bin/pronto_item_a_item.py:42`, `bin/tests/test_pista_lateral.sh:170`, `bin/tests/test_pista_lateral.sh:178`, `bin/tests/test_pista_lateral.sh:231`
- **Escritores e leitores.** A sessão publica `PRONTO` por obra e SHA no RELATO. O arquivamento leva o registro a `RELATO-ARQUIVO.md`. `marcador` compõe a chave e `tem_pronto` lê os dois arquivos. O hook obtém obra e HEAD da chamada e consulta o juiz diretamente. A suite fornece chave e SHA à CLI, lê seu rc e usa a saída na recusa. O hook emite a decisão PreToolUse. A leitura dos mesmos arquivos em M9a procura registros `entrega`. O selo escreve suas próprias linhas de fixture. `app/docs/RELATO.md:1083`, `app/docs/RELATO.md:1084`, `CLAUDE.md:98`, `CLAUDE.md:445`, `bin/pronto_item_a_item.py:20`, `bin/pronto_item_a_item.py:26`, `bin/pronto_item_a_item.py:56

[truncado em 20000 de 22540 caracteres]

## 21/11 16:07 · sessao bd43daa99632 · - · - · origem=Ronald (tmux)

## 2. Invocação do Codex

- A cura incide na entrega de ficha do hook, em `bin/hook_pista_pronto.py:256-269,291-310`, e no selo desse caminho em `bin/tests/test_pista_lateral.sh:125-155,255-260,272-298,402-414,461-462`. O reconhecimento atual passa por `comandos`, `_comando`, `_chama` e `_valores`. O ramo da suite também usa esses auxiliares. `bin/hook_pista_pronto.py:54-94,97-133,146-173,299-307`.
- A sessão escreve a invocação e seus valores em `tool_input.command`. A ficha de rascunho leva `CODEX_OBRA=<ID>-RASCUNHO`, `CODEX_PAPEL=leitura` e a ficha no primeiro `--anexo` absoluto. O hook lê esses valores, a ficha e suas referências. `app/docs/RELATO.md:1093-1095,1099`, `CLAUDE.md:347-350`, `bin/hook_pista_pronto.py:176-287,291-316`. A cadeia de referência lê entregas e `.id` pelo disjuntor, registros `entrega` dos RELATOs ou blobs de `main`. `bin/hook_pista_pronto.py:190-253`, `bin/disjuntor.py:855-876`, `bin/codex_pergunta.sh:464-469`.
- O reconhecedor de referência é `disjuntor._codex_cmds`. Ele obtém trechos de `_trechos`, remove palavras de controle com `_PALAVRA_CHAVE` e produz `papel`, `volta`, `obra` e `anexos`. Seus leitores atuais são `_codex_cmd` sem chamador localizado, a decisão B4/ANEXO-ENXUTO, a classificação B3 em exceção e a guarda B3 no fluxo normal. A duplicação do hook significa que ainda há dois reconhecedores do texto em uso. `bin/disjuntor.py:1365-1377,1432-1492,1502-1513,1811-1824,1851-1859,1947-1955`, `bin/hook_pista_pronto.py:258,299-307`.
- `.claude/settings.json:62-79` registra os dois `PreToolUse`; `:3-9` registra o Stop. O Stop lê o `argv` e o ambiente deprocesso vivo em `/proc`, para provar o censo em andamento. Handoff e alarme delegam esse reconhecimento. O wrapper lê argumentos e ambiente reais quando executado. `bin/hook_stop_fila1.py:604-631,730-785,804-809`, `bin/handoff_sessao.sh:130`, `bin/alarme_sessao_ociosa.py:137`, `bin/codex_pergunta.sh:57-67,79-84,100,247,324-331`. Os selos desses leitores estão em `bin/tests/test_hook_esperando_com_prova.sh:201-220,241-245`, `bin/tests/test_censo_antes_da_vaga.sh:258-264` e `bin/tests/test_disjuntor.sh:544-548,575-598,1278-1290`.
- O exemplo do anexo com obra `X` demonstra a diferença de reconhecimento. Para incidir em M9a a obra precisa terminar em `-RASCUNHO`. A leitura antiga também aceita `export` anterior, opções de shell e anexos tokenizados com `shlex`; o parser canônico reconhece atribuições prefixadas e extrai anexos por regex. Essa compatibilidade é técnica dentro da forma M9a já documentada. `bin/hook_pista_pronto.py:67-92,123-126,258-269`, `bin/disjuntor.py:1448-1462`, `bin/codex_pergunta.sh:61`, `app/docs/RELATO.md:1094`.

## 8. PRONTO citado

- O ponto da cura é a varredura dos dois RELATOs por `tem_pronto`. O contrato exclui linha citada e indentada. O código compara a linha com o marcador exato ou seguido de espaço e não controla cercas. `bin/pronto_item_a_item.py:8-11,20-23,26-46`. O selo escreve suas linhas e testa aspas de citação no formato `>`; os pontos pertinentes são `bin/tests/test_pista_lateral.sh:107,116-117,167-179,231-240,361-366,377-378`.
- A sessão escreve o PRONTO item a item; o arquivamento leva as entradas antigas ao RELATO-ARQUIVO. `app/docs/RELATO.md:1083-1084`, `CLAUDE.md:445`. Os dois chamadores são o hook que extrai obra, cópia e HEAD e o executor que lê SHA, passa obra/SHA ao CLI e usa seu retorno. `bin/hook_pista_pronto.py:136-173,313-323`, `bin/pronto_item_a_item.py:49-62`, `bin/suite.sh:324-325,339-341`, `.claude/settings.json:72-79`. A regra aparece no comando canônico `CLAUDE.md:95-99`. O ramo M9a lê esses mesmos arquivos para registros de `entrega` por outra função. `bin/hook_pista_pronto.py:190-203`. Marcador literal efetivamente publicado no padrão completo e capturas de publicação [nao verificado].
- Excluir a citação implementa a regra registrada. `app/docs/RELATO.md:1083`, `bin/pronto_item_a_item.py:8-11`.

## 9. Opções da suite [nao verificado]

- Os pontos de documentação são `bin/suite.sh:22-33`, lidos por `--help` em `:164`, e `CLAUDE.md:95-103`. A pista recusa `--parallel auto` antes da contingência e envia só o N explícito. `bin/suite.sh:331-332,343-354`, `app/docs/BACKLOG.md:2227`.
- O laudo atribui recusa a `--espera`, mas o parser aceita inteiros não negativos. A opção fica fora do pedido e da espera SSH e entra na trava quando se chega à VM. O chamador `volta_intermediaria.sh` sempre a fornece e pode herdar a chave na volta lateral. `bin/suite.sh:158-159,343-348,368,451-460`, `bin/volta_intermediaria.sh:32`, `app/docs/RELATO.md:1082`.
- `PARALELO`, `PARALELO_DADO` e `ESPERA` são inicializados e escritos no parser, e lidos na montagem da linha remota, no `--seco` e na execução da VM. Na VM o `PARALELO` chega a `manage.py test`, e `ESPERA` chega a `flock -w` pela trava. `bin/suite.sh:145-159,332,343-348,403-405,451-460`, `bin/trava_teste.sh:22,25-38`. Selos em `bin/tests/test_suite_motivo.sh:91-92,113-115,150-151`, `bin/tests/test_pista_lateral.sh:206-210,429-430`, `bin/tests/test_trava_teste.sh:121,134`. O leitor do comando canônico recorta o bloco e para antes da prosa da opção. `bin/tests/test_suite_sh.sh:22-30,50-55`.

## DECISOES

Para 2, 8 e a correção documental de 9, perguntas de lei: 0. Caso se queira transformar a ausência de efeito de `--espera` na pista em recusa, há uma pergunta de escopo. A rota com chave deve impedir `--espera` mesmo para a volta lateral que hoje sempre o fornece? `bin/suite.sh:158-159,330-343`, `bin/volta_intermediaria.sh:32`, `app/docs/RELATO.md:1082`.

_7719 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
