# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 00:49** · publicado em **15/11/2026 00:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 00:49 · sessao 2b5f0dfe31a1 · MADRUGADA-PREPARA · leitura · origem=Code

A incompatibilidade central **SEGUE**: o retro entrega `foto_ausente.batida_id`; o gerador e o derivador de origem procuram `ultima_batida.id`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:3606`)

O vínculo Git desta cópia ao hash solicitado não pôde ser confirmado: `git rev-parse HEAD` não reconheceu repositório. As referências abaixo são das linhas lidas no diretório fornecido. **[nao verificado quanto à identidade Git e à comparação histórica entre commits]** [nao verificado]

## RECONFERÊNCIA DAS CITAÇÕES

As ocorrências repetidas entre cef, matriz e r.r estão reunidas nas tabelas. **MUDOU** indica referência ou enunciado que precisa de atualização; não certifica quando ocorreu a alteração. **[nao verificado quanto ao histórico]**

### Referências que MUDARAM

| Citação dos anexos | Referência atual | Conferência |
|---|---|---|
| `models.py:2437` — r.r | `app/chamados/models.py:2514` | Contrato da coluna de episódio BATIDA; campo em `app/chamados/models.py:2518`. |
| `models.py:2784` — r.r | `app/chamados/models.py:2861` | Pergunta datada recebe dia do turno; conversa tem porta própria. |
| `models.py:2795`, `:2802` — r.r | `app/chamados/models.py:2872`; `app/chamados/models.py:2879` | `abrir_conversa`; grava dia civil local do pedido. |
| `models.py:2850-2859` — matriz | `app/chamados/models.py:2872` | Nascimento da conversa; `conversa=True` e `localdate()` em `app/chamados/models.py:2879`. |
| `models.py:2867` — r.r | `app/chamados/models.py:2949` | `_nascer` recebe ou deriva episódio antes de perguntar o dia. |
| `models.py:2876` — r.r | `app/chamados/models.py:2958` | Consulta `dia_do_turno_da_linha`, passando o dia pedido como insumo. |
| `models.py:2965-2976` — matriz | `app/chamados/models.py:2992` | A recusa citada é de episódio já representado por linha recusada em outro dia; não é recusa genérica de qualquer dia pedido divergente. O nascimento toma o dia do juiz. (`app/chamados/models.py:2938`) |
| `models.py:2982` — matriz | `app/chamados/models.py:3009` | Recusa de episódio retratado antes do reuso. |
| `models.py:3450` — r.r | `app/chamados/models.py:3592` | `_chave_de_identidade` deriva origem quando não é conversa e a origem está ausente. |
| `models.py:3453` — r.r | `app/chamados/models.py:3595` | Definição de `batida_origem_do_nascimento`. |
| `models.py:3457` — cef/r.r | `app/chamados/models.py:3599` | Advertência sobre contexto fundido; ele pode apontar outra anomalia e não recompõe a origem do acervo. |
| `models.py:3462-3466`, particularmente `:3464` — cef/r.r | `app/chamados/models.py:3603`; `app/chamados/models.py:3606` | Família BATIDA e leitura de `ultima_batida.id`. |
| `models.py:3468` — r.r | `app/chamados/models.py:3610` | Exige batida pertencente ao colaborador da disputa. |
| `models.py:3490` — cef/r.r | `app/chamados/models.py:3631` | Índice por episódio e motivo; parcial, condicionado a horizonte, origem preenchida, ausência de conversa e vias admitidas. |
| `models.py:3508-3523` — matriz | `app/chamados/models.py:3595` | Derivador completo: família, envelope, existência e posse da batida. |
| `models.py:3537-3550` — matriz | `app/chamados/models.py:3624` | Índices por família; condição do episódio em `app/chamados/models.py:3633`. |
| `models.py:3604` — r.r | `app/chamados/models.py:3746` | Literal do catálogo legado; não é o envelope da disputa. |
| `models.py:4094` — r.r | `app/chamados/models.py:4236` | Reexportação da porta e do gerador. |
| `core/juizes.py:208` — r.r | `app/core/juizes.py:212` | Registro de `turnos_do_colab`; a antiga linha aponta hoje outro registro. |
| `core/juizes.py:1199` — r.r | `app/core/juizes.py:1205` | Registro de `selfie_ausente`. |
| `docs/CORTES.md:283` — r.r | `app/docs/CORTES.md:334` | Corte integral da selfie; também resumido em `app/docs/CORTES.md:94`. |
| `docs/LEIS.md:152` como L-114 — r.r | `app/docs/LEIS.md:153` | Certificado, impacto e termômetro. |
| `docs/LEIS.md:153` como L-113 — r.r | `app/docs/LEIS.md:154` | Nascimento permitido em exportada; regime de alteração do gravado continua distinto. |
| `BACKLOG.md:2109` — matriz | `app/docs/BACKLOG.md:2166` | Mesmo desencontro entre envelope emitido e consumido; texto atualizado com aval posterior. |

### Referências que SEGUEM

Em cada linha abaixo, as referências dos anexos permanecem no mesmo arquivo, linha e significado indicado. As referências relativas da matriz foram expandidas para `app/`. (`app/config/crons.py:686`; `app/chamados/services/disputa_emissao.py:300`)

| Arquivo e citações conferidas — SEGUE | Significado conservado |
|---|---|
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:45`, `:65`, `:66-69` | Janela desde agora, padrão de três dias de duração, bordas inclusivas, origem de aparelho e exclusão de retratadas. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:70-77` | Duas fontes de foto; delegação ao juiz; minutos reais desde a batida. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:81`, `:89-92` | Cabeçalho temporal e retorno anterior à emissão quando não há `--apply`. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:95-106`, especialmente `:96`, `:98-102` | Emissão com `foto_ausente`, sem `ultima_batida`; PK, tipo, instante local do fato e proveniência. |
| `app/ponto/registro_batida.py:72`, `:75-77`, `:80-91` | PWA zero; APK/NULL noventa minutos; override preserva PWA; comparação `>=`. A lista de origens começa em `app/ponto/registro_batida.py:71`. |
| `app/config/crons.py:686` | Cron `26 7 * * *`, retro com `--apply`. |
| `app/api/views.py:447-453`, `:507`, `:655` | Juiz da selfie no ato; obtenção de instante efetivo; entrega desse instante à porta da batida. |
| `app/api/views.py:758-766` | Envelope com PK, tipo e `timestamp=agora`; chamada da porta. |
| `app/api/views.py:833-859` | Upload: definição em `app/api/views.py:835`, anexação em `:847`, resolução em `:857`, resposta em `:859`. O intervalo citado continua correto. |
| `app/api/views_core.py:439`, `:752-758` | Mesmo formato do envelope na API gêmea. |
| `app/ponto/services/triagem_batida.py:735`, `:739` | Emissão de `saida_sem_entrada`; dia do juiz em `app/ponto/services/triagem_batida.py:740`. |
| `app/chamados/services/fabrica_da_celula.py:71`, `:139`, `:172`, `:174` | Motivos pelos marcos da ata; transporte de dia; chamadas da porta e do gerador. |
| `app/chamados/services/declaracao_texto.py:42` | Tabela de motivo por posição e tipo de marco. |
| `app/chamados/management/commands/smoke_chamados.py:103` | Chamada direta do gerador. |
| `app/chamados/services/disputa_emissao.py:72`, `:294`, `:300`, `:325-329` | Referência de batida para grade; chamada e definição do gerador; dedup de topo quando falta episódio. |
| `app/chamados/services/disputa_emissao.py:371-377`, `:387-392` | Primeiro lê `motivo.data`; depois procura episódio e pergunta seu dia ao juiz. |
| `app/chamados/services/disputa_emissao.py:436-441`, `:470-471` | Dedup sequencial de conversa, nascimento e retorno das perguntas. |
| `app/chamados/services/disputa_emissao.py:535-566`, especialmente `:548`, `:552-558`, `:560-565` | Agregação do contexto, contador, instante do merge, substituição das demais chaves de topo e persistência. |
| `app/chamados/services/disputa_emissao.py:568-603`, especialmente `:568`, `:587-592`, `:597` | Nascimento do chamado e da disputa; contexto inicial e trilha. |
| `app/chamados/services/disputa_emissao.py:634-688`, especialmente `:659`, `:664`, `:670`, `:682` | Porta pública, motivos sem repetição, descrição de origem e chamadas do gerador. |
| `app/api/foto_recebida.py:28-54`, `:57-85` | Resposta automática pela porta, validação pelo dono, seleção por última batida e tratamento coletivo das perguntas. |
| `app/chamados/juizes.py:138`, `:141-168`, `:226` | Último degrau temporal, compartilhado entre singular e lote. A entidade de entrada precisa da correção descrita abaixo. |
| `app/ponto/turnos.py:2927-2936`, `:2955` | Dia do turno da batida; não reclamada usa dia local; retratada fica fora do universo. |
| `app/chamados/catalogo/motivos.py:16`, `:35`, `:43`, `:53` | Identidade BATIDA por episódio; motivo da triagem MARCO; família pelo catálogo. |
| `app/chamados/catalogo/query.py:45-66`; `app/chamados/catalogo/perguntas.py:93-98` | Interpolação de data quando o texto a pede; texto de foto permanece escolha sem `{data}`. |
| `app/chamados/tests/test_e1_v5_familia.py:25`, `:98` | Selo da declaração de família e das chaves dos índices; não prende o envelope dos emissores. |
| `CLAUDE.md:142`, `:143`, `:146`, `:148`, `:178`, `:179`, `:411` | Zonas invioláveis, fuso, previsto, idempotência, separação dos donos e regime do gravado exportado. |
| `app/docs/LEIS.md:107`, `:136`, `:145` | Fuso; casos pela regra antes do código; horizonte e backfill único. |
| `app/docs/JUIZES.md:14`, `:20`, `:38` | Prevalência do vivo, juiz do turno e dono da validação. |
| `app/colaboradores/services/ficha.py:215`; `app/escala/services/furos_diarios.py:281`; `app/relatorios/views.py:1675` | Homônimos de ficha/relatório, separados do envelope da disputa; a ficha tem escritor em `app/colaboradores/services/ficha.py:147`. |

### Testes citados: símbolo SEGUE, alcance conferido [nao verificado]

Os métodos citados existem nos sítios abaixo. Execução e resultado dos testes: **[nao verificado]**. [nao verificado]

| Citação da matriz/preparo | Local atual e alcance lido |
|---|---|
| A1 — dry-run | `app/ponto/tests/test_bug126_selfie_invisivel.py:103`: verifica ausência de disputa, não ausência de toda escrita possível. |
| A3 — origem retroativa | `app/ponto/tests/test_bug126_selfie_invisivel.py:91`: exclui origem retroativa; não demonstra o caso `retratada_em`. |
| A4 — manual e origem do juiz | `app/ponto/tests/test_bug126_selfie_invisivel.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:45`. |
| A5 — FotoBatida | `app/ponto/tests/test_bug126_selfie_invisivel.py:74`; `app/ponto/tests/test_selfie_ausente_juiz.py:103`. |
| A6 — Batida.foto | `app/ponto/tests/test_selfie_ausente_juiz.py:112`. |
| A7 — prazo e canal NULL | `app/ponto/tests/test_bug126_selfie_invisivel.py:84`; `app/ponto/tests/test_selfie_ausente_juiz.py:41`. |
| A8 — retro elegível | `app/ponto/tests/test_bug126_selfie_invisivel.py:65`: confere `batida_id`/`_por` em `app/ponto/tests/test_bug126_selfie_invisivel.py:71`; não confere pergunta ou episódio. |
| A9 — agregação sobre PWA | `app/ponto/tests/test_selfie_ausente_juiz.py:119`: confere disputas; payload sintético também sem `ultima_batida` em `app/ponto/tests/test_selfie_ausente_juiz.py:122`. |
| A10 — repetição | `app/ponto/tests/test_bug126_selfie_invisivel.py:108`: asserção conta disputas em `app/ponto/tests/test_bug126_selfie_invisivel.py:113`. |
| A12 — API PWA | `app/api/tests/test_api_bater_pwa.py:64`, `:78`, `:95`: motivo, contador e foto; não identidade da pergunta. |
| A13 — prazo delegado | `app/ponto/tests/test_selfie_ausente_juiz.py:82`. |
| B1 — conversa sem envelope | `app/chamados/tests/test_a2_data_evento_batida.py:53`: motivo diferente de foto; data civil conferida em `app/chamados/tests/test_a2_data_evento_batida.py:56`. |
| B4 — corrida no índice | `app/chamados/tests/test_e1_v5_indice.py:69`: inserção crua por episódio, usando GPS. |
| B5 — episódios distintos e repetição | `app/chamados/tests/test_e1_v5_indice.py:84`; `app/chamados/tests/test_a2_data_evento_batida.py:22`. |
| B6 — segunda batida | `app/chamados/tests/test_e1_v6_horizonte.py:262`: contexto sintético; não executa o retro. |
| B9 — texto de foto sem data | `app/chamados/tests/test_cenarios_disputa.py:43`. |
| B10 — conversa fora do índice | `app/chamados/tests/test_e1_v5_indice.py:96`: permissividade do índice, não dedup sequencial do gerador. |
| C1/C4 — upload | `app/chamados/tests/test_b8_smoke_portas.py:60`: confere Batida.foto e FotoBatida em `app/chamados/tests/test_b8_smoke_portas.py:68`; não confere fecho. |
| C2 — aparar motivo | `app/chamados/tests/test_porta_chamado_e3.py:56`, `:58`, `:62`: porta direta e repetição; não upload. |
| D2 — selo da selfie | `app/ponto/tests/test_selfie_ausente_juiz.py:130`: AST nas APIs; não demonstra contrato de episódio do retro. |

“SEM PROVA” não foi convertido em inexistência universal de teste: cobertura indireta ou dinâmica de todos esses cenários permanece **[nao verificado]**. Há teste de conversa com episódio retratado usando GPS, sem demonstrar especificamente foto. (`app/chamados/tests/test_e1_v6_horizonte.py:445`)

### SUMIU / fontes ausentes

Nenhuma das funções citadas acima SUMIU; seus sítios atuais constam das tabelas, incluindo `app/chamados/models.py:3595`, `app/chamados/services/disputa_emissao.py:300` e `app/chamados/services/disputa_emissao.py:634`.

**SUMIU da cópia disponível — existência histórica [nao verificado]:** o caminho fonte `logs/PREPARACAO/censos/CONTRATO-DE-ENTRADA-DO-FIO/r.r`. Os textos cef/matriz/q.q/r.r/aval foram conferidos pelo envelope, sem arquivo local numerado. **[nao verificado quanto à proveniência material desses anexos]**

`backlog_4obras.txt` e `arquivos_em_voo.txt`, referidos em q.q/r.r, não estão disponíveis no envelope nem como arquivos desta cópia. O cruzamento histórico de raias e a conclusão “arquivos em comum = 0” ficam **[nao verificado]**.

### Correções de significado da matriz

| Trecho | Estado da afirmação |
|---|---|
| A1–A7, A12–A13 | **SEGUE** o comportamento de entrada descrito; os limites das provas estão na tabela de testes. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66`; `app/ponto/registro_batida.py:80`; `app/api/views.py:447`) |
| A8–A9 | **SEGUE** o envelope incompatível e a preservação de chave antiga ausente na emissão nova. O resultado concreto em produção não está demonstrado pelo envelope. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:548`) |
| A10 | **MUDOU o alcance da conclusão:** não duplicar disputa não prova “mesmo estado”; o merge incrementa contador e troca instante. Os campos estão dentro de `contexto[motivo]`, não em colunas próprias da disputa. (`app/chamados/services/disputa_emissao.py:552`; `app/chamados/services/disputa_emissao.py:558`; `app/ponto/tests/test_bug126_selfie_invisivel.py:113`) |
| A11 | **MUDOU o alcance:** a porta procura disputa aberta; a garantia do índice de pergunta é parcial e não autoriza enunciado irrestrito para todo acervo. (`app/chamados/services/disputa_emissao.py:531`; `app/chamados/models.py:3633`) |
| B1–B2 | **SEGUE** a conversa quando falta dia e o dedup de topo quando falta episódio. (`app/chamados/services/disputa_emissao.py:325`; `app/chamados/services/disputa_emissao.py:431`) |
| B3 | **SEGUE** a possibilidade de consumir episódio antigo; “nasce uma pergunta errada” não é resultado inevitável, pois a porta também pode reutilizar linha. (`app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:2990`; `app/chamados/models.py:3091`) |
| B4–B6 | **SEGUE**, respeitadas as condições do índice e o domínio do fato. (`app/chamados/models.py:2795`; `app/chamados/models.py:3631`) |
| B7 | **SEGUE com ramos separados:** gerador sem dia pode produzir conversa; porta datada com origem retratada recusa. (`app/chamados/services/disputa_emissao.py:431`; `app/chamados/services/fio.py:1685`) |
| B8 | **MUDOU a explicação:** dia pedido diferente não provoca, por si, a recusa citada; o episódio determina o dia pelo juiz. (`app/chamados/models.py:2938`; `app/chamados/models.py:2992`) |
| B9–B10 | **SEGUE** o texto sem data e o dedup sequencial de conversa; mudar o texto continua pergunta separada. (`app/chamados/catalogo/perguntas.py:93`; `app/chamados/services/disputa_emissao.py:436`) |
| C1–C4 | **SEGUE** o código de seleção por disputa/última batida e validação coletiva. Há corte escrito posterior específico para o alcance por episódio. (`app/api/foto_recebida.py:67`; `app/api/foto_recebida.py:73`; `app/api/foto_recebida.py:79`; `app/docs/CORTES.md:441`) |
| D1 | **MUDOU a entidade descrita:** o contexto fraco vem do chamado, não diretamente da disputa. O nascimento recebe cópias separadas; o merge da disputa não atualiza o chamado nesse ramo. (`app/chamados/juizes.py:100`; `app/chamados/juizes.py:138`; `app/chamados/services/disputa_emissao.py:580`; `app/chamados/services/disputa_emissao.py:590`; `app/chamados/services/disputa_emissao.py:562`) |
| D2 | **SEGUE** o alcance limitado do selo existente. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`) |
| D3 | Os números e a distribuição do passivo continuam apenas como dado anexado; reconstrução individual e alcance da recuperação **[nao verificado]**. A restrição do contexto fundido permanece escrita em `app/chamados/models.py:3599`. |

## BRIEF ATUALIZADO — CONTRATO-DE-ENTRADA-DO-FIO

1. **Objeto da leitura:** contrato entre emissão retro, disputa e nascimento da pergunta; origem em `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`.
2. **Desencontro confirmado:** retro grava PK em `foto_ausente.batida_id`; origem e dia procuram `ultima_batida.id`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/models.py:3606`; `app/chamados/services/disputa_emissao.py:387`)
3. **Dono:** a classificação ESTRUTURA do brief anterior era inferência; permanece PERGUNTA separada, sob a separação escrita dos donos. (`CLAUDE.md:179`)
4. **Regra de preparação:** casos vêm da regra antes do código; escolha sem resposta escrita permanece pergunta. (`app/docs/LEIS.md:136`)
5. **Identidade escrita:** `foto_ausente` pertence à família BATIDA, cuja chave é episódio e motivo. (`app/chamados/catalogo/motivos.py:16`; `app/chamados/catalogo/motivos.py:43`)
6. **Dia escrito:** o episódio consulta o juiz do turno; uma batida não reclamada usa seu dia local, não uma conversão livre do envelope. (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2955`)
7. **Repetição:** a porta de pergunta e o índice tratam identidade por episódio; o contador de emissões da disputa continua diferente dessa identidade. (`app/chamados/models.py:2795`; `app/chamados/models.py:3631`; `app/chamados/services/disputa_emissao.py:552`)
8. **Contexto acumulado:** o próprio derivador declara que contexto fundido pode apontar outra anomalia e não recupera a origem histórica do acervo. (`app/chamados/models.py:3599`)
9. **Tempo:** instante efetivo da batida, recepção e dia do turno são insumos distintos; o envelope atual da API contém recepção. (`app/api/views.py:507`; `app/api/views.py:655`; `app/api/views.py:761`; `app/ponto/turnos.py:2928`)
10. **Selos existentes:** família/índice e delegação à selfie estão cobertos pelos selos citados; a obrigação e o alcance de selo de envelope ficam como PERGUNTA. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:130`)
11. **Outros consumidores:** upload seleciona disputa por `ultima_batida.id`; dia fraco lê o contexto do chamado e extrai dia civil do ISO. (`app/api/foto_recebida.py:67`; `app/chamados/juizes.py:100`; `app/chamados/juizes.py:166`)
12. **Restrições escritas:** motor exige aval; escrita de batida tem porta; previsto tem fonte única. (`CLAUDE.md:142`; `CLAUDE.md:143`; `CLAUDE.md:148`)
13. **Exportada:** nascimento e alteração do gravado têm regimes distintos; não há autorização de alteração genérica do fechamento nesta leitura. (`app/docs/LEIS.md:154`; `CLAUDE.md:411`)
14. **Passivo:** 135/15/44/76 são números do anexo; critério de classificação, contaminação por contexto e recuperação individual **[nao verificado]**. O uso posterior do contexto fundido tem a limitação escrita em `app/chamados/models.py:3599`.
15. **E3 no nascimento:** existe recusa de resposta carimbada sem texto antes das travas e também no `save`; dia e projeção de decisão passam pelos pontos atuais de escrita. (`app/chamados/models.py:2919`; `app/ch

