# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 01:46** · publicado em **10/11/2026 01:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 01:46 · sessao 479a5d42a969 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Cura escrita, com certificação pendente do Code/opus [nao verificado]. O corte usa somente a lâmpada `nunca_bateu`, centralizada em `dia_so_na_c5`; a fábrica devolve `[]` antes de emitir pergunta ou pauta de falta de turno (`chamados/services/fabrica_da_celula.py:67,85,163`).

Referências abaixo relativas a `app/`.

| Arquivo tocado | Motivo |
|---|---|
| `chamados/services/fabrica_da_celula.py:67` | Regra única de destino C5. |
| `chamados/services/disputa_emissao.py:757` | Mesmo corte no repasse de órfãos, antes do atalho da ata. |
| `chamados/services/resposta_tardia.py:46` | Impede reabrir chamado de dia destinado à C5. |
| `chamados/services/pergunta_no_app.py:35` | Nomeia a retenção e seu destino administrativo. |
| `chamados/management/commands/disparar_perguntas_competencia.py:22` | Exclui `nunca_bateu` dos alvos e calcula o contador irmão. |
| `chamados/management/commands/vigiar_fio_morto.py:182` | Publica número, esperado e diagnóstico no carimbo. |
| `core/management/commands/pipeline_placar.py:85` | Compartilha a leitura por dia, valida inteiros e alarma a divergência com dono CADASTRO. |
| `chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:164` | Selos executáveis sem Django, RED da base e oito mutantes. |
| `chamados/tests/test_e4a2_nunca_bateu.py:19` | Selo de integração com ORM real, preparado para a suíte. |
| `core/tests/test_fio_mudo_placar.py:139` | Integra o novo contador aos testes de carimbo e placar. |
| `chamados/tests/test_o261_vigia_fio.py:811` | Atualiza a conferência integral do carimbo. |
| `chamados/tests/test_e4a2_disparar_e_vigia.py:192` | Acrescenta o contador à fixture do relatório. |
| `chamados/tests/test_o261_fabrica_le_a_lampada.py:84` | Inverte somente a expectativa de `nunca_bateu`, conforme o aval. |

O vizinho que esperava retorno à fábrica para **todo** veredito acusador falhou com `'nunca_bateu_na_lista_c5' is not None`; agora preserva essa expectativa para os demais vereditos (`chamados/tests/test_o261_fabrica_le_a_lampada.py:84`). A fixture do relatório também quebrou por ausência de `cnb`; após o ajuste, seus dois testes por AST passaram (`chamados/tests/test_e4a2_disparar_e_vigia.py:192,218`).

O contador compara **pares**, não apenas totais: `numero` é a quantidade de dias únicos com chamado vivo excluídos dos alvos; `esperado` é quantos desses mesmos pares aparecem na função real `colabs_nunca_bateu`; `fora_da_c5` identifica os ausentes (`chamados/management/commands/disparar_perguntas_competencia.py:22,38,40`).

As janelas diferem: o disparar usa competência global; a C5 começa na competência anterior **por empresa** e termina ontem. A comparação usa explicitamente a interseção dessas janelas. Hoje continua excluído dos alvos `nunca_bateu`, mas fica fora dessa comparação (`chamados/management/commands/disparar_perguntas_competencia.py:24,34,58,79`; `escala/services/cadastro_realidade.py:177`).

| Campo/valor | Quem escreve ou fornece | Quem lê no caminho alterado |
|---|---|---|
| `CelulaDia.veredito` | Porta da lavra, `ponto/portas/celula.py:983,1044`; ramo sem adesão, `ponto/services/cartorio.py:1173,1252` | Regra única, `fabrica_da_celula.py:67`; C5, `cadastro_realidade.py:180` |
| Colaborador/data da célula | Porta de criação, `ponto/portas/celula.py:863` | Seleção e dedup do contador, `disparar_perguntas_competencia.py:69,80,37` |
| `Chamado.status_local` | Nascimento/transição, `chamados/models.py:1054,228` | Universo `VIVOS`, `disparar_perguntas_competencia.py:69` |
| `hoje`, janela e empresa | Relógio da rodada, `vigiar_fio_morto.py:165`; autoridades de janela, `ponto/janelas.py:60,66` | Contador e C5, `disparar_perguntas_competencia.py:31`; `cadastro_realidade.py:174` |
| Lista C5 e retenção do desligado | Lista real, `cadastro_realidade.py:137,200`; retrato lavrado, `folha/management/commands/lavrar_retrato_folha.py:74` | Interseção do esperado, `disparar_perguntas_competencia.py:38` |
| `nome`, `numero`, `esperado`, `dono`, `fora_da_c5`, `universo` | `disparar_perguntas_competencia.py:40` | Relatório, `:183`; carimbo, `vigiar_fio_morto.py:232,239,240` |
| `censo/esperado.nunca_bateu_sem_adesao` e `hoje` do carimbo | `vigiar_fio_morto.py:228,232,239` | Placar: mesmo dia, inteiros e comparação, `pipeline_placar.py:95,100,106,136` |
| Causa `nunca_bateu_na_lista_c5` | `disputa_emissao.py:759` | Retenção e catálogo de apresentação, `pergunta_no_app.py:35,101` |

Os leitores executáveis de `VEREDITOS_ACUSAM` encontrados por `git grep` ficaram classificados assim:

| Classe | Leitores |
|---|---|
| Emissão de pergunta: receberam corte | `fabrica_da_celula.py:100`; `disputa_emissao.py:772`, alcançado pelo corte em `:757` |
| Reabertura de chamado: recebeu corte | `resposta_tardia.py:46` |
| Vitalidade; sem nova emissão nesse leitor | `chamados/juizes.py:1321,1374`; `chamados/models.py:3357,3358` |
| Absolvição, fechamento ou retenção de filhos | `chamados/reconciliador.py:641,1027`; `services/silencio_ausencia.py:41`; `services/limbo_folgas.py:194` |
| Auditoria e contagem | `services/vigia_fio.py:331`; `auditar_invariantes_chamados.py:198,343,1391,1605,1647` |
| Folha, vigias e relatório | `folha/export.py:1233`; `folha/services/vigia_pausa.py:155`; `ponto/services/vigia_ausencia_veredito.py:41`; `ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`; `relatorios/dia_da_frota.py:55` |

A porta ADESÃO abre ação de cadastro para supervisão/RH e declara expressamente que não há questionário para o colaborador responder; por isso não recebeu o corte de emissão de pergunta (`chamados/services/adesao.py:19,74,77`).

Os contraexemplos estão cobertos: desligado entra na C5 somente pela lavra que segura o dia; adesão posterior não reinterpreta célula antiga; ausência já lavrada como `concorde` sai da C5 (`chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:209,233`).

Comandos reproduzíveis:

```bash
selo_e4a2=chamados.tests.test_e4a2_nunca_bateu_sem_banco

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest "$selo_e4a2" -v

E4A2_BASE=43fb14698cc20dbb3802f2583a36792ce034d3ad \
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  "$selo_e4a2.NuncaBateuSemBancoTest.test_RED_zero_pergunta_um_item_c5" \
  "$selo_e4a2.NuncaBateuSemBancoTest.test_RED_cartorio_chama_fabrica_real_sem_emitir_e_c5_lista" -v

for mutante_e4a2 in \
  fabrica_sem_corte contador_conta_nunca_bateu esperado_literal \
  placar_sem_contador carimbo_esperado_literal carimbo_numero_literal \
  repasse_sem_corte tardia_sem_corte
do
  E4A2_MUTANTE="$mutante_e4a2" PYTHONDONTWRITEBYTECODE=1 \
    python3 -m unittest "$selo_e4a2.NuncaBateuSemBancoTest" -q
done
```

Saída observada: **13 testes OK**; base: **2 falhas de asserção**; oito mutantes: **rc 1 por asserções, sem ERROR**. Os alvos e a verificação das mesmas asserções estão em `chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:27,322`.

A tentativa abaixo parou antes dos testes com `ModuleNotFoundError: No module named 'django'` (`manage.py:11`). Não houve execução de banco; a suíte Django pedida e os demais módulos vizinhos permanecem **[nao verificado]**. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  core.tests.test_fio_mudo_placar \
  chamados.tests.test_o261_vigia_fio \
  chamados.tests.test_e4a2_disparar_e_vigia \
  chamados.tests.test_e4a2_nunca_bateu \
  --settings=config.settings.ci -v 2
```

## 10/11 01:29 · sessao 41d9b8add366 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Implementei A/B/C nestes arquivos:

- [pipeline_placar.py:85](/home/ronald/wt-codex-fio-e0-v4-e4a-2-012227/app/core/management/commands/pipeline_placar.py:85): recebe `dia`; fotografia de outra data retorna `FIO_MUDO NAO_MEDIDO`, informando as duas datas.
- [vigiar_fio_morto.py:228](/home/ronald/wt-codex-fio-e0-v4-e4a-2-012227/app/chamados/management/commands/vigiar_fio_morto.py:228): carimba `cfm['esperado']`; retirado o literal independente.
- [test_fio_mudo_placar.py:46](/home/ronald/wt-codex-fio-e0-v4-e4a-2-012227/app/core/tests/test_fio_mudo_placar.py:46): `TestCase`, imports normais, comandos por `call_command`, temporário em `/tmp`, consultas proibidas e oito mutantes percorridos por um teste. [nao verificado]

Sobre escritores, leitores e semântica:

- `hoje` nasce de `timezone.localdate(agora)` e é serializado pelo vigia; `_carimbar` publica o arquivo somente na rodada de frota (`app/chamados/management/commands/vigiar_fio_morto.py:167`, `:225`, `:137`, `:222`).
- O único definidor de `esperado.fio_mudo_alvos` passa a ser o contador; o vigia transporta seu retorno (`app/chamados/management/commands/disparar_perguntas_competencia.py:88`; `app/chamados/management/commands/vigiar_fio_morto.py:228`).
- O placar lê ambas as chaves; `fio_estado` compara todas as entradas de `esperado` e apresenta `hoje` (`app/core/management/commands/pipeline_placar.py:94`; `bin/fio_estado.py:125`, `:144`).
- `hoje` representa dia civil local. A competência é uma janela derivada desse dia (`app/chamados/management/commands/vigiar_fio_morto.py:167`; `app/chamados/management/commands/disparar_perguntas_competencia.py:32`). Há uma diferença herdada de relógio: o placar sem `--dia` usa `date.today()` do processo, enquanto o vigia usa o fuso Django; equivalência dos fusos em produção é [nao verificado] (`app/core/management/commands/pipeline_placar.py:147`; `app/config/settings/base.py:93`).
- `placar(dia)` passado exige fotografia daquele dia. O leitor continua abrindo o mesmo arquivo; se ele contiver outro dia, retorna `NAO_MEDIDO`, sem reconstruir histórico (`app/core/management/commands/pipeline_placar.py:92`; `app/core/tests/test_fio_mudo_placar.py:148`).

Comando da suíte solicitada, incluindo os testes encontrados de `pipeline_placar`: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  core.tests.test_fio_mudo_placar \
  chamados.tests.test_o261_vigia_fio \
  chamados.tests.test_e4a2_disparar_e_vigia \
  core.tests.test_crons_quebrados \
  chamados.tests.test_contract_crons \
  --settings=config.settings.ci -v 2
```

A tentativa parou antes da coleta: `ModuleNotFoundError: No module named 'django'`. Portanto, suíte, RED e mordidas não executaram; nenhum resultado funcional está certificado [nao verificado]. [nao verificado]

Comando dos casos RED — alvo positivo, fotografia de outro dia e esperado vindo do contador (`app/core/tests/test_fio_mudo_placar.py:128`, `:136`, `:187`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  core.tests.test_fio_mudo_placar.FioMudoPlacarTest.test_RED_alvo_um_alarma_e_sai_dois_com_chave \
  core.tests.test_fio_mudo_placar.FioMudoPlacarTest.test_RED_carimbo_de_outro_dia_nao_mede_o_dia_do_placar \
  core.tests.test_fio_mudo_placar.FioMudoPlacarTest.test_RED_carimbo_publica_esperado_do_contador_da_rodada \
  --settings=config.settings.ci -v 2
```

O comando abaixo percorre **cada um** dos oito mutantes e exige a mensagem da asserção pretendida (`app/core/tests/test_fio_mudo_placar.py:257`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  core.tests.test_fio_mudo_placar.FioMudoPlacarTest.test_mutantes_em_memoria_sao_rejeitados_pela_assercao_pretendida \
  --settings=config.settings.ci -v 2
```

| Mutante — declaração em `app/core/tests/test_fio_mudo_placar.py:32` | Mordeu? |
|---|---|
| `leitor_ignora_hoje` | [nao verificado] |
| `leitor_ignora_divergencia` | [nao verificado] |
| `carimbo_esperado_literal` | [nao verificado] |
| `ausencia_lida_como_zero` | [nao verificado] |
| `leitor_limiar_proprio` | [nao verificado] |
| `placar_rc_zero` | [nao verificado] |
| `carimbo_numero_literal` | [nao verificado] |
| `placar_sem_leitor` | [nao verificado] |

Compilação em memória dos três arquivos e dos oito mutantes passou; `git diff --check` não apontou problemas. Certificação pela suíte: [nao verificado]. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ruff check --no-cache \
  core/management/commands/pipeline_placar.py \
  chamados/management/commands/vigiar_fio_morto.py \
  core/tests/test_fio_mudo_placar.py
```

Ruff não executou: `No module named ruff`; limpeza Ruff [nao verificado]. RED na base e verificações com banco também ficaram sem rodar [nao verificado].

## 10/11 01:29 · sessao 5bfff31eecde · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · verificador · origem=Code

**Parecer: cura parcial.** O comando de suporte e os quatro bloqueadores de testes foram corrigidos, mas `ESTEIRA` ainda admite formas que o leitor reconhece e a guarda libera; também persistem reservas mais amplas que os leitores de algumas famílias (`app/pautas/management/commands/suporte_hasner.py:134`; `app/pautas/tests/test_prefixos_reservados.py:243`; `app/core/management/commands/alarme_esteira.py:15`; `app/pautas/marcadores.py:30`; `app/pautas/services.py:48`).

**1. Achados PROCEDENTES da triagem, um por um**

| Achado | Situação na volta 3 |
|---|---|
| `casefold` bloqueava caixa que os leitores não reconhecem | **Curado no código:** a guarda usa `strip()` e comparação sensível à caixa, sem `casefold` (`app/pautas/services.py:46`; `app/pautas/services.py:48`). |
| Reserva bruta `ESTEIRA*` bloqueava prosa | **Parcialmente curado:** `ESTEIRANTE` e `ESTEIRA de compras` passam, mas a nova expressão não representa todas as chaves aceitas pelo leitor. Detalhamento no item 2 (`app/pautas/marcadores.py:30`; `app/core/management/commands/alarme_esteira.py:15`; `app/core/management/commands/alarme_esteira.py:34`). |
| `suporte_hasner --responder` sem guarda | **Curado:** valida antes de responder/fechar e converte `ValidationError` em `CommandError` (`app/pautas/management/commands/suporte_hasner.py:134`; `app/pautas/management/commands/suporte_hasner.py:139`). |
| Censo dependia de `git ls-files` | **Curado:** percorre a árvore com `os.walk`, lê os arquivos e propaga erro de diretório (`app/pautas/tests/test_prefixos_reservados_ast.py:29`; `app/pautas/tests/test_prefixos_reservados_ast.py:34`; `app/pautas/tests/test_prefixos_reservados_ast.py:43`). |
| Selo legado exigia declaração na fábrica | **Curado:** admite declaração/inclusão no catálogo e importação na fábrica; mantém a restrição dos usos operacionais (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:160`; `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:164`; `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:265`). |
| Mutantes executados somente em `__main__` | **Curado:** uma classe coletável `SimpleTestCase` chama o executor e reprova sobreviventes (`app/pautas/tests/test_prefixos_reservados.py:243`; `app/pautas/tests/test_prefixos_reservados.py:246`). |
| Skip engolia `ImportError` de produção | **Curado para imports de produção:** eles ficaram fora do `try`. Resta a ressalva sobre erro interno ao próprio `import django`, descrita no item 5 (`app/pautas/tests/test_prefixos_reservados.py:225`; `app/pautas/tests/test_prefixos_reservados.py:239`). |

A dualidade prosa/identidade continua no campo `texto`; não houve separação técnica dessa identidade nesta obra (`app/pautas/models.py:89`; `app/ponto/services/pautas_do_esmeril.py:67`).

**2. Escritores, leitores e formas exatas**

A guarda examina `texto.strip()`; o escritor comum também aplica `strip()` antes de persistir. Os leitores abaixo não aplicam normalização equivalente antes da seleção. O comportamento efetivo de collation no banco de produção permanece **[nao verificado]** (`app/pautas/services.py:46`; `app/pautas/services.py:180`; `app/chamados/services/fabrica_da_celula.py:305`).

Na tabela, **P** é o prefixo exato da primeira coluna; “igual” compara o início do texto após o `strip` do escritor. As chamadas de escrita desembocam no escritor físico comum (`app/pautas/services.py:197`).

