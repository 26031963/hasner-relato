# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 16:22** · publicado em **31/10/2026 16:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 16:22 · sessao 2ca4f78aa1d8 · F1C4-3-EMISSORES · censo · origem=Code

**Há seis classificações sustentáveis e dois achados cuja classe fica [nao verificado].** O material não permite concluir “zero falhas do Codex”: falta estabelecer quando os defeitos específicos dos itens 3 e 4 entraram nos hashes examinados. (`opus_v11.txt:4–5`; `v8.q:1–2`.)

Nos anexos colados, conto linhas desde a primeira linha após cada identificador, incluindo linhas vazias. Não atribuo descarte a `triagens.txt`, cujo bloco veio vazio; os registros locais de fila não individualizam os defeitos finais dos itens 3 e 4. (`app/docs/RELATO.md:29`; `app/docs/BACKLOG.md:822`; descarte específico: **[nao verificado]**.)

1. **Corte/backlog divergentes da migração — (c), alteração posterior à última chamada guardada.** (`opus_v11.txt:2`; `v8.q:1`.)

   **Pergunta:** “Ha sitio de producao que ainda responde […] por conta propria […] sem perguntar a chamados/juizes.py::dia_sem_trabalho?” (`v4.q:15–16`.)

   **Resposta:** “a migração está declarada no corte, mas não realizada nesse sítio.” (`v4.r:42`.)

   Codex viu o antecedente — supra com predicado próprio —, mas o achado final é outro: **migrar em `b3bef403` e deixar a documentação anterior**. Não cabe transformar o descarte do antecedente em descarte desse defeito novo. (`v1.r:88`; `opus_v11.txt:2`.)

   **Entrada:** tema supra já na **v1**; corte discutido na **v4**; contradição causada pela migração documentada na **v11**, após a chamada v8. (`v1.r:88`; `v4.r:42`; `opus_v11.txt:1–2`; `v8.q:1`.)

2. **Número atribuído ao universo errado na mensagem — (c), dado de produção/mensagem final.** (`opus_v11.txt:3`; `CLAUDE.md:557`.)

   **Pergunta:** “antes, sombra com novos CHAMADOS por emissor; ate 50 pousa”. (`v2.q:2`.)

   **Resposta:** “A sombra com novos CHAMADOS por emissor, e o atendimento ao teto de 50, são **[não verificado]** no material fornecido.” (`v2.r:1`.)

   A fonte local associa **0/127.784** a ata explícita **`indefinido`**, confirmando a diferença de universo apontada pelo opus. O valor **158/143.452** está sustentado apenas pelo dado raspado fornecido. (`app/docs/BACKLOG.md:806`; `opus_v11.txt:3`.)

   **Entrada:** o número anterior já constava da obra precedente; a frase acusada está documentada na **v11**. Primeira volta em que essa frase foi escrita: **[nao verificado]**. (`app/docs/BACKLOG.md:806`; `opus_v11.txt:1–3`.)

3. **Catraca aceita o nome, mas deixa passar `ata=cel.ata` — classe [nao verificado].**

   **Pergunta:** “Os testes novos mordem a reversao de cada um dos 4 sitios individualmente?”; checklist: “selo/RED sem mordida”. (`v1.q:6–7`.) [nao verificado]

   **Resposta:** “também não exigem estruturalmente delegação ao juiz — um clone equivalente passa nos asserts de retorno.” (`v1.r:105`.)

   Isso comprova que Codex viu **uma fragilidade de delegação nos testes antigos**; não comprova que viu a catraca específica de `com_insumo_dia_sem_trabalho` nem o mutante descrito pelo opus. (`v1.r:105`; `opus_v11.txt:4`.) [nao verificado]

   **Entrada:** tema de proteção estrutural presente na **v1**; primeira volta da catraca específica: **[nao verificado]**. Se já estava num hash examinado, pode caber **(a)**; se nasceu depois da v8, pode caber **(c)**. Não há evidência para escolher nem para atribuir **(b)**. **[nao verificado]**

4. **`_protesta` antecipa o juiz; cláusula inatingível; omissão em `_C18` — classe integral [nao verificado].**

   **Pergunta:** “A pergunta registrada em `_C18` e respondida por outra funcao […] com predicado igual ou diferente?” (`v4.q:14–16`.)

   **Resposta anterior:** “Protesto e espera pelo fim do turno usam `trabalha is not False`.” (`v1.r:87`.)

   Portanto, **o predicado do cartório foi visto**. A cópia também o contém literalmente: `return _cel.trabalha is not False and _acesos(x, _ata) is not True`. (`v1.r:87`; `app/ponto/services/cartorio.py:1204–1205`.)

   A consequência final — cláusula do juiz inatingível após a delegação — não é o mesmo achado que simplesmente listar esse predicado. Quando surgiu essa delegação, e se a omissão específica de `_C18` já estava nos hashes examinados: **[nao verificado]**. (`opus_v11.txt:5`.)

   **Entrada:** sítio/predicado na **v1**; `_C18` entra explicitamente na pergunta da **v4**; primeira volta da combinação final: **[nao verificado]**. (`v1.r:87`; `v4.q:11–16`.)

5. **Sombra só do supra; cartório não medido; efeito com tipo `None` — (c), quanto à medição final.** (`opus_v11.txt:6`; `CLAUDE.md:557`.)

   **Pergunta:** “antes, sombra com novos CHAMADOS por emissor”. (`v2.q:2`.)

   **Resposta:** a sombra e o teto permaneciam “**[não verificado]** no material fornecido”. (`v2.r:1`.)

   A semântica geral de `None` já fora percebida: “tipo ausente, `None` ou vazio não satisfaz `A`”. Isso não demonstra exame da migração final nem cobertura do caminho final do cartório. (`v1.r:7`; `opus_v11.txt:6`.)

   **Entrada:** semântica de tipo ausente/`None` na **v1**; sombra explicitamente exigida na **v2**; medição específica acusada documentada na **v11**. Sua primeira execução e a volta da mudança comportamental: **[nao verificado]**. (`v1.r:7`; `v2.q:2`; `opus_v11.txt:6`.)

6. **`v11_green.log` vazio; suíte sem hash — (c), suíte/log.** (`opus_v11.txt:7`; `CLAUDE.md:557`.) [nao verificado]

   **Pergunta relacionada:** checklist “selo/RED sem mordida”; não há pedido específico de conferência desse log final. (`v1.q:7`; conteúdo anterior de `v11_green.log`: **[nao verificado]**.)

   **Resposta:** “Execução de testes/mutantes, respeito aos tetos após a mudança e incidência por colaborador em produção: **[não verificado]**.” (`v1.r:3`.) [nao verificado]

   **Entrada:** arquivo vazio documentado na **v11**. Quando a suíte passou a não imprimir hash: **[nao verificado]**. (`opus_v11.txt:7`.) [nao verificado]

7. **`c5c43360` sem linha LEI-AKITA — (e), NÃO PERGUNTADO sobre a mensagem.** (`v3.q:1–30`; `opus_v11.txt:8`.)

   **Pergunta mais próxima:** “Aponte tambem qualquer outro defeito no delta que estas perguntas nao cobrem.” Ela não solicita auditoria da mensagem nem da linha LEI-AKITA. (`v3.q:30`.)

   **Resposta:** abre com “Reprovo a volta 3 pelo aval literal”, discutindo a fronteira de competência. Não há trecho específico apontando a ausência da linha na mensagem nos registros fornecidos. (`v3.r:1`; trecho específico: **[nao verificado]**.)

   **Entrada:** fábrica presente desde a **v1**; commit acusado já identificado na **v3**. Portanto, não cabe justificar este item como “commit ainda não escrito”. A ausência efetiva da linha tem como evidência fornecida o relato do opus. (`v1.r:1`; `v3.q:1`; `opus_v11.txt:8`.) [nao verificado]

8. **Mensagem afirma ausência de arquivos em PROTEGE — (e), NÃO PERGUNTADO, com divergência material na evidência.** (`v2.q:1–20`; `v3.q:1–30`; `v4.q:1–21`; `v8.q:1–12`; `opus_v11.txt:9`.)

   **Pergunta mais próxima:** “Leis citadas pelos sitios (grep LEIS/CORTES) e app/docs/JUIZES.md vao anexos.” (`v2.q:20`.)

   **Resposta mais próxima:** “A L-061 exige leitura da célula/ata; ela não demonstra equivalência dos predicados sobreviventes.” Isso discute a lei, sem conferir a afirmação da mensagem contra PROTEGE. (`v2.r:139`.)

   **Ressalva:** nesta cópia, **PROTEGE está vazio nas três leis citadas pelo opus**. Os arquivos aparecem em outra coluna; a documentação distingue expressamente DONO de PROTEGE. Assim, a cópia não confirma essa parte do achado final. (`app/docs/LEIS.md:43–48`; `app/docs/LEIS.md:57`; `app/docs/LEIS.md:81`; `app/docs/LEIS.md:91`; `app/docs/LEIS.md:95`.)

   **Entrada:** primeira volta da afirmação na mensagem: **[nao verificado]**. O resumo não identifica seu commit; não há base para declará-la posterior à v8. (`opus_v11.txt:9`.) [nao verificado]

**Contagem das classificações sustentadas acima** — zeros significam ausência de atribuição comprovada neste censo, não prova de inexistência de falhas. (`CLAUDE.md:554–558`; pendências dos itens 3 e 4: **[nao verificado]**.)

| Classe | Quantidade | Achados |
|---|---:|---|
| (a) Não viu com tema presente | 0 comprovados | — |
| (b) Viu e triagem descartou | 0 comprovados | — |
| (c) Fora do alcance | 4 | 1, 2, 5, 6 |
| (e) Não perguntado | 2 | 7, 8 |
| (d) Nenhum viu | 0 comprovados | — |
| **Classe [nao verificado]** | **2** | **3, 4** |
| **Total** | **8** | |

## 31/10 16:20 · sessao 0d61e8d5537d · PADRAO-DE-CIMA · leitura · origem=Code

O ponto decisivo é separar **o B3 desta cópia** do mecanismo futuro: hoje ele dispara com três reprovações, rearma por CLI e não consome linha liberadora; a pergunta no topo apenas cala a cobrança do Stop. O contrato documentado já descreve hash e consumo, mas isso ainda não está nesses caminhos executáveis. A implementação triestado da volta 4 é [nao verificado]. ([bin/disjuntor.py:68](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/bin/disjuntor.py:68), `bin/disjuntor.py:585`, `bin/disjuntor.py:947`; `bin/hook_stop_fila1.py:1049`; `app/docs/RELATO.md:11–13`.)

**1) As formas existentes não constituem hoje um registro uniforme de eventos.** Os exemplos abaixo têm diferenças relevantes para contar ocorrências e delimitar 24 horas. (`app/docs/RELATO.md:4`, `app/docs/RELATO.md:12`, `app/docs/RELATO.md:141`; `app/docs/RELATO-ARQUIVO.md:983`.)

| Forma observada | Exemplo e localização | O que permite extrair |
|---|---|---|
| Modelo literal de linha | `DESENHO <obra>: lei=... pergunta=... <arquivo>:<linha>` — `app/docs/RELATO.md:12` | É **exemplo dentro do contrato**, com placeholders; contá-lo produziria evento fictício. |
| Reprovação/desenho no topo | `REPROVA-E-DESENHO V3-RETOMA (12:4x, ...)` — `app/docs/RELATO.md:4` | Obra explícita; hora aproximada, sem data própria. Cita motor, alimentação, regras e núcleo, sem entidade única declarada. |
| Pergunta B3 respondida | `[RESPONDIDA 23:5x...] PERGUNTA DE DESENHO O261-E-F5 (...), 30/10 23:4x` — [RELATO.md:141](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/app/docs/RELATO.md:141) | Obra e data explícitas; horas aproximadas da resposta e da pergunta são diferentes. Há referências a chamado, célula, folha e funções distintas. |
| Pergunta histórica com texto intermediário | `PERGUNTA DE DESENHO (nao devolve turno) -- **GEO-SILENCIO-DE-PING...` — `app/docs/RELATO-ARQUIVO.md:983` | Não segue imediatamente `DESENHO <obra>`; não tem hora nem arquivo:linha próprios. Referências aparecem nos itens seguintes, em `:985` e `:987`. |
| Pergunta respondida e riscada | `~~(pergunta de DESENHO -- ...` — `app/docs/RELATO.md:947` | A data disponível é da resposta. Hora de abertura e identidade normalizada do evento: [nao verificado]. |
| Classificação de achado/custo | `(DESENHO)`, `= DESENHO`, `DESENHO, NAO DEFEITO` — `app/docs/RELATO.md:1302`, `:1333`, `:4058`; `app/docs/RELATO-ARQUIVO.md:6876`, `:7130` | São menções/classificações em prosa; buscar somente a palavra contaria coisas diferentes de uma linha de evento. |
| Título de arquitetura | `### O DESENHO DA LINHA (ponto.OcorrenciaHE)` — `app/docs/RELATO-ARQUIVO.md:22802` | Explicita um modelo, mas não uma ocorrência no formato do aval; o título anterior tem data aproximada em `:22783`. |

`23:4x` fornece uma dezena de minutos, não um instante para decidir uma borda estrita de 24 horas. O selo existente transforma `HH:Mx` em `HH:M0`, usa São Paulo e busca ano no `git blame`, mas verifica **títulos de seção**, não timestamps de cada pergunta. (`app/docs/RELATO.md:141`; `bin/tests/test_hora_da_secao_nao_e_futura.sh:21`, `:25`, `:41–49`.)

Também não é seguro herdar sempre a data da seção anterior: o preâmbulo do RELATO contém entradas de 31/10 antes da primeira seção, datada de 30/10; o histórico recebeu blocos no fim e títulos que perderam o cabeçalho original. O mtime é alterado por escritores de máquina e não demonstra a hora individual do DESENHO. (`app/docs/RELATO.md:41`, `:185`; `app/docs/RELATO-ARQUIVO.md:23–30`; `app/core/esteira_vigia.py:879–884`.)

**2) O repo já responde bem sobre sítios de escrita; não oferece, nas fontes examinadas, um resolvedor geral de toda leitura ou escrita para uma entidade única.** O principal componente reaproveitável é [censo_escritas.py:469](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/app/core/censo_escritas.py:469), cuja saída é `{arquivo, linha, entidade, op, expr}`; as demais fontes têm escopos diferentes. (`app/core/portas.py:9`; `app/core/juizes.py:3`; `app/core/mapa/contrato.py:13`.)

| Fonte existente | Resposta que realmente fornece |
|---|---|
| `core/censo_escritas.py` | Varredura AST, sem Django, de construtores e operações ORM; identifica variáveis amarradas ao modelo e `self` na classe. Marca inferências por campo, `update_fields`, relação e nome. (`app/core/censo_escritas.py:3`, `:6–45`, `:401`.) |
| `core/portas.py::PORTAS` | **Entidade → módulos autorizados a escrever**; não afirma que toda linha do módulo pertence à entidade. (`app/core/portas.py:9`.) |
| `core/portas.py::FAMILIA_DE` | Entidade → família. `CelulaDia` e `FuroDiario`, por exemplo, compartilham família; isso não os torna uma entidade só. (`app/core/portas.py:40–44`.) |
| `core/juizes.py::JUIZES` | Pergunta/família → autoridade `arquivo::função`; não é catálogo de modelos lidos. (`app/core/juizes.py:3–14`.) |
| `core/mapa/contrato.py` | Inscrição de módulo: `@bolha`, entradas, saídas e chamadas; `@bolha` designa família, sem campo estruturado de modelo lido/escrito. (`app/core/mapa/contrato.py:13–25`.) |
| Censos especializados | Há leitura/escrita por modelo em universos específicos, como Batida; não é cobertura geral de entidades. (`bin/esmeril_batida_censo.py:184–200`, `:234–262`.) |
| `esmeril_chamado_censo.py::_onde` | Já associa intervalos de linhas a classe/função por AST; resolve o escopo da citação, não a entidade. (`bin/esmeril_chamado_censo.py:76–89`.) |

Um caminho determinístico **proposto, ainda não implementado [nao verificado]**, seria normalizar o caminho relativamente a `app/`, localizar a expressão e sua função por AST, resolver modelos explícitos e consultar os sítios do censo; aceitar entidade única com evidência, preservando a distinção entre atribuição direta e inferência. As bases existentes são `app/core/censo_escritas.py:361`, `:451`, `:469` e `bin/esmeril_chamado_censo.py:76`.

Os casos que impedem escolher automaticamente uma entidade são concretos:

- **Arquivo com várias entidades:** `ponto/portas/celula.py` é porta de `CelulaDia` e `FuroDiario`; fechamento é porta de `FechamentoMensal` e `PeriodoFechado`. Inverter o mapa e escolher a primeira entidade perderia informação. (`app/core/portas.py:111–112`, `:495–496`.)
- **Origem desconhecida:** parâmetros, `form.save()` e `apps.get_model` podem ficar no balde `entidade=None`; inferência por nome é declarada fraca. (`app/core/censo_escritas.py:38–45`, `:429`.)
- **Falha confundida com vazio:** erro sintático retorna `[]`; erro de leitura é ignorado na varredura. Esses retornos, isoladamente, não sustentam `LIVRE` num juiz triestado. (`app/core/censo_escritas.py:451–456`, `:485–488`.)
- **Cadastro AST imperfeito:** `modelos()` aceita classes com base em `models.py`, inclusive a exceção `ResolvedoraCruzada`; também chaveia pelo nome simples da classe. (`app/core/censo_escritas.py:89–111`; `app/core/portas.py:60–65`.)
- **Nome coloquial versus identidade:** a classe principal é `ChamadoColaborador`; um alias normativo `Chamado → ChamadoColaborador` para este instrumento é [nao verificado]. (`app/chamados/models.py:21`; `app/core/portas.py:58`.)
- **Referência histórica versus hash atual:** o próprio histórico corrige caminho inexistente e número de linha deslocado; interpretar uma referência antiga diretamente na árvore atual pode apontar outra expressão. (`app/docs/RELATO-ARQUIVO.md:990`.)

**3) `_obra_de` reconhece identidade textual da obra, sem consultar entidade ou programa.** Ele procura âncoras como `obra X`, `Obra: X`, markdown e `leitura ... da X`, valida os tokens e escolhe a primeira ocorrência válida entre essas âncoras. Aceita IDs com hífen ou letras+algarismos; IDs sem hífen podem carregar cauda distintiva, como `O261 (C)`. (`bin/disjuntor.py:78–93`; [bin/disjuntor.py:478](/home/ronald/codex_ws/5693e4622c2055d69d24d99ea57d816f98b001c8/bin/disjuntor.py:478), `:492–503`.)

No **Agent**, o B3 atual chama `_obra_de` somente sobre `tool_input.prompt`, para `subagent_type == 'verificador'` e leitura final; a descrição não é usada nesse reconhecimento. Leitura de desenho é excluída por `_eh_final`. Construtores e verificações sem “final” não entram nessa trava B3. (`bin/disjuntor.py:94–98`, `:473–475`, `:944–954`.)

No **Codex**, o wrapper exige `CODEX_OBRA` e `CODEX_PAPEL` e escreve `OBRA:` no prompt. Porém o parser do PreToolUse extrai apenas papel, volta e anexos: **não extrai `CODEX_OBRA` nem chama `_obra_de` para obter a obra da consulta**. Isso precisará de tratamento próprio para cumprir o anexo. (`bin/codex_pergunta.sh:43–48`, `:118–131`; `bin/disjuntor.py:849–862`, `:938–954`.)

Há vínculos documentais obra→programa: O280 aparece como E3, V3-BATERIA como E1 e O261-E-CELULA-20-UM-DONO como E2 do PROGRAMA-CELULA-E-O-DIA. As formas variam entre `PROGRAMA:`, “esta obra é a E2” e “linha da E4”; parser único dessas relações e herança entre aliases/subobras são [nao verificado]. (`app/docs/BACKLOG.md:1398`, `:1399`, `:782`, `:774`.)

Programa tampouco equivale a uma tabela: o ADR reúne status em `CelulaDia` e valor em `DiaPago`. A prioridade entre agrupar por entidade e por programa precisa ser declarada, senão obras de tabelas diferentes podem acabar no mesmo grupo por programa. (`app/docs/ADR/0002-celula-e-o-dia.md:4–8`; política de prioridade: [nao verificado].)

**4) O anexo deixa decisões necessárias em aberto.** As observações sobre suas omissões abaixo se referem ao texto fornecido no prompt, sem arquivo local com numeração verificável: [nao verificado].

