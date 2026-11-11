# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 06:22** · publicado em **11/11/2026 06:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 06:22 · sessao b6de4d034e52 · FIO-E0-V4-E3 · construtor · origem=Code

Cura em [ponto/services/cartorio.py:1233](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1233): primeiro argumento volta à chamada direta da porta. A classificação permanece depois da guarda (`cartorio.py:1227`); o marcador conserva `_insumo_nb` (`cartorio.py:1219–1222`).

```diff
@@ -1233 +1233 @@
-                    _insumo_nb, marcos, chamados_vivos=0, chamados_total=0, cobrancas_vivas=0,
+                    com_insumo_dia_sem_trabalho(_dia_nb, trabalha=cel.trabalha, ata=_ata_nb), marcos, chamados_vivos=0, chamados_total=0, cobrancas_vivas=0,
```

| Execução sem banco | Resultado | Testemunha |
|---|---|---|
| Catraca na base informada | RC 1; 1 FAIL, 0 ERROR | `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:226` |
| Catraca na cura | RC 0; 3 testes | mesmo arquivo:200 | [nao verificado]
| Memória NB | RC 0; 15 testes | `ponto/tests/test_e3_nb_impressao_memoria.py:84` |
| Pós-suite | RC 0; 5 testes | `chamados/tests/test_e3_pos_suite_sem_banco.py:19` |
| Medida do turno | RC 0; 35 testes | `core/tests/test_medida_turno_do_par.py:478` |
| Mutante: argumento `_insumo_nb` | RC 1; 1 FAIL, 0 ERROR | catraca:226 |
| Mutante: argumento `_dia_nb` | RC 1; 1 FAIL, 0 ERROR | catraca:226 |
| Mutante: classificação antes da guarda | RC 1; 1 FAIL, 0 ERROR | memória NB:236 |

`LINHAS_REGISTRADAS` passou sem ajuste (`core/tests/test_medida_turno_do_par.py:539`). O alvo do marcador permanece igual (`ponto/tests/x1_mutantes.py:24`).

Comandos dos testes e do mutante de antecipação: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria -v

PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e3_pos_suite_sem_banco -v

E3_NB_MUTANTE=classificacao_antes_guarda PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria.ImpressaoNuncaBateuMemoriaTest.test_F3_pulada_nao_classifica_nem_carrega -v
```

Comando da medida, conforme procedimento sem Django de `core/tests/test_medida_turno_do_par.py:20`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import pathlib, types, unittest
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
suite = types.ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(p.resolve()) [nao verificado]
source = p.read_text().replace('from django.test import SimpleTestCase',
                              'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(p), 'exec'), suite.__dict__) [nao verificado]
unittest.main(module=suite, verbosity=2) [nao verificado]
PY
```

Para reproduzir catraca, RED e os dois mutantes, execute o bloco abaixo com `CASO=green`, `CASO=base`, `CASO=indireto` e `CASO=sem_porta`, respectivamente. As asserções originais são extraídas de `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:200`; as mutações ficam em memória.

```bash
CASO=green PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import ast, os, subprocess, unittest
from pathlib import Path
from unittest.mock import patch

p = Path('ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py')
nomes = {'DECLARADOS', '_chamadas_do_supra', 'CatracaProdutoresDoDictTest'}
nodes = [n for n in ast.parse(p.read_text()).body
         if getattr(n, 'name', None) in nomes or isinstance(n, ast.Assign)
         and any(isinstance(t, ast.Name) and t.id in nomes for t in n.targets)]
ns = {'ast': ast, 'APP': Path.cwd(), 'SimpleTestCase': unittest.TestCase}
exec(compile(ast.Module(body=nodes, type_ignores=[]), str(p), 'exec'), ns)
caso = os.environ['CASO']
alvo = Path('ponto/services/cartorio.py').resolve()
ler = Path.read_text
fonte = ler(alvo)
if caso == 'base':
    fonte = subprocess.run(
        ['git', 'show', '769ed8cfb0cecf6de81bcfb5c7441a4668b87353:app/ponto/services/cartorio.py'],
        check=True, capture_output=True, text=True).stdout
elif caso != 'green':
    antes = '                    com_insumo_dia_sem_trabalho(_dia_nb, trabalha=cel.trabalha, ata=_ata_nb), marcos,'
    depois = {'indireto': '                    _insumo_nb, marcos,',
              'sem_porta': '                    _dia_nb, marcos,'}[caso]
    assert fonte.count(antes) == 1
    fonte = fonte.replace(antes, depois, 1)
with patch.object(Path, 'read_text',
                  lambda p, *a, **kw: fonte if p.resolve() == alvo else ler(p, *a, **kw)):
    r = unittest.TextTestRunner(verbosity=2).run(
        unittest.defaultTestLoader.loadTestsFromTestCase(ns['CatracaProdutoresDoDictTest']))
print(f'CASO={caso} TESTES={r.testsRun} FAIL={len(r.failures)} ERROR={len(r.errors)}') [nao verificado]
raise SystemExit(0 if r.wasSuccessful() else 1)
PY
```

Ruff: comando tentou executar, retornou RC 1 com `No module named ruff`; análise **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ruff check --no-cache ponto/services/cartorio.py
```

Suite inteira e opus final nesta volta: **[nao verificado]**. [nao verificado]

## 11/11 06:04 · sessao cd87c28e1945 · FIO-E0-V4-E4B · construtor · origem=Code

Corrigi o enxerto dos mutantes, mas **o item 1 permanece incompleto**: não cortei o custo por S143 nem aumentei os tetos para encobri-lo. A pendência está registrada junto aos números. [test_selo_chokepoint_escrita.py:183](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/tests/test_selo_chokepoint_escrita.py:183)

Por sítio:

| Sítio | Delta: 1 / 3 chamados | Resultado |
|---|---:|---|
| `um_colaborador_por_identidade` | 0 / 0 diretamente | Troca instâncias; não consulta. [juizes.py:1121](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/juizes.py:1121) |
| `trava_na_ordem` no wrapper | 0 / 0 adicional | Há uma chamada por entrada; as linhas diferentes da base/head não representam duplicação. [disputa_emissao.py:1878](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1878) |
| Nova carga `_ancoradas` | +2 / +4 | Continua uma consulta por S143. Sua lista não substitui S128: os universos diferem quanto a validadas, carimbadas e disputa fechada. [models.py:344](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:344), [disputa_emissao.py:1904](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1904) |
| Residual | +1 / +1 | Atribuição à cadeia C6→tranca **[nao verificado]**. Acrescentei pareamento por função à sonda. [e4b_c4_medir_queries.py:58](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_medir_queries.py:58) |

`sem_cerca` e `sem_rollback` copiavam os globais antes da instalação do spy. O corpo mutado chamava `_executar_dia` antigo, sem executar a cascata adversarial instalada depois. Agora o enxerto conserva os globais vivos; mantive `call_count == 1` e as precondições de ressuscitação. [e4b_c4_base.py:12](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_base.py:12), [test_e4b_c4_orm.py:57](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:57), [test_e4b_c4_orm.py:133](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:133)

O mapa economizou **zero no chokepoint fornecido: 214/214**. Preparei outro ensaio, com pergunta viva nos dois laços e teto pareado medido durante o teste. Ele exige diferença antes de aceitar a morte do mutante; isso ainda não prova economia em `registrar_batida`. Execução ORM **[nao verificado]**. [test_e4b_c4_orm.py:156](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:156) [nao verificado]

O RED do spy passou; `globais_copiados` morreu na assertion-alvo. O protocolo passou; `mapa_falha_aceita` morreu por aceitar resultado sem diferença comprovada. [test_e4b_c4_base.py:17](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_base.py:17), [test_e4b_c4_orm_protocolo.py:77](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm_protocolo.py:77) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1

git diff -- .

python3 -m chamados.tests.test_e4b_c4_base
python3 -m chamados.tests.test_e4b_c4_base --mutante globais_copiados

python3 -m chamados.tests.test_e4b_c4_orm_protocolo
python3 -m chamados.tests.test_e4b_c4_orm_protocolo --mutante mapa_falha_aceita
```