| Prefixo reservado P | Quem escreve | Quem lê e forma casada | Guarda × leitor |
|---|---|---|---|
| `LIMBO EM COMPETENCIA EXPORTADA` | `app/chamados/services/fabrica_da_celula.py:274` | `P`; depois procura `[motivo:…]` (`app/chamados/services/fabrica_da_celula.py:254`; `app/chamados/services/fabrica_da_celula.py:255`) | Igual ao filtro inicial. |
| `CADASTRO: PASSADO SEM TURNO PREVISTO` | Acrescenta `:` após P (`app/chamados/services/fabrica_da_celula.py:362`) | `P`, **sem exigir `:` final** (`app/chamados/services/fabrica_da_celula.py:305`) | Igual. |
| `[ORFA-ENTERRADA]` | `app/chamados/management/commands/reabrir_orfas_enterradas.py:209` | `P` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:202`) | Igual. |
| `[PASSIVO-VALIDADO]` | Cabeça e filhas (`app/chamados/services/passivo_validado.py:230`; `app/chamados/services/passivo_validado.py:233`) | `P col<pk> <AAAA-MM>` (`app/chamados/services/passivo_validado.py:159`; `app/chamados/services/passivo_validado.py:164`) | Guarda mais ampla. |
| `[PASSIVO-VALIDADO-FORA]` | `app/chamados/services/passivo_validado.py:267` | `P col<pk> <AAAA-MM>` (`app/chamados/services/passivo_validado.py:254`; `app/chamados/services/passivo_validado.py:256`) | Guarda mais ampla. |
| `ARVORE VERMELHA` | Acrescenta `:` após P (`app/core/management/commands/alarme_arvore.py:52`) | `P`, sem exigir `:` (`app/core/management/commands/alarme_arvore.py:16`) | Igual; captura também prosa. |
| `ESTEIRA` | `P <chave>: …` (`app/core/management/commands/alarme_esteira.py:56`) | `P <chave>:`; chave não é validada como token (`app/core/management/commands/alarme_esteira.py:15`; `app/core/management/commands/alarme_esteira.py:34`) | **Diverge:** guarda exige `ESTEIRA [^\s:]+:` (`app/pautas/marcadores.py:30`). |
| `[CRON QUEBRADO]` | `app/core/services/crons_quebrados.py:68`; `app/core/services/crons_quebrados.py:72` | `P <nome> (empresa <emp-ou->)` (`app/core/services/crons_quebrados.py:54`) | Guarda mais ampla. |
| `[FERIAS/PA-VENCIDO]` | `MARCADORES[codigo]` (`app/ferias/management/commands/pautas_de_ferias.py:145`; `app/ferias/management/commands/pautas_de_ferias.py:164`) | `P` (`app/ferias/management/commands/pautas_de_ferias.py:152`) | Igual. |
| `[FERIAS/PA-VENCENDO]` | Mesmo escritor parametrizado (`app/ferias/management/commands/pautas_de_ferias.py:145`; `app/ferias/management/commands/pautas_de_ferias.py:164`) | `P` (`app/ferias/management/commands/pautas_de_ferias.py:152`) | Igual. |
| `[FERIAS/CADASTRO]` | Mesmo escritor parametrizado (`app/ferias/management/commands/pautas_de_ferias.py:145`; `app/ferias/management/commands/pautas_de_ferias.py:164`) | `P` (`app/ferias/management/commands/pautas_de_ferias.py:152`) | Igual. |
| `[PRE-FECHAMENTO]` | `app/folha/management/commands/pre_fechamento.py:111` | `P` para encerramento; `P <AAAA-MM-DD>` para deduplicação diária (`app/folha/management/commands/pre_fechamento.py:92`; `app/folha/management/commands/pre_fechamento.py:98`) | Igual ao leitor amplo; mais ampla que o diário. |
| `[DIA EM ABERTO]` | `app/pautas/management/commands/pauta_dia_em_aberto.py:140` | `P` (`app/pautas/management/commands/pauta_dia_em_aberto.py:108`) | Igual. |
| `[FOLGA NAO DEFINIDA - EXPORTADA]` | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:135` | `P` (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:126`) | Igual. |
| `[PAGO X DEVIDO - EXPORTADA]` | Cabeça e filhas (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:208`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:212`) | Chave `P col<pk> <AAAA-MM>` seguida de espaço; filhas acrescentam ` > `; reversão usa Python `startswith(P)` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:54`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:243`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:250`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:277`) | Igual ao leitor da reversão; mais ampla que os leitores da chave. |
| `[A6/REVISAO-SEMANAL]` | Dono fornece P ao escritor semanal (`app/ponto/services/vigia_ausencia.py:239`; `app/pautas/semanal.py:278`) | `P <AAAA-Wss>` para semana; `P` para fechar a família (`app/pautas/semanal.py:199`; `app/pautas/semanal.py:228`) | Igual ao leitor amplo; mais ampla que o semanal. |
| `[SUP/REVISAO-SEMANAL]` | Dono fornece P ao escritor semanal (`app/escala/services/fase_12x36.py:278`; `app/pautas/semanal.py:278`) | Mesmos leitores parametrizados (`app/pautas/semanal.py:199`; `app/pautas/semanal.py:228`) | Igual ao leitor amplo; mais ampla que o semanal. |
| `Bateu ponto durante afastamento. A batid` | Emite `TEXTO_AFASTADO`, com possível complemento (`app/ponto/services/afastado_avisa.py:76`) | Primeiros **40 caracteres**, P (`app/pautas/marcadores.py:81`; `app/ponto/services/afastado_avisa.py:63`) | Igual; captura também prosa. |
| `[HE-AUTORIZADA-EM-EXPORTADA]` | `app/ponto/services/autorizacao_he_periodo.py:245` | `P` (`app/ponto/services/autorizacao_he_periodo.py:242`) | Igual. |
| `[COBERTURA-A-CONFIRMAR]` | `app/ponto/services/cobertura.py:336` | `P` (`app/ponto/services/cobertura.py:243`) | Igual. |
| `[APP-401]` | `app/ponto/services/pautas_dos_deslogados.py:78` | `P` (`app/ponto/services/pautas_dos_deslogados.py:66`) | Igual. |
| `[DISCORDANTE]` | `app/ponto/services/pautas_dos_discordantes.py:180` | ORM e Python usam `P`; depois divide no primeiro `:` e aplica `strip` à chave (`app/ponto/services/pautas_dos_discordantes.py:166`; `app/ponto/services/pautas_dos_discordantes.py:98`; `app/ponto/services/pautas_dos_discordantes.py:100`) | Igual ao reconhecimento inicial. |
| `[TRANCA]` | `app/chamados/services/competencia_trancada.py:235` | `P` (`app/chamados/services/competencia_trancada.py:221`) | Igual. |
| `[A4/SEM-DOCUMENTO]` | `app/ponto/services/ausencia.py:1060` | `P` (`app/ponto/services/ausencia.py:1044`) | Igual. |
| `[A1-FERIAS/AG#` | Completa com `<agendamento.pk>]` (`app/ponto/services/ausencia.py:1231`; `app/ponto/services/ausencia.py:1250`) | `P<agendamento.pk>]` (`app/ponto/services/ausencia.py:1234`) | Guarda mais ampla. |
| `[ESMERIL ` — **espaço final** | Completa com `<ação>]` (`app/ponto/services/pautas_do_esmeril.py:16`; `app/ponto/services/pautas_do_esmeril.py:50`; `app/ponto/services/pautas_do_esmeril.py:79`) | Seleciona `P`; extrai ação até `]`, sem exigir que esse fechamento exista (`app/ponto/services/pautas_do_esmeril.py:66`; `app/ponto/services/pautas_do_esmeril.py:67`) | Igual ao reconhecimento inicial. |

**Lista A — leitor reconhece, guarda deixa passar:**

- `ESTEIRA a b: relato`: o comando aceita `chave='a b'`, o escritor produz esse início e o leitor procura exatamente `ESTEIRA a b:`; o regex da guarda falha no espaço interno (`app/core/management/commands/alarme_esteira.py:34`; `app/core/management/commands/alarme_esteira.py:56`; `app/core/management/commands/alarme_esteira.py:15`; `app/pautas/marcadores.py:30`).
- `ESTEIRA :a: relato`: a chave `:a` também é aceita; o regex exige um caractere diferente de `:` antes do primeiro delimitador (`app/core/management/commands/alarme_esteira.py:34`; `app/core/management/commands/alarme_esteira.py:15`; `app/pautas/marcadores.py:30`).

A sonda em memória das funções reais devolveu **200 e uma escrita simulada** para ambos. Isso demonstra o desvio na porta isolada; existência de segunda cabeça viva em produção: **[nao verificado]** (`app/pautas/tests/test_prefixos_reservados.py:34`; `app/pautas/views.py:76`; `app/pautas/views.py:88`).

`ESTEIRA a:b:` **é barrado**: o regex já casa `ESTEIRA a:`, pois não exige fim da string (`app/pautas/marcadores.py:30`; `app/pautas/services.py:47`).

**Lista B — guarda barra formas que nenhum leitor encontrado daquela família casa:**

| Exemplo barrado | Parte exigida pelo leitor que está ausente |
|---|---|
| `[PASSIVO-VALIDADO] relato` | ` col<pk> <AAAA-MM>` (`app/pautas/services.py:48`; `app/chamados/services/passivo_validado.py:159`; `app/chamados/services/passivo_validado.py:164`). |
| `[PASSIVO-VALIDADO-FORA] relato` | ` col<pk> <AAAA-MM>` (`app/pautas/services.py:48`; `app/chamados/services/passivo_validado.py:254`; `app/chamados/services/passivo_validado.py:256`). |
| `[CRON QUEBRADO] relato` | Nome e trecho ` (empresa …)` (`app/pautas/services.py:48`; `app/core/services/crons_quebrados.py:54`). |
| `[A1-FERIAS/AG# sem fechamento` | Identificador seguido de `]` (`app/pautas/services.py:48`; `app/ponto/services/ausencia.py:1231`; `app/ponto/services/ausencia.py:1234`). |

`ARVORE VERMELHA no jardim` e prosa iniciada pelos 40 caracteres de `AFASTADO` ficam corretamente reservadas **segundo os leitores atuais**, que também as capturam (`app/core/management/commands/alarme_arvore.py:16`; `app/ponto/services/afastado_avisa.py:63`).

**3. Portas humanas**

| Porta | Resultado |
|---|---|
| Criar cabeça por HTTP | Passa pela guarda antes da escrita (`app/pautas/views.py:76`; `app/pautas/views.py:77`). |
| Responder por HTTP | Passa pela guarda antes da resposta (`app/pautas/views.py:114`; `app/pautas/views.py:115`). |
| Compositor e formulário de resposta | Encaminham às respectivas rotas guardadas (`app/templates/core/_copiloto.html:544`; `app/templates/core/_copiloto.html:462`; `app/pautas/urls.py:9`; `app/pautas/urls.py:13`). |
| `suporte_hasner --responder` | Passa pela guarda antes de responder ou fechar (`app/pautas/management/commands/suporte_hasner.py:134`). |
| API de mensageria examinada | Apenas consulta e serializa pautas (`app/api/views_mensageria.py:1323`; `app/api/views_mensageria.py:1331`; `app/api/views_mensageria.py:1340`). |
| Formulário de edição | Não há rota de edição no URLconf de Pauta (`app/pautas/urls.py:7`). Outra rota dinâmica: **[nao verificado]**. |
| Admin, ModelForm ou serializer de escrita registrado dinamicamente | Não localizado no censo; ausência universal: **[nao verificado]**. |

**Caminhos que não passam:**

- **Shell chamando diretamente `services.escrever` ou `services.responder`:** a primeira função não invoca a guarda; a segunda delega à primeira. Uso humano efetivo em produção: **[nao verificado]** (`app/pautas/services.py:170`; `app/pautas/services.py:180`; `app/pautas/services.py:197`; `app/pautas/services.py:209`).
- **ORM direto pelo shell:** o campo não declara validador reservado e o modelo não apresenta `clean`/`save` próprios. Uso efetivo: **[nao verificado]** (`app/pautas/models.py:89`; `app/pautas/models.py:110`; `app/pautas/models.py:136`).

Os comandos emissores `alarme_arvore` e `alarme_esteira` também não chamam a guarda, mas constroem o início com seus próprios marcadores antes dos argumentos fornecidos (`app/core/management/commands/alarme_arvore.py:52`; `app/core/management/commands/alarme_esteira.py:56`).

**4. Valores e imports**

O censo acima identifica **21 arquivos donos**, portanto a premissa de “24 módulos” não corresponde aos arquivos enumerados; são **26 prefixos reservados**, incluindo as três entradas de `FERIAS` (`app/pautas/marcadores.py:36`; `app/pautas/marcadores.py:90`).

**Comparando os valores removidos no diff fornecido com o catálogo atual, não há alteração dos marcadores.** As transformações menos triviais também preservam o resultado:

- `TEXTO[:40]` passou a `AFASTADO = TEXTO_AFASTADO[:40]` (`app/pautas/marcadores.py:61`; `app/pautas/marcadores.py:81`; `app/ponto/services/afastado_avisa.py:11`).
- `[A1-FERIAS/AG#%s]` continua sendo o prefixo aberto mais `%s]` (`app/pautas/marcadores.py:85`; `app/ponto/services/ausencia.py:1231`).
- `[ESMERIL %s]` continua sendo `[ESMERIL ` mais `%s]`, preservando o espaço (`app/pautas/marcadores.py:88`; `app/ponto/services/pautas_do_esmeril.py:16`).
- `[TRANCA]` permanece o mesmo início do texto e da consulta (`app/pautas/marcadores.py:78`; `app/chamados/services/competencia_trancada.py:221`; `app/chamados/services/competencia_trancada.py:230`).

Essa conclusão usa o **diff fornecido**; comparação independente dos objetos Git de base/HEAD e autenticação do hash da cópia: **[nao verificado]**.

**Não há ciclo novo através do catálogo:** ele contém dados e derivações locais, sem imports; `pautas.__init__` está vazio. A guarda importa o catálogo, sem carregar os donos (`app/pautas/marcadores.py:1`; `app/pautas/marcadores.py:81`; `app/pautas/marcadores.py:90`; `app/pautas/services.py:32`). Execução completa dos imports com Django: **[nao verificado]**.

**5. Coleta, mutantes e skip**

**Estruturalmente coletáveis pela suíte Django, sem `.git`.** A suíte monta a árvore em `/app`, chama `manage.py test`, inclui `pautas`, e usa um runner derivado de `DiscoverRunner`; o censo novo percorre arquivos diretamente (`bin/suite.sh:100`; `bin/suite.sh:102`; `bin/regua.sh:23`; `app/config/settings/ci.py:40`; `app/core/runner_quarentena.py:72`; `app/pautas/tests/test_prefixos_reservados_ast.py:34`).

**Os mutantes agora participam da suíte:** `MutantesPrefixosTest` chama `executar_mutantes`; existe também teste que verifica essa chamada (`app/pautas/tests/test_prefixos_reservados.py:243`; `app/pautas/tests/test_prefixos_reservados.py:246`; `app/pautas/tests/test_prefixos_reservados_ast.py:186`).

Nas execuções isoladas, sem escrita de arquivos:

- Executor: **73 mortos, zero sobreviventes, rc 0**, com GREEN obrigatório e erros de infraestrutura não aceitos como morte dos mutantes de porta/selo (`app/pautas/tests/mutar_prefixos_reservados.py:173`; `app/pautas/tests/mutar_prefixos_reservados.py:191`; `app/pautas/tests/mutar_prefixos_reservados.py:195`; `app/pautas/tests/mutar_prefixos_reservados.py:227`).
- Dois módulos de testes: **19 testes, OK, um skip por Django indisponível** (`app/pautas/tests/test_prefixos_reservados.py:225`; `app/pautas/tests/test_prefixos_reservados.py:229`).

Execução efetiva pelo runner Django, integração HTTP/banco e RED contra a base: **[nao verificado]**. O ramo CLI `--base` ainda exige `git show`, mas não participa da execução coletada dos mutantes (`app/pautas/tests/mutar_prefixos_reservados.py:236`).

**O skip não engole mais falhas de importação dos serviços, views ou models.** Ainda captura qualquer `ImportError` produzido durante o próprio `import django`, sem conferir sua origem; fora do runner, isso pode rotular um Django quebrado como ausente. No runner Django, a dependência já precisa carregar para iniciar a coleta (`app/pautas/tests/test_prefixos_reservados.py:225`; `app/pautas/tests/test_prefixos_reservados.py:239`; `app/core/runner_quarentena.py:15`).

**Há mordida real, mas cobertura incompleta:** o selo fiscaliza a origem de keywords `texto__startswith`, e as derivações consideram principalmente o começo da expressão. Ele não prova equivalência completa da forma reconhecida pela guarda. O desvio de `ESTEIRA` coexistiu com censo sem erros e os 73 mutantes mortos (`app/pautas/tests/test_prefixos_reservados_ast.py:144`; `app/pautas/tests/test_prefixos_reservados_ast.py:173`; `app/pautas/tests/mutar_prefixos_reservados.py:210`; `app/pautas/marcadores.py:30`; `app/core/management/commands/alarme_esteira.py:15`).

**6. Classes de reprovação**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando autoridade registrada | **Não identificado no domínio de cadastro sem turno:** o registro aponta para `pauta_cadastro_existente`, e o escritor continua consultando essa função (`app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:358`). |
| Segundo escritor fora da porta | **Nenhum escritor físico novo identificado:** criação e resposta continuam pelo serviço registrado. O shell pode contornar a guarda usando esse próprio serviço, conforme item 3 (`app/core/portas.py:510`; `app/pautas/services.py:197`; `app/pautas/services.py:211`). |
| Relógio fora da borda | **Não introduzido pela guarda:** ela depende somente do texto e do catálogo (`app/pautas/services.py:46`; `app/pautas/services.py:48`). |
| Derivação paralela de fonte única | **Presente na interpretação da forma de `ESTEIRA`:** a guarda codifica uma gramática própria, mais estreita que a aceita pelo comando/leitor. Os valores dos marcadores, por sua vez, estão centralizados (`app/pautas/marcadores.py:30`; `app/core/management/commands/alarme_esteira.py:15`; `app/core/management/commands/alarme_esteira.py:34`). |
| Selo/RED sem mordida | **Bloqueadores anteriores curados; cobertura da forma ainda insuficiente.** Há asserções e mutantes executáveis, mas eles não detectam o furo descrito (`app/pautas/tests/test_prefixos_reservados.py:243`; `app/pautas/tests/mutar_prefixos_reservados.py:191`; `app/pautas/tests/test_prefixos_reservados_ast.py:173`). |
| Ordem de trava | **Preserv

[truncado em 20000 de 20995 caracteres]

## 10/11 01:24 · sessao f84f7073acf7 · - · - · origem=Ronald (tmux)

As quatro curas da triagem estão implementadas:

| Achado | Verificação |
|---|---|
| Censo dependia de Git | Curado: `os.walk` enumera a árvore, exclui runtime/tests e propaga erros de diretório (`app/pautas/tests/test_prefixos_reservados_ast.py:29`, `:34`, `:42`). |
| Selo legado exigia constante na fábrica | Curado: permite declaração/inclusão somente no catálogo e import somente na fábrica; mantém restrição de usos operacionais (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:160`, `:162`, `:164`, `:265`). |
| Mutantes apenas em `__main__` | Curado: `MutantesPrefixosTest(SimpleTestCase)` chama o executor e exige mortos positivos/nenhum sobrevivente (`app/pautas/tests/test_prefixos_reservados.py:243`). |
| Skip engolia `ImportError` de produção | Curado: `try` contém somente `import django`; imports de produção estão no `else` (`app/pautas/tests/test_prefixos_reservados.py:225`, `:239`). |

Os testes são estruturalmente coletáveis: a suite inclui `pautas`, monta `app/` e executa `manage.py test`; o runner deriva de `DiscoverRunner`. Nenhum teste novo consta na lista de quarentena (`bin/regua.sh:23`; `bin/suite.sh:100`; `app/core/runner_quarentena.py:72`; `app/core/testes_em_quarentena.txt:6`).

O teste de coleta verifica dinamicamente que a classe chama o executor; há mutante que remove essa chamada e mutante que amplia o `try` para produção (`app/pautas/tests/test_prefixos_reservados_ast.py:186`; `app/pautas/tests/mutar_prefixos_reservados.py:121`).

Executei em modo somente leitura com `PYTHONDONTWRITEBYTECODE=1 python3 -B`: [nao verificado]

- Executor: rc 0, **73 mortos, zero sobreviventes**; resultado emitido pelo executor (`app/pautas/tests/mutar_prefixos_reservados.py:227`).
- Dois módulos `unittest`: rc 0, **19 testes, OK, um skip por Django indisponível** (`app/pautas/tests/test_prefixos_reservados.py:229`).

Execução efetiva no runner Django, HTTP/banco e RED contra a base: **[nao verificado]**. O modo `--base` ainda depende de `git show`, mas fica exclusivamente no ramo CLI; não participa da execução coletada dos mutantes (`app/pautas/tests/mutar_prefixos_reservados.py:236`).

O skip não esconde mais `ImportError` de `pautas.services`, models ou views. Tecnicamente, ainda captura qualquer `ImportError` produzido durante o próprio `import django`, sem conferir `erro.name`; não demonstra distinção entre pacote ausente e import interno quebrado (`app/pautas/tests/test_prefixos_reservados.py:225`). Isso não permite esconder falha de produção no runner Django já iniciado (`app/core/runner_quarentena.py:15`; `app/pautas/tests/test_prefixos_reservados.py:239`).

Há mordida real: o executor exige GREEN prévio e não aceita erros de infraestrutura como morte de mutantes de porta/selo (`app/pautas/tests/mutar_prefixos_reservados.py:173`, `:189`). O selo prova origem reservada dos keywords `texto__startswith`; não prova equivalência de sufixos, igualdade escritor/leitor, preservação byte a byte, Python `startswith`, `contains` ou regex (`app/pautas/tests/test_prefixos_reservados_ast.py:144`, `:173`; `app/pautas/tests/mutar_prefixos_reservados.py:205`).

Ordem preservada no escritor de cadastro: `atomic → trava → juiz → escrever`; contador reutiliza o seletor (`app/chamados/services/fabrica_da_celula.py:355`, `:356`, `:358`, `:360`, `:336`). A guarda não lê relógio (`app/pautas/services.py:46`).

## 10/11 01:24 · sessao df111cff54f0 · - · - · origem=Ronald (tmux)

3. **Portas humanas identificadas com texto livre estão guardadas:** criação HTTP valida antes de `sv.escrever`; resposta HTTP valida antes de `sv.responder`; suporte valida antes de responder/fechar e converte `ValidationError` em `CommandError` (`app/pautas/views.py:76`, `app/pautas/views.py:114`, `app/pautas/management/commands/suporte_hasner.py:134`, `app/pautas/management/commands/suporte_hasner.py:139`).

| Categoria | Resultado e evidência |
|---|---|
| Compositor/formulário | Envia para `/pautas/escrever/`, cuja view guarda o texto (`app/templates/core/_copiloto.html:544`; `app/pautas/views.py:76`). |
| Formulário de resposta | Envia para `/pautas/<id>/responder/`, cuja view guarda o texto (`app/templates/core/_copiloto.html:462`; `app/pautas/views.py:114`). |
| Formulário de edição de texto existente | Não há rota de edição no URLconf de Pauta; ausência universal de rota dinâmica: [nao verificado] (`app/pautas/urls.py:7`). |
| API de mensageria | A função examinada apenas lista/serializa pautas (`app/api/views_mensageria.py:1323`, `app/api/views_mensageria.py:1331`, `app/api/views_mensageria.py:1340`). |
| Django admin / serializers / ModelForms | Registro de `Pauta` não identificado no censo da cópia; ausência universal/dinâmica: [nao verificado]. O app config não registra modelos nem possui `ready` (`app/pautas/apps.py:4`). |
| Shell de suporte usando `suporte_hasner` | Guardado (`app/pautas/management/commands/suporte_hasner.py:134`). |
| Shell chamando diretamente `services.escrever` ou `services.responder` | **Não passa pela guarda**: `escrever` valida destinatário/âncora/vazio e grava, sem chamar `validar_texto_humano`; `responder` apenas delega a ele (`app/pautas/services.py:170`, `app/pautas/services.py:180`, `app/pautas/services.py:197`, `app/pautas/services.py:209`). Uso efetivo desse caminho humano em produção: [nao verificado]. |
| ORM direto pelo shell | Campo não declara validador reservado e modelo não define `clean`/`save` próprios; o caminho padrão permanece possível pelo código, uso real: [nao verificado] (`app/pautas/models.py:89`, `app/pautas/models.py:110`, `app/pautas/models.py:136`). |

**Emissores de sistema acionáveis por CLI** continuam sem guarda por desenho: `alarme_arvore` e `alarme_esteira` colocam seu próprio marcador antes dos argumentos, usando o escritor comum (`app/core/management/commands/alarme_arvore.py:50`, `app/core/management/commands/alarme_arvore.py:52`; `app/core/management/commands/alarme_esteira.py:54`, `app/core/management/commands/alarme_esteira.py:56`). Outro texto humano incorporado em Pauta é resposta tardia de chamado, mas recebe prefixo fixo `[RESPOSTA-TARDIA]` antes do conteúdo (`app/chamados/services/resposta_tardia.py:58`, `app/chamados/services/resposta_tardia.py:65`).

**Achado importante para pergunta 2: a cura de ESTEIRA ainda deixa formas reconhecidas passarem.** A guarda exige `ESTEIRA [^\s:]+:`; contudo, o leitor concatena qualquer `chave`, e o comando só aplica `strip`, sem impedir espaços internos ou `:` (`app/pautas/marcadores.py:30`; `app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:34`). Exemplo demonstrável: `--motivo 'a b'` produz/lê `ESTEIRA a b:`, forma que a guarda deixa passar; idem chave `a:b` produz/lê `ESTEIRA a:b:` (`app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:56`; `app/pautas/marcadores.py:30`). Existência dessas chaves em produção: [nao verificado].

**Classes:** escritor físico permanece `Pauta.objects.create` no serviço; resposta chama esse escritor e o registro de portas autoriza somente `pautas/services.py` (`app/pautas/services.py:197`, `app/pautas/services.py:211`; `app/core/portas.py:510`). A ordem da trava de cadastro continua `atomic → trava_na_ordem → pauta_cadastro_existente → escrever` (`app/chamados/services/fabrica_da_celula.py:355`, `app/chamados/services/fabrica_da_celula.py:356`, `app/chamados/services/fabrica_da_celula.py:358`, `app/chamados/services/fabrica_da_celula.py:360`).

## 10/11 01:21 · sessao aec38e1851e1 · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · construtor · origem=Code

Cura implementada: guarda sensível à caixa, `strip()` igual ao escritor e reserva ESTEIRA limitada a `ESTEIRA <token>:`. O suporte valida antes de responder e converte `ValidationError` em `CommandError` (`pautas/services.py:46`; `pautas/marcadores.py:30`; `pautas/management/commands/suporte_hasner.py:134`).

Os sete arquivos alterados:

- [marcadores.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-011046/app/pautas/marcadores.py:30): forma ESTEIRA no catálogo único.
- [services.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-011046/app/pautas/services.py:40): remove `casefold` e consulta a forma catalogada.
- [suporte_hasner.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-011046/app/pautas/management/commands/suporte_hasner.py:134): fecha a porta humana do comando.
- [test_prefixos_reservados.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-011046/app/pautas/tests/test_prefixos_reservados.py:140): RED das formas, comando, skip restrito ao import Django e mutantes coletáveis por `SimpleTestCase`.
- [test_prefixos_reservados_ast.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-011046/app/pautas/tests/test_prefixos_reservados_ast.py:29): enumera a árvore com `os.walk`, sem Git; verifica também coleta e limites do skip.
- [test_juiz_pauta_cadastro_sem_turno.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-011046/app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:157): admite declaração/registro no catálogo e import na fábrica; mantém usos operacionais restritos.
- [mutar_prefixos_reservados.py](/home/ronald/wt-codex-pauta-sem-turno-prefixo-reservado-011046/app/pautas/tests/mutar_prefixos_reservados.py:74): executor compartilhado com a coleta Django e novos mutantes.

O RED inicial, antes da cura, teve **68 falhas de asserção, zero erros**. O replay final contra `d2230cd…`, incluindo explicitamente `[TRANCA] x`, teve **69 falhas, zero erros** (`pautas/tests/test_prefixos_reservados.py:140`; `:161`; `pautas/tests/mutar_prefixos_reservados.py:236`).

Resultados da sonda, executando os corpos reais com persistência substituída (`pautas/tests/test_prefixos_reservados.py:35`; `:79`):

| Caso | Base | Atual |
|---|---|---|
| `Esteira de compras` | HTTP 400 | HTTP 200 |
| `esteira x:` | HTTP 400 | HTTP 200 |
| `ESTEIRA build:` | HTTP 400 | HTTP 400 |
| suporte: `[TRANCA] x` | sem CommandError; 1 escrita | CommandError substituído; 0 escritas e 0 fechamentos |

Comandos executados a partir de `app/`:

```bash
# Replay RED: saída testes=6 falhas=69 erros=0; rc 1 esperado [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados \
  --base d2230cd194a41ff9c79fb0e7773fb46a6dee44d6

# Depois: 19 testes descobertos; 18 passaram, 1 skip [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  pautas.tests.test_prefixos_reservados \
  pautas.tests.test_prefixos_reservados_ast

# Depois: 73 mortos; 0 sobreviventes; rc 0
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados
```

O executor imprime **o comando individual de cada um dos 73 mutantes** (`pautas/tests/mutar_prefixos_reservados.py:219`). O selo legado matou, entre outros, `legado_leitor_fora_do_dono`, que acrescenta uma leitura operacional no catálogo (`pautas/tests/mutar_prefixos_reservados.py:135`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m pautas.tests.mutar_prefixos_reservados \
  --mutante legado_leitor_fora_do_dono
```

Ruff retornou `All checks passed!` nos sete arquivos, com este comando:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache \
  pautas/marcadores.py pautas/services.py \
  pautas/management/commands/suporte_hasner.py \
  pautas/tests/test_prefixos_reservados.py \
  pautas/tests/test_prefixos_reservados_ast.py \
  pautas/tests/mutar_prefixos_reservados.py \
  chamados/tests/test_juiz_pauta_cadastro_sem_turno.py
```

Quanto ao censo: **o escritor persistente único encontrado é `Pauta.objects.create` em `pautas/services.py:197`**, após `strip()` em `:180`. Respostas passam pelo mesmo escritor (`pautas/services.py:211`). As portas humanas validam em `pautas/views.py:76`, `:114` e `pautas/management/commands/suporte_hasner.py:134`.

Na tabela abaixo, os caminhos partem de `app/`; E identifica o emissor e L o leitor:

| Forma reservada | Emissor e leitor |
|---|---|
| LIMBO / CADASTRO sem turno | E `chamados/services/fabrica_da_celula.py:270`, `:360`; L `:254`, `:305` |
| ORFA-ENTERRADA | E `chamados/management/commands/reabrir_orfas_enterradas.py:208`; L `:202` |
| PASSIVO-VALIDADO / FORA | E `chamados/services/passivo_validado.py:229`, `:266`; L `:164`, `:256` |
| TRANCA | E `chamados/services/competencia_trancada.py:234`; L `:221` |
| ARVORE VERMELHA | E `core/management/commands/alarme_arvore.py:50`; L `:16` |
| ESTEIRA `<token>:` | E `core/management/commands/alarme_esteira.py:54`; L `:15` |
| CRON QUEBRADO | E `core/services/crons_quebrados.py:71`; L `:54` |
| FERIAS: três famílias | E `ferias/management/commands/pautas_de_ferias.py:163`; L `:152` |
| PRE-FECHAMENTO | E `folha/management/commands/pre_fechamento.py:110`; L `:92`, `:98` |
| DIA EM ABERTO | E `pautas/management/commands/pauta_dia_em_aberto.py:139`, `:142`; L `:108` |
| FOLGA EXPORTADA | E `pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`, `:139`; L `:126` |
| PAGO X DEVIDO | E `pautas/management/commands/pauta_pago_x_devido_exportada.py:207`, `:211`; L `:243`, `:250`, `:277` |
| Revisão ausência / supervisão | Donos `ponto/services/vigia_ausencia.py:239` e `escala/services/fase_12x36.py:278`; E comum `pautas/semanal.py:276`; L `:199`, `:228` |
| AFASTADO, 40 caracteres | E `ponto/services/afastado_avisa.py:72`; L `:63` |
| HE EXPORTADA | E `ponto/services/autorizacao_he_periodo.py:244`; L `:242` |
| COBERTURA-A-CONFIRMAR | E `ponto/services/cobertura.py:333`; L `:243` |
| APP-401 | E `ponto/services/pautas_dos_deslogados.py:77`; L `:66` |
| DISCORDANTE | E `ponto/services/pautas_dos_discordantes.py:177`; L `:98`, `:166` |
| SEM-DOCUMENTO / FERIAS RECUSADA | E `ponto/services/ausencia.py:1058`, `:1248`; L `:1044`, `:1234` |
| ESMERIL | E `ponto/services/pautas_do_esmeril.py:78`; L `:66`, `:67` |

Os outros chamadores diretos encontrados emitem textos fora dessas reservas: aparelhos compartilhados e resposta tardia (`colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107`; `chamados/services/resposta_tardia.py:62`).

**Nenhum dos leitores listados usa `icontains`, `iexact`, `istartswith` ou regex sem caixa para identificar essas reservas**; usam `texto__startswith` ou `str.startswith`, conforme as referências da tabela. Respostas também precisam da guarda: há leitura explícita de filhas em `pautas/management/commands/pauta_pago_x_devido_exportada.py:250`; CADASTRO restringe a cabeças em `chamados/services/fabrica_da_celula.py:304`. Collation de produção e leitores dinâmicos: **[nao verificado]**.

Não rodaram integração Django, contagem real em banco, suíte completa nem A16 completo: **[nao verificado]**. O teste de zero Pauta está entregue em `pautas/tests/test_prefixos_reservados.py:287`; o skip por ausência de Django está em `:225`. Os 15 testes do selo legado passaram somente na execução isolada descrita em `pautas/tests/mutar_prefixos_reservados.py:46`. A certificação fica com Code/suíte e opus final.

## 10/11 01:15 · sessao 19d6c776708f · FIO-E0-V4-E4A-2 · verificador · origem=Code

**Parecer: REPROVADO para declarar a volta pronta.** O leitor novo aceita fotografia sem validar data ou origem, e os testes novos não provam sua integração real com Django. (`app/core/management/commands/pipeline_placar.py:85`; `app/core/tests/test_fio_mudo_placar.py:50`)

A comparação histórica abaixo usa o diff fornecido; conferência independente dos objetos Git dos hashes **[nao verificado]**. Execução da suíte e condições atuais de produção **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê o carimbo**

   O escritor operacional localizado é `vigiar_fio_morto.Command.handle`: monta o registro, chama `_carimbar` somente em frota e publica por arquivo `.novo` seguido de `os.replace`. O caminho aceita `settings.VIGIA_FIO_CARIMBO`; sem override, usa `<BASE_DIR>/logs/fio_morto.json`. (`app/chamados/management/commands/vigiar_fio_morto.py:105`; `app/chamados/management/commands/vigiar_fio_morto.py:130`; `app/chamados/management/commands/vigiar_fio_morto.py:222`)

   O inventário das chaves de topo e seus leitores operacionais é:

   | Chaves | Quem lê e para quê |
   |---|---|
   | `medido_em`, `hoje`, `banco` | `fio_estado` calcula idade/apresenta a identificação; o novo leitor do placar ignora esses campos. (`bin/fio_estado.py:127`; `bin/fio_estado.py:143`; `app/core/management/commands/pipeline_placar.py:91`) |
   | `schema` | É publicado; não participa dos leitores operacionais examinados. (`app/chamados/management/commands/vigiar_fio_morto.py:227`; `bin/fio_estado.py:143`; `app/core/management/commands/pipeline_placar.py:91`) |
   | `esperado`, `censo` | `fio_estado` compara **todas as entradas de `esperado`** com `censo`; o placar lê somente o par `fio_mudo_alvos`. (`bin/fio_estado.py:121`; `app/core/management/commands/pipeline_placar.py:94`) |
   | `gritou`, `alarma_em_0` | `fio_estado` apresenta o alarme da rodada e a situação do alarme do alcance. (`bin/fio_estado.py:146`; `bin/fio_estado.py:182`; `bin/fio_estado.py:204`) |
   | `alarma_sempre`, `alarme_passivo_ligado` | São publicados; não participam desses leitores operacionais. (`app/chamados/management/commands/vigiar_fio_morto.py:229`; `bin/fio_estado.py:143`; `app/core/management/commands/pipeline_placar.py:91`) |
   | `alcance` | `fio_estado` lê `alcance_universo`, `alcance_cego` e `alcance_de_hoje_pra_frente`; as demais entradas não participam de seu veredito/apresentação. (`bin/fio_estado.py:74`; `bin/fio_estado.py:142`) |
   | `materializada_viva_exportada`, `viva_em_disputa_fechada` | São publicados; não participam desses leitores operacionais. (`app/chamados/management/commands/vigiar_fio_morto.py:237`; `bin/fio_estado.py:143`; `app/core/management/commands/pipeline_placar.py:91`) |

   Dentro de `esperado`, estão os nove contadores: `fio_morto`, `fio_morto_cobrando`, `fio_morto_com_alvo`, `dias_futuros_no_universo`, `respondida_sem_resposta`, `materializada_viva`, `fio_morto_alcance`, `cadastro_sem_turno` e `fio_mudo_alvos`. Todos são publicados com zero. (`app/chamados/management/commands/vigiar_fio_morto.py:90`; `app/chamados/management/commands/vigiar_fio_morto.py:98`)

   O restante de `censo` é o dicionário integral de `censo()`, incluindo listas, classes, contadores auxiliares e chaves `porta_*`; o vigia acrescenta os cinco complementos ao serializar. O bloco `alcance` também é publicado integralmente, incluindo causas, baldes, competências, detalhes e listas. (`app/chamados/services/vigia_fio.py:356`; `app/chamados/services/vigia_fio.py:371`; `app/chamados/services/vigia_fio.py:535`; `app/chamados/services/vigia_fio.py:571`; `app/chamados/management/commands/vigiar_fio_morto.py:233`)

   **A chave nova muda, sim, um veredito existente:** quando divergir do esperado, entra automaticamente em `fora_do_esperado` e na linha “FORA DO ESPERADO (carimbo inteiro)”. Não há conjunto fechado nesse comparador. A manchete continua sendo exclusivamente a do alcance. (`bin/fio_estado.py:125`; `bin/fio_estado.py:175`; `bin/fio_estado.py:191`)

   `gerar_estado.py` delega a leitura e a frase a `fio_estado`, herdando esse efeito. `placar_code.sh` lê somente a linha `FABRICA-POR-EVENTO` do outro arquivo, `logs/fio_mudo.log`; não ganhou leitor de `fio_mudo_alvos`. (`bin/gerar_estado.py:260`; `bin/placar_code.sh:388`)

   Há comparação **fechada nos testes**, já atualizada: igualdade integral de `censo` e `esperado`. Há também escritores de fixtures nos testes AST e no selo shell; isso não constitui segundo escritor operacional. (`app/chamados/tests/test_o261_vigia_fio.py:814`; `app/chamados/tests/test_o261_vigia_fio.py:824`; `app/core/tests/test_fio_mudo_placar.py:81`; `bin/tests/test_fio_estado.sh:34`)

2. **Derivação do contador: base × cópia**

   Pelo bloco removido/adicionado do diff fornecido, a seleção foi extraída, preservando o corpo antigo; a confirmação independente contra o objeto da base **[nao verificado]**. Na cópia, linha do disparar e número carimbado usam a mesma função. (`app/chamados/management/commands/disparar_perguntas_competencia.py:151`; `app/chamados/management/commands/vigiar_fio_morto.py:181`)

   | Aspecto | Resultado |
   |---|---|
   | Universo | Chamados em `VIVOS`, vinculados à célula dentro da janela, sem pergunta viva; conserva parcial só lista, colaborador ativo, dedup da disputa e barreira de linha do fato. (`app/chamados/management/commands/disparar_perguntas_competencia.py:44`; `app/chamados/management/commands/disparar_perguntas_competencia.py:53`; `app/chamados/management/commands/disparar_perguntas_competencia.py:74`) |
   | Janela | Disparar passa limites explícitos. A extensão nova permite omiti-los e chama `janela_atual(hoje)`. Ambos os crons usam a janela **global**, sem corte por empresa. (`app/chamados/management/commands/disparar_perguntas_competencia.py:32`; `app/chamados/management/commands/disparar_perguntas_competencia.py:111`; `app/chamados/management/commands/disparar_perguntas_competencia.py:151`; `app/ponto/janelas.py:60`) |
   | Dedup | Continua por `(colaborador.pk, dia, motivo)`, escolhendo o primeiro motivo ainda não visto por passagem e marcando-o somente depois das barreiras. Não equivale a enumerar todos os motivos de cada chamado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:63`; `app/chamados/management/commands/disparar_perguntas_competencia.py:86`) |
   | Hoje/futuro | Não ganhou exclusão por data; delega o teto por instante a `vencidos_no_instante`, que consulta `cartorio.marcos_vencidos`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:63`; `app/chamados/services/fabrica_da_celula.py:15`) |
   | Empresas | A outra extensão é o filtro opcional `empresas`. Disparar não o passa; vigiar passa seu recorte. Mesmo com recorte, a janela continua global. (`app/chamados/management/commands/disparar_perguntas_competencia.py:48`; `app/chamados/management/commands/disparar_perguntas_competencia.py:151`; `app/chamados/management/commands/vigiar_fio_morto.py:181`) |
   | Amostragem | O disparar mede em sua rodada; vigiar mede novamente, mais tarde na corrente. Portanto a função é a mesma, mas os números não são necessariamente a mesma fotografia. (`app/config/crons.py:1521`; `app/config/crons.py:1718`) |

   Nas bordas, disparar e vigiar capturam, cada um, um `timezone.now()` e derivam `hoje` desse instante. O placar usa `date.today()` e **duas** chamadas separadas de `datetime.now()`; ele não fornece relógio ao contador, apenas lê o arquivo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:109`; `app/chamados/management/commands/vigiar_fio_morto.py:166`; `app/core/management/commands/pipeline_placar.py:143`)

   **Existe escape herdado do relógio:** `_ja_perguntou(d, motivo, dia)` não recebe `hoje`; chama `fio_vivo`, que chama `pergunta_viva(p)` sem esse argumento. Um ramo chega a `_dentro_da_janela_viva(..., hoje=None)` e lê `date.today()`. A primeira guarda do contador, por contraste, recebe `hoje`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:53`; `app/chamados/management/commands/disparar_perguntas_competencia.py:78`; `app/chamados/services/disputa_emissao.py:1419`; `app/chamados/juizes.py:1434`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:577`)

