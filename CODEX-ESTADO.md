# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 16:05** · publicado em **05/11/2026 16:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 16:05 · sessao 0afe9d59238c · MARCO-CLEAR · censo · origem=Code

O Stop atual **consome um marco já gravado**, mas ainda não identifica eventos no transcript para chamar o gravador. O gravador atual usa a hora da execução, enquanto o contrato pede a hora do evento. (`bin/hook_stop_fila1.py:862-904`; `bin/marco_de_clear.py:43-45`; `contrato.txt:4`.)

As referências `contrato.txt:1-8` abaixo numeram as linhas do anexo fornecido, contando o cabeçalho como linha 1. Presença efetiva dos campos no payload de produção e execução real dos hooks: **[nao verificado]**.

**1. Censo de `ultimo_clear.json` e seus campos**

O escritor operacional é [bin/marco_de_clear.py](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/bin/marco_de_clear.py:23): monta o objeto, preserva o histórico de clears sob `flock` e publica com `os.replace`. Os chamadores configurados são `SessionStart(compact)` e `SessionStart(clear)`. (`bin/marco_de_clear.py:32-61`; `.claude/settings.json:34-59`.)

| Campo | Quem escreve e de onde vem | Quem lê e o que faz |
|---|---|---|
| `hora` | Gravador: `datetime.now().timestamp()`, dentro da trava. (`bin/marco_de_clear.py:38-44`.) | `ler_marco` converte para float e compõe `ref`; `avaliar` usa como base de horas/deploys; `ler` usa para o ESTADO. (`bin/orcamento_contexto.py:158-161`, `:329-337`, `:374-377`.) |
| `iso` | Gravador: texto local, sem offset, `%Y-%m-%d %H:%M:%S`. (`bin/marco_de_clear.py:44`.) | `ler_marco` não o repassa. Outro consumidor operacional: **[nao verificado]**. (`bin/orcamento_contexto.py:160-162`.) |
| `origem` | Primeiro argumento; na ausência, `payload.source` ou `?`. (`bin/marco_de_clear.py:32`.) | Identifica a base da avaliação; no Stop, compõe RESET e seleciona/rotula o piso. (`bin/orcamento_contexto.py:160`, `:331`; `bin/hook_stop_fila1.py:889-903`.) |
| `session_id` | Payload, convertido em string; ausente vira vazio. (`bin/marco_de_clear.py:45`.) | Entra em `ref=hora:session_id`; Stop compara essa referência com `est.marco_ref`. Não compara com a sessão do payload Stop. (`bin/orcamento_contexto.py:160-161`; `bin/hook_stop_fila1.py:863-870`, `:1187-1190`.) |
| `source` | Payload, convertido em string; ausente vira vazio. (`bin/marco_de_clear.py:45`.) | `ler_marco` não o repassa. Outro consumidor operacional: **[nao verificado]**. (`bin/orcamento_contexto.py:160-162`.) |
| `clear_horas` | Gravador relê o anterior, conserva números, acrescenta `hora` quando `origem=clear` e mantém os últimos 20. (`bin/marco_de_clear.py:50-57`.) | `ler_marco` normaliza; Stop procura clears posteriores ao disparo e decide ATENDIDO/SEGUIU pela janela. (`bin/orcamento_contexto.py:159-162`; `bin/hook_stop_fila1.py:877-887`.) |

Os leitores diretos de conteúdo são o próprio gravador, para preservar `clear_horas`, e `orcamento_contexto.ler_marco`. Os consumidores indiretos são Stop, `avaliar`, `ler` para ESTADO e a CLI de orçamento. (`bin/marco_de_clear.py:50-52`; `bin/orcamento_contexto.py:152-164`, `:329`, `:361`, `:401-415`; `bin/hook_stop_fila1.py:862-904`; `bin/gerar_estado.py:274-276`.)

`clear_hora` é derivado como `max(clear_horas)`, não um campo persistido pelo gravador; consumidor adicional desse derivado: **[nao verificado]**. (`bin/orcamento_contexto.py:162`; `bin/marco_de_clear.py:44-57`.)

O disjuntor obtém a última medida de `orcamento_estado.json` e os pisos pelas funções de orçamento; nesses caminhos não consulta o marco. (`bin/disjuntor.py:1594-1619`.)

**2. Censo de `pisos.log` e `cota.json`**

| Arquivo | Escritor e evento | Leitores |
|---|---|---|
| `pisos.log` | `registrar_piso`, append. O **Stop** chama quando há `piso_pendente` e consegue medir tokens positivos; grava a hora do Stop. Também existe semeadura pela CLI `--semear-pisos`, fora de hook. (`bin/orcamento_contexto.py:198-207`, `:295-315`, `:405-410`; `bin/hook_stop_fila1.py:891-903`; `.claude/settings.json:3-9`.) | Direto: `ultimo_piso`. Indiretos: `gatilho_tokens` → avaliação C1; `disjuntor.ler` → `_pisos_txt` → ESTADO; CLI. (`bin/orcamento_contexto.py:210-232`, `:340-345`, `:409`; `bin/disjuntor.py:1614-1616`, `:1669-1674`; `bin/gerar_estado.py:280-281`.) |
| `cota.json` | `statusline_cota.main`, acionado por **statusLine**, não por Stop/SessionStart. Só substitui o arquivo com percentual e reset numéricos finitos; publica por tempfile/replace. (`bin/statusline_cota.py:26-30`, `:40-54`; `.claude/settings.json:74-76`; `bin/disjuntor_settings.json:17-19`.) | Direto: `disjuntor.ler_cota`. Indiretos: `escada`, `b4_decide`, B4 no Stop, restrição de Codex e painel/ESTADO. (`bin/disjuntor.py:1064-1076`, `:1108-1129`, `:1202-1204`, `:1374-1375`, `:1619`; `bin/hook_stop_fila1.py:1043-1044`; `bin/gerar_estado.py:280-281`.) |

`pisos.log` grava quatro colunas: data, hora, origem e tokens. `ultimo_piso` filtra origem, exige tokens com `isdigit` e escolhe a maior dupla textual data/hora. Não recebe sessão nem referência do marco. A semeadura usa o timestamp do primeiro turno posterior ao evento, enquanto o Stop usa seu próprio `agora`. (`bin/orcamento_contexto.py:204-205`, `:214-220`, `:307-314`; `bin/hook_stop_fila1.py:902`.)

| Campo de `cota.json` | Escritor | Leitor de campo |
|---|---|---|
| `seven_day_pct` | `rate_limits.seven_day.used_percentage`. (`bin/statusline_cota.py:45-51`.) | `ler_cota`; retorna percentual para B4/painel. (`bin/disjuntor.py:1069-1074`, `:1111-1129`, `:1619`.) |
| `seven_day_resets_at` | `rate_limits.seven_day.resets_at`. (`bin/statusline_cota.py:45-51`.) | `ler_cota`; reset vencido resulta em `None`. (`bin/disjuntor.py:1069-1073`.) |
| `hora` | `time.time()`. (`bin/statusline_cota.py:51`.) | Não participa da decisão de `ler_cota`; outro consumidor: **[nao verificado]**. (`bin/disjuntor.py:1064-1076`.) |
| `contexto_tokens`, `contexto_pct` | `context_window.total_input_tokens/used_percentage`. (`bin/statusline_cota.py:52-53`.) | Não participam de `ler_cota`; outros consumidores: **[nao verificado]**. (`bin/disjuntor.py:1064-1076`.) |
| `session_id` | Payload da statusline. (`bin/statusline_cota.py:53`.) | Não participa de `ler_cota`; outro consumidor: **[nao verificado]**. (`bin/disjuntor.py:1064-1076`.) |

Há ainda leitura **de metadados** de pisos/cota: `_prova_arquivo` usa `stat` e dispositivo/inode para impedir que esses arquivos provem ESPERANDO, inclusive por hardlink; não interpreta seus campos. (`bin/hook_stop_fila1.py:583-599`.)

Em `app/`, aparecem referências documentais ao instrumento; outro leitor/escritor operacional desses arquivos: **[nao verificado]**. (`app/docs/RELATO.md:809`; `app/docs/BACKLOG.md:1750`.)

**3. Payload e leituras do transcript no Stop**

O Stop aceita JSON no stdin e extrai somente `transcript_path` quando string. As fixtures enviam `session_id` e `transcript_path`; isso não comprova o payload real de produção, que permanece **[nao verificado]**. (`bin/hook_stop_fila1.py:1184-1192`; `bin/tests/test_orcamento_contexto.sh:106-110`; `bin/tests/test_disjuntor.sh:82`.)

As leituras existentes são indiretas:

| Finalidade | Chamada no Stop → leitura |
|---|---|
| Piso | `hook_stop_fila1.py:899` → `orcamento_contexto.py:278-290`, que abre o transcript inteiro em `:240-250`. |
| Contexto | `hook_stop_fila1.py:904` → `orcamento_contexto.py:327`, `:89-133`: últimos 4 MiB, último assistant sem `isSidechain`. |
| Base quando falta marco | `orcamento_contexto.py:333` → primeiro timestamp em `:136-149`. |
| Agentes sem entrega | `hook_stop_fila1.py:792` → `disjuntor.py:366-382`. |
| Última mensagem humana/B4 | `hook_stop_fila1.py:1044` → `disjuntor.py:1204`, `:1114`, `:1079-1105`. |
| Verificadores/B3 | `hook_stop_fila1.py:1048` → transcripts de subagentes em `disjuntor.py:736-750`, `:878`. |

Existe reconhecimento de `/clear` e `compact_boundary` para **medir pisos**, mas não uma decisão que compare esses eventos ao marco e grave outro. A transição atual depende de o `ref` do arquivo já ter mudado. (`bin/orcamento_contexto.py:253-290`; `bin/hook_stop_fila1.py:862-904`.)

**4. C1, ESTADO e marco velho**

C1 é avaliado em `orcamento_contexto.avaliar`, chamado pelo Stop. O limiar é `(último piso compact + 200000)//2`; sem piso compact ou com piso ≥200000, não liga. Só tokens ≥limiar entram na decisão; horas/deploys são medidas. (`bin/hook_stop_fila1.py:904`; `bin/orcamento_contexto.py:55`, `:225-232`, `:327-345`.)

`disjuntor._pisos_txt` apresenta os pisos compact/clear com data e o limiar C1, ou `nao medido`/`NAO LIGA`. (`bin/disjuntor.py:1614-1616`, `:1669-1674`.)

A linha de orçamento do ESTADO chama `orcamento_contexto.ler/linha`: contexto vem da última medida do Stop; horas são recalculadas contra `marco.hora`, ou contra a base salva quando falta marco. Os “marcos” exibidos são **linhas do histórico de deploy**, não eventos clear/compact. (`bin/gerar_estado.py:274-276`; `bin/orcamento_contexto.py:167-192`, `:352-398`.)

Um marco legível, mesmo velho de dias, continua valendo: horas acumulam sob o rótulo “desde o clear”, inclusive quando a origem é compact, sem aviso específico de idade. Pisos também não expiram por idade; o painel continua mostrando a data antiga. Valores concretos atuais: **[nao verificado]**. (`bin/orcamento_contexto.py:210-232`, `:374-398`; `bin/disjuntor.py:1672-1674`.)

**5. Caminhos que o contrato deixa sem definição**

| Caso | Lacuna |
|---|---|
| Duas sessões simultâneas | Comparar `session_id != marco` contra um arquivo global pode produzir alternância A→B→A como sucessivas novidades. O `flock` atual serializa gravações, mas não separa propriedade por sessão. (`contrato.txt:4-6`; `bin/marco_de_clear.py:19-20`, `:38-45`.) |
| Transcript de agente | Não define validação de pertencimento à sessão principal. Stop aceita qualquer string; filtrar linhas `isSidechain` não valida a identidade do arquivo, e o reconhecimento de boundaries nem aplica esse filtro. (`contrato.txt:4-6`; `bin/hook_stop_fila1.py:1187-1190`; `bin/orcamento_contexto.py:112`, `:253-270`.) |
| Compact sem Stop posterior | A dependência do primeiro Stop está declarada, mas “latência = 1 turno” não cobre ausência definitiva desse Stop nem estabelece prazo de gravação. (`contrato.txt:4`, `:6`.) |
| Clear seguido de compact antes do primeiro Stop | Falta definir replay e ordem dos dois eventos, preservação de `clear_horas` e medição dos dois pisos. Hoje o histórico preservado permite ATENDIDO após compact; o Stop mede somente a origem do marco corrente. (`contrato.txt:4-7`; `bin/marco_de_clear.py:52-57`; `bin/hook_stop_fila1.py:877-902`.) |
| Relógio | Falta definir timestamp ausente/inválido/futuro, fuso, regressão e evento antigo descoberto depois. O escritor usa `now`, e as horas resultam de subtração direta, sem correção de regressão. (`contrato.txt:4-6`; `bin/marco_de_clear.py:43-45`; `bin/orcamento_contexto.py:84-86`, `:336`, `:376`.) |
| Idempotência | É exigida, mas falta chave de identidade do evento. O escritor atual lê nova hora em cada chamada e acrescenta outro clear; repetir a chamada não deduplica o evento. (`contrato.txt:6`; `bin/marco_de_clear.py:43-57`.) |

A localização da futura chamada dentro do Stop também falta: hoje há saídas por prova do RELATO e B4 antes do orçamento; colocá-la apenas dentro do orçamento não garante execução em todo Stop. (`contrato.txt:4`; `bin/hook_stop_fila1.py:1198-1217`.)

**6. Responsabilidade dos campos futuros e referências de testes** [nao verificado]

Para os seis campos atuais do marco, o contrato mantém `marco_de_clear.py` como escritor único; a tabela do item 1 identifica os consumidores existentes. A futura origem de `hora` passa a ser o evento do transcript, mas essa interface ainda não existe no escritor atual. (`contrato.txt:2`, `:4`; `bin/marco_de_clear.py:43-45`.)

O contador “eventos no transcript × marcos gravados” está pedido para o ESTADO, porém não define nomes dos campos, armazenamento, escritor, universo contado ou chave de deduplicação. Esses detalhes são **[nao verificado]**; o contador atual de “marcos” conta deploys e não satisfaz essa medida. (`contrato.txt:7`; `bin/orcamento_contexto.py:167-192`, `:397-398`.)

Incluindo `bin/tests/` no censo, há escritores/leitores adicionais de árvores fictícias:

- **Marco:** criação direta, chamada do gravador, leitura de origem/session_id, corrupção/FIFO e alteração de `clear_horas`. (`bin/tests/test_orcamento_contexto.sh:91-94`, `:258-261`, `:384`, `:465-473`, `:486-488`; `bin/tests/test_disjuntor.sh:139`, `:145`, `:157`, `:163`.)
- **Pisos:** semeadura, manipulação e leitura de asserções. (`bin/tests/test_orcamento_contexto.sh:103`; `bin/tests/test_disjuntor.sh:58`, `:130-154`, `:579-580`.)
- **Cota:** chamada do escritor, escrita direta de NaN e leitura de asserções. (`bin/tests/test_disjuntor.sh:435-456`.)
- **Configuração:** `test_handoff_sessao` lê e valida os comandos SessionStart; não lê o conteúdo do marco. (`bin/tests/test_handoff_sessao.sh:57-79`.)

A configuração da cópia contém os hooks, mas aponta para caminhos absolutos `/home/ronald/saas-hasner`. A causa “SessionStart da raiz inerte em app/” é declarada no contrato; sua execução efetiva em produção permanece **[nao verificado]**. (`.claude/settings.json:9`, `:44`, `:57`; `contrato.txt:3`.)

## 05/11 16:04 · sessao edcbe2aa857a · H12H06 · verificador · origem=Code

**O achado 3 continua parcial:** o aplicar pode escrever após uma transferência de empresa concluída entre o censo e a trava, conferindo a entrega somente da empresa antiga. A reversão ganhou a conferência que falta no aplicar. (`app/ponto/management/commands/rederivar_falta_declarada.py:79`, `app/ponto/management/commands/rederivar_falta_declarada.py:175`, `app/ponto/management/commands/rederivar_falta_declarada.py:194`, `app/ponto/management/commands/rederivar_falta_declarada.py:244`.)

Análise estática, somente leitura. Execução dos testes, RED/GREEN, ocorrência em produção, sombra, hash da09 e ordem efetiva dos pousos: **[nao verificado]**. Confirmação independente do hash e merge-base: **[nao verificado]**. [nao verificado]

**1. Fechamento dos achados da volta 2**

| Achado | Estado nesta volta |
|---|---|
| **1 — Reversão sobre estado diferente/empresa diferente** | **Fechado para os contraexemplos apresentados.** O arquivo inclui colaborador, datas, tipo e status; a reversão compara essa identidade, empresa atual, minutos esperados e entrega antes de escrever. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:241`, `app/ponto/management/commands/rederivar_falta_declarada.py:244`, `app/ponto/management/commands/rederivar_falta_declarada.py:247`, `app/ponto/management/commands/rederivar_falta_declarada.py:249`.) |
| **2 — Arquivo sobrescrevível** | **Fechado.** Permanecem nome com PID/UUID, abertura exclusiva e retorno sem arquivo quando não há alvo; agora há teste de colisão com sentinela. (`app/ponto/management/commands/rederivar_falta_declarada.py:152`, `app/ponto/management/commands/rederivar_falta_declarada.py:161`, `app/ponto/management/commands/rederivar_falta_declarada.py:164`, `app/ponto/tests/test_h06_reversao_guardas.py:230`.) |
| **3 — Concorrência/ranges/atomicidade** | **Parcial.** Registro, aplicar e reverter compartilham a trava Empresa; permanece atomicidade e conferência das competências atravessadas. Entretanto, aplicar não reconfirma a empresa atual após o censo, permitindo travar/conferir A e escrever ausência hoje em B. (`app/folha/models.py:181`, `app/ponto/management/commands/rederivar_falta_declarada.py:173`, `app/ponto/management/commands/rederivar_falta_declarada.py:175`, `app/ponto/management/commands/rederivar_falta_declarada.py:194`, `app/ponto/management/commands/rederivar_falta_declarada.py:229`, `app/ponto/management/commands/rederivar_falta_declarada.py:231`.) |
| **4 — Leitor H12 divergente** | **Fechado no código.** TXT e linha compartilham o predicado; contador, atributo, borda e badge usam `rescisao`. A cobertura do contador continua incompleta, conforme D3. (`app/folha/export.py:870`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`.) |

O contraexemplo residual é: censo em A encontra falta720/grade420; transferência para B já entregue termina; aplicar trava A, busca a ausência pelo PK, aceita os mesmos campos/minutos, grava420 e pergunta entrega somente de A. A operação de transferência admite alterar empresa. É uma possibilidade estática; ocorrência real: **[nao verificado]**. (`app/colaboradores/services/colaborador.py:33`, `app/colaboradores/services/colaborador.py:55`, `app/ponto/management/commands/rederivar_falta_declarada.py:79`, `app/ponto/management/commands/rederivar_falta_declarada.py:174`, `app/ponto/management/commands/rederivar_falta_declarada.py:183`, `app/ponto/management/commands/rederivar_falta_declarada.py:186`, `app/ponto/management/commands/rederivar_falta_declarada.py:194`.)

Quanto às demais pendências apontadas na resposta v2:

| Pendência | Estado |
|---|---|
| Rollback real na segunda gravação | **Fechado como caso estático de teste:** a primeira chamada executa a porta real e a segunda lança erro dentro do `atomic`. Resultado executado: **[nao verificado]**. (`app/ponto/tests/test_h06_reversao_guardas.py:212`, `app/ponto/tests/test_h06_reversao_guardas.py:219`, `app/ponto/management/commands/rederivar_falta_declarada.py:229`.) |
| Mordida de `x`→`w` | **Fechado para essa mutação:** o teste cria exatamente o nome que será aberto e exige preservação da sentinela. (`app/ponto/tests/test_h06_reversao_guardas.py:236`, `app/ponto/tests/test_h06_reversao_guardas.py:239`, `app/ponto/tests/test_h06_reversao_guardas.py:241`.) |
| Parcialidade480 versus financeiro | **Aberto, preexistente:** cobertura exige minutos abaixo480; os leitores financeiros mantêm outros critérios. (`app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2598`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`.) |
| Rateio uniforme | **Aberto, preexistente:** o rateio continua no leitor financeiro. (`app/folha/export.py:121`.) |
| Trilha engolida/prova posterior | **Parcial:** foto divergente aborta dentro da transação; auditoria continua best-effort. (`app/ponto/management/commands/rederivar_falta_declarada.py:202`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`.) |
| Cadastro atual decide passado | **Aberto, apartado pelo aval:** a janela histórica continua lendo o corte atual. (`app/ponto/janelas.py:50`, `app/ponto/services/fechamento_lista.py:104`, `app/docs/PROMPTS.md:1861`.) |
| Arquivos antigos incompatíveis | **Aberto como compatibilidade:** agora faltarem também `data_fim`, `tipo` ou `status` causa recusa, mesmo havendo `empresa_id`. Existência de arquivos afetados em produção: **[nao verificado]**. (`app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:227`.) |

