# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 22:06** · publicado em **31/10/2026 22:08** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 22:06 · sessao 06d2750e0795 · - · - · origem=Ronald (tmux)

Há mudança sintética em todos os caminhos citados com pontas e pausa **aware UTC com segundos**. Sonda em memória com os corpos reais extraídos por AST: entrada `2026-10-15 01:00Z`, saída `10:00Z`, pausa `07:59:30Z–08:01Z`, `prorrogacao=False`; equivalem a `22:00–07:00`, pausa `04:59:30–05:01` local. A conversão é a de `app/ponto/motor_calculo_v2.py:73-79`; a fixture corresponde a `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:460-464`.

| Caminho | Mutante de 14a61ef3 | Corpo atual | Evidência |
|---|---:|---:|---|
| Desconto | 1 | 0,5 | `app/ponto/motor_calculo_v2.py:186-196`; mutante `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:483-508` |
| `MotorBase._noturno_dos_intervalos` | 1 | 0,5 | `app/ponto/motor_calculo_v2.py:1915-1916` |
| `_ContextoNoturno._noturno_dos_intervalos` | 1 | 0,5 | `app/ponto/calculador/regras.py:60-63` |
| `calcular_noturnas`, fator 1 | `(419,419)` | `(419,5;419,5)` | `app/ponto/motor_calculo_v2.py:971-980` |
| `noturnas_do_dia`, fator 1 | `(419,419)` | `(419,5;419,5)` | `app/ponto/calculador/regras.py:80-89` |
| `noturno_no_par`, fator 1 | relógio/legais 419 | relógio/legais 419,5 | `app/ponto/calculador/periodo.py:508-511` |
| `he_noturna`, HE100=538,5 min e HE50=0 | `(0,419)` | `(0,419,5)` | `app/ponto/motor_calculo_v2.py:440-451` |

O caso HE usa todos os 538,5 minutos trabalhados como extra: `_inicio_do_trecho_extra` recua até a entrada, portanto consulta a mesma janela inteira. O recuo pelo trabalho está em `app/ponto/motor_calculo_v2.py:392-406`; a subtração do desconto está em `:441-443`.

O caminho de **HE por período** importa `he_noturna` em `app/ponto/calculador/regras.py:868`, fornece pontas/intervalos/flag em `:886-888`, soma os retornos em `:889-890` e escreve horas nas rubricas em `:896-898`. Com uma ponta que recebe a jornada inteira em HE100, o mesmo delta chega à rubrica: `419/60 = 6,983333333333333 h` contra `419,5/60 = 6,991666666666666 h`; essa propagação decorre das divisões explícitas em `:896-898`. Execução integral de `do_dia`: **[nao verificado]**.

Escritores e leitores externos relevantes:

| Valor | Quem escreve/fornece | Quem lê |
|---|---|---|
| Flag do motor | `MotorBase.__init__`, recebe parâmetro ou consulta regra legal e atribui `self.prorrogacao_noturna_pos5h`: `app/ponto/motor_calculo_v2.py:758-762` | `calcular_noturnas` `:971-973`, delegador `:1915-1916`, casca de HE `:1002-1006` |
| Flag no contexto | `_ContextoNoturno.__init__`: `app/ponto/calculador/regras.py:60-61` | Método emprestado em `:63`, cujo corpo lê o atributo em `app/ponto/motor_calculo_v2.py:1916` |
| Flag na CCT | `cct_do_motor` constrói `CCT(prorrogacao_pos5h=bool(motor.prorrogacao_noturna_pos5h))`: `app/ponto/calculador/nucleo.py:344-351` | `noturno_no_par`: `app/ponto/calculador/periodo.py:508-510` |
| Pontas efetivas/intervalos carimbados | `_carimbar_segmento`: `app/ponto/motor_calculo_v2.py:1828-1830` | Releitura passa `e`, `s_` e `intervalos_do_motor` para `noturno_no_par`: `app/ponto/calculador/periodo.py:752` |
| Retorno de noturnas do motor | `calcular_noturnas`: `app/ponto/motor_calculo_v2.py:971-975` | `_escrever_noturnas`, que grava `periodo.minutos_noturnos_relogio/legais`: `:991-993` |
| Retorno de HE do motor | `he_noturna`: `app/ponto/motor_calculo_v2.py:448-451` | `_aplicar_he_noturna`, que grava campos do período: `:1002-1010` |
| Retorno de noturnas do calculador | `noturnas_do_dia`: `app/ponto/calculador/regras.py:85-89` | Rubrica/detalhes do dia: `:908-914`; encapsulamento em `NoturnoV3`: `app/ponto/calculador/periodo.py:508-511` |
| `NoturnoV3.relogio_min/legais_min` | Construtor em `app/ponto/calculador/periodo.py:511` | Releitura escreve os campos do período em `:752-754` |
| HE por período | Acumuladores `_s50/_s100`: `app/ponto/calculador/regras.py:877`, `:889-890` | Rubricas `horas_extras_50_noturna/100_noturna`: `:896-898` |

**“Aware UTC com segundos não muda nenhum chamador” NÃO procede**: a sonda acima demonstra mudança numérica nos leitores reais. Impacto em colaborador, rubrica persistida ou dados produtivos: **[nao verificado]**.

A sonda usou `python3 -B` e AST em memória; Django não está instalado neste ambiente. Não houve acesso a banco/log, import de modelos ou gravação de arquivo. Execução Django canônica e integração completa: **[nao verificado]**.

## 31/10 22:03 · sessao 63fba4a5c37a · - · - · origem=Ronald (tmux)

Inventário, na cópia atual:

| Arquivo | Escritor → leitor | Excluído |
|---|---|---|
| `logs/sessao/disjuntor.log` | `trilha`, inclusive `_anuncia` de B3/PDC/rearme → `disjuntor.ler`; `bin/disjuntor.py:135`, `bin/disjuntor.py:975`, `bin/disjuntor.py:1051`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:2168` | Sim; `bin/hook_stop_fila1.py:585`. |
| `logs/sessao/disjuntor_estado.json` | `_com_estado` → `_com_estado`, `ler_estado`; `bin/disjuntor.py:267`, `bin/disjuntor.py:278`, `bin/disjuntor.py:299` | Sim; `bin/hook_stop_fila1.py:585`. |
| Seu `.lock` | `_com_estado`: cria/usa `flock`; `bin/disjuntor.py:256`, `bin/disjuntor.py:260` | Sim; `bin/hook_stop_fila1.py:586`. |
| `logs/padrao_de_cima_escopo.json` | `_anuncia` PDC → `_pdc_grava_escopo`; leitor `_pdc_escopo_lido`/`estado_pdc`; `bin/disjuntor.py:1462`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:1526`, `bin/disjuntor.py:1546` | Sim, pelo dono; `bin/hook_stop_fila1.py:593`. |
| `logs/sessao/padrao_de_cima_geracao` | `decide_pdc`/`_pdc_geracao_grava` → `_pdc_geracao_lida`/`estado_pdc`; `bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1497`, `bin/disjuntor.py:1548` | Sim, pelo dono; `bin/hook_stop_fila1.py:593`. |
| `logs/sessao/cota.json` | Status line → `ler_cota`; `bin/statusline_cota.py:49`, `bin/statusline_cota.py:54`, `bin/disjuntor.py:1653` | Sim; `bin/hook_stop_fila1.py:586`. |
| `logs/sessao/pisos.log` | Hook chama `OC.registrar_piso` → `ultimo_piso`, `gatilho_tokens`, `disjuntor.ler`; `bin/hook_stop_fila1.py:911`, `bin/orcamento_contexto.py:198`, `bin/orcamento_contexto.py:210`, `bin/orcamento_contexto.py:227`, `bin/disjuntor.py:2230` | Sim; `bin/hook_stop_fila1.py:585`. |
| `logs/hook_stop_fila1.log` | `_trilha` → `b2_decide`; `bin/hook_stop_fila1.py:48`, `bin/disjuntor.py:1754`, `bin/disjuntor.py:1761` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/hook_stop_fila1.json` | `_zera_teto`, contador principal → contador principal; `bin/hook_stop_fila1.py:1079`, `bin/hook_stop_fila1.py:1267`, `bin/hook_stop_fila1.py:1288` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/hook_stop_espera.json` e `.lock` | `_prova_arquivo`: memória/trava → a própria função; `bin/hook_stop_fila1.py:615`, `bin/hook_stop_fila1.py:625`, `bin/hook_stop_fila1.py:637` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/sessao/orcamento_estado.json` e `.lock` | Hook: `_orc_salva`, `_orcamento` → `_orcamento_travado`, `OC.ler`, `disjuntor.ler`; `bin/hook_stop_fila1.py:764`, `bin/hook_stop_fila1.py:832`, `bin/hook_stop_fila1.py:856`, `bin/orcamento_contexto.py:357`, `bin/disjuntor.py:2210` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/sessao/orcamento.log` | `_orc_trilha` → `OC.ler`, `disjuntor.ler`; `bin/hook_stop_fila1.py:738`, `bin/orcamento_contexto.py:364`, `bin/disjuntor.py:2169` | Sim; `bin/hook_stop_fila1.py:596`. |
| `app/docs/RELATO.md` | `relato_negacao` delega a `esteira_vigia.no_relato` → hook e decisão PDC; `bin/disjuntor.py:234`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:884`, `bin/hook_stop_fila1.py:429`, `bin/disjuntor.py:1376` | Sim; `bin/hook_stop_fila1.py:596`. |
| `logs/codex/triagem.log` | `--triagem`, via `_triagem_sob_trava` → `_triagens`; `bin/disjuntor.py:837`, `bin/disjuntor.py:839`, `bin/disjuntor.py:861`, `bin/disjuntor.py:2326` | **Não**; lista completa em `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:593`, `bin/hook_stop_fila1.py:596`. |
| `<resposta>.lock` | `triagem` cria e toma `LOCK_SH`; wrapper também usa a trava; `bin/disjuntor.py:806`, `bin/disjuntor.py:813`, `bin/codex_pergunta.sh:157` | **Não**; `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:596`. |

`BACKLOG` também é excluído, embora não haja escrita nele nesses três componentes nos sítios encontrados; sua leitura e exclusão estão em `bin/hook_stop_fila1.py:173`, `bin/hook_stop_fila1.py:596`; ausência absoluta de outro escritor indireto: [nao verificado].

Os temporários também **não estão na lista**: `.disjuntor_estado.*`, `.padrao_de_cima_escopo.*`, `.cota.*`, `.hook_stop_espera.*`, `.orcamento_estado.*`, `.RELATO.*.novo`; criação/substituição em `bin/disjuntor.py:278`, `bin/disjuntor.py:1518`, `bin/statusline_cota.py:49`, `bin/hook_stop_fila1.py:633`, `bin/hook_stop_fila1.py:764`, `app/core/esteira_vigia.py:879`; lista em `bin/hook_stop_fila1.py:596`.

Para temporários, a omissão não demonstra automaticamente uma liberação: cada escrita usa nome/inode novo e depois `os.replace`; a memória exige a mesma chave caminho+dispositivo+inode em duas observações, com mtime crescente; `bin/disjuntor.py:278`, `bin/disjuntor.py:281`, `bin/hook_stop_fila1.py:609`, `bin/hook_stop_fila1.py:629`, `bin/hook_stop_fila1.py:649`. Uma corrida com duas observações durante a escrita não está proibida pela lista; exploração concreta: [nao verificado].

`triagem.log` é o residual estável mais claro: é append no mesmo arquivo e não está excluído, portanto o avanço entre observações satisfaz `_prova_arquivo`; `bin/disjuntor.py:839`, `bin/hook_stop_fila1.py:609`, `bin/hook_stop_fila1.py:649`. Sua escrita requer a operação explícita de triagem e sidecar válido; não há evidência de que seja produzida pelo simples Stop/`--padrao-parada`; `bin/disjuntor.py:822`, `bin/disjuntor.py:827`, `bin/disjuntor.py:2326`. **Fila**, por inventário incompleto; chamar isso de avanço autônomo sem produtor seria [nao verificado].

A trava de `no_relato` é no diretório `app/docs`, sem arquivo `.lock` próprio; diretórios não servem de prova; `app/core/esteira_vigia.py:844`, `app/core/esteira_vigia.py:848`, `bin/hook_stop_fila1.py:581`.

Fragilidades adicionais:

- **Fila:** `_pdc_grava_escopo` e status line podem abandonar temporários quando há exceção após `mkstemp`; seus `except` apenas registram/engolem, sem `unlink`; `bin/disjuntor.py:1518`, `bin/disjuntor.py:1522`, `bin/statusline_cota.py:49`, `bin/statusline_cota.py:55`. Hook, orçamento, estado DJ e `no_relato` possuem limpeza explícita; `bin/hook_stop_fila1.py:638`, `bin/hook_stop_fila1.py:769`, `bin/disjuntor.py:284`, `app/core/esteira_vigia.py:885`.
- **Fila:** a frase do caso 8b sobre o escopo liberar porque o mtime anda omite que o escopo é substituído atomicamente e a chave inclui inode; regravação normalmente cria primeira observação de nova chave, enquanto a geração é reescrita no lugar; `bin/tests/test_hook_esperando_com_prova.sh:124`, `bin/disjuntor.py:1518`, `bin/disjuntor.py:1521`, `bin/hook_stop_fila1.py:609`, `bin/disjuntor.py:1487`. O teste pode morder a ausência de exclusão pelo **motivo esperado**, sem reproduzir um `ALLOW` no escopo; `bin/tests/test_hook_esperando_com_prova.sh:135`, `bin/tests/test_hook_esperando_com_prova.sh:87`.
- A escrita da geração ocorre antes do estado, e o escopo é escrito no anúncio sob a mesma trava; a alteração de comentários não muda essa ordem; `bin/disjuntor.py:1454`, `bin/disjuntor.py:1462`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:275`, `bin/disjuntor.py:292`.

## 31/10 22:03 · sessao 9156207494da · - · - · origem=Ronald (tmux)

- O import é tardio e ocorre só depois de o alvo existir e ser regular; `ImportError`, `SyntaxError`, falha de acesso e ausência das constantes entram em `except Exception` e devolvem motivo não vazio, invalidando a prova por arquivo — inclusive quando acompanhada de PID válido. PID sozinho não executa esse trecho. (`bin/hook_stop_fila1.py:576`, `bin/hook_stop_fila1.py:590`, `bin/hook_stop_fila1.py:685`)

- **Fila — memória congelada em falha transitória.** O retorno de erro antecede aquisição da trava, leitura e gravação de `ESPERA`; conserva a observação anterior, contrariando o “sempre” da docstring. Ao recuperar, um avanço ocorrido durante a indisponibilidade pode liberar na primeira leitura recuperada, sem movimento posterior à recuperação. A decisão ainda mede apenas avanço desde a última observação persistida. (`bin/hook_stop_fila1.py:564`, `bin/hook_stop_fila1.py:594`, `bin/hook_stop_fila1.py:615`, `bin/hook_stop_fila1.py:629`, `bin/hook_stop_fila1.py:649`)

- `HOOK_STOP_RAIZ` troca os caminhos de dados; o código importado é procurado no diretório do `__file__` do hook. As duas constantes relativas são combinadas com `RAIZ` do hook, portanto não é preciso existir `disjuntor.py` na árvore de dados alternativa. (`bin/hook_stop_fila1.py:26`, `bin/hook_stop_fila1.py:33`, `bin/hook_stop_fila1.py:591`, `bin/hook_stop_fila1.py:593`, `bin/disjuntor.py:1101`, `bin/disjuntor.py:1104`)

- No fluxo normal de `main`, o disjuntor já foi importado pela passagem B4 antes da prova de espera. O import novo normalmente reutiliza o módulo carregado; `sys.path.insert` não garante recarregar outra versão já presente em `sys.modules` [nao verificado quanto à ocorrência neste host]. (`bin/hook_stop_fila1.py:1046`, `bin/hook_stop_fila1.py:1215`, `bin/hook_stop_fila1.py:1240`)