Comandos ORM para a sessão; o executor mantém GREEN como condição para contar mutantes. [e4b_c4_verificar.py:67](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_verificar.py:67)

```bash
export PYTHONDONTWRITEBYTECODE=1

python3 -m chamados.tests.e4b_c4_verificar --orm

c4_orm=chamados.tests.test_e4b_c4_orm.CercaC4ORMTest

for c4_mutante in sem_cerca sem_rollback sem_alvo; do
  E4B_C4_MUTANTE="$c4_mutante" python3 manage.py test \
    "$c4_orm.test_RED_Ch12_P90_D90_fora_da_competencia_K6_false_desfaz" --noinput
done

for c4_mutante in foto_so_dia foto_antes_C; do
  E4B_C4_MUTANTE="$c4_mutante" python3 manage.py test \
    "$c4_orm.test_RED_foto_inteira_sob_C_e_reversao_antes_do_primeiro_ato" --noinput
done

E4B_C4_QUERY_MUTANTE=tranca_sem_mapa python3 manage.py test \
  "$c4_orm.test_RED_mapa_comum_economiza_query_entre_dois_selects" --noinput

for c4_modo in base head; do
  E4B_C4_QUERIES="$c4_modo" python3 manage.py test \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_gate_da_premissa_fecha_com_dia_e_celula \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_gravar_estado_sem_pergunta \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_registrar_batida_com_1_furo_vivo_que_ela_fecha \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_registrar_batida_com_3_furos_vivos_que_ela_fecha \
    --noinput
done
```

## 11/11 06:02 · sessao 70e72c812f20 · - · - · origem=Ronald (tmux)

Não achei corte seguro das cargas `_ancoradas` com as restrições atuais:

- A API devolve uma `list` já avaliada, sem validadas e incluindo carimbadas/disputas fechadas; S128 tem universo diferente. Fundir perde casos; complemento mantém duas consultas. `chamados/models.py:336`, `chamados/services/disputa_emissao.py:1904`.
- A carga anterior do gate não chega ao serviço: `_f1_ancoradas` é limpa antes do `save` que dispara S143. `chamados/models.py:216`, `chamados/models.py:296`.
- O identity map não consulta diretamente; o custo pode vir do consumidor C6 e do memo por instância fora de request. `chamados/juizes.py:1134`, `chamados/juizes.py:1054`.
- O wrapper contém uma chamada de trava por entrada, seguida de uma chamada do corpo. O receiver contém uma chamada do wrapper. `chamados/services/disputa_emissao.py:1878`, `chamados/signals.py:115`.

Existe consulta por pergunta viva em `reabrir_credor_de_pergunta_viva`: recarrega o credor em `chamados/services/disputa_emissao.py:1826`. Passar o credor já conhecido poderia cortá-la, mas conservar essa instância entre perguntas exige provar frescor diante de reentrada por outro objeto; o próprio modelo documenta esse risco. `chamados/models.py:274`. Não recomendo esse corte sem RED adversarial.

Atribuir o residual +1 exclusivamente à tranca continua [nao verificado]. A cadeia nova alcança a tranca antes de julgar carimbo/célula, mas isso sozinho não demonstra o delta SQL observado. `chamados/services/disputa_emissao.py:1821`, `chamados/juizes.py:1329`.

## 11/11 05:50 · sessao 4cfcfad110c6 · FIO-E0-V4-E3 · construtor · origem=Code

Candidata implementada em cinco arquivos. **20 testes sem banco + 35 da medida passaram; sete mutantes morreram por asserção; Ruff limpo.** ORM, contagem SQL após a cura e certificação final: **[nao verificado]**. [nao verificado]

- **F1/F2:** o marcador deixou de consultar `_vd_nb`. Agora consulta o juiz existente de dia sem trabalho; `isento` continua entrando exclusivamente pela parte com horizonte `regra >= 4`. A classificação e `derivar_veredito` permanecem responsáveis pela lavra. [cartorio.py:1217](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1217), [cartorio.py:1232](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1232).
- **F3:** classificação, cadastro e carga de turnos ficaram depois da guarda, dentro de `apply_`. A impressão consulta somente a ata em memória e o juiz puro; isso elimina o ciclo impressão → classificação → impressão. `ponto/services/cartorio.py:1218`, `:1227`, `:1231`.
- **Registro:** somente a ocorrência movida passou de `1319` para `1321`. `core/tests/test_medida_turno_do_par.py:315`.
- **S2/S3:** adaptei o carregamento AST para fornecer os helpers reais novos; a asserção do hash legado e da cobertura cega permanece. `chamados/tests/test_e3_pos_suite_sem_banco.py:94`, `:139`.
- **X1:** atualizei somente o alvo textual de `sem_fonte_no_gancho`. `ponto/tests/x1_mutantes.py:24`.

Adotei “insumo já carregado pelo marcador” como o insumo de dia sem trabalho que o marcador anterior representava indiretamente. **A fonte agora distingue dia sem trabalho, em vez de todo veredito não acusador.** A equivalência do gancho distante para trabalho justificado, turno em curso e parcial sem resto fica **[nao verificado]**; o veredito continua passando pelo classificador e por `derivar_veredito`. `ponto/services/cartorio.py:1222`, `:1232`.

A hipótese causal procede no HEAD recebido: o veredito alimentava o marcador antes da guarda. A atribuição estática das seis queries adicionais é:

| Sítio | Queries inferidas |
|---|---:|
| Cadastro dos vínculos — `ponto/services/cartorio.py:1111` | 1 |
| Vínculos de `dias_encerrados` — `ponto/turnos.py:2149` | 1 |
| Batidas de `turnos_do_colab` — `ponto/turnos.py:1915` | 1 |
| Candidatos a vínculo — `escala/alimentacao.py:148` | 1 |
| Escala para ler a ata — `ponto/turnos.py:1559` | 1 |
| DNA da janela — `escala/utils.py:100` | 1 |

Isso é **inferência do código, não captura SQL**. `ata_do_dia`, `classificar_dia` e `derivar_veredito` são puros nesses sítios. `ponto/services/cartorio.py:701`, `ponto/supra_juiz.py:204`, `escala/models.py:1624`. Main=59 e HEAD recebido=65 vêm do raspado **[nao verificado nesta volta]**; resultado SQL após a cura: **[nao verificado]**. O teto 59 permanece em `ponto/tests/test_selo_chokepoint_escrita.py:118`.

O diff completo pode ser obtido sem escrita:

```bash
git diff -- ponto/services/cartorio.py ponto/tests/x1_mutantes.py core/tests/test_medida_turno_do_par.py chamados/tests/test_e3_pos_suite_sem_banco.py
git diff --no-index -- /dev/null ponto/tests/test_e3_nb_impressao_memoria.py
```

Os REDs e mutantes estão em [test_e3_nb_impressao_memoria.py:25](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:25). Executei: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest \
  ponto.tests.test_e3_nb_impressao_memoria \
  chamados.tests.test_e3_pos_suite_sem_banco -v
