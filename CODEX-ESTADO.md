# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 08:42** · publicado em **17/11/2026 08:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 08:42 · sessao e193028b9d06 · ENSAIO-DA-FOLHA-11 · leitura · origem=Code

**A. Export Domínio**

A porta é `/folha/exportar/`; a geração usa **POST `/folha/exportar/gerar/`**, view `gerar_dominio_txt`, com permissão `editar_folha`. (`app/config/urls.py:34`; `app/folha/urls.py:13`; `app/folha/urls.py:14`; `app/folha/views.py:102`)

A cadeia efetiva é:

1. Sanitizar empresa/mês/ano → carregar empresa → `porta_export.conferir` → conferir integração Domínio → exigir algum fechamento gravado `aberto` ou `aprovado`. (`app/folha/views.py:109`; `app/folha/views.py:128`; `app/folha/views.py:138`; `app/folha/views.py:148`)
2. `recalcular_fechamento_mes(... somente_leitura=True, retornar_objeto=True)` → `classificar_export(... fechamentos=_fechs_lido)` → aprovar parte dos fechamentos gravados elegíveis. (`app/folha/views.py:207`; `app/folha/views.py:212`; `app/folha/views.py:243`)
3. `montar_linhas_export(... itens=_itens, fechamentos=_fechs_lido)` → recusar linhas vazias → `validar_layout_dominio` → `conteudo_do_txt` → `nome_canonico` → resposta TXT, auditoria e `ExportacaoDominio.registrar`. (`app/folha/views.py:268`; `app/folha/views.py:270`; `app/folha/views.py:275`; `app/folha/views.py:280`; `app/folha/views.py:285`; `app/folha/views.py:288`; `app/folha/views.py:297`)

**Efeitos e pureza:**

| Função/caminho | Efeito observado no código |
|---|---|
| `porta_export.conferir` | Consulta o carimbo; exige medição de hoje, mesma competência e zero falhas. Não chama `lavrar`, que é o escritor separado do snapshot. (`app/folha/porta_export.py:800`; `app/folha/porta_export.py:768`) |
| `aprovar_lote` | Atualiza `FechamentoMensal`: status, aprovador e data. A view registra `aprovacao_por_geracao`. (`app/ponto/services/fechamento.py:1323`; `app/folha/views.py:250`) |
| `ExportacaoDominio.registrar` | Sempre cria registro com conteúdo, SHA-256, usuário e contagem de linhas; também marca reexportação. (`app/folha/models.py:174`) |
| Auditoria do POST | Grava `export_txt` e, quando houve aprovação, `aprovacao_por_geracao`. (`app/folha/views.py:254`; `app/folha/views.py:289`; `app/core/observ.py:94`) |
| TXT em disco | A view entrega `HttpResponse`; a cópia fica no `TextField` de `ExportacaoDominio`, sem salvamento explícito de TXT em disco nessa cadeia. (`app/folha/views.py:285`; `app/folha/views.py:297`; `app/folha/models.py:86`) |
| Recalculador em leitura | Evita salvar fechamento, mas contém emissão de logs mesmo nesse modo; o logger `ponto` está configurado para arquivo. (`app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:845`; `app/ponto/services/fechamento.py:987`; `app/config/settings/saas.py:160`; `app/config/settings/saas.py:181`) |
| Juiz/montador | Consultam banco e células; alcançam cache de catálogo em RAM e logging condicional da resolução de convenção. Portanto não são estritamente puros. (`app/folha/export.py:740`; `app/folha/export.py:1247`; `app/ponto/catalogo/ausencias.py:267`; `app/core/regua_cct.py:269`) |
| Transformações puras | `montar_linha_10`, `montar_bloco_falta`, `partes_da_linha`, `validar_layout_dominio`, `agregar_export_dominio`, `conteudo_do_txt` e `hash_do_txt` operam sobre entradas já fornecidas. (`app/folha/export.py:34`; `app/folha/export.py:215`; `app/folha/export.py:224`; `app/folha/export.py:266`; `app/folha/export.py:310`; `app/folha/export.py:525`; `app/folha/export.py:542`) |

**Conteúdo e hash sem gravar:** existe a composição `montar_linhas_export(empresa, 11, 2026)` → `conteudo_do_txt(linhas)` → `hash_do_txt(conteudo)`, sem registrar exportação nem aprovar fechamento. O conteúdo usa CRLF, inclusive no final; lista vazia vira `''`, cujo SHA-256 também é calculável. (`app/folha/export.py:690`; `app/folha/export.py:746`; `app/folha/export.py:539`; `app/folha/export.py:553`)

**Mas ela não cumpre uma garantia literal de zero escrita incluindo cache/log:** o montador alcança `vigente()`, que atualiza cache em RAM quando frio ou vencido; a consulta de convenção pode emitir warning. Somente `conteudo_do_txt` + `hash_do_txt`, recebendo linhas prontas, é a transformação pura. Por esse critério literal, A recebe **“nao existe”** no mapa. (`app/folha/export.py:740`; `app/folha/export.py:185`; `app/ponto/catalogo/ausencias.py:331`; `app/ponto/catalogo/ausencias.py:276`; `app/core/regua_cct.py:269`; `app/folha/export.py:539`; `app/folha/export.py:553`)

**Competência aberta e fechamento:**

- O montador padrão lê `FechamentoMensal` existente e aceita `aberto` e `aprovado`; não exige aprovação prévia. Sem fechamentos, retorna linhas vazias. A porta HTTP recusa explicitamente quando não encontra nenhum fechamento nesses dois status. (`app/folha/export.py:687`; `app/folha/export.py:714`; `app/folha/export.py:719`; `app/folha/views.py:163`)
- Há parâmetro `fechamentos=` para objetos fornecidos pelo chamador; o admin usa objetos recalculados em leitura, enquanto a chamada padrão lê os gravados. Portanto são autoridades distintas para os valores. (`app/folha/export.py:700`; `app/folha/views.py:207`; `app/folha/views.py:268`)
- Para corte cadastrado igual a 21, novembro corresponde a **21/10–20/11/2026**; o corte é por empresa, com default 21 e limite de 2 a 28. O corte efetivo das empresas do banco de ensaio está **[nao verificado]**. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:54`)

**Quem decide e quais motivos devolve:**

- **`classificar_export` decide quem entra**, nesta precedência: `rescisao_modulo_proprio` → `sem_codigo_dominio` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → entrada com `motivo=None`. (`app/folha/export.py:765`; `app/folha/export.py:842`; `app/folha/export.py:866`; `app/folha/export.py:959`; `app/folha/export.py:968`; `app/folha/export.py:974`)
- Separadamente, devolve `travas_aprovacao`: `conta_vazia`, `turno_aberto`, `sem_ancora_12x36`. Essas travas afetam o carimbo de aprovação; não substituem a decisão de entrada. O carimbo do TXT ignora `conta_vazia`. (`app/folha/export.py:830`; `app/folha/export.py:848`; `app/folha/export.py:762`; `app/folha/views.py:243`)
- **`motivos_retencao_celula` decide a retenção pelo espelho**; a função compatível `motivos_retencao_espelho` ignora os três primeiros argumentos e delega a ela. Isento e desligado retornam vazio. (`app/folha/export.py:1301`; `app/folha/export.py:1205`)
- Seu vocabulário é `sem_celula`, `espelho_furo`, `espelho_discordante`, `espelho_indefinida`, `espelho_cobranca_indevida`, `espelho_cobrado`, `espelho_nunca_bateu`, `espelho_fato_em_ausencia`. Há exceções para protestos integralmente não cobráveis e furo somente de pausa. (`app/folha/export.py:1272`; `app/folha/export.py:1285`; `app/folha/export.py:1294`; `app/escala/models.py:1767`)
- **`prontidao` é indicador**, com `{pct, trab, previsto, cego, motivo}`; motivos `isento`, `rescisao`, `sem_escala` ou `None`. Possui memo de janela em RAM e não participa da cadeia decisória do montador. (`app/folha/export.py:1025`; `app/folha/export.py:1037`; `app/folha/export.py:1063`; `app/folha/export.py:1108`; `app/folha/export.py:714`)
- Os retornos não trazem responsável estruturado `admin`/`colaborador`/`sistema`. Há `detalhe` com orientação textual, como cadastrar vínculo/escala e recalcular, mas sem campo de atribuição de ação. (`app/folha/export.py:837`; `app/folha/export.py:961`; `app/folha/export.py:1259`)

**Conferências e rollout:**

- `validar_layout_dominio` confere tamanho 43 do reg10, constante `11`, campos numéricos e quantidade de reg11 da rubrica 8792; reg11 alcançado individualmente tem tamanho, flag e data conferidos. **Limitação:** os filhos reg11 contados sob 8792 são pulados por `i=j`, sem passar pela validação individual de data/formato. (`app/folha/export.py:266`; `app/folha/export.py:280`; `app/folha/export.py:291`; `app/folha/export.py:294`)
- `agregar_export_dominio` soma valores inteiros crus por matrícula/rubrica; ignora registros incompatíveis. Não compara com fechamento, não converte horas por 100 e não separa competência/empresa na chave. (`app/folha/export.py:310`)
- “Em rollout” lê-se pelo booleano `Empresa.em_rollout`, default `False`. `colaboradores_em_operacao` acrescenta empresa em rollout e vínculo vigente ao universo de colaboradores comuns. O dropdown do export usa `empresas_visiveis`: ativas, excluindo CNPJ `11222333*`, sem exigir rollout. (`app/colaboradores/models.py:45`; `app/colaboradores/queries.py:50`; `app/colaboradores/queries.py:78`; `app/folha/views.py:89`)

**B. Espelho-ponto em lote**

A porta é **`/relatorios/espelho/lote/`**, view `espelho_lote`, com login e verificação de staff. GET mostra formulário; POST pede empresa/competência ou datas livres e devolve HTML de progresso. O download final é **`/relatorios/espelho/lote/<id>/baixar/`**, view `baixar_lote_espelho`, com resposta `application/pdf`. (`app/config/urls.py:30`; `app/relatorios/urls.py:29`; `app/relatorios/urls.py:33`; `app/relatorios/views.py:1151`; `app/relatorios/views.py:1255`; `app/relatorios/views.py:1311`)

A cadeia é `espelho_lote` → `pedir` → `disparar` → command `gerar_cartoes_em_lote` → `assumir`/`executar` → **`gerar_pdf_espelho_lote_bytes`**. O lote produz PDF único, em ordem alfabética, com espelhos por colaborador. (`app/relatorios/views.py:1227`; `app/relatorios/views.py:1246`; `app/relatorios/lote_espelho.py:127`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:52`; `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`; `app/relatorios/lote_espelho.py:205`; `app/relatorios/services.py:165`)

**Há escritas em várias camadas:**

- Pedido cria `LoteEspelho`; o clique grava `cartao_lote`, ou `cartao_lote_dedup` para pedido repetido vivo. (`app/relatorios/lote_espelho.py:103`; `app/relatorios/views.py:1238`; `app/relatorios/views.py:1251`)
- Disparo cria diretório/log em mídia, escreve o log, inicia subprocesso e atualiza PID/início. (`app/relatorios/lote_espelho.py:124`; `app/relatorios/lote_espelho.py:130`; `app/relatorios/lote_espelho.py:132`; `app/relatorios/lote_espelho.py:138`)
- Execução atualiza progresso, salva PDF em `cartoes_lote/`, atualiza resultado e registra evento final. Até o polling pode gravar estado de erro pelo vigia de processo morto. (`app/relatorios/lote_espelho.py:203`; `app/relatorios/lote_espelho.py:250`; `app/relatorios/lote_espelho.py:254`; `app/relatorios/models.py:125`; `app/relatorios/views.py:1272`; `app/relatorios/lote_espelho.py:75`)

**Fora do HTTP:** o gerador aceita `request=None`, mas também exige `user`. No modo competência chama `gerar_pdf_espelho` → `gerar_pdf_espelho_bytes` → `gerar_pdf_auditavel`, que cria `RegistroPDFAuditavel`. Portanto, **não basta chamá-lo diretamente para obter geração sem escrita**. (`app/relatorios/services.py:165`; `app/relatorios/services.py:235`; `app/relatorios/services.py:70`; `app/relatorios/pdf_espelho.py:1103`; `app/relatorios/pdf_base.py:152`)

Com **ambas as datas explícitas**, há outro ramo que evita o auditável e gera PDF informacional em `BytesIO`. Ele exige parâmetros adicionais e não estabelece garantia estrita de zero cache/log nos serviços transitivos. Se a tentativa auditável falhar, o lote também usa esse informacional como fallback. (`app/relatorios/services.py:211`; `app/relatorios/services.py:235`; `app/relatorios/services.py:244`; `app/relatorios/services.py:250`; `app/relatorios/pdf_espelho.py:1056`; `app/ponto/services/espelho.py:557`; `app/ponto/catalogo/ausencias.py:267`)

**O papel por colaborador contém:**

- Nome, matrícula **Hasner**, CPF, admissão, empresa, posto, cargo, escala com código/ciclo/horários e carimbo da convenção. **PIS não é impresso no cabeçalho**, embora exista no cadastro. (`app/relatorios/pdf_espelho.py:731`; `app/relatorios/pdf_espelho.py:738`; `app/relatorios/pdf_espelho.py:743`; `app/colaboradores/models.py:335`)
- Totais: trabalhadas; HE 50%/100% ou HE genérica; adicional noturno; atraso; saída antecipada; intrajornada; dias de abono, falta e em aberto; saldo mensal de banco quando diferente de zero. (`app/relatorios/pdf_espelho.py:766`; `app/relatorios/pdf_espelho.py:773`; `app/relatorios/pdf_espelho.py:782`; `app/relatorios/pdf_espelho.py:791`; `app/relatorios/pdf_espelho.py:815`)
- Grade diária: previsto, marcações, motivo, trabalhado pago e colunas condicionais NOTURNO, ATRASO, S.ANT, HE50, HE100 e INTRAJ; inclui marcações espúrias e informação de procedência. (`app/relatorios/pdf_espelho.py:844`; `app/relatorios/pdf_espelho.py:857`; `app/relatorios/pdf_espelho.py:885`; `app/relatorios/pdf_espelho.py:950`; `app/relatorios/pdf_espelho.py:976`)

Para competência aberta, o coletor limita os dias até ontem; selecionar novembro não implica imprimir jornadas futuras até 20/11. O lote inteiro inclui, por padrão, situações `ativo`, `afastado` e `ferias`; ausência de batidas é informativo e não elimina o colaborador. (`app/relatorios/pdf_espelho.py:287`; `app/relatorios/services.py:102`; `app/relatorios/services.py:165`; `app/relatorios/services.py:228`)

**O trabalho longo vai para job.** A configuração tem `gthread`, quatro threads e timeout 30; a própria view documenta que esse timeout não limita a duração de uma requisição longa e desloca o caderno para processo separado. (`app/gunicorn.conf.py:19`; `app/gunicorn.conf.py:21`; `app/relatorios/views.py:1216`; `app/relatorios/lote_espelho.py:132`)

**C. Casamento sem nome/CPF**

O PDF imprime **`matricula_hasner`**; o TXT usa **`codigo_dominio.strip().zfill(10)`**, no campo matrícula, posições Python `[2:12]`. São campos distintos: não há chave técnica comum direta impressa nos dois artefatos. (`app/relatorios/pdf_espelho.py:739`; `app/folha/export.py:728`; `app/folha/export.py:251`; `app/colaboradores/models.py:302`; `app/colaboradores/models.py:311`)

A ponte por leitura de cadastro é **matrícula Hasner → Colaborador → código Domínio → matrícula TXT**, preservando o contexto da empresa. A matrícula Hasner é única e anulável; `codigo_dominio` não declara unicidade. Igualdade, preenchimento e duplicidades reais no banco de ensaio: **[nao verificado]**. (`app/colaboradores/models.py:302`; `app/colaboradores/models.py:311`; `app/colaboradores/models.py:321`; `app/folha/export.py:728`)

MAPA: A nao existe; B nao

## 17/11 08:41 · sessao 79086f9ebfb3 · COMPLEMENTO-2-QUEIMA-1711 · verificador · origem=Code

**AVAL-DEVOLVIDO — por item:** a porta lateral exigida pelo item 2 não está disponível nas portas examinadas; no item 5, faltam fechar atividade real e idade da reserva. (`bin/sombra.sh:67-80,255-260`; `bin/sonda_leitura.sh:63-70,93-97`; `bin/codex_limite_estado.py:82-88`; `fila_reserva.md:3-10`, literal)

Referências aos anexos contam linhas desde a primeira linha de cada literal fornecido.

**Item 2**

**1. As premissas conferem? Parcialmente.**

- **Banco lateral:** há previsão de `juliani_ref_*` dentro de `sombra_db`, mas isso não constitui uma porta pronta de restore histórico. `sombra.sh` fixa `ALVO=sombra` e executa drop/create/restore nesse alvo; a sonda escolhe apenas produção ou sombra. O settings da sombra também fixa nome e servidor. (`CLAUDE.md:205-206`; `bin/sombra.sh:67-80,236-260`; `bin/sonda_leitura.sh:63-70,93-97`; `app/config/settings/sombra.py:29,47-50`)
- **Tabela no dump:** o backup inclui o schema inteiro; `folha` pertence aos aplicativos do tenant e sua migration cria `RetratoFolha`. Isso sustenta a inclusão **se a tabela existia naquele schema quando o dump foi produzido**. Existência, integridade, tabela e retratos efetivos dos dumps de 03/11 e 10/11: **[nao verificado]**. (`scripts/backup_tenant.sh:3-12`; `app/config/settings/saas.py:34-45`; `app/folha/migrations/0011_retratofolha.py:15-29`)
- **Leitura versus derivação:** a consulta usa `linhas` gravadas e não chama o motor nem o lavrador. Entretanto, executa os leitores atuais: `dono_da_linha` computa o balde exibido mediante fio gravado, lookup e teto temporal; para legado sem fio válido, há interpretação por `_fio_cru`. É leitor canônico do painel, não uma coluna histórica de balde simplesmente copiada. (`smk_b_retrato.py:7-20`, literal; `app/folha/retrato_folha.py:351-376,436-449,468-493`)
- **“Divisão do painel”:** na consulta fornecida, corresponde ao eixo `dono`: Admin, Colab, Hoje, Sem ação disponível e Sem leitura (sistema). `chamado` entra em Admin; a chave JSON `dono` guarda o fio. A consulta imprime também o eixo separado `tipo`. (`smk_b_retrato.py:19-25`, literal; `app/folha/retrato_folha.py:170-201`)

**2. Colide com regra vigente?**

O desenho lateral não colide por si: bancos laterais no servidor de ensaio já estão previstos. Executá-lo por `sombra.sh --refazer`, porém, substituiria justamente a sombra excluída pelo aval; `scripts/restore_tenant.sh` também não serve como alternativa, pois é expressamente vedado pela regra. (`CLAUDE.md:205-206`; `bin/sombra.sh:255-260`; `complemento2_queima_util_1711.txt:3,7`, literal)

PostgreSQL e leitor devem respeitar o CPUSET de teste; dado pessoal não pode ser exportado, e a sessão principal não pode fazer a sonda inline. Medir espaço, registrar insuficiência e esperar disputa de disco/CPU, sem pedir a trava, são condições adicionais expressas, ainda sem execução demonstrada: **[nao verificado]**. Uma porta nova, se construída como instrumento, exige contrato prévio. (`CLAUDE.md:87,206,344-348,357-365,394`; `bin/sombra_db.sh:91-98`; `complemento2_queima_util_1711.txt:5`, literal)

**3. Que condições não fecham no literal?**

- **“Mesma consulta” não recupera automaticamente a tela histórica.** O código atual interpreta linhas antigas; `hoje=timezone.localdate()` participa do balde Hoje, e `em_rollout` será lido de cada banco. Portanto, referência temporal e universo de empresas também podem variar. A ressalva do item 3 deve acompanhar a comparação, sem atribuição causal à obra. (`smk_b_retrato.py:9,11,16-20`, literal; `app/folha/retrato_folha.py:489-493`; `complemento2_queima_util_1711.txt:4`, literal)
- **Schema antigo:** compatibilidade física com o ORM atual e formato efetivo do JSON são **[nao verificado]**. O modelo documenta evolução de chaves sem migration e leitura de legado; isso não prova compatibilidade dos dois dumps concretos. (`app/folha/models.py:304-323,334-341`)
- **“Sem retrato” versus zero:** a consulta não imprime uma empresa sem retrato e inicializa totais que podem terminar em zero; `linhas_do_retrato(None)` também retorna lista vazia. É necessário distinguir presença por empresa/competência antes de totalizar. Falha de schema tampouco prova “sem retrato”. (`smk_b_retrato.py:13-16,30-34`, literal; `app/folha/retrato_folha.py:98-111`; `complemento2_queima_util_1711.txt:3`, literal)
- **Timestamp e descarte:** `lavrado_em` significa início da rodada do lavrador. Apagar o lateral ao fim está autorizado, mas encerramento com alvo protegido, inclusive em falha/interrupção, não foi demonstrado: **[nao verificado]**. Os anexos fornecem a consulta, sem os resultados numéricos das três datas: **[nao verificado]**. (`app/folha/models.py:356-360`; `smk_b_retrato.py:1-34`, literal; `complemento2_queima_util_1711.txt:3-4`, literal)

**4. Toca exportada, dinheiro ou `!` fora do declarado?**

O escopo autoriza restaurar e descartar o lateral e ler retratos existentes; não autoriza alterar produção, gravado exportado ou recalcular folha. Mantido esse limite, não há movimento de dinheiro/exportada declarado. Cumprimento na execução futura: **[nao verificado]**. (`complemento2_queima_util_1711.txt:3-5,7`, literal; `CLAUDE.md:265-268,411-413`)

**Item 5**

**1. As premissas conferem? Parcialmente.**

- **Publicação:** `relato.sh` gera `ESTADO.md`, passa pela raspagem, copia ao repositório definido por `DEST_RELATO` — padrão `/home/ronald/hasner-relato` — e faz commit/push para `origin HEAD`. O destino remoto concreto da instalação é **[nao verificado]**. (`bin/relato.sh:2,23,37-38,114-117,145-147`)
- **“Em curso”:** o pareamento atual usa `(obra, hash, pid)` e chama de “em curso” um `ini` sem `fim`, sem conferir processo vivo. O PID registrado é `$$`, do wrapper; `ini` antecede espera por slot, trava, possíveis recusas e execução do Codex. O código reconhece que isso registra tentativa, sem provar execução. (`bin/codex_limite_estado.py:63-67,82-88`; `bin/codex_pergunta.sh:272-273,308-309,364-394,425-426`; `bin/disjuntor.py:2071-2075`)
- **Fila:** o literal contém zero ABERTOS: R1 DESPACHADO e R2–R7 ENTREGUE. Há horários junto aos estados, mas nenhuma evidência de que cada abertura receba data/hora própria. Essa premissa permanece **[nao verificado]**. (`fila_reserva.md:3-10`, literal)

**2. Colide com regra vigente?**

A nova redação resolve objeções anteriores: substitui “maior intervalo”, fixa a foto, o predicado do alarme, ausência/ilegibilidade como SEM DADO, ausência de despacho automático e desligamento por Ronald. Não revoga CODEX-NAO-FICA-OCIOSO nem os cortes de orçamento. (`complemento2_queima_util_1711.txt:6,10`, literal; `fila_reserva.md:1-2`, literal; `bin/disjuntor.py:1468-1485`)

Continuam exigidos contrato antes da construção, preservação dos indicadores existentes e pouso próprio de instrumento. Já existe produtor da linha Codex; contrato efetivamente registrado para esta alteração: **[nao verificado]**. (`bin/gerar_estado.py:267-270,337-339`; `CLAUDE.md:327-328,389-394`; `app/docs/PISTA.md:170-176`)

**3. Que condições não fecham no literal?**

- **Atividade e órfão:** falta fechar se espera do wrapper conta como chamada em curso. `fim` depende do trap EXIT e de gravação no log; um processo morto sem encerramento registrado continuaria “em curso” no leitor atual. Um ABERTO de 45 minutos mais esse órfão esconderia o alarme exigido. Ocorrência real desse órfão: **[nao verificado]**. (`bin/codex_pergunta.sh:269-282,364-394`; `bin/codex_limite_estado.py:82-88`; `complemento2_queima_util_1711.txt:6`, literal)
- **Hora do ABERTO:** é necessário um instante de abertura com data e hora; horário do estado exibido não comprova abertura. ABERTO sem hora deixa a idade **SEM DADO**, sem permitir idade zero ou verde. Com nenhum ABERTO, idade é não aplicável. (`fila_reserva.md:3-10`, literal; `CLAUDE.md:35-38`; `complemento2_queima_util_1711.txt:6`, literal) [nao verificado]
- **Instante da foto:** gerar ESTADO ocorre antes das demais etapas e do push. É preciso fixar a coleta em relação à publicação; essas etapas não constituem uma leitura atômica. (`bin/relato.sh:38-42,97-117,145-147`)
- **Borda das 24 h:** a janela pertence ao contexto de inícios por hora; uma chamada ainda viva pode ter começado antes dela. Faltam explicitar limites e agrupamentos horários. O leitor atual aceita a borda exata e timestamps futuros porque testa apenas `agora-t <= 24h`; arquivo ausente vira listas vazias, incompatível com reutilização direta para SEM DADO. (`bin/codex_limite_estado.py:49-78`; `complemento2_queima_util_1711.txt:6`, literal)
- **Mordida:** com dados válidos, ABERTO de 45 minutos e nenhuma chamada acusa; com uma chamada realmente em curso, não acusa; exatamente 30 minutos não satisfaz `>30`. A amostra tem vinte pares completos `ini/fim`, mas é uma cauda, sem prova da foto na publicação nem das 24 horas completas: **[nao verificado]**. (`complemento2_queima_util_1711.txt:6`, literal; `chamadas_amostra.log:1-40`, literal)

**4. Toca exportada, dinheiro ou `!` fora do declarado?**

O escopo mede log/fila e publica agregados; não declara alteração de dinheiro ou competência exportada. A publicação passa pelo fluxo público de raspagem, sem autorização para publicar fontes brutas. Implementação futura respeitando esses limites: **[nao verificado]**. (`complemento2_queima_util_1711.txt:6`, literal; `bin/relato.sh:2,114-117`; `CLAUDE.md:265-268`)

VEREDITO: item 2 PROCEDE -- falta fechar porta lateral nomeada, compatibilidade e ausência distinta de zero (`bin/sombra.sh:67-80,255-260`; `smk_b_retrato.py:13-16,30-34`, literal); item 5 PROCEDE -- ini sem fim não prova atividade e a hora de abertura não está demonstrada (`bin/codex_limite_estado.py:82-88`; `fila_reserva.md:3-10`, literal).

## 17/11 08:41 · sessao b9a945b88925 · - · - · origem=Ronald (tmux)

Auditoria de pureza:

