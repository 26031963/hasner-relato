# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 02:54** · publicado em **10/11/2026 02:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 10/11 02:24 · sessao 7add2b45be56 · - · - · origem=Ronald (tmux)

- **16 mutantes: mordida confirmada na execução em memória.** Rodei os dois módulos sem banco com `PYTHONDONTWRITEBYTECODE=1`: **36 testes, OK**, incluindo o harness que exige instalação da mutação, nenhum erro acidental, exatamente uma falha e a mensagem pretendida. `app/ponto/tests/test_reverter_exportada_sem_banco.py:391`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:447`, `app/ponto/tests/test_reverter_regen_sem_banco.py:89`.

- **Não identifiquei afrouxamento indevido no delta.** `concluido → laco_terminado` corrige a promessa do estado; `quem.processo=None` corresponde ao ator devolvido pelo duplo, com outro teste cobrando ator real não nulo; falha no DEPOIS agora exige auditoria ANTES e caminho/hashes no erro; arquivo recusado conserva exigência de parada e porta não chamada. `app/ponto/tests/test_reverter_exportada_sem_banco.py:221`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:223`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:247`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:261`, `app/ponto/tests/test_reverter_exportada_sem_banco.py:280`.

- **Django real verifica existência, conteúdo e resultado, mas não toda a precedência pedida.** As primeiras inspeções do JSON e da auditoria ANTES acontecem **depois de `call_command` retornar**; não há inspeção na entrada da porta. Portanto ausência total reprova, mas gravação tardia antes do retorno pode escapar. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:155`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:159`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:168`.

- **Contraexemplo estático:** deslocar a preparação para imediatamente depois da porta, antes do registro do resultado, conserva o JSON/auditoria disponíveis quando o teste os inspeciona; o comando conserva a coleção anterior de células e a porta consulta outra instância; o financeiro continua pendente no capture. Essa mutação não foi executada com Django **[nao verificado]**. `app/ponto/management/commands/aplicar_col900.py:161`, `app/ponto/management/commands/aplicar_col900.py:166`, `app/ponto/management/commands/aplicar_col900.py:184`, `app/ponto/management/commands/aplicar_col900.py:186`, `app/ponto/portas/celula.py:776`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:155`.

- **A precedência efetiva é cobrada pelo teste com duplos:** `_reverter` inspeciona JSON preparado, auditoria ANTES, hashes, DNA e fsync ao entrar na porta. `app/ponto/tests/test_reverter_exportada_sem_banco.py:181`.

- **Hash posterior: o Django real tem mordida substantiva.** Após executar os callbacks, exige mudança do hash e igualdade entre hash gravado no JSON e hash real atual; também exige DNA restaurado e `DiaPago=240`. Isso detecta foto do hash financeiro antigo no cenário testado. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:175`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:179`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:192`.

- **O teste Django real foi acrescentado conforme a cura formal do PROCEDENTE 4**, com `call_command`, `LOGS` temporário, auditoria real e capture natural dos callbacks; sua execução nesta revisão e o resultado informado de 74 testes permanecem **[nao verificado]**. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:139`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:153`.

## 10/11 02:20 · sessao b927768c6655 · FIO-E0-V4-E4A-2 · verificador · origem=Code

**A volta 7 cura os principais defeitos da triagem, mas não realiza literalmente o contador pedido no aval e deixa um leitor externo com expectativa incompatível.** O contador publica uma interseção de chamados com C5; o relatório antigo ainda soma a causa C5 sob “esperado 0”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:34-51`; `bin/placar_code.sh:317-320`)