- **Disparo e ação incompletos:** “segunda ocorrência na mesma entidade/programa em 24 h” aparece em MORDE, mas falta como regra operacional explícita. Falta integralmente a linha do aval `PADRAO-DE-CIMA <entidade>: classe=... padrao=... armadilha=... obra=...`, seus campos obrigatórios, validação e autor responsável. [nao verificado]
- **Universo de eventos:** acrescenta `PERGUNTA DE DESENHO` sem dizer se é alias do marcador ou ocorrência independente; não define exclusão de exemplos, perguntas respondidas, repetições e movimentação para arquivo. Esses formatos existem nos documentos. (`app/docs/RELATO.md:12`, `:141`; `app/docs/RELATO-ARQUIVO.md:23–40`; política: [nao verificado].)
- **Tempo e agrupamento:** faltam data/ano/fuso, tratamento de `HH:Mx`, timestamp ausente/futuro, vários modelos, aliases, várias referências e prioridade entidade/programa. [nao verificado]
- **`NAO_SEI = nega`:** não define quais chamadas negar quando a entidade é desconhecida. Negar indiscriminadamente ultrapassaria o escopo “obras daquela entidade” do aval. Além disso, o envelope atual captura exceção e libera; a política nova precisa chegar até ele. (`bin/disjuntor.py:989–990`; escopo novo: [nao verificado].)
- **Consumo e rearme:** “mesma mecânica de `linha_libera_b3`” remete a código futuro. O texto B3 disponível exige frescor, SHA consumido e transferência para `liberado`; o código atual só transfere para `rearmado` por CLI. Falta definir se PADRAO libera uma chamada ou um episódio, como trata a terceira ocorrência e como evita redisparar imediatamente pelas mesmas duas linhas ainda dentro das 24 h. (`app/docs/RELATO.md:12`; `bin/disjuntor.py:585–595`; política nova: [nao verificado].)
- **Falha e limite:** não cobre leitura parcial, fonte sintaticamente inválida, JSON corrompido, trava ocupada, prazo/tamanho de varredura e falha ao persistir consumo. O estado atual serializa por flock, mas mantém a decisão mesmo quando a gravação falha; isso exige regra específica para garantir consumo 1x. (`bin/disjuntor.py:219–259`; política nova: [nao verificado].)
- **Medida:** o painel atual conta disparos pelo primeiro token da regra e negativas globais, sem liberações por entidade; a lista exibida também é fixa. Faltam formato dos eventos novos, denominadores e apresentação de medida ilegível. (`bin/disjuntor.py:1030–1055`, `:1147–1160`; medida nova: [nao verificado].)
- **Quem desliga:** `pausar.sh` é escritor genérico de arquivo de pausa; não é, por si, integração com o disjuntor. A chave lida hoje é `DISJUNTOR_DESLIGADO`, global e reservada ao Ronald; comandos do Code que a nomeiem são negados. Falta escolher chave global ou específica, cobertura, condição de retorno e autorização de uso de `pausar.sh`. (`bin/pausar.sh:31–47`; `bin/disjuntor.py:128–157`; decisão nova: [nao verificado].)

O MORDE do anexo também precisa distinguir “duas entidades diferentes” de “duas entidades diferentes **dentro do mesmo programa**”: o aval permite agrupamento por programa, mas não estabelece a precedência entre os dois critérios. Resultado esperado desse caso: [nao verificado].

**5) Estes são os escritores e leitores comprovados que delimitam os arquivos candidatos; o estado específico de PADRAO-DE-CIMA ainda é [nao verificado].**

| Arquivo/estado | Quem escreve | Quem lê |
|---|---|---|
| `app/docs/RELATO.md`, topo humano | O código reconhece escrita manual e preserva o topo; autoria concreta das linhas: [nao verificado]. (`app/core/esteira_vigia.py:829–840`.) | Stop lê duas primeiras linhas úteis; B3 procura DESENHO e identidade da obra nelas. (`bin/hook_stop_fila1.py:411–444`.) |
| `RELATO.md`, linhas de máquina | Disjuntor chama `relato_negacao → esteira_vigia.no_relato`, com flock do diretório e temporário único. Outros escritores declarados são Edit manual, gerador de cortes e deploy agendado, fora dessa trava. (`bin/disjuntor.py:209–211`, `:988`; `app/core/esteira_vigia.py:834–840`.) | Stop verifica afirmações sem prova; publicação lê e raspa o documento. (`bin/hook_stop_fila1.py:1171–1183`; `bin/relato.sh:54–56`, `:98–104`.) |
| `app/docs/RELATO-ARQUIVO.md` | Conduta manda mover material antigo; cabeçalho registra movimentos e append. Escritor automático de rotação: [nao verificado]. (`CLAUDE.md:610–613`; `app/docs/RELATO-ARQUIVO.md:23–40`.) | B3 atual aponta somente para RELATO vivo. Leitura do arquivo por PADRAO-DE-CIMA: [nao verificado]. (`bin/disjuntor.py:57`; `bin/hook_stop_fila1.py:34`, `:430`.) |
| Fontes de modelos, portas e juízes | São declarações no código; autoria efetiva de cada alteração: [nao verificado]. (`app/core/portas.py:98`; `app/core/juizes.py:156`.) | Censo lê modelos/fontes por AST; mapa lê os registros de portas e juízes. (`app/core/censo_escritas.py:89`, `:469`; `app/core/mapa/contrato.py:157–177`.) |
| `BACKLOG.md` e ADR 0002 | Registram os vínculos de programa; escritor único dessas relações: [nao verificado]. (`app/docs/BACKLOG.md:782`, `:1398`; `app/docs/ADR/0002-celula-e-o-dia.md:4`.) | Stop lê BACKLOG para fila; resolvedor automático obra→programa: [nao verificado]. (`bin/hook_stop_fila1.py:33`, `:170`.) |
| `logs/sessao/disjuntor_estado.json` e `.lock` | `_com_estado` lê, trava e grava; varredura B3 e rearme o modificam. **PreToolUse e Stop também escrevem ao varrer B3.** (`bin/disjuntor.py:219–253`, `:559–595`, `:950`; `bin/hook_stop_fila1.py:1050`.) | `ler_estado`, consultas B3 e painel. Consumo por entidade/SHA de PADRAO: [nao verificado]. (`bin/disjuntor.py:265`, `:598`, `:1094`.) |
| `logs/sessao/disjuntor.log` | `trilha`, disparos, rearme e negativas; falha do append é silenciada. (`bin/disjuntor.py:112–125`, `:590`, `:985`.) | `disjuntor.ler` calcula janela; `linha` formata; gerador do ESTADO chama ambos. (`bin/disjuntor.py:1030`, `:1147`; `bin/gerar_estado.py:278–280`.) |
| `DISJUNTOR_DESLIGADO` | Ronald, fora do Code, segundo contrato e guarda. (`bin/disjuntor.py:28–31`, `:136–157`.) | PreToolUse e Stop verificam existência antes de agir. (`bin/disjuntor.py:128`, `:980`; `bin/hook_stop_fila1.py:1040`.) |
| Arquivo escolhido para pausa e `logs/pausas.log` | `pausar.sh` grava dono/motivo/saída, faz rename e registra criação/remoção. (`bin/pausar.sh:24–47`.) | `pausar.sh --ler`; vigia inspeciona nomes específicos de pausa, sem integração automática com um nome novo. (`bin/pausar.sh:19–23`; `app/core/esteira_vigia.py:472–481`.) |
| `.claude/settings.json` | Autoria efetiva: [nao verificado]; guarda impede retirar o hook pelos caminhos reconhecidos. (`bin/disjuntor.py:148–157`.) | Configura PreToolUse com caminho absoluto do host; ativação efetiva em produção: [nao verificado]. (`.claude/settings.json:62–68`.) |
| Pergunta/resposta Codex | Escritor da pergunta: [nao verificado]; wrapper monta o prompt e dirige a resposta para `codex exec -o`. (`bin/codex_pergunta.sh:118–135`, `:157`.) | Wrapper lê pergunta/anexos; PreToolUse atual analisa o comando, sem ler a pergunta para resolver obra/entidade. (`bin/codex_pergunta.sh:131–133`; `bin/disjuntor.py:849–862`.) |

## 31/10 16:19 · sessao 4a312e2f06f8 · - · - · origem=Ronald (tmux)

**Achado 7 — recomendo (e), NAO PERGUNTADO.** O commit já estava escrito na volta 3: abertura de `v3.q`, literal, “Obra F1C4-3-EMISSORES volta 3, commit c5c43360”. Portanto, não sustenta a justificativa “commit ainda não escrito”. Nenhuma pergunta fornecida pede auditoria da mensagem ou presença da linha `LEI-AKITA`; nenhuma resposta fornecida acusa sua ausência. A frase genérica “Aponte tambem qualquer outro defeito no delta que estas perguntas nao cobrem” (`v3.q`, fechamento; numeração original [nao verificado]) não explicita esse tema.

O trecho mais próximo de resposta é `v3.r`: “Execução dos testes, execução das mutações e efeitos reais sobre os números de produção: **[nao verificado]**.” Ele não trata da mensagem. **Trecho de resposta sobre a falta da linha: inexistente nos anexos fornecidos** [nao verificado quanto à numeração]. O sítio `fabrica_da_celula.py` participa desde v1; o commit acusado entra documentalmente em v3.q. A regra exige a linha em `CLAUDE.md:62–64`; o selo distingue ausência de linha de validação de seu conteúdo em `bin/tests/test_lei_akita.sh:142–143`. A ausência efetiva no commit só está atestada por `opus_v11.txt`, item 7; mensagem integral e histórico não disponíveis [nao verificado].

**Achado 8 — recomendo (e), com ressalva material.** Nenhuma pergunta fornecida pede conferir a afirmação da mensagem contra a coluna PROTEGE. O mais próximo é `v2.q`, literal: “Leis citadas pelos sitios (grep LEIS/CORTES) e app/docs/JUIZES.md vao anexos.” `v2.r` responde: “A L-061 exige leitura da célula/ata; ela não demonstra equivalência dos predicados sobreviventes.” Isso discute a lei, sem auditar a coluna ou a mensagem. **Trecho de resposta sobre a afirmação “nenhuma lei lista os arquivos em PROTEGE”: inexistente nos anexos fornecidos** [nao verificado quanto à numeração].

**A cópia não confirma a alegação “L-061, L-033, L-050 listam em PROTEGE”.** O cabeçalho põe PROTEGE na quinta coluna (`app/docs/LEIS.md:57`); essa célula está vazia nas três linhas (`app/docs/LEIS.md:81`, `:91`, `:95`). Os arquivos mencionados nessas linhas aparecem na coluna **DONO**, anterior à coluna PROTEGE. Assim, há divergência entre a cópia e o resumo Opus, que deve ser preservada no laudo. O selo efetivamente lê a coluna PROTEGE por nome (`bin/tests/test_lei_protege_sitio.sh:58`), não a coluna DONO.

A volta de entrada da **afirmação na mensagem** do item 8 é **[nao verificado]**: `opus_v11.txt` não identifica seu commit; não há como dizer que surgiu depois de v8. O tema geral L-061 já aparece em `v2.r`, mas isso não data a afirmação acusada. Sem mensagem integral, não transformar essa lacuna em prova de classe (c) por commit posterior. `triagens.txt` vazio não permite classe (b) para nenhum dos dois [nao verificado quanto à numeração dos anexos]. [nao verificado]

## 31/10 16:19 · sessao 664f4c3bfd34 · - · - · origem=Ronald (tmux)

**Achado 2 — (c), dado de produção e mensagem posterior à última chamada.** A fonte local diz literalmente: “ata explicita 'indefinido' divergiria […] Prod 31/10: 0 de 127.784 atas” (`app/docs/BACKLOG.md:806`). Isso sustenta a distinção de universo apontada pelo opus; não prova autonomamente o novo número 158/143.452, que consta somente do dado raspado (`opus_v11.txt`, item 2; linha original [nao verificado]).

- Pergunta pertinente: `v2.q`, aval: “antes, sombra com novos CHAMADOS por emissor; ate 50 pousa” — linha original [nao verificado].
- Resposta: `v2.r`, item 1: “A sombra com novos CHAMADOS por emissor, e o atendimento ao teto de 50, são [não verificado] no material fornecido.” — linha original [nao verificado].
- Entrada: o número 0/127.784 vem da obra anterior F1C4-3, segundo a própria identificação “Codex verif4 F1C4-3” (`app/docs/BACKLOG.md:806`). Sua atribuição errada no commit b3bef403 é documentada em v11, depois da última chamada v8; volta anterior exata de introdução da frase [nao verificado].

**Achado 5 — (c), medição não disponível e mudança final não examinada.** A cópia ainda contém o predicado antigo do supra (`app/ponto/supra_juiz.py:443`) e a chamada do cartório sem o insumo novo (`app/ponto/services/cartorio.py:1207`). Portanto não permite reconstituir a sombra final nem a implementação migrada descrita no opus (`opus_v11.txt`, item 5; linha original [nao verificado]).

- Pergunta: `v2.q`, mesmo aval literal de sombra citado acima — linha original [nao verificado].
- Resposta sobre limite: `v2.r`, item 1, mesma ressalva de medição acima — linha original [nao verificado].
- Resposta que já percebeu a semântica: `v1.r`, item 1: “tipo ausente, `None` ou vazio não satisfaz `A`” — linha original [nao verificado]. Isso mostra conhecimento da semântica geral, **não descoberta do defeito da migração final**.
- Entrada: tema sombra presente em v2; semântica de tipo `None` presente em v1; migração e sombra finais documentadas em v11. Volta exata entre esses marcos [nao verificado].

**Achado 6 — (c), suíte/log final.** A regra local exclui expressamente “RED, suite, dado de prod” da falha do Codex (`CLAUDE.md:557`). [nao verificado]

- Pergunta relacionada: `v1.q`, item 5: “selo/RED sem mordida” — linha original [nao verificado].
- Resposta: `v1.r`, abertura: “Execução de testes/mutantes […] [não verificado]” — linha original [nao verificado]. [nao verificado]
- Última resposta: `v8.r`, item 3: “execução da suíte Django com as mutações: [não verificado]” — linha original [nao verificado]. [nao verificado]
- Entrada: o arquivo `v11_green.log` e seus zero bytes aparecem no achado final v11; ocorrência nas voltas anteriores [nao verificado]. A idade do comportamento “suite nao imprime hash” também fica [nao verificado]. [nao verificado]

Não há linha fornecida em `triagens.txt`; portanto nenhum destes três pode receber classe (b) com descarte comprovado — conteúdo da triagem [nao verificado].

## 31/10 16:19 · sessao 3f534d69f56a · - · - · origem=Ronald (tmux)

**Achado 1: (c), defeito documental criado na v11, após a última chamada guardada, v8.** O antecedente “predicado próprio no supra” já era conhecido, mas isso não equivale ao defeito novo “migrou e deixou corte/backlog divergentes”. (`opus_v11.txt`, item 1; `v8.q`, abertura; `v4.r`, item 2.)

- Pergunta pertinente: `v4.q`, item 2: “Ha sitio de producao que ainda responde [...] por conta propria [...] sem perguntar a chamados/juizes.py::dia_sem_trabalho?”
- Resposta: `v4.r`, item 2: “a migração está declarada no corte, mas não realizada nesse sítio.”
- Evidência local do antecedente: `app/ponto/supra_juiz.py:443` mantém predicado próprio; `app/docs/BACKLOG.md:806` mantém `SUPRA-DELEGA-DIA-SEM-TRABALHO` aberta.
- Entrada: tema supra já aparece na **v1**; registro/corte aparece na **v4**; contradição documental causada pela migração pertence à **v11** conforme `opus_v11.txt`, item 1.

**Achado 3: origem exata da catraca e primeira volta são [nao verificado].** A cópia não contém `com_insumo_dia_sem_trabalho`; o sítio final não pode ser confrontado diretamente. O tema genérico “testes exigem delegação?” aparece na **v1**, e Codex apontou explicitamente esse tipo de mutante sobrevivente. (`v1.q`, itens 4–5; `v1.r`, item 4.) [nao verificado]

- Pergunta: `v1.q`, item 4: “Os testes novos mordem a reversao de cada um dos 4 sitios individualmente?” [nao verificado]
- Resposta: `v1.r`, item 4: “também não exigem estruturalmente delegação ao juiz — um clone equivalente passa nos asserts de retorno.”
- Isso **não prova que Codex viu o defeito específico** “nome da função aceito com `ata=cel.ata`”. Sua primeira exposição e classificação definitiva dependem de saber quando nasceu a catraca, informação ausente. [nao verificado]
- Não usar **(b)**: nenhuma linha fornecida de triagem vincula esse mutante ao descarte. [nao verificado]

**Achado 4: o predicado do cartório foi visto; a consequência final de cláusula inatingível não foi apontada nos registros fornecidos.** A classificação única exige separar antecedente de alteração posterior. (`v1.r`, item 3; `v2.r`, item 6; `v3.r`, item 6; `opus_v11.txt`, item 4.)

- Pergunta explícita: `v3.q`, item 6: “QUEM MAIS LE celula com trabalha=None em chamados/ e ponto/ (cartorio, vigias, protesto FURO_SEM_COBRANCA, contadores) [...]?”
- Resposta explícita: `v3.r`, item 6: “Cartório | `None` passa em `_protesta`”.
- Pergunta ainda mais direta: `v4.q`, item 2: “Ha sitio de producao que ainda responde [...] por conta propria [...]?”
- A resposta `v4.r`, item 2, enumera predicados próprios, mas **omite `_protesta`**; no item 1 descreve `_C18` e não registra esse sítio. (`v4.r`, itens 1–2.)
- O predicado existe na cópia em `app/ponto/services/cartorio.py:1204–1205`: `return _cel.trabalha is not False and _acesos(x, _ata) is not True`.
- O juiz antigo lê `cel.ata` e começa pela cláusula `cel.trabalha is False`. (`app/chamados/juizes.py:407–408`.)
- Entrada verificável: sítio/predicado já na **v1**; `_C18` na **v4**. Primeira volta em que a delegação nova tornou a cláusula inatingível: **[nao verificado]**.
- **(a)** para o predicado inteiro seria incorreto, pois ele foi visto; **(b)** exige descarte documentado que não temos. Para “inatingível após delegação” pode caber **(c)** se a delegação nasceu após v8, mas essa cronologia específica precisa de evidência adicional. [nao verificado]

**Cuidado com triagem local:** `app/docs/RELATO.md:75` diz “2o predicado do supra descartado para fila”, mas refere-se à obra anterior **F1C4-3-NONE-COBRA**, não ao defeito documental da v11 EMISSORES. `app/docs/RELATO.md:29` contém triagem v1 EMISSORES, mas seus rótulos “P2a/P2b” não são identificáveis univocamente na resposta v1 fornecida. Não sustenta atribuição automática a **(b)**. (`app/docs/RELATO.md:29`; `app/docs/RELATO.md:75`.)

## 31/10 16:18 · sessao 14252be083ec · - · - · origem=Ronald (tmux)

**B3 presente × B3 prometido**

- O código desta cópia ainda usa limiar 3, acumula reprovações por agente/obra e libera por `--rearma-b3`, transferindo agentes para `rearmado`; não há consumo de linha nesta implementação (`bin/disjuntor.py:68`, `bin/disjuntor.py:545`, `bin/disjuntor.py:585`, `bin/disjuntor.py:1170`).
- O topo com `DESENHO` apenas cala o lembrete do Stop; a negativa do PreToolUse continua enquanto a obra estiver disparada (`bin/hook_stop_fila1.py:411`, `bin/hook_stop_fila1.py:1049`, `bin/disjuntor.py:947`).
- O contrato B3 **documentado** já exige reconhecedor único `linha_libera_b3`, duas linhas úteis do topo, linha posterior ao disparo (`mtime`), hash consumido uma vez e reprovações transferidas para `liberado`; isso está no texto, ainda não neste código (`app/docs/RELATO.md:11`, `app/docs/RELATO.md:12`). A versão triestado v4 mencionada pelo usuário não foi fornecida como código: [nao verificado].
- O contrato B3 documentado ainda diz “falha aberto”; o envelope real do PreToolUse captura qualquer exceção e libera. Assim, escrever simplesmente “NAO SEI = nega (molde B3 v4)” não descreve a implementação disponível (`app/docs/RELATO.md:13`, `bin/disjuntor.py:989`). No Stop, exceção também retorna `None`, mantendo o caminho anterior (`bin/hook_stop_fila1.py:1035`, `bin/hook_stop_fila1.py:1061`).

**Lacunas/contradições no anexo**

O anexo foi recebido apenas no prompt; referências ao conteúdo dele abaixo são [nao verificado no repo].

