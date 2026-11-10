# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 08:46** · publicado em **10/11/2026 08:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 08:46 · sessao 42f15a7e4013 · - · - · origem=Ronald (tmux)

Mapa dos campos:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `fora_do_universo` | `app/ponto/services/cluster_espurio.py:109-110`, devolvido em `:148` | Serialização integral em `app/ponto/management/commands/detectar_cluster_espurio.py:63`; contratos em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:222-225`, `:254-260`. |
| `excluidos` | `app/ponto/services/cluster_espurio.py:126-145`, devolvido em `:148` | Serialização em `app/ponto/management/commands/detectar_cluster_espurio.py:63`; contratos em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:229-247`, `:362-366`. |
| `numero` | Soma por chamado em `app/ponto/services/cluster_espurio.py:125`, devolvido em `:146` | JSON em `app/ponto/management/commands/detectar_cluster_espurio.py:63`; decisão do alarme e mensagem em `:70-73`; contratos exemplares em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:302`, `:315`, `:348-349`. |
| `esperado` | Constante zero em `app/ponto/services/cluster_espurio.py:146` | JSON e comparação em `app/ponto/management/commands/detectar_cluster_espurio.py:63`, `:70-73`; contratos em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:227`, `:356`; fixture sintética de valor 1 em `:416-421`. |

A semântica é coerente nos leitores encontrados: `excluidos` é catálogo de motivos, inclusive sem ocorrências; `fora_do_universo` contém quantidades observadas por via; `numero` conta chamados com assinatura, preservando duplicatas do mesmo par; `esperado` é o alvo do alarme (`app/ponto/services/cluster_espurio.py:109-148`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233-247`, `:342-349`; `app/ponto/management/commands/detectar_cluster_espurio.py:70`). Não encontrei outro consumidor produtivo desse retorno na busca integral da cópia; consumidores externos são **[nao verificado]**.

`None` e `''` **não quebram o `json.dumps` atual**. Executei, em memória, o serviço e o comando reais pelos helpers: `{None: 1, '': 1}` saiu como `{"null": 1, "": 1}`; o rótulo de `None` continua sob a chave textual `"null"` (`app/ponto/services/cluster_espurio.py:141-148`; `app/ponto/management/commands/detectar_cluster_espurio.py:63`; helpers em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:150-166`).

Há uma limitação reproduzida: se existir também a via desconhecida literal `'null'`, ela colide no JSON com a chave Python `None`. Reproduzi fonte `{None: 2, '': 1, 'null': 3}` → JSON com duas propriedades `"null"` → `json.loads` preserva apenas `3`; os rótulos também colidem. O código admite valores desconhecidos e não normaliza as chaves (`app/ponto/services/cluster_espurio.py:110`, `:142-145`; `app/ponto/management/commands/detectar_cluster_espurio.py:63`). Ocorrência de `'null'` e leitor externo afetado: **[nao verificado]**.

O cron configurado chama `--alarme`; o envelope lê o código de saída, sem interpretar esses campos JSON (`app/config/crons.py:598`; `bin/cron_run.sh:11-13`, `:22-25`). Portanto não há quebra demonstrada desse leitor existente; a colisão é uma limitação do formato de saída, condicionada a um valor que o dado fornecido não comprova.

## 10/11 08:45 · sessao db4c347226f8 · - · - · origem=Ronald (tmux)

Conferência dos escritores:

| Via/rótulo | Escritor real e comparação |
|---|---|
| `TRANSICAO_HUMANA` → “Via humana” | `transicionar` valida a transição e chama `_aplicar`; este grava `EVENTO_VIA['TRANSICAO_HUMANA']`. O backfill também atribui `admin` a resolvido/fechado com respondente fora dos usuários de sistema. Rótulo compatível; não prova passagem histórica por `transicionar`. (`app/chamados/models.py:128`, `app/chamados/models.py:199`, `app/chamados/models.py:447`, `app/chamados/migrations/0039_a3_via_backfill.py:26`) |
| `ENCERRAR` → “desligamento ou ajuste retroativo” | O gate documenta ambos e grava `EVENTO_VIA['ENCERRAR']`. Há chamadores concretos para desligamento e ajuste retroativo. (`app/chamados/models.py:1605`, `app/chamados/models.py:1619`, `app/colaboradores/services/desligamento.py:155`, `app/ponto/services/justificativa.py:400`, `app/chamados/reconciliador.py:496`) |
| `CANCELAR` → “Cancelamento de chamado vivo…” | O gate descreve descarte administrativo/emissor aposentado; o motor permite CANCELAR apenas de aberto/em_análise/registrado. O backfill carimba `cancelado` pela condição status cancelado + via vazia, sem provar motivo histórico. (`app/chamados/models.py:773`, `app/chamados/models.py:783`, `app/chamados/models.py:788`, `app/chamados/catalogo/motor.py:116`, `app/chamados/migrations/0039_a3_via_backfill.py:25`) |
| `SUPERAR` → “Duplicata cancelada com irmão único provado no backfill” | A migration seleciona cancelados de módulos de furo, exige irmão único do mesmo colaborador/data e grava `superado` com FK. O gate atual foi removido; o estado permanece para o acervo. Rótulo fiel ao escritor remanescente. (`app/chamados/migrations/0039_a3_via_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:20`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/models.py:794`) |
| `VIA_LEGADO` → “Terminal sem via derivável no backfill” | Após as derivações anteriores, a migration grava `legado` nos terminais ainda com via vazia. Rótulo fiel. (`app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:28`) |
| Competência trancada | O serviço aplica apenas se medição declarou trancada e chama `encerrar_por_competencia_trancada`; o gate grava a constante própria pelo núcleo. (`app/chamados/services/competencia_trancada.py:167`, `app/chamados/services/competencia_trancada.py:184`, `app/chamados/models.py:1639`) |
| `''` / `None` | São explicitamente rotulados como vazios; ocorrência persistida de `None` não está demonstrada. (`app/ponto/services/cluster_espurio.py:141`, `app/ponto/services/cluster_espurio.py:142`; ocorrência: [nao verificado]) |

Nenhuma via conhecida excluída cai em “desconhecida”: as seis vias diferentes de RECONCILIAR constam nominalmente no dicionário; o fallback usa `setdefault`. RECONCILIAR pertence ao universo contado. (`app/chamados/catalogo/motor.py:88`, `app/chamados/catalogo/motor.py:93`, `app/chamados/catalogo/motor.py:95`, `app/ponto/services/cluster_espurio.py:110`, `app/ponto/services/cluster_espurio.py:111`, `app/ponto/services/cluster_espurio.py:126`, `app/ponto/services/cluster_espurio.py:144`)

Ressalva de precisão: os textos descrevem classes de via; os escritores não comprovam, individualmente, ADMIN/DP, premissa administrativa ou motivo histórico de cada registro. CANCELAR recebe `motivo` livre; ENCERRAR não recebe motivo; o backfill usa heurísticas de campos. (`app/chamados/models.py:773`, `app/chamados/models.py:789`, `app/chamados/models.py:1605`, `app/chamados/migrations/0039_a3_via_backfill.py:25`, `app/chamados/migrations/0039_a3_via_backfill.py:26`)

O rótulo ENCERRAR tampouco esgota todos os usos globais do gate: troca de escala encerra chamados `escala_aceite`, e regularização externa encerra seu chamado. Esses usos não demonstram exclusão incorretamente rotulada no módulo cluster: regularização cria módulo próprio; troca de escala filtra categoria. (`app/colaboradores/services/vinculo.py:384`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/services/regularizacao_ext.py:205`, `app/chamados/services/regularizacao_ext.py:250`)

## 10/11 08:45 · sessao 2a7073c850aa · - · - · origem=Ronald (tmux)

B: **os três mutantes morrem pelas asserções pretendidas**, executando a fonte real alterada apenas em memória. O carregador lê/compila a fonte; serviço e comando são carregados inteiros; juízes puros vêm da fonte real. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:31`, `:104`, `:108`, `:130`)

| Mutante | Falha observada na reexecução |
|---|---|
| FK cruzada: remove guarda de colaborador | `1 != 0` e `0 != 1`, ambas na asserção de `numero`; a FK cruzada tem assinatura inversa à célula correta. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:63`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:299`, `:302`) |
| Consulta por par | `9 != 3` na contagem de avaliações; a asserção anterior de `numero` passa. Há três colaboradores, três datas por colaborador e consultas vazias também são contadas. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:66`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:83`, `:307`, `:313`, `:315`, `:316`) |
| Rótulos humana/cascata trocados | Falha na igualdade exata de `excluidos`, independentemente de ocorrências. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:74`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233`, `:247`) |

Executei `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_o169_cluster_encerrado_sem_lastro ponto.tests.test_o169_cluster_encerrado_mutantes -v`: **61 testes OK, incluindo os 35 mutantes listados; nenhum desses 35 sobreviveu**. Resultado desta execução [nao verificado por artefato persistido]; definição/coleta dos mutantes em `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13`, `:150`.

O harness exige alvo textual único, rejeita erros de infraestrutura e exige falha de asserção; nos três casos acima confirmei também a linha exata que falhou. (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:135`, `:141`, `:142`)

Limite: a contagem observa avaliações do dublê, inclusive resultado vazio; SQL real, banco, gates de escrita e produção permanecem [nao verificado]. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:7`, `:81`, `:158`)

## 10/11 03:16 · sessao 3f625a779cd6 · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · construtor · origem=Code

Corrigido na origem: o ramo sem SHA retorna `sem-commit:<arquivo>` ([esteira_vigia.py:1174](core/esteira_vigia.py:1174)).

O [relatório de entrega](pautas/tests/prefixos_reservados_volta5.r:5) contém o censo de quem escreve/lê cada campo, arquivos tocados, comandos, saídas e comando individual dos mutantes.

Resultados locais registrados:

- RED nas bases `5b6861f6f` e `2c998ff23`, ambos com zero erros ([saída](pautas/tests/prefixos_reservados_volta5.r:67)).
- Prefixos: 30 testes, 1 skip; core/zumbido: 11 testes OK ([saída](pautas/tests/prefixos_reservados_volta5.r:89)).
- **88 mutantes mortos; zero sobreviventes**, incluindo chamada nova a `_pauta` sem cobertura ([saída](pautas/tests/prefixos_reservados_volta5.r:112)).
- 2.205 rótulos reais válidos; caminho com espaço recusado, com stderr registrado ([saída](pautas/tests/prefixos_reservados_volta5.r:353)).

Ruff ausente ([saída](pautas/tests/prefixos_reservados_volta5.r:108)). Integração Django/HTTP/banco e suite completa: **[nao verificado]**. Certificação permanece com Code e opus final.

## 10/11 03:04 · sessao d0f364fe0074 · O169-FECHADA · construtor · origem=Code

Cura entregue em três arquivos:

- [cluster_espurio.py:126](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:126): rótulos por via conhecida, cascata com “desligamento ou ajuste retroativo”, vazias nomeadas e desconhecida reservada às demais vias. `numero`, `esperado` e `universo` preservados.
- [test_o169_cluster_encerrado_sem_lastro.py:233](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233): contratos de rótulo exato, FK cruzada e K consultas para K colaboradores com vários pares.
- [test_o169_cluster_encerrado_mutantes.py:63](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:63): três mutantes exigidos e seis adicionais para os rótulos.

Saída local: **61 testes OK; 35 mutantes executados individualmente, todos mordidos por asserção**. O harness rejeita erros de infraestrutura como prova. ([mutantes:131](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:131)) [nao verificado]

O RED de rótulos retornou `FAILED (failures=1)` antes da cura; depois passou. Comandos dos contratos: ([contrato:233](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:233)) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_o169_cluster_encerrado_sem_lastro.ClusterEncerradoSemLastroTest.test_RED_rotulo_fiel_por_via_exata -v

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_o169_cluster_encerrado_sem_lastro ponto.tests.test_o169_cluster_encerrado_mutantes -v
```

