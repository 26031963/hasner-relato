# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 09:22** · publicado em **27/11/2026 09:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 09:22 · sessao e0dac6bc0611 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

Registrei as portas de `celula_absolveu`, `fantasma` e `competencia_trancada` para os casos **06, 07, 27 e 37**. O diff de código tem **30 inserções e zero `except` novos**. [core/portas.py](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/core/portas.py:396) · [Diff stat](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:37)

**25, 26 e 28 ficaram parados** pela regra do censo. Os leitores ou escritores dos campos envolvidos estão em `chamados/models.py` e `chamados/services/disputa_emissao.py`, arquivos proibidos nesta volta. [Censo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:20)

Na verificação isolada do selo, os quatro recortes falharam na base, passaram com as declarações e voltaram a falhar sob os mutantes. A execução completa em Django/PostgreSQL fica com o Code **[nao verificado]**. O recibo contém o diagnóstico dos sete REDs, a linha `--only` e o comando de cada mutante. [construtor.r](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:5) · [Comandos](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/construtor.r:48)

## 27/11 09:21 · sessao 217e2dcf82a3 · PAR-PELO-TURNO · construtor · origem=Code

**PAR-PELO-TURNO c1v6 aplicada na cópia indicada.** O recorte em memória passou **95 testes**, incluindo composição de fatias e vizinhos. O diff passou em `git diff --check` e acrescentou **zero `except`**. [Comandos e vereditos](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:29) · [Auditoria e diff](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:146) [nao verificado]

- **Na base**, 1a, 2, 3a, 3b, 3d, 4a e 4c falharam pelos motivos esperados. Em 4c, a marca de `fim+2` derrubava `sem_movimento` e mudava o saldo de `0,00 h` para `−193,60 h`. Com a cura, os dois braços deram `0,00 h` e `sem_movimento=True`. [Evidência da base e da cura](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:6)
- **1b, 5 e 6** passaram na base. Os mutantes os fizeram falhar ao trazer as marcas de col297 para a janela errada, esvaziar a ata e retirar a C9. No total, **15 mutantes falharam por asserção** e o alias que preserva a ata passou. A evidência traz o comando de cada um. [Comandos dos mutantes](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:43) [nao verificado]

**Pendências para a certificação**

- A suíte Django da obra, composição e vizinhos parou na importação de `django.core`. Os testes completos de PDF e fechamento ficam **[nao verificado]**. [Comando e erro](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:123) [nao verificado]
- Em **3a**, `test_MORDE_a_divida_so_ENCOLHE_e_nao_apodrece` falha porque o inventário ainda exige a chamada direta a `batidas_apuraveis`. O PDF passou a usar `batidas_apuraveis_na_janela`. A asserção existente segue como estava. [Selo do PDF](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/tests/test_pdf_nao_calcula.py:107) · [Carga do PDF](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/pdf_espelho.py:280) [nao verificado]
- **3c e 4b** ficaram fora da volta conforme o recorte. Em **6**, o ensaio sintético deu um par de **782 min**, teto de **732 min** e uma C9. O laudo nominal do col155 em 08/11 e o impacto de frota por rubrica ficam **[nao verificado]**. [Itens em aberto](/home/ronald/wt-codex-par-pelo-turno-010806/app/docs/par_pelo_turno_c1v6_evidencia.txt:137)

## 27/11 09:12 · sessao bd73245393ec · CODEX-CONSTROI-MOTOR · construtor · origem=Code

Implementei a guarda do CCM no vigia. O juiz é chamado antes dos dois comandos Docker. Qualquer `rc != 0` grava stdout, stderr e a linha de recusa em `saida`, e retorna `None`. O diff acrescenta **0 `except`**. [core/esteira_vigia.py:748](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:748)

- O isolamento retorna `None` antes de classificar ou gravar o cache. [core/esteira_vigia.py:740](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:740)
- O auto-revert mantém o revert aplicado sem prova, registra a recusa na Pauta e no RELATO e retorna `revert aplicado, reteste recusado (CCM)`. [core/esteira_vigia.py:1322](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:1322) [nao verificado]
- O congelado e a autocura já exigiam verde para abrir quarentena e criar fatia. Acrescentei ao congelado um retorno específico para a recusa, antes do alarme de vermelho. [core/esteira_vigia.py:1377](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:1377) [core/esteira_vigia.py:1467](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/esteira_vigia.py:1467) [nao verificado]

**Evidência da volta**

Os seis casos foram escritos com o vigia ainda no HEAD `59600163ccc9cffb8f3ae148beede7cd7866ae86`. A leitura desse HEAD mostrou que `_rodar_testes` começava pelo Docker, que o isolamento classificava `None` como `'arvore'` e que o auto-revert desfazia o revert com `None`. A sintaxe dos dois arquivos e dos sete mutantes foi conferida. O RED, o GREEN e os mutantes seguem **[nao verificado]** em execução. A pista 2 documentada exige SSH e uma cópia commitada, operações vedadas nesta volta. [HEAD core/esteira_vigia.py:747, :741, :1315] [core/tests/test_vigia_ccm.py:38](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_vigia_ccm.py:38) [docs/BACKLOG.md:2375](/home/ronald/wt-codex-codex-constroi-motor-090747/app/docs/BACKLOG.md:2375)

Os vizinhos retornaram `rc 1` após a cura, com `FAILED (errors=2)` na importação. Os erros foram `No module named 'django.contrib'` e `No module named 'django.test'`. Nenhum caso dos dois módulos chegou a executar. [core/tests/test_esteira_sem_humano.py:10](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_esteira_sem_humano.py:10) [core/tests/test_esteira_status_verdade.py:13](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_esteira_status_verdade.py:13)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v \
  core.tests.test_esteira_sem_humano core.tests.test_esteira_status_verdade

git --no-optional-locks diff -- core/esteira_vigia.py
git --no-optional-locks diff --no-index /dev/null core/tests/test_vigia_ccm.py
```

**Comandos do RED, GREEN e mutantes na pista 2**

Com o checkout Git e o Django disponíveis, a função abaixo roda o teste novo com a versão escolhida do vigia em memória. Saídas de execução **[nao verificado]**. [core/tests/test_vigia_ccm.py:18](/home/ronald/wt-codex-codex-constroi-motor-090747/app/core/tests/test_vigia_ccm.py:18) [nao verificado]

```bash
ccm() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
import ast
import subprocess
import sys
import unittest
from pathlib import Path

import django
from django.conf import settings

settings.configure(SECRET_KEY="ccm-teste-isolado") [nao verificado]
django.setup()
from core import esteira_vigia as vigia

modo, *args = sys.argv[1:]
p = Path(vigia.__file__)
fonte = p.read_text(encoding="utf-8")
if modo == "head":
    fonte = subprocess.check_output([
        "git", "show",
        "59600163ccc9cffb8f3ae148beede7cd7866ae86:app/core/esteira_vigia.py",
    ], text=True)
elif modo == "mutante":
    nome, antes, depois = args
    no = next(n for n in ast.parse(fonte).body if isinstance(n, ast.FunctionDef) and n.name == nome)
    linhas = fonte.splitlines(keepends=True)
    trecho = "".join(linhas[no.lineno - 1:no.end_lineno])
    assert trecho.count(antes) == 1, (nome, antes)
    linhas[no.lineno - 1:no.end_lineno] = [trecho.replace(antes, depois, 1)]
    fonte = "".join(linhas)
else:
    assert modo == "cura", modo

exec(compile(fonte, str(p), "exec"), vars(vigia))
unittest.main(module="core.tests.test_vigia_ccm", argv=["ccm", "-v"])
PY
}

ccm head
ccm cura
ccm mutante _rodar_testes 'juiz.returncode != 0' 'juiz.returncode == 1'
ccm mutante _rodar_testes 'juiz.stdout + juiz.stderr' 'juiz.stdout'
ccm mutante _rodar_testes 'CCM-REDUZ-2711-0845' 'CCM'
ccm mutante _rodar_testes $'return None\n    pw =' $'return False\n    pw ='
ccm mutante _isolar 'if ok is None:' 'if ok is False:'
ccm mutante auto_revert 'if verde is None:' 'if verde is False:' [nao verificado]
ccm mutante curar_por_relogio 'if verde is None:' 'if verde is False:' [nao verificado]
```

## 27/11 09:05 · sessao e6e68478a2db · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**C2u parcial.** Apliquei os itens 2, 5 e 6. Os itens 1, 3 e 4 dependem de autorização explícita para editar `motor_calculo_v2.py`, que a ordem também proíbe sob pena de descarte. [Intermitente:3841](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3841) · [Comercial:3671](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3671) · [Partido:3103](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3103) [nao verificado]

| Item | RED antes da cura e resultado observado |
|---|---|
| 1. Intermitente | RED em `(750, 570, 0)` para trabalhado, noturno e fora. O teste exige `(720, 540, 30)` e segue RED. [Teste:31](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:31) | [nao verificado]
| 2. Pausa | A releitura dava `(300, 20)` para trabalhado e pausa, contra `(320, 0)` do motor e da alimentação. Com o gatilho compartilhado, os três dão `(320, 0)`. Duas fixtures existentes passaram a informar o desconto do motor, com as asserções originais preservadas. [RED:38](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:38) · [Cura:755](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:755) · [Fixtures:203](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:203) |
| 3. Comercial | RED em **HE 0 e saldo −33,83 h**, com 610 trabalhados e previsto de 660. Em memória, condicionar o previsto a `saida_da_base_da_he` deu **HE 130 e saldo −36,00 h** sem cauda. O controle com saída às 07h20 continuou com HE zero. [RED:60](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:60) · [Controle:145](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:145) |
| 4. Partido | O teste de igualdade literal deu RED para `MotorTurnoPartido`. `MotorTurnoPartidoNoturno` já coincide com `59600163c`. [Teste:72](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:72) |
| 5. Export | `AttributeError` no resultado composto vazio e `KeyError` no período sem chave. Ambos passaram com a leitura por `dia_pago._dia_de`. [REDs:15](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_export.py:15) · [Cura:346](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:346) |
| 6. Catraca | O censo acusou a lista antiga. Registrei `[3586, 3716, 3734, 3759, 3763, 3764]`. A catraca passou **35/35** e a restauração da lista antiga em memória voltou a falhar. [Registro:309](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:309) · [Asserção:564](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:564) | [nao verificado]

**Conflitos medidos nos itens 3 e 4**

- **Pontualidade sem cauda.** Com os marcos do `main` em memória, o caso `test_entrada_depois_da_meia_noite_com_d1_de_folga_tem_atraso_abatido_pela_parcial` deu atraso **0** e exige **5**. `test_sem_parcial_o_atraso_e_o_bruto` deu **0** e exige **12**. Nos dois, a saída é às 07h50, no marco. [Com parcial:119](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:119) · [Sem parcial:129](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:129)
- **Partido.** Na cópia literal em memória, `test_partido_com_intervalo_entre_dois_periodos` e `test_partido_preserva_os_490_minutos_do_main` deram `ValueError`. O Partido do `main` desempacota quatro valores e a janela atual retorna cinco. O teste do construtor da tripla passou nessa simulação. Os oito casos de `EscritorUnicoDaBaseDaHeTest` passaram na cópia de trabalho. [Janela:1939](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1939) · [Intervalo:147](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:147) · [490 minutos e tripla:51](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:51) · [Escritor:661](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:661) [nao verificado]

**Execução.** Os 11 casos focados de pausa e export passaram em `unittest` em memória, com as fronteiras externas substituídas. A chamada conjunta de `ponto.tests.test_ramo2b_*`, `ponto.tests.test_o280_bateria_da_lei`, `core.tests.test_medida_turno_do_par` e `folha.tests` parou no import de `django.core`. O veredito da suíte Django e a certificação do Code e do Opus final estão pendentes. O arquivo `verif_c2t.md` ficou [nao verificado]. [Entrada da suíte:11](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/manage.py:11) [nao verificado]

Os quatro mutantes em memória produziram os REDs esperados. Os dois primeiros saíram com `rc 1`, e os do export com `rc 2`, respectivamente `AttributeError` e `KeyError`. Comandos para reproduzir com Django disponível. [Mutantes:14](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2u_mutantes.py:14)

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -B -m ponto.tests.ramo2b_c2u_mutantes releitura_ignora_desconto
python3 -B -m ponto.tests.ramo2b_c2u_mutantes alimentacao_ignora_desconto
python3 -B -m ponto.tests.ramo2b_c2u_mutantes export_exige_mapa
python3 -B -m ponto.tests.ramo2b_c2u_mutantes export_exige_chave
```

Para reproduzir o mutante da catraca sem Django, execute o trecho `unittest` documentado no teste com a lista do Comercial restaurada em memória para `[3586, 3717, 3735, 3760, 3764, 3765]`. [Comando da catraca:30](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:30) · [Entrada mutada:309](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:309) [nao verificado]

**Diff da cópia.** `git diff --check` passou. A contagem de `except` adicionados foi zero, incluindo os arquivos novos. [Pausa:35](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/alimentacao.py:35) · [Export:589](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:589) [nao verificado]

```bash
git diff HEAD -- .
for arquivo in ponto/tests/*c2u*.py; do
  git diff --no-index -- /dev/null "$arquivo"
done
```

## 27/11 09:03 · sessao 3569bcc00a47 · E4D-CONTRATO-ROTA-B · leitura · origem=Code

## 1. Caso 14

O RED é `MatrizHistoricoTest.test_caso_14_l130_preserva_historico_e_a_nova_resposta`. A fixture começa com `respondida_em=T0`, texto vazio e duas linhas `RESPOSTA`, uma antiga com `07:00` e outra vazia. Depois de chamar a porta L-130, o teste exige `[RESPOSTA, RESPOSTA, REABERTURA]`, preserva a foto das duas linhas antigas, confere a projeção vazia e verifica que uma nova resposta `09:00` fica vigente. [test_e4d_matriz_reds.py:644](/home/ronald/codex_ws/4e9ae2ddd8125dba58277ceb719022587d6036da/app/chamados/tests/test_e4d_matriz_reds.py:644)

**A asserção da linha 654 diverge da opção (A).** Ela deve exigir que a terceira linha seja um ato próprio `SANEAMENTO` e que seu campo próprio de lei contenha `L-130`. As verificações de preservação do histórico, limpeza e nova resposta continuam valendo. A cópia registra a decisão de usar campo próprio, mas o modelo ainda carece desse ato e desse campo. O nome do campo é **[nao verificado]**. `ato_motivo` é texto da trilha e `da_ressuscitacao` é a marca de `REABERTURA` da ressuscitação. `app/chamados/tests/test_e4d_matriz_reds.py:654–661` · `app/docs/PROMPTS.md:2950` · `app/docs/BACKLOG.md:2163` · `app/chamados/models.py:4326–4365`

## 2. Colisão com o c1 RED4

**Sim, no contrato descrito pela E4D.** A docstring remete a `C1:234–254` para uma `REABERTURA` marcada e seu efeito na tarefa. O aval transcrito exige `SANEAMENTO` para a mesma devolução L-130. O ponto comum é `devolver_instante_sem_texto_l130`, em `app/chamados/services/validacao.py`. A colisão é sobre o valor de **`DecisaoPergunta.ato`**, definido em `app/chamados/models.py`. A projeção que a porta limpa é **`PerguntaDisputa.respondida_em`**, junto com `resposta_colab`. Hoje a porta registra a foto `RESPOSTA` se faltar, limpa o par e grava auditoria. As asserções literais do c1 RED4 e o nome do campo da marca da E4C são **[nao verificado]**. `app/chamados/tests/test_e4d_matriz_reds.py:644–656` · `app/docs/RELATO.md:172` · `app/docs/RELATO.md:2346` · `app/chamados/services/validacao.py:444–490` · `app/chamados/models.py:4344–4349,3658–3660`

## 3. Arquivos em comum e casos independentes

| Arquivo no pedido da E4C | Dependência da E4D |
|---|---|
| `app/chamados/models.py` | **Precisa mudar.** Aloja o tipo e os campos de `DecisaoPergunta`, seu redutor e a porta `registrar`. O caso 14 requer o ato próprio. Os casos 16 e 17 também exercitam diretamente `registrar` e `save`. `app/chamados/models.py:4326–4365,4405–4408,4493–4504,4590–4623` · `app/chamados/tests/test_e4d_matriz_reds.py:663–699` |
| `0057` e `0058` de `app/chamados/migrations/` | **Dependência de esquema, sem edição dessas duas demonstrada.** A cópia usa o nome da `0057` na fixture e reconhece `entrada_em` e `entrada_origem`. O caso 42 depende dessa distinção temporal e o 45 da partição novo/estoque. O conteúdo das duas migrações é **[nao verificado]**. `app/chamados/tests/test_e4d_matriz_reds.py:181–194,238–243,788–807,832–869` · `app/docs/PROMPTS.md:2974` |
| `app/ponto/validacao.py` | **O caminho correspondente na E4D é `app/chamados/services/validacao.py`.** Precisa mudar na devolução L-130 e nas portas que hoje salvam validação ou motivo da recusa diretamente. `app/chamados/services/validacao.py:164–178,444–490,992–1000` · `app/core/portas.py:396–403` |
| `app/chamados/disputa_emissao.py` | **O caminho correspondente é `app/chamados/services/disputa_emissao.py`.** Precisa mudar na rota completa. Hoje a resposta humana salva a projeção antes de registrar o ato, e a reabertura vazia do caso 11 exige alterar a seleção e a identidade do pedido nessa porta. `app/chamados/services/disputa_emissao.py:1529–1567,1695–1715` · `app/chamados/tests/test_e4d_matriz_reds.py:570–606` |

**Casos com caminho de construção fora desses arquivos**, pelo predicado dos REDs da cópia:

- **06, 07, 27 e 37** — asserções de exceção e selo em `app/core/portas.py`, usando as portas atuais de carimbo, retroativo e descarimbo. Os testes exigem zero ato novo nesses recortes. `app/chamados/tests/test_e4d_matriz_reds.py:305–349,526–538,898–916,1126–1165,1830–1853`
- **25 e 26** — seleção pela absorção e encaminhamento do legado em `materializacao.py`. O 25 já tem a porta de reabertura e a trilha da raiz disponíveis. `app/chamados/services/materializacao.py:1179–1212,1230–1285` · `app/chamados/tests/test_e4d_matriz_reds.py:1545–1613`
- **28** — trava, releitura e compensação no comando do espelho. A gravação da via já tem porta própria e a `REABERTURA` já é um tipo registrado. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:64–77` · `app/chamados/models.py:3662–3671,4326–4337` · `app/chamados/tests/test_e4d_matriz_reds.py:1167–1224`

**Guardas que já podem ser preservadas sem editar esses arquivos** — **13, 15, 19, 21, 22, 23, 24, 36, 43 e 46**. Para o **30**, API, modal e raio-x podem ser trabalhados à parte, mas o fechamento da face SQL pelo mesmo ato é **[nao verificado]** sem mexer na autoridade compartilhada. O **33** está fora da E4D no brief registrado. `app/chamados/tests/e4d_matriz_mutantes.py:48–97` · `app/chamados/tests/test_e4d_matriz_reds.py:1647–1663` · `app/chamados/juizes.py:1676–1681,1863–1868` · `app/chamados/models.py:4506–4515` · `app/docs/RELATO.md:292`

A lista indica independência técnica por leitura. A ordem de pouso registrada continua E4C-F5-INSTANTE → E4D. Identidade Git do hash, execução dos testes e estado de produção **[nao verificado]**. `app/docs/RELATO.md:67`

## 27/11 08:54 · sessao 0d97a1b1fe83 · CODEX-CONSTROI-MOTOR · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 27/11 08:53 · sessao e9a63f948616 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

**REPROVADO — `1f9541e71`.** A c2t fecha a herança da cauda no Plantão e retira a cauda da soma direta do banco semanal. O recorte ainda produz duas respostas para o mesmo período, `_lim_dia` muda dinheiro em jornada sem cauda e a catraca de linhas falha. `app/ponto/motor_calculo_v2.py:3784`, `:3593`, `:3709–3724`, `:3650–3678`; `app/ponto/calculador/alimentacao.py:135–150`; `app/ponto/calculador/periodo.py:753–770`; `app/core/tests/test_medida_turno_do_par.py:309`, `:564–568`.

1. **Plantão — curado no motor. O escopo ainda vaza para o Intermitente.** O Plantão continua chamando `Motor12x36.calcular_periodo`, com a flag desligada. No ensaio isolado, main e c2t deram os mesmos **1.440 min trabalhados, 540 min noturnos de relógio e 40 min fora da janela**. O Intermitente herda a flag ligada. Com marcos 19h–07h e saída às 07h30, o mesmo ensaio deu **720 trabalhados, 540 noturnos e 30 fora** no main, contra **750 trabalhados, 570 noturnos e zero fora** na c2t. O cadastro aceita os marcos e a fábrica seleciona a classe Intermitente. `app/ponto/motor_calculo_v2.py:795`, `:1917–1920`, `:3780–3807`, `:3841–3853`, `:3919–3922`; `app/escala/services/cadastro_tipo.py:205–227`; `app/ponto/tests/test_ramo2b_c2t_motor.py:14–36`; main `app/ponto/motor_calculo_v2.py:1863`. Incidência em produção [nao verificado].

2. **Igualdade na borda da pausa — cura parcial.** Sem cauda no dia mapeado, os dois caminhos voltam ao `<` do main. Com cauda, há dois recortes. A alimentação respeita `intra_descontada > 0`. A releitura recorta pelos intervalos carimbados e reescreve o desconto. No ensaio com o motor real, marcos 19h–00h, batidas 18h40–00h20 e pausa 18h50–19h20, motor e alimentação deram **320 min trabalhados**. A releitura deu **300 min trabalhados e 20 min de pausa**. O carimbo do intervalo antecede o gatilho que decide se o motor o desconta. `app/ponto/motor_calculo_v2.py:2985–3015`; `app/ponto/calculador/alimentacao.py:78–89`, `:135–151`; `app/ponto/calculador/periodo.py:304–312`, `:603–625`, `:753–770`; main `app/ponto/calculador/alimentacao.py:134` e `app/ponto/calculador/periodo.py:306`.

   O **dia** vem do juiz registrado `turnos_do_colab`. O motor projeta `Turno.data_turno` e publica `resultado.dia_da_jornada`, que ambos os leitores consultam. A duplicação introduzida é do predicado **“dia com cauda”** e do recorte da pausa. A alimentação ainda usa dois universos para esse predicado. Um portador aberto pode habilitar `<=` para outro par do dia, mas fica fora do `dias_com_cauda` entregue a `rubricas_por_dia`. O próprio motor já tem uma divergência de dia anterior à c2t, pela herança de até seis horas e pelo mapa que o Comercial reaplica. `app/docs/JUIZES.md:20–25`; `app/core/juizes.py:215–216`; `app/ponto/motor_calculo_v2.py:932–960`, `:1160–1162`, `:2613–2623`, `:2777–2786`, `:3585–3587`, `:3777`; `app/ponto/calculador/alimentacao.py:82–95`, `:120–124`, `:173–183`, `:474–479`, `:550–553`; `app/ponto/calculador/periodo.py:615–625`.

3. **Banco semanal do Comercial — curada a inclusão direta da cauda.** `trab_semana` soma a base da HE. O teste novo fixa **2.430 min trabalhados, 2.400 min de base, HE zero e saldo de −4 h**. A paridade do saldo com o main continua quebrada pelo item 4. Também permanece a mistura preexistente de datas na mesma conta semanal. O trabalho usa o dia da jornada e a dedução de HE usa a data civil da entrada. O teste novo, com HE zero, não exercita essa dedução. `app/ponto/motor_calculo_v2.py:3585–3593`, `:3709–3724`; `app/ponto/tests/test_ramo2b_c2t_motor.py:39–54`; main `app/ponto/motor_calculo_v2.py:3557–3572`; `app/docs/LEIS.md:117`, `:178`.

4. **`_lim_dia` — não curado.** O ramo ainda troca os **480 min** do main pelo previsto da jornada em qualquer cruzamento com os dois marcos, inclusive com saída no marco. Em ensaio isolado do mensal, com **610 min trabalhados**, previsto de **660** e saída às 07h no marco, a HE passou de **130 para zero** e o saldo de **−36,00 h para −33,83 h**. `_jornada_do_dia` consulta a fonte prevista na L-024. O aval localizado limita o efeito a turno que cruza **com saída depois do marco**. Aval para mudar o limite em jornada sem cauda [nao verificado]. A pontualidade também tem mudança fora desse recorte, com teste Comercial que sai no próprio marco e exige 5 ou 12 min de atraso. `app/ponto/motor_calculo_v2.py:1419–1434`, `:3650–3692`, `:3709–3724`, `:2257–2267`, `:2305–2308`, `:2329–2333`; main `app/ponto/motor_calculo_v2.py:3504–3526`; `app/docs/LEIS.md:75`; `app/docs/PROMPTS.md:3052`, `:3078`, `:3087`; `app/ponto/tests/test_ramo2b_c2l_red_motor.py:102–133`, `:145–156`.

5. **Partido — o corte da cauda foi preservado, e o corpo do motor foi alterado.** A flag `False` conserva a saída efetiva no marco. Na comparação com o main, `MotorTurnoPartido` passou a gravar pela porta da tripla e a chamar leitores novos no total diário, na intrajornada, na dobra e no ganho noturno. Isso descumpre a leitura literal de **“motor do Partido intocado”**. O teste de 490 min cobre um cenário de paridade. `app/docs/PROMPTS.md:3087`; `app/ponto/motor_calculo_v2.py:1917–1920`, `:3103–3106`, `:3131–3143`, `:3193–3195`, `:3241–3244`, `:3348–3352`, `:3387–3390`; main `app/ponto/motor_calculo_v2.py:2999–3014`, `:3058–3061`, `:3107–3112`, `:3211–3219`, `:3250–3259`; `app/ponto/tests/test_ramo2b_c2l_red_motor.py:51–61`.

6. **Esmeril dos quatro campos.** As conversões para horas são explícitas. A granularidade, a fonte do valor e o dia divergem nos pontos abaixo.

   | Campo | Quem escreve e quem lê |
   |---|---|
   | `saida_da_base_da_he` | `_escrever_base_da_he` grava o **instante**. Em par fechado é a ponta da base, limitada ao par. No portador aberto é o marco do último par creditado com cauda. O split noturno da HE, a alimentação e a releitura consultam o marcador. `app/ponto/motor_calculo_v2.py:1093–1105`, `:1933–1939`, `:1942–1971`, `:2763–2786`; `app/ponto/calculador/alimentacao.py:82–83`, `:113–117`, `:173–183`; `app/ponto/calculador/periodo.py:625`, `:758–762`. |
   | `minutos_base_da_he` | A mesma porta grava **minutos líquidos do par** ou **a soma dos pares creditados** no aberto. `_trabalhado_da_base_da_he` lê o valor para HE, banco, releitura e teto do export. O oráculo calcula outra medida, **por dia**, pelos segmentos recortados, e a entrega a `regras.do_dia` e à lavratura `oraculo`. O caso do portador aberto no item 2 mostra onde esses universos se separam. `app/ponto/motor_calculo_v2.py:286–290`, `:1942–1971`, `:2763–2786`, `:3593`, `:3618–3622`; `app/ponto/calculador/periodo.py:768`; `app/ponto/calculador/alimentacao.py:474–479`, `:550–553`; `app/ponto/calculador/regras.py:614–626`; `app/ponto/services/fechamento.py:1289–1320`; `app/folha/porta_export.py:342–365`. |
   | `minutos_trabalhados` | Os motores escrevem **minutos líquidos** no par, o crédito do aberto acumula minutos no portador, o bloqueio zera e `reler` pode reescrever. O mensal e o `DiaPago` convertem por `/60`. A promoção de folga em feriado também lê o integral, e a janela é desativada em par marcado como folga. A lavratura usa o mapa com queda para a data civil, enquanto o teto do export exige a chave no mapa. `app/ponto/motor_calculo_v2.py:329–334`, `:1628–1636`, `:1682–1687`, `:2777`, `:3025`, `:3141`, `:3514`; `app/ponto/calculador/periodo.py:748–749`, `:763`; `app/ponto/services/fechamento.py:922–923`, `:958–990`, `:1224–1241`; `app/ponto/services/dia_pago.py:126–132`, `:173–179`, `:234–267`; `app/folha/porta_export.py:106–122`, `:347–354`. |
   | `saldo_banco_horas` | O Comercial soma minutos semanais e publica **horas mensais**. O fechamento soma as fatias, grava o saldo e o passa à linha de ajuste sem data do `DiaPago`, também na versão oráculo. O retrato de exportada lê o saldo do fechamento. Cartão, PDF e conferência do export leem horas. A lista de eventos do TXT não emite saldo. `app/ponto/motor_calculo_v2.py:3709–3724`; `app/ponto/services/fechamento.py:118`, `:318–320`, `:1128`, `:1204`, `:1232`, `:1308`; `app/ponto/services/dia_pago.py:348–360`, `:504–520`, `:542`, `:550–563`; `app/relatorios/cartao_pela_celula.py:114`, `:191`; `app/relatorios/pdf_espelho.py:531`, `:815–818`; `app/folha/porta_export.py:507–518`; `app/folha/export.py:439–461`. |

7. **Export — falha de leitura para resultado composto vazio.** `medir` acessa `.dia_da_jornada` antes de o helper poder devolver uma lista vazia. `ResultadoMes` nasce sem esse atributo e a composição só o publica quando existe mapa de fatia não vazio. Um resultado composto vazio recebido nesse ponto levanta `AttributeError`. Para período sem chave, o helper levanta `KeyError`, enquanto `DiaPago` usa a data civil. O teto testemunha HE **junto com** intra positiva. A pendência de HE tem outra leitura, pelo espelho. `app/folha/porta_export.py:347`, `:356–362`, `:525–534`, `:551`, `:590–594`; `app/ponto/motor_calculo_v2.py:302–326`; `app/ponto/services/fechamento.py:200–202`, `:331–336`; `app/ponto/services/espelho.py:1263–1265`, `:1330–1331`; `app/ponto/services/dia_pago.py:126–132`. Ocorrência de ponta a ponta [nao verificado].

8. **Catraca, RED e regressão de testes.** O censo AST real achou **`[3586, 3716, 3734, 3759, 3763, 3764]`** para o Comercial. A catraca exige **`[3586, 3717, 3735, 3760, 3764, 3765]`** e compara as listas exatamente. Os dois REDs novos passaram em memória e cada mutante caiu por uma asserção, com zero erros, usando adaptações mínimas para importar sem Django. O fixture fabrica 20 min de desconto num par de duas horas, fora dos gatilhos dos motores, e deixa escapar o caso do item 2. Executei também o censo da tripla. Ele encontrou apenas a porta declarada. Os limites documentados com `vars()` e `copy` seguido de `setattr` continuaram escapando nos ensaios. `app/core/tests/test_medida_turno_do_par.py:309`, `:564–568`; `app/ponto/tests/test_ramo2b_c2t_escopo.py:14–51`; `app/ponto/tests/ramo2b_c2t_mutantes.py:17–28`; `app/ponto/tests/test_ramo2b_c2_base_da_he.py:661–727`; `app/ponto/motor_calculo_v2.py:1954–1958`, `:2999–3015`, `:3492–3503`.

   A comparação com a cópia do `59600163c` confirmou que **`test_saldo_semanal_comercial_usa_trabalhado` nasceu na obra**, em arquivo ausente no main. A c2t o substitui pelo teste de saldo sobre a base. Já no acumulado desde o main, há asserções preexistentes alteradas em janela de HE, noturno, extra declarada e B-I28. O delta c2t acrescenta guardas, aritmética e testes, sem novos `except` ou leituras do relógio nos trechos alterados. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:96–97`; `app/ponto/tests/test_ramo2b_c2t_motor.py:39–54`; `app/ponto/tests/test_janela_he_saida.py:82–98`, `:112–118`, `:148–154`; `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:61–79`, `:98–109`; `app/ponto/tests/test_o146_extra_declarada_da_escala.py:539–544`; `app/ponto/tests/test_o280_bateria_da_lei.py:5133–5139`, `:5173–5181`; `app/ponto/calculador/alimentacao.py:78–83`, `:138–150`; `app/ponto/calculador/periodo.py:304–312`, `:623–625`, `:753–762`; `app/ponto/motor_calculo_v2.py:3588–3593`, `:3709–3724`, `:3780–3785`.

Suíte Django completa, DIFF de frota, efeito nas competências exportadas e histórico Git de `eb2019f99` [nao verificado].

## 27/11 08:52 · sessao 65f100357ca0 · - · - · origem=Ronald (tmux)

**REPROVADO.**

1. **Item 2. Cura parcial.** O `<=` ficou restrito aos dias com ao menos um período marcado. Os dois leitores usam `resultado.dia_da_jornada`, portanto consomem a data publicada pelo motor. A autoridade registrada é `turnos_do_colab`, com `parear_turnos` e `_data_do_turno` na geometria. A c2t duplica a pergunta «o dia tem cauda?» e a lógica de recorte. `alimentacao` ainda calcula dois conjuntos de dias com cauda, com critérios diferentes. Um portador aberto recebe legitimamente a tripla no motor, habilita o `<=` e pode ficar fora do conjunto que chega às rubricas. [app/core/juizes.py:215] [app/ponto/turnos.py:312] [app/ponto/motor_calculo_v2.py:949] [app/ponto/motor_calculo_v2.py:1160] [app/ponto/motor_calculo_v2.py:2777] [app/ponto/calculador/alimentacao.py:78] [app/ponto/calculador/alimentacao.py:90] [app/ponto/calculador/alimentacao.py:138] [app/ponto/calculador/alimentacao.py:173] [app/ponto/calculador/periodo.py:291] [app/ponto/calculador/periodo.py:625] [nao verificado]

2. **Contraexemplo do recorte.** Ene­saiando em memória as funções reais com uma janela sintética, marcos 19:00–00:00, batidas 18:40–00:20, pausa 18:50–19:20 e piso zero, o 12x36 devolveu 320 trabalhados, base 300 e pausa descontada zero. A alimentação também deu 320. A releitura passou a 300 trabalhados e 20 de pausa. Ela usa a tripla para ativar o recorte sem consultar se o motor descontou a pausa. Ocorrência em produção [nao verificado]. [app/ponto/motor_calculo_v2.py:2985] [app/ponto/motor_calculo_v2.py:3000] [app/ponto/calculador/alimentacao.py:138] [app/ponto/calculador/periodo.py:307] [app/ponto/calculador/periodo.py:753] [nao verificado]

3. **Catraca vermelha.** Executei o censo AST puro. As linhas reais B1 do Comercial são `[3586,3716,3734,3759,3763,3764]`. A catraca congelada exige `[3586,3717,3735,3760,3764,3765]` e compara por igualdade. Os dois controles sintéticos da c2t passam em memória e suas duas mutações falham por asserção, sem erro. Suite com Django [nao verificado]. [app/core/medida_turno_do_par.py:448] [app/core/tests/test_medida_turno_do_par.py:309] [app/core/tests/test_medida_turno_do_par.py:564] [app/ponto/tests/test_ramo2b_c2t_escopo.py:43] [app/ponto/tests/test_ramo2b_c2t_escopo.py:51] [app/ponto/tests/ramo2b_c2t_mutantes.py:21] [nao verificado]

4. **Escopo adicional.** O `MotorIntermitente` herda a flag do 12x36 e a fábrica o seleciona explicitamente. O mesmo ensaio gerou 320 trabalhados, base 300 e cauda no Intermitente. O cadastro fornece marcos ao motor, inclusive por fallback no template. A pontualidade também muda sem saída além do marco. O teste da virada termina às 07:50, o próprio marco, e espera atraso de 5 ou 12 min. [app/ponto/motor_calculo_v2.py:795] [app/ponto/motor_calculo_v2.py:1917] [app/ponto/motor_calculo_v2.py:2265] [app/ponto/motor_calculo_v2.py:3841] [app/ponto/motor_calculo_v2.py:3921] [app/escala/models.py:1144] [app/ponto/tests/test_ramo2b_c2l_red_motor.py:102] [app/ponto/tests/test_ramo2b_c2l_red_motor.py:119] [app/docs/PROMPTS.md:3052] [nao verificado]

5. **Export.** A nova testemunha usa acesso direto ao mapa e à chave do período. O dataclass permite resultado vazio sem o atributo e o resultado composto só materializa o mapa quando recebe um mapa não vazio. Assim o acesso pode levantar antes de a testemunha examinar a lista vazia, além de poder levantar `KeyError` para período sem chave. A lavratura e a outra testemunha ainda usam o fallback de `_dia_de`. Ocorrência em produção [nao verificado]. [app/folha/porta_export.py:106] [app/folha/porta_export.py:347] [app/folha/porta_export.py:590] [app/ponto/motor_calculo_v2.py:302] [app/ponto/services/fechamento.py:202] [app/ponto/services/fechamento.py:331] [app/ponto/services/espelho.py:1198] [app/ponto/services/dia_pago.py:126]

## 27/11 08:52 · sessao 6a0fa390890d · - · - · origem=Ronald (tmux)

