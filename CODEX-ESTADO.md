# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 15:01** · publicado em **31/10/2026 15:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 15:01 · sessao b7922e87983c · E4-D5-PROD · sonda · origem=Code

| Empresa | Competência | Janela | entregue | celulas | lavradas | ate_ontem | Distribuição de status | Referência |
|---|---|---|---|---:|---:|---:|---|---|
| 1 | 10/2026 | 2026-09-21..2026-10-20 | False | 90 | 0 | 90 | `{None: 90}` | [nao verificado] |
| 1 | 11/2026 | 2026-10-21..2026-11-20 | False | 62 | 0 | 20 | `{None: 20}` | [nao verificado] |
| 2 | 10/2026 | 2026-09-21..2026-10-20 | False | 13340 | 0 | 13340 | `{None: 13340}` | [nao verificado] |
| 2 | 11/2026 | 2026-10-21..2026-11-20 | False | 13575 | 143 | 4303 | `{None: 4219, 'ausencia': 3, 'ok': 41, 'folga': 30, 'irregular': 9, 'escala_suspeita': 1}` | [nao verificado] |
| 3 | 10/2026 | 2026-09-21..2026-10-20 | False | 3538 | 0 | 3538 | `{None: 3538}` | [nao verificado] |
| 3 | 11/2026 | 2026-10-21..2026-11-20 | False | 3622 | 21 | 1165 | `{None: 1156, 'folga': 3, 'irregular': 3, 'ok': 3}` | [nao verificado] |
| 4 | 10/2026 | 2026-09-21..2026-10-20 | False | 666 | 0 | 666 | `{None: 666}` | [nao verificado] |
| 4 | 11/2026 | 2026-10-21..2026-11-20 | False | 713 | 4 | 230 | `{None: 228, 'folga': 2}` | [nao verificado] |

DRY:

| Empresa | Competência | ja_lavrado | completaveis | completados | fila_cartorio | Referência |
|---|---|---:|---:|---:|---:|---|
| 1 | 10/2026 | 0 | 0 | 0 | 90 | [nao verificado] |
| 1 | 11/2026 | 0 | 0 | 0 | 20 | [nao verificado] |
| 2 | 10/2026 | 0 | 13340 | 0 | 0 | [nao verificado] |
| 2 | 11/2026 | 84 | 4145 | 0 | 74 | [nao verificado] |
| 3 | 10/2026 | 0 | 3538 | 0 | 0 | [nao verificado] |
| 3 | 11/2026 | 9 | 1129 | 0 | 27 | [nao verificado] |
| 4 | 10/2026 | 0 | 666 | 0 | 0 | [nao verificado] |
| 4 | 11/2026 | 2 | 221 | 0 | 7 | [nao verificado] |

**ERROS=0** [nao verificado]

## 31/10 15:00 · sessao e828f50b15de · V3-RETOMA · verificador · origem=Code

**BLOQUEIA — a HE principal e seu split noturno podem usar CCTs de datas diferentes.** A principal recebe a CCT diária, mas o split conserva as extras calculadas pelo motor da fatia (`app/ponto/calculador/alimentacao.py:514`, `:535`; `app/ponto/services/fechamento.py:711`).

1. **BLOQUEIA — HE100 noturna positiva com HE100 total zero.** O fechamento escolhe a CCT em `fatia_ini`; o motor grava a dobra nos períodos; a alimentação copia esses minutos para `he_pontas`, sem reconciliá-los com a nova CCT diária (`app/ponto/services/fechamento.py:711`; `app/ponto/motor_calculo_v2.py:2747`; `app/ponto/calculador/alimentacao.py:92–96`).

   Se a CCT de dobra vencer dentro da mesma fatia, um feriado posterior pode perder a HE100 principal e conservar sua parcela noturna. Contraprova em memória com as funções da cópia: plantão 23h–05h, trabalhado=previsto=360, `feriado_em_dobra=False`, `he_pontas=[(entrada, saída, 0, 360)]` resulta em **HE100=0 e HE100 noturna=6** (`app/ponto/calculador/regras.py:769`, `:775–810`, `:822–834`). Ocorrência em produção: **[nao verificado]**.

   Os testes novos não exercitam essa composição: praça/vigência usam períodos falsos com extras zeradas e verificam somente HE100 de feriado; o teste noturno injeta os 360 minutos diretamente (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:41`, `:69–82`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:44–48`).

2. **NAO-BLOQUEIA — aumento de consultas sem cache equivalente nesse caminho.** Agora são **D chamadas de `get_motor_cct` por colaborador/janela**, sendo D os dias com minutos válidos, unidos aos dias bloqueados, dentro da janela. Não são automaticamente todos os dias civis do mês (`app/ponto/calculador/alimentacao.py:472–480`, `:514`). Antes eram **T chamadas**, pelos templates distintos, conforme o trecho removido do anexo (`delta_7e0f.diff`, hunk de `alimentacao.py:382`).

   Assim, com um template e 30 dias visitados, passa de **1 para 30** chamadas nessa porta. No fechamento completo, a contagem passa de **F+O+T para F+O+D**: F motores das fatias e O processamento adicional de órfãos (`app/ponto/services/fechamento.py:263`, `:711`, `:793`, `:1239`). Contagens reais por colaborador/mês e impacto de latência: **[nao verificado]**.

   Cada chamada relê o switch; com ele ativo, recarrega vínculo e, para empresa CCT, consulta aplicações. As escalas/células já disponíveis na porta não são repassadas à fábrica (`app/core/regua_cct.py:444–458`, `:329`, `:366`; `app/escala/alimentacao.py:298–300`; `app/ponto/calculador/alimentacao.py:400`, `:426–438`). `_FONTE_DO_MOTOR` guarda somente metadados por instância; não evita essa resolução (`app/core/regua_cct.py:488–506`).

3. **NAO-BLOQUEIA — quatro testes passariam sem a cura, por inspeção estática:** [nao verificado]

   - `test_a_dobra_continua_paga`: verifica a atribuição que já ocorria antes do retorno removido (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:45–46`; `app/ponto/calculador/regras.py:780–785`).
   - `test_MORDE_NACIONAL_dobra_no_intermitente`: o nacional já estava no conjunto entregue ao código antigo (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:120–122`).
   - `test_MORDE_dia_FORA_da_praca_nao_dobra`: o antigo `ini=21/09` antecede ambos os vínculos, iniciados em outubro; sem vínculo, não encontra a aplicação específica da praça e já produz zero (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:72`, `:171–185`; `app/core/regua_cct.py:329–330`, `:256–257`).
   - `test_MORDE_dia_DENTRO_da_vigencia_dobra`: a convenção da empresa já estava vigente em 21/09; a leitura antiga já concedia a dobra (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:72`, `:199–214`; `app/core/regua_cct.py:261–264`).

   Execução da suíte contra o código anterior: **[nao verificado]**. [nao verificado]

4. **NAO-BLOQUEIA — cobertura incompleta dos demais efeitos.** O caso novo trabalha 360 minutos contra 420 previstos; portanto não protege o `elif` contra soma indevida de HE50 quando houver excedente positivo. Também não cobre saída antecipada, HE50 noturna ou parâmetros noturnos mudando com praça/vigência (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:38`, `:69–82`; `app/ponto/calculador/regras.py:794`, `:831`, `:851–861`; `app/ponto/calculador/alimentacao.py:515–537`).

Os **chamadores de produção** são: `regras.do_dia` ← `rubricas_por_dia`, em `app/ponto/calculador/alimentacao.py:523`; `rubricas_por_dia` ← fechamento, em `app/ponto/services/fechamento.py:1239`, e comparador, em `app/ponto/management/commands/diff_calculador.py:417`. O fechamento entrega os valores à lavratura como versão **`oraculo`**, que substitui os campos correspondentes; os filtros de exportação examinados continuam selecionando `motor` (`app/ponto/services/fechamento.py:1256–1262`; `app/ponto/services/dia_pago.py:320–328`; `app/folha/porta_export.py:284`).

Quanto ao comportamento além dos asserts, retirar o retorno mantém a dobra substituindo o excedente e permite executar **ambos os splits noturnos, adicional noturno, atraso, saída antecipada e detalhes de cadastro incompatível** (`app/ponto/calculador/regras.py:779–861`). A seleção diária altera dobra, prorrogação pós-05h e hora reduzida conforme praça/vigência; a exclusão de pontualidade continua sendo lida da família do motor (`app/ponto/calculador/alimentacao.py:411–412`, `:514–538`; `app/core/regua_cct.py:261–262`, `:329–330`).

O calendário agora é o conjunto devolvido pelo motor: os motores comuns conservam o conjunto recebido; o intermitente o esvazia. **Uma data ausente do conjunto ainda consulta o calendário nacional**, mesmo quando o conjunto não está vazio (`app/ponto/motor_calculo_v2.py:740`, `:901–905`, `:3495`; `app/core/feriados.py:91–95`). Isso não recalcula feriados municipais pela praça histórica do dia: o fechamento continua montando o conjunto pelo posto do colaborador, e `folga_e_feriado` continua usando esse conjunto original (`app/ponto/services/fechamento.py:621–623`; `app/core/feriados.py:113–114`; `app/ponto/calculador/alimentacao.py:530–531`).

## 31/10 14:59 · sessao f17443fe0e70 · REPROVA-E-DESENHO-CODEX · verificador · origem=Code

**REPROVADO: o código cobre o fluxo sequencial básico, mas não fecha o contrato.** Há chamadas que passam sem atualizar o B3, bloqueios indevidos e voltas antigas que reaparecem após reset. (`bin/disjuntor.py:940`, `bin/disjuntor.py:1226`, `bin/disjuntor.py:852`, `bin/disjuntor.py:883`)

Execução do selo e ocorrência dos cenários em produção: **[nao verificado]**. Não executei o selo porque ele cria, altera e apaga arquivos. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

| Item do contrato | Veredito sobre o código |
|---|---|
| **1. Três chamadas distintas PROCEDENTES por obra** | **Parcial.** O leitor deduplica por identidade, separa obras e usa limiar três; entretanto, o disparo depende de alguém executar `decide_b3`, e a porta direta do wrapper não executa essa decisão. (`bin/disjuntor.py:783`, `bin/disjuntor.py:805`, `bin/disjuntor.py:859`, `bin/disjuntor.py:940`) |
| **2. Não triada ou DESCARTADO não conta** | **Cumpre com a fonte íntegra.** Vale o último veredito; somente PROCEDENTES entram na lista recalculada. Falta ou corrupção da fonte tem os defeitos descritos abaixo. (`bin/disjuntor.py:794`, `bin/disjuntor.py:805`, `bin/disjuntor.py:811`, `bin/disjuntor.py:854`) |
| **3. Negar próximo Opus final e Codex verificador até liberação válida; outros papéis passam** | **Não cumpre integralmente.** Existem as duas guardas e o reconhecedor compartilhado, mas a porta direta consulta estado atrasado, o parser pode negar outro papel e o PreToolUse falha aberto. (`bin/disjuntor.py:625`, `bin/disjuntor.py:940`, `bin/disjuntor.py:1310`, `bin/disjuntor.py:1320`, `bin/disjuntor.py:1359`) |
| **4. Escritor único; identidade do wrapper; somente verificador** | **Parcial.** Há uma implementação de escrita da triagem e recusa estrutural de identidade ausente, papel diferente e SHA divergente. A escrita não participa da trava do estado, e os campos não comprovam autoria pelo wrapper. (`bin/disjuntor.py:752`, `bin/disjuntor.py:776`, `bin/disjuntor.py:788`) |
| **5. Liberação/rearme zeram e impedem recontagem das antigas** | **Não cumpre integralmente.** Arquiva as PROCEDENTES atuais; perde a identidade de voltas anteriormente contadas que estejam descartadas ou ausentes da fonte durante o reset. (`bin/disjuntor.py:854`, `bin/disjuntor.py:883`, `bin/disjuntor.py:921`) |

Os caminhos de falha encontrados são estes; os cenários descrevem consequências do código, sem afirmar ocorrência em produção. (`bin/disjuntor.py:770`, `bin/disjuntor.py:814`, `bin/disjuntor.py:940`)

1. **A quarta chamada direta pode passar após três PROCEDENTES.** Três `--triagem` gravam o resultado, mas não disparam o B3. Se a operação seguinte chamar o wrapper diretamente, com ambiente herdado ou por uma forma não reconhecida pelo parser, `--b3-parada` encontra o estado anterior livre e retorna zero. Isso também acontece se o estado estiver ausente, mesmo existindo triagens. (`bin/disjuntor.py:788`, `bin/disjuntor.py:791`, `bin/disjuntor.py:947`, `bin/disjuntor.py:956`; `bin/codex_pergunta.sh:54`)

2. **A chamada direta continua negada após uma linha válida de liberação.** Com a obra disparada, escrever um DESENHO/ERRO-BOBO novo e válido não basta para essa porta: ela não lê o RELATO. Sem `decide_b3` intermediário, o wrapper continua recusando. (`bin/disjuntor.py:877`, `bin/disjuntor.py:940`, `bin/disjuntor.py:953`; `bin/codex_pergunta.sh:54`)

3. **O parser pode negar outro papel ou outra obra.** Com OBRA-X parada, o comando abaixo executaria com papel `leitura`, mas o parser pega a primeira atribuição e o B3 o nega. Uma ocorrência dentro de `DUMMY="CODEX_PAPEL=verificador"` também pode prevalecer. Para `CODEX_OBRA="O9 (C)"`, o parser lê somente `O9`, podendo negar pela obra errada. (`bin/disjuntor.py:1226`, `bin/disjuntor.py:1228`, `bin/disjuntor.py:1310`)

   ```bash
   CODEX_OBRA=OBRA-X CODEX_PAPEL=verificador CODEX_PAPEL=leitura bin/codex_pergunta.sh h q r
   ```

4. **O próximo Opus final passa quando a decisão levanta exceção.** Estado corrompido, trava ocupada ou triagem sem permissão de leitura impedem a decisão; `main_pretool` registra FALHA e termina sem negar. O Stop também omite o recado B3 nesse caminho. A porta do wrapper, especificamente para estado ilegível, recusa: são comportamentos diferentes. (`bin/disjuntor.py:257`, `bin/disjuntor.py:264`, `bin/disjuntor.py:798`, `bin/disjuntor.py:1359`, `bin/disjuntor.py:949`; `bin/hook_stop_fila1.py:1056`)

5. **A classificação textual pode pular a guarda do Opus.** Um prompt de leitura final da OBRA-X que inclua a citação antiga “nao e a leitura final” faz `_eh_final` retornar falso. Uma obra não reconhecida também pula a consulta B3. (`bin/disjuntor.py:113`, `bin/disjuntor.py:498`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1320`)

