# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 02:20** · publicado em **10/11/2026 02:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 10/11 02:11 · sessao a40989c19dee · FIO-E0-V4-E4A-2 · construtor · origem=Code

Aplicada a cura nos quatro itens, com execução Django e certificação pendentes [nao verificado]. [nao verificado]

Arquivos alterados e motivo:

| Arquivo | Alteração |
|---|---|
| [resposta_tardia.py:45](chamados/services/resposta_tardia.py:45) | Retirado o corte; resposta consulta `_renascer_por_premissa_viva`. |
| [fabrica_da_celula.py:67](chamados/services/fabrica_da_celula.py:67) | Helper nomeado pelo fato da lâmpada. |
| [disputa_emissao.py:757](chamados/services/disputa_emissao.py:757), [pergunta_no_app.py:35](chamados/services/pergunta_no_app.py:35) | Causa `nunca_bateu_sem_adesao`; destino C5 na descrição. |
| [disparar_perguntas_competencia.py:34](chamados/management/commands/disparar_perguntas_competencia.py:34) | Consulta C5 com mesmos `hoje/empresas`; desligado excluído vai ao balde RH na [linha 126](chamados/management/commands/disparar_perguntas_competencia.py:126). |
| [test_selo_emissor_fio_mudo.py:43](chamados/tests/test_selo_emissor_fio_mudo.py:43), [test_o261_fabrica_le_a_lampada.py:117](chamados/tests/test_o261_fabrica_le_a_lampada.py:117) | Fixtures com adesão coerente e emissão do evento ausente, preservando o estoque mudo. |
| [test_resposta_tardia.py:33](chamados/tests/test_resposta_tardia.py:33) | Fixture de furo ajustada; asserção original preservada; caso explícito `nunca_bateu` acrescentado. |
| [test_medida_turno_do_par.py:268](core/tests/test_medida_turno_do_par.py:268) | Registro atualizado para ocorrência real `1199`. |
| [test_e4a2_nunca_bateu.py:68](chamados/tests/test_e4a2_nunca_bateu.py:68) | Selo Django com vínculo, marcos vencidos, ata acusadora, controle positivo e mutantes. |

Apagado `chamados/tests/test_e4a2_nunca_bateu_sem_banco.py`; ausência conferida por `exists=False`, sem conferência git [nao verificado].

Censo dos campos e valores envolvidos:

| Campo/valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `veredito='nunca_bateu'` | Cartório calcula pela adesão em [cartorio.py:1165](ponto/services/cartorio.py:1165); modelo delega à porta em [models.py:1826](escala/models.py:1826). | Helper [fabrica_da_celula.py:69](chamados/services/fabrica_da_celula.py:69), repasse [disputa_emissao.py:758](chamados/services/disputa_emissao.py:758), contador e resposta tardia. |
| `status_local` | `_gravar_estado`, [models.py:228](chamados/models.py:228). | Seleção do contador e [resposta_tardia.py:35](chamados/services/resposta_tardia.py:35); renascimento permanece na [porta existente:983](chamados/models.py:983). |
| `situacao`, identidade/empresa | Porta do colaborador, [colaborador.py:55](colaboradores/services/colaborador.py:55); desligamento atribui em [desligamento.py:104](colaboradores/services/desligamento.py:104). | Novo filtro de universo em [disparar:38](chamados/management/commands/disparar_perguntas_competencia.py:38). |
| Mapa C5 `{colab: dias}` | Autoridade [cadastro_realidade.py:180](escala/services/cadastro_realidade.py:180), incluindo retenção dos desligados. | Contador consulta na [linha 34](chamados/management/commands/disparar_perguntas_competencia.py:34). |
| `nome/numero/esperado/dono/fora_da_c5/universo` | [Contador:46](chamados/management/commands/disparar_perguntas_competencia.py:46): `numero=|P|`, `esperado=|P∩C5|`. | Disparar [linha 193](chamados/management/commands/disparar_perguntas_competencia.py:193), vigiar [linha 210](chamados/management/commands/vigiar_fio_morto.py:210), carimbo [linha 231](chamados/management/commands/vigiar_fio_morto.py:231), placar [linha 98](core/management/commands/pipeline_placar.py:98). |
| `barrados_desligado` → `barrados['desligado']` | [Contador:49](chamados/management/commands/disparar_perguntas_competencia.py:49), soma na [linha 126](chamados/management/commands/disparar_perguntas_competencia.py:126). | Relatório RH já existente, [linha 200](chamados/management/commands/disparar_perguntas_competencia.py:200). |
| Causa `nunca_bateu_sem_adesao` | [disputa_emissao.py:759](chamados/services/disputa_emissao.py:759). | `retencao`, `dono`, `medir`, `do_dia` em [pergunta_no_app.py:99](chamados/services/pergunta_no_app.py:99). |

Os leitores acima distinguem lâmpada de pertença: o helper lê `veredito`; a pertença é consultada à C5; o placar compara os números publicados ([fábrica:69](chamados/services/fabrica_da_celula.py:69), [contador:34](chamados/management/commands/disparar_perguntas_competencia.py:34), [placar:105](core/management/commands/pipeline_placar.py:105)). Outros pressupostos em execução: [nao verificado].

O RED principal está em [test_e4a2_nunca_bateu.py:183](chamados/tests/test_e4a2_nunca_bateu.py:183):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4a2_nunca_bateu.NuncaBateuSoC5Test.test_RED_furo_emite_nunca_bateu_nao_emite_e_entrega_um_item_c5 --settings=config.settings.ci -v 2
```

Os oito mutantes estão [declarados na linha 33](chamados/tests/test_e4a2_nunca_bateu.py:33): fábrica sem corte, repasse sem corte, contador tratando nunca_bateu como alvo, esperado literal, esperado tautológico, universo ignorando C5, perda do balde RH e retorno do corte tardio. **O comando abaixo percorre cada um**, exigindo sua asserção específica no [método coletável:201](chamados/tests/test_e4a2_nunca_bateu.py:201):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4a2_nunca_bateu.NuncaBateuSoC5Test.test_mutantes_em_memoria_sao_rejeitados_pela_assercao_pretendida --settings=config.settings.ci -v 2
```

Comando da rodada solicitada, **não executado** [nao verificado]:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4a2_nunca_bateu chamados.tests.test_resposta_tardia chamados.tests.test_selo_emissor_fio_mudo chamados.tests.test_o261_fabrica_le_a_lampada core.tests.test_medida_turno_do_par core.tests.test_fio_mudo_placar --settings=config.settings.ci -v 2
```

Saída local: compilação em memória dos dez arquivos passou; os oito mutantes têm âncora única e sintaxe válida; o censo existente retornou `B1=[1199]` ([instrumento:448](core/medida_turno_do_par.py:448)). Isso não verifica a mordida Django [nao verificado].

Sem acesso a banco nesta execução. Django, freezegun e ruff estão ausentes no Python disponível; `python3 -m ruff check --no-cache ...` terminou com `No module named ruff`. Suite, RED executado, rejeição dos mutantes e ruff limpo permanecem [nao verificado]. [nao verificado]

## 10/11 02:04 · sessao 969c0e5df6ab · REVERTER-DIA-SEM-RECALCULO · verificador · origem=Code

**Não atesto cumprimento integral do aval:** (a) e (c) estão implementadas; (b) existe no caminho bem-sucedido, mas falhas posteriores podem deixar reversões gravadas sem hash final ou sem auditoria do ato. (`app/ponto/management/commands/aplicar_col900.py:164`; `app/ponto/portas/celula.py:779`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:225`.)

