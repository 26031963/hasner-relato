# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 19:27** · publicado em **06/11/2026 19:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 19:27 · sessao 23b336eb4620 · OTIMIZA-3 · verificador · origem=Code

**REPROVA: a classe “alvo carregado por loader continua com zero” permanece aberta.** Há contraexemplos que deixam **arquivo e símbolo com zero**, sem sequer registrar opacidade. O contrato trata esses zeros como afirmações. (`bin/quem_chama.py:8`, `bin/quem_chama.py:554`, `bin/quem_chama.py:684`, `bin/quem_chama.py:703`, `bin/quem_chama.py:1113`)

As reproduções abaixo foram feitas em memória. Execução integral do selo, histórico Git e comportamento em produção: **[não verificado]**.

**1. BLOQUEIA — P1(h): o valor completo montado é calculado, mas descartado.**

Para um alvo hipotético `app/pkg/mod.py::f`, estes contraexemplos deixam **ambas as facetas com zero**:

```python
importlib.import_module('pkg.' + 'mod').f()
runpy.run_path('app/pkg/mo' + 'd.py')
```

`_prefixo` calcula o resultado completo e devolve `inteiro=True`. O consumidor de caminho apenas marca `literal=True`; o de nome só acrescenta nomes completos nos casos relativo/fromlist. Depois, a recuperação lexical procura um literal completo que essas expressões não contêm. (`bin/quem_chama.py:554`, `bin/quem_chama.py:684`, `bin/quem_chama.py:703`, `bin/quem_chama.py:775`, `bin/quem_chama.py:779`)

O comportamento das bordas pedidas é:

| Prefixo | Resultado |
|---|---|
| Vazio | Os produtores descartam; nome vira opaco, sem proteger o alvo. Chamar diretamente `sob_prefixo('nome', '')` ligaria todos; caminho vazio não liga nenhum. (`bin/quem_chama.py:507`, `bin/quem_chama.py:509`, `bin/quem_chama.py:686`, `bin/quem_chama.py:695`) |
| Um caractere | É aceito. Nome `'a'` alcança todos os módulos pela alternativa `'app.' + modulo`; caminho `'a'` alcança todos os caminhos iniciados por `app`. (`bin/quem_chama.py:507`, `bin/quem_chama.py:508`, `bin/quem_chama.py:513`) |
| Sem separador final | É prefixo textual: `'pkg'` também alcança `pkgx`; `'app/pkg'` também alcança `app/pkgx`. (`bin/quem_chama.py:507`, `bin/quem_chama.py:513`) |
| Com separador final | `'pkg.'` delimita o nome; `partes.append('')` preserva a barra de `'app/pkg/'`, excluindo `app/pkgx/`. (`bin/quem_chama.py:507`, `bin/quem_chama.py:510`) |

Há outro falso zero concreto:

```python
APP_DIR = 'app'
leaf = 'mod' + '.py'
runpy.run_path(os.path.join(APP_DIR, 'pkg', leaf))
```

A raiz variável é pulada, produzindo `pkg/`. Esse prefixo não corresponde ao caminho armazenado `app/pkg/mod.py`; o sítio é considerado resolvido e não entra em OPACO. (`bin/quem_chama.py:569`, `bin/quem_chama.py:572`, `bin/quem_chama.py:513`, `bin/quem_chama.py:686`)

**2. BLOQUEIA — P1(a,b): `getattr` imediato foi corrigido; aliases atribuídos continuam invisíveis.**

Funcionam o `getattr(importlib, 'import_module')(literal)`, o apelido de `from importlib import import_module as im` e atributos como `il.import_module`. A identificação usa o `getattr` imediato, os `ImportFrom` e o último atributo. (`bin/quem_chama.py:600`, `bin/quem_chama.py:602`, `bin/quem_chama.py:607`)

Este contraexemplo mantém o símbolo com zero e não registra OPACO:

```python
im = getattr(importlib, 'import_module')
im('pkg.mod').f()
```

O mesmo ocorre com `g = getattr; g(importlib, 'import_module')('pkg.mod')`: atribuições de funções não alimentam o mapa de apelidos. (`bin/quem_chama.py:600`, `bin/quem_chama.py:604`, `bin/quem_chama.py:610`)

O prefixo literal `'app.'` foi acrescentado às chaves de loader, às regex textuais e aos pontos de entrada. Portanto `import_module('app.pkg.mod')` passa a ligar `app/pkg/mod.py`. (`bin/quem_chama.py:450`, `bin/quem_chama.py:451`, `bin/quem_chama.py:455`, `bin/quem_chama.py:780`)

Entretanto, o leitor de imports AST conserva a comparação antiga. O contraexemplo `import app.pkg.mod as m; m.f()` não produz achado para esse alvo: os dois ramos de import comparam somente `alvo['modulo']`, sem consultar as novas chaves. (`bin/quem_chama.py:747`, `bin/quem_chama.py:753`)

**3. BLOQUEIA — P1(c,d): builtin e subprocesso têm lacunas que o aviso textual não cobre.**

| Mecanismo | Liga | Deixa escapar |
|---|---|---|
| `exec`/`compile` AST | Nomes simples e aliases de `ImportFrom`; caminhos literais ou prefixos reconhecidos. (`bin/quem_chama.py:106`, `bin/quem_chama.py:612`, `bin/quem_chama.py:618`) | `builtins.exec` e `builtins.compile`: qualquer atributo com esses nomes é excluído, junto com `re.compile`. (`bin/quem_chama.py:603`) |
| Texto `.sh/config/string` | Os loaders de `LOADERS_CAMINHO`; o conjunto expandido do AST não alimenta a regex textual. (`bin/quem_chama.py:111`, `bin/quem_chama.py:487`) | Python embutido usando `exec(open(...).read())`. (`bin/quem_chama.py:487`, `bin/quem_chama.py:877`) |
| Python por subprocesso | A presença de string reconhecida por `_RX_PYTHON` ou da cadeia exata `sys.executable` ativa a associação lexical de caminhos do arquivo. (`bin/quem_chama.py:666`, `bin/quem_chama.py:669`, `bin/quem_chama.py:775`) | Não examina a expressão do argumento do script nem resolve aliases de `sys`. (`bin/quem_chama.py:666`, `bin/quem_chama.py:674`) |

Contraexemplos reproduzidos:

```python
builtins.exec(open('app/pkg/mod.py').read(), G)
G['f']()                              # símbolo zero; OPACO vazio

subprocess.run(['python3', 'app/pkg/mo' + x])
# x == 'd.py': arquivo e símbolo zero; OPACO vazio

import sys as s
subprocess.run([s.executable, 'app/pkg/mod.py'])
# símbolo zero; OPACO vazio
```

Os resultados decorrem, respectivamente, da exclusão de atributos builtin, da ausência de análise dos argumentos de subprocesso e da exigência da cadeia exata `sys.executable`. (`bin/quem_chama.py:603`, `bin/quem_chama.py:666`, `bin/quem_chama.py:669`, `bin/quem_chama.py:682`)

Também permanece zero para o símbolo no shell:

```sh
python3 -c "exec(open('app/pkg/mod.py').read())"
```

A citação protege o arquivo; não há marcador LOADER nem import reconhecido para proteger o símbolo. Isso é execução Python embutida, embora `exec cat arquivo.py` de shell deva continuar excluído. (`bin/quem_chama.py:111`, `bin/quem_chama.py:872`, `bin/quem_chama.py:875`, `bin/quem_chama.py:877`)

**4. BLOQUEIA — P1(e): o ponto fixo exclui arquivos necessários antes de começar.**

O invólucro externo de um nível da fixture funciona. Porém `cand` exige que o arquivo contenha um token de loader conhecido **antes** da descoberta de invólucros. Um segundo arquivo que apenas chama o primeiro fica fora das três rodadas. (`bin/tests/test_quem_chama.sh:308`, `bin/tests/test_quem_chama.sh:318`, `bin/quem_chama.py:1038`, `bin/quem_chama.py:1041`)

Contraexemplo:

```python
# one.py
def one(path):
    return run_path(path)

# two.py
from one import one
def two(path):
    return one(path)

# caller.py
two('app/pkg/mod.py')
```

O símbolo do alvo continua com zero. Também escapam o alias `from one import one as x; x(caminho)` e uma cadeia de quatro níveis, mesmo fazendo todos os arquivos passarem pelo filtro textual. (`bin/quem_chama.py:612`, `bin/quem_chama.py:1039`, `bin/quem_chama.py:1041`)

Além disso, o retorno guarda apenas `nome_curto → tipo`. Duas funções `load`, em arquivos diferentes, uma de caminho e outra de nome, colidem em `novo_.update`; o chamador pode receber o tipo errado e manter zero. (`bin/quem_chama.py:639`, `bin/quem_chama.py:1044`)

**5. BLOQUEIA — P1(f,g): atribuições e relativos não compõem suas resoluções.**

`_valores` acompanha somente `Assign` cujo destino seja `Name`, por até dois saltos. Não acompanha `AnnAssign`, atributos, desempacotamento nem valores internos de expressões. O `package=` relativo não passa por `_valores`. (`bin/quem_chama.py:653`, `bin/quem_chama.py:658`, `bin/quem_chama.py:714`)

Contraexemplos com alvo efetivamente carregado e facetas ainda zeradas:

```python
a = 'pkg.' + input()
b = a
c = b
import_module(c)                     # terceiro salto

x: str = 'pkg.' + input()
import_module(x)                     # AnnAssign

import_module('.' + leaf, package='pkg')
import_module('.mo' + suffix, package='pkg')
```

No primeiro relativo, o prefixo só de pontos é descartado e o sítio vira opaco; no segundo, `.mo` vira prefixo absoluto que não encontra `pkg.mod`, sem opacidade. O pacote só é aplicado quando o nome já foi considerado inteiro. (`bin/quem_chama.py:695`, `bin/quem_chama.py:700`, `bin/quem_chama.py:713`)

`import_module('.mod', package='pkg')` literal funciona no AST e agora também no texto. A regex textual exige a grafia `import_module(...)` e pacote literal; aliases textuais continuam fora desse reconhecimento. (`bin/quem_chama.py:113`, `bin/quem_chama.py:499`, `bin/quem_chama.py:714`)

**6. BLOQUEIA — P1(i), P2: OPACO é aviso global, não proteção contra falso zero.**

Contraexemplo entre arquivos:

```python
# tabela.py
NOME = 'pkg.mod'

# caller.py
from tabela import NOME
importlib.import_module(NOME).f()
```

O loader aparece no cabeçalho, mas não fica ligado à faceta de `f`; ela continua varrida e zerada. `opacos` é anexado como metadado, enquanto apenas os conjuntos `loader` e `embutido` alteram tipos e achados. (`bin/quem_chama.py:701`, `bin/quem_chama.py:1085`, `bin/quem_chama.py:1095`, `bin/quem_chama.py:1113`, `bin/quem_chama.py:1129`)

O censo da cópia encontrou **31 sítios únicos emitidos** em `bin/**` e `app/**`, aplicando a descoberta de invólucros e a deduplicação do render: (`bin/quem_chama.py:1041`, `bin/quem_chama.py:1054`, `bin/quem_chama.py:1129`)

```text
app/api/tests/test_me_posto_geo.py:113,126
app/chamados/tests/test_contract_crons.py:22
app/chamados/tests/test_o137_esmeril_chamado.py:160
app/chamados/tests/test_selo_minimo_modulos.py:54
app/core/erros.py:51
app/core/intake.py:487,494
app/core/management/commands/describe.py:80
app/core/tests/test_contract_direcao_a16.py:157
app/core/tests/test_contract_import_local_resolve.py:75,86
app/core/tests/test_marca_cron15.py:64
app/core/tests/test_selo_contratos_estruturais.py:24,76,86,93,113,116,126,179,202,204,207
app/core/tests/test_selo_estrutural.py:28,31
app/escala/tests/test_selo_grade_pede_o_instante.py:207
app/ponto/tests/test_a4_estados_com_porta.py:191
bin/leitor_dia_estado.py:32
bin/prova_de_casca.py:86
bin/recusa_estado.py:31
```

Esse número **não significa 31 falsos zeros**. Há sítios classificados como opacos apesar de o próprio arquivo conter literais suficientes para ligar seus alvos, como `bin/leitor_dia_estado.py:25` e `bin/prova_de_casca.py:68`. O cabeçalho diz “sem literal no arquivo”, mas o cálculo considera a expressão do argumento. (`bin/quem_chama.py:682`, `bin/quem_chama.py:695`, `bin/quem_chama.py:1130`)

Remover somente a exceção que silencia `exec/compile` acrescenta **quatro sítios**, levando esse censo operacional a **35**: (`bin/quem_chama.py:689`)

```text
app/core/tests/test_access_log_persistido.py:76
app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:43
app/ponto/tests/test_o219_versao_da_regra.py:253
bin/sondas_rollback.py:53
```

Os dois testes de `ponto` executam fragmentos AST; esses quatro sítios não demonstram quatro módulos falsamente zerados. Quantidade de alvos reais do range carregados **exclusivamente** por opacos externos e ainda com zero no RESUMO: **[não verificado]**. (`app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:38`, `app/ponto/tests/test_o219_versao_da_regra.py:247`)

`_COM_NOME_NA_FRENTE` retira corretamente o primeiro argumento posicional dos quatro loaders, evitando usar o nome lógico como evidência de caminho. Mas exclui somente o keyword `name`: `SourceFileLoader(fullname='unused', path=externo)` aceita `fullname` como literal de caminho e silencia OPACO. `run_path(externo, run_name='__main__')` tem o mesmo problema. (`bin/quem_chama.py:108`, `bin/quem_chama.py:586`, `bin/quem_chama.py:587`, `bin/quem_chama.py:684`)

**7. FILA — P3: o prefixo falso esconde três zeros do próprio contador.**

O sítio `test_detectores_isencao.py:8` recebe apenas três nomes: `detectar_ausencias`, `processar_alertas_turno` e `processar_alertas_avancados`. Entretanto, seu prefixo alcança os 104 `.py` daquele diretório: os três argumentos, o pacote pai e **100 arquivos adicionais atribuídos ao sítio**. (`app/ponto/tests/test_detectores_isencao.py:8`, `app/ponto/tests/test_detectores_isencao.py:12`, `app/ponto/tests/test_detectores_isencao.py:15`, `app/ponto/tests/test_detectores_isencao.py:18`, `bin/quem_chama.py:507`)

Em uma simulação em memória com todos esses comandos como alvos, **121 símbolos em 47 arquivos** recebem LOADER exclusivamente por essa atribuição falsa. A lista é:

```text
app/ponto/management/commands/aplicar_09_corte_b.py:30
app/ponto/management/commands/aplicar_09_he_intra.py:32
app/ponto/management/commands/aplicar_col900.py:23
app/ponto/management/commands/aplicar_janela_he_total.py:33
app/ponto/management/commands/apurar_furo_da_exportada.py:49
app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:20
app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:29
app/ponto/management/commands/backfill_b5_insumos.py:38
app/ponto/management/commands/carimbo_gravado.py:29
app/ponto/management/commands/celula_veredito_velho.py:30
app/ponto/management/commands/censo_col900.py:50
app/ponto/management/commands/censo_fase_12x36.py:28
app/ponto/management/commands/curar_residuo_bug115.py:36
app/ponto/management/commands/desvio_o68b.py:20
app/ponto/management/commands/detectar_intervalo_ausente.py:26
app/ponto/management/commands/detectar_vinculo_divergente.py:18
app/ponto/management/commands/diff_col900.py:19
app/ponto/management/commands/diff_espelho_alimentacao.py:28
app/ponto/management/commands/diff_janela_he.py:22
app/ponto/management/commands/diff_janela_he_total.py:53
app/ponto/management/commands/diff_reclassificar_partido.py:26
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:48
app/ponto/management/commands/escalonar_chamados_supervisao.py:24
app/ponto/management/commands/fechar_chamados_meio_periodo.py:30
app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:18
app/ponto/management/commands/folga_que_sumiu.py:28
app/ponto/management/commands/gerar_batidas_smoke_motor.py:22
app/ponto/management/commands/gerar_celulas.py:16
app/ponto/management/commands/marcar_foto_ausente_retro.py:45
app/ponto/management/commands/passivo_disputa_retro.py:42
app/ponto/management/commands/plano_b_no_dinheiro.py:40
app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:13
app/ponto/management/commands/recalcular_fechamento.py:38
app/ponto/management/commands/reconciliar_fantasmas.py:46
app/ponto/management/commands/recusar_ponta_pequena.py:49
app/ponto/management/commands/rederivar_minutos_h06_comp11.py:45
app/ponto/management/commands/rejulgar_ausencia_em_folga.py:20
app/ponto/management/commands/reverter_situacao_afastado.py:44
app/ponto/management/commands/supra_juiz.py:45
app/ponto/management/commands/testar_pwa.py:21
app/ponto/management/commands/tripwire_celulas.py:12
app/ponto/management/commands/tripwire_tipo_batida.py:18
app/ponto/management/commands/vigia_b5_ausencia.py:46
app/ponto/management/commands/vigia_pareador_celula.py:16
app/ponto/management/commands/vigia_plantio_overnight.py:16
app/ponto/management/commands/vigiar_ausencia_x_veredito.py:21
app/ponto/management/commands/vigiar_motor_autoridade.py:12
```

Retirar somente os achados de prefixo faz estes três símbolos voltarem a **zero estático**:

- `_chaves_marcos` — `app/ponto/management/commands/gerar_celulas.py:19`.
- `_competencia_de` — `app/ponto/management/commands/passivo_disputa_retro.py:45`.
- `WARN` — `app/ponto/management/commands/testar_pwa.py:27`.

Isso demonstra zeros do contador escondidos pelo falso vínculo. Ausência de consumidores externos/dinâmicos e “zero verdadeiro de produção” para aplicação da L-111: **[não verificado]**. A lei exige zero de produção; o contrato do instrumento admite falsos positivos. (`app/docs/LEIS.md:137`, `bin/quem_chama.py:97`)

Os três testes de settings também atribuem seus sítios a seis perfis adicionais: `ci`, `ci_tenants`, `dev`, `smoke`, `sombra`, `teste_motor`. Seus símbolos já eram não varridos por serem settings; não são novos rebaixamentos. (`app/core/tests/test_selo_erro_com_mensagem.py:80`, `app/core/tests/test_selo_log_nao_e_cego.py:120`, `app/core/tests/test_trilha_com_contexto.py:103`, `bin/quem_chama.py:384`)

Os prefixos de `FORMATADOR` e `sonda:` não acrescentaram outro alvo falso observado na cópia. O primeiro aponta para `relatorios.lote_espelho`; o segundo é um nome de código compilado. (`app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:37`, `app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:68`, `app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:126`, `bin/sondas_rollback.py:53`)

**8. FILA — P4: mapa de escritores/leitores e semânticas incompatíveis.**

| Valor | Quem escreve/define | Quem lê |
|---|---|---|
| `atrib` | `bin/quem_chama.py:651`, `bin/quem_chama.py:656` | `bin/quem_chama.py:661` |
| `_valores` | `bin/quem_chama.py:658` | Recursão `bin/quem_chama.py:662`; caminho `bin/quem_chama.py:682`; nome `bin/quem_chama.py:695` |
| `Alvos.por_mod`, incluindo `app.` | `bin/quem_chama.py:446`, `bin/quem_chama.py:448`, `bin/quem_chama.py:450` | Texto relativo `bin/quem_chama.py:500`; nomes compostos `bin/quem_chama.py:736`; literais `bin/quem_chama.py:780` |
| `Alvos.sob_prefixo` | `bin/quem_chama.py:504` | `bin/quem_chama.py:739` |
| Retorno de `involucros` | `bin/quem_chama.py:639`, `bin/quem_chama.py:641` | `bin/quem_chama.py:1044` |
| `novo_` | `bin/quem_chama.py:1042`, `bin/quem_chama.py:1044` | Comparação `bin/quem_chama.py:1045`; transferência `bin/quem_chama.py:1047` |
| `opacos` local | `bin/quem_chama.py:664`, `bin/quem_chama.py:690`, `bin/quem_chama.py:701`, `bin/quem_chama.py:718` | Retorno `bin/quem_chama.py:719`; recepção/acúmulo `bin/quem_chama.py:730`, `bin/quem_chama.py:732` |
| `opacos` global/metadado | `bin/quem_chama.py:1037`, `bin/quem_chama.py:1096` | Primeiro alvo `bin/quem_chama.py:1129`; cabeçalho/contador `bin/quem_chama.py:1130`, `bin/quem_chama.py:1132` |
| `LOADERS_CAMINHO` | `bin/quem_chama.py:103` | `bin/quem_chama.py:106`, `bin/quem_chama.py:111` |
| `LOADERS_CAMINHO_AST` | `bin/quem_chama.py:106` | `bin/quem_chama.py:612`, `bin/quem_chama.py:618`, `bin/quem_chama.py:1039` |
| `_SO_BUILTIN` | `bin/quem_chama.py:107` | `bin/quem_chama.py:603` |
| `_COM_NOME_NA_FRENTE` | `bin/quem_chama.py:108` | `bin/quem_chama.py:586` |
| `_RX_LOADER_CAMINHO` | `bin/quem_chama.py:111` | `bin/quem_chama.py:487` |
| `_RX_PYTHON` | `bin/quem_chama.py:109` | `bin/quem_chama.py:669` |
| `_RX_REL_TXT` | `bin/quem_chama.py:113` | `bin/quem_chama.py:499` |

Há quatro incompatibilidades demonstradas:

- `inteiro=True` agora inclui valor calculado, mas os consumidores ainda dependem de encontrar um `Constant` completo: contraexemplo do achado 1. (`bin/quem_chama.py:559`, `bin/quem_chama.py:684`, `bin/quem_chama.py:779`)
- `linhas['caminho']` significa chamada real **ou mera presença** de interpretador; ambos são apresentados como “loader de caminho”. (`bin/quem_chama.py:666`, `bin/quem_chama.py:669`, `bin/quem_chama.py:678`, `bin/quem_chama.py:733`)
- OPACO significa argumento não resolvido, embora o cabeçalho anuncie ausência de literal no arquivo: contraexemplo `prova_de_casca`. (`bin/prova_de_casca.py:68`, `bin/prova_de_casca.py:86`, `bin/quem_chama.py:1130`)
- Imports AST mantêm a semântica anterior de módulo único, apesar das novas chaves `app.`: contraexemplo do achado 2. (`bin/quem_chama.py:450`, `bin/quem_chama.py:747`, `bin/quem_chama.py:753`)

**9. BLOQUEIA — P5: o selo tem verificações mascaradas de mecanismos novos.**

Nas fixtures r9/r10 exercitadas em memória, sobreviveram estes mutantes:

| Mutante | Por que passa |
|---|---|
| Retirar `compile` de `_kind` | A fixture usa `exec(compile(...))`; o `exec` externo ainda protege `ld_cp`. (`bin/quem_chama.py:618`, `bin/tests/test_quem_chama.sh:295`, `bin/tests/test_quem_chama.sh:595`) |
| Reduzir `_valores` de dois para um salto | A fixture `ld_cte` já contém o basename completo, suficiente para associação lexical. (`bin/quem_chama.py:658`, `bin/quem_chama.py:775`, `bin/tests/test_quem_chama.sh:196`) |

[truncado em 20000 de 23183 caracteres]

## 06/11 19:26 · sessao f9837b72f319 · ESMERIL-SEMANTICA · censo · origem=Code

**Encontrei pares novos divergentes em 6 campos de modelo, 4 parâmetros e no payload C9. Dois campos ficaram como “não sei”.** Contagem consolidada dos sítios examinados, sem medida de incidência em produção: **[nao verificado]**.

As famílias abaixo seguem `app/core/contratos_estruturais.py:45`; os endereços das autoridades de turno, ausência, chamado e fechamento constam em `app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:50`, `app/docs/JUIZES.md:54` e `app/docs/JUIZES.md:78`.

**(2) Pares divergentes**

1. **`Batida.timestamp`: data crua × data local na fatia.** O escritor preserva o instante recebido; o fechamento usa `timestamp.date()` para decidir quais batidas pertencem à fatia. Escritor: `app/ponto/registro_batida.py:158`. Leitor: `app/ponto/services/fechamento.py:252`. **Famílias: batida → folha/export.**

   Caso: fatia 01–03/10/2026, dias próprios `{01,03}`; E/S em `02/10 00:15Z` e `02/10 01:15Z`, correspondentes a **01/10 21:15 e 22:15 locais**. O filtro elimina as duas por enxergar dia 02; o motor recebe a lista sem esse par. Fuso configurado: `app/config/settings/base.py:93`; filtro e entrega ao motor: `app/ponto/services/fechamento.py:252`, `app/ponto/services/fechamento.py:278`. Efeito monetário final: **[nao verificado]**.

2. **`Batida.timestamp`: meia-noite seguinte tratada como fim inclusivo.** O escritor aceita o instante; dois consumidores constroem o primeiro instante posterior ao período e o entregam a uma função cujo fim usa `<=`. Escritor: `app/ponto/registro_batida.py:158`. Leitores: `app/ponto/services/abriu_nao_bateu.py:173` e `app/ponto/services/dias_em_aberto.py:121`; inclusão efetiva: `app/ponto/turnos.py:2868`. **Famílias: batida; folha/export no passivo.**

   Casos: única batida em **07/11 00:00 local**, consulta de **06/11** → `_bateu_no_dia=True`; competência terminando em **20/10**, dez dias em aberto e única batida em **21/10 00:00 local** → ela entra no contador e permite `quase_nunca_bateu`, em lugar de `nunca_bateu`. Sítios: `app/ponto/services/abriu_nao_bateu.py:174`, `app/ponto/services/dias_em_aberto.py:122`, `app/ponto/services/dias_em_aberto.py:57`.

3. **`Batida.timestamp`: fração do último segundo excluída.** O escritor grava o timestamp sem truncamento; `dia_do_colab` encerra o dia em `23:59:59`, com comparação inclusiva. Escritor: `app/ponto/registro_batida.py:158`. Leitor: `app/ponto/services/dia_do_colab.py:56`, passando por `app/ponto/turnos.py:2868`. **Família: batida.**

   Caso: batida em **06/11/2026 23:59:59.500000−03:00** → não aparece nas batidas de 06/11, cujo teto é `23:59:59.000000`. Montagem da janela e consulta: `app/ponto/services/dia_do_colab.py:55`, `app/ponto/services/dia_do_colab.py:60`.

4. **`Batida.timestamp`: dia civil usado como início de uma consulta por dia de turno.** O juiz pode atribuir uma entrada após meia-noite à véspera; a regularização começa a busca na data civil dessa entrada. Escritor do instante: `app/ponto/registro_batida.py:158`; atribuição do turno: `app/ponto/turnos.py:299`, `app/ponto/turnos.py:306`; leitor: `app/ponto/services/regularizacao.py:60`. **Família: turno/marcos.**

   Caso: E em **21/09 00:00 local**, turno da véspera **23:50–07:50** → juiz responde **20/09**; consulta iniciada em **21/09** perde esse turno e a regularização responde “Esta batida nao inicia um turno aberto”. Filtro por dia de turno: `app/ponto/turnos.py:1993`; recusa: `app/ponto/services/regularizacao.py:64`. Esse caso **não chega à gravação** de `Justificativa.data_turno`: `app/ponto/services/regularizacao.py:94`.

5. **`Justificativa.data_turno`: escritor grava dia civil; leitores esperam dia do turno.** A triagem conserva a data local da entrada, mesmo tendo recebido o turno pareado, e depois grava essa data na justificativa. Escritores: `app/ponto/services/triagem_batida.py:180`, `app/ponto/services/triagem_batida.py:260`. Leitores: `app/ponto/services/efeito_ausencia.py:43` e `app/ponto/services/fechamento.py:654`. **Origem: turno/marcos; consumo: folha/export.**

   Caso: a mesma E de **21/09 00:00**, pertencente ao turno **20/09**, produz justificativa com `data_turno=21/09`. Depois de aprovada, consultar justificadas de **20/09–20/09** não a encontra. A triagem carrega o universo sem corte por data e mantém a entrada na lista: `app/ponto/services/triagem_batida.py:175`, `app/ponto/services/triagem_batida.py:178`, `app/ponto/services/triagem_batida.py:185`; exclusão pelo intervalo: `app/ponto/services/efeito_ausencia.py:43`.

6. **Fila de flip: produtor fornece dia civil; leitor pergunta por dia do turno.** `fila_flip_recusas` produz `dia=localtime(timestamp).date()`; `avaliar_fila` entrega esse dia a `dia_encerrado`. Produtor: `app/ponto/services/flip_auto.py:140`; leitor: `app/ponto/services/flip_auto.py:285`; contrato do juiz: `app/ponto/turnos.py:2043`. **Família: turno/marcos.**

   Caso: vizinha **S21/09 07:00**, do turno **20/09 19:00–21/09 07:00**, já encerrado; agora **21/09 08:00**, com próximo turno noturno previsto → a fila consulta **21/09**, conta `dia_em_curso` e pula o item do turno anterior. Consumo e saída: `app/ponto/services/flip_auto.py:268`, `app/ponto/services/flip_auto.py:286`. O parâmetro `etiquetas_pela_celula.dia` também recebe essa data e consulta o DNA dela; o recuo só ocorre quando a célula consultada diz folga: `app/ponto/services/flip_auto.py:201`, `app/ponto/services/flip_auto.py:208`, `app/ponto/services/flip_auto.py:310`.