Para emitir e executar o comando de **cada mutante**, individualmente, usando a lista entregue em [MUTANTES:13](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import subprocess
from ponto.tests.test_o169_cluster_encerrado_mutantes import MUTANTES

prefixo = 'ponto.tests.test_o169_cluster_encerrado_mutantes.MutantesO169Test.test_mutante_'
for nome in MUTANTES:
    comando = ['python3', '-m', 'unittest', prefixo + nome, '-v']
    print('PYTHONDONTWRITEBYTECODE=1 ' + ' '.join(comando), flush=True)
    subprocess.run(comando, check=True)
PY
```

Ruff sobre todo `app/`: `All checks passed!`, com as regras de [ruff.toml:4](/home/ronald/wt-codex-o169-fechada-025847/app/ruff.toml:4). Comando executado:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache .
```

Quem escreve e quem lê os campos usados pelo contador:

| Campos | Escritor | Leitor |
|---|---|---|
| `status_local`, `via_encerramento` | [_gravar_estado:228](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:228), [via:245](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:245); escritores por via citados em [cluster:127](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:127) | [contador:106](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:106) |
| Chamado: `colaborador_id`, `modulo_origem`, `contexto_json`, `celula_id` | [abrir:958](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:958), [persistência:971](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/models.py:971) | [contador:106](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:106), [dia_do_fato:98](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/juizes.py:98) |
| Célula: `colaborador_id`, `data`, `trabalha` | [geração:259](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/portas/celula.py:259), [edição:600](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/portas/celula.py:600) | [contador:117](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:117), [dia_sem_trabalho:426](/home/ronald/wt-codex-o169-fechada-025847/app/chamados/juizes.py:426) |
| `ata`: órfãs, lâmpadas e tipo/cobertura do dia | [ata_do_dia:704](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cartorio.py:704), [lavra:1042](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/portas/celula.py:1042) | [_leitura_da_celula:25](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:25) |
| `nome`, `numero`, `esperado`, `dono`, `universo`, `fora_do_universo`, `excluidos` | [contador:146](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/services/cluster_espurio.py:146), em memória | [comando:63](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/management/commands/detectar_cluster_espurio.py:63); alarme lê `numero != esperado` em [comando:70](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/management/commands/detectar_cluster_espurio.py:70) |
| `consultas_celulas`, no teste | [dublê:83](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:83), inclusive consulta vazia | [contrato:316](/home/ronald/wt-codex-o169-fechada-025847/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:316) | [nao verificado]

Não executados: banco/SQL, produção, concorrência, medição de frota/cron e certificação Code/opus. Esses resultados permanecem **[nao verificado]**.

## 10/11 02:58 · sessao b27ce983d57b · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · construtor · origem=Code

**Parei conforme o contrato: há um ramo que produz chave inválida.** Sem SHA, `dono_do_teste` retorna `sem commit (<arquivo>)`; esse valor compõe `quarentena:zumbido:<dono>`, contendo espaços (`core/esteira_vigia.py:1174`, `core/esteira_vigia.py:1212`). Ocorrência em produção: **[nao verificado]**.

As alterações ficaram **parciais**:

- [marcadores.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-025046/app/pautas/marcadores.py:31): gramática única e reserva derivada.
- [alarme_esteira.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-025046/app/core/management/commands/alarme_esteira.py:29): validação antes de consultar, escrever ou fechar.
- [test_prefixos_reservados.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-025046/app/pautas/tests/test_prefixos_reservados.py:298): equivalência e recusas.
- [test_prefixos_reservados_ast.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-025046/app/pautas/tests/test_prefixos_reservados_ast.py:194): selo contra regex paralela.
- [mutar_prefixos_reservados.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-025046/app/pautas/tests/mutar_prefixos_reservados.py:103): novos mutantes e reprodução por hash.

O teste de censo cobre as ações de `decidir`, mas faltam as chamadas diretas a `_pauta`; portanto os resultados abaixo **não demonstram o censo completo** (`pautas/tests/test_prefixos_reservados.py:266`; `core/esteira_vigia.py:1212`).

Comandos executados e resultados parciais — RED reproduzível pelo executor, incluindo falha de equivalência (`pautas/tests/mutar_prefixos_reservados.py:261`; `pautas/tests/test_prefixos_reservados.py:313`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados --base 5b6861f6f7c9d0f5ac85dc935dac9b2235f024e4
# 11 testes; 93 falhas; 0 erros; rc 1 [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest pautas.tests.test_prefixos_reservados pautas.tests.test_prefixos_reservados_ast
# 25 testes; OK; 1 skip [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados
# 79 mortos; 0 sobreviventes
```

Cada mutante tem comando individual impresso pelo executor (`pautas/tests/mutar_prefixos_reservados.py:243`).

Ruff e testes legados de alarme não iniciaram: módulos `ruff` e `django` ausentes. Ruff limpo e integração HTTP/banco: **[nao verificado]**. **Entrega interrompida, sem certificação.** [nao verificado]

## 10/11 02:54 · sessao 38d53a47ac9d · O169-FECHADA · verificador · origem=Code

1. **P1: cobre todas as vias em estado estável; a igualdade não é garantida sob concorrência.** O `Counter` recebe todos os encerrados do módulo, e somente RECONCILIAR sai de `fora_do_universo`; `''` e `None` permanecem. O filtro `colaborador_id` vale para ambas as consultas: nesse caso, a igualdade compara os encerrados **daquele colaborador**. ([cluster_espurio.py:106](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:106))

   As partes vêm de dois SELECTs separados. Um registro lido como `legado` no primeiro e `sistema` no segundo pode aparecer nas duas partes: soma 2 para um encerrado. O contador não estabelece uma fotografia comum; ocorrência real desse cenário: **[nao verificado]**. ([cluster_espurio.py:109](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:109), [cluster_espurio.py:112](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:112))

   Os três rótulos fixos correspondem às constantes. Há duas ressalvas textuais: qualquer via adicional recebe “desconhecida (backfill)”, sem comprovar sua origem; `cascade` recebe “desligamento”, embora o gate também contemple ajuste retroativo. Registro concreto de O169 malrotulado: **[nao verificado]**. ([cluster_espurio.py:126](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:126), [motor.py:88](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/chamados/catalogo/motor.py:88), [models.py:1605](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/chamados/models.py:1605))

2. **P2: a FK reaproveitada corresponde ao par escolhido pelo juiz**, em estado estável com a unicidade declarada aplicada: o código exige mesmo colaborador/data, e `CelulaDia` declara esse par único. ([cluster_espurio.py:117](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:117), [models.py:1833](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/escala/models.py:1833))

   **Outro dia no JSON não aciona fallback:** `dia_do_fato` escolhe primeiro a data da própria FK. **Outro colaborador aciona fallback**, que consulta o colaborador do chamado **na data da FK**. Portanto, ele corrige a pessoa consultada; não recupera a data do contexto divergente. ([juizes.py:98](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/chamados/juizes.py:98), [cluster_espurio.py:113](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:113), [cluster_espurio.py:120](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:120))

   O contador faz **2 + K consultas**, com K colaboradores que tenham pares faltantes: no máximo uma consulta adicional por colaborador; com `colaborador_id`, no máximo três no total. Isso não inclui o restante do comando. O cron está configurado para 06:24 com `--alarme`, sem filtro de colaborador; aceitabilidade do custo novo: **[nao verificado]**. ([cluster_espurio.py:109](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:109), [cluster_espurio.py:122](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:122), [crons.py:598](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/config/crons.py:598))

3. **Quem escreve e lê:** o contador constrói os dois dicionários em memória. O comando imprime ambos no JSON integral; “universo vazio” considera somente `universo.total`; `--alarme` considera pendências e `numero != esperado`, **não** os dois dicionários. ([cluster_espurio.py:110](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:110), [cluster_espurio.py:126](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:126), [detectar_cluster_espurio.py:62](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/management/commands/detectar_cluster_espurio.py:62), [detectar_cluster_espurio.py:70](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/management/commands/detectar_cluster_espurio.py:70))

   Os leitores de teste verificam contagens e rótulos; esperar somente três exclusões ocorre no cenário sem vias extras e é compatível com o serviço. Não identifiquei leitor com interpretação incompatível na cópia. ([test_o169_cluster_encerrado_sem_lastro.py:218](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:218), [test_o169_cluster_encerrado_sem_lastro.py:313](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:313))

4. **Os 26 declarados mordem a fonte real, executada com dependências dubladas.** O harness lê e altera serviço/comando em memória, exige alvo único e falha de asserção, rejeitando erro de infraestrutura. Reexecutei: **49 testes OK, incluindo os 26 mutantes mortos**. SQL e persistência não são exercitados. ([test_o169_cluster_encerrado_mutantes.py:99](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:99), [test_o169_cluster_encerrado_sem_lastro.py:100](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:100))

   **Há sobreviventes adicionais de P1/P2**, ensaiados somente em memória: remover a guarda do colaborador da FK; consultar por par em vez de agrupar por colaborador; trocar os rótulos humana/cascata. Os contratos não montam FK cruzada, registram linhas retornadas em vez de quantidade de consultas e não exigem associação exata dos rótulos conhecidos. Esses três não pertencem aos 26 declarados. ([cluster_espurio.py:117](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:117), [test_o169_cluster_encerrado_sem_lastro.py:80](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:80), [test_o169_cluster_encerrado_sem_lastro.py:253](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:253), [test_o169_cluster_encerrado_sem_lastro.py:315](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:315))

5. **Não identifiquei regressão determinística de `numero` em estado estável e íntegro.** Continua selecionando encerrados por RECONCILIAR, usando o dia do juiz e contando chamados, inclusive vários do mesmo par. As outras vias ganharam contagem informativa, permanecendo fora de `numero`; o reaproveitamento da FK preserva a célula pelo par único. Equivalência durante alterações concorrentes e impacto real no cron: **[nao verificado]**. ([cluster_espurio.py:110](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:110), [cluster_espurio.py:125](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/ponto/services/cluster_espurio.py:125), [models.py:1833](/home/ronald/codex_ws/0ed50976462ab65fffc0d0b322e3d9aa5de1be52/app/escala/models.py:1833))

## 10/11 02:52 · sessao c86210e57c9f · - · - · origem=Ronald (tmux)

P1 e leitores, somente código da cópia:

- **Estado estável:** todas as vias diferentes de RECONCILIAR entram em `fora_do_universo`, incluindo `''` e `None`; `colaborador_id` aplica-se às duas consultas. Assim, a soma das partes coincide com todos os encerrados **do mesmo recorte por colaborador**, em estado estável. (`app/ponto/services/cluster_espurio.py:106`, `:109`, `:110`, `:112`, `:134`; teste: `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:204`, `:221`, `:229`.)
- **Concorrência:** a invariância não é garantida incondicionalmente: as partes vêm de dois SELECTs separados, sem snapshot comum estabelecido pelo contador. Reproduzi em memória usando a função real: um registro `legado` na primeira leitura e `sistema` na segunda produziu `universo=1`, `fora={'legado':1}`, soma=2 para um registro encerrado. Isso ocorre também com `colaborador_id=1`. Concorrência real de produção `[nao verificado]`. (`app/ponto/services/cluster_espurio.py:109`, `:112`.)
- **NULL:** o código cobre `None`; o teste injeta esse valor explicitamente. Entretanto, o campo do modelo não declara `null=True`; ocorrência SQL NULL em produção `[nao verificado]`. (`app/ponto/services/cluster_espurio.py:110`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:214`; `app/chamados/models.py:54`.)
- **Rótulos:** as três chaves fixas são `admin`, `cascade`, `competencia_trancada` e correspondem às constantes; legado/vazia/None recebem o texto exigido pela triagem. Não identifiquei valor concreto do módulo com rótulo errado. (`app/chamados/catalogo/motor.py:88`, `:92`, `:95`; `app/ponto/services/cluster_espurio.py:126`, `:132`.)
- **Ressalva ao rótulo cascade:** o gate documenta “desligamento, ajuste retroativo”, portanto o texto “Cascata de desligamento” é mais estreito que o uso geral do gate. Os chamadores retroativos inspecionados filtram outros módulos, então não há demonstração de erro concreto em O169. (`app/chamados/models.py:1605`; `app/ponto/services/justificativa.py:55`, `:60`, `:394`, `:400`.)
- **Escritor dos campos de saída:** apenas o contador constrói e retorna os dois dicionários; são dados em memória. (`app/ponto/services/cluster_espurio.py:110`, `:126`, `:132`, `:135`.)
- **Leitor de produção:** `detectar_cluster_espurio` imprime o contador completo em JSON, portanto imprime ambos. A mensagem “universo vazio” lê exclusivamente `universo.total`; `--alarme` considera exclusivamente pendências e `numero != esperado`, sem considerar `fora_do_universo` ou `excluidos`. (`app/ponto/management/commands/detectar_cluster_espurio.py:62`, `:63`, `:64`, `:70`.)
- **Leitores de teste:** verificam contagens, partição, presença de rótulos e rótulos desconhecidos; outro teste espera só as três exclusões fixas quando nenhum registro de via desconhecida foi alimentado. Isso é compatível com o `setdefault` atual. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:218`, `:221`, `:225`, `:227`, `:234`, `:240`, `:313`; `app/ponto/services/cluster_espurio.py:131`.)
- Não encontrei outro consumidor do contador nem leitor que atribua significado incompatível aos campos na cópia. Os consumidores identificados são o comando e os contratos mencionados. (`app/ponto/management/commands/detectar_cluster_espurio.py:62`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:150`.)

