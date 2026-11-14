# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 10:34** · publicado em **14/11/2026 10:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 10:34 · sessao 8f443ecca58e · HJ-091 · sonda · origem=Code

Transcrição do dado raspado; os números abaixo não foram confirmados em produção. [nao verificado]

Universo: **3001 ausências de férias, 116 agendamentos, 2943 dias vivos**. [nao verificado]

1. **`ferias_ausencia_sem_agendamento` — aprovadas sem agendamento vivo cobrindo: 58; colaboradores: 53.** PKs literais: `4635, 4839, 4165, 4613, 4630, 4634, 4633, 4631, 4615, 4614, 4620, 4617, 4611, 4623, 4628, 4618, 4624, 4625, 4627, 4629, 4621, 4626, 4622, 4651, 4652, 4654, 4653, 4656, 4664, 4663, 4661, 4662, 4655, 4658, 4659, 4660, 4657, 4665, 4641, 4666, 4667, 4642, 4636, 4669, 4643, 4668, 4644, 4645, 4637, 4646, 4649, 4648, 4647, 4650, 4638, 4639, 4640, 4632`. [nao verificado]

2. **`ferias_dia_sem_ausencia` — dias de agendamento vivo sem ausência aprovada: 4; colaboradores: 2.** A saída fornece tuplas `(colaborador_id, data, [agendamento_id])`, sem PK de ausência: `(127, '2026-10-02', [114]), (344, '2026-11-05', [103]), (344, '2026-11-06', [103]), (344, '2026-11-07', [103])`. [nao verificado]

3. **`rejeitada_ou_cancelada_dentro_de_agendamento_vivo`: 3; colaboradores: 1.** PKs literais: `3977, 3995, 3985`. Todas constam como `rejeitada`, colaborador `344`, cobertura pelo agendamento `103`. [nao verificado]

4. **Backfill — sem marca `#N` na descrição: 58.** PKs literais: `4635, 4612, 4839, 4613, 4630, 4634, 4633, 4631, 4615, 4614, 4620, 4617, 4611, 4623, 4628, 4618, 4624, 4625, 4627, 4629, 4621, 4626, 4622, 4651, 4652, 4654, 4653, 4656, 4664, 4663, 4661, 4662, 4655, 4658, 4659, 4660, 4657, 4665, 4641, 4666, 4667, 4642, 4636, 4669, 4643, 4668, 4644, 4645, 4637, 4646, 4649, 4648, 4647, 4650, 4638, 4639, 4640, 4632`. A PK `4612` está rejeitada; as demais, aprovadas. [nao verificado]

5. **Backfill — `#N` não é agendamento vivo cobrindo o dia: 2.** PKs literais: `4165, 2634`, ambas aprovadas; marcas de agendamento `109` e `77`, respectivamente. [nao verificado]

**#3977 aparece na classe 3**, rejeitada, com marca `#103` e cobertura `[103]`. **#2634 aparece na classe 5**, aprovada, com marca `#77` e cobertura `[114]`. [nao verificado]

As classes específicas **“rejeitadas sem origem dentro de agendamento vivo”** e **“aprovadas sem origem dentro de agendamento vivo”** não têm contagens nem PKs apresentados como tais no recorte; seus resultados são **[nao verificado]**, não zero. [nao verificado]

No trecho fornecido: **mensagens ERRO: 0; Traceback: 0**. O encerramento literal é **`ERROS 0`**. [nao verificado]

## 14/11 10:34 · sessao 5409579ac854 · HJ-091 · sonda · origem=Code

| Campo | Valor |
|---|---|
| sonda_hash | `f8fc49270edcade2` [nao verificado] |
| cpuset | `4-7` [nao verificado] |
| modo | LEITURA [nao verificado] |
| universo_admin_encerrado_colab_com_ferias | 845 [nao verificado] |
| com_dia_do_fato | 419 [nao verificado] |
| dia_coberto_por_ferias | 27 [nao verificado] |
| veto_admin_sem_fato_novo_ativo_hoje | 16 [nao verificado] |
| erros | 0 [nao verificado] |
| pks_veto | `[3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801]` [nao verificado] |
| Ocorrências literais de `ERRO` no dado anexo | 0 [nao verificado] |
| Ocorrências literais de `Traceback` no dado anexo | 0 [nao verificado] |

## 14/11 10:33 · sessao 3a83fe721204 · [nome]-ANDAM-JUNTO · verificador · origem=Code

O aval foi confrontado com a transcrição fornecida; leitura do arquivo literal em `logs/AVAIS-1313/ramos_andam_junto.txt`: **[nao verificado]**.

1. **PROCEDE: aceita tabelas de 8 e 10 colunas sem deslocamento.** O cabeçalho determina o esquema; uma linha de oito células sob cabeçalho de dez vai para “nao lidas”. Cabeçalhos de obras fora do esquema também provocam rejeição das linhas. (`bin/placar_grade.py:70`, `bin/placar_grade.py:75`, `bin/placar_grade.py:81`, `bin/placar_grade.py:84`)

   **PROCEDE: existem células semanticamente inválidas que entram na contagem.** `REGISTRADAxyz` passa porque o estado usa `startswith`; `lei`, `arquivo congelado` e `colisao com` passam sem identificar qual lei, arquivo ou obra. Número sem fonte também passa, embora o aval peça fonte. Já `fecha BO=abc` e `portao=talvez` são rejeitados. (`bin/placar_grade.py:86`, `bin/placar_grade.py:49`, `bin/placar_grade.py:54`, `bin/placar_grade.py:95`, `bin/placar_grade.py:98`)

2. **PROCEDE: a soma implementa a adição dos valores informados por estado.** O resultado publicado é EM VOO **2**, BRIEF PRONTO **1**, REGISTRADA **9**, demais estados **0**; **6 de 297** linhas medidas. Isso soma os números registrados; correspondência com BO distintos e ausência de sobreposição: **[nao verificado]**. (`bin/placar_grade.py:109`, `bin/placar_grade.py:113`, `app/docs/GRADE-MESTRA.md:26`)

   **PROCEDE: complemento não aumenta a contagem de obras.** Ele fica separado, participa da soma e aparece no ranking identificado como complemento. Totais, estados e baldes de obras usam somente `lidas`. (`bin/placar_grade.py:103`, `bin/placar_grade.py:133`, `bin/placar_grade.py:139`, `bin/placar_grade.py:223`, `bin/placar_grade.py:225`)

   **PROCEDE: `?` entra em N, mas não em k.** Isso vale para obras e complementos válidos de dez colunas; tabelas de oito colunas e linhas rejeitadas ficam fora desse denominador. (`bin/placar_grade.py:48`, `bin/placar_grade.py:109`, `bin/placar_grade.py:114`, `bin/placar_grade.py:116`)

3. **PROCEDE: há mordida nos asserts novos abaixo.** Cada mutante indicado altera o `.py` e derruba a respectiva verificação. (`bin/tests/test_placar_grade.sh:106`–`121`)

| Assert em `bin/tests/test_placar_grade.sh` | Mutante do `.py` que o derruba |
|---|---|
| `:106`, `--placar` termina com sucesso | Provocar exceção na seção nova: `bin/placar_grade.py:222`. |
| `:107`, quatro obras | Encaminhar complementos para `lidas`: `bin/placar_grade.py:103`. |
| `:108`, uma não lida | Desligar rejeição de portão: `bin/placar_grade.py:98`. |
| `:109`, inválida listada com motivo | Remover listagem das rejeitadas: `bin/placar_grade.py:230`. |
| `:111`, soma e denominador exatos | Excluir complementos da soma, ou converter `?` em zero: `bin/placar_grade.py:109`, `bin/placar_grade.py:48`. |
| `:112`, linha exata de FB-QUATRO | Imprimir apenas o número, perdendo a fonte: `bin/placar_grade.py:225`. |
| `:113`, ordem e exclusão das não medidas | Ordenar crescente: `bin/placar_grade.py:224`. |
| `:117`, conferência verde | Retornar erro no caminho válido: `bin/placar_grade.py:189`. |
| `:118`, soma no cabeçalho | Remover inclusão da soma: `bin/placar_grade.py:143`. |
| `:120`, fixture realmente mudou | **Não verifica o `.py`**; protege a transformação feita pelo `sed` em `:119`. |
| `:121`, alteração deixa cabeçalho vermelho | Desligar comparação do cabeçalho: `bin/placar_grade.py:183`. |

   **NAO PROCEDE: cobertura completa das rejeições.** Faltam casos com número inválido, zero, largura incompatível e estado inválido de complemento. Remover a rejeição de número inválido ou a conferência de largura passa pelas fixtures existentes; trocar `fb is False` por teste de falsidade também deixa o erro de rejeitar zero escapar. (`bin/tests/test_placar_grade.sh:83`–`104`; `bin/placar_grade.py:81`, `bin/placar_grade.py:95`)

   **NAO PROCEDE: FB-COMPL ausente deixando o assert vazio.** A linha existe; o `sed` a altera e o `cmp` acusa transformação sem efeito. Porém, retirar o sufixo `(compl.)` escapa: o `awk` verifica somente o identificador em `$2`. (`bin/tests/test_placar_grade.sh:98`, `bin/tests/test_placar_grade.sh:113`, `bin/tests/test_placar_grade.sh:119`, `bin/tests/test_placar_grade.sh:120`; `bin/placar_grade.py:225`)

4. **NAO PROCEDE quebra de formato nos consumidores identificados.** O conferidor usa marcadores, o selo espera a soma nova e a publicação transporta os documentos inteiros. (`bin/placar_grade.py:147`, `bin/placar_grade.py:183`, `bin/tests/test_placar_grade.sh:111`, `bin/tests/test_placar_grade.sh:118`)

| Consumidor/caminho | Compatibilidade |
|---|---|
| `relato.sh` | Gera pelo leitor atualizado, raspa e inclui GRADE/PLACAR na publicação: `bin/relato.sh:101`, `bin/relato.sh:119`, `bin/relato.sh:120`, `bin/relato.sh:137`. |
| Raspador | Lê texto inteiro e aplica máscara, sem interpretar colunas: `bin/raspar_publico.py:28`, `bin/raspar_publico.py:31`. |
| Régua/selos | A régua executa o selo por `test_*.sh`; ele confere também a GRADE viva: `bin/regua.sh:138`, `bin/tests/test_placar_grade.sh:124`. |
| `pos_push` | Opera o derivado de TICKETS: `bin/pos_push.sh:19`, `bin/pos_push.sh:20`, `bin/pos_push.sh:29`. |
| Hook Stop | Usa BACKLOG e RELATO: `bin/hook_stop_fila1.py:33`, `bin/hook_stop_fila1.py:34`. |

   Outros leitores externos ou seleção automática da vaga pela tabela: **[nao verificado]**. O código examinado escreve o ranking; essa escrita não comprova um consumidor automático. (`bin/placar_grade.py:222`–`226`)

5. **PROCEDE o desvio de portão em DECLARADO-X-GRAVADO; os demais preenchimentos têm as ressalvas abaixo.** (`app/docs/GRADE-MESTRA.md:270`; `app/docs/RELATO.md:7`)