7. **`PerguntaDisputa.data_evento`: dia da saída × dia do turno no fechamento.** Para `orfao_14h`, o produtor pode resolver o dia da saída esperada; o fechamento do fio filtra esse campo pela data do turno que o admin resolveu. Produtor: `app/escala/servico_jornada.py:491`, `app/chamados/services/disputa_emissao.py:1041`; gravação: `app/chamados/models.py:2476`; leitor novo A2: `app/ponto/services/veredito_celula.py:176`. **Família: chamado, com geometria de turno/marcos.**

   Caso: turno **20/09 19:00–21/09 07:00**, pergunta de saída com `data_evento=21/09`; resolver o turno **20/09** consulta `data_evento=20/09` e não seleciona essa pergunta para fechamento. Data da saída esperada: `app/escala/servico_jornada.py:203`, `app/escala/servico_jornada.py:209`; fechamento: `app/ponto/services/veredito_celula.py:174`. O próprio nascimento reconhece que evento e chamado podem diferir por cruzamento da meia-noite: `app/chamados/models.py:2411`.

8. **`Ausencia.data_fim`: nulo aberto × nulo de um único dia.** O escritor usa `None` para intervalos abertos; leitores reduzem esse valor ao início ou exigem início igual ao dia consultado. Escritor: `app/ponto/services/ausencia.py:70`, `app/ponto/services/ausencia.py:343`; leitores novos A2: `app/ponto/services/dia_decidido.py:552`, `app/ponto/services/dia_decidido.py:558`, `app/ponto/services/atestados_lote.py:58`, `app/ponto/services/cartorio.py:356`. **Família: ausencia/ferias.**

   Caso: afastamento aberto iniciado em **01/10**, consulta **01–06/11** → mapa de decisões `{}` e relatório sem a linha. No cartório, uma alteração desse afastamento também não satisfaz a consulta de ausência para **06/11**, pois o ramo nulo exige `data=06/11`. Sítios: `app/ponto/services/dia_decidido.py:560`, `app/ponto/services/atestados_lote.py:58`, `app/ponto/services/cartorio.py:355`.

9. **`Ausencia.aprovada_em` e `Ausencia.criado_em`: instante cru no CSV × apresentação local no painel.** O escritor carimba o instante; `quem_quando` o repassa sem conversão e o CSV aplica `strftime` diretamente. Escritores: `app/ponto/services/ausencia.py:349`, `app/ponto/models.py:813`, `app/ponto/models.py:873`; leitor: `app/ponto/services/atestados_lote.py:131`, via `app/ponto/services/dia_decidido.py:508`. **Família: ausencia/ferias.**

   Caso: `2026-11-06T01:30Z` → CSV **06/11 01:30**; painel **05/11 22:30**. O mesmo ocorre com `criado_em` quando `aprovada_em` está nulo. Conversão local do painel: `app/templates/ponto/partials/painel_ausencia.html:261`, `app/templates/ponto/partials/painel_ausencia.html:188`; configuração: `app/config/settings/base.py:93`.

10. **Payload C9: dia do turno + hora de marco lidos como mesmo dia civil.** O produtor grava dia da célula e marcos faltantes; o fallback de `lastro.julgar` combina todas as horas com esse mesmo dia, sem encadear a madrugada. Produtor: `app/ponto/services/cartorio.py:1778`, `app/ponto/management/commands/detectar_ausencias.py:706`; leitor: `app/ponto/services/lastro.py:305`. **Famílias: chamado → turno/marcos.**

    Caso: `data_turno=20/09`, marco **S07:00**, batidas **E20/09 19:00 / S21/09 07:00**, sem lâmpada correspondente → alvo **20/09 07:00**, distância de **1.440 minutos**, classificação `outro_horario`. Esse caminho depende da ausência da lâmpada: `app/ponto/services/lastro.py:289`, `app/ponto/services/lastro.py:299`, `app/ponto/services/lastro.py:308`, `app/ponto/services/lastro.py:320`.

**(1) Quadro restrito a “sim” e “não sei”**

E conta funções escritoras explícitas em A2; L conta a **amostra de funções leitoras abertas**, não o total global. Os sítios externos de suporte estão identificados. Completude global dos escritores/leitores fora de A2: **[nao verificado]**.

| Campo/parâmetro | Significados encontrados | Escritores — n e sítios | Leitores — n, amostra lida | Divergente? | Caso que quebra |
|---|---|---|---|---|---|
| `Batida.timestamp` | Instante preservado: `app/ponto/registro_batida.py:158`; data crua: `app/ponto/services/fechamento.py:252`; limites civis incompatíveis: `app/ponto/services/dia_do_colab.py:56` | **0 diretos A2**; porta externa **1**, `app/ponto/registro_batida.py:94` | Amostra **8**: os consumidores dos pares 1–4, fila de recusas, avaliação do flip e lastro, nos sítios acima | **sim** | Pares **1–4 e 6** |
| `Justificativa.data_turno` | Data do turno no contrato: `app/ponto/models.py:144`; data civil da entrada no escritor: `app/ponto/services/triagem_batida.py:260` | **2**: `app/ponto/services/regularizacao.py:94`; `app/ponto/services/triagem_batida.py:260` | Amostra **4**: `app/ponto/services/efeito_ausencia.py:43`; `app/ponto/services/fechamento.py:654`; `app/ponto/services/justificativa.py:200`; `app/ponto/services/cartorio.py:359` | **sim** | Par **5**; regularização tem a recusa anterior descrita no par **4** |
| `PerguntaDisputa.data_evento` | Dia da saída esperada: `app/escala/servico_jornada.py:491`; igualdade com dia do turno resolvido: `app/ponto/services/veredito_celula.py:176` | **0 A2**; suporte externo lido: nascimento em `app/chamados/models.py:2476` e reapontamento em `app/chamados/services/fio.py:834` | **2**: `app/ponto/services/dia_do_colab.py:65`; `app/ponto/services/veredito_celula.py:176` | **sim** | Par **7** |
| `Ausencia.data_fim` | Fim inclusivo ou aberto: `app/ponto/services/ausencia.py:67`; nulo reduzido ao início: `app/ponto/services/dia_decidido.py:558` | **2 explícitos**: `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:500`; derivação adicional no modelo: `app/ponto/models.py:907` | Amostra **4**: `app/ponto/services/dia_decidido.py:552`; `app/ponto/services/atestados_lote.py:58`; `app/ponto/services/cartorio.py:356`; `app/ponto/services/vigia_ausencia_veredito.py:48` | **sim** | Par **8** |
| `Ausencia.aprovada_em` | Instante aware: `app/ponto/services/ausencia.py:349`; texto sem conversão local: `app/ponto/services/atestados_lote.py:131` | **2 A2**: `app/ponto/services/ausencia.py:349`, `app/ponto/services/ausencia.py:917`; auto-carimbo externo: `app/ponto/models.py:873` | Amostra **3**: `app/ponto/services/dia_decidido.py:508`; `app/ponto/services/atestados_lote.py:131`; `app/ponto/services/cartorio.py:357` | **sim** | Par **9** |
| `Ausencia.criado_em` | Instante automático: `app/ponto/models.py:813`; fallback cru para apresentação: `app/ponto/services/dia_decidido.py:508` | **0 explícitos A2**; automático em `app/ponto/models.py:813` | Amostra **2**: `app/ponto/services/dia_decidido.py:508`; `app/ponto/services/atestados_lote.py:131` | **sim** | Par **9**, quando não há aprovação |
| `_bateu_no_dia.dia` | Dia civil convertido em intervalo que inclui a meia-noite seguinte: `app/ponto/services/abriu_nao_bateu.py:172` | **1 produtor A2**: `app/ponto/services/abriu_nao_bateu.py:130` | **1**: `app/ponto/services/abriu_nao_bateu.py:174` | **sim** | Par **2** |
| `dia_do_colab.data` | Dia civil convertido em intervalo sem a fração final do segundo: `app/ponto/services/dia_do_colab.py:56` | **0 chamadas A2**; origem externa completa **[nao verificado]** | **1**: `app/ponto/services/dia_do_colab.py:60` | **sim** | Par **3** |
| `etiquetas_pela_celula.dia` | Data civil produzida pela fila: `app/ponto/services/flip_auto.py:140`; dia do DNA/turno: `app/ponto/services/flip_auto.py:201` | **1 chamada A2**: `app/ponto/services/flip_auto.py:310` | **1**: `app/ponto/services/flip_auto.py:201` | **sim** | Par **6** |
| `fechar_o_fio_do_dia.data` | Dia do turno declarado/resolvido: `app/ponto/services/veredito_celula.py:326`; comparação direta com dia do evento: `app/ponto/services/veredito_celula.py:176` | **2 chamadas A2**: `app/ponto/services/veredito_celula.py:325`; `app/ponto/services/declarar_dia.py:204` | **1**: `app/ponto/services/veredito_celula.py:174` | **sim** | Par **7** |
| `OrdemSubstituicao.data_inicio` e `.data_fim` — **2 campos** | Disponibilidade usa sobreposição estrita: `app/ponto/utils.py:205`; atividade inclui as duas pontas: `app/ponto/models.py:1147` | **2 funções por campo**: `app/ponto/services/cobertura.py:105`; `app/ponto/services/cobertura.py:291` | Amostra: disponibilidade chamada em `app/ponto/services/cobertura.py:83`; atividade em `app/ponto/models.py:1147`, `app/ponto/models.py:1155` | **não sei** | Há desacordo nas bordas; intenção do escritor e saída temporal incorreta alcançável **[nao verificado]**. Duas ordens ativas adjacentes não servem de prova: constraints em `app/ponto/models.py:1124`, `app/ponto/models.py:1130` |
| Payload `data_turno` + hora de marco | Dia do turno produzido: `app/ponto/services/cartorio.py:1779`; hora combinada no mesmo dia civil: `app/ponto/services/lastro.py:305` | **1 caminho novo lido**: `app/ponto/services/cartorio.py:1778` → `app/ponto/management/commands/detectar_ausencias.py:706` | **1**: `app/ponto/services/lastro.py:305` | **sim** | Par **10**, condicionado à ausência da lâmpada |

**Demais examinados:** 43 campos de modelo e 117 parâmetros sem novo desacordo identificado nos sítios lidos; isso não certifica unanimidade global fora do recorte. **[nao verificado: consolidação da leitura e da enumeração AST, sem artefato persistido]**

**(3) Contagem**

| Universo examinado | Total | Divergentes | Não sei | Sem novo desacordo encontrado |
|---|---:|---:|---:|---:|
| Campos de modelo | **51** | **6** | **2** | **43** |
| Parâmetros | **121** | **4** | **0** | **117** |
| Payload C9, separado dos anteriores | **1** | **1** | **0** | **0** |

Contagens consolidadas: **[nao verificado: enumeração AST em memória e união das identidades examinadas]**. Os dois “não sei” são os limites de `OrdemSubstituicao`, pelo motivo indicado no quadro; incidência dos casos em produção: **[nao verificado]**.

Dos 51 campos, **46 estão nas linhas CAMPO fornecidas** e **5 foram encontrados no código além delas**: `MetricaSnapshot.data_ini/data_ref/calculado_em`, `LogAuditoria.criado_em` e `LoteHolerite.publicado_em`; exemplos de uso e declaração em `app/inteligencia/models.py:90`, `app/inteligencia/models.py:96`, `app/ponto/services/retratar_humano.py:39` e `app/ponto/services/fechamento.py:1523`. Dos 121 parâmetros, **44** têm nome `dia`/`data_*`, incluindo quatro `data_` internos do cartório, e **77** são as extensões temporais `data`, `ini`, `fim` e `dia_*`: **[nao verificado: contagem AST sem artefato persistido]**.

## 06/11 19:26 · sessao 2e9b4e1cb20c · ESMERIL-SEMANTICA · censo · origem=Code

**Encontrei 4 campos divergentes, em 3 desacordos. O censo integral de campos de A1 não ficou fechado; as contagens abaixo distinguem a cobertura realizada da cobertura ainda [nao verificado].** (app/chamados/services/validacao.py:826; app/chamados/veredito_lavrado.py:182; app/chamados/services/janela_colab.py:73)

**(1) Quadro**

`E` conta sítios de atribuição/criação abertos, incluindo limpeza ou restauração; `R` conta funções leitoras identificadas na amostra indicada. Não conta `update_fields` como outro escritor. A quantidade total de leitores por modelo, além dessas amostras, é **[nao verificado]**.

| Campo/parâmetro | Significados encontrados | Escritores — n e sítios | Leitores — n, amostra lida | Divergente? | Caso que quebra |
|---|---|---|---|---|---|
| `PerguntaDisputa.data_evento` | **Dia do turno/célula**, inclusive para saída amanhã: app/chamados/services/declaracao_texto.py:123, app/chamados/services/declaracao_texto.py:326. **Dia civil da hora respondida**, na busca de conflito: app/chamados/services/validacao.py:826. | **3 sítios finais**: criação em app/chamados/models.py:2476; preenchimento e restauração em app/chamados/services/fio.py:834 e app/chamados/services/fio.py:843. Também abertos os chamadores da declaração e emissão: app/chamados/services/declaracao_texto.py:326, app/chamados/services/declaracao_texto.py:615, app/chamados/services/disputa_emissao.py:396 e app/chamados/services/disputa_emissao.py:777. | **2 funções na amostra decisiva**: classificação em app/chamados/services/validacao.py:798; plantio em app/chamados/services/materializacao.py:344, com ajuste em app/chamados/services/materializacao.py:420. | **Sim** | Turno **20/10/2026 19h → 21/10 07h**; `data_evento=20/10`, resposta e marco `07:00`; batida existente **21/10 07:05**. A classificação procura **20/10 06h–08h** e devolve `LOTE_A`, cujo rótulo diz “sem batida perto”; o plantio deriva **21/10 07h**. (app/chamados/services/validacao.py:765; app/chamados/services/validacao.py:826; app/chamados/services/validacao.py:830; app/chamados/juizes.py:310) |
| `PerguntaDisputa.julgado_em` | **Instante da lavra**, escrito com `timezone.now()`; depois, **data crua desse instante**, comparada com a data de expiração. (app/chamados/veredito_lavrado.py:228; app/chamados/veredito_lavrado.py:185) | **3**: lavra em app/chamados/veredito_lavrado.py:228; restauração em memória em app/chamados/veredito_lavrado.py:244; backfill em app/chamados/management/commands/backfill_veredito_lavrado.py:139. | **4 funções**: `esta_stale`, `foto_da_lavra`, varredura e backfill. (app/chamados/veredito_lavrado.py:173; app/chamados/veredito_lavrado.py:237; app/chamados/services/raiox.py:31; app/chamados/management/commands/backfill_veredito_lavrado.py:139) | **Sim** | Expiração **21/11**; relavra **21/11 01:00Z = 20/11 22h local**; consulta **21/11 03:30Z = 21/11 00:30 local**. Retorna `False` para stale porque `julgado_em.date()==21/11`, embora a lavra seja da véspera local. (app/chamados/veredito_lavrado.py:185; app/config/settings/base.py:93) |
| `PerguntaDisputa.janela_expira_em` | **Data civil da virada da janela**, inclusive na classificação do backfill por `localdate()`; leitor compara com `now().date()` cru. (app/chamados/veredito_lavrado.py:116; app/chamados/management/commands/backfill_veredito_lavrado.py:94; app/chamados/management/commands/backfill_veredito_lavrado.py:113; app/chamados/services/raiox.py:36) | **3**: app/chamados/veredito_lavrado.py:229; restauração em app/chamados/veredito_lavrado.py:245; app/chamados/management/commands/backfill_veredito_lavrado.py:140. | **4 funções**, mesmas amostras da linha anterior; comparações em app/chamados/veredito_lavrado.py:182 e app/chamados/services/raiox.py:41. | **Sim** | Expiração **21/11**, lavra **20/11 20:00Z**, agora **21/11 00:30Z = 20/11 21:30 local** → stale `True`, **2h30 antes da virada civil**. (app/chamados/veredito_lavrado.py:182; app/chamados/veredito_lavrado.py:185; app/config/settings/base.py:93) |
| `ChamadoColaborador.silenciado_ate` | **Fim temporário da supressão**, após o qual pode reaparecer na fila; versus **marca persistente de arquivo**, cujo vencimento vira histórico e cuja presença continua excluindo a lista do colaborador. (app/chamados/services/arquivo.py:150; app/chamados/services/painel.py:188; app/chamados/services/verbos.py:51; app/chamados/services/janela_colab.py:73) | **2**: colocar prazo em app/chamados/services/arquivo.py:166; limpar em app/chamados/services/arquivo.py:201. | **4 funções na amostra**: predicado do modelo, fila, verbos e lista do colaborador. (app/chamados/models.py:480; app/chamados/services/painel.py:188; app/chamados/services/verbos.py:51; app/chamados/services/janela_colab.py:73) | **Sim** | Chamado vivo, dentro da competência, `silenciado_ate=06/11/2026 12:00Z`, agora `13:00Z`: entra na fila do painel, vira `HISTORICO` nos verbos e continua excluído da lista do colaborador. **Qual política deve prevalecer: [nao verificado].** (app/chamados/services/painel.py:188; app/chamados/services/verbos.py:51; app/chamados/services/janela_colab.py:73) |
| `PerguntaDisputa.materializada_em` — novo leitor I6 | Instante de processamento; auditor extrai `.date()` cru para separar o piso histórico “até 24/08”. A origem/fuso dos carimbos históricos dessa via é **[nao verificado]**. (app/chamados/models.py:2827; app/chamados/management/commands/auditar_invariantes_chamados.py:452; app/chamados/management/commands/auditar_invariantes_chamados.py:478) | **2 sítios centrais abertos**: atribuição em app/chamados/models.py:2558; limpeza em app/chamados/models.py:2700. **Escritor histórico de `legado_sem_lastro`: [nao verificado]**; o auditor declara ausência de escritor vivo em app/chamados/management/commands/auditar_invariantes_chamados.py:445. | **2 funções na amostra**: estado em app/chamados/models.py:2499; universo I6 em app/chamados/management/commands/auditar_invariantes_chamados.py:455. | **Não sei** | Carimbo **25/08/2026 01:00Z = 24/08 22h local** cai em violação pela data crua; pela data local cairia no piso. Não fechei o par com o escritor histórico. (app/chamados/management/commands/auditar_invariantes_chamados.py:478; app/config/settings/base.py:93) |
| `chamados_sem_dia_do_fato.data_ini` | Parâmetro declarado, **sem consumo no corpo**; significado pretendido **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:75; app/chamados/services/juizes_do_dia.py:92) | **0 fornecimentos explícitos localizados**: o chamador aberto usa os defaults. (app/chamados/services/fila.py:83) | **0 usos no corpo**: consulta todos os vivos. (app/chamados/services/juizes_do_dia.py:92) | **Não sei** | Informar `data_ini=01/11/2026` não restringe a consulta; resultado devido para esse argumento **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:92) |
| `chamados_sem_dia_do_fato.data_fim` | Parâmetro declarado, **sem consumo no corpo**; significado pretendido **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:75; app/chamados/services/juizes_do_dia.py:92) | **0 fornecimentos explícitos localizados**: app/chamados/services/fila.py:83. | **0 usos no corpo**: app/chamados/services/juizes_do_dia.py:92. | **Não sei** | Informar `data_fim=31/10/2026` não restringe a consulta; resultado devido **[nao verificado]**. (app/chamados/services/juizes_do_dia.py:92) |

Ocorrência desses casos no acervo de produção: **[nao verificado]**.

**(2) Somente os divergentes — pares e famílias**

- **`data_evento`:** escritor da declaração **app/chamados/services/declaracao_texto.py:326**, persistido em **app/chamados/models.py:2476**, × leitor **app/chamados/services/validacao.py:826**. O desacordo aparece contra a interpretação de turno usada em **app/chamados/services/materializacao.py:420**. Família **chamado**, com geometria **turno/marcos**. (app/core/contratos_estruturais.py:48; app/core/contratos_estruturais.py:51; app/docs/JUIZES.md:24)
- **`julgado_em` e `janela_expira_em`:** escritores **app/chamados/veredito_lavrado.py:228** e **app/chamados/veredito_lavrado.py:229** × leitores **app/chamados/veredito_lavrado.py:182**, **app/chamados/veredito_lavrado.py:185** e sua varredura em **app/chamados/services/raiox.py:41**. Família **chamado**. (app/core/contratos_estruturais.py:51)
- **`silenciado_ate`:** escritor temporário **app/chamados/services/arquivo.py:166**, cuja renovação evita reaparecimento conforme **app/chamados/services/arquivo.py:150**, × leitores **app/chamados/services/janela_colab.py:73** e **app/chamados/services/verbos.py:51**. Família **chamado**. (app/core/contratos_estruturais.py:51)

**(3) Contagem e cobertura**

| Cobertura realizada | Examinados | Divergentes | Não sei | Sem divergência encontrada na amostra |
|---|---:|---:|---:|---:|
| Campos: 28 declarados em `chamados`, mais início/fim/cancelamento de cobertura | **31** | **4** | **1** | **26** |
| Parâmetros temporais, incluindo `data` temporal e os quatro `ini/fim` do anexo | **65** | **0** | **2** | **63** |
| **Total dessa cobertura** | **96** | **4** | **3** | **89** |

As declarações dos campos estão em app/chamados/models.py:54, app/chamados/models.py:1602, app/chamados/models.py:1686, app/chamados/models.py:2218, app/chamados/models.py:3181, app/chamados/models.py:3218 e app/chamados/models.py:3260; a cobertura foi comparada entre app/ponto/services/cobertura.py:57 e app/chamados/detector_anomalias.py:71. **Exaustão dos chamadores de todos os 65 parâmetros: [nao verificado].**

Separadamente, examinei `dia_corte` como **inteiro de corte**, não data; os dois parâmetros FCM chamados `data` são **payloads**, não temporais. (app/chamados/management/commands/arquivar_competencia_encerrada.py:181; app/chamados/fcm_utils.py:50; app/chamados/fcm_utils.py:102)

**O total completo de campos externos alcançados pelo inventário A1 e a concordância de todos os seus escritores não foram fechados: [nao verificado]. Portanto, 96 é a cobertura acima, não um certificado de conclusão de todo A1.**

## 06/11 19:25 · sessao 93a650db3dbe · ESMERIL-SEMANTICA · censo · origem=Code

**Há divergências demonstráveis, mas esta volta não fechou o censo universal de escritores.** Os números abaixo contam sítios efetivamente abertos; totais globais de escritores/leitores permanecem **[não verificado]**. Os casos são deduções estáticas do código, sem execução nem confirmação de incidência em produção **[não verificado]**.

**(2) Pares divergentes — prioridade**

| ID | Campo/parâmetro | Escritor ou fornecedor × leitor que discorda | Família estrutural |
|---|---|---|---|
| D1 | `OrdemSubstituicao.data_fim` | A designação recebe uma janela cuja disponibilidade usa fim **exclusivo** (`app/ponto/services/cobertura.py:83`, `:106`; `app/ponto/utils.py:143`, `:206`). `esta_ativa_em` e `ativa_para` incluem o instante final (`app/ponto/models.py:1147`, `:1156`). | **escala**, conforme `app/core/contratos_estruturais.py:145`. |
| D2 | `Justificativa.data_turno` | O cron grava o **dia da jornada** recebido do turno (`app/ponto/management/commands/processar_alertas_turno.py:393`). O formulário recebe somente o timestamp da entrada, oferece seu **dia civil** e grava esse dia (`app/ponto/views.py:1584`; `app/templates/ponto/justificativa_form.html:30`; `app/ponto/views.py:1638`). Os leitores procuram cobertura e invalidam fechamento por `j.data_turno` (`app/ponto/views.py:1681`, `:1805`). | **turno/marcos**, pelo domínio do dia do turno (`app/docs/JUIZES.md:20`). |
| D3 | `ChamadoColaborador.criado_em` — par novo | O nascimento usa carimbo automático (`app/chamados/models.py:61`). `ping_geo` calcula o filtro diário com `agora = tz.now()` e `agora.date()` cru (`app/ponto/views.py:2946`, `:3015`), enquanto outros leitores diários usam `timezone.localdate()` (`app/ponto/utils.py:29`, `:34`). | **chamado** (`app/core/contratos_estruturais.py:51`). |
| D4 | `DisputaSupervisao.aberta_em` | Carimbo de nascimento automático (`app/chamados/models.py:1686`) × idade calculada subtraindo `.date()` cru do carimbo do **dia local** atual (`app/ponto/management/commands/alertar_disputas_pendentes.py:31`, `:52`, `:73`). | **chamado** (`app/core/contratos_estruturais.py:51`). |
| D5 | `Ausencia.data` e `data_fim` — par novo | O escritor mantém um **intervalo inclusivo**, possivelmente aberto (`app/ponto/services/ausencia.py:66`, `:75`, `:342`). O painel de fechamento seleciona apenas ausências cujo **início** está na competência (`app/ponto/views.py:1004`), embora a cobertura considere sobreposição (`app/ponto/turnos.py:2753`). | **ausencia/ferias**, com consumo em **folha/export** (`app/core/contratos_estruturais.py:134`, `:158`). |
| D6 | `AgendamentoFerias.data_inicio` e `data_fim` — pares novos | O escritor grava dias inclusivos, `fim = início + dias − 1` (`app/ferias/services.py:313`, `:322`). A disponibilidade converte uma janela de instantes **[início,fim)** em duas datas e consulta sobreposição inclusiva (`app/ponto/utils.py:143`, `:198`; `app/ferias/services.py:51`). Outro leitor apresenta o próprio último dia como “Retorno previsto” (`app/ponto/utils.py:111`). | **ausencia/ferias** (`app/core/contratos_estruturais.py:134`). |
| D7 | `DecisaoHE.data` | A porta grava a decisão sobre o dia informado (`app/ponto/portas/he.py:67`; `app/ponto/views.py:524`, `:529`). Um consumidor procura autorização pelo **dia da jornada** (`app/ponto/motor_calculo_v2.py:1456`, `:2461`); outro recompõe **dia civil da entrada** e consulta essa chave (`:1528`, `:1653`). | **turno/marcos**, efeito em cálculo (`app/docs/JUIZES.md:20`, `:29`). |
| D8 | `Empresa.janela_he_desde` e `_empresa_da_janela_do_dia.dia` | O fornecedor grava o **primeiro dia da competência** (`app/ponto/management/commands/diff_janela_he_total.py:104`, `:106`; porta em `app/colaboradores/services/estrutura.py:17`). A comparação de vigência recebe jornada em um caminho e civil da entrada em outro (`app/ponto/motor_calculo_v2.py:1393`, `:1451`, `:1528`, `:1529`). | **folha/export**, com geometria de **turno/marcos** (`app/docs/JUIZES.md:29`, `:35`). |
| D9 | `Batida.timestamp → dia derivado` — pares novos de DSR | O motor atribui e guarda o dia da jornada (`app/ponto/motor_calculo_v2.py:2403`, `:2408`, `:2460`). O DSR usa esse dia para presença, mas usa civil da entrada para extras e denominador (`:2578`, `:2639`, `:2643`). O comercial agrupa pela jornada e depois refaz DSR por civil (`:3313`, `:3317`, `:3441`, `:3454`). | **turno/marcos**, efeito em cálculo (`app/docs/JUIZES.md:20`, `:29`). |
| D10 | `_feriado_dobra_no_dia.dia` | O contrato exige **início do plantão** (`app/ponto/motor_calculo_v2.py:928`). Dois fornecedores passam `e.date()` do segmento (`:2809`, `:3255`); outros passam dia da jornada (`:3092`, `:3337`; `app/ponto/calculador/regras.py:338`, `:351`). | **turno/marcos**, efeito em cálculo (`app/docs/JUIZES.md:20`, `:29`). |
| D11 | `tripwire_celulas`: `ini/fim` de duas funções | Os defaults prometem competência da empresa, mas são produzidos sem empresa, portanto com corte padrão 21 (`app/ponto/management/commands/tripwire_celulas.py:78`, `:85`, `:232`, `:247`; `app/ponto/janelas.py:50`). Os leitores aplicam a mesma janela às células de várias empresas (`tripwire_celulas.py:97`, `:257`). | **celula/precedencia**, recorte de **folha/export** (`app/core/contratos_estruturais.py:47`, `:52`; `app/docs/JUIZES.md:35`). |

**(3) Contagem e limite de fechamento**

| Universo efetivamente examinado | Quantidade | Classificação |
|---|---:|---|
| Campos de modelo | **34** | **11 divergentes**, **6 não sei específicos**, **17 sem divergência na amostra**, mas sem fechamento universal dos escritores **[não verificado]**. |
| Posições de parâmetros, contando separadamente cada função/argumento | **186** | **6 divergentes**: os dois parâmetros de D8/D10 e os quatro extremos de D11; **180 sem fechamento dos fornecedores** **[não verificado]**. |
| Total examinado | **220** | **17 divergentes; 203 não sei para a pergunta universal; nenhum “não” universal demonstrado** **[não verificado quanto à exaustividade global]**. |