- O caminho `montar_linhas_export` não persiste modelos diretamente, mas chama `datas_falta` para cada fechamento selecionado (`app/folha/export.py:725`, `app/folha/export.py:740`). Essa chamada chega a `descontam_vigentes` (`app/folha/export.py:185`, `app/folha/export.py:180`), que chama `vigente` (`app/ponto/catalogo/ausencias.py:331`). Com cache frio ou vencido, `vigente` escreve `_CACHE['v']` e `_CACHE['t']` em RAM; TTL de 60 segundos (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`, `app/ponto/catalogo/ausencias.py:279`).
- Portanto, a montagem canônica empresa/competência não oferece contrato de **zero escrita incluindo cache**, embora ofereça separação da persistência ORM (`app/folha/export.py:697`, `app/ponto/catalogo/ausencias.py:279`). Sob leitura literal do pedido, recomendo `MAPA: A nao existe`, explicando que existe montagem sem persistência, mas não rigorosamente pura (`app/folha/export.py:740`, `app/ponto/catalogo/ausencias.py:279`).
- `conteudo_do_txt` e `hash_do_txt` são puros para entradas já prontas: CRLF e SHA-256, respectivamente (`app/folha/export.py:539`, `app/folha/export.py:552`). Isso não resolve a seleção/montagem empresa/competência (`app/folha/export.py:714`, `app/folha/export.py:725`).
- Há log condicional transitivo: `montar_linhas_export` chama `feriado_12x36_em_dobra`, que consulta `regua_para`, chegando a `_aplicacao_vigente`; empate entre aplicações vigentes emite `logger.warning` (`app/folha/export.py:732`, `app/folha/export.py:380`, `app/core/regua_cct.py:366`, `app/core/regua_cct.py:269`). Em SaaS esse WARNING chega ao console; o handler de arquivo da raiz aceita ERROR (`app/config/settings/saas.py:155`, `app/config/settings/saas.py:137`, `app/config/settings/saas.py:183`).
- `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)` constrói objetos destacados e coleta retorno, evitando o `save/update` de FM (`app/ponto/services/fechamento.py:876`, `app/ponto/services/fechamento.py:890`, `app/ponto/services/fechamento.py:1128`). Contudo, há INFO condicionais fora da proteção dessa flag e ERROR por exceção (`app/ponto/services/fechamento.py:845`, `app/ponto/services/fechamento.py:987`, `app/ponto/services/fechamento.py:1305`). O logger `ponto` escreve INFO em `/app/logs/ponto.log` (`app/config/settings/saas.py:160`, `app/config/settings/saas.py:181`).

Espelho:

- O lote por competência tenta o auditável por colaborador (`app/relatorios/services.py:235`, `app/relatorios/services.py:237`), cuja cadeia cria `RegistroPDFAuditavel` (`app/relatorios/services.py:70`, `app/relatorios/pdf_espelho.py:1103`, `app/relatorios/pdf_base.py:152`).
- Passar ambas as datas explícitas pula o auditável e chama diretamente o informacional; o resultado é unido em `BytesIO` (`app/relatorios/services.py:211`, `app/relatorios/services.py:235`, `app/relatorios/services.py:246`, `app/relatorios/services.py:250`, `app/relatorios/services.py:262`). Esse é um ramo com argumentos adicionais, e continua sujeito ao cache transitivo e aos logs do motor; não constitui contrato cache/log-zero apenas com empresa/competência (`app/relatorios/services.py:165`, `app/relatorios/pdf_espelho.py:394`, `app/core/regua_cct.py:448`).
- O informacional usa `BytesIO`, `SimpleDocTemplate` e devolve bytes, sem criar registro auditável nesse corpo (`app/relatorios/pdf_espelho.py:1056`, `app/relatorios/pdf_espelho.py:1070`). Ele também consulta `get_commit_git`, que executa somente `git rev-parse HEAD` ou lê variável de ambiente (`app/relatorios/utils_audit.py:12`, `app/relatorios/utils_audit.py:18`).
- O job externo escreve progresso, arquivo, estado final e evento de observabilidade (`app/relatorios/lote_espelho.py:203`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/lote_espelho.py:251`, `app/relatorios/lote_espelho.py:254`).
- `calibrar_ancora_se_aplicavel` está aposentada e retorna zero; não encontrei chamada do gerador para o escritor `recalibrar_ancora_retroativa` (`app/escala/utils.py:209`, `app/escala/utils.py:214`; ausência de chamada: [nao verificado]).

## 17/11 08:40 · sessao 0e8e8e08493b · - · - · origem=Ronald (tmux)

A — evidência consolidada, somente leitura estática:

- A porta está montada sob `/folha/`, com formulário em `exportar/` e POST em `exportar/gerar/`, nomes `folha:exportar` e `folha:gerar_txt` (`app/config/urls.py:34`; `app/folha/urls.py:13-14`). A tela exige `editar_folha`; o gerador também exige essa ação e método POST (`app/folha/views.py:83-106`). A prévia monta o formulário POST quando há elegíveis (`app/folha/views.py:416-430`).
- Cadeia efetiva do POST: sanitiza empresa/mês/ano → carrega Empresa → `porta_export.conferir` → `codigo_no_sistema_dominio` → exige algum `FechamentoMensal` gravado aberto/aprovado → `recalcular_fechamento_mes(... somente_leitura=True, retornar_objeto=True)` → `classificar_export(... fechamentos=_fechs_lido)` → aprova os elegíveis sem travas consideradas → `montar_linhas_export(... itens=_itens, fechamentos=_fechs_lido)` → `validar_layout_dominio` → `conteudo_do_txt` → `nome_canonico` → resposta TXT + auditoria + `ExportacaoDominio.registrar` (`app/folha/views.py:109-166`, `207-268`, `275-301`).
- `conferir` apenas consulta `MetricaSnapshot`; exige carimbo de hoje, da mesma competência, medição válida e zero falhas; devolve mensagem de recusa quando falta ou diverge (`app/folha/porta_export.py:800-867`). `lavrar`, função separada, escreve esse snapshot por `update_or_create` (`app/folha/porta_export.py:768-796`).
- Escritas do POST: `aprovar_lote` faz `qs.update(status='aprovado', aprovado_por, aprovado_em)`; a porta só o chama sobre gravados abertos dos elegíveis sem travas, ignorando `conta_vazia`; registra auditoria `aprovacao_por_geracao` (`app/folha/views.py:243-256`; `app/folha/export.py:762`; `app/ponto/services/fechamento.py:1323-1332`). Registra também auditoria `export_txt` e nova `ExportacaoDominio`, com conteúdo, hash, linhas, usuário e marca de reexportação (`app/folha/views.py:288-300`; `app/folha/models.py:174-188`). `registrar_log` chega a `LogAuditoria.objects.create` (`app/core/auditoria.py:8-25`; `app/core/observ.py:94-138`). O arquivo é entregue como `HttpResponse`; nessa cadeia o conteúdo fica no banco como `TextField`, sem escrita explícita de TXT em disco (`app/folha/views.py:280-301`; `app/folha/models.py:82-86`).
- `montar_linhas_export` não aprova nem registra exportação: devolve `(linhas, entra_set)`; usa integração ativa, rubricas empresa sobre global, juiz de entrada e fechamentos aberto/aprovado (`app/folha/export.py:690-746`, `984-991`). Converte `colaborador.codigo_dominio.strip().zfill(10)` no campo chamado matrícula do TXT, competência `AAAAMM`, eventos com valor não zero e rubrica cadastrada, mais datas de falta (`app/folha/export.py:723-745`).
- Existe composição sem persistência de negócio: `linhas, ids = montar_linhas_export(empresa, 11, 2026)`; `conteudo = conteudo_do_txt(linhas)`; `hash = hash_do_txt(conteudo)` (`app/folha/export.py:690-746`, `525-553`). `conteudo_do_txt` é junção CRLF com CRLF final, ou `''` se vazio; hash é SHA-256 de `conteudo.encode()` (`app/folha/export.py:539`, `552-553`).
- Esse caminho padrão lê os `FechamentoMensal` existentes; sem eles, o juiz e o montador iteram vazio e devolvem linhas vazias, conteúdo `''` e SHA-256 dessa string — não há recusa de competência aberta no montador (`app/folha/export.py:783-788`, `714-746`, `539`, `553`). Aberto é status admitido (`app/folha/export.py:687`, `717-722`). A porta HTTP, por outro lado, recusa se não existir nenhum gravado aberto/aprovado, com “Nenhum fechamento calculado… recalcule a competencia” (`app/folha/views.py:148-166`).
- Para a mesma autoridade atual usada pelo POST, pode-se obter objetos destacados pelo recalculador e injetá-los no juiz/montador; o recalculador semeia a linha gravada se existir, senão constrói objeto `status='aberto'`, sem salvar no ramo de leitura (`app/ponto/services/fechamento.py:876-901`, `1128-1144`; `app/folha/views.py:207-212`, `265-268`). Não chamar isso de função pura: pode emitir logs `info`/`error`, mesmo em leitura (`app/ponto/services/fechamento.py:843-846`, `982-988`, `1303-1307`).
- Também não chamar `classificar_export`/montador de “puros” no sentido estrito: fazem consultas e alcançam cache de catálogo no processo, atualizado em `vigente()`; consulta CCT pode emitir warning quando há aplicações empatadas (`app/folha/export.py:789-791`, `974-978`, `1002-1013`, `363-380`, `740`; `app/ponto/catalogo/ausencias.py:257-280`; `app/core/regua_cct.py:265-274`). Não encontrei escrita persistente de célula nessa composição; os métodos de retenção consultam `CelulaDia` e escalas (`app/folha/export.py:1247-1298`). O agente purity está auditando a cadeia transitiva.
- Puramente locais: `montar_linha_10`, `montar_bloco_falta`, `partes_da_linha`, `validar_layout_dominio`, `agregar_export_dominio`, `rubrica_atraso_parcial`, `eventos_do_fechamento`, `conteudo_do_txt`, `hash_do_txt`, pelas operações de seus corpos (`app/folha/export.py:34-45`, `215-255`, `266-321`, `383-461`, `525-553`). `prontidao` tem memo global `_JANELA_MEMO` no processo, escrito quando resolve a janela, e não participa do juiz/montador (`app/folha/export.py:1022-1025`, `1063-1067`; `765-981`, `690-746`).
- Juiz de quem entra: `classificar_export`, cadeia de motivos efetivos: `rescisao_modulo_proprio` → `sem_codigo_dominio` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → entra (`app/folha/export.py:765-780`, `866-884`, `959-980`). `nao_aprovado` sobrevive na legenda da UI, mas não é emitido pelo juiz atual; o status não é porta do juiz (`app/folha/views.py:14-23`; `app/folha/export.py:851-855`).
- Segunda pergunta do mesmo juiz: `travas_aprovacao`, tupla de `conta_vazia`, `turno_aberto`, `sem_ancora_12x36`; afeta aprovação, separadamente de entrada no TXT (`app/folha/export.py:792-848`; `app/folha/views.py:215-248`).
- Juiz subordinado de retenção do espelho: `motivos_retencao_espelho` exige `fech`, ignora os três primeiros parâmetros e delega a `motivos_retencao_celula` (`app/folha/export.py:1301-1307`). Isento e desligado devolvem vazio; nos demais, motivos são `sem_celula` e `espelho_` + veredito acusador (`app/folha/export.py:1205-1213`, `1264-1298`). Vocabulário acusador: `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia` (`app/escala/models.py:1767-1768`). Protestos todos não cobráveis e furo só de pausa isentam retenção (`app/folha/export.py:1285-1294`, `1173`).
- `prontidao` devolve `{pct, trab, previsto, cego, motivo}` como indicador; motivos `isento`, `rescisao`, `sem_escala`, ou `None`; não decide entrada (`app/folha/export.py:1025-1039`, `1068-1113`; `765-981`).
- O retorno do juiz não contém campo normalizado de “quem age” (`admin`, `colaborador`, `sistema`): item contém IDs/nome/código/status/motivo/detalhe/travas e extras específicos de cada motivo (`app/folha/export.py:837-850`, `866-980`). O retorno diário também contém apenas data, motivo, célula e veredito (`app/folha/export.py:1259-1262`). Há instruções em prosa em `detalhe`, por exemplo cadastrar vínculo/escala e recalcular no `cadastro_zero`, mas sem responsável estruturado (`app/folha/export.py:961-964`).
- `validar_layout_dominio`: reg10 com 43 caracteres; constante `11` nos offsets 22:24; campos numéricos matrícula, competência, rubrica, valor e código de sistema; reg8792 exige número de reg11 consecutivos igual ao valor; reg11 alcançado isoladamente verifica comprimento, dígitos, flag `1` e data válida; outros registros dão erro (`app/folha/export.py:266-307`). Limite concreto: no ramo8792 ele conta reg11 consecutivos e avança `i = j`, portanto pula validação individual de comprimento/data desses filhos (`app/folha/export.py:280-292`). Não valida semanticamente mês/competência, vínculo de rubrica com pessoa nem catálogo nesses testes de layout (`app/folha/export.py:266-307`).
- `agregar_export_dominio` não valida/reconcilia: aceita reg10 com 43 caracteres e valor inteiro, ignora demais e soma inteiros crus por matrícula e rubrica; não divide horas por100 nem agrupa por competência/código de empresa (`app/folha/export.py:310-321`).
- “Empresas em rollout” é campo `Empresa.em_rollout`, booleano default `False` (“Empresa em producao… piloto/rollout”); leitura ORM direta é filtro nesse campo (`app/colaboradores/models.py:40-46`). `colaboradores_em_operacao` significa colaboradores comuns, em empresa com essa flag e vínculo vigente hoje; comuns exige ativo, empresa ativa e username com onze dígitos (`app/colaboradores/queries.py:15-29`, `50-65`). Dropdown de export usa `empresas_visiveis`: ativas, exclui CNPJ11222333*, ordena razão social; não exige rollout (`app/folha/views.py:83-93`; `app/colaboradores/queries.py:78-87`).
- Janela11/2026 corresponde a21/10–20/11 somente com corte21; a função respeita o cadastro de corte por empresa, default21, clamp2–28 (`app/ponto/janelas.py:10-21`, `36-57`; `app/colaboradores/models.py:46`).

## 17/11 08:40 · sessao 863522386e97 · - · - · origem=Ronald (tmux)

B — Porta e cadeia:

- A rota completa é `/relatorios/espelho/lote/`: prefixo em `app/config/urls.py:30`, rota/view `espelho_lote` em `app/relatorios/urls.py:29`. A view exige login e `_requer_staff`; GET entrega formulário, POST lê empresa/mês/ano ou datas livres (`app/relatorios/views.py:1151`, `app/relatorios/views.py:1162`, `app/relatorios/views.py:1169`, `app/relatorios/views.py:1172`, `app/relatorios/views.py:1195`, `app/relatorios/views.py:1212`).
- O POST chama `lote_espelho.pedir(...)`, registra auditoria `cartao_lote` e chama `disparar`; pedido duplicado vivo registra `cartao_lote_dedup` (`app/relatorios/views.py:1227`, `app/relatorios/views.py:1238`, `app/relatorios/views.py:1246`, `app/relatorios/views.py:1251`).
- `pedir` cria `LoteEspelho`; `disparar` cria diretório de mídia, abre/grava `media/cartoes_lote/<id>.log`, inicia `nice -n 10 ... manage.py gerar_cartoes_em_lote <id> --schema ...`, atualiza PID/início e registra evento (`app/relatorios/lote_espelho.py:103`, `app/relatorios/lote_espelho.py:124`, `app/relatorios/lote_espelho.py:127`, `app/relatorios/lote_espelho.py:130`, `app/relatorios/lote_espelho.py:138`, `app/relatorios/lote_espelho.py:140`).
- O command chama `assumir` e `executar`; `assumir` grava estado/PID, `executar` chama o gerador `relatorios.services.gerar_pdf_espelho_lote_bytes`, grava progresso, salva PDF e estado/placar e evento final (`app/relatorios/management/commands/gerar_cartoes_em_lote.py:52`, `app/relatorios/management/commands/gerar_cartoes_em_lote.py:57`, `app/relatorios/lote_espelho.py:161`, `app/relatorios/lote_espelho.py:203`, `app/relatorios/lote_espelho.py:205`, `app/relatorios/lote_espelho.py:250`, `app/relatorios/lote_espelho.py:251`, `app/relatorios/lote_espelho.py:254`). O arquivo usa `FileField(upload_to='cartoes_lote/')` (`app/relatorios/models.py:125`).
- O download final é `/relatorios/espelho/lote/<id>/baixar/`, view `baixar_lote_espelho`, que abre o arquivo pronto em `rb` e responde `application/pdf` como anexo; não chama gerador (`app/relatorios/urls.py:33`, `app/relatorios/views.py:1295`, `app/relatorios/views.py:1308`, `app/relatorios/views.py:1311`). O POST retorna HTML da tela de progresso; o arquivo final é um PDF único (`app/relatorios/views.py:1255`, `app/relatorios/views.py:1274`, `app/relatorios/services.py:167`, `app/relatorios/services.py:262`).

Chamar sem HTTP ≠ chamar sem escrita:

- O gerador aceita `request=None`, mas exige argumento `user`, além de empresa/mês/ano (`app/relatorios/services.py:165`, `app/relatorios/services.py:175`). O próprio job o chama assim (`app/relatorios/lote_espelho.py:205`).
- Na competência padrão, ele chama `gerar_pdf_espelho` → `gerar_pdf_espelho_bytes` → `gerar_pdf_auditavel`, que executa `RegistroPDFAuditavel.objects.create`; portanto **não é gerador sem escrita só com empresa/competência**, mesmo sem criar job nem salvar arquivo (`app/relatorios/services.py:235`, `app/relatorios/services.py:237`, `app/relatorios/services.py:70`, `app/relatorios/pdf_espelho.py:1103`, `app/relatorios/pdf_base.py:152`).
- Se o auditável lança exceção, o lote tenta PDF informacional; só pula colaborador se esse também falhar (`app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/services.py:254`). Ausência de batidas é apenas informativo e não retira do caderno (`app/relatorios/services.py:228`, `app/relatorios/services.py:234`).
- Existe ramo distinto passando **ambas** `data_ini` e `data_fim`: ele evita tentativa auditável e chama diretamente `gerar_pdf_espelho_informacional_bytes`, que constrói PDF em `BytesIO` e retorna bytes (`app/relatorios/services.py:211`, `app/relatorios/services.py:235`, `app/relatorios/services.py:246`, `app/relatorios/services.py:250`, `app/relatorios/pdf_espelho.py:1056`, `app/relatorios/pdf_espelho.py:1070`). Isso não é a chamada “só empresa/competência”; ademais os serviços chamados pelo coletor possuem ramos de `logger.warning`, portanto não prometer silêncio absoluto de logs (`app/relatorios/pdf_espelho.py:598`, `app/ponto/services/espelho.py:557`, `app/ponto/services/espelho.py:570`, `app/ponto/services/espelho.py:651`).
- Não afirmar que ausência de fechamento necessariamente força fallback: embora a docstring mencione isso, o bloqueio implementado do auditável é disputa aberta; o coletor suporta ausência de `FechamentoMensal`, usando lavratura ou estado “sem apuracao ainda” (`app/relatorios/services.py:168`, `app/relatorios/pdf_espelho.py:1084`, `app/relatorios/pdf_espelho.py:1113`, `app/relatorios/cartao_pela_celula.py:71`, `app/relatorios/cartao_pela_celula.py:321`, `app/relatorios/cartao_pela_celula.py:340`).

Conteúdo impresso:

- Nome, matrícula **Hasner**, CPF, admissão, empresa, posto, escala (código/ciclo/horário), cargo e carimbo da convenção (`app/relatorios/pdf_espelho.py:731`, `app/relatorios/pdf_espelho.py:738`, `app/relatorios/pdf_espelho.py:743`). O cabeçalho completo não inclui PIS, `codigo_dominio` nem PK; PIS existe no cadastro (`app/relatorios/pdf_espelho.py:738`, `app/colaboradores/models.py:335`).
- Totais/badges: trabalhadas; HE 50%, HE 100% ou HE genérica quando as faixas não vieram; adicional noturno legal; atraso; saída antecipada; intrajornada; dias de abono/falta; dias em aberto; saldo mensal do banco quando não zero (`app/relatorios/pdf_espelho.py:766`, `app/relatorios/pdf_espelho.py:771`, `app/relatorios/pdf_espelho.py:782`, `app/relatorios/pdf_espelho.py:791`, `app/relatorios/pdf_espelho.py:815`).
- Dia a dia: dia, horário previsto, marcações, motivo, colunas condicionais NOTURNO/ATRASO/S.ANT/HE50/HE100/INTRAJ e trabalhado pago; marcações normais, espúrias com `*` e horários de isenção com `(i)`; palavra do dia e procedência de marcações (`app/relatorios/pdf_espelho.py:844`, `app/relatorios/pdf_espelho.py:857`, `app/relatorios/pdf_espelho.py:865`, `app/relatorios/pdf_espelho.py:881`, `app/relatorios/pdf_espelho.py:889`, `app/relatorios/pdf_espelho.py:923`, `app/relatorios/pdf_espelho.py:950`, `app/relatorios/pdf_espelho.py:976`).
- Na competência, os totais do cartão consultam `fechamento_lido` online; o TXT padrão usa `FechamentoMensal` gravado. Não presumir igualdade apenas pelos comentários de “mesma fonte” (`app/relatorios/cartao_pela_celula.py:73`, `app/folha/export.py:719`).
- O mês/ano resolve janela da competência; o coletor limita os dias até ontem. A validação HTTP que recusa data final hoje/futura ocorre no modo datas livres (`app/relatorios/pdf_espelho.py:661`, `app/relatorios/pdf_espelho.py:287`, `app/relatorios/views.py:1187`, `app/relatorios/services.py:38`). Portanto, para competência ainda aberta, não pressupor PDF até 20/11 com dias futuros preenchidos (`app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:602`).
- O trabalho longo vai para processo/job; configuração tem `worker_class="gthread"`, quatro threads e timeout 30, e a view documenta que esse timeout não encerra requisição longa (`app/relatorios/lote_espelho.py:132`, `app/gunicorn.conf.py:19`, `app/gunicorn.conf.py:21`, `app/relatorios/views.py:1219`).
- Atenção ao polling rotulado “LEITURA”: `_tela_lote_espelho` chama `vigiar`, que pode gravar estado ERRO se processo morreu (`app/relatorios/views.py:1285`, `app/relatorios/views.py:1272`, `app/relatorios/lote_espelho.py:75`).

C — Chave de casamento:

- O espelho imprime `matricula_hasner`; o TXT preenche a “matrícula” usando `codigo_dominio.strip().zfill(10)`, posições 2:12 (`app/relatorios/pdf_espelho.py:739`, `app/folha/export.py:728`, `app/folha/export.py:738`, `app/folha/export.py:251`).
- Esses são campos distintos; `matricula_hasner` é única, mas nullable; `codigo_dominio` não tem `unique=True` (`app/colaboradores/models.py:302`, `app/colaboradores/models.py:311`). Os artefatos, portanto, não apresentam chave técnica comum direta para casamento sem nome/CPF; é necessária ponte de leitura no cadastro: matrícula Hasner → colaborador → código Domínio → matrícula TXT (`app/relatorios/pdf_espelho.py:739`, `app/colaboradores/models.py:302`, `app/colaboradores/models.py:311`, `app/folha/export.py:728`). Igualdade real desses valores no banco de ensaio: **[não verificado]**.

## 17/11 08:39 · sessao da21324c0f35 · - · - · origem=Ronald (tmux)

**Item 2 — evidências para consolidação**

1. **Premissas: parcialmente conferem.**
   - Existe previsão vigente de banco lateral `juliani_ref_*` **dentro de `sombra_db`**, separado de produção; isso não autoriza usar a sombra única como alvo deste item. (`CLAUDE.md:205-206`; `complemento2_queima_util_1711.txt:3,7`, literal)
   - **As portas indicadas não implementam restore para nome próprio:** `sombra.sh` crava `ALVO=sombra`, guarda esse nome e executa `dropdb/createdb/pg_restore` nele; a CLI não oferece alvo nem arquivo histórico específicos. `sonda_leitura.sh` aceita apenas arquivo, opcionalmente `--sombra`, escolhendo entre `saas_hasner` e `sombra`. (`bin/sombra.sh:67-80,236-260,436-495`; `bin/sonda_leitura.sh:63-70,93-97`)
   - Só mudar `POSTGRES_DB` não resolve: `config.settings.sombra` fixa nome `sombra` e host `sombra_db`. (`app/config/settings/sombra.py:29,47-50`)
   - O produtor de backup faz dump de schema inteiro, sem filtro de tabela; `folha` está nos `TENANT_APPS`, e a migration cria `RetratoFolha`, com `mes/ano/linhas/lavrado_em/empresa`. Portanto, um dump íntegro de `juliani` feito depois da aplicação dessa migration **deve** carregar a tabela. Existência, integridade, schema e conteúdo efetivos dos dumps de 03/11 e 10/11: **[nao verificado]**. (`scripts/backup_tenant.sh:3-12`; `app/config/settings/saas.py:34-45`; `app/folha/migrations/0011_retratofolha.py:15-29`)
   - `smk_b_retrato.py` lê o retrato gravado, filtra as empresas `em_rollout=True`, recorta bloqueados e agrupa em memória; não chama `montar_linhas`, `fora_da_folha` nem `classificar_export`. (`smk_b_retrato.py:7-20`, literal; `app/folha/retrato_folha.py:89-122,674-709`)
   - **`dono_da_linha` é o leitor canônico do eixo**, com lookup do fio gravado e teto temporal; não relavra nem consulta motor de cálculo. Para linha legada sem `dono`, usa `_fio_cru(status,motivo,None)`, interpretação prevista do gravado antigo. (`app/folha/retrato_folha.py:351-376,436-449,468-493`; `app/folha/models.py:315-323`)
   - “Divisão do painel”, se for a usada em `DONO(retrato)` pela consulta-base, corresponde a `Admin`, `Colab`, `Hoje`, `Sem ação disponível`, `Sem leitura (sistema)`; a chave JSON `dono` guarda **fio**, não esse balde da UI. A consulta também imprime outro eixo, `TIPO`; não é divisão organizacional cadastrada. (`smk_b_retrato.py:19-25`, literal; `app/folha/retrato_folha.py:167-201,588-597`)

2. **Colisão: não necessária para o desenho lateral; existe se tentarem encaixá-lo na porta errada.**
   - O banco nomeado deve ficar no servidor de ensaio, com lado PostgreSQL e Python no cpuset teste; os limites são `0-3` produção e `4-7` teste, e `sombra_db` nasce/é ajustado nesse cpuset. (`CLAUDE.md:87,205`; `bin/recursos.sh:40-56,81`; `bin/sombra_db.sh:91-98`)
   - Usar `sombra.sh --refazer` apagaria/substituiria a sombra única, contrariando o item; usar `scripts/restore_tenant.sh` restaura no banco do compose com `--clean` e é expressamente proibido pela regra da sombra. (`bin/sombra.sh:255-260`; `scripts/restore_tenant.sh:8`; `CLAUDE.md:206`; `complemento2_queima_util_1711.txt:3,7`, literal)
   - Dado pessoal não pode ser exportado; a sonda e a contagem seguem pelo Codex, sem sonda inline da sessão principal. O wrapper exige saída raspada antes do Codex. (`CLAUDE.md:206,344-350,357-365`)
   - O item exige medir espaço antes, não restaurar se faltar e esperar conflito de disco/CPU com a espinha, sem pedir a trava da sombra; implementação concreta desse preflight e dessa espera: **[nao verificado]**. (`complemento2_queima_util_1711.txt:5`, literal)

3. **Condições literais que não fecham ainda.**
   - “Mesma consulta” produz **leitura atual de retratos antigos**, não a antiga tela histórica: `hoje=timezone.localdate()` participa do balde `Hoje`, e a seleção `em_rollout` será a de cada banco consultado. Assim, variam dados, código de leitura, referência temporal e possivelmente universo de empresas; a ressalva do item 3 deve acompanhar o resultado e impedir atribuição causal à obra. (`smk_b_retrato.py:9,11,16-20`, literal; `app/folha/retrato_folha.py:489-493`; `complemento2_queima_util_1711.txt:4`, literal)
   - Schema físico histórico compatível com o ORM atual e formato real do JSON nos dois dumps: **[nao verificado]**. Chaves `dono/tipo/saldo/datas` evoluíram sem migration, e o leitor admite legado; isso não prova a compatibilidade efetiva de cada arquivo. (`app/folha/models.py:304-323,334-341`; `app/folha/retrato_folha.py:436-449,496-503`)
   - **A consulta-base não distingue sozinha “sem retrato” de zero:** o `for` não imprime empresa sem retrato; contadores globais começam vazios e acabam imprimindo zero. `linhas_do_retrato(None)` também devolve `[]`. Precisa conferir presença por empresa/competência antes de totalizar; ausência não pode virar zero. Tabela ausente/incompatível tampouco demonstra ausência de retrato. (`smk_b_retrato.py:13-16,30-34`, literal; `app/folha/retrato_folha.py:98-111`; `complemento2_queima_util_1711.txt:3`, literal)
   - O timestamp pedido existe e é impresso, mas significa **início da rodada do lavrador**, não instante de `save`; compará-lo com data do dump exige manter essa distinção. (`smk_b_retrato.py:23`, literal; `app/folha/models.py:356-360`)
   - “Apagar ao fim” é expresso no aval, porém falta porta nomeada com guarda de alvo e encerramento garantido também em falha/interrupção; as portas disponíveis apagam a própria sombra no começo do refazer. (`complemento2_queima_util_1711.txt:3`, literal; `bin/sombra.sh:67-80,255-260`)

4. **Exportada, dinheiro ou `!`:**
   - O escopo declarado é restauração lateral, leitura e descarte do banco criado; não manda alterar `FechamentoMensal`, exportação nem regra de cálculo. Se mantido nesse limite, não exige `!` por dinheiro/exportada. Se atingir produção, gravado exportado, escala/vínculo ou arquivo usado por produção, cai nas proibições vigentes. Execução futura respeitando esses limites: **[nao verificado]**. (`complemento2_queima_util_1711.txt:3-5,7`, literal; `CLAUDE.md:265-268`)

