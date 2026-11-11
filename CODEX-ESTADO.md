# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 07:09** · publicado em **11/11/2026 07:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 07:09 · sessao 9e7364a57f24 · - · - · origem=Ronald (tmux)

**4. NÃO PROCEDE como fechamento geral dos chamadores.** Há caminhos que continuam executando suite inteira sem motivo, e a documentação canônica continua ensinando uma chamada agora recusada (`bin/regua.sh:165`, `bin/pre-push.sh:186`, `CLAUDE.md:96`, `bin/suite.sh:124`).

- `volta_intermediaria.sh` chama `suite.sh --volta` sem motivo; recorte parcial válido passa, seleção cobrindo todas as LABELS é recusada. O wrapper transforma essa recusa rc 2 em rc 1 (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:37`, `bin/volta_intermediaria.sh:43`; `bin/suite.sh:119`, `bin/suite.sh:124`).
- `placar_estrutural.py` contém texto de comandos em `fonte`, com labels válidas de `ponto`; esses recortes passam na guarda atual. O arquivo não executa esses comandos nesse sítio (`app/core/placar_estrutural.py:109`, `bin/regua.sh:23`, `bin/suite.sh:89`, `bin/suite.sh:119`).
- `regua.sh` e `bin/pre-push.sh` chamam a trava diretamente; a suite inteira continua sem `--motivo`, fora da guarda de `suite.sh`. A trava aceita comando arbitrário depois de `--` (`bin/regua.sh:165`, `bin/regua.sh:168`; `bin/pre-push.sh:142`, `bin/pre-push.sh:186`; `bin/trava_teste.sh:29`, `bin/trava_teste.sh:42`).
- A cópia antiga de hook também roda arsenal inteiro diretamente por Docker e testa `tenants` com settings próprio; nenhuma guarda de motivo nesse caminho (`app/core/hooks/pre-push.sh:54`, `app/core/hooks/pre-push.sh:82`, `app/core/hooks/pre-push.sh:87`).
- O comando canônico `bash ~/saas-hasner/bin/suite.sh` agora recebe rc 2 por seleção vazia, embora a documentação permaneça sem motivo (`CLAUDE.md:96`, `bin/suite.sh:118`, `bin/suite.sh:124`, `bin/suite.sh:131`).
- Lançadores e execuções reais em `logs/`: **[não verificado]**. Os comandos históricos citados no RELATO que não levam motivo seriam recusados pela guarda atual, caso repetidos (`app/docs/RELATO.md:681`, `app/docs/RELATO.md:1137`; `bin/suite.sh:124`).

## 11/11 07:08 · sessao 903ad71eab22 · FIO-E0-V4-E3 · construtor · origem=Code

Cura escrita: o booleano do juiz alimenta o marcador e a renomeação; trabalho sem cobertura integral volta a `nunca_bateu`, independentemente do veredito derivado. A chamada a `com_insumo_dia_sem_trabalho` continua como primeiro argumento de `classificar_dia`. [cartorio.py:1220](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1220), [cartorio.py:1234](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1234), [cartorio.py:1253](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1253).

Trechos funcionais do diff:

```diff
+ sem_trabalho = _dia_sem_trabalho_pelo_juiz(_insumo_nb)
- sem_adesao=not _coberto_inteiro_nb and _dia_sem_trabalho_pelo_juiz(_insumo_nb)
+ sem_adesao=not _coberto_inteiro_nb and sem_trabalho

- if not _coberto_inteiro_nb and _vd_nb in cel.VEREDITOS_ACUSAM:
+ if not _coberto_inteiro_nb and not sem_trabalho:
      _vd_nb = 'nunca_bateu'
```

As duas linhas sintéticas agora recebem `data_civil=None`; o registro da ocorrência mudou somente de 1321 para 1322. [tarefa_do_dia.py:316](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/services/tarefa_do_dia.py:316), [tarefa_do_dia.py:331](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/services/tarefa_do_dia.py:331), [test_medida_turno_do_par.py:315](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/core/tests/test_medida_turno_do_par.py:315).

Resultados locais, todos com `PYTHONDONTWRITEBYTECODE=1` e `python3 -B`:

| Execução | Resultado |
|---|---|
| `-m unittest ponto.tests.test_e3_nb_impressao_memoria -v` | rc **0**, 18 testes | [nao verificado]
| `-m unittest chamados.tests.test_e3_pos_suite_sem_banco -v` | rc **0**, 7 testes | [nao verificado]
| `-m chamados.tests.e3_pos_suite_sem_banco --modo green` | rc **0**, incluindo os 35 testes de medida | [nao verificado]
| Catraca F1C4.3 extraída por AST | rc **0**, 3 testes | [nao verificado]

Fontes: [test_e3_nb_impressao_memoria.py:89](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:89), [test_e3_pos_suite_sem_banco.py:19](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:19), [e3_pos_suite_sem_banco.py:20](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_pos_suite_sem_banco.py:20), [catraca F1C4.3:215](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:215).

Contra `d6a86f314941e40271974d5b9e0c6bdd9d1149e1`, os três REDs de trabalho deram rc **1**, com três assertions e zero erros: justificado, turno em curso e parcial sem resto. [test_e3_nb_impressao_memoria.py:275](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:275).

```bash
export PYTHONDONTWRITEBYTECODE=1
E3_NB_BASE=d6a86f314941e40271974d5b9e0c6bdd9d1149e1 \
python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria -v
```

Mutantes X1: **rc 1 por assertion, ERROR=0 em cada um**. Acrescentei `--sem-banco`; `so_acusador` morreu no teste de trabalho justificado. [x1_mutantes.py:23](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:23), [x1_mutantes.py:44](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:44). [nao verificado]

```bash
python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante literal_incondicional
python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_fonte_no_gancho
python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante so_acusador
```

Mutantes pós-suite: **rc 1 por assertion, ERROR=0 em cada nome abaixo**. Os dois alvos do hash agora encontram a expressão viva; o RED preserva o tipo do dia na ata para morder cobertura integral. [e3_pos_suite_sem_banco.py:30](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/e3_pos_suite_sem_banco.py:30), [test_e3_pos_suite_sem_banco.py:144](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/chamados/tests/test_e3_pos_suite_sem_banco.py:144). [nao verificado]

```bash
for mutante in \
  C6_redundante hash_marca_todo_sem_adesao hash_marca_cobertura_integral \
  data_civil_pergunta_usa_turno data_civil_celula_usa_turno \
  C6_ignora_instante memo_sem_instante materializada_crua resposta_crua slot_futuro
do
  python3 -B -m chamados.tests.e3_pos_suite_sem_banco \
    --modo mutante --mutante "$mutante"
done
```

Também executei os oito mutantes do módulo de memória: **rc 1, assertion presente e nenhum ERROR em cada execução**. Este comando reproduz cada seletor registrado, mais a classificação antes da guarda. [test_e3_nb_impressao_memoria.py:25](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:25), [test_e3_nb_impressao_memoria.py:66](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:66). [nao verificado]

```bash
python3 -B - <<'PY'
import os
import subprocess
import sys
from ponto.tests.test_e3_nb_impressao_memoria import MUTANTES

casos = {nome: dados[2] for nome, dados in MUTANTES.items()}
casos['classificacao_antes_guarda'] = 'test_F3_pulada_nao_classifica_nem_carrega'
for nome, teste in casos.items(): [nao verificado]
    alvo = 'ponto.tests.test_e3_nb_impressao_memoria.ImpressaoNuncaBateuMemoriaTest.' + teste [nao verificado]
    r = subprocess.run([sys.executable, '-B', '-m', 'unittest', alvo, '-v'],
                       env=dict(os.environ, E3_NB_MUTANTE=nome))
    print(nome, 'rc=', r.returncode)
PY
```

Ruff: **rc 0, `All checks passed!`**, com as regras de [ruff.toml:5](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ruff.toml:5).

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache .
```

Os caminhos ORM de X1 continuam exigindo banco isolado; não os executei. [x1_mutantes.py:91](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:91). Suite integral, delta de produção e regeneração de TXT: **[nao verificado]**. [nao verificado]

## 11/11 06:57 · sessao 12eb01f6e5d4 · FIO-E0-V4-E3 · verificador · origem=Code

**1. Classes de reprovação**

| Classe | Veredito e evidência |
|---|---|
| Segundo juiz | **NÃO PROCEDE nos caminhos examinados.** A carga pergunta ao C6 registrado; grade, vínculo, slot e pareamento continuam delegados às autoridades existentes. `app/core/juizes.py:278`; `app/chamados/services/tarefa_do_dia.py:491`; `app/chamados/services/carga_pergunta_viva.py:119`, `:156`, `:162`, `:181`. |
| Segundo escritor fora da porta | **NÃO PROCEDE como escritor novo.** A projeção temporal devolve valores locais; a tarefa acrescenta `viva_c6` em memória. A lavra continua passando pelas portas existentes. Isso não garante estoque intocado, conforme pergunta 3. `app/chamados/juizes.py:1369`, `:1380`; `app/chamados/services/tarefa_do_dia.py:491`; `app/ponto/services/cartorio.py:1255`, `:1277`. |
| Relógio fora da borda | **PROCEDE como ocorrência residual em sítio produtivo mudado.** `classificar_falta` ainda lê `_tz.now()` quando chamado sem `agora`; há chamador avulso nessa condição. O caminho novo da tarefa fornece o instante explicitamente. `app/escala/servico_jornada.py:387`, `:566`; `app/chamados/services/tarefa_do_dia.py:491`. |
| Derivação paralela da fonte única | **NÃO PROCEDE nos caminhos examinados.** O carregador reutiliza grade, vínculo e corpo do pareador; suas batidas de apuração continuam entrando por `batidas_apuraveis`. A leitura crua das testemunhas preserva a distinção existente entre idempotência do plantio e prova viva. `app/chamados/services/carga_pergunta_viva.py:156`, `:162`, `:181`; `app/ponto/turnos.py:3017`; `app/chamados/juizes.py:806`, `:814`. |
| Selo/RED sem mordida | **PROCEDE pontualmente.** Dois mutantes auxiliares procuram uma expressão antiga, ausente do cartório atual; o executor levanta `ValueError` antes da assertion. Além disso, o teste de estoque aceita expressamente a pergunta carimbada após reconciliar. `app/chamados/tests/e3_pos_suite_sem_banco.py:34`, `:37`, `:106`; `app/ponto/services/cartorio.py:1222`; `app/ponto/tests/test_x1_folga_sem_adesao.py:263`. |
| Campo com dois significados | **PROCEDE.** `data_civil` normalmente vem do instante local do marco, ou é desconhecida; nas linhas sintéticas novas recebe diretamente o dia do turno. O marcador também não cobre toda a classe que a cura absolve, conforme pergunta 2. `app/chamados/services/tarefa_do_dia.py:103`, `:316`, `:331`; `app/ponto/services/cartorio.py:1222`, `:1252`. |

**Inclusivo/exclusivo: NÃO PROCEDE achado.** A projeção inclui fatos exatamente em `agora`; o recorte de batidas usa `[lo, hi)`; a busca de testemunha conserva os dois extremos inclusivos. São contratos distintos explicitados no código. `app/chamados/juizes.py:1372`, `:1374`, `:1376`, `:244`; `app/chamados/services/carga_pergunta_viva.py:178`.

**2. Equivalência entre juiz, marcador e “absolvida sem cobertura integral” — NÃO PROCEDE**

O marcador pergunta **“sem cobertura integral E dia sem trabalho”**; a cura preserva **qualquer veredito não acusador**. Essas condições não são equivalentes. `app/ponto/services/cartorio.py:1211`, `:1222`, `:1237`, `:1252`.

Contraexemplos de código, sem afirmar ocorrência em produção:

| Entrada concreta, sem adesão | Juiz / marcador | Resultado da cura |
|---|---|---|
| `trabalha=True`, `tipo_dia='trabalho'`, previsto 480, realizado zero, sem batidas, `justificado=True` | `False / False` | `concorde`: a justificativa remove o furo; lista vazia deriva concorde. `app/chamados/juizes.py:456`; `app/ponto/supra_juiz.py:371`; `app/escala/models.py:1627`. |
| Mesmo trabalho, sem justificativa, `turno_encerrado=False` | `False / False` | `concorde`: o furo exige turno encerrado. `app/chamados/juizes.py:456`; `app/ponto/supra_juiz.py:313`, `:316`; `app/escala/models.py:1627`. |
| `trabalha=True`, ausência parcial aprovada, todos os marcos dispensados, nenhum `missing` | `False / False` | `concorde`: `_parcial_sem_resto` impede o furo. `app/chamados/juizes.py:454`; `app/ponto/supra_juiz.py:311`, `:314`. |
| Parcial aprovada com resto `missing`, turno encerrado | `False / False` | Continua `nunca_bateu`: o resto acusa e recebe a renomeação. `app/ponto/supra_juiz.py:367`; `app/ponto/services/cartorio.py:1252`. |

**Há consequência funcional:** o turno em curso pode ser lavrado `concorde`, sem marcador. Quando encerra, a impressão desse ramo continua igual e a guarda pode pular o julgamento **antes de consultar o encerramento**. O ramo normal imprime `enc`; o ramo sem adesão não imprime. `app/ponto/services/cartorio.py:1220`, `:1223`, `:1227`, `:1236`, `:1360`, `:1362`.

Também deixa de existir a garantia de rejulgamento pela primeira batida distante para esses absolvidos sem marcador: o caminho normal pode encontrar impressão igual e veredito diferente de NB, portanto pular. `app/ponto/services/cartorio.py:1222`, `:1372`, `:1376`.

**3. Proibições do aval — PROCEDE violação possível pelo código**

- **Mudar retenção de TRABALHO: PROCEDE.** O hunk deixa de impor NB aos trabalhos justificados, em curso ou parcialmente cobertos sem resto. Em julgamento efetivo — célula nova, impressão alterada ou execução forçada — podem passar de NB para `concorde`; a retenção lê esse veredito. O próprio teste exige concorde para trabalho justificado. `app/ponto/services/cartorio.py:1227`, `:1252`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:270`; `app/folha/export.py:1241`, `:1262`.

- **Tocar pergunta/chamado: NÃO PROCEDE como escrita direta nova; PROCEDE como efeito indireto não impedido.** A célula absolvida pode entrar no reconciliador existente, que carimba a pergunta e pode fechar a disputa. O consumidor de eventos executa esse caminho. O teste novo aceita a alteração da pergunta. `app/chamados/reconciliador.py:595`, `:611`, `:641`, `:1365`; `app/ponto/tests/test_x1_folga_sem_adesao.py:263`.

- **Fallback novo: NÃO PROCEDE nos hunks examinados.** O adaptador exige o insumo e pergunta ao juiz existente; a carga guarda exceções para relançá-las no mesmo degrau, sem convertê-las em veredito substituto. `app/ponto/supra_juiz.py:70`; `app/chamados/services/carga_pergunta_viva.py:120`; `app/chamados/juizes.py:352`.

Quantidade de trabalhos alterados, existência dessas perguntas nos dias raspados e fechamento efetivo de chamados: **[nao verificado]**.

**4. Escritores, leitores e significados — PROCEDE divergência no marcador; NÃO PROCEDE novo escritor dos carimbos**

| Campo/valor | Quem escreve ou alimenta | Quem lê |
|---|---|---|
| `validada_em` | Validação e porta do carimbo; também existem operações de limpeza. `app/chamados/services/validacao.py:134`, `:199`; `app/chamados/models.py:3135`, `:3266`. | Projeção/C6; fora do diff, carimbo, propriedade `viva` e filtro SQL atual. `app/chamados/juizes.py:1369`, `:1384`; `app/chamados/models.py:3139`, `:3359`, `:3375`. |
| `respondida_em`, `resposta_colab` | Serviço de resposta, respostas automáticas e limpeza. `app/chamados/services/disputa_emissao.py:1467`, `:1468`, `:1593`, `:1652`, `:1674`. | Projeção, C6 e slot; fora do diff, filtro SQL e materializador. `app/chamados/juizes.py:347`, `:1369`, `:1401`; `app/chamados/models.py:3348`; `app/chamados/services/materializacao.py:358`, `:411`. |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, persistido pelas portas; limpeza pela porta de descarimbo. `app/chamados/models.py:3097`, `:3129`, `:3152`, `:3266`. | Projeção/C6; fora do diff, filtro SQL e leitores atuais da pergunta. `app/chamados/juizes.py:1370`, `:1396`, `:1460`; `app/chamados/models.py:3350`; `app/chamados/services/pergunta_no_app.py:58`. |
| Tupla `projecao_temporal_da_pergunta` | Apenas variáveis locais; não reescreve o modelo. `app/chamados/juizes.py:1369`, `:1380`. | Dois leitores produtivos: slot e C6. `app/chamados/juizes.py:347`, `:1384`. |
| `agora` | Argumento da tarefa, armazenado nas cargas. `app/chamados/services/tarefa_do_dia.py:367`, `:562`; `app/chamados/services/carga_pergunta_viva.py:88`. | Decisões, passivo, lavra, efeitos, fila, C6 e classificação de falta. `app/chamados/services/tarefa_do_dia.py:391`, `:399`, `:402`, `:410`, `:415`, `:491`; `app/escala/servico_jornada.py:387`. |
| `sem_adesao` → impressão | Cartório calcula o booleano; `impressao_insumos` acrescenta o fragmento; a porta grava a impressão. `app/ponto/services/cartorio.py:1222`, `:525`, `:1277`; `app/ponto/portas/celula.py:1066`. | Guardas de igualdade dos dois ramos e idempotência da porta. `app/ponto/services/cartorio.py:1223`, `:1372`; `app/ponto/portas/celula.py:1062`. |
| `_cels`, `_cand`, `_vinculos`, `_bats` | Carga comum da janela e carregador extraído. `app/ponto/turnos.py:2980`, `:2986`, `:2990`, `:3017`. | Grade e pareamento da carga C6. `app/chamados/services/carga_pergunta_viva.py:156`, `:174`, `:177`, `:181`. |
| `_perg`, `_dec`, `_passivo`, `_lavra`, `_efeitos`, `_fila` | Consultas e autoridades carregadas pela tarefa. `app/chamados/services/tarefa_do_dia.py:374`, `:389`, `:398`, `:403`, `:410`, `:415`. | Corpo puro e decisão da ação exigível. `app/chamados/services/tarefa_do_dia.py:128`, `:133`, `:142`, `:147`, `:523`. |
| `viva_c6` | Resposta transitória do C6. `app/chamados/services/tarefa_do_dia.py:491`. | Exposição administrativa e residual A/S. `app/chamados/services/tarefa_do_dia.py:245`, `:328`. |
| `_exportadas` / `exportada` | Carga consulta K6 e monta o conjunto. `app/chamados/services/tarefa_do_dia.py:442`, `:483`. | Filtra candidatas e protege as exposições novas. `app/chamados/services/tarefa_do_dia.py:485`, `:245`, `:327`. |
| `exportacoes` e resultado K6 | Registros vêm de `registrar`; vigência é anotada pelo modelo. K6 devolve booleano ou registro. `app/folha/models.py:174`, `:203`; `app/folha/export.py:591`, `:594`. | Novo leitor em lote na tarefa; leitores avulsos abaixo conservam a assinatura anterior. `app/chamados/services/tarefa_do_dia.py:442`. |

**Leitores fora do diff:**

- A carga/tarefa tem consumidor produtivo externo no instrumento de sombra, que constrói a carga e lê sua saída. `app/chamados/management/commands/diff_e3_sombra.py:115`, `:125`.
- Carimbo, propriedade `viva`, reconciliador e app continuam perguntando pelo estado atual, sem `agora`. A projeção não lhes entrega campos reescritos. `app/chamados/models.py:3139`, `:3375`; `app/chamados/reconciliador.py:661`; `app/chamados/services/pergunta_no_app.py:58`; `app/chamados/juizes.py:1380`.
- K6 é lido por fechamento, comparação e tela de TXT, ranking, vigia, pautas, backfill, HE pendente, lavra de competência e esmeril. `app/folha/export.py:618`, `:655`; `app/folha/views.py:354`; `app/ponto/services/fechamento.py:1537`; `app/folha/services/ranking_he.py:89`; `app/chamados/services/vigia_fio.py:841`, `:843`; `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:471`, `:498`; `app/ponto/services/he_pendente_lavrado.py:223`; `app/ponto/management/commands/lavrar_status_competencia.py:43`; `app/ponto/services/esmeril_espelho.py:58`.

**O leitor incompatível relevante é o gancho do cartório:** ele depende de mudança de impressão ou da palavra NB; a cura agora permite absolvidos sem ambos. `app/ponto/services/cartorio.py:1222`, `:1252`, `:1372`.

**5. Números de folha/TXT já exportados — NÃO PROCEDE alteração direta; NÃO SEI certificar delta zero após regeneração**

O hunk de `folha/export.py` altera a alimentação de K6, preservando booleano/registro e validando a tripla. Os hunks de materialização encaminham a carga aos mesmos derivadores; não acrescentam escrita de valores de folha. `app/folha/export.py:581`, `:588`, `:594`; `app/chamados/services/materializacao.py:373`, `:389`, `:425`.

O TXT armazenado continua sendo a cópia gravada por `registrar`, e o download devolve esse conteúdo. O teste C4 compara todas as colunas e contagens de fechamento/exportação depois de julgar a célula. `app/folha/models.py:182`, `:186`; `app/folha/views.py:561`; `app/ponto/tests/test_x1_folga_sem_adesao.py:150`, `:164`.

Entretanto, uma **nova geração** consulta a retenção atual e filtra quem entra no TXT. Uma célula que deixa NB pode alterar elegibilidade; C4 não executa essa regeneração. `app/folha/export.py:1241`, `:1262`, `:968`, `:714`, `:726`; `app/ponto/tests/test_x1_folga_sem_adesao.py:164`. Delta numérico efetivo em competências exportadas: **[nao verificado]**.

A suíte integral e as mortes dos mutantes informadas não foram reproduzidas integralmente nesta verificação; incidência dos contraexemplos em produção: **[nao verificado]**. [nao verificado]

## 11/11 06:54 · sessao b7e6cd8498b1 · TRAVA-SELO · verificador · origem=Code

**1. NÃO PROCEDE que todos os achados da v2 estejam fechados ou declarados como limite no próprio sítio.** Há correções efetivas, limites explícitos e lacunas restantes. (`bin/suite.sh:86–98`; `bin/vizinhos_da_volta.py:11–12,45–72`)

| Achado da v2 | Situação na v3 |
|---|---|
| Inteira disfarçada por branco, repetição, várias linhas ou `<app>.tests` | **Corrigido para essas formas:** a guarda separa todas as linhas e verifica pertinência por app, aceitando também seu pacote `.tests`. (`bin/suite.sh:94–100`) |
| Valores de flags depois de `--` virando recorte | **Corrigido:** `EXTRA` deixou de participar da seleção julgada; continua sendo encaminhado ao comando. (`bin/suite.sh:57,95–98,158–159`) |
| `--volta` devolvendo todas as labels | **Corrigido:** o helper preenche `ONLY` antes da guarda, sem dispensa por presença de `VOLTA`. (`bin/suite.sh:71–76,93–109`) |
| União de módulos equivalente à inteira | **Limite declarado**, sem fechamento da classe. (`bin/suite.sh:89–91`) |
| Comando canônico recusado e selo antigo aprovando-o | **Permanece, sem limite declarado nesses sítios:** a documentação continua com chamada nua; o selo aprova sua presença textual. (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:31–32,72–81`) |
| Cobertura insuficiente do selo | **Melhorou, mas permanece:** os casos acrescentados não incluem execução de `--volta`. (`bin/tests/test_suite_motivo.sh:31–48`) |
| Filtro relativo e normalização de `__init__.py` | **Fecha o contraexemplo anterior**, retirando a restrição de pacote; não fecha a classe geral de dependências Python transitivas. (`bin/vizinhos_da_volta.py:52–60,68–72`) |
| Imports dinâmicos e templates transitivos | **Limites declarados no helper.** (`bin/vizinhos_da_volta.py:11–12`) |
| Reexportações, efeitos transitivos de `__init__.py`, dependências transitivas de JSON e remoções | **Não fecha nem declara integralmente essas classes:** os padrões continuam derivados diretamente dos arquivos tocados, sem propagação transitiva. (`bin/vizinhos_da_volta.py:11–12,45–72`) |
| Migration com consumidores em outros apps | **Não fecha:** acrescenta somente o app do caminho; o comentário declara que esquema não se segue por texto, mas não resolve os consumidores externos. (`bin/vizinhos_da_volta.py:9,49–50`) |
| Selos que varrem diretório/AST | **Corrige o caso nominal do varredor de templates; não automatiza a classe:** a seleção continua dependendo de lista fixa. (`bin/selos_globais_da_volta.txt:25`; `bin/vizinhos_da_volta.py:73–74`) |
| Descoberta fora de `tests/` | **Parcial:** inclui `tests.py`, mas continua excluindo testes na raiz do app com outros nomes, inclusive exemplos reais. (`bin/vizinhos_da_volta.py:26–36`; `app/ferias/test_services_leitura.py:5–13`; `app/ferias/tests_disponibilidade.py:7–14`) |
| Rc 75 prevalecendo sobre vermelho | **Permanece corrigido.** (`bin/volta_intermediaria.sh:37–44`) |