[truncado em 20000 de 36567 caracteres]

## 15/11 00:49 · sessao a309270b1b28 · MADRUGADA-PREPARA · sonda · origem=Code

A quantidade atual de vínculos inválidos e o efeito de um saneamento permanecem **[nao verificado]**. A cópia já contém a constraint no modelo e uma migration com `CHECK ... NOT VALID`; sua aplicação no banco é **[nao verificado]**. (`app/escala/models.py:1037`, `app/escala/migrations/0041_ec_vigencia_not_valid.py:43`.)

Os escritores operacionais das datas são:

| Escritor | Escrita |
|---|---|
| `executar_vinculo` | Fecha pela porta; reabre com fim nulo; cria com início informado. `app/colaboradores/services/vinculo.py:214`, `app/colaboradores/services/vinculo.py:241`, `app/colaboradores/services/vinculo.py:277`, `app/colaboradores/services/vinculo.py:283`, `app/colaboradores/services/vinculo.py:287`, `app/colaboradores/services/vinculo.py:291`. |
| `fechar_vigencia` | Atualiza fim e desativa, somente onde início ≤ fim informado. `app/colaboradores/services/vinculo.py:641`. |
| `reabrir_vigencia_impossivel` | Atualiza fim e reativa. `app/colaboradores/services/vinculo.py:730`. |
| `encerrar_vinculo` | Atribui fim e salva. `app/colaboradores/services/vinculo.py:844`, `app/colaboradores/services/vinculo.py:845`. |
| `criar_trecho_retroativo` | Cria com início e fim. `app/colaboradores/services/vinculo.py:970`, `app/colaboradores/services/vinculo.py:972`. |
| `corrigir_vinculo_retroativo` | Atribui fim e salva. `app/colaboradores/services/vinculo.py:982`, `app/colaboradores/services/vinculo.py:983`. |
| `reverter_para_snapshot` | Reescreve início e fim do snapshot. `app/colaboradores/services/vinculo.py:1031`, `app/colaboradores/services/vinculo.py:1032`. |
| `ajustar_vinculo_pelo_sistema` | Fecha pela porta e cria outro início. `app/colaboradores/services/vinculo.py:1087`, `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1090`. |
| `desfazer_ajuste_do_sistema` | Reabre com fim nulo ou fecha pela porta e cria outro início. `app/colaboradores/services/vinculo.py:1106`, `app/colaboradores/services/vinculo.py:1108`, `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1111`. |
| `fechar_vinculos_ativos` | Delega o fechamento dos vínculos `ativa=True`. `app/colaboradores/services/vinculo.py:862`, `app/colaboradores/services/vinculo.py:863`. |
| `gravar_regua` | Aceita `**campos` e faz `update(**campos)`, sem lista restritiva de campos nessa função. Os chamadores encontrados passam campos de régua. `app/colaboradores/services/vinculo.py:1048`, `app/colaboradores/services/vinculo.py:1070`, `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:742`, `app/escala/regua_defesa.py:765`. |

As entradas desses escritores incluem wizard, chat, régua, comando de fase, desvinculação, desligamento, correção retroativa, recusa de aceite, ajuste automático e reparo de vigência. (`app/colaboradores/views.py:423`, `app/colaboradores/services/troca_pelo_chat.py:75`, `app/escala/regua_defesa.py:321`, `app/ponto/management/commands/censo_fase_12x36.py:423`, `app/escala/views.py:269`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`, `app/colaboradores/services/desligamento.py:148`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`, `app/chamados/services/acoes_chamado.py:149`, `app/escala/services/escala_auto_executor.py:103`, `app/escala/services/escala_auto_executor.py:145`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:107`.)

O admin de `EscalaColaborador` usa `SoLeitura`, que recusa adição, alteração e exclusão. Fixtures de testes também criam vínculos; não são entradas operacionais. (`app/escala/admin.py:19`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`, `app/tenants/tests.py:44`, `app/colaboradores/tests/test_ficha_do_colab.py:88`.)

Os leitores diretos do intervalo apresentam estes comportamentos:

| Leitor | Tratamento observável de fim < início |
|---|---|
| Carregamento de vínculos do período | Filtra interseção com a janela, sem validar o par início/fim; `mais=` também carrega geradoras fora desse filtro. `app/escala/alimentacao.py:149`, `app/escala/alimentacao.py:151`, `app/escala/alimentacao.py:155`. |
| `escalas_do_periodo` | Delega ao carregador plural. `app/escala/alimentacao.py:185`. |
| `vinculo_cobre_a_data` e versão SQL | Exigem início ≤ dia e fim nulo ou fim ≥ dia; um par invertido não cobre dia algum. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:223`. |
| `vinculo_do_dia` | Confere limites da geradora; no caminho sem geradora usa `vinculo_cobre_a_data`. O invertido não responde pelo dia. `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`. |
| `previstas_do_periodo` | Recorta a fatia e pula explicitamente fim < início. `app/escala/alimentacao.py:390`, `app/escala/alimentacao.py:392`. |
| Apuração por fatias | Recorta pelos limites do vínculo e pula explicitamente fatia invertida. `app/ponto/services/fechamento.py:206`, `app/ponto/services/fechamento.py:207`, `app/ponto/services/fechamento.py:242`. |
| Censo `vigencias_impossiveis` | Chama o validador real e recolhe as recusas. `app/colaboradores/services/vinculo.py:496`, `app/colaboradores/services/vinculo.py:499`, `app/colaboradores/services/vinculo.py:500`. |
| `vinculos_inconsistentes` | Identifica invertidos e os separa do cálculo de sobreposição. `app/escala/services/vinculos_inconsistentes.py:24`, `app/escala/services/vinculos_inconsistentes.py:30`, `app/escala/services/vinculos_inconsistentes.py:37`. |
| `vigencia_sem_trilha` | Separa o passivo usando `vigencias_impossiveis`. `app/escala/services/vigencia_sem_trilha.py:66`, `app/escala/services/vigencia_sem_trilha.py:77`. |
| Rejulgamento de furos | Usa `data__range` com início e fim recortado; não valida o par nesse trecho. `app/escala/regua_defesa.py:686`. |
| Piso da fase | Consulta cobertura pelo juiz SQL; sem cobertura, usa o menor início, sem consultar fim nessa alternativa. `app/escala/services/fase_na_tela.py:160`, `app/escala/services/fase_na_tela.py:165`. |
| Propositor | Escolhe pelo juiz do dia e depois exclui encerramentos anteriores ao piso de aplicação. `app/escala/services/propositor.py:448`, `app/escala/services/propositor.py:460`. |
| Explicação de cadastro zero no export | Alimenta o previsto por dia; também lê finais para explicar encerramento. `app/folha/export.py:938`, `app/folha/export.py:957`. |
| Signals | Capturam e comparam os pares anterior/atual e calculam janelas de regeneração; esses trechos não validam fim ≥ início. `app/escala/signals.py:25`, `app/escala/signals.py:57`, `app/escala/signals.py:60`, `app/escala/signals.py:68`, `app/escala/signals.py:121`, `app/escala/signals.py:200`. |
| Diagnóstico comparativo | Conserva a seleção antiga como referência de comparação. `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:117`. |
| Gerador de batidas smoke | Usa interseção de datas sem validar o par nesse filtro. `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73`. |

Outros consumidores chegam à cobertura/seleção pelas funções reais acima:

| Família | Sítios |
|---|---|
| Folgas, calendário e jornada | `app/escala/folgas.py:451`, `app/escala/folgas.py:487`, `app/escala/folgas.py:493`, `app/escala/folgas.py:539`, `app/escala/folgas.py:624`, `app/escala/services/semana_do_posto.py:75`, `app/colaboradores/services/calendario.py:505`, `app/escala/servico_jornada.py:63`. |
| Cadastro e ficha | `app/escala/services/cadastro_realidade.py:489`, `app/escala/services/cadastro_realidade.py:509`, `app/escala/services/cadastro_realidade.py:756`, `app/escala/services/cadastro_realidade.py:760`, `app/colaboradores/queries.py:64`, `app/colaboradores/services/ficha.py:129`. |
| Células e regeneração | `app/ponto/management/commands/gerar_celulas.py:283`, `app/ponto/portas/celula.py:229`, `app/ponto/portas/celula.py:339`, `app/ponto/portas/celula.py:906`, `app/ponto/portas/regen_exportada.py:147`, `app/ponto/services/cadastro_futuro.py:70`. |
| Espelho, diagnóstico e PDF | `app/ponto/services/diagnostico_escala.py:53`, `app/ponto/services/espelho.py:286`, `app/ponto/services/espelho.py:1065`, `app/relatorios/pdf_espelho.py:307`, `app/relatorios/pdf_espelho.py:571`. |
| Calculador e turnos | `app/ponto/calculador/alimentacao.py:192`, `app/ponto/turnos.py:1960`, `app/ponto/turnos.py:2166`, `app/ponto/turnos.py:2361`, `app/ponto/turnos.py:2986`, `app/ponto/turnos.py:3058`. |
| Precedência, núcleo e suprajuiz | `app/ponto/precedencia.py:312`, `app/ponto/precedencia.py:323`, `app/ponto/precedencia.py:349`, `app/ponto/supra_juiz.py:174`, `app/ponto/supra_juiz.py:185`, `app/ponto/nucleo.py:15`, `app/ponto/nucleo.py:26`. |
| Folha, feriados e CCT | `app/folha/previa.py:62`, `app/folha/previa.py:81`, `app/folha/export.py:359`, `app/folha/export.py:1252`, `app/core/feriados.py:154`, `app/core/feriados.py:161`, `app/core/regua_cct.py:329`. |
| Chamados e API | `app/chamados/services/limbo_folgas.py:200`, `app/chamados/management/commands/auditar_invariantes_chamados.py:361`, `app/chamados/management/commands/auditar_invariantes_chamados.py:391`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1538`, `app/api/views_mensageria.py:979`. |
| Triagem, cartório e vigias | `app/ponto/services/triagem_batida.py:149`, `app/ponto/services/triagem_batida.py:402`, `app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1903`, `app/ponto/services/vigia_cadastro_futuro.py:58`, `app/ponto/management/commands/vigiar_motor_autoridade.py:51`. |

Há ainda leituras somente do início e consultas somente de vínculos abertos; elas não demonstram uma precondição sobre a ordem das duas datas. (`app/ponto/services/espelho.py:43`, `app/ponto/management/commands/detectar_ausencias.py:177`, `app/ponto/management/commands/detectar_ausencias.py:247`, `app/ponto/management/commands/supra_juiz.py:149`, `app/ponto/management/commands/tripwire_celulas.py:67`, `app/chamados/detector_anomalias.py:58`, `app/escala/services/escala_auto.py:172`, `app/ponto/services/cluster_espurio.py:185`, `app/relatorios/furos.py:61`, `app/chamados/services/disputa_emissao.py:341`.)

Uma suposição adicional de que “fim sempre é maior ou igual ao início”, além dos predicados descritos, é **[nao verificado]**. O censo AST existente também declara limites de detecção; ele não prova ausência universal de acessos dinâmicos. (`app/core/censo_pergunta2.py:10`, `app/core/censo_pergunta2.py:17`.)

Para interpretar a sonda:

- `empN` usa `Colaborador.empresa_id`; a competência respeita o corte dessa empresa e recebe mês/ano pelo fim da janela. (`app/colaboradores/models.py:321`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:60`.)
- “Exportada” significa resposta verdadeira de `competencia_entregue`; “aberta”, neste relatório, significa resposta falsa desse juiz. (`app/folha/export.py:556`, `app/folha/export.py:588`, `app/folha/export.py:594`.)
- `desligado` vem de `esta_desligado`; `situacao` preserva também `afastado` e `ferias`. (`app/core/juizes.py:271`, `app/colaboradores/services/desligamento.py:23`, `app/colaboradores/models.py:295`.)
- `EscalaColaborador` não tem campo de criação/alteração; `regua_avaliada_em` e `regua_corroborada_em` pertencem à régua. (`app/escala/models.py:871`, `app/escala/models.py:884`, `app/escala/models.py:888`, `app/escala/models.py:1399`.)
- As batidas são contadas pelo instante local no intervalo completo, usando `batidas_apuraveis`; sua atribuição a um dia de turno é outra função. (`app/ponto/turnos.py:45`, `app/ponto/turnos.py:2875`, `app/ponto/turnos.py:2886`.)

Segue somente o texto. O envelope aceita `--sombra`, mas também escreve credencial, temporário e trilha; por isso não foi chamado nesta sessão. (`bin/sonda_leitura.sh:67`, `bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:248`.) Execução desta sonda no Django e resultados: **[nao verificado]**.

```python
# Texto para: bin/sonda_leitura.sh --sombra /caminho/sonda_o50.py
# Censo real: app/colaboradores/services/vinculo.py:479
# Cobertura: app/escala/alimentacao.py:205
# Juizes declarados: app/core/juizes.py:271 e :337

import json
import re
from collections import Counter, defaultdict
from contextlib import ExitStack
from datetime import datetime, time, timedelta

from django.conf import settings
from django.db import connections
from django.db.models import Count
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from colaboradores.services.desligamento import esta_desligado
from colaboradores.services.vinculo import vigencias_impossiveis
from escala.alimentacao import vinculo_cobre_a_data
from escala.models import CelulaDia, EscalaColaborador
from folha.export import competencia_entregue
from ponto.janelas import janela_atual
from ponto.turnos import batidas_apuraveis, data_local


def somente_select(execute, sql, params, many, context):
    # Precedente:
    # app/chamados/management/commands/diff_e3_sombra.py:30
    if many or not isinstance(sql, str):
        raise RuntimeError("SQL recusado")

    s = sql.strip()
    if not re.match(r"^SELECT\b", s, re.I):
        raise RuntimeError("SQL recusado")

    # Recusa comentarios, literais SQL e comandos multiplos.
    # Os valores destas consultas seguem nos parametros do ORM.
    if any(x in s for x in (";", "--", "/*", "*/", "'")):
        raise RuntimeError("SQL recusado")

    t = re.sub(r'"(?:[^"]|"")*"', "IDENT", s)
    if re.search(
        r"\b(WITH|INTO|INSERT|UPDATE|DELETE|MERGE|CREATE|ALTER|DROP|"
        r"TRUNCATE|COPY|CALL|DO|SET|RESET|BEGIN|COMMIT|ROLLBACK|" [nao verificado]
        r"SAVEPOINT|RELEASE|GRANT|REVOKE|FOR|LOCK)\b",
        t,
        re.I,
    ):
        raise RuntimeError("SQL recusado")

    # Recusa chamadas SQL fora do necessario para estas consultas.
    permitidos = {
        "COUNT", "IN", "AND", "OR", "NOT", "WHERE",
        "FROM", "SELECT", "EXISTS", "AS",
    }
    chamadas = re.findall(r"([A-Za-z_][A-Za-z_0-9$]*)\s*\(", t)
    if any(nome.upper() not in permitidos for nome in chamadas):
        raise RuntimeError("SQL recusado")

    return execute(sql, params, many, context)


def coletar():
    conn = connections["default"]
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or conn.settings_dict.get("NAME") != "sombra"
        or conn.settings_dict.get("HOST") != "sombra_db"
        or conn.settings_dict.get("USER") != "leitor"
        or getattr(conn, "schema_name", None) != "juliani"
        or not conn.get_autocommit()
    ):
        raise RuntimeError("Envelope recusado")

    # Sem limitar empresa, ativa, periodo ou quantidade esperada.
    # app/colaboradores/services/vinculo.py:491
    censo = vigencias_impossiveis()
    esperados = {
        pk: (cid, ini, fim, ativa)
        for pk, cid, ini, fim, ativa in censo
    }
    if len(esperados) != len(censo):
        raise RuntimeError("Censo duplicado")

    cids = {cid for _, cid, _, _, _ in censo}
    campos = (
        "pk", "colaborador_id", "data_inicio", "data_fim", "ativa",
    )
    por_colab = defaultdict(list)
    por_pk = {}

    for ec in (
        EscalaColaborador.objects.filter(colaborador_id__in=cids)
        .only(*campos)
        .order_by("pk")
    ):
        por_colab[ec.colaborador_id].append(ec)
        por_pk[ec.pk] = ec

    colabs = {
        c.pk: c
        for c in Colaborador.objects.filter(pk__in=cids).only(
            "pk", "empresa_id", "situacao"
        )
    }
    empresas = {
        e.pk: e
        for e in Empresa.objects.only("pk", "dia_inicio_competencia")
    }

    totais_emp = Counter({eid: 0 for eid in empresas})
    entregue = {}
    linhas = []
    um_dia = timedelta(days=1)
    tz = timezone.get_default_timezone()

    with timezone.override(tz):
        for pk in sorted(esperados):
            ec = por_pk[pk]
            if (
                ec.colaborador_id,
                ec.data_inicio,
                ec.data_fim,
                ec.ativa,
            ) != esperados[pk]:
                raise RuntimeError("Universo mudou durante a leitura")

            c = colabs[ec.colaborador_id]
            empresa = empresas[c.empresa_id]

            # Intervalo solicitado: [data_fim, data_inicio].
            lo, hi = ec.data_fim, ec.data_inicio
            grupos = {}

            def grupo(dia):
                # app/ponto/janelas.py:60
                ji, jf = janela_atual(dia, empresa)
                k = (jf.year, jf.month)
                tripla = (empresa.pk, jf.month, jf.year)

                # app/folha/export.py:556
                if tripla not in entregue:
                    entregue[tripla] = bool(
                        competencia_entregue(*tripla)
                    )

                if k not in grupos:
                    grupos[k] = {
                        "competencia": (
                            f"{jf.year:04d}-{jf.month:02d}"
                        ),
                        "janela": [ji.isoformat(), jf.isoformat()],
                        "entregue": entregue[tripla],
                        "estado": (
                            "exportada"
                            if entregue[tripla]
                            else "aberta"
                        ),
                        "recorte_intervalo": None,
                        "dias_intervalo": 0,
                        "celulas_intervalo": 0,
                        "celulas_depois_inicio": 0,
                        "celulas_intervalo_geradas_por_este": 0,
                        "celulas_depois_inicio_geradas_por_este": 0,
                        "batidas_intervalo": 0,
                        "_dias_celulas_intervalo": set(),
                        "_dias_celulas_depois_inicio": set(),
                    }

                return grupos[k], jf

            # Inclui competencias sem celulas ou batidas.
            d = lo
            while d <= hi:
                g, jf = grupo(d)
                ate = min(hi, jf)
                g["recorte_intervalo"] = [
                    d.isoformat(), ate.isoformat(),
                ]
                g["dias_intervalo"] += (ate - d).days + 1
                if ate == hi:
                    break
                d = ate + um_dia

            # Todas as celulas do colaborador:
            # [lo, hi] e, separadamente, data > hi, sem teto.
            # Proveniencia: app/escala/models.py:1692
            celulas = (
                CelulaDia.objects
                .filter(colaborador_id=c.pk, data__gte=lo)
                .order_by()
                .values("data", "escala_geradora_id")
                .annotate(n=Count("pk"))
            )
            for cel in celulas:
                dia, n = cel["data"], cel["n"]
                g, _ = grupo(dia)
                parte = (
                    "intervalo" if dia <= hi else "depois_inicio"
                )
                g["celulas_" + parte] += n
                g["_dias_celulas_" + parte].add(dia)
                if cel["escala_geradora_id"] == ec.pk:
                    g[
                        "celulas_" + parte + "_geradas_por_este"
                    ] += n

            # Inclui todas as horas do ultimo dia.
            # app/ponto/turnos.py:2875; data_local em :45
            ti = timezone.make_aware(
                datetime.combine(lo, time.min), tz
            )
            tf = timezone.make_aware(
                datetime.combine(hi, time.max), tz
            )
            for ts in batidas_apuraveis(c, ti, tf).values_list(
                "timestamp", flat=True
            ):
                g, _ = grupo(data_local(ts))
                g["batidas_intervalo"] += 1

            # Um outro vinculo individual cobrindo ambas as pontas.
            # app/escala/alimentacao.py:214
            outros = [
                f"#{outro.pk}"
           

[truncado em 20000 de 23309 caracteres]

## 15/11 00:47 · sessao 05c12876a823 · MADRUGADA-PREPARA · sonda · origem=Code

**A18/A19 da matriz são identificáveis; sua equivalência aos “itens 18–19 da E4B” não está demonstrada.** A árvore também usa “18–19” para o fim do programa E4B. O `fatias.md` fornecido descreve leitores; atribuir a projeção legada R/V a uma fatia executora da E4c permanece **[nao verificado]**. (`app/docs/BACKLOG.md:1977`; `app/docs/GRADE-MESTRA.md:717`)

A leitura abaixo usa **A18/A19** e identifica separadamente a operação real **R/V → DecisaoPergunta**. O comando E4B chama fábrica, registro de decisões e materialização; não contém uma aplicação automática de recusa humana A19. Exportadas são recusadas pelo planejamento e novamente antes da execução. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:260`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:304`)

