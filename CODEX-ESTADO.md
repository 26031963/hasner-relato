# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 11:58** · publicado em **17/11/2026 12:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 11:58 · sessao 85143d556d07 · RESPOSTAS-1150 · verificador · origem=Code

**AVAL-DEVOLVIDO: item 2.** As três costuras anteriores recebem uma direção, mas ainda faltam identidade e validade da tentativa, observador da morte, isolamento entre empresas e definição do contador. O carimbo atual não representa execução e sua chave não conserva duas competências da mesma empresa no mesmo dia. (`app/inteligencia/models.py:87–105`; `app/folha/porta_export.py:772–796`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–95`.)

A comparação integral com **PORTA-DO-EXPORT-SEM-BECO** permanece **[nao verificado]**: seu texto não acompanha o dado fornecido e não foi localizado na cópia. A porta documentada exige prova diária e recusa por divergência; o teste também exige apresentar a lista. (`app/folha/porta_export.py:2–15`; `app/folha/tests/test_porta_do_export.py:50–60`.)

**(a) Estado no registro do job; PID gravado e conferido**

1. **Premissas:** o padrão existe, mas pertence a `LoteEspelho`. Em `models.py:151` há uma guarda de `job_morreu`: estado fora de execução ou PID ausente retorna `False`. A checagem seguinte usa carência de 15 segundos e `os.kill(pid, 0)`. Essa linha não cria registro nem muda estado. (`app/relatorios/models.py:100–104`, `:118–132`, `:140–161`.)

   Quem grava PID/início é `lote_espelho.disparar`, com `Popen.pid`; `assumir` confirma `os.getpid()` e grava GERANDO. Quem lê é `job_morreu`; quem transforma morte em ERRO é `vigiar`. Portanto, “um escritor” corresponde ao serviço canônico, com várias funções, não a uma única instrução de escrita. (`app/relatorios/lote_espelho.py:66–79`, `:132–138`, `:149–162`.)

   Na porta do export, hoje existe **carimbo em modelo de banco, `MetricaSnapshot`**. Não é arquivo nem registro de execução com PID/estado. O command mede e chama `lavrar`; o TXT chama `conferir`. (`app/inteligencia/models.py:87–105`; `app/folha/porta_export.py:768–808`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–92`; `app/folha/views.py:128–133`.)

2. **Colisão:** o desenho pode cumprir L-007 se início, resultado, morte e repetição passarem pelo mesmo serviço canônico, com tripwire e trilha. Distribuir essas escritas entre view, command e vigia sem essa porta violaria a regra. O serviço e a proteção específicos do export permanecem **[nao verificado]**. (`app/docs/LEIS.md:65`; `CLAUDE.md:32–34`.)

3. **Literal:** falta identificar qual registro será criado e quem observará sua morte, em qual acesso ou periodicidade. No padrão citado, a observação ocorre na tela/poll e antes de deduplicar novo pedido; não é uma conversão espontânea do banco. (`app/relatorios/views.py:1270–1291`; `app/relatorios/lote_espelho.py:93–98`; `app/templates/relatorios/_lote_espelho_progresso.html:12–14`.)

   Também falta fechar disparos simultâneos e conclusão atrasada de uma tentativa anterior. O padrão tem constraint parcial por pedido em curso; o snapshot do export tem apenas unicidade empresa/dia, sem tentativa ou posse da execução. (`app/relatorios/models.py:167–171`; `app/relatorios/lote_espelho.py:101–113`; `app/inteligencia/models.py:102–105`.)

4. **Exportada, dinheiro, `!`:** controle de execução não demonstra alteração monetária; a escrita atual mostrada é de snapshot. Não autoriza tocar gravado exportado, motor, escala/vínculo ou regra fora do pedido. (`app/folha/porta_export.py:772–796`; `CLAUDE.md:265–268`.)

**(b) Resultado por empresa e competência; zero, divergente N ou falhou/motivo**

1. **Premissas:** a leitura exige empresa, competência e dia, mas a persistência **não conserva um registro distinto por empresa e competência no mesmo dia**. A chave é `chave/escopo/escopo_id/data_ref`; mês/ano ficam em `detalhe`. Consequência: medir outra competência da mesma empresa atualiza a mesma linha, e a competência anterior passa a ser recusada por incompatibilidade. (`app/inteligencia/models.py:102–105`; `app/folha/porta_export.py:772–776`, `:807–817`.)

   O valor atual é zero, positivo ou `-1` quando `falhas=None`. O motivo `erro_cartao_x_txt` existe no retorno de `medir`, mas não integra o JSON gravado por `lavrar`, embora `conferir` tente lê-lo. (`app/folha/porta_export.py:737`, `:764`, `:774–796`, `:818–820`.)

   A crítica ao código de saída confere: erro de medição, ausência de medição e divergência podem produzir `SystemExit(2)`. A leitura do TXT atual já usa o carimbo, não esse código. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88–95`, `:178–181`; `app/folha/porta_export.py:807–821`.)

2. **Colisão:** persistir o resultado sem reinterpretar a medição preserva a autoridade existente. Recalcular N no novo job ou no contador criaria outra conta. O registro vivo aponta `classificar_export` e `prontidao` como autoridades de fechamento. (`CLAUDE.md:20–22`, `:35–36`; `app/core/juizes.py:319–331`.)

3. **Literal:** é necessário distinguir resultado da tentativa atual de um verde anterior. Hoje uma exceção em `medir` acontece antes de `lavrar`; portanto pode deixar intacto um carimbo verde do mesmo dia. Sem vínculo entre tentativa e resultado, o novo FALHOU pode coexistir com uma prova antiga que ainda libera o TXT. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–92`; `app/folha/porta_export.py:807–821`, `:867`.)

   O dia também precisa permanecer explícito: `lavrar` obtém a data local **ao gravar**. O carimbo de ontem não é apagado por esse fluxo, mas não libera hoje; a leitura filtra somente hoje. Uma execução atravessando meia-noite recebe o dia da conclusão na forma atual. (`app/folha/porta_export.py:21–23`, `:771–776`, `:803–813`.)

4. **Exportada, dinheiro, `!`:** N deve conservar a composição atual. Os contadores próprios de exportada e linha/badge declarados fora de `falhas` não podem virar novos bloqueadores por esta mudança de registro. (`app/folha/porta_export.py:687–691`, `:705–712`, `:762–764`; `CLAUDE.md:265–268`.)

**(c) Texto de FALHOU e DIVERGENTE**

1. **Premissas:** esta cópia não possui enumeração operacional EM CONFERÊNCIA/PRONTA/DIVERGENTE/FALHOU na porta. `conferir` devolve uma mensagem ou string vazia; a view transmite a mensagem ao envelope de resposta. (`app/folha/porta_export.py:800–867`; `app/folha/views.py:128–133`.)

   Os textos atuais, com os placeholders preservados, são:

   | Condição atual | Texto retornado |
   |---|---|
   | Sem carimbo hoje | `A PORTA DO EXPORT NAO FOI MEDIDA HOJE para %s. O TXT nao sai sem a prova de que a tela, o PDF, o fechamento e o TXT estao no mesmo numero. Rode: %s` (`app/folha/porta_export.py:811–813`.) |
   | Outra competência | `O carimbo da porta de hoje e da competencia %02d/%s, e o export pedido e %02d/%s. Rode: %s` (`app/folha/porta_export.py:815–817`.) |
   | Valor nulo ou negativo | `A porta do export NAO PUDE MEDIR (%s). Selo que nao mede nao libera. Rode: %s` — motivo ausente vira `motivo nao registrado`. (`app/folha/porta_export.py:818–820`.) |
   | Valor positivo | `TXT RECUSADO: os leitores discordam em %d ponto(s) no universo do TXT de %s. %s` — último `%s` contém a lista montada. (`app/folha/porta_export.py:822–866`.) |
   | Zero | `''`: libera esta guarda, sem mensagem PRONTA. (`app/folha/porta_export.py:867`; `app/folha/views.py:129–134`.) |

   A frase “O suporte Hasner ja foi avisado” não aparece nesse retorno. Sua troca incide sobre apresentação anterior/proposta cuja presença nesta cópia é **[nao verificado]**. O texto atual já fala em **pontos**, não colaboradores. (`app/folha/porta_export.py:865–866`.)

2. **Colisão:** “Avise o suporte Hasner.” elimina a promessa de notificação. Entretanto, a apresentação precisa continuar informando as divergências: a porta e seu teste exigem lista, não apenas aviso genérico. (`app/folha/porta_export.py:4`, `:822–866`; `app/folha/tests/test_porta_do_export.py:50–60`.)

3. **Literal:** o novo FALHOU solicitado é: “Nao foi possivel conferir os numeros agora. Tente de novo em alguns minutos. Se repetir, avise o suporte Hasner.” Sua implementação é **[nao verificado]**. Para “tente de novo” fechar, falta vincular o pedido à execução/repetição: o fluxo atual apenas devolve recusa com instrução de command. (`app/folha/porta_export.py:809–820`; `app/folha/views.py:128–133`.)

4. **Exportada, dinheiro, `!`:** trocar mensagem não altera o número por si; modificar simultaneamente os critérios de liberação ultrapassaria essa troca textual. A guarda atual bloqueia nulo/negativo/positivo e libera zero. (`app/folha/porta_export.py:818–821`, `:865–867`; `CLAUDE.md:37–38`.)

**(d) Motivo persistido, recusa crítica e contador no ESTADO**

1. **Premissas:** `core/recusa_critica` é módulo de log e leitura, não modelo nem emissão de nível CRITICAL. Sua porta `registrar(...)` emite **WARNING**, com campos fixos, para o logger `ponto.recusa_critica`; a configuração encaminha-o a `/app/logs/ponto.log`. (`app/core/recusa_critica.py:30–46`, `:66–82`; `app/config/settings/saas.py:160–181`.)

   A afirmação anterior “o erro não fica gravado” precisa ser delimitada: **a recusa do POST já gera linha** pelo caminho `gerar_dominio_txt → resposta_acao → registrar`. Isso registra a mensagem da recusa; não persiste o motivo interno perdido por `lavrar` nem comprova aviso ao suporte. (`app/folha/views.py:128–133`; `app/core/respostas.py:50–56`; `app/folha/porta_export.py:776–796`, `:818–820`.)

2. **Colisão:** usar esse emissor conserva o escritor existente da recusa. A trilha exigida por L-007 ainda precisa conservar solicitante, antes/depois e motivo: o formato atual não possui campos separados de antes/depois, empresa, competência ou tentativa. (`app/docs/LEIS.md:65`; `app/core/recusa_critica.py:66–80`.)

   O contador deve ler uma fonte canônica. O lookup `JUIZES.md` declara que o registro vivo manda; ele próprio ainda diz “o que bloqueia… SEM juiz”, enquanto o registro vivo já aponta `classificar_export`. Estado operacional não demonstra nascimento de juiz; registrar nova autoridade exige a frase literal da TRAVA JUIZ-NOVO. (`app/docs/JUIZES.md:7–15`, `:118–121`; `app/core/juizes.py:319–331`; `CLAUDE.md:74–76`.)

3. **Literal:** o ESTADO atual importa `bin/recusa_estado.py`, que chama o contador específico **Resolver dia recusado hoje**. Ele filtra a porta `veredito_celula`; não é o contador do export. (`bin/gerar_estado.py:262–266`, `:333–335`; `app/core/recusa_critica.py:35–37`, `:122–125`.)

   Faltam quatro definições:

   - **Unidade:** `contar` soma linhas, sem deduplicar job/empresa/competência e sem filtrar estado ou motivo. É necessário escolher eventos/tentativas ou pares com resultado ruim; falhou e depois passou não têm o mesmo significado nessas duas contas. (`app/core/recusa_critica.py:104–119`.)
   - **Emissão:** registrar resultado ruim no job e registrar novamente cada POST recusado multiplicaria linhas; o envelope possui `recusas=()` para evitar nova emissão quando já gravada. A ligação concreta do job permanece **[nao verificado]**. (`app/core/respostas.py:41–56`.)
   - **“Hoje”:** o leitor compara a data do **prefixo do log**, não `dia=`. O host usa `date.today()`; a API usa `timezone.localdate()`. Django configura São Paulo; igualdade dos fusos efetivos de host/emissor/container é **[nao verificado]**. (`app/core/recusa_critica.py:42–46`, `:109–115`; `bin/recusa_estado.py:37–45`; `app/api/views_mensageria.py:1864–1872`; `app/config/settings/base.py:93–95`.)
   - **Observação indisponível:** o precedente distingue “sem arquivo” de zero. O contador atual declara não ter esperado zero; o novo esperado zero precisa pertencer à nova medida. (`bin/recusa_estado.py:11–12`, `:41–54`.)

4. **Exportada, dinheiro, `!`:** o emissor escreve log e o contador lê arquivos; esses corpos não escrevem folha monetária. Efeitos adicionais da implementação futura são **[nao verificado]**. (`app/core/recusa_critica.py:66–82`, `:91–125`.)

**R7 — processo morto vira FALHOU**

1. **Premissas:** o padrão distingue processo inexistente por `ProcessLookupError`. PID ausente retorna falso; `PermissionError` significa vivo; não há comparação de identidade além do número do PID. Portanto a checagem não cobre literalmente qualquer job abandonado nem detecta reaproveitamento do PID. (`app/relatorios/models.py:151–161`.)
2. **Colisão:** a transição precisa passar pelo escritor canônico do novo estado. Não há necessidade demonstrada de novo juiz de negócio; há necessidade de cumprir L-007. (`CLAUDE.md:32–34`, `:74–76`.)
3. **Literal:** falta dizer quem executará a checagem do export e quando, inclusive sem tela aberta, durante a carência, com PID nulo e após reinício. No padrão citado, tela/poll e novo pedido acionam `vigiar`; sua conexão com o export é **[nao verificado]**. (`app/relatorios/views.py:1270–1291`; `app/relatorios/lote_espelho.py:93–98`.)
4. **Exportada, dinheiro, `!`:** detectar morte deve apenas encerrar aquela tentativa e impedir liberação sem prova válida; não autoriza corrigir os dados medidos. A guarda atual exige carimbo de hoje/mesma competência e bloqueia medição negativa. (`app/folha/porta_export.py:803–820`; `CLAUDE.md:265–268`.)

**R8 — uma empresa falha e outra passa no mesmo disparo**

1. **Premissas:** isso não é garantido pelo command atual. Ele percorre empresas ativas em ordem e aborta no primeiro erro. Empresas anteriores podem ter carimbo; posteriores não são medidas. Quando `cartao_x_txt=None`, grava negativo antes de abortar. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–95`.)
2. **Colisão:** separar execução por empresa é compatível com preservar a medição; transformar resultado global em resultado local altera seu universo. `cartao_x_txt` mede a competência inteira e integra o resultado de cada empresa. (`app/folha/porta_export.py:723–727`, `:753–764`; `CLAUDE.md:35–38`.)
3. **Literal:** R8 fecha para falha própria de uma execução, deixando a outra ser concluída e registrada. Não pode prometer que outra empresa ficará verde diante de divergência global de cartão/TXT. Também precisa cobrir a empresa que falha **antes** da que passa, pois hoje essa ordem interrompe o disparo. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–95`; `app/folha/porta_export.py:753–764`.)
4. **Exportada, dinheiro, `!`:** isolamento de falhas não autoriza retirar divergência global de `falhas`, mudar tolerância ou mover os contadores de exportada para dentro do bloqueio. (`app/folha/porta_export.py:705–712`, `:762–764`; `CLAUDE.md:179`, `:265–268`.)

**Escritores e leitores dos campos/valores envolvidos**

| Campo/valor | QUEM ESCREVE/produz | QUEM LÊ |
|---|---|---|
| Empresa, mês, ano solicitados | POST fornece; view sanitiza. Command recebe mês/ano e seleciona empresas ativas. (`app/folha/views.py:109–120`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27–35`, `:86`.) | `medir`, `lavrar` e `conferir`. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88–92`; `app/folha/views.py:129`.) |
| `chave='porta_export_leitores'`, `escopo='empresa'`, `escopo_id`, `data_ref`, `data_ini` | `lavrar`; dia obtido por `_hoje`. (`app/folha/porta_export.py:18–23`, `:771–775`.) | `conferir` filtra chave/escopo/empresa/data; não lê `data_ini`. (`app/folha/porta_export.py:807–808`.) |
| `detalhe.mes`, `detalhe.ano` | `lavrar`. (`app/folha/porta_export.py:776`.) | `conferir` compara com o pedido. (`app/folha/porta_export.py:814–817`.) |
| N: `falhas_proprias`, `falhas_da_competencia`, `falhas` | `medir`; `lavrar` persiste somente o escalar em `valor`, com ausência convertida em `-1`. (`app/folha/porta_export.py:687–691`, `:762–764`, `:774`.) | Command agrega as partes; `conferir` lê `valor` e formata N. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140–145`, `:178–181`; `app/folha/porta_export.py:818–821`, `:865–866`.) |
| Listas `tela_x_pdf`, `topo_x_coluna`, `colabs_no_builder`, `calendario_x_espelho`, `minuto_em_duas_rubricas`; `cartao_x_txt`; HE pendente/trava/dias | `medir` produz; `lavrar` persiste. (`app/folha/porta_export.py:692–695`, `:745–752`, `:777–796`.) | Command conta/imprime; `conferir` monta a lista da recusa. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:96–107`, `:153–155`; `app/folha/porta_export.py:823–864`.) |
| Outros detalhes lavrados: `universo`, `fechamento_x_soma_dias_pagos`, `espelho_x_dia_pago`, `espelho_x_dia_pago_nao_medido`, `colabs_sem_lavratura`, `lavratura_sem_furo_apurado`, `linha_x_badge_divergentes`, `linha_x_badge_na_exportada`, `linha_x_badge_nao_medido`, `dias_em_aberto` | `lavrar` copia do resultado. (`app/folha/porta_export.py:776–790`.) | Command lê o retorno vivo para publicação; esses campos não compõem a mensagem de `conferir`. Outros leitores desses detalhes persistidos: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:96–174`; `app/folha/porta_export.py:814–867`.) |
| `erro_cartao_x_txt` | `medir` captura texto da exceção, limitado a 160 caracteres; devolve no resultado, sem persistência por `lavrar`. (`app/folha/porta_export.py:664–675`, `:737`, `:776–796`.) | `conferir` tenta ler do JSON e usa fallback; command também publica textos do retorno. (`app/folha/porta_export.py:820`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:102–103`, `:164–166`.) |
| `calculado_em` | Modelo, `auto_now=True`. (`app/inteligencia/models.py:96`.) | Não é consultado por `conferir`; leitor específico dessa hora na nova tela: **[nao verificado]**. (`app/folha/porta_export.py:807–867`.) |
| PID/início do padrão citado | `disparar` e `assumir`, no serviço `lote_espelho`. (`app/relatorios/lote_espelho.py:138`, `:161–162`.) | `LoteEspelho.job_morreu`. (`app/relatorios/models.py:151–156`.) |
| Estado, motivo, fim e limpeza do PID no padrão | `vigiar`, `falhar`, `executar`. (`app/relatorios/lote_espelho.py:75–78`, `:251–253`, `:264–266`.) | Tela/poll do lote. (`app/relatorios/views.py:1272–1279`; `app/templates/relatorios/_lote_espelho_progresso.html:18–41`.) |
| Solicitante do padrão, `criado_por` | `pedir` recebe `user` e grava. (`app/relatorios/lote_espelho.py:83`, `:103–106`.) | Serviço usa como ator dos eventos de disparo/resultado/falha. (`app/relatorios/lote_espelho.py:140–145`, `:254–257`, `:267–270`.) |
| Novos estado/PID/tentativa/resultado/motivo por empresa–competência do export | Serviço, modelo e transições: **[nao verificado]**. | Tela, vigia e vínculo com `conferir`: **[nao verificado]**. |
| Mensagem atual da porta | `conferir`. (`app/folha/porta_export.py:811–867`.) | View → `resposta_acao` → toast/mensagens; template mostra mensagens. (`app/folha/views.py:128–133`; `app/core/respostas.py:63–71`; `app/templates/folha/exportar.html:10`.) |
| Recusa: `usuario`, `porta`, `objeto`, `colab`, `dia`, `tipo`, `motivo` | `registrar`: extrai usuário/rota, recebe complementos e normaliza; envelopes o chamam. Sem usuário, sai `-`. (`app/core/recusa_critica.py:54–81`; `app/core/respostas.py:54–56`, `:78–80`.) | `ler_linha` extrai todos; `contar` usa data/porta e agrupa usuário, sem usar objeto/dia/estado para deduplicar. (`app/core/recusa_critica.py:85–88`, `:104–119`.) |
| Data/hora prefixada na linha | Formatter `full` e handler `pontofile`. (`app/config/settings/saas.py:122`, `:160–166`.) | Regex captura a data; contador compara ao dia alvo. (`app/core/recusa_critica.py:42–46`, `:109–114`.) |
| Contagem atual, rótulo e dia alvo do ESTADO | `contar`/`resolver_dia_recusado` produzem em memória; host fornece `date.today()`. (`app/core/recusa_critica.py:117–125`; `bin/recusa_estado.py:37–45`.) | `recusa_estado.linha` e `gerar_estado`. (`bin/recusa_estado.py:48–54`; `bin/gerar_estado.py:265–266`.) |
| Novo N “porta do export falhou ou divergiu hoje”, esperado 0 | Fonte, unidade, filtro, deduplicação e compositor: **[nao verificado]**. | Integração ao ESTADO: **[nao verificado]**. |