1. Falta declarar como **ação normativa** o limiar “segunda DESENHO da mesma entidade/programa em 24 h”; ele aparece apenas em MORDE. Falta integralmente a linha liberadora do aval `PADRAO-DE-CIMA <entidade>: classe=... padrao=... armadilha=... obra=...`, seus campos válidos, quem a escreve e como verifica vínculo com a obra bloqueada. [nao verificado]
2. Incluir `PERGUNTA DE DESENHO` amplia a forma literal `DESENHO <obra>` do aval; precisa dizer se é alias aceito, ocorrência distinta ou repetição da mesma pergunta. Também faltam deduplicação topo/corpo/arquivo e identidade estável do evento. [nao verificado]
3. “Com hora nas últimas 24 h” não define data/ano/fuso, `12:4x`, herança do cabeçalho, falta de hora ou instante futuro. “Tabela/modelo (ou PROGRAMA)” não resolve múltiplos modelos, aliases, prioridade entidade versus programa ou ausência de classificação. [nao verificado]
4. “NAO SEI = nega” precisa especificar **quem** será negado quando a entidade da ocorrência ou da chamada é desconhecida; negar tudo muda o escopo do aval “obras daquela entidade”. O envelope disponível deixa exceções passarem (`bin/disjuntor.py:989`). [nao verificado para a política nova]
5. “Mesma mecânica do linha_libera_b3” depende de código futuro: o texto B3 disponível especifica frescor, hash e transferência para `liberado`, mas o código só implementa rearme explícito (`app/docs/RELATO.md:12`, `bin/disjuntor.py:585`).
6. Falta definir se o disparo expira com a janela, fica travado até liberação, se a linha libera uma chamada ou um episódio e como tratar terceira ocorrência. No B3 documentado, a reprovação seguinte dispara novamente (`app/docs/RELATO.md:12`). [nao verificado para PADRAO-DE-CIMA]
7. Faltam limites de tamanho/tempo de varredura, erro de leitura parcial, estado corrompido, lock ocupado, gravação falhada e concorrência de dois consumidores. O estado atual usa flock e rename, mas permite manter decisão mesmo se a persistência falhar: isso é crítico para “consumir uma vez” (`bin/disjuntor.py:219`, `bin/disjuntor.py:230`, `bin/disjuntor.py:250`, `bin/disjuntor.py:254`).
8. O painel atual conta disparos pelo primeiro token depois de `DISJUNTOR` — regra, não entidade — e negativas globais; não conta liberações. A lista exibida de regras é fixa. Portanto, a medida anunciada pelo anexo ainda exige contrato de eventos/identidade, esperado e erro de medida (`bin/disjuntor.py:1030`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1147`).
9. “Ronald ou `pausar.sh`” não identifica arquivo de desligamento, cobertura nem condição de religar. O mecanismo atual só lê existência de `DISJUNTOR_DESLIGADO`, reservado ao terminal do Ronald; o Code é impedido até de citar a chave em Bash. `pausar.sh` é escritor genérico: não há integração automática dele com um novo instrumento (`bin/disjuntor.py:128`, `bin/disjuntor.py:144`, `bin/disjuntor.py:979`, `bin/pausar.sh:31`). O vigia só examina os nomes `esteira.pausada` e `esteira.integrando` nessa rotina; não fiscaliza automaticamente qualquer arquivo novo de pausa (`app/core/esteira_vigia.py:472`).

**Escritores/leitores relevantes**

| Arquivo/estado | Quem escreve | Quem lê |
|---|---|---|
| `app/docs/RELATO.md` — topo humano | Escrita manual é reconhecida explicitamente; identidade do agente que escreveu cada linha: [nao verificado] (`app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:839`). | Stop lê duas linhas úteis, ignorando título e brancos; B3 busca DESENHO e ID nessa janela (`bin/hook_stop_fila1.py:420`, `bin/hook_stop_fila1.py:411`). Leitor novo PADRAO-DE-CIMA: [nao verificado]. |
| `RELATO.md` — negativas de máquina | `main_pretool → relato_negacao → esteira_vigia.no_relato`; escreve seção ALARMES, preservando topo, com trava no diretório e temporário único (`bin/disjuntor.py:988`, `bin/disjuntor.py:209`, `app/core/esteira_vigia.py:834`, `app/core/esteira_vigia.py:859`). | Stop checa afirmações sem prova por subprocesso (`bin/hook_stop_fila1.py:1172`, `bin/hook_stop_fila1.py:1178`). |
| `logs/sessao/disjuntor_estado.json` + `.lock` | `_com_estado`; varredura B3 e rearme o modificam (`bin/disjuntor.py:219`, `bin/disjuntor.py:560`, `bin/disjuntor.py:585`). | `ler_estado`, `b3_disparados`; PreToolUse e Stop chamam varredura que também escreve (`bin/disjuntor.py:265`, `bin/disjuntor.py:598`, `bin/disjuntor.py:950`, `bin/hook_stop_fila1.py:1050`). Estado novo de consumo por entidade: [nao verificado]. |
| `logs/sessao/disjuntor.log` | `trilha`, `dispara`, rearme, `main_pretool`; append, falha de trilha é silenciada (`bin/disjuntor.py:112`, `bin/disjuntor.py:123`, `bin/disjuntor.py:590`, `bin/disjuntor.py:985`). | `disjuntor.ler` calcula 24 h; `linha` formata; `gerar_estado.py` chama ambos (`bin/disjuntor.py:1030`, `bin/disjuntor.py:1147`, `bin/gerar_estado.py:277`). |
| `DISJUNTOR_DESLIGADO` | Ronald fora do Code, segundo contrato e guarda (`bin/disjuntor.py:28`, `bin/disjuntor.py:136`). | `desligado`, chamado antes de agir no PreToolUse e Stop (`bin/disjuntor.py:128`, `bin/disjuntor.py:980`, `bin/hook_stop_fila1.py:1040`). |
| Arquivo arbitrário de pausa + `logs/pausas.log` | `pausar.sh` grava QUEM/QUANDO/POR_QUE/SAIDA, rename e trilha; `--soltar` registra e remove (`bin/pausar.sh:24`, `bin/pausar.sh:31`, `bin/pausar.sh:39`). | `pausar.sh --ler`; vigia reconhece dono/saída para dois nomes específicos (`bin/pausar.sh:19`, `app/core/esteira_vigia.py:472`, `app/core/esteira_vigia.py:481`). |
| `.claude/settings.json` | O molde atribui aplicação à sessão no pouso; autoria efetiva desta cópia: [nao verificado] (`bin/disjuntor_settings.json:2`). | Configura Stop e PreToolUse com caminhos absolutos do host; ativação real em produção: [nao verificado] (`.claude/settings.json:9`, `.claude/settings.json:68`). |
| Pergunta/resposta do Codex | Escritor da pergunta: [nao verificado]; resposta é destino `-o` do `codex exec` (`bin/codex_pergunta.sh:40`, `bin/codex_pergunta.sh:157`). | Wrapper lê pergunta/anexos e monta prompt (`bin/codex_pergunta.sh:111`, `bin/codex_pergunta.sh:131`). PreToolUse atual não lê a pergunta para extrair obra (`bin/disjuntor.py:849`). |

Detalhe útil para pergunta 3: wrapper exige `CODEX_OBRA` e papel por ambiente, mas `_codex_cmds` atual só extrai papel/volta/anexos; não extrai `CODEX_OBRA`, pergunta nem entidade. Assim B3 atual não usa `_obra_de` em chamada Codex; ele só a usa no Agent verificador final (`bin/codex_pergunta.sh:43`, `bin/disjuntor.py:859`, `bin/disjuntor.py:947`).

## 31/10 16:18 · sessao 2237df423709 · - · - · origem=Ronald (tmux)

Há bases reaproveitáveis para **escritas**, mas elas não entregam uma função geral “arquivo:linha → entidade lida ou escrita”; `varrer_arvore` entrega sítios de escrita, `PORTAS` declara escritores permitidos e `JUIZES` declara autoridades por pergunta/família. (`app/core/censo_escritas.py:469`; `app/core/portas.py:9`; `app/core/juizes.py:3`)

**Fonte principal para arquivo:linha → entidade**

- `app/core/censo_escritas.py` é puro AST/os, sem Django; `varrer_arvore(app_root)` devolve registros `{arquivo, linha, entidade, op, expr}`, com caminhos relativos a `app/`. A operação da linha é identificada por `no.lineno`, portanto o resultado localiza o início da chamada, não todo seu intervalo. (`app/core/censo_escritas.py:1`; `app/core/censo_escritas.py:361`; `app/core/censo_escritas.py:469`; `app/core/censo_escritas.py:489`)
- Reconhece construtores `Modelo(...)`, operações ORM de criação/alteração/exclusão, `save/delete` de variável ligada ao modelo na função e `self` na classe do modelo. Preserva aliases de importação e amarrações de querysets/`get_object_or_404`. (`app/core/censo_escritas.py:6`; `app/core/censo_escritas.py:261`; `app/core/censo_escritas.py:285`; `app/core/censo_escritas.py:335`; `app/core/censo_escritas.py:364`; `app/core/censo_escritas.py:401`)
- Já fornece inferências explicitamente marcadas: `[campo]`, `[update_fields]`, `[relacionado]`, `[nome]`. A última é declarada fraca; nome mais longo vence, empate não atribui. Gerente reverso e campo só entram quando identificam um modelo único. (`app/core/censo_escritas.py:15`; `app/core/censo_escritas.py:20`; `app/core/censo_escritas.py:38`; `app/core/censo_escritas.py:151`; `app/core/censo_escritas.py:165`; `app/core/censo_escritas.py:182`)
- O comando `censo_escritas` já publica `arquivo:linha entidade expressão`, inclusive o balde não atribuível, e declara que só lê arquivo, sem banco. Para o instrumento no host, seria reaproveitável o módulo puro, sem executar `manage.py`. (`app/core/management/commands/censo_escritas.py:1`; `app/core/management/commands/censo_escritas.py:25`; `app/core/management/commands/censo_escritas.py:35`; `app/core/management/commands/censo_escritas.py:39`)

**Outros mapas úteis, com limites**

| Fonte | O que responde / limite |
|---|---|
| `core/portas.py::PORTAS` | Entidade → módulos que podem escrever; não prova que toda linha desses módulos pertence àquela entidade. (`app/core/portas.py:9`) |
| `core/portas.py::FAMILIA_DE` | Entidade → família. Família não pode substituir entidade sem mudar o aval. `CelulaDia` e `FuroDiario` têm a mesma família, por exemplo. (`app/core/portas.py:38`; `app/core/portas.py:44`) |
| `core/juizes.py::JUIZES` | Família/pergunta → `arquivo::função`/`Classe.método`; diz quem decide, não fornece entidade lida. (`app/core/juizes.py:3`; `app/core/juizes.py:7`; `app/core/juizes.py:156`) |
| `core/mapa/contrato.py` | Lê docstring de módulo com `@bolha`, `@entra`, `@sai`, `@chama` etc.; `@bolha` é família e não há campo estruturado de modelo lido/escrito. (`app/core/mapa/contrato.py:13`; `app/core/mapa/contrato.py:39`; `app/core/mapa/contrato.py:83`) |
| `bin/esmeril_chamado_censo.py::_onde` | Já mapeia números de linha para `(classe, função)` por intervalos AST; útil para associar uma citação a função, mas não resolve sua entidade. (`bin/esmeril_chamado_censo.py:76`) |
| `core/medida_leitor_do_dia.py` | Mede derivação de status/valor com vocabulários e lista de leitores; não é catálogo geral de modelos lidos. (`app/core/medida_leitor_do_dia.py:14`; `app/core/medida_leitor_do_dia.py:32`; `app/core/medida_leitor_do_dia.py:70`) |

**Ambiguidades reais**

- Inverter porta por arquivo pode devolver várias entidades: `ponto/portas/celula.py` escreve `CelulaDia` e `FuroDiario`; `ponto/services/fechamento.py` escreve `FechamentoMensal` e `PeriodoFechado`. Não se pode escolher a primeira. (`app/core/portas.py:111`; `app/core/portas.py:112`; `app/core/portas.py:495`; `app/core/portas.py:496`)
- `x.save()` com origem desconhecida, parâmetro, `form.save()` e `apps.get_model` podem ficar com `entidade=None`; o contrato do censo exige publicar esse balde. (`app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:429`; `app/core/censo_escritas.py:507`)
- O AST de `modelos()` aceita qualquer classe de topo com base em `models.py`, não apenas modelo Django. O repo documenta o falso positivo `ResolvedoraCruzada(ValueError)` e o teste que passou a consultar o registro Django para validar `FAMILIA_DE`. Portanto a lista AST sozinha não comprova modelo. (`app/core/censo_escritas.py:108`; `app/core/portas.py:60`; `app/core/tests/test_censo_escritas.py:264`)
- Modelos são chaveados somente por nome da classe, com `setdefault`; duplicidade de nomes em apps distintas não se distingue pelo resultado desse cadastro. (`app/core/censo_escritas.py:89`; `app/core/censo_escritas.py:111`)
- Arquivo ilegível e fonte com erro sintático podem virar ausência no censo atual: `varrer_fonte` retorna `[]` em `SyntaxError`; `varrer_arvore` ignora erro de leitura. Para usar como juiz NAO_SEI, seria necessário distinguir erro de “nenhuma escrita encontrada”. (`app/core/censo_escritas.py:451`; `app/core/censo_escritas.py:485`)
- Inferir por campo ou padrão não prova o modelo. Outro censo do próprio repo declara explicitamente que datas de início/fim não distinguem vínculo de férias/período aquisitivo/ordem, e classifica nominalmente as exceções. (`app/core/censo_pergunta2.py:15`; `app/core/censo_pergunta2.py:42`)
- “Chamado” do aval não é o nome da classe principal existente: ela é `ChamadoColaborador`; normalização/alias “Chamado → ChamadoColaborador” não está estabelecida nos mapas examinados. (`app/chamados/models.py:21`; `app/core/portas.py:58`; alias: [nao verificado])

**Caminho determinístico proposto**

Usar a citação normalizada relativamente a `app/`, identificar o nó/função por AST, consultar os sítios de `censo_escritas` nessa chamada/função e aceitar somente entidade única sustentada pelo código; conservar marca de inferência. Quando a linha é só leitura, ou função toca vários modelos, ou a citação não bate com a árvore, devolver NAO_SEI em vez de expandir para a família. Esta é proposta, não comportamento implementado. (bases: `app/core/censo_escritas.py:469`; `bin/esmeril_chamado_censo.py:76`; proposta: [nao verificado])

**Ligação obra → PROGRAMA**

Há vínculos documentais concretos, porém textuais e com várias formas:

- `O-PROJECOES` tem `PROGRAMA: complemento STATUS-DO-DIA-NA-CELULA do PROGRAMA-CELULA-E-O-DIA`. (`app/docs/BACKLOG.md:730`)
- `O280` tem `PROGRAMA: E3 do PROGRAMA-CELULA-E-O-DIA`. (`app/docs/BACKLOG.md:1398`)
- `V3-BATERIA-DE-CASOS-PELA-REGRA` tem `PROGRAMA: E1 do PROGRAMA-CELULA-E-O-DIA`. (`app/docs/BACKLOG.md:1399`)
- `O261-E-CELULA-20-UM-DONO` declara em prosa “esta obra é a E2” desse programa. (`app/docs/BACKLOG.md:782`)
- `E4-CAL-RETIDO-FOLHA-NO-LEITOR` declara “linha da E4 do PROGRAMA...”, enquanto `PROGRAMA-CELULA-E-O-DIA-DUAS-PISTAS` é complemento do programa. (`app/docs/BACKLOG.md:774`; `app/docs/BACKLOG.md:773`)
- ADR 0002 identifica seu programa, mas também mostra que programa não equivale a uma só tabela: status em `CelulaDia`, valor em `DiaPago`. (`app/docs/ADR/0002-celula-e-o-dia.md:4`; `app/docs/ADR/0002-celula-e-o-dia.md:8`)

Assim, há material para declarar um vínculo explícito e auditável, mas interpretar “complemento”, etapas, programa pai e família como equivalentes seria nova regra. Parser único já implementado de obra→programa/entidade: [nao verificado].

## 31/10 16:18 · sessao 3fb9a544fd51 · - · - · origem=Ronald (tmux)

P1 — formas e limites observados:

| Forma | Evidência | Hora e entidade |
|---|---|---|
| Forma literal apenas como **exemplo de contrato**: ``DESENHO <obra>: lei=... pergunta=... <arquivo>:<linha>`` | `app/docs/RELATO.md:12` | Sem hora; placeholders não constituem evento. Excluir contratos/exemplos é necessário para não contar ocorrência fictícia. |
| Topo real: `REPROVA-E-DESENHO V3-RETOMA (12:4x, ...)` | `app/docs/RELATO.md:4` | Obra explícita, hora aproximada sem data; várias referências (motor, alimentação, regras, núcleo) e `CCT.feriados`, sem entidade única declarada. |
| `> **[RESPONDIDA 23:5x...] PERGUNTA DE DESENHO O261-E-F5 (...), 30/10 23:4x:**` | `app/docs/RELATO.md:141` | Data/hora aproximada do evento; distinguir hora da resposta `23:5x`, do disparo e do evento `23:4x`. “família chamado” aparece, mas também célula, folha, vigia, referências `fora_da_folha.py:51` e `raiox.py:151`; primeiro arquivo:linha não define entidade por si. |
| `PERGUNTA DE DESENHO (nao devolve turno) -- **GEO-SILENCIO-DE-PING: ...` | `app/docs/RELATO-ARQUIVO.md:983` | Obra após texto intermediário, sem hora própria nem arquivo:linha na linha; referências de código aparecem nos itens seguintes, por exemplo `ponto/presenca.py:12` em `:985`, `static/pwa.js:404` em `:987`. Timestamp exato [nao verificado]. |
| Prosa respondida/riscada: `~~(pergunta de DESENHO -- ...` | `app/docs/RELATO.md:947` | Hora disponível é da resposta `26/10 22:1x`, não da abertura; nomes de funções sem localização. Contagem como novo evento exige decisão de contrato [nao verificado]. |
| Prosa de custo/classificação, não marcador de evento: `(DESENHO)`, `= DESENHO`, `DESENHO, NAO DEFEITO` | `app/docs/RELATO.md:1302`, `:1333`, `:4058`; `app/docs/RELATO-ARQUIVO.md:6876`, `:7130` | Buscar apenas substring `DESENHO` conta classificações, títulos e menções históricas, além de perguntas. |
| Título de desenho: `### O DESENHO DA LINHA (ponto.OcorrenciaHE)` | `app/docs/RELATO-ARQUIVO.md:22802` | Modelo explícito, mas nenhuma obra/evento no formato avalizado; título pai contém `02/10 03:5x` em `:22783`. |

O selo de hora existente só reconhece `^## DD/MM HH:M[x\d]`, fixa TZ São Paulo, transforma dezena em `HH:M0` e infere ano de `git blame`; ele verifica título futuro, não entrega hora por evento (`bin/tests/test_hora_da_secao_nao_e_futura.sh:21`, `:25`, `:41-49`). Portanto `23:4x` oferece intervalo de dez minutos, sem instante exato para uma borda estrita de 24h (`app/docs/RELATO.md:141`).

Não é seguro herdar sempre o título anterior: a primeira seção do RELATO está em `:185`, datada 30/10, enquanto o preâmbulo contém entradas 31/10 (`app/docs/RELATO.md:41`, `:57`, `:63`). O arquivo histórico é anexado no fim para preservar citações, pode ter `###` que perderam seu `##` original e não é uma sequência temporal simples (`app/docs/RELATO-ARQUIVO.md:23-30`). A dieta inclusive manteve cópias de contratos no vivo para leitores de testes, enquanto a história ficou no arquivo; contar vivo+arquivo sem identidade/deduplicação pode contar representação repetida (`app/docs/RELATO-ARQUIVO.md:34-40`).

P5 — RELATO e arquivo:

- **Topo manual / perguntas DESENHO:** o código documenta “topo escrito a mão” e preserva seus bytes; autoria concreta das linhas observadas [nao verificado] (`app/core/esteira_vigia.py:829-840`). A conduta manda perguntas de desenho ao topo (`CLAUDE.md:458-459`).
- **RELATO escritor de máquina:** `app/core/esteira_vigia.py::no_relato`, seção `## ALARMES DA ESTEIRA`; flock do diretório, temporário único e `os.replace` (`app/core/esteira_vigia.py:815-840`, `:844-884`). Este escritor altera mtime do documento inteiro; mtime não prova hora de cada DESENHO (`:879-884`).
- **Outros escritores do RELATO:** gerador de SEUS CORTES lê e regrava inteiro (`bin/gerar_cortes.py:114-136`); deploy agendado insere uma linha e regrava (`bin/deploy_agendado.sh:31-44`). O próprio escritor único declara que estes e o Edit manual não tomam sua trava (`app/core/esteira_vigia.py:839-840`).
- **Leitor do topo atual:** hook lê só as duas primeiras linhas úteis, pula cabeçalho/brancas, tira crases iniciais; não lê todo corpo para B3 (`bin/hook_stop_fila1.py:420-444`). `_pergunta_b3_escrita` aceita substring `DESENHO` e identidade de obra por `DJ.obras_da_linha` (`:411-417`).
- **Leitores/prova/publicação:** checador lê RELATO e verifica afirmações fortes com PROVA em janela de seis linhas (`bin/relato_afirma_com_prova.py:33-50`, `:86-99`); wrapper de publicação o invoca e raspa/copia o RELATO para destino público (`bin/relato.sh:54-56`, `:98-104`). Publicador não é escritor do RELATO original nesses trechos.
- **RELATO-ARQUIVO:** regra exige mover material de mais de três dias no mesmo commit (`CLAUDE.md:610-613`); lei registra isso como conduta sem guarda mecânica (`app/docs/LEIS.md:135`). Cabeçalho documenta cortes manuais por conteúdo, movimento integral e append para preservar linhas (`app/docs/RELATO-ARQUIVO.md:3-14`, `:23-30`, `:42-50`). Escritor automático/estado de rotação [nao verificado].

## 31/10 16:16 · sessao fb824fd80c4f · ADR-0003 · censo · origem=Code

Leitura estática do código. **O total absoluto de leitores do repositório ficou `[nao verificado]`**: abaixo conto funções comprovadas, com até oito exemplos por campo. `≥8` significa oito leitores comprovados apresentados; `0 localizado` não prova inexistência. Execução em produção, conteúdo efetivamente armazenado no JSON e correspondência do snapshot com o HASH informado: `[nao verificado]`.

Os caminhos são relativos a `app/`. Para reduzir repetição nas tabelas, as referências abreviadas têm estas expansões literais:

| Referência | Arquivo |
|---|---|
| `M:linha` | `escala/models.py:linha` |
| `P:linha` | `ponto/portas/celula.py:linha` |
| `C:linha` | `ponto/services/cartorio.py:linha` |
| `G:linha` | `ponto/management/commands/gerar_celulas.py:linha` |
| `L:linha` | `escala/services/leitor_celula.py:linha` |
| `J:linha` | `chamados/juizes.py:linha` |
| `F:linha` | `folha/export.py:linha` |
| `S:linha` | `ponto/status_dia.py:linha` |
| `T:linha` | `ponto/turnos.py:linha` |
| `U:linha` | `escala/utils.py:linha` |

Os nomes abreviados de escritores usados abaixo são:

| Nome | Função e persistência |
|---|---|
| **GER** | `gerar_celulas_periodo`: cria e salva células — `P:593`, `P:605`, `P:615`. |
| **REG** | `regenerar_celulas_vinculo`: cria, limpa previsão fora da vigência ou regrava previsão — `P:231`, `P:247`, `P:259`. |
| **REV** | `reverter_regeneracao_dia`: restaura DNA anterior e salva — `P:527`, `P:538`. |
| **LAV** | `lavrar_veredito`: atribui julgamento/status e persiste por `salvar_e_emitir` — `P:709`, `P:743`, `P:760`, `P:775`. |
| **CAR** | `carimbar`: grava impressão, datas e insumos, incluindo campos preparados por LAV — `P:787`, `P:800`, `P:807`. |
| **PREC** | `carimbar_precedencia`: grava `ata.precedencia` — `P:811`, `P:819`, `P:822`. |
| **LOTE** | `carimbar_insumos_em_lote`: atribui insumos e executa `bulk_update` — `P:826`, `P:838`, `P:843`. |
| **SAVE** | `salvar_e_emitir`: `cel.save(update_fields=campos)`, compartilhado por LAV/CAR/PREC — `P:634`, `P:640`. |

`regenerar_celulas_dia` delega a REG com janela de um dia; não contém outra gravação (`P:474`). A fachada `CelulaDia.lavrar_veredito` delega a LAV (`M:1794`).

Os portões e eventos referenciados nas tabelas significam:

| Código | Fato comprovado |
|---|---|
| **PG** | GER pula `origem='editada'`; no passado só preenche DNA nulo; hoje/futuro podem ser regravados. Nenhuma consulta de exportação, tranca, `FechamentoMensal` ou versão nesse corpo — `P:555`, `P:598`, `P:600`, `P:609`. |
| **PR** | REG e REV consultam `_competencias_exportadas`; `apesar_da_lavra` dispensa essa guarda — `P:196`, `P:199`, `P:238`, `P:520`. A consulta usa `competencia_fechada_para_celula`, que combina TXT vigente com holerite do colaborador em lote publicado — `G:173`, `F:599`, `F:603`. `FechamentoMensal.status` não é esse portão — `G:132`. REG também protege origem humana — `P:226`, `P:251`; REV exige origem gerada e DNA anterior — `P:518`. | [nao verificado]
| **PJ** | Lavra/carimbo normais: **nenhum portão de competência exportada/trancada ou `FechamentoMensal`** nos escritores — `P:709`, `P:787`, `P:811`. Há idempotência por conteúdo — `P:753`, `P:795`, `P:816`. O cartório compara impressão; `forcar` permite rejulgamento — `C:1157`, `C:1161`. |
| **PS** | Exclusivamente no command `lavrar_status_competencia`: barra `competencia_entregue` — `ponto/management/commands/lavrar_status_competencia.py:40`; completa somente status nulo com carimbo atual — `C:726`, `C:729`, `C:733`. |
| **PB** | Backfill de insumos: nenhum portão de competência; seleciona células julgadas e chama LOTE — `ponto/management/commands/backfill_b5_insumos.py:60`, `:72`, `:81`. |
| **EG** | GER pelo cron 05:50 — `config/crons.py:407`; alterações de vigência/âncora chamam REG — `escala/signals.py:71`, `:117`; alteração de template — `escala/services/cadastro_tipo.py:348`; atualização de folgas — `escala/folgas.py:220`; edição do vínculo — `colaboradores/services/vinculo.py:320`. |
| **EJ** | Cartório pelo cron 06:27 — `config/crons.py:597`; vigia de hora a cada dez minutos — `config/crons.py:348`; signals de batida save/delete, ausência save/delete e justificativa save — `ponto/signals.py:132`, `:139`, `:144`, `:151`, `:162`; chamado criado/mudança de status — `chamados/signals.py:165`, `:180`. |
| **EA** | Decisão humana chama `cel.lavrar_veredito(via='admin', …)` — `ponto/services/veredito_celula.py:330`. |
| **EB** | Backfill de insumos — `ponto/management/commands/backfill_b5_insumos.py:72`, `:81`; lote de status — `ponto/management/commands/lavrar_status_competencia.py:55`. |

Na coluna **FP**, “não” significa que **o valor da coluna não é serializado diretamente no fingerprint**. A definição integral das partes está em `C:333`–`C:390`; influências indiretas por seleção de insumos aparecem depois no grafo.

| Coluna | Escritores de produção | Fonte do valor gravado | Regravação/evento | Portão | Leitores comprovados; contagem | FP |
|---|---|---|---|---|---|---|
| `colaborador` | GER `P:593`; REG `P:247`. | `ec.colaborador_id` — `P:247`, `P:594`. | Só criação nos escritores encontrados; alteração posterior `[nao verificado]`. | GER: PG; REG: PR. | **≥8**: `celulas_do_periodo` (`escala/alimentacao.py:42`); `escala_vigente` (`escala/servico_jornada.py:46`); `propostas` (`escala/services/propositor.py:488`); `montar_previa` (`folha/previa.py:51`); `dia_da_frota` (`relatorios/dia_da_frota.py:42`); `lastro.julgar` (`ponto/services/lastro.py:233`); `aplicar_precedencia_na_celula` (`C:1674`); `emitir_evento_celula` (`P:631`). | Não — `C:333`. |
| `data` | GER `P:594`; REG `P:247`. | Dia iterado na janela solicitada — `P:208`, `P:571`. | Só criação nos escritores encontrados; alteração posterior `[nao verificado]`. | GER: PG; REG: PR. | **≥8**: `celulas_do_periodo` (`escala/alimentacao.py:42`); `escala_vigente` (`escala/servico_jornada.py:46`); `propostas` (`escala/services/propositor.py:488`); `grade_da_celula` (`L:325`); `motivos_retencao_celula` (`F:1188`); `montar_previa` (`folha/previa.py:77`); `orfas_e_isencao_da_ata` (`relatorios/cartao_pela_celula.py:288`); `emitir_evento_celula` (`P:631`). | Não diretamente; seleciona janela/versão — `C:1104`, `C:1148`, `C:947`. |
| `trabalha` | GER `P:595`, `P:612`; REG `P:231`, `P:247`, `P:260`; REV `P:532`. | Cálculo EC/template — `P:243`, `P:590`; FolgaDia — `M:1257`, `M:1327`; âncora/ciclo — `M:1260`; feriado/praça — `M:1340`, `M:1362`; REV usa `dna_anterior.marcos` — `P:527`, `P:532`. | Sim: EG, regeneração explícita e reversão — `P:260`, `P:532`. | PG/PR conforme escritor. | **≥8**: `dia_sem_trabalho` (`J:408`); `eh_dia_trabalho` (`M:1072`); `grade_da_celula` (`L:345`); `classificar_falta` (`escala/servico_jornada.py:303`); `contexto_do_dia_texto` (`pautas/services.py:410`); `etiquetas_pela_celula` (`ponto/services/flip_auto.py:208`); `_veredito_do_dia` (`ponto/services/dia_do_colab.py:121`); `dia_da_frota` (`relatorios/dia_da_frota.py:47`). | Não — `C:333`. |
| `origem` | GER/REG gravam `'gerada'` — `P:248`, `P:595`; default — `M:1659`. | Literal/default, não batidas — mesmas linhas. | Escritor posterior de `'editada'` não localizado `[nao verificado]`. | Criação GER: nenhum de competência; REG: PR. | **6**: `CelulaDia.__str__` (`M:1810`); `celulas_contra_o_template.Command.handle` (`escala/management/commands/celulas_contra_o_template.py:74`); GER (`P:598`); REG (`P:226`); REV (`P:518`); `dia_do_colab` (`ponto/services/dia_do_colab.py:90`). | Não — `C:333`. |
| `escala_geradora` | GER `P:595`, `P:613`; REG `P:248`, `P:260`; REV `P:535`; declaração `SET_NULL` — `M:1661`. | EC fornecida; na reversão, `dna_anterior.vinculo_id` — `P:533`. | Sim: EG/REG/REV; deleção da EC pode nulificar pela relação — `M:1661`. | PG/PR; `SET_NULL`: nenhum portão de competência declarado no campo — `M:1661`. | **7**: `vinculo_do_dia` (`escala/alimentacao.py:238`); `contraprova_piso` (`chamados/management/commands/auditar_invariantes_chamados.py:389`); GER (`P:611`); REG (`P:228`); `regenerar_em_exportada` (`ponto/portas/regen_exportada.py:141`); `turnos_abertos_de` (`T:2316`); `dia_da_frota`, pré-carga utilizada na consulta do vínculo (`relatorios/dia_da_frota.py:35`, `:42`). | Não — `C:333`. |
| `dna` | GER `P:596`, `P:604`, `P:614`; REG `P:232`, `P:248`, `P:260`; REV `P:530`. | `montar_dna`: template/marcos/pausas/ciclo/jornada/intervalo/extra declarada — `G:76`, `G:81`, `G:95`, `G:119`; condicionado a `trabalha` — `G:79`; REV copia `dna_anterior` — `P:527`. | Sim: EG/REG/REV; GER também enriquece DNA nulo passado — `P:603`. | PG/PR. | **≥8**: `marcos_do_dia` (`M:1099`); `pausas_do_dia` (`M:1162`); `extra_declarada_do_dia` (`M:1206`); `marcos_dna_periodo` (`U:97`); `pausas_de_celulas` (`U:130`); `contexto_do_dia_texto` (`pautas/services.py:406`); `periodos_do_dia` (`ponto/juiz_batida.py:158`); `_julgar_colab_corpo` (`C:998`). | **Sim: somente `dna_versao` e `marcos`** — `C:337`. |
| `gerada_em` | `auto_now_add`, nos creates GER/REG — `M:1670`, `P:247`, `P:593`. | Relógio na criação — `M:1670`. | Não regravado pelos caminhos encontrados; comportamento declarado — `M:1675`. | Criação GER/REG conforme PG/PR. | **2**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `janelas_reabertas` (`escala/services/fase_12x36.py:190`). | Não — `C:333`. |
| `regenerada_em` | REG `P:262`; REV `P:536`. | `timezone.now()` — mesmas linhas. | Sim em regeneração alterada/reversão. O ramo de limpeza fora da vigência salva só `trabalha/dna` — `P:231`, `P:233`. | PR. | **7**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `janelas_reabertas` (`escala/services/fase_12x36.py:189`); `houve_fato_novo` (`C:295`); `dia_do_colab` (`ponto/services/dia_do_colab.py:91`); `fila_de_orfas` (`escala/services/regeneracao.py:145`); `_marcar_cobrancas_orfas` (`G:242`); `supra_juiz.Command.handle` (`ponto/management/commands/supra_juiz.py:153`). | Não — `C:333`. |
| `regeneracoes` | Default `0` — `M:1691`; REG `P:263`; REV `P:537`. | Valor anterior, ou zero, mais um — `P:263`, `P:537`. | Sim em REG/REV; GER não grava contador — `P:615`; limpeza fora da vigência também não — `P:233`. | PR. | **2**: REG (`P:263`); REV (`P:537`). | Não — `C:333`. |
| `dna_anterior` | REG `P:261`; REV limpa — `P:531`. | DNA imediatamente anterior — `P:259`; na reversão, literal `None` — `P:531`. | Sim: sobrescrito por nova regeneração; apagado na reversão — mesmas linhas. | PR. | **2**: REV (`P:518`, `P:527`); `aplicar_col900.Command.handle` (`ponto/management/commands/aplicar_col900.py:68`). | Não — `C:333`. |
| `editada_em` | Nenhum escritor de produção localizado `[nao verificado]`; o serviço registra “nenhum declarado hoje” — `escala/services/fase_12x36.py:161`. | `[nao verificado]`. | `[nao verificado]`. | `[nao verificado]`. | **3**: `marcos_do_lote` (`chamados/veredito_lavrado.py:154`); `janelas_reabertas` (`escala/services/fase_12x36.py:190`); `houve_fato_novo` (`C:295`). | Não — `C:333`. |
| `editada_por` | Nenhum escritor explícito localizado `[nao verificado]`; relação declara `SET_NULL` — `M:1701`. | Escrita humana `[nao verificado]`; nulificação por deleção declarada — `M:1702`. | Deleção do usuário: `SET_NULL` — `M:1702`; demais eventos `[nao verificado]`. | Nenhum no campo — `M:1702`; demais caminhos `[nao verificado]`. | **0 localizado** `[nao verificado]`. | Não — `C:333`. |
| `julgada_em` | CAR/SAVE — `P:800`, `P:807`, `P:640`. | Argumento `agora` do julgamento — `P:800`. | Sim por EJ quando impressão/insumos/lavra diferem; repetição idêntica retorna sem salvar — `P:795`. | PJ. | **7**: `marcos_do_lote` (`chamados/veredito_lavrado.py:154`); `janelas_reabertas` (`escala/services/fase_12x36.py:189`); CAR (`P:796`); `_julgar_colab_corpo` (`C:1000`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:109`); `backfill_b5_insumos.Command.handle` (`ponto/management/commands/backfill_b5_insumos.py:61`); `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:55`). | Não — `C:333`. |
| `impressao` | CAR/SAVE — `P:801`, `P:807`. | `impressao_insumos` — `C:998`, `C:1148`; composição detalhada abaixo — `C:333`. | Sim por EJ; comparação controla pulo — `C:1157`; `forcar` ignora pulo — `C:1161`. | PJ; versão participa do hash, não é bloqueio — `C:352`. | **3**: CAR (`P:797`); `_julgar_colab_corpo` (`C:1000`, `C:1157`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:110`). | Não é insumo do próprio hash — `C:333`. |
| `veredito` | LAV, persistido por SAVE/CAR — `P:770`, `P:775`, `P:807`. | Cartório: literal ou `derivar_veredito(codigos)` — `P:658`; códigos vêm da grade, batidas, cobertura, chamados/fio e cadastro — `C:1207`; humano fornece decisão — `P:750`, `ponto/services/veredito_celula.py:330`. | Sim por EJ/EA; cartório preserva decisão admin — `P:738`; idempotência — `P:753`. | PJ; precedência do admin é guarda de decisão, não de competência — `P:738`. | **≥8**: `_trilha_da_recusa` (`chamados/models.py:1148`); `retratar_chamados_absolvidos_por_celula` (`chamados/reconciliador.py:963`); `_absolvido_pela_celula` (`chamados/reconciliador.py:982`); `motivos_a_perguntar` (`chamados/services/fabrica_da_celula.py:86`); `grade_da_celula` (`L:365`); `motivos_retencao_celula` (`F:1215`); `montar_previa` (`folha/previa.py:94`); `dia_da_frota` (`relatorios/dia_da_frota.py:51`). | Não — `C:333`. |
| `veredito_via` | LAV — `P:771`, `P:775`. | Argumento validado: `cartorio/admin` — `P:720`, `P:771`. | Sim por EJ/EA, respeitando decisão admin — `P:738`. | PJ. | **≥8**: `veredito_que_a_lavra_escreve` (`P:656`); `_status_alvo` (`P:694`); LAV (`P:722`); `_trilha_da_recusa` (`chamados/models.py:1154`); `retratar_chamados_absolvidos_por_celula` (`chamados/reconciliador.py:964`); `_chave_memo_pv` (`J:793`); `_pergunta_viva_calc` (`J:1046`); `contexto_calendario` (`colaboradores/services/calendario.py:513`). | Não — `C:333`. |
| `veredito_em` | LAV — `P:772`, `P:775`. | `agora or timezone.now()` — `P:772`. | Sim quando muda julgamento/ata; completar só status não move essa data — `P:753`, `P:757`. | PJ; lote de status PS não move o campo — `P:757`. | **4**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `contexto_calendario` (`colaboradores/services/calendario.py:513`); `decisao_do_dia` (`ponto/services/veredito_celula.py:109`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:107`). | Não — `C:333`. |
| `protestos` | LAV — `P:769`, `P:775`. | Cartório copia códigos atuais — `P:746`, `C:1207`; admin preserva a própria coluna — `P:751`. | Sim por EJ; EA pode salvar a lista preservada — `P:751`, `P:769`. | PJ. | **6**: `motivos_retencao_celula` (`F:1228`); `nao_retem_pela_pausa` (`F:1147`); LAV (`P:722`); `_completar_status` (`C:733`); `julgar_colab` (`C:864`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:108`). | Não — `C:333`. |
| `ata` | LAV `P:741`, `P:768`; PREC `P:819`; persistência SAVE/CAR — `P:640`, `P:807`. | Grade/DNA/batidas/ausências/relógio — `C:564`, `C:610`, `C:713`; precedência — `C:1674`. LAV preserva `precedencia` anterior — `P:732`. | Sim por EJ; precedência aplicada no julgamento unitário após a lavra — `C:1721`; mesmo veredito com ata diferente também relavra — `P:735`, `P:767`. | PJ; decisão admin não impede atualizar ata — `P:738`, `P:741`. | **≥8**: `dia_sem_trabalho` (`J:407`); `_celula_furada` (`J:442`); `grade_da_celula` (`L:331`); `montar_previa` (`folha/previa.py:83`); `referencia_do_espelho` (`folha/validacao_pdf.py:47`); `periodos_do_dia` (`ponto/juiz_batida.py:157`); LAV (`P:731`); PREC (`P:816`). | Não diretamente — `C:333`; pode influenciar insumos indiretos, conforme grafo. |
| `ausencia` | CAR `P:802`; LOTE `P:838`, `P:843`; relação `SET_NULL` — `M:1764`. | `ausencia_cobre(..., para='cobranca')` — `C:981`, `C:1106`; backfill — `ponto/management/commands/backfill_b5_insumos.py:72`. | Sim por EJ/EB; deleção também pode nulificar — `M:1765`. | CAR: PJ; LOTE: PB; `SET_NULL`: nenhum declarado no campo — `M:1765`. | **4**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:75`); CAR (`P:797`); LOTE (`P:835`); `assinaturas_do_colab` (`ponto/services/esmeril_espelho.py:133`). | Não: o hash recebe **status da cobertura consultada**, não esta FK — `C:335`, `C:1106`, `C:1148`. |
| `tem_entrada` | CAR `P:803`; LOTE `P:839`, `P:843`. | `insumos_da_ata(ata,dna)`: lâmpada externa E/`dna.marcos.hi`; sem lâmpadas consulta `n_orfas` — `C:643`, `C:649`, `C:650`, `C:665`. | Sim por EJ/EB — `C:1578`, `ponto/management/commands/backfill_b5_insumos.py:73`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:80`); CAR (`P:798`); LOTE (`P:836`). | Não — `C:333`. |
| `tem_saida` | CAR `P:803`; LOTE `P:839`, `P:843`. | Mesma função, lâmpada externa S/`dna.marcos.hf` — `C:650`, `C:655`, `C:665`. | Sim por EJ/EB — `C:1578`, `ponto/management/commands/backfill_b5_insumos.py:73`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:80`); CAR (`P:798`); LOTE (`P:836`). | Não — `C:333`. |
| `insumos_em` | CAR `P:804`; LOTE `P:840`, `P:843`. | Argumento `agora`; backfill lê relógio — `P:804`, `ponto/management/commands/backfill_b5_insumos.py:59`. | Sim quando carimbo/insumos diferem; repetição idêntica não grava — `P:795`, `P:835`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:71`); CAR (`P:796`); LOTE (`P:835`). | Não — `C:333`. |
| `status_dia` | `_aplicar_status`, dentro de LAV; SAVE/CAR persistem — `P:705`, `P:743`, `P:760`, `P:773`. | `_status_alvo`: veredito/via + fatos atuais da grade — `P:694`, `P:698`; mapa admin — `S:52`, `S:64`; fatos incluem batidas, previsto/realizado, isenção, parcial e precedência humana — `S:124`, `S:143`. | Sim por EJ/EA; EB completa somente nulo/carimbo atual — `C:726`, `C:733`. | Lavra normal PJ; command PS. | **5**: `status_do_dia_lavrado` (`S:181`); `_completar_status` (`C:726`); `_status_alvo` (`P:697`); `_aplicar_status` (`P:703`); LAV (`P:723`). | Não — `C:333`. |
| `status_rotulo` | Mesmo escritor/persistência de status — `P:705`, `P:743`, `P:760`, `P:773`. | Regra de status e vocabulário; exceção de rótulo do isento — `S:28`, `S:48`, `S:60`, `S:120`. | Sim por EJ/EA/EB, no mesmo ato do status — `P:705`, `C:733`. | PJ/PS. | **3**: `status_do_dia_lavrado` (`S:182`); `_status_alvo` (`P:697`); `_aplicar_status` (`P:703`). | Não — `C:333`. |