```

Resultado: 20 testes, RC=0. A execução da medida usa o procedimento sem Django documentado em `core/tests/test_medida_turno_do_par.py:20`; resultado: 35 testes, RC=0.

Comandos individuais reproduzíveis; base e mutantes abaixo retornaram RC=1 por asserção, sem ERROR:

```bash
E3_NB_TEST=ponto.tests.test_e3_nb_impressao_memoria.ImpressaoNuncaBateuMemoriaTest

PYTHONDONTWRITEBYTECODE=1 E3_NB_BASE=9710c1733f2b2e18f9a397b525875d631d94c5f0 python3 -B -m unittest \
  "$E3_NB_TEST.test_F1_costura_do_isento_e_unica" \
  "$E3_NB_TEST.test_F2_comp09_nao_rejulga" \
  "$E3_NB_TEST.test_F3_pulada_nao_classifica_nem_carrega"

PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=isento_no_marcador python3 -B -m unittest "$E3_NB_TEST.test_F1_costura_do_isento_e_unica"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=sem_fonte python3 -B -m unittest "$E3_NB_TEST.test_folga_vai_pelo_juiz_e_guarda_a_fonte"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=literal_incondicional python3 -B -m unittest "$E3_NB_TEST.test_folga_vai_pelo_juiz_e_guarda_a_fonte"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=sem_isento python3 -B -m unittest "$E3_NB_TEST.test_isento_no_horizonte_rejulga"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=sem_horizonte python3 -B -m unittest "$E3_NB_TEST.test_F2_comp09_nao_rejulga"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=cobertura_cega python3 -B -m unittest "$E3_NB_TEST.test_B5u_cobertura_visivel_rejulga"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=classificacao_antes_guarda python3 -B -m unittest "$E3_NB_TEST.test_F3_pulada_nao_classifica_nem_carrega"
```

Labels ORM exatos para a sessão, **não executados aqui [nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci \
  ponto.tests.test_e3_isento_na_impressao.RamoNuncaBateu.test_a_costura_MORDE \
  ponto.tests.test_e3_isento_na_impressao.RamoNuncaBateu09.test_isento_comp_09_nao_rejulga \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_gate_da_premissa_fecha_com_dia_e_celula

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci \
  ponto.tests.test_e3_isento_na_impressao \
  ponto.tests.test_selo_b5u_nunca_bateu_ve_ausencia \
  ponto.tests.test_x1_folga_sem_adesao \
  ponto.tests.test_selo_chokepoint_escrita \
  core.tests.test_medida_turno_do_par
```

X1 completo, base e dois mutantes ORM, **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo green
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo base
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo mutante --mutante literal_incondicional
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo mutante --mutante sem_fonte_no_gancho
```

Censo: os chamadores de produção de `impressao_insumos` continuam sendo os dois ramos do cartório, com reexportação pelo command. `ponto/services/cartorio.py:1220`, `:1361`; `ponto/management/commands/processar_cartorio.py:20`. `_vd_nb` é consumido localmente pela renomeação, lavra e fatos do status; `sem_adesao` é consumido pelo hash. `ponto/services/cartorio.py:525`, `:1252`, `:1255`, `:1260`.

Os demais leitores são testes; o censo integral, incluindo menções documentais, é reproduzível com: [nao verificado]

```bash
rg -n '\bimpressao_insumos\b|\b_vd_nb\b|sem_adesao=' \
  --glob '*.py' --glob '!**/migrations/**'