3. **Fotografia velha, empresa e ordem dos crons**

   **Sim: fotografia velha pode ser usada sem qualquer indicação de velhice.** O leitor não consulta `hoje` nem `medido_em`: um `0/0` antigo não alarma; um `1/0` antigo continua alarmando. Até um placar de dia passado recebe a última fotografia disponível. (`app/core/management/commands/pipeline_placar.py:91`; `app/core/management/commands/pipeline_placar.py:101`; `app/core/management/commands/pipeline_placar.py:131`; `app/core/management/commands/pipeline_placar.py:144`)

   **Recorte por empresa não sobrescreve o carimbo pela porta canônica:** o escritor só carimba quando `empresas is None`. Entretanto, o leitor aceita qualquer arquivo no caminho com os dois inteiros, sem conferir banco, schema ou escopo. Contaminação efetiva em produção **[nao verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:219`; `app/chamados/management/commands/vigiar_fio_morto.py:241`; `app/core/management/commands/pipeline_placar.py:91`)

   **Não existe `NAO_MEDIDO` automático toda manhã:** o arquivo válido de ontem continua aceito. Arquivo ausente, legado sem a chave ou inválido alarma imediatamente, em qualquer horário, sem distinguir “ainda não devido” de “medição atrasada”. Assim, há possibilidade de alarme prematuro enquanto aguarda a primeira medição válida; ocorrência diária em produção **[nao verificado]**. (`app/core/management/commands/pipeline_placar.py:98`; `app/core/management/commands/pipeline_placar.py:131`)

   **A agenda não garante vigiar antes do placar.** O placar roda às 07:40 e declara dependência apenas de `auditar_invariantes_chamados`; vigiar está na corrente após `vigiar_pausa_export`. O encadeador pode esperar e expirar sem que essa agenda independente deixe de executar. A documentação também mostra essa separação. (`app/config/crons.py:747`; `app/config/crons.py:1718`; `bin/encadeado.sh:84`; `bin/encadeado.sh:92`; `app/docs/CRONS.md:39`; `app/docs/CRONS.md:61`)

   Há diferença entre os leitores: `fio_estado` invalida a manchete depois de 26 horas; o leitor novo não tem expiração. (`bin/fio_estado.py:66`; `bin/fio_estado.py:140`; `app/core/management/commands/pipeline_placar.py:91`)

4. **O alcance real dos testes** [nao verificado]

   Os testes novos executam fonte **transformado**: removem todos os imports, substituem `BaseCommand` por `object`, zeram `CRONS/EMPRESAS`, trocam `medidos` e injetam colaboradores. Provam comportamentos desses fragmentos, mas não importação, descoberta dos comandos ou integração dos módulos reais. (`app/core/tests/test_fio_mudo_placar.py:50`; `app/core/tests/test_fio_mudo_placar.py:60`; `app/core/tests/test_fio_mudo_placar.py:82`; `app/core/tests/test_fio_mudo_placar.py:147`)

   **São coletáveis no runner Django:** herdar de `unittest.TestCase` não os exclui; `core` integra as labels, a suíte chama `manage.py test` e o runner deriva de `DiscoverRunner`. Execução efetiva dessa árvore e resultado da suíte **[nao verificado]**. (`app/core/tests/test_fio_mudo_placar.py:71`; `bin/regua.sh:23`; `bin/suite.sh:102`; `app/core/runner_quarentena.py:72`)

   Os dez mutantes novos não são percorridos por um teste: uma variável seleciona **um** durante o import. `suite.sh` não contém laço nem repasse explícito dessas variáveis ao Docker. (`app/core/tests/test_fio_mudo_placar.py:30`; `app/core/tests/test_fio_mudo_placar.py:43`; `bin/suite.sh:96`)

   `FIO_MUDO_BASE` chama `git show`; a montagem declarada leva `app/` para `/app`, sem o `.git` da raiz. Esse mecanismo não funciona no cenário sem histórico indicado na pergunta. (`app/core/tests/test_fio_mudo_placar.py:27`; `bin/suite.sh:74`; `bin/suite.sh:100`)

   O temporário escreve dentro de `app/core/tests` durante a execução. Há cleanup registrado, portanto resíduo permanente não é consequência necessária; interrupção abrupta pode deixá-lo. (`app/core/tests/test_fio_mudo_placar.py:74`; `app/core/tests/test_fio_mudo_placar.py:75`; `app/core/tests/test_fio_mudo_placar.py:81`)

   **Existe prova real adjacente:** o teste do vigia usa `override_settings` e `call_command`, agora compara o contador real e a chave nova. Outro teste chama o disparar real com captura de SQL. Isso não cobre a integração real do novo leitor do placar. (`app/chamados/tests/test_o261_vigia_fio.py:780`; `app/chamados/tests/test_o261_vigia_fio.py:814`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:320`; `app/core/tests/test_fio_mudo_placar.py:79`)

   A forma mínima proposta é manter a coleta e acrescentar:

   - Teste Django com módulos importados normalmente, relógio congelado, temporário fora da árvore e `override_settings(VIGIA_FIO_CARIMBO=...)`; chamar vigia e placar por `call_command`, verificando conteúdo, texto e rc com/sem `--alarme`. O molde já existe no teste do vigia. (`app/chamados/tests/test_o261_vigia_fio.py:780`)
   - Casos explícitos de fotografia velha, dia solicitado e identidade/escopo, hoje ausentes dos testes do leitor. (`app/core/tests/test_fio_mudo_placar.py:105`; `app/core/tests/test_fio_mudo_placar.py:119`)
   - Um teste coletado que percorra os mutantes **em memória**, preservando imports e corrigindo os aliases onde são lidos; exigir a falha da asserção pretendida. Já existe molde de iteração de mutantes em memória no teste vizinho. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:220`)

   Implementação e eficácia dessa proposta **[nao verificado]**.

5. **Classes de reprovação**

   | Classe | Constatação |
   |---|---|
   | Segundo juiz | Não identifiquei juiz novo na extração: ela continua consultando os predicados existentes. O registro mantém declarados os respondedores antigos até sua retirada na E3. (`app/chamados/management/commands/disparar_perguntas_competencia.py:53`; `app/chamados/management/commands/disparar_perguntas_competencia.py:63`; `app/core/juizes.py:153`) |
   | Segundo escritor fora da porta | Não apareceu nova emissão de pergunta nesse caminho. Há, porém, efeito lateral herdado possível no juiz da tranca: `get_user_sistema()` usa `get_or_create`; portanto leitura estritamente sem DML depende dessa condição preexistente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:88`; `app/chamados/juizes.py:1177`; `app/core/utils.py:18`) |
   | Relógio fora da borda | Presente no caminho herdado `_ja_perguntou → fio_vivo → pergunta_viva → date.today()`, descrito no item 2. (`app/chamados/services/disputa_emissao.py:1419`; `app/chamados/juizes.py:1434`; `app/chamados/juizes.py:577`) |
   | Derivação paralela | O número tem função compartilhada. O **esperado tem duas definições**: retorno do contador e literal independente em `ESPERADO_DO_CARIMBO`; a linha imprime o primeiro, o JSON publica o segundo. Hoje coincidem. (`app/chamados/management/commands/disparar_perguntas_competencia.py:88`; `app/chamados/management/commands/vigiar_fio_morto.py:98`; `app/chamados/management/commands/vigiar_fio_morto.py:207`; `app/chamados/management/commands/vigiar_fio_morto.py:228`) |
   | Selo/RED sem mordida | Há asserções contra fragmentos, mas integração real do leitor e execução sistemática dos novos mutantes não estão provadas por esse arquivo. RED executado na base **[nao verificado]**. (`app/core/tests/test_fio_mudo_placar.py:43`; `app/core/tests/test_fio_mudo_placar.py:58`) |
   | Ordem de trava | O contador novo ficou dentro de `tranca_em_lote`; esse contexto organiza memo, não adquire lock de banco. Não identifiquei inversão nova nesse trecho. (`app/chamados/management/commands/vigiar_fio_morto.py:174`; `app/chamados/juizes.py:1108`) |
   | Pouso antes do pré-requisito | A condição documentada exige zero medido com C11 no ar; o comentário do selo continua citando o zero antigo. Cumprimento posterior da condição **[nao verificado]**. (`app/docs/RELATO.md:26`; `bin/tests/test_papel_prazo_nao_deriva.sh:303`) |
   | ESMERIL-SEMANTICA | Há mistura temporal concreta: `placar(dia)` confronta execuções daquele dia, mas agrega fotografia sem vínculo com esse dia. Não há demonstração de mudança do significado da seleção `fio_mudo_alvos`. (`app/core/management/commands/pipeline_placar.py:71`; `app/core/management/commands/pipeline_placar.py:94`; `app/core/management/commands/pipeline_placar.py:131`) |