1. **Condições do aval e IDA**

   **(a) Sim, na cópia:** há **1 chamador produtivo** de `reverter_regeneracao_dia` com `apesar_da_lavra`: o ramo manual `aplicar_col900 --reverter`. O selo varre chamadas por AST e exige exatamente esse arquivo; não encontrou automático. Sua cobertura reconhece nome simples/qualificado e keyword explícita; aliases, argumento posicional ou `**kwargs` não são cobertos pelo contador. (`app/ponto/management/commands/aplicar_col900.py:164`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:30`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:52`.)

   **(b) Parcialmente garantida:** havendo alguma competência lavrada e aplicação com flag, prepara `/app/logs/reverter_col…json`, incluindo todas as competências pedidas, hashes anteriores, valores e DNA; depois acrescenta hashes finais e chama auditoria. Porém, falha no `replace` posterior deixa apenas a foto preparada, após duas chamadas à porta; falha na auditoria deixa JSON concluído sem o LogAuditoria do ato. Os próprios testes aceitam esses estados. (`app/ponto/management/commands/aplicar_col900.py:57`; `app/ponto/management/commands/aplicar_col900.py:74`; `app/ponto/management/commands/aplicar_col900.py:83`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:225`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:234`.)

   **(c) Sim:** sem flag, a guarda retorna `False` antes de salvar célula, emitir evento ou chamar o rabo financeiro. Há teste sem banco e teste Django que compara célula, DiaPago e hash antes/depois. (`app/ponto/portas/celula.py:779`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:321`; `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:114`.)

   **IDA:** o diff fornecido preserva seu ramo; na cópia, IDA e VOLTA chamam `_o_chao_do_dia_mudou`. Identidade integral com a base `65719b368`: **[nao verificado]**. (`app/ponto/management/commands/aplicar_col900.py:180`; `app/ponto/portas/celula.py:354`; `app/ponto/portas/celula.py:811`.)

2. **Escritores, leitores e significados**

   | Campo/fonte | Quem escreve ou produz; quem lê |
   |---|---|
   | JSON: identificação, dias, quem, quando, motivo, flag, competências e DNA | `_preparar_reversao` monta; `_gravar_reversao` serializa e escreve. O laço acrescenta resultados; `_concluir_reversao` acrescenta hashes finais, estado e data. (`app/ponto/management/commands/aplicar_col900.py:34`; `app/ponto/management/commands/aplicar_col900.py:62`; `app/ponto/management/commands/aplicar_col900.py:79`; `app/ponto/management/commands/aplicar_col900.py:159`; `app/ponto/management/commands/aplicar_col900.py:167`.) |
   | Leitura do JSON de reversão | A conclusão usa o dicionário em memória, sem reabrir o arquivo. Consumidor produtivo que leia esse novo JSON para restaurar o estado: **[nao verificado]**. (`app/ponto/management/commands/aplicar_col900.py:76`; `app/ponto/management/commands/aplicar_col900.py:83`.) |
   | LogAuditoria | Comando monta ação, objeto, descrição e payloads; `evento_kw → evento → LogAuditoria.objects.create` persiste. A tela genérica lê os registros. (`app/ponto/management/commands/aplicar_col900.py:90`; `app/core/observ.py:133`; `app/core/observ.py:142`; `app/core/views.py:705`.) |
   | `hash_do_gravado` | Lê FechamentoMensal; o comando grava seu retorno como `hash_antes`/`hash_depois` e os copia à auditoria. O escritor financeiro continua sendo `recalcular_fechamento_mes`. (`app/ponto/portas/regen_exportada.py:62`; `app/ponto/management/commands/aplicar_col900.py:70`; `app/ponto/management/commands/aplicar_col900.py:85`; `app/ponto/services/fechamento.py:1160`.) |
   | `valores_do_gravado` | Lê somente `CAMPOS` de `aplicar_09_corte_b`, retornando dict ou `None`; o comando grava em `valores_antes`. Não é cópia integral de FechamentoMensal nem de DiaPago. (`app/ponto/portas/regen_exportada.py:77`; `app/ponto/management/commands/aplicar_09_corte_b.py:67`; `app/ponto/management/commands/aplicar_col900.py:71`.) |
   | `competencia_fechada_para_celula` | Juiz K9 existente: exportação vigente **ou** holerite publicado da pessoa. Alimenta `competencias[].lavrada` e a guarda da porta, via `_competencias_exportadas`. (`app/core/juizes.py:337`; `app/folha/export.py:610`; `app/ponto/management/commands/aplicar_col900.py:58`; `app/ponto/management/commands/gerar_celulas.py:173`.) |
   | `competencia_do_fato` | Delega à janela da empresa. Fotografia e recálculo usam a data pedida; para esta chamada, é o dia da célula, sem nova classificação de turno. (`app/ponto/management/commands/aplicar_col900.py:57`; `app/ponto/services/fechamento.py:1594`; `app/ponto/services/fechamento.py:1602`; `app/ponto/services/fechamento.py:1809`.) |

   **“Lavrada” não significa “trancada”:** o JSON usa K9; trancada consulta `PeriodoFechado` sem reabertura. Não encontrei leitor específico do novo JSON que imponha a segunda interpretação **[nao verificado]**. (`app/folha/export.py:589`; `app/ponto/services/fechamento.py:1464`; `app/ponto/services/fechamento.py:1473`.)

   **Ator pode divergir:** JSON força fallback `sistema:aplicar_col900`; a coluna `LogAuditoria.ator` deriva separadamente de `sys.argv`. Em `call_command`/shell pode ficar nula ou identificar o comando hospedeiro. `usuario_so` identifica usuário do SO, não usuário Django. (`app/ponto/management/commands/aplicar_col900.py:64`; `app/core/observ.py:22`; `app/core/observ.py:117`.)

   **Há leitor histórico com outro formato:** `janelas_de_regeneracao` espera `desde/ate` na ação `regenerar_celulas_vinculo`; a VOLTA grava `dna/revertida`, sendo ignorada por esse leitor. A proteção atual também lê `regenerada_em`, que a VOLTA atualiza. (`app/escala/services/regeneracao.py:58`; `app/ponto/portas/celula.py:792`; `app/ponto/portas/celula.py:799`; `app/escala/services/regeneracao.py:87`.)

3. **Ordem, interrupção e hash posterior**

   O JSON anterior passa por escrita, `flush` e `fsync` antes da porta. Não há `fsync` do diretório nessa implementação; durabilidade absoluta perante queda de energia fica **[nao verificado]**. (`app/ponto/management/commands/aplicar_col900.py:40`; `app/ponto/management/commands/aplicar_col900.py:75`; `app/ponto/management/commands/aplicar_col900.py:164`.)

   **O lote não é atômico.** Em autocommit, cada dia encerra seu `atomic` antes do rabo; se um dia posterior levantar, os anteriores permanecem commitados. Até o dia que levantou pode já estar gravado, caso a saída ocorra depois desse `atomic`; seu resultado só é acrescentado após a porta retornar. (`app/ponto/management/commands/aplicar_col900.py:149`; `app/ponto/management/commands/aplicar_col900.py:164`; `app/ponto/portas/celula.py:785`; `app/ponto/portas/celula.py:811`.)

   **`SystemExit` do `_preparar`: não executa o `finally`**, pois a preparação acontece antes do `try`. Uma saída dentro do laço executa o `finally`, mas sua conclusão também pode falhar. (`app/ponto/management/commands/aplicar_col900.py:146`; `app/ponto/management/commands/aplicar_col900.py:149`; `app/ponto/management/commands/aplicar_col900.py:173`.)

   **Hash posterior:** sem transação externa, o `on_commit` executa imediatamente, antes de a porta retornar; nesse cenário o hash vem depois da tentativa de recálculo. Sob `atomic` externo, o callback pode continuar pendente quando o comando lê `hash_depois`. O comando não impõe uma dessas condições. (`app/ponto/portas/celula.py:411`; `app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1824`; `app/ponto/management/commands/aplicar_col900.py:85`.)

   **“Concluído” não certifica sucesso financeiro:** marca término do laço; exceções financeiras são engolidas. Também pode haver FechamentoMensal atualizado seguido de falha na lavratura de DiaPago. (`app/ponto/management/commands/aplicar_col900.py:172`; `app/ponto/services/fechamento.py:1818`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1238`.)

4. **Testes: coleta, mordida e alcance** [nao verificado]

   **São coletáveis:** classes `unittest.TestCase` ficam fora do `__main__`; `ponto` integra as labels, e o runner usa descoberta com poda de quarentena. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:51`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:292`; `bin/regua.sh:23`; `bin/suite.sh:102`; `app/core/runner_quarentena.py:72`.)

   **O isolamento é restaurável, mas substitui a integração:** `patch.dict(sys.modules)` fica em contexto; ORM, juiz, hash, auditoria, arquivos, `fsync` e `replace` são duplos. Compila-se o comando e chama-se `handle` diretamente. Na porta extraída por AST, `atomic=nullcontext` e o dinheiro é um dicionário alterado pelo mock. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:100`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:133`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:173`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:294`.)

   **Suite versus CLI:** `test_RED…` roda contra a fonte atual na suite; comparação com `--base` e campanha dos dez `--mutante` ficam exclusivamente no `__main__`. Dois mutantes locais — segundo chamador e guarda removida — rodam na suite; o contrato AST separado também contém mutantes locais. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:179`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:344`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:56`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:337`; `app/ponto/tests/test_reverter_regen_sem_banco.py:86`.)

   Nos ensaios desta análise, passaram 28 testes sem banco e morreram os dez mutantes CLI; não foi criado relatório persistido **[nao verificado por arquivo de resultado]**. RED contra a base, suite Django completa e opus próprios: **[nao verificado]**. [nao verificado]

   Um teste real com `call_command`, banco/arquivos isolados e commits efetivamente executados poderia verificar parser, imports, juiz, hash, JSON e LogAuditoria reais na composição inteira. Os atuais substituem esses componentes; os Django existentes chamam a porta e forçam `on_commit` imediato. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:116`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:140`; `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:114`; `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:127`.)

5. **Classes de reprovação**

   | Classe | Constatação |
   |---|---|
   | Segundo juiz | Não identificado no novo caminho: pergunta ao K9 e compartilha o rabo existente. (`app/ponto/management/commands/aplicar_col900.py:58`; `app/ponto/portas/celula.py:811`.) |
   | Segundo escritor | Não há segundo escritor técnico de LogAuditoria: usa `evento`. O JSON tem escritor próprio no comando; não escreve célula ou dinheiro. A exigência da trilha completa, porém, permanece no chamador. (`app/core/observ.py:133`; `app/ponto/management/commands/aplicar_col900.py:34`; `app/ponto/management/commands/aplicar_col900.py:90`.) |
   | Relógio fora da borda | Novos horários estão no comando. O rabo já lê `localdate` internamente; violação nova dessa classe: **[nao verificado]**. (`app/ponto/management/commands/aplicar_col900.py:66`; `app/ponto/management/commands/aplicar_col900.py:87`; `app/ponto/portas/celula.py:426`.) |
   | Derivação paralela | Hash, valores e competência delegam às funções existentes. (`app/ponto/management/commands/aplicar_col900.py:53`; `app/ponto/management/commands/aplicar_col900.py:57`; `app/ponto/management/commands/aplicar_col900.py:70`.) |
   | Selo sem mordida | Há mordida da guarda; cobertura integrada do comando continua ausente **[nao verificado]**. (`app/ponto/tests/test_reverter_exportada_sem_banco.py:321`; `app/ponto/tests/test_reverter_exportada_sem_banco.py:337`.) |
   | Ordem de trava | VOLTA lê célula/guarda antes do `atomic`, sem `select_for_update`; falta serialização visível. Inversão concreta de locks: **[nao verificado]**. (`app/ponto/portas/celula.py:776`; `app/ponto/portas/celula.py:785`.) |
   | Pouso antes do pré-requisito | JSON anterior precede o ato. Suite/opus anteriores ao pouso: **[nao verificado]**. (`app/ponto/management/commands/aplicar_col900.py:146`; `app/ponto/management/commands/aplicar_col900.py:164`.) |
   | Campo com dois significados | Ressalvas concretas: ator do JSON versus coluna `ator`; `concluido` significa laço terminado, não recálculo certificado. (`app/ponto/management/commands/aplicar_col900.py:64`; `app/core/observ.py:117`; `app/ponto/management/commands/aplicar_col900.py:172`.) |