**Recomendação:** item 2 **PROCEDE / AVAL-DEVOLVIDO** para fechar porta lateral nomeada, compatibilidade dos dumps, detecção explícita de ausência e descarte seguro; o leitor `dono_da_linha` não é motivo para reprovar como motor de cálculo. (`bin/sombra.sh:67-80,255-260`; `bin/sonda_leitura.sh:63-70,93-97`; `app/folha/retrato_folha.py:468-493`; `CLAUDE.md:35-38`)

## 17/11 08:39 · sessao 89861552b2f3 · - · - · origem=Ronald (tmux)

**Item 5 — PROCEDE: permanecem lacunas operacionais na definição da foto, atividade e idade da reserva.** O literal já substitui “maior intervalo”, estabelece o predicado do alarme, falha por ausência/ilegibilidade, ausência de despacho e quem desliga; essas objeções anteriores foram respondidas. (`complemento2_queima_util_1711.txt:6`, literal)

1. **Premissas.** `relato.sh` gera `ESTADO.md`, raspa nomes/CPF, copia ao repositório público definido por `DEST_RELATO` ou `/home/ronald/hasner-relato`, commita e faz `git push origin HEAD`. O remoto concreto da instalação é **[nao verificado]**. (`bin/relato.sh:2,11,23,37-38,114-117,145-147`)

   O escritor do log registra timestamp local, obra, papel, hash12, PID do **wrapper** (`$$`) e `ini|fim`; o pareamento existente usa `(obra, hash, pid)`. O leitor atual chama de “em curso” simplesmente `ini` sem `fim`; não verifica processo vivo. (`bin/codex_pergunta.sh:266-273,308-309`; `bin/codex_limite_estado.py:63-67,82-88`)

   `ini` precede a espera pelo slot, a trava da resposta, recusas possíveis e a execução do Codex. Portanto, mede tentativa do wrapper iniciada; não prova execução ativa do Codex. O próprio disjuntor explicita isso. (`bin/codex_pergunta.sh:308-309,364-394,425-426`; `bin/disjuntor.py:2071-2075`)

   A fila fornecida apresenta R1 DESPACHADO e R2–R7 ENTREGUE: **zero ABERTOS no literal**. Há HH:MM junto aos estados, mas nenhuma linha ABERTO nem campo/data explicitamente identificado como instante de abertura; a premissa de que todo ABERTO tenha a hora de abertura gravada é **[nao verificado]**. (`fila_reserva.md:3-10`, literal)

2. **Regras.** Medir e publicar não revoga CODEX-NAO-FICA-OCIOSO: o anexo mantém a fila e reserva a Ronald desligar a regra. Também não autoriza ultrapassar os cortes de orçamento existentes. (`fila_reserva.md:1-2`, literal; `complemento2_queima_util_1711.txt:6,10`, literal; `bin/disjuntor.py:1468-1485`)

   O contador deve entrar no produtor existente da linha Codex, preservando suas medidas e o placar exigido, com contrato antes da construção e pouso de instrumento separado. Contrato de cinco linhas efetivamente registrado no RELATO para este contador: **[nao verificado]**. (`bin/gerar_estado.py:267-270,337-339`; `bin/codex_limite_estado.py:235-249`; `CLAUDE.md:327-328,389-394`; `app/docs/PISTA.md:170-176`)

   A raspagem é requisito material porque ESTADO vai ao repositório público; publicar log/fila brutos não está autorizado pelo pedido de três números e contexto por hora. (`bin/relato.sh:2,114-117`; `complemento2_queima_util_1711.txt:6`, literal)

3. **Condições que faltam fechar.**

   - **Atividade real:** definir se conta wrapper vivo, incluindo espera, ou execução efetiva do Codex. `ini` sem `fim` é só evidência de falta de encerramento registrado; `fim` é escrito pelo trap EXIT, e o append tolera erro sem informar ao consumidor. Um órfão de processo morto pode permanecer “em curso” indefinidamente no leitor atual. Existência desse caso na amostra: **[nao verificado]**. (`bin/codex_pergunta.sh:269-282,308-309,364-394`; `bin/codex_limite_estado.py:82-88`)
   - **Contraexemplo que morde:** um ABERTO de 45 minutos com um `ini` órfão de processo morto teria `(a)=1` no leitor atual e esconderia a situação que o aval manda acusar. O caso dado de chamada realmente em curso deve continuar sem acusar. (`bin/codex_limite_estado.py:82-88`; `complemento2_queima_util_1711.txt:6`, literal)
   - **Abertura:** falta definir gravação e leitura de instante completo da entrada em ABERTO, incluindo eventual reabertura. ABERTO legível sem hora não é arquivo ausente/ilegível, portanto a falha declarada não cobre literalmente esse caso; idade não pode virar zero nem “verde” por inferência. (`fila_reserva.md:3-10`, literal; `complemento2_queima_util_1711.txt:6`, literal; `CLAUDE.md:35-38`) [nao verificado]
   - **Instante da foto:** a coleta atual ocorre na geração de ESTADO, antes de gerar sessão, verificar/raspar documentos, gerar CODEX-ESTADO/placar, copiar, commitar e publicar. “Na hora em que publica” ainda precisa fixar o instante da coleta e sua relação com publicação bem-sucedida; não há atomicidade entre essas etapas. (`bin/relato.sh:38-42,55-73,97-117,145-147`)
   - **24 horas:** separar a janela dos inícios por hora da contagem de chamadas ainda vivas: uma chamada iniciada antes da borda pode continuar ativa. Fixar limite inferior/superior e agrupamento horário. O leitor existente só testa `agora - t <= 24h`, aceita a borda exata e também timestamps futuros; não é definição suficiente do novo contexto. (`bin/codex_limite_estado.py:71-78`; `complemento2_queima_util_1711.txt:6`, literal)
   - **SEM DADO:** o leitor atual transforma arquivo de chamadas ausente em listas vazias; reutilizá-lo diretamente permitiria confundir ausência com zero. A nova medida precisa distinguir esses estados, além de tratar registros incompletos necessários à idade/atividade. (`bin/codex_limite_estado.py:49-68`; `complemento2_queima_util_1711.txt:6`, literal)

   A amostra termina em 08:19:53 e os vinte `ini` exibidos têm `fim` exibido; isso não prova atividade zero na hora futura da publicação nem cobre toda a janela de 24 horas, pois foi fornecida como cauda. A fila fornecida também não exercita ABERTO de 45 minutos. (`chamadas_amostra.log:1-40`, literal; `fila_reserva.md:4-10`, literal)

4. **Exportada, dinheiro, `!`.** O escopo declarado só lê fontes e publica contador: não prevê alteração de competência exportada, dinheiro, escala/vínculo ou arquivo usado por PROD. Implementação futura é **[nao verificado]**; continuam vigentes os limites de `!`, raspagem e pouso próprio de instrumento. (`complemento2_queima_util_1711.txt:6`, literal; `CLAUDE.md:265-268,327-328,411-413`; `bin/relato.sh:114-117`)

## 17/11 08:19 · sessao 728030717fda · E4C-F2 · verificador · origem=Code

**Parecer: bloqueia.** Há contraexemplos com pares de resposta legais que rompem a conservação: ressuscitação de pergunta validada, resposta automática posterior à validação e texto composto apenas de brancos. (`app/chamados/models.py:1647`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/models.py:4173`)

Execução Django/SQL, incidência em produção, contagens mencionadas em comentários e identidade Git da cópia: **[nao verificado]**. Os contraexemplos abaixo são de código, sem afirmar ocorrência em produção.

1. **[bloqueia] Ressuscitar pergunta validada conserva justamente a validação que a porta apaga — perguntas 1 e 6.**

   Contraexemplo: pergunta com `validada_em=T2`, `materializada_em=T2`, resposta `(None, '')`, via `completude_turno` e célula furada. A ressuscitação grava a foto `VALIDACAO(T2)` e depois zera validação, resposta e carimbo; não grava ato que desfaça a validação. A dobra conserva `T2`, portanto o novo `estado` é **VALIDADA**, embora a projeção persistida esteja sem validação. O próprio teste exige esse resultado. (`app/chamados/models.py:1629`; `app/chamados/models.py:1647`; `app/chamados/models.py:4230`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:377`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:381`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:383`)

   Esse par de resposta é legal: não tem instante nem texto após o trim. Logo, a exceção dos pares ilegais não cobre a mudança. (`app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1541`)

   Outro caminho com o mesmo formato é `devolver_instante_sem_texto_l130`: fotografa a resposta e zera o par sem ato limpador. Nesse caminho, porém, a guarda exclui perguntas validadas; permanece divergência do instante bruto, mas a resposta continua falsa pelo texto vazio. (`app/chamados/services/validacao.py:392`; `app/chamados/services/validacao.py:436`; `app/chamados/services/validacao.py:439`; `app/chamados/services/validacao.py:440`; `app/chamados/juizes.py:1552`)

2. **[bloqueia] RESPOSTA recebe um efeito que nem todos os seus escritores gravam — perguntas 1, 2 e 5.**

   | Escritor de RESPOSTA | O que faz com a projeção | Coerência com a regra nova |
   |---|---|---|
   | Resposta humana | Grava texto/instante e zera validação existente. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`) | Coerente com RESPOSTA desfazendo validação. (`app/chamados/models.py:4201`) |
   | Resposta automática | Grava somente texto/instante; preserva validação. (`app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1678`) | Incoerente quando a resposta fica depois da validação. (`app/chamados/models.py:4232`) |
   | Resposta pela trilha | Grava somente instante; preserva texto e validação. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1698`; `app/chamados/services/disputa_emissao.py:1699`) | Mesma incoerência se o instante recuperado for posterior à validação. (`app/chamados/models.py:4232`) |
   | Foto da projeção, inclusive backfill/nascimento | Copia o par, com motivo especial. (`app/chamados/models.py:4175`; `app/chamados/models.py:4176`) | Excepcionalmente não desfaz validação, pelo conteúdo de `ato_motivo`. (`app/chamados/models.py:4201`) |

   Contraexemplo legal da automática: projeção inicial `(validada_em=T1, respondida_em=None, resposta_colab='')`; executar automática com texto e `T2>T1`. A projeção termina `(T1,T2,texto)`, mas a dobra termina `(None,T2,texto)`. A única guarda da automática é texto já preenchido; não há guarda de validação. (`app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/models.py:4201`)

   O chamador real também não impede: responde automaticamente **antes** de chamar `validar_pergunta`; esta encontra o carimbo preservado e devolve `ja_validada`. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/chamados/services/validacao.py:685`)

   A trilha também não guarda validação: só exige texto presente e instante ausente. Sua entrada pertence ao par ilegal excluído do corte, mas continua refutando a afirmação de que todo escritor de RESPOSTA grava o efeito atribuído pela dobra. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/juizes.py:1541`)

   Portanto, há o problema de semântica pedido: **RESPOSTA humana desfaz validação; automática/trilha não; foto é distinguida por uma frase em `ato_motivo`**. (`app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1677`; `app/chamados/services/disputa_emissao.py:1698`; `app/chamados/models.py:4201`)

3. **[bloqueia] Brancos sem instante são legais, mas deixam de ser conservados — pergunta 3.**

   Contraexemplo: sem linhas, `respondida_em=None`, `resposta_colab=' \t '`. Os predicados consideram esse par legal, porque brancos são texto vazio. `_sem_correspondencia` não fabrica RESPOSTA sem instante, e a dobra devolve `resposta_colab=''`. (`app/chamados/juizes.py:1528`; `app/chamados/juizes.py:1534`; `app/chamados/juizes.py:1541`; `app/chamados/models.py:4173`; `app/chamados/models.py:4226`)

   Pelo diff fornecido, `resposta_display` antes devolvia os brancos; agora devolve `''`. As fotos de SILENCIO e RETRATACAO também trocam os brancos por vazio. Isso viola conservação literal fora dos pares ilegais. (`app/chamados/models.py:3753`; `app/chamados/models.py:3755`; `app/chamados/models.py:2032`; `app/chamados/models.py:2164`)

   O M15 monta precisamente `None + brancos` e exige SILENCIO vazio: ele certifica essa mudança, não sua conservação. (`app/chamados/tests/test_cortes_f2_f9_m15.py:50`; `app/chamados/tests/test_cortes_f2_f9_m15.py:57`)

4. **[bloqueia] `q_vivas` deixa de conter toda pergunta viva pelo C6 — pergunta 4.**

   No contraexemplo da ressuscitação validada, o C6 recebe `validada_em=None` da projeção e pode declarar viva a pergunta da célula furada; o novo grau 0 SQL a exclui pela VALIDACAO conservada no ato. Isso contradiz o contrato explícito de continência. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1405`; `app/chamados/juizes.py:1456`; `app/chamados/models.py:3556`; `app/chamados/models.py:3525`)

   O efeito alcança seleções anteriores ao juiz: o app filtra por `q_vivas` antes de chamar C6; o fio faz isso para disputas fechadas. A pergunta excluída não chega ao juiz. (`app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/fio.py:255`; `app/chamados/services/fio.py:269`)

   Outros leitores que discordam no mesmo domínio:

   | Leitor que permanece | Discordância concreta |
   |---|---|
   | Card administrativo, lendo `validada_em` | Ressuscitada aparece sem selo de validação e pode oferecer ações, enquanto `estado` diz VALIDADA. (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:42`; `app/chamados/models.py:3179`) |
   | Porta de validação, lendo projeção | No caso da automática, responde `ja_validada`, enquanto o estado pelo ato volta a RESPONDIDA. (`app/chamados/services/validacao.py:685`; `app/chamados/models.py:3187`) |
   | Fila de materialização | Continua exigindo validação projetada e não reconhece a VALIDADA apenas pelo ato da ressuscitada. (`app/chamados/services/materializacao.py:2093`; `app/chamados/services/materializacao.py:2110`) |
   | Partição do app e lista `respondidas` da API | Julgam a projeção, enquanto `.respondida` passou a julgar a dobra. (`app/chamados/services/pergunta_no_app.py:82`; `app/api/views.py:2578`; `app/chamados/models.py:3729`) |
   | Payload da API | Mistura texto/instante projetados com `resposta_display` migrado; por exemplo, pode devolver brancos em `resposta_colab` e vazio em `resposta_display`. (`app/api/views.py:2561`; `app/api/views.py:2562`; `app/api/views.py:2563`) |

   **`aguarda_admin` não é leitor independente da projeção:** continua exatamente derivado de `estado == RESPONDIDA`. Sua fonte está preservada, mas herda as mudanças de `estado`. (`app/api/views.py:2550`; `app/api/views.py:2553`)

   `tarefa_do_dia` já lê atos, porém julga a última linha, não a dobra: com V(T1) seguida de RESPOSTA-foto(T2), a dobra preserva validação e a tarefa pede VALIDAR pela última RESPOSTA. Essa divergência existe sem alterar `tarefa_do_dia`. (`app/chamados/models.py:4201`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:162`)

5. **[bloqueia] Prefetch velho altera o resultado da leitura — pergunta 5.**

   Contraexemplo concreto de operações:

   - Carregar `p` com decisões prefetched contendo V(T1); há fornecedor desse formato no fio. (`app/chamados/services/fio.py:263`)
   - Responder novamente em T2 pelo core: ele atualiza a instância, zera a validação e registra R(T2). (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`)
   - `registrar` usa `bulk_create` e não invalida o cache relacionado da instância. `leitura(p)` continua lendo o cache anterior. (`app/chamados/models.py:4247`; `app/chamados/models.py:4249`; `app/chamados/models.py:4217`)
   - Como R(T2) falta no cache, o novo par projetado entra como passivo com `desfaz=False`: V(T1) sobrevive. Em instância fresca, a RESPOSTA real desfaz V(T1). (`app/chamados/models.py:4221`; `app/chamados/models.py:4201`; `app/chamados/models.py:4232`)

   Resultado: mesmo banco, `leitura` da instância antiga validada e leitura fresca não validada; a face SQL acompanha a fresca. `prefetch_related_objects` também não resolve por si a instância que já chegou com decisões carregadas. (`app/chamados/models.py:1945`; `app/chamados/models.py:3494`; `app/chamados/models.py:4217`)

   O core aceita a instância e volta a lê-la após registrar, em `resposta_display`; encadeamento HTTP concreto passando uma instância prefetched até esse core: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1439`; `app/chamados/services/disputa_emissao.py:1560`)

6. **[fila] Há N+1 novo em caminhos sem prefetch — pergunta 5.**

   O carregamento do app não faz prefetch de decisões. Cada `_item` da API lê `estado` e `resposta_display`; ambos executam `leitura`, que busca `decisoes.all()` sem cache. São duas buscas adicionais de decisões por pergunta nesse percurso, nos loops de histórico e pendentes. (`app/chamados/services/pergunta_no_app.py:52`; `app/api/views.py:2550`; `app/api/views.py:2562`; `app/chamados/models.py:3178`; `app/chamados/models.py:3753`; `app/chamados/models.py:4217`; `app/api/views.py:2567`; `app/api/views.py:2577`)

   Também há busca por pergunta nas fotos de fechamento e retratação: os respectivos lotes não recebem prefetch nesse carregamento. O prefetch de `sem_veredito_entre` não garante cache nas novas instâncias buscadas depois em `self.perguntas.all()`. (`app/chamados/models.py:1945`; `app/chamados/models.py:2000`; `app/chamados/models.py:2032`; `app/chamados/models.py:2160`; `app/chamados/models.py:2164`)

   Contagem executada e impacto de latência: **[nao verificado]**.

7. **[fila] Os testes não passam por universo vazio, mas não fecham a conservação — pergunta 5.** [nao verificado]

   Há asserções que impedem vazio: universo com 13 perguntas e oito validadas; registro com sete leitores; descarimbo efetivo; ressuscitação de exatamente uma pergunta; M18 exigindo exatamente uma chamada observada ao C6. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:165`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:168`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:303`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:326`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:353`; `app/chamados/tests/test_cortes_f2_f9_m18.py:72`)

   As lacunas concretas são:

   - Automática e trilha são exercitadas **antes** da validação; não mordem os escritores que preservam validação. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:188`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:191`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:235`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:239`)
   - Os helpers recarregam a pergunta, retirando do teste o cache velho. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:38`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:176`)
   - O controle da ressuscitação usa instante sem texto, justamente uma exceção do corte. A caracterização seguinte exige a quebra com pergunta validada legal. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:352`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:381`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:383`)
   - A mordida do selo conserva projeção e atos alinhados; ela demonstra retorno aos leitores após descarimbo, mas não distingue leitura antiga da projeção de leitura nova dos atos nesses casos. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:315`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:328`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:331`)
   - M4/M13/M14 mordem mudança para ato com projeção vazia; essa mudança não é, por si, prova de conservação. M15 exige a normalização de brancos identificada no achado 3. (`app/chamados/tests/test_cortes_f2_f9_m4.py:51`; `app/chamados/tests/test_cortes_f2_f9_m13.py:43`; `app/chamados/tests/test_cortes_f2_f9_m14.py:48`; `app/chamados/tests/test_cortes_f2_f9_m15.py:57`)

   Execução dos testes e dos mutantes: **[nao verificado]**. [nao verificado]

8. **[nota] Face SQL e dobra concordam com dados frescos; isso não resolve os bloqueios — pergunta 3.**

   A equivalência estrutural da validação é:

   | Combinação | Resultado das duas faces |
   |---|---|
   | VALIDACAO real sem limpador posterior | Validada. (`app/chamados/models.py:3497`; `app/chamados/models.py:4230`) |
   | Empate entre linhas reais | Desempate por pk. (`app/chamados/models.py:3496`; `app/chamados/models.py:4219`; `app/chamados/models.py:4224`) |
   | Validação passiva empatada com limpador real | Passivo vem depois e permanece. (`app/chamados/models.py:3501`; `app/chamados/models.py:4221`) |
   | Carimbo casado com VALIDACAO posteriormente desfeita | Não recria passivo. (`app/chamados/models.py:3499`; `app/chamados/models.py:4177`) |
   | RESPOSTA-foto sem VALIDACAO-foto | Não limpa validação anterior. (`app/chamados/models.py:3494`; `app/chamados/models.py:4201`) |
   | Texto apenas de brancos | Não participa do predicado de validação; afeta resposta/display separadamente. (`app/chamados/models.py:3494`; `app/chamados/models.py:4232`) |

   `_sem_correspondencia` é efetivamente o mesmo corpo usado pela porta de fotos e pela dobra; a diferença relevante é a origem de `ja`: banco na porta, decisões carregadas na dobra. Com cache velho, essa origem pode divergir, como no achado 5. (`app/chamados/models.py:4160`; `app/chamados/models.py:4162`; `app/chamados/models.py:4218`; `app/chamados/models.py:4222`)

   A verificação em memória das funções extraídas por AST não encontrou divergência em 50.895 combinações finitas; execução do SQL real: **[nao verificado]**. A conclusão geral acima decorre dos dois ramos do predicado e da ordenação da dobra, não dessa amostra isoladamente. (`app/chamados/models.py:3494`; `app/chamados/models.py:3503`; `app/chamados/models.py:4224`; `app/chamados/models.py:4232`)

9. **[nota] Demais classes de reprovação — pergunta 5.**

   - **Segundo juiz:** `respondida` continua chamando o C21 registrado; mudou seu insumo. O bloqueio demonstrado é a divergência com C6 e com os escritores, não a criação de outro predicado de “instante E texto”. (`app/core/juizes.py:291`; `app/core/juizes.py:306`; `app/chamados/models.py:3729`)
   - **Derivação paralela:** a correspondência passiva foi compartilhada. A condição de RESPOSTA limpadora permanece expressa em Python e SQL; hoje as expressões concordam entre si, mas não com todos os escritores. (`app/chamados/models.py:4162`; `app/chamados/models.py:3494`; `app/chamados/models.py:4201`; `app/chamados/services/disputa_emissao.py:1677`)
   - **Relógio fora da borda:** os novos corpos da face SQL e da dobra não leem relógio. A dobra lê inclusive atos futuros; `tarefa_do_dia` corta pelo horizonte recebido, portanto são leituras temporalmente diferentes. (`app/chamados/models.py:3490`; `app/chamados/models.py:4217`; `app/chamados/services/tarefa_do_dia.py:423`)
   - **Ordem de trava:** os novos leitores não adquirem travas; os blocos de escrita examinados continuam adquirindo a ordem centralizada antes de escrever. Não demonstrei inversão nova causada pela F2. (`app/chamados/models.py:1628`; `app/chamados/models.py:2015`; `app/chamados/models.py:2132`; `app/chamados/models.py:2134`; `app/chamados/models.py:2796`)
   - **Segundo escritor da sequência:** a escrita física de produção localizada continua em `registrar`; o modelo também permite inserção inicial por `save`, embora recuse reescrita. Não apareceu produtor novo fora da porta no diff fornecido. (`app/chamados/models.py:4249`; `app/chamados/models.py:4136`; `app/chamados/models.py:4139`)
   - **Ato/campo conservado novo:** os atos continuam os oito declarados; `LeituraDoAto` é estrutura de retorno, não campo persistido. (`app/chamados/models.py:19`; `app/chamados/models.py:4090`; `app/chamados/models.py:4091`)

10. **[nota] Censo dos escritores e dos resets — pergunta 1.**

    Nesta tabela, V=`validada_em`, R=`respondida_em`, T=`resposta_colab`, M=`materializada_em`, via=`via_resolucao`.

    | Escritor | Campos/linha registrada no mesmo ato | A leitura reproduz a projeção? |
    |---|---|---|
    | Nascimento | Recebe campos por `**campos`, salva e grava fotos R/V. (`app/chamados/models.py:3149`; `app/chamados/models.py:3152`; `app/chamados/models.py:3154`) | Sim para as fotos correspondentes. (`app/chamados/models.py:4173`; `app/chamados/models.py:4177`) |
    | Resposta humana | T/R; V=None quando validada; fotos antigas + RESPOSTA. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`) | Na ordenação cronológica correspondente, sim. (`app/chamados/models.py:4224`; `app/chamados/models.py:4232`) |
    | Automática | T/R; fotos + RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1678`) | Não no contraexemplo do achado 2. |
    | Pela trilha | R; RESPOSTA, preservando V. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1699`) | Não necessariamente; achado 2. |
    | Validação | V=agora; fotos + VALIDACAO no mesmo instante. (`app/chamados/services/validacao.py:168`; `app/chamados/services/validacao.py:173`) | VALIDACAO grava V na dobra. (`app/chamados/models.py:4230`) |
    | Devolução por recusa de materialização | **V=None**; fotos + DEVOLUCAO. (`app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:279`) | DEVOLUCAO limpa V e preserva R/T. (`app/chamados/models.py:4189`; `app/chamados/models.py:4232`) |
    | Devolução BUG-PAR-103 | Descarimbo e **V=None**; fotos + DEVOLUCAO. (`app/chamados/services/validacao.py:346`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`) | Mesmo efeito sobre V/R/T. (`app/chamados/models.py:4232`; `app/chamados/models.py:4234`) |
    | Reabertura/recusa | **V=None, R=None, T=''**, mais descarimbo; RECUSA ou REABERTURA com foto. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648`) | Esses atos limpam V/R/T. (`app/chamados/models.py:4189`; `app/ch

[truncado em 20000 de 35429 caracteres]

## 17/11 08:19 · sessao 3c73bf371cb4 · PISTA-VOLTOU-1711 · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 17/11 08:19 · sessao 0cef7d1edeea · - · - · origem=Ronald (tmux)

- **Pré-opus** = suíte **inteira da árvore da obra verde antes do opus final**; não basta recorte, nem suíte rodada depois da leitura final. A regra não exige VM explicitamente: `CLAUDE.md:377-378`; `app/docs/PISTA.md:113-114`.
- **Não concluir colisão automática entre pré-opus na pista e pouso certificado na VM.** O contrato antigo dizia pista não certificar nem pré-opus (`app/docs/BACKLOG.md:2098`), mas registro posterior diz expressamente “PISTA-2 certifica pre-opus” (`app/docs/BACKLOG.md:2266`). O precedente de lateral também separa pista e certificação de pouso na VM: `app/docs/CORTES.md:493`. Isso exige reconhecer a evolução documental, sem transportar a proibição antiga como absoluta.
- **SUITE-UMA-VEZ-POR-ARVORE:** a parte operativa exige identidade da árvore, ausência de mudanças em `.py` entre veredito e merge, `cmp` contra a cópia e uma suíte para comboio; árvore diversa não herda certificação: `CLAUDE.md:320-324`; `app/docs/PISTA.md:35-38`. Duas execuções determinadas pelo arranjo pista/VM não são, por si, violação provada; o precedente da VM após pista está em `app/docs/CORTES.md:493`.
- **VM antes do pouso:** pode certificar o pouso e ser a suíte pré-opus se ocorrer **antes** desse opus, inteira e verde da mesma árvore (`CLAUDE.md:377-378`, `CLAUDE.md:320-321`). Se ocorrer depois, não sana retroativamente pré-opus ausente, recortado, vermelho ou de outra árvore (`app/docs/PISTA.md:113-114`, `app/docs/PISTA.md:38`).
- **PISTA-2 item 2:** o literal vigente permite espinha nas duas quando “VM livre e pista livre”, enquanto o item 3 proposto envia à pista com VM ocupada/fila. Há mudança operacional delimitada que precisa ser lida como exceção do novo complemento; não é dedutível da regra anterior: `app/docs/CORTES.md:490`.
- **“2 construtores”** mede vagas/papéis de construção simultâneos, teto padrão `1 opus + 1 Codex`; medidas, censos, sondas e RED por exemplo ficam fora desse teto (`app/docs/LEIS.md:147`). O teto é conferido antes de abrir (`CLAUDE.md:291-292`); o wrapper guarda apenas um Codex construtor por repositório e não guarda o limite de opus (`CLAUDE.md:355-356`). “Dois construtores da espinha abertos agora”, com nomes e obras, não foi demonstrado pelo dado colado: `[nao verificado]`.
- **Decisor da rota:** decisões técnicas e encaixe de raia são do Code (`CLAUDE.md:276-281`). A rota em `suite.sh` continua apenas registrada, construção mediante novo aval; até lá, conduta (`app/docs/BACKLOG.md:2100`). Não atribuir decisão automática a esse script.
- **Estado externo:** `app/docs/BACKLOG.md:2101` e `app/docs/RELATO.md:99` registram censo parcial e peças faltantes; são registros da cópia, não medições atuais desta revisão. Não afirmar paridade atual da pista como provada.

## 17/11 08:18 · sessao b1472cd961fc · - · - · origem=Ronald (tmux)