```

Ruff passou nos cinco arquivos alterados com `check --no-cache`; `git diff --check` também passou. A certificação da cura permanece com a suite e o opus final **[nao verificado]**. [nao verificado]

## 11/11 05:40 · sessao 0e7c4b4de1ab · - · - · origem=Ronald (tmux)

A cura com `dia_sem_trabalho` mantém F1/F2/F3 e a classe de folga, mas **estreita o gancho de adesão distante**: trabalho justificado, turno em curso e parcial sem resto também podem receber `concorde` pela classificação atual sem serem dia sem trabalho. Nesses casos o marcador anterior era verdadeiro; o juiz puro responderia falso (`ponto/supra_juiz.py:204`, `ponto/supra_juiz.py:369`, `chamados/juizes.py:441`).

Há colisão entre os argumentos já presentes no hash: X1 cria trabalho e folga com o mesmo DNA, sem batida/cobertura e na mesma regra; B5 exige impressão legada no trabalho, enquanto X1 exige fonte distinta na folga (`ponto/tests/test_x1_folga_sem_adesao.py:56`, `ponto/tests/test_x1_folga_sem_adesao.py:134`, `ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:129`). Portanto um marcador exclusivamente desses argumentos não distingue os dois.

Preservar a classificação ampla sem queries exigiria expor carga e grade anterior à isenção: os vínculos já existem em `rows`, as células em `_cels_gr`, e a isenção é aplicada posteriormente (`escala/utils.py:1296`, `escala/utils.py:1308`, `escala/utils.py:1850`). Apenas passar `isento_ponto=False` não elimina o efeito prévio da isenção sobre as células da grade (`escala/utils.py:855`).

Não escrevi arquivos; resultados ORM e contagem real de queries: [nao verificado].

## 11/11 05:10 · sessao 7f992e0a1aee · FIO-E0-V4-E4B · construtor · origem=Code

Entregues o [diff](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.diff:1) e o [recibo com comandos de cada teste e mutante](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:124). [nao verificado]

- **Fixture:** julho ficava abaixo do piso automático. A entrada passou para 21/09, competência 10, mantendo as precondições fortes. [Rastro da causa](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:7). [nao verificado]
- **S143:** mantido `+1` justificado por chamado; fundir perderia validadas do S128. Os dois laços agora compartilham o mapa canônico do colaborador. [Justificativa por sítio](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:53).
- **Sonda:** modo base usa o arquivo inteiro do main `fb9269ea9`, incluindo reexports em memória. [Implementação](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_base.py:9).

Observado localmente: 59+2 testes de memória e 40 selos passaram; os mutantes novos caíram nas assertions indicadas. [Registro](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:124). [nao verificado]

Tetos `60/28/213/269` são **deduzidos, a remedir**. GREEN ORM, mortes ORM e opus: **[não verificado]**. [Comandos pela trava](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:162).

## 11/11 05:00 · sessao c34bbb0cd360 · - · - · origem=Ronald (tmux)

A fusão fiel não cabe na porta atual:

- Main `fb9269ea9:app/chamados/services/disputa_emissao.py:1823` já consulta perguntas com destino no chamado, `materializada_em=NULL` e disputa aberta; inclui validadas.
- `_ancoradas` retorna `list`, exclui validadas e inclui outros universos (`chamados/models.py:336`, `:344`). Não fornece QuerySet para `union`.
- Perder validadas altera comportamento: C6 dá morta (`chamados/juizes.py:1325`), mas `carimbar` pode escrever materialização/via e salvar (`chamados/models.py:3126`, `:3152`). Também altera disputas candidatas (`chamados/services/disputa_emissao.py:1977`, `:2000`).
- Buscar complemento de validadas ou usar prefetch conserva duas cargas. Portanto permanece justificável **+1 por S143**, sem duplicar o predicado canônico (`chamados/models.py:344`).

Há reutilização canônica possível da tranca:

```python
colab = chamado._state.fields_cache.get('colaborador')
mapa = {chamado.colaborador_id: colab} if colab is not None else {}
```

Passar `mapa` aos **dois** `um_colaborador_por_identidade` evita criar donos separados do memo; a função aceita mapa sem consultar banco (`chamados/juizes.py:1121`, `:1134`). Seed apenas da FK quente evita adicionar consulta em chamados sem perguntas. Memo da tranca pertence ao request/instância (`chamados/juizes.py:1054`).

Antes de reconciliar na batida, cartório chama E2 (`ponto/services/cartorio.py:969`, `:974`), que usa C6 e tranca sobre o `colab` recebido (`chamados/reconciliador.py:650`, `:654`, `:661`). Contudo a consulta de chamados não aquece explicitamente `ch.colaborador` com essa mesma instância (`ponto/services/cartorio.py:117`). Compartilhar mapa entre os dois laços remove sua duplicação; aproveitamento do memo anterior e redução exata são **[não verificado]**.

Tetos previstos se cair o único `+1 periodo_trancado`: **60/28/213/269 [não verificado]**. Permanecem incrementos main→head **+1/+1/+2/+4**, compatíveis com os sítios S143 do `delta_main_head.txt` fornecido.

Ao ajustar assinatura do identity map nas chamadas, atualizar o double `lambda ps: ps` (`chamados/tests/test_e4b_classe_e_memoria.py:291`) para aceitar `mapa=None`; caso contrário a suíte de memória falha antes da assertion-alvo.

## 11/11 04:59 · sessao 919502c461f8 · - · - · origem=Ronald (tmux)

Causa da fixture: julho pertence à competência 07, abaixo do piso automático `2026-09`; `forcar=True` apenas força julgamento e não equivale a `toque=True`. O cartório cala a emissão do chamado, enquanto a fábrica ainda emite a pergunta (`ponto/services/cartorio.py:270`, `:312`, `:1687`, `:1695`, `:1707`, `:1773`, `:1008`).

Caminho real:

- `registrar_batida` cria a batida (`ponto/registro_batida.py:170`).
- O signal julga o dia anterior e o próprio dia (`ponto/signals.py:108`, `:131`).
- `julgar_celula` chama `julgar_colab`, que chama a fábrica (`ponto/services/cartorio.py:2000`, `:1008`).
- A fábrica chama o emissor (`chamados/services/fabrica_da_celula.py:172`).
- O emissor deriva o destino; zero candidatos retorna `(None, None)` (`chamados/services/disputa_emissao.py:411`, `:238`).
- O reatamento automático também não salva julho: passa por `reatar_pergunta_orfa` e pelo c2, que barra nascimento abaixo do mesmo piso (`chamados/services/disputa_emissao.py:459`; `chamados/reconciliador.py:1621`, `:828`, `:840`).

Entrada coerente proposta: construir o segundo fio em **21/09/2026**, competência **10/2026**, mantendo o alvo do backfill em 09/2026. Essa data continua passada no freeze de 25/09, fica fora do alvo e dentro do horizonte automático. Adaptar a exportação e a expectativa BANG para 10/2026 (`chamados/tests/test_e4_backfill.py:28`, `:49`; `ponto/services/cartorio.py:320`).

Isso preserva a precondição forte e evita criar ou alterar diretamente chamado, pergunta, veredito ou carimbo históricos. Alcance efetivo e execução ORM com essa entrada: [não verificado].

## 11/11 04:57 · sessao 18b1b34d5d9d · FIO-E0-V4-E3 · construtor · origem=Code

Entregues o [diff](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite.diff:1) e o [relato S1–S7, com causas, cura e censo](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_pos_suite.md:7).

Verificação local: [46 testes sem banco verdes](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_vizinhos.txt:2), [selos AST verdes](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_green.txt:12), [mutantes mortos](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_pos_suite.md:35) e [Ruff limpo](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_ruff.txt:1). [nao verificado]

[Comandos de RED, mutantes e labels ORM](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_comandos.txt:1).

ORM, 11 testes X1, suíte inteira e opus final: **[não verificado]**. [nao verificado]

## 11/11 04:37 · sessao e7d57b8fb5e2 · - · - · origem=Ronald (tmux)

Causa S2/S3: o commit `6c1fae937` acrescentou `sem_adesao:1` ao hash de todo o ramo sem adesão (`ponto/services/cartorio.py:522–526`, `:1212–1214`). O B5-u constrói deliberadamente a impressão anterior sem esse marcador (`ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:129`): isso rompe a idempotência em `:142` e faz o mutante cego relavrar em `:185`.

Cura sugerida: calcular `_ata_nb`, `_cods_nb` e `_vd_nb` antes da impressão; acrescentar o marcador somente quando:

```python
sem_adesao=not _coberto_inteiro_nb and _vd_nb not in cel.VEREDITOS_ACUSAM
```

Assim a seleção usa `derivar_veredito`, já chamado em `ponto/services/cartorio.py:1248`, sem predicado próprio de folga. Trabalho acusador e cobertura integral conservam a impressão anterior; folga absolvida sem cobertura conserva o marcador necessário ao C3 (`ponto/tests/test_x1_folga_sem_adesao.py:116–146`).

Não basta selecionar todo veredito não acusador: ausência aprovada também ficaria marcada, e o B5-u cego continuaria divergindo. Usar o veredito persistido também falha: a primeira lavra mudaria vazio para `concorde`, rompendo C5 (`ponto/tests/test_x1_folga_sem_adesao.py:172–190`).

Atualizar somente a âncora textual de `sem_fonte_no_gancho` no executor de mutantes para a expressão nova; conservar o teste C3 (`ponto/tests/x1_mutantes.py:23–26`).

Nenhum arquivo alterado; execução ORM e resultado da sugestão [não verificado].

## 11/11 04:37 · sessao 6804993530b2 · - · - · origem=Ronald (tmux)

S1 nasceu antes do c8c: o histórico do teste aponta `ddf5980ac`/`dc0ecbdeb`; o c8c `6b8898fdc` ampliou o universo para todas as perguntas elegíveis (`chamados/services/tarefa_do_dia.py:492–506`). O selo exige zero chamadas ao C6 e uma leitura do painel naquele cenário (`chamados/tests/test_e3_portas_tarefa.py:118–126`).

A cura mínima é decidir **por dia** se existe consumidor de `viva_c6`:

- Dia sem trabalho: precisa, porque `expor_admin` é chamado pelos marcos dispensados (`tarefa_do_dia.py:260–268`) e A/S consulta todas as perguntas (`:327–328`).
- Motivo fora do mapa ou posição ausente da grade: precisa (`:314–319`).
- Trabalho com todas as perguntas dentro da grade: dispensa C6; esse é o cenário S1 (`chamados/tests/e3_fixtures.py:41–48`, `:65–72`).

Quando precisar, preservar **todas as perguntas daquele dia**, evitando restaurar o filtro individual que o c8c curou. A posição ausente precisa continuar recebendo vida (`chamados/tests/test_e3_c8c_sem_banco.py:40–64`; `test_e3_c8c_orm.py:35–49`).

Não usar `q_vivas` como C6 exato: seu contrato é continência, explicitamente não igualdade (`chamados/models.py:3336–3345`). O painel só entrega verbo/fila (`chamados/juizes.py:1475–1493`); seu crédito usa esse pré-filtro generoso (`chamados/services/raiox.py:74`).

Para S4–S6, o recorte deve devolver valores dentro de `juizes.py`, consumidos como locais por `_pergunta_viva_calc`, sem copiar nem atribuir campos de PerguntaDisputa. Os pontos são `juizes.py:1346`, `:1357`, `:1362`, `:1383`, `:1389`, `:1421`. Materialização futura deve resultar em ausência de carimbo efetivo antes de alcançar o juiz do lastro (`:1421–1429`). Atenção: `_resposta_concorda_com_a_luz` lê resposta (`:1323`) e a carga pré-calcula slots usando a pergunta (`chamados/services/carga_pergunta_viva.py:119`); resposta futura não pode vazar por esses caminhos.

## 11/11 04:18 · sessao a480d6252eb2 · FIO-E0-V4-E4B · construtor · origem=Code

Entreguei o [diff](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.diff>) e o [recibo com causas F1–F8 e comandos individuais](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:6>).

- **F1–F2:** fixture reescrita pelas portas reais, com pré-condições explícitas. [Código:56](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:56>)
- **F3:** caracterização declarada da recusa de nascimento sem dia. [Código:206](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:206>)
- **F4:** `1 != 0` significava reversão ausente; o teste agora exige foto integral e reversão antes do primeiro ato. [Código:145](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:145>) [nao verificado]
- **F5–F8:** comparação estática encontrou consultas substituídas; tetos ajustados com C8 e sonda BASE/HEAD preparada. [Análise:48](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:48>)

Passaram **52 testes em memória, 40 selos e 5 testes do executor**; dois mutantes do executor caíram por assertion. [Resultados e comandos:104](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:104>) [nao verificado]

**GREEN ORM, números da base e cinco mortes ORM: [nao verificado]. Ruff limpo: [nao verificado] — ferramenta ausente.** [Pendências e comandos pela trava:130](</home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v2.txt:130>)

## 11/11 04:06 · sessao 044058884828 · - · - · origem=Ronald (tmux)

F1–F4 compartilham uma fixture que interfere no próprio dia-alvo: Ch12 nasce ligado a `self.cel` de setembro, e o `post_save` julga esse dia imediatamente, antes de montar P90/D90 de julho. A chamada extra de ressuscitação só ocorre dentro de `_executar_dia`; o teste não demonstra que esse hook foi alcançado. [chamados/tests/test_e4b_c4_orm.py:58](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:58), [chamados/signals.py:187](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/signals.py:187), [chamados/tests/test_e4b_c4_orm.py:90](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:90). [nao verificado]

O `1 != 0` de F4 corresponde a **uma reversão esperada, nenhuma capturada**, pela asserção `len(fotos)`, não à sobra de uma linha. A reversão só é gravada havendo itens executáveis. [chamados/tests/test_e4b_c4_orm.py:148](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:148), [backfill_fio_morto_e4b.py:258](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:258).

Proposta concreta para a fixture:

- Montar primeiro o dia de julho, com **batida S real sem E**, usando `registrar_batida`, e pedir julgamento/fábrica; selecionar P90, D90 e seu Ch12 efetivamente produzidos. Isso evita transformar setembro durante a montagem. A fábrica se abstém para `nunca_bateu`, portanto a batida real fornece a adesão necessária. [fabrica_da_celula.py:183](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fabrica_da_celula.py:183).
- Fechar D90 pela porta `d90.fechar(user, motivo=FECHOU_ADMIN_ENCERRA, resolucao=...)`, que carimba a muda e grava o fechamento. Não fabricar carimbo/fechamento por `.update`. [chamados/models.py:1918](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1918).
- Antes do ensaio, exigir que `ch12.perguntas_a_ressuscitar()` contenha P90. A porta exclui apenas as vias fora do fato; testa C6 na cópia descarimbada e reabre a disputa após ressuscitar. [chamados/models.py:1483](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1483), [chamados/models.py:1506](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1506), [chamados/models.py:1573](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:1573).
- Exigir separadamente `call_count` do hook e quantidade ressuscitada; assim falha de alcance não se confunde com falha da porta. O teste atual só verifica `any(nascimentos)`. [chamados/tests/test_e4b_c4_orm.py:99](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:99). [nao verificado]

Há duas caracterizações necessárias: o elo datado setembro→julho é recusado pela porta de nascimento via `resolvedora_valida`, e `PerguntaDisputa.abrir(data_evento=None)` levanta. Portanto a indeterminada atual, criada por `.update(data_evento=None)`, não representa saída dessas portas. [chamados/models.py:2879](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:2879), [chamados/models.py:2787](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:2787), [chamados/tests/test_e4b_c4_orm.py:83](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:83).

Nenhum arquivo alterado; a proposta e os cinco mutantes ORM permanecem **[nao verificado]**.

## 11/11 04:04 · sessao 945ef0509c6f · - · - · origem=Ronald (tmux)

F5–F8: a c4 **substituiu consultas**, sem acrescentar uma consulta nos dois sítios apontados:

- HEAD `disputa_emissao.py:1799`: `_ancoradas()` substitui a consulta por destino não validado da base `b20a37f3e:app/chamados/services/disputa_emissao.py:1789`.
- HEAD `disputa_emissao.py:1915`: `_ancoradas()` substitui a consulta a todas as perguntas de disputas abertas do colaborador da base `b20a37f3e:app/chamados/services/disputa_emissao.py:1907`.
- Os quatro tetos são idênticos na base e em HEAD: `ponto/tests/test_selo_chokepoint_escrita.py:116`, `:118`, `:161`, `:162`.

O caminho é fechamento → S143 (`chamados/signals.py:96`, `:114`) → resolver (`chamados/services/disputa_emissao.py:1878`) → consulta canônica (`:1915`). `_ancoradas()` executa uma consulta por chamada, inclusive vazia: `chamados/models.py:336–347`. Os números efetivos na base permanecem **[nao verificado]**, pois não executei ORM/banco.

Não recomendo simplesmente usar `_ancoradas()` também como `perguntas`: a primeira exclui validadas (`chamados/models.py:344`); S128 inclui perguntas validadas com `materializada_em=NULL` em disputa aberta (`chamados/services/disputa_emissao.py:1904–1908`). Fundir ingenuamente enfraquece o universo.

Além disso, o RED atual exige uma chamada à origem `_ancoradas()` (`chamados/tests/test_e4b_classe_e_memoria.py:384–388`). A alternativa conservadora é manter a consulta canônica necessária e justificar C8 ao lado dos tetos, deixando os números da base explicitamente pendentes da execução pela trava.

## 11/11 04:00 · sessao 9323309d2863 · FIO-E0-V4-E3 · construtor · origem=Code

Aplicadas as duas correções de teste: [X1:129](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:129) e [c8c:40](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_c8c_orm.py:40). [nao verificado]

```diff
--- a/app/ponto/tests/test_x1_folga_sem_adesao.py
+++ b/app/ponto/tests/test_x1_folga_sem_adesao.py
@@ -128,2 +128,3 @@
             out = self._julgar()  # proximo julgamento, SEM forcar
