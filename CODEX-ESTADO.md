# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 21:19** · publicado em **11/11/2026 21:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 21:19 · sessao 3c18a0449066 · PERGUNTA-DUPLA · construtor · origem=Code

Criado somente [test_pergunta_dupla_saida_noturna.py](/home/ronald/wt-codex-pergunta-dupla-211041/app/chamados/tests/test_pergunta_dupla_saida_noturna.py:1). O ensaio usa API real, relógio congelado por etapa, signals preservados e observador aditivo de nascimento; exige exatamente uma pergunta `orfao_14h`, não conversa, em D, incluindo linhas carimbadas na contagem (`chamados/tests/test_pergunta_dupla_saida_noturna.py:85,120,135,188,234`).

**Esperado no HEAD: VERDE.** Não encontrei produtor alcançável que entregue D+1 para essa saída ausente; não fabriquei contexto para obter RED. Execução Django e mordida do mutante: **[nao verificado]**, pois banco está vedado nesta construção. Sintaxe e compilação em memória passaram; isso não certifica comportamento (`chamados/tests/test_pergunta_dupla_saida_noturna.py:1`).

Censo dos escritores e da origem do dia:

| Escritor real | Valor e semântica |
|---|---|
| Detector `orfao_14h` | `data` vem de `resolver_data_evento` → juiz da batida: **D** (`chamados/detector_anomalias.py:41`; `escala/servico_jornada.py:508`). |
| Fábrica da célula | `contexto[motivo].data` recebe o dia da célula: **D** (`chamados/services/fabrica_da_celula.py:172`). |
| Pendentes adicionais | Consulta `dia_do_turno_da_batida`; grava contexto quando cria pergunta: **D** (`chamados/services/disputa_emissao.py:283,292,296`). |
| Alertas de turno | Grava `tm.data_turno` e timestamp da entrada; **D** vem do turno materializado (`ponto/management/commands/processar_alertas_turno.py:316,349`). |
| Alertas avançados | `ontem` é seleção pelo calendário, mas consulta trabalho, turno materializado e juiz da aberta antes de gravar: **D** neste cenário (`ponto/management/commands/processar_alertas_avancados.py:49,58,63,127`). |
| Detector de ausências/cartório | Grava o dia previsto da passada/célula e `horario_previsto`; não converte uma saída civil em dia do turno (`ponto/management/commands/detectar_ausencias.py:138,706`). |
| Retroativo de turno aberto | Grava `t.data_turno` e timestamp da entrada; **D**. É ato sem agenda de cron (`ponto/management/commands/emitir_turno_aberto_retroativo.py:88,128`; `config/crons.py:934`). |
| Reconciliador c2 | Copia o dia recebido do fio; não faz nova conversão pelo pareador. O chamador prioriza `p.data_evento` (`chamados/reconciliador.py:690,811,920,1567`). |
| Reconciliador de grade | Grava o dia da grade; cron está em sombra, sem `--apply` (`ponto/management/commands/reconciliar_grade.py:517`; `config/crons.py:462`). |
| Web: entrada bloqueada | `pendentes` contém dia **civil da entrada**, D para E às 19h. Exige outra tentativa de E bloqueada, ausente deste ensaio (`ponto/views.py:275,279,301`; `ponto/services/triagem_batida.py:180`). |

Os escritores civis encontrados não demonstram D+1 para a saída faltante:

- `data_batida` do detector descreve batida **existente** em folga ou fora da escala (`chamados/detector_anomalias.py:174,220`).
- A API grava `data_turno=localdate(batida.timestamp)` para desvio **leve**, categoria fora do mapa de regularização de saída (`api/views_core.py:710,717`; `chamados/models.py:3664`).
- `saida_sem_entrada` tem a data corrigida pelo juiz nas duas APIs e na triagem; cobra a entrada ausente e exige S existente (`api/views_core.py:741,747`; `api/views.py:747,753`; `ponto/services/triagem_batida.py:729,739`).
- Os contextos de justificativa copiam data informada ou `j.data_turno`; suas categorias não constam do mapa de saída (`api/views.py:1948,1968,1978`; `ponto/services/justificativa.py:297,302`; `chamados/models.py:3664`).

Quem escreve e quem lê os campos envolvidos:

| Campo | Escrita | Leitura |
|---|---|---|
| Pergunta: `motivo`, `conversa`, `data_evento` | `_nascer` recebe motivo/conversa; `fio.gravar_dia_do_fato` atribui a data; INSERT em `save` (`chamados/models.py:3014,3017`; `chamados/services/fio.py:800`). | Identidade/dedup, fábrica, fio e plantio (`chamados/models.py:2753`; `chamados/services/fabrica_da_celula.py:148`; `chamados/services/fio.py:1630`; `chamados/services/materializacao.py:280`). |
| `DisputaSupervisao.contexto[motivo].data` | Produtores acima; agregador persiste nascimento/merge (`chamados/services/disputa_emissao.py:559,562,590`). | Gerador, derivação do destino e reconciliador (`chamados/services/disputa_emissao.py:191,372`; `chamados/reconciliador.py:1560`). |
| `ChamadoColaborador.contexto_json`, cinco chaves | Produtores acima; atualização pela porta de contexto (`chamados/services/contexto.py:102`). | Parser, repasse sem origem e derivação do destino (`chamados/data_evento.py:16`; `chamados/services/disputa_emissao.py:1232,145`). |

Os dois sítios ainda preservam uma data recebida: leitura crua do contexto e parser sem origem; `dia_do_turno_da_celula` apenas declara o valor (`chamados/services/disputa_emissao.py:372,451,1232`; `ponto/turnos.py:2939`).

Comando para o Code:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_pergunta_dupla_saida_noturna --settings=config.settings.ci
```

Mutante descrito, sem aplicação: em `chamados/services/disputa_emissao.py:451`, trocar `_dtc_g(_dia_p)` por `_dtc_g(_dia_p + datetime.timedelta(days=1))`. Executar o mesmo comando; o repasse conserva D, enquanto o gerador entrega D+1. O assert deve reprovar as duas chaves; mordida executada **[nao verificado]** (`chamados/tests/test_pergunta_dupla_saida_noturna.py:33,240`).

Texto para o RELATO: **cura do +1 presente nesta cópia** (`escala/servico_jornada.py:508`); estado efetivamente “no ar” **[nao verificado]**. Atribuição histórica dos pares e situação atual da batida 147824 continuam **[nao verificado]**, no passivo BATIDA/histórico, L-099. Certificação permanece com Code e opus final.

## 11/11 21:07 · sessao 4be47f121636 · BUG-ORFAO-COM-PAR · construtor · origem=Code

Arquivo: [test_bug_orfao_com_par_ensaio.py:1](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:1). Classe candidata nomeada com `RED`; ensaio pela API real, relógio por passo e captura da pilha de nascimento ([arquivo:84](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:84), [arquivo:122](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:122)).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_bug_orfao_com_par_ensaio --settings=config.settings.ci
```

Expectativa estática no HEAD:

| ASSERT | Esperado | Por quê |
|---|---|---|
| A, após E | Zero perguntas nascidas, inclusive carimbadas | Turno ainda em curso: `escala/servico_jornada.py:385`. |
| B, 06:30:48 | Zero nascimentos após cartório e reconciliação | Fábrica exige marco vencido; S vence às 07:30: `chamados/services/fabrica_da_celula.py:69`, `ponto/services/cartorio.py:882`. |
| C, após S | Um turno fechado ancorado em D; zero nascimentos acumulados | Seleção pelo dia do turno: `ponto/turnos.py:1992`. |

**Execução comportamental no HEAD: [nao verificado].** Não executei Django porque esta obra proíbe banco. Não afirmo RED nem “classe não reproduz no HEAD”; essa conclusão depende da suite. [nao verificado]

**Mutante descrito, sem aplicação:** emitir `orfao_14h` imediatamente após E, dispensando vencimento, em `api/views_core.py:758`, alcançando `chamados/services/disputa_emissao.py:444`. Executar o mesmo comando na cópia mutante; ASSERT A deve falhar. Morte efetiva: [nao verificado]. Especificação em [arquivo:20](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:20).

| Campo lido | Quem escreve | Quem lê em produção |
|---|---|---|
| Pergunta: `motivo` | `chamados/models.py:3014`, INSERT `:3017` | `chamados/juizes.py:941` |
| Pergunta: `data_evento` | `chamados/services/fio.py:800` | `chamados/services/fabrica_da_celula.py:148` |
| Pergunta: `materializada_em` | Carimbo `chamados/models.py:3099`; limpeza `:3208`, `:3266` | C6 `chamados/juizes.py:1370`; E2 `chamados/reconciliador.py:629` |
| Batida: `timestamp`, `tipo` | `ponto/registro_batida.py:157` | `ponto/turnos.py:1217` |
| Célula: `ata` | Cartório `ponto/services/cartorio.py:1837` → porta `ponto/portas/celula.py:1033` | `chamados/juizes.py:965` |

Certificação permanece com o Code e o opus final. [nao verificado]

## 11/11 21:04 · sessao ae9a66b9c290 · DATA-EVENTO-CRU · censo · origem=Code

Numeração dos anexos: primeira linha do conteúdo = 1, incluindo linhas vazias e excluindo `== anexo ==`. Em **b**, considero a triagem da própria resposta; descarte posterior pela sessão é **[nao verificado]**.

| Achado | Letra | Trecho q | Trecho r | Motivo em 1 linha |
|---|---|---|---|---|
| **1 — templates sem ponto** (`opus_final.r:3`) | **a** | “o regex de template deixa escapar algum leitor” (`contrato_catraca.q:1`) | Reconhece `with` e aponta `attr` como escape (`preverif_catraca.r:9`, `:11`) | A cobertura dos escapes foi perguntada; `regroup` e `dictsort` ficaram sem identificação. |
| **2 — dict de filtro em variável** (`opus_final.r:4`) | **a** | “Algum leitor real NOVO ficou de fora por regra nova (chave de dict literal como rotulo...)?” (`preverif_catraca_v2.q:1`) | “não localizei”; “As chaves literais encontradas são payload/trilha” (`preverif_catraca_v2.r:14`) | A exclusão de chaves foi expressamente questionada; o dict usado como filtro não foi identificado. |
| **3 — isenção ampla de return** (`opus_final.r:5`) | **e** | A v3 informa “sobrecontagem admitida” do retorno e pergunta sua estabilidade (`preverif_catraca_v3.q:2`, `:6`) | “`return 'data_evento'`, continua contado”; depois bloqueia a compensação (`preverif_catraca_v3.r:5`, `:19`, `:25`) | A pergunta examinava a sobrecontagem; a isenção defeituosa pertence à cura posterior, registrada na [volta 4](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7968). |
| **4 — comentário multilinha** (`opus_final.r:6`) | **a** | “o regex de template deixa escapar algum leitor” (`contrato_catraca.q:1`) | Recomenda “retirar comentários antes de contar”, mencionando `{# ... #}` (`contrato_catraca.r:40`) | A revisão do regex estava no alcance; a resposta não distinguiu a semântica de comentário em uma linha da multilinha. |
| **5 — ESCRITA pelo nome da chamada** (`opus_final.r:7`) | **a** | “Algum leitor real NOVO ficou de fora por regra nova (... destino de escrita, DESTINO_KW)?” (`preverif_catraca_v2.q:1`) | “Leitor real novo omitido pelas exclusões novas: não localizei”; demonstra apenas a regressão do subscrito (`preverif_catraca_v2.r:14`) | As exclusões de escrita foram perguntadas; a isenção indistinta pelo nome do método não foi apontada. |
| **6 — total no ESTADO** (`opus_final.r:8`) | **e** | Pergunta “Quem escreve e quem le o JSON” e colisões com enumeradores (`preverif_catraca.q:7`) | “Publicação desse total no BACKLOG/ESTADO: **[nao verificado]**” (`preverif_catraca.r:36`) | Nenhuma q cobra explicitamente o produtor do total no ESTADO; a resposta menciona a publicação sem verificá-la. |
| **7 — código e JSON sobem juntos** (`opus_final.r:9`) | **b** | Pergunta se acrescentar leitor fica vermelho e se a catraca “so desce” (`preverif_catraca.q:3`) | “aumentar código **e** allowlist juntos pode passar; não há comparação com o baseline anterior” (`preverif_catraca.r:7`) | O Codex viu e tratou como limite, deixando-o fora da lista final de bloqueantes (`preverif_catraca.r:7`, `:40`). |

**Contagem da classificação acima:** **a = 4; b = 1; c = 0; d = 0; e = 2. Total = 7** (`opus_final.r:3–9`).

A promessa do achado 6 está em [RELATO.md:7950](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7950) nesta cópia; [RELATO.md:7966](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7966) registra a volta 3.

## 11/11 21:03 · sessao 8e89de2fb777 · - · - · origem=Ronald (tmux)

| Achado | Letra sugerida | Evidência |
|---|---|---|
| 1 | **a** | `contrato_catraca.q:1`: “o regex de template deixa escapar algum leitor”; `preverif_catraca.q:4`: “quais formas de ler data_evento o varredor nao ve”. A resposta enumera `with` e `attr`, sem `regroup`/`dictsort` (`preverif_catraca.r:9,11,20`). |
| 2 | **a** | `preverif_catraca_v2.q:1`: “Algum leitor real NOVO ficou de fora por regra nova (chave de dict literal como rotulo...)?”; resposta: “não localizei [...] As chaves literais encontradas são payload/trilha” (`preverif_catraca_v2.r:14`). A pergunta cobre a exclusão; a resposta não identifica o dict de filtro. |
| 3 | **e**, com distinção temporal | A última pergunta trata a **sobrecontagem** de `return 'data_evento'` (`preverif_catraca_v3.q:2,4,6`). O Codex encontra compensação e a bloqueia (`preverif_catraca_v3.r:19,27,29`). A isenção ampla de `return` do opus é defeito da cura posterior, sem pergunta de revisão correspondente nos anexos; a passagem v3→v4 registra “PROCEDE e curado” (`app/docs/RELATO.md:7968`). Não classificar **b**: o achado visto pelo Codex foi acolhido, não descartado (`app/docs/RELATO.md:7968`). |

Numeração considerada: primeira linha do conteúdo de cada anexo = 1; linhas vazias incluídas.

Para o achado 2, a resposta inicial já reconhecia escapes por `**` (`preverif_catraca.r:11,16`), mas não há trecho demonstrando descarte pela triagem do caso específico do dict literal guardado em variável **[nao verificado]**.

## 11/11 20:57 · sessao ef30d638d860 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Implementados mapa explícito, recusa `TipoSemFaixa` e contador por tipo ativo. A certificação pelo Code/suite e opus final permanece **[nao verificado]**. (`ponto/precedencia.py:52`, `:96`; `ponto/services/vigia_ausencia.py:155`)

Arquivos alterados:

- [precedencia.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/precedencia.py:48): comentário, mapa, exceção independente de `VetoDePrecedencia` e último recurso de `_nome_do_tipo` devolvendo o código cru.
- [vigia_ausencia.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/services/vigia_ausencia.py:155): `tipos_ativos_sem_faixa`, consultando `faixa_de` com `try/except TipoSemFaixa`; frases com códigos e URL da tela.
- [test_lastro_faixa_sem_default.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/tests/test_lastro_faixa_sem_default.py:11): testes novos, incluindo fixture pk 5356, fantasma, inativação, sementes, choices e consulta à autoridade. [nao verificado]
- [test_a6_vigia_familia_ausencia.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/ponto/tests/test_a6_vigia_familia_ausencia.py:124): expectativa de oito linhas.
- [test_calendario_ausencia_pela_autoridade.py](/home/ronald/wt-codex-lastro-faixa-sem-default-204847/app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:52): preservado o caso que dependia do default; agora exige a recusa nomeada.

Mapa final:

| Faixa | Códigos | Fonte |
|---|---|---|
| `ferias` | `ferias` | `ponto/precedencia.py:53` |
| `afastamento` | `afastamento_inss`, `suspensao`, `licenca_maternidade`, `licenca_paternidade`, `licenca_luto` | `ponto/precedencia.py:54` |
| `atestado` | `atestado`, `atestado_inss`, `abon_hr`, `abono`, `atraso`, `aviso_home_office`, `declaracao`, `declaracao_acompanhante_sem_ab`, `declaracao_de_acompanhante`, `desligamento`, `falta`, `reciclagem`, `reducao_de_7_dias_do_aviso`, `saida_antecipada`, `treinamento`, `atestado_medico`, `obito`, `outros` | `ponto/precedencia.py:59` |
| `folga` | `folga_compensatoria`, `feriado_folga`, `folga`, `folga_abonada`, `troca_de_plantao` | `ponto/precedencia.py:79` |

Cada entrada tem justificativa de uma linha. A conferência por grep e AST não encontrou faltantes entre os 17 choices e as 17 sementes. (`ponto/precedencia.py:52`; `ponto/models.py:661`; `ponto/migrations/0052_seed_tipoausencia.py:9`)

A execução Django parou por ausência de Django, antes dos testes (`manage.py:11`). RED/GREEN de integração, inclusive pk 5356, estão **[nao verificado]**. Em checagem isolada, com dependências simuladas, passaram três testes puros e o contrato do contador; ambos os mutantes foram mordidos. (`ponto/tests/test_lastro_faixa_sem_default.py:13`, `:19`, `:56`, `:132`)

Comando RED, para o HEAD/base informado **com somente o teste novo aplicado**; execução nesse HEAD **[nao verificado]**: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 CONGELAR_EM='2026-11-11 12:00:00' \
python3 manage.py test ponto.tests.test_lastro_faixa_sem_default \
--settings=config.settings.ci --noinput
```

Comando GREEN para o Code; resultado **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 CONGELAR_EM='2026-11-11 12:00:00' \
python3 manage.py test \
ponto.tests.test_lastro_faixa_sem_default \
ponto.tests.test_a6_vigia_familia_ausencia \
ponto.tests.test_a2_precedencia \
colaboradores.tests.test_calendario_ausencia_pela_autoridade \
--settings=config.settings.ci --noinput
```

Mutante **default de volta**, somente em memória:

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - <<'PY'
import django
django.setup()
from django.core.management import call_command
from ponto import precedencia as p
p.faixa_de = lambda tipo: p._FAIXA_DE_TIPO.get(tipo, 'atestado')
call_command('test', 'ponto.tests.test_lastro_faixa_sem_default', interactive=False)
PY
```

O teste do fantasma ficou vermelho na checagem isolada. O contador do fantasma também deve ficar vermelho na integração **[nao verificado]**. O caso pk 5356 continua verde nesse mutante porque `folga` permanece explicitamente mapeada. (`ponto/tests/test_lastro_faixa_sem_default.py:13`, `:92`, `:108`; `ponto/precedencia.py:81`)

Mutante **contador lê o dict**, somente em memória:

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - <<'PY'
import django
django.setup()
import inspect
from django.core.management import call_command
from ponto import precedencia as p
from ponto.services import vigia_ausencia as v
src = inspect.getsource(v.contadores)
src = src.replace('faixa_de(_t.codigo)', '_FAIXA_DE_TIPO[_t.codigo]')
src = src.replace('except TipoSemFaixa:', 'except KeyError:')
v.__dict__['_FAIXA_DE_TIPO'] = p._FAIXA_DE_TIPO
exec(compile(src, v.__file__, 'exec'), v.__dict__)
call_command('test', 'ponto.tests.test_lastro_faixa_sem_default.LastroFaixaSemDefaultTest.test_MORDE_contador_pergunta_a_autoridade_em_vez_do_dict', interactive=False)
PY
```

Esse mutante ficou vermelho na checagem isolada porque deixou de consultar `faixa_de('abono')`; integração **[nao verificado]**. (`ponto/tests/test_lastro_faixa_sem_default.py:132`)

Escritores e leitores dos campos envolvidos:

| Campo | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `TipoAusencia.codigo` | UI gera na criação; porta insere; seed inicializa. (`ponto/views_tipos_ausencia.py:89`; `ponto/services/ausencia.py:1310`; `ponto/migrations/0052_seed_tipoausencia.py:33`) | UI, catálogo, vigia e novo contador. (`ponto/views_tipos_ausencia.py:31`; `ponto/catalogo/ausencias.py:250`; `ponto/services/vigia_ausencia.py:144`, `:162`) |
| `TipoAusencia.ativo` | Checkbox da UI → porta; seed inicializa. (`ponto/views_tipos_ausencia.py:80`; `ponto/services/ausencia.py:1305`; `ponto/migrations/0052_seed_tipoausencia.py:36`) | Catálogo e novo contador. (`ponto/catalogo/ausencias.py:249`; `ponto/services/vigia_ausencia.py:160`) |
| `Ausencia.tipo` | Porta de criação/edição; fixtures dos testes. (`ponto/services/ausencia.py:342`, `:418`, `:498`; `ponto/tests/test_lastro_faixa_sem_default.py:96`) | Precedência histórica, nomeação e vigia existente. (`ponto/precedencia.py:128`, `:298`, `:406`; `ponto/services/vigia_ausencia.py:105`) |
| Faixa | Definição no mapa; resultado do dia montado pela precedência. (`ponto/precedencia.py:52`, `:429`) | `faixa_de` lê o mapa; fatos, veto e contador consultam essa autoridade. (`ponto/precedencia.py:103`, `:406`, `:522`, `:543`; `ponto/services/vigia_ausencia.py:162`) |

As escritas novas de cadastro/inativação são fixtures de teste. (`ponto/tests/test_lastro_faixa_sem_default.py:111`, `:119`)

Nos quatro leitores preservados, uma recusa durante a chamada capturada produz:

- **Espelho:** warning e continuidade com mapas vazios, podendo ficar sem palavras da janela. (`ponto/services/espelho.py:644`)
- **Cartório:** na aplicação posterior da precedência, registra a exceção e retorna `out`. (`ponto/services/cartorio.py:2032`)
- **Situação de férias:** devolve `False` para descanso. (`ferias/situacao.py:93`)
- **Semana do posto:** devolve `trabalho`. (`escala/services/semana_do_posto.py:175`)

## 11/11 20:49 · sessao 683fb9683207 · CASO-0711-HJ003 · sonda · origem=Code

**A 52340 já existia antes da criação da S. O dado também registra `celula_absolveu` antes da S. Portanto, ele não demonstra nem “emissor ignorou um par disponível” nem “pergunta continuou cobrando após o par”.** O nascimento precede o log de emissão; a cobrança vigente depende do juiz, não apenas da existência da linha. (`app/chamados/models.py:3017`, `app/chamados/models.py:3025`, `app/chamados/juizes.py:1428`.) Valores históricos transcritos da sonda, sem conferência independente: [nao verificado].

**1) Intervalo de nascimento sustentado**

| Limite | Evidência |
|---|---|
| **Superior: não depois do log exibido como 31/10/2026 19:00:41.** | Linha fornecida `LOG objeto=52340 modelo=PerguntaDisputa acao=pergunta_emitida ...`. Na cópia, o INSERT acontece em `app/chamados/models.py:3017`; o log, depois dele, em `app/chamados/models.py:3025`. O relógio do log é `auto_now_add`, `app/core/models.py:307`. Valor histórico: [nao verificado]. |
| **Inferior: não determinado pelo pacote.** | A E tem `criado_em=31/10 19:00:40`, mas falta provar que aquela execução causou o nascimento da pergunta. Coincidência de segundo e usuário não fornece essa causalidade. [nao verificado]. |
| **Limite superior adicional, mais fraco: já existia no processamento de 01/11 06:30:48.** | Campo fornecido `P52340.materializada_em`. É escrito separadamente do nascimento, em `app/chamados/models.py:3099`. Valor histórico: [nao verificado]. |

Assim, **nasceu antes de 01/11 07:00:33**, criação fornecida da 143189; o intervalo estreito **19:00:40–19:00:41 não está provado**, nem o instante exato do INSERT. Valores históricos: [nao verificado].

`data_evento=31/10` não fornece limite de nascimento: a porta grava ali o **dia do fato**, por `gravar_dia_do_fato`, não o relógio da inserção. (`app/chamados/models.py:3016`, `app/chamados/services/fio.py:790`, `app/chamados/services/fio.py:800`.)