**NÃO PROCEDE fuga nova nos exemplos de ordem e `read -d ''` examinados:**

| Caso | Resultado |
|---|---|
| Helper falha, mesmo imprimindo labels | Rc 1 antes da guarda e da execução, por `pipefail` e tratamento da atribuição. (`bin/suite.sh:34,74–75`) |
| `--seco --volta` | Executa helper e guarda antes do seco: falha do helper dá 1; inteira sem motivo dá 2; recorte chega ao seco. (`bin/suite.sh:71–76,102–115`) |
| `ONLY` com tab ou várias linhas | O `IFS` explícito contempla espaço, tab e newline; seleção vazia é inteira. (`bin/suite.sh:94–98`) |
| Array vazio sob `set -u` | No confronto em memória com Bash 5.2.21, não houve erro: vazio/branco/tab chegaram à recusa prevista. Outras versões específicas: **[nao verificado]**. (`bin/suite.sh:34,94–109`) |
| `--only ""` | Continua abortando no parser; não recebeu tratamento próprio. (`bin/suite.sh:51`) |

**PROCEDE uma lacuna restante:** `--only '--tag x'` é considerado recorte pela guarda, mas encaminha apenas flag e valor, sem label. Não foi criada pela troca de ordem nem pelo novo `read`; o conjunto efetivamente executado pelo Django é **[nao verificado]**. (`bin/suite.sh:51,95–98,158–159`)

**2. PROCEDE que os cinco casos novos discriminam suas classes; NÃO PROCEDE cobertura completa.** O juiz observa rc, chegada à trava e custo anterior à recusa. (`bin/tests/test_suite_motivo.sh:24–27,40–44`)

| Caso novo | Razão pertinente da mordida |
|---|---|
| `-- --tag x` | Detecta restaurar os valores de `EXTRA` como seleção. (`bin/tests/test_suite_motivo.sh:40`; `bin/suite.sh:95–98`) |
| Todas com repetição | Mata a troca de pertinência por contagem. (`bin/tests/test_suite_motivo.sh:41,67`) |
| `appA.tests appB` | Detecta retirar a alternativa `<app>.tests`. (`bin/tests/test_suite_motivo.sh:42`; `bin/suite.sh:98`) |
| Duas linhas | Detecta voltar ao `read` que considera apenas a primeira linha. (`bin/tests/test_suite_motivo.sh:43`; `bin/suite.sh:95`) |
| Dois módulos, positivo | Detecta contagem e normalização excessiva de qualquer módulo para seu app. Demonstra chegada à trava falsa, sem validar execução desses módulos. (`bin/tests/test_suite_motivo.sh:19,24–27,44,67`) |

Os cinco mutantes declarados têm mortes pertinentes: ausência da guarda pelos negativos; presença do flag por branco/todas; contagem por repetição e módulos; regex frouxa pelos motivos inválidos; custo antecipado pelo marcador de `teste_envfile`. (`bin/tests/test_suite_motivo.sh:17,26,31–48,65–72`)

**PROCEDE sobrevivência de mutantes plausíveis:**