- O import novo não executa `decide_pdc`: só acessa constantes. Assim, por si, não regrava geração, escopo nem trilha; a transição que os grava continua em `decide_pdc` → `_com_estado` → `_anuncia`. (`bin/hook_stop_fila1.py:592`, `bin/hook_stop_fila1.py:593`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:1466`)

- Os leitores externos do hook não ganham essa dependência em tempo de importação: handoff pede funções de fila; interrompidas importa três funções de fila; backlog_estado importa o módulo; o disjuntor pede o recorte do topo e `H.RELATO`. O import novo só está no corpo de `_prova_arquivo`. (`bin/handoff_sessao.sh:49`, `bin/handoff_sessao.sh:64`, `bin/handoff_sessao.sh:260`, `bin/handoff_sessao.sh:282`, `bin/interrompidas_estado.py:53`, `bin/backlog_estado.py:20`, `bin/disjuntor.py:689`, `bin/disjuntor.py:703`, `bin/hook_stop_fila1.py:590`)

- Os dois consumidores internos da prova de espera — orçamento e `main` — compartilham `_ESPERA_MEMO`; uma falha fica memorizada durante esse processo e não é reavaliada no segundo consumidor. (`bin/hook_stop_fila1.py:704`, `bin/hook_stop_fila1.py:997`, `bin/hook_stop_fila1.py:1240`)

- **Fila — captura não universal.** `SystemExit`, `KeyboardInterrupt` e encerramento por sinal não são contidos pelos `except Exception` do trecho, de `_esperando_com_prova` nem de `main`; eventual falha dessa natureza não produz necessariamente decisão JSON. Não há `SystemExit` no carregamento normal de `disjuntor.py`; seu `sys.exit(main(...))` está protegido por `__name__ == '__main__'`. (`bin/hook_stop_fila1.py:594`, `bin/hook_stop_fila1.py:690`, `bin/hook_stop_fila1.py:1314`, `bin/disjuntor.py:2342`)

- **Fila — efeitos ambientais do import.** Escrita automática de bytecode, cache existente e configuração `PYTHONDONTWRITEBYTECODE` no host são [nao verificado]. O comando Stop da cópia usa `python3` sem `-B`, e a lista de exclusões não contempla `__pycache__`; não afirmar que houve arquivo `.pyc` escrito sem evidência de ambiente. (`.claude/settings.json:9`, `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:596`)

- Quanto às classes 1–4 e 6, o trecho novo consulta as duas autoridades de caminho, não cria novo cálculo de decisão, escritor explícito, leitura de relógio nem aquisição de trava; a aquisição da trava de mtime continua depois da importação. Classe 5 depende da auditoria do teste; classe 7/pouso real é [nao verificado]. (`bin/hook_stop_fila1.py:590`, `bin/hook_stop_fila1.py:593`, `bin/hook_stop_fila1.py:615`)

## 31/10 22:03 · sessao 71cefccbab80 · - · - · origem=Ronald (tmux)

Auditoria estática; resultados de execução dos selos: [nao verificado].

- **8b morde a exclusão dos dois destinos, por motivo específico.** `_espera_bloqueia` exige simultaneamente `block`, `siga: ITEM-VIVO` e `NOPROVA.*arquivo do proprio hook`; bloquear por primeira observação não basta (`bin/tests/test_hook_esperando_com_prova.sh:80-89`, `:128-134`). Contra o ancestral sem as duas exclusões, os dois casos ficam vermelhos por esse critério; excluindo apenas um destino, o caso do outro continua vermelho (`bin/hook_stop_fila1.py:593-608`; anexo do diff).

- **FILA — a narrativa do caso do escopo exagera o que ele demonstra.** O escopo é escrito por `mkstemp` e `os.replace`; a memória identifica o alvo por caminho, dispositivo e inode. Na sequência normal com inode novo, regravar o escopo reinicia a observação daquele alvo e produz `arquivo nao andou`, em vez de liberar o Stop (`bin/disjuntor.py:1518-1521`; `bin/hook_stop_fila1.py:609-610`, `:629-650`). A geração, ao contrário, é truncada e escrita no mesmo arquivo, portanto oferece o avanço de mtime com identidade estável necessário ao bypass anterior (`bin/disjuntor.py:1478-1491`; `bin/hook_stop_fila1.py:609-650`). Reciclagem efetiva de inode nesta cópia: [nao verificado].

- **FILA — 8b não prova dependência das constantes do dono.** Uma cura com os dois caminhos literais atuais passa os mesmos casos: o teste consulta as constantes, mas não as muda e não verifica de onde a implementação obteve os caminhos (`bin/tests/test_hook_esperando_com_prova.sh:128-134`; `bin/disjuntor.py:1101-1104`; `bin/hook_stop_fila1.py:593`). Seria necessário deslocar uma constante na cópia de teste e testar o novo destino para distinguir essas implementações (`bin/tests/test_hook_esperando_com_prova.sh:128-134`).

- **FILA — uma exclusão ampla por diretório também passa 8b e não é confrontada pelo positivo atual.** Os dois novos destinos ficam em `logs`; o arquivo com escritor que deve liberar fica fora da árvore, em `$_tmp/agente.out`. Não há positivo de produtor legítimo dentro de `logs` nesses casos (`bin/tests/test_hook_esperando_com_prova.sh:38`, `:107-114`, `:128-134`; `bin/disjuntor.py:1101-1104`). A cura por diretório precisa preservar o teste de regularidade anterior para manter o motivo esperado nos casos `arquivo=logs` (`bin/hook_stop_fila1.py:578-582`; `bin/tests/test_hook_esperando_com_prova.sh:122-123`).

- **FILA — 8b não confirma que a regravação ocorreu.** `_pdc` descarta stdout/stderr, seus dois retornos não são conferidos e o único controle posterior é `-f`; não se compara mtime, inode ou conteúdo antes/depois. Com a exclusão presente, o caso pode ficar verde mesmo se a segunda chamada falhar ou parar de regravar (`bin/tests/test_hook_esperando_com_prova.sh:127-134`; `bin/hook_stop_fila1.py:598-608`). A mordida preservada é da exclusão, não da reprodução material do movimento alegado (`bin/tests/test_hook_esperando_com_prova.sh:87`, `:134`).

- **Os novos returns 7/8 corrigem duas ausências reais de observação do caso `desligado`.** O return 7 rejeita saída vazia acompanhada de status diferente de zero; o return 8 exige exatamente uma nova ocorrência da mensagem da chave por chamada. Ambos rodam para o payload cru construtor e o verificador (`bin/tests/test_padrao_de_cima.sh:328-334`). O caminho real captura o JSON não objeto, reconhece a chave, chama `trilha('FALHA', ... DISJUNTOR_DESLIGADO...)` e retorna zero (`bin/disjuntor.py:2078-2081`, `:2094-2101`).

- **Contra b3c0f8ec, os novos returns 7/8 devem passar por inspeção:** o delta de `disjuntor.py` fornecido só troca comentários; a implementação da chave que eles observam permanece a mesma (`bin/disjuntor.py:2099-2101`; anexo do diff). Execução contra esse ancestral: [nao verificado].

- **FILA — não há mutante dedicado aos novos returns 7/8.** O mutante `v9-cru-ignora-chave` remove a guarda da chave, levando à negativa e ao return 6 antes das novas verificações; os outros mutantes de `desligado` removem as portas do wrapper. Portanto os novos asserts são úteis, mas sua mordida específica contra crash mudo ou ausência da trilha não está demonstrada pelo catálogo atual (`bin/tests/test_padrao_de_cima.sh:325-334`, `:711-718`, `:816`). O harness sabe reconhecer `return 7` e `return 8` literais como asserts, caso um mutante dedicado os alcance (`bin/tests/_mordida.sh:139-145`, `:214-225`).

- **FILA menor — return 8 não confere literalmente o evento `FALHA`.** Ele conta apenas a substring `DISJUNTOR_DESLIGADO, ferramenta liberada`, embora o comentário diga “1 linha FALHA”; uma troca do rótulo do evento mantendo a mensagem passaria esse assert (`bin/tests/test_padrao_de_cima.sh:329-334`; `bin/disjuntor.py:2100`).

## 31/10 21:58 · sessao 590560d188d5 · TRAVA-COBERTURA · leitura · origem=Code

**O contrato muda a identidade do dono do trabalho e exige migração dos chamadores, dos recursos e dos selos.** O selo atual exige que matar a chamada mate o container; a proposta exige que ele sobreviva. A sombra, por sua vez, entrega a posse ao chamador por fd 8, sem receber um comando para destacar. (`bin/tests/test_trava_dono_para_filho.sh:54–63`; `bin/sombra_trava.sh:21–39`; `contrato.txt:2`.)

Uso `contrato.txt:1–5` para as cinco cláusulas numeradas do anexo. Os contraexemplos abaixo são sequências condicionais de revisão, não ocorrências medidas em produção; ocorrência real [nao verificado].

**1. O que cada linha encontra hoje e o que muda**

| Linha | Código atual da cópia | Mudança contratada e lacunas |
|---|---|---|
| **1 — quem chama** | A trava define `QUEM` como `ESTEIRA_QUEM` ou basename, acrescentando `$$`; aceita somente `--espera`, `--quem` e `--`. A suite faz `exec` para a trava. (`bin/trava_teste.sh:36`; `bin/trava_teste.sh:85–93`; `bin/suite.sh:96`.) | Introduz obra, volta, fallback de identidade e três verbos. A semântica de `--abre-volta` além da assinatura — persistência, monotonicidade e rejeição de pedidos atrasados — não está especificada. (`contrato.txt:1–2`; detalhes [nao verificado].) |
| **1 — PreToolUse** | `pretool` **já extrai** o comando Bash, mas retorna antes de qualquer fiscalização posterior quando não há `agent_id`, não é `Agent` e não contém `codex_pergunta`. Depois disso não há bloqueio Docker/suite. (`bin/disjuntor.py:1430–1475`.) | A nova fiscalização precisa anteceder esse retorno para alcançar a sessão principal. O matcher já é vazio; ampliar apenas a configuração não resolve. (`bin/disjuntor.py:1431–1432`; `.claude/settings.json:62–68`; `contrato.txt:1,3`.) |
| **2 — quem espera** | A própria trava segura fd 9, espera `flock`, lança o comando em background sem esse fd e faz `wait`. TERM/INT/HUP acionam `_parar`, que procura container pelo rótulo, solicita parada e sai 143. (`bin/trava_teste.sh:96–103`; `bin/trava_teste.sh:159–171`.) | Separa esperador e job, acrescenta registros, arquivo de rc e superação de jobs ainda em fila. Para sombra, destacar apenas `trancar_sombra` não transfere a continuação: a função atualmente retorna para o restante do script executar. (`contrato.txt:2`; `bin/sombra_trava.sh:21–39`; `bin/sombra.sh:438–461`.) |
| **3 — falha/limite** | A trava já retorna 75 por timeout, falha de `docker ps`, dono vivo sem posse, falha ao parar órfão e suite sem rótulo persistente. Já compara PID **e starttime** para validar o dono do container. Não usa pidfile nem arquivo de rc. A sombra retorna **1** quando sua espera falha. (`bin/trava_teste.sh:39–56`; `bin/trava_teste.sh:97–99`; `bin/trava_teste.sh:115–149`; `bin/sombra_trava.sh:28–35`.) | Acrescenta validação/poda dos registros, timeout de nascimento e rc persistido. “Sem pidfile = job não nasceu” não é demonstrado por um timeout; o protocolo de nascimento não foi fornecido. Também falta explicitar se a sombra migra seu timeout de 1 para 75. (`contrato.txt:3`; implementação [nao verificado].) |
| **4 — medida** | A trava de testes escreve `.quem`; a sombra escreve `sombra_espera.jsonl`, com leitor próprio de esperas longas. (`bin/trava_teste.sh:102–103`; `bin/sombra_trava.sh:30–31`; `bin/sombra_trava.sh:42–60`.) | Introduz journal de eventos, listagem e mensagem de RELATO no stderr. Os valores esperados “0” são metas do contrato, sem medição apresentada. Destino da mensagem quando o esperador já morreu [nao verificado]. (`contrato.txt:4`.) |
| **5 — quem desliga** | Hoje sinais comuns na trava param o filho; a aquisição seguinte também para containers rotulados de donos mortos. A sombra não tem mecanismo equivalente de rótulo e `_parar`. (`bin/trava_teste.sh:120–144`; `bin/trava_teste.sh:159–168`; `bin/sombra_trava.sh:21–39`; `bin/sombra.sh:182–192`.) | Introduz cancelamento por obra. “Job só morre por cancela-obra” precisa ser conciliado com o TERM por superação da linha 2 e com a recuperação de órfãos existente. Commit exato de reversão, drenagem dos jobs destacados e tratamento dos novos arquivos no rollback [nao verificado]. (`contrato.txt:2,5`.) |

O bloqueio novo também precisa definir sua relação com o disjuntor desligado e com exceções: hoje a chave pula `pretool`, e exceções fora de B3 liberam a ferramenta. (`bin/disjuntor.py:1486–1488`; `bin/disjuntor.py:1496–1506`; política futura [nao verificado].)

**2. Quem escreve e quem lê**

| Campo/arquivo | Escritores e leitores atuais | Responsabilidade nova |
|---|---|---|
| **`<trava>.fila/`** | Não há acesso a esse registro nos fluxos atuais de aquisição de testes ou sombra. (`bin/trava_teste.sh:85–171`; `bin/sombra_trava.sh:21–39`.) | Job registra identidade/estado; entrada nova consulta e supera; cancelador consulta; `--fila` lista e leitores podam mortos. Nomes dos arquivos, publicação atômica, sincronização e remoção terminal [nao verificado]. (`contrato.txt:1–3`.) |
| **Arquivo de rc** | O rc vem do `wait`; não há arquivo intermediário. (`bin/trava_teste.sh:169–171`.) | Job escreve, esperador lê e devolve. Caminho, exclusividade por job, publicação atômica e ordenação entre rc, saída final e remoção do registro [nao verificado]. (`contrato.txt:2–3`.) |
| **`<trava>.fila.jsonl`** | Os fluxos atuais não escrevem esse journal. (`bin/trava_teste.sh:96–171`; `bin/sombra_trava.sh:21–39`.) | Eventos são escritos pelo mecanismo novo; cancelamento inclui quem/motivo. Não há leitor do journal nomeado no contrato — `--fila` não é explicitamente definido como leitor dele. Escritor de cada transição e garantia contra eventos duplicados/perdidos [nao verificado]. (`contrato.txt:4–5`.) |
| **`.quem`** | Escrito após aquisição e no EXIT. Lido por `_quem` e pela mensagem de timeout. `_quem` consulta primeiro o flock: se estiver preso e o texto disser livre, responde “não sei”. (`bin/trava_teste.sh:59–81`; `bin/trava_teste.sh:98`; `bin/trava_teste.sh:102–103`.) | O contrato não define seu novo escritor/formato. Se ficar com o esperador, o EXIT dele poderá publicar “livre” com job vivo; a defesa atual de `_quem` evita aceitar esse texto enquanto o flock continuar preso. (`contrato.txt:2`; `bin/trava_teste.sh:61–66`; `bin/trava_teste.sh:103`; migração [nao verificado].) |
| **Rótulo do container** | Chave derivada do **basename da trava**; valor `$$-starttime`. Inserido apenas quando o comando começa por `docker run`. Lido por `_rotulados`, `_quem`, recuperação de órfãos e `_parar`; `_sem_rotulo` procura suites sem rótulo de trava alguma. (`bin/trava_teste.sh:37`; `bin/trava_teste.sh:51–56`; `bin/trava_teste.sh:71–78`; `bin/trava_teste.sh:101`; `bin/trava_teste.sh:109–156`; `bin/trava_teste.sh:162–163`.) | A identidade precisa representar o **job destacado**. Se conservar a identidade do esperador, sua morte fará o dono parecer morto. Para sombra, o vínculo precisa ser criado nos containers hoje sem esse rótulo. (`contrato.txt:2,5`; `bin/trava_teste.sh:47–49`; `bin/sombra.sh:183–191`; implementação [nao verificado].) |
| **rc e stdout/stderr do chamador** | Saída do filho segue pelos descritores herdados; o processo espera seu rc. (`bin/trava_teste.sh:169–171`.) | O esperador passa a retransmitir. Precisa entregar toda a saída antes de devolver o rc e suportar desaparecimento do consumidor sem matar o job. Esse protocolo [nao verificado]. (`contrato.txt:2`.) |

Os consumidores concretos de saída e rc são:

| Consumidor | Leitura e dependência |
|---|---|
| **Suite** | `exec` preserva a interface de processo, saída e rc da trava. (`bin/suite.sh:96–103`.) |
| **Régua** | Grava log, captura rc, rejeita códigos fora de 0/1 sem carimbo e julga log+rc. (`bin/regua.sh:165–180`.) |
| **Pre-push** | Captura ambos os fluxos em arquivo, filtra **depois** da chamada e devolve `_rc`; esse trecho não exige retransmissão ao vivo. (`bin/pre-push.sh:142–157`.) |
| **Vigia da árvore** | Nos três runs, usa arquivo de saída mais rc para decidir publicação. (`bin/vigia_arvore.sh:83–100`; `bin/vigia_arvore.sh:113–119`; `bin/vigia_arvore.sh:132–138`.) |
| **Régua relógio** | Consome fluxo por `tee`/`grep`, captura `PIPESTATUS[0]` e depois julga log+rc. Latência máxima exigida [nao verificado]. (`bin/regua_relogio.sh:51–62`.) |
| **Régua calendário** | Captura arquivo e rc por rodada; só compara resultados após veredito válido. (`bin/regua_calendario.sh:46–62`.) |
| **Isolamento** | Preserva somente 75; para os demais códigos decide pelo último `OK`, podendo perder um 143. (`bin/isolamento.sh:33–38`.) |
| **Placar** | Extrai contador por `sed \| head -1`, sem usar o rc; ausência vira `nao_medido`. O consumidor pode fechar antes do fim do produtor. (`bin/placar_code.sh:125–133`.) |
| **Molde fatia** | Distingue 75 e depois consulta `OK` nos arquivos. (`bin/molde_fatia/rodar.sh:30–36`; `bin/molde_fatia/rodar.sh:52–56`.) |
| **Molde autocura** | Distingue 75 e publica fim para os demais retornos. (`bin/molde_autocura/rodar.sh:22–33`.) |
| **`_rodar_testes`** | `subprocess.run` espera, grava saída e julga conteúdo+rc; 75 e resultado indefinido viram exceções próprias. (`app/core/esteira_vigia.py:794–809`.) |
| **Integrador** | Espera a régua; procura, no stdout daquela chamada, o caminho anunciado do log e julga aquele arquivo com o rc. (`app/core/integrador_lote.py:379–392`.) |

Há ainda leitores indiretos que não consultarão automaticamente a fila nova:

- **Posse:** handoff consulta diretamente o flock das duas travas; o disjuntor consulta o flock da sombra; o hook de Stop usa essa decisão para impedir `/clear` durante DIFF/apply. Soltar o fd antes do término altera essas leituras. (`bin/handoff_sessao.sh:223–234`; `bin/disjuntor.py:307–326`; `bin/hook_stop_fila1.py:785–795`.)
- **Métrica antiga da sombra:** `--esperas-longas` lê `sombra_espera.jsonl`; o placar consome a saída e converte `?` em problema. O journal novo não substitui esse formato sem migração. (`bin/sombra_trava.sh:42–60`; `bin/placar_code.sh:155–160`.)
- **Atividade durante execução:** o vigia lê conteúdo e mtime de arquivos de saída para classificar atividade e decidir ações. Acumular a saída até o fim pode alterar essas decisões quando o fluxo passar por esses arquivos; uma exigência universal de stdout em tempo real [nao verificado]. (`app/core/esteira_vigia.py:710–723`; `app/core/esteira_vigia.py:1552–1575`.)

**3. Contraexemplos que o texto ainda permite**

| Sequência | Consequência e fundamento |
|---|---|
| **Mesma obra, mesma volta:** A entra; B entra com o mesmo N. | A regra só elimina voltas **menores**. Ambos podem executar sequencialmente; não existe deduplicação por igualdade prevista. Se executar duas vezes é indesejado, falta uma regra. (`contrato.txt:2`.) |
| **Pedido antigo chega atrasado:** B/X/2 entra e varre a fila; depois A/X/1 finalmente se registra. | A varredura passada não encontra A. Sem máximo persistente por obra e validação na admissão, A ainda pode executar. O comportamento de `--abre-volta` necessário para impedir isso [nao verificado]. (`contrato.txt:1–2`.) |
| **Volta vazia:** A/X/vazia espera; B/X/2 chega. | A é imune à superação por definição. Logo, a proposta não promete limpar todos os pedidos antigos da obra. (`contrato.txt:1–2`.) |
| **PID reciclado:** A morre; seu PID passa a pertencer a outro processo; um leitor encontra o registro antigo. | Conferir starttime deve podar A, sem sinalizar o novo processo. Porém o texto não define nova validação imediatamente antes de TERM ao PGID: conferir, perder o processo e depois sinalizar ainda deixa uma janela. O código atual valida identidade do dono do container, sem protocolo novo de grupos. (`contrato.txt:2–3`; `bin/trava_teste.sh:39–49`; proteção futura [nao verificado].) |
| **Esperador morre antes do pidfile:** lançamento foi iniciado, mas registro/pidfile ainda não foi publicado; esperador morre e há nova tentativa. | O primeiro job pode continuar e a segunda tentativa criar outro. Mesmo com esperador vivo, ultrapassar 30 s sem pidfile não prova ausência do job. Falta protocolo para localizar/conciliar nascimento tardio. (`contrato.txt:2–3`; implementação [nao verificado].) | [nao verificado]
| **Superação no instante do flock:** A consta `fila`; A adquire flock; antes de publicar `rodando`, B decide superá-lo; A inicia Docker. | B pode matar um job já adquirido ou presumir “ainda sem container” quando isso deixou de valer. É necessário ordenar aquisição, estado e autorização de lançamento com a decisão de superação. Hoje há trabalho entre adquirir e lançar, mas nenhum estado de fila. (`contrato.txt:2`; `bin/trava_teste.sh:97–101`; `bin/trava_teste.sh:113–169`; atomicidade futura [nao verificado].) |
| **Duas travas distintas, basenames distintos:** cada uma protege uma suite no mesmo recurso. | Cada trava consulta seus rótulos; a detecção de suites sem rótulo ignora containers rotulados por **qualquer** trava. As duas podem deixar de enxergar a concorrência entre si. Na cópia normal o caminho é fixo; o cenário exige cópias/reapontamento. (`bin/trava_teste.sh:34–37`; `bin/trava_teste.sh:51–56`; `bin/tests/test_trava_teste.sh:202`.) |
| **Duas travas distintas, mesmo basename:** caminhos diferentes produzem a mesma chave. | Seus containers passam a compartilhar o espaço de identificação, podendo gerar 75 por dono vivo de outra trava ou recuperação cruzada de órfão. As filas, derivadas dos caminhos completos, continuam distintas. (`bin/trava_teste.sh:37`; `bin/trava_teste.sh:120–133`; `contrato.txt:2`.) |
| **Ausência transitória de rc:** job está vivo, ainda não publicou rc, e o esperador interpreta ausência como 143. | Produz rc falso e permite ao chamador limpar recursos enquanto o job trabalha. “Sem rc = 143” precisa ser aplicado após constatação de término/morte, não durante polling. Essa ordenação [nao verificado]. (`contrato.txt:2–3`.) | [nao verificado]
| **143 correto, leitor incorreto:** log já contém `OK`; job é interrompido e o esperador devolve 143. | Isolamento pode devolver sucesso, pois só preserva 75. As cadeias dos moldes também ignoram rc da régua e escolhem o `/tmp/regua_*.log` mais novo, podendo aproveitar resultado alheio. (`bin/isolamento.sh:37–38`; `bin/molde_fatia/cadeia.sh:51–55`; `bin/molde_autocura/cadeia.sh:48–50`.) |
| **Registro ilegível ou morto sem nova leitura:** gravação interrompida deixa registro ilegível; ou job morre e ninguém consulta depois. | O ilegível manda fechar em 75, sem recuperação definida; a poda de mortos ocorre apenas “na leitura”, sem garantia de quando acontecerá. Limpeza eventual automática [nao verificado]. (`contrato.txt:3`.) |
| **Cancelamento pede parada, mas Docker falha:** TERM chega; consulta/parada do container falha. | O `_parar` atual ainda sai 143 sem confirmar pista vazia. Portanto 143 não prova que o container terminou; a recuperação seguinte tem verificação mais forte e retorna 75 se o stop falhar. (`bin/trava_teste.sh:159–166`; `bin/trava_teste.sh:129–144`; `contrato.txt:5`.) |

Sobreviver ao esperador, por si só, é o comportamento pretendido; o problema dos cenários acima é sobreviver sem identidade recuperável, sem recursos ou sem consumidor do resultado. (`contrato.txt:2–5`; garantias de recuperação [nao verificado].)

**4. Chamadores que precisam mudar junto**

- **Sombra com fd 8 herdado: quebra se a migração destacar apenas a aquisição.** A variável herdada faz a função retornar imediatamente, sem conferir o descritor. Preservar a variável e perder fd 8 deixa a continuação sem adquirir a trava; remover a herança enquanto o pai segura a trava faz o filho disputar com o pai. (`bin/sombra_trava.sh:22–28`.)

  O par de simulação precisa conservar uma posse e uma identidade ao longo das duas fotos e da comparação: fotos gravam `SOMBRA_TRAVA_ID`; comparação rejeita identidades diferentes. Destacar cada foto com identidade nova também quebra o par. (`bin/simular_folha.sh:76`; `bin/simular_folha.sh:82–85`; `bin/simular_folha.sh:96–101`.)

- **Pre-push: o caminho normal pode manter sua interface, mas a morte do esperador expõe recursos temporários.** O hook apaga o archive no EXIT, enquanto o job monta esse diretório. Se o hook sair enquanto o job ainda espera, o archive pode desaparecer antes do Docker nascer. Relógio e calendário têm o mesmo vínculo entre temporário montado e limpeza no EXIT. (`bin/pre-push.sh:39–40`; `bin/pre-push.sh:144`; `bin/regua_relogio.sh:31–32`; `bin/regua_relogio.sh:54`; `bin/regua_calendario.sh:26–27`; `bin/regua_calendario.sh:48`.)

- **Cron: esperar até o resultado preserva o encadeamento; sobreviver ao shell muda o significado da medição.** O envelope mede fim/rc do comando, remove sua marca no EXIT e publica fim somente em 0/2. A sombra é encadeada por `--refazer && --bloco`: se esse shell morrer durante refazer, o job sobrevivente não executa sozinho a continuação `&& --bloco`. (`bin/cron_run.sh:9–25`; `app/config/crons.py:331–333`.) Cron efetivamente instalado em produção [nao verificado].

- **Integrador e cadeias: vida do teste e posse da árvore precisam continuar coordenadas.** O integrador segura locks externos; `pegar_arvore` usa fd 6/8. Herdá-los pode mantê-los presos depois da morte do chamador; fechá-los pode liberar a árvore enquanto o job ainda a lê. Além disso, a cadeia desfaz arquivos no EXIT e o integrador desfaz aplicações quando recebe `TesteNaoRodou`. (`bin/integrador.sh:32`; `bin/integrador.sh:41–51`; `bin/trava.sh:60–68`; `bin/molde_fatia/cadeia.sh:25–35`; `app/core/integrador_lote.py:653–656`.) Política dos descritores no job destacado [nao verificado].

- **Desligamento da esteira e selos: exigem revisão explícita.** O matador atual sinaliza grupos encontrados pelos detentores do lock da esteira; a nova sessão pode escapar ou continuar descobrível conforme os descritores herdados. O controlador também identifica trabalho pelo grupo. (`app/core/esteira_vigia.py:1026–1044`; `bin/esteira_slots.sh:34–47`; herança futura [nao verificado].) Já o selo de dono contradiz diretamente a sobrevivência proposta, tanto no TERM quanto no KILL; a régua executa esse selo antes da suite. (`bin/tests/test_trava_dono_para_filho.sh:54–80`; `bin/regua.sh:138–143`.)

O selo estático existente continua útil, mas reconhece texto/composição sintática: função Shell contendo `trava_teste.sh` e unidade Python contendo a constante correspondente. Isso não demonstra, sozinho, passagem efetiva pela trava em todos os caminhos; a implementação do novo gate também precisará distinguir execução de mera menção no comando. (`bin/tests/test_trava_teste.sh:83–91`; `bin/tests/test_trava_teste.sh:114–122`; gate futuro [nao verificado].)

## 31/10 21:57 · sessao d596822205ca · F1C4-3-RG-SEM-CELULA-CASO · leitura · origem=Code

**1. O dia entra em `mortos`?**

- **Na cópia atual, não entra por `_indef`.** A ausência da chave faz `_cels_rg_m.get((c.pk, _d))` devolver `None`; o teste de existência falha antes da chamada a `dia_sem_trabalho`. Portanto `_indef` fica vazio para esse chamado de um único dia. Isso não impede entrada pelos outros caminhos abaixo. (`app/ponto/management/commands/reconciliar_grade.py:281-291`)
- **Quanto ao pai de `9651ef38`: o trecho exato e suas linhas são `[nao verificado]`**, pois a cópia não disponibiliza esse histórico. A documentação local descreve a regra anterior como `sem_ancora → mortos → RETRACT_INDEFINIDO`, e o backlog registra especificamente que o dia sem âncora e sem célula “deixou de ser morto”. Isso sustenta o comportamento anterior descrito, mas não comprova sua atribuição à revisão solicitada. (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:4-7`; `app/docs/BACKLOG.md:832`)