## 10/11 02:52 · sessao 3dea71adcda2 · - · - · origem=Ronald (tmux)

P2 está correto para o par definido pelo juiz, com limites:

- A FK reaproveitada precisa coincidir em colaborador e data; `CelulaDia` declara unicidade desse par, também na migração. Portanto, num estado estático com esquema íntegro, é a mesma linha que a consulta devolveria. (`app/ponto/services/cluster_espurio.py:117`; `app/escala/models.py:1833`; `app/escala/migrations/0030_celuladia.py:35`.)
- **Dia divergente do contexto não aciona fallback:** `dia_do_fato` retorna primeiro a data da própria FK. Assim, a comparação de data no reaproveitamento já nasce satisfeita; isso é preexistente ao delta. (`app/chamados/juizes.py:98`; `app/ponto/services/cluster_espurio.py:113`; `app/ponto/services/cluster_espurio.py:117`.)
- **FK de outro colaborador:** é rejeitada; fallback procura o colaborador do chamado **na data da FK**, não na data do contexto. Isso respeita o juiz atual, mas não corrige eventual vínculo com dia errado. (`app/ponto/services/cluster_espurio.py:113`; `app/ponto/services/cluster_espurio.py:117`; `app/ponto/services/cluster_espurio.py:124`; `app/chamados/juizes.py:98`.)
- Consultas lógicas do contador: **2 + K**, sendo K o número de colaboradores com pelo menos um par não atendido pelas FKs aproveitadas. Por colaborador há no máximo uma consulta de fallback; com `colaborador_id`, no máximo três consultas no total. Não há query por chamado no juiz forte. (`app/ponto/services/cluster_espurio.py:109`; `app/ponto/services/cluster_espurio.py:112`; `app/ponto/services/cluster_espurio.py:122`; `app/chamados/juizes.py:127`.)
- Cron real configurado para **06:24, `--alarme`, sem filtro de colaborador**; o piso declarado é 30 segundos. Aceitabilidade do custo novo no cron: **[nao verificado]**. (`app/config/crons.py:598`; `app/config/crons.py:136`.)

Há uma lacuna material na cobertura de P2: removi **somente em memória** a guarda `and ch.celula.colaborador_id == par[0]`; o mutante sobreviveu aos **23 contratos**. O teste de FK usa o mesmo colaborador, enquanto o teste de colaboradores distintos usa chamados sem FK. (`app/ponto/services/cluster_espurio.py:117`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:253`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:283`.)

Contraexemplo executado em memória: chamado do colab1 ligado à célula assinada do colab2; célula própria do colab1 sem assinatura no mesmo dia. Código atual retorna `numero=0` e consulta o par próprio; o mutante retorna `numero=1`, sem fallback. A guarda atual evita esse erro, mas nenhum dos 26 mutantes declarados a remove. (`app/ponto/services/cluster_espurio.py:117`; `app/ponto/services/cluster_espurio.py:125`; `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13`.)

Quanto à contagem, não identifiquei regressão determinística em estado estático íntegro: permanece a leitura por par do juiz e uma contagem por chamado. A troca altera o momento da leitura de células reaproveitadas e o número de consultas; equivalência sob alterações concorrentes e impacto real de desempenho são **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:112`; `app/ponto/services/cluster_espurio.py:118`; `app/ponto/services/cluster_espurio.py:125`.)

## 10/11 02:52 · sessao 5d4004847511 · - · - · origem=Ronald (tmux)

Os 26 mutantes declarados mordem a fonte real: leem serviço/comando da cópia, exigem trecho único, substituem somente em memória e exigem falha de asserção, rejeitando erros de infraestrutura (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:99`, `:100`, `:103`, `:106`, `:107`). O carregador compila e executa essa fonte; ORM e funções auxiliares do comando são dublados (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:31`, `:41`, `:107`, `:112`, `:114`, `:122`).

Executei os dois módulos com `PYTHONDONTWRITEBYTECODE=1`: **49 testes OK**, incluindo os 26 mutantes; nenhum listado sobreviveu. Resultado sem arquivo persistido: `[nao verificado]`. [nao verificado]

Há, porém, **três mutantes adicionais sobreviventes** em memória, cada qual passando os 23 contratos sem erro/falha:

- **P2: remover a guarda do colaborador da FK.** A guarda está em `app/ponto/services/cluster_espurio.py:117`. O único teste de reaproveitamento monta FK da pessoa correta (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:253`); o teste de pessoas diferentes não monta FK (`:283`). Prova específica: chamado da pessoa 1 com FK assinada da pessoa 2 e célula correta sem assinatura: original retorna 0 e consulta célula correta; mutante retorna 1 sem fallback. Execução em memória: `[nao verificado]`.
- **P2: consultar uma vez por par, em vez de agrupar por colaborador.** A mudança cabe no laço `app/ponto/services/cluster_espurio.py:122`; o duplo registra linhas retornadas, não número de consultas (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:80`). Os testes não distinguem as duas estratégias (`:242`, `:263`). Execução em memória: `[nao verificado]`.
- **P1: trocar os rótulos humana/cascata.** Os rótulos ficam em `app/ponto/services/cluster_espurio.py:127`; os testes verificam presença e termos genéricos dos conhecidos, sem associar texto exato à via (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:225`, `:313`, `:315`). Execução em memória: `[nao verificado]`.