- Restaurar `[ -n "$VOLTA" ] && return 1` em `_suite_inteira`: nenhuma chamada do juiz usa `--volta`, e a dispensa reabriria a inteira produzida pelo helper. (`bin/suite.sh:71–76,93–102`; `bin/tests/test_suite_motivo.sh:31–48`)
- Trocar `${ONLY:-$LABELS}` por `$LABELS` no executor: os recortes passariam a inteira, mas a trava falsa apenas registra a chamada e retorna zero; o juiz não confere os argumentos. (`bin/suite.sh:158`; `bin/tests/test_suite_motivo.sh:19,24–27,44–48`)

Os confrontos foram feitos com trechos e mutações em memória. Execução integral do selo: **[nao verificado]**; ele cria, copia e remove arquivos temporários. (`bin/tests/test_suite_motivo.sh:10–24`)

**3. NÃO PROCEDE invalidade por ausência de pacote `tests/`, nem duplicata textual criada pela mudança.** `comunicados/tests.py` vira `comunicados.tests`, correspondente ao módulo existente; candidatos e saída usam conjuntos. (`bin/vizinhos_da_volta.py:31–42,75–76`; `app/comunicados/tests.py:4–10`)

A retirada do filtro amplia os candidatos por homônimos, conforme declarado; ela não altera a formação das labels. (`bin/vizinhos_da_volta.py:31–32,58–60,68–72`)

**PROCEDE risco novo de selecionar testes de outra configuração:** tocar `app/chamados/models.py` permite selecionar `tenants.tests`, que importa esse módulo e usa `FastTenantTestCase`. A suite fixa CI, cuja configuração base não instala `tenants` nem `django_tenants`; esses apps aparecem na configuração SaaS. Erro efetivo no executor: **[nao verificado]**. (`bin/vizinhos_da_volta.py:35–36,54–57,68–72`; `app/tenants/tests.py:1–5,104`; `bin/suite.sh:159`; `app/config/settings/ci.py:1`; `app/config/settings/base.py:8–30`; `app/config/settings/saas.py:22–24`)

**NÃO SEI sobre duplicação efetiva de casos:** app inteiro e módulo do mesmo app podem coexistir na saída; o conjunto elimina strings iguais, não sobreposição de escopo. A resolução fica no Django delegado pelo runner. (`bin/vizinhos_da_volta.py:40–50,68–74`; `app/core/runner_quarentena.py:72–76`; comportamento externo: **[nao verificado]**)

**4. PROCEDE: escritores e leitores continuam identificáveis.**

| Variável | Quem escreve | Quem lê |
|---|---|---|
| `ONLY` da suite | Inicialização, parser e saída do helper. (`bin/suite.sh:47,51,74`) | Exclusão com `--volta`, validação da saída, guarda, seco e comando final. (`bin/suite.sh:72,76,95,114,158`) |
| `LABELS` da suite | Derivada por `grep` da régua. (`bin/suite.sh:66`) | Guarda e fallback do comando final. O seco imprime o marcador literal `<LABELS da regua>`. (`bin/suite.sh:97–98,114,158`) |
| `LABELS` canônica | Lista literal na régua. (`bin/regua.sh:23`) | Suite e demais portas que a extraem. (`bin/suite.sh:66`; `bin/pre-push.sh:184–187`; `app/core/hooks/pre-push.sh:80–83`) |
| `ONLY` própria da régua | Inicialização e parser próprios. (`bin/regua.sh:50–51`) | Comando e decisão de sair sem carimbo. (`bin/regua.sh:168,181`) |
| `LABELS` da fixture | Lista falsa `appA appB`. (`bin/tests/test_suite_motivo.sh:18`) | Suite copiada e mutante de contagem. (`bin/suite.sh:66,97,158`; `bin/tests/test_suite_motivo.sh:67`) |

Relógio, calendário, vigia e autocura mantêm leituras próprias da régua, independentes da posição da atribuição na suite. (`bin/regua_relogio.sh:27–29`; `bin/regua_calendario.sh:22,45`; `bin/vigia_arvore.sh:75,88`; `bin/molde_autocura/rodar.sh:25–26`) Os selos também leem a fonte ou inspecionam textualmente essa derivação. (`bin/tests/test_labels_fonte_unica.sh:20–34,103–105`; `bin/tests/test_suite_sh.sh:50–56`)

Mensagem do commit, motivos de descarte e resultados efetivos em produção: **[nao verificado]**. [nao verificado]

## 11/11 06:54 · sessao 2c2d11d5be1a · - · - · origem=Ronald (tmux)

- **Relógio fora da borda: PROCEDE pelo critério literal de sítio produtivo mudado.** `classificar_falta` mantém `_tz.now()` quando o avulso não fornece `agora`; o lote exige instante explícito. Leitor externo continua chamando sem instante. `app/escala/servico_jornada.py:292`, `app/escala/servico_jornada.py:387`, `app/escala/servico_jornada.py:566`. O novo caminho da tarefa fornece `self.agora`. `app/chamados/services/tarefa_do_dia.py:491`.

- **Segundo juiz / segundo escritor: NÃO PROCEDE nos sítios examinados.** C6 continua registrado em `core/juizes.py:278`; a carga chama esse juiz em `tarefa_do_dia.py:491`. A projeção apenas calcula variáveis locais e retorna tupla, sem alterar os campos da pergunta. `app/chamados/juizes.py:1369`, `app/chamados/juizes.py:1380`. A tarefa escreve somente `viva_c6` transitório e substitui a FK do colaborador em memória. `app/chamados/services/tarefa_do_dia.py:480`, `app/chamados/services/tarefa_do_dia.py:491`.

- **Derivação paralela de grade/slot/turnos: NÃO PROCEDE no código examinado.** A carga delega a `grade_da_celula`, `escala_vigente`, `_turnos_do_colab_puro`, `_slot_esperado` e `_batida_existente_na_janela`. `app/chamados/services/carga_pergunta_viva.py:119`, `app/chamados/services/carga_pergunta_viva.py:156`, `app/chamados/services/carga_pergunta_viva.py:162`, `app/chamados/services/carga_pergunta_viva.py:181`, `app/chamados/services/carga_pergunta_viva.py:190`. As batidas do pareamento continuam passando por `batidas_apuraveis`. `app/ponto/turnos.py:3017`.

- **Campo temporal com dois significados: não encontrei nova confusão demonstrável.** A projeção usa instante inclusivo: somente carimbos `> agora` são ocultados. `app/chamados/juizes.py:1372`, `app/chamados/juizes.py:1374`, `app/chamados/juizes.py:1376`. O recorte de batidas é `[lo, hi)`, enquanto o dia da carga é intervalo fechado; são eixos e contratos explícitos. `app/chamados/services/carga_pergunta_viva.py:175`, `app/chamados/services/carga_pergunta_viva.py:178`, `app/chamados/services/tarefa_do_dia.py:342`. A carga conserva deliberadamente identidade, ata, cobertura, fila e tranca atuais; não promete fotografia histórica integral. `app/chamados/services/tarefa_do_dia.py:354`.

Censo dos valores projetados:

| Valor | Escritor existente | Novos leitores |
|---|---|---|
| `validada_em` | `app/chamados/services/validacao.py:134`; também porta `carimbar` em `app/chamados/models.py:3135` | Projeção: `app/chamados/juizes.py:1369`; C6: `app/chamados/juizes.py:1384` |
| `respondida_em`, `resposta_colab` | `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1468`; resposta automática: `app/chamados/services/disputa_emissao.py:1652`; trilha: `app/chamados/services/disputa_emissao.py:1674` | Projeção: `app/chamados/juizes.py:1369`; C6: `app/chamados/juizes.py:1401`; slot: `app/chamados/juizes.py:347` |
| `materializada_em`, `via_resolucao` | `app/chamados/models.py:3097`; portas de gravação: `app/chamados/models.py:3129`, `app/chamados/models.py:3152`, `app/chamados/models.py:3252` | Projeção: `app/chamados/juizes.py:1370`; C6: `app/chamados/juizes.py:1396`, `app/chamados/juizes.py:1422`, `app/chamados/juizes.py:1460` |
| `agora` | Recebido na tarefa, guardado na carga e transferido à carga C6: `app/chamados/services/tarefa_do_dia.py:562`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/carga_pergunta_viva.py:88` | C6, slot e CF: `app/chamados/juizes.py:883`, `app/chamados/juizes.py:347`, `app/escala/servico_jornada.py:387` |
| `viva_c6` | `app/chamados/services/tarefa_do_dia.py:491` | `app/chamados/services/tarefa_do_dia.py:244`, `app/chamados/services/tarefa_do_dia.py:328` |

Leitores fora do diff continuam usando campos atuais: estado e propriedade `viva` do modelo, porta do carimbo, reconciliador e exposição no app. Não recebem uma pergunta com carimbos reescritos pela projeção. `app/chamados/models.py:3041`, `app/chamados/models.py:3139`, `app/chamados/models.py:3375`, `app/chamados/reconciliador.py:661`, `app/chamados/services/pergunta_no_app.py:58`.

O auxiliar de testemunha ainda lê carimbo/via crus, mas o novo C6 somente o alcança após verificar que a materialização projetada existe; não encontrei bypass concreto de carimbo futuro nesse caminho. `app/chamados/juizes.py:679`, `app/chamados/juizes.py:707`, `app/chamados/juizes.py:1460`, `app/chamados/juizes.py:1467`.

Executei os 14 testes sem banco de `test_e3_c8c_sem_banco` e `test_e3_pos_suite_sem_banco`: passaram; transcrição em arquivo inexistente, portanto resultado externo `[nao verificado]`. Os casos exercitam os contratos temporais e a passagem de instante. `app/chamados/tests/test_e3_c8c_sem_banco.py:70`, `app/chamados/tests/test_e3_c8c_sem_banco.py:123`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:43`.

## 11/11 06:54 · sessao ceceb2501ce1 · - · - · origem=Ronald (tmux)

**Q5 — NÃO PROCEDE para alteração direta do gravado; NÃO SEI para delta real após relavra/regeneração.** A mudança em `folha/export.py` só alimenta o juiz K6 com registros previamente anotados, conserva bool/registro e rejeita outra tripla; não altera cálculo nem persiste valores (`app/folha/export.py:556`, `app/folha/export.py:581`, `app/folha/export.py:588`). O teste C4 compara todas as colunas e contagens de fechamento/exportação depois do julgamento, mas não executa recalculador nem regerador (`app/ponto/tests/test_x1_folga_sem_adesao.py:150`, `app/ponto/tests/test_x1_folga_sem_adesao.py:164`).

O TXT já armazenado é a coluna `conteudo`, escrita em novo registro por `registrar`; baixar a exportação devolve essa cópia (`app/folha/models.py:86`, `app/folha/models.py:174`, `app/folha/views.py:548`, `app/folha/views.py:561`). Porém não se pode certificar “TXT futuro idêntico”: a retenção lê o veredito atual da célula, a classificação lê essa retenção e a montagem filtra quem entra; retirar NB pode alterar elegibilidade numa nova geração (`app/folha/export.py:1241`, `app/folha/export.py:1261`, `app/folha/export.py:968`, `app/folha/export.py:714`, `app/folha/export.py:726`). Quantidade real: [nao verificado].

**Q4 — carga e leitores.**

| Valor | Escritor/alimentador | Leitor |
|---|---|---|
| `agora` da carga | argumento gravado em `self.agora` | decisões, passivo, lavra, efeitos, fila, C6, teto e saída (`app/chamados/services/tarefa_do_dia.py:367`, `:391`, `:398`, `:402`, `:410`, `:415`, `:491`, `:506`, `:523`) |
| `_cadastro`, `_perg` | consultas e agrupamento | grade, seleção C6 e corpo puro (`app/chamados/services/tarefa_do_dia.py:370`, `:374`, `:380`, `:466`, `:480`, `:499`, `:521`) |
| `_dec`, `_retratada_depois` | sequência E1, corte inclusivo `ato_em <= agora` | corpo puro (`app/chamados/services/tarefa_do_dia.py:389`, `:391`, `:394`, `:523`, `:525`) |
| `_passivo`, `_lavra`, `_efeitos`, `_fila` | autoridades E1, lavra, batidas apuráveis e painel | `_pela_decisao` (`app/chamados/services/tarefa_do_dia.py:398`, `:403`, `:410`, `:415`, `:128`, `:142`, `:147`) |
| `_exportadas` | `_ler_exportadas` pergunta K6 após anotação de vigência | seleção e guardas das extensões (`app/chamados/services/tarefa_do_dia.py:441`, `:442`, `:483`, `:485`, `:245`, `:327`) |
| `viva_c6` | resposta de `pergunta_viva` | somente exposição administrativa e residual A/S (`app/chamados/services/tarefa_do_dia.py:491`, `:245`, `:328`) |
| `_cels`, `_vinculos`, `_cand` herdados | carga comum da janela | grade/carga C6 (`app/ponto/turnos.py:2980`, `:2986`, `:2990`, `app/chamados/services/tarefa_do_dia.py:499`, `app/chamados/services/carga_pergunta_viva.py:87`) |
| Registros K6: empresa/mês/ano, vigência, invalidação | `registrar`, `invalidar`, `anotar_vigencia` | K6 e seus leitores avulsos (`app/folha/models.py:174`, `:143`, `:203`, `app/folha/export.py:581`, `:591`) |

O consumidor produtivo externo da carga encontrado é o instrumento de sombra: constrói em `app/chamados/management/commands/diff_e3_sombra.py:115`, lê a tarefa em `:125` e projeta somente `rota/fila/verbos/fio` em `:27`, `:42`. Não encontrei consumidor externo de `viva_c6`; os dois leitores estão em `app/chamados/services/tarefa_do_dia.py:245` e `:328`.