**2. Outros caminhos e o retorno de `classificar_falta`**

Há três caminhos além de `_indef`:

- **`avaliado_limpo`:** recebe o dia quando não há `miss`. Porém, no processamento informado, `sem_ancora` adiciona o dia a `indefinidos` e executa `continue` antes dessa avaliação. Logo esse dia não entra por `avaliado_limpo`. (`app/ponto/management/commands/reconciliar_grade.py:139-156`)
- **`_coberto`:** pode incluir o dia diretamente em `mortos`. Exige vínculo `ec`; sem vínculo devolve `False`. Com vínculo, aceita a ausência selecionada por `ausencia_cobre(..., para='cobranca')` se seu tipo não for `falta`, ou uma substituição do titular naquele posto, sobreposta ao dia, em estado `designada`, `em_curso` ou `concluida`. Não exige `CelulaDia`. (`app/ponto/management/commands/reconciliar_grade.py:266-267`; `app/ponto/management/commands/reconciliar_grade.py:391-410`)
- **`_lei_cala_no_dia`:** se a lei calar em todas as datas de `dts`, une `dts` a `mortos`. Para um único dia basta essa consulta devolver `True`. O auxiliar testa `classificar_falta(...)[1] is None`; se a chamada lançar exceção, devolve `False`. (`app/ponto/management/commands/reconciliar_grade.py:290-291`; `app/ponto/marcos.py:166-176`)

**Sem `CelulaDia`, `classificar_falta` não tem retorno único.** A consulta devolve `None` e apenas deixa de disparar a guarda de célula com `trabalha is False`; o restante continua. (`app/escala/servico_jornada.py:262-271`; `app/escala/servico_jornada.py:302-304`)

O resultado depende destes insumos:

- **Ausência:** uma ausência que alcance o dia, não rejeitada e que não cubra apenas parte dele faz retornar `(None, None, None)`. Esse caminho não exige vínculo. (`app/escala/servico_jornada.py:340-345`; `app/ponto/turnos.py:2516-2533`; `app/ponto/turnos.py:2583-2600`)
- **Vínculo/escala:** sem célula, `escala_vigente` consulta os vínculos; `vinculo_do_dia` escolhe entre os que cobrem a data, priorizando `ativa` e `data_inicio`, ou devolve `None`. Ausência de célula não implica ausência de vínculo. (`app/escala/servico_jornada.py:46-58`; `app/escala/alimentacao.py:173-184`; `app/escala/alimentacao.py:253-254`)
- **Turno completo e teto temporal:** turno noturno completo faz calar. Havendo vínculo e tipo de escala, também cala quando existe fim esperado e `now() < fim_turno(...)`. Nessa chamada, `fim_turno` recebe o tipo, sem `ec`; os marcos vêm do template e são encadeados através da meia-noite. (`app/escala/servico_jornada.py:354-378`; `app/escala/servico_jornada.py:89-93`; `app/escala/servico_jornada.py:68-84`)
- **Batidas e marcos esperados:** ultrapassadas as guardas, usa os turnos do núcleo. Sem célula, `ec.marcos_do_dia` recorre ao template; esse método do template não decide trabalho/folga. (`app/escala/servico_jornada.py:390-435`; `app/escala/models.py:1094-1108`; `app/escala/models.py:326-345`)

Exemplos condicionais: com universo de batidas vazio e marcos esperados de entrada/saída, retorna `('E', 'saida_sem_entrada', 'ENTROU')`; sem marcos esperados, retorna `('S', 'orfao_14h', 'SAIU')`. Dia completo pode devolver `(None, None, None)`, e falha ao obter o universo do núcleo também faz calar. Portanto **“sem célula” sozinho não prova nem retratação nem manutenção**. (`app/escala/servico_jornada.py:390-402`; `app/escala/servico_jornada.py:440-480`)

**3. Quem consome o resultado**

| Consumidor | Efeito |
|---|---|
| `RETRACT_INDEFINIDO` | Incrementa se `dts` não vazio estiver contido em `mortos` e `_indef` não vazio, **antes** de consultar `premissa_morta`. No caso sem célula, esse ramo não incrementa. (`app/ponto/management/commands/reconciliar_grade.py:281-298`) |
| `RETRACT` e `retracts` | Só incrementa/adiciona depois de `premissa_morta` autorizar. (`app/ponto/management/commands/reconciliar_grade.py:298-304`) |
| `--apply` | Chama `chamados.reconciliador.retratar`; o conteúdo depende de `_indef`. A chamada ocorre depois da contagem. (`app/ponto/management/commands/reconciliar_grade.py:303-316`) |
| Stdout | Publica os contadores no `PLANO`, o tamanho de `retracts` e até 40 candidatos. (`app/ponto/management/commands/reconciliar_grade.py:318-325`) |
| Tripwire | `divergencia_grade_cartorio = len(creates) + len(retracts)`; não soma `RETRACT_INDEFINIDO` separadamente. (`app/ponto/management/commands/reconciliar_grade.py:337-342`) |

O contador representa candidatos: `retratar` pode devolver `False`, e esse retorno não é usado pelo comando para desfazer a contagem. (`app/chamados/reconciliador.py:520-526`; `app/chamados/reconciliador.py:546-548`; `app/ponto/management/commands/reconciliar_grade.py:303-316`)

Leitores/configuração encontrados:

- **Cron:** configuração às `06:20`, empresas `2,3,4`, sem `--apply`; stdout/stderr vão para `logs/reconciliar_grade.log`. (`app/config/crons.py:443`; `app/config/crons.py:32`; `app/config/crons.py:1801-1802`)
- **Vigia/placar:** extrai o último `divergencia_grade_cartorio` desse log, exige arquivo modificado hoje e publica `divergencia_grade_x_cartorio`. Esse valor não integra o `TOTAL` do Code. (`bin/placar_code.sh:528-532`)
- **Vigia de execução:** o envelope acompanha o código de saída e aciona `cron_quebrado` nas falhas; não interpreta esses contadores. (`bin/cron_run.sh:11-13`; `bin/cron_run.sh:27-38`)
- **Selos:** validam o contrato do tripwire, presença/ausência de `RETRACT_INDEFINIDO` e ausência de `--apply` no cron. (`app/ponto/tests/test_reconciliar_grade_sombra.py:71-88`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:61-74`; `app/chamados/tests/test_hx_um_emissor.py:5-9`)

Execução efetiva e valores em produção: `[nao verificado]`.

**4. Testes do caso específico** [nao verificado]

**Não localizei teste que execute esse comando com `sem_ancora=True` e ausência de `CelulaDia` para o dia do chamado.** A lacuna está explicitamente registrada como aberta. (`app/docs/BACKLOG.md:832`)

Os testes próximos cobrem situações diferentes: [nao verificado]

- `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py` simula grade indefinida, mas sempre cria `CelulaDia(trabalha=None)`. O caso chamado “sem ata” passa `ata=None`; a célula continua existindo. (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:28-34`; `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:55-70`)
- `test_a2_marco_morto.py` também cria a célula antes de executar com `apply=True` e verificar que `retratar` não foi chamado. (`app/ponto/tests/test_a2_marco_morto.py:46-55`)
- `test_reconciliar_sem_ancora_visivel.py` só inspeciona o código procurando contador/lista; não monta o cenário. (`app/ponto/tests/test_reconciliar_sem_ancora_visivel.py:11-18`)
- Em `app/chamados/tests`, o selo do comando verifica os argumentos do cron; o teste de recuperação da data cria célula existente e chama `_cham_dts`, sem exercitar esse julgamento em `handle`. (`app/chamados/tests/test_hx_um_emissor.py:5-9`; `app/chamados/tests/test_c5_emissores.py:39-51`)

## 31/10 21:56 · sessao 3ff5b8f0a0eb · - · - · origem=Ronald (tmux)

Não localizei teste que exercite **`sem_ancora=True` com ausência da chave `(colab, dia)` em `celulas_do_periodo`**. A lacuna está registrada explicitamente como obra aberta em `app/docs/BACKLOG.md:832`: “dia sem ancora e sem celula deixou de ser morto sem caso na bateria”.

Os testes próximos usam **célula existente**: [nao verificado]

- `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py` simula grade indefinida (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:28`–34), mas `_rodar` sempre cria `CelulaDia(trabalha=None, ata=ata)` (`:55`) antes de executar `handle` (`:58`). Os testes cobrem ata de trabalho (`:61`–65), `ata=None` (`:67`–70) e ata de folga (`:72`–74). **“Sem ata” não significa “sem CelulaDia”** (`:55`, `:68`).
- `test_cobranca_em_dia_indefinido_fica_viva_limbo_cobra` também simula `sem_ancora=True` (`app/ponto/tests/test_a2_marco_morto.py:26`–32), mas cria expressamente `CelulaDia(trabalha=None)` (`:46`). Com `apply=True` (`:50`), verifica que `retratar` nem foi chamado (`:52`) e que o chamado permanece vivo (`:54`–55).
- `test_plano_declara_sem_ancora` apenas inspeciona o texto do comando e exige `SEM_ANCORA`/`sem_ancora_dias`; não monta um caso com chamado e ausência de célula (`app/ponto/tests/test_reconciliar_sem_ancora_visivel.py:11`–18).

Em `app/chamados/tests`, os casos encontrados ligados ao comando não exercitam esse cenário:

- O teste de `_cham_dts` cria célula com `trabalha=True` e verifica a data recuperada do vínculo do chamado com ela (`app/chamados/tests/test_c5_emissores.py:39`–51).
- O selo do cron verifica ausência de `--apply`, sem executar o julgamento de dia indefinido (`app/chamados/tests/test_hx_um_emissor.py:5`–9).
- Os testes das portas chamam auxiliares `_relavrar_orfa` e `_disputa`, não `Command.handle` (`app/chamados/tests/test_porta_chamado_e2.py:62`–72; `app/chamados/tests/test_porta_chamado_e3.py:92`–94).

Há teste de **ausência de célula em outro escopo**: `GateCelulaTest` deixa `d_sem` sem criação correspondente (`app/ponto/tests/test_t31_gate_celula.py:16`–25) e verifica `celula_veta_cobranca(..., d_sem) is None` (`:32`–35). Ele não executa `reconciliar_grade.handle` nem testa `_indef`/`mortos` (`:32`–35).

O comentário dos testes descreve a regra anterior como `sem_ancora -> mortos -> RETRACT_INDEFINIDO` (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:4`–7); a correspondência byte a byte com o **pai de `9651ef38`** continua `[nao verificado]`, pois não encontrei histórico Git ou reprodução desse pai na cópia.

## 31/10 21:56 · sessao c686af4f9bc0 · - · - · origem=Ronald (tmux)

Não encontrei a versão histórica nem referência a `9651ef38` na cópia; o conteúdo exato e as linhas do pai de `9651ef38` ficam **[nao verificado]**.

Há duas evidências locais sobre a mudança:

- `app/docs/BACKLOG.md:832`: registra explicitamente **“reconciliar_grade.py:281-282, dia sem ancora e sem celula deixou de ser morto sem caso na bateria”**.
- `app/ponto/management/commands/reconciliar_grade.py:277`: comentário histórico informa **“Ate aqui `sem_ancora` ia inteiro para `mortos`”**.

Na cópia atual, `_indef` exige célula existente antes de perguntar ao juiz: `app/ponto/management/commands/reconciliar_grade.py:281-282`. Sem chave no mapa, `.get(...)` retorna `None`, o primeiro operando falha e o dia não entra em `_indef`; portanto não entra em `mortos` **por esse caminho** (`app/ponto/management/commands/reconciliar_grade.py:283`).