6. **Uma volta já contada pode contar novamente após liberação ou rearme.** Cenário: `a,b,c` PROCEDENTES disparam; `a` vira DESCARTADO; uma decisão recalcula `[b,c]`; a liberação arquiva somente `[b,c]`. Depois, `a` volta a PROCEDENTE e, com apenas duas novas `d,e`, dispara novamente como `[a,d,e]`. O rearme sofre o mesmo problema porque suas pendências também contêm somente PROCEDENTES atuais. (`bin/disjuntor.py:851`, `bin/disjuntor.py:854`, `bin/disjuntor.py:883`, `bin/disjuntor.py:921`)

7. **O rearme não alcança triagens ainda sem registro B3 da obra.** Três PROCEDENTES gravadas antes de qualquer varredura, seguidas de `--rearma-b3`, encontram `r` ausente: o comando recusa o rearme e não arquiva aquelas voltas. Elas podem disparar na primeira decisão posterior. (`bin/disjuntor.py:917`, `bin/disjuntor.py:919`, `bin/disjuntor.py:859`, `bin/disjuntor.py:1551`)

8. **Fonte ausente apaga contagem conhecida e pode permitir recontagem.** Com duas voltas conhecidas, perder `triagem.log` faz a decisão substituir a lista por vazia. Se a obra já estava disparada, liberar durante essa ausência arquiva uma lista vazia; restaurar a trilha permite que as mesmas três voltas antigas disparem novamente. (`bin/disjuntor.py:800`, `bin/disjuntor.py:854`, `bin/disjuntor.py:869`, `bin/disjuntor.py:883`)

9. **Fonte truncada ou corrompida produz contagem silenciosamente errada.** Linhas inválidas são ignoradas. Uma última linha DESCARTADO truncada deixa prevalecer o PROCEDENTE anterior, podendo disparar indevidamente; perder linhas PROCEDENTE subconta. (`bin/disjuntor.py:798`, `bin/disjuntor.py:803`, `bin/disjuntor.py:805`)

10. **A triagem é gravada fora da trava do estado.** Duas triagens iguais concorrentes podem ambas observar ausência e acrescentar duas linhas. Isso quebra a idempotência da escrita, mas **não duplica sozinho a contagem**, porque o leitor deduplica pela identidade. O JSON B3 é gravado sob `flock`; a escrita desprotegida identificada é `triagem.log`. (`bin/disjuntor.py:252`, `bin/disjuntor.py:273`, `bin/disjuntor.py:784`, `bin/disjuntor.py:788`, `bin/disjuntor.py:805`)

11. **Chamadas concorrentes no mesmo caminho podem misturar resposta e identidade.** A termina como verificador; B, papel leitura, sobrescreve o mesmo arquivo; A então calcula o SHA do conteúdo de B e grava seu próprio sidecar. A triagem aceita a resposta de leitura com identidade de verificador. Não há exclusão por caminho envolvendo resposta e sidecar. (`bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:184`, `bin/codex_pergunta.sh:186`, `bin/codex_pergunta.sh:187`; `bin/disjuntor.py:779`, `bin/disjuntor.py:781`)

12. **Uma tentativa negada pode destruir a identidade da resposta anterior.** Reutilizar o caminho numa obra parada apaga `.id` antes de consultar B3; a tentativa é negada e a resposta anterior, ainda existente, deixa de ser triável. Além disso, a falha do `rm` não interrompe o script, podendo deixar um sidecar antigo. (`bin/codex_pergunta.sh:27`, `bin/codex_pergunta.sh:52`, `bin/codex_pergunta.sh:54`; `bin/disjuntor.py:776`)

13. **`mtime` não garante que a resposta pertence à chamada.** Uma resposta legítima com `mtime` igual ao cabeçalho não recebe identidade, apesar de rc zero. Condicionalmente, uma resposta antiga com timestamp futuro passa em `-nt` se a execução retornar zero sem substituí-la, recebendo identidade nova. Ocorrência dessas condições: **[nao verificado]**. (`bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:141`, `bin/codex_pergunta.sh:184`, `bin/codex_pergunta.sh:190`)

14. **Os campos do sidecar não atestam sua origem.** Fabricar os campos exigidos com SHA correto satisfaz a validação; alterar `papel` de leitura para verificador mantém o SHA da resposta válido. O próprio selo fabrica identidades dessa forma. Portanto, a exigência literal de identidade *gravada pelo wrapper* excede o que a checagem demonstra. (`bin/disjuntor.py:752`, `bin/disjuntor.py:759`, `bin/disjuntor.py:779`, `bin/disjuntor.py:781`; `bin/tests/test_disjuntor.sh:1129`)

15. **A leitura apresenta zeros compatíveis com “verde” sem ter lido a trilha.** `ler` inicia disparos, negações e liberações zerados; ignora `OSError` na trilha; `linha` apresenta `B3=0` e os demais zeros. Estado ausente também resulta em zero para `b3_sem_obra`; estado ilegível, nesse campo específico, preserva “nao sei”. (`bin/disjuntor.py:1403`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1531`, `bin/disjuntor.py:295`, `bin/disjuntor.py:1469`)

A cobertura de `b3_codex` e dos mutantes é parcial; os resultados de execução permanecem **[nao verificado]**. (`bin/tests/test_disjuntor.sh:1125`, `bin/tests/test_disjuntor.sh:1272`)

| Item | Casos e mutantes existentes | O que fica sem caso que morde |
|---|---|---|
| **1** | Duas passam, terceira dispara; obras separadas; mesma identidade triada cinco vezes. Mutantes `sem-codex`, `limiar-2`, `por-achado`. (`bin/tests/test_disjuntor.sh:1147`, `bin/tests/test_disjuntor.sh:1151`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1272`, `bin/tests/test_disjuntor.sh:1282`) | Uma chamada real produzindo exatamente uma identidade: as respostas e sidecars são fabricados pelo teste. (`bin/tests/test_disjuntor.sh:1129`) |
| **2** | DESCARTADO, não triado e PROCEDENTE→DESCARTADO; mutantes `conta-descartado` e `acumula`. (`bin/tests/test_disjuntor.sh:1140`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1273`, `bin/tests/test_disjuntor.sh:1284`) | Fonte ausente, truncada ou ilegível. Os negativos usam ausência do recado B3, podendo aceitar uma falha aberta. (`bin/tests/test_disjuntor.sh:1135`, `bin/tests/test_disjuntor.sh:1142`, `bin/tests/test_disjuntor.sh:1180`) |
| **3** | Negação de Opus/Codex, leitura permitida, Stop e DESENHO liberando; mutantes `volta-nao-nega` e `porta-sempre-livre`. (`bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1159`, `bin/tests/test_disjuntor.sh:1164`, `bin/tests/test_disjuntor.sh:1275`, `bin/tests/test_disjuntor.sh:1287`) | **A porta real do wrapper.** O teste chama somente Python, depois de `_parada` já materializar o disparo; não testa wrapper como primeira operação após terceira triagem ou após liberação. (`bin/tests/test_disjuntor.sh:1191`, `bin/tests/test_disjuntor.sh:1192`) |
| **4** | Recusa sem sidecar, outro papel, veredito inválido e SHA divergente; mutantes `sem-sidecar` e `sem-sha`. (`bin/tests/test_disjuntor.sh:1137`, `bin/tests/test_disjuntor.sh:1173`, `bin/tests/test_disjuntor.sh:1276`, `bin/tests/test_disjuntor.sh:1286`) | Produção/autoria da identidade pelo wrapper e concorrência da escrita. Os mutantes copiam apenas `.py`, excluindo o shell. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1207`) |
| **5** | Liberação seguida de nova volta isolada; reclassificação depois da liberação; rearme com PROCEDENTES pendentes. Mutantes retiram arquivamento na liberação e pendências no rearme. (`bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1281`, `bin/tests/test_disjuntor.sh:1285`) | Reclassificação **antes** do reset, fonte ausente durante reset e rearme sem registro B3 prévio. (`bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1183`, `bin/tests/test_disjuntor.sh:1186`) |

**O buraco decisivo do selo é o item 3 na integração com o wrapper:** retirar `codex_pergunta.sh:54` fica fora do percurso exercitado. O item 4 também não tem prova da identidade realmente emitida pelo wrapper, e o item 5 não tem o ordenamento que ressuscita voltas antigas. ERRO-BOBO e validação detalhada do topo têm casos gerais em `b3`/`b3_topo`, mas isso não cobre essas lacunas de integração e reset. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1192`, `bin/tests/test_disjuntor.sh:1207`, `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:700`)

## 31/10 14:59 · sessao e59b691f8ed7 · - · - · origem=Ronald (tmux)

- **BLOQUEIA — HE noturna usa CCT de outra data.** O fechamento monta o motor em `fatia_ini` (`app/ponto/services/fechamento.py:711`); esse motor grava a dobra no período (`app/ponto/motor_calculo_v2.py:2747`). A alimentação copia suas HE50/100 (`app/ponto/calculador/alimentacao.py:92`) e as entrega intactas (`app/ponto/calculador/alimentacao.py:535`). Entretanto, os totais passam a usar a CCT diária (`app/ponto/calculador/alimentacao.py:514`), enquanto os splits continuam usando as extras antigas (`app/ponto/calculador/regras.py:822`). Reprodução em memória com funções extraídas por AST: plantão 23h–05h, trabalhado=previsto=360, CCT diária sem dobra, período antigo com 360 minutos de HE100 ⇒ `HE100=0`, `HE100_noturna=6`; os ramos responsáveis estão em `app/ponto/calculador/regras.py:775` e `app/ponto/calculador/regras.py:834`. Ocorrência em produção: **[nao verificado]**.

- **NAO-BLOQUEIA — cobertura incompleta da retirada do `return`.** Além dos três efeitos exercitados, o fluxo agora calcula saída antecipada e publica detalhes de cadastro incompatível (`app/ponto/calculador/regras.py:851`, `app/ponto/calculador/regras.py:858`), além de HE50 noturna (`app/ponto/calculador/regras.py:831`). O cenário novo tem trabalhado=360 e previsto=420 (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:38`), portanto não testa dobra com excedente positivo nem protege a exclusão da HE50 pelo `elif` (`app/ponto/calculador/regras.py:794`).

- **NAO-BLOQUEIA — teste que não morde.** `test_a_dobra_continua_paga` verifica apenas a dobra, já atribuída antes do antigo retorno; passaria sem retirar esse retorno (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:45`; `app/ponto/calculador/regras.py:780`).

O chamador de produção de `regras.do_dia` é `app/ponto/calculador/alimentacao.py:523`; `rubricas_por_dia` é chamado pela lavratura do fechamento (`app/ponto/services/fechamento.py:1239`, versão `oraculo` em `:1262`) e pelo comando comparador (`app/ponto/management/commands/diff_calculador.py:417`).

## 31/10 14:58 · sessao 95de571d2afe · - · - · origem=Ronald (tmux)

- **BLOQUEIA — CCT diária e split noturno podem usar bases diferentes.** A HE 100 principal agora usa CCT do dia (`app/ponto/calculador/alimentacao.py:514`, `:531`), mas as quantidades usadas no split vêm dos períodos já calculados pelo motor (`app/ponto/calculador/alimentacao.py:92`, `:93`, `:96`; `app/ponto/calculador/regras.py:822`, `:827`, `:834`). O fechamento mantém um motor por fatia (`app/ponto/services/fechamento.py:263`, `:270`). Assim, numa fatia cuja CCT de dobra vence durante o período, um feriado noturno posterior ao vencimento pode ficar com HE100 principal zero e HE100 noturna positiva; ocorrência em produção **[nao verificado]**. Os testes novos não cobrem essa composição: usam períodos falsos com extras zeradas (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:69`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:44`) e leem apenas HE100 de feriado (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:80`, `:82`).

- **NAO-BLOQUEIA — quatro testes novos passam sem a cura**, por inspeção estática; execução contra versão anterior **[nao verificado]**: [nao verificado]
  - `test_a_dobra_continua_paga`: a atribuição de dobra já precedia o `return` removido; verifica somente manutenção dos 6h (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:45`; `app/ponto/calculador/regras.py:780`, `:785`).
  - `test_MORDE_NACIONAL_dobra_no_intermitente`: o nacional já está no conjunto fornecido ao código antigo (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:120`, `:121`).
  - `test_MORDE_dia_FORA_da_praca_nao_dobra`: a data antiga consultada era 21/09, anterior aos vínculos iniciados em outubro; sem vínculo, não há praça nem aplicação específica, portanto zero já satisfaz a assertiva (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:72`, `:171`, `:174`, `:183`; `app/core/regua_cct.py:329`, `:330`; `app/core/regua_cct.py:257`).
  - `test_MORDE_dia_DENTRO_da_vigencia_dobra`: a convenção global já está vigente em 21/09; a leitura antiga já concede dobra ao dia dentro da vigência (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:72`, `:199`, `:200`, `:212`; `app/core/regua_cct.py:261`, `:262`).

- **NAO-BLOQUEIA — cobertura não demonstra os outros parâmetros da CCT diária.** Todos os casos de praça/vigência usam jornadas 07–19 e inspecionam só dobra; mudanças de prorrogação pós-05h e hora reduzida não são verificadas nesses cenários (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:69`, `:80`, `:82`; `app/ponto/calculador/alimentacao.py:515`, `:516`, `:537`). O teste noturno separado injeta explicitamente 360 minutos de HE100, sem testar sua obtenção pelo fechamento (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:41`).

## 31/10 14:58 · sessao 68c67012b951 · - · - · origem=Ronald (tmux)

- **NAO-BLOQUEIA — aumento de consultas por dia.** A porta faz **D chamadas** de `get_motor_cct` por colaborador/janela, sendo D o número de dias presentes em `v.minutos` unido aos bloqueados, filtrados pela janela; não são necessariamente todos os dias do mês. Antes eram **T chamadas**, uma por template distinto, conforme o diff fornecido. Evidências: `app/ponto/calculador/alimentacao.py:472`, `:474`, `:476`, `:477`, `:479`, `:514`, `:400`.