VEREDITO item 2: PROCEDE

## 17/11 11:55 · sessao cbdde1e84f94 · RESPOSTAS-1150 · leitura · origem=Code

`Todas.tsv:N` abaixo indica a linha do anexo fornecido, mantendo a emenda na linha 41. (todas.tsv:41)

| grupo | quantas | lei ou ! | censo fechado |
|---|---:|---:|---:|
| E4c (todas.tsv:71) | 1 | 1 | 1 |
| E4d e CLASSE-D (todas.tsv:1–80) | 0 | 0 | 0 |
| contrato 20 (todas.tsv:40,60,63,64,68) | 5 | 1 | 1 |
| rabos da E3 (todas.tsv:79,80) | 2 | 1 | 0 |
| O48 (todas.tsv:59,74) | 2 | 1 | 0 |
| O54 (todas.tsv:36,52,58,66) | 4 | 2 | 0 |
| calendario e app (todas.tsv:1–80) | 0 | 0 | 0 |
| outras (todas.tsv:1–75) | 41 | 10 | 9 |

IDs por grupo:

- **E4c:** FIO-E0-V4-E4-ESCRITORES. (todas.tsv:71)
- **E4d e CLASSE-D:** nenhum. (todas.tsv:1–80)
- **contrato 20:** CHAMADO-EM-RAIA; REABRIR-PERGUNTA-A-PREMISSA; LEGADO-51-ACESAS; DIA-DA-BATIDA-JUIZ-UNICO; TIPO-NA-FONTE. (todas.tsv:40,60,63,64,68)
- **rabos da E3:** E3-CADASTRO-ADMIN; E3-ENTRADA-ORFA-SEM-TURNO. (todas.tsv:79,80)
- **O48:** RETIFICACAO-DA-10-8069-PRIMEIRO; RETRATO-POR-EVENTO. Normalizei o grupo `folha-online (O48)` para **O48**, pelo identificador explícito da coluna. (todas.tsv:59,74)
- **O54:** O54; CONCORDE-PREVISTO-ZERO; O327; R1-ESTRUTURA-91. Normalizei `espelho-verdade (O54)` para **O54**, pelo identificador explícito da coluna. (todas.tsv:36,52,58,66)
- **calendario e app:** nenhum. (todas.tsv:1–80)
- **outras**, preservando a coluna:
  - PERGUNTA-SEM-SELO; C6-TETO-MARCO-PENDENTE; PASSIVO-VALIDADO-K6; PROGRAMA-CELULA-E-O-DIA. (todas.tsv:1,6,7,8)
  - A2-RAISE-NO-FUROS; O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL; E1-PERGUNTA-AO-DP-CONTADOR; V-FASE-AS-OF-CADASTRO-SEM-LINHA. (todas.tsv:10,11,12,13)
  - BACKFILL-UNICO; ISENTO-PONTO-JUIZ-REGISTRADO; DIA-DA-PERGUNTA-E-O-TURNO; O261-VETO-NA-PORTA. (todas.tsv:16,17,19,20)
  - EXPIRACAO-DA-ABERTA-COPIA-ANTIGA; JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO; JUIZ-DE-DONO-DO-DIA; VINCULO-LINHA-DO-TEMPO. (todas.tsv:21,22,23,24)
  - PLACAR-ESTRUTURAL; L111-ORFAOS-DO-MOTOR; O219; O129. (todas.tsv:26,27,28,32)
  - O105; O44; O95; O92; O136. (todas.tsv:34,35,37,38,39)
  - O178; O179; O180; O183; O229; O216; SOBRA-NA-BORDA-O-QUE-FICOU. (todas.tsv:44,45,46,47,48,49,50)
  - FERIADO-DIA-CHAVE; LISTA-POR-COMPETENCIA; REGUA-DO-PERIODO-COMPLEMENTO-19-10; LOTE-VAZA-ENTRE-COLABS. (todas.tsv:55,56,57,61)
  - BATERIA-QUEIXAS-ADMIN; MOTOR-LE-O-TURNO; RAIZ-DA-DISPUTA; LAVRA-PREMATURA-CONCORDE; JV1-MERGE-FILA. (todas.tsv:67,69,72,73,75)

**Total: 55 obras; 16 com lei_ou_bang=S; 11 com censo=S.** (todas.tsv:1–80)

As 56 esperadas não se reproduzem: o anexo contém 80 obras, sendo 55 PENDENTE REAL, 18 FEITA, cinco SUPERADA/ABSORVIDA e duas SEM PROVA. Não há IDs duplicados nem variante da classe PENDENTE REAL. (todas.tsv:1–80)

A emenda separada é `TOTAL=40` seguido de **DOSSIES-DAS-CELULAS**, cuja classe é **SEM PROVA**, portanto não acrescenta pendente; a outra SEM PROVA é **O117**. O quadro do primeiro lote informa corretamente 27 pendentes; as demais linhas fornecem 28, somando 55. A causa da expectativa de 56 e eventual ID ausente são [nao verificado]. (todas.tsv:33,41; todas.tsv:1–80)

TOTAL PENDENTES REAIS: 55

## 17/11 11:55 · sessao 56f320519012 · CORTE-FOLHA-ESPELHO · leitura · origem=Code

**Não é possível fechar a classificação causal dos 31 no dia original com este recorte.** O TSV traz o resultado para o **dia da pergunta**, mas não traz a data efetivamente devolvida por `_data_furo_da_grade` na leitura original nem identifica os dois `ata_sem_lampadas`. A atribuição individual de (a)–(e) permanece **[nao verificado]**.

O código não permite reconstruir essa data apenas pelas perguntas: `_data_furo_da_grade` lê os contextos da disputa/chamado, busca a batida de origem e retorna seu dia do turno se a grade disser `trabalho`. A tarefa, por sua vez, seleciona perguntas por `data_evento == dia`. Portanto, encontrar ação em outro dia não comprova sozinho toda a condição **(d)**. Fontes: `app/chamados/services/disputa_emissao.py:54`, `app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:82`; `app/chamados/services/tarefa_do_dia.py:262`.

Há ainda uma diferença decisiva: **`nao_sei=True` e `passivo_sem_linha` podem coexistir com ação alcançável**. O código produz `passivo_sem_linha` nas instâncias, mas pode publicar dono e ação pelos verbos residuais; o contrato manda verificar `rota`. Fontes: `app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:665`, `app/chamados/services/tarefa_do_dia.py:698`, `app/chamados/services/tarefa_do_dia.py:712`.

No TSV transcrito, todas as 31 pessoas têm pelo menos uma pergunta viva cujo dia devolve porta para colaborador ou admin. Isso afasta “disputa sem pergunta viva” **nesse dump**, mas não determina a causa da leitura anterior de produção. A equivalência entre os dois retratos é **[nao verificado]**.

Dois cuidados individuais impedem classificações apressadas:

- **col727:** p21248 devolve `folga/nada`, mas p56610 devolve `admin/validar/porta_fila`; atribuir **(c)** à pessoa inteira é **[nao verificado]**.
- **col956:** p35170 está em `folga`, porém devolve `admin/validar/porta_fila`, apesar de `passivo_sem_linha`; atribuir **(b)** ou **(c)** apenas por esses campos é **[nao verificado]**. A tarefa admite exposição administrativa em dia sem trabalho: `app/chamados/services/tarefa_do_dia.py:267`, `app/chamados/services/tarefa_do_dia.py:288`.

**Contagem:** no dump, **0 pessoas sem pergunta viva**; para a causa histórica no dia original, **31 indeterminadas**. A distribuição dessas 31 entre (b), (c), (d) e (e), incluindo a identidade dos dois desconhecidos originais, é **[nao verificado]**. Não classifico falta de informação como “outra causa”.

As obras encontradas cobrem estas classes; sua aplicação individual depende da causa ainda não demonstrada:

| Classe | Obra e cobertura documentada |
|---|---|
| **(a) Nasceu sem pergunta** | **BO-DISPUTA-SEM-PERGUNTA**, estacionada como insumo da RAIZ por **BO-DISPUTA-DESENHO-RAIZ**. `app/docs/GRADE-MESTRA.md:1023`; `app/docs/BACKLOG.md:774`. |
| **(a) Ficou aberta depois de perder todas as vivas** | **DISPUTA-4418-ABERTA-COM-TUDO-RESOLVIDO**. `app/docs/GRADE-MESTRA.md:1027`. |
| **(b) Viva sem tarefa** | Obra específica para cada caso: **[nao verificado]**. `passivo_sem_linha` isolado não demonstra ausência de porta: `app/chamados/services/tarefa_do_dia.py:665`. |
| **(c) Folga/ausência de turno** | **DIA-SEM-TRABALHO-PORTA** cobre batida em folga/feriado; **E4A1-C6-X-PASSADO-SEM-TURNO** cobre a divergência descrita entre fábrica e C6. Aplicação aos casos: **[nao verificado]**. `app/docs/BACKLOG.md:1940`; `app/docs/BACKLOG.md:2148`. |
| **(d) Dia da disputa versus dias das perguntas** | **RAIZ-DA-DISPUTA**, com **RAIZ-DA-DISPUTA-F0-CASO-7279**, cujo escopo inclui a disputa absorver perguntas de outro dia. Cobertura de uma mudança específica no uso de `_data_furo_da_grade`: **[nao verificado]**. `app/docs/BACKLOG.md:2055`; `app/docs/BACKLOG.md:2670`; `app/docs/GRADE-MESTRA.md:108`. |
| **(e) `ata_sem_lampadas`** | Resultado produzido em `app/chamados/services/tarefa_do_dia.py:234` e `app/chamados/services/tarefa_do_dia.py:240`. Obra específica comprovada para os dois casos: **[nao verificado]**. |
| Problema do par do intervalo | **BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS** cobre validações isoladas das metades. Ter motivo de intervalo no TSV não comprova esse defeito **[nao verificado]**. `app/docs/BACKLOG.md:2178`; `app/docs/GRADE-MESTRA.md:1024`. |
| Divergência entre projeções e atos | **FIO-E0-V4-E4-ESCRITORES**, etapa **E4c LEITORES DIVERGE + selo 2**. Vincular cada `passivo_sem_linha` a essa etapa é **[nao verificado]**. `app/docs/BACKLOG.md:2035`; `app/docs/GRADE-MESTRA.md:107`. |

Na tabela final, **I** significa “causa no dia original indeterminada”; a pergunta citada é uma testemunha de ação **no dia dela**, conforme o TSV transcrito. **O?** significa “obra individual não determinada”, sem afirmar “sem obra”. As atribuições individuais permanecem **[nao verificado]**.

| Pessoa | Causa — evidência disponível | Obra |
|---|---|---|
| col193 | **I** — p33805, colaborador responde. [nao verificado] | O? |
| col238 | **I** — p49400, admin valida, mesmo com `nao_sei`. [nao verificado] | O? |
| col247 | **I** — p58549, admin valida. [nao verificado] | O? |
| col248 | **I** — p56220, admin valida. [nao verificado] | O? |
| col266 | **I** — p25096, colaborador responde. [nao verificado] | O? |
| col269 | **I** — p15586, colaborador responde. [nao verificado] | O? |
| col276 | **I** — p42790, admin resolve dia. [nao verificado] | O? |
| col439 | **I** — p53577, admin valida. [nao verificado] | O? |
| col478 | **I** — p45783, admin valida, mesmo com `nao_sei`. [nao verificado] | O? |
| col521 | **I** — p56881, colaborador responde. [nao verificado] | O? |
| col570 | **I** — p38697, admin resolve dia. [nao verificado] | O? |
| col727 | **I** — p21248 sem ação; p56610 com admin validando. [nao verificado] | O? |
| col850 | **I** — p58478, admin valida. [nao verificado] | O? |
| col854 | **I** — p49663, admin valida, mesmo com `nao_sei`. [nao verificado] | O? |
| col861 | **I** — p26860, colaborador responde. [nao verificado] | O? |
| col874 | **I** — p58368, admin valida. [nao verificado] | O? |
| col898 | **I** — p55561, admin valida. [nao verificado] | O? |
| col920 | **I** — p31695, colaborador responde, mesmo com `nao_sei`. [nao verificado] | O? |
| col930 | **I** — p26892, colaborador responde. [nao verificado] | O? |
| col956 | **I** — p35170, folga com admin validando. [nao verificado] | O? |
| col983 | **I** — p41303, colaborador responde. [nao verificado] | O? |
| col986 | **I** — p44878, colaborador responde. [nao verificado] | O? |
| col989 | **I** — p44114, colaborador responde. [nao verificado] | O? |
| col1008 | **I** — p56200, colaborador responde. [nao verificado] | O? |
| col1010 | **I** — p53353, colaborador responde. [nao verificado] | O? |
| col1018 | **I** — p54128, colaborador responde. [nao verificado] | O? |
| col1040 | **I** — p57635, admin valida. [nao verificado] | O? |
| col56 | **I** — p41903, colaborador responde. [nao verificado] | O? |
| col99 | **I** — p40911, colaborador responde. [nao verificado] | O? |
| col108 | **I** — p26995, colaborador responde. [nao verificado] | O? |
| col151 | **I** — p55742, admin valida. [nao verificado] | O? |

## 17/11 11:55 · sessao 158de115794e · - · - · origem=Ronald (tmux)

**Colisões e limites do item 2:**

- **L-007:** qualquer novo estado persistido precisa de um escritor canônico, tripwire e trilha da ação crítica; o aval não nomeia essa porta nem define suas transições. A regra está em `CLAUDE.md:32–34`, espelhada em `app/docs/LEIS.md:65`. Portanto a construção deverá fechar quem grava início, resultado e morte sem criar escritores concorrentes (`CLAUDE.md:32–34`).
- **Juiz único:** `JUIZES.md` é lookup, enquanto `core/juizes.py` é registro vivo (`app/docs/JUIZES.md:7–15`). O lookup ainda diz “o que bloqueia … SEM juiz” (`app/docs/JUIZES.md:118–121`), mas o registro vivo já aponta `classificar_export` para as perguntas de entrada no TXT e travas de aprovação (`app/core/juizes.py:319–331`). O estado operacional de execução pode ler a medição existente; transformar PID/contador/vigia em novo juiz de dinheiro ou prontidão contrariaria a regra de leitores e a regra de vigia (`CLAUDE.md:20–22`, `CLAUDE.md:170–173`).
- **TRAVA JUIZ-NOVO:** uma autoridade nova registrada exige a frase literal “corte Ronald: juiz `<nome>` nasce” no CORTES.md (`CLAUDE.md:74–76`). Não é possível afirmar que o item exige um juiz novo apenas por acrescentar estado operacional; eventual novo predicado de negócio ou domínio precisa ser explicitado antes de avaliar essa trava (`CLAUDE.md:74–76`, `CLAUDE.md:176–179`).
- **PORTA-DO-EXPORT-SEM-BECO:** texto e vigência literal desse aval não constam do dado raspado fornecido; a busca nos principais documentos da cópia não localizou seu nome. A comparação integral com ele permanece **[nao verificado]**. Há autorização histórica explícita para recusar TXT com leitores divergentes no módulo e na view (`app/folha/porta_export.py:2–15`, `app/folha/views.py:121–133`).

**Exportada, dinheiro e `!`:**

- A escrita atual dessa porta é o snapshot, com resultado e detalhes; sua leitura bloqueia a geração do TXT antes das etapas seguintes (`app/folha/porta_export.py:772–796`, `app/folha/views.py:128–133`). Só acrescentar controle de execução, motivo e apresentação não demonstra mudança em rubrica ou gravado monetário; a implementação futura permanece **[nao verificado]**.
- É fora desse item alterar a composição das divergências que bloqueiam: a soma está em `app/folha/porta_export.py:687–691` e `app/folha/porta_export.py:762–764`; divergências específicas de exportada e linha/badge estão expressamente fora de `falhas` (`app/folha/porta_export.py:705–712`).
- Mexer no gravado de exportada, escala/vínculo, motor ou regra fora do pedido exige `!`; “dinheiro não se moveu” não amplia o aval (`CLAUDE.md:265–268`). Logo nenhum dos subitens nem R7/R8 autoriza essas intervenções (`CLAUDE.md:265–268`).

**Costura adicional para R8:**

O resultado operacional pode ser por empresa, mas nem todo insumo da divergência é independente: `cartao_x_txt` abrange a competência inteira, devolve o mesmo número por empresa e não aceita `--empresa`; `falhas` por empresa inclui esse número global (`app/folha/porta_export.py:723–727`, `app/folha/porta_export.py:753–764`). Assim, “uma empresa falha e outra passa” fecha para falha própria da execução; não promete verde para outra empresa quando existe divergência global de cartão/TXT (`app/folha/porta_export.py:723–727`, `app/folha/porta_export.py:753–764`).

**Texto atual de divergência:**

A frase já fala em pontos: `TXT RECUSADO: os leitores discordam em %d ponto(s) no universo do TXT de %s. %s`; não promete aviso ao suporte (`app/folha/porta_export.py:865–866`). A troca de “O suporte Hasner ja foi avisado” incide sobre uma apresentação proposta/anterior, cuja existência nesta cópia é **[nao verificado]**.

## 17/11 11:55 · sessao f34469953972 · - · - · origem=Ronald (tmux)

**2(d): o mecanismo existe; o vínculo da porta do export e a definição do contador ainda precisam fechar.** (`app/core/recusa_critica.py:66–82`; `bin/gerar_estado.py:262–266`.)

1. **Premissa:** `core/recusa_critica` é um módulo de emissão/leitura de log, não um modelo. A porta `registrar(request, *, motivo, tipo='erro', colab=None, dia=None, objeto=None, porta=None)` emite **WARNING**, não CRITICAL. Escreve uma linha `RECUSA-ACAO-CRITICA porta=... usuario=... colab=... dia=... objeto=... tipo=... motivo=...`. (`app/core/recusa_critica.py:30–46`, `:66–82`.) O destino configurado é `/app/logs/ponto.log`, com três rotados; `ponto` usa esse handler sem propagação. (`app/config/settings/saas.py:160–181`.)

   Os envelopes `resposta_acao` e `resposta_render` chamam essa porta para `erro`/`aviso`; também aceitam lista de recusas e `recusas=()` para evitar dupla emissão. (`app/core/recusa_critica.py:32–34`; `app/core/respostas.py:41–56`, `:74–80`.) A emissão efetiva pela futura falha/divergência do export e sua ligação ao registro do job são [nao verificado].

2. **Colisão:** usar a porta existente é coerente com “um escritor”; os novos campos/estados do job precisam escritor canônico e tripwire, e a ação crítica pede usuário, antes/depois e motivo. Apenas uma linha de recusa não substitui automaticamente essa trilha: o formato atual não possui campos separados `antes`/`depois`, empresa ou competência. (`CLAUDE.md:32–34`; `app/docs/LEIS.md:65`; `app/core/recusa_critica.py:76–80`.) O contador deve ler a fonte real, com rótulo/universo coincidentes. (`CLAUDE.md:35–36`.) Nova autoridade registrada em `core/juizes.py` exigiria a frase de nascimento no CORTES; o item fornecido não comprova esse nascimento [nao verificado]. (`CLAUDE.md:74–76`; `app/docs/LEIS.md:69`.)