Não confundir: esses três **não pertencem aos 26 declarados**; demonstram lacunas além do catálogo atual (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:13`).

## 10/11 02:49 · sessao f4a1dda2ec83 · O169-FECHADA · construtor · origem=Code

Implementei os dois procedentes. Arquivos alterados:

- [cluster_espurio.py:88](/home/ronald/wt-codex-o169-fechada-024149/app/ponto/services/cluster_espurio.py:88): publica contagens e rótulos de todas as vias externas ao universo; consulta células pelos pares pedidos e reaproveita a FK carregada. O cálculo de `numero` continua usando `_leitura_da_celula`. (`ponto/services/cluster_espurio.py:109`, `:117`, `:123`, `:125`.)
- [test_o169_cluster_encerrado_sem_lastro.py:204](/home/ronald/wt-codex-o169-fechada-024149/app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:204): cobertura da soma das partes, vias desconhecidas, pares cruzados e reaproveitamento da FK.
- [test_o169_cluster_encerrado_mutantes.py:44](/home/ronald/wt-codex-o169-fechada-024149/app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:44): nove mutantes novos; o harness exige falha de asserção e rejeita erro de infraestrutura. (`ponto/tests/test_o169_cluster_encerrado_mutantes.py:106`.)

Referências abaixo relativas a `app/`.

| Campo lido ou produzido | Quem escreve | Quem lê |
|---|---|---|
| `Chamado.status_local` | Nascimento: `chamados/models.py:938`, `:971`; transições: `:228` | Contador: `ponto/services/cluster_espurio.py:106` |
| `Chamado.via_encerramento` | Núcleo: `chamados/models.py:245`, `:249`; gates humano/reconciliar/cascade/tranca: `:450`, `:654`, `:1619`, `:1639`; backfill legado: `chamados/migrations/0039_a3_via_backfill.py:28` | Contagem e seleção: `ponto/services/cluster_espurio.py:109`, `:111` |
| `Chamado.modulo_origem`, `colaborador_id` | Emissor: `ponto/services/cluster_espurio.py:181`; persistência: `chamados/models.py:971` | Filtros e pares: `ponto/services/cluster_espurio.py:106`, `:108`, `:113` |
| `Chamado.celula_id` / `celula` | Nascimento: `chamados/models.py:958`; vínculo posterior: `chamados/services/vinculos.py:99` | Dia: `chamados/juizes.py:98`; reaproveitamento: `ponto/services/cluster_espurio.py:117` |
| `contexto_json.data_turno` | Emissor: `ponto/services/cluster_espurio.py:195`; porta de contexto: `chamados/services/contexto.py:76`, `:102`, `:125`, `:161` | Juiz chamado pelo contador: `chamados/juizes.py:100`, `:114`, `:118` |
| `CelulaDia.colaborador_id`, `data`, `trabalha` | Porta: `ponto/portas/celula.py:259`, `:272`, `:600`, `:603` | Pares: `ponto/services/cluster_espurio.py:117`, `:123`; trabalho: `chamados/juizes.py:441` |
| `ata.orfas`, `lampadas[].acesa`, `tipo_dia`, `cobertura_parcial` | Construção: `ponto/services/cartorio.py:689`, `:704`, `:711`, `:733`; persistência: `ponto/portas/celula.py:1042` | Predicados compartilhados: `ponto/services/cluster_espurio.py:25`, `:29`, `:32`; `chamados/juizes.py:435` |
| `nome`, `numero`, `esperado`, `dono`, `universo`, `fora_do_universo`, `excluidos` | Retorno do contador: `ponto/services/cluster_espurio.py:125`, `:126`, `:133` | Impressão integral: `ponto/management/commands/detectar_cluster_espurio.py:63`; universo vazio: `:64`; alarme: `:70`; contratos: `ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:218`, `:221` |

Os três REDs executados contra a base falharam por asserção: ausência de `fora_do_universo`, **4 células lidas em vez de 2** e consulta repetida da FK. (`ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:218`, `:250`, `:261`.) Comando reproduzível, sem reescrever arquivos:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import subprocess
import unittest
from ponto.tests.test_o169_cluster_encerrado_sem_lastro import ClusterEncerradoSemLastroTest as C

fonte = subprocess.check_output([
    'git', 'show',
    '39b28d8e30a5d3fc1b2519cad47f22f71c01ee22:app/ponto/services/cluster_espurio.py',
], text=True)
B = type('ContratoNaBase', (C,), {'fonte_servico': fonte})
nomes = [
    'test_RED_toda_via_fora_do_universo_contada_e_rotulada',
    'test_RED_celulas_lidas_so_dos_pares_pedidos',
    'test_RED_reaproveita_fk_carregada_sem_buscar_o_mesmo_par',
]
r = unittest.TextTestRunner(verbosity=2).run(
    unittest.TestSuite(B(n) for n in nomes))
raise SystemExit(not r.wasSuccessful())
PY
```

Saída: `Ran 3 tests — FAILED (failures=3)`, rc **1**.

Após a cura, os dois módulos passaram: **49 testes**, incluindo **26 mutantes mordidos**; também passaram com `PYTHONHASHSEED=1` e `2`. Os contratos estão em `ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:96`; os mutantes, em `ponto/tests/test_o169_cluster_encerrado_mutantes.py:13`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_o169_cluster_encerrado_sem_lastro \
  ponto.tests.test_o169_cluster_encerrado_mutantes -q
```

Saída: `Ran 49 tests — OK`, rc **0**.

Executei também cada mutante individualmente. Este comando imprime e executa o comando de cada um, incluindo `legado_nao_contado`, `produto_cartesiano` e os dois de recarga da FK. (`ponto/tests/test_o169_cluster_encerrado_mutantes.py:44`, `:63`, `:70`, `:73`.)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import shlex
import subprocess
import sys
from ponto.tests.test_o169_cluster_encerrado_mutantes import MUTANTES

for nome in MUTANTES:
    cmd = [sys.executable, '-m', 'unittest',
           'ponto.tests.test_o169_cluster_encerrado_mutantes.'
           'MutantesO169Test.test_mutante_' + nome, '-q']
    print('PYTHONDONTWRITEBYTECODE=1 ' + shlex.join(cmd), flush=True)
    subprocess.run(cmd, check=True)
PY
```

Saída de cada um: `Ran 1 test — OK`, rc **0**.

Ruff da aplicação inteira: `All checks passed!`, rc **0**, usando a configuração de `ruff.toml:5`.

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache .
```

Suite completa, SQL real, gates persistidos, dados de produção e certificação pelo Code/opus final: **[nao verificado]**. Os limites dos testes sem banco estão declarados em `ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:1`.

## 10/11 02:41 · sessao 51caec412156 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Aplicados os quatro procedentes. [Relato com arquivos tocados, censo de escritores/leitores e comandos de cada mutante](/home/ronald/wt-codex-fio-e0-v4-e4a-2-023058/app/RELATO_FIO_E0_V4_E4A_2.txt:6). [nao verificado]

- RED contra a base reproduziu `(2,1)` em vez de `(1,1)`. [Saída](/home/ronald/wt-codex-fio-e0-v4-e4a-2-023058/app/RELATO_FIO_E0_V4_E4A_2.txt:51).
- Passaram 7 testes em memória, 12 mutantes individuais e 11 verificações puras existentes. [Comandos e resultados](/home/ronald/wt-codex-fio-e0-v4-e4a-2-023058/app/RELATO_FIO_E0_V4_E4A_2.txt:55). [nao verificado]
- Django e Ruff ausentes: integração e lint **[não verificado]**. Certificação Code/opus pendente. [Limites](/home/ronald/wt-codex-fio-e0-v4-e4a-2-023058/app/RELATO_FIO_E0_V4_E4A_2.txt:99).

## 10/11 02:37 · sessao d497ed64097c · O169-FECHADA · verificador · origem=Code

**Não atende integralmente ao item (3):** o contador mede `ENCERRADOS + via=sistema`, inclui decisões administrativas feitas por `reconciliar` e deixa `legado` fora sem declarar essa exclusão. A assinatura é lida pela função real compartilhada com a abertura. (`app/ponto/services/cluster_espurio.py:103`, `app/ponto/services/cluster_espurio.py:112`, `app/ponto/services/cluster_espurio.py:115`, `app/ponto/services/veredito_celula.py:202`, `app/chamados/migrations/0039_a3_via_backfill.py:28`.)

A identidade Git do HEAD/base não pôde ser autenticada nesta cópia: **[nao verificado]**. As referências abaixo são às linhas dos arquivos disponíveis.

1. **Universo, exclusões e cumprimento do aval**

   O universo efetivo é: módulo `cluster_batida_espuria`, estado `resolvido` ou `fechado`, via `EVENTO_VIA['RECONCILIAR']`, cujo valor é `sistema`; opcionalmente um colaborador. Não há janela, filtro de situação atual ou exigência de escala ativa nesse contador. (`app/ponto/services/cluster_espurio.py:11`, `app/ponto/services/cluster_espurio.py:103`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:95`.)

   Conta **chamados**, inclusive vários no mesmo par pessoa/dia. Só soma quando encontra a célula atual desse par e `_leitura_da_celula` responde assinatura viva: pelo menos duas órfãs próximas e uma lâmpada apagada, respeitando `dia_sem_trabalho`. Sem dia, célula ou insumos medidos, não soma; esses casos permanecem no `universo.total`. (`app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/services/cluster_espurio.py:33`, `app/ponto/services/cluster_espurio.py:108`, `app/ponto/services/cluster_espurio.py:112`, `app/ponto/services/cluster_espurio.py:114`.)

   As exclusões declaradas são `admin`, `cascade` e `competencia_trancada`, todas justificadas como ADMIN/DP, L-099. (`app/ponto/services/cluster_espurio.py:115`, `app/chamados/catalogo/motor.py:88`, `app/chamados/catalogo/motor.py:92`.)

   **Há uma lacuna de cobertura demonstrável no código:** o backfill pode gravar `legado` em encerrados de qualquer módulo; esses registros não entram no contador nem aparecem nas exclusões. Via vazia também fica fora, e o próprio teste exige esse comportamento. Existência e quantidade atual desses clusters em produção: **[nao verificado]**. (`app/chamados/migrations/0039_a3_via_backfill.py:26`, `app/chamados/migrations/0039_a3_via_backfill.py:28`, `app/ponto/services/cluster_espurio.py:104`, `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:188`, `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:190`.)

   Portanto, o subconjunto selecionado é medido pela função real, mas **“via do gate reconciliar” não equivale a “encerramento automático”**, universo exigido pelo aval. (`aval_tres_respostas_1110_0040.txt:1`, anexo; `app/ponto/services/veredito_celula.py:202`; `app/chamados/models.py:654`.)