Leitores K6 externos preservam a assinatura anterior: tela de exportação (`app/folha/views.py:354`), comparação TXT (`app/folha/export.py:655`), fechamento (`app/ponto/services/fechamento.py:1537`), ranking (`app/folha/services/ranking_he.py:89`), vigia (`app/chamados/services/vigia_fio.py:841`), cadastro/exportadas (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93`), backfill E1 (`app/chamados/management/commands/backfill_decisoes_pergunta.py:471`). O novo argumento `exportacoes=` aparece no consumidor da carga em `app/chamados/services/tarefa_do_dia.py:442`.

**Q1 — campo com dois significados: PROCEDE, candidato independente.** `data_civil` normalmente vem do instante local do marco ou fica `None` quando o instante não existe (`app/chamados/services/tarefa_do_dia.py:103`). Nas novas linhas sintéticas recebe diretamente `dia`, que é o dia do turno, com `hora/tipo=None` (`app/chamados/services/tarefa_do_dia.py:182`, `:218`, `:316`, `:331`). Uma saída fora da grade de turno 19–07 do dia D passa a trazer `data_civil=D` sem instante que justifique essa data; a convenção anterior produziria desconhecido (`app/chamados/services/tarefa_do_dia.py:103`, `:316`). Não encontrei leitor produtivo externo desse campo; o instrumento de sombra descarta os detalhes das instâncias (`app/chamados/management/commands/diff_e3_sombra.py:27`, `:42`).

**Selos.** Há exatamente 11 métodos `test_` em X1; o requisito `testsRun == 11` é consistente (`app/ponto/tests/test_x1_folga_sem_adesao.py:74`, `:83`, `:95`, `:144`, `:147`, `:150`, `:169`, `:185`, `:217`, `:233`, `:242`; `app/ponto/tests/x1_mutantes.py:78`). Os testes de tarefa exportada protegem as novas exposições e explicitamente conservam a ação anterior E2 (`app/chamados/tests/test_e3_c8b_sem_banco.py:118`, `:125`).

## 11/11 06:54 · sessao 6478c829e97a · - · - · origem=Ronald (tmux)

**Q2 — NÃO PROCEDE a equivalência.** `sem_adesao` significa `não cobertura integral && dia_sem_trabalho`; a absolvição decorre de `derivar_veredito` e só é renomeada NB se o resultado acusa. São perguntas diferentes. `app/ponto/services/cartorio.py:1211`, `app/ponto/services/cartorio.py:1222`, `app/ponto/services/cartorio.py:1237`, `app/ponto/services/cartorio.py:1252`.

Contraexemplos sintéticos, sem alegação de ocorrência em produção:

| Entrada | `dia_sem_trabalho` / marcador | Veredito novo |
|---|---|---|
| `trabalha=True`, `tipo_dia='trabalho'`, previsto=480, realizado=0, sem batidas, `justificado=True` | False / False | `concorde`: o classificador remove FURO e `[]` deriva concorde. `app/chamados/juizes.py:456`, `app/ponto/supra_juiz.py:371`, `app/escala/models.py:1627`. |
| Mesma jornada, sem justificativa, `turno_encerrado=False` | False / False | `concorde`: o furo exige turno encerrado. `app/chamados/juizes.py:456`, `app/ponto/supra_juiz.py:313`, `app/ponto/supra_juiz.py:316`. |
| `trabalha=True`, `tipo_dia='ausencia'`, cobertura parcial aprovada, todos os marcos dispensados, nenhum missing | False / False | `concorde`: `_parcial_sem_resto` impede FURO. `app/chamados/juizes.py:454`, `app/ponto/supra_juiz.py:311`, `app/ponto/supra_juiz.py:314`. |
| Parcial aprovada com resto missing, turno encerrado | False / False | `nunca_bateu`: acusa FURO e recebe a renomeação. `app/ponto/supra_juiz.py:313`, `app/ponto/supra_juiz.py:367`, `app/ponto/services/cartorio.py:1252`. |

O teste existente já exige `concorde` para trabalho justificado; o teste de trabalho NB cobre apenas trabalho ordinário, e o de parcial cobre apenas parcial com resto. `app/ponto/tests/test_e3_nb_impressao_memoria.py:264`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:270`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:281`.

**Consequência adicional — PROCEDE falha do gancho.** Nos três absolvidos acima, o marcador não entra. O ramo NB não imprime `enc`/`teto`; sua guarda pode pular antes de consultar se o turno encerrou. O ramo normal só força pela palavra NB ou por mudança de impressão. Assim, concorde sem marcador perde a garantia de rejulgamento pela primeira batida distante. `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1223`, `app/ponto/services/cartorio.py:1227`, `app/ponto/services/cartorio.py:1360`, `app/ponto/services/cartorio.py:1372`.

Executei os corpos AST usando a infraestrutura sem banco existente: trabalho justificado, turno em curso e parcial sem resto produziram `concorde`/marcador False; após encerrar o turno, a segunda execução pulou e conservou concorde. Simulando primeira batida viva distante, com janela local vazia e sem veto/chamados, justificado e parcial sem resto também pularam com o mesmo hash. São experiências sintéticas com bordas instrumentadas, não dados de produção. Infraestrutura: `app/ponto/tests/test_e3_nb_impressao_memoria.py:92`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:130`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:167`; consequências no corpo: `app/ponto/services/cartorio.py:1227`, `app/ponto/services/cartorio.py:1376`.

**Q3 — PROCEDE mudança possível de retenção de TRABALHO.** O hunk deixa de renomear como NB o trabalho justificado, em curso ou parcial sem resto. Quando julgado efetivamente — célula nova, impressão alterada ou `forcar=True` — esses casos passam de acusação NB para concorde. A folha retém NB e não retém concorde por esse motivo. `app/ponto/services/cartorio.py:1227`, `app/ponto/services/cartorio.py:1252`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:270`, `app/folha/export.py:1241`, `app/folha/export.py:1261`. Quantidade de trabalhos afetados em produção: **[não verificado]**.

**Pergunta/chamado — NÃO PROCEDE escritor direto novo no hunk; PROCEDE efeito indireto permitido pelo código existente.** A folga que deixa de acusar pode entrar no reconciliador, que carimba a pergunta por `VIA_CELULA_ABSOLVEU` e pode fechar a disputa. O consumidor de eventos chama esse caminho. `app/ponto/services/cartorio.py:1255`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:611`, `app/chamados/reconciliador.py:641`, `app/chamados/reconciliador.py:1365`. O novo teste de estoque explicitamente aceita a pergunta carimbada após reconciliar, em vez de exigir preservação integral. `app/ponto/tests/test_x1_folga_sem_adesao.py:255`, `app/ponto/tests/test_x1_folga_sem_adesao.py:263`. Existência dessas perguntas nas folgas raspadas e fechamento efetivo de pais: **[não verificado]**.

**Fallback — NÃO PROCEDE achado no trecho X1 lido.** O adaptador exige o insumo e pergunta ao juiz; insumo ausente levanta, sem predicado substituto. A renomeação consulta `VEREDITOS_ACUSAM`. `app/ponto/supra_juiz.py:65`, `app/ponto/supra_juiz.py:70`, `app/ponto/services/cartorio.py:1252`.

Comparação com a base limitada ao diff fornecido; a cópia não disponibiliza histórico Git para `git show`. **[não verificado]**

## 11/11 06:53 · sessao e32ab3974910 · - · - · origem=Ronald (tmux)

**2. PROCEDE para os casos medidos; NÃO PROCEDE cobertura completa.** O juiz distingue rc, chegada à trava e custo antes de recusa, mas não executa nem valida os argumentos encaminhados ao Django. (`bin/tests/test_suite_motivo.sh:19,24–27`)

| Caso novo | Mutação isolada que detecta |
|---|---|
| `-- --tag x` | Restaurar interpretação dos valores de `EXTRA` como seleção: retorna `0 sim`, contrariando `2 nao`. (`bin/tests/test_suite_motivo.sh:40`; `bin/suite.sh:95–98`) |
| Todas com repetição | Mutante de contagem deixa passar três palavras para dois apps. (`bin/tests/test_suite_motivo.sh:41,67`) |
| `appA.tests appB` | Retirar a alternativa `"$a.tests"` deixa passar. (`bin/tests/test_suite_motivo.sh:42`; `bin/suite.sh:98`) |
| Duas linhas | Voltar ao `read -r -a`, sem `-d ''`, lê apenas `appA` e deixa passar. (`bin/tests/test_suite_motivo.sh:43`; `bin/suite.sh:95`) |
| Dois módulos, esperado positivo | Detecta tanto contagem quanto normalização excessiva de qualquer módulo para seu app. (`bin/tests/test_suite_motivo.sh:44,67`; `bin/suite.sh:95–98`) |

Os confrontos acima foram executados apenas com trechos e mutações em memória; a execução integral do selo permanece **[nao verificado]**, pois ele cria/copia/remove arquivos. (`bin/tests/test_suite_motivo.sh:10–24`)

**Os cinco mutantes declarados morrem por razões pertinentes:** ausência da guarda pelos negativos; presença de flag por branco/todas; contagem por repetição e pelo positivo com módulos; regex frouxa pelos motivos com conteúdo adicional; guarda após custo pelo marcador de `teste_envfile`. (`bin/tests/test_suite_motivo.sh:31–48,65–72,17,26`)

A precedência do mutante de contagem não causa recusa de `ONLY=""` com motivo válido: `&&` e `||` associam à esquerda e o motivo ainda participa da condição; no confronto em memória, esse caso retornou `0 sim`. (`bin/tests/test_suite_motivo.sh:46–47,67`; precedência da linguagem: **[nao verificado]**)

**Há sobreviventes plausíveis:**

- Restaurar `[ -n "$VOLTA" ] && return 1` no início de `_suite_inteira`: nenhuma chamada do juiz usa `--volta`, portanto passa e reabre a exceção para helper que devolva todas as labels. (`bin/suite.sh:71–76,93–102`; `bin/tests/test_suite_motivo.sh:31–48`)
- Trocar `${ONLY:-$LABELS}` por `$LABELS` no executor: os recortes passam a executar a inteira; a trava falsa apenas grava o marcador e retorna zero, enquanto o juiz não examina seu argv. (`bin/suite.sh:158`; `bin/tests/test_suite_motivo.sh:19,24–27,44–48`)

**O achado do comando canônico continua PROCEDE:** a documentação mantém a chamada nua, recusada pela guarda; o selo antigo aprova presença textual de `bin/suite.sh` e mantém uma fixture nua como positiva, sem declaração local desse limite. (`CLAUDE.md:96`; `bin/suite.sh:95–109`; `bin/tests/test_suite_sh.sh:31–32,72–81`)

## 11/11 06:53 · sessao a8546779729d · - · - · origem=Ronald (tmux)

**Q1 — NÃO PROCEDE fechamento integral; NÃO PROCEDE fuga nova nos exemplos de ordem/read.** (`bin/suite.sh:71–110`)

- `--volta` que retorna todas as labels agora encontra a guarda já com `ONLY` preenchido e recebe rc 2 sem motivo; falha do helper, mesmo imprimindo labels, resulta rc 1 por `pipefail`. (`bin/suite.sh:34,74–76,95–109`)
- `--seco --volta` executa o helper e a guarda antes do seco; helper falho encerra rc 1, seleção inteira sem motivo encerra rc 2, seleção parcial chega ao seco. (`bin/suite.sh:71–76,102–115`)
- Tab isolado, labels separadas por tab e labels em várias linhas são separados pelo `IFS` explícito; vazio é inteira. Confrontei os trechos em memória sob Bash 5.2.21: todos os casos vazios/inteiros recusaram rc 2, sem erro de array vazio. Para outras versões específicas do Bash 5: **[nao verificado]**. (`bin/suite.sh:34,94–98,102–109`)
- Os argumentos depois de `--` deixaram de constituir recorte: sem `ONLY`, a guarda vê seleção vazia. Isso fecha a fuga antiga de `-- --tag x`, `-k padrao` e `--exclude-tag x`. (`bin/suite.sh:57,95–98,158–159`)
- `app.tests` agora vale como cobertura do app; a soma de módulos permanece explicitamente admitida como limite no próprio sítio. (`bin/suite.sh:89–98`)
- Permanece fuga de seleção por flags **dentro de ONLY**: `--only '--tag x'` contém palavras para a guarda, mas chega ao comando somente como flags/valor, sem labels. Confrontei a liberação da guarda em memória; conjunto efetivamente executado pelo Django: **[nao verificado]**. Esse comportamento já era possível antes do delta, portanto não é fuga nova da troca de ordem. (`bin/suite.sh:51,95–98,158–159`; comparação antiga: anexo `preverif_v2.r`, sem arquivo local **[nao verificado]**)
- `--only ""` continua abortando na expansão obrigatória do parser. (`bin/suite.sh:51`)
- O comando canônico nu continua na documentação e a fixture boa continua aprovando-o por simples referência textual à porta: achado antigo permanece. (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:31–32,72–81`)

**Q1 — descoberta: correções parciais e limites parciais.** (`bin/vizinhos_da_volta.py:11–12,26–36,51–72`)

- Retirada do filtro fecha o contraexemplo anterior de `pkg/tests/sub/__init__.py` citado por `from .sub import algo`, porque a regex relativa agora se aplica a qualquer candidato. (`bin/vizinhos_da_volta.py:52–60,68–72`)
- Não fecha a classe inteira de relativos: tocado `pkg/foo/bar.py` e consumidor `from ..foo.bar import x` não casam com `pkg.foo.bar`, caminho, `from pkg.foo import bar`, nem com o relativo que exige pontos imediatamente seguidos de `bar`; ocorrência concreta na obra: **[nao verificado]**. (`bin/vizinhos_da_volta.py:52–60`)
- Não há fechamento transitivo de imports/reexportações; o laço confronta diretamente cada candidato com os padrões dos caminhos tocados. O limite declarado cobre imports montados em string e templates transitivos, mas não declara expressamente toda essa classe Python. (`bin/vizinhos_da_volta.py:11–12,45–72`)
- `tests.py` foi incluído, mas arquivos `app/<app>/test_*.py` continuam fora tanto da obra direta quanto dos candidatos. Há caminhos concretos como `app/ferias/test_cadastro_idempotente.py`; conteúdo não inspecionado: **[nao verificado]**. (`bin/vizinhos_da_volta.py:26–28,35–36`)
- O vermelho prevalecendo sobre rc 75 permanece corrigido. (`bin/volta_intermediaria.sh:37–44`)

**Q2 — lacunas úteis para a consolidação.** (`bin/tests/test_suite_motivo.sh:14–19,29–49`)