1. **Escopo do item 2 parcialmente curado.** Os calculadores leem `resultado.dia_da_jornada` e não refazem a data civil, mas implementam separadamente o predicado que habilita o recorte. Num ensaio em memória com o mapa fornecido, o motor e a alimentação somaram 150 min em dois pares da mesma jornada. A releitura somou 130 min porque descontou do primeiro par uma pausa que o motor apenas carimbou. O teste positivo fabrica `intra_descontada=20` para um par de 120 min, abaixo dos gatilhos dos dois motores. Os dois mutantes c2t falharam por asserção e os dois testes originais passaram em memória. Integração Django e produção [nao verificado]. `app/core/juizes.py:215-224`, `app/ponto/motor_calculo_v2.py:932-960,2993-3000,3489-3501`, `app/ponto/calculador/alimentacao.py:78-88,135-150`, `app/ponto/calculador/periodo.py:603,615-625,753-770`, `app/ponto/tests/test_ramo2b_c2t_escopo.py:20-24,36-51`, `app/ponto/tests/ramo2b_c2t_mutantes.py:21-28`.

2. **Intermitente fora do aval.** O `MotorIntermitente` recebe a política verdadeira por herança e o cadastro aceita marcos. Em memória, com janela 19–07 e saída às 07:30, ele devolveu saída efetiva 07:30, fora 0 e base às 07:00. O Plantão devolveu 07:00, fora 30 e base às 07:00. Ocorrência na frota [nao verificado]. `app/ponto/motor_calculo_v2.py:795,1917-1920,3780-3784,3841-3853,3919-3922`, `app/escala/services/cadastro_tipo.py:205-227`, `app/escala/models.py:326-345,347-368`, `app/ponto/tests/test_s6_intermitente_feriado.py:76-80`.

3. **Base da HE.** A tripla tem uma porta de escrita, mas a saída tem duas interpretações. No par fechado é o fim efetivo da sua base, que pode inclusive ser a entrada. No portador aberto conserva o fim do último par que teve cauda, enquanto os minutos acumulam várias bases. Os leitores da guarda incluem o portador, mas o oráculo geométrico só publica `dias_com_cauda` depois de descartar abertos. O motor e a testemunha do export leem os minutos gravados. A lavra `oraculo` recompõe os minutos da base do dia pela geometria, através de `rubricas_por_dia`, e escreve pela mesma porta `lavrar`. Caso real com cauda no portador aberto [nao verificado]. `app/ponto/motor_calculo_v2.py:286-290,1922-1939,1942-1971,2763-2786`, `app/ponto/calculador/alimentacao.py:78-95,120-124,173-183,474-479,550-553`, `app/ponto/calculador/regras.py:614-626`, `app/ponto/calculador/periodo.py:625,653-659,758-768`, `app/ponto/services/fechamento.py:1289-1320`, `app/folha/porta_export.py:342-364`.

4. **Trabalhado e banco.** O trabalhado é escrito em minutos pelos motores, no bloqueio e no crédito de aberto. A releitura também altera o objeto em memória. Os leitores persistidos convertem para horas, com a lavra do dia consumindo o mapa e usando data civil na falta da chave, como a testemunha correspondente do export. O saldo é mensal, em horas, gravado no Fechamento e copiado para ajuste sem dia no DiaPago. O TXT emite HE, intra e noturno do Fechamento. O saldo chega ao painel, cartão, PDF e comparação do export via soma de DiaPago. A soma semanal Comercial usa dia da jornada para o trabalhado e dia civil da entrada para o abatimento de HE, o que pode divergir na borda domingo–segunda. `app/ponto/motor_calculo_v2.py:329-334,1628-1636,2777,3025,3141,3514,3593,3709-3724`, `app/ponto/calculador/periodo.py:748-749,763`, `app/ponto/services/fechamento.py:318-320,922-923,978-990,1007-1009,1128,1204,1224-1241,1308`, `app/ponto/services/dia_pago.py:126-132,173-179,234-267,348-360,504-520,542,550-563`, `app/folha/porta_export.py:106-122,507-518`, `app/folha/export.py:439-461,739-747`, `app/relatorios/cartao_pela_celula.py:114,191`, `app/relatorios/pdf_espelho.py:531,815-818`, `app/templates/ponto/partials/painel_fechamento.html:37-38`.

5. **Leitor do teto no export.** O consumidor acessa `resultado.dia_da_jornada` diretamente, inclusive quando o resultado composto tem zero períodos. O resultado nasce sem o mapa e a composição só o cria para mapa de fatia não vazio. Um colaborador com fechamento, escala e zero batidas que passe a classificação pode levantar `AttributeError` antes de o teto avaliar a lista vazia. O selo apresenta sempre mapa preenchido e intra positiva. Produção e antiguidade do defeito no main [nao verificado]. `app/folha/porta_export.py:525-532,590-594`, `app/ponto/services/fechamento.py:200-202,331-336`, `app/ponto/services/espelho.py:1198-1202,1265,1330-1331`, `app/ponto/motor_calculo_v2.py:303-325`, `app/folha/export.py:840-857,911-977,1214-1222`, `app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:43-62`.

## 27/11 08:51 · sessao fcc0da312b07 · RAMO2-B-EM-TURNO-QUE-CRUZA · leitura · origem=Code

**DIFF de impacto L-110.** O escritor real é `ponto.services.dia_pago.lavrar`. `recalcular_fechamento_mes(..., somente_leitura=True, lavrar_dias_pagos=True)` devolve o `FechamentoMensal` em memória e chama `lavrar` com os períodos e mapas da mesma apuração, na versão `motor`. A partir de 10/2026, `rubricas_por_dia` monta o mapa que alimenta uma segunda chamada ao mesmo escritor, na versão `oraculo`. [app/docs/LEIS.md:136] [app/ponto/services/fechamento.py:1152-1168,1215-1265,1289-1325] [app/ponto/calculador/alimentacao.py:353-370]

`lavrar` apaga e recria as linhas por colaborador, competência e versão. Grava as rubricas, os minutos de grade, o furo apurado, os detalhes e os carimbos de origem. A data segue o mapa do dia da jornada quando disponível. A linha de ajuste tem data nula e leva o reflexo de DSR e o saldo do banco. [app/ponto/services/dia_pago.py:34-47,126-141,335-361] [app/ponto/models.py:315-387]

O `fm` usa os mesmos 26 campos do anexo, mais `empresa`. O `dp` usa as linhas datadas de `motor`, com as rubricas convertidas de horas para minutos e os três campos de grade na unidade original. Os hashes cobrem as duas tabelas inteiras no schema `juliani`, com ambas as versões e os ajustes. A sonda exige ajustes recém-lavrados para toda a frota e só grava o JSON depois de conferir os hashes e o estado da sequência após o rollback. [app/ponto/management/commands/aplicar_09_corte_b.py:67-74] [app/ponto/services/fechamento.py:1152-1160,1289-1320] [app/ponto/services/dia_pago.py:335-361] [app/ponto/models.py:325-351] [anexo espurias_txt11_s3.py:189-231]

Para as duas árvores, use a mesma sombra já preparada, a trava da sombra e o mesmo `AGORA`. Execute no `tenant_command shell --schema=juliani` com o mount `/sombra`, que corresponde a `logs/sombra`. O atalho `bin/sombra.sh --rodar` consulta a lavra em produção e a importa antes de executar a sonda. [bin/sombra_trava.sh:4-7] [bin/sombra.sh:136-167,182-191,483-492] [app/ponto/services/fechamento.py:634-636,1134-1141]

**Estimativa:** 4–8 minutos por competência em cada árvore, ou 8–16 minutos para o par, fora a espera pela trava. Conferi a sintaxe em memória. A execução no PostgreSQL, os hashes resultantes, o tempo real e a identificação da cópia como `1f9541e71` estão [nao verificado].

### SCRIPT

```python
ROT = "main"
MES = 11
ANO = 2026
AGORA = None  # Opcional: o mesmo ISO 8601 com offset nas duas arvores.

import hashlib
import json
import logging
import re
import sys
from contextlib import redirect_stderr, redirect_stdout
from datetime import datetime
from io import StringIO
from pathlib import Path
from time import monotonic
from traceback import extract_tb
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from ponto import motor_calculo_v2
from ponto.models import DiaPago, FechamentoMensal
from ponto.services import dia_pago, fechamento

EMPRESAS = (2, 3, 4)
BANCO, SCHEMA = "sombra", "juliani"
QUOTE = connection.ops.quote_name


class FalhaSonda(RuntimeError):
    pass


def erro_sem_dados(tipo, erro, tb):
    if tipo is FalhaSonda:
        print(str(erro), file=sys.stderr)
    else:
        origem = extract_tb(tb)[-1]
        print(f"{origem.filename}:{origem.lineno} {tipo.__name__}", file=sys.stderr)


def tabela(modelo):
    return f"{QUOTE(SCHEMA)}.{QUOTE(modelo._meta.db_table)}"


def estado_das_linhas():
    estado = {}
    with connection.cursor() as cursor:
        for modelo in (FechamentoMensal, DiaPago):
            pk = QUOTE(modelo._meta.pk.column)
            cursor.execute(f"""
                SELECT count(*),
                       md5(coalesce(string_agg(md5(to_jsonb(t)::text), ''
                                                ORDER BY t.{pk}), ''))
                FROM {tabela(modelo)} AS t
            """)
            n, md5 = cursor.fetchone()
            estado[modelo.__name__] = {"n": n, "md5": md5}
    return estado


def estado_da_sequencia(cursor, sequencia):
    cursor.execute(f"SELECT last_value, is_called FROM {sequencia}")
    return cursor.fetchone()


def ler_dp(primeiro_pk, fm):
    campos = dia_pago.CAMPOS_DIA
    grade = set(dia_pago.CAMPOS_GRADE)
    versoes = ("motor", "oraculo") if (ANO, MES) >= (2026, 10) else ("motor",)
    dp = {cid: {} for cid in fm}
    ajustes = set()
    linhas = DiaPago.objects.filter(
        pk__gte=primeiro_pk, mes=MES, ano=ANO,
        colaborador__empresa_id__in=EMPRESAS, versao__in=versoes,
    ).order_by().values_list("colaborador_id", "data", "tipo", "versao", *campos)

    for cid, data, tipo, versao, *valores in linhas.iterator(chunk_size=2000):
        cid = str(cid)
        if cid not in dp:
            raise FalhaSonda(f"DiaPago fora do universo colab_id={cid}")
        if tipo == DiaPago.TIPO_AJUSTE and data is None:
            chave = (cid, versao)
            if chave in ajustes:
                raise FalhaSonda(f"Ajuste duplicado colab_id={cid} versao={versao}")
            ajustes.add(chave)
        elif tipo == DiaPago.TIPO_DIA and data is not None:
            if versao == "motor":
                dp[cid][data.isoformat()] = {
                    campo: valor if campo in grade else valor * 60
                    for campo, valor in zip(campos, valores, strict=True)
                }
        else:
            raise FalhaSonda(f"Tipo/data de DiaPago inconsistente colab_id={cid}")

    faltam = {(cid, versao) for cid in fm for versao in versoes} - ajustes
    if faltam:
        cid, versao = min(faltam)
        raise FalhaSonda(f"Ajustes ausentes={len(faltam)} primeiro_colab_id={cid} versao={versao}")
    return dp


def main():
    inicio = monotonic()
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict.get("HOST") != "sombra_db"
        or connection.settings_dict.get("NAME") != BANCO
        or connection.schema_name != SCHEMA
        or connection.in_atomic_block
        or not connection.get_autocommit()
    ):
        raise FalhaSonda("Requer sessao sombra_db/sombra/juliani em autocommit.")
    if not re.fullmatch(r"[A-Za-z0-9_-]+", ROT) or not 1 <= MES <= 12:
        raise FalhaSonda("ROT ou MES invalido.")

    agora = datetime.fromisoformat(AGORA) if AGORA is not None else timezone.now()
    if timezone.is_naive(agora):
        raise FalhaSonda("AGORA requer offset.")
    motor_md5 = hashlib.md5(Path(motor_calculo_v2.__file__).read_bytes()).hexdigest()
    fm = {}

    with transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ WRITE")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != (BANCO, SCHEMA):
                raise FalhaSonda("Banco ou schema efetivo fora da sombra.")

        empresas = set(Empresa.objects.filter(pk__in=EMPRESAS).values_list("pk", flat=True))
        if empresas != set(EMPRESAS):
            raise FalhaSonda("Escopo incompleto de empresas.")
        if (MES, ANO) in fechamento.GUARDA_COMPETENCIA and any(
            fechamento.empresas_exportadas_no_escopo(MES, ANO, empresa_id=eid)
            for eid in EMPRESAS
        ):
            raise FalhaSonda("A guarda da competencia impede medir toda a frota.")

        antes = estado_das_linhas()
        coluna = DiaPago._meta.pk.column
        with connection.cursor() as cursor:
            cursor.execute("SELECT pg_get_serial_sequence(%s, %s)", [tabela(DiaPago), coluna])
            sequencia = cursor.fetchone()[0]
            if sequencia is None:
                raise FalhaSonda("Sequencia de DiaPago ausente.")
            seq_antes = estado_da_sequencia(cursor, sequencia)
            cursor.execute(f"SELECT coalesce(max({QUOTE(coluna)}), 0) + 1 FROM {tabela(DiaPago)}")
            primeiro_pk = int(cursor.fetchone()[0])
            # RESTART dentro de atomic protege a sequencia e identifica as linhas frescas.
            cursor.execute(f"ALTER SEQUENCE {sequencia} RESTART WITH {primeiro_pk}")

        with (
            patch.object(timezone, "now", new=lambda: agora),
            patch.object(logging.Logger, "handle", new=lambda _logger, _registro: None),
            redirect_stdout(StringIO()),
            redirect_stderr(StringIO()),
        ):
            for eid in EMPRESAS:
                universo = set(fechamento.universo_da_competencia(
                    MES, ANO, empresa_id=eid,
                ).values_list("pk", flat=True))
                apurado = fechamento.recalcular_fechamento_mes(
                    MES, ANO, empresa_id=eid, somente_leitura=True, lavrar_dias_pagos=True,
                )
                if apurado.keys() != universo:
                    raise FalhaSonda(
                        f"FM incompleto empresa={eid} apurados={len(apurado)} universo={len(universo)}"
                    )
                fm.update({
                    str(cid): {
                        "empresa": eid,
                        **{
                            campo: None if valor is None else float(valor)
                            for campo, valor in campos.items()
                        },
                    }
                    for cid, campos in apurado.items()
                })
            dp = ler_dp(primeiro_pk, fm)
        transaction.set_rollback(True)

    depois = estado_das_linhas()
    with connection.cursor() as cursor:
        seq_depois = estado_da_sequencia(cursor, sequencia)
    if antes != depois or seq_antes != seq_depois:
        raise FalhaSonda("Divergencia de linhas ou sequencia apos rollback.")

    saida = Path(f"/sombra/ramo2diff_diapago_{ROT}_{ANO}_{MES:02d}.json")
    conteudo = json.dumps({
        "fm": fm, "dp": dp, "fm_antes": antes, "fm_depois": depois, "motor_md5": motor_md5,
    }, sort_keys=True, allow_nan=False)
    with saida.open("x", encoding="utf-8") as arquivo:
        arquivo.write(conteudo + "\n")
    print(f"{saida.name} colabs={len(fm)} agora={agora.isoformat()} segundos={monotonic() - inicio:.1f}")


sys.excepthook = erro_sem_dados
main()
```

### FIM

## 27/11 08:45 · sessao c12b28a112d8 · CODEX-CONSTROI-MOTOR · leitura · origem=Code

## 1. Fonte de autoria do teste [nao verificado]

**Não existe na c12 um manifesto persistido que marque um caminho como `CRIADO` pelo Codex construtor. Existe o `<resposta>.inventario`, que persiste os caminhos tocados pela chamada.** O inventário emite `TOCADO`, `PROIBIDO` ou `FALHA`. `TOCADO` agrupa criação, edição e outras diferenças. A resposta é escolhida pelo chamador e fica sob `logs/`. O wrapper não exige `logs/<OBRA>/` e não grava o inventário na `wt`. `bin/codex_pergunta.sh:89–100,145–151,229–250,457–467,835–837`

- **Onde já se sabe que é criação.** `_INVENTARIO_PY` monta `arvore` com `git ls-tree` do `HASH`, compara o FS e o status Git com essa base e, no modo positivo, reconhece o teste do app autorizado, recusa se o caminho já está em `arvore` e exige que exista no FS final. A chamada exige inventário vazio na entrada e na revalidação. Assim, para aquela chamada em modo positivo, um `TOCADO` da classe de teste aceita permite derivar a criação. O escritor único dessa informação seria o próprio wrapper, na saída desse mesmo inventário final. O arquivo bruto é escrito antes da prova de vida da lease, que ainda pode invalidar sua atribuição à chamada. `bin/codex_pergunta.sh:164–166,196–250,555–570,791–797,835–853`
- **O que os recibos provam.** `CODEX_RESULTADO` e `.rc` dão o desfecho da chamada. `.r` é a resposta do Codex, e `.id` liga uma resposta bem-sucedida à obra, papel, hash de entrada, PID e SHA da resposta. O JSONL é a saída do executor e o registro da vaga guarda obra, `wt`, base, `HEAD`, `main` e desfecho. Nenhum desses recibos publica a lista de criações. `rc_sem_colher.py` procura o `.rc` em `logs/<OBRA>/`. O inventário e os recibos da chamada real desta obra ficam [nao verificado]. `bin/codex_pergunta.sh:306–325,353–359,587–605,765–775,809–827,856–890` · `bin/rc_sem_colher.py:50–82`

## 2. Fonte da aprovação pelo Opus final

**Não existe na c12 uma fonte executável que ateste `Opus final APROVOU este diff`. Existe um leitor executável do texto `APROVADO` no transcript do verificador final.** `_veredito_e_obra` lê a obra do prompt e o veredito da primeira linha de `SubagentHandback`. `_reprovacoes_finais` procura `agent-*.jsonl` com `agentType=verificador` e entrega ao B3 somente os `REPROVADO`. O leitor não vincula a aprovação a um hash do diff nem verifica que o modelo era Opus. O Stop e o PreToolUse acionam essa leitura para o B3. `bin/disjuntor.py:438–466,649–671,839–853,981–1001,2233–2235,2255–2264` · `bin/hook_stop_fila1.py:1305–1321` · `.claude/agents/verificador.md:2–7,18`

- **RELATO e merge.** `linhas_libera_b3` reconhece `DESENHO` ou `ERRO-BOBO` no topo e `decide_b3` usa a linha para liberar um B3 disparado. `commit_msg.sh`, quando há `MERGE_HEAD`, passa `OBRA: <id>` a `--merge-obra`. O juiz consulta `estado_b3` sem transcript e aceita `LIVRE`, inclusive quando a obra nunca disparou. `LIBERA-B3` e `B3-MERGE livre` são estados desse portão. `bin/disjuntor.py:729–757,1025–1043,1086–1094,1289–1305` · `bin/commit_msg.sh:23–38`
- **`opus.md` ou linha de aprovação no RELATO.** Leitor que use qualquer dos dois como aprovação do diff nos três sítios [nao verificado]. O parecer fornecido abre com `REPROVADO`. Aprovação final, hash aprovado e execução em produção [nao verificado]. `r.txt:1`