A contagem de parâmetros compreende **45** posições no cálculo/janelas, **96** no núcleo/portas, **44** nos comandos examinados e **1** em `utils.alertas_turno_visiveis`; inclui os nomes adicionais presentes no inventário, como `ini/fim`, `dia_corte`, booleanos e dicionários. As definições estão, respectivamente, em `app/ponto/calculador/regras.py:576`, `app/ponto/motor_calculo_v2.py:82`, `app/ponto/turnos.py:705`, `app/ponto/management/commands/tripwire_celulas.py:70` e `app/ponto/utils.py:222`; o detalhamento integral da contagem não foi persistido **[não verificado]**.

**(1) Quadro — divergentes e não sei**

“N” abaixo significa **sítios lidos**, não total global. Onde há fornecedores ou escritores adicionais sem censo encerrado, a totalidade é **[não verificado]**.

| Campo/parâmetro | Significados encontrados | Escritores: n e sítios | Leitores: n e amostra lida | Divergente? | Caso concreto → resultado incompatível |
|---|---|---|---|---|---|
| D1 — `OrdemSubstituicao.data_fim` | Instante final exclusivo × inclusivo (`app/ponto/utils.py:206`; `app/ponto/models.py:1147`). | **2**: `app/ponto/services/cobertura.py:106`, `:291`. | **5**: `app/ponto/utils.py:206`; `app/ponto/models.py:1147`, `:1156`; `app/ponto/management/commands/detectar_ausencias.py:370`; `app/ponto/management/commands/reconciliar_grade.py:409`. | **sim** | Ordem 06/11 07:00–19:00, consulta às 19:00: método declara ativa; disponibilidade para janela iniciando às 19:00 não reconhece sobreposição (`app/ponto/models.py:1147`; `app/ponto/utils.py:206`). |
| D2 — `Justificativa.data_turno` | Dia da jornada × dia civil da entrada (`app/ponto/management/commands/processar_alertas_turno.py:393`; `app/templates/ponto/justificativa_form.html:30`). | **3**: cron `processar_alertas_turno.py:393`; web `app/ponto/views.py:1638`; API `app/api/views.py:2010`. | **3**: busca da pendente, cobertura e invalidação (`app/ponto/views.py:1622`, `:1681`, `:1805`). | **sim** | Jornada 20/09, marcos 19:00–07:00, entrada 21/09 00:05 atribuída à véspera: cron procura/grava 20/09; formulário envia 21/09 e procura outra pendência (`app/ponto/turnos.py:299`, `:306`; `app/ponto/views.py:1622`). |
| D3 — `ChamadoColaborador.criado_em` | Instante de nascimento × chave diária UTC crua × chave diária local (`app/chamados/models.py:61`; `app/ponto/views.py:3015`; `app/ponto/utils.py:29`). | **1 mecanismo automático**, acionado pelo nascimento em `app/chamados/models.py:932`. | **3 amostras**: `app/ponto/views.py:3015`; `app/ponto/utils.py:34`, `:97`. | **sim** | Em 07/11 01:00Z, ainda 06/11 22:00 local, o emissor procura dia 07/11; o chamado recém-criado pertence ao dia local 06/11. A repetição pode deixar de encontrá-lo (`app/ponto/views.py:2946`, `:3015`; `app/chamados/models.py:867`). |
| D4 — `DisputaSupervisao.aberta_em` | Instante de abertura × idade por data UTC/local misturadas (`app/chamados/models.py:1686`; `app/ponto/management/commands/alertar_disputas_pendentes.py:52`). | **1 mecanismo automático**; **3 nascimentos lidos** em `app/chamados/services/disputa_emissao.py:523`, `:962`, `:1182`. | **2**, ambos lidos: `app/ponto/management/commands/alertar_disputas_pendentes.py:52`, `:73`. | **sim** | Abertura 06/11 02:30Z = 05/11 23:30 local; execução 06/11 08:00 local → “hoje”, embora tenha sido “ontem” (`alertar_disputas_pendentes.py:52`, `:54`, `:56`; fuso em `app/config/settings/base.py:93`, `:95`). |
| D5 — `Ausencia.data/data_fim` | Intervalo inclusivo/aberto × seleção somente pelo início (`app/ponto/services/ausencia.py:66`; `app/ponto/views.py:1004`). | **2 atos da porta**: criação `app/ponto/services/ausencia.py:342`; edição `:499`, `:500`; normalização adicional em `app/ponto/models.py:907`. | **2 amostras**: painel `app/ponto/views.py:1004`; cobertura `app/ponto/turnos.py:2753`. | **sim**, 2 campos | Ausência 01/09–30/09, competência 21/09–20/10 → cobre 21–30/09, mas não aparece na lista de ausências do painel porque começou antes de 21/09 (`app/ponto/views.py:1004`). |
| D6 — `AgendamentoFerias.data_inicio/data_fim` | Dias inclusivos × projeção de janela exclusiva; último dia × retorno (`app/ferias/services.py:313`; `app/ponto/utils.py:143`, `:198`, `:111`). | **1 ato de criação lido**: `app/ferias/services.py:322`; demais atos globais **[não verificado]**. | **2 amostras**: disponibilidade `app/ponto/utils.py:198`; descrição `:111`. | **sim**, 2 campos | Cobertura `[06/11 19:00,07/11 00:00)` e férias iniciando em 07/11 → pessoa excluída apesar de não haver interseção de instantes. Férias até 20/11 → texto anuncia “Retorno previsto: 20/11”, embora diga que volta no dia seguinte (`app/ponto/utils.py:198`, `:111`). |
| D7 — `DecisaoHE.data` | Dia autorizado/jornada × civil da entrada (`app/ponto/portas/he.py:67`; `app/ponto/motor_calculo_v2.py:1456`, `:1528`). | **1 sítio da data**: `app/ponto/portas/he.py:67`. | **3**: carga `app/ponto/motor_calculo_v2.py:1417`; consultas `:1456`, `:1653`. | **sim** | Autorização em 20/09; jornada 20/09 com entrada 21/09 02:30 e saída 07:00 → consulta por 21/09 ignora autorização. Com marco 19:00, a entrada pode ser deslocada para 21/09 19:00 (`app/ponto/motor_calculo_v2.py:1528`, `:1653`; `app/ponto/janela_he.py:94`, `:102`). |
| D8 — `Empresa.janela_he_desde`; `_empresa_da_janela_do_dia.dia` | Vigência a partir da competência/jornada × dia civil do segmento (`app/colaboradores/models.py:119`; `app/ponto/motor_calculo_v2.py:1451`, `:1529`). | Campo: **2 fornecedores A3 lidos**, `app/ponto/management/commands/diff_janela_he.py:103`, `diff_janela_he_total.py:106`, pela porta `app/colaboradores/services/estrutura.py:17`. Parâmetro: **2 chamadas**, motor `:1451`, `:1529`. | **1 comparação de vigência**: `app/ponto/motor_calculo_v2.py:1393`. | **sim**, 1 campo + 1 parâmetro | Vigência 21/09; jornada 20/09 com entrada civil em 21/09 → ramo da jornada considera anterior à vigência; ramo civil aplica a janela. Delta monetário exato **[não verificado]** (`app/ponto/motor_calculo_v2.py:1393`, `:1528`). |
| D9 — `Batida.timestamp → dia derivado` | Instante preservado; atribuição posterior à jornada × reextração civil no DSR (`app/ponto/registro_batida.py:153`; `app/ponto/motor_calculo_v2.py:2460`, `:2639`). | **2 criações da batida**: `app/ponto/registro_batida.py:153`, `:158`; **2 atribuições de dia examinadas**: motor `:2460`, `:3313`. | **5 grupos de leitura**: presença `:2578`; reflexo `:2639`, `:2643`; banco comercial `:3423`; presença comercial `:3441`, `:3454`; reflexo comercial `:3466`, `:3470`. | **sim**, no dia derivado | Competência termina domingo 20/09; período atribuído a 20/09, entrada civil 21/09 e 60 min de HE → presença entra na semana 14–20, mas seus 60 min ficam fora do reflexo dessa semana. O comercial ainda pode marcar falta em 20/09 (`app/ponto/motor_calculo_v2.py:2429`, `:2431`, `:2639`, `:3454`). |
| D10 — `_feriado_dobra_no_dia.dia` | Início do plantão × data civil do segmento (`app/ponto/motor_calculo_v2.py:928`, `:2809`). | **5 fornecedores lidos**: motor `:2809`, `:3255`, `:3092`, `:3337`; `app/ponto/calculador/regras.py:351`. | **1 predicado**: `app/ponto/motor_calculo_v2.py:934`. | **sim** | Jornada 11/10, cauda 12/10 03:00–07:00, feriado em 12/10, dobra ligada e janela desligada → ramo por segmento lança 240 min de HE100 feriado; consulta pela jornada 11/10 devolve zero (`app/ponto/motor_calculo_v2.py:2809`, `:2812`; `app/ponto/calculador/regras.py:351`). |
| D11 — `medir_pausa_faltando.ini/fim`; `medir_sobra_com_tipo_trocado.ini/fim` | Competência empresarial prometida × competência padrão 21–20 (`app/ponto/management/commands/tripwire_celulas.py:78`, `:85`, `:232`, `:247`). | **1 atribuição conjunta por função**: `tripwire_celulas.py:86`, `:248`; chamadas sem argumentos em `:378`, `:389`. | **1 filtro por função**: `tripwire_celulas.py:97`, `:257`. | **sim**, 4 parâmetros | Empresa corte 5; referência 06/11/2026 → deveria consultar 05/11–04/12, mas consulta 21/10–20/11. Inclui dias da competência anterior e omite 21/11–04/12 (`app/ponto/janelas.py:24`, `:50`, `:63`). |
| `PerguntaDisputa.data_evento` — par novo | Dia do fato; para conversa, dia local da emissão sem identidade factual (`app/chamados/models.py:2403`, `:2422`, `:2427`). O mapa usa igualdade da data como “furo coberto por pergunta” (`app/ponto/management/commands/mapa_divergencia.py:264`, `:275`). | **2 atos abertos**: nascimento `app/chamados/models.py:2476`; preenchimento no reapontamento `app/chamados/services/fio.py:832`, `:847`. Total **[não verificado]**. | **1 filtro + 1 extração**: `app/ponto/management/commands/mapa_divergencia.py:272`, `:273`. | **não sei** | Conversa `ancora_ausente` emitida em 06/11 e furo em 06/11 → `furo_com_pd` sobe 1 e o dia sai do restante. Isso ocorre pelo código; se a métrica pretende cobertura **factual** ou presença de qualquer pergunta permanece **[não verificado]** (`mapa_divergencia.py:275`, `:276`). |
| `EventoCelula.data` | Snapshot do dia da célula (`app/ponto/portas/celula.py:891`). | **1**: `app/ponto/portas/celula.py:891`. | **0 leitores semânticos encontrados**; consumidor aberto agrupa por colaborador (`app/chamados/reconciliador.py:1340`). Total **[não verificado]**. | **não sei** | Não há par leitor demonstrado nem saída errada demonstrada **[não verificado]**. |
| `FuroDiario.avaliada_em` | Instante da avaliação (`app/ponto/portas/celula.py:1127`). | **1**: `app/ponto/portas/celula.py:1127`. | **0 leitores do carimbo encontrados**; leitor aberto usa a data do furo (`app/escala/services/furos_diarios.py:133`). Total **[não verificado]**. | **não sei** | Consumidor temporal do carimbo e caso que quebra **[não verificado]**. |
| `Colaborador.data_demissao` | Data do desligamento; dia ainda não é “posterior à demissão” no suprajuiz, mas já exclui disponibilidade quando igual ao fim solicitado (`app/colaboradores/services/desligamento.py:105`; `app/ponto/supra_juiz.py:194`; `app/ponto/utils.py:180`). | **2 caminhos abertos**: agendamento `app/colaboradores/services/desligamento.py:85`; efetivação `:105`. Total **[não verificado]**. | **2 amostras**: `app/ponto/supra_juiz.py:194`; `app/ponto/utils.py:180`. | **não sei** | Demissão em 20/11 e cobertura terminando em 20/11 → disponibilidade exclui; outro leitor só considera posterior a partir de 21/11. Se é política específica de disponibilidade ou discordância do último dia útil **[não verificado]**. |
| `Justificativa.aprovada_em` | Instante da decisão, incluindo rejeição (`app/ponto/views.py:1798`, `:1877`). | **3 sítios abertos**: `app/ponto/views.py:1798`, `:1877`; `app/ponto/services/justificativa.py:370`. Total **[não verificado]**. | Leitor semântico do valor não fechado **[não verificado]**. | **não sei** | Caso que quebra **[não verificado]**. |
| `Justificativa.atualizado_em` | Última gravação automática × card de decisões “hoje” (`app/ponto/models.py:180`; `app/ponto/views.py:1738`, `:1740`). | **1 mecanismo automático**; acionadores completos **[não verificado]**. | **1 filtro lido**: `app/ponto/views.py:1740`. | **não sei** | Uma alteração posterior em justificativa decidida poderia deslocar o card; caminho concreto posterior não demonstrado **[não verificado]**. |
| `montar_lampadas.data`, incluído nos 180 parâmetros sem fechamento | Dia âncora; com `hi==hf`, a lâmpada final fica em D, enquanto o encadeamento da autoridade produz D+1 (`app/ponto/management/commands/gerar_celulas.py:67`; `app/escala/servico_jornada.py:82`). | **1 produtor aberto**: `app/ponto/management/commands/gerar_celulas.py:36`. | **1 consumidor aberto**, protegido por veto à igualdade (`app/ponto/management/commands/reconciliar_grade.py:159`, `:169`; `app/ponto/marcos.py:108`). | **não sei** | D=06/11, `hi=hf=15:00` → lâmpada final 06/11 15:00 versus autoridade 07/11 15:00. O leitor examinado não alcança o caso; outro consumidor desprotegido **[não verificado]**. |

**Demais 17 campos: sem divergência nas amostras, sem “não” universal.** São os seis campos examinados de `CelulaDia` (`data`, `regenerada_em`, `editada_em`, `julgada_em`, `veredito_em`, `insumos_em`), `FuroDiario.data`, `DecisaoHE.revogada_em`, os dois campos de `TurnoMaterializado`, `OrdemSubstituicao.data_inicio`, `Justificativa.saida_informada/criado_em`, `Ausencia.aprovada_em/prazo_documento`, `PingGeo.timestamp` e `Colaborador.notificacao_negada_em`. As amostras incluem confrontos de carimbos em `app/escala/services/regeneracao.py:84`, vigência em `app/ponto/models.py:1257`, jornada/fim previsto em `app/ponto/management/commands/processar_alertas_turno.py:97`, `:100`, materialização da saída em `app/ponto/views.py:1810`, prazo em `:2555`, ordenação dos pings em `:3008` e presença do carimbo de negativa em `:1959`; a totalidade dos escritores globais desses 17 campos permanece **[não verificado]**.

**Demais parâmetros:** dos **180 sem fechamento**, incluindo `montar_lampadas.data` acima, não foi exaurida a origem de todos os argumentos; ausência de contradição no corpo não prova concordância de todos os fornecedores **[não verificado]**. Há também nomes não temporais: sete `dia` são dicionários da grade (`app/ponto/propositor.py:34`; `app/ponto/turnos.py:2644`; `app/ponto/status_dia.py:166`; `app/ponto/supra_juiz.py:60`, `:65`, `:109`, `:204`), além de outros dois dicionários nos comandos (`app/ponto/management/commands/e6_oraculo.py:203`, `:230`); `dia_de_trabalho` é booleano (`app/ponto/calculador/regras.py:577`).

## 06/11 19:25 · sessao 8ee5489b7bff · - · - · origem=Ronald (tmux)

Censo somente leitura, reproduzido com `python3 -B`, carregando `bin/quem_chama.py` por `exec(compile(...))` e preservando tudo em memória.

1. **Cabeçalho emitido: 31 sítios únicos.** O conjunto é recolhido por `varrer_py` e deduplicado no render. (`bin/quem_chama.py:731`, `bin/quem_chama.py:1130`)

   - `app/api/tests/test_me_posto_geo.py:113,126`
   - `app/chamados/tests/test_contract_crons.py:22`
   - `app/chamados/tests/test_o137_esmeril_chamado.py:160`
   - `app/chamados/tests/test_selo_minimo_modulos.py:54`
   - `app/core/erros.py:51`
   - `app/core/intake.py:487,494`
   - `app/core/management/commands/describe.py:80`
   - `app/core/tests/test_contract_direcao_a16.py:157`
   - `app/core/tests/test_contract_import_local_resolve.py:75,86`
   - `app/core/tests/test_marca_cron15.py:64`
   - `app/core/tests/test_selo_contratos_estruturais.py:24,76,86,93,113,116,126,179,202,204,207`
   - `app/core/tests/test_selo_estrutural.py:28,31`
   - `app/escala/tests/test_selo_grade_pede_o_instante.py:207`
   - `app/ponto/tests/test_a4_estados_com_porta.py:191`
   - `bin/leitor_dia_estado.py:32`
   - `bin/prova_de_casca.py:86`
   - `bin/recusa_estado.py:31`

   “Opaco” **não significa ausência de literal no arquivo**: `bin/prova_de_casca.py:68` fornece nomes completos, `bin/leitor_dia_estado.py:25` fornece basename e `app/chamados/tests/test_selo_minimo_modulos.py:23` fornece módulos completos. Essas chamadas entram no cabeçalho porque sua expressão de argumento não foi resolvida; os mesmos arquivos ainda podem ligar os alvos lexicalmente. (`bin/quem_chama.py:695`, `bin/quem_chama.py:781`, `bin/quem_chama.py:785`)

2. **Omissões operacionais do cabeçalho: quatro `exec`.** Remover somente a condição `nome not in ('exec', 'compile')` adiciona exatamente estes quatro sítios, levando o conjunto a 35. (`bin/quem_chama.py:689`)

   - `app/core/tests/test_access_log_persistido.py:76`
   - `app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:43`
   - `app/ponto/tests/test_o219_versao_da_regra.py:253`
   - `bin/sondas_rollback.py:53`

   Nem todos carregam um módulo inteiro: os dois testes de `ponto` executam fragmentos AST. O caso `test_access_log_persistido` executa as configurações nomeadas no próprio arquivo, que continuam ligadas pelos basenames; portanto a omissão do cabeçalho não prova zero nesse caso. (`app/ponto/tests/test_logger_ponto_vai_para_arquivo.py:38`, `app/ponto/tests/test_o219_versao_da_regra.py:247`, `app/core/tests/test_access_log_persistido.py:37`, `app/core/tests/test_access_log_persistido.py:73`)

3. **Prefixos reais: sete ocorrências.**

   - `config.settings.` nos três testes: `app/core/tests/test_selo_erro_com_mensagem.py:82`, `app/core/tests/test_selo_log_nao_e_cego.py:122`, `app/core/tests/test_trilha_com_contexto.py:105`.
   - `ponto.management.commands.`: `app/ponto/tests/test_detectores_isencao.py:8`.
   - `relatorios.lote_espelho`: `app/relatorios/tests/test_lote_erro_diz_que_e_erro.py:68,126`.
   - Caminho `sonda:`: `bin/sondas_rollback.py:53`.

   Os dois últimos prefixos não acrescentam falso alvo real observado: o único módulo começando por `relatorios.lote_espelho` é `app/relatorios/lote_espelho.py`; nenhum caminho-alvo começa por `sonda:`. Existência/comparação verificadas pela cópia; execução real desses caminhos [não verificado].

4. **Falsos positivos por sítio.**

   Os três testes de settings enumeram apenas `base`, `saas`, `prod`, mas associam também os seis perfis `ci`, `ci_tenants`, `dev`, `smoke`, `sombra`, `teste_motor`. O pacote `__init__` não deve ser contado como falso: importar filhos carrega o pai. Essas facetas de símbolo já eram não varridas por serem settings; o prefixo não muda seu tipo. (`app/core/tests/test_selo_erro_com_mensagem.py:80`, `app/core/tests/test_selo_log_nao_e_cego.py:120`, `app/core/tests/test_trilha_com_contexto.py:103`, `bin/quem_chama.py:384`)

   O teste dos detectores chama somente três commands: `detectar_ausencias`, `processar_alertas_turno`, `processar_alertas_avancados`. O prefixo associa os 104 arquivos `.py` existentes naquele diretório: três chamados, o pacote pai e **100 atribuições falsas àquele sítio**. Desses 100, 50 têm algum símbolo originalmente varrível; os demais contêm apenas `Command`/membros ou nenhum símbolo, sem mudança para LOADER. (`app/ponto/tests/test_detectores_isencao.py:8`, `app/ponto/tests/test_detectores_isencao.py:12`, `app/ponto/tests/test_detectores_isencao.py:15`, `app/ponto/tests/test_detectores_isencao.py:18`, `bin/quem_chama.py:380`, `bin/quem_chama.py:382`)

5. **Rebaixamentos efetivamente novos na simulação com todos os commands tocados.**

   Montei em memória um range hipotético alterando todas as linhas dos 104 commands, com o restante da árvore fornecido pela cópia. **125 símbolos de 49 arquivos** ficam LOADER exclusivamente pelos achados de prefixo. Desses, 121 símbolos em **47 arquivos falsamente atribuídos ao sítio**; os outros quatro símbolos pertencem aos dois commands efetivamente carregados `detectar_ausencias` e `processar_alertas_turno`. Isso é simulação sobre a cópia, não o RESUMO histórico do range real. (`bin/quem_chama.py:739`, `bin/quem_chama.py:1089`)

   Dos 50 candidatos já enviados ao pai, excluir `detectar_par_relampago.py`, `e6_oraculo.py` e `reconciliar_grade.py`: já recebem LOADER por outros sítios, portanto seu tipo não passa a LOADER exclusivamente pelo prefixo.

6. **Três zeros estáticos reais da cópia ficam escondidos por associação falsa de prefixo.** Ao retirar somente os achados “por nome/caminho montado”, estes três símbolos voltam a zero:

   - `app/ponto/management/commands/gerar_celulas.py:19` — `_chaves_marcos`.
   - `app/ponto/management/commands/passivo_disputa_retro.py:45` — `_competencia_de`.
   - `app/ponto/management/commands/testar_pwa.py:27` — `WARN`.

   Os dois primeiros nomes aparecem somente em suas definições no universo `app/**, bin/**`, segundo busca textual na cópia. O sítio `test_detectores_isencao.py:8` não carrega esses commands. O quarto zero estático escondido é `processar_alertas_turno.py:26::TIPOS_POS`, mas esse módulo é efetivamente carregado pelo teste, logo seu rebaixamento é correto. (`app/ponto/tests/test_detectores_isencao.py:15`)

   **Ausência de consumidores externos/dinâmicos em produção e “zero verdadeiro” de produção: [não verificado].** Há prova de zero do próprio contador na cópia e de que o prefixo falso o remove, suficiente para documentar o custo admitido pela triagem.

7. **Zeros globais do range real e quantidade de alvos carregados exclusivamente por opacos externos: [não verificado].** A cópia não contém `.git`; o diff fornecido toca apenas o instrumento e seu selo, não todos os módulos candidatos. Não confundir os 31 sítios emitidos com 31 falsos zeros: vários possuem literais locais suficientes, e outros visam testes/settings já fora da contagem. (`bin/quem_chama.py:360`, `bin/quem_chama.py:384`, `bin/quem_chama.py:781`, `bin/quem_chama.py:785`)

## 06/11 19:23 · sessao c91fdee52e15 · - · - · origem=Ronald (tmux)

Achados novos do bloco, por leitura estática; não há medida de incidência em PROD [nao verificado].

1. **`Justificativa.data_turno`: divergente.** `triagem_bater_ponto` lê todas as batidas, sem recorte pelo dia, e guarda a data civil local da entrada no contexto; `ramo_regularizar_saida` grava essa data em `Justificativa.data_turno` (`app/ponto/services/triagem_batida.py:175`, `:178`, `:180`, `:185`, `:239`, `:260`). O leitor `efeitos_da_ausencia` usa o campo como dia a justificar no intervalo da folha (`app/ponto/services/efeito_ausencia.py:40`, `:43`); `recalcular_fechamento_mes` também (`app/ponto/services/fechamento.py:651`, `:654`). Caso: entrada **21/09/2026 00:00 −03**, turno previsto da véspera **23:50–07:50**, sem turno anterior ocupando a véspera: `_data_do_turno` responde **20/09**, mas a triagem grava **21/09** (`app/ponto/turnos.py:283`, `:299`, `:306`). Depois de aprovada, consultar justificadas de **20/09–20/09** exclui essa justificativa (`app/ponto/services/efeito_ausencia.py:43`). A aprovação ainda invalida o fechamento pelo dia **21/09** (`app/ponto/services/justificativa.py:200`). Família do leitor: **folha/export**; origem da discordância: **turno/marcos**, conforme `app/core/contratos_estruturais.py:48`, `:52` e `app/docs/JUIZES.md:20`.

   Acessibilidade: `_tad` só muda o contexto `turno_aberto`; não retira a entrada de `entradas_sem_saida`, cuja guarda é duração superior a meia hora (`app/ponto/services/triagem_batida.py:191`, `:199`, `:205`). **Não atribuir esse mesmo exemplo ao escritor `regularizacao.py:94`**: seu recorte anterior pode recusar antes da gravação [nao verificado neste bloco].

2. **Dia produzido pela fila de flip: divergente, novo par para `Batida.timestamp`.** `fila_flip_recusas` produz `dia=localtime(timestamp).date()` — dia civil (`app/ponto/services/flip_auto.py:138`, `:140`). `avaliar_fila` entrega esse dia a `dia_encerrado`, que responde pelo dia do turno (`app/ponto/services/flip_auto.py:268`, `:285`; `app/ponto/turnos.py:2043`, `:2046`). Caso: vizinha **S21/09 07:00**, pertencente ao turno **20/09 19:00–21/09 07:00**, já fechado; agora **21/09 08:00**, com próximo turno previsto **21/09 19:00–22/09 07:00**. A fila pergunta por **21/09**, registra `dia_em_curso` e pula o item; perguntou pelo turno seguinte (`app/ponto/services/flip_auto.py:285`, `:286`). A primeira fonte também produz dia civil e o consome como dia do turno em `batidas_do_dia_turno` (`app/ponto/services/flip_auto.py:63`, `:104`). Família: **turno/marcos** (`app/docs/JUIZES.md:20`).

   O parâmetro **`flip_auto.etiquetas_pela_celula.dia`** participa da mesma mistura: recebe o dia civil da fila, busca DNA desse dia e só tenta a véspera quando a célula consultada diz folga (`app/ponto/services/flip_auto.py:201`, `:208`, `:214`, `:310`).

3. **Payload `data_turno` + hora de marco em `lastro.julgar`: divergente, par novo C9.** A emissão recebe o dia da célula e os marcos faltantes; grava `data_turno` e `marcos_faltantes` juntos (`app/ponto/services/cartorio.py:1778`, `:1779`; `app/ponto/management/commands/detectar_ausencias.py:706`, `:709`). O fallback de `lastro.julgar` combina toda hora com aquele mesmo dia civil, sem encadear a madrugada (`app/ponto/services/lastro.py:304`, `:305`, `:306`, `:308`). Caso: `data_turno=20/09/2026`, marco **S07:00**, batidas **E20/09 19:00**, **S21/09 07:00**, sem lâmpada correspondente na ata: o alvo vira **20/09 07:00**, a saída fica a **1.440 min**, excedendo a tolerância de **90 min**, e o leitor classifica `outro_horario` (`app/ponto/services/lastro.py:59`, `:289`, `:299`, `:308`, `:320`). Família do payload: **chamado**; geometria discordante: **turno/marcos** (`app/docs/JUIZES.md:24`, `:56`). O fallback é condicional à ausência da lâmpada; com lâmpada encontrada, prevalece sua leitura (`app/ponto/services/lastro.py:289`, `:298`).

**Campos distintos encontrados neste bloco: 19**, contando os três de `MetricaSnapshot` e `LogAuditoria.criado_em`, ausentes das linhas CAMPO do anexo, mas usados pelos serviços (`app/inteligencia/models.py:90`, `:91`, `:96`; `app/ponto/services/retratar_humano.py:39`):