- **Não há cache equivalente no caminho da fábrica.** Cada chamada consulta `ParametroSistema`; com switch ativo, resolve novamente o vínculo e, para empresa CCT, consulta aplicações de convenção. A porta já tem escalas e células, mas não as repassa à fábrica. Evidências: `app/core/regua_cct.py:444`, `:458`, `:329`, `:366`, `:253`; `app/escala/alimentacao.py:284`, `:298`, `:300`, `:43`, `:116`; `app/ponto/calculador/alimentacao.py:426`, `:438`, `:400`.

- **Outros caches não substituem esse trabalho:** `_FONTE_DO_MOTOR` guarda metadados por instância, e os memos do motor guardam previsto/escala certa. Evidências: `app/core/regua_cct.py:488`, `:494`, `:506`; `app/ponto/motor_calculo_v2.py:1170`, `:2223`.

- **Total no fechamento:** `F + O + D` chamadas, com F fatias efetivamente processadas e O igual a uma chamada adicional quando existe processamento de órfãos; antes, `F + O + T`. A parte D só ocorre na lavratura normal/backfill e competência ≥10/2026; `somente_leitura=True` puro não entra nela. Evidências: `app/ponto/services/fechamento.py:263`, `:711`, `:793`, `:1159`, `:1233`, `:1239`. Contagens reais de F/T/D, tempo e impacto em produção: **[nao verificado]**.

- **Chamadores diretos de produção:** fechamento mensal e comando de comparação. O fechamento entrega o resultado à lavratura da versão `oraculo`; o comando compara os resultados quando recebeu insumos da autoridade. Evidências: `app/ponto/services/fechamento.py:1239`, `:1245`, `:1256`, `:1262`; `app/ponto/management/commands/diff_calculador.py:417`, `:424`.

- **Entradas transitivas usuais do fechamento:** botão de recalcular, comando de recalcular, backfill de dias pagos e recálculo por evento. Evidências: `app/ponto/views.py:1253`; `app/ponto/management/commands/recalcular_fechamento.py:118`; `app/ponto/management/commands/lavrar_dias_pagos.py:106`; `app/ponto/services/fechamento.py:1778`.

- **Quem lê o resultado:** `dia_pago.lavrar` substitui campos pelos valores emitidos e registra as recusas; a versão é fornecida pelo fechamento como `oraculo`. Os filtros de exportação examinados continuam lendo `motor`. Evidências: `app/ponto/services/dia_pago.py:320`, `:327`, `:328`, `:329`, `:331`; `app/ponto/services/fechamento.py:1262`; `app/folha/porta_export.py:284`, `:589`.

## 31/10 14:56 · sessao 5d04f0795ca4 · - · - · origem=Ronald (tmux)

Conformidade parcial: o JSON B3 é alterado e persistido sob `flock`, mas a triagem não participa da mesma trava e a porta do wrapper lê somente o estado persistido. `bin/disjuntor.py:236`, `bin/disjuntor.py:252`, `bin/disjuntor.py:273`, `bin/disjuntor.py:784`, `bin/disjuntor.py:940`.

Falhas concretas:

- **Terceira triagem sem decisão intermediária:** três voltas PROCEDENTE são gravadas; chamada seguinte usa papel/obra herdados ou chama wrapper diretamente. A triagem não decide B3, e `b3_parada` não lê a triagem: estado anterior livre produz rc 0 e permite a quarta chamada. `bin/disjuntor.py:788`, `bin/disjuntor.py:791`, `bin/disjuntor.py:940`, `bin/disjuntor.py:956`, `bin/disjuntor.py:1226`, `bin/disjuntor.py:1310`.
- **Liberação ainda não processada:** obra disparada recebe DESENHO/ERRO-BOBO válido novo; wrapper chamado diretamente continua negando porque a porta não examina RELATO nem executa a decisão. `bin/disjuntor.py:940`, `bin/disjuntor.py:953`; reconhecimento/liberação só em `bin/disjuntor.py:877`.
- **Opus liberado por erro:** estado corrompido, trava ocupada ou triagem ilegível levantam exceção; `main_pretool` registra FALHA e permite a ferramenta. A porta do wrapper recusa estado ilegível, portanto a falha aberta aqui é especialmente do próximo opus. `bin/disjuntor.py:257`, `bin/disjuntor.py:264`, `bin/disjuntor.py:798`, `bin/disjuntor.py:1323`, `bin/disjuntor.py:1359`.
- **Classificação do opus contornável por texto:** prompt de leitura final da OBRA-X que também cita “nao e a leitura final” faz `_eh_final` retornar falso; nenhuma consulta B3 ocorre. Obra não reconhecida também pula a consulta. `bin/disjuntor.py:113`, `bin/disjuntor.py:498`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1320`.
- **Volta temporariamente descartada ressuscita após liberação:** a,b,c PROCEDENTE disparam; a vira DESCARTADO; decisão recalcula `[b,c]`; nova linha válida libera e arquiva só `[b,c]`; a volta a PROCEDENTE e soma com apenas duas novas d,e para disparar novamente. `bin/disjuntor.py:851`, `bin/disjuntor.py:854`, `bin/disjuntor.py:859`, `bin/disjuntor.py:883`.
- **Mesmo defeito no rearme:** após decidir com a descartada, o rearme arquiva somente listas atuais e pendências PROCEDENTE; a não é arquivada e volta depois. `bin/disjuntor.py:921`, `bin/disjuntor.py:922`, `bin/disjuntor.py:924`. O selo faz D→P depois da liberação, portanto não cobre esse ordenamento. `bin/tests/test_disjuntor.sh:1169`.
- **Triagem ausente apaga contagem conhecida:** arquivo ausente vira `{}`; próxima decisão substitui a lista conhecida por vazia. Com duas voltas anteriormente contadas e trilha perdida, falta a base para disparar na terceira. `bin/disjuntor.py:800`, `bin/disjuntor.py:854`.
- **Liberar com trilha ausente permite recontagem:** disparado continua verdadeiro, mas a contagem passa a vazia; liberar arquiva lista vazia; quando a trilha reaparece, antigas voltas voltam a contar. `bin/disjuntor.py:854`, `bin/disjuntor.py:869`, `bin/disjuntor.py:883`.
- **Linha inválida é silenciosamente ignorada:** uma última linha DESCARTADO truncada mantém PROCEDENTE anterior, possibilitando disparo indevido; linhas PROCEDENTE corrompidas subcontam. `bin/disjuntor.py:798`, `bin/disjuntor.py:803`.
- **Fonte fora da trava:** duas triagens simultâneas iguais podem ler ausência e acrescentar duas linhas. Isso quebra a idempotência da escrita, mas não duplica sozinho a contagem, pois o leitor deduplica por volta. `bin/disjuntor.py:784`, `bin/disjuntor.py:788`, `bin/disjuntor.py:805`.
- **Painel imprime zero sem ter lido a trilha:** contadores começam zerados; OSError em `disjuntor.log` é ignorado; saída apresenta `B3=0`, negações e liberações zero. `bin/disjuntor.py:1403`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1531`. Estado B3 ilegível preserva “nao sei” para `b3_sem_obra`, mas estado ausente vira zero. `bin/disjuntor.py:295`, `bin/disjuntor.py:1469`.

O Stop também omite o recado B3 quando a decisão levanta exceção; ele não transforma esse erro em recado de estado desconhecido. `bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:1056`, `bin/hook_stop_fila1.py:1061`, `bin/hook_stop_fila1.py:1297`.

Não identifiquei escrita do JSON B3 fora da trava; a escrita desprotegida identificada é a fonte `triagem.log`. `bin/disjuntor.py:272`, `bin/disjuntor.py:788`, `bin/disjuntor.py:906`, `bin/disjuntor.py:936`.

## 31/10 14:55 · sessao b3f98c69fbd0 · - · - · origem=Ronald (tmux)

Achados do wrapper e integração, por inspeção:

- **Bloqueio direto usa estado atrasado.** O wrapper chama apenas `b3_parada`, que lê `disparado`, sem consumir triagens nem examinar liberação no RELATO. Três PROCEDENTES sem um `decide_b3` intermediário deixam a chamada direta passar; DESENHO válido sem decisão intermediária deixa a chamada direta negada. (`bin/codex_pergunta.sh:53-54`; `bin/disjuntor.py:940-956`)
- **Janela entre autorização e execução.** A checagem ocorre antes da preparação e da espera pelo slot. Uma chamada autorizada pode esperar; outra decisão dispara o B3; a primeira finalmente executa Codex sem consultar novamente. (`bin/codex_pergunta.sh:54`; `bin/codex_pergunta.sh:143-164`)
- **Outro papel pode ser negado pelo parser.** `CODEX_OBRA=O CODEX_PAPEL=verificador CODEX_PAPEL=leitura bin/codex_pergunta.sh h q r` é classificado como verificador, pois `re.search` pega a primeira ocorrência, enquanto o shell usa a última atribuição. Também ocorre com `DUMMY="CODEX_PAPEL=verificador" CODEX_OBRA=O CODEX_PAPEL=leitura ...`. Se O estiver parada, B3 nega leitura. Confirmado executando apenas as funções extraídas da AST em memória. (`bin/disjuntor.py:1226-1230`; `bin/disjuntor.py:1310-1311`)
- **Algumas formas não varrem antes da porta.** `env CODEX_OBRA=O CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` produz lista vazia no parser; ambiente herdado também não fornece obra/papel ao parser. Nessas formas, só sobra a consulta ao estado atrasado. (`bin/disjuntor.py:1150-1152`; `bin/disjuntor.py:1217-1231`)
- **Uma tentativa negada apaga a identidade anterior.** Reutilizar o caminho de resposta numa obra parada remove o `.id` antes de negar, tornando a resposta anterior ainda não triada impossível de triar por esse caminho. (`bin/codex_pergunta.sh:52-54`; `bin/disjuntor.py:776-778`)
- **Resposta e identidade não têm exclusão mútua por caminho.** Duas chamadas com o mesmo arquivo podem misturar conteúdo e identidade: A verificador termina, B leitura sobrescreve resposta, A calcula SHA do conteúdo de B e grava identidade verificador. A triagem aceita conteúdo de leitura como volta verificador. (`bin/codex_pergunta.sh:163-164`; `bin/codex_pergunta.sh:184-187`; `bin/disjuntor.py:779-783`)
- **`-nt` não demonstra autoria.** Resposta legítima com o mesmo mtime do cabeçalho não ganha identidade; resposta antiga com mtime futuro pode ganhar nova identidade se a chamada retornar zero sem atualizar a resposta. São cenários condicionais, sem ocorrência de produção demonstrada. (`bin/codex_pergunta.sh:125`; `bin/codex_pergunta.sh:141`; `bin/codex_pergunta.sh:184-190`)
- **Identidade textual pode ser fabricada.** A triagem verifica presença dos campos, papel e SHA; não consulta um registro confiável de emissão pelo wrapper. Fabricar `.r.id` com SHA correto satisfaz a validação; o próprio selo faz isso. (`bin/disjuntor.py:752-759`; `bin/disjuntor.py:779-783`; `bin/tests/test_disjuntor.sh:1129-1131`)

**Cobertura:** `b3_codex` fabrica os sidecars, faz chamadas simples no parser e só testa `--b3-parada` depois que `_parada` já materializou o estado. Portanto não exercita a porta real do wrapper, os cenários acima nem liberação por chamada direta. Remover `codex_pergunta.sh:54` deixa todos esses casos do selo inalterados; o mutante `porta-sempre-livre` testa somente a implementação Python. (`bin/tests/test_disjuntor.sh:1129-1135`; `bin/tests/test_disjuntor.sh:1158-1159`; `bin/tests/test_disjuntor.sh:1188-1193`; `bin/tests/test_disjuntor.sh:1287-1289`)

Execução do selo e ocorrências em produção: **[nao verificado]**.

## 31/10 14:55 · sessao 12307a10ccfd · - · - · origem=Ronald (tmux)

Cobertura estática do selo, sem executá-lo: o script cria temporários e altera a árvore fictícia; a execução não foi realizada nesta auditoria somente leitura. `bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:50`. Resultado real do selo: `[nao verificado]`.

| Item | O que tem caso que morde | Lacunas |
|---|---|---|
| 1 — três voltas distintas por obra | Duas passam, terceira dispara; obras separadas; mesma identidade triada cinco vezes grava uma linha. Mutantes retiram Codex, baixam limiar e tornam identidade nova por triagem. `bin/tests/test_disjuntor.sh:1147`, `bin/tests/test_disjuntor.sh:1151`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1272`, `bin/tests/test_disjuntor.sh:1274`, `bin/tests/test_disjuntor.sh:1282`. | A identidade é fabricada pelo próprio teste; não comprova que uma chamada real do wrapper produz exatamente uma volta. `bin/tests/test_disjuntor.sh:1129`. |
| 2 — não triada/descartada não conta | Exercita ambas e PROCEDENTE→DESCARTADO antes da terceira; mutantes contam descartados ou acumulam lista antiga. `bin/tests/test_disjuntor.sh:1140`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1273`, `bin/tests/test_disjuntor.sh:1284`. | Os negativos usam ausência de texto B3; não verificam ausência de FALHA nem sucesso do processo. `bin/tests/test_disjuntor.sh:1135`, `bin/tests/test_disjuntor.sh:1142`, `bin/tests/test_disjuntor.sh:1146`, `bin/tests/test_disjuntor.sh:1180`. |
| 3 — negar Opus e Codex, liberar por topo, outros papéis passam | Exercita negativa de ambos, leitura passando, Stop e DESENHO liberando; mutantes retiram negativa Codex e fazem `b3_parada` sempre livre. `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1158`, `bin/tests/test_disjuntor.sh:1159`, `bin/tests/test_disjuntor.sh:1161`, `bin/tests/test_disjuntor.sh:1164`, `bin/tests/test_disjuntor.sh:1275`, `bin/tests/test_disjuntor.sh:1287`. | A porta real do wrapper nunca é executada; consulta-se apenas Python, com disparo previamente materializado por `_parada`. Não testa Codex como primeira operação após terceira triagem nem wrapper como primeira operação após DESENHO. `bin/tests/test_disjuntor.sh:1188`, `bin/tests/test_disjuntor.sh:1191`, `bin/tests/test_disjuntor.sh:1192`. |
| 4 — escritor único e identidade do wrapper/verificador | Recusa ausência de sidecar, outro papel, veredito inválido e SHA divergente; mutantes retiram sidecar e SHA. `bin/tests/test_disjuntor.sh:1136`, `bin/tests/test_disjuntor.sh:1172`, `bin/tests/test_disjuntor.sh:1276`, `bin/tests/test_disjuntor.sh:1286`. | Sem integração com identidade produzida pelo wrapper e sem concorrência entre triagem e decisão/rearme/liberação; os sidecars são fabricados, e os mutantes copiam apenas `.py`. `bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1207`, `bin/tests/test_disjuntor.sh:1272`. |
| 5 — zerar e não recontar | Liberação seguida de nova volta isolada, reclassificação de volta liberada e rearme incluindo procedentes ainda não varridas; mutantes removem tombstone de liberação e pendentes do rearme. `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1169`, `bin/tests/test_disjuntor.sh:1181`, `bin/tests/test_disjuntor.sh:1281`, `bin/tests/test_disjuntor.sh:1285`. | Não intercala escrita de triagem com reset e não testa rearme antes de existir registro B3 da obra. `bin/tests/test_disjuntor.sh:1182`, `bin/tests/test_disjuntor.sh:1186`; a função recusa obra sem registro. `bin/disjuntor.py:917`. |