3. **Literal não fechado:**
   - O leitor existente conta **linhas**, sem deduplicação por job, tentativa, empresa, competência ou estado; só compara data do **prefixo do log** e, opcionalmente, porta. Não usa o campo `dia=` para definir “hoje”. (`app/core/recusa_critica.py:104–119`.) Logo é preciso dizer se o novo `N` soma eventos/tentativas ou pares empresa–competência, e como trata retentativas e FALHOU→DIVERGENTE [nao verificado].
   - O contador atual do ESTADO é especificamente “Resolver dia recusado hoje”: filtra `veredito_celula`, não export; ESTADO importa esse leitor. (`app/core/recusa_critica.py:35–37`, `:122–126`; `bin/gerar_estado.py:262–266`, `:333–335`.) A nova linha “porta do export falhou ou divergiu hoje: N”, esperado 0, é [nao verificado].
   - No host, `bin/recusa_estado.py` fornece `datetime.date.today()`; no endpoint, fornece `timezone.localdate()`. O Django configura São Paulo, mas igualdade do relógio/fuso efetivo do host, processo emissor e container é [nao verificado]. (`bin/recusa_estado.py:37–45`; `app/api/views_mensageria.py:1864–1872`; `app/config/settings/base.py:93–95`.)
   - `registrar` aceita `request=None` por usar `getattr`, mas nesse caso o usuário sai `-`; se o job não tem request, é necessário definir como conservar solicitante e identidade empresa/competência/tentativa. A API permite `porta` e `objeto` explícitos; a definição desses valores é [nao verificado]. (`app/core/recusa_critica.py:54–80`.)
   - O precedente distingue ausência de arquivo de zero; não convém converter observação indisponível em “esperado 0 atingido”. (`bin/recusa_estado.py:41–54`.) Seu contador existente declara **não** ter esperado 0; portanto o novo esperado deve pertencer ao novo contador. (`bin/recusa_estado.py:11–12`.)

4. **Exportada/dinheiro/`!`:** a porta de recusa mostrada escreve log, e seu contador lê arquivos; não há nesses corpos operação de exportação nem cálculo monetário. (`app/core/recusa_critica.py:66–82`, `:91–125`.) Efeitos da implementação futura fora desses corpos são [nao verificado].

**Escritores/leitores do padrão disponível:**

| Dado | Quem escreve/produz | Quem lê |
|---|---|---|
| `usuario` | `registrar` extrai `request.user.pk/username`, ausente=`-`. (`app/core/recusa_critica.py:73–74`.) | Regex e contador retornam agrupamento por usuário. (`app/core/recusa_critica.py:42–46`, `:112–119`.) |
| `porta` | Argumento explícito, senão `resolver_match.view_name`/path. (`app/core/recusa_critica.py:54–57`, `:77`.) | `contar`, comparando último segmento após `:`. (`app/core/recusa_critica.py:115–116`.) |
| `objeto` | Argumento explícito, senão kwargs ordenados da rota. (`app/core/recusa_critica.py:60–63`, `:79`.) | `ler_linha` entrega no dict; contador atual não o usa. (`app/core/recusa_critica.py:85–88`, `:112–119`.) |
| `colab`, `dia` | Chamador informa; `registrar` normaliza pk/ISO; ausentes=`-`. (`app/core/recusa_critica.py:49–51`, `:75–79`.) | Regex extrai; contador atual não usa esses campos para filtro. (`app/core/recusa_critica.py:42–46`, `:112–119`.) |
| `tipo`, `motivo` | Chamador/envelopes fornecem; `registrar` reduz espaços e emite WARNING. (`app/core/respostas.py:54–56`, `:78–80`; `app/core/recusa_critica.py:79–81`.) | `ler_linha`; contador atual conta sem filtrar `tipo`/motivo. (`app/core/recusa_critica.py:85–88`, `:112–119`.) |
| Data prefixada/hora e arquivo | Formatter `full` e handler `pontofile`. (`app/config/settings/saas.py:122`, `:160–166`.) | `ler_linha` captura data; `contar` compara ao alvo. (`app/core/recusa_critica.py:42–46`, `:109–114`.) |
| Dia alvo | Host: `date.today()`; API: `timezone.localdate()`. (`bin/recusa_estado.py:41`; `app/api/views_mensageria.py:1870`.) | `resolver_dia_recusado` → `contar`. (`app/core/recusa_critica.py:122–125`.) |
| `recusas`, `por_usuario`, rótulo | Leitor `contar` e wrapper produzem em memória. (`app/core/recusa_critica.py:117–125`.) | `bin/recusa_estado.py:48–54`; ESTADO importa em `bin/gerar_estado.py:265–266`; API publica em `app/api/views_mensageria.py:1870–1873`. |
| Empresa/competência/tentativa/motivo do novo job; novo `N` export | Escritores e leitores propostos [nao verificado]. | [nao verificado]. |

## 17/11 11:55 · sessao fe5f1741355d · - · - · origem=Ronald (tmux)

(a)/R7: o padrão citado existe, mas pertence a `LoteEspelho`, com `estado`, `erro`, `pid`, `iniciado_em` e `concluido_em`; não é um job da porta do export. (`app/relatorios/models.py:71`, `:118–132`.)

| Dado | Quem escreve | Quem lê |
|---|---|---|
| Empresa, competência, filtros, chave | `lote_espelho.pedir`, com criação protegida pela constraint parcial | `pedir` para dedup e `executar` para gerar. (`app/relatorios/lote_espelho.py:92–113`, `:200–207`; `app/relatorios/models.py:167–171`.) |
| PID/início | `disparar` grava `Popen.pid`; `assumir` confirma `os.getpid()`, início e GERANDO | `LoteEspelho.job_morreu`. (`app/relatorios/lote_espelho.py:132–138`, `:161–162`; `app/relatorios/models.py:151–156`.) |
| Morte/estado ERRO/motivo/fim/PID limpo | `vigiar`; exceção tratada por `falhar` | Tela lê `estado`, `erro`, progresso e placar. (`app/relatorios/lote_espelho.py:66–79`, `:261–270`; `app/relatorios/views.py:1272–1279`; `app/templates/relatorios/_lote_espelho_progresso.html:18–41`.) |
| Conclusão/placar/fim/PID limpo | `executar` | Tela do lote. (`app/relatorios/lote_espelho.py:239–253`; `app/relatorios/views.py:1276–1279`.) |

O método só acusa morte com estado em curso, PID preenchido e depois de carência de 15 segundos; usa `os.kill(pid, 0)`, considera `ProcessLookupError` morte e `PermissionError` vivo. PID ausente não vira FALHOU; identidade além do número do PID não é conferida. (`app/relatorios/models.py:151–161`.)

Quem observa é `vigiar`, chamado na tela/poll e antes da dedup de um novo pedido; o template consulta a cada dois segundos enquanto em curso. Sem acesso/pedido, essa conversão não ocorre por esses caminhos. Ligação da porta export a esse vigia: [nao verificado]. (`app/relatorios/views.py:1270–1291`; `app/relatorios/lote_espelho.py:96–98`; `app/templates/relatorios/_lote_espelho_progresso.html:12–14`.)

Hoje o registro da porta é **carimbo em modelo `MetricaSnapshot`**, não arquivo e não registro de execução com PID/estado/tentativa. `lavrar` escreve; `conferir` lê; a view do TXT lê a mensagem e recusa se não vazia. (`app/folha/porta_export.py:768–808`; `app/inteligencia/models.py:87–105`; `app/folha/views.py:128–133`.)

A competência está somente em `detalhe`; a chave única é chave/escopo/empresa/dia. Portanto duas competências da mesma empresa no mesmo dia sobrescrevem o mesmo carimbo; a leitura recusa se a competência pedida divergir da última gravada. (`app/folha/porta_export.py:772–776`, `:815–817`; `app/inteligencia/models.py:102–105`.)

O dia é `timezone.localdate()` no momento da gravação; ontem permanece como outro snapshot, mas `conferir` só lê hoje. Não há estado “ontem”: ausência de carimbo hoje segue o ramo “NAO FOI MEDIDA HOJE”. (`app/folha/porta_export.py:21–23`, `:771–776`, `:803–813`.)

Concorrência ainda precisa de decisão concreta: a gravação atual não carrega tentativa, claim ou PID; duas medições podem disputar a mesma chave empresa/dia e o resultado não identifica qual execução o produziu. Uma medição que levanta exceção antes de `lavrar` também deixa eventual verde anterior do mesmo dia intacto. (`app/folha/porta_export.py:772–796`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–95`.)

R8 não fecha no command atual: ele percorre empresas ativas em ordem, grava cada resultado e aborta no primeiro erro; uma empresa anterior pode ter sido carimbada e as seguintes nem são medidas. `cartao_x_txt=None` grava negativo antes de abortar. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–95`.)

Separar resultados por empresa não autoriza reinterpretar `cartao_x_txt`: seu universo continua sendo a competência inteira, repetido por empresa. (`app/folha/porta_export.py:723–727`, `:753–764`.)

## 17/11 11:54 · sessao f0a7ca579d21 · E4C-F2 · verificador · origem=Code

**REPROVADO — a idempotência concorrente não fecha:** duas chamadas podem selecionar a mesma pergunta antes da trava e depois gravar duas REABERTURAS compensadoras. O RED_4 verifica chamadas sequenciais. (`app/chamados/models.py:1615`, `app/chamados/models.py:1622`, `app/chamados/models.py:1634`, `app/chamados/models.py:1681`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:184`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:189`)

1. **Triagem da volta 1**

| Item | Conferência |
|---|---|
| **1 — ato futuro** | **Descarte limitado ao retrato fornecido.** A medida informada afasta ocorrência naquele estoque; não corrige a possibilidade na porta. O compensador continua usando `now()`, enquanto a leitura ordena também atos futuros. Reprodução/ocorrência além do dado fornecido: **[nao verificado]**. (`app/chamados/models.py:1664`, `app/chamados/models.py:1676`, `app/chamados/models.py:4279`, `app/chamados/models.py:4288`) |
| **2 — motivo decide validação** | **Cura semântica procede.** Python e SQL passaram a consultar o campo explícito. A afirmação sobre cobertura completa do tripwire precisa ser restringida: ele só reconhece certos `dict(...)`. (`app/chamados/models.py:3532`, `app/chamados/models.py:4266`; `app/chamados/tests/test_f2_resposta_mantem_validacao.py:35`) |
| **3 — golden facultativo/vazio** | **Cura procede para esse achado.** Arquivo obrigatório, verbos não vazios, CSF com P4 e comparação das tarefas completas. Geração efetiva na main e conservação sobre a frota inteira: **[nao verificado]**. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:150`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:153`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:155`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:159`) |
| **4 — toda selecionada recebe R/idempotência** | **Cura da cobertura procede; idempotência apenas sequencial.** O `continue` foi retirado. Falta revalidar o universo sob a trava para impedir duplicação concorrente. (`app/chamados/models.py:1667`, `app/chamados/models.py:1675`, `app/chamados/models.py:1681`) |
| **5 — N+1 no explicador/vigia** | **Descarte como regressão deste delta procede pelo diff fornecido.** Os caminhos continuam lendo `estado` em laço; a mudança do app acrescenta prefetch em outro caminho. Conferência independente do ancestral: **[nao verificado]**. (`app/chamados/services/explicador.py:95`, `app/chamados/services/explicador.py:160`, `app/chamados/services/explicador.py:171`; `app/chamados/services/vigia_fio.py:259`; `app/chamados/services/pergunta_no_app.py:58`) |
| **6 — dono descrito muda** | **Descarte como mudança da fila oficial procede; a mudança descritiva permanece.** PENDENTE produz dono colaborador; o explicador declara essa lista descritiva. A tarefa oficial exclui a compensadora. (`app/chamados/services/explicador.py:96`, `app/chamados/services/explicador.py:174`; `app/chamados/services/tarefa_do_dia.py:426`) |
| **7 — autor da foto no empate** | **Cura procede no caminho atual.** O laço conserva o último autor, com ordenação padrão por instante/pk. (`app/chamados/models.py:1672`, `app/chamados/models.py:1674`, `app/chamados/models.py:4175`) |
| **8–11 — conferência sem defeito** | **Não ratifico em bloco.** Atomicidade das escritas e ordem das travas procedem; a idempotência concorrente e a abrangência do tripwire exigem ressalvas. (`app/chamados/models.py:1629`, `app/chamados/models.py:1634`, `app/chamados/models.py:1683`; `app/chamados/tests/test_f2_resposta_mantem_validacao.py:35`) |

2. **Quem escreve e quem lê `resposta_mantem_validacao`**

Os quatro montadores de RESPOSTA encontrados declaram o campo explicitamente: (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:15`)

| Montador | Valor |
|---|---|
| `responder_pergunta_disputa_core` | `False`. (`app/chamados/services/disputa_emissao.py:1525`) |
| `responder_automaticamente` | `True`. (`app/chamados/services/disputa_emissao.py:1679`) |
| `carimbar_resposta_pela_trilha` | `True`. (`app/chamados/services/disputa_emissao.py:1701`) |
| `linhas_da_projecao_sem_linha` | `True`. (`app/chamados/models.py:4236`) |

O modelo fornece default `False`; a 0055 escreve `True` no histórico selecionado. A persistência dos montadores passa por `registrar`, que constrói `cls(**ln)` e executa `bulk_create`. (`app/chamados/models.py:4170`, `app/chamados/models.py:4313`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`)

Os backfills inspecionados reutilizam a fábrica da projeção: `backfill_decisoes_pergunta` consulta suas linhas; sua escrita própria é SILENCIO. `backfill_fio_morto_e4b` registra diretamente o resultado da fábrica. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

Os leitores operacionais são a regra Python, chamada pela leitura, e a face SQL. A foto E4b também captura o campo por enumerar os campos concretos. (`app/chamados/models.py:4266`, `app/chamados/models.py:4284`, `app/chamados/models.py:3532`; `app/chamados/services/foto_e4b.py:210`, `app/chamados/services/foto_e4b.py:280`)

**Não encontrei escritor concreto de produção omitindo o campo.** Fixtures de produção de `DecisaoPergunta`, ou escritores externos à cópia: **[nao verificado]**.

**O tripwire não enxerga todos os formatos solicitados.** Ele só visita chamadas ao nome literal `dict`, com `ato` reconhecível como `.RESPOSTA` ou `'resposta'`. Construtores, `objects.create`, `bulk_create([D(...)])`, dicionários literais passados a `registrar` e montagem indireta escapam desse teste. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:23`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:35`)

O selo anterior fiscaliza construções e operações diretas pelo nome `DecisaoPergunta`, mas não exige o campo num dicionário literal entregue à porta autorizada. Falta ampliar a fiscalização das entradas de RESPOSTA; `registrar` também não recusa campo omitido. (`app/chamados/tests/test_e1_entradas.py:196`, `app/chamados/tests/test_e1_entradas.py:198`; `app/chamados/models.py:4303`, `app/chamados/models.py:4313`)

3. **Semântica e migration 0055**

O campo agora tem significado único: dizer se aquela RESPOSTA conserva a validação vigente. O motivo deixou de decidir isso em Python e SQL. O leitor decisório de motivo encontrado em `materializacao.py` restringe-se a **DEVOLUCAO BUG-PAR**, não RESPOSTA. (`app/chamados/models.py:4266`, `app/chamados/models.py:3532`; `app/chamados/services/materializacao.py:275`)