O teste citado pelo comentário não cobre ausência de célula: `_rodar` sempre cria uma `CelulaDia`, inclusive quando `ata=None` (`app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:55`, `:67-68`).

## 31/10 21:56 · sessao 31b27f047daf · - · - · origem=Ronald (tmux)

Leitores e fluxo do resultado na cópia:

- `RETRACT_INDEFINIDO` incrementa quando todas as datas estão em `mortos` e `_indef` é não vazio, **antes** do veto por `premissa_morta`; portanto pode subir sem gerar `RETRACT`. `RETRACT` e `retracts.append` só acontecem após esse veto. (`app/ponto/management/commands/reconciliar_grade.py:292–304`)
- Com `--apply`, chama `chamados.reconciliador.retratar`; a mensagem depende de `_indef`. (`app/ponto/management/commands/reconciliar_grade.py:305–316`)
- O stdout imprime todos os contadores em `PLANO`, o tamanho de `retracts` e até 40 itens da lista. (`app/ponto/management/commands/reconciliar_grade.py:318–325`)
- O tripwire usa **`len(creates) + len(retracts)`**, não soma diretamente `RETRACT_INDEFINIDO`; imprime também `create`, `retract`, `orfa_dry`, esperado e dono. (`app/ponto/management/commands/reconciliar_grade.py:337–342`)
- O cron configurado roda às **06:20**, com `--empresa 2,3,4`, sem `--apply`; o log padrão é `reconciliar_grade.log`, com stdout/stderr redirecionados pelo renderizador. (`app/config/crons.py:443`, `app/config/crons.py:32`, `app/config/crons.py:1801–1802`)
- O leitor do contador é `bin/placar_code.sh`: extrai o último `divergencia_grade_cartorio` do log, exige log modificado hoje e publica `divergencia_grade_x_cartorio`; esse valor não participa do `TOTAL` do Code. (`bin/placar_code.sh:528–532`)
- O envelope do cron acompanha o **exit code**, não interpreta `RETRACT`/tripwire; registra execução e aciona `cron_quebrado` nas falhas. (`bin/cron_run.sh:11–13`, `bin/cron_run.sh:27–38`)
- Selos que leem o stdout: `test_reconciliar_grade_sombra` verifica existência e contrato completo do tripwire; `test_f1c4_3_reconciliar_grade_pergunta_ao_juiz` verifica ausência/presença de `RETRACT_INDEFINIDO` e divergência zero. (`app/ponto/tests/test_reconciliar_grade_sombra.py:71–88`, `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:61–74`)
- O selo do cron cobra ausência de `--apply`. (`app/chamados/tests/test_hx_um_emissor.py:5–9`)

Execução real do cron, conteúdo de log e valor do contador em produção: `[nao verificado]`.

## 31/10 21:56 · sessao 8dc87397ccfa · - · - · origem=Ronald (tmux)

Leitura concluída sem alterações. Não encontrei `AGENTS.md` no resultado de `rg --files -g AGENTS.md`; instruções externas a esse conjunto [não verificado].

**Chamadores e consumidores de rc/stdout**

| Chamador | Fluxo atual e dependência |
|---|---|
| `suite.sh` | Faz `exec env ESTEIRA_QUEM=... bash trava_teste.sh`, herdando stdout/stderr e devolvendo o rc pelo próprio processo: `bin/suite.sh:96`. Não declara obra/volta nesse ponto: `bin/suite.sh:96`. |
| Régua | Suite escreve em `$LOG`; lê `$?`; só permite 0/1 julgar e devolve outros códigos sem carimbo; juiz lê log+rc: `bin/regua.sh:165`, `bin/regua.sh:170`, `bin/regua.sh:175`, `bin/regua.sh:180`. |
| Pre-push | Captura stdout/stderr em arquivo; guarda rc verdadeiro; imprime filtro só depois de terminar; devolve `_rc`: `bin/pre-push.sh:142`, `bin/pre-push.sh:145`, `bin/pre-push.sh:146`, `bin/pre-push.sh:157`. Portanto não exige stdout ao vivo nesse trecho. |
| Vigia da árvore | Captura stdout/stderr em log; lê rc e passa ambos ao juiz nos três runs: `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:86`, `bin/vigia_arvore.sh:99`, `bin/vigia_arvore.sh:113`, `bin/vigia_arvore.sh:132`, `bin/vigia_arvore.sh:137`. |
| Régua relógio | Pipeline `trava ... 2>&1 | tee ... | grep ...`; captura `PIPESTATUS[0]` e julga log+rc: `bin/regua_relogio.sh:51`, `bin/regua_relogio.sh:56`, `bin/regua_relogio.sh:57`, `bin/regua_relogio.sh:62`. É leitor de fluxo incremental via `tee`/filtro; necessidade operacional de latência específica [não verificado]. |
| Régua calendário | Cada run escreve log; lê rc; 75 retorna 75; restantes passam pelo juiz: `bin/regua_calendario.sh:46`, `bin/regua_calendario.sh:50`, `bin/regua_calendario.sh:51`, `bin/regua_calendario.sh:56`. |
| Isolamento | Só preserva 75; para qualquer outro rc decide pelo último `OK` do log: `bin/isolamento.sh:33`, `bin/isolamento.sh:37`, `bin/isolamento.sh:38`. Contraexemplo: log contém `OK`, job morre 143; `_iso_roda` pode devolver 0 e chamador declarar “verdes sem a fatia”: `bin/isolamento.sh:49`, `bin/isolamento.sh:50`. |
| Placar | `trava ... | sed ... | head -1` dentro de substituição; ignora rc e lê contador; ausente vira `nao_medido`: `bin/placar_code.sh:125`, `bin/placar_code.sh:132`, `bin/placar_code.sh:133`. `head` pode encerrar leitura antes do fim; preservação do job exige relay tolerar consumidor fechado [não verificado quanto à implementação proposta]. |
| Molde fatia | Chamadas em `bin/molde_fatia/rodar.sh:30` e `bin/molde_fatia/rodar.sh:44`; só distingue 75, depois julga `OK` no arquivo: `bin/molde_fatia/rodar.sh:34`, `bin/molde_fatia/rodar.sh:35`, `bin/molde_fatia/rodar.sh:53`, `bin/molde_fatia/rodar.sh:55`, `bin/molde_fatia/rodar.sh:56`. |
| Molde autocura | Chamada em `bin/molde_autocura/rodar.sh:22`; só distingue 75; publica fim para outros rc: `bin/molde_autocura/rodar.sh:28`, `bin/molde_autocura/rodar.sh:32`, `bin/molde_autocura/rodar.sh:33`. |
| `_rodar_testes` | `subprocess.run` espera processo, escreve saída em arquivo e usa rc+conteúdo no juiz: `app/core/esteira_vigia.py:794`, `app/core/esteira_vigia.py:802`, `app/core/esteira_vigia.py:803`, `app/core/esteira_vigia.py:805`, `app/core/esteira_vigia.py:807`. |
| Integrador | Wrapper herda locks fd7/fd8 e chama Python: `bin/integrador.sh:41`, `bin/integrador.sh:44`, `bin/integrador.sh:51`. `_suite` espera régua, lê rc, busca caminho anunciado no trecho novo de stdout e julga log específico: `app/core/integrador_lote.py:379`, `app/core/integrador_lote.py:380`, `app/core/integrador_lote.py:385`, `app/core/integrador_lote.py:388`, `app/core/integrador_lote.py:390`. Não usa mais “log mais novo”. |
| Cron | Geração envelopa comando em `cron_run.sh` e redireciona saída para log: `app/config/crons.py:1431`, `app/config/crons.py:1791`. Runner espera `bash -c`, mede duração/rc, grava fim só em 0/2 e alarma outros rc: `bin/cron_run.sh:11`, `bin/cron_run.sh:13`, `bin/cron_run.sh:22`, `bin/cron_run.sh:32`. Régua relógio/calendário no crontab instalado [não verificado]; entrada de vigia existe em `app/config/crons.py:277`. |

**Quebras concretas/condicionais por destaque**

1. **Selo atual quebra diretamente.** Régua executa todos os selos de host antes da suite e aborta se qualquer falhar: `bin/regua.sh:138`, `bin/regua.sh:140`. Selo exige que TERM no PID da chamada mate container e produza rc143: `bin/tests/test_trava_dono_para_filho.sh:54`, `bin/tests/test_trava_dono_para_filho.sh:60`, `bin/tests/test_trava_dono_para_filho.sh:62`. Também exige KILL da chamada deixar ORFAO: `bin/tests/test_trava_dono_para_filho.sh:68`, `bin/tests/test_trava_dono_para_filho.sh:74`, `bin/tests/test_trava_dono_para_filho.sh:77`. Contrato faz esse PID virar esperador cuja morte preserva job; selo precisa mudar junto, ou régua fica vermelha antes de testar.

2. **Recursos temporários seguem pertencendo ao chamador.** Pre-push cria archive e registra `rm -rf` no EXIT: `bin/pre-push.sh:39`, `bin/pre-push.sh:40`; job monta aquele archive: `bin/pre-push.sh:144`. Se esperador morrer antes da aquisição, hook sair pelo rc/sinal e executar EXIT, archive pode sumir antes do `docker run`; execução desse sinal/EXIT específico em produção [não verificado]. Mesmo problema estrutural para calendário com `$TMP/app` (`bin/regua_calendario.sh:26`, `bin/regua_calendario.sh:27`, `bin/regua_calendario.sh:48`), relógio com shim em `$TMP` (`bin/regua_relogio.sh:31`, `bin/regua_relogio.sh:32`, `bin/regua_relogio.sh:54`) e fallback efêmero do vigia (`bin/vigia_arvore.sh:23`, `bin/vigia_arvore.sh:58`, `bin/vigia_arvore.sh:85`).

3. **Locks extras não podem ser herdados sem decisão.** Integrador segura fd7 `/tmp/regua.lock`, fd8 temporário de push e locks da árvore via `trava.sh`: `bin/integrador.sh:32`, `bin/integrador.sh:41`, `bin/integrador.sh:51`; árvore é fd8 e raia fd6: `bin/trava.sh:60`, `bin/trava.sh:64`. Destaque preservando FDs pode manter locks depois do dono morrer; fechando-os pode deixar job lendo árvore já livre para outro escritor. Política do contrato para esses FDs [não verificado].