6. **Itens 3–5 do `opus_final1.txt`**

   - **Item 3 permanece:** lista vazia produz `crons={}`; o shell não imprime linhas, o `grep` falha e aparece “vigia falhou”. A implementação atual conserva essa cadeia. (`app/config/crons.py:2032`; `app/config/crons.py:2097`; `app/chamados/services/vigia_loop.py:80`; `app/chamados/services/vigia_loop.py:92`; `bin/placar_code.sh:204`)
   - **Item 4 permanece pendente de comprovação:** não há no dado fornecido nova medida pós-deploy em PROD com `crons=['disparar_perguntas_competencia']` **[nao verificado]**. A função continua aceitando esse recorte explícito. (`app/chamados/services/vigia_loop.py:71`)
   - **Item 5 não foi demonstrado como resolvido:** testes novos e um ramo opcional de Git não evidenciam RED executado da base. Existe fonte histórica embutida de outra base no teste adjacente, mas isso não comprova a execução exigida. (`app/core/tests/test_fio_mudo_placar.py:27`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:373`) Evidência nova em log **[nao verificado]**.

## 10/11 01:12 · sessao 33b91741b66b · - · - · origem=Ronald (tmux)

- **Extração preserva a seleção antiga segundo o diff fornecido**, com duas extensões: janela global quando `ini` e `fim` são omitidos e filtro opcional `empresas`; o disparar continua passando janela explícita e `colab`, sem filtro de empresas. Não foi possível comparar com objeto Git da base: esta cópia não contém repositório Git; comparação independente do hash da base [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:21`, `:32`, `:45`, `:48`, `:151`; bloco removido de `diff_v4.patch` fornecido.)

- **Universo:** chamados com `status_local ∈ VIVOS`, data da célula entre `ini` e `fim`, sem pergunta viva, respeitando parcial só lista, colaborador ativo, dedup da disputa e existência de linha do fato. Dedup local continua por `(colaborador.pk, dia, motivo)`, acrescido somente depois das barreiras; toma o primeiro motivo ainda não visto por passagem. Não acrescenta exclusão explícita de hoje/futuro: o teto por instante é delegado a `vencidos_no_instante`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:44`, `:53`, `:59`, `:63`, `:74`, `:78`, `:83`, `:86`; `app/chamados/services/fabrica_da_celula.py:15`.)

- **Janela:** cron do disparar passa `--comp-corrente --alarme`; disparar obtém `janela_atual(hoje)` e entrega os limites ao contador. Vigiar chama contador sem limites, que também usa `janela_atual(hoje)` sem empresa. Portanto ambos usam corte global, normalmente 21, inclusive quando vigiar foi chamado com `--empresa`; esse argumento restringe colaboradores, não determina corte individual. (`app/config/crons.py:1521`; `app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `:151`, `:32`, `:48`; `app/chamados/management/commands/vigiar_fio_morto.py:181`; `app/ponto/janelas.py:50`, `:60`.)

- **Relógios nas bordas:** disparar lê um `timezone.now()` e deriva `timezone.localdate(agora)`; vigiar faz igual, em sua própria rodada. Assim o mesmo contador pode produzir números distintos em instantes diferentes, sem mudança na derivação. Placar usa `date.today()` e duas chamadas separadas de `datetime.now()`; não recalcula contador, apenas lê fotografia. Django declara `America/Sao_Paulo`, mas fuso efetivo dos processos em produção [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:109`; `app/chamados/management/commands/vigiar_fio_morto.py:166`; `app/core/management/commands/pipeline_placar.py:143`; `app/config/settings/base.py:93`.)

- **Há relógio interno herdado:** embora a primeira guarda passe `_viva(p, hoje=hoje)`, a segunda chama `_ja_perguntou` sem hoje; ela chama `fio_vivo`, que chama `pergunta_viva(p)` sem hoje. O fallback pode chegar a `date.today()` em `_dentro_da_janela_viva`. O diff mostra essa chamada já no corpo antigo: problema preservado, não introduzido pela extração. (`app/chamados/management/commands/disparar_perguntas_competencia.py:53`, `:78`; `app/chamados/services/disputa_emissao.py:1418`; `app/chamados/juizes.py:1434`, `:1389`, `:577`; bloco removido de `diff_v4.patch` fornecido.)

- **Carimbo velho é aceito:** `alarmes_fio_mudo` verifica exclusivamente presença/tipo inteiro de `censo.fio_mudo_alvos` e `esperado.fio_mudo_alvos`. Não lê `hoje`, `medido_em`, `banco` ou `schema`; não recebe `dia` nem horário devido. Uma fotografia antiga `0/0` permite ausência de alarme, e uma antiga `1/0` continua alarmando. Até `pipeline_placar --dia <passado>` anexa a última fotografia disponível. (`app/core/management/commands/pipeline_placar.py:85`, `:94`, `:101`, `:131`, `:144`.)

- **Outra empresa:** o escritor canônico impede que execução com `--empresa` substitua a fotografia de frota. Entretanto o leitor não valida origem/escopo; aceitará qualquer JSON no caminho configurado que contenha os dois inteiros, inclusive de outro banco/schema. Contaminação efetiva em produção [nao verificado]. (`app/chamados/management/commands/vigiar_fio_morto.py:156`, `:219`, `:222`, `:226`, `:241`; `app/core/management/commands/pipeline_placar.py:92`.)

- **`NAO_MEDIDO` não surge automaticamente toda manhã:** não há expiração diária, então o carimbo válido de ontem continua sendo usado. Surge com arquivo ausente, ilegível, sem chave ou com tipo inválido, e ocorre em qualquer horário até existir fotografia válida, sem tolerância ligada à execução prevista. Afirmar ocorrência diária real seria [nao verificado]. (`app/core/management/commands/pipeline_placar.py:88`, `:91`, `:98`, `:131`.)

- **Cron não garante vigiar antes do placar:** `vigiar_fio_morto` pertence à corrente, depois de `vigiar_pausa_export`; `pipeline_placar` tem horário independente 07:40 e declara dependência apenas de `auditar_invariantes_chamados`. O encadeador espera carimbos dos gatilhos da corrente, mas o placar não espera o carimbo do vigiar. A corrente pode atrasar/falhar sem impedir placar. (`app/config/crons.py:1718`, `:747`; `bin/encadeado.sh:84`, `:92`; `app/docs/CRONS.md:39`, `:45`, `:61`.)

- **Divergência documental:** consulta marca cartório 06:28, código marca 06:27. O próprio documento declara `config/crons.py` como fonte prevalecente. (`app/docs/CRONS.md:33`, `:7`; `app/config/crons.py:616`.)

- **Metadado esperado tem duas definições:** contador retorna `esperado=0`; a linha VFM imprime esse retorno, mas o JSON usa outro literal, de `ESPERADO_DO_CARIMBO`, não `cfm['esperado']`. Hoje coincidem; mudar somente retorno faria linha e JSON divergirem. A seleção e o número continuam compartilhados. (`app/chamados/management/commands/disparar_perguntas_competencia.py:88`; `app/chamados/management/commands/vigiar_fio_morto.py:98`, `:207`, `:228`.)

## 10/11 01:12 · sessao 29ee3fe4196e · - · - · origem=Ronald (tmux)

4. O arquivo novo é **coletável**, mas prova código transformado, não a integração dos módulos reais: remove todos os imports, troca `BaseCommand` por `object`, zera `CRONS/EMPRESAS` e injeta funções/mocks (`app/core/tests/test_fio_mudo_placar.py:50-62`, `:82-83`, `:147-177`, `:205-211`). A classe herda `unittest.TestCase`, compatível com a descoberta usada pelo runner; `core` está nas labels, a suite chama `manage.py test`, e o runner delega a `DiscoverRunner.build_suite` (`app/core/tests/test_fio_mudo_placar.py:71`; `bin/regua.sh:23`; `bin/suite.sh:102-103`; `app/config/settings/ci.py:40`; `app/core/runner_quarentena.py:72-76`). Fiz somente parse e coleta stdlib em memória: sete testes coletados; execução Django nessa cópia **[nao verificado]**.

A cópia contém `[nome]`, sintaticamente válido; o `[nome]` do anexo é alteração da raspagem e não defeito do arquivo real (`app/core/tests/test_fio_mudo_placar.py:26-29`).

Os mutantes novos **não são percorridos pela suite**: só um mutante selecionado por variável de ambiente altera a string durante o import; o script Docker não repassa explicitamente `FIO_MUDO_MUTANTE` ou `FIO_MUDO_BASE` (`app/core/tests/test_fio_mudo_placar.py:30-47`; `bin/suite.sh:96-103`). Herança eventual dessas variáveis por env-files/imagem **[nao verificado]**.

`FIO_MUDO_BASE` depende de executar `git show`; o container recebe a montagem de `app/` em `/app`, sem montagem do `.git` da raiz. Portanto esse ramo depende de Git/histórico externo à montagem declarada e falha no cenário sem `.git` descrito na pergunta (`app/core/tests/test_fio_mudo_placar.py:27-29`; `bin/suite.sh:74`, `:100`).

O `setUp` cria diretório dentro de `app/core/tests`; a limpeza está registrada e ocorre normalmente, mas a execução efetivamente escreve na árvore e uma interrupção abrupta pode deixar resíduos (`app/core/tests/test_fio_mudo_placar.py:73-81`, `:127-131`).

**Existe prova real ao lado**, inclusive atualizada pelo v4: `_rodar` usa `override_settings(VIGIA_FIO_CARIMBO=...)` e `call_command('vigiar_fio_morto', ...)`; o teste compara o carimbo ao contador real e inclui a chave nova (`app/chamados/tests/test_o261_vigia_fio.py:780-785`, `:814-828`). Também existe chamada real de `disparar_perguntas_competencia` com captura de SQL (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:315-331`). Isso não cobre, sozinho, a integração real do leitor novo (`app/core/tests/test_fio_mudo_placar.py:79-84`).