- Porta atual da pista é manual e exige árvore commitada: `git update-ref refs/pista/<sha> <sha>` e `ssh pista pede <sha> [--only <labels>] [--parallel N]`; espera por `ssh pista espera <id>`, consulta por `log`/`estado`, fila própria com uma suite por vez fora da trava VM — `app/docs/BACKLOG.md:2098`.
- O contrato atual é explícito: pista é **PRE-FILTRO**, “não certifica pouso nem a suite que o opus recebe (essas seguem na VM pela trava)” — `app/docs/BACKLOG.md:2098`. Portanto o problema do item 3 não se limita à certificação do pouso: usar pista como a única suite anterior ao opus altera também essa cláusula.
- A rota dentro de `bin/suite.sh` está somente registrada, construção depende de novo aval; até lá o uso é conduta — `app/docs/BACKLOG.md:2100`; o executor atual roda `docker run` local sob `trava_teste.sh` — `bin/suite.sh:317`.
- `--quem` apenas lê `/tmp/juliani_db_test.lock.quem` — `bin/trava_teste.sh:21`, `bin/trava_teste.sh:28`. A trava real é adquirida com `flock -w`; dono é escrito somente após aquisição e substituído por “livre” ao sair; nenhum registro de esperadores aparece nesse fluxo — `bin/trava_teste.sh:35`, `bin/trava_teste.sh:40`, `bin/trava_teste.sh:41`. A documentação avisa que responde chamador, não suite em voo — `CLAUDE.md:131`.
- O histórico/bloco de fila no ESTADO ainda é obra registrada aberta — `app/docs/BACKLOG.md:2654`. Não há nesse mecanismo leitura comprovada de “com fila” na hora do pedido; situação real em produção `[nao verificado]`.
- `pista_lock.sh` usa outro arquivo (`logs/.pista_lock`), registra dono por existência do arquivo e não usa o flock da VM; a prioridade é explicitamente não automatizada — `bin/pista_lock.sh:11`, `bin/pista_lock.sh:14`, `bin/pista_lock.sh:18`, `bin/pista_lock.sh:29`.
- O teto antigo é 2 construtores = 1 opus + 1 Codex; leituras/censos/sondas/RED por exemplo ficam fora do teto — `app/docs/LEIS.md:147`. O wrapper só guarda uma vaga Codex; “1 opus” é conduta — `CLAUDE.md:355`. Dois construtores abertos não mede ocupação nem fila de suites; o disparador “na espinha” ainda precisaria identificar as duas obras no momento do pedido `[nao verificado]`.
- O censo da certificação está registrado como parcial, com três peças faltantes e scripts remotos não lidos — `app/docs/BACKLOG.md:2101`. Isso é registro documental, não medição nova do estado atual `[nao verificado]`.

## 17/11 08:14 · sessao 0c92dca9b7d6 · - · - · origem=Ronald (tmux)

Auditoria somente de código, sem banco/log/testes. Os números de produção que aparecem em comentários do código são **[nao verificado]**; por exemplo, os 187 casos em `app/chamados/models.py:4200` e os 15.114/50.829 em `app/chamados/models.py:4213`.

1. **Bloqueia — quebra da continência de `q_vivas` e do C6.** Ressuscitação zera `validada_em` conservando a linha VALIDACAO (`app/chamados/models.py:1629`, `app/chamados/models.py:1647`). A nova dobra mantém VALIDACAO (`app/chamados/models.py:4228`), `estado` devolve VALIDADA (`app/chamados/models.py:3179`), e `q_vivas` exclui a pergunta (`app/chamados/models.py:3556`). Entretanto, o C6 lê `validada_em=None` da projeção (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1405`) e, numa célula `furo`, devolve viva (`app/chamados/juizes.py:1456`). O contrato escrito exige que toda viva esteja no SQL (`app/chamados/models.py:3525`). O app usa o SQL antes do C6, portanto perde a pergunta antes de julgá-la (`app/chamados/services/pergunta_no_app.py:52`, `app/chamados/services/pergunta_no_app.py:61`); o mesmo ocorre no fio para disputas fechadas (`app/chamados/services/fio.py:255`, `app/chamados/services/fio.py:269`).

2. **Bloqueia — leitura pode depender de cache anterior ao ato.** `leitura` usa `p.decisoes.all()` (`app/chamados/models.py:4217`); `registrar` faz `bulk_create` e não invalida o cache da instância recebida (`app/chamados/models.py:4247`, `app/chamados/models.py:4249`). Contraexemplo: objeto prefetched com VALIDACAO em T1; resposta nova em T2 pelo core zera a projeção de validação (`app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1494`) e registra RESPOSTA (`app/chamados/services/disputa_emissao.py:1525`). Na instância com cache, falta a linha R(T2), então a resposta projetada entra como passivo com `desfaz=False` (`app/chamados/models.py:4221`), conservando V(T1); numa instância fresca, R(T2) desfaz V(T1) (`app/chamados/models.py:4201`). Existe fornecedor real de instâncias prefetched no fio (`app/chamados/services/fio.py:263`) e o core relê o mesmo objeto depois de registrar, em `resposta_display` (`app/chamados/services/disputa_emissao.py:1560`). Encadeamento HTTP concreto que passa uma dessas instâncias prefetched ao core: **[nao verificado]**.

3. **Fila — N+1 introduzido na API e nas fotos.** O carregamento do app não prefetch decisões (`app/chamados/services/pergunta_no_app.py:52`). `_item` chama `estado` e `resposta_display` (`app/api/views.py:2550`, `app/api/views.py:2562`), ambos chamando `leitura` (`app/chamados/models.py:3178`, `app/chamados/models.py:3753`), que consulta decisões sem cache (`app/chamados/models.py:4217`): até **2N consultas adicionais** nos loops de histórico e pendentes (`app/api/views.py:2567`, `app/api/views.py:2577`). As fotos novas também consultam por pergunta no fechamento (`app/chamados/models.py:2000`, `app/chamados/models.py:2032`) e retratação (`app/chamados/models.py:2164`). O prefetch feito dentro de `sem_veredito_entre` (`app/chamados/models.py:1945`) não garante cache nas novas instâncias de `self.perguntas.all()` em `app/chamados/models.py:2000`. Medição efetiva de queries/latência: **[nao verificado]**.

4. **Bloqueia — duas verdades na mesma tela/API.** No contraexemplo da ressuscitação validada, `estado=VALIDADA`, mas o card testa `perg.validada_em` e oferece ações como não validada (`app/templates/chamados/partials/_pergunta_card.html:20`, `app/templates/chamados/partials/_pergunta_card.html:23`, `app/templates/chamados/partials/_pergunta_card.html:42`). Correção administrativa também aceita a projeção sem validação (`app/chamados/services/acoes_disputa.py:349`), embora o estado diga VALIDADA (`app/chamados/models.py:3179`). Materialização filtra pelo carimbo projetado, portanto não trata essa VALIDADA pelo ato (`app/chamados/services/materializacao.py:2093`, `app/chamados/services/materializacao.py:2110`). `aguarda_admin` continua derivado de `estado == RESPONDIDA`, exatamente na fonte proibida de alterar (`app/api/views.py:2550`, `app/api/views.py:2553`); **não** é leitor independente da projeção. Entretanto, a mesma resposta inclui texto/instante projetados junto do display migrado (`app/api/views.py:2561`, `app/api/views.py:2562`, `app/api/views.py:2563`), permitindo payload incoerente quando projeção e atos divergem. A seleção `respondidas` continua julgando a projeção (`app/api/views.py:2578`).

5. **Nota — tarefa do dia já interpreta atos, mas de outra maneira.** Ela julga a última linha, não a dobra: RESPOSTA → VALIDAR (`app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:159`, `app/chamados/services/tarefa_do_dia.py:162`); VALIDACAO → conferir lavra/efeito (`app/chamados/services/tarefa_do_dia.py:167`); SILENCIO → encerrada (`app/chamados/services/tarefa_do_dia.py:176`). Contraexemplo: V(T1) seguida de R-foto(T2); a dobra preserva validade pelo motivo especial (`app/chamados/models.py:4201`), mas a tarefa pede VALIDAR pela última RESPOSTA (`app/chamados/services/tarefa_do_dia.py:159`). Seu horizonte corta atos futuros (`app/chamados/services/tarefa_do_dia.py:423`); a nova leitura não corta (`app/chamados/models.py:4217`). Alteração de tarefa_do_dia não consta do diff fornecido; identidade do diff com o commit real **[nao verificado]**, pois a cópia não possui `.git`.

Censo de leitores diretos abaixo. Notação: caminhos relativos a `app/`; múltiplas linhas na mesma entrada indicam sítios no mesmo arquivo. Não confundir leitores do resultado `LeituraDoAto` com leitores da projeção: `models.py:1948`, `models.py:3179`, `models.py:3753` já recebem o resultado migrado.

**`validada_em`:**

- `chamados/models.py`: `_universo_da_ressuscitacao` 1554; `ressuscitar_perguntas` 1630; `sem_veredito_entre` 1948; `estado` 3179; `carimbar` 3266,3273; SQL 3500,3501,3503; passivo 4157,4177,4178,4179; dobra 4223.
- `chamados/juizes.py`: memo 856; projeção temporal/C6 1389.
- `chamados/reconciliador.py`: 632,1051.
- `chamados/services/materializacao.py`: 199,201,1155,2093,2110.
- `chamados/services/validacao.py`: 173,181,269,343,392,640,685,898,1064,1085.
- `chamados/services/disputa_emissao.py`: 1484,1485,1636,2062.
- `chamados/services/declaracao_texto.py:228`; `chamados/services/vigia_carimbo.py:98`; `chamados/services/verbos.py:264,270`; `chamados/services/painel.py:67,270,271,272,273`; `chamados/services/acoes_disputa.py:349`.
- `ponto/services/flip_auto.py:130`; `ponto/services/procedencia_batida.py:74`.
- Histórico dinâmico: `chamados/services/historico.py:101,105`.
- Comandos: `chamados/management/commands/backfill_decisoes_pergunta.py:56,60,102`; `reabrir_orfas_enterradas.py:68`; `encerrar_perguntas_com_fato.py:32`; `auditar_invariantes_chamados.py:206,1077,1578,1607,1772,1784`; `bug_par_103.py:68`.
- Templates: `templates/chamados/partials/_pergunta_card.html:20,23,42`; `modal_fio.html:228`.
- Fotografias genéricas: `chamados/services/foto_e4b.py:250,251,272`; reversão `chamados/services/passivo_validado.py:64,66,83`.

**`respondida_em`:**

- `chamados/models.py`: 1554,1630,2974,3266,3270,3409,3411,3644,3645,4157,4173,4174,4175,4223.
- `chamados/juizes.py`: `_slot_esperado` 352; memo 856; projeção/C6 1389; `respondida_sem_texto` 1534; `texto_sem_carimbo` 1541; juiz 1552; SQL 1572,1580,1591; fila/ordenação 1765.
- `chamados/veredito_lavrado.py:207,208`; `chamados/services/painel.py:67`; `chamados/services/vigia_fio.py:747`.
- `chamados/services/materializacao.py:1154`; `chamados/services/validacao.py:434,913,1256`; `chamados/services/disputa_emissao.py:1635,1692,1861,2074,2091`; `chamados/services/fio.py:187`; `chamados/services/raiox.py:138`.
- `chamados/services/verbos.py:337,338,344,347`.
- Histórico dinâmico `chamados/services/historico.py:99,105`; reversão `chamados/services/passivo_validado.py:66,83`; fotografia genérica `chamados/services/foto_e4b.py:251,272` e detector de ressuscitação `chamados/services/foto_e4b.py:180`.
- API: `api/views.py:1165,2563,2639`; `api/views_mensageria.py:569,570`.
- Comandos: `chamados/management/commands/estoque_l130_instante_sem_texto.py:58`; `curar_perguntas_em_limbo.py:46,69`; `auditar_invariantes_chamados.py:1107`; `backfill_decisoes_pergunta.py:56,60,504`.
- Templates: `templates/chamados/partials/modal_fio.html:220`; `_card_atendimento.html:106,143,391`; `_pergunta_card.html:32`.

**`resposta_colab`:**

- `chamados/models.py`: 1554,1630,2032,2164,2975,3269,3408,3412,3644,3646,3753,4176,4180.
- `chamados/juizes.py`: 364,857,1360,1389,1534,1541,1552,1572,1580,1791,1938,2042,2047,2064.
- `chamados/services/materializacao.py`: 546,574,625,671,678,718,733,770,953,1050,1112,1153,1372,1554,1580,1589,1641.
- `chamados/services/validacao.py`: 175,281,356,434,467,912,1183,1185,1187,1192,1193,1194,1195,1198.
- `chamados/services/disputa_emissao.py`: 1635,1663,1692,1700,1861,2079.
- `chamados/services/declaracao_texto.py:418`; `chamados/services/explicador.py:55,102,103`; `chamados/services/acoes_disputa.py:357`; `chamados/services/fio.py:377`; `chamados/services/historico.py:109`; `chamados/services/vigia_fio.py:750`; `chamados/services/raiox.py:138`; `chamados/views_cobrar.py:293`.
- `chamados/templatetags/chamados_tags.py:139,166`; `ponto/services/veredito_celula.py:197`; `ponto/services/dia_do_colab.py:70`.
- API: `api/views.py:1163,2561`; `api/views_mensageria.py:567`.
- Comandos: `chamados/management/commands/estoque_l130_instante_sem_texto.py:58`; `curar_perguntas_em_limbo.py:32,46`; `validar_classe_a.py:80`; `auditar_invariantes_chamados.py:1108`; `backfill_decisoes_pergunta.py:61,326,504`.
- Templates: `templates/chamados/partials/modal_fio.html:219`; `_card_atendimento.html:106,114,141,143,148,154,182,183,391`; `_lote_preview.html:24`.
- Fotografia genérica: `chamados/services/foto_e4b.py:251,272`.

**`materializada_em`:**

- `chamados/models.py`: 1539,1948,2031,3181,3185,3195,3265,3321,3322,3440,3452,3469,3542,3544.
- `chamados/juizes.py`: 292,683,711,858,1390,1502.
- `chamados/reconciliador.py`: 368,631,1051,1147,1157,1507.
- `chamados/veredito_lavrado.py:203,204`; `chamados/views.py:1257`.
- `chamados/services/materializacao.py`: 148,261,262,289,326,862,874,1018,1110,1112,1117,1156,1680,2094,2107,2155.
- `chamados/services/validacao.py`: 264,341,394,643,769,805.
- `chamados/services/disputa_emissao.py`: 228,776,1084,1104,1340,1637,1930,2062.
- `chamados/services/fabrica_da_celula.py:171`; `chamados/services/raiox.py:135,182`; `chamados/services/painel.py:67`; `chamados/services/relavra_via.py:181,244`; `chamados/services/passivo_validado.py:534,536`.
- `ponto/services/veredito_celula.py:196`; `ponto/services/procedencia_batida.py:74`; `ponto/management/commands/reconciliar_fantasmas.py:213`.
- Histórico dinâmico `chamados/services/historico.py:103,105`; fotografia genérica `chamados/services/foto_e4b.py:251,272` e detector `foto_e4b.py:180`; reversão `chamados/services/passivo_validado.py:64,83`.
- Comandos: `chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`; `encerrar_perguntas_com_fato.py:32`; `auditar_disputas.py:71,150`; `reabrir_orfas_enterradas.py:67,88,91,129,142`; `backfill_decisoes_pergunta.py:101,325`; `bug_par_103.py:66`; `auditar_invariantes_chamados.py:471,539,589,907,957,1077,1158,1579,1607,1773`.
- `chamados/services/regularizacao_ext.py:220` lê **outro modelo**, RegularizacaoExterna, e não integra esse domínio.

**`via_resolucao`:**

- `chamados/models.py`: 1543,2025,2035,2161,2166,2854,2885,3194,3265,3321,3340,3421,3429,3440,3452,3469,3476,3542,3544,3712,3718.
- `chamados/juizes.py`: 683,695,857,1390.
- `chamados/reconciliador.py:1146,1157`; `chamados/services/fabrica_da_celula.py:149`; `chamados/services/competencia_trancada.py:276`.
- `chamados/services/materializacao.py`: 261,292,862,868,1111,1153.
- `chamados/services/validacao.py:341,435`; `chamados/services/disputa_emissao.py:337,409,428,446,1637,2072,2087,2090`.
- `chamados/services/fio.py`: 1148,1215,1294,1401,1484,1925,1961,1983,2028.
- `chamados/services/tarefa_do_dia.py:256,257`; `chamados/services/explicador.py:118`; `chamados/services/relavra_via.py:118,121,123,155,181,245`; `chamados/services/passivo_validado.py:141,142,530`; `ponto/services/cartorio.py:1651`.
- Fotografia genérica `chamados/services/foto_e4b.py:251,272`; reversão `chamados/services/passivo_validado.py:64,83`.
- Comandos: `chamados/management/commands/estoque_l130_instante_sem_texto.py:59`; `migrar_data_evento_dia_do_turno.py:70,71,72`; `encerrar_perguntas_com_fato.py:33,42,49,66,76`; `auditar_invariantes_chamados.py:203,207,434,543,957,1716`; `reabrir_orfas_enterradas.py:67,129,142,160`; `relavrar_via_i6.py:58,59,62,66,68,88`; `backfill_decisoes_pergunta.py:101,314,330,398,504`; `bug_par_103.py:66`.
- Template `templates/chamados/partials/_card_atendimento.html:192`.

**Campos da DecisaoPergunta:**

- `ato`: SQL em `chamados/models.py:3494,3497,3499`; histórico em `chamados/models.py:3740,3744`; correspondência em `chamados/models.py:4160,4161`; dobra em `chamados/models.py:4218,4219`; tarefa em `chamados/services/tarefa_do_dia.py:159,163,165,167,176,178,187,260,425`; BUG-PAR em `chamados/services/materializacao.py:275`; backfill em `chamados/management/commands/backfill_decisoes_pergunta.py:99,315`.
- `ato_em`: SQL `chamados/models.py:3496,3500,3501`; ordenação do modelo `chamados/models.py:4122`; correspondência `chamados/models.py:4161`; dobra `chamados/models.py:4218,4219,4221`; carga da tarefa `chamados/services/tarefa_do_dia.py:422,423,431`; template `templates/chamados/partials/modal_fio.html:210,212`.
- `ato_texto`: dobra `chamados/models.py:4219,4221`; tarefa `chamados/services/tarefa_do_dia.py:160`; template `templates/chamados/partials/modal_fio.html:210,212`.
- `ato_motivo`: SQL `chamados/models.py:3494`; dobra `chamados/models.py:4219`; BUG-PAR `chamados/services/materializacao.py:275`; template `templates/chamados/partials/modal_fio.html:212`.
- `pk` da decisão: desempate SQL `chamados/models.py:3496`; ordenação `chamados/models.py:4122`; desempate da dobra `chamados/models.py:4219`. A tarefa ordena por `ato_em,pk` (`chamados/services/tarefa_do_dia.py:422`).
- Todos esses campos, incluindo pk, também são lidos pela fotografia genérica `chamados/services/foto_e4b.py:250,251,280`.

Completude absoluta para acessos indiretos construídos dinamicamente: **[nao verificado]**. O censo acima combina ocorrências por `rg`, cargas AST e inspeção dos leitores genéricos `foto_e4b`, `historico` e `passivo_validado`; não inclui comentários nem código de testes como leitores de produção. [nao verificado]

## 17/11 08:13 · sessao 356668db39e3 · - · - · origem=Ronald (tmux)

1. **Bloqueia — ressuscitar mantém VALIDACAO vigente na dobra após apagar a projeção.** `ressuscitar_perguntas` fotografa os campos antes do reset, sem ato limpador: `app/chamados/models.py:1629`, `:1647`. Com `validada_em=T2`, `materializada_em=T2`, par vazio e célula furada, a projeção final fica sem validação, mas a dobra lê `T2`, `estado=VALIDADA`, e a pergunta sai de `q_vivas`. O teste exige expressamente essa divergência: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:377`, `:381`, `:382`, `:383`, `:384`. O par de resposta desse caso é legal, portanto não cabe na exceção dos pares ilegais: `app/chamados/juizes.py:1534`, `:1541`.

2. **Bloqueia — conservação literal também muda num par legal com brancos.** Cenário: `respondida_em=None`, `resposta_colab=' \t '`, nenhuma linha. Brancos são texto vazio, portanto esse par é legal: `app/chamados/juizes.py:1526`, `:1534`, `:1541`; `app/chamados/services/fio.py:1617`. `_sem_correspondencia` não cria RESPOSTA sem instante; a dobra retorna `''`: `app/chamados/models.py:4173`, `:4225`, `:4236`. Assim `resposta_display` passa a `''`, enquanto antes devolvia os brancos segundo o diff fornecido: `app/chamados/models.py:3753`. O mesmo acontece nas fotos SILENCIO/RETRATACAO: `app/chamados/models.py:2032`, `:2164`. M15 cria precisamente None+brancos e exige SILENCIO vazio: `app/chamados/tests/test_cortes_f2_f9_m15.py:46`, `:50`, `:55`.

3. **Nota — não encontrei contraexemplo entre a face SQL e a dobra com dados frescos.** A equivalência é estrutural: validação vigente significa alguma VALIDACAO real sem limpador posterior por `(ato_em,pk)`, ou VALIDACAO passiva sem linha correspondente e sem limpador estritamente posterior ao carimbo. São os dois ramos SQL: `app/chamados/models.py:3494`, `:3495`, `:3497`, `:3499`, `:3501`, `:3503`; e a ordenação/redução da dobra: `:4217`, `:4221`, `:4224`, `:4230`, `:4232`. Empate real usa pk; passivo empatado vem depois. Passivo casado com VALIDACAO desfeita não é recriado: `:4177`. RESPOSTA foto não limpa validação mesmo sem VALIDACAO foto: `:4201`, `:4221`. Brancos não participam desse predicado de validação: `:3494`, `:4232`.

   Executei em memória as três funções reais extraídas por AST, comparadas com tradução literal do predicado SQL, sobre 50.895 combinações de 0–3 linhas, dois instantes com empates, seis tipos/efeitos e projeções vazias/preenchidas; nenhum contraexemplo. Execução SQL real e equivalência na presença de cache velho: **[nao verificado]**. [nao verificado]

4. **Fila — os testes têm mordidas, mas deixam fora os casos que ameaçam a conservação.** O conjunto SQL não pode passar vazio: exige 13 perguntas e oito validadas; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:165`, `:168`. O selo exige sete leitores reais, descarimbo efetivo e sete resultados verdadeiros: `:303`, `:310`, `:326`, `:331`. O controle exige ressuscitação de exatamente uma pergunta: `:353`. M18 exige exatamente uma chamada observada ao C6: `app/chamados/tests/test_cortes_f2_f9_m18.py:72`. Portanto não identifiquei passagem por universo vazio nessas asserções.

   Entretanto, os auxiliares recarregam a pergunta, ocultando prefetch velho: `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:38`, `:176`. Automática e trilha são testadas somente **antes** da validação: `:188`, `:191`, `:235`, `:239`. O controle de ressuscitação usa justamente o par ilegal instante sem texto: `:352`. A caracterização valida a quebra do item 1: `:381`, `:383`. O registro dos sete não inclui `_ancoradas`, relavra nem as duas fotos: `:277`; M13/M14/M15 cobrem os três primeiros, enquanto cobertura da foto RETRATACAO no diff: **[nao verificado]**.

5. **Nota — outro reset sem ato limpador existe, mas sua guarda exclui validação.** `devolver_instante_sem_texto_l130` fotografa o passivo e zera o par sem REABERTURA/DEVOLUCAO: `app/chamados/services/validacao.py:416`, `:436`, `:439`, `:440`. A guarda recusa `validada_em` preenchido: `:392`. A dobra conserva instante/texto da linha enquanto a projeção fica None/`''`; geralmente a resposta continua falsa porque o texto é vazio após trim: `app/chamados/juizes.py:1552`. Incidência atual: **[nao verificado]**.

6. **Nota — não caracterizei juiz duplicado, relógio novo fora da borda ou inversão nova de trava no trecho F2.** `respondida` continua delegando ao C21 registrado; o que muda é seu insumo: `app/core/juizes.py:306`; `app/chamados/models.py:3730`. A dobra não consulta relógio: `app/chamados/models.py:4217`. A ordem global continua centralizada e a ressuscitação a adquire antes das escritas: `app/chamados/models.py:2759`, `:2796`, `:1628`. A nova regra que RESPOSTA desfaz validação deve, contudo, ser confrontada com cada escritor; ela depende de `ato_motivo` para distinguir efeitos: `app/chamados/models.py:4201`. Isso é mudança semântica verificável, não prova de duplicação do C21.

## 17/11 08:12 · sessao 77b07fcbb0f9 · CODEX-RESERVA-R7 · leitura · origem=Code

**Os mapas precisam de atualização:** HE revogada foi curada; LOTE7 curou dois achados de ausência; os três defeitos centrais de férias permanecem; o fallback do calendário passou a declarar desconhecimento. (`app/ponto/portas/he.py:197`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1756`; `app/ferias/models.py:234`; `app/ferias/services.py:263`; `app/ferias/services.py:652`; `app/colaboradores/services/calendario.py:800`)

Commits abaixo são **atribuições documentais**: a cópia não contém histórico Git; ancestralidade, commit introdutor exato e ocorrências em produção nesta auditoria: **[nao verificado]**. O fluxo documentado entrega a cópia por `git archive`. (`CLAUDE.md:344`) [nao verificado]

Convenção: **PROCEDE** preserva a conclusão, inclusive composição ou desenho; **MUDOU** também identifica premissa do inventário refutada, sem presumir mudança posterior. **—** significa cura/commit não identificado **[nao verificado]**. [nao verificado]

Obras referenciadas nas tabelas:

| Sigla | Obra e commit registrado | [nao verificado]
|---|---|
| H | HE-REVOGADA-NA-PORTA: merge `271ecba2e`, origem `5937d715b`. (`app/docs/RELATO.md:259`) |
| L7 | LOTE7 c7: merge `42fd467a0`, origem `a094967ee`. (`app/docs/RELATO.md:164`) |
| R3 | E3-RESTO-R3/LU3: registro de pouso c17 a partir de `690fde88b`; commit introdutor da alteração específica **[nao verificado]**. (`app/docs/RELATO.md:235`; `app/colaboradores/services/calendario.py:135`) |

**Mapa 07 — HE**