4. **Restauração da árvore é ligada ao chamador.** Cadeia instala trap EXIT que faz checkout/remove arquivos e recarrega aplicação: `bin/molde_fatia/cadeia.sh:25`, `bin/molde_fatia/cadeia.sh:27`, `bin/molde_fatia/cadeia.sh:29`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:35`; régua é chamada depois: `bin/molde_fatia/cadeia.sh:51`. Se cadeia sai enquanto teste destacado continua, a árvore montada pode mudar sob o job; proteção contratual [não verificado].

5. **Matador da esteira não alcança nova sessão automaticamente.** Vigia descobre grupos dos detentores do lock e manda TERM/KILL para eles: `app/core/esteira_vigia.py:1026`, `app/core/esteira_vigia.py:1031`, `app/core/esteira_vigia.py:1034`, `app/core/esteira_vigia.py:1037`. Job criado em nova sessão/grupo pode sobreviver a esse desligamento; se herdar o lock, o `fuser` pode descobri-lo também. FDs herdados pelo contrato [não verificado].

6. **Unidade do integrador já permite descendentes sobreviverem ao principal.** `KillMode=process`: `bin/hasner-integrador.service:10`. Portanto `setsid` não resolve por si só protocolo de fim, recursos e locks; serviço instalado [não verificado].

7. **Consumidores antigos ainda aceitam resultado textual sem rc.** Cadeias ignoram rc da régua e escolhem `/tmp/regua_*.log` mais novo, verificando `Ran`/`OK`: `bin/molde_fatia/cadeia.sh:51`, `bin/molde_fatia/cadeia.sh:53`, `bin/molde_fatia/cadeia.sh:54`, `bin/molde_fatia/cadeia.sh:55`; autocura idem `bin/molde_autocura/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:50`. Rc143 correto do relay não basta para esses leitores.

**Leitores complementares**

- Handoff mede somente `flock`, não `.quem` nem fila: `bin/handoff_sessao.sh:223`, `bin/handoff_sessao.sh:230`. Fila existente com lock momentaneamente livre pode aparecer LIVRE; adaptação prevista [não verificado].
- Placar lê contador de esperas longas da sombra: `bin/placar_code.sh:155`, `bin/placar_code.sh:158`.
- Leitor direto externo de `.quem` não foi encontrado; na trava ele é lido em `bin/trava_teste.sh:63`, `bin/trava_teste.sh:80`, `bin/trava_teste.sh:98` e escrito em `bin/trava_teste.sh:102`, `bin/trava_teste.sh:103`. Escritor deve passar ao job para o esperador morto não publicar “livre” enquanto ele roda.

## 31/10 21:56 · sessao 7f44a2164616 · - · - · origem=Ronald (tmux)

Achados da sombra, somente por leitura:

- `trancar_sombra` é uma função carregada por `source`; ela abre fd 8 no próprio chamador, espera `flock`, exporta `SOMBRA_TRAVA_HERDADA=1` e `SOMBRA_TRAVA_ID`, e retorna 0 para o chamador executar a continuação. Ela não recebe um comando que possa destacar: `bin/sombra_trava.sh:21-39`. Os chamadores carregam a função em `bin/sombra.sh:87`, `bin/simular_folha.sh:45`, `bin/diff_janela_he_total.sh:17`.
- O caminho herdado retorna 0 apenas pela variável `SOMBRA_TRAVA_HERDADA=1`, sem conferir fd 8, identidade ou posse: `bin/sombra_trava.sh:22`. Logo, uma adaptação que preserve a variável mas feche/perca fd 8 executaria continuação sem adquirir a trava; uma adaptação que remova a herança tentaria adquirir novamente enquanto o pai ainda detém a trava: consequências condicionais dessa lógica, `bin/sombra_trava.sh:22-28`.
- Destacar somente a função que adquire a trava não basta: os trabalhos protegidos permanecem nas continuações `bin/sombra.sh:438-450`, `bin/sombra.sh:451`, `bin/sombra.sh:456-461`, `bin/sombra.sh:487-492`, `bin/simular_folha.sh:67-77`, `bin/simular_folha.sh:96-101`, `bin/diff_janela_he_total.sh:18-42`. O contrato não especifica como transferir essas continuações para o job destacado: `[nao verificado]`.

Dependências de herança e identidade:

- `simular_folha par` adquire uma trava e executa, sequencialmente, as duas fotos e a comparação; cada falha de foto interrompe o par e o processo retorna o rc da comparação: `bin/simular_folha.sh:96-101`.
- Cada foto escreve `${SOMBRA_TRAVA_ID}` no arquivo `<rotulo>.trava` após sucesso do container: `bin/simular_folha.sh:73-77`. A comparação lê os dois arquivos e sai 5 se os IDs divergem ou são `?`: `bin/simular_folha.sh:82-85`. Duas fotos destacadas independentemente, com IDs novos, quebrariam esse contrato mesmo se executadas sem concorrência: consequência de `bin/sombra_trava.sh:37-38` e `bin/simular_folha.sh:82-85`.
- `diff_janela_he_total` também grava e imprime o ID da trava: `bin/diff_janela_he_total.sh:41-42`; não localizei outro leitor desse arquivo na árvore pesquisada `[nao verificado]`.
- `sombra.sh --com-a-sombra` executa o comando arbitrário e devolve seu rc explicitamente: `bin/sombra.sh:452-461`. `sombra.sh --rodar` passa saída por `grep` e `tee`, escreve `logs/<rotulo>.out`, depois imprime o caminho: `bin/sombra.sh:490-493`. Seu último `echo` pode encobrir o rc da pipeline atual: `bin/sombra.sh:492-493`.

Cancelamento e containers:

- A implementação atual da sombra não associa seus containers a um rótulo de dono nem define `_parar`: receitas `docker run` em `bin/sombra.sh:182-192`, `bin/simular_folha.sh:47-57`, `bin/diff_janela_he_total.sh:30-40`; a trava atual só abre fd/espera/retorna: `bin/sombra_trava.sh:21-39`.
- Portanto, “sombra idem” do contrato 5 exige nova associação dos containers ao job e parada efetiva deles; matar apenas o grupo do shell não está demonstrado como suficiente `[nao verificado]`. A distinção de parentesco do container com o daemon já está documentada e testada para a outra trava em `bin/tests/test_trava_dono_para_filho.sh:4-14`.
- O bloco executa múltiplos comandos dentro de um container, escreve rc/duração individual em `resultado.tsv`, e só depois consolida e carimba o resultado: `bin/sombra.sh:351-407`. Um cancelamento antes da consolidação deixa esse trabalho incompleto por esse fluxo: `bin/sombra.sh:385-407`.

Leitores que precisam preservar:

- A trava atual escreve `logs/sombra_espera.jsonl` com `ts`, `quem`, `espera_s`, `ok`, `timeout_s`, `pid`: `bin/sombra_trava.sh:15-18`, `bin/sombra_trava.sh:30-31`. `--esperas-longas` lê esse formato, conta ilegíveis como problema e imprime `?` se o arquivo não existe: `bin/sombra_trava.sh:42-60`.
- `placar_code.sh` consome essa saída, extrai o número ou `?`, e converte `?` em 1: `bin/placar_code.sh:155-160`. Substituir esse log por `<trava>.fila.jsonl` sem manter/migrar o leitor altera a medição: consequência desses sítios.
- `handoff_sessao.sh` pergunta diretamente ao `flock` das duas travas e imprime LIVRE/TOMADA: `bin/handoff_sessao.sh:223-234`.
- `disjuntor.diff_apply_em_curso` pergunta ao flock da sombra; inexistente = livre; erro = em curso: `bin/disjuntor.py:307-326`. Essa decisão é lida em `bin/disjuntor.py:1186`, `bin/disjuntor.py:1211`, `bin/disjuntor.py:1435`.
- O hook de Stop usa o mesmo leitor para impedir `/clear` durante DIFF/apply: `bin/hook_stop_fila1.py:785-795`. Perder fd 8 antes do fim do trabalho faria esses leitores observarem uma trava livre durante trabalho ainda ativo: consequência dos sítios acima.

Cron e destacamento:

- O cron da sombra encadeia `--refazer && --bloco`: `app/config/crons.py:331-333`. Um esperador que continue vivo, espere o trabalho e retorne o mesmo rc preserva esse encadeamento; a implementação futura disso é `[nao verificado]`.
- O envelope cron cria a marca de “em curso”, remove-a no EXIT, espera `bash -c`, e mede seu fim/rc: `bin/cron_run.sh:9-13`; publica arquivo de fim apenas para rc 0 ou 2: `bin/cron_run.sh:22-25`; abre/fecha alarme pelos mesmos resultados: `bin/cron_run.sh:29-35`.
- Se o esperador/shell morrer e o job de `--refazer` sobreviver, o trabalho prossegue fora da duração/marca do envelope; o `&& --bloco` do shell morto não se executa: consequência de `app/config/crons.py:332`, `bin/cron_run.sh:10-13` e da sobrevivência proposta no contrato 2. O estado real do cron de produção é `[nao verificado]`.

Não encontrei AGENTS.md nos ancestrais consultados nem na listagem da cópia; presença fora desses locais `[nao verificado]`.

## 31/10 21:55 · sessao 901e2a5609d1 · - · - · origem=Ronald (tmux)

Achados só por leitura; não executei selos, pois eles criam/removem arquivos (`bin/tests/test_trava_teste.sh:156-182`, `bin/tests/test_trava_dono_para_filho.sh:33-43`).

- **PreToolUse atual:** já extrai o texto de todo Bash em `bin/disjuntor.py:1430`, mas retorna imediatamente para sessão sem `agent_id`, ferramenta diferente de Agent e comando sem `codex_pergunta` (`bin/disjuntor.py:1431-1432`). Os ramos posteriores só fazem B1, Codex/B3/B4-CODEX e Agent/B4; não há regra para Docker/suite (`bin/disjuntor.py:1434-1475`). O teste atual inclusive declara que Bash da sessão principal passa (`bin/tests/test_disjuntor.sh:265`).
- **Mudança necessária nas linhas 1/3 do contrato:** a checagem suite deve executar antes daquele retorno; apenas acrescentá-la depois deixa escapar a sessão principal (`bin/disjuntor.py:1431-1432`). O matcher já é vazio, portanto a configuração chama o hook sem selecionar somente Bash (`.claude/settings.json:62-68`, `bin/disjuntor_settings.json:5-11`). A configuração aponta para `/home/ronald/saas-hasner`, não para esta cópia; qual código foi efetivamente carregado na produção [não verificado].
- **Desligamento e falhas:** `guarda_chave` executa antes de tudo; com chave presente, `pretool` inteiro é pulado (`bin/disjuntor.py:1486-1488`). Exceção fora do ramo B3 libera a ferramenta (`bin/disjuntor.py:1496-1506`). A proposta não define como o novo bloqueio interage com esses caminhos [não verificado].
- **Leitor indireto da sombra:** `diff_apply_em_curso` lê `logs/sombra.lock` e tenta `flock` compartilhado, sem ler `.quem` (`bin/disjuntor.py:66`, `bin/disjuntor.py:307-326`); B1 só age quando esse leitor diz livre (`bin/disjuntor.py:1434-1439`). Se a implementação destacada soltar a trava antes do trabalho efetivo acabar, esse leitor passa a considerar o DIFF terminado — inferência de `bin/disjuntor.py:318-324`.

O selo estático já trabalha por comando lógico Shell e por unidade AST Python; ele é executado pela régua, que percorre todos os `bin/tests/test_*.sh` e interrompe ao primeiro erro (`bin/tests/test_trava_teste.sh:48-62`, `bin/tests/test_trava_teste.sh:84-122`, `bin/regua.sh:138-143`). O universo inclui `.sh` recursivamente em `bin/` e Python em `bin/`/`app/`, excluindo diretórios `tests` e arquivos `test_*`; outras extensões e comandos interativos não entram nesse scanner (`bin/tests/test_trava_teste.sh:125-133`).

Limites demonstráveis por inspeção do selo, sem afirmar exploração real:

- Em Shell, uma função é considerada travada se seu corpo **contém o texto** `trava_teste.sh`; uma chamada cujo primeiro token seja essa função é aprovada (`bin/tests/test_trava_teste.sh:83-91`). Não demonstra que todos os caminhos da função executam a trava.
- Em Python, basta uma constante terminando em `trava_teste.sh` na mesma unidade da constante com `manage.py test`; não verifica fluxo entre elas (`bin/tests/test_trava_teste.sh:114-122`).
- O reconhecedor exige o literal `manage.py test` com um espaço; composição dinâmica do comando ou espaçamento diferente não tem cobertura demonstrada (`bin/tests/test_trava_teste.sh:44`, `bin/tests/test_trava_teste.sh:85`, `bin/tests/test_trava_teste.sh:117`).
- Uma futura regra que só procure nomes no texto inteiro do Bash pode aprovar `echo suite.sh; docker run … manage.py test …`; isso é contraexemplo da interpretação textual do contrato, implementação futura [não verificado]. [nao verificado]

**Incompatibilidade concreta com destaque:** o selo atual lança a trava em background, dá TERM ao PID retornado por `$!` e exige container parado e rc 143 (`bin/tests/test_trava_dono_para_filho.sh:54-63`). Aquele PID virar ESPERADOR cuja morte não mata o job contradiz o teste. Sem adaptar o selo, a régua reprovaria a mudança, pois executa esse arquivo (`bin/regua.sh:138-143`). O teste de KILL também espera que o mesmo PID seja o dono e que sua morte produza container órfão (`bin/tests/test_trava_dono_para_filho.sh:68-80`).

## 31/10 21:53 · sessao d3273d2284fb · PADRAO-DE-CIMA · verificador · origem=Code

**REPROVADO.** Permanecem caminhos de mordida falsa, atribuição falsa ao prazo e dois leitores fora da cura: o primeiro leitor de modelos e o consumidor de prova de `ESPERANDO`. (`bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:293`; `app/core/censo_escritas.py:105`; `bin/hook_stop_fila1.py:585`)

Parecer por leitura estática. Execução dos selos, ocorrência em produção e autenticação do hash pelo histórico Git: **[nao verificado]**.

**1. Escritores e leitores dos estados**

| Estado | Quem escreve | Quem lê; migração pendente |
|---|---|---|
| `logs/sessao/padrao_de_cima_geracao` | `_pdc_geracao_grava`, chamada pelo callback PDC antes da tentativa de gravar o estado. (`bin/disjuntor.py:1454`; `bin/disjuntor.py:1487`; `bin/disjuntor.py:1491`) | `_pdc_geracao_lida`, usada no fallback. Há também o leitor genérico de **mtime** `_prova_arquivo`, cuja exclusão dos arquivos do próprio instrumento não recebeu o arquivo novo. (`bin/disjuntor.py:1499`; `bin/disjuntor.py:1548`; `bin/hook_stop_fila1.py:578`; `bin/hook_stop_fila1.py:585`) |
| `escopo.geracao` | `_pdc_grava_escopo`, depois de estado gravado, pelo anúncio ainda sob a trava. (`bin/disjuntor.py:292`; `bin/disjuntor.py:1462`; `bin/disjuntor.py:1514`) | `_pdc_escopo_lido` carrega; `estado_pdc` compara com o marcador. Não identifiquei outro escritor desse campo na cópia. (`bin/disjuntor.py:1529`; `bin/disjuntor.py:1549`) |
| Arquivo `disjuntor_estado.json` | Porta produtiva `_com_estado`: lê, executa callback e substitui por temporário. Fixtures também escrevem diretamente. (`bin/disjuntor.py:267`; `bin/disjuntor.py:275`; `bin/disjuntor.py:281`; `bin/tests/test_padrao_de_cima.sh:576`; `bin/tests/test_padrao_de_cima.sh:620`) | `_com_estado` e `ler_estado`; o painel consome via `_pdc_painel`, inclusive pelo gerador do ESTADO. (`bin/disjuntor.py:302`; `bin/disjuntor.py:1636`; `bin/disjuntor.py:2236`; `bin/gerar_estado.py:281`) |
| `pdc.eventos`, `nomeados`, `libera_vistas`, `fotos`, `disparadas` | Callback de `decide_pdc`. Fixtures alteram eventos, versão e agrupamentos sem atualizar geração. (`bin/disjuntor.py:1385`; `bin/disjuntor.py:1402`; `bin/disjuntor.py:1440`; `bin/disjuntor.py:1445`; `bin/disjuntor.py:1449`; `bin/tests/test_padrao_de_cima.sh:574`) | A própria decisão, janela, agrupamento e painel. O painel não consulta geração; apresenta contagens, sem decidir liberação. (`bin/disjuntor.py:1391`; `bin/disjuntor.py:1303`; `bin/disjuntor.py:1308`; `bin/disjuntor.py:1633`) |
| `<raiz>.proc`: `INI/FIM/SINAL/PRAZO` | `_mordida_sondado` trunca; `sitecustomize` escreve INI, FIM e SINAL; a função shell escreve PRAZO; o controle copia a linha de base. São escritores distintos deliberados. (`bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:55`; `bin/tests/_mordida.sh:59`; `bin/tests/_mordida.sh:65`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:210`) | `abertos` interpreta os registros de mutante e controle. O `timeout` do Bash separado do wrapper continua sem produzir PRAZO. (`bin/tests/_mordida.sh:244`; `bin/tests/_mordida.sh:261`; `bin/tests/test_padrao_de_cima.sh:289`; `bin/codex_pergunta.sh:177`) |
| `.exc` | Truncamento, callback de toda exceção RAISE da cópia e cópia da linha de base. (`bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:209`) | Controle procura cinco tipos de erro de programação; verificador lê para declarações e para a nova regra de `SystemExit`. (`bin/tests/_mordida.sh:206`; `bin/tests/_mordida.sh:258`; `bin/tests/_mordida.sh:285`; `bin/tests/_mordida.sh:314`) |
| `.esc` | Truncamento, `sys.excepthook` e cópia da linha de base. (`bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:125`; `bin/tests/_mordida.sh:211`) | Verificador desconta declaradas e compara multiplicidade das assinaturas restantes com o controle. (`bin/tests/_mordida.sh:259`; `bin/tests/_mordida.sh:319`; `bin/tests/_mordida.sh:320`) |
| `.novo` | `_mordida_muta` trunca e grava textos de substituição separados por NUL. (`bin/tests/_mordida.sh:169`; `bin/tests/_mordida.sh:192`) | Regra de `SystemExit` e conferência das exceções declaradas. (`bin/tests/_mordida.sh:269`; `bin/tests/_mordida.sh:284`; `bin/tests/_mordida.sh:310`) |
| `.orig.d` | `_mordida_muta` apaga a coleção anterior e guarda o original do arquivo mutado. (`bin/tests/_mordida.sh:169`; `bin/tests/_mordida.sh:176`; `bin/tests/_mordida.sh:178`) | As duas conferências anteriores; se o original estiver ausente, ambas usam o arquivo **atual** como substituto da prova original. (`bin/tests/_mordida.sh:291`; `bin/tests/_mordida.sh:292`; `bin/tests/_mordida.sh:308`; `bin/tests/_mordida.sh:309`) |
| `_MORDIDA_PROC` | `_mordida_sondado` atribui e depois esvazia; não é local nem restaura valor anterior. (`bin/tests/_mordida.sh:157`; `bin/tests/_mordida.sh:162`) | A função `timeout`, para escolher onde apendar PRAZO. (`bin/tests/_mordida.sh:135`) |
| Função shell `timeout` | Definição global ao carregar o harness nos dois selos. (`bin/tests/_mordida.sh:131`; `bin/tests/test_padrao_de_cima.sh:34`; `bin/tests/test_disjuntor.sh:45`) | Chamadas dos casos passam a usá-la, inclusive chamadas antigas do selo disjuntor. Ela não é exportada para o Bash novo do wrapper. (`bin/tests/test_padrao_de_cima.sh:601`; `bin/tests/test_disjuntor.sh:579`; `bin/tests/test_disjuntor.sh:661`; `bin/tests/test_padrao_de_cima.sh:289`) |

Há vários callbacks escritores do **arquivo** de estado, mas passam pela mesma porta: B1, triagem, B3, rearme, B2 e B4. Esses callbacks alteram outras chaves; não constituem outro escritor produtivo de `pdc.eventos` que precisasse incrementar a geração PDC. (`bin/disjuntor.py:494`; `bin/disjuntor.py:845`; `bin/disjuntor.py:986`; `bin/disjuntor.py:1059`; `bin/disjuntor.py:1782`; `bin/disjuntor.py:1808`; `bin/disjuntor.py:2058`; `bin/disjuntor.py:2066`)

**2. Classes que já reprovaram obra**

| Classe | Julgamento neste diff |
|---|---|
| 1. Segundo juiz registrado | **Violação não evidenciada.** PDC continua decidida por `decide_pdc`/`estado_pdc`; o caminho novo da chave consulta a mesma `desligado`. O lookup declara que o registro vivo é `core/juizes.py`. (`bin/disjuntor.py:1360`; `bin/disjuntor.py:1537`; `bin/disjuntor.py:2096`; `app/docs/JUIZES.md:10`) |
| 2. Segundo escritor fora da porta | **Não encontrei segundo escritor produtivo de PDC.** Existem escritores diretos nas fixtures, sem geração; isso impede tratar a geração como detector universal de qualquer alteração do JSON, mas não demonstra outro escritor produtivo. (`bin/disjuntor.py:1385`; `bin/disjuntor.py:1454`; `bin/tests/test_padrao_de_cima.sh:576`; `bin/tests/test_padrao_de_cima.sh:620`) |
| 3. Relógio fora da borda | **Violação nova não evidenciada.** A geração usa aleatoriedade; a decisão continua recebendo ou capturando `agora`, repassado ao escopo. (`bin/disjuntor.py:1369`; `bin/disjuntor.py:1462`; `bin/disjuntor.py:1486`) |
| 4. Derivação paralela de fonte única | **Violação nova não evidenciada.** O escopo permanece uma projeção da decisão, com geração adicionada. A falsa prova de prazo abaixo é um problema de evidência, não demonstração de juiz de negócio duplicado. (`bin/disjuntor.py:1514`; `bin/disjuntor.py:1515`; `bin/tests/_mordida.sh:135`) |
| 5. Selo/RED sem mordida | **Sim, bloqueia.** Há mordida aceita sem execução da decisão e autorização testada apenas por ausência de saída. (`bin/tests/_mordida.sh:293`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/test_padrao_de_cima.sh:329`) |
| 6. Ordem de trava | **Inversão nova não demonstrada.** Geração e escopo ficam na mesma transição protegida. Há, entretanto, leitura bloqueante remanescente dentro dessa seção crítica, detalhada abaixo. (`bin/disjuntor.py:1454`; `bin/disjuntor.py:1462`; `bin/disjuntor.py:1466`; `app/core/censo_escritas.py:105`) |
| 7. Pouso antes do pré-requisito | **[nao verificado].** A exigência é suíte antes do opus e opus antes do pouso; os anexos não demonstram sua execução. (`CLAUDE.md:549`; `CLAUDE.md:553`) | [nao verificado]

**3. O que o harness prova — e os caminhos de veredito errado**

A aceitação exige: mutação com âncoras únicas e sintaxe válida; caso com rc não zero atribuído ao último assert reconhecido; linha de base existente; ausência de contaminação adicional segundo os contadores; declarações presentes e admitidas; ausência de exceção escapada adicional. A checagem sintática prova compilação, não execução do módulo nem passagem pela decisão. (`bin/tests/_mordida.sh:181`; `bin/tests/_mordida.sh:185`; `bin/tests/_mordida.sh:197`; `bin/tests/_mordida.sh:218`; `bin/tests/_mordida.sh:220`; `bin/tests/_mordida.sh:226`; `bin/tests/_mordida.sh:262`; `bin/tests/_mordida.sh:314`; `bin/tests/_mordida.sh:323`)

“Mordeu pelo prazo” significa apenas: houve mais processos sem FIM, com SINAL cujo **grupo** aparece numa linha PRAZO, do que no controle. PRAZO, por sua vez, significa que `wait` devolveu 124. Não há registro independente de expiração nem identificação da origem do sinal. (`bin/tests/_mordida.sh:134`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:253`; `bin/tests/_mordida.sh:254`; `bin/tests/_mordida.sh:266`)

- **B1 — bloqueia: saída antecipada Python aceita como mordida.** Mutar `return padrao_parada(argv[1])` para `return 1` faz `i14_mesma` retornar pelo assert 1, sem chamar a decisão. INI/FIM fecham normalmente. `SystemExit` nasce em `sys.exit(main(...))`, linha original e ausente do texto novo; a guarda não acusa. O caso foi derrubado por uma saída antecipada que o harness afirma excluir. (`bin/disjuntor.py:2333`; `bin/disjuntor.py:2343`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:59`; `bin/tests/_mordida.sh:293`)

- **B2 — bloqueia: mutante shell que não roda o instrumento também é aceito.** Trocar `set -uo pipefail` do wrapper por `exit 1` passa `bash -n`; `wrapper` retorna seu assert 1. Nenhum Python do wrapper precisa executar. O verificador penaliza processos abertos adicionais, mas não exige execução nem quantidade mínima de processos; a regra de saída plantada só examina `SystemExit` Python. (`bin/codex_pergunta.sh:27`; `bin/tests/_mordida.sh:197`; `bin/tests/test_padrao_de_cima.sh:294`; `bin/tests/_mordida.sh:262`; `bin/tests/_mordida.sh:285`)

- **B3 — bloqueia: 124 e grupo não provam prazo.** Como contraexemplo estático, a substituição abaixo em `main` cria um filho que se mata, espera por ele e devolve 124 imediatamente; o pai sai pela linha original de `sys.exit`. O filho herda a sonda e o grupo do comando temporizado. A função grava PRAZO, `resolvedor_fifo` retorna assert 1 e o verificador classifica o filho como morto pelo prazo, embora o caminho não espere os 20 segundos. (`bin/disjuntor.py:2333`; `bin/disjuntor.py:2343`; `bin/tests/_mordida.sh:65`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:254`; `bin/tests/test_padrao_de_cima.sh:601`; `bin/tests/test_padrao_de_cima.sh:603`)

  ```python
  return __import__('subprocess').call([
      __import__('sys').executable, '-c',
      'import os; os.kill(os.getpid(), 15)'
  ]) and 124
  ```

  Também não se distingue um SIGTERM externo recebido por um descendente antes do vencimento de um timeout que expire depois: a associação considera somente o grupo, sem instante ou emissor do sinal. (`bin/tests/_mordida.sh:65`; `bin/tests/_mordida.sh:135`; `bin/tests/_mordida.sh:254`)

- **F1 — fila: falso VERMELHO por `SystemExit` capturada.** `.exc` registra toda ocorrência, mas o filtro novo acusa qualquer `SystemExit` em linha considerada plantada, mesmo capturada e seguida da decisão real. Além disso, `plant` contém todas as linhas dos blocos novos, inclusive contexto inalterado, sem distinguir posição ou arquivo. Isso pode recusar uma mordida válida; não demonstra liberação indevida do instrumento produtivo. (`bin/tests/_mordida.sh:115`; `bin/tests/_mordida.sh:284`; `bin/tests/_mordida.sh:285`; `bin/tests/_mordida.sh:293`)

- **F2 — fila: contaminações podem se compensar por quantidade.** O controle não rejeita processos sem FIM ou SIGTERM externo; esses totais viram tolerância. Um aborto do mutante pode substituir outro aborto do controle sem aumentar o contador. A comparação não demonstra correspondência entre operações equivalentes. (`bin/tests/_mordida.sh:205`; `bin/tests/_mordida.sh:206`; `bin/tests/_mordida.sh:255`; `bin/tests/_mordida.sh:262`; `bin/tests/_mordida.sh:264`)

