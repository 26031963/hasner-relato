# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 13:37** · publicado em **31/10/2026 13:38** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 13:37 · sessao 608be0c6532d · CLAUDE-MD-RACIONAL · verificador · origem=Code

1. **Sim, com o mesmo efeito.** Data e obrigação de citar o aval estão em `CLAUDE.md:444-445`. As condições conjuntas — função existente, autoridade única medida, mesmo predicado e universo igual, medidos na sessão — continuam em `CLAUDE.md:460-464`; predicado novo ou duas candidatas exigem pergunta em `CLAUDE.md:453-454`. Correspondem a `41b4d89d:CLAUDE.md:478-486`.

   `app/docs/CORTES.md:64` e `app/docs/CORTES.md:283` continuam encontrando o aval pelo nome e pela data. **A numeração é histórica:** o antigo item 3 corresponde agora ao item 5 de PARADA, identificado pela ponte em `CLAUDE.md:444`.

2. **Não encontrei perda de cláusula, nome, script ou condição nos cortes desta volta.** A preservação aparece nestes pontos:

   | Trecho | Evidência atual |
   |---|---|
   | PARADA: RED, script e três sentidos | `CLAUDE.md:442-443` |
   | RAIAS: fonte do handoff e avais de hoje não cumpridos | `CLAUDE.md:483-484` |
   | CODEX: apelidos retirados da linha duplicada | `CODEX-SONDA-LEITURA` em `CLAUDE.md:524`; `SUITE-ANTES-DO-OPUS` em `CLAUDE.md:515` e `CLAUDE.md:549` | [nao verificado]
   | RELATO: mesma pergunta, frase de nascimento e declaração em `JUIZES_DO_PRAZO` | `CLAUDE.md:607`, `CLAUDE.md:624-627` |
   | PLACAR: exercício, dois zeros, rótulo, mostrar sem fechamento e L-095 | `CLAUDE.md:565-570` |
   | PONTUALIDADE: ponteiro e regra | `CLAUDE.md:601`, `app/docs/LEIS.md:116` |
   | DINHEIRO: backup/renome pelo admin | `CLAUDE.md:577` |

3. **Não identifiquei efeito adicional além da precedência dos cinco consolidados.** A exceção está expressamente limitada a eles em `app/docs/PISTA.md:10-12`. O selo extrai os blocos pelas âncoras, deixando o cabeçalho fora dos hashes; verifica nomes, hashes e ponteiros, sem comparar a frase antiga (`bin/tests/test_claude_md_regras_preservadas.sh:43-78`). Nenhuma dependência dessa frase foi encontrada nos leitores examinados; o leitor de `PISTA.md` identificado está em `bin/tests/test_claude_md_regras_preservadas.sh:35`.

4. **Contagem medida com `wc -c`**, incluindo as quebras de linha:

   | Conteúdo | Bytes | Localização |
   |---|---:|---|
   | 7b com título, até antes de `## 8.` | **20.995** | `CLAUDE.md:436-628` |
   | Mesmo trecho sem a linha do título | **20.911** | `CLAUDE.md:437-628` |
   | Linha do título | **84** | `CLAUDE.md:436` |
   | Arquivo inteiro | **57.796** | `CLAUDE.md:1-632` |

## 31/10 13:37 · sessao aa15e33c6c46 · - · - · origem=Ronald (tmux)

A escrita física encontrada é `Pauta.objects.create(..., texto=texto[:500])`; a porta normaliza com `strip`, rejeita vazio e corta a 500 no último passo. `responder` passa pela mesma porta. (`app/pautas/services.py:163-182`, `app/pautas/services.py:189-197`)