Forma mínima proposta: manter a coleta existente; acrescentar teste Django com `call_command('pipeline_placar', ...)`, `override_settings` e temporário fora da árvore; usar módulos importados normalmente, congelar o relógio e controlar só as entradas externas necessárias. Reaproveitar a fixture e chamada real do vigia para conferir o caminho contador → carimbo → placar; migrar os mutantes para um teste que percorra todos em memória e passe a mesma verificação a cada versão, preservando os imports. O repositório já possui os moldes `override_settings+call_command` e mutantes em memória (`app/chamados/tests/test_o261_vigia_fio.py:780-785`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:220-227`). A eficácia da implementação proposta permanece **[nao verificado]**.

6. Sobre os itens 3–5 da reprovação:

- **Item 3 permanece:** lista vazia → `crons={}` → nenhuma linha impressa → `grep` falha → mensagem “vigia falhou”; os sítios não foram tocados pelo diff v4 fornecido (`app/config/crons.py:2032`; `app/chamados/services/vigia_loop.py:80-99`; `bin/placar_code.sh:204-209`; anexo `diff_v4.patch` **[nao verificado: sem arquivo local do anexo]**).
- **Item 4:** não há nova prova de medição pós-deploy em PROD nos dados fornecidos; cumprimento **[nao verificado]**. [nao verificado]
- **Item 5:** os testes novos e o ramo opcional `FIO_MUDO_BASE` não constituem evidência de RED executado na base; além disso o ramo Git não resolve a execução na montagem sem `.git` (`app/core/tests/test_fio_mudo_placar.py:27-29`; `bin/suite.sh:100`). Há verificações de mutantes e fonte antiga embutida no teste Django existente, mas RED em log da base exigida continua **[nao verificado]** (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:220-227`, `:373`).

## 10/11 01:11 · sessao cac1af8de873 · - · - · origem=Ronald (tmux)

1. **Escritor de produção localizado:** `vigiar_fio_morto.Command.handle` monta o registro completo e chama `_carimbar` somente para frota (`empresas is None`); recorte `--empresa` imprime que não carimbou. `_carimbar` escreve `.novo` e faz `os.replace`. O caminho vem de `settings.VIGIA_FIO_CARIMBO` ou `<BASE_DIR>/logs/fio_morto.json`. (`app/chamados/management/commands/vigiar_fio_morto.py:105`, `:118`, `:130`, `:222`, `:241`.)

2. **Chaves de topo escritas:** `medido_em`, `hoje`, `banco`, `schema`, `esperado`, `alarma_sempre`, `alarma_em_0`, `alarme_passivo_ligado`, `gritou`, `censo`, `alcance`, `materializada_viva_exportada`, `viva_em_disputa_fechada`. (`app/chamados/management/commands/vigiar_fio_morto.py:223`.)

   - `esperado` contém nove contadores: `fio_morto`, `fio_morto_cobrando`, `fio_morto_com_alvo`, `dias_futuros_no_universo`, `respondida_sem_resposta`, `materializada_viva`, `fio_morto_alcance`, `cadastro_sem_turno`, **`fio_mudo_alvos`**, todos zero. (`app/chamados/management/commands/vigiar_fio_morto.py:90`, `:98`.)
   - `censo` conserva todas as chaves de `censo()`, adiciona os cinco complementos `respondida_sem_resposta`, `materializada_viva`, `fio_morto_alcance`, `cadastro_sem_turno`, **`fio_mudo_alvos`**; `fio_do_admin_sem_resposta` é atualizado depois dos dois censos. As chaves originais e dinâmicas estão declaradas no serviço. (`app/chamados/management/commands/vigiar_fio_morto.py:182`, `:233`; `app/chamados/services/vigia_fio.py:356`, `:371`.)
   - `alcance` conserva o resultado completo do censo de alcance: universo, competências, portas, futuro, contagem, papéis, detalhes, listas e causas/baldes derivados; `alcance_cego` é adicionado antes do retorno. (`app/chamados/services/vigia_fio.py:535`, `:571`, `:588`.)
   - `viva_em_disputa_fechada` contém `perguntas` e `disputas`. (`app/chamados/services/vigia_fio.py:759`.)

3. **Leitor direto host:** `bin/fio_estado.py` abre o caminho fixo `app/logs/fio_morto.json`; não usa o override Django. Lê `censo`, `esperado`, `medido_em`, `hoje`, `banco`, `gritou`, `alarma_em_0`, os campos necessários de `alcance` e um subconjunto nominal do censo para apresentação. Não usa `schema`, `alarma_sempre`, `alarme_passivo_ligado`, `materializada_viva_exportada` ou `viva_em_disputa_fechada` em sua saída/veredito. (`bin/fio_estado.py:65`, `:104`, `:121`, `:143`.)

4. **A chave nova altera SIM o veredito de leitor já existente:** `fio_estado._ler_carimbo` percorre **todo** `esperado.items()` e compara cada valor com `censo.get(k)`. Logo `fio_mudo_alvos != esperado.fio_mudo_alvos` entra automaticamente em `fora_do_esperado` e aparece na linha **“FORA DO ESPERADO (carimbo inteiro)”**. Não há conjunto fechado impedindo a chave nova. A manchete continua regida exclusivamente por `fio_morto_alcance` e sua validade; ela pode continuar mostrando alcance zero e, abaixo, acusar FIO MUDO. (`bin/fio_estado.py:125`, `:175`, `:191`.)

5. **Leitor indireto:** `bin/gerar_estado.py` chama `fio_estado.ler` e `fio_estado.linha`, portanto herda exatamente o efeito da chave nova sobre a seção de fio morto. (`bin/gerar_estado.py:260`.)

6. **Leitor novo:** `pipeline_placar.alarmes_fio_mudo` usa o mesmo resolvedor de caminho do escritor; lê exclusivamente `censo.fio_mudo_alvos` e `esperado.fio_mudo_alvos`, exige `type(...) is int`, retorna `NAO_MEDIDO` por ausência/forma inválida e alarma por desigualdade. Não valida `medido_em`, `hoje`, `banco`, `schema`, escopo ou empresa. O alarme entra incondicionalmente em todo `placar(dia)` e pode causar rc 2 com `--alarme`. (`app/core/management/commands/pipeline_placar.py:10`, `:85`, `:94`, `:131`, `:153`.)

7. **`placar_code.sh` não lê esse JSON nem a chave nova:** lê apenas a linha `FABRICA-POR-EVENTO` de `logs/fio_mudo.log`, com verificação de data do arquivo, publicando `perguntas_que_o_julgamento_nao_fez`. A volta 4 não liga `fio_mudo_alvos` ao seu `TOTAL`. (`bin/placar_code.sh:388`, `:389`, `:390`.)

8. **Leitores/escritores de fixtures encontrados:** teste Django de carimbo chama o comando real com `override_settings`, lê o JSON e compara **o dicionário completo** de `censo` e `esperado` — conjunto fechado no teste, já atualizado pela volta 4; testes de alcance leem campos selecionados. O novo teste AST escreve JSON próprio e lê o carimbo gerado pelo módulo reconstruído; o selo shell escreve fixtures em temporário e invoca `fio_estado.py`. (`app/chamados/tests/test_o261_vigia_fio.py:783`, `:809`, `:814`, `:824`; `app/chamados/tests/test_o261e_f5_alcance.py:274`, `:290`, `:465`; `app/core/tests/test_fio_mudo_placar.py:74`, `:81`, `:181`; `bin/tests/test_fio_estado.sh:32`, `:71`, `:74`.)

9. **Outro endereço de escrita em testes:** settings CI substituem o carimbo por arquivo em temporário externo à árvore; esse é um destino alternativo, não um segundo escritor de produção. (`app/config/settings/ci.py:30`.)

## 10/11 01:08 · sessao 1b9519f63a52 · O169-FECHADA · construtor · origem=Code

**Parei pela condição explícita do brief:** fechamento automático e decisão administrativa do dia gravam o mesmo par: `status_local='resolvido'`, `via_encerramento='sistema'`. Não é possível incluir um e excluir o outro pelos valores exigidos. (`ponto/services/cluster_espurio.py:124`; `ponto/services/veredito_celula.py:202`; `chamados/models.py:654`; `chamados/catalogo/motor.py:95`; `chamados/catalogo/motor.py:111`)

| Escritor | Estado/via gravados |
|---|---|
| Automático: `reconciliar_colab` | `resolvido/sistema`, via `reconciliar()`. (`ponto/services/cluster_espurio.py:124`; `chamados/models.py:654`; `chamados/catalogo/motor.py:111`) |
| Decisão administrativa do dia — deveria ficar fora | **Também `resolvido/sistema`**, pelo mesmo gate. (`ponto/services/veredito_celula.py:202`; `chamados/models.py:654`) |
| Transição humana | Destino solicitado, incluindo `resolvido` ou `fechado`; via `admin`. (`chamados/services/acoes_chamado.py:313`; `chamados/catalogo/motor.py:95`; `chamados/catalogo/motor.py:100`) |
| Desligamento | `fechado/cascade`. (`colaboradores/services/desligamento.py:155`; `chamados/models.py:1619`; `chamados/catalogo/motor.py:95`; `chamados/catalogo/motor.py:114`) |
| Tranca | `fechado/competencia_trancada`. (`chamados/services/competencia_trancada.py:184`; `chamados/models.py:1632`; `chamados/models.py:1639`) |

O núcleo escreve `status_local` e `via_encerramento`; ao voltar a vivo, limpa a via. A trilha `estado_chamado` registra estado anterior, novo e via, preservando a colisão acima. (`chamados/models.py:228`; `chamados/models.py:245`; `chamados/models.py:249`; `chamados/models.py:292`)

Os leitores relevantes encontrados são:

- **Serviço de cluster:** seleciona somente `VIVOS`; o dedup consulta qualquer estado. Assim, um encerrado com assinatura pode escapar tanto de `a_fechar` quanto de `a_abrir`. (`ponto/services/cluster_espurio.py:66`; `ponto/services/cluster_espurio.py:84`; `ponto/services/cluster_espurio.py:107`)
- **Vigia:** lê essas pendências; atualmente não examina encerrados com assinatura. (`ponto/management/commands/detectar_cluster_espurio.py:50`; `ponto/management/commands/detectar_cluster_espurio.py:55`)
- **Reabertura:** lê `cascade` para impedir renascimento e `admin` para exigir fato novo. (`chamados/models.py:1007`; `chamados/models.py:1342`)
- **Arquivo e destranca:** leem `competencia_trancada`. (`chamados/views_arquivar.py:64`; `chamados/services/competencia_trancada.py:259`)
- **Auditoria:** verifica terminal sem via e divergência entre status e última trilha. (`chamados/management/commands/auditar_invariantes_chamados.py:1069`; `chamados/services/vigia_loop.py:114`)

**ESMERIL-SEMANTICA:** não ficou comprovada uma suposição universal “cluster encerrado = premissa morta”. O resolvedor de perguntas ainda consulta `pergunta_viva` antes de carimbar; porém existe fallback genérico que aceita destino encerrado como prova, com rechecagem limitada pela janela. Alcançar clusters concretos nesse vínculo: **[nao verificado]**. (`chamados/services/disputa_emissao.py:1840`; `chamados/juizes.py:760`; `chamados/juizes.py:766`; `chamados/juizes.py:776`)

Entrega interrompida antes da edição: nenhum arquivo alterado; contador, RED novo e mutantes não implementados nem executados; ruff e suíte não executados. Certificação: **[nao verificado]**. O impedimento é a colisão `resolvido/sistema` demonstrada acima. [nao verificado]

## 10/11 01:06 · sessao f142f1dcfd25 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Item 2 implementado para revisão, com contador único `fio_mudo_alvos`, esperado `0`, dono `ESTRUTURA` ([declaração](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/chamados/management/commands/disparar_perguntas_competencia.py:88)).

Arquivos tocados e motivos:

- [disparar_perguntas_competencia.py:21](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/chamados/management/commands/disparar_perguntas_competencia.py:21): extrai a seleção existente; a linha `VIGIA FIO MUDO` usa o número dessa função.
- [vigiar_fio_morto.py:181](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/chamados/management/commands/vigiar_fio_morto.py:181): chama a mesma função, imprime VFM e grava número/esperado no carimbo. A chave fica fora dos grupos de alarme próprios.
- [pipeline_placar.py:85](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/core/management/commands/pipeline_placar.py:85): lê número e esperado do JSON. Ausência, ilegibilidade ou chave inválida geram `FIO_MUDO NAO_MEDIDO`, porque falta de medição não equivale a zero.
- [test_fio_mudo_placar.py:105](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/core/tests/test_fio_mudo_placar.py:105): novos REDs, prova da função comum e dez mutantes.
- [test_e4a2_disparar_e_vigia.py:151](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/chamados/tests/test_e4a2_disparar_e_vigia.py:151): adapta as provas de relógio/relatório à extração.
- [test_o261_vigia_fio.py:814](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/chamados/tests/test_o261_vigia_fio.py:814): acrescenta a chave à comparação integral do carimbo.
- [test_cobparc5_parcial_sem_periodo_so_lista.py:327](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:327): atualiza o endereço da leitura de `parcial_so_lista`.

Quem escreve: `_carimbar`, chamado com a chave nova em [vigiar_fio_morto.py:223](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/chamados/management/commands/vigiar_fio_morto.py:223). Quem lê: o novo leitor do placar, acima, e [fio_estado.py:125](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/bin/fio_estado.py:125). Nos três leitores perguntados não há conjunto fechado que impeça a chave: `fio_estado` percorre `esperado.items()`, [gerar_estado.py:260](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/bin/gerar_estado.py:260) delega a ele, e [placar_code.sh:388](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/bin/placar_code.sh:388) raspa o texto `FABRICA-POR-EVENTO`.

RED executado antes da cura: o alvo `1` devolveu alarmes `[]`. A reprodução final na base falhou nos três contratos: leitor, carimbo e função comum. Depois: **7 testes OK**; os **10 mutantes retornaram rc 1 por asserção**, sem erros de infraestrutura ([provas e mutações](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/core/tests/test_fio_mudo_placar.py:30)). [nao verificado]

Comandos reproduzíveis, em `app/`:

```bash
export PYTHONDONTWRITEBYTECODE=1
teste_fio=core.tests.test_fio_mudo_placar

# RED na base: três falhas esperadas
FIO_MUDO_BASE=11f82cfec38bb5a0292e17ab29147a1d67ab25f7 python3 -m unittest \
 "$teste_fio.FioMudoPlacarTest.test_RED_alvo_um_alarma_e_sai_dois_com_chave" \
 "$teste_fio.FioMudoPlacarTest.test_RED_carimbo_publica_numero_esperado_e_linha_sem_alarme_proprio" \
 "$teste_fio.FioMudoPlacarTest.test_mesma_funcao_alimenta_a_linha_e_o_carimbo" -v

# Depois: 7 testes OK [nao verificado]
python3 -m unittest "$teste_fio" -v

# Cada comando abaixo deve falhar por asserção
FIO_MUDO_MUTANTE=placar_sem_leitor python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=placar_comparacao_invertida python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=placar_limiar_proprio python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=placar_rc_zero python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=carimbo_sem_numero python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=carimbo_esperado_um python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=disparar_numero_proprio python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=vigiar_contador_proprio python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=contador_constante python3 -m unittest "$teste_fio" -v
FIO_MUDO_MUTANTE=contador_sem_dedup python3 -m unittest "$teste_fio" -v
```

Ruff nos sete arquivos: `All checks passed!`, com:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache \
 chamados/management/commands/{disparar_perguntas_competencia,vigiar_fio_morto}.py \
 core/management/commands/pipeline_placar.py \
 core/tests/test_fio_mudo_placar.py \
 chamados/tests/{test_e4a2_disparar_e_vigia,test_o261_vigia_fio,test_cobparc5_parcial_sem_periodo_so_lista}.py