| Linha preenchida | Veredito |
|---|---|
| DECLARADO-X-GRAVADO | **PROCEDE erro:** registra colisão, omitindo o impedimento de lei pedido no aval. O RELATO confirma retirada da ordem até corte e cinco questões abertas. `app/docs/GRADE-MESTRA.md:270`; `app/docs/RELATO.md:7`, `app/docs/RELATO.md:24`. |
| BO-ADMIN-2710 | **NAO PROCEDE divergência:** `censo` corresponde à pendência de medidas, dono e destino. `app/docs/GRADE-MESTRA.md:135`. |
| RAIZ-DA-DISPUTA | **NAO PROCEDE categoria inválida:** identifica E3 e `chamados/models.py`. Interseção atual dos diffs: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:106`. |
| CONTRATO-DE-ENTRADA-DO-FIO | **NAO PROCEDE categoria inválida:** identifica E3 e `chamados/juizes.py`. Interseção atual dos diffs: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:685`. |
| FIO-E0-V4-E3-LEITORES | **PROCEDE inconsistência:** registra colisão consigo mesma; isso não demonstra arquivo comum com outra raia, critério de RAIAS. Portão substituto: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:117`; `CLAUDE.md:309`. |
| O232 | **PROCEDE falta de sustentação do “em voo”:** aponta JUSTIFICATIVA, que a própria GRADE registra INTERROMPIDA. Colisão atual com agente vivo: **[nao verificado]**. `app/docs/GRADE-MESTRA.md:851`, `app/docs/GRADE-MESTRA.md:267`. |

Validação do leitor e mutantes feita em memória. Execução integral do selo shell: **[nao verificado]**; ele cria e remove arquivos temporários, contrariando a restrição desta auditoria. (`bin/tests/test_placar_grade.sh:17`, `bin/tests/test_placar_grade.sh:71`)

## 14/11 10:33 · sessao 9bd2d678f7ef · - · - · origem=Ronald (tmux)

| ID | col/dia na linha do BACKLOG | Obra na GRADE-MESTRA | Portão e prova |
|---|---|---|---|
| HJ-057 | nenhuma linha encontrada [nao verificado] | nenhuma [nao verificado] | censo [nao verificado] |
| HJ-058 | col=n/a; ocorrência 06/11, universo desde 30/09 — app/docs/BACKLOG.md:2510 | UI-ANEXO-AUSENCIA — app/docs/GRADE-MESTRA.md:1517; O30 — app/docs/GRADE-MESTRA.md:480 | lei: vale da F1 antes de F2/F3 — app/docs/BACKLOG.md:1311; rebuild com pillow_heif também pendente — app/docs/GRADE-MESTRA.md:1517 |
| HJ-058-MEDIA-AUSENCIAS | col/dia=n/a — app/docs/BACKLOG.md:2511 | nenhuma [nao verificado] | censo: origem antes de cura — app/docs/BACKLOG.md:2511 |
| HJ-066 | col/dia=n/a — app/docs/BACKLOG.md:2503 | nenhuma [nao verificado] | censo de arquivos/diff atual [nao verificado] |
| HJ-067 | apenas referência, sem col/dia — app/docs/BACKLOG.md:2500 | nenhuma vinculação inequívoca [nao verificado]; RAIZ-DA-DISPUTA existe em app/docs/GRADE-MESTRA.md:103 | censo: F0 antes de HJ-067 — app/docs/BACKLOG.md:2500; colisão atual [nao verificado] |
| HJ-068 | col/dia=n/a — app/docs/BACKLOG.md:2504 | nenhuma [nao verificado] | censo de arquivos/diff atual [nao verificado] |
| HJ-069 | apenas referência, sem col/dia — app/docs/BACKLOG.md:2492 | nenhuma [nao verificado]; UI-ESCALA-REQUADRO não aparece na GRADE [nao verificado] | censo: drawer antes; vaga exige ausência de arquivo comum — app/docs/BACKLOG.md:2492 |
| HJ-071 | col/dia=n/a — app/docs/BACKLOG.md:2505 | nenhuma [nao verificado] | censo: medir primeiro se hora_ini/hora_fim estão gravadas — app/docs/BACKLOG.md:2505 |
| HJ-073 | col/dia=n/a — app/docs/BACKLOG.md:2506 | nenhuma [nao verificado] | censo + F0 antes da cura — app/docs/BACKLOG.md:2506. O RELATO registra resultado do censo, sem prova de cura: app/docs/RELATO.md:532 |
| HJ-074 | col148 / 02/11 — app/docs/BACKLOG.md:2507 | FERIADO-ESCALA — app/docs/GRADE-MESTRA.md:122 | lei: FERIADO-USA-A-LEI-DE-1609 é dependência nomeada — app/docs/GRADE-MESTRA.md:122; já havia construção na linha app/docs/BACKLOG.md:728. Bloqueio atual ou “livre” [nao verificado] |
| HJ-075 | col1032 / dia=n/a; 13/11 08:52 é conferência do servidor — app/docs/BACKLOG.md:2508 | nenhuma [nao verificado] | censo: aguarda print para confirmar causa — app/docs/BACKLOG.md:2508 |
| HJ-075-EC-FIRST | col/dia=n/a — app/docs/BACKLOG.md:2509 | nenhuma [nao verificado] | censo de alcance/arquivos atual [nao verificado]; linha registra juiz duplicado de vínculo — app/docs/BACKLOG.md:2509 |
| HJ-090 | col/dia=n/a na linha própria — app/docs/BACKLOG.md:1874 | HJ-090 — app/docs/GRADE-MESTRA.md:263 | “livre” como bloqueio atual [nao verificado]; a cópia registra pouso/NO AR em app/docs/RELATO.md:8. Não classificar como ainda colidindo apenas pela GRADE antiga |
| HJ-091 | referência, col/dia=n/a — app/docs/BACKLOG.md:1874 | HJ-090, resolve também HJ-091 — app/docs/GRADE-MESTRA.md:263 | colisão com HJ-090 em ferias/models.py e ferias/services.py registrada em app/docs/RELATO.md:98; esse bloqueio é histórico, pois HJ-090 consta pousada em app/docs/RELATO.md:8. Portão atual: censo [nao verificado] |
| HJ-092 | col/dia=n/a; há cel128589 — app/docs/BACKLOG.md:741 | HJ-092 — app/docs/GRADE-MESTRA.md:123; TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO — app/docs/GRADE-MESTRA.md:464 | lei/decisão humana: classe BATIDA, candidatas ao admin — app/docs/BACKLOG.md:741; dependência decisão do admin — app/docs/GRADE-MESTRA.md:123 |
| HJ-093 | col1004 / 27/10 — app/docs/BACKLOG.md:743 | JUSTIFICATIVA-PAGA-O-PERIODO — app/docs/GRADE-MESTRA.md:264; HJ-093 — app/docs/GRADE-MESTRA.md:1371 | lei O168 para pergunta sem veredito — app/docs/BACKLOG.md:743; cobertura financeira exige DIFF/motor/folha — app/docs/GRADE-MESTRA.md:264. Colisão atual com E3 [nao verificado] |
| HJ-095 | referência, col/dia=n/a — app/docs/BACKLOG.md:2109 | CONTRATO-DE-ENTRADA-DO-FIO — app/docs/GRADE-MESTRA.md:682 | censo/portão de arquivo atual [nao verificado]; dependência documentada E3 e DESLIGADO/FERIADO — app/docs/BACKLOG.md:2109. Não converter dependência em colisão sem diff |

Todos os IDs HJ-057+ acima: **“nao medida pela sonda”**; cobertura deriva exclusivamente do anexo, portanto [nao verificado] no padrão arquivo:linha exigido.

Não encontrei na cópia a lista dos seis arquivos congelados da E3; qualquer portão “arquivo congelado” específico seria [nao verificado]. A regra disponível exige arquivo comum, não família/app: CLAUDE.md:309.

## 14/11 10:32 · sessao fbb17bd64033 · - · - · origem=Ronald (tmux)

- Regra exata do mapa: divergente quando `d.segura=true`, dono por `InsumosDoDono.dono`; sem divergência quando não segura e lavrado não irregular; indeterminado quando sem col/dia, futuro ou irregular sem segurar. `app/docs/RELATO.md:8252`.
- Universo original: 80; 50 com col/dia e 30 sem identificação suficiente. `app/docs/RELATO.md:8251`.
- Totais originais: divergente ESTRUTURA 6, CADASTRO 3, BATIDA 5; sem divergência 33; indeterminado 33. `app/docs/RELATO.md:8255`, `:8256`, `:8257`, `:8258`, `:8259`.
- Totais hoje pelo dado fornecido: divergente ESTRUTURA 3 (`HJ-012-U2`, `HJ-017-U1`, `HJ-030-U1`); CADASTRO 3 (`HJ-001-U1`, `HJ-001-U2`, `HJ-016-U1`); BATIDA 3 (`HJ-003-U1`, `HJ-015-U1`, `HJ-027-U1`); sem divergência 38; indeterminado 33. Soma `3+3+3+38+33=80`. `[nao verificado]` — derivação dos anexos sem arquivo local.
- Mudanças de classe: `HJ-002-U1` D→S; `HJ-005-U1` D→I; `HJ-005-U2`, `HJ-005-U4`, `HJ-005-U5` D→S; `HJ-050-U2` I→S. `[nao verificado]` — anexos.
- Mudanças adicionais sem mudança de classe: `HJ-005-U3` export fora→entra e chamados resolvido→fechado; `HJ-013-U3` export fora→entra; `HJ-014-U2` export entra→fora. `[nao verificado]` — anexos.
- `HJ-002-U1`: status irregular→ok, export fora→entra, dono bruto BATIDA→ESTRUTURA, chamado `31891` em_analise→resolvido; segura true→false reconstruído do motivo antigo e do dado novo. `[nao verificado]`.
- `HJ-005-U1/U2/U4`: export fora→entra, chamados resolvido→fechado, segura true→false; U1 permanece irregular, U2/U4 permanecem escala_suspeita; U1 vira indeterminado porque irregular sem segurar. `[nao verificado]`.
- `HJ-005-U5`: export mantém fora; motivo novo `rescisao_modulo_proprio`, segura true→false, chamado `33487` resolvido→fechado; antigo motivo de export não consta, portanto delta desse motivo não demonstrado. `[nao verificado]`.
- `HJ-050-U2`: previsto→ok, futuro na leitura→não futuro; export mantém entra. `[nao verificado]`.
- Cuidado de normalização: o JSON antigo publica `dono=null` para S/I; isso não mede o `b.dono` bruto dessas unidades. Não interpretar `null→ESTRUTURA/CADASTRO/BATIDA` como mudança de dono real. `[nao verificado]` — esquema dos anexos.
- `grupo` e `obra` não aparecem na saída nova a–f; remedição desses campos não demonstrada. O registro antigo informa obra por unidade no mapa privado e classificação de obras ainda pendente. `app/docs/RELATO.md:8260`, `app/docs/RELATO.md:8265`.
- O registro aponta `logs/hj_0511/sonda_a.py` como instrumento, mapa privado e saída histórica; o diretório é ignorado. `app/docs/RELATO.md:8250`, `app/docs/RELATO.md:8263`, `app/docs/RELATO.md:8264`, `.gitignore:53`. Busca local não encontrou o script/mapa/JSON anexado `[nao verificado]`.
- Assinaturas atuais: `InsumosDoDono.__init__(..., *, agora, ...)` em `app/ponto/management/commands/e6_oraculo.py:128`; `dono(self,data)` em `:182`; `status_do_dia_lavrado(...,*,agora=None)` em `app/ponto/status_dia.py:201`; `classificar_export(empresa,mes,ano,fechamentos=None)` em `app/folha/export.py:765`; `porta_alcancavel_do_dia(celula_id,*,alimentacao=None)` em `app/chamados/juizes.py:1622`.
- Erros hoje: anexo informa `com_erro=0`, `processadas=50`, `puladas=30`, `soma=80`; nenhuma unidade com erro demonstrada. Mudança de assinatura desde 05/11 não verificável sem versões do script; não cabe inventar erro nem converter erro em zero. `[nao verificado]`.

## 14/11 10:32 · sessao 5535b70aa6a8 · [nome]-ANDAM-JUNTO · censo · origem=Code

padrao=n/a (medida). Universo original: **8 itens**, enumerados em `app/docs/CORTES.md:308`; complemento do item 8: **3 subitens**, em `app/docs/CORTES.md:310`.

| id | dono fechado | destino |
|---|---|---|
| HJ-005-U1 · col42 | ESTRUTURA — `app/docs/RELATO.md:8255` | Por unidade: [não verificado] — `app/docs/RELATO.md:8265` |
| HJ-005-U2 · col42 | ESTRUTURA — `app/docs/RELATO.md:8255` | Por unidade: [não verificado] — `app/docs/RELATO.md:8265` |
| HJ-005-U4 · col42 | ESTRUTURA — `app/docs/RELATO.md:8255` | Por unidade: [não verificado] — `app/docs/RELATO.md:8265` |

Os três casos de col42 estão registrados conjuntamente com pk4803/pk4867/pk5101 em **COBERTURA-PARCIAL-UM-JUIZ**, `app/docs/BACKLOG.md:786`. A correspondência individual HJ-005-U1/U2/U4 → pk permanece [não verificado].

| id do item | dono L-099 | destino registrado |
|---|---|---|
| BO-ADMIN-2710.1 · col42 | **ESTRUTURA** — `app/docs/RELATO.md:3311` | **COBERTURA-PARCIAL-UM-JUIZ** — `app/docs/BACKLOG.md:786`; `app/docs/GRADE-MESTRA.md:34` |
| BO-ADMIN-2710.2 · col789 | **ESTRUTURA** — `app/docs/CORTES.md:312`; substituição da classificação anterior em `app/docs/CORTES.md:315` | **JUIZ-DO-PAR-DUAS-ENTRADAS** — encaminhamento em `app/docs/CORTES.md:315`; `app/docs/BACKLOG.md:776`; `app/docs/GRADE-MESTRA.md:33` |
| BO-ADMIN-2710.3 · col789/col129 | **ESTRUTURA** — `app/docs/RELATO.md:3313`; col129 confirmado em `app/docs/CORTES.md:354` | col789, pk2916: **JUSTIFICATIVA-PAGA-O-PERIODO** — `app/docs/BACKLOG.md:1875`; `app/docs/GRADE-MESTRA.md:264`. col129: **JUIZ-DO-PAR-DUAS-ENTRADAS** — `app/docs/CORTES.md:354`; `app/docs/BACKLOG.md:776` |
| BO-ADMIN-2710.4 · col789 · pk22430 | **CADASTRO** — `app/docs/CORTES.md:315` | **lista do admin** — `app/docs/CORTES.md:312` |
| BO-ADMIN-2710.5 · col247 · pk34577 | **ESTRUTURA** — `app/docs/RELATO.md:3315` | **JUIZ-DO-PAR-DUAS-ENTRADAS** — encaminhamento posterior em `app/docs/CORTES.md:313`; `app/docs/BACKLOG.md:776`; `app/docs/GRADE-MESTRA.md:33` |
| BO-ADMIN-2710.6 · col42/col129/col789 | **ESTRUTURA** — `app/docs/RELATO.md:3316` | **FIO-LE-O-JUIZ** — `app/docs/BACKLOG.md:794`; `app/docs/GRADE-MESTRA.md:35`. Classe adicional: **Q-VIVAS-LEITORES-SEM-JUIZ** — `app/docs/RELATO.md:3316`; `app/docs/BACKLOG.md:1521`; `app/docs/GRADE-MESTRA.md:604` |
| BO-ADMIN-2710.7 · col130 · pk28883 | **ESTRUTURA** — `app/docs/RELATO.md:3317` | **REGISTRADO-NAO-E-VIVO** — `app/docs/BACKLOG.md:780`; `app/docs/GRADE-MESTRA.md:134` |
| BO-ADMIN-2710.8 · col789 · pk1473 | **CADASTRO** — `app/docs/RELATO.md:3318` | **lista do admin** — `app/docs/RELATO.md:3308` |
| BO-ADMIN-2710.8.1 · col789 | **ESTRUTURA** — `app/docs/RELATO.md:3318` | **TRILHA-DO-VINCULO-MENTE** — `app/docs/BACKLOG.md:778`; `app/docs/GRADE-MESTRA.md:133` |
| BO-ADMIN-2710.8.2 · col789 | **dono não fechado** — `app/docs/RELATO.md:3318` | **TENTATIVA-DO-ADMIN-VIRA-COBRANCA** — `app/docs/BACKLOG.md:779`; `app/docs/GRADE-MESTRA.md:467` |
| BO-ADMIN-2710.8.3 · col789 · pk1473 | **dono não fechado individualmente**; encaminhamento coletivo ESTRUTURA, letra F — `app/docs/CORTES.md:313` | **E4-REGENERAR-VINCULO-SEM-VIGENCIA**, complemento — `app/docs/RELATO.md:3318`; obra-mãe **CASA-ESCALA-E4** — `app/docs/BACKLOG.md:1683`; `app/docs/GRADE-MESTRA.md:657` |

No item 3, os **19 casos sem justificativa** continuam com causa não medida; dono individual não fechado — `app/docs/RELATO.md:3313`. O destino separado de col789 substitui o agrupamento inicial com o item 1 — `app/docs/CORTES.md:330`.

Contagem por dono, calculada sobre as linhas acima, com os oito originais de `app/docs/CORTES.md:308` e os três complementos de `app/docs/CORTES.md:310`:

| dono | 8 originais | 3 complementos | total |
|---|---:|---:|---:|
| ESTRUTURA | 6 | 1 | 7 |
| CADASTRO | 2 | 0 | 2 |
| BATIDA | 0 | 0 | 0 |
| decisão do admin | 0 | 0 | 0 |
| dono não fechado | 0 | 2 | 2 |
| **soma** | **8** | **3** | **11** |

Contagem por destino em **conjuntos exclusivos**: cada item conta uma vez, inclusive quando tem dois destinos; enumeração de origem em `app/docs/CORTES.md:308` e `app/docs/CORTES.md:310`.

| destino | ids dos itens | n |
|---|---|---:|
| COBERTURA-PARCIAL-UM-JUIZ | 1 — `app/docs/BACKLOG.md:786` | 1 |
| JUIZ-DO-PAR-DUAS-ENTRADAS | 2, 5 — `app/docs/CORTES.md:313` | 2 |
| JUSTIFICATIVA-PAGA-O-PERIODO + JUIZ-DO-PAR-DUAS-ENTRADAS | 3 — `app/docs/BACKLOG.md:1875`; `app/docs/CORTES.md:354` | 1 |
| lista do admin | 4, 8 — `app/docs/CORTES.md:312`; `app/docs/RELATO.md:3308` | 2 |
| FIO-LE-O-JUIZ + Q-VIVAS-LEITORES-SEM-JUIZ | 6 — `app/docs/RELATO.md:3316`; `app/docs/CORTES.md:313` | 1 |
| REGISTRADO-NAO-E-VIVO | 7 — `app/docs/BACKLOG.md:780` | 1 |
| TRILHA-DO-VINCULO-MENTE | 8.1 — `app/docs/BACKLOG.md:778` | 1 |
| TENTATIVA-DO-ADMIN-VIRA-COBRANCA | 8.2 — `app/docs/BACKLOG.md:779` | 1 |
| CASA-ESCALA-E4 / E4-REGENERAR-VINCULO-SEM-VIGENCIA | 8.3 — `app/docs/BACKLOG.md:1683`; `app/docs/RELATO.md:3318` | 1 |
| **soma: 8 originais + 3 complementos** | `app/docs/CORTES.md:308`; `app/docs/CORTES.md:310` | **11** |

## 14/11 10:31 · sessao 0ab81a309e32 · - · - · origem=Ronald (tmux)

**PROCEDE: o bloco novo morde erros reais**, confirmado executando os mutantes abaixo somente em memória. Cada linha mostra o assert e uma alteração do `.py` que o derruba:

| Assert novo | Mutante que o derruba |
|---|---|
| shell:106, sucesso de `--placar` | Provocar exceção ao acrescentar a seção nova, Python:222. |
| shell:107, `obras lidas: 4` | Mandar complemento para `lidas`, Python:103. |
| shell:108, `nao lidas: 1` | Desligar rejeição de portão, Python:98. |
| shell:109, inválida listada com motivo | Desligar rejeição de portão, Python:98; ou remover listagem, Python:230. |
| shell:111, soma e `3 de 5` | Excluir `compl` de `com`, Python:109; ou converter `?` para zero, Python:48. |
| shell:112, linha exata FB-QUATRO | Imprimir `fecha_bo_n` em vez de `fecha BO`, Python:225. |
| shell:113–114, ordem e exclusão das não medidas | Ordenar crescente, Python:224; ou converter `?` para zero, Python:48. |
| shell:117, `--conferir` verde após geração | Remover soma de `cabecalho`, Python:143: o sumário antigo permanece fora dos marcadores e entra no trecho lido como tabela, produzindo não lidas. | [nao verificado]
| shell:118, soma no cabeçalho | Remover soma de `cabecalho`, Python:143. |
| shell:120, fixture realmente mudou | É uma proteção da transformação `sed`, não uma verificação de comportamento do `.py`, shell:119–120. |
| shell:121, cabeçalho antigo fica vermelho | Desligar comparação do cabeçalho, Python:183; ou excluir complemento da soma, Python:109. |

Aqui, `shell` = `bin/tests/test_placar_grade.sh`; `Python` = `bin/placar_grade.py`.

**NÃO PROCEDE: cobertura completa das novas rejeições.** Os seguintes mutantes passam por todos os asserts novos, confirmado em memória:

- Remover `fb is False` e deixar `fecha BO` inválido entrar: Python:95–97; a fixture não possui valor inválido dessa coluna, shell:85–104.
- Remover a conferência de quantidade de células: Python:81; todas as linhas da fixture possuem a largura esperada, shell:83–104.
- Rejeitar zero trocando `fb is False` por teste de falsidade: Python:95; a fixture usa somente 4, 2, 1, 3 e `?`, shell:85–104. [nao verificado]
- Remover o sufixo ` (compl.)` da tabela: Python:225; o `awk` confere apenas `$2`, shell:113.
- Não há linha com estado inválido de complemento, portanto essa rejeição não é exercitada: shell:96–104; Python:86–92.

**NÃO PROCEDE: assert novo passa vazio porque FB-COMPL está ausente.** Ela está na fixture, shell:98; a mutação a altera, shell:119, e `cmp` acusa transformação sem efeito, shell:120. A soma e o denominador exigem também o complemento não medido de shell:99, por meio do valor exato de shell:110–111. O problema demonstrado é falta de casos para certas regras, conforme shell:83–104.

**Extra para Q1 — PROCEDE: célula semanticamente inválida pode entrar.** Uma linha de dez colunas com estado `REGISTRADAxyz` foi contada em memória como uma lida e zero não lidas: a canonização usa `startswith`, Python:86. Portões sem identificação, como `lei` e `colisao com`, também são aceitos por igualdade com o prefixo, Python:54; números sem fonte são aceitos em Python:49. Se a validade exigida for o contrato literal de fonte e identificação do aval, esses validadores são permissivos, Python:49–54.

## 14/11 10:31 · sessao 8d21a0a63336 · COERENCIA-PELO-EFEITO · censo · origem=Code

O número sai de **`horas_atraso + horas_saida_antecipada`**, em horas; o registro 10 grava `round(horas × 100)` quando a rubrica não é de dias. O gerador HTTP recalcula esses campos em memória antes de montar o TXT; a montagem sem injeção lê o fechamento gravado. (`app/folha/export.py:399`, `app/folha/export.py:451`, `app/folha/export.py:42`, `app/folha/views.py:208`, `app/folha/views.py:268`, `app/folha/export.py:717`)

Não há número de produção, batidas ou cadastro efetivo no dado fornecido que permitam reconstruir uma linha concreta: **valor, código e unidade cadastrados em produção [nao verificado]**. A resolução efetiva depende das rubricas ativas da empresa/global. (`app/folha/export.py:988`)

Na tabela, caminhos sem prefixo são relativos a `app/`; `bin/` é relativo à raiz. Contam-se as **89 linhas D**; linhas E documentam escritores e transportes. “SEM JUIZ” indica ausência de registro específico da pergunta, sem estender automaticamente a autoridade de outro predicado; os registros consultados estão em `core/juizes.py:182`, `core/juizes.py:727`, `core/juizes.py:1192` e `core/juizes.py:1310`. “SEM CASO” significa caso pela regra não identificado nesta leitura; inexistência absoluta de outro caso **[nao verificado]**.

| passo | decisao | arquivo::funcao:linha | grava em | relido por | juiz | caso pela regra |
|---|---|---|---|---|---|---|
| E1 | Campos mensais: decimais de duas casas, padrão zero. | `ponto/models.py::FechamentoMensal:425` | `horas_atraso`, `horas_saida_antecipada` | `folha/export.py::eventos_do_fechamento:451` | — | — |
| E2 | Escritor calculado: atribui os dois campos; leitura entrega objeto; escrita usa `save` no aberto e `update` nos demais. | `ponto/services/fechamento.py::recalcular_fechamento_mes:1001`, `:1128`, `:1144`, `:1160`, `:1171` | Objeto em memória ou `FechamentoMensal` | Gerador: `folha/views.py:208`; montagem gravada: `folha/export.py:719` | — | — |
| E3 | Outro escritor mensal: restauração aceita ambos os campos por `**_upd`. Chamadores passam fotografia anterior. | `ponto/services/fechamento.py::restaurar_fechamento:1380`, `:1393`; `ponto/management/commands/aplicar_09_corte_b.py:265`; `ponto/management/commands/desvio_o68b.py:196` | `FechamentoMensal` | Montagem gravada: `folha/export.py:719` | — | Conteúdo restaurado e execução [nao verificado] |
| E4 | Escritores diretos fora do serviço: com `DRY=False`, restauram os campos por `setattr` e `save`. | `bin/restore_relavratura_10_2026.py:66`, `:79`, `:81`; `bin/restore_o263_comp10.py:37`, `:48`, `:50`; campos em `ponto/management/commands/aplicar_09_corte_b.py:70` | `FechamentoMensal`; também recriam `DiaPago` em `bin/restore_relavratura_10_2026.py:86` e `bin/restore_o263_comp10.py:56` | Leitores do gravado | — | Execução e fotografias externas [nao verificado] |
| E5 | Escritores dos minutos dos períodos: inicialização, julgamento do par, zeramentos, teto/rateio, pontas internas e Zona 5. | `ponto/motor_calculo_v2.py::PeriodoCalculo:226`; `MotorBase._julgar_pontualidade:2219`, `:2226`; `MotorBase._aplicar_teto_pontualidade:1095`, `:1158`, `:1194`; `MotorBase.calcular_mes:2710`, `:2714`; `MotorTurnoPartido.calcular_mes:3079` | `minutos_atraso`, `minutos_saida_antecipada` | `ResultadoMes.horas_atraso:347`, `.horas_saida_antecipada:351` | — | Decisões discriminadas abaixo |
| E6 | Escritores diários: lavra agrega períodos e crédito; rubricas recebidas podem substituir campos; retrato cria zeros quando falta linha. | `ponto/services/dia_pago.py::lavrar:185`, `:190`, `:329`, `:360`; `::retratar_exportada:469` | `DiaPago`; versões chamadas em `ponto/services/fechamento.py:1201` e `:1277` | Espelho: `ponto/services/dia_pago.py::rubricas_do_dia:910`; calendário: `core/juizes.py:870` | — | Decisões discriminadas abaixo |
| E7 | Restauração diária adicional existe, mas seu ramo de escrita está bloqueado pelo `DRY=True` fixo desta cópia. | `bin/restore_badge_linha_09_10.py:23`, `:27`, `:56`, `:63` | `DiaPago`, se o ramo fosse habilitado | Leitores da lavratura | — | Execução e dados externos [nao verificado] |
| E8 | A 8069 usa minutos de relógio divididos por 60; não aplica fator de hora noturna. Os parâmetros noturnos/CCT são injetados separadamente. | `ponto/motor_calculo_v2.py::ResultadoMes.horas_atraso:348`, `.horas_saida_antecipada:360`; `core/regua_cct.py::get_motor_cct:466`; `folha/export.py::eventos_do_fechamento:447`, `:451` | Parcelas distintas | TXT | — | Não é parcela adicional da 8069 |
| D01 | Se `tipo == 'dias'`, arredonda inteiro; nos demais tipos, arredonda `valor × 100`. Entrada: valor em horas e `rub.tipo`. | `folha/export.py::montar_linha_10:39` | Campo numérico de nove posições do reg. 10 | `folha/export.py::partes_da_linha:251`; `folha/pdf_export.py::_fmt:36` | SEM JUIZ | SEM CASO |
| D02 | Soma `float(atraso or 0) + float(antecipada or 0)` para `atraso_diurno`. | `folha/export.py::rubrica_atraso_parcial:399`; `::eventos_do_fechamento:451` | Par evento/valor | `folha/export.py::montar_linhas_export:730`; `relatorios/cartao_pela_celula.py:270` | `_TT5`, registro `core/juizes.py:732` | SEM CASO; `folha/tests/test_export_8069_antecipada.py::Export8069Antecipada::test_evento_8069_soma_atraso_e_antecipada:22` só confere a soma |
| D03 | Flag `isento_ponto` verdadeira elimina todos os eventos. Entrada: cadastro do colaborador. | `core/constants.py::isento_ponto:18`; `folha/export.py::eventos_do_fechamento:418` | Lista vazia | Montagem e homologação: `folha/export.py:730`, `:1002` | SEM JUIZ específico da isenção | SEM CASO |
| D04 | Somente rubricas ativas; a específica da empresa sobrescreve a global do mesmo evento. | `folha/export.py::_rubricas_map:988` | Mapa evento → rubrica | Montagem: `folha/export.py:713`; classificação: `:790` | SEM JUIZ | SEM CASO |
| D05 | Valor falsy ou rubrica inexistente não produz linha. | `folha/export.py::montar_linhas_export:733` | Inclusão em `_buf` | TXT: `folha/export.py:745` | SEM JUIZ específico desse descarte | SEM CASO |
| D06 | Só monta para fechamento com status em `('aberto','aprovado')` e colaborador classificado como `entra`. | `folha/export.py::STATUS_NO_TXT:687`; `::montar_linhas_export:716`, `:717`, `:726` | Universo de linhas | TXT | SEM JUIZ específico do filtro de status | SEM CASO |
| D07 | Demissão não nula e `<= fim_janela` exclui do TXT mensal. | `folha/export.py::sai_pela_rescisao:60`; `::classificar_export:866` | `status='fora'`, motivo rescisão | Montagem: `folha/export.py:716` | `_K2`, `core/juizes.py:320` | SEM CASO |
| D08 | Código Domínio vazio após `strip` exclui. | `folha/export.py::classificar_export:840`, `:872` | `fora/sem_codigo_dominio` | Montagem: `folha/export.py:716` | `_K2`, `core/juizes.py:320` | SEM CASO |
| D09 | Batida apurável cuja data local está dentro de férias da janela retém o colaborador. | `folha/export.py::dias_ferias_com_batida:491`, `:499`, `:501`; `::classificar_export:876` | `fora/ferias_com_batida` | Montagem: `folha/export.py:716` | `_K2`, `core/juizes.py:320` | SEM CASO |
| D10 | Trabalhadas zero, pelo menos duas batidas apuráveis e nenhum dia com previsto positivo resultam em `cadastro_zero`. | `folha/export.py::classificar_export:902`, `:924`, `:942`, `:946` | Retenção | Montagem: `folha/export.py:716` | `_K2`, `core/juizes.py:320` | SEM CASO |
| D11 | Isento ou desligado devolve nenhuma retenção por célula. | `folha/export.py::motivos_retencao_celula:1207` | Motivos vazios | `classificar_export:968` | `_K2`, `core/juizes.py:320`; desligado registrado em `:267` | SEM CASO |
| D12 | Recorta por admissão/demissão; célula ausente passada ou célula passada sem veredito e com `trabalha` definido retém, com exceção do dia da demissão. | `folha/export.py::_dias_sem_decisao:1239`, `:1270`, `:1295` | `sem_celula` | `classificar_export:968` | `_K2`, `core/juizes.py:320` | SEM CASO |
| D13 | Dia sem célula e sem vínculo que o cubra é ignorado pela retenção. | `folha/export.py::_dias_sem_decisao:1254`, `:1267` | Exclui dia auditado | `classificar_export:968` | `_E2`, `core/juizes.py:1310`; `_K2`, `:320` | SEM CASO |
| D14 | Veredito acusador retém, salvo conjunto de protestos não vazio inteiramente contido em `NAO_COBRAVEIS_DO_COLABORADOR`, ou exceção da pausa. | `folha/export.py::_dias_sem_decisao:1273`, `:1285`, `:1293` | Motivo `espelho_<veredito>` | `classificar_export:968` | `_K2`, `core/juizes.py:320` | SEM CASO |
| D15 | Exige protestos exatamente `{'FURO_PARCIAL'}`, resposta sem desalinhamento/abertura/incerteza e pelo menos uma lâmpada apagada, todas de pausa. | `folha/export.py::nao_retem_pela_pausa:1173`; `::_furo_so_da_pausa:1151` | Dispensa retenção | `_dias_sem_decisao:1293` | `_K2`, `core/juizes.py:320`; juiz da ata em `:1193` | `folha/tests/test_furo_so_intervalo_nao_retem.py::FuroSoIntervaloNaoRetemTest::test_so_o_intervalo_faltando_NAO_retem:83`: motivos `[]`; L-033 em `docs/LEIS.md:81` |
| D16 | Catálogo homologado vazio desliga o gate; caso contrário, código de evento não zero ou de falta por datas, presente no mapa e fora do catálogo, retém. | `folha/export.py::_codigos_nao_homologados:999`, `:1005`, `:1011`, `:1013`; `::classificar_export:974` | `fora/nao_homologado` | Montagem: `folha/export.py:716` | `_K2`, `core/juizes.py:320` | SEM CASO |
| D17 | O gerador exige existir algum fechamento gravado aberto/aprovado antes do cálculo em memória. | `folha/views.py::gerar_dominio_txt:148`, `:163` | Libera ou recusa a requisição | Cálculo: `folha/views.py:208` | SEM JUIZ específico dessa guarda | SEM CASO |
| D18 | Gerador HTTP escolhe recalcular em memória; montagem sem `fechamentos=` escolhe consultar o gravado. | `folha/views.py::gerar_dominio_txt:208`, `:268`; `folha/export.py::montar_linhas_export:717` | Fonte do número exportado | TXT | SEM JUIZ específico da escolha de fonte | SEM CASO |
| D19 | Gate exige snapshot da empresa com `data_ref` igual a hoje; usa o mais recente por id. | `folha/porta_export.py::conferir:807`, `:811` | Libera/recusa TXT | `folha/views.py:129` | SEM JUIZ | SEM CASO |
| D20 | Mês e ano do detalhe do carimbo devem coincidir com os pedidos. | `folha/porta_export.py::conferir:815` | Libera/recusa TXT | `folha/views.py:130` | SEM JUIZ | SEM CASO |
| D21 | Medida ausente, negativa ou positiva barra; zero libera. | `folha/porta_export.py::conferir:818`, `:821`, `:867` | Libera/recusa TXT | `folha/views.py:130` | SEM JUIZ | SEM CASO |
| D22 | Falhas somam divergências de tela/PDF, topo, builder, calendário, rubricas, cartão/TXT e HE condicionada pela flag; comparações de `DiaPago`/badge ficam publicadas fora dessa soma. | `folha/porta_export.py::medir:687`, `:697`, `:711`, `:764` | Valor do carimbo | `folha/porta_export.py::conferir:818` | SEM JUIZ específico da composição | SEM CASO |
| D23 | Universo do cálculo inclui situação diferente de desligado, ou desligado com demissão desde o início da janela; empresa restringe quando informada. | `ponto/services/fechamento.py::universo_da_competencia:431`, `:439` | Colaboradores processados | `recalcular_fechamento_mes:554` | SEM JUIZ específico | SEM CASO |
| D24 | Corte vem da empresa, padrão 21, limitado a `[2,28]`; a janela usa esse corte. | `ponto/janelas.py::corte_da_empresa:50`; `::janela_fechamento:57` | Limites da competência | `ponto/services/fechamento.py:621`; `folha/export.py:51` | SEM JUIZ específico de `janela_fechamento`; `_F5` registra `janela_atual` em `core/juizes.py:304` | `ponto/tests/test_periodo_apuracao.py::PeriodoApuracaoFuncTest::test_corte_26:34`: 26/05–25/06; L-026 em `docs/LEIS.md:77`. Clamp sem caso identificado |
| D25 | Cada fatia usa vigência e dias atribuídos ao vínculo; fatia sem dias próprios é pulada; resultados são concatenados. | `ponto/services/fechamento.py::apurar_por_fatia:206`, `:209`, `:229`, `:235`, `:294` | `ResultadoMes` composto | `recalcular_fechamento_mes:755` | SEM JUIZ específico da composição das fatias | `ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F5_troca_de_vinculo_no_meio_da_competencia:1867`: 16h trabalhadas, 1h atraso, 2h antecipada em `:1944`; regra em `:1883` |
| D26 | Batidas da fatia são filtradas pelo dia local, com margem de um dia; dentro da fatia, excluem-se dias atribuídos a outro vínculo. | `ponto/services/fechamento.py::apurar_por_fatia:204`, `:244`, `:253`, `:260` | Batidas entregues ao motor | `motor.calcular_mes`, `ponto/services/fechamento.py:286` | SEM JUIZ específico desse recorte | SEM CASO |
| D27 | Guarda cadastrada para competência exportada exclui colaboradores da escrita; leitura pura não consulta empresas exportadas para essa guarda. | `ponto/services/fechamento.py::GUARDA_COMPETENCIA:36`; `::recalcular_fechamento_mes:539`, `:560` | Permissão de regravar mensal/diário | Escritor mensal: `:1144`; diário: `:1191` | SEM JUIZ específico da guarda | SEM CASO |
| D28 | Para folha, só ausência aprovada entra; para cobrança, exclui apenas rejeitada. | `ponto/turnos.py::_pelo_efeito:2516` | Universo de ausências | `ponto/services/fechamento.py:639`, `:683`; `credito_parcial.py:26` | Pergunta “ausência cobre para folha”, `core/juizes.py:190` | SEM CASO |
| D29 | Cobertura expande dia/range, abre até o fim apenas para tipos em aberto e sobrescreve por ordem crescente `(data,pk)`: vence o mais recente. | `ponto/turnos.py::cobertura_ausencia_periodo:2753`; `::_cobertura_pura:2772`, `:2781` | Mapa dia → ausência | `ponto/services/fechamento.py:683` | `_A2`, `core/juizes.py:253` | SEM CASO |
| D30 | Efeito vem do catálogo vivo, cache de 60s; tabela vazia/indisponível usa catálogo interno; tipo desconhecido retorna `None`. | `ponto/catalogo/ausencias.py::vigente:275`, `:278`; `::efeito_vigente:294` | Efeito abona/desconta/suprime | `ponto/services/fechamento.py:697`; `ponto/turnos.py:2692` | `_A1`, `core/juizes.py:252` | SEM CASO |
| D31 | Parcial exige tipo que aceita horas, um único dia e `0 < minutos < 480`. | `ponto/turnos.py::cobre_parte_do_dia:2600`; constante `:2582` | Classificação parcial/integral | `credito_parcial.py:27`; `ponto/turnos.py:2677` | SEM JUIZ específico | SEM CASO pela fronteira normativa de 480 |
| D32 | Abono integral exige efeito `ABONA`, tipo diferente de falta e ausência não parcial. | `ponto/turnos.py::ausencia_explica_o_dia_inteiro:2677`; `::dias_com_ausencia_de_dia_inteiro:2691` | `dias_com_ausencia_inteira` do motor | `ponto/services/fechamento.py:687`, `:751`; motor `:1092` | SEM JUIZ específico da composição | SEM CASO substantivo CLT/CCT/L-NNN identificado |
| D33 | Justificativa aprovada com `data_turno` na janela entra em justificadas; ausência parcial paga não abona o dia; `ABONA` justifica, efeito diferente de `DESCONTA` suprime. | `ponto/services/fechamento.py::recalcular_fechamento_mes:660`, `:694`, `:698` | `datas_justificadas`, `_datas_suprimidas` | Motor montado em `ponto/services/fechamento.py:736` | SEM JUIZ específico dessa combinação | SEM CASO |
| D34 | Crédito inclui ausência paga, parcial e com data inicial dentro da janela; soma seus minutos nessa data. | `ponto/services/credito_parcial.py::creditos_por_dia:26` | Mapa crédito por dia | `ponto/services/fechamento.py:999`; `dia_pago.py:191` | SEM JUIZ | SEM CASO CLT/CCT/L-NNN identificado |
| D35 | Para abater crédito, dia do atraso é a data local da entrada do período, não o mapa `dia_da_jornada`. | `ponto/services/credito_parcial.py::dia_do_periodo:34`; `::atraso_por_dia:42` | Mapa atraso por dia | `ponto/services/fechamento.py:998` | SEM JUIZ específico dessa atribuição | SEM CASO |
| D36 | Usa `min(atraso[d],credito[d])`; crédito não atravessa data nem vira saldo de atraso. | `ponto/services/credito_parcial.py::abater_no_proprio_dia:57` | Minutos utilizados | `ponto/services/fechamento.py:1000` | SEM JUIZ | SEM CASO CLT/CCT/L-NNN identificado |
| D37 | Mensal arredonda atraso após abater crédito e limitar a zero; antecipada só é arredondada, sem esse abatimento. | `ponto/services/fechamento.py::recalcular_fechamento_mes:1001` | Dois campos mensais | `folha/export.py:451` | SEM JUIZ específico | SEM CASO |
| D38 | Totais somam somente `periodos`, dividindo minutos por 60; `periodos_ft` não participa. | `ponto/motor_calculo_v2.py::ResultadoMes.horas_atraso:348`; `.horas_saida_antecipada:360` | Totais do resultado | `ponto/services/fechamento.py:1001` | SEM JUIZ específico dessas properties | SEM CASO direto dessas properties |
| D39 | Dia presente no mapa de ausência inteira zera atraso e antecipada de todos os períodos agrupados. | `ponto/motor_calculo_v2.py::MotorBase._aplicar_teto_pontualidade:1092` | Minutos dos períodos | Properties `:348`, `:360` | SEM JUIZ específico desse zeramento | SEM CASO substantivo identificado |
| D40 | Qualquer período com `turno_aberto=True` desarma a pontualidade do dia. | `ponto/motor_calculo_v2.py::MotorBase._aplicar_teto_pontualidade:1131`, `:1156` | Ambos os minutos viram zero | Properties `:348`, `:360` | SEM JUIZ específico; não consulta `dia_encerrado` registrado em `core/juizes.py:235` | `ponto/tests/test_pontualidade_em_turno_aberto.py::PontualidadeEmTurnoAbertoTest::test_RED_col890_o_dia_com_turno_aberto_NAO_cobra_saida_antecipada:78`: antecipada zero; L-073 em `docs/LEIS.md:100` |
| D41 | Havendo par com duas pontas e duração `<= 0` ou `<= TETO_RETRATACAO_SEG`, zera pontualidade do dia. | `ponto/motor_calculo_v2.py::MotorBase._aplicar_teto_pontualidade:1146`, `:1153` | Ambos os minutos viram zero | Properties `:348`, `:360` | SEM JUIZ específico | SEM CASO pela regra do limiar de 360s; par nulo tem caso em `ponto/tests/test_pontualidade_em_turno_aberto.py:92` |
| D42 | Previsto `None` mantém a pontualidade sem aplicar teto. | `ponto/motor_calculo_v2.py::MotorBase._aplicar_teto_pontualidade:1170` | Conserva minutos | Properties `:348`, `:360` | SEM JUIZ específico dessa degradação | SEM CASO identificado para esse ramo |
| D43 | Se atraso+antecipada excedem `max(0,previsto−trabalhados−fora_janela)`, reduz a soma ao teto. | `ponto/motor_calculo_v2.py::MotorBase._aplicar_teto_pontualidade:1173`, `:1186` | Minutos dos períodos | Properties `:348`, `:360` | SEM JUIZ específico do teto | `ponto/tests/test_l093_trabalhado_real.py::TetoUsaOTrabalhadoRealTest::test_RED_col890_a_antecipada_cai_de_10_66_para_8_23:52`: 493,8min; L-093 em `docs/LEIS.md:111` |
| D44 | Redução mantém proporção: fator `teto/soma`, multiplicação de cada parcela e arredondamento de duas casas. | `ponto/motor_calculo_v2.py::MotorBase._aplicar_teto_pontualidade:1191` | Atraso/antecipada rateados | Properties `:348`, `:360` | SEM JUIZ específico do rateio | `ponto/tests/test_l093_trabalhado_real.py::TetoUsaOTrabalhadoRealTest::test_MORDE_o_rateio_entre_as_duas_rubricas_se_mantem:92`: soma 280min, razão 2:1; L-093 em `docs/LEIS.md:111` |
| D45 | Com pelo menos dois períodos, `hi/hf` e sem os quatro marcos, somente o primeiro conserva atraso e o último conserva antecipada. | `ponto/motor_calculo_v2.py::MotorBase.calcular_mes:2698`, `:2704`, `:2709` | Zera pontas internas | Teto em `ponto/motor_calculo_v2.py:2718` | SEM JUIZ específico | SEM CASO CLT/CCT/L-NNN identificado |
| D46 | Zona 5 só reavalia com fonte de marcos chamável, pelo menos dois períodos no dia e os quatro marcos preenchidos. | `ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes:3024`, `:3031` | Habilita reescrita | `ponto/motor_calculo_v2.py:3077` | SEM JUIZ específico da habilitação | SEM CASO |
| D47 | P1/P2 vêm de `_match_marcos`; exige slot não faltante, tipo real E e período de mesma entrada HH:MM, sem reutilização. | `ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes:3062`, `:3066`, `:3069` | Reescreve ambas as parcelas em `:3079` | Teto posterior do motor | `_T4`, `core/juizes.py:212`, para alinhamento; associação ao período é adicional | `ponto/tests/test_turno_partido_zona5_marco_pela_grade.py::Zona5MarcoPelaGradeTest::test_MORDE_entrada_4h10_atrasada_e_atraso:30`: 250min; referência expressa em L-084, `docs/LEIS.md:116` |
| D48 | Sem escala ou sem marco inicial utilizável, retorna sem julgar pontualidade; lê EC, depois template e finalmente campos crus. | `pont