| Chamador de escrita | Referência |
|---|---|
| View escrever | `app/pautas/views.py:76` |
| View responder → service responder | `app/pautas/views.py:112` |
| CLI suporte responder → service responder | `app/pautas/management/commands/suporte_hasner.py:133` |
| Semanal | `app/pautas/semanal.py:276` |
| Dia em aberto: cabeça/filhas | `app/pautas/management/commands/pauta_dia_em_aberto.py:127`, `:139`, `:142` |
| Pré-fechamento | `app/folha/management/commands/pre_fechamento.py:110` |
| Aparelhos compartilhados | `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107` |
| Cobertura | `app/ponto/services/cobertura.py:333` |
| Ausência/documento vencido; férias recusadas | `app/ponto/services/ausencia.py:1039`, `:1229` |
| Discordantes | `app/ponto/services/pautas_dos_discordantes.py:177` |
| Afastado avisa | `app/ponto/services/afastado_avisa.py:74` |
| Deslogados | `app/ponto/services/pautas_dos_deslogados.py:77` |
| HE | `app/ponto/services/autorizacao_he_periodo.py:244` |
| Esmeril | `app/ponto/services/pautas_do_esmeril.py:76` |
| Férias | `app/ferias/management/commands/pautas_de_ferias.py:167` |
| Cron quebrado | `app/core/services/crons_quebrados.py:71` |
| Alarme esteira; árvore | `app/core/management/commands/alarme_esteira.py:54`, `app/core/management/commands/alarme_arvore.py:50` |
| Limbo exportado | `app/chamados/services/fabrica_da_celula.py:185` |
| Passivo validado: cabeça/filhas/quem saiu | `app/chamados/services/passivo_validado.py:229`, `:233`, `:266` |
| Resposta tardia | `app/chamados/services/resposta_tardia.py:62` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:224` |
| Órfãs enterradas | `app/chamados/management/commands/reabrir_orfas_enterradas.py:197` |

Entre os emissores automáticos, fábrica e cobertura usam âncora `dia`; views aceitam a âncora enviada e respostas herdam a âncora original. (`app/chamados/services/fabrica_da_celula.py:185`, `app/ponto/services/cobertura.py:334`, `app/pautas/views.py:80`, `app/pautas/services.py:196`)

O campo declara `max_length=500`; a migration inicial apenas cria o schema. (`app/pautas/models.py:89`, `app/pautas/migrations/0001_initial.py:39`)

Defeitos adicionais:

- **A exceção limbo exportado pode ser contornada pelo lote.** O evento envia à Pauta e retorna sem perguntar, mas o universo FIO MUDO do lote seleciona chamado vivo sem filtrar `trabalha` ou competência, procura motivo pela ata e chama diretamente `perguntar`. Para chamado vivo de limbo exportado com motivo faltante e sem pergunta existente, a Pauta não barra esse caminho. O cron está configurado com `--apply`, e não apenas como vigia. Ocorrência desses candidatos em produção: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:153-157`, `app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`, `:178-224`, `:277-301`, `app/config/crons.py:1464-1466`)
- **Comentário novo contradiz a implementação:** diz que `escrever` corta o fim do corpo para acomodar contexto; agora o contexto cede, preservado o corte global de 500. (`app/chamados/services/fabrica_da_celula.py:187-188`, `app/pautas/services.py:174-182`)
- A guarda de idempotência faz consulta seguida de criação sem trava local; o modelo não tem unicidade marcador/âncora. Duas chamadas concorrentes podem ambas observar ausência e escrever. Serialização externa ou duplicação ocorrida: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:180-190`, `app/pautas/models.py:123-131`)

## 31/10 13:37 · sessao c6c1828c7027 · - · - · origem=Ronald (tmux)

Não encontrei perda nos cortes **desta volta**:

- **PARADA:** RED e três sentidos continuam em `CLAUDE.md:442-443`, equivalentes a `41b4d89d:CLAUDE.md:76-84`.
- **RAIAS:** `CLAUDE.md:484` recupera exatamente as condições “não de leitor novo” e “avais de hoje não cumpridos” da base `41b4d89d:CLAUDE.md:611-612`.
- **CODEX:** eliminou apenas repetição dos apelidos: `CODEX-SONDA-LEITURA` permanece em `CLAUDE.md:524` e `app/docs/PISTA.md:98`; `SUITE-ANTES-DO-OPUS`, em `CLAUDE.md:515`, `CLAUDE.md:549` e `app/docs/PISTA.md:113-114`.
- **PLACAR:** exercício, duas condições de zero, rótulo, mostrar sem fechamento e manutenção da L-095 permanecem em `CLAUDE.md:565-570`, equivalentes à base `41b4d89d:CLAUDE.md:515-523`.
- **RELATO:** “mesma pergunta”, declaração em `JUIZES_DO_PRAZO` e frase citando a regra continuam conjuntamente em `CLAUDE.md:607` e `CLAUDE.md:624-627`, equivalentes à base `41b4d89d:CLAUDE.md:658-663`.
- **PONTUALIDADE:** nesta volta apenas juntou linhas; ponteiro em `CLAUDE.md:601`, conteúdo preservado em `app/docs/LEIS.md:116`, correspondente à base `41b4d89d:CLAUDE.md:593-600`.
- **DINHEIRO:** responsabilidade do admin pelo backup/renome permanece em `CLAUDE.md:577`, correspondente à base `41b4d89d:CLAUDE.md:535`.

## 31/10 13:37 · sessao 16a8af0f3457 · - · - · origem=Ronald (tmux)

- Os quatro testes novos são `test_RED_contexto_460...`, `test_RED_contexto_480...`, `test_RED_contexto_700...` e `test_porta_escrever...`; os três primeiros delegam ao mesmo helper, e o quarto chama `escrever` diretamente. (`app/chamados/tests/test_f1c4_3_emissores.py:265`, `:284`, `:287`, `:290`, `:295`)

- Uma mutação local que derruba os quatro é, em `app/pautas/services.py:176`, substituir apenas o primeiro argumento `texto` da tupla por `texto[:500 - len(_ctx) - 4]`. Isso reintroduz o corte antigo do corpo, mantendo o novo corte do contexto. (`app/pautas/services.py:174`, `:176`)

| Teste | Efeito dessa mutação | [nao verificado]
|---|---|
| Contexto 460 | Guarda somente 36 caracteres do corpo; preserva o marcador, mas mutila a tag completa, fazendo a segunda passada criar outra pauta. O desempacotamento `(p,)` encontra duas. (`app/pautas/services.py:176`; `app/chamados/services/fabrica_da_celula.py:181`, `:182`; `app/chamados/tests/test_f1c4_3_emissores.py:278`) |
| Contexto 480 | Guarda somente 16 caracteres do corpo; perde o marcador completo, e `_pautas()` não encontra as pautas criadas. (`app/pautas/services.py:176`; `app/chamados/tests/test_f1c4_3_emissores.py:193`, `:278`) |
| Contexto 700 | O corpo integrado tem 154 caracteres; `texto[:-204]` resulta vazio, perdendo o marcador e fazendo falhar o desempacotamento. (`app/chamados/services/fabrica_da_celula.py:164`, `:189`; `app/chamados/services/declaracao_texto.py:44`; `app/chamados/tests/test_f1c4_3_emissores.py:278`, `:291`) |
| Porta direta | O corpo tem 78 caracteres; a iteração 100 permanece válida, mas a iteração 460 reduz o corpo a 36 e falha no `startswith` exigido. (`app/chamados/tests/test_f1c4_3_emissores.py:299`, `:300`, `:306`; `app/pautas/services.py:176`) |

- Na execução normal, todos entram no bloco novo: o corpo integrado tem 154 caracteres, portanto `_sobra=342`; o direto tem 78, portanto `_sobra=418`; todos os contextos simulados são não vazios. O literal `'[DIA x] '` tem oito caracteres, então os contextos têm exatamente o comprimento anunciado. (`app/chamados/services/fabrica_da_celula.py:189`; `app/chamados/tests/test_f1c4_3_emissores.py:274`, `:299`, `:301`; `app/pautas/services.py:174`, `:175`)

- Entretanto, os três testes de integração continuam passando se `app/pautas/services.py:175` virar `if False:`: não exigem contexto nem `[DIA ` no resultado; exigem somente marcador, tag, tamanho, unicidade e ausência de perguntas. O teste direto detecta esse bypass. (`app/chamados/tests/test_f1c4_3_emissores.py:278`, `:279`, `:280`, `:281`, `:282`, `:306`)

- Os quatro também aceitam substituir `_ctx[:_sobra]` por `_ctx[:7]`, anexando apenas `[DIA x]`; o teste direto verifica somente esse prefixo, não a extensão correta do contexto. (`app/pautas/services.py:176`; `app/chamados/tests/test_f1c4_3_emissores.py:279`, `:306`)

- Nenhum dos quatro cobre contexto vazio, corpo contendo `[DIA `, `T>=496`, `_sobra<=0` ou corpo maior que 500; retirar o `[:500]` final não os derruba. O nome “nunca o corpo” não vale universalmente: o service continua cortando qualquer corpo acima de 500. (`app/chamados/tests/test_f1c4_3_emissores.py:274`, `:299`, `:300`, `:301`; `app/pautas/services.py:168`, `:174`, `:175`, `:182`)

- As falhas descritas foram derivadas por leitura e simulação isolada de strings; execução efetiva dos testes mutados: [não verificado]. [nao verificado]

## 31/10 13:36 · sessao 9315cc53a28b · LEI-AKITA-UNIVERSO-DO-PUSH · verificador · origem=Code

**O portão ainda tem brechas:** `origin/main` ausente produz verde vazio, e o `pre-push` não chama este selo. (`bin/tests/test_lei_akita.sh:93`, `bin/tests/test_lei_akita.sh:101`, `bin/tests/test_lei_akita.sh:153`; `bin/pre-push.sh:12–19`)

1. **O universo não é literalmente `origin/main..HEAD`.** É esse conjunto limitado à primeira parentalidade e ao piso do corte. (`bin/tests/test_lei_akita.sh:94–101`)

   - **`HEAD == origin/main`:** retorna vazio corretamente; significa ausência de commits pendentes, não prova de julgamento anterior. (`bin/tests/test_lei_akita.sh:101–107`)
   - **`origin/main` inexistente — falha:** o Git falha, mas o helper descarta status e stderr; stdout vazio vira `[]`, seguido de `PADRAO_FIM`, aceito pelo shell. Portanto cala commits que deveria julgar. (`bin/tests/test_lei_akita.sh:92–107`, `bin/tests/test_lei_akita.sh:143–153`)
   - **Referência desatualizada:** não há sincronização nessa função. Se estiver atrasada, poderá rejulgar commits já publicados; ela conhece a referência local, não o estado atual do servidor. (`bin/tests/test_lei_akita.sh:92–101`)
   - **Merge `--no-ff`:** só julga o merge; mensagens defeituosas dos commits da raia ficam fora. Essa política está declarada no comentário, mas difere do range literal completo. (`bin/tests/test_lei_akita.sh:97–104`)
   - **Outra brecha no merge:** `git show --name-only` pode omitir arquivos incorporados sem conflito; assim, `n/a` pode passar embora o pouso integre negócio. A decisão depende justamente dessa lista. Essa fragilidade já existia antes da mudança de universo. (`bin/tests/test_lei_akita.sh:83–85`, `bin/tests/test_lei_akita.sh:103–104`)
   - **Corte na raiz:** tratado corretamente: sem pai, `pisos=[]`, evitando revisão inválida. O próprio caso plantado usa essa configuração. (`bin/tests/test_lei_akita.sh:99`, `bin/tests/test_lei_akita.sh:127–130`)

2. **Quem chama e quando.** O chamador operacional encontrado é o glob de `bin/regua.sh`, que executa os selos e decide pelo exit code. Porém seu atalho `JA VERDE` termina antes desse glob e compara arquivos `.py`/`.html`, sem considerar mensagens dos commits ou posição de `origin/main`. (`bin/regua.sh:33–35`, `bin/regua.sh:84–94`, `bin/regua.sh:138–145`)

   **`bin/pre-push.sh` não chama `test_lei_akita.sh`.** Chama individualmente outros dois selos e declara expressamente que o loop de `bin/tests/` não roda ali. (`bin/pre-push.sh:12–19`)

   Há caminhos concretos que deixam o commit novo escapar: o molde roda a régua antes do commit; depois commita e chama novamente sem `--force`, permitindo o atalho sobre o verde anterior; então faz push. O molde de autocura repete a sequência. (`bin/molde_fatia/cadeia.sh:51`, `bin/molde_fatia/cadeia.sh:68–85`; `bin/molde_autocura/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:65–82`)

   O integrador também roda `regua.sh --force` antes de `_subir`, que cria os commits e depois empurra. (`app/core/integrador_lote.py:375`, `app/core/integrador_lote.py:701`, `app/core/integrador_lote.py:719–739`)

   **Não encontrei chamada automática do selo depois do push nesses fluxos.** O passo explicitamente posterior, `pos_push.sh`, faz fetch e atualiza TICKETS, sem chamar a régua. A brecha encontrada é julgamento pré-commit seguido de ausência no hook. (`bin/push.sh:6–8`; `bin/pos_push.sh:18–39`; `bin/pre-push.sh:12–19`)

   A atualidade efetiva de `origin/main` em cada execução histórica está **[nao verificado]**.

3. **O MORDE cobre uma história linear mínima:** corte-raiz → publicado ruim → novo ruim, exigindo exatamente o hash do novo. As conclusões abaixo são por análise da asserção; executar o caso integral criaria arquivos e repositório temporário. (`bin/tests/test_lei_akita.sh:122–140`)

   | Mutante | Resultado pelo caso plantado |
   |---|---|
   | Retirar `^origin/main` | **Morre:** acusa também o publicado. (`bin/tests/test_lei_akita.sh:101`, `bin/tests/test_lei_akita.sh:132–139`) |
   | Calar `julgar` ou devolver universo vazio | **Morre:** perde o novo obrigatório. (`bin/tests/test_lei_akita.sh:104–106`, `bin/tests/test_lei_akita.sh:139`) |
   | Aplicar `^corte^` mesmo na raiz | **Morre:** revisão inválida resulta em vazio. (`bin/tests/test_lei_akita.sh:93`, `bin/tests/test_lei_akita.sh:99`, `bin/tests/test_lei_akita.sh:139`) |
   | Julgar somente `HEAD` | **Sobrevive:** existe apenas um commit novo. (`bin/tests/test_lei_akita.sh:132–139`) |
   | Remover o piso | **Sobrevive:** o fixture já tem corte-raiz. (`bin/tests/test_lei_akita.sh:99`, `bin/tests/test_lei_akita.sh:129–130`) |
   | Remover `--first-parent` | **Sobrevive:** não há merge no fixture. (`bin/tests/test_lei_akita.sh:101`, `bin/tests/test_lei_akita.sh:127–136`) |
   | Continuar ignorando erros Git | **Sobrevive:** não se planta referência ausente nem erro de comando. (`bin/tests/test_lei_akita.sh:125–136`) |

   O juiz compartilhado rejeita o termo inventado quando recebe o commit; a garantia incompleta está na seleção e na execução pelo portão. (`bin/tests/test_lei_akita.sh:88–89`, `bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:110–118`)

4. **A LÁPIDE 72 está correta nos termos e na correspondência principal com as curas.** (`app/docs/LAPIDES.md:785–786`)

   **`ac721607`: `Event-driven architecture` existe literalmente.** O encerramento passa pelo consumidor de eventos, enquanto o reconciliador periódico atua como vigia sem escrita; corresponde à descrição da lápide. (`app/docs/GLOSSARIO.md:16`; `app/chamados/reconciliador.py:1304–1309`; `app/ponto/management/commands/reconciliar_perguntas_orfas.py:21–35`; `app/docs/LAPIDES.md:785`)

   **`2cd634cb`: `Architectural Invariants + Fitness Functions` existe literalmente.** A cura move `pre_fechamento` de 05:15 para 05:25 e ajusta o vizinho declarado; preserva a recusa de cron que não cabe, fazendo a configuração satisfazer o invariante existente. (`app/docs/GLOSSARIO.md:14`; `app/config/crons.py:214–219`, `app/config/crons.py:252–258`, `app/config/crons.py:387`; `app/docs/LAPIDES.md:786`)

5. **Não há allowlist dos dois hashes nem juiz novo:** eles aparecem como história; a decisão continua em `julgar`, compartilhada pelo universo e pelo MORDE. As exigências de padrão conhecido e restrição de `n/a` já pertencem à L-123. (`bin/tests/test_lei_akita.sh:53–56`, `bin/tests/test_lei_akita.sh:73–90`, `bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:118`; `app/docs/LEIS.md:144`)

   **`LEI_AKITA_R` passa a controlar toda a raiz auditada**, inclusive documentos, código e histórico; o default continua sendo `/home/ronald/saas-hasner`, independentemente do diretório de execução. A régua não define essa variável na chamada. (`bin/tests/test_lei_akita.sh:17`, `bin/tests/test_lei_akita.sh:28–46`, `bin/tests/test_lei_akita.sh:60–65`, `bin/tests/test_lei_akita.sh:143`; `bin/regua.sh:140`)

   **O contrato de saída permanece:** os marcadores Python são consumidos pelo próprio shell; a régua usa o exit code e imprime a saída na falha. O `lei_akita: OK` não informa quantos commits foram julgados, portanto não distingue julgamento efetivo de universo vazio. (`bin/tests/test_lei_akita.sh:145–163`; `bin/regua.sh:140–143`)

## 31/10 13:36 · sessao 9d8875003f72 · - · - · origem=Ronald (tmux)

3. **Não há efeito adicional identificado.** O cabeçalho mantém a precedência de `PISTA.md` e abre exceção expressa somente para CODEX, PARADA E AUTONOMIA, DINHEIRO, RAIAS E POUSO e RELATO consolidados (`app/docs/PISTA.md:8-12`; antes: `e8d0cb0b:app/docs/PISTA.md:8-10`).

Os 17 blocos conferem com os hashes registrados, incluindo suas condições e scripts; a mudança está fora dos blocos ancorados (`app/docs/PISTA.md:16`; `bin/tests/pista_blocos.ref:1-17`; extração em `bin/tests/test_claude_md_regras_preservadas.sh:47-52`).

**Nenhum leitor da frase antiga foi encontrado em `bin/` ou `.claude/`.** O leitor identificado verifica nomes, hashes e ponteiros; não interpreta nem compara a frase de precedência (`bin/tests/test_claude_md_regras_preservadas.sh:43-78`). Os hooks configurados executam outros arquivos (`.claude/settings.json:9`, `:20`, `:29`, `:40`, `:44`, `:53`, `:57`, `:68`).

O selo integral não foi executado porque cria arquivos temporários; os hashes foram conferidos em memória (`bin/tests/test_claude_md_regras_preservadas.sh:89-112`; `bin/tests/pista_blocos.ref:1-17`).

## 31/10 13:36 · sessao 09345b96188f · - · - · origem=Ronald (tmux)

Inventário dos leitores de `Pauta.texto` em produção na cópia:

| Sítio | O que lê/procura |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:180-182` | Prefixo `LIMBO EM COMPETENCIA EXPORTADA`; depois substring exata `[motivo:<m>]` em todo texto, para deduplicar motivos. |
| `app/pautas/semanal.py:199` | Prefixo `<marcador> <semana ISO>`, âncora competência. |
| `app/pautas/semanal.py:228` | Prefixo do marcador para fechar pautas vivas da família. |
| `app/pautas/semanal.py:245-248` | Substring `n=<contadores separados por vírgula>` no texto da semana anterior. |
| `app/pautas/management/commands/pauta_dia_em_aberto.py:108` | Prefixo `[DIA EM ABERTO]`; âncora competência (`:104-108`). |
| `app/ferias/management/commands/pautas_de_ferias.py:149-156` | Prefixo de `MARCADORES[codigo]`: `[FERIAS/PA-VENCIDO]`, `[FERIAS/PA-VENCENDO]`, `[FERIAS/CADASTRO]` (`:30-33`). |
| `app/folha/management/commands/pre_fechamento.py:91-98` | Prefixo `[PRE-FECHAMENTO]` e prefixo mais específico com a data de hoje (`:29,93`). |
| `app/chamados/services/competencia_trancada.py:211` | Prefixo `[TRANCA]`, âncora competência. |
| `app/chamados/services/passivo_validado.py:159-164` | Prefixo `[PASSIVO-VALIDADO] col<id> AAAA-MM` (`:44`). |
| `app/chamados/services/passivo_validado.py:254-256` | Prefixo `[PASSIVO-VALIDADO-FORA] col<id> AAAA-MM` (`:48`). |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:190-191` | Prefixo `[ORFA-ENTERRADA]` (`:32`). |
| `app/core/management/commands/alarme_arvore.py:16` | Prefixo `ARVORE VERMELHA` (`:11`). |
| `app/core/management/commands/alarme_esteira.py:15` | Prefixo `ESTEIRA <chave>:` (`:10`). |
| `app/core/services/crons_quebrados.py:53-54` | Prefixo `[CRON QUEBRADO] <nome> (empresa <emp ou ->)` (`:15`). |
| `app/ponto/services/autorizacao_he_periodo.py:240-242` | Prefixo `[HE-AUTORIZADA-EM-EXPORTADA]` (`:124`), cabeças vivas em competência. |
| `app/ponto/services/pautas_dos_deslogados.py:65-66` | Prefixo `[APP-401]` (`:26`); chave restante vem da âncora, não do texto (`:68`). |
| `app/ponto/services/afastado_avisa.py:62-65` | Prefixo formado pelos primeiros 40 caracteres de `TEXTO`; âncora colaborador. |
| `app/ponto/services/ausencia.py:1022-1025` | Prefixo `[A4/SEM-DOCUMENTO]`; âncora colaborador. |
| `app/ponto/services/ausencia.py:1212-1215` | Prefixo `[A1-FERIAS/AG#<id>]`; âncora colaborador. |
| `app/ponto/services/cobertura.py:241-243` | Prefixo `[COBERTURA-A-CONFIRMAR]` (`:207`); âncora dia. |
| `app/ponto/services/pautas_do_esmeril.py:63-65` | Prefixo `[ESMERIL `; extrai a ação até o primeiro `]`; âncora posto. |
| `app/ponto/services/pautas_dos_discordantes.py:165-167` | Prefixo `[DISCORDANTE]` (`:37`), depois `_chave_do_texto` pega todo o trecho anterior ao primeiro `:` (`:96-100`); âncora competência (`:39`). |
| `app/pautas/views.py:40,47` | Serializa integralmente cabeça e respostas; sem interpretar conteúdo. |
| `app/api/views_mensageria.py:1324-1328` | Serializa integralmente texto da pauta na API de mensageria. |
| `app/templates/core/_copiloto.html:405,424` | Exibe cabeça e respostas por `textContent`; sem interpretar marcador/contexto. |
| `app/templates/colaboradores/detalhe.html:119` | Exibe texto com `truncatechars:180`: comprimento afeta somente a apresentação. |
| `app/pautas/management/commands/suporte_hasner.py:111,113` | Imprime integralmente cabeça e respostas no CLI. |

A única busca executável pelo trecho literal `[DIA ` está na **entrada** de `escrever`, para decidir se acrescenta contexto; não é um parser da pauta persistida (`app/pautas/services.py:163-169`). A construção do contexto coloca data/nome/matrícula/marcos/batidas/acusação/URL no corpo, mas não há consumidor de produção encontrado que extraia esses campos de `Pauta.texto`; os consumidores acima apenas os apresentam (`app/pautas/services.py:415-425`; `app/pautas/views.py:40`; `app/templates/core/_copiloto.html:405`; `app/templates/colaboradores/detalhe.html:119`).

A troca pode mudar a deduplicação do limbo porque preserva o prefixo e as tags que o corte anterior podia remover; o leitor busca ambos no corpo inteiro e não depende do contexto (`app/chamados/services/fabrica_da_celula.py:180-182`; `app/pautas/services.py:170-182`). O leitor de cobertura também recebe textos de dia, mas só procura seu prefixo; todos os demais leitores de negócio citados filtram outras âncoras, logo não passam pelo ramo de contexto alterado (`app/ponto/services/cobertura.py:241-243`; `app/pautas/services.py:168`).

Defeito associado: a nova justificativa “o dia o DP reabre pela ancora” não funciona pelo clique do painel: `ROTA` contempla somente `colab`, `chamado` e `posto`; `dia` cai no aviso de que não possui tela própria (`app/pautas/services.py:173`; `app/templates/core/_copiloto.html:564-574`). O URL textual do calendário fica no fim do contexto e pode desaparecer no novo corte; o painel também o renderiza como texto simples, sem link (`app/pautas/services.py:174-176,423-425`; `app/templates/core/_copiloto.html:405`). A ausência de rota é preexistente; a alteração passa a depender dessa rota ao permitir contexto ausente (`app/templates/core/_copiloto.html:564-574`; `app/pautas/services.py:173-176`).

Fora da aplicação de produção, a infraestrutura de sombra inventaria `Pauta.texto` para mascaramento: preserva marcador inicial entre colchetes e calcula hash do restante; não é leitor de negócio (`bin/sombra_regras_pessoais.py:24,29-33,81-84`; `bin/sombra_mascara_pessoal.py:44`).

## 31/10 13:35 · sessao 65da1b509851 · - · - · origem=Ronald (tmux)

Universo e MORDE, somente leitura:

- **Não é literalmente `origin/main..HEAD`**: usa `--first-parent` e exclui ancestrais do pai do corte; commits da segunda parentalidade e anteriores ao piso ficam fora. Isso é explicitamente descrito e implementado em `bin/tests/test_lei_akita.sh:94-101`. Ambos os filtros já existiam no pai; a extração da função e `^origin/main` são a mudança do commit. `bin/tests/test_lei_akita.sh:91-107` [comparação Git executada; diff sem arquivo de evidência].
- **HEAD igual a origin/main:** universo vazio esperado, sem falhas de padrão; a função retorna `[]` e imprime `PADRAO_FIM`, aceito pelo shell. `bin/tests/test_lei_akita.sh:101-107`, `143-153`.
- **Origin inexistente:** `rev-list` falha, mas o helper usa apenas stdout e ignora status/stderr; stdout vazio vira `[]`, depois `PADRAO_FIM`. Portanto o selo cala indevidamente o universo. `bin/tests/test_lei_akita.sh:92-93`, `101-107`, `143-153`.
- **Origin atrasado:** commits já publicados, mas posteriores à referência local, voltam ao julgamento; não há atualização da referência na função. `bin/tests/test_lei_akita.sh:92-101`.
- **Corte na raiz:** o teste de existência do pai deixa `pisos=[]`, evitando a revisão inválida `^<raiz>^`. O próprio fixture usa corte-raiz. `bin/tests/test_lei_akita.sh:99`, `127-130`.
- **Merge `--no-ff`:** só a mensagem do merge é julgada, por causa de `--first-parent`. Além disso, `git show --name-only` usa o diff combinado, podendo ocultar código de negócio integrado; `n/a` então pode passar com lista de arquivos só documentais. `bin/tests/test_lei_akita.sh:97-104`, `83-85`. Confirmei em leitura Git: `c7ea207a` mostra só BACKLOG/PROMPTS/RELATO nessa chamada, enquanto o diff contra primeiro pai inclui crons.py, juizes.py, models.py etc. [nao verificado por arquivo:linha para a saída Git]. Essa fragilidade já existe no pai.

**MORDE do universo:** o fixture tem apenas `corte-raiz → publicado ruim → novo ruim`, exigindo exatamente o hash do novo. `bin/tests/test_lei_akita.sh:127-139`.

Por análise do código, ele mata:

- Retirar `^origin/main`: acusa também o publicado. `bin/tests/test_lei_akita.sh:101`, `132-139`.
- Calar o juiz ou retornar universo vazio: perde o novo esperado. `bin/tests/test_lei_akita.sh:104-106`, `139`.
- Impor `^corte^` mesmo quando corte é raiz: Git falha, universo fica vazio e diverge do esperado. `bin/tests/test_lei_akita.sh:93`, `99-101`, `129-139`.

Sobrevivem:

- Remover `--first-parent`: fixture é linear. `bin/tests/test_lei_akita.sh:101`, `127-136`.
- Remover o piso: fixture já tem corte-raiz, portanto `pisos=[]`. `bin/tests/test_lei_akita.sh:99`, `129-130`.
- Julgar somente HEAD: há só um novo commit. `bin/tests/test_lei_akita.sh:132-139`.
- Escolher o último corte em vez do primeiro: há só uma ocorrência introduzida. `bin/tests/test_lei_akita.sh:94`, `129`.
- Ignorar falhas Git: fixture não planta referência ausente nem falhas de comando. `bin/tests/test_lei_akita.sh:125-136`.

Não executei o MORDE integral nem mutações em disco, pois ele cria arquivos e repositório temporário. `bin/tests/test_lei_akita.sh:122-136`.

## 31/10 13:34 · sessao 881fe507cad9 · - · - · origem=Ronald (tmux)

- **O selo não é chamado pelo pre-push.** O hook chama individualmente `test_lei_protege_sitio.sh` e `test_achado_por.sh`, e seu comentário registra que o loop de `bin/tests/` não roda ali; seu arsenal posterior roda testes Django (`bin/pre-push.sh:12`, `:16`, `:19`, `:155`, `:182`, `:194`). A cópia em `app/core/hooks/pre-push.sh` também só chama suas guardas próprias e a suíte (`app/core/hooks/pre-push.sh:7`, `:54`, `:57`, `:87`). O wrapper instalado delega ao `bin/pre-push.sh` (`bin/hooks.sh:56`).

- **Chamador real encontrado: o glob da régua**, que executa cada selo com Bash, consome seu exit code, imprime a saída somente na falha e aborta (`bin/regua.sh:138`, `:140`, `:142`, `:143`). **A régua pode sair antes desse glob** com `JA VERDE`, comparando apenas a impressão de arquivos `.py`/`.html` e status anterior; mensagens de commit e posição de `origin/main` não participam dessa condição (`bin/regua.sh:25`, `:33`, `:84`, `:86`, `:93`). Embora calcule `ARVORE_FP`, o atalho da própria régua não o compara (`bin/regua.sh:45`, `:86`).

- **Falha concreta do fluxo:** o molde roda a régua antes do commit, faz o commit e chama a régua mais duas vezes, todas sem `--force`; essas chamadas posteriores podem atalhar sobre o verde anterior. O push vem depois e seu hook não chama este selo (`bin/molde_fatia/cadeia.sh:51`, `:68`, `:70`, `:81`, `:85`; `bin/pre-push.sh:12`). O molde de autocura tem a mesma sequência (`bin/molde_autocura/cadeia.sh:48`, `:65`, `:67`, `:78`, `:82`).

- **Integrador também pode publicar commit que o selo ainda não viu:** sua suíte chama `bin/regua.sh --force`, porém `_subir` faz os commits depois e em seguida dá push (`app/core/integrador_lote.py:375`, `:691`, `:701`, `:719`, `:739`). Há ainda caminho de empurrar commits locais antes de rodar o lote, protegido somente pelo hook existente (`bin/integrador.sh:36`, `:44`, `:52`).

- **Estado de `origin/main`:** os chamadores acima usam a referência local existente; não há atualização acoplada ao selo, cuja consulta usa diretamente `^origin/main` (`bin/tests/test_lei_akita.sh:93`, `:101`). Sua atualidade efetiva no momento de cada execução histórica: **[nao verificado]**.

- **Não encontrei chamada automática do selo pós-push nos fluxos rastreados.** Nas cadeias todas as chamadas da régua antecedem o push; depois há deploy (`bin/molde_fatia/cadeia.sh:81`, `:85`, `:89`; `bin/molde_autocura/cadeia.sh:78`, `:82`, `:86`). O wrapper `push.sh` chama `pos_push.sh` após sucesso, mas esse script só atualiza `origin`, reescreve derivados de TICKETS e commita TICKETS (`bin/push.sh:6`, `:8`; `bin/pos_push.sh:18`, `:19`, `:20`, `:29`, `:30`). Uma régua posterior pode legitimamente encontrar universo vazio; isso não comprova que o commit foi julgado antes (`bin/tests/test_lei_akita.sh:101`, `:107`, `:148`).

- **Consumidores e contrato:** `LEI_AKITA_R` passa a controlar toda a raiz do selo, inclusive suas verificações iniciais e leitura dos termos; os chamadores encontrados não definem essa variável (`bin/tests/test_lei_akita.sh:17`, `:28`, `:35`, `:45`, `:60`, `:65`; `bin/regua.sh:140`). A régua depende do exit code, não do texto `lei_akita: OK` (`bin/regua.sh:140`). Os marcadores Python continuam internos, interpretados pelo shell do próprio selo: `PADRAO_FALHA`, `PADRAO_FIM`, `PADRAO_SEM_CORTE`, `MORDE_OK`, `MORDE_FALHOU` (`bin/tests/test_lei_akita.sh:145`, `:147`, `:148`, `:151`, `:152`, `:153`, `:156`). A saída pública final continua sendo `lei_akita: OK (...)` ou `lei_akita: N falha(s)` (`bin/tests/test_lei_akita.sh:160`, `:163`).

## 31/10 13:34 · sessao 4f29e9379c5b · - · - · origem=Ronald (tmux)

A LAPIDE 72 está correta quanto aos termos e à realização principal dos diffs históricos, conferidos via `git diff ac721607^1 ac721607` e `git show 2cd634cb`:

- **Event-driven architecture** existe literalmente em `app/docs/GLOSSARIO.md:16`. O diff de `ac721607` move o encerramento para o consumidor de eventos e transforma o cron em vigia: execução atual em `app/chamados/reconciliador.py:1307`, escrita pela porta em `app/chamados/reconciliador.py:1099`, vigia em `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29` e troca para `--alarme` em `app/config/crons.py:1469`. A descrição de `app/docs/LAPIDES.md:785` corresponde.
- **Architectural Invariants + Fitness Functions** existe literalmente em `app/docs/GLOSSARIO.md:14`. O diff de `2cd634cb` muda `VIZINHO_SOMBRA` e o horário de `pre_fechamento`, ambos 05:15 → 05:25; o estado resultante está em `app/config/crons.py:219` e `app/config/crons.py:387`. A recusa existente permanece em `app/config/crons.py:252`; a alteração faz a configuração satisfazer o invariante. A descrição de `app/docs/LAPIDES.md:786` corresponde.
- O primeiro commit também realiza fonte única ao extrair `expiracao_da_aberta`, lida por `turno_aberto_vivo` e pelo motivo do dia encerrado; isso não torna falso o padrão principal nomeado na lápide (`app/ponto/turnos.py:1991`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2099`; termo existente em `app/docs/GLOSSARIO.md:11`).
- O diff de `d8912301` contra o pai altera somente o selo e acrescenta a lápide. Não modifica `julgar`: continua uma única função, chamada pelo universo e pelos casos plantados (`bin/tests/test_lei_akita.sh:73`, `bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:118`). Os hashes publicados aparecem em comentários/documentação, sem condição de dispensa (`bin/tests/test_lei_akita.sh:53`, `app/docs/LAPIDES.md:785`, `app/docs/LAPIDES.md:786`).
- As exigências verificadas já pertencem à L-123: termo conhecido, restrição de `n/a` e conferência humana contra o diff (`app/docs/LEIS.md:144`, `app/docs/PLANO-PADROES.md:53`, `app/docs/PLANO-PADROES.md:58`). Não há novo termo no glossário nem nova lei no diff; o novo universo implementa o aval fornecido e está descrito em `bin/tests/test_lei_akita.sh:51`.

## 31/10 13:34 · sessao 7cfa3b05c897 · GRADE-FOLGA · verificador · origem=Code

A cura liga o `status` à precedência, mas não unifica todo o resultado do calendário. Restam cobertura incompleta do teste e comprovação pendente do pouso v3. (`app/colaboradores/services/calendario.py:803`, `:890`; `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:55`; `app/ponto/services/cartorio.py:173`.)

1. **Autoridade e caminho restante do calendário.** Sem o argumento, `fatos_do_periodo` carrega as folgas da pessoa, seleciona a válida e escreve `fatos['folga_lancada']`; também decide `faixa` pela precedência. O calendário lê ambos para classificar o dia. (`app/ponto/precedencia.py:281`, `:358`, `:392`, `:408`; `app/colaboradores/services/calendario.py:795`, `:803`.)

   Porém, `eh_trabalho` continua vindo do vínculo **ativo**, alimentado por `_folgas_mes`, e é devolvido sem o ajuste local de `_classificar`. No próprio cenário novo, pode sair `status='folga'` e `eh_trabalho=True`: a célula declara trabalho e o modelo a lê primeiro. (`app/colaboradores/services/calendario.py:772`, `:890`, `:116`; `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:40`; `app/escala/models.py:1071`.)

   Esse é o caminho restante por foto: sem célula, o fallback considera a data presente na foto como folga, sem distinguir origem. A diferença entre previsão bruta e status final, isoladamente, não demonstra regressão: `resolver_dia_oferecido` também exige um status que peça decisão, excluindo `folga`. (`app/escala/models.py:1073`, `:1252`, `:1280`; `app/colaboradores/services/calendario.py:181`, `:189`.)

   Os contadores acompanham o **status**; badges continuam lendo feriados, rubricas lavradas e chamados. Portanto, não passam todos a consumir `faixa`, nem isso demonstra por si um badge incorreto. (`app/colaboradores/services/calendario.py:937`, `:944`, `:838`, `:845`, `:898`.)

2. **Custo: sem N+1 novo de FolgaDia.** O carregador executa uma consulta antes do laço diário; a seleção acessa campos `_id` já carregados, sem buscar relações. (`app/ponto/precedencia.py:207`, `:281`, `:337`, `:238`.)

   Há duas ressalvas: o legacy recarrega por **segmento**, e o caminho completo já consulta o nome de cada feriado por data. Nenhuma dessas consultas nasce do seletor novo. (`app/escala/utils.py:985`, `:995`, `:770`, `:1119`; `app/ponto/precedencia.py:407`; `app/core/feriados.py:100`.)

3. **O RED tem mordida, mas a cobertura é parcial.** Pelo rastreio do código, a mão presa ao velho muda de `falta` para `folga`; a asserção nova distingue esses resultados. Execução integrada vermelho/verde: **[não verificado]**. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:54`, `:55`; `app/colaboradores/services/calendario.py:116`, `:133`.)

   O controle do calendário velho mede uma exclusão válida, mas fraca: `assertNotEqual(..., 'folga')` também aceita `sem_dados`, `previsto` ou outro status errado, embora a docstring prometa `falta`. Ele já passava com a foto restrita ao ativo. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:10`, `:60`, `:62`; `app/colaboradores/services/calendario.py:593`.)

   **Falta testar o outro defeito citado pela própria cura:** folga de calendário do ativo quando a geradora do dia é outro vínculo. A fixture atual sempre põe o ativo como geradora; também não cobra contadores, badges ou o contrato de `eh_trabalho`. (`app/colaboradores/services/calendario.py:608`; `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:41`, `:55`, `:62`.)

4. **Triagem da volta 2.** Não encontrei dependência nova que transforme `o_dia_ja_e_folga` ou `_folga_avulsa` em bloqueadores desta lavra: continuam na idempotência/veto da porta; o caminho alterado usa diretamente os helpers novos. (`app/ponto/services/declarar_dia.py:90`; `app/ponto/precedencia.py:501`; `app/escala/utils.py:1115`.)

   A troca de ordem alcança células/órfãs e a leitura da ausência parcial, portanto “nenhum `tipo_dia` mudou” não comprova ausência de efeito nos demais campos. Impacto financeiro adicional ao anexo: **[não verificado]**. (`app/escala/utils.py:1794`, `:1127`, `:1202`.)

5. **Classes estruturais e pouso.** O predicado novo está centralizado e é chamado pela grade e precedência; nesses trechos não aparece novo escritor persistente, relógio ou aquisição de trava. (`app/ponto/precedencia.py:236`, `:358`; `app/escala/utils.py:1115`, `:1124`.) O calendário conserva classificação própria, já reconhecida como migração pendente no registro; não é juiz criado por este diff. (`app/core/juizes.py:169`; `app/colaboradores/services/calendario.py:93`.)

   **Não daria o pouso como comprovado:** a v3 já está declarada, mas exige antes ensaio `--apply` com dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0`. Esses resultados não constam do material fornecido: **[não verificado]**. Isso é pré-requisito sem comprovação, não prova de que um pouso indevido ocorreu. (`app/ponto/services/cartorio.py:110`, `:173`.)

## 31/10 13:33 · sessao 11088239546e · CLAUDE-MD-RACIONAL · leitura · origem=Code

1. **Não no histórico fornecido.** No commit **`8ce1e8c2`**, o texto removido de `CLAUDE.md` e adicionado a `app/docs/PISTA.md` passa diretamente de (4) para (6), literalmente:

   > escopo, mesmo fora do diff. (6) ALERTA no ESTADO

   Referências: `hist_triagem.txt:148` e `hist_triagem.txt:347`.

2. **Sim, no aval de 30/10 10:3x em `PROMPTS.md`**, consta literalmente:

   > (5) retroativo: NOTURNO (a) 1,5 / (b) 2,3 / (b) parcial 4,6; V-FASE refeita pelo item (2)

   Referência: [app/docs/PROMPTS.md:1671](/home/ronald/codex_ws/a7b66d14d0bcf92e6f71f940cef6b3fdf6063dd7/app/docs/PROMPTS.md:1671). Esse item não aparece nos blocos dos dois arquivos exibidos no commit `8ce1e8c2` (`hist_triagem.txt:137–149` e `hist_triagem.txt:336–348`). **Commit que registrou esse aval em `PROMPTS.md`: [nao verificado]**; o anexo não o identifica.

## 31/10 13:32 · sessao 6e629cbc1fa1 · - · - · origem=Ronald (tmux)

Não encontrei bloqueador novo no seletor ou nos builders, nestes pontos:

- **Carga de folgas sem N+1:** uma consulta traz todas as `FolgaDia` da pessoa na janela; o laço usa apenas campos já carregados (`app/ponto/precedencia.py:207-209`). Seletor e predicado são puros e acessam `calendario_origem_id`/`escala_colaborador_id`, sem acessar relações (`app/ponto/precedencia.py:213-240`). A precedência chama o carregador antes do laço diário (`app/ponto/precedencia.py:281-282,337-358`).
- **Nuance:** o caminho completo ainda consulta nomes de feriado por data: `app/ponto/precedencia.py:407` chama `nome_do_feriado`, que executa `_qs(data,data)` (`app/core/feriados.py:98-102,27-29`). É anterior ao diff fornecido; não decorre da carga de `FolgaDia`.
- **Builders:** ambos chamam o mesmo carregador e seletor (`app/escala/utils.py:770,1115-1128,1794`). No legacy, a carga acontece por segmento, pois cada segmento chama o builder individual (`app/escala/utils.py:985-995`); portanto não prometer “uma única consulta por calendário inteiro” para esse builder.
- **Autoridade:** o vínculo do dia continua vindo do juiz registrado (`app/core/juizes.py:1255`; `app/escala/utils.py:969-973,1256-1268`). A precedência consulta esse mesmo juiz (`app/ponto/precedencia.py:351`) e ambos usam o mesmo predicado de elegibilidade da folga (`app/ponto/precedencia.py:358`; `app/escala/utils.py:1124`). Não identifiquei segundo juiz introduzido nesses trechos.
- **Contrato antigo de `folgas_lancadas`:** não há outro chamador no código passando o argumento; a única ocorrência restante é a assinatura em `app/ponto/precedencia.py:261`. A mudança de valor unitário para lista não deixa chamador antigo demonstrável na cópia.
- **Fila da volta 2:** `o_dia_ja_e_folga` responde à idempotência da porta, escolhendo o vínculo e consultando sua foto (`app/ponto/services/declarar_dia.py:73-91`). `_folga_avulsa` alimenta veto de lançamento (`app/ponto/precedencia.py:161-166,501-509`). A lavra alterada não chama nenhum deles: usa diretamente os novos helpers (`app/ponto/precedencia.py:281-282,358`; `app/escala/utils.py:1115-1128`). Não encontrei dependência nova que torne esses achados bloqueadores desta cura.
- **Ordem:** folga agora precede todos os overlays de ausência no builder por turno (`app/escala/utils.py:1794-1797`). Ausência continua cobrindo folga e falta continua sem substituí-la (`app/escala/utils.py:1174-1178`). Há consequência possível sobre dados auxiliares: folga limpa células e órfãs (`app/escala/utils.py:1126-1128`), e ausência parcial mede seu período usando essas células (`app/escala/utils.py:1202`). O anexo já declara diferença não-tipo medida; outras vítimas ou efeito financeiro são [não verificado].
- **Classes estruturais:** os novos helpers e overlay não escrevem banco nem leem relógio (`app/ponto/precedencia.py:195-240`; `app/escala/utils.py:1103-1128`). Não introduzem ordem de trava nesses trechos. A repetição das leituras de relógio dos builders individuais é preexistente e recebe `ref` explícito no caminho por segmentos (`app/escala/utils.py:658,964,995`), enquanto o builder por turno deriva a data de `agora` recebido (`app/escala/utils.py:1242`).

Execução real, números de consultas em runtime e impacto além do dado anexado: [não verificado].

## 31/10 13:32 · sessao 693245cde62f · - · - · origem=Ronald (tmux)

- **Pouso v3 sem prova suficiente nos anexos:** a própria declaração exige ensaio `--apply` anterior ao pouso com dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0`; esses três números não foram fornecidos. Cumprimento do pré-requisito: **[não verificado]**. Não confundir ausência de prova com prova de pouso indevido — `app/ponto/services/cartorio.py:173-175`. A reconstrução das projeções também integra o pouso — `app/docs/LEIS.md:142`.

- **Horizonte e relógio:** `versao_para` usa declaração literal e o juiz existente `janela_atual`; não introduz leitura de relógio ou decisão pela exportação — `app/ponto/services/cartorio.py:188-207`. O teste atualizado verifica o leitor vivo, conservando 09 na versão 1, elevando 10 à versão 3 e deixando 08 fora — `app/ponto/tests/test_o219_versao_da_regra.py:78-93`.

- **Segundo juiz:** não identifiquei nova cópia do predicado no trecho alterado: `fatos_do_dia`, registrado em `app/core/juizes.py:160-161`, delega ao período em `app/ponto/precedencia.py:255-257`; período e grade usam o mesmo seletor — `app/ponto/precedencia.py:358`, `app/escala/utils.py:1115`, `app/escala/utils.py:1124`. O predicado ficou num único local — `app/ponto/precedencia.py:236-240`.

- **Segundo escritor, relógio e ordem de trava:** o carregador faz leitura ORM e o overlay altera o resultado em memória; os trechos novos não adicionam escrita persistente, aquisição de trava ou relógio — `app/ponto/precedencia.py:205-210`, `app/escala/utils.py:1114-1128`.

- **Força dos testes:** os testes da grade verificam tanto a grade quanto a precedência para os cinco cenários e os dois builders; a docstring reconhece controles que já passavam — `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:21-25`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70-104`. O controle do calendário só exige `status != 'folga'`, portanto também aceita um status incorreto diferente de folga; não prova o `falta` prometido na docstring — `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:10`, `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:58-63`. Execução RED/verde: **[não verificado]**.

## 31/10 13:32 · sessao 5e1b6dd36cd1 · - · - · origem=Ronald (tmux)

Achado de incompletude: `eh_trabalho` ainda sai de caminho paralelo. O calendário calcula pelo vínculo ativo e `_folgas_mes` (`app/colaboradores/services/calendario.py:772`), enquanto `status` lê `faixa` e `fatos['folga_lancada']` (`app/colaboradores/services/calendario.py:795`, `app/colaboradores/services/calendario.py:803`). `_classificar` troca somente sua variável local `trabalho` para `False` (`app/colaboradores/services/calendario.py:111`, `app/colaboradores/services/calendario.py:116`); o dicionário devolve o `eh_trabalho` original (`app/colaboradores/services/calendario.py:890`).

No próprio caso novo, a célula declara `trabalha=True` (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:40`), e `eh_dia_trabalho` retorna esse valor antes de consultar folgas (`app/escala/models.py:1071`). Portanto, por leitura estática, a folga manual do velho produz `status='folga'` e `eh_trabalho=True`; o teste cobra exclusivamente o primeiro campo (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:55`). Execução vermelho/verde: [nao verificado].

A autoridade de `fatos['folga_lancada']` é `fatos_do_periodo`, pelo seletor comum (`app/ponto/precedencia.py:358`) e escrita booleana (`app/ponto/precedencia.py:408`); `faixa` vem da ordenação da precedência (`app/ponto/precedencia.py:392`, `app/ponto/precedencia.py:400`). O calendário passa a alcançar essa autoridade sem fornecer `folgas_lancadas` (`app/colaboradores/services/calendario.py:604`). Os contadores seguem os `status` já classificados (`app/colaboradores/services/calendario.py:937`, `app/colaboradores/services/calendario.py:944`); badges continuam lendo feriados e rubricas lavradas (`app/colaboradores/services/calendario.py:838`, `app/colaboradores/services/calendario.py:840`, `app/colaboradores/services/calendario.py:845`).

Restam `_folgas_mes` como alimentação de `folgas_foto` (`app/colaboradores/services/calendario.py:606`) e do cálculo paralelo de trabalho (`app/colaboradores/services/calendario.py:773`). O fallback por foto trata uma data presente como folga sem examinar sua origem (`app/escala/models.py:1252`, `app/escala/models.py:1280`, `app/escala/models.py:1288`).

O controle do calendário velho não demonstra a regressão corrigida: lança folga de calendário no vínculo velho, mas a foto antiga do calendário era a do ativo (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:60`, `app/colaboradores/services/calendario.py:593`). Além disso, apenas cobra `status != 'folga'`, aceitando inclusive estados diferentes do `falta` prometido na docstring (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:10`, `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:62`). Ele serve como controle negativo fraco; não cobre o caso citado no comentário da cura — calendário do ativo num dia cuja geradora é outra (`app/colaboradores/services/calendario.py:608`).

## 31/10 13:30 · sessao ebfc1284978e · F1C4-3-EMISSORES · verificador · origem=Code

O corte de texto pode destruir a chave de deduplicação da pauta; os testes atuais não exercitam esse limite (`app/pautas/services.py:171–177`; `app/chamados/tests/test_f1c4_3_emissores.py:218–257`). O inventário **completo** de `4236474c..49b05829` fica **[não verificado]**: a cópia não contém histórico Git nem esse comparativo; o anexo fornecido cobre `41b4d89d..49b05829` **[não verificado quanto à base pedida]**.

1. As mutações abaixo são conclusões por leitura estática; o resultado de sua execução está **[não verificado]**.

| Teste de histórico | Mutação mínima que o deixaria vermelho | O que distingue | [nao verificado]
|---|---|---|
| `test_RED_limbo_em_exportada_duas_vezes_uma_pauta` | Em `fabrica_da_celula.py:183`, trocar a seleção de motivos novos por `novos = motivos`: o segundo julgamento criaria outra pauta, contrariando a contagem final (`app/chamados/services/fabrica_da_celula.py:183–190`; `app/chamados/tests/test_f1c4_3_emissores.py:218–225`). | **Não detecta** restringir o histórico a `objects.vivas()`: a primeira pauta continua viva (`app/pautas/models.py:57–64`; `app/chamados/tests/test_f1c4_3_emissores.py:218–225`). |
| `test_RED_limbo_em_exportada_pauta_feita_nao_reabre` | Acrescentar `feito_em__isnull=True` à consulta histórica: a feita seria ignorada e outra pauta nasceria (`app/chamados/services/fabrica_da_celula.py:180–190`; `app/chamados/tests/test_f1c4_3_emissores.py:233–240`). | Detecta `objects.vivas()`, mas não a exclusão **somente das canceladas** (`app/pautas/models.py:57–64`; `app/chamados/tests/test_f1c4_3_emissores.py:227–241`). |
| `test_RED_limbo_em_exportada_pauta_cancelada_nao_reabre` | Acrescentar `cancelado_em__isnull=True`: a cancelada seria ignorada e outra pauta nasceria (`app/chamados/services/fabrica_da_celula.py:180–190`; `app/chamados/tests/test_f1c4_3_emissores.py:249–256`). | Detecta `objects.vivas()`, mas não a exclusão **somente das feitas** (`app/pautas/models.py:57–64`; `app/chamados/tests/test_f1c4_3_emissores.py:243–257`). |

Quanto às portas:

- **Feita:** substituir `marcar_feita_pelo_sistema` por um no-op, ou escrever `feito_em` apenas em memória, é detectado: o teste recarrega e exige o campo persistido. A viva e a cancelada não chamam essa porta (`app/chamados/tests/test_f1c4_3_emissores.py:218–251`; `app/pautas/services.py:264–266`).
- **Feita:** remover a escrita de `lido_em`, o recarregamento interno, `_trilha` ou `lavrar` passa por esses testes, desde que `feito_em` seja persistido; nenhuma dessas outras obrigações é afirmada (`app/pautas/services.py:264–268`; `app/chamados/tests/test_f1c4_3_emissores.py:233–241`).
- **Cancelada:** no-op, escrita apenas em memória ou escrita de outro timestamp são detectados pelo recarregamento e pela exigência de `cancelado_em` preenchido (`app/pautas/services.py:286–287`; `app/chamados/tests/test_f1c4_3_emissores.py:249–251`).
- **Cancelada:** remover autorização do autor, recusa de pauta feita, guarda contra cancelamento repetido, recarregamento interno, trilha ou contador passa nesses casos: a chamada acontece uma vez, pelo autor, numa pauta ainda não feita (`app/pautas/services.py:280–289`; `app/chamados/tests/test_f1c4_3_emissores.py:247–257`).

Há duas lacunas diretamente relacionadas ao histórico: retirar `pai__isnull=True` passa porque nenhum caso cria uma resposta; e limpar `feito_em`/`cancelado_em` no segundo julgamento também pode passar, pois os timestamps são conferidos **antes**, sem nova conferência do estado final (`app/chamados/services/fabrica_da_celula.py:180`; `app/chamados/tests/test_f1c4_3_emissores.py:218–257`).

2. Para o corte, sejam `C = len(_ctx)`, `T = len(texto_original)` e `e` a posição imediatamente após o marcador ou uma tag. O espaço calculado é **`496 − C`**, e ainda há um `[:500]` no momento de gravar (`app/pautas/services.py:169–177`).

Para `C ≤ 496`, o elemento inteiro sobrevive quando `496 − C ≥ e`. O marcador tem **30 caracteres**; portanto fica inteiro até **C=466** e é mutilado entre **467 e 496**. As tags vêm depois dele, então começam a desaparecer antes (`app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:189–190`; `app/pautas/services.py:171`).

| Conteúdo original | Último `C ≤ 496` que preserva marcador e todas as tags |
|---|---:|
| Uma tag `[motivo:orfao_14h]` | **447** |
| Uma tag `[motivo:saida_sem_entrada]` | **439** |
| Uma tag `[motivo:intervalo_saida]` ou `[motivo:intervalo_volta]` | **441** |
| As quatro tags acima juntas | **370** |

Os limites da tabela resultam dos nomes definidos em `app/chamados/services/declaracao_texto.py:42–46`, do formato em `app/chamados/services/fabrica_da_celula.py:189–190` e do corte em `app/pautas/services.py:171`.

**Acima de 496, o índice fica negativo.** `texto[:-1]` conserva tudo menos o último caractere; não significa “nenhum espaço disponível”. Para `C>496`, o tamanho conservado do original é `max(0, T + 496 − C)`, antes da concatenação e do corte final (`app/pautas/services.py:171–177`).

Isso torna a perda **não monotônica**. Para o texto com somente `[motivo:orfao_14h]`, cujo tamanho original calculado é 154, acontece o seguinte (`app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:189–190`; `app/pautas/services.py:171–177`):

| Comprimento do contexto | Marcador inteiro | Tag inteira |
|---|---|---|
| Até 447 | Sim | Sim |
| 448–466 | Sim | Não |
| 467–496 | Não | Não |
| 497–601 | Sim, reaparece | Sim, reaparece |
| 602–620 | Sim | Não |
| A partir de 621 | Não | Não |

Com **C=496**, o original vira `texto[:0]`; com **C=497**, vira `texto[:-1]`; nesse exemplo, somente a partir de **C=650** o índice negativo elimina o original inteiro (`app/pautas/services.py:171`; formato original em `app/chamados/services/fabrica_da_celula.py:189–190`).

O comprimento do contexto depende destes componentes:

| Componente | Limitação verificável |
|---|---|
| Nome | `CharField(max_length=200)`; incluído sem corte adicional (`app/colaboradores/models.py:320`; `app/pautas/services.py:419–420`). |
| Matrícula | `IntegerField`, convertido em texto, ou `'-'` (`app/colaboradores/models.py:302–305`; `app/pautas/services.py:420`). |
| Marcos | Até seis chaves canônicas; seus valores JSON são concatenados sem limite individual nessa função (`app/escala/models.py:1620–1647`; `app/pautas/services.py:406–410`). |
| Batidas | Todas as apuráveis da janela de 39 horas, sem teto de quantidade; hora formatada em cinco caracteres mais o tipo de um caractere (`app/pautas/services.py:411–413`; `app/ponto/models.py:24`; `app/ponto/turnos.py:2845–2852`). |
| Acusação | Veredito limitado a 20 caracteres, acrescido de todas as horas apagadas da ata, sem corte de quantidade ou tamanho nessa composição (`app/escala/models.py:1738`; `app/pautas/services.py:414–417`). |
| Data e link | Data formatada, rota com identificação do colaborador e parâmetros de mês/ano (`app/pautas/services.py:418–420`; `app/colaboradores/urls.py:24`). |

Logo, **não existe teto de 496 para `_ctx`**. Perder o marcador faz a consulta histórica ignorar a pauta inteira; perder uma tag faz aquele motivo não entrar em `ja`, permitindo escrevê-lo novamente (`app/pautas/services.py:392–420`; `app/chamados/services/fabrica_da_celula.py:180–190`). Incidência e comprimentos reais em produção: **[não verificado]**.

3. Não é possível certificar a lista completa de campos alterados desde `4236474c` **[não verificado]**. Este é o mapa verificável dos campos cujo uso muda no delta fornecido:

| Campo | Quem escreve | Quem lê no caminho alterado |
|---|---|---|
| `CelulaDia.trabalha` | Geração, regeneração e restauração pela porta (`app/ponto/portas/celula.py:590–615`; `:243–265`; `:532–538`). | `dia_sem_trabalho`, chamado pelos quatro emissores; o desvio para DP ainda testa `None` diretamente (`app/chamados/juizes.py:407–408`; `app/chamados/services/fabrica_da_celula.py:80`; `:153`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`). |
| `ata.tipo_dia` | Cartório monta; porta atribui a ata (`app/ponto/services/cartorio.py:567`; `app/ponto/portas/celula.py:767–768`). | Juiz único (`app/chamados/juizes.py:407–408`). |
| `ata.sem_ancora` | Grade produz e cartório copia (`app/escala/utils.py:729`; `:1514`; `:1701–1702`; `app/ponto/services/cartorio.py:570`). | Cluster e intervalo deixam de usá-lo como veto; passam ao juiz, que não consulta essa chave (`app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/chamados/juizes.py:407–408`). |
| `Pauta.ancora_tipo`, `ancora_id`, `destinatario_departamento` | Emissor fornece `dia`, `colaborador:data`, `dp`; `escrever` persiste (`app/chamados/services/fabrica_da_celula.py:175–186`; `app/pautas/services.py:172–177`). | Deduplicação histórica; listagem, chip e contador (`app/chamados/services/fabrica_da_celula.py:180–181`; `app/pautas/services.py:314–315`; `:328–342`). |
| `Pauta.texto`, marcador e tags | Emissor fabrica; porta acrescenta contexto e corta (`app/chamados/services/fabrica_da_celula.py:189–190`; `app/pautas/services.py:163–177`). | `startswith` reconhece marcador; substring reconhece cada motivo (`app/chamados/services/fabrica_da_celula.py:180–182`). |
| `Pauta.pai` | Emissor omite argumento, usando `None`; respostas fornecem o pai (`app/pautas/services.py:155`; `:177`; `:192`). | Guarda histórica exige cabeça (`app/chamados/services/fabrica_da_celula.py:180`). |
| `remetente_departamento`, `autor_user` | Autor de sistema e remetente `ti` fornecidos pelo emissor; resolução e persistência na porta (`app/chamados/services/fabrica_da_celula.py:185–186`; `app/pautas/services.py:157–177`). | Resposta consulta remetente/autor; cancelamento consulta autor (`app/pautas/services.py:188–192`; `:280–281`). |
| `feito_em`, `lido_em`, `cancelado_em` | Portas de leitura, fechamento humano, fechamento de sistema e cancelamento (`app/pautas/services.py:207`; `:239–241`; `:264–266`; `:286–287`). | `vivas()` usa os estados; a guarda nova deliberadamente consulta todo o histórico sem filtrá-los (`app/pautas/models.py:57–64`; `app/chamados/services/fabrica_da_celula.py:180–182`). |

Há também os campos E3 visíveis na cópia; sua inclusão no intervalo solicitado é **[não verificado]**:

- **`status_dia` e `status_rotulo`:** declarados no model e criados pela migration 0044; atribuídos por `_aplicar_status` e persistidos pela porta. O leitor público consulta ambos; a porta também os compara/conserva, e a fila D5 consulta `status_dia` (`app/escala/models.py:1784–1785`; `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13–21`; `app/ponto/portas/celula.py:634–642`; `:697–706`; `app/ponto/status_dia.py:171–183`; `app/ponto/services/cartorio.py:726`).
- **`fatos_do_status`:** argumento transitório, com `tipo_dia`, `previsto_min`, `tem_batida`, `isento`, `cobertura_parcial` e `folga_humana`; produzido pelo helper, entregue pelo cartório, encaminhado pelo model e consumido pela regra da porta (`app/ponto/status_dia.py:124–149`; `app/ponto/services/cartorio.py:1563–1564`; `app/escala/models.py:1794–1796`; `app/ponto/portas/celula.py:693–698`).

Além do truncamento, encontrei estes defeitos ou limites:

- **Contradição com o aval literal (a):** o código permite pergunta nova em exportada quando `trabalha=True`, e o teste exige esse nascimento. Isso contraria “Pergunta NOVA […] só em competência ABERTA”; autorização posterior está **[não verificado]** (`app/chamados/services/fabrica_da_celula.py:153–160`; `app/chamados/tests/test_f1c4_3_emissores.py:212–216`).
- **Desvio para DP contornável:** o ramo de fio mudo seleciona chamado vivo sem filtro de `trabalha`, agrega os motivos e chama `perguntar` diretamente. Um limbo exportado com chamado vivo e motivo ainda sem pergunta pode alcançar esse caminho sem consultar a pauta; o lote está configurado com `--apply` (`app/chamados/management/commands/disparar_perguntas_competencia.py:155–168`; `:220–244`; `:276–289`; `app/chamados/services/fabrica_da_celula.py:114–126`; `app/config/crons.py:1464–1466`). Ocorrência real: **[não verificado]**.
- **Deduplicação vulnerável à concorrência:** consulta e criação não estão protegidas conjuntamente; duas execuções podem ler ausência antes de inserir. A transação começa dentro de `escrever`, e não existe unicidade por dia/motivo no modelo; serialização externa e ocorrência real estão **[não verificado]** (`app/chamados/services/fabrica_da_celula.py:180–190`; `app/pautas/services.py:152–177`; `app/pautas/models.py:123–132`).
- **COL717 não está provado:** os testes usam colaborador sintético, sem reproduzir seu chamado existente; preservação do único chamado e ausência de porta nova para COL717 estão **[não verificado]** (`app/chamados/tests/test_f1c4_3_emissores.py:57–65`). Também não se deve equiparar achado de intervalo a chamado novo: essa emissão está aposentada; cluster, por sua vez, chama a porta de abertura (`app/ponto/management/commands/detectar_intervalo_ausente.py:149–157`; `app/ponto/services/cluster_espurio.py:87–105`).

## 31/10 13:28 · sessao 2e02f2c7fa32 · - · - · origem=Ronald (tmux)

O intervalo completo `4236474c..49b05829` não é reconstruível nesta cópia sem histórico Git nem diff dessa base; atribuir cada campo a esse intervalo fica **[não verificado]**. O `emis5_diff.txt` fornecido cobre outra base (`41b4d89d`); nele há novas leituras/escritas de campos existentes, sem alteração de models/migrations **[não verificado quanto ao intervalo pedido]**.

Mapa verificável dos campos alcançados pelo emissor:

| Campo | Quem escreve na cópia | Quem lê no caminho alterado |
|---|---|---|
| `CelulaDia.trabalha` | Geração em `app/ponto/portas/celula.py:590-615`; regeneração em `app/ponto/portas/celula.py:243-265`; restauração em `app/ponto/portas/celula.py:532-538`. | Juiz `app/chamados/juizes.py:407-408`, chamado pelos quatro emissores em `app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/materializacao.py:160`, `app/ponto/services/cluster_espurio.py:28`, `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; o ramo novo também distingue `None` diretamente em `app/chamados/services/fabrica_da_celula.py:153`. |
| `ata.tipo_dia` | Construído em `app/ponto/services/cartorio.py:567`, entregue à porta em `app/ponto/services/cartorio.py:1557-1559`, atribuído em `app/ponto/portas/celula.py:767-768`. | Juiz em `app/chamados/juizes.py:407-408`. |
| `ata.sem_ancora` | Origem na grade em `app/escala/utils.py:729`, `app/escala/utils.py:1514`, `app/escala/utils.py:1701-1702`; copiado para ata em `app/ponto/services/cartorio.py:570`. | Os novos gates de cluster/intervalo leem o juiz em `app/ponto/services/cluster_espurio.py:28` e `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; esse juiz não consulta `sem_ancora` em `app/chamados/juizes.py:407-408`. |
| `Pauta.ancora_tipo`, `ancora_id`, `destinatario_departamento` | Novo chamador fornece `dia`, `colab:data`, `dp` em `app/chamados/services/fabrica_da_celula.py:175-186`; porta persiste em `app/pautas/services.py:172-177`. | Guarda histórica em `app/chamados/services/fabrica_da_celula.py:180-181`; chip em `app/pautas/services.py:328-329`; listagem e contador em `app/pautas/services.py:314-315`, `app/pautas/services.py:340-342`. |
| `Pauta.texto`: marcador e tags `[motivo:…]` | Fabricados em `app/chamados/services/fabrica_da_celula.py:189-190`; contexto e truncamento em `app/pautas/services.py:163-177`. | Marcador pelo `startswith`, tags por substring em `app/chamados/services/fabrica_da_celula.py:180-182`. |
| `Pauta.pai` | Novo emissor omite argumento, portanto usa `None` de `app/pautas/services.py:155`, persistido em `app/pautas/services.py:177`; respostas passam pai em `app/pautas/services.py:192`. | Dedup exige cabeça em `app/chamados/services/fabrica_da_celula.py:180`. |
| `Pauta.remetente_departamento`, `autor_user` | Novo emissor fornece autor e remetente informado em `app/chamados/services/fabrica_da_celula.py:185-186`; resolução em `app/pautas/services.py:157`; gravação em `app/pautas/services.py:173-177`. | Autoria autoriza cancelamento em `app/pautas/services.py:280-281`; remetente e autor orientam resposta em `app/pautas/services.py:188-192`. |
| `feito_em`, `lido_em`, `cancelado_em` | Fecho da máquina em `app/pautas/services.py:264-266`; cancelamento em `app/pautas/services.py:286-287`; marcação humana em `app/pautas/services.py:239-241`. | Definição de viva em `app/pautas/models.py:57-64`; a nova guarda histórica não os filtra em `app/chamados/services/fabrica_da_celula.py:180-182`. |

Apêndice E3 — candidatos visíveis na cópia, cuja inclusão no intervalo `4236474c..49b05829` é **[não verificado]**:

- **`status_dia` e `status_rotulo`:** campos em `app/escala/models.py:1784-1785`; criação explícita por `AddField` em `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13-21`. A porta atribui ambos em `app/ponto/portas/celula.py:701-706` e persiste por `salvar_e_emitir` em `app/ponto/portas/celula.py:634-642`; entradas pela lavra em `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:752-775`.
- **Leitores das colunas E3:** leitor público em `app/ponto/status_dia.py:171-183`; comparação/conservação interna em `app/ponto/portas/celula.py:697`, `app/ponto/portas/celula.py:703`; fila D5 lê `status_dia` em `app/ponto/services/cartorio.py:726`. O próprio módulo declara que as telas ainda aguardam E4 em `app/ponto/status_dia.py:8-9`.
- **`fatos_do_status`:** é argumento/dicionário transitório, não coluna; produzido em `app/ponto/status_dia.py:124-149`, entregue pelo cartório em `app/ponto/services/cartorio.py:734-735`, `app/ponto/services/cartorio.py:1046-1047`, `app/ponto/services/cartorio.py:1563-1564`, encaminhado pelo model em `app/escala/models.py:1794-1796`, consumido pela porta/regra em `app/ponto/portas/celula.py:693-698`. Suas chaves são `tipo_dia`, `previsto_min`, `tem_batida`, `isento`, `cobertura_parcial`, `folga_humana`, em `app/ponto/status_dia.py:146-149`; leitura pela regra em `app/ponto/status_dia.py:70-121`.
- **Homônimos de apresentação:** `status_rotulo` em `app/escala/views.py:55` e `app/colaboradores/services/calendario.py:884` é chave de payload derivada, não leitura da coluna `CelulaDia.status_rotulo`; a coluna é lida explicitamente pelo leitor em `app/ponto/status_dia.py:182`.

Outro defeito verificável: a deduplicação nova não é atômica entre consulta e criação: lê histórico em `app/chamados/services/fabrica_da_celula.py:180-183` e depois chama a escrita em `app/chamados/services/fabrica_da_celula.py:185`; a transação começa somente dentro de `escrever`, em `app/pautas/services.py:152-153`, e as constraints de `Pauta` não impõem unicidade desse fato em `app/pautas/models.py:112-132`. Assim, duas execuções concorrentes podem ler ausência e criar duas pautas; ocorrência em produção **[não verificado]**.

Outro caminho que merece constar: o ramo de fio mudo do lote não filtra `trabalha` em `app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`, agrega seus alvos em `app/chamados/management/commands/disparar_perguntas_competencia.py:244` e chama `perguntar` diretamente em `app/chamados/management/commands/disparar_perguntas_competencia.py:276-289`, sem passar pelo novo encaminhamento para Pauta de `app/chamados/services/fabrica_da_celula.py:153-157`. Já o caminho principal do lote exclui limbo pelo filtro `trabalha=True` em `app/chamados/management/commands/disparar_perguntas_competencia.py:76`; portanto ele também não mede a nova elegibilidade do evento. Incidência real nesses universos **[não verificado]**.

## 31/10 13:28 · sessao 04f7ce2e5937 · - · - · origem=Ronald (tmux)

Achados adicionais, por leitura estática:

- O aval fornecido diz que pergunta **nova só nasce em competência aberta**, mas a implementação restringe o desvio para DP a `trabalha is None`; com `True`, chama `perguntar` mesmo em exportada. O teste exige essa pergunta nascer: `app/chamados/services/fabrica_da_celula.py:153-160`; `app/chamados/tests/test_f1c4_3_emissores.py:212-216`. O cumprimento desse aval está contradito por código e teste; eventual autorização posterior [não verificado].

- O novo bloqueio pode ser contornado pelo caminho **fio mudo do lote**: seleciona chamados vivos com célula, sem filtro de `trabalha`, calcula motivos, e entrega diretamente a `perguntar`, que não consulta competência nem histórico de Pauta. Logo, limbo exportado com chamado vivo e fato ainda sem pergunta pode voltar ao app por esse caminho: `app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`, `:181-226`, `:276-289`; `app/chamados/services/fabrica_da_celula.py:114-126`. O lote está configurado com `--apply`: `app/config/crons.py:1464-1466`. A primeira via do lote filtra `trabalha=True`, portanto não atribuir a ela o limbo: `app/chamados/management/commands/disparar_perguntas_competencia.py:76`.

- A idempotência nova não é protegida contra concorrência dentro da função: consulta o histórico, decide `novos` e depois cria; duas execuções podem ler histórico vazio antes das duas inserções. A transação de `escrever` começa somente na criação, e o modelo não declara unicidade por dia/motivo: `app/chamados/services/fabrica_da_celula.py:180-190`; `app/pautas/services.py:152-177`; `app/pautas/models.py:123-132`. Ocorrência efetiva em produção e eventual serialização externa [não verificado].

- Não confundir resultado de `furos_intervalo` com abertura de chamado: o comando mantém apenas reconciliação de legado e sua emissão está aposentada por `continue`: `app/ponto/management/commands/detectar_intervalo_ausente.py:122-139`, `:149-157`. Já cluster pode abrir um chamado por dia encontrado: `app/ponto/services/cluster_espurio.py:23-35`, `:87-105`.

- O efeito específico sobre COL717 não está demonstrado pelo dado fornecido [não verificado]. Não se pode concluir que uma nova porta efetivamente nasceu ou que o único chamado foi preservado apenas pelos testes apresentados [não verificado]. [nao verificado]

## 31/10 13:28 · sessao 59f02be287a2 · - · - · origem=Ronald (tmux)

Análise estática; execução dos mutantes [não verificado].

| Teste de histórico | Mutação mínima da produção que o deixaria vermelho | Limite da prova | [nao verificado]
|---|---|---|
| `test_RED_limbo_em_exportada_duas_vezes_uma_pauta` | Trocar `novos = [m for m in motivos if m not in ja]` por `novos = motivos`: o segundo julgamento escreveria outra pauta e `len(...) == 1` falharia. (`app/chamados/services/fabrica_da_celula.py:183-190`; `app/chamados/tests/test_f1c4_3_emissores.py:218-225`) | Trocar a consulta histórica por `Pauta.objects.vivas().filter(...)` **não** é detectado por este teste: a primeira pauta permanece viva; `vivas()` admite pautas sem `feito_em` e sem `cancelado_em`. (`app/chamados/tests/test_f1c4_3_emissores.py:218-225`; `app/pautas/models.py:57-64`) |
| `test_RED_limbo_em_exportada_pauta_feita_nao_reabre` | Acrescentar `feito_em__isnull=True` à consulta da deduplicação: a pauta feita deixaria de fornecer os motivos já vistos; surgiria outra e a contagem falharia. (`app/chamados/services/fabrica_da_celula.py:180-190`; `app/chamados/tests/test_f1c4_3_emissores.py:233-240`) | Também detectaria a troca por `objects.vivas()`, mas não prova preservação de `feito_em` após o segundo julgamento: o timestamp é conferido **antes**, e depois só se conferem retorno, quantidade e perguntas. (`app/pautas/models.py:57-64`; `app/chamados/tests/test_f1c4_3_emissores.py:234-241`) |
| `test_RED_limbo_em_exportada_pauta_cancelada_nao_reabre` | Acrescentar `cancelado_em__isnull=True` à consulta da deduplicação: a cancelada seria ignorada, outra pauta nasceria e a contagem falharia. (`app/chamados/services/fabrica_da_celula.py:180-190`; `app/chamados/tests/test_f1c4_3_emissores.py:249-256`) | Também detectaria `objects.vivas()`, mas não prova preservação de `cancelado_em` após o segundo julgamento: a conferência do timestamp antecede esse julgamento. (`app/pautas/models.py:57-64`; `app/chamados/tests/test_f1c4_3_emissores.py:250-257`) |

Quanto às mutações das portas:

- **`marcar_feita_pelo_sistema`:** o teste da feita detecta um `return pauta` sem escrita, ou escrita somente no objeto em memória: ele recarrega do banco e exige `feito_em` preenchido. O teste da viva e o da cancelada não chamam essa porta. (`app/chamados/tests/test_f1c4_3_emissores.py:218-235`, `:243-251`; `app/pautas/services.py:248-266`)
- **`marcar_feita_pelo_sistema`:** remover a escrita de `lido_em`, o `refresh_from_db` interno, `_trilha` ou `lavrar` não é detectado pelas asserções desses testes, desde que `feito_em` continue persistido. O próprio teste faz seu recarregamento. (`app/pautas/services.py:264-268`; `app/chamados/tests/test_f1c4_3_emissores.py:233-241`)
- **`cancelar`:** o teste da cancelada detecta um no-op, escrita somente em memória ou troca de `cancelado_em` por outro campo: após recarregar, exige o timestamp correto. Os outros dois testes não chamam essa porta. (`app/chamados/tests/test_f1c4_3_emissores.py:218-241`, `:245-251`; `app/pautas/services.py:286-287`)
- **`cancelar`:** remover a autorização do autor, a recusa de pauta feita, a guarda de cancelamento repetido, o recarregamento interno, `_trilha` ou `lavrar` não é detectado por esses casos: há uma única chamada, pelo autor, em pauta ainda não feita, e as asserções não cobrem trilha ou contadores. (`app/pautas/services.py:280-289`; `app/chamados/tests/test_f1c4_3_emissores.py:247-257`)

Outras lacunas concretas desses testes: [nao verificado]

- Retirar `pai__isnull=True` da deduplicação passa pelos três casos: nenhum cria resposta com `pai`; portanto não há caso que diferencie cabeça de resposta. (`app/chamados/services/fabrica_da_celula.py:180`; `app/chamados/tests/test_f1c4_3_emissores.py:218-257`; `app/pautas/services.py:184-192`)
- Os testes não impedem uma regressão que **reabra a própria pauta**, limpando seu timestamp no segundo julgamento: exigem uma pauta ao final, mas não revalidam o estado final dela. (`app/chamados/tests/test_f1c4_3_emissores.py:234-241`, `:250-257`)
- O teste inicial de encaminhamento verifica apenas a presença do prefixo textual `'[motivo:'`; não exige o motivo completo e correto. Os casos históricos cobrem repetição do mesmo cenário, sem introduzir um segundo motivo posteriormente. (`app/chamados/tests/test_f1c4_3_emissores.py:199-205`, `:218-257`)

## 31/10 13:25 · sessao 2bdf6996f2ed · CLAUDE-MD-RACIONAL · verificador · origem=Code

A consolidação perde cláusulas explícitas, incluindo o registro do juiz de prazo e o prazo do DIFF da troca de motor. Portanto, não sustenta integralmente “fusão sem mudar regra”. (pai:CLAUDE.md:563–567, 661–662; novo:CLAUDE.md:594–595, 624–626)

Nas referências abaixo, **pai = `41b4d89d`** e **novo = `e25a0940`**.

1. **Cláusulas removidas sem reprodução nos consolidados**

| Trecho removido, com linha do pai | O que entrou no lugar |
|---|---|
| “sem fechamento, **MOSTRAR o numero do motor rotulado** — zero seria perda de informacao”. (pai:CLAUDE.md:520–521) | Mantém somente os dois critérios negativos do placar: nenhum número sem rótulo e nenhuma derivação própria. **Sem destino para a obrigação positiva de mostrar o número.** (novo:CLAUDE.md:567–570) |
| A pergunta antiga “quem **CHAMA** o motor”, mediante AST, “morre”. (pai:CLAUDE.md:522–523) | Conserva “EXERCICIO, nao CAMINHO”, mas **não reproduz a determinação sobre a pergunta antiga do selo**. (novo:CLAUDE.md:567–570) |
| Handoff deve conter HEAD/origin, status, ORDEM VIVA/item em curso **lidos do `hook_stop_fila1`**, PAREI, avais, processos vivos e próximo passo. (pai:CLAUDE.md:610–612) | Mantém comando, hooks e teto de 60 linhas. **Sem destino para a lista obrigatória e para o leitor canônico da fila.** (novo:CLAUDE.md:508–512) |
| Novo juiz de prazo: escrever a frase **e declarar em `JUIZES_DO_PRAZO`**. (pai:CLAUDE.md:661–662) | Mantém frase, citação da regra e número intocado. **Sem destino para a inscrição em `JUIZES_DO_PRAZO`.** (novo:CLAUDE.md:624–626) |
| “backup e renomeacao sao do admin”. (pai:CLAUDE.md:535) | Mantém nome canônico e armazenamento das versões. **Sem destino para essa atribuição de responsabilidade.** (novo:CLAUDE.md:589–593) |
| DIFF **ANTECIPADO** permanece em exportada **e troca de motor**, uma vez como impacto. (pai:CLAUDE.md:563–567) | Preserva “ANTES” na exportada; na troca de motor mantém uma vez/impacto, **sem destino para a antecipação**. (novo:CLAUDE.md:582–583, 594–595) |
| “Sem criterio escrito no aval, vale a L-009 como hoje.” (pai:CLAUDE.md:579) | Mantém condições do aval de critério e parada por violação, mas **não reproduz essa cláusula residual**; a L-009 aparece genericamente no cabeçalho. (novo:CLAUDE.md:575–576, 586–588) |

Outras cláusulas também desapareceram dos consolidados, **mas continuam expressas em fontes mantidas**:

| Trecho removido | Texto novo e preservação externa |
|---|---|
| Declaração de autoridade no CORTES deve vir “**citando este aval**”. (pai:CLAUDE.md:483) | A citação desaparece do item consolidado; continua em `PISTA.md`. (novo:CLAUDE.md:462–464; app/docs/PISTA.md:52–53) |
| Codex: “tarefa **so por delegacao do Claude**”. (pai:CLAUDE.md:504) | Exige wrapper, mas não reproduz quem delega; a restrição continua em `PISTA.md`. (novo:CLAUDE.md:521–528; app/docs/PISTA.md:72–74) |
| “o pendente **VOLTA se o contador sair do esperado**”. (pai:CLAUDE.md:653–654) | Diz apenas “frase de reversao”; gatilho e consequência continuam na L-118. (novo:CLAUDE.md:621; app/docs/LEIS.md:139) |
| Moratória identifica `contratos_estruturais`, seu juiz, nascimento por corte literal e extinção automática em 20/20. (pai:CLAUDE.md:644–647) | Resume para “Ate 20/20”; os detalhes continuam na L-117 referenciada. (novo:CLAUDE.md:455; app/docs/LEIS.md:138) |

Essa distinção importa porque a própria PISTA declara que seu texto literal prevalece sobre o resumo divergente. (app/docs/PISTA.md:8–12)

2. **Diferenças de condição, alcance ou prazo**

| Antes | Depois | Diferença |
|---|---|---|
| Bug novo que muda o desenho: parar e pôr no topo dos **PENDENTES**. (pai:CLAUDE.md:459–460) | Vai ao topo do **RELATO**, como pergunta de desenho com números; a obra para e as outras seguem. (novo:CLAUDE.md:458–459) | Troca o destino do registro e explicita uma parada limitada à obra. |
| Sonnet para leitura: **somente LIMITE 75 ou falha**, com motivo registrado. (pai:CLAUDE.md:506) | Agrupa “leitura/sonda” e admite também “**ou motivo na linha do DESPACHO**”. (novo:CLAUDE.md:532–534) | Alarga textualmente a exceção de leitura. A exceção por motivo já existia para **sonda**, enquanto leitura permanecia restrita a limite/falha. (app/docs/PISTA.md:92–93, 159–160) |
| Juiz de prazo existente deve responder à **MESMA pergunta**. (pai:CLAUDE.md:659–660) | Pergunta a juiz de `JUIZES_DO_PRAZO` “**se ja houver**”. (novo:CLAUDE.md:623–624) | Omite a condição explícita de correspondência da pergunta. |
| Não numerar nem propor **lei nova por iniciativa própria**. (pai:CLAUDE.md:645–646) | “nao numera nem propoe **lei**”. (novo:CLAUDE.md:455) | O enunciado fica mais restritivo: desaparecem “nova” e “por iniciativa própria”. A L-117 mantém a ressalva. (app/docs/LEIS.md:138) |
| Decidir quando uma **obra registrada** entra como complemento de outra. (pai:CLAUDE.md:481–482) | Decidir quando um **achado** entra como complemento. (novo:CLAUDE.md:461–462) | Troca o objeto da delegação. |
| DIFF **antecipado** também na troca de motor. (pai:CLAUDE.md:563–567) | DIFF da troca uma vez como impacto, sem “antes”. (novo:CLAUDE.md:594–595) | Perde o prazo explícito, embora preserve finalidade e frequência. |

Há ainda uma **ambiguidade nova no B3**: “reprovacao do verificador final” entra como gatilho sem explicitar a **terceira** reprovação. O código referenciado exige três; portanto, a referência ao B3 permite recuperar o limiar, mas o enunciado isolado parece abranger qualquer reprovação. (novo:CLAUDE.md:458–459; bin/disjuntor.py:11–12, 68)

3. **Scripts que realmente leem estrutura ou conteúdo do `CLAUDE.md`**

| Script | O que procura | Efeito do diff |
|---|---|---|
| `bin/tests/test_lei_akita.sh` | Título `## 0. LEI-AKITA`, recorte até seção 1, 13 itens numerados e frase `LEI-AKITA: origem=`. (bin/tests/test_lei_akita.sh:28–31) | Alvos preservados. (novo:CLAUDE.md:14, 45, 63, 78) |
| `bin/tests/test_suite_sh.sh` | Título literal `## 3. TESTES - COMANDO CANONICO`, comando seguinte e presença de `bin/suite.sh`. (bin/tests/test_suite_sh.sh:22–32, 37–42) | Título e comando preservados. (novo:CLAUDE.md:170–171) |
| `bin/tests/test_segredo_fora_do_argv.sh` | Sequência proibida `-e DB_PASSWORD=`, com filtro para linhas contendo crase. (bin/tests/test_segredo_fora_do_argv.sh:19, 33) | A busca continua aplicável; não depende dos títulos ou das regras fundidas da 7b. (bin/tests/test_segredo_fora_do_argv.sh:33) |
| `bin/tests/test_claude_md_regras_preservadas.sh` | Nomes de regra/`L-NNN`, referências a LAPIDES, nomes e âncoras da PISTA, hashes dos blocos da PISTA. (bin/tests/test_claude_md_regras_preservadas.sh:43–77) | Os apelidos e ponteiros continuam presentes; o `.ref` acrescenta `PENDENTE-SAI` e `PRE-VERIFICADOR`. (novo:CLAUDE.md:436–443, 477–482, 514–520, 603–606; novo:bin/tests/claude_md_regras.ref:76, 85) |

**Nenhuma dependência textual desses leitores ficou sem alvo pela fusão**, conforme comparação estática dos predicados e documentos citados; execução integral dos selos: **[não verificado]**.

O selo de preservação **não prova equivalência semântica**: no `CLAUDE.md`, confere nomes e referências; a comparação literal por hash incide sobre `PISTA.md`. (bin/tests/test_claude_md_regras_preservadas.sh:57, 69–77)

Não entram nessa lista os testes de mera existência em `raia_agente.sh` e `test_raia_declarada_no_estado.sh`, nem o reconhecimento do nome de anexo no disjuntor. (bin/raia_agente.sh:29; bin/tests/test_raia_declarada_no_estado.sh:17; bin/disjuntor.py:772, 886–888)

4. **Sequência das letras e item (5)**

O trecho removido do **CLAUDE.md não trazia essa enumeração**: era um resumo com ponteiro para a PISTA, seguido da regra MODO-COMUM, classe `(d)`. (pai:CLAUDE.md:508–509)

No texto literal referido, a triagem já enumerava **`(a), (b), (c), (e)`**; o bloco MODO-COMUM seguinte definia **`(d)`**. O novo reúne essas cinco classes como **`(a), (b), (c), (e), (d)`**. **Nenhuma letra desapareceu.** (pai:app/docs/PISTA.md:122–125, 133–138; novo:CLAUDE.md:557–559)

O salto de **item `(4)` para `(6)`**, sem `(5)`, **já existia na PISTA**. Não foi uma supressão deste diff. Separadamente, o novo CODEX tem item **5, PRE-VERIFICADOR**. (pai e novo:app/docs/PISTA.md:128–130; novo:CLAUDE.md:544)

_1064 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