```

As provas novas executam código por AST com dependências substituídas ([escopo](/home/ronald/wt-codex-fio-e0-v4-e4a-2-005728/app/core/tests/test_fio_mudo_placar.py:1)). Não rodaram: suite Django/ORM, banco, medição de produção ou pós-deploy. Condição de pouso e certificação Code/opus final: **[não verificado]**. [nao verificado]

## 10/11 01:00 · sessao c3956c64fa93 · REVERTER-DIA-SEM-RECALCULO · verificador · origem=Code

A VOLTA passa a usar o caminho comum, mas “dinheiro 1x por competência” vale **por chamada**, e “humano não rejulga” protege somente o julgamento direto. Essas ressalvas precisam acompanhar a aprovação. (`app/ponto/portas/celula.py:427`, `app/ponto/portas/celula.py:450`, `app/ponto/portas/celula.py:811`)

**1. Quem escreve e quem lê**

| Campo/entidade | Escritor no caminho da VOLTA | Leitores relevantes |
|---|---|---|
| `CelulaDia.ata/veredito` | Cartório produz o julgamento; `lavrar_veredito` altera os campos; `carimbar` persiste por `salvar_e_emitir`, junto com o evento. (`app/ponto/services/cartorio.py:1811`, `app/ponto/services/cartorio.py:1833`, `app/ponto/portas/celula.py:1032`, `app/ponto/portas/celula.py:904`) | Grade prefere minutos/tipo lavrados e transporta veredito; juiz de cobrança lê `trabalha/ata.tipo_dia`. (`app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:424`, `app/chamados/juizes.py:426`) |
| `DiaPago` | `recalcular_por_evento` chama o fechamento; este entrega o resultado à lavratura, que apaga e recria as linhas da competência/versão. (`app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/dia_pago.py:359`) | Calendário lê linhas por data; testemunha do export lê linhas da versão escolhida e carimbos da versão `motor`. (`app/colaboradores/services/calendario.py:290`, `app/folha/porta_export.py:75`, `app/folha/porta_export.py:284`) |
| `FechamentoMensal` | Aberto usa `save`; demais estados usam `update` dos valores calculados e carimbos. (`app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1160`) | TXT seleciona o fechamento gravado e extrai seus eventos. (`app/folha/export.py:711`, `app/folha/export.py:722`) |
| `Chamado.celula/atualizado_em` | Rejunção chama `apontar_celula`, que preserva apontamento existente e escreve pela porta. (`app/chamados/rejuncao.py:107`, `app/chamados/services/vinculos.py:66`, `app/chamados/services/vinculos.py:93`) | `dia_do_fato` passa a preferir `chamado.celula.data`. (`app/chamados/juizes.py:98`) |
| Estado/encerramento/urgência do chamado | Falta espúria chama `reconciliar`; `_gravar_estado` escreve o estado e os carimbos, com rebaixamento de urgência. (`app/chamados/services/feriado_reavaliar.py:47`, `app/chamados/models.py:228`, `app/chamados/models.py:244`, `app/chamados/signals.py:72`) | Receivers de encerramento resolvem perguntas e acionam o cartório quando muda o estado. (`app/chamados/signals.py:96`, `app/chamados/signals.py:115`, `app/chamados/signals.py:192`, `app/chamados/signals.py:216`) |
| Carimbos/resolução e veredito das perguntas | S143 chama o resolvedor; este usa `PerguntaDisputa.carimbar`, podendo atualizar resposta/validação e relavrar `veredito_lavrado/julgado_em/janela_expira_em`. (`app/chamados/signals.py:115`, `app/chamados/services/disputa_emissao.py:1860`, `app/chamados/models.py:3128`, `app/chamados/models.py:3149`) | Estado da pergunta e seleção das ainda não materializadas. (`app/chamados/models.py:3043`, `app/chamados/services/disputa_emissao.py:1820`) |

No financeiro, o alcance inclui as 15 rubricas de dia e os ajustes DSR/banco; no fechamento inclui também totais, contadores, motivos/causa do espelho e carimbos, conforme a lista efetivamente persistida. (`app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:43`, `app/ponto/services/fechamento.py:1160`)

A rejunção alcança **todos os chamados vivos de escopo DIA sem célula do colaborador**, inclusive fora dos dias revertidos; somente a falta espúria fica recortada pelos dias tocados. (`app/ponto/portas/celula.py:498`, `app/chamados/rejuncao.py:59`, `app/ponto/portas/celula.py:501`)

Não encontrei dependência executável de “VOLTA não recalcula/não rejunta” nos leitores citados. Existe, porém, documentação agora desatualizada que lista a reversão como caso sem evento; o comando correspondente apenas conta e alarma. (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:14`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:34`, `app/chamados/signals.py:319`)

Na semântica, competência vem da **data do fato**, pelo corte da empresa; o dia dos períodos financeiros vem de `resultado.dia_da_jornada`, com fallback civil somente sem mapa. O diff reutiliza essas autoridades. (`app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1594`, `app/ponto/services/dia_pago.py:715`, `app/ponto/janelas.py:60`, `app/ponto/services/fechamento.py:1217`, `app/ponto/services/dia_pago.py:128`)

**2. Contraexemplos além do golden**

- **Futuro:** pula o juiz direto e solicita dinheiro. Como a grade prefere a ata existente, DNA restaurado para 240 com ata preservada em 480 pode continuar produzindo previsto financeiro 480; os novos testes exigem somente o disparo nesse caso. (`app/ponto/portas/celula.py:431`, `app/ponto/portas/celula.py:445`, `app/escala/services/leitor_celula.py:399`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:105`)
- **Humano:** mesma ressalva financeira. Além disso, falta espúria encerrada pelo callback pode chamar o cartório novamente pelo signal, sem filtro `_HUMANOS`; a lavra preserva veredito administrativo, mas permite atualizar a ata. Portanto “ata humana sempre intacta” não é garantia geral desse caminho. (`app/ponto/portas/celula.py:427`, `app/chamados/services/feriado_reavaliar.py:47`, `app/chamados/signals.py:216`, `app/ponto/portas/celula.py:1003`)
- **Exportada com `apesar_da_lavra`:** sim, alcança dinheiro de exportada. O argumento abre a guarda do DNA; depois o callback chama normalmente o escritor mensal. **Exceção: 09/2026 exportada continua barrada**, porque a guarda financeira exclui esses colaboradores antes da escrita. (`app/ponto/portas/celula.py:779`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:558`)
- **Motivo em exportada:** o motivo da VOLTA chega ao log posterior do evento, mas não é repassado como `motivo_exportada`; o aviso específico pode dizer “sem motivo declarado”. (`app/ponto/portas/celula.py:812`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:1813`, `app/ponto/services/fechamento.py:547`)
- **Sem `dna_anterior`:** retorna `False` antes da restauração e do rabo; igualmente para célula inexistente ou origem diferente de `gerada`. (`app/ponto/portas/celula.py:776`)
- **Lote:** N reversões efetivas geram N pedidos financeiros, mesmo na mesma competência. O comando chama a porta por dia; cada chamada cria seu próprio dicionário de deduplicação. Cada cálculo alcança a competência inteira daquele colaborador. (`app/ponto/management/commands/aplicar_col900.py:65`, `app/ponto/management/commands/aplicar_col900.py:77`, `app/ponto/portas/celula.py:450`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/dia_pago.py:359`)
- **Exceções em autocommit:** falhas tratadas no julgamento, financeiro ou callbacks não desfazem a restauração anterior. Pode haver DNA restaurado com projeções antigas; falha do juiz não impede a tentativa financeira. (`app/ponto/portas/celula.py:785`, `app/ponto/portas/celula.py:437`, `app/ponto/portas/celula.py:445`, `app/ponto/portas/celula.py:495`, `app/ponto/services/fechamento.py:1818`)
- **Falha financeira parcial:** fechamento é persistido antes de `DiaPago`; erro da lavratura é engolido, e seu `delete/bulk_create` não está envolvido em um atomic próprio. O caminho permite projeções financeiras parcialmente atualizadas; ocorrência em produção [não verificado]. (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/fechamento.py:1238`, `app/ponto/services/dia_pago.py:359`)

**3. Atomic externo e ordem**

O rabo está fora do `atomic` **local**. Dentro de um atomic externo, o juiz continua síncrono; dinheiro e reavaliação ficam no `on_commit` externo. Isso preserva juiz → dinheiro desta chamada, mas sair do bloco local não significa que a reversão já foi commitada. (`app/ponto/portas/celula.py:785`, `app/ponto/portas/celula.py:811`, `app/ponto/portas/celula.py:434`, `app/ponto/services/fechamento.py:1824`, `app/ponto/portas/celula.py:502`)

Há ainda caminho do juiz por `reconciliar`, com `atomic(savepoint=False)`: exceção que atravesse esse bloco pode comprometer o atomic externo mesmo sendo posteriormente engolida pelo rabo. Reprodução desse cenário [não verificado]. (`app/ponto/services/cartorio.py:1499`, `app/chamados/models.py:650`, `app/ponto/portas/celula.py:437`)

O financeiro também não é necessariamente o primeiro callback: o julgamento pode provocar fechamento de disputa/pergunta, que agenda retomada de regularizações ou fechamento de beco antes dele. A existência desses dados no caso concreto [não verificado]. (`app/chamados/signals.py:115`, `app/chamados/signals.py:144`, `app/chamados/signals.py:238`, `app/ponto/portas/celula.py:458`)

**4. Os testes mordem?** [nao verificado]

O golden tem mordida: `.get()` exige `DiaPago` existente, a montagem exige 480, e a VOLTA exige DNA antigo, `dna_anterior=None`, ata 240 e dinheiro 240. Ausência de fixture financeira falha, em vez de passar vazia. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:58`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:80`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:92`)

Futuro/humano também detectam ausência completa do rabo: além do assert negativo do juiz, exigem chamada financeira única. Entretanto mockam o financeiro e não demonstram seu resultado nem exercitam a cascata com falta espúria. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:99`)

O AST cobra chamada única, ausência de juiz/dinheiro diretos, posição e argumentos; seus mutantes exercitam essas restrições. Ele não verifica fluxo alcançável nem o corpo do helper. (`app/ponto/tests/test_reverter_regen_sem_banco.py:54`, `app/ponto/tests/test_reverter_regen_sem_banco.py:68`, `app/ponto/tests/test_reverter_regen_sem_banco.py:90`)

Executei os 11 testes AST: passaram. Também aceitei, em memória, mutantes com retorno antecipado dentro do atomic e helper vazio; resultados sem artefato persistido [não verificado]. [nao verificado]

O **RED na base com quatro failures não está comprovado** pelo material fornecido. A string `_FONTE_BASE` reduzida não substitui execução contra a base real. [não verificado] (`app/ponto/tests/test_reverter_regen_sem_banco.py:25`)

**5. Classes de reprovação**

Não aparece novo juiz, escritor fora da porta ou derivação financeira paralela no trecho alterado: ele delega ao helper comum e às autoridades existentes. Relógio continua sendo lido no helper e no cartório; não há nova leitura introduzida pela chamada. (`app/ponto/portas/celula.py:811`, `app/ponto/portas/celula.py:426`, `app/ponto/services/cartorio.py:1976`)

As ressalvas concretas são **alcance insuficiente dos selos para a promessa ampla**, ausência de deduplicação entre VOLTAs e proteção humana restrita ao disparo direto. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:99`, `app/ponto/portas/celula.py:450`, `app/chamados/signals.py:216`)

Há documentação com dois significados de `DiaPago.data`: modelo/calendário dizem “dia da entrada”, enquanto o escritor prioriza o mapa da jornada. A divergência é preexistente e o diff a alcança ao recalcular. (`app/ponto/models.py:298`, `app/ponto/models.py:319`, `app/colaboradores/services/calendario.py:288`, `app/ponto/services/dia_pago.py:128`)

Não há trava nova no trecho substituído; execução concorrente e cumprimento de pré-requisitos de pouso [não verificado]. (`app/ponto/portas/celula.py:811`)

**6. Outro sítio faz em dobro?**

O chamador produtivo externo encontrado, `aplicar_col900`, não rejulga nem recalcula explicitamente depois da reversão: imprime e retorna. (`app/ponto/management/commands/aplicar_col900.py:77`, `app/ponto/management/commands/aplicar_col900.py:85`)

Existem repetições indiretas: encerramento de falta espúria pode chamar o mesmo juiz novamente; e o consumidor do `EventoCelula` já tenta rejunção, agora também tentada pelo callback novo. A rejunção é protegida por FK nula e pela guarda da porta, portanto repete tentativa, sem criar outro escritor. (`app/chamados/signals.py:216`, `app/ponto/portas/celula.py:796`, `app/chamados/reconciliador.py:1364`, `app/ponto/portas/celula.py:498`, `app/chamados/rejuncao.py:62`, `app/chamados/services/vinculos.py:96`)

## 10/11 00:59 · sessao df47402d072f · - · - · origem=Ronald (tmux)

- **Chamador externo:** na cópia, o chamador produtivo encontrado é `aplicar_col900 --reverter`: percorre os dias e chama a porta uma vez por dia; depois só imprime o resultado e retorna, sem segundo julgamento ou cálculo explícito (`app/ponto/management/commands/aplicar_col900.py:65`, `:77`, `:79`, `:85`). A deduplicação financeira é local a uma chamada do rabo: dicionário novo em `app/ponto/portas/celula.py:450`, preenchimento em `:456`, disparo em `:458`. Portanto N reversões bem-sucedidas na mesma competência podem produzir N recálculos; não existe deduplicação do lote nesse chamador (`app/ponto/management/commands/aplicar_col900.py:65`, `:77`).

- **Rejunção:** o callback não se limita aos dias revertidos: recebe o colaborador inteiro (`app/ponto/portas/celula.py:498`); seleciona chamados vivos, de escopo DIA, com FK `celula` nula (`app/chamados/rejuncao.py:59`); resolve a data pelo catálogo e busca a célula existente (`:83`, `:88`); escreve pela porta `apontar_celula` (`:107`). Essa porta altera `celula` e `atualizado_em`, trava a ordem antes do save e preserva apontamento existente (`app/chamados/services/vinculos.py:66`, `:96`, `:98`, `:100`). Leitor do vínculo: `dia_do_fato` passa a preferir `chamado.celula.data` (`app/chamados/juizes.py:98`).

- **Falta espúria e perguntas:** o outro trecho do callback recorta os dias tocados (`app/ponto/portas/celula.py:501`); o filtro admite célula/data_turno, mas a decisão consulta `dia_do_fato`, `fatos_do_dia` e `premissa_morta` (`app/chamados/services/feriado_reavaliar.py:76`, `:30`, `:33`, `:38`). O fechamento usa `reconciliar` (`:47`), cujo escritor de estado move `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em` e urgência (`app/chamados/models.py:228`, `:244`, `:246`, `:296`). Os receivers podem resolver perguntas e julgar novamente o dia (`app/chamados/signals.py:114`, `:216`). Perguntas recebem carimbo/via e, quando há resposta textual, os carimbos de resposta/validação pela porta `carimbar`; a relavra move `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/services/disputa_emissao.py:1860`; `app/chamados/models.py:3128`, `:3130`, `:3149`). Leitores desses estados incluem `PerguntaDisputa.estado` (`app/chamados/models.py:3043`) e o coletor que seleciona perguntas não materializadas (`app/chamados/services/disputa_emissao.py:1820`).

- **Humano não é garantia universal de “zero chamadas ao juiz”:** o rabo pula apenas o julgamento direto pelo filtro `_HUMANOS` (`app/ponto/portas/celula.py:427`, `:431`). Se o callback fechar uma falta espúria, o signal pode chamar o mesmo `julgar_celula` sem esse filtro (`app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/signals.py:216`). O juiz preserva o veredito administrativo, mas pode atualizar a ata (`app/ponto/portas/celula.py:1003`, `:1006`, `:1008`). Os novos testes sem chamado espúrio não exercitam essa cascata (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:26`, `:99`). Dia futuro continua barrado pelo próprio cartório (`app/ponto/services/cartorio.py:1985`).

- **Atomic externo:** o rabo está depois do atomic interno (`app/ponto/portas/celula.py:785`, `:811`), mas o juiz roda sincronicamente (`:434`), enquanto dinheiro e reavaliação são agendados por `on_commit` (`app/ponto/services/fechamento.py:1824`; `app/ponto/portas/celula.py:502`). A ordem de registro é juiz → dinheiro → callback do rabo; confirmação experimental da execução em transação externa: **[não verificado]**. Não se deve dizer “já commitada” apenas porque saiu do atomic interno: existe caminho do juiz por `reconciliar`, que abre `atomic(savepoint=False)` (`app/ponto/services/cartorio.py:1499`; `app/chamados/models.py:650`). Exceção nesse bloco, depois engolida pelo rabo (`app/ponto/portas/celula.py:437`), pode comprometer a transação externa; reprodução: **[não verificado]**. A gravação específica da ata/veredito tem atomic próprio (`app/ponto/portas/celula.py:904`).

- **Callbacks anteriores ao dinheiro:** o próprio julgamento pode fechar disputa (`app/chamados/services/disputa_emissao.py:1960`), cujo signal agenda retomada de regularizações (`app/chamados/signals.py:134`, `:144`); saves de pergunta também agendam fechamento de beco (`:227`, `:238`). Logo o dinheiro da VOLTA não é necessariamente o primeiro callback registrado (`app/ponto/portas/celula.py:434`, `:458`). Existência desses dados e recálculos indiretos no caso concreto: **[não verificado]**.

- **Exceções comuns:** julgamento direto e disparo financeiro têm `try/except` separados (`app/ponto/portas/celula.py:437`, `:459`); o callback protege rejunção e falta espúria separadamente com `engolir` (`:495`, `:499`), que captura `Exception` (`app/core/observ.py:88`). O serviço financeiro também protege execução e agendamento (`app/ponto/services/fechamento.py:1818`, `:1825`). Isso sustenta best effort para exceções previstas nessas fronteiras; não sustenta garantia absoluta contra rollback de atomic externo (`app/chamados/models.py:650`).

- **Rejunção duplicada existente:** a VOLTA já emite `EventoCelula` (`app/ponto/portas/celula.py:796`), cujo consumidor também chama rejunção (`app/chamados/reconciliador.py:1364`). O novo callback acrescenta outra tentativa (`app/ponto/portas/celula.py:498`); ambas passam pelo filtro de FK nula e pela guarda da porta (`app/chamados/rejuncao.py:62`; `app/chamados/services/vinculos.py:96`). É repetição de disparo do mesmo escritor, não um segundo juiz (`app/chamados/rejuncao.py:107`).

## 10/11 00:58 · sessao 68effa817189 · - · - · origem=Ronald (tmux)

Achados financeiros, todos da cópia:

- **Competência exportada é alcançada.** `apesar_da_lavra` apenas dispensa a guarda de restauração do DNA; não é passado para o financeiro. O rabo chama `recalcular_por_evento` com recálculo habilitado por padrão; o callback chama o escritor mensal sem trava geral por exportação. Há uma exceção concreta: **09/2026 exportada continua barrada pela `GUARDA_COMPETENCIA`**, que exclui esses colaboradores do universo antes das escritas. (`app/ponto/portas/celula.py:779`, `app/ponto/portas/celula.py:811`, `app/ponto/portas/celula.py:445`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:558`.)

- **O motivo não acompanha a trilha específica de exportada.** A VOLTA fornece `motivo_recalculo`; o rabo encaminha como `motivo`; mas `recalcular_por_evento` não repassa `motivo_exportada` ao escritor. O aviso da competência exportada pode registrar `'(sem motivo declarado)'`; o motivo aparece somente no `info` posterior do callback. (`app/ponto/portas/celula.py:812`, `app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:547`, `app/ponto/services/fechamento.py:1813`.)

- **N VOLTAs em lote produzem N pedidos de recálculo**, mesmo pertencendo à mesma competência: a deduplicação é um dicionário local dentro de cada execução do rabo; a VOLTA entrega sempre `{dia}`. O callback não implementa deduplicação entre chamadas. (`app/ponto/portas/celula.py:450`, `app/ponto/portas/celula.py:456`, `app/ponto/portas/celula.py:811`, `app/ponto/services/fechamento.py:1824`.)

- **Futuro e humano: disparo financeiro não prova que o previsto volta a 240.** O rabo pula o juiz nesses casos, mas solicita dinheiro. O leitor da grade prefere o `minutos_previstos` já lavrado na ata; só usa o juiz do previsto quando o campo é `None`. Portanto uma ata preservada com 480 pode produzir novamente `DiaPago.minutos_previstos=480`, apesar do DNA restaurado. O teste desses casos espia o disparo financeiro, sem executar essa conta. (`app/ponto/portas/celula.py:427`, `app/ponto/portas/celula.py:431`, `app/ponto/portas/celula.py:445`, `app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:399`, `app/escala/services/leitor_celula.py:411`, `app/ponto/services/fechamento.py:1060`, `app/ponto/services/dia_pago.py:387`.)

- **O financeiro reescreve a competência inteira desse colaborador**, não apenas o dia revertido. O escritor recebe `colaborador_ids=[pk]`; produz o fechamento da janela; lavra as versões `motor` e, a partir de 10/2026, `oraculo`; cada `lavrar` apaga e recria as linhas daquela versão/competência, inclusive ajuste mensal. (`app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/fechamento.py:1277`, `app/ponto/services/dia_pago.py:348`, `app/ponto/services/dia_pago.py:359`.)

Escritores e leitores:

| Campos | Escritor efetivo | Leitores concretos |
|---|---|---|
| `FechamentoMensal`: horas trabalhadas/noturnas/extras e seus splits, folga, atraso, saída antecipada, falta, intra, turnos abertos, inconsistências, DSR, banco, minutos previstos/abonados/realizados, dias previstos/incertos, motivos/causa do espelho e carimbos | Aberto: `save`; demais status: `update` com a lista de campos. (`app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1160`.) | GET do fechamento consulta o gravado; TXT lê os fechamentos e extrai eventos; percentual usa o previsto materializado quando há carimbo. (`app/ponto/views.py:1134`, `app/folha/export.py:711`, `app/folha/export.py:722`, `app/folha/export.py:1037`.) |
| `DiaPago`: 15 campos de dia e dois de ajuste, `furo_apurado`, detalhe, versão e carimbos | `dia_pago.lavrar`, alimentado pelo mesmo resultado do fechamento; `delete` + `bulk_create`. (`app/ponto/services/dia_pago.py:34`, `app/ponto/services/dia_pago.py:43`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/dia_pago.py:345`, `app/ponto/services/dia_pago.py:359`.) | Calendário lê linhas por data e `rubricas_do_dia`; testemunhas do export leem versão `motor`. (`app/colaboradores/services/calendario.py:290`, `app/colaboradores/services/calendario.py:655`, `app/ponto/services/dia_pago.py:855`, `app/folha/porta_export.py:75`, `app/folha/porta_export.py:284`.) |

Semântica:

- **Competência usa a data do fato**, sem substituir por hoje ou pelo mês civil: callback → `competencia_do_fato` → normalização local de datetime → `competencias_do_intervalo` → `janela_atual`, cujo corte vem da empresa. (`app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1602`, `app/ponto/services/fechamento.py:1567`, `app/ponto/services/fechamento.py:1594`, `app/ponto/services/dia_pago.py:715`, `app/ponto/janelas.py:60`.)