A conservação histórica procede **no domínio permitido pelo esquema**: (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:25`)

| Motivo antigo | Resultado da 0055 |
|---|---|
| `''` | Continua `False`: RESPOSTA desfaz validação. |
| `' '` ou outro texto não vazio | Recebe `True`: conserva validação; não há trim. |
| `NULL` | Não pertence ao contrato da coluna, declarada sem `null=True`. Não cabe afirmar equivalência incondicional para estoque fora desse contrato. (`app/chamados/migrations/0051_decisao_pergunta.py:48`; `app/chamados/models.py:4147`) |

A sequência **AddField → RunPython → SET CONSTRAINTS ALL IMMEDIATE → AddConstraint** está corretamente ordenada para criar a coluna, preencher e drenar eventos diferidos antes do DDL seguinte; repete o mecanismo documentado na 0051. Aplicação efetiva no PostgreSQL: **[nao verificado]**. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:22`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:27`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:29`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:30`; `app/chamados/migrations/0051_decisao_pergunta.py:102`)

4. **REABERTURA para toda selecionada e efeitos nos leitores**

Toda pergunta que chega ao universo da escrita recebe a compensadora, inclusive sem resposta e sem validação. (`app/chamados/models.py:1667`, `app/chamados/models.py:1675`, `app/chamados/models.py:1681`)

| Leitor | Efeito para a pergunta sem resposta nem validação |
|---|---|
| Leitura/face SQL | R continua sendo limpeza; o par vigente permanece vazio. (`app/chamados/models.py:3532`, `app/chamados/models.py:4296`, `app/chamados/models.py:4298`) |
| Histórico/modal | **Muda o que devolve:** R integra `ATOS_COM_FOTO` e entra em respostas anteriores mesmo com foto vazia; o modal mostra “sem texto”, reabertura, instante e motivo. (`app/chamados/models.py:4140`, `app/chamados/models.py:3782`; `app/templates/chamados/partials/modal_fio.html:212`) |
| Tarefa oficial | Descarta a marcada antes de montar a sequência. R comum continua tendo efeito de devolução. (`app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:163`) |
| Lista sem veredito/CSF | Exige resposta vigente pelo juiz; a R vazia não satisfaz essa condição. (`app/chamados/models.py:1985`, `app/chamados/models.py:1987`) |
| Auditor I18/I20 | Lê projeção; não conta R nem detecta compensadora duplicada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1076`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1106`) |
| Cura de limbo | Seleciona pelo par projetado e conjunto nominal, sem contar R. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`) |
| Materialização | O reconhecimento BUG-PAR exige DEVOLUCAO; ausência rejeitada continua usando a reabertura comum. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:1167`) |
| Foto auditável E4b | Passa a incluir a nova decisão. (`app/chamados/services/foto_e4b.py:280`) |

Relatório adicional que conte R e altere resultado: **[nao verificado]**.

**Duas chamadas sequenciais não gravam duas R:** a primeira retira `materializada_em`, exigida pela seleção seguinte. É exatamente por isso que o RED_4 passa. (`app/chamados/models.py:1543`, `app/chamados/models.py:3460`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:184`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:189`)

**Duas chamadas concorrentes podem gravar duas R:** ambas selecionam os mesmos IDs; a segunda espera a trava, mas depois consulta apenas esses PKs, sem exigir materialização atual, e insere outra compensadora. Existe chamada pública sem `atomic` envolvendo toda essa sequência. (`app/chamados/models.py:1615`, `app/chamados/models.py:1622`, `app/chamados/models.py:1667`; `app/chamados/views_arquivar.py:51`, `app/chamados/views_arquivar.py:86`)

Falta reler/revalidar o universo sob a trava **antes das fotos e da R**, mantendo a ordem das aquisições, e acrescentar prova concorrente. Ocorrência em produção: **[nao verificado]**. (`app/chamados/models.py:1634`, `app/chamados/models.py:1635`, `app/chamados/models.py:1681`)

5. **Foto `_vpor`**

A cura procede no caminho atual: as fotos do passivo são persistidas antes da consulta nova; o prefetch sem `order_by` próprio herda `Meta.ordering = ['ato_em', 'pk']`; o laço sobrescreve o autor até a última VALIDACAO do instante vigente. A leitura aplica o mesmo desempate às linhas reais. (`app/chamados/models.py:1635`, `app/chamados/models.py:1668`, `app/chamados/models.py:1672`, `app/chamados/models.py:4175`, `app/chamados/models.py:4288`)

O RED_7 distingue os autores A/B no mesmo instante e exige B. Execução e quantidade real de empates: **[nao verificado]**. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:205`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:210`)

6. **Classes de reprovação e mordida dos testes** [nao verificado]

- **Segundo juiz:** não aparece uma autoridade concorrente nova nesses trechos. A marca identifica procedência; a tarefa continua na autoridade registrada; a validação tem leitura e face SQL explicitamente correspondentes. (`app/chamados/models.py:4196`, `app/chamados/models.py:3521`; `app/core/juizes.py:304`)
- **Segundo escritor:** a persistência permanece na porta; o censo da marca aponta a ressuscitação. A fiscalização de todas as formas de entrada de RESPOSTA continua incompleta, conforme item 2. (`app/chamados/models.py:4313`; `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:70`; `app/chamados/tests/test_f2_resposta_mantem_validacao.py:35`)
- **Relógio:** a leitura nova ocorre no escritor; a leitura da sequência não consulta relógio. (`app/chamados/models.py:1664`, `app/chamados/models.py:4282`)
- **Travas:** C → D → Ch → P antecede as escritas. O defeito encontrado é a seleção anterior à trava. (`app/chamados/models.py:1634`, `app/chamados/models.py:2834`, `app/chamados/models.py:2843`, `app/chamados/models.py:2856`, `app/chamados/models.py:2860`)
- **Mordida:** motivo/campo discordantes testam os dois sentidos contra leitura e SQL; RED(a), controle(c), RED_7 e teto do app têm expectativas substantivas. O RED_4 não cobre concorrência. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:51`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:453`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:480`; `app/api/tests/test_f2_teto_consultas_app.py:56`)
- **Golden vazio/ausente:** não passa silenciosamente nessas condições. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:150`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:154`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:155`)
- **Pouso:** o deploy oficial migra antes do HUP, com selo de reciclagem antes da migration. Código novo executado contra banco sem 0055 é incompatível, pois já exige a coluna. Deploy invertido efetivo: **[nao verificado]**. (`bin/deploy.sh:306`, `bin/deploy.sh:317`, `bin/deploy.sh:360`; `app/chamados/models.py:4170`, `app/chamados/models.py:3532`)
- **Dois significados:** a dupla semântica da RESPOSTA foi retirada; permanece o reconhecimento textual de DEVOLUCAO BUG-PAR, fora dessa cura. (`app/chamados/models.py:4266`; `app/chamados/services/materializacao.py:275`)

7. **Outro ponto do delta**

O teste antigo de ordem esvazia o universo do compensador ao devolver `[]` em `order_by`; portanto não prova a inserção de R naquele teste. Isso limita a evidência, embora o código mantenha R dentro do mesmo `atomic` da limpeza. (`app/chamados/tests/test_e4_expand.py:323`; `app/chamados/models.py:1629`, `app/chamados/models.py:1681`, `app/chamados/models.py:1683`)

Execução da suíte Django, ensaio PostgreSQL e resultados de produção além dos anexos: **[nao verificado]**. [nao verificado]

## 17/11 11:51 · sessao a5768cbcf31a · - · - · origem=Ronald (tmux)

O código distingue claramente os dois dias:

- `_data_furo_da_grade` usa contexto da disputa/chamado, busca a batida de origem e resolve seu dia do turno; só retorna esse dia se a grade disser `trabalho`. Não escolhe entre perguntas vivas. `app/chamados/services/disputa_emissao.py:45`, `:54`, `:68`, `:76`, `:82`.
- A tarefa agrupa perguntas por `data_evento` e considera somente as do dia consultado. `app/chamados/services/tarefa_do_dia.py:244`, `:262`, `:406`, `:412`, `:580`.
- O TSV não fornece a data efetivamente retornada por `_data_furo_da_grade`, nem os contextos/batidas necessários para calculá-la; portanto, afirmar individualmente a condição completa de **(d)** permanece **[nao verificado]**.
- A equivalência temporal entre leitura de produção anterior e dump de hoje também permanece **[nao verificado]**.

Resultados possíveis no código:

| Resultado | Código produtor |
|---|---|
| Fora do vínculo, nada devido | `app/chamados/services/tarefa_do_dia.py:211` |
| Isento, nada devido | `app/chamados/services/tarefa_do_dia.py:214` |
| Competência trancada, nada devido | `app/chamados/services/tarefa_do_dia.py:221` |
| Sem célula, `nao_sei=True` | `app/chamados/services/tarefa_do_dia.py:230` |
| Sem lâmpadas e nenhum marco vencido, `nao_venceu` | `app/chamados/services/tarefa_do_dia.py:234` |
| Sem lâmpadas com exigibilidade, `ata_sem_lampadas` | `app/chamados/services/tarefa_do_dia.py:240` |
| Folga/ausência dispensa cobrança do colaborador, mas pode expor validação administrativa | `app/chamados/services/tarefa_do_dia.py:267`, `:288` |
| Projeção sem decisão correspondente, `passivo_sem_linha`, ações da instância vazias, desconhecimento | `app/chamados/services/tarefa_do_dia.py:154`, `:276`, `:338`, `:430` |

**`nao_sei=True` e `passivo_sem_linha` não equivalem a ninguém poder agir.** O código seleciona donos por ações e verbos residuais antes do fallback `nao_sei/nada`, e publica a ação residual administrativa. `app/chamados/services/tarefa_do_dia.py:665`, `:669`, `:698`, `:708`. O contrato documenta explicitamente desconhecimento coexistindo com porta; alcance depende de `rota`. `app/chamados/services/tarefa_do_dia.py:712`.

Há um teste existente exatamente para `passivo_sem_linha` com ações vazias e saída `porta_fila/admin/validar/nao_sei=True`; não o executei. `app/chamados/tests/test_e3_portas_tarefa.py:431`.

Nos dados fornecidos, todas as 31 pessoas apresentam pelo menos uma pergunta viva cujo dia tem porta `colab` ou `admin`; essa constatação documental não prova a causa histórica no dia do furo **[nao verificado]**. Cuidados nominais:

- **col727**: p21248 é `folga/nada`, mas p56610 tem `admin/validar/porta_fila`; não reduzir a pessoa inteira a (c) **[nao verificado]**.
- **col956**: apesar de `folga`, p35170 já devolve `admin/validar/porta_fila`; não reduzir a (c) **[nao verificado]**.
- **col238, col478, col854, col956**: `passivo_sem_linha` nas instâncias coexiste com porta administrativa; não classificar (b) ignorando a porta publicada **[nao verificado]**. [nao verificado]
- **col920**: `nao_sei=True` coexiste com `responder/colaborador` e `porta_app`; desconhecimento não elimina essa ação **[nao verificado]**.
- O TSV não identifica quais duas pessoas tinham `ata_sem_lampadas` na leitura original; seus `nao_sei` são do dia da pergunta **[nao verificado]**.

## 17/11 11:51 · sessao f13c15001e2a · - · - · origem=Ronald (tmux)

| Situação | Obra encontrada e limite da cobertura |
|---|---|
| Disputa agrega perguntas de outros dias | **RAIZ-DA-DISPUTA-F0-CASO-7279** trata expressamente da regra que faz disputa aberta absorver pergunta de outro dia; **RAIZ-DA-DISPUTA** cobre o elo raiz↔filhos. `app/docs/BACKLOG.md:2670`, `app/docs/BACKLOG.md:2055`; `app/docs/GRADE-MESTRA.md:108` |
| Disputa sem pergunta | **BO-DISPUTA-SEM-PERGUNTA** cobre nascimento sem pergunta; **BO-DISPUTA-DESENHO-RAIZ** a mantém estacionada como insumo da F1 da RAIZ. `app/docs/BACKLOG.md:2049`, `app/docs/BACKLOG.md:774`; `app/docs/GRADE-MESTRA.md:1023` |
| Disputa aberta após todas as perguntas morrerem | **DISPUTA-4418-ABERTA-COM-TUDO-RESOLVIDO**, registrada sem construção. `app/docs/GRADE-MESTRA.md:1027` |
| Metades do intervalo validadas isoladamente | **BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS**, complemento **BUG-PAR-FROTA-1110**. O escopo exige o problema do par; motivo de intervalo ou `passivo_sem_linha` sozinho não o comprova [nao verificado]. `app/docs/BACKLOG.md:2178`, `app/docs/BACKLOG.md:2054`; `app/docs/GRADE-MESTRA.md:1024` |
| Divergência entre estados projetados e atos | **FIO-E0-V4-E4-ESCRITORES**, etapa **E4c LEITORES DIVERGE + selo 2**. **E4C-F1-NOVA-JUIZ-RESPONDIDA** cobre leitores do juiz de resposta. Cobertura específica de cada `passivo_sem_linha` [nao verificado]. `app/docs/BACKLOG.md:2035`, `app/docs/BACKLOG.md:1982`, `app/docs/BACKLOG.md:824`; `app/docs/GRADE-MESTRA.md:107` |
| Folga com fato sem decisão | **DIA-SEM-TRABALHO-PORTA** cobre batida em folga/feriado com decisão administrativa. A presença da batida necessária em cada caso do TSV [nao verificado]. `app/docs/BACKLOG.md:1940` |
| Pergunta viva em passado sem turno | **E4A1-C6-X-PASSADO-SEM-TURNO**, prevista no CONTRACT E4b/E4c. Não equivale automaticamente a `ata_sem_lampadas` [nao verificado]. `app/docs/BACKLOG.md:2148`; `app/docs/GRADE-MESTRA.md:708` |

**O81 / ATA-SEM-LAMPADA não deve aparecer como obra aberta para os dois desconhecidos:** seu estado é FECHADA e o escopo final foi o recorte do espelho, preservando atas antigas. A cobertura da tarefa `ata_sem_lampadas` por ela [nao verificado]. `app/docs/BACKLOG.md:1269`, `app/docs/BACKLOG.md:1474`; `app/docs/GRADE-MESTRA.md:1720`

Não encontrei menção literal a `passivo_sem_linha` nesses dois documentos; obra específica para esse resultado: **sem obra identificada** [nao verificado].

## 17/11 11:50 · sessao f95d892b04ab · - · - · origem=Ronald (tmux)

Achado adicional concreto: **idempotência concorrente não fecha**. O universo é calculado antes da transação e da trava; `alvo` conserva apenas `pk__in=ids`. Duas chamadas que calculem os mesmos ids antes de adquirir a trava podem executar sequencialmente os blocos internos e ambas inserir R: a segunda não revalida a materialização depois de esperar a primeira. O lote também não exige materialização presente. É consequência do código, sem reprodução executada. (`app/chamados/models.py:1615`, `:1622`, `:1629`, `:1634`, `:1667`, `:1681`, `:3458`)

Há chamador público sem transação externa obrigatória: `desarquivar_chamado` chama diretamente a porta depois de levantar o arquivo. O RED4 faz duas chamadas sequenciais, portanto não cobre esse interleaving. (`app/chamados/views_arquivar.py:51`, `:77`, `:86`; `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:184`, `:189`)

Sobre os selos e demais classes:

- O golden agora é obrigatório, há guarda de verbos não vazios, P4 assegura CSF não vazio e as duas tarefas completas são comparadas. Não passa silenciosamente por golden ausente ou resultados inteiramente vazios. Geração efetiva na main e execução: **[nao verificado]**. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:150`, `:153`, `:154`, `:155`, `:159`, `:160`; golden não vazio em `app/chamados/tests/f2_ressuscitar_tarefa_main.json:66`, `:71`)
- Normalização de `verbos` aplica rótulo de chamado à chave, coerente com a intenção do teste. O rótulo dos chamados externos é apenas módulo, podendo colidir caso a fixture venha a conter dois chamados externos do mesmo módulo; isso é limite do normalizador, sem colisão atual demonstrada. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:38`, `:39`, `:136`)
- O teste motivo/campo tem mordida semântica real: provoca discordância nos dois sentidos e confere leitura, SQL e complemento contra a expectativa. Trocar de volta pelo motivo inverte os resultados. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:51`, `:136`, `:143`, `:150`)
- RED(a) verifica antes validado e depois projeção, leitura, SQL, estado, C6 e inclusão de vivas no SQL; controle(c) verifica pergunta legal fora do universo sem R nem alteração de contagem. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:449`, `:453`, `:456`, `:461`, `:474`, `:480`, `:481`)
- RED7 constrói empate com autores diferentes e exige o segundo. RED4 tem guardas não vazias e contagens 0→1→1 para duas formas. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:177`, `:183`, `:185`, `:190`, `:205`, `:210`)
- O teste de ordem antigo esvazia o universo do compensador: seu `order_by` devolve `[]`. Continua conferindo captura→update e atomic do evento, mas não a inserção de R nesse teste. (`app/chamados/tests/test_e4_expand.py:323`, `:327`, `:332`, `:344`)
- O teto do app verifica conteúdo efetivo com 1 e 5 perguntas e compara consultas; não é mero teto sobre resposta vazia. (`app/api/tests/test_f2_teto_consultas_app.py:53`, `:56`, `:59`, `:60`)
- O tripwire de RESPOSTA só visita chamadas ao nome literal `dict`; não alcança construtores, `objects.create`, literais `{...}`, montagem posterior nem valores de `ato` indiretos. Seu mutante prova apenas o subconjunto que ele visita. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:23`, `:35`, `:37`, `:68`)
- A marca da ressuscitação tem varredor mais amplo e anti-vacuidade nominal; a porta genérica continua aceitando dicionários e `bulk_create` sem impor procedência em execução. (`app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:28`, `:32`, `:36`, `:42`, `:70`; `app/chamados/models.py:4303`, `:4313`)
- Não há inversão nova de trava dentro da porta: C→D→Ch→P precede fotos e compensador, no mesmo atomic da limpeza. O defeito concorrente acima é a seleção anterior à trava, não a ordem da aquisição. (`app/chamados/models.py:1629`, `:1634`, `:1635`, `:1681`, `:1683`, `:2834`, `:2843`, `:2856`, `:2860`)
- O relógio novo é lido na porta escritora; leitura ordena a sequência e não lê relógio. (`app/chamados/models.py:1664`, `:4282`, `:4288`)
- `es_da_ressuscitacao` identifica procedência; a tarefa mantém a autoridade registrada e exclui a linha antes de montar sua sequência. Não identifica resposta/validação por conta própria. (`app/chamados/models.py:4196`; `app/chamados/services/tarefa_do_dia.py:426`; `app/core/juizes.py:304`)
- Deploy oficial verifica reciclagem, migra e só então aplica HUP. `max_requests=0` é a cura declarada da publicação por reciclagem; a introdução histórica do script não deve ser lida como condição vigente. Código novo contra banco pré0055 é incompatível, pois o modelo e o SQL já exigem a coluna; deploy invertido efetivo é **[nao verificado]**. (`bin/deploy.sh:44`, `:306`, `:317`, `:360`; `app/chamados/models.py:4170`, `:3532`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:22`)
- A sequência 0055 é AddField→preencher→SET CONSTRAINTS ALL IMMEDIATE→AddConstraint; segue o molde explícito da 0051 para drenar eventos diferidos antes de DDL. Execução PostgreSQL e compatibilidade durante versões mistas: **[nao verificado]**. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:21`, `:27`, `:29`, `:30`; `app/chamados/migrations/0051_decisao_pergunta.py:101`, `:104`)

## 17/11 11:50 · sessao dd1ee2204a88 · - · - · origem=Ronald (tmux)

1. **Toda selecionada recebe R no caminho atual.** O universo exige `materializada_em IS NOT NULL`, exclui fora do fato e passa pelo C6; o laço da escrita não tem mais `continue` por ausência de resposta/validação. Fotos, compensador e descarimbo ficam sob o mesmo `atomic`, depois da trava. (`app/chamados/models.py:1542`, `app/chamados/models.py:1547`, `app/chamados/models.py:1576`, `app/chamados/models.py:1629`, `app/chamados/models.py:1634`, `app/chamados/models.py:1667`, `app/chamados/models.py:1681`)

2. **Idempotência sequencial procede; concorrente não está garantida.** A primeira chamada limpa `materializada_em`; uma chamada iniciada depois dela sai do universo. O RED4 testa exatamente essa ordem, contando uma R nas duas perguntas e chamando novamente só depois. (`app/chamados/models.py:1543`, `app/chamados/models.py:3460`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:184`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:189`)

   **Defeito concreto por interleaving:** duas chamadas podem selecionar os mesmos `ids` antes de qualquer uma pegar a trava. A segunda espera, mas depois usa `alvo` filtrado somente pelos PKs antigos, grava outra R e faz um `UPDATE` sem exigir materialização atual. Falta recalcular/revalidar o universo sob a trava antes das fotos e do compensador. (`app/chamados/models.py:1615`, `app/chamados/models.py:1622`, `app/chamados/models.py:1634`, `app/chamados/models.py:1667`, `app/chamados/models.py:1681`, `app/chamados/models.py:3458`)

   Há chamador concreto sem transação externa declarada: `desarquivar_chamado` chama a ressuscitação diretamente; `arquivo.levantar` só pede a trava condicional que retorna quando não existe `atomic`. Não é necessário presumir que todos os chamadores já seguram C. (`app/chamados/views_arquivar.py:51`, `app/chamados/views_arquivar.py:77`, `app/chamados/views_arquivar.py:86`, `app/chamados/services/arquivo.py:209`, `app/chamados/models.py:496`)

3. **Pergunta sem resposta nem validação ganha efeito visível no histórico.** `REABERTURA` integra `ATOS_COM_FOTO`; `respostas_anteriores` acrescenta a linha mesmo com foto vazia. O modal passa a mostrar “(sem texto)”, “reaberta para correcao”, instante e motivo da ressuscitação. Portanto “nenhum leitor muda o que devolve” seria falso; o estado vigente continua vazio, mas o histórico muda. (`app/chamados/models.py:4140`, `app/chamados/models.py:3782`, `app/chamados/models.py:4135`, `app/templates/chamados/partials/modal_fio.html:205`, `app/templates/chamados/partials/modal_fio.html:212`)

4. **Tarefa oficial:** a carga descarta a marcada antes de montar a sequência/eleger o último ato; R comum continua significando devolução ao colaborador. Para silêncio e sem atos, a R compensadora não passa a ser o último ato da tarefa. (`app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:156`, `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:176`)

5. **Demais leitores solicitados:**
   - A leitura e a face SQL tratam R como limpeza; se resposta e validação já eram vazias, continuam vazias. (`app/chamados/models.py:4251`, `app/chamados/models.py:4252`, `app/chamados/models.py:4297`, `app/chamados/models.py:4299`, `app/chamados/models.py:3532`)
   - A lista sem veredito exige resposta legal vigente; a R vazia não a faz entrar na lista/CSF. (`app/chamados/models.py:1985`, `app/chamados/models.py:1986`, `app/chamados/models.py:1987`)
   - I18/I20 leem projeção, sem contar R; não detectam duplicação da compensadora nem verificam sua prevalência na sequência. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1076`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1106`)
   - `curar_perguntas_em_limbo` seleciona pelo par projetado e pelo conjunto nominal, sem contar R. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`)
   - `materializacao.py` consulta a sequência para o rótulo BUG-PAR somente por DEVOLUCAO com prefixo; a compensadora não satisfaz esse predicado. Ausência rejeitada ainda usa R comum. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:1167`)
   - A foto E4b captura todas as decisões e todos os campos concretos, portanto acrescenta a R ao retrato auditável. (`app/chamados/services/foto_e4b.py:210`, `app/chamados/services/foto_e4b.py:280`)
   - O teto de reaberturas do colaborador vem de `Disputa.reaberturas_colab`, não da contagem das decisões R; a reabertura de sistema tampouco consome `reaberturas_admin`. (`app/chamados/models.py:2367`, `app/chamados/models.py:2383`, `app/chamados/services/fio.py:388`, `app/chamados/models.py:2307`)
   - Relatório específico contando R: não encontrado nos leitores diretos inspecionados; existência fora desses caminhos [nao verificado].

6. **Foto `_vpor`: cura procede no caminho da porta.** As fotos do passivo são persistidas antes da consulta nova com `prefetch_related('decisoes')`; esse prefetch usa `Meta.ordering = ['ato_em', 'pk']`. O laço sobrescreve `_vpor` até a última VALIDACAO do instante vigente, e `leitura` ordena pela mesma dupla para linhas reais. Não há prefetch customizado invertendo a ordem nesse caminho. (`app/chamados/models.py:1635`, `app/chamados/models.py:1667`, `app/chamados/models.py:1672`, `app/chamados/models.py:1674`, `app/chamados/models.py:4175`, `app/chamados/models.py:4284`, `app/chamados/models.py:4288`)

   O RED7 tem duas validações no mesmo instante com autores distintos e exige o segundo autor na foto; a asserção distingue a implementação anterior. Execução do teste e contagem real de empates [nao verificado]. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:205`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:210`)

## 17/11 11:50 · sessao c7e55a092905 · - · - · origem=Ronald (tmux)

Os quatro montadores de RESPOSTA encontrados declaram o campo: humana `False`; automática, carimbo pela trilha e foto da projeção `True`. A persistência permanece em `registrar`, via `bulk_create(cls(**ln))`. (`app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1679`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/models.py:4236`, `app/chamados/models.py:4313`)

Os backfills não acrescentam montador independente de RESPOSTA: `backfill_decisoes_pergunta.linhas_a_criar` consulta a mesma fábrica; sua escrita própria é SILENCIO. `backfill_fio_morto_e4b` registra o resultado da fábrica. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

**O tripwire não cobre todos os formatos pedidos.** Ele inspeciona somente chamadas cujo nome é literalmente `dict`, com `ato` explícito como atributo `RESPOSTA` ou constante `'resposta'`. Não vê `DecisaoPergunta(...)`, `objects.create(...)`, dicionário literal passado a `registrar`, `bulk_create([D(...)])`, montagem posterior por atribuição ou valor indireto de `ato`. A prova executada em memória devolveu `[]` para os quatro primeiros formatos. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:23`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:35`)

Não localizei escritor concreto de produção omitindo o campo fora desses quatro, nem fixture de produção de `DecisaoPergunta`; isso é resultado do censo da cópia, não garantia sobre fontes externas. [nao verificado]

O campo tem significado operacional único: RESPOSTA conserva ou desfaz a validação. Leitura Python e face SQL consultam o campo. O leitor decisório restante encontrado de `ato_motivo` restringe-se a DEVOLUCAO BUG-PAR, não RESPOSTA. (`app/chamados/models.py:4266`, `app/chamados/models.py:4284`, `app/chamados/models.py:3532`, `app/chamados/services/materializacao.py:275`)