O produtor atual emite nove chaves de topo em `ata_do_dia`; PREC acrescenta a décima, `precedencia` (`C:567`, `C:591`, `P:819`). Há oito chaves possíveis em cada item de `lampadas`, contando as opcionais (`C:501`, `C:540`, `C:424`, `C:620`, `C:714`), e seis dentro de `precedencia` (`C:1675`). Os elementos de `orfas` e `precedencia.suprimidos` são strings, sem subchaves (`C:574`, `ponto/precedencia.py:351`).

Na tabela seguinte, **LAV/CAR** significa a preparação de `ata` por LAV e sua persistência por SAVE ou CAR (`P:768`, `P:775`, `P:807`). **EJ/PJ** mantém exatamente os eventos e portões definidos acima. Todas as chaves têm **FP não diretamente**, pelo corpo de `impressao_insumos` (`C:333`–`C:390`).

| Chave de `ata` | Escritores de produção | Fonte do valor gravado | Regravação/evento | Portão | Leitores comprovados; contagem | FP |
|---|---|---|---|---|---|---|
| `tipo_dia` | `ata_do_dia` → LAV/CAR — `C:567`. | `dia.tipo_dia`; overlays FolgaDia/Ausencia/isenção na grade — `U:1109`, `U:1137`, `U:1785`. | Sim, EJ — `C:1557`. | PJ. | **3**: `dia_sem_trabalho` (`J:408`); `grade_da_celula` (`L:344`); `medir_ata_fantasma` (`ponto/management/commands/tripwire_celulas.py:342`). | Não — `C:333`. |
| `lampadas` | `lampadas_do_dia`/`ata_do_dia` → LAV/CAR — `C:565`; transformações por dispensa/teto — `C:615`, `C:712`. | Match da grade, marcos/DNA, órfãs e sinal de realizado — `C:444`, `C:456`, `C:565`. | Sim, EJ — `C:1557`. | PJ. | **≥8**: `_celulas_da_ata` (`L:93`); `n_ok_lavrado` (`L:81`); `_celula_furada

[truncado em 20000 de 37797 caracteres]

## 31/10 16:12 · sessao 16cb111707cb · - · - · origem=Ronald (tmux)

Leitores enviados nas mensagens anteriores, com até oito funções comprovadas por coluna/chave e contagens do conjunto enumerado. O total absoluto do repositório permanece `[nao verificado]`.

Correções finais:

- `ata.precedencia`: **2 leitores confirmados** — `carimbar_precedencia` (`app/ponto/portas/celula.py:816`) e `lavrar_veredito`, que preserva a chave durante a substituição da ata (`app/ponto/portas/celula.py:732`).
- `julgada_em`: **7 funções confirmadas** nas referências enviadas; `processar_cartorio` não conta como leitor direto desse campo: seleciona por colaborador/data e delega (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`).
- `regenerada_em`: retire a referência a `regenerar_celulas_vinculo:207`; aquela linha lê `data`, não `regenerada_em` (`app/ponto/portas/celula.py:207`).
- Os nomes corretos são `_chave_memo_pv` para a leitura de veredito/via (`app/chamados/juizes.py:793`) e `decisao_do_dia` para a leitura ORM de veredito/via/em (`app/ponto/services/veredito_celula.py:109`).
- `ata.marcos`: nenhuma chave produtora/leitor direto localizado `[nao verificado]`; o construtor atual emite `lampadas`, não `marcos` (`app/ponto/services/cartorio.py:567`).
- `editada_por`, `ata.lampadas[].dispensada` e os seis filhos de `ata.precedencia`: nenhum leitor individual localizado; exaustividade `[nao verificado]`.
- Os totais enviados cobrem funções confirmadas de `app/`, incluindo comandos operacionais; total semântico dos scripts de `bin/` `[nao verificado]`.