Abreviações: `he` = `app/ponto/portas/he.py`; `hpl` = `app/ponto/services/he_pendente_lavrado.py`; `pexp` = `app/folha/porta_export.py`; `fech` = `app/ponto/services/fechamento.py`. (`he:130`; `hpl:209`; `pexp:800`; `fech:448`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| T1 — período × dia: piso e escritor compartilhado | PROCEDE | `he:577`, `he:595`, `he:206` | — |
| T1b — período contava revogada como decisão | CURADO | Trava linha crua, consulta `_vigente`, só barra vigente: `he:583`, `he:589`, `he:590` | H |
| T2 — “Não” humano sobre revogada dava `no_op` | CURADO | Idempotência compara vigente; gravação limpa revogação: `he:195`, `he:197`, `he:83` | H |
| T3 — lavrador × gestão: soma compartilhada | PROCEDE | `hpl:55`, `hpl:92`, `pexp:194` | — |
| T4 — gestão × trava TXT × motor usam vigentes | PROCEDE | `app/ponto/services/gestao_he.py:87`, `pexp:684`, `app/ponto/motor_calculo_v2.py:1417` | — |
| T5 — callback × porta de revogação | PROCEDE | `app/ponto/management/commands/lavrar_he_pendente.py:56`, `he:481`, `he:494`, `he:500` | H migrou também a guarda do revogador |
| T6 — relavratura após decisão e recuperação pelo cron | MUDOU | Callback captura falha: `fech:1818`, `fech:1824`. Os crons citados lavram retrato/recusam ponta; não recompõem fechamento: `app/config/crons.py:1153`, `:1577`, `:1587` | Recuperação alegada pelo cron não demonstrada; mudança temporal **[nao verificado]** |
| T7 — `_exportada` delega à autoridade de entrega | PROCEDE | `app/ponto/services/autorizacao_he_periodo.py:113`, `:117` | — |
| T8 — TXT da tela × comando | PROCEDE | Tela confere e injeta cálculo lido: `app/folha/views.py:128`, `:209`, `:268`; comando monta gravado e registra sem conferir: `app/folha/management/commands/regerar_txt_dominio.py:85`, `:118` | — |
| E1 — `_gravar` escreve estado | PROCEDE | `he:44`, `he:66`, `he:75`, `he:84` | — |
| E2 — `_marcar_revogada` escreve marca | PROCEDE | `he:375`, `he:401`, `he:403` | — |
| E3 — “selos escrevem direto” | MUDOU | Referências são inspeções AST/texto, não fixtures escritoras: `app/ponto/tests/test_o214_ponta_pequena.py:325`; `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:426`, `:445` | Premissa refutada; alteração posterior **[nao verificado]** |
| E4 — `.delete()` no comando é prosa | PROCEDE | `app/ponto/management/commands/recusar_ponta_pequena.py:219` | — |
| E5 — restore de DiaPago fora da porta | PROCEDE | Delete e `bulk_create`: `bin/restore_relavratura_10_2026.py:85`, `:86` | — |
| E6 — script passa argumento inexistente | PROCEDE | Chamada: `bin/relavrar_o120_col221_09.py:74`; assinatura: `fech:448` | — |
| E7 — trilha best-effort | PROCEDE | `he:100`, `he:414` | — |
| S1 — autorizado → não tem saída | PROCEDE | `he:197`, `he:206` | — |
| S2 — não → autorizado tem saída | PROCEDE | `he:175`, `he:197`, `he:206` | — |
| S3 — revogada sem saída pelo “Não” humano | CURADO | `he:196`, `he:200`; caso estático no teste: `app/ponto/tests/test_he_revogada_na_porta.py:50` | H |
| S4 — revogada barrava período | CURADO | `he:589`, `he:590`; caso estático no teste: `app/ponto/tests/test_he_revogada_na_porta.py:92` | H |

Sítios que o inventário não individualizou:

| leitor/escritor adicional | resultado |
|---|---|
| `_vigente` dentro da porta | Centraliza a consulta também para recusa automática e revogação, além dos dois caminhos defeituosos: `he:148`, `he:347`, `he:481`. |
| Escritor e leitor do retrato HE | Persistem/devolvem fotografia, separada da decisão viva: `hpl:177`, `hpl:182`, `hpl:193`. |
| Escritor final do TXT | `registrar` cria exportação sem consultar trava HE; depende do chamador: `app/folha/models.py:174`, `:182`. |
| Persistência e leitura do carimbo do portão | Flag/lista são persistidas e depois lidas de `MetricaSnapshot`: `pexp:795`, `pexp:807`, `pexp:814`, `pexp:853`. Envelhecimento entre atos é possível pelo mecanismo; ocorrência **[nao verificado]**. |
| Outros consumidores da janela HE | Cadastro-realidade, vigia e pauta usam `competencias_a_lavrar`: `app/escala/services/cadastro_realidade.py:553`; `app/chamados/services/vigia_fio.py:511`; `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:96`. |

Perguntas: **Q1–Q2** foram resolvidas no executável pela H; **Q3** permanece no comando; **Q4** teve comentário da relavratura corrigido, mas o cabeçalho ainda diz “não paga”; **Q5** lê fotografia persistida; **Q6** mantém trilha best-effort; **Q7** implementa corrente + anterior não entregue; **Q8** mantém relógio dentro da porta. (`he:200`; `he:589`; `app/folha/management/commands/regerar_txt_dominio.py:85`; `he:18`; `he:218`; `pexp:807`; `he:414`; `hpl:220`; `hpl:223`; `he:401`)

**Resumo 07:** T1b/T2/S3/S4 curados pela H; emissão alternativa do TXT permanece; E3 e a promessa de recuperação pelo cron precisam de correção no inventário. (`he:197`; `he:590`; `app/folha/management/commands/regerar_txt_dominio.py:85`; `app/ponto/tests/test_o214_ponta_pequena.py:325`; `app/config/crons.py:1153`)

**Mapa 08 — ausência/atestado**

Abreviações: `aus` = `app/ponto/services/ausencia.py`; `pv` = `app/ponto/views.py`; `mod` = `app/ponto/models.py`. (`aus:955`; `pv:2745`; `mod:660`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| L1 — matriz única e chamadores | PROCEDE | Matriz/porta: `aus:914`, `aus:955`; férias continuam recusadas nessa porta: `aus:968` | L7 acrescentou acordamento: `aus:989` |
| Nascimento explícito × reabertura | PROCEDE | Reabrir não recebe `status`: `aus:433`; nascimento usa pedido: `aus:486`; reabertura deriva: `aus:1298` | — |
| Pré-veto PWA × criação | PROCEDE | Veto singular PWA: `pv:2670`; precedência na porta: `aus:465`; API chama diretamente: `app/api/views.py:1812`, `:1818` | — |
| Efeito `None`: folha × espelho | PROCEDE | Retorno possível: `app/ponto/catalogo/ausencias.py:295`; folha suprime: `app/ponto/services/fechamento.py:700`; coletor só suprime `SUPRIME`: `app/ponto/services/efeito_ausencia.py:61` | EFEITO-NONE-UM-LEITOR ainda em fila: `app/docs/BACKLOG.md:1907` |
| Documento: matriz × gate × vencimento | PROCEDE | `aus:915`, `aus:773`, `aus:1177`; gate admite confirmação explícita: `pv:2754` | — |
| Contradição documento × rejulgamento por tipo | PROCEDE | Perguntas distintas: `aus:757`, `aus:689`; edição rejulga quando muda tipo: `aus:674` | — |
| Fechar chamado da ausência decidida | MUDOU | Leitor permanece: `aus:851`, `aus:863`; decisão agora atomic, transição/carimbos e acordamento: `aus:1079`, `aus:1088`, `aus:1123` | L7 |
| Docstring `retornar_mesmo_tipo` | PROCEDE | Texto antigo: `aus:295`; rejeitada reabre antes: `aus:399`, `aus:433`; retorno vem depois: `aus:436` | — |
| L2 — criar: save e trilha | MUDOU | Save em atomic: `aus:561`, `aus:562`; evento: `aus:585`; novo acordamento: `aus:597` | L7 |
| Editar período/tipo fora de `mudar_estado` | MUDOU | Escrita permanece: `aus:648`, `aus:659`; invalidação agora compara insumo antes/depois: `aus:669`; rejulgamento: `aus:674` | L7 para propagação; escritor separado permanece |
| Rejeitar férias por update sem trilha/signal | CURADO | Trava: `aus:1488`; save por linha: `aus:1498`; trilha obrigatória/rollback: `aus:1499`, `aus:1506`; acordamento: `aus:1508` | L7 |
| Apagar/espelhar férias | MUDOU | Deletes: `aus:1374`, `aus:1401`, `aus:1461`; novos acordamentos: `aus:1376`, `aus:1403`, `aus:1463`; existente rejeitada ainda preservada: `aus:1385` | L7 |
| `rederivar_fim` sem invalidar fechamento | CURADO | Captura antes, salva/refresca e acorda projeção: `aus:1751`, `aus:1752`, `aus:1756` | L7; descrição antiga ainda em `app/docs/BACKLOG.md:2207` |
| Migration 0057 forward | PROCEDE | Lista congelada, update e trilha: `app/ponto/migrations/0057_a4_backfill_pendente.py:30`, `:43`, `:44` | — |
| Migration 0057 reverse | PROCEDE | Escreve `pendente`, sem trilha nesse corpo: `app/ponto/migrations/0057_a4_backfill_pendente.py:58`, `:65` | — |
| Admin Django, antes não reaberto | PROCEDE | `AusenciaAdmin(SoLeitura)`: `app/ponto/admin.py:38`; bloqueios: `app/core/admin.py:59`, `:62`, `:65` | O167 declarado: `app/ponto/admin.py:39` |
| L3 — quatro estados atuais sem terminais | PROCEDE | `aus:914`, `aus:922` | — |
| Rejeitada → aprovada sem chamador identificado | PROCEDE | Seta: `aus:922`; aprovadores selecionam abertas: `pv:2369`, `pv:2752`, `pv:2764`; reabertura deriva aguardando: `aus:1298` | Chamador externo/dinâmico **[nao verificado]** |
| `pendente` legado sem saída runtime | PROCEDE | Reverse escreve: `app/ponto/migrations/0057_a4_backfill_pendente.py:65`; filas/matriz excluem: `aus:912`, `aus:914` | Existência de linhas **[nao verificado]** |
| Carimbos antigos após reabrir | PROCEDE | Reabertura não limpa: `aus:1298`, `aus:1320`; transição não limpa aprovador/data: `aus:975`, `aus:986` | — |

Sítios que o inventário não individualizou:

| leitor/escritor adicional | resultado |
|---|---|
| API de chamados lê status legado | “Anexar comprovante” exige `status == 'pendente'`: `app/api/views.py:1061`. A tarefa usa estados atuais: `app/chamados/services/tarefa_do_dia.py:312`. |
| `Ausencia.save` deriva campos | Escreve `aprovada_em` inclusive para rejeitada, minutos e `data_fim`: `mod:882`, `mod:887`, `mod:912`, `mod:917`. |
| Copiloto pede aprovação explícita | Também alcança o defeito da reabertura: `app/ponto/services/checar_ausencia.py:39`, `:47`; `aus:433`. |
| Veto na edição | Edição usa sobreposição singular sem `linha_que_veta`, diferentemente da criação: `aus:637`, `aus:465`; precedência: `app/ponto/precedencia.py:136`. |
| Leitor/invalidador da L7 | Lê insumo pelo predicado da folha e invalida dias antes/depois: `aus:138`, `aus:192`, `aus:195`. |
| Signals de save/delete | Rejulgam células e acordam consumidores: `app/ponto/signals.py:143`, `:150`, `:256`, `:269`, `:272`. |
| Leitores dos carimbos antigos | Histórico usa aprovador/data como quem decidiu/quando: `app/ponto/services/dia_decidido.py:514`, `:520`, `:522`; vigia lê aprovação sem documento/aprovador: `app/ponto/services/vigia_ausencia.py:115`, `:120`. |
| Outros leitores de estado/período | TXT: `app/folha/export.py:87`; relatório: `app/relatorios/views.py:197`, `:205`; score usa não rejeitada: `app/inteligencia/management/commands/calcular_scores.py:12`, `:14`. |
| Residual documentado da L7 | Há fila para erro de banco engolido dentro do atomic e relógio da linha aberta: `app/docs/BACKLOG.md:858`, `:859`. Reprodução **[nao verificado]**. |

Perguntas: **Q1–Q4, Q6–Q7 e Q9** mantêm os mecanismos apontados; decisão normativa adicional **[nao verificado]**. **Q5** ganhou porta de férias com trilha/signal próprios; **Q8** ganhou acordador nos escritores listados. (`aus:433`; `pv:2670`; `app/ponto/services/fechamento.py:700`; `aus:922`; `aus:773`; `aus:1285`; `app/ponto/migrations/0057_a4_backfill_pendente.py:65`; `aus:1498`; `aus:1506`; `aus:669`; `aus:1756`)

**Resumo 08:** os três defeitos centrais permanecem; L7 curou rejeição de férias sem trilha/signal e rederivação de fim sem invalidação, além de mudar a propagação das escritas. (`aus:433`; `pv:2670`; `app/ponto/services/fechamento.py:700`; `aus:1498`; `aus:1756`)

**Mapa 09 — férias**

Abreviações: `fm` = `app/ferias/models.py`; `fs` = `app/ferias/services.py`; `fv` = `app/ferias/views.py`. (`fm:65`; `fs:644`; `fv:245`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| L1.1 — P1–P3, nascimento do PA | PROCEDE | `fv:178`, `fs:416`, `fs:425`, `fm:204` | — |
| L1.2 — P4–P16, alterações do PA | PROCEDE | Juiz/setter: `fm:65`, `fm:207`; persistências: `fs:199`, `fs:236`, `fs:399`, `fs:434`, `fs:855` | — |
| P-a — pode agendar: status × porta | PROCEDE | Property exige adquirido: `fm:234`; porta normal não exige: `fv:245`, `fs:578`; solicitação exige: `fs:764` | Residual registrado: `app/docs/RELATO.md:847` |
| P-b — vencendo: data × gravado | PROCEDE | Juiz: `fm:260`; chip/listas: `fv:36`, `fv:38`, `fv:64`; serviço: `fs:263`; mensageria: `app/api/views_mensageria.py:905` | RC1-2 registrada, sem construtor: `app/docs/BACKLOG.md:759` |
| L1.3 — A1–A3, nascimentos do AG | PROCEDE | `fs:273`, `fs:504`, `fs:556`, `fs:748`, `fm:357` | — |
| A-AP — duplicata na aprovação | PROCEDE | Normal: `fs:569`; aprovação faz `full_clean`: `fs:785`; sobreposição: `fm:396`, `fm:433` | Divergência residual de efeito **[nao verificado]** |
| L1.4 — janela/status; comentário de concluído | PROCEDE | Juiz: `fm:276`; edição/cron: `fs:708`, `fs:721`, `fs:953`; comentário inexato: `fm:304` | — |
| L1.5 — cancelamento DP de férias iniciadas | PROCEDE | Botão por data: `fm:340`, `app/templates/ferias/_secao_colab.html:159`; botão por gravado: `app/templates/ferias/saldo.html:70`; porta aceita: `fs:652` | Caso mantido em fila/lei: `app/docs/RELATO.md:842` |
| L1.5b — regra admin repetida | PROCEDE | `fm:348`, `fm:308`, `fs:678` | FERIAS-2X-FILA: `app/docs/BACKLOG.md:1994` |
| L1.6 — A12, correção excepcional de concluído | MUDOU | Permissão preservada: `fv:438`, `fs:678`; agora traduz falha da trilha: `fv:449`, `fv:452` | L7 |
| L1.7 — C1–C2, envio/reenvio | PROCEDE | Filtro externo: `fv:945`; escritor só veta cancelado e zera carimbo: `fs:887`, `fs:889` | Corrida real **[nao verificado]** |
| L1.8 — C3–C4, resposta do colaborador | PROCEDE | Filtro externo: `fv:994`; recusa não limpa carimbo: `fs:907`, `fs:908`; confirmação carimba: `fs:911` | Corrida real **[nao verificado]** |
| L1.9 — S1, aprovação sob trava | PROCEDE | `fv:800`, `fv:801`, `fv:817`; PA sob trava: `fs:764` | HJ-090; absorção registrada: `app/docs/BACKLOG.md:1965` |
| L1.10 — S2, rejeição | PROCEDE | `fv:801`, `fv:825`, `fs:795` | — |
| L2 — portas por arquivo e selo | PROCEDE | `app/core/portas.py:488`; selo reaberto: `app/ponto/tests/test_chokepoint_ausencia_gate.py:25`, `:53` | Execução do selo **[nao verificado]** |
| Admin sem PA/AG/SOL | PROCEDE | Registra somente Afastamento: `app/ferias/admin.py:2`, `:4` | — |
| Setter em memória | PROCEDE | Setter: `fm:215`; persistências: `fs:199`, `fs:587`, `fs:935` | — |
| “17 funções persistindo” | MUDOU | Censo estático: 18 funções com save/create/get_or_create, mais uma com delete. A amostra omitia `fs:316`, `fs:412`; exclusão em `fs:876`, `fs:877` | Contagem refutada; novidade temporal **[nao verificado]** |
| Limites do censo | PROCEDE | Não atribuíveis/exclusões: `app/core/censo_escritas.py:43`, `:49`, `:54` | — |
| Trilha admin/concluída × J | MUDOU | Novas capturas: `fv:395`, `fv:449`; logs de AG ainda após a porta: `fv:410`, `fv:459`; J dentro do atomic: `fv:543` | L7 não curou a separação do log de AG |
| Relógio dentro da porta | PROCEDE | `fs:654`, `fs:681`, `fs:686`, `fs:934`, `fm:94`, `fv:29`, `fv:117` | Residual registrado: `app/docs/RELATO.md:847` |
| Views sem `login_required` | PROCEDE | Controle por ação: `fv:435`, `fv:499`; recusa anônimo: `app/colaboradores/mixins.py:326` | Continua refutada a ausência de controle apenas pelo decorator |
| L3 — AG.cancelado terminal | PROCEDE | Catálogo/guardas: `fm:307`, `fs:678`, `fs:954`; exclusão do registro: `fs:875` | Por desenho no código |
| AG.concluído não terminal | PROCEDE | `fs:721`, `fs:954`, `fv:451`; comentário contraditório: `fm:304` | — |
| SOL.pendente sem nascente produtiva identificada | PROCEDE | Default: `fm:481`; leitores: `fv:41`, `fv:785`; escritores de avaliação: `fs:756`, `fs:797` | Canal externo/linhas existentes **[nao verificado]** |
| SOL.aprovada/rejeitada terminais pela nascente | PROCEDE | Guarda pendente: `fv:802`; destinos: `fs:756`, `fs:797` | Por desenho no código |
| CONF.confirmado terminal na view, não no escritor | PROCEDE | `fv:945`, `fs:887`, `fs:888` | — |
| Batida em férias: auxiliar por datas | PROCEDE | `app/ponto/utils.py:87`, `:89`; detector reaberto: `app/chamados/detector_anomalias.py:115` | Conclusão global sobre todas as guardas **[nao verificado]** |
| Colaborador.situacao × férias | PROCEDE | Leitura: `app/api/views.py:403`; campos do espelho: `fm:442` | Escritor de férias alterando situação não identificado **[nao verificado]** |

Sítios que o inventário não individualizou:

| leitor/escritor adicional | resultado |
|---|---|
| Relatório/PDF de férias | Também filtra vencido pelo gravado e vencendo por adquirido + literal 60: `app/relatorios/services.py:286`, `:288`. |
| Alertas de inteligência | Vencidas pelo gravado; vencendo pelo juiz: `app/inteligencia/management/commands/gerar_alertas.py:86`, `:100`, `:103`. |
| API do colaborador | Expõe status gravado do AG: `app/api/views.py:2244`, `:2254`. |
| Cron de aviso | Seleciona em_curso/adquirido e sincroniza pela porta: `app/ferias/management/commands/alertar_ferias_vencendo.py:22`, `:32`. |
| Preview/intake | Pergunta ao validador da view, propagando P-a: `fs:346`, `fs:347`; contraste com `fm:234`. |
| Catálogo das frações no aviso | Continua copiado: `app/ferias/situacao.py:119`, `fm:364`; família documentada: `app/docs/BACKLOG.md:747`. |
| Persistências omitidas da amostra L2 | Intake grava dias vendidos/status e art.130 grava direito: `fs:316`, `fs:412`; já mencionados em L1, mas ausentes da amostra de persistências. |
| Template de confirmação | Lê diretamente confirmação para badge/envio: `app/templates/ferias/_secao_colab.html:122`, `:124`. |

Perguntas: **Q1–Q3** preservam as divergências de cancelamento, agendamento e vencimento; **Q4–Q5** preservam carimbo e guardas externas; **Q6** segue sem nascente produtiva identificada; **Q7** mantém relógios internos; **Q8** mantém comentário inexato. Sentido normativo dos campos e ocorrências reais: **[nao verificado]**. (`fs:652`; `fm:234`; `fs:263`; `fs:908`; `fv:945`; `fv:994`; `fv:785`; `fs:654`; `fm:304`)

**Resumo 09:** os três defeitos centrais permanecem; L7 mudou a apresentação da falha de trilha nas views administrativas; o censo de “17 funções” estava incompleto. (`fm:234`; `fs:263`; `fs:652`; `fv:395`; `fv:449`; `fs:316`; `fs:412`; `fs:876`)

**Mapa 10 — feriado/calendário**

Abreviações: `vc` = `app/core/views_config.py`; `fer` = `app/core/feriados.py`; `cal` = `app/colaboradores/services/calendario.py`; `m13` = `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py`. (`vc:41`; `fer:57`; `cal:97`; `m13:20`)

| item | estado | arquivo:linha HEAD | obra |
|---|---|---|---|
| T1 — nacional manual × importação | PROCEDE | Form aceita abrangência/fonte: `vc:50`, `vc:55`, `vc:83`; importação exige allowlist: `vc:113`, `vc:140` | — |
| T1b — unicidade nacional × UF/município | PROCEDE | Form preserva campos: `vc:56`, `vc:85`; importação usa vazios: `vc:145`; chave: `app/core/mod

[truncado em 20000 de 26104 caracteres]

## 17/11 08:12 · sessao e339e34d0a75 · - · - · origem=Ronald (tmux)

Inventário de escritores por leitura estática; execução e incidência em produção [nao verificado].

1. **Bloqueia — RESPOSTA automática conserva validação que a F2 remove.** `responder_automaticamente` só recusa texto já existente em `app/chamados/services/disputa_emissao.py:1663`, escreve resposta/instante em `:1674`, salva só esses campos em `:1677` e registra RESPOSTA comum em `:1678`. Não há guarda de `validada_em`. O chamador declara explicitamente que não filtra validação em `app/api/foto_recebida.py:32`, responde em `:48` e só depois chama o dono em `:51`; este retorna `ja_validada` sem alterar carimbo em `app/chamados/services/validacao.py:685`. Contraexemplo: projeção legal `(validada_em=T1, respondida_em=None, resposta_colab='')`; resposta automática em `T2>T1`. Projeção termina `(T1,T2,texto)`, mas a leitura termina `(None,T2,texto)`, porque a foto VALIDACAO preservada em `disputa_emissao.py:1673` vem antes da RESPOSTA que desfaz em `app/chamados/models.py:4201` e `:4232`. O teste cobre automática **antes** de validar, não automática sobre validada (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:184`, `:191`).

2. **Bloqueia para coerência do escritor; incidência fora dos ilegais [nao verificado] — trilha também conserva validação.** A guarda só pergunta instante/texto em `app/chamados/services/disputa_emissao.py:1692`; escreve só instante em `:1696`, salva em `:1698` e registra RESPOSTA comum em `:1699`. Se houver validação em T1 e a nota testemunhar resposta T2>T1, a projeção conserva T1 e a leitura a limpa (`app/chamados/models.py:4232`). A entrada desse escritor exige texto sem instante, portanto pertence ao par ilegal excluído do corte (`disputa_emissao.py:1692`). O chamador do comando não guarda validação (`app/chamados/management/commands/curar_perguntas_em_limbo.py:55`, `:67`).

3. **Escritores de V/R/T e respectivos atos:**

| Escritor | Campos escritos | Linha gravada no mesmo ato | Coerência |
|---|---|---|---|
| Nascimento `app/chamados/models.py:3149`, `:3152` | Campos recebidos por `**campos` | Fotos RESPOSTA/VALIDACAO por `:3154`, corpo em `:4173`/`:4177` | Fotos mantêm projeção; futuros atos podem removê-la (`:4224`, `:4232`). |
| Resposta humana `app/chamados/services/disputa_emissao.py:1481` | Texto e instante; V=None em `:1494` | Fotos antigas em `:1480`, RESPOSTA em `:1525` | Coerente na ordem cronológica usual: RESPOSTA desfaz V (`models.py:4201`). |
| Automática `disputa_emissao.py:1674` | Texto/instante; não altera V | Fotos + RESPOSTA em `:1678` | Contraexemplo no achado 1. |
| Pela trilha `disputa_emissao.py:1696` | Instante; não altera V/T | RESPOSTA em `:1699` | Contraexemplo no achado 2. |
| Validação `app/chamados/services/validacao.py:168` | V=agora | Fotos + VALIDACAO em `:173` | VALIDACAO atribui V em `models.py:4230`. |
| Devolução da recusa `validacao.py:273` | V=None | Fotos + DEVOLUCAO em `:279` | DEVOLUCAO limpa V (`models.py:4189`, `:4232`). |
| BUG-PAR-103 `validacao.py:349` | V=None e descarimbo em `:346` | Fotos + DEVOLUCAO em `:354` | Limpa V; preserva par (`models.py:4232`, `:4234`). |
| Reabrir/recusar `disputa_emissao.py:1609`, `:1648` | V=None, R=None, T='' | REABERTURA/RECUSA com foto em `:1631`; guardas do universo em `:1645` | A dobra limpa os três (`models.py:4189`, `:4190`, `:4232`, `:4234`). |
| Ressuscitar `models.py:1647` | V=None, R=None, T='' | Só fotos anteriores em `:1629`; nenhuma limpeza | Não conserva validação; teste caracteriza expressamente (`test_e4c_f2_leitura_do_ato.py:369`). | [nao verificado]
| Estoque I-sem-T `validacao.py:440` | R=None, T='' | Só fotos em `:439`; nenhuma limpeza | Leitura conserva instante anterior, embora o juiz dê falso pelo texto vazio (`models.py:4228`). Entrada é ilegal (`validacao.py:408`). |
| Carimbar `models.py:3271`, `:3274` | R=quando e/ou V=quando se faltantes e texto presente | Nenhuma linha própria em `:3291`; leitura usa passivo `:4221` | Funciona enquanto não houver correspondência histórica conflitante no mesmo instante (`:4173`, `:4177`). |
| Simulação de ressuscitação `models.py:1569` | V=None, R=None, T='' em cópia | Nenhuma | Só memória; cópia em `:1556`. |
| Desfazer carimbo recusado `models.py:3302` | R/V retornam aos valores anteriores | Nenhuma | Só memória; chamada em `:3280`/`:3283`/`:3294`. |

4. **Escritores físicos de materializada/via.** `_escrever_carimbo` atribui `materializada_em` e `via_resolucao` em `app/chamados/models.py:3238` e `:3240`; persistências pelas portas `carimbar` em `:3291`, `carimbar_identidade` em `:3329`, `descarimbar` em `:3351`; lote grava `materializada_em=None, via_resolucao=''` em `:3420`. Além disso, `encerrar_por_fato_no_espelho` muda só via em `:3431`/`:3432`, e `relavrar` muda só via em `app/chamados/services/relavra_via.py:125`/`:127`. Nascimento recebe quaisquer desses campos por `**campos` (`models.py:3149`). A F2 não dobra materializada/via (`models.py:4236`).

5. **Chamadores das portas do carimbo**, para inventário completo dos responsáveis sem duplicar atribuições:

- `app/chamados/models.py:1647`, `:2026`, `:2167`, `:3203`, `:3443`, `:3454`, `:3471`, `:3478`.
- `app/chamados/reconciliador.py:407`, `:597`, `:1150`.
- `app/chamados/services/relavra_via.py:246`.
- `app/chamados/services/fio.py:1410`.
- `app/chamados/services/validacao.py:346`, `:440`, `:520`, `:560`.
- `app/chamados/services/competencia_trancada.py:279`.
- `app/chamados/services/materializacao.py:1046`, `:1461`, `:1686`, `:1968`, `:2167`.
- `app/chamados/services/disputa_emissao.py:1499`, `:1648`, `:2008`, `:2082`.
- `app/ponto/services/veredito_celula.py:190`.
- Comandos: `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`.

6. **Escritores de DecisaoPergunta.** Escrita física única localizada é `app/chamados/models.py:4249`: `bulk_create([cls(**ln)...])`; recebe ato/instante/texto/motivo dos produtores seguintes. A linha existente recusa `save` em `:4136`; lote recusa update/delete/bulk_update em `:4062`. PK é gerado na inserção [nao verificado: mecanismo do Django/banco não foi executado]; nenhum produtor listado atribui PK explicitamente (`:4249`).

| Produtor | Atos |
|---|---|
| `app/chamados/models.py:1629` | Fotos RESPOSTA/VALIDACAO antes da ressuscitação |
| `models.py:2030` | SILENCIO do fechamento |
| `models.py:2162` | RETRATACAO |
/page| `models.py:3154` | Fotos do nascimento |
| `models.py:4175`, `:4179` | Corpo que fabrica fotos com motivo especial |
| `app/chamados/services/disputa_emissao.py:1525` | Fotos + RESPOSTA humana |
| `disputa_emissao.py:1631` | RECUSA/REABERTURA |
| `disputa_emissao.py:1678` | Fotos + RESPOSTA automática |
| `disputa_emissao.py:1699` | RESPOSTA pela trilha |
| `app/chamados/services/validacao.py:173` | Fotos + VALIDACAO |
| `validacao.py:279`, `:354` | Fotos + DEVOLUCAO |
| `validacao.py:439` | Fotos do estoque I-sem-T |
| `app/chamados/services/materializacao.py:1048` | ABSORCAO |
| `app/ponto/services/veredito_celula.py:195` | SILENCIO |
| `app/chamados/management/commands/backfill_fio_morto_e4b.py:141` | Fotos |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:324` | SILENCIO |

A migração também muda a representação histórica: renomeia texto/instante/motivo e introduz `ato='reabertura'` nas linhas anteriores, em `app/chamados/migrations/0051_decisao_pergunta.py:42` e `:50`. Não introduz escritor novo no diff F2 apresentado [nao verificado: ausência de metadados git na cópia].

## 17/11 08:11 · sessao 9ff7a0da1629 · CODEX-RESERVA-R3 · leitura · origem=Code

**Os 12 permanecem “sem causa” fechada.** O resumo R3 informa o sintoma, mas não traz as batidas com tipos, papéis, carimbos e `data_turno` necessários para identificar o ramo executado em cada caso. [não verificado]

O caminho do código a discriminar é:

1. `turnos_do_colab` carrega os papéis da ata, segmenta pelo vínculo do dia e chama `parear_turnos` sobre cópias das batidas. (`app/ponto/turnos.py:1944`, `:1965-2000`)
2. O pareador prioriza o papel da ata; a pausa depende dos carimbos e das bordas. Há reconhecimento por duração mesmo sem marcos, e pares declarados/ata podem passar apesar dos tipos invertidos. (`app/ponto/turnos.py:392-418`, `:811-883`, `:1310-1326`, `:1382`)
3. `_data_do_turno` decide a atribuição à véspera; o realizado seleciona por `data_turno` e desconta pausa reconhecida ou janela declarada. (`app/ponto/turnos.py:285-309`, `:719-771`)
4. A tela monta os cards pelos períodos do motor, enquanto `minutos_realizados` vem do realizado do juiz, com intervalo do vínculo escolhido. (`app/ponto/services/espelho.py:286-294`, `:450-460`, `:675`, `:783`)

Na tabela, **“?” significa hipótese de triagem**, sem dono comprovado pela L-099. (`app/docs/LEIS.md:122`)

| colNNN | Dia | Causa | Arquivo:linha — regra a discriminar | Dono provável | Precisa sonda |
|---|---|---|---|---|---|
| col369 | 2026-10-12 | **sem causa** — falta identificar os carimbos/papéis que recusaram o vão e o intervalo efetivo. [não verificado] | `app/ponto/turnos.py:811-883`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col864 | 2026-10-12 | **sem causa** — folga/feriado não demonstra, sozinho, ausência de reconhecimento por duração. [não verificado] | `app/ponto/turnos.py:392-418`, `:1217-1221`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col114 | 2026-11-08 | **sem causa** — o vão relatado não revela se ambas as pontas eram interiores e reconhecidas pela autoridade. [não verificado] | `app/ponto/turnos.py:811-883`, `:545-546` | ESTRUTURA? [não verificado] | S |
| col114 | 2026-11-15 | **sem causa** — faltam papéis, marcas e pausa efetivamente declarada pelo dia. [não verificado] | `app/ponto/turnos.py:811-883`; `app/ponto/services/espelho.py:286-294` | ESTRUTURA? [não verificado] | S |
| col723 | 2026-10-08 | **sem causa** — falta distinguir recusa do par, reconhecimento de outro par e desconto da janela declarada. [não verificado] | `app/ponto/turnos.py:811-883`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col746 | 2026-09-24 | **sem causa** — faltam bordas, carimbos e intervalo efetivo para explicar o desconto do vão relatado. [não verificado] | `app/ponto/turnos.py:811-883`, `:756-766` | ESTRUTURA? [não verificado] | S |
| col876 | 2026-09-21 | **sem causa** — “fora do turno” não identifica tipo, papel, motivo da separação nem dia atribuído à ponta. [não verificado] | `app/ponto/turnos.py:1382`, `:1402-1419`, `:285-309` | BATIDA? [não verificado] | S |
| col130 | 2026-10-29 | **sem causa** — faltam os turnos completos para distinguir ponta solta de segmento atribuído a outro turno/dia. [não verificado] | `app/ponto/turnos.py:285-309`, `:1417-1419`, `:719-721` | BATIDA? [não verificado] | S |
| col220 | 2026-11-10 | **sem causa** — falta saber qual decisão de papel, absorção ou fechamento deixou a batida fora. [não verificado] | `app/ponto/turnos.py:1310-1326`, `:1371-1382`, `:1430-1457` | BATIDA? [não verificado] | S |
| col868 | 2026-09-22 | **sem causa** — escala diurna diante de batidas noturnas sustenta candidata CADASTRO; faltam vínculo escolhido, geradora/DNA e papéis para comprová-la. [não verificado] | `app/ponto/turnos.py:1695-1700`, `:1965-2000`; `app/docs/LEIS.md:122` | CADASTRO? [não verificado] | S |
| col516 | 2026-10-23 | **sem causa** — folga com realizado e nenhum turno E6 não distingue atribuição do dia, pareamento e fonte do número. [não verificado] | `app/ponto/turnos.py:719-771`; `app/ponto/services/espelho.py:450-460`; `app/ponto/management/commands/e6_oraculo.py:263-269` | Indeterminado [não verificado] | S |
| col884 | 2026-11-08 | **sem causa** — inversão de tipos não prova recusa: pares declarados/ata/mistos podem passar. Atestado rejeitado não explica sozinho o realizado. [não verificado] | `app/ponto/turnos.py:811-815`, `:875-883`, `:2528-2531` | BATIDA? [não verificado] | S |

**Total por causa:** sem causa = **12**; causas nomeadas confirmadas por caso = **0**. Consolidação do dado fornecido, sem reconsulta de produção. [não verificado]

Duas ressalvas da cópia: `turnos.py:502-567` contém leitores de intervalo; `parear_turnos` começa em `:1192`. O trecho `:2798-2814` expande datas de ausência; o filtro de rejeitada está em `:2528-2531`. Além disso, realizado não comprova pagamento: a linha financeira da tela imprime `rubricas`. (`app/templates/ponto/espelho.html:268-286`)

A sonda única abaixo cobre os 12. Usa as funções reais, captura as **cópias carimbadas** e informa a fonte do número. (`app/ponto/services/espelho.py:1194`; `app/ponto/turnos.py:1984-2000`; `app/ponto/management/commands/e6_oraculo.py:263-269`)

Execute em contexto Django **já inicializado**, no tenant correto, com `PK_POR_COL` confirmado. A equivalência entre o rótulo raspado `colNNN` e o PK não está demonstrada no dado; por isso o script não inventa essa correspondência. Execução e resultado em produção: [não verificado].

```python
# Contexto Django JA inicializado, tenant correto, sem redirecionar para arquivo.
# Definir previamente PK_POR_COL: mapa confirmado dos rotulos abaixo para PKs.
import json
import logging
from contextlib import ExitStack
from datetime import date, datetime, time
from decimal import Decimal

from django.apps import apps
from django.db import connections, transaction, DatabaseError
from django.db.models import Q
from django.utils import timezone

if not apps.ready:
    raise RuntimeError("Requer contexto Django ja inicializado")

CASOS = [
    ("col369", "2026-10-12", 10), ("col864", "2026-10-12", 10),
    ("col114", "2026-11-08", 11), ("col114", "2026-11-15", 11),
    ("col723", "2026-10-08", 10), ("col746", "2026-09-24", 10),
    ("col876", "2026-09-21", 10), ("col130", "2026-10-29", 11),
    ("col220", "2026-11-10", 11), ("col868", "2026-09-22", 10),
    ("col516", "2026-10-23", 11), ("col884", "2026-11-08", 11),
]
mapa = globals().get("PK_POR_COL", {})
if any(col not in mapa for col, _, _ in CASOS):
    raise RuntimeError("Falta PK_POR_COL confirmado para todos os rotulos")

schema = getattr(connections["default"], "schema_name", None)
if schema == "public":
    raise RuntimeError("Selecionar o tenant correto antes da sonda")

def serializar(x):
    if isinstance(x, (date, datetime, time)):
        return x.isoformat()
    if isinstance(x, Decimal):
        return str(x)
    # Impede serializacao acidental de modelo por __str__.
    raise TypeError("Objeto nao permitido: " + type(x).__name__)

def emitir(tipo, **dados):
    print(json.dumps(dict(tipo=tipo, **dados), default=serializar,
                     ensure_ascii=False))

def campos(obj, nomes):
    return {k: getattr(obj, k) for k in nomes.split()}

falhas_sql = []

def vigiar(execute, sql, params, many, context):
    try:
        return execute(sql, params, many, context)
    except DatabaseError as exc:
        # Falha capturada internamente pelo espelho tambem impede FIM_OK.
        falhas_sql.append(dict(alias=context["connection"].alias,
                               classe=type(exc).__name__))
        raise

BATIDA = (
    "pk timestamp tipo sequencia is_intervalo origem "
    "aprovada retratada_em escala_id"
)
FLAGS = "_papel_ata _intra_dur _intra_ata _intra_declarado _eco_flush"
LINHA = (
    "minutos_realizados pago_h rubricas realizado_sem_turno realizado_aberto "
    "realizado_longo realizado_janela_descontada folga feriado veredito_dia"
)
GRADE = "tipo hora hora_marco tipo_real missing delta status"
EC = "pk tipo_escala_id data_inicio data_fim ativa trabalha_em_feriado"
TE = (
    "pk tipo_ciclo intervalo_modo hora_inicio hora_fim "
    "hora_inicio_intervalo hora_fim_intervalo intervalo_duracao_min"
)

nivel = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    from colaboradores.models import Colaborador
    from escala.alimentacao import (
        celulas_do_periodo, escalas_do_periodo, vinculo_do_dia,
    )
    from ponto.models import Batida, Ausencia
    from ponto.janelas import janela_fechamento
    from ponto.services.espelho import (
        espelho_do_colab, _realizado_do_dia_tela,
    )
    from ponto.management.commands.e6_oraculo import (
        InsumosDoDono, minutos_do_espelho,
    )
    from ponto.juiz_batida import periodos_do_dia
    from ponto.turnos import (
        turnos_do_colab, janela_com_pad, papel_por_minuto_da_ata,
        dia_das_batidas, _papel_da_ata, _pares_marcados, _fim_fechado,
        intervalos_do_turno, pausas_abaixo_do_piso,
    )

    agora = timezone.now()
    hoje = timezone.localdate(agora)

    with ExitStack() as stack:
        aliases = list(connections)
        for alias in aliases:
            conn = connections[alias]
            if conn.vendor != "postgresql" or conn.in_atomic_block:
                raise RuntimeError(
                    alias + ": requer PostgreSQL sem transacao aberta"
                )
            stack.enter_context(transaction.atomic(using=alias))
            with conn.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cur.execute("SHOW transaction_read_only")
                if cur.fetchone()[0] != "on":
                    raise RuntimeError(alias + ": READ ONLY nao ativado")
            stack.enter_context(conn.execute_wrapper(vigiar))

        emitir("contexto", agora=agora, schema=schema,
               fotografia="estado atual; nao restaura o snapshot R3")
        cache = {}

        for col, iso, mes in CASOS:
            d = date.fromisoformat(iso)
            chave = (col, mes, d.year)

            if chave not in cache:
                c = Colaborador.objects.get(pk=mapa[col])
                ini, fim = janela_fechamento(mes, d.year, c.empresa)

                # Mesma entrada publica usada pelo E6:
                # app/ponto/management/commands/e6_oraculo.py:316
                esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
                vi, vf = esp["data_ini"], esp["data_fim"]
                lo, hi = janela_com_pad(vi, vf)

                cels = celulas_do_periodo([c.pk], lo, hi)
                por_data = {dd: ce for (_, dd), ce in cels.items()}
                tela_cels = {
                    k: ce for k, ce in cels.items() if vi <= k[1] <= vf
                }
                ecs = sorted(escalas_do_periodo(
                    c, vi, vf,
                    mais=[ce.escala_geradora_id
                          for ce in tela_cels.values()],
                ), key=lambda e: e.data_inicio)

                bs = list(Batida.objects.filter(
                    colaborador=c, timestamp__date__range=(lo, hi),
                ).order_by("timestamp", "pk"))

                papel = papel_por_minuto_da_ata(
                    c, lo, hi, celulas=por_data,
                    batidas=[b for b in bs if b.retratada_em is None],
                )
                ts = turnos_do_colab(c, vi, vf, celulas=por_data)
                dias_b = dia_das_batidas(c, vi, vf, turnos=ts)
                ins = InsumosDoDono(
                    c, ini, fim, hoje, espelho=esp, agora=agora,
                )
                cache[chave] = (
                    c, ini, fim, esp, cels, tela_cels, ecs,
                    bs, papel, ts, dias_b, ins,
                )

            (c, ini, fim, esp, cels, tela_cels, ecs,
             bs, papel, ts, dias_b, ins) = cache[chave]

            linhas = [x for x in esp["dias"] if x["data"] == d]
            if len(linhas) != 1:
                raise RuntimeError(col + " " + iso + ": falta linha unica")

            de = linhas[0]
            numero, fonte = minutos_do_espelho(de)
            releitura = _realizado_do_dia_tela(
                c, d, ts, ecs, tela_cels,
            )

            emitir(
                "alvo", col=col, dia=d, competencia=[mes, d.year],
                janela=[ini, fim],
                visual=[esp["data_ini"], esp["data_fim"]],
                numero=numero, fonte=fonte,
                linha={k: de.get(k) for k in LINHA.split()},
                grade=[
                    {k: x.get(k) for k in GRADE.split()}
                    for x in (de.get("realizado_grade") or [])
                ],
                realizado_releitura=releitura._asdict(),
                e6_tem_chave=iso in ins.oraculo.minutos,
                e6_minutos=ins.oraculo.minutos.get(iso),
                e6_horas=ins.horas.get(iso),
                e6_envelope=ins.oraculo.envelope.get(iso),
                e6_corte=iso in ins.oraculo.cortes,
                # Classificacao automatica; nao confirma a causa factual.
                dono_automatico=ins.dono(d),
                cards=[
                    dict(entrada=p.entrada, saida=getattr(p, "saida", None),
                         aberto=p.turno_aberto)
                    for p in de["turnos"]
                ],
            )

            flo, fhi = janela_com_pad(ini, fim)
            pks_foco = set()

            for b in bs:
                if flo <= timezone.localdate(b.timestamp) <= fhi:
                    pks_foco.add(b.pk)
                    emitir(
                        "batida", col=col, alvo=d, dados=campos(b, BATIDA),
                        instante_local=timezone.localtime(b.timestamp),
                        papel_consultado=_papel_da_ata(b, papel),
                        dia_exibicao=dias_b.get(b.pk),
                    )

            # Carimbos nas COPIAS usadas pelo pareador:
            # app/ponto/turnos.py:1984-2000
            for t in ts:
                if not (
                    flo <= t.data_turno <= fhi
                    or any(b.pk in pks_foco for b in t.batidas)
                ):
                    continue
                emitir(
                    "turno", col=col, alvo=d, data_turno=t.data_turno,
                    entrada_pk=getattr(t.entrada, "pk", None),
                    saida_pk=getattr(t.saida, "pk", None),
                    fim_contado_pk=getattr(_fim_fechado(t), "pk", None),
                    aberto=t.aberto, cross_meianoite=t.cross_meianoite,
                    pares_marcados=_pares_marcados(t),
                    pausas_validas=intervalos_do_turno(t),
                    pausas_abaixo_piso=pausas_abaixo_do_piso(t),
                    batidas=[
                        dict(campos(b, BATIDA), **{
                            k: getattr(b, k, None) for k in FLAGS.split()
                        })
                        for b in t.batidas
                    ],
                )

            contexto_ecs = escalas_do_periodo(
                c, flo, fhi,
                mais=[ce.escala_geradora_id for ce in cels.values()],
            )
            for e in contexto_ecs:
                emitir(
                    "vinculo_candidato", col=col, alvo=d,
                    dados=campos(e, EC), template=campos(e.tipo_escala, TE),
                )

            datas = {d} | {
                dd for _, dd in cels if flo <= dd <= fhi
            }
            for dd in sorted(datas):
                ce = cels.get((c.pk, dd))
                e = vinculo_do_dia(c.pk, dd, contexto_ecs, celulas=cels)
                te = e.tipo_escala if e else None
                emitir(
                    "celula", col=col, alvo=d, dia=dd,
                    celula=(
                        campos(ce, "pk trabalha origem escala_geradora_id")
                        if ce else None
                    ),
                    dna_marcos=((ce.dna or {}).get("marcos") if ce else None),
                    juiz=periodos_do_dia(ce, te) if ce else None,
                    vinculo_escolhido=campos(e, EC) if e else None,
                    livre=te._intervalo_livre() if te else None,
                    intervalo_efetivo=(
                        e.intervalo_do_dia(dd, celulas=cels) if e else None
                    ),
                )

            aus = Ausencia.objects.filter(
                colaborador=c, data__lte=fhi,
            ).filter(
                Q(data__gte=flo) | Q(data_fim__gte=flo)
                | Q(data_fim__isnull=True, tipo__in=Ausencia.TIPOS_EM_ABERTO)
            )
            for a in aus:
                emitir(
                    "ausencia", col=col, alvo=d,
                    dados=campos(
                        a, "pk tipo data data_fim status minutos hora_ini hora_fim"
                    ),
                )

        if falhas_sql:
            raise RuntimeError(
                "Consulta falhou, inclusive em ramo capturado: "
                + json.dumps(falhas_sql)
            )
        for alias in aliases:
            transaction.set_rollback(True, using=alias)

    print("FIM_OK")
finally:
    logging.disable(nivel)
```

## 17/11 08:11 · sessao 8794266ec695 · CODEX-RESERVA-R6 · leitura · origem=Code

**CODEX-RESERVA-R6 — leituras herdadas**

Os números dos relatos abaixo são **registros históricos**, com suas fontes. Os scripts são propostas para execução no shell Django do tenant correto; seus resultados em produção estão **[nao verificado]**. `FIM_OK` indica conclusão da sonda, sem significar aprovação do critério de negócio.

---

**O275**

- **Pedido:** cadastrar “feriado é dia de trabalho” no `TipoEscala`, desligado por padrão, com leitura única em `eh_dia_trabalho_calculado`. A leitura deveria distinguir esse cadastro do flag do vínculo e esclarecer quais ciclos seriam alcançados. (`app/docs/BACKLOG.md:1651`; `app/docs/RELATO-ARQUIVO.md:750`)
- **Estado:** INTERROMPIDA; pouso retido na escolha FORMA A × FORMA B. (`app/docs/BACKLOG.md:1651`; `app/docs/GRADE-MESTRA.md:1769`)
- **Resposta:** “tipo × vínculo” **já foi respondido pelo censo da própria O275**: perguntas disjuntas, com precedência por `base`. Continua aberta a escolha entre alcançar todo ciclo salvo intermitente — A — ou somente `6x1/5x2/semanal` — B. (`app/docs/RELATO-ARQUIVO.md:750`; `app/docs/RELATO-ARQUIVO.md:751`)
- **Prova no código atual:** a fase de `12x36/24x48` retorna diretamente; no restante, o leitor apenas transforma trabalho em folga por `feriado_vira_folga`. O flag existente pertence ao vínculo e sua aplicação restringe ciclos semanais. O comportamento inverso pedido pela O275 ainda não está nesse leitor. (`app/escala/models.py:1289`; `app/escala/models.py:1365`; `app/escala/models.py:1369`; `app/escala/models.py:896`; `app/escala/models.py:1392`)
- **Próximo passo:** resolver A/B e retomar a construção registrada na raia; produção não decide essa escolha de lei. A pendência permanece na lista posterior de leis abertas. (`app/docs/GRADE-MESTRA.md:1769`; `app/docs/RELATO.md:837`)

---

**LAUDO-col33 — LAUDO-[nome]-COL33-0310**

- **Pedido:** rejulgar células antigas da competência 10 após a cura ATA-X-TURNO; caso nominal: célula `120035`, chamado `28365`. A leitura deveria responder se faltava cura, rejulgamento ou corte para o restante. (`app/docs/BACKLOG.md:1813`)
- **Estado herdado:** universo A aplicado; restante aguardando corte. A GRADE ainda registra PARADA. (`app/docs/BACKLOG.md:1813`; `app/docs/GRADE-MESTRA.md:250`)
- **Resposta:** **o caso nominal já foi respondido**: o universo A registrou célula concorde e chamado resolvido. O restante recebeu depois o corte **REJULGAR-COMP10-RESTO-DA-FROTA**, com atos G1/G2 e G3-col444. Portanto, “restante aguarda corte” envelheceu. (`app/docs/RELATO.md:4264`; `app/docs/RELATO.md:4265`; `app/docs/CORTES.md:360`; `app/docs/RELATO.md:4078`; `app/docs/RELATO.md:4079`; `app/docs/RELATO.md:4083`)
- **Prova no código atual:** a cura procura a saída órfã no dia seguinte quando o turno cruza a meia-noite e a transfere para o marco correspondente. O teste descreve precisamente col33, entrada em 03/10 e saída 08:59 de 04/10. (`app/escala/utils.py:1971`; `app/escala/utils.py:1996`; `app/escala/utils.py:2039`; `app/escala/tests/test_saida_valida_cross_meia_noite.py:5`)
- **Próximo passo:** acompanhar o residual **SOMBRA-X-PROD-COL988**, ainda aberto no estado revisado de CORTES. Os pendentes históricos col865/923/991 e col988 exigiam nova medida após reversão; situação viva **[nao verificado]**. (`app/docs/CORTES.md:102`; `app/docs/RELATO.md:4081`)

Sonda do caso nominal e das células residuais citadas nos atos; consulta somente estado persistido. (`app/docs/BACKLOG.md:1813`; `app/docs/RELATO.md:4078`; `app/docs/RELATO.md:4079`; `app/docs/RELATO.md:4081`)

```python
from django.db import connection, transaction
from escala.models import CelulaDia
from chamados.models import ChamadoColaborador

ids = {
    120035, 124639, 124645, 108785, 106809,
    124666, 124669, 124670,
}
with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    vistas = set()
    for c in CelulaDia.objects.filter(pk__in=ids).order_by("pk"):
        vistas.add(c.pk)
        ata = c.ata or {}
        print("CELULA", f"col{c.colaborador_id}", c.pk, c.data,
              c.veredito, c.veredito_via, c.julgada_em,
              {"n_missing": ata.get("n_missing"),
               "n_orfas": ata.get("n_orfas"),
               "minutos_realizados": ata.get("minutos_realizados")})
    print("CELULAS_NAO_LOCALIZADAS", sorted(ids - vistas))
    for ch in ChamadoColaborador.objects.filter(
        pk=28365, colaborador_id=33
    ):
        print("CHAMADO", "col33", ch.pk,
              ch.status_local, ch.via_encerramento)
print("FIM_OK")
```

---

**V3-E1**

- **Pedido:** bateria/arnês da v3; esclarecer a interrupção e revalidar os lotes. (`app/docs/BACKLOG.md:2244`; `app/docs/GRADE-MESTRA.md:1007`)
- **Estado:** INTERROMPIDA, etiqueta `MOTIVO-NAO-SEI`; bloqueios registrados C06/C16/24x48. A GRADE ainda fala em lotes não validados. (`app/docs/BACKLOG.md:2244`; `app/docs/GRADE-MESTRA.md:1775`)
- **Resposta:** o motivo pode ser especificado pelo registro posterior: **25 casos VALIDADOS verdes na v3; C05/C06/C16/C17 PENDENTE_DP; STATUS com 14 falhas em 11 casos**. O antigo `VALIDADOS=0` foi superado. **C17 já recebeu resposta:** HE 0 e intra 60, permanecendo pendente de conferência DP/contador. (`app/docs/RELATO.md:2985`; `app/docs/RELATO.md:2987`; `app/docs/RELATO.md:2994`)
- **Prova no código atual:** intra usa o máximo entre intervalo declarado, cadastrado e piso; o limite de HE é `previsto + min(suprimido, declarado)`. Assim, indenização de intra não gera automaticamente HE adicional. Para noturnas, o calculador declara falta de entrada quando não recebe os parâmetros de prorrogação e hora reduzida. (`app/ponto/calculador/regras.py:760`; `app/ponto/calculador/regras.py:764`; `app/ponto/calculador/regras.py:771`; `app/ponto/calculador/regras.py:613`)
- **Próximo passo:** recuperar o arnês da referência registrada, separar falhas STATUS das dúvidas de valor e resolver C06/C16/24x48 com o dono/DP. Os casos 24x48 perguntam qual norma fixa intervalo, noturno, extra e antecipada; o número produzido pelo código não resolve essa escolha. (`app/docs/BACKLOG.md:2244`; `app/docs/RELATO.md:2985`; `app/docs/RELATO.md:3231`; `app/docs/RELATO.md:3232`)

---

**V3-RETOMA-PERIODOS**

- **Pedido:** eliminar reconstruções por pontas, usando os períodos/pausas da mesma fonte do espelho; HE por período, transbordo reverso e parada quando faltar período na célula. A leitura deveria verificar a cura da classe inteira. (`app/docs/BACKLOG.md:1056`; `app/docs/RELATO.md:2778`)
- **Estado:** INTERROMPIDA; P4 incompleto, P5 pousou isoladamente. (`app/docs/BACKLOG.md:1056`; `app/docs/GRADE-MESTRA.md:1777`)
- **Resposta:** **a cura não está integral**. A alimentação já lê pontas efetivas e intervalos carimbados pelo motor, mas o núcleo ainda deriva pausa pelo gap entre pares. No fluxo de rubricas, célula ausente não provoca a parada exigida; a lavratura do oráculo ainda captura exceção genericamente e registra erro. (`app/ponto/calculador/alimentacao.py:76`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/nucleo.py:149`; `app/ponto/calculador/alimentacao.py:476`; `app/ponto/calculador/alimentacao.py:489`; `app/ponto/services/fechamento.py:1297`)
- **Prova posterior:** **P5-PRORROGACAO-POR-INTERVALO**, caminho b, pousou como `16c27de8`. A função atual recorta o desconto dos intervalos e limita a parte ao todo; esse pouso resolve P5, sem concluir P4. (`app/docs/RELATO.md:8703`; `app/docs/RELATO.md:8710`; `app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:191`; `app/ponto/motor_calculo_v2.py:196`; `app/docs/BACKLOG.md:1056`)
- **Próximo passo:** resolver o período não guardado e concluir P4. Essa conclusão estrutural já é possível pela leitura do código. (`app/docs/GRADE-MESTRA.md:1777`; `app/ponto/services/fechamento.py:1297`)

---

**JANELA-DO-NOTURNO**

- **Pedido:** permitir cadastrar a janela noturna da CCT. A leitura deveria responder se existe configuração efetivamente consumida pelo cálculo. (`app/docs/BACKLOG.md:1738`)
- **Estado:** FILA no BACKLOG, PARADA na GRADE; exige aval para o motor. (`app/docs/BACKLOG.md:1738`; `app/docs/GRADE-MESTRA.md:234`)
- **Resposta:** **continua cravada em 22h–05h**. Além das constantes, a função usa diretamente `time(5, 0)` e `time(22, 0)`, sem parâmetro de janela. Alterar somente as constantes não altera esses literais. (`app/ponto/motor_calculo_v2.py:44`; `app/ponto/motor_calculo_v2.py:82`; `app/ponto/motor_calculo_v2.py:101`; `app/ponto/motor_calculo_v2.py:103`)
- **Prova:** o calculador reutiliza essa função. A P5 posterior trata intervalos/prorrogação e continua chamando `minutos_noturnos`; ela não acrescenta janela parametrizada a essa função. (`app/ponto/calculador/regras.py:85`; `app/ponto/motor_calculo_v2.py:191`; `app/ponto/motor_calculo_v2.py:196`)
- **Próximo passo:** manter a pendência de cadastro e consumo da janela, com o aval registrado para o sítio. A resposta estrutural dispensa dado de produção. (`app/docs/BACKLOG.md:1738`; `app/ponto/motor_calculo_v2.py:82`)

---

**troca S5b**

- **Pedido:** trocar sem criar desconto novo; comparar contra o gravado, inclusive por colaborador, explicando pareamento e dupla contagem. A leitura deveria verificar a fidelidade do DIFF à lavratura e distinguir escrita paralela de troca dos leitores. (`app/docs/BACKLOG.md:1231`; `app/docs/BACKLOG.md:1377`; `app/ponto/management/commands/diff_calculador.py:49`)
- **Estado herdado:** PAREI pelo critério individual de col516/col174; GRADE repete PARADA. (`app/docs/BACKLOG.md:1377`; `app/docs/GRADE-MESTRA.md:787`)
- **Resposta:** **o bloqueio individual antigo foi respondido pela porta única de alimentação**. A causa era diferença de pareamento, descrita no código atual; o fecho posterior do critério registra zero colaborador divergente naquela medição. O primeiro anúncio de fecho teve retratação por erro UTC e não deve ser usado sozinho. (`app/ponto/calculador/alimentacao.py:4`; `app/ponto/calculador/alimentacao.py:12`; `app/docs/RELATO-ARQUIVO.md:24452`; `app/docs/RELATO-ARQUIVO.md:24867`)
- **Prova no código atual:** a escrita `oraculo` existe ao lado de `motor`, desde 10/2026; os somadores e leitores consultados continuam usando `motor`. **A escrita paralela está implementada; a troca desses leitores não ocorreu.** O relato posterior separa o fecho da escrita do bloqueio da troca por O119. (`app/ponto/services/fechamento.py:1265`; `app/ponto/services/fechamento.py:1294`; `app/ponto/services/dia_pago.py:550`; `app/folha/porta_export.py:284`; `app/docs/RELATO-ARQUIVO.md:24247`; `app/docs/RELATO-ARQUIVO.md:24288`)
- **Próximo passo:** remedir com o instrumento real contra gravado e períodos do fechamento, expondo erros e cegueiras antes de concluir sobre a troca. A paridade atual está **[nao verificado]**. (`app/ponto/management/commands/diff_calculador.py:45`; `app/ponto/management/commands/diff_calculador.py:49`; `app/ponto/management/commands/diff_calculador.py:592`)

Sonda sem CSV, usando as opções existentes do instrumento; a saída identifica pessoas por `colNNN`. (`app/ponto/management/commands/diff_calculador.py:31`; `app/ponto/management/commands/diff_calculador.py:604`; `app/ponto/management/commands/diff_calculador.py:656`)

```python
import ast
import io
from django.core.management import call_command
from django.db import connection, transaction

saida = io.StringIO()
with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    call_command(
        "diff_calculador",
        mes=10, ano=2026,
        contra_gravado=True,
        periodos_do_fechamento=True,
        limite_colabs=0,
        csv=None,
        stdout=saida,
    )
    texto = saida.getvalue()
    print(texto)
    linha = next(x for x in texto.splitlines()
                 if x.startswith("dia-colab comparados:"))
    contadores = ast.literal_eval(linha[linha.index("{"):])
    assert contadores.get("dia_colab_comparado", 0) > 0, "universo vazio"
    erros = {k: v for k, v in contadores.items()
             if v and (k.startswith("erro")
                       or k == "sem_resultado_do_fechamento"
                       or "FALHOU" in k)}
    assert not erros, erros
print("FIM_OK")
```

---

**COL369**

- **Pedido:** explicar inversões em 23/09 versus 26–27/09; trabalho com realizado zero sob Ausência 4538; vínculo 1296 impossível versus 1313 sem folga. (`app/docs/BACKLOG.md:1246`)
- **Estado herdado:** “ABERTO — nenhum medido”, REGISTRADA na GRADE. Foi superado parcialmente por **COL369-TIPOS-INVERTIDOS**, **COL369-VINCULO-1296** e pauta da folga. (`app/docs/BACKLOG.md:1246`; `app/docs/GRADE-MESTRA.md:182`; `app/docs/BACKLOG.md:1380`; `app/docs/BACKLOG.md:1368`; `app/docs/BACKLOG.md:1379`)
- **Resposta:**
  - **Inversão:** a medição posterior explicou a diferença pela pausa real: 59 minutos em 23/09, 73/64 em 26–27/09, diante de cadastro de 60. O contrafactual explicou 2,00 h de intra e 2,29 h trabalhadas; a obra era medir, sem aplicar cura. Hoje o motor e o pareador consultam a autoridade da ata quando legível. (`app/docs/RELATO-ARQUIVO.md:23361`; `app/docs/RELATO-ARQUIVO.md:23370`; `app/docs/RELATO-ARQUIVO.md:23379`; `app/ponto/motor_calculo_v2.py:534`; `app/ponto/turnos.py:823`)
  - **Realizado zero:** **BO-B DIAPAGO-SE-CONTRADIZ / B-B21** corrigiu a classe: trabalho em ausência entra no realizado e abono cobre o restante do previsto. A prova histórica preservou a 09; o estoque específico de col369 em 29–30/09 permanece **[nao verificado]**. (`app/ponto/services/dia_pago.py:375`; `app/ponto/services/dia_pago.py:393`; `app/ponto/services/dia_pago.py:400`; `app/docs/RELATO.md:5475`; `app/docs/CORTES.md:344`)
  - **Vínculo:** ficou decidido **NÃO TROCAR**; pauta 908 pediu à supervisão o dia de folga. O DIFF completo corrigiu a previsão inicial: troca isolada moveria **−7,07 h e removeria zero furos**, pois as células exigiriam regeneração separada. (`app/docs/BACKLOG.md:1368`; `app/docs/BACKLOG.md:1379`; `app/docs/RELATO-ARQUIVO.md:26535`; `app/docs/RELATO-ARQUIVO.md:26546`)
- **Prova:** o leitor atual devolve `cel.trabalha`; a correção do mecanismo de ausência calcula `coberto = min(realizado, previsto)` e `abono = previsto − coberto`. (`app/escala/models.py:1095`; `app/ponto/services/dia_pago.py:394`; `app/ponto/services/dia_pago.py:396`)
- **Próximo passo:** conferir estoque e cadastro atuais por sonda, preservando a decisão registrada de não trocar o vínculo. (`app/docs/BACKLOG.md:1368`)

Sonda dos campos gravados e da mesma derivação de grade usada pela folha. (`app/folha/export.py:329`; `app/ponto/services/dia_pago.py:364`; `app/ponto/models.py:349`)

```python
from datetime import date
from django.db import connection, transaction
from ponto.models import DiaPago, FechamentoMensal
from escala.models import EscalaColaborador
from folha.export import grade_do_fechamento
from ponto.services.dia_pago import por_dia_da_grade

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    for e in EscalaColaborador.objects.filter(
        colaborador_id=369
    ).order_by("data_inicio", "pk"):
        print("VINCULO", "col369", e.pk, e.ativa,
              e.data_inicio, e.data_fim, e.tipo_escala_id)
    for r in DiaPago.objects.filter(
        colaborador_id=369,
        data__range=(date(2026, 9, 19), date(2026, 10, 1)),
    ).order_by("data", "versao").values(
        "data", "mes", "ano", "versao", "horas_trabalhadas",
        "horas_intra_indenizada", "minutos_realizados",
        "minutos_abonados", "minutos_previstos",
    ):
        print("DIAPAGO", "col369", r)
    for f in FechamentoMensal.objects.filter(
        colaborador_id=369, mes=9, ano=2026
    ).select_related("colaborador__empresa"):
        abono, previsto, realizado = por_dia_da_grade(grade_do_fechamento(f))
        for d in (date(2026, 9, 29), date(2026, 9, 30)):
            print("GRADE", "col369", d, previsto.get(d),
                  realizado.get(d), abono.get(d))
print("FIM_OK")
```

---

**O236**

- **Pedido:** decompor por motivo os retidos históricos 200/238/94, pela função real, e medir quantos a L-033 libera. (`app/docs/BACKLOG.md:1293`)
- **Estado herdado:** achado medido, não curado; abertura por motivo não feita; REGISTRADA na GRADE. (`app/docs/BACKLOG.md:1293`; `app/docs/GRADE-MESTRA.md:185`)
- **Resposta:** **a premissa “L-033 só no papel” foi superada por O254/O259**. A porta atual isenta o dia com exclusivamente `FURO_PARCIAL` e confirmação de que somente marcos de pausa estão apagados. Dias mistos continuam retidos; desligados e isentos também têm saída explícita. A decomposição atual da população depende de produção. (`app/docs/RELATO-ARQUIVO.md:43115`; `app/docs/RELATO-ARQUIVO.md:42950`; `app/folha/export.py:1173`; `app/folha/export.py:1207`; `app/folha/export.py:1293`)
- **Prova:** `com_dias=True` fornece os próprios dias e motivos do juiz; o vigia chama a mesma isenção. CORTES também corrige números herdados sem lastro no arquivo alegado, que não devem ser reciclados como prova atual. (`app/folha/export.py:1186`; `app/folha/export.py:1212`; `app/folha/services/vigia_pausa.py:19`; `app/docs/CORTES.md:327`; `app/docs/CORTES.md:328`)
- **Próximo passo:** medir motivos e universo da isenção, mantendo o critério existente. A quantidade atual liberada/retida está **[nao verificado]**. (`app/docs/BACKLOG.md:1293`)

Sonda de todas as empresa-competências existentes em `FechamentoMensal`, com classificação real do export e censo real da pausa. (`app/folha/export.py:765`; `app/folha/services/vigia_pausa.py:100`)

```python
from collections import Counter
from django.db import connection, transaction
from colaboradores.models import Empresa
from ponto.models import FechamentoMensal
from folha.export import classificar_export
from folha.services.vigia_pausa import censo

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION READ ONLY")
    competencias = list(
        FechamentoMensal.objects.order_by().values_list(
            "colaborador__empresa_id", "mes", "ano"
        ).distinct()
    )
    print("UNIVERSO_EMPRESA_COMPETENCIA", len(competencias))
    for eid, mes, ano in sorted(competencias):
        empresa = Empresa.objects.get(pk=eid)
        itens = classificar_export(empresa, mes, ano)
        resumo = Counter()
        for item in itens:
            motivos = tuple(item.get("motivos_espelho") or ())
            chave = (item["status"], item["motivo"], motivos)
            resumo[chave] += 1
            print("O236", f"col{item['colaborador_id']}",
                  eid, mes, ano, chave)
        print("RESUMO", eid, mes, ano, dict(resumo))
        pausa = censo(empresas=[empresa], competencias=[(mes, ano)])
        for universo in pausa["universos"]:
            universo.pop("cnpj8", None)
        print("L033", eid, mes, ano, pausa)
print("FIM_OK")
```

---

**AUSENCIA-UMA-VIGENTE**

- **Pedido:** resolver abono+falta que desconta, pela lei da família, sem apagar registro; aplicação em exportada depende da autorização registrada. A leitura deveria localizar os produtores da sobreposição e a divergência entre cobertura e desconto. (`app/docs/BACKLOG.md:1941`; `app/docs/RELATO.md:2258`)
- **Estado:** REGISTRADA. O relato histórico já respondeu a leitura, medindo quatro dias contraditórios e identificando os escritores. (`app/docs/GRADE-MESTRA.md:259`; `app/docs/RELATO.md:2258`)
- **Resposta no código atual:** **a contradição continua possível**: desconto soma todas as aprovadas que descontam; cobertura escolhe uma, pela maior `(data, pk)`. A constraint limita colaborador/tipo/data, permitindo tipos diferentes no mesmo dia. (`app/folha/export.py:87`; `app/folha/export.py:140`; `app/ponto/turnos.py:2765`; `app/ponto/turnos.py:2793`; `app/ponto/models.py:836`)
- **Prova dos produtores:** `retornar_mesmo_tipo` permite tipo diferente ao lado e continua usado pela materialização; a ap

[truncado em 20000 de 24796 caracteres]

## 17/11 08:10 · sessao 7fe12853c20b · CODEX-RESERVA-R4 · leitura · origem=Code

Os **dois “Nosso, sem obra” são os mesmos dois do GRADE-RESOLVE**, não quatro casos distintos. Ambos ganharam complemento de investigação; nenhuma cura específica está demonstrada para esses dias. (`app/docs/RELATO.md:666`; `app/docs/BACKLOG.md:865`; `app/docs/BACKLOG.md:1589`)

**Limite do histórico:** esta cópia não contém metadados Git. Portanto, `git log`, ancestralidade e diferenças desde 05/11 ficam **[nao verificado]**; os hashes abaixo têm lastro documental. A própria GRADE registra essa ausência. (`app/docs/GRADE-MESTRA.md:1711`)

**(a) Causa, cobertura e menor escopo**

A coluna “menor escopo” contém propostas de investigação, não curas já demonstradas: **[nao verificado]**.

| Caso | Causa demonstrável no código e limite | Obra registrada que cobre | Menor escopo |
|---|---|---|---|
| **HJ-005-U1 — col42, 01/10** | O ramo de fato real em ausência aprovada pode produzir `FATO_EM_AUSENCIA`, convertido em irregularidade/retenção. A ausência efetivamente usada, seu período e a posição da batida nesse período estão **[nao verificado]**. (`app/ponto/supra_juiz.py:392`; `app/ponto/supra_juiz.py:413`; `app/ponto/status_dia.py:68`; `app/folha/export.py:1293`) | **BO-ADMIN-2710**, complemento de censo. **Sem obra de cura demonstrada**: COBERTURA-PARCIAL nomeia 08/15/22-10; o registro ressalva que sua cobertura de 01/10 não foi provada. (`app/docs/BACKLOG.md:865`; `app/docs/BACKLOG.md:874`; `app/docs/GRADE-MESTRA.md:137`) | Reconferir retenção, lavra, ausência/período e batidas de 01/10 na mesma leitura; resolver dono/destino. Só depois delimitar correção do ramo que divergir. (`app/docs/BACKLOG.md:865`) |
| **HJ-012-U2 — col239, 12/10** | `ESTRUTURA` é também retorno **residual** do juiz: não ter predicado CADASTRO/BATIDA não prova cobrança indevida. A ingestão e a medição anterior discordam: “espelho_cobrado/ESTRUTURA” versus “legítimo → DP”. Defeito específico **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:110`; `app/docs/BACKLOG.md:1589`) | **BUG-RETRATO-VELHO-NA-TELA**, complemento de reconciliação. **Sem cura específica demonstrada** para esse dia. (`app/docs/BACKLOG.md:1589`; `app/docs/GRADE-MESTRA.md:845`) | Reconciliar retenção atual, lavra, dono e regra do feriado. Aviso de frescor do retrato não basta para provar correção da cobrança. (`app/docs/BACKLOG.md:1589`; `app/folha/retrato_folha.py:1413`) |
| **G3-10 — col163, 10/10** | Classificação residual ESTRUTURA; na medição, não segurava e tinha status `ausencia`. Causa de defeito **[nao verificado]**. (`app/docs/RELATO.md:9274`; `app/docs/RELATO.md:9281`; `app/ponto/management/commands/e6_oraculo.py:110`) | **G3G4-ESTRUTURA-RESIDUO** registra o levantamento; a própria linha diz **sem obra**, padrão não nomeado. (`app/docs/GRADE-MESTRA.md:274`; `app/docs/BACKLOG.md:2208`) | Primeiro demonstrar divergência positiva entre autoridades; localizar o primeiro resultado contraditório e nomear uma classe antes de construir. (`app/docs/RELATO.md:9281`) |
| **G3-11 — col163, 17/10** | Residual ESTRUTURA; não segurava, status `folga_trabalhada`. Defeito **[nao verificado]**. (`app/docs/RELATO.md:9275`; `app/docs/RELATO.md:9281`) | Mesmo registro de levantamento; **sem obra de cura demonstrada**. (`app/docs/BACKLOG.md:2208`) | Reconferir trabalho/folga, retenção e pagamento; só abrir cura se houver contradição demonstrada. (`app/docs/RELATO.md:9281`) |
| **G3-12 — col873, 26/09** | Residual ESTRUTURA; não segurava, status `ausencia`. Defeito **[nao verificado]**. (`app/docs/RELATO.md:9276`; `app/docs/RELATO.md:9281`) | Mesmo registro; **sem obra de cura demonstrada**. (`app/docs/BACKLOG.md:2208`) | Reconferir autoridades e natureza da ausência; eventual tratamento de competência entregue depende do alcance efetivamente demonstrado. (`app/docs/RELATO.md:9281`) |

**Totais de (a):** 2 casos do quadro GRADE-RESOLVE, com 2 complementos de investigação; mais 3 resíduos históricos registrados na GRADE. São **5 casos examinados e nenhuma cura específica demonstrada** nesses registros. Os três resíduos não aumentam os “2 sem obra” do quadro de 18 ESTRUTURA. (`app/docs/RELATO.md:666`; `app/docs/BACKLOG.md:865`; `app/docs/BACKLOG.md:1589`; `app/docs/BACKLOG.md:2208`)

**(b) O que mudou desde 05/11**

Estas mudanças justificam reconferência; sua aplicação a cada pessoa/dia continua **[nao verificado]** sem nova sonda.

| Referência | Mudanças documentadas posteriores à ingestão | O que pode mudar |
|---|---|---|
| **A — ausências** | H12H06-A, 05/11 22:1x, `ca278fc4`; H12H06-B, 06/11, raia `97754f8d`/comboio `62017e70`. (`app/docs/RELATO.md:9210`; `app/docs/RELATO.md:9212`; `app/docs/RELATO.md:2324`) | Faltas pela jornada da grade e leitores de cobertura parcial; houve aplicação em 28 linhas da competência 11, sem vínculo individual demonstrado com estes casos. (`app/docs/RELATO.md:9211`; `app/docs/RELATO.md:2395`; `app/docs/RELATO.md:3107`) |
| **M — motor e turnos** | V2 item 2, 07/11, `a23831c3`; BUG-ENC-CELULAS-FORMA, 15/11 23:0x, `7825fec5b`. (`app/docs/BACKLOG.md:1954`; `app/docs/RELATO.md:256`) | Feriado/HE100 noturna; correção da forma das células fornecidas ao juiz de turnos. (`app/docs/BACKLOG.md:1954`; `app/docs/BACKLOG.md:1021`) |
| **F — fatos e fio** | E1 `a8a692f55` (08/11); E1-FILA `7aa48401` e E2/primeira E3 `7c16da320` (09/11); E4a-1 `892159c5f`, E4a-2 `613a51498`, E4b `087fe758a`. (`app/docs/BACKLOG.md:2008`; `app/docs/BACKLOG.md:2009`; `app/docs/BACKLOG.md:2021`; `app/docs/BACKLOG.md:2035`; `app/docs/BACKLOG.md:2043`; `app/docs/BACKLOG.md:2044`) | Nascimento de fatos/perguntas, tarefa do dia, atos e recuperação do fio. Pouso de código não comprova aplicação do acervo por caso. (`app/docs/BACKLOG.md:2022`; `app/docs/BACKLOG.md:2044`) |
| **P — leitores e portas** | CXR `94d41ea5` (08/11); E3 X1 `e70a54f7e` (11/11); E3 v10 merge `90f3fa66b`, deploy 14/11 22:42; R3 c17 `f8ee770a5` (16/11). (`app/docs/BACKLOG.md:886`; `app/docs/RELATO.md:1446`; `app/docs/RELATO.md:614`; `app/docs/RELATO.md:597`; `app/docs/BACKLOG.md:770`) | Visibilidade do cadastro apontado pelo motor, retenção de células e ação alcançável no fio. (`app/docs/BACKLOG.md:886`; `app/docs/RELATO.md:1439`; `app/docs/RELATO.md:610`; `app/docs/BACKLOG.md:770`) |
| **D — desligados** | LEI-DESLIGADO-NAO-TRAVA `9e2c54017` (13/11); LOTE3 c7 `be4a6219e` (16/11). (`app/docs/BACKLOG.md:731`; `app/docs/RELATO.md:184`) | Desligado deixa de reter a folha; tratamento da situação sem porta. Ser desligado em qualquer caso listado: **[nao verificado]**. (`app/folha/export.py:1197`; `app/docs/RELATO.md:184`) |
| **J — resposta e férias** | O168 `d33c0a899` (13/11); HJ-091 `8376da7aa` (14/11); F1 B1 `3adf703e3` (16/11). (`app/docs/BACKLOG.md:2093`; `app/docs/BACKLOG.md:750`; `app/docs/RELATO.md:174`) | Reconciliação de conversa/resposta, origem das ausências de férias e juiz de resposta. (`app/docs/BACKLOG.md:2093`; `app/docs/BACKLOG.md:750`; `app/docs/RELATO.md:174`) |
| **B — batida e intervalo** | Dedup `b6d268dfd` e CONTRATO-DE-ENTRADA-DO-FIO `77dfa94fa` (15/11); BUG-PAR `41f5648af` (16/11 21:41). (`app/docs/RELATO.md:475`; `app/docs/RELATO.md:32`) | Entrada de novos fatos, horário/tipo entregue ao fio e respostas de intervalo. Não comprova correção retroativa dos dias antigos. (`app/docs/RELATO.md:475`; `app/docs/BACKLOG.md:2084`) |
| **L — porta de ausência** | LOTE7 c7 `42fd467a0` (16/11). (`app/docs/RELATO.md:164`) | Porta de ausência/regularização; o controle documentado do kernel não demonstra mudança nos casos deste quadro. (`app/docs/RELATO.md:175`) |

**Não contam como cura posterior à ingestão:** F3-BECO-1 já estava NO AR às 17:5x de 05/11; C20-JUIZ-3 está registrado às 20:2x, enquanto a ingestão aparece às 20:4x. A nota “remedir depois do F3” não comprova que aquele pouso ocorreu depois da sonda. (`app/docs/BACKLOG.md:2210`; `app/docs/RELATO.md:9154`; `app/docs/RELATO.md:9165`; `app/docs/RELATO.md:9177`)

**Tabela nominal CADASTRO/BATIDA**

O agregado é **11 CADASTRO + 9 BATIDA**, com **4 CADASTRO e 1 BATIDA sem individualizar**. A cópia não fornece o manifesto completo que liga cada linha ao quadro v2. Portanto, a associação dos registros do levantamento dos 67 ao balde v2 é **[nao verificado]**; eles estão separados abaixo pela origem documental. (`app/docs/RELATO.md:240`; `app/docs/RELATO.md:585`)

| Caso documentado | Identificação disponível | Mudança que justifica conferir; limite |
|---|---|---|
| HJ-001-U1 — CADASTRO | col836; dia por unidade **[nao verificado]**. (`app/docs/RELATO.md:9172`) | Escala suspeita/espelho indefinido; verificar **P**, além do F3 já citado na própria pendência. Resultado atual **[nao verificado]**. (`app/docs/RELATO.md:9177`) |
| HJ-001-U2 — CADASTRO | col836; dia por unidade **[nao verificado]**. (`app/docs/RELATO.md:9172`) | Mesmo encaminhamento documental; igualdade da causa entre U1/U2 **[nao verificado]**. (`app/docs/RELATO.md:9177`) |
| HJ-016-U1 — CADASTRO | col297; dia **[nao verificado]**. (`app/docs/RELATO.md:9172`) | **M/P** podem alterar insumos ou leitura; mudança específica **[nao verificado]**. |
| BO-0611/07 — CADASTRO | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Sem pessoa/dia/causa, atribuir uma mudança específica seria **[nao verificado]**. |
| BO-1311-ROTEIRO/04 — CADASTRO | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-1311-ROTEIRO/10 — CADASTRO | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-ADMIN-2710/8.2 — CADASTRO | col789; datas exatas do caso **[nao verificado]**. (`app/docs/BACKLOG.md:866`; `app/docs/RELATO.md:585`) | Reexaminar vínculo/folgas e **P**; cura do resíduo não está demonstrada na obra da trilha. (`app/docs/BACKLOG.md:866`) |
| **4 CADASTRO sem ID** | Individualização ausente. (`app/docs/RELATO.md:240`) | Pessoa, dia, causa e impacto de commit **[nao verificado]**. |
| HJ-003-U1 — BATIDA | col407; data da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **M/F/P** são candidatos; há par col407 21/10 em outro quadro, mas identidade com esta unidade **[nao verificado]**. (`app/docs/RELATO.md:2348`) |
| HJ-005-U5 — BATIDA | col42; data exata da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **A/M/P**. Um relato diz BATIDA→sem divergência, porém conserva a remedição sem prova; não fechar por ele. (`app/docs/RELATO.md:752`) |
| HJ-015-U1 — BATIDA | col39; data da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **M/F/P**. Equivalência com G4-01, col39 15/10, **[nao verificado]**. (`app/docs/RELATO.md:9278`) |
| HJ-027-U1 — BATIDA | col292; data da unidade **[nao verificado]**. (`app/docs/RELATO.md:9173`) | **M/F/P**. Outro quadro contém col292 21/09; equivalência com a unidade **[nao verificado]**. (`app/docs/RELATO.md:2348`) |
| BO-0611/01 — BATIDA | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-0611/04 — BATIDA | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| BO-0611/13 — BATIDA | Pessoa/dia **[nao verificado]**. (`app/docs/RELATO.md:585`) | Mudança específica **[nao verificado]**. |
| HJ-093 — BATIDA no levantamento dos 67 | col1004, 27/10. Outro registro confirma classe JUSTIFICATIVA-PAGA e pergunta viva por decisão humana. (`app/docs/RELATO.md:585`; `app/docs/BACKLOG.md:798`) | Reconferir **A/J/P**; a divergência de enquadramento entre registros impede repetir “BATIDA” como diagnóstico atual. (`app/docs/BACKLOG.md:798`) |
| **1 BATIDA sem ID** | Individualização ausente. (`app/docs/RELATO.md:240`) | Pessoa, dia, causa e impacto de commit **[nao verificado]**. |

Há ainda pares CADASTRO/BATIDA no levantamento G3/G4, mas sua pertença aos baldes v2 não está comprovada; não os somei novamente. Isso inclui dias que já **não seguravam** apesar do dono CADASTRO/BATIDA. (`app/docs/RELATO.md:9268`; `app/docs/RELATO.md:9269`; `app/docs/RELATO.md:9270`; `app/docs/RELATO.md:9271`; `app/docs/RELATO.md:9272`; `app/docs/RELATO.md:9277`; `app/docs/RELATO.md:9278`)

**Os nove casos vinculados a obras antigas NO AR**

São **9 casos em 5 obras**, cujos pousos citados são de outubro. Esses pousos não constituem mudanças posteriores à ingestão de 05/11. (`app/docs/BACKLOG.md:874`; `app/docs/BACKLOG.md:864`; `app/docs/BACKLOG.md:882`; `app/docs/BACKLOG.md:868`; `app/docs/BACKLOG.md:866`)

| Caso | Obra antiga | Última situação fornecida; reconferência necessária |
|---|---|---|
| HJ-005-U2 | COBERTURA-PARCIAL, `fb813587`, 28/10. (`app/docs/BACKLOG.md:874`) | ABERTO no anexo C1; verificar **A/M/P**. Resultado posterior **[nao verificado]**. (`app/docs/RELATO.md:240`) |
| HJ-005-U4 | Mesma obra. (`app/docs/BACKLOG.md:874`) | ABERTO no anexo C1; mesmos candidatos, sem fechamento posterior demonstrado. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/1 | Mesma obra; col42 08/15/22-10. (`app/docs/BACKLOG.md:874`) | ABERTO, parcial sem período no anexo C1; conferir os três dias separadamente. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/2 | JUIZ-DO-PAR, `b3bf529e`, 29/10. (`app/docs/BACKLOG.md:864`) | Faltam os sete dias exatos no anexo; impacto individual **[nao verificado]**. |
| BO-ADMIN-2710/5 | Mesma obra; col247 24/10. (`app/docs/BACKLOG.md:864`) | ABERTO no C1; **M/P** e BUG-PAR posterior justificam nova leitura, sem presumir pertença ao universo curado. (`app/docs/RELATO.md:240`; `app/docs/BACKLOG.md:2084`) |
| HJ-030-U1 | FIO-LE-O-JUIZ, `7c9e4c27`, 27/10. (`app/docs/BACKLOG.md:882`) | FECHADO **somente o sintoma de invisibilidade**, conforme anexo; não equivale a fechar todos os problemas do dia. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/6 | Mesma obra. (`app/docs/BACKLOG.md:882`) | Falta tela/perfil do supervisor no anexo; **P** pode mudar a porta, alcance por perfil **[nao verificado]**. |
| BO-ADMIN-2710/7 | REGISTRADO-NAO-E-VIVO, 27/10; col130 05/10. (`app/docs/BACKLOG.md:868`) | FECHADA somente a pendência de fila, conforme anexo; demais sintomas **[nao verificado]**. (`app/docs/RELATO.md:240`) |
| BO-ADMIN-2710/8.1 | TRILHA-DO-VINCULO-MENTE, `5e9a0a51`, 28/10. (`app/docs/BACKLOG.md:866`) | Continua sem juiz leitor da fidelidade histórica no anexo. Sonda de retenção/dono não certifica a trilha. (`app/docs/RELATO.md:240`; `app/docs/BACKLOG.md:866`) |

**Sonda proposta — somente leitura, sem execução nesta reserva**

O corpo abaixo chama as autoridades reais: retenção por `dias_que_seguram`, status lavrado e `InsumosDoDono.dono`. Mantém ausências de pessoa/dia como lacunas; não transforma dono em prova de divergência. (`app/folha/fora_da_folha.py:125`; `app/ponto/status_dia.py:201`; `app/ponto/management/commands/e6_oraculo.py:182`; `app/docs/RELATO.md:9168`)

É um **payload para runtime Django já inicializado**, com tenant selecionado e role `leitor`; não chama `django.setup()` nem o lançador que grava arquivos. A configuração SaaS cria diretório/configura log em arquivo, e `sonda_leitura.sh` regrava credenciais. Execução e resultado deste payload: **[nao verificado]**. (`app/config/settings/saas.py:118`; `app/config/settings/saas.py:138`; `bin/sonda_leitura.sh:121`)

```python
# Executar em runtime Django JA inicializado, Python sem bytecode,
# tenant ja selecionado e credencial leitor. Nao salvar arquivo.
# Fontes das autoridades:
# app/folha/fora_da_folha.py:125
# app/ponto/status_dia.py:201
# app/ponto/management/commands/e6_oraculo.py:128 e :182
# app/ponto/janelas.py:60
import sys
sys.dont_write_bytecode = True

import contextlib
import datetime as dt
import io
import json
import logging
import re

from django.apps import apps
from django.db import connection, transaction
from django.utils import timezone

assert apps.ready, "Runtime Django nao inicializado"
assert not connection.in_atomic_block, "Exige transacao propria"
assert getattr(connection, "schema_name", None) not in (None, "public"), \
    "Tenant nao selecionado"

from colaboradores.models import Colaborador
from folha.fora_da_folha import dias_que_seguram
from ponto.janelas import janela_atual
from ponto.status_dia import status_do_dia_lavrado
from ponto.management.commands.e6_oraculo import InsumosDoDono

# (caso, colNNN, datas comprovadas).
# () significa DATA AUSENTE; nunca preencher por ordem presumida.
# HJ: app/docs/RELATO.md:9172 e :9173.
# BO/roteiro: app/docs/RELATO.md:585.
# HJ-093: app/docs/BACKLOG.md:798.
CASOS = [
    ("HJ-001-U1", 836, ()),
    ("HJ-001-U2", 836, ()),
    ("HJ-016-U1", 297, ()),
    ("BO-0611/07", None, ()),
    ("BO-1311-ROTEIRO/04", None, ()),
    ("BO-1311-ROTEIRO/10", None, ()),
    ("BO-ADMIN-2710/8.2", 789, ()),
    ("HJ-003-U1", 407, ()),
    ("HJ-005-U5", 42, ()),
    ("HJ-015-U1", 39, ()),
    ("HJ-027-U1", 292, ()),
    ("BO-0611/01", None, ()),
    ("BO-0611/04", None, ()),
    ("BO-0611/13", None, ()),
    ("HJ-093", 1004, ("2026-10-27",)),

    # Casos antigos NO AR; datas apenas quando comprovadas.
    # app/docs/BACKLOG.md:874, :864, :838, :868, :866.
    ("HJ-005-U2", 42, ()),
    ("HJ-005-U4", 42, ()),
    ("BO-ADMIN-2710/1", 42,
     ("2026-10-08", "2026-10-15", "2026-10-22")),
    ("BO-ADMIN-2710/2", None, ()),
    ("BO-ADMIN-2710/5", 247, ("2026-10-24",)),
    ("HJ-030-U1", 94, ("2026-10-16",)),
    ("BO-ADMIN-2710/6", None, ()),
    ("BO-ADMIN-2710/7", 130, ("2026-10-05",)),
]
# /8.1 exige leitor da fidelidade da trilha: nao certificado aqui.
# Manifesto completo v2 e ligacao dos aliases: [nao verificado].
# Lacunas expressamente registradas em app/docs/RELATO.md:240.
SEM_ID = {"CADASTRO": 4, "BATIDA": 1}

def emitir(obj):
    print(json.dumps(obj, ensure_ascii=True, sort_keys=True))

def apenas_select(execute, sql, params, many, context):
    # Bloqueio conservador: nao permite SET, DML, DDL, WITH ou EXPLAIN.
    # A transacao READ ONLY permanece como segunda barreira.
    if not re.match(r"^\s*SELECT\b", sql, flags=re.I):
        raise RuntimeError("SQL_NAO_SELECT_BLOQUEADO")
    return execute(sql, params, many, context)

resultados = []
erros = 0
cache = {}
agora = timezone.now()
hoje = timezone.localtime(agora).date()
nivel_log = logging.root.manager.disable
logging.disable(logging.CRITICAL)

try:
    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("""
                SELECT current_user, rolsuper, rolbypassrls
                FROM pg_roles WHERE rolname = current_user
            """)
            role, superuser, bypass = cur.fetchone()
            assert role == "leitor" and not superuser and not bypass

            cur.execute("""
                SELECT EXISTS (
                    SELECT 1
                    FROM information_schema.tables
                    WHERE table_type = 'BASE TABLE'
                      AND table_schema NOT IN
                          ('pg_catalog', 'information_schema')
                      AND has_table_privilege(
                          current_user,
                          format('%I.%I', table_schema, table_name),
                          'INSERT,UPDATE,DELETE,TRUNCATE,TRIGGER'
                      )
                )
            """)
            assert cur.fetchone()[0] is False, "Role possui escrita"

        with connection.execute_wrapper(apenas_select):
            for caso, cid, datas in CASOS:
                if cid is None or not datas:
                    resultados.append({
                        "caso": caso,
                        "col": None if cid is None else f"col{cid}",
                        "resultado": "NAO_VERIFICADO",
                        "motivo": "pessoa_ou_data_ausente",
                    })
                    continue

                for texto in datas:
                    base = {"caso": caso, "col": f"col{cid}", "dia": texto}
                    try:
                        dia = dt.date.fromisoformat(texto)
      

[truncado em 20000 de 24415 caracteres]

## 17/11 08:09 · sessao 1b51d1bc7272 · CODEX-RESERVA-R5 · leitura · origem=Code

**São 43 obras PARADA fora do balde ESPINHA-E: 19 LATERAL-ESTRUTURA, 14 INSTRUMENTO, 9 UI e 1 de 20/20.** A contagem considera IDs distintos nas tabelas principais `| id | balde |`, com estado iniciado por `PARADA`; exclui complementos, obsoletas e a lista de raias do rodapé. Esse é o critério do parser da GRADE. Portanto, **22 não corresponde à contagem desta cópia**. Se “espinha” também incluir o balde 20/20, o recorte fica em **42**. (bin/placar_grade.py:69; bin/placar_grade.py:80; bin/placar_grade.py:118; app/docs/GRADE-MESTRA.md:527; app/docs/GRADE-MESTRA.md:1757)

Abaixo, referências sem diretório pertencem a **`app/docs/`**. **DESTRAVADA-JA** significa que a trava nominal foi resolvida ou que a obra já pousou/fechou; pode restar implementação, revisão ou baixa documental. A cópia é preparada por `git archive`; **`git log` e ancestralidade dos hashes: [não verificado]**. Os hashes citados são os registrados nos arquivos, sem consulta à árvore viva. (CLAUDE.md:344) [nao verificado]

| obra | linha GRADE | trava | destrava | conferido HEAD | classe |
|---|---|---|---|---|---|
| HE-DE-14H-NUM-DIA — col945 12/09 | GRADE-MESTRA.md:177 | `! REGEN-EM-EXPORTADA` para o gravado da 09 | `!` específico para o ato na exportada | **Existe para o gravado.** Causa já lida; HEAD documentado já calcula HE zero. A 09 continua condicionada ao `!`; valor original de produção não foi reproduzido integralmente. (RELATO.md:120) | **ESPERA-RONALD** |
| BUG-ANCORA-NAO-REGENERA | GRADE-MESTRA.md:178 | Decisão sobre col824 e col899 | Encaminhar o resíduo ao cadastro/admin | **Resolvida:** FECHADA pelo CORTE B em 16/11; pendência marcada respondida, nenhum vínculo alterado. Signal da cura permanece presente. (BACKLOG.md:1208; RELATO.md:133; PENDENTES_RONALD.json:295; app/escala/signals.py:113) | **DESTRAVADA-JA — baixa** |
| O-GEO-DECISAO | GRADE-MESTRA.md:183 | Fecho do ESPELHO-VERDADE; O198 antes do complemento | Fechar ESPELHO-VERDADE; depois O198 para GEO-SILENCIO | **Existe.** Portão reafirmado; V0 em voo, V1–V4 registradas. O198 conserva ordem de parar após o censo. (RELATO.md:123; GRADE-MESTRA.md:889; GRADE-MESTRA.md:893; BACKLOG.md:1544) | **ESPERA-OBRA — ESPELHO-VERDADE; O198 no complemento** |
| O99 | GRADE-MESTRA.md:190 | Decisão sobre 61 dias; TXT | CORTE B: resíduo ao cadastro/admin | **Resolvida como obra:** FECHADA em 16/11. A linha ainda conserva esperas históricas de cura1/TXT, mas o fechamento posterior é explícito. (BACKLOG.md:1342; RELATO.md:133) | **DESTRAVADA-JA — baixa** |
| LOTE-1-PENDENTES | GRADE-MESTRA.md:193 | Autorização da troca; decisões dos itens 2/5; cadastro do item 3 | Respostas/`!` dos itens 2, 4 e 5; dado cadastral do item 3 | **Preservada:** só item 1 feito; demais atos continuam condicionados às respostas. Item 3 também depende do fim da ausência #3186. (BACKLOG.md:1369) | **ESPERA-RONALD**, com dado adicional |
| O28 | GRADE-MESTRA.md:195 | DIFF na aberta; paga ao DP; `!` depois do número | Medir DIFF e obter `!` nominal | **Existe.** Corte C restringiu o escopo, mas o registro diz “nada medido”; não equivale ao `!` do ato. (BACKLOG.md:1399; RELATO.md:125) | **ESPERA-RONALD**, precedida da medida |
| O58 | GRADE-MESTRA.md:198 | Parecer jurídico para hash; Card 4 | Parecer para hash; registro de emissão pode avançar separadamente | **Parcial:** fonte mantém exigência do parecer e libera o registro independentemente dele. Parecer posterior recebido: **[não verificado]**. (BACKLOG.md:1433) | **ESPERA-DADO — parecer jurídico** |
| O147 | GRADE-MESTRA.md:201 | `!`; REGEN-EM-EXPORTADA | Medir efeito financeiro dos três atrasados e apresentar para decisão | **Preservada na fonte:** “não começada”, primeiro o número, depois a decisão sobre a 09. Medida/aval posterior: **[não verificado]**. (BACKLOG.md:1492) | **ESPERA-RONALD**, precedida da medida |
| O175 | GRADE-MESTRA.md:204 | Corte sobre exportada/paga; censo para construção | Regra já definida: recusar exportada/paga; exceção com `!` nominal | **Corte resolvido; defeito presente.** Comando e núcleo ainda recomputam sem essa guarda. Pode retomar censo/brief; não é autorização para apply. (BACKLOG.md:1521; RELATO.md:121; app/ponto/management/commands/materializar_turnos.py:16; app/ponto/nucleo.py:40) | **DESTRAVADA-JA — preparação/construção condicionada ao censo** |
| CARIMBO-POR-ORIGEM | GRADE-MESTRA.md:225 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho e censo dos modelos/escritores | **Existe o portão de obra.** Desenho já registrado: corrigir save/QuerySet na origem. Exemplo do problema permanece: `reabrir` salva sem atualizar o carimbo. (BACKLOG.md:1695; app/ponto/services/fechamento.py:1354; GRADE-MESTRA.md:889) | **ESPERA-OBRA — ESPELHO-VERDADE** |
| EMPATES-SEM-VIGENCIA | GRADE-MESTRA.md:226 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho, censo e competência de referência definida | **Existe.** Desenho registrado, porém contador ainda filtra apenas `ativo`; juiz filtra vigência. A competência de referência continua indicada como pergunta dele. (BACKLOG.md:1698; app/core/services/aplicacao_convencao.py:60; app/core/regua_cct.py:259) | **ESPERA-OBRA — ESPELHO-VERDADE**, com decisão residual |
| GRAVADO-NAO-CARIMBA-A-REGUA | GRADE-MESTRA.md:229 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho e censo; carimbo no ato da lavra | **Portão permanece.** Decisão de desenho já registrada; a fonte conserva a medição de 0/17 competências nomeáveis pela autoridade, sem cura. Medida atual desses 17: **[não verificado]**. (BACKLOG.md:1721; GRADE-MESTRA.md:889) | **ESPERA-OBRA — ESPELHO-VERDADE** |
| ABRANGENCIA-DOS-QUATRO-INSTRUMENTOS-SEM-PDF | GRADE-MESTRA.md:232 | Chegada dos PDFs | Receber os quatro instrumentos e conferir a base territorial | **Preservada na fonte:** declara expressamente que não fecha por nova medição, mas pela chegada dos PDFs. Recebimento posterior: **[não verificado]**. (BACKLOG.md:1736) | **ESPERA-DADO — PDFs** |
| JANELA-DO-NOTURNO-CRAVADA | GRADE-MESTRA.md:234 | `!` para zona inviolável | Aval nominal para alterar o motor e tornar a janela cadastrável | **Existe.** Janela continua constante 22h–05h no motor; fonte exige aval para esse sítio. Aval específico posterior: **[não verificado]**. (app/ponto/motor_calculo_v2.py:44; app/ponto/motor_calculo_v2.py:45; BACKLOG.md:1738) | **ESPERA-RONALD** |
| SEM-CELULA-11-DEVE-TER-CELULA | GRADE-MESTRA.md:243 | `!` para col305 | Decisão sobre o vínculo do col305 | **Resolvida:** `! COL305=(b)` aplicado em 25/10, desativando ec965. Remedição de 16/11 confirma ausência de vínculo cobrindo a 11 e conclui “nada a aplicar”. (RELATO.md:3982; RELATO.md:130) | **DESTRAVADA-JA — baixa** | [nao verificado]
| LAUDO-col33-0310 | GRADE-MESTRA.md:250 | Corte sobre o restante da frota | Corte REJULGAR-COMP10-RESTO-DA-FROTA | **Resolvida:** corte dado em 27/10; CORTES registra atos aplicados e lista HE regenerada. Resíduo col988 ficou em obra própria. (CORTES.md:360; CORTES.md:102; BACKLOG.md:839) | **DESTRAVADA-JA — reconciliar fechamento/resíduo** | [nao verificado]
| H12H06-SEPARA-E-CRONTAB | GRADE-MESTRA.md:257 | Censo de trava; desenho da parte B | Resposta H12H06-B-SIMPLES e pouso da B | **Resolvida:** pergunta respondida, B pousada e aplicação das 28 linhas registrada. O comando correspondente está nesta cópia. (RELATO.md:2401; RELATO.md:2324; RELATO.md:3107; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:2) | **DESTRAVADA-JA — baixa da espera antiga** |
| BO-0611-05 | GRADE-MESTRA.md:270 | Liberação B3; revisão; smoke | LIBERA-B3, certificação e pouso | **PARADA vencida:** corte LIBERA-B3 registrado; opus aprovou, merge `fb9269ea9`, deploy e push registrados. Smoke a/b/d continuava devido; isso não desfaz o pouso já realizado. (CORTES.md:433; RELATO.md:9354; RELATO.md:9368) | **DESTRAVADA-JA — pousada, com smoke residual** |
| SETNULL-ORFA-DNA | GRADE-MESTRA.md:280 | Censo no brief; atrás do ESPELHO-VERDADE | Fecho do espelho e deleção de EC pela porta da célula | **Portão existe; desenho já dado.** Relação ainda usa `SET_NULL`, mantendo presente o sítio descrito no achado. (BACKLOG.md:2259; app/escala/models.py:1692; GRADE-MESTRA.md:889) | **ESPERA-OBRA — ESPELHO-VERDADE** |
| SELOS-CANON-I2 | GRADE-MESTRA.md:292 | SELOS-CANON-I3 | Pouso do I3 | **Existe:** I3 conserva matriz/volta 4 e certificações/pouso pendentes. (BACKLOG.md:805; RELATO.md:1770) | **ESPERA-OBRA — SELOS-CANON-I3** | [nao verificado]
| CONTENCAO-QUE-MORDE-V2 | GRADE-MESTRA.md:304 | Contrato 20; contrato novo | Fechar contrato 20 e apresentar novo contrato de cinco linhas | **Existe:** estacionamento preservado; contrato “chamado, um juiz” ainda não está marcado verde e conserva dependências O168/O169/LASTRO. (BACKLOG.md:977; RELATO.md:2763; app/core/contratos_estruturais.py:262; app/core/contratos_estruturais.py:270) | **ESPERA-OBRA — contrato 20**, depois contrato novo |
| MAIS-CODEX-PLACAR | GRADE-MESTRA.md:331 | Duas curas de produto no ar | Segunda cura e encerramento da moratória | **Resolvida expressamente:** moratória caiu com `9ed75130` + `77eb28ce`; placar voltou à fila sob CONTRATO-DE-INSTRUMENTO. (RELATO.md:3463) | **DESTRAVADA-JA** |
| GATE-EXPORT-10 | GRADE-MESTRA.md:337 | Decisão do DP sobre HE; autorização | Decisões/`!`, relavrar carimbo e executar o ato | **Fonte ainda bloqueia:** registro específico conserva 184 dias sem decisão. Estado atual desses dias **[não verificado]**; há contradição posterior entre “10 exportada” no aval e juiz dizendo não entregue. (BACKLOG.md:1185; RELATO.md:4038; RELATO.md:130) | **ESPERA-RONALD — conservadora** |
| ~~PROVA-DOMINIO~~ | GRADE-MESTRA.md:339 | Execução cortada | Baixa para estado terminal | **Já CORTADA**, fora da fila desde 28/09; fonte manda “Não executar”. (BACKLOG.md:1207) | **DESTRAVADA-JA — somente baixa para CORTADA** |
| HIGIENE-DE-CONTEXTO | GRADE-MESTRA.md:350 | Autorização do item 5, CLAUDE→LAPIDES | Aval para mover a história | **Espera antiga superada:** CLAUDE-MD-ENXUTO recebeu aval e pousou `9ce5181d`, com história em LAPIDES e pistas em PISTA. Meta integral de tamanho não fechou e foi parada pelo aval. (BACKLOG.md:1050; BACKLOG.md:1357; RELATO.md:2879) | **DESTRAVADA-JA — remover espera antiga** |
| O64 | GRADE-MESTRA.md:357 | Investigação do ambiente; `!` | Explicar divergência prod/sombra; depois autorizar apply | **Preservada na fonte:** 10 em produção contra 14 na sombra, sem causa definida. Repetição atual ou solução posterior: **[não verificado]**. (BACKLOG.md:1440) | **ESPERA-DADO — causa da divergência**, depois `!` |
| O91 | GRADE-MESTRA.md:360 | `!` de instalação | Reconciliar com corte “não instala” | **Espera contradita pelo próprio cabeçalho:** FECHADA pelo corte “não instala”, enquanto a célula antiga ainda pede `!`. Isso não autoriza instalar. (BACKLOG.md:1467) | **DESTRAVADA-JA — baixa, sem instalação** |
| O176 | GRADE-MESTRA.md:371 | Corte para conferir conteúdo do pacote | Corte sobre o critério por conteúdo | **Existe:** fonte mantém ESPERA CORTE; código ainda lê títulos e verifica presença do rótulo. (BACKLOG.md:1522; app/core/esteira_vigia.py:296; app/core/esteira_vigia.py:312) | **ESPERA-RONALD** |
| O279 | GRADE-MESTRA.md:388 | Liberação ou cura da regra operacional | Resolver a restrição que impediu corrigir o religador | **Código antigo presente:** `while true` e detecção textual de `PAREI`. Restrição operacional atual do agente: **[não verificado]**. (BACKLOG.md:1655; bin/keepalive.sh:3; bin/keepalive.sh:7) | **ESPERA-RONALD** |
| O296 | GRADE-MESTRA.md:393 | `!` para sítio na zona inviolável | `!` nominal para o comentário no motor | **Fonte preserva a espera** para o décimo sítio; nove externos podem ser separados. Contagem atual e aval posterior: **[não verificado]**. (BACKLOG.md:1673) | **ESPERA-RONALD** |
| PROTEGE-VAZIA-EM-55-LEIS | GRADE-MESTRA.md:403 | Autorização para alterar LEIS.md | Autorizar movimentação entre colunas existentes | **Defeito presente:** L-006 tem sítios em DONO e PROTEGE vazio; selo ainda pula célula vazia. Total atual “55”: **[não verificado]**. (BACKLOG.md:1724; LEIS.md:64; bin/tests/test_lei_protege_sitio.sh:77) | **ESPERA-RONALD** |
| RESIDUO-PREMIG-NO-BIND-MOUNT | GRADE-MESTRA.md:404 | Autorização para apagar | Decisão nominal sobre os arquivos do bind mount | **Autorização continua exigida na fonte.** Os 64 arquivos foram descritos como ignorados pelo git; presença atual no bind mount **[não verificado]** nesta cópia arquivada. (BACKLOG.md:1730) | **ESPERA-RONALD**, com existência atual não verificada |
| PAUTA-PREFIXO-IDENTIDADE-NO-TEXTO | GRADE-MESTRA.md:445 | Corte de desenho; congelamento | Decidir desenho da identidade textual | **Congelamento preservado; defeitos presentes:** motivo vazio retorna normalmente; identidade textual continua cortada em `[:480]`. (BACKLOG.md:2173; app/core/management/commands/alarme_esteira.py:34; app/core/management/commands/alarme_esteira.py:56) | **ESPERA-RONALD** |
| O256 | GRADE-MESTRA.md:476 | Porta PDF; O261; smoke final | PDF publicado, O261 curada; smoke como revisão final | **Travas de avanço vencidas:** rota PDF existe; O261 consta fechada; O310 e lente pousaram. Corte diz explicitamente que smoke do retrato não segura push. Smoke positivo posterior aos últimos pousos: **[não verificado]**. (app/folha/urls.py:24; BACKLOG.md:1652; BACKLOG.md:1687; BACKLOG.md:1696; CORTES.md:65) | **DESTRAVADA-JA — revisão de smoke residual** |
| AVISO-ENTRADA-ANTECIPADA | GRADE-MESTRA.md:477 | Descongelamento pela L-096 | Aval que alcance o serviço Python ou encerramento do congelamento | **Existe:** BACKLOG mantém CONGELADA. Exceção da raia UI cobre templates/static/testes, enquanto esta entrega inclui `aviso_da_batida.py`. (BACKLOG.md:1212; CLAUDE.md:311; LEIS.md:114) | **ESPERA-RONALD** | [nao verificado]
| AVISO-ESPURIA-JANELA — desenho | GRADE-MESTRA.md:478 | Confirmação do admin da folha e do dono | Confirmação de ambos sobre janela e mensagem | **Preservada:** “REGISTRADO, NAO CONSTRUIR”, aguardando confirmação. Resposta posterior: **[não verificado]**. (BACKLOG.md:1213) | **ESPERA-RONALD**, também admin |
| BUG-APP-SESSAO-401 | GRADE-MESTRA.md:479 | Autorização; smoke nas duas cascas | `!` e smoke para a metade APP | **Preservada:** admin no ar, APP aguardando `!`/smoke. Autorização e smoke posteriores: **[não verificado]**. (BACKLOG.md:1238) | **ESPERA-RONALD** |
| AVISO-DO-DONO | GRADE-MESTRA.md:481 | Portão da 09; descongelamento; smoke no celular | Liberação específica e smoke do push | **Congelamento explícito permanece.** Exceção de UI não cobre automaticamente o emissor de push por evento. Portão da 09 cumprido e smoke específico: **[não verificado]**. (BACKLOG.md:1254; CLAUDE.md:311) | **ESPERA-RONALD** | [nao verificado]
| F2 | GRADE-MESTRA.md:486 | Contratos 22/22; O157 | Fecho estrutural e O157 | **Existe, com teto antigo:** teto atual é 20. O157 não começou; código ainda conta somente `tipo='falta'`, sem a suspensão exigida pelo corte. (app/core/contratos_estruturais.py:1041; BACKLOG.md:1501; app/ferias/models.py:144) | **ESPERA-OBRA — O157 + fecho estrutural** |
| O30 | GRADE-MESTRA.md:487 | Vale F1; `pillow_heif`; rebuild | Rebuild HEIC e liberação do complemento F3 POS-20 | **Parcial:** vale F1 já dado em 03/10, “A F2 ABRE”. F3 continua POS-20, dependente do decodificador/próximo rebuild. Rebuild concluído: **[não verificado]**. (PENDENTES_RONALD.json:2093; RELATO.md:1112; BACKLOG.md:1401) | **ESPERA-OBRA — rebuild HEIC / F3 POS-20** |
| O206 | GRADE-MESTRA.md:493 | Correção da prévia; smoke das duas cascas | Prévia pela autoridade e smoke aceito | **Resolvida:** template lê texto da autoridade; comunicação aprovada em 17/10. Restos de push nativo/disputa foram separados na O311. (app/templates/chamados/partials/_js_cobranca.html:30; PENDENTES_RONALD.json:2360; BACKLOG.md:1688) | **DESTRAVADA-JA** |
| O310-RAIOX | GRADE-MESTRA.md:499 | Corte para história, modelo e migration | Corte específico para histórico | **Existe para o complemento:** lente já NO AR `3ea519d7`, mas fonte mantém “história (modelo + migration) é corte dele”. Corte posterior: **[não verificado]**. (BACKLOG.md:1696) | **ESPERA-RONALD** |
| E3-ENTRADA-ORFA-SEM-TURNO — balde 20/20 | GRADE-MESTRA.md:121 | Lei para dia sem turno com batida | Corte sobre a entrada real órfã sem turno previsto | **Preservada:** fonte distingue esse caso da lei do dia sem nada; ainda aparece na fila como pergunta de lei. Corte posterior: **[não verificado]**. (BACKLOG.md:2176; RELATO.md:1729) | **ESPERA-RONALD** |

**Totais da classificação acima**, mantendo o recorte das 43 linhas principais e as ressalvas de cada linha. (GRADE-MESTRA.md:121; GRADE-MESTRA.md:177; GRADE-MESTRA.md:280; GRADE-MESTRA.md:292; GRADE-MESTRA.md:445; GRADE-MESTRA.md:476; GRADE-MESTRA.md:499)

| balde | DESTRAVADA-JA | ESPERA-RONALD | ESPERA-OBRA | ESPERA-DADO | total |
|---|---:|---:|---:|---:|---:|
| LATERAL-ESTRUTURA | 7 | 5 | 5 | 2 | **19** |
| INSTRUMENTO | 4 | 7 | 2 | 1 | **14** |
| UI | 2 | 5 | 2 | 0 | **9** |
| 20/20 | 0 | 1 | 0 | 0 | **1** |
| **Total** | **13** | **18** | **9** | **3** | **43** |

## 17/11 08:08 · sessao 320413038536 · - · - · origem=Ronald (tmux)

Confirmado: corrija L2 para **MUDOU — contagem incorreta**, sem atribuir isso a uma obra posterior. Há **18 funções com save/create/get_or_create**, mais **1 função com delete**, total **19** incluindo exclusão; os sítios adicionais à amostra são `agendar_ferias_porta`, `app/ferias/services.py:316`; `aplicar_art130`, `app/ferias/services.py:412`; e `excluir_periodo_aquisitivo`, `app/ferias/services.py:876`.

Esses sítios podem já existir no momento do inventário; novidade temporal/commit [nao verificado]. [nao verificado]

Delta das perguntas 1–8:

| pergunta | conferência atual |
|---|---|
| 1 — cancelamento iniciado | Permanece: botão por data `app/ferias/models.py:340`, botão por status `app/templates/ferias/saldo.html:70`, porta aceita `em_gozo` `app/ferias/services.py:652`; fila/lei explicitada em `app/docs/RELATO.md:842`. |
| 2 — PA em_curso pode agendar | Permanece: `pode_agendar` exige adquirido `app/ferias/models.py:234`; porta normal não exige esse estado `app/ferias/views.py:245`, `app/ferias/services.py:578`; solicitação exige `app/ferias/services.py:765`. |
| 3 — vencendo/vencido | Permanece: juiz por data `app/ferias/models.py:260`, filtros gravados/literal 60 `app/ferias/views.py:64`, `app/ferias/services.py:263`; RC1-2 sem construtor `app/docs/BACKLOG.md:759`. |
| 4 — confirmado_em na recusa | Permanece: recusa salva somente confirmação `app/ferias/services.py:908`; sentido histórico ou vigente [nao verificado]. |
| 5 — guardas sob trava | Permanece: guardas externas `app/ferias/views.py:945`, `app/ferias/views.py:994`; escritores só vetam cancelado `app/ferias/services.py:887`, `app/ferias/services.py:905`. |
| 6 — SOL sem nascente | Criação produtiva não identificada; leitura permanece `app/ferias/views.py:41`, `app/ferias/views.py:785`; canal externo/dado existente [nao verificado]. |
| 7 — relógio residual | Permanece `app/ferias/services.py:654`, `app/ferias/services.py:681`, `app/ferias/services.py:686`, `app/ferias/services.py:934`, `app/ferias/views.py:29`, `app/ferias/views.py:117`; residual explicitamente em fila `app/docs/RELATO.md:847`. |
| 8 — comentário concluído | Permanece inexato `app/ferias/models.py:304`; troca de janela e cron podem mover concluído `app/ferias/services.py:721`, `app/ferias/services.py:954`. |

_6399 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