A migração conserva a regra no domínio permitido: `''` permanece `False`; espaço e demais textos não vazios tornam-se `True`, sem trim. `NULL` não pertence ao esquema: o campo é não anulável desde a origem e continua assim. Portanto não há base para afirmar conservação sobre estoque hipotético com `NULL`; sua existência seria [nao verificado]. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:25`, `app/chamados/migrations/0050_resposta_anterior.py:24`, `app/chamados/migrations/0051_decisao_pergunta.py:48`, `app/chamados/models.py:4147`)

A sequência declarada da 0055 é AddField → RunPython → SET CONSTRAINTS ALL IMMEDIATE → AddConstraint, repetindo o mecanismo usado pela 0051 antes de ALTER TABLE posterior. Aplicação efetiva no PostgreSQL permanece [nao verificado]. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:22`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:27`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:29`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:30`, `app/chamados/migrations/0051_decisao_pergunta.py:101`)

O teste semântico novo tem mordida por construção: motivo preenchido com campo falso deve desfazer; motivo vazio com campo verdadeiro deve conservar. O teste da constraint usa VALIDACAO com campo verdadeiro e exige `IntegrityError`. Execução desses testes com banco: [nao verificado]. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:154`)

## 17/11 11:49 · sessao c2475fbabde5 · CORTE-FOLHA-ESPELHO · leitura · origem=Code

**O delta principal é retirar “disputa aberta” da decisão:** o corte passa a exigir pergunta viva por C6, dia dentro da competência e ação humana publicada pela tarefa do dia. Uma disputa fechada pode conter pergunta viva; uma aberta pode não conter nenhuma elegível. (`app/chamados/juizes.py:1403`; `app/docs/CORTES.md:409`; aplicação do complemento: [nao verificado])

**Sobre o erro 56712:** o caminho de leitura identificado alcança `tarefa → carga_da_vida → _slot_esperado → _ts_tipo_do_plantio → _tipo_pelo_marco`. A divergência registra `ERROR` e incrementa contador em memória; esse ponto não demonstra escrita na pergunta ou no banco. A ocorrência individual, a frequência nas últimas 24 horas e eventual erro devolvido ao usuário permanecem [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/carga_pergunta_viva.py:119`; `app/chamados/juizes.py:375`; `app/chamados/services/materializacao.py:568`; `app/core/observ.py:73`)

**(i) Censo — somente o que muda**

| Leitores do pd.r | Delta |
|---|---|
| PDF auditável e suas cascas: emissão, lote, job, avulso por competência e anexação | Passam a consumir a resposta composta por competência. Hoje o bloqueio ainda pergunta apenas pela disputa aberta. (`app/relatorios/pdf_espelho.py:1085`; `app/relatorios/services.py:237`; `app/relatorios/views.py:1003`; `app/relatorios/views.py:1127`; mudança: [nao verificado]) |
| Classificador, montador/TXT e leitores indiretos da classificação enumerados no pd.r | Herdam a unidade nova pelo classificador; não recebem filtros próprios de pergunta. Hoje o ramo impeditivo consulta a célula. (`app/folha/export.py:716`; `app/folha/export.py:968`; mudança: [nao verificado]) |
| `dias_que_seguram`, `fora_da_folha`, `o_que_segura` | Precisam receber perguntas/dias elegíveis e ações da composição. Hoje selecionam apenas `furo_espelho` e buscam seus dias no retentor da célula. (`app/folha/fora_da_folha.py:121`; `app/folha/fora_da_folha.py:156`; `app/folha/fora_da_folha.py:203`; mudança: [nao verificado]) |
| Tarefa do dia/C19 | Entra como autoridade de ação e alcance, sem ganhar regra própria de retenção. Publica ações por instância e também portas residuais do dia. (`app/chamados/services/tarefa_do_dia.py:42`; `app/chamados/services/tarefa_do_dia.py:627`) |
| Retrato e tela do admin | O lavrador precisa gravar decisão, perguntas/dias e ações recebidos; a tela continua lendo o gravado. Hoje um motivo novo cairia no dono `admin`, com dias possivelmente ausentes e ações vazias. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:1214`; `app/folha/retrato_folha.py:1232`; `app/folha/retrato_folha.py:1249`; `app/folha/views_retrato.py:194`; mudança: [nao verificado]) |
| Prévia e cobertura da proposta | Precisam acompanhar a resposta composta: hoje a prévia consulta perguntas datadas, mas a cobertura da retenção compara dias da célula. (`app/folha/previa.py:64`; `app/folha/previa.py:169`; mudança: [nao verificado]) |
| Vigia de alcance | Herda o universo novo por `dias_que_seguram`; não deve medir apenas o antigo conjunto de furos. (`app/chamados/services/vigia_fio.py:510`; `app/chamados/services/vigia_fio.py:537`; mudança: [nao verificado]) |

**Não há migração geral do helper público.** Gates de cadastro/benefício, LEDs globais, drawer, cobranças, regularização externa, seleção/travamento/fechamento de recipientes e verbos continuam respondendo suas perguntas operacionais. HTML/API do espelho hoje apresentam existência global de disputa; isso não pode ser apresentado como impedimento desta competência sem consumir a composição. (`app/chamados/services/disputa_emissao.py:34`; `app/colaboradores/mixins.py:300`; `app/colaboradores/services/situacional.py:117`; `app/colaboradores/services/drawer.py:71`; `app/chamados/services/regularizacao_ext.py:59`; `app/ponto/views.py:142`; `app/api/views.py:1670`; alcance proposto: [nao verificado])

Badges e pendências do app conservam sua partição; o complemento não autoriza aproveitar esta obra para migrar a B2. Aprovação conserva a pergunta separada de `travas_aprovacao`; vigia de pausa conserva sua medição de L-033. (`app/chamados/services/pergunta_no_app.py:76`; `app/ponto/services/fechamento_lista.py:100`; `app/folha/services/vigia_pausa.py:125`; limite B2: [nao verificado — complemento, item 8])

**(ii) Onde mora a resposta única**

**Não encontrei o predicado completo já composto nos caminhos examinados.** C6 responde vida, C19 responde tarefa e K2 responde entrada no TXT; o registro os mantém separados. A inexistência absoluta fora desses caminhos permanece [nao verificado]. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:330`)

O endereço mais próximo é `tarefa_do_dia`, com `carga_da_tarefa(colaboradores, ini, fim, agora)`: a carga já seleciona perguntas factuais na janela inclusiva e consulta C6 em parte dos caminhos. Entretanto, tarefas normais também são produzidas pela sequência de decisões sem exigir C6 naquele ramo; pode haver validação administrativa sem pergunta viva. Portanto, C19 **não é hoje a resposta completa**. (`app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:357`; `app/chamados/services/tarefa_do_dia.py:534`)

O pré-desenho deve declarar **uma composição única para o corte**, consumida por K2 e pelo PDF, reaproveitando C6, C19, janela e exceções existentes. **Essa composição seria juiz novo**; não é correto anunciá-la como função já existente nem alterar silenciosamente o helper global. Endereço proposto: módulo de corte compartilhado na fronteira da folha, ainda sem implementação ou registro. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:330`; proposta: [nao verificado])

A composição precisa preservar estes cuidados:

- O dia factual da pergunta já tem leitura própria: conversa não fornece dia do turno; a janela respeita o corte da empresa. (`app/chamados/juizes.py:338`; `app/ponto/janelas.py:54`)
- Não exigir `fechada_em IS NULL`: C6 não usa essa guarda e existe pergunta respondida viva em recipiente encerrado. (`app/chamados/juizes.py:1403`; `app/docs/CORTES.md:409`)
- Não usar `bool(porta)`: desconhecimento pode publicar envelope com `rota=None`. Tampouco `nao_sei=True` apaga uma ação que esteja publicada. (`app/chamados/services/tarefa_do_dia.py:719`)
- Não exigir ação no topo para reconhecer colaborador respondedor: `porta_app` pode ter `acao=None`; o responder aparece nas instâncias. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:679`; `app/chamados/services/tarefa_do_dia.py:701`)
- Conservar pergunta, dia, ação, responsável, porta e desconhecimento como evidências; o complemento não estabelece uma exigência adicional de igualdade de `pergunta_id` para todo verbo administrativo do dia. (`app/chamados/services/tarefa_do_dia.py:648`; `app/chamados/services/tarefa_do_dia.py:684`; contrato futuro: [nao verificado])

**(iii) Matriz**

Leio o “só segura” literalmente para o ramo espelho/disputa: um NÃO não pode voltar a ser FORA apenas pela mesma célula acusadora ou ausente, sem pergunta elegível e ação. Esses caminhos laterais existem hoje. Outras exclusões da folha continuam independentes. (`app/folha/export.py:1270`; `app/folha/export.py:1293`; `app/folha/export.py:1295`; `app/folha/export.py:959`; `app/folha/export.py:974`; interpretação: [nao verificado — complemento, itens 1–2])

Na tabela, cada resultado representa a sequência **export → PDF → retrato → tela do admin**:

- **R:** FORA pelo corte → informacional → grava perguntas/dias e ações impeditivos → mostra quem pode agir. [nao verificado — pré-desenho]
- **N:** sem impedimento por essa unidade → auditável permitido quanto a esse bloqueio → sem retenção atribuída à pergunta → sem pendência impeditiva desta folha. Não garante ENTRA contra outras exclusões. [nao verificado — pré-desenho]
- **E:** N, com incidente para nossa lista, dono ESTRUTURA/RAIZ-DA-DISPUTA; não vira tarefa do admin. [nao verificado — complemento, item 2]

| Estado da pergunta | Dia | Sem exceção L-033 | Só intervalo, L-033 comprovada |
|---|---|---|---|
| Viva com ação | Dentro | **R**. [nao verificado] | **N**; pergunta pode continuar viva. (`app/docs/LEIS.md:81`; aplicação PDF: [nao verificado]) |
| Viva com ação | Fora | **N**. [nao verificado] | **N**. [nao verificado] |
| Viva com ação aparente | Sem dia | **N**; ação sem dia identificado não demonstra tarefa elegível na janela. [nao verificado] | Não demonstrável sem identificar o dia; permanece **N**. [nao verificado] |
| Viva sem ação | Dentro | **E**. [nao verificado] | **E**, sem retenção. [nao verificado] |
| Viva sem ação | Fora | **E**. [nao verificado] | **E**, sem retenção. [nao verificado] |
| Viva sem ação | Sem dia | **E**. [nao verificado] | L-033 não demonstrável; **E**. [nao verificado] |
| Morta | Dentro | **N**. [nao verificado] | **N**. [nao verificado] |
| Morta | Fora | **N**. [nao verificado] | **N**. [nao verificado] |
| Morta | Sem dia | **N**. [nao verificado] | L-033 não demonstrável; **N**. [nao verificado] |

Pergunta morta normalmente é histórico; se restar recipiente sem ação, o recipiente residual vai para ESTRUTURA. Uma pessoa com pergunta elegível e outro beco continua retida pela primeira; o beco segue separado para nossa lista. [nao verificado — aplicação do complemento, itens 1–2]

L-033 exige protestos exatamente `{'FURO_PARCIAL'}`, sem desalinhamento, turno aberto ou desconhecimento, e somente lâmpadas de pausa apagadas. Motivo misto ou ponta faltante não recebe a exceção. (`app/folha/export.py:1150`; `app/folha/export.py:1155`; `app/folha/export.py:1173`)

O limite das competências entregues do pd.r permanece; o complemento não autoriza aplicar retroativamente o corte, tocar a porta do export ou produção. [nao verificado — pd.r fornecido; complemento, item 8]

**(iv) REDs — especificações, sem execução [nao verificado]**

Cada linha deve conferir a cadeia inteira da matriz, incluindo retrato e apresentação do responsável. [nao verificado — pré-desenho]

| Linha | Caso que morde → expectativa |
|---|---|
| Viva/ação/dentro | Disputa **fechada**, pergunta respondida viva, admin pode validar → R; controle com colaborador respondedor e `acao=None` no topo também retém. (`app/docs/CORTES.md:409`; `app/chamados/services/tarefa_do_dia.py:701`; expectativa: [nao verificado]) |
| Viva/ação/fora | Pergunta morta dentro e única viva acionável fora → N; detecta uso da menor data da disputa. (`app/chamados/juizes.py:85`; expectativa: [nao verificado]) |
| Viva/ação/sem dia | Fio aparenta ação, mas pergunta não tem dia factual → N; abertura/criação não substitui dia. (`app/chamados/juizes.py:338`; expectativa: [nao verificado]) |
| Viva/sem ação/dentro | Célula acusa, C6 viva, envelope apenas `nao_sei` e `rota=None` → E; detecta retenção residual e falso alcance. (`app/folha/export.py:1293`; `app/chamados/services/tarefa_do_dia.py:722`; expectativa: [nao verificado]) |
| Viva/sem ação/fora | Recipiente aberto, pergunta fora, ninguém age → E; PDF não bloqueia globalmente. (`app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Viva/sem ação/sem dia | Pergunta sem dia e sem tarefa publicada → E; controle sem pergunta também não retém. (`app/chamados/services/tarefa_do_dia.py:408`; `app/relatorios/tests/test_relatorios.py:84`; expectativa: [nao verificado]) |
| Morta/dentro | Pergunta validada, célula ainda acusa, nenhuma outra elegível → N; detecta célula reintroduzindo o veto. (`app/chamados/juizes.py:1405`; `app/folha/export.py:1293`; expectativa: [nao verificado]) |
| Morta/fora | Disputa aberta somente com perguntas mortas fora → N, sem bloqueio auditável. (`app/chamados/juizes.py:1405`; `app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Morta/sem dia | Pergunta validada sem dia em recipiente aberto → N; recipiente residual sem ação segue para ESTRUTURA. (`app/chamados/juizes.py:1405`; expectativa: [nao verificado]) |
| L-033 | Viva acionável dentro, entrada/saída presentes e só pausa faltando → N; controles com ponta faltante ou protesto adicional não ganham exceção. (`app/folha/export.py:1173`; expectativa compartilhada com PDF: [nao verificado]) |

Os controles transversais precisam incluir limites `ini/fim`, cortes distintos por empresa, várias perguntas/recipientes, passagem de viva para morta no mesmo relógio e coexistência de ação com desconhecimento. Isso morde `.first()`, resumo do recipiente e leituras em instantes diferentes. (`app/ponto/janelas.py:10`; `app/chamados/services/disputa_emissao.py:40`; `app/chamados/services/tarefa_do_dia.py:721`; `app/chamados/services/tarefa_do_dia.py:741`; expectativas: [nao verificado])

**(v) As três decisões de leigo**

1. **Sem descobrir o dia, a pessoa fica impedida por essa pergunta?** O complemento responde: não, pois exige dia dentro da janela; sem ação, vai para ESTRUTURA. [nao verificado — complemento, itens 1–2]

2. **Um problema deste mês já resolvido continua segurando porque a disputa ficou aberta por outro mês?** O complemento responde: não se C6 disser morta; só conta a pergunta viva, dentro e acionável. (`app/chamados/juizes.py:1403`; aplicação: [nao verificado])

3. **Passe livre e desligados continuam protegidos?** O complemento não responde nominalmente, mas a lei existente já responde: o retentor exclui ambos, e desligado não retém em nenhum dia da competência. Não sobra decisão nova nessa pergunta. (`app/folha/export.py:1197`; `app/folha/export.py:1207`; `app/core/constants.py:13`)

Não identifiquei nova pergunta de lei necessária neste delta; falta declarar e construir a composição, não decidir novamente vida, tarefa, janela ou essas proteções. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/ponto/janelas.py:54`; avaliação do delta: [nao verificado])

A classificação individual dos 29 e dos dois desconhecidos, a remedição dos 107 e sua distribuição por empresa/ação continuam [nao verificado]: o dado fornecido contém os totais mencionados no aval, sem as linhas individuais necessárias.

perguntas de lei: 0

## 17/11 11:48 · sessao d93020e59613 · CORTE-FOLHA-ESPELHO · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1 e 2.** O complemento resolve a unidade e nomeia o C6, mas ainda deixa abertos o dia irresolúvel, a composição da L-033 e o significado operacional de “quem age”. (`app/chamados/juizes.py:338`; `app/chamados/juizes.py:1482`; `app/folha/export.py:1173`; `app/chamados/services/tarefa_do_dia.py:637`)

As referências aos anexos recebidos no enunciado, sem arquivo:linha consultável, ficam marcadas **[nao verificado]**.

No **item 1**, a premissa confere parcialmente:

1. **As premissas conferem no código?**

   “Pergunta viva pelo C6” tem autoridade existente: `pergunta_viva`, registrada em `core/juizes.py`; o wrapper memoriza e o corpo `_pergunta_viva_calc` julga. Não equivale a disputa aberta, pergunta sem resposta ou simples ausência de validação. O C6 considera validação, tranca, vias de encerramento, resposta, célula e testemunho da materialização; uma resposta pode conservar a pergunta viva para julgamento administrativo. (`app/core/juizes.py:291`; `app/chamados/juizes.py:866`; `app/chamados/juizes.py:1403`; `app/chamados/juizes.py:1423`)

   “De supervisão” corresponde à entidade `DisputaSupervisao`, ligada à pergunta por FK. O nascimento usual também cria chamado de categoria/módulo `disputa_supervisao`, área `supervisao`; não é inferência pelo papel de quem age. (`app/chamados/models.py:1774`; `app/chamados/models.py:2521`; `app/chamados/services/disputa_emissao.py:582`; `app/chamados/services/disputa_emissao.py:591`)

   Para pergunta **de fato**, `data_evento` é o dia do turno. Para **conversa**, o mesmo campo recebe o dia civil da conversa, que não é competência de fato. O helper usado pelo C6/tranca devolve `None` para conversa e `data_evento` para fato, sem alternativa pela célula. A tarefa também seleciona somente `conversa=False`, pela janela de `data_evento`. (`app/chamados/models.py:2888`; `app/chamados/models.py:2900`; `app/chamados/juizes.py:338`; `app/chamados/services/tarefa_do_dia.py:405`)

   Portanto, a fonte já existente para alinhar pergunta e tarefa é **dia do turno em `data_evento`, discriminando conversa**. Usar `celula.data` como alternativa muda o universo: essa alternativa existe no resolvedor do chamado-pai, mas não na seleção da tarefa. (`app/chamados/juizes.py:81`; `app/chamados/juizes.py:83`; `app/chamados/services/tarefa_do_dia.py:408`)

2. **Colide com aval ou lei vigente?**

   A unificação dos leitores concorda com testemunha lendo a mesma autoridade. Entretanto, **viva + dentro da janela + alguém age ainda não basta para SIM**: a L-033 mantém pagável o furo somente de intervalo, com pergunta aberta. A composição precisa chamar a isenção existente, sem deixar o novo bloqueio contorná-la. (`CLAUDE.md:20`; `app/docs/LEIS.md:81`; `app/folha/export.py:1158`; `app/folha/export.py:1293`)

   C6, C19, C7 e respostas sem veredito têm perguntas diferentes e autoridades registradas. Nenhum deles, individualmente, responde ao novo predicado combinado da folha; seu endereço e composição ainda precisam aparecer no pré-desenho. Não cabe declará-lo como autoridade existente de predicado e universo idênticos. (`app/core/juizes.py:291`; `app/core/juizes.py:292`; `app/core/juizes.py:304`; `app/core/juizes.py:308`; `CLAUDE.md:279`)

   O complemento não redefine “competência aberta” do aval anterior [nao verificado]. K6 significa existência de TXT vigente não invalidado; o marco `aberta` significa ausência dos demais marcos. São recortes distintos, portanto a pendência anterior de alcance permanece. (`app/folha/export.py:591`; `app/ponto/services/fechamento.py:1523`; `app/ponto/services/fechamento.py:1543`)