## 31/10 16:12 · sessao 758e4f350d15 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo são leitura estática; referências relativas à raiz.

| Campo | Escrita, fonte e regravação comprovadas |
|---|---|
| `julgada_em` | `carimbar`: recebe `agora`, grava `cel.julgada_em=agora`; salva por `salvar_e_emitir` (`app/ponto/portas/celula.py:800`, `:807`, `:640`). Regrava quando hash/ausência/pontas/lavra diferem; caso idêntico retorna sem salvar (`:795-799`). |
| `impressao` | `carimbar`, valor `impressao_insumos`; atribuição `app/ponto/portas/celula.py:801`; chamadas do cartório `app/ponto/services/cartorio.py:998`, `:1148`, `:1579`. |
| `ausencia` | `carimbar` e `carimbar_insumos_em_lote`, atribuições `app/ponto/portas/celula.py:802`, `:838`; `bulk_update` `:843`. Fonte `ausencia_cobre(...para='cobranca')`: `app/ponto/services/cartorio.py:981`, `:1106`; backfill `app/ponto/management/commands/backfill_b5_insumos.py:72`. |
| `tem_entrada`, `tem_saida` | `carimbar` e lote: `app/ponto/portas/celula.py:803`, `:839`, `:843`. Fonte `insumos_da_ata(ata,dna)` `app/ponto/services/cartorio.py:1578`; lê `ata.lampadas` e `dna.marcos.hi/hf`, confrontando tipo/hora/acesa; sem lâmpadas usa `n_orfas` (`:643-665`). |
| `insumos_em` | `carimbar` e lote; relógio `agora` (`app/ponto/portas/celula.py:804`, `:840`). |
| `veredito` | `lavrar_veredito` atribui/salva (`app/ponto/portas/celula.py:770`, `:775`). Cartório usa literal ou `derivar_veredito(codigos)` (`:647-664`); fonte normal classificação da grade, cobertura, batidas, chamados/fio, cadastro e relógio (`app/ponto/services/cartorio.py:1204-1217`, `:1244-1245`). Admin prevalece sobre cartório (`app/ponto/portas/celula.py:738-744`). |
| `veredito_via` | `lavrar_veredito`, parâmetro `via` (`app/ponto/portas/celula.py:771`, `:775`); validado contra vias declaradas (`:720-721`). |
| `veredito_em` | `lavrar_veredito`, `agora or timezone.now()` (`app/ponto/portas/celula.py:772`). Atualização exclusiva do status não o move (`:753-766`). |
| `protestos` | `lavrar_veredito`, cartório copia `codigos`, admin preserva lista existente (`app/ponto/portas/celula.py:746`, `:751`, `:769`). |
| `ata` | `lavrar_veredito` (`app/ponto/portas/celula.py:741`, `:768`, `:775`) e `carimbar_precedencia` (`:819-822`). Lavra preserva somente chave de outra operação `precedencia` (`:94`, `:730-734`). |
| `status_dia`, `status_rotulo` | `_aplicar_status` atribui ambos (`app/ponto/portas/celula.py:701-706`), persistidos por `lavrar_veredito` (`:743`, `:760`, `:773-775`). Fonte `_status_alvo`: decisão admin ou `status_da_lavra(veredito=v, **fatos)` (`:686-698`). Fatos da grade: tipo, previsto, batidas casadas/órfãs/realizado, isenção e parcial; para `indefinida`, precedência de FolgaDia/feriado (`app/ponto/status_dia.py:124-149`). Regras/rotulagem `:60-121`. |

Portões e gatilhos:

- Cartório normal: fila de **todas** as células passadas; `processar_cartorio.Command.handle` não recorta exportação/tranca/FechamentoMensal (`app/ponto/management/commands/processar_cartorio.py:49-65`). Funções de persistência acima tampouco consultam esses portões (`app/ponto/portas/celula.py:709-843`).
- `REGRA_VERSOES=((1,'2026-09'),(2,'2026-10'))` (`app/ponto/services/cartorio.py:110`). `versao_para` usa `janela_atual` e maior versão cujo horizonte alcança competência (`:191-197`): entra no hash (`:352-353`), não impede escrita. `forcar` ignora igualdade da impressão (`:1161-1163`).
- `lavrar_status_competencia` **barra entregue** por `competencia_entregue` (`app/ponto/management/commands/lavrar_status_competencia.py:40-42`). Executa `julgar_colab(completar_status=True)` (`:55-56`); só completa status NULL com carimbo atual, lendo `veredito/protestos` existentes (`app/ponto/services/cartorio.py:726-735`). Não confundir essa guarda com a lavra cotidiana.
- Backfill insumos seleciona apenas `julgada_em NOT NULL`, não tem corte competência; lê ausência/ata/dna e chama lote (`app/ponto/management/commands/backfill_b5_insumos.py:60-84`).
- Cron real `processar_cartorio --apply`: **06:27**, dependente de `reconciliar_grade` (`app/config/crons.py:597`); comentários do serviço citando 06:28 não são o agendamento atual. Vigia de hora: `*/10 * * * *`, `--apply` (`:348`).
- Signals: batida save/delete (`app/ponto/signals.py:132-140`); Ausencia save/delete (`:144-152`); Justificativa save (`:162-165`); todos chegam a `julgar_celula` (`:85-93`). Chamado criado ou com mudança `status_local`: `app/chamados/signals.py:165-180`.
- `julgar_celula` barra futuro/inexistente e, salvo teto, espera turno de hoje (`app/ponto/services/cartorio.py:1700-1713`); aplica precedência após julgar (`:1721-1723`).

`ata` atual tem **10 chaves de topo**: nove produzidas por `ata_do_dia`, mais `precedencia`. `ata.marcos` não aparece nos produtores atuais abaixo; `marcos` pertence a `dna.marcos` ou à grade (`app/ponto/services/cartorio.py:444-449`, `:932`).

| Chave de ata | Produtor/fonte |
|---|---|
| `tipo_dia` | `ata_do_dia`: `dia.tipo_dia` (`app/ponto/services/cartorio.py:567`). |
| `lampadas` | `ata_do_dia` chama `lampadas_do_dia` com células da grade, marcos, DNA, órfãs e sinal de realizado (`:564-567`). |
| `minutos_previstos` | Inteiro de `dia.minutos_previstos` (`:568`); grade calcula segmentos previstos E→S (`app/escala/utils.py:1578-1582`); intermitente pode copiar realizado (`:2008-2010`). |
| `minutos_realizados` | Inteiro de `dia.minutos_realizados` (`app/ponto/services/cartorio.py:569`); grade usa `realizado_do_dia` e zero explícito quando sem turno (`app/escala/utils.py:1985-1998`). |
| `sem_ancora` | Booleano do dia da grade (`app/ponto/services/cartorio.py:570`). |
| `n_orfas` | `len(dia.orfas)` (`:571`). |
| `orfas[]` | Horas convertidas em `str`; elementos sem subchaves (`:574`). |
| `n_missing` | Contagem `lampadas[].acesa is False`, ou missing da grade (`:575-576`); recalculada após dispensa (`:622`) e teto (`:716`). |
| `n_celulas` | `len(lampadas)` (`:591`). |
| `lampadas[].tipo` | Papel do marco do DNA/grade (`:449`, `:501`, `:540`, `:551`). |
| `lampadas[].hora` | Hora prevista do DNA/grade (`:501`, `:540`, `:551`). |
| `lampadas[].acesa` | Match da grade, proximidade quando marco ausente da régua, ou desconhecido (`:495-510`, `:540-553`); ausência aprovada integral/justificativa pode tornar True (`:610-620`); teto torna None (`:713-714`). |
| `lampadas[].luz` | Hora real casada, ou None (`:501-507`, `:541`, `:552`); dispensa zera luz (`:619`). |
| `lampadas[].tipo_real` | Tipo real fornecido pela grade; None quando ausente/proximidade sem tipo (`:503-507`, `:542`, `:553`). |
| `lampadas[].isencao` | Opcional True quando célula da grade marca isenção (`:424-425`, `:543`, `:554`). |
| `lampadas[].dispensada` | Opcional tipo da Ausencia integral aprovada, ou `'justificativa'` (`:610-620`). |
| `lampadas[].pendente` | Opcional True quando marco ainda não venceu (`:713-714`). |
| `precedencia` | `aplicar_precedencia_na_celula` consulta `fatos_do_dia` e chama `carimbar_precedencia` (`:1673-1682`); grava `app/ponto/portas/celula.py:819-822`. |
| `precedencia.tipo_do_dia` | Vencedor dos fatos (`app/ponto/services/cartorio.py:1676`; `app/ponto/precedencia.py:349-356`). |
| `precedencia.faixa` | Faixa do vencedor (`app/ponto/services/cartorio.py:1677`; `app/ponto/precedencia.py:350`, `:357`). |
| `precedencia.rotulo` | `ROTULOS`/tipo capitalizado (`app/ponto/services/cartorio.py:1678`; `app/ponto/precedencia.py:358`). |
| `precedencia.suprimidos[]` | Lista de **strings**, faixas perdedoras únicas (`app/ponto/services/cartorio.py:1679`; `app/ponto/precedencia.py:351-354`). Não contém dicionários/subchaves; vocabulário `PRECEDENCIA` (`:46`). |
| `precedencia.era_dia_previsto` | `trabalha` da célula, ou cálculo EC quando desconhecido (`app/ponto/services/cartorio.py:1680`; `app/ponto/precedencia.py:305-327`). |
| `precedencia.texto` | `texto_da_supressao(f)`; deriva vencedor e suprimidos (`app/ponto/services/cartorio.py:1681`; `app/ponto/precedencia.py:375-380`). |

Fontes adicionais da ata:

- Grade carrega EC/template, células da janela e FolgaDia (`app/escala/utils.py:1240-1260`, `:1356-1376`), batidas (`:1267`), aplica overlays FolgaDia/Ausencia/isenção (`:1785-1790`).
- FolgaDia altera tipo, limpa células/órfãs (`:1109-1120`); Ausencia vem de cobertura para folha, Justificativa aprovada do banco (`:1137-1159`, `:1170-1203`).
- Precedência consulta Ausencia não rejeitada, FolgaDia, feriados da praça e vínculos/células (`app/ponto/precedencia.py:227-245`, `:271-307`, `:332-360`).
- Não encontrei caminho comprovado de `AplicacaoConvencao`/régua CCT à produção da ata/status; atribuir essa fonte: `[nao verificado]`. A “régua” do veto do fingerprint vem de template degenerado/suspeito (`app/ponto/services/furos_vetados.py:33-43`).

Fingerprint:

`sha256('|'.join(partes).encode()).hexdigest()[:64]` (`app/ponto/services/cartorio.py:390`).

Partes obrigatórias: batidas `pk:tipo:timestamp.isoformat`, cobertura `str(status)`, chamados `pk:status_local`, **somente** `dna.dna_versao` concatenado com `dna.marcos` (`:333-338`). Opcionais: veto (`:339-340`), teto de marcos vencidos (`:344-345`), versão (`:352-353`), turno em curso `enc` (`:362-363`), quantidade de fios vivos (`:377-378`), parcial e estado de período (`:385-389`). Chamador normal fornece esses fatos (`:1104-1153`); nunca-bateu usa batidas/chamados vazios, cobertura, DNA, versão e parcial (`:998-999`). **Ata e demais colunas não são partes diretas** nesse corpo (`:333-389`).

Arestas principais, origem → destino:

- `dna → impressao`: `app/ponto/services/cartorio.py:337`.
- `data → impressao`: versão por competência e teto/turno (`:945-947`, `:1104-1153`).
- `ata.lampadas`, `ata.n_orfas`, `dna.marcos → tem_entrada/tem_saida`: `:643-665`.
- `veredito`, `veredito_via → status_dia/status_rotulo`: `app/ponto/portas/celula.py:694-698`; regra/rotulação `app/ponto/status_dia.py:64-121`.
- `protestos → veredito/status` no completar-status: `app/ponto/services/cartorio.py:733-735`.
- `trabalha → veredito`: `_protesta` `:1204-1208`; cadastro sem vínculo lê trabalha (`app/ponto/supra_juiz.py:170-174`).
- `trabalha → ata.precedencia.era_dia_previsto → demais precedencia`: `app/ponto/precedencia.py:305-340`, `:349-360`.
- `ata.precedencia anterior → ata nova`: preservação `app/ponto/portas/celula.py:730-734`.
- `ata.lampadas → ata.n_missing/n_celulas`: `app/ponto/services/cartorio.py:575`, `:591`.
- `ata.lampadas atuais → veredito`: `app/ponto/services/cartorio.py:1182-1208`, `:1244-1245`.

**Há ciclo temporal comprovado `ata anterior → ata nova`:**

1. Grade prévia chama `turnos_do_colab` alimentando células persistidas (`app/escala/utils.py:1519-1526`).
2. `turnos_do_colab` chama `papel_por_minuto_da_ata` (`app/ponto/turnos.py:1911`), que consulta `periodos_do_dia(cel,te)` (`:1629-1636`); juiz lê `ata/dna` (`app/ponto/juiz_batida.py:157-161`).
3. Grade passa turno ao matcher (`app/escala/utils.py:1643-1645`); matcher usa papéis para construir células (`:562-585`).
4. Cartório transforma células em ata persistida (`app/ponto/services/cartorio.py:564-567`, `:1557-1580`).

Também há dependência de **outra célula**: as luzes de todas são acumuladas e impedem tratar a luz vizinha como órfã (`app/ponto/turnos.py:1652`, `:1733-1735`); reconciliação pode retirar órfã de D+1 ao preencher saída de D (`app/escala/utils.py:1911-1913`, `:1936`, `:1979-1980`). Para calcular realizado, a grade já substitui atas por cópias com lâmpadas atuais em memória (`:1821-1838`), mas isso não remove a leitura anterior usada no pré-alinhamento (`:1519-1526`).

## 31/10 16:10 · sessao c2253669ab73 · - · - · origem=Ronald (tmux)

Escritores concretos encontrados, excluindo testes/migrations: [nao verificado]

| Coluna | Escritores e fontes provados |
|---|---|
| colaborador | `regenerar_celulas_vinculo`: `create(colaborador_id=ec.colaborador_id)` em `app/ponto/portas/celula.py:247`; `gerar_celulas_periodo`: `create` em `app/ponto/portas/celula.py:593`. |
| data | Mesmos `create`, data iterada na janela: `app/ponto/portas/celula.py:208`, `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:571`, `app/ponto/portas/celula.py:594`. |
| trabalha | Regenerador, inclusive limpeza fora da vigência: `app/ponto/portas/celula.py:231`, `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:260`; gerador: `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:612`; reversor deriva de `dna_anterior.marcos`: `app/ponto/portas/celula.py:527`, `app/ponto/portas/celula.py:532`. |
| origem | `'gerada'` nos dois `create`: `app/ponto/portas/celula.py:248`, `app/ponto/portas/celula.py:595`; default em `app/escala/models.py:1659`. Escritor de `'editada'` não encontrado `[nao verificado]`. |
| escala_geradora | Regenerador: `app/ponto/portas/celula.py:248`, `app/ponto/portas/celula.py:260`; gerador: `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:613`; reversor usa `dna_anterior.vinculo_id`: `app/ponto/portas/celula.py:533`. Há `on_delete=SET_NULL`: `app/escala/models.py:1660`. |
| dna | Regenerador: `app/ponto/portas/celula.py:232`, `app/ponto/portas/celula.py:248`, `app/ponto/portas/celula.py:260`; gerador: `app/ponto/portas/celula.py:596`, `app/ponto/portas/celula.py:604`, `app/ponto/portas/celula.py:614`; reversor copia `dna_anterior`: `app/ponto/portas/celula.py:527`, `app/ponto/portas/celula.py:530`. |
| gerada_em | `auto_now_add` em `app/escala/models.py:1670`, acionado pelos `create` acima. |
| regenerada_em | Relógio em regeneração: `app/ponto/portas/celula.py:262`; relógio em reversão: `app/ponto/portas/celula.py:536`. |
| regeneracoes | Valor anterior +1 na regeneração: `app/ponto/portas/celula.py:263`; reversão: `app/ponto/portas/celula.py:537`. Default 0: `app/escala/models.py:1691`. |
| dna_anterior | DNA anterior à regeneração: `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:261`; limpo pelo reversor: `app/ponto/portas/celula.py:531`. |
| editada_em | Nenhum escritor encontrado `[nao verificado]`; o próprio serviço declara “escritor de producao: nenhum declarado hoje” em `app/escala/services/fase_12x36.py:161`. |
| editada_por | Nenhum escritor explícito encontrado `[nao verificado]`; deleção do usuário aplica `SET_NULL`: `app/escala/models.py:1701`. |
| julgada_em | `carimbar`: argumento `agora`, `app/ponto/portas/celula.py:800`, persistido via `salvar_e_emitir` em `app/ponto/portas/celula.py:807` → `app/ponto/portas/celula.py:640`. |
| impressao | `carimbar`: argumento `impressao`, `app/ponto/portas/celula.py:801`, mesmo save acima. |
| veredito | `lavrar_veredito`: `app/ponto/portas/celula.py:770`. Cartório deriva `veredito or derivar_veredito(codigos)` em `app/ponto/portas/celula.py:658`; admin fornece `veredito` em `app/ponto/portas/celula.py:750`. |
| veredito_via | `lavrar_veredito`: argumento via em `app/ponto/portas/celula.py:771`. |
| veredito_em | `lavrar_veredito`: argumento agora ou relógio em `app/ponto/portas/celula.py:772`. |
| protestos | `lavrar_veredito`: cópia dos códigos do cartório em `app/ponto/portas/celula.py:746`, ou preservação dos protestos anteriores no admin em `app/ponto/portas/celula.py:751`; atribuição em `app/ponto/portas/celula.py:769`. |
| ata | `lavrar_veredito`: copia argumento e preserva `precedencia` anterior se ausente no novo argumento em `app/ponto/portas/celula.py:730`; atribui em `app/ponto/portas/celula.py:741` ou `app/ponto/portas/celula.py:768`; `carimbar_precedencia` modifica chave em `app/ponto/portas/celula.py:819` e salva em `app/ponto/portas/celula.py:822`. |
| ausencia | `carimbar` argumento ausência em `app/ponto/portas/celula.py:802`; `carimbar_insumos_em_lote` em `app/ponto/portas/celula.py:838` + `bulk_update` em `app/ponto/portas/celula.py:843`. Deleção da ausência aplica `SET_NULL`: `app/escala/models.py:1764`. |
| tem_entrada | `carimbar`: argumento em `app/ponto/portas/celula.py:803`; lote em `app/ponto/portas/celula.py:839`, `app/ponto/portas/celula.py:843`. |
| tem_saida | Idem tem_entrada: `app/ponto/portas/celula.py:803`, `app/ponto/portas/celula.py:839`, `app/ponto/portas/celula.py:843`. |
| insumos_em | `carimbar`: agora em `app/ponto/portas/celula.py:804`; lote: agora em `app/ponto/portas/celula.py:840`, `app/ponto/portas/celula.py:843`. |
| status_dia | `_aplicar_status` em `app/ponto/portas/celula.py:705`, chamado por `lavrar_veredito` e salvo em `app/ponto/portas/celula.py:743`, `app/ponto/portas/celula.py:760`, `app/ponto/portas/celula.py:775`. |
| status_rotulo | Mesmo escritor e saves de status_dia: `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:743`, `app/ponto/portas/celula.py:760`, `app/ponto/portas/celula.py:775`. |