6. **09/2026 com flag**

   **Sim, continua barrada financeiramente nas empresas exportadas.** A flag abre a guarda da célula; o escritor financeiro não recebe essa flag e exclui os colaboradores dessas empresas pela `GUARDA_COMPETENCIA`. Isso não afirma bloqueio de toda 09 aberta. (`app/ponto/portas/celula.py:779`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:448`; `app/ponto/services/fechamento.py:539`; `app/ponto/services/fechamento.py:560`.)

   **`test_09` está armado para provar esse caso:** monta DiaPago de 480 antes de exportar, exporta 09, reverte com flag e exige DNA restaurado, DiaPago integral preservado, hash de Fechamento igual e contador da guarda aumentado. Sua execução Django ficou **[nao verificado]**. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:61`; `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:133`.)

   Ele não prova identidade integral do FechamentoMensal: compara o hash, que exclui identificadores, determinados carimbos e aprovação; também não prova a ordem natural de `on_commit`, substituído no teste. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:107`; `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:142`; `app/ponto/portas/regen_exportada.py:37`.)

## 10/11 02:02 · sessao 5c12c35e05e3 · - · - · origem=Ronald (tmux)

4. **Coletáveis, com limites claros.** Classes `unittest.TestCase` e métodos `test_*` estão fora do `__main__` (`app/ponto/tests/test_reverter_exportada_sem_banco.py:51,92,292`); `ponto` está nas labels (`bin/regua.sh:23`), a suite chama `manage.py test` (`bin/suite.sh:102-103`) e usa `DiscoverRunner` com poda de quarentena (`app/core/runner_quarentena.py:72-91`). A quarentena atual não inclui esses testes (`app/core/testes_em_quarentena.txt:6-8`).

- `sys.modules` é substituído com contexto restaurável; arquivos, `fsync`, `replace`, ORM, juízes, hash, ator e auditoria são duplos; o comando é compilado e recebe `handle()` diretamente (`app/ponto/tests/test_reverter_exportada_sem_banco.py:100-140,173-177`). A porta real é extraída por AST, mas `atomic=nullcontext`, persistência e rabo financeiro são substituídos (`app/ponto/tests/test_reverter_exportada_sem_banco.py:294-318`).
- O teste chamado `test_RED...` roda normalmente na suite, contra a fonte atual (`app/ponto/tests/test_reverter_exportada_sem_banco.py:179-211`). **A campanha `--base/--mutante` só roda como CLI**, sob `__main__`; não é disparada pela coleta (`app/ponto/tests/test_reverter_exportada_sem_banco.py:344-381`).
- Dois ensaios locais de mutantes rodam dentro da suite: segundo chamador sintético e guarda removida (`app/ponto/tests/test_reverter_exportada_sem_banco.py:56-59,337-341`). O segundo comprova que a remoção deixa passar e alterar; a campanha CLI é que reaplica o teste negativo ao fonte mutado (`app/ponto/tests/test_reverter_exportada_sem_banco.py:321-341,359-360`).
- Um Django `call_command` real, verificando banco e JSON isolado, cobriria descoberta/parser do comando, imports/modelos verdadeiros, `competencia_fechada_para_celula`, serialização, gravação efetiva de `LogAuditoria`, ator de processo e hashes reais na composição comando→porta→dinheiro. O teste atual substitui precisamente essas dependências e ignora `BaseCommand` real (`app/ponto/tests/test_reverter_exportada_sem_banco.py:116-140,173-177`). Os Django existentes testam a **porta**, com `on_commit` substituído por execução imediata; não testam `call_command`, JSON ou fechamento natural da transação (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:20-22,114-148`).

**Execução observada, sem arquivo de resultado:** 28/28 testes dos dois módulos sem banco passaram. Os dez mutantes CLI falharam como esperado: `sem_pre` 9 failures; `ignora_io` 4; `sem_pos` 5 + 1 error; `hash_falso` 1; `sem_valores` 4; `sem_dna` 4; `sem_trilha` 3; `ignora_juiz` 9; `foto_em_aberta` 1; `guarda` 1. `--base 65719b368` não comprovou RED: `git show` retornou 128; fonte da base indisponível [nao verificado]. Campanha implementada em `app/ponto/tests/test_reverter_exportada_sem_banco.py:344-381`.

6. **09/2026 exportada continua barrada para dinheiro com flag.** A competência permanece na guarda (`app/ponto/services/fechamento.py:36-43`); o escritor financeiro não recebe `apesar_da_lavra` (`app/ponto/services/fechamento.py:448-451,1798-1810`) e exclui colaboradores de empresas exportadas antes do cálculo/escrita (`app/ponto/services/fechamento.py:539-564`).

`test_09` está armado para esse caso: primeiro monta dinheiro real de 480 antes da exportação (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:61-80`); exporta 09, aplica flag, exige DNA restaurado, DiaPago integral igual, hash de Fechamento igual e contador da guarda aumentado (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:133-148`). **Sua execução Django não foi realizada [nao verificado].**

O teste não prova bloqueio de 09 aberta, pois a guarda exige exportação (`app/ponto/services/fechamento.py:560-564`), nem identidade integral do FechamentoMensal: compara seu hash, que omite identificadores/carimbos/aprovação (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:107-112,147`; `app/ponto/portas/regen_exportada.py:37-38,56-74`).

## 10/11 02:02 · sessao 5639b493fe56 · - · - · origem=Ronald (tmux)

- **Chamadores:** varredura AST produtiva encontrou **1 chamada total** de `reverter_regeneracao_dia`, com `apesar_da_lavra`, no comando manual; nenhum automático visível — `app/ponto/management/commands/aplicar_col900.py:164`.
- **Selo:** sem flag, competência lavrada devolve `False` antes de escrita, evento e rabo — `app/ponto/portas/celula.py:776`, `:779`, `:785`, `:811`.
- **IDA:** permanece no mesmo rabo compartilhado — `app/ponto/portas/celula.py:354`; o diff fornecido não altera sua implementação. Comparação integral com base `65719b368` [nao verificado], pois a cópia não fornece Git.
- **Lote parcial:** comando não envolve o lote em transação; cada dia abre atomic próprio, encerrado antes do rabo. Em autocommit, dias anteriores ficam gravados se um posterior falhar — `app/ponto/management/commands/aplicar_col900.py:149`; `app/ponto/portas/celula.py:785`, `:809`.
- **Hash depois:** no CLI em autocommit, `on_commit` executa o callback antes de retornar, e o hash final vem depois. Sob atomic externo, o callback pode permanecer pendente quando `_concluir_reversao` lê o hash — `app/ponto/services/fechamento.py:1807`, `:1824`; `app/ponto/management/commands/aplicar_col900.py:83`, `:175`. A configuração efetiva de produção [nao verificado].
- **“Concluído” não garante dinheiro recalculado:** exceções do recálculo são engolidas; inclusive a lavratura de DiaPago pode falhar depois de FechamentoMensal mudar — `app/ponto/services/fechamento.py:1818`, `:1160`, `:1200`, `:1238`; o comando conclui pelo término do loop — `app/ponto/management/commands/aplicar_col900.py:172`.
- **Competência:** rabo e fotografia usam a mesma autoridade `janela_atual`; para `date` do dia da célula, não encontrei divergência concreta. O rabo deduplica pelo fim da janela; o adaptador `competencia_do_fato` passa por `competencias_do_intervalo`, que usa o mesmo fim — `app/ponto/portas/celula.py:447`; `app/ponto/services/fechamento.py:1594`, `:1602`; `app/ponto/services/dia_pago.py:716`. Não há remapeamento para dia de turno nessa chamada; datetime é normalizado para data local — `app/ponto/services/fechamento.py:1568`.
- **Concorrência:** VOLTA lê célula e guarda antes do atomic, sem `select_for_update`; edição humana trava dentro do atomic. Ausência de serialização visível, mas inversão nova de locks [nao verificado] — `app/ponto/portas/celula.py:776`, `:785`, `:592`.
- **09/2026:** flag abre a célula, mas não atravessa a guarda financeira de empresa exportada. `recalcular_fechamento_mes` não recebe essa flag; exclui do universo os colaboradores das empresas exportadas na competência guardada — `app/ponto/services/fechamento.py:36`, `:448`, `:539`, `:560`. Não generalizar para 09 aberta: a condição exige exportação — `app/ponto/services/fechamento.py:561`.
- **Juiz da lavra:** `_competencias_exportadas` enumera candidatos e consulta `competencia_fechada_para_celula`, incluindo TXT e holerite publicado, sem inventar regra nova — `app/ponto/management/commands/gerar_celulas.py:146`, `:161`, `:169`, `:173`.
- **Relógio:** VOLTA lê `timezone.now()` dentro da porta; o rabo lê `localdate()` para decidir re-julgamento. Ambos são preexistentes ao diff v3 fornecido — `app/ponto/portas/celula.py:792`, `:426`.

## 10/11 02:02 · sessao 7fb48bb09e80 · - · - · origem=Ronald (tmux)

- **Juiz existente, sem clonagem:** `_preparar_reversao` pergunta a `competencia_fechada_para_celula` e usa `competencia_do_fato`; ambos são delegações, não predicados financeiros novos (`app/ponto/management/commands/aplicar_col900.py:53`, `:57`, `:58`; `app/core/juizes.py:337`; `app/ponto/services/fechamento.py:1594`, `:1602`).

- **Significados distintos:** `competencias[].lavrada` do JSON recebe K9, isto é, exportação vigente **ou** holerite publicado da pessoa (`app/ponto/management/commands/aplicar_col900.py:58`, `:69`; `app/folha/export.py:610`, `:614`). Não significa período trancado: essa outra pergunta usa `PeriodoFechado.reaberto_em IS NULL` (`app/ponto/services/fechamento.py:1464`, `:1473`). O código novo não confunde os predicados, embora o nome amplo “lavrada” exija essa leitura (`app/ponto/management/commands/aplicar_col900.py:69`).

- **Competência:** os dias pedidos alimentam `competencia_do_fato`; a cadeia usa a janela da empresa, não mês civil, relógio atual ou nova derivação de turno (`app/ponto/management/commands/aplicar_col900.py:57`; `app/ponto/services/fechamento.py:1594`, `:1604`; `app/ponto/services/dia_pago.py:702`, `:716`). A guarda da célula enumera competências e pergunta ao mesmo K9; sua janela delega ao mesmo motor de janelas (`app/ponto/management/commands/gerar_celulas.py:173`, `:176`; `app/folha/export.py:47`; `app/ponto/janelas.py:54`, `:60`).

- **Hash:** `hash_do_gravado` lê `FechamentoMensal`, usa MD5 truncado em 12 caracteres e exclui IDs, competência, carimbos e aprovador; portanto não prova identidade integral da linha (`app/ponto/portas/regen_exportada.py:35`, `:37`, `:62`, `:70`, `:74`). O comando grava o resultado como `hash_antes`/`hash_depois`, sem calcular outro hash (`app/ponto/management/commands/aplicar_col900.py:70`, `:85`).

- **Valores:** `valores_do_gravado` retorna os campos de `aplicar_09_corte_b.CAMPOS` ou `None`; não fotografa integralmente `FechamentoMensal` nem `DiaPago` (`app/ponto/portas/regen_exportada.py:77`; `app/ponto/management/commands/aplicar_09_corte_b.py:67`). O JSON os guarda em `valores_antes` (`app/ponto/management/commands/aplicar_col900.py:71`). Leitores anteriores distinguem explicitamente `None` de pagamento zero (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`; `app/chamados/services/passivo_validado.py:174`).