**2. Os três descartes**

| Descarte | Conferência |
|---|---|
| **D1 — Nenhum apply v1 em produção** | **Não demonstravelmente falso pelo código; premissa operacional [nao verificado].** O código comprova a recusa do formato antigo, mas não comprova ausência de aplicações anteriores. (`app/ponto/management/commands/rederivar_falta_declarada.py:224`, `app/ponto/management/commands/rederivar_falta_declarada.py:227`.) |
| **D2 — Janela atual autorizada; MODO-COMUM separado** | **Motivo compatível com o aval.** Isso aparta a cura, mas não elimina a dependência do cadastro atual. (`app/docs/PROMPTS.md:1860`, `app/docs/PROMPTS.md:1861`, `app/ponto/janelas.py:50`, `app/ponto/services/fechamento_lista.py:104`.) |
| **D3 — Teste do partial cobre a soma da view** | **Motivo falso.** A view soma `r['rescisao']`; o teste chama diretamente o partial e só inspeciona `data-resc`. Alterar `n_rescisao` para0 mantém esse teste verde. (`app/ponto/views.py:1215`, `app/ponto/tests/test_h06_reversao_guardas.py:269`, `app/ponto/tests/test_h06_reversao_guardas.py:271`, `app/ponto/tests/test_h06_reversao_guardas.py:272`.) |

**3. Escritores, decisões de escrita e travas**

O censo abaixo distingue os escritores persistentes de produção dos chamadores que apenas delegam a escrita. (`app/folha/models.py:162`, `app/folha/models.py:186`, `app/folha/views.py:297`, `app/folha/management/commands/regerar_txt_dominio.py:118`.)

| Escritor/caminho de `ExportacaoDominio` | Toma a trava? |
|---|---|
| `registrar`: criação do registro | **Sim**, dentro de `atomic`, antes do `create`. (`app/folha/models.py:179`, `app/folha/models.py:181`, `app/folha/models.py:186`.) |
| `invalidar`: invalidação, motivo e substituição | **Não.** É outro escritor de campos da mesma entidade, dentro da porta declarada. (`app/folha/models.py:158`, `app/folha/models.py:162`, `app/core/portas.py:504`.) |
| View de geração | Delega a `registrar`; montagem e conteúdo antecedem a trava. (`app/folha/views.py:208`, `app/folha/views.py:268`, `app/folha/views.py:281`, `app/folha/views.py:297`.) |
| Comando de regeração | Delega a `registrar`; depois chama `invalidar`, fora do `atomic` local já encerrado de `registrar`. (`app/folha/management/commands/regerar_txt_dominio.py:118`, `app/folha/management/commands/regerar_txt_dominio.py:122`, `app/folha/models.py:192`.) |

`invalidar` sem trava não demonstra sozinho uma nova entrega concorrente: seu chamador de produção invalida o antigo depois de registrar a substituta. Entretanto, a afirmação “todo escritor de ExportacaoDominio toma a trava” é falsa. (`app/folha/models.py:162`, `app/folha/management/commands/regerar_txt_dominio.py:118`, `app/folha/management/commands/regerar_txt_dominio.py:122`.)

| Chamador direto de `competencia_entregue` que participa de decisão de escrita | Trava anterior |
|---|---|
| `rederivar.handle`, pré-checagem | **Não**; existe rechecagem final protegida. (`app/ponto/management/commands/rederivar_falta_declarada.py:143`, `app/ponto/management/commands/rederivar_falta_declarada.py:173`, `app/ponto/management/commands/rederivar_falta_declarada.py:194`.) |
| `rederivar._entregues` | **Não** nas pré-checagens do aplicar; **sim** quando chamado pela reversão. (`app/ponto/management/commands/rederivar_falta_declarada.py:70`, `app/ponto/management/commands/rederivar_falta_declarada.py:147`, `app/ponto/management/commands/rederivar_falta_declarada.py:231`, `app/ponto/management/commands/rederivar_falta_declarada.py:249`.) |
| `competencia_fechada_para_celula` | **Não**; decide bloqueio de regeneração/restauração. (`app/folha/export.py:622`, `app/ponto/management/commands/gerar_celulas.py:173`, `app/ponto/portas/celula.py:199`, `app/ponto/portas/celula.py:529`.) |
| `txt_vigente_x_gravado` | **Não** na decisão de regerar; a trava aparece posteriormente em `registrar`. (`app/folha/export.py:659`, `app/folha/management/commands/regerar_txt_dominio.py:106`, `app/folha/management/commands/regerar_txt_dominio.py:113`, `app/folha/management/commands/regerar_txt_dominio.py:118`.) |
| `marcos_da_competencia` | **Não**; alimenta os consumidores abaixo. (`app/ponto/services/fechamento.py:1521`, `app/ponto/services/fechamento.py:1499`.) |
| `competencias_a_lavrar` | **Não**; seleciona competências para revogação HE e snapshot. (`app/ponto/services/he_pendente_lavrado.py:223`, `app/ponto/management/commands/lavrar_he_pendente.py:39`, `app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/management/commands/lavrar_he_pendente.py:71`.) |
| `esmeril_espelho.janela` | **Não**; seleciona o universo que será lavrado como snapshot. (`app/ponto/services/esmeril_espelho.py:58`, `app/ponto/services/esmeril_espelho.py:219`, `app/ponto/services/esmeril_espelho.py:303`, `app/ponto/services/esmeril_espelho.py:324`.) |
| `vigia_fio.competencia_do_dia_entregue` | **Não**; alimenta gates de estado, reapontamento, reemissão e escolha Pauta/Pergunta. (`app/chamados/services/vigia_fio.py:840`, `app/chamados/services/vigia_fio.py:842`, `app/chamados/models.py:347`, `app/chamados/services/fio.py:770`, `app/chamados/reconciliador.py:713`, `app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/fabrica_da_celula.py:188`.) |

Os consumidores indiretos de `marcos_da_competencia`/`empresas_exportadas_no_escopo` que decidem escrita também não aderem à trava nova:

- Guarda do recálculo e exclusão do universo gravável. (`app/ponto/services/fechamento.py:530`, `app/ponto/services/fechamento.py:552`, `app/ponto/services/fechamento.py:555`.)
- Guarda do retrato de `DiaPago` e escolha entre motor/retrato na lavratura. (`app/ponto/services/dia_pago.py:449`, `app/ponto/services/dia_pago.py:457`, `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `app/ponto/management/commands/lavrar_dias_pagos.py:123`.)
- Pauta DP após autorização de HE, embora exista `atomic`. (`app/ponto/services/autorizacao_he_periodo.py:118`, `app/ponto/services/autorizacao_he_periodo.py:288`, `app/ponto/services/autorizacao_he_periodo.py:299`, `app/ponto/services/autorizacao_he_periodo.py:343`.)
- Escolha entre Pauta e reabertura de órfãs. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:95`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:99`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`.)
- Cobrança que produz push e nota. (`app/chamados/services/cobranca_folha.py:68`, `app/chamados/views_cobrar.py:95`, `app/chamados/views_cobrar.py:123`, `app/chamados/views_cobrar.py:132`.)

Preview, carimbo do ranking e informação do comando de status são leitores diretos sem decisão de escrita nesses pontos. (`app/folha/views.py:354`, `app/folha/services/ranking_he.py:89`, `app/ponto/management/commands/lavrar_status_competencia.py:43`.)

**Ordem/deadlock:** aplicar faz Empresa→Ausencia; reverter ordena as Empresas e depois trava Ausencia. As consultas não restringem `of=('self',)` e a ordenação de Ausencia referencia `colaborador__nome`, portanto não se deve assumir trava limitada à ausência. SQL efetivo e conjunto de linhas travadas: **[nao verificado]**. (`app/ponto/management/commands/rederivar_falta_declarada.py:173`, `app/ponto/management/commands/rederivar_falta_declarada.py:174`, `app/ponto/management/commands/rederivar_falta_declarada.py:230`, `app/ponto/management/commands/rederivar_falta_declarada.py:232`, `app/ponto/models.py:819`.)

Existem gates que primeiro travam Colaborador, mas não demonstrei neles a aresta inversa até Empresa. Também não há, nos caminhos examinados de fechamento, outro `select_for_update` de `FechamentoMensal`: aparecem `get_or_create`, `update` e leitura simples antes de `save`. **Ciclo concreto Empresa×Colaborador×FechamentoMensal: [nao verificado]**. (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/services/abertura.py:164`, `app/ponto/services/fechamento.py:889`, `app/ponto/services/fechamento.py:1316`, `app/ponto/models.py:497`, `app/ponto/models.py:503`.)

O recálculo por evento mostrado é agendado em `on_commit`; esse caminho não prova uma trava de fechamento dentro da transação que segura Empresa/Ausencia. (`app/ponto/services/fechamento.py:1794`, `app/ponto/services/fechamento.py:1808`.)

**4. Os testes novos mordem?** [nao verificado]

As respostas são contrafactuais estáticas, por teste isolado; execução dos mutantes: **[nao verificado]**. [nao verificado]

| Teste | Mordida e mutante que sobrevive | [nao verificado]
|---|---|
| **Tipo alterado** | Morde retirar a comparação de tipo. **Sobrevive:** retirar `status`/`data_fim` da identidade, pois o caso só altera tipo. (`app/ponto/tests/test_h06_reversao_guardas.py:193`, `app/ponto/tests/test_h06_reversao_guardas.py:196`, `app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/management/commands/rederivar_falta_declarada.py:241`.) |
| **Empresa alterada** | Morde retirar a comparação da empresa atual. **Sobrevive:** retirar a trava da reversão; a transferência do teste é serial. Também não cobre transferência entre censo e aplicar. (`app/ponto/tests/test_h06_reversao_guardas.py:202`, `app/ponto/tests/test_h06_reversao_guardas.py:206`, `app/ponto/management/commands/rederivar_falta_declarada.py:231`, `app/ponto/management/commands/rederivar_falta_declarada.py:244`.) |
| **Falha na segunda gravação** | Morde retirar o `atomic` da reversão: a primeira reposição ficaria720. **Sobrevive:** retirar o `save` da porta; nenhuma primeira escrita ocorreria e a segunda chamada ainda lançaria o erro mockado. (`app/ponto/tests/test_h06_reversao_guardas.py:212`, `app/ponto/tests/test_h06_reversao_guardas.py:219`, `app/ponto/tests/test_h06_reversao_guardas.py:228`, `app/ponto/management/commands/rederivar_falta_declarada.py:229`, `app/ponto/services/ausencia.py:532`.) |
| **Arquivo existente** | Morde `x`→`w`. **Sobrevive:** substituir abertura exclusiva por `if exists: raise FileExistsError` seguido de abertura `w`; o caso serial passa apesar de reintroduzir corrida. (`app/ponto/tests/test_h06_reversao_guardas.py:230`, `app/ponto/tests/test_h06_reversao_guardas.py:237`, `app/ponto/tests/test_h06_reversao_guardas.py:239`, `app/ponto/management/commands/rederivar_falta_declarada.py:164`.) |
| **Mesma trava Empresa** | Morde remover o SQL de trava do registro/aplicar. **Sobrevivem:** filtrar uma Empresa errada ou inexistente, retirar a trava da reversão, ou retirar a rechecagem final do aplicar. Só exige tabela+`FOR UPDATE` e alguma consulta ED posterior; não verifica PK, linha efetivamente travada ou concorrência. (`app/ponto/tests/test_h06_reversao_guardas.py:249`, `app/ponto/tests/test_h06_reversao_guardas.py:253`, `app/ponto/tests/test_h06_reversao_guardas.py:260`, `app/ponto/tests/test_h06_reversao_guardas.py:261`, `app/folha/export.py:565`, `app/ponto/management/commands/rederivar_falta_declarada.py:194`, `app/ponto/management/commands/rederivar_falta_declarada.py:231`.) |
| **Tela lê rescisão da linha** | Morde restaurar a regra antiga de `data-resc`. **Sobrevivem:** restaurá-la somente na borda/badge, zerar o contador da view ou clonar o predicado no produtor. O teste só afirma dois atributos HTML. (`app/ponto/tests/test_h06_reversao_guardas.py:267`, `app/ponto/tests/test_h06_reversao_guardas.py:272`, `app/ponto/tests/test_h06_reversao_guardas.py:273`, `app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/views.py:1215`, `app/ponto/services/fechamento_lista.py:109`.) |

Os métodos anulados com `None` na subclasse continuam definidos na classe base; isso não demonstra remoção dos testes antigos da suíte. (`app/ponto/tests/test_h06_reversao_guardas.py:34`, `app/ponto/tests/test_h06_reversao_guardas.py:90`, `app/ponto/tests/test_h06_reversao_guardas.py:185`.)

**5. Quem escreve e quem lê os campos**

O arquivo tem um escritor, `json.dump`, e um carregador, `_reverter`; todas as chaves são exigidas antes da transação de reversão. (`app/ponto/management/commands/rederivar_falta_declarada.py:165`, `app/ponto/management/commands/rederivar_falta_declarada.py:220`, `app/ponto/management/commands/rederivar_falta_declarada.py:224`.)

| Campo do arquivo | Quem escreve/produz | Quem lê |
|---|---|---|
| `pk` | `x.pk`. (`app/ponto/management/commands/rederivar_falta_declarada.py:165`.) | Busca, existência e seleção da reposição. (`app/ponto/management/commands/rederivar_falta_declarada.py:232`, `app/ponto/management/commands/rederivar_falta_declarada.py:237`, `app/ponto/management/commands/rederivar_falta_declarada.py:255`.) |
| `empresa_id` | Empresa solicitada, `emp.pk`. (`app/ponto/management/commands/rederivar_falta_declarada.py:128`, `app/ponto/management/commands/rederivar_falta_declarada.py:165`.) | Travas ordenadas, comparação com empresa atual e consulta de entrega. (`app/ponto/management/commands/rederivar_falta_declarada.py:230`, `app/ponto/management/commands/rederivar_falta_declarada.py:244`, `app/ponto/management/commands/rederivar_falta_declarada.py:249`.) |
| `colaborador_id` | `_identidade(x)`. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/management/commands/rederivar_falta_declarada.py:48`.) | Comparação de identidade; colaborador da ausência também alimenta a consulta da empresa atual. (`app/ponto/management/commands/rederivar_falta_declarada.py:233`, `app/ponto/management/commands/rederivar_falta_declarada.py:241`.) |
| `data` | `_identidade`, em ISO. (`app/ponto/management/commands/rederivar_falta_declarada.py:48`.) | Comparação de identidade; competências usam a data da ausência atual. (`app/ponto/management/commands/rederivar_falta_declarada.py:60`, `app/ponto/management/commands/rederivar_falta_declarada.py:241`.) |
| `data_fim` | `_identidade`, ISO ou `None`. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/management/commands/rederivar_falta_declarada.py:48`.) | Comparação de identidade; expansão das competências usa o fim atual. (`app/ponto/management/commands/rederivar_falta_declarada.py:60`, `app/ponto/management/commands/rederivar_falta_declarada.py:241`.) |
| `tipo` | `_identidade(x)`. (`app/ponto/management/commands/rederivar_falta_declarada.py:44`, `app/ponto/management/commands/rederivar_fal

[truncado em 20000 de 28347 caracteres]

## 05/11 16:04 · sessao f8a28c6cc87d · - · - · origem=Ronald (tmux)

Cadeia principal e leitores:

- `_vivos_que_a_premissa_matou` filtra **somente** `modulo_origem='cluster_batida_espuria'` e `status_local__in=VIVOS`, obtém o dia por `dia_do_fato(ch, fortes=True)` e passa **booleano** `_d.isoformat() in datas` como `FATO_CLUSTER_ESPURIO`; ausência de dia pula o chamado. `app/ponto/services/cluster_espurio.py:39-52`.
- `pendencias` chama `dias_com_cluster`, deriva `datas` dos achados e devolve a lista sob `a_fechar`. A janela vem de `ref - timedelta(days=dias - 1)` até `ref`; `JANELA_DIAS=14`. Consequentemente, dia fora da janela e célula sem ata podem faltar no conjunto igual a um dia medido sem assinatura. `app/ponto/services/cluster_espurio.py:12`, `:21-35`, `:73-82`.
- Únicos consumidores de produção diretos dessa lista localizados: `reconciliar_colab` itera `p['a_fechar']`, chama `ch.reconciliar()` e só incrementa fechados se devolveu `True`; comando `detectar_cluster_espurio` em DRY/alarme itera a lista e a registra como pendência `premissa_morta`. `app/ponto/services/cluster_espurio.py:95-99`; `app/ponto/management/commands/detectar_cluster_espurio.py:50-56`.
- `reconciliar_colab` só atua para colaborador `ativo` com `EscalaColaborador(ativa=True,data_fim__isnull=True)`. `app/ponto/services/cluster_espurio.py:91-94`.
- Comando manual `detectar_cluster_espurio --apply` chama o ato; `--alarme` sempre impede escrita, mesmo junto com `--apply`; `--dias` permite mudar a janela e default é 14. `app/ponto/management/commands/detectar_cluster_espurio.py:25-46`.
- Cron direto do detector é `06:24 --alarme`, dependente de `reconciliar_grade`; portanto esse cron direto só vigia. `app/config/crons.py:598`.
- Cron `processar_cartorio --apply` às `06:27` por empresa chama `julgar_colab` para células passadas. Depois do corpo, `apply_` e `_julgadas` verdadeiro acionam `_cluster(c, hoje)`. `app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:38-65`; `app/ponto/services/cartorio.py:981-994`.
- `julgar_celula` encontra célula, impõe limites temporais e chama `julgar_colab`; assim os sinais que chegam a ela também podem acionar cluster. `app/ponto/services/cartorio.py:1960-1991`.
- Sinais de salvar/apagar `Batida` e `Ausencia`, e salvar `Justificativa`, usam `_julgar_dias` → `cartorio.julgar_celula`; há guarda contra recursão `em_julgamento`. `app/ponto/signals.py:85-93`, `:131-165`.
- Criação ou mudança de estado de `ChamadoColaborador` chama `cartorio.julgar_celula` pelo dia do juiz, também com guarda contra recursão. `app/chamados/signals.py:155-180`.

Outros acessos indiretos ao mesmo julgamento, todos sujeitos a `apply_`/`_julgadas` antes do cluster (`app/ponto/services/cartorio.py:981-994`):