1. **PROCEDENTES da triagem**

   - **1 — Curado no código, sem inverter a asserção antiga.** `resposta_tardia.receber()` aceita a tupla acusadora e consulta `_renascer_por_premissa_viva()`, sem excluir `nunca_bateu`. O teste anterior conserva resposta não tardia, chamado vivo e declaração chamada; o novo teste acompanha a porta real com `side_effect`. (`app/chamados/services/resposta_tardia.py:44-48`; `app/chamados/tests/test_resposta_tardia.py:70-83`; `app/chamados/tests/test_resposta_tardia.py:93-110`)
   - **2 — Fixtures corrigidas nos sítios apontados.** Há entrada efetiva antes do signal nos casos de furo; o teste de causa mantém o dia sem batidas, sustentando a adesão com um par fechado anterior. A catraca registra `1199`. (`app/chamados/tests/test_resposta_tardia.py:34-40`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:53-64`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:245-254`; `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:118-130`; `app/core/tests/test_medida_turno_do_par.py:268`)
   - **Não houve inversão ou afrouxamento das asserções desses reparos de fixture no delta fornecido.** Na obra inteira, porém, houve inversões deliberadas: testes que exigiam emissão pelo cron passaram a exigir contagem, zero emissão e alarme. Isso corresponde à conversão do comando em vigia. (`app/chamados/tests/test_resposta_tardia.py:70-83`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:282-301`; `app/chamados/management/commands/disparar_perguntas_competencia.py:225-228`)
   - **3 — Nomes curados; universo curado por pessoa, com ressalvas.** O helper agora declara apenas leitura da lâmpada; a causa também nomeia o fato. Desligado sem qualquer entrada na C5 sai da comparação e entra no balde RH; `esperado` não é simplesmente copiado de `numero`. As diferenças de universo e de unidade estão nos itens seguintes. (`app/chamados/services/fabrica_da_celula.py:67-69`; `app/chamados/services/disputa_emissao.py:757-759`; `app/chamados/management/commands/disparar_perguntas_competencia.py:38-49`; `app/chamados/management/commands/disparar_perguntas_competencia.py:125-126`)
   - **4 — Selo novo substancialmente curado.** O teste Django prepara vínculo, marco vencido e ata acusadora; exige emissão positiva para furo, zero para `nunca_bateu` e exatamente um par na C5 real. Repasse e contador também têm controles positivos. A execução Django desses selos permanece **[não verificado]**. (`app/chamados/tests/test_e4a2_nunca_bateu.py:74-100`; `app/chamados/tests/test_e4a2_nunca_bateu.py:110-142`)

2. **Aval item (2): emissão atendida; contador não literal**

   **Fábrica e vigia migraram.** A fábrica corta antes de pauta/emissão; o caminho da célula usa a mesma função de motivos; o fio mudo dá `continue` antes de acrescentar qualquer alvo; o repasse corta antes do atalho da ata. Cartório e consumidor de eventos chegam à fábrica protegida. (`app/chamados/services/fabrica_da_celula.py:85-86`; `app/chamados/services/fabrica_da_celula.py:163-164`; `app/chamados/services/fabrica_da_celula.py:222-225`; `app/chamados/services/fabrica_da_celula.py:279-281`; `app/chamados/management/commands/disparar_perguntas_competencia.py:87-89`; `app/chamados/services/disputa_emissao.py:757-761`; `app/ponto/services/cartorio.py:1006-1010`)

   A conta efetiva é:

   | Valor | O que mede |
   |---|---|
   | `numero` | Pares únicos `(colab,dia)` com chamado vivo no recorte do disparar, após barreira de desligados e limite temporal. |
   | `esperado` | Quantos desses mesmos pares aparecem na C5: `len(pares & vistos)`. |
   | `dono` | Literal `CADASTRO`. |

   (`app/chamados/management/commands/disparar_perguntas_competencia.py:34-51`; `app/chamados/management/commands/disparar_perguntas_competencia.py:78-89`)

   **Isso não é tamanho integral da C5.** A C5 não exige chamado e inclui células desde a competência anterior de cada empresa até ontem. Sua apresentação ainda agrega todas as datas em **uma assinatura C5 por colaborador**. Portanto o contador não mede nem todas as datas C5 nem todas as assinaturas/pessoas C5. (`app/escala/services/cadastro_realidade.py:177-209`)

   Consequência: um dia C5 sem chamado, ou fora do recorte corrente do disparar, não participa de nenhum dos dois números; igualdade entre eles prova inclusão dos pares selecionados, não completude da lista C5. (`app/chamados/management/commands/disparar_perguntas_competencia.py:65-89`; `app/chamados/management/commands/disparar_perguntas_competencia.py:45-47`; `app/escala/services/cadastro_realidade.py:180-200`)

   Há outra borda: **desligado com um dia conservado na C5 passa pela barreira por pessoa**, mas outro dia dele, legitimamente excluído pela lavra, pode aparecer em `fora_da_c5`. O contador testa presença do colaborador; a C5 decide retenção dia a dia. Ocorrência em produção: **[não verificado]**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:38-48`; `app/escala/services/cadastro_realidade.py:195-200`)

   Os **129 itens / 44 células / 15 colaboradores**, enquanto suas células conservarem `veredito='nunca_bateu'`, ficam fora de `fio_mudo_alvos` pelos caminhos auditados. A correspondência desses números com a população efetiva, e ALVOS zero em produção, são **[não verificado]**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:87-89`; `app/chamados/services/fabrica_da_celula.py:85-86`)

3. **Quem escreve, quem lê e qual significado**

   | Campo/valor | Escritor ou produtor | Leitores e significado |
   |---|---|---|
   | `veredito='nunca_bateu'` | Cartório deriva o resultado no ramo sem adesão e chama `lavrar_veredito`; o modelo delega à porta canônica. | Helper/fábrica, repasse, resposta tardia e C5 leem a lâmpada. A tupla acusadora continua incluindo `nunca_bateu`. |
   | Causa `nunca_bateu_sem_adesao` | Retorno de `motivo_fora_da_fabrica`. | `retencao`, `medir`, `do_dia`, `CAUSAS` e `dono`: motivo de retenção de **chamados**, dono `admin`. |
   | `fio_mudo_alvos` | `contador_fio_mudo`: quantidade de tuplas selecionadas `(colab,dia,motivo)`, esperado zero, dono `ESTRUTURA`. | Disparar imprime; vigiar publica; placar compara número e esperado. |
   | Contador `nunca_bateu_sem_adesao` | `contador_nunca_bateu`: pares únicos selecionados, interseção esperada, dono `CADASTRO`. | Disparar imprime; vigiar publica número, esperado e diagnóstico; placar compara. |
   | `barrados['desligado']` | Ramo antigo incrementa para situação diferente de `ativo`; ramo novo soma pares únicos de desligados ausentes da C5. | Relatório do disparar imprime o agregado como barreira RH. |

   Referências: veredito — `app/ponto/services/cartorio.py:1165-1173`, `app/ponto/services/cartorio.py:1237-1258`, `app/escala/models.py:1819-1828`, `app/ponto/portas/celula.py:1011-1049`, `app/escala/models.py:1768`; causa — `app/chamados/services/disputa_emissao.py:757-759`, `app/chamados/services/pergunta_no_app.py:35`, `app/chamados/services/pergunta_no_app.py:99-101`, `app/chamados/services/pergunta_no_app.py:139-194`; contadores/barreiras — `app/chamados/management/commands/disparar_perguntas_competencia.py:38-51`, `app/chamados/management/commands/disparar_perguntas_competencia.py:111-129`.

   **Carimbo:** `vigiar_fio_morto` chama o contador, copia seus valores para `censo` e `esperado`, guarda o diagnóstico C5 completo e grava pela `_carimbar`. Somente a execução sem recorte de empresas publica o carimbo de frota. (`app/chamados/management/commands/vigiar_fio_morto.py:118-137`; `app/chamados/management/commands/vigiar_fio_morto.py:181-182`; `app/chamados/management/commands/vigiar_fio_morto.py:225-248`)

   **Placar:** lê o carimbo, exige o mesmo dia civil e inteiros, compara `numero != esperado` e produz `NAO_MEDIDO` para ausência/forma inválida. Não recalcula C5 nem alvos. (`app/core/management/commands/pipeline_placar.py:85-107`; `app/core/management/commands/pipeline_placar.py:135-136`)

   **ESMERIL semântico:** causa e contador têm o mesmo nome, mas unidades diferentes: chamados retidos versus pares únicos com recorte. Não são números intercambiáveis. O dia do fato vem da célula/catálogo; `hoje` do carimbo identifica o dia civil local da medição. A janela padrão do contador é global, enquanto a C5 usa janela por empresa. (`app/chamados/services/pergunta_no_app.py:78-84`; `app/chamados/services/pergunta_no_app.py:151-173`; `app/chamados/management/commands/disparar_perguntas_competencia.py:65-89`; `app/escala/services/cadastro_realidade.py:180-183`; `app/chamados/management/commands/vigiar_fio_morto.py:166-167`)

4. **Os testes mordem?** [nao verificado]

   **Os controles centrais deixaram de passar por cenário vazio:** exigem marco vencido, ata acusadora, emissão positiva da fábrica/repasse e alvo positivo do contador antes dos casos negativos. (`app/chamados/tests/test_e4a2_nunca_bateu.py:94-100`; `app/chamados/tests/test_e4a2_nunca_bateu.py:110-142`)

   Os oito mutantes de `nunca_bateu` e os oito do placar executam o controle antes da mutação, exigem alvo textual único e aceitam somente `AssertionError` com mensagem pretendida. Um erro genérico de setup não satisfaz esse contrato. **A execução Django de cada mutante é [não verificado].** (`app/chamados/tests/test_e4a2_nunca_bateu.py:33-56`; `app/chamados/tests/test_e4a2_nunca_bateu.py:201-216`; `app/core/tests/test_fio_mudo_placar.py:274-302`)

   Foram executados em memória dez verificadores puros de fonte, relógio, relatório e SQL, com suas mutações, sem falhas; isso não substitui os testes Django. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:75-107`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:162-171`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:221-228`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:265-288`)

   **Lacunas:** a coleção do placar não muta especificamente as duas publicações C5 nem a chamada do leitor C5; existe teste normal dessas publicações. O novo caso de desligado cobre exclusão total, sem testar o contador com retenção parcial por lavra. (`app/core/tests/test_fio_mudo_placar.py:34-45`; `app/core/tests/test_fio_mudo_placar.py:139-146`; `app/chamados/tests/test_e4a2_nunca_bateu.py:155-169`)

   Resultado informado “Ran 1693, OK …”: **[não verificado]**.

5. **Classes de reprovação anteriores**

   - **Segundo juiz/escritor novo:** não identificado nos trechos alterados; o helper lê a lâmpada, o contador consulta a C5 real, a fábrica conserva a emissão e a resposta tardia usa a porta existente. O contador não rederiva adesão nem retenção da folha; deriva seu recorte adicional. (`app/chamados/services/fabrica_da_celula.py:67-69`; `app/chamados/services/fabrica_da_celula.py:197-199`; `app/chamados/management/commands/disparar_perguntas_competencia.py:31-47`; `app/chamados/services/resposta_tardia.py:45-48`)
   - **Relógio fora da borda: presente, herdado.** `_ja_perguntou` não recebe `hoje`, chama `fio_vivo`, que chama `pergunta_viva` sem o dia injetado; o caminho pode chegar ao `date.today()`. A leitura direta nova usa `hoje`, mas essa segunda consulta continua divergente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`; `app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/services/disputa_emissao.py:1421-1422`; `app/chamados/juizes.py:1434`; `app/chamados/juizes.py:577`)
   - **Campo com dois significados: presente nos relatórios**, pelas unidades distintas de causa e contador e pela expectativa zero do leitor externo. (`app/chamados/services/pergunta_no_app.py:170-173`; `app/chamados/management/commands/disparar_perguntas_competencia.py:46-47`; `bin/placar_code.sh:319-320`)
   - **Ordem da tranca:** o contador novo está dentro do contexto de lote no vigiar; isso não comprova toda ordem de locks em execução concorrente, que fica **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:174-182`; `app/chamados/juizes.py:1108-1118`)
   - **Pouso após pré-requisitos:** a dependência e a condição ALVOS zero estão registradas; atendimento operacional e pouso efetivo são **[não verificado]**. (`app/docs/BACKLOG.md:1871`)

6. **Leitores fora do diff**

   **Sim: `bin/placar_code.sh` precisa migrar sua expectativa.** Ele soma dinamicamente todas as causas, incluindo a nova causa C5, e publica o total como “pergunta fora do app; esperado 0”. Isso não cria `fio_mudo_alvos`, mas apresenta retenção legítima de cadastro sob uma expectativa incompatível. O comando fornecedor também mantém essa descrição antiga no cabeçalho/help. (`bin/placar_code.sh:314-320`; `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:1-2`; `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:10`)

   **`bin/fio_estado.py` já comporta os novos campos:** percorre genericamente o `esperado` do carimbo, compara com `censo` e mostra divergências pelo nome recebido. Ele não impõe zero próprio aos campos novos. (`bin/fio_estado.py:121-126`; `bin/fio_estado.py:191-194`)

## 10/11 02:19 · sessao 9a51ecf2e292 · REVERTER-DIA-SEM-RECALCULO · construtor · origem=Code

Entreguei alterações em três arquivos:

- [aplicar_col900.py:35](/home/ronald/wt-codex-reverter-dia-sem-recalculo-021142/app/ponto/management/commands/aplicar_col900.py:35): auditoria do ANTES obrigatória, ator copiado do registro devolvido, `fsync` do diretório, preparação dentro do protocolo e conclusão via `on_commit`. Falhas na conclusão levantam `SystemExit` com caminho e hashes. Estados finais: `laco_terminado` ou `interrompido`.
- [test_reverter_exportada_sem_banco.py:31](/home/ronald/wt-codex-reverter-dia-sem-recalculo-021142/app/ponto/tests/test_reverter_exportada_sem_banco.py:31): contador AST ampliado, remoção do `__main__`, REDs e mutantes coletáveis em memória.
- [test_reverter_regen_recalcula_dinheiro.py:139](/home/ronald/wt-codex-reverter-dia-sem-recalculo-021142/app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:139): integração com `call_command`, exportação, JSON temporário, LogAuditoria/hash reais e `captureOnCommitCallbacks(execute=True)`.

O escritor físico do JSON continua único: `_gravar_reversao`. A conclusão lê o dicionário em memória. (`ponto/management/commands/aplicar_col900.py:35`, `:97`)

| Campos | Quem escreve/produz | Quem lê; significado |
|---|---|---|
| `colab`, `dias`, `quando`, `motivo`, `apesar_da_lavra` | `_preparar_reversao`, dos argumentos e relógio. (`aplicar_col900.py:68`) | Auditorias anterior/posterior. (`aplicar_col900.py:84`, `:111`) |
| `quem.processo`, `quem.usuario_so` | Processo vem de `log.ator`; usuário SO vem de `getpass`. (`aplicar_col900.py:88`) | Auditoria posterior. Nulo é preservado, sem fallback. (`aplicar_col900.py:108`) |
| `competencias[].mes/ano/lavrada` | Competência delegada a `competencia_do_fato`; lavrada delegada ao K9. (`aplicar_col900.py:63`, `:73`) | Conclusão usa mês/ano; K9 significa exportação vigente ou holerite publicado. (`aplicar_col900.py:100`; `folha/export.py:610`) |
| `hash_antes`, `hash_depois`, `valores_antes` | Helpers existentes leem FechamentoMensal; comando grava os retornos. (`ponto/portas/regen_exportada.py:62`, `:77`; `aplicar_col900.py:74`, `:100`) | Auditorias e diagnóstico de falha. Valores seguem `CAMPOS`, não constituem cópia integral de DiaPago. (`aplicar_col900.py:109`, `:119`; `ponto/management/commands/aplicar_09_corte_b.py:67`) |
| `celulas` | Fotografia de `CelulaDia.dna`. (`aplicar_col900.py:76`) | Teste integrado confere a fotografia. (`test_reverter_regen_recalcula_dinheiro.py:164`) Consumidor produtivo de restauração: **[nao verificado]**. | [nao verificado]
| `estado`, `registrado_em` | `_concluir_reversao`. (`aplicar_col900.py:101`) | Auditoria posterior. `laco_terminado` mede término do laço; recálculo continua sujeito ao tratamento existente de falhas. (`aplicar_col900.py:113`; `ponto/services/fechamento.py:1818`) |
| `resultados[].dia/revertido/resultado` | Laço operacional. (`aplicar_col900.py:179`, `:187`) | Auditoria posterior. (`aplicar_col900.py:113`) |
| LogAuditoria: ação, modelo, objeto, descrição e payloads | Comando monta; `evento_kw → evento` persiste pelo escritor único. (`aplicar_col900.py:79`, `:105`; `core/observ.py:126`, `:133`) | Tela genérica consulta os registros. (`core/views.py:705`) |

Permanece o leitor histórico que espera `desde/ate` na auditoria da regeneração e ignora o payload da VOLTA; está fora desta alteração. (`escala/services/regeneracao.py:58`; `ponto/portas/celula.py:799`) Outro consumidor produtivo atribuindo significado diferente ao novo JSON: **[nao verificado]**.

Comandos executados e saídas locais, sem relatório persistido **[nao verificado por arquivo de resultado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_reverter_exportada_sem_banco \
  ponto.tests.test_reverter_regen_sem_banco -v
# Ran 36 tests — OK
```