- **JSON:** `_preparar_reversao` monta identificação, datas, ator, motivo, flag, competências, hashes, valores e DNA; `_gravar_reversao` é seu escritor de arquivo; o laço acrescenta resultados e `_concluir_reversao` acrescenta hashes finais/estado/data (`app/ponto/management/commands/aplicar_col900.py:34`, `:62`, `:79`, `:159`, `:167`). Consumidor produtivo que carregue esse novo JSON para efetuar restauração: **[nao verificado]**. O caminho é levado à descrição e ao payload de auditoria (`app/ponto/management/commands/aplicar_col900.py:92`, `:98`).

- **LogAuditoria:** o comando chama `evento_kw`, adaptador de `evento`; este faz o único `LogAuditoria.objects.create` dessa cadeia, dentro de savepoint (`app/ponto/management/commands/aplicar_col900.py:90`; `app/core/observ.py:142`, `:146`, `:133`). Há evento por célula na porta e evento por ato no comando, com informações diferentes, ambos pelo mesmo escritor técnico (`app/ponto/portas/celula.py:799`; `app/ponto/management/commands/aplicar_col900.py:90`). A tela genérica lê os registros e seus filtros (`app/core/views.py:705`).

- **Ator merece ressalva:** JSON força fallback `sistema:aplicar_col900`; a coluna `LogAuditoria.ator` deriva separadamente de `sys.argv` e só reconhece processos `manage.py` (`app/ponto/management/commands/aplicar_col900.py:64`; `app/core/observ.py:22`, `:29`, `:117`). Em `call_command`, shell ou comando hospedeiro, JSON e coluna `ator` podem divergir; `usuario_so` é usuário do SO, não usuário Django (`app/ponto/management/commands/aplicar_col900.py:65`; `app/core/observ.py:118`, `:123`).

- **Leitor histórico de eventos da porta:** `janelas_de_regeneracao` procura `desde`/`ate`; o evento de VOLTA usa `dna`/`revertida`, portanto aquele leitor o ignora por `KeyError` (`app/escala/services/regeneracao.py:58`; `app/ponto/portas/celula.py:804`). A reescrita atual também deixa `regenerada_em`, consumido pelo alimentador/juiz de reescrita; não depende apenas daquela janela histórica (`app/ponto/portas/celula.py:792`; `app/escala/services/regeneracao.py:72`, `:87`).

## 10/11 01:57 · sessao aabf45dcacb9 · REVERTER-DIA-SEM-RECALCULO · construtor · origem=Code

Implementada a trilha L-092 no comando manual: foto antes da porta, hashes posteriores no mesmo caminho de JSON e `LogAuditoria` pelo `evento_kw`, o mesmo idioma da porta (`ponto/management/commands/aplicar_col900.py:34`, `:50`, `:79`; `ponto/portas/celula.py:798`).

Arquivos alterados:

- [aplicar_col900.py](/home/ronald/wt-codex-reverter-dia-sem-recalculo-014633/app/ponto/management/commands/aplicar_col900.py:34): trilha por ato; falha na gravação anterior interrompe antes da porta.
- [test_reverter_exportada_sem_banco.py](/home/ronald/wt-codex-reverter-dia-sem-recalculo-014633/app/ponto/tests/test_reverter_exportada_sem_banco.py:1): RED, contador AST, guardas e mutantes executáveis em memória.
- [test_reverter_regen_recalcula_dinheiro.py](/home/ronald/wt-codex-reverter-dia-sem-recalculo-014633/app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:114): integração para exportada sem/com flag e guarda da 09.
- [reavaliar_ausencias_feriado.py](/home/ronald/wt-codex-reverter-dia-sem-recalculo-014633/app/ponto/management/commands/reavaliar_ausencias_feriado.py:20) e [signals.py](/home/ronald/wt-codex-reverter-dia-sem-recalculo-014633/app/chamados/signals.py:320): corrigidas as duas descrições antigas.

O censo dos campos lidos ou gravados ficou assim:

| Campos | Quem escreve/fornece | Quem lê nesta alteração |
|---|---|---|
| `colab`, `dias`, `motivo`, `aplicar`, `apesar_da_lavra` | Argumentos manuais, `aplicar_col900.py:108` | `aplicar_col900.py:125`, `:143`, `:164` |
| `Colaborador.pk`, `empresa_id`, `empresa` | Porta de cadastro, `colaboradores/services/colaborador.py:34`, `:57`, `:91` | Consulta atual em `aplicar_col900.py:145`; competência/juiz em `:57` |
| `CelulaDia.data`, `dna`, `dna_anterior`; `dna.marcos.hi` | Porta em `ponto/portas/celula.py:259`, `:272`, `:786`; produtor do DNA em `gerar_celulas.py:73` | Foto em `aplicar_col900.py:72`; exibição em `:152` |
| `mes`, `ano`, `lavrada` | Autoridades existentes: `competencia_do_fato`, `ponto/services/fechamento.py:1602`; juiz registrado, `folha/export.py:589` | `aplicar_col900.py:57` |
| `hash_antes`, `hash_depois` | Função reutilizada `regen_exportada.py:62` | JSON/trilha em `aplicar_col900.py:70`, `:85`, `:94` |
| `valores_antes` | Função reutilizada `regen_exportada.py:77`; universo de campos em `aplicar_09_corte_b.py:69` | Foto em `aplicar_col900.py:71` |
| Campos financeiros do fechamento | Escritor existente, `ponto/services/fechamento.py:898`, `:1163`; status/observação em `:1323` | Helpers anteriores, `regen_exportada.py:56`, `:77` |
| `quem.processo`, `quem.usuario_so`, `quando`, `concluido_em`, `estado`, `resultados`, caminho | Comando em `aplicar_col900.py:62`, `:74`, `:86`, `:159`, `:167`; processo em `core/observ.py:22` | Conclusão em `aplicar_col900.py:79`; leitura do JSON pelo selo em `test_reverter_exportada_sem_banco.py:154` |
| Auditoria: ação, modelo, objeto, descrição, antes/depois | Adaptador em `aplicar_col900.py:90`; escritor único em `core/observ.py:134` | Consulta geral em `core/views.py:705`; selo em `test_reverter_exportada_sem_banco.py:205` |

`quem` registra processo e usuário do sistema operacional; identidade humana autenticada além disso permanece **[nao verificado]** (`aplicar_col900.py:64`).

Os contraexemplos estão contemplados:

- **Exportada sem flag:** retorno `False`, sem escrita nem rabo; selo local em `test_reverter_exportada_sem_banco.py:321`. Comparação real de célula, DiaPago e hash preparada em `test_reverter_regen_recalcula_dinheiro.py:114`, **[nao verificado]** por integração.
- **Duas competências:** um JSON contém ambas, inclusive a aberta, antes da primeira porta (`test_reverter_exportada_sem_banco.py:179`).
- **Apenas abertas com flag:** sem JSON (`test_reverter_exportada_sem_banco.py:240`).
- **Sem efeito em exportada com flag:** mantém foto, resultados falsos e hashes iguais (`test_reverter_exportada_sem_banco.py:258`).
- **09/2026 com flag:** a célula pode voltar; o dinheiro continua sujeito à guarda existente (`ponto/portas/celula.py:780`, `ponto/services/fechamento.py:36`, `:560`). Integração específica preparada em `test_reverter_regen_recalcula_dinheiro.py:133`, **[nao verificado]**.