- `vigia_de_hora.rodar` → `julgar_celula`, cron a cada 10 minutos `--apply`: `app/ponto/services/vigia_de_hora.py:131`; `app/config/crons.py:367`.
- `reconciliar_apos_vinculo` → `julgar_celula`: `app/colaboradores/services/pos_vinculo.py:50-72`.
- Regeneração de células → `julgar_celula`: `app/ponto/portas/celula.py:360-372`.
- `rejulgar_furos_do_vinculo` → `julgar_colab`: `app/escala/regua_defesa.py:677-693`.
- `_ctx_c2` sem lâmpadas → `julgar_celula`: `app/chamados/reconciliador.py:644-671`.
- Cobrança manual → `julgar_celula(forcar=True,toque=True)`: `app/chamados/services/cobrar_dia.py:37-39`.
- Comandos especiais também chamam o julgamento: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:76-78`; `app/ponto/management/commands/curar_residuo_bug115.py:133-143`; `app/ponto/management/commands/celula_veredito_velho.py:144`. `lavrar_status_competencia` usa `completar_status=True` e retorna antes do cluster: `app/ponto/services/cartorio.py:949-950`.

Testes encontrados: [nao verificado]

- Fechamento após **retratação real** das batidas: `test_evento_fecha_o_chamado_cuja_premissa_caiu`, `app/ponto/tests/test_varredura_cluster_evento.py:50-62`.
- Vigia não escreve, acusa pendência com saída 2 e fica satisfeito após reconciliação: `app/ponto/tests/test_varredura_cluster_evento.py:64-84`.
- Cron é vigia com `--alarme` e sem `--apply`: `app/ponto/tests/test_varredura_cluster_evento.py:87-95`.
- Contador só conta o `True` do gate, com `pendencias` mockado: `app/chamados/tests/test_fechar3_retorno_do_gate.py:205-216`.
- Detector identifica rajada e não identifica turno normal: `app/ponto/tests/test_detectar_cluster_espurio.py:47-74`.
- Leitura de dia pelo juiz, verificada por fonte: `app/chamados/tests/test_c_miudos_1.py:38-44`.
- Parcial sem período fica na lista e conserva assinatura, sem abrir: `app/ponto/tests/test_cobparc7_cluster_le_a_lista.py:31-43`.
- Catálogo fixa o mapeamento `cluster_batida_espuria → cluster_espurio`: `app/chamados/tests/test_premissa_catalogo.py:85-106`.
- Não localizei teste que exerça explicitamente **fechar por idade**, **fora da janela** ou **sem ata**; existência desse selo específico permanece `[nao verificado]`. Os testes acima não provam tal cobertura: o fechamento real é por retratação e o teste do contador mocka `pendencias` (`app/ponto/tests/test_varredura_cluster_evento.py:54-62`; `app/chamados/tests/test_fechar3_retorno_do_gate.py:214-216`).

## 05/11 16:04 · sessao 6275a964bf6c · CODEX-SUFOCA-10-textos · leitura · origem=Code

Os contraexemplos abaixo são cenários construídos pelos ramos do código. Reprodução executada, ocorrência em produção, colaboradores atingidos e impacto financeiro: **[nao verificado]**. Não usei especulação para sustentar os achados.

1. **Ausência que desconta é anunciada como abonada.** A aprovação envia incondicionalmente “`n dia(s) abonado(s)`” em [views.py:2639](app/ponto/views.py:2639) e no FCM em [views.py:2643](app/ponto/views.py:2643). O catálogo distingue `falta` como `DESCONTA`, “sem abono” ([ausencias.py:88](app/ponto/catalogo/ausencias.py:88)), e o gate permite aprová-la sem documento ([ausencia.py:600](app/ponto/services/ausencia.py:600)).
   
   **Contraexemplo:** tipo cadastrado `falta/desconta`, ausência de um dia aguardando decisão, sem documento; DP aprova. A mensagem afirma “1 dia abonado”, embora o efeito continue sendo desconto. **Certeza: alta.**

2. **Tela chama horas de “pagas” sem apuração; PDF também omite essa ressalva.** Sem fechamento nem `DiaPago`, a autoridade registra `fonte_dos_totais='sem apuracao ainda'` e preserva o resumo do motor ([cartao_pela_celula.py:340](app/relatorios/cartao_pela_celula.py:340), [cartao_pela_celula.py:203](app/relatorios/cartao_pela_celula.py:203)). A tela imprime “Trabalhadas (pagas)” sem consultar essa fonte ([espelho.html:38](app/templates/ponto/espelho.html:38)); o PDF imprime o total “Trabalhadas” sem a ressalva ([pdf_espelho.py:766](app/relatorios/pdf_espelho.py:766)).
   
   **Contraexemplo:** competência exata, vínculo 08–17 com intervalo de uma hora, batidas 08/12/13/17, nenhum fechamento nem `DiaPago`. O total do motor aparece como “pagas” na tela e sem indicação de ausência de apuração no PDF. **Certeza: alta.**

3. **“Dias” no PDF de retidos conta motivos distintos.** A coluna usa `len(motivos_espelho)` ([retidos_pdf.py:88](app/folha/retidos_pdf.py:88), [retidos_pdf.py:90](app/folha/retidos_pdf.py:90)). Essa lista vem de um conjunto de motivos, deduplicado antes do retorno ([export.py:1197](app/folha/export.py:1197), [export.py:1240](app/folha/export.py:1240)).
   
   **Contraexemplo:** colaborador apto nos demais requisitos, fechamento existente e cinco dias com veredito `furo`, sem isenção de retenção. Todos produzem `espelho_furo`; a lista tem um elemento e o PDF mostra **Dias = 1**, para cinco dias ([export.py:1235](app/folha/export.py:1235)). **Certeza: alta.**

4. **“Horário” do PDF pode informar o horário de outro vínculo.** O coletor escolhe o vínculo do último dia da janela ([pdf_espelho.py:307](app/relatorios/pdf_espelho.py:307)). Cada linha com turno imprime o início/fim daquele único template na coluna “Horario” ([pdf_espelho.py:865](app/relatorios/pdf_espelho.py:865), [pdf_espelho.py:903](app/relatorios/pdf_espelho.py:903)).
   
   **Contraexemplo:** vínculo 08–17 até dia 10; vínculo 19–07 depois, ambos dentro da competência. Um dia anterior à troca, com batidas e turno 08–17, recebe **Horario 19:00 07:00**. Os marcos congelados da célula daquele dia não participam dessa impressão ([models.py:1124](app/escala/models.py:1124)). **Certeza: alta.**

5. **“O sistema não está pedindo nada” pode coexistir com férias para confirmar.** A frase depende somente de `not pendencias` ([meu_atendimento.html:117](app/templates/chamados/meu_atendimento.html:117)); essa lista contém chamados que não são solicitações ([views.py:614](app/chamados/views.py:614)). Férias aguardando confirmação são carregadas separadamente ([views.py:627](app/chamados/views.py:627)) e exibidas com botões “Confirmar” e “Rejeitar” ([_meu_atendimento_ferias.html:3](app/templates/ferias/_meu_atendimento_ferias.html:3)).
   
   **Contraexemplo:** colaborador sem chamados listados e agendamento enviado pelo DP para confirmação, estado produzido por [services.py:487](app/ferias/services.py:487). A página pede confirmação das férias e afirma “Nenhuma pendência. O sistema não está pedindo nada de você”. **Certeza: alta.**

6. **Alterar somente o status responde “Mensagem enviada”.** Conteúdo, ocorrência de resposta e pushes ficam dentro de `if conteudo` ([acoes_chamado.py:289](app/chamados/services/acoes_chamado.py:289)). Uma transição de status também marca `_fez=True`, acionando a mesma confirmação ([acoes_chamado.py:313](app/chamados/services/acoes_chamado.py:313), [acoes_chamado.py:323](app/chamados/services/acoes_chamado.py:323)).
   
   **Contraexemplo:** gestor autorizado, chamado aberto, POST com `conteudo=''` e `novo_status='em_analise'`. Essa transição é permitida ([motor.py:100](app/chamados/catalogo/motor.py:100)); não há mensagem de resposta, mas o retorno afirma envio. **Certeza: alta.**

7. **“Colaborador notificado” não depende do resultado do envio.** A reabertura administrativa chama FCM e ignora o retorno, depois afirma notificação ([acoes_disputa.py:283](app/chamados/services/acoes_disputa.py:283), [acoes_disputa.py:295](app/chamados/services/acoes_disputa.py:295)). Token vazio retorna `ok=False, motivo='sem_token'` ([fcm_utils.py:104](app/chamados/fcm_utils.py:104)).
   
   **Contraexemplo:** disputa fechada reabrível, admin autorizado, motivo válido, justificativa suficiente e colaborador com `fcm_token=''`. O fluxo anuncia “Colaborador notificado” mesmo sem envio FCM. A eventual leitura posterior da pergunta é **[nao verificado]**. **Certeza: alta quanto ao envio não garantido.**

8. **Intervalo registrado parcialmente é descrito como “não registrado”.** O aviso e o tooltip usam essa descrição ([espelho.html:87](app/templates/ponto/espelho.html:87), [espelho.html:286](app/templates/ponto/espelho.html:286)). O motor também indeniza a diferença entre intervalo mínimo e intervalo efetivamente registrado ([motor_calculo_v2.py:2771](app/ponto/motor_calculo_v2.py:2771)).
   
   **Contraexemplo:** 12x36, mínimo de 60 minutos, batidas E07:00/S12:00/E12:30/S19:00; apuração correspondente exibida no espelho. Há pausa registrada de 30 minutos e outros 30 indenizados, mas o texto a descreve como não registrada. **Certeza: alta, restrita à descrição do fato.**

Não consegui verificar entrega real de mensagens, PDFs efetivamente emitidos, aparência renderizada, frequência desses estados ou valores pagos: **[nao verificado]**. A classificação abaixo atribui à **ESTRUTURA o desacordo entre apresentação e estado**, conforme a exigência de coerência com o cadastro existente da L-099 ([CLAUDE.md:176](CLAUDE.md:176)).

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 (ESTRUTURA/CADASTRO/BATIDA) |
|---|---|---|---|---|
| Desconto anunciado como abono | `app/ponto/views.py:2639` | Aprovar um dia de `falta/desconta` | Alta | ESTRUTURA |
| “Pagas” e totais sem ressalva de apuração | `app/templates/ponto/espelho.html:38`; `app/relatorios/pdf_espelho.py:766` | Batidas completas; sem fechamento e sem `DiaPago` | Alta | ESTRUTURA |
| “Dias” conta motivos distintos | `app/folha/retidos_pdf.py:90` | Cinco dias de furo mostram 1 | Alta | ESTRUTURA |
| Horário do último vínculo em dias anteriores | `app/relatorios/pdf_espelho.py:907` | Troca 08–17 → 19–07 dentro da competência | Alta | ESTRUTURA |
| Nenhuma pendência com férias para confirmar | `app/templates/chamados/meu_atendimento.html:120` | Sem chamados; férias aguardando confirmação | Alta | ESTRUTURA |
| Mensagem enviada sem conteúdo | `app/chamados/services/acoes_chamado.py:323` | Somente aberto → em análise | Alta | ESTRUTURA |
| Notificação afirmada sem envio FCM | `app/chamados/services/acoes_disputa.py:295` | Reabertura válida com token vazio | Alta | ESTRUTURA |
| Pausa registrada chamada de não registrada | `app/templates/ponto/espelho.html:87` | Pausa de 30 minutos; mínimo de 60 | Alta | ESTRUTURA |

## 05/11 16:04 · sessao ffc61dc4860c · - · - · origem=Ronald (tmux)

O fechamento direto é `reconciliar_colab → pendencias['a_fechar'] → ch.reconciliar()`, contando somente retorno verdadeiro; nenhum Followup/nota é anexado por esse corpo (`app/ponto/services/cluster_espurio.py:85-124`).

- A seleção só busca `modulo_origem='cluster_batida_espuria'` e estados VIVOS; obtém o dia pelo juiz, pula chamado sem dia e converte pertinência a `datas` em booleano `FATO_CLUSTER_ESPURIO` (`app/ponto/services/cluster_espurio.py:39-52`). A lista `datas` vem dos achados dentro da janela (`app/ponto/services/cluster_espurio.py:73-82`).
- `premissa_morta` conserva fato `None`, mas recebe `False` para todo dia ausente de `datas`, apagando a distinção janela/ausência de ata/assinatura medida ausente (`app/chamados/catalogo/premissa.py:127-150`; `app/ponto/services/cluster_espurio.py:50`).
- O ato chama o motor e os gates; se permitidos, grava `_d.novo_estado` pela via RECONCILIAR (`app/chamados/models.py:607-621`, `:637-659`). RECONCILIAR usa via `sistema` e transforma `aberto`, `em_analise` ou `registrado` em `resolvido` (`app/chamados/catalogo/motor.py:95`, `:111`).
- A guarda da premissa de furo pode recusar; para chamado fora do universo de furo retorna `True` imediatamente (`app/chamados/models.py:680-701`). O universo é módulo CALADOS_POR_ISENCAO ou categoria/subcategoria dos mapas (`app/chamados/models.py:1194-1200`).
- `_gravar_estado` muda `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; `respondido_por` só muda com `set_user`, que `reconciliar` não passa. Encerramento rebaixa urgência e carimba a saída de vivo (`app/chamados/models.py:222-240`, `:620`). Reabertura limpa `resolvido_em`/via, portanto os campos atuais não representam todo o histórico (`app/chamados/models.py:242-247`).
- A trilha é `LogAuditoria(acao='estado_chamado', modelo='ChamadoColaborador', objeto_id=pk)`, com `valor_antes={'status_local': anterior}`, `valor_depois={'status_local': novo, 'via': via}` e descrição equivalente; não inclui `FATO`, janela, dias, ata, motivo nem nome da rotina (`app/chamados/models.py:274-279`).
- `LogAuditoria` possui usuário, ação, modelo, id do objeto, descrição, JSON antes/depois, IP, ator e timestamp (`app/core/models.py:295-307`). O escritor é best-effort; falha não impede fechamento (`app/core/observ.py:94-105`, `:124-139`).
- A autoria da auditoria vem da request ou de `sistema:<comando>`; identifica contexto de execução, sem identificar qual rotina interna fechou (`app/core/observ.py:22-32`, `:108-123`). Assim `sistema:detectar_cluster_espurio` é evidência do comando, enquanto um evento do cartório sob outro comando/request não ganha marcador cluster (`app/ponto/management/commands/detectar_cluster_espurio.py:45-48`; `app/ponto/services/cartorio.py:990-994`).
- `FollowupChamado` tem `autor_tipo`, `autor_user`, conteúdo, data e chave de evento, mas nenhum campo tipo/motivo persistido (`app/chamados/models.py:1580-1610`). A porta `nota.anexar_ocorrencia` só usa `tipo` na chave quando há `objeto_ref`; sem isso, grava conteúdo/autor sem marcador de tipo (`app/chamados/services/nota.py:75-109`).
- O signal de disputa fechada é outro caminho que chama `reconciliar`, e então anexa sua própria nota (`app/chamados/signals.py:18-45`). `via='sistema'` sozinho não discrimina o caminho.
- O `save` do encerramento pode resolver perguntas (S143) e disparar julgamento de célula; esses receivers não acrescentam motivo do cluster (`app/chamados/signals.py:75-94`, `:164-180`).

Conclusão: os motivos históricos janela/sem ata/lastro medido são indistinguíveis no registro escrito por esse caminho; o total exato atribuído à rotina em eventos do cartório também não é identificável somente por `estado_chamado` (`app/chamados/models.py:275-278`; `app/core/observ.py:108-123`; `app/ponto/services/cluster_espurio.py:95-99`).

## 05/11 16:04 · sessao cf2fef38142b · - · - · origem=Ronald (tmux)

1. **Arredondamento do total de HE diverge da soma emitida — ESTRUTURA.** O fechamento arredonda o total combinado em `app/ponto/services/fechamento.py:910`, mas arredonda as parcelas separadamente em `app/ponto/services/fechamento.py:911` e `app/ponto/services/fechamento.py:913`; o TXT emite essas parcelas em `app/folha/export.py:430` e `app/folha/export.py:431`, convertendo cada uma para centésimos em `app/folha/export.py:42`.

   **Contraexemplo concreto:** estado do motor com HE50=131 minutos e HE100=11 minutos. Total de tela: `round(142/60,2)=2,37h`; parcelas: `round(131/60,2)=2,18h` e `round(11/60,2)=0,18h`; TXT: `218+18=236`, equivalente a **2,36h**. A conta foi reproduzida com Python3, sem imports da aplicação nem escrita; expressões provenientes de `app/ponto/services/fechamento.py:910` e `app/folha/export.py:42`.

   **Cenário de batidas que pode produzir esse estado:** duas jornadas de turno partido, régua relógio, pausas completas, HE autorizada, sem feriado/folga: primeira com 131 minutos extras, segunda com 11. O split diário destina até 120 minutos a HE50 e o excedente a HE100 em `app/ponto/motor_calculo_v2.py:3138`; o total mensal soma minutos e divide por 60 em `app/ponto/motor_calculo_v2.py:324` e `app/ponto/motor_calculo_v2.py:328`. **Especulação rotulada:** a execução integral dessas batidas pelo pareador, células e motor não foi realizada [não verificado].

   **Tela comprovada:** caminho correto **`app/templates/ponto/_fech_linha.html:32`**, exibindo `r.fechamento.horas_extras|floatformat:2`. O GET carrega `FechamentoMensal` gravado em `app/ponto/views.py:1135`, monta o mapa em `app/ponto/views.py:1139` e passa-o ao construtor em `app/ponto/views.py:1151`; o construtor reutiliza esse objeto em `app/ponto/services/fechamento_lista.py:77` e `app/ponto/services/fechamento_lista.py:102`. O TXT atual deriva objetos em modo leitura em `app/folha/views.py:207`, passando-os ao classificador em `app/folha/views.py:212`; recebe os mesmos arredondamentos do derivador em `app/ponto/services/fechamento.py:1112`.

   **Certeza:** alta para a divergência aritmética e os leitores estáticos; média para o cenário completo ponta a ponta [não verificado].

2. **Precisão visual menor no espelho — apresentação, sem perda financeira demonstrada.** Noturnas aparecem com uma casa em `app/templates/ponto/espelho.html:59`; TXT usa centésimos em `app/folha/export.py:42`. Contraexemplo: valor de fechamento **1,24h** aparece como **1,2h** no espelho e **124** no TXT; o resumo pode ler esse mesmo valor em `app/relatorios/cartao_pela_celula.py:100`. Certeza alta no formato; renderização real e ocorrência em produção [não verificado]. Dono ESTRUTURA quanto à diferença de apresentação.

**Limites:** não executei Django, pareador, navegador, testes que criam banco, nem consultei banco/log [não verificado]. Não há prova de colaborador afetado, quantidade de ocorrências ou impacto em dinheiro no anexo fornecido [não verificado]. [nao verificado]

## 05/11 16:04 · sessao e74d7e58badf · - · - · origem=Ronald (tmux)

Rastreio estático, sem execução e sem banco; ocorrências na produção [não verificado].

| Evento emitido | Origem do número |
|---|---|
| `hora_extra_diurna` | `FechamentoMensal.horas_extras_50` (`app/folha/export.py:430`), arredondado a 2 casas a partir de `resultado.horas_extra_50` (`app/ponto/services/fechamento.py:911`); motor soma minutos extras dos `periodos` e divide por 60 (`app/ponto/motor_calculo_v2.py:325`). |
| `hora_extra_100_diurna` | HE100 total menos HE100 feriado (`app/folha/export.py:409`, `app/folha/export.py:431`); fechamento recebe motor (`app/ponto/services/fechamento.py:913`) e pode acrescentar feriado tratado como folga (`app/ponto/services/fechamento.py:947`). |
| `hora_extra_feriado_diurna` | Campo HE100 feriado (`app/folha/export.py:409`, `app/folha/export.py:432`), motor mais períodos de feriado encontrados entre folgas trabalhadas (`app/ponto/services/fechamento.py:940`, `app/ponto/services/fechamento.py:946`). Suprimido conforme ciclo e cláusula final da competência (`app/folha/export.py:411`). |
| `folga_trabalhada_diurna` | Campo próprio (`app/folha/export.py:435`): soma minutos de `periodos_ft` com escala certa e exclui os classificados como feriado (`app/ponto/services/fechamento.py:925`, `app/ponto/services/fechamento.py:944`, `app/ponto/services/fechamento.py:949`). |
| `adicional_noturno` | Campo `horas_noturnas` (`app/folha/export.py:436`), arredondado em `app/ponto/services/fechamento.py:909`; motor soma minutos noturnos legais de `periodos + periodos_ft` (`app/ponto/motor_calculo_v2.py:321`). |
| `intrajornada_diurna` | Campo `horas_intra_indenizada` (`app/folha/export.py:437`), arredondado em `app/ponto/services/fechamento.py:988`; motor soma intrajornada de `periodos + periodos_ft` (`app/ponto/motor_calculo_v2.py:370`). |
| `atraso_diurno` | Atraso + saída antecipada (`app/folha/export.py:440`, `app/folha/export.py:388`). Fechamento abate créditos da ausência parcial no próprio dia antes do arredondamento do atraso (`app/ponto/services/fechamento.py:982`, `app/ponto/services/fechamento.py:985`); saída antecipada vem do motor (`app/ponto/services/fechamento.py:986`). |
| `falta_diurna` | Não lê `horas_falta`: consulta ausências aprovadas do catálogo DESCONTA e expande datas (`app/folha/export.py:76`, `app/folha/export.py:149`, `app/folha/export.py:169`); TXT escreve `len(datas)` e registros filhos com datas (`app/folha/export.py:207`, `app/folha/export.py:209`). |

A porta efetiva do TXT deriva fechamento em memória, pela mesma função do escritor, e injeta esses objetos na classificação e montagem; portanto seria incorreto descrever o TXT real como simples leitura do fechamento gravado (`app/folha/views.py:207`, `app/folha/views.py:212`, `app/folha/views.py:268`). Batidas vêm de `batidas_apuraveis` (`app/ponto/services/fechamento.py:623`); células do envelope são carregadas e alimentam o motor (`app/ponto/services/fechamento.py:596`, `app/ponto/services/fechamento.py:741`); CCT é escolhida para cada fatia de vínculo (`app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:727`).

DSR é calculado e guardado (`app/ponto/services/fechamento.py:1085`, `app/ponto/services/fechamento.py:1087`), mas não aparece na lista retornada de eventos (`app/folha/export.py:428`–450). O comentário declara que o Domínio faz o reflexo, porém essa operação externa [não verificado].