2. **Quem escreve, quem lê e ESMERIL-SEMANTICA**

   | Campo | Escritores e portas relevantes | Leitores relevantes |
   |---|---|---|
   | `status_local` | Nascimento pelo motor e `get_or_create`; transições por `_gravar_estado`. (`app/chamados/models.py:935`, `app/chamados/models.py:938`, `app/chamados/models.py:971`, `app/chamados/models.py:228`.) | Contador lê encerrados; reconciliador lê vivos; motor decide fechamento. (`app/ponto/services/cluster_espurio.py:103`, `app/ponto/services/cluster_espurio.py:66`, `app/chamados/models.py:673`.) |
   | `via_encerramento` | `_gravar_estado` carimba terminal e limpa ao voltar à vida. Chamadores entregam via humana, reconciliar, cascade ou tranca; o backfill também escreve `legado`. (`app/chamados/models.py:245`, `app/chamados/models.py:249`, `app/chamados/models.py:450`, `app/chamados/models.py:654`, `app/chamados/models.py:1619`, `app/chamados/models.py:1639`, `app/chamados/migrations/0039_a3_via_backfill.py:28`.) | Contador; proteção contra renascimento de cascade; proteção de fechamento administrativo; arquivo e destranca. (`app/ponto/services/cluster_espurio.py:104`, `app/chamados/models.py:1007`, `app/chamados/models.py:1342`, `app/chamados/views_arquivar.py:64`, `app/chamados/services/competencia_trancada.py:259`.) |
   | `modulo_origem` | Emissor fornece a constante; gate persiste no nascimento. (`app/ponto/services/cluster_espurio.py:166`, `app/chamados/models.py:971`.) | Contador, fecho, dedup e catálogo da data. (`app/ponto/services/cluster_espurio.py:103`, `app/ponto/services/cluster_espurio.py:66`, `app/ponto/services/cluster_espurio.py:85`, `app/chamados/juizes.py:114`.) |
   | `contexto_json` | Emissor fornece `data_turno`, órfãs e missing; alterações posteriores passam pelas operações da porta do contexto. (`app/ponto/services/cluster_espurio.py:179`, `app/chamados/services/contexto.py:76`, `app/chamados/services/contexto.py:102`, `app/chamados/services/contexto.py:125`, `app/chamados/services/contexto.py:161`.) | Dedup, juiz da data e catálogo; também identificação do dia em `furos_sem_cobranca`. (`app/ponto/services/cluster_espurio.py:85`, `app/chamados/juizes.py:100`, `app/chamados/catalogo/modulos.py:225`, `app/ponto/services/furos_sem_cobranca.py:69`.) |
   | `celula` do chamado | Gate resolve a FK no nascimento; alterações posteriores pela porta `apontar_celula`, chamada pela rejunção e pelos recuperadores de vínculo. (`app/chamados/models.py:958`, `app/chamados/models.py:1240`, `app/chamados/services/vinculos.py:99`, `app/chamados/rejuncao.py:107`.) | `dia_do_fato` prefere a FK; contador carrega essa relação para obter o dia. (`app/chamados/juizes.py:98`, `app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:108`.) |
   | `CelulaDia`, especialmente `trabalha`/DNA | Porta gera células e regenera cadastro; regeneração escreve `trabalha`, DNA e carimbos. (`app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:276`, `app/ponto/portas/celula.py:863`.) | Abertura e contador consultam células; `dia_sem_trabalho` lê `trabalha` e ata. (`app/ponto/services/cluster_espurio.py:46`, `app/ponto/services/cluster_espurio.py:110`, `app/chamados/juizes.py:435`, `app/chamados/juizes.py:441`.) |
   | Ata de `CelulaDia` | Cartório constrói órfãs/lâmpadas e entrega à lavra; porta escreve e salva. `carimbar_precedencia` altera a chave de precedência na mesma ata. (`app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:711`, `app/ponto/services/cartorio.py:1813`, `app/ponto/portas/celula.py:1042`, `app/ponto/portas/celula.py:914`, `app/ponto/portas/celula.py:1094`.) | `_leitura_da_celula`, `dia_sem_trabalho` e consumidor acordado pela alteração da ata. (`app/ponto/services/cluster_espurio.py:27`, `app/ponto/services/cluster_espurio.py:32`, `app/chamados/juizes.py:435`, `app/ponto/portas/celula.py:892`.) |

   **ESMERIL confirmado:** `fechar_o_fio_do_dia` é usado pela decisão administrativa, chama `c.reconciliar()` e registra “admin decidiu”. Assim, `sistema` identifica o gate usado, sem garantir autoria automática ou morte da assinatura. O contador reconhece explicitamente que inclui esse admin. (`app/ponto/services/veredito_celula.py:202`, `app/ponto/services/veredito_celula.py:205`, `app/ponto/services/veredito_celula.py:346`, `app/chamados/models.py:654`, `app/ponto/services/cluster_espurio.py:93`.)

   Há ainda um leitor que pode atribuir força probatória ao encerramento: no fallback de slot não derivável, o destino em `ENCERRADOS` pode provar materialização, sem filtro de módulo/via. A rechecagem usa `classificar_falta` somente dentro da janela viva; não consulta a assinatura de cluster. Vínculo concreto pergunta→cluster atingindo esse ramo em produção: **[nao verificado]**. (`app/chamados/juizes.py:745`, `app/chamados/juizes.py:760`, `app/chamados/juizes.py:766`, `app/chamados/juizes.py:768`, `app/chamados/juizes.py:776`.)

   Isso não autoriza generalizar para todo resolvedor: o receiver de encerramento chama um resolvedor que consulta `pergunta_viva` antes de carimbar. (`app/chamados/signals.py:115`, `app/chamados/services/disputa_emissao.py:1840`.)

   **Dia:** no nascimento normal, coincide: a abertura toma `cel.data`, grava esse dia em `data_turno` e resolve a FK pelo mesmo `dia_do_fato(fortes=True)`. Depois, porém, a FK vence o JSON; igualdade histórica universal não está garantida. Existe inclusive recuperador que escolhe dia por batida próxima da emissão e sobrescreve contexto/FK. Ocorrência dessa alteração em cluster: **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:49`, `app/ponto/services/cluster_espurio.py:179`, `app/chamados/models.py:1234`, `app/chamados/juizes.py:98`, `app/chamados/management/commands/carimbar_por_batida_coincidente.py:81`, `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98`.)

3. **Estados antes verdes que passam a sair 2**

   O cron diário das 06:24 passa a sair 2 mesmo com `pend=[]`, bastando o contador divergir de zero. (`app/config/crons.py:598`, `app/ponto/management/commands/detectar_cluster_espurio.py:70`, `app/ponto/management/commands/detectar_cluster_espurio.py:74`.)

   **Regeneração fornece um contraexemplo ao diagnóstico “o fechamento foi defeituoso”:** um encerramento pode ter ocorrido sem assinatura; depois, uma regeneração permitida muda o DNA passado e o cartório relavra uma assinatura. Fora dos 14 dias, o reconciliador não tenta renascer aquele encerrado, mas o contador o inclui. O novo alarme acusa a combinação atual, sem provar erro no ato anterior. Ocorrência real desse cenário: **[nao verificado]**. (`app/ponto/portas/celula.py:110`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:434`, `app/ponto/services/cartorio.py:1005`, `app/ponto/services/cluster_espurio.py:44`, `app/ponto/services/cluster_espurio.py:161`, `app/ponto/services/cluster_espurio.py:112`.)

   **Admin:** o teste exige contar um encerrado atribuído ao admin com assinatura parcial remanescente, mas apenas fabrica estado/ata; não executa o fluxo administrativo. Um caso administrativo legítimo concreto que permaneça assim até o cron é **[nao verificado]**. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:162`.)

   **Reaberto já persistido:** fica fora, porque deixa `ENCERRADOS` e perde a via. Há possibilidade de leitura concorrente desatualizada entre a seleção dos chamados e a consulta das células; o contador não revalida o estado. Ocorrência dessa corrida: **[nao verificado]**. (`app/chamados/catalogo/motor.py:113`, `app/chamados/models.py:249`, `app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:110`.)

   Também é incorreta a afirmação da entrega de que dedup por qualquer estado sempre impede renascimento: dentro da janela, `reconciliar_colab` chama `abrir`, e este tenta renascer o encerrado encontrado. (`app/ponto/services/cluster_espurio.py:161`, `app/ponto/services/cluster_espurio.py:164`, `app/chamados/models.py:921`.)

4. **`fechados_por_idade` ainda existe**

   O grep da cópia encontrou três linhas:

   - `app/docs/CORTES.md:430`
   - `app/docs/RELATO.md:10`
   - `app/docs/BACKLOG.md:1877`

   Portanto, o nome ainda permanece em documentação, inclusive no corte e na descrição da obra. Nesta cópia, o corte indicado na pergunta está em **430**, não 434. (`app/docs/CORTES.md:430`, `app/docs/RELATO.md:10`, `app/docs/BACKLOG.md:1877`.)

5. **Classes de reprovação**

   | Classe | Resultado da leitura |
   |---|---|
   | Segundo juiz / derivação paralela | O contador reutiliza `_leitura_da_celula` e `dia_do_fato`; não duplica esses predicados. A dívida anterior do cluster continua registrada como `SEM_JUIZ`; há também pendência anterior de limiar duplicado. (`app/ponto/services/cluster_espurio.py:108`, `app/ponto/services/cluster_espurio.py:112`, `app/core/juizes.py:1198`, `app/docs/BACKLOG.md:1793`.) |
   | Segundo escritor fora da porta | O novo contador só seleciona e conta; o comando imprime e alarma. Não acrescenta escrita nos campos auditados. (`app/ponto/services/cluster_espurio.py:103`, `app/ponto/services/cluster_espurio.py:113`, `app/ponto/management/commands/detectar_cluster_espurio.py:62`.) |
   | Relógio fora da borda | O contador novo não consulta relógio; a referência de hoje permanece no comando. (`app/ponto/services/cluster_espurio.py:88`, `app/ponto/management/commands/detectar_cluster_espurio.py:40`.) |
   | Campo com dois significados | Confirmada a confusão entre **via do gate** e **origem automática**. (`app/chamados/catalogo/motor.py:95`, `app/ponto/services/veredito_celula.py:202`, `app/ponto/services/cluster_espurio.py:104`.) |
   | Ordem de trava | A leitura nova não adquire travas nem altera sua ordem; tampouco oferece fotografia consistente entre as duas consultas. Os gates de escrita continuam usando a ordem centralizada. (`app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:110`, `app/chamados/models.py:711`.) |
   | Pouso antes do pré-requisito | A regra exige suite inteira verde e depois opus final. Cumprimento no hash desta obra: **[nao verificado]**. (`CLAUDE.md:374`.) | [nao verificado]

   **Os 17 mutantes mordem código real do contador/comando, mas não certificam o encerramento real.** O carregador compila as fontes da cópia; os mutantes substituem trechos dessas fontes e exigem falha de asserção, rejeitando erro de infraestrutura como mordida. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:30`, `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:64`, `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:74`.)

   A descrição “só ORM, relógio e I/O são dublês” está incompleta: **`pendencias` e `reconciliar_colab` também são mocks**. O mutante `recorta_por_idade` modifica o contador; não restaura o fechamento por idade. Assim, esses verdes não provam gates, SQL, ciclo administrativo ou regeneração seguida de renascimento. (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:4`, `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:100`, `app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:38`.)

   O teste anterior de idade efetivamente chama `pendencias` e `reconciliar_colab` e confere estado persistido, mas depende de Django/TestCase. Sua execução, a suite completa e o RED histórico nesta auditoria: **[nao verificado]**. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:21`, `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:56`, `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74`.)

6. **Performance**

   Carrega todos os **encerrados pela via `sistema`** do módulo, com objetos completos e `select_related('celula')`; não todos os encerrados de qualquer via. Depois carrega células completas por dois filtros independentes: colaboradores `IN C` e datas `IN D`. Isso alcança combinações cruzadas, além dos pares efetivamente pedidos. (`app/ponto/services/cluster_espurio.py:103`, `app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:110`.)

   A segunda carga pode chegar a `|C| × |D|` células existentes. Exemplo aritmético: 1.000 colaboradores e 365 datas permitem até 365.000 células nessa consulta. A assinatura ainda é recalculada por chamado, e a célula/ata pode ter sido carregada tanto pelo join quanto pela segunda consulta. (`app/ponto/services/cluster_espurio.py:107`, `app/ponto/services/cluster_espurio.py:110`, `app/ponto/services/cluster_espurio.py:112`.)

   Os índices declarados de chamado cobrem status, urgência e criação; não há composto declarado para módulo/via/status. A célula tem unicidade e índice pessoa/data. O scheduler atribui 30 segundos ao detector. Cardinalidade atual, tamanho plausível da frota afetada, plano SQL, memória e duração após o diff: **[nao verificado]**. (`app/chamados/models.py:108`, `app/escala/models.py:1833`, `app/escala/models.py:1836`, `app/config/crons.py:136`.)

## 10/11 02:34 · sessao 01935d990d69 · - · - · origem=Ronald (tmux)

1. Universo efetivo: chamados `cluster_batida_espuria`, estado `resolvido/fechado`, via `sistema`, todas as idades e situações; `--colab` é único recorte opcional. Conta chamados, inclusive duplicados do mesmo dia; a assinatura vem da célula atual por `(colaborador_id,dia)`. Sem dia/célula/ata medida não soma. (`app/ponto/services/cluster_espurio.py:103-114`; `app/chamados/catalogo/motor.py:30`; `app/ponto/services/cluster_espurio.py:15-37`.)

   Não equivale a “encerramentos automáticos”: decisão administrativa chama `reconciliar()` e grava a mesma via `sistema`; o teste exige contá-la mesmo com ausência parcial e assinatura remanescente. (`app/ponto/services/veredito_celula.py:201-205`; `app/chamados/models.py:654`; `app/chamados/catalogo/motor.py:95`; `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:162-167`.)

   Há exclusão silenciosa alcançável: `via='legado'`. O backfill grava essa via em terminais de qualquer módulo sem via anterior; portanto alcança clusters, mas ela não aparece nas três exclusões declaradas. Há também via vazia no esquema/default e no reverso da migration; a existência de exemplos atuais em produção é [nao verificado]. (`app/chamados/migrations/0039_a3_via_backfill.py:26-28`; `app/ponto/services/cluster_espurio.py:103-118`; `app/chamados/models.py:54`; `app/chamados/migrations/0039_a3_via_backfill.py:34`.)

2. O filtro presume identidade entre a via do evento e a origem automática; a porta administrativa acima desfaz essa identidade. A via representa “quem/como deixou de ser vivo”, sem motivo causal de idade/lastro. (`app/chamados/catalogo/motor.py:86-96`; `app/ponto/services/veredito_celula.py:201-205`.)

3. Cron muda para rc 2 quando o novo número diverge do esperado, mesmo sem pendência antiga; roda diariamente às 06:24. (`app/ponto/management/commands/detectar_cluster_espurio.py:62-74`; `app/config/crons.py:598`.)

   Um chamado já reaberto não pertence ao universo atual, e reabertura limpa a via; porém o contador não usa transação/lock e mantém objetos encerrados da primeira consulta enquanto lê células numa segunda consulta: corrida com reabertura é possível por construção, ocorrência real [nao verificado]. (`app/ponto/services/cluster_espurio.py:103-112`; `app/chamados/models.py:247-249`.)

   Regeneração legítima pode mudar o passado e relavrar assinatura. A regeneração admite reescrever cadastro passado, salva DNA/trabalha e manda rejulgar; o cartório então chama o reconciliador. Se o dia estiver fora da janela de abertura, o contador lê a assinatura nova mas o reconciliador não tenta renascer o chamado daquele dia. Isso não prova fechamento original sem lastro. (`app/ponto/portas/celula.py:110-117`; `app/ponto/portas/celula.py:263-277`; `app/ponto/portas/celula.py:422-434`; `app/ponto/services/cartorio.py:998-1005`; `app/ponto/services/cluster_espurio.py:40-49`; `app/ponto/services/cluster_espurio.py:161-179`.)

   Dentro da janela, não é correto dizer que dedup sempre impede renascimento: `abrir()` chama a porta de renascimento quando encontra encerrado. (`app/chamados/models.py:906-924`.)

4. `rg -n --hidden --glob '!.git/**' 'fechados_por_idade' .` encontrou três linhas: `app/docs/CORTES.md:430`, `app/docs/RELATO.md:10`, `app/docs/BACKLOG.md:1877`. O corte nesta cópia está na linha 430, não 434. (`app/docs/CORTES.md:430`.)

5. O contador novo usa a função comum de assinatura, mas existe duplicação anterior do limiar de 15 minutos no classificador de órfãs; o próprio backlog a registra como pendência. (`app/ponto/services/cluster_espurio.py:112`; `app/escala/orfa_classe.py:26-28`; `app/escala/orfa_classe.py:40-45`; `app/ponto/services/cluster_espurio.py:33-35`; `app/docs/BACKLOG.md:1793`.)