## 3. Seleção dos três sítios na VM

| Sítio | Seleção, árvore e ponto do filtro |
|---|---|
| **Régua** | O shell escolhe `${ONLY:-$LABELS}`. `LABELS` contém apps e `--only` fornece labels explícitas. A régua monta sempre `~/saas-hasner/app`, mesmo quando o script é chamado de uma `wt`, e chama `manage.py test` direto pelo `trava_teste.sh`. O ponto de despacho é `:165–169`. Com label de app ou pacote, a lista de casos se forma na descoberta do Django, na chamada `super().build_suite()` do runner de CI. A poda de quarentena vem depois da descoberta. Para impedir também a carga do módulo na VM, a exclusão precisa valer antes de o Django importar o candidato. O glob de selos de host é outro fluxo e o selo do RELATO abre o fonte por glob. `bin/regua.sh:13–23,46–51,138–145,165–169` · `app/config/settings/ci.py:40` · `app/core/runner_quarentena.py:72–96` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:143–148` |
| **Vigia da esteira** | A lista do isolamento nasce em `_isolar`, na extração e deduplicação dos IDs de `FAIL`/`ERROR` de `green.out`. O reteste monta `d/orig/app`, fotografia da viva feita **antes** da cura, e usa Docker direto pela `_rodar_testes`. O ponto comum aos retestes é a lista `ids` imediatamente antes de montar o argv nessa função. Reversão e relógio alimentam o mesmo ponto com IDs de `logs/vigia_arvore.falhas` para `R/app`. A autocura usa uma cópia temporária da viva. `app/core/esteira_vigia.py:725–762,1301–1317,1352–1363,1429–1448` · `bin/molde_fatia/rodar.sh:9–14,43–51` |
| **Vizinhos da volta** | Lê `base..HEAD` Git da árvore recebida e enumera `test*.py` no FS de `app/`. `poe()` é o único ponto que acumula as labels de teste tocado presente, vizinho por AST ou texto, app de migration e selo global. `suite.sh --volta` chama o seletor e retém apps de `LABELS`. O seletor entrega candidatos antes de a rota ser conhecida. A exclusão **da execução na VM** cabe no ramo local de `suite.sh`, depois da decisão de rota e antes do `manage.py test`. Uma label de app ou pacote pode redescobrir o módulo mesmo se sua label pontual sumir. `bin/vizinhos_da_volta.py:25–51,92–132` · `bin/suite.sh:198–216,260–285,502–529,560–569` |

## 4. Como o mesmo teste chega à pista 2 [nao verificado]

- **`--dir <wt> --volta <base>`** passa por Vizinhos antes da rota. **`--dir <wt> --only <label>`** recebe a label do chamador e pula Vizinhos. A inteira **`--dir <wt> --motivo "pre-opus <OBRA>"`** usa os apps de `LABELS`. Nenhum desses pedidos executa `regua.sh` ou `esteira_vigia.py`. `suite.sh` lê a linha `LABELS=` da régua, usa o `HEAD` da cópia ou fotografa alterações de `app/` num commit, publica `refs/pista/<sha>` e envia `ssh pista pede <sha> --only ...`. O contrato do atendente remoto está documentado fora dos executores da VM. O job real desta obra fica [nao verificado]. `bin/suite.sh:195–216,417–452,502–507` · `app/docs/BACKLOG.md:2375`
- **O mesmo argv hoje pode cair na VM.** Indisponibilidade ou incompatibilidade antes do aceite, e recusa remota sem execução, levam a `ROTA VM`. Depois do aceite, falha de transporte vira erro de infraestrutura. A inteira de pouso e a chamada sem `--dir` seguem na VM. Filtrar a saída única de Vizinhos antes dessa bifurcação também tiraria o teste da pista. A inteira pré-Opus e o recorte explícito também precisam respeitar a restrição se chegarem ao executor local. `bin/suite.sh:319–342,351–381,444–449,468–479,502–529,560–569`

## 5. O que distingue antes e depois do merge

- **Régua.** A distinção atual é só a presença do arquivo no `app/` vivo. Se o teste está apenas na `wt`, a régua canônica não o vê. Se os bytes chegam ao disco vivo antes do commit de merge, o bind já os vê. Impressão digital e carimbo medem a árvore, sem julgar se a entrega entrou em `main`. `bin/regua.sh:14–15,25–45,165–169,185–203` · `CLAUDE.md:352–354`
- **Vigia.** `orig/app` fixa os bytes da viva na abertura daquela fatia. Um `orig` posterior pode conter o teste pousado. Os retestes de reversão e relógio usam a viva diretamente. A decisão do isolamento usa o `mtime` de `green.out` e pode reutilizar um resultado sem reler os IDs. Nenhum desses pontos consulta `main` para saber se aquela entrega pousou. `bin/molde_fatia/rodar.sh:9–14` · `app/core/esteira_vigia.py:725–743,1301–1317,1352–1363`
- **Vizinhos.** A entrada é `<árvore> <base>` e o delta termina no `HEAD` daquela árvore. A mesma `wt` pode fornecer o mesmo delta após o merge em outra árvore. Sem `--dir`, `suite.sh --volta` usa sua própria raiz. O desligamento automático exigido pelo aval depende de saber que a entrega foi integrada a `main`. A ref viva já é lida pelo wrapper na admissão, mas nenhum dos três seletores usa esse dado como prova de pouso. `bin/vizinhos_da_volta.py:25–30` · `bin/suite.sh:200–206` · `bin/codex_pergunta.sh:306–310,575–576` · `aval.txt:1`

## 6. REDs e impacto nos chamadores

Use o mesmo módulo da sonda, `app/core/tests/test_model_sem_migration_pendente.py`, e um teste existente como controle. O anexo relata `rc 0` e ausência na base para a criação da fixture. Os resultados abaixo são os esperados pela leitura. Execução dos três REDs e do controle na pista [nao verificado]. `matriz_sem_flag.txt:7–10` · `bin/codex_pergunta.sh:232–250` · `aval.txt:1`

| RED | Entrada e estado | Hoje | Esperado pelo aval |
|---|---|---|---|
| **Régua** | O `app/` montado pela régua de teste contém os dois módulos. O novo tem origem na chamada do construtor, está pendente e é descobrível por `core`. | A descoberta inclui os dois. | Na VM roda o existente. O novo roda na pista com a cópia que o contém. Uma fixture só na `wt` não exercita a régua canônica. `bin/regua.sh:14,23,165–169` · `app/core/runner_quarentena.py:72–96` · `bin/suite.sh:417–445` · `aval.txt:1` |
| **Vigia** | `green.out` nomeia os dois IDs, cache frio, `orig/app` da fixture com os módulos disponíveis. O novo está pendente. | `_isolar` entrega os dois IDs ao Docker. Na fotografia anterior à criação, o módulo novo estaria ausente, mas seu ID ainda seria passado e a tentativa influenciaria a classificação. | Retesta só o existente. Se todos os IDs forem excluídos, o isolamento fica sem reteste e sem veredito derivado dele. A pista pode rodar o novo na cópia que o contém. `app/core/esteira_vigia.py:125–144,725–762` · `bin/molde_fatia/rodar.sh:9–14` · `bin/suite.sh:417–445` · `aval.txt:1` |
| **Vizinhos** | Teste novo já commitado na `wt`, ausente de `<base>`, com um vizinho existente. Mesmo recorte `--dir … --volta …`, pendente, uma vez em `ROTA VM` e outra com pista aceita. | O novo sai como `obra` e chega aos dois executores. Antes do commit o diff não o nomeia, embora o glob ainda possa achá-lo como vizinho. | Na VM roda o existente, inclusive se uma label de pacote cobrir o novo. Na pista o mesmo novo continua na seleção. `bin/vizinhos_da_volta.py:25–45,92–95,116–132` · `bin/suite.sh:441–445,502–507,560–569` · `aval.txt:1` |

- **Pre-push e carimbo.** `bin/hooks.sh` instala wrappers para o `pre-push` da árvore chamadora e para o `commit-msg` da principal. `bin/pre-push.sh` testa o SHA empurrado em um `git archive`, lê `LABELS` da régua e pode pular o arsenal se o carimbo `OK` corresponder à árvore e ao commit. Um carimbo de corrida com exclusões pode, portanto, fazer o push pular testes que nunca rodaram. A variante em `app/core/hooks/pre-push.sh` lê a mesma linha, monta a viva e usa o carimbo por impressão digital. Qual hook está instalado [nao verificado]. `bin/hooks.sh:63–83` · `bin/pre-push.sh:27–45,50–74,142–145,184–199` · `app/core/hooks/pre-push.sh:18–25,54–55,80–88`
- **Outros leitores de `LABELS`.** `suite.sh`, vigia da árvore, réguas de relógio e calendário, molde de autocura e certificador da pista leem a declaração literal. Trocar seu formato quebra esses leitores e o selo da fonte única. Mudar a seleção dentro da régua deixa independentes as chamadas Docker do vigia da árvore e dos hooks. O certificador também exige que o argv corresponda a `LABELS` e que `Found − QUARENTENA-PODADO = Ran`. `bin/suite.sh:195–196` · `bin/vigia_arvore.sh:75–88,101–149` · `bin/regua_relogio.sh:27–29,51–55` · `bin/regua_calendario.sh:22–26` · `bin/molde_autocura/rodar.sh:22–26` · `bin/pista_certifica.sh:67–69,83–92` · `bin/tests/test_labels_fonte_unica.sh:20–35,80–95`
- **Volta intermediária e vigia.** `volta_intermediaria.sh` faz Ruff e o selo do disjuntor na VM, chama `suite.sh --dir … --volta …`, conta labels em `.porque` e agrega o rc. Se a seleção esvaziar antes da guarda, a volta já trata isso como vermelho. Se esvaziar depois e cair em `${ONLY:-$LABELS}`, reabre a suíte padrão. No vigia, reutilizar o cache por `mtime` após mudar aprovação ou pouso pode manter a classificação antiga. Passar uma lista vazia ao Docker ou tratá-la como reprovação também altera a classificação. `bin/volta_intermediaria.sh:18–46` · `bin/suite.sh:206–216,560–569` · `app/core/esteira_vigia.py:125–144,725–762`

## 27/11 08:41 · sessao 60054934ae3a · SONDA-ESPURIAS-TXT11 · leitura · origem=Code

1. **Como tirar a batida e reler o dia.** A porta da casa é `ponto.registro_batida.retratar_batida`. Ela preenche `retratada_em`, põe `aprovada=False` e agenda o recálculo para `on_commit`. `turnos.batidas_apuraveis` exclui a marca pelo `retratada_em` preenchido. Na sonda, B altera temporariamente só esse campo nos IDs da s1. Isso permite executar a mesma sequência explícita em A e B, sem disparar em B os julgamentos por batida ligados ao `save`. [app/ponto/registro_batida.py:255-301] [app/ponto/turnos.py:2941-2968] [app/ponto/signals.py:20-36,108-117,136-145] [app/ponto/services/fechamento.py:1822-1848]

   A sequência é `nucleo.recompute_turnos` → `cartorio.julgar_colab(apply_=True, forcar=True)` → `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False)` → `classificar_export` → `montar_linhas_export` → `validar_layout_dominio` → `conteudo_do_txt`. O cartório relavra a ata e o veredito das células existentes. O fechamento usa as batidas e as células na apuração, e o export lê a célula para decidir retenção. A seleção da s1 é feita uma vez antes das passadas. O script acompanha o PK até a sobra emitida pela grade, preservando marcas distintas no mesmo minuto e marcas atribuídas a um turno que cruza a meia-noite. [app/ponto/nucleo.py:40-67] [app/ponto/services/cartorio.py:959-981,1187-1209,1905-1932] [app/ponto/services/fechamento.py:624-653,762-775,1037-1038,1152-1160] [app/folha/views.py:214-219,272-288] [app/folha/export.py:977-983,1256-1305] [app/escala/utils.py:623-681,1747-1782,1863-1902,2057-2094]

2. **O ensaio precisa de `READ WRITE`.** O núcleo escreve `TurnoMaterializado`, e o cartório escreve ata, veredito, trilha e outbox. O recálculo em memória e a montagem do TXT são os do molde. A aprovação em `FechamentoMensal` e o registro em `ExportacaoDominio` pertencem à view HTTP. A sonda usa `atomic(durable=True)` com erro sentinela no fim, e confere contagens e impressões das linhas antes e depois, inclusive nas duas tabelas da view. As sequências são reiniciadas dentro de cada savepoint para que a conferência também cubra seus valores. Execução e reversão na sombra [nao verificado]. [app/ponto/nucleo.py:57-66] [app/ponto/portas/celula.py:981-1002,1133-1140,1218-1239] [app/core/observ.py:124-138] [app/ponto/services/fechamento.py:1152-1168,1215-1225] [app/folha/views.py:253-263,292-307]

3. **Determinismo é medido.** A1, A2, B1 e B2 partem do mesmo estado, com o mesmo relógio, e são comparados byte a byte. Há ordenações sem desempate por PK no universo do fechamento e na leitura das batidas. A igualdade efetiva de cada lado é [nao verificado]. `BASE`, em stderr, compara A1 ao TXT produzido antes de relavrar. Se der `False`, o teto publicado mede o efeito das sobras entre as duas passadas relavradas, e a diferença do TXT corrente fica explícita. [app/ponto/services/fechamento.py:459-465,634-639] [app/ponto/turnos.py:2961-2968] [app/folha/export.py:525-539,725-755] [nao verificado]

`TXT_RUBRICAS_MUDADAS` conta pares `(colab_id, rubrica)` com diferença nos registros 10/11, como no molde. `RUBRICA` soma por código e mostra `B − A`. Horas saem dos centésimos do TXT convertidos em minutos. Faltas usam os minutos das datas filhas pelo mapa de faltas da casa. Uma troca de datas ou deltas que se anulam pode aumentar a contagem sem gerar uma linha `RUBRICA`. As até 20 linhas `COLAB` são um limite global. As medidas das empresas 2, 3 e 4 estão [nao verificado]. [app/folha/export.py:34-45,95-143,215-255,733-755]

### SCRIPT

```python
import logging
import sys
from collections import defaultdict
from contextlib import redirect_stderr, redirect_stdout
from datetime import date, datetime, time, timedelta
from decimal import Decimal
from io import StringIO
from traceback import extract_tb
from types import SimpleNamespace
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from chamados import fcm_utils, push_utils [nao verificado]
from chamados.models import (
    ChamadoColaborador, DecisaoPergunta, DisputaSupervisao,
    FollowupChamado, PerguntaDisputa,
)
from colaboradores.models import Colaborador, Empresa
from core.models import LogAuditoria
from escala import utils as grade
from escala.models import CelulaDia, EventoCelula
from escala.orfa_classe import classificar_orfas
from folha import export
from folha.models import ExportacaoDominio
from ponto import turnos
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.models import (
    Batida, DiaPago, FechamentoMensal, Justificativa, TurnoMaterializado,
)
from ponto.nucleo import recompute_turnos
from ponto.services import cartorio, fechamento

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA = (date(2026, 10, 21), date(2026, 11, 20))
REF_S1 = "[app/escala/orfa_classe.py:53-78] [app/escala/utils.py:972-979,2138-2144]"
REF_TXT = "[app/folha/export.py:34-45,215-255,725-755]"
REF_FALTA = "[app/folha/export.py:95-143,215-221]"
REF_SOMBRA = "[app/config/settings/sombra.py:47-65] [bin/sombra.sh:78-80,490-491]"
MODELOS = (
    Batida, CelulaDia, TurnoMaterializado, FechamentoMensal, DiaPago,
    ExportacaoDominio, LogAuditoria, EventoCelula, ChamadoColaborador,
    Justificativa, DisputaSupervisao, PerguntaDisputa, DecisaoPergunta,
    FollowupChamado,
)


class ReverterSonda(Exception):
    pass


class FalhaSonda(RuntimeError):
    pass


class HoraComId(str):
    pass


class InstanteComId(datetime):
    def strftime(self, formato):
        hora = super().strftime(formato)
        if formato == "%H:%M" and hasattr(self, "batida_id"):
            hora = HoraComId(hora)
            hora.batida_id = self.batida_id
        return hora


def log_sem_dados(_logger, registro):
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def alvos_da_s1(empresa, agora):
    ini, fim = export.janela_competencia(empresa, MES, ANO)
    if (ini, fim) != JANELA:
        raise FalhaSonda(
            f"Janela fora do aval empresa={empresa.pk} "
            "[app/folha/export.py:47-51] [app/ponto/janelas.py:54-57]"
        )
    primeiro = timezone.make_aware(datetime.combine(ini - timedelta(days=1), time.min))
    ultimo = timezone.make_aware(datetime.combine(fim + timedelta(days=3), time.min))
    ultimo -= timedelta(microseconds=1)
    ref = fim + timedelta(days=1)
    dias = (ref - ini).days + 1
    alvos = {}

    localtime_real = timezone.localtime
    pessoas = Colaborador.objects.filter(empresa=empresa).order_by("pk")
    with (
        patch.object(grade, "Exception", (), create=True),
        patch.object(turnos, "Exception", (), create=True),
    ):
        for colab in pessoas.iterator(chunk_size=200):
            batidas = list(turnos.batidas_apuraveis(colab, primeiro, ultimo))
            if not batidas:
                continue

            kwargs = dict(dias=dias, ref=ref, agora=agora)
            linhas = grade.montar_grade_prevista_periodo(colab, batidas, **kwargs)["dias"]
            if len(linhas) != dias:
                raise FalhaSonda(f"Grade fora da janela colab_id={colab.pk} {REF_S1}")
            classes = [classificar_orfas(linha) for linha in linhas[1:]]
            if not any(classes):
                continue

            # Só transporta a identidade. Os datetime e HH:MM mantêm seus valores.
            # [app/escala/utils.py:314-327,699-705,1367-1385,1766-1782,1882-1891]
            por_objeto = {id(b.timestamp): b.pk for b in batidas}

            def localtime_com_id(value=None, timezone=None):
                instante = localtime_real(value, timezone)
                pk = por_objeto.get(id(value))
                if pk is None or sys._getframe(1).f_globals is not vars(grade):
                    return instante
                marcado = InstanteComId(
                    instante.year, instante.month, instante.day,
                    instante.hour, instante.minute, instante.second,
                    instante.microsecond, tzinfo=instante.tzinfo, fold=instante.fold,
                )
                marcado.batida_id = pk
                return marcado

            with patch.object(timezone, "localtime", localtime_com_id):
                marcadas = grade.montar_grade_prevista_periodo(
                    colab, batidas, **kwargs,
                )["dias"]

            ids = set()
            for linha, marcada, classificacao in zip(
                linhas[1:], marcadas[1:], classes, strict=True,
            ):
                horas = marcada.get("orfas") or []
                if horas != (linha.get("orfas") or []):
                    raise FalhaSonda(f"Instrumentação mudou a grade colab_id={colab.pk} {REF_S1}")
                for hora in horas:
                    if hora in classificacao:
                        pk = getattr(hora, "batida_id", None)
                        if pk is None:
                            raise FalhaSonda(f"Sobra sem identidade colab_id={colab.pk} {REF_S1}")
                        ids.add(pk)
            alvos[colab.pk] = ids

    return alvos