Fontes e eventos:

- `trabalha` vem de `EscalaColaborador.eh_dia_trabalho_calculado`, chamado pelo regenerador em `app/ponto/portas/celula.py:243` e gerador em `app/ponto/portas/celula.py:590`. A regra se declara sem leitura de célula em `app/escala/models.py:1226`; lê FolgaDia em `app/escala/models.py:1257`, âncora/ciclo em `app/escala/models.py:1260`, a foto em `app/escala/models.py:1280`, continuação da foto em `app/escala/models.py:1327`, regra do template em `app/escala/models.py:1337` e feriado por praça em `app/escala/models.py:1340`, `app/escala/models.py:1362`.
- `montar_dna` lê template, marcos/pausas em `app/ponto/management/commands/gerar_celulas.py:76`, `app/ponto/management/commands/gerar_celulas.py:81`; produz `dna_versao`, `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `marcos`, `minutos_jornada`, `intervalo_livre`, `lampadas` em `app/ponto/management/commands/gerar_celulas.py:95`; extra declarada em `app/ponto/management/commands/gerar_celulas.py:119`.
- Gerador cron 05:50 em `app/config/crons.py:407`; o command chama o escritor em `app/ponto/management/commands/gerar_celulas.py:311`.
- Vigência alterada provoca regeneração em `app/escala/signals.py:34`, `app/escala/signals.py:71`; âncora alterada em `app/escala/signals.py:79`, `app/escala/signals.py:117`; foto FolgaDia em `app/escala/folgas.py:192`, `app/escala/folgas.py:220`; vínculo humano em `app/colaboradores/services/vinculo.py:320`; alteração template em `app/escala/services/cadastro_tipo.py:348`.
- Gerador normal respeita origem editada e passado, mas enriquece DNA nulo passado em `app/ponto/portas/celula.py:598`–`app/ponto/portas/celula.py:605`; regrava futuro em `app/ponto/portas/celula.py:609`–`app/ponto/portas/celula.py:615`. Não há consulta de competência exportada nesse corpo: `app/ponto/portas/celula.py:555`–`app/ponto/portas/celula.py:617`.
- Regenerador usa `_competencias_exportadas` em `app/ponto/portas/celula.py:199`, barra em `app/ponto/portas/celula.py:238`, mas `apesar_da_lavra` ignora essa guarda em `app/ponto/portas/celula.py:196`. Reversor tem mesma guarda em `app/ponto/portas/celula.py:520`. `_competencias_exportadas` chama `competencia_fechada_para_celula` em `app/ponto/management/commands/gerar_celulas.py:173`; este combina TXT vigente (`competencia_entregue`) e holerite próprio com lote publicado em `app/folha/export.py:599`–`app/folha/export.py:606`. Não usa FechamentoMensal.status, conforme documentação em `app/ponto/management/commands/gerar_celulas.py:132`.
- Lavra humana do veredito em `app/ponto/services/veredito_celula.py:330`; ela não escreve origem/editada_em/editada_por (chamada explícita somente de lavrar_veredito nessa linha).
- Lote backfill: ausência de `ausencia_cobre`, flags de `insumos_da_ata(cel.ata, cel.dna)` em `app/ponto/management/commands/backfill_b5_insumos.py:72`; chama carimbo lote em `app/ponto/management/commands/backfill_b5_insumos.py:81`. Nenhum portão de competência no corpo `app/ponto/management/commands/backfill_b5_insumos.py:51`–`app/ponto/management/commands/backfill_b5_insumos.py:84`.
- Status deriva do veredito/via em `app/ponto/portas/celula.py:694`–`app/ponto/portas/celula.py:698`. Admin usa mapa em `app/ponto/status_dia.py:52`, aplicado em `app/ponto/status_dia.py:64`. Cartório usa fatos da grade/batidas/realizado/isento/cobertura parcial/precedência em `app/ponto/status_dia.py:124`–`app/ponto/status_dia.py:149`, regra em `app/ponto/status_dia.py:96`–`app/ponto/status_dia.py:121`.
- `lavrar_status_competencia` barra competência entregue em `app/ponto/management/commands/lavrar_status_competencia.py:40`; chama `julgar_colab(completar_status=True)` em `app/ponto/management/commands/lavrar_status_competencia.py:55`.

Arestas especialmente relevantes:

- `dna → dna_anterior` na regeneração: `app/ponto/portas/celula.py:259`–`app/ponto/portas/celula.py:261`.
- `dna_anterior → dna`, `dna_anterior → trabalha`, `dna_anterior → escala_geradora` na reversão: `app/ponto/portas/celula.py:527`–`app/ponto/portas/celula.py:535`.
- Portanto há ciclo operacional `dna ↔ dna_anterior` se o grafo inclui eventos sucessivos de regeneração/reversão; a reversão zera dna_anterior em `app/ponto/portas/celula.py:531`, evitando ping-pong conforme documentação em `app/ponto/portas/celula.py:497`.
- `regeneracoes → regeneracoes` incremento: `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:537`.
- `protestos → protestos` preservação pelo admin: `app/ponto/portas/celula.py:751`.
- `ata.precedencia → ata.precedencia` preservação por lavra: `app/ponto/portas/celula.py:732`.
- `veredito/veredito_via → status_dia/status_rotulo`: `app/ponto/portas/celula.py:694`–`app/ponto/portas/celula.py:698`.

## 31/10 16:09 · sessao 12e38d744b15 · GRADE-FOLGA-DO-VINCULO · leitura · origem=Code

**O horizonte da versão 3 controla a impressão que provoca o rejulgamento; ele não seleciona uma implementação antiga da regra de folga.** Quando um dia anterior ao horizonte é rejulgado, o cartório usa a grade com a regra nova. O filtro que recupera a folga da ata antiga pertence à leitura do espelho e não ao caminho do cartório. (`app/ponto/services/cartorio.py:948`, `:962–965`, `:1165–1178`; `app/escala/utils.py:1163–1171`, `:2329–2330`.)

Todas as referências abaixo são de **ea38de71**, salvo indicação **B/c7ea207a**. A reprodução das contagens e a atribuição causal de cada uma das 16 células anteriores a 21/09 ficam **[nao verificado]**.

**1. Caminho que decide `ata.tipo_dia`**

O caminho é:

1. O comando carrega **todas as células passadas** do colaborador e chama `julgar_colab`, repassando `--forcar`. (`app/ponto/management/commands/processar_cartorio.py:49–65`.)
2. O serviço chama diretamente `montar_grade_prevista_periodo`. O dispatcher escolhe o builder por turno ou o legacy. (`app/ponto/services/cartorio.py:948`; `app/escala/utils.py:2076–2082`.)
3. No builder por turno, a grade recebe o tipo inicial `folga` ou `trabalho`, depois os overlays de folga lançada, ausência e isenção. (`app/escala/utils.py:1794–1804`, `:1837–1842`.)
4. `ata_do_dia` copia literalmente `dia.get('tipo_dia')`. O cartório entrega essa ata à lavra e persiste o resultado pelo carimbo. **Não é `classificar_dia` que escolhe esse campo da ata.** (`app/ponto/services/cartorio.py:584`, `:1574–1581`, `:1595–1597`.)

**Esse caminho usa a regra nova.** Em B, o overlay buscava qualquer `FolgaDia` do vínculo recebido e era repetido para cada vínculo da janela. Em C, carrega todas as folgas da pessoa e aceita a primeira válida para o vínculo daquele dia: manual vale independentemente do vínculo; calendário exige o vínculo do dia. (**B:** `app/escala/utils.py:1109–1120`, `:1785–1788`; **C:** `app/escala/utils.py:1119–1125`; `app/ponto/precedencia.py:207–219`, `:236–240`.)

**Não há consulta a `versao_para`, `VERSAO_GRADE_FOLGA` ou `REGRA_VERSOES` para escolher a regra de folga nesse overlay.** A versão é calculada separadamente para compor a impressão. O teste `< VERSAO_GRADE_FOLGA` ocorre em `_ata_abaixo_da_regra_de_folga`, chamado por `grade_espelho_janela`, que o cartório não usa. (`app/escala/utils.py:1114–1125`, `:1163–1171`, `:2330`; `app/ponto/services/cartorio.py:948`, `:962–965`.)

A alteração de `colaboradores/services/calendario.py` pertence à apresentação do calendário, incluindo `eh_trabalho`; ela não integra esse caminho de construção da ata. (`app/colaboradores/services/calendario.py:93–105`, `:617–623`, `:838–840`; `app/ponto/services/cartorio.py:948`.) `escala/folgas.py` já preserva a folga de calendário no vínculo original. (`app/escala/folgas.py:405–406`.) Sua ausência de alteração entre os commits foi conferida pelo diff Git, mas não tem prova isolada em arquivo:linha: **[nao verificado]** nesse formato.

**2. Com e sem `--forcar`**

**Com `--forcar`, sim: dias com `versao_para < 3` são rejulgados.** A flag desativa o pulo por impressão igual, tanto no ramo normal quanto no ramo sem nenhuma batida na vida; não existe condição de versão nessa decisão. (`app/ponto/services/cartorio.py:1017–1023`, `:1174–1183`.)

Rejulgar não garante alteração dos valores. Mesmo uma decisão administrativa preserva o veredito contra o cartório, mas permite atualizar ata e status. (`app/ponto/portas/celula.py:735–745`.)

**Sem `--forcar`, o cron declarado executa `--apply`.** No ramo normal, uma célula é pulada somente quando tem `julgada_em`, impressão igual, veredito preenchido e veredito diferente de `nunca_bateu`. Logo, impressão divergente, ausência de julgamento/veredito ou `nunca_bateu` após adesão permitem novo julgamento, inclusive na competência 2026-09. (`app/config/crons.py:597`; `app/ponto/services/cartorio.py:1171–1183`.)

Na competência 2026-09, `versao_para` continua **1**: a função escolhe a maior versão cujo horizonte alcança a competência, e as versões 2 e 3 começam em 2026-10. Isso mantém apenas o componente `regra:1`; não congela os demais componentes. (`app/ponto/services/cartorio.py:110`, `:208–214`, `:369–370`.)

A impressão contém:

| Componente | Conteúdo |
|---|---|
| Batidas | `pk`, tipo e timestamp das batidas apuráveis de D−1 a D+1 |
| Cobertura | Status da ausência para cobrança |
| Chamados | `pk` e `status_local` dos chamados de módulos de batida |
| DNA | `dna_versao` e `marcos` |
| Condicionais | Veto, teto de marcos vencidos, versão da regra, turno em curso, quantidade de chamados vivos com fio e estado da cobertura parcial |

Fontes: `app/ponto/services/cartorio.py:350–407`, `:943–945`, `:1122–1125`, `:1151–1170`; seleção dos chamados em `:90–105`.

**`tipo_dia`, a ata inteira e `status_dia` não são componentes diretos do hash.** Há, porém, componentes derivados da grade, especialmente `periodo_parcial`. (`app/ponto/services/cartorio.py:350–407`, `:1170`.)

Mesmo sem flag no cron, há segunda passada interna com `forcar=True` para células reconciliadas. (`app/ponto/services/cartorio.py:803–818`.)

**3. A impressão abaixo da versão 3 pode mudar?**

**Sim, pelo componente `cobparcial`, mesmo mantendo `regra:1` e o mesmo `tipo_dia` final.** O mecanismo comprovado é:

- O overlay de folga limpa células e órfãs. (`app/escala/utils.py:1132–1134`.)
- A ausência parcial mantém essas listas e calcula `periodo_parcial` pelas horas disponíveis. (`app/escala/utils.py:1231–1248`.)
- Essas horas vêm das células cumpridas e órfãs; havendo alguma dentro do período declarado, o resultado é `dentro`; sem nenhuma, `fora`. (`app/ponto/turnos.py:2611–2614`, `:2640–2647`.)
- O cartório incorpora esse estado como `cobparcial:1:<estado>`, sem filtro por versão. (`app/ponto/services/cartorio.py:402–407`, `:1170`.)

Portanto, retirar uma folga inválida pode preservar batidas, mudar `fora→dentro` e alterar a impressão, enquanto B e C terminam com `tipo_dia='ausencia'`. Essa é uma consequência do código; sua ocorrência nas células informadas está **[nao verificado]**. (`app/escala/utils.py:1124–1125`, `:1231–1245`.)

Também mudou a ordem dos overlays: B fazia folga→ausência **por vínculo**; C aplica a folga uma vez, antes de todas as ausências. Isso pode alterar células e órfãs sem exigir mudança do tipo final. (**B:** `app/escala/utils.py:1785–1788`; **C:** `app/escala/utils.py:1837–1840`.)

**4. Consumidores da competência 2026-09 exportada**

**Folha e cálculo online.** `grade_da_celula` prefere `ata.tipo_dia` ao campo `trabalha`, e entrega também minutos, lâmpadas, órfãs e veredito. A folha usa esse leitor na janela da competência; `trabalho` e `ausencia` entram no universo de dias previstos. Assim, `folga→trabalho` pode mudar previsto, realizado considerado, cobertura e pendências. O efeito numérico dessas células está **[nao verificado]**. (`app/escala/services/leitor_celula.py:325–365`; `app/folha/export.py:318–327`; `app/ponto/services/fechamento.py:1014–1035`.)

A competência exportada **não congela essa leitura online**: `fechamento_lido` recalcula em memória; a guarda de escrita depende da lista de exportadas, que não é carregada no modo de leitura puro. Os totais de tela/cartão usam essa leitura. (`app/ponto/services/fechamento.py:523–548`, `:1727–1729`; `app/relatorios/cartao_pela_celula.py:69–76`.)

**Espelho.** O caminho principal lê a grade da ata e deriva dela o indicador de folga. Uma ata alterada pode retirar esse indicador; o status visual depende também de ausência, inconsistência, batidas, isenção e previsto. Há fallback para o builder em atas agregadas sem marcos. (`app/ponto/services/espelho.py:529–548`, `:690`, `:152–171`.) No fallback, o filtro abaixo da versão 3 só restaura folga **se a ata ainda disser folga**; uma ata já alterada para trabalho não é recuperada por ele. (`app/escala/utils.py:1160–1171`.)

**Novo export.** Os eventos monetários são montados a partir dos fechamentos, mas a retenção consulta os vereditos e protestos atuais das células. Portanto, mudar ata não prova mudança imediata das quantias; mudar veredito/protestos pode alterar quem entra em um novo TXT. (`app/folha/export.py:695–725`, `:942–946`, `:1188`, `:1215–1238`.)

**Export já gravado.** O download devolve `reg.conteudo`; o PDF do TXT traduz esse registro salvo. Esses downloads não são regenerados a partir da ata atual. (`app/folha/views.py:548–563`, `:818–834`.)

**`CelulaDia.status_dia` é distinto do status visual do espelho.** Existe leitor explícito do campo persistido, mas os caminhos acima calculam suas apresentações pelas fontes descritas. A ausência universal de leitores indiretos ou dinâmicos fica **[nao verificado]**. (`app/ponto/status_dia.py:171–185`; `app/ponto/services/espelho.py:152–183`.)

**5. Outra mudança capaz de explicar os dias anteriores a 21/09**

Na inspeção do diff, não encontrei alteração independente da correção de folga no classificador, hash, reconciliação ou lavra. Esse inventário negativo foi conferido pelo Git, mas não possui prova isolada por arquivo:linha: **[nao verificado]** nesse formato.

Há efeitos da própria correção além de `tipo_dia`: a precedência passou de guardar somente a primeira folga por data e eventualmente descartá-la para guardar todas e escolher a primeira válida; além disso, há a mudança de ordem dos overlays e o efeito sobre cobertura parcial descritos acima. (**B:** `app/ponto/precedencia.py:234–238`, `:313–315`; **C:** `app/ponto/precedencia.py:207–219`, `:358`; `app/escala/utils.py:1837–1840`, `:1245`.)

**B e C sucessivos no mesmo banco também não isolam apenas a troca de código:** o julgamento lê chamados/fios atuais, pode fechar chamados e criar perguntas; há segunda passada e releitura após emissão. Assim, parte das diferenças pode decorrer do estado deixado pela rodada B. Atribuir esse efeito às oito células restantes exige comparar seus insumos e chamados/perguntas entre as fases: **[nao verificado]**. (`app/ponto/services/cartorio.py:1086`, `:1151–1156`, `:1298–1305`, `:840–845`, `:814–818`, `:870–883`.)

## 31/10 16:08 · sessao d2e4aeb966ef · - · - · origem=Ronald (tmux)

Q5: a inspeção de `git diff c7ea207a..ea38de71` encontrou quatro módulos de produção alterados: `calendario.py`, `escala/utils.py`, `precedencia.py` e `cartorio.py`; não apareceu alteração independente em classificação, serialização de chamados, reconciliação ou status. O inventário completo do diff não possui um `arquivo:linha` próprio: [nao verificado por esse formato de referência].

Há, porém, efeitos além da troca explícita de `tipo_dia`:

- **Precedência com múltiplas folgas:** B guardava somente a primeira `FolgaDia` por data e depois podia descartá-la (`c7ea207a:app/ponto/precedencia.py:234`, `:313`). C conserva todas e escolhe a primeira válida (`app/ponto/precedencia.py:207`, `:217`, `:358`). Isso pode mudar a precedência mesmo quando o tipo final da grade permanece igual; a precedência determina os fatos vencedores em `app/ponto/precedencia.py:380` e `:392`.
- **Ausência parcial com mesmo `tipo_dia='ausencia'`:** aplicar folga limpa células e órfãs (`app/escala/utils.py:1132`). A ausência parcial preserva essas listas e calcula `periodo_parcial` pelas horas disponíveis (`app/escala/utils.py:1231`, `:1241`, `:1245`). Retirar uma folga inválida pode preservar batidas e alterar `fora`/`dentro`, sem trocar o tipo final; as horas vêm das células e órfãs (`app/ponto/turnos.py:2640`), e a decisão do período está em `app/ponto/turnos.py:2611`. Esse estado entra na impressão (`app/ponto/services/cartorio.py:1170`, `:402`).
- **Estado deixado pela fase B:** o julgamento lê chamados e perguntas atuais (`app/ponto/services/cartorio.py:1086`, `:1151`, `:1224`), pode reconciliar chamados (`:1298`, `:1332`) e criar perguntas (`:840`). Existe segunda passada forçada (`:814`) e releitura após emissão (`:870`), mas sobras posteriores são descartadas com contador (`:823`). Assim, diferenças B→C podem conter efeitos do estado já modificado por B. Atribuir isso aos oito dias concretos é [nao verificado].

O novo `trabalho_do_dia` pertence à apresentação do calendário (`app/colaboradores/services/calendario.py:93`, `:838`). O gate que recupera a ata antiga pertence a `grade_espelho_janela` (`app/escala/utils.py:1163`, `:1170`, `:2330`); o cartório chama diretamente o builder (`app/ponto/services/cartorio.py:948`).

O cron declarado executa `processar_cartorio --apply`, sem `--forcar`: `app/config/crons.py:597`.

## 31/10 16:08 · sessao 34395c524c81 · - · - · origem=Ronald (tmux)

O caminho do cartório usa a regra nova de folga **antes de qualquer decisão sobre pular a célula**, sem selecionar implementação velha conforme `versao_para`: `app/ponto/services/cartorio.py:948`, `app/escala/utils.py:1837`, `app/escala/utils.py:1114`, `app/escala/utils.py:1124`, `app/ponto/services/cartorio.py:1178`.

Caminho comprovado:

- `processar_cartorio` carrega todas as células passadas do colaborador e chama `julgar_colab`, repassando `--forcar`: `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/management/commands/processar_cartorio.py:65`.
- `_julgar_colab_corpo` monta a grade com `montar_grade_prevista_periodo`: `app/ponto/services/cartorio.py:948`. Esse dispatcher seleciona builder por turno ou legacy: `app/escala/utils.py:2076`. A configuração default da flag é `True`; valor efetivo do ambiente [nao verificado]: `app/config/settings/base.py:165`.
- O builder por turno determina vínculo de cada data pela juíza, alimentada com vínculos e células: `app/escala/utils.py:1291`, `app/escala/utils.py:1303`, `app/escala/utils.py:1305`, `app/escala/utils.py:1311`.
- O tipo inicial é `folga` ou `trabalho`: `app/escala/utils.py:1794`, `app/escala/utils.py:1800`. Depois aplica folga lançada, ausências e isenção: `app/escala/utils.py:1837`, `app/escala/utils.py:1840`, `app/escala/utils.py:1842`.
- A folga lançada carrega **todas** as `FolgaDia` da pessoa no período: `app/escala/utils.py:1119`, `app/ponto/precedencia.py:207`. Para cada dia, aceita a primeira candidata válida: `app/escala/utils.py:1124`, `app/ponto/precedencia.py:217`. Manual vale sempre; calendário exige o vínculo do dia: `app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:238`, `app/ponto/precedencia.py:240`.
- Overlay escreve `tipo_dia='folga'`, limpa células e órfãs: `app/escala/utils.py:1132`.
- `ata_do_dia` copia literalmente `dia.get('tipo_dia')`: `app/ponto/services/cartorio.py:584`. A lavra recebe essa ata e os fatos do status; `carimbar` persiste o resultado: `app/ponto/services/cartorio.py:1574`, `app/ponto/services/cartorio.py:1576`, `app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1596`.

O gate com `versao_para < VERSAO_GRADE_FOLGA` existe no **overlay de leitura de ata antiga**, e só transforma grade viva `trabalho` em `folga` se a ata ainda disser `folga`: `app/escala/utils.py:1160`, `app/escala/utils.py:1163`, `app/escala/utils.py:1169`, `app/escala/utils.py:1170`. Quem o chama é `grade_espelho_janela`, depois do builder: `app/escala/utils.py:2329`, `app/escala/utils.py:2330`. O cartório chama o builder diretamente, portanto não passa por esse gate: `app/ponto/services/cartorio.py:948`.

Comparação confirmada pelo Git local (`git -C /home/ronald/wt-v3r2b-133946 diff c7ea207a..ea38de71`):

- Em B, `_aplicar_folgas_foto` consultava somente o vínculo passado e aceitava qualquer FolgaDia dele, sem verificar vínculo do dia; no builder por turno isso era repetido para cada vínculo da janela: **c7ea207a**, `app/escala/utils.py:1101`, `app/escala/utils.py:1109`, `app/escala/utils.py:1116`, `app/escala/utils.py:1785`, `app/escala/utils.py:1787`.
- Em C, a regra é a seleção por pessoa e vínculo do dia descrita acima: **ea38de71**, `app/escala/utils.py:1119`, `app/escala/utils.py:1124`, `app/escala/utils.py:1837`.
- `escala/folgas.py` não mudou nesse diff [nao verificado por arquivo:linha; comprovado por diff Git]. Seu comportamento já preservava calendário no vínculo original: `app/escala/folgas.py:405`, `app/escala/folgas.py:406`.
- Na precedência, além de extrair o predicado, C passou a guardar todas as candidatas por dia e testar cada uma; B guardava só a primeira, podendo descartar uma candidata válida posterior: **C**, `app/ponto/precedencia.py:207`, `app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:217`; **B**, `app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:238` — estas últimas linhas precisam ser conferidas novamente antes de usar [nao verificado].
- `colaboradores/services/calendario.py` deixou de fornecer à precedência `{data: True}` apenas do vínculo ativo, permitindo o carregador completo: `app/colaboradores/services/calendario.py:617`. Também passou a publicar `eh_trabalho` com o mesmo juiz utilizado na classificação: `app/colaboradores/services/calendario.py:93`, `app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:840`. Esse arquivo não integra o caminho de construção da ata pelo cartório descrito acima: `app/ponto/services/cartorio.py:948`, `app/escala/utils.py:2076`.

Há ainda uma mudança de **ordem dos overlays** dentro da mesma alteração de folga: em B, para cada vínculo aplicava folga e depois ausência, permitindo que folga de um vínculo posterior limpasse células/órfãs de uma ausência já aplicada; em C, aplica a folga única antes de todas as ausências: **B**, `app/escala/utils.py:1785`, `app/escala/utils.py:1787`, `app/escala/utils.py:1788`; **C**, `app/escala/utils.py:1837`, `app/escala/utils.py:1838`, `app/escala/utils.py:1840`. Se isso explica alguma das oito células sem mudança de tipo, [nao verificado] sem os respectivos insumos.

A mudança pode afetar `status_dia` indiretamente: seus fatos vêm do mesmo `dia`, inclusive tipo, previsto, células, órfãs e realizado: `app/ponto/status_dia.py:143`, `app/ponto/status_dia.py:146`. O status usa `folga` para dia sem batida e tipo folga ou previsto zero; com batida e tipo folga, usa `folga_trabalhada`: `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:106`, `app/ponto/status_dia.py:115`.

## 31/10 16:08 · sessao d0f45fcacb67 · - · - · origem=Ronald (tmux)

Conclusões no snapshot `ea38de71`, somente leitura:

- **Leitor direto de `ata.tipo_dia`:** `grade_da_celula` consulta todas as `CelulaDia` da janela, lê a ata e prefere `ata['tipo_dia']`; só usa `cel.trabalha` se a chave não existe. Entrega também minutos, lâmpadas, órfãs e veredito. Não há consulta a exportação ou versão nessa função. (`app/escala/services/leitor_celula.py:321-371`, especialmente `:325`, `:331`, `:344-365`.)

- **Folha:** `grade_do_fechamento` chama esse leitor na janela da competência (`app/folha/export.py:318-327`). Um `folga → trabalho` muda a inclusão no universo de dias previstos e na classificação de causa/furos (`app/folha/causas.py:39-45`, `:58-66`, `:92-108`). Na derivação de folha, `trabalho` e `ausencia` entram em previsto/realizado; folga fica fora (`app/ponto/services/fechamento.py:1014-1035`; a derivação por dia está em `app/ponto/services/dia_pago.py:384-398`). Não se pode afirmar quanto mudaria sem examinar os minutos/lâmpadas das células específicas: **[não verificado]**.

- **A guarda da competência 09 não congela leitura online.** A guarda lista `(9, 2026)` (`app/ponto/services/fechamento.py:36-43`). A lista `_exportadas` só é carregada quando há escrita, ou lavratura de DiaPago (`:523-524`); a exclusão pela guarda exige `_guarda and _exportadas` (`:544-548`). `fechamento_lido` calcula com `somente_leitura=True` e devolve objeto sem salvar (`:1703-1729`). Tela e PDF usam essa leitura para os totais da folha (`app/relatorios/cartao_pela_celula.py:69-76`, `:320-323`). Assim, uma ata alterada pode repercutir no cálculo online da competência 09 exportada, embora a escrita dos registros monetários continue protegida pelos ramos indicados.

- **Espelho web:** a montagem obtém a grade da ata a partir de `max(data_ini, apur_ini)` (`app/ponto/services/espelho.py:517-538`); o trecho visual anterior vem do builder (`:539-547`). A classificação da grade alimenta `folga` (`:684-690`, `:779-782`). Com a ata mudada, pode desaparecer o badge “Folga” (`app/templates/ponto/espelho.html:160-166`), e, na ausência de batidas, ausência, inconsistência ou isenção, o status deixa de seguir o ramo `folga` e passa a `sem_registro` se houver previsto passado, senão `vazio` (`app/ponto/services/espelho.py:152-171`). A exceção para ata agregada sem marcos recorre ao builder, portanto não cabe prometer efeito uniforme em todo espelho (`:548-571`).

- **PDF de espelho:** lê a lista de dias de `espelho_do_colab` e a filtra pela janela pedida (`app/relatorios/pdf_espelho.py:592-599`). Reaplica a palavra do dia com a lista de dias em aberto do papel (`:624-634`); a coluna observações imprime `palavra_dia` (`:912-920`). A palavra é produzida com fatos de precedência, decisão/cobertura e dias em aberto, e não simplesmente com `ata.tipo_dia` (`app/ponto/services/espelho.py:339-356`, `:655-660`). Logo “mudou tipo_dia ⇒ PDF necessariamente imprime Trabalho” não está provado: **[não verificado]**.

- **Novo export:** `montar_linhas_export` lê os `FechamentoMensal` gravados, classifica quem entra e emite os eventos desses fechamentos (`app/folha/export.py:692-727`). Os valores de eventos saem de campos monetários do fechamento (`:409-441`), portanto alterar só a ata não prova alteração imediata das quantias emitidas. Entretanto o filtro de retenção lê as células atuais da competência: `classificar_export` chama o retentor (`:942-946`), que consulta `CelulaDia` e usa veredito e protestos (`:1188`, `:1215-1238`). Se o cartório alterar veredito/protestos, pode mudar quem entra ou fica de fora do **novo** TXT, independentemente de números gravados idênticos.

- **Export existente:** baixar o TXT entrega `reg.conteudo`, sem regerar (`app/folha/views.py:548-563`). O PDF do TXT também traduz o registro já salvo (`:818-835`). Portanto mudanças em ata/veredito não alteram esses downloads existentes.

- **`status_dia` é outro campo.** O leitor `status_do_dia_lavrado` consulta e devolve `CelulaDia.status_dia/status_rotulo`, com precedência do relógio antes do encerramento (`app/ponto/status_dia.py:171-185`). A busca global de produção não encontrou chamada dessa função fora de sua definição e do registro em `app/core/juizes.py:173`; folha e espelho examinados usam os caminhos acima. A ausência universal de qualquer leitura dinâmica/indireta é **[não verificado]**. Não confundir `CelulaDia.status_dia` com o `dia['status']` que `_status_dia` monta no espelho (`app/ponto/services/espelho.py:152-183`).

## 31/10 16:08 · sessao 92cb4569b890 · TRAVA-COBERTURA · censo · origem=Code

A cobertura do selo é incompleta: **calendário e autocura executam a lista inteira sem a trava**, e esses disparos não são cobrados por ele (`bin/regua_calendario.sh:22`, `bin/regua_calendario.sh:45`, `bin/molde_autocura/rodar.sh:25`, `bin/molde_autocura/rodar.sh:26`, `bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:59`).

Nos quadros, **CI** significa `config.settings.ci`: host `DB_HOST` ou `juliani_db_test`; banco de teste `REGUA_DB` ou `test_juliani` (`app/config/settings/ci.py:12`, `app/config/settings/ci.py:17`). Os valores efetivos vindos dos env-files/imagem em execução estão **[nao verificado]**. “Inteira” significa a lista `LABELS` da régua; `tenants` roda separado (`bin/regua.sh:23`, `CLAUDE.md:197`).

**Disparos em `bin/`:**

| Arquivo:linha do disparo | Passa pela trava? | Banco/settings | Quem chama | Escopo |
|---|---|---|---|---|
| `bin/suite.sh:102` | **Sim**, direto: `bin/suite.sh:96` | CI; repassa `REGUA_DB` exportado: `bin/suite.sh:89`; sem ele, default CI | Humano, comando canônico: `CLAUDE.md:171` | Inteira ou recorte/teste indicado por `--only`: `bin/suite.sh:47`, `bin/suite.sh:102` |
| `bin/regua.sh:168` | **Sim**, direto: `bin/regua.sh:165` | CI; default declarado: `bin/regua.sh:159`; settings: `bin/regua.sh:169` | Integrador: `app/core/integrador_lote.py:375`; cadeias nas linhas abaixo | Inteira ou `--only`: `bin/regua.sh:23`, `bin/regua.sh:51` |
| `bin/pre-push.sh:186` | **Sim**, por `_teste_irmao`: `bin/pre-push.sh:142` | CI, sem `REGUA_DB` explícito: `bin/pre-push.sh:186`, `bin/pre-push.sh:188` | Wrapper de hook produzido por `bin/hooks.sh:56`; instalação efetiva **[nao verificado]** | Inteira, `$LABELS`: `bin/pre-push.sh:184`, `bin/pre-push.sh:187` |
| `bin/pre-push.sh:198` | **Sim**, mesmo wrapper: `bin/pre-push.sh:142` | `teste_motor`: `bin/pre-push.sh:199`; host fixo `juliani_db_test`, conexão `juliani`: `app/config/settings/teste_motor.py:6`, `app/config/settings/teste_motor.py:7`; nome final do banco criado pelo Django **[nao verificado]** | Mesmo hook: `bin/hooks.sh:56` | Recorte `tenants`: `bin/pre-push.sh:198` |
| `bin/regua_relogio.sh:55` | **Sim**, direto: `bin/regua_relogio.sh:51` | Shim `cfg_tz` herda CI: `bin/regua_relogio.sh:35`; sem `REGUA_DB` explícito: `bin/regua_relogio.sh:52` | Comentário declara cron: `bin/regua_relogio.sh:48`; inscrição atual em `config/crons.py` **[nao verificado]** | Inteira, `APPS=LABELS`: `bin/regua_relogio.sh:27`, `bin/regua_relogio.sh:29` |
| `bin/vigia_arvore.sh:86` | **Sim**, direto: `bin/vigia_arvore.sh:83` | CI; `test_juliani_vigia` na primeira rodada: `bin/vigia_arvore.sh:88`; `test_juliani_vigia_b` na repetição: `bin/vigia_arvore.sh:106` | Cron horário: `app/config/crons.py:277`; também `_revigiar`: `app/core/esteira_vigia.py:1141` | Inteira primeiro; recorte das falhas depois: `bin/vigia_arvore.sh:88`, `bin/vigia_arvore.sh:105`, `bin/vigia_arvore.sh:106` |
| `bin/vigia_arvore.sh:134` | **Sim**, direto: `bin/vigia_arvore.sh:131` | CI; `REGUA_DB=test_juliani_vigia_viva`: `bin/vigia_arvore.sh:132` | Mesmo cron/revigia acima; ramo interno após repetição: `bin/vigia_arvore.sh:113` | Recorte `$IDS2`: `bin/vigia_arvore.sh:113`, `bin/vigia_arvore.sh:134` |
| `bin/molde_fatia/rodar.sh:30` | **Não**, Docker direto: `bin/molde_fatia/rodar.sh:28` | CI; `REGUA_DB=test_juliani___FATIA__m`: `bin/molde_fatia/rodar.sh:29` | Esteira: `bin/molde_fatia/esteira.sh:15`; refabricação de `rodar.sh` gerado: `bin/refabricar.sh:59` | Recorte de um módulo, `core.tests.test_selo_mapa_contrato`: `bin/molde_fatia/rodar.sh:30` |
| `bin/molde_fatia/rodar.sh:41` | **Sim**, direto: `bin/molde_fatia/rodar.sh:40` | CI; RED `test_juliani___FATIA__r`, GREEN `test_juliani___FATIA__g`: `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:47` | Mesma esteira: `bin/molde_fatia/esteira.sh:15` | Recortes RED/GREEN; retirada da inteira é explícita: `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:49` |
| `bin/molde_autocura/rodar.sh:20` | **Não**, Docker direto: `bin/molde_autocura/rodar.sh:20` | CI; RED `test_juliani_esacred`, GREEN `test_juliani_esacfull`: `bin/molde_autocura/rodar.sh:24`, `bin/molde_autocura/rodar.sh:26` | Esteira de autocura: `bin/molde_autocura/esteira.sh:14`; pacote gerado pelo vigia: `app/core/esteira_vigia.py:1426` | RED recortado por IDs; **GREEN inteiro**: `bin/molde_autocura/rodar.sh:22`, `bin/molde_autocura/rodar.sh:25`, `bin/molde_autocura/rodar.sh:26` |
| `bin/isolamento.sh:33` | **Não**, Docker direto: `bin/isolamento.sh:31` | CI; `test_juliani_iso_$fatia`: `bin/isolamento.sh:45`, `bin/isolamento.sh:58` | Cadeias: `bin/molde_fatia/cadeia.sh:55`, `bin/molde_autocura/cadeia.sh:52` | Recorte dos IDs falhos: `bin/isolamento.sh:20`, `bin/isolamento.sh:40` |
| `bin/regua_calendario.sh:45` | **Não**, Docker direto: `bin/regua_calendario.sh:43` | Shim herda CI: `bin/regua_calendario.sh:30`; `test_juliani_cald20`, `test_juliani_cald21`, `test_juliani_cald01`: `bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:44` | Chamador atual **[nao verificado]** | **Inteira três vezes**, com datas diferentes: `bin/regua_calendario.sh:22`, `bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:45` |
| `bin/placar_code.sh:124` | **Não**, Docker direto: `bin/placar_code.sh:122` | CI; `REGUA_DB=test_juliani_placar`: `bin/placar_code.sh:123`, `bin/placar_code.sh:126` | Chamador atual **[nao verificado]** | Recorte de uma classe, `CContratoCredencialSemTrilhaTest`: `bin/placar_code.sh:125` |

Os nomes com `__FATIA__` são moldes substituídos pelo gerador; os arquivos materializados em execução estão **[nao verificado]** (`bin/molde_fatia/gerar.py:24`, `bin/molde_fatia/gerar.py:29`, `bin/molde_fatia/gerar.py:32`).

**Disparos em `app/`:**

O executor comum `_rodar_testes` monta `manage.py test` em `app/core/esteira_vigia.py:757` e executa o subprocesso diretamente em `app/core/esteira_vigia.py:760`, **sem `trava_teste.sh`**. Injeta o argumento `banco` em `REGUA_DB` e usa CI (`app/core/esteira_vigia.py:752`, `app/core/esteira_vigia.py:758`).

| Arquivo:linha do disparo | Passa pela trava? | Banco/settings | Quem chama | Escopo |
|---|---|---|---|---|
| `app/core/hooks/pre-push.sh:82` | **Não**, helper Docker direto: `app/core/hooks/pre-push.sh:54`; outro lock em `app/core/hooks/pre-push.sh:32` | CI, sem `REGUA_DB` explícito: `app/core/hooks/pre-push.sh:84` | Pretende ser hook: `app/core/hooks/pre-push.sh:2`; ativação atual **[nao verificado]** | Inteira `$LABELS`: `app/core/hooks/pre-push.sh:80`, `app/core/hooks/pre-push.sh:83` |
| `app/core/hooks/pre-push.sh:87` | **Não**, mesmo helper: `app/core/hooks/pre-push.sh:54` | `teste_motor`: `app/core/hooks/pre-push.sh:88`; host fixo `juliani_db_test`: `app/config/settings/teste_motor.py:6`; banco de teste final **[nao verificado]** | Mesmo hook; ativação **[nao verificado]** | Recorte `tenants`: `app/core/hooks/pre-push.sh:87` |
| `app/core/esteira_vigia.py:740` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_esiso`: `app/core/esteira_vigia.py:740` | `_isolar`, callback de `passada`: `app/core/esteira_vigia.py:1473` | Recorte FAIL/ERROR de `green.out`, contra `orig/app`: `app/core/esteira_vigia.py:736`, `app/core/esteira_vigia.py:737` |
| `app/core/esteira_vigia.py:1269` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_revert`: `app/core/esteira_vigia.py:1270` | `auto_revert`, pela passada ou CLI: `app/core/esteira_vigia.py:1552`, `app/core/esteira_vigia.py:1579` | Recorte das falhas; fallback no módulo `core.tests.test_esteira_sem_humano`: `app/core/esteira_vigia.py:1259`, `app/core/esteira_vigia.py:1269` |
| `app/core/esteira_vigia.py:1319` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_congelado`, com `CONGELAR_EM`: `app/core/esteira_vigia.py:1319`, `app/core/esteira_vigia.py:1320` | `curar_por_relogio`, passada ou CLI: `app/core/esteira_vigia.py:1554`, `app/core/esteira_vigia.py:1585` | Recorte das falhas registradas: `app/core/esteira_vigia.py:1310` |
| `app/core/esteira_vigia.py:1406` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_esauto`: `app/core/esteira_vigia.py:1406` | `autocura`, pela passada: `app/core/esteira_vigia.py:1500` | Um ID por execução, `[x[0]]`; número de métodos abrangidos depende desse ID **[nao verificado]**: `app/core/esteira_vigia.py:1406` |
| `app/core/integrador_lote.py:373` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_lote`: `app/core/integrador_lote.py:373` | `_suite(ids)`, pelo bisect: `app/core/integrador_lote.py:782`, `app/core/integrador_lote.py:789` | Recorte dos vermelhos: `app/core/integrador_lote.py:776` |
| `app/core/integrador_lote.py:375` | **Sim**, via régua: `bin/regua.sh:165` | CI/default, pela régua: `bin/regua.sh:159`, `bin/regua.sh:169` | `_suite()` sem IDs, por `rodar`: `app/core/integrador_lote.py:639`, `app/core/integrador_lote.py:672`, `app/core/integrador_lote.py:691` | Inteira: `app/core/integrador_lote.py:368`, `bin/regua.sh:23` |
| `app/core/integrador_lote.py:403` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_lote_iso`: `app/core/integrador_lote.py:403` | `_isolamento`, por `rodar`: `app/core/integrador_lote.py:658` | Recorte dos vermelhos, sem as fatias: `app/core/integrador_lote.py:399` |
| `app/core/integrador_lote.py:769` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_quarentena`, `SO_QUARENTENA=1`: `app/core/integrador_lote.py:769` | `_conferir_quarentena`, por `_subir`: `app/core/integrador_lote.py:757` | Recorte da quarentena: `app/core/integrador_lote.py:766`; seleção só dos quarentenados: `app/core/runner_quarentena.py:78` |