[truncado em 20000 de 37234 caracteres]

## 14/11 10:31 · sessao 11164a2e6f4e · - · - · origem=Ronald (tmux)

| Item | Dono fechado | Destino registrado |
|---|---|---|
| 5 · col247 · pk34577 | ESTRUTURA — app/docs/RELATO.md:3315 | JUIZ-DO-PAR-DUAS-ENTRADAS — app/docs/BACKLOG.md:776; encaminhamento posterior substitui PERGUNTA-DO-DIA — app/docs/CORTES.md:313 |
| 6 · col42/col129/col789 | ESTRUTURA — app/docs/RELATO.md:3316 | FIO-LE-O-JUIZ — app/docs/BACKLOG.md:794; app/docs/GRADE-MESTRA.md:35. Outra classe do mesmo item: Q-VIVAS-LEITORES-SEM-JUIZ — app/docs/BACKLOG.md:1521; encaminhamento em app/docs/RELATO.md:3316 |
| 7 · col130 · pk28883 | ESTRUTURA — app/docs/RELATO.md:3317 | REGISTRADO-NAO-E-VIVO — app/docs/BACKLOG.md:780; app/docs/GRADE-MESTRA.md:134 |
| 8 · col789 · pk1473 | CADASTRO — app/docs/RELATO.md:3318 | lista do admin — app/docs/RELATO.md:3308 |
| 8.1 · col789 · pk1472/pk1473 | ESTRUTURA — app/docs/RELATO.md:3318 | TRILHA-DO-VINCULO-MENTE — app/docs/BACKLOG.md:778; app/docs/GRADE-MESTRA.md:133 |
| 8.2 · col789 | dono não fechado — app/docs/RELATO.md:3318 | TENTATIVA-DO-ADMIN-VIRA-COBRANCA — app/docs/BACKLOG.md:779; app/docs/GRADE-MESTRA.md:467 |
| 8.3 · col789 · pk1473 | dono individual não fechado; incluído na letra F entre encaminhamentos estruturais — app/docs/CORTES.md:313 | complemento E4-REGENERAR-VINCULO-SEM-VIGENCIA — app/docs/RELATO.md:3318; obra-mãe CASA-ESCALA-E4 — app/docs/BACKLOG.md:1683; app/docs/GRADE-MESTRA.md:657 |