def reler_celulas(empresa, alvo_colabs, agora):
    ini, fim = turnos.janela_com_pad(*JANELA)
    hoje = timezone.localdate(agora)
    emissor = Emissor(stdout=StringIO(), stderr=StringIO())
    pessoas = Colaborador.objects.filter(
        empresa=empresa, pk__in=alvo_colabs,
    ).order_by("pk")
    for colab in pessoas:
        recompute_turnos(colab, ini, fim)
        celulas = list(CelulaDia.objects.filter(
            colaborador=colab, data__range=(ini, fim), data__lt=hoje,
        ).order_by("data"))
        if celulas:
            cartorio.julgar_colab(
                colab, celulas, hoje=hoje, agora=agora, apply_=True,
                emissor=emissor, out=cartorio.contadores(), forcar=True,
            )


def medir_txt(empresa):
    with patch.object(fechamento, "Exception", fechamento._GradeSemCelula, create=True):
        fechs = fechamento.recalcular_fechamento_mes(
            MES, ANO, empresa_id=empresa.pk, somente_leitura=True,
            retornar_objeto=True, lavrar_dias_pagos=False,
        )
    objetos = list(fechs.values())
    itens = export.classificar_export(empresa, MES, ANO, fechamentos=objetos)

    montar = export.montar_linha_10
    campos = []

    def observar_linha_10(matricula, comp, rubrica, tipo, valor, cod_sistema):
        linha = montar(matricula, comp, rubrica, tipo, valor, cod_sistema)
        mat = str(matricula).zfill(10)
        cod = str(rubrica).zfill(4)
        inicio = len(f"10{mat}{comp}{cod}11")
        fim = -len(str(cod_sistema).zfill(10))
        campos.append((mat, cod, tipo, inicio, fim))
        return linha

    with patch.object(export, "montar_linha_10", observar_linha_10):
        linhas, entra = export.montar_linhas_export(
            empresa, MES, ANO, itens=itens, fechamentos=objetos,
        )
    erros = export.validar_layout_dominio(linhas)
    txt = export.conteudo_do_txt(linhas)

    por_matricula = defaultdict(list)
    for item in itens:
        cid = item["colaborador_id"]
        if cid in entra and fechs[cid].status in export.STATUS_NO_TXT:
            por_matricula[item["codigo_dominio"].zfill(10)].append(cid)

    grupos = defaultdict(list)
    pais = iter(campos)
    pai = None
    for linha in txt.splitlines():
        parte = export.partes_da_linha(linha)
        if linha.startswith("10"):
            mat, cod, tipo, inicio, fim = next(pais)
            if erros:
                cru = int(linha[inicio:fim])
            else:
                if parte is None or (parte["matricula"], parte["rubrica"]) != (mat, cod):
                    raise FalhaSonda(f"Registro 10 não confere {REF_TXT}")
                cru = parte["valor"]
            ids = por_matricula[mat]
            if len(ids) != 1 or not (cod.isascii() and cod.isdigit()):
                raise FalhaSonda(f"Autoria ou rubrica indeterminada {REF_TXT}")
            pai = (tipo, cru, [])
            grupos[ids[0], cod].append(pai)
        elif pai is not None and parte is not None and parte["reg"] == "11":
            pai[2].append(parte["data"])
        else:
            raise FalhaSonda(f"Registro filho inválido {REF_TXT}")
    if next(pais, None) is not None:
        raise FalhaSonda(f"Registro 10 sem linha no TXT {REF_TXT}")

    registros = {}
    minutos = {}
    faltas = {}
    for (cid, cod), grupo in grupos.items():
        chave = (cid, cod)
        registros[chave] = tuple(sorted(
            (cru, tuple(sorted(datas))) for _, cru, datas in grupo
        ))
        total = Decimal(0)
        for tipo, cru, datas in grupo:
            if tipo != "dias":
                total += Decimal(cru) * Decimal("0.6")
                continue
            if not datas:
                raise FalhaSonda(f"Rubrica em dias sem datas filhas colab_id={cid} {REF_FALTA}")
            if cid not in faltas:
                mapa = export.minutos_de_falta_por_dia(fechs[cid].colaborador, *JANELA)
                faltas[cid] = {d.strftime("%Y%m%d"): Decimal(str(m)) for d, m in mapa.items()}
            for dia in sorted(set(datas)):
                total += faltas[cid][dia]
        minutos[chave] = total

    return SimpleNamespace(txt=txt.encode("utf-8"), registros=registros,
                           minutos=minutos, erros_layout=len(erros))


def sequencias():
    # O banco da sombra é PostgreSQL 17. [bin/sombra_db.sh:45]
    quote = connection.ops.quote_name
    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT n.nspname, c.relname, s.seqincrement, s.seqmin, s.seqmax, s.seqcycle
            FROM pg_catalog.pg_sequence s
            JOIN pg_catalog.pg_class c ON c.oid = s.seqrelid
            JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
            WHERE n.nspname IN (%s, %s)
            ORDER BY n.nspname, c.relname
            """,
            ("juliani", "public"),
        )
        saida = {}
        for schema, nome, passo, minimo, maximo, ciclo in cursor.fetchall():
            tabela = f"{quote(schema)}.{quote(nome)}"
            cursor.execute(f"SELECT last_value, is_called FROM {tabela}")
            valor, chamado = cursor.fetchone()
            saida[tabela] = (valor, chamado, passo, minimo, maximo, ciclo)
    return saida


def reiniciar_no_savepoint(estado):
    with connection.cursor() as cursor:
        for nome, (valor, chamado, passo, minimo, maximo, ciclo) in estado.items():
            proximo = valor + passo if chamado else valor
            if ciclo and proximo > maximo:
                proximo = minimo
            if ciclo and proximo < minimo:
                proximo = maximo
            cursor.execute(f"ALTER SEQUENCE {nome} RESTART WITH {int(proximo)}")


def estado_das_linhas():
    quote = connection.ops.quote_name
    schema = quote(connection.schema_name)
    estado = {}
    with connection.cursor() as cursor:
        for modelo in MODELOS:
            tabela = quote(modelo._meta.db_table)
            pk = quote(modelo._meta.pk.column)
            cursor.execute(
                f"""
                SELECT count(*),
                       md5(coalesce(string_agg(md5(to_jsonb(t)::text), '' ORDER BY t.{pk}), ''))
                FROM {schema}.{tabela} AS t
                """
            )
            estado[modelo.__name__] = cursor.fetchone()
    return estado


def rodada(empresa, colabs, pks, agora, estado_sequencias):
    with transaction.atomic():
        reiniciar_no_savepoint(estado_sequencias)
        if pks:
            marcas = turnos.batidas_apuraveis(None).filter(
                colaborador__empresa=empresa, pk__in=pks,
            )
            # Perturbação de sombra antes da relavratura comum.
            # [app/ponto/turnos.py:2941-2968] [app/ponto/signals.py:34-36,136-140,211-220]
            tiradas = marcas.update(retratada_em=agora)
            if tiradas != len(pks) or marcas.exists():
                raise FalhaSonda("Retirada incompleta [app/ponto/turnos.py:2960-2968]")
        reler_celulas(empresa, colabs, agora)
        foto = medir_txt(empresa)
        transaction.set_rollback(True)
    return foto


def linhas_da_medida(empresa_id, a, a2, b, tiradas):
    zero = Decimal(0)
    mudadas = set()
    total_a, total_b = defaultdict(Decimal), defaultdict(Decimal)
    detalhes = []
    for chave in sorted(a.registros.keys() | b.registros.keys()):
        cid, cod = chave
        ma, mb = a.minutos.get(chave, zero), b.minutos.get(chave, zero)
        total_a[cod] += ma
        total_b[cod] += mb
        if a.registros.get(chave) != b.registros.get(chave):
            mudadas.add(chave)
            if ma != mb:
                detalhes.append(
                    f"COLAB empresa={empresa_id} colab_id={cid} rubrica={cod} delta_min={mb - ma:f}"
                )
        elif ma != mb:
            raise FalhaSonda(f"Unidade ambígua para registros iguais colab_id={cid} {REF_TXT}")

    linhas = [
        f"TETO empresa={empresa_id} TXT_RUBRICAS_MUDADAS={len(mudadas)} "
        f"COLABS_TXT={len({cid for cid, _ in mudadas})} BATIDAS_TIRADAS={tiradas}"
    ]
    for cod in sorted(total_a.keys() | total_b.keys()):
        ma, mb = total_a[cod], total_b[cod]
        if ma != mb:
            linhas.append(
                f"RUBRICA empresa={empresa_id} rubrica={cod} "
                f"min_A={ma:f} min_B={mb:f} delta_min={mb - ma:f}"
            )
    linhas.append(f"IDEMP empresa={empresa_id} A1==A2={a.txt == a2.txt}")
    return linhas, detalhes


def main():
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict["HOST"] != "sombra_db"
        or connection.settings_dict["NAME"] != "sombra"
        or connection.schema_name != "juliani"
    ):
        raise FalhaSonda(f"Conexão fora da sombra {REF_SOMBRA}")

    saida, detalhes, diagnosticos = [], [], []
    try:
        with patch.object(logging.Logger, "handle", log_sem_dados), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ WRITE")
                cursor.execute("SELECT current_database(), current_schema()")
                if cursor.fetchone() != ("sombra", "juliani"):
                    raise FalhaSonda(f"Banco ou schema fora da sombra {REF_SOMBRA}")

            antes = estado_das_linhas()
            seq_antes = sequencias()
            agora = timezone.now()
            empresas = list(Empresa.objects.filter(pk__in=EMPRESAS).order_by("pk"))
            if tuple(e.pk for e in empresas) != EMPRESAS:
                raise FalhaSonda("Escopo incompleto de empresas [app/colaboradores/models.py:40]")

            # Silencia os transportes. As portas de cálculo, cartório e TXT seguem reais.
            # [app/chamados/push_utils.py:12-18,89-111] [nao verificado]
            # [app/chamados/services/disputa_emissao.py:1887-1906]
            with (
                patch.object(timezone, "now", new=lambda: agora),
                patch.object(push_utils, "_enviar_push_usuario", return_value={"ok": False}), [nao verificado]
                patch.object(fcm_utils, "push_fcm_colaborador", return_value=False), [nao verificado]
                redirect_stdout(StringIO()),
                redirect_stderr(StringIO()),
            ):
                for empresa in empresas:
                    alvos = alvos_da_s1(empresa, agora)
                    pks = {pk for ids in alvos.values() for pk in ids}
                    original = medir_txt(empresa)
                    a1 = rodada(empresa, alvos, set(), agora, seq_antes)
                    a2 = rodada(empresa, alvos, set(), agora, seq_antes)
                    b1 = rodada(empresa, alvos, pks, agora, seq_antes)
                   

[truncado em 20000 de 21750 caracteres]

## 27/11 08:29 · sessao 1814e0d2b981 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**c2t parcial.** Apliquei a cura do item 2. Os REDs do Plantão e do banco Comercial ficaram reproduzidos e continuam falhando. As duas curas exigem edição do motor, que a ordem também põe na lista proibida. O teste existente do banco exige −3,5 h para o mesmo caso em que o item 3 pede −4 h. [ponto/motor_calculo_v2.py:3712] [ponto/motor_calculo_v2.py:3781] [ponto/tests/test_ramo2b_c2l_red_motor.py:96] [nao verificado]

### Item 2 · A igualdade é necessária

Reverter os dois `<=` em memória fez falhar `test_pausa_colada_na_entrada_efetiva_e_a_mesma_do_motor` e `test_segmentos_descontam_a_pausa_colada_na_entrada_efetiva`, no 12x36 e no Comercial. A janela recorta a pausa de 18:50–19:20 para 19:00–19:20, colada à entrada efetiva. Com `<`, os dois leitores descartam os 20 min de pausa e devolvem 800 min trabalhados onde o motor devolve 780. [ponto/tests/test_ramo2b_c2h_saida.py:230] [ponto/tests/test_ramo2b_c2h_saida.py:247] [ponto/tests/test_ramo2b_c2h_saida.py:266]

O diff restringe a igualdade ao **dia da jornada** em que algum par tem `saida_da_base_da_he` preenchida. Os dois REDs novos falharam antes da cura porque o dia sem cauda devolvia 100 min em vez de 120. Ambos passaram depois, inclusive com a cauda registrada em outro par do mesmo dia. [ponto/calculador/alimentacao.py:82] [ponto/calculador/alimentacao.py:146] [ponto/calculador/periodo.py:307] [ponto/calculador/periodo.py:625] [ponto/tests/test_ramo2b_c2t_escopo.py:27]

### Plantão e banco Comercial

| Verificação | Resultado |
|---|---|
| Plantão 07:00–07:00(+1), batida às 07:40 | Saída efetiva às 07:40, 1.480 min trabalhados e 580 noturnos. O `main` deu 07:00, 1.440 e 540. A base da HE ficou em 1.440 min no caso testado. [ponto/tests/test_ramo2b_c2t_motor.py:14] |
| Pontualidade do Plantão | O cálculo por par delega ao `Motor12x36.calcular_periodo`, que termina sem chamar `_julgar_pontualidade`. O mensal herdado passa pelo teto L-093. O motor também compartilha o recorte da HE noturna e o crédito de pares fechados em dia aberto. [ponto/motor_calculo_v2.py:3804] [ponto/motor_calculo_v2.py:3099] [ponto/motor_calculo_v2.py:2944] [ponto/motor_calculo_v2.py:1100] [ponto/motor_calculo_v2.py:2763] |
| Limite diário e banco do Plantão | O cálculo delegado usa `_jornada_do_dia`. O `_lim_dia` da c2m e a gravação do saldo semanal pertencem ao override do Comercial. [ponto/motor_calculo_v2.py:3079] [ponto/motor_calculo_v2.py:3672] [ponto/motor_calculo_v2.py:3725] |
| Comercial com cauda de 30 min | O saldo atual deu −3,5 h e o `main` deu −4 h. No cálculo atual, o RED confirmou 2.430 min trabalhados, 2.400 min na base e HE zero antes de falhar no saldo. [ponto/tests/test_ramo2b_c2t_motor.py:39] |

### Execução e diff

Em memória, com as dependências do Django substituídas apenas no ensaio, **11 testes passaram**. Foram os dois REDs do item 2 após a cura, os cinco de `ReleituraDaBaseDaHeTest`, o da pausa com base curta e os três de virada da pontualidade. Os quatro mutantes abaixo deram **rc 1 por asserção** no mesmo ensaio. A execução normal de `test_ramo2b_*` e `test_o280_bateria_da_lei` parou na importação de `django.core`, antes de rodar testes. [ponto/tests/test_ramo2b_c2t_escopo.py:36] [ponto/tests/test_ramo2b_c2h_saida.py:199] [ponto/tests/test_ramo2b_c2l_red_motor.py:63] [ponto/tests/test_ramo2b_c2l_red_motor.py:113] [ponto/tests/ramo2b_c2t_mutantes.py:17] [manage.py:11] [nao verificado]

Comandos para a suíte e os mutantes com Django disponível. [manage.py:18] [ponto/tests/ramo2b_c2t_mutantes.py:31] [ponto/tests/ramo2b_c2l_mutantes.py:68] [ponto/tests/ramo2b_c2n_mutantes.py:51] [nao verificado]

```sh
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci

python3 manage.py test \
  $(for f in ponto/tests/test_ramo2b_*.py; do printf 'ponto.tests.%s\n' "$(basename "$f" .py)"; done) \
  ponto.tests.test_o280_bateria_da_lei

python3 -m ponto.tests.ramo2b_c2t_mutantes segmentos_aceitam_pausa_sem_cauda
python3 -m ponto.tests.ramo2b_c2t_mutantes releitura_aceita_pausa_sem_cauda
python3 -m ponto.tests.ramo2b_c2n_mutantes ignora_pausa_na_entrada
python3 -m ponto.tests.ramo2b_c2l_mutantes pausa_na_ponta_descartada
```

O diff tem cinco arquivos e **zero `except` novos**, incluindo os arquivos novos. [ponto/calculador/alimentacao.py:82] [ponto/calculador/periodo.py:291] [ponto/tests/test_ramo2b_c2t_escopo.py:1] [ponto/tests/test_ramo2b_c2t_motor.py:1] [ponto/tests/ramo2b_c2t_mutantes.py:1]

```sh
git --no-optional-locks diff --relative HEAD
for f in ponto/tests/ramo2b_c2t_mutantes.py ponto/tests/test_ramo2b_c2t_*.py; do
  git --no-optional-locks diff --no-index -- /dev/null "$f"
done
```

## 27/11 08:28 · sessao 92b2ddb05e4c · RAIA-ESPURIAS-1811-1410 · leitura · origem=Code

**O efeito das espúrias no TXT da competência 11 está [nao verificado]** para as empresas 2, 3 e 4. A sonda abaixo mede os dias com sobras vivas na grade. A execução na sombra e a correspondência da cópia com o hash informado estão [nao verificado].

**1. Autoridade e contagem.** O próprio `JUIZES.md` aponta para o registro vivo. Nele, a pergunta «a batida é espúria (cluster)?» está em `SEM_JUIZ['batida']`. As duas candidatas que o registro nomeia são `detectar_cluster_espurio`, que delega a `dias_com_cluster` e à assinatura da ata em `_leitura_da_celula` (duas ou mais órfãs com consecutivas a até 15 minutos e ao menos uma lâmpada apagada, em dia de trabalho), e a guarda de `_match_marcos`, que marca como órfã a batida que perde o marco pela disputa de proximidade, ressalvada a exceção da guarda. O propositor também tem um rótulo `ESPURIA` próprio para sobras próximas de um slot ocupado. [app/docs/JUIZES.md:10] [app/core/juizes.py:1231] [app/ponto/management/commands/detectar_cluster_espurio.py:26] [app/ponto/services/cluster_espurio.py:15] [app/ponto/services/cluster_espurio.py:40] [app/escala/utils.py:595] [app/escala/utils.py:654] [app/ponto/propositor.py:169]

Para o passo 3a, usei **espúria no sentido de órfã da grade**, como no cartão. `classificar_orfas` é a função nomeada no aval da razão da sobra e devolve as classes das órfãs que a grade já encontrou. O script conta um dia-colab quando esse retorno tem alguma classe, inclusive `indefinida`. A grade é refeita com `batidas_apuraveis`, cujo predicado de vida é `retratada_em__isnull=True`. Assim, a contagem mede sobras da grade alimentada por batidas vivas. A ata do cluster guarda horas `HH:MM`, o que limita a identificação individual de uma batida. [app/relatorios/pdf_espelho.py:1027] [app/docs/BACKLOG.md:2087] [app/escala/orfa_classe.py:53] [app/ponto/turnos.py:2941] [app/ponto/turnos.py:2961] [app/ponto/services/cartorio.py:761]

**2. Lei e lado B.** A **L-038**, pelo corte `PDF-SEM-REGRA-PROPRIA`, manda mostrar a batida que nenhum turno reclama no dia em que aconteceu. `dia_das_batidas` aplica isso para a apresentação, e a grade põe a sobra sem turno na sua data civil. A **L-025** define o universo da apuração pelo filtro de retratação. O fechamento já usa essa entrada. O motor também já lê papéis da ata, com cobertura parcial. O papel `X` inclui casos de pausa e por isso é insuficiente como seleção de espúrias. [app/docs/LEIS.md:86] [app/docs/CORTES.md:328] [app/ponto/turnos.py:2971] [app/ponto/turnos.py:3034] [app/escala/utils.py:1864] [app/docs/LEIS.md:76] [app/ponto/services/fechamento.py:652] [app/ponto/motor_calculo_v2.py:521] [app/ponto/turnos.py:1768]

O lado A da view usa `recalcular_fechamento_mes` em leitura, `classificar_export`, `montar_linhas_export`, validação do layout e `conteudo_do_txt`. Para o lado B, essa mesma sequência precisaria receber, na entrada do fechamento, as batidas já julgadas pela regra para toda a janela. Não encontrei essa porta na cópia. `dia_das_batidas` devolve datas para apresentação e `classificar_orfas` devolve classes por `HH:MM`. A troca de entrada exigiria decidir quais batidas correspondem às sobras e o que fazer com elas na apuração. Aplico o fallback pedido, só com o passo 3a. O juiz de dono disponível para o passo 3c é `dono_da_divergencia`, chamado por `InsumosDoDono.dono`. [app/folha/views.py:214] [app/folha/views.py:272] [app/folha/views.py:282] [app/ponto/services/fechamento.py:652] [app/ponto/turnos.py:2971] [app/escala/orfa_classe.py:53] [app/ponto/management/commands/e6_oraculo.py:53] [app/ponto/management/commands/e6_oraculo.py:182]

### SCRIPT

```python
import logging
import sys
from datetime import date, datetime, time, timedelta
from traceback import extract_tb
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from escala import utils as grade
from escala.orfa_classe import classificar_orfas
from folha.export import janela_competencia
from ponto import turnos

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA_DO_AVAL = (date(2026, 10, 21), date(2026, 11, 20))

# Passo 3a. A razão da sobra vem por HH:MM e a entrada da folha usa Batida.
# [app/escala/orfa_classe.py:53] [app/ponto/services/fechamento.py:652]


class ReverterSonda(Exception):
    pass


class FalhaSonda(RuntimeError):
    pass


def log_sem_dados(_logger, registro):
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def contar(empresa, agora):
    ini, fim = janela_competencia(empresa, MES, ANO)
    if (ini, fim) != JANELA_DO_AVAL:
        raise FalhaSonda(
            f"Empresa {empresa.pk}: janela {ini}/{fim} fora do aval "
            "[app/folha/export.py:47] [app/ponto/janelas.py:54]"
        )

    # Entrada e bordas da grade do cartório. [app/ponto/services/cartorio.py:1187-1209]
    primeiro = timezone.make_aware(datetime.combine(ini - timedelta(days=1), time.min))
    depois_do_ultimo = timezone.make_aware(datetime.combine(fim + timedelta(days=3), time.min))
    ultimo = depois_do_ultimo - timedelta(microseconds=1)
    ref = fim + timedelta(days=1)
    dias = (ref - ini).days + 1

    colabs = dias_colab = 0
    pessoas = Colaborador.objects.filter(empresa=empresa).order_by("pk")
    for colab in pessoas.iterator(chunk_size=200):
        batidas = list(turnos.batidas_apuraveis(colab, primeiro, ultimo))
        if not batidas:
            continue
        linhas = grade.montar_grade_prevista_periodo(
            colab, batidas, dias=dias, ref=ref, agora=agora,
        )["dias"]
        if len(linhas) != dias:
            raise FalhaSonda(
                f"Empresa {empresa.pk} colab={colab.pk}: grade fora da janela "
                "[app/escala/utils.py:972] [app/ponto/services/cartorio.py:1203]"
            )
        # linhas[0] é o dia de contexto após o fim. [app/escala/utils.py:972-979]
        n = sum(bool(classificar_orfas(linha)) for linha in linhas[1:])
        colabs += int(n > 0)
        dias_colab += n
    return colabs, dias_colab


def main():
    try:
        with patch.object(logging.Logger, "handle", log_sem_dados), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            if connection.schema_name != "juliani":
                raise FalhaSonda("Schema esperado juliani [bin/sombra.sh:490]")

            agora = timezone.now()
            # Deixa subir falhas capturadas nesses leitores.
            # [app/escala/utils.py:192,927,1242] [app/ponto/turnos.py:1634]
            with (
                patch.object(timezone, "now", new=lambda: agora),
                patch.object(grade, "Exception", (), create=True),
                patch.object(turnos, "Exception", (), create=True),
            ):
                empresas = list(Empresa.objects.filter(pk__in=EMPRESAS).order_by("pk"))
                if tuple(e.pk for e in empresas) != EMPRESAS:
                    raise FalhaSonda("Escopo incompleto de empresas [app/colaboradores/models.py:40]")
                medidas = [(e.pk, *contar(e, agora)) for e in empresas]
            raise ReverterSonda()
    except ReverterSonda:
        for empresa_id, colabs, dias_colab in medidas:
            print(f"ESPURIAS empresa={empresa_id} colabs={colabs} dias_colab={dias_colab}", flush=True)
    except FalhaSonda:
        raise
    except Exception as erro:
        origem = extract_tb(erro.__traceback__)[-1]
        raise FalhaSonda(f"{origem.filename}:{origem.lineno} {type(erro).__name__}") from None


main()
```

### FIM

## 27/11 07:41 · sessao 2b9617406233 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

```diff
diff --git a/app/ponto/tests/test_ramo2b_c2l_red_motor.py b/app/ponto/tests/test_ramo2b_c2l_red_motor.py
--- a/app/ponto/tests/test_ramo2b_c2l_red_motor.py
+++ b/app/ponto/tests/test_ramo2b_c2l_red_motor.py
@@ -9 +9 @@
-from folha.tests.test_porta_do_export import _medir_com_espelho
+from ponto.tests.test_ramo2b_porta_7a_testemunha import _medir_com_espelho
```

O `rg` encontrou uma referência indireta ao caminho antigo em `app/ponto/tests/ramo2b_c2l_mutantes.py:57`. O script passa esse caminho ao carregador de testes em `app/ponto/tests/ramo2b_c2l_mutantes.py:77`, e a classe está em `app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:43`. O diff se limita ao import solicitado.

## 27/11 06:59 · sessao 684d56ff6bd6 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Movi `_medir_com_espelho` e `TestemunhaDaHeNaPortaTest` para `ponto/tests`. Conferi que o helper, os patches e as asserções foram preservados byte a byte. A mudança ficou restrita a dois arquivos.

- [app/folha/tests/test_porta_do_export.py:15](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/tests/test_porta_do_export.py:15) — removi o bloco e os imports de módulo que ficaram sem uso. O trecho de `he_acima_do_teto_por_dia` foi preservado. [Trecho preservado](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/tests/test_porta_do_export.py:133)
- [app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:1](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:1) — adicionei a docstring de duas linhas e o bloco movido. [Helper](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:11) · [Testemunha](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:43)

### RED e mutantes

O RED falhou na base `c57f01232` com o achado `('folha/tests/test_porta_do_export.py', 'MotorBase')`. Após a mudança, os quatro testes do selo passaram. [Asserção do selo](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_selo_bolha_nao_importa.py:27)

```bash
# RED na base
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest core.tests.test_selo_bolha_nao_importa.SeloBolhaNaoImportaTest.test_MORDE_a_bolha_da_folha_nao_importa_o_motor

# Selo completo após a cura
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning -m unittest core.tests.test_selo_bolha_nao_importa
```

**Mutante 1.** Reintroduzi o import em `folha/` na AST em memória. O selo voltou a falhar pelo achado esperado. [Varredura de imports](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/mapa/contrato.py:128)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning - <<'PY'
import ast
import unittest
from pathlib import Path
from unittest.mock import patch
from core.mapa import contrato

folha = 'folha/tests/test_porta_do_export.py'
arvore = contrato._arvore
mutante = ast.parse(Path(folha).read_text() + '\nfrom ponto.motor_calculo_v2 import MotorBase\n')
alvo = ('core.tests.test_selo_bolha_nao_importa.SeloBolhaNaoImportaTest.'
        'test_MORDE_a_bolha_da_folha_nao_importa_o_motor')
with patch.object(contrato, '_arvore', side_effect=lambda rel: mutante if rel == folha else arvore(rel)):
    resultado = unittest.TextTestRunner().run(unittest.defaultTestLoader.loadTestsFromName(alvo))
assert resultado.testsRun == 1 and not resultado.errors
assert len(resultado.failures) == 1 and repr((folha, 'MotorBase')) in resultado.failures[0][1]
PY
```

**Mutante 2.** Omiti a classe no texto em memória. A conferência estrutural detectou a perda da testemunha. [Classe e método](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:43)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
from pathlib import Path

p = Path('ponto/tests/test_ramo2b_porta_7a_testemunha.py')
fonte = p.read_text()
classe = 'TestemunhaDaHeNaPortaTest'
metodo = 'test_he_da_cauda_entra_na_lista_e_nas_falhas_da_porta'

def contem_testemunha(texto):
    return any(
        isinstance(n, ast.ClassDef) and n.name == classe
        and any(isinstance(m, ast.FunctionDef) and m.name == metodo for m in n.body)
        for n in ast.parse(texto).body
    )

assert contem_testemunha(fonte)
mutante = fonte.split(f'\nclass {classe}(', 1)[0]
assert not contem_testemunha(mutante)
PY
```

A execução local da testemunha parou no import porque o Python disponível não tem `django.test`. A execução da testemunha e da suíte completa fica para a certificação do Code e do Opus final [nao verificado]. [Import da testemunha](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:8) [nao verificado]

### Diff

```diff
diff --git a/app/folha/tests/test_porta_do_export.py b/app/folha/tests/test_porta_do_export.py
--- a/app/folha/tests/test_porta_do_export.py
+++ b/app/folha/tests/test_porta_do_export.py
@@ -14,68 +14,11 @@ seriam duas contas para a mesma pergunta -- a doenca que esta serie inteira pers
 """
 import datetime
 import inspect