Os 16 mutantes do protocolo compartilham este comando; a campanha exige a asserção pretendida e rejeita erros acidentais. (`ponto/tests/test_reverter_exportada_sem_banco.py:404`, `:447`)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_reverter_exportada_sem_banco.MutantesProtocoloTest -v
```

Casos: `sem_pre`, `ignora_io`, `sem_antes`, `sem_pos`, `hash_falso`, `sem_valores`, `sem_dna`, `sem_depois`, `ignora_juiz`, `foto_em_aberta`, `sem_fsync_diretorio`, `ator_forjado`, `estado_ambiguo`, `antes_do_commit`, `erro_sem_hash`, `preparar_fora_try`. (`ponto/tests/test_reverter_exportada_sem_banco.py:404`)

Posicional, aliases e `**kwargs` estão no teste de segundo chamador. (`ponto/tests/test_reverter_exportada_sem_banco.py:65`)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_reverter_exportada_sem_banco.ChamadorUnicoTest -v
```

RED contra a base:

```bash
GIT_OPTIONAL_LOCKS=0 git show \
  e948e19b9c3e2a10c03cb2688210f830f0bd18dd:app/ponto/management/commands/aplicar_col900.py |
PYTHONDONTWRITEBYTECODE=1 python3 -c '
import sys, unittest
from ponto.tests import test_reverter_exportada_sem_banco as t
t.FONTE_COMANDO = sys.stdin.read()
suite = unittest.TestSuite(t.TrilhaComandoTest(n) for n in ( [nao verificado]
    "test_RED_json_antes_da_porta_duas_competencias_e_trilha_depois",
    "test_preparacao_dentro_do_protocolo"))
r = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
sys.exit(not r.wasSuccessful())
'
```