Achado forte: a seleção de CCT/ciclo no final da competência pode apagar feriado apurado em fatia anterior. Certeza alta, dono ESTRUTURA. O motor roda por vínculo e data inicial da fatia (`app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:277`, `app/ponto/services/fechamento.py:727`), enquanto a cláusula exportada é consultada apenas no último dia (`app/folha/export.py:359`, `app/folha/export.py:369`). O TXT subtrai HE feriado do total e depois zera a parcela feriado se a última regra 12x36 não paga dobra (`app/folha/export.py:409`–412). Contraexemplo estático: primeira fatia 12x36, dobra=True, 12h de feriado; segunda fatia 12x36, dobra=False. Com fechamento HE100=12h e HE100feriado=12h, o TXT produz HE100 normal=0 e feriado=0; alcance produtivo [não verificado]. A mesma seleção final merece conferência para mudança de ciclo (`app/folha/export.py:330`).

Achado adicional: helper de 8932 sem ligação ao emissor. Certeza alta para não emissão, média para defeito de negócio, dono ESTRUTURA. `datas_abonadas` calcula dias de ausência justificada (`app/folha/export.py:190`), e os testes provam apenas esse helper (`app/folha/tests/test_export_8932.py:37`). A lista de eventos e montagem do TXT não o consomem (`app/folha/export.py:428`, `app/folha/export.py:711`–725); não existe evento correspondente nas escolhas do cadastro (`app/folha/models.py:13`–38). Contraexemplo: atestado aprovado de três dias, como o teste existente (`app/folha/tests/test_export_8932.py:40`), devolve três datas ao helper, mas não gera rubrica 8932 pelo emissor. Obrigatoriedade real dessa rubrica para empresas atuais [não verificado]; a expectativa está declarada apenas no comentário do teste (`app/folha/tests/test_export_8932.py:2`).

## 05/11 16:04 · sessao 08a8953bbbdb · - · - · origem=Ronald (tmux)

Censo de `pisos.log`, `cota.json`, C1 e ESTADO (somente código; situação de produção não verificada).

**`logs/sessao/pisos.log`**

- Escritor de conteúdo: `orcamento_contexto.registrar_piso`, append no formato `AAAA-MM-DD HH:MM:SS <origem> <tokens>`; a data vem de `quando`, a origem é literal e os tokens viram inteiro; exceção é engolida. (`bin/orcamento_contexto.py:198-207`)
- Chamador em hook: Stop configurado em `.claude/settings.json:3-9`; ao observar mudança do marco, arma `piso_pendente`; obtém o primeiro turno após a última ocorrência daquela origem no transcript e registra com **hora do Stop** (`agora`), consumindo a pendência somente com inteiro positivo. (`bin/hook_stop_fila1.py:870-903`; `bin/orcamento_contexto.py:278-290`)
- Chamador fora de hook: CLI `--semear-pisos` chama `semear_pisos`; varre transcripts principais `~/.claude/projects/-home-ronald-saas-hasner*/*.jsonl`, por padrão 7 dias, encontra compact/clear e usa **timestamp do primeiro turno posterior**, depois grava pela mesma função. Não possui deduplicação nesse laço. (`bin/orcamento_contexto.py:295-315`, `bin/orcamento_contexto.py:405-410`)
- Leitor direto de conteúdo: `ultimo_piso`; exige quatro tokens separados por espaço, filtra coluna 3 pela origem, aceita coluna 4 somente `isdigit`, escolhe a maior dupla textual `(data,hora)` e devolve tokens/data. **Não filtra por sessão, marco atual ou idade.** (`bin/orcamento_contexto.py:210-222`)
- Leitor derivado para C1: `gatilho_tokens` chama `ultimo_piso(...,'compact')`; sem piso ou piso ≥200000, C1 não liga; caso contrário `(piso+200000)//2`. (`bin/orcamento_contexto.py:55`, `bin/orcamento_contexto.py:225-232`)
- Leitor derivado para painel: `disjuntor.ler` consulta ambas as origens e o gatilho; `_pisos_txt` publica tokens com data, ou `nao medido`, e gatilho ou `NAO LIGA`. (`bin/disjuntor.py:1614-1616`, `bin/disjuntor.py:1669-1674`)
- Leitor indireto final: `gerar_estado` chama `disjuntor.ler/linha` e insere o resultado em “Disjuntores”. (`bin/gerar_estado.py:279-283`, `bin/gerar_estado.py:336-338`, `bin/gerar_estado.py:365`)
- Leitor de metadados, sem conteúdo/campos: `_prova_arquivo` faz `stat` de pisos/cota e compara dispositivo/inode para impedir que arquivos do próprio mecanismo, incluindo hardlinks, provem ESPERANDO. (`bin/hook_stop_fila1.py:583-599`)
- Escritores/leitores de **fixtures**, sem efeito comprovado em produção: semeadura em `bin/tests/test_orcamento_contexto.sh:103`; semeadura/manipulação/asserções em `bin/tests/test_disjuntor.sh:58`, `:130`, `:133`, `:135`, `:149-154`, `:579-580`.

**`logs/sessao/cota.json`**

- Escritor operacional direto: `statusline_cota.main`; stdin com `rate_limits.seven_day` deve fornecer percentual e reset numéricos finitos (bool rejeitado), grava tempfile e `os.replace`. Sem esses dois valores não substitui a cota existente. (`bin/statusline_cota.py:26-30`, `bin/statusline_cota.py:35-54`)
- Evento: **statusLine**, não SessionStart/Stop/PreCompact; comando configurado em `.claude/settings.json:74-76`; há o mesmo trecho de configuração em `bin/disjuntor_settings.json:17-19`. Disparo real no host [nao verificado].
- Campos escritos: `seven_day_pct`, `seven_day_resets_at`, `hora=time.time()`, `contexto_tokens=context_window.total_input_tokens`, `contexto_pct=context_window.used_percentage`, `session_id` do payload. (`bin/statusline_cota.py:51-53`)
- Único leitor operacional direto localizado de conteúdo: `disjuntor.ler_cota`; lê **somente** `seven_day_pct` e `seven_day_resets_at`; reset ≤agora, tipos inválidos, NaN/inf ou exceção retornam `None`. Não usa idade/sessão/contexto para decidir. (`bin/disjuntor.py:1064-1076`; declaração explícita dos campos forenses em `bin/statusline_cota.py:11-13`)
- Leitores derivados: `escada` chama `ler_cota` e compara com 85%; `b4_decide` chama `escada`, depois `ler_cota` e compara com 75%. (`bin/disjuntor.py:85-86`, `bin/disjuntor.py:1108-1129`)
- Consumo em PreToolUse: Agent novo passa por `b4_decide`; hook registrado em `.claude/settings.json:62-68`. (`bin/disjuntor.py:1451-1474`)
- Consumo em Stop: `_disjuntor_contido('b4',...)` chama `b4_stop`, que consulta `escada`. (`bin/hook_stop_fila1.py:1034-1044`; `bin/disjuntor.py:1204`)
- Consumo em painel: `disjuntor.ler` inclui `ler_cota`; `linha` imprime porcentagem ou `nao sei`; `gerar_estado` inclui essa linha. (`bin/disjuntor.py:1619`, `bin/disjuntor.py:1690-1695`; `bin/gerar_estado.py:280-281`)
- Leitor apenas de metadados: proteção ESPERANDO já descrita. (`bin/hook_stop_fila1.py:585-599`)
- Fixtures: helper chama escritor real em raiz de teste (`bin/tests/test_disjuntor.sh:435-437`); escrita direta de NaN (`:453`); leituras/asserções de conteúdo (`:445`, `:456`). Não foram executadas.

**C1 / “desde o clear” / marco velho**

- C1 é efetivamente avaliado em `orcamento_contexto.avaliar`, chamado pelo Stop; `disjuntor.py` apenas apresenta seu gatilho no trecho dos pisos. (`bin/hook_stop_fila1.py:904`; `bin/orcamento_contexto.py:340-345`; `bin/disjuntor.py:1669-1674`)
- Base temporal da avaliação: `marco['hora']` se marco existe; senão primeiro timestamp do transcript. Calcula horas e deploys desde essa base, mas **só tokens ≥gatilho entram em `motivos`**; horas/marcos não disparam C1. (`bin/orcamento_contexto.py:327-345`)
- ESTADO: `gerar_estado` chama `orcamento_contexto.ler/linha`; `ler` usa marco atual ou `ultima_medida.base_epoch`; recalcula horas/deploys, porém contexto vem da última medida salva pelo Stop. (`bin/gerar_estado.py:274-276`; `bin/orcamento_contexto.py:352-379`)
- Linha literal: `contexto: ... | desde o clear: %.1fh, ... marcos | CLEAR atendido ... x seguiu sem clear ...`. Quando sem marco há aviso “desde o inicio do transcript”; sem ambas as bases, “nao sei”. (`bin/orcamento_contexto.py:382-398`)
- **Marco velho de dias não expira nem recebe aviso específico:** horas continuam aumentando, ainda com rótulo “desde o clear”, mesmo se a origem atual for compact; os pisos também permanecem utilizáveis sem prazo, exibindo a data antiga. (`bin/orcamento_contexto.py:374-398`, `bin/orcamento_contexto.py:210-232`; `bin/disjuntor.py:1672-1674`)
- Contexto antigo no painel do disjuntor mostra somente `Stop HH:MM`, sem data/idade, enquanto pisos mostram data completa. (`bin/disjuntor.py:1686-1687`, `bin/disjuntor.py:1672-1674`)
- O limite de 24h do painel de disjuntores não expira os pisos/cota: pisos vêm de `OC.ultimo_piso` sem corte, cota vale até reset. (`bin/disjuntor.py:1614-1619`; `bin/orcamento_contexto.py:210-222`; `bin/disjuntor.py:1069-1074`)

Nenhum leitor/escritor operacional desses arquivos foi localizado em `app/` pela busca de caminhos, nomes das funções e campos; exclusividade além desses usos textuais [nao verificado].

## 05/11 16:04 · sessao 4aafdc5cd5e1 · - · - · origem=Ronald (tmux)

Achados estáticos, sem banco, logs ou execução de testes; incidência em produção [não verificado]. [nao verificado]

1. **Sete parâmetros CCT são cadastrados, mas não chegam ao motor. Certeza alta; dono ESTRUTURA.** Lista: adicional noturno, tolerâncias por marcação/dia, intrajornada mínima, compensação autorizada/janela e teto de prorrogação; o formulário os recebe, mas a fábrica injeta somente quatro outros parâmetros (`app/core/views_config.py:275`; `app/core/regua_cct.py:72`; `app/core/regua_cct.py:458`). **A tela avisa explicitamente “sem efeito no cálculo”**; não caracterizar como ocultação (`app/templates/core/config/sindicato_form.html:57`, `:62`, `:67`, `:74`, `:79`, `:84`; checkbox compensação perto de `:110`).

   Contraexemplo concreto: CCT vigente com `intrajornada_minima_min=30`; escala diurna de oito horas, intervalo cadastrado e realizado de 30min, sem `parametros` especial. O valor 30 é aceito pelo modelo, porém `_intra_minima` calcula `max(intervalo_cadastrado, piso_intervalo_art71(previsto))`; previsto acima de 360min retorna piso60, produzindo 30min indenizados, apesar da CCT30 (`app/core/models.py:85`; `app/ponto/motor_calculo_v2.py:1824`; `app/escala/models.py:17`; `app/ponto/motor_calculo_v2.py:2771`). A parcela segue para evento `intrajornada_diurna` pelo fechamento (`app/ponto/services/fechamento.py:988`; `app/folha/export.py:437`). **Especulação:** conformidade jurídica da CCT e necessidade de aplicar30 [não verificado].

2. **Tolerâncias de dinheiro são constantes próprias, independentes das tolerâncias CCT. Certeza alta; dono ESTRUTURA.** HE diária usa10min, pontualidade usa10min por ponta, intrajornada ignora supressão até10min (`app/ponto/motor_calculo_v2.py:51`, `:60`, `:69`; consumidores `:2006`, `:2776`, `:2845`, `:3138`).

   Contraexemplo: CCT `tolerancia_marcacao_min=20`, jornada 08–12/13–17, primeira entrada08:15 e demais batidas nos marcos, sem ausência/abono. `aplicar_tolerancia` devolve15min de atraso porque compara com constante10; alterar20→30 na CCT não muda esse julgamento (`app/ponto/motor_calculo_v2.py:1973`; `:2006`; parâmetros não injetados em `app/core/regua_cct.py:458`). Exportador soma atraso+antecipada no evento parcial (`app/folha/export.py:440`). Resultado completo em TXT depende dos demais portões [não verificado].

3. **Faixa diária HE50/HE100 contém120 literal fora de cadastro. Certeza alta; dono ESTRUTURA.** O motor separa primeiras120min em HE50 e excedente em HE100; ocorre no cálculo12x36 e no fechamento diário do turno partido (`app/ponto/motor_calculo_v2.py:2849`; `:3141`). `teto_prorrogacao_dia_min` é cadastrado e validado, mas não injetado (`app/core/models.py:70`; `:87`; `app/core/regua_cct.py:458`).

   Contraexemplo: dia normal, previsto conhecido, janela HE autorizada, sem intra suprimida, excedente150min; código lança120min HE50 e30min HE100, inclusive com teto CCT60 (`app/ponto/motor_calculo_v2.py:2847`). **Não equiparar** teto máximo de prorrogação à faixa percentual: código comprova ambos os fatos, mas regra esperada para faixa, limite ou recusa [não verificado].

4. **Toda CCT vigente força régua `legais`; efeito restrito ao turno partido. Certeza alta; dono ESTRUTURA.** `regua_para` constrói parâmetros e sobrescreve `regua_excedente='legais'`, enquanto TP limita jornada a480 e acrescenta ganho de hora reduzida (`app/core/regua_cct.py:374`; `:378`; `app/ponto/motor_calculo_v2.py:3119`). **Sindicato não possui campo persistido `regua_excedente`**; não apresentar como valor cadastrado sobrescrito (`app/core/models.py:34`–`:74`).

   Contraexemplo: TP diurno, previsto e realizado540min líquidos, sem feriado e sem supressão de intra. Com switch CCT ligado e aplicação vigente, limite cai a480 e diferença60 vira HE50; em `relogio`, limite540 produz zero (`app/core/regua_cct.py:458`; `app/ponto/motor_calculo_v2.py:3115`; `:3122`; `:3131`; `:3141`). A condição de aplicação usa empresa/praça/vigência (`app/core/regua_cct.py:253`; `:261`; `:265`). Adequação da regra ao contrato concreto [não verificado].

Cuidado adicional: não afirmar que o motor multiplica horas pelo adicional CCT20/30%. O evento exportado é a quantidade `horas_noturnas`; percentual aplicado pelo Domínio efetivo [não verificado] (`app/folha/export.py:436`). O catálogo tem percentual, mas o arquivo importado em produção e sua correspondência ao cadastro deste hash [não verificado] (`app/folha/models.py:273`).

Nenhum desses cenários prova ocorrência em qualquer `colNN`; o anexo fornecido contém lookup de autoridades, sem cadastro, batidas ou TXT observados [não verificado].

## 05/11 16:04 · sessao 192b3ea36465 · - · - · origem=Ronald (tmux)

**Stop atual.** O código lê JSON de stdin, extrai somente `transcript_path` quando string e ignora `session_id`; payload ilegível resulta em transcript ausente. As fixtures enviam ambos os campos, mas a presença no payload real de produção é [nao verificado]. (`bin/hook_stop_fila1.py:1184-1192`; `bin/tests/test_disjuntor.sh:82`; `bin/tests/test_orcamento_contexto.sh:108`.)

O Stop já lê o transcript **indiretamente**:

- Piso: chama `OC.piso_do_ciclo` em `bin/hook_stop_fila1.py:899`; esta lê o arquivo inteiro por `_ler_transcript`, encontra `compact_boundary` e `/clear` e procura o primeiro turno principal com usage. (`bin/orcamento_contexto.py:240-290`.)
- Contexto: chama `OC.avaliar` em `bin/hook_stop_fila1.py:904`; este chama `medir_contexto`, que lê os últimos 4 MiB e usa o último assistant sem `isSidechain`. (`bin/orcamento_contexto.py:64-65`, `bin/orcamento_contexto.py:89-133`, `bin/orcamento_contexto.py:327`.)
- Fallback sem marco: `avaliar` chama `inicio_do_transcript`, que procura o primeiro timestamp. (`bin/orcamento_contexto.py:136-149`, `bin/orcamento_contexto.py:330-334`.)
- Guarda de clear/agentes: `_guarda_clear` chama `DJ.agentes_sem_entrega`, que abre o transcript principal e reconhece lançamentos e notificações de término. (`bin/hook_stop_fila1.py:785-795`; `bin/disjuntor.py:366-382`.)
- B4/escada: Stop chama `DJ.b4_stop`, que usa `escada` e eventualmente `ultima_humana`, leitora dos últimos 2 MB. (`bin/hook_stop_fila1.py:1043-1044`; `bin/disjuntor.py:1202-1204`, `bin/disjuntor.py:1108-1117`, `bin/disjuntor.py:1079-1105`.)
- B3: Stop chama `DJ.julga_b3`; `decide_b3` chama `_reprovacoes_finais`, que percorre os transcripts dos verificadores no diretório derivado do transcript principal. (`bin/hook_stop_fila1.py:1048`; `bin/disjuntor.py:977-983`, `bin/disjuntor.py:878`, `bin/disjuntor.py:736-750`, `bin/disjuntor.py:335-339`.)

**Não há hoje decisão “houve clear/compact no transcript desde o marco” que grave outro marco.** O Stop lê o marco existente, compara `ref` com o estado e reage quando **o arquivo já mudou**; usa o transcript para calcular piso/contexto, sem comparar `session_id` do payload nem chamar `marco_de_clear.py`. (`bin/hook_stop_fila1.py:862-904`, `bin/hook_stop_fila1.py:1184-1192`.)

**Hooks na cópia.** Stop chama `hook_stop_fila1.py`; PreCompact manual/auto chama somente `handoff_sessao.sh`; SessionStart compact/clear chama `marco_de_clear.py` com a respectiva origem. A configuração usa caminhos absolutos `/home/ronald/saas-hasner`, não o caminho desta cópia. A alegação de SessionStart inerte em sessão `app/` é dado do contrato; sua execução real não foi comprovada por código/log. (`.claude/settings.json:3-59`; `contrato.txt:3` do anexo; produção [nao verificado].)

**Lacunas do contrato** — numeração do anexo: cabeçalho=1, LEI=2, CAUSA=3, itens 1–5=4–8:

| Caso | Lacuna demonstrável |
|---|---|
| Duas sessões simultâneas | O contrato detecta novidade por `session_id != marco`, mas mantém um único marco global. A→B→A pode tornar sessões antigas “novas” repetidamente; não define propriedade por sessão nem ordenação dos eventos concorrentes. O escritor atual tem `flock`, que serializa escrita, porém conserva só um `session_id`; isso não resolve semanticamente a alternância. (`contrato.txt:4`, `contrato.txt:6`; `bin/marco_de_clear.py:20`, `bin/marco_de_clear.py:38-45`, `bin/marco_de_clear.py:61`.) |
| Transcript de agente | O contrato não define validação de que o transcript pertence à sessão principal. Stop aceita qualquer string em `transcript_path`; os leitores de contexto/piso descartam linhas `isSidechain`, mas não validam identidade de arquivo/sessão. Aceitação de transcript de agente pelo novo gravador é [nao verificado]. (`contrato.txt:4-6`; `bin/hook_stop_fila1.py:1187-1190`; `bin/orcamento_contexto.py:112`, `bin/orcamento_contexto.py:240-250`, `bin/orcamento_contexto.py:270`.) |
| Compact sem Stop posterior | A dependência do primeiro Stop está declarada, mas “latência = 1 turno” não cobre ausência definitiva desse Stop nem dá prazo real; sem Stop não há chamada prevista. (`contrato.txt:4`, `contrato.txt:6`.) |
| Clear seguido de compact antes do primeiro Stop | Contrato não determina replay dos dois eventos, ordem nem conservação do clear para ATENDIDO e pisos das duas origens. O escritor atual preserva até 20 `clear_horas`, e Stop usa esse histórico para reconhecer ATENDIDO mesmo após compact; chamar só o último evento perde esse insumo. (`contrato.txt:4-7`; `bin/marco_de_clear.py:33-57`; `bin/hook_stop_fila1.py:873-890`.) |
| Relógio | Contrato exige hora do evento, mas não define timezone, timestamp ausente/inválido/futuro, relógio regressivo nem critério para evento antigo descoberto depois. O escritor atual ignora timestamp recebido e usa `datetime.now()` dentro da trava; leitores subtraem epoch do marco do relógio da borda. Isso exige mudança de interface/semântica para cumprir o contrato. (`contrato.txt:4`, `contrato.txt:6`; `bin/marco_de_clear.py:41-45`; `bin/orcamento_contexto.py:84-86`, `bin/orcamento_contexto.py:330-337`; `bin/hook_stop_fila1.py:844`.) |
| Idempotência | É exigida, mas sem chave de evento/critério descrito. Escritor atual sempre usa nova hora e acrescenta clear, portanto duas invocações iguais não equivalem a um único marco. (`contrato.txt:6`; `bin/marco_de_clear.py:43-57`.) |