3. **Alguma condição não fecha no literal?**

   Dos quatro pontos da revisão anterior:

   | Ponto | Resultado da conferência |
   |---|---|
   | Vários fatos dentro e fora | **Fecha pela unidade pergunta**, desde que cada candidata seja examinada individualmente. Reutilizar o dia do pai reabre o defeito: ele devolve o menor dia. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:85`) |
   | Quais perguntas contam | **Fecha quanto à vida:** C6 é explícito no complemento [nao verificado]. Ainda falta discriminar fato/conversa e conservar expressamente o filtro de disputa aberta: C6 e `fechada_em` são condições distintas, e existe fluxo que reconhece pergunta viva em disputa fechada. (`app/chamados/juizes.py:338`; `app/chamados/models.py:1888`; `app/chamados/services/validacao.py:253`) |
   | Pergunta sem dia resolvível | **Segue aberto.** Os campos admitem NULL e C6 pode devolver viva sem dia nem célula. As portas atuais exigirem dia no nascimento não elimina essa possibilidade do modelo/acervo. (`app/chamados/models.py:2519`; `app/chamados/models.py:2528`; `app/chamados/models.py:2892`; `app/chamados/juizes.py:1482`) |
   | Composição da L-033 | **Segue aberta.** A isenção existe, mas não é consequência de consultar C6 ou tarefa. (`app/folha/export.py:1173`; `app/chamados/services/tarefa_do_dia.py:733`) |

   Pergunta sem dia não pode ser automaticamente classificada como “fora da janela” nem como “ninguém age”: falta insumo para consultar a tarefa daquele dia. O destino desses casos não está determinado nos itens 1 e 2 fornecidos [nao verificado]; a tarefa exige um dia dentro da carga. (`app/chamados/services/tarefa_do_dia.py:572`; `app/chamados/services/tarefa_do_dia.py:733`)

4. **Toca exportada, dinheiro ou `!` fora do declarado?**

   Implementado, o corte altera quem produz eventos financeiros: o montador omite quem não foi classificado como `entra`. Esse é o efeito pretendido, mas não autoriza alterar cálculo, escala, vínculo ou gravado exportado. (`app/folha/export.py:716`; `app/folha/export.py:726`; `CLAUDE.md:265`; `CLAUDE.md:411`)

   Também não basta proteger escritas: mudar o helper global mudaria leitores atuais sem janela, inclusive o PDF. O pré-desenho precisa preservar o alcance histórico e definir o limite de competência antes da troca. (`app/chamados/services/disputa_emissao.py:40`; `app/relatorios/pdf_espelho.py:1085`; `app/folha/export.py:591`)

No **item 2**, a direção “sem beco” é compatível com separar responsabilidade estrutural, mas o literal ainda não determina uma resposta única:

1. **As premissas conferem no código?**

   A tarefa responde por **colaborador + dia do turno + instância de marco**. Devolve `pergunta_id`, razões e ações `{acao,de,porta}`; depois publica `fio`, `rota`, `fila`, `verbos`, `acao` e `nao_sei`. O `fio` é uma seleção de quem age a seguir; as instâncias preservam os demais donos. (`app/chamados/services/tarefa_do_dia.py:42`; `app/chamados/services/tarefa_do_dia.py:46`; `app/chamados/services/tarefa_do_dia.py:54`; `app/chamados/services/tarefa_do_dia.py:733`)

   **Não existem exatamente os três pares citados.** Há `perguntar`, `responder`, `validar`, `materializar`, `rejulgar`, `decidir_ausencia`, `entregar_documento`, além de `resolver_dia` e `decidir` residual. Os donos das ações incluem `sistema`, `colaborador` e `admin`; a projeção também distingue `chamado`, `nada` e `nao_sei`. (`app/chamados/services/tarefa_do_dia.py:68`; `app/chamados/services/tarefa_do_dia.py:72`; `app/chamados/services/tarefa_do_dia.py:669`; `app/chamados/services/tarefa_do_dia.py:684`; `app/chamados/services/tarefa_do_dia.py:698`)

   Ação exigível não garante porta alcançável: resposta depende de pendência publicada no app; validação depende da âncora; sistema tem regras próprias de alcance. Portanto, contar somente `acoes` ou somente o nome do dono não reproduz a porta publicada. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:654`; `app/chamados/services/tarefa_do_dia.py:660`)

2. **Colide com aval ou lei vigente?**

   Encaminhar defeito comprovado do sistema à ESTRUTURA concorda com L-099. **L-007**, porém, trata de escritor canônico, tripwire e trilha; sozinha não demonstra que toda ausência de ação seja defeito estrutural. A tarefa também encerra exigências legitimamente por isenção, tranca ou marco ainda não vencido. (`CLAUDE.md:179`; `app/docs/LEIS.md:65`; `app/chamados/services/tarefa_do_dia.py:214`; `app/chamados/services/tarefa_do_dia.py:221`; `app/chamados/services/tarefa_do_dia.py:234`)

   O encaminhamento precisa compor com o retrato: o corte registrado anteriormente agrupava Chamado/Ninguém em Admin, e o código atual atribui `admin` a motivo diferente de `furo_espelho`. O novo destino estrutural exige integração explícita para não reaparecer automaticamente na lista administrativa. (`app/docs/CORTES.md:332`; `app/folha/retrato_folha.py:374`)

   L-033 e K6 continuam necessários: ter ação não revoga a isenção de intervalo nem define competência aberta. (`app/docs/LEIS.md:81`; `app/folha/export.py:591`; `app/ponto/services/fechamento.py:1543`)

3. **Alguma condição não fecha no literal?**

   Permanecem estas escolhas:

   - **Ação de qual pergunta?** A publicação agrega o dia inteiro e pode acrescentar `resolver_dia` sem `pergunta_id`. Falta dizer se basta ação de qualquer assunto daquele dia ou se ela precisa resolver a pergunta candidata. (`app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:640`; `app/chamados/services/tarefa_do_dia.py:684`)
   - **Lista fechada ou exemplos?** O destino de `entregar_documento`, ações de sistema e demais verbos não está definido pelo parêntese do complemento [nao verificado]. Esses valores existem na tarefa. (`app/chamados/services/tarefa_do_dia.py:68`; `app/chamados/services/tarefa_do_dia.py:185`)
   - **Ausência conhecida versus desconhecimento.** Célula ausente produz `nao_sei`, não uma negativa de ação. Desconhecimento pode coexistir com ação administrativa; não se pode transformar todo `nao_sei` em “ninguém age” nem descartar sua ação. (`app/chamados/services/tarefa_do_dia.py:230`; `app/chamados/services/tarefa_do_dia.py:274`; `app/chamados/services/tarefa_do_dia.py:336`; `app/chamados/services/tarefa_do_dia.py:719`)
   - **Viva, na janela, sem ação:** o item propõe retirar essa retenção [nao verificado], mas falta dizer se retira somente o impedimento por pergunta ou também a retenção independente pela célula. Hoje célula ausente ou acusadora pode reter sem consultar tarefa; preservar esse ramo não garante liberar a pessoa. (`app/folha/export.py:1270`; `app/folha/export.py:1273`; `app/folha/export.py:1293`)
   - **Destino concreto:** RAIZ-DA-DISPUTA existe como obra ESTRUTURA, com esqueleto indicado, e sua F1 já recebe o desenho da disputa sem pergunta. Isso sustenta o encaixe documental. Arquivo receptor, responsável e forma de entrada da “lista nossa” e deste novo complemento ao brief permanecem [nao verificado]. (`app/docs/BACKLOG.md:2055`; `app/docs/CORTES.md:483`)

   A ocorrência e quantidade dessas combinações nos grupos citados pelo anexo não podem ser atribuídas individualmente com o dado fornecido **[nao verificado]**.

4. **Toca exportada, dinheiro ou `!` fora do declarado?**

   Liberar pessoas antes retidas altera o conjunto de eventos enviados e pode alterar a aprovação automática associada à geração do TXT. O pré-desenho deve declarar esse impacto, preservando o gravado exportado e os demais crivos. (`app/folha/export.py:726`; `app/folha/views.py:244`; `app/folha/views.py:246`; `CLAUDE.md:411`)

   “Não retém por esta pergunta” também não garante entrada no TXT: permanecem rescisão, código Domínio, férias com batida, cadastro e homologação. Tampouco garante PDF auditável, pois o lote tenta o informacional após qualquer exceção da tentativa auditável. (`app/folha/export.py:866`; `app/folha/export.py:872`; `app/folha/export.py:877`; `app/folha/export.py:959`; `app/folha/export.py:974`; `app/relatorios/services.py:244`)

Para os dois itens, os campos e valores determinantes têm estes escritores e leitores:

| Campo/valor | QUEM ESCREVE ou deriva | QUEM LÊ |
|---|---|---|
| Disputa: `colaborador`, `motivos`, `contexto`, `chamado` | Emissão cria; agregação atualiza motivos/contexto; há nascimentos por órfãos e âncora. (`app/chamados/services/disputa_emissao.py:550`; `app/chamados/services/disputa_emissao.py:576`; `app/chamados/services/disputa_emissao.py:601`; `app/chamados/services/disputa_emissao.py:1137`; `app/chamados/services/disputa_emissao.py:1372`) | Identidade da pergunta, C6 e resolução do chamado. (`app/chamados/models.py:3597`; `app/chamados/juizes.py:1474`; `app/chamados/juizes.py:75`) |
| Disputa: `fechada_em` / aberta | `fechar`; reaberturas admin/sistema zeram. (`app/chamados/models.py:2013`; `app/chamados/models.py:2230`; `app/chamados/models.py:2254`) | Property aberta, bloqueador global e partição do app. (`app/chamados/models.py:1888`; `app/chamados/services/disputa_emissao.py:40`; `app/chamados/services/pergunta_no_app.py:81`) |
| Pergunta: `disputa`, `conversa`, `motivo`, texto | Portas fato/conversa e `_nascer`; emissor fornece texto e motivo. (`app/chamados/models.py:2894`; `app/chamados/models.py:2906`; `app/chamados/models.py:3124`; `app/chamados/services/disputa_emissao.py:448`) | Helper do dia, seleção da tarefa e posição do marco. (`app/chamados/juizes.py:342`; `app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:130`) |
| Pergunta: `data_evento` | Atribuição única `gravar_dia_do_fato`; nascimento, costura e re-chaveamento persistem. (`app/chamados/services/fio.py:837`; `app/chamados/models.py:3126`; `app/chamados/services/fio.py:926`; `app/chamados/services/fio.py:1090`) | C6/tranca pelo helper; carga da tarefa pela janela. (`app/chamados/juizes.py:338`; `app/chamados/juizes.py:995`; `app/chamados/services/tarefa_do_dia.py:408`) |
| Pergunta: `celula`, `colaborador`, `instancia_marco`, `batida_origem` | Nascimento deriva identidade; reuso/re-chaveamento podem reapontar célula. (`app/chamados/models.py:3592`; `app/chamados/models.py:3616`; `app/chamados/models.py:3108`; `app/chamados/services/fio.py:1085`) | C6, juiz do turno e posicionamento na tarefa. (`app/chamados/juizes.py:1418`; `app/chamados/services/fio.py:1701`; `app/chamados/services/tarefa_do_dia.py:132`) |
| `resposta_colab`, `respondida_em` | Resposta humana/automática, reparo pela trilha; carimbo pode preencher instante; ressuscitação limpa. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/models.py:3240`; `app/chamados/models.py:1555`) | C21, projeção temporal/C6 e pendência do app. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1423`; `app/chamados/services/pergunta_no_app.py:81`) |
| `validada_em` | Validação carimba; devolução, resposta nova e ressuscitação limpam; carimbo também pode preencher. (`app/chamados/services/validacao.py:168`; `app/chamados/services/validacao.py:273`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/models.py:1554`; `app/chamados/models.py:3245`) | C6 e juiz de respostas sem veredito. (`app/chamados/juizes.py:1405`; `app/chamados/models.py:1923`) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, pelas portas carimbar/identidade/descarimbar e lote. (`app/chamados/models.py:3183`; `app/chamados/models.py:3207`; `app/chamados/models.py:3362`) | C6 e exclusões da tarefa. (`app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1482`; `app/chamados/services/tarefa_do_dia.py:255`) |
| Célula: dia/DNA; `ata`, `veredito`, `veredito_via`, `protestos`; lâmpadas | Portas geram/editam célula; `lavrar_veredito` escreve julgamento/ata; leitor projeta `acesa_lavrada`. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:863`; `app/ponto/portas/celula.py:1032`; `app/escala/services/leitor_celula.py:137`) | C6, tarefa e isenção L-033. (`app/chamados/juizes.py:1454`; `app/chamados/services/tarefa_do_dia.py:108`; `app/folha/export.py:1173`) |
| Empresa: `dia_inicio_competencia`; mês/ano e janela | Tela fornece cadastro às portas de empresa; janela é derivada pelo juiz. Mês/ano são parâmetros da consulta. (`app/colaboradores/views.py:515`; `app/colaboradores/views.py:542`; `app/colaboradores/services/estrutura.py:13`; `app/ponto/janelas.py:54`) | Export, tarefa e lote PDF. (`app/folha/export.py:1306`; `app/chamados/services/tarefa_do_dia.py:408`; `app/relatorios/services.py:214`) |
| `agora`; tranca do dia | Relógio entra pela borda; portas trancam/reabrem período; C20 deriva tranca com toque admin. (`app/chamados/services/tarefa_do_dia.py:737`; `app/ponto/services/fechamento.py:1400`; `app/ponto/services/fechamento.py:1432`; `app/chamados/juizes.py:978`) | Projeção temporal C6, corte dos atos e tarefa. (`app/chamados/juizes.py:1391`; `app/chamados/services/tarefa_do_dia.py:423`; `app/chamados/services/tarefa_do_dia.py:459`) |
| Decisão: `pergunta`, `ato`, `ato_em`, `ato_texto`, `absorcao_ausencia` | Porta única `DecisaoPergunta.registrar`; resposta, validação/devolução e absorção fornecem atos. (`app/chamados/models.py:4110`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; `app/chamados/services/materializacao.py:1045`) | Carga ordena por instante/PK e tarefa lê a última decisão aplicável. (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:158`) |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` e gravador de lote; portas também conservam lavra no mesmo ato. (`app/chamados/veredito_lavrado.py:254`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/services/fio.py:1089`) | Carga verifica vigência e tarefa consulta efeito. (`app/chamados/services/tarefa_do_dia.py:432`; `app/chamados/services/tarefa_do_dia.py:168`) |
| Ausência: `status`, período/cobertura | Portas `mudar_estado`, `decidir` e `_aplicar_periodo`. (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:1051`; `app/ponto/services/ausencia.py:1057`) | Tarefa distingue documento, decisão admin e aprovação. (`app/chamados/services/tarefa_do_dia.py:179`; `app/chamados/services/tarefa_do_dia.py:242`; `app/chamados/services/tarefa_do_dia.py:309`) |
| Efeito: batida ligada por `pergunta_origem` | Materialização vincula pela porta `anotar`. (`app/chamados/services/materializacao.py:1509`) | Carga lê batidas apuráveis existentes até `agora`; tarefa distingue efeito e lâmpada. (`app/chamados/services/tarefa_do_dia.py:442`; `app/chamados/services/tarefa_do_dia.py:173`) |
| Chamado: `status_local`, silêncio; fila e verbos derivados | `_gravar_estado`; portas de arquivo; `alimentar_fila` deriva mapas pelo painel. (`app/chamados/models.py:201`; `app/chamados/services/arquivo.py:166`; `app/chamados/services/arquivo.py:202`; `app/chamados/juizes.py:1583`) | Publicação de alcance e dono da tarefa. (`app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:665`) |
| `pendentes_app` | Partição do app cruzada com C6 pela carga. (`app/chamados/services/pergunta_no_app.py:76`; `app/chamados/services/tarefa_do_dia.py:529`; `app/chamados/services/tarefa_do_dia.py:539`) | Publicação de `porta_app`. (`app/chamados/services/tarefa_do_dia.py:651`) |
| Tarefa e porta: instâncias, ações, razões, desconhecimento, rota, fio, fila | Derivação pela tarefa e `_portas_da_tarefa`; `porta_da_tarefa` projeta. (`app/chamados/services/tarefa_do_dia.py:190`; `app/chamados/services/tarefa_do_dia.py:708`; `app/chamados/services/tarefa_do_dia.py:712`) | Folha, Vigia e furo sem canal. (`app/folha/fora_da_folha.py:70`; `app/chamados/services/vigia_fio.py:357`; `app/chamados/services/furo_sem_canal.py:94`) |
| K6: tripla, vigência, invalidação; ENTREGUE | `ExportacaoDominio.registrar`/`invalidar`; vigência e ENTREGUE são derivados. (`app/folha/models.py:174`; `app/folha/models.py:158`; `app/folha/models.py:224`; `app/folha/export.py:591`) | Marcos da competência e tarefa. (`app/ponto/services/fechamento.py:1537`; `app/chamados/services/tarefa_do_dia.py:479`) |
| Nova resposta SIM/NÃO; lista ESTRUTURA | Endereço, derivador e escritor/destinatário concreto da lista ainda

[truncado em 20000 de 20634 caracteres]

## 17/11 11:47 · sessao 3da0e9f6bd91 · - · - · origem=Ronald (tmux)

Conclusões para consolidar, só da cópia e do anexo:

- A unidade nova **não exige disputa aberta**: C6 lê validação, tranca, resolução, resposta, célula e fato; não lê `disputa.fechada_em`. Logo uma pergunta viva com ação dentro de disputa fechada precisa entrar na avaliação; não conservar o filtro A do pd.r. (`app/chamados/juizes.py:1403`, `app/chamados/juizes.py:1423`, `app/chamados/juizes.py:1482`) Existe corte literal de pergunta respondida que continua viva, “só Validar”, mesmo com disputa encerrada. (`app/docs/CORTES.md:409`)
- **Não basta acrescentar uma retenção à classificação atual.** O ramo atual retém por célula ausente/acusadora sem exigir pergunta ou ação; manter esse ramo como impedimento independente permitiria reter exatamente os becos que o complemento manda liberar. (`app/folha/export.py:1270`, `app/folha/export.py:1293`, `app/folha/export.py:1295`; interpretação do complemento: [nao verificado])
- “Sem impedimento pela pergunta” precisa preservar as **outras exclusões** — rescisão, código, férias com batida, cadastro zero, catálogo —, sem significar ENTRA garantido. (`app/folha/export.py:866`, `app/folha/export.py:872`, `app/folha/export.py:877`, `app/folha/export.py:959`, `app/folha/export.py:974`)

A matriz nova tem uma única combinação afirmativa: **C6 viva + ação da própria pergunta + dia dentro + não L-033**; as demais não retêm por essa unidade. Isso é expectativa do complemento, ainda não implementação. [nao verificado]

| Estado | Dentro, não L-033 | Fora, não L-033 | Sem dia | L-033 comprovada |
|---|---|---|---|---|
| Viva com ação | Retém / PDF informacional | Não retém / auditável permitido | Não retém / auditável permitido | Não retém / auditável permitido |
| Viva sem ação | Não retém; ESTRUTURA | Não retém; ESTRUTURA | Não retém; ESTRUTURA | Não retém; ESTRUTURA |
| Morta | Não retém por essa pergunta | Não retém por essa pergunta | Não retém por essa pergunta | Não retém por essa pergunta |

Todas as expectativas da tabela: [nao verificado — complemento fornecido]. Retrato/tela devem registrar a mesma decisão, evidências e dono; sem ação não pode virar pendência do admin. [nao verificado — complemento fornecido]

A exceção L-033 exige protestos **exatamente** `{'FURO_PARCIAL'}` e resposta do juiz dos períodos sem desalinhamento, turno aberto ou desconhecimento, com alguma lâmpada apagada e todas de pausa. “Sem dia” não permite afirmar L-033; a retenção já é NÃO pela falta da condição temporal, sem inventar exceção. (`app/folha/export.py:1150`, `app/folha/export.py:1155`, `app/folha/export.py:1173`)

REDs que mordem a mudança, especificações sem execução: [nao verificado]

| Linha | Caso que morde |
|---|---|
| Viva com ação / dentro | Disputa **fechada**, pergunta respondida viva, admin pode validar; célula não retinha antes: deve reter; controle com mesma pergunta fora libera. (`app/chamados/juizes.py:1423`; cenário/expectativa: [nao verificado]) |
| Viva com ação / fora | Primeira pergunta dentro já morta; única viva acionável fora: não reter nem herdar `min(data)` da disputa. (`app/chamados/juizes.py:85`; expectativa: [nao verificado]) |
| Viva com ação / sem dia | Disputa aberta, tarefa aparenta ação, dia ausente: não transformar abertura/criação em dia dentro. (`app/chamados/models.py:1820`, `app/chamados/models.py:2899`; expectativa: [nao verificado]) |
| Viva sem ação / dentro | Célula acusadora e pergunta C6 viva, tarefa sem verbo: liberar impedimento e registrar ESTRUTURA; detecta retenção residual da célula. (`app/folha/export.py:1293`; expectativa: [nao verificado]) |
| Viva sem ação / fora | Disputa aberta fora, nenhum verbo: não bloquear PDF e não mandar para admin. (`app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Viva sem ação / sem dia | Recipiente aberto sem pergunta datada/acionável: não bloqueia PDF; lista ESTRUTURA. O teste atual cria disputa sem pergunta e exige `ValueError`; agora essa expectativa é superada pelo complemento. (`app/relatorios/tests/test_relatorios.py:84`; expectativa: [nao verificado]) |
| Morta / dentro | Célula ainda acusa, mas pergunta validada/morta e nenhum outro credor elegível: não reter por célula solta. (`app/chamados/juizes.py:1405`, `app/folha/export.py:1293`; expectativa: [nao verificado]) |
| Morta / fora | Disputa aberta só com perguntas mortas fora: auditável permitido; abertura não substitui C6. (`app/relatorios/pdf_espelho.py:1085`; expectativa: [nao verificado]) |
| Morta / sem dia | Pergunta validada sem dia em recipiente aberto: C6 morta prevalece, sem bloqueio PDF. (`app/chamados/juizes.py:1405`; expectativa: [nao verificado]) |
| L-033 / viva acionável dentro | Entrada/saída batidas, apenas pausa faltando: pergunta continua viva, não retém nem torna PDF informacional; controle com ponta faltante ou segundo protesto não ganha exceção. (`app/docs/LEIS.md:81`, `app/folha/export.py:1173`; expectativa PDF: [nao verificado]) |

Sobre as três perguntas leigas do pd.r:

1. **Sem saber o dia, a pessoa fica impedida?** Complemento responde: não por esta unidade, pois exige dia dentro. Sem ação, dono ESTRUTURA; sem dia com ação não autoriza inventar data. [nao verificado — complemento fornecido]
2. **Problema deste mês já resolvido continua impedindo porque a disputa segue aberta por outro mês?** Complemento responde: não se C6 disser morta; só a viva acionável dentro conta. Recipiente fechado também não prova morte. (`app/chamados/juizes.py:1403`; aplicação do complemento: [nao verificado])
3. **Passe livre e desligados continuam protegidos?** Complemento não responde nominalmente, mas a lei anterior já responde: o retentor retorna vazio para isento ou desligado, com literal “desligado NÃO retém … NENHUM dia”. Preservar essa proteção não requer nova pergunta de lei. (`app/folha/export.py:1197`, `app/folha/export.py:1207`; `app/core/constants.py:13`; `app/docs/CORTES.md:441`)

Não identifiquei pergunta de lei nova necessária nesta parte; dúvidas de “como correlacionar pergunta e verbo” são contrato da autoridade/implementação, não autorização para inventar critério. [nao verificado]

## 17/11 11:47 · sessao a732081df2ae · - · - · origem=Ronald (tmux)

**Autoridade:** não encontrei função existente que responda integralmente “há pergunta viva C6 desta pessoa, dentro da competência, com ação humana publicada pela tarefa?”. O registro separa C6, C19 e K2; portanto dar endereço único a esse predicado completo seria **juiz novo de composição**, reaproveitando essas autoridades. A busca realizada não prova inexistência absoluta fora dos caminhos examinados. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:330`; inexistência absoluta: [nao verificado])

O endereço existente mais próximo é `chamados/services/tarefa_do_dia.py::tarefa_do_dia`, com `carga_da_tarefa(colaboradores, ini, fim, agora)`: a carga seleciona perguntas factuais, `conversa=False`, por `data_evento` inclusivo na janela, e consulta C6 para as exposições especiais e para as pendentes do app. Contudo, a tarefa normal de marco também pode nascer da sequência de decisões sem consultar C6 nesse ramo. (`app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:485`; `app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:334`)

**Não serve `bool(porta_da_tarefa(...))`:** desconhecimento publica envelope com `rota=None`; a tarefa também oferece validar célula sem pergunta viva, Resolver dia ou verbo residual de fila. Logo é preciso ler a ação humana publicada e manter a exigência independente de pergunta viva C6 na janela. Não criar novo juiz para “quem age”: essa resposta já pertence a C19. (`app/chamados/services/tarefa_do_dia.py:357`; `app/chamados/services/tarefa_do_dia.py:665`; `app/chamados/services/tarefa_do_dia.py:684`; `app/chamados/services/tarefa_do_dia.py:698`; `app/chamados/services/tarefa_do_dia.py:712`; `app/core/juizes.py:159`)

O literal fornecido não exige explicitamente que a ação esteja na instância com o mesmo `pergunta_id`; exige que a tarefa do dia dê ação. Introduzir essa exigência adicional seria interpretação além do complemento. [nao verificado — complemento fornecido]

**Leitura com efeito de observabilidade:** a tarefa chama `carga_da_vida`, que antecipa `_slot_esperado`; este chama `_ts_tipo_do_plantio`, que chama `_tipo_pelo_marco`. A divergência entre marco cobrado e ata chama `registrar_engolido('chamados.materializacao', 'tipo_marco_divergente', ...)`. (`app/chamados/services/tarefa_do_dia.py:534`; `app/chamados/services/carga_pergunta_viva.py:117`; `app/chamados/juizes.py:375`; `app/chamados/services/materializacao.py:656`; `app/chamados/services/materializacao.py:568`)

Nesse ponto, o efeito comprovado é incremento de contador em memória e `logging.error`, não escrita em `PerguntaDisputa` ou chamada ao escritor de `LogAuditoria`; o escritor deste último é outra função, `evento`. A pergunta 56712, ocorrências nas últimas 24 horas e devolução de erro em telas/app continuam [nao verificado]. (`app/core/observ.py:73`; `app/core/observ.py:94`; `app/core/observ.py:134`)

**Decisão antiga 3:** já há lei literal independente do complemento: desligado não retém em **nenhum dia da competência**; o retentor retorna vazio para desligado ou isento, e C19 retorna sem tarefa para isento. Portanto não cabe reapresentar genericamente essa proteção como lei ausente. A interação de eventual novo predicado deve preservar esses guardas. (`app/folha/export.py:1197`; `app/folha/export.py:1207`; `app/chamados/services/tarefa_do_dia.py:214`; `app/core/constants.py:13`)

## 17/11 11:46 · sessao 81ab028b7e9d · - · - · origem=Ronald (tmux)

- **Entram como leitores da evidência da retenção:** `fora_da_folha.dias_que_seguram`, `fora_da_folha`, `o_que_segura`, `retrato_folha.montar_linhas` e, por herança, tela/admin, barra, LED, drawer e PDF do retrato. Hoje o universo enriquecido é exclusivamente `motivo == 'furo_espelho'`; uma nova retenção por pergunta com célula limpa ficaria sem esse enriquecimento. (`app/folha/fora_da_folha.py:156`; `app/folha/fora_da_folha.py:165`; `app/folha/fora_da_folha.py:203`; `app/folha/retrato_folha.py:1214`; `app/folha/views_retrato.py:194`; `app/folha/views_retrato.py:200`)
- **Tarefa do dia fornece ação/alcance, mas seu resumo não certifica a pergunta:** a tarefa conserva `pergunta_id` e ações nas instâncias; o resumo pode obter dono por outras portas do dia ou pelo residual “decidir”, e pode publicar `resolver_dia` com `pergunta_id=None`. A composição precisa conservar o vínculo entre pergunta elegível e testemunho da ação, evitando usar só `fio` do dia como prova. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:665`; `app/chamados/services/tarefa_do_dia.py:686`; `app/chamados/services/tarefa_do_dia.py:690`; `app/chamados/services/tarefa_do_dia.py:707`)
- **RED que morde colaborador respondedor:** `porta_app` pode ter `acao=None` no topo; a ação `responder` está na instância. Portanto `porta.get('acao')` excluiria o respondedor, embora tenha ação alcançável. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:679`; `app/chamados/services/tarefa_do_dia.py:701`; `app/chamados/services/tarefa_do_dia.py:709`)
- **RED que morde desconhecimento:** `porta_da_tarefa` pode devolver envelope com `rota=None` porque `nao_sei=True`; `bool(porta)` não prova ação disponível. Também pode haver `nao_sei=True` com rota válida, portanto desconhecimento por si só não apaga uma ação publicada. (`app/chamados/services/tarefa_do_dia.py:719`; `app/chamados/services/tarefa_do_dia.py:721`; `app/chamados/services/tarefa_do_dia.py:722`)
- **RED que morde retrato:** acrescentar apenas motivo novo ao export produziria dono `admin`, tipo `sem_causa`, dias/datas desconhecidos e ações vazias. O retrato precisa gravar as evidências recebidas da composição: perguntas/dias que efetivamente retêm e suas ações publicadas, sem refazer C6 ou a tarefa. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:388`; `app/folha/retrato_folha.py:1232`; `app/folha/retrato_folha.py:1248`; `app/folha/retrato_folha.py:1249`)
- **Sem ação vai à estrutura no complemento; hoje ainda ocupa baldes da UI:** a cópia conserva `sem_acao_disponivel` e `sem_leitura_sistema`; o agrupamento misto conserva dias sem porta junto da pessoa com ação. Assim, a projeção futura precisa separar os incidentes estruturais do impedimento acionável, inclusive no misto. (`app/folha/retrato_folha.py:191`; `app/folha/retrato_folha.py:194`; `app/folha/fora_da_folha.py:97`; `app/folha/fora_da_folha.py:238`; destino novo: [nao verificado — complemento fornecido, item 2])
- **Tela continua testemunha do gravado:** lê `RetratoFolha`, usa `linhas_do_retrato` e conta o recorte pelas datas gravadas; o filtro de período altera dias/saldo, não a presença da pessoa na lista. Não cabe acrescentar nela consulta independente a disputa/pergunta. (`app/folha/views_retrato.py:194`; `app/folha/views_retrato.py:200`; `app/folha/views_retrato.py:225`; `app/folha/views_retrato.py:230`; `app/folha/retrato_folha.py:1327`)
- **Mais um consumidor indireto do universo:** `vigia_fio.censo_alcance` lê `dias_que_seguram` e deduplica por colaborador/dia; passa a acompanhar o universo novo se continuar medindo quem segura a folha. (`app/chamados/services/vigia_fio.py:510`; `app/chamados/services/vigia_fio.py:537`; `app/chamados/services/vigia_fio.py:541`; `app/chamados/services/vigia_fio.py:544`)
- **Prévia precisa acompanhar a consequência, sem virar autoridade:** lê classificação e perguntas datadas, mas `_proposta_cobre_retencao` só compara dias retidos pela célula; pode prometer liberação mesmo restando pergunta elegível se não consumir a resposta composta. (`app/folha/previa.py:43`; `app/folha/previa.py:64`; `app/folha/previa.py:169`; `app/folha/previa.py:173`)
- **Permanecem perguntas distintas:** lista de fechamento lê ENTRA e, separadamente, `travas_aprovacao`; nova retenção não autoriza deduzir novo veto de aprovação. O vigia de pausa continua perguntando pelo retentor da célula para medir L-033. (`app/ponto/services/fechamento_lista.py:73`; `app/ponto/services/fechamento_lista.py:100`; `app/ponto/services/fechamento_lista.py:102`; `app/folha/services/vigia_pausa.py:125`)

## 17/11 11:45 · sessao 3da05f32583e · PORTA-EXPORT · verificador · origem=Code

**Não aprovo ainda:** há caminhos que ocultam FALHOU, carimbam hoje uma medição iniciada ontem e publicam VERDE após lavrar um carimbo vermelho na mesma rodada. O último problema já existia na agregação anterior e permanece. (`app/folha/porta_export.py:1083`, `app/folha/porta_export.py:318`, `app/folha/porta_export.py:774`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:150`.)