O selo não inclui helper na fixture, não chama `--volta`, não chama `--seco` e não cobre tab em `ONLY`; os casos novos existentes estão nas linhas 40–44. Um mutante que restaure a dispensa de guarda quando `VOLTA` está preenchido sobreviveria ao juiz atual, porque nenhuma chamada do juiz preenche `VOLTA`. (`bin/tests/test_suite_motivo.sh:14–19,29–49`; `bin/suite.sh:102`)

Outro sobrevivente plausível: trocar o `IFS` explícito por separação que exclua tab e preserve espaço/newline. Os testes existentes usam espaço e newline; `ONLY=$'appA\tappB'` passaria a virar uma única palavra para a guarda, enquanto o comando continuaria separando por tab. (`bin/tests/test_suite_motivo.sh:31–48`; `bin/suite.sh:95,158`)

## 11/11 06:53 · sessao a73c06899c99 · - · - · origem=Ronald (tmux)

**P3 — NÃO PROCEDE invalidade por faltar pacote `tests/`, nem duplicata textual.** `tests.py` vira `<app>.tests`, pela remoção de `.py` e troca de `/` por `.`, e não `<app>.tests.tests`; o exemplo `comunicados.tests` corresponde ao módulo que define `ComunicadoContratoTests`. Candidatos e saída são conjuntos. (`bin/vizinhos_da_volta.py:26–36,37–42,75–76`; `app/comunicados/tests.py:4–10`)

**NÃO SEI sobre aceitação completa pelo executor.** A suite fixa `--settings=config.settings.ci`; o runner local herda `DiscoverRunner` e delega `build_suite` ao Django, cuja implementação não está na cópia. (`bin/suite.sh:158–159`; `app/config/settings/ci.py:40`; `app/core/runner_quarentena.py:15,72–76`; implementação externa: **[nao verificado]**)

**Há risco novo concreto de selecionar testes de outra configuração.** Como todos os `tests.py` entram, tocar `app/chamados/models.py` seleciona `tenants.tests`, pois este importa `chamados.models`; reproduzi a seleção em memória substituindo apenas o resultado do `git diff`. `tenants.tests` depende de `FastTenantTestCase`, mas `ci` usa `base`, cuja lista de apps não contém `tenants`/`django_tenants`; esses apps pertencem à configuração `saas`. Erro efetivo no executor: **[nao verificado]**. (`bin/vizinhos_da_volta.py:35–36,54–57,68–72`; `app/tenants/tests.py:1–2,5,19,104`; `app/config/settings/ci.py:1`; `app/config/settings/base.py:8–30`; `app/config/settings/saas.py:22–24`)

**Sobreposição de escopo continua possível:** migration acrescenta o app inteiro e a descoberta pode acrescentar módulos do mesmo app; o conjunto elimina igualdade de strings, não essa sobreposição. Duplicação efetiva de casos depende do Django: **[nao verificado]**. (`bin/vizinhos_da_volta.py:40–50,68–74`; `app/core/runner_quarentena.py:75–76`)

Fechamento dos achados v2 do helper:

| Classe | Conclusão |
|---|---|
| Filtro de relativo / `pkg/tests/sub/__init__.py` com consumidor `from .sub import algo` | **Corrige o contraexemplo apontado:** normaliza `__init__`, produz o nome `sub` e aplica a regex relativa a todos os candidatos. A ampliação pode trazer sobras por homônimos, conforme comentário explícito. (`bin/vizinhos_da_volta.py:52–60,68–72`) |
| Reexportações e dependências Python transitivas | **Não fecha e não declara esse limite.** Os padrões nascem somente dos arquivos tocados; selecionar um teste não cria padrões para dependências intermediárias. O limite textual menciona import montado em string e templates transitivos, não reexportações Python. (`bin/vizinhos_da_volta.py:11–13,45–72`) |
| `__init__.py`, em geral | **Parcial:** normalização e filtro corrigidos; permanece a busca textual direta, sem propagação geral dos efeitos de inicialização. (`bin/vizinhos_da_volta.py:45–60,68–72`) |
| Imports dinâmicos | **Limite declarado no sítio**, com exemplo real de import montado por f-string na cópia. (`bin/vizinhos_da_volta.py:11–12`; `app/ponto/tests/test_detectores_isencao.py:7–18`) |
| JSON/templates | Busca direta por caminho/basename permanece; **herança/inclusão transitiva de template é limite declarado**. Dependência transitiva de JSON não recebe declaração equivalente. (`bin/vizinhos_da_volta.py:11–12,61–66,68–72`) |
| Migration entre apps | Continua puxando apenas o primeiro componente do caminho, sem interpretar dependências entre apps. O comentário explicita “app inteiro” e “esquema não se segue por texto”, mas não apresenta defesa para consumidores em outros apps. (`bin/vizinhos_da_volta.py:9,49–50`) |
| Selos que varrem diretório/AST | **Corrige o caso nominal**, incluindo o varredor de templates; **não fecha a classe automaticamente**, pois a fonte continua uma lista fixa. (`bin/selos_globais_da_volta.txt:25`; `app/core/tests/test_contract_template_bem_formado.py:40,76–86`; `bin/vizinhos_da_volta.py:73–74`) |
| Remoções | Continua preservando o caminho para procurar citações; o arquivo removido não entra como obra por causa de `exists()`. Transitividade continua ausente e não está declarada para dependências Python. (`bin/vizinhos_da_volta.py:21–23,47–48,51–72`) |
| Descoberta fora de `tests/` | **Corrige `tests.py`, mas não toda a classe:** continuam fora `test_*.py` e `tests_disponibilidade.py` na raiz do app. Há exemplos reais com `TestCase` e imports de produção em `ferias`. (`bin/vizinhos_da_volta.py:26–36`; `app/ferias/test_services_leitura.py:5–13`; `app/ferias/tests_disponibilidade.py:7–14`) |

Colisão concreta entre um `tests.py` e uma pasta irmã `tests/` não foi encontrada na leitura dos três `tests.py` da cópia; essa ausência não dispõe de referência `arquivo:linha`: **[nao verificado]**.

## 11/11 06:46 · sessao f28b56e2711d · TRAVA-SELO · verificador · origem=Code

**1. NÃO PROCEDE que o delta feche todas as classes da volta 1.** A seleção continua sendo interpretada de forma diferente pela guarda e pelo comando executado. (`bin/suite.sh:81–87`, `bin/suite.sh:157–158`)

| Achado PROCEDE da v1 | Situação na v2 |
|---|---|
| Inteira disfarçada de recorte; `ONLY` com dois significados | **Parcialmente corrigido.** Branco e lista integral literal são recusados; permanecem as fugas abaixo. (`bin/suite.sh:83–98`) |
| Comando canônico recusado, selo antigo sem acusar | **Não corrigido.** A documentação mantém a chamada nua; o selo continua verificando strings e aprovando a fixture nua. (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:31–32,50–56,72–81`) |
| Selo novo exercita arquivo real, mas tem cobertura insuficiente | **Parcialmente corrigido.** Há novos negativos e quatro mutantes; continuam faltando classes de seleção. (`bin/tests/test_suite_motivo.sh:31–43,47–66`) |
| Descoberta incompleta de vizinhos | **Não fecha a classe**, conforme pergunta 3. (`bin/vizinhos_da_volta.py:49–71`) |
| Rc 75 prevalecendo sobre vermelho | **Corrigido na composição dos rc**, conforme pergunta 4. (`bin/volta_intermediaria.sh:37–44`) |
| Identificação de escritores/leitores | Continua identificável; a mudança de ordem tem efeito na precedência dos erros, conforme pergunta 5. (`bin/suite.sh:66–67,86,157`) |

**PROCEDE: há fugas na guarda nova.** (`bin/suite.sh:81–87`)

| Caso solicitado | Resultado da análise |
|---|---|
| Labels pontuadas cuja união cobre todos os apps | **Escapa.** A guarda exige cada token literal de `LABELS`; não reconhece cobertura por `ponto.tests` ou pela união de módulos. A equivalência concreta desses módulos à inteira nesta obra é **[nao verificado]**. (`bin/suite.sh:86–87`) |
| Uma label repetida | **Não cria fuga por si.** Todas as labels mais uma duplicata continuam recusadas; repetir apenas uma label não cobre as demais. O algoritmo verifica pertinência, não quantidade. (`bin/suite.sh:86–89`) |
| `-- --tag x`, `-- -k padrao`, `-- --exclude-tag x` | **Escapa da autorização.** Descarta a flag, mas incorpora seu valor como suposta label. Com `ONLY` vazio, libera e depois encaminha **todas as `LABELS`** ao executor. Quantos testes os filtros efetivamente selecionariam: **[nao verificado]**. (`bin/suite.sh:84–87,157–158`) |
| `-- --exclude-tag`, sem valor | Não apresenta essa fuga: não sobra seleção e a guarda recusa sem motivo. Aceitação pelo executor com motivo válido: **[nao verificado]**. (`bin/suite.sh:84–85,91–98`) |
| `--volta` cujo helper devolve todas as `LABELS` | **Escapa.** `VOLTA` dispensa a guarda antes de executar o helper; não há nova avaliação depois de preencher `ONLY`. (`bin/suite.sh:81,103–108`) |
| Espaço duplo em `LABELS` ou entre labels de `ONLY` | **Não cria fuga por si.** O `for` e o `read -a` separam essas palavras; a comparação continua por label. (`bin/suite.sh:83,86–87`) |
| `--only ""` | Continua abortando no parser, antes da guarda; não recebeu tratamento próprio como seleção vazia. (`bin/suite.sh:51`) |

Há outra divergência: `read` considera somente a primeira linha de `ONLY`, enquanto a expansão final separa também as seguintes. Com a fixture, `ONLY=$'appA\nappB'` libera na guarda e encaminha ambos. (`bin/suite.sh:83,157`; `bin/tests/test_suite_motivo.sh:18`)

**2. PROCEDE: os quatro mutantes declarados morrem pela razão pretendida; NÃO PROCEDE cobertura completa.** O juiz distingue recusa, chegada à trava e custo antes da recusa. (`bin/tests/test_suite_motivo.sh:17–27,31–43`)

| Mutante | Razão da morte |
|---|---|
| `aceita_sem_motivo` | Negativos deixam de retornar `2 nao`. (`bin/tests/test_suite_motivo.sh:31–39,60`) |
| `selecao_pela_presenca_do_flag` | Libera branco, todas as labels e todas após `--`. (`bin/tests/test_suite_motivo.sh:37–39,61`) |
| `regex_frouxa` | Aceita espaço extra, `;` e quebra de linha nos motivos negativos. (`bin/tests/test_suite_motivo.sh:34–36,62`) |
| `guarda_depois_do_custo` | Insere `teste_envfile` na condição: a recusa continua rc 2, mas vira `2 custo`, contrariando `2 nao`. (`bin/tests/test_suite_motivo.sh:17,26,64–66`) |

**Há mutante plausível sobrevivente:** substituir a pertinência de cada app pela igualdade entre quantidade de labels selecionadas e quantidade de apps. Preservando o caso vazio, passa os 13 casos atuais; com a fixture, `--only 'appB appA appA'` libera a inteira porque três palavras diferem dos dois apps. O vivo recusa essa entrada. (`bin/suite.sh:85–89`; `bin/tests/test_suite_motivo.sh:18,31–43`)

Esses comportamentos foram confrontados em memória. Execução integral do selo: **[nao verificado]**; o próprio selo cria, copia e remove arquivos temporários. (`bin/tests/test_suite_motivo.sh:10–24`)

**3. PROCEDE: há falso-negativo novo no filtro relativo; NÃO PROCEDE atribuir uma regressão ao glob isoladamente.** (`bin/vizinhos_da_volta.py:32–33,49–57,68–69`)

Contraexemplo de classe: toque em `pkg/tests/sub/__init__.py`; teste consumidor em `pkg/tests/test_x.py` contendo `from .sub import algo`. A normalização produz `pkg.tests.sub`, a regex relativa casa, mas o filtro descarta porque `pkg/tests` não começa por `pkg/tests/sub`. No delta v1 anexado, o padrão relativo desse caso não tinha esse filtro. Ocorrência concreta no diff da obra: **[nao verificado]**. (`bin/vizinhos_da_volta.py:49–57,68–69`)

O prefixo textual tampouco resolve níveis de importação: não interpreta quantos pontos sobem no pacote nem verifica fronteiras entre componentes de pasta. (`bin/vizinhos_da_volta.py:56–57,68`)

| Classe da v1 | Fechamento |
|---|---|
| Relativos/reexportações | **Não fecha:** permanece correspondência textual direta, sem fechamento transitivo. (`bin/vizinhos_da_volta.py:49–71`) |
| `__init__.py` | Normaliza o módulo, mas sofre a regressão acima e não propaga automaticamente para todos os consumidores. (`bin/vizinhos_da_volta.py:49,57,68–71`) |
| Imports dinâmicos | **Não fecha.** Existe teste que monta o destino com f-string; o helper procura formas textuais fixas. (`app/ponto/tests/test_detectores_isencao.py:7–18`; `bin/vizinhos_da_volta.py:51–56`) |
| JSON/templates | Deixaram de ser descartados pela extensão, mas entram por caminho/basename; herança e dependência transitiva continuam sem resolução. (`bin/vizinhos_da_volta.py:58–71`) |
| Migration | Puxa o app tocado; não acrescenta regra de dependência de esquema entre apps. (`bin/vizinhos_da_volta.py:46–47,65–71`) |
| Selos que varrem diretório/AST | Continua lista fixa. Há varredor de templates fora dela; seleção por um diff concreto: **[nao verificado]**. (`bin/vizinhos_da_volta.py:72–73`; `bin/selos_globais_da_volta.txt:1–24`; `app/core/tests/test_contract_template_bem_formado.py:40,76–86`) |
| Remoções | Agora preserva o caminho e procura citações, mas não fecha dependências transitivas. (`bin/vizinhos_da_volta.py:19–21,44–71`) |

O glob novo amplia a profundidade, mas mantém a lacuna antiga fora da pasta `tests`: por exemplo, `app/comunicados/tests.py` contém `TestCase`, importa produção e fica fora dos candidatos e da obra direta. (`bin/vizinhos_da_volta.py:24–25,32–33`; `app/comunicados/tests.py:4–10`)

**NÃO SEI** se a descoberta coincide exatamente com o executor: o runner da cópia delega ao Django; essa implementação não foi fornecida. (`app/core/runner_quarentena.py:72–76`; comportamento completo: **[nao verificado]**)

**4. PROCEDE: a tabela fecha os casos pedidos quando os comandos retornam ao wrapper.** (`bin/volta_intermediaria.sh:37–44`)

| Ruff | Disjuntor | Testes | Marcador próprio | Rc final | [nao verificado]
|---|---|---|---|---|
| 0 | 0 | 0 | nenhum | 0 |
| vermelho | qualquer | 75 | `VOLTA_VERMELHA` | 1 |
| 0 | 0 | 75 | `VOLTA_SEM_VEZ` | 75 |
| 0 | 0 | 1 | `VOLTA_VERMELHA` | 1 |
| 0 | 0 | outro não zero, exceto 75 | `VOLTA_VERMELHA` | 1 |

As linhas da tabela decorrem do `if/elif` e da consolidação subsequente. (`bin/volta_intermediaria.sh:37–44`)

Timeout **da espera pela trava** retorna 75; vermelho anterior vence esse resultado. Um timeout externo que encerre o wrapper antes da consolidação não tem conclusão garantida por esse fluxo: **[nao verificado]**. (`bin/trava_teste.sh:36–38`; `bin/volta_intermediaria.sh:32–44`)

**5. NÃO PROCEDE dependência demonstrada da ordem antiga; PROCEDE mudança na precedência de falhas.** (`bin/suite.sh:66–67,91–114`)

| Quem escreve/lê | Papel |
|---|---|
| `bin/regua.sh:23` | Declara a lista literal. |
| `bin/suite.sh:66` | Lê a régua e escreve sua variável `LABELS`. |
| `bin/suite.sh:86` | A guarda lê `LABELS`. |
| `bin/suite.sh:157` | O comando usa `LABELS` quando `ONLY` está vazio. |
| `bin/suite.sh:113` | Imprime o texto literal `<LABELS da regua>`; não expande a variável. |
| `bin/tests/test_suite_motivo.sh:18` | Escreve a lista falsa da fixture. |
| `bin/tests/test_labels_fonte_unica.sh:20–34,103–105` | Lê a fonte e julga cobertura. |
| `bin/tests/test_suite_sh.sh:50–56` | Inspeciona textualmente a presença da leitura, sem exigir posição. |

Outras portas fazem sua própria leitura da régua: pre-push, hook versionado, relógio, calendário, vigia e autocura. Essas leituras não dependem da posição da atribuição dentro de `suite.sh`. (`bin/pre-push.sh:184–187`; `app/core/hooks/pre-push.sh:80–83`; `bin/regua_relogio.sh:27–29`; `bin/regua_calendario.sh:22,45`; `bin/vigia_arvore.sh:75,88`; `bin/molde_autocura/rodar.sh:25–26`)

Agora uma falha ao obter `LABELS` encerra com rc 1 **antes** da recusa por motivo e antes de `--seco`, inclusive para chamadas com `--volta`. O comentário que promete recusa antes do custo de ler labels ficou incompatível com a ordem atual. (`bin/suite.sh:66–67,73,91–114`)

Mensagem do commit, motivos declarados de descarte e resultados reais em produção: **[nao verificado]**. [nao verificado]

## 11/11 06:45 · sessao be835ce87e0b · - · - · origem=Ronald (tmux)

Confirmado: o mutante que troca a pertinência de cada app por **igualdade da quantidade de labels** sobrevive aos 13 casos atuais. (`bin/suite.sh:85–89`; `bin/tests/test_suite_motivo.sh:31–43`)

Com a fixture `LABELS="appA appB"`, `--only 'appB appA appA'` recebe **rc 2 no vivo e rc 0 no mutante**: a duplicata permite liberar a seleção integral sem motivo. Verificado por simulação em memória, sem arquivos. (`bin/tests/test_suite_motivo.sh:18`; `bin/suite.sh:85–98`)

## 11/11 06:45 · sessao 011136d78e46 · - · - · origem=Ronald (tmux)

**3. PROCEDE: a classe não fecha, e há falso-negativo novo plausível no filtro relativo.** O glob amplia profundidade, mas o filtro por prefixo de pasta perde consumidores de inicializadores de pacote. (`bin/vizinhos_da_volta.py:32`, `bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:57`, `bin/vizinhos_da_volta.py:68`)

- **Regressão nova demonstrável:** tocado `pkg/tests/sub/__init__.py`; consumidor `pkg/tests/test_x.py` contém `from .sub import algo`. A normalização produz módulo `pkg.tests.sub`, a regex relativa casa, porém o filtro exige que `pkg/tests` comece por `pkg/tests/sub` e descarta o consumidor. Na v1 anexada, `pkg` era `pkg.tests`, satisfazia `pkg.endswith('tests')` e a regex relativa casava sem esse filtro. Simulação em memória confirmou `old_match=True`, `new_regex_match=True`, `new_package_filter_passes=False`; ocorrência concreta na obra: **[nao verificado]**. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:50`, `bin/vizinhos_da_volta.py:56`, `bin/vizinhos_da_volta.py:57`, `bin/vizinhos_da_volta.py:68`)
- **Glob:** `tests/**/test*.py` amplia o antigo `tests/*.py`; não demonstrou regressão por si. Permanece a lacuna antiga de testes fora da pasta `tests`: existem `TestCase` em `app/comunicados/tests.py` e testes em `app/ferias/test_cadastro_idempotente.py`, excluídos tanto da obra quanto dos candidatos. O runner delega a descoberta ao Django; a implementação exata do executor não está fornecida: **[nao verificado]**. (`bin/vizinhos_da_volta.py:24`, `bin/vizinhos_da_volta.py:32`; `app/comunicados/tests.py:4`, `app/comunicados/tests.py:10`; `app/ferias/test_cadastro_idempotente.py:2`, `app/ferias/test_cadastro_idempotente.py:6`; `app/ferias/tests.py:19`; `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`)