- **Dia dos períodos usa o mapa do motor**, não necessariamente a data civil de cada entrada: o fechamento passa `resultado.dia_da_jornada`; `_dia_de` consulta esse mapa e só depois cai em `localtime(entrada).date()`. A janela de batidas inclui as pontas para capturar turnos cruzando a competência. (`app/ponto/services/fechamento.py:1217`, `app/ponto/services/dia_pago.py:126`, `app/ponto/services/fechamento.py:624`.)

- Há documentação antiga que ainda descreve a data civil da entrada como regra universal, mas o código acima prioriza o mapa. Isso aparece no comentário do calendário e na docstring/modelo `DiaPago`; não prova um novo segundo significado introduzido pelo diff. (`app/colaboradores/services/calendario.py:288`, `app/ponto/models.py:298`, `app/ponto/models.py:319`, `app/ponto/services/dia_pago.py:128`.)

Exceções e atomicidade financeira:

- O callback financeiro engole tanto falhas de execução quanto falhas ao registrar `on_commit`; não há operação de desfazer DNA nesse caminho. (`app/ponto/services/fechamento.py:1818`, `app/ponto/services/fechamento.py:1823`.)
- A persistência de `FechamentoMensal` ocorre antes da lavratura de `DiaPago`; falhas da lavratura são engolidas. O corpo de `lavrar` não envolve seu `delete` + `bulk_create` em um `atomic`. Assim o código permite divergência parcial em falha, conforme transação/erro concreto; ocorrência em produção é **[não verificado]**. (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:1201`, `app/ponto/services/fechamento.py:1238`, `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:359`.)

Não encontrei, nesses leitores financeiros examinados, uma condição que exija “VOLTA não recalcula”: eles consomem o gravado e aceitam sua atualização pelo evento. Essa conclusão cobre os caminhos citados, não prova ausência em toda a árvore. (`app/colaboradores/services/calendario.py:290`, `app/colaboradores/services/calendario.py:655`, `app/folha/export.py:711`, `app/folha/porta_export.py:284`.)

## 10/11 00:58 · sessao 1e22f8bbbcd9 · - · - · origem=Ronald (tmux)

- **Mordida real no golden:** `_dia_pago()` usa `.get()` de `DiaPago` motor/tipo dia; falta de fixture falha, não passa vazia. A montagem exige previsto 480 antes da VOLTA; golden exige DNA restaurado, `dna_anterior=None`, ata 240 e DiaPago 240. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:56`, `:77`, `:88`)

- **Futuro/humano:** `juiz.assert_not_called()` isolado seria negativo fraco, mas vem acompanhado de `dinheiro.assert_called_once_with(...)`, DNA restaurado e ata ainda 480. Portanto o desaparecimento completo do rabo reprova esses casos. Porém não demonstram resultado financeiro: mockam o dinheiro; a ata deliberadamente permanece 480. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:99`, `:111`, `:117`)

- **RED “4 failures”:** compatível com três comportamentais mais o AST real, mas execução contra base e banco não demonstrada pelo material disponível **[não verificado]**. O exemplo `_FONTE_BASE` é apenas string reduzida; seu teste exige rejeição, portanto continua verde quando o verificador funciona. (`app/ponto/tests/test_reverter_regen_sem_banco.py:25`, `:81`, `:90`)

- **AST:** executei somente leitura com `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest app.ponto.tests.test_reverter_regen_sem_banco -v`: **11 testes OK** **[não verificado por arquivo:linha; execução nesta sessão]**. Valida uma definição, uma chamada comum, ausência de escritores diretos, posição imediatamente após o `with` e argumentos exatos. (`app/ponto/tests/test_reverter_regen_sem_banco.py:54`, `:59`, `:68`, `:76`)

- **Limite concreto AST:** executei mutações apenas em memória e o verificador aceitou (a) `return True` dentro do `atomic`, tornando o rabo inalcançável; (b) corpo `_o_chao_do_dia_mudou` reduzido a `pass` **[não verificado por arquivo:linha; execução nesta sessão]**. A causa está no escopo do verificador: examina chamadas e posição sintática apenas na VOLTA, sem provar fluxo nem comportamento do helper. Os comportamentais cobrem parte dessa lacuna. (`app/ponto/tests/test_reverter_regen_sem_banco.py:54`, `:59`, `:68`)

- **Lacunas dos novos testes:** não há casos de exportada/`apesar_da_lavra`, sem DNA anterior, lote/dedup, exceções, callbacks de rejunção/falta espúria nem `atomic` externo. O patch de `on_commit` chama callbacks imediatamente e ignora a semântica real de commit, por isso não demonstra ordem sob transação externa. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:20`, `:74`, `:85`, `:88`, `:111`, `:117`)

- **Escritor ata/veredito:** `ata_do_dia` produz tipo, lâmpadas, minutos e contagens; cartório chama `cel.lavrar_veredito(...save=False, ata=...)` e `carimbar` persiste os campos pela porta `salvar_e_emitir`, que grava célula e evento no mesmo `atomic`. (`app/ponto/services/cartorio.py:696`, `:704`, `:1813`, `:1833`; `app/ponto/portas/celula.py:1032`, `:1052`, `:1070`, `:899`)

- **Leitores:** grade lê ata e dá prioridade ao previsto lavrado; somente ausência dessa chave consulta `minutos_previstos_do_dia`. Transporta veredito e ata para consumidores. `chamados.juizes.dia_sem_trabalho` lê `trabalha`/`ata.tipo_dia`, com exceção parcial; export lê veredito/protestos para retenção. (`app/escala/services/leitor_celula.py:379`, `:397`, `:399`, `:411`, `:424`, `:428`; `app/chamados/juizes.py:426`; `app/folha/export.py:1206`, `:1233`, `:1245`)

- **Guardas e semântica:** rabo exclui datas futuras e `_HUMANOS=('trabalhou','faltou','justificado')`; esses são precisamente os vereditos administrativos. Mesmo excluídos, dinheiro é disparado; como grade prioriza ata, DNA restaurado não implica previsto restaurado nesses casos. O teste confirma explicitamente ata 480. (`app/ponto/portas/celula.py:426`, `:431`, `:445`, `:458`; `app/colaboradores/services/pos_vinculo.py:14`; `app/escala/models.py:1759`; `app/escala/services/leitor_celula.py:399`; `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:109`)

- **Dia civil versus turno:** `julgar_celula` rejeita futuro; hoje exige fim de turno salvo folga/teto explícito. O batch trata passado cujo turno segue aberto e conta batidas pelo dia-turno da grade, não pela data civil da batida. Portanto o filtro `dia <= hoje` do rabo não substitui todas essas guardas do juiz. (`app/ponto/services/cartorio.py:1985`, `:1993`, `:1997`, `:1356`, `:1379`)

- **Humano não significa congelamento universal da ata:** o rabo pula vereditos humanos, mas a operação `lavrar_veredito` quando chamada pelo cartório sobre `veredito_via='admin'` preserva veredito e pode atualizar ata. Portanto a promessa do teste é sobre esse gatilho direto; callbacks e outros emissores precisam ser auditados separadamente. (`app/ponto/portas/celula.py:431`, `:1002`, `:1005`, `:1008`)

## 10/11 00:54 · sessao f30997273ac9 · PAUTA-SEM-TURNO-PREFIXO-RESERVADO · verificador · origem=Code

**1. CONFORME quanto aos leitores encontrados na cópia; comparação BYTE A BYTE com a base: [nao verificado].** O histórico necessário para conferir `a4b809999 → d2230cd…` não está disponível nesta cópia; portanto, preservação dos valores anteriores e diff integral permanecem **[nao verificado]**.

Valores atuais e donos conferidos abaixo. Na coluna final, escrita e leitura referem-se ao mesmo arquivo, salvo indicação explícita.

| Constante e valor atual | Declaração | Escritor; leitor |
|---|---|---|
| `PAUTA_LIMBO_EXPORTADA = 'LIMBO EM COMPETENCIA EXPORTADA'` | `app/pautas/marcadores.py:8` | `app/chamados/services/fabrica_da_celula.py:274`; leitura `:254` |
| `PAUTA_SEM_TURNO = 'CADASTRO: PASSADO SEM TURNO PREVISTO'` | `app/pautas/marcadores.py:11` | `app/chamados/services/fabrica_da_celula.py:362`; leitura `:305` |
| `ORFA_ENTERRADA = '[ORFA-ENTERRADA]'` | `app/pautas/marcadores.py:14` | `app/chamados/management/commands/reabrir_orfas_enterradas.py:209`; leitura `:202` |
| `PASSIVO_VALIDADO = '[PASSIVO-VALIDADO]'` | `app/pautas/marcadores.py:17` | `app/chamados/services/passivo_validado.py:230`; leitura `:164` |
| `PASSIVO_VALIDADO_FORA = '[PASSIVO-VALIDADO-FORA]'` | `app/pautas/marcadores.py:20` | `app/chamados/services/passivo_validado.py:267`; leitura `:256` |
| `ARVORE_VERMELHA = 'ARVORE VERMELHA'` | `app/pautas/marcadores.py:23` | `app/core/management/commands/alarme_arvore.py:52`; leitura `:16` |
| `ESTEIRA = 'ESTEIRA'` | `app/pautas/marcadores.py:26` | `app/core/management/commands/alarme_esteira.py:56`; leitura `:15` |
| `CRON_QUEBRADO = '[CRON QUEBRADO]'` | `app/pautas/marcadores.py:29` | `app/core/services/crons_quebrados.py:72`; leitura `:54` |
| `FERIAS['pa_vencido'] = '[FERIAS/PA-VENCIDO]'` | `app/pautas/marcadores.py:33` | `app/ferias/management/commands/pautas_de_ferias.py:164`; leitura `:152` |
| `FERIAS['pa_vencendo'] = '[FERIAS/PA-VENCENDO]'` | `app/pautas/marcadores.py:34` | `app/ferias/management/commands/pautas_de_ferias.py:164`; leitura `:152` |
| `FERIAS['cadastro_pa'] = '[FERIAS/CADASTRO]'` | `app/pautas/marcadores.py:35` | `app/ferias/management/commands/pautas_de_ferias.py:164`; leitura `:152` |
| `PRE_FECHAMENTO = '[PRE-FECHAMENTO]'` | `app/pautas/marcadores.py:39` | `app/folha/management/commands/pre_fechamento.py:111`; leituras `:92`, `:98` |
| `DIA_EM_ABERTO = '[DIA EM ABERTO]'` | `app/pautas/marcadores.py:42` | `app/pautas/management/commands/pauta_dia_em_aberto.py:140`; leitura `:108` |
| `FOLGA_EXPORTADA = '[FOLGA NAO DEFINIDA - EXPORTADA]'` | `app/pautas/marcadores.py:45` | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:135`; leitura `:126` |
| `PAGO_DEVIDO_EXPORTADA = '[PAGO X DEVIDO - EXPORTADA]'` | `app/pautas/marcadores.py:48` | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:208`, `:212`; leituras `:243`, `:250`, `:277` |
| `REVISAO_AUSENCIA = '[A6/REVISAO-SEMANAL]'` | `app/pautas/marcadores.py:51` | `app/ponto/services/vigia_ausencia.py:239` → `app/pautas/semanal.py:278`; leituras `app/pautas/semanal.py:199`, `:228` |
| `REVISAO_SUPERVISAO = '[SUP/REVISAO-SEMANAL]'` | `app/pautas/marcadores.py:54` | `app/escala/services/fase_12x36.py:278` → `app/pautas/semanal.py:278`; leituras `app/pautas/semanal.py:199`, `:228` |
| `TEXTO_AFASTADO`; `AFASTADO = TEXTO_AFASTADO[:40]` | `app/pautas/marcadores.py:57`, `:77` | `app/ponto/services/afastado_avisa.py:76`; leitura `:63` |
| `HE_EXPORTADA = '[HE-AUTORIZADA-EM-EXPORTADA]'` | `app/pautas/marcadores.py:62` | `app/ponto/services/autorizacao_he_periodo.py:245`; leitura `:242` |
| `COBERTURA_A_CONFIRMAR = '[COBERTURA-A-CONFIRMAR]'` | `app/pautas/marcadores.py:65` | `app/ponto/services/cobertura.py:336`; leitura `:243` |
| `APP_401 = '[APP-401]'` | `app/pautas/marcadores.py:68` | `app/ponto/services/pautas_dos_deslogados.py:78`; leitura `:66` |
| `DISCORDANTE = '[DISCORDANTE]'` | `app/pautas/marcadores.py:71` | `app/ponto/services/pautas_dos_discordantes.py:180`; leituras `:98`, `:166` |
| `TRANCA = '[TRANCA]'` | `app/pautas/marcadores.py:74` | `app/chamados/services/competencia_trancada.py:235`; leitura `:221` |
| `SEM_DOCUMENTO = '[A4/SEM-DOCUMENTO]'` | `app/pautas/marcadores.py:80` | `app/ponto/services/ausencia.py:1060`; leitura `:1044` |
| `FERIAS_RECUSADA = '[A1-FERIAS/AG#'` | `app/pautas/marcadores.py:81` | `app/ponto/services/ausencia.py:1250`; leitura `:1234` |
| `ESMERIL = '[ESMERIL '` — espaço final significativo | `app/pautas/marcadores.py:84` | `app/ponto/services/pautas_do_esmeril.py:79`; formação `:16`, `:50`; leituras `:66`, `:67` |

`TEXTO_AFASTADO` contém exatamente a concatenação abaixo; seu prefixo reservado é `Bateu ponto durante afastamento. A batid` (`app/pautas/marcadores.py:57`, `:77`):

```text
Bateu ponto durante afastamento. A batida foi registrada e o dia segue explicado pela ausencia -- nao e recusa. Confira se o afastamento ainda vale: se o colaborador voltou, encerre a AUSENCIA de afastamento (a situacao sai dela, nao do cadastro); se nao voltou, oriente que nao precisa bater.
```