Banco, logs, execução integrada e cron implantado: **[nao verificado]**.

1. **Leitores e escritores**

   | Linha/campo | Quem escreve | Quem lê |
   |---|---|---|
   | Carimbo `porta_export_leitores:AAAA-MM` | `lavrar`, usando `chave_do_carimbo`: `app/folha/porta_export.py:775`, `app/folha/porta_export.py:920`. Chamador produtivo encontrado: command em `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`. | `estado_da_porta`, pela mesma fábrica: `app/folha/porta_export.py:1078`. |
   | Carimbo `valor` | `lavrar`: falhas, ou `-1` quando não mediu: `app/folha/porta_export.py:777`. | Classificação da porta: `app/folha/porta_export.py:1085`; magnitude da mensagem: `app/folha/porta_export.py:863`. |
   | Carimbo `detalhe` | `lavrar` monta os campos: `app/folha/porta_export.py:779`. | Lista de divergências em `conferir`: `app/folha/porta_export.py:818`; motivo em `_campos_do_texto`: `app/folha/porta_export.py:1095`. |
   | Pedido `porta_export_pedido:AAAA-MM` | `pedir` cria; `assumir` atualiza/cria: `app/folha/porta_export.py:1009`, `app/folha/porta_export.py:1029`. Fábrica: `app/folha/porta_export.py:925`. | `_linha_do_pedido`: `app/folha/porta_export.py:977`; usado pelo retry e pela porta: `app/folha/porta_export.py:1014`, `app/folha/porta_export.py:1077`. |
   | Pedido `detalhe['estado']` | PEDIDO em `pedir`: `app/folha/porta_export.py:1005`; EM_CURSO em `assumir`: `app/folha/porta_export.py:1032`; demais transições por `_mudar_estado`: `app/folha/porta_export.py:988`. `encerrar`, `vigiar` e falha do disparo chegam por essa função: `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1054`, `app/folha/porta_export.py:1061`, `app/folha/porta_export.py:1141`. | CAS, retry, vigia e decisão da porta: `app/folha/porta_export.py:990`, `app/folha/porta_export.py:1017`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1080`. |
   | Pedido `pedido_em`, demais timestamps, `motivo`, `mes/ano/origem` | Criação, assunção e transições: `app/folha/porta_export.py:1005`, `app/folha/porta_export.py:1032`, `app/folha/porta_export.py:987`. | Vigia lê `pedido_em`: `app/folha/porta_export.py:1057`; apresentação lê `motivo`: `app/folha/porta_export.py:1097`. Não encontrei consumidor funcional dos demais metadados. |
   | Pedido `valor` | Sempre `None` nesses escritores: `app/folha/porta_export.py:1011`, `app/folha/porta_export.py:1031`. | A decisão usa seu estado, não seu valor: `app/folha/porta_export.py:1080`. |
   | `data_ref` | Dia consultado separadamente por `lavrar`, `pedir` e `assumir`: `app/folha/porta_export.py:774`, `app/folha/porta_export.py:1004`, `app/folha/porta_export.py:1028`. | Filtro do pedido e do carimbo: `app/folha/porta_export.py:978`, `app/folha/porta_export.py:1079`. |
   | `data_ini` | Mesmo dia da gravação: `app/folha/porta_export.py:778`, `app/folha/porta_export.py:1010`, `app/folha/porta_export.py:1031`. | Não participa da decisão da porta: `app/folha/porta_export.py:1077`. |
   | `calculado_em` | `auto_now` no modelo e atualização explícita nas transições: `app/inteligencia/models.py:96`, `app/folha/porta_export.py:991`. | Não participa da validade do carimbo ou do prazo do pedido: `app/folha/porta_export.py:1057`, `app/folha/porta_export.py:1078`. |

   Existe leitor genérico `serie(chave, ...)`, sem chamada identificada para essas chaves, e apresentação genérica pelo `__str__`: `app/inteligencia/metricas.py:62`, `app/inteligencia/models.py:113`. O escritor genérico exige chave registrada; não encontrei registro dessas chaves nele: `app/inteligencia/metricas.py:39`.

   **Não encontrei leitor ativo da chave antiga sem competência.** `CHAVE` permanece como prefixo da fábrica; escritor e leitor usam a fábrica. A chave cabe no campo existente e sua unicidade inclui escopo, empresa e dia. (`app/folha/porta_export.py:18`, `app/folha/porta_export.py:920`, `app/folha/porta_export.py:776`, `app/folha/porta_export.py:1078`, `app/inteligencia/models.py:87`, `app/inteligencia/models.py:103`.)

   Nos testes há escritor direto de fixture; o helper compartilhado delega a `lavrar`. Isso não constitui segundo escritor produtivo. (`app/folha/tests/test_porta_do_export.py:40`, `app/folha/porta_export.py:875`.)

2. **Liberação da porta**

   Com os valores produzidos por `medir/lavrar`, não encontrei liberação sem uma linha zero da chave, empresa e `data_ref` consultados. `conferir` libera somente PRONTA. (`app/folha/porta_export.py:764`, `app/folha/porta_export.py:1078`, `app/folha/porta_export.py:1085`, `app/folha/porta_export.py:813`.)

   **Mas FALHOU pode ser encoberto pelo verde anterior:** verde de hoje → nova execução do cron/manual → exceção → pedido FALHOU → PRONTA pelo carimbo anterior. FALHOU só determina FALHA quando não existe carimbo. Reproduzi esse ramo em memória: `conferir` devolveu `''`. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:132`, `app/folha/porta_export.py:1083`, `app/folha/porta_export.py:1090`, `app/folha/porta_export.py:813`.)

   **Há também fragilidade no “de hoje”:** `medir` congela seu instante inicial; `lavrar` consulta o dia novamente ao terminar. Uma medição iniciada antes da meia-noite pode ganhar carimbo de hoje usando teto temporal de ontem. (`app/folha/porta_export.py:318`, `app/folha/porta_export.py:338`, `app/folha/porta_export.py:774`.)

3. **Request e concorrência**

   O request chama `abrir_a_porta`, que pede e dispara o mesmo command por `Popen`; esse caminho não chama `medir/lavrar`. A recusa ocorre antes da montagem do TXT. O cálculo já existente para montar o arquivo continua depois da porta. (`app/folha/views.py:130`, `app/folha/views.py:135`, `app/folha/porta_export.py:1110`, `app/folha/porta_export.py:1138`, `app/folha/views.py:210`.)

   Para **a chamada principal por alvo**, dois processos usando a mesma chave e o mesmo banco não entram simultaneamente em `medir`: a trava antecede assunção, medição, lavra e encerramento; quem perde pula o alvo. Dois cliques também disputam a linha única do pedido. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:122`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:127`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:130`, `app/inteligencia/models.py:103`, `app/folha/porta_export.py:1008`.)

   **A exclusão não cobre toda submedição:** `medir(A)` chama `cartao_x_txt_divergentes`, que percorre todas as empresas ativas. Clique em A e cron medindo B podem medir simultaneamente as mesmas empresas nesse subcontador, apesar das travas distintas. (`app/folha/porta_export.py:667`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:63`.)

   A identidade efetiva do banco/schema entre `saas_ui` e `saas_core` é **[nao verificado]**. O código usa o schema da conexão, sem incorporar nome de container na chave. (`app/folha/porta_export.py:934`, `app/folha/porta_export.py:1128`.)

4. **Estados, vigia e colisões**

   Para processo morto, não encontrei prisão permanente demonstrável: EM_CURSO sem trava vira FALHOU; PEDIDO sem trava expira após 300 segundos, quando consultado. No retry, `pedido_em` é recomposto dinamicamente; não falta timestamp. (`app/folha/porta_export.py:900`, `app/folha/porta_export.py:1054`, `app/folha/porta_export.py:1060`, `app/folha/porta_export.py:1019`, `app/folha/porta_export.py:989`.)

   **Há corrida no vigia:** consulta a trava e depois altera a linha condicionado apenas ao texto do estado. Entre essas operações, outra execução pode assumir a mesma linha como EM_CURSO; o vigia antigo pode marcar FALHOU sobre a execução nova. Se o CAS perde, também não recarrega o objeto, podendo responder EM_CONFERENCIA com estado desatualizado. (`app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1054`, `app/folha/porta_export.py:990`, `app/folha/porta_export.py:992`, `app/folha/porta_export.py:1029`.)

   A consulta de `pg_locks` está coerente com dois int4 válidos: banco corrente, `classid` convertido para unsigned, `objid=k2` positivo e `objsubid=2`. Comportamento contra PostgreSQL em execução: **[nao verificado]**. (`app/folha/porta_export.py:969`, `app/folha/porta_export.py:971`.)

   **A chave tem colisões e limite de faixa:**

   - Mesma empresa/mês em 2000 e 2100 colidem por `ano % 100`; ambos os anos passam pelo sanitizador da view. (`app/folha/porta_export.py:937`, `app/core/sanitiza.py:35`.)
   - CRC32 não garante separação de schemas. Exemplo calculado em memória: `tenant_8r7val5qgvx7` e `tenant_j01vp43zm44n` produzem `3328595997`. Existência desses schemas em produção: **[nao verificado]**. (`app/folha/porta_export.py:935`.)
   - Empresa `214749`, competência 11/2026, produz `k2=2147492611`, fora de int4, sem validação antes do SQL. Existência desse ID em produção: **[nao verificado]**. (`app/folha/porta_export.py:937`, `app/folha/porta_export.py:952`.)