O log identifica **usuário 415 no contexto da emissão**, porque `registrar_log(None, ...)` pode herdar o usuário da requisição. Ele não identifica exclusivamente API ou signal/cartório executado dentro dela. (`app/chamados/models.py:3025`, `app/core/auditoria.py:22`, `app/core/observ.py:108`, `app/core/observ.py:115`.) Rota e chamador históricos: [nao verificado].

Os registros `modelo=CelulaDia objeto=52340` e `modelo=CelulaDia objeto=7279` não demonstram julgamento da célula 133672; os totais de ações na janela também não individualizam a pergunta. Conteúdo histórico fornecido: [nao verificado].

**2) Quem cria e quem materializa**

Há duas cadeias relevantes na cópia; **a sonda não permite escolher qual delas emitiu historicamente a 52340**. [nao verificado].

| Cadeia | Caminho |
|---|---|
| **API do kernel** | `/api/ponto/bater/` aponta para `views_core` (`app/api/urls_core.py:5`, `app/api/urls_core.py:14`). Detecta anomalias **antes** da batida (`app/api/views_core.py:420`), grava pela porta (`app/api/views_core.py:645`) e depois chama a emissão (`app/api/views_core.py:758`). |
| **Signal → cartório → fábrica** | `post_save(Batida)` → julgamento (`app/ponto/signals.py:131`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`) → fábrica (`app/ponto/services/cartorio.py:1032`) → faltantes vencidos da ata (`app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`) → disputa e gerador (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`). |
| **Convergência do nascimento** | Gerador → `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:444`) → porta do fato (`app/chamados/models.py:2789`) → INSERT e log (`app/chamados/models.py:3017`, `app/chamados/models.py:3025`). |

Na fábrica, **`orfao_14h` também significa “marco final S faltante”**: a tradução é `('fim','S') → 'orfao_14h'`. O nome não prova que aquele emissor mediu quatorze horas. (`app/chamados/services/declaracao_texto.py:44`, `app/chamados/services/fabrica_da_celula.py:71`.)

Para a via observada, o caminho atual é:

`retratar_perguntas_absolvidas_por_celula` → `p.carimbar(VIA_CELULA_ABSOLVEU, usuário_sistema)` → atribuição do carimbo → consulta ao C6 → persistência. (`app/chamados/reconciliador.py:587`, `app/chamados/reconciliador.py:595`, `app/chamados/models.py:3129`, `app/chamados/models.py:3139`, `app/chamados/models.py:3152`.)

**`materializada_em` preenchida com resposta e validação nulas é compatível com absolvição automática sem plantio.** Esse chamador não pede assinatura de resposta/validação; a porta só as preenche quando recebe esse pedido e existe texto. (`app/chamados/reconciliador.py:595`, `app/chamados/models.py:3130`, `app/chamados/models.py:3134`.) Logo, o carimbo fornecido não prova batida criada nem resposta humana; atribuir a execução histórica exatamente a esse fluxo permanece [nao verificado].

**Todos os escritores atuais do carimbo de `PerguntaDisputa`, incluindo limpezas:**

| Escritor/porta | Operação |
|---|---|
| `_escrever_carimbo` | Atribui instante, autor e via juntos. `app/chamados/models.py:3099`, `app/chamados/models.py:3100`, `app/chamados/models.py:3101`. |
| `carimbar` | Usa o escritor acima e salva somente se C6 aceitar a pergunta como morta. `app/chamados/models.py:3129`, `app/chamados/models.py:3139`, `app/chamados/models.py:3152`. |
| `carimbar_identidade` | Usa o mesmo escritor e salva pela porta de identidade. `app/chamados/models.py:3183`, `app/chamados/models.py:3185`, `app/chamados/models.py:3190`. |
| `_desfazer_em_memoria` | Restaura o carimbo anterior quando o ato falha ou é recusado. `app/chamados/models.py:3162`. |
| `descarimbar` | Limpa os três campos; persistência opcional. `app/chamados/models.py:3208`, `app/chamados/models.py:3212`. |
| `descarimbar_em_lote` | Segundo sítio de escrita direta: limpa os três campos por UPDATE. `app/chamados/models.py:3264`, `app/chamados/models.py:3266`. |

Os chamadores produtivos dessas portas são:

| Operação | Chamadores |
|---|---|
| Carimbar | `app/chamados/models.py:3287`; `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:595`; `app/chamados/services/materializacao.py:566`, `app/chamados/services/materializacao.py:1282`, `app/chamados/services/materializacao.py:1436`; `app/chamados/services/disputa_emissao.py:1863`, `app/chamados/services/disputa_emissao.py:1937`. |
| Carimbar identidade | `app/chamados/models.py:2061`, `app/chamados/models.py:3064`; `app/chamados/services/fio.py:1359`; `app/chamados/services/materializacao.py:929`, `app/chamados/services/materializacao.py:1104`. |
| Descarimbar individualmente | `app/chamados/models.py:1506` — simulação sem salvar —, `app/chamados/models.py:3298`, `app/chamados/models.py:3315`, `app/chamados/models.py:3322`; `app/chamados/services/relavra_via.py:246`. |
| Descarimbar em lote | `app/chamados/models.py:1566`; `app/chamados/services/disputa_emissao.py:1630`. |

O selo declara esses dois escritores diretos e distingue o homônimo de **RegularizacaoExterna**, que não escreve a pergunta. Também declara limitações da análise estática; portanto, isto não certifica escritores externos ao código da cópia. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`.)

**3) Depois da S: leitores, absolvição e motivo de não reescrever**

A cadeia da S alcança **a data local e a véspera**; portanto, não existe nesse signal um recorte que necessariamente abandone 31/10. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:135`.)

| Leitor/caminho após a S | Decisão na cópia |
|---|---|
| **Cartório/grade** | Carrega universo apurável em janela ampliada e monta a grade. `app/ponto/services/cartorio.py:1142`, `app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`. Lavra ata e veredito: `app/ponto/services/cartorio.py:1837`, `app/ponto/services/cartorio.py:1839`. |
| **Fábrica: perguntas já carimbadas** | Pode coletar carimbadas e pedir retirada de carimbo sem lastro. `app/chamados/services/fabrica_da_celula.py:148`, `app/chamados/services/fabrica_da_celula.py:156`, `app/chamados/services/fabrica_da_celula.py:209`. |
| **Reconciliador síncrono da batida** | Roda depois do cartório e chama E2 antes do reatamento. `app/ponto/signals.py:206`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:1432`, `app/chamados/reconciliador.py:1433`. |
| **E2: célula absolveu** | Seleciona somente perguntas **sem materialização**, sem validação e com célula julgada não acusatória; exige C6 morto. `app/chamados/reconciliador.py:629`, `app/chamados/reconciliador.py:630`, `app/chamados/reconciliador.py:637`, `app/chamados/reconciliador.py:661`. |
| **C1: sem turno aberto** | Retorna imediatamente se já materializada. `app/chamados/reconciliador.py:366`. |
| **Reatamento de órfãs** | Exige destino nulo, disputa aberta e **materialização nula**. `app/chamados/reconciliador.py:1501`, `app/chamados/reconciliador.py:1503`, `app/chamados/reconciliador.py:1504`. |
| **Consumidor da lâmpada** | Chama E2 e encerramento de órfãs. `app/chamados/reconciliador.py:1365`, `app/chamados/reconciliador.py:1375`. O universo de órfãs também exige **materialização nula**. `app/chamados/reconciliador.py:1049`. |
| **Predicado da lâmpada** | Exige fato do marco provado, pergunta viva e encerramento temporal; então pode gravar fantasma e auditoria. `app/chamados/reconciliador.py:1084`, `app/chamados/reconciliador.py:1086`, `app/chamados/reconciliador.py:1090`, `app/chamados/reconciliador.py:1147`. |
| **Retirada de carimbo sem lastro** | Aceita materializadas fora das vias de identidade; só descarimba quando C6 diz **viva**. Carimbo com lastro permanece. `app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:242`, `app/chamados/services/relavra_via.py:246`. |

**Isso explica por que os tratadores que exigem materialização nula não voltariam a carimbar a ficha fornecida: ela já estava carimbada às 06:30:48. Não demonstra defeito desses tratadores.** Filtro: `app/chamados/reconciliador.py:629`; estado histórico: [nao verificado].

O decisor da cobrança é C6:

- `orfao_14h` consulta a lâmpada `S/hf`, lendo DNA e ata da célula. (`app/chamados/juizes.py:924`, `app/chamados/juizes.py:948`, `app/chamados/juizes.py:965`.)
- Com carimbo e lâmpada própria acesa, a pergunta fica morta **antes** de examinar outros furos do dia. (`app/chamados/juizes.py:1428`.)
- `celula_absolveu` não é identidade terminal; célula acusatória pode fazê-la continuar viva. (`app/chamados/models.py:2510`, `app/chamados/juizes.py:1434`.)
- O rótulo `estado` consulta C6 antes de chamar a pergunta materializada de resolvida. (`app/chamados/models.py:3044`, `app/chamados/models.py:3046`.)

Portanto, **“por que não absolveu quando a S chegou?” contém uma premissa não demonstrada**: existe registro de absolvição anterior; falta provar cobrança posterior. Ata, DNA, veredito da célula 133672, resultado de C6 e execução dos consumidores não aparecem no pacote. [nao verificado].

A disputa continuar aberta também não prova vitalidade dessa pergunta: C7 consulta o conjunto inteiro de perguntas e respostas sem veredito. (`app/chamados/models.py:1848`, `app/chamados/models.py:1853`.)

**4) Classe, RED e mutante**

**A classe causal da 52340 ainda não pode ser fechada pelo pacote.** Nascer antes da S elimina a explicação “ignorou aquela S já existente”, mas não prova sozinho cobrança prematura: falta confirmar qual turno/marco a emissão estava cobrando. [nao verificado].

Há uma razão adicional para não presumir essa identidade histórica: o próprio código documenta que `resolver_data_evento` anteriormente gravava o dia da saída esperada, D+1, enquanto hoje devolve o dia do turno. Isso não prova qual versão emitiu a 52340. (`app/escala/servico_jornada.py:498`, `app/escala/servico_jornada.py:501`, `app/escala/servico_jornada.py:509`.) Versão efetivamente executada em 31/10: [nao verificado].

**Classe a ensaiar:** emissão de cobrança sem premissa temporal vigente no nascimento, contrariando **juiz único/escritor único**, com identidade do turno mantida pela fonte única. Esses padrões já existem no desenho. (`app/docs/GLOSSARIO.md:11`, `app/docs/GLOSSARIO.md:12`.) Atribuição dessa classe à 52340: [nao verificado].

O contrato temporal existente é concreto: `classificar_falta` cala antes do fim quando encontra vínculo/tipo; a fábrica só cobra faltantes vencidos, pelo instante encadeado do marco +30 minutos. (`app/escala/servico_jornada.py:385`, `app/escala/servico_jornada.py:388`, `app/chamados/services/fabrica_da_celula.py:69`, `app/ponto/services/cartorio.py:882`.)

**Ensaio mínimo proposto, ainda não executado: [nao verificado].**

| Relógio, nesta ordem | Ação e observação exigidas |
|---|---|
| **D 19:00** | Célula/vínculo noturno 19→07; registrar **somente E** pelo endpoint do kernel, deixando os signals reais rodarem. Exigir zero nascimento de pergunta de saída para o fim ainda futuro. Entrada real: `app/api/urls_core.py:14`, `app/api/views_core.py:645`. |
| **D+1 06:30** | Executar cartório/reconciliação reais, ainda sem S. Exigir novamente zero nascimento indevido. Teto real: `app/ponto/services/cartorio.py:882`, `app/chamados/services/fabrica_da_celula.py:69`. |
| **D+1 07:00** | Registrar S pela mesma porta; deixar julgamento e reconciliação reais ocorrerem. `app/ponto/signals.py:135`, `app/ponto/signals.py:215`. |
| **Após S e após vencimento aplicável** | Exigir um turno fechado ancorado em D e nenhuma pergunta indevida **nascida em qualquer etapa**, inclusive uma que depois tenha sido absolvida. Pareamento/seleção: `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1995`. |

Se a reprodução depender do agregado anterior, acrescentar a disputa aberta e seu contexto **com evidência do conteúdo**, sem inventar uma pergunta anterior para obter vermelho. Contexto histórico necessário: [nao verificado].

**Mutante pertinente proposto:** depois de gravar E, inserir no caminho de emissão um `orfao_14h` para D apenas porque o turno abriu, dispensando a autoridade temporal. O ponto observado é a emissão posterior à gravação, `app/api/views_core.py:758`, chegando a `app/chamados/services/disputa_emissao.py:444`. A primeira asserção deve mordê-lo; execução e morte efetiva do mutante: [nao verificado].

O mutante anterior de recorte civil da S não testa esse nascimento anterior à S. E verificar apenas “nenhuma pergunta viva ao final” deixaria passar nascimento falso seguido de absolvição. Adequação ao contrato proposto: [nao verificado].

**Não há RED demonstrado nesta leitura**: há especificação do ensaio. Uma fixture 19→07 que já passe na cópia não pode ser apresentada como reprodução vermelha da 52340. [nao verificado].

**Escritores e leitores dos demais campos usados**

A tabela identifica os sítios relevantes a esta análise; o censo integral de escritores do carimbo está acima.

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Log: `acao`, `modelo`, `objeto_id`, `usuario`, `ator` | `app/core/observ.py:118`, `app/core/observ.py:123`, `app/core/observ.py:134`. | Leitura histórica deste pacote: sonda fornecida [nao verificado]. |
| Log: `criado_em` | Django, `auto_now_add`: `app/core/models.py:307`. | Histórico: `app/chamados/services/historico.py:90`, `app/chamados/services/historico.py:92`. |
| Pergunta: `motivo`, `disputa` | Nascimento: `app/chamados/models.py:3014`. | Marco: `app/chamados/juizes.py:941`; universo por disputa: `app/chamados/reconciliador.py:628`. |
| Pergunta: `data_evento` | `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016`. | `app/chamados/reconciliador.py:1088`, `app/chamados/services/fabrica_da_celula.py:148`. |
| Pergunta: `celula` | Nascimento: `app/chamados/models.py:3014`; rechaveamento: `app/chamados/services/fio.py:1038`. | `app/chamados/juizes.py:944`. |
| Pergunta: `chamado_destino` | Nascimento: `app/chamados/models.py:3014`; costura: `app/chamados/services/fio.py:925`. | `app/chamados/reconciliador.py:1501`. |
| Pergunta: `materializada_em`, `materializada_por` | Portas e chamadores enumerados acima. | C6: `app/chamados/juizes.py:1370`; elegibilidade: `app/chamados/reconciliador.py:629`; auditoria do autor: `app/chamados/reconciliador.py:1145`. |
| Pergunta: `via_resolucao` | Carimbo: `app/chamados/models.py:3101`; troca isolada de via: `app/chamados/models.py:3275`, `app/chamados/services/relavra_via.py:125`. | `app/chamados/juizes.py:1370`, `app/chamados/juizes.py:1422`. |
| Pergunta: `respondida_em`, `resposta_colab` | Resposta: `app/chamados/services/disputa_emissao.py:1467`; automática: `app/chamados/services/disputa_emissao.py:1652`; assinatura pelo carimbo: `app/chamados/models.py:3132`. | `app/chamados/juizes.py:1369`, `app/chamados/juizes.py:1401`. |
| Pergunta: `validada_em` | Validação: `app/chamados/services/validacao.py:134`; carimbo: `app/chamados/models.py:3135`; limpezas: `app/chamados/models.py:1566`, `app/chamados/services/disputa_emissao.py:1480`. | `app/chamados/juizes.py:1385`, `app/chamados/reconciliador.py:630`. |
| Batida: `timestamp`, `tipo` | `app/ponto/registro_batida.py:157`. | Ordenação: `app/ponto/turnos.py:1217`; datas do julgamento: `app/ponto/signals.py:111`; papel: `app/ponto/turnos.py:359`. |
| Batida: `criado_em` | Django, `auto_now_add`: `app/ponto/models.py:79`. | Guarda de chegada: `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362`. |
| Célula: `dna` | Geração: `app/ponto/portas/celula.py:260`; edição: `app/ponto/portas/celula.py:604`; regeneração: `app/ponto/portas/celula.py:875`. | `app/chamados/juizes.py:948`. |
| Célula: `ata`, lâmpadas, `veredito` | Montagem/lavra: `app/ponto/services/cartorio.py:1837`; escrita: `app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:1035`. | Lâmpadas: `app/chamados/juizes.py:965`; veredito: `app/chamados/juizes.py:1432`. |
| Disputa: `fechada_em` | Porta `fechar`: `app/chamados/models.py:1855`. | Elegibilidade: `app/chamados/reconciliador.py:1503`; fechamento condicionado: `app/chamados/reconciliador.py:611`. |

## 11/11 20:45 · sessao b601eef00d07 · - · - · origem=Ronald (tmux)

O nascimento está provado **antes da S**, mas o limite inferior estrito não está provado. O único emissor de log `pergunta_emitida` na cópia está em `app/chamados/models.py:3025`, executado **depois** do `p.save(force_insert=True)` em `app/chamados/models.py:3017`. O `criado_em` do log é `auto_now_add`, em `app/core/models.py:307`. Portanto, com o LOG fornecido, a inserção aconteceu **até 31/10/2026 19:00:41**, antes da criação da S em 01/11 07:00:33. Afirmar o intervalo estreito **[19:00:40,19:00:41]** exigiria provar que a pergunta nasceu especificamente da execução da E; a coincidência com o log da foto e o usuário 415 apenas corrobora essa hipótese. Limite inferior: **[nao verificado]**.

`data_evento=31/10` não serve de relógio de nascimento: é o dia do fato, gravado pela porta do turno em `app/chamados/models.py:3016`; a data do turno é resolvida em `app/chamados/models.py:2875`. `materializada_em` também não é nascimento: seu escritor é separado, em `app/chamados/models.py:3099`.

O log identifica uma **request autenticada no contexto do usuário 415**, pelo encadeamento `app/chamados/models.py:3025` → `app/core/auditoria.py:22` → `app/core/observ.py:108`–`118`, com contexto instalado em `app/core/middleware.py:78`. A rota HTTP específica e a distinção **emissão pelo signal/cartório versus emissão posterior pela API** continuam **[nao verificado]**: ambos podem herdar esse mesmo usuário. O dado não contém URL, descrição completa do evento, contexto da disputa nem callstack.

Cadeias existentes:

- API → detector: `app/api/views.py:434` (clone `app/api/views_core.py:420`) → `app/chamados/detector_anomalias.py:32` → universo apurável e pareador `app/ponto/utils_turno_orfao.py:28`, `:31`. API emite por `app/api/views.py:764` → `app/chamados/services/disputa_emissao.py:662`, `:670`/`:682` → `:444` → porta `app/chamados/models.py:2789` → nascimento `:3017` e log `:3025`.
- Signal de Batida → `app/ponto/signals.py:131`, `:135` → julgamento da véspera e dia local (`:111`–`:112`) → cartório `:93` → fábrica `app/ponto/services/cartorio.py:1032` → faltantes vencidos da ata `app/chamados/services/fabrica_da_celula.py:64`, `:69`, `:71` → emissão em `:172`, `:174` → mesma porta.
- Materialização pela absolvição da célula: `app/chamados/reconciliador.py:584` → usuário automático `:587` → **único chamador produtivo da via `celula_absolveu`** em `:595` → `PerguntaDisputa.carimbar` em `app/chamados/models.py:3103` → atribuição em `:3129`/`:3099` → C6 `:3139` → save `:3152`.

`materializada_em` com R/V nulas é compatível com uma **pergunta retirada da cobrança sem resposta e sem plantio**, pois o escritor da absolvição chama `carimbar` sem argumento `resposta` (`app/chamados/reconciliador.py:595`). A porta só assina resposta/validação quando há pedido explícito e texto (`app/chamados/models.py:3130`–`:3137`). Também não pede lavra de veredito, explicando a compatibilidade com lavra vazia; prova da execução histórica exata, além da via registrada: **[nao verificado]**.

Todos os escritores diretos atuais de `PerguntaDisputa.materializada_em`:

| Escritor direto | Operação |
|---|---|
| `app/chamados/models.py:3099` | `_escrever_carimbo`: instante, autor e via juntos. |
| `app/chamados/models.py:3264`–`:3266` | `descarimbar_em_lote`: limpa os três campos. |

Confirmei por AST em memória, reutilizando apenas as funções de análise do selo `app/chamados/tests/test_o261e_f2_porta_carimbo.py:65`–`:202`, sem importar a aplicação. A atribuição de `app/chamados/services/regularizacao_ext.py:198` é homônima de **RegularizacaoExterna**, não da pergunta; o próprio selo a distingue em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`–`:49`. Limites dessa análise estática estão declarados em `:111`–`:115`.

Todas as portas e chamadores produtivos que chegam a esses escritores:

| Porta/categoria | Chamadores |
|---|---|
| `carimbar` → atribuição `models.py:3129`, persistência `:3152` | `models.py:3287` (fantasma); `reconciliador.py:405` (sem turno aberto), `:595` (célula absolveu); `services/materializacao.py:566` (absorção por ausência), `:1282` (materializar uma), `:1436` (pendentes aprovadas); `services/disputa_emissao.py:1863` (destino resolvido), `:1937` (sem credor). Todos sob `app/chamados/`. |
| `carimbar_identidade` → atribuição `models.py:3183`/`:3185`, save `:3190` | `models.py:3064` (admin decidiu), `:2061` (motivo retratado); `services/fio.py:1359` (duplicata); `services/materializacao.py:929` (paridade recusada), `:1104` (colisão/fato ocupado). |
| Reversão em memória | `models.py:3162`, chamada ao desfazer carimbo recusado/falho em `:3141`, `:3144`, `:3155`. |
| `descarimbar` → `models.py:3208`, save opcional `:3212` | `models.py:1506` (**simulação, sem save**), `:3298` (reabrir fantasma), `:3315` (resposta nova), `:3322` (competência reaberta); `services/relavra_via.py:246` (carimbo sem lastro). |
| `descarimbar_em_lote` → `models.py:3266` | `models.py:1566` (ressuscitar perguntas); `services/disputa_emissao.py:1630` (reabrir para correção). |

O primeiro lugar que explica por que a absolvição não volta a tratar a pergunta depois da S é seu próprio universo: `app/chamados/reconciliador.py:629` exige `materializada_em IS NULL`. Com o carimbo de 06:30:48, a 52340 fica de fora. O leitor `lavrar_sem_turno_aberto` também retorna imediatamente quando o carimbo existe, em `app/chamados/reconciliador.py:366`–`:367`. Isso explica a seleção na cópia; o caminho efetivamente percorrido em produção depois da S continua **[nao verificado]**.

## 11/11 20:45 · sessao e7647ffdc603 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

O default alcança também leituras históricas; removê-lo exige tratar explicitamente os códigos antigos e os leitores que hoje capturam exceções. A origem é `app/ponto/precedencia.py:79`; a leitura histórica chega a ela em `app/ponto/precedencia.py:273–279` e `:381`.

Hash informado: `29e201d0ca2c9a6534ece9880e4a989a629f9987`. HEAD **[nao verificado]**: esta cópia não contém `.git`. Os números de produção abaixo vêm exclusivamente da sonda fornecida; sua reprodução no banco está **[nao verificado]**.

**1. Os 17 tipos ativos fora do mapa**

A regra escrita põe ausências humanas na faixa `atestado`, expressamente incluindo abono e falta; abre uma exceção para ausência que **é a própria folga**. Isso é classificação de precedência, não diagnóstico médico nem efeito financeiro. (`app/ponto/precedencia.py:48–63`)

Os campos não determinam uma faixa por si: `efeito` responde pagamento; `fim` responde duração; `medico` responde relatórios. O mapa, por exemplo, mantém `atestado_inss` em `atestado` e licenças em `afastamento`. (`app/ponto/catalogo/ausencias.py:9–12`, `:75–82`; `app/ponto/models.py:589–590`; `app/ponto/precedencia.py:54–59`)

O universo de 17 códigos e seus atributos é o da sonda fornecida **[nao verificado]**. Na tabela, “alternativa” indica uma interpretação que o material não permite resolver apenas pelo nome.