A cópia de hook em `app/` tem uma declaração de que seria a fonte, mas outro selo registra que essa migração “NAO LANDOU”; o instalador atual produz delegação para **`bin/pre-push.sh`** (`app/core/hooks/hooks_fonte.py:10`, `bin/tests/test_recursos_sourced.sh:28`, `bin/hooks.sh:56`). Portanto, sua presença é comprovada; seu uso atual permanece **[nao verificado]**.

**Delegadores que também disparam essas suites:**

| Arquivo:linha do disparo | Trava, banco e escopo | Quem chama |
|---|---|---|
| `bin/molde_fatia/cadeia.sh:51`, `bin/molde_fatia/cadeia.sh:70`, `bin/molde_fatia/cadeia.sh:81` | **Sim**, via régua; CI/default, inteira: `bin/regua.sh:165`, `bin/regua.sh:168`, `bin/regua.sh:169` | Chamador atual **[nao verificado]**; a esteira atual entrega ao integrador: `bin/molde_fatia/esteira.sh:26` |
| `bin/molde_autocura/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:67`, `bin/molde_autocura/cadeia.sh:78` | **Sim**, via régua; CI/default, inteira: `bin/regua.sh:165`, `bin/regua.sh:168`, `bin/regua.sh:169` | `bin/molde_autocura/esteira.sh:23` |
| `bin/molde_fatia/esteira.sh:15` | **Não** para MAPA; **sim** para RED/GREEN; bancos e recortes do molde: `bin/molde_fatia/rodar.sh:29`, `bin/molde_fatia/rodar.sh:30`, `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:47` | Lançadores genéricos abaixo; associação com pacote materializado **[nao verificado]** |
| `bin/molde_autocura/esteira.sh:14` | **Não**, RED recortado e GREEN inteiro, nos bancos do molde: `bin/molde_autocura/rodar.sh:20`, `bin/molde_autocura/rodar.sh:24`, `bin/molde_autocura/rodar.sh:26` | Lançadores genéricos abaixo; associação com pacote materializado **[nao verificado]** |
| `bin/integrador.sh:52` | **Sim** para inteira via régua; **não** para recortes do executor Python: `app/core/integrador_lote.py:375`, `bin/regua.sh:165`, `app/core/integrador_lote.py:373`, `app/core/esteira_vigia.py:760`; bancos conforme quadro anterior | Service: `bin/hasner-integrador.service:13`; timer `*:0/5`: `bin/hasner-integrador.timer:6` |
| `bin/vigia_esteira.sh:11` | **Não** para diagnósticos Python; bancos/recortes acima: `app/core/esteira_vigia.py:740`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1319`, `app/core/esteira_vigia.py:1406`, `app/core/esteira_vigia.py:760` | Service: `bin/hasner-esteira.service:15`; timer `*:0/5`: `bin/hasner-esteira.timer:7`; declarado fora do cron: `app/config/crons.py:739` |
| `app/core/esteira_vigia.py:1141` | **Sim**, via `vigia_arvore`; CI, inteira e possíveis repetições recortadas: `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:88`, `bin/vigia_arvore.sh:106`, `bin/vigia_arvore.sh:131` | `_revigiar`, descrito em `app/core/esteira_vigia.py:1135` |
| `bin/refabricar.sh:59` | Executa `rodar.sh` materializado; **sim/não, banco e escopo [nao verificado]**, dependem desse arquivo | Uso humano documentado: `bin/refabricar.sh:11` |
| `bin/deploy_agendado.sh:73` | Executa `esteira.sh` fornecida; **sim/não, banco e escopo [nao verificado]**, dependem do pacote | Cron avulso gerado em `/etc/cron.d`, fora de `config/crons.py`: `bin/deploy_agendado.sh:54`, `bin/deploy_agendado.sh:55` |
| `bin/fabricante.sh:187` | Lança `esteira.sh` gerada; **sim/não, banco e escopo [nao verificado]**, dependem do pacote | Service: `bin/hasner-fabricante.service:14`; timer: `bin/hasner-fabricante.timer:8` |
| `app/core/esteira_vigia.py:913`, `app/core/esteira_vigia.py:917` | Lança `esteira.sh` por systemd ou fallback; **sim/não, banco e escopo [nao verificado]**, dependem do pacote | `relancar`: `app/core/esteira_vigia.py:936`; passada: `app/core/esteira_vigia.py:1516` |

**(1) O que o selo detecta**

O critério executado é textual e por **arquivo**:

- Varre somente `bin/*.sh`; pula caminhos contendo `test_` e a própria trava (`bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:41`).
- Remove comentários de linha inteira e linhas contendo `pgrep|pkill|pidof`; exige o literal `manage.py test` (`bin/tests/test_trava_teste.sh:53`, `bin/tests/test_trava_teste.sh:54`).
- Havendo `REGUA_DB` em qualquer parte do código restante, isenta o arquivo, salvo se também encontrar `test api chamados` ou o literal `test ${ONLY:-$LABELS}` (`bin/tests/test_trava_teste.sh:59`, `bin/tests/test_trava_teste.sh:60`).
- Para aceitar um arquivo cobrado, basta encontrar `trava_teste.sh`; não verifica a ligação entre essa ocorrência e cada disparo (`bin/tests/test_trava_teste.sh:62`).
- O molde de fatia recebe apenas um `grep` separado procurando a string da trava no arquivo (`bin/tests/test_trava_teste.sh:70`).

| Chamadores do censo | Detecta/cobra? Por quê |
|---|---|
| `suite.sh`, `regua.sh` | **Sim, por arquivo**: usam o padrão especial `${ONLY:-$LABELS}`; a presença da trava satisfaz a checagem (`bin/suite.sh:96`, `bin/suite.sh:102`, `bin/regua.sh:165`, `bin/regua.sh:168`, `bin/tests/test_trava_teste.sh:59`, `bin/tests/test_trava_teste.sh:62`). |
| Ambos os disparos de `bin/pre-push.sh` | **Sim, por arquivo**, sem distinguir os dois comandos; aceita a ocorrência do helper com trava (`bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:198`, `bin/tests/test_trava_teste.sh:62`). |
| `regua_relogio.sh` | **Sim**: literal presente, sem `REGUA_DB` explícito; aceita a trava (`bin/regua_relogio.sh:51`, `bin/regua_relogio.sh:55`, `bin/tests/test_trava_teste.sh:54`, `bin/tests/test_trava_teste.sh:60`). |
| Disparos de `vigia_arvore.sh` | **Não cobra**: têm `REGUA_DB` e usam `$1`/`$IDS2`, fora dos padrões especiais, embora realmente passem pela trava (`bin/vigia_arvore.sh:84`, `bin/vigia_arvore.sh:86`, `bin/vigia_arvore.sh:132`, `bin/vigia_arvore.sh:134`, `bin/tests/test_trava_teste.sh:59`). |
| `isolamento.sh`, `placar_code.sh`, `regua_calendario.sh` | **Não cobra**: isenção textual por `REGUA_DB`; `$LABELS` do calendário não casa com o padrão especial (`bin/isolamento.sh:31`, `bin/placar_code.sh:123`, `bin/regua_calendario.sh:44`, `bin/regua_calendario.sh:45`, `bin/tests/test_trava_teste.sh:59`). |
| `molde_fatia/rodar.sh:41` | **Só verifica presença textual**, pelo grep específico (`bin/molde_fatia/rodar.sh:40`, `bin/tests/test_trava_teste.sh:70`). |
| `molde_fatia/rodar.sh:30` | **Não detecta esse bypass**: a trava da outra função satisfaz o mesmo grep (`bin/molde_fatia/rodar.sh:30`, `bin/molde_fatia/rodar.sh:40`, `bin/tests/test_trava_teste.sh:70`). |
| `molde_autocura/rodar.sh` | **Não**: subdiretório fora do glob e sem checagem específica (`bin/molde_autocura/rodar.sh:20`, `bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:70`). |
| Hook em `app/`, executor Python e todos os seus consumidores | **Não**: fora de `bin/*.sh`; o selo não segue chamadas transitivas (`app/core/hooks/pre-push.sh:82`, `app/core/esteira_vigia.py:760`, `bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:54`). |
| Demais delegadores do quadro anterior | **Não os descobre como disparadores de testes**: chamar régua, Python ou esteira não satisfaz o literal exigido (`bin/molde_fatia/cadeia.sh:51`, `bin/integrador.sh:52`, `bin/vigia_esteira.sh:11`, `bin/tests/test_trava_teste.sh:54`). |

O selo também testa serialização e saída 75, mas em **cópia com lock isolado**, não no lock global nem exercitando cada chamador (`bin/tests/test_trava_teste.sh:103`, `bin/tests/test_trava_teste.sh:119`, `bin/tests/test_trava_teste.sh:134`). Não o executei: ele cria arquivos temporários e cópias (`bin/tests/test_trava_teste.sh:77`, `bin/tests/test_trava_teste.sh:101`, `bin/tests/test_trava_teste.sh:103`).

**(2) Motivos declarados para ficar fora**

- **Isolamento, placar e calendário:** há declaração nominal de isenção por banco próprio e por supostamente não executarem `LABELS` inteiro; o relato também declara que ficaram intocados por escopo (`app/docs/RELATO-ARQUIVO.md:38181`). **A justificativa do calendário diverge do código**, que lê `LABELS` e executa tudo três vezes (`bin/regua_calendario.sh:22`, `bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:45`).
- **Isolamento:** declara banco próprio e finalidade de repetir apenas falhas sem a fatia (`bin/isolamento.sh:4`, `bin/isolamento.sh:23`). **Calendário:** declara cópia e banco próprio; o shim, contudo, herda CI, incluindo a regra de host (`bin/regua_calendario.sh:14`, `bin/regua_calendario.sh:30`, `app/config/settings/ci.py:12`).
- **MAPA do molde de fatia:** declara por que executa o selo antes de copiar para a árvore, e usa banco próprio; dispensa individual da trava **[nao verificado]** (`bin/molde_fatia/rodar.sh:23`, `bin/molde_fatia/rodar.sh:29`).
- **Autocura, inclusive seu GREEN inteiro:** bancos próprios estão declarados; motivo específico para dispensar a trava **[nao verificado]** (`bin/molde_autocura/rodar.sh:24`, `bin/molde_autocura/rodar.sh:26`).
- **Diagnósticos Python do vigia e integrador:** declaram isolamento, confirmação com relógio congelado, bisect e conferência informativa; justificativa individual de bypass **[nao verificado]** (`app/core/esteira_vigia.py:726`, `app/core/esteira_vigia.py:1306`, `app/core/integrador_lote.py:776`, `app/core/integrador_lote.py:762`). Existe apenas a isenção geral do selo para recortes com banco próprio (`bin/tests/test_trava_teste.sh:55`, `bin/tests/test_trava_teste.sh:60`).
- **Hook em `app/`:** declara serialização por **outro lock**, `/tmp/regua.lock`, inclusive herança; não comprova dispensa de `trava_teste.sh` (`app/core/hooks/pre-push.sh:29`, `app/core/hooks/pre-push.sh:32`, `app/core/hooks/pre-push.sh:38`). Motivo específico **[nao verificado]**.
- Autorização específica adicional em `CORTES.md` ou `PISTA.md` **[nao verificado]**. Há declaração contrária, “uma suite por vez na trava”, em `app/docs/CORTES.md:258`; o comando canônico mantém até `--only` dentro dela (`CLAUDE.md:174`).

**(3) `bin/trava_teste.sh --quem`**

Ele lê exclusivamente **`/tmp/juliani_db_test.lock.quem`**: se o arquivo não estiver preenchido, ou a leitura falhar, imprime `livre` e termina **antes de consultar o flock** (`bin/trava_teste.sh:21`, `bin/trava_teste.sh:28`, `bin/trava_teste.sh:35`).

O lock real é o flock do descritor 9 sobre **`/tmp/juliani_db_test.lock`**. Depois de adquiri-lo, o script escreve nome informado por `ESTEIRA_QUEM` ou basename, PID do wrapper e horário; o `trap EXIT` escreve `livr

[truncado em 20000 de 21024 caracteres]

## 31/10 16:05 · sessao 05d1cebb902a · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 16:05 · sessao 31daa219b7c3 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 16:05 · sessao e593285a4f13 · - · - · origem=Ronald (tmux)

O selo faz varredura textual, por arquivo, não por disparo: percorre somente `bin/*.sh`, pula caminhos contendo `test_` e `trava_teste.sh`, remove comentários de linha inteira e linhas contendo `pgrep|pkill|pidof`, exige literal `manage.py test`, e aceita a presença textual de `trava_teste.sh`. (`bin/tests/test_trava_teste.sh:40-63`)

A isenção efetiva é: se o texto contém `REGUA_DB`, pula o arquivo, **exceto** se também contiver `test api chamados` ou o literal `test ${ONLY:-$LABELS}`. Não avalia o valor do banco nem segue variáveis, funções ou cadeias de scripts. (`bin/tests/test_trava_teste.sh:59-62`)

Aplicação aos disparadores conhecidos:

| Chamador | Selo detecta/cobra? | Prova |
|---|---|---|
| `bin/pre-push.sh` — ambos os disparos | Sim, por arquivo; aceita string da trava | `bin/pre-push.sh:142`, `:186`, `:198`; critério `bin/tests/test_trava_teste.sh:54-62` |
| `bin/regua.sh` | Sim; padrão especial impede isenção por `REGUA_DB` | `bin/regua.sh:165-168`; `bin/tests/test_trava_teste.sh:59-62` |
| `bin/suite.sh` | Sim; mesmo padrão especial | `bin/suite.sh:96-102`; `bin/tests/test_trava_teste.sh:59-62` |
| `bin/regua_relogio.sh` | Sim; não passa `REGUA_DB` | `bin/regua_relogio.sh:49-55`; `bin/tests/test_trava_teste.sh:54-62` |
| `bin/vigia_arvore.sh` — ambos os locais | **Não cobra**: encontra literal, mas isenta por `REGUA_DB`; os comandos usam `$1` e `$IDS2`, não os padrões especiais | `bin/vigia_arvore.sh:84-86`, `:132-134`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/isolamento.sh` | Não cobra: isento por `REGUA_DB` | `bin/isolamento.sh:31-33`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/placar_code.sh` | Não cobra: isento por `REGUA_DB` | `bin/placar_code.sh:123-126`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/regua_calendario.sh` | **Não cobra, embora rode LABELS inteiro**: isenção textual por `REGUA_DB`, comando usa `$LABELS` | `bin/regua_calendario.sh:22`, `:44-45`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/molde_fatia/rodar.sh:41` | Só grep separado procurando string da trava no arquivo bruto | `bin/tests/test_trava_teste.sh:70-71`; trava presente em `bin/molde_fatia/rodar.sh:40` |
| `bin/molde_fatia/rodar.sh:30` | **Não detecta o bypass deste disparo**: grep anterior aceita a trava presente na outra função | `bin/molde_fatia/rodar.sh:28-30`, `:40-41`; `bin/tests/test_trava_teste.sh:70-71` |
| `bin/molde_autocura/rodar.sh` | Não: subdiretório fora do glob, sem verificação específica | disparo `bin/molde_autocura/rodar.sh:20`; glob/verificação específica `bin/tests/test_trava_teste.sh:40`, `:70` |
| `app/core/hooks/pre-push.sh` — ambos | Não: fora de `bin/*.sh` | `app/core/hooks/pre-push.sh:82`, `:87`; `bin/tests/test_trava_teste.sh:40` |
| `app/core/esteira_vigia.py::_rodar_testes` | Não: fora de `bin/*.sh` | `app/core/esteira_vigia.py:751-760`; `bin/tests/test_trava_teste.sh:40` |
| Wrappers que só chamam `suite.sh`/`regua.sh` | Não descobertos diretamente se não contêm `manage.py test`; nenhuma análise transitiva | `bin/tests/test_trava_teste.sh:54`, `:62` |

Além disso, cobra executável da trava, pelo menos dois arquivos aceitos, e testa serialização/rc 75 usando **cópia com lock isolado**; não exercita o lock global nem prova cobertura de todos os chamadores. (`bin/tests/test_trava_teste.sh:20`, `:85`, `:103-110`, `:119-137`)

Motivos declarados:

- Isenção genérica de recortes com banco próprio está escrita no selo: `bin/tests/test_trava_teste.sh:8`, `:27-28`, `:55-61`.
- Documento nomeia `isolamento.sh`, `placar_code.sh` e `regua_calendario.sh` como sem trava, com `REGUA_DB` próprio, “isenção declarada do selo, porque nao rodam o LABELS inteiro”; também declara não mexer por escopo da obra. **A parte referente ao calendário diverge do código atual**, que deriva LABELS e roda a lista completa três vezes. (`app/docs/RELATO-ARQUIVO.md:38181`; `bin/regua_calendario.sh:22`, `:41-45`)
- `app/core/hooks/pre-push.sh` declara serialização por **outro lock**, `/tmp/regua.lock`, herdável com `REGUA_TRAVA_HERDADA=1`; não declara autorização específica para ficar fora de `trava_teste.sh`. (`app/core/hooks/pre-push.sh:29-41`; motivo específico `[nao verificado]`)
- Para `molde_autocura`, o disparo isolado do MAPA em `molde_fatia` e `_rodar_testes`, motivo explícito individual para bypass `[nao verificado]`; os bancos próprios estão no código, mas isso sozinho não declara uma exceção individual. (`bin/molde_autocura/rodar.sh:20`, `:24-26`; `bin/molde_fatia/rodar.sh:28-30`; `app/core/esteira_vigia.py:752`)
- A regra canônica manda `--only` passar pela trava e diz que `regua.sh`/`bin/pre-push.sh` mantêm chamadas próprias **porque já pegam a trava**, evitando aninhamento. (`CLAUDE.md:174-179`)
- Não localizei autorização específica de bypass em `app/docs/CORTES.md` ou `app/docs/PISTA.md` `[nao verificado]`; há, em sentido contrário, “uma suite por vez na trava” e “um push só, sem outra suite rodando junto”. (`app/docs/CORTES.md:258`, `:349`)

`--quem`:

- Lê exclusivamente `/tmp/juliani_db_test.lock.quem`; se não estiver não vazio, ou o `cat` falhar, imprime `livre` e sai antes de abrir/consultar `flock`. (`bin/trava_teste.sh:21`, `:28`, `:35-36`)
- O lock real é adquirido no descritor 9 sobre `/tmp/juliani_db_test.lock`; só depois o script escreve o informativo. O texto contém `ESTEIRA_QUEM` ou basename, PID do wrapper e data; não identidade conferida de container ou processo Django. (`bin/trava_teste.sh:23`, `:35-40`)
- Pode ler informação anterior entre aquisição e escrita, ou `livre desde...` durante o trap antes de encerrar o wrapper: informativo e lock não são uma leitura atômica. (`bin/trava_teste.sh:35-42`, inferência da ordem)
- Se o wrapper morrer sem executar o trap, o último detentor pode permanecer no arquivo; ocorrência real nesta árvore `[nao verificado]`. A atualização para `livre` depende do trap EXIT. (`bin/trava_teste.sh:40-41`)
- Suites sem a trava são invisíveis ao informativo; a própria documentação declara que `livre` nunca garante ausência de suite. (`CLAUDE.md:203-205`; `bin/suite.sh:7-13`)
- Há relato explícito de containers órfãos ainda testando enquanto `--quem` respondia `livre` após morte do lançador; a existência deles agora `[nao verificado]`. (`app/docs/BACKLOG.md:1421`; `app/docs/RELATO-ARQUIVO.md:1380`)


_1131 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