Execução local: **42 testes sem banco passaram**; o contador AST encontrou exatamente um chamador manual com flag (`test_reverter_exportada_sem_banco.py:52`; suites abaixo). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_reverter_exportada_sem_banco \
  ponto.tests.test_reverter_regen_sem_banco \
  chamados.tests.test_e4_expand -q
# Ran 42 tests — OK
```

O RED na base falhou por `falta JSON unico por ato`; o mesmo selo passou depois (`test_reverter_exportada_sem_banco.py:154`, `:179`, `:355`). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.test_reverter_exportada_sem_banco \
  --base 65719b36843f2267aaec224aec89df01626979fc \
  TrilhaComandoTest.test_RED_json_antes_da_porta_duas_competencias_e_trilha_depois -q
# rc 1 — FAILED (failures=1)
```

Os dez mutantes retornaram **rc 1**; alterações ficam somente em memória (`test_reverter_exportada_sem_banco.py:344`). Comando de cada um:

```bash
mutar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.test_reverter_exportada_sem_banco \
    --mutante "$1" "${2:-TrilhaComandoTest}" -q
}
mutar sem_pre
mutar ignora_io
mutar sem_pos
mutar hash_falso
mutar sem_valores
mutar sem_dna
mutar sem_trilha
mutar ignora_juiz
mutar foto_em_aberta
mutar guarda GuardaPortaSemBancoTest.test_sem_flag_barrada_e_nada_muda
```

O segundo chamador sintético acusa dois, pelo mesmo contador usado no pouso (`test_reverter_exportada_sem_banco.py:56`): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  ponto.tests.test_reverter_exportada_sem_banco.ChamadorUnicoTest -v
```

A integração não executou: os dois módulos pararam no import de Django (`test_reverter_regen_recalcula_dinheiro.py:9`, `test_porta_celula_dia.py:24`). Ruff também não está instalado; **integração e Ruff limpo [nao verificado]**. Comandos pendentes para o ambiente da suite: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_reverter_regen_recalcula_dinheiro \
  ponto.tests.test_porta_celula_dia \
  ponto.tests.test_reverter_exportada_sem_banco \
  ponto.tests.test_reverter_regen_sem_banco \
  chamados.tests.test_e4_expand --settings=config.settings_test

PYTHONDONTWRITEBYTECODE=1 python3 -m ruff check .
```

Entrega de construtor; certificação por Code/suite e opus final permanece **[nao verificado]**. [nao verificado]

## 10/11 01:57 · sessao bc47ec2ad95c · FIO-E0-V4-E4A-2 · verificador · origem=Code

**Parecer: a volta 5 está corrigida; eu reprovaria a volta 6 nesta forma.** O placar exige o carimbo do dia e lê o esperado publicado pelo contador. Permanecem um corte indevido no recebimento de resposta, uma comparação que alarma exclusão legítima da C5 e problemas nos selos novos. (`app/core/management/commands/pipeline_placar.py:94`, `app/chamados/management/commands/vigiar_fio_morto.py:231`, `app/chamados/services/resposta_tardia.py:46`, `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:213`)

A classificação abaixo é estática: reprodução das dez failures, execução da suite, estado atual de produção e confirmação independente do hash: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado**

| Campo/valor | Quem escreve ou produz | Quem lê |
|---|---|---|
| `CelulaDia.veredito`, especialmente `nunca_bateu` | O cartório calcula adesão pela primeira Batida não retratada e entrega `nunca_bateu` à lavra quando não há cobertura integral. A atribuição final pertence à porta `lavrar_veredito`. (`app/ponto/services/cartorio.py:1165`, `app/ponto/services/cartorio.py:1252`, `app/ponto/portas/celula.py:1044`) | O helper novo lê a palavra; fábrica, repasse, contador e resposta tardia usam esse helper. A C5 consulta a mesma coluna, acrescentando seu universo próprio. (`app/chamados/services/fabrica_da_celula.py:67`, `app/chamados/services/fabrica_da_celula.py:85`, `app/chamados/services/disputa_emissao.py:757`, `app/chamados/management/commands/disparar_perguntas_competencia.py:79`, `app/chamados/services/resposta_tardia.py:46`, `app/escala/services/cadastro_realidade.py:180`) |
| `status_local` | O núcleo de escrita é `_gravar_estado`; a resposta tardia tentava reabrir pela porta `_renascer_por_premissa_viva`, que continua responsável pelas guardas. (`app/chamados/models.py:201`, `app/chamados/models.py:228`, `app/chamados/models.py:983`) | O contador seleciona `VIVOS`; o repasse exclui `ENCERRADOS`; resposta tardia deixa chamados vivos seguirem antes do corte novo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`, `app/chamados/services/disputa_emissao.py:755`, `app/chamados/services/resposta_tardia.py:35`) |
| `fio_mudo_alvos.numero/esperado` | O contador produz `len(_mudos)` e esperado `0`. Disparar imprime; vigiar publica ambos sem recalcular. (`app/chamados/management/commands/disparar_perguntas_competencia.py:117`, `app/chamados/management/commands/disparar_perguntas_competencia.py:186`, `app/chamados/management/commands/vigiar_fio_morto.py:231`, `app/chamados/management/commands/vigiar_fio_morto.py:239`) | O placar lê `censo[nome]` e `esperado[nome]`, exige inteiros e compara igualdade. (`app/core/management/commands/pipeline_placar.py:98`) |
| `nunca_bateu_sem_adesao`: `nome`, `numero`, `esperado`, `dono`, `fora_da_c5`, `universo` | `contador_nunca_bateu` monta o bloco inteiro; `dono='CADASTRO'`, número por pares e esperado pela interseção com a C5. (`app/chamados/management/commands/disparar_perguntas_competencia.py:31`, `app/chamados/management/commands/disparar_perguntas_competencia.py:40`) | Disparar e vigiar imprimem número, esperado, dono e universo; vigiar grava o bloco completo e suas duas projeções numéricas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:183`, `app/chamados/management/commands/vigiar_fio_morto.py:210`, `app/chamados/management/commands/vigiar_fio_morto.py:232`, `app/chamados/management/commands/vigiar_fio_morto.py:240`) |
| Carimbo: `hoje`, `medido_em`, novas entradas em `censo`/`esperado` | Vigiar captura um instante, deriva o dia local e grava somente em frota; `_carimbar` publica por arquivo temporário e `replace`. (`app/chamados/management/commands/vigiar_fio_morto.py:166`, `app/chamados/management/commands/vigiar_fio_morto.py:225`, `app/chamados/management/commands/vigiar_fio_morto.py:134`) | O placar exige `hoje` igual ao dia solicitado; `bin/fio_estado.py` compara genericamente as entradas de `esperado` com `censo` e calcula idade por `medido_em`. (`app/core/management/commands/pipeline_placar.py:94`, `bin/fio_estado.py:121`, `bin/fio_estado.py:129`) |
| Causa `nunca_bateu_na_lista_c5` | É retorno classificatório de `motivo_fora_da_fabrica`; descrição e dono ficam em `CAUSAS`. (`app/chamados/services/disputa_emissao.py:759`, `app/chamados/services/pergunta_no_app.py:35`) | `retencao` consome a causa; `dono`, `medir` e `do_dia` a transformam em classificação, contadores e apresentação. (`app/chamados/services/pergunta_no_app.py:99`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:167`, `app/chamados/services/pergunta_no_app.py:189`) |

**Há ESMERIL-SEMÂNTICA concreto:** `dia_so_na_c5` prova somente “a coluna contém `nunca_bateu`”; a causa afirma “na lista C5”. A pertença real depende também da janela e, para desligados, da lavra da folha. Portanto a causa pode anunciar uma entrega inexistente. (`app/chamados/services/fabrica_da_celula.py:69`, `app/chamados/services/disputa_emissao.py:759`, `app/escala/services/cadastro_realidade.py:180`, `app/escala/services/cadastro_realidade.py:197`)

Também não são equivalentes “nunca usou o app” e “sem adesão”: o produtor mede primeira **Batida não retratada**, enquanto o texto da C5 fala em uso do aplicativo. Login ou resposta não são o insumo dessa adesão. (`app/ponto/services/cartorio.py:1165`, `app/escala/services/cadastro_realidade.py:72`)

**2. Resposta existente e resposta tardia**

É necessário separar duas portas:

- **Responder uma `PerguntaDisputa` existente:** se estiver viva pelo juiz, a API chama o core; ele grava `resposta_colab`, `respondida_em` e a decisão de resposta. Esse fluxo não passa pelo corte de `resposta_tardia`. Pergunta morta recebe recusa explícita, 404. (`app/api/views.py:2634`, `app/api/views.py:2649`, `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1503`)
- **Responder no fio de chamado encerrado:** o corte novo impede até consultar a porta de renascimento quando a célula é `nunca_bateu`. O conteúdo vira nota e auditoria; somente texto livre em `conteudo` abre Pauta DP. Hora/motivo/escape ficam registrados na nota, mas não seguem a declaração normal. (`app/chamados/services/resposta_tardia.py:46`, `app/chamados/services/resposta_tardia.py:53`, `app/chamados/services/resposta_tardia.py:60`, `app/chamados/views.py:1089`)

Logo, **a resposta não desaparece, nem é silêncio HTTP**: a view devolve 200, `estado='resposta_tardia'` e mensagem. Entretanto, nesse segundo fluxo, perde-se o processamento normal da declaração recebida. (`app/chamados/views.py:1053`, `app/chamados/services/resposta_tardia.py:15`)

**Meu voto para `test_resposta_tardia.py:74` é CÓDIGO.** A lei declarada manda tentar renascer pela porta existente quando a célula ainda acusa, respeitando as guardas dela. `nunca_bateu` permanece em `VEREDITOS_ACUSAM`; o corte automático de fabricação não demonstra uma exceção para receber resposta efetiva. Não inverteria a asserção para legitimar essa mudança. (`app/chamados/services/resposta_tardia.py:6`, `app/escala/models.py:1767`, `app/chamados/models.py:983`, `app/chamados/tests/test_resposta_tardia.py:74`)

A fixture também precisa ficar coerente: pede `furo`, mas não cria primeira Batida; criar o chamado dispara julgamento, capaz de relavrar `nunca_bateu`. Corrigir apenas a fixture esconderia a regressão: mantenha o caso de furo e acrescente explicitamente o recebimento de resposta em célula `nunca_bateu`. (`app/chamados/tests/test_resposta_tardia.py:25`, `app/chamados/tests/test_resposta_tardia.py:33`, `app/chamados/signals.py:208`, `app/ponto/services/cartorio.py:1173`)

**3. Classificação das dez failures**

Os sete casos do selo de fio mudo compartilham fixtures que criam pessoa sem Batidas, célula `furo` e depois chamado; esse último ato pode relavrar a célula pelo signal. A correção é montar **furo efetivo e coerente**, preservando a lei testada, e cobrir `nunca_bateu` separadamente. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:43`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:227`, `app/chamados/signals.py:208`, `app/ponto/services/cartorio.py:1165`)

| Failure | Cura | O que preservar |
|---|---|---|
| `test_contador_por_causa_devolve_o_dia_a_quem_PODE_agir` | **TESTE** | Fixture de furo com adesão coerente; preservar “pergunta encerrada + marco apagado → admin”. Não trocar simplesmente a expectativa pela nova causa C5. (`app/chamados/tests/test_o261_fabrica_le_a_lampada.py:106`, `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:120`) |
| `test_02_MORDE_celula_que_acusa_renasce_e_a_resposta_segue` | **CÓDIGO**, mais ajuste da fixture | Preservar resposta normal quando a premissa continua viva; retirar a mordaça adicional do recebimento. (`app/chamados/tests/test_resposta_tardia.py:63`, `app/chamados/services/resposta_tardia.py:46`) |
| `test_MORDE_o_ciclo_conta_o_chamado_mudo_sem_perguntar` | **TESTE** | Contagem 1, nenhuma emissão e rc 2 para furo elegível. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:266`) |
| `test_MORDE_competencia_exportada_VOLTA_a_ser_pedida` | **TESTE** | Manter exportação real e a prova de que ela não cala o furo elegível. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:87`) |
| `test_MORDE_desligado_e_barrado_e_NOMEADO` | **TESTE** | Manter desligado com furo efetivo e a barreira RH nomeada; C5 agora sai antes dessa barreira. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:78`, `app/chamados/management/commands/disparar_perguntas_competencia.py:79`, `app/chamados/management/commands/disparar_perguntas_competencia.py:103`) |
| `test_MORDE_o_cron_leva_os_dois_caminhos_e_diz_por_porta` | **TESTE** | Preservar os universos e a distribuição por empresa/porta. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:176`) |
| `test_MORDE_o_emissor_ACHA_o_chamado_mudo` | **TESTE** | Preservar seleção do chamado mudo, inclusive quando o caminho da célula já foi servido. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:68`) |
| `test_MORDE_pergunta_MORTA_do_MESMO_FATO_e_piso_F1t` | **TESTE** | Preservar identidade do fato e `FIO DECIDIDO 1`; essa barreira continua no contador. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:129`, `app/chamados/management/commands/disparar_perguntas_competencia.py:112`) |
| `test_sem_chave_conta_e_nao_grava_nada` | **TESTE** | Preservar contagem, ausência de DRY/APLICADO e quantidade de perguntas estável. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:205`) |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` | **TESTE / registro** | Atualizar a ocorrência congelada `1196 → 1199`, conservando detector e igualdade. (`app/core/tests/test_medida_turno_do_par.py:268`, `app/chamados/services/disputa_emissao.py:1199`, `app/core/tests/test_medida_turno_do_par.py:539`) |

Portanto, **oito ajustes de fixture, um de código e um de registro**. O selo de fio mudo continua guardando leis válidas: seleção, identidade, desligado nomeado, exportação sem mordaça, relatório e ciclo sem escrita. O corte C5 não autoriza apagar essas provas. (`app/chamados/tests/test_selo_emissor_fio_mudo.py:1`, `app/chamados/management/commands/disparar_perguntas_competencia.py:79`)

**4. Contador `nunca_bateu_sem_adesao`**

Chamando de `P` os pares selecionados, o código efetivamente calcula:

```text
P = pares de chamado VIVO com célula nunca_bateu
    ∩ janela do disparar
    ∩ [início da anterior por empresa, ontem]