6. Custo: uma consulta materializa todos os chamados selecionados com `select_related('celula')`; outra materializa células para `colaborador_id IN C` e `data IN D`, abrangendo combinações cruzadas, não só os pares presentes nos chamados. O teto lógico dessa segunda carga é `|C|×|D|` células existentes, mesmo que haja poucos pares de interesse. JSONs de célula podem ser carregados pelo join e novamente pela segunda consulta; a leitura da assinatura repete-se por chamado. (`app/ponto/services/cluster_espurio.py:107-112`; `app/escala/models.py:1697`; `app/escala/models.py:1833`.)

   Índices declarados de chamado cobrem status/urgência/criação; não há índice composto declarado para módulo/via/status. A célula tem par único e índice colaborador/data. O scheduler reserva 30 segundos ao detector; cardinalidade, plano SQL, memória e duração atuais são [nao verificado]. (`app/chamados/models.py:103-113`; `app/escala/models.py:1833-1837`; `app/config/crons.py:136`.)

## 10/11 02:34 · sessao f1829c7fb19c · - · - · origem=Ronald (tmux)

Censo dos caminhos relevantes ao contador:

| Campo | Escritores | Leitores relevantes |
|---|---|---|
| `status_local` | Nascimento pelo motor e `get_or_create`: `app/chamados/models.py:935`, `:938`, `:971`. Transições pelo núcleo `_gravar_estado`: `:228`, `:296`. | Contador seleciona `ENCERRADOS`: `app/ponto/services/cluster_espurio.py:103`. Fecho existente seleciona `VIVOS`: `:66`. Gate decide no motor: `app/chamados/models.py:673`. |
| `via_encerramento` | Núcleo carimba terminal e limpa vivo: `app/chamados/models.py:245`, `:249`. `reconciliar` entrega `EVENTO_VIA['RECONCILIAR']`: `:654`; humano entrega via humana: `:450`; cascade: `:1619`; tranca: `:1639`. | Contador exige via do reconciliar: `app/ponto/services/cluster_espurio.py:104`. Renascimento barra cascade: `app/chamados/models.py:1007`. Reabertura protege fechamento admin: `:1342`. |
| `modulo_origem` | Emissor cluster fornece constante: `app/ponto/services/cluster_espurio.py:166`; nascimento efetivo: `app/chamados/models.py:971`. | Contador: `app/ponto/services/cluster_espurio.py:103`; fecho: `:66`; dedup: `:85`; juiz da data consulta catálogo: `app/chamados/juizes.py:114`. |
| `contexto_json` | Emissor fornece dia, órfãs e missing: `app/ponto/services/cluster_espurio.py:179`. Fora do nascimento, portas escrevem: `app/chamados/services/contexto.py:76`, `:102`, `:125`, `:161`. | Dedup cluster: `app/ponto/services/cluster_espurio.py:85`; juiz da data: `app/chamados/juizes.py:100`; catálogo lê chave declarada: `app/chamados/catalogo/modulos.py:225`. |
| `celula` do chamado | Nascimento resolve FK: `app/chamados/models.py:958`; procura por colaborador e dia: `:1240`. Porta posterior: `app/chamados/services/vinculos.py:93`, `:99`, `:101`. | `dia_do_fato` prefere FK antes do contexto: `app/chamados/juizes.py:98`. Contador carrega FK e depois procura célula atual pelo par pessoa/data: `app/ponto/services/cluster_espurio.py:107`, `:110`. |
| `CelulaDia`/ata | Geração/criação pela porta: `app/ponto/portas/celula.py:259`, `:600`. Cartório constrói ata: `app/ponto/services/cartorio.py:696`, `:704`, `:711`; lavra pela porta: `:1813`. Porta escreve ata: `app/ponto/portas/celula.py:1015`, `:1042`; `carimbar` também escreve chave da ata: `:1094`. | Assinatura cluster lê `trabalha`/ata pelo juiz e lê listas órfãs/lâmpadas: `app/ponto/services/cluster_espurio.py:25`, `:27`, `:29`, `:32`. `dia_sem_trabalho`: `app/chamados/juizes.py:435`, `:439`, `:441`. |

**ESMERIL: `sistema` não demonstra fechamento automático.** O catálogo define `RECONCILIAR → sistema` (`app/chamados/catalogo/motor.py:95`), mas `fechar_o_fio_do_dia`, uma decisão administrativa, chama `c.reconciliar()` e registra “admin decidiu” (`app/ponto/services/veredito_celula.py:201`, `:205`). O mesmo caminho serve ao veredito administrativo (`:346`) e à declaração humana de folga (`app/ponto/services/declarar_dia.py:202`, `:206`). O contador admite explicitamente esses casos (`app/ponto/services/cluster_espurio.py:92`).

O cluster emitido normalmente não é “chamado de furo”: categoria `batida_espuria` (`app/ponto/services/cluster_espurio.py:169`), módulo ausente de `CALADOS_POR_ISENCAO` (`app/chamados/catalogo/modulos.py:235`) e categoria/subcategoria ausentes dos mapas (`app/chamados/models.py:3664`, `:3676`); a classificação é feita em `:1244`. Assim, sua guarda de premissa permite `reconciliar` sem avaliar assinatura (`app/chamados/models.py:727`). Ainda existe a guarda de pergunta viva (`:685`).

Isso prova mistura de origens administrativas e automáticas no mesmo valor. **Não prova, isoladamente, falso positivo de um fluxo administrativo normal:** “trabalhou” exige grade sem órfãs/missing (`app/ponto/services/simular_dia.py:11`), e os demais vereditos criam ausência aprovada (`app/ponto/services/veredito_celula.py:330`). O teste “admin” somente fabrica estado e ata; não executa o escritor administrativo (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:162`). Caso administrativo legítimo concreto que sobreviva com assinatura até o cron: **[nao verificado]**.

**Dia:** o nascimento normal usa `cel.data` (`app/ponto/services/cluster_espurio.py:49`), grava esse dia no contexto (`:179`) e resolve FK pelo mesmo `dia_do_fato(fortes=True)` (`app/chamados/models.py:1234`). Portanto coincide nesse caminho. A igualdade histórica universal não é garantida: hoje a FK vence o contexto (`app/chamados/juizes.py:98`), e o comando de batida coincidente escolhe dia pela proximidade de `criado_em`, sem confrontar o dia anterior (`app/chamados/management/commands/carimbar_por_batida_coincidente.py:69`, `:81`), sobrescreve contexto e aponta célula (`:98`, `:100`). Ocorrência concreta em cluster: **[nao verificado]**.

**Reaberto:** deixa de integrar o universo por estado e perde a via (`app/chamados/catalogo/motor.py:113`; `app/chamados/models.py:249`; `app/ponto/services/cluster_espurio.py:103`). Isso não produz falso positivo estável no contador; inconsistência entre leituras concorrentes das duas consultas permanece **[nao verificado]**.

**Evento e fonte única:** cartório chama reconciliação depois da lavra (`app/ponto/services/cartorio.py:998`, `:1004`). O novo contador reaproveita `_leitura_da_celula` (`app/ponto/services/cluster_espurio.py:112`) e `dia_do_fato` (`:108`), sem escritor novo nesses campos (`:103`). Exaustividade de todos os leitores e escritores da aplicação: **[nao verificado]**.

## 10/11 02:33 · sessao 2eb698062457 · - · - · origem=Ronald (tmux)

Os 17 mutantes mordem o contador e o comando reais compilados da cópia: `carregar()` lê/compila a fonte (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:30`), o serviço é carregado em `:90`, o comando em `:114`, e cada mutante altera aquela fonte em memória e exige falha de asserção sem erro de infraestrutura (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:64`, `:74`, `:75`).

O contrato **não exercita o fechamento real**: substitui `pendencias` e `reconciliar_colab` por `Mock` (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:100`, `:102`, `:103`). Portanto, a frase “Só ORM, relógio e I/O [...] são dublês” está incompleta (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:4`). Os juízes puros e `_leitura_da_celula` são reais; o ORM, as relações e o relógio são substituídos (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:92`, `:95`, `:107`).

O mutante `recorta_por_idade` altera **o recorte do contador**, não o fechamento por idade (`app/ponto/tests/test_o169_cluster_encerrado_mutantes.py:38`). Os testes antigos que realmente chamam `pendencias` e `reconciliar_colab`, verificando o estado persistido, estão em `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:56`; dependem de Django/TestCase (`:21`). Sua execução nesta auditoria: [nao verificado].

Executei os 35 testes sem escrita: 35 OK em 0,564 s; sem transcrição persistida para referência arquivo:linha [nao verificado]. Também executei uma mutação adicional só em memória, substituindo `_fato` por `False` no fecho (`app/ponto/services/cluster_espurio.py:75`): os 18 contratos continuaram verdes; execução sem artefato persistido [nao verificado]. Isso corresponde à lacuna documentável dos mocks em `app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:100`.

Quanto às classes:

- **Segundo juiz/derivação paralela:** o novo contador chama o mesmo `_leitura_da_celula` da abertura (`app/ponto/services/cluster_espurio.py:47`, `:112`); esta consulta `dia_sem_trabalho` (`:21`, `:25`), autoridade declarada em `app/core/juizes.py:136`. O registro ainda reconhece dívida do cluster como `SEM_JUIZ`, anterior à medida (`app/core/juizes.py:1198`).
- **Segundo escritor:** o novo corpo seleciona chamados/células, conta e devolve dict; não escreve os modelos (`app/ponto/services/cluster_espurio.py:103`). O comando imprime e levanta `SystemExit(2)` (`app/ponto/management/commands/detectar_cluster_espurio.py:62`, `:74`).
- **Relógio:** o novo contador não usa relógio (`app/ponto/services/cluster_espurio.py:88`); o comando lê `localdate` na borda (`app/ponto/management/commands/detectar_cluster_espurio.py:40`).
- **Ordem de trava:** o contador adiciona duas seleções sem transação ou trava explícita (`app/ponto/services/cluster_espurio.py:107`, `:110`). A lista materializada de chamados não é revalidada antes da contagem (`:112`); uma reabertura concorrente pode gerar leitura desatualizada, condicional ao intercalamento; ocorrência real [nao verificado].
- **Pré-requisito de pouso:** a regra exige suite inteira verde antes do opus final (`CLAUDE.md:374`). Certificação desses passos não consta dos testes do contrato, que explicitamente não verificam SQL/gates/produção (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:6`); cumprimento nesta obra [nao verificado].

Exclusão silenciosa adicional: o próprio contrato cria chamado com `via=''` e exige que não conte (`app/ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:188`, `:190`), enquanto a lista pública de exclusões contém apenas admin/cascade/tranca (`app/ponto/services/cluster_espurio.py:115`). Existência de terminal sem via no dado produtivo fornecido [nao verificado].

## 10/11 02:30 · sessao 65327b80d588 · O169-FECHADA · construtor · origem=Code

Implementei `cluster_encerrado_sem_lastro` pela assinatura real, com universo e exclusões declarados. O vigia imprime o contador, informa “universo vazio” e sai com código 2 quando `numero != esperado`. ([contador](/home/ronald/wt-codex-o169-fechada-022038/app/ponto/services/cluster_espurio.py:88); [vigia](/home/ronald/wt-codex-o169-fechada-022038/app/ponto/management/commands/detectar_cluster_espurio.py:62))

Resultados observados:

- **RED antes:** cinco testes, nove falhas com subcasos. ([registro](/home/ronald/wt-codex-o169-fechada-022038/app/docs/O169-FECHADA-construtor2.r:40)) [nao verificado]
- **Depois:** 18 testes passaram; 17 mutantes foram mordidos por asserção. ([saída](/home/ronald/wt-codex-o169-fechada-022038/app/docs/O169-FECHADA-construtor2.r:56)) [nao verificado]
- **Ruff 0.17.4:** limpo nos arquivos tocados. A versão declarada, 0.16.4, ficou [nao verificado]. ([registro](/home/ronald/wt-codex-o169-fechada-022038/app/docs/O169-FECHADA-construtor2.r:92))
- **ORM:** não executou por falta de Django; certificação Code/opus permanece [nao verificado]. ([registro](/home/ronald/wt-codex-o169-fechada-022038/app/docs/O169-FECHADA-construtor2.r:105))

**Fallback:** sim, esse ramo aceita destino encerrado como testemunha sem filtrar módulo ou via; a rechecagem ocorre apenas dentro da janela viva. Vínculos concretos com cluster em produção: [nao verificado]. ([juizes.py:760](/home/ronald/wt-codex-o169-fechada-022038/app/chamados/juizes.py:760))

A [entrega completa](/home/ronald/wt-codex-o169-fechada-022038/app/docs/O169-FECHADA-construtor2.r:1) contém arquivos/motivos, comandos individuais dos mutantes e o censo de escritores e leitores com arquivo:linha.

## 10/11 02:26 · sessao 7e3a8326b035 · REVERTER-DIA-SEM-RECALCULO · verificador · origem=Code

**As cinco curas foram implementadas, com duas ressalvas: o teste Django não prova a precedência do ANTES perante a porta, e rollback externo não deixa trilha completa no banco.** A preparação precede a porta; a conclusão usa `on_commit`; o teste real inspeciona o ANTES somente após `call_command` retornar. (`app/ponto/management/commands/aplicar_col900.py:169`, `app/ponto/management/commands/aplicar_col900.py:199`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:159`)