**4. Cada caso e mutante novo morde o quê**

Os caminhos abaixo são deduções do código; execução e tempo observado são **[nao verificado]**.

| Caso/mutante | Julgamento |
|---|---|
| Extensão `resolvedor_fonte` + `v9-resolvedor-v-parado` | **Morde a versão 2:** o vazio plantado com `res=2` é aceito quando V=2; não redispara, produzindo `return 16`. Porém o trecho novo também passaria contra 5b39ae82: V=3 e a condição de reclassificação já eram código anterior no diff fornecido. É cobertura adicional, não RED desta cura. (`bin/tests/test_padrao_de_cima.sh:574`; `bin/tests/test_padrao_de_cima.sh:578`; `bin/tests/test_padrao_de_cima.sh:805`; `bin/disjuntor.py:1105`; `bin/disjuntor.py:1392`) |
| `resolvedor_fifo` + `v9-modelos-fifo` | O primeiro FIFO prende o `open` reintroduzido; rc124 leva a `return 1`. Morde a remoção de `_abre` nesse leitor, condicionado às limitações da prova de prazo. Não cobre o leitor anterior `CE.modelos`. (`bin/tests/test_padrao_de_cima.sh:600`; `bin/tests/test_padrao_de_cima.sh:603`; `bin/tests/test_padrao_de_cima.sh:806`; `bin/disjuntor.py:1222`) |
| `resolvedor_fifo` + `v9-sitio-fifo` | Primeiro subcaso passa; o segundo FIFO prende o `open` do sítio e leva a `return 2`. O caso exige rc4 antes do limite, mas não mede a alegação “no ato”: uma espera inferior a 20 segundos seguida de rc4 passa. (`bin/tests/test_padrao_de_cima.sh:604`; `bin/tests/test_padrao_de_cima.sh:607`; `bin/tests/test_padrao_de_cima.sh:808`) |
| `escopo_velho` + `v9-escopo-de-outra-geracao` | **Morde a comparação removida:** escopo antigo omite A; estado ilegível e RELATO neutro permitem LIVRE, derrubando o assert final. O caso não confere token, ordem de escrita nem recriação. Uma cura incorreta que sempre recuse o fallback também passa **esse caso**; `escopo_duvida` oferece o contraponto LIVRE na suíte. (`bin/tests/test_padrao_de_cima.sh:612`; `bin/tests/test_padrao_de_cima.sh:621`; `bin/tests/test_padrao_de_cima.sh:810`; `bin/tests/test_padrao_de_cima.sh:513`; `bin/disjuntor.py:1567`) |
| Extensão `desligado` + `v9-cru-ignora-chave` | **Morde a negativa que reaparece** nos arrays com a chave presente, por `return 6`. **B4 — bloqueia:** não confere rc. Trocar o corpo do ramo novo por `raise SystemExit(1)` deixa stdout vazio e faz esse caso passar, embora o processo tenha encerrado com erro. (`bin/tests/test_padrao_de_cima.sh:328`; `bin/tests/test_padrao_de_cima.sh:329`; `bin/tests/test_padrao_de_cima.sh:331`; `bin/tests/test_padrao_de_cima.sh:811`; `bin/disjuntor.py:2099`) |
| Meta `mutante-que-nao-compila` | Exercita a recusa de sintaxe Python antes do caso. **Fila:** não há meta correspondente para o novo `bash -n`. (`bin/tests/test_padrao_de_cima.sh:831`; `bin/tests/_mordida.sh:187`; `bin/tests/_mordida.sh:197`) |
| Meta `saida-plantada` | Exercita um `raise SystemExit` diretamente plantado; não cobre retorno antecipado através da linha original de saída nem `exit` shell — B1/B2. (`bin/tests/test_padrao_de_cima.sh:833`; `bin/tests/test_padrao_de_cima.sh:834`; `bin/tests/_mordida.sh:293`) |
| Meta `sigterm-sem-prazo` | Exercita automorte direta, sem timeout. Não cobre 124 produzido pelo filho nem sinal externo dentro de grupo posteriormente marcado PRAZO — B3. (`bin/tests/test_padrao_de_cima.sh:836`; `bin/tests/test_padrao_de_cima.sh:837`; `bin/tests/_mordida.sh:254`) |

Os mutantes antigos tocados de sítio/modelos continuam retirando o tratamento que transforma fonte ilegível em dúvida, e o de escopo continua deslocando a gravação para fora da trava; suas âncoras foram adaptadas ao novo código. As alterações dos chamadores nos dois selos propagam a mensagem de falha do mutador. (`bin/tests/test_padrao_de_cima.sh:738`; `bin/tests/test_padrao_de_cima.sh:785`; `bin/tests/test_padrao_de_cima.sh:793`; `bin/tests/test_padrao_de_cima.sh:698`; `bin/tests/test_disjuntor.sh:1501`; `bin/tests/test_disjuntor.sh:1510`)

**Fila:** os metas exigem uma falha contendo determinado trecho; não exigem que aquela seja a única causa dentro da mensagem. Um meta pode passar com contaminação adicional agregada na mesma falha. (`bin/tests/_mordida.sh:326`; `bin/tests/_mordida.sh:335`; `bin/tests/_mordida.sh:336`)

**5. Demais erros e fragilidades**

- **B5 — bloqueia: o primeiro leitor de modelos não foi migrado.** `_prepara` chama `CE.modelos` **antes** de `_so_modelos`. Ali permanece `isfile` seguido de `open(...).read()`: trocar regular por FIFO entre essas operações prende o resolvedor dentro do flock do estado. O FIFO estático do caso é rejeitado pelo `isfile` e não exercita essa janela. A cura protege o segundo leitor, não toda a cadeia. (`bin/disjuntor.py:1222`; `bin/disjuntor.py:1224`; `app/core/censo_escritas.py:102`; `app/core/censo_escritas.py:105`; `bin/disjuntor.py:260`; `bin/disjuntor.py:275`; `bin/tests/test_padrao_de_cima.sh:600`)

- **B6 — bloqueia: geração nova pode virar prova falsa de `ESPERANDO`.** Declare `ESPERANDO: agente-x | arquivo=logs/sessao/padrao_de_cima_geracao`; a primeira avaliação memoriza o mtime. Uma decisão PDC reescreve o arquivo mesmo sem evento novo. Na próxima avaliação, a exclusão não reconhece o arquivo como produto do próprio instrumento, o mtime avançado satisfaz a prova e o Stop pode liberar. O escopo preexistente também está ausente dessa exclusão. (`bin/hook_stop_fila1.py:658`; `bin/hook_stop_fila1.py:679`; `bin/hook_stop_fila1.py:585`; `bin/hook_stop_fila1.py:623`; `bin/hook_stop_fila1.py:640`; `bin/hook_stop_fila1.py:1233`; `bin/disjuntor.py:1454`; `bin/disjuntor.py:1521`)

- **Fila — o prazo não garante encerramento.** O wrapper não acrescenta escalada para SIGKILL; se o código ignorar SIGTERM, `wait` pode permanecer preso. Isso limita a robustez do harness contra outros mutantes, embora os dois novos mutantes FIFO mantenham o handler da sonda. (`bin/tests/_mordida.sh:63`; `bin/tests/_mordida.sh:133`; `bin/tests/_mordida.sh:134`)

- **Fila — prazo do wrapper ficou fora do protocolo.** `_xw` inicia outro Bash; a função não é exportada, e o `timeout` interno continua sem PRAZO. Uma morte legítima nesse caminho pode ser classificada como SIGTERM sem prazo, em vez de receber a atribuição pretendida. (`bin/tests/test_padrao_de_cima.sh:289`; `bin/codex_pergunta.sh:177`; `bin/tests/_mordida.sh:131`; `bin/tests/_mordida.sh:264`)

- **Fila — sem suporte a casos paralelos no mesmo selo.** Os casos compartilham `A/EST`; `_arvore` remove a árvore inteira. Linhas de base por nome e `meta.out` também são compartilhados. Os loops atuais são sequenciais e execuções independentes recebem `mktemp` distintos; paralelizar casos internamente pode produzir falhas de fixture aceitas como mordida. (`bin/tests/test_padrao_de_cima.sh:29`; `bin/tests/test_padrao_de_cima.sh:35`; `bin/tests/test_padrao_de_cima.sh:39`; `bin/tests/test_padrao_de_cima.sh:683`; `bin/tests/_mordida.sh:209`; `bin/tests/_mordida.sh:335`; `bin/tests/test_disjuntor.sh:39`)

- **Fila — sonda não reentrante.** Uma chamada aninhada de `_mordida_sondado` sobrescreve os globais e termina esvaziando `_MORDIDA_PROC`, sem restaurar o contexto exterior. Não há chamada aninhada demonstrada pelos chamadores atuais. (`bin/tests/_mordida.sh:156`; `bin/tests/_mordida.sh:157`; `bin/tests/_mordida.sh:162`; `bin/tests/_mordida.sh:204`; `bin/tests/_mordida.sh:216`)

- **Fila — VERMELHO por premissa de permissões.** `escopo_velho` depende de `chmod 555` realmente impedir a substituição no diretório. Se o executor tiver privilégio para escrever ali, falta a falha esperada e o caso reprova código intacto pelo `grep`. Identidade e privilégios do executor real: **[nao verificado]**. (`bin/tests/test_padrao_de_cima.sh:615`; `bin/tests/test_padrao_de_cima.sh:619`)

- **Fila — temporário do escopo sem limpez

[truncado em 20000 de 21588 caracteres]

## 31/10 21:49 · sessao af7b8582d461 · O261-E-F1C4-MEMO-C6 · censo · origem=Code

**A chave não cobre todos os insumos do C6. Há também um caminho condicional no código de produção que pode mudar batidas entre duas consultas às mesmas perguntas. Sua ocorrência e uma divergência efetiva em produção são [nao verificado].** (`app/chamados/juizes.py:788–796`; `app/ponto/services/cartorio.py:1141,1394`; `app/chamados/services/regularizacao_ext.py:136,155,181`)

1) **Quem escreve e quem lê `_pv_memo`**

Os acessos executáveis ao atributo encontrados na cópia são estes. (`app/chamados/juizes.py:813–824`; `app/chamados/models.py:1439`)

| Operação | Sítio |
|---|---|
| **Escreve/substitui** `(chave, resultado)` | `pergunta_viva`, depois do cálculo; recalcula a chave para incluir relações carregadas durante a avaliação. `app/chamados/juizes.py:816–824` |
| **Lê** o atributo, compara a chave e devolve o resultado armazenado | `pergunta_viva`: `getattr`, comparação de `_m[0]` e retorno de `_m[1]`. `app/chamados/juizes.py:813–815` |
| **Remove** o atributo da cópia da pergunta | `_universo_da_ressuscitacao`, antes de consultar C6 sobre o estado projetado da ressuscitação. `app/chamados/models.py:1427–1445` |

Os testes do memo exercitam o atributo **indiretamente**, chamando `pergunta_viva`: repetição na mesma instância, comparação com instância recém-carregada, alterações de campos da chave, alteração sem `save` e repetição sem queries. Não acessam diretamente `_pv_memo` nesses casos. (`app/chamados/tests/test_selo_memo_pergunta_viva.py:81–114,117–178`)

Os demais registros textuais são comentários/docstrings sobre sua duração e uma referência ao nome da função da chave; não são acessos executáveis ao atributo. (`app/chamados/juizes.py:905,943`; `SERIE_T.md:2106`)

2) **Insumos lidos que ficam fora da chave**

A chave contém somente:

```text
p.validada_em, respondida_em, resposta_colab, via_resolucao,
materializada_em, data_evento, tipo_resposta, celula_id,
p.celula.veredito, p.celula.veredito_via,
p.chamado_destino_id, status_local do destino já carregado
```

O status do destino entra como `'?'` quando a relação ainda não está no cache. (`app/chamados/juizes.py:778–796`)

As dependências abaixo ficam fora dessa tupla; sua utilização depende do ramo alcançado pelo cálculo. (`app/chamados/juizes.py:1032–1118`)

| Insumo omitido | Leitura no cálculo ou nos chamados |
|---|---|
| **`p.motivo`** | Escolhe o marco cobrado, participa da concordância com a luz e seleciona dados do contexto. `app/chamados/juizes.py:863,1000,362`; `app/chamados/services/materializacao.py:327–333` |
| **`cel.dna['marcos']`** | Horários do marco e dos pares de pausa, inclusive existência de segunda pausa. `app/chamados/juizes.py:870–884,1002–1007` |
| **`cel.ata['lampadas']`** | `hora`, `tipo`, `acesa` e `luz` determinam fato provado e concordância com a resposta. `app/chamados/juizes.py:887–893,1008–1029` |
| **Marcos pendentes da ata** | C6 chama `marcos_pendentes`; ela lê `ata.lampadas[*].pendente`. Aqui **lastro é o módulo**, não uma leitura de campo `cel.lastro`. `app/chamados/juizes.py:1094–1096`; `app/ponto/services/lastro.py:138–150` |
| **`cel.trabalha`** | `_celulas_da_pergunta` fornece a própria FK ao classificador; `classificar_falta` cala quando `trabalha is False`. `app/chamados/juizes.py:598–606`; `app/escala/servico_jornada.py:302–304` |
| **Identidade e disponibilidade da célula** | `cel.data`, `cel.colaborador_id`, `colab.pk` e campos diferidos determinam se a FK será aproveitada ou se haverá outra consulta. `app/chamados/juizes.py:602–606`; `app/escala/servico_jornada.py:267–271` |
| **Disputa, colaborador e empresa** | A tranca usa `p.disputa.colaborador`, `empresa_id` e empresa; classificação, slot e turnos também dependem do colaborador. `app/chamados/juizes.py:909–921,1100–1102,344,369` |
| **Competência e tranca viva** | Corte da empresa, existência da linha `PeriodoFechado`, empresa/mês/ano, `reaberto_em` e `fechado_em`. `app/ponto/janelas.py:50–63`; `app/ponto/services/fechamento.py:1432–1438`; `app/chamados/juizes.py:918–928` |
| **Toque humano posterior à tranca** | Trilhas, usuário, instante, ação, modelo, objeto, descrição e células/chamados relacionados ao dia. `app/chamados/juizes.py:971–978` |
| **Batidas e retratações** | Existência, tipo, timestamp, colaborador, vínculo à pergunta e `retratada_em`; afetam plantio comprovado, slot e completude do turno. `app/chamados/juizes.py:233–237,258–266,695–702,751–755`; `app/ponto/turnos.py:1861–1864` |
| **Trilha de retratação da pergunta** | Consulta `LogAuditoria` pelo ato e pelo `p.pk`. `app/chamados/juizes.py:275–280` |
| **Ausências** | Existência/cobertura, status, tipo, datas e minutos; a classificação consulta ausências e distingue cobertura parcial. `app/escala/servico_jornada.py:340–345`; `app/ponto/turnos.py:2509–2533,2595–2600` |
| **Contextos da disputa e do destino** | `disputa.contexto[motivo].data/hora_tentativa` e `chamado_destino.contexto_json.marcos_faltantes`, incluindo motivo/hora/tipo. `app/chamados/juizes.py:359–370,498–529`; `app/chamados/services/materializacao.py:329–333,394–406` |
| **Vínculos, geradora, vigência e template de escala** | Determinam escala e marcos esperados, inclusive fallback quando não há DNA utilizável. `app/escala/servico_jornada.py:46–58,354,427–435`; `app/escala/alimentacao.py:237–254`; `app/escala/models.py:1094–1108` |
| **Outros dados utilizados pela grade do slot** | Admissão, demissão, isenção, ata e `trabalha`, por chamadas a `grade_da_celula`. `app/chamados/services/materializacao.py:218–227,267–268`; `app/escala/services/leitor_celula.py:321–372` |
| **Relógio** | `date.today()` muda o alcance da janela; `timezone.now()` participa do teto do turno. `app/chamados/juizes.py:539–565`; `app/escala/servico_jornada.py:375–378` |

**Cartório em curso exige uma distinção:** C6 lê diretamente `p.celula`; ele não consulta automaticamente `ata_deste_julgamento` ou `veredito_deste_julgamento`. Quem consulta esses mapas é `celula_deste_julgamento`, que produz uma projeção da célula. Na ressuscitação, essa projeção é entregue a uma cópia da pergunta e seu memo é removido antes da avaliação. (`app/chamados/juizes.py:1044,866,392–395`; `app/ponto/portas/celula.py:675–683`; `app/chamados/models.py:1427–1445`)

A tranca possui **outro memo**, no request ou, fora dele, no colaborador. Ele guarda a consulta da tranca inclusive quando resulta em `None`; o memo de toque humano guarda somente o resultado positivo. Portanto, invalidar apenas `_pv_memo` não garante nova consulta da tranca. (`app/chamados/juizes.py:914–923,944–948,965–980`)

3) **Tempo de vida e objetos que o preservam**

O código executável não impõe duração por request: o atributo permanece na instância e o resultado é reutilizado enquanto a chave coincidir. “Morre com o request” é a descrição da intenção; não há identificador de request ou prazo no mecanismo. (`app/chamados/juizes.py:799–824`)

Há preservação concreta das mesmas perguntas nestes contêineres:

| Contêiner | Sobrevida demonstrada |
|---|---|
| **Listas do fio/modal** | As perguntas carregadas entram em `perguntas_por_chamado`, são anexadas a `ch.perguntas_disputa` e passadas ao explicador. `.viva`, `estado` e `fio_vivo` consultam C6 sobre esses objetos. `app/chamados/services/fio.py:222–249,279–281,308–309`; `app/chamados/services/explicador.py:124,133–137`; `app/chamados/models.py:2479,2641` |
| **Prefetch do cartório** | Os chamados guardam perguntas prefetched; o mesmo manager é usado em duas consultas ao fio dentro do corpo. `app/ponto/services/cartorio.py:96–107,1141,1393–1394` |
| **Lista temporária do gate de chamado** | `_f1_ancoradas` conserva perguntas entre o plano e `_gravar_estado`, que as julga novamente. `app/chamados/models.py:362–364,210–218` |
| **Prefetch do relatório** | `classificar` recebe perguntas do prefetch e chama C6; o fluxo monta resultados sem mutação desses insumos demonstrada. `app/chamados/relatorios_disputa.py:11–17,39–52` |

O command do cartório conserva as células em `cels`, e o serviço pode reutilizá-las nas passadas seguintes. Entretanto, cada entrada no corpo recarrega os chamados e suas perguntas: isso **não demonstra sobrevivência do mesmo `p._pv_memo` entre passadas**, nem identidade entre a célula do loop e `p.celula`. (`app/ponto/management/commands/processar_cartorio.py:59–65`; `app/ponto/services/cartorio.py:777,802,867,1071,1105`; `app/chamados/juizes.py:598–606`)

4) **Caminhos de produção: chamada → mutação → nova chamada**