numero     = |P|
esperado   = |P ∩ C5|
fora_da_c5 = P − C5
```

A seleção e essas operações estão em `app/chamados/management/commands/disparar_perguntas_competencia.py:31`, `app/chamados/management/commands/disparar_perguntas_competencia.py:70` e `app/chamados/management/commands/disparar_perguntas_competencia.py:79`.

O dedup é por `(colaborador, dia)`, independentemente do número de chamados; pergunta viva não exclui o par, pois o corte acontece antes desse exame. O esperado mede **cobertura de P pela C5**, não o tamanho integral da C5; itens C5 sem chamado vivo ficam invisíveis. (`app/chamados/management/commands/disparar_perguntas_competencia.py:37`, `app/chamados/management/commands/disparar_perguntas_competencia.py:41`, `app/chamados/management/commands/disparar_perguntas_competencia.py:79`)

- **Hoje:** não produz divergência, porque sai dos dois números pelo teto ontem. (`app/chamados/management/commands/disparar_perguntas_competencia.py:32`)
- **Empresa sem corte configurado:** recebe default 21 e clamp; a janela é calculada pelo juiz, sem exigir um registro de “janela”. (`app/ponto/janelas.py:36`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:66`)
- **Desligado sem lavra que o retenha:** produz divergência por construção. Entra em `P`, mas legitimamente sai da C5; o teste novo exige exatamente `1/0`. O placar alarma essa desigualdade. (`app/chamados/management/commands/disparar_perguntas_competencia.py:79`, `app/escala/services/cadastro_realidade.py:153`, `app/escala/services/cadastro_realidade.py:197`, `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:213`, `app/core/management/commands/pipeline_placar.py:105`)

Esse alarme pode persistir enquanto o par continuar no recorte; não é necessariamente eterno, porque a janela anda. A ocorrência concreta em produção é **[nao verificado]**. A correção deve alinhar universo e significado da divergência à autoridade C5, preservando suas exclusões; fazer `esperado=numero` apenas tornaria a comparação tautológica. (`app/chamados/management/commands/disparar_perguntas_competencia.py:35`, `app/escala/services/cadastro_realidade.py:144`)

O relógio novo está corretamente recebido da borda e repassado à C5. Continua o escape herdado `_ja_perguntou → fio_vivo`, sem `hoje`, alcançando fallback de relógio interno. (`app/chamados/management/commands/vigiar_fio_morto.py:166`, `app/chamados/management/commands/disparar_perguntas_competencia.py:38`, `app/chamados/services/disputa_emissao.py:1421`, `app/chamados/juizes.py:577`)

**5. Testes novos e suite** [nao verificado]

**A classe C reaparece parcialmente:** o teste sem banco extrai funções por AST, remove decorators e executa em namespace artificial; também conserva `E4A2_BASE` com `git show` e seleção de mutante por ambiente. (`app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:39`)

Duas ressalvas impedem uma acusação excessiva:

- Os mutantes **já são percorridos por teste coletável**, com controle e mutação em memória; não dependem apenas de rodadas externas. (`app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:321`)
- Sem `E4A2_BASE`, não há chamada de git. A montagem só de `app/` quebra esse modo BASE, não necessariamente a execução padrão. A suite executa descoberta Django sobre suas labels. (`app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:41`, `bin/suite.sh:59`, `bin/suite.sh:100`)

Há duplicação dos cenários zero-pergunta/C5 e cartório entre os dois testes novos; publicação e placar também repetem provas já existentes com comandos reais. (`app/chamados/tests/test_e4a2_nunca_bateu.py:19`, `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:164`, `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:269`, `app/core/tests/test_fio_mudo_placar.py:105`)

**O selo Django novo não demonstra mordida específica do corte:** a fixture compartilhada não cria vínculo/turno; sem vínculo, não há marcos vencidos, e outra guarda já devolve zero motivos. Assim, zero perguntas pode continuar verde mesmo retirando o corte. Essa é uma conclusão estática; execução do mutante: **[nao verificado]**. (`app/chamados/tests/test_fabrica_por_evento.py:22`, `app/ponto/services/cartorio.py:840`, `app/ponto/services/cartorio.py:856`, `app/chamados/services/fabrica_da_celula.py:25`, `app/chamados/services/fabrica_da_celula.py:38`)

Forma mínima recomendada: um teste Django real com vínculo, marcos vencidos e ata acusadora; controle positivo `furo → emite`, depois `nunca_bateu → não emite/C5`; casos exclusivos de contador/repasse em chamadas reais importadas; reutilização do selo de carimbo/placar. Retirar o carregador AST/env/git e conservar mutantes coletáveis no padrão dos comandos reais. (`app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:39`, `app/core/tests/test_fio_mudo_placar.py:274`)