-from types import SimpleNamespace
-from unittest.mock import MagicMock, Mock, patch
-from zoneinfo import ZoneInfo
 
-from django.test import SimpleTestCase, TestCase
+from django.test import TestCase
 from freezegun import freeze_time
 
 
-def _medir_com_espelho(resultado, *, previsto, declarado):
-    """Mede o espelho fornecido pelo caminho da porta, com as fronteiras de I/O em memória."""
-    from colaboradores.models import Colaborador
-    from escala.models import CelulaDia
-    from folha.porta_export import medir
-    from ponto.models import DiaPago, FechamentoMensal
-
-    empresa = SimpleNamespace(pk=1, dia_inicio_competencia=21)
-    colab = SimpleNamespace(pk=125)
-    sem_linhas = Mock(first=Mock(return_value=None), values_list=Mock(return_value=[]))
-    agora = datetime.datetime(2026, 11, 23, 12, tzinfo=ZoneInfo('America/Sao_Paulo'))
-
-    def cartao_x_txt(*_args, stdout, **_kwargs):
-        stdout.write('cartao_x_txt_divergentes=0\n')
-
-    with (patch('folha.porta_export._tz_ato.now', return_value=agora),
-          patch('folha.export.classificar_export', return_value=[{'status': 'entra', 'colaborador_id': colab.pk}]),
-          patch.object(Colaborador.objects, 'filter', return_value=Mock(first=Mock(return_value=colab))),
-          patch.object(CelulaDia.objects, 'filter', return_value=[]),
-          patch.object(DiaPago.objects, 'filter', return_value=MagicMock(values_list=Mock(return_value=[]))),
-          patch.object(FechamentoMensal.objects, 'filter', return_value=sem_linhas),
-          patch('ponto.services.espelho.espelho_do_colab', return_value={'resultado': resultado}),
-          patch('ponto.services.espelho.autoridade_do_periodo', return_value=SimpleNamespace(resultado=resultado)),
-          patch('relatorios.pdf_espelho._coletar_dados_espelho_mes', return_value={}),
-          patch('ponto.services.fechamento.empresas_exportadas_no_escopo', return_value=[]),
-          patch('ponto.services.dia_pago.soma_da_competencia', return_value=None),
-          patch('escala.utils.minutos_previstos_do_dia', return_value=previsto),
-          patch('escala.utils.intervalo_declarado_do_dia', return_value=declarado),
-          patch('django.core.management.call_command', side_effect=cartao_x_txt)):
-        return medir(empresa, 11, 2026)
-
-
-class TestemunhaDaHeNaPortaTest(SimpleTestCase):
-    def test_he_da_cauda_entra_na_lista_e_nas_falhas_da_porta(self):
-        # L-097(b), L-031: trabalhado 800, base e previsto 720, intra 60,
-        # declarado 0. HE 80 passa do teto 0 e aciona a sétima testemunha.
-        from ponto.motor_calculo_v2 import MotorBase, PeriodoCalculo
-
-        entrada = datetime.datetime(2026, 11, 5, 19, tzinfo=ZoneInfo('America/Sao_Paulo'))
-        p = PeriodoCalculo(entrada, entrada + datetime.timedelta(minutes=800),
-                           minutos_trabalhados=800, minutos_intrajornada_indenizada=60, minutos_extra=80)
-        MotorBase._escrever_base_da_he(p, 720, entrada + datetime.timedelta(minutes=720), 0)
-        resultado = SimpleNamespace(periodos=[p], periodos_ft=[], dia_da_jornada={id(p): entrada.date()})
-
-        medicao = _medir_com_espelho(resultado, previsto=720, declarado=0)
-
-        self.assertEqual(medicao['universo'], 1)
-        self.assertEqual(medicao['minuto_em_duas_rubricas'], [
-            {'colab': 125, 'dia': '2026-11-05', 'he_min': 80, 'intra_min': 60,
-             'trab_min': 800, 'previsto_min': 720, 'teto_he': 0},
-        ])
-        self.assertEqual((medicao['falhas_proprias'], medicao['falhas']), (1, 1))
-
-
 # RELOGIO CONGELADO: este arquivo fala de "carimbo de HOJE" e "carimbo de ONTEM", entao ele LE o relogio por
 # construcao -- e o selo `test_selo_teste_sem_relogio::test_MORDE_teste_novo_com_relogio_solto_e_vermelho` me
 # cobrou na hora, com razao. Congelado, as duas datas sao deterministicas e a lei do carimbo continua medida.