| Tipo | Faixa indicada e justificativa |
|---|---|
| `abon_hr` | **atestado**: o comentário cita esse código entre fatos humanos que vencem feriado/folga/jornada. (`app/ponto/precedencia.py:48–50`) |
| `abono` | **atestado**: abono é expressamente incluído nessa faixa; pagar não o transforma em férias ou folga. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:102`) |
| `atraso` | **atestado**, enquanto lançamento de ausência parcial: a regra geral cobre o fato humano; duração de um dia/parcialidade não define outra precedência. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:90`) |
| `aviso_home_office` | **atestado** pelo contrato atual de ausência abonada; **trabalho** é defensável se representar efetivo trabalho remoto. Decide a natureza do lançamento, ausente na sonda **[nao verificado]**. O código o chama de aviso “cumprido em home office” e o mantém como ausência abonada. (`app/ponto/models.py:678`; `app/ponto/catalogo/ausencias.py:104`; `app/ponto/precedencia.py:48–50`) |
| `declaracao` | **atestado**: comparecimento médico parcial, abonado, sem exceção própria no mapa. (`app/ponto/catalogo/ausencias.py:96`; `app/ponto/precedencia.py:48–50`) |
| `declaracao_acompanhante_sem_ab` | **atestado** pela regra do fato humano; descontar não significa `trabalho` nem identifica afastamento. O significado específico além dos atributos fornecidos está **[nao verificado]**. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:9–12`) |
| `declaracao_de_acompanhante` | **atestado** pela mesma regra; `medico` classifica relatórios, não cria precedência própria. (`app/ponto/precedencia.py:48–50`; `app/ponto/models.py:589–590`) |
| `desligamento` | **atestado** pela regra geral aplicada a uma `Ausencia`; **afastamento** é defensável se representar dispensa/suspensão de comparecimento. Se significa término do vínculo, enquadrá-lo numa dessas faixas pode sequer representar o fato correto. Qual desses sentidos foi cadastrado está **[nao verificado]**. (`app/ponto/precedencia.py:48–50`, `:54–57`) |
| `falta` | **atestado**: está explicitamente nomeada na regra geral, apesar de descontar. A faixa não deve virar `trabalho` por causa do desconto. (`app/ponto/precedencia.py:48–50`; `app/ponto/catalogo/ausencias.py:88`) |
| `folga` | **folga**: a exceção escrita diz que folga lançada como ausência “é a folga do dia”. (`app/ponto/precedencia.py:60–63`) |
| `folga_abonada` | **folga**, se o nome descreve descanso lançado; **atestado**, se “folga” foi usado para abonar ausência em jornada mantida. Decide descanso versus justificativa, não o atributo `abona`; o conteúdo dos lançamentos está **[nao verificado]**. (`app/ponto/precedencia.py:48–50`, `:60–63`; `app/ponto/catalogo/ausencias.py:10`) |
| `licenca_luto` | **atestado** pela regra geral; **afastamento** é defensável por analogia às outras licenças já mapeadas. O catálogo/form declaram licença paga de dois dias, mas não resolvem essa distinção de precedência. (`app/ponto/catalogo/ausencias.py:98`; `app/ponto/forms.py:20`; `app/ponto/precedencia.py:48–57`) |
| `reciclagem` | **atestado** enquanto ausência abonada; **trabalho** se for treinamento obrigatório efetivamente trabalhado. A sonda não informa obrigação, jornada ou realização **[nao verificado]**. A única regra de faixa aplicável explicitamente é a geral. (`app/ponto/precedencia.py:48–50`) |
| `reducao_de_7_dias_do_aviso` | **atestado** pela regra geral; **afastamento** é defensável se representar dispensa integral de comparecimento durante o aviso. A configuração fornecida não resolve essa escolha nem prova seu fundamento CLT **[nao verificado]**. (`app/ponto/precedencia.py:48–57`) |
| `saida_antecipada` | **atestado**, enquanto ausência parcial lançada: o catálogo a distingue por parcialidade/duração, sem faixa própria. (`app/ponto/catalogo/ausencias.py:91`; `app/ponto/precedencia.py:48–50`) |
| `treinamento` | **atestado** no contrato atual, que o cadastra como ausência abonada; **trabalho** se representar serviço efetivamente prestado. Decide a natureza real do evento, não “empresa paga”; isso está **[nao verificado]** na sonda. (`app/ponto/catalogo/ausencias.py:103`; `app/ponto/models.py:674`; `app/ponto/precedencia.py:48–50`) |
| `troca_de_plantao` | **folga no dia cedido / trabalho no dia assumido**, quando há troca confirmada: o corte exige um par e confirmação administrativa. Para uma linha isolada que seja apenas pedido, a equivalência a qualquer desses lados está **[nao verificado]**; tratá-la indistintamente como `atestado` perpetuaria a ambiguidade. (`app/docs/CORTES.md:187`) |

Não há, no material fornecido, fundamentação CLT suficiente para fechar juridicamente os sentidos ambíguos dos códigos personalizados. Essa fundamentação permanece **[nao verificado]**; os campos não autorizam inventar uma conversão automática para faixa. (`app/ponto/catalogo/ausencias.py:9–12`, `:75–82`; `app/ponto/models.py:589–590`)

**2. Histórico, chamadas e exceções**

A sonda informa `troca_de_posto` inativo sem usos e os órfãos `atestado_medico:4`, `obito:1`, `outros:1`; datas/status desses seis registros estão **[nao verificado]**.

**Sim, esses códigos são alcançáveis hoje.** `fatos_do_periodo` consulta diretamente `Ausencia`, sem cruzar cadastro ou atividade, excluindo somente rejeitadas; depois classifica cada ocorrência com `faixa_de`. Não existe limite a “dia atual”. (`app/ponto/precedencia.py:273–279`, `:336–339`, `:381`)

Censo das chamadas executáveis diretas:

| Sítio | Uso | Se `faixa_de` levantar |
|---|---|---|
| `app/ponto/precedencia.py:79` | Único leitor de `_FAIXA_DE_TIPO`; atualmente aplica o default. | É a origem proposta da recusa. |
| `app/ponto/precedencia.py:103` | Último recurso para nomear tipo, após tentar `get_tipo_display`. | Propaga: o `except` anterior não envolve esta linha. (`:96–103`) |
| `app/ponto/precedencia.py:381` | Classifica cada ausência da janela. | Interrompe a montagem da janela; não há captura local. (`:379–416`) |
| `app/ponto/precedencia.py:497` | Verifica se o lançamento atravessa folga humana. | Propaga antes de percorrer as folgas. (`:496–508`) |
| `app/ponto/precedencia.py:518` | Monta o texto de veto sobre folga humana. | Propaga; um desconhecido já recusado em `:497` não chega aqui. (`:514–518`) |

Não foram encontradas chamadas diretas adicionais em testes; o teste de abono chega indiretamente pela A2, e os testes de veto chegam pela porta pública. (`app/ponto/tests/test_a2_precedencia.py:395`, `:410`, `:420`) Cobertura de chamadas construídas dinamicamente está **[nao verificado]**.

Os leitores históricos afetados, com o tratamento local da exceção:

| Leitor | Resultado |
|---|---|
| `fatos_do_dia` | Delega e propaga. (`app/ponto/precedencia.py:256`) |
| Calendário | Loga e relança: pode falhar a leitura da janela histórica. (`app/colaboradores/services/calendario.py:507–519`) |
| Espelho | Loga e continua com mapas de fatos/decisões vazios: pode perder palavras da janela. (`app/ponto/services/espelho.py:644–651`) |
| Status do dia | Propaga na alimentação da lavra e nos ramos sem lavra. (`app/ponto/status_dia.py:181`, `:298`, `:304`) |
| Cartório | A aplicação propaga; `julgar_celula` captura e registra a falha da precedência. (`app/ponto/services/cartorio.py:1985`, `:2032–2039`) |
| Situação de férias | Captura e devolve `False` para descanso. (`app/ferias/situacao.py:93–99`) |
| Semana do posto | Captura e devolve `trabalho`. (`app/escala/services/semana_do_posto.py:175–179`) |
| Indicador de folha sem furo | Propaga ao consultar os fatos das datas com lâmpadas apagadas. (`app/folha/services/sem_furo.py:20–26`) |
| Dia do colaborador | Propaga. (`app/ponto/services/dia_do_colab.py:15`) |
| Deslogados / faltas de hoje / abriu e não bateu | Propagam localmente. (`app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/abriu_nao_bateu.py:123`) |
| Detector de ausências / reavaliação feriado-folga | Propagam localmente. (`app/ponto/management/commands/detectar_ausencias.py:616`; `app/chamados/services/feriado_reavaliar.py:33`) |

Chamadores do veto capturam **somente `VetoDePrecedencia`**: criação de ausência traduz para `AusenciaInvalida(codigo='sobreposicao')`; declaração devolve texto; férias traduzem para `ValidationError` ou lista de erros. As duas chamadas em folgas propagam. (`app/ponto/services/ausencia.py:379–383`; `app/ponto/services/declarar_dia.py:119–122`; `app/ferias/models.py:434–439`; `app/ferias/views.py:253–256`; `app/escala/folgas.py:310`, `:336`)

Portanto, uma exceção nova independente escapa desses tradutores; fazê-la herdar do veto sem adaptação a rotularia como **sobreposição** na criação de ausência. (`app/ponto/services/ausencia.py:382–383`)

Os demais testes com chamadas indiretas aos fatos/veto estão nestes sítios; uma exceção inesperada falha o teste, enquanto os testes de veto esperam a exceção específica: [nao verificado]

- `app/ponto/tests/test_bug107_justificar_falta.py:84`; `app/ponto/tests/test_o198_falta_em_dia_sem_jornada.py:104`; `app/ponto/tests/test_bo_atestado_range_absolve_tudo.py:133`.
- `app/ponto/tests/test_grade_folga_juiz_sitios.py:74`; `app/ponto/tests/test_precedencia_pela_celula.py:77`; `app/ponto/tests/test_precedencia_vinculo_encerrado.py:53`; `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:43`.
- `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:72`; `app/colaboradores/tests/test_calendario_um_juiz.py:118`; `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:64`.
- `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:139`; `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:55`.

Já existe teste que exige **warning + propagação** quando a A2 falha no calendário: `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:264–276`.

**3. Casa do contador e caso que morde**

A casa é **`vigia_ausencia.contadores`**, que já compara cadastro/código, entrega `catalogo_diverge_do_cadastro` e `tipos_fora_do_catalogo` e reúne os acesos. (`app/ponto/services/vigia_ausencia.py:50–59`, `:142–184`, `:207–212`)

A entrega semanal também existe: revisão → pauta → comando → cron. Não precisa nascer outro vigia. (`app/ponto/services/vigia_ausencia.py:227–242`; `app/ponto/management/commands/vigiar_familia_ausencia.py:23–29`; `app/config/crons.py:739–740`)

O placar existente publica “esperado 0” no mesmo domínio:

- ausência aprovada sem efeito na célula: `bin/placar_code.sh:413–416`;
- “nunca bateu” em dia coberto, explicitamente precedência: `bin/placar_code.sh:417–419`;
- ausência sem célula com vínculo: `bin/placar_code.sh:420`;
- ausência bloqueante sem fim, DP/cadastro: `bin/placar_code.sh:243–247`.

O molde é o teste que cria divergência real, exige `n=1` e nome do código, depois verifica acordo → zero. (`app/ponto/tests/test_a6_vigia_familia_ausencia.py:102–119`)

Para esta obra, o caso deve contar **tipo ativo sem faixa mesmo sem nenhuma Ausencia**; mapear ou inativar deve zerá-lo. A unidade é tipo, não lançamento. O molde também exige presença das linhas em zero e pauta somente para contador aceso. (`app/ponto/tests/test_a6_vigia_familia_ausencia.py:124–132`, `:178–193`) Esse novo caso e sua execução estão **[nao verificado]**.

O mutante “default voltou” precisa ser mordido por chamada real de `faixa_de(tipo_fantasma)` esperando recusa; apenas contar chaves ausentes do mapa não prova que o default morreu. A origem atual do comportamento é `app/ponto/precedencia.py:79`; RED/GREEN/mutante novos estão **[nao verificado]**.

**4. Escritores, leitores e descoberta pelo admin**

| Estado | Escritores e leitores comprovados |
|---|---|
| `TipoAusencia.codigo` | UI gera código somente na criação; porta faz o INSERT. Edição pela UI preserva o código. (`app/ponto/views_tipos_ausencia.py:83–94`; `app/ponto/services/ausencia.py:1301–1310`) |
| `TipoAusencia.ativo` | UI lê checkbox; porta aplica `campos` e salva. (`app/ponto/views_tipos_ausencia.py:67–80`; `app/ponto/services/ausencia.py:1305–1308`) |
| Leitores diretos do cadastro | UI lista todos e conta usos por código; catálogo seleciona ativos e chaveia por código; vigia carrega todos para comparação. (`app/ponto/views_tipos_ausencia.py:24–50`; `app/ponto/catalogo/ausencias.py:249–251`; `app/ponto/services/vigia_ausencia.py:143`) |
| Escrita inicial do cadastro | Seed cria código e `ativo=True`. (`app/ponto/migrations/0052_seed_tipoausencia.py:30–37`) |
| `Ausencia.tipo` | Criação monta `kwargs`, instancia e salva; edição atribui `ausencia.tipo` e salva. (`app/ponto/services/ausencia.py:341–345`, `:385`, `:418`, `:498–509`) |
| Férias | Espelhamento chama a mesma porta com `tipo='ferias'`. (`app/ponto/services/ausencia.py:1202–1207`) |

As portas canônicas das duas entidades estão registradas no mesmo serviço, com selo de escritores externos e allowlist vazia. (`app/core/portas.py:481–487`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:23–26`, `:53–57`)

Os chamadores de criação incluem UI, API, materialização de chamados, veredito da célula e checagem de ausência; regularização/abandono usam o wrapper `garantir_ausencia`. (`app/ponto/views.py:2415`, `:2536`; `app/api/views.py:1805`; `app/chamados/services/materializacao.py:490`, `:526`; `app/ponto/services/veredito_celula.py:336`; `app/ponto/services/checar_ausencia.py:46`; `app/ponto/services/ausencia.py:453–461`; `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`)

`Ausencia.tipo` é lido para validação e edição, efeito/documento, cobertura/range, precedência, grade, fechamento, exportação, vigia e resumo. (`app/ponto/services/ausencia.py:193–195`, `:474–475`, `:611–628`; `app/ponto/models.py:728–729`, `:840–842`; `app/ponto/turnos.py:2534`, `:2600`, `:2774`; `app/ponto/precedencia.py:381`; `app/escala/utils.py:1189`; `app/ponto/services/fechamento.py:693–701`; `app/folha/export.py:88–91`; `app/ponto/services/vigia_ausencia.py:104`; `app/inteligencia/resumo.py:43–44`) Censo fechado de todos os acessos dinâmicos e templates a esse campo: **[nao verificado]**.

**Hoje o admin não descobre falta de faixa no save.** A UI valida nome, efeito e duplicidade; a porta persiste sem perguntar à precedência. O signal apenas invalida cache. (`app/ponto/views_tipos_ausencia.py:61–95`; `app/ponto/services/ausencia.py:1301–1310`; `app/ponto/models.py:1235–1238`)

Se mudar **somente** `faixa_de`, o cadastro continuará salvando; a descoberta pode ocorrer já no primeiro lançamento, por `veto_de_lancamento:497`, ou na primeira leitura dos fatos. Não necessariamente será uma leitura visível ao admin, pois há capturas que continuam o processamento. (`app/ponto/services/ausencia.py:377–383`; `app/ponto/precedencia.py:497`; `app/ponto/services/espelho.py:645–651`; `app/ponto/services/cartorio.py:2032–2039`)

O encaixe para evitar isso é: **recusa nasce em `faixa_de`; a porta de cadastro consulta essa autoridade antes de criar/ativar, e a UI traduz a recusa nomeadamente**. Não copiar o mapa para a UI. A porta existente para isso é `gravar_tipo_ausencia`; essa validação ainda não existe. (`app/ponto/precedencia.py:76–79`; `app/ponto/services/ausencia.py:1301–1310`) Implementação desse encaixe: **[nao verificado]**.

Não colocar essa exigência como condição de registrar batida: a lei proíbe barrá-la, e o caminho existente grava antes de acionar fechamento, capturando falhas posteriores. O vigia precisa expor o problema mesmo quando esse processamento continua. (`CLAUDE.md:143–144`; `app/ponto/registro_batida.py:157–160`, `:170–183`)

A referência antiga do anexo a uma guarda em `models.py:884` **não corresponde à cópia**: nessa linha há o laço de derivação de minutos; o `save` lido não valida faixa. (`app/ponto/models.py:868–908`)

**5. Contraexemplos e dinheiro**

- **Rótulo depende do default hoje:** desconhecido vencedor recebe faixa `atestado`, e o resultado usa `ROTULOS[faixa]`, produzindo “Atestado”. (`app/ponto/precedencia.py:66–79`, `:403–405`)
- **`TRAVESSA_A_FOLGA` não libera folga:** contém somente `ferias`; tanto `atestado` quanto `folga` produzem `False`. A troca não autoriza atravessar outra folga humana. (`app/ponto/precedencia.py:496–518`)
- **A mudança não é apenas cosmética:** `folga` passa a perder para feriado; muda faixa/rótulo, status nos ramos sem lavra e seleção de cobrança espúria por folga. (`app/ponto/precedencia.py:46`, `:396–405`; `app/ponto/status_dia.py:308–316`; `app/chamados/services/feriado_reavaliar.py:33–39`)
- **Dinheiro usa outra pergunta:** nesse caminho, fechamento lê cobertura e `efeito`; `ABONA` justifica o dia e efeito diferente de `DESCONTA` suprime previsto. Há inclusive tratamento atual de efeito desconhecido `None`, independente do default de faixa. (`app/ponto/services/fechamento.py:675–701`)
- **Export lê o lavrado:** não recalcula a folha para emitir o TXT. (`app/folha/export.py:4–9`)
- **Exportada não é trava financeira geral:** o recálculo deixou de ser recusado por estar exportada; existe guarda específica para setembro/2026. O TXT anterior preserva conteúdo/hash ao ser invalidado. (`app/ponto/services/fechamento.py:485–498`, `:36–43`, `:558–566`; `app/folha/models.py:143–162`)

Portanto, trocar **somente a faixa** de `folga` não muda diretamente a regra de abono citada; afirmar dinheiro zero em toda competência exportada exigiria medição que a sonda não contém. Impacto monetário, competências atingidas e números de frota: **[nao verificado]**.

## 11/11 20:45 · sessao 9c60ffe32f88 · - · - · origem=Ronald (tmux)

**O RED anterior, com as duas batidas previamente cadastradas e relógio depois da S, não cobre o nascimento durante o turno aberto.** A fábrica decide vencimento por instante (`cartorio.py:882`) e a API detecta anomalias antes da escrita (`app/api/views.py:434`, `:654`); o teste deve conservar essa ordem.

Caminho real disponível na cópia:

1. API executa `detectar_anomalias(colab)` antes de registrar a batida: `app/api/views.py:434`.
2. Detector geométrico lê `batidas_apuraveis` e `parear_turnos`, selecionando turnos abertos: `app/ponto/utils_turno_orfao.py:28-35`; `app/chamados/detector_anomalias.py:31-36`.
3. Na E, API consulta turno aberto e `classificar_falta` ainda antes da escrita: `app/api/views.py:633-645`. Na S, faz a consulta correspondente: `:605-628`.
4. Registra a batida pela porta: `app/api/views.py:654-668`.
5. Depois da escrita, reaproveita os motivos detectados anteriormente e chama `criar_ou_atualizar_disputa`: `app/api/views.py:757-766`.
6. A porta chama o gerador tanto para disputa nova quanto existente: `app/chamados/services/disputa_emissao.py:669-682`.
7. Gerador traduz o motivo recebido em pergunta e chama `PerguntaDisputa.abrir`; não reaplica o teto do fim nessa chamada: `app/chamados/services/disputa_emissao.py:312-315`, `:444-452`.

**Isso não identifica o chamador histórico da pk52340:** API e signal/cartório podem executar no ato da E. `pergunta_emitida`, usuário415 e coincidência de segundo não trazem stack, origem nem contexto. Atribuição exclusiva à API ou ao signal: **[nao verificado]**.

Guardas existentes que um RED temporal precisa exercitar:

- `classificar_falta` cala antes do fim, **desde que encontre vínculo e tipo de escala**: `app/escala/servico_jornada.py:385-389`.
- Fábrica lê faltantes da ata e só permite os marcos vencidos: `app/chamados/services/fabrica_da_celula.py:64-71`.
- Vencimento é instante do marco encadeado +30min: `app/ponto/services/cartorio.py:879-883`.
- DNA/marcos/vínculo históricos da célula133672 e contexto da disputa no nascimento não estão no dado. Qual guarda falhou ou foi contornada para emitir às19:00:41: **[nao verificado]**.

**RED mínimo proposto, ainda não executado [nao verificado]:**

- Fixture com célula/vínculo noturno real 19→07; se necessário à reprodução, disputa aberta com pergunta anterior, conservando o agregado histórico.
- Relógio D19:00: registrar somente E pela API/porta real; deixar os signals reais rodarem. Assert: nenhum `orfao_14h` para o fim desse turno ainda futuro.
- Avançar para D+1 06:30: executar cartório/reconciliador reais. Assert: continua sem cobrança do fim ainda futuro.
- Avançar para D+1 07:00: registrar S pela mesma porta; deixar julgamento e reconciliação reais rodarem.
- Assert: um turno fechado ancorado em D, nenhum `orfao_14h` indevido nascido para esse turno. Não basta assertar ausência de pergunta **viva** ao final: isso aprovaria nascimento falso seguido de absolvição automática. Proposta **[nao verificado]**.

**Mutante pertinente proposto [nao verificado]:** permitir ao emissor criar `orfao_14h` a partir de um turno apenas aberto, sem obedecer ao teto do fim. A asserção imediatamente após E deve falhar. O mutante de “ignorar S em D+1 por recorte civil” do relatório anterior não explica a emissão comprovadamente anterior à S; sua participação neste caso é **[nao verificado]**.

Cuidado ao nomear a classe: **emissão prematura por desobediência ao juiz temporal** é hipótese testável; determinar o ramo histórico responsável continua **[nao verificado]**. “Não viu um par já disponível” é incompatível com nascimento às19:00 do dia anterior à criação da S, conforme dado fornecido **[nao verificado: sem arquivo:linha da sonda na cópia]**.

O teste existente `test_detector_noturno_s122` não cobre esse RED: seu helper replica `ultima.tipo` (`app/api/tests/test_detector_noturno_s122.py:20-25`), e o cenário19→07 testa esse helper local, sem endpoint, signals, cartório ou ordem temporal completa (`:27-33`).

## 11/11 20:45 · sessao aa7ce1705ee5 · - · - · origem=Ronald (tmux)

Achado principal: a premissa “não absolveu quando S chegou” não está demonstrada. A ficha já traz `via_resolucao=celula_absolveu`, carimbo às 06:30:48, anterior à S; isso é compatível com resolução automática já registrada, sem resposta nem validação. O escritor atual dessa via é `retratar_perguntas_absolvidas_por_celula → p.carimbar(...)`, em `app/chamados/reconciliador.py:595`; o significado da via está em `app/chamados/models.py:2475` e `:2536`. A atribuição histórica exata dessa execução permanece [nao verificado].

Caminhos após a S:

| Caminho/leitor | Evidência e consequência |
|---|---|
| Signal do cartório | `_cartorio_post_save_batida` chama `_julgar_dias`, em `app/ponto/signals.py:131-135`; datas são véspera e data local do timestamp, `:108-112`; cada uma é julgada em `:93`. Portanto S em 01/11 alcança também 31/10. |
| Universo do cartório | Carrega batidas apuráveis e monta grade em `app/ponto/services/cartorio.py:1158-1161`; janela vai de D−1 até D+3, `:1142-1143`, portanto não perde S por um recorte civil D. |
| Lavra de lâmpadas | Ata deriva da grade em `app/ponto/services/cartorio.py:1837`; é entregue a `cel.lavrar_veredito` em `:1839` e persistida pela porta em `:1859-1860`. Lâmpada `acesa` deriva de `missing` em `:671`, `:701-703`; ata monta `lampadas` em `:726-728`. |
| Fábrica pós julgamento | `app/ponto/services/cartorio.py:1030-1032` chama `perguntar_pela_celula`. Esta pode ler perguntas carimbadas e desfazer carimbo sem lastro em `app/chamados/services/fabrica_da_celula.py:207-209`. Coleta usa `:148-156`. |
| Reconciliador síncrono | Registrado depois do cartório, `app/ponto/signals.py:206-215`. Chama primeiro E2 e depois órfãs em `app/chamados/reconciliador.py:1432-1433`. |
| E2 / absolvição da célula | Universo exige `materializada_em IS NULL`, `validada_em IS NULL`, célula julgada não acusatória, `app/chamados/reconciliador.py:627-642`; depois exige C6 morto, `:661-662`, e dia não trancado, `:667-668`; escreve `celula_absolveu`, `:595`. **A ficha materializada fornecida já fica fora desse universo.** |
| Reatador de órfãs | `app/chamados/reconciliador.py:1500-1505` exige destino NULL, disputa aberta e materialização NULL. **52340 materializada também fica fora.** C1 possui guarda adicional explícita em `:366-367`. |
| Consumidor da lâmpada | `app/chamados/reconciliador.py:1365` chama E2; `:1375` chama `encerrar_orfas_pela_lampada`. Este percorre `_orfas_abertas` em `:1134`, cujo filtro exige materialização NULL em `:1049`. |
| Predicado da lâmpada | `veredito_orfa` exige ausência de resposta, fato do próprio marco provado, C6 vivo e encerramento temporal, `app/chamados/reconciliador.py:1082-1096`. Grava via fantasma e auditoria em `:1147-1155`. **Não reescreve uma pergunta já materializada.** |
| Eventual retirada de carimbo | `app/chamados/services/relavra_via.py:181-183` aceita materializadas fora das vias de identidade; relê travada e só descarimba se C6 a considerar viva, `:236-246`. Conserva carimbo com lastro, `:242-243`. |

O leitor que decide se a pergunta ainda cobra é C6. `celula_absolveu` não é uma via de identidade: a lista delas está em `app/chamados/models.py:2510`. Para pergunta materializada, a lâmpada própria acesa mata a pergunta antes do veredito geral, `app/chamados/juizes.py:1428-1430`; célula não acusatória também a mata quando não há pendentes ou quando o marco próprio já aconteceu, `:1445-1447`. Se a célula acusa e não existe prova do marco, pode continuar viva apesar do carimbo, `:1432-1435`. O rótulo `estado` respeita essa ordem: consulta C6 primeiro, `app/chamados/models.py:3044-3047`.

O vínculo do motivo com a lâmpada é `orfao_14h → ('S','hf')`, em `app/chamados/juizes.py:924`; `fato_do_marco_no_espelho` lê `motivo`, `celula`, `dna.marcos` e `ata.lampadas`, em `:941-968`; falta de prova retorna `None`, `:963-970`, não absolvição automática.

A sonda não fornece DNA, ata, veredito da célula 133672, resultado de `pergunta_viva`, eventos do consumidor nem código efetivamente implantado nos instantes históricos. Portanto “a S acendeu hf”, “a pergunta permaneceu viva”, “os sinais falharam” e “o carimbo das 06:30 estava correto/incorreto” são [nao verificado]. A mera permanência da linha 52340 não prova falha de retratação: estado resolvido conserva a linha, `app/chamados/models.py:3046-3047`.

Há ainda uma separação necessária: E2 fecha a disputa somente se `pode_fechar()` permitir, `app/chamados/reconciliador.py:610-614`; C7 exige nenhuma resposta sem veredito e nenhum fio vivo de **todas** as perguntas, `app/chamados/models.py:1848-1853`. Disputa 7279 continuar aberta não prova que 52340 continua viva.

## 11/11 20:43 · sessao 53af30c0ffc0 · - · - · origem=Ronald (tmux)

Censo somente leitura. Seguem os sítios comprovados; completude de caminhos dinâmicos não resolvíveis por busca estática: [nao verificado].

**TipoAusencia.codigo/ativo**

- Escritor runtime: `app/ponto/services/ausencia.py:1301-1310`; edição aplica `campos` com `setattr` e `save` (:1306-1308), criação escreve `codigo` (:1310). UI fornece `ativo` em `app/ponto/views_tipos_ausencia.py:80`, gera código em :89 e chama a porta em :86/:94. Código não é editável nessa UI (:84-85).
- Seed escreve ambos: `app/ponto/migrations/0052_seed_tipoausencia.py:30-37`; reversão lê os códigos e apaga os tipos (:40-42). `0062_fim_dos_17_tipos.py:47-54` e `0063_atestado_aceita_horas.py:20/:24` leem `codigo` para alterar outras flags.
- Leitores diretos runtime: lista/painel/duplicidade em `app/ponto/views_tipos_ausencia.py:31/:47/:50/:60/:92`; catálogo ativo em `app/ponto/catalogo/ausencias.py:249-251`; vigia lê todos e indexa código em `app/ponto/services/vigia_ausencia.py:143`.
- Save/delete apenas invalidam cache: `app/ponto/models.py:1235-1238`.
- Consumidores indiretos leem o catálogo: códigos válidos `app/ponto/catalogo/ausencias.py:284-285`; efeito :293-295; rótulo :298-300; choices :303-305; flags/conjuntos :308-358/:366-387/:455-467.
- Testes relevantes escrevem/leem cadastro em `app/ponto/tests/test_cadastro_tipo.py:14-34`, `test_porta_ausencia_ferias.py:144-152`, `test_smoke_clique_tipo_ausencia.py:114/:139/:148`.

**Ausencia.tipo — escritores e chamadores**

- INSERT runtime: `app/ponto/services/ausencia.py:341-345` monta os campos, :385 instancia, :418 salva. Alteração explícita de tipo: :498, persistência :509.
- Criadores externos pedem à porta: `app/ponto/views.py:2415/:2536`; `app/api/views.py:1805`; `app/chamados/services/materializacao.py:490/:526`; `app/ponto/services/veredito_celula.py:336`; `app/ponto/services/checar_ausencia.py:46`, acionado por `app/core/views_ia.py:985`.
- Wrapper idempotente `garantir_ausencia`: `app/ponto/services/ausencia.py:453-461`; consumidores `app/ponto/services/regularizacao.py:118` e `app/ponto/management/commands/marcar_abandono_provisorio.py:76`.
- Espelho de férias cria dentro da mesma porta: `app/ponto/services/ausencia.py:1202-1203`.
- Editores externos: `app/ponto/views.py:2794-2796` e `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`.
- Portas declaradas em `app/core/portas.py:481-487`; selo AST com allowlist vazia em `app/ponto/tests/test_chokepoint_ausencia_gate.py:23-26/:53-57`. Não executei o selo [nao verificado].
- Django admin é leitor: `app/ponto/admin.py:37-45`.
- Migrações de esquema: `app/ponto/migrations/0008_ausencia.py:21`, `0012_ausencia_dias_corridos_alter_ausencia_tipo.py:18-21`, `0039_alter_ausencia_tipo.py:13-16`, `0050_alter_ausencia_tipo.py:13-16`, `0070_alter_ausencia_tipo.py:19-22`. Não são remapeamento de valores históricos; 0070 declara apenas alargamento (:1-6).
- Migração de estado lê tipo: `app/ponto/migrations/0057_a4_backfill_pendente.py:37`. Não encontrei migração que reescreva os valores de `Ausencia.tipo`; caminhos externos à cópia [nao verificado].

**Ausencia.tipo — leitores runtime por grupo**

- Precedência: `app/ponto/precedencia.py:103/:127/:278/:339/:381/:408`.
- Cobertura, range, parcial e afastamento: `app/ponto/turnos.py:2534/:2558/:2600/:2677/:2715/:2721/:2762/:2774/:2835/:3139/:3161`; sinal calcula dias em `app/ponto/signals.py:124`.
- Modelo: estado documental `app/ponto/models.py:729`; efeito :842; falta/minutos :877; range aberto :902; choices/display :661-679/:749/:862.
- Serviço de ausência: busca/idempotência `app/ponto/services/ausencia.py:248/:255/:270/:298`; edição/auditoria :493/:496; reestado :543/:546/:1120; aprovação/efeito/documento :611/:621/:628; pauta/registro :649/:670/:675/:1060; férias :1183/:1266/:1281.
- Cálculo/folha: `app/ponto/motor_calculo_v2.py:1102`; `app/ponto/services/fechamento.py:694`; `app/ponto/services/credito_parcial.py:27`; `app/ponto/services/efeito_ausencia.py:48/:55`; `app/folha/export.py:70/:88/:91/:118`.
- Apresentação do dia: `app/ponto/services/dia_decidido.py:422/:481/:496/:521/:614`; calendário `app/colaboradores/services/calendario.py:674-677`; espelho/grade `app/escala/utils.py:1189`.
- UI/API: `app/ponto/views.py:2186/:2198-2201/:2212-2222/:2243/:2349/:2358/:2528/:2614/:2649/:2653/:2707/:2711/:2732/:2740/:2770/:2796/:2809/:2811/:2840/:2876-2880`; `app/api/views.py:1858/:3081/:3101`; `app/api/views_mensageria.py:742/:745/:759/:761/:1033/:1070-1072`; cadastro conta usos em `app/ponto/views_tipos_ausencia.py:24`.
- Relatórios: `app/relatorios/views.py:198/:205/:209/:213/:228/:282/:292/:302/:304`; lote documental `app/ponto/services/atestados_lote.py:60/:64/:98/:128`; alerta médico `app/ponto/services/alertas_ausencia.py:50`.
- Chamados: `app/chamados/juizes.py:244`; `app/chamados/services/materializacao.py:82/:625/:679`; `app/chamados/services/historico.py:132`; `app/chamados/services/silencio_ausencia.py:85/:90-91/:105/:109-110`; `app/chamados/services/adesao.py:69`; cartório `app/ponto/services/cartorio.py:794`.
- Cobrança de furos: `app/escala/services/furos_diarios.py:223-224`; `app/ponto/management/commands/reconciliar_grade.py:402`.
- Aviso de batida: `app/ponto/services/aviso_da_batida.py:89/:92`.
- Vigias e correções: `app/ponto/services/vigia_ausencia.py:84/:92/:95/:104-105/:116/:122/:134-137/:216/:219`; `vigia_ausencia_veredito.py:56`; comandos `ausencia_bloqueante_sem_fim.py:28/:39`, `ausencias_sem_efeito_na_celula.py:49`, `curar_ausencia_sem_fim.py:31/:36`, `fechar_chamados_ausencia.py:34`, `reabrir_chamados_ausencia_aberta.py:43`, `rejulgar_status_por_tipo.py:24/:29/:33`, `rejulgar_ausencia_em_folga.py:57`, `rederivar_minutos_h06_comp11.py:121/:142/:146/:169`.
- Férias: `app/ferias/models.py:145/:431`; `app/ferias/views.py:886`.
- Inteligência: `app/inteligencia/management/commands/calcular_scores.py:14`, `gerar_alertas.py:58`; `app/inteligencia/resumo.py:43-44`; `app/inteligencia/views.py:127`.
- Operação de sombra inclui a coluna no lote incremental: `bin/sombra_incremental.sh:36`.

**Descoberta pelo admin e posição da recusa**

Hoje salvar TipoAusencia não consulta faixa: valida nome/efeito/código e chama a porta em `app/ponto/views_tipos_ausencia.py:61-95`, que persiste sem consultar precedência em `app/ponto/services/ausencia.py:1301-1310`. A classe TipoAusencia não sobrescreve `save`: `app/ponto/models.py:562-625`. Portanto, trocar **apenas** `faixa_de` por recusa não faz o admin descobrir no save; descoberta fica dependente de uma chamada posterior da precedência ou do vigia. Qual seria a primeira chamada real em produção [nao verificado].

A autoridade deve continuar `faixa_de`; para descobrir no cadastro, a porta `gravar_tipo_ausencia` precisa consultar essa autoridade antes de persistir criação/ativação e a UI traduzir a recusa. Isso é recomendação de desenho, não comportamento já implementado [nao verificado]. Criação/edição de Ausencia também precisam tradução explícita: hoje validam catálogo vivo em `app/ponto/services/ausencia.py:193-195/:473-475`, e o veto captura somente `VetoDePrecedencia` (:377-383).

A cópia **não contém a guarda de tipo em Ausencia.save citada pelo anexo**: :884 é o laço de minutos; o método visível termina em `super().save` em `app/ponto/models.py:868-908`.

**Batida e exceções**

- Lei: `CLAUDE.md:143-144`.
- Gravação ocorre em `app/ponto/registro_batida.py:157-158`; fechamento posterior :160 é protegido por catch amplo com warning/contexto em :175-183.
- Signal de batida chama julgamento em `app/ponto/signals.py:131-135`; o julgamento está dentro de `engolir` (:85-93).
- `engolir` captura, incrementa contador e registra ERROR com contexto/causa: `app/core/observ.py:73-91`.
- Logo a recusa de faixa não deve ser introduzida como veto em `registrar_batida`; deve permanecer na autoridade e ser apresentada nas portas de cadastro/lançamento, com exposição pelo vigia existente. Entrega desse erro ao admin por esses catches, sem alteração adicional: [nao verificado].

## 11/11 20:43 · sessao 7015638e31e9 · - · - · origem=Ronald (tmux)

Censo direto completo: `_FAIXA_DE_TIPO` só é lido por `faixa_de`, no `.get(tipo, 'atestado')` de `app/ponto/precedencia.py:79`. `faixa_de` possui exatamente quatro chamadas executáveis, todas no mesmo módulo; a busca global e a leitura por AST, incluindo aliases de importação, não encontraram chamadas diretas em testes.

| Chamada | Papel | Exceção de `faixa_de` |
|---|---|---|
| `app/ponto/precedencia.py:103` | Último recurso de `_nome_do_tipo`, após tentar `get_tipo_display()` | Propaga; o `except Exception` de `:100` envolve somente a tentativa anterior de display |
| `app/ponto/precedencia.py:381` | Classifica cada ausência encontrada na janela | Propaga, interrompendo a construção de todo o resultado |
| `app/ponto/precedencia.py:497` | Decide se o novo lançamento atravessa folga humana | Propaga antes de carregar/percorrrer as folgas |
| `app/ponto/precedencia.py:518` | Nomeia a faixa do lançamento recusado sobre folga humana | Propaga; com recusa de desconhecido em `:497`, esse mesmo desconhecido nem chega aqui |

A consulta histórica não lê `TipoAusencia.ativo`, nem exige cadastro: busca diretamente `Ausencia`, exclui apenas `status='rejeitada'` e seleciona interseção da janela, inclusive períodos abertos (`app/ponto/precedencia.py:273`). Depois distribui as linhas pelos dias e chama `faixa_de` (`app/ponto/precedencia.py:336`, `app/ponto/precedencia.py:381`). Assim, um código inativo ou sem cadastro pode chegar à classificação em leitura passada. A sonda mostra zero ocorrências para `troca_de_posto` e seis ocorrências órfãs, mas não mostra datas/status desses órfãos; quais leituras concretas os encontrariam permanece **[nao verificado]**.

Chamadores indiretos de produção e comportamento local:

| Leitor de fatos | Comportamento perante exceção |
|---|---|
| `app/ponto/precedencia.py:256` | `fatos_do_dia` delega a `fatos_do_periodo`; propaga |
| `app/ponto/status_dia.py:181` | Alimentação da lavra para veredito indefinido; propaga |
| `app/ponto/status_dia.py:298`, `:304` | Natureza do dia sem lavra/batida; propaga |
| `app/colaboradores/services/calendario.py:507` | Loga e **relança**, em `:515` |
| `app/ponto/services/espelho.py:648` | Captura qualquer `Exception`, loga e continua sem os fatos/palavras da janela, em `:650` |
| `app/ferias/situacao.py:94` | Captura qualquer `Exception` e devolve `False` para descanso, em `:95` |
| `app/escala/services/semana_do_posto.py:177` | Captura qualquer `Exception` e devolve `'trabalho'`, em `:178` |
| `app/folha/services/sem_furo.py:25` | Propaga; lê fatos para filtrar dias com lâmpadas apagadas |
| `app/ponto/services/cartorio.py:1985` | `aplicar_precedencia_na_celula` propaga; `julgar_celula` captura/loga em `:2033` |
| `app/ponto/services/dia_do_colab.py:15` | Propaga |
| `app/ponto/services/deslogados.py:166` | Propaga |
| `app/ponto/services/faltas_de_hoje.py:61` | Propaga |
| `app/ponto/services/abriu_nao_bateu.py:123` | Propaga |
| `app/ponto/management/commands/detectar_ausencias.py:616` | Propaga |
| `app/chamados/services/feriado_reavaliar.py:33` | Propaga |

Chamadores de `veto_de_lancamento`:

- `app/ponto/services/ausencia.py:380`: captura **somente** `VetoDePrecedencia` e traduz para `AusenciaInvalida(codigo='sobreposicao')`, em `:382`.
- `app/ponto/services/declarar_dia.py:120`: captura **somente** `VetoDePrecedencia` e retorna seu texto, em `:121`.
- `app/ferias/models.py:435`: captura **somente** `VetoDePrecedencia` e traduz para `ValidationError`, em `:438`.
- `app/ferias/views.py:254`: captura **somente** `VetoDePrecedencia` e acrescenta texto à lista de erros, em `:255`.
- `app/escala/folgas.py:310`, `:336`: sem captura local.

Consequência: uma nova exceção que não herde de `VetoDePrecedencia` escapa desses tradutores; se herdar, será rotulada como sobreposição no serviço de ausência, o que também merece tratamento explícito (`app/ponto/services/ausencia.py:382`).

A proteção da batida já existe: o signal de `Batida` chama `_julgar_dias` (`app/ponto/signals.py:131`), cuja chamada de `julgar_celula` está dentro de `engolir` (`app/ponto/signals.py:85`). A aplicação da precedência tem ainda captura própria no cartório (`app/ponto/services/cartorio.py:2032`). O fechamento por evento agenda `on_commit` e captura falhas tanto no processamento quanto no agendamento (`app/ponto/services/fechamento.py:1798`, `:1818`, `:1823`).

Contraexemplos comprovados:

- Hoje a A2 devolve rótulo da **faixa**, portanto desconhecido vencedor gera `Atestado` (`app/ponto/precedencia.py:79`, `:405`). `_nome_do_tipo` normalmente usa o display do tipo e só consulta a faixa como último recurso (`:96`, `:103`).
- `TRAVESSA_A_FOLGA` contém exclusivamente `'ferias'`; trocar a classificação de `folga` de `atestado` para `folga` mantém o resultado desse teste em `False` (`app/ponto/precedencia.py:496`).
- A troca altera ordenação contra feriado, faixa e rótulo do resultado (`app/ponto/precedencia.py:396`, `:405`); também muda a natureza de ausência para folga no leitor sem lavra (`app/ponto/status_dia.py:308`) e passa a participar da seleção de cobranças espúrias de feriado/folga (`app/chamados/services/feriado_reavaliar.py:36`).
- Não há chamada de `faixa_de`, do mapa ou dos fatos em `motor_calculo_v2.py`, `services/fechamento.py` ou `folha/export.py`; a exportação lê fechamento já lavrado e não recalcula (`app/folha/export.py:1`).
- **Não afirmar “competência exportada nunca recalcula”**: a cópia removeu essa recusa geral; identifica as exportadas e registra o recálculo (`app/ponto/services/fechamento.py:539`). Há uma guarda específica para **09/2026** (`:36`), que exclui do universo de escrita os colaboradores de empresas exportadas (`:560`). O TXT entregue e o gravado atual podem divergir, e há comparação explícita entre eles (`app/folha/export.py:631`, `:655`). Efeito monetário desta mudança na frota é **[nao verificado]**.

Testes indiretos relevantes, sem chamadas diretas de `faixa_de`/mapa: [nao verificado]

- Ordem e supressão: `app/ponto/tests/test_a2_precedencia.py:120`, `:127`, `:134`, `:139`, `:150`.
- Recusas humanas e exceções esperadas: `app/ponto/tests/test_a2_precedencia.py:410`, `:420`, `:427`, `:433`, `:440`; férias atravessando folga: `:747`.
- Abono fora do mapa hoje classificado pela A2: `app/ponto/tests/test_a2_precedencia.py:395`.
- Falha da A2 deve produzir log e propagar no calendário: `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:264`.
- Demais arquivos com chamadas indiretas: `app/ponto/tests/test_bug107_justificar_falta.py:84`; `test_o198_falta_em_dia_sem_jornada.py:104`; `test_bo_atestado_range_absolve_tudo.py:133`; `test_grade_folga_juiz_sitios.py:74`; `test_precedencia_pela_celula.py:77`; `test_precedencia_vinculo_encerrado.py:53`; `test_precedencia_folga_de_calendario_e_do_vinculo.py:43`; `test_e4_1_cron_le_o_juiz_nao_a_lavra.py:72`; `app/colaboradores/tests/test_calendario_um_juiz.py:118`; `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:64`; `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:139`; `test_jv1_volta4_posto_do_dia.py:55`.

## 11/11 20:42 · sessao 294466f1e75a · - · - · origem=Ronald (tmux)

Casa existente para o contador: `app/ponto/services/vigia_ausencia.py::contadores`, leitor puro declarado em `:50-51`. O vigia já compara cadastro e catálogo em `:142-152` e entrega as linhas `catalogo_diverge_do_cadastro` (`:167-173`) e `tipos_fora_do_catalogo` (`:180-184`). Zero não acende: `:211-212`. É o endereço mais próximo para `tipos_ativos_sem_faixa`, contando **tipos ativos**, sem exigir uso em Ausencia; essa última parte é proposta de encaixe, não comportamento atual.

A entrega já existe: `revisao_semanal` passa essas mesmas linhas ao escritor de pauta em `app/ponto/services/vigia_ausencia.py:227-242`; comando `app/ponto/management/commands/vigiar_familia_ausencia.py:23-29`; cron semanal já declarado em `app/config/crons.py:739-740`. Não precisa nascer vigia/cron novo.

O placar existente já possui “esperado 0” do mesmo domínio:

- `ausencias_aprovadas_sem_efeito_na_celula`: `bin/placar_code.sh:413-416`.
- `nunca_bateu_em_dia_coberto`, explicitamente de precedência: `bin/placar_code.sh:417-419`.
- `ausencia_sem_celula_com_vinculo`: `bin/placar_code.sh:420`.
- `ausencia_bloqueante_sem_fim`, de DP/cadastro: `bin/placar_code.sh:243-247`.

O comando de ausência/ata está declarado como morador do placar, sem cron novo, em `app/config/crons.py:1194-1196`. O A6 usa mecanismo zero/não acende, mas não contém literalmente “esperado 0” (`app/ponto/services/vigia_ausencia.py:9-12`).

Molde que morde: `app/ponto/tests/test_a6_vigia_familia_ausencia.py:102-112` cria divergência real cadastro/código e exige `n=1` com o código na mensagem; `:114-119` confere acordo→0. O estoque limpo exige total zero e presença de todas as linhas em `:124-132`; pauta só com a linha acesa e teto semanal em `:178-193`. Para o novo contador, o caso correspondente é tipo ativo fora do mapa→1, mesmo sem Ausencia; mapear/inativar→0, e restaurar default não pode tornar o contador verde — desenho proposto, execução [nao verificado].

Sobre dinheiro/exportada:

- Fechamento lê cobertura e **efeito**, sem consultar faixa nesse trecho: `app/ponto/services/fechamento.py:675-701`. `ABONA` entra em justificadas; efeito diferente de `DESCONTA` suprime previsto (`:698-701`), inclusive código desconhecido com efeito `None`, conforme comentário `:678-679`.
- A cobertura desempata por data/pk, não por faixa: `app/ponto/turnos.py:2735-2754`, corpo `:2765-2783`.
- O coletor comum de efeitos também usa efeito/cobertura: `app/ponto/services/efeito_ausencia.py:53-62`.
- O export traduz fechamento já lavrado e não recalcula: `app/folha/export.py:4-9`.
- Exportada **não tem trava global de recálculo**: `app/ponto/services/fechamento.py:485-498`, implementação da trilha `:539-547`. Existe guarda específica de setembro/2026 em `:19-43`, aplicada em `:558-566`.
- O TXT anterior preserva conteúdo/hash ao invalidar: `app/folha/models.py:143-162`.