O universo original é **8 itens**, explicitamente contado em app/docs/BACKLOG.md:777 e enumerado em app/docs/CORTES.md:308. Os três 8.x são complementos do item 8, conforme app/docs/CORTES.md:310; somá-los exige declarar universo ampliado de **11 unidades**, mantendo os oito itens originais.

Cuidado com col247: o mesmo dia aparece posteriormente como BATIDA em app/docs/RELATO.md:8361 e HJ-031-U1 indeterminado em app/docs/RELATO.md:8259. Esses registros classificam a irregularidade do dia; a substituição explícita do dono do defeito estrutural do BO item 5 está [não verificado].

## 14/11 10:31 · sessao f4db4c359ac6 · - · - · origem=Ronald (tmux)

| Item | Dono fechado | Destino registrado |
|---|---|---|
| 2 — col789 | ESTRUTURA — `app/docs/CORTES.md:312`; a classificação anterior por L-084 foi substituída em `app/docs/CORTES.md:315`. | JUIZ-DO-PAR-DUAS-ENTRADAS — `app/docs/CORTES.md:315`, `app/docs/BACKLOG.md:776`, `app/docs/GRADE-MESTRA.md:33`. |
| 3 — col789, pk2916 | ESTRUTURA — `app/docs/RELATO.md:3313`. | JUSTIFICATIVA-PAGA-O-PERIODO — separada da COBERTURA por `app/docs/CORTES.md:330`; caso registrado em `app/docs/BACKLOG.md:1875`; grade em `app/docs/GRADE-MESTRA.md:264`. |
| 3 — col129 | ESTRUTURA para o caso de realizado/DiaPago zerados — `app/docs/CORTES.md:354`. | JUIZ-DO-PAR-DUAS-ENTRADAS — `app/docs/CORTES.md:354`, `app/docs/BACKLOG.md:776`, `app/docs/GRADE-MESTRA.md:33`. |
| 4 — col789, pk22430 | CADASTRO — `app/docs/CORTES.md:312`, confirmado em `app/docs/CORTES.md:315`. | Lista do admin — `app/docs/CORTES.md:312`, `app/docs/RELATO.md:3035`. |

A causa dos 19 casos sem justificativa permanece não medida no resultado original do item 3 — `app/docs/RELATO.md:3313`.

O complemento de pk22430 preso a vínculo encerrado está registrado separadamente na TRILHA-DO-VINCULO-MENTE; não substitui a classificação CADASTRO do defeito por mês do item 4 — `app/docs/BACKLOG.md:778`, `app/docs/CORTES.md:315`.

## 14/11 10:31 · sessao 0b41ff7b9e70 · - · - · origem=Ronald (tmux)

| id | dono fechado | destino registrado |
|---|---|---|
| HJ-005-U1 · col42 | ESTRUTURA — app/docs/RELATO.md:8255 | COBERTURA-PARCIAL-UM-JUIZ — app/docs/BACKLOG.md:786; app/docs/GRADE-MESTRA.md:34. Correspondência individual U1↔pk [não verificado]. |
| HJ-005-U2 · col42 | ESTRUTURA — app/docs/RELATO.md:8255 | COBERTURA-PARCIAL-UM-JUIZ — app/docs/BACKLOG.md:786; app/docs/GRADE-MESTRA.md:34. Correspondência individual U2↔pk [não verificado]. |
| HJ-005-U4 · col42 | ESTRUTURA — app/docs/RELATO.md:8255 | COBERTURA-PARCIAL-UM-JUIZ — app/docs/BACKLOG.md:786; app/docs/GRADE-MESTRA.md:34. Correspondência individual U4↔pk [não verificado]. |
| BO-ADMIN-2710.1 · col42 · pk4803/pk4867/pk5101 | ESTRUTURA — app/docs/RELATO.md:3311 | COBERTURA-PARCIAL-UM-JUIZ — app/docs/BACKLOG.md:786; app/docs/GRADE-MESTRA.md:34 |
| BO-ADMIN-2710.3 · col789 · pk2916 | ESTRUTURA — app/docs/RELATO.md:3313 | JUSTIFICATIVA-PAGA-O-PERIODO — app/docs/BACKLOG.md:1875; app/docs/GRADE-MESTRA.md:264; separação expressa em app/docs/CORTES.md:330 |

HJ-005-U1/U2/U4 aparecem literalmente em app/docs/RELATO.md:8255; o BACKLOG identifica coletivamente os três casos por col42 e pk4803/pk4867/pk5101 em app/docs/BACKLOG.md:786. A associação individual de cada HJ ao respectivo pk não está demonstrada nessas fontes. [não verificado]

O registro distingue o defeito ESTRUTURA do dado CADASTRO: parcial sem período declarado vai para lista do admin, conforme app/docs/CORTES.md:319; essa regra está implementada em app/ponto/supra_juiz.py:395 e app/ponto/turnos.py:2634. Isso não autoriza substituir retrospectivamente o dono ESTRUTURA medido para HJ-005-U1/U2/U4 em app/docs/RELATO.md:8255.

BO-ADMIN-2710.3 contém ainda 19 casos sem justificativa cuja causa não foi medida; dono individual não fechado — app/docs/RELATO.md:3313. col129 pertence também ao caso RT-05 da FIO-LE-O-JUIZ, conforme app/docs/CORTES.md:321; atribuir todos os casos col129 à JUSTIFICATIVA-PAGA-O-PERIODO seria [não verificado].

## 14/11 10:31 · sessao de523b4ffb36 · - · - · origem=Ronald (tmux)

**PROCEDE — portão de DECLARADO-X-GRAVADO incorreto.** A GRADE registra `colisao com BUG-PAR/E3`, mas o RELATO mantém a obra fora da ordem por cinco perguntas de lei abertas. O portão exigido pelo aval anexo é `lei`, não apenas colisão. Evidências: app/docs/GRADE-MESTRA.md:270; app/docs/RELATO.md:7; app/docs/RELATO.md:23; app/docs/RELATO.md:24.

**PROCEDE — E3 recebe uma autocolisão sem significado operacional.** O portão diz expressamente “a propria obra em voo”; RAIAS exige arquivo comum com outra raia para impedir paralelismo. Evidências: app/docs/GRADE-MESTRA.md:117; CLAUDE.md:309; CLAUDE.md:310. Qual portão deveria substituir a autocolisão: [nao verificado].

**PROCEDE — portão de O232 carece de prova de obra em voo.** A linha aponta `colisao com JUSTIFICATIVA`, enquanto JUSTIFICATIVA aparece INTERROMPIDA e na sequência posterior à E3. Evidências: app/docs/GRADE-MESTRA.md:851; app/docs/GRADE-MESTRA.md:267; app/docs/RELATO.md:7. Colisão atual com agente vivo: [nao verificado].

**NAO PROCEDE — BO-ADMIN-2710 divergir do portão pedido.** `censo` reproduz o aval anexado e combina com a pendência de medir, atribuir dono e destino por item. Evidências: app/docs/GRADE-MESTRA.md:135; app/docs/BACKLOG.md:777.

**NAO PROCEDE — RAIZ e CONTRATO terem portões fora das categorias permitidas.** Registram `colisao com E3`, nomeando respectivamente models.py e juizes.py. O histórico também identifica esses arquivos na E3. Evidências: app/docs/GRADE-MESTRA.md:106; app/docs/GRADE-MESTRA.md:685; app/docs/RELATO.md:309. Interseção do diff vivo atual: [nao verificado].

Os seis preenchimentos são exatamente:

| Linha da GRADE | Obra | fecha BO | portão |
|---|---|---:|---|
| 106 | RAIZ-DA-DISPUTA | 1 | colisao com E3 |
| 117 | FIO-E0-V4-E3-LEITORES | 2 | colisao consigo |
| 135 | BO-ADMIN-2710 | 3 | censo |
| 270 | DECLARADO-X-GRAVADO | 4 | colisao com BUG-PAR/E3 |
| 685 | CONTRATO-DE-ENTRADA-DO-FIO, complemento | 1 | colisao com E3 |
| 851 | O232, complemento | 1 | colisao com JUSTIFICATIVA |

Cada célula numérica usa a fonte genérica `roteiro BO-[nome]-1311 / mapa HJ`; os valores coincidem com o aval anexado, mas a identificação das unidades que sustentam cada número e uma medição atual da produção são [nao verificado]. Evidências: app/docs/GRADE-MESTRA.md:106; app/docs/GRADE-MESTRA.md:117; app/docs/GRADE-MESTRA.md:135; app/docs/GRADE-MESTRA.md:270; app/docs/GRADE-MESTRA.md:685; app/docs/GRADE-MESTRA.md:851.

O arquivo literal logs/AVAIS-1313/ramos_andam_junto.txt está ausente nesta cópia: localização/conteúdo local [nao verificado]; comparação do aval feita com o anexo fornecido.

## 14/11 10:31 · sessao 67189aa9f6fc · COERENCIA-PELO-EFEITO · censo · origem=Code