diff --git a/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py b/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py
new file mode 100644
--- /dev/null
+++ b/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py
@@ -0,0 +1,62 @@
+"""Este caso mora em ponto/ porque constrói o PeriodoCalculo pelo motor.
+A bolha da folha não importa o motor (selo test_selo_bolha_nao_importa)."""
+import datetime
+from types import SimpleNamespace
+from unittest.mock import MagicMock, Mock, patch
+from zoneinfo import ZoneInfo
+
+from django.test import SimpleTestCase
+
+
+def _medir_com_espelho(resultado, *, previsto, declarado):
+    """Mede o espelho fornecido pelo caminho da porta, com as fronteiras de I/O em memória."""
+    from colaboradores.models import Colaborador
+    from escala.models import CelulaDia
+    from folha.porta_export import medir
+    from ponto.models import DiaPago, FechamentoMensal
+
+    empresa = SimpleNamespace(pk=1, dia_inicio_competencia=21)
+    colab = SimpleNamespace(pk=125)
+    sem_linhas = Mock(first=Mock(return_value=None), values_list=Mock(return_value=[]))
+    agora = datetime.datetime(2026, 11, 23, 12, tzinfo=ZoneInfo('America/Sao_Paulo'))
+
+    def cartao_x_txt(*_args, stdout, **_kwargs):
+        stdout.write('cartao_x_txt_divergentes=0\n')
+
+    with (patch('folha.porta_export._tz_ato.now', return_value=agora),
+          patch('folha.export.classificar_export', return_value=[{'status': 'entra', 'colaborador_id': colab.pk}]),
+          patch.object(Colaborador.objects, 'filter', return_value=Mock(first=Mock(return_value=colab))),
+          patch.object(CelulaDia.objects, 'filter', return_value=[]),
+          patch.object(DiaPago.objects, 'filter', return_value=MagicMock(values_list=Mock(return_value=[]))),
+          patch.object(FechamentoMensal.objects, 'filter', return_value=sem_linhas),
+          patch('ponto.services.espelho.espelho_do_colab', return_value={'resultado': resultado}),
+          patch('ponto.services.espelho.autoridade_do_periodo', return_value=SimpleNamespace(resultado=resultado)),
+          patch('relatorios.pdf_espelho._coletar_dados_espelho_mes', return_value={}),
+          patch('ponto.services.fechamento.empresas_exportadas_no_escopo', return_value=[]),
+          patch('ponto.services.dia_pago.soma_da_competencia', return_value=None),
+          patch('escala.utils.minutos_previstos_do_dia', return_value=previsto),
+          patch('escala.utils.intervalo_declarado_do_dia', return_value=declarado),
+          patch('django.core.management.call_command', side_effect=cartao_x_txt)):
+        return medir(empresa, 11, 2026)
+
+
+class TestemunhaDaHeNaPortaTest(SimpleTestCase):
+    def test_he_da_cauda_entra_na_lista_e_nas_falhas_da_porta(self):
+        # L-097(b), L-031: trabalhado 800, base e previsto 720, intra 60,
+        # declarado 0. HE 80 passa do teto 0 e aciona a sétima testemunha.
+        from ponto.motor_calculo_v2 import MotorBase, PeriodoCalculo
+
+        entrada = datetime.datetime(2026, 11, 5, 19, tzinfo=ZoneInfo('America/Sao_Paulo'))
+        p = PeriodoCalculo(entrada, entrada + datetime.timedelta(minutes=800),
+                           minutos_trabalhados=800, minutos_intrajornada_indenizada=60, minutos_extra=80)
+        MotorBase._escrever_base_da_he(p, 720, entrada + datetime.timedelta(minutes=720), 0)
+        resultado = SimpleNamespace(periodos=[p], periodos_ft=[], dia_da_jornada={id(p): entrada.date()})
+
+        medicao = _medir_com_espelho(resultado, previsto=720, declarado=0)
+
+        self.assertEqual(medicao['universo'], 1)
+        self.assertEqual(medicao['minuto_em_duas_rubricas'], [
+            {'colab': 125, 'dia': '2026-11-05', 'he_min': 80, 'intra_min': 60,
+             'trab_min': 800, 'previsto_min': 720, 'teto_he': 0},
+        ])
+        self.assertEqual((medicao['falhas_proprias'], medicao['falhas']), (1, 1))
```

## 27/11 06:51 · sessao 6c7f3f7c903e · RAMO2-B-EM-TURNO-QUE-CRUZA · leitura · origem=Code

**O atraso aparece porque a obra muda o dia usado para buscar o marco de entrada.** A mudança de `efetivo_saida` explica a passagem da cauda de `fora` para `trabalhado`. O cálculo da pontualidade recebe a **saída bruta** nas duas versões. `app/ponto/motor_calculo_v2.py:2166,2206,2330` no main → `app/ponto/motor_calculo_v2.py:2265–2267,2307,2431` na obra. As linhas do main abaixo foram reconstituídas pelo `diff.txt`.

### 1. Onde o main deixava o atraso em zero

No main, `Motor12x36ComEscala` chama `_julgar_pontualidade`, que consulta `marcos_do_dia(e.date())` e ancora a entrada prevista em `combine(e.date(), hora_prev)`. Se `hora_prev` for falsy, retorna com o atraso inicial em zero. Se houver marco, a condição para lançar atraso é `_de is not None and _de > 10`, com `_de = entrada_bruta − prevista_entrada`. Assim, se o main encontrou os horários informados para o 218 de 15/11 e o 200 de 03/11 na própria data civil, comparou `03:17:11` com `21:00` e `02:00:33` com `22:00` **do mesmo dia**. Os dois deltas são negativos. Os marcos que a consulta civil retornou de fato, inclusive para o 218 de 22/11, 296, 601 e 841, estão **[nao verificado]**. `dias.txt:3–10`; `app/ponto/motor_calculo_v2.py:60,226,2087,2119,2166,2179–2180,2206,3674` no main → `app/ponto/motor_calculo_v2.py:60,226,2182,2214,2265–2267,2280–2281,2307,3827` na obra.

Os hunks `@@ -2162,8 +2257,14`, `@@ -2203,7 +2304,7` e `@@ -2228,7 +2329,7` passam `_dia_j`, obtido de `_dia_do_turno_do_par(entrada)`, à consulta dos marcos, à data do marco de entrada e ao teste de volta do intervalo. A L-084 só devolve `(0, 0)` se **ambas** as distâncias aos marcos existirem e tiverem módulo **maior que 180 min**. Com o `hf` correto e as saídas próximas dele no anexo, a ponta de saída fica abaixo desse limiar. A igualdade `efetivo_saida == marco` é um resultado do recorte da janela. O hunk `@@ -1842,28 +1896,34` troca o retorno `(marco, fora)` por `(saida, 0.0, marco)` quando há bloqueio e o turno cruza, preservando o marco para a base da HE. `app/ponto/motor_calculo_v2.py:1863,2059–2062,2101–2118,2166,2206,2231–2232,2330` no main → `app/ponto/motor_calculo_v2.py:1908–1920,2154–2157,2196–2213,2265–2267,2307,2331–2333,2431` na obra.

### 2. Colab 218, em 15/11

**Sim. `420 − 102,87 = 317,13 min`**, exatamente o atraso exibido. Com o marco informado de `21:00–05:00`, o bruto reconstruído é **377,18 min**, de **14/11 às 21:00 (D−1)** até **15/11 às 03:17:11 (D)**. O marco previsto pode existir em 14/11 mesmo sem um par batido naquela noite. O resultado é compatível com a L-093 reduzindo o bruto a `317,13`. O valor imediatamente antes do rateio, após eventual abono parcial, está **[nao verificado]**. `dias.txt:3–5`; `app/ponto/motor_calculo_v2.py:1225–1247,1276–1286,2087,2206` no main → `app/ponto/motor_calculo_v2.py:1268–1290,1319–1329,2182,2265–2267,2307` na obra.

### 3. Colab 841

O rateio fica em `_aplicar_teto_pontualidade`. Se `soma > teto`, ele multiplica cada atraso e cada antecipada do dia por `teto / soma` e arredonda cada campo a duas casas. Os valores exibidos somam **38,25 min** nas duas versões (`38,25 + 0` e `5,42 + 32,83`). A mudança de data do marco permite que o segundo par entre na soma com atraso positivo e passe a receber parte do teto. A cauda do segundo par já entrava no cálculo do teto no main como `fora` e entra na obra como `trabalhado`. Para esse par, os valores publicados são `6,47 + 5,90` e `12,38`, respectivamente. O rateio observado é compatível com os dois pares no mesmo grupo diário. O mapa do grupo e os valores anteriores ao rateio estão **[nao verificado]**. `dias.txt:10`; `app/ponto/motor_calculo_v2.py:1125–1128,1263–1286,2166,2206,2831` no main → `app/ponto/motor_calculo_v2.py:1168–1171,1306–1329,2265–2267,2307,2944` na obra.

### 4. Dia da jornada pela L-085

A diferença mostrada pelo diff é **qual dia a pontualidade consulta**. O main já usava o mapa do juiz na janela e no agrupamento mensal. O agrupamento usa a entrada bruta e pode herdar o dia do par anterior quando o intervalo entre eles é de até seis horas, medido pela **saída bruta**. No 841, os dois pares estão separados por `1h51min14s`, o que satisfaz essa condição se forem consecutivos no laço. Uma mudança de `dia_da_jornada` entre as versões para qualquer par do anexo está **[nao verificado]**. `dias.txt:10`; `app/ponto/motor_calculo_v2.py:972–973,1646,2510–2521,2536,2573` no main → `app/ponto/motor_calculo_v2.py:1015–1016,1692,2611–2622,2637,2674` na obra.

## 27/11 06:25 · sessao 0496a3491b21 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

## Volta 2

1. **`P_CRUZA_PARTIDO_SEM_DECISAO` — fechado.** O P2 bate saída às 07:30, conta até 07:00 e dá **240 min trabalhados, 30 fora, 240 noturnos de relógio, HE50 = 0 e HE100 = 0**. Com P1, são `300 + 240 = 540` min para uma jornada de 540. O controle `Autorizado` mantém 07:30, 270 trabalhados e 30 de HE50. O recibo do hash `c57f0123` registra **17/17 OK** no módulo. `app/ponto/tests/test_ramo2b_turno_que_cruza.py:141–154,203–241,279–283` · `app/ponto/motor_calculo_v2.py:1900–1920,3106,3131–3146` · `tqc.log:2–17`

2. **Split da HE noturna — divergência confirmada na cópia, origem em relação ao main pendente.** No Partido, o recorte devolve `s_he == s`, então a tripla fica `None`. O motor usa `periodo.saida` **bruta** nesse caso. O oráculo passa `efetivo_saida` à mesma função de split. Em memória, com saída batida às 05:30, efetiva às 04:00, prorrogação desligada e HE50 de `180/7` min fornecida ao período, obtive **0 min de HE50 noturna no motor e 25,7142857 min pela ponta que o oráculo usa**. O teste `PARTIDO_LEGAIS` exige HE zero, e os dois `P_CRUZA_PARTIDO_*` conferem HE50/100 sem conferir seus subconjuntos noturnos. `app/ponto/motor_calculo_v2.py:464–485,1094–1109,1900–1920,1967–1971,3106` · `app/ponto/calculador/alimentacao.py:88–115` · `app/ponto/calculador/regras.py:845–865` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:392–403` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:227–230`

   A leitura de `periodo.saida` em `59600163c:app/ponto/motor_calculo_v2.py:1059` é **[nao verificado]**. A cópia veio sem metadados Git e o diff solicitado falhou **[nao verificado]**. Se o main já passa a saída bruta com os mesmos insumos, o defeito do **motor** é preexistente e cabe na fila. A antiguidade da divergência no **oráculo** exige conferir também a ponta que ele passava no main. A linha `PARTIDO-HE-NOTURNA-SAIDA-BRUTA` fica **[nao verificado]**. O registro visível `PAUSA-INTERNA-NO-PARTIDO` trata da pausa dentro do período. `app/ponto/motor_calculo_v2.py:1094–1109` · `app/ponto/calculador/alimentacao.py:106–115` · `app/docs/BACKLOG.md:2159–2161` · `CLAUDE.md:425–426`

3. **Equivalência com o main — fechada para as asserções dos dois controles, pendente para o restante.** O recibo registra uma foto de trabalho `d72222b3` sobre `59600163c`, com dois testes passando e dois `ImportError`. Os dois `P_CRUZA_PARTIDO_*` dão evidência pelo `calcular_mes` para o par-alvo, o dia atribuído pelo juiz, a saída efetiva, os minutos arredondados, a acusação de cadastro e a conservação do bruto. Os dois testes c2p param no import antes de rodar `SEMANA_FLOAT` ou `PARTIDO_LEGAIS`. Portanto, o recibo comprova a ausência dos dois helpers naquela foto. Os valores dessas duas fixtures no main, o split, a equivalência bit a bit e as saídas dos demais leitores ficam **[nao verificado]**. `main.log:2–30` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:203–241` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:160–176,222–236,378–403`

## Alcance do motor e dos leitores

O anexo `delta.patch` traz os dois arquivos de teste. A lista **exata** de hunks de `59600163c..HEAD` fica **[nao verificado]**. Estes são os trechos da cópia que consegui rastrear até o Partido e o Partido Noturno, que herda o mesmo cálculo. `delta.patch:1` · `app/ponto/motor_calculo_v2.py:3103–3147,3830–3837`

| Trecho alcançado | Efeito verificável na cópia |
|---|---|
| `PeriodoCalculo`, helpers e porta da tripla | O construtor passa pela porta. Com a tripla vazia, os helpers leem o trabalhado e `noturno legal − noturno de relógio`. `app/ponto/motor_calculo_v2.py:235–299,1941–1971` |
| Flag, janela e `MotorTurnoPartido.calcular_periodo` | O `False` do Partido mantém `s_he == s`. Com recorte, ambos ficam no marco e a sobra vai para `minutos_fora_janela`. Com autorização, ambos ficam na batida. `app/ponto/motor_calculo_v2.py:792–795,1898–1939,3103–3146` |
| `MotorBase.calcular_mes` herdado | O bloqueio de folga/feriado sem previsto zera a tripla com as rubricas. O crédito de turno aberto acumula trabalhado, fora e base. Para os pares produzidos pelo Partido, o crédito lê a base pelo trabalhado e mantém a tripla vazia. `app/ponto/motor_calculo_v2.py:1628–1643,2671–2675,2763–2786,3155–3158` |
| `MotorTurnoPartido.calcular_mes` | A soma diária que alimenta também `semanal_44`, o piso de intra e a dobra de feriado leem o helper de trabalhado. A régua `legais` lê o helper de ganho. Com a tripla vazia, essas leituras devolvem os números do próprio par. O split compartilhado é chamado na dobra, na HE diária e na semanal. `app/ponto/motor_calculo_v2.py:286–299,3193–3195,3237–3252,3342–3354,3375–3440` |

O trecho da virada em `_julgar_pontualidade` é chamado pelo Comercial e pelo `Motor12x36ComEscala`. O Partido usa a própria Zona 5, com marcos do dia da jornada. `app/ponto/motor_calculo_v2.py:2260–2267,3257–3330,3566,3827`

- **`reler`.** É caminho de ensaio. Mede e reescreve o trabalhado pelo segmento efetivo e, com a tripla vazia, o helper lê esse mesmo valor para excesso e dobra. Também reescreve intra, HE e, quando recebe CCT, noturno. Um delta numérico contra o main por esse caminho fica **[nao verificado]**. `app/ponto/calculador/periodo.py:547–575,753–785,843–875`
- **`porta_export`.** O teto agrupa pelo mapa estrito de dia da jornada e usa o helper. Para o Partido, a base cai no trabalhado. A conferência só entra com **HE e intra positivas** e usa base de relógio e previsto líquido. A régua `legais` do motor tem ainda ganho noturno e limite de 480. Mudança no resultado da porta contra o main ou no TXT fica **[nao verificado]**. O split noturno, isoladamente, não altera as linhas de HE emitidas pelo TXT, que usa os totais e o adicional noturno. `app/folha/porta_export.py:342–366,588–595` · `app/ponto/motor_calculo_v2.py:3382–3409` · `app/folha/export.py:420–448`
- **Oráculo do fechamento.** Para o Partido com tripla vazia, a base segue os segmentos efetivos sem marcar `dias_com_cauda`. A divergência demonstrada na cópia está na **ponta do split**. O fechamento grava o split do motor e também lavra a versão `oraculo` com rubricas próprias. O valor dessa rubrica no main e o impacto em produção ficam **[nao verificado]**. `app/ponto/calculador/alimentacao.py:106–115,166–178,469–474,545–548` · `app/ponto/services/fechamento.py:938–944,1289–1318`

## Quem escreve e quem lê

| Campo conferido | Escrita e leitura |
|---|---|
| `entrada`, `saida`, `efetivo_entrada`, `efetivo_saida` | `calcular_periodo` inicializa as batidas cruas. `_carimbar_segmento` grava as efetivas e o bloqueio integral pode colapsar a saída efetiva. O split do motor lê as cruas no fallback. `reler` e o oráculo leem o segmento efetivo. `app/ponto/motor_calculo_v2.py:1094–1105,1642–1643,2042–2050,3122–3140` · `app/ponto/calculador/periodo.py:753–761` · `app/ponto/calculador/alimentacao.py:88–115` |
| `minutos_trabalhados`, `minutos_fora_janela`, `intra_descontada` | Os calculadores por par gravam trabalhado e fora. O Partido deixa o desconto interno no default. Bloqueio integral, crédito de aberto e `reler` têm reescritas próprias. O teto de pontualidade lê trabalhado + fora, `DiaPago` soma trabalhado, e o oráculo lê fora e o desconto para montar seus segmentos. `app/ponto/motor_calculo_v2.py:264,1319–1321,1628–1636,2777–2778,3015–3028,3139–3146,3501–3517` · `app/ponto/calculador/periodo.py:182–187,743–769` · `app/ponto/services/dia_pago.py:173–186` · `app/ponto/calculador/alimentacao.py:98–99,133–147` |
| `minutos_base_da_he`, `saida_da_base_da_he`, `ganho_noturno_da_base_da_he` | `_escrever_base_da_he` grava os três juntos. Motor, `reler` e porta leem a base pelo helper. O mensal `legais` lê o ganho pelo outro helper. Split, `reler` e oráculo leem a ponta. `app/ponto/motor_calculo_v2.py:286–299,1100–1105,1942–1971,3193–3195,3386–3390` · `app/ponto/calculador/periodo.py:758–767` · `app/folha/porta_export.py:342–354` · `app/ponto/calculador/alimentacao.py:114–115,168–178` |
| Noturno de relógio/legal, HE total/50/100, intra indenizada | `_escrever_noturnas` grava o par de noturnas. O mensal do Partido grava HE e o suprimido do vão. O bloqueio integral zera esses campos e `reler` os reescreve no ensaio. Ganho, split e limite leem os campos correspondentes. `DiaPago` soma as rubricas e a porta usa HE total e intra no teto. `app/ponto/motor_calculo_v2.py:293–299,1085–1109,1632–1636,1973–2011,3244–3252,3347–3354,3402–3409,3435–3440` · `app/ponto/calculador/periodo.py:769–785,866–875` · `app/ponto/services/dia_pago.py:178–186` · `app/folha/porta_export.py:350–361` |
| Dia do turno e acusação de cadastro | O mensal guarda o mapa do juiz e expõe o dia por período. `_acusa_cadastro_x_realidade` acrescenta a acusação de janela. O teste lê os dois, o oráculo e a porta usam o mapa por período e o espelho consome o relato. `app/ponto/motor_calculo_v2.py:1812–1829,2609–2617,2945,3190–3209` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:224–233` · `app/ponto/calculador/alimentacao.py:78–88` · `app/folha/porta_export.py:342–348` · `app/ponto/services/espelho.py:748–755` |

## Checklist

- **2º juiz:** o delta apresentado altera testes. Os casos mensais passam pela escolha de motor e pelo juiz de turno. O novo caso direto injeta a janela pronta, então a montagem real da janela fica coberta apenas pelos casos que passam pelo mensal. `delta.patch:1` · `app/core/juizes.py:222–224` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:203–226` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:437–458`
- **2º escritor:** executei a varredura AST do próprio selo em leitura. Para a tripla, ela encontrou apenas `_escrever_base_da_he`. Para as demais rubricas, `reler` é um escritor de ensaio e o motor tem os ajustes indicados acima. A varredura declara suas limitações para atributos dinâmicos. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:644–677` · `app/ponto/motor_calculo_v2.py:1954–1958` · `app/ponto/calculador/periodo.py:547–552,743–785`
- **Relógio solto:** os casos mensais congelam o tempo. O teste direto usa instantes fixos, feriados vazios e janela injetada. `app/ponto/tests/test_ramo2b_turno_que_cruza.py:168` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:124,443–458`
- **Derivação paralela:** `reler` mede o trabalhado pelo carimbo e o oráculo mede os segmentos. O split tem uma função compartilhada, com **pontas diferentes** no caso demonstrado. `app/ponto/calculador/periodo.py:753–769` · `app/ponto/calculador/alimentacao.py:106–115,469–474` · `app/ponto/calculador/regras.py:845–865`
- **Selo com mordida:** o novo teste direto distingue `42761/60` de `44960/60 − 2199/60`, que diferem em ponto flutuante. Os selos do Partido cobram o recorte, mas deixam descoberto o split com HE positiva e cauda excluída. A suíte integral desta volta fica **[nao verificado]**. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:222–236,392–403,443–463` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:227–230` · `tqc.log:3–17`
- **Campo com dois significados:** `minutos_trabalhados` é o contado, e a presença para o teto soma trabalhado e fora. A tripla `None` significa que a base numérica coincide com o trabalhado. O comentário “par sem cauda” também é usado para um Partido com cauda física já recortada, e o fallback do split trata esse caso como se pudesse usar a batida bruta. `app/ponto/motor_calculo_v2.py:228–246,1096–1105,1319–1321,1917–1920,1963–1969`