| Campo | Leitura/escrita examinada; resultado local |
|---|---|
| `Batida.timestamp` | Divergência da fila flip acima; outros usos locais convertem para hora local ou comparam instantes (`app/ponto/services/flip_auto.py:138`, `:140`, `:285`; `app/ponto/services/triagem_batida.py:669`, `:674`). |
| `Batida.retratada_em` | Instante usado como presença/ausência e prazo de sete dias; borda inclusiva coerente entre botão e execução (`app/ponto/services/retratar_humano.py:53`, `:109`, `:203`). |
| `Justificativa.data_turno` | Divergente acima (`app/ponto/services/triagem_batida.py:260`). |
| `Justificativa.saida_informada` | Datetime local aware, com data explícita ou virada do dia; sem divergência observada nesse escritor (`app/ponto/services/triagem_batida.py:247`, `:254`, `:256`, `:264`). |
| `EscalaColaborador.data_inicio` | Ordenação por início de vigência (`app/ponto/services/faltas_de_hoje.py:36`). |
| `EscalaColaborador.data_fim` | Teste de ausência de fim (`app/ponto/services/cluster_espurio.py:119`). |
| `EscalaColaborador.regua_corroborada_em` | Carimbo devolvido sem transformação (`app/ponto/services/furos_vetados.py:70`). |
| `CelulaDia.data` | Dia da célula; intervalos inclusivos e consultas por igualdade coerentes nos sítios amostrados (`app/ponto/services/cluster_espurio.py:46`; `app/ponto/services/vigia_entrada.py:120`; `app/ponto/services/passivo_s84.py:94`). |
| `CelulaDia.julgada_em` | Instante repassado ao julgamento anterior (`app/ponto/services/cluster_espurio.py:104`). |
| `FuroDiario.data` | Dia da lâmpada, intervalo inclusivo (`app/ponto/services/deslogados.py:99`, `:101`). |
| `TurnoMaterializado.data_turno` | Dia do turno usado para recorte de recomputação (`app/ponto/services/flip_batida.py:38`, `:39`). |
| `PerguntaDisputa.validada_em` | Instante: corte móvel e procedência (`app/ponto/services/flip_auto.py:126`, `:130`; `app/ponto/services/procedencia_batida.py:74`). |
| `PerguntaDisputa.materializada_em` | Instante alternativo de procedência (`app/ponto/services/procedencia_batida.py:74`). |
| `ChamadoColaborador.criado_em` | Janelas de instante e apresentação local nos sítios lidos (`app/ponto/services/triagem_batida.py:455`; `app/ponto/services/faltas_de_hoje.py:84`). |
| `ChamadoColaborador.prazo_sla` | Produzido pelo juiz do prazo sobre `timezone.now()` (`app/ponto/services/triagem_batida.py:346`, `:469`). |
| `LogAuditoria.criado_em` | Ordenação por instante, exibição em localtime (`app/ponto/services/retratar_humano.py:39`, `:94`; `app/ponto/services/flip_batida.py:77`). |
| `MetricaSnapshot.data_ini` | Dia da medição nestas duas chaves (`app/ponto/services/teto_na_lista.py:115`; `app/ponto/services/vinculo_na_lista.py:120`). |
| `MetricaSnapshot.data_ref` | Dia da medição e ordenação por data (`app/ponto/services/teto_na_lista.py:114`, `:124`; `app/ponto/services/vinculo_na_lista.py:119`, `:129`). |
| `MetricaSnapshot.calculado_em` | Instante automático e apresentação ISO (`app/inteligencia/models.py:96`; `app/ponto/services/teto_na_lista.py:128`; `app/ponto/services/vinculo_na_lista.py:133`). |

Os **17 campos restantes** não apresentaram divergência nos sítios locais acima; a unanimidade de todos os escritores externos desses campos não foi certificada por este explorador [nao verificado].

Parâmetros do inventário neste bloco: **10**, já enviados com identidades; **1 divergente** (`etiquetas_pela_celula.dia`), **9 sem divergência local observada**. Esses nove são `furos_vetados.{vinculo_do_emissor.data,motivo_do_veto.data,furos_vetados_por_regua.ini/fim}`, `lastro.{marcos_acesos_do_dia.data,_texto_da_nota.dia}`, `passivo_s84.competencia_de.dia`, `simular_dia.grade_dia.data`, `vigia_entrada.do_dia.data` (`app/ponto/services/furos_vetados.py:18`, `:27`, `:47`, `:61`; `app/ponto/services/lastro.py:185`, `:193`, `:343`, `:354`; `app/ponto/services/passivo_s84.py:38`, `:46`; `app/ponto/services/simular_dia.py:5`, `:7`; `app/ponto/services/vigia_entrada.py:111`, `:120`).

## 06/11 19:23 · sessao 4d7422dcc947 · - · - · origem=Ronald (tmux)

Recorte auditado: 18 arquivos atribuídos, 33 parâmetros temporais, sendo 17 `dia/data_*` e 16 `data/ini/fim`; identidades e declarações constam nos sítios listados abaixo. Contagens referem funções distintas deste recorte, não execuções. [nao verificado: medição por AST em memória]

| Campo/parâmetro | Divergência e caso concreto | Escritores no recorte | Leitores no recorte |
|---|---|---|---|
| `Ausencia.data_fim` | `None` significa intervalo aberto para `TIPOS_EM_ABERTO`, mas leitores o reduzem ao início. Ausência aberta em 01/10, consulta 01–06/11 → `decididos_do_periodo` devolve `{}` e relatório em lote devolve `[]`. Escrita: `app/ponto/services/ausencia.py:70`, `:197`, `:343`; leituras: `app/ponto/services/dia_decidido.py:552`, `:558`; `app/ponto/services/atestados_lote.py:58`. | 2 explícitos: `criar_ausencia`, `editar_ausencia_registro` — `app/ponto/services/ausencia.py:343`, `:500`. `mudar_estado:834` também repassa o campo ao `save`, cuja derivação está em `app/ponto/models.py:902`. | 8 diretos: criar `ausencia.py:286`; editar `:497`; abrir chamado `:652`; decidir `:947`; decididos `dia_decidido.py:552`; linha do tempo `:616`; vigia `vigia_ausencia.py:92`; censo `vigia_ausencia_veredito.py:48`. Mais leitor derivado: `atestados_lote.py:58`. |
| `Ausencia.aprovada_em` | Instante aware entregue cru ao CSV, enquanto painel o apresenta em fuso local. `2026-11-06T01:30Z` → CSV `06/11/2026 01:30`, painel `05/11/2026 22:30`. Escrita: `app/ponto/services/ausencia.py:349`; transporte: `app/ponto/services/dia_decidido.py:508`; CSV: `app/ponto/services/atestados_lote.py:131`; painel: `app/templates/ponto/partials/painel_ausencia.html:261`; fuso/configuração: `app/config/settings/base.py:93`, `:95`. | 2: criar `ausencia.py:349`; decidir `:917` — este aceita carimbo passado pelo chamador. O modelo auto-carimba quando ausente: `app/ponto/models.py:873`. | 1 direto: `quem_quando`, `dia_decidido.py:508`; mais CSV derivado `atestados_lote.py:131`. |
| `Ausencia.criado_em` | Mesmo desacordo UTC/local quando `aprovada_em` é nulo, pois vira o carimbo do CSV. Mesmo caso concreto acima. Fallback: `app/ponto/services/dia_decidido.py:508`; CSV: `app/ponto/services/atestados_lote.py:131`; painel do mesmo DTO: `app/templates/ponto/partials/painel_ausencia.html:188`. | 0 explícitos no recorte; `auto_now_add` em `app/ponto/models.py:813`. | 2 diretos: `quem_quando`, `dia_decidido.py:508`; `vigia_ausencia.contadores`, `vigia_ausencia.py:109`, `:188`, `:193`. Mais CSV derivado `atestados_lote.py:131`. |
| `_bateu_no_dia.dia` | Dia civil é transformado em janela terminando na meia-noite seguinte, mas o juiz inclui essa ponta. Batida única em **07/11/2026 00:00 local**, consulta por **06/11** → `True`; consumidor pode excluir a pessoa como quem “já bateu hoje”. Montagem: `app/ponto/services/abriu_nao_bateu.py:172`; fim inclusivo: `app/ponto/turnos.py:2868`; exclusão: `app/ponto/services/abriu_nao_bateu.py:130`. | 1 produtor da janela: `app/ponto/services/abriu_nao_bateu.py:172`. | 1 consumidor direto do parâmetro: mesma função, chamada `:174`; efeito em `com_nomes:130`. |

Os três casos de CSV/ausência aberta foram caracterizados usando os corpos reais extraídos por AST, com objetos e cargas em memória; nenhum Django, banco, log ou arquivo foi escrito. [nao verificado: execução da caracterização sem artefato persistido]

Pares divergentes e famílias:

- **`ausencia/ferias`**: escritor `app/ponto/services/ausencia.py:343` × leitores `app/ponto/services/dia_decidido.py:552`, `:558` e `app/ponto/services/atestados_lote.py:58`. Família declarada em `app/core/contratos_estruturais.py:126`.
- **`ausencia/ferias`**: escritor `app/ponto/services/ausencia.py:349` e auto-carimbo `app/ponto/models.py:873` × leitor `app/ponto/services/atestados_lote.py:131`, via `app/ponto/services/dia_decidido.py:508`. Família: `app/core/contratos_estruturais.py:126`.
- **`ausencia/ferias`**: escritor automático `app/ponto/models.py:813` × leitor CSV `app/ponto/services/atestados_lote.py:131`, pelo fallback `app/ponto/services/dia_decidido.py:508`. Família: `app/core/contratos_estruturais.py:126`.
- **`batida`**, com borda de janela: escritor do instante `app/ponto/registro_batida.py:158` × leitor `app/ponto/services/abriu_nao_bateu.py:172`, que chama o intervalo inclusivo de `app/ponto/turnos.py:2868`. Família declarada em `app/core/contratos_estruturais.py:46`.

Há um desacordo adicional de **dia civil × dia do turno** em `regularizacao`, mas o caso é barrado antes da escrita: `app/ponto/services/regularizacao.py:60` começa a busca na data civil da entrada; `turnos_do_colab` filtra por data do turno (`app/ponto/turnos.py:1849`, `:1993`). Para E=21/09 00:00 local e turno da véspera 23:50–07:50, o juiz atribui 20/09 (`app/ponto/turnos.py:299`, `:306`); a busca iniciada em 21/09 perde o turno e responde “Esta batida nao inicia um turno aberto” (`app/ponto/services/regularizacao.py:64`). Não afirmar que `Justificativa.data_turno` errado foi gravado: esse ramo não chega à escrita `regularizacao.py:94`.

Identidades examinadas para união pelo pai:

- `Batida.timestamp`, `Batida.retratada_em`: `app/ponto/services/adesao.py:35`, `:45`; `AlertaTurno.data_turno`: `app/ponto/services/alerta_turno.py:25`.
- `Ausencia.data/data_fim/prazo_documento/aprovada_em/atualizado_em/criado_em`: `app/ponto/services/ausencia.py:342`, `:343`, `:345`, `:349`, `:1283`; `app/ponto/services/vigia_ausencia.py:109`.
- `AgendamentoFerias.data_inicio/data_fim`: `app/ponto/services/ausencia.py:1184`.
- `Justificativa.data_turno/saida_informada/aprovada_em/atualizado_em`: `app/ponto/services/regularizacao.py:94`, `:97`, `:106`; `app/ponto/services/justificativa.py:192`.
- `ChamadoColaborador.criado_em`: `app/ponto/services/justificativa.py:54`; `DisputaSupervisao.fechada_em`: `app/ponto/services/entrada_sincronizada.py:56`; `CelulaDia.data`: `app/ponto/services/vigia_ausencia_veredito.py:53`.
- Descobertos fora da lista CAMPO fornecida: `MetricaSnapshot.data_ref/calculado_em`, declarados em `app/inteligencia/models.py:91`, `:96`, lidos em `app/ponto/services/abriu_nao_bateu.py:157`, `:161`.

Nos 33 parâmetros, apenas `_bateu_no_dia.dia` apresentou divergência própria comprovada; os erros de intervalo aberto pertencem ao significado de `Ausencia.data_fim`, sem tornar `data_ini/data_fim` dos leitores unidades diferentes. [nao verificado: classificação consolidada do recorte]

## 06/11 19:23 · sessao 53ef591127f2 · - · - · origem=Ronald (tmux)

Achados e mapa para consolidação, todos em `bin/quem_chama.py`.

1. **BLOQUEIA — valores completos montados são calculados e descartados.** `_prefixo` retorna `('pkg.mod', True)` para `'pkg.'+'mod'` (`:554–559`); o ramo nome só emite associação completa para `__import__` com fromlist ou relativo (`:703–718`). A recuperação lexical posterior exige um `Constant` inteiro (`:779–782`). Reproduzi `importlib.import_module('pkg.'+'mod').f()` contra `app/pkg/mod.py::f`: arquivo e símbolo com zero, RESUMO com dois zeros. Equivalente em caminho: `runpy.run_path('app/pkg/mo'+'d.py')`; `:684–685` só marca `literal=True`, não emite o caminho calculado, e `:775–778` não encontra literal completo/basename.

2. **BLOQUEIA — `getattr` direto foi coberto, seus apelidos não.** `:600–601` reconhece apenas `getattr(...)(...)` imediato; apelidos de loader só vêm de `ImportFrom` (`:607–614`). Reproduzi `im=getattr(importlib,'import_module'); im('pkg.mod').f()`: símbolo zero, OPACO vazio. Também `g=getattr; g(importlib,'import_module')('pkg.mod').f()` deixa símbolo zero. Formas `from importlib import import_module as im`, atributos `il.import_module`, e `getattr(il,'import_module')(literal)` funcionam pelos mesmos trechos.

3. **BLOQUEIA — exclusão de atributos builtin também exclui builtins verdadeiros.** `:602–603` devolve `None` para qualquer atributo `exec/compile`, protegendo `re.compile`, mas também escondendo `builtins.exec` e `builtins.compile`. Reproduzi `builtins.exec(open('app/pkg/mod.py').read(),G); G['f']()`: símbolo zero, OPACO vazio. Texto usa apenas `LOADERS_CAMINHO` (`:111/:487`), portanto não resolve `exec(open(...))` embutido; AST usa o conjunto expandido (`:106/:618`).

4. **BLOQUEIA — subprocesso só aciona o detector lexical antigo.** `:666–671` marca “caminho” pela simples presença de `sys.executable` ou string de interpretador; não extrai expressão de script nem registra opacidade. Reproduzi `x='d.py'; subprocess.run(['python3','app/pkg/mo'+x])`: ambas facetas zero, OPACO zero. `import sys as s; subprocess.run([s.executable,'app/pkg/mod.py'])` deixa símbolo zero, pois `_cadeia` precisa resultar exatamente `sys.executable` (`:666`). Literais completos com `python3` e `sys.executable` são protegidos por `:775–778`.

5. **BLOQUEIA — “ponto fixo” não é ponto fixo completo e exclui wrappers usuais.** Candidatos exigem token de loader builtin no próprio arquivo (`:1038–1040`), antes de descobrir wrappers; há teto de três rodadas (`:1041`). Reproduzi:
   - `one(path)->run_path(path)` em `one.py`, `two(path)->one(path)` em `two.py` sem token builtin, e chamada `two('app/pkg/mod.py')`: símbolo zero.
   - `from one import one as x; x('app/pkg/mod.py')`: símbolo zero; alias de wrapper não é resolvido.
   - cadeia com quatro wrappers, todos passando o filtro por comentário contendo `run_path`: quarto wrapper deixa símbolo zero.
   
   `extra` também guarda somente `fn.name→kind` (`:639`), e `novo_.update` sobrescreve colisões entre arquivos (`:1044`). Dois `load`, um de caminho e outro de nome, podem classificar o chamador pelo tipo errado e produzir símbolo zero.

6. **BLOQUEIA — prefixo de caminho relativo à raiz `app` pode não ligar nenhum alvo.** `join(APP_DIR,'pkg',leaf)` perde a raiz não literal e produz `'pkg/'` (`:569–578`). `sob_prefixo` compara esse prefixo diretamente a `a['rel']`, sem adicionar `app/` (`:509–513`). Com `APP_DIR='app'`, folha externa `mod.py` e alvo `app/pkg/mod.py`, a execução carrega o alvo, mas o prefixo não o encontra. `_loaders_py` considera resolvido e não registra OPACO (`:686–690`).

7. **BLOQUEIA/FILA — limites adicionais de atribuição/relativo.** `_valores` escreve apenas `Assign` com alvo `Name`, resolve dois saltos e não percorre expressões internas (`:651–663`). Reproduzi `a='pkg.'+input(); b=a; c=b; import_module(c)` e `x: str='pkg.'+input(); import_module(x)`: ambos deixam duas facetas zero e OPACO. O nome relativo montado `import_module('.'+leaf,package='pkg')` também vira OPACO sem vínculo: prefixo só de pontos é descartado (`:695–701`); package só é usado quando o nome já foi considerado inteiro (`:713–718`). O texto relativo só aceita grafia literal `import_module(...)`, com package literal (`:113/:499–501`).

8. **FILA — `_COM_NOME_NA_FRENTE` não normaliza os keywords dos loaders.** A exclusão só remove primeiro posicional; keywords excluem somente `name` (`:586–587`). Reproduzi `SourceFileLoader(fullname='unused',path=external)` → `opacos=[]`: `fullname` é interpretado como caminho literal (`:684–690`). Isso cala o aviso mesmo sem caminho conhecido. Analogamente `run_path(external,run_name='__main__')` trata o nome de execução como evidência de caminho. A emissão efetiva do falso zero para alvo carregado por `external` segue do consumidor lexical `:775–778`, que não recebe o valor externo.

9. **P4 — mapa de escritores e leitores.**

| Campo/constante | Escreve/define | Lê |
|---|---|---|
| `atrib` | `:651/:656` | `_valores :661` |
| `_valores` | `:658–663` | `:682/:695` |
| `Alvos.por_mod` | `:446/:448/:450` | `:500/:736/:780` |
| `Alvos.sob_prefixo` | `:504–513` | `:739` |
| retorno `involucros` | `:639–641` | `:1044` |
| `novo_` | `:1042/:1044` | `:1045/:1047` |
| `extra` | `:1037/:1047` | `:1044/:1054`, `_kind :622/:636/:675` |
| `opacos` local | `:664/:690/:701/:718` | retorno `:719`, recepção `:730`, acumulação `:732` |
| `opacos` global | `:1037/:732` | `:1054/:1096` |
| `a['opacos']` | `:1096` | só `alvos[0]`, `:1129` |
| cabeçalho/contagem de opacos | `:1130–1132` | RESUMO `:1144–1145` não usa isso para proteger zeros |
| `LOADERS_CAMINHO` | `:103–104` | `:106/:111` |
| `LOADERS_CAMINHO_AST` | `:106` | `:612/:618/:1039` |
| `_SO_BUILTIN` | `:107` | `:603` |
| `_COM_NOME_NA_FRENTE` | `:108` | `:586` |
| `_RX_LOADER_CAMINHO` | `:111` | `:487` |
| `_RX_PYTHON` | `:109` | `:669` |
| `_RX_REL_TXT` | `:113` | `:499` |

**Semânticas incompatíveis:** `_prefixo` agora devolve valor completo calculado, mas leitores de `inteiro` ainda presumem que algum literal inteiro será recuperado na varredura de `Constant` (`:684–685/:703–718/:774–782`). `linhas['caminho']` agora significa chamada de loader **ou** mera presença de string `python3`/atributo `sys.executable`; `ldr_txt` apresenta ambos como sítio de loader (`:666–670/:678/:733–734`). OPACO é anunciado como “sem literal no arquivo” (`:1130`), embora seja decidido por argumentos da chamada e exclua `exec/compile` (`:682–701`).

**P1(h):** prefixo vazio de nome faria `sob_prefixo` ligar todos os módulos (`:507–508`), mas os produtores filtram vazio/pontos (`:695–701`); caminho vazio produziria candidatos vazios (`:509–513`). Prefixo de um caractere é aceito e usa `startswith`, sem exigir `.` ou `/` (`:507–513`). Em particular prefixo nome `'a'` liga todo módulo de app por causa da alternativa `'app.'+modulo` (`:508`). Barra final preservada por `partes.append('')` impede `app/mp/` de casar `app/mpx/` (`:510–513`), mas sem barra final o casamento parcial é propositalmente amplo.

## 06/11 19:22 · sessao 2567ac09899c · - · - · origem=Ronald (tmux)

Dois pares novos confirmados para `Batida.timestamp`; ambos no consumidor A2, com escritor de suporte em `app/ponto/registro_batida.py:153` e `:158`.

| Campo | Divergência | Escritores A2 | Leitores no meu lote | Caso concreto |
|---|---|---:|---|---|
| `Batida.timestamp` | Instante aware é interpretado como data crua para selecionar a fatia, embora os limites sejam datas de competência/vínculo: `app/ponto/services/fechamento.py:204`, `:245`, `:252`. O próprio fechamento usa data localizada em outro ramo: `:784`. | 0; escritor de suporte: `app/ponto/registro_batida.py:153`, `:158`. | 7 funções: `fechamento.apurar_por_fatia:245`; `fechamento.recalcular_fechamento_mes:784`; `diagnostico_escala.diagnostico_escala:160`; `esmeril_espelho.assinaturas_do_colab:108`; `espelho.montar_dias:455`; `dias_em_aberto.passivo_da_competencia:122`; `piso_intervalo.pares_abaixo_do_piso:42`. | Fatia 01–03/10/2026, dias próprios `{01,03}`, dia 02 atribuído a outro vínculo. E/S UTC `02/10 00:15`, `02/10 01:15`, equivalentes locais a `01/10 21:15`, `22:15`. O filtro real remove ambas em `fechamento.py:252`; `motor.calcular_mes` recebe lista vazia em `:278`. Efeito monetário final [nao verificado]. |
| Mesmo campo, segundo par | Limite produzido como primeiro instante **posterior** ao período é consumido inclusivamente: `app/ponto/services/dias_em_aberto.py:121`, `:122` × `app/ponto/turnos.py:2868`. | Mesmo acima. | Mesmo conjunto acima. | Competência até 20/10/2026, dez dias em aberto e única batida `21/10 00:00-03:00`. Ela entra no contador (`dias_em_aberto.py:122`, `:129`) e classifica como `quase_nunca_bateu`, enquanto excluí-la resulta em `nunca_bateu` (`:57`, `:60`; limiar dez em `:30`). |

As expressões dos filtros e a função `classificar` foram executadas por extração AST em memória: resultados `2 → 0` batidas no primeiro caso e `quase_nunca_bateu → nunca_bateu` no segundo. Reprodução integrada e ocorrência em PROD [nao verificado].

Pares e famílias:

- `registro_batida.py:153` × `services/fechamento.py:252`: família do campo **batida**, família do sítio consumidor **folha/export**; a matriz nomeia o escritor de Batida em `app/core/contratos_estruturais.py:100` e o fechamento em `:158`.
- `registro_batida.py:158` × `services/dias_em_aberto.py:122`, passando pelo limite inclusivo de `turnos.py:2868`: mesmas famílias; competência e fechamento são encaminhados aos juízes indicados em `app/docs/JUIZES.md:78`.

Campos únicos examinados no meu lote, pertencentes ao inventário: **19** — `Batida.{timestamp,retratada_em}`, `EscalaColaborador.{data_inicio,data_fim}`, `CelulaDia.data`, `FolgaDia.data`, `Colaborador.data_demissao`, `Justificativa.data_turno`, `OrdemSubstituicao.{data_inicio,data_fim,push_enviado_em}`, `DiaPago.{data,lavrado_em}`, `FechamentoMensal.{previsto_em,derivado_em,atualizado_em,aprovado_em}`, `PeriodoFechado.{fechado_em,reaberto_em}`. Identidades sustentadas pelas respectivas referências do inventário fornecido; contagem reproduzível desta enumeração [nao verificado]. [nao verificado]

**1 campo divergente, dois pares novos**; nos demais 18 não confirmei novo par divergente neste lote. Isso não prova concordância universal de escritores/leitores fora do recorte [nao verificado].

Além do inventário, encontrei quatro campos reais usados nesses arquivos: `MetricaSnapshot.{data_ini,data_ref,calculado_em}` (`app/inteligencia/models.py:90`, `:91`, `:96`; escrita `he_pendente_lavrado.py:184`, `:185` e `esmeril_espelho.py:325`; leitura `he_pendente_lavrado.py:204`, `:206` e `esmeril_espelho.py:347`), e `LoteHolerite.publicado_em` (`app/holerite/models.py:30`; leitura `fechamento.py:1523`). Não confirmei divergência nesses quatro; escritores externos completos [nao verificado].

Retiro o candidato de duas `OrdemSubstituicao` ativas em janelas adjacentes: as constraints por substituto/posto impedem esse estado (`app/ponto/models.py:1124`, `:1130`). A disponibilidade usa sobreposição estrita (`app/ponto/utils.py:205`), e a atividade inclui a borda (`app/ponto/models.py:1147`), mas não fechei um par **escritor × leitor** com efeito temporal incorreto alcançável para esses campos [nao verificado].

## 06/11 19:22 · sessao 4346034ea03c · - · - · origem=Ronald (tmux)

Nenhum par temporal novo comprovadamente divergente neste recorte. O dia da jornada é derivado em `app/ponto/turnos.py:284` e `app/ponto/turnos.py:297`, copiado para `TurnoMaterializado` em `app/ponto/nucleo.py:50` e lido como jornada em `app/ponto/status_dia.py:273`. As bordas dos períodos de dias são inclusivas em `app/ponto/turnos.py:1993`, `app/ponto/turnos.py:2163` e `app/ponto/precedencia.py:335`.

**Contagem local:** 13 campos com escritores abertos; 96 slots de parâmetros com corpos examinados, dos quais 50 chamados `data`, `data_*` ou `dia` e 46 chamados `ini`/`fim`. Esta contagem foi obtida por AST somente leitura dos 15 módulos atribuídos; não significa fornecedores/chamadores exaustivamente rastreados. [não verificado quanto à totalidade dos fornecedores]

**Dois campos ficam “não sei”:**

| Campo | Escritores A3 abertos | Leitores | Motivo |
|---|---|---|---|
| `EventoCelula.data` | 1: copia `cel.data`, `app/ponto/portas/celula.py:891` | Nenhum leitor semântico encontrado; o consumidor trabalha por colaborador, `app/chamados/reconciliador.py:1340` | Sem leitor para confrontar; total global de leitores [não verificado]. |
| `FuroDiario.avaliada_em` | 1: `timezone.now()`, `app/ponto/portas/celula.py:1127` | Nenhum leitor efetivo encontrado; ranking filtra a **data** em `app/escala/services/furos_diarios.py:133` | A documentação diz “apurado em”, `app/ponto/portas/celula.py:31`, mas consumidor desse carimbo [não verificado]. |

**Outros 11 campos, sem divergência nas amostras lidas:**

| Campo | Escritores A3: n e linhas | Amostra de leitores aberta |
|---|---|---|
| `CelulaDia.data` | 3: `app/ponto/portas/celula.py:259`, `:600`, `:850` | Chave de dia: `app/ponto/turnos.py:1584`; período fechado: `app/ponto/portas/celula.py:219`. |
| `CelulaDia.regenerada_em` | 2: `app/ponto/portas/celula.py:274`, `:792` | Instante de reescrita, comparação estrita: `app/escala/services/regeneracao.py:84`, `:120`. |
| `CelulaDia.editada_em` | 2: `app/ponto/portas/celula.py:601`, `:607` | Mesmo confronto: `app/escala/services/regeneracao.py:84`, `:120`. |
| `CelulaDia.julgada_em` | 1: `app/ponto/portas/celula.py:1060` | Marco de invalidação: `app/chamados/veredito_lavrado.py:154`; existência de julgamento: `app/ponto/management/commands/vigia_b5_ausencia.py:55`. |
| `CelulaDia.veredito_em` | 1: `app/ponto/portas/celula.py:1032` | Marco de invalidação: `app/chamados/veredito_lavrado.py:153`. |
| `CelulaDia.insumos_em` | 2: `app/ponto/portas/celula.py:1064`, `:1100` | Distingue insumos examinados de ausentes: `app/ponto/management/commands/vigia_b5_ausencia.py:71`. |
| `FuroDiario.data` | 1: `app/ponto/portas/celula.py:1125` | Dia da grade produzido em `app/escala/services/furos_diarios.py:51` e filtrado inclusivamente em `:133`. |
| `DecisaoHE.data` | 1: `app/ponto/portas/he.py:67` | Chave `date` no cálculo: `app/ponto/motor_calculo_v2.py:1417`. |
| `DecisaoHE.revogada_em` | 2: limpa em `app/ponto/portas/he.py:83`; marca em `:373` | Vigência por nulidade: `app/ponto/models.py:1257`. |
| `TurnoMaterializado.data_turno` | 1: `app/ponto/nucleo.py:50` | Dia da jornada na cobertura: `app/ponto/management/commands/processar_alertas_turno.py:97`. |
| `TurnoMaterializado.dt_fim_previsto` | 1: `app/ponto/nucleo.py:55` | Instante previsto: `app/ponto/selecao_periodo.py:290`; `app/ponto/management/commands/processar_alertas_turno.py:100`. |

Números de escritores acima contam sítios sintáticos A3; leitores globais não foram contados. [não verificado]

**Candidato que não deve virar divergência afirmada:** `date.today()` em `app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:816` e `app/ponto/turnos.py:2728` convive com `timezone.localdate()` em `app/ponto/signals.py:122` e `app/ponto/status_dia.py:254`. A configuração declara São Paulo e `USE_TZ=True` em `app/config/settings/base.py:93`; fuso efetivo do processo/SO e eventual ativação de outro fuso [não verificado]. Portanto, diferença de resultados desse candidato [não verificado].