Assim, trocar apenas a faixa de `folga` não altera diretamente o algoritmo de abono mostrado acima; impacto financeiro total ou indireto não foi medido e fica **[nao verificado]**. A sonda não informa competências nem valores financeiros.

## 11/11 20:35 · sessao 3e51f6e3629d · CASO-0711-HJ003 · sonda · origem=Code

**A regra atual é “disputa aberta mais recente do colaborador”, sem filtro pelo dia da pergunta.** Isso permite que uma disputa aberta em um dia receba perguntas de outros dias. A eleição usa `.first()` sobre a ordenação `-aberta_em`. (`app/chamados/services/disputa_emissao.py:531`; `app/chamados/models.py:1804`.)

**A sonda confirma a ligação 7279 → 33492, mas não identifica o chamador histórico que inseriu cada pergunta.** Esses valores são dado fornecido, sem verificação independente: **[nao verificado]**. A cópia também não contém metadados Git; identidade criptográfica com HEAD/PROD: **[nao verificado]**.

Referências abreviadas:

| Sigla | Arquivo |
|---|---|
| M | [app/chamados/models.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/models.py) |
| E | [app/chamados/services/disputa_emissao.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/services/disputa_emissao.py) |
| F | [app/chamados/services/fio.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/services/fio.py) |
| J | [app/chamados/juizes.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/juizes.py) |
| MT | [app/chamados/services/materializacao.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/services/materializacao.py) |

**1) F0 DELTA — escritores encontrados.**

A tabela separa atribuição e persistência; construtor, proxy e pedido a uma porta não são automaticamente escritores persistentes. Exemplos concretos: inserção real em `M:3017`, proxy em `M:2876`, simulação restaurada em `F:779`/`F:783`.

| Sítio | Campos e operação persistente | Comparação com `f0f1.r` |
|---|---|---|
| `PerguntaDisputa._nascer` | Insere `P.disputa` e `P.chamado_destino`: construtor `M:3014`, save `M:3017`. | Mesmo endereço. |
| `_nascer`, reuso | Troca `P.chamado_destino`: atribuição `M:2982`, save com lista dinâmica `M:3006`. Retorna a pergunta existente, preservando sua dona: `M:3008`. | Mesmo endereço. |
| `reapontar_resolvedora` | Troca `P.chamado_destino`, inclusive para `None`: `F:925`, save `F:927`. | Mesmo endereço. |
| `_abrir_ou_agregar_disputa` | Insere DS e sua FK `chamado`: `E:587`, valor em `E:591`. | Mesmo endereço. |
| `criar_disputa_a_partir_de_chamados_orfaos` | Insere DS e sua FK `chamado`: `E:1123`, valor em `E:1127`. | Mesmo endereço. |
| `criar_disputa_ancora_de_chamado` | Insere DS e sua FK `chamado`: `E:1358`, valor em `E:1362`. | Mesmo endereço. |
| Nascimentos da DS acima | `aberta_em` pelo ORM, `auto_now_add=True`; fechamento inicialmente nulo; resolução e motivo vazios. (`M:1756`; `M:1757`; `M:1773`; `M:1793`.) | Nascimentos já listados; o script anterior não pesquisava todos esses campos. Comparação do script anexado: **[nao verificado]**. |
| `DisputaSupervisao.fechar` | `fechada_em=now`, `resolucao=argumento`, `motivo_fechamento=motivo`: `M:1932`, `M:1934`, `M:1935`; save `M:1936`. | Mesmo endereço. |
| `reabrir_admin` | Limpa os três campos: `M:2149`, `M:2151`, `M:2152`; save `M:2161`. | Mesmo endereço. |
| `reabrir_sistema` | Limpa os três campos: `M:2173`, `M:2175`, `M:2176`; save `M:2177`. | Mesmo endereço. |
| **`limpar_resolucao_residual`** | **Escreve `resolucao=''`: `M:2265`, save `M:2266`; somente aberta com texto residual: `M:2262`.** | **Sítio adicional ao inventário anterior. Ser código acrescentado depois das 02h: [nao verificado].** |

**Delta demonstrável:** o inventário anterior omitiu `limpar_resolucao_residual`; os escritores principais que ele enumerou continuam nos mesmos endereços da tabela. Nenhum deles desapareceu ou mudou de endereço. A classificação temporal “criado/movido/removido desde as 02h” exige a árvore anterior, ausente: **[nao verificado]**. (`M:2265`; demais endereços acima.)

Há deslocamentos em **chamadas**, sem mudança do escritor final: os fechamentos em materialização aparecem agora em `MT:591` e `MT:1338`, e a reabertura em `MT:691`; no anexo eram `586`, `1333` e `686`, respectivamente. Endereços anteriores: **[nao verificado]**; atuais conferidos: `MT:591`, `MT:1338`, `MT:691`.

Os efeitos e escapes também pertencem ao censo:

| Forma | Classificação conferida |
|---|---|
| `M:367` | Memória: reassocia instância do mesmo chamado após conferir o PK em `M:366`. |
| `M:2876` | Proxy para o juiz do dia; inserção real ocorre em `M:3014`/`M:3017`. |
| `F:779`, `F:783` | Simula destino e restaura no `finally`; não persiste essas atribuições. |
| Exclusão de chamado | Pode zerar `DS.chamado` e `P.chamado_destino` por `SET_NULL`. (`M:1751`; `M:3387`.) |
| Exclusão da DS | Elimina P por `CASCADE`; não transfere sua dona. (`M:2418`.) |
| `**campos` do nascimento | Expansão chega ao construtor; não existe restrição geral de nomes nessa assinatura. (`M:2776`; `M:2789`; `M:2820`; `M:3014`.) |
| `descarimbar_em_lote(**outros)` | Capacidade genérica latente: a guarda proíbe somente campos do carimbo; o update recebe os demais. (`M:3255`; `M:3264`.) Os chamadores nominais atuais passam campos de resposta/validação. (`M:1566`; `E:1593`; `E:1630`.) |
| `PerguntaDisputa.save` | Persistência genérica por `super().save`, dentro de atomic; não elege uma dona ou destino. (`M:3413`; `M:3422`; `M:3423`.) |
| `regularizacao_ext.py:256` | `reg.chamado` pertence a outra entidade. (`app/chamados/services/regularizacao_ext.py:256`.) |
| Migration `0051`, update | Escreve `colaborador_id`; lê `disputa_id` no `OuterRef`. (`app/chamados/migrations/0051_decisao_pergunta.py:25`.) |

As migrations pertinentes definem os campos, sem constituírem eleições adicionais de runtime: DS em `app/chamados/migrations/0013_disputasupervisao.py:23`; dona em `app/chamados/migrations/0014_perguntadisputa.py:27`; destino em `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:18`; motivo do fechamento em `app/chamados/migrations/0049_disputa_motivo_fechamento.py:14`.

O script abaixo enumera candidatos e mantém fila explícita para receptores desconhecidos, expansões, dicionários, reflexão e SQL. A tabela acima é a classificação após leitura dos corpos. Ausência universal de escrita por reflexão ou SQL montado dinamicamente: **[nao verificado]**.

```python
import ast
import sys
from collections import Counter
from pathlib import Path

FIELDS = {
    "disputa", "disputa_id", "chamado_destino", "chamado_destino_id",
    "aberta_em", "fechada_em", "resolucao", "motivo_fechamento",
    "chamado", "chamado_id",
}
MODELS = {"PerguntaDisputa", "DisputaSupervisao"}
OPS = {"create", "get_or_create", "update_or_create",
       "update", "bulk_create", "bulk_update", "save"}
PORTAS = {"cls", "abrir", "abrir_conversa", "_sob_contrato", "_nascer"}
rows, queue, errors = [], [], []

def strings(node):
    return {n.value for n in ast.walk(node)
            if isinstance(n, ast.Constant) and isinstance(n.value, str)}

for path in sorted(Path(".").rglob("*.py")):
    if (set(path.parts) & {".git", "__pycache__", "node_modules", "tests"}
            or path.name == "tests.py"
            or path.name.startswith(("test_", "tests_"))):
        continue
    try:
        tree = ast.parse(path.read_text(encoding="utf8"), str(path))
    except (SyntaxError, UnicodeError, OSError) as exc:
        errors.append((str(path), str(exc)))
        continue
    parents = {c: n for n in ast.walk(tree) for c in ast.iter_child_nodes(n)}
    aliases = {a.asname or a.name: a.name
               for n in ast.walk(tree) if isinstance(n, ast.ImportFrom)
               for a in n.names}

    def owners(node):
        result = []
        while node in parents:
            node = parents[node]
            if isinstance(node, (ast.ClassDef, ast.FunctionDef,
                                 ast.AsyncFunctionDef)):
                result.append(node.name)
        return result[::-1]

    def emit(node, kind, pending=False):
        section = "migration" if "migrations" in path.parts else "runtime"
        (queue if pending else rows).append(
            f"{path}:{node.lineno}|{section}|{'.'.join(owners(node))}|{kind}")

    for node in ast.walk(tree):
        if (isinstance(node, ast.Attribute)
                and isinstance(node.ctx, ast.Store) and node.attr in FIELDS):
            emit(node, "ATR " + ast.unparse(node))
        if (isinstance(node, ast.Assign)
                and isinstance(parents.get(node), ast.ClassDef)
                and parents[node].name in MODELS):
            for target in node.targets:
                if isinstance(target, ast.Name) and target.id in FIELDS:
                    emit(node, "SCHEMA " + target.id)
        if isinstance(node, (ast.Dict, ast.Subscript)) and strings(node) & FIELDS:
            emit(node, "REVER dict/subscript", True)
        if not isinstance(node, ast.Call):
            continue
        end = getattr(node.func, "attr", getattr(node.func, "id", ""))
        end = aliases.get(end, end)
        root = ast.unparse(node.func).split(".")[0]
        root = aliases.get(root, root)
        direct = {k.arg for k in node.keywords} & FIELDS
        known = False
        if (end in {"CreateModel", "AddField", "AlterField"}
                and strings(node) & (FIELDS | {
                    "perguntadisputa", "disputasupervisao"})):
            emit(node, "SCHEMA migration " + end)
        if direct and end in OPS | PORTAS | MODELS:
            emit(node, "KW " + ",".join(sorted(direct)))
            known = True
        if root in MODELS and end in {
                "create", "get_or_create", "update_or_create"}:
            emit(node, "[nome]/defaults " + root)
            known = True
        if end in OPS and strings(node) & FIELDS:
            emit(node, "REVER lista/expressao " + end)
            known = True
        if end in OPS | PORTAS | MODELS:
            if not known or any(k.arg is None for k in node.keywords):
                emit(node, "REVER receptor/kwargs " +
                     ast.unparse(node.func), True)
        if end in {"setattr", "execute", "executemany",
                   "raw", "RawSQL", "RunSQL"}:
            emit(node, "REVER reflexao/SQL " + end, True)
        if end in {"reapontar_resolvedora", "descarimbar_em_lote"}:
            emit(node, "DELEGACAO " + end)

for row in sorted(set(rows)):
    print(row)
print("FILA:", dict(Counter(
    r.rsplit("|", 1)[-1].split()[1] for r in queue)))
if "--fila" in sys.argv:
    print("\n".join(sorted(set(queue))))
for path, error in errors:
    print("ERRO:", path, error)
if errors:
    raise SystemExit(2)
```

O selo existente permite arquivos, não funções escritoras: destino admite `models.py` e `fio.py`; sua detecção usa regex de atribuição/update e exclui migrations. Portanto, sozinho não prova Single Writer por função. (`app/core/tests/test_contract_escritores_estado.py:61`; `:179`; `:191`.)

**2) Caso-prova 7279 — cada pergunta e sua eleição.**

As seis perguntas vinculadas à 7279 no dado fornecido são estas; a atribuição histórica individual a agrupador, fábrica, declaração ou API permanece **[nao verificado]**. O escritor normal atual de `P.disputa` é o construtor em `M:3014`, persistido em `M:3017`; reuso preserva a dona em `M:3008`.

| Pergunta | `data_evento` fornecida | `chamado_destino_id` fornecido | Sítio atual que insere a dona; autoria histórica |
|---|---|---|---|
| 47210 | 22/10/2026 | 33491 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 51727 | **29/10/2026** | 35801 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 51823 | 30/10/2026 | `None` | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 52340 | 31/10/2026 | `None` | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 53458 | 02/11/2026 | 36718 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 53628 | 03/11/2026 | 36809 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |

As outras perguntas mostradas pela sonda pertencem a outras disputas: 47199 → 7272; 49408 → 7431. Dado fornecido: **[nao verificado]**.

Os caminhos atuais que podem entregar perguntas à dona são:

| Caminho | Eleição da DS e chegada ao escritor |
|---|---|
| Emissão comum/fábrica | Núcleo seleciona aberta em `E:531`; wrapper gera perguntas também no reuso em `E:682`; pedido de nascimento em `E:444`. A fábrica chega pelo wrapper em `app/chamados/services/fabrica_da_celula.py:172`. |
| Agrupamento de órfãos | Seleciona aberta em `E:1096`; pedidos passam por `_criar_pergunta_se_couber`, `E:837`. |
| Declaração do colaborador | Obtém DS em `app/chamados/services/declaracao_texto.py:360`; abre P em `:376`; seleção delegada ao núcleo em `E:627`. |
| Declaração administrativa | Obtém DS em `app/chamados/services/declaracao_texto.py:659`; abre P em `:666`; mesmo núcleo em `E:627`. |
| Âncora | Seleciona aberta em `E:1332`; abre conversa em `E:1371`. |
| Complemento de P existente | Pode usar diretamente a dona da pergunta encontrada, em vez da aberta global. (`E:1070`; `E:1073`; `E:1081`.) |

Nos três seletores de abertura, a expressão é:

```python
DisputaSupervisao.objects.filter(
    colaborador=colaborador, fechada_em__isnull=True,
).first()
```

Ela mora em `E:531`, `E:1096` e `E:1332`; a ordenação é `M:1804`. Portanto, é **`.first()` da mais recente aberta do colaborador**, sem condição sobre `data_evento`; não é eleição por dia. O helper leitor `colaborador_em_disputa` repete essa consulta em `E:40`.

**Se 7279 era a mais recente aberta no instante da emissão, esses caminhos a elegiam mesmo para outro dia.** O estado completo naquele instante e o caminho percorrido por cada PK: **[nao verificado]**. A possibilidade vem dos seletores acima; perguntas de dia novo são solicitadas também no reuso em `E:682`.

**3) Chamado 33492 ↔ disputa 7279.**

Pelo dado fornecido, **sim**: `7279.chamado_id=33492` e a consulta inversa devolveu `[7279]`; ambos são **[nao verificado]** independentemente. O modelo confirma FK nullable com `SET_NULL` e reversa `disputa_origem`; não existe unicidade nessa definição. (`M:1749`.)

Quem grava a ligação:

| Nascimento | Quem obtém o chamado e grava a FK |
|---|---|
| Núcleo | Obtém por `ChamadoColaborador.abrir` em `E:568`; grava em `E:591`. |
| Agrupador | Obtém o pai em `E:1104`; grava em `E:1127`. |
| Âncora | Obtém o pai em `E:1339`; grava em `E:1362`. |

Os ramos de reuso mantêm o chamado-pai e a abertura; alteram motivos/contexto. (`E:541`; `E:562`; `E:565`; `E:1131`; `E:1366`.) A assinatura de 33492 é compatível com o agrupador, mas atribuir seu nascimento histórico a esse caminho é **[nao verificado]**.

A apresentação combina dois leitores:

- `chamado_que_lista` escolhe `P.chamado_destino_id`; quando nulo, usa `DS.chamado_id`. (`F:116`; `F:117`.)
- O modal mostra **`Chamado.criado_em`**; o card também agrupa por essa data. (`app/templates/chamados/partials/modal_fio.html:86`; `app/templates/chamados/partials/_card_atendimento.html:5`.)

Aplicando essa regra às FKs fornecidas, **51823 e 52340 seriam listadas no pai 33492**, enquanto as outras quatro têm os destinos próprios indicados na tabela. A execução efetiva da tela: **[nao verificado]**. Regra: `F:117`, `F:240`, `F:243`.

Isso permite “chamado criado em 22/10 contendo pergunta de 30/10/31/10”: a data do cabeçalho é a criação do chamado, e as perguntas têm suas próprias datas. O catálogo declara os containers de disputa sem chave de dia. (`M:67`; `app/templates/chamados/partials/modal_fio.html:86`; `app/chamados/catalogo/modulos.py:144`; `:146`; `:223`.)

Quanto ao **`em_analise`**, a agregação não renova o pai por dia. Enquanto a DS está aberta, o receiver de fechamento do pai retorna; depois do fechamento, tenta `ch.reconciliar()`, que ainda pode recusar. (`E:531`; `E:566`; `app/chamados/signals.py:28`; `:42`.) O ato específico que deixou 33492 em análise e eventuais tentativas de encerramento são **[nao verificado]**.

**4) Fechamento — condição, chamadores e limite da prova.**

C7 exige simultaneamente:

```python
not self.respostas_sem_veredito()
and not fio_vivo(list(self.perguntas.all()))
```

Esse é o corpo de `pode_fechar`, em `M:1853`. A porta `fechar` recusa primeiro respostas sem veredito (`M:1886`), depois perguntas vivas no fechamento ordinário (`M:1895`); a escrita ocorre em `M:1932`.

| Pergunta | Bloqueio por resposta sem veredito, aplicando `M:1843` ao dado |
|---|---|
| 47210 | Não entra: validação/materialização preenchidas no dado. **[nao verificado]** |
| **51727** | **Entra se não houver tranca aplicável:** resposta registrada, validação e materialização nulas. **[nao verificado]** |
| 51823 | Não entra: materialização preenchida no dado. **[nao verificado]** |
| 52340 | Não entra: materialização preenchida no dado. **[nao verificado]** |
| 53458 | Não entra: validação/materialização preenchidas no dado. **[nao verificado]** |
| 53628 | Não entra: materialização preenchida no dado. **[nao verificado]** |

**51727 fornece o bloqueio concreto condicionado à tranca:** basta o timestamp da resposta; não é necessário conhecer seu texto para essa lista. (`M:1843`; `M:1844`; `M:1846`.) A sonda não trouxe a tranca nem os toques posteriores usados pelo juiz; por isso, o retorno efetivo de `pode_fechar(7279)` é **[nao verificado]**. (`J:974`; `J:991`.)

Materialização preenchida também não prova, sozinha, ausência de pergunta viva: C6 consulta via, marco, célula e teto. A vitalidade exata das perguntas sem validação não pode ser reconstruída integralmente com o dado entregue. (`J:1396`; `J:1428`; `J:1432`; `J:1445`; resultado concreto: **[nao verificado]**.)

Chamadas diretas encontradas à porta `DisputaSupervisao.fechar`:

| Contexto | Sítio atual |
|---|---|
| Foto recebida | `app/api/foto_recebida.py:80` |
| Reconciliador: sem turno aberto | `app/chamados/reconciliador.py:412` |
| Reconciliador: absolvição pela célula | `app/chamados/reconciliador.py:612` |
| Reconciliador: evento da célula | `app/chamados/reconciliador.py:1171` |
| Porta administrativa com motivo | `M:1950` |
| Remoção do último motivo | `M:2072` |
| Desligamento | `app/colaboradores/services/desligamento.py:142` |
| Entrada sincronizada | `app/ponto/services/entrada_sincronizada.py:59` |
| Materialização de ausência | `MT:591` |
| Materialização: disputa quitada | `MT:1338` |
| Autofecho do agrupador | `E:1284` |
| Resolução por chamado resolvido | `E:1964` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:204` |
| Resolução de becos | `app/chamados/services/becos.py:57` |
| Comando de perguntas com fato | `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` |

O juiz também é consultado separadamente na classificação de becos e no planejamento da competência trancada. (`app/chamados/services/becos.py:33`; `app/chamados/services/competencia_trancada.py:153`.)

**“Nunca fechou” não está provado pelo snapshot.** Ele informa aberta no instante fornecido: **[nao verificado]**. `reabrir_sistema` limpa fechamento sem consumir o contador administrativo, portanto os contadores zerados não excluem fechamento seguido de reabertura automática. (`M:2163`; `M:2173`; `M:2177`.) Quais chamadores executaram ou recusaram para 7279: **[nao verificado]**.

Os escritores e leitores dos campos usados no diagnóstico ficam assim; os leitores são os relevantes às respostas, sem pretensão de cobrir todas as telas:

| Campo | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `P.disputa` | Nascimento `M:3014`/`M:3017`; escape genérico `M:3264`. | Identidade `M:3430`; roteamento `F:116`. |
| `P.chamado_destino` | Nascimento `M:3014`; reuso `M:2982`/`M:3006`; reaponte `F:925`/`F:927`; FK `M:3387`. | Apresentação `F:117`; reuso `M:2968`; simulação do juiz `F:778`. |
| `DS.aberta_em` | ORM nos três nascimentos; definição `M:1756`. | Ordenação da eleição `M:1804`; ordenação de cores `M:1819`. |
| `DS.fechada_em` | Nascimento `M:1757`; fechar `M:1932`; reabrir `M:2149`, `M:2173`. | Aberta `M:1824`; seletores `E:532`, `E:1097`, `E:1333`; signal `app/chamados/signals.py:28`. |
| `DS.resolucao` | Fechar `M:1934`; reabrir `M:2151`, `M:2175`; limpeza `M:2265`. | Idemp

[truncado em 20000 de 21940 caracteres]

## 11/11 20:33 · sessao cb17498e4229 · CASO-0711-HJ003 · censo · origem=Code

O “Decidir” **não garante autorização para decidir**: o verbo é calculado sem consultar o usuário; a autorização vem depois, pelo departamento do chamado e, em algumas portas, pela permissão da ação. Não encontrei veto de permissão diferente entre os 16 motivos de pergunta. (`app/chamados/catalogo/natureza.py:41`; `app/chamados/catalogo/natureza.py:58`; `app/chamados/services/fio.py:566`)

Censo estático, sem cura. Correspondência desta cópia ao HEAD de `main`, permissões efetivas dos usuários e execução em produção: **[não verificado]**. A cópia disponibilizada não permite confirmar o hash por Git. **[não verificado]**

Para reduzir repetição nas tabelas, os caminhos abaixo são expandidos assim: `C/` = `app/chamados/`; `T/` = `app/templates/chamados/`; `U/` = `app/colaboradores/`.

**Perfis considerados e significado de S/N.**

- **Supervisor:** perfil administrativo `supervisao` criado pela UI, com `is_staff=True`, grupo `gestor` e setores de supervisão; permissões conforme a semeadura padrão. Existência dessa configuração nos usuários de produção: **[não verificado]**. (`app/core/views_usuarios.py:48`; `app/core/views_usuarios.py:52`; `app/core/views_usuarios.py:295`; `app/core/views_usuarios.py:299`; `U/mixins.py:355`)
- **Admin:** `admin_ti` criado pela UI como superusuário. (`app/core/views_usuarios.py:58`; `app/core/views_usuarios.py:296`)
- **Staff₀:** cenário explícito de usuário autenticado, somente `is_staff=True`, sem superusuário, competências ou permissões adicionais. Não é um perfil cadastrado comprovado pelo anexo. **[não verificado]**
- **S:** a porta da ação indicada aceita o perfil, satisfeitas as condições de estado/dado. **N:** falta botão autorizado ou a porta recusa a ação. Aceitar o perfil não garante materialização ou fechamento: essas operações ainda têm guardas de negócio. (`C/services/fio.py:566`; `C/services/fio.py:660`; `C/models.py:1848`)

`is_staff` permite entrar na fila, mas **não é bypass de `tem_acao`**. Na autorização contextual, usuário sem competências declaradas passa pelo ramo legado; usuário com competências precisa pertencer ao departamento, salvo gestor geral. (`C/views.py:35`; `U/mixins.py:322`; `C/utils.py:47`; `C/utils.py:60`; `C/utils.py:65`)

**Origem do rótulo.**

| Informação | Quem produz | Quem lê |
|---|---|---|
| Verbo `decidir` | `C/catalogo/natureza.py:58`; partição em `C/services/verbos.py:47` | Painel: `C/services/painel.py:383`; lista: `T/partials/_lista_chamados.html:86` |
| Texto “Decidir” da linha | `C/services/verbos.py:149` | `T/partials/_lista_resumo.html:71`; `T/partials/_lista_chamados.html:86` |
| “DECIDIR / Use os botões” do fio | `C/services/desenlace.py:78`; contexto em `C/services/fio.py:403` | `T/partials/modal_fio.html:117` |
| Autorização dos botões | `C/utils.py:68`; mapa em `C/views.py:933` | `T/partials/modal_fio.html:123`; `T/partials/modal_fio.html:652` |
| Tags de apresentação | `C/templatetags/chamados_tags.py:27` delega autorização; `:68` delega opções de movimento | Os templates acima; as tags não classificam o chamado como `decidir`. (`C/templatetags/chamados_tags.py:30`; `C/templatetags/chamados_tags.py:75`) |

A lista mostra o verbo e abre o fio; não contém um POST “Decidir” próprio. No modal, o verbo aparece antes da guarda contextual dos botões. (`T/partials/_lista_chamados.html:42`; `T/partials/_lista_chamados.html:86`; `T/partials/modal_fio.html:117`; `T/partials/modal_fio.html:123`)

**Portas usadas pelas matrizes.** Cada código identifica o botão, a URL, a view e a guarda; portanto, as linhas seguintes reutilizam a mesma cadeia comprovada.

| Código / ação | Template:linha do botão | URL → view:linha | Guarda:linha |
|---|---|---|---|
| **R — responder/mover chamado** | `T/partials/modal_fio.html:672`, seletor `:665` | `/chamados/modal/<colab>/` (`C/urls.py:39`) → `C/views.py:869` → `C/services/fio.py:708` | Entrada `C/views.py:872`; render `T/partials/modal_fio.html:652`; POST `C/services/fio.py:714`; transição `:721` |
| **Q — validar/rejeitar pergunta** | `T/partials/modal_fio.html:284`, `:298`; confirmação de horário `:269` | Mesma URL/view de R → `C/services/fio.py:585`, `:625` | `C/services/fio.py:566`; conferência da pergunta listada `:598`, `:635` |
| **H — corrigir resposta de hora** | `T/partials/modal_fio.html:312` | Mesma URL/view de R → `C/services/fio.py:625` | `C/services/fio.py:566`; condição de tipo no template `T/partials/modal_fio.html:300` |
| **F — regularizar/fechar disputa** | `T/partials/modal_fio.html:359` | Mesma URL/view de R → `C/services/fio.py:646` | Contextual `:566`; resolução `:648`; pergunta sem resposta `:660`; juiz final `C/models.py:1848` |
| **L — aceitar folga sugerida** | `T/partials/modal_fio.html:551` | `/chamados/local/<id>/` (`C/urls.py:14`) → `C/views.py:359`, despacho `:431` | Contextual `C/views.py:420`; condições da sugestão `C/services/limbo_folgas.py:84` |
| **G — confirmar/realocar régua** | `T/partials/modal_fio.html:567`, `:576` | URL local → `C/views.py:438`, `:449` | Contextual `C/views.py:420`; realocação consulta vínculo `app/escala/regua_defesa.py:316` |
| **E — aprovar exceção de horário** | `T/partials/modal_fio.html:599` | URL local → `C/views.py:426` | Contextual `:420`; `_pode_aprovar` aceita staff ou grupo gestor/admin `C/views.py:41` |
| **O — perguntar sobre furos sem pergunta** | `T/partials/modal_fio.html:629` | URL modal → `C/services/fio.py:549` → `C/services/acoes_disputa.py:210` | Entrada gestor `C/views.py:872`; render `T/partials/modal_fio.html:617` |
| **I — pedir confirmação da âncora legada** | `T/partials/modal_fio.html:645` | URL modal → `C/services/fio.py:521` → `C/services/acoes_disputa.py:197` | Contextual `C/services/acoes_disputa.py:187` |
| **P — aplicar/recusar proposta de escala** | `T/partials/modal_fio.html:54`, `:64` | URL modal → `C/services/fio.py:509` | `editar_escalas`: `C/services/proposta_no_fio.py:136`, `:168`; chave declarada em `app/core/intake.py:53` |
| **A — decidir ausência na tela própria** | Link `T/detalhe_local.html:233`; botões `app/templates/ponto/partials/painel_ausencia.html:319`, `:332` | `/chamados/local/<id>/ir-para-ausencia/` → `C/views.py:1494`; depois `/ponto/ausencias/<aid>/aprovar/` ou `rejeitar/` (`app/ponto/urls.py:65`, `:74`) | Link exige `ver_ausencias`, `C/views.py:1509`; POST exige `editar_ausencias`, `app/ponto/views.py:2603`, `:2684` |

Há também a apresentação genérica R no fio completo e no detalhe local: botões em `T/fio_colaborador.html:272` e `T/detalhe_local.html:403`; views em `C/views.py:659` e `:359`; guardas em `C/services/fio.py:714` e `C/views.py:420`. (`C/urls.py:21`; `C/urls.py:14`)

**Matriz por motivo da pergunta.** A autorização é a do chamado que lista a pergunta; o motivo sozinho não determina o verbo nem o departamento. A matriz abaixo considera um chamado de supervisão. (`C/services/fio.py:563`; `C/services/fio.py:566`; `C/services/fio.py:598`; `C/catalogo/natureza.py:41`)

| Motivo — fonte | Botão/porta | Supervisor | Admin | Staff₀ |
|---|---|:---:|:---:|:---:|
| `orfao_14h` — `C/catalogo/perguntas.py:48` | Q/H | S | S | S |
| `saida_sem_entrada` — `C/catalogo/perguntas.py:56` | Q/H | S | S | S |
| `intervalo_saida` — `C/catalogo/perguntas.py:124` | Q/H | S | S | S |
| `intervalo_volta` — `C/catalogo/perguntas.py:132` | Q/H | S | S | S |
| `ancora_ausente` — `C/catalogo/perguntas.py:118` | Q | S | S | S |
| `colisao_escala` — `C/catalogo/perguntas.py:70` | Q | S | S | S |
| `sem_posto` — `C/catalogo/perguntas.py:36` | Q | S | S | S |
| `posto_desativado` — `C/catalogo/perguntas.py:64` | Q | S | S | S |
| `ordem_substituicao_invalida` — `C/catalogo/perguntas.py:76` | Q | S | S | S |
| `em_ferias` — `C/catalogo/perguntas.py:42` | Q | S | S | S |
| `batida_fora_escala_grave` — `C/catalogo/perguntas.py:82` | Q | S | S | S |
| `gps_ausente` — `C/catalogo/perguntas.py:88` | Q | S | S | S |
| `foto_ausente` — `C/catalogo/perguntas.py:94` | Q | S | S | S |
| `gps_mock` — `C/catalogo/perguntas.py:100` | Q | S | S | S |
| `device_comprometido` — `C/catalogo/perguntas.py:106` | Q | S | S | S |
| `liveness_indisponivel` — `C/catalogo/perguntas.py:112` | Q | S | S | S |

A permissão `materializar_saida` **não é exigida por Q/H/F**: a validação entra em `C/services/validacao.py:373` e chama a materialização em `:461`. Essa permissão pertence à outra porta, de saída retroativa de justificativa. (`app/ponto/views.py:3118`)

Isso não significa que todos os motivos materializem com sucesso. `posto_dropdown` e `escala_dropdown` não têm ramo no despacho mostrado, que trata `hora`, `data_hora` e `escolha`; a ausência de efeito cai na recusa. Essa diferença atinge os perfis igualmente, não é veto ao supervisor. (`C/services/materializacao.py:1226`; `C/services/materializacao.py:1239`)

**Matriz por tipo do chamado na fila.** Nas linhas R, o resultado é para responder/mover o chamado. O departamento vem do catálogo: `colab/cadastro/supervisao → supervisao`, `dp → dp`, `sistema → ti`. (`C/utils.py:27`; `C/utils.py:41`; guardas da porta R acima)

| Tipo — declaração no catálogo | Porta | Supervisor | Admin | Staff₀ |
|---|---|:---:|:---:|:---:|
| `batida_ausente` — `C/catalogo/modulos.py:105` | R; O quando aplicável | S | S | S |
| `turno_aberto` — `C/catalogo/modulos.py:107` | R; O quando aplicável | S | S | S |
| `turno_aberto_24h` — `C/catalogo/modulos.py:109` | R; O quando aplicável | S | S | S |
| `intervalo_pendente` — `C/catalogo/modulos.py:111` | R; O quando aplicável | S | S | S |
| `par_relampago` — `C/catalogo/modulos.py:113` | R | S | S | S |
| `cluster_batida_espuria` — `C/catalogo/modulos.py:115` | R | S | S | S |
| `justificativa` — `C/catalogo/modulos.py:117` | R | S | S | S |
| `ausencia_colab` — `C/catalogo/modulos.py:120` | R / alternativa A | **N / S** | S / S | S / **N** |
| `colaborador_em_ferias` — `C/catalogo/modulos.py:123` | R | **N** | S | S |
| `sem_previsao_escala` — `C/catalogo/modulos.py:126` | R; L quando aplicável | S | S | S |
| `ancora_limbo` — `C/catalogo/modulos.py:131` | R; I quando aplicável | S | S | S |
| `regua_degenerada` — `C/catalogo/modulos.py:133` | R | S | S | S |
| `regua_suspeita` — `C/catalogo/modulos.py:135` | R; G quando aplicável | S | S | S |
| `escala_colaborador` — `C/catalogo/modulos.py:137` | R | S | S | S |
| `colaborador_sem_posto` — `C/catalogo/modulos.py:139` | R | S | S | S |
| `desligamento_revisao` — `C/catalogo/modulos.py:141` | R | **N** | S | S |
| `disputa_supervisao` — `C/catalogo/modulos.py:144` | R; Q/H/F conforme estado | S | S | S |
| `disputa_supervisao_manual` — `C/catalogo/modulos.py:146` | R; Q/H/F conforme estado | S | S | S |
| `chamados` — `C/catalogo/modulos.py:149` | R, conforme área¹ | S/N | S | S |
| `regularizacao_externa` — `C/catalogo/modulos.py:151` | R | S | S | S |
| `acesso_bloqueado` — `C/catalogo/modulos.py:154` | R | **N** | S | S |
| `agenda_dia` — `C/catalogo/modulos.py:156` | R | S | S | S |
| `gps_fora_geofence` — `C/catalogo/modulos.py:160` | **Validar, não Decidir**² | — | — | — |
| `gps_ausente` — `C/catalogo/modulos.py:162` | **Registro**² | — | — | — |
| `geofence` — `C/catalogo/modulos.py:166` | R | S | S | S |
| `batida_fora_escala_leve` — `C/catalogo/modulos.py:168` | **Registro**² | — | — | — |
| `entrada_bloqueada_turno_aberto` — `C/catalogo/modulos.py:182` | R | S | S | S |
| `batida_fora_horario` — `C/catalogo/modulos.py:184` | R; E quando aplicável | S | S | S |
| `ping_geo_fora` — `C/catalogo/modulos.py:186` | R | S | S | S |
| `app_colaborador` — `C/catalogo/modulos.py:188` | R, conforme área¹ | S/N | S | S |
| `justificativa_colab` — `C/catalogo/modulos.py:190` | R | S | S | S |
| `mensagem_dp` — `C/catalogo/modulos.py:192` | R | **N** | S | S |
| `beneficio` — `C/catalogo/modulos.py:195` | R³ | **N** | S | S |
| `smoke` — `C/catalogo/modulos.py:198` | R | **N** | S | S |

¹ Para `chamados` e `app_colaborador`, uma área válida da solicitação prevalece sobre o dono do módulo. Supervisor: **S** para supervisão; **N** para outro departamento, sem competência acumulada. (`C/rotulos.py:26`; `C/utils.py:39`; `C/utils.py:65`)

² A distinção é pelo **módulo**, não pelo motivo homônimo de pergunta. `gps_fora_geofence` recebe `validar`; `gps_ausente`, `batida_fora_escala_leve`, `plataforma_mudanca` e `desligamento_revisao` são módulos de auditoria. Portanto, `desligamento_revisao` também fica fora do `Decidir` produzido pela partição normal, apesar da autorização R indicada na tabela. (`C/catalogo/natureza.py:18`; `C/catalogo/natureza.py:29`; `C/catalogo/natureza.py:44`)

³ R existe no modal normal; o modo benefício suprime essa caixa, inclusive para admin. (`T/partials/modal_fio.html:651`)

**Condições adicionais e botões que não executam.**

| Causa/ação específica | Botão → porta/guarda | Supervisor | Admin | Staff₀ |
|---|---|:---:|:---:|:---:|
| Proposta de escala | P; exige `editar_escalas`, sem guarda correspondente no template. (`T/partials/modal_fio.html:40`; `C/services/proposta_no_fio.py:136`) | S | S | **N** |
| Datas divergentes: conferir | `T/partials/modal_fio.html:177` → `/chamados/disputa/pergunta/<id>/conferir-data/`, `C/urls.py:18` → `C/views.py:1292`; `gerir_chamados` em `:1310` | S | S | **N** |
| Reabrir questionário | `T/partials/modal_fio.html:372` → URL `C/urls.py:20`, view `C/views.py:1372`; a recusa exige faltar **tanto** `gerir_chamados` **quanto** autorização contextual. (`C/views.py:1404`) | S | S | S |
| Reabertura administrativa | `T/partials/modal_fio.html:412` → modal → `C/services/acoes_disputa.py:235`; aceita `is_staff`, apesar da mensagem citar admin. (`:245`) | S | S | S |
| Fechar disputa com pergunta sem resposta | F não é oferecido; aparece cobrança. (`T/partials/modal_fio.html:334`; `:349`; guarda POST `C/services/fio.py:660`) | N | N | N |
| Fechamento manual com `ancora_ausente` | Aviso no lugar de F. (`T/partials/modal_fio.html:327`) | N | N | N |
| Aplicar declaração extraída do texto, **no modal** | `T/partials/modal_fio.html:143`, ação `:140` → modal; sem despacho em `processar_post_fio`, termina em `C/services/fio.py:742` | **N** | **N** | **N** |
| Aplicar correção de pergunta cega | `T/partials/modal_fio.html:193`, ação `:190` → mesmo término `C/services/fio.py:742` | **N** | **N** | **N** |
| Apagar pergunta cega | `T/partials/modal_fio.html:200`, ação `:197` → mesmo término `C/services/fio.py:742` | **N** | **N** | **N** |

As três últimas falhas são de despacho, não de permissão. Aplicar declaração tem despacho na **outra** porta, o detalhe local. (`C/views.py:478`)

As recusas contextuais examinadas não precisam retornar HTTP 403: o modal renderiza a mensagem de erro; outras portas usam o envelope de resposta/redirect. A entrada sem gestor retorna 404. (`C/services/fio.py:566`; `C/services/fio.py:494`; `app/core/respostas.py:63`; `app/core/respostas.py:71`; `C/views.py:872`)

**Causas com “Decidir” sem botão executável pelo supervisor no fio.**

- `colaborador_em_ferias`, `mensagem_dp`, `beneficio` são do DP; `acesso_bloqueado` e `smoke`, do TI. O verbo não filtra o perfil, mas a caixa R é suprimida e o POST recusa pelo departamento. (`C/catalogo/modulos.py:123`; `:192`; `:195`; `:154`; `:198`; `C/catalogo/natureza.py:41`; `T/partials/modal_fio.html:652`; `C/services/fio.py:714`)
- Solicitações `chamados`/`app_colaborador` de departamento diferente de supervisão têm o mesmo resultado, sem competência adicional. (`C/utils.py:39`; `C/utils.py:65`)
- `ausencia_colab` tem essa divergência **no fio**, mas possui alternativa executável no detalhe → tela de ausências para o supervisor padrão; portanto, não é uma ausência absoluta de porta. (`C/catalogo/modulos.py:120`; `T/detalhe_local.html:233`; `C/views.py:1509`; `app/ponto/views.py:2603`; `U/mixins.py:361`)
- Proposta de escala perde execução se `editar_escalas` tiver sido revogada; conferência de data perde execução se faltar `gerir_chamados`. Grants/revogações reais: **[não verificado]**. (`C/services/proposta_no_fio.py:136`; `C/views.py:1310`; `app/core/views_quadro.py:43`; `:45`)

Nenhum desses tipos foi comprovado como integrante atual da fila de produção pelo dado raspado; também não foi demonstrado que alguma das perguntas da disputa 7279 sofreu recusa por permissão. **[não verificado]**

**Quem escreve e quem lê os campos/permissões usados neste censo.** A tabela identifica escritores dos caminhos rastreados; não afirma exclusividade onde ela não foi demonstrada.

| Campo/permissão | Quem escreve | Quem lê |
|---|---|---|
| `Group.permissions`, `rbac_*` | Semeadura `U/mixins.py:397`, acionada por `U/signals.py:30`; concessão/revogação na UI `app/core/views_quadro.py:43`, `:45` | `tem_acao`: `U/mixins.py:330`; consulta `get_all_permissions` em `:331` |
| `editar_escalas`, `gerir_chamados`, `ver_ausencias`, `editar_ausencias` padrão | Lista de supervisão `U/mixins.py:355`; aplicação em `:397` | Portas P/A/conferência citadas acima |
| `materializar_saida` | Padrões DP/TI `U/mixins.py:364`, `:376`; mesma UI de permissões acima | Porta de justificativa `app/ponto/views.py:3118` |
| `User.groups` | Criação `app/core/views_usuarios.py:299`; alterações `:204`, `:206` | Gestor `C/views.py:38`; competências `U/mixins.py:139` |
| `User.is_staff` | `app/core/views_usuarios.py:295`; sincronização de perfil `app/core/credenciais.py:139` | `C/views.py:36`, `:43`; reabertura administrativa `C/services/acoes_disputa.py:245` |
| `User.is_superuser` | `app/core/views_usuarios.py:296`; alternância `:82` | `U/mixins.py:163`, `:328`; gestor geral `C/utils.py:49` |
| `Setor.competencia`, `ativo` | Criação `U/services/estrutura.py:97`; ativação `:109` | Competências do usuário `U/mixins.py:140`; semeadura `U/signals.py:30` |
| `Colaborador.perfil` | `app/core/credenciais.py:135` | `get_perfil` em `U/mixins.py:171`; transição do chamado `C/models.py:131` |
| `modulo_origem`, `area`, `contexto_json` do chamado | Campos recebidos na abertura/criação: `C/models.py:961`, `:971`, `:1070` | Departamento `C/utils.py:37`, `:41`; verbo `C/services/verbos.py:44`; condições dos botões `T/partials/modal_fio.html:544`, `:588` |
| `dono` do módulo | Catálogo `C/catalogo/modulos.py:103`; estrutura `_m` em `:72` | Resolução de departamento `C/utils.py:41` |
| `status_local` | Núcleo de estado `C/models.py:228`; criação `:1070` | Transição `C/models.py:124`; botões `T/partials/modal_fio.html:651` |
| `motivo`, `tipo_resposta`, opções da pergunta | Catálogo `C/catalogo/perguntas.py:34`; abertura pelo emissor `C/services/disputa_emissao.py:439` | Template `T/partials/modal_fio.html:300`; materialização `C/services/materializacao.py:1226` |
| `resposta_colab`, `respondida_em` | `C/services/disputa_emissao.py:1467` | Validação `C/services/validacao.py:393`; template `T/partials/modal_fio.html:217` |
| `validada_em`, `validada_por` | Carimbo `C/services/validacao.py:134`; resposta nova limpa em `C/services/disputa_emissao.py:1480` | Validação `C/services/validacao.py:396`; template `T/partials/modal_fio.html:228` |
| `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | Porta de fechamento `C/models.py:1932` | Guarda `C/models.py:1879`; roteamento `C/services/fio.py:600` |
| `todas_respondidas` | Contexto `C/services/fio.py:340` | Escolha do botão F `T/partials/modal_fio.html:334` |
| `pode_agir_map`, `is_admin_ti`, `pode_conferir_data` | `C/views.py:933`, `:936`; contexto parcial `C/services/fio.py:474` | `T/partials/modal_fio.html:123`, `:382`, `:172` |

**Scripts AST/grep usados, executados por stdin, sem importar o aplicativo.** O primeiro enumera os catálogos; o segundo verifica chamadas RBAC nos serviços. (`C/catalogo/motivos.py:33`; `C/catalogo/modulos.py:103`; `C/services/validacao.py:373`; `C/services/materializacao.py:1226`)

```bash
python3 -B - <<'PY'
import ast
from pathlib import Path
for name in ('motivos', 'modulos'):
    p = Path('app/chamados/catalogo') / (name + '.py')
    t = ast.parse(p.read_text())
    print(p)
    for n in ast.walk(t):
        if isinstance(n, ast.Assign) and any(
            isinst

[truncado em 20000 de 22013 caracteres]

## 11/11 20:33 · sessao 1500ae33b4b3 · CASO-0711-HJ003 · sonda · origem=Code

**O par está fechado na saída TURNO fornecida, mas o dado não identifica quem emitiu a pk52340 nem demonstra que a saída já existia quando ela nasceu.** A sonda também informa um processamento da pergunta **29min45s antes** da criação da saída. Portanto, atribuir este caso a um corte por dia civil seria antecipar uma conclusão. [nao verificado]

**1) O sítio que emitiu a pk52340 permanece [nao verificado].**

Os discriminantes fornecidos são: `motivo=orfao_14h`, `data_evento=2026-10-31`, `celula_id=133672`, `batida_origem_id=None`, `instancia_marco=fim`, `conversa=False`, para col407. [nao verificado]

Esses campos não constituem uma assinatura exclusiva do emissor:

| Campo observado | O que o código permite concluir |
|---|---|
| `motivo=orfao_14h` | Pode significar **marco final S faltante**: `marco_para_motivo` apenas traduz o casamento de tipo/hora com o fim; não verifica par nem mede 14h. `app/ponto/marcos.py:33`, `app/ponto/marcos.py:34`, `app/ponto/marcos.py:49`, `app/ponto/marcos.py:50`. |
| `data_evento` e `celula_id` | A porta comum determina o dia do fato, busca sua célula e grava ambos; isso serve a diferentes chamadores. `app/chamados/models.py:2875`, `app/chamados/models.py:2896`, `app/chamados/models.py:3014`, `app/chamados/models.py:3016`. |
| `instancia_marco=fim` | Deriva automaticamente do motivo pela tabela posição/tipo; não identifica a rota. `app/chamados/services/declaracao_texto.py:44`, `app/chamados/services/declaracao_texto.py:55`, `app/chamados/models.py:3438`, `app/chamados/models.py:3440`. |
| `batida_origem_id=None` | É esperado para a família MARCO: o derivador automático de episódio só atende à família BATIDA. Logo, NULL não prova que o emissor deixou de consultar batidas. `app/chamados/models.py:3448`, `app/chamados/models.py:3461`. |
| `conversa=False` | Identifica nascimento pela porta do fato; não distingue os chamadores dessa porta. `app/chamados/models.py:2789`, `app/chamados/models.py:3014`. |

As cadeias aplicáveis na cópia são:

| Entrada | Cadeia verificada |
|---|---|
| **Cron do cartório** | `06:27 --apply`, por empresa → `processar_cartorio` → `julgar_colab` → `perguntar_pela_celula` → fábrica → disputa/pergunta. `app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1032`; `app/chamados/services/fabrica_da_celula.py:172`. |
| **Signal de batida** | `post_save(Batida)` → julgamento da data local e da véspera → cartório → mesma fábrica. `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1032`. |
| **Detector contínuo** | Cron `*/5` → `detectar_ausencias` → chamado → repasse para disputa. A detecção corrente procura **entrada**; o cartório também reutiliza sua porta para cobrar marcos nomeados. `app/config/crons.py:364`; `app/ponto/management/commands/detectar_ausencias.py:335`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/services/cartorio.py:1815`. |
| **Turno aberto retroativo** | `turnos_do_colab` → exige aberto → exige idade ≥24h → abre chamado `turno_aberto_24h`. Não reconstrói o par. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:100`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`. |
| **API/anomalias** | Detector de órfão → disputa → geração de perguntas. O detector usa `batidas_apuraveis` e `parear_turnos`. `app/api/views.py:434`, `app/api/views.py:764`; `app/chamados/detector_anomalias.py:32`; `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`. |