| passo | decisao | arquivo::funcao:linha | grava em | relido por | juiz | caso pela regra |
|---|---|---|---|---|---|---|
| Escopo | Valor efetivo desta rubrica em produção, cadastro aplicado e batidas correspondentes: **[nao verificado]**. Contagens abaixo abrangem as linhas D01–D77; W/R são inventário de escritores/leitores. | `app/folha/export.py::eventos_do_fechamento:448` | — | — | Correspondência nominal com o registro; helpers sem entrada própria recebem SEM JUIZ. | Execução e aprovação dos testes: **[nao verificado]**. |
| Registro BP/BD | BP = `JUIZES['batida']['quantas horas este PAR (turno) vale?']`; BD = `JUIZES['batida']['que periodos e que intervalo teve este dia?']`. | `app/core/juizes.py:1193`, `app/core/juizes.py:1194` | — | BP registra a porta polimórfica `MotorBase.calcular_periodo`. | BP / BD | — |
| Registro demais | K2 = entrada no TXT; TT5 = totais; E1/E2 = vínculo/cobertura; TI/TP/TM = intervalo/partido/marcos; CP/CD/CF/FL = previsto/dia de trabalho/feriado/folga lançada. | `app/core/juizes.py:320`, `:732`, `:1310`, `:214`, `:220`, `:213`, `:185`, `:184`, `:188`, `:204` | — | Nomes abreviados usados abaixo. | Entradas efetivas de `JUIZES`. | — |
| D01 · TXT | Se `tipo == 'dias'`, inteiro arredondado; nos demais tipos, `int(round(float(valor)*100))`. Portanto, com rubrica de horas, **hora decimal → centésimos de hora**, sem conversão para HH:MM. | `app/folha/export.py::montar_linha_10:39` | Campo numérico de nove posições da linha 10. | `agregar_export_dominio:316`; `pdf_export` — R05. | SEM JUIZ | SEM CASO |
| D02 · valor | `intrajornada_diurna` recebe exatamente `float(fech.horas_intra_indenizada)`, em horas decimais; não aplica multiplicador indenizatório. | `app/folha/export.py::eventos_do_fechamento:448` | Tupla evento/valor em memória. | `montar_linhas_export:730`; `_codigos_nao_homologados:1002`. | TT5 | `app/folha/tests/test_export_dsr_reflexo.py::DsrEhReflexoTest::test_intrajornada_continua_sendo_emitida:38`: Art.71 §4; campo 13 → evento 13. |
| D03 · emissão | `if not valor: continue`; `if not rub: continue`. Zero ou evento sem rubrica resolvida não produz linha. | `app/folha/export.py::montar_linhas_export:733` | Lista `linhas`. | Gerador do TXT. | SEM JUIZ | SEM CASO |
| D04 · cadastro da rubrica | Só rubricas `ativo=True`; mapa global é atualizado pelo mapa da empresa: empresa prevalece sobre global para o mesmo evento. | `app/folha/export.py::_rubricas_map:988` | Mapa evento → rubrica; fornece `codigo_fopag` e `tipo`. | `montar_linhas_export:713`; `classificar_export:790`. | SEM JUIZ | SEM CASO |
| D05 · seleção da montagem | Só monta fechamentos com status em `('aberto','aprovado')` e colaborador presente em `entra_set`. | `app/folha/export.py::montar_linhas_export:716`; constante `:687` | Lista de linhas. | TXT e PDF de exportação. | SEM JUIZ | SEM CASO |
| D06 · isenção | Se `isento_ponto(colaborador)` for verdadeiro, retorna lista vazia de eventos; a função consultada lê a flag, não o cargo. | `app/folha/export.py::eventos_do_fechamento:418`; `app/core/constants.py::isento_ponto:18` | Eventos vazios. | Montagem e homologação. | TT5 | SEM CASO |
| D07 · rescisão | Exclui quando `data_demissao is not None and data_demissao <= fim_janela`. | `app/folha/export.py::sai_pela_rescisao:60`; `::classificar_export:866` | Item `fora/rescisao_modulo_proprio`. | `montar_linhas_export:716`. | K2 | SEM CASO |
| D08 · identificação | Código do Domínio vazio após `strip()` exclui o colaborador. | `app/folha/export.py::classificar_export:840`, `:872` | Item `fora/sem_codigo_dominio`. | Montagem. | K2 | SEM CASO |
| D09 · férias | Havendo batida apurável cuja data local cai em faixa de férias intersectada com a competência, retém o colaborador. | `app/folha/export.py::dias_ferias_com_batida:491`, `:499`; `::classificar_export:877` | Item `fora/ferias_com_batida`. | Montagem. | K2 | SEM CASO |
| D10 · cadastro zero | Com trabalhadas zero, pelo menos duas batidas apuráveis e nenhum dia com previsto positivo, retém por `cadastro_zero`. | `app/folha/export.py::classificar_export:902`, `:924`, `:942`, `:946` | Item `fora/cadastro_zero`. | Montagem. | K2 | SEM CASO |
| D11 · homologação | Catálogo vazio desliga o gate; caso contrário, evento não zero com rubrica existente e código fora do catálogo retém. | `app/folha/export.py::_codigos_nao_homologados:999`, `:1005`, `:1011`; `::classificar_export:974` | Lista de códigos; item `fora/nao_homologado`. | Montagem. | SEM JUIZ específico do helper; chamada por K2 em `app/core/juizes.py:320`. | SEM CASO |
| D12 · dispensa de retenção | Isento ou `esta_desligado(colab)` recebe motivos vazios de retenção do espelho. | `app/folha/export.py::motivos_retencao_celula:1207` | Motivos em memória. | `classificar_export:968`; fechamento `app/ponto/services/fechamento.py:1068`. | SEM JUIZ | SEM CASO |
| D13 · universo da retenção | Recorta por admissão/demissão; dia sem célula e sem vínculo que o cubra é pulado. | `app/folha/export.py::_dias_sem_decisao:1239`, `:1241`, `:1255`, `:1267` | Motivos/dias retidos. | `motivos_retencao_celula:1212`. | SEM JUIZ | SEM CASO |
| D14 · célula ausente | Retém célula inexistente em dia passado, exceto dia da demissão; também retém veredito nulo com `trabalha is not None` nas mesmas condições temporais. | `app/folha/export.py::_dias_sem_decisao:1270`, `:1295` | Motivo `sem_celula`. | K2 por `motivos_retencao_espelho:1307`. | SEM JUIZ | SEM CASO |
| D15 · protestos | Veredito acusador retém salvo conjunto não vazio de protestos inteiramente contido em `NAO_COBRAVEIS_DO_COLABORADOR`, ou exceção da pausa. | `app/folha/export.py::_dias_sem_decisao:1273`, `:1286`, `:1293` | Motivo `espelho_…`. | Classificação do export. | SEM JUIZ | SEM CASO |
| D16 · pausa faltante | Exige protestos exatamente `{'FURO_PARCIAL'}`, ata sem desalinhamento/abertura/incerteza, ao menos uma lâmpada apagada e todas as apagadas pertencentes às pausas. | `app/folha/export.py::nao_retem_pela_pausa:1173`; `::_furo_so_da_pausa:1151` | Dispensa da retenção; não muda a intra gravada. | `_dias_sem_decisao:1293`; vigia da pausa. | SEM JUIZ | `app/folha/tests/test_furo_so_intervalo_nao_retem.py::FuroSoIntervaloNaoRetemTest::test_so_o_intervalo_faltando_NAO_retem:83`: L-033; entrada/saída presentes e pausa ausente → motivos `[]`. |
| D17 · portão do cartão | Compara intra do resumo com intra do fechamento **gravado**; diferença maior que `0.02h` constitui divergência. | `app/relatorios/cartao_pela_celula.py::divergencia:249`, `:269` | Divergências; contador cartão × TXT. | `app/folha/porta_export.py::medir:667`. | SEM JUIZ | SEM CASO |
| D18 · reinterpretação da porta | Agrega períodos normais pela data da entrada; com HE e intra positivas calcula `max(0,trab-prev-min(intra,intervalo_declarado))`; acusa HE acima desse teto mais um minuto. | `app/folha/porta_export.py::medir:535`, `:546`, `:561` | `dif_rubrica`. | Total de falhas em `:687`; `conferir`. | SEM JUIZ | SEM CASO |
| D19 · gate final | Exige snapshot de hoje, da mesma competência, com valor não negativo; valor positivo recusa o TXT. | `app/folha/porta_export.py::conferir:807`, `:815`, `:818`, `:821`, `:865`; `app/folha/views.py:129` | Resposta de recusa/liberação. | Gerador HTTP. | SEM JUIZ | SEM CASO |
| D20 · arredondamento mensal | Grava `round(resultado.horas_intrajornada_indenizada,2)`. | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:1004` | `FechamentoMensal.horas_intra_indenizada`, decimal de duas casas em `app/ponto/models.py:428`. | Evento D02; CSV; cartão. | SEM JUIZ | SEM CASO |
| D21 · persistência | `somente_leitura` devolve objeto/dicionário; fora desse modo, status aberto usa `save()`, demais estados usam `update()`, incluindo intra. | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:1128`, `:1137`, `:1144`, `:1160`, `:1174` | Mensal, ou objeto destacado. | Export gravado; `fechamento_lido` em R03. | SEM JUIZ | SEM CASO |
| D22 · guarda do model | Objeto existente cujo status original e atual sejam ambos `aprovado` levanta erro no `save`; o escritor mensal acima usa `update` nesse ramo. | `app/ponto/models.py::FechamentoMensal.save:496`; `app/ponto/services/fechamento.py:1160` | Permite/impede persistência por `save`. | Chamadores do model. | SEM JUIZ | SEM CASO |
| D23 · guarda histórica | Com competência em `GUARDA_COMPETENCIA` e empresas exportadas no escopo, exclui seus colaboradores do recálculo que escreve. A configuração contém `(9,2026)`. | `app/ponto/services/fechamento.py::recalcular_fechamento_mes:539`, `:560`; configuração `:36` | Universo do escritor. | Laço em `:619`. | SEM JUIZ | SEM CASO |
| D24 · universo mensal | Inclui quem não está `desligado`, ou desligado com demissão a partir do início da janela da respectiva empresa; aplica escopo de empresa e, no chamador, IDs explícitos. | `app/ponto/services/fechamento.py::universo_da_competencia:431`, `:439`; `::recalcular_fechamento_mes:556` | Query de colaboradores. | Apuração mensal. | SEM JUIZ | SEM CASO |
| D25 · competência | Corte da empresa, com default 21 e clamp entre 2 e 28; início no mês anterior e fim em corte menos um no mês da competência. | `app/ponto/janelas.py::corte_da_empresa:50`; `::periodo_apuracao:15`; `::janela_fechamento:57` | Limites da apuração. | Fechamento `:621`; export `app/folha/export.py:50`. | SEM JUIZ nominal de `janela_fechamento`; registro aponta `janela_atual` em `app/core/juizes.py:335`. | SEM CASO |
| D26 · parcelas mensais | Soma minutos de intra de **períodos normais e folgas trabalhadas**, dividindo por 60. | `app/ponto/motor_calculo_v2.py::ResultadoMes.horas_intrajornada_indenizada:370` | Horas em memória. | Escritor mensal D20; lavratura diária. | SEM JUIZ | `app/ponto/tests/test_dia_pago_soma.py::LavraDaFolgaTrabalhadaTest::test_MORDE_ft_certa_leva_hora_NOTURNO_e_INTRA_e_nao_leva_antecipada_nem_HE:220`: regra escrita em `:181`; 60 minutos de intra da folga → 1h lavrada. |
| D27 · restauração | Aceita campos existentes, exclui identificadores, exige motivo e grava os valores recebidos sem derivá-los; intra pode integrar o dicionário. | `app/ponto/services/fechamento.py::restaurar_fechamento:1374`, `:1378`, `:1380`, `:1392` | Campo mensal por `update`. | Export gravado. | SEM JUIZ | SEM CASO |
| D28 · limites da fatia | Início = máximo de janela, início do vínculo e início operacional; fim = mínimo de janela e fim do vínculo. Fatia invertida é pulada. | `app/ponto/services/fechamento.py::apurar_por_fatia:206`, `:207`, `:242` | Universo de períodos da fatia. | `motor.calcular_mes:286`; composição `:294`. | SEM JUIZ | SEM CASO |
| D29 · cobertura do vínculo | Exige início declarado e não posterior ao dia, fim ausente ou não anterior, e vínculo ativo ou com fim declarado. | `app/escala/alimentacao.py::vinculo_cobre_a_data:214`; `::tem_vigencia_declarada:196` | Elegibilidade do vínculo. | Fatias; previsto; retenção. | E2 — `app/core/juizes.py:1310` | SEM CASO |
| D30 · vínculo do dia | Geradora da célula só responde se encontrada e dentro das datas; sem geradora, escolhe maior `(ativa,data_inicio)` entre vínculos que cobrem o dia. | `app/escala/alimentacao.py::vinculo_do_dia:279`, `:282`, `:285` | Dono do dia. | Fatias `app/ponto/services/fechamento.py:191`; previsto. | E1 — `app/core/juizes.py:1310` | SEM CASO |
| D31 · dia sem dono | Na resolução da sobreposição, só acrescenta dia sem dono se exatamente um vínculo o cobre e ele é o vínculo da fatia; fatia sem dias próprios é pulada. | `app/ponto/services/fechamento.py::apurar_por_fatia:227`, `:230`, `:235` | `_meus`; extensão da fatia. | Seleção de batidas. | SEM JUIZ | SEM CASO |
| D32 · batidas por fatia | Usa data local, pad de um dia nas pontas e exclui dias internos atribuídos a outro vínculo. | `app/ponto/services/fechamento.py::apurar_por_fatia:203`, `:244`, `:253`, `:260` | `batidas_fatia`. | Motor `:286`. | SEM JUIZ | SEM CASO |
| D33 · universo apurável | Só `retratada_em IS NULL`; filtros inclusivos por instante e colaborador; ordenação por timestamp. | `app/ponto/turnos.py::batidas_apuraveis:2876` | Lista/query de batidas. | Fechamento `app/ponto/services/fechamento.py:633`; férias no export. | SEM JUIZ | SEM CASO |
| D34 · deduplicação | Mantém apenas tipos E/S; elimina mesmo tipo consecutivo quando o gap assinado é menor que `self.DUPLICATA`. | `app/ponto/motor_calculo_v2.py::MotorBase.remover_duplicatas:902`, `:909` | Batidas limpas. | `calcular_mes:2351`; montagem de períodos. | SEM JUIZ | SEM CASO |
| D35 · motor | Sem template usa 12x36; ciclo/base intermitente seleciona intermitente; base desconhecida levanta; partido contínuo pode selecionar comercial ou 12x36 conforme cruzamento da meia-noite. | `app/ponto/motor_calculo_v2.py::get_motor:3640`, `:3649`, `:3651`, `:3659`, `:3687` | Instância de motor. | Apuração da fatia. | SEM JUIZ | SEM CASO |
| D36 · partido | Sem as duas horas de pausa retorna falso; com ambas exige vão circular **maior que 120 minutos**. | `app/escala/servico_jornada.py::eh_turno_partido:154`, `:156` | Escolha de geometria. | `get_motor:3662`. | TP — `app/core/juizes.py:220` | SEM CASO |
| D37 · dia da jornada | Usa mapa do juiz ou data local da entrada; período iniciado de zero a seis horas após saída anterior herda seu dia; exclui jornada anterior/posterior à fatia. | `app/ponto/motor_calculo_v2.py::MotorBase.calcular_mes:2403`, `:2407`, `:2423`, `:2429` | Mapa dia/período; períodos incluídos. | Folga, janela, soma mensal e DiaPago. | SEM JUIZ específico desse predicado. | SEM CASO |
| D38 · papel da batida | Papel da ata precede tipo gravado; papéis X/Xi marcam intervalo; proximidade de marco de pausa pode absorver a batida como interna. | `app/ponto/turnos.py::parear_turnos:1310`, `:1316`, `:1356`, `:1382` | Turnos e marcas de intervalo em memória. | `_pares_marcados`; motor. | SEM JUIZ nominal de `parear_turnos`; registro de turnos aponta `turnos_do_colab` em `app/core/juizes.py:208`. | SEM CASO |
| D39 · pausa por duração | Sem bordas, exige S→E dentro do teto e S posterior provando continuidade; com bordas, exige S distante mais de 90 minutos e retorno distante ou continuidade comprovada. | `app/ponto/turnos.py::_marca_intra_duracao:386`, `:413`, `:453` | `_intra_dur` nas batidas em memória. | Pareamento; valoração da pausa. | SEM JUIZ | SEM CASO |
| D40 · envelope do turno | Com duas bordas distintas: jornada circular mais quatro horas; senão envelope recebido. No fechamento por S, exceder teto separa aberto/órfã; duas pontas distantes podem restaurar envelope de 26h. | `app/ponto/turnos.py::_teto_da_jornada:78`; `::parear_turnos:1444`, `:1447` | Geometria dos turnos. | Intervalos e motor. | SEM JUIZ | SEM CASO |
| D41 · pares de pausa | Pares declarados têm prioridade; demais exigem marcas, exclusão das bordas, combinação admitida de papéis/tipos e ordem temporal não negativa. | `app/ponto/turnos.py::_pares_marcados:811`, `:880` | Pares candidatos de pausa. | `intervalos_do_turno:545`. | SEM JUIZ | SEM CASO |
| D42 · piso do gozado | Só considera descanso os pares marcados com duração de pelo menos 15 minutos. | `app/ponto/turnos.py::intervalos_do_turno:544`; constante `:499` | Pares de intervalo reconhecido. | `_intra_real`; turno partido. | TI — `app/core/juizes.py:214` | `app/ponto/tests/test_bug100_intra_da_autoridade.py::ParCurtoNaoEIntrajornadaTest::test_MORDE_par_de_2min_nao_e_intervalo_e_o_minimo_INTEIRO_e_devido:87`: Art.71 §4; pausa de 2 → 60 minutos indenizados. |
| D43 · ata substitui geometria | Exige regime com marcos, células alimentadas, ata alinhada/fechada, batidas reais correspondentes e quantidade de batidas internas não superior às lâmpadas acesas; senão preserva geometria anterior. | `app/ponto/motor_calculo_v2.py::_periodos_pelo_marco:559`, `:573`, `:632`, `:641`, `:673` | Substituição dos períodos e intervalos do dia. | `calcular_periodo`. | SEM JUIZ | SEM CASO |
| D44 · pausas da ata | Aceita pares de lâmpadas acesas com instantes presentes e retorno não anterior; soma pausas múltiplas; fallback cadastrado é informação da resposta. | `app/ponto/juiz_batida.py::periodos_do_dia:280`, `:287`, `:291`, `:294` | `intervalo.pausas/minutos/fonte`. | `_periodos_pelo_marco:689`; gate da pausa. | BD — `app/core/juizes.py:1193` | SEM CASO |
| D45 · recorte do gozado | Cada pausa vira interseção com as pontas efetivas: `max(início,e)`/`min(fim,s)`; só conserva extensão positiva. | `app/ponto/motor_calculo_v2.py::MotorBase._intervalos_na_janela:1885` | Intervalos efetivos. | `_intra_real`; carimbo do período; noturno. | SEM JUIZ | `app/ponto/tests/test_o280_bateria_da_lei.py::BugsTest::test_B_O272b_com_a_janela_LIGADA_o_recorte_nao_desconta_duas_vezes:2552`: L-097; pausa fora da janela → gozado 0, intra 60. |
| D46 · duração gozada | Soma diferenças dos instantes locais dos pares e retorna `max(0,total)`, em minutos de relógio. | `app/ponto/motor_calculo_v2.py::MotorBase._intra_real:1909` | `intra_real`. | Cálculo do suprimido. | SEM JUIZ | SEM CASO |
| D47 · período aberto | Saída ausente retorna antes do cálculo de intra, mantendo o default zero. | `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2752`; `::MotorComercial.calcular_periodo:3217`; default `:235` | `PeriodoCalculo.minutos_intrajornada_indenizada`. | Soma mensal. | BP — `app/core/juizes.py:1194` | SEM CASO |
| D48 · 12x36 | Só entra no cálculo da intra quando duração **bruta efetiva >360 minutos**. Plantão delega a esse cálculo; intermitente o herda. | `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2767`; `::MotorPlantao.calcular_periodo:3535`; `::MotorIntermitente:3569` | Minutos indenizados do período. | Mensal e diário. | BP | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F2b_PINADO_o_intervalo_de_15_min_da_faixa_4h_6h_e_suprimido:1353`: CLT71 §1/§4; seis horas sem pausa → 15 minutos. Marcado `expectedFailure` em `:1352`. |
| D49 · suprimido | Calcula `max(0,mínimo-intra_real)`; grava apenas suprimido positivo após tolerância. | `app/ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo:2773`, `:2784`; `::MotorComercial.calcular_periodo:3246`, `:3255` | Minutos indenizados. | Soma mensal. | BP | `app/ponto/tests/test_intra_suprimida_art71.py::TestIntraSuprimidaArt71::test_intra_parcial_30min_indeniza_30min_suprimidos:40`: CLT71 §4; 30 gozados → 0,5h. |
| D50 · tolerância da intra | Suprimido **≤10 minutos** vira zero; acima disso permanece integral. | `app/ponto/motor_calculo_v2.py:69`; `::Motor12x36.calcular_periodo:2776`; `::MotorComercial.calcular_periodo:3247`; `::MotorTurnoPartido.calcular_mes:2998` | Minutos indenizados ou zero. | Mensal. | BP para os `calcular_periodo`; sem registro específico do ramo mensal partido. | SEM CASO — `app/ponto/tests/test_intra_tolerancia.py:21`, `:34` testa o corte, mas o fundamento dos **10** não identifica CLT/CCT/L-NNN. |
| D51 · comercial | Bruta >240 desconta pausa; somente **líquida >360** calcula indenização. | `app/ponto/motor_calculo_v2.py::MotorComercial.calcular_periodo:3239`, `:3245` | Minutos indenizados. | Mensal. | BP | `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F2b_PINADO_o_intervalo_de_15_min_da_faixa_4h_6h_e_suprimido:1353`: esperado normativo 15; `expectedFailure`, `:1352`. |
| D52 · partido elegível | Exige pelo menos dois períodos **normais** no dia; saída atual presente e saída/entrada seguinte atribuídas ao mesmo objeto turno. | `app/ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes:2967`, `:2971`, `:2981`, `:2984` | Elegibilidade do gap. | Parcela de intra do próximo período. | SEM JUIZ | SEM CASO |
| D53 · partido: parcela | Gap vem do par reconhecido por `intervalos_do_turno`, ou zero; com gap menor que mínimo e suprimido >10, acumula a diferença no período seguinte. | `app/ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes:2987`, `:2996`, `:3000` | `minutos_intrajornada_indenizada += suprimido`. | Mensal. | SEM JUIZ | `app/ponto/tests/test_turno_partido_zona4.py::TestTurnoPartidoZona4Intra::test_gap_30min_indeniza_30min_suprimido:19`: Art.71

[truncado em 20000 de 37890 caracteres]

## 14/11 10:30 · sessao 63aee0765761 · - · - · origem=Ronald (tmux)

**Q4 — NAO PROCEDE quebra de formato nos consumidores identificados.**

| Consumidor | Leitura e efeito |
|---|---|
| `placar_grade.py --conferir` | Extrai todo o bloco pelos marcadores e compara com `cabecalho(lidas, nao, compl)`, já incluindo a soma nova; não depende da quantidade de linhas do cabeçalho. `bin/placar_grade.py:147`, `bin/placar_grade.py:180`, `bin/placar_grade.py:183`. |
| Selo `test_placar_grade.sh` | Confere expressamente a soma no PLACAR e no cabeçalho; chama `--conferir` na GRADE viva. `bin/tests/test_placar_grade.sh:110`, `bin/tests/test_placar_grade.sh:111`, `bin/tests/test_placar_grade.sh:117`, `bin/tests/test_placar_grade.sh:123`. |
| `relato.sh` | Gera o PLACAR pelo leitor atualizado e entrega GRADE/PLACAR inteiros ao raspador; inclui ambos em add/commit. `bin/relato.sh:101`, `bin/relato.sh:119`, `bin/relato.sh:120`, `bin/relato.sh:127`, `bin/relato.sh:137`. |
| `raspar_publico.py` | Lê o texto inteiro, mascara e confere nomes; não interpreta quantidade de colunas nem a linha `Estados:`. `bin/raspar_publico.py:28`, `bin/raspar_publico.py:31`, `bin/raspar_publico.py:36`; a máscara trabalha com substituições textuais em `bin/nome_no_texto.py:91`, `bin/nome_no_texto.py:109`, `bin/nome_no_texto.py:121`. |
| Régua | Aciona todos os selos `tests/test_*.sh`, incluindo o selo do placar. `bin/regua.sh:138`, `bin/regua.sh:140`. |

**NAO PROCEDE atribuir leitura deste placar ao `pos_push` ou ao Stop hook:** `pos_push` chama os derivados de TICKETS e verifica/commita somente TICKETS; o Stop hook aponta para BACKLOG e RELATO. `bin/pos_push.sh:19`, `bin/pos_push.sh:20`, `bin/pos_push.sh:22`, `bin/pos_push.sh:29`; `bin/hook_stop_fila1.py:33`, `bin/hook_stop_fila1.py:34`.

**[nao verificado]** Não identifiquei outro consumidor mediante busca textual ampla na cópia. Isso não prova ausência de leitores externos, nem execução/publicação em produção.

**[nao verificado]** Não identifiquei código que consuma a nova tabela para escolher automaticamente a vaga do item 2; a afirmação de que “é dela que a regra lê” está no comentário de `bin/placar_grade.py:22`, enquanto o código apenas escreve a tabela em `bin/placar_grade.py:222`–`226`.

## 14/11 10:25 · sessao 01c6df45798f · - · - · origem=Ronald (tmux)

Complementos:

- **Veto por dia sem jornada — caso L-089:** `app/ponto/tests/test_o198_falta_em_dia_sem_jornada.py:11-12` escreve a regra; `FaltaEmDiaSemJornadaTest::test_falta_em_dia_sem_jornada_pela_CELULA_e_recusada:100` exige recusa com célula `trabalha=False`; `test_falta_em_dia_sem_jornada_pela_ARITMETICA_e_recusada:114` exige recusa no sábado; `test_o_RANGE_recusa_nomeando_o_dia_que_ofende:150` exige recusa do intervalo quarta–sábado, nomeando sábado. O contraponto `True` aceita em `test_MORDE_o_MESMO_dia_com_a_celula_TRABALHA_segue_aceito:108`; `None` aceita em `TriEstadoNaoRecusaTest::test_MORDE_sem_vinculo_que_responda_NAO_recusa:218` e `test_MORDE_12x36_sem_ancora_devolve_None_e_NAO_recusa:226`.

- **FALTA_DIA/minutos da grade — caso L-024:** `app/chamados/tests/test_h06_falta_pela_grade.py:1-3,11-19` escreve norma e respostas. `FaltaDeclaradaPelaRespostaLeAGrade::test_MORDE_6x1_de_7h_desconta_420:86`, `test_MORDE_12x36_desconta_o_previsto_da_grade_660:90`, `test_MORDE_meio_periodo_desconta_240:94` conferem respectivamente420/660/240min. `FaltaDeclaradaPelaPerguntaLeAGrade::test_MORDE_declara_falta_nasce_sem_numero_e_deriva_no_flip:110` confere nascimento0 e aprovação420 (`:118-122`). `FaltaEmRangeSomaAGradeDiaADia::test_range_seg_a_qua_soma_a_grade:128` confere1260min (`:134`). Isso cobre minutos, não contagem do TXT.

- **Falta decidida/furo — caso L-039:** `app/docs/LEIS.md:87` associa a lei e nomeia `app/relatorios/tests/test_falta_um_significado.py::FaltaUmSignificado::test_MORDE_o_furo_sem_decisao_nao_vira_falta_e_nao_some:121`. Sem ausência, `dias_falta=0` (`:128`); cria uma falta aprovada (`:134-135`), exige `dias_falta=1` e retira a data de em_aberto (`:137-140`). Método sem paridade; os outros métodos dessa classe incluem paridade/estrutura (`:93-119,144-178`).

- **Estado inicial/aprovação/reabertura:** há regra de corte e casos comportamentais em `app/ponto/tests/test_a4_estados_com_porta.py:11-24` e `app/ponto/tests/test_reverter_rejeicao.py:4-16`, porém ligação específica a CLT/CCT/L-NNN para essas decisões não foi localizada [não verificado]. Para filtro estrito, **SEM CASO**. Casos encontrados: `OEstadoNasceComDonoTest::test_MORDE_tipo_que_exige_doc_SEM_anexo_nasce_aguardando_documento:92`, `test_MORDE_com_anexo_nasce_aguardando_decisao:98`, `test_tipo_que_NAO_exige_doc_nasce_aguardando_decisao:105`; `TransicaoTest::test_so_o_DP_aprova:168`, `test_MORDE_aprovar_sem_passar_pelo_DP_nao_acontece:175`; `ReverterRejeicaoTest::test_MORDE_o_ANEXO_decide_o_destino:51` confere estados opostos conforme presença de anexo (`:59-66`).

- **Sobreposição/abono sobre falta:** regra de corte09/09 escrita em `app/ponto/tests/test_bug107_justificar_falta.py:18-20`, com casos `JustificarFaltaPassaTest::test_MORDE_atestado_sobre_falta_ENTRA:66`, `test_falta_sobre_falta_continua_recusada:86`, `test_MORDE_atestado_sobre_ATESTADO_continua_recusado:94`; ligação CLT/CCT/L-NNN dessa regra não localizada [não verificado]. O teste de entrada espera `aguardando_documento`, sem aprovação automática (`:73-77`).

Registro real, sem usar docstrings: `app/core/juizes.py:254` registra `veto_de_lancamento`, pergunta `_A3` definida em `:427`; `:255` registra `estado_inicial`, `_A4` em `:428`; `:256` registra `ABERTAS`, `_A5` em `:429`. Entrada própria para aprovação/`mudar_estado`/materialização/reabertura em `JUIZES` não localizada [não verificado].

## 14/11 10:24 · sessao 8af52e747d91 · - · - · origem=Ronald (tmux)

Adicionais confirmados com expectativa independente:

| Regra | Caso | Resposta |
|---|---|---|
| L022, `app/docs/LEIS.md:73` | `app/escala/tests/test_o37_foto_parcial.py::O37FotoParcialTest::test_RED_col418_os_dias_fora_da_foto_ALTERNAM:69` | Foto com folgas pares até20/09→21–30 alternam `[True,False]*5` (:73–76). |
| L022 | Mesmo arquivo/classe, `test_MORDE_a_foto_em_dia_IMPAR_inverte_a_fase:87` | Foto ímpar→`[False,True]*5` (:94–98). |
| L023, `app/docs/LEIS.md:74` | `app/escala/tests/test_foto_nao_apaga_o_ciclo.py::FotoNaoApagaOCicloTest::test_MORDE_5x2_o_caso_do_BO_sabado_com_uma_folga_no_mes:50` | Folga avulsa não apaga sábado/domingo=False e sexta=True (:55–58). |
| L023 | Mesmo arquivo/classe, `test_MORDE_6x1_a_mesma_regra:60` | Domingo=False, sábado=True (:64–65). |
| L025, `app/docs/LEIS.md:76` | `app/ponto/tests/test_batidas_apuraveis.py::BatidasApuraveisTest::test_retratada_fora_do_universo:32` | Duas batidas, uma retratada→somente timestamp da válida (:37–44). |
| L025 | Mesmo arquivo/classe, `test_pendente_dp_conta:46` | `aprovada=False`, sem retratação→universo de1 (:51–56). |
| L026, `app/docs/LEIS.md:77` | `app/ponto/tests/test_periodo_apuracao.py::PeriodoApuracaoFuncTest::test_corte_26:34` | Competência06/2026,corte26→26/05–25/06 (:35). |
| L026 | Mesmo arquivo/classe, `test_default_21:31` | Competência05/2026→21/04–20/05 (:32). |
| L026 | Mesmo arquivo/classe, `test_jan_corte_26_vira_ano:37` | Competência01/2026,corte26→26/12/2025–25/01/2026 (:38). |
| L020, `app/docs/LEIS.md:71` | `app/escala/tests/test_celula_dia.py::MarcosDnaTest::test_dna_congelado_vence_edicao_template:243` | Cadastro07–19 congelado; template muda para08→marcos continuam07/19 (:246–250). |
| L032, `app/docs/LEIS.md:80` | `app/ponto/tests/test_he_intra_dobrada.py::HeIntraDobradaTest::test_RED_doze_horas_sem_intervalo_NAO_tem_hora_extra:77` | 07–19 sem intervalo→intra60min e HE0 (:82–86). |
| L032 | Mesmo arquivo/classe, `test_MORDE_treze_horas_sem_intervalo_TEM_uma_hora_de_extra:88` | 07–20 sem intervalo→intra60min e HE60min (:90–93). |
| L032/Art.71 | `app/ponto/tests/test_e3_metade_intervalo_cadastrado.py::MetadeIntervaloCadastradoTest::test_v3_o_PISO_vem_do_JUIZ_por_dia_e_NUNCA_de_minutos_jornada:68` | Previsto345→piso15;480→60;240→0 (:81–89). |
| L093, `app/docs/LEIS.md:111` | `app/ponto/tests/test_l093_trabalhado_real.py::TetoUsaOTrabalhadoRealTest::test_MORDE_o_rateio_entre_as_duas_rubricas_se_mantem:92` | Prev480,trab100,fora100,atraso200,antecipada100→soma280 e razão2:1 (:95–99). |
| L084, `app/docs/LEIS.md:116` | `app/ponto/tests/test_l084_vence_o_t8.py::L084VenceOT8Test::test_RED_a_entrada_a_452_min_do_marco_VOLTA_a_ser_atraso:69` | Cadastro07–19,batidas14:32–19→452min atraso (:75–78). |
| L033, `app/docs/LEIS.md:81` | `app/folha/tests/test_furo_so_intervalo_nao_retem.py::FuroSoIntervaloNaoRetemTest::test_so_o_intervalo_faltando_NAO_retem:83` | Entrada/saída presentes, pausa ausente→lista de retenções vazia (:85–86). |

A bateria de lavratura não contém assertivas de `atraso`/`antecipada` nem caso de feriado; contém realizado, lâmpadas, órfãs e cadastro. Seu caso `OrfaNasDuasPontasTest::test_MORDE_as_duas_pontas_sao_orfas_e_nada_some:633` fundamenta1180min realizados na L084, mas não certifica o raio90 (`app/ponto/tests/test_bateria_lavratura.py:637–644`).

## 14/11 10:24 · sessao fe3322a77de8 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo foram inspecionados em leitura; execução em produção, conteúdos do cadastro e frequência de uso: [nao verificado].

| Decisão | Predicado real e insumo | Escrita / leitor | Registro |
|---|---|---|---|
| Catálogo seleciona tipo descontável | `_do_banco` seleciona `TipoAusencia.ativo=True`; `vigente` usa cache <60s, banco ou `CATALOGO`; `descontam_vigentes` seleciona efeito DESCONTA | `app/ponto/catalogo/ausencias.py::_do_banco:249`, `::vigente:275`, `::efeito_vigente:293`; cadastro escrito por `app/ponto/services/ausencia.py::gravar_tipo_ausencia:1306`; campos recebidos em `app/ponto/views_tipos_ausencia.py::salvar_tipo_ausencia:64` | `ausencia/ferias → efeito_vigente`, `app/core/juizes.py:252`; política de cache/fallback **SEM JUIZ** |
| Criar somente tipo válido | `tipo not in codigos_validos()` recusa | `app/ponto/services/ausencia.py::criar_ausencia:194`; construção dos campos :341; `a.save()` :418 | `fim_de` registrado para duração/tipo em `app/core/juizes.py:273`, mas **criar_ausencia/codigos_validos SEM JUIZ nomeado** |
| Derivar fim | `indeterminado or tipo in TIPOS_EM_ABERTO → None`; `dias_corridos>1 → data+dias−1`; resto None | `app/ponto/services/ausencia.py::derivar_data_fim:70`; criação :197, edição :477 | `fim_de` registrado :273; **derivar_data_fim SEM JUIZ nomeado** |
| Aceitar parcial declarado | `checar_parcial and minutos and tipo not in parciais_vigentes()` recusa | `app/ponto/services/ausencia.py::criar_ausencia:206`; edição aplica mesma condição sem flag em :480 | **SEM JUIZ** para corte da porta |
| Reenvio mesmo fato | `se_existir=='devolver_mesmo_fato'` devolve `(colaborador,tipo,data)` não rejeitado | `app/ponto/services/ausencia.py::criar_ausencia:246` | **SEM JUIZ** |
| Reabrir rejeitada em vez de duplicar | Busca `(colaborador,tipo,data,status='rejeitada')`; antes de reabrir, veto pelo mesmo tipo/data ou `linha_que_veta`, dependendo de `se_existir`; conflito retorna nos modos retornar, senão recusa | `app/ponto/services/ausencia.py::criar_ausencia:254`, :270, :277, :291 | Veto registrado em `app/core/juizes.py:254`; política de reabertura **SEM JUIZ** |
| Modo chamado pode criar tipo diferente ao lado | `se_existir=='retornar_mesmo_tipo'`: mesmo tipo/data retorna; inexistente zera conflitos, sem consultar veto posterior (somente `se_existir=='veto'` o consulta) | `app/ponto/services/ausencia.py::criar_ausencia:294`, :302, :377 | **SEM JUIZ** para exceção |
| Modo automático retorna conflito | Existindo conflito e `se_existir=='retornar'`, retorna linha, sem criar | `app/ponto/services/ausencia.py::criar_ausencia:309` | **SEM JUIZ** para política |
| Abono sobre falta atravessa sobreposição | Só ausência existente `tipo=='falta'` e tipo novo em `abonam_vigentes`; qualquer linha não absolvida veta | `app/ponto/precedencia.py::abona_sobre_falta:127`; `::linha_que_veta:145`; usada em criar :322 e veto :461 | Registro da porta `veto_de_lancamento`, `app/core/juizes.py:254`; helpers não nomeados |
| Estado inicial por documento | Documento ou tipo férias → aguardando_decisao; exige documento e sem documento → aguardando_documento; demais aguardando_decisao | `app/ponto/services/ausencia.py::estado_inicial:792` | Registrado `app/core/juizes.py:255` |
| Status explícito substitui estado inicial | `status=(status or _st_inicial)`; status aprovado carimba aprovador/data | `app/ponto/services/ausencia.py::criar_ausencia:344`, :347 | **SEM JUIZ** para substituição |
| Veto da folga lançada | `fl is not None and tipo=='folga'` recusa; `fl is not None and not _vence_a_folga` recusa; `_vence_a_folga = faixa_de(tipo) in ('ferias',)` | `app/ponto/precedencia.py::veto_de_lancamento:496`, :510, :511, :514 | Veto registrado `app/core/juizes.py:254`; folga registrada :204 |
| Falta somente jornada conhecida como trabalho | Para `tipo in ('falta',)`, existe vínculo e `eh_dia_trabalho(d) is False` → recusa; vínculo ausente/None não recusa | `app/ponto/precedencia.py::veto_de_lancamento:536` | Veto :254 e dia trabalho :184 de `app/core/juizes.py` |
| Unicidade entre vivas | Mesmo colaborador/tipo/data proibido se status != rejeitada | `app/ponto/models.py::Ausencia.Meta:827`; `criar_ausencia` traduz IntegrityError dessa constraint em recusa em `app/ponto/services/ausencia.py:421` | **SEM JUIZ** específico |
| Editar muda diretamente tipo e range | Valida tipo/parcial; `ausencia_sobrepoe` excluindo própria PK recusa qualquer outra; grava tipo/data/fim/dias e zera ou recebe minutos | `app/ponto/services/ausencia.py::editar_ausencia_registro:474`, :487, :498, :508, :509 | **SEM JUIZ** para porta de edição; não chama `veto_de_lancamento` |
| Transição decide aprovação/rejeição | Mesmo estado retorna; `novo not in _TRANSICOES[atual]` recusa; tabela permite aguardando_decisao→aprovada e aprovada→rejeitada, rejeitada→aprovada/abertas | `app/ponto/services/ausencia.py::_TRANSICOES:768`; `::mudar_estado:819`; grava status :825 e salva :836 | `ABERTAS` registrado :256; **mudar_estado/_TRANSICOES SEM JUIZ** |
| DP decide ausência | `veredito not in ('aprovada','rejeitada')` recusa; delega `mudar_estado` | `app/ponto/services/ausencia.py::decidir:905`, :912 | **SEM JUIZ** para decisão DP |
| Reabrir volta ao estado do anexo | Exige rejeitada e motivo não vazio; `_reabrir` consulta `estado_inicial(tipo,bool(documento novo ou atual))`, depois muda estado | `app/ponto/services/ausencia.py::reverter_rejeicao:1098`; `::_reabrir:1120`, :1142 | Estado inicial registrado :255; política reverter **SEM JUIZ** |
| Prazo vencido não converte em falta | `hoje > prazo_documento` e aplicar → aguardando_decisao | `app/ponto/services/ausencia.py::escalonar_aguardando_documento:1000` | **SEM JUIZ** para vencimento/transição |
| Resposta de chamado determina falta aprovada | `resposta in ESCAPES_QUE_VIRAM_AUSENCIA`; tabela `FALTA_DIA→('falta','aprovada')`, `ATESTADO→('atestado','')`; se `p.data_evento is None`, nada cria | `app/chamados/services/disputa_emissao.py::responder_pergunta_disputa_core:1538`; tabela `app/chamados/services/materializacao.py:603`; `::_materializar_ausencia_de_resposta:523`, :527 | **SEM JUIZ** para resposta→tipo/status |
| Pergunta declara_falta produz ausência a decidir | `_DECLARA_FALTA_MAPA.get(resposta.strip())`; desconhecida retorna; data é `p.data_evento`, senão contexto.data, senão erro; sem status explícito | `app/chamados/services/materializacao.py::_declarar_ausencia_de_pergunta:474`, :479, :491; mapa `app/chamados/models.py:3772`; acionamento `materializacao.py:1083` | **SEM JUIZ** para mapa/data |
| Veredito humano da célula | Ramo diferente de trabalhou escolhe `'falta' if veredito=='faltou' else tipo_ausencia`; sem tipo recusa; cria aprovada | `app/ponto/services/veredito_celula.py::veredito_celula:333`, :337 | **SEM JUIZ** específico |
| Regularização cria dias intermediários | Entrada deve existir, pertencer ao colaborador e ser E; `turnos_do_colab` deve identificar turno aberto; laço D+1 interrompe antes de hoje; cria falta sem status explícito | `app/ponto/services/regularizacao.py::regularizar_turno_orfao:52`, :62, :66, :111`, :118 | Turnos registrados `app/core/juizes.py:208`; seleção dos dias e conversão **SEM JUIZ** |
| Abandono provisório cria dias intermediários | Ativo+usuário+onboarding; último turno aberto com entrada; entrada ≤ agora−72h e dias ausente≥4; laço D+1..ontem cria falta sem status explícito | `app/ponto/management/commands/marcar_abandono_provisorio.py::Command.handle:35`, :47, :52, :59, :70, :76 | Turnos registrados :208; corte/seleção/conversão **SEM JUIZ** |
| Admin cria ausência já aprovada | `lancar_ausencia` passa tipo/data/range/minutos/documento e `status='aprovada'` | `app/ponto/views.py::lancar_ausencia:2415`; copiloto mesma aprovação em `app/ponto/services/checar_ausencia.py::lancar_ausencia_porta:46` | **SEM JUIZ** para aprovação direta |
| PWA tem veto antecipado próprio | Qualquer `ausencia_sobrepoe` recusa antes de `criar_ausencia`; não aplica `abona_sobre_falta` nessa etapa | `app/ponto/views.py::submeter_ausencia:2525`; criação :2536 | **SEM JUIZ** para veto desse leitor |
| API calcula quantidade de dias | Se data_fim legível e >=data, dias=fim−data+1; erro de parsing passa sem alterar; cria com modo devolver_mesmo_fato | `app/api/views.py::api_ausencias:1790`, :1805 | **SEM JUIZ** específico |
| Férias espelhadas | Agendamento ativo se status em agendado/em_gozo/concluido; se inativo apaga existentes; ativo cria uma ausência férias aprovada por dia inexistente; remove sobras | `app/ponto/services/ausencia.py::espelhar_ferias:1181`, :1189, :1199, :1203, :1212 | **SEM JUIZ** para espelhamento |
| Férias excluídas/rejeitadas em lote | Apagar férias do colaborador/range, preservando exceto; rejeitar férias não rejeitadas do range | `app/ponto/services/ausencia.py::apagar_ausencias_de_ferias:1266`, :1271; `::rejeitar_ausencias_de_ferias:1282` | **SEM JUIZ** específico |
| Re-derivação de range pelo modelo | Fim<data recusa; fim existente deriva dias; sem fim, dias>1 e tipo não aberto deriva fim | `app/ponto/models.py::Ausencia.save:894`, :899, :902 | `fim_de` registrado :273; `Ausencia.save` **SEM JUIZ nomeado** |
| Expansão das datas do export | Fim explícito prevalece; sem fim, tipo aberto usa fim da janela, senão dia único; recorta max(início)/min(fim), inclui cada dia | `app/ponto/turnos.py::dias_da_ausencia:2791`; chamada por `app/folha/export.py::datas_no_periodo:69` | Registro aponta `cobertura_ausencia_periodo` em `app/core/juizes.py:253`, **não dias_da_ausencia** |