**6. Classes já reprovadas**

| Classe | Resultado nesta volta |
|---|---|
| Segundo juiz registrado | **Não demonstrado só pela existência do helper.** `_C5` do registro significa “dia do chamado”, não a lista Cadastro C5; `_C12` continua sendo `motivo_fora_da_fabrica`. O defeito comprovado é anunciar pertença à C5 usando um predicado mais amplo. (`app/core/juizes.py:111`, `app/core/juizes.py:277`, `app/core/juizes.py:284`, `app/chamados/services/fabrica_da_celula.py:69`) |
| Segundo escritor fora da porta | Não aparece nas alterações de produção examinadas: o carimbo copia os contadores e o corte tardio suprime uma chamada à porta existente. (`app/chamados/management/commands/vigiar_fio_morto.py:231`, `app/chamados/services/resposta_tardia.py:46`) |
| Relógio fora da borda | Presente no caminho herdado de `_ja_perguntou`; o novo contador recebe `hoje`. (`app/chamados/services/disputa_emissao.py:1422`, `app/chamados/juizes.py:577`, `app/chamados/management/commands/disparar_perguntas_competencia.py:38`) |
| Derivação paralela da fonte única | O esperado literal do carimbo foi corrigido. A equivalência “lâmpada `nunca_bateu` = entregue à C5” continua divergindo da leitura única declarada pela C5. (`app/chamados/management/commands/vigiar_fio_morto.py:231`, `app/escala/services/cadastro_realidade.py:76`, `app/escala/services/cadastro_realidade.py:197`) |
| Selo sem mordida | Risco demonstrado estaticamente no novo teste de integração, pela ausência do controle positivo exigível. (`app/chamados/tests/test_e4a2_nunca_bateu.py:27`, `app/chamados/tests/test_fabrica_por_evento.py:22`, `app/ponto/services/cartorio.py:856`) |
| Campo com dois significados | Presente na equiparação de `nunca_bateu` a “nunca usou o app” e no nome/causa que anuncia lista efetiva sem verificar seu universo. (`app/ponto/services/cartorio.py:1165`, `app/escala/services/cadastro_realidade.py:78`, `app/chamados/services/pergunta_no_app.py:35`) |

## 10/11 01:54 · sessao 98a3e151b86b · - · - · origem=Ronald (tmux)

A resposta precisa separar **responder uma PerguntaDisputa existente** de **responder o fio de um chamado encerrado**: são portas diferentes (`app/api/views.py:2607`; `app/chamados/views.py:1041`).

**Pergunta existente:** a API verifica `pergunta_viva`; se viva, chama o core e grava a resposta, sem passar por `resposta_tardia`. Se morta, retorna 404 explícito. O formulário HTML segue a mesma porta (`app/api/views.py:2634-2660`; `app/chamados/views.py:1237-1246`, `app/chamados/views.py:1280-1287`). O core escreve `resposta_colab`, `respondida_em` e `DecisaoPergunta` no mesmo atomic (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1503-1512`).

**Fio de chamado encerrado:** o corte novo impede tentar renascer quando `veredito == nunca_bateu`; chamado vivo retorna antes desse corte. No caminho tardio, o conteúdo não desaparece: vira nota e auditoria; somente `conteudo` livre abre Pauta DP. Hora/motivo/escape ficam na nota, sem seguir a declaração normal. A view devolve 200 com `estado=resposta_tardia`, portanto não é silêncio HTTP (`app/chamados/services/resposta_tardia.py:35-69`; `app/chamados/views.py:1053-1055`, `app/chamados/views.py:1089-1100`).

**Meu voto para a falha de `test_resposta_tardia.py:74`: CÓDIGO.** Não inverter a asserção para legitimar que uma resposta recebida deixe de seguir o caminho normal quando a premissa permanece viva: essa é precisamente a lei declarada pelo serviço e pela view. O corte de emissão automática da C5 não demonstra autorização para essa mudança no recebimento de resposta (`app/chamados/services/resposta_tardia.py:6-10`, `app/chamados/services/resposta_tardia.py:46-49`; `app/chamados/views.py:1048-1049`). A fixture também merece correção: pede `furo`, mas não cria Batida; criar o chamado aciona julgamento, e ausência de primeira Batida permite lavrar `nunca_bateu`. Corrigir somente essa fixture esconderia o novo corte (`app/chamados/tests/test_resposta_tardia.py:25-45`, `app/chamados/tests/test_resposta_tardia.py:63-77`; `app/chamados/signals.py:208-216`; `app/ponto/services/cartorio.py:1165-1173`, `app/ponto/services/cartorio.py:1252-1254`).

“Usou o app” e “tem adesão” não são equivalentes técnicos nesta cópia: adesão deriva da primeira Batida não retratada, não de login ou resposta. A C5 usa a lavra; não reconsulta uso atual do aplicativo (`app/ponto/services/cartorio.py:1165-1167`; `app/escala/services/cadastro_realidade.py:74-78`, `app/escala/services/cadastro_realidade.py:181-183`).

| Failure | Cura | Fundamentação |
|---|---|---|
| `test_contador_por_causa_devolve_o_dia_a_quem_PODE_agir` | **TESTE** | Constrói `furo`, cria chamado e pergunta sem Batidas; o signal pode relavrar `nunca_bateu`, agora causa C5/admin. Para conservar a lei original “pergunta encerrada + marco ainda apagado”, montar adesão real e manter a asserção original; cobrir C5 em caso próprio (`app/chamados/tests/test_o261_fabrica_le_a_lampada.py:25-51`, `app/chamados/tests/test_o261_fabrica_le_a_lampada.py:106-121`; `app/chamados/tests/test_pergunta_no_app.py:30-46`; `app/chamados/signals.py:208-216`). |
| `test_02_MORDE_celula_que_acusa_renasce_e_a_resposta_segue` | **CÓDIGO**, com fixture a tornar coerente | Guarda resposta efetiva quando premissa viva; não substituir por expectativa tardia. A fixture cria `furo` sem primeira Batida e permite relavra C5 (`app/chamados/tests/test_resposta_tardia.py:63-77`; `app/chamados/services/resposta_tardia.py:6-10`, `app/chamados/services/resposta_tardia.py:46-49`; `app/ponto/services/cartorio.py:1165-1173`). |
| `test_MORDE_o_ciclo_conta_o_chamado_mudo_sem_perguntar` | **TESTE** | `_mudo` cria pessoa sem Batidas, célula `furo` e depois chamado: sinal pode trocar a lavra. Preservar contagem 1, zero emissão e rc 2 para **furo real** (`app/chamados/tests/test_selo_emissor_fio_mudo.py:227-239`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:266-275`; `app/chamados/signals.py:208-216`). |
| `test_MORDE_competencia_exportada_VOLTA_a_ser_pedida` | **TESTE** | Mesmo `_mudo` sem adesão; manter prova de que exportação não cala **furo**. Não eliminar a exportação real nem inverter para zero (`app/chamados/tests/test_selo_emissor_fio_mudo.py:43-56`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:87-117`). |
| `test_MORDE_desligado_e_barrado_e_NOMEADO` | **TESTE** | Mesmo `_mudo`; `nunca_bateu` agora sai antes da barreira `desligado`. Usar célula efetivamente `furo` para continuar guardando a barreira RH e sua contagem nomeada (`app/chamados/tests/test_selo_emissor_fio_mudo.py:43-56`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:78-85`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79-81`, `app/chamados/management/commands/disparar_perguntas_competencia.py:103-105`). |
| `test_MORDE_o_cron_leva_os_dois_caminhos_e_diz_por_porta` | **TESTE** | Restaurar fixture do universo furo e manter relatório por empresa/porta (`app/chamados/tests/test_selo_emissor_fio_mudo.py:176-182`; `app/chamados/management/commands/disparar_perguntas_competencia.py:195-207`). |
| `test_MORDE_o_emissor_ACHA_o_chamado_mudo` | **TESTE** | Restaurar furo real após efeitos de criação; manter seleção do chamado cujo caminho da célula já foi servido (`app/chamados/tests/test_selo_emissor_fio_mudo.py:43-56`, `app/chamados/tests/test_selo_emissor_fio_mudo.py:68-76`). |
| `test_MORDE_pergunta_MORTA_do_MESMO_FATO_e_piso_F1t` | **TESTE** | Restaurar furo real; manter identidade do fato e `FIO DECIDIDO 1`, pois a barreira continua no contador (`app/chamados/tests/test_selo_emissor_fio_mudo.py:129-156`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110-114`). |
| `test_sem_chave_conta_e_nao_grava_nada` | **TESTE** | Restaurar furo real e manter contagem 1, ausência DRY/APLICADO e quantidade de perguntas estável (`app/chamados/tests/test_selo_emissor_fio_mudo.py:205-213`). |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` | **TESTE / registro** | Registro ainda fixa derivação em `1196`; ocorrência foi deslocada para `1199` pelas três linhas do corte. Atualizar localização, conservando detector e igualdade (`app/core/tests/test_medida_turno_do_par.py:268`, `app/core/tests/test_medida_turno_do_par.py:539-543`; `app/chamados/services/disputa_emissao.py:1199`). |

As leis guardadas por `test_selo_emissor_fio_mudo` continuam pertinentes para o universo **furo elegível**: seleção de chamado mudo, identidade do fato, desligado nomeado, exportação sem mordaça, relatório por empresa e ciclo sem escrita. O próprio arquivo declara esses objetivos, e o contador conserva essas barreiras; o novo ramo C5 antecede todas elas (`app/chamados/tests/test_selo_emissor_fio_mudo.py:1-5`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79-119`).