A escrita converge em `PerguntaDisputa.abrir`, chamada pelo gerador ou pelo repasse, e na inserção de `_nascer`. O código registra um evento `pergunta_emitida`, mas esse evento não está no dado fornecido. `app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:837`; `app/chamados/models.py:3014`; `app/chamados/models.py:3025`. A associação desse nascimento à pk52340 é **[nao verificado]**.

**2) Não está demonstrado que algum emissor tenha ignorado um par já disponível. [nao verificado]**

A saída fornecida é:

```text
TURNO (2026-10-31, 142873, 143189, (...), False, True)
```

Os últimos campos significam `aberto=False` e `cross_meianoite=True`; sua ordem e construção estão em `app/ponto/turnos.py:43`, `app/ponto/turnos.py:321`, `app/ponto/turnos.py:322`. As pks e o resultado concreto são da sonda. [nao verificado]

O pareador ordena por `timestamp`, abre pela entrada e fecha com a saída posterior; não separa esse par na virada civil. `app/ponto/turnos.py:1217`, `app/ponto/turnos.py:1419`, `app/ponto/turnos.py:1491`.

A localização exata das decisões na cópia é:

| Hipótese | O que foi encontrado |
|---|---|
| **Janela de um dia civil** | `turnos_do_colab(D,D)` amplia a carga para **D−2 até D+1**, chama o pareador e depois seleciona os turnos pelo dia. Não equivale a parear somente batidas civis de D. `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1863`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1995`. |
| **Filtro por tipo no universo** | `batidas_apuraveis` filtra retratação nula, colaborador e limites opcionais; não elimina E ou S. `app/ponto/turnos.py:2876`, `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:2881`. |
| **Uso literal de `batidas_apuraveis`** | O detector de órfão a chama antes de `parear_turnos`. Já `turnos_do_colab` monta sua própria consulta com retratação nula e janela ampliada, chegando ao mesmo pareador; não chama literalmente `batidas_apuraveis`. `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`; `app/ponto/turnos.py:1863`, `app/ponto/turnos.py:1992`. |
| **Derivação por marco** | A fábrica lê faltantes da ata, verifica vencimento e traduz `(fim,S)` em `orfao_14h`. Esse caminho cobra uma lâmpada apagada, sem realizar um novo pareamento E→S. `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`, `app/chamados/services/fabrica_da_celula.py:71`; `app/chamados/services/declaracao_texto.py:44`. |
| **Relógio** | A fábrica pergunta ao cartório pelo vencimento do marco; o cartório encadeia os instantes e aplica marco +30min. `app/chamados/services/fabrica_da_celula.py:46`; `app/ponto/services/cartorio.py:879`, `app/ponto/services/cartorio.py:882`. |
| **Ordem de chegada** | A geometria ordena por `timestamp`; isso não prova quais linhas estavam disponíveis na emissão histórica. `app/ponto/turnos.py:1217`. Disponibilidade naquele instante: **[nao verificado]**. |

Há, contudo, **classificação própria residual** em `classificar_falta`: depois de obter os turnos, ela calcula presença de E/S, contagem por tipo e conserva um retorno final `orfao_14h`. `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`.

Precisamente: a saída órfã é derivada por `tem_e and not tem_s` em `app/escala/servico_jornada.py:467`; por turno aberto em `app/escala/servico_jornada.py:478`; por contagem insuficiente de S em `app/escala/servico_jornada.py:485`; e pelo retorno terminal em `app/escala/servico_jornada.py:492`. **Esse último pode devolver órfão com E+S presentes quando não há sequência esperada**, pois o retorno de dia completo está dentro de `if esperada`. `app/escala/servico_jornada.py:469`, `app/escala/servico_jornada.py:489`, `app/escala/servico_jornada.py:492`.

Isso comprova lógica residual na classe de código, mas **não comprova que esse ramo emitiu a pk52340**. A sonda não traz a alimentação histórica desse julgamento. [nao verificado]

**3) A cronologia impede tratar “par existente na sonda” como “par existente na emissão”. [nao verificado]**

| Registro | Instante fornecido |
|---|---|
| Batida pk142873, `criado_em` | 31/10/2026 19:00:40. [nao verificado] |
| Pergunta pk52340, `materializada_em` | 01/11/2026 06:30:48. [nao verificado] |
| Batida pk143189, `criado_em` | 01/11/2026 07:00:33. [nao verificado] |
| Pergunta pk52340, instante de criação | Não fornecido. **[nao verificado]** |

`materializada_em` é carimbo de **processamento**, escrito separadamente do nascimento; não pode ser renomeado como criação. `app/chamados/models.py:3393`, `app/chamados/models.py:3099`, `app/chamados/models.py:3017`.

O carimbo fornecido antecede a criação da saída em **29min45s**. Sob o fluxo normal, isso é compatível com uma pergunta já existente antes da saída; o dado não permite reconstruir sua emissão nem explicar por que recebeu aquele processamento. A comparação exata **criação da pergunta × criação das batidas** permanece **[nao verificado]**.

**4) Classe, censo e RED.**

A classe investigada é **violação do juiz único da geometria do turno: emissão de órfão por derivação concorrente de presença, contagem ou recorte de batidas**. O resíduo concreto está em `classificar_falta`; sua atribuição à pk52340 continua **[nao verificado]**. `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`.

O censo AST percorreu 758 arquivos, 4.960 funções e produziu 78 candidatas, sem erro de parse; esses números vêm da execução em memória, sem arquivo de resultado. [nao verificado]

| Emissor/alimentador revisado | Pareamento ou derivação encontrada |
|---|---|
| `detectar_anomalias` | Delega ao detector que chama universo apurável + pareador. `app/chamados/detector_anomalias.py:32`; `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`. |
| API em `views` e `views_core` | Consulta `turno_aberto_de` e motivo da lei. `app/api/views.py:615`, `app/api/views.py:623`; `app/api/views_core.py:606`, `app/api/views_core.py:614`. |
| Triagem PWA | Consulta `turno_aberto_de` e motivo da lei antes da emissão. `app/ponto/services/triagem_batida.py:647`, `app/ponto/services/triagem_batida.py:649`, `app/ponto/services/triagem_batida.py:736`. |
| `detectar_ausencias` | Consulta `turnos_do_colab`; seu caminho corrente procura entrada. `app/ponto/management/commands/detectar_ausencias.py:335`, `app/ponto/management/commands/detectar_ausencias.py:492`. |
| `emitir_turno_aberto_retroativo` | Consulta `turnos_do_colab` e exige aberto. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92`. |
| `processar_alertas_turno` | Recalcula núcleo e consome materialização. `app/ponto/management/commands/processar_alertas_turno.py:67`, `app/ponto/management/commands/processar_alertas_turno.py:324`. |
| `processar_alertas_avancados` | Recalcula núcleo e consulta duração do turno aberto. `app/ponto/management/commands/processar_alertas_avancados.py:57`, `app/ponto/management/commands/processar_alertas_avancados.py:63`. |
| Cartório → fábrica | **Por marco:** universo apurável → grade/ata → faltantes vencidos → motivo. `app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`; `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:71`. |
| Cartório → detector | **Por marco:** lê apagados e traduz o motivo. `app/ponto/services/cartorio.py:1915`, `app/ponto/services/cartorio.py:1919`, `app/ponto/services/cartorio.py:1815`. |
| `reconciliar_grade` | **Por marco:** alimenta grade com batidas cruas, lê `missing`, traduz; antes de emitir consulta `classificar_falta`. Não prova corte civil do par. `app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:173`, `app/ponto/management/commands/reconciliar_grade.py:243`. |
| Complementação/repasse de disputas | Consome ata ou `classificar_falta`, sem outro algoritmo E→S nesses corpos. `app/chamados/services/disputa_emissao.py:861`, `app/chamados/services/disputa_emissao.py:876`, `app/chamados/services/disputa_emissao.py:1200`, `app/chamados/services/disputa_emissao.py:1237`. |
| Declaração de horário | **Por marco:** faltantes da ata → casamento de horário → motivo → porta. `app/chamados/services/declaracao_texto.py:334`, `app/chamados/services/declaracao_texto.py:341`, `app/chamados/services/declaracao_texto.py:346`, `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666`. |
| **`classificar_falta`** | **Derivação residual própria**, embora o universo venha do pareador: presença, contagem e retorno terminal. `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`. |

Nas rotas revisadas, não foi comprovado outro pareamento E→S independente por dia civil. **A certificação de TODO emissor possível permanece [nao verificado]**: o script seleciona candidatas sintáticas, e a revisão dos caminhos foi manual; não resolve integralmente chamadas dinâmicas, callbacks e reexportações.

Script reproduzível, somente leitura, sem importar a aplicação ou acessar banco:

```bash
python3 -B - <<'PY'
import ast
from pathlib import Path
from collections import Counter

motivos = {
    'orfao_14h', 'saida_sem_entrada', 'turno_aberto',
    'turno_aberto_24h', 'saida_pendente', 'entrada_sem_saida',
}
helpers = {
    'marco_para_motivo', '_marcos_faltantes', 'classificar_falta',
    'motivo_da_lei', 'detectar_bloqueio_turno_orfao',
    'criar_disputa_a_partir_de_chamados_orfaos',
    'criar_ou_atualizar_disputa', 'gerar_perguntas_disputa',
    '_criar_pergunta_se_couber',
}
totais = Counter()
linhas, erros = [], []

class Corpo(ast.NodeVisitor):
    def __init__(self):
        self.nodes = []

    def visit(self, node):
        self.nodes.append(node)
        if not isinstance(node, (
            ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef
        )):
            super().visit(node)

for arquivo in sorted(Path('app').rglob('*.py')):
    if {'tests', 'migrations'} & set(arquivo.parts):
        continue
    if arquivo.name.startswith(('test_', 'tests')):
        continue
    totais['files'] += 1
    try:
        tree = ast.parse(arquivo.read_text(), filename=str(arquivo))
    except (SyntaxError, UnicodeError) as exc:
        erros.append((str(arquivo), str(exc)))
        continue
    aliases = {
        alias.asname or alias.name: alias.name
        for node in ast.walk(tree) if isinstance(node, ast.ImportFrom)
        for alias in node.names
    }
    for func in ast.walk(tree):
        if not isinstance(func, (ast.FunctionDef, ast.AsyncFunctionDef)):
            continue
        totais['functions'] += 1
        corpo = Corpo()
        for stmt in func.body:
            corpo.visit(stmt)
        chamadas = {
            aliases.get(node.func.id, node.func.id)
            if isinstance(node.func, ast.Name) else node.func.attr
            for node in corpo.nodes if isinstance(node, ast.Call)
            and isinstance(node.func, (ast.Name, ast.Attribute))
        }
        literais = {
            node.value for node in corpo.nodes
            if isinstance(node, ast.Constant)
            and isinstance(node.value, str)
        } & motivos
        if literais or chamadas & helpers:
            totais['candidates'] += 1
            linhas.append((
                str(arquivo), func.lineno, func.name,
                ','.join(sorted(literais)),
                ','.join(sorted(chamadas & helpers)),
            ))

print(dict(totais), 'parse_errors', erros)
for linha in linhas:
    print('%s:%s %s | %s | %s' % linha)
PY
```

O **RED mínimo proposto** usa duas batidas apuráveis: E em D às 19:00 e S em D+1 às 07:00, com célula/escala noturna correspondente. Congela o relógio depois da saída e do prazo aplicável, chama o caminho real de emissão e exige: turno único fechado e **nenhuma pergunta `orfao_14h` para D**. O mutante descarta a resposta fechada do pareador e procura S somente no dia civil D: deve gerar a pergunta e fazer o RED falhar. Proposta não executada. [nao verificado]

O teste existente de travessia 22–06 verifica apenas o pareador; não substitui esse teste de emissão. `app/ponto/tests/test_turnos.py:12`.

A correção de classe proposta é fazer os emissores de **órfão geométrico** consumirem exclusivamente a decisão do pareador, eliminando decisões concorrentes por presença/contagem; cobrança de marco deve conservar sua causa explícita. Aplicação concreta à pk52340 depende da origem e do instante de nascimento que faltam no dado. [nao verificado]

Para os campos usados acima, seguem os escritores e leitores relevantes da cópia; a tabela não pretende ser o censo de todos os leitores de cada coluna.

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Pergunta `motivo` | Nascimento: `app/chamados/models.py:3014`. | Identidade: `app/chamados/services/fio.py:1460`. |
| Pergunta `data_evento` | Ponto de atribuição: `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016`. | Identidade: `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495`. |
| Pergunta `celula_id` | Nascimento/reuso: `app/chamados/models.py:3014`, `app/chamados/models.py:2966`; rechaveamento: `app/chamados/services/fio.py:1038`. | Juízo da célula: `app/chamados/juizes.py:1331`. |
| Pergunta `batida_origem_id` | Derivação no nascimento: `app/chamados/models.py:3448`; MARCO retorna NULL em `app/chamados/models.py:3461`. | Dia do episódio: `app/chamados/services/fio.py:1487`. |
| Pergunta `instancia_marco` | `app/chamados/models.py:3440`. | Identidade: `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495`. |
| Pergunta `conversa` | Portas do fato/conversa: `app/chamados/models.py:2789`, `app/chamados/models.py:2801`; inserção: `app/chamados/models.py:3014`. | `app/chamados/services/fio.py:1445`. |
| Pergunta `materializada_em` | Carimbo: `app/chamados/models.py:3099`. | Estado: `app/chamados/models.py:3046`. |
| Batida `timestamp` / `tipo` | Registro: `app/ponto/registro_batida.py:157`. | Ordenação/papel: `app/ponto/turnos.py:1217`, `app/ponto/turnos.py:1382`. |
| Batida `criado_em` | Django, `auto_now_add`: `app/ponto/models.py:79`. | Guarda de eco: `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362`. |
| Batida `retratada_em` | Anotação/retratação: `app/ponto/registro_batida.py:252`, `app/ponto/registro_batida.py:274`. | Universo apurável: `app/ponto/turnos.py:2876`. |
| Turno `data_turno` / `aberto` / `cross_meianoite` | Construção: `app/ponto/turnos.py:318`, `app/ponto/turnos.py:321`, `app/ponto/turnos.py:322`. | Seleção/classificação: `app/escala/servico_jornada.py:405`, `app/escala/servico_jornada.py:478`, `app/escala/servico_jornada.py:256`. |
| Ata/grade `missing` | Montagem da grade e lavra da ata: `app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:1859`. | Leitura dos faltantes: `app/chamados/services/declaracao_texto.py:131`. |

## 11/11 20:31 · sessao 15554d92a5c8 · CASO-0711-HJ003 · sonda · origem=Code

O problema é compatível com **a mesma saída identificada por dois dias diferentes**: dia do turno e dia civil da saída. O dedup distingue essas datas; o escritor único atual não corrige automaticamente uma data civil recebida como se fosse da célula. Isso está demonstrado no código; qual emissor criou cada pk histórico permanece **[nao verificado]**. (`app/chamados/models.py:2753`; `app/chamados/services/fio.py:1638`; `app/ponto/turnos.py:2939`)

1. Para os dois pares, os valores fornecidos são estes. Datas em 2026; R=`respondida_em`, V=`validada_em`, M=`materializada_em`. Como a sonda veio no texto, sem arquivo numerado, sua transcrição recebe **[nao verificado]**.

| pk | motivo | `data_evento` | célula / `celula_data` / trabalho | disputa | R | V | M / via |
|---|---|---|---|---|---|---|---|
| 47199 | `orfao_14h` | 21/10 | 133662 / 21/10 / sim | 7272 | 01/11 07:02:58 | 03/11 08:05:06 | 22/10 06:45:19 / `celula_absolveu` **[nao verificado]** |
| 47210 | `orfao_14h` | 22/10 | 133663 / 22/10 / folga | 7279 | 22/10 08:07:38 | 06/11 14:45:08 | 06/11 14:45:08 / `declaracao` **[nao verificado]** |
| 53458 | `orfao_14h` | 02/11 | 133674 / 02/11 / sim | 7279 | 05/11 07:00:59 | 05/11 13:01:42 | 05/11 13:01:42 / `fato_ocupado` **[nao verificado]** |
| 53628 | `orfao_14h` | 03/11 | 133675 / 03/11 / folga | 7279 | — | — | 04/11 06:30:51 / `celula_absolveu` **[nao verificado]** |

As três primeiras têm resposta, validação e carimbo; 53628 tem somente carimbo. **Carimbo não prova criação de batida**: as portas também carimbam encerramentos por identidade, e o juiz exige testemunha externa para reconhecer materialização efetiva. (`app/chamados/models.py:3097`; `app/chamados/models.py:3168`; `app/chamados/juizes.py:662`)

O nascimento das quatro perguntas não pode ser datado pelos carimbos acima. `PerguntaDisputa` não declara `criado_em`; o INSERT e o registro `pergunta_emitida` ficam no nascimento, mas essa trilha não foi fornecida. Instante de criação e chamada histórica responsável por cada pk: **[nao verificado]**. (`app/chamados/models.py:2406`; `app/chamados/models.py:3017`; `app/chamados/models.py:3025`)

Segundo o aval fornecido, cada par representa a mesma saída faltante **[nao verificado]**. O mecanismo descrito no código corresponde ao caso: o resolvedor antigo entregava o dia da saída esperada, D+1; o leitor interpretava esse valor como dia do turno e acrescentava novamente a cauda noturna. (`app/escala/servico_jornada.py:498`)

A distinção entre as hipóteses é:

- **Dois emissores históricos diferentes ou duas invocações do mesmo:** a sonda não identifica; **[nao verificado]**.
- **Dois significados de `data_evento`:** o código documenta explicitamente esse defeito histórico. (`app/escala/servico_jornada.py:501`)
- **Dedup que não casa D com D+1:** confirmado estruturalmente. A consulta inclui `data_evento`; trocar somente a disputa não deveria duplicar a mesma chave, mas trocar o dia cria outra chave. (`app/chamados/models.py:2753`; `app/chamados/models.py:2776`; `app/chamados/models.py:3483`)

Portanto, não atribuo 47199 à fábrica e 47210 ao detector — nem faço essa atribuição para 53458/53628 — sem trilha de emissão **[nao verificado]**. No HEAD, o nascimento factual converge em `_nascer`, que recebe motivo/disputa/célula e grava o dia pela função única. (`app/chamados/models.py:3014`; `app/chamados/models.py:3016`)

2. **O HEAD corrigiu os caminhos principais, mas não permite garantir “todos os emissores usam o juiz da batida”.** A chave de MARCO é:

`(colaborador, data_evento, motivo, instancia_marco)`

Ela exclui conversa e vias fora do fato; o índice parcial cobre datas desde 21/09/2026. `orfao_14h` pertence à família MARCO. (`app/chamados/models.py:2403`; `app/chamados/models.py:3482`; `app/chamados/catalogo/motivos.py:34`)

Os pré-dedups da emissão ainda consultam disputa/motivo/data; a porta final procura o universo do fato e escolhe a canônica entre disputas. Nenhuma dessas etapas equipara automaticamente D a D+1 para um marco sem episódio. (`app/chamados/services/disputa_emissao.py:417`; `app/chamados/services/disputa_emissao.py:1415`; `app/chamados/models.py:2905`; `app/chamados/services/fio.py:1638`)

| Caminho de pergunta de saída | Origem do dia no HEAD |
|---|---|
| Detector → geração | Batida pendente → `resolver_data_evento` → `dia_do_turno_da_batida`. (`app/chamados/detector_anomalias.py:41`; `app/escala/servico_jornada.py:508`) |
| Pendentes adicionais | Consulta o juiz por pk; não utiliza o dia civil listado no item. (`app/chamados/services/disputa_emissao.py:272`) |
| Repasse com batida de origem | Usa `resolver_data_evento`; sem resposta, não emite. (`app/chamados/services/disputa_emissao.py:1205`) |
| Fábrica da célula | Entrega o dia da célula à geração. (`app/chamados/services/fabrica_da_celula.py:172`) |
| Complemento de marcos | Usa o dia do marco faltante pela porta da célula. (`app/chamados/services/disputa_emissao.py:873`; `app/chamados/services/disputa_emissao.py:880`) |
| Declaração do colaborador | Usa o dia da célula faltante. (`app/chamados/services/declaracao_texto.py:375`) |
| Declaração por texto/admin | Usa o dia da célula faltante. (`app/chamados/services/declaracao_texto.py:665`) |

Os caminhos que **ainda aceitam uma data sem consultar o turno da batida** são:

- `gerar_perguntas_disputa`: aceita `contexto[motivo]['data']`, converte diretamente para `date` e depois a envolve em `dia_do_turno_da_celula`. Isso é adequado quando a fonte é a célula; não verifica um contexto civil legado ou incorreto. (`app/chamados/services/disputa_emissao.py:371`; `app/chamados/services/disputa_emissao.py:443`)
- Repasse **sem origem**: usa `data_evento_do_contexto`, cujo parser aceita `data_turno`, `data_batida`, `entrada_timestamp`, `pendentes` e `horario_previsto`, sem consultar o pareador. (`app/chamados/services/disputa_emissao.py:1232`; `app/chamados/data_evento.py:16`)

`chamados/data_evento.py` é esse parser; o juiz da pertença da batida está em `ponto/turnos.py`. A função `dia_do_turno_da_celula(d)` apenas declara o dia recebido, sem convertê-lo. (`app/chamados/data_evento.py:27`; `app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2939`)

Assim, **com os produtores corrigidos entregando D, o mesmo fato deve convergir para uma pergunta**; reprodução executada desse cenário: **[nao verificado]**. **Se uma rota ainda entregar D+1 como contexto de marco, permanece possível uma segunda pergunta**, porque a porta preserva esse dia e o dedup o considera outra chave. (`app/chamados/services/fio.py:1630`; `app/chamados/services/fio.py:1638`; `app/chamados/models.py:2753`)

3. A batida **147824** aparece na sonda com `origem=disputa_s84_retro`, timestamp **23/10 07:00**, criada/aprovada em **06/11 14:45:08**, aprovação por pk657, `aprovada=True`, `sequencia=1`, sem retratação, canal, GPS ou timestamps de dispositivo/sincronização **[nao verificado]**. `sequencia=1` não significa entrada: `tipo` é outro campo; o TURNO fornecido apresenta essa batida como S **[nao verificado]**. (`app/ponto/models.py:23`)

O aval identifica **47210 → 147824**, e os carimbos fornecidos coincidem **[nao verificado]**. Porém a sonda não trouxe `Batida.pergunta_origem_id`; a ligação por FK permanece **[nao verificado]**. No HEAD, o materializador grava essa ligação e a migração a lê. (`app/chamados/services/materializacao.py:951`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`)

**Não aparece outra batida real do mesmo instante/tipo no recorte fornecido**; inexistência fora desse recorte: **[nao verificado]**. Pergunta dupla para a saída de 22/10 e plantio deslocado para 23/10 não demonstram, por si, batida duplicada **[nao verificado]**.

A cura do +1 está presente nesta cópia: o resolvedor consulta o dia do turno, e o ajuste do plantio usa a cadeia de marcos a partir desse dia. “No ar desde 07/11” é premissa do enunciado **[nao verificado]**. (`app/escala/servico_jornada.py:495`; `app/chamados/juizes.py:296`)

O registro local do apply de 09/11 lista exclusivamente **50484, 45495, 41639 e 47164**, quatro vivas e zero materializadas. Logo **47199/47210 não foram migradas naquele apply**. (`app/docs/RELATO.md:612`)

O motivo exato de exclusão exige respeitar a ordem do comando:

| Pergunta | O que o código permite concluir |
|---|---|
| Ambas | A seleção exige `tipo_resposta='hora'`, motivo admitido, data preenchida e `conversa=False`. O tipo da resposta não foi raspado: participação na seleção inicial **[nao verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205`) |
| 47199 | Se não houver plantada viva ligada, a via `celula_absolveu` resulta em `sem_fonte`. Existência dessa ligação: **[nao verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65`) |
| 47210 | Admitindo a FK para 147824 e o turno 23/10 mostrado, a saída é 23/10 e `data_evento` é 22/10: o comando faz `continue`, fora do universo, antes das demais recusas. Premissa FK: **[nao verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:158`) |
| Materializada que passe essas guardas | `orfao_14h` divergente é recusada como `fora_do_aval` **antes** da verificação de colisão. Portanto não há prova de exclusão de 47210 “por colisão com 47199”. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`) |

A migração rechaveia a pergunta pela porta; ela não repara o timestamp da batida 147824. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`)