Escritores físicos adicionais e exclusões:

- `app/ponto/services/ausencia.py::rederivar_fim:1297` chama `Ausencia.save()`; pode alterar fim/dias/minutos conforme predicados do modelo.
- `app/ponto/services/ausencia.py::apagar_ausencias_em_ensaio:1289` exige banco de ensaio e apaga queryset :1291; pode remover falta se queryset a contiver.
- AusenciaAdmin está em leitura por `SoLeitura`, `app/ponto/admin.py:37`; **JustificativaAdmin usa ModelAdmin normal**, `app/ponto/admin.py:18`, portanto configuração expõe escritor genérico de justificativa; execução efetiva [nao verificado].
- Aprovações explícitas de Justificativa: `app/ponto/services/justificativa.py::aprovar_justificativa_dp:205`, save :208; `::materializar_saida_dp:383`, save :386; `app/ponto/views.py::aprovar_justificativa_lote:1796`, save :1799.
- Rejeições explícitas: `app/ponto/services/justificativa.py:283`; `app/ponto/views.py:1875`; `app/chamados/reconciliador.py:533`; `app/ponto/services/triagem_batida.py:608`; `app/ponto/services/entrada_sincronizada.py:41`.
- Criação/edição Justificativa: `app/ponto/views.py:1625`, :1635; `app/api/views.py:1986`, :1992; `app/ponto/services/regularizacao.py:91`, :105; `app/ponto/management/commands/processar_alertas_turno.py:394`.
- Justificativa aprovada pode materializar batida S pelo chokepoint: `app/ponto/services/justificativa.py:188`; porta exige saida>entrada :159, batida criada :195 e não vinculada a outra justificativa :197. Materialização retroativa repete essas verificações em :355, :361, :370, :372.
- Aprovação de ausência dispara tratamento de célula/chamados pelo evento, `app/ponto/signals.py:143`, :275; `app/chamados/services/silencio_ausencia.py::reconciliar_por_ausencia:78` suspende abertas e :94 trata aprovadas. **Esses trechos não rejeitam/apagam uma Ausencia falta concorrente**; o export seleciona diretamente as faltas aprovadas em `app/folha/export.py:87` e expande todas via :160.