+        self.assertIn('cods', out, 'C3: a folga foi pulada em vez de rejulgada apos a adesao')
         self.assertIn(self.d, out['cods'], 'C3: perdeu rejulgamento ao perder a palavra NB')

--- a/app/chamados/tests/test_e3_c8c_orm.py
+++ b/app/chamados/tests/test_e3_c8c_orm.py
@@ -39,3 +39,8 @@
         agora = _ts(D, 21)
+        carga = carga_da_tarefa([self.c], D, D, agora)
+        carga._ler_vida()
+        self.assertIn(p.pk, [q.pk for q in carga._perg.get((self.c.pk, D), ())
+                             if hasattr(q, 'viva_c6')],
+                      'a pergunta fora da grade tambem precisa receber a vida do C6')
         avulso = tarefa_do_dia(self.c, D, agora)
-        lote = carga_da_tarefa([self.c], D, D, agora).tarefa(self.c, D)
+        lote = carga.tarefa(self.c, D)
```

`_julgar()` devolve o `out` de `julgar_celula`, após atualizar a célula (`ponto/tests/test_x1_folga_sem_adesao.py:59`). No rejulgamento normal, esse dicionário recebe `cods[data]`; quando a célula é pulada, incrementa `pulados` sem produzir essa chave (`ponto/services/cartorio.py:1436`, `:1372`, `:1914`).

Escolhi **(a)** no c8c. Sim: o mutante exclui a pergunta de `candidatas`, mas ela permanece em `_perg`; sem passar pelo escritor de `viva_c6`, chega ao leitor de `:245`. O leitor de `:328` depende do ramo `fato_em_ausencia`. A nova assertion verifica a alimentação antes desses consumidores, preservando as assertions finais (`chamados/tests/e3_c8c_mutantes.py:17`; `chamados/services/tarefa_do_dia.py:487`, `:506`, `:536`, `:245`, `:328`; `chamados/tests/test_e3_c8c_orm.py:42`).

| Mutante abrangido | Teste que deve cair por FAIL | [nao verificado]
|---|---|
| `sem_fonte_no_gancho` | `FolgaSemAdesaoTest.test_c3_primeira_batida_viva_distante_rejulga_folga` — assertion em `ponto/tests/test_x1_folga_sem_adesao.py:129` |
| `universo_incompleto` | `C8cCargaReal.test_RED_posicao_sumiu_avulso_e_lote_reais` — assertion em `chamados/tests/test_e3_c8c_orm.py:42` |

Os harnesses permanecem sem prefixo `test_`; X1 continua exigindo GREEN com 11 testes (`ponto/tests/x1_mutantes.py:79`; `chamados/tests/e3_c8c_mutantes.py:66`).

Comandos individuais, **somente no ambiente isolado da sessão**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.x1_mutantes --modo mutante --mutante sem_fonte_no_gancho
PYTHONDONTWRITEBYTECODE=1 E3_C8C_MUTANTE_ORM=universo_incompleto python3 manage.py test chamados.tests.test_e3_c8c_orm.C8cCargaReal --noinput -v 2
```