| Classe da v1 | Fechamento na v2 |
|---|---|
| Relativo/reexportação | **Não fecha:** referências textuais diretas; sem resolução dos níveis nem fechamento transitivo. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:56`, `bin/vizinhos_da_volta.py:65`) |
| `__init__.py` | Normaliza o nome, mas introduz a regressão de prefixo descrita acima; trânsito continua ausente. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:57`, `bin/vizinhos_da_volta.py:68`) |
| `importlib`/destino dinâmico | Nome literal pode casar; montagem dinâmica continua sem resolução. Existe teste que monta o destino com f-string. (`bin/vizinhos_da_volta.py:51`; `app/ponto/tests/test_detectores_isencao.py:7`, `app/ponto/tests/test_detectores_isencao.py:8`, `app/ponto/tests/test_detectores_isencao.py:12`) |
| JSON/template | Agora participa por caminho/basename; dependência transitiva, herança e destino montado não receberam resolução. (`bin/vizinhos_da_volta.py:58`, `bin/vizinhos_da_volta.py:63`, `bin/vizinhos_da_volta.py:65`) |
| Migration | Puxa o app tocado; não há regra nova para apps dependentes do esquema sem referência textual. (`bin/vizinhos_da_volta.py:46`, `bin/vizinhos_da_volta.py:47`, `bin/vizinhos_da_volta.py:65`) |
| Selo que varre diretório/AST | Continua lista fixa, sem declaração de escopo. Existe varredor de todos os templates fora dela; exclusão por um diff concreto: **[nao verificado]**. (`bin/vizinhos_da_volta.py:72`; `bin/selos_globais_da_volta.txt:1`; `bin/selos_globais_da_volta.txt:24`; `app/core/tests/test_contract_template_bem_formado.py:40`, `app/core/tests/test_contract_template_bem_formado.py:76`, `app/core/tests/test_contract_template_bem_formado.py:78`) |
| Remoções | Conserva agora o caminho removido e procura citações; não inclui como obra módulo inexistente nem fecha trânsito. (`bin/vizinhos_da_volta.py:19`, `bin/vizinhos_da_volta.py:21`, `bin/vizinhos_da_volta.py:44`, `bin/vizinhos_da_volta.py:48`, `bin/vizinhos_da_volta.py:65`) |

## 11/11 06:44 · sessao 90a0e384d5c4 · - · - · origem=Ronald (tmux)

**4. PROCEDE — a composição fecha os casos pedidos no fluxo normal.** (`bin/volta_intermediaria.sh:16,37–44`)

| ruff | disjuntor | testes | marcador próprio | rc final | [nao verificado]
|---|---|---|---|---|
| 0 | 0 | 0 | nenhum | 0 |
| vermelho | qualquer | 75 | `VOLTA_VERMELHA` | 1 |
| 0 | 0 | 75 | `VOLTA_SEM_VEZ` | 75 |
| 0 | 0 | 1 | `VOLTA_VERMELHA` | 1 |

A condição `if/elif` torna os marcadores próprios exclusivos; vermelho anterior vence testes 75. (`bin/volta_intermediaria.sh:37–44`)

O timeout **da espera pela trava** retorna 75; a suite repassa `--espera` e executa a trava. (`bin/trava_teste.sh:36–38`; `bin/suite.sh:53,151`; `bin/volta_intermediaria.sh:32`) Timeout externo que mate o wrapper antes da consolidação: **[nao verificado]**. Execução real desses casos: **[nao verificado]**.

**5. NÃO PROCEDE dependência demonstrada da ordem antiga; PROCEDE mudança na precedência dos erros.** (`bin/suite.sh:66–67,86,91–114,157`)

| Papel | Arquivo:linha |
|---|---|
| Declaração literal da lista | `bin/regua.sh:23` |
| Suite escreve sua variável `LABELS`, lendo a fonte | `bin/suite.sh:66` |
| Guarda lê a variável | `bin/suite.sh:86` |
| Comando lê como fallback de `ONLY` | `bin/suite.sh:157` |
| Saída seca imprime apenas texto literal `<LABELS da regua>`, sem ler `$LABELS` | `bin/suite.sh:113` |

As demais portas leem diretamente a régua e escrevem sua própria variável: pre-push (`bin/pre-push.sh:184–187`), hook versionado (`app/core/hooks/pre-push.sh:80–83`), relógio (`bin/regua_relogio.sh:27–29`), calendário (`bin/regua_calendario.sh:22,45`), vigia (`bin/vigia_arvore.sh:75,88`) e molde de autocura (`bin/molde_autocura/rodar.sh:25–26`). Essas leituras não dependem da posição da atribuição dentro da suite. (`bin/suite.sh:66`; referências anteriores)

O selo textual exige a presença da leitura, sem verificar sua ordem. (`bin/tests/test_suite_sh.sh:50–56`)

Agora falha na leitura de LABELS produz rc 1 **antes** da recusa e de `--seco`; o comentário “ANTES de qualquer custo (labels...)” contradiz a ordem atual. (`bin/suite.sh:66–67,73,91–114`)

**Pendência da volta 1 permanece: comando canônico e selo antigo.** A documentação ainda manda executar a suite nua, recusada pela guarda; o selo continua aprovando apenas a referência textual à porta, inclusive na fixture “boa” nua. (`CLAUDE.md:96`; `bin/suite.sh:85,91–98`; `bin/tests/test_suite_sh.sh:31–32,72–81`)

## 11/11 06:44 · sessao be835ce87e0b · - · - · origem=Ronald (tmux)

**2. PROCEDE quanto aos quatro mutantes declarados; NÃO PROCEDE como cobertura completa da guarda.** O juiz exige recusa `2 nao`, chegada `0 sim` e sinaliza custo antes da recusa como `2 custo`. (`bin/tests/test_suite_motivo.sh:26–43`)

| Mutante | Razão da morte |
|---|---|
| `aceita_sem_motivo` | Desliga a guarda; negativos deixam de retornar 2. (`bin/tests/test_suite_motivo.sh:31–39,60`) |
| `selecao_pela_presenca_do_flag` | Libera branco, todas as labels e todas após `--`; esses casos agora são negativos explícitos. (`bin/tests/test_suite_motivo.sh:37–39,61`) |
| `regex_frouxa` | Aceita obra com espaço extra, `;` ou quebra de linha, contrariando os três negativos novos. (`bin/tests/test_suite_motivo.sh:34–36,62`) |
| `guarda_depois_do_custo` | Executa `teste_envfile` antes de decidir; a recusa continua rc 2, mas recebe `2 custo`. (`bin/tests/test_suite_motivo.sh:17,26,64–66`) |

**Há mutante plausível sobrevivente:** substituir a verificação de pertinência de cada app por comparação da quantidade de palavras selecionadas com a quantidade de apps. Os casos atuais distinguem apenas zero, uma e duas labels; com a fixture `appA appB`, esse mutante passa todos, mas recusaria incorretamente `--only 'appA appA'`. (`bin/suite.sh:85–89`; `bin/tests/test_suite_motivo.sh:18,31–43`)

As variantes foram verificadas com `bash -n` por stdin e simulação em memória do parser/guarda real; as quatro são sintaticamente válidas e apresentam as falhas acima. Execução integral do selo: **[nao verificado]**, pois ele cria e remove arquivos temporários. (`bin/tests/test_suite_motivo.sh:10–24`)

## 11/11 06:40 · sessao bc1418e2b9d3 · TRAVA-SELO · verificador · origem=Code

**1. PROCEDE — há caminho para encaminhar todas as LABELS sem motivo válido.** A guarda verifica apenas se `ONLY`/`VOLTA` estão vazios; não verifica o alcance efetivo da seleção. Basta fornecer todas as labels em `--only`. (`bin/suite.sh:70`, `bin/suite.sh:142`; lista em `bin/regua.sh:23`)