A causalidade exata de cada execução falha não foi observada com traceback nem reexecução; o anexo fornece somente nomes de failures, e as classificações acima resultam da leitura estática das fixtures e dos caminhos (`[nao verificado]`).

## 10/11 01:54 · sessao a58d0ae0eeb7 · - · - · origem=Ronald (tmux)

- **Universo:** seja `P` o conjunto de `(colaborador_id, dia)` de chamados `VIVOS`, com célula `nunca_bateu`, dentro da janela explícita/global do disparar e também entre início da janela anterior **por empresa** e ontem. `P` inclui chamados com pergunta viva, pois o corte C5 vem antes desse exame; dedup é por par, sem motivo. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:31`, `:35`, `:37`, `:57`, `:70`, `:79`, `:82`.

- **Comparação efetiva:** `numero=|P|`, `esperado=|P∩C5|`, `fora_da_c5=P−C5`. Portanto verifica apenas `P⊆C5`; não mede toda a lista C5 e nunca detecta C5 sem chamado vivo. Por construção, `esperado≤numero`; o caso `(0,1)` do teste do placar verifica transporte genérico, mas não pode nascer deste contador. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:38`, `:40`; `app/core/tests/test_fio_mudo_placar.py:146`.

- **Hoje não causa diferença:** sai dos dois números pela condição `dia<=ontem`; datas anteriores ao piso empresarial também saem. Uma empresa sem corte configurado recebe default `21` e clamp `[2,28]`; o motor calcula janela, não depende de registro de janela no banco. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:32`, `:36`; `app/ponto/janelas.py:36`, `:50`, `:60`, `:66`. Empresa inexistente/relacionamento nulo em produção: `[nao verificado]`.

- **Desligado sem lavra gera diferença legítima da C5:** o contador inclui o par antes da barreira de desligado; C5 exclui desligados cujos dias não aparecem em `RetratoFolha`, balde bloqueado, motivo `furo_espelho`. A exclusão é lei explícita, não falha da lista. O novo teste exige exatamente `numero=1`, `esperado=0`. Assim o alarme pode cobrar uma igualdade incompatível com a seleção legítima enquanto o par continuar no recorte; não é necessariamente eterno, pois a janela anda. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:79`, `:103`; `app/escala/services/cadastro_realidade.py:144`, `:153`, `:185`, `:195`, `:198`; `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:210`. Ocorrência real na produção: `[nao verificado]`.

- **Relógio novo:** bordas capturam `agora` e `hoje=timezone.localdate(agora)`; contador e C5 recebem esse `hoje`. Portanto não há novo relógio interno nesse caminho. O escape herdado existe em `_ja_perguntou→fio_vivo`, sem repassar `hoje`, chegando ao fallback `date.today()`. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:139`, `:181`, `:38`; `app/chamados/management/commands/vigiar_fio_morto.py:166`, `:181`; `app/chamados/services/disputa_emissao.py:1421`; `app/chamados/juizes.py:577`. O próprio placar usa relógio da máquina na borda `handle`: `app/core/management/commands/pipeline_placar.py:148`.

- **Veredito: produtor e porta:** cartório define adesão pela primeira `Batida` não retratada, não pelo login/uso do app. Sem batida, julga cobertura e grava `nunca_bateu` quando não há cobertura integral, via `cel.lavrar_veredito`; escritor final é `ponto/portas/celula.py::lavrar_veredito`. Fontes: `app/ponto/services/cartorio.py:1165`, `:1173`, `:1242`, `:1252`, `:1253`; `app/ponto/portas/celula.py:983`, `:1044`. `nunca_bateu` continua acusando em `CelulaDia.VEREDITOS_ACUSAM`: `app/escala/models.py:1767`.

- **ESMERIL semântico concreto:** `dia_so_na_c5` significa apenas “lâmpada `nunca_bateu`”, mas seu nome/documentação e a causa `nunca_bateu_na_lista_c5` sugerem pertença/entrega efetiva na C5. Isso é falso para hoje, fora da janela e desligado sem lavra. C5 ainda explica a classe como “quem nunca usou o app”, enquanto produtor prova somente “sem batida não retratada”; responder uma pergunta não cria batida nesse produtor. Fontes: `app/chamados/services/fabrica_da_celula.py:67`; `app/chamados/services/disputa_emissao.py:758`; `app/chamados/services/pergunta_no_app.py:35`; `app/escala/services/cadastro_realidade.py:72`, `:78`, `:180`, `:198`; `app/ponto/services/cartorio.py:1165`.

- **Juiz duplicado — cuidado com diagnóstico:** o `_C5` de `core/juizes.py` é **dia do chamado**, não a lista Cadastro C5. O juiz registrado `_C12` é `motivo_fora_da_fabrica`, que agora compõe o helper; um predicado que apenas lê lâmpada não prova, sozinho, segundo juiz registrado. O problema demonstrável é o helper afirmar “vai/está só na C5” com conjunto maior que a C5 real, cuja própria documentação declara `colabs_nunca_bateu` como leitura única. Fontes: `app/core/juizes.py:111`, `:118`, `:277`, `:284`; `app/chamados/services/fabrica_da_celula.py:67`; `app/escala/services/cadastro_realidade.py:76`, `:180`. `_C19=tarefa_do_dia` existe, mas os respondedores anteriores estão explicitamente vivos até E3; não cabe declarar duplicação nova apenas pela coexistência. Fonte: `app/core/juizes.py:150`, `:153`, `:291`.

- **Fonte única do carimbo corrigida:** `vigiar_fio_morto` publica número e esperado diretamente dos contadores; placar compara somente carimbo cujo `hoje` coincide com dia solicitado, sem limiar próprio. Novo agregado `nunca_bateu_sem_adesao` completo é publicado no topo; placar lê apenas cópias numéricas de `censo` e `esperado`. Fontes: `app/chamados/management/commands/vigiar_fio_morto.py:231`, `:237`, `:240`; `app/core/management/commands/pipeline_placar.py:94`, `:98`, `:105`, `:135`.

## 10/11 01:53 · sessao a33a400afcae · - · - · origem=Ronald (tmux)

Item 5: **a classe C reaparece parcialmente**, com duas ressalvas importantes.

- Há AST-exec de função isolada, retirada de decorators e namespace artificial: `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:39-54`. Há leitura de `E4A2_BASE` e `git show`: `:41-43`; mutação por env: `:44-49`.
- **Os mutantes não dependem exclusivamente de execução externa**: o método coletável de `MutantesTest` percorre controles e oito mutantes, aplicados em memória por `patch.dict(os.environ, ...)`: `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:321-350`.
- **Sem `E4A2_BASE`, não há chamada de git**: condição explícita em `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:41-43`. Portanto ausência de `.git` não impede, por si, a execução padrão; o modo BASE é incompatível com a montagem que disponibiliza somente `app/`: `bin/suite.sh:74`, `:100-103`.
- Os módulos pertencem às labels `chamados`/`core`: `bin/regua.sh:23`; a suite lê essas labels e executa `manage.py test`: `bin/suite.sh:59`, `:102-103`. A classe sem banco deriva de `unittest.TestCase`: `app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:96`; o runner da casa herda `DiscoverRunner` e entrega a descoberta ao pai: `app/core/runner_quarentena.py:72-76`. Execução efetiva dessa coleta nesta análise: **[nao verificado]**.

**Duplicação:** as provas “zero pergunta/um C5” e “cartório chama fábrica” são repetidas no sem banco (`app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:164-194`) e integração (`app/chamados/tests/test_e4a2_nunca_bateu.py:19-47`). O sem banco também repete publicação/carimbo/placar (`:269-318`) já guardados pelos comandos reais em `app/core/tests/test_fio_mudo_placar.py:105-146`, `:148-215`.

**Selo sem mordida específica no teste Django novo:** a fixture compartilhada cria empresa, posto, colaborador e célula, sem criar vínculo/turno: `app/chamados/tests/test_fabrica_por_evento.py:22-28`. Sem vínculo, `marcos_com_teto` retorna vazio: `app/ponto/services/cartorio.py:840-857`; o passado sem marcos retorna `None`: `app/chamados/services/fabrica_da_celula.py:25-29`; `motivos_pela_ata` devolve `[]` sem vencidos: `:38-41`. Assim, retirar o corte de `nunca_bateu` ainda pode manter as asserções de zero perguntas verdes, por outra guarda; o teste novo não estabelece marcos/ata exigíveis nem controle positivo: `app/chamados/tests/test_e4a2_nunca_bateu.py:27-47`. Resultado de mutação executada: **[nao verificado]**.

**Forma mínima recomendada:** manter um teste Django de integração com vínculo, marcos vencidos e ata acusadora; provar primeiro que `furo` emite, depois que `nunca_bateu` não emite e permanece na C5; reutilizar a prova existente de comandos reais para carimbo/placar (`app/core/tests/test_fio_mudo_placar.py:105-146`). Migrar somente os casos exclusivos do sem banco — dedup/interseção/desligado/pergunta viva/repasse — para chamadas reais importadas, com fixtures ORM ou costuras explícitas; retirar o carregador AST/env/git (`app/chamados/tests/test_e4a2_nunca_bateu_sem_banco.py:39-54`). Para mutantes coletáveis, o padrão já existente preserva imports, recompila módulo integral e percorre controles/mutantes dentro do teste: `app/core/tests/test_fio_mudo_placar.py:274-302`.

Adicional: o comentário “não reexecuta os testes dele” merece conferência, pois a classe `FabricaPorEvento` é importada globalmente no novo módulo (`app/chamados/tests/test_e4a2_nunca_bateu.py:11-17`); a ausência de duplicação efetiva pela descoberta Django é **[nao verificado]**.

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

_3324 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