Saída: duas falhas esperadas — “falta auditoria ANTES da porta” e “preparacao fora do protocolo”; retorno 1. **[nao verificado por arquivo de resultado]**

Ruff disponível, versão 0.17.4, retornou `All checks passed!` nos três arquivos; `git diff --check` sem saída. **[nao verificado por arquivo de resultado]**

Não executei os testes Django, a suite completa ou o opus: Django/freezegun estão ausentes neste Python e não houve acesso a banco. Integração real e certificação permanecem **[nao verificado]**. [nao verificado]

## 10/11 02:19 · sessao 443fa65a7205 · - · - · origem=Ronald (tmux)

Achados para os itens 2, 3 e 6:

- **Literal “esperado = tamanho da lista C5” não realizado.** `numero=len(P)` e `esperado=len(P∩C5)`, onde P reúne pares únicos `(colab,dia)` com chamado vivo na janela do disparar, dentro do piso da anterior por empresa até ontem. Dias C5 sem chamado, anteriores à janela do disparar ou de outros colaboradores quando há `--colab` não entram no esperado. C5 inteira cobre anterior por empresa até ontem; seu formatador cria uma assinatura C5 por colaborador. Portanto número de chamados, pares/dias C5 e assinaturas/pessoas C5 são três unidades distintas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:34`, `:42`, `:46`, `:47`, `:78`, `:81`, `:87`; `app/escala/services/cadastro_realidade.py:180`, `:197`, `:204`, `:208`)

- **A fonte C5 é consultada, sem copiar adesão/lavra de folha.** O contador chama `colabs_nunca_bateu`; a regra específica de retenção de dias dos desligados continua somente em C5. O contador consulta presença do colaborador como chave C5, depois confronta seus pares. (`app/chamados/management/commands/disparar_perguntas_competencia.py:31`, `:34`, `:38`, `:45`; `app/escala/services/cadastro_realidade.py:185`, `:191`, `:195`, `:198`)

- **Mesmo nome, significados distintos e leitores distintos.** A causa `nunca_bateu_sem_adesao` é produzida por `motivo_fora_da_fabrica`, descreve o fato da lâmpada e tem dono `admin` no catálogo; `medir` conta chamados de cobrança ativos, sem limitação à janela do contador, um item por chamado. O contador homônimo conta pares deduplicados em recorte temporal e tem dono `CADASTRO`. Não localizei troca entre os dois namespaces, mas não são medidas equivalentes. (`app/chamados/services/disputa_emissao.py:757`, `:759`; `app/chamados/services/pergunta_no_app.py:35`, `:139`, `:151`, `:167`, `:169`, `:172`; `app/chamados/management/commands/disparar_perguntas_competencia.py:44`, `:46`, `:47`)

Mapa de escrita/leitura:

| Campo/valor | Escreve/produz | Lê |
|---|---|---|
| helper da lâmpada | `app/chamados/services/fabrica_da_celula.py:67`–`:69` | fábrica `:85`, `:163`; repasse `app/chamados/services/disputa_emissao.py:758`; contador `app/chamados/management/commands/disparar_perguntas_competencia.py:87` |
| causa | `app/chamados/services/disputa_emissao.py:759` | `retencao` `app/chamados/services/pergunta_no_app.py:99`; `medir` `:167`; `do_dia` `:189`; texto/dono `:193`–`:194`; command `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:19`; cobrança manual `app/chamados/services/cobrar_dia.py:46`, `:59`; dia do colaborador `app/ponto/services/dia_do_colab.py:101`, `:108` |
| `fio_mudo_alvos` número/esperado | seleção `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `:127` | relatório `:191`, `:198`; vigia `app/chamados/management/commands/vigiar_fio_morto.py:181`, `:208`; carimbo `:231`, `:239`; pipeline `app/core/management/commands/pipeline_placar.py:85`, `:98`, `:99`, `:135` |
| contador C5 número/esperado | `app/chamados/management/commands/disparar_perguntas_competencia.py:46`–`:51` | relatório `:193`–`:195`; vigia `app/chamados/management/commands/vigiar_fio_morto.py:182`, `:210`; carimbo `:232`, `:239`, `:240`; pipeline `app/core/management/commands/pipeline_placar.py:136` |
| `barrados['desligado']` | inicializa `app/chamados/management/commands/disparar_perguntas_competencia.py:74`; ramo antigo `:111`–`:112`; soma novos pares `:126` | relatório `:200`–`:204` |
| fotografia em disco | `_carimbar`, `app/chamados/management/commands/vigiar_fio_morto.py:118`, `:135`, `:137` | pipeline `app/core/management/commands/pipeline_placar.py:92`; painel genérico `bin/fio_estado.py:121`, `:125`, `:192`; ESTADO via `bin/gerar_estado.py:260`–`:261` |