## Veredito — REPROVADO

1. **Item 1 fechado.** A asserção corrigida bate com a conta e o módulo registra 17/17 OK. `app/ponto/tests/test_ramo2b_turno_que_cruza.py:141–154` · `tqc.log:3–17`
2. **Item 2 pendente.** A diferença de split entre motor e oráculo está demonstrada na cópia. Para baixar o achado como fila preexistente, falta conferir as duas pontas no main e localizar o registro alegado **[nao verificado]**. `app/ponto/motor_calculo_v2.py:1094–1109` · `app/ponto/calculador/alimentacao.py:106–115` · `CLAUDE.md:425–426`
3. **Item 3 parcialmente fechado.** Os dois controles mensais sustentam equivalência para o que conferem. Os dois imports interrompem as outras comparações, e a lista exata de hunks e a equivalência dos demais caminhos seguem **[nao verificado]**. `main.log:2–30` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:217–241` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:222–236,387–403`

## 27/11 06:17 · sessao 25a38f481b30 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

1. **Sem decisão.** Pela RAMO2-PARTIDO-2611-1812 (a) e pela L-097(b), o Partido 19:00–07:00 tem vão de 00:00 a 03:00 e o segundo par termina efetivamente no marco de 07:00. São **240 min trabalhados**, **30 min fora da janela** e **240 min noturnos** de 03:00 a 07:00 com a prorrogação da fixture. A HE do dia é `300 + 240 − 540 = 0`. A conta coincide com a saída registrada no anexo `tqc.log`. `docs/LEIS.md:115`, `ponto/motor_calculo_v2.py:3106`, [ponto/tests/test_ramo2b_turno_que_cruza.py:141](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_turno_que_cruza.py:141)

2. **Autorizado.** A conta concorda com o teste. A janela mantém a saída às 07:30, com 270 min trabalhados, 270 noturnos e zero fora da janela. `300 + 270 − 540 = 30` min de HE50 no último par, com HE100 zero. `ponto/motor_calculo_v2.py:1906`, `ponto/motor_calculo_v2.py:3402`, [ponto/tests/test_ramo2b_turno_que_cruza.py:154](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_turno_que_cruza.py:154)

3. **`acusa=False`.** `_TPART` fixa o marco em 07:00 na linha 60. O cadastro da empresa da fixture fixa `teto_saida` em 60 min na linha 179. A regra acusa acima do teto. Como `30 < 60`, a saída fica abaixo dele. `ponto/tests/test_ramo2b_turno_que_cruza.py:60`, `ponto/tests/test_ramo2b_turno_que_cruza.py:179`, `ponto/janela_he.py:200`

### Diff

```diff
--- a/app/ponto/tests/test_ramo2b_turno_que_cruza.py
+++ b/app/ponto/tests/test_ramo2b_turno_que_cruza.py
@@ -141,8 +141,8 @@
-    # TURNO PARTIDO que cruza (19-07, vao 00-03 > 2 h = partido de verdade): par 03:00 -> 07:30 de D+1, 30 min depois
-    # do marco 07:00. Conta 270; noturno com prorrogacao (jornada comum, Sumula 60 II): 03:00 -> 07:30 = 270. HE do
-    # DIA pela base recortada: 300 + 240 = 540 contra 540 -> 0.
+    # RAMO2-PARTIDO-2611-1812 (a) e L-097(b): no Partido 19-07 (vao 00-03), sem decisao, o par 03:00 -> 07:30
+    # de D+1 recorta no marco 07:00: 240 trabalhados, 30 fora da janela e 240 noturnos (03:00 -> 07:00 com
+    # prorrogacao). HE do dia: 300 + 240 = 540 contra a jornada de 540 -> 0.
     'P_CRUZA_PARTIDO_SEM_DECISAO': (
         _TPART, _cels(_TPART, _D),
         [(_D, '19:00:00', 'E'), (_D1, '00:00:00', 'S'), (_D1, '03:00:00', 'E'), (_D1, '07:30:00', 'S')], _D, None,
         '2026-11-06 03:00',
-        ('2026-11-06 03:00', '2026-11-06 07:30', 270.0, 0.0, 270.0, 0.0, 0.0), False),
+        ('2026-11-06 03:00', '2026-11-06 07:00', 240.0, 30.0, 240.0, 0.0, 0.0), False),
```

### RED e mutantes

Reproduzi em memória a tupla do anexo pelo `test_RED_partido_cruza_sem_decisao`, substituindo `_roda` pela observação registrada. A expectativa corrigida passou. A tupla antiga e os quatro mutantes individuais falharam na comparação da tupla. Suite Django após o diff e certificação do Code e do Opus final [nao verificado]. `ponto/tests/test_ramo2b_turno_que_cruza.py:230`, `ponto/tests/test_ramo2b_turno_que_cruza.py:279`

```bash
tqc_replay() {
  PYTHONDONTWRITEBYTECODE=1 python3 -S - "$1" <<'PY'
import ast
import sys
import unittest
from datetime import datetime
from pathlib import Path
from types import SimpleNamespace

nome = 'P_CRUZA_PARTIDO_SEM_DECISAO'
modo = sys.argv[1]
arquivo = Path('ponto/tests/test_ramo2b_turno_que_cruza.py')
arvore = ast.parse(arquivo.read_text())
arvore.body = [n for n in arvore.body if not isinstance(n, ast.ImportFrom)]
arvore.body[-1].decorator_list = []
escopo = dict(TestCase=unittest.TestCase, timezone=SimpleNamespace(localtime=lambda dt: dt))
exec(compile(arvore, str(arquivo), 'exec'), escopo)

casos = escopo['CASOS']
caso = casos[nome]
esperado = list(caso[-2])
antigos = {1: '2026-11-06 07:30', 2: 270.0, 3: 0.0, 4: 270.0}
for i in antigos if modo == 'red' else [] if modo == 'controle' else [int(modo)]:
    esperado[i] = antigos[i]
casos[nome] = (*caso[:-2], tuple(esperado), caso[-1])

e, s, batida = map(datetime.fromisoformat,
                   ('2026-11-06 03:00', '2026-11-06 07:00', '2026-11-06 07:30'))
par = SimpleNamespace(entrada=e, saida=batida, efetivo_entrada=e, efetivo_saida=s,
                      minutos_trabalhados=240, minutos_fora_janela=30, minutos_noturnos_relogio=240,
                      minutos_extra_50=0, minutos_extra_100=0)
motor = SimpleNamespace(_dia_turno_juiz_calc={e: '2026-11-05'}, _cadastro_x_realidade=[])
teste = escopo['Ramo2bTurnoQueCruzaTest']('test_RED_partido_cruza_sem_decisao') [nao verificado]
teste._roda = lambda nome: (motor, None, [par]) [nao verificado]
resultado = teste.run() [nao verificado]
assert not resultado.errors, resultado.errors
assert resultado.wasSuccessful() == (modo == 'controle'), resultado.failures
for _, falha in resultado.failures:
    assert 'Tuples differ' in falha and nome in falha, falha
print(modo, 'PASSOU' if resultado.wasSuccessful() else 'REJEITADO na comparação da tupla') [nao verificado]
PY
}

tqc_replay controle  # expectativa corrigida
tqc_replay red       # tupla antiga
tqc_replay 1         # saída efetiva 07:30
tqc_replay 2         # trabalhado 270
tqc_replay 3         # fora da janela 0
tqc_replay 4         # noturno 270
```

## 27/11 06:11 · sessao 4f8a76333721 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

## Aval e main

**A leitura da c2p é defensável, mas o texto permite outra leitura.** A opção (a) responde a uma parada sobre **pausa interna**. A expressão “motor do Partido intocado”, junto do recorte de pouso para 12x36 e Comercial, sustenta preservar também o tratamento anterior da cauda. A leitura mais estreita adia apenas a grade de pausa interna e mantém a emenda geral da L-097(b), que diz que “nenhum par perde trabalhado”. A opção (a) delimita o pouso, mas deixa implícita a consequência para `minutos_fora_janela` no Partido. [`PROMPTS.md:3087`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/docs/PROMPTS.md:3087) · [`LEIS.md:115`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/docs/LEIS.md:115)

**Na cópia, o Partido põe a cauda não autorizada em `minutos_fora_janela` quando a janela se aplica.** O `False` da classe leva ao retorno `(marco, fora, marco)`, e o calculador grava `fora`. `Autorizado` passa antes desse ramo e mantém a saída batida. O comentário e dois controles descrevem o recorte como comportamento do main. Para **`59600163c` especificamente**, a cópia veio sem histórico Git e o anexo traz apenas o delta de testes. A confirmação direta desse commit fica **[nao verificado]**. `motor_calculo_v2.py:1900–1920,3106,3131–3146` · `test_ramo2b_c2h_saida.py:147–166` · `test_ramo2b_c2l_red_motor.py:51–61`

## 1. Contas

| Caso | Conta refeita |
|---|---|
| **Partido `SEMANA_FLOAT`** | Segunda, 20:07:19 → 05:00:00 = `31961/60 = 532,683333… min`. Até a batida de 05:36:39 são `34160/60 = 569,333333… min`. Cauda = `2199/60 = 36,65 min`. Na cópia, **trabalhado = base = 532,683333…**, com **36,65 fora**. As bases de segunda a sexta são `31961`, `32277`, `31873`, `32374` e `32915` segundos. Somam `2690 min`, exatamente `2640 + 5 × 10`. **HE semanal zero**, sem alerta. `test_ramo2b_c2_base_da_he.py:79–97,213–236` · `motor_calculo_v2.py:3193–3195,3410–3439` |
| **Partido 6x1 `legais`** | P1 = `221 min`. P2 até 04:00 = `180 min`, todos noturnos. De 04:00 a 05:00 ficam `60 min` fora. Base de relógio = `401`. Hora reduzida = `180 × 8/7 = 1440/7`, com ganho de `180/7`. Previsto = `600 − 180 = 420`. Vão gozado = `180`, suprimido = `0`, limite = `min(420, 480) + 0 = 420`. Excedente = `47/7 = 6,714285… min`, dentro dos 10. **HE zero**. Incluir apenas o ganho noturno da cauda elevaria o excedente a `107/7 = 15,285714… min`, integralmente em HE50. A exceção da cl. 38-d é do 12x36. `test_ramo2b_c2_base_da_he.py:64–65,100–104,378–403` · `motor_calculo_v2.py:44–51,1993–2011,3237–3252,3375–3404` · `LEIS.md:84` · `regua_cct.py:467–472` |
| **12x36 e Comercial** | 19:07:19 → 07:36:39 = `44960/60 = 749,3333333333334 min` trabalhados. Até 07:00 são `42761/60 = 712,6833333333333 min` de base. Os `36,65 min` da cauda entram no trabalhado e **fora = 0**. Subtrair em ponto flutuante dá `712,6833333333334`, um ULP acima da medição direta. O teste acerta ao exigir igualdade exata. `test_ramo2b_c2_base_da_he.py:437–463` · `motor_calculo_v2.py:1918–1920,2985–3028,3473–3517` |

## 2. Caminho do novo teste [nao verificado]

O teste chama os `calcular_periodo` reais e percorre o recorte, a medição e a gravação da base. O patch entrega pronta a parte que o mensal precisa resolver: pareamento e intervalos, dia do turno, marcos do vínculo, folga, vigência, L-084, extra declarada e decisão do admin. Um marco de D+1 errado na janela real ficaria invisível nesse teste. `test_ramo2b_c2_base_da_he.py:443–463` · `fechamento.py:297–311` · `motor_calculo_v2.py:1682–1722,1742–1810,2500–2511,2560–2568,2661–2675` · `janela_he.py:41–61` [nao verificado]

Há duas diferenças relevantes. O 12x36 com horário cadastrado usa `Motor12x36ComEscala`, que chama o mesmo cálculo e depois relê a entrada efetiva para pontualidade. O Comercial mensal zera a HE feita por par e a apura de novo por dia, usando o previsto do dia da jornada quando cruza. O teste também injeta `660` sem fornecer o intervalo descontado do previsto. Ele prova os três números da asserção. A mesma igualdade bit a bit pelo mensal fica **[nao verificado]**. Os dois testes reescritos do Partido já chamam `calcular_mes`. `motor_calculo_v2.py:1464–1492,3580–3602,3672–3685,3810–3827,3927–3928` · `test_ramo2b_c2_base_da_he.py:160–176,455–463` [nao verificado]

## 3. Escritores e leitores

| Sítio | O que faz |
|---|---|
| **Motor** | Os três `calcular_periodo` gravam trabalhado e fora. A tripla da base é gravada por `_escrever_base_da_he`. No Partido recortado, a saída efetiva já é o marco, a tripla fica vazia e `_trabalhado_da_base_da_he` devolve o trabalhado. No 12x36 e Comercial com cauda incorporada, o leitor devolve a base explícita. O Comercial mensal usa a base para HE e o trabalhado integral para banco. O crédito do **dia aberto** e o bloqueio de HE100 também ajustam os campos pela regra do motor. `motor_calculo_v2.py:286–299,1628–1635,1941–1971,2768–2786,3025–3028,3141–3146,3193–3195,3382–3393,3514–3519,3591–3594` |
| **`dia_pago.py`** | Soma trabalhado, noturno e HE já calculados, pelo dia do mapa quando fornecido. A cauda excluída do trabalhado do Partido fica fora dessa soma. `dia_pago.py:126–132,173–186` |
| **`periodo.py::reler`** | Mede e **reescreve** trabalhado pela geometria efetiva. Lê a tripla para HE e dobra, e soma trabalhado com fora para o teto de pontualidade. No caminho de par duplicado, zera também `minutos_fora_janela`. A geometria efetiva do Partido já chega cortada. `periodo.py:182–187,344–366,743–769,807–813,843–859` |
| **`folha/porta_export.py`** | Compara o trabalhado com a lavratura e lê a base pela mesma função do motor. A pendência vem do espelho. O teto de conferência quando há HE **e** intra usa base de relógio e previsto líquido. Ele não reproduz o ganho noturno nem `min(previsto, 480)` do Partido `legais`. `porta_export.py:106–122,181–201,342–365` · `motor_calculo_v2.py:3382–3404` |
| **`credito_parcial.py`** | Fornece ausências parciais e créditos. Dos períodos, lê atraso e agrupa pela data civil da entrada, que pode divergir do dia da jornada. O teto do motor usa `trabalhado + fora`, então a cauda do Partido continua contando como presença para a L-093. `credito_parcial.py:35–50,62–102` · `motor_calculo_v2.py:1319–1321,1356–1357` |

**Há um leitor que usa a ponta errada no Partido.** Quando a tripla está vazia, `_aplicar_he_noturna` usa a saída **bruta**. Rodei essa função em memória com 01:00 → 05:30, saída efetiva às 04:00, HE50 de `180/7` e prorrogação desligada. Com a tripla vazia, saiu **0 min de HE50 noturna**. Com o fim da base às 04:00, saiu **25,7142857 min**. O teste `PARTIDO_LEGAIS` espera HE zero, então não alcança essa divergência. Impacto em produção **[nao verificado]**. `motor_calculo_v2.py:464–485,1094–1109,1967–1971,3131–3146` · `test_ramo2b_c2_base_da_he.py:396–399`

## 4. Teste que contradiz [nao verificado]

**`test_RED_partido_cruza_sem_decisao` ainda exige a cauda dentro.** Para P2 de 03:00 a 07:30, com marco às 07:00, exige saída efetiva **07:30**, **270 trabalhados**, **0 fora** e **270 noturnos**. Pelo ramo da cópia, a expectativa é **07:00**, **240 trabalhados**, **30 fora** e **240 noturnos** com a prorrogação dessa fixture. O vizinho `Autorizado` continua coerente ao exigir os 270. O caso dos 11 minutos também é controle distinto, pois o juiz põe o par em D+1, fora da janela do turno anterior. A execução da suíte fica **[nao verificado]**. [`test_ramo2b_turno_que_cruza.py:141`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/ponto/tests/test_ramo2b_turno_que_cruza.py:141) · `test_ramo2b_turno_que_cruza.py:227–230,279–283` · `test_ramo2b_c2_base_da_he.py:425–434` · `motor_calculo_v2.py:1900–1920,3106,3141–3146`

## 5. Checklist

- **Segundo juiz e derivação paralela.** O delta só muda testes. Na cópia, `reler` rederiva o trabalhado e a versão oráculo do fechamento mede a base novamente pelos segmentos efetivos. O patch do novo teste fornece a resposta da janela. `test_ramo2b_c2_base_da_he.py:437–463` · `periodo.py:753–769` · `alimentacao.py:168–178,469–474,545–548` · `fechamento.py:1289–1305` [nao verificado]
- **Segundo escritor.** A varredura AST executada em leitura encontrou só `_escrever_base_da_he` para a tripla. Para trabalhado e fora há as reescritas do motor e de `reler` descritas acima. `test_ramo2b_c2_base_da_he.py:644–677` · `motor_calculo_v2.py:1954–1958` · `periodo.py:743–767`
- **Relógio solto.** Os testes mensais do Partido têm `freeze_time`. O teste direto usa instantes fixos, janela injetada e feriados vazios. `test_ramo2b_c2_base_da_he.py:124–125,437–458` [nao verificado]
- **Selo com mordida.** O anexo mostra **5/5 mutantes mortos**, com oito falhas de asserção. O resultado da suíte limpa e o efeito nas competências 11/12 e no TXT ficam **[nao verificado]**. `mut4.log:6,29,51,61,75,84,93,104` [nao verificado]
- **Campo com dois significados.** `minutos_trabalhados` é o contado após a janela. A presença para o teto soma trabalhado e fora. Uma tripla `None` significa que a base numérica coincide com o trabalhado, inclusive quando houve cauda física recortada. O comentário “par sem cauda” é impreciso nesse caso e a leitura da saída bruta no split noturno mostra a consequência. `motor_calculo_v2.py:228–246,286–290,1096–1105,1319–1321,1963–1969`

## Veredito — REPROVADO

1. O selo mensal legado do Partido sem decisão exige o oposto da c2p para a mesma regra de saída. `test_ramo2b_turno_que_cruza.py:144–148,279–280` · `test_ramo2b_c2_base_da_he.py:222–236`
2. O split de HE noturna do Partido pode usar a cauda excluída para posicionar HE devida antes do marco. A c2p cobre o ganho na base, mas o caso com HE positiva segue sem selo nessa volta. `motor_calculo_v2.py:1094–1109,1967–1971` · `test_ramo2b_c2_base_da_he.py:378–403`
3. A c2p apresenta o destino da cauda como conclusão direta do aval. O texto sustenta preservar o Partido, mas a equivalência com o comportamento de `59600163c` permanece **[nao verificado]**. `PROMPTS.md:3087` · `LEIS.md:115` · `test_ramo2b_c2_base_da_he.py:4–6`

_8592 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