| Operação | Modelo, campos e escritor na cópia |
|---|---|
| **A18: recusa técnica** | `PerguntaDisputa.recusa_motivo` recebe o motivo técnico truncado em 240 caracteres. Em colisão sem carimbo, delega também `materializada_em`, `materializada_por_id` e `via_resolucao=fato_ocupado`. Escritores: `app/chamados/services/materializacao.py:1096`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1108`; executor do carimbo: `app/chamados/models.py:3182`, `app/chamados/models.py:3268`, `app/chamados/models.py:3273`. |
| **A19: recusa humana** | `PerguntaDisputa.recusa_motivo` recebe autor e motivo; a reabertura limpa `respondida_em`, `resposta_colab`, `validada_em`, `validada_por`, `materializada_em`, `materializada_por` e `via_resolucao`. Escritores: `app/chamados/services/validacao.py:619`, `app/chamados/services/validacao.py:628`; operação delegada: `app/chamados/services/disputa_emissao.py:1595`, `app/chamados/services/disputa_emissao.py:1634`; executor: `app/chamados/models.py:3363`. |
| **A19: decisão preservada** | Insere `DecisaoPergunta` com `pergunta_id`, `ato=recusa`, `ato_texto`, `respondida_original_em`, `validada_original_em`, `validada_original_por_id`, `ato_em`, `ato_por`, `ato_motivo`. Produtor: `app/chamados/services/disputa_emissao.py:1617`; executor: `app/chamados/models.py:4096`. |
| **Projeção legada R/V** | Insere DP `resposta`: `pergunta_id`, `ato`, `ato_em=respondida_em`, `ato_texto=resposta_colab`, `ato_motivo`; DP `validacao`: os mesmos campos, com `ato_em=validada_em` e `ato_por_id=validada_por_id`. Produtor: `app/chamados/models.py:4074`, `app/chamados/models.py:4078`; executor: `app/chamados/models.py:4096`. Identificação como E4c: **[nao verificado]**. |
| **Notas e auditoria** | A18 e A19 podem inserir `FollowupChamado`: `chamado_id`, `autor_tipo`, `autor_user`, `conteudo`, `anexo_hash`, e chave de evento quando cabível. A19 também chama os adaptadores de `LogAuditoria`. (`app/chamados/services/materializacao.py:1113`; `app/chamados/services/validacao.py:633`; `app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:101`; `app/chamados/services/validacao.py:637`; `app/chamados/services/validacao.py:643`; `app/core/observ.py:134`) |
| **Eventos decorrentes** | DP registrada e reabertura chamam `acordar_celulas`; A18 com carimbo também pode chegar por signal. A escrita é `EventoCelula(celula_id, colaborador_id, data)`, com os defaults do modelo; localizar a célula apenas a lê. (`app/chamados/models.py:4097`; `app/chamados/services/disputa_emissao.py:1635`; `app/chamados/signals.py:257`; `app/chamados/models.py:3325`; `app/ponto/portas/celula.py:954`; `app/escala/models.py:1860`) |

Isso não fecha todas as cascatas: salvar pergunta agenda um callback de beco; quando elegível, ele resolve perguntas e fecha a disputa. O consumidor da outbox é outra execução. Linhas e efeitos efetivamente atingidos por essas cascatas: **[nao verificado]**. (`app/chamados/signals.py:230`; `app/chamados/signals.py:240`; `app/chamados/services/becos.py:54`; `app/chamados/services/becos.py:57`; `app/chamados/reconciliador.py:1316`)

**Nos leitores examinados do GRAVADO/TXT, não há leitura direta dos campos R/V/C/recusa, das DP, notas ou eventos acima.** A montagem seleciona `FechamentoMensal`, lê seus valores e consulta autoridades de ausência e retenção. Cobertura de todo despacho transitivo/dinâmico: **[nao verificado]**. (`app/folha/export.py:714`; `app/folha/export.py:719`; `app/folha/export.py:730`; `app/folha/export.py:740`; `app/folha/export.py:968`)

| Leitor | O que efetivamente lê |
|---|---|
| **Valores do TXT** | FM: `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, `horas_folga_trabalhada`, `horas_noturnas`, `horas_intra_indenizada`, `horas_atraso`, `horas_saida_antecipada`. (`app/folha/export.py:420`; `app/folha/export.py:439`) |
| **Entrada/retenção** | FM e cadastro; a retenção consulta o juiz da célula. (`app/folha/export.py:785`; `app/folha/export.py:831`; `app/folha/export.py:968`; `app/folha/export.py:1306`) |
| **Faltas e rubricas** | Datas de falta vêm da autoridade de ausência; o código da rubrica vem do mapa de rubricas. (`app/folha/export.py:169`; `app/folha/export.py:184`; `app/folha/export.py:735`; `app/folha/export.py:740`; `app/folha/export.py:984`) |
| **TXT já registrado** | `ExportacaoDominio` guarda `conteudo`, `hash_sha256` e `total_linhas`; `registrar` recebe o conteúdo pronto. (`app/folha/models.py:82`; `app/folha/models.py:174`; `app/folha/models.py:182`) |

Portanto, **ausência de escrita direta em horas não certifica dinheiro zero**. `fechamento_lido()` deriva o estado atual sem persistir; `txt_vigente_x_gravado()` compara o TXT vigente com a montagem atual. Nenhuma dessas funções fornece, por si, o estado posterior hipotético de A18/A19. Delta atribuível em minutos ou rubricas: **[nao verificado]**. (`app/ponto/services/fechamento.py:1759`; `app/folha/export.py:655`)

A sonda abaixo usa dois universos distintos, sem somar suas unidades:

- **E4B: dia-colaborador**, `sem_porta` de `_alcance`, mais o complemento declarado de classe e em `[hoje−45, hoje)`. Esse complemento não representa todo o histórico exportado. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:153`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:248`)
- **Projeção R/V: novas linhas DP**, pelo alcance real `linhas_a_criar()`: R ou V presente, horizonte canônico/sem dia ou conversa; conversa e sem dia ficam separados. O comando existente declara que não aplica essa projeção. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:73`)

A18/A19 não expõem alcance planejável em leitura; a sonda conta esses **dois escritores sem alcance chamável**, deixando suas quantidades de linhas e mudanças como `null`, nunca zero. A atribuição da projeção à E4c também fica explícita como não verificada. (`app/chamados/services/materializacao.py:1089`; `app/chamados/services/validacao.py:562`)

Texto para o envelope `bin/sonda_leitura.sh --sombra <sonda.py>`; execução e resultados: **[nao verificado]**. (`bin/sonda_leitura.sh:56`)

```python
# Apenas TEXTO: nao salvar nem executar nesta leitura.
# Envelope: bin/sonda_leitura.sh --sombra <sonda.py>
# RV_DP nomeia a operacao real; atribuicao a E4c nao verificada.

import io
import json
import logging
import re
from collections import Counter
from contextlib import ExitStack, redirect_stdout, redirect_stderr


class SQLRecusado(Exception):
    pass


def so_select(execute, sql, params, many, context):
    texto = sql.strip()
    if (
        many
        or not re.match(r"^SELECT\b", texto, re.I)
        or ";" in texto
        or re.search(
            r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
            texto, re.I,
        )
    ):
        raise SQLRecusado()
    return execute(sql, params, many, context)


def medir():
    from django.db import connections
    from django.utils import timezone

    saida = []
    with ExitStack() as pilha:
        for conn in connections.all():
            pilha.enter_context(conn.execute_wrapper(so_select))

        from folha.export import competencia_entregue
        from folha.models import ExportacaoDominio
        from colaboradores.models import Colaborador
        from chamados.management.commands.backfill_fio_morto_e4b import (
            _alcance, _exportadas_classe_e,
        )
        from chamados.management.commands.backfill_decisoes_pergunta import (
            linhas_a_criar,
        )

        agora = timezone.localtime()
        hoje = agora.date()

        # Enumerar triplas nao decide entrega: somente K6 decide.
        # folha/export.py:556,580
        triplas = set(
            ExportacaoDominio.objects.order_by()
            .values_list("empresa_id", "mes", "ano").distinct()
        )
        entregue = {}

        def k6(chave):
            if chave not in entregue:
                entregue[chave] = competencia_entregue(*chave) is True
            return entregue[chave]

        exportadas = {k for k in triplas if k6(k)}

        # E4B: alcance declarado, unidade dia-colaborador.
        # backfill_fio_morto_e4b.py:76,153,237
        _, base = _alcance(None, hoje, agora)
        extra = _exportadas_classe_e(None, hoje)
        dias = {(x[1], x[2]): x for x in base}
        for x in extra:
            dias.setdefault((x[1], x[2]), x)

        empresas = dict(
            Colaborador.objects.filter(
                pk__in={cid for cid, dia in dias}
            ).values_list("pk", "empresa_id")
        )
        e4b_exp = Counter()
        e4b_partes = Counter(
            exportada=0, nao_exportada=0, indeterminada=0,
        )
        for (cid, dia), x in dias.items():
            emp = empresas.get(cid)
            try:
                # Competencia produzida pelo alcance real.
                mes, ano = map(int, x[3].split("/"))
                chave = (emp, mes, ano)
            except (ValueError, TypeError, AttributeError):
                e4b_partes["indeterminada"] += 1
                continue

            if emp is None:
                e4b_partes["indeterminada"] += 1
            elif k6(chave):
                e4b_partes["exportada"] += 1
                e4b_exp[chave] += 1
                exportadas.add(chave)
            else:
                e4b_partes["nao_exportada"] += 1

        assert sum(e4b_partes.values()) == len(dias)
        assert sum(e4b_exp.values()) == e4b_partes["exportada"]

        # Projecao: alcance real, unidade nova linha DP.
        # backfill_decisoes_pergunta.py:44,67,73,79,82
        rv = linhas_a_criar()
        rv_partes = Counter(
            exportada=0, nao_exportada=0,
            conversa=0, sem_competencia=0,
        )
        rv_exp = {}
        total_rv = 0

        for chave, atos in rv.items():
            r = atos.get("resposta", [])
            v = atos.get("validacao", [])
            n = len(r) + len(v)
            assert n == sum(len(itens) for itens in atos.values())
            total_rv += n
            emp, mes, ano = chave

            if mes is None or ano is None or emp is None:
                balde = (
                    "conversa" if ano == "conversa"
                    else "sem_competencia"
                )
                rv_partes[balde] += n
            elif k6(chave):
                rv_partes["exportada"] += n
                exportadas.add(chave)
                rv_exp[chave] = dict(
                    resposta=len(r),
                    validacao=len(v),
                    total=n,
                    perguntas=len({pk for cid, pk in r + v}),
                )
            else:
                rv_partes["nao_exportada"] += n

        assert sum(rv_partes.values()) == total_rv
        assert (
            sum(x["total"] for x in rv_exp.values())
            == rv_partes["exportada"]
        )

        for emp, mes, ano in sorted(exportadas):
            desconhecido = dict(
                alcance="sem alcance chamavel",
                linhas=None,
                mudariam_campo_lido_gravado_TXT=None,
            )
            proj = rv_exp.get(
                (emp, mes, ano),
                dict(resposta=0, validacao=0, total=0, perguntas=0),
            )
            saida.append(dict(
                empresa="emp%d" % emp,
                competencia="%02d/%04d" % (mes, ano),
                E4B_dias_no_alcance=e4b_exp[(emp, mes, ano)],
                A18=desconhecido,
                A19=desconhecido,
                sem_alcance_chamavel=2,
                RV_DP=dict(
                    proj, mudariam_campo_lido_gravado_TXT=None,
                ),
                atribuicao_RV_DP_a_E4c="nao_verificado",
            ))

        saida.append(dict(
            TOTAL_E4B_dias=len(dias),
            partes=dict(e4b_partes),
        ))
        saida.append(dict(
            TOTAL_RV_DP=total_rv,
            partes=dict(rv_partes),
        ))
        saida.append(dict(
            TOTAL_exportadas=len(exportadas),
            escritores_distintos_sem_alcance_chamavel=2,
            pares_escritor_competencia_sem_alcance_chamavel=(
                2 * len(exportadas)
            ),
            delta_minutos_rubricas="nao_verificado",
        ))
    return saida


ok = False
nivel_anterior = logging.root.manager.disable
try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
        linhas = medir()
    for linha in linhas:
        print(json.dumps(linha, sort_keys=True, ensure_ascii=True))
    ok = True
except Exception:
    # Nao imprimir excecao, parametros SQL ou dados pessoais.
    print("ERRO leitura incompleta; resultados nao verificados")
finally:
    logging.disable(nivel_anterior)
    print("FIM completo=%d" % int(ok))
```

## 15/11 00:46 · sessao 81c7a97b4c43 · - · - · origem=Ronald (tmux)

Censo consolidado da pergunta: **17 funções persistentes do núcleo, 18 sítios**; nascimento e reuso pertencem à mesma `_nascer` (`app/chamados/models.py:3089`, `app/chamados/models.py:3099`). O mecanismo existente reconheceu 21 funções/21 sítios amplos; quatro gravam apenas campos auxiliares e o insert por `cls(...)` exigiu complemento manual (`app/core/censo_escritas.py:469`, `app/chamados/services/materializacao.py:857`, `app/chamados/services/materializacao.py:1109`, `app/chamados/services/validacao.py:625`, `app/chamados/services/validacao.py:701`).

As 17 funções persistentes:

| Função | Escrita |
|---|---|
| `PerguntaDisputa._nascer` | `app/chamados/models.py:3089`, `:3099` |
| `PerguntaDisputa.carimbar` | `app/chamados/models.py:3235` |
| `PerguntaDisputa.carimbar_identidade` | `app/chamados/models.py:3273` |
| `PerguntaDisputa.descarimbar` | `app/chamados/models.py:3295` |
| `PerguntaDisputa.descarimbar_em_lote` | `app/chamados/models.py:3363` |
| `PerguntaDisputa.encerrar_por_fato_no_espelho` | `app/chamados/models.py:3375` |
| `DecisaoPergunta.registrar` | `app/chamados/models.py:4096` |
| `gravar_lavras_em_lote` | `app/chamados/veredito_lavrado.py:250` |
| `lavrar` | `app/chamados/veredito_lavrado.py:275` |
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1508` |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1662` |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1683` |
| `reapontar_resolvedora` | `app/chamados/services/fio.py:966` |
| `rechavear_data_evento` | `app/chamados/services/fio.py:1082` |
| `relavrar` | `app/chamados/services/relavra_via.py:127` |
| `_carimbar_validacao` | `app/chamados/services/validacao.py:162` |
| `devolver_validacao_recusada` | `app/chamados/services/validacao.py:227` |

Categorias sobrepostas: destino tem **2 funções/3 sítios**; ciclo resposta/validação/carimbo/via tem **12 funções**, incluindo nascimento; lavra tem duas portas próprias, além dos saves que persistem a lavra calculada com `salvar=False`; trilha tem uma porta (`app/chamados/models.py:3065`, `app/chamados/models.py:3095`, `app/chamados/services/fio.py:964`, `app/chamados/veredito_lavrado.py:240`, `app/chamados/veredito_lavrado.py:254`, `app/chamados/models.py:4086`).

Delta decisivo E3: correção agora **sempre** limpa resposta, validação e carimbo pela porta do lote; não toca linha fora do fato nem registra falsa reabertura para ela; `desfazer_absorcao` foi removido (`app/chamados/services/disputa_emissao.py:1586`, `:1595`, `:1631`, `:1634`). Recusa humana ganhou veto explícito a pergunta fora do fato (`app/chamados/services/validacao.py:609`). O par `respondida_em` preenchido/texto vazio ganhou guarda no nascimento, save, lote e resposta automática (`app/chamados/models.py:2917`, `:3529`, `:3343`; `app/chamados/services/disputa_emissao.py:1657`).

Identidade: `_chave_de_identidade` deriva colaborador, instância e episódio apenas no save de inserção; migração histórica I1 é separada do universo vivo (`app/chamados/models.py:3530`, `:3568`, `:3575`, `:3583`, `:3592`; `app/chamados/migrations/0051_decisao_pergunta.py:22`, `:25`, `:27`).

Leitores abreviáveis: `estado`, C6, app, validação, materializador, histórico e stale da lavra (`app/chamados/models.py:3117`; `app/chamados/juizes.py:1399`; `app/chamados/services/pergunta_no_app.py:45`, `:76`; `app/chamados/services/validacao.py:418`; `app/chamados/services/materializacao.py:1344`; `app/chamados/models.py:3643`; `app/chamados/veredito_lavrado.py:224`).

## 15/11 00:46 · sessao fb6bf50f898f · - · - · origem=Ronald (tmux)

Na cópia atual, a raiz mantém **2 estados**: aberta (`fechada_em=None`) e fechada (`fechada_em!=None`); o motivo do fechamento aceita `veredito`, `vazia`, `desligamento`, `admin_encerra` ou `competencia_trancada`, enquanto vazio representa aberta ou legado. (`app/chamados/models.py:1804`, `:1827`, `:1840`, `:1870`)

**Matriz da raiz**