**Leitor fora do diff com expectativa antiga:** `bin/placar_code.sh` lê dinamicamente todas as causas, soma seus números e mantém o rótulo “esperado 0”. Logo uma retenção C5 legítima continua contribuindo para esse total esperado zero; isso não a transforma em `fio_mudo_alvos`, mas é uma expectativa diferente da comparação dinâmica C5. O command também mantém a descrição “esperado 0, dono sistema”, embora publique corretamente os donos por causa. (`bin/placar_code.sh:317`–`:320`; `app/chamados/management/commands/chamados_vivos_sem_pergunta_no_app.py:1`, `:10`, `:17`, `:20`)

**`bin/fio_estado.py` não precisa de migração nominal:** itera todas as chaves de `esperado`, compara com `censo` e mostra todas as divergências. Os novos campos entram automaticamente; quando iguais não têm linha própria nem dono apresentado, e o bloco diagnóstico completo `nunca_bateu_sem_adesao` é ignorado. (`bin/fio_estado.py:125`–`:126`, `:143`–`:162`, `:191`–`:194`)

**Relógio e universos:**

- Vigia captura um instante, deriva dia civil local, passa ambos ao contador e só publica carimbo com `empresas is None`; recorte empresarial não sobrescreve fotografia de frota. (`app/chamados/management/commands/vigiar_fio_morto.py:166`–`:181`, `:223`–`:228`, `:246`–`:248`)
- Pipeline compara esse dia civil com o dia solicitado e compara número/esperado inteiro; porém sua borda herdada usa `date.today()` e duas leituras `datetime.now()`, sem `timezone.localdate`. Divergência efetiva de fuso/virada em produção: [não verificado]. (`app/core/management/commands/pipeline_placar.py:94`–`:105`, `:148`–`:150`)
- Fio mudo usa dia da célula/turno, enquanto retenção usa a data canônica do módulo; causas não usam data civil de conversa como fato do turno. (`app/chamados/management/commands/disparar_perguntas_competencia.py:78`, `:92`; `app/chamados/services/pergunta_no_app.py:78`–`:82`, `:114`–`:116`; `app/chamados/services/disputa_emissao.py:1413`–`:1415`)
- Janela default do contador é global, sem empresa; C5 e piso da comparação são por empresa. Essa interseção é declarada, mas não equivale ao universo integral C5. (`app/chamados/management/commands/disparar_perguntas_competencia.py:42`, `:57`, `:65`–`:67`; `app/escala/services/cadastro_realidade.py:177`–`:182`)
- Relógio herdado fora da borda permanece: contador → `_ja_perguntou` → `fio_vivo` → `pergunta_viva` sem `hoje` → fallback `date.today()`. É a fila já declarada pela triagem, não cura desta volta. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/services/disputa_emissao.py:1421`–`:1422`; `app/chamados/juizes.py:1434`, `:577`)
- `barrados['desligado']` mistura ramo antigo `situacao != ativo` por passagem/candidato com novos pares únicos estritamente `desligado` sem chave C5; o novo barramento ocorre antes do corte temporal. O rótulo RH continua sendo impresso, mas a unidade não é uniforme. (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`–`:43`, `:49`, `:111`–`:112`, `:126`, `:200`)