**CONFORME — as derivações encontradas estão cobertas pela guarda atual**, pois continuam começando pelo reservado: cron acrescenta nome/empresa; semanal acrescenta semana; pago/devido distingue cabeça e filha; férias recusada acrescenta identificador e `]`; esmeril acrescenta ação e `]` (`app/core/services/crons_quebrados.py:54`; `app/pautas/semanal.py:199`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:243`, `:250`; `app/ponto/services/ausencia.py:1231`; `app/ponto/services/pautas_do_esmeril.py:16`; `app/pautas/services.py:45`).

Não apareceu outro prefixo de produção fora do catálogo no rastreio estático. A ausência absoluta de leitores dinâmicos ou de outras formas não detectáveis por essa busca permanece **[nao verificado]**. O selo novo fiscaliza somente keywords `texto__startswith`; não fiscaliza `contains`, `icontains` ou regex (`app/pautas/tests/test_prefixos_reservados_ast.py:155-164`).

**2. PROCEDENTE — a guarda recusa prosa além da forma reconhecida pelos leitores.** Ela aplica `strip().casefold()` e compara o prefixo bruto de todos os reservados (`app/pautas/services.py:44-45`).

| Caso | Resultado e diferença para o leitor |
|---|---|
| `ESTEIRANTE` ou `ESTEIRA de compras` | A guarda bloqueia pelos primeiros **7 caracteres**; o leitor exige `ESTEIRA <chave>:` (`app/pautas/marcadores.py:26`; `app/core/management/commands/alarme_esteira.py:15`). |
| `ARVORE VERMELHA no jardim` | Bloqueio pelos primeiros **15 caracteres**, mas aqui o próprio leitor também captura essa prosa (`app/pautas/marcadores.py:23`; `app/core/management/commands/alarme_arvore.py:16`). |
| Texto começando por `Bateu ponto durante afastamento. A batid` | Bloqueio pelos primeiros **40 caracteres**, embora o escritor emita a sentença completa; o leitor realmente usa esses 40 (`app/pautas/marcadores.py:77`; `app/ponto/services/afastado_avisa.py:63`, `:76`). |
| `[A1-FERIAS/AG#abc] relato` | A guarda bloqueia o prefixo aberto; o leitor monta identificador de agendamento seguido de `]` (`app/pautas/marcadores.py:81`; `app/ponto/services/ausencia.py:1231-1234`). |

Os leitores não fazem `casefold` explicitamente; há também leitura Python por `str.startswith`, sensível à caixa (`app/ponto/services/pautas_dos_discordantes.py:98`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:277`). Collation e comportamento efetivo do banco de produção: **[nao verificado]**.

**Correção mínima:** normalizar como o escritor (`strip`) e reservar as formas efetivamente lidas, incluindo delimitadores e sufixos necessários; retirar `casefold` onde a leitura for sensível à caixa (`app/pautas/services.py:177`; `app/core/management/commands/alarme_esteira.py:15`; `app/ponto/services/ausencia.py:1231`).

Para `ARVORE VERMELHA` e `AFASTADO`, **não existe ajuste somente na guarda que simultaneamente libere essa prosa e preserve os leitores atuais**: eles próprios classificam a prosa como identidade do sistema. É necessário estreitar também o leitor ou distinguir origem/identidade técnica do texto; compatibilidade histórica dessa alteração: **[nao verificado]** (`app/core/management/commands/alarme_arvore.py:16`; `app/ponto/services/afastado_avisa.py:63`).

Ocorrências humanas legítimas em produção: **[nao verificado]**.

**3. PROCEDENTE — existe comando humano sem guarda.** `suporte_hasner --responder ID --texto ...` aceita texto livre, verifica apenas vazio e chama `sv.responder` diretamente (`app/pautas/management/commands/suporte_hasner.py:45`, `:47`, `:127-134`). O serviço encaminha ao escritor comum, que não aplica a reserva humana (`app/pautas/services.py:177`, `:194`, `:208-214`).

**Correção mínima:** chamar `validar_texto_humano(texto)` no comando antes de responder e converter `ValidationError` em `CommandError` (`app/pautas/management/commands/suporte_hasner.py:132-138`).

Esse comando restringe o pai ao departamento de suporte; não prova, por si, falsificação de cabeça `PAUTA_SEM_TURNO`, cujo seletor exige `pai__isnull=True`. Impacto concreto em produção: **[nao verificado]** (`app/pautas/management/commands/suporte_hasner.py:122`; `app/chamados/services/fabrica_da_celula.py:304`).

**CONFORME nas rotas examinadas:** escrever e responder HTTP chamam a guarda; o URLconf de Pauta não expõe edição/encaminhamento; a API de mensageria examinada lista pautas (`app/pautas/views.py:76`, `:114`; `app/pautas/urls.py:7-14`; `app/api/views_mensageria.py:1323-1347`). Ausência universal de admin ou serializador registrado dinamicamente: **[nao verificado]**.

**4. CONFORME — catálogo folha e direção A16.** `pautas/marcadores.py` contém dados e derivações locais, sem imports de donos/models; os donos importam esse catálogo diretamente (`app/pautas/marcadores.py:1-111`; `app/ponto/services/afastado_avisa.py:11`; `app/chamados/services/fabrica_da_celula.py:239`, `:296`). Essa dependência não cria retorno pelo catálogo nem cruza a fronteira `ponto ↔ chamados` fiscalizada pelo A16 (`app/core/tests/test_contract_direcao_a16.py:99-104`, `:112-137`).

Quais donos **passaram** a importar `pautas.*`, comparados à base: **[nao verificado]**. A suíte A16 completa também permanece **[nao verificado]**; o teste novo executa somente seu detector extraído (`app/pautas/tests/test_prefixos_reservados_ast.py:236-265`).

**5. PROCEDENTE — há bloqueadores de certificação.**

- **Censo incompatível com a montagem da suíte:** executa `git ls-files`, mas a suíte monta apenas `app/` e a cópia de commit deliberadamente não contém `.git`. Correção mínima: enumerar os Python da árvore montada, preservando exclusões e sem depender do Git (`app/pautas/tests/test_prefixos_reservados_ast.py:19-25`; `bin/suite.sh:100-103`; `bin/arvore_do_push.sh:7`).

- **Selo legado do juiz ficou incompatível:** exige a definição de `PAUTA_SEM_TURNO` na fábrica e proíbe sua definição/uso nos novos locais. A reprodução isolada do corpo AST falhou nessa exigência; execução no runner Django: **[nao verificado]**. Correção mínima: admitir estritamente definição/inclusão no catálogo e import na fábrica, mantendo os usos operacionais restritos aos donos (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:147-150`, `:251-260`; `app/chamados/services/fabrica_da_celula.py:296`; `app/pautas/marcadores.py:11`, `:88`).

- **Os 59 mutantes não rodam automaticamente na suíte:** o executor chama o laço somente em `main`, sob `__name__ == '__main__'`; o runner normal delega à descoberta de testes. Correção mínima: expor uma asserção coletável que execute os mutantes em memória, após eliminar a dependência de Git (`app/pautas/tests/mutar_prefixos_reservados.py:85`, `:110`, `:151-152`; `app/core/runner_quarentena.py:75-76`; `app/core/tests/test_censo_escritas.py:278`).

- **Skip amplo demais:** qualquer `ImportError`, inclusive transitivo de produção, vira “Django não instalado”. Correção mínima: permitir skip somente pela ausência efetiva de Django e propagar os demais erros (`app/pautas/tests/test_prefixos_reservados.py:134-146`).

**CONFORME — os testes normais têm asserções reais**, incluindo recusa antes da escrita e contagem de cabeças/filhas quando Django carrega. A ausência de Django no construtor não torna o skip permanente: existe ramo de integração e a dependência está declarada (`app/pautas/tests/test_prefixos_reservados.py:81-98`, `:147`, `:173-200`; `app/requirements.txt:13`).

O selo AST verifica origem do **leitor**, não igualdade entre escritor e leitor; tampouco verifica os sufixos derivados. Portanto, não certifica preservação BYTE A BYTE (`app/pautas/tests/test_prefixos_reservados_ast.py:126-133`, `:155-164`). Integração Django, RED na base e execução dos 59 mutantes nesta cópia: **[nao verificado]**.

**6. Classes solicitadas.**

| Classe | Veredito |
|---|---|
| **2º juiz** | **CONFORME — não identificado nesta regra.** Escritor consulta `pauta_cadastro_existente` após a trava; contador reutiliza o seletor (`app/chamados/services/fabrica_da_celula.py:318`, `:336`, `:355-358`). |
| **2º escritor fora da porta** | **CONFORME — não identificado no rastreio.** Respostas e comando usam o escritor comum; o defeito do comando é contornar a guarda humana (`app/pautas/services.py:194`, `:208`; `app/pautas/management/commands/suporte_hasner.py:133`). |
| **Relógio fora da borda** | **CONFORME no catálogo/guarda: não.** A validação usa somente texto e catálogo; alterações históricas fora desse escopo: **[nao verificado]** (`app/pautas/services.py:38-48`; `app/pautas/marcadores.py:77`). |
| **Derivação paralela de fonte única** | **CONFORME na cópia: não identificada.** O corte de 40 caracteres nasce no catálogo e o dono importa texto e prefixo (`app/pautas/marcadores.py:77`; `app/ponto/services/afastado_avisa.py:11`). |
| **Selo/RED sem mordida** | **PROCEDENTE — sim quanto à certificação automática dos mutantes**, além dos bloqueadores de Git e contrato legado. Correções no item 5; os testes normais não são vazios (`app/pautas/tests/mutar_prefixos_reservados.py:151`; `app/pautas/tests/test_prefixos_reservados.py:81`). |
| **Campo com dois significados — ESMERIL-SEMANTICA** | **PROCEDENTE — sim, persiste.** `texto` guarda prosa e também identidade/ação interpretada pelo sistema. Fechar todas as portas humanas reduz a colisão; eliminar a dualidade exige identidade/origem técnica separada da prosa (`app/pautas/models.py:89`; `app/ponto/services/pautas_do_esmeril.py:50`, `:66-68`; `app/chamados/services/fabrica_da_celula.py:305`). |

## 10/11 00:53 · sessao 45b533905b0e · REVERTER-DIA-SEM-RECALCULO · construtor · origem=Code

Criados somente:

- [test_reverter_regen_recalcula_dinheiro.py:61](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:61): arranjo 240 → IDA 480 → VOLTA 240, com `on_commit` imediato; inclui futuro e veredito humano na mesma fixture.
- [test_reverter_regen_sem_banco.py:52](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_sem_banco.py:52): contrato AST e nove mutantes locais, sem variável de ambiente.

**RED do (b) na base:** 11 testes, uma falha no [fonte real:83](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_sem_banco.py:83): [nao verificado]

```text
AssertionError: VOLTA deve chamar o rabo comum uma vez e nenhum escritor direto: rabos=0; diretas=['julgar_celula']
FAILED (failures=1)
```

Comando executado:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_reverter_regen_sem_banco -v
```

Os nove mutantes foram rejeitados pelas asserções dos [casos:90](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_reverter_regen_sem_banco.py:90). Comando executado separadamente para cada um:

```bash
for caso in bloco_da_base sem_rabo rabo_duplicado juiz_paralelo \
  dinheiro_paralelo rabo_dentro_do_atomic dias_vazios \
  recalculo_desligado motivo_perdido
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
    "ponto.tests.test_reverter_regen_sem_banco.VerificadorMordeTest.test_morde_$caso" \
    -v || exit
done
```

**Mock existente:** nenhum ajuste indicado pela leitura. O [vizinho:262](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/tests/test_porta_celula_dia.py:262) intercepta `ponto.services.cartorio.julgar_celula`; o [rabo comum:425](/home/ronald/wt-codex-reverter-dia-sem-recalculo-004859/app/ponto/portas/celula.py:425) importa esse mesmo alvo e conserva os argumentos esperados. Compatibilidade executada: **[não verificado]**.

Ruff retornou `All checks passed!` nos dois arquivos:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff \
  check --no-cache \
  ponto/tests/test_reverter_regen_recalcula_dinheiro.py \
  ponto/tests/test_reverter_regen_sem_banco.py
```

**Sem rodar:** os três casos ORM de (a), inclusive seu RED financeiro; (b) pelo runner Django; e os vizinhos `test_porta_celula_dia` e `test_ordem_regen_julga_antes_do_dinheiro`. Django está ausente no Python consultado, e banco foi vedado nesta volta. Resultados dessas execuções: **[não verificado]**.

## 10/11 00:51 · sessao 3dce6c2db4b1 · - · - · origem=Ronald (tmux)

1. **CONFORME quanto ao catálogo atual:** censo em memória de todos os Python de produção encontrou 26 reservados e nenhum `texto__startswith` fora dele; os pontos de entrada são `app/pautas/marcadores.py:86` e `app/pautas/tests/test_prefixos_reservados_ast.py:155`. Não encontrei `texto__contains`/`texto__icontains` de produção na cópia; essa ausência resulta da busca, não tem linha própria. **Comparação BYTE A BYTE com a4b809999: [nao verificado]**, porque a cópia não contém `.git`/base acessível.

Mapa atual conferido; E = escrita; L = leitura:

| Constante; valor/declaração | E | L |
|---|---|---|
| `PAUTA_LIMBO_EXPORTADA = 'LIMBO EM COMPETENCIA EXPORTADA'`, `app/pautas/marcadores.py:8` | `app/chamados/services/fabrica_da_celula.py:274` | mesmo arquivo:254 |
| `PAUTA_SEM_TURNO = 'CADASTRO: PASSADO SEM TURNO PREVISTO'`, catálogo:11 | fábrica:362 | fábrica:305 |
| `ORFA_ENTERRADA = '[ORFA-ENTERRADA]'`, catálogo:14 | `app/chamados/management/commands/reabrir_orfas_enterradas.py:209` | mesmo:202 |
| `PASSIVO_VALIDADO = '[PASSIVO-VALIDADO]'`, catálogo:17 | `app/chamados/services/passivo_validado.py:230` | mesmo:164 |
| `PASSIVO_VALIDADO_FORA = '[PASSIVO-VALIDADO-FORA]'`, catálogo:20 | passivo:267 | passivo:256 |
| `ARVORE_VERMELHA = 'ARVORE VERMELHA'`, catálogo:23 | `app/core/management/commands/alarme_arvore.py:52` | mesmo:16 |
| `ESTEIRA = 'ESTEIRA'`, catálogo:26 | `app/core/management/commands/alarme_esteira.py:56` | mesmo:15 |
| `CRON_QUEBRADO = '[CRON QUEBRADO]'`, catálogo:29 | `app/core/services/crons_quebrados.py:72` | mesmo:54 |
| `FERIAS`: `'[FERIAS/PA-VENCIDO]'`, `'[FERIAS/PA-VENCENDO]'`, `'[FERIAS/CADASTRO]'`, catálogo:32 | `app/ferias/management/commands/pautas_de_ferias.py:164` | mesmo:152 |
| `PRE_FECHAMENTO = '[PRE-FECHAMENTO]'`, catálogo:39 | `app/folha/management/commands/pre_fechamento.py:111` | mesmo:92 e :98 |
| `DIA_EM_ABERTO = '[DIA EM ABERTO]'`, catálogo:42 | `app/pautas/management/commands/pauta_dia_em_aberto.py:140` | mesmo:108 |
| `FOLGA_EXPORTADA = '[FOLGA NAO DEFINIDA - EXPORTADA]'`, catálogo:45 | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:135` | mesmo:126 |
| `PAGO_DEVIDO_EXPORTADA = '[PAGO X DEVIDO - EXPORTADA]'`, catálogo:48 | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:208` e :212 | mesmo:243, :250 e :277 |
| `REVISAO_AUSENCIA = '[A6/REVISAO-SEMANAL]'`, catálogo:51 | `app/ponto/services/vigia_ausencia.py:239` → `app/pautas/semanal.py:278` | semanal:199 e :228 |
| `REVISAO_SUPERVISAO = '[SUP/REVISAO-SEMANAL]'`, catálogo:54 | `app/escala/services/fase_12x36.py:278` → semanal:278 | semanal:199 e :228 |
| `TEXTO_AFASTADO`, 293 caracteres, catálogo:57; `AFASTADO = TEXTO_AFASTADO[:40]`, catálogo:77 | `app/ponto/services/afastado_avisa.py:76` | mesmo:63 |
| `HE_EXPORTADA = '[HE-AUTORIZADA-EM-EXPORTADA]'`, catálogo:62 | `app/ponto/services/autorizacao_he_periodo.py:245` | mesmo:242 |
| `COBERTURA_A_CONFIRMAR = '[COBERTURA-A-CONFIRMAR]'`, catálogo:65 | `app/ponto/services/cobertura.py:336` | mesmo:243 |
| `APP_401 = '[APP-401]'`, catálogo:68 | `app/ponto/services/pautas_dos_deslogados.py:78` | mesmo:66 |
| `DISCORDANTE = '[DISCORDANTE]'`, catálogo:71 | `app/ponto/services/pautas_dos_discordantes.py:180` | mesmo:98 e :166 |
| `TRANCA = '[TRANCA]'`, catálogo:74 | `app/chamados/services/competencia_trancada.py:235` | mesmo:221 |
| `SEM_DOCUMENTO = '[A4/SEM-DOCUMENTO]'`, catálogo:80 | `app/ponto/services/ausencia.py:1060` | mesmo:1044 |
| `FERIAS_RECUSADA = '[A1-FERIAS/AG#'`, catálogo:81 | ausência:1250 | ausência:1231 e :1234 |
| `ESMERIL = '[ESMERIL '`, catálogo:84 | `app/ponto/services/pautas_do_esmeril.py:79` (formatação :50 e :16) | mesmo:66 e :67 |

Nas linhas abreviadas “catálogo”, trata-se de `app/pautas/marcadores.py`; “fábrica”, `app/chamados/services/fabrica_da_celula.py`; “passivo”, `app/chamados/services/passivo_validado.py`; “semanal”, `app/pautas/semanal.py`; “ausência”, `app/ponto/services/ausencia.py`.

2. **PROCEDENTE: guarda mais larga que as buscas.** `strip().casefold()` + prefixo bruto está em `app/pautas/services.py:44`. Demonstração em memória: recusa `ESTEIRANTE`, `ESTEIRA de compras`, `esteira de compras`, `arvore vermelha no jardim`, `[A1-FERIAS/AG#abc] relato`. O leitor ESTEIRA exige `'ESTEIRA <chave>:'` (`app/core/management/commands/alarme_esteira.py:15`), e férias exige pk seguido de `]` (`app/ponto/services/ausencia.py:1231`). Esses exemplos não apresentam as formas completas exigidas pelos respectivos leitores.

Os prefixos têm 7 (`ESTEIRA`), 15 (`ARVORE VERMELHA`) e 40 caracteres (`'Bateu ponto durante afastamento. A batid'`); suas declarações são `app/pautas/marcadores.py:26`, :23 e :77. A guarda recusa prosa começando pelos 40 caracteres, enquanto o escritor emite os 293 de `TEXTO_AFASTADO` (`app/ponto/services/afastado_avisa.py:76`). A existência de casos humanos reais em produção é **[nao verificado]**.

Correção mínima: fazer a guarda reproduzir o texto persistido (`strip`, `app/pautas/services.py:177`) e os predicados reais dos leitores; eliminar `casefold` onde o leitor é sensível à caixa; reservar ESTEIRA com chave e `:`, férias recusada com número e `]`. A configuração da suíte usa PostgreSQL (`app/config/settings_test.py:4`); comportamento efetivo do banco não foi exercitado **[nao verificado]**.

**ARVORE/AFASTADO exigem corrigir também o leitor**: os leitores atuais capturam a própria prosa de prefixo comum (`app/core/management/commands/alarme_arvore.py:16`, `app/ponto/services/afastado_avisa.py:63`). Só afrouxar a guarda reabre a falsificação. Pode-se estreitar ambos os lados para a sentença de sistema completa, ou separar identidade/origem de texto livre; a compatibilidade com registros históricos é **[nao verificado]**.

3. **CONFORME quanto às diferenças locais verificadas:** as derivações presentes continuam começando pelo reservado: passivo inclui colab/competência (`app/chamados/services/passivo_validado.py:159`), cron inclui nome/empresa (`app/core/services/crons_quebrados.py:54`), semanal inclui semana (`app/pautas/semanal.py:199`), pago/devido inclui chave e formas distintas de cabeça/filha (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:243`, :250). A guarda bruta cobre essas formas, com excesso; não achei leitor dessas famílias cuja forma escapasse à guarda atual.

4. **CONFORME quanto a ESMERIL reservado:** escritor deriva `'[ESMERIL %s]'` (`app/ponto/services/pautas_do_esmeril.py:16`), leitor busca `'[ESMERIL '` e extrai ação até `]` (`mesmo:66`, :67). A guarda cobre inclusive ações desconhecidas, como o leitor amplo atual (`app/pautas/services.py:45`).

## 10/11 00:51 · sessao f41df8bfb784 · - · - · origem=Ronald (tmux)

4. **CONFORME:** catálogo sem imports e sem dependência de donos/models; import isolado carregou apenas `pautas` e `pautas.marcadores` (`app/pautas/marcadores.py:1`, `app/pautas/marcadores.py:86`). Donos importam diretamente esse módulo, por exemplo `app/ponto/services/afastado_avisa.py:11`, `app/chamados/services/fabrica_da_celula.py:239`, `app/core/management/commands/alarme_esteira.py:10`. Esses imports não cruzam a fronteira ponto↔chamados fiscalizada por A16 (`app/core/tests/test_contract_direcao_a16.py:99`, `app/core/tests/test_contract_direcao_a16.py:112`). O detector extraído passou; suíte A16 completa e comparação dos imports anteriores: **[nao verificado]** (`app/pautas/tests/test_prefixos_reservados_ast.py:236`).

5. **PROCEDENTE — infraestrutura do selo incompatível com runner:** o censo executa `git ls-files` (`app/pautas/tests/test_prefixos_reservados_ast.py:20`), enquanto a suite monta apenas `app/` em `/app` (`bin/suite.sh:100`, `bin/regua.sh:167`) e a porta declara que a cópia no container não contém `.git` (`bin/arvore_do_push.sh:7`). Correção mínima: enumerar os arquivos Python da árvore montada, preservando as exclusões atuais (`app/pautas/tests/test_prefixos_reservados_ast.py:23`). Execução real no container: **[nao verificado]**. Nesta cópia, rodagem isolada resultou em 12 testes, um erro por ausência de `.git` e um skip por Django ausente; causa localizada em `app/pautas/tests/test_prefixos_reservados_ast.py:20` e `app/pautas/tests/test_prefixos_reservados.py:142`.

**PROCEDENTE — mutantes fora da suite:** `mutar_prefixos_reservados.py` é executor optativo; o laço está em `main`, chamado somente sob `__name__ == '__main__'` (`app/pautas/tests/mutar_prefixos_reservados.py:85`, `app/pautas/tests/mutar_prefixos_reservados.py:110`, `app/pautas/tests/mutar_prefixos_reservados.py:151`). Suite e régua rodam descoberta normal de labels (`bin/suite.sh:102`, `bin/regua.sh:168`); o runner só delega e aplica quarentena (`app/core/runner_quarentena.py:75`). Mesmo carregado explicitamente pelo loader unittest, o módulo fornece apenas os três testes das classes importadas (`app/pautas/tests/mutar_prefixos_reservados.py:19`), sem executar os mutantes. Correção mínima: expor execução e asserção dos mutantes num teste coletável, após retirar dependência de `.git`.

**CONFORME, com limite:** testes normais têm mordida: verificam recusa antes da escrita (`app/pautas/tests/test_prefixos_reservados.py:81`), aceitação de prosa (`app/pautas/tests/test_prefixos_reservados.py:91`) e, quando Django está disponível, status e contagem real de cabeças/filhas (`app/pautas/tests/test_prefixos_reservados.py:173`, `app/pautas/tests/test_prefixos_reservados.py:186`). Ausência de Django no construtor não torna o skip permanente: existe ramo de integração após import bem-sucedido (`app/pautas/tests/test_prefixos_reservados.py:147`), e Django está declarado em `app/requirements.txt:13`.

**PROCEDENTE — skip excessivamente amplo:** qualquer `ImportError` das dependências transitivas vira “Django não instalado” (`app/pautas/tests/test_prefixos_reservados.py:134`, `app/pautas/tests/test_prefixos_reservados.py:142`). Correção mínima: condicionar somente a ausência efetiva de Django e propagar demais falhas de importação.

**Limite do selo:** ele verifica apenas keywords `texto__startswith`, não os escritores nem contains/icontains/regex (`app/pautas/tests/test_prefixos_reservados_ast.py:155`). Aceita origem da concatenação pela esquerda sem verificar sufixos (`app/pautas/tests/test_prefixos_reservados_ast.py:126`). Probe em memória com escritor divergente e leitor usando `TRANCA` retornou zero erros; portanto igualdade escritor/leitor não é certificada por esse selo (`app/pautas/tests/test_prefixos_reservados_ast.py:159`).

6. **Selo/RED sem mordida: SIM quanto à certificação automática dos mutantes; NÃO quanto a vazio dos testes normais**, conforme distinção acima (`app/pautas/tests/mutar_prefixos_reservados.py:151`, `app/pautas/tests/test_prefixos_reservados.py:81`). **Relógio fora da borda: não identificado no catálogo/guarda/testes novos**; a guarda usa apenas texto e catálogo (`app/pautas/services.py:38`), e o catálogo contém dados (`app/pautas/marcadores.py:8`). Demais classes ficam para a consolidação dos outros rastreios.

_3309 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