| Transição | Escritor real | Guarda | Leitores |
|---|---|---|---|
| Inexistente → aberta, núcleo | `app/chamados/services/disputa_emissao.py::_abrir_ou_agregar_disputa:587` | Trava universo do colaborador; procura aberta; cria somente na ausência. (`:530`, `:531`, `:535`, `:587`) | `colaborador_em_disputa` (`mesmo arquivo:34`); `particao_do_app` (`app/chamados/services/pergunta_no_app.py:76`) |
| Inexistente → aberta, repasse | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1123` | Exclui dia na lista admin; regularizável; chamado sem pergunta não materializada, salvo caminho da ata; ausência de disputa aberta. (`:1047`, `:1086`, `:1096`, `:1103`) | Mesmos leitores anteriores. (`:34`; `app/chamados/services/pergunta_no_app.py:81`) |
| Inexistente → aberta, âncora | `app/chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1358` | Categoria `ancora_limbo`; chamado não encerrado; nenhuma pergunta de âncora não materializada; ausência de disputa aberta. (`:1316`, `:1318`, `:1323`, `:1332`, `:1338`) | Mesmos leitores anteriores. (`:34`; `app/chamados/services/pergunta_no_app.py:81`) |
| Aberta A → aberta A∪B | Núcleo `:565`; repasse `:1132`, `:1161`, `:1256`; âncora `:1367`, todos em `app/chamados/services/disputa_emissao.py` | Núcleo compara conjuntos e só salva campos alterados; repasse/âncora atualizam motivos nos seus ramos. (`:537`, `:540`, `:564`, `:1131`, `:1160`, `:1255`, `:1366`) | Gerador de perguntas (`mesmo arquivo:300`); classificação do relatório (`app/chamados/relatorios_disputa.py:11`) |
| Fechada A → fechada A∪B, complemento | `app/chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos:1084` | Pergunta do destino sem materialização, sem filtro de dona aberta; dia identificado; ata disponível; complemento criou pergunta. (`:1070`, `:1074`, `:1077`, `:1081`, `:1082`) | Pergunta viva em dona fechada entra no histórico do app (`app/chamados/services/pergunta_no_app.py:81`); medida pronta em `vigia_fio.viva_em_disputa_fechada` (`app/chamados/services/vigia_fio.py:758`) |
| Aberta → fechada `vazia`/`veredito` | `app/chamados/models.py::DisputaSupervisao.fechar:1996`, persistência `:2000` | Autor; ausência de resposta sem veredito; nenhuma pergunta viva; zero perguntas escolhe `vazia`, demais escolhem `veredito`. (`:1948`, `:1950`, `:1960`, `:1962`, `:1969`) | Signal do pai (`app/chamados/signals.py:29`); app (`app/chamados/services/pergunta_no_app.py:81`); contador de vazias (`app/chamados/services/vigia_fio.py:748`) |
| Aberta → fechada `desligamento` | Mesmo `fechar:1996`; chamada `app/colaboradores/services/desligamento.py:167` | Exceção aceita; autor; ainda recusa resposta sem veredito; carimba perguntas vivas por decisão administrativa e registra silêncio antes do fechamento. (`app/chamados/models.py:1940`, `:1948`, `:1950`, `:1982`, `:1988`) | Signal e app. (`app/chamados/signals.py:29`; `app/chamados/services/pergunta_no_app.py:81`) |
| Aberta → fechada `admin_encerra` | `app/chamados/models.py::DisputaSupervisao.admin_encerra:2014` → `fechar:1996` | Motivo escrito não vazio; demais guardas anteriores. (`:2011`, `:2012`, `:1950`, `:1982`) | Signal e app. (`app/chamados/signals.py:29`; `app/chamados/services/pergunta_no_app.py:81`) |
| Aberta → fechada `competencia_trancada` | Mesmo `fechar:1996`; chamada `app/chamados/services/competencia_trancada.py::encerrar:229` | Chamador pede `pode_fechar`; porta exige autor e ausência de respostas sem veredito; não entra no ramo que carimba mudas. (`app/chamados/services/competencia_trancada.py:227`; `app/chamados/models.py:1948`, `:1950`, `:1982`) | Signal e app. (`app/chamados/signals.py:29`; `app/chamados/services/pergunta_no_app.py:81`) |
| Fechada → aberta, admin | `app/chamados/models.py::DisputaSupervisao.reabrir_admin:2213`, save `:2225` | Fechada; contador <2; autor. Casca exige staff/grupo, motivo válido e justificativa ≥20 caracteres. (`:2202`, `:2204`, `:2206`; `app/chamados/services/acoes_disputa.py:245`, `:251`, `:253`, `:255`) | App; procura de aberta pelo emissor. (`app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/disputa_emissao.py:531`) |
| Fechada → aberta, sistema | `app/chamados/models.py::DisputaSupervisao.reabrir_sistema:2237`, save `:2241` | Somente fechada; aberta retorna `False`; não incrementa contador; anexa nota se há chamado. (`:2231`, `:2242`, `:2244`) | App, helper/motor. (`app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/disputa_emissao.py:34`; `app/ponto/services/disputa.py:31`) |
| Aberta/questionário corrigível → aberta/questionário pendente | `app/chamados/models.py::DisputaSupervisao.reabrir_por_colab:2301`, contador `:2303`, save `:2304` | Autor; dona aberta; contador <2; `perguntas_corrigiveis` não vazio. (`:2282`, `:2284`, `:2288`, `:2290`, `:2292`) | App e relatório. (`app/chamados/services/pergunta_no_app.py:81`; `app/chamados/relatorios_disputa.py:24`) |
| Aberta A → aberta A−motivo; último motivo encaminha fechamento | `app/chamados/models.py::DisputaSupervisao.remover_motivo:2127`, save `:2129`, eventual `fechar:2136` | Autor; relê sob trava; aberta; motivo presente. Retrai perguntas preservando trilha/identidade; se nenhum motivo resta, pede fechamento normal, que pode recusar. (`:2079`, `:2090`, `:2095`, `:2097`, `:2100`, `:2120`, `:2125`, `:2130`, `:2136`) | Gerador e relatório. (`app/chamados/services/disputa_emissao.py:300`; `app/chamados/relatorios_disputa.py:11`) |
| Motivos com foto ausente → motivos sem foto ausente | `app/chamados/models.py::DisputaSupervisao.aparar_motivo_respondido:2032` | Motivo precisa existir; remove motivo/contexto, sem fechar por conta própria. (`:2025`, `:2029`, `:2032`; chamada `app/api/foto_recebida.py:72`) | Fechamento separado do chamador e relatório. (`app/api/foto_recebida.py:80`; `app/chamados/relatorios_disputa.py:11`) |
| Aberta com resolução residual → aberta com resolução vazia | `app/chamados/models.py::DisputaSupervisao.limpar_resolucao_residual:2329`, save `:2330` | Aberta e texto não vazio; nota se há chamado. (`:2326`, `:2331`, `:2333`) | Signal lê resolução nos próximos fechamentos; app lê estado de abertura. (`app/chamados/signals.py:46`; `app/chamados/services/pergunta_no_app.py:81`) |
| Contexto sem chave → contexto com chave, preserva estado | `app/chamados/models.py::DisputaSupervisao.anotar_contexto:2169` | Só se chave ainda não tem valor. (`:2164`, `:2165`, `:2167`) | Os consumidores da chave dependem do chamador; inventário completo por chave [nao verificado]. |
| Contexto anterior → contexto novo, preserva estado | `app/chamados/models.py::DisputaSupervisao.renovar_contexto:2187` | Só se valor diferente. (`:2182`, `:2183`, `:2185`) | Os consumidores da chave dependem do chamador; inventário completo por chave [nao verificado]. |
| Contexto da disputa → contexto com dia de pergunta de entrada pendente | `app/chamados/services/disputa_emissao.py::perguntar_entradas_pendentes:296` | Classificação do dia devolveu motivo; só salva contexto se criou pergunta. (`:288`, `:289`, `:293`, `:295`) | `gerar_perguntas_disputa` chamado na própria operação. (`:294`) |

**Censo executado sem importar a aplicação**

Executei apenas em memória os helpers AST do selo: 765 arquivos de produção; zero violações no selo `status_local/fechada_em`; para a raiz, três funções persistem `fechada_em`: `fechar`, `reabrir_admin`, `reabrir_sistema`. A fronteira/exclusões e o mecanismo estão em `app/chamados/tests/test_selo_status_so_pelas_portas.py:15`, `:319`, `:332`, `:340`; os seis achados são atributo/save em `app/chamados/models.py:1996`, `:2000`, `:2213`, `:2225`, `:2237`, `:2241`.

Ampliando a leitura do mesmo helper aos campos `fechada_em`, `fechada_por[_id]`, `resolucao`, `motivo_fechamento`, as únicas funções persistentes são essas três e `limpar_resolucao_residual`: **4 funções**. Os dois splats sinalizados adicionalmente pertencem ao outro modelo, `ChamadoColaborador.abrir/criar`, nos seus próprios gerentes `cls.objects`; não são gravações da raiz. (`app/chamados/models.py:1018`, `:1142`, `:1996`, `:2213`, `:2237`, `:2329`)

O censo geral puro da cópia identificou **19 sítios persistentes em 13 funções da raiz**: nove saves em métodos da raiz; dez sítios no emissor distribuídos entre `perguntar_entradas_pendentes`, núcleo, repasse e âncora. Seu mecanismo atribui import tardio antes da varredura e distingue construção/save/gerente relacionado. (`app/core/censo_escritas.py:3`, `:6`, `:451`, `:455`, `:469`; sítios: `app/chamados/models.py:2000`, `:2032`, `:2129`, `:2169`, `:2187`, `:2225`, `:2241`, `:2304`, `:2330`; `app/chamados/services/disputa_emissao.py:296`, `:565`, `:587`, `:1084`, `:1123`, `:1132`, `:1161`, `:1256`, `:1358`, `:1367`)

Esse censo geral contém **131 operações não atribuíveis na árvore inteira**; esse total não pode fundamentar exaustividade universal por modelo sem classificação adicional. A limitação é parte do mecanismo. (`app/core/censo_escritas.py:43`, `:469`) O selo específico também declara limites para receptor/tipo genérico, nome dinâmico de campo e splat não resolvido; portanto despacho dinâmico arbitrário permanece [nao verificado]. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `:291`)

A varredura AST das chamadas encontrou **15 chamadas diretas de fechamento da raiz**, mais um homônimo `ChamadoColaborador.fechar`; cinco de reabertura sistema; uma admin; duas colaborador; duas remoção de motivo; uma aparagem; nenhuma chamada produtiva de `admin_encerra` ou limpeza residual. (`app/chamados/reconciliador.py:413`, `:613`, `:1172`; `app/chamados/services/materializacao.py:592`, `:1339`; `app/chamados/services/disputa_emissao.py:1284`, `:2099`; `app/chamados/models.py:2014`, `:2136`; `app/chamados/services/becos.py:57`; `app/chamados/services/competencia_trancada.py:229`; `app/colaboradores/services/desligamento.py:167`; `app/ponto/services/entrada_sincronizada.py:60`; `app/api/foto_recebida.py:80`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`; homônimo `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`; demais chamadas explicitadas na matriz e abaixo)

Os cinco chamadores de sistema continuam: ressuscitação, validação recusada, ausência rejeitada, comando de órfãs e carimbo sem lastro. (`app/chamados/models.py:1622`; `app/chamados/services/validacao.py:243`; `app/chamados/services/materializacao.py:692`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`; `app/chamados/services/relavra_via.py:261`)

A leitura AST considerou import local e apelido: `DisputaSupervisao as _DS` em `ChamadoColaborador.transicionar`, e `_DSup_S93` em `meu_atendimento`; não apareceu referência de porta armazenada como bound method. (`app/chamados/models.py:189`; `app/chamados/views.py:620`; resultado da varredura em memória: [nao verificado por artefato persistido])

Não apareceu referência a esses tokens da raiz em `__init__.py`; o reexport relevante está no final de `models.py`, incluindo `colaborador_em_disputa` e funções do emissor. (`app/chamados/models.py:4235`, `:4236`; ausência em `__init__.py`: [nao verificado por artefato persistido])

As migrações que definem o estado são esquema/defaults: criação original e adição do motivo; gravação de registros da raiz em `RunPython` não apareceu na pesquisa desses campos. (`app/chamados/migrations/0013_disputasupervisao.py:24`, `:25`, `:28`; `app/chamados/migrations/0049_disputa_motivo_fechamento.py:14`; ausência operacional: [nao verificado por artefato persistido])

**Deltas materiais úteis**

- O fechamento/portas da raiz estão deslocados em relação ao anexo: escrita do fechamento `1996`, administrativa `2213`, sistema `2237`, residual `2329`. (`app/chamados/models.py:1996`, `:2213`, `:2237`, `:2329`)
- `pode_fechar` possui **14 chamadas diretas atuais**, incluindo relatório e explicador que agora leem o juiz completo. (`app/chamados/relatorios_disputa.py:24`; `app/chamados/services/explicador.py:139`; demais: `app/chamados/reconciliador.py:412`, `:612`, `:1171`; `app/ponto/services/entrada_sincronizada.py:59`; `app/chamados/services/materializacao.py:591`, `:1322`; `app/chamados/services/disputa_emissao.py:1278`, `:2098`; `app/chamados/services/competencia_trancada.py:178`, `:227`; `app/chamados/services/becos.py:33`, `:56`)
- O predicado de resposta sem veredito explicitamente mantém resposta carimbada sem texto: `respondida_em != None OU texto`, desde que não validada/materializada e não trancada. (`app/chamados/models.py:1899`, `:1905`)
- Para medir o caminho “pergunta viva em dona fechada” já existe a função real `vigia_fio.viva_em_disputa_fechada(empresas=None)`, que retorna perguntas e disputas distintas, com `q_vivas()` e julgamento final pelo mesmo `pergunta_viva`. (`app/chamados/services/vigia_fio.py:758`, `:769`, `:775`, `:780`)
- Para medir nascimentos sem perguntas encerrados, a função real é `vigia_fio.disputa_vazia()`, que conta motivo persistido `vazia`. (`app/chamados/services/vigia_fio.py:748`, `:755`)
- Não há número atual desses dois indicadores no envelope fornecido. [nao verificado]

## 15/11 00:45 · sessao 0c16e9d4443f · - · - · origem=Ronald (tmux)

Mapa consolidado; nenhum símbolo citado desapareceu nos arquivos examinados. Não há comparação histórica Git nesta cópia; estados abaixo conferem os sítios e significados descritos pelos anexos contra o código atual. [nao verificado quanto ao histórico]

**MUDOU — `app/chamados/models.py`; distinguir as duas bases dos anexos.**

| Citação antiga | Sítio atual | Conferência |
|---|---|---|
| r.r `:2437` | `:2514–2519` | Episódio BATIDA em coluna; escrito no nascimento e protegido contra apagar batida. |
| r.r `:2784` | `:2861–2864` | `data_evento` da pergunta datada é dia do turno; conversa tem porta própria. |
| r.r `:2795`, `:2802` | `:2872–2880` | `abrir_conversa`: dia civil do pedido, atribuído por `localdate()` em `:2879`. |
| matriz `:2850–2859` | `:2872–2880` | Mesmo nascimento de conversa; `conversa=True`, sem derivar episódio. |
| r.r `:2867` | `:2949–2951` | `_nascer` deriva/recebe episódio antes do juiz do dia. |
| r.r `:2876` | `:2958–2961` | Dia recebido de `fio.dia_do_turno_da_linha`, com episódio e `pedido=data_evento`. |
| matriz `:2965–2976` | `:2992–3003` | `FatoEmOutroDia` trata episódio/fecho recusado noutro dia; **não** recusa genericamente uma data pedida divergente. O nascimento grava o dia do juiz (`:2938–2942`). |
| matriz `:2982` | `:3009` | Recusa episódio retratado antes de reusar a linha eleita. |
| r.r `:3450` | `:3591–3592` | `_chave_de_identidade` deriva origem se pergunta não é conversa e origem está ausente. |
| r.r `:3453` | `:3595` | Definição de `batida_origem_do_nascimento`. |
| cef/r.r `:3457` | `:3599–3601` | Contexto fundido pode apontar outro episódio; acervo não ganha origem por essa derivação posterior. |
| cef `:3462–3466`; r.r `:3464` | `:3603–3608` | Família BATIDA e leitura de `contexto.ultima_batida.id`, exatamente em `:3606`. |
| r.r `:3468` | `:3610` | PK só retorna se batida pertence ao colaborador da disputa. |
| cef/r.r `:3490` | `:3631–3636` | Índice por `(batida_origem, motivo)`, parcial, com horizonte, sem conversa e fora das vias retiradas do fato. |
| matriz `:3508–3523` | `:3595–3610` | Derivador do episódio: família, envelope, existência e colaborador. |
| matriz `:3537–3550` | `:3624–3636` | Índices por família; BATIDA exige origem não nula e dia `>= HORIZONTE_DO_FATO`. |
| r.r `:3604` | `:3746` | Literal `foto_ausente` do catálogo legado de perguntas; não é envelope. |
| r.r `:4094` | `:4235–4236` | Reexportação da porta e do gerador de `services.disputa_emissao`. |

**Mudança relevante ao nascimento com E3:** há recusa de `respondida_em` sem texto antes das travas em `app/chamados/models.py:2919–2923`; `save` também chama a guarda em `:3529`, com regra documentada em `:3536–3540`. O nascimento persiste o dia via escritor único em `:3096–3100` e registra projeções de decisão em `:3101–3102`.

**SEGUE — demais referências examinadas.**

| Arquivo e referências citadas | Conferência atual |
|---|---|
| `app/chamados/juizes.py:138`, `:141–168`, `:151`, `:159–166`, `:226` | Mesmos locais; carimbos fracos são último degrau. |
| `app/api/foto_recebida.py:28–54`, `:51`, `:57–85`, `:61–69`, `:67–69`, `:70–73`, `:73`, `:79–83` | Mesmos locais; seleciona disputa pela última batida, valida conjunto de perguntas e apara/fecha disputa. |
| `app/ponto/turnos.py:2927–2936`, `:2928`, `:2935`, `:2955` | Mesmo adaptador do juiz; batida não reclamada usa data local, retratada fica fora do universo. |
| `app/chamados/catalogo/motivos.py:16`, `:35`, `:43`, `:53` | Mesmos locais; identidade BATIDA por episódio, triagem MARCO e família no catálogo. |
| `app/chamados/catalogo/query.py:45–66` | Mesmo texto interpolado; template sem `{data}` retorna texto em `:56–57`. |
| `app/chamados/catalogo/perguntas.py:93–98` | Mesmo texto de `foto_ausente`, tipo escolha, sem data interpolada. |
| `app/chamados/tests/test_e1_v5_familia.py:25`, `:98` | Mesmo selo: AST da declaração `Motivo(familia=...)` e chave dos índices; não confere envelope dos emissores. |
| `app/chamados/tests/test_e1_v5_indice.py:69` | `CorridaCruaNoIndiceTest::test_MORDE_batida_duas_cruas_no_mesmo_episodio_dao_UMA_linha`, mesmo teste. |
| `app/chamados/tests/test_e1_v5_indice.py:84` | `IndiceNaoSobrepoeTest::test_episodios_diferentes_no_mesmo_dia_sao_duas_linhas`, mesmo teste. |
| `app/chamados/tests/test_e1_v5_indice.py:96` | `IndiceNaoSobrepoeTest::test_conversa_e_a_linha_fora_do_fato_nao_entram`, mesmo teste. |
| `app/chamados/tests/test_a2_data_evento_batida.py:22` | `DataEventoBatidaTest::test_pergunta_de_batida_nasce_datada_e_dedupa`; duas origens distintas e reemissão da mesma em `:34–44`. |
| `app/chamados/tests/test_a2_data_evento_batida.py:53` | `DataEventoBatidaTest::test_contexto_sem_ultima_batida_nasce_conversa`; confere data civil em `:56`. |
| `app/chamados/tests/test_e1_v6_horizonte.py:262` | `EmissorBatidaPorEpisodioTest::test_RED_segunda_batida_na_disputa_aberta_ganha_pergunta`; origens em `:272–273`, repetição em `:275–276`. |
| `app/chamados/tests/test_cenarios_disputa.py:43` | `HelperTextoPerguntaTest::test_cenario_11_foto_ausente_sem_data`; texto sem data em `:44–47`. |
| `app/chamados/tests/test_b8_smoke_portas.py:60` | `test_upload_foto_carimba_batida_e_fotobatida`; verifica Batida.foto e FotoBatida em `:68–69`, não fechamento de disputa. |
| `app/chamados/tests/test_porta_chamado_e3.py:58`, `:62` | Mesmo teste da porta `aparar_motivo_respondido('foto_ausente')`, repetição idempotente; não é teste do upload. |
| `app/docs/JUIZES.md:14`, `:20`, `:38` | Mesmo lookup: vivo prevalece, geometria do turno e dono de validação. |

**Duas referências ao registro vivo MUDARAM:** `app/core/juizes.py:208` do r.r, que pretendia registrar `turnos_do_colab`, corresponde agora a `app/core/juizes.py:212`; a entrada de `selfie_ausente` antes citada em `:1199` fica em `app/core/juizes.py:1205`.

**Correção necessária da matriz D1:** o juiz fraco lê `ChamadoColaborador.contexto_json`, não diretamente `DisputaSupervisao.contexto`: leitura singular em `app/chamados/juizes.py:100`, chamada em `:138`; lote em `:226`. A ponte no nascimento do chamado existe em `app/chamados/services/disputa_emissao.py:580`; a disputa recebe o contexto separadamente em `:590`. A agregação de disputa escreve seu contexto em `:562–565` e não atualiza o contexto do chamado nesse ramo (`:535–566`).

**Semântica temporal:** o campo da API contém recepção: `agora=timezone.now()` em `app/api/views.py:497`, serializado em `:761`. O leitor fraco extrai `.date()` do ISO **sem converter fuso** em `app/chamados/juizes.py:166`; portanto obtém o dia civil do offset serializado. O retro serializa o instante da batida convertido para local em `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`. Esse dia civil, o instante e o dia do turno têm contratos distintos (`app/ponto/turnos.py:2928–2936`).

**Escritores/leitores adicionais indispensáveis ao CENSO:**

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `batida_origem_id` | `app/chamados/models.py:2949–2951`; `:3591–3592`; persistência do nascimento em `:3100` | `models.py:2795–2804` dedup por episódio/acervo do dia; `fio.py:1295` chave do episódio; `:1526–1533` pergunta o dia ao juiz; `:1554–1570` verifica episódio sem dia; `:1685–1687` dia/recusa; `:1846` universo do fato; `models.py:3631–3636` índice. |
| `data_evento` | Atribuição única `app/chamados/services/fio.py:839`; nascimento chama em `models.py:3099`; conversa passa data civil em `models.py:2879`; portas de mudança declaradas em `fio.py:835–837` | `fio.py:1660–1702` resolve dia do fato; `fio.py:1178–1187` contrato de saída; `models.py:2798–2804` chave para localizar linhas; `juizes.py:76–83` exclui conversa e lê dias dos fatos. |
| `colaborador_id` da pergunta | `app/chamados/models.py:3573–3578` deriva da disputa e recusa divergência | `models.py:3610` confere posse da batida; `fio.py:1527` chave do cache leva colaborador e episódio; `fio.py:1705–1714` lê dono da linha. |
| `instancia_marco` / `conversa` | `app/chamados/models.py:3579–3589` deriva instância; portas datada/conversa em `:2867` / `:2879`; nascimento em `:3097` | `fio.py:1669–1677` separa conversa/dia civil, episódio/dia do turno e MARCO/célula; `models.py:3625–3636` condições dos índices. |
| Resposta automática / validação no upload | `app/api/foto_recebida.py:48` chama porta da resposta; `:51` chama dono da validação | Escopo de seleção é disputa em `:67–69`, todas as perguntas de foto em `:73` ou todas da disputa em `:79`; fechamento em `:80`. |

Para B7, há teste do ramo de episódio retratado com `gps_ausente`, em `app/chamados/tests/test_e1_v6_horizonte.py:445–454`; cobertura específica de `foto_ausente` continua [nao verificado]. O gerador pode fazer conversa quando não obtém dia (`app/chamados/services/disputa_emissao.py:431–441`); a porta datada recusa origem retratada pelo juiz (`app/chamados/services/fio.py:1685–1687`).

Execução dos testes e comportamento de produção: [nao verificado]. [nao verificado]

## 15/11 00:45 · sessao 4ede009033c2 · - · - · origem=Ronald (tmux)

Auditoria estática dos emissores e da porta concluída; nenhuma alteração realizada, nenhuma aplicação importada, nenhum teste executado **[nao verificado em arquivo — registro desta execução]**. [nao verificado]

### Mapa das citações sob minha responsabilidade

Todas as referências abaixo **SEGUEM**, no mesmo arquivo, linha e significado:

| Arquivo | Citações conferidas | Significado |
|---|---|---|
| `app/ponto/management/commands/marcar_foto_ausente_retro.py` | `:45`, `:65`, `:66-69`, `:70-72`, `:74-77`, `:81`, `:89-92`, `:95-106`, `:96`, `:98-102` | Janela de três durações de 24h por padrão; bordas inclusivas; filtro de origem/retração; foto por ambos os registros; juiz compartilhado; dry-run; emissor entrega somente chave do motivo, sem `ultima_batida`. |
| `app/ponto/registro_batida.py` | `:72`, `:75-77`, `:80-91`, `:83`, `:89`, `:91` | PWA=0, APK/NULL=90; override só APK/NULL; foto por OR; vencimento `>=`. A origem `('app',)` está em `:71`. |
| `app/config/crons.py` | `:686` | Cron `26 7 * * *`, comando retro com `--apply`. A referência da matriz é relativa a `app/`, como declarado no próprio anexo. |
| `app/api/views.py` | `:447-453`, `:453`, `:507`, `:655`, `:758-766`, `:759`, `:760`, `:761` | Juiz no ato; instante efetivo da batida separado de `agora`; PK/tipo/recepção no envelope; chamada da porta. |
| `app/api/views.py` | `:833-859` | **SEGUE** o intervalo da matriz: decorators `:833-834`, definição `api_upload_foto :835`, anexo da foto `:847`, resolução da disputa `:857`, resposta `:859`. Não há prova de mudança só porque a definição está em `:835`: o anexo citou um intervalo. |
| `app/api/views_core.py` | `:439`, `:752`, `:753`, `:754`, `:755`, `:758` | API gêmea com o mesmo envelope e mesma separação entre instante efetivo e recepção. |
| `app/ponto/services/triagem_batida.py` | `:735`, `:739` | Emissor de `saida_sem_entrada`, com dia do juiz em `:740` e hora civil da tentativa em `:741`; delegação de dia em `:729-730`. |
| `app/chamados/services/fabrica_da_celula.py` | `:71`, `:139`, `:172`, `:174` | Motivos por marcos da ata; envelope `{motivo: {'data': dia.isoformat()}}`; porta e gerador. |
| `app/chamados/management/commands/smoke_chamados.py` | `:103` | Chamada direta do gerador. |
| `app/chamados/services/disputa_emissao.py` | `:72`, `:294`, `:300`, `:325-329`, `:326`, `:371-377`, `:372`, `:387-392`, `:436-441`, `:436-438`, `:470-471` | Leitura do episódio para dia; gerador; dedup sem episódio; data de motivo antes da batida; conversa quando falta dia; retorno das perguntas. |
| Mesmo arquivo | `:535-566`, `:548`, `:552-554`, `:556`, `:557`, `:558`, `:560-561`, `:562`, `:564-565` | Agregação; preservação de chaves antigas ausentes no payload; merge dos motivos; contador por emissão; recepção do merge; substituição das demais chaves de topo; persistência. |
| Mesmo arquivo | `:568-603`, `:568`, `:587-592`, `:590`, `:597`, `:634-688`, `:634`, `:659`, `:664`, `:670`, `:682` | Nascimento do chamado/disputa, trilha de abertura, porta pública, normalização de motivos e duas chamadas ao gerador. |

**Nenhum SUMIU encontrado neste conjunto.** As linhas do modelo que encontrei mudaram, como já encaminhado: chamadas `2867→2949`, `3450→3592`; definição `3453→3595`; leitor `3464→3606` (`app/chamados/models.py:2949`; `app/chamados/models.py:3592`; `app/chamados/models.py:3595`; `app/chamados/models.py:3606`).

### Funções, escritores e leitores

| Campo/contrato | Escritores atuais | Leitores atuais / significado |
|---|---|---|
| `ultima_batida.id` | API principal `app/api/views.py:759`; gêmea `app/api/views_core.py:753`; persistência genérica no nascimento `app/chamados/services/disputa_emissao.py:590` e agregado `:561-565`. | Episódio do colaborador no derivador `app/chamados/models.py:3606`; episódio para dia do turno no gerador `app/chamados/services/disputa_emissao.py:387-392`; referência para grade `:72-76`; seleção da disputa no upload `app/api/foto_recebida.py:68`. |
| `ultima_batida.tipo` | `app/api/views.py:760`; `app/api/views_core.py:754`. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `ultima_batida.timestamp` | `app/api/views.py:761`; `app/api/views_core.py:755`. É `agora`, instante da recepção, proveniente de `app/api/views.py:497` / `app/api/views_core.py:483`. | `_dia_do_contexto_fraco` lê em `app/chamados/juizes.py:160` e extrai `.date()` em `:166`: dia civil do timestamp recebido, sem converter esse instante ao juiz do turno. |
| `foto_ausente.tipo_batida` | `app/api/views.py:453`; `app/api/views_core.py:439`; retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:99`. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `foto_ausente.batida_id` | Retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:100`: PK da batida. | Nenhum leitor específico dessa chave aninhada localizado **[nao verificado quanto a leitura dinâmica/genérica]**. Os consumidores de episódio leem `ultima_batida.id` (`app/chamados/models.py:3606`; `app/chamados/services/disputa_emissao.py:387`). |
| `foto_ausente.batida_em` | Retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`: instante efetivo da batida convertido ao fuso local e serializado ISO. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `foto_ausente._por` | Retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:102`: proveniência fixa do passe. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `contexto[motivo].data` | Fábrica `app/chamados/services/fabrica_da_celula.py:172`; triagem `app/ponto/services/triagem_batida.py:740`; enriquecimentos do gerador `app/chamados/services/disputa_emissao.py:292`, `:353`. | Dia de turno/furo para gerador `app/chamados/services/disputa_emissao.py:372-375`; destino `:191-196`. Retro não grava esta chave (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`). |
| `contexto[motivo].qtd` | `_abrir_ou_agregar_disputa`, `app/chamados/services/disputa_emissao.py:552-557`: default anterior 1 e incremento em cada merge do mesmo motivo. | O próprio merge lê o contador anterior em `:552`; não é cálculo de batidas distintas (`:553-557`). Outro leitor semântico do envelope não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `contexto[motivo].ultima_em` | `app/chamados/services/disputa_emissao.py:558`: `now()` do merge. | Leitor semântico específico não localizado **[nao verificado quanto a leitura dinâmica/genérica]**. |
| `Batida.timestamp` no caminho das APIs | Principal passa `ts_efetivo` à porta em `app/api/views.py:655`; gêmea em `app/api/views_core.py:646`. | Retro mede minutos reais `(agora - b.timestamp).total_seconds()/60` em `app/ponto/management/commands/marcar_foto_ausente_retro.py:76`; janela inclusiva em `:67`; instante local transportado em `:101`. |
| `tem_foto` | Retro computa OR em `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; API no ato informa ausência em `app/api/views.py:450` / `app/api/views_core.py:436`. | Juiz recusa cobrança com foto em `app/ponto/registro_batida.py:86`. |
| `decorrido_min`, `prazo_apk_min` | Retro fornece minutos e override em `app/ponto/management/commands/marcar_foto_ausente_retro.py:76-77`; APIs fornecem zero em `app/api/views.py:450` / gêmea `:436`. | Juiz escolhe prazo em `app/ponto/registro_batida.py:88-90` e compara inclusivamente em `:91`. |
| `--dias` | Argumento inteiro em `app/ponto/management/commands/marcar_foto_ausente_retro.py:54`. | Duração desde agora em `:65`, filtro inclusivo `:67`; não é contagem de dias do turno. |
| `origem` da porta | APIs `app/api/views.py:765`, `app/api/views_core.py:759`; retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:104`; triagem `app/ponto/services/triagem_batida.py:744`; fábrica `app/chamados/services/fabrica_da_celula.py:172`. | Texto e trilha `app/chamados/services/disputa_emissao.py:664`, `:597-599`; não substitui identidade do episódio. |