O corte `continue` impede toda célula cuja lâmpada ainda seja `nunca_bateu` de chegar à inclusão em `_mudos`; fábrica e repasse também cortam antes de emitir. A pertença efetiva dos 129 itens/44 células/15 colaboradores, ou sua quantidade após relavra/renascimento, permanece [não verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:87`–`:89`, `:124`; `app/chamados/services/fabrica_da_celula.py:85`–`:86`, `:163`–`:164`; `app/chamados/services/disputa_emissao.py:758`–`:759`)

## 10/11 02:19 · sessao 14fde6cf8cbf · - · - · origem=Ronald (tmux)

- **PROCEDENTE 1 curado:** `receber()` voltou a consultar a porta `_renascer_por_premissa_viva()` para qualquer `VEREDITOS_ACUSAM`, sem corte de `nunca_bateu` (`app/chamados/services/resposta_tardia.py:45`). O teste antigo conserva HTTP 200, resposta não tardia, chamado vivo e declaração chamada; o novo caso verifica a porta real com `side_effect` (`app/chamados/tests/test_resposta_tardia.py:70`, `app/chamados/tests/test_resposta_tardia.py:93`).

- **PROCEDENTE 2 curado nas fixtures examinadas:** entrada efetiva antes do signal em resposta tardia e fio mudo; adesão anterior com par fechado mantém o dia do teste de causa sem batidas (`app/chamados/tests/test_resposta_tardia.py:34`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:53`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:245`, `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:118`). Catraca registra `1199` (`app/core/tests/test_medida_turno_do_par.py:268`). A comparação com o delta fornecido não mostra inversão das asserções desses casos; na obra inteira há inversões intencionais da emissão do cron para zero, coerentes com sua conversão em vigia (`app/chamados/tests/test_selo_emissor_fio_mudo.py:282`).

- **PROCEDENTE 4 substancialmente curado:** teste Django monta vínculo real, Batida somente para furo, marco vencido e ata acusadora; controle furo emite e `nunca_bateu` entrega zero perguntas e exatamente um par C5 (`app/chamados/tests/test_e4a2_nunca_bateu.py:74`, `app/chamados/tests/test_e4a2_nunca_bateu.py:94`, `app/chamados/tests/test_e4a2_nunca_bateu.py:110`). Repasse e contador usam chamadas reais com controles positivos (`app/chamados/tests/test_e4a2_nunca_bateu.py:119`, `app/chamados/tests/test_e4a2_nunca_bateu.py:133`).

- **Os oito mutantes C5 têm controle antes da mutação e exigem `AssertionError` com mensagem específica**, dificultando aceitar erro de setup como mordida (`app/chamados/tests/test_e4a2_nunca_bateu.py:33`, `app/chamados/tests/test_e4a2_nunca_bateu.py:201`). A coleção do placar faz o mesmo para oito mutantes (`app/core/tests/test_fio_mudo_placar.py:34`, `app/core/tests/test_fio_mudo_placar.py:274`). Execução desses testes Django e de cada mutante: **[não verificado]**.

- **Verificação executada sem escrita:** dez métodos puros extraídos em memória — fonte, relógio, relatório e coletor SQL — passaram, incluindo suas mutações; isso verifica os métodos das linhas indicadas e não equivale à suíte Django integral (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:75`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:162`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:221`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:265`).

- **Lacunas concretas:** mutantes específicos de publicação/leitura do contador C5, presentes no arquivo removido fornecido pelo usuário, não foram repostos na coleção do placar; a coleção atual muta `fio_mudo_alvos`, embora o teste normal confira publicação/divergência C5 (`app/core/tests/test_fio_mudo_placar.py:34`, `app/core/tests/test_fio_mudo_placar.py:139`). O novo teste de desligado cobre somente o excluído da C5; não cobre sua conservação pela lavra integrada ao contador (`app/chamados/tests/test_e4a2_nunca_bateu.py:155`). Há cobertura anterior da própria C5 com lavra (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:97`).