Os pontos sem caso que morde são, principalmente, a integração real do **item 3** e a produção/autoria/serialização da triagem do **item 4**: remover a guarda de `codex_pergunta.sh:53` ou sua geração de identidade em `codex_pergunta.sh:184` fica fora do percurso exercitado, pois o caso fabrica identidades e o mecanismo de mutação copia apenas Python. `bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1192`, `bin/tests/test_disjuntor.sh:1207`.

Há uma fragilidade transversal: `_passa` exige stdout vazio **e** nenhuma FALHA nova, mas `_parada ... && return` só verifica ausência do recado B3; uma exceção pode satisfazer esses negativos. `bin/tests/test_disjuntor.sh:98`, `bin/tests/test_disjuntor.sh:1135`, `bin/tests/test_disjuntor.sh:1177`.

ERRO-BOBO e validação detalhada do topo têm cobertura no caso geral `b3`/`b3_topo`, embora não sejam repetidos em `b3_codex`. `bin/tests/test_disjuntor.sh:359`, `bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:700`.

## 31/10 14:31 · sessao c7f1ca21315f · REPROVA-E-DESENHO-CODEX · verificador · origem=Code

**REPROVADO: o contrato não está fechado.** Há disparo com volta já DESCARTADA, chamadas que escapam do bloqueio e rearme que pode ressuscitar contagem anterior. (`bin/disjuntor.py:839`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:912`)

A análise abaixo usa o diff fornecido e a cópia dos arquivos. Correspondência da cópia com o commit, ocorrência em produção e resultado da execução do selo/mutantes: **[nao verificado]**. A suíte escreve e apaga arquivos, portanto sua execução contrariaria a restrição de somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

| Item | Cumpre? | Evidência |
|---|---|---|
| **1. Três chamadas distintas, da mesma obra, PROCEDENTES; N achados = uma volta** | **Parcialmente.** O percurso nominal separa por obra, deduplica identidades e dispara em três. A identidade e sua conservação têm falhas descritas abaixo. | `bin/disjuntor.py:773`, `bin/disjuntor.py:795`, `bin/disjuntor.py:839`, `bin/disjuntor.py:850` |
| **2. Não triada ou DESCARTADA não conta** | **Não.** O filtro da fonte considera o último veredito, mas a contagem persistida nunca retira uma volta reclassificada como DESCARTADO. | `bin/disjuntor.py:801`, `bin/disjuntor.py:844`, `bin/disjuntor.py:849` |
| **3. Negar próximo opus final e Codex verificador; liberar pelo mesmo topo; outros papéis passam** | **Parcialmente.** As duas negativas e a liberação compartilhada existem, mas dependem de reconhecimento textual incompleto e falham abertas em exceções. Há também negativa indevida de outro papel. | `bin/disjuntor.py:868`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:1290`, `bin/disjuntor.py:1329` |
| **4. Escritor único e identidade do wrapper, somente verificador** | **Parcialmente.** A porta `--triagem` recusa ausência de sidecar e papel diferente, mas só confere campos preenchidos; não comprova que a resposta existe nem que aquele sidecar pertence à chamada atual. O append não tem trava. | `bin/disjuntor.py:752`, `bin/disjuntor.py:759`, `bin/disjuntor.py:771`, `bin/disjuntor.py:778`, `bin/disjuntor.py:1515` |
| **5. Liberação/rearme zeram; voltas liberadas não retornam** | **Parcialmente.** Funciona para identidades já incorporadas ao JSON. O rearme não incorpora triagens pendentes antes de zerar, e a liberação pode usar uma fotografia obsoleta da triagem. | `bin/disjuntor.py:821`, `bin/disjuntor.py:842`, `bin/disjuntor.py:874`, `bin/disjuntor.py:912` |

Os caminhos de falha identificados são estes; os cenários descrevem consequências do código, não ocorrências comprovadas em produção. (`bin/disjuntor.py:762`, `bin/disjuntor.py:804`, `bin/disjuntor.py:1263`)

1. **DESCARTADO continua na contagem: disparo e negação indevidos.** A é PROCEDENTE; uma decisão guarda A. Depois A vira DESCARTADO, e B/C são triadas PROCEDENTES. A decisão seguinte conserva A e acrescenta B/C: dispara com três, embora somente duas continuem PROCEDENTES. `_codex_procedentes` recalcula a fonte corretamente; a falha está na união cumulativa de `r['codex']`. (`bin/disjuntor.py:801`, `bin/disjuntor.py:839`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`)

2. **Codex verificador passa com a obra parada por diferença entre shell real e parser.** Com B3 de OBRA-C disparado, `env CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` não produz chamada reconhecida pelo parser. Também escapam ambiente previamente exportado, valores obtidos por expansão de variáveis e execução dentro de `bash -c` ou outro script. O wrapper lê o ambiente efetivo e executa o Codex sem consultar B3. Parte desses limites já está declarada no código, mas continua incompatível com a garantia absoluta do contrato. (`bin/disjuntor.py:1117`, `bin/disjuntor.py:1120`, `bin/disjuntor.py:1196`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:157`)

3. **O parser pode negar outro papel ou consultar a obra errada.** `CODEX_PAPEL=verificador CODEX_PAPEL=leitura ...` é reconhecido como verificador, embora o wrapper receba leitura; a ordem inversa libera o verificador. O mesmo problema afeta atribuições repetidas de obra. O regex também encontra `CODEX_PAPEL=verificador` dentro do valor de outra variável. Além disso, `CODEX_OBRA="O9 (C)"` vira `O9`: pode liberar a obra parada `O9 (C)` ou negar essa chamada por um B3 de `O9`. A identidade com cauda é suportada pelo reconhecedor do opus. (`bin/disjuntor.py:525`, `bin/disjuntor.py:1196`, `bin/disjuntor.py:1198`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`)

4. **Um sidecar basta, mesmo sem resposta; a origem não é comprovada.** Se existir apenas `R.id` com os cinco campos preenchidos e papel verificador, `--triagem R procedente` aceita: não abre R. Um sidecar fabricado ou alterado também passa. Alterar a identidade entre duas triagens permite representar uma única chamada como duas voltas. O leitor do log, por sua vez, aceita qualquer linha com quatro campos e veredito permitido, sem conferir seu lastro. Portanto existe a porta escritora pretendida, mas a exclusividade e a procedência não são garantidas por essas validações. (`bin/disjuntor.py:755`, `bin/disjuntor.py:759`, `bin/disjuntor.py:768`, `bin/disjuntor.py:773`, `bin/disjuntor.py:794`)

5. **Reutilizar o arquivo de resposta perde ou troca o lastro da chamada.** Duas chamadas bem-sucedidas para R deixam somente o último `R.id`; uma triagem atrasada da primeira passa a usar a identidade da segunda. Outro cenário: R tinha sidecar de verificador; uma chamada de papel leitura sobrescreve R e termina com erro; o sidecar anterior permanece, permitindo triar a resposta nova como verificador. Se RC=0 ocorrer sem resposta nova, mas R antigo existir, o wrapper grava identidade nova para o arquivo antigo. Falhar a escrita do sidecar também não invalida previamente a identidade velha. (`bin/codex_pergunta.sh:40`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:178`, `bin/codex_pergunta.sh:180`, `bin/disjuntor.py:768`, `bin/disjuntor.py:771`)

6. **A identidade pode fundir chamadas distintas.** Ela usa hash truncado em 12 caracteres, PID e término com precisão de segundos. Se duas chamadas coincidirem nesses campos — por exemplo, PID reciclado com relógio recuado — serão uma única volta; se a chave coincidir com uma liberada/rearmada, a chamada nova será ignorada. Ocorrência dessa colisão em produção: **[nao verificado]**. (`bin/codex_pergunta.sh:179`, `bin/disjuntor.py:773`, `bin/disjuntor.py:795`, `bin/disjuntor.py:842`)

7. **Triagem gravada fora da trava e decisão com fonte obsoleta.** O teste de idempotência e o append não compartilham trava: dois processos podem verificar ausência e anexar duas linhas iguais. **Isso, isoladamente, não conta duas voltas**, pois o leitor reduz por chave e o acumulador verifica pertença. Contudo, `decide_b3` lê as triagens antes de adquirir a trava do JSON: uma reclassificação ou terceira triagem concluída nesse intervalo fica fora da decisão; uma liberação também pode deixar de consumir voltas já PROCEDENTES quando efetivamente grava. (`bin/disjuntor.py:774`, `bin/disjuntor.py:778`, `bin/disjuntor.py:795`, `bin/disjuntor.py:821`, `bin/disjuntor.py:844`, `bin/disjuntor.py:897`)

8. **Rearme bem-sucedido pode redisparar imediatamente com triagens anteriores.** A já está no JSON; B/C/D já estão PROCEDENTES no log, mas ainda não foram varridas. O rearme arquiva somente A e retorna sucesso. Na decisão seguinte B/C/D entram e disparam, sem nenhuma volta nova após o rearme. A função não lê a triagem antes de zerar. (`bin/disjuntor.py:908`, `bin/disjuntor.py:912`, `bin/disjuntor.py:913`, `bin/disjuntor.py:925`, `bin/disjuntor.py:839`)

9. **Erro de leitura pode abrir ambas as portas, inclusive com disparo persistido.** `triagem.log` ilegível ou transformado em diretório levanta antes de consultar o estado; somente `FileNotFoundError` é tratado pelo leitor. JSON inválido, falha ao abrir a trava ou trava ocupada também levantam. O PreToolUse captura a exceção e libera a ferramenta; o Stop captura e omite o recado. Já log ausente retorna vazio, e linhas malformadas são ignoradas: se o JSON ainda não acumulou as voltas, a falta de fonte vira ausência de contagem. (`bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:794`, `bin/disjuntor.py:821`, `bin/disjuntor.py:248`, `bin/disjuntor.py:257`, `bin/disjuntor.py:263`, `bin/disjuntor.py:1329`, `bin/hook_stop_fila1.py:1056`)

10. **A leitura final do opus também pode escapar pelo texto do prompt.** `LEITURA FINAL REPROVA-E-DESENHO-CODEX` reconhece “final”, mas não a obra, pois falta uma das âncoras esperadas. `Parecer final da obra REPROVA-E-DESENHO-CODEX` não reconhece “final” pelo vocabulário fechado. Incluir no histórico do prompt a frase `nao e a leitura final` desativa o reconhecimento de todo o prompt. Nessas formas a chamada não consulta B3. São limites preexistentes, mas afetam o item 3 pedido. (`bin/disjuntor.py:103`, `bin/disjuntor.py:111`, `bin/disjuntor.py:113`, `bin/disjuntor.py:500`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1290`)

11. **O ESTADO pode mostrar zeros/“nenhuma” sem ter conseguido ler.** `ler` começa com contadores vazios e ignora `OSError` das trilhas; `linha` imprime B3=0, negativas=0 e liberações=0. Assim, trilha ausente/ilegível pode produzir aparência de normalidade mesmo com obra disparada no JSON; esse painel não lista os B3 ativos. Falha de leitura das chamadas Codex vira `None`, depois `{}`, depois “nenhuma”. O campo `b3_sem_obra`, especificamente, preserva “nao sei” quando o estado é ilegível. (`bin/disjuntor.py:1373`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:1425`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1439`, `bin/disjuntor.py:1476`, `bin/disjuntor.py:1479`, `bin/disjuntor.py:1493`)

Há ainda uma diferença temporal relevante: a terceira triagem apenas anexa ao log; o disparo acontece na próxima decisão. Um DESENHO escrito entre esses dois momentos entra na fotografia do disparo e não libera. Isso não atende à leitura literal de “dispara quando houver três”; segue, porém, o protocolo existente de exigir linha posterior ao disparo observado. O contrato precisa escolher esse instante para eliminar a ambiguidade. (`bin/disjuntor.py:778`, `bin/disjuntor.py:825`, `bin/disjuntor.py:850`, `bin/disjuntor.py:859`, `bin/disjuntor.py:870`, `bin/disjuntor.py:883`)

**Não há fundamento para acusar escrita do JSON B3 fora da trava:** a gravação e os anúncios acontecem dentro de `_com_estado`, após `flock`. O estado da triagem e sua leitura antecipada são os pontos fora dessa proteção. A liberação/rearme das identidades já observadas também limpa as listas e impede sua reincorporação. (`bin/disjuntor.py:252`, `bin/disjuntor.py:273`, `bin/disjuntor.py:284`, `bin/disjuntor.py:287`, `bin/disjuntor.py:778`, `bin/disjuntor.py:821`, `bin/disjuntor.py:842`, `bin/disjuntor.py:876`, `bin/disjuntor.py:912`)

O selo e seus mutantes têm esta cobertura:

| Item | O que exercitam | O que fica sem caso que morde |
|---|---|---|
| **1** | Obras separadas; duas passam/três param; repetir a mesma triagem grava uma linha. Mutantes `sem-codex`, `limiar-2`, `por-achado`. (`bin/tests/test_disjuntor.sh:1147`, `bin/tests/test_disjuntor.sh:1150`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1251`, `bin/tests/test_disjuntor.sh:1259`) | Wrapper real, conservação/colisão da identidade e reutilização da resposta. A fixture escreve somente `achado`; `por-achado` muda a identidade a cada **triagem**, não testa resposta com vários achados. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1259`) |
| **2** | Descartadas desde o início e não triadas; mutante que inclui descartadas. (`bin/tests/test_disjuntor.sh:1140`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1250`) | **PROCEDENTE → decisão → DESCARTADO antes do disparo.** A reclassificação testada ocorre somente depois da liberação. (`bin/tests/test_disjuntor.sh:1169`) |
| **3** | Negativa de opus final e Codex, permissão de leitura, recado Stop e liberação DESENHO; mutante remove a negativa Codex. (`bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1158`, `bin/tests/test_disjuntor.sh:1160`, `bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1252`) | Ambiente real, sintaxes alternativas, falsas negativas/positivas do parser e falhas de leitura. Codex tampouco é a primeira borda após a terceira triagem: `_parada` já dispara antes. (`bin/tests/test_disjuntor.sh:1133`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`) |
| **4** | Recusa sidecar ausente, papel leitura e veredito inválido; mutante `sem-sidecar`. (`bin/tests/test_disjuntor.sh:1136`, `bin/tests/test_disjuntor.sh:1137`, `bin/tests/test_disjuntor.sh:1138`, `bin/tests/test_disjuntor.sh:1253`) | Existência/vínculo da resposta, procedência do sidecar, escritor exclusivo e concorrência. O próprio teste fabrica `.id`; a cópia dos mutantes inclui apenas `bin/*.py`. (`bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1184`) |
| **5** | Liberação DESENHO, quarta volta isolada e reclassificação de volta já liberada; mutante deixa `codex` sem limpar na liberação. (`bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1166`, `bin/tests/test_disjuntor.sh:1169`, `bin/tests/test_disjuntor.sh:1258`) | **Rearme Codex.** Os casos antigos rearmam reprovações opus. Retirar Codex da união de `rearmado`, mantendo a limpeza imediata, não é exercitado pelo caso novo. Também falta rearme com triagens anteriores ainda não varridas. (`bin/tests/test_disjuntor.sh:398`, `bin/tests/test_disjuntor.sh:1081`, `bin/tests/test_disjuntor.sh:1124`, `bin/tests/test_disjuntor.sh:1172`, `bin/disjuntor.py:912`) |

Os testes antigos acrescentam cobertura compartilhada de ERRO-BOBO e de anúncio sob a trava; não substituem os casos ausentes de triagem e rearme Codex. (`bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:1063`, `bin/tests/test_disjuntor.sh:1081`)

O próprio selo ainda aceita ausência de sinal como sucesso em vários negativos: `_parada` procura apenas texto B3, sem verificar nova FALHA. Uma exceção que silencie a decisão pode passar nesses pontos, embora `_passa` tenha uma checagem mais forte. Portanto, **o item 5, no ramo de rearme Codex, é o que fica inteiramente sem caso específico que morde; os demais têm cobertura parcial e as lacunas acima.** (`bin/tests/test_disjuntor.sh:98`, `bin/tests/test_disjuntor.sh:1134`, `bin/tests/test_disjuntor.sh:1145`, `bin/tests/test_disjuntor.sh:1148`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1170`)

## 31/10 14:29 · sessao b13e3e31e25d · - · - · origem=Ronald (tmux)

Auditoria somente leitura dos itens 1, 2, 4 e 5:

- **Contagem nominal correta:** há limiar 3, isolamento por obra, deduplicação por identidade e arquivo com N achados conta uma identidade; achados da resposta nem são percorridos. A fonte considera apenas o último veredito PROCEDENTE. `bin/disjuntor.py:76`, `bin/disjuntor.py:773`, `bin/disjuntor.py:795`, `bin/disjuntor.py:801`, `bin/disjuntor.py:839`, `bin/disjuntor.py:844`, `bin/disjuntor.py:850`.

- **Defeito certo: DESCARTADO continua contando depois de observado PROCEDENTE.** A fonte é recalculada, mas `r['codex']` recebe apenas união; nenhuma identidade deixa a lista por reclassificação. Cenário: A PROCEDENTE → decisão guarda A; A vira DESCARTADO; B e C PROCEDENTE → decisão dispara com `[A,B,C]`, embora só B e C sejam procedentes. Reprodução feita somente em memória com mocks; o resultado decorre de `bin/disjuntor.py:801`, `bin/disjuntor.py:839`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`.

- **Defeito certo: rearme não zera as triagens já existentes ainda não observadas.** `rearma_b3` arquiva apenas as listas existentes no JSON e não consulta a triagem. Cenário: obra já existe com lista vazia; D/E/F estão PROCEDENTE no log; rearme retorna sucesso; próxima decisão incorpora D/E/F e dispara imediatamente. Reprodução em memória confirmou. `bin/disjuntor.py:908`, `bin/disjuntor.py:912`, `bin/disjuntor.py:913`, `bin/disjuntor.py:925`, `bin/disjuntor.py:839`.

- **Liberação/rearme dos itens efetivamente observados estão corretos:** movem as identidades para conjuntos de exclusão e limpam ambas as listas. Reclassificar uma identidade já liberada não a ressuscita. `bin/disjuntor.py:842`, `bin/disjuntor.py:874`, `bin/disjuntor.py:876`, `bin/disjuntor.py:912`.

- **Estado da triagem fora da trava:** o check de idempotência e o append não usam flock; a leitura para decidir ocorre antes da trava do JSON. Dois processos podem anexar a mesma triagem duas vezes, embora isso **não** conte duas voltas porque o leitor reduz por identidade. Uma decisão pode usar veredito obsoleto, ou uma liberação deixar de consumir voltas procedentes anexadas entre a leitura e a transição. `bin/disjuntor.py:774`, `bin/disjuntor.py:778`, `bin/disjuntor.py:795`, `bin/disjuntor.py:821`, `bin/disjuntor.py:897`. O JSON B3 é efetivamente escrito sob flock; não encontrei outra gravação desse JSON fora da trava nos arquivos examinados. `bin/disjuntor.py:248`, `bin/disjuntor.py:252`, `bin/disjuntor.py:267`, `bin/disjuntor.py:273`.

- **Identidade insuficientemente vinculada à resposta:** `_le_id` verifica somente cinco campos não vazios; não verifica existência da resposta, conteúdo, hash, mtime, proveniência do wrapper ou correspondência com a chamada. Só o sidecar basta para triagem. `bin/disjuntor.py:752`, `bin/disjuntor.py:755`, `bin/disjuntor.py:759`, `bin/disjuntor.py:768`.

- **Sidecar obsoleto permite triar resposta de outro papel:** o wrapper aceita reutilizar RESPOSTA e só substitui `.id` no sucesso; não invalida identidade anterior no começo nem no erro. Cenário: R.id antigo de verificador; nova chamada `leitura` escreve R e termina com erro; a triagem aceita a resposta nova como verificador antigo. `bin/codex_pergunta.sh:40`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:178`, `bin/disjuntor.py:771`.

- **Reutilização/sobrescrita perde ou atribui indevidamente identidade:** duas chamadas bem-sucedidas para o mesmo caminho deixam só o último `.id`, impedindo triagem posterior da anterior; RC0 sem resposta nova, mas com R antigo existente, gera nova identidade para conteúdo antigo; falha ao abrir `.id` para gravar pode preservar identidade anterior e deduplicar a nova chamada como antiga. `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:178`, `bin/codex_pergunta.sh:180`, `bin/disjuntor.py:773`, `bin/disjuntor.py:774`.

- **Colisão estrutural de identidade subconta chamadas:** usa somente hash12 + PID + término com precisão de segundos. Com PID reciclado e mesmo segundo — ou relógio recuado — duas chamadas distintas podem fundir-se; também podem coincidir com identidade já liberada/rearmada e nunca contar. Ocorrência real: [nao verificado]. `bin/codex_pergunta.sh:179`, `bin/disjuntor.py:773`, `bin/disjuntor.py:842`.

- **Fonte ausente ou linhas corrompidas viram ausência de voltas:** log inexistente retorna `{}`; linhas fora do formato são ignoradas, sem representar “não sei”. Se não houver identidades previamente acumuladas, uma decisão permite prosseguir mesmo sem fonte suficiente para concluir zero. `bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:794`, `bin/disjuntor.py:801`.

Sobre descartadas anteriores à liberação: elas nunca integraram a contagem e não entram em `liberado`; reclassificá-las depois pode fazê-las contar. Isso é demonstrável no código, mas considerar toda volta anterior automaticamente consumida exige interpretação além de “voltas já liberadas não voltam a contar”. `bin/disjuntor.py:801`, `bin/disjuntor.py:874`, `bin/disjuntor.py:842`.

## 31/10 14:29 · sessao f6faf85cf3dc · - · - · origem=Ronald (tmux)

Achados de bloqueio e leitura, por inspeção da cópia; ocorrência em produção [nao verificado].

1. **Codex pode passar com B3 disparado.** O bloqueio depende exclusivamente da obra e papel extraídos de atribuições literais anteriores ao comando; o wrapper lê o ambiente efetivo e executa sem consultar B3. Cenários que o parser confirmou em memória:
   - `env CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador bin/codex_pergunta.sh h q r` → nenhuma chamada reconhecida.
   - `export CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador; bin/codex_pergunta.sh h q r` → papel `?`, obra vazia.
   - `CODEX_OBRA="$OBRA" CODEX_PAPEL="$PAPEL" ...` → expansão não resolvida.
   - `bash -c "CODEX_OBRA=OBRA-C CODEX_PAPEL=verificador ..."` → nenhuma chamada reconhecida.
   - Execução por outro script ou fora do PreToolUse também não encontra guarda dentro do wrapper. Fontes: `bin/disjuntor.py:1120`, `bin/disjuntor.py:1186`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:157`. Alguns limites já são expressamente declarados como resíduos em `bin/disjuntor.py:1117`.

2. **Negação indevida ou ausência de negação por primeira atribuição.** `CODEX_PAPEL=verificador CODEX_PAPEL=leitura` é lido como verificador e negado, embora o wrapper receba leitura; ordem inversa libera verificador. O mesmo ocorre com `CODEX_OBRA=OBRA-C CODEX_OBRA=OBRA-LIVRE` e ordem inversa. Além disso, `OUTRA="CODEX_PAPEL=verificador" CODEX_PAPEL=leitura` é lido como verificador. Fontes: `bin/disjuntor.py:1195`, `bin/disjuntor.py:1196`, `bin/disjuntor.py:1198`, `bin/disjuntor.py:1280`, `bin/codex_pergunta.sh:44`.

3. **Obra com espaços é truncada.** `CODEX_OBRA="O9 (C)" CODEX_PAPEL=verificador ...` vira obra `O9`, embora wrapper preserve `O9 (C)`; pode liberar obra parada `O9 (C)` ou negar indevidamente devido a B3 de `O9`. Essa identidade com cauda é explicitamente suportada pelo leitor de obras do opus. Fontes: `bin/disjuntor.py:1198`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:525`, `bin/codex_pergunta.sh:44`.

4. **Falha de leitura abre ambas as portas.** `decide_b3` lê a triagem antes do estado; `triagem.log` como diretório ou sem permissão levanta, pois só ausência é capturada. Isso libera Codex e opus mesmo se JSON já disser `disparado=true`. Estado JSON inválido, trava ocupada ou falha de abrir a trava também levantam e encontram o mesmo caminho que libera a ferramenta. Fontes: `bin/disjuntor.py:821`, `bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:248`, `bin/disjuntor.py:257`, `bin/disjuntor.py:263`, `bin/disjuntor.py:1329`. No Stop, a falha apenas omite o recado B3: `bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:1056`.

5. **Opus: reconhecimento textual preexistente deixa passar leitura final.** `LEITURA FINAL REPROVA-E-DESENHO-CODEX` é final mas não identifica obra; `Parecer final da obra REPROVA-E-DESENHO-CODEX` não é reconhecido como final; adicionar ao histórico `nao e a leitura final` invalida o reconhecimento de todo o prompt. Portanto B3 não é consultado nesses casos. Fontes: `bin/disjuntor.py:103`, `bin/disjuntor.py:111`, `bin/disjuntor.py:113`, `bin/disjuntor.py:498`, `bin/disjuntor.py:503`, `bin/disjuntor.py:1290`. Outro `subagent_type` também passa pelo B3, mas isso coincide com a exclusão contratual de outros papéis; classificar uma leitura como verificador apesar de outro tipo seria interpretação [nao verificado]. Fonte: `bin/disjuntor.py:1290`.

6. **Leitura produz zeros/“nenhuma” sem dado.** `ler` inicializa disparos, negativas e liberações em zero, ignora `OSError` das trilhas e `linha` apresenta esses zeros; não consulta obras B3 paradas, apenas `b3_sem_obra`. Se a trilha está ausente/ilegível, pode exibir B3=0 mesmo com estado disparado. Falha de `ler_chamadas` produz `None`, convertido para `{}` e depois “nenhuma”. Fontes: `bin/disjuntor.py:1373`, `bin/disjuntor.py:1405`, `bin/disjuntor.py:1425`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1476`, `bin/disjuntor.py:1479`, `bin/disjuntor.py:1493`, `bin/disjuntor.py:1501`. Não confundir com estado ilegível para `b3_sem_obra`: esse campo preserva “nao sei”. Fonte: `bin/disjuntor.py:1439`.

7. **Não encontrei escrita do JSON B3 fora da trava.** A única escrita desse JSON está em `_com_estado`, depois do `flock` e antes do fechamento do descritor. A triagem é apendada fora dessa trava e sua fotografia é lida antes de adquiri-la; são problemas distintos. Fontes: `bin/disjuntor.py:252`, `bin/disjuntor.py:270`, `bin/disjuntor.py:287`, `bin/disjuntor.py:778`, `bin/disjuntor.py:821`.

8. **Ressalva explícita de operação:** a chave `DISJUNTOR_DESLIGADO` suspende o PreToolUse e o recado Stop. É uma exceção preexistente declarada, não uma regressão escondida do diff. Fontes: `bin/disjuntor.py:34`, `bin/disjuntor.py:1320`, `bin/hook_stop_fila1.py:1039`.

9. **Janela entre terceira triagem e disparo persistido:** `triagem` só apenda; se DESENHO válido for escrito após a terceira triagem e antes da primeira decisão B3, a decisão dispara, fotografa essa linha como anterior e nega. Pelo contrato literal de disparar ao alcançar três, é atraso de disparo com negação adicional; pelo protocolo preexistente que exige linha posterior ao disparo observado, é comportamento esperado. Fontes: `bin/disjuntor.py:778`, `bin/disjuntor.py:825`, `bin/disjuntor.py:850`, `bin/disjuntor.py:859`, `bin/disjuntor.py:870`, `bin/disjuntor.py:883`.

O caso `b3_codex` usa apenas atribuições literais, obras sem espaços e sidecars fabricados; seus mutantes não exercitam wrapper real, variantes de ambiente, atribuições repetidas, falhas de leitura nem saída de estado sem dados. Fontes: `bin/tests/test_disjuntor.sh:1128`, `bin/tests/test_disjuntor.sh:1133`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1249`.

## 31/10 14:28 · sessao de83cf872bd2 · - · - · origem=Ronald (tmux)

O selo cobre percursos básicos, mas não fecha os cinco itens; os sete mutantes Codex alteram apenas `disjuntor.py`, e o teste fabrica resposta e identidade sem executar o wrapper. (`bin/tests/test_disjuntor.sh:1128`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1260`)

| Item | Cobertura efetiva | Lacuna |
|---|---|---|
| 1 — três chamadas distintas da mesma obra | Isolamento entre obras, duas passam/três bloqueiam, repetição da mesma triagem produz uma linha. Mutantes `sem-codex`, `limiar-2` e `por-achado`. (`bin/tests/test_disjuntor.sh:1147`, `bin/tests/test_disjuntor.sh:1150`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1251`, `bin/tests/test_disjuntor.sh:1259`) | Não testa identidade produzida pelo wrapper, reutilização do caminho da resposta, colisões ou chamadas concorrentes; `_rcx` sempre fabrica identidades com pids diferentes e resposta contendo apenas `achado`. (`bin/tests/test_disjuntor.sh:1128`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1150`, `bin/tests/test_disjuntor.sh:1155`) |
| 2 — não triada/descartada não conta | Três descartadas iniciais, três não triadas; mutante que conta descartadas. (`bin/tests/test_disjuntor.sh:1140`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1250`) | Falta `PROCEDENTE → DESCARTADO` antes do disparo: o acumulador apenas acrescenta e não reconcilia a retirada, permitindo disparo com duas procedentes atuais. O único caso de reclassificação ocorre depois da liberação. (`bin/disjuntor.py:839`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`, `bin/tests/test_disjuntor.sh:1169`) |
| 3 — bloqueios e liberação | Bloqueia Agent final, Codex verificador e Stop; permite papel leitura; libera com DESENHO. Mutante remove negativa Codex. (`bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1158`, `bin/tests/test_disjuntor.sh:1160`, `bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1252`) | B3 Codex usa apenas comando canônico com variáveis literais; testes de outras sintaxes pertencem ao caso de orçamento `codex`, sem obra parada por B3. Não testa Codex como primeira borda após a terceira triagem: `_parada` já provoca o disparo antes. (`bin/tests/test_disjuntor.sh:1133`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:562`, `bin/tests/test_disjuntor.sh:569`) |
| 4 — escritor e identidade | Recusa sidecar ausente, papel leitura e veredito inválido; mutante `sem-sidecar`. (`bin/tests/test_disjuntor.sh:1136`, `bin/tests/test_disjuntor.sh:1137`, `bin/tests/test_disjuntor.sh:1138`, `bin/tests/test_disjuntor.sh:1253`) | Não verifica origem real do sidecar, existência/conteúdo da resposta, escrita exclusiva nem concorrência; a própria fixture fabrica o lastro e `_mutante` copia só arquivos Python. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1130`, `bin/tests/test_disjuntor.sh:1184`) |
| 5 — zerar e não ressuscitar | Liberação DESENHO, quarta volta isolada e reclassificação de volta liberada; mutante que deixa toda a lista Codex na liberação. (`bin/tests/test_disjuntor.sh:1163`, `bin/tests/test_disjuntor.sh:1166`, `bin/tests/test_disjuntor.sh:1169`, `bin/tests/test_disjuntor.sh:1258`) | **Rearme de Codex não tem caso que morde.** O rearme antigo usa reprovações Opus; o caso de atomicidade também usa `reprova`. Retirar Codex da união de `rearmado`, mantendo a limpeza imediata, deixaria as voltas voltarem na decisão seguinte sem ser exercitado pelo selo Codex. (`bin/tests/test_disjuntor.sh:398`, `bin/tests/test_disjuntor.sh:1081`, `bin/tests/test_disjuntor.sh:1124`, `bin/tests/test_disjuntor.sh:1172`, `bin/disjuntor.py:912`, `bin/disjuntor.py:913`, `bin/disjuntor.py:842`) |

Os testes antigos acrescentam cobertura compartilhada de ERRO-BOBO, validade do topo, gravação e trava; seus estados são alimentados por reprovações Opus, portanto não substituem casos sobre a triagem Codex concorrente ou seu rearme. (`bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:398`, `bin/tests/test_disjuntor.sh:779`, `bin/tests/test_disjuntor.sh:834`, `bin/tests/test_disjuntor.sh:1063`)

Há também falso positivo no próprio selo: `_parada` só procura texto B3, e vários negativos aceitam qualquer saída sem esse texto; uma falha que silencie o bloqueio pode passar nesses pontos. `_passa` verifica ausência de nova FALHA, mas não envolve esses negativos. (`bin/tests/test_disjuntor.sh:98`, `bin/tests/test_disjuntor.sh:100`, `bin/tests/test_disjuntor.sh:1134`, `bin/tests/test_disjuntor.sh:1145`, `bin/tests/test_disjuntor.sh:1148`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1170`)

Resultado da execução da suíte e mortes efetivamente observadas dos mutantes: [nao verificado]. [nao verificado]

## 31/10 14:05 · sessao 9f9ab0ecebe5 · LEI-AKITA-UNIVERSO-DO-PUSH · verificador · origem=Code

**Aval: VERMELHO.** A volta corrige a travessia das raias e a comparação dos merges, mas ainda permite omitir o julgamento de curas e pode julgar um HEAD diferente do commit enviado. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:144`; `bin/pre-push.sh:22`, `bin/pre-push.sh:36`)

Comparei integralmente os dois arquivos com o snapshot do pai disponível. Autenticidade dos SHAs pelo Git: **[nao verificado]**, pois esta cópia não contém `.git`. O MORDE integral não foi executado: ele cria arquivos temporários; executei somente seus 13 casos unitários e contraprovas em memória. (`bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:180`, `bin/tests/test_lei_akita.sh:184`)

1. **Universo efetivamente julgado**

   A enumeração corresponde a:

   `ancestrais(HEAD) − ancestrais(origin/main) − ancestrais(primeiro pai do corte)`

   O último desconto só existe quando o corte tem pai. Depois dessa enumeração, somente commits com linha iniciada por `LEI-AKITA:` chegam ao juiz. (`bin/tests/test_lei_akita.sh:127`, `bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:139`)

   | Situação | Comportamento |
   |---|---|
   | HEAD igual a origin/main | Enumera zero e julga zero. Pode ficar VERDE se as demais verificações passarem; esse vazio é correto para o contrato. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:136`, `bin/tests/test_lei_akita.sh:243`) |
   | Commits locais à frente | Enumera todos os alcançáveis exclusivamente por HEAD, respeitando o piso; pula os sem linha LEI-AKITA. (`bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:139`) |
   | Merge de raia, dois pais | Inclui o merge e os commits exclusivos da raia: saiu `--first-parent`. Cada um passa pelo mesmo filtro de mensagem. (`bin/tests/test_lei_akita.sh:130`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:137`) |
   | origin/main ausente | `rev-list` falha; vira `PADRAO_ERRO` e falta `PADRAO_FIM`, portanto VERMELHO. (`bin/tests/test_lei_akita.sh:121`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:237`) |
   | Repositório raso | Rejeita antes de procurar o corte ou enumerar commits. (`bin/tests/test_lei_akita.sh:125`, `bin/tests/test_lei_akita.sh:126`) |
   | Corte ausente | Rejeita explicitamente; deixou de interpretar ausência como universo vazio. (`bin/tests/test_lei_akita.sh:127`, `bin/tests/test_lei_akita.sh:128`, `bin/tests/test_lei_akita.sh:129`) |

   “Publicado” significa aqui **alcançável pela referência local `origin/main`**. Não há consulta ao servidor nessa função; referência desatualizada pode fazer julgar história já publicada. Estado real dessa referência: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:118`, `bin/tests/test_lei_akita.sh:135`)

2. **Lista de arquivos**

   | Commit | Lista usada | Aval | [nao verificado]
   |---|---|---|
   | Comum | `git diff --name-only h^1 h` | Representa a alteração contra seu pai. (`bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:144`) |
   | Merge | O mesmo diff, contra o primeiro pai | Representa o que o pouso acrescenta à linha receptora, incluindo resoluções; corrige o vazio que `show` podia devolver. (`bin/tests/test_lei_akita.sh:142`, `bin/tests/test_lei_akita.sh:144`) |
   | Raiz | `git show --name-only --format= h` | Busca os arquivos introduzidos pela raiz. (`bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:144`) |

   Para caminhos comuns, a escolha corresponde ao que cada commit traz. Entretanto, a saída é textual, sem `-z`, e é passada por `splitlines()`; negócio depende de o texto começar literalmente com `app/`. Um caminho recebido entre aspas, como `"app/ponto/a\303\247ao.py"`, passa como não negócio e libera `n/a`. A falha do classificador foi reproduzida em memória; reprodução da emissão desse caminho pelo Git real: **[nao verificado]**. (`bin/tests/test_lei_akita.sh:92`, `bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:145`)

3. **Vocabulário aceito**

   São **55 termos normalizados**. O selo ignora caixa, normaliza espaços e remove crases, asteriscos e conteúdo entre parênteses. Além deles, aceita exatamente `n/a (medida)` e `n/a (instrumento)` quando não identifica arquivo de negócio. (`bin/tests/test_lei_akita.sh:61`, `bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:90`, `bin/tests/test_lei_akita.sh:99`)

   | Fonte | Termos aceitos |
   |---|---|
   | `app/docs/GLOSSARIO.md:11` | single source of truth; aggregate root; celula soberana; fonte unica |
   | `app/docs/GLOSSARIO.md:12` | single writer principle; juiz unico; escritor unico |
   | `app/docs/GLOSSARIO.md:13` | sinal derivado do estado; reconciliacao level-triggered; lampada |
   | `app/docs/GLOSSARIO.md:14` | architectural invariants + fitness functions; contratos estruturais |
   | `app/docs/GLOSSARIO.md:15` | architecture conformance test; selo |
   | `app/docs/GLOSSARIO.md:16` | event-driven architecture; crons virando evento |
   | `app/docs/GLOSSARIO.md:17` | transactional outbox; o-outbox |
   | `app/docs/GLOSSARIO.md:18` | strangler fig pattern; construir ao lado e trocar inteira |
   | `app/docs/GLOSSARIO.md:19` | shadow mode; parallel run; sombra |
   | `app/docs/GLOSSARIO.md:20` | merge queue; comboio; pouso |
   | `app/docs/GLOSSARIO.md:21` | backfill; reprocessing; rejulgar o passivo |
   | `app/docs/GLOSSARIO.md:22` | cqrs com projecoes reconstruiveis; espelho; diapago; fechamento; retrato e pilulas |
   | `app/docs/GLOSSARIO.md:23` | duplicated business logic; logic drift; derivacao paralela |
   | `app/docs/GLOSSARIO.md:24` | bitemporal data; vinculo do dia x vinculo de hoje |
   | `app/docs/GLOSSARIO.md:25` | immutable event log + compensating event; batida so retratada |
   | `app/docs/GLOSSARIO.md:26` | adr; leis; cortes |
   | `app/docs/GLOSSARIO.md:27` | idempotencia pela chave do fato; dedup do fato |
   | `app/docs/PLANO-PADROES.md:46`, `app/docs/PLANO-PADROES.md:47`, `app/docs/PLANO-PADROES.md:48` — adicionais | um juiz por pergunta; single writer; evento em vez de varredura; log imutavel; outbox; bitemporal; projecao reconstruivel; cqrs; fitness function |

   **Não coincide integralmente com a redação da lei.** A enumeração acompanha o NORTE e as duas primeiras colunas do glossário, mas a CLAUDE admite termo existente nos dois arquivos, e a própria L-123 repete essa redação ampla. (`app/docs/PLANO-PADROES.md:48`, `app/docs/PLANO-PADROES.md:49`, `app/docs/PLANO-PADROES.md:53`; `CLAUDE.md:68`)

   Recusas comprovadas na função real:

   - `Architecture Decision Record`: aparece explicitamente como padrão conhecido e também no glossário; é perdido pela remoção dos parênteses e pela restrição ao NORTE. (`app/docs/PLANO-PADROES.md:41`; `app/docs/GLOSSARIO.md:26`; `bin/tests/test_lei_akita.sh:78`, `bin/tests/test_lei_akita.sh:84`)
   - `projection rebuild` e `read model replay`: aparecem como padrões conhecidos no plano, mas ficam fora do trecho extraído. (`app/docs/PLANO-PADROES.md:33`; `bin/tests/test_lei_akita.sh:78`, `bin/tests/test_lei_akita.sh:104`)

   No sentido contrário, aceita `padrao=juiz unico (Outro Padrao Totalmente Novo)`: elimina o complemento antes da comparação. Esse valor integral não é um dos termos enumerados. (`bin/tests/test_lei_akita.sh:62`, `bin/tests/test_lei_akita.sh:104`)

4. **Caminhos de VERDE sem o julgamento devido** [nao verificado]

   - **Cura sem linha LEI-AKITA:** é pulada. Uma mensagem contendo somente `cura` e `padrao=Outro Padrao Totalmente Novo` não produz falha. As verificações anteriores cobram a menção no molde, não a presença em cada commit. Isso deixa descoberta a exigência de que nenhuma cura entre sem padrão. (`bin/tests/test_lei_akita.sh:45`, `bin/tests/test_lei_akita.sh:46`, `bin/tests/test_lei_akita.sh:139`; `CLAUDE.md:66`)
   - **Campo falso ou duplicado:** `xpadrao=juiz unico` é aceito; `padrao=Padrao Inventado Do Nada, padrao=juiz unico` também. A regex não exige fronteira de campo e só usa a última ocorrência. Ambas as contraprovas passaram na função real. (`bin/tests/test_lei_akita.sh:95`, `bin/tests/test_lei_akita.sh:98`)
   - **Ref enviada diferente de HEAD:** o selo pode julgar universo vazio de HEAD enquanto outra ref enviada contém commit ruim. A seleção do SHA enviado acontece depois do selo. (`bin/tests/test_lei_akita.sh:135`; `bin/pre-push.sh:22`, `bin/pre-push.sh:36`)
   - **Régua com carimbo:** retorna “JA VERDE” antes de executar a pasta de selos; sua condição compara arquivos Python/HTML, sem identidade das mensagens ou do universo Git. (`bin/regua.sh:33`, `bin/regua.sh:84`, `bin/regua.sh:93`, `bin/regua.sh:138`)

5. **Os MORDE mordem, mas deixam mutantes sobreviver**

   Os 13 casos unitários exercitam a função real e distinguem termo inventado, substring, ausência/vazio, `n/a` em negócio e duas linhas com uma inválida. O cenário Git procura quatro commits ruins, incluindo raia e merge, excluindo o publicado. (`bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:187`, `bin/tests/test_lei_akita.sh:200`)

   Mutantes concretos:

   | Mutante | Por que sobrevive |
   |---|---|
   | Trocar `_limpo(termo) not in VOCAB` por `_limpo(termo) in {'padrao inventado do nada', 'a', 'writer principle'}` | Passou nos 13 casos executados em memória e aceitou `Outro Padrao Totalmente Novo`. O cenário Git também usa somente aquele inventado literal; os demais controles permanecem iguais. (`bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:154`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:189`, `bin/tests/test_lei_akita.sh:195`) |
   | Na ausência do corte, usar `corte=['HEAD']` | O teste “sem corte” também não tem `origin/main`; falharia por essa segunda ausência e passaria, porque aceita qualquer `PADRAO_ERRO`. (`bin/tests/test_lei_akita.sh:128`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:191`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:223`) |
   | Remover `*pisos` do `rev-list` | O corte plantado é raiz, logo seu piso já é vazio. Não testa a não retroatividade com corte não raiz. (`bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:182`) |
   | Comparar merges com o segundo pai | Ambos os lados plantados alteram negócio; o merge com `n/a` continuaria vermelho contra qualquer pai. (`bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:188`, `bin/tests/test_lei_akita.sh:194`, `bin/tests/test_lei_akita.sh:197`) |
   | Rejeitar qualquer universo vazio | Não existe caso saudável `HEAD==origin/main`; o cenário principal exige quatro ruins. Esse mutante bloquearia o main sincronizado sem reprovar o MORDE. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:200`) |

   Sobrevivência integral desses mutantes com Git real: **[nao verificado]**; além do primeiro teste unitário executado, a conclusão acima é por inspeção dos caminhos. [nao verificado]

6. **Chamadores e bloqueio de push legítimo** [nao verificado]

   A régua executa todos os `bin/tests/test_*.sh` e aborta no primeiro vermelho. A chamada nova no pre-push ocorre antes do atalho do arsenal, fechando o escape “régua já verde” para o push comum de HEAD. `SKIP_TESTS=1` continua desligando tudo. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`; `bin/pre-push.sh:5`, `bin/pre-push.sh:22`, `bin/pre-push.sh:65`)

   **Sim, pode bloquear push legítimo:** exclusão de branch e envio de outra ref válida passam pelo julgamento de HEAD antes de o hook reconhecer o alvo. Um commit inválido apenas no HEAD basta para barrá-los. O seletor existente já distingue exclusão e SHA enviado, mas é chamado depois. (`bin/pre-push.sh:22`, `bin/pre-push.sh:36`, `bin/pre-push.sh:37`; `app/core/arvore_do_push.py:23`, `app/core/arvore_do_push.py:34`, `app/core/arvore_do_push.py:43`)

   Há também raiz fixa: o wrapper delega ao pre-push do repositório corrente, mas a chamada do selo aponta para `~/saas-hasner`, e o selo usa `/home/ronald/saas-hasner` por padrão. Em clone/worktree, pode avaliar outra árvore. Instalação real do hook: **[nao verificado]**. (`bin/hooks.sh:56`; `bin/pre-push.sh:22`; `bin/tests/test_lei_akita.sh:17`)

7. **Outros defeitos e limites**

   - O vocabulário e as verificações estruturais vêm dos arquivos no disco, enquanto mensagens e caminhos vêm dos commits. Uma alteração documental ainda não commitada pode mudar o vocabulário que aprova o push. (`bin/tests/test_lei_akita.sh:28`, `bin/tests/test_lei_akita.sh:70`, `bin/tests/test_lei_akita.sh:77`, `bin/tests/test_lei_akita.sh:138`)
   - O preparo dos repositórios MORDE descarta o código de retorno dos comandos Git. Erros de preparação podem ser confundidos com o erro que o caso queria provar; o teste sem corte exemplifica essa falta de isolamento. (`bin/tests/test_lei_akita.sh:176`, `bin/tests/test_lei_akita.sh:178`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:224`)
   - O selo confere nome e classificação de caminhos; não confere se o diff realiza o padrão declarado. Essa obrigação permanece explicitamente com o verificador humano, portanto um VERDE mecânico não a certifica. (`bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:145`; `CLAUDE.md:70`; `app/docs/PLANO-PADROES.md:58`)

## 31/10 14:04 · sessao d089eab7c55b · CLAUDE-MD-RACIONAL · verificador · origem=Code

**As três correções recuperam o alcance anterior; o PRONTO numérico confere.** A literalidade é do núcleo operativo, não uma reprodução integral dos parágrafos antigos. Fontes comparadas abaixo.

1. **As três cláusulas**

   **(4) Moratória L-117 — mesmo alcance.**

   Antes:

   > Ate `contratos_estruturais` = **20/20** (juiz `core/contratos_estruturais.py::total()`, hoje 15/20), o Code **nao numera nem propoe lei nova por iniciativa propria**: lei nova so nasce de CORTE LITERAL dele.

   `a7b66d14:CLAUDE.md:644–646`.

   Volta 8:

   > Ate 20/20 o Code nao numera nem propoe lei nova por iniciativa propria: lei nova so nasce de CORTE LITERAL dele (L-117).

   `2f5e38c6:CLAUDE.md:455`.

   Foram recuperados tanto **“lei nova por iniciativa propria”** quanto a exceção **“CORTE LITERAL dele”**. O encaminhamento da pergunta com números e continuidade da fila permanece no mesmo item; “Ate 20/20” preserva o término da moratória. O contador e seu juiz continuam identificados no arquivo. `a7b66d14:CLAUDE.md:646–647`; `2f5e38c6:CLAUDE.md:453–457`, `:259–263`.

   **(5) AUTONOMIA-DE-CODIGO, item 2 — mesmo alcance.**

   Antes:

   > **qual raia ou agente pega qual obra**, e quando uma obra registrada entra como COMPLEMENTO de outra;

   `a7b66d14:CLAUDE.md:481–482`.

   Volta 8:

   > qual raia ou agente pega qual obra e quando uma obra registrada entra como COMPLEMENTO de outra;

   `2f5e38c6:CLAUDE.md:461–462`.

   Recupera **obra registrada como complemento de outra**, mantendo decisão do Code, registro em uma linha e dispensa de aval. O destino de achados continua sendo cláusula separada. `a7b66d14:CLAUDE.md:479`, `:487`; `2f5e38c6:CLAUDE.md:460–462`.

   **(7a) CODEX-PRE-VERIFICADOR — mesmo alcance geral.**

   Antes:

   > antes de declarar pronto, Codex `CODEX_PAPEL=verificador` (diff + leis dos sitios + classes que ja reprovaram)

   `a7b66d14:CLAUDE.md:507`.

   Volta 8:

   > PRE-VERIFICADOR. (a) TODO construtor, ANTES de declarar pronto, `CODEX_PAPEL=verificador`

   `2f5e38c6:CLAUDE.md:543`.

   A redação anterior não limitava a obrigação a cura de negócio; o texto apontado por ela também dizia **“O construtor, ANTES de declarar pronto”**. A volta 8 elimina a restrição introduzida pelo pai, sem criar nova categoria de sujeitos. `a7b66d14:CLAUDE.md:507`; `a7b66d14:app/docs/PISTA.md:100–101`; `034b0e38:CLAUDE.md:543`.

2. **Os cortes dos cabeçalhos não retiram regra, condição ou ponteiro exclusivo.**

   - **PLACAR:** saiu somente “resposta a uma pergunta de LEI que eu levantei”. Essa informação histórica não se repete, mas não condicionava a regra. Permanecem autor/data, exercício, dois verdes, L-095 e `lap-18`. `034b0e38:CLAUDE.md:566`; `2f5e38c6:CLAUDE.md:566–570`.
   - **“historia em LAPIDES”:** saíram apenas essas palavras dos cabeçalhos de PARADA, RAIAS e DINHEIRO. Os ponteiros específicos permanecem respectivamente em `:443`, `:482` e `:574`. `034b0e38:CLAUDE.md:440`, `:479`, `:573`; `2f5e38c6:CLAUDE.md:443`, `:482`, `:574`.

3. **PRONTO conferido em bytes, sem normalizar o texto.**

   | Medida | Resultado |
   |---|---:|
   | 7b, do início do título até antes de `## 8.` | **20.955 B ≤ 21.000 B** |
   | Arquivo inteiro | **57.756 B ≤ 62.000 B** |

   Fontes medidas: `2f5e38c6:CLAUDE.md:436–627` e `2f5e38c6:CLAUDE.md:1–631`.

   Os ponteiros `LAPIDES.md#lap-N` preservam **69 ocorrências e 68 destinos distintos**, com mesmas multiplicidades nas três versões. Do pai à volta 8, também preservam a sequência; frente a `a7b66d14`, houve reordenação. A duplicata é `lap-63`. `a7b66d14:CLAUDE.md:1–672`; `034b0e38:CLAUDE.md:1–631`; `2f5e38c6:CLAUDE.md:1–631`, especialmente `:231`, `:235`.

4. **Outros defeitos**

   Não identifiquei outro defeito material no delta: ele se limita às três correções e aos quatro cortes de cabeçalho examinados, preservando o restante do texto do pai. `034b0e38:CLAUDE.md:440`, `:455`, `:461–462`, `:479`, `:543`, `:566`, `:573`; mesmas posições em `2f5e38c6:CLAUDE.md`.

   A execução do selo completo e de seus quatro MORDE fica **[nao verificado]**; o script cria e apaga arquivos temporários, contrariando a restrição desta auditoria. `bin/tests/test_claude_md_regras_preservadas.sh:89–114`.

## 31/10 14:04 · sessao 21b77f1d4341 · - · - · origem=Ronald (tmux)

Achados por inspeção estática; MORDE integral não executado porque cria diretórios e arquivos (`bin/tests/test_lei_akita.sh:180`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:184`).

1. **O teste “sem corte” não isola a falta do corte.** `plantar(..., marcador=False)` não cria `origin/main`; essa referência só é criada no outro cenário. O teste aceita qualquer `SystemExit` começando por `PADRAO_ERRO`. Mutante sobrevivente: substituir a rejeição da ausência do corte por `if not corte: corte=['HEAD']`; o teste continuaria passando por erro de `origin/main` na enumeração. Em repositório com `origin/main`, permitiria escolher arbitrariamente `HEAD` como corte. (`bin/tests/test_lei_akita.sh:128`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:175`, `bin/tests/test_lei_akita.sh:191`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:223`.)

2. **O piso não é testado.** Mutante sobrevivente: remover `*pisos` da chamada de `rev-list`. O único cenário saudável cria o corte na raiz, portanto `pisos=[]`; além disso, a raiz já é ancestral do publicado. Não distingue preservar a não retroatividade de julgá-la incorretamente. (`bin/tests/test_lei_akita.sh:132`, `bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:179`, `bin/tests/test_lei_akita.sh:182`, `bin/tests/test_lei_akita.sh:191`.)

3. **O primeiro pai do merge não é comprovado pelo MORDE.** Mutante sobrevivente: usar o segundo pai somente nos merges. Ambos os lados plantados alteram arquivos de negócio; comparar com qualquer pai deixa o merge `n/a (instrumento)` vermelho. Para distinguir os pais, precisa de cenário assimétrico, por exemplo primeiro lado com negócio e raia apenas com documentação. (`bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:187`, `bin/tests/test_lei_akita.sh:194`, `bin/tests/test_lei_akita.sh:197`.)

4. **A lista do commit raiz não é testada.** Mutante sobrevivente: substituir o ramo `else git('show',...)` por `else ''`. Nenhuma raiz é julgada no cenário de quatro novos: o corte raiz está atrás de `origin/main`. Com raiz de negócio efetivamente no universo, esse mutante aceitaria `n/a`. (`bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:182`, `bin/tests/test_lei_akita.sh:191`, `bin/tests/test_lei_akita.sh:198`.)

5. **Não há cenário saudável de universo vazio.** Mutante sobrevivente: inserir, após a enumeração, `if not hs: raise SystemExit('PADRAO_ERRO vazio')`. Ele bloquearia `HEAD==origin/main`, mas o cenário positivo exige exatamente quatro novos ruins; os outros cenários esperam erros. (`bin/tests/test_lei_akita.sh:135`, `bin/tests/test_lei_akita.sh:198`, `bin/tests/test_lei_akita.sh:200`, `bin/tests/test_lei_akita.sh:205`, `bin/tests/test_lei_akita.sh:212`, `bin/tests/test_lei_akita.sh:220`.)

6. **Fragilidade adicional da lista de caminhos:** a enumeração usa texto sem `-z` e depois `splitlines()`, enquanto negócio depende de `startswith('app/')`. Um caminho que o Git imprima entre aspas pode escapar dessa classificação; renomeação para fora de `app/` também merece caso próprio, pois não há `--no-renames` ou leitura explícita de origem/destino. (`bin/tests/test_lei_akita.sh:92`, `bin/tests/test_lei_akita.sh:144`, `bin/tests/test_lei_akita.sh:145`.) Reprodução com Git real desses dois casos: **[nao verificado]**.

## 31/10 14:02 · sessao 8191a99f63f2 · - · - · origem=Ronald (tmux)

- **A chamada nova fecha o atalho do arsenal.** `bin/pre-push.sh:22` executa o selo antes de `SKIP_ARSENAL=1` (`bin/pre-push.sh:65–69`), enquanto a régua percorre os selos de host apenas depois do retorno antecipado “JA VERDE” (`bin/regua.sh:84–94`, `138–145`). `SKIP_TESTS=1` continua pulando tudo (`bin/pre-push.sh:5`).

- **Pode bloquear push legítimo e julgar outro commit.** A chamada acontece antes de calcular `PUSH_SHA` e reconhecer push só de exclusão (`bin/pre-push.sh:22`, `36–38`). O selo enumera `HEAD` de `R`, independentemente do SHA/ref enviado (`bin/tests/test_lei_akita.sh:17`, `135`). Assim, exclusão de branch ou push de outra ref válida pode ser barrado por commit ruim apenas no HEAD; inversamente, HEAD igual a origin/main deixa vazio o universo desse selo mesmo que a ref empurrada contenha commit ruim (`bin/pre-push.sh:22`, `36`; `bin/tests/test_lei_akita.sh:135–140`). O próprio seletor existente distingue SHA enviado de HEAD e exclusões (`app/core/arvore_do_push.py:8–10`, `19–43`).

- **Raiz fixa agrava clones/worktrees.** O wrapper instalado executa o pre-push da raiz do repositório corrente (`bin/hooks.sh:49–56`), mas a chamada nova aponta para `~/saas-hasner` e o selo usa `/home/ronald/saas-hasner` por padrão (`bin/pre-push.sh:22`; `bin/tests/test_lei_akita.sh:17`). Sem configuração explícita, pode julgar a árvore principal em vez do clone/worktree cujo push disparou o hook (mesmas referências).

- **Chamadores identificados:** pre-push direto (`bin/pre-push.sh:22`); pasta de host pela régua, executando cada `test_*.sh` com Bash e abortando no primeiro vermelho (`bin/regua.sh:138–145`); documentação do molde confirma esse vínculo (`bin/molde_fatia/LEIAME:48–49`). O instalador canônico usa `bin/pre-push.sh` como fonte (`bin/hooks.sh:9–11`); o instalador legado dentro de app também aponta para essa fonte (`app/core/hooks/hooks.sh:9–11`). Hook realmente instalado: [nao verificado].

- **Cache da régua permanece inadequado para selo de histórico:** retorno antecipado compara somente a impressão digital de arquivos Python/HTML (`bin/regua.sh:25–35`, `84–94`), sem comparar mensagens/HEAD/origin/main; portanto uma mudança só no histórico não força o julgamento dos commits pela pasta de host (mesmas referências). A nova chamada cobre esse problema no pre-push comum, antes do atalho do arsenal (`bin/pre-push.sh:22`, `65–69`).

- O pai está disponível em `../4e519aea9b9be16f1ad219c6470d0415bf928e2f`; comparação das versões de `bin/pre-push.sh` mostra apenas as três linhas atuais `20–22`. A identidade Git desse snapshot não foi validada: [nao verificado]. `logs/claude_md_racional/akita_v2.r` não está na árvore disponível: [nao verificado].

## 31/10 14:02 · sessao f63197d45ae2 · - · - · origem=Ronald (tmux)

As três alterações recuperam o sentido das cláusulas selecionadas. A literalidade é do núcleo operativo, com remoção de Markdown/quebras; não houve restauração integral dos parágrafos antigos. Fontes abaixo.

1. **Moratória L-117**
   
   Original, `a7b66d14:CLAUDE.md:644–647`:
   > **(L-117)** Ate `contratos_estruturais` = **20/20** (juiz `core/contratos_estruturais.py::total()`, hoje 15/20), o Code **nao numera nem propoe lei nova por iniciativa propria**: lei nova so nasce de CORTE LITERAL dele. Caso que parece pedir lei vai ao TOPO do RELATO como PERGUNTA com numeros e a fila SEGUE (PAREI-DE-LEI-NAO-DEVOLVE-TURNO). Ela se extingue sozinha em 20/20.

   Atual, `2f5e38c6:CLAUDE.md:453–457`:
   > 3. PERGUNTA DE LEI NAO DEVOLVE TURNO. Falta lei, predicado novo de juiz ou duas candidatas: linha `lei: <a que falta>` no TOPO do RELATO COM NUMEROS, e a esteira SEGUE o proximo item da ORDEM VIVA que nao depende dela; a resposta entra quando vier. Ate 20/20 o Code nao numera nem propoe lei nova por iniciativa propria: lei nova so nasce de CORTE LITERAL dele (L-117). Se uma lei do desenho ESCRITA ja responde [...]

   A volta 8 restaura literalmente, sem o negrito/quebra, “nao numera nem propoe lei nova por iniciativa propria: lei nova so nasce de CORTE LITERAL dele”. Recupera a distinção que o pai perdera ao dizer apenas “nao numera nem propoe lei” (`034b0e38:CLAUDE.md:455`). O encaminhamento da pergunta está no próprio item 3 atual. O referente de 20/20 continua identificável no arquivo: juiz e placar em `2f5e38c6:CLAUDE.md:259–263`, término do estrutural em `:271–272`. **Ressalva de literalidade integral:** a redação atual não repete na moratória o nome do contador, o juiz, o antigo estado 15/20 e a frase explícita de autoextinção; “Ate 20/20” preserva o termo final. Não identifiquei estreitamento/alargamento da proibição restaurada nessas fontes.

2. **AUTONOMIA-DE-CODIGO, item 2**

   Original, `a7b66d14:CLAUDE.md:479–482`:
   > **O Code DECIDE sozinho, registra em UMA linha no RELATO e segue, sem aval**, em quatro coisas: [...] 2. **qual raia ou agente pega qual obra**, e quando uma obra registrada entra como COMPLEMENTO de outra;

   Atual, `2f5e38c6:CLAUDE.md:460–462`:
   > 5. DECISAO TECNICA E DO CODE, em UMA linha no RELATO, sem aval: [...] qual raia ou agente pega qual obra e quando uma obra registrada entra como COMPLEMENTO de outra; [...]

   Recupera exatamente sujeito e relação: **obra registrada** → **COMPLEMENTO de outra**. O pai dizia “quando achado entra como COMPLEMENTO” (`034b0e38:CLAUDE.md:461–462`), confundindo essa delegação com o destino de achados, que já tem enunciado próprio e permanece na sequência (`2f5e38c6:CLAUDE.md:462`). Sem mudança de alcance identificada.

3. **CODEX-PRE-VERIFICADOR**

   Original, `a7b66d14:CLAUDE.md:507`:
   > CODEX-PRE-VERIFICADOR e SUITE-ANTES-DO-OPUS: antes de declarar pronto, Codex `CODEX_PAPEL=verificador` (diff + leis dos sitios + classes que ja reprovaram); procedente corrige e repete (>3 voltas sobe); so entao suite INTEIRA verde, depois UM opus final; medida por obra no RELATO. (PISTA.md#codex-pre-verificador) [nao verificado]

   Atual, `2f5e38c6:CLAUDE.md:543–549`:
   > 5. PRE-VERIFICADOR. (a) TODO construtor, ANTES de declarar pronto, `CODEX_PAPEL=verificador` com: diff do hash da copia; LEIS dos sitios tocados [...] (c) Suite INTEIRA da arvore da obra VERDE, depois UM opus final [...] [nao verificado]

   A cláusula original não restringe a “cura de negocio”; essa restrição aparece no pai (`034b0e38:CLAUDE.md:543`) e é removida na volta 8. “TODO construtor” explicita o alcance geral já contido no comando antigo “antes de declarar pronto”. **Não é reprodução literal do enunciado antigo**, pois “TODO construtor” não constava da linha 507; é restauração semântica do alcance, não ampliação demonstrável por esses textos. O ponteiro `PISTA.md#codex-pre-verificador` permanece em `2f5e38c6:CLAUDE.md:518`.

Não encontrei defeito material adicional nas três substituições acima; eventuais diferenças nas outras partes das consolidações não foram objeto desta subtarefa. [não verificado]

## 31/10 14:02 · sessao e868ae83e323 · - · - · origem=Ronald (tmux)

As remoções desta volta não retiram regra, condição ou ponteiro específico:

- **PARADA E AUTONOMIA:** sai somente `; historia em LAPIDES` (`034b0e38:CLAUDE.md:440`). Continuam os caminhos dos instrumentos (`2f5e38c6:CLAUDE.md:440`), o selo e as seis âncoras `lap-45/46/17/16/11/49` (`2f5e38c6:CLAUDE.md:442-443`).
- **RAIAS E POUSO:** sai somente `; historia em LAPIDES` (`034b0e38:CLAUDE.md:479`). Continuam os nomes das regras (`2f5e38c6:CLAUDE.md:477-479`), os ponteiros PISTA (`2f5e38c6:CLAUDE.md:481`) e `lap-52/15/53` (`2f5e38c6:CLAUDE.md:482`).
- **DINHEIRO:** sai somente `; historia em LAPIDES` (`034b0e38:CLAUDE.md:573`). Continuam as referências L-092/L-110 (`2f5e38c6:CLAUDE.md:573`) e `lap-55/59/08/09/10/48/12` (`2f5e38c6:CLAUDE.md:574`).
- **PLACAR:** sai somente `, resposta a uma pergunta de LEI que eu levantei` (`034b0e38:CLAUDE.md:566`). Essa informação histórica não reaparece literalmente, mas não enuncia condição ou regra. Autor e data permanecem (`2f5e38c6:CLAUDE.md:566`); exercício, dois verdes, preservação da L-095 e `lap-18` permanecem (`2f5e38c6:CLAUDE.md:567-570`).

A obrigação geral de mover história sem apagar, mantendo ponteiro e regra, também permanece (`2f5e38c6:CLAUDE.md:614-615`).

Extração por `LAPIDES\.md#lap-\d+`: **69 ocorrências, 68 destinos únicos**, com sequência integral idêntica em `a7b66d14`, `034b0e38` e `2f5e38c6`; a duplicata é `lap-63` (`2f5e38c6:CLAUDE.md:231` e `2f5e38c6:CLAUDE.md:235`). Nenhum defeito adicional identificado nas remoções examinadas.

## 31/10 14:02 · sessao cebde2888d4a · - · - · origem=Ronald (tmux)

A função real, extraída e executada apenas em memória, produz **55 termos normalizados**, além das duas exceções `n/a`; remove parênteses, asteriscos e crases, converte para minúsculas e normaliza espaços. (`bin/tests/test_lei_akita.sh:61–90`)

Lista exata, agrupada pela origem:

| Origem | Termos normalizados aceitos |
|---|---|
| `app/docs/GLOSSARIO.md:11` | single source of truth; aggregate root; celula soberana; fonte unica |
| `app/docs/GLOSSARIO.md:12` | single writer principle; juiz unico; escritor unico |
| `app/docs/GLOSSARIO.md:13` | sinal derivado do estado; reconciliacao level-triggered; lampada |
| `app/docs/GLOSSARIO.md:14` | architectural invariants + fitness functions; contratos estruturais |
| `app/docs/GLOSSARIO.md:15` | architecture conformance test; selo |
| `app/docs/GLOSSARIO.md:16` | event-driven architecture; crons virando evento |
| `app/docs/GLOSSARIO.md:17` | transactional outbox; o-outbox |
| `app/docs/GLOSSARIO.md:18` | strangler fig pattern; construir ao lado e trocar inteira |
| `app/docs/GLOSSARIO.md:19` | shadow mode; parallel run; sombra |
| `app/docs/GLOSSARIO.md:20` | merge queue; comboio; pouso |
| `app/docs/GLOSSARIO.md:21` | backfill; reprocessing; rejulgar o passivo |
| `app/docs/GLOSSARIO.md:22` | cqrs com projecoes reconstruiveis; espelho; diapago; fechamento; retrato e pilulas |
| `app/docs/GLOSSARIO.md:23` | duplicated business logic; logic drift; derivacao paralela |
| `app/docs/GLOSSARIO.md:24` | bitemporal data; vinculo do dia x vinculo de hoje |
| `app/docs/GLOSSARIO.md:25` | immutable event log + compensating event; batida so retratada |
| `app/docs/GLOSSARIO.md:26` | adr; leis; cortes |
| `app/docs/GLOSSARIO.md:27` | idempotencia pela chave do fato; dedup do fato |
| `app/docs/PLANO-PADROES.md:46–48` | um juiz por pergunta; single writer; evento em vez de varredura; log imutavel; outbox; bitemporal; projecao reconstruivel; cqrs; fitness function |

`n/a (medida)` e `n/a (instrumento)` são aceitos exatamente nessa grafia quando nenhum caminho classificado como negócio aparece; a classificação é textual: começa com `app/`, não contém `/tests/` e não começa com `app/docs/`. (`bin/tests/test_lei_akita.sh:90–103`)

**Divergência documental:** a implementação acompanha a enumeração restrita do NORTE e as duas primeiras colunas do glossário; porém `CLAUDE.md:68` admite termo existente em qualquer dos dois arquivos, e `PLANO-PADROES.md:53` diz “termo do GLOSSARIO ou deste arquivo”. Pela redação ampla, há falsos vermelhos comprovados: `projection rebuild` e `read model replay` aparecem como padrões conhecidos (`app/docs/PLANO-PADROES.md:33`), `Architecture Decision Record` aparece como padrão conhecido (`app/docs/PLANO-PADROES.md:41`) e no próprio glossário (`app/docs/GLOSSARIO.md:26`), mas todos são recusados. A causa é limitar o plano ao trecho NORTE e remover todo conteúdo entre parênteses antes da extração. (`bin/tests/test_lei_akita.sh:77–87`, `bin/tests/test_lei_akita.sh:104–105`)

**Falsos verdes comprovados na função real:**

- `padrao=Padrao Inventado Do Nada, padrao=juiz unico` retorna aprovação: escolhe somente o último campo, mesmo com duas declarações explícitas de padrão. O MORDE verifica duas linhas, mas não duas declarações na mesma linha. (`bin/tests/test_lei_akita.sh:94–98`, `bin/tests/test_lei_akita.sh:162`)
- `padrao=juiz unico (Padrao Inventado Do Nada)` retorna aprovação porque todo texto entre parênteses desaparece; o campo completo inexiste na lista documental. (`bin/tests/test_lei_akita.sh:61–63`, `bin/tests/test_lei_akita.sh:104`)
- `padrao=sinal derivado do estado, reconciliacao level-triggered`, copiando a célula técnica do glossário sem seu comentário parentético, retorna “linha LEI-AKITA sem padrao=”: a regex não consegue consumir vírgula de valor. As duas partes isoladas são aceitas. (`app/docs/GLOSSARIO.md:13`, `bin/tests/test_lei_akita.sh:85`, `bin/tests/test_lei_akita.sh:95–97`)

Não há aceite de substrings como `a` ou `Writer Principle`: a comparação é por pertinência do termo normalizado ao conjunto; os dois exemplos têm MORDE negativo explícito. (`bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:160–161`)

_1095 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