Minutos e DSR devem ficar como ramos paralelos, sem atribuir contribuição à quantidade TXT:

- `Ausencia.save` deriva minutos somente se aprovada+tipo falta+minutos falsy, somando `minutos_previstos_do_dia` no range, `app/ponto/models.py:877`–887. Esses minutos não são entrada de `datas_falta`, `app/folha/export.py:180`–185, nem de `montar_bloco_falta`, :218.
- DSR não consta na lista retornada por `eventos_do_fechamento`, `app/folha/export.py:435`–461; comentários de :453–460 não substituem essa constatação do corpo executável. Nenhum conversor DSR→Ausencia foi encontrado nas buscas realizadas: [nao verificado] para inexistência absoluta.
- Férias na semente têm ABONA e falta DESCONTA, `app/ponto/catalogo/ausencias.py:88`, :101; o banco pode alterar esses efeitos, :249–251. Portanto exclusão das férias de `datas_falta` depende do cadastro vivo, não de filtro literal `tipo=='falta'`.

Casos pela regra relevantes, sem rodar testes: [nao verificado]

- Derivação **auxiliar de minutos**, pela L-024 explicitada em `app/chamados/tests/test_h06_falta_pela_grade.py:1` e casos/respostas :11–19: `FaltaDeclaradaPelaRespostaLeAGrade::test_MORDE_6x1_de_7h_desconta_420` :86, `::test_MORDE_12x36_desconta_o_previsto_da_grade_660` :90; `FaltaEmRangeSomaAGradeDiaADia::test_range_seg_a_qua_soma_a_grade` :128. Conferem 420/660/1260 minutos; não conferem TXT.
- Veto da jornada, L-089 escrita em `app/ponto/tests/test_o198_falta_em_dia_sem_jornada.py:11`: `FaltaEmDiaSemJornadaTest::test_falta_em_dia_sem_jornada_pela_CELULA_e_recusada` :100; `::test_falta_em_dia_sem_jornada_pela_ARITMETICA_e_recusada` :114; `::test_o_RANGE_recusa_nomeando_o_dia_que_ofende` :150. Resposta verificada é recusa, não número de dias do TXT.
- `app/ponto/tests/test_bug107_justificar_falta.py:18` enuncia o corte de abono sobre falta; `JustificarFaltaPassaTest::test_MORDE_atestado_sobre_falta_ENTRA` :66 confere existência e aguardando_documento. Não cita CLT/CCT/L-NNN no trecho lido; pelo critério estrito pedido, **SEM CASO** para número TXT.
- Os demais predicados acima: caso numérico contra CLT/CCT/L-NNN não confirmado por este levantamento; **SEM CASO [nao verificado]**, para consolidação com a pesquisa dos outros agentes.

## 14/11 10:23 · sessao a96a9742d24e · - · - · origem=Ronald (tmux)

Checagem adicional:

| Decisão | Caso pela regra e esperado |
|---|---|
| Entrada antes do marco; piso/teto | `app/ponto/tests/test_janela_he_regra.py::JanelaHeRegraTest::test_RED_06h40_com_marco_07h00_nao_gera_HE:83`: trabalho 660, HE 0. `::test_MORDE_06h52_esta_dentro_do_piso_e_NADA_muda:91`: trabalho 668, Art.58 §1 em `:92`. `::test_RED_05h30_passa_do_teto_e_entra_em_CADASTRO_x_REALIDADE:97`: trabalho 660, HE 0 e acusação. L-097 escrita no módulo `:2`. |
| Vigência da janela | `app/ponto/tests/test_janela_he_regra.py::JanelaHeRegraTest::test_MORDE_a_vigencia_protege_a_competencia_09:107`: antes da vigência, trabalho 680; L-092 em `:108`, asserção `:110`. |
| Recorte da saída | `app/ponto/tests/test_janela_he_saida.py::JanelaHeSaidaTest::test_RED_so_a_saida_07h06_conta_ate_o_marco:91`: trabalho 660, HE 0, asserções `:94`. L-097 no módulo `:2`. |
| Âncora da saída | `app/ponto/tests/test_janela_he_saida.py::JanelaHeSaidaTest::test_MORDE_marco_de_saida_ancorado_na_ENTRADA_nao_no_dia_da_saida:104`: 19–07:20 → trabalho 660, antecipada 0, asserções `:112`, `:114`. |
| Saída desligada / acima do teto | Mesmo arquivo, `::JanelaHeSaidaTest::test_MORDE_janela_de_saida_DESLIGADA_nao_clipa_nada:119`: trabalho 666, asserção `:125`; `::test_RED_saida_acima_do_teto_entra_em_CADASTRO_x_REALIDADE:142`: trabalho 660 e acusação, asserções `:145`, `:147`. |
| Autorização vigente por dia | `app/ponto/tests/test_janela_he_saida.py::DiaAutorizadoContaTest::test_RED_o_dia_AUTORIZADO_passa_a_contar_os_minutos_de_fora:255`: trabalho 740. `::test_MORDE_a_decisao_e_do_DIA_e_nao_da_PONTA:269`: ambas as pontas devolvidas, trabalho 735. `::test_MORDE_a_decisao_de_OUTRO_dia_nao_vale_para_este:277`: trabalho 720. L-097 escrita na classe `:199`. |
| Recusa, revogação, autorização | `app/ponto/tests/test_o280_bateria_da_lei.py::BugsTest::test_B_O273_recusa_revogada_nao_move_o_dia_e_a_autorizacao_move:2606`: sem decisão 720/HE0/fora40 (`:2655`); autorizado 760/HE40/fora0 (`:2684`). Regra L-097 aberta `:2617`. |
| L-084: duas pontas longe | `app/ponto/tests/test_o280_bateria_da_lei.py::ContratoDaTrocaTest::test_F9c_as_DUAS_pontas_longe_nao_se_julgam_pelo_marco:3363`: marcos 07–19, batidas 10:30–15:30 → trabalho 300, atraso 0, antecipada 0, intra 0; regra `:3364`, asserção `:3390`. |
| Janela pelo dia da jornada | `app/ponto/tests/test_o280_bateria_da_lei.py::V3FrotaTest::test_v3_B_P4a_PINADO_a_lei_paga_o_par_CRU:4962`: L-097, 02:30–05 após marco 19 do dia anterior → 150 minutos, asserção `:4982`; `expectedFailure` em `:4961`. |
| Cauda fora da janela da jornada | `app/ponto/tests/test_o280_bateria_da_lei.py::V3FrotaTest::test_v3_B_I28_PINADO_o_minuto_fora_do_marco_nao_gera_HE:5124`: par 07:32–07:43 depois do marco 07 → HE50 0, asserção `:5142`; L-097 em `:5125`, `expectedFailure` em `:5123`. |
| Deduplicação de par | `app/ponto/tests/test_o280_bateria_da_lei.py::V3FrotaTest::test_v3_B_P4b_o_par_REPETIDO_paga_UM_turno_UMA_vez:5253`: Art.4/71 e L-110, turno 07–19 duplicado → trabalho 720, intra 60, HE 0 (`:5345`); parcela da cópia, inclusive intra, igual a 0 (`:5337`). |
| Folga carrega intra | `app/ponto/tests/test_dia_pago_soma.py::LavraDaFolgaTrabalhadaTest::test_MORDE_ft_certa_leva_hora_NOTURNO_e_INTRA_e_nao_leva_antecipada_nem_HE:220`: 60 minutos do período → intra 1h (`:234`); regra escrita Súmula 437 em `:181`. Irmão `::test_MORDE_ft_sem_escala_certa_leva_a_hora_para_trabalhadas_com_noturno_e_intra:245`: intra 1h em `:256`. |
| Bloqueio de rubricas no dia sem trabalho previsto | **SEM CASO pelo critério estrito**: `app/ponto/tests/test_calculador_regras.py::RegrasDoDiaTest::test_MORDE_dia_sem_trabalho_previsto_o_calculador_NAO_afirma_rubrica:60` exige `{}` em `:71`, mas seu fundamento descreve divergência contra o motor em `:61`; não identifica CLT/CCT/L-NNN. |
| Fallback sem previsto em dia normal | `app/ponto/tests/test_calculador_regras.py::RegrasDoDiaTest::test_RED_sem_previsto_o_piso_do_Art71_vem_do_FATO:93`: 540 minutos sem previsto/pausa → intra 1h (`:104`), Art.71 escrito `:94`. |

Os casos da `V3FrotaTest` exercitam a releitura V3; a própria costura chama `reler` em `app/ponto/tests/test_o280_bateria_da_lei.py:3559`. A execução e aprovação dos casos permanecem [nao verificado].

## 14/11 10:22 · sessao 660c3616200d · - · - · origem=Ronald (tmux)

Evidências para consolidação, somente da cópia:

- **Número do TXT:** `app/folha/export.py::montar_linhas_export:740` chama `datas_falta`, resolve `'falta_diurna'` em `:741` e só emite quando datas e rubrica existem (`:742-744`). `::montar_bloco_falta:218` passa **`len(datas)`**, e `:219-220` emite um registro 11 por elemento. Não há deduplicação: `::_datas_ausencia_tipo_janela:159-162` usa `extend` e `sorted`.
- **Unidade:** `app/folha/export.py::montar_linha_10:39-43` grava `round(valor)` quando `rub.tipo == 'dias'`; qualquer outro tipo grava `round(valor*100)`. A unidade/configuração efetiva em produção é **[não verificado]**.
- **Não sai de `FechamentoMensal.horas_falta`:** a lista de `app/folha/export.py::eventos_do_fechamento:439-461` não contém falta inteira; ela é emitida depois, separadamente (`:740-744`). Portanto tolerância, noturno, CCT e supressão de feriado 12x36 desse caminho não compõem seu número.
- **A guarda de isenção não protege esse bloco:** `app/folha/export.py::eventos_do_fechamento:418-419` retorna lista vazia por isenção; `::montar_linhas_export:740-744` ainda consulta e emite falta sem perguntar isenção.

**Ramo paralelo de horas, não numerador da rubrica:**

- `app/ponto/services/fechamento.py::recalcular_fechamento_mes:873-875` chama `minutos_de_falta_por_dia` e soma minutos/60; `:1003` atribui `round(total_falta,2)` a `fechamento.horas_falta`; `:1173` persiste no `update` de `:1160`.
- `app/ponto/services/fechamento.py::recalcular_fechamento_mes:1200-1205` passa o mesmo mapa a `dia_pago.lavrar`; outra versão também recebe o mapa (`:1277-1281`).
- `app/ponto/services/dia_pago.py::lavrar:274-277` acumula `m/60` em `DiaPago.horas_falta`, guarda `falta_min`; `:360` faz `bulk_create`. Não transformar essas horas em origem do `len(datas)` do TXT.

**Leitores e recomputação:**

| Leitor | Comportamento efetivo |
|---|---|
| Cartão/espelho com fechamento | `app/relatorios/cartao_pela_celula.py::totais_da_folha:71-76`: verifica existência do fechamento, mas chama `fechamento_lido`; este **recalcula** via `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)`, `app/ponto/services/fechamento.py::fechamento_lido:1759-1761`. |
| Mapeamento do resumo | `app/relatorios/cartao_pela_celula.py::_totais_do_fechamento:92,104-105` consulta novamente `datas_falta`, conta e lista datas; `:115` lê as horas do objeto recebido. |
| Sem fechamento, com `DiaPago` | `app/relatorios/cartao_pela_celula.py::_totais_da_lavratura:143-145` lê soma lavrada; `:167,183-185` consulta novamente `datas_falta` para os dias e lê horas lavradas. |
| Seleção da fonte e atualização do resumo | `app/relatorios/cartao_pela_celula.py::folha_manda:321-346` escolhe fechamento online, depois lavratura; `::aplicar_nos_totais:203-211` não sobrescreve com `None`. |
| Em aberto | `app/relatorios/cartao_pela_celula.py::folha_manda:373-374` consulta novamente a falta decidida na **janela pedida**, inclusive período livre, convertendo-a em `set` para subtrair dos candidatos. Isso deduplica nesse leitor, enquanto o TXT conta a lista. |
| PDF | `app/relatorios/pdf_espelho.py:461-463` inicialmente coloca a falta apurada do motor no resumo; `:628` chama `folha_manda`, que pode substituí-la pela decidida. |
| Tela/app do espelho | `app/ponto/services/espelho.py:1283-1284` chama o mesmo `folha_manda`. |
| Comparador cartão × TXT | `app/relatorios/cartao_pela_celula.py::divergencia:250-253` lê fechamento **gravado**, porém seu mapeamento consulta `datas_falta` viva (`:92`). Compara `dias_falta` com tolerância `>0.5` (`:265-269`); não compara as datas. Consumidor: `app/relatorios/management/commands/cartao_x_txt_divergentes.py::Command.handle:63-70`. |
| Palavra da ausência | `app/ponto/services/dia_decidido.py::rotulo_de:492-497` deriva veredito e lê `ausencia.minutos`; só zera a exibição de minutos para abono de dia inteiro (`:494-495`). O desconto permanece com minutos gravados. |

**Portas da saída:**

1. `app/folha/views.py::gerar_dominio_txt:128-133` bloqueia segundo `porta_export.conferir`.
2. `app/folha/porta_export.py::conferir:806-808` lê o último carimbo da empresa de hoje; bloqueia ausência (`:811`), competência diferente (`:815`), valor nulo/negativo (`:818`) ou positivo (`:821,865-866`); zero libera (`:867`). **SEM JUIZ específico registrado** para `conferir` em `app/core/juizes.py` [não verificado para registro indireto].
3. `app/folha/views.py::gerar_dominio_txt:148-166` exige existir fechamento gravado aberto/aprovado. Em seguida **recalcula online** (`:207-209`), classifica com esses objetos (`:211-212`) e monta TXT com os mesmos objetos (`:265-268`).
4. `app/folha/export.py::montar_linhas_export:716-727` exige classificação `entra` e status em `STATUS_NO_TXT = ('aberto','aprovado')` (`:687`).
5. `app/folha/export.py::classificar_export:866-875` exclui demissão `<= fim_janela` (`::sai_pela_rescisao:60`) e código vazio; `:877-884` exclui férias com batida; `:902-966` exclui horas trabalhadas zero + pelo menos duas batidas apuráveis + nenhum dia com minutos previstos; `:968-978` exclui retenção do espelho e código não homologado.
6. `app/folha/export.py::dias_ferias_com_batida:491-502` recorta períodos de férias à janela e cruza data local das batidas apuráveis.
7. `app/folha/export.py::_codigos_nao_homologados:999-1000` desliga gate se catálogo vazio; `:1013-1018` inclui código da falta somente quando há datas e rubrica.
8. `app/folha/export.py::_rubricas_map:988-990` lê rubricas ativas globais e da empresa, empresa sobrescreve global.
9. `app/folha/views.py::gerar_dominio_txt:275-279` bloqueia erro de layout. `app/folha/export.py::validar_layout_dominio:280-290` confere contagem dos registros 11 contra valor **apenas quando código literal é `'8792'`**, não pelo evento configurado.

**Predicados internos da retenção:**

- `app/folha/export.py::motivos_retencao_celula:1205-1212`: isento ou desligado não retém; desligado tem juiz específico registrado em `app/core/juizes.py:267`.
- `app/folha/export.py::_dias_sem_decisao:1238-1247`: usa hoje, recorta admissão/demissão e consulta células.
- `:1252-1255,1267-1268`: sem célula e sem vínculo que cubra o dia é ignorado. Cobertura de vínculo registrada `_E2`, `app/core/juizes.py:1303,1310-1311`.
- `:1269-1272`: célula ausente retém apenas dia passado que não seja dia de demissão.
- `:1273,1285-1294`: veredito acusador retém, exceto protestos não vazios inteiramente contidos em `NAO_COBRAVEIS_DO_COLABORADOR` ou exceção da pausa.
- `:1295-1296`: veredito nulo com `trabalha is not None`, dia passado, fora do dia de demissão, retém.
- `app/folha/export.py::nao_retem_pela_pausa:1173`: exige protestos exatamente `{'FURO_PARCIAL'}` e `_furo_so_da_pausa`.
- `::_furo_so_da_pausa:1150-1155`: lê `periodos_do_dia`; desalinhado, em aberto ou não sei não isentam; exige pelo menos uma lâmpada apagada e todas referentes à pausa. `periodos_do_dia` está registrado em `app/core/juizes.py:1192-1194`.

**Registro real:** `_K1`/aprovação e `_K2`/entrada no TXT apontam a `classificar_export` em `app/core/juizes.py:319-320`; `_K3` é prontidão (`:321`). Não confundir travas de aprovação com exclusão do TXT: `app/folha/views.py::gerar_dominio_txt:243-252` aplica travas somente ao carimbo; a montagem usa `_itens`/`_fechs_lido` em `:268`.

## 14/11 10:22 · sessao 01c6df45798f · - · - · origem=Ronald (tmux)

Casos que podem sustentar decisões específicas da cadeia:

| Decisão | Caso pela regra | Resposta escrita e conferida |
|---|---|---|
| Furo só no intervalo dispensa retenção | `app/folha/tests/test_furo_so_intervalo_nao_retem.py::FuroSoIntervaloNaoRetemTest::test_so_o_intervalo_faltando_NAO_retem:83` | L-033 escrita em `:1-4`; entrada 08:02 e saída 17:03 presentes, duas lâmpadas de pausa apagadas → motivos `[]` (`:84-86`). |
| Ponta ausente mantém retenção | Mesmo arquivo/classe, `test_MORDE_entrada_faltando_RETEM:94`, `test_MORDE_saida_faltando_RETEM:98` | Pela mesma L-033, motivos `['espelho_furo']` (`:95-100`). Há limites adicionais contra ata ausente, divergente, `acesa=None`, mistura de protestos e ausência de pausa (`:111-175`). |
| Demissão dentro da janela sai por rescisão | `app/folha/tests/test_rescisao_modulo_proprio.py::RescisaoModuloProprioTest::test_desligado_na_janela_vira_rescisao:24` | Texto cita CLT art.477 (`:1-5`); demissão 10/07 → `fora`, `rescisao_modulo_proprio` (`:25-32`). |
| Borda de demissão | Mesmo arquivo, `H12DemissaoNaJanelaTest::test_demissao_no_ultimo_dia_da_janela_segue_rescisao:72`; `test_MORDE_demissao_depois_da_janela_nao_e_rescisao:75` | Último dia 20/07 mantém rescisão, 21/07 exclui esse motivo (`:44-52,72-76`); variante corte16 em `H12DemissaoNaJanelaCorte16Test:79-83`. |
| Falta aprovada inteira é dia decidido, sem acusação | `app/ponto/tests/test_bo_inss_nunca_bateu_le_a_precedencia.py::RamoNuncaBateuTest::test_RED_falta_aprovada_e_dia_decidido_nao_acusa:139` | L-039 expressa em `:140`; falta aprovada de 480 minutos (`:53,92-94,144`) → veredito fora de `VEREDITOS_ACUSAM` (`:147-148`). Não testa o número do TXT. |
| Justificativa dispensa marco ausente | `app/ponto/tests/test_ausencia_ata_dispensa.py::DispensaPorJustificativa::test_MORDE_dia_justificado_dispensa_o_marco:58` | Caso e L-061 escritos em `:51-55`; entrada ausente/saída presente+dia justificado → `n_missing=0` (`:59-63`). |
| Vínculo desativado sem fim não retém dia sem célula | `app/folha/tests/test_e4_parte5_dinheiro.py::RetencaoTest::test_MORDE_desativado_sem_fim_nao_retem_dia_sem_celula:69` | Regra escrita em `:4-10` sob L-110 e corte16/09; motivos `[]` (`:71-72`). |
| Vínculo ativo aberto retém dia sem célula | Mesmo arquivo/classe, `test_ativo_aberto_segue_retendo_dia_sem_celula:74` | Regra em `:6`; motivos `['sem_celula']` (`:76-77`). Método `test_PROPRIEDADE_ha_vinculo_no_dia_e_a_juiza:60` deve ser excluído: compara contra a própria autoridade (`:67`). |

Para as decisões diretas `len(datas)`, seleção `status='aprovada'`, tipos que descontam, expansão e recorte da ausência, **não localizei caso contra CLT/CCT/L-NNN que exerça o valor de `falta_diurna`** [não verificado]. Existem testes funcionais, mas sem regra normativa escrita: [nao verificado]

- `app/folha/tests/test_integracao.py::DatasFaltaTests::test_janela_status_tipo_ordem:92`: verifica seleção e ordenação (`:93-97`).
- `app/folha/tests/test_export_range.py::ExportRangeTest::test_falta_range_expande_todos_os_dias:27`: três dias → três datas (`:30-38`); `test_range_cruzando_borda_da_janela_recorta:40`: quatro dias cruzando início → duas datas internas (`:43-51`).
- `app/folha/tests/test_integracao.py::BlocoFaltaTests::test_bloco_8792_e_datas:74`: duas datas → `000000002` e dois registros11 (`:75-82`).
- `app/ponto/tests/test_catalogo_ausencias.py::TestParidadeComHardcoded::test_falta_desconta_e_esta_sozinha:36` pertence explicitamente à classe de paridade (`:23-27`).
- `app/folha/tests/test_ferias_folha.py` cita CLT142/143 (`:8-10`), porém seus métodos conferem choices, helpers e texto de código (`:21-38`), sem caso numérico de exclusão no8792.

Não usar como prova normativa do TXT:

- `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:11-13` declara expressamente **caracterização medida, não lei**; os números de falta/DSR são da tabela `MEDIDO` (`:54-67`).
- `app/ponto/tests/test_o280_bateria_da_lei.py::FronteiraTest::test_F6_ausencia_sobreposta_a_batida_nao_apaga_o_trabalho:1956` tem Lei605/49art.6 e resposta480min/0faltas/0abonos (`:1957-1966`), mas injeta `datas_justificadas` diretamente no motor (`:1964-1965`).
- `app/ponto/tests/test_o280_bateria_da_lei.py::AbsurdoTest::test_A1_zero_batidas_em_dia_previsto_e_falta_com_numero:2140` tem regra Lei605/49art.6+CLT473 (`:446-448`) e resposta1falta/previsto60 (`:2145-2146`), porém mede a apuração do motor, sem escrever/aprovar `Ausencia` nem emitir8792.
- `app/ponto/tests/test_fechamento_ausencia.py::FechamentoAusenciaPagaTest::test_atestado_semana_nao_perde_dsr:126` confere DSRperdido0 (`:143-149`), mas o contrato declarado é “mesma regra do coletor do espelho” (`:8-11`); sem CLT/CCT/L-NNN no arquivo.
- `app/folha/tests/test_bug8792_rateio_da_falta_por_dia.py::RateioDaFaltaPorDiaTest::test_as_partes_somam_o_total_mesmo_quando_NAO_divide:82` cita L-110 e confere3300min/9dias (`:83-92`), mas testa rateio de minutos, não a contagem8792; os próprios comentários distinguem `len(datas)` do rateio (`:21-24`).

_5057 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