Executei os dois módulos sem banco: **36 testes, OK**, incluindo os 16 mutantes. A reprodução dos “74, OK”, a execução Django e a confirmação independente do hash informado ficam **[nao verificado]**. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:392`, `app/ponto/tests/test_reverter_regen_sem_banco.py:84`)

**1. PROCEDENTES 1–5 e asserções**

| PROCEDENTE | Verificação |
|---|---|
| **1 — protocolo e falhas** | Curado conforme pedido: auditoria ANTES obrigatória, JSON antes da porta, `fsync` do arquivo e diretório, preparação dentro do `try`, auditoria DEPOIS e `SystemExit` contextual quando o DEPOIS falha. (`app/ponto/management/commands/aplicar_col900.py:35`, `app/ponto/management/commands/aplicar_col900.py:79`, `app/ponto/management/commands/aplicar_col900.py:118`, `app/ponto/management/commands/aplicar_col900.py:165`) |
| **2 — hash após callbacks** | Curado: a conclusão é registrada depois da última porta. O teste com fila de callbacks exige que permaneça `preparado` até executá-los. (`app/ponto/management/commands/aplicar_col900.py:184`, `app/ponto/management/commands/aplicar_col900.py:199`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:289`) |
| **3 — estado e ator** | Curado: `laco_terminado/interrompido`; `quem.processo` vem diretamente de `log.ator`, sem fallback inventado. (`app/ponto/management/commands/aplicar_col900.py:88`, `app/ponto/management/commands/aplicar_col900.py:101`) |
| **4 — mutantes coletados e Django real** | Curado formalmente: mutantes em método coletável, executados em memória; teste real com `call_command`, pasta temporária, auditoria e hashes reais, `captureOnCommitCallbacks(execute=True)`. Ressalva de precedência abaixo. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:392`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:139`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:153`) |
| **5 — contador AST** | Curado no alcance solicitado: quinto posicional, `*args`, `**kwargs`, flag nomeada e alias de importação; inclui intrusos posicionais e aliases. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:31`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:65`) |

Não identifiquei afrouxamento indevido: a troca de estado corrige sua promessa; o ator nulo do duplo é cobrado por igualdade, e existe contraste com ator não nulo; falha no DEPOIS agora exige auditoria ANTES e hashes/caminho no erro. A exigência financeira de retorno a 240 permanece. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:221`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:261`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:280`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:193`)

**2. Condições do aval e IDA**

- **Chamador automático zero:** a varredura passou exigindo exatamente um chamador produtivo com flag, `aplicar_col900`; o chamado está no ramo `--reverter`. Isso verifica os fontes alcançados pelo contador; invocações externas desse comando ficam **[nao verificado]**. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:50`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:60`, `app/ponto/management/commands/aplicar_col900.py:146`, `app/ponto/management/commands/aplicar_col900.py:184`)
- **Trilha em exportada:** preparação consulta o juiz da lavra e fotografa todas as competências pedidas quando alguma está lavrada; grava em `/app/logs`. Falha no ANTES impede a porta; falha tratada no DEPOIS produz saída não zero com caminho e hashes disponíveis. Isso atende à cura prescrita, mas não garante ambos os logs e ambos os hashes em qualquer falha possível. (`app/ponto/management/commands/aplicar_col900.py:32`, `app/ponto/management/commands/aplicar_col900.py:63`, `app/ponto/management/commands/aplicar_col900.py:86`, `app/ponto/management/commands/aplicar_col900.py:118`)
- **VOLTA sem flag barrada:** a guarda retorna antes de escrever; há contraste com flag e teste real comparando célula, DiaPago e hash inteiros. (`app/ponto/portas/celula.py:779`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:487`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:117`)

A IDA conserva seu chamado ao rabo comum, e a VOLTA passou a chamar esse mesmo rabo. A guarda financeira de 09/2026 continua presente; o teste distingue restauração da célula de preservação do dinheiro nessa competência. Integridade histórica além do diff fornecido: **[nao verificado]**. (`app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:811`, `app/ponto/services/fechamento.py:36`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:195`)

**3. Escritores, leitores e significado dos campos**

| Campo/grupo | Quem escreve/origem | Quem lê |
|---|---|---|
| JSON `colab`, `dias`, `quando`, `motivo`, `apesar_da_lavra` | Preparação: colaborador, argumentos e relógio. (`app/ponto/management/commands/aplicar_col900.py:68`) | Auditorias e testes; a conclusão reutiliza motivo e flag. (`app/ponto/management/commands/aplicar_col900.py:84`, `app/ponto/management/commands/aplicar_col900.py:111`) |
| `competencias[].mes/ano/lavrada` | `competencia_do_fato` e juiz `competencia_fechada_para_celula`. (`app/ponto/management/commands/aplicar_col900.py:63`) | Preparação, conclusão e teste real. (`app/ponto/management/commands/aplicar_col900.py:99`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:165`) |
| `hash_antes/hash_depois` | Helpers leem **FechamentoMensal**; preparação/conclusão armazenam. (`app/ponto/portas/regen_exportada.py:62`, `app/ponto/management/commands/aplicar_col900.py:74`, `app/ponto/management/commands/aplicar_col900.py:100`) | Logs ANTES/DEPOIS e testes. (`app/ponto/management/commands/aplicar_col900.py:82`, `app/ponto/management/commands/aplicar_col900.py:114`) |
| `valores_antes` | Helper lê o subconjunto `CAMPOS`; pode devolver `None`. (`app/ponto/portas/regen_exportada.py:77`, `app/ponto/management/commands/aplicar_09_corte_b.py:67`) | Teste real compara a fotografia. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:165`) |
| `celulas[data]` | Preparação copia o DNA atual; não é o `dna_anterior`. (`app/ponto/management/commands/aplicar_col900.py:76`) | Testes conferem a fotografia anterior ao ato. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:193`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:164`) |
| `quem.processo/usuario_so` | `log.ator` e `getpass.getuser()`. (`app/ponto/management/commands/aplicar_col900.py:88`) | Descrição/payload DEPOIS e testes. (`app/ponto/management/commands/aplicar_col900.py:108`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:181`) |
| `estado`, `registrado_em` | Preparação escreve `preparado`; conclusão escreve estado do laço e horário do registro. (`app/ponto/management/commands/aplicar_col900.py:71`, `app/ponto/management/commands/aplicar_col900.py:101`) | Log DEPOIS e testes; seu `quando` corresponde a `registrado_em`. (`app/ponto/management/commands/aplicar_col900.py:111`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:191`) |
| `resultados[].dia/resultado/revertido` | Laço: sem vínculo ou booleano devolvido pela porta. (`app/ponto/management/commands/aplicar_col900.py:179`, `app/ponto/management/commands/aplicar_col900.py:187`) | Log DEPOIS e teste; não mede sucesso financeiro. (`app/ponto/management/commands/aplicar_col900.py:113`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:184`) |
| LogAuditoria: ação, modelo, objeto, descrição, antes/depois | Comando monta os payloads; `evento_kw → evento → objects.create` persiste. (`app/ponto/management/commands/aplicar_col900.py:79`, `app/ponto/management/commands/aplicar_col900.py:105`, `app/core/observ.py:134`, `app/core/observ.py:142`) | Teste real consulta as duas ações; tela genérica exibe metadados e descrição. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:168`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:185`, `app/templates/core/log_auditoria.html:55`) |
| LogAuditoria `ator/usuario/ip/criado_em` | `evento` resolve processo ou request; timestamp é automático. `ator` pode ser nulo fora de `manage.py`. (`app/core/observ.py:22`, `app/core/observ.py:108`, `app/core/models.py:303`) | JSON copia `ator`; tela mostra `usuario`, não interpreta `quem.processo`. (`app/ponto/management/commands/aplicar_col900.py:88`, `app/templates/core/log_auditoria.html:58`) |

O hash cobre somente FechamentoMensal, exclui campos de controle e usa MD5 truncado a 12 caracteres; não certifica DNA nem DiaPago. O escritor financeiro continua sendo o fechamento. (`app/ponto/portas/regen_exportada.py:37`, `app/ponto/portas/regen_exportada.py:70`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`)

Na célula, a VOLTA lê elegibilidade e os dois DNAs; escreve `dna`, limpa `dna_anterior`, recompõe `trabalha/escala_geradora`, atualiza `regenerada_em/regeneracoes` e emite EventoCelula. Geradora, trabalho e carimbo têm leitores existentes. (`app/ponto/portas/celula.py:776`, `app/ponto/portas/celula.py:785`, `app/escala/alimentacao.py:270`, `app/ponto/services/dia_do_colab.py:91`, `app/escala/services/regeneracao.py:119`)

**Existe leitor com outro contrato:** a VOLTA usa a ação `regenerar_celulas_vinculo` com `{dna,revertida}`; `janelas_de_regeneracao` seleciona essa ação, exige `desde/ate` e pula a linha. É a incompatibilidade já indicada para fila. (`app/ponto/portas/celula.py:799`, `app/escala/services/regeneracao.py:58`)

**Ordem:** em autocommit, o financeiro é solicitado dentro da porta e roda antes de sua devolução; sob `atomic` externo, seus callbacks são registrados antes da conclusão. Entretanto, falhas financeiras são engolidas com warning: a ordem garante posterioridade à tentativa, não sucesso do recálculo. (`app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1818`, `app/ponto/services/fechamento.py:1824`, `app/ponto/management/commands/aplicar_col900.py:199`)

**Rollback externo:** pela composição do protocolo, ficam no disco o JSON inicial `preparado`, `resultados=[]`, sem `hash_depois/registrado_em`; alterações bancárias e auditoria ANTES são desfeitas, e a conclusão não roda. Arquivo e banco não formam uma unidade atômica. Execução desse cenário com Django: **[nao verificado]**. (`app/ponto/management/commands/aplicar_col900.py:71`, `app/ponto/management/commands/aplicar_col900.py:89`, `app/ponto/management/commands/aplicar_col900.py:99`, `app/core/observ.py:133`)

**4. Mordida do Django real e dos mutantes**

- **JSON antes da porta:** o teste real reprova ausência, mas não prova precedência: verifica o arquivo após `call_command` retornar. Gravação tardia, ainda antes desse retorno, não é excluída por essas asserções. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:155`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:159`)
- **Auditoria ANTES:** ausência total reprova pelo `.get`; sua presença antes da entrada na porta também não é observada pelo teste real. Essa ordem é cobrada pelo duplo na entrada de `_reverter`. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:168`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:181`)
- **Hash antecipado:** há mordida substantiva: após callbacks, exige hash diferente do anterior e igualdade entre JSON e hash real, além de DNA restaurado e DiaPago 240. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:175`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:192`)
- **16 mutantes:** todos passaram pelo verificador de falha pretendida: instalação obrigatória, zero erros acidentais, exatamente uma falha e mensagem esperada. Isso valida os probes com duplos; não equivale a 16 mutações executadas no Django real. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:404`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:447`)