O contador “eventos no transcript × marcos gravados” está pedido, mas o contrato não define universo (sessão, janela, histórico) nem como contar eventos reconstruídos versus repetidos; desenho e campos futuros além do que o contrato explicita são [nao verificado]. (`contrato.txt:7`.)

## 05/11 16:03 · sessao ea5aa115e9c0 · - · - · origem=Ronald (tmux)

Censo de `ultimo_clear.json`, somente leitura:

| Papel/campo | Evidência e uso |
|---|---|
| Escritor operacional | `bin/marco_de_clear.py:44-61`: reconstitui objeto e faz `os.replace`; chamado por SessionStart compact/clear em `.claude/settings.json:34-57`. |
| Leitor direto do arquivo | `bin/orcamento_contexto.py:152-164`: `ler_marco`; escritor relê somente `clear_horas` anterior em `bin/marco_de_clear.py:50-57`. |
| `hora` | Escrita com relógio local atual dentro da trava, `bin/marco_de_clear.py:38-44`; normalizada para float em `bin/orcamento_contexto.py:158`; participa de `ref` em `:161`; base de horas/deploys em `:331-337` e ESTADO em `:374-377`. |
| `iso` | Escrita como horário local sem offset em `bin/marco_de_clear.py:44`; excluída da projeção de `ler_marco`, `bin/orcamento_contexto.py:160-162`; outros leitores não encontrados [nao verificado]. |
| `origem` | Vem de argv ou fallback payload `source`, `bin/marco_de_clear.py:32`; normalizada em `bin/orcamento_contexto.py:160`; identifica base em `:331`, mensagem RESET em `bin/hook_stop_fila1.py:889-890`, origem do piso em `:899-903`. |
| `session_id` | Vem do payload e vira string, `bin/marco_de_clear.py:45`; normalizado em `bin/orcamento_contexto.py:160`; entra em `ref` em `:161`; Stop compara apenas ref anterior/novo em `bin/hook_stop_fila1.py:863-870`. |
| `source` | Vem do payload, `bin/marco_de_clear.py:45`; excluído da projeção em `bin/orcamento_contexto.py:160-162`; outros leitores não encontrados [nao verificado]. |
| `clear_horas` | Escritor preserva números anteriores, acrescenta `hora` se origem clear e limita aos últimos 20, `bin/marco_de_clear.py:52-57`; leitor normaliza e deriva `clear_hora=max` em `bin/orcamento_contexto.py:159-162`; Stop filtra clears posteriores ao disparo e usa janela para ATENDIDO/SEGUIU em `bin/hook_stop_fila1.py:877-887`. |
| `clear_hora` | Campo derivado, não persistido pelo escritor: `bin/orcamento_contexto.py:162`, `bin/marco_de_clear.py:44-57`; consumidor além do retorno não encontrado [nao verificado]. |

Leitores indiretos operacionais:

- Stop lê via `OC.ler_marco`, usa ref para encerrar/rearmar ciclo e `origem` para registrar piso; passa o mesmo marco ao avaliador: `bin/hook_stop_fila1.py:862-904`.
- `avaliar` lê marco quando não recebido e calcula horas/deploys desde `hora`: `bin/orcamento_contexto.py:328-337`.
- `ler` lê marco para a linha do ESTADO; `gerar_estado` chama `ler/linha`: `bin/orcamento_contexto.py:361-398`, `bin/gerar_estado.py:274-276`.
- CLI chama `ler/linha` com `--linha` ou `avaliar` no caminho comum: `bin/orcamento_contexto.py:401-415`.
- Disjuntor lê pisos/gatilho e estado de orçamento, não usa `ler_marco` nesses caminhos: `bin/disjuntor.py:1594-1617`.

Testes, separados dos escritores operacionais: [nao verificado]

- `bin/tests/test_orcamento_contexto.sh:91-94` escreve marco fictício com hora/origem/session_id; `:258-261` chama escritor e lê origem/session_id por grep; `:384` escreve hora/origem; `:465-473` cria FIFO/lixo; `:486-488` lê e altera clear_horas.
- `bin/tests/test_disjuntor.sh:139`, `:145`, `:157`, `:163` chama escritor compact com `HOOK_STOP_RAIZ` da árvore fictícia.
- `bin/tests/test_handoff_sessao.sh:57-79` lê configuração e valida comandos SessionStart, não o conteúdo do marco.

Concorrência/idempotência:

- Há lock exclusivo para leitura/preservação/escrita e replace atômico: `bin/marco_de_clear.py:38-61`.
- A localização é global da raiz, não particionada por sessão: `bin/marco_de_clear.py:19-20`; o session_id apenas compõe o último objeto e ref: `:45`, `bin/orcamento_contexto.py:161`.
- Cada execução obtém nova hora atual e clear acrescenta essa hora; não existe deduplicação por identidade de evento nesse escritor: `bin/marco_de_clear.py:43-57`.
- O escritor atual não recebe hora do transcript; usa `datetime.datetime.now()`: `bin/marco_de_clear.py:43-45`.
- O parser de piso filtra `isSidechain` no primeiro turno, mas `_marcos_do_transcript` não filtra sidechain em compact_boundary: `bin/orcamento_contexto.py:253-274`.
- O relato antigo lista `deploy.sh` como leitor em `app/docs/RELATO.md:809`; a relação executável encontrada é o orçamento lendo o histórico de deploy em `bin/orcamento_contexto.py:167-192`. Leitura de ultimo_clear por deploy.sh [nao verificado].

## 05/11 16:02 · sessao 68da65759605 · CODEX-SUFOCA-09-selo-nao-morde · leitura · origem=Code

**Há selos que não mordem os mutantes abaixo.** Dois aceitam períodos vazios; outro aceita universo vazio; dois verificam a ligação da cláusula de feriado sem garantir seu efeito. As referências estão nos achados. Execução dos mutantes e sobrevivência à suíte completa: **[não verificado]**. [nao verificado]

Os contraexemplos adicionais são construídos, não ocorrências de produção. O anexo fornecido contém apenas a tabela de autoridades; pessoas afetadas, frequência e prejuízo: **[não verificado]**.

1. **Atraso correto mesmo apagando todo o trabalho — certeza alta, por análise estática.**  
   `test_sem_ec_caminho_atual` verifica somente `all(p.minutos_atraso == 0 for p in r.periodos)`. Não exige período algum. O próprio cenário entrega escala 12x36, 07–19, com batidas E07:05/S19:00 em 21/07/2026, omitindo `escala_colaborador`. [app/ponto/tests/test_motor_dna.py:56](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/tests/test_motor_dna.py:56)

   **Mutante concreto:** inserir no início de `MotorBase.calcular_mes`: `if escala_colaborador is None: return ResultadoMes(colaborador_id=colaborador_id or 0)`. O resultado nasce com `periodos=[]`: `all([])` aprova, enquanto `horas_trabalhadas` vira zero. O teste irmão informa vínculo e não atravessa esse desvio. (`app/ponto/motor_calculo_v2.py:2265`, `:274`, `:295`; `app/ponto/tests/test_motor_dna.py:49`)

2. **Dobra de feriado aprovada por `0 == 0` — certeza alta, por análise estática.**  
   `test_feriado_no_dia_do_turno_dobra_o_turno_inteiro` compara minutos de feriado com minutos trabalhados, ambos somados da mesma lista. Não fixa quantidade positiva nem preservação do turno. [app/ponto/tests/test_motor_comercial_dia_do_turno.py:76](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/tests/test_motor_comercial_dia_do_turno.py:76)

   **Contraexemplo do próprio teste:** comercial 18–03, pausas 21–22 e 00:30–01; em 10–11/06/2026, E18/S21/E22/S00:30/E01/S04:30; feriado no dia 10. (`app/ponto/tests/test_motor_comercial_dia_do_turno.py:16`, `:52`, `:56`, `:77`)

   **Mutante concreto:** no início de `MotorComercial.calcular_mes`, `if self.feriados: return ResultadoMes()`. A dobra passa como `0 == 0`; o irmão com feriado no dia seguinte também aceita zero. Os dois demais testes desse arquivo recebem feriados vazios. (`app/ponto/motor_calculo_v2.py:3295`; `app/ponto/tests/test_motor_comercial_dia_do_turno.py:61`, `:68`, `:72`, `:80`)

3. **Selo “UNIVERSO” não coloca ninguém no universo — certeza alta, por análise estática.**  
   `test_MORDE_itens_e_o_UNIVERSO_e_nao_so_os_retidos_por_furo` exige apenas `total == len(itens)` e `all('status' in item...)`. Sua classe não cria `FechamentoMensal`; o próprio arquivo reconhece essa fixture vazia. O universo produtivo vem dos fechamentos. [app/folha/tests/test_haiku_fora_da_folha.py:300](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/tests/test_haiku_fora_da_folha.py:300) (`app/folha/tests/test_haiku_fora_da_folha.py:318`; `app/folha/export.py:764`)

   **Mutante concreto:** trocar `_out['itens'] = itens` por `_out['itens'] = []`. Nesse teste, continuam verdadeiros `0 == len([])` e `all([])`. **Contraexemplo construído:** acrescentar um fechamento na competência; `total=1`, mas a pessoa desaparece de `itens`. A existência desse estado em produção é **[não verificado]**. (`app/folha/fora_da_folha.py:195`, `:199`)

4. **Cláusula presente no texto, desligada no TXT — certeza alta, por análise estática.**  
   `test_MORDE_os_dois_chamadores_do_export_leem_a_clausula` conta ocorrências de strings no fonte. Não verifica o valor efetivamente recebido pelos dois chamadores. [app/folha/tests/test_export_59a.py:58](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/tests/test_export_59a.py:58)

   **Mutante concreto:** acrescentar `and False` ao argumento de um chamador:
   ```python
   feriado_12x36_em_dobra=feriado_12x36_em_dobra(fech.colaborador, fech.mes, fech.ano) and False
   ```
   A substring contada permanece intacta. **Contraexemplo construído:** fechamento 12x36, cláusula verdadeira, 20h de HE100 de feriado e rubrica configurada; o chamador suprime as 20h. Os testes diretos demonstram a diferença entre passar `True` e `False`, mas não atravessam essa montagem. (`app/folha/export.py:713`, `:411`; `app/folha/tests/test_export_59a.py:48`, `:54`)

5. **“Data da competência” significa apenas “alguma data” — certeza alta, por análise estática.**  
   O teste substitui `regua_para` por retorno constante verdadeiro e verifica somente se o argumento é `datetime.date`. [app/folha/tests/test_export_59a.py:65](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/tests/test_export_59a.py:65)

   **Mutante concreto:** em `feriado_12x36_em_dobra`, trocar `[1]` por `[0]`: consulta o início da janela em lugar do fim e continua satisfazendo o teste. (`app/folha/export.py:359`)

   **Especulação rotulada — cadastro sintético:** empresa em regime CCT, corte 21, vínculo 12x36 e aplicação única de convenção com dobra vigente desde 01/09/2026. Na competência 09/2026, o início é 21/08 e o fim 20/09; consultar o início perde a convenção e pode suprimir as 20h do exemplo anterior. A janela, o filtro de vigência e a supressão estão no código; ocorrência desse cadastro em produção é **[não verificado]**. (`app/ponto/janelas.py:10`; `app/core/regua_cct.py:261`, `:363`; `app/folha/export.py:411`)

6. **Prova do TXT concorda consigo mesma mesmo ignorando valores — certeza alta, com cobertura complementar.**  
   O selo L113 A0 compara o hash gravado com a mesma `hash_do_txt`; suas diferenças de conteúdo usam letras, como `bbb → ccc`. Isso deixa sobreviver, nesses testes, uma função que ignore alterações numéricas. [app/folha/tests/test_l113_a0_forma_do_txt.py:106](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/tests/test_l113_a0_forma_do_txt.py:106) (`app/folha/tests/test_l113_a0_forma_do_txt.py:115`, `:137`, `:158`; `app/folha/models.py:176`)

   **Mutante concreto**, substituindo o retorno de `hash_do_txt`:
   ```python
   return hashlib.sha256(
       ''.join('0' if c.isdigit() else c for c in conteudo).encode()
   ).hexdigest()
   ```
   **Contraexemplo construído:** uma linha da matrícula 4321, rubrica 0150, muda de 8,00 para 6,00 horas. Só os dígitos mudam; o hash mutado permanece igual e o conferidor responde `divergente=False`. (`app/folha/export.py:34`, `:542`, `:655`)

   **Limite comprovado:** outro selo compara conteúdo numérico com SHA256 independente e detectaria esse mutante. Portanto este achado não demonstra ausência de proteção na suíte inteira. (`app/folha/tests/test_txt_invalidado_com_trilha.py:56`)

Não classifiquei como defeito o simples uso de fechamento preparado para testar exportação: o contrato do módulo recebe valores já lavrados e traduz para TXT. Isso limita o que o teste prova sobre batidas → cálculo, mas não torna seu teste de tradução inválido. (`app/folha/export.py:4`)

Também há controles positivos numéricos de DSR e HE noturna — respectivamente 3h de HE/4h de reflexo e 1h de HE/1h de HE noturna. O `skipIf` localizado no retrato depende de Chromium e cobre apresentação; não o contei como skip de cálculo monetário. (`app/ponto/tests/test_dsr_reflexo_sumula172.py:67`; `app/ponto/tests/test_he_noturna_sumula264.py:39`; `app/folha/tests/test_retrato_folha_smoke_chromium.py:223`)

Mock da própria função usado enganosamente como prova de valor, skip condicional de cálculo, execução dos mutantes, impacto real e correspondência criptográfica da cópia ao hash informado: **[não verificado]**. Os donos abaixo são classificação analítica das falhas dos selos, segundo a separação L-099; não são classificação de pessoas ou batidas de produção. (`CLAUDE.md:176`)

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 (ESTRUTURA/CADASTRO/BATIDA) |
|---|---|---|---|---|
| `all([])` aprova trabalho apagado | `app/ponto/tests/test_motor_dna.py:63` | E07:05/S19:00; retorno mensal vazio sem EC informado | Alta, estática | ESTRUTURA |
| Dobra aceita `0 == 0` | `app/ponto/tests/test_motor_comercial_dia_do_turno.py:78` | Turno completo 18–04:30 desaparece quando há feriado | Alta, estática | ESTRUTURA |
| Universo vazio aprova omissão | `app/folha/tests/test_haiku_fora_da_folha.py:308` | Um fechamento existente; `itens=[]` | Alta, estática | ESTRUTURA |
| Contagem textual aceita cláusula falsa | `app/folha/tests/test_export_59a.py:63` | 12x36 com dobra e 20h de feriado; argumento recebe `and False` | Alta, estática | ESTRUTURA |
| Mock constante encobre data errada | `app/folha/tests/test_export_59a.py:71` | CCT começa em 01/09; consulta 21/08 em vez de 20/09 | Alta, estática | ESTRUTURA |
| Hash sem oráculo independente local | `app/folha/tests/test_l113_a0_forma_do_txt.py:110` | Rubrica 0150 muda 8h → 6h; hash ignora dígitos | Alta, estática; outro selo detecta | ESTRUTURA |

## 05/11 16:02 · sessao f36865a96600 · CODEX-SUFOCA-08-falso-zero · leitura · origem=Code

Há caminhos que publicam **zero ou `OK` sem medição válida**: exceções descartadas, fontes indisponíveis e universos vazios. Exemplos diretos estão no auditor de chamados e no DIFF do PDF. (`app/chamados/management/commands/auditar_invariantes_chamados.py:650–667,1166–1174,1835`; `app/relatorios/management/commands/diff_pdf_x_espelho.py:82–84,138–140`)

**Leitura da tabela:** os contraexemplos são **ESPECULAÇÃO ROTULADA — estados sintéticos**, não ocorrências atribuídas a colaboradores reais. Certeza **alta** significa que o caminho decorre diretamente do código; **média** identifica um recorte declarado que pode ser interpretado como garantia maior. Ocorrência, frequência e pessoas afetadas em produção: **[não verificado]**.

O contador citado no anexo, `copias_pergunta_2`, retornou zero nesta cópia. A validação somente em memória detectou filtros literais, mas deixou escapar a mesma janela por `**dict` ou dividida em duas instruções. Não encontrei erro de sintaxe no universo efetivamente varrido. Isso demonstra limites do detector, sem provar cópia escondida nesta árvore. (`app/docs/JUIZES.md:100–102`; `app/core/censo_pergunta2.py:65–94,98–115`; `bin/placar_code.sh:482–483`)

Há proteções que impedem generalizar a acusação: lastro indisponível contribui **1** ao total; pausa e retido calado publicam `cego`; falha externa do cartão × TXT propaga `None` para `falhas`. (`bin/placar_code.sh:111–123`; `app/folha/services/vigia_pausa.py:142`; `app/folha/vigia_retido_calado.py:54,64`; `app/folha/porta_export.py:664–675,764`)

Não consegui verificar banco, logs, migrações aplicadas, código carregado em produção, registros efetivamente excluídos ou identidade do hash por metadados Git: **[não verificado]**. Os coletores consultados fixam `juliani`; isso comprova o escopo, sem demonstrar tenant errado. Outros tenants e sua cobertura operacional: **[não verificado]**. (`app/config/crons.py:17–21`; `bin/placar_code.sh:116–117,470–471`)

O dono abaixo é o **defeito ou limite do instrumento**. Cadastro errado e batida incompleta não passam automaticamente a ESTRUTURA; classificar casos reais como CADASTRO/BATIDA depende dos insumos ausentes. (`CLAUDE.md:176`) A tabela enumera os caminhos identificados; cobertura integral dos demais contadores: **[não verificado]**.