| Entrada | Resultado |
|---|---|
| Sem argumentos | Recusa rc 2, oferecendo `--only`/`--volta`. (`bin/suite.sh:70–77`) |
| `--only ""` | Aborta na expansão `${2:?…}`, antes da guarda; execução direta retornou **rc 1**, não 2. (`bin/suite.sh:51`) |
| `--only " "` | Passa pela guarda; a expansão sem aspas desaparece e encaminha `manage.py test` **sem labels posicionais**. A equivalência exata dessa descoberta com todas as LABELS é **[nao verificado]**. (`bin/suite.sh:70,142`; `app/core/runner_quarentena.py:72–76`) |
| `--only` contendo todas as LABELS | Encaminha todas sem motivo. É a fuga suficiente para responder “sim”. (`bin/suite.sh:51,70,142`; `bin/regua.sh:23`) |
| `--volta` com diff vazio válido | Produz os 24 selos globais; não cai na suite inteira nem na recusa “diff vazio”. (`bin/vizinhos_da_volta.py:17,36–39`; `bin/selos_globais_da_volta.txt:1–24`; `bin/suite.sh:85–87`) |
| `-- <labels>` sem recorte/motivo | Recusa: `EXTRA` não participa da guarda. (`bin/suite.sh:57,70`) |
| `--only " " -- <todas as labels>` | Passa; todas as labels chegam pelo `EXTRA`, como único bloco posicional após as opções. (`bin/suite.sh:57,70,142–143`) |
| `--only X -- <outras labels>` | A guarda libera, mas a cópia registra recusa do argparse por duas levas posicionais. Não contar esse formato como suite executada. (`bin/suite.sh:142–143`; `app/docs/RELATO.md:1363`) |
| `--seco` | Sem motivo/recorte recebe rc 2; com entrada autorizada imprime e sai 0 antes de executar testes. (`bin/suite.sh:70–93`) |
| Ordem e repetição | Antes de `--`, prevalece a última atribuição; depois dele, os argumentos ficam em `EXTRA`. `--only` e `--volta` juntos são recusados, independentemente da ordem. (`bin/suite.sh:48–61,83`) |
| Motivo com quebra de linha | Na inteira nominal, não casa com a regex ancorada e é recusado; um `ONLY` não vazio continua dispensando essa validação. (`bin/suite.sh:69–70`) |