**Existe uma família condicional pelo cartório e pelos signals de fechamento.** A identidade preservada é a pergunta prefetched de um chamado `y`; a mutação intermediária pode ocorrer ao reconciliar outro chamado `x`. Existência desses registros no ambiente produtivo: **[nao verificado]**. (`app/ponto/services/cartorio.py:96–107,1141,1390–1394`)

A sequência codificada é:

1. O command chama `julgar_colab`; o corpo carrega os chamados com perguntas prefetched e avalia `_fio_vivo` dessas perguntas. (`app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1071,96–97,1141`)
2. Um chamado `x` passa por `reconciliar`, nos ramos de marcos satisfeitos, marcos inexistentes ou cobrança indevida. A porta grava seu estado dentro de transação. (`app/ponto/services/cartorio.py:1287,1300,1321`; `app/chamados/models.py:614–619,279`)
3. O signal S143 recebe o fechamento e chama `resolver_perguntas_por_chamado_resolvido`. (`app/chamados/signals.py:75–92`)
4. O resolvedor inclui **todas as disputas abertas do colaborador** em `_cands` e fecha aquelas liberadas pelo C7. A guarda de credores vivos pertence ao laço anterior de carimbo; não elimina candidatos desse laço final. (`app/chamados/services/disputa_emissao.py:1621–1633,1672–1679`)
5. O fechamento salva a disputa e agenda `retomar_regularizacoes_confirmadas` no commit. (`app/chamados/models.py:1846–1850`; `app/chamados/signals.py:97–121`)
6. A retomada busca regularizações confirmadas e não materializadas. Sem disputa aberta restante, pode **adicionar**, **remover** ou **corrigir tipo/timestamp** de batidas. (`app/chamados/services/regularizacao_ext.py:218–224,51–65,77,136,153–155,181–185`)
7. Sem transação externa abrangendo o julgamento, o callback pode executar na saída da transação de `x.reconciliar`. O command e `julgar_colab` não introduzem tal transação externa nesses trechos. (`app/chamados/models.py:614–619`; `app/ponto/management/commands/processar_cartorio.py:59–65`; `app/ponto/services/cartorio.py:770–777`)
8. O corpo volta a consultar as perguntas do prefetch de `y`, se ele continuar vivo. O chamado `x` encerrado é pulado. Batidas não participam da chave, permitindo reutilizar o memo anterior de `p`. (`app/ponto/services/cartorio.py:1390–1394`; `app/chamados/juizes.py:788–796,813–815`)

A guarda `em_julgamento()` evita reentrada do cartório pelo signal da batida; ela não impede a gravação da batida. (`app/ponto/signals.py:85–93`; `app/ponto/registro_batida.py:157–161`)

Para essa sequência afetar um insumo efetivamente consultado, C6 pode ter alcançado `_turno_tem_saida` numa pergunta textual materializada: remover/corrigir a saída altera a testemunha externa sem alterar a tupla. Essa combinação concreta e a mudança efetiva de resultado em produção permanecem **[nao verificado]**. (`app/chamados/juizes.py:1097–1114,624–649,754–755,359–370`; `app/chamados/services/regularizacao_ext.py:136,155`)

Os outros candidatos principais não demonstram, por si, esse problema:

| Candidato | Por que não comprova reutilização após mutação omitida |
|---|---|
| **Publicar ata/veredito em curso** | Os mapas mudam entre as consultas, mas não atualizam automaticamente a FK das perguntas. A lavra da célula do loop ocorre depois da segunda consulta. `app/ponto/services/cartorio.py:1188,1259,1394,1565–1586`; `app/chamados/juizes.py:392–395,1044` |
| **Desarquivar e registrar toque humano** | Há ressuscitação antes e depois da trilha, mas cada chamada faz nova consulta, copia a pergunta e remove seu memo. `app/chamados/views_arquivar.py:70,77–86`; `app/chamados/models.py:1420–1445` |
| **Responder pelo PWA/API** | As consultas sobre `p` precedem a escrita; o core altera campos da chave. Não aparece nova consulta ao C6 sobre o `p` original depois da criação de ausência. `app/chamados/views.py:1242,1257,1283`; `app/api/views.py:2683,2695–2702`; `app/chamados/services/disputa_emissao.py:1247–1266,1295,1324` |
| **Validar/materializar** | A validação carrega por ID; a cauda consulta outro queryset. O signal transmite `disputa_id` e recarrega disputa/perguntas. `app/chamados/services/validacao.py:359,424–433`; `app/chamados/services/materializacao.py:1233`; `app/chamados/juizes.py:1130`; `app/chamados/signals.py:195–202`; `app/chamados/services/becos.py:67–68` |
| **Gate de chamado / desfazer carimbo** | O gate altera destino/status, presentes na chave; desfazer carimbo relê a pergunta e altera materialização/via, também presentes. `app/chamados/models.py:339–352`; `app/chamados/services/relavra_via.py:221–235`; `app/chamados/juizes.py:790–795` |

O anexo fornecido não comprova uma ocorrência dessas sequências em produção, nem quantidade de memos incorretos: **[nao verificado]**.

## 31/10 21:49 · sessao f4f7b1e2fa91 · - · - · origem=Ronald (tmux)

Achados dos casos/mutantes, por leitura estática; execução dos selos não verificada.

- **Bloqueia — saída antecipada ainda dá falsa mordida.** Inserir `return 1` no começo de `main` deixa `sys.exit(main(...))` na linha original; `i14_mesma` retorna pelo assert, sem chamar a decisão. A verificação de `SystemExit` aceita essa linha original. Também inserir `exit 1` no começo do wrapper passa `bash -n`, derruba `wrapper` no assert e não executa Python da cópia. (`bin/disjuntor.py:2320`, `bin/disjuntor.py:2343`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/test_padrao_de_cima.sh:294`, `bin/tests/_mordida.sh:197`, `bin/tests/_mordida.sh:285`, `bin/tests/_mordida.sh:293`.)

- **Bloqueia — stdout vazio não prova liberação no caso ampliado.** As duas execuções novas de `pretool` ignoram rc. Substituir o corpo de `if chave` por `raise SystemExit(1)` deixa stdout vazio, mantém as portas anteriores funcionando e faz `desligado` terminar com sucesso, apesar da saída antecipada. O mutante declarado verifica somente a volta da negativa. (`bin/tests/test_padrao_de_cima.sh:321`, `bin/tests/test_padrao_de_cima.sh:328`, `bin/tests/test_padrao_de_cima.sh:329`, `bin/tests/test_padrao_de_cima.sh:331`, `bin/tests/test_padrao_de_cima.sh:811`, `bin/disjuntor.py:2099`.)

| Mutante novo | Caminho da mordida / limite |
|---|---|
| `v9-resolvedor-v-parado` | V=2 aceita o vazio plantado com res=2; não redispara e o caso retorna16. A extensão passa também contra 5b39ae82: V=3 e a guarda de reclassificação já existiam no diff fornecido. (`bin/tests/test_padrao_de_cima.sh:574`, `bin/tests/test_padrao_de_cima.sh:578`, `bin/tests/test_padrao_de_cima.sh:805`, `bin/disjuntor.py:1105`, `bin/disjuntor.py:1392`.) |
| `v9-modelos-fifo` | O primeiro FIFO prende `open`; timeout retorna124 e o caso retorna1. Exige a prova de prazo do harness, cujas fragilidades foram enviadas pelo outro explorer. (`bin/tests/test_padrao_de_cima.sh:600`, `bin/tests/test_padrao_de_cima.sh:601`, `bin/tests/test_padrao_de_cima.sh:603`, `bin/tests/test_padrao_de_cima.sh:806`.) |
| `v9-sitio-fifo` | Primeiro subcaso passa; segundo FIFO prende `open`, timeout retorna124 e o caso retorna2. (`bin/tests/test_padrao_de_cima.sh:604`, `bin/tests/test_padrao_de_cima.sh:605`, `bin/tests/test_padrao_de_cima.sh:607`, `bin/tests/test_padrao_de_cima.sh:808`.) |
| `v9-escopo-de-outra-geracao` | Sem comparação, escopo antigo omite A; estado corrompido e relato neutro chegam a LIVRE, disparando return4 do caso. Trocar a comparação por `if True` também passa esse caso; a suíte completa o rejeitaria pela exigência de LIVRE em `escopo_duvida`. (`bin/tests/test_padrao_de_cima.sh:620`, `bin/tests/test_padrao_de_cima.sh:621`, `bin/tests/test_padrao_de_cima.sh:810`, `bin/tests/test_padrao_de_cima.sh:513`, `bin/disjuntor.py:1549`, `bin/disjuntor.py:1567`.) |
| `v9-cru-ignora-chave` | Desativa a guarda; arrays com chave voltam a produzir negativa, disparando return6. Não distingue liberação normal de crash silencioso. (`bin/tests/test_padrao_de_cima.sh:329`, `bin/tests/test_padrao_de_cima.sh:811`, `bin/disjuntor.py:2102`, `bin/disjuntor.py:2111`.) |

- **Fila — precisão dos novos casos.** FIFO só exige rc4 antes de20s, sem demonstrar “no ato”; não cobre troca concorrente regular→FIFO. `escopo_velho` não inspeciona geração, ordem da gravação, falha de geração, recriação/restauração ou concorrência. (`bin/tests/test_padrao_de_cima.sh:601`, `bin/tests/test_padrao_de_cima.sh:607`, `bin/tests/test_padrao_de_cima.sh:612`, `bin/tests/test_padrao_de_cima.sh:621`.)

- **Bloqueia — leitor anterior ainda pode esperar FIFO.** Antes do novo `_abre`, o resolvedor chama `CE.modelos`, que faz `isfile` seguido de `open`; troca concorrente regular→FIFO pode bloquear ali. O FIFO estático do selo não exercita essa janela. (`bin/disjuntor.py:1222`, `bin/disjuntor.py:1224`, `app/core/censo_escritas.py:102`, `app/core/censo_escritas.py:105`, `bin/tests/test_padrao_de_cima.sh:600`.)

- **Fila — metas estreitos.** Sintaxe exercita somente `.py`, deixando novo `bash -n` sem meta; SystemExit exercita somente `raise` plantado direto; SIGTERM exercita somente sinal próprio fora de timeout. Cada meta aceita incremento de uma falha contendo substring, mesmo se houver causas adicionais na mesma mensagem. (`bin/tests/test_padrao_de_cima.sh:831`, `bin/tests/test_padrao_de_cima.sh:833`, `bin/tests/test_padrao_de_cima.sh:836`, `bin/tests/_mordida.sh:197`, `bin/tests/_mordida.sh:335`, `bin/tests/_mordida.sh:336`.)

Classes: classe5 evidenciada acima; classes1/3/4/6 não tiveram violação específica demonstrada nesta leitura — permanecem juiz PDC único, relógio na borda e geração/escopo dentro de `_com_estado`. Pouso antes de pré-requisito é **[não verificado]**; a exigência consta em `CLAUDE.md:549` e `CLAUDE.md:553`, mas os anexos não trazem execução/pouso. (`bin/disjuntor.py:1360`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1462`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:1537`.)

## 31/10 21:48 · sessao 5dca7ee72020 · - · - · origem=Ronald (tmux)

Há **cadeia de produção condicional alcançável**, inclusive sem passar por `becos`; ocorrência concreta no dado fornecido: **[nao verificado]**.

Sequência:

1. O cartório avalia o fio dos chamados carregados: `app/ponto/services/cartorio.py:1141`.
2. Um chamado `x` reconcilia em `:1287`, `:1300` ou `:1321`.
3. `reconciliar` abre transação e chama `_gravar_estado`: `app/chamados/models.py:614-618`; esta salva o chamado em `:279`.
4. S143 recebe o save terminal e chama o resolvedor: `app/chamados/signals.py:75-92`.
5. O resolvedor inclui **todas as disputas abertas do colaborador** em `_cands`: `app/chamados/services/disputa_emissao.py:1621-1623`; no laço final, faz `refresh`, consulta C7 e chama `d.fechar`: `:1672-1679`.
6. `d.fechar` salva a disputa fechada: `app/chamados/models.py:1846-1850`.
7. O signal agenda retomada da regularização: `app/chamados/signals.py:97-121`.
8. Ao commit de `reconciliar`, regularizações confirmadas ainda não materializadas são retomadas: `app/chamados/services/regularizacao_ext.py:218-224`.
9. Essa porta pode criar batida (`:70-82`), remover (`:131-140`) ou corrigir tipo/timestamp (`:149-155`).
10. O cartório volta a avaliar `_fio_vivo` em perguntas de **outro chamado `y` ainda vivo**: `app/ponto/services/cartorio.py:1390-1394`. `x` terminal é pulado em `:1391`.

Guardas e limitações:

- A regularização exige `status='confirmada'` (`app/chamados/services/regularizacao_ext.py:51-54`) e **nenhuma disputa aberta do colaborador** (`:58-65`): a disputa fechada precisa ser a última aberta.
- C7 pode fechar disputa com chamado `y` ainda vivo, desde que suas perguntas estejam mortas: o laço final `app/chamados/services/disputa_emissao.py:1672-1679` não consulta credores; o guarda de credor vivo em `:1628-1633` protege apenas o bloco anterior.
- `julgar_colab` não envolve o corpo em `atomic`; apenas ativa `_tl.ativo` e executa o corpo (`app/ponto/services/cartorio.py:770-777`). Portanto, no command sem transação externa, o callback pode executar antes de retornar a `:1394`; transação externa em uma invocação específica: **[nao verificado]**.
- `em_julgamento()` impede reentrada do cartório pelo signal da batida (`app/ponto/signals.py:85-93`); não impede a gravação pelo chokepoint (`app/ponto/registro_batida.py:157-161`).

Testemunha lógica para mudança de insumo efetivamente lido: pergunta de `y`, tipo texto, materializada, não validada, via sem encerramento por identidade, célula sem veredito, dia fora da janela viva. C6 alcança `_pergunta_materializada_de_fato` (`app/chamados/juizes.py:1097-1114`), cujo ramo texto consulta `_turno_tem_saida` (`:754-755`, `:359-370`). Uma regularização que remove a saída pode mudar esse resultado sem mudar qualquer componente da chave (`:788-796`). Essa combinação de registros em produção permanece **[nao verificado]**.

## 31/10 21:48 · sessao 8b45f086c323 · - · - · origem=Ronald (tmux)

Cartório contém reutilização comprovada de `PerguntaDisputa`: `chamados_que_falam_de_batida` carrega perguntas com prefetch e guarda os chamados em listas (`app/ponto/services/cartorio.py:96-107`); o corpo consulta essas perguntas em `:1141` e volta a consultar o mesmo manager em `:1393-1394`, que delega a `pergunta_viva` (`app/chamados/juizes.py:1142`).

A publicação de ata/veredito correntes entre essas consultas (`app/ponto/services/cartorio.py:1188`, `:1259`) não altera `p.celula`: o C6 lê a FK diretamente (`app/chamados/juizes.py:1044`, `:866`), e a alimentação de `classificar_falta` devolve essa mesma FK (`:598-606`). A projeção do julgamento corrente aparece explicitamente na cópia de ressuscitação, com remoção do memo (`app/chamados/models.py:1427-1445`). A lavra da célula do loop ocorre depois da segunda consulta (`app/ponto/services/cartorio.py:1565-1586`).

Há, porém, uma cadeia condicional de produção capaz de mudar batidas entre as duas consultas:

1. Primeiro `_fio_vivo` sobre perguntas prefetched: `app/ponto/services/cartorio.py:1141`.
2. Um chamado `x` é reconciliado: `:1287`, `:1300` ou `:1321`; a porta usa transação e grava seu estado (`app/chamados/models.py:614-619`, `:279`).
3. O signal S143 chama o resolver (`app/chamados/signals.py:75-92`).
4. Esse resolver inclui **todas** as disputas abertas do colaborador em `_cands` (`app/chamados/services/disputa_emissao.py:1621-1623`) e fecha as que passam pelo C7 (`:1672-1679`). As guardas de credores vivos em `:1627-1633` pertencem ao laço anterior de carimbo; não eliminam candidatos do laço final.
5. Fechar a disputa salva seu estado (`app/chamados/models.py:1846-1850`) e agenda retomada de regularizações no commit (`app/chamados/signals.py:97-121`).
6. A retomada busca regularizações confirmadas e não materializadas (`app/chamados/services/regularizacao_ext.py:218-224`). A materialização exige nenhuma disputa aberta (`:58-65`) e pode adicionar batidas (`:77`, `:181-185`), removê-las (`:136`) ou corrigi-las (`:153-155`).
7. Ao sair da transação de `x.reconciliar`, a execução volta ao corpo do cartório (`app/chamados/models.py:614-619`), que depois pode consultar novamente as perguntas prefetched de outro chamado `y` ainda vivo (`app/ponto/services/cartorio.py:1390-1394`). O ramo pula `x` se ele se tornou terminal (`:1391`).

Condições necessárias: o fechamento deve deixar nenhuma disputa aberta; precisa existir regularização confirmada aplicável; outro chamado `y` deve continuar vivo e reter perguntas já avaliadas no prefetch. A existência desses registros em produção é **[nao verificado]**. A cadeia é alcançável pelo código; ocorrência ou divergência efetiva em produção é **[nao verificado]**.

As passadas do cartório reutilizam as células de `cels` (`app/ponto/services/cartorio.py:777`, `:802`, `:867`), mas recarregam os chamados/perguntas a cada entrada no corpo (`:1071`, `:96-97`). Portanto a repetição de passadas não comprova, sozinha, sobrevivência do mesmo `p._pv_memo`; tampouco comprova que a célula do loop seja a mesma instância de `p.celula` (`app/ponto/services/cartorio.py:1105`; `app/chamados/juizes.py:598-606`).

## 31/10 21:48 · sessao 538c525ade29 · - · - · origem=Ronald (tmux)

Achados do harness, por leitura estática; os selos não foram executados. [não verificado]

1. **Bloqueia — saída antecipada ainda vira mordida VERDE.** Trocar `return padrao_parada(argv[1])` por `return 1` faz o caso `i14_mesma` retornar seu assert 1 sem chamar a decisão. O processo fecha normalmente; o `SystemExit` nasce na linha original `sys.exit(main(...))`, ausente do texto novo, e escapa da nova guarda. Referências: `bin/disjuntor.py:2333`, `bin/disjuntor.py:2343`, `bin/tests/test_padrao_de_cima.sh:116`, `bin/tests/_mordida.sh:220`, `bin/tests/_mordida.sh:285`, `bin/tests/_mordida.sh:293`.

2. **Bloqueia — `.sh` que sai antes de executar o instrumento também morde.** Trocar `set -uo pipefail` do wrapper por `exit 1` passa `bash -n`; o caso `wrapper` retorna assert 1 antes de executar qualquer Python do wrapper. `.proc/.exc/.esc` vazios não impedem aprovação: o checker penaliza excesso de processos abertos, nunca exige processo ou execução do código mutado. Referências: `bin/codex_pergunta.sh:27`, `bin/tests/_mordida.sh:197`, `bin/tests/test_padrao_de_cima.sh:289`, `bin/tests/test_padrao_de_cima.sh:294`, `bin/tests/_mordida.sh:262`, `bin/tests/_mordida.sh:327`.

3. **Bloqueia — rc 124 não prova expiração.** Exemplo de mutação em `disjuntor.py:2333`:
   ```python
   return __import__('subprocess').call([
       __import__('sys').executable, '-c',
       'import os; os.kill(os.getpid(), 15)'
   ]) and 124
   ```
   O filho herda a sonda, registra `SINAL` no grupo do timeout; o pai fecha normalmente e retorna 124 imediatamente pela linha original `sys.exit`. A função grava `PRAZO`; `resolvedor_fifo` retorna assert 1; o checker aceita e anuncia mordida pelo prazo sem que tenham passado 20 segundos. Caminho: `bin/tests/_mordida.sh:63`, `bin/tests/_mordida.sh:133`, `bin/tests/_mordida.sh:135`, `bin/tests/_mordida.sh:254`, `bin/tests/_mordida.sh:266`, `bin/tests/test_padrao_de_cima.sh:601`, `bin/tests/test_padrao_de_cima.sh:603`, `bin/disjuntor.py:2343`. Caminho estático, não executado. [não verificado]

4. **Fila — `SystemExit` capturada também causa VERMELHO.** A sonda registra toda exceção; a nova prova não consulta se `SystemExit` encerrou o processo. Um mutante válido que levante e capture `SystemExit`, depois efetivamente execute a decisão, é recusado como saída cedo. Além disso, `plant` compara texto global, sem distinguir arquivo, posição ou linha original repetida no bloco novo. Referências: `bin/tests/_mordida.sh:115`, `bin/tests/_mordida.sh:284`, `bin/tests/_mordida.sh:285`, `bin/tests/_mordida.sh:293`.

5. **Fila — comparação de contaminação por totais permite compensação.** O controle pode conter um processo sem fechamento e o mutante trocar esse por outro processo abortado; totais iguais passam. Também aceita sinal externo até a quantidade já encontrada no controle. O controle não rejeita essas mortes, apenas rc e cinco tipos de exceção de programação. Referências: `bin/tests/_mordida.sh:205`, `bin/tests/_mordida.sh:206`, `bin/tests/_mordida.sh:255`, `bin/tests/_mordida.sh:262`, `bin/tests/_mordida.sh:264`.

6. **Fila — prazo não impõe limite de encerramento.** A função não usa `--kill-after`; um mutante que ignore SIGTERM pode manter `wait` indefinidamente. Referências: `bin/tests/_mordida.sh:133`, `bin/tests/_mordida.sh:134`.

Mapa dos estados:

| Estado | Escritores | Leitores |
|---|---|---|
| `.proc` | Truncamento em `_mordida_sondado`; sonda escreve INI/FIM/SINAL; função `timeout` escreve PRAZO; controle copia baseline. `bin/tests/_mordida.sh:155`, `:55`, `:59`, `:65`, `:135`, `:210` | `abertos`, chamado para mutante e controle. `bin/tests/_mordida.sh:247`, `:261` |
| `.exc` | Truncamento; callback RAISE; cópia do controle. `bin/tests/_mordida.sh:155`, `:114`, `:209` | Controle procura cinco tipos; `regs` lê eventos do mutante para declarações e SystemExit. `bin/tests/_mordida.sh:206`, `:236`, `:258`, `:285`, `:314` |
| `.esc` | Truncamento; excepthook; baseline. `bin/tests/_mordida.sh:155`, `:125`, `:211` | Checker conta diferenças e desconta exceções declaradas. `bin/tests/_mordida.sh:259`, `:319`, `:320` |
| `.novo` | `_mordida_muta` trunca e depois grava os textos novos separados por NUL. `bin/tests/_mordida.sh:169`, `:192` | Guarda de SystemExit e validação das declarações. `bin/tests/_mordida.sh:269`, `:284`, `:310` |
| `.orig.d` | `_mordida_muta` remove o anterior e grava original do arquivo. `bin/tests/_mordida.sh:169`, `:176`, `:178` | Guarda de SystemExit e declarações; ausência cai para arquivo atual. `bin/tests/_mordida.sh:291`, `:292`, `:308`, `:309` |
| `_MORDIDA_PROC` | `_mordida_sondado` atribui e depois esvazia, sem restaurar valor anterior. `bin/tests/_mordida.sh:157`, `:162` | Função `timeout`. `bin/tests/_mordida.sh:135` |
| Função `timeout` | Definição global ao dar source no harness. `bin/tests/_mordida.sh:131`, `bin/tests/test_padrao_de_cima.sh:34`, `bin/tests/test_disjuntor.sh:45` | Casos PDC e disjuntor. `bin/tests/test_padrao_de_cima.sh:601`, `:605`, `bin/tests/test_disjuntor.sh:579`, `:661` |

A função não é exportada para o Bash novo do wrapper; seu `timeout` interno continua binário, sem anotação PRAZO. Referências: `bin/tests/_mordida.sh:131`, `bin/tests/test_padrao_de_cima.sh:289`, `bin/codex_pergunta.sh:177`.

Casos do mesmo processo de selo não suportam paralelismo: compartilham árvore `A`, arquivo `EST`, variáveis globais da sonda, baseline por nome do caso e `meta.out` único. Os loops atuais são sequenciais e duas execuções independentes usam diretórios `mktemp` separados. Referências: `bin/tests/test_padrao_de_cima.sh:29`, `:35`, `:36`, `:39`, `:683`, `bin/tests/test_disjuntor.sh:39`, `bin/tests/_mordida.sh:156`, `:209`, `:335`.

## 31/10 21:48 · sessao 00c6433b6e74 · - · - · origem=Ronald (tmux)

Achados PDC, por leitura estática:

1. **Bloqueia — leitor de modelos não migrado mantém janela de bloqueio em FIFO.** `_prepara` chama `CE.modelos` antes do novo `_so_modelos` (`bin/disjuntor.py:1222`, `bin/disjuntor.py:1224`). Esse primeiro leitor faz `isfile(c)` e depois `open(c).read()` sem `O_NONBLOCK` (`app/core/censo_escritas.py:102`, `app/core/censo_escritas.py:105`). Se um arquivo regular virar FIFO entre as duas operações, prende dentro da transição que já detém o flock (`bin/disjuntor.py:260`, `bin/disjuntor.py:275`). O novo caso planta FIFO antes da chamada, quando `isfile` já o rejeita; não cobre a troca concorrente (`bin/tests/test_padrao_de_cima.sh:600`, `bin/tests/test_padrao_de_cima.sh:601`). O `_abre` novo protege o segundo leitor e o sítio citado: abre com `O_NONBLOCK|O_NOFOLLOW`, valida `fstat` e recusa não regular (`bin/disjuntor.py:1250`, `bin/disjuntor.py:1277`, `bin/orcamento_contexto.py:74`, `bin/orcamento_contexto.py:76`).

2. **Bloqueia — arquivo novo do próprio instrumento pode provar ESPERANDO.** O disjuntor reescreve `padrao_de_cima_geracao` em toda decisão PDC, mesmo sem alteração de eventos (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1491`). O consumidor `_prova_arquivo` exclui os arquivos do próprio disjuntor por lista e inode, mas a lista não inclui geração (`bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:598`). Caminho: topo declara `ESPERANDO: produtor | arquivo=logs/sessao/padrao_de_cima_geracao`; a primeira avaliação memoriza mtime, uma chamada PDC o altera, a seguinte considera a espera provada (`bin/hook_stop_fila1.py:658`, `bin/hook_stop_fila1.py:679`, `bin/hook_stop_fila1.py:620`, `bin/hook_stop_fila1.py:640`, `bin/hook_stop_fila1.py:642`). Isso libera o Stop por atividade do instrumento, sem prova da entrega esperada (`bin/hook_stop_fila1.py:1231`, `bin/hook_stop_fila1.py:1233`). O escopo preexistente também está ausente dessa exclusão (`bin/disjuntor.py:1521`, `bin/hook_stop_fila1.py:585`).