O mapa de escrita/leitura dos campos citados é este; ele descreve o HEAD, não prova a chamada histórica de cada pk.

| Campos | Quem escreve | Quem lê |
|---|---|---|
| Pergunta: motivo, disputa, colaborador, conversa | Nascimento e identidade: `app/chamados/models.py:3014`, `:3430` | Dedup/identidade: `app/chamados/models.py:2753` |
| `data_evento` | Atribuição única: `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016` | Dedup: `app/chamados/models.py:2753`; plantio: `app/chamados/juizes.py:296` |
| `celula_id` | Seleção/nascimento: `app/chamados/models.py:2896`, `:3014`; reuso `:2966`; rechaveamento `app/chamados/services/fio.py:1038` | Guarda pessoa/dia: `app/chamados/juizes.py:651` |
| Célula: data, trabalha | Geração: `app/ponto/portas/celula.py:259` | Dia e folga: `app/chamados/juizes.py:99`, `:455` |
| `instancia_marco`, `batida_origem_id` | Identidade: `app/chamados/models.py:3438`, `:3448` | Dedup: `app/chamados/models.py:2753`, `:2717`; dia: `app/chamados/services/fio.py:1633` |
| `respondida_em` | Resposta: `app/chamados/services/disputa_emissao.py:1468`; automática/trilha `:1653`, `:1674`; carimbo `app/chamados/models.py:3132` | Estado: `app/chamados/models.py:3044` |
| `validada_em` | Validação: `app/chamados/services/validacao.py:134`; carimbo `app/chamados/models.py:3135`; nova resposta limpa em `app/chamados/services/disputa_emissao.py:1480` | Estado: `app/chamados/models.py:3041` |
| `materializada_em`, autor, via | Ponto único: `app/chamados/models.py:3097` | Estado: `app/chamados/models.py:3041`; migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147` |
| Batida: timestamp, tipo, origem, aprovação/autor/data | Materializador fornece: `app/chamados/services/materializacao.py:932`; registro persiste: `app/ponto/registro_batida.py:157` | Dedup: `app/ponto/registro_batida.py:107`; auditoria: `app/chamados/services/materializacao.py:965` |
| Batida: `pergunta_origem_id` | `app/chamados/services/materializacao.py:951` | Migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138` |
| Batida: `criado_em` | ORM, `auto_now_add`: `app/ponto/models.py:79` | Vigia: `app/ponto/management/commands/vigia_plantio_overnight.py:76` |
| Batida: sequência, intervalo, canal, GPS, dispositivo/sincronização, retratação | Campos/defaults: `app/ponto/models.py:25`, `:55`, `:60`, `:67`, `:75`; INSERT: `app/ponto/registro_batida.py:157` | Leitor original desses campos na sonda: **[nao verificado]**; retratação é lida pelo dedup em `app/ponto/registro_batida.py:109` |

4. O RED cabe em [app/chamados/tests/test_plantio_noturno_mais1.py:75](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/tests/test_plantio_noturno_mais1.py:75), na classe `OrfaoNoturnoTest`. A fixture já dispõe de 12x36, horário 19–07, D=21/10 e relógio congelado em 22/10; os testes atuais verificam resolvedor, plantio e contexto do detector. (`app/chamados/tests/test_plantio_noturno_mais1.py:30`; `:48`; `:75`; `:78`; `:90`)

A extensão mínima proposta é:

- **Fixture:** colNNN, célula D de trabalho, D+1 de folga, E em D às 19h e nenhuma S; executar detector/geração e fábrica/repasse para essa mesma saída.
- **Resultado exigido:** exatamente uma pergunta `orfao_14h`, `data_evento=D`, célula D, `instancia_marco=fim`, `conversa=False`; repetir os caminhos conserva a pk.
- **Mutante:** o produtor do detector volta a entregar D+1 enquanto a fábrica entrega D. A contagem deve chegar a duas e reprovar o teste. [nao verificado]

Essa é uma especificação de RED, **não criada nem executada [nao verificado]**. Ela testa a cooperação dos emissores contra a separação D/D+1 que o dedup atual permite. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/models.py:2753`)

## 11/11 20:31 · sessao 5e912c9d1eed · - · - · origem=Ronald (tmux)

Censo executado em memória: **758 arquivos, 4.960 funções, 78 candidatas, zero erros de parse**. Escopo: `app/**/*.py`, excluindo diretórios `tests`, `migrations` e arquivos começando em `test_`/`tests`. Esses números são resultado da execução do script abaixo; sem arquivo persistido: `[nao verificado]`.

O levantamento **não demonstrou outro emissor que reconstrua E→S por dia civil sem consultar o juiz**. Demonstrou rotas que dão o nome `orfao_14h` a **marco de saída apagado**, além da contagem residual em `classificar_falta`; portanto, “não chama `parear_turnos` diretamente” não equivale a “pareia por conta própria”. (`app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:71`, `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:480`)

| Candidato / rota de emissão | Origem da decisão e classificação |
|---|---|
| `detector_anomalias.detectar_anomalias` → disputa | Consulta `detectar_bloqueio_turno_orfao`; este lê `batidas_apuraveis` e chama `parear_turnos`. Sem reconstrução própria do par. (`app/chamados/detector_anomalias.py:32`, `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`) |
| `api.views.api_bater_ponto` | Consulta `turno_aberto_de`, `em_intervalo` e `_motivo_da_lei`; as anomalias então recebem `orfao_14h`. (`app/api/views.py:615`, `app/api/views.py:623`, `app/api/views.py:636`, `app/api/views.py:640`) |
| `api.views_core.api_bater_ponto` | Mesma delegação da cópia acima. (`app/api/views_core.py:606`, `app/api/views_core.py:614`, `app/api/views_core.py:627`, `app/api/views_core.py:631`) |
| `triagem_batida.triagem_bater_ponto` / `ramo_registrar_es` | Triagem usa universo/pareador; ramo S pergunta `turno_aberto_de` e `motivo_da_lei`, antes de emitir `saida_sem_entrada`. (`app/ponto/services/triagem_batida.py:137`, `app/ponto/services/triagem_batida.py:648`, `app/ponto/services/triagem_batida.py:736`) |
| `detectar_ausencias.handle` → `_abrir_chamado` | Busca comparecimento por `turnos_do_colab`, sem fatia civil de batidas; `_marcos_faltantes` nomeia uma entrada prevista, não calcula E→S. (`app/ponto/management/commands/detectar_ausencias.py:335`, `app/ponto/management/commands/detectar_ausencias.py:336`, `app/ponto/management/commands/detectar_ausencias.py:492`) |
| `emitir_turno_aberto_retroativo.handle` | Lê `turnos_do_colab`, depois abre chamado pela porta. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`) |
| `processar_alertas_turno` → `_abrir_chamado_critico` | Faz `recompute_turnos`, consome materialização e abre chamado; não reconstitui o par. (`app/ponto/management/commands/processar_alertas_turno.py:67`, `app/ponto/management/commands/processar_alertas_turno.py:324`) |
| `processar_alertas_avancados.handle` | Faz recompute e pergunta `turno_aberto_ha`; emissão N5 deriva desse resultado. (`app/ponto/management/commands/processar_alertas_avancados.py:57`, `app/ponto/management/commands/processar_alertas_avancados.py:63`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| **Cartório → fábrica da célula** | Cartório entrega `batidas_apuraveis` à grade. A fábrica lê faltantes da ata, aplica vencimento por instante e traduz `(fim,S)` em motivo. É **rota por marco**, não pareamento E→S. (`app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`, `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`, `app/chamados/services/fabrica_da_celula.py:71`, `app/chamados/services/fabrica_da_celula.py:172`) |
| **Cartório → `_marcos_do_furo_parcial` → detector** | Lê `lastro.marcos_apagados`, traduz pelo `marco_para_motivo` e repassa ao emissor de chamado. É **rota por marco**, sem novo cálculo do par. (`app/ponto/services/cartorio.py:1915`, `app/ponto/services/cartorio.py:1919`, `app/ponto/services/cartorio.py:1815`) |
| **`reconciliar_grade.handle`** | Monta grade com `c.batidas.all()` **cru**, identifica `missing` e traduz pelo `marco_para_motivo`. Divergência de universo real a censar; isso não prova separação civil E/S. (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:173`) |
| `disputa_emissao._complementar_marcos_faltantes` | Usa `_marcos_faltantes`, mapeia posição/tipo e pede criação. **Rota por ata**. (`app/chamados/services/disputa_emissao.py:861`, `app/chamados/services/disputa_emissao.py:876`, `app/chamados/services/disputa_emissao.py:881`) |
| `disputa_emissao.criar_disputa_a_partir_de_chamados_orfaos` | Pode consumir ata, complementar marcos ou consultar `_classificar_falta`; não possui algoritmo próprio de pareamento. (`app/chamados/services/disputa_emissao.py:1081`, `app/chamados/services/disputa_emissao.py:1175`, `app/chamados/services/disputa_emissao.py:1200`, `app/chamados/services/disputa_emissao.py:1237`) |
| `declaracao_texto.declarar_hora_colab` / `aplicar_declaracao` | Primeiro lê faltantes; motivo vem da tabela `(posição,tipo)`; a abertura recebe o resultado. `_marcos_faltantes` lê `grade_da_celula` e `missing`. **Rota por ata**. (`app/chamados/services/declaracao_texto.py:103`, `app/chamados/services/declaracao_texto.py:131`, `app/chamados/services/declaracao_texto.py:334`, `app/chamados/services/declaracao_texto.py:346`, `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666`) |
| **`classificar_falta`**, alimentador dos emissores | Universo canônico via `turnos_do_colab`; conserva lógica própria `tem_e/tem_s`, contagem por tipo e retorno final de órfão. Não se pode dizer “zero lógica residual”. (`app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:465`, `app/escala/servico_jornada.py:478`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`) |

`marco_para_motivo` é tradutor: retorna `orfao_14h` ao identificar a saída final, sem consultar batidas nem declarar geometricamente uma entrada órfã. (`app/ponto/marcos.py:14`, `app/ponto/marcos.py:34`, `app/ponto/marcos.py:50`)

Script executado, reproduzível por stdin, sem arquivo:

```bash
python3 - <<'PY'
import ast, pathlib, collections
M = {'orfao_14h', 'saida_sem_entrada', 'turno_aberto',
     'turno_aberto_24h', 'saida_pendente', 'entrada_sem_saida'}
H = {'marco_para_motivo', '_marcos_faltantes', 'classificar_falta',
     'motivo_da_lei', 'detectar_bloqueio_turno_orfao',
     'criar_disputa_a_partir_de_chamados_orfaos',
     'criar_ou_atualizar_disputa', 'gerar_perguntas_disputa',
     '_criar_pergunta_se_couber'}
count = collections.Counter()
rows, errors = [], []

class Body(ast.NodeVisitor):
    def __init__(self):
        self.nodes = []
    def visit(self, n):
        self.nodes.append(n)
        if not isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef,
                              ast.ClassDef)):
            super().visit(n)

for p in sorted(pathlib.Path('app').rglob('*.py')):
    if any(x in {'tests', 'migrations'} for x in p.parts):
        continue
    if p.name.startswith(('test_', 'tests')):
        continue
    count['files'] += 1
    try:
        tree = ast.parse(p.read_text(), filename=str(p))
    except (SyntaxError, UnicodeError) as exc:
        errors.append((str(p), str(exc)))
        continue
    aliases = {a.asname or a.name: a.name
               for n in ast.walk(tree) if isinstance(n, ast.ImportFrom)
               for a in n.names}
    for f in ast.walk(tree):
        if not isinstance(f, (ast.FunctionDef, ast.AsyncFunctionDef)):
            continue
        count['functions'] += 1
        v = Body()
        for stmt in f.body:
            v.visit(stmt)
        calls = {
            aliases.get(n.func.id, n.func.id)
            if isinstance(n.func, ast.Name) else n.func.attr
            for n in v.nodes if isinstance(n, ast.Call)
            and isinstance(n.func, (ast.Name, ast.Attribute))
        }
        motifs = {n.value for n in v.nodes
                  if isinstance(n, ast.Constant)
                  and isinstance(n.value, str)} & M
        if motifs or calls & H:
            count['candidates'] += 1
            rows.append((str(p), f.lineno, f.name,
                         ','.join(sorted(motifs)),
                         ','.join(sorted(calls & H))))
print(dict(count), 'parse_errors', errors)
for row in rows:
    print('%s:%s %s | %s | %s' % row)
PY
```

Limite a preservar no relatório: esse AST gera **candidatas sintáticas**, depois os caminhos acima foram lidos transitivamente à mão; ele não resolve completamente imports reexportados, callbacks, `getattr`, dispatch dinâmico nem colisões de aliases locais. A afirmação “TODO emissor possível foi eliminado por grafo transitivo exaustivo” permanece **`[nao verificado]`**.

## 11/11 20:30 · sessao 1ac523deae44 · - · - · origem=Ronald (tmux)

Evidência para consolidação, considerando supervisor criado com `is_staff=True` e grupo `gestor` conforme recorte informado pelo pai:

- Todas as perguntas usam a mesma porta e a mesma guarda contextual, sem RBAC específico por motivo: `app/chamados/services/fio.py:558`, `app/chamados/services/fio.py:566`. Os serviços de validação/materialização não chamam `tem_acao`, `has_perm`, `get_perfil` ou `tem_perfil`; verificação AST resultou `[]` nos dois arquivos. O despacho da materialização é por `tipo_resposta`: `app/chamados/services/materializacao.py:1226`.
- `materializar_saida` pertence à porta de justificativa (`app/ponto/views.py:3118`), e não ao caminho de validar pergunta, que entra em `app/chamados/services/validacao.py:373` e chama materialização em `app/chamados/services/validacao.py:461`.
- Supervisor, admin e staff passam `_is_gestor` se staff ou nos grupos literais `dp_rh`, `gestor`, `admin_ti`: `app/chamados/views.py:24`, `app/chamados/views.py:35`. Staff não é bypass do departamento: o bypass é superuser/`tem_perfil(admin_ti)`, `app/chamados/utils.py:47`; a guarda normal lê competências, `app/chamados/utils.py:52`.
- “Reabrir administrativamente”, apesar da mensagem `admin_ti`, aceita qualquer `is_staff`: `app/chamados/services/acoes_disputa.py:245`.

Matriz dos motivos — mesma porta, mesma guarda:

| Motivo | Tipo | Fonte |
|---|---|---|
| `sem_posto` | `posto_dropdown` | `app/chamados/catalogo/perguntas.py:36` |
| `em_ferias` | `escolha` | `app/chamados/catalogo/perguntas.py:42` |
| `orfao_14h` | `hora` | `app/chamados/catalogo/perguntas.py:48` |
| `saida_sem_entrada` | `hora` | `app/chamados/catalogo/perguntas.py:56` |
| `posto_desativado` | `escolha` | `app/chamados/catalogo/perguntas.py:64` |
| `colisao_escala` | `escala_dropdown` | `app/chamados/catalogo/perguntas.py:70` |
| `ordem_substituicao_invalida` | `escolha` | `app/chamados/catalogo/perguntas.py:76` |
| `batida_fora_escala_grave` | `escolha` | `app/chamados/catalogo/perguntas.py:82` |
| `gps_ausente` | `escolha` | `app/chamados/catalogo/perguntas.py:88` |
| `foto_ausente` | `escolha` | `app/chamados/catalogo/perguntas.py:94` |
| `gps_mock` | `escolha` | `app/chamados/catalogo/perguntas.py:100` |
| `device_comprometido` | `escolha` | `app/chamados/catalogo/perguntas.py:106` |
| `liveness_indisponivel` | `escolha` | `app/chamados/catalogo/perguntas.py:112` |
| `ancora_ausente` | `data_hora` | `app/chamados/catalogo/perguntas.py:118` |
| `intervalo_saida` | `hora` | `app/chamados/catalogo/perguntas.py:124` |
| `intervalo_volta` | `hora` | `app/chamados/catalogo/perguntas.py:132` |

Botões e portas compartilhados:

| Botão | Template | URL → view → serviço/guarda |
|---|---|---|
| Validar | `app/templates/chamados/partials/modal_fio.html:284` (implausível: `:269`) | `chamados:modal_fio`, `app/chamados/urls.py:39` → `app/chamados/views.py:869`, gate `:872` → `app/chamados/services/fio.py:585`, guarda `:566` |
| Rejeitar resposta | `app/templates/chamados/partials/modal_fio.html:298` | mesma URL/view → `app/chamados/services/fio.py:625`, guarda `:566` |
| Corrigir (somente hora) | `app/templates/chamados/partials/modal_fio.html:312`, condição `:300` | mesma URL/view → `app/chamados/services/fio.py:625`, guarda `:566` |
| Validar/recusar/corrigir no detalhe | `app/templates/chamados/partials/_pergunta_card.html:93`, `:110` | `app/chamados/urls.py:14` → `app/chamados/views.py:359`; guarda área `:420`, despachos `:483`, `:505`, `:523` |

Atenção materialização: `posto_dropdown` e `escala_dropdown` não têm ramo em `app/chamados/services/materializacao.py:1226`; caem na recusa de ausência de efeito em `app/chamados/services/materializacao.py:1239`. Isso é recusa de implementação/estado para todos os perfis, não recusa de permissão. Rejeitar resposta segue disponível, conforme despacho `app/chamados/services/fio.py:637`.

Quem escreve / quem lê:

| Campo | Escritor | Leitor |
|---|---|---|
| `motivo`, `tipo_resposta`, opções | catálogo `app/chamados/catalogo/perguntas.py:34`; emissor lê em `app/chamados/services/disputa_emissao.py:314`, grava na abertura `:439` e `:448` | tipo lido no template `app/templates/chamados/partials/modal_fio.html:300` e materializador `app/chamados/services/materializacao.py:1226` |
| `resposta_colab`, `respondida_em` | `app/chamados/services/disputa_emissao.py:1467` | template `app/templates/chamados/partials/modal_fio.html:217`; validação `app/chamados/services/validacao.py:393` |
| `validada_em`, `validada_por` | carimbo `app/chamados/services/validacao.py:134`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1480` | validação `app/chamados/services/validacao.py:396`; template `app/templates/chamados/partials/modal_fio.html:228` |
| permissão `rbac_*` | semeadura `app/colaboradores/mixins.py:354`; supervisor inclui gerir_chamados `:362` | `tem_acao`, `app/colaboradores/mixins.py:322`; consulta `get_all_permissions` `:331` |
| departamento/dono | catálogo `app/chamados/catalogo/modulos.py:103`; mapa dono→departamento `app/chamados/utils.py:27` | `departamento_do_chamado`, `app/chamados/utils.py:32`; `pode_agir_chamado`, `:68` |

Script AST executado sem escrever arquivos:

```bash
python3 -B - <<'PY'
import ast
from pathlib import Path

for rel in [
    'app/chamados/services/validacao.py',
    'app/chamados/services/materializacao.py',
]:
    tree = ast.parse(Path(rel).read_text())
    found = [
        (n.lineno, ast.unparse(n.func))
        for n in ast.walk(tree)
        if isinstance(n, ast.Call)
        and any(k in ast.unparse(n.func)
                for k in ['tem_acao', 'has_perm', 'get_perfil', 'tem_perfil'])
    ]
    print(rel, 'chamadas RBAC:', found)
PY
```

Saída: `[]` em ambos. Nenhum teste/runtime/produção executado; permissões efetivas de usuários permanecem [não verificado]. [nao verificado]

## 11/11 20:30 · sessao 0db0d6465a77 · - · - · origem=Ronald (tmux)

AST em memória confirmou que `PerguntaDisputa`, declarada entre `app/chamados/models.py:2406` e `app/chamados/models.py:3546`, não declara `criado_em` nem `criada_em`; `materializada_em` é carimbo de processamento, atribuído em `app/chamados/models.py:3099`, não instante de nascimento (`app/chamados/models.py:3393`). A hora de emissão da pk52340 permanece `[nao verificado]`.

O pareador ordena por `timestamp`, abre em E e fecha na S posterior, incluindo travessia da meia-noite (`app/ponto/turnos.py:1217`, `app/ponto/turnos.py:1419`, `app/ponto/turnos.py:1491`). O `Turno` fornecido para 142873→143189 representa fechado e `cross_meianoite=True`, conforme a ordem dos campos e sua construção (`app/ponto/turnos.py:43`, `app/ponto/turnos.py:321`). A correspondência concreta com essas pks vem somente da sonda fornecida `[nao verificado]`.

| Campo | Escritor da cópia | Leitor relevante |
|---|---|---|
| Pergunta `motivo` | `_nascer`, `app/chamados/models.py:3014` | identidade, `app/chamados/services/fio.py:1460` |
| `data_evento` | `gravar_dia_do_fato`, `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016` | `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495` |
| `celula` | busca pelo dia do fato em `app/chamados/models.py:2896`; nascimento em `app/chamados/models.py:3014`; reuso em `app/chamados/models.py:2966`; rechave em `app/chamados/services/fio.py:1038` | `app/chamados/juizes.py:1331` |
| `colaborador` | identidade no nascimento, `app/chamados/models.py:3430` | `app/chamados/services/fio.py:1459` |
| `instancia_marco` | `app/chamados/models.py:3437`, `app/chamados/models.py:3440`, usando `app/chamados/services/declaracao_texto.py:55` | `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495` |
| `batida_origem` | `app/chamados/models.py:3448`; família MARCO retorna `None` em `app/chamados/models.py:3461` | `app/chamados/services/fio.py:1487` |
| `conversa` | porta `abrir` passa False, `app/chamados/models.py:2789`; `abrir_conversa` passa True, `app/chamados/models.py:2801`; persistência em `app/chamados/models.py:3014` | `app/chamados/juizes.py:78`, `app/chamados/services/fio.py:1445` |
| `materializada_em` / `via_resolucao` | `app/chamados/models.py:3099`, `app/chamados/models.py:3101` | `app/chamados/juizes.py:1370` |
| Batida `timestamp` / `tipo` / `sequencia` | porta app passa em `app/api/views_core.py:645`, `app/api/views_core.py:650`; create em `app/ponto/registro_batida.py:157` | `timestamp`: `app/ponto/turnos.py:1217`; `tipo`: `app/ponto/turnos.py:1382`; `sequencia`: contador em `app/ponto/turnos.py:2387` |
| Batida `criado_em` | Django `auto_now_add`, `app/ponto/models.py:79` | guard de eco, `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362` |
| Batida `retratada_em` | `retratar_batida` passa anotação em `app/ponto/registro_batida.py:274`; grava por `setattr`/save em `app/ponto/registro_batida.py:252` | universo em `app/ponto/turnos.py:2876` |

`batidas_apuraveis` não filtra aprovação ou tipo: apenas retratação nula, colaborador e janela opcionais (`app/ponto/turnos.py:2876`, `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:2881`). `turnos_do_colab` não chama literalmente essa função: carrega batidas com retratação nula e janela com pad próprio, depois chega a `parear_turnos` (`app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1863`, `app/ponto/turnos.py:1992`). O cartório usa `batidas_apuraveis` na entrada da grade (`app/ponto/services/cartorio.py:1156`, `app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`); seus insumos entrada/saída vêm das lâmpadas, não de presença de tipos E/S (`app/ponto/services/cartorio.py:823`, `app/ponto/services/cartorio.py:845`).

RED proposto: fixture com E às 19:00 em D e S às 07:00 em D+1, ambas apuráveis, escala/célula noturna; congelar o relógio após a S e o prazo aplicável; chamar a porta emissora e exigir nenhuma pergunta `orfao_14h` para D, além de turno único fechado. Mutante: descartar a resposta fechada do pareador e decidir ausência de S apenas no dia civil D; precisa gerar a pergunta e matar o RED. Proposta não executada `[nao verificado]`. O teste existente comprova somente o pareamento 22–06, não a emissão (`app/ponto/tests/test_turnos.py:12`).

_4026 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