| Achado | Arquivo:linha | Contra-exemplo — **ESPECULAÇÃO ROTULADA, sintético** | Certeza | Dono L-099 |
|---|---|---|---|---|
| **`copias_pergunta_2`: filtro equivalente invisível** | `app/core/censo_pergunta2.py:75–94`; `bin/placar_code.sh:482–483` | Novo leitor faz `filter(**{'data_inicio__lte': d, 'data_fim__gte': d})`, ou aplica início e fim em instruções separadas. A janela existe, mas o detector devolve vazio. | Alta | ESTRUTURA |
| **`copias_pergunta_2`: arquivo quebrado desaparece** | `app/core/censo_pergunta2.py:64–68` | Fonte contém uma cópia literal e erro de sintaxe no final: o arquivo inteiro retorna conjunto vazio. Erro assim nesta cópia: não encontrado na validação realizada. | Alta | ESTRUTURA |
| **`copias_pergunta_2`: exclusão por função inteira** | `app/core/censo_pergunta2.py:16–18,43–59,113–115` | `detectar_anomalias`, já em `OUTRA_PERGUNTA`, ganha também consulta de vigência de `EscalaColaborador`. A função continua integralmente subtraída do censo. | Alta | ESTRUTURA |
| **`pdf_x_espelho_divergentes`: exceção elimina empresa** | `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:55–63,112–114` | Empresa tem colaborador com PDF divergente, mas `classificar_export` lança exceção. O comando pula a empresa sem registrar erro e pode imprimir zero divergentes e zero medidos. | Alta | ESTRUTURA |
| **`pdf_x_espelho_divergentes`: dia ausente não diverge** | `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:78–85,97–105` | Espelho contém dia de 480 minutos; PDF devolve lista vazia. `pdf.get(data)` resulta `None`, o dia é ignorado e o contador permanece zero, inclusive com colaborador contado como medido. | Alta | ESTRUTURA |
| **DIFF do resumo: `DIFF ZERO` sem comparar** | `app/relatorios/management/commands/diff_pdf_x_espelho.py:58–62,75–84,138–140` | Nenhum colaborador casa com as empresas escolhidas, ou ambos os produtores retornam resumos vazios. `sem_dado` não impede a conclusão “MESMO número em todas as chaves”. | Alta | ESTRUTURA |
| **DIFF do resumo: interseção vazia vira igualdade** | `app/relatorios/management/commands/diff_pdf_x_espelho.py:89–93,125–140` | PDF devolve `{'trabalhadas': 8}` e espelho `{'total_relogio': 0}`. As diferenças de chaves são impressas separadamente; nenhuma chave comum diverge e ainda sai `DIFF ZERO`. | Alta | ESTRUTURA |
| **`cartao_x_txt_divergentes`: classificação descartada** | `app/relatorios/management/commands/cartao_x_txt_divergentes.py:44–61,71–75` | Há cartão divergente, mas `classificar_export` lança `ValueError` para sua empresa. Ela desaparece da comparação; sem outros itens, publica zero. | Alta | ESTRUTURA |
| **Porta do export: zeros restritos ao TXT** — `tela_x_pdf`, `topo_x_coluna`, `colabs_no_builder`, `minuto_em_duas_rubricas`, `espelho_x_dia_pago`, `he_pendente` | `app/folha/porta_export.py:469–515,532–601,718–729`; `app/folha/export.py:748–750` | Único colaborador tem batidas e divergência espelho 8h/PDF 0h, mas está sem código Domínio. Fica fora de `entra`; essas medidas permanecem vazias. **Recorte declarado:** o zero não certifica coerência da frota inteira. | Média: alcance declarado | ESTRUTURA da interpretação |
| **`lavratura_sem_furo_apurado`: falha só aparece no log** | `app/folha/porta_export.py:352,423–442,704` | Competência aberta tem `DiaPago` somável, mas a consulta de `furo_apurado` falha. O tratamento registra aviso, sem acrescentar divergência ou não medição; a lista publicada continua vazia. | Alta | ESTRUTURA |
| **`colabs_nao_certificados`: JSON válido sem universo** | `app/folha/services/certificacao.py:63–64,81–88`; `app/folha/management/commands/colabs_nao_certificados.py:15–22` | Arquivo de certificação existente contém `{}`. A função calcula `certificada=False`, mas `nao_certificados=0`; o comando publica apenas esse zero. | Alta | ESTRUTURA |
| **TXT: `q1.competencias_com_valor_movido`, `q2.diverge` e ambos `cego` zerados pelo filtro** | `app/folha/management/commands/vigiar_txt_dominio.py:54–59,113–120`; `app/folha/services/vigia_txt.py:200–204,346–351` | `--empresa` recebe ID inexistente. `empresas=[]` esvazia o acervo; divergência e cegueira ficam zero. Mesmo com `--alarme`, termina “OK — 0 divergência e 0 cegueira”. | Alta | ESTRUTURA |
| **`I5b`, `I5c`, `I5d`: `except: pass` por chamado** | `app/chamados/management/commands/auditar_invariantes_chamados.py:631,640–667,1835–1837` | Único chamado vivo está coberto por ausência aprovada, mas `ausencia_cobre` falha: I5b sai `OK 0`. Falha ao obter dia/cobertura também deixa I5c/I5d vazios. | Alta | ESTRUTURA |
| **`S6`: juiz falha, mas pergunta continua no denominador** | `app/chamados/management/commands/auditar_invariantes_chamados.py:1154–1174,1835` | Uma pergunta materializada, não stale, tem veredito lavrado; o juiz lança exceção. O comando pula a comparação e publica `OK 0`, anunciando “amostra de 1”. | Alta | ESTRUTURA |
| **`S6`: zero amostral não cobre o acervo** | `app/chamados/management/commands/auditar_invariantes_chamados.py:1147–1159` | Existem 301 perguntas elegíveis; a única divergente fica fora das 300 sorteadas. Zero significa ausência de divergência detectada na amostra, sem garantia sobre todas as perguntas. | Média: amostra declarada | ESTRUTURA da interpretação |
| **`I13`: zero literal, sem contagem** | `app/chamados/management/commands/auditar_invariantes_chamados.py:996–997,1835–1837` | Uma porta lateral adicional passa a existir. O invariante continua recebendo literalmente `0`, com referências estáticas, e aparece como `OK`. | Alta | ESTRUTURA |
| **`B5A lavrado_x_derivado` / `B5B julgada_sem_lavra`: vazio recebe “lavra íntegra”** | `app/ponto/management/commands/vigia_b5_ausencia.py:54–64,82,95–108` | Tenant tem células geradas, nenhuma julgada. `conferidas=0`, ambos os contadores são zero e a mensagem final afirma “lavra íntegra”. | Alta | ESTRUTURA |
| **`VIGIA-PLANTIO-OVERNIGHT achados`: erro vira `None`** | `app/ponto/management/commands/vigia_plantio_overnight.py:28–49,68–69` | Batida aprovada de origem `disputa_s84_retro` pertence a turno em folga; `eh_dia_trabalho` falha. `trab=None` não entra no ramo `is False`, deixando `achados=0`. Sem vínculo/tipo, também pula. | Alta | ESTRUTURA |
| **`bordas_orfas_de_par_fechado`: cegueira parcial não acende `cego`** | `app/ponto/services/vigia_pareador_celula.py:97–114`; `app/ponto/management/commands/vigia_pareador_celula.py:60–68` | ColA tem um turno posicionável sem erro. ColB tem dois turnos fechados no mesmo dia e borda órfã; seus turnos são pulados. A existência do turno de A permite `contador=0, cego=0`, embora B não tenha sido conferido. `turnos_sem_posicao` fica disponível no universo. | Alta | ESTRUTURA |
| **`trilha_x_banco`: ausência de trilha/status não acusa** | `app/chamados/services/vigia_loop.py:114–124` | Chamado muda sem trilha: fica fora do universo. Alternativamente, última trilha contém `valor_depois={}`: existe universo, mas `st=None` impede a divergência. Ambos podem dar zero. | Alta | ESTRUTURA |
| **`trocas_de_estado_em_loop`: corte rompe o par** | `app/chamados/services/vigia_loop.py:28–47,55–58` | Agora 05/11 às 12h: fechamento em 04/11 às 11h59 e reabertura às 12h01, sem fato novo. Só a reabertura entra nas últimas 24h; não forma par e retorna zero. | Alta no limite temporal | ESTRUTURA do instrumento |
| **`escritas_por_cron[cron]`: só escrita auditada com ator exato** | `app/chamados/services/vigia_loop.py:64–68,84–99`; `bin/placar_code.sh:197–202` | Cron altera três chamados sem trilha, ou registra ator `sistema` em vez de `sistema:<cron>`. Publica zero escritas. **Limite explícito no rótulo:** não demonstra ausência de escrita fora da trilha. | Média: alcance declarado | ESTRUTURA da interpretação |
| **`crons_quebrados`: fonte ausente ou registro descartado** | `app/core/services/crons_quebrados.py:29–47`; `app/core/management/commands/cron_quebrado.py:24–29` | Cron morreu com exit 137, mas `placar.jsonl` está ausente/ilegível: retorna `[]` e imprime zero. Registro com JSON/data inválidos é descartado; registro sem `exit` assume sucesso. | Alta | ESTRUTURA |
| **`crons_declarados_nao_instalados` / `crons_instalados_nao_declarados`: erro sem diff vira zero** | `bin/placar_code.sh:35–39,459–460`; `bin/crons.sh:27` | Docker falha ao renderizar os crons. O comando sai 2 com “render falhou”; o coletor ignora o retorno e procura somente linhas `> `/`< `. Como não existem, publica ambos zero. | Alta | ESTRUTURA |
| **`credencial_sem_trilha`: teste não medido vira zero** | `bin/placar_code.sh:137–144,203` | Imagem de teste indisponível, ou execução falha antes do marcador. `sed` não extrai valor; `${_cred:-0}` publica zero. | Alta | ESTRUTURA |
| **`parado_citado_em_commit_posterior`: falta de histórico vira ausência de citação** | `bin/tickets_placar.sh:54–75,89–91`; `bin/placar_code.sh:49–51` | Há ticket parado citado em commit posterior, mas `git log` falha e fornece histórico vazio. Nenhuma citação é encontrada. Se o script inteiro falhar sem marcador, o coletor também substitui por zero. | Alta | ESTRUTURA |
| **`deploy_sem_contrato`: erro não reconhecido vira zero** | `bin/placar_code.sh:146–148,175` | `contrato_deploy.sh` está ausente ou ilegível. Execução falha, porém a mensagem não começa com `   - `. `grep -c` produz `"0"`; o fallback `:-1` não substitui esse valor. | Alta | ESTRUTURA |
| **`prompts_repetidos` / `testes_em_quarentena`: fonte indisponível vira zero** | `bin/placar_code.sh:394–397,527–529` | `PROMPTS.md` está ilegível ou `bc` indisponível: saída vazia vira zero. Arquivo de quarentena no caminho absoluto está ausente: `grep` falha e `echo 0` fornece o número. | Alta | ESTRUTURA |
| **`paginas_500_5min`: coleta falha ou filtro exclui parte dos 5xx** | `bin/selo_paginas_500.sh:8–19` | `docker logs` falha nas duas cascas: as mensagens não casam com o padrão e a soma fica zero. Também uma linha ` -> 510 ` não casa com `50[0-9]`, apesar do rótulo “5xx”. | Alta | ESTRUTURA |
| **`horas_esteira_parada_com_fatia_pronta`: eventos ausentes viram 0,0h** | `app/config/esteira.py:22–48,56–71`; `bin/placar_code.sh:509–510` | Fatia espera há duas horas, mas o arquivo de eventos sumiu. Leitor devolve `[]`, acumulador retorna `0.0`. JSON inválido ou evento sem timestamp também pode desaparecer da medição. | Alta | ESTRUTURA |
| **`TOTAL` do placar: indisponibilidade contribui zero** | `bin/placar_code.sh:470–473,549–554` | `cron_quebrado` falha sem marcador: a linha individual mostra `?`, porém `_n_cq=0`. Se as outras parcelas forem zero, o agregado anuncia `OK — nenhum contador ... aceso` e sai 0. | Alta | ESTRUTURA |
| **PDF/cartão/B5: tenant válido, porém vazio** | `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:55–56,112–114`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:44–51,75`; `app/ponto/management/commands/vigia_b5_ausencia.py:54–64,107–108` | Operador pretende avaliar tenant A, mas executa no tenant B, migrado e vazio, fornecendo competência quando necessária. As consultas vazias permitem zeros. **Tenant errado efetivamente utilizado: [não verificado].** | Alta no comportamento; ocorrência não verificada | ESTRUTURA do instrumento |

## 05/11 16:02 · sessao a80a75718350 · - · - · origem=Ronald (tmux)

Três achados estáticos, todos com dono **ESTRUTURA**:

1. **KPI chama de “pagas” horas sem apuração da folha.**  
   O topo imprime “Trabalhadas (pagas)” e o número sem consultar `fonte_dos_totais` — `app/templates/ponto/espelho.html:38-39`. Quando não há fechamento nem lavratura, `folha_manda` registra `fonte_dos_totais='sem apuracao ainda'`, mas mantém o resumo do motor — `app/relatorios/cartao_pela_celula.py:340-346`, `:203-204`; o motor alimenta esses totais em `app/ponto/services/espelho.py:439-441`.  
   **Contraexemplo:** vínculo 08–17, batidas 08/12/13/17 e nenhum `FechamentoMensal`/`DiaPago` na competência: topo publica 8h “pagas”; a linha diária, corretamente, pode informar “sem apuração ainda” — `app/templates/ponto/espelho.html:269-271`.  
   **Certeza alta.** Ocorrência em produção: **[nao verificado]**.

2. **“Nenhuma pendência” apesar de confirmação de batidas pendente na mesma página.**  
   O texto “Nenhuma pendência. O sistema não está pedindo nada de você” depende apenas de `not pendencias` — `app/templates/chamados/meu_atendimento.html:117-120`. Essa lista contém apenas chamados que não são solicitações — `app/chamados/views.py:613-614`. Regularizações externas aguardando são buscadas separadamente — `app/chamados/views.py:623-625` — e exibidas com o título “Confirmação de batidas pendente” e pedido de confirmar ou recusar — `app/templates/chamados/meu_atendimento.html:6-9`.  
   **Contraexemplo:** colaborador sem chamados no fio, com uma `RegularizacaoExterna(status='aguardando')`: há ação pedida no topo e “nenhuma pendência” na aba Pendências.  
   **Certeza alta.** Ocorrência em produção: **[nao verificado]**.

3. **Intrajornada indenizada descrita como intervalo “não registrado”, mesmo quando foi registrado parcialmente.**  
   O aviso e o tooltip dizem “intervalos não registrados” — `app/templates/ponto/espelho.html:85-87`, `:286`. O cálculo indeniza a diferença entre o mínimo e a intrajornada real, portanto também alcança intervalo registrado curto — `app/ponto/motor_calculo_v2.py:2771-2784`.  
   **Contraexemplo:** 12x36, mínimo de 60 minutos, batidas E07:00/S12:00/E12:30/S19:00: o par do intervalo registra 30 minutos; os outros 30 são indenizados, excedendo a tolerância de 10 minutos — `app/ponto/motor_calculo_v2.py:69`, `:2773-2784`. O aviso descreve o intervalo como não registrado.  
   **Certeza alta**, restrita à descrição do fato; não é avaliação da legalidade do cálculo.

Sem banco, logs ou execução de cenários, não verifiquei frequência, colaboradores atingidos, valores reais ou renderização em produção **[nao verificado]**. Os exemplos acima são estados concretos derivados dos ramos lidos; não são registros do dado raspado. Não precisei recorrer a especulação para esses achados.

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| “Pagas” sem apuração | `app/templates/ponto/espelho.html:38`; `app/relatorios/cartao_pela_celula.py:340` | Batidas completas 08/12/13/17; sem fechamento e sem DiaPago | Alta | ESTRUTURA |
| “Nenhuma pendência” com confirmação pendente | `app/templates/chamados/meu_atendimento.html:120`; `app/chamados/views.py:624` | Sem chamados no fio; regularização externa aguardando | Alta | ESTRUTURA |
| Intervalo registrado chamado de não registrado | `app/templates/ponto/espelho.html:87`; `app/ponto/motor_calculo_v2.py:2773` | Pausa registrada de 30 min diante de mínimo 60 min | Alta | ESTRUTURA |

## 05/11 16:02 · sessao 82d4017fdba8 · - · - · origem=Ronald (tmux)

Três achados sustentados por leitura estática; ocorrência em produção e entrega efetiva das notificações: [nao verificado].

1. **Aprovação de falta anuncia “dia abonado”.** A view aprova ausências abertas e envia incondicionalmente `"{n} dia(s) abonado(s)"` pelos dois canais: `app/ponto/views.py:2596`, `app/ponto/views.py:2633`, `app/ponto/views.py:2639`, `app/ponto/views.py:2643`. O gate aceita falta sem documento: `app/ponto/services/ausencia.py:600-618`; o catálogo declara falta como `DESCONTA`: `app/ponto/catalogo/ausencias.py:88`.
   - Contraexemplo: cadastro do tipo `falta` com efeito `desconta`; ausência de um dia aguardando decisão, sem documento e sem batidas; DP aprova. O tipo continua sem abono, mas a mensagem diz “1 dia(s) abonado(s)”.
   - Certeza **alta**; dono **ESTRUTURA**. Não depende de especulação sobre produção.

2. **Mudança de status sem conteúdo responde “Mensagem enviada”.** A ocorrência e os pushes só são produzidos quando `conteudo` não está vazio: `app/chamados/services/acoes_chamado.py:286-312`. Alterar apenas o status também marca `_fez=True`, fazendo retornar “Mensagem enviada”: `app/chamados/services/acoes_chamado.py:313-323`. O caminho está ligado à view: `app/chamados/views.py:547`.
   - Contraexemplo: gestor autorizado, chamado aberto comum; POST com `conteudo=''` e `novo_status='em_analise'`. Há transição de estado, sem ocorrência de resposta nem push, mas a confirmação afirma envio de mensagem. [nao verificado]
   - Certeza **alta**; dono **ESTRUTURA**.

3. **Reabertura afirma “Colaborador notificado” quando não existe token.** O fluxo reabre a disputa e cria pergunta: `app/chamados/services/acoes_disputa.py:252-264`. Chama o FCM ignorando seu retorno: `app/chamados/services/acoes_disputa.py:283-289`; depois anuncia “Colaborador notificado”: `app/chamados/services/acoes_disputa.py:295`. Sem token, o serviço retorna `ok=False, motivo='sem_token'`: `app/chamados/fcm_utils.py:101-106`.
   - Contraexemplo: disputa fechada reabrível, admin autorizado, motivo válido e justificativa ≥20 caracteres, colaborador com `fcm_token=''`. A reabertura ocorre e nenhum envio FCM acontece; a resposta afirma notificação.
   - Certeza **alta**; dono **ESTRUTURA**.

| achado | arquivo:linha | contraexemplo | certeza | dono L-099 |
|---|---|---|---|---|
| Falta aprovada anunciada como abonada | `app/ponto/views.py:2639` | Tipo `falta/desconta`, um dia, sem anexo; aprovação DP | Alta | ESTRUTURA |
| Status alterado anunciado como mensagem enviada | `app/chamados/services/acoes_chamado.py:323` | Conteúdo vazio; aberto → em análise | Alta | ESTRUTURA |
| Notificação afirmada sem canal FCM | `app/chamados/services/acoes_disputa.py:295` | Reabertura válida; token vazio | Alta | ESTRUTURA |

## 05/11 16:02 · sessao 4d32bad084a8 · SOMBRA-NAO-CABE-2 · verificador · origem=Code

A cura do **NÃO do toque** está correta, mas ainda há divergência temporal: o lote conserva empresa e tranca para outros colaboradores, inclusive depois de uma reabertura confirmada. Isso impede garantir o mesmo veredito do avulso. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974, app/chamados/juizes.py:1037–1044)

Identidade Git do commit/pai, execução dos testes e mutantes, ocorrência dos cenários em produção e pouso: [nao verificado]. [nao verificado]

1. **Quem escreve e quem lê.**

   O dono é escolhido nesta ordem: lote; requisição; instância do colaborador. As mesmas funções abaixo escrevem e consultam as chaves nos três casos; mudar o dono amplia a duração e o compartilhamento do resultado. (app/chamados/juizes.py:963–974)

   | Estado | Quem escreve | Quem lê |
   |---|---|---|
   | `_tranca_memo` | `_memo_da_tranca` cria no dono por `setdefault`; um teste também o remove explicitamente. (app/chamados/juizes.py:974, app/ponto/tests/test_selo_chokepoint_escrita.py:417) | `dia_da_pergunta_trancado` e `_admin_tocou_o_dia_depois`, através de `_memo_da_tranca`. (app/chamados/juizes.py:937, app/chamados/juizes.py:1037) |
   | `('empresa', empresa_id)` | `dia_da_pergunta_trancado` guarda `colab.empresa`. (app/chamados/juizes.py:938–940) | A mesma função entrega a empresa guardada a `janela_atual`. (app/chamados/juizes.py:941) |
   | `('tranca', empresa_id, mês, ano)` | A mesma função guarda o retorno de `periodo_trancado`, **inclusive `None`**. (app/chamados/juizes.py:942–944) | A mesma função reutiliza a resposta e, havendo objeto, seu `fechado_em`. (app/chamados/juizes.py:945–951) |
   | `('toque', colab.pk, data, desde)` | `_admin_tocou_o_dia_depois` grava somente `True`. (app/chamados/juizes.py:1041–1043) | A mesma função retorna imediatamente quando encontra SIM. (app/chamados/juizes.py:1038–1040) |
   | `('sistema',)` | `_toque_humano_no_dia` grava `get_user_sistema().pk`. (app/chamados/juizes.py:1062–1064) | O filtro de humanos exclui esse PK. (app/chamados/juizes.py:1065–1066) |
   | `LOTE_DA_TRANCA` | Declaração com `None`; `tranca_em_lote` instala `{}` e restaura o token em `finally`. (app/chamados/juizes.py:984–997) | `_memo_da_tranca` prioriza seu dicionário; o context manager verifica reentrância; os testes consultam sua presença. (app/chamados/juizes.py:967–969, app/chamados/juizes.py:990, app/chamados/tests/test_snc2_toque_em_lote.py:172–177) |
   | `REQUISICAO_DA_TRILHA` | O middleware instala a requisição e restaura seu token; o teste novo também instala/restaura um contexto. (app/core/middleware.py:76–82, app/chamados/tests/test_snc2_toque_em_lote.py:199–207) | `_memo_da_tranca` escolhe o dono fora do lote; `evento` obtém usuário/IP. São contextos distintos. (app/chamados/juizes.py:970–974, app/core/observ.py:108–115) |

   As chaves antigas `toques_lote` e a coleta plural foram substituídas pelo caminho único `_admin_tocou_o_dia_depois` → `_toque_humano_no_dia`; nenhum NÃO é gravado nesse memo de toque. (app/chamados/juizes.py:1024–1078)

   As fontes dessas chaves também têm escritores fora do diff:

   | Fonte | Escritores | Leitura relevante |
   |---|---|---|
   | Empresa do colaborador | Edição atribui `empresa_id`; transferência atribui a empresa do posto e salva pela porta do colaborador. (app/colaboradores/views.py:1313, app/colaboradores/views.py:1383–1388, app/colaboradores/services/colaborador.py:73–76) | `dia_da_pergunta_trancado` lê `empresa_id` e `empresa`. (app/chamados/juizes.py:932–942) |
   | Corte da empresa | Formulário recebe o corte e chama `atualizar_empresa`, que salva o objeto. (app/colaboradores/views.py:496–499, app/colaboradores/views.py:521–523, app/colaboradores/services/estrutura.py:17–20) | `janela_atual` → `corte_da_empresa` → `periodo_apuracao_atual`. (app/ponto/janelas.py:24–33, app/ponto/janelas.py:50–63) |
   | Tranca, reabertura e `fechado_em` | `trancar_periodo` cria/retranca; `reabrir_periodo` grava `reaberto_em`. (app/ponto/services/fechamento.py:1392–1403, app/ponto/services/fechamento.py:1421–1425) | `periodo_trancado` seleciona somente linha com `reaberto_em` nulo; o memo guarda sua resposta. (app/ponto/services/fechamento.py:1448–1454, app/chamados/juizes.py:944) |
   | Usuário sistema | `get_user_sistema` usa `get_or_create`; o comando garantidor cria e normaliza atributos. (app/core/utils.py:18, app/core/management/commands/garantir_user_sistema.py:13–41) | `_toque_humano_no_dia` memoiza e exclui seu PK; não consulta permissões ou perfil do autor da trilha. (app/chamados/juizes.py:1062–1066) |
   | Identidade da pergunta | `PerguntaDisputa.abrir` recebe disputa/data e cria a pergunta, associando célula do dia. (app/chamados/models.py:2387–2419, app/chamados/models.py:2465) | `dia_da_pergunta_trancado` lê `data_evento` e `disputa.colaborador`. (app/chamados/juizes.py:931–933) |

   A trilha consultada por `_toque_humano_no_dia` é esta:

   | Campos/ato | Quem escreve | Quem lê |
   |---|---|---|
   | `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `criado_em` | `registrar_log` adapta para `evento`; `evento` cria a linha; `criado_em` é automático. (app/core/auditoria.py:22–24, app/core/observ.py:118–137, app/core/models.py:295–307) | O helper filtra tempo estritamente posterior, usuário não nulo e diferente do sistema, ação/modelo/objeto e a marca do dia na descrição. (app/chamados/juizes.py:1065–1078) |
   | Cobrar | `cobrar_dia` escreve `cobrar_dia_manual`, modelo `Colaborador`, PK do colaborador e `dia=<ISO>`, antes do cartório. (app/chamados/services/cobrar_dia.py:29–39) | Primeiro `exists()` do helper. (app/chamados/juizes.py:1067–1069) |
   | Resolver/lavrar célula | O serviço escreve `veredito_celula`; **a porta da célula também escreve essa ação/modelo**, com ator, inclusive no caminho de lavra. (app/ponto/services/veredito_celula.py:329–336, app/ponto/portas/celula.py:770–789) | O helper consulta a ação sobre células atuais do colaborador/dia; não filtra `via`. (app/chamados/juizes.py:1070–1074) |
   | Desarquivar | A view chama `arquivo.levantar`; havendo escrita, a porta chama `_trilha` → `evento`. (app/chamados/views_arquivar.py:77–86, app/chamados/services/arquivo.py:193–209, app/chamados/services/arquivo.py:130–138) | O helper identifica chamados atuais do colaborador ligados às células daquele dia. (app/chamados/juizes.py:1075–1078) |
   | Célula: PK, colaborador, data | A porta cria células com colaborador/data. (app/ponto/portas/celula.py:247–248, app/ponto/portas/celula.py:601–604) | O helper busca suas identidades para Resolver e Desarquivar. (app/chamados/juizes.py:1070–1078) |
   | Chamado: PK, colaborador, célula | `abrir`/`criar` gravam o nascimento; `apontar_celula` preenche somente vínculo ausente. (app/chamados/models.py:913–932, app/chamados/models.py:1028–1048, app/chamados/services/vinculos.py:92–99) | O helper reconstrói o vínculo na consulta após cada resultado negativo. (app/chamados/juizes.py:1075–1078) |

   O contexto do comando cobre `censo`, `censo_alcance` e os três contadores adicionais. C6 é alcançado pelo fio, pela alimentação das portas, pelo app e pelos contadores de perguntas vivas. (app/chamados/management/commands/vigiar_fio_morto.py:171–176, app/chamados/services/vigia_fio.py:416, app/chamados/services/vigia_fio.py:603, app/chamados/juizes.py:1294–1297, app/chamados/juizes.py:1313, app/chamados/services/vigia_fio.py:754, app/chamados/services/vigia_fio.py:812)

   Fora do vigia, C6 também condiciona ressuscitação e carimbo; a leitura da tranca atende respostas sem veredito, emissor e reconciliador. Isso mantém esses leitores no alcance da revisão, sem demonstrar que abram lote em produção. (app/chamados/models.py:1447, app/chamados/models.py:2585–2598, app/chamados/models.py:1769–1776, app/chamados/services/disputa_emissao.py:1585–1589, app/chamados/reconciliador.py:610–616)