5. **Classes históricas e mordida do selo**

   Não identifiquei juiz duplicado nem segundo escritor produtivo do carimbo. Competência corrente continua delegada a `janela_atual`; a medição continua consultando `classificar_export`, registrado para entrada no TXT. (`app/ponto/services/gestao_he.py:428`, `app/core/juizes.py:345`, `app/folha/porta_export.py:469`, `app/core/juizes.py:330`, `app/folha/porta_export.py:775`.)

   Estão presentes **relógio fora de uma borda comum**, **dia da medição confundido com dia da lavra** e **uso do verde anterior encobrindo falha posterior**. No rollout, o relógio ainda é consultado por empresa. (`app/folha/porta_export.py:318`, `app/folha/porta_export.py:774`, `app/folha/porta_export.py:1083`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:48`, `app/ponto/services/gestao_he.py:427`.)

   **O selo AST morde literais, mas não garante toda mensagem de template.** Reproduzi em memória: detecta `'docker exec ...'` e `'Rode: ...'`; deixa passar `'docker ' + 'exec ...'` e `'Ro' + 'de:'`. Ele examina constantes individuais, não o texto montado. (`app/folha/tests/test_porta_sem_beco.py:371`, `app/folha/tests/test_porta_sem_beco.py:372`.)

   A varredura cobre Python dos apps enumerados, incluindo porta e view, mas exclui HTML, JavaScript e commands. Há fontes reais de texto nesses locais excluídos. (`app/folha/tests/test_porta_sem_beco.py:384`, `app/folha/tests/test_porta_sem_beco.py:388`, `app/folha/tests/test_porta_sem_beco.py:390`, `app/templates/folha/exportar.html:12`, `app/static/pwa.js:357`.)

   Além disso, exceção do command pode virar `motivo`, ser interpolada e chegar às mensagens da tela; o selo não acompanha esse fluxo dinâmico. Vazamento real de texto proibido por esse caminho: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:132`, `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1097`, `app/folha/views.py:135`, `app/core/respostas.py:69`, `app/templates/folha/exportar.html:10`.)

6. **Recorte, soma e SystemExit**

   Para empresa positiva, o command carimba apenas os alvos filtrados; rollout seleciona ativas em rollout. **`--empresa 0`, porém, seleciona todas as ativas**, porque o filtro depende de truthiness; `--rollout --empresa 0` também escapa da rejeição de argumentos incompatíveis. A submedição global continua fora desse recorte. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:45`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:49`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:53`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`.)

   A soma entre competências distintas conta cada `(mes, ano)` uma vez. **Dentro da mesma competência, a última leitura sobrescreve a anterior.** Reproduzi em memória o `handle`: próprias zero, contador global 4 para A e depois 0 para B → carimbos 4 e 0 → saída final SELO VERDE. A identidade por empresa passa nos dois casos. Esse risco preexistente permanece. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:150`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:189`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:226`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:236`.)

   Os testes de soma fixam uma competência; o teste rollout contempla uma empresa. Não demonstram duas competências na mesma execução. (`app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:136`, `app/folha/tests/test_porta_sem_beco.py:224`.)

   `SystemExit` dentro de `medir` escapa de `except Exception`, mas o `finally` solta a trava e permite recuperação pelo vigia. As saídas explícitas posteriores à medição ocorrem depois de CONCLUIDO; portanto não prendem permanentemente a empresa já terminada. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:131`, `app/folha/porta_export.py:956`, `app/folha/porta_export.py:1054`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:136`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:139`.)

## 17/11 11:45 · sessao 4278c4537fa3 · - · - · origem=Ronald (tmux)

Item 2 tem premissa parcial e condições ainda abertas:

- A tarefa responde por **colaborador, dia do turno e instância de marco**; não por uma única pergunta. As instâncias incluem `pergunta_id`, mas a publicação agrega ações e portas do dia inteiro, inclusive `resolver_dia` sem pergunta associada. Falta dizer se basta ação de qualquer assunto daquele dia ou se a ação precisa resolver a pergunta candidata. (`app/chamados/services/tarefa_do_dia.py:733`; `app/chamados/services/tarefa_do_dia.py:284`; `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:640`; `app/chamados/services/tarefa_do_dia.py:684`)
- Não existem apenas os três verbos declarados no complemento: há `perguntar`, `responder`, `validar`, `materializar`, `rejulgar`, `decidir_ausencia`, `entregar_documento`; os donos incluem `sistema`, além de colaborador e admin. A publicação também acrescenta `resolver_dia` e conserva `decidir` residual. (`app/chamados/services/tarefa_do_dia.py:68`; `app/chamados/services/tarefa_do_dia.py:72`; `app/chamados/services/tarefa_do_dia.py:684`; `app/chamados/services/tarefa_do_dia.py:698`)
- Uma ação exigível na instância não garante porta humana alcançável: colaborador precisa constar dos pendentes no app; validação exige âncora utilizável; sistema tem regras próprias de alcance. (`app/chamados/services/tarefa_do_dia.py:651`; `app/chamados/services/tarefa_do_dia.py:654`; `app/chamados/services/tarefa_do_dia.py:660`)
- A tarefa seleciona perguntas `conversa=False` por **`data_evento` dentro da janela**, sem fallback para `celula.data`. Dentro do marco, usa a pergunta mais recente da identidade, após exclusões por via/retratação. Não enumera todas as perguntas C6 vivas como tarefas independentes. (`app/chamados/services/tarefa_do_dia.py:405`; `app/chamados/services/tarefa_do_dia.py:255`; `app/chamados/services/tarefa_do_dia.py:262`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:332`)
- Célula ausente produz `nao_sei=True`, razão `sem_celula`; ausência de lâmpadas e passivo sem linha também podem produzir desconhecimento. Isso não significa “sei que ninguém age”. `nao_sei` pode coexistir com ação e porta admin. (`app/chamados/services/tarefa_do_dia.py:230`; `app/chamados/services/tarefa_do_dia.py:240`; `app/chamados/services/tarefa_do_dia.py:154`; `app/chamados/services/tarefa_do_dia.py:274`; `app/chamados/services/tarefa_do_dia.py:336`; `app/chamados/services/tarefa_do_dia.py:719`)
- `porta_da_tarefa=None` significa ausência conhecida de porta; desconhecimento publica envelope com `rota=None`. O leitor precisa consultar `rota` e `nao_sei`, preservando a distinção. (`app/chamados/services/tarefa_do_dia.py:712`; `app/chamados/services/tarefa_do_dia.py:722`; `app/folha/fora_da_folha.py:86`)
- Há saídas sem tarefa por razões declaradas: fora do vínculo, isenção, competência trancada ou marco ainda não vencido. A ausência de tarefa, isoladamente, não demonstra defeito estrutural. A ocorrência concreta dessas combinações com pergunta C6 viva exige medição [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:77`; `app/chamados/services/tarefa_do_dia.py:211`; `app/chamados/services/tarefa_do_dia.py:214`; `app/chamados/services/tarefa_do_dia.py:221`; `app/chamados/services/tarefa_do_dia.py:234`)
- RAIZ-DA-DISPUTA tem destino documental: fila ESTRUTURA e esqueleto indicado no BACKLOG; o corte BO-DISPUTA-DESENHO-RAIZ encaminha matriz/laudo à F1, sem construir. “Lista nossa” e o endereço concreto do novo brief não foram identificados [nao verificado]. (`app/docs/BACKLOG.md:2055`; `app/docs/CORTES.md:483`)
- L-007 trata de escritor canônico e rastreabilidade; a atribuição ESTRUTURA/CADASTRO/BATIDA está explicitamente em L-099. Portanto, a referência a L-007 não basta para provar que todo caso sem ação é ESTRUTURA. (`app/docs/LEIS.md:65`; `app/docs/LEIS.md:122`)

| Insumo/valor | Quem escreve/deriva | Quem lê |
|---|---|---|
| `DecisaoPergunta.pergunta`, `ato`, `ato_em`, `ato_texto`, `absorcao_ausencia` | Porta única `registrar`, grava novas linhas. Chamadores: resposta, validação/devolução e absorção. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`; `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; `app/chamados/services/materializacao.py:1045`) | Carga ordena por `ato_em,pk`, limita a `agora`; `_pela_decisao` lê a última linha. (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:423`; `app/chamados/services/tarefa_do_dia.py:158`) |
| `Ausencia.status` | `mudar_estado`, escritor único; `decidir` é a porta do ato DP. (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:976`; `app/ponto/services/ausencia.py:1057`) | Escolha entre documento do colaborador, decisão admin e encerramento pela aprovação. (`app/chamados/services/tarefa_do_dia.py:179`; `app/chamados/services/tarefa_do_dia.py:309`) |
| Célula `ata`, `veredito`, `protestos`; lâmpada derivada | `lavrar_veredito` atribui/persiste os campos; leitor da célula transporta `acesa` como `acesa_lavrada`. (`app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1091`; `app/ponto/portas/celula.py:1098`; `app/escala/services/leitor_celula.py:114`; `app/escala/services/leitor_celula.py:137`) | Carga lê `grade_da_celula`; tarefa extrai lâmpadas e instâncias. (`app/chamados/services/tarefa_do_dia.py:544`; `app/chamados/services/tarefa_do_dia.py:108`; `app/chamados/services/tarefa_do_dia.py:125`) |
| Pergunta `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`; grava após consultar juiz de materialização. (`app/chamados/veredito_lavrado.py:254`; `app/chamados/veredito_lavrado.py:270`; `app/chamados/veredito_lavrado.py:275`) | Carga aceita só sim/não ainda vigentes; decisão usa essa leitura. (`app/chamados/services/tarefa_do_dia.py:432`; `app/chamados/services/tarefa_do_dia.py:435`; `app/chamados/services/tarefa_do_dia.py:168`) |
| Chamado `status_local`, `silenciado_ate` | `_gravar_estado`; portas de arquivo atribuem e limpam silêncio. (`app/chamados/models.py:201`; `app/chamados/models.py:228`; `app/chamados/services/arquivo.py:166`; `app/chamados/services/arquivo.py:202`) | `qs_vivos`, `qs_na_fila`, partição de verbos. (`app/chamados/services/painel.py:155`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:44`) |
| `verbo`, `fila_ids`, `fila_cel`, `fila_dia` | Projeções de `particionar_verbos` e `alimentar_fila`; não são novo estado persistido. (`app/chamados/services/verbos.py:47`; `app/chamados/juizes.py:1594`; `app/chamados/juizes.py:1597`; `app/chamados/juizes.py:1608`; `app/chamados/juizes.py:1613`) | Publicação de portas da tarefa. (`app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:650`; `app/chamados/services/tarefa_do_dia.py:665`) |
| `pendentes_app` | `particao_do_app`: resposta sem instante e disputa aberta; carga cruza com C6 no mesmo relógio. (`app/chamados/services/pergunta_no_app.py:76`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/tarefa_do_dia.py:529`; `app/chamados/services/tarefa_do_dia.py:539`) | Publicação da porta do colaborador. (`app/chamados/services/tarefa_do_dia.py:651`) |
| Resultado tarefa: `instancias`, `pergunta_id`, `acoes.{acao,de,porta}`, `nao_sei`, `razao` | `_tarefa_pura` e `_pela_decisao`, derivados; contrato declara zero escritores. (`app/chamados/services/tarefa_do_dia.py:135`; `app/chamados/services/tarefa_do_dia.py:190`; `app/chamados/services/tarefa_do_dia.py:284`; `app/chamados/services/tarefa_do_dia.py:733`) | `_portas_da_tarefa` agrega ações. (`app/chamados/services/tarefa_do_dia.py:637`) |
| Porta: `rota`, `fila`, `verbos`, `fio`, `acao`, `ausencia`, `nao_sei` | `_portas_da_tarefa` publica; `porta_da_tarefa` projeta. (`app/chamados/services/tarefa_do_dia.py:708`; `app/chamados/services/tarefa_do_dia.py:712`) | Folha, Vigia, furo sem canal e beco de cadastro. (`app/folha/fora_da_folha.py:70`; `app/folha/fora_da_folha.py:217`; `app/chamados/services/vigia_fio.py:357`; `app/chamados/services/furo_sem_canal.py:94`; `app/escala/services/cadastro_realidade.py:466`) |

O novo impedimento também precisa preservar o dono no retrato: hoje qualquer motivo diferente de `furo_espelho` recebe `admin`, e motivo sem mapeamento vira `sem_causa`. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:388`)

## 17/11 11:44 · sessao 85be606504fd · - · - · origem=Ronald (tmux)

Achados para os itens 1 e 2, somente pelo código:

- **C6 é `chamados.juizes.pergunta_viva`, não `q_vivas`, disputa aberta ou pergunta não validada isoladamente.** Registro em `app/core/juizes.py:291`; wrapper e memo em `app/chamados/juizes.py:866`; lei em `app/chamados/juizes.py:1403`. `q_vivas` é pré-filtro deliberadamente mais amplo; valor exato exige C6 ou `.viva`. (`app/chamados/models.py:3464`; `app/chamados/models.py:3470`; `app/chamados/models.py:3502`; `app/chamados/models.py:3512`)

- **Vida não exige dia nem célula.** Após os degraus anteriores, `materializada_em is None` devolve `True` mesmo com `data_evento=None` e `celula=None`. Ambos os campos aceitam NULL. Logo “pergunta viva sem dia” é admitida pelo código; não é contradição impossível. (`app/chamados/juizes.py:1482`; `app/chamados/models.py:2519`; `app/chamados/models.py:2528`)

- C6 mata validada, dia trancado e vias terminais; usa resposta com instante **e texto**, carimbo, veredito/lâmpadas da célula, falta e testemunho do fato. Resposta pode conservar vida para o admin, portanto viva não significa apenas “colaborador ainda deve responder”. (`app/chamados/juizes.py:1404`; `app/chamados/juizes.py:1409`; `app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1423`; `app/chamados/juizes.py:1435`; `app/chamados/juizes.py:1454`; `app/chamados/juizes.py:1470`; `app/chamados/juizes.py:1489`)

- **“Dia da pergunta” exige discriminar fato/conversa.** `data_evento` é dia do turno para a porta `abrir`; `abrir_conversa` grava dia civil da conversa, que não tem turno nem competência de fato. O helper usado pelo C6/tranca retorna `None` para conversa e `data_evento` para fato, sem fallback à célula. (`app/chamados/models.py:2888`; `app/chamados/models.py:2900`; `app/chamados/models.py:2906`; `app/chamados/juizes.py:338`; `app/chamados/juizes.py:342`; `app/chamados/juizes.py:995`)

- **O resolvedor do chamado-pai é inadequado ao novo quantificador por pergunta.** Ele descarta conversa, prefere `data_evento`, usa `celula.data` como fallback e devolve o mínimo de todos os fatos. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:79`; `app/chamados/juizes.py:81`; `app/chamados/juizes.py:85`). Trocar unidade para pergunta fecha a ambiguidade de múltiplos fatos se medir cada pergunta individualmente; utilizar aquele mínimo reabre o erro.

- **Supervisão é a entidade `DisputaSupervisao`, relacionada à pergunta por FK; não apenas um rótulo de quem age.** O modelo reúne pessoa, motivos, contexto e chamado-pai; seu nascimento usual abre chamado de categoria/módulo `disputa_supervisao`, área `supervisao`. (`app/chamados/models.py:1774`; `app/chamados/models.py:1800`; `app/chamados/models.py:1813`; `app/chamados/models.py:2521`; `app/chamados/services/disputa_emissao.py:582`; `app/chamados/services/disputa_emissao.py:591`; `app/chamados/services/disputa_emissao.py:601`)

- **C6 não é filtro de `fechada_em`.** O corpo calcula a vida da pergunta sem exigir disputa aberta; `aberta` é outra pergunta, `fechada_em is None`. A existência possível de pergunta viva em disputa fechada está reconhecida e é reparada por reabertura no fluxo de devolução da validação. (`app/chamados/juizes.py:1403`; `app/chamados/models.py:1888`; `app/chamados/services/validacao.py:253`; `app/chamados/services/validacao.py:283`). O aval precisa conservar explicitamente o filtro anterior “disputa aberta” ou declarar sua retirada; o texto do complemento diz que o resto segue, segundo anexo fornecido [nao verificado].

- **C7 continua distinto:** disputa pode fechar se não há resposta sem veredito e não há fio vivo por C6. O juiz de retenção da folha não deve substituir C6, C7 ou tarefa. (`app/chamados/models.py:1928`; `app/chamados/models.py:1934`; `app/chamados/juizes.py:1510`; `app/chamados/juizes.py:1519`)

Escritores/leitores relevantes:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `DisputaSupervisao.colaborador`, `motivos`, `contexto`, `chamado` | Emissão usual cria em `app/chamados/services/disputa_emissao.py:601`; órfãos `:1137`; âncora `:1372`; agregação de motivos/contexto `:550` e `:576`. | Identidade da pergunta lê pessoa da disputa em `app/chamados/models.py:3597`; C6 lê pessoa em `app/chamados/juizes.py:1474`; resolvedor do chamado lê perguntas em `app/chamados/juizes.py:75`. |
| `fechada_em` / aberta | `fechar` grava em `app/chamados/models.py:2013`; `reabrir_admin` zera `:2230`; `reabrir_sistema` zera `:2254`. | Property aberta `app/chamados/models.py:1888`; C6 não o usa em seu corpo `app/chamados/juizes.py:1403`. |
| `data_evento` | Atribuição única `gravar_dia_do_fato`, `app/chamados/services/fio.py:837` e `:847`; nascimento persiste `app/chamados/models.py:3126`; costura `app/chamados/services/fio.py:926` e `:974`; re-chaveamento `:1084` e `:1090`. O dia vem do juiz de turno `app/chamados/services/fio.py:1698`. | C6/tranca pelo helper `app/chamados/juizes.py:338`; resolvedor do pai `:81`; juiz do turno da linha `app/chamados/services/fio.py:1698`. |
| `conversa` | Portas `abrir`/`abrir_conversa`, `app/chamados/models.py:2894` e `:2906`; inserção `:3124`. | Helper do dia `app/chamados/juizes.py:342`; resolvedor do pai `:79`; juiz do turno `app/chamados/services/fio.py:1699`. |
| `celula` | Nascimento `app/chamados/models.py:3124`; re-chaveamento `app/chamados/services/fio.py:1085`; reuso pode reapontar a identidade conforme `app/chamados/models.py:3108`. | C6 `app/chamados/juizes.py:1418` e `:1454`; fallback do pai `:83`. |
| `colaborador`, `instancia_marco`, `batida_origem` da pergunta | Derivação no nascimento por `_chave_de_identidade`, `app/chamados/models.py:3592`, `:3599`, `:3607`, `:3616`; episódio vem do contexto de nascimento `:3620`. | Juiz do turno `app/chamados/services/fio.py:1701` e `:1706`; resolução do dono `:1723`. |
| `resposta_colab`, `respondida_em` | Resposta humana `app/chamados/services/disputa_emissao.py:1481`; automática `:1674`; reparo de instante pela trilha `:1696`; carimbo pode preencher instante com texto em `app/chamados/models.py:3240`; ressuscitação zera o par em `:1555`; lote por `:3362`. | C21 `app/chamados/juizes.py:1544` e `:1552`; C6 lê par projetado em `:1404` e `:1423`. |
| `validada_em` | Validação `app/chamados/services/validacao.py:161` e `:168`; devolução zera `:273`; resposta nova zera `app/chamados/services/disputa_emissao.py:1494`; carimbo `app/chamados/models.py:3245`; ressuscitação `:1554`. | C6 `app/chamados/juizes.py:1405`; projeção temporal `:1389`. |
| `materializada_em`, `via_resolucao` | Atribuição única `_escrever_carimbo`, `app/chamados/models.py:3207`; portas carimbar/identidade/descarimbar explicadas `:3183`; lote `:3362`. | C6 `app/chamados/juizes.py:1416`, `:1444`, `:1450`, `:1482`; projeção temporal `:1390`. |
| Nova resposta única da folha | Endereço, derivador e autoridade novos não determinados pelo anexo fornecido [nao verificado]. | Pretendidos export/PDF segundo aval fornecido [nao verificado]. |

O nascimento atual exige dia e verifica contrato de saída no mesmo atomic, mas isso não elimina a nulabilidade do acervo nem determina destino dos casos sem dia. (`app/chamados/models.py:2892`; `app/chamados/models.py:2915`; `app/chamados/models.py:2923`; `app/chamados/models.py:2528`)

Minha avaliação parcial: item 1 ainda PROCEDE por “dia da pergunta”/sem dia e composição L-033 pendente; o complemento fecha unidade e quais vivas, e evita o mínimo do pai apenas se a aplicação respeitar essa unidade. Item 2 precisa da leitura da tarefa pelo agente correspondente para concluir; “sem ação” não pode ser inferido apenas de C6. (`app/chamados/juizes.py:338`; `app/chamados/juizes.py:1482`; `app/core/juizes.py:304`; composição L-033 [nao verificado])

_6539 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