Execuções pela trava, conforme os caminhos fornecidos; conteúdo dos scripts [nao verificado]:

```bash
PYTHONDONTWRITEBYTECODE=1 bash logs/FIO-E0-V4-E3/x1_rodar_mutantes.sh
PYTHONDONTWRITEBYTECODE=1 bash logs/FIO-E0-V4-E3/c8c_rodar_mutantes_orm.sh
```

Ruff limpo **[nao verificado]**: o comando abaixo retornou `No module named ruff`. A execução dos testes ORM alterados fica pendente da sessão **[nao verificado]**. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py chamados/tests/test_e3_c8c_orm.py
```

## 11/11 03:57 · sessao 4d4630f2ce92 · FIO-E0-V4-E4B · construtor · origem=Code

C4 escrita. Execuções locais: **52 testes em memória, 40 selos e 43 ensaios individuais de mutação, rc=0**. ORM, tetos e produção continuam **[nao verificado]**; certificação fica com Code e opus. [Recibo e resultados](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:13). [nao verificado]

| Item | Arquivo e entrega | RED / mutante / rc |
|---|---|---|
| 1 | [Harness:135](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_classe_e_memoria.py:135): atributo do pacote pai, módulos reais com sentinela, restauração e coletor | RED=1; GREEN=0; mutantes exigem assertion, rc=0 |
| 2 | [disputa_emissao.py:1799](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1799): `_ancoradas()` por destino unida à ressuscitação | RED na base=1; `D3_volta_segunda_copia`=0 |
| 3 | [Backfill:183](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/management/commands/backfill_fio_morto_e4b.py:183): protocolo, foto integral sob C e cerca antes do commit | REDs e cinco mutantes ORM preparados; rc **[nao verificado]** | [nao verificado]
| 4 | [foto_e4b.py:18](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:18): fora do alvo recusa mesmo com K6=False; BANG identifica competência/linha | Mutantes de alvo, exportada e indeterminada: rc=0; ORM **[nao verificado]** |
| 5 | [disputa_emissao.py:1915](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1915): evento lê `_ancoradas()` | RED de seleção=1; mutante=0; **número de queries [nao verificado]** |
| 6 | [Terminal:211](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_o261e_f1_porta_encerramento.py:211): fechado gravado, reabertura com nota e contraexemplo administrativo | REDs/mutante ORM: **[nao verificado]** |
| 7 | [foto_e4b.py:60](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/foto_e4b.py:60): DIFF por colaborador, nascimentos, push e dinheiro da autoridade | REDs=1; mutantes DIFF/push/dinheiro=0; ORM **[nao verificado]** | [nao verificado]
| 8 | [Testemunhas:688](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_classe_e_memoria.py:688): mesma recusa, motivo/instante novos e limbo `31,39` | Três mutantes mortos por assertion; rc=0 |
| 9 | [Passivo:61](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_passivo.py:61): caracterização invertida; universo e soma preservados | REDs/mutante ORM: **[nao verificado]** |
| 10 | [Registros dos selos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:96): relógio, B2 e tratamento explícito do rollback | REDs=1; selos=0; selos mutados=1 |
| 11 | [BACKLOG.md:1875](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/BACKLOG.md:1875): `Parallel Change / Expand-Contract`, fase BACKFILL | RED=1; positivo=0; mutante=1 |

Comandos executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4b_classe_e_memoria chamados.tests.test_e4b_foto_memoria
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos
```

Os **43 comandos individuais**, com respectivos rc, estão no [recibo:198](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:198). Os quatro comandos de selos mutados, cada qual com rc=1 por assertion, estão no [recibo:105](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:105).

TestCase ORM entregues para o Code, com execução **[nao verificado]**:

- [CercaC4ORMTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:19): Ch12/P90/D90 fora do alvo, exportada, indeterminada, foto sob C e DIFF monetário.
- [F1HumanoETripwireTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_o261e_f1_porta_encerramento.py:192): terminal, admin sem fato novo e mutante.
- [PassivoE4bTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_passivo.py:24).
- [BackfillE4bTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:109).
- [RespostaVisivelTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_b2d_resposta_visivel.py:26).
- [SeloChokepointEscritaTest](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/tests/test_selo_chokepoint_escrita.py:218).

Nomes completos dos métodos e comandos ORM, incluindo mutantes e os quatro tetos: [recibo:111](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:111).

**QUEM ESCREVE / QUEM LÊ:** [censo por campo — 249 campos declarados mais PKs](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-campos.txt:6), complementado pelo [censo de efeitos e campos temporários](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4.txt:143). Escritores/leitores não identificados estão marcados **[nao verificado]**.

## 11/11 03:12 · sessao 2771d9db9c73 · - · - · origem=Ronald (tmux)

Exploração somente leitura; nenhuma execução ORM.