3. **Fila — temporário de escopo vaza na falha posterior à criação.** `_pdc_grava_escopo` cria temporário, serializa e faz `replace`; o `except` apenas registra falha, sem `unlink(tmp)` (`bin/disjuntor.py:1518`, `bin/disjuntor.py:1521`, `bin/disjuntor.py:1522`). O escritor de estado vizinho tem limpeza explícita, portanto o comportamento é inconsistente (`bin/disjuntor.py:283`, `bin/disjuntor.py:286`). Falha de `json.dump`, fechamento ou `replace` deixa `.padrao_de_cima_escopo.*`; ocorrência no ambiente fornecido é [não verificado].

4. **Fila — função com parâmetro `raiz` mistura árvores.** `decide_pdc(raiz)` usa a raiz explícita para resolvedor, geração e estado, mas `_relato_consistente()` ignora esse parâmetro e lê `hook_stop_fila1.RELATO` global (`bin/disjuntor.py:1370`, `bin/disjuntor.py:1372`, `bin/disjuntor.py:1376`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:707`). A CLI normal alinha por ambiente; consumidor Python que fornece outra raiz pode gravar eventos de outra árvore (`bin/disjuntor.py:67`, `bin/disjuntor.py:1360`, `bin/disjuntor.py:1402`). É preexistente, mas está no escopo de leitores da função tocada (`bin/disjuntor.py:1376`).

5. **Fila — escrita da geração tem validação depois da operação destrutiva e não verifica escrita completa.** `O_TRUNC` ocorre antes de validar arquivo regular e o retorno de `os.write` é ignorado (`bin/disjuntor.py:1487`, `bin/disjuntor.py:1489`, `bin/disjuntor.py:1491`). Escrita parcial faz escopo e geração discordarem, causando recusa posterior; não identifiquei liberação indevida por essa falha isolada, porque o fallback compara igualdade e recusa geração vazia (`bin/disjuntor.py:1501`, `bin/disjuntor.py:1549`). Não há `fsync` entre geração, estado e escopo; comportamento após queda do host é [não verificado] (`bin/disjuntor.py:1491`, `bin/disjuntor.py:281`, `bin/disjuntor.py:1521`).

Mapa:

| Estado | Escritores | Leitores e observação |
|---|---|---|
| `PDC_GERACAO` | `_pdc_geracao_grava`, invocado no callback PDC sob flock (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`). | `_pdc_geracao_lida`, somente no fallback de `estado_pdc` (`bin/disjuntor.py:1499`, `bin/disjuntor.py:1548`); leitor indireto de mtime do Stop não foi migrado (`bin/hook_stop_fila1.py:578`, `bin/hook_stop_fila1.py:585`). |
| `escopo.geracao` | `_pdc_grava_escopo`, após estado gravado e ainda sob flock (`bin/disjuntor.py:1462`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:292`). | `_pdc_escopo_lido` carrega JSON, `estado_pdc` compara com geração (`bin/disjuntor.py:1529`, `bin/disjuntor.py:1549`). |
| Arquivo de estado | Porta produtiva `_com_estado`, com arquivo temporário e `replace` (`bin/disjuntor.py:244`, `bin/disjuntor.py:278`, `bin/disjuntor.py:281`). Também fixtures gravam diretamente, sem geração (`bin/tests/test_padrao_de_cima.sh:103`, `bin/tests/test_padrao_de_cima.sh:565`, `bin/tests/test_padrao_de_cima.sh:576`, `bin/tests/test_padrao_de_cima.sh:620`). | `_com_estado` lê para transição, `ler_estado` lê para consultas/painel (`bin/disjuntor.py:267`, `bin/disjuntor.py:302`, `bin/disjuntor.py:1636`). |
| `pdc.eventos`, `nomeados`, `libera_vistas`, `fotos`, `disparadas` | Callback de `decide_pdc`; não encontrei segundo escritor produtivo no censo textual, mas testes editam diretamente (`bin/disjuntor.py:1385`, `bin/disjuntor.py:1402`, `bin/disjuntor.py:1440`, `bin/disjuntor.py:1445`, `bin/disjuntor.py:1449`, `bin/tests/test_padrao_de_cima.sh:560`, `bin/tests/test_padrao_de_cima.sh:572`). | Decisão, agrupamento, janela e painel (`bin/disjuntor.py:1391`, `bin/disjuntor.py:1304`, `bin/disjuntor.py:1311`, `bin/disjuntor.py:1636`). |

Os demais callbacks de `_com_estado` continuam escritores do **mesmo arquivo**, mas alteram outras chaves e preservam PDC; não precisam incrementar geração quando só muda B1/B3/B4/triagem (`bin/disjuntor.py:481`, `bin/disjuntor.py:831`, `bin/disjuntor.py:908`, `bin/disjuntor.py:1033`, `bin/disjuntor.py:1778`, `bin/disjuntor.py:1792`, `bin/disjuntor.py:2058`, `bin/disjuntor.py:2060`). O painel continua lendo PDC sem geração, mas sua função é apresentar contagem, não liberar ferramenta (`bin/disjuntor.py:1633`, `bin/disjuntor.py:1644`).

A cura de `DISJUNTOR_DESLIGADO` no ramo de exceção está conectada à mesma autoridade `desligado()` usada no caminho normal e no wrapper; não cria juiz concorrente (`bin/disjuntor.py:151`, `bin/disjuntor.py:1600`, `bin/disjuntor.py:2085`, `bin/disjuntor.py:2096`). O caso novo verifica stdout vazio, sem verificar rc ou conteúdo JSON da negativa; qualquer saída antecipada sem stdout também satisfaz essa parte isolada (`bin/tests/test_padrao_de_cima.sh:328`, `bin/tests/test_padrao_de_cima.sh:329`).

## 31/10 21:47 · sessao d420147522b3 · - · - · origem=Ronald (tmux)

Não comprovei, nos fluxos de views/services/signals/models examinados, sequência de produção que altere insumo omitido da chave e volte ao C6 sobre a mesma `PerguntaDisputa` ou mesma `p.celula`; os candidatos abaixo reutilizam objetos somente em leitura, alteram campos da chave ou recarregam as perguntas. (`app/chamados/services/fio.py:222`, `app/chamados/models.py:1420`, `app/chamados/services/validacao.py:359`)

| Fluxo | Sequência e conclusão |
|---|---|
| Render do fio/modal | Carrega `p` com `select_related` e `prefetch_related` → guarda as mesmas instâncias em listas → anexa a `ch.perguntas_disputa` → chama `.viva` → passa a lista ao explicador. Este preserva os objetos e chama `p.estado` e depois `fio_vivo(ps)`. Há reutilização real do memo, sem alteração dos insumos do C6 comprovada nesse trecho. (`app/chamados/services/fio.py:222`, `app/chamados/services/fio.py:239`, `app/chamados/services/fio.py:249`, `app/chamados/services/fio.py:279`, `app/chamados/services/fio.py:308`, `app/chamados/services/explicador.py:124`, `app/chamados/services/explicador.py:133`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:2479`) |
| Gate de fechamento do chamado | `_plano_pergunta_viva` guarda as perguntas em `self._f1_ancoradas`; `_gravar_estado` recupera a lista e chama `_vivas_entre` novamente. O julgamento temporário muda `status_local` e aponta `p.chamado_destino` para `self`; destino e status do destino estão na chave. (`app/chamados/models.py:362`, `app/chamados/models.py:210`, `app/chamados/models.py:218`, `app/chamados/models.py:339`, `app/chamados/models.py:348`, `app/chamados/juizes.py:794`) |
| Desarquivar / toque humano | `ch.reabrir()` chama `ressuscitar_perguntas`; depois `arquivo.levantar` salva e grava a trilha do toque; a view chama `ressuscitar_perguntas` novamente. Entretanto, cada chamada cria queryset novo, copia cada `p`, copia `_state.fields_cache`, substitui a célula pela projeção do julgamento e remove `_pv_memo` antes do C6. Não é prova de reutilização do memo anterior. (`app/chamados/views_arquivar.py:70`, `app/chamados/models.py:1389`, `app/chamados/views_arquivar.py:77`, `app/chamados/services/arquivo.py:208`, `app/chamados/services/arquivo.py:209`, `app/chamados/views_arquivar.py:86`, `app/chamados/models.py:1420`, `app/chamados/models.py:1427`, `app/chamados/models.py:1430`, `app/chamados/models.py:1434`, `app/chamados/models.py:1439`) |
| Responder pelo PWA | C6 sobre `p` → eventual segunda consulta ao C6 **antes** da escrita → core altera resposta, respondida, validação e carimbos da chave → pode criar ausência → retorna sem nova chamada ao C6 sobre o `p` original. As perguntas da absorção são obtidas por novo filtro. (`app/chamados/views.py:1242`, `app/chamados/views.py:1257`, `app/chamados/views.py:1283`, `app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1265`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/materializacao.py:501`, `app/chamados/services/materializacao.py:509`, `app/chamados/services/disputa_emissao.py:1324`) |
| Responder pela API | Carrega e julga `p` → chama o mesmo core → devolve resposta HTTP; não volta ao C6 sobre `p`. (`app/api/views.py:2680`, `app/api/views.py:2683`, `app/api/views.py:2695`, `app/api/views.py:2702`) |
| Resgate pela resposta | Usa o destino já acessível pela pergunta, mas escreve arquivo/silêncio e trilha `resgatar_por_resposta`; essa ação não está entre os três toques humanos reconhecidos pelo C6. (`app/chamados/services/materializacao.py:658`, `app/chamados/services/materializacao.py:665`, `app/chamados/juizes.py:935`) |
| Validar pergunta | Recebe ID e faz `.get()` de nova instância; carimba validação, materializa e atualiza os carimbos com `refresh_from_db`. Não há consulta posterior ao C6 sobre `perg` nessa função. A cauda consulta `perguntas_vivas_de`, que usa queryset com `order_by`; o signal da pergunta transmite somente `disputa_id` e o serviço de becos recarrega disputa e perguntas. (`app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:424`, `app/chamados/services/validacao.py:430`, `app/chamados/services/validacao.py:433`, `app/chamados/services/materializacao.py:1233`, `app/chamados/juizes.py:1130`, `app/chamados/signals.py:195`, `app/chamados/signals.py:201`, `app/chamados/services/becos.py:67`) |
| Desfazer carimbo sem lastro | Recebe perguntas, mas substitui cada recebida por releitura com `select_for_update`; chama C6 e só depois muda `materializada_em`/`via_resolucao`, presentes na chave. (`app/chamados/services/relavra_via.py:213`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:231`, `app/chamados/juizes.py:790`) |
| Relatório de disputas | Prefetch guarda perguntas/célula/destino e `classificar` chama C6 nas perguntas desse cache; o fluxo apresentado somente monta e ordena resultados. (`app/chamados/relatorios_disputa.py:39`, `app/chamados/relatorios_disputa.py:42`, `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:17`, `app/chamados/relatorios_disputa.py:44`, `app/chamados/relatorios_disputa.py:51`) |

O memo não tem prazo baseado em request no corpo executável: ele fica no atributo da instância e é reutilizado enquanto a chave coincidir; a morte com o request é descrita no comentário, não implementada por um encerramento ou limpeza ali. (`app/chamados/juizes.py:809`, `app/chamados/juizes.py:812`, `app/chamados/juizes.py:824`)

_1264 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