- **Vazio:** os novos controles centrais exigem emissão/alvo positivos e ata não vazia, impedindo aprovação apenas por ausência de cenário (`app/chamados/tests/test_e4a2_nunca_bateu.py:95`, `app/chamados/tests/test_e4a2_nunca_bateu.py:113`, `app/chamados/tests/test_e4a2_nunca_bateu.py:135`). O veto de DML também exige um alvo em cada caminho e verifica callbacks; seus controles demonstram UPDATE imediato e adiado sem alteração de contagens (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:305`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:334`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:352`). Resultado integral informado de 1693 testes: **[não verificado]**.

## 10/11 02:18 · sessao de42b151306e · - · - · origem=Ronald (tmux)

- **Aval literal do contador não realizado.** `numero` é a quantidade deduplicada de pares `(colab,dia)` provenientes de chamados vivos no recorte do disparar, limitada por início da competência anterior da empresa e ontem; `esperado` é apenas a interseção desses pares com a C5, dono `CADASTRO`. Não é tamanho integral da C5 (`app/chamados/management/commands/disparar_perguntas_competencia.py:22-48`, especialmente 34,38-47). A C5 busca células sem exigir chamado, desde a anterior por empresa até ontem (`app/escala/services/cadastro_realidade.py:177-200`). Além disso, a lista de apresentação gera **uma assinatura C5 por colaborador**, agregando suas datas no detalhe (`app/escala/services/cadastro_realidade.py:204-209`). Convém explicitar se “tamanho da lista C5” significa pessoas/assinaturas ou dias; o contador mede dias-colaborador selecionados por chamados, não qualquer dessas duas totalidades.

- **Universo de desligados foi ajustado por pessoa, mas conserva uma borda problemática por dia.** O contador exclui desligado somente se `colab.pk not in c5` (`app/chamados/management/commands/disparar_perguntas_competencia.py:38-40`); a C5 real inclui/exclui **cada dia**, pela lavra de `furo_espelho` (`app/escala/services/cadastro_realidade.py:185-200`). Portanto, desligado com um dia retido na C5 pode ter outro dia legitimamente excluído pela C5 contado em `numero`, ausente em `esperado` e listado como divergência. Trata-se de consequência estática desses predicados, não ocorrência demonstrada em produção [não verificado]. O teste novo cobre desligado totalmente fora da C5, não esse caso misto (`app/chamados/tests/test_e4a2_nunca_bateu.py:145-160`).

- **Sem rederivação de adesão/lavra da folha dentro do contador.** Ele pergunta à função canônica `colabs_nunca_bateu` e monta conjuntos para comparar o próprio recorte (`app/chamados/management/commands/disparar_perguntas_competencia.py:31-47`). O piso por empresa é calculado separadamente por `janela_anterior`; isso é recorte adicional do contador, não reprodução da regra de retenção da C5 (`app/chamados/management/commands/disparar_perguntas_competencia.py:41-44`).

- **Os dois caminhos do disparar excluem células que ainda tenham `veredito='nunca_bateu'`.** No fio mudo, o `continue` antecede todas as adições a `_mudos` (`app/chamados/management/commands/disparar_perguntas_competencia.py:87-89,124`). No caminho da célula, o vigia delega a `motivos_a_perguntar`, que retorna vazio para esse veredito (`app/chamados/services/fabrica_da_celula.py:85-86,214-225`). Assim, por esses caminhos, aqueles fatos não viram `fio_mudo_alvos` enquanto conservarem esse veredito. Correspondência dos 129 itens/44 células/15 colaboradores com a cópia efetiva [não verificado].

- **Fábrica e repasse migrados.** `perguntar_pela_celula` corta `nunca_bateu` antes de pauta e emissão (`app/chamados/services/fabrica_da_celula.py:163-164`). O helper apenas lê a lâmpada, sem afirmar pertencimento à lista (`app/chamados/services/fabrica_da_celula.py:67-69`). O repasse retorna causa `nunca_bateu_sem_adesao` antes do atalho da ata (`app/chamados/services/disputa_emissao.py:757-761`); o predicado de regularização delega a essa função (`app/chamados/services/disputa_emissao.py:790-796`) e é aplicado nos dois ramos do repasse (`app/chamados/services/disputa_emissao.py:1065-1067,1086-1088`). Os caminhos cartório e consumidor terminam na mesma fábrica (`app/ponto/services/cartorio.py:1006-1010`; `app/chamados/services/fabrica_da_celula.py:279-281`).

- **Resposta tardia curada na porta existente.** A condição voltou a aceitar qualquer veredito da tupla acusadora e chama `_renascer_por_premissa_viva`, sem corte para nunca_bateu (`app/chamados/services/resposta_tardia.py:44-48`). A porta conserva situação ativa, cascade, tranca, veto e o gate de `reabrir` (`app/chamados/models.py:1004-1024`).

- **Escritor de veredito e origem do fato.** O cartório lê a primeira Batida não retratada para adesão (`app/ponto/services/cartorio.py:1165-1173`); no ramo sem adesão, julga a precedência, renomeia para `nunca_bateu` quando não há cobertura integral e chama `cel.lavrar_veredito` (`app/ponto/services/cartorio.py:1237-1258`). O método do modelo delega à porta (`app/escala/models.py:1819-1828`); a porta aplica a precedência do admin e grava veredito, via, instante, protestos e ata (`app/ponto/portas/celula.py:1011-1049`). A obra acrescentou leitura desse resultado, não novo juiz de adesão (`app/chamados/services/fabrica_da_celula.py:67-69`).

- **Segundo escritor/juiz novo nos trechos auditados: não identificado.** O disparar apenas retorna contagens e o helper lê veredito (`app/chamados/management/commands/disparar_perguntas_competencia.py:125-129`; `app/chamados/services/fabrica_da_celula.py:67-69`). O escritor de perguntas continua `perguntar`, usando as portas de disputa e geração (`app/chamados/services/fabrica_da_celula.py:140-155,197-199`). Correção integral de todas as ordens de trava e pouso efetivo após pré-requisitos [não verificado].

_3339 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