As cinco chamadas da porta permanecem nas APIs `app/api/views.py:764`, `app/api/views_core.py:758`, retro `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`, triagem `app/ponto/services/triagem_batida.py:735`, fábrica `app/chamados/services/fabrica_da_celula.py:172`.

As cinco chamadas do gerador permanecem em `app/chamados/management/commands/smoke_chamados.py:103`, `app/chamados/services/disputa_emissao.py:294`, `:670`, `:682`, `app/chamados/services/fabrica_da_celula.py:174`.

O AST leu 892 arquivos sem erro sintático, contra os 887 registrados no anexo; contagem instrumental **[nao verificado em arquivo]**. Mantém o alcance limitado por literais/nomes/aliases de importação; despacho dinâmico **[nao verificado]**.

### Provas citadas na matriz: existência e alcance

Todos os testes citados abaixo **SEGUEM**, como código lido; resultado de execução **[nao verificado]**. [nao verificado]

| Referência da matriz | Localização atual e limite |
|---|---|
| A1 `test_dry_run_nao_escreve` | `app/ponto/tests/test_bug126_selfie_invisivel.py:103`: testa ausência de disputa, não ausência de toda escrita. |
| A3 retroativa | `app/ponto/tests/test_bug126_selfie_invisivel.py:91`: exclui origem retroativa; não testa `retratada_em`. |
| A4 manual / juiz origem | `app/ponto/tests/test_bug126_selfie_invisivel.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:45`. |
| A5 FotoBatida | `app/ponto/tests/test_bug126_selfie_invisivel.py:74`; `app/ponto/tests/test_selfie_ausente_juiz.py:103`. |
| A6 Batida.foto | `app/ponto/tests/test_selfie_ausente_juiz.py:112`. |
| A7 prazo / NULL | `app/ponto/tests/test_bug126_selfie_invisivel.py:84`; `app/ponto/tests/test_selfie_ausente_juiz.py:41`. |
| A8 emissor retro | `app/ponto/tests/test_bug126_selfie_invisivel.py:65`, asserts `batida_id`/`_por` em `:71-72`; não confere pergunta/episódio. |
| A9 agregação PWA | `app/ponto/tests/test_selfie_ausente_juiz.py:119`; payload sintético em `:122` também omite `ultima_batida`. Confere número de disputas, não episódio. |
| A10 repetição | `app/ponto/tests/test_bug126_selfie_invisivel.py:108`, assert `:113`: somente número de disputas. |
| A12 API PWA | `app/api/tests/test_api_bater_pwa.py:64`, `:78`, `:95`: presença de motivo, contador e ausência com foto; não confere identidade da pergunta. |
| A13 prazo delegado | `app/ponto/tests/test_selfie_ausente_juiz.py:82`: troca prazo do juiz e observa retro. |
| B1 conversa genérica | `app/chamados/tests/test_a2_data_evento_batida.py:53`: motivo usado é `batida_fora_escala_grave`, sem retro; assert `data_evento=localdate()` em `:56`. |
| B4 índice episódio | `app/chamados/tests/test_e1_v5_indice.py:69`: corrida crua, motivo `gps_ausente`; assert de origem em `:74`. |
| B5 episódios diferentes | `app/chamados/tests/test_e1_v5_indice.py:84`; `app/chamados/tests/test_a2_data_evento_batida.py:22`. |
| B6 segunda batida | `app/chamados/tests/test_e1_v6_horizonte.py:262`: usa contexto sintético com instante da batida `:260`, não o `agora` da API; não executa retro. |
| B9 texto foto | `app/chamados/tests/test_cenarios_disputa.py:43`. |
| B10 conversas fora do índice | `app/chamados/tests/test_e1_v5_indice.py:96`: confere permissividade do índice, não dedup sequencial do gerador. A repetição de conversa do episódio retratado tem teste próprio em `app/chamados/tests/test_e1_v6_horizonte.py:445`, com motivo `gps_ausente` em `:451`. |
| C1/C4 upload | `app/chamados/tests/test_b8_smoke_portas.py:60`: teste existe; fecho da disputa não demonstrado por esta referência **[nao verificado quanto ao fecho no teste]**. |
| C2 porta aparar | `app/chamados/tests/test_porta_chamado_e3.py:56`, asserts em `:58`, `:62`: porta diretamente, sem upload. |
| Selo delegação selfie | `app/ponto/tests/test_selfie_ausente_juiz.py:130`: AST só nas duas APIs, não prende envelope do retro. |

### Distinções que não podem virar resposta de desenho

- A8/B1 “estado pretendido” está apresentado como cura no anexo, enquanto o código atual apenas fornece o payload incompatível e cai na conversa se não houver data/episódio; escolha do transporte continua pergunta D1 (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:387`; `:431-441`).
- A9/B3 preservam o risco de contexto anterior porque chaves ausentes no payload não são removidas; modelo preciso evento/agregado continua pergunta D2 (`app/chamados/services/disputa_emissao.py:548-561`).
- A10 “2x = mesmo estado” excede a prova do teste: o contador e `ultima_em` mudam a cada merge; sem corte específico, não transformar essa frase em decisão D6 (`app/chamados/services/disputa_emissao.py:552-558`; `app/ponto/tests/test_bug126_selfie_invisivel.py:113`).
- “Instante do fato” e “recepção” continuam distintos no próprio código; qual significado deve ter o envelope é D3, sem resposta aqui (`app/api/views.py:507`; `:655`; `:761`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`).
- B7 “conversa ou recusa” são ramos distintos: gerador conhece episódio mas juiz sem dia → conversa, com dedup de conversa; a recusa do modelo requer conferir a entrada datada (`app/chamados/services/disputa_emissao.py:431-441`; `app/chamados/tests/test_e1_v6_horizonte.py:445`).
- “SEM PROVA” dos anexos deve ser registrado como **teste específico não demonstrado**, não como inexistência universal de prova **[nao verificado quanto a todos os testes indiretos/dinâmicos]**. [nao verificado]

## 15/11 00:44 · sessao f865e2f688d8 · - · - · origem=Ronald (tmux)

Censo dos escritores da cópia, sem alterações:

| Função | Escrita de datas |
|---|---|
| `executar_vinculo` | Reabre `data_fim=None` em `app/colaboradores/services/vinculo.py:277`, persiste em `:283`; cria com `data_inicio` em `:287`/`:291`; fecha vínculos pela porta em `:214` e `:241`. |
| `fechar_vigencia` | Atualiza `data_fim` em `app/colaboradores/services/vinculo.py:641`, restringindo a `data_inicio <= data_fim`. |
| `reabrir_vigencia_impossivel` | Atualiza `data_fim` em `app/colaboradores/services/vinculo.py:730`; valida em `:724`. |
| `encerrar_vinculo` | Atribui e salva `data_fim` em `app/colaboradores/services/vinculo.py:844`/`:845`; valida em `:835`. |
| `criar_trecho_retroativo` | Cria com ambas as datas em `app/colaboradores/services/vinculo.py:970`/`:972`; valida em `:969`. |
| `corrigir_vinculo_retroativo` | Atribui e salva `data_fim` em `app/colaboradores/services/vinculo.py:982`/`:983`; valida em `:979`. |
| `reverter_para_snapshot` | Reescreve ambas as datas em `app/colaboradores/services/vinculo.py:1031`/`:1032`; valida cada par em `:1015`. |
| `ajustar_vinculo_pelo_sistema` | Fecha pela porta em `app/colaboradores/services/vinculo.py:1087`; cria com início em `:1089`/`:1090`. |
| `desfazer_ajuste_do_sistema` | Reabre fim nulo em `app/colaboradores/services/vinculo.py:1106`; fecha pela porta em `:1108`; cria com início em `:1110`/`:1111`. |
| `fechar_vinculos_ativos` | Delega a `fechar_vigencia` em `app/colaboradores/services/vinculo.py:862`; seleciona somente `ativa=True` em `:863`. |
| `gravar_regua` | Porta genérica aceita `**campos` em `app/colaboradores/services/vinculo.py:1048` e atualiza sem allowlist em `:1070`; seus chamadores atuais passam campos de régua em `app/escala/regua_defesa.py:662`, `:719`, `:742`, `:765`. |

Rotas que chegam a esses escritores:

- Wizard: `app/colaboradores/views.py:423`; chat: `app/colaboradores/services/troca_pelo_chat.py:75`; régua: `app/escala/regua_defesa.py:321`; comando de fase: `app/ponto/management/commands/censo_fase_12x36.py:423`.
- Desvinculação: `app/escala/views.py:269`; backfill de desligados: `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`; desligamento: `app/colaboradores/services/desligamento.py:148`.
- Retroativo: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196` e `:212`; snapshot: `app/chamados/services/acoes_chamado.py:149`.
- Automático: `app/escala/services/escala_auto_executor.py:103` e `:145`; reparo: `app/escala/management/commands/corrigir_vinculo_vigencia.py:86` e `:107`.

O admin de EC herda `SoLeitura` em `app/escala/admin.py:19`; adição, alteração e exclusão retornam `False` em `app/core/admin.py:59`, `:62`, `:65`. O bootstrap cria Empresa, Setores e User em `app/tenants/bootstrap.py:25`, `:32`, `:34`.

Há escritores de fixtures em testes, por exemplo `app/tenants/tests.py:44`, `app/colaboradores/tests/test_ficha_do_colab.py:88` e `app/colaboradores/tests/test_situacional_pergunta_em_lote.py:62`. A varredura AST contou 829 sítios diretos com keywords dessas datas em 643 arquivos de teste; não existe artefato persistido dessa contagem [não verificado].

A cópia já declara a constraint em `app/escala/models.py:1037` e contém migration com `CHECK ... NOT VALID` em `app/escala/migrations/0041_ec_vigencia_not_valid.py:43`. Aplicação em prod [não verificado].

## 15/11 00:44 · sessao db1f8fd7598c · - · - · origem=Ronald (tmux)

Textos escritos que sustentam casos de especificação, sem usar o código como gabarito: a L-110 exige “cadastro tal, batidas tais” e manda a pergunta sem resposta pela regra para a lista separada (`app/docs/LEIS.md:136`).

| Transição do mapa | Caso escrito pela regra → esperado | Texto da regra |
|---|---|---|
| N1 + P1 | Cadastro com intervalo fixo; entrada, retorno do intervalo e saída registrados, saída para intervalo ausente; pergunta inexistente → emitir UMA pergunta do marco apagado, a partir do sinal da célula. | “lampada apagada no miolo passa a gerar UMA pergunta […] nascida do sinal da celula (nunca de varredura)” (`app/docs/CORTES.json:772`). |
| N1 — selfie | Cadastro de colaborador com batida de aparelho APK sem foto; prazo de 90 minutos vencido → cobrança abre disputa; em PWA, ausência de foto no ato → cobrança abre disputa. | “foto ausente e COBRANCA (abre disputa […]): […] prazo do CANAL (PWA = no ato; APK = 90 min)” (`app/docs/CORTES.md:94`). |
| P2 — dedup | Mesmo cadastro, dia, marco faltante e pergunta do mesmo fato; sinal repetido → uma pergunta desse fato, sem duplicação. O texto não decide a dona fechada nem autoriza reapontamento arbitrário. | “UMA pergunta” e “dedup pela chave do fato” (`app/docs/CORTES.json:772`). |
| F1 | Cadastro com dia já resolvido; perguntas do questionário sem viva e nenhuma resposta sem veredito → disputa elegível a fechar; resposta sem veredito → fechamento recusado. | “fecha quando `pergunta_viva(disputa)` esta vazio (C6), e a porta […] recusa resposta sem veredito” (`app/docs/BACKLOG.md:1549`). O motivo persistido `vazia` para zero perguntas não recebe gabarito específico nesse texto. |
| F1 — recusa por idade | Cadastro e batidas ainda deixando fato sem solução; pergunta muda há 30 dias → idade não absolve nem fecha; pergunta parada vai à lista administrativa. | “silencio de 30 dias e QUALQUER fechamento por idade MORREM como absolvicao — pergunta parada vira lista do admin, nao fechamento” (`app/docs/BACKLOG.md:1549`). |
| F2 + P10 no desligamento | Cadastro desligado com pergunta já respondida sem veredito → encerrar também essa pergunta por via própria de desligamento e listar dia/pergunta/resposta no chamado de revisão; muda também recebe via própria. | O corte antigo de manter respondida viva foi substituído (`app/docs/PROMPTS.md:2222`); novo conteúdo registrado em `app/docs/BACKLOG.md:732`; “via propria desligamento tambem para as mudas” (`app/docs/CORTES.json:1610`). Não declarar a cura implementada com base nessa regra. |
| F3 — respondida | Cadastro ativo, pergunta respondida sem decisão do dia; mero encerramento administrativo da disputa → a regra escrita de 28/10 rejeita tratá-la como decisão humana que não ocorreu. A extensão exata da revogação de 13/11 ao `admin_encerra`, fora de desligamento, fica [nao verificado]. | “encerrar a disputa […] nao decide o dia” e proibição de `admin_decidiu` em já respondida (`app/docs/CORTES.json:1258`); o corte sucessor trata desligamento (`app/docs/BACKLOG.md:732`). |
| F4 / tranca por leitura | Cadastro com dia em competência trancada, sem toque humano posterior → C6 lê a tranca sem novo carimbo; destrancar devolve vitalidade; toque humano posterior exclui aquele colab-dia da tranca. | “C6 LE a tranca […] sem carimbo e sem estado novo; destrancar devolve a pergunta viva” (`app/docs/PROMPTS.md:1338`); exceção do toque humano (`app/docs/PROMPTS.md:1342`). A escolha de fechar a raiz pelo motivo `competencia_trancada` continua sem gabarito específico nessas cláusulas. |
| R1/R2 — desligado | Cadastro desligado e disputa fechada → não reabrir a disputa do desligado. | “so chamado; disputa de desligado nao reabre” (`app/docs/CORTES.json:1617`). O texto não decide as demais reaberturas. |
| M2 + P4, por episódio de foto | Cadastro com batida que originou cobrança de foto; foto entregue, inclusive fora do prazo → extinguir a cobrança do episódio identificado por `batida_origem`. | “por episodio via batida_origem” e “foto fora do prazo tambem extingue” (`app/docs/CORTES.json:1596`). O texto não determina a cadeia exata de campos de resposta/validação nem o fechamento de toda raiz. |
| P3, primeira resposta humana | Cadastro ativo, pergunta sem resposta; admin tenta dar a primeira resposta em nome do colaborador → não permitido. | “admin nao da a primeira resposta pelo colab” (`app/docs/CORTES.json:1659`). Essa cláusula não responde ao caso de nova resposta invalidar validação anterior. |
| P5 + P9 — intervalo | Cadastro com intervalo fixo faltante, pergunta respondida com hora e validada pelo admin → registrar intervalo e retirar indenização do dia; sem resposta ou recusada → indenizar pela política escrita. | “Resposta validada pelo admin vira o registro do intervalo e a indenizacao do dia sai; sem resposta ou recusada […] Art. 71 par. 4, indeniza” (`app/docs/CORTES.json:772`). Não usar isso como regra genérica de plausibilidade/folga de toda pergunta. |
| P9 — fato que chega após resposta | Cadastro coerente com escala/posto, pergunta respondida, batida posterior acende o marco: concorda com a resposta pela régua de validação automática → pergunta morre sozinha; discorda → permanece viva para admin. | Corte literal registrado em `app/docs/BACKLOG.md:1731`. |
| P10 — duplicata com episódio | Mesmo cadastro, batida/dia/motivo comprovadamente iguais; pergunta COM episódio e outra SEM episódio → conservar a COM episódio e retirar a SEM pela via fora do fato, com trilha; sem prova de mesmo fato, conservar ambas. | D8 RAIZ (`app/docs/BACKLOG.md:2016`). Essa regra não responde a todas as outras vias de identidade. |
| Destino F1 | Cadastro, dia e fato únicos, dois chamados candidatos do mesmo fato com PKs distintos; mesma pergunta reapresentada em ordens diferentes → destino eleito pelo fato, empate pelo menor PK; um escritor. | “escritor unico […] eleitor por fato com empate menor PK” (`app/docs/PROMPTS.md:2085`); L-007: “campo de estado tem 1 escritor canonico + tripwire” (`app/docs/LEIS.md:65`). |
| T1 — trilha de ação crítica | Mesmo cadastro/batidas; ação crítica muda estado → registrar usuário, antes/depois e motivo. | “acao critica grava usuario, antes/depois e motivo” (`app/docs/LEIS.md:65`). Isso não define sozinho os oito atos nem imutabilidade da sequência. |

A regra da L-033 dá um caso de conservação: cadastro com jornada, entrada e saída presentes e só intervalo faltando → dia pagável e pergunta segue aberta (`app/docs/LEIS.md:81`). Não transformá-la em autorização genérica para fechar a pergunta.

Os invariantes escritos para a matriz são: aberta tem ao menos uma viva; push somente com viva pendente daquele colaborador; eleitor único do destino; fonte única do dia (`app/docs/CORTES.json:1694`). Eles permitem casos positivos/negativos de emissão e push, mas não resolvem a escolha entre agregar à dona fechada, reabrir ou criar outra: essa decisão permanece expressamente a pergunta N5 da F1 (`app/docs/CORTES.json:1708`).

Transições/partes sem resposta normativa específica encontrada nos textos examinados e que devem ir a C, preservando o comportamento atual somente em A:

- N2: critério de agregar motivos/contexto à mesma raiz; N3: motivo/pergunta novo em dona fechada; N1: quando criar raiz vazia e como tratar geração sem pergunta viva — N5 já está preservada no corte (`app/docs/CORTES.json:1708`); escolha normativa adicional [nao verificado].
- F1: motivo `vazia` versus `veredito` e precisão de exceções; F3: mecanismo das mudas e alcance da revogação; F4: fechamento da raiz por tranca, além de C6 por leitura [nao verificado].
- R1: teto administrativo 2 e justificativa mínima 20; R2: conjunto de causas sistêmicas além da proibição para desligado; R3: teto colaborador 2 e critérios exatos de corrigibilidade [nao verificado].
- M1: destino das perguntas na retratação de motivo; M2: campos exatos do automatismo da foto, além da extinção por episódio [nao verificado].
- P2: troca de célula/destino e conservação da dona; P3: sobrescrita de respondida/validada e invalidação anterior; P4: reparo pela trilha e data do carimbo [nao verificado].
- P5: demais tipos de resposta, plausibilidade e ramo folga; P6: devolução da validação recusada e reabertura da dona; P7: motivo obrigatório e limpeza da resposta na recusa; P8: limpeza completa na correção/desabsorção [nao verificado].
- P9: todas as demais vias e condições de carimbo; P10: `admin_decidiu`, `motivo_retratado`, `paridade_recusada`, `fato_ocupado` fora dos recortes normativos acima; P11: condições gerais de descarimbo/ressuscitação; P12/P13: troca de via e relavra [nao verificado].
- V1/V2: escolha dos três valores e política de falha/backfill; T1: oito atos e imutabilidade; I1: regra do preenchimento histórico de identidade [nao verificado].

Cuidado com cortes curtos sem a pergunta de origem: `LU-20 = nao validavel` e `LU-21 = reinicia` estão escritos, mas o referente completo não está nesses registros; não atribuir escopo inventado (`app/docs/CORTES.json:1673`, `app/docs/CORTES.json:1680`).

## 15/11 00:44 · sessao 215a8f9a2af7 · - · - · origem=Ronald (tmux)

- Universo real: `F.dias_que_seguram(empresa, mes, ano)` chama `classificar_export` e `_dias_que_retem`; esta delega a `motivos_retencao_celula(..., com_dias=True)`. Não reconstruir pelo TSV. (`app/folha/fora_da_folha.py:109`, `:126`, `:144`, `:105`)
- Carga/tarefa: `F._carga(colabs, ini, fim, AGORA)` → `carga_da_tarefa`; usar `carga.tarefa(colab, dia)` e `porta_da_tarefa(t)['fio']`. (`app/folha/fora_da_folha.py:58`, `app/chamados/services/tarefa_do_dia.py:648`, `:549`, `:638`)
- Vencimento deve preservar o anexo: ler **pai da disputa**, `p.disputa.chamado.prazo_sla`, e chamar `prazo_estourou(prazo, AGORA)`, cujo critério é `prazo <= agora`. Não trocar pelo destino retornado por `chamado_que_lista`, nem pelo prazo derivado de cobrança. (`app/chamados/catalogo/motor.py:54`, `app/chamados/services/fio.py:97`, `:117`; seleção do anexo `[nao verificado]`)
- Previsto único: `escala.utils.minutos_previstos_do_dia(colab, data, celulas=None, folgas=None, escalas=None)`. Ela consulta vínculo, dia de trabalho, marcos e todas as pausas; não usa `minutos_jornada`. (`app/escala/utils.py:2230`, `:2234`, `:2260`, `:2266`, `:2269`, `:2291`, `:2320`)
- Para alimentar essa função pela carga: `carga.celulas_de(cid, ini, fim)` oferece o formato `{(cid, data): celula}`; `carga._vinculos[cid]` contém todos os vínculos carregados, compatíveis com o contrato de `escalas`. (`app/ponto/turnos.py:2988`, `:2990`, `:3003`; `app/escala/utils.py:2252`)
- Relógio: `dias_que_seguram` chega a `timezone.localdate()`; congelar `AGORA` evita calendário divergente. A carga declara que vários estados continuam atuais mesmo recebendo horizonte histórico. (`app/folha/export.py:1238`; `app/chamados/services/tarefa_do_dia.py:370`, `:374`)
- Proteção de leitura já existente: `somente_leitura`, em `diff_e3_sombra.py`, recusa comandos múltiplos, CTE, `INTO`, `NEXTVAL`, operações de escrita e outros SQL não reconhecidos. O comando instala wrappers em todos os aliases. (`app/chamados/management/commands/diff_e3_sombra.py:30`, `:94`)
- **Dependência crítica:** antes da carga, ler usuário reservado com SELECT e abrir `tranca_em_lote(sistema_pk=...)`, como o comando existente faz; isso impede dependências de chegarem ao `get_user_sistema/get_or_create`. Ausência do usuário deve ser erro, sem criação. (`app/chamados/management/commands/diff_e3_sombra.py:96`, `:100`; `app/chamados/tests/test_e3_diff_sombra.py:26`, `:38`; `app/chamados/juizes.py:1225`)
- Escritor de `prazo_sla`: `ChamadoColaborador.definir_prazo_sla`, atribuição e `save` exclusivos dessa porta; emissores também fornecem o prazo na criação. Leitor do vencimento: `prazo_estourou`. (`app/chamados/models.py:574`, `:589`, `:592`; `app/chamados/services/disputa_emissao.py:581`, `:1117`, `:1352`, `:1725`; `app/chamados/services/cobranca.py:178`; `app/chamados/catalogo/motor.py:54`)
- Escritor de `CelulaDia.veredito/protestos`: `ponto.portas.celula.lavrar_veredito`; leitor da retenção: `_dias_sem_decisao`. (`app/ponto/portas/celula.py:1032`, `:1092`, `:1093`, `:1098`; `app/folha/export.py:1273`, `:1286`, `:1293`)
- O envelope só monta a árvore em `/app` e a sonda em `/sonda/sonda.py`; não disponibiliza automaticamente o TSV colado. Portanto, a conferência precisa de texto embutido ou entrada explicitamente fornecida e legível. (`bin/sonda_leitura.sh:214`, `:217`, `:238`, `:240`)
- As leituras atuais da tarefa carregam `veredito_lavrado` e executam `_tarefa_pura`/`_portas_da_tarefa`, sem chamada explícita ao materializador nesses trechos. (`app/chamados/services/tarefa_do_dia.py:421`, `:424`, `:549`, `:557`, `:567`)

## 15/11 00:44 · sessao 31ff7cb1277f · - · - · origem=Ronald (tmux)

Transitivos confirmados:

- **A19 e projeção:** `DecisaoPergunta.registrar` insere DP e chama `PerguntaDisputa.acordar_celulas` (`app/chamados/models.py:4096`, `:4097`). A reabertura também chama `acordar_celulas` explicitamente (`app/chamados/services/disputa_emissao.py:1635`).
- **`acordar_celulas` apenas lê `CelulaDia`** para localizar célula e delega inserção do evento (`app/chamados/models.py:3323`, `:3325`, `:3327`, `:3331`). A escrita concreta é **`escala.EventoCelula(celula_id, colaborador_id, data)`**, em `app/ponto/portas/celula.py:954`; `criado_em` e defaults de processamento estão em `app/escala/models.py:1860`.
- **A18 colisão** salva C/via, acionando o receiver da outbox (`app/chamados/signals.py:253`, `:257`, `:259`). Gravar somente `recusa_motivo` não passa pelo conjunto `CAMPOS_QUE_ACORDAM` (`app/chamados/models.py:3303`, `app/chamados/signals.py:257`).
- **Não há escrita direta de `CelulaDia`, `FechamentoMensal.derivado_em` ou L** nas funções `acordar_celulas`, `reabrir_para_correcao`, `carimbar_identidade` e `DecisaoPergunta.registrar` examinadas (`app/chamados/models.py:3314`, `:3251`, `:4086`; `app/chamados/services/disputa_emissao.py:1562`). Generalização para todo despacho dinâmico: **[nao verificado]**.
- A função de invalidação de FM escreveria `derivado_em=None` e `atualizado_em` quando chamada, mas pertence a outro caminho; nenhuma chamada direta A18/A19/DP foi encontrada (`app/ponto/services/fechamento.py:1616`, `:1656`; cobertura dinâmica **[nao verificado]**).
- **L** tem escritores próprios: `gravar_lavras_em_lote` (`app/chamados/veredito_lavrado.py:240`, `:250`) e `lavrar`, que grava `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`:270`, `:272`, `:273`, `:275`).

Existe cascata **após commit**: qualquer `PerguntaDisputa.save` agenda `becos.fechar_beco_da_disputa` (`app/chamados/signals.py:230`, `:240`). Se o classificador real permitir, chama resolver e `DisputaSupervisao.fechar` (`app/chamados/services/becos.py:25`, `:54`, `:57`, `:74`). Esse fechamento grava F (`app/chamados/models.py:1996`–`:2000`) e dispara os receivers da disputa, inclusive transição do chamado-pai e retomada de regularização (`app/chamados/signals.py:43`, `:139`, `:146`, `:165`). Quantidade/campos efetivamente atingidos nessa cascata sem dados/alcance leitor completo: **[nao verificado]**.

O consumidor da outbox é outra execução, `consumir_eventos_celula`, que chama rejunção, retratações, lastro e fábrica (`app/chamados/reconciliador.py:1316`, `:1365`–`:1388`). Seu efeito não deve ser confundido com o simples INSERT do evento (`app/ponto/portas/celula.py:954`).

A19 também cria `LogAuditoria` pelos dois adaptadores de auditoria (`app/chamados/services/validacao.py:637`, `:643`; `app/core/auditoria.py:22`; `app/core/observ.py:134`–`:138`), além da nota já informada.

## 15/11 00:44 · sessao 697894d24fd9 · - · - · origem=Ronald (tmux)

Conferência das referências sob minha responsabilidade:

| Citação anexada | Estado | Sítio desta cópia e conteúdo |
|---|---|---|
| `CLAUDE.md:142` | SEGUE | Motor exige aval explícito. (`CLAUDE.md:142`) |
| `CLAUDE.md:143` | SEGUE | Porta única de escrita da batida; motor não fabrica batida. (`CLAUDE.md:143`) |
| `CLAUDE.md:146` | SEGUE | UTC no banco; apresentação UTC-3; sonda usa `timezone.localtime`. (`CLAUDE.md:146`) |
| `CLAUDE.md:148` | SEGUE | Grade como fonte única de previsto. (`CLAUDE.md:148`) |
| `CLAUDE.md:178` | SEGUE | Idempotência estrutural: 2x = mesmo estado, uma trilha, zero duplicata; parâmetro consumido ou declarado sem efeito. (`CLAUDE.md:178`) |
| `CLAUDE.md:179` | SEGUE | Separação ESTRUTURA/CADASTRO/BATIDA; só ESTRUTURA é cura de código. (`CLAUDE.md:179`) |
| `CLAUDE.md:411` | SEGUE | Gravado exportado não muda por caminho genérico; regime da porta específica nas linhas seguintes. (`CLAUDE.md:411`) |
| `app/docs/LEIS.md:107` | SEGUE | L-080 inclui UTC/UTC-3. (`app/docs/LEIS.md:107`) |
| `app/docs/LEIS.md:136` | SEGUE | L-110: casos pela regra antes do código, perguntas sem resposta escrita separadas, código atual não é gabarito. (`app/docs/LEIS.md:136`) |
| `app/docs/LEIS.md:145` | SEGUE | L-124: horizonte para frente, passado FOTO até fato novo, backfill único. (`app/docs/LEIS.md:145`) |
| `app/docs/LEIS.md:152` citado como L-114 | MUDOU | L-114 está em `app/docs/LEIS.md:153`; a linha 152 agora contém L-115. (`app/docs/LEIS.md:152`; `app/docs/LEIS.md:153`) |
| `app/docs/LEIS.md:153` citado como L-113 | MUDOU | L-113 está em `app/docs/LEIS.md:154`; conserva nascimento permitido em exportada, trilha e DIFF anterior ao ato de frota. (`app/docs/LEIS.md:154`) |
| `app/docs/JUIZES.md:14` | SEGUE | Registro vivo prevalece sobre lookup. (`app/docs/JUIZES.md:14`) |
| `app/docs/JUIZES.md:20` | SEGUE | `turnos_do_colab` continua no lookup da geometria do turno. (`app/docs/JUIZES.md:20`) |
| `app/docs/JUIZES.md:38` | SEGUE | `validar_pergunta` continua escritor único de `validada_em`. (`app/docs/JUIZES.md:38`) |
| `app/core/juizes.py:208` citado como registro `turnos_do_colab` | MUDOU | Registro em `app/core/juizes.py:212`; pergunta: a que turno/data_turno pertence a batida. A linha 208 hoje registra folga lançada. (`app/core/juizes.py:208`; `app/core/juizes.py:212`) |
| `app/core/juizes.py:1199` citado como registro `selfie_ausente` | MUDOU | Registro em `app/core/juizes.py:1205`; comentários de prazo/canal em 1202–1204. (`app/core/juizes.py:1202`; `app/core/juizes.py:1205`) |
| `app/docs/CORTES.md:283` citado como corte da selfie | MUDOU | Texto integral em `app/docs/CORTES.md:334`; resumo também em 94. A linha 283 atual trata de outras assinaturas. (`app/docs/CORTES.md:283`; `app/docs/CORTES.md:334`) |
| `BACKLOG.md:2109` na matriz | MUDOU | Caminho correto `app/docs/BACKLOG.md:2166`: mesmo desencontro emissor/leitores, entrega do episódio e selo AST; texto recebeu atualização de aval. (`app/docs/BACKLOG.md:2166`) |
| `app/colaboradores/services/ficha.py:215` | SEGUE | Lê `app['ultima_batida']` de ficha; escritor próprio em 147, sem relação com contexto da disputa. (`app/colaboradores/services/ficha.py:147`; `app/colaboradores/services/ficha.py:215`) |
| `app/escala/services/furos_diarios.py:281` | SEGUE | Escreve data civil local de última batida em linha de relatório, outro domínio. (`app/escala/services/furos_diarios.py:281`) |
| `app/relatorios/views.py:1675` | SEGUE | Formata a última batida da linha de relatório. (`app/relatorios/views.py:1675`) |

Nenhuma referência normativa acima SUMIU; houve deslocamentos e duas referências antigas de LEIS passaram a apontar outra lei. (`app/docs/LEIS.md:152`; `app/docs/LEIS.md:153`; `app/docs/LEIS.md:154`)

Há dois cortes específicos desta obra, ausentes do envelope anexado:

- LU-09 diz literalmente “por episodio via batida_origem” e que foto fora do prazo também extingue. É corte explícito para D5. (`app/docs/CORTES.md:441`; `app/docs/CORTES.json:1596`)
- LU-10 diz “retirar o campo sem leitor”, sem nomear o campo. Sua aplicação a `qtd` permanece **[nao verificado]**; o texto normativo reproduzido em JSON também não identifica o campo. (`app/docs/CORTES.md:442`; `app/docs/CORTES.json:1603`)

A declaração posterior de que D1–D4 estariam decididos não substitui o teor de um corte: o registro não reproduz as decisões nem identifica o enunciado que escolhe o transporte, o alcance do merge ou o significado do timestamp. Portanto, essas escolhas devem continuar como perguntas separadas, sem resposta **[nao verificado quanto ao corte específico]**.

Para o CENSO: o juiz do dia de turno tem registro verificável; o juiz de ausência de foto tem registro e corte verificáveis; a validação tem lookup verificável. (`app/core/juizes.py:212`; `app/core/juizes.py:1205`; `app/docs/CORTES.md:334`; `app/docs/JUIZES.md:38`)

Não transportar dados adicionais de produção encontrados em documentos da árvore: eles não pertencem ao envelope autorizado **[nao verificado pelo dado fornecido]**.

## 15/11 00:44 · sessao fcbe4f6072a4 · - · - · origem=Ronald (tmux)

Leitores comprovados do intervalo e comportamento:

| Leitor | Evidência | Tratamento de fim < início |
|---|---|---|
| `escalas_do_periodo_por_colab` | `app/escala/alimentacao.py:149-151` | Filtra interseção (`início <= fim_janela`, fim nulo ou `fim >= início_janela`); **não verifica** `fim >= início`. Pode incluir um invertido numa janela suficientemente abrangente. `mais=` readmite geradoras fora do filtro, `:155-159`. |
| `escalas_do_periodo` | `app/escala/alimentacao.py:185-186` | Delega ao anterior. |
| `vinculo_cobre_a_data` | `app/escala/alimentacao.py:214-216` | Exige `início <= dia <= fim`, além de vigência declarada. Invertido não cobre nenhum dia. |
| `q_vinculo_cobre_a_data` | `app/escala/alimentacao.py:223-225` | Mesma regra em SQL. |
| `vinculo_do_dia`, geradora | `app/escala/alimentacao.py:279-284` | Recusa dia fora dos dois limites; invertido sempre retorna `None`. |
| `vinculo_do_dia`, fallback | `app/escala/alimentacao.py:285-286` | Usa `vinculo_cobre_a_data`; invertido não vira candidato. |
| `vinculo_do_dia_carregado` | `app/escala/alimentacao.py:289` | Carregador da juíza, sem novo predicado; docstring em `:292-294`. |
| `previstas_do_periodo` | `app/escala/alimentacao.py:390-393` | Recorta o intervalo e pula explicitamente `fim < ini`. |
| Apuração por fatias | `app/ponto/services/fechamento.py:206-207`, `:242-243` | Recorta e pula explicitamente fatia invertida. |
| `vinculos_inconsistentes` | `app/escala/services/vinculos_inconsistentes.py:24-37` | Detecta invertidos em `:30-31`, exclui-os da análise de sobreposição. |
| `vigencia_sem_trilha` | `app/escala/services/vigencia_sem_trilha.py:66-77` | Usa `vigencias_impossiveis`, separa o passivo e não o soma aos demais sem trilha. |
| `rejulgar_furos_do_vinculo` | `app/escala/regua_defesa.py:686-688` | Usa `data__range=(data_inicio, min(data_fim, ontem))`; não valida par, range invertido seleciona zero. |
| `piso_da_fase` | `app/escala/services/fase_na_tela.py:160-165` | Cobertura pela juíza SQL; sem cobertura, retorna menor início de todos os vínculos, sem conferir fim. |
| Propositor | `app/escala/services/propositor.py:448`, `:460-461` | Seleciona pelo juiz; depois pula fim anterior ao piso de aplicação. |
| Exportação, explicação de cadastro zero | `app/folha/export.py:938-957` | Passa lista inteira ao previsto por dia; lê finais apenas para mostrar o último encerramento. |
| Signals de mudança de vigência | `app/escala/signals.py:25-28`, `:57-75`, `:121-126`, `:200-204` | Comparam vigência anterior/atual e calculam janela de regeneração; não exigem `fim >= início` nesses trechos. |

Consumidores que delegam a cobertura/seleção à função real:

- Folgas: `app/escala/folgas.py:451`, `:487`, `:493`, `:539`, `:624`.
- Semana do posto: `app/escala/services/semana_do_posto.py:75`.
- Cadastro realidade: `app/escala/services/cadastro_realidade.py:489`, `:509`, `:756`, `:760`.
- Consultas de colaboradores: `app/colaboradores/queries.py:64`.
- Calendário: `app/colaboradores/services/calendario.py:505-506`.
- Ficha: `app/colaboradores/services/ficha.py:129`; exibição de início/fim em `:86-93`, trocas por início em `:131-135`.
- Serviço jornada: `app/escala/servico_jornada.py:63-66`.
- Geração de células, outros vínculos: `app/ponto/management/commands/gerar_celulas.py:283-287`.
- Porta de regeneração: `app/ponto/portas/celula.py:229`, `:339`, `:906-909`.
- Regeneração exportada: `app/ponto/portas/regen_exportada.py:147-152`.
- Diagnóstico de escala: `app/ponto/services/diagnostico_escala.py:53`.
- Espelho: `app/ponto/services/espelho.py:286`, `:1065`.
- PDF: `app/relatorios/pdf_espelho.py:307`, `:571`.
- Calculador: `app/ponto/calculador/alimentacao.py:192-196`, carregamento por limite inicial em `:421`.
- Turnos: `app/ponto/turnos.py:1960`, `:2166`, `:2361`, `:2986`, `:3058`.
- Precedência: `app/ponto/precedencia.py:312`, `:323`, `:349`.
- Prévia folha: `app/folha/previa.py:62`, `:81`.
- Exportação: `app/folha/export.py:359`, `:1252`.
- Feriados: `app/core/feriados.py:154`, `:161`.
- Régua CCT: `app/core/regua_cct.py:329`.
- Supremo juiz: `app/ponto/supra_juiz.py:174`, `:185`.
- Núcleo: `app/ponto/nucleo.py:15`, `:26`.
- Limbo folgas: `app/chamados/services/limbo_folgas.py:200`.
- Invariantes chamados: `app/chamados/management/commands/auditar_invariantes_chamados.py:361`, `:391`, `:1538`.
- Triagem batida: `app/ponto/services/triagem_batida.py:149`, `:402`.
- API mensageria: `app/api/views_mensageria.py:979`.
- Cartório: `app/ponto/services/cartorio.py:1768`, `:1903`.
- Vigia futuro: `app/ponto/services/vigia_cadastro_futuro.py:58`.
- Vigia autoridade: `app/ponto/management/commands/vigiar_motor_autoridade.py:51`.
- Cadastro futuro: `app/ponto/services/cadastro_futuro.py:70`.

Leitores unilaterais não demonstram suposição sobre `fim >= início`: piso do espelho usa apenas início em `app/ponto/services/espelho.py:43-45`; detector ausência usa apenas início em `app/ponto/management/commands/detectar_ausencias.py:177`, `:247`; supra diagnóstico usa apenas início em `app/ponto/management/commands/supra_juiz.py:149`; tripwire usa início e teto em `app/ponto/management/commands/tripwire_celulas.py:67`. Consultas de “aberto” excluem todos os finais não nulos: `app/chamados/detector_anomalias.py:58-62`, `app/escala/services/escala_auto.py:172`, `app/ponto/services/cluster_espurio.py:185`, `app/relatorios/furos.py:61`, `app/chamados/services/disputa_emissao.py:341`.

Diagnóstico conserva regra antiga explicitamente: `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:117-118`. Gerador smoke usa interseção, sem validação do par: `app/ponto/management/commands/gerar_batidas_smoke_motor.py:73-79`.

Executei o censo AST **existente**, somente em memória com `python3 -B`/`runpy`, sem Django nem banco: `copias('app')` retornou conjunto vazio. Algoritmo em `app/core/censo_pergunta2.py:64-117`; cânones em `:28-32`, baseline em `:40`, outros modelos em `:43-60`. Esse selo detecta pares de limites, não todo acesso unilateral; seus limites estão declarados em `:10-18`. Portanto, chamar isso de censo universal de qualquer acesso dinâmico seria `[nao verificado]`.

Atenção: a cópia contém uma constraint **condicional**; ler o contexto antes de afirmar que protege todo passivo: `app/escala/models.py:1039-1040`.

## 15/11 00:43 · sessao 9dc5c8ea9331 · - · - · origem=Ronald (tmux)

A autoridade do dinheiro por dia é `DiaPago` versão `motor`, lida por `linhas_do_periodo` e interpretada por `pago_do_dia`/`rubricas_do_dia`; o TXT lê `FechamentoMensal` gravado e `eventos_do_fechamento`, portanto `fechamento_lido` não substitui o lavrado atual. (`app/ponto/services/dia_pago.py:833`; `app/ponto/services/dia_pago.py:867`; `app/ponto/services/dia_pago.py:901`; `app/folha/export.py:719`; `app/folha/export.py:730`; `app/ponto/services/fechamento.py:1735`)

| Campos lidos | Escritor | Leitor real |
|---|---|---|
| `DiaPago` chaves `colaborador/mes/ano/data/tipo/versao`, campos de `CAMPOS_DIA` | `lavrar`: atribuições `:173`, `:190`, `:234`, `:244`, `:259`, `:269`, `:274`, `:288`, persistência `:345`–`:360` | `linhas_do_periodo`: `app/ponto/services/dia_pago.py:855`; `rubricas_do_dia`: `:914`; `pago_do_dia`: `:890` |
| `DiaPago.minutos_abonados/minutos_previstos/minutos_realizados` | `por_dia_da_grade` calcula mapas: `app/ponto/services/dia_pago.py:364`; `lavrar` incorpora: `:288`; `retratar_exportada` atualiza: `:481`, `:495`, `:538` | `soma_do_periodo`: `app/ponto/services/dia_pago.py:784` |
| `DiaPago.detalhe.folga_feriado_min` | `app/ponto/services/dia_pago.py:257` | `pago_do_dia`: `app/ponto/services/dia_pago.py:897` |
| `DiaPago.horas_reflexo_dsr` | Ajuste `data=None`: `app/ponto/services/dia_pago.py:350`; retrato exportada: `:506`, `:516`, `:542` | `soma_da_competencia`: `app/ponto/services/dia_pago.py:550` |
| `FechamentoMensal` existência/chaves | `get_or_create`: `app/ponto/services/fechamento.py:898`; persistência `:1144` | `soma_do_periodo`: `app/ponto/services/dia_pago.py:805`; TXT: `app/folha/export.py:719` |
| `FechamentoMensal.semanas_dsr_ok/semanas_dsr_perdido/horas_reflexo_dsr` | `app/ponto/services/fechamento.py:1101` | Reflexo copiado ao retrato: `app/ponto/services/dia_pago.py:506`; contador: `app/inteligencia/views.py:119` |

Sem fechamento deve ser `sem apuracao ainda`, sem linha com fechamento deve ser `sem lavratura`; nenhum desses silêncios deve virar zero. (`app/ponto/services/dia_pago.py:761`; `app/ponto/services/dia_pago.py:805`)

O DSR é semanal no motor e o reflexo é ajuste da competência sem data; não pode ser rateado aos dias nem contado repetidamente por cada dia do recorte. (`app/ponto/motor_calculo_v2.py:2590`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/motor_calculo_v2.py:3460`; `app/ponto/services/dia_pago.py:348`; `app/ponto/services/dia_pago.py:755`)

Não foi encontrada API real de contrafactual completo que receba “este dia passa a falta/abono” e devolva simultaneamente minutos e DSR; fechamento dessa ausência em toda a árvore: [nao verificado]. As funções próximas não fazem essa pergunta:

- `minutos_de_falta_por_dia` lê ausências já gravadas; `_rateio_por_dia` distribui minutos de uma ausência recebida. (`app/folha/export.py:125`; `app/folha/export.py:95`)
- `por_dia_da_grade` calcula abono de grade já julgada. (`app/ponto/services/dia_pago.py:364`)
- `efeitos_da_ausencia` consulta as ausências/justificativas atuais. (`app/ponto/services/efeito_ausencia.py:28`; `app/ponto/services/efeito_ausencia.py:40`; `app/ponto/services/efeito_ausencia.py:53`)
- `veredito_celula` cria ausência e lavra o dia; é escritor. (`app/ponto/services/veredito_celula.py:331`; `app/ponto/services/veredito_celula.py:337`; `app/ponto/services/veredito_celula.py:352`)
- `simular_dia` oferece grade, teste de coerência e flips em memória, sem simulação de ausência/DSR. (`app/ponto/services/simular_dia.py:5`; `app/ponto/services/simular_dia.py:11`; `app/ponto/services/simular_dia.py:26`)

Recomendação: imprimir os campos pagos separadamente em minutos, mantendo os campos `minutos_*` na unidade nativa; não somar adicional noturno, extras/subsets e trabalho numa quantidade única de “minutos pagos”. Os subsets noturnos/feriado já pertencem aos totais. (`app/ponto/services/dia_pago.py:34`; `app/ponto/services/dia_pago.py:904`; `app/folha/export.py:424`)

## 15/11 00:43 · sessao e11c49530784 · - · - · origem=Ronald (tmux)

Funções reais encontradas, sem executar banco nem gravar arquivos [nao verificado]:

| Pergunta | Função real e limite |
|---|---|
| Previsto diário | `escala.utils.minutos_previstos_do_dia(colab, data, celulas=None, folgas=None, escalas=None)`; seleciona vínculo, pede dia de trabalho, marcos e pausas. `app/escala/utils.py:2230`, `:2260`, `:2266`, `:2269`, `:2291`. |
| Abono efetivo já decidido na grade | `ponto.services.dia_pago.por_dia_da_grade(grade)` devolve mapas `(abono, previsto, realizado)`; usa `tipo_dia`, `minutos_previstos` e `minutos_realizados`; cobre somente a parcela prevista não realizada. `app/ponto/services/dia_pago.py:364`, `:387`, `:393`, `:395`, `:402`. |
| Ausência real → justificadas/suprimidas | `efeitos_da_ausencia(colaborador, data_ini, data_fim)` consulta justificativas e ausências reais; não aceita cenário hipotético. `app/ponto/services/efeito_ausencia.py:28`, `:40`, `:47`, `:53`, `:59`. |
| Falta decidida em minutos por dia | `folha.export.minutos_de_falta_por_dia` consulta ausências reais que descontam e chama `_rateio_por_dia`; não calcula “tornar este dia falta”. `app/folha/export.py:125`, `:138`, `:140`. |
| DSR | `MotorBase.calcular_mes` calcula faltas e direito por semana; reflexo também usa dados semanais. `app/ponto/motor_calculo_v2.py:2265`, `:2593`, `:2600`, `:2629`, `:2636`, `:2651`, `:2655`. `MotorComercial.calcular_mes` recompõe DSR próprio. `app/ponto/motor_calculo_v2.py:3308`, `:3460`. |
| Leitura mensal recalculada | `fechamento_lido` chama a função de fechamento com `somente_leitura=True`; sua interface não recebe cenário. `app/ponto/services/fechamento.py:1735`, `:1759`. |
| Ato que transforma em falta/justificado | `veredito_celula` chama `criar_ausencia`, fecha fio e lavra célula; é escritor, inadequado à sonda. `app/ponto/services/veredito_celula.py:332`, `:337`, `:347`, `:352`. |
| Simulação já existente | `simular_dia.grade_dia` gera grade sobre batidas; `com_flips` simula inversões. Não fornece cenário de ausência/abono/DSR nessas interfaces. `app/ponto/services/simular_dia.py:5`, `:26`. |

Não identifiquei função real pronta que receba “este dia passa a ausência descontante/abonado” e devolva o delta completo em minutos e DSR; existência em outros caminhos permanece [nao verificado]. A montagem dos conjuntos que o motor recebe está dentro do fechamento, ligada à cobertura real das ausências; substituí-la na sonda criaria interpretação própria do cenário. `app/ponto/services/fechamento.py:660`, `:683`, `:693`, `:724`, `:739`.

Deltas individuais de DSR não podem ser somados como parcelas independentes: o cálculo decide a semana com base no conjunto de faltas e acumula semanas perdidas, sem atribuir uma parcela a cada dia. `app/ponto/motor_calculo_v2.py:2587`, `:2600`, `:2605`, `:2655`.

Para cumprir literalmente o pedido sem criar regra, imprimir `sem funcao real` para os contrafactuais completos; “manter” pode continuar exibindo a leitura existente, mas uma API específica de delta desse cenário não foi identificada [nao verificado].

## 15/11 00:43 · sessao 1b274078e0f5 · - · - · origem=Ronald (tmux)

Referências e decisões para a sonda:

- `EscalaColaborador` não tem campo de criação ou alteração. A classe começa em `app/escala/models.py:871` e termina antes de `AgendaDia`, em `app/escala/models.py:1399`; seus únicos `DateTimeField` são `regua_avaliada_em` (`:884`) e `regua_corroborada_em` (`:888`), que não representam criação/alteração. Emitir `campo_criacao_alteracao=null`, sem substituir por datas de régua.
- Perguntar desligamento a `esta_desligado(colaborador)`, implementada em `app/colaboradores/services/desligamento.py:23-32` e registrada em `app/core/juizes.py:271`. Ela compara o estado gravado com `'desligado'`. Não rotular automaticamente a resposta falsa como “ativo”: há também `afastado` e `ferias` em `app/colaboradores/models.py:295-300`. Emitir o booleano do juiz e `situacao` real.
- Competência de uma data: `ponto.janelas.janela_atual(data, empresa)`, em `app/ponto/janelas.py:60-63`. O mês/ano são os da data final da janela, conforme `periodo_apuracao`, em `app/ponto/janelas.py:10-21`. Para um intervalo, iterar blocos de competência avançando do fim da janela para o dia seguinte.
- Classificação exportada/aberta: chamar `competencia_entregue(empresa_id, mes, ano)`, em `app/folha/export.py:556-594`, registrada em `app/core/juizes.py:337`. Ela exige uma `ExportacaoDominio` vigente e não invalidada; não substituir por `FechamentoMensal.status`.
- Batidas: `batidas_apuraveis` filtra timestamps com `>= ini` e `<= fim`, em `app/ponto/turnos.py:2875-2883`. Passar datas simples perderia as horas do último dia. Usar limites conscientes de fuso: meia-noite inicial e último microssegundo do dia final; ou chamar o juiz com início consciente e acrescentar `timestamp__lt` à meia-noite do dia seguinte ao final.
- Outro vínculo cobrindo todo o intervalo: usar a função real `vinculo_cobre_a_data(other, lo)` e a mesma para `hi`; seu predicado contínuo está em `app/escala/alimentacao.py:214-216`. Isso verifica existência de um vínculo individual cobrindo ambas as pontas, distinto de união de vários vínculos.
- Há precedente de `execute_wrapper` que aceita somente SELECT e recusa CTE, múltiplos comandos, `INTO` e palavras de escrita/efeito em `app/chamados/management/commands/diff_e3_sombra.py:30-38`; instalação em todos os aliases, `:92-95`. Não imprimir SQL ou parâmetros em erro.
- Backend é `django_tenants.postgresql_backend`, em `app/config/settings/saas.py:85`; o envelope invoca `tenant_command shell --schema=juliani`, em `bin/sonda_leitura.sh:239-240`. O comportamento interno de `SET search_path` está em dependência externa, ausente da cópia: `[não verificado]`.
- Não executar o envelope nesta sessão: embora o corpo da sonda possa usar somente SELECT, o envelope reescreve credencial/role (`bin/sonda_leitura.sh:126-143`), concede permissões (`:157-181`), cria temporário (`:216-218`) e grava trilha (`:248-250`). Entregar somente o comando futuro como texto.

## 15/11 00:43 · sessao 863362a85a00 · - · - · origem=Ronald (tmux)

- E4B real é `backfill_fio_morto_e4b`. O alcance básico é `censo_alcance` filtrado por `rota == 'sem_porta'`; unidade dia-colaborador, não pergunta. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:79`; `app/chamados/services/vigia_fio.py:549`)
- Esse universo percorre competências de `competencias_a_lavrar`, atual e anterior ainda não entregue, via `dias_que_seguram`; não representa todo histórico exportado. (`app/chamados/services/vigia_fio.py:505`, `:584`, `:590`)
- O complemento exportado efetivamente declarado é `_exportadas_classe_e(empresas, hoje)`: perguntas das empresas visíveis em `[hoje−45, hoje)`, competência não explicitamente aberta e `reabrir_credor_de_pergunta_viva(..., executar=False) != None`; deduplica por `(colaborador_id, data_evento)`. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:153`, `:164`, `:166`, `:173`, `:175`, `:178`)
- Esse planejamento inclui recusas; não significa “aplicaria”. A porta calcula `credor_toca_exportada` e limbo e, com `executar=False`, retorna `recusa:<motivo>` ou `reabriria`, sem executar reabertura. (`app/chamados/services/disputa_emissao.py:1842`, `:1846`, `:1856`)
- `ato_do_dia` é seleção real de leitura das portas por dia: `reabrir_credor`, `registrar_decisao`, `perguntar`, `materializar`, `rejulgar`; usa a tarefa real e o plano de reabertura. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:82`, `:87`, `:90`, `:97`, `:109`)
- Para “projeção” há função de alcance pronta: `backfill_decisoes_pergunta.linhas_a_criar()`, retornando `{(empresa_id, mes, ano): {'resposta': [(cid, pk)], 'validacao': [...]}}`. Seu universo declarado é pergunta com R ou V não nulo e `q_do_horizonte_ou_sem_dia() OR conversa`; conversa e dia nulo vão a baldes sem competência de fato. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:55`, `:56`, `:59`, `:73`, `:75`, `:79`, `:82`)
- A projeção produz decisões `RESPOSTA` com `pergunta_id/ato/ato_em/ato_texto/ato_motivo`, e `VALIDACAO` com esses campos mais `ato_por_id`. Só correspondência exata `(pergunta_id, ato, ato_em)` evita nova linha. Escritor factual é `DecisaoPergunta.registrar`, via `bulk_create`. (`app/chamados/models.py:4070`, `:4074`, `:4076`, `:4078`, `:4080`, `:4086`, `:4096`)
- O comando E1 apenas mede essas linhas; seu apply escreve itens 2 e 4, não projeção. O E4B pode chamar o executor de projeção por `registrar_decisao`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `:6`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:138`, `:141`)
- O anexo `fatias.md` descreve troca de leitores e não identifica operação que aplique a “fatia de projeção”; atribuir automaticamente o alcance E1 acima à E4c é **[nao verificado]**.
- A função real dia→competência entregue é `competencia_do_dia_entregue`: `janela_atual` → `competencia_entregue`; dia nulo retorna `None`, não aberta. (`app/chamados/services/vigia_fio.py:843`, `:855`, `:859`, `:862`)
- Envelope esperado: `bin/sonda_leitura.sh --sombra <arquivo.py>`; utiliza settings de sombra e shell Django `tenant_command --schema=juliani`. O próprio envelope cria/regrava arquivos e infraestrutura; nesta tarefa somente seu texto deve ser entregue. (`bin/sonda_leitura.sh:56`, `:94`, `:126`, `:134`, `:216`, `:239`, `:249`)