**Parâmetros: atenção ao significado não temporal.** Sete `dia` são dicionários da grade, não datas: `app/ponto/propositor.py:34`, `app/ponto/turnos.py:2644`, `app/ponto/status_dia.py:166`, `app/ponto/supra_juiz.py:60`, `:65`, `:109`, `:204`. Não são divergências entre escritores e leitores: seus corpos usam `.get()`/indexação do dicionário nos respectivos sítios.

**Lista dos 50 slots `data`/`data_*`/`dia`, para consolidar sem duplicar:**

- `turnos.py`: `realizado_dos_turnos:705(data)`, `realizado_do_dia:772(data)`, `dia_encerrado:2031(data)`, `dia_encerrado_motivo:2090(data)`, `_dia_encerrado_puro:2100(data)`, `_dia_encerrado_motivo_puro:2105(data)`, `ausencia_cobre:2536(data_ref)`, `horas_reais_do_dia:2644(dia)`, `afastamentos_no_dia:2705(dia)`, `afastado_hoje:2723(dia)`, `dias_da_ausencia:2774(data,data_fim)`, `batidas_do_dia_turno:3013(dia)`, `turno_aberto_ha:3115(dia)`, `CargaDaJanela.celula:2970(data)`.
- `nucleo.py`: `dt_fim_previsto_de:31(data_turno)`.
- `selecao_periodo.py`: `janela_turno_de:43(data_ref)`.
- `marcos.py`: `marco_vencido:70(data_turno)`, `celula_veta_cobranca:122(dia)`, `_lei_cala_no_dia:166(dia)`.
- `propositor.py`: `dia_degenerado:34(dia)`, `confianca_do_dia:228(data)`, `dia_posicional_completo:325(dia)`.
- `precedencia.py`: `folgas_lancadas_do_periodo:194(data_ini,data_fim)`, `fatos_do_dia:242(data)`, `fatos_do_periodo:259(data_ini,data_fim)`, `veto_de_lancamento:430(data,data_fim)`.
- `status_dia.py`: `fatos_do_status:166(dia,data)`, `status_do_dia_lavrado:201(data)`.
- `supra_juiz.py`: `com_insumo_dia_sem_trabalho:60(dia)`, `_dia_sem_trabalho_pelo_juiz:65(dia)`, `motivo_retratacao:109(dia)`, `cadastro_do_dia:142(data_demissao)`, `_f:160(data)`, `cadastro_do_dia_carregado:166(data)`, `_insumos_do_vinculo:181(data,data_demissao)`, `classificar_dia:204(dia,data)`.
- `portas/celula.py`: `editar_dia:534(dia)`, `regenerar_celulas_dia:703(dia)`, `reverter_regeneracao_dia:740(dia)`.
- `portas/he.py`: `_gravar:44(data)`, `decidir_he:138(data)`, `recusar_ponta_pequena:269(data)`, `revogar_recusa_do_sistema:393(data)`.

Todos os caminhos acima são relativos a `app/ponto/`. Os 46 slots `ini`/`fim` adicionais são: turnos 36, núcleo 6, turno_leitura 2 e portas/celula 2; confira a soma: os 15 slots estritos de turnos mais seus 36 = 51. [contagem AST; arquivos e linhas da lista acima e definições do inventário fornecido]

Não examinei `PerguntaDisputa.data_evento` nesta divisão; nenhum par novo dele foi demonstrado aqui. [não verificado]

## 06/11 19:21 · sessao 1156cd4e59b9 · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — o selo certifica `compile` por acaso de fixture.** A fixture usa `exec(compile(...))`; retirar somente `compile` de `_kind` mantém `exec` como loader e preserva todas as condições verificadas para `ld_cp`. Em memória, só muda o texto interno `loader de caminho :2,2` para `:2`, que nenhuma asserção confere. Contraexemplo que o mutante perde: `co = compile(open(os.path.join(R, 'app', 'mx', N)).read(), 'co', 'exec'); eval(co)`. A forma exigiria reconhecer `compile`, sem outro loader AST para mascarar sua ausência. (`bin/tests/test_quem_chama.sh:295`, `bin/tests/test_quem_chama.sh:595`, `bin/tests/test_quem_chama.sh:596`, `bin/quem_chama.py:618`, `bin/quem_chama.py:775`)

2. **BLOQUEIA — duas rodadas de resolução e ponto fixo não estão certificados.** Os mutantes `_valores(..., salto=1)` e `range(1)` produziram os mesmos achados LOADER e sítios opacos nas fixtures r9/r10 extraídas e executadas em memória. `ld_cte` contém o basename completo `'ld_cte.py'`, portanto o reconhecimento lexical já o associa sem acompanhar `CAM → DOIS`; a fixture de invólucro externo tem apenas um nível. Contraexemplos: `a='pkg.'+x; b=a; import_module(b)` exige os dois saltos; `f2(p) → f1(p) → import_module(p)` exige a segunda rodada. (`bin/quem_chama.py:658`, `bin/quem_chama.py:1041`, `bin/quem_chama.py:775`, `bin/tests/test_quem_chama.sh:196`, `bin/tests/test_quem_chama.sh:197`, `bin/tests/test_quem_chama.sh:308`, `bin/tests/test_quem_chama.sh:318`, `bin/tests/test_quem_chama.sh:583`)

3. **FILA — outros mutantes sobrevivem às fixtures r9/r10 examinadas.** Aceitar atributo `x.exec(...)` enquanto se mantém a exclusão de `re.compile` deixa os achados idênticos: o comentário promete `x.exec`, mas só existe fixture `re.compile`. Retirar `(?:app\.)?` das regex textuais também deixa os achados idênticos: `ld_app` é chamada AST, coberta por `por_mod`, sem exigir a variante textual. Contraexemplos adicionais ausentes das fixtures são `.format`, concatenação `+`, `Path /`, prefixo vazio, prefixo de um caractere e prefixo parcial sem separador. (`bin/quem_chama.py:603`, `bin/quem_chama.py:451`, `bin/quem_chama.py:455`, `bin/quem_chama.py:450`, `bin/quem_chama.py:554`, `bin/quem_chama.py:560`, `bin/quem_chama.py:566`, `bin/tests/test_quem_chama.sh:286`, `bin/tests/test_quem_chama.sh:355`, `bin/tests/test_quem_chama.sh:356`, `bin/tests/test_quem_chama.sh:327`)

4. **FILA — o teste OPACO verifica presença individual, sem verificar completude.** `r10-opaco` exige apenas `bin/carrega_lib.py:5` no cabeçalho; omitir todos os demais opacos preservaria essa asserção. Não confere quantidade, ausência de sítios indevidos nem proteção das facetas dos alvos carregados somente por opacos. Contraexemplo: manter esse sítio fixo e descartar os demais em `opacos`; a condição continua verdadeira. (`bin/tests/test_quem_chama.sh:620`, `bin/tests/test_quem_chama.sh:621`, `bin/quem_chama.py:731`, `bin/quem_chama.py:1129`)

5. **FILA — `ld_cfg` e `ld_shn` conservam asserções genéricas mascaradas, mas seus grupos têm mordida.** As condições “não varrido” e “sem zero” podem continuar verdadeiras por EMBUTIDO quando o loader textual desaparece; entretanto as asserções específicas `carrega por ponto de entrada` e `carrega por nome` exigem o sítio LOADER. Portanto não é correto afirmar que os grupos inteiros passam por acaso. (`bin/tests/test_quem_chama.sh:602`, `bin/tests/test_quem_chama.sh:603`, `bin/tests/test_quem_chama.sh:604`, `bin/tests/test_quem_chama.sh:606`, `bin/tests/test_quem_chama.sh:608`, `bin/quem_chama.py:1081`, `bin/quem_chama.py:1092`)

6. **FILA — os quatro contraexemplos nomeados não são coletivamente vácuos.** Retirar `partes.append('')` associa indevidamente `app/mpx/ld_mpx.py` ao prefixo `app/mp/` e faz `r10-prefixo-e-diretorio` falhar; aceitar `re.compile` faz `r10-re-compile-nao-carrega` falhar; usar `LOADERS_CAMINHO_AST` na regex textual rebaixa o símbolo citado por `exec cat` e faz `r10-exec-shell-nao-carrega` falhar. Esses efeitos foram reproduzidos em memória. `r10-fora-do-prefixo` passaria isoladamente se todo reconhecimento de prefixos fosse removido, porém as positivas `r10-montado-prefixo` e `r10-montado-caminho` exigem que esse reconhecimento funcione. (`bin/quem_chama.py:511`, `bin/quem_chama.py:603`, `bin/quem_chama.py:112`, `bin/tests/test_quem_chama.sh:611`, `bin/tests/test_quem_chama.sh:612`, `bin/tests/test_quem_chama.sh:613`, `bin/tests/test_quem_chama.sh:616`, `bin/tests/test_quem_chama.sh:618`, `bin/tests/test_quem_chama.sh:619`)

7. **FILA — RED histórico deixou de pular silenciosamente.** A ausência de `c6c3785b` agora incrementa falhas; sua presença executa as três verificações específicas de `placar`. A execução integral do selo histórico e a sobrevivência dos mutantes ao selo inteiro são **[não verificado]**: esta cópia não possui `.git`, e o selo cria diretório temporário, arquivos e commits, contrariando a restrição desta auditoria. As reproduções acima executaram somente código e fixtures em memória, com `python3 -B`. (`bin/tests/test_quem_chama.sh:39`, `bin/tests/test_quem_chama.sh:43`, `bin/tests/test_quem_chama.sh:44`, `bin/tests/test_quem_chama.sh:674`, `bin/tests/test_quem_chama.sh:678`, `bin/tests/test_quem_chama.sh:679`, `bin/tests/test_quem_chama.sh:680`)

## 06/11 19:21 · sessao 7d6410b54bc1 · - · - · origem=Ronald (tmux)

Dois campos divergentes comprovados, mesma falha de fuso: `PerguntaDisputa.julgado_em` e `janela_expira_em`. Escritor usa instante aware e dia civil; leitor compara o dia UTC cru com a data civil. (`app/chamados/veredito_lavrado.py:228`, `app/chamados/veredito_lavrado.py:229`, `app/chamados/veredito_lavrado.py:182`, `app/chamados/veredito_lavrado.py:185`)

| Campo | Escritores abertos | Leitores do inventário abertos | Divergência |
|---|---|---|---|
| `julgado_em` | **3 atribuições**: lavrar `app/chamados/veredito_lavrado.py:228`; restauração em memória `app/chamados/veredito_lavrado.py:244`; backfill `app/chamados/management/commands/backfill_veredito_lavrado.py:139` | **5 ocorrências**: `app/chamados/veredito_lavrado.py:179`, `app/chamados/veredito_lavrado.py:185`, `app/chamados/veredito_lavrado.py:189`, `app/chamados/veredito_lavrado.py:237`, `app/chamados/services/raiox.py:31` | **Sim**: `now()` grava instante; `.date()` compara sua data UTC com dia civil. (`app/chamados/veredito_lavrado.py:228`, `app/chamados/veredito_lavrado.py:185`) |
| `janela_expira_em` | **3 atribuições**: lavrar `app/chamados/veredito_lavrado.py:229`; restauração `app/chamados/veredito_lavrado.py:245`; backfill `app/chamados/management/commands/backfill_veredito_lavrado.py:140` | **7 ocorrências**: duas em `app/chamados/veredito_lavrado.py:182`; `app/chamados/veredito_lavrado.py:185`; `app/chamados/veredito_lavrado.py:237`; `app/chamados/services/raiox.py:32`; backfill `app/chamados/management/commands/backfill_veredito_lavrado.py:121`, `app/chamados/management/commands/backfill_veredito_lavrado.py:135` | **Sim**: data civil da virada da competência comparada a `agora.date()` UTC. (`app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:118`, `app/chamados/veredito_lavrado.py:182`) |

O backfill confirma explicitamente o significado civil: calcula `hoje=timezone.localdate()` e classifica `expira_em(...) <= hoje`. (`app/chamados/management/commands/backfill_veredito_lavrado.py:94`, `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, `app/chamados/management/commands/backfill_veredito_lavrado.py:113`)

Par divergente da família **chamado**: `lavrar` escreve `julgado_em=timezone.now()` e `janela_expira_em=expira_em(data_evento)`; `esta_stale` e `raiox.pks_stale_por_varredura` leem os dias UTC crus. (`app/core/contratos_estruturais.py:51`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/veredito_lavrado.py:229`, `app/chamados/veredito_lavrado.py:182`, `app/chamados/veredito_lavrado.py:185`, `app/chamados/services/raiox.py:36`, `app/chamados/services/raiox.py:41`)

Caso concreto, executado extraindo a AST da função real `esta_stale`, sem Django/banco:

- `janela_expira_em=2026-11-21`, `julgado_em=2026-11-20T20:00Z`, `agora=2026-11-21T00:30Z` (=20/11 21:30 local) → **True**, 2h30 antes da virada civil. (`app/chamados/veredito_lavrado.py:182`, `app/chamados/veredito_lavrado.py:185`; fuso configurado `app/config/settings/base.py:93`)
- Após relavra em `2026-11-21T01:00Z` (=20/11 22:00 local), mantendo a mesma expiração, `agora=2026-11-21T03:30Z` (=21/11 00:30 local) → **False**: `julgado_em.date()==janela_expira_em`, embora a lavra tenha ocorrido na véspera local. (`app/chamados/veredito_lavrado.py:185`)

`data_evento=2026-09-21` gera essa expiração quando o dia civil é 20/11: corrente 21/10–20/11, anterior 21/09–20/10; na próxima janela o fato já fica fora. (`app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:116`, `app/chamados/veredito_lavrado.py:118`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:66`)

Contagens são ocorrências sintáticas, excluindo `update_fields`, migrations/tests e usos indiretos como a tupla de reversão; incidência em produção **[nao verificado]**. (`app/chamados/services/passivo_validado.py:65`)

Parâmetros examinados: **20**, nenhum com divergência própria comprovada; não confundir `expira_em.data_evento` com o defeito do relógio. Referências das declarações:

- **10 em juizes**: `_ajustar_saida_overnight.data_evento` :285; `_celula_furada.data_iso` :431; `_dentro_da_janela_viva.data_evento` :556; `_admin_tocou_o_dia_depois.data` :1030; `_toque_humano_no_dia.data` :1053; `via_pelo_fato.data_evento` :1434; `folha_julga_pontualidade.data` :1616; `dia_em_competencia_trancada.dia` :1759; `_turno_cruza_meia_noite.dia` :1817; `cobranca_viva_do_dia.dia` :1886. (`app/chamados/juizes.py:285`, `app/chamados/juizes.py:431`, `app/chamados/juizes.py:556`, `app/chamados/juizes.py:1030`, `app/chamados/juizes.py:1053`, `app/chamados/juizes.py:1434`, `app/chamados/juizes.py:1616`, `app/chamados/juizes.py:1759`, `app/chamados/juizes.py:1817`, `app/chamados/juizes.py:1886`)
- **8 em reconciliador**: `turnos_abertos_de.dia`, `_tem_turno_aberto.dia`, `lavrar_sem_turno_aberto.dia`, `_ctx_c2.dia`, `_ctx_c2.data_ctx`, `_dia_fora_do_alcance_c.dia`, `_viva_em_competencia_aberta.dia`, `reemitir_chamado_do_dia.dia`. (`app/chamados/reconciliador.py:82`, `app/chamados/reconciliador.py:161`, `app/chamados/reconciliador.py:340`, `app/chamados/reconciliador.py:665`, `app/chamados/reconciliador.py:706`, `app/chamados/reconciliador.py:742`, `app/chamados/reconciliador.py:751`)
- **2 restantes**: `expira_em.data_evento`, `relavrar_dia.dia`. (`app/chamados/veredito_lavrado.py:95`, `app/chamados/services/relavra_via.py:140`)

Todos os corpos desses parâmetros foram lidos; exaustão dos chamadores upstream **[nao verificado]**. Outros campos tocados sem censo completo de escritores foram enviados ao pai como cobertura insuficiente. **[nao verificado]**

## 06/11 19:21 · sessao 506ddc419ad0 · - · - · origem=Ronald (tmux)

Pares divergentes encontrados na leitura, todos sem medição de incidência em produção [não verificado]:

1. **`DecisaoHE.data`: dia autorizado × data civil da entrada.** Escritor único explícito: `app/ponto/portas/he.py:67`; a view converte o dia informado em `date` e o encaminha à porta (`app/ponto/views.py:524`, `app/ponto/views.py:529`). O motor carrega decisões por esse dia (`app/ponto/motor_calculo_v2.py:1417`), mas `_janela_do_dia` calcula `dia = entrada.date()` (`app/ponto/motor_calculo_v2.py:1528`) e procura a autorização por essa chave civil (`app/ponto/motor_calculo_v2.py:1653`). Outro consumidor procura a autorização pelo dia da jornada (`app/ponto/motor_calculo_v2.py:1456`, chamado em `app/ponto/motor_calculo_v2.py:2461`).
   - **Caso:** jornada de 20/09, entrada local em 21/09 02:30, saída 07:00, marcos 19:00–07:00, autorização em 20/09, janela ativa anteriormente. A autorização de 20/09 não casa com a chave 21/09; `entrada_efetiva` desloca a entrada para **21/09 19:00**, além da saída, por `app/ponto/janela_he.py:94` e `app/ponto/janela_he.py:102`. A guarda L084 exige ambas as pontas distantes; saída exatamente no marco não a aciona (`app/ponto/motor_calculo_v2.py:1947`–`:1949`). O juiz admite entrada pós-meia-noite pertencendo à véspera (`app/ponto/turnos.py:297`–`:306`).
   - Família: **turno/marcos**, efeito em cálculo/folha (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:29`).
   - Contagem comprovada: **1 sítio explícito de escrita da data**, **3 sítios de leitura relevantes lidos**: carga `:1417`, consulta por jornada `:1456`, consulta civil `:1653`. Total global de leitores [não verificado].

2. **`Empresa.janela_he_desde` e parâmetro `_empresa_da_janela_do_dia.dia`: vigência da competência × civil da entrada.** O cadastro declara que se deve informar o primeiro dia da competência (`app/colaboradores/models.py:117`–`:121`). O comando calcula essa data pela janela e a passa ao cadastro (`app/ponto/management/commands/aplicar_janela_he_total.py:95`–`:102`). A porta grava dinamicamente (`app/colaboradores/services/estrutura.py:17`–`:20`). O mesmo leitor compara `dia < desde` (`app/ponto/motor_calculo_v2.py:1393`) recebendo **jornada** em `:1451` e **civil** em `:1529`, derivado em `:1528`.
   - **Caso:** vigência 21/09; turno da jornada 20/09 com entrada 21/09 00:05. Pelo dia da jornada a janela ainda não vale; pelo ramo civil ela já vale (`app/ponto/motor_calculo_v2.py:1393`, `app/ponto/motor_calculo_v2.py:1528`). Resultado: aplicação da janela a turno da competência anterior; consequência monetária exata depende dos marcos/limites [não verificado].
   - Família: **folha/export** para vigência de janela, com geometria **turno/marcos** (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:29`, `app/docs/JUIZES.md:35`).
   - Contagem do parâmetro: **2 chamadores**, `:1451`/`:1529`; **1 leitura comparativa**, `:1393`. Escritores globais do campo não exauridos [não verificado].

3. **Parâmetro `MotorBase._feriado_dobra_no_dia.dia`: jornada × civil do segmento.** Sua definição exige dia de início do plantão (`app/ponto/motor_calculo_v2.py:928`–`:934`). Os chamadores por período passam `e.date()` (`app/ponto/motor_calculo_v2.py:2809`, `app/ponto/motor_calculo_v2.py:3255`); os chamadores por jornada passam `dia` (`app/ponto/motor_calculo_v2.py:3092`, `app/ponto/motor_calculo_v2.py:3337`). O calculador também usa jornada (`app/ponto/calculador/regras.py:338`–`:351`).
   - **Caso:** jornada 11/10, cauda 12/10 03:00–07:00, feriado 12/10, `feriado_em_dobra=True`, janela de HE desligada. O ramo por período aplica **240 min em HE100 feriado** por 12/10 (`app/ponto/motor_calculo_v2.py:2809`–`:2812`); a regra por jornada consulta 11/10 e devolve **0** (`app/ponto/calculador/regras.py:351`). Incidência concreta no acervo [não verificado].
   - Família: **turno/marcos**, efeito **folha/export** (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:29`).
   - Contagem: **5 chamadores produtivos lidos**, discriminados acima; leitor da decisão em `app/ponto/motor_calculo_v2.py:934`.

4. **Dia de jornada publicado × DSR/banco de horas que recompõem dia civil.** A base obtém o dia pelo juiz e permite herança da cauda (`app/ponto/motor_calculo_v2.py:2403`–`:2408`), grava o mapa (`:2460`) e o publica (`:1073`–`:1075`). O DSR usa esse mapa para presença (`:2577`–`:2579`), mas usa civil da entrada para HE e denominador do reflexo (`:2639`, `:2643`–`:2644`). O comercial agrupa pelo juiz (`:3313`–`:3317`), mas seu banco desconta HE por civil (`:3423`) e o DSR refeito usa civil (`:3441`, `:3454`, `:3466`, `:3470`–`:3471`).
   - **Caso:** competência terminando domingo 20/09, período cuja entrada civil é segunda 21/09, mapeado pelo juiz para 20/09, com 60 min de HE. O período entra na competência pelo dia da jornada (`:2429`–`:2432`); a semana 14–20 reconhece o trabalho, mas exclui os 60 min do numerador do reflexo por consultar 21/09 (`:2639`). No comercial, a releitura do DSR pode declarar falta em 20/09 apesar do mapa daquele período (`:3441`, `:3454`). Valor total depende dos demais dias [não verificado].
   - Este é um **par novo sobre o dia derivado**, não uma nova atribuição ao campo persistido `TurnoMaterializado.data_turno`; a ligação a esse campo específico [não verificado].
   - Família: **turno/marcos**, efeito **folha/export** (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:29`).

Censo de parâmetros dos arquivos atribuídos: **41 posições temporais**, incluindo os nomes adicionais do inventário `data`, `dia_corte`, `dia_base`, `dia_por_periodo`; não inclui `ini/fim/inicio` genéricos nem booleano `dia_de_trabalho`. A enumeração foi conferida pela AST da cópia; referência persistida desse resultado [não verificado]:

- `calculador/alimentacao.py`: `marcos_por_dia(data_ini,data_fim)` `:163`; `_no_dia_da_jornada(dia)` `:212`; `insumos_do_calculador(data_ini,data_fim)` `:305`; `rubricas_por_dia(data_ini,data_fim)` `:353` — **7**.
- `calculador/periodo.py`: `segmentos_do_par(dia)` `:291`; `periodo_do_dia(dia)` `:340`; `pontualidade_no_dia(dia)` `:434`; `regua_do_dia(dia)` `:516` — **4**.
- `calculador/regras.py`: `pontualidade_do_dia(dia)` `:170`; `dobra_de_feriado_do_dia(dia)` `:313`; `folga_trabalhada_do_dia(dia)` `:367`; `do_dia(dia)` `:578` — **4**.
- `motor_calculo_v2.py`: `is_feriado(data)` `:30`; `_turnos_do_juiz(data_ini,data_fim)` `:867`; `_dia_do_turno(data_ini,data_fim)` `:884`; `_feriado_dobra_no_dia(dia)` `:914`; `_folga_pelo_dia_previsto(dia)` `:936`; `eh_feriado(data)` `:956`; `_aplicar_teto_pontualidade(dia_por_periodo)` `:1024`; `_extra_declarada(dia)` `:1337`; `_empresa_da_janela_do_dia(dia)` `:1372`; `_dia_sem_previsto_fora_da_janela(dia)` `:1422`; `_acusa_cadastro_x_realidade(dia)` `:1655`; os três `calcular_mes(data_ini,data_fim)` `:2265`, `:2899`, `:3295`; `_prev(dia_base)` `:3040` — **20**.
- `janela_he.py`: `pontas_do_relato(dia)` `:234` — **1**.
- `janelas.py`: `periodo_apuracao(dia_corte)` `:10`; `periodo_apuracao_atual(data,dia_corte)` `:24`; `janela_atual(data)` `:60`; `janela_anterior(data)` `:66` — **5**.

Para os **demais parâmetros**, não identifiquei divergência na amostra lida; o levantamento de todos os chamadores externos não foi exaurido, logo não é legítimo converter isso em “todos concordam” [não verificado]. As janelas geram extremos `date` inclusivos (`app/ponto/janelas.py:16`–`:21`); a base filtra jornadas por `< ini` e `> fim` (`app/ponto/motor_calculo_v2.py:2429`–`:2432`), concordando nas bordas inclusivas.

## 06/11 19:21 · sessao 7baf1fdb2610 · - · - · origem=Ronald (tmux)

Achados confirmados e limites para consolidação:

| Campo/parâmetro | Significados e par | Escritores/leitores lidos | Resultado |
|---|---|---|---|
| `DisputaSupervisao.aberta_em` | Instante `DateTimeField(auto_now_add=True)` em `app/chamados/models.py:1686`; leitor extrai **data crua** do instante e subtrai do **dia local** em `app/ponto/management/commands/alertar_disputas_pendentes.py:31`, `:52`, `:73`. Configuração São Paulo e `USE_TZ=True`: `app/config/settings/base.py:93`, `:95`. | 1 mecanismo escritor automático, acionado nos 3 nascimentos lidos: `app/chamados/services/disputa_emissao.py:523`, `:962`, `:1182`. A3: 2 leituras do campo, ambas lidas (`alertar_disputas_pendentes.py:52`, `:73`). Ausência de outros mecanismos escritores não provada por atribuição de entidade [não verificado]. | **SIM.** Entrada armazenada `2026-11-06T02:30Z` = `05/11 23:30` local; execução `06/11 08:00` local produz `dias_aberta=0`/“hoje”, quando a abertura local ocorreu “ontem”. Derivação de `app/ponto/management/commands/alertar_disputas_pendentes.py:52`. Família **chamado**: `app/core/contratos_estruturais.py:51`. |
| `tripwire_celulas.medir_pausa_faltando.ini/fim` | Docstring promete competência pelo corte da empresa (`app/ponto/management/commands/tripwire_celulas.py:78`); escritor dos defaults chama `janela_atual(date.today())` **sem empresa** (`:85`, `:86`), produzindo corte 21 (`app/ponto/janelas.py:50`, `:60`). Leitor filtra células de várias empresas pela mesma janela (`tripwire_celulas.py:89`, `:97`). | 1 atribuição que produz ambos os defaults (`:85-86`); 1 filtro leitor que consome ambos (`:97`); chamador real sem argumentos lido (`:378`). | **SIM, 2 parâmetros.** Empresa com corte 5, referência `06/11/2026`: competência empresarial `05/11..04/12`; consulta `21/10..20/11`, incluindo `21/10..04/11` e omitindo `21/11..04/12`. Derivação de `app/ponto/janelas.py:24`, `:50`, `:63`. Família **celula/precedencia**, recorte de competência **folha/export**: `app/core/contratos_estruturais.py:47`, `:52`; autoridade da janela em `app/docs/JUIZES.md:35`, `:75`. |
| `tripwire_celulas.medir_sobra_com_tipo_trocado.ini/fim` | Mesma promessa no comentário (`app/ponto/management/commands/tripwire_celulas.py:232`), mesmos defaults sem empresa (`:247`, `:248`), filtro de células sem discriminar empresa (`:257`). | 1 atribuição conjunta (`:247-248`); 1 filtro leitor (`:257`); chamador sem argumentos (`:389`). | **SIM, 2 parâmetros.** Mesmo caso corte 5 acima, com a mesma janela errada. Família **celula/precedencia**, recorte **folha/export**, pelas referências da linha anterior. |
| `gerar_celulas.montar_lampadas.data` | Escritor gera `hf` no próprio dia quando `hi==hf`, pois só avança horas **menores** que `hi` (`app/ponto/management/commands/gerar_celulas.py:55`, `:67`, `:69`). Autoridade encadeia marco `<=` ao anterior em D+1 (`app/escala/servico_jornada.py:82`), explicitamente tratando igualdade como turno de 24h (`:100`). | 1 produtor lido (`gerar_celulas.py:36`); leitor do instante lido (`app/ponto/marcos.py:80`, `:88`), chamado por `app/ponto/management/commands/reconciliar_grade.py:169`. Porém esse caminho veta igualdade antes: `reconciliar_grade.py:159` → `app/ponto/marcos.py:108`. | **NÃO SEI quanto ao par que quebra.** `06/11`, `hi=hf=15:00` gera `hf=06/11T15:00`, enquanto `marcos_turno` produz `07/11T15:00`; o consumidor concreto aberto tem guarda. Outro leitor que consuma esse valor sem a guarda [não verificado]. Família **turno/marcos**: `app/docs/JUIZES.md:24`. |
| `PerguntaDisputa.data_evento` — par novo A3 | Escritor de conversa grava dia local **da emissão**, sem identidade de fato (`app/chamados/models.py:2403`, `:2422`, `:2427`). `mapa_divergencia` cruza todas as perguntas da janela com dias de furo, denominando isso “furo coberto por pergunta” (`app/ponto/management/commands/mapa_divergencia.py:264`, `:270`, `:275`), e remove os dias do restante (`:276`). | Escritores abertos: nascimento `app/chamados/models.py:2476`, fallback conversa `:2427`; reapontamento que só preenche ausente `app/chamados/services/fio.py:832`, `:834`, `:847`. A3: 1 filtro leitor (`mapa_divergencia.py:272`) e 1 leitura de valor (`:273`), ambos lidos. Censo total dos escritores não fechado [não verificado]. | **NÃO SEI.** Caso concreto de código: conversa `ancora_ausente` emitida em `06/11`, furo em `06/11`, sem pergunta factual correspondente → `furo_com_pd` aumenta 1 e o dia sai de `_resto` (`:275-276`). O código realmente faz essa conta; decidir se a métrica pretende cobertura factual ou simples presença de qualquer pergunta não está explicitado além do comentário (`:264`). Família **chamado**: `app/core/contratos_estruturais.py:51`. |

Contagem defensável desta exploração: **2 campos de modelo com exame específico** (`DisputaSupervisao.aberta_em`, `PerguntaDisputa.data_evento`), sendo **1 divergente e 1 não sei**, pelas referências acima. Outros campos foram abertos como insumos, sem fechar seus escritores [não verificado].

**44 parâmetros tiveram corpos abertos**; **4 divergentes** são os quatro `ini/fim` dos dois tripwires (`app/ponto/management/commands/tripwire_celulas.py:70`, `:199`). `montar_lampadas.data` é **não sei** pelo consumidor protegido descrito acima (`gerar_celulas.py:36`; `reconciliar_grade.py:159`). Para os outros, não encontrei contradição no corpo/amostra, mas **não fechei todos os chamadores/escritores**, portanto não sustento “todos concordam” [não verificado].

A lista exata dos 44 foi enviada na mensagem anterior, com arquivo/função/linha. Três nomes não representam uma data individual: `e6_oraculo.diferenca_e_a_janela_declarada.dia` e `minutos_do_espelho.dia` são dicionários (`app/ponto/management/commands/e6_oraculo.py:224`, `:251`); `diff_espelho_alimentacao._motor.dia_inteiro` alimenta conjunto de dias de ausência inteira (`app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `:57`).