2. **Contraexemplos concretos durante o lote.**

   **Reabertura — lote=morta; avulso=viva.** Cenário construído: empresa E com corte 21, julho/2026 trancado; P1 de `col01`, dia **10/07/2026**, e P2 de `col02`, dia **11/07/2026**, sem validação, resposta, materialização ou toque. A forma de pergunta sem esses carimbos está representada pela fixture existente. (app/ponto/janelas.py:24–33, app/chamados/tests/test_c7_red_minimos.py:110–118)

   - P1 aquece o lote, guardando `('tranca', E, 7, 2026)=pf`. (app/chamados/juizes.py:942–945)
   - Durante a rodada, o admin reabre julho em outra requisição: a view busca seu próprio objeto e a porta confirma `reaberto_em`. (app/ponto/views.py:1541–1548, app/ponto/services/fechamento.py:1421–1425)
   - P2 é julgada **pela primeira vez**. O lote reutiliza `pf`, não encontra toque e C6 retorna `False`. O avulso fresco recebe `None` de `periodo_trancado` e, sem outros impedimentos, retorna `True`. (app/chamados/juizes.py:945–951, app/chamados/juizes.py:1130–1135, app/chamados/juizes.py:1201–1202, app/ponto/services/fechamento.py:1454)

   Usar **outro colaborador e pergunta ainda não julgada** distingue o defeito do memo por instância e de `_pv_memo`: o novo contexto compartilha a tranca entre colaboradores; sem lote, o colaborador fresco tem outro memo. O diff total fornecido introduz precisamente essa prioridade do lote. (app/chamados/juizes.py:835–847, app/chamados/juizes.py:967–974)

   **Tranca — lote=viva; avulso=morta.** Inverta o estado inicial: julho aberto, P1 guarda `None`; durante o lote o admin tranca julho; P2 de `col02`, sem toque posterior, continua viva pelo `None` guardado, enquanto a consulta fresca encontra a tranca. O gancho de trancar não elimina o exemplo: perguntas deixam de receber carimbo e morrem por leitura da tranca. (app/chamados/juizes.py:942–947, app/ponto/services/fechamento.py:1392–1412, app/chamados/services/competencia_trancada.py:8–11, app/chamados/juizes.py:1134–1135)

   **Corte — lote=morta; avulso=viva.** E começa com corte 21, julho trancado e agosto aberto; P1 aquece a empresa. Durante a rodada, o admin muda o corte para 10; P2 fresca de `col02`, dia **11/07/2026**, continua classificada em julho pela empresa guardada, enquanto o avulso usa agosto. A classificação histórica pelo cadastro atual é herdada; conservar o cadastro antigo entre colaboradores é efeito do novo dono. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974, app/colaboradores/views.py:496–523, app/colaboradores/services/estrutura.py:17–20, app/ponto/janelas.py:24–33)

   **Célula — diferença adicional do diff total contra a base 630131b1.** P2 de `col03`, dia **05/08/2026**, competência aberta, sem carimbos, tem célula `furo`. `perguntas_do_app` agora carrega a célula junto das perguntas. Entre esse carregamento e o julgamento de P2, o admin confirma Resolver dia=`trabalhou`, com ata sem marcos pendentes. A instância já carregada conserva `furo` e C6 retorna viva; no caminho anterior fornecido, a célula seria carregada em `_chave_memo_pv` depois do ato e o ramo `trabalhou`, sem pendências, retornaria morta. É diferença do instante de leitura, independente do memo da tranca. (app/chamados/services/pergunta_no_app.py:46–53, app/chamados/juizes.py:801, app/ponto/services/veredito_celula.py:325–330, app/escala/models.py:1759–1768, app/chamados/juizes.py:1175–1190)

   **Toque e chamado que ganha célula:** os contraexemplos específicos do v1 foram curados: após NÃO, a trilha e os vínculos são consultados novamente; os novos testes representam Cobrar durante a rodada e chamado antes sem célula. O segundo teste escreve diretamente a FK, sem exercitar `apontar_celula`. (app/chamados/juizes.py:1037–1044, app/chamados/juizes.py:1070–1078, app/chamados/tests/test_snc2_toque_em_lote.py:135–162, app/chamados/services/vinculos.py:92–99)

   Execução dessas intercalações, ocorrência em produção e estado de versões anteriores à base fornecida: [nao verificado].

3. **Regra de negócio alterada sem aval demonstrado.**

   A regra escrita é tranca **viva**, descontado o toque humano posterior; destrancar devolve a pergunta por leitura. No lote, isso passa a depender da primeira resposta obtida para aquela empresa/competência, mesmo depois de trancar ou reabrir. Essa diferença altera o insumo que decide o retorno de C6. Aval para essa diferença temporal: [nao verificado]. (app/chamados/juizes.py:919–951, app/chamados/juizes.py:967–974, app/chamados/juizes.py:1134–1135)

   O endereço registrado continua sendo C6, `pergunta_viva`, que delega a `_pergunta_viva_calc`. Não apareceu outro juiz completo; manter o endereço não preservou a atualidade da autoridade consultada. (app/core/juizes.py:266, app/chamados/juizes.py:822–848)

   A mudança de carregamento da célula também alcança API e ficha, além de `_no_app`, `medir` e `do_dia`; portanto seu alcance ultrapassa o comando. (app/api/views.py:2552, app/colaboradores/services/ficha.py:166, app/chamados/juizes.py:1313, app/chamados/services/pergunta_no_app.py:139, app/chamados/services/pergunta_no_app.py:162)

   A trilha continua sendo consultada por pergunta/dia negativo, podendo exigir três `exists()` e buscas de células/chamados; o comentário do comando sobre “uma query só por dia” não descreve todos esses caminhos. (app/chamados/management/commands/vigiar_fio_morto.py:168–170, app/chamados/juizes.py:1067–1078)

4. **Selos: mordida por inspeção.**

   Nesta tabela, “144c7fac” significa voltar o caminho de toque ao pai fornecido mantendo os testes atuais; execução dos mutantes: [nao verificado].

   | Teste | Com caminho de toque de 144c7fac | Com comando sem lote |
   |---|---|---|
   | Desempenho | Falharia: exige `lg5-lg1=4`, incompatível com coleta plural única. | Passaria: abre lote diretamente. (app/chamados/tests/test_snc2_toque_em_lote.py:85–109) |
   | Paridade estática | Passaria: atos e vínculos precedem as avaliações. | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:111–133) |
   | Cobrar durante lote | Falharia ao consultar P2 depois do toque. | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:135–147) |
   | Chamado ganha célula | Falharia pelo conjunto anterior. | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:149–162) |
   | Comando dentro do lote | Passaria: o contexto já existia no pai fornecido. | Falharia pela presença registrada no `censo`. (app/chamados/tests/test_snc2_toque_em_lote.py:164–177, app/chamados/management/commands/vigiar_fio_morto.py:171–172) |
   | Guarda por instância | Passaria: não abre lote nem requisição. | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:179–187) |
   | Requisição depois do lote | Passaria: instala requisição nova depois de descartar o lote anterior. | Passaria. (app/chamados/tests/test_snc2_toque_em_lote.py:189–207) |

   O selo do comando morde a remoção integral do contexto, mas observa somente `censo`: os outros consumidores poderiam sair do contexto sem alterar sua asserção. Os testes dinâmicos modificam toque ou FK; não exercitam alteração de tranca, reabertura ou corte durante o lote. (app/chamados/tests/test_snc2_toque_em_lote.py:135–177, app/chamados/management/commands/vigiar_fio_morto.py:171–176)

5. **Triagem, linha por linha.**

   | Linha | O descarte ou “curado” se sustenta? |
   |---|---|
   | **2 — NÃO/P1/P2** | **Sim, para a causa apontada:** só SIM do toque é guardado; o RED introduz o toque depois do aquecimento. (app/chamados/juizes.py:1037–1044, app/chamados/tests/test_snc2_toque_em_lote.py:135–147) |
   | **2b — chamado ganha célula** | **Sim, para esse caso:** após NÃO, o vínculo é relido. O RED reproduz o estado por `update`, sem chamar a porta. (app/chamados/juizes.py:1070–1078, app/chamados/tests/test_snc2_toque_em_lote.py:149–162) |
   | **3 — congelamento do insumo** | **Parcial:** curado o conjunto negativo de toques; permanecem empresa e tranca congeladas para toda a rodada. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974) |
   | **4a — teste sem lote/request** | **Parcial:** a guarda antiga continua por instância; a nova instala requisição, mas somente depois do lote. Ambas podem passar com o caminho do pai. (app/chamados/tests/test_snc2_toque_em_lote.py:179–207) |
   | **4b — `nullcontext`** | **Sim:** importação direta exige a existência do contexto. (app/chamados/tests/test_snc2_toque_em_lote.py:28–30) |
   | **4c — nenhum teste chama comando** | **Sim, para remover todo o contexto:** há `call_command` e observação do `censo`; cobertura dos demais consumidores continua limitada. (app/chamados/tests/test_snc2_toque_em_lote.py:164–177) |
   | **5 — cadastro/vínculos atuais, descartado** | **Não como descarte integral:** o comportamento histórico é herdado, mas o lote amplia a fotografia de empresa/tranca entre colaboradores. Retirar o NÃO do toque não elimina esse prolongamento. (app/chamados/juizes.py:938–947, app/chamados/juizes.py:967–974) |
   | **6a — `contains` × regex** | **Sim:** o caminho atual tem um reconhecedor direcionado do Cobrar e um chamador do helper. (app/chamados/juizes.py:1041, app/chamados/juizes.py:1067–1069) |
   | **6b — `get_or_create`, descartado** | **Preexistência sustentada; só leitura incondicional, não:** a dependência ainda pode criar usuário. “Fora do diff” não retira o escritor do alcance da revisão. (app/core/utils.py:18, app/chamados/juizes.py:1062–1064, CLAUDE.md:371–372) |
   | **6c — relógio interno, descartado** | **Preexistência sustentada; classe permanece:** C6 pode ler `date.today()` apesar de o comando capturar `agora/hoje`. (app/chamados/juizes.py:562, app/chamados/juizes.py:1191, app/chamados/management/commands/vigiar_fio_morto.py:163–164) |
   | **6d — suíte/pouso, descartado** | **Permanece sem comprovação:** a regra exige suíte inteira verde e depois leitura final; o descarte não comprova essas etapas. (CLAUDE.md:369–373) O anexo relata 283 testes, resultados de sombra e MD5 iguais; confirmação independente e suficiência como suíte inteira: [nao verificado]. | [nao verificado]

   A regra local manda levar os descartes à leitura final, que decide fila ou bloqueio; também inclui expressamente escritores e leitores fora do diff no escopo. (CLAUDE.md:368–373)

6. **Classes que já reprovaram obra.**

   - **Segundo juiz:** C6 continua registrado uma vez; o helper segue subordinado a ele. A reprovação demonstrada é a reutilização de insumo desatualizado. (app/core/juizes.py:266, app/chamados/juizes.py:839, app/chamados/juizes.py:1041)
   - **Segundo escritor fora da porta:** a trilha examinada continua chegando a `evento`; subsiste criação condicional de usuário na dependência preexistente. (app/core/observ.py:134–138, app/core/utils.py:18)
   - **Relógio fora da borda:** permanece a leitura interna herdada em `_dentro_da_janela_viva`. (app/chamados/juizes.py:562, app/chamados/juizes.py:1191)
   - **Derivação paralela:** saiu a reconstrução plural; entretanto o memo pode devolver uma tranca diferente da resposta atual de `periodo_trancado`. (app/chamados/juizes.py:942–947, app/chamados/juizes.py:1047–1078, app/ponto/services/fechamento.py:1454)
   - **Selo/RED sem mordida:** há REDs que mordem a causa anterior, mas paridade estática e requisição posterior não distinguem a cura do pai; falta cobertura das alterações de tranca/corte demonstradas acima. (app/chamados/tests/test_snc2_toque_em_lote.py:111–162, app/chamados/tests/test_snc2_toque_em_lote.py:189–207)
   - **Ordem de trava:** o contexto instala/restaura um dicionário, sem estabelecer transação ou trava contra escritores externos; as dependências declaradas do cron continuam cartório e reconciliação. Cumprimento operacional: [nao verificado]. (app/chamados/juizes.py:987–997, app/config/crons.py:1680–1682)
   - **Pouso antes do pré-requisito:** execução de suíte inteira, leitura final e ordem efetiva de pouso: [nao verificado]. A exigência está em CLAUDE.md:369–373. [nao verificado]
   - **Sítio sem chamador:** o helper novo tem chamador no juiz; o contexto tem chamador no comando; `perguntas_do_app` tem leitores reais na API e na ficha. Esses sítios sobreviventes têm uso demonstrado

[truncado em 20000 de 20957 caracteres]

## 05/11 16:02 · sessao bd5668197954 · - · - · origem=Ronald (tmux)

Três achados fortes, por leitura estática; ocorrência em produção [nao verificado].

1. **PDF omite “sem apuracao ainda” dos totais.** Sem `FechamentoMensal` nem `DiaPago`, a autoridade define `fonte_dos_totais='sem apuracao ainda'`, preservando os valores do motor (`app/relatorios/cartao_pela_celula.py:340`, `app/relatorios/cartao_pela_celula.py:345`, `app/relatorios/cartao_pela_celula.py:203`). O PDF chama essa autoridade (`app/relatorios/pdf_espelho.py:628`), mas seus badges mostram “Trabalhadas” e demais rubricas sem ler a fonte (`app/relatorios/pdf_espelho.py:766`, `app/relatorios/pdf_espelho.py:771`, `app/relatorios/pdf_espelho.py:791`).
   - **Contraexemplo construído:** competência exata, escala válida, batidas 08:00/12:00/13:00/17:00, nenhum fechamento nem dia pago. O total do motor aparece sem informar que não há apuração persistida. A ausência de registros faz as autoridades retornarem `None` (`app/relatorios/cartao_pela_celula.py:71`, `app/ponto/services/dia_pago.py:557`).
   - **Certeza alta; dono ESTRUTURA.**

2. **“Horario” repete o template do fim da janela em todos os dias.** O coletor escolhe a escala do último dia (`app/relatorios/pdf_espelho.py:307`) e devolve seu `tipo_escala` único (`app/relatorios/pdf_espelho.py:646`). Em cada linha com turnos, “Horario” imprime `hora_inicio/hora_fim` desse mesmo template (`app/relatorios/pdf_espelho.py:865`, `app/relatorios/pdf_espelho.py:903`, `app/relatorios/pdf_espelho.py:907`). Os marcos efetivos do dia podem vir do DNA congelado da célula (`app/escala/models.py:1124`, `app/escala/models.py:1131`).
   - **Contraexemplo construído:** competência com vínculo 08:00–17:00 até dia 10 e vínculo 19:00–07:00 depois; dias anteriores com turnos registrados aparecem com “Horario 19:00 07:00”. Também dispara com célula antiga 08:00–17:00 e template posteriormente alterado para 19:00–07:00, pelo mesmo caminho citado.
   - **Certeza alta; dono ESTRUTURA.**

3. **PDF de retidos rotula tipos de motivo como “Dias”.** O cabeçalho diz “Dias”, mas calcula `len(motivos_espelho)` (`app/folha/retidos_pdf.py:88`, `app/folha/retidos_pdf.py:90`). Essa lista recebe os motivos da autoridade de retenção (`app/folha/export.py:942`, `app/folha/export.py:946`), que os deduplica em `set`, sem contar datas (`app/folha/export.py:1197`, `app/folha/export.py:1201`, `app/folha/export.py:1240`). A rota chama esse fluxo (`app/folha/views.py:858`, `app/folha/views.py:859`).
   - **Contraexemplo construído:** colaborador com cadastro apto, fechamento existente e cinco dias com veredito `furo`, sem protestos que isentem retenção e sem isenção por pausa. Os cinco dias produzem apenas `['espelho_furo']`; o PDF mostra **Dias = 1** (`app/folha/export.py:1235`, `app/folha/export.py:1236`).
   - **Certeza alta; dono ESTRUTURA.**