**5. Classes que já reprovaram obra**

| Classe | Situação |
|---|---|
| Segundo juiz / derivação financeira paralela | Não introduzidos: VOLTA chama um rabo comum; este chama cartório e a porta financeira existentes. (`app/ponto/portas/celula.py:811`, `app/ponto/portas/celula.py:434`, `app/ponto/portas/celula.py:458`) |
| Segundo escritor fora da porta | Não identificado no delta: reposição fica na porta e dinheiro no fechamento; JSON é trilha. (`app/ponto/portas/celula.py:794`, `app/ponto/services/fechamento.py:1160`, `app/ponto/management/commands/aplicar_col900.py:35`) |
| Relógio fora da borda | Nenhum relógio decisório novo: a VOLTA reutiliza o rabo; os horários novos do comando são carimbos da trilha. (`app/ponto/portas/celula.py:426`, `app/ponto/management/commands/aplicar_col900.py:70`, `app/ponto/management/commands/aplicar_col900.py:102`) |
| Selo sem mordida | Os mutantes mordem; permanece a lacuna temporal específica do teste Django descrita acima. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:456`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:159`) |
| Ordem de trava | Continua a leitura sem `select_for_update`, antes do `atomic`; não há nova inversão demonstrada. (`app/ponto/portas/celula.py:776`, `app/ponto/portas/celula.py:785`) |
| Pouso antes do pré-requisito | Ordem efetiva de deploy/pouso **[nao verificado]**. | [nao verificado]
| Campo com dois significados | Estado e ator novos foram corrigidos; persiste o formato incompatível da ação antiga para `janelas_de_regeneracao`. (`app/ponto/management/commands/aplicar_col900.py:88`, `app/ponto/management/commands/aplicar_col900.py:101`, `app/ponto/portas/celula.py:804`, `app/escala/services/regeneracao.py:63`) |

## 10/11 02:24 · sessao 08d109c13068 · - · - · origem=Ronald (tmux)

Os PROCEDENTES 1–3 foram implementados conforme a cura descrita: ANTES obrigatório, JSON durável antes da porta, DEPOIS com erro contextual, conclusão em `on_commit`, estado preciso e ator copiado do LogAuditoria (`app/ponto/management/commands/aplicar_col900.py:47`, `:79`, `:88`, `:99`, `:118`, `:165`, `:199`).

A garantia não equivale a sucesso incondicional: o recalculo pode falhar e apenas registrar warning; `laco_terminado` continua significando término do laço (`app/ponto/services/fechamento.py:1818`; `app/ponto/management/commands/aplicar_col900.py:101`).

| Campo/grupo | Escritor e origem | Leitor observado |
|---|---|---|
| `colab`, `dias`, `quando`, `motivo`, flag | Preparação, argumentos e relógio (`aplicar_col900.py:68`) | Preparação/conclusão e testes (`aplicar_col900.py:84`, `:107`; `test_reverter_regen_recalcula_dinheiro.py:162`) | [nao verificado]
| Competências e `lavrada` | Autoridades existentes (`aplicar_col900.py:63`; `folha/export.py:609`) | Fotografia e conclusão (`aplicar_col900.py:73`, `:99`) |
| `hash_antes/depois` | Helpers leem exclusivamente FechamentoMensal (`regen_exportada.py:56`, `:62`) | Auditorias e testes (`aplicar_col900.py:82`, `:114`; `test_reverter_regen_recalcula_dinheiro.py:177`) | [nao verificado]
| `valores_antes` | Subconjunto `CAMPOS`, podendo ser `None` (`regen_exportada.py:77`; `aplicar_09_corte_b.py:67`) | Testes (`test_reverter_regen_recalcula_dinheiro.py:165`) | [nao verificado]
| `celulas` | Cópia do DNA atual (`aplicar_col900.py:76`) | Testes (`test_reverter_regen_recalcula_dinheiro.py:164`) | [nao verificado]
| `quem` | `log.ator` e `getpass.getuser()` (`aplicar_col900.py:88`) | Conclusão e testes (`aplicar_col900.py:108`; `test_reverter_regen_recalcula_dinheiro.py:181`) | [nao verificado]
| `estado`, `registrado_em` | Conclusão (`aplicar_col900.py:101`) | Auditoria DEPOIS e testes (`aplicar_col900.py:111`; `test_reverter_regen_recalcula_dinheiro.py:180`) | [nao verificado]
| `resultados` | Laço: sem vínculo ou retorno booleano da porta (`aplicar_col900.py:179`, `:187`) | Auditoria DEPOIS e teste (`aplicar_col900.py:113`; `test_reverter_regen_recalcula_dinheiro.py:184`) | [nao verificado]
| LogAuditoria ANTES/DEPOIS | `evento_kw → evento → objects.create` (`core/observ.py:133`, `:142`); payloads (`aplicar_col900.py:79`, `:105`) | Testes específicos e tela genérica (`test_reverter_regen_recalcula_dinheiro.py:168`, `:185`; `core/views.py:705`) |

O hash não certifica DiaPago nem DNA: consulta somente FechamentoMensal, exclui campos de controle e usa MD5 truncado a 12 caracteres (`app/ponto/portas/regen_exportada.py:37`, `:70`). Se a leitura posterior falhar, o erro inclui caminho e hashes já obtidos, com `None` nos ausentes (`app/ponto/management/commands/aplicar_col900.py:118`).

Sob rollback externo, a consequência esperada é: arquivo inicial `preparado`, `resultados=[]`, sem `hash_depois` nem `registrado_em`; auditoria ANTES e alterações bancárias revertidas, callbacks descartados. A composição arquivo/banco não é atômica (`app/ponto/management/commands/aplicar_col900.py:68`, `:89`, `:99`, `:199`; `app/core/observ.py:133`). Execução desse cenário com Django: [nao verificado].

Há um leitor preexistente com contrato diferente: `janelas_de_regeneracao` seleciona a ação compartilhada com a IDA e exige `desde/ate`, enquanto a VOLTA escreve `dna/revertida`; ele ignora essa linha, conforme a fila já declarada (`app/escala/services/regeneracao.py:58`; `app/ponto/portas/celula.py:799`). Também permanece a consulta sem `select_for_update` (`app/ponto/portas/celula.py:776`).

## 10/11 02:24 · sessao 9eefe90fc0d8 · - · - · origem=Ronald (tmux)

- A VOLTA chama o mesmo rabo da IDA após seu `atomic`; a IDA já o chama, e a edição também segue esse desenho. A mudança fornecida troca apenas o bloco final da VOLTA; comprovação histórica por Git indisponível nesta cópia [nao verificado]. Referências atuais: `app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:366`, `app/ponto/portas/celula.py:785`, `app/ponto/portas/celula.py:811`.

- O rabo pergunta ao cartório antes de agendar dinheiro; exclui futuro e veredito humano, deduplica por competência usando `janela_atual`, chama `recalcular_por_evento` e agenda rejunção/falta espúria. Não introduz juiz ou escritor financeiro paralelo. `app/ponto/portas/celula.py:422`, `app/ponto/portas/celula.py:431`, `app/ponto/portas/celula.py:434`, `app/ponto/portas/celula.py:447`, `app/ponto/portas/celula.py:456`, `app/ponto/portas/celula.py:458`, `app/ponto/portas/celula.py:493`, `app/ponto/portas/celula.py:502`.

- Em autocommit, o callback financeiro é solicitado dentro da porta; sob transação externa, ele fica registrado antes da conclusão do comando. O código financeiro agenda `_rodar` por `on_commit`, e `_rodar` calcula a competência pela autoridade `competencia_do_fato`. `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1824`, `app/ponto/management/commands/aplicar_col900.py:192`.

- **A ordem não prova sucesso financeiro:** o juiz e o disparo financeiro engolem exceções; o próprio callback financeiro também engole falhas e apenas registra warning. Portanto `laco_terminado` é semanticamente adequado, e não certifica recalculado. `app/ponto/portas/celula.py:437`, `app/ponto/portas/celula.py:459`, `app/ponto/services/fechamento.py:1818`, `app/ponto/services/fechamento.py:1825`, `app/ponto/management/commands/aplicar_col900.py:101`.

- A guarda da VOLTA sem flag precede qualquer escrita e retorna `False` em lavrada; o universo consultado é composto pelo juiz `competencia_fechada_para_celula`, compartilhado com a IDA. `app/ponto/portas/celula.py:779`, `app/ponto/portas/celula.py:782`, `app/ponto/management/commands/gerar_celulas.py:146`, `app/ponto/management/commands/gerar_celulas.py:152`, `app/folha/export.py:589`.

- Setembro/2026 continua protegido na porta financeira: a competência consta da guarda e os colaboradores das empresas exportadas são excluídos do universo da escrita, com tentativa de auditoria. A flag da célula não desativa essa guarda. `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:560`, `app/ponto/services/fechamento.py:564`, `app/ponto/services/fechamento.py:569`.

- Há **formato preexistente incompatível sob a mesma ação de auditoria**: a VOLTA grava `acao='regenerar_celulas_vinculo'` com `{dna, revertida}`; o leitor `janelas_de_regeneracao` seleciona essa mesma ação, espera `desde`/`ate` e ignora o registro por `KeyError`. É exatamente a fila já descartada da triagem, não uma cura desta volta. `app/ponto/portas/celula.py:800`, `app/ponto/portas/celula.py:804`, `app/escala/services/regeneracao.py:58`, `app/escala/services/regeneracao.py:63`, `app/escala/services/regeneracao.py:65`.

- O leitor vivo do carimbo reconhece a VOLTA como reescrita: ela atualiza `regenerada_em`/`regeneracoes`; `reescrita_da_celula` combina regeneração e edição, alimentando cobrança órfã e apresentação do dia. `app/ponto/portas/celula.py:792`, `app/ponto/portas/celula.py:793`, `app/escala/services/regeneracao.py:72`, `app/escala/services/regeneracao.py:77`, `app/escala/services/regeneracao.py:119`, `app/ponto/services/dia_do_colab.py:93`.

- A ausência preexistente de trava continua: a célula é lida antes do `atomic`, sem `select_for_update`; o delta fornecido não altera esse trecho. Não comprova nova inversão de ordem de trava. `app/ponto/portas/celula.py:776`, `app/ponto/portas/celula.py:785`; histórico além do delta [nao verificado].

- O evento é escrito dentro do mesmo `atomic` da reposição; seu consumidor relê célula/chamados atuais, passando pelas portas existentes, e não é um segundo motor financeiro. `app/ponto/portas/celula.py:794`, `app/ponto/portas/celula.py:796`, `app/ponto/portas/celula.py:896`, `app/chamados/reconciliador.py:1320`, `app/chamados/reconciliador.py:1363`, `app/chamados/reconciliador.py:1368`.

- O contrato AST cobra um único rabo, proíbe chamadas diretas de juiz/dinheiro e exige o rabo imediatamente após o `atomic`; seus mutantes incluem juiz paralelo, dinheiro paralelo, duplicação e posição dentro do `atomic`. `app/ponto/tests/test_reverter_regen_sem_banco.py:60`, `app/ponto/tests/test_reverter_regen_sem_banco.py:74`, `app/ponto/tests/test_reverter_regen_sem_banco.py:99`, `app/ponto/tests/test_reverter_regen_sem_banco.py:107`, `app/ponto/tests/test_reverter_regen_sem_banco.py:115`, `app/ponto/tests/test_reverter_regen_sem_banco.py:123`.

_3345 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