## 06/11 19:21 · sessao e10f909645af · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **`silenciado_ate`: há duas semânticas explícitas.** O prazo vencido com premissa viva “volta à fila” em `app/chamados/models.py:83`; a porta justifica renovar para evitar esse reaparecimento em `app/chamados/services/arquivo.py:150`. Já o leitor transforma vencido em **HISTORICO**, continuando fora da operação, em `app/chamados/services/verbos.py:48`; a janela do colaborador exige `NULL`, independentemente do prazo, em `app/chamados/services/janela_colab.py:73`. A documentação do emissor dos isentos também assume “vencido vira histórico” em `app/chamados/services/silencio_isentos.py:5`. Família **chamado**. Exemplo: chamado vivo, `silenciado_ate=2026-11-06T12:00Z`, agora `13:00Z`: prazo vencido e leitor do modelo permite fila; `particionar_verbos` devolve HISTORICO e janela do colaborador exclui. Qual política representa o resultado devido é **[nao verificado]**.

- **`PerguntaDisputa.materializada_em`: não sei, novo leitor com risco UTC/civil.** `universo_i6` extrai o dia cru do timestamp em `app/chamados/management/commands/auditar_invariantes_chamados.py:478`, contra corte `2026-08-24` em `app/chamados/management/commands/auditar_invariantes_chamados.py:452`. Entrada `2026-08-25T01:00Z`, equivalente a `24/08 22:00` local, entra na violação; a leitura civil local entraria no piso. Entretanto essa comparação só alcança `legado_sem_lastro`, declaradamente sem escritor vivo em `app/chamados/models.py:2277` e `app/chamados/management/commands/auditar_invariantes_chamados.py:445`; origem/fuso histórico do carimbo e ocorrência no acervo **[nao verificado]**. Família **chamado**.

- **`julgado_em` / `janela_expira_em`: leitor adicional para achado do explorador `juizes`.** Varredura usa `now().date()` e `_jul.date()` crus em `app/chamados/services/raiox.py:36` e `app/chamados/services/raiox.py:41`. O cron consome esse resultado em `app/chamados/management/commands/lavrar_perguntas_stale.py:58`. Escritor adicional lido: backfill grava `julgado_em=timezone.now()` e expiração recebida de `expira_em` em `app/chamados/management/commands/backfill_veredito_lavrado.py:94`, `app/chamados/management/commands/backfill_veredito_lavrado.py:139` e `app/chamados/management/commands/backfill_veredito_lavrado.py:140`.

**Parâmetros: 19 examinados nesta faixa**, contando `data` temporal e `dia_corte`; **17 sem divergência observada na amostra**, **2 não sei por ausência de consumo**: `chamados_sem_dia_do_fato.data_ini` e `.data_fim` são declarados em `app/chamados/services/juizes_do_dia.py:75`, mas o corpo consulta todos os vivos e não utiliza nenhum deles em `app/chamados/services/juizes_do_dia.py:92`. Limites solicitados não alteram o resultado; significado pretendido **[nao verificado]**.

Os outros 17 parâmetros foram lidos nestes corpos:

| Parâmetros | Sítios |
|---|---|
| `_competencia.data`, `.dia_corte` | `app/chamados/management/commands/arquivar_competencia_encerrada.py:177` |
| `motivos_pela_ata.dia`, `perguntar.dia` | `app/chamados/services/fabrica_da_celula.py:34`, `app/chamados/services/fabrica_da_celula.py:129` |
| `ListaDoAdmin.dia_do_admin.data_iso`, `.dia_na_lista.data_iso`, `.papel_do_dia.data_iso`; `dono_fora_do_fio.data`; `competencia_do_dia_entregue.dia` | `app/chamados/services/vigia_fio.py:222`, `app/chamados/services/vigia_fio.py:230`, `app/chamados/services/vigia_fio.py:238`, `app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:822` |
| `_gaveta_de.dia`, `_chave_de_toque.dia` | `app/chamados/services/fila_por_causa.py:214`, `app/chamados/services/fila_por_causa.py:295` |
| `cobrar_dia.data` | `app/chamados/services/cobrar_dia.py:16` |
| `_marco_ja_perguntado.dia`, `do_dia.data` | `app/chamados/services/pergunta_no_app.py:99`, `app/chamados/services/pergunta_no_app.py:152` |
| `semana_muda.data`, `pode_calar.data` | `app/chamados/services/limbo_folgas.py:144`, `app/chamados/services/limbo_folgas.py:160` |
| `reavaliar_dia.data` | `app/chamados/services/feriado_reavaliar.py:59` |

Se a contagem incluir os quatro parâmetros extras `ini/fim` do inventário, somam-se `_texto.ini/fim` e `universo_adesao.ini/fim`: apresentação e filtro inclusivo `data__range`, respectivamente, em `app/chamados/services/adesao.py:12` e `app/chamados/services/adesao.py:25`; total **23**, mantendo **2 não sei**. Não há evidência adicional de divergência nesses quatro corpos. `app/chamados/services/adesao.py:16`, `app/chamados/services/adesao.py:34`.

Amostras adicionais de campos abertas: cobertura escreve início/fim como datetimes aware e exige fim maior que início em `app/ponto/services/cobertura.py:57`, `app/ponto/services/cobertura.py:67`, `app/ponto/services/cobertura.py:105`; detector compara os mesmos instantes em `app/chamados/detector_anomalias.py:71`. Timestamp de batida é convertido para local antes de agrupamento em `app/chamados/services/limbo_folgas.py:35` e `app/chamados/services/desvio_escala.py:62`; este último aplica depois uma heurística própria de madrugada em `app/chamados/services/desvio_escala.py:66`, cuja equivalência completa com o pareamento de turnos é **[nao verificado]**.

## 06/11 19:20 · sessao ef95a29037c8 · - · - · origem=Ronald (tmux)

Achado confirmado: **`PerguntaDisputa.data_evento`: dia do turno × dia civil da hora respondida**.

- Escritor: `app/chamados/services/declaracao_texto.py:299` obtém `dia` do marco/célula e passa `data_evento=dia` em `:326`; persistência em `app/chamados/models.py:2476`.
- Leitor divergente: `app/chamados/services/validacao.py:826` combina diretamente `data_evento + HH:MM`, procurando conflitos em `:828`.
- Materialização interpreta o mesmo campo como dia do turno: `app/chamados/services/materializacao.py:414` combina inicialmente e `:420` ajusta overnight; `app/chamados/juizes.py:303` consulta marcos e `:310–313` pode escolher o dia seguinte.
- Caso por código: turno 20/10/2026 19h → 21/10 07h; pergunta `data_evento=20/10`, resposta `07:00`, marco esperado `07:00`, batida existente em 21/10 07:05, nenhuma perto de 20/10 07h. O leitor procura no dia 20 e retorna `LOTE_A` em `validacao.py:830`, cujo rótulo afirma “sem batida perto” em `:765`; o plantio deriva dia 21. Ocorrência em produção: **[nao verificado]**.
- Família: **chamado**, com geometria **turno/marcos**; famílias declaradas em `app/core/contratos_estruturais.py:56`.

Variante do mesmo leitor: resposta `data_hora` tem sua data descartada para calcular o delta em `app/chamados/juizes.py:1576–1578`; `classe_do_lote` usa `data_evento` em `validacao.py:826`, enquanto o materializador usa a data explícita respondida em `materializacao.py:974–977`.

**Parâmetros examinados: 20; divergentes identificados entre esses parâmetros: 0; campo divergente identificado: 1 (`data_evento`).** Contagem por declarações:

| Arquivo | Parâmetros |
|---|---:|
| `materializacao.py:55`, `:193`, `:227` | 3 |
| `regularizacao_ext.py:70`, `:322` | 2 |
| `disputa_emissao.py:80`, `:149`, `:627`, `:732`, `:742`, `:756`, `:783`, `:818`, `:1209` | 9 |
| `declaracao_texto.py:53`, `:65`, `:214` | 3 |
| `fio.py:784` | 1 |
| `passivo_validado.py:86`, `:474` | 2 |

Nas demais leituras realizadas desta subfatia, não identifiquei outro par temporal divergente; isso não constitui contagem exaustiva de todos os campos/modelos do grupo. **[nao verificado]**

## 06/11 19:15 · sessao 62e8ac40805c · ESPURIA-TELA · censo · origem=Code

**`tipo` e `tipo_real` não são sinônimos:** o produtor grava o papel previsto em `tipo` e conserva em `tipo_real` o tipo gravado da batida atribuída. O espelho tem dois sítios que apresentam essa batida usando o previsto; outros leitores comparam os dois campos ou reinterpretam deliberadamente o papel da batida. (`app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:688`, `app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:237`, `app/ponto/turnos.py:1380`)

Os números **200 dias/303 lâmpadas divergentes e 578 dias com tipo desconhecido** são os fornecidos pela sonda, sem confirmação independente nesta leitura. A correspondência do hash informado com um HEAD e a execução efetiva dos caminhos abaixo em produção são **[nao verificado]**.

**P1 — escritores**

| Arquivo:linha | Escritor | Significado e origem dos campos |
|---|---|---|
| `app/escala/utils.py:326` | `_celula_do_marco` | Na célula intermediária, escreve `tipo=papel=tipo_m`, do marco; escreve `tipo_real` com o argumento recebido da batida. |
| `app/escala/utils.py:619`, `app/escala/utils.py:661` | `_match_marcos` | Alimenta o escritor acima com o tipo do marco e, separadamente, o tipo da batida casada. |
| `app/escala/utils.py:1566`, `app/escala/utils.py:1569`, `app/escala/utils.py:1572` | `montar_grade_prevista_periodo_por_turno`, intermitente | Escreve `tipo/papel` alternando E/S **pela posição**; `tipo_real` vem da batida. |
| `app/escala/utils.py:1849` | Mesmo builder, intervalo | Escreve `tipo=papel='I'`, categoria de intervalo; conserva E/S da batida em `tipo_real`. |
| `app/ponto/services/cartorio.py:586`, `app/ponto/services/cartorio.py:638` | `lampadas_do_dia`, marco do DNA sem coluna correspondente | `tipo=t` vem da chave prevista do DNA. Se encontra hora por proximidade, acende a lâmpada e escreve **`tipo_real=None`**. |
| `app/ponto/services/cartorio.py:642`, `app/ponto/services/cartorio.py:644` | Mesmo ramo, sem casamento | Escreve o tipo previsto e `tipo_real=None`; `acesa` fica desconhecida ou falsa. |
| `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:679` | `lampadas_do_dia`, marco do DNA com coluna | Escreve `tipo=t`, previsto; se acesa, copia `x.get('tipo_real')`; apagada recebe `None`. |
| `app/ponto/services/cartorio.py:688`, `app/ponto/services/cartorio.py:690` | `lampadas_do_dia`, ramo sem casamento utilizável com DNA | `tipo` vem de `marcos[j].tipo`, com reserva em `x.tipo`; `tipo_real` vem exclusivamente de `x.tipo_real`, salvo missing, que recebe `None`. |
| `app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:704` | `ata_do_dia` | Coloca as lâmpadas produzidas acima em `ata['lampadas']`; não redefine os tipos. |
| `app/escala/utils.py:1907`, `app/escala/utils.py:1913` | Builder, ata provisória em memória | Usa **o mesmo** `lampadas_do_dia` numa cópia da célula, antes de consultar o realizado. |
| `app/ponto/services/cartorio.py:1800`, `app/ponto/services/cartorio.py:1802` | Fluxo de lavratura do cartório | Entrega a ata à porta de lavratura. |
| `app/ponto/portas/celula.py:990`, `app/ponto/portas/celula.py:1001`, `app/ponto/portas/celula.py:1028`, `app/ponto/portas/celula.py:1035` | `lavrar_veredito` | Copia e persiste a ata recebida; não decide o significado de `tipo` nem de `tipo_real`. |

**Há caminhos atuais com lâmpada acesa e `tipo_real=None`:**

| Caminho | Evidência |
|---|---|
| **Proximidade:** existe marco do DNA sem coluna correspondente na régua; uma hora real ainda não usada, até 30 minutos distante, acende a lâmpada. Só há a hora para copiar, não o tipo da batida. | `app/ponto/services/cartorio.py:624`, `app/ponto/services/cartorio.py:632`, `app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:640` |
| **Isenção:** uma célula sem batida recebe crédito, hora prevista e `missing=False`, sem receber tipo real; o cartório propaga o carimbo de isenção. | `app/escala/utils.py:858`, `app/ponto/services/cartorio.py:556`, `app/ponto/services/cartorio.py:679` |
| **Dispensa por ausência/justificativa:** uma lâmpada apagada passa a acesa, com `luz=None` e `dispensada`; não se fabrica tipo de batida. | `app/ponto/services/cartorio.py:773`, `app/ponto/services/cartorio.py:774`, `app/ponto/services/cartorio.py:775`, `app/ponto/services/cartorio.py:776` |
| **Cobertura parcial:** a grade produz célula dispensada com `tipo_real=None`; o cartório conserva a dispensa. | `app/escala/utils.py:1267`, `app/escala/utils.py:1268`, `app/ponto/services/cartorio.py:561` |
| **Propagação de desconhecido:** os ramos normais usam `x.get('tipo_real')`; uma célula sem essa informação produz `None`. O leitor também trata chave ausente como desconhecida. | `app/ponto/services/cartorio.py:679`, `app/ponto/services/cartorio.py:690`, `app/escala/services/leitor_celula.py:128` |

Nas construções atuais de lâmpadas acima, a chave é escrita explicitamente; **ausência de valor** não exige ausência da chave. Esses caminhos explicam como a condição pode existir, mas a distribuição dos **578 dias** entre proximidade, isenção, dispensa, atas antigas ou outras origens é **[nao verificado]**. (`app/ponto/services/cartorio.py:640`, `app/ponto/services/cartorio.py:679`, `app/ponto/services/cartorio.py:690`)

**P2 — censo estático dos leitores localizados**

Nas tabelas, **M** significa tipo previsto do marco/lâmpada; **R**, `tipo_real`; **B**, `Batida.tipo`; **C**, classificação recalculada pela regra descrita. Cada linha informa o efeito para **M=E/R=S**; quando existem outras condições, o efeito é condicional. A inexistência absoluta de outros sítios é **[nao verificado]**.

**Adaptação, tela, calendário e PDF**