Nenhum destes achados exige especulação sobre intenção do usuário ou regra de pagamento; são divergências entre condição, dado e texto nos caminhos citados. PDFs efetivamente gerados, frequência, colaboradores atingidos e renderização visual [nao verificado].

## 05/11 16:02 · sessao 3cbc5d59a49a · CODEX-SUFOCA-07-corrida-trava · leitura · origem=Code

**Os pontos mais fortes são a leitura antes da trava e a gravação de instâncias antigas:** aparecem no núcleo, no julgamento da célula, nas disputas/perguntas e no fechamento (`app/ponto/nucleo.py:45-60`; `app/ponto/portas/celula.py:745-783`; `app/chamados/services/disputa_emissao.py:466-528`; `app/ponto/services/fechamento.py:889`, `:1121-1128`).

Os contra-exemplos abaixo são construídos a partir do código; **não representam ocorrências comprovadas em produção** [não verificado]. “C” representa um colaborador fictício, com cadastro válido; “D”, um dia já encerrado [não verificado em execução]. O vínculo da cópia ao HASH informado não pôde ser atestado por Git [não verificado].

1. **Deduplicação de batida permite duas inserções próximas. Certeza: alta no mecanismo estático.**

   A porta procura mesma batida na janela de 60 segundos, consulta a chave exata e verifica colisão antes do `create`, sem uma trava envolvendo essas operações. A unicidade declarada cobre apenas colaborador + timestamp (`app/ponto/registro_batida.py:107-122`, `:139-159`; `app/ponto/models.py:106`).

   **Contra-exemplo:** C sem batida próxima; A recebe E às 08:00:00 e B recebe E às 08:00:01. Ambas concluem as consultas antes de qualquer inserção; ambas criam. Sequencialmente, a segunda reutilizaria a primeira. Com timestamp idêntico, a constraint impede duplicata, mas a porta não trata a corrida entre consulta e inserção para devolver a linha vencedora (`app/ponto/registro_batida.py:113-122`, `:157-161`).

   **Especulação:** ocorrência, resposta HTTP efetiva e impacto em produção [não verificado].

2. **Reconstrução do núcleo pode duplicar turnos ou repor geometria antiga. Certeza: alta.**

   `recompute_turnos` calcula todas as linhas **antes** do `atomic`; dentro dele apenas apaga e insere. `TurnoMaterializado` declara índices, sem unicidade das linhas reconstruídas (`app/ponto/nucleo.py:45-60`; `app/ponto/models.py:1213-1220`).

   **Contra-exemplo de duplicata:** existe E às 08:00, mas nenhum turno materializado. A e B calculam o mesmo turno; ambas apagam o conjunto vazio antes dos inserts; ambas inserem a mesma geometria. **Contra-exemplo de perda:** A calcula turno aberto; chega S às 18:00 e B reconstrói fechado; A executa depois seu delete/rebuild antigo e repõe aberto. O recebimento da batida aciona essa reconstrução por signal (`app/ponto/signals.py:19-36`).

   **Especulação:** duplicatas existentes e duração da geometria incorreta [não verificado].

3. **O cartório pode sobrescrever julgamento novo e decisão administrativa. Certeza: alta.**

   O lote carrega células sem trava, materializa a lista de batidas e posteriormente salva ata, impressão e veredito. O `atomic` da persistência cobre o save e o evento, sem reler os insumos nem comparar uma versão (`app/ponto/management/commands/processar_cartorio.py:59-65`; `app/ponto/services/cartorio.py:1123-1126`, `:1802-1823`; `app/ponto/portas/celula.py:642-650`).

   **Contra-exemplo com batida:** A calcula D com E08 e S18 ausente; B recebe S18, julga com o par completo e grava; A salva depois sua ata/impressão antigas. O signal de batida chama o julgamento, mas não impede a escrita posterior de A (`app/ponto/signals.py:85-93`, `:131-135`; `app/ponto/portas/celula.py:808-815`).

   **Contra-exemplo administrativo:** A carregou `veredito_via='cartorio'`; B decide `faltou` e grava via admin; A continua com a instância antiga. A guarda “admin vence” consulta apenas `cel.veredito_via` em memória e A pode repor a via cartório (`app/ponto/services/veredito_celula.py:330`; `app/ponto/portas/celula.py:745-753`, `:777-783`).

   **Especulação:** ocorrência e correção posterior por novas passadas [não verificado].

4. **A primeira abertura pode produzir duas disputas, apesar do `select_for_update`. Certeza: alta.**

   A trava incide sobre uma disputa aberta **já existente**. Quando a consulta retorna vazia, o código segue para abrir/reutilizar o chamado e criar a disputa, sem repetir a consulta inicial. A disputa não declara unicidade (`app/chamados/services/disputa_emissao.py:466-469`, `:504-528`; `app/chamados/models.py:1731-1737`).

   **Contra-exemplo:** C com saída ausente, nenhuma disputa aberta. A e B leem ausência de disputa. A obtém a trava do colaborador dentro de `ChamadoColaborador.abrir`, cria chamado e disputa; B espera, reutiliza o chamado e cria outra disputa. A trava do chamado começa depois da consulta que decidiu criar a disputa (`app/chamados/models.py:863-867`; `app/chamados/services/disputa_emissao.py:520-528`).

   **Especulação:** disputas reais afetadas [não verificado].

5. **Perguntas do mesmo fato podem nascer duplicadas. Certeza: alta.**

   `PerguntaDisputa.abrir` procura a identidade do fato e depois cria, sem trava própria; o modelo não declara unicidade. A geração acontece depois do retorno do helper transacional da disputa (`app/chamados/models.py:2417-2420`, `:2465-2466`, `:2800-2803`; `app/chamados/services/disputa_emissao.py:598-618`).

   **Contra-exemplo:** já existe disputa aberta para C; falta a pergunta do motivo M em D. A retorna do helper e pausa na geração; B também retorna. Ambas consultam a identidade ainda inexistente e ambas inserem. Esse caso independe da duplicação de disputa do item anterior (`app/chamados/services/disputa_emissao.py:618`; `app/chamados/models.py:2420`, `:2465`).

   **Especulação:** quantidade de perguntas duplicadas em produção [não verificado].

6. **Correção de resposta pode conservar validação da resposta anterior. Certeza: alta para a janela de gravação.**

   O PWA carrega a pergunta e verifica suas guardas antes de chamar o core. O core só limpa a validação se ela estiver preenchida naquela instância; o validador também lê antes de iniciar seu `atomic` (`app/chamados/views.py:1239-1257`; `app/chamados/services/disputa_emissao.py:1282-1301`; `app/chamados/services/validacao.py:359-368`, `:395-396`).

   **Contra-exemplo:** pergunta respondida 18:00, ainda não validada. A carrega a pergunta para corrigir para 17:00 e pausa após as guardas. B valida/materializa 18:00 e termina. A continua com `validada_em=None` em memória e salva apenas resposta/respondida, preservando os campos de validação que B gravou. Pode ficar texto 17:00 com validação e batida de 18:00 (`app/chamados/services/disputa_emissao.py:1284-1301`; `app/chamados/services/validacao.py:131-133`; `app/chamados/services/materializacao.py:894-900`).

   **Especulação:** ocorrência e reparo integral pelos receivers posteriores [não verificado].

7. **`analisar()` pode ressuscitar um chamado cancelado. Certeza: alta.**

   `analisar()` decide pelo estado da instância, sem recarregar/travar. A gravação para estado vivo limpa os campos de encerramento e salva depois (`app/chamados/models.py:763-769`, `:242-243`, `:274-279`).

   **Contra-exemplo:** a view carrega um `ausencia_colab` aberto; B cancela e termina; A chama `analisar()` sobre a instância antiga e grava `em_analise`. O cancelamento usa recarga protegida, mas ela não protege contra esse escritor posterior. O catálogo declara cancelado como terminal absoluto (`app/chamados/views.py:1507-1513`; `app/chamados/models.py:744-751`; `app/chamados/catalogo/motor.py:104`, `:112`).

   **Especulação:** ocorrência desse fluxo em produção [não verificado].

8. **Atualizações de chaves diferentes do contexto JSON podem se apagar. Certeza: alta.**

   As portas copiam o JSON da instância, mesclam em memória e salvam o campo inteiro. O `atomic` começa depois da leitura; a própria documentação declara a ausência de proteção contra corrida (`app/chamados/services/contexto.py:25-30`, `:70-77`, `:96-102`).

   **Contra-exemplo:** A e B carregam contexto K. A acrescenta `escalado_n2_1h`; B renova `marcos_faltantes` usando K. Se B salva depois, elimina a chave acrescentada por A. Há escritores dessas chaves nos comandos de escalonamento e nomeação de marcos (`app/ponto/management/commands/escalonar_chamados_supervisao.py:138`; `app/chamados/management/commands/nomear_marco_por_lampada.py:109`).

   **Especulação:** notificações repetidas decorrentes da perda do carimbo [não verificado].

9. **Recálculo pode desfazer uma aprovação de fechamento. Certeza: alta.**

   O recálculo obtém o fechamento sem trava e, se a instância estava aberta, faz save inteiro. A proteção do modelo só recusa quando tanto o status gravado quanto o da instância são `aprovado` (`app/ponto/services/fechamento.py:889-892`, `:1121-1128`; `app/ponto/models.py:497-503`).

   **Contra-exemplo:** fechamento de C aberto. A carrega; B aprova pelo lote; A salva depois sua instância aberta, com os campos antigos de aprovação. A condição do modelo não barra esse save porque `self.status` continua aberto. A unicidade mensal impede duas linhas, mas não essa perda (`app/ponto/services/fechamento.py:1313-1316`; `app/ponto/models.py:485`, `:498`).

   **Especulação:** aprovações efetivamente perdidas [não verificado].

10. **Recálculo antigo pode apagar uma atualização ou invalidação e parecer atual. Certeza: alta.**

    As batidas e justificativas aprovadas são carregadas antes da persistência; ao terminar, o recálculo grava `derivado_em=agora` sem comparar a versão dos insumos (`app/ponto/services/fechamento.py:624`, `:651-656`, `:1127-1128`, `:1173`).

    **Contra-exemplo com batida:** A coleta E08 sem saída; B recebe S18 e seu recálculo por evento termina; A salva depois o resultado antigo com carimbo posterior (`app/ponto/services/fechamento.py:1791-1794`, `:1808`).

    **Contra-exemplo com justificativa:** A coleta as justificativas aprovadas; B aprova uma justificativa sem saída informada e invalida o fechamento; A salva sem considerar essa aprovação e preenche novamente `derivado_em`. O vigia que procura apenas `NULL` deixa de selecionar essa linha (`app/ponto/services/justificativa.py:190-201`; `app/ponto/services/fechamento.py:1636-1640`, `:1675-1680`).

    **Especulação:** valores financeiros afetados e duração do estado antigo [não verificado].

11. **Anotações de batida perdem operações e repetem retratação. Certeza: alta.**

    O flip calcula o oposto a partir da instância carregada; `anotar` compara essa instância e salva sem releitura. A retratação também testa o carimbo da instância e gera um novo timestamp (`app/ponto/views.py:3255-3256`; `app/ponto/services/flip_batida.py:25-33`; `app/ponto/registro_batida.py:244-263`, `:271-276`).

    **Contra-exemplos:** duas execuções carregam E e ambas invertem para S: duas inversões aceitas deixam S, enquanto sequencialmente deixariam E. Duas retratações carregam `retratada_em=None`, gravam t1/t2 e deixam duas trilhas; o segundo carimbo substitui o primeiro (`app/ponto/services/flip_batida.py:25-33`; `app/ponto/registro_batida.py:254-276`).

    **Especulação:** ocorrência e autores/motivos reais envolvidos [não verificado].

As travas existentes limitam o alcance desses achados: `ChamadoColaborador.abrir` segura o colaborador **antes** do dedup, portanto seu `get_or_create` sem unique não é, isoladamente, prova de duplicação entre chamadas desse gate (`app/chamados/models.py:863-867`, `:932`). O runner geral marca execução por PID e executa o comando; os encadeados têm `flock`, o vigia de hora tem advisory lock e o consumidor de eventos também tem exclusão específica (`bin/cron_run.sh:9-11`; `bin/encadeado.sh:78-82`; `app/ponto/services/vigia_de_hora.py:97-101`; `app/chamados/management/commands/consumir_eventos_celula.py:18-23`).

Não consegui verificar crontab instalado, sobreposição real, múltiplos hosts, isolamento efetivo, constraints aplicadas no banco, reprodução concorrente, quantidade de linhas afetadas ou dano financeiro [não verificado]. Contagens e relatos históricos em comentários não foram tratados como dado atual de produção [não verificado]. O dono dos mecanismos descritos é **ESTRUTURA**: os contra-exemplos mantêm o mesmo cadastro e os mesmos fatos de entrada; a divergência nasce da ordem das execuções, conforme o critério L-099 (`CLAUDE.md:176`). [nao verificado]

| achado | arquivo:linha | contra-exemplo | certeza | dono L-099 |
|---|---|---|---|---|
| 1. Dedup de batida | `app/ponto/registro_batida.py:113`, `:157`; `app/ponto/models.py:106` | Duas E às 08:00:00/08:00:01 passam antes dos inserts | Alta | ESTRUTURA |
| 2. Reconstrução do núcleo | `app/ponto/nucleo.py:45-60`; `app/ponto/models.py:1213-1220` | Dois rebuilds inserem a mesma geometria; rebuild antigo repõe aberto | Alta | ESTRUTURA |
| 3. Julgamento antigo vence | `app/ponto/services/cartorio.py:1123`, `:1822`; `app/ponto/portas/celula.py:745`, `:783` | S18 ou decisão administrativa chega entre leitura e save | Alta | ESTRUTURA |
| 4. Disputa duplicada | `app/chamados/services/disputa_emissao.py:467`, `:523` | Duas consultas vazias; duas disputas para o mesmo chamado | Alta | ESTRUTURA |
| 5. Pergunta duplicada | `app/chamados/models.py:2420`, `:2465`, `:2800` | Mesmo C/motivo/D ausente nas duas consultas | Alta | ESTRUTURA |
| 6. Resposta × validação | `app/chamados/services/disputa_emissao.py:1286`, `:1301`; `app/chamados/services/validacao.py:396` | Correção 17:00 salva após validação/materialização de 18:00 | Alta | ESTRUTURA |
| 7. Cancelamento perdido | `app/chamados/models.py:763-769`, `:279`; `app/chamados/views.py:1513` | Instância aberta chama analisar após cancelamento | Alta | ESTRUTURA |
| 8. Contexto perdido | `app/chamados/services/contexto.py:70-77`, `:96-102` | Renovar marcos apaga carimbo de escalonamento concorrente | Alta | ESTRUTURA |
| 9. Aprovação perdida | `app/ponto/services/fechamento.py:1128`, `:1316`; `app/ponto/models.py:498` | Recálculo carregado aberto salva depois da aprovação | Alta | ESTRUTURA |
| 10. Invalidação perdida | `app/ponto/services/fechamento.py:651`, `:1127`, `:1640`, `:1680` | Aprovação invalida; recálculo antigo repõe carimbo preenchido | Alta | ESTRUTURA |
| 11. Anotação concorrente | `app/ponto/services/flip_batida.py:25-33`; `app/ponto/registro_batida.py:254`, `:274` | Duas inversões deixam S; duas retratações substituem t1 por t2 | Alta | ESTRUTURA |

## 05/11 16:01 · sessao 7b19689bbfab · - · - · origem=Ronald (tmux)

Complemento dos campos persistidos:

| Campo de `Ausencia` | Escritores/produtores |
|---|---|
| `pk` | `BigAutoField`; criação monta `Ausencia(**kwargs)` e executa `save`. (`app/ponto/migrations/0008_ausencia.py:20`; `app/ponto/services/ausencia.py:385`, `:418`.) |
| `colaborador_id` | Recebido por `criar_ausencia`, incluído nos kwargs; FK do modelo. Nenhuma atribuição posterior identificada na porta. (`app/ponto/services/ausencia.py:130`, `:342`; `app/ponto/models.py:743`.) |
| `tipo`, `data` | Criação inclui os argumentos; edição atribui ambos e salva. (`app/ponto/services/ausencia.py:342`, `:497`, `:498`, `:508`.) |
| `data_fim` | `derivar_data_fim` produz a data/`None` na criação e edição; modelo também completa o range quando `dias_corridos > 1`. (`app/ponto/services/ausencia.py:66`, `:75`, `:76`, `:77`, `:197`, `:343`, `:477`, `:499`; `app/ponto/models.py:902`, `:907`.) |
| `status` | Criação usa argumento explícito ou resultado de `estado_inicial`; default do modelo; `mudar_estado` atribui e salva; `rejulgar_status_pelo_tipo` chama essa porta; rejeição de férias usa `update` em lote. (`app/ponto/services/ausencia.py:333`, `:344`, `:550`, `:828`, `:839`, `:1285`, `:1287`; `app/ponto/models.py:799`, `:800`.) |
| `empresa_id` | Não pertence à ausência: a empresa é a FK do seu colaborador; a transferência pode alterar `Colaborador.empresa`. (`app/ponto/models.py:743`; `app/colaboradores/models.py:321`; `app/colaboradores/services/colaborador.py:33`, `:55`, `:60`, `:61`, `:73`, `:76`.) |

A porta registrada para `Ausencia` é `ponto/services/ausencia.py`. (`app/core/portas.py:480`, `:485`.)

## 05/11 16:01 · sessao 4f87a610d763 · - · - · origem=Ronald (tmux)

Achei um candidato limitado aos selos indicados, com cobertura complementar em outro teste: [nao verificado]

**L113 A0 aceita uma prova calculada errada nos dois lados.** O teste compara o hash gravado com `hash_do_txt` novamente, inclusive no vazio; não crava digest conhecido. Suas diferenças de conteúdo usam letras (`bbb → ccc`, `aaa → bbb`). Referências: `app/folha/tests/test_l113_a0_forma_do_txt.py:106-111`, `:115-126`, `:137-145`, `:158-165`.

Mutante concreto em `app/folha/export.py:542`:

```python
return hashlib.sha256(
    ''.join('0' if c.isdigit() else c for c in conteudo).encode()
).hexdigest()
```

Ele preserva as comparações desse arquivo: escritor e conferidor compartilham a função (`app/folha/models.py:176-185`; `app/folha/export.py:647-656`), e diferenças alfabéticas continuam distintas (`app/folha/tests/test_l113_a0_forma_do_txt.py:115-126`). Certeza **alta por análise estática**; execução do teste mutado **[não verificado]**.

Contraexemplo: uma única linha TXT válida da matrícula 4321, competência 09/2026, rubrica 0150, passa de 8,00 para 6,00 horas, mantendo os demais campos. A montagem muda somente dígitos (`app/folha/export.py:34-45`), o mutante devolve o mesmo hash, e `txt_vigente_x_gravado` retorna `divergente=False` (`app/folha/export.py:655-660`). Dono sugerido **ESTRUTURA**, pela falha da prova, e não pelo cadastro ou batida — classificação interpretativa, baseada em `CLAUDE.md:176`.

**Limite essencial:** esse mutante não passa pela suíte inteira: `app/folha/tests/test_txt_invalidado_com_trilha.py:58-61` usa conteúdo numérico e compara com `hashlib.sha256` independente. Portanto o achado é “este selo isolado não morde”, não “ausência global de proteção”.

`test_haiku_export.py` também sobreviveria a esse mutante por análise estática: v1 tem duas linhas e v2 uma, preservando hashes distintos mesmo com dígitos normalizados; o parser de diferenças continua recebendo conteúdo intacto e os deltas são cravados. Referências: `app/folha/tests/test_haiku_export.py:67-77`, `:87-89`, `:101-110`. Execução **[não verificado]**.

Não encontrei mock da própria função nesses dois arquivos; `test_haiku_export.py:101-110` chama o comparador real e verifica deltas concretos. Os skips localizados fora desse núcleo pertencem ao parser/entrega de holerites, não à defesa dos valores solicitados, e não recomendo incluí-los como achados de dinheiro (`app/holerite/tests/test_selo_c1_parser_vivo.py:83-93`; `app/holerite/tests/test_selo_job_lote.py:57-61`).

_1770 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