- Dinheiro online: [`ponto/services/fechamento.py:1735`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/fechamento.py:1735), `fechamento_lido(colaborador, mes, ano)`, chama o escritor existente em `somente_leitura=True`, sem salvar (`:1759`). `None` significa ausência de resposta e deve permanecer explícito (`:1747`).
- Leitores reais: [`ponto/views.py:993`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/views.py:993), `:1480`; [`relatorios/cartao_pela_celula.py:69`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/relatorios/cartao_pela_celula.py:69).
- DSR e faltas: fechamento recebe DSR em [`ponto/services/fechamento.py:1101`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/fechamento.py:1101), grava em `:1173–1179`. Autoridade das faltas por dia: [`folha/export.py:125`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/folha/export.py:125), já lida pelo escritor em `fechamento.py:873`.
- DiaPago: campos monetários declarados em [`ponto/services/dia_pago.py:34`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/dia_pago.py:34); escritor `lavrar`, grava dias e ajuste em `:345–360`. Fotografar linhas completas por colaborador, incluindo ajuste/versões; não somar novamente no command. O ajuste contém DSR (`:350–358`).
- Eventos de fechamento ficam em `on_commit`: [`ponto/services/fechamento.py:1798`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/fechamento.py:1798), `:1824`. Logo só comparar raw DiaPago/fechamento gravado dentro do DRY pode ocultar o dinheiro que mudaria após commit; incluir `fechamento_lido` antes/depois.
- Push silenciado: [`chamados/services/disputa_emissao.py:1742`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1742), emissor retorna em `:1753–1756`. Um coletor ContextVar no próprio ramo silenciado consegue contar tentativas por colaborador/disputa, inclusive agregações, sem executar push.
- Relógio: entrada a retirar é [`core/tests/testes_sem_relogio.txt:32`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/core/tests/testes_sem_relogio.txt:32), `test_b2d_resposta_visivel.py`. Só um método tem freeze hoje (`test_b2d_resposta_visivel.py:79`); `setUp` e demais métodos continuam sem congelamento (`:40`, `:67`, `:114`).
- B2: registrar `chamados/management/commands/backfill_fio_morto_e4b.py::Command.handle` na lista informativa [`core/tests/test_medida_turno_do_par.py:353`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/core/tests/test_medida_turno_do_par.py:353), em ordem. Não alterar números/critérios em `:100–101`; lista é informativa, conforme `:12–15`. Há receita sem Django/banco em `:20–69`.
- Padrão: corrigir a linha da obra em [`docs/BACKLOG.md:1875`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/BACKLOG.md:1875) para `padrao=Parallel Change / Expand-Contract, fase BACKFILL`, conforme [`docs/GLOSSARIO.md:18`](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/GLOSSARIO.md:18).

## 11/11 03:12 · sessao 92fe8513932e · - · - · origem=Ronald (tmux)

Leitura concluída, sem alterações nem execução ORM.

- **Trava D1:** `PerguntaDisputa.trava_na_ordem` aceita a coleção de colaboradores primeiro; depois, por colaborador, `abertas_do_colaborador=True` cobre disputas abertas e chamados vivos. A disputa fechada ressuscitada fica coberta pela trava C já adquirida. (`chamados/models.py:2629`, `:2652`, `:2664`, `:1578`.)
- **Fixture Ch12/P90/D90:** reutilizar `BaseE4b`; Ch12 vivo associado ao dia tratado, P90 ancorada nele mas com `data_evento`/célula de outro dia, D90 fechada. A cascata real chama `ressuscitar_perguntas` sem recorte de data; E2 também seleciona pelo colaborador inteiro. (`chamados/tests/test_e4_backfill.py:39`; `ponto/services/cartorio.py:1633`, `:970`; `chamados/reconciliador.py:627`.)
- **Terminal→terminal:** teste está em `F1HumanoETripwireTest.test_i_terminal_para_terminal_passa`. Inverter o resultado final para `em_analise`; exigir a sequência de auditoria `resolvido→fechado→em_analise`, nota E4B única e pergunta sem carimbo. A auditoria anterior ao save permite comprovar que `fechado` foi gravado antes da reentrada. (`chamados/tests/test_o261e_f1_porta_encerramento.py:211`; `chamados/models.py:292`.)
- **Contraexemplo admin:** utilizar `VIA_ADMIN`, dia explícito e `resolvido_em` posterior ao último fato; exigir `recusa:admin_sem_fato_novo`, credor terminal e nota de recusa. Fixtures existentes demonstram essa montagem. (`chamados/tests/test_reabrir_pergunta_a_premissa.py:45`, `:73`; `chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117`; `ponto/services/cartorio.py:329`.)
- **Tetos existentes:** reutilizar `SeloChokepointEscritaTest`, principalmente `test_gate_da_premissa_fecha_com_dia_e_celula`, `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` e `test_registrar_batida_com_3_furos_vivos_que_ela_fecha`; não alterar constantes. (`ponto/tests/test_selo_chokepoint_escrita.py:218`, `:345`, `:388`, `:393`.)

Resultados atuais e redução de queries: **[nao verificado]**.

## 11/11 03:09 · sessao 11b3832e7d26 · VERIFICADOR-EM-FABLE · censo · origem=Code

As referências aos anexos numeram o conteúdo colado, incluindo linhas vazias.

**O #3 exige classificação mista:** a falsa negativa estava perguntada e não foi identificada; chave desligada e filtro de tipo foram vistos e aceitos pela triagem. (`preverif1.q:4`; `preverif1.r:9`; `preverif1.r:11`; `preverif1.r:19`; `triagem.txt:3`)

**No #4, a atribuição à triagem é parcial:** pv1 apontou acúmulo sem janela e foi considerado PROCEDENTE. Depois, pv2 registrou a ressalva temporal, classificada como descritiva. A consequência específica “primeira ocorrência nova deixa vermelho para sempre” não aparece literalmente nas respostas; o código atual acrescenta observações e conta seus IDs sem filtro temporal. (`preverif1.r:3`; `triagem.txt:2`; `preverif2.r:5`; `triagem.txt:6`; `bin/disjuntor.py:2182`; `bin/disjuntor.py:2201`)

**O #5 não é (c):** a ausência de `linha()` e a primeira falha por `KeyError` foram identificadas por leitura/reprodução em memória, independentemente da execução integral da suíte. (`preverif3.r:33`; `preverif3.r:35`) [nao verificado]

**Há sobreposição entre (e) e (d) no pedido para o #6:** nenhum trecho pergunta ou responde sobre a adequação do padrão escolhido. Pela regra da cópia, seria **(e)**: MODO-COMUM exige que Codex **e opus** tenham deixado passar, enquanto o opus registrou esse achado. Pela definição ampliada de (d) no pedido, também caberia (d); não há desempate fornecido. (`preverif1.q:8`; `preverif2.q:9`; `preverif3.q:9`; `opus_final_v3.txt:7`; `CLAUDE.md:390`)