**2. PROCEDE — o comando canônico quebra, e o selo antigo não acusa.** (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:31–58`)

| Chamador/instrução | Passa a ser recusado? |
|---|---|
| `CLAUDE.md:96`: chamada nua | **Sim, rc 2**, antes de env-file, montagem e trava. (`bin/suite.sh:70–77,104,117,136`) |
| `bin/volta_intermediaria.sh:32` | **Não pela guarda de motivo**: fornece `--volta`; ainda pode falhar na seleção/montagem/trava. (`bin/suite.sh:70,82–87,113–118,136`) |
| `bin/tests/test_suite_motivo.sh:24` | As chamadas negativas são recusadas deliberadamente; positivas e `--only` devem alcançar a trava falsa. (`bin/tests/test_suite_motivo.sh:29–34`) |
| Orientação de recorte em `app/core/placar_estrutural.py:109–110` | `--only` é admitido pela guarda; trata-se de texto publicado, não execução Python da suite. (`bin/suite.sh:70`) |
| Receitas com apenas `--dir` | **Sim**, se coladas sem motivo/recorte: aparecem em `app/docs/RELATO-ARQUIVO.md:37800`, `app/docs/RELATO.md:681,1137,1145` e na orientação de `app/docs/PROMPTS.md:1069`/`app/docs/BACKLOG.md:1433`. (`bin/suite.sh:50,70–77`) |

`regua.sh`, `pre-push.sh` e o hook versionado executam testes diretamente; esta guarda não os alcança. (`bin/regua.sh:165–169`; `bin/pre-push.sh:186–188`; `app/core/hooks/pre-push.sh:82–84`)

Outros chamadores em `.claude/` ou hooks não foram localizados na busca textual; ausência de chamadas construídas dinamicamente: **[nao verificado]**.

O `test_suite_sh.sh` permanece **logicamente verde**: procura strings, não executa o comando canônico. Sua fixture “boa” é justamente a chamada nua agora recusada. Verde efetivamente executado nesta revisão: **[nao verificado]**. (`bin/tests/test_suite_sh.sh:31–58,72–88`)

**3. PROCEDE quanto ao arquivo real e ao mutante; NÃO PROCEDE como prova completa da guarda.** O teste copia o `suite.sh` real e o executa com recursos, labels e trava falsos. Não replica a implementação da guarda, mas também não exercita a integração real. (`bin/tests/test_suite_motivo.sh:14–24,38–39`)

O `sed` corresponde à guarda atual e troca sua condição por `if false; then`; desliga efetivamente a recusa. O selo também acusa mutação que não se aplicou. (`bin/suite.sh:70`; `bin/tests/test_suite_motivo.sh:44–48`)

| Mutante plausível | Cobertura |
|---|---|
| Guarda desligada | **Morre**: as três entradas negativas chegam à trava falsa. (`bin/tests/test_suite_motivo.sh:29–31,44–48`) |
| Regex frouxa, como aceitar qualquer texto após `pre-opus ` | **Pode sobreviver**: faltam negativos com espaços, caracteres indevidos e quebra de linha. (`bin/tests/test_suite_motivo.sh:29–34`) |
| Guarda depois da preparação que pode chamar Docker | **Pode sobreviver**: `teste_envfile` falso é inócuo, e o juiz observa somente rc e chegada à trava. O `teste_envfile` real pode executar Docker. (`bin/tests/test_suite_motivo.sh:17,22–34`; `bin/recursos.sh:122`) |
| Guarda depois do `exec` final | **Morre**: seria inalcançável e a chamada nua chegaria à trava falsa. (`bin/suite.sh:136`; `bin/tests/test_suite_motivo.sh:29`) |
| Parser aceitando `--only ""` como seleção em branco | **Pode sobreviver**: nenhum caso testa vazio ou whitespace. (`bin/tests/test_suite_motivo.sh:29–34`) |

As seis expectativas atuais também foram confrontadas em memória: sobreviveram variantes com regex frouxa, guarda após `teste_envfile` e vazio convertido em espaço; isso confirma a limitação dessas expectativas, sem executar o selo original. (`bin/tests/test_suite_motivo.sh:29–34`; pontos alterados: `bin/suite.sh:51,69–78,104`)

**Sim, está integrado à pasta de selos da régua**, pelo glob `tests/test_*.sh`; usa subprocessos e arquivos temporários, com a trava falsa encerrando antes do Docker. Não exige banco/Docker para seus casos atuais. (`bin/regua.sh:138–144`; `bin/tests/test_suite_motivo.sh:10–24`)

**4. PROCEDE — existem lacunas de classe na descoberta.** O helper usa referências textuais diretas, não AST nem fechamento transitivo; considera apenas `.py` existentes. (`bin/vizinhos_da_volta.py:10–12,26–35`)

| Classe | Limitação | Regra mínima sugerida |
|---|---|---|
| Import relativo/reexportação | Não resolve pacote, nível ou trânsito; o padrão relativo só aparece sob condição especial. (`bin/vizinhos_da_volta.py:25–35`) | Resolver imports no contexto do pacote e fechar dependências transitivas. |
| `__init__.py` | Há normalização parcial do inicializador de produção; inicializador de testes não entra como obra nem produção. (`bin/vizinhos_da_volta.py:13,19–24`) | Propagar alterações do inicializador aos consumidores e ao pacote de testes correspondente. |
| `importlib`/string | Nome completo literal pode casar; destinos montados dinamicamente não são resolvidos. (`bin/vizinhos_da_volta.py:26–35`) | Resolver destinos estáticos e ampliar conservadoramente quando indeterminados. |
| Fixture JSON | Alterações não-Python são descartadas. (`bin/vizinhos_da_volta.py:12`) | Incluir dependências de recursos e seus consumidores. |
| Template | Também descartado pelo filtro de extensão. (`bin/vizinhos_da_volta.py:12`) | Incluir consumidores, herança e inclusões de templates. |
| Migration | Pode entrar como `.py`, mas não há propagação por dependência de esquema. (`bin/vizinhos_da_volta.py:12,21–35`) | Selecionar testes do app e de seus dependentes de esquema. |
| Selo que varre diretório/AST | Não relaciona escopo varrido com diff; a lista fixa cobre somente os selos nela declarados. (`bin/vizinhos_da_volta.py:31–37`) | Declarar escopos de varredura e selecionar por interseção com arquivos tocados. |

Há ainda descoberta incompleta de testes e exclusão de arquivos removidos: candidatos exigem `tests/*.py` com nome começando por `test`, e tocados precisam continuar existindo. A regra mínima é acompanhar a descoberta do executor e preservar dependências de remoções. (`bin/vizinhos_da_volta.py:12,15–16`)

Para **cada classe**, o risco é excluir um teste dependente e obter volta verde com vermelho fora do recorte; ocorrência concreta nesta obra: **[nao verificado]**. [nao verificado]

**5. NÃO PROCEDE que o rc final distinga os estados sem ambiguidade.** Existem 0, 1 e 75, mas **75 prevalece mesmo quando ruff ou disjuntor já estão vermelhos**. Além disso, timeout escreve tanto `VOLTA_VERMELHA` quanto `VOLTA_SEM_VEZ`. (`bin/volta_intermediaria.sh:35–39`)

- **`TESTE_DOCKER`: correto** na composição: vem de `recursos.sh` e sua expansão fornece as flags ao Docker. (`bin/volta_intermediaria.sh:15,23`; `bin/recursos.sh:81–82`)
- **`PIPESTATUS[0]`: correto** para capturar Docker/ruff imediatamente após o pipeline; falha exclusiva do `tail` não entra nesse valor. (`bin/volta_intermediaria.sh:24–25`)
- **`VOLTA_ESPERA`: chega**, convertido em `--espera`, armazenado e repassado ao `flock -w`. (`bin/volta_intermediaria.sh:32`; `bin/suite.sh:53,136`; `bin/trava_teste.sh:27,36`)
- **`VOLTA_PORQUE`: chega**, pelo ambiente, e determina o destino do stderr do helper. (`bin/volta_intermediaria.sh:32`; `bin/suite.sh:85`)

**6. PROCEDE — escritores e leitores identificados abaixo.** (`bin/suite.sh:47–56,70–93`; `bin/vizinhos_da_volta.py:36–42`)

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `MOTIVO` | Inicialização e parser `--motivo`; sobrescreve eventual ambiente homônimo. (`bin/suite.sh:47,54`) | Guarda, recusa e saída seca. (`bin/suite.sh:70,72,91`) |
| `VOLTA` | Inicialização e parser; wrapper fornece `BASE`. (`bin/suite.sh:47,55`; `bin/volta_intermediaria.sh:13,32`) | Guarda, ativação do helper, argumento/erro e saída seca. (`bin/suite.sh:70,82,85–86,91`) |
| `SECO` | Inicialização 0 e parser escreve 1. (`bin/suite.sh:47,56`) | Condição que imprime e encerra. (`bin/suite.sh:90–93`) |
| `VOLTA_PORQUE` | Wrapper fornece o caminho no ambiente. (`bin/volta_intermediaria.sh:32`) | Suite lê o caminho e abre/trunca o destino; helper escreve justificativas no stderr; wrapper lê o arquivo para contar labels. (`bin/suite.sh:85`; `bin/vizinhos_da_volta.py:40–42`; `bin/volta_intermediaria.sh:34`) |
| `selos_globais_da_volta.txt` | Lista literal versionada; escritor automático não identificado: **[nao verificado]**. (`bin/selos_globais_da_volta.txt:1–24`) | Helper lê o arquivo **ao lado de si próprio**, não da cópia-alvo, e acrescenta suas entradas. (`bin/vizinhos_da_volta.py:36–37`) |

Sem `VOLTA_PORQUE`, a justificativa vai para `/dev/null`. O motivo aparece na recusa/saída seca; registro dele no RELATO: **[nao verificado]**. (`bin/suite.sh:72,85,91`)

**7. Checklist — há reprovações de cobertura e representação de estado; as demais classes não foram demonstradas no diff.** (`bin/suite.sh:70,142`; `bin/tests/test_suite_motivo.sh:29–34`; `bin/volta_intermediaria.sh:35–39`)

| Classe | Parecer |
|---|---|
| Segundo juiz duplicando registrado | **NÃO PROCEDE no código alterado examinado**: os novos julgamentos são de autorização/seleção de testes e consolidação do instrumento; duplicação de juiz de domínio não demonstrada. (`bin/suite.sh:69–87`; `bin/vizinhos_da_volta.py:18–37`; `bin/volta_intermediaria.sh:35–39`) |
| Segundo escritor fora da porta | **NÃO PROCEDE no fluxo examinado**: a suite mantém montagem delegada e execução pela trava; os novos destinos são saídas do instrumento. (`bin/suite.sh:117,136`; `bin/volta_intermediaria.sh:29,32,37–41`) |
| Relógio fora da borda | **NÃO PROCEDE como regra de domínio**: os novos `date` medem/publicam duração no wrapper de host. Estabilidade perante ajuste do relógio civil: **[nao verificado]**. (`bin/volta_intermediaria.sh:16,40`) |
| Derivação paralela da lista integral | **NÃO PROCEDE como cópia de LABELS**: a lista integral continua lida da régua; o TXT declara um subconjunto fixo de módulos. Porém, **o selo não detecta esse TXT como repetição**: só busca determinada forma inline de `manage.py test … colaboradores … ponto`. (`bin/suite.sh:99`; `bin/vizinhos_da_volta.py:36`; `bin/tests/test_labels_fonte_unica.sh:38–46`) |
| Selo sem mordida | **PROCEDE como cobertura insuficiente**: o novo mata o mutante declarado, mas omite as fugas identificadas; o antigo aprova textualmente o comando canônico recusado. (`bin/tests/test_suite_motivo.sh:29–34,44–48`; `bin/tests/test_suite_sh.sh:31–58,72–81`) |
| Ordem de trava | **NÃO PROCEDE inversão demonstrada**: a recusa antecede preparação, e os testes Django continuam sob a trava; ruff roda antes dela. (`bin/suite.sh:70–77,104,136–143`; `bin/volta_intermediaria.sh:23,32`) |
| Campo com dois significados | **PROCEDE**: `ONLY` “tem texto” é tratado como “é recorte”, embora possa representar toda a suite ou nenhum argumento; rc 75 também pode coexistir com vermelho anterior. (`bin/suite.sh:70,142`; `bin/volta_intermediaria.sh:35–39`) |

## 11/11 06:35 · sessao ac4da24cbf3b · - · - · origem=Ronald (tmux)

P4 — **PROCEDE: a seleção tem lacunas de classe.** O helper considera apenas `.py` existentes no diff e procura referências textuais diretas; não resolve AST, grafo transitivo ou dependências de recursos. (`bin/vizinhos_da_volta.py:10`, `bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:31`)

| Classe | Limite e regra mínima sugerida |
|---|---|
| Import relativo | O padrão relativo só é acrescentado quando `pkg.endswith('tests')`; não resolve pacote nem nível. Resolver `ImportFrom` em contexto e dependências transitivas. (`bin/vizinhos_da_volta.py:25`, `bin/vizinhos_da_volta.py:29`) |
| `__init__.py` | Há normalização do inicializador de produção para pacote; portanto existe suporte parcial. O inicializador de testes não entra como obra nem produção. Propagar dependência do inicializador aos consumidores do pacote, incluindo o pacote de testes. (`bin/vizinhos_da_volta.py:13`, `bin/vizinhos_da_volta.py:19`, `bin/vizinhos_da_volta.py:21`, `bin/vizinhos_da_volta.py:24`) |
| `importlib`/strings | Uma string contendo o módulo completo pode casar; nomes montados dinamicamente não têm resolução. Resolver destinos estáticos e usar recorte conservador do pacote quando o destino permanecer indeterminado. (`bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:34`) |
| Fixture JSON | Mudanças não-Python são descartadas. Incluir dependências de recursos carregados pelos testes; fallback por app consumidor. (`bin/vizinhos_da_volta.py:12`) |
| Template | Também descartado como arquivo tocado. Incluir dependências de templates, inclusões, herança e consumidores; fallback por app. (`bin/vizinhos_da_volta.py:12`) |
| Migration | Pode entrar como produção `.py`, mas só seleciona consumidores por citação literal. Propagar mudança de esquema aos testes do app e apps dependentes. (`bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:21`, `bin/vizinhos_da_volta.py:26`) |
| Selo que varre diretório/AST | A descoberta não relaciona escopo varrido com arquivos alterados. Registrar escopos de varredura e selecionar pela interseção com o diff. Alguns scanners estão cobertos pela lista fixa, mas isso não constitui regra geral. (`bin/vizinhos_da_volta.py:31`, `bin/vizinhos_da_volta.py:36`, `bin/selos_globais_da_volta.txt:23`, `bin/selos_globais_da_volta.txt:24`) |

O risco de cada lacuna é excluir um teste dependente do recorte; a ocorrência concreta de volta verde com teste externo quebrado é **[nao verificado]**. A possibilidade de omissão decorre dos filtros e da procura textual. (`bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:34`)

Há mais uma classe: descoberta de testes incompleta. Os candidatos exigem arquivo diretamente em diretório chamado `tests`, nome começando com `test`; isso deixa fora outras organizações e nomes de módulos. A regra mínima é usar a descoberta compatível com o executor, antes de calcular dependências. (`bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:16`)

P7, fonte única — **NAO PROCEDE que o selo atual detecte o TXT como repetição.** Ele busca linhas contendo `manage.py test`, `colaboradores` e `ponto`, nessa ordem, em `bin/` e `app/core/hooks`; o TXT contém labels individuais, sem esse comando. (`bin/tests/test_labels_fonte_unica.sh:38`, `bin/tests/test_labels_fonte_unica.sh:39`, `bin/tests/test_labels_fonte_unica.sh:40`, `bin/selos_globais_da_volta.txt:1`)

A lista integral continua sendo lida da régua; o TXT acrescenta uma seleção global manual distinta. Classificá-lo como duplicação da lista integral não é sustentado por esse fluxo. (`bin/suite.sh:99`, `bin/vizinhos_da_volta.py:36`)

Scanners globais efetivamente existentes percorrem diretórios independentemente de importações: o contrato de escritores usa `os.walk`, e o de leitores de vitalidade percorre arquivos e analisa AST. Ambos constam da lista fixa. (`app/core/tests/test_contract_escritores_estado.py:188`, `app/core/tests/test_contract_escritores_estado.py:190`, `app/chamados/tests/test_contract_leitores_vitalidade.py:292`, `app/chamados/tests/test_contract_leitores_vitalidade.py:302`, `bin/selos_globais_da_volta.txt:23`, `bin/selos_globais_da_volta.txt:24`)

## 11/11 06:34 · sessao 2a37b2983b12 · - · - · origem=Ronald (tmux)

**5. PROCEDE parcialmente: os três resultados existem, mas 75 encobre vermelho anterior.** `VOLTA_VERMELHA` é escrito se qualquer etapa falhar; `VOLTA_SEM_VEZ` também é escrito quando a suite retorna 75. A consolidação dá prioridade a 75, mesmo se ruff ou disjuntor já falharam. Portanto “75 = apenas não conseguiu a vez” não é garantido. (`bin/volta_intermediaria.sh:35`, `bin/volta_intermediaria.sh:36`, `bin/volta_intermediaria.sh:38`, `bin/volta_intermediaria.sh:39`)

- **`$TESTE_DOCKER`: correto na leitura estática.** O wrapper carrega `recursos.sh`; ali as flags são construídas e exportadas; a expansão sem aspas produz os argumentos das flags utilizadas no Docker. (`bin/volta_intermediaria.sh:15`, `bin/recursos.sh:81`, `bin/recursos.sh:82`, `bin/volta_intermediaria.sh:23`)
- **`PIPESTATUS`: correto para capturar o Docker/ruff.** A posição zero é lida imediatamente após `docker … | tail -15`; assim recebe o código do Docker, não o do `tail`. A falha exclusiva do `tail` não é incorporada a `ruff_rc`. (`bin/volta_intermediaria.sh:24`, `bin/volta_intermediaria.sh:25`)
- **`VOLTA_ESPERA`: chega à trava.** O wrapper transforma o ambiente, com default 3600, em `--espera`; suite armazena em `ESPERA` e repassa à trava; a trava usa em `flock -w`. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:53`, `bin/suite.sh:136`, `bin/trava_teste.sh:27`, `bin/trava_teste.sh:36`)
- **`VOLTA_PORQUE`: chega ao suite.** O wrapper fornece a variável no ambiente da chamada e suite a usa como destino de stderr do helper. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:85`)
- **Limite adicional:** a coleta inicial de `.py` não verifica o rc do `git diff`, embora `pipefail` esteja ativo; sem `errexit`, a atribuição pode falhar e o processamento continuar com `PY` vazio e `ruff_rc=0`. A etapa posterior do helper usa `check=True`, portanto falha de diff nessa etapa normalmente produz suite rc1, não verde. (`bin/volta_intermediaria.sh:10`, `bin/volta_intermediaria.sh:18`, `bin/volta_intermediaria.sh:21`, `bin/vizinhos_da_volta.py:10`, `bin/vizinhos_da_volta.py:11`, `bin/suite.sh:86`)
- **Execução real e permissões de escrita dos logs:** [nao verificado].

**6. Mapa de escritores e leitores.**

| Valor novo | Quem escreve | Quem lê |
|---|---|---|
| `MOTIVO` | Inicialização vazia; parser `--motivo`. O ambiente homônimo é sobrescrito. (`bin/suite.sh:47`, `bin/suite.sh:54`) | Guarda de formato; mensagem de recusa; saída seca. (`bin/suite.sh:70`, `bin/suite.sh:72`, `bin/suite.sh:91`) |
| `VOLTA` | Inicialização vazia; parser `--volta`. O wrapper fornece o valor posicional `BASE` como argumento. (`bin/suite.sh:47`, `bin/suite.sh:55`, `bin/volta_intermediaria.sh:13`, `bin/volta_intermediaria.sh:32`) | Guarda; ativação do recorte; argumento do helper; mensagem de falha; saída seca. (`bin/suite.sh:70`, `bin/suite.sh:82`, `bin/suite.sh:85`, `bin/suite.sh:86`, `bin/suite.sh:91`) |
| `SECO` | Inicialização zero; parser `--seco` escreve 1. (`bin/suite.sh:47`, `bin/suite.sh:56`) | Condicional que imprime e encerra antes de labels/env-file/montagem/trava/Docker. (`bin/suite.sh:90`, `bin/suite.sh:93`, `bin/suite.sh:99`, `bin/suite.sh:104`, `bin/suite.sh:117`, `bin/suite.sh:136`) |
| `VOLTA_PORQUE` | Wrapper define o caminho `"$OUT.porque"` no ambiente do subprocesso. Chamador externo também pode defini-lo: o consumidor lê o ambiente. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:85`) | Suite lê o caminho e abre/trunca o arquivo para stderr; helper escreve justificativas, produção tocada e total nesse stderr; wrapper lê o arquivo para contar labels. (`bin/suite.sh:85`, `bin/vizinhos_da_volta.py:40`, `bin/vizinhos_da_volta.py:41`, `bin/vizinhos_da_volta.py:42`, `bin/volta_intermediaria.sh:34`) |
| `selos_globais_da_volta.txt` | Lista literal versionada, 24 entradas; escritor automático não identificado no fluxo analisado. (`bin/selos_globais_da_volta.txt:1`, `bin/selos_globais_da_volta.txt:24`) | Helper lê o arquivo **ao lado dele próprio**, não dentro de `wt`; acrescenta cada entrada ao conjunto de saída e à justificativa. (`bin/vizinhos_da_volta.py:36`, `bin/vizinhos_da_volta.py:37`) |

`VOLTA_PORQUE` tem default `/dev/null`, portanto justificativas não são preservadas quando suite é chamado diretamente sem essa variável. (`bin/suite.sh:85`)

## 11/11 06:34 · sessao 015a5f061c14 · - · - · origem=Ronald (tmux)

**Pergunta 2 — PROCEDE: a documentação canônica quebra e o selo não acusa.**

| Chamador/referência | Efeito da guarda |
|---|---|
| `CLAUDE.md:96`: `bash ~/saas-hasner/bin/suite.sh` | Recusa rc 2, oferecendo `--volta` e `--only`, antes do env-file/docker (`bin/suite.sh:69-77`). |
| `bin/volta_intermediaria.sh:32` | Passa `--volta`, `--dir` e `--espera`; não exige motivo nessa modalidade (`bin/suite.sh:70`, `82-87`). |
| `bin/tests/test_suite_motivo.sh:24` | Executa cópia da porta; chamadas nuas são deliberadamente recusadas e válidas/`--only` devem chegar à trava falsa (`bin/tests/test_suite_motivo.sh:29-34`). |
| `app/core/placar_estrutural.py:109-110` | Publica instruções com `--only`; a guarda não as recusa (`bin/suite.sh:70`). É texto, não execução Python da suite (`app/core/placar_estrutural.py:109`). |
| Exemplos de uso de `bin/suite.sh:23-32` | `--motivo` válido, `--only` e `--volta` passam pela guarda; exemplos isolados `--dir`, `--parallel`, `--espera`, `-- --failfast`, ou `--seco` sem motivo/recorte são recusados (`bin/suite.sh:50-77`, `90-94`). |
| Receita histórica `app/docs/RELATO-ARQUIVO.md:37800` | Se colada hoje sem complemento, `--dir` sozinho é recusado (`bin/suite.sh:50`, `69-77`). |
| Orientação histórica `app/docs/PROMPTS.md:1069` e `app/docs/BACKLOG.md:1433`: suite por `--dir` | Se usada apenas assim, é recusada pela mesma guarda (`bin/suite.sh:69-77`). |

Não localizei outros chamadores executáveis de `suite.sh` em `.claude/` ou hooks; completude diante de chamadas construídas dinamicamente: **[nao verificado]**. `regua.sh` e `pre-push.sh` executam sua própria chamada, portanto esta guarda não os alcança (`bin/regua.sh:168`; `bin/pre-push.sh:186`; `app/core/hooks/pre-push.sh:82`; explicação em `CLAUDE.md:101-103`).

**O selo `test_suite_sh.sh` permanece logicamente verde:** cobra apenas presença textual de `bin/suite.sh` no bloco (`bin/tests/test_suite_sh.sh:31-42`) e strings de trava, `manage.py test`, leitura de LABELS e ausência de DB_PASSWORD (`bin/tests/test_suite_sh.sh:50-58`). Seu fixture “bom” é exatamente a chamada nua que agora é recusada (`bin/tests/test_suite_sh.sh:72-81`). Não executa esse comando; verde medido nesta revisão: **[nao verificado]**. Executá-lo criaria arquivos temporários, contrariando a regra de leitura (`bin/tests/test_suite_sh.sh:63-76`).

**Pergunta 7, fonte única — PROCEDE a cegueira do selo; não confundir com uma cópia literal de LABELS.**

`test_labels_fonte_unica.sh` não reconhece o novo `.txt` como lista repetida: seu detector exige a expressão `manage.py test … colaboradores … ponto` (`bin/tests/test_labels_fonte_unica.sh:38-46`); o arquivo novo contém somente módulos por linha (`bin/selos_globais_da_volta.txt:1-24`). O helper lê essa lista diretamente (`bin/vizinhos_da_volta.py:36-37`), enquanto a lista de apps permanece em `bin/regua.sh:23`. A lista fixa tem autorização no anexo fornecido; classificação disso como violação ampla da fonte única, além da cegueira demonstrada do selo: **[nao verificado]**.

## 11/11 06:34 · sessao e94ad08110dc · FIO-E0-V4-E4B · construtor · origem=Code

Cura aplicada: um lote existente cobre S143/S128 e permanece até o retorno do C7. [Fonte](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1923). [nao verificado]

Entregues o [diff](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/E4B_C4V5.patch) e o [recibo com comandos e RCs](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/E4B_C4V5_ENTREGA.txt:27):

- Sem banco: grupos de 50, 16 e 40 testes, todos rc=0. [nao verificado]
- RED sobre a base: rc=1, assertion `1 != 5`.
- Dois mutantes novos de tranca detectados; mutante do protocolo também falhou como esperado. [Evidências](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/E4B_C4V5_ENTREGA.txt:50).

Contagem esperada, **[nao verificado]**:

| Caso | Queries |
|---|---:|
| reconciliar | 60 |
| `_gravar_estado` sem pergunta | 28 |
| batida, 1 chamado fecha | 213 |
| batida, 3 chamados fecham | 269 |

Tetos 213/269 preservados; o custo `+N _ancoradas` continua como pergunta de desenho no [C8](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/tests/test_selo_chokepoint_escrita.py:189).

O [RED ORM pareado e seu mutante](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:34) estão preparados para o Code. Execução ORM e certificação final: **[nao verificado]**.

_3682 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