## 15/11 00:43 · sessao b34c0ce428a2 · - · - · origem=Ronald (tmux)

- **Juiz de exportada:** `competencia_entregue(empresa_id, mes, ano, registro=False, *, exportacoes=None)`, em `app/folha/export.py:556`. Lê exportações da tripla, aplica `anotar_vigencia`/`so_vigentes` e aceita o vigente somente com `invalidada_em is None`, em `app/folha/export.py:580`. Não equivale a `.exists()` sobre `ExportacaoDominio`, em `app/folha/export.py:591`.

- **TXT a partir do gravado:** `montar_linhas_export` seleciona `FechamentoMensal` da empresa/competência com `status` aberto/aprovado, em `app/folha/export.py:717`; consulta a classificação dos colaboradores em `app/folha/export.py:714`. Os valores das linhas vêm de `eventos_do_fechamento`, em `app/folha/export.py:730`.

- **Campos monetários lidos:** `horas_extras_100_feriado`, `horas_extras_100`, `horas_extras_50`, `horas_folga_trabalhada`, `horas_noturnas`, `horas_intra_indenizada`, `horas_atraso` e `horas_saida_antecipada`, em `app/folha/export.py:420` e `app/folha/export.py:439`. Mapeamento evento→rubrica: `RubricaDominio` ativa da empresa sobre a global, em `app/folha/export.py:984`.

- **Faltas no TXT:** datas vêm das ausências aprovadas cujo efeito vigente desconta, por `datas_falta` → `datas_falta_janela`, em `app/folha/export.py:184` e `app/folha/export.py:169`; entram no bloco de falta em `app/folha/export.py:740`. `minutos_de_falta_por_dia` também fornece os minutos usados pela derivação de `horas_falta`, em `app/folha/export.py:125` e `app/ponto/services/fechamento.py:873`.

- **Nenhuma leitura direta de R/V/C/recusa da pergunta** foi encontrada em `folha/export.py`; a montagem lê os campos de FM e as autoridades de ausência acima. Exaustividade transitiva por execução dinâmica: **[nao verificado]**.

- **ExportacaoDominio:** guarda a cópia do TXT em `conteudo`, `hash_sha256` e `total_linhas`, em `app/folha/models.py:82`; `registrar` recebe o conteúdo pronto, calcula seu hash e cria um registro histórico, em `app/folha/models.py:174`. Não deriva minutos ou valores de perguntas, em `app/folha/models.py:176`.

- **Comparação real, sem escrita:** `txt_vigente_x_gravado` obtém o vigente pelo juiz e compara seu hash com o TXT montado do gravado atual, em `app/folha/export.py:655`. Isso mede divergência atual; não mede o efeito hipotético de A18/A19, em `app/folha/export.py:657`.

- **Derivação real em leitura:** `fechamento_lido` chama `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)`, em `app/ponto/services/fechamento.py:1759`. O objeto é destacado, sem persistência de FM, em `app/ponto/services/fechamento.py:890` e `app/ponto/services/fechamento.py:1128`. Não passar `lavrar_dias_pagos=True`: essa combinação escreve `DiaPago`, em `app/ponto/services/fechamento.py:1191`. Comparar esse objeto com o gravado mede defasagem atual, não o delta atribuível a uma operação hipotética específica: **[nao verificado]**.

- **Possível ambiguidade a registrar:** a árvore chama os “itens 18–19” da E4B de “FIM DO PROGRAMA”, em `app/docs/BACKLOG.md:1977`; a grade registra “itens 18–19; autorização das exportadas”, em `app/docs/GRADE-MESTRA.md:717`. Portanto não equiparar silenciosamente esses itens às portas **A18/A19** da matriz. O enunciado anexado aponta explicitamente para A18/A19; correspondência dos dois vocabulários: **[nao verificado]**.

- **Outro alcance real encontrado:** `projecoes_vencidas_qs(mes, ano, empresa_id)` seleciona FM com `derivado_em=NULL`, em `app/ponto/services/fechamento.py:1668` e `app/ponto/services/fechamento.py:1696`. Sem mês/ano usa a competência corrente de cada empresa, em `app/ponto/services/fechamento.py:1692`. Não há evidência de que esse alcance seja a “fatia de projeção da E4c” do anexo: **[nao verificado]**.

## 15/11 00:43 · sessao 31ff7cb1277f · - · - · origem=Ronald (tmux)

A18/A19 foram localizadas; os endereços do anexo mudaram na cópia.

- **A18 técnica:** `_carimbar_recusa` em `app/chamados/services/materializacao.py:1089`. Grava `PerguntaDisputa.recusa_motivo` em `:1108`/`:1109`; colisão sem carimbo delega a `carimbar_identidade` em `:1105`. Essa porta grava `materializada_em`, `materializada_por_id`, `via_resolucao` (`app/chamados/models.py:3182`, `:3183`, `:3184`, `:3268`, `:3269`) e, quando passado, `recusa_motivo` (`:3270`–`:3273`). O resultado técnico produz nota `recusa_materializacao` (`materializacao.py:1113`), via `FollowupChamado.objects.create` (`app/chamados/services/nota.py:88`, `:103`, `:107`). Não cria DP de recusa humana nesse corpo (`materializacao.py:1089`).
- **A19 humana:** `recusar_pergunta` em `app/chamados/services/validacao.py:562`. Grava `PerguntaDisputa.recusa_motivo` (`:619`, `:625`), chama `reabrir_para_correcao(..., ato='recusa')` (`:628`). Esta prepara limpeza de `respondida_em`, `validada_em`, `validada_por`, `resposta_colab` (`app/chamados/services/disputa_emissao.py:1595`), registra DP com `pergunta_id`, `ato`, `ato_texto`, foto anterior R/V/autor, `ato_em`, `ato_por`, `ato_motivo` (`:1617`–`:1623`) e delega limpeza adicional de C/via (`:1634`; `app/chamados/models.py:3363`–`:3365`). Acorda células (`disputa_emissao.py:1635`), produz nota/auditoria/push (`validacao.py:633`, `:637`, `:643`, `:648`).
- **Alcance A18/A19:** nenhuma das duas funções expõe modo de planejamento em leitura (`materializacao.py:1089`; `validacao.py:562`). Selecionar candidatas copiando suas guardas não seria chamar alcance real. A sonda deve contar `sem alcance chamavel`; número de linhas que efetivamente tocariam: **[nao verificado]**.

A **projeção legada** dispõe de alcance leitor real: `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `linhas_a_criar()`. Retorna mapa por `(empresa_id, mês, ano)` com atos `resposta`/`validacao` e pares `(colaborador_id, pergunta_pk)` (`:45`, `:82`, `:91`). Seu universo é pergunta com R ou V, no horizonte canônico ou sem dia, mais conversa (`:55`–`:61`). Conversa e dia ausente são baldes próprios (`:73`–`:76`); competência factual vem de `competencia_do_fato` (`:79`).

O produtor canônico é `DecisaoPergunta.linhas_da_projecao_sem_linha` (`app/chamados/models.py:4057`), com correspondência pergunta+ato+instante (`:4070`, `:4074`, `:4078`). Produziria novas DP com:

- Resposta: `pergunta_id`, `ato`, `ato_em`, `ato_texto`, `ato_motivo` (`:4076`, `:4077`).
- Validação: os mesmos campos, mais `ato_por_id` (`:4080`–`:4082`).

Executor: `DecisaoPergunta.registrar` (`models.py:4086`), inserção em `:4096`, seguida de eventos das células em `:4097`. O comando existente declara que **não aplica** essa fatia (`backfill_decisoes_pergunta.py:3`–`:6`). Identificar essa operação como a “fatia de projeção da E4c” a partir somente do `fatias.md` fornecido: **[nao verificado]**.

## 15/11 00:35 · sessao 9fef593b873d · A20-E-COMBOIO · leitura · origem=Code

**O par conferido tem fonte histórica no código: `LogAuditoria`.** A existência e a integridade dessa trilha em produção, assim como os números pedidos, estão **[nao verificado]**. O payload é escrito em `app/chamados/services/validacao.py:702-707`.

**(a)** Os campos pertencem a `PerguntaDisputa`: `data_conferida_em` é `DateTimeField` anulável; `data_conferida_por` é FK anulável para `auth.User`, com `SET_NULL` (`app/chamados/models.py:2536-2540`).

O único escritor explícito atual da conferência é `conferir_data_do_fato`: atribui `timezone.now()` e `autor`, salvando somente esses dois campos. O contrato de escritores declara esse mesmo endereço; a view apenas verifica POST/permissão e delega (`app/chamados/services/validacao.py:699-701`; `app/core/tests/test_contract_escritores_estado.py:113-117`; `app/chamados/views.py:1306-1314`). A FK também admite nulificação pela exclusão do usuário, por `SET_NULL` (`app/chamados/models.py:2538`).

A porta exige justificativa, lê o par vigente, recusa concordância e recusa novo carimbo se `data_conferida_em` já estiver preenchida (`app/chamados/services/validacao.py:689-698`). Após salvar, solicita a auditoria com este payload (`app/chamados/services/validacao.py:702-707`):

```python
acao = 'conferir_data'
modelo = 'PerguntaDisputa'
objeto_id = p.pk
descricao = (
    'Data do fato conferida: vale %s (o chamado dizia %s). %s'
    % (ev, turno, just)
)
valor_antes = {'data_turno': turno.isoformat() if turno else None}
valor_depois = {
    'data_evento': ev.isoformat() if ev else None,
    'justificativa': just,
}
```

Portanto, o par histórico ordenado é **`(valor_depois['data_evento'], valor_antes['data_turno'])`**. Esses JSONs são encaminhados à gravação de `LogAuditoria`; a auditoria é *best-effort*, podendo falhar depois do carimbo (`app/core/auditoria.py:22-29`; `app/core/observ.py:105-139`; `app/chamados/services/validacao.py:701-707`).

`DecisaoPergunta` não possui ato de conferência nem campos dedicados ao par; a porta de conferência não registra decisão nessa sequência (`app/chamados/models.py:4000-4027`; `app/chamados/services/validacao.py:683-710`). Assim, **não cabe afirmar que o par nunca foi registrado**: existe a trilha estruturada acima. Sua cobertura real está **[nao verificado]**.

**(b)** O par vigente é **`(ev, turno)` retornado por `data_do_fato_de(p)`**: `ev = p.data_evento`; `turno = dia_do_fato(chamado_que_lista(p), fortes=True)` (`app/chamados/services/fio.py:77-88`). `data_evento` é campo anulável da pergunta (`app/chamados/models.py:2501-2504`).

O chamado considerado é o destino, com fallback para o chamado da disputa. Seu dia forte prioriza `chamado.celula.data`, depois a chave do catálogo e o fallback `contexto_json['data_turno']`; sem dia forte retorna `None` (`app/chamados/services/fio.py:116-117`; `app/chamados/juizes.py:98-128`). Logo, ler apenas o contexto bruto não reproduz a função real (`app/chamados/services/fio.py:84-88`).

O materializador lê essa função, mas sua liberação atual consulta apenas a presença do carimbo: `_diverge and p.data_conferida_em is None` (`app/chamados/services/materializacao.py:1197-1199`).

**(c)** Cabe usar **`competencia_do_dia_entregue(p.data_evento, empresa, memo)`**: ela compõe `janela_atual` com `competencia_entregue`, usando mês/ano do **fim da janela da empresa**, e retorna `None` sem data (`app/chamados/services/vigia_fio.py:843-865`; `app/ponto/janelas.py:60-63`).

`True` identifica exportada; `False`, aberta pelo critério K6 — exportação vigente e não invalidada. Isso não depende do estado aberto/fechado da disputa (`app/folha/export.py:580-594`). A sonda preserva `sem_data` como terceira parte, conforme o anexo fornecido **[nao verificado]**.

**(d)** Texto proposto, sem execução em produção **[nao verificado]**. A recuperação será conservadora: exige datas ISO completas e um único par histórico distinto; trilhas repetidas com o mesmo par são aceitas. Ausência, payload inválido ou pares conflitantes entram em `NAO_RECUPERAVEL`. A comparação usa a tupla vigente inteira, inclusive eventual componente `None`; não usa o booleano de divergência entre os dois dias atuais. Essas são regras explícitas desta sonda proposta **[nao verificado]**.

```python
# A20-E-COMBOIO item (1): texto proposto; execucao [nao verificado].
# Para o mesmo envelope Django/tenant --sombra do anexo.
#
# Universo: PerguntaDisputa.data_conferida_em preenchida.
# Fonte: app/chamados/models.py:2536.
# Par historico: LogAuditoria conferir_data.
# Fonte: app/chamados/services/validacao.py:702-707.
# Par vigente: data_do_fato_de(p), componentes 0 e 2.
# Fonte: app/chamados/services/fio.py:77-88.
# Particao: competencia_do_dia_entregue sobre data_evento vigente.
# Fonte: app/chamados/services/vigia_fio.py:843-865.
#
# Unidade: pergunta; nenhuma chamada a portas de escrita.
# Saida: contagens, #pk e emp; sem descricao/justificativa/datas/pessoas.

import datetime
import io
import logging
import re
from collections import Counter, defaultdict
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from itertools import islice

from django.db import connections

linhas = []
erros = 0
bloqueios = 0
universo = None
total = 0
completo = False
nivel_anterior = logging.root.manager.disable

estados = ('DIFERENTE', 'IGUAL', 'NAO_RECUPERAVEL')
particoes = ('exportada', 'aberta', 'sem_data')
parte = Counter()       # (estado, particao) -> perguntas
por_emp = Counter()     # (empresa_id, estado, particao) -> perguntas
motivos_nr = Counter()
exemplos = defaultdict(list)
memo = {}


def somente_select(execute, sql, params, many, context):
    global bloqueios
    texto = sql.strip()
    if (many or not re.match(r"(?is)^SELECT\b", texto) or ";" in texto
            or re.search(r"(?is)\bINTO\b|\bFOR\s+(UPDATE|SHARE)\b",
                         texto)):
        bloqueios += 1
        raise RuntimeError("SQL bloqueado")
    return execute(sql, params, many, context)


def dia_iso(valor):
    # Nulo, chave ausente e formato inesperado nao viram data presumida.
    if not isinstance(valor, str):
        raise ValueError("dia invalido")
    dia = datetime.date.fromisoformat(valor)
    if dia.isoformat() != valor:
        raise ValueError("dia nao canonico")
    return dia


try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
        with ExitStack() as pilha:
            for alias in connections:
                pilha.enter_context(
                    connections[alias].execute_wrapper(somente_select)
                )

            from chamados.models import PerguntaDisputa
            from chamados.services.fio import data_do_fato_de
            from chamados.services.vigia_fio import (
                competencia_do_dia_entregue,
            )
            from core.models import LogAuditoria

            qs = (
                PerguntaDisputa.objects
                .filter(data_conferida_em__isnull=False)
                .select_related(
                    'colaborador',
                    'chamado_destino__celula',
                    'disputa__chamado__celula',
                )
                .order_by('pk')
            )
            universo = qs.count()
            it = qs.iterator(chunk_size=500)

            while True:
                lote = list(islice(it, 500))
                if not lote:
                    break

                ids = [p.pk for p in lote]
                vistos = Counter()
                pares = defaultdict(set)
                invalidos = set()

                # Le somente identificador e os dois componentes JSON.
                # Nao carrega descricao nem justificativa.
                logs = (
                    LogAuditoria.objects
                    .filter(
                        acao='conferir_data',
                        modelo='PerguntaDisputa',
                        objeto_id__in=ids,
                    )
                    .order_by('pk')
                    .values_list(
                        'objeto_id',
                        'valor_antes__data_turno',
                        'valor_depois__data_evento',
                    )
                )

                for pid, turno_antigo, evento_antigo in logs.iterator(
                        chunk_size=2000):
                    vistos[pid] += 1
                    try:
                        par = (
                            dia_iso(evento_antigo),
                            dia_iso(turno_antigo),
                        )
                    except (TypeError, ValueError, OverflowError):
                        invalidos.add(pid)
                    else:
                        pares[pid].add(par)

                for p in lote:
                    ev, _diverge, turno = data_do_fato_de(p)
                    vigente = (ev, turno)
                    emp = p.colaborador.empresa_id

                    entregue = competencia_do_dia_entregue(
                        ev, emp, memo
                    )
                    if entregue is True:
                        k = 'exportada'
                    elif entregue is False:
                        k = 'aberta'
                    elif entregue is None:
                        k = 'sem_data'
                    else:
                        raise RuntimeError("retorno de particao invalido")

                    motivo = None
                    if vistos[p.pk] == 0:
                        motivo = 'sem_trilha'
                    elif p.pk in invalidos:
                        motivo = 'payload_invalido'
                    elif len(pares[p.pk]) != 1:
                        motivo = 'pares_conflitantes'

                    if motivo is not None:
                        estado = 'NAO_RECUPERAVEL'
                        motivos_nr[motivo] += 1
                    else:
                        conferido = next(iter(pares[p.pk]))
                        estado = (
                            'DIFERENTE'
                            if vigente != conferido
                            else 'IGUAL'
                        )

                    parte[(estado, k)] += 1
                    por_emp[(emp, estado, k)] += 1
                    chave = (estado, k)
                    if len(exemplos[chave]) < 10:
                        exemplos[chave].append(p.pk)
                    total += 1

            soma_partes = sum(parte.values())
            soma_empresas = sum(por_emp.values())

            if not (total == universo == soma_partes == soma_empresas):
                raise RuntimeError("soma do universo nao confere")

            for estado in estados:
                for k in particoes:
                    soma_emp = sum(
                        n for (emp, e, particao), n in por_emp.items()
                        if e == estado and particao == k
                    )
                    if soma_emp != parte[(estado, k)]:
                        raise RuntimeError("soma por empresa nao confere")

            if sum(motivos_nr.values()) != sum(
                    parte[('NAO_RECUPERAVEL', k)]
                    for k in particoes):
                raise RuntimeError("soma dos motivos nao confere")

            if bloqueios:
                raise RuntimeError("houve SQL bloqueado")

            linhas.append(
                f"UNIVERSO|perguntas_com_data_conferida_em={universo}"
            )

            for estado in estados:
                for k in particoes:
                    pks = ','.join(
                        f"#{pk}" for pk in exemplos[(estado, k)]
                    )
                    linhas.append(
                        f"PARTE|{estado}|{k}"
                        f"|perguntas={parte[(estado, k)]}"
                        f"|exemplos={pks}"
                    )

                n = sum(parte[(estado, k)] for k in particoes)
                n_emp = sum(
                    v for (emp, e, k), v in por_emp.items()
                    if e == estado
                )
                linhas.append(
                    f"TOTAL|{estado}|perguntas={n}"
                    f"|soma_das_partes={n}"
                    f"|soma_empresas={n_emp}|bate={n == n_emp}"
                )

            empresas = sorted({emp for emp, e, k in por_emp})
            for emp in empresas:
                for estado in estados:
                    for k in particoes:
                        linhas.append(
                            f"EMPRESA|emp{emp}|{estado}|{k}"
                            f"|perguntas={por_emp[(emp, estado, k)]}"
                        )

            nr = sum(
                parte[('NAO_RECUPERAVEL', k)] for k in particoes
            )
            linhas.append(
                f"NAO_RECUPERAVEL|perguntas={nr}"
                f"|perguntas_com_data_conferida_em={universo}"
            )
            for motivo, n in sorted(motivos_nr.items()):
                linhas.append(
                    f"MOTIVO_NR|{motivo}|perguntas={n}"
                )

            completo = True

except Exception as exc:
    erros += 1
    # Nao imprime mensagem da excecao: pode conter dados/SQL/parametros.
    linhas = [f"ERRO|tipo={type(exc).__name__}"]

finally:
    logging.disable(nivel_anterior)

soma_partes = sum(parte.values())
soma_empresas = sum(por_emp.values())
diferentes = sum(parte[('DIFERENTE', k)] for k in particoes)
soma_emp_diferentes = sum(
    n for (emp, estado, k), n in por_emp.items()
    if estado == 'DIFERENTE'
)
nr = sum(parte[('NAO_RECUPERAVEL', k)] for k in particoes)

bate = (
    completo
    and erros == 0
    and bloqueios == 0
    and universo == total == soma_partes == soma_empresas
    and diferentes == soma_emp_diferentes
)
medicao_integral = bate and nr == 0

for linha in linhas:
    print(linha)

print(
    f"FIM|erros={erros + bloqueios}"
    f"|sql_bloqueados={bloqueios}"
    f"|completo={completo}"
    f"|universo={universo}"
    f"|total={total}"
    f"|soma_das_partes={soma_partes}"
    f"|soma_empresas={soma_empresas}"
    f"|diferentes={diferentes}"
    f"|soma_empresas_diferentes={soma_emp_diferentes}"
    f"|nao_recuperavel={nr}"
    f"|bate={bate}"
    f"|medicao_integral={medicao_integral}"
)
```

Com `NAO_RECUPERAVEL > 0`, `DIFERENTE` informa apenas os casos demonstrados pela trilha recuperada; o total exato pedido permanece **[nao verificado]**.

_5448 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