| Achado | Classe | Trecho q | Trecho r |
|---|---|---|---|
| **1 — prefixo confunde negativa sem obra com B3 de obra parada** | **(a), falha do Codex.** O tema dos efeitos da negativa estava presente. O código usa o prefixo ambíguo, extrai `B3` como regra e reconhece tokens de obra na mensagem. (`bin/disjuntor.py:1646`; `bin/disjuntor.py:262`; `bin/disjuntor.py:638`) | “A negativa nova pula algum efeito que antes acontecia nessa chamada (trilha, dispara, oks do CENSO, b4)? Isso importa?” (`preverif1.q:5`) | **nenhum.** A resposta apenas afirmou: “O registro externo `NEGOU`, resposta `deny` e tentativa de registro no RELATO continuam acontecendo.” Isso não identifica a colisão semântica. (`preverif1.r:23`) |
| **2 — mesmo nome para lista persistida e número retornado** | **(b), falha da triagem.** Motivo registrado: “FILA (nome), consumidores usam o numero.” (`triagem.txt:9`) | “Cada campo persistido (`b3_sem_obra`, `b3_sem_obra_obs`) tem agora UM significado, em todo escritor e leitor?” (`preverif3.q:4`) | “Atenção ao nome na saída: `ler()['b3_sem_obra']` continua sendo o **número derivado de observações**, não a lista histórica persistida.” (`preverif3.r:5`) |
| **3 — reconhecimento de final; chave; tipo do agente** | **(a)** para falsa negativa por mencionar leitura final; **(b)** para chave desligada/outra grafia e cobertura restrita por tipo. Motivo da triagem: “saida declarada do instrumento, fila.” (`triagem.txt:3`) | “E existe pedido NAO-final ou final COM obra legivel que agora passa a ser negado indevidamente?”; “chave desligada”; “subagent_type com outra grafia”. (`preverif1.q:4`) | **Falsa negativa: nenhum.** A resposta disse “não aparece falsa negativa nova dentro do contrato dos parsers.” (`preverif1.r:19`) **Demais partes:** “Com a chave desligada, `main_pretool` pula a decisão”; “Outra grafia de `subagent_type` e ausência de `tool_input` também não entram na negativa nova”; “com metadata `agentType == 'verificador'`”. (`preverif1.r:11`; `preverif1.r:9`) |
| **4 — observações acumulam sem janela nem baixa** | **(b), parcial**, pela ressalva temporal vista e tratada como descritiva; o BLOQUEIA original de pv1 foi aceito, não descartado. (`triagem.txt:2`; `triagem.txt:6`) | “O contador ‘esperado 0’ passa a contar so entradas novas, e o historico aparece separado sem sumir?” (`preverif2.q:4`) | “O escritor acrescenta IDs à lista sem remover os anteriores; o leitor retorna seu comprimento, sem janela temporal ou marco da alteração.” (`preverif1.r:3`) Depois: “Não existe comparação temporal com um marco da cura nesse caminho.” (`preverif2.r:5`) A consequência literal sobre a primeira ocorrência nova: **nenhum**. |
| **5 — caso não chama `linha()`; mutante cai primeiro por exceção** | **(b), falha da triagem.** Motivo registrado: “DESCARTADO para esta obra, linha de fila; conferido em memoria pelo Codex.” (`triagem.txt:7`) | “O caso b3_sem_obra_historico e os 3 mutantes (historico-reconta, forma-invalida-vira-zero, duplicata-reconta) MORDEM de verdade?” (`preverif3.q:8`) | “`forma-invalida-vira-zero`: cai primeiro por `KeyError` em `{}`”; “apesar do comentário, esse caso não chama `linha()`”. (`preverif3.r:33`; `preverif3.r:35`) |
| **6 — padrão Architecture Conformance Test inadequado à guarda de runtime** | **(e) pela regra da cópia; também (d) pela definição sobreposta do pedido**, conforme ressalva acima. (`CLAUDE.md:383`; `CLAUDE.md:390`; `opus_final_v3.txt:7`) | **nenhum** sobre adequação do padrão à guarda de runtime. As listas de classes perguntadas não incluem esse tema. (`preverif1.q:8`; `preverif2.q:9`; `preverif3.q:9`) | **nenhum** sobre esse tema, inclusive lateralmente. (`preverif1.r:29`; `preverif2.r:33`; `preverif2.r:37`; `preverif3.r:37`) |

## 11/11 03:09 · sessao 291b543431b5 · VERIFICADOR-EM-FABLE · verificador · origem=Code

APROVA

1. **A negativa passa a ter regra distinta de `B3`.** `relato_negacao` captura hífens e escreve `NEGOU B3-SEM-OBRA`; a guarda retorna essa negativa antes de consultar a parada da obra (`bin/disjuntor.py:262`, `bin/disjuntor.py:279`, `bin/disjuntor.py:1845`).

   Os demais leitores não precisam interpretar essa regra: `ler()` separa eventos pelo terceiro token, contando `NEGOU` como negativa e somente `DISJUNTOR` como disparo. O Stop recebe o mapa de obras paradas; `gerar_estado.py` delega a leitura e apresentação ao disjuntor. Não encontrei falsa negativa ou contagem indevida de parada introduzida pela troca nesses caminhos (`bin/disjuntor.py:1985`, `bin/disjuntor.py:1995`, `bin/disjuntor.py:2003`, `bin/hook_stop_fila1.py:1204`, `bin/gerar_estado.py:280`).

2. **Existe sobreposição lexical com IDs legítimos, mas ambas as negativas são inertes no consumidor encontrado.** `B3-SEM-OBRA` e `B3-NAO-SEI` são valores diferentes, ambos aceitos por `_OBRA_ID`/`_OBRA_HIF`; portanto `obras_da_linha` pode devolver esses nomes como obras (`bin/disjuntor.py:157`, `bin/disjuntor.py:163`, `bin/disjuntor.py:619`, `bin/disjuntor.py:637`).

   Isso não transforma nenhuma delas em obra parada ou liberação: o consumidor decisório exige também que a linha comece por `DESENHO ` ou `ERRO-BOBO `. Nem a mensagem `DISJUNTOR…` nem a linha datada `NEGOU…` satisfazem essa condição, inclusive quando a obra consultada tem exatamente um desses IDs (`bin/disjuntor.py:279`, `bin/disjuntor.py:735`, `bin/disjuntor.py:737`, `bin/disjuntor.py:748`).

3. **Os retornos 5 e 6 verificam o RELATO produzido pela porta real.** `_arvore` inicializa o RELATO de mentira; `_pre` executa `disjuntor.py pretool` com essa raiz; `main_pretool` chama `relato_negacao`, que encaminha a linha a `no_relato`. Os dois `grep` verificam a presença da regra nova e a ausência da regra `B3` (`bin/tests/test_disjuntor.sh:79`, `bin/tests/test_disjuntor.sh:85`, `bin/disjuntor.py:1907`, `bin/disjuntor.py:294`, `bin/tests/test_disjuntor.sh:1474`, `bin/tests/test_disjuntor.sh:1475`).

   **Ressalva precisa sobre a mordida:** o mutante `prefixo-do-disparo` já cai no retorno **1**, pela mudança do stdout; ele não chega aos retornos 5 e 6. Portanto mata a regressão, mas sozinho não demonstra a mordida dessas duas verificações (`bin/tests/test_disjuntor.sh:1471`, `bin/tests/test_disjuntor.sh:1548`). Execução integral da suíte shell: [nao verificado].

4. **Não encontrei reaparecimento das classes citadas no delta.** A negativa conserva a porta única de escrita; a regra ganha prefixo próprio, enquanto o disparo conserva `DISJUNTOR B3 <obra>`; o selo consulta o RELATO resultante. Não há novo escritor nem novo reconhecedor paralelo nesse trajeto (`bin/disjuntor.py:294`, `bin/disjuntor.py:1149`, `bin/disjuntor.py:1647`, `bin/tests/test_disjuntor.sh:1474`).

5. **Nenhum FILA fornecido demonstra bloqueio adicional da cura literal do item (1).**

   - O número publicado deriva explicitamente de `b3_sem_obra_obs`; o histórico é contado separadamente. Não são dois significados no mesmo campo persistido (`bin/disjuntor.py:2045`, `bin/disjuntor.py:2201`, `bin/disjuntor.py:2207`).
   - O contador cumulativo preserva ocorrências posteriores; não aplicar janela é compatível com acompanhar a meta “zero a partir daqui” do aval fornecido [nao verificado em arquivo] (`bin/disjuntor.py:2183`, `bin/disjuntor.py:2202`).
   - A identificação ampla de “final”, o filtro pelo tipo `verificador` e a chave desligada continuam limitando a cobertura. Não cabe afirmar garantia universal para qualquer agente ou configuração (`bin/disjuntor.py:177`, `bin/disjuntor.py:840`, `bin/disjuntor.py:1842`, `bin/disjuntor.py:1897`).
   - A falta de chamada a `linha()` no selo histórico e a morte incidental do mutante continuam fraquezas de cobertura; não demonstram apresentação incorreta no código atual (`bin/tests/test_disjuntor.sh:1485`, `bin/tests/test_disjuntor.sh:1542`, `bin/disjuntor.py:2202`, `bin/disjuntor.py:2275`).

O zero efetivo em produção e a identificação individual das seis obras históricas permanecem [nao verificado].

_3661 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