| Arquivo:linha | Função/sítio | Leitura/regra | Uso | Resultado para E/S |
|---|---|---|---|---|
| `app/escala/services/leitor_celula.py:122`, `app/escala/services/leitor_celula.py:128`, `app/escala/services/leitor_celula.py:129`, `app/escala/services/leitor_celula.py:144` | `_celulas_da_ata` | M e R separados | Transporte para consumidores | Entrega `tipo=papel=E`, `tipo_real=S`. |
| `app/escala/utils.py:1112`, `app/escala/utils.py:1128`, `app/escala/utils.py:1130` | `montar_realizado_grade` | M e R | Modelo de apresentação | Entrega `tipo=E`, `tipo_real=S`, `divergente=True`. |
| `app/templates/ponto/espelho.html:151` | Calendário compacto | M no ramo `realizado_grade`; B no fallback | Cor da marca realizada | Usa `hx-b-E`; esse ramo não apresenta o alerta de divergência. |
| `app/templates/ponto/espelho.html:237` | Cartão diário, “Batidas” | M; consulta `divergente` separadamente | Seta e alerta | Mostra **▶ hora + alerta**, apesar de R=S. |
| `app/templates/ponto/espelho.html:223`, `app/templates/ponto/espelho.html:234`, `app/templates/ponto/espelho.html:235` | Previsto e faltantes | M | Ícone do marco previsto/apagado | Previsto continua E; lâmpada acesa não entra como faltante. |
| `app/templates/ponto/espelho.html:79`, `app/templates/ponto/espelho.html:246` | Batida crua/fallback | B | Rótulo e seta | Mostra S/saída. |
| `app/colaboradores/services/calendario.py:658`, `app/colaboradores/services/calendario.py:659` | `contexto_calendario` | B | Listas entradas/saídas | Coloca a batida em saídas. |
| `app/colaboradores/services/calendario.py:693`, `app/colaboradores/services/calendario.py:698` | Mesmo contexto | B, pela lista de entradas | Presença/geo | Essa S não fornece entrada principal; sem outra E, geo fica cinza. |
| `app/templates/colaboradores/partials/_calendario_grade.html:118`, `app/templates/colaboradores/partials/_calendario_grade.html:121` | Calendário administrativo | B | Seta, texto e ação | Mostra S quando o bloco abre; só S, sem entrada nem retratada, pode ficar fora da guarda do bloco. |
| `app/colaboradores/services/drawer.py:48`, `app/colaboradores/services/drawer.py:50`, `app/colaboradores/services/drawer.py:55` | `contexto_drawer` | B; pareamento E→S | Duração de presença | Trata S como fechamento; resultado depende das vizinhas. |
| `app/colaboradores/services/drawer.py:62`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:48` | Drawer | B transportado | Letra da batida | Mostra S. |
| `app/colaboradores/services/detalhe.py:84`, `app/templates/colaboradores/detalhe.html:182`, `app/templates/colaboradores/detalhe.html:242` | Detalhe/mapa | B | Letra e Entrada/Saída | Mostra S/Saída. |
| `app/core/views.py:999`, `app/core/views.py:1017`, `app/core/views.py:1018` | `batida_detalhe_json` | B | Histórico e `tipo_label` | Entrega S/Saida. |
| `app/templates/core/_d2_feed.html:14`, `app/templates/core/dashboard2.html:237`, `app/templates/core/dashboard2.html:261` | Feed/drawer/histórico | B transportado | Cor, símbolo e rótulo | Apresenta saída. |
| `app/templates/chamados/fio_colaborador.html:118`, `app/templates/chamados/fio_colaborador.html:125` | Geo no fio | B/display | Rótulo | Mostra Saída. |
| `app/templates/ponto/batidas_offline_pendentes.html:60` | Offline pendente | B | Badge | Mostra Saída. |
| `app/relatorios/pdf_espelho.py:881` | `_build_story` | B | Alimenta desenho de marcações | Entrega S ao desenho. |
| `app/relatorios/pdf_espelho.py:164`, `app/relatorios/pdf_espelho.py:67` | `Marcacoes.draw` / `_desenhar_triangulo` | Tipo recebido de B | Triângulo no PDF | Desenha saída, para a esquerda. |
| `app/ponto/models.py:112`, `app/ponto/admin.py:13`, `app/ponto/admin.py:14` | `Batida.__str__` / `BatidaAdmin` | B | Texto, coluna e filtro administrativo | Mantém S. |

**Chamados, perguntas, cobrança e materialização**

| Arquivo:linha | Função/sítio | Leitura/regra | Uso | Resultado para E/S |
|---|---|---|---|---|
| `app/chamados/detector_anomalias.py:201`, `app/chamados/detector_anomalias.py:210`, `app/chamados/detector_anomalias.py:213` | Detector de anomalia | B; refaz `_match_marcos(..., derivar=False)`; identifica por hora+R | Classificar desvio/abrir chamado | Localiza a S no marco E; decisão posterior depende da posição e distância. |
| `app/chamados/detector_anomalias.py:223`, `app/chamados/detector_anomalias.py:226`, `app/chamados/detector_anomalias.py:231` | Continuação do detector | Marco ocupado e tipo recebido | Borda, gravidade e contexto | Preserva `tipo_batida=S`; pode medir desvio contra o marco E. |
| `app/chamados/reconciliador.py:221` | `_par_integro` | B | Integridade do par | Considera S; depende do tipo da outra batida. |
| `app/chamados/juizes.py:551` | `entrada_de_marco` | M cobrado; reserva legada E/S | Identificar marco de entrada | Seleciona o marco, sem rebatizar a batida existente. |
| `app/chamados/juizes.py:910`, `app/chamados/juizes.py:913` | `fato_do_marco_no_espelho` | M+hora e `acesa` | Fato do marco cobrado | Considera o marco E cumprido; não exige R=E. |
| `app/chamados/juizes.py:1111`, `app/chamados/juizes.py:1112`, `app/chamados/juizes.py:1130` | `_resposta_concorda_com_a_luz` | M+hora; compara resposta com luz | Concordância da resposta | Pode concordar com a luz S atribuída ao E, se as demais guardas passarem. |
| `app/ponto/services/lastro.py:78`, `app/ponto/services/lastro.py:92` | `_marcos_cobrados` / `_lampada` | M do contexto e da lâmpada | Selecionar cobrança | Seleciona a lâmpada E. |
| `app/ponto/services/lastro.py:175`, `app/ponto/services/lastro.py:179` | `marcos_acesos` | M para localizar; estado para decidir | Cobrança ainda tem furo? | Retorna verdadeiro para esse marco E aceso por S. |
| `app/ponto/services/lastro.py:131`, `app/ponto/services/lastro.py:148` | `marcos_apagados` / `marcos_pendentes` | M | Nomear furos/pendências | Acesa não entra nos apagados. |
| `app/ponto/services/lastro.py:287`, `app/ponto/services/lastro.py:291`, `app/ponto/services/lastro.py:308` | `julgar` | Lâmpada primeiro; sem lâmpada exige B igual ao cobrado | Lastro da cobrança | Com lâmpada aceita E/S; no recuo, uma S não satisfaz cobrança E. |
| `app/ponto/services/lastro.py:346`, `app/ponto/services/lastro.py:356` | `_texto_da_nota` | M para selecionar a luz | Texto de encerramento | Afirma que “a batida que este chamado cobrava EXISTE”, sem informar R=S. |
| `app/chamados/services/declaracao_texto.py:101`, `app/chamados/services/declaracao_texto.py:112`, `app/chamados/services/declaracao_texto.py:118` | `_marcos_faltantes` | M apagado | Nomear início/fim/ida/volta faltante | E aceso por S não vira faltante. |
| `app/chamados/services/declaracao_texto.py:137`, `app/chamados/services/declaracao_texto.py:142` | `_casar_hora` | C: proximidade circular até 240 min; recusa empate próximo | Associar declaração a marco faltante | O par aceso não basta; atua sobre faltantes. |
| `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/declaracao_texto.py:426` | `projecao_por_marco` | Posição+M | Converter marco em motivo | Conserva o papel previsto. |
| `app/chamados/services/declaracao_texto.py:579`, `app/chamados/services/declaracao_texto.py:586`, `app/templates/chamados/partials/modal_fio.html:129` | `propostas_do_chamado` / confirmação | M escolhido | Rótulo da **nova** batida proposta | Propõe Entrada para marco E; não rotula diretamente a S existente. |
| `app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:179` | `_plantio_preenche_furo_na_data` | Slots M; luzes/órfãs apenas por hora | Segunda avaliação posicional do plantio | Não distingue R=S nesse casamento. |
| `app/chamados/services/materializacao.py:274`, `app/chamados/services/materializacao.py:291`, `app/chamados/services/materializacao.py:297` | `_tipo_pelo_marco` | Marco cobrado; depois marco da ata próximo, até 240 min | Tipo da nova batida | Deriva E do marco, sem alterar ali a S existente. |
| `app/chamados/services/materializacao.py:98`, `app/chamados/services/materializacao.py:104` | `_vizinha_mesmo_tipo` | B | Recusar plantio E-E/S-S | A S conflita com plantio S. |
| `app/chamados/services/explicador.py:55`, `app/chamados/services/explicador.py:61`, `app/chamados/services/explicador.py:64` | `_explicar_validada` | B versus tipo esperado do plantio | Explicação da recusa | Explicita gravada S/esperada E. |
| `app/chamados/juizes.py:1573`, `app/chamados/juizes.py:1613` | `delta_da_resposta` | C: motivo entrada/volta/âncora→E; demais→S | Tipo e delta da resposta | Tipo é o esperado pela pergunta; não é R. |
| `app/chamados/juizes.py:1537`, `app/chamados/juizes.py:1542` | `_marco_esperado_da_grade` | Motivo e E/S→hi/hf/hii/hfi | Referência prevista da resposta | Usa o previsto. |
| `app/chamados/rotulos.py:102`, `app/chamados/rotulos.py:104`, `app/chamados/rotulos.py:105` | `direcao_da_resposta` | Tipo esperado+ sinal do delta | Atraso/antecipada | Classifica pelo tipo esperado da resposta. |
| `app/chamados/services/disputa_emissao.py:985` | `criar_disputa_a_partir_de_chamados_orfaos` | M faltante; S→ida, E→volta | Motivo de pergunta | Lâmpada acesa não entra por si. |
| `app/chamados/services/limbo_folgas.py:36`, `app/chamados/services/limbo_folgas.py:41` | `sugerir_folga` | Só B=E, agrupadas por dia da semana | Sugestão cadastral | Essa S não conta como entrada. |

**Juiz de períodos, pareador, motor e janela de HE**

| Arquivo:linha | Função/sítio | Leitura/regra | Uso | Resultado para E/S |
|---|---|---|---|---|
| `app/ponto/juiz_batida.py:174`, `app/ponto/juiz_batida.py:193`, `app/ponto/juiz_batida.py:239`, `app/ponto/juiz_batida.py:243`, `app/ponto/juiz_batida.py:246` | `periodos_do_dia` | Esperado do DNA; no posicional, M; conserva R | Papel, divergência e períodos | Produz papel E e `tipo_divergente=True`. |
| `app/ponto/juiz_batida.py:258`, `app/ponto/juiz_batida.py:259`, `app/ponto/juiz_batida.py:280` | Mesmo juiz, bordas/pausas | Chaves hi/hf e pares de pausa | Entrada/saída/intervalo/pergunta | Se for hi, a luz S fornece a entrada; R não veta isso. |
| `app/ponto/turnos.py:1643`, `app/ponto/turnos.py:1654`, `app/ponto/turnos.py:1680`, `app/ponto/turnos.py:1694` | `papel_por_minuto_da_ata` | Papel do juiz; batida única por minuto, bordas completas e guardas | Mapa para pareamento | Pode mapear a S como E; o par sozinho não garante passagem pelas guardas. |
| `app/ponto/turnos.py:888`, `app/ponto/turnos.py:903`, `app/ponto/turnos.py:1380` | `_papel_da_ata` / `parear_turnos` | Papel da ata prevalece; reserva em B | Abrir/fechar turno | Com papel E, trata a S como entrada; sem papel, mantém S. |
| `app/ponto/turnos.py:1355`, `app/ponto/turnos.py:1372` | `parear_turnos`, absorção de intervalo | Geometria dos marcos e B | Classificar miolo do turno | Pode absorver como intervalo, conforme a figura completa. |
| `app/ponto/turnos.py:342`, `app/ponto/turnos.py:357`, `app/ponto/turnos.py:368` | `_marca_ecos_flush` | B=E, criação e envelope temporal | Desconsiderar eco | Essa S não entra como candidata E. |
| `app/ponto/turnos.py:383`, `app/ponto/turnos.py:426`, `app/ponto/turnos.py:451`, `app/ponto/turnos.py:474` | `_marca_intra_duracao` | B S→E; duração/distância; ramo S/S | Intrajornada | Continua S; pode iniciar pausa conforme as demais guardas. |
| `app/ponto/turnos.py:607`, `app/ponto/turnos.py:615` | `_fim_fechado` | Pareamento E→S por B | Última saída/realizado | Pode fechar uma E anterior. |
| `app/ponto/turnos.py:636`, `app/ponto/turnos.py:649`, `app/ponto/turnos.py:655` | `_borda_posicional` | C: posição após borda fechada e guardas de pausa | Borda para realizado | Não consulta R da lâmpada; depende do conjunto. |
| `app/ponto/turnos.py:873`, `app/ponto/turnos.py:875`, `app/ponto/turnos.py:878` | `_pares_marcados` | B S/E ou carimbo/papel da ata | Intervalos subtraídos | Pode aceitar pausa por papel mesmo com tipos gravados invertidos. |
| `app/ponto/turnos.py:2395`, `app/ponto/turnos.py:2403`, `app/ponto/turnos.py:2419` | `decidir_tipo_estrito` | C: marco oposto até 10 min; próprio além de 30 min | Decidir flip | Pode devolver E para S próxima do marco E. |
| `app/ponto/turnos.py:2422`, `app/ponto/turnos.py:2433`, `app/ponto/turnos.py:2443` | `tipo_por_marcos` | C: único tipo de marco casado até 30 min | Tipo retroativo | Pode devolver E em lugar do S recebido. |
| `app/ponto/turnos.py:2448`, `app/ponto/turnos.py:2471`, `app/ponto/turnos.py:2486` | `proximo_tipo_de` | Intermitente: oposto da última B; demais: turno/intervalo/marcos | Próximo tipo | Última S pode sugerir E; demais caminhos dependem do estado. |
| `app/ponto/turnos.py:2496`, `app/ponto/turnos.py:2506` | `em_intervalo` | Última batida absorvida B=S | Estado de intervalo | Pode dizer intervalo, conforme o turno. |
| `app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:632`, `app/ponto/motor_calculo_v2.py:639`, `app/ponto/motor_calculo_v2.py:673`, `app/ponto/motor_calculo_v2.py:688` | `_periodos_pela_ata` | Entrada/saída do juiz; timestamps reais e cobertura | **Períodos usados no dinheiro** | Pode usar a S como entrada do período. |
| `app/ponto/motor_calculo_v2.py:677`, `app/ponto/motor_calculo_v2.py:678` | Mesmo caminho | `tipo_divergente` | Contador | Conta o dia como divergente; não rejeita o período por isso. |
| `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:893` | `MotorBase._dia_do_turno` | B=E dentro do turno | Entrada→dia | Essa S não entra pela condição E. |
| `app/ponto/motor_calculo_v2.py:897`, `app/ponto/motor_calculo_v2.py:902`, `app/ponto/motor_calculo_v2.py:909` | `remover_duplicatas` | B; consecutivas iguais dentro da tolerância | Universo do cálculo | Trata como S para deduplicação. |
| `app/ponto/motor_calculo_v2.py:2924`, `app/ponto/motor_calculo_v2.py:2927` | `MotorTurnoPartido.calcular_mes` | Índice timestamp+B | Associar períodos à jornada | Indexa como S. |
| `app/ponto/motor_calculo_v2.py:3058`, `app/ponto/motor_calculo_v2.py:3060`, `app/ponto/motor_calculo_v2.py:3066` | Mesmo motor, zona 5 | Recasamento dos períodos, rotulados E/S; exige R=E na célula resultante | P1/P2 e descontos | Uma célula resultante E/R=S não é aceita como entrada nesse trecho; o R aqui vem dos períodos remontados. |
| `app/ponto/janela_he.py:195`, `app/ponto/janela_he.py:204`, `app/ponto/janela_he.py:218` | `marcar_pontas_fora` | Primeira M=E/última M=S 

[truncado em 20000 de 39775 caracteres]

## 06/11 19:14 · sessao f7ec6d200508 · ESMERIL-SEMANTICA · censo · origem=Code

**Há quatro campos com divergência demonstrada; o censo de “todos os escritores” não ficou fechado.** As contagens abaixo distinguem sítios abertos de totais não demonstrados; `update_fields` e atribuições apenas em memória não contam como novos escritores persistentes. A existência de escrita dinâmica impede usar o RESUMO nominal como total por modelo (`app/colaboradores/services/colaborador.py:55`; `app/colaboradores/services/colaborador.py:60`). **Contagens consolidadas em memória: [nao verificado].**

No quadro, **“não” significa concordância nos escritores encontrados e na amostra declarada de leitores**, não prova sobre leitores não examinados. **E/L são quantidades de sítios efetivamente abertos**; total universal diferente disso permanece [nao verificado].

| Campo | Significados encontrados | Escritores: n e sítios abertos | Leitores: n e amostra lida | Divergente? | Caso que quebra: entrada → saída errada |
|---|---|---|---|---|---|
| **`ponto.Batida.timestamp`** | Instante; apresentação converte para local (`app/colaboradores/services/detalhe.py:85`). A sonda extrai o dia do datetime cru (`app/escala/management/commands/hub_smoke.py:100`). Fuso configurado: São Paulo, `USE_TZ=True` (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`). | **4** encontrados: criações (`app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:157`), correção (`app/chamados/services/regularizacao_ext.py:155`), semeadura dinâmica (`app/ponto/registro_batida.py:335`). Exaustividade dos valores dinâmicos [nao verificado]. | Amostra de **5 sítios**: apresentação local (`app/colaboradores/services/detalhe.py:85`), intervalo de instantes e agrupamento local (`app/colaboradores/services/calendario.py:241`; `app/colaboradores/services/calendario.py:244`), filtro local e extração crua da sonda (`app/escala/management/commands/hub_smoke.py:98`; `app/escala/management/commands/hub_smoke.py:100`). Total por entidade [nao verificado]. | **sim** | `2026-11-07T01:00Z` = **06/11 22h local**. O filtro inclui a batida no dia 06, mas `win_map` produz chave **07/11**. Se a batida nasce/some entre as fotografias, uma diferença do dia local 06 não é descontada como tráfego, podendo produzir `IDEMPOTENTE=False` indevido (`app/escala/management/commands/hub_smoke.py:100`; `app/escala/management/commands/hub_smoke.py:131`; `app/escala/management/commands/hub_smoke.py:148`). Ocorrência em produção [nao verificado]. |
| **`chamados.PerguntaDisputa.data_evento`** | Para `orfao_14h`, **dia civil da saída esperada** (`app/escala/servico_jornada.py:483`; `app/escala/servico_jornada.py:491`). Sem fato informado, pode ser o dia local da conversa (`app/chamados/models.py:2424`; `app/chamados/models.py:2427`). O calendário usa esse campo para bloquear uma chave de **dia do turno**, cujas batidas estão indexadas pela entrada (`app/colaboradores/services/calendario.py:224`; `app/colaboradores/services/calendario.py:477`). | **2 persistências**: nascimento (`app/chamados/models.py:2476`) e preenchimento de data ausente (`app/chamados/services/fio.py:834`; persistência em `app/chamados/services/fio.py:847`). A emissão fornece a data resolvida em `app/chamados/services/disputa_emissao.py:1041`. | **2 ocorrências B**, ambas abertas: seleção por período (`app/colaboradores/services/calendario.py:473`) e indexação (`app/colaboradores/services/calendario.py:477`); decisão consumidora em `app/colaboradores/services/calendario.py:738`. | **sim** | Hoje **06/11/2026**; turno **18/10 22h → saída prevista 19/10 06h**; pergunta respondida `"06:00"`, ainda viva, sem validação, materialização, tranca ou luz concordante. Guarda-se **19/10**; o leitor produz `pode_ultimo_recurso=False` para **19/10** e pode produzir `True` para **18/10**, sem outra pergunta daquele dia (`app/chamados/juizes.py:1136`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1162`; `app/colaboradores/services/calendario.py:738`). Visibilidade final do botão também depende de `app/colaboradores/services/calendario.py:97`; ocorrência em produção [nao verificado]. |
| **`ponto.Ausencia.data_fim`** | **Último dia incluído**: início + dias − 1 (`app/ponto/services/ausencia.py:66`; `app/ponto/services/ausencia.py:77`). Disponibilidade apresenta esse mesmo dia como **retorno** (`app/colaboradores/views.py:2924`; `app/templates/colaboradores/partials/disponibilidade.html:55`). | **3 operações abertas**: criação (`app/ponto/services/ausencia.py:343`), edição (`app/ponto/services/ausencia.py:500`), materialização de férias (`app/ponto/services/ausencia.py:1203`). Total universal [nao verificado]. | Amostra de **3 sítios**: cobertura inclusiva (`app/ponto/turnos.py:2554`), calendário pela autoridade (`app/colaboradores/services/calendario.py:485`), retorno apresentado (`app/colaboradores/views.py:2924`). | **sim** | Ausência iniciada em **06/11/2026**, por **3 dias** → fim **08/11** → tela informa **“retorno est. 08/11”**, ainda dentro da cobertura; primeiro dia fora dela: **09/11** (`app/ponto/services/ausencia.py:77`; `app/colaboradores/views.py:2924`; `app/templates/colaboradores/partials/disponibilidade.html:55`). Ocorrência em produção [nao verificado]. |
| **`ferias.AgendamentoFerias.data_fim` — encontrado fora dos CAMPO do anexo** | Último dia incluído (`app/ferias/services.py:313`); um bloco apresenta como retorno (`app/colaboradores/views.py:2904`), outro usa fim + 1 (`app/colaboradores/views.py:2853`). | **1 operação aberta**: cálculo e criação (`app/ferias/services.py:313`; `app/ferias/services.py:322`). Todos os demais escritores [nao verificado]. | **2 sítios**, ambos abertos: `app/colaboradores/views.py:2853`; `app/colaboradores/views.py:2904`. | **sim** | Início **01/11/2026**, **10 dias** → fim **10/11** → bloco “parados” informa retorno **10/11**; bloco “voltando” informa **11/11** (`app/ferias/services.py:313`; `app/colaboradores/views.py:2853`; `app/colaboradores/views.py:2904`). Ocorrência em produção [nao verificado]. |
| `escala.EscalaColaborador.data_inicio` | Dia inicial inclusivo da vigência (`app/escala/alimentacao.py:214`). | **5**: `app/colaboradores/services/vinculo.py:282`, `app/colaboradores/services/vinculo.py:914`, `app/colaboradores/services/vinculo.py:955`, `app/colaboradores/services/vinculo.py:1011`, `app/colaboradores/services/vinculo.py:1026`. | Amostra **2**: cobertura inclusiva (`app/escala/alimentacao.py:214`), validação da faixa (`app/colaboradores/services/vinculo.py:546`). | não | Nenhum demonstrado na amostra citada. |
| `escala.EscalaColaborador.data_fim` | Último dia incluído; troca em D encerra o anterior em D−1 (`app/colaboradores/services/vinculo.py:202`; `app/escala/alimentacao.py:215`). | **8**: `app/colaboradores/services/vinculo.py:268`, `app/colaboradores/services/vinculo.py:615`, `app/colaboradores/services/vinculo.py:691`, `app/colaboradores/services/vinculo.py:798`, `app/colaboradores/services/vinculo.py:914`, `app/colaboradores/services/vinculo.py:924`, `app/colaboradores/services/vinculo.py:955`, `app/colaboradores/services/vinculo.py:1021`. | Amostra **2**: cobertura (`app/escala/alimentacao.py:215`), consulta de interseção inclusiva (`app/escala/alimentacao.py:151`). | não | Nenhum demonstrado na amostra citada. |
| `escala.EscalaColaborador.data_ancora_colaborador` | Dia de referência da **fase do ciclo**; pode ser preservado antes do novo início de vigência (`app/colaboradores/services/vinculo.py:1012`; `app/colaboradores/services/vinculo.py:1027`). Leitor usa diferença em dias e módulo do ciclo (`app/escala/services/fase_conflitante.py:45`). | **8**: `app/colaboradores/services/vinculo.py:263`, `app/colaboradores/services/vinculo.py:272`, `app/colaboradores/services/vinculo.py:284`, `app/colaboradores/services/vinculo.py:915`, `app/colaboradores/services/vinculo.py:923`, `app/colaboradores/services/vinculo.py:937`, `app/colaboradores/services/vinculo.py:1012`, `app/colaboradores/services/vinculo.py:1027`. | Amostra **2**: fase (`app/escala/services/fase_conflitante.py:45`; `app/escala/services/fase_conflitante.py:47`). | não | Nenhum demonstrado. A alteração de `app/colaboradores/management/commands/corrigir_escala_retroativa.py:110` é projeção em memória, não persistência. |
| `escala.EscalaColaborador.regua_avaliada_em` | Instante da avaliação (`app/escala/regua_defesa.py:660`; `app/escala/regua_defesa.py:767`). | **2 pedidos persistentes**: `app/escala/regua_defesa.py:666`; `app/escala/regua_defesa.py:767`. | Amostra **2 guardas de presença**: `app/escala/regua_defesa.py:560`; `app/escala/regua_defesa.py:626`. | não | Nenhum demonstrado na amostra citada. |
| `escala.EscalaColaborador.regua_corroborada_em` | Instante da confirmação ou nulo após desfazer/expirar (`app/escala/regua_defesa.py:667`; `app/escala/regua_defesa.py:722`; `app/escala/regua_defesa.py:743`). | **3 pedidos persistentes**, nos sítios citados. | **1 apresentação local**: `app/escala/regua_defesa.py:658`. | não | Nenhum demonstrado na amostra citada. |
| `escala.AgendaDia.data` | Dia específico da agenda (`app/escala/models.py:1429`). | **1**: `app/escala/services/agenda.py:18`. | Amostra **2**: identidade por dia (`app/colaboradores/views.py:2229`), apresentação (`app/escala/models.py:1460`). | não | Nenhum demonstrado na amostra citada. |
| `escala.AgendaDia.criado_em` | Instante automático do nascimento (`app/escala/models.py:1441`). | Automático na criação aberta: `app/escala/services/agenda.py:17`. | Amostra **1 ordenação**: `app/colaboradores/views.py:2230`. | não | Nenhum demonstrado na amostra citada. |
| `escala.FolgaDia.data` | Dia de folga; a fotografia também pertence ao mês calendário (`app/escala/models.py:1588`; `app/escala/folgas.py:210`). | **3**: `app/escala/folgas.py:185`; `app/escala/folgas.py:313`; `app/escala/folgas.py:337`. | Amostra **3**: período inclusivo (`app/escala/alimentacao.py:100`), fotografia mensal (`app/escala/models.py:1250`), fase (`app/escala/services/fase_conflitante.py:74`). | não | Nenhum demonstrado na amostra citada. |
| `escala.CelulaDia.data` | Dia da célula/turno; períodos de dias inclusivos (`app/ponto/portas/celula.py:827`; `app/escala/alimentacao.py:77`). | **3 criações**: `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:600`; `app/ponto/portas/celula.py:850`. | Amostra **4**: `app/escala/alimentacao.py:77`; `app/escala/services/leitor_celula.py:350`; `app/escala/services/regeneracao.py:103`; `app/escala/services/propositor.py:473`. | não | `< hoje` no propositor exclui hoje e inclui ontem; não conflita com uma janela inclusiva terminada ontem (`app/escala/services/propositor.py:473`). |
| `escala.CelulaDia.gerada_em` | Instante do nascimento, separado da reescrita (`app/escala/models.py:1702`; `app/ponto/portas/celula.py:264`). | Automático nas **3 criações** citadas na linha anterior. | **1 comparação de fato novo**: `app/escala/services/fase_12x36.py:190`; comparação estrita em `app/escala/services/fase_12x36.py:192`. | não | Nenhum demonstrado na amostra citada. |
| `escala.CelulaDia.editada_em` | Instante da edição humana (`app/escala/models.py:1732`). | **2**: `app/ponto/portas/celula.py:601`; `app/ponto/portas/celula.py:607`. | Amostra **2**: máximo dos carimbos (`app/escala/services/regeneracao.py:84`), posterior estrito à cobrança (`app/escala/services/regeneracao.py:120`). | não | Nenhum demonstrado na amostra citada. |
| `escala.CelulaDia.julgada_em` | Instante em que o cartório imprimiu os insumos; repetição idêntica não o move (`app/ponto/portas/celula.py:1054`; `app/ponto/portas/celula.py:1060`). | **1**: `app/ponto/portas/celula.py:1060`. | **1 comparação estrita**: `app/escala/services/fase_12x36.py:189`; `app/escala/services/fase_12x36.py:192`. | não | Nenhum demonstrado na amostra citada. |
| `escala.CelulaDia.insumos_em` | Instante da observação dos insumos; nulo distingue “nunca visto” (`app/ponto/portas/celula.py:1052`). | **2**: `app/ponto/portas/celula.py:1064`; `app/ponto/portas/celula.py:1100`. | **2 guardas**: `app/ponto/portas/celula.py:1056`; `app/ponto/portas/celula.py:1095`. | não | Nenhum demonstrado na amostra citada. |
| `escala.EventoCelula.processado_em` | Instante do processamento; nulo significa pendente (`app/chamados/reconciliador.py:1378`; `app/chamados/reconciliador.py:1386`). | **1**: `app/chamados/reconciliador.py:1386`. | Amostra **3 condições de pendência**: `app/escala/models.py:1867`; `app/escala/models.py:1869`; `app/escala/models.py:1871`. | não | Nenhum demonstrado na amostra citada. |
| `escala.FuroDiario.data` | Dia da grade; apuração inclusiva limitada a ontem (`app/escala/services/furos_diarios.py:27`). | **1**: `app/ponto/portas/celula.py:1125`. | Amostra **2 períodos inclusivos**: `app/escala/services/furos_diarios.py:133`; `app/escala/services/furos_diarios.py:178`. | não | Nenhum demonstrado na amostra citada. |
| `escala.DecisaoProposta.decidida_em` | Instante da decisão; recusa recente usa duração de 30 dias e limite inferior inclusivo (`app/escala/services/propositor.py:700`; `app/escala/services/propositor.py:702`). | Automático em **2 criações**: `app/escala/services/propositor.py:717`; `app/escala/services/propositor.py:773`. | **1 filtro**: `app/escala/services/propositor.py:702`. | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.Colaborador.data_demissao` | Dia do desligamento: futuro agenda; chegada do dia efetiva. O mesmo dia encerra a vigência (`app/colaboradores/services/desligamento.py:57`; `app/colaboradores/services/desligamento.py:123`). | **3**: `app/colaboradores/services/desligamento.py:86`; `app/colaboradores/services/desligamento.py:105`; `app/colaboradores/services/desligamento.py:211`. | Amostra **3**: cron `<= hoje` (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:39`), células posteriores `>` e fechamento (`app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46`; `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`). | não | Nenhum desacordo demonstrado entre efetivação administrativa e último dia incluído da vigência. |
| `colaboradores.Colaborador.notificacao_negada_em` | Instante da primeira negativa; leitor testa presença (`app/ponto/views.py:1959`; `app/ponto/views.py:1962`). | **1**: `app/ponto/views.py:1962`. | **1**: `app/ponto/views.py:1959`. | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.Colaborador.gps_concedido_em`; `camera_concedida_em` | Instantes da concessão, consumidos como conclusão booleana (`app/core/views.py:1083`; `app/core/views.py:1086`; `app/colaboradores/models.py:413`). | **1 por campo**, nos dois sítios citados. | Amostra **2 por campo**: mapa e consumo (`app/colaboradores/models.py:407`; `app/colaboradores/models.py:408`; `app/colaboradores/models.py:413`), onboarding (`app/core/views.py:483`; `app/core/views.py:485`). | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.HistoricoVinculo.vigencia_ini` | Dia inicial do trecho descrito (`app/colaboradores/services/historico_vinculo.py:15`). | **1**: `app/colaboradores/services/historico_vinculo.py:15`. | **1 identificação do trecho**: `app/escala/services/escala_auto_executor.py:130`. | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.HistoricoVinculo.data` | Dia do evento/efeito; pode ser futuro ou retroativo, distinto do instante de gravação (`app/colaboradores/services/desligamento.py:90`; `app/colaboradores/services/vinculo.py:337`). | **2**: `app/colaboradores/services/historico_vinculo.py:15`; `app/colaboradores/services/historico_vinculo.py:26`. | **1 apresentação “em”**: `app/colaboradores/models.py:592`. | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.HistoricoVinculo.criado_em` | Instante da gravação; prazo de desfazer usa explicitamente seu dia local (`app/colaboradores/models.py:584`; `app/escala/services/escala_auto_executor.py:127`). | Automático: `app/colaboradores/models.py:584`. | **2**: `app/escala/services/escala_auto_executor.py:127`; `app/escala/services/escala_auto_executor.py:157`. | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.SolicitacaoBeneficio.criado_em` | Instante da criação, ordenado por recência (`app/colaboradores/models.py:791`; `app/colaboradores/views.py:2017`). | Automático na criação aberta em `app/colaboradores/services/beneficios.py:22`. | **2**: `app/colaboradores/views.py:2004`; `app/colaboradores/views.py:2017`. | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.DeviceColaborador.ultimo_login` | Instante de criação/relogin (`app/colaboradores/models.py:920`). | **2 operações**: `app/colaboradores/services/aparelho.py:28`; `app/colaboradores/services/aparelho.py:53`. | **4 ordenações por recência**: `app/colaboradores/management/commands/backfill_plataforma_fossil.py:50`; `app/colaboradores/painel_cell.py:29`; `app/colaboradores/services/ficha.py:140`; `app/colaboradores/services/situacional.py:21`. | não | Nenhum demonstrado na amostra citada. |
| `chamados.DisputaSupervisao.fechada_em` | Instante de fechamento; nulo significa aberta (`app/chamados/models.py:1861`; `app/colaboradores/services/drawer.py:70`). | **3**: fechar (`app/chamados/models.py:1861`), duas reaberturas (`app/chamados/models.py:2063`; `app/chamados/models.py:2087`). | Amostra **2 de 6 ocorrências B localizadas**: `app/colaboradores/services/drawer.py:70`; `app/colaboradores/services/desligamento.py:139`. Total nominal B [nao verificado]. | não | Nenhum demonstrado na amostra citada. |
| `chamados.DisputaSupervisao.aberta_em` | Instante automático da abertura (`app/chamados/models.py:1686`). | Automático: `app/chamados/models.py:1686`. | **1 ordenação**: `app/colaboradores/services/drawer.py:71`. | não | Nenhum demonstrado na amostra citada. |
| `ponto.AlertaGeofence.obsoleto_em` | Instante em que a acusação caducou; nulo mantém sua validade (`app/ponto/management/commands/reconciliar_alertas_geofence.py:50`; `app/ponto/management/commands/reconciliar_alertas_geofence.py:52`). | **1**: `app/ponto/management/commands/reconciliar_alertas_geofence.py:52`. | **3**, todos abertos: `app/colaboradores/services/detalhe.py:61`; `app/colaboradores/services/detalhe.py:65`; `app/colaboradores/services/detalhe.py:68`. | não | Nenhum demonstrado na amostra citada. |
| `ponto.AlertaGeofence.criado_em` | Instante automático da criação (`app/ponto/models.py:244`). | Automático: `app/ponto/models.py:244`. | **1 ordenação**: `app/colaboradores/services/detalhe.py:62`. | não | Nenhum demonstrado na amostra citada. |
| `ponto.Justificativa.data_turno` | Dia da entrada do turno (`app/ponto/services/regularizacao.py:73`; `app/ponto/services/triagem_batida.py:252`). | **5**: `app/ponto/views.py:1635`; `app/api/views.py:2007`; `app/ponto/services/triagem_batida.py:258`; `app/ponto/services/regularizacao.py:91`; `app/ponto/management/commands/processar_alertas_turno.py:391`. | **3 ocorrências atribuídas ao modelo**: dois limites inclusivos em `app/escala/utils.py:1219` e extração em `app/escala/utils.py:1220`. Os demais `t.data_turno` não foram atribuídos a este modelo. | não | Nenhum demonstrado na amostra citada. |
| `ferias.AgendamentoFerias.data_inicio` — extra | Primeiro dia incluído (`app/ferias/services.py:313`). | **1 criação aberta**: `app/ferias/services.py:322`; total universal [nao verificado]. | Amostra **2**: `app/colaboradores/views.py:2903`; `app/colaboradores/views.py:2904`. | não | Nenhum demonstrado na amostra citada. |
| `core.LogAuditoria.criado_em` — extra | Instante automático da trilha; janela histórica entrega esse instante sem convertê-lo em dia (`app/core/models.py:307`; `app/escala/services/regeneracao.py:67`). | Automático: `app/core/models.py:307`; todos os produtores de trilha [nao verificado]. | Amostra **2**: carregamento (`app/escala/services/regeneracao.py:60`), comparação estrita com instante da cobrança (`app/escala/services/regeneracao.py:125`). | não | Nenhum demonstrado na amostra citada. |
| `colaboradores.Empresa.janela_he_desde` | Di

[truncado em 20000 de 55312 caracteres]

## 06/11 19:13 · sessao 98323398dab6 · - · - · origem=Ronald (tmux)

Passagem concluída: **80 parâmetros literais em 70 funções**; **71 temporais em 61 funções**, excluindo nove parâmetros que recebem dicts do dia. A enumeração foi por AST em memória; não criei arquivos. [nao verificado]

| Grupo | Parâmetros temporais | Sentido/unidade/bordas e sítios |
|---|---:|---|
| `calculador/alimentacao.py` | 7 | `marcos_por_dia.data_ini/data_fim` (`:163`); `_no_dia_da_jornada.dia` (`:212`); `insumos_do_calculador.data_ini/data_fim` (`:305`); `rubricas_por_dia.data_ini/data_fim` (`:353`). Datas da jornada; `_no_dia...` muda apenas ano/mês/dia do instante (`:221`); rubricas recortam `ini <= k <= fim` (`:483` aproximadamente; verificar linha exata antes de citar). **`marcos_por_dia.data_ini` não é consumido pelo corpo** (`:188`–`:209`): não sei/sem efeito. |
| `calculador/periodo.py` | 4 | `segmentos_do_par.dia` (`:291`), `periodo_do_dia.dia` (`:340`), `pontualidade_no_dia.dia` (`:434`), `regua_do_dia.dia` (`:516`). Data da jornada, conservada nos segmentos (`:307`, `:309`), chave do agrupamento (`:355`), transmitida à pontualidade (`:476`) e ao previsto (`:523`). |
| `calculador/regras.py` | 4 | `pontualidade_do_dia.dia` (`:170`); `dobra_de_feriado_do_dia.dia` (`:313`); `folga_trabalhada_do_dia.dia` (`:367`); `do_dia.dia` (`:578`). Dia da jornada para teto diário, folga e feriado (`:270`, `:351`, `:394`, `:852`). `dia_de_trabalho` booleano não entra pelo critério literal (`:577`). |
| `janela_he.py` | 1 | `pontas_do_relato.dia` (`:234`): data da jornada publicada pelo motor, igualdade exata com `relato['dia']` (`:278`), recebido de `espelho.py:741`; o motor escreve a mesma chave em `motor_calculo_v2.py:1671`. | [nao verificado]
| Comandos: `curar_residuo_bug115` | 1 | `classificar.dia` (`:39`): dia do fato ou `None`, utilizado apenas para distinguir ausência de dia (`:40`); chamador recebe `dias_do_fato` e consulta `CelulaDia.data=d` (`:74`, `:78`, `:82`). |
| Comandos: `detectar_ausencias` | 5 | `data_ref` em `_hora_de_cobrar:460`, `_marcos_faltantes:475`, `_chamado_dia_sem_grade:500`, `_abrir_chamado:570`, `_afastamento_cobre:812`: data da jornada/grade, não instante. Consulta de célula por igualdade (`:468`), marcos do mesmo dia (`:483`), competência pela janela da empresa (`:592`), SLA no último dia às 23:59 locais (`:593`; `agora` é localizado em `:70`). |
| Comandos: `detectar_par_relampago` | 2 | `_poe.data_turno:86`, `_alvo_retratavel.data_turno:199`: data da jornada; composição local para busca com pad, limite inferior inclusivo e superior exclusivo (`:203`–`:207`); duração calculada de instantes (`:89`). |
| Comandos: fábrica smoke | 3 | `gerar_batidas_smoke_motor._trabalha_no_dia.dia:138`, `_batidas_do_dia.dia:148`, `_compor.dia:178`: dia de início; saída/intervalos overnight recebem dia+1 (`:155`, `:165`, `:166`); instantes aware locais (`:179`). |
| Comandos: competência/alertas | 5 | `passivo_disputa_retro._competencia_de.dia:45` → `janela_atual` (`:52`); `processar_alertas_avancados._enviar_push_dp.data_turno:227` só formatação (`:240`); `processar_alertas_turno._enviar_alerta.data_turno:225`, `_abrir_chamado_critico.data_turno:324`, `_abrir_justificativa_automatica.data_turno:389`: data da jornada recebida do materializado, enviada ao alerta (`:227`), contexto (`:332`) e justificativa (`:393`). |
| `marcos.py` | 3 | `marco_vencido.data_turno:70`, `celula_veta_cobranca.dia:122`, `_lei_cala_no_dia.dia:166`: datas da jornada; marcos de madrugada recebem dia+1 (`:102`, `:103`); vencimento inclusivo `<= agora` (`:88`, `:104`). O único chamador encontrado passa agora local naive (`management/commands/reconciliar_grade.py:81`, `:169`), compatível com a composição naive deste ramo. |
| `motor_calculo_v2.py` | 16 | `data_ini/data_fim` em `_turnos_do_juiz:867`, `_dia_do_turno:884`, `MotorBase.calcular_mes:2265`, `MotorTurnoPartido.calcular_mes:2899`, `MotorComercial.calcular_mes:3295` = 10. `dia` em `_feriado_dobra_no_dia:914`, `_folga_pelo_dia_previsto:936`, `_extra_declarada:1337`, `_empresa_da_janela_do_dia:1372`, `_dia_sem_previsto_fora_da_janela:1422`, `_acusa_cadastro_x_realidade:1655` = 6. Datas da jornada; recorte da competência fechado (`:2429`, `:2431`); início da janela de HE inclusivo (`:1393`). **Divergência nos dois parâmetros do MotorComercial:** a mesma apuração usa jornada para incluir o período (`:2429`–`:2432`) e data civil da entrada para DSR (`:3441`, `:3454`). |
| `nucleo.py` | 1 | `dt_fim_previsto_de.data_turno:31`: dia de início da jornada, usado para escolher escala e compor fim previsto (`:36`, `:37`). |
| `portas/celula.py` | 3 | `editar_dia.dia:534`, `regenerar_celulas_dia.dia:703`, `reverter_regeneracao_dia.dia:740`: data exata da célula; guarda de competência inclusiva (`:570`, `:781`), igualdade na consulta (`:593`, `:776`); regeneração unitária transmite `[dia,dia]` sem extensão (`:735`, `:736`). |
| `precedencia.py` | 5 | `folgas_lancadas_do_periodo.data_ini/data_fim:194`, `fatos_do_periodo.data_ini/data_fim:259`, `veto_de_lancamento.data_fim:430`: intervalo de datas fechado (`:207`, `:275`, `:335`); fim omitido no veto significa somente o dia inicial (`:444`). |
| `propositor.py` | 1 | `dia_posicional_completo.dia:325`: data consultada na escala (`:334`, `:337`), mas coleta batidas pelo dia civil local (`:348`). A própria função declara resultado não confiável/noturno (`:328`); não usei isso como novo bug provado, porque `False` nesse caso é parte do contrato declarado. |
| `selecao_periodo.py` | 1 | `janela_turno_de.data_ref:43`: data do início previsto; composição aware local e fim no dia seguinte se overnight (`:144`–`:149`, `:164`–`:169`). |
| `supra_juiz.py` | 2 | `cadastro_do_dia.data_demissao:142`, `_insumos_do_vinculo.data_demissao:181`: data civil do desligamento; só datas **posteriores**, `data > data_demissao`, contam como após demissão (`:192` aproximadamente; verificar linha exata). |
| `turnos.py` | 6 | `ausencia_cobre.data_ref:2536`, `afastamentos_no_dia.dia:2705`, `afastado_hoje.dia:2723`, `dias_da_ausencia.data_fim:2774`, `batidas_do_dia_turno.dia:3013`, `turno_aberto_ha.dia:3115`: datas; cobertura e expansão incluem o último dia (`:2554`, `:2718`, `:2787`); consulta por jornada (`:3034`, `:3122`). **Não sei quanto ao default de `afastado_hoje`: usa `datetime.date.today()` (`:2728`), cujo fuso efetivo depende do processo não observado.** |
| `utils.py` | 1 | `alertas_turno_visiveis.data_turno:222`: data da jornada, igualdade com `AlertaTurno.data_turno` (`:228`). |

Os **nove homônimos não temporais**, excluídos dos 71: `e6_oraculo.diferenca_e_a_janela_declarada.dia:203`, `minutos_do_espelho.dia:230`; `propositor.dia_degenerado.dia:34`; `status_dia.fatos_do_status.dia:166`; `supra_juiz.com_insumo_dia_sem_trabalho.dia:60`, `_dia_sem_trabalho_pelo_juiz.dia:65`, `motivo_retratacao.dia:109`, `classificar_dia.dia:204`; `turnos.horas_reais_do_dia.dia:2644`. Todos operam por `.get(...)`/dict, não por aritmética de datas nos respectivos corpos.

**Caso concreto adicional para o MotorComercial:** janela `[20/09/2026,20/09/2026]`, jornada prevista `20/09 23:50 → 21/09 08:00`, entrada real `21/09 00:00`, saída `08:00`, `datas_previstas_trabalho={20/09}` e sem justificativas. O juiz atribui a entrada a 20/09 (`turnos.py:297`–`:306`), o mapa do motor preserva essa data (`motor_calculo_v2.py:894`), e a base inclui o período (`:2429`–`:2432`). O override comercial constrói `datas_trabalho={21/09}` (`:3441`), encontra falta em 20/09 (`:3454`) e incrementa DSR perdido (`:3476` aproximadamente; conferir linha exata). O efeito decorre do código; existência desse cadastro em produção é [nao verificado].

Sugestão de consolidação desta fatia: **71 temporais; 2 divergentes** (`MotorComercial.data_ini/data_fim`), **2 não sei** (lower bound não consumido e default dependente do fuso do processo), **67 sem discordância encontrada na amostra**. Essas categorias não constituem prova universal de todos os chamadores. [nao verificado]

## 06/11 19:13 · sessao 1d061e11b3ef · ESMERIL-SEMANTICA · censo · origem=Code

**Há divergências. O censo universal não ficou fechado:** abaixo distingo os pares demonstrados, a concordância na amostra e os campos cuja cobertura ficou insuficiente. Os números E/L são **sítios efetivamente abertos**, não os totais globais por nome do `RESUMO`. Contagens globais por entidade permanecem [não verificado].

Os casos são consequências do código com entradas concretas; ocorrência em produção permanece [não verificado]. O fuso configurado é `America/Sao_Paulo`, com `USE_TZ=True`. (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`)

**(1) Quadro — divergências demonstradas**

| Campo | Significados encontrados | Escritores: n e sítios abertos | Leitores: n da amostra lida | Divergente? | Caso que quebra |
|---|---|---|---|---|---|
| `ChamadoColaborador.criado_em` | Instante de criação; API apresenta hora local; PDF apresenta datetime cru. (`app/chamados/models.py:61`; `app/api/views.py:1151`; `app/relatorios/pdf_chamados.py:114`) | E=1 automático: `app/chamados/models.py:61`. | L=3: `app/api/views.py:1151`; `app/relatorios/pdf_chamados.py:114`; `app/relatorios/views.py:924`. | **sim** | `2026-11-07T01:30Z` → PDF **07/11 01:30**, API **06/11 22:30**. (`app/relatorios/pdf_chamados.py:114`; `app/api/views.py:1151`) |
| `FollowupChamado.criado_em` | Instante; apresentação local na API e crua no PDF. (`app/chamados/models.py:1604`; `app/api/views.py:1138`; `app/relatorios/pdf_chamados.py:141`) | E=1 automático: `app/chamados/models.py:1604`. | L=2: `app/api/views.py:1138`; `app/relatorios/pdf_chamados.py:141`. | **sim** | Mesmo instante → PDF **07/11 01:30**, API **06/11 22:30**. (mesmos sítios) |
| `ExportacaoDominio.gerado_em` | Instante; Haiku/PDF convertem para local; aviso da prévia e `__str__` formatam cru. (`app/folha/haiku_export.py:42`; `app/folha/pdf_export.py:209`; `app/folha/views.py:363`; `app/folha/models.py:110`) | E=1 automático: `app/folha/models.py:80`; nascimento em `app/folha/models.py:182`. | L=5: os quatro formatadores citados e ordenação em `app/folha/models.py:136`. | **sim** | `2026-11-07T01:30Z` → aviso “Já exportado” **07/11 01:30**; PDF **06/11 22:30**. (`app/folha/views.py:363`; `app/folha/pdf_export.py:209`) |
| `Holerite.avisado_em` | Instante de aviso; PDF mostra local; CSV remove o fuso e imprime hora crua. (`app/holerite/servicos.py:114`; `app/holerite/views.py:1020`; `app/holerite/views.py:1093`) | E=2: `app/holerite/servicos.py:114`; `app/holerite/views.py:1823`. | L=2: `app/holerite/views.py:1020`; `app/holerite/views.py:1093`. | **sim** | `2026-11-07T01:30Z` → CSV **07/11/2026 01:30**, PDF **06/11 22:30**. (mesmos leitores) |
| `AssinaturaHolerite.assinado_em` | Instante da assinatura; PDF local e CSV cru. (`app/holerite/models.py:196`; `app/holerite/views.py:1022`; `app/holerite/views.py:1095`) | E=1 automático: `app/holerite/models.py:196`. | L=2: `app/holerite/views.py:1022`; `app/holerite/views.py:1095`. | **sim** | Mesmo instante → CSV **07/11 01:30**, PDF **06/11 22:30**. (mesmos leitores) |
| `PeriodoAquisitivo.data_fim` | Criação: aniversário seguinte menos um dia. Edição: início +364 dias. Leitor entende último dia inclusivo do ciclo. (`app/ferias/views.py:170`; `app/ferias/services.py:469`; `app/ferias/models.py:98`) | E=6: criação por `defaults` em `app/ferias/services.py:260`; edição `:469`; testes `app/ferias/tests.py:47`, `:79`, `:254`, `:267`. | L=3: `app/ferias/models.py:98`; `app/ferias/models.py:146`; `app/ferias/views.py:204`. | **sim** | Início **01/03/2023** → criação termina **29/02/2024**; editar a mesma entrada termina **28/02/2024**. Em 29/02, saldo positivo, o leitor responde `adquirido` um dia antes. (`app/ferias/views.py:170`; `app/ferias/services.py:469`; `app/ferias/models.py:98`) |
| `PeriodoAquisitivo.data_limite_concessao` | Criação soma ano de calendário ao fim; edição soma 365 dias. Prazo é inclusivo: vence quando `< hoje`. (`app/ferias/views.py:171`; `app/ferias/services.py:470`; `app/ferias/models.py:100`) | E=6: `app/ferias/services.py:260`, `:470`; testes `app/ferias/tests.py:47`, `:80`, `:255`, `:268`. | L=3: `app/ferias/models.py:100`; `app/ferias/situacao.py:193`; `app/inteligencia/views.py:17`. | **sim** | Início **02/03/2022** → fim **01/03/2023** nas duas rotas; limite da criação **01/03/2024**, da edição **29/02/2024**. Em 01/03, edição produz `vencido` antecipadamente. (escritores e leitor citados) |
| `Ausencia.data` | Escritor conserva início de um intervalo; relatório anual usa ano do início como universo, embora depois conte dias ocupados. (`app/ponto/services/ausencia.py:342`; `app/relatorios/views.py:278`; `app/relatorios/views.py:292`) | E=2 atos: criação `app/ponto/services/ausencia.py:342`; edição `:499`. | L=4: `app/relatorios/views.py:197`, `:278`, `:292`; `app/inteligencia/views.py:232`. | **sim** | Atestado aprovado **31/12/2025–02/01/2026** → relatório de 2026 exclui o registro e perde **dois dias**. (`app/relatorios/views.py:278`; `app/ponto/turnos.py:2785`) |
| `Ausencia.data_fim` | Fim inclusivo do intervalo; leitores que filtram somente pelo início não conservam essa cobertura. (`app/ponto/services/ausencia.py:66`; `app/ponto/services/ausencia.py:343`; `app/relatorios/views.py:278`) | E=2 atos: `app/ponto/services/ausencia.py:343`; `:500`. | L=3: `app/relatorios/views.py:198`, `:292`; expansão em `app/ponto/turnos.py:2787`. | **sim**, mesmo mecanismo | Mesmo atestado → seus dias finais de janeiro desaparecem antes da expansão inclusiva. (`app/relatorios/views.py:278`; `app/relatorios/views.py:292`) |
| `PerguntaDisputa.data_evento` | Há dia civil do evento: `orfao_14h` recebe **dia da saída esperada**; conversa recebe dia local da emissão; prévia da folha usa o campo como chave do **dia da célula**. (`app/escala/servico_jornada.py:485`; `app/escala/servico_jornada.py:491`; `app/chamados/models.py:2427`; `app/folha/previa.py:67`; `app/folha/previa.py:95`) | E=2 persistentes abertos: nascimento `app/chamados/models.py:2476`; preenchimento no reapontamento `app/chamados/services/fio.py:834`. Origem da saída em `app/chamados/services/disputa_emissao.py:1041`. | L=5 sítios: `app/api/views.py:1168`, `:1169`; `app/api/views_mensageria.py:568`; `app/folha/previa.py:65`, `:95`. | **sim** | Turno **20/10 19h–21/10 07h**, saída faltante; pergunta datada **21/10**. Na competência terminada em **20/10**, a prévia exclui a pergunta e informa `material='nada'` para a célula de 20/10, mesmo existindo material sobre sua saída. (`app/escala/servico_jornada.py:491`; `app/folha/previa.py:65`; `app/folha/previa.py:95`; `app/folha/previa.py:146`) |
| **Argumento/payload** `contexto_json.data_turno`, C9 | Escritor usa dia civil local da batida; leitor usa como data da célula. O detector já havia localizado o dia do turno. (`app/api/views.py:745`; `app/chamados/models.py:1201`; `app/chamados/detector_anomalias.py:161`) | E=2: `app/api/views.py:745`; clone `app/api/views_core.py:745`. | L=2: `app/chamados/models.py:1195`, `:1201`. | **sim** | Entrada **05/11 19h**, fim previsto **06/11 07h**, saída **07h20** → desvio leve do turno de **05/11**, mas payload recebe **06/11** e procura a célula desse dia. (`app/chamados/detector_anomalias.py:197`; `:243`; `app/api/views.py:745`; `app/chamados/models.py:1201`) |

Há também perda da borda final em “Sem ausência no período”: a janela **01–06/11** exclui uma pessoa cuja única ausência começa em **10/11**, pois o filtro aplica apenas `data >= início`. Isso afeta os mesmos campos `Ausencia.data/data_fim`, sem acrescentar outra identidade à contagem. (`app/inteligencia/views.py:45`; `app/inteligencia/views.py:46`; `app/inteligencia/views.py:232`; `app/inteligencia/views.py:235`)

**Quadro — sem divergência na amostra declarada**

“Não” nesta parte significa concordância dos escritores e leitores examinados; universalidade fora da cobertura permanece [não verificado].

| Campo — identidades agrupadas quando iguais | Significados encontrados | Escritores abertos | Leitores: amostra lida | Divergente? | Caso que quebra |
|---|---|---|---|---|---|
| `Feriado.data` | Date civil; limites inclusivos. (`app/core/views_config.py:45`; `app/core/feriados.py:29`) | E=3: `app/core/views_config.py:63`, `:82`, `:142`. | L=2 sítios: `app/core/feriados.py:29`, `:83`. | não | Nenhum demonstrado nesses sítios. |
| `CredencialUsuario.desde` | Instante da mudança observada; comparação com emissão UTC do token, igualdade aceita. (`app/api/credencial.py:100`; `:133`; `:134`) | E=1: `app/api/credencial.py:100`. | L=2: `app/api/credencial.py:121`, `:134`. | não | Nenhum demonstrado nesses sítios. |
| `CredencialUsuario.legado_visto_em` | Instante da última observação; intervalo mínimo de uma hora. (`app/api/credencial.py:162`; `:164`) | E=1: `app/api/credencial.py:164`. | L=1 sítio: `app/api/credencial.py:162`. | não | Nenhum demonstrado. |
| `CredencialUsuario.atualizado_em` | Instante automático de atualização, usado para ordenação. (`app/core/models.py:275`; `app/core/management/commands/replay_credenciais.py:103`) | E=2 saves que acionam o campo: `app/api/credencial.py:101`, `:165`. | L=1 sítio: `app/core/management/commands/replay_credenciais.py:103`. | não | Nenhum demonstrado. |
| `TokenAcessoWeb.criado_em`, `.usado_em` — 2 | Instantes; TTL aceita criação exatamente no limite; uso exige carimbo nulo. (`app/core/models.py:333`; `app/core/views.py:1272`) | Criação automática; E=1 consumo: `app/core/views.py:1272`. | L=1 sítio para ambos: `app/core/views.py:1272`. | não | Nenhum demonstrado. |
| `PlacarSituacional.lavrado_em` | Instante da lavra, apresentado localmente. (`app/core/management/commands/lavrar_placar_situacional.py:106`; `app/core/placar_leitura.py:65`) | E=1: comando citado. | L=1: `app/core/placar_leitura.py:65`. | não | Nenhum demonstrado. |
| `PlacarNavbar.lavrado_em` | Instante da lavra; idade em minutos. (`app/core/management/commands/lavrar_badge_navbar.py:93`; `app/core/context_processors.py:126`) | E=1: comando citado. | L=3: `app/core/context_processors.py:124`, `:126`; `app/core/models.py:476`. | não | Nenhum demonstrado. |
| `PingGeo.timestamp` | Instante do recebimento; janela com início inclusivo. (`app/api/views_core.py:908`; `:987`) | E=2: `app/api/views.py:2432`; `app/api/views_core.py:970`. | L=4: `app/api/views.py:2449`, `:2451`; `app/api/views_core.py:987`, `:989`. | não | Nenhum demonstrado. |
| `Colaborador.gps_concedido_em`, `.camera_concedida_em` — 2 | Instante de concessão; leitor testa presença. (`app/core/views.py:1083`; `:1086`; `:483`; `:485`) | E=1 por campo: `app/core/views.py:1083`, `:1086`. | L=1 por campo: `app/core/views.py:483`, `:485`. | não | Nenhum demonstrado. |
| `PeriodoAquisitivo.data_inicio` | Dia civil inicial. (`app/ferias/views.py:167`; `app/ferias/services.py:468`) | E=6: `app/ferias/services.py:260`, `:468`; `app/ferias/tests.py:47`, `:78`, `:253`, `:266`. | L=2: `app/ferias/views.py:204`; `app/ferias/models.py:193`. | não | Divergem os finais derivados, já apresentados; o início mantém a unidade. |
| `AgendamentoFerias.data_inicio`, `.data_fim` — 2 | Dates civis, ambos inclusivos; consulta de batidas converte para `[início 00h, fim+1 00h)`. (`app/ferias/models.py:362`; `app/ferias/situacao.py:279`; `:280`) | E=10 por campo: serviços `app/ferias/services.py:178`, `:280`, `:322`, `:379`, `:436`; cinco construções em `app/ferias/tests.py:98`, `:106`, `:113`, `:125`, `:135`, finais nas linhas seguintes. | L=5: `app/ferias/models.py:281`, `:362`, `:373`; `app/ferias/services.py:51`; `app/ferias/situacao.py:279`. | não | Nenhum demonstrado nas bordas examinadas. |
| `SolicitacaoFerias.data_inicio_desejada`, `.data_fim_desejada` — 2 | Dates desejados, copiados para agendamento e validados. (`app/ferias/services.py:436`; `:437`; `:443`) | E=2 testes por campo: `app/ferias/tests.py:151`, `:192`, finais nas linhas seguintes. Escritor de produção [não verificado]. | L=1 por campo: `app/ferias/services.py:436`, `:437`. | não na amostra | Escrita de produção não fechada. [não verificado] |
| `SolicitacaoFerias.criado_em` | Instante automático, ordenação cronológica. (`app/ferias/models.py:477`; `app/ferias/views.py:704`) | E=1 automático: declaração citada. | L=1: `app/ferias/views.py:704`. | não | Nenhum demonstrado. |
| `Afastamento.data_inicio`, `.data_retorno_real` — 2 | Datas civis; retorno encerra, duração exclui o dia do retorno. (`app/ferias/models.py:503`; `:508`; `:534`; `:535`) | Admin dos campos: `app/ferias/admin.py:12`; início em três testes `app/ferias/tests_disponibilidade.py:35`, `:44`, `:96`; retorno em `:45`. | L=2: `app/ferias/models.py:529`, `:534`; testemunha `app/ferias/tests_disponibilidade.py:47`. | não | Nenhum demonstrado; 20 até 5 dias atrás vale 15 dias no teste citado. |
| `AlertaInteligencia.criado_em` | Instante; deduplicação por dia local e ordenação. (`app/inteligencia/models.py:70`; `app/inteligencia/management/commands/gerar_alertas.py:27`) | E=1 automático. | L=2: `app/inteligencia/management/commands/gerar_alertas.py:27`; `app/inteligencia/views.py:131`. | não | Nenhum demonstrado. |
| `MetricaSnapshot.data_ini`, `.data_ref` — 2 | Dates da janela ou do dia da medição, conforme chave. (`app/inteligencia/metricas.py:51`; `app/folha/porta_export.py:773`; `:775`) | E=5 por campo: `app/inteligencia/metricas.py:51`; `app/inteligencia/management/commands/lavrar_fabrica.py:50`, `:59`; `app/inteligencia/management/commands/lavrar_jornada_lixo.py:62`; `app/folha/porta_export.py:773`. | L=3: `app/inteligencia/metricas.py:67`; `app/api/views_mensageria.py:612`; `app/folha/porta_export.py:808`. | não | Conferência do export exige dia atual **e** competência correspondente. (`app/folha/porta_export.py:808`; `:815`) |
| `LoteHolerite.avisado_em`, `.iniciado_em`; `EnvioHolerite.iniciado_em` — 3 | Instantes; aviso expõe ISO; inícios medem duração em segundos. (`app/holerite/views.py:1807`; `app/holerite/models.py:84`; `:285`) | E≥1 por campo: `app/holerite/views.py:1826`; `app/holerite/management/commands/processar_lote_holerite.py:66`; `app/holerite/management/commands/enviar_holerites_em_lote.py:53`. | L=1 por campo: sítios citados. | não na amostra | Outros escritores de início não integralmente abertos. [não verificado] |
| `Comunicado.inicio`, `.fim`, `.criado_em` — 3 | Vigência de instantes **[início, fim)**; criação ordena cronologicamente. (`app/comunicados/models.py:53`; `:55`; `app/comunicados/views.py:48`) | Limites de teste: `app/comunicados/tests.py:113`, `:114`; defaults/automático `app/comunicados/models.py:38`, `:41`. | L=2 para limites: `app/comunicados/models.py:53`; `app/comunicados/services.py:13`; L=1 criação: `app/comunicados/views.py:48`. | não | Fim exclusivo é coerente nos dois leitores examinados. |
| `Cliente.criado_em` | Instante automático, ordenado/exposto. (`app/tenants/models.py:17`; `app/tenants/views.py:27`; `:33`) | E=1 automático. | L=2: `app/tenants/views.py:27`, `:33`. | não | Nenhum demonstrado. |
| `Pauta.prazo`, `.lido_em`, `.feito_em`, `.cancelado_em`, `.criado_em` — 5 | Prazo civil inclusivo; demais são instantes de atos/criação. (`app/pautas/services.py:185`; `:357`; `:215`; `:248`; `:294`; `app/pautas/models.py:104`) | E: prazo=1 `:185`; lido=3 `:215`, `:248`, `:273`; feito=2 `:248`, `:273`; cancelado=1 `:294`; criado automático. Todos em `app/pautas/services.py`, salvo declaração citada. | Amostra: prazo `app/pautas/services.py:357`; presença dos carimbos `:213`, `:244`, `:292`; criação ordenada `:337`. | não | Prazo só atrasa quando `< localdate`, conservando o próprio dia. (`app/pautas/services.py:351`; `:357`) |
| `ExportacaoDominio.invalidada_em` | Instante da primeira invalidação; nulo significa não invalidado. (`app/folha/models.py:154`; `:158`; `app/folha/export.py:583`) | E=1: `app/folha/models.py:158`. | L=2: `app/folha/export.py:583`; apresentação local `app/folha/haiku_export.py:51`. | não | Nenhum demonstrado. |
| `RetratoFolha.lavrado_em` | Instante da lavra; frescor compara instantes; dia da lavra é local. (`app/folha/management/commands/lavrar_retrato_folha.py:76`; `app/folha/retrato_folha.py:1303`; `app/folha/vigia_retido_calado.py:39`) | E=1: comando citado. | L=4: `app/folha/retrato_folha.py:1297`, `:1303`, `:1325`; `app/folha/vigia_retido_calado.py:39`. | não | Nenhum demonstrado. |
| `LoteEspelho.data_ini`, `.data_fim`, `.iniciado_em` — 3 | Pedido conserva dates inclusivos; início é instante usado para carência. (`app/relatorios/lote_espelho.py:104`; `app/relatorios/pdf_espelho.py:602`; `app/relatorios/models.py:153`) | E=1 por limite `app/relatorios/lote_espelho.py:104`; E=2 início `:145`, `:160`. | Limites: L=3 `app/relatorios/lote_espelho.py:171`, `:172`, `:207`; início: L=1 `app/relatorios/models.py:153`. | não | Nenhum demonstrado. |

**Quadro — “não sei”, com as identidades preservadas**

| Campo — quantidade entre parênteses | Significados encontrados | Escritores abertos | Leitores: amostra lida | Divergente? | Caso que quebra / motivo |
|---|---|---|---|---|---|
| `Sindicato.cct_vigencia_inicio/fim`; `AplicacaoConvencao.criado_em`; `ParametroSistema.atualizado_em`; `LogConfiguracao.criado_em`; `LogAuditoria.criado_em`; `CredencialUsuario.carimbado_visto_em` **(7)** | Dates de vigência e datetimes declarados. (`app/core/models.py:38`, `:39`, `:158`, `:204`, `:226`, `:307`, `:273`) | Defaults/automáticos parcialmente identificados nas declarações; censo de atribuições [não verificado]. | Há filtros de auditoria por dia em `app/core/views.py:717`, `:722`; demais pares não fechados. | não sei | Cobertura de escritores/leitores insuficiente. [não verificado] |
| `PeriodoAquisitivo.criado_em/atualizado_em`; `AgendamentoFerias.confirmado_em/criado_em`; `SolicitacaoFerias.avaliado_em`; `Afastamento.data_retorno_estimada/criado_em/atualizado_em` **(8)** | Automáticos; confirmação/avaliação por `now`; retorno estimado é previsão civil. (`app/ferias/models.py:128`, `:129`, `:319`, `:504`, `:513`, `:514`; `app/ferias/services.py:418`, `:487`, `:500`) | Avaliação E=2 `app/ferias/services.py:418`, `:458`; confirmação E=2 `:487`, `:500`; atualizado também `:266`; demais declarações/admin. | Sem leitor temporal suficiente aberto para cada identidade. [não verificado] | não sei | Falta fechar os pares; não é prova de desacordo. [não verificado] |
| `ScoreColaborador.atualizado_em`; `AlertaInteligencia.lido_em`; `MetricaSnapshot.calculado_em` **(3)** | Carimbos de atualização/leitura/cálculo. (`app/inteligencia/models.py:21`, `:65`, `:96`) | Automáticos; duas escritas de leitura localizadas em `app/inteligencia/views.py:424`, `:437`, sem abertura integral dos corpos. | Pares insuficientes. [não verificado] | não sei | Cobertura incompleta. [não verificado] |
| `LoteHolerite.publicado_em/concluido_em`; `Holerite.publicado_em/visto_em/substituido_em/falha_em/ultimo_lembrete_em`; `EnvioHolerite.criado_em/concluido_em` **(9)** | Instantes declarados; CSV formata publicação/leitura crus. (`app/holerite/models.py:30`, `:60`, `:111`, `:136`, `:154`, `:168`, `:172`, `:262`, `:264`; `app/holerite/views.py:1092`, `:1094`) | Exemplos abertos: publicação `app/holerite/management/commands/processar_lote_holerite.py:130`; visto `app/holerite/views.py:1364`; falha `app/holerite/servicos.py:114`, `:121`, `:130`; conclusões `app/holerite/management/commands/enviar_holerites_em_lote.py:76`, `:81`. Totais [não verificado]. | CSV de publicação/visto aberto; faltou leitor local equivalente. Demais consumidores insuficientes. | não sei | Não atribuí ao CSV uma convenção esperada sem par equivalente aberto. [não verificado] |
| `GrupoComunicado.criado_em`; `LeituraComunicado.lido_em`; `TemplatePush.atualizado_em` **(3)** | Instantes automáticos. (`app/comunicados/models.py:12`, `:74`, `:89`) | Declarações citadas. | Leitores temporais não fechados. [não verificado] | não sei | Cobertura incompleta. [não verificado] |
| `ContadorPauta.lavrado_em` **(1)** | Instante automático e explícito da atualização. (`app/pautas/models.py:163`; `app/pautas/services.py:355`) | E=1 explícito, além do au

[truncado em 20000 de 39259 caracteres]

_2459 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
