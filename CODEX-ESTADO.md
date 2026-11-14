# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 15:06** · publicado em **14/11/2026 15:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 15:06 · sessao 645ea3ab9938 · - · - · origem=Ronald (tmux)

2. **PROCEDENTE — verde com guarda parcialmente inoperante.** A bateria de cinco casos exercita o hook plantado para `$R/bin`; o instalado e cada alternativo recebem apenas dois casos. Um hook que contenha a referência textual, recuse exclusivamente `Padrao Inventado Do Nada` e aceite o restante passa esses dois casos, inclusive os MORDE existentes, embora aceite `n/a (instrumento)` em `app/ponto`. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:81`, `:86–90`, `:105`, `:107–115`, numeração do diff v3.)

2. **PROCEDENTE — vermelho falso por mudança de localização.** `_roda` copia somente o hook para outro diretório. Um alternativo válido em `$wt/.githooks`, com `exec bash "$(dirname "$0")/../bin/commit_msg.sh" "$@"`, passa no teste textual, mas perde o destino ao ser copiado para `$T/alt.*/.git/hooks`. A recusa de mensagem sem linha então acusa uma instalação válida. Existência desse wrapper em produção: **[nao verificado]**. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:87`, `:90`.)

2. **PROCEDENTE — plantados herdam `core.hooksPath` global.** Os commits sintéticos não impõem seu próprio hooksPath; uma configuração global válida pode fazer o Git ignorar os hooks e mutantes gravados em `.git/hooks`, causando reprovação falsa do MORDE. Configuração global efetiva: **[nao verificado]**. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:59–66`, `:101–109`, numeração do diff v3.)

A varredura apresentada não escreve diretamente em `$wt`: consulta configuração e executa os casos em repositórios plantados; o wrapper novo determina `TOPO` pelo Git daquele plantado. Executar uma cópia do hook não restringe acessos absolutos do seu conteúdo. Efeito colateral efetivo em repositório real: **[nao verificado]**, pois os corpos dos alternativos não foram fornecidos. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:35–42`, `:87–90`; `bin/commit_msg.sh:19–29`, diff v3.)

## 14/11 15:05 · sessao 73a4d5d24f27 · - · - · origem=Ronald (tmux)

4. **PROCEDENTE — mapa de escritores/leitores e significado:**

- Escrita única das constantes: `app/ferias/models.py:251` (`FRACAO_MINIMA=14`) e `app/ferias/models.py:252` (`FRACOES_MAXIMAS=3`). O juiz lê a primeira como limiar da **maior** fração e a segunda como limite da quantidade de frações não nulas; não exige 14 de cada fração (`app/ferias/models.py:267-273`).
- Leitores adicionais: importação em `app/ferias/situacao.py:57`; mínimo nas mensagens em `app/ferias/situacao.py:234-235,346`; máximo na mensagem em `app/ferias/situacao.py:242`; mínimo no predicado de pré-aviso em `app/ferias/situacao.py:341`. O candidato acrescenta importação em `ferias/test_frac5_texto_agendar.py:13` e leitura no texto esperado em `ferias/test_frac5_texto_agendar.py:26-27` **do anexo**.
- A duração é de dias corridos, incluindo início e fim: `app/ferias/models.py:371,382-385`; reconstrução do fim por início + dias − 1 em `app/ferias/services.py:331,565`; histórico calcula fim − início + 1 em `app/ferias/services.py:524`; mensagens explicitam “dias corridos” em `app/ferias/situacao.py:233,345`.
- Leitura de frações antigas usa `dias` persistido, sem recalcular pelas datas: `app/ferias/situacao.py:115-122`; `app/ferias/models.py:362-374`. A fração atualmente validada tem dias recalculados e sua coerência conferida em `app/ferias/models.py:371,382-385`.
- `_trocar_janela` recebe datas e duração separadas, grava os três valores e não chama `clean`: `app/ferias/services.py:694,716,729`; seus wrappers estão em `app/ferias/services.py:736-745`. As views calculam o fim inclusivo em `app/ferias/views.py:507,718`. Coerência dos registros efetivamente persistidos: **[não verificado]**.

5. **PROCEDENTE — derivação paralela localizada:** o pré-aviso usa diretamente `int(dias) < FRACAO_MINIMA`, sem consultar o conjunto de frações nem `violacoes_de_fracionamento`; por isso uma segunda fração curta pode gerar aviso mesmo com outra ≥14. O texto manda verificar as demais e não recusa a operação (`app/ferias/situacao.py:341-346`); o aviso do período completo consulta o juiz (`app/ferias/situacao.py:223-242`).

O candidato compara o literal do template às constantes importadas; não passa a produzir o texto do template a partir do juiz (`ferias/test_frac5_texto_agendar.py:13,25-27` **do anexo**; `templates/ferias/agendar.html:10` **do anexo**).

## 14/11 15:05 · sessao 3d00bf154b5f · - · - · origem=Ronald (tmux)

Item 1 — **PROCEDENTE: remarcação normal aceita fracionamento que a criação recusa.** O formulário permite `min="1"` e chama `editar_agendamento`; a view chama `remarcar_agendamento`, que delega a `_trocar_janela`. Essa porta verifica estado e saldo, troca os dados e chama `save()`, sem `clean()`; o `save()` também não chama `clean()`. Portanto, uma única fração futura de 14 dias pode virar 2, satisfeitas as demais guardas; a criação dos mesmos 2 dias falha no `par1`. Evidências: `app/templates/ferias/_secao_colab.html:130`, `:135`; `app/ferias/views.py:723`; `app/ferias/services.py:707`, `:712`, `:716`, `:729`, `:745`; `app/ferias/models.py:374`, `:444`.

O texto proposto corresponde aos dois predicados do juiz para frações positivas: `max(fr) >= 14` e `len(fr) <= 3`; `[14,2]` passa e não existe mínimo de 5 nesse juiz. Evidências: `app/ferias/models.py:251`, `:252`, `:267`, `:269`, `:271`; texto proposto no anexo `app/templates/ferias/agendar.html:10`.

Criação individual, lote e escrita do Haiku chegam ao mesmo juiz: individual `app/ferias/views.py:314` → `app/ferias/services.py:578` → `app/ferias/views.py:259` → `app/ferias/services.py:330`; lote `app/ferias/views.py:654`, `:669`; Haiku `app/core/intake.py:34`, `:35`, `app/core/views_ia.py:684`, `app/ferias/services.py:301`, `:306`.

**PROCEDENTE: aprovação não equivale a agendamento aceito.** A solicitação é gravada como aprovada antes da tentativa de materialização. A materialização chama `full_clean()` e usa o mesmo juiz, mas a recusa vira aviso; a aprovação permanece e a view responde `ok=True`. Evidências: `app/ferias/services.py:756`, `:759`, `:785`, `:790`, `:791`; `app/ferias/views.py:799`.

As exceções deliberadas são diferentes da remarcação normal: histórico dispensa regras de fração futura e salva; correção administrativa explicitamente dispensa `full_clean()` e apresenta regra como aviso. Evidências: `app/ferias/services.py:529`, `:530`, `:547`; `app/ferias/views.py:508`, `:510`, `:513`, `:517`, `:531`.

API de férias recusa POST com 405; Django admin desta app registra somente Afastamento. Evidências: `app/api/views.py:2220`, `:2224`; `app/ferias/admin.py:2`, `:4`. Mensageria/Haiku de consulta retransmite avisos do core; evidências: `app/api/views_mensageria.py:772`, `:812`, `:824`; `mensageria/nucleo/ferramentas.py:2103`, `:2117`, `:2119`. Comportamento de PWA fora da cópia: **[não verificado]**.

Possível apoio ao item 5: o aviso hipotético não pergunta ao juiz agregado: usa diretamente `int(dias) < FRACAO_MINIMA`, enquanto a situação existente chama `violacoes_de_fracionamento`. Porém sua mensagem manda verificar as outras frações e não afirma proibição individual; é aviso conservador, não outra porta de recusa. Evidências: `app/ferias/situacao.py:227`, `:341`, `:344`, `:345`, `:346`.

## 14/11 15:05 · sessao f31cd24de3b1 · - · - · origem=Ronald (tmux)

Item 3: **PROCEDENTE, preexistente; adiamento não é cura.** `_commitar` devolve falso na recusa (`app/core/integrador_lote.py:481`); `_subir` apenas deixa de acumular a fatia, sem desfazer a cópia nem interromper (`app/core/integrador_lote.py:714-720`); faz push incondicional e, com rc zero, deploy (`app/core/integrador_lote.py:739-753`). A fatia recusada permanece na árvore publicada; só as commitadas recebem FIM (`app/core/integrador_lote.py:763-770`).

Não há fundamento para chamar isso de regressão introduzida pelo delta: o comportamento já está na cópia-base (`app/core/integrador_lote.py:719-753`). A cura HOOKMSG adiciona outra causa de recusa à mesma falha preexistente (`diff_v3.patch`, `bin/commit_msg.sh:29-31`).

A pausa protege a entrada oficial: o wrapper consulta os portões antes de montar e adquirir a árvore (`bin/integrador.sh:19-24`, `bin/integrador.sh:32`); a pausa fecha esses portões (`bin/trava.sh:40-42`, `bin/trava.sh:62-65`). Contudo, “dormente” não vale como garantia universal: a CLI admite `rodar` diretamente (`app/core/integrador_lote.py:857-859`), e `rodar` copia e recarrega sem consultar a pausa (`app/core/integrador_lote.py:614-638`). Uso direto em produção: **[não verificado]**.

Portanto, manter na fila é compatível com a pausa informada, mas a condição completa é **antes de qualquer execução do integrador, inclusive direta**. “Antes de religar a esteira” cobre o caminho oficial; não fecha todas as entradas (`bin/integrador.sh:19-32`; `app/core/integrador_lote.py:857-859`). O texto integral do aval E3-V10 e o estado atual da pausa não foram verificados independentemente: **[não verificado]**.

Classe concreta: **ordem/propagação de reprovação** — commit recusado não interrompe a publicação da árvore copiada (`app/core/integrador_lote.py:719-753`).

## 14/11 15:05 · sessao b7f716709981 · - · - · origem=Ronald (tmux)

Inventário do item 2:

- **Texto antigo ainda na cópia:** “menor deve ter no mínimo 14 dias”, em `app/templates/ferias/agendar.html:10`; é precisamente a linha substituída no diff fornecido. `app/templates/ferias/agendar.html:10`
- **Outro template:** “até 3 frações, uma ≥14 dias”; alinhado à regra atual, mas com números literais. `app/templates/ferias/_secao_colab.html:63`
- **Erros da porta:** máximo 3 frações; pelo menos uma com no mínimo 14 dias. Também são números literais. `app/ferias/models.py:368`, `app/ferias/models.py:376`
- **Aviso sobre lançamentos existentes:** nenhuma fração alcança `FRACAO_MINIMA`, explicitamente em dias corridos; ou quantidade superior a `FRACOES_MAXIMAS`. `app/ferias/situacao.py:231`, `app/ferias/situacao.py:240`
- **Aviso sobre proposta/correção:** se `int(dias) < FRACAO_MINIMA`, pede verificar as outras frações; deriva o aviso sem consultar o conjunto, mas não declara recusa. `app/ferias/situacao.py:341`, `app/ferias/situacao.py:344`
- **Mensagem na correção administrativa:** coleta esse aviso e o concatena à mensagem de sucesso. `app/ferias/views.py:516`, `app/ferias/views.py:549`, `app/ferias/views.py:552`
- **Haiku:** API entrega os avisos de situação e de proposta; ferramenta repassa os textos, sem regra própria nesses pontos. `app/api/views_mensageria.py:812`, `app/api/views_mensageria.py:824`, `app/api/views_mensageria.py:847`, `mensageria/nucleo/ferramentas.py:2103`, `mensageria/nucleo/ferramentas.py:2117`

Outro texto usuário equivalente a “menor deve ter 14”, fora da linha substituída, não foi localizado na busca textual. Ausência absoluta, respostas efetivamente geradas pelo Haiku e comunicados persistidos em banco: [não verificado].

## 14/11 15:02 · sessao 4e2677a2198a · - · - · origem=Ronald (tmux)

Use a fixture pronta `_Cenario` de [test_o261_c_viva_sem_chamado_do_dia.py:58](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:58).

- **c2 :954:** `_cenario('orfao_14h', DIA_ABERTO)`, com `DIA_ABERTO=date(2026,10,6)`, seguido de `reemitir_chamado_do_dia(p, c, DIA_ABERTO)`. A fixture fornece escala, entrada, célula com furo e protesto; há controle positivo para esse motivo em [test_o261_c_viva_sem_chamado_do_dia.py:222](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:222).
- **HIST :1642:** `_cenario('orfao_14h', DIA_ABERTO, viva=False, encerrado=True)`, seguido de `reatar_pergunta_orfa(p, c)`. A pergunta nasce validada; o histórico resolvido nasce por último para evitar reabertura pelos signals. [test_o261_c_viva_sem_chamado_do_dia.py:110](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:110), [test_o261_c_viva_sem_chamado_do_dia.py:117](chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:117).
- **I terminal:** a assinatura é `I.encerrar()`, sem argumentos. Execute antes de inserir P; com P viva ancorada, o gate pode recusar ou reatar. [models.py:1605](chamados/models.py:1605), [models.py:1617](chamados/models.py:1617).

Execução desses casos: [não verificado].

## 14/11 15:01 · sessao d125622dc511 · HOOKMSG · verificador · origem=Code

As referências marcadas **diff** usam a numeração nova; **cópia** indica o código disponível.

1. **PROCEDENTE — cura incompleta na régua.** `LEI_AKITA_COMMIT` herdado **sozinho** ainda seleciona o julgamento isolado, encerrando o selo antes do universo e do MORDE; a régua chama o script sem limpar essa variável. O pre-push foi saneado. Ocorrência real desse ambiente: [nao verificado]. (diff: `bin/tests/test_lei_akita.sh:25`, `bin/tests/test_lei_akita.sh:997`, `bin/pre-push.sh:38`; cópia: `bin/regua.sh:140`.)

   A guarda não derruba o MORDE interno normal: ele chama diretamente `python3 -c`, sem passar novamente pela guarda shell; o hook também remove HEAD/MORDE antes de chamar o selo. (diff: `bin/tests/test_lei_akita.sh:1015`, `bin/commit_msg.sh:18`.)

2. **PROCEDENTE — mensagem sem linha pode ser recusada antes do retorno `None`.** `VOCAB = vocabulario(R)` executa antes de `julgar_commit`; GLOSSARIO ausente ou PLANO-PADROES/NORTE ilegível encerra o Python antes da checagem da mensagem. Portanto, um commit inicial ou uma árvore sem esses documentos pode ser recusado mesmo sem linha LEI-AKITA. (cópia: `bin/tests/test_lei_akita.sh:95`, `:101`, `:111`, `:129`; diff: `bin/tests/test_lei_akita.sh:418`.)

   Além disso, todos passam por `write-tree`/`commit-tree`; falha nesses comandos ou ausência de resposta do juiz produz recusa, independentemente da mensagem. (diff: `bin/commit_msg.sh:20`, `:26`; `bin/tests/test_lei_akita.sh:999`.)

   | Chamador | Efeito pelo código |
   |---|---|
   | `pos_push` | Mensagem fixa sem linha; passa se construção do objeto e carga do vocabulário funcionarem. (cópia: `bin/pos_push.sh:30`; diff: `bin/commit_msg.sh:26`, `bin/tests/test_lei_akita.sh:418`.) |
   | `relato` | Mensagem sem linha em outro repositório; publica os documentos na raiz. Se esse destino usar o hook novo e não tiver `app/docs/GLOSSARIO.md`, será recusado. Instalação do hook nesse destino: [nao verificado]. (cópia: `bin/relato.sh:23`, `:115`, `:126`, `:136`; `bin/tests/test_lei_akita.sh:95`.) |
   | `codex_estado` | Mensagem sem linha no destino do relato; mesma condição acima. Na recusa, encerra antes do push. (cópia: `bin/codex_estado.sh:14`, `:78`, `:83`.) |
   | Integrador | Lê `msg_commit.txt`; conteúdo concreto [nao verificado]. Sem linha, sofre a mesma dependência antecipada do vocabulário; falha de commit segue pelo caminho do item 5. (cópia: `app/core/integrador_lote.py:481`, `:719`; `bin/tests/test_lei_akita.sh:129`.) |
   | Merges de raia | Mensagem concreta [nao verificado]. Sem linha, mesma dependência; os pais são montados incluindo `MERGE_HEAD`. (cópia: `CLAUDE.md:91`; diff: `bin/commit_msg.sh:22`, `:24`, `bin/tests/test_lei_akita.sh:418`.) |

3. **PROCEDENTE — dois verdes restantes.**

   - **“Bateria 5/5” sem bateria do script real.** `$P` é plantado, mas `_bateria "$P"` nunca roda. A bateria só roda contra o mutante, cobrando uma única divergência; o instalado recebe apenas inventado/sem linha. Recusar termo válido, recusar `n/a` instrumental ou aceitar `n/a` em negócio pode permanecer verde. (diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:68`, `:80`, `:85`, `:97`, `:112`.)
   - **Hooks alternativos continuam avaliados só por texto.** `_varre_wt` usa `_delega`; o 1b executa apenas o hook do common dir. Um hook alternativo executável que cite o script em comentário e saia zero passa na varredura — exatamente a espécie de falso demonstrada pelo MORDE(c). Existência desse desvio real: [nao verificado]. (diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:23`, `:40`, `:90`, `:99`.)

   O `while read <<<"$lista"` não acrescenta uma entrada vazia nessa construção; lista vazia é tratada antes. Pasta ausente agora é vermelho deliberado, conforme a triagem; ocorrência real [nao verificado]. (diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:31`, `:33`, `:35`, `:41`.)

4. **nada.** O selo exige somente a presença de `LEI-AKITA` nesses textos; os campos acrescentados preservam esse veredito. O fabricante entrega o prompt inteiro ao modelo, enquanto `gerar.py` lê outros moldes, sem consumir o LEIAME. Efeito nas próximas mensagens geradas: [nao verificado]. (diff: `bin/tests/test_lei_akita.sh:58`; cópia: `bin/fabricante.sh:158`, `bin/molde_fatia/gerar.py:29`.)

5. **PROCEDENTE — reclassifico o descarte do integrador.** Continuar o push não demonstra que “a fatia sai sozinha”: a recusa apenas impede sua inclusão em `subiram`, sem desfazer os arquivos copiados; o deploy seguinte continua e importa a árvore viva. Assim, código da fatia recusada pode ser publicado sem o commit. Ocorrência real: [nao verificado]. (diff: `bin/commit_msg.sh:29`; cópia: `app/core/integrador_lote.py:317`, `:719`, `:739`, `:753`; `bin/deploy.sh:327`.)

   Existe ainda um reload anterior à suite e ao commit; a recusa posterior também não desfaz esse efeito. (cópia: `app/core/integrador_lote.py:638`, `:719`.)

## 14/11 15:00 · sessao 4e2677a2198a · - · - · origem=Ronald (tmux)

Sugestões para os testes ORM, sem substituir os escritores: [nao verificado]

| Chamador | Menor caminho |
|---|---|
| Gate de encerramento | Chamar `I._gate_pergunta_viva('fechado', registrar=True, plano=([p], [(p, G)]))`; a recusa da porta levanta `ValueError`. `chamados/models.py:421`, `chamados/models.py:437` |
| Reconciliador direto | Substituir somente `_derivar_chamado_destino` por `(G, D)` e chamar `reatar_pergunta_orfa(p, colab)`. `chamados/reconciliador.py:1557`, `chamados/reconciliador.py:1655` |
| Reconciliador histórico | Derivador retorna `(None, None)`; criar histórico terminal único, fora do alcance novo. Usar motivo não intervalo como contraexemplo. `chamados/reconciliador.py:1594`, `chamados/reconciliador.py:1642` |
| Reemissão c2 | G vivo com `modulo_origem='batida_ausente'` e `data_turno=D` evita gates de nascimento. A porta `abrir` e o reaponte permanecem reais. `chamados/reconciliador.py:838`, `chamados/reconciliador.py:954` |
| Reata ao LIMBO | `reatar_perguntas_ao_limbo([p.pk], G)` consulta e escreve pelas portas reais. `chamados/services/limbo_folgas.py:239`, `chamados/services/limbo_folgas.py:244` |
| Agrupamento de furo | `agrupar_furo(I, G)`; para isolar o chamador, substituir apenas `_vivas_ancoradas` por `[p]` e `reconciliar` por `False`. `chamados/services/limbo_folgas.py:279`, `chamados/services/limbo_folgas.py:282` |
| Migração do fio | `migrar_resolvedoras_vivas(I, G)`, com P única viva em I e G sem perguntas; conferir `recusadas`. `chamados/services/fio.py:2130`, `chamados/services/fio.py:2174` |
| Reversão do beco | Executar `Command._reverter` com `builtins.open` fornecendo `StringIO` do JSON; ORM e reaponte reais. `chamados/management/commands/reatar_beco_folga_ao_limbo.py:119`, `chamados/management/commands/reatar_beco_folga_ao_limbo.py:127` |

Decisão sugerida para reversão: aplicar a mesma guarda, sem exceção. LIMBO→I eleito passa; I eleito vivo→G recusa. O reaponte para `None` atualmente não consulta `_costura_nao_mata_o_fio`; protegê-lo exige chamada incondicional e suporte a destino nulo, ou declarar explicitamente esse caso fora do delta. `chamados/services/fio.py:910`

Existem fixtures úteis de migração, LIMBO e encerramento em `chamados/tests/test_e1_v5_chave_familia.py:169`, `chamados/tests/test_o261e_f1_codex_v5.py:106`, `chamados/tests/test_o261e_f1_porta_encerramento.py:46`. Execução ORM e resultados: [não verificado].

## 14/11 15:00 · sessao 9046ca1fcf8d · - · - · origem=Ronald (tmux)

**PROCEDENTE — reclassificar o descarte do integrador.** Continuar o push não isola a fatia recusada: `_commitar` falso apenas deixa de acrescentá-la a `subiram`; não chama `_desfazer` nem retira seus arquivos da árvore (`app/core/integrador_lote.py:719-720`). Os arquivos já foram copiados (`app/core/integrador_lote.py:317-324`), e o deploy posterior segue (`app/core/integrador_lote.py:739,753`), importando a árvore do disco (`bin/deploy.sh:327,360`). Portanto a fatia recusada pode ser publicada sem commit; ocorrência real [nao verificado].

Há ainda um reload anterior ao julgamento: `deploy.sh --reload-copia` roda antes da suite e dos commits (`app/core/integrador_lote.py:638-639`). A recusa posterior não desfaz esse efeito (`app/core/integrador_lote.py:719-720`).

A verificação de índice chamada pelo pre-push não fecha esse caminho: `--conferir` verifica somente existência e chamada do pre-commit, depois retorna 0 (`bin/index_vs_arvore.sh:35-58`).

## 14/11 14:59 · sessao d50aab665e87 · - · - · origem=Ronald (tmux)

Item 3 — dois PROCEDENTE:

- **Bateria 5/5 sem execução do caso real.** `_bateria` define cinco verificações, mas só é chamada contra o mutante que aceita tudo; o instalado passa por dois casos do 1b. O repo `$P` é plantado e nunca submetido a `_bateria`. Recusar termo legítimo, recusar `n/a` em `bin/` ou aceitar `n/a` em `app/ponto` pode permanecer verde, embora a saída anuncie “bateria 5/5”. Citações do diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:68`, `:80`, `:85`, `:97`, `:112`.

- **Hooks de `core.hooksPath` continuam com delegação apenas textual.** A varredura usa `_delega "$hp"`; o 1b executa exclusivamente o hook do common dir. Assim, uma worktree cujo hooksPath contenha hook executável que cite `bin/commit_msg.sh` em comentário e termine em `exit 0` passa na varredura. O MORDE(c) demonstra exatamente esse falso, mas exercita `_roda`, sem aplicar essa prova aos alvos alternativos da varredura. Citações do diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:23`, `:40`, `:90`, `:99`. Existência desse desvio na máquina real: [nao verificado].

O `while read ... <<< "$lista"` não introduz, sozinho, uma entrada vazia adicional: a substituição de comando remove os delimitadores finais e a here-string acrescenta o delimitador para a última entrada; não identifico achado nesse ponto do diff `bin/tests/test_hook_mensagem_em_toda_copia.sh:31` e `:41`. Worktree registrada sem pasta agora gera diagnóstico deliberadamente; ocorrência na máquina real: [nao verificado].

## 14/11 14:58 · sessao 51aec7f36248 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — cura incompleta do ambiente herdado na régua.** `bin/regua.sh:138-140` executa o selo sem remover `LEI_AKITA_COMMIT`. A nova guarda só recusa quando também há HEAD/MORDE; COMMIT sozinho ainda seleciona julgamento isolado e retorna antes de universo/MORDE (`bin/tests/test_lei_akita.sh:25-27,1000-1003`, linhas novas do diff). O pre-push está saneado (`bin/pre-push.sh:38`, diff); ocorrência real desse ambiente na régua [nao verificado].

A guarda não derruba o MORDE interno normal: ele chama diretamente `python3 -c`, sem reentrar na guarda shell (`bin/tests/test_lei_akita.sh:1015`, linha nova; base `:979`). A régua normal não acrescenta COMMIT/HEAD/MORDE (`bin/regua.sh:138-140`); o pre-push remove COMMIT (`bin/pre-push.sh:38`, diff); o hook remove HEAD/MORDE (`bin/commit_msg.sh:18`, diff).

4. **nada.** O selo lê os dois textos, mas exige somente a presença de `LEI-AKITA` (`bin/tests/test_lei_akita.sh:30,58-59`, linhas novas; base `:24,52-53`). Outro selo exige `registro.baixa`, fora do trecho alterado (`bin/tests/test_baixa_diferida_host.sh:34`; `bin/fabricante_prompt.md:49-58`, diff). O fabricante entrega o prompt inteiro ao modelo (`bin/fabricante.sh:158-162`); o gerador lê apenas `esteira.sh`, `rodar.sh` e `cadeia.sh` (`bin/molde_fatia/gerar.py:29-35`). O efeito nas próximas saídas do modelo [nao verificado].

## 14/11 14:54 · sessao 9e692c4352d3 · HOOKMSG · verificador · origem=Code

Referências **B** = cópia/base; **P** = linhas resultantes do patch fornecido. HEAD efetivo, instalação dos hooks e censo das 113 worktrees: **[nao verificado]**.

**1. Extração de `julgar_commit`: nada isoladamente; PROCEDENTE na integração.**

A extração preserva o julgamento: merge continua pelo hunk próprio; raiz pelo `show`; demais commits pelo diff contra primeiro pai, com `--no-renames` e `-z`. `medida_do_commit` continua recebendo o SHA julgado. Duplicatas continuam dispensadas antes do julgamento e `EXCECAO_NOMINAL` aplicada depois. Há releitura de mensagem/pais, sem alteração demonstrada do predicado. (`B bin/tests/test_lei_akita.sh:458–486`; `P bin/tests/test_lei_akita.sh:408–430,490–505`)

**PROCEDENTE:** o modo novo pode desviar o juiz do push. O pre-push define somente `LEI_AKITA_HEAD`; um `LEI_AKITA_COMMIT=<SHA válido aprovado>` herdado ativa a saída antecipada, força esse SHA como alvo e pula universo, catraca e verificações gerais. Isso contradiz “nenhum env de bypass nasce”; as outras guardas do pre-push continuam existindo. (`B bin/pre-push.sh:38`; `P bin/tests/test_lei_akita.sh:23,507–512,996–1002`; `contrato.txt:1`, cláusula 5)

**2. Objeto sintético: PROCEDENTE quanto à equivalência geral.**

**`GIT_INDEX_FILE`: nada contra a ordem apresentada.** O `write-tree` ocorre antes da remoção das variáveis `GIT_*`, portanto recebe o índice herdado do hook. O cálculo de `CASA` acontece em subshell e não muda o diretório corrente. Não há fundamento no script para afirmar que ele troca prematuramente o índice temporário pelo índice normal. (`P bin/commit_msg.sh:15,18–19,27`)

A entrega efetiva do índice temporário pelo Git e o ciclo interno dos comandos abaixo não foram reproduzidos nesta cópia: **[nao verificado]**.

| Caso | Classificação |
|---|---|
| **Merge direto, `git merge -m`** | **Julga a errada** no fluxo automático em que o hook antecede a gravação de `MERGE_HEAD` **[nao verificado quanto ao ciclo interno do Git]**. Sem esse arquivo, o script monta somente o pai HEAD: passa a julgar todo o delta recebido, em vez do hunk próprio do merge. Pode recusar `n/a (instrumento)` num merge limpo que o juiz do push aceitaria. (`P bin/commit_msg.sh:20–25`; `P bin/tests/test_lei_akita.sh:416–429`) |
| **`merge --no-ff --no-commit` seguido de `commit`** | **Julga a estrutura certa**, desde que `MERGE_HEAD` contenha os pais finais: usa árvore do índice, HEAD e os pais desse arquivo. Continua sujeito à divergência de limpeza da mensagem abaixo. (`P bin/commit_msg.sh:19–25`) |
| **`--amend`** | **Julga a errada relativamente ao commit final:** usa HEAD como pai, portanto julga o delta da emenda. Emenda apenas de mensagem pode ter lista de arquivos vazia e aceitar `n/a` embora o commit final contenha negócio. É limite expressamente declarado, coberto posteriormente pelo push. (`P bin/commit_msg.sh:10–11,21`; `P bin/tests/test_lei_akita.sh:427–430`; `contrato.txt:1`, cláusula 3) |
| **Commit inicial** | **Julga a estrutura certa:** sem HEAD, monta raiz; o juiz enumera os arquivos pelo `show`. Se faltar vocabulário na árvore, recusa antes do julgamento. (`P bin/commit_msg.sh:19–25`; `P bin/tests/test_lei_akita.sh:91–131,427–429`) |
| **`commit -a` / `commit <paths>`** | **Julga a árvore certa se o Git entregar o índice preparado:** o script preserva `GIT_INDEX_FILE` até concluir a montagem. A comparação efetiva com um commit desses: **[nao verificado]**. (`P bin/commit_msg.sh:19–27`) |
| **`cleanup=verbatim` / `scissors`** | **Pode julgar mensagem errada.** O script aplica sempre `stripspace -s`, sem consultar o modo de cleanup. Em scissors, pode julgar uma linha inválida abaixo do corte e recusar indevidamente; em verbatim, não preserva necessariamente os bytes que serão gravados. Com `core.commentChar=L`, por exemplo, a limpeza pode eliminar a própria linha LEI-AKITA. Aplicação efetiva desses modos pelo Git: **[nao verificado]**. (`P bin/commit_msg.sh:17,24–25`) |
| **Rebase / cherry-pick** | **Sem invocação do hook, não julga.** Se chamado para um commit novo de um pai, a montagem usa HEAD; se envolver emenda/reword, existe o problema de `--amend`. A generalização “sem `--edit` não chama” aparece somente como comentário, sem prova na cópia: **[nao verificado]**. (`P bin/commit_msg.sh:10–11,21–28`) |

O objeto também não reproduz explicitamente todos os metadados e assinatura do commit final; o juiz usa mensagem, árvore e pais, portanto identidade exata do SHA não é demonstrada nem necessária para esse predicado. (`P bin/commit_msg.sh:25`; `P bin/tests/test_lei_akita.sh:413–430`)

**3. Modo `LEI_AKITA_COMMIT`: nada quanto aos itens 1–3 decidirem o resultado; vocabulário decide.**

Os itens 1–3 **rodam antes**, mas apenas acumulam `falhas`; não há `errexit`. O modo commit oculta seu stdout e retorna antes de consultar esse contador. Assim, esses itens não recusam nem aprovam por conta própria nesse caminho. (`B bin/tests/test_lei_akita.sh:16,24–53,983–987`; `P bin/tests/test_lei_akita.sh:23,996–1002`)

O vocabulário é carregado efetivamente antes do ramo commit; GLOSSARIO ou NORTE ilegíveis recusam. `producao` e a catraca são definidos, mas não chamados nesse modo; o ramo também sai antes do MORDE. (`P bin/tests/test_lei_akita.sh:91–131,394–400,507–513`)

**ALVO correto no caminho normal:** o shell passa `LEI_AKITA_HEAD="$LEI_AKITA_COMMIT"` ao Python, que lê o vocabulário da árvore sintética. Entretanto, `LEI_AKITA_MORDE=1` herdado força o ALVO Python para HEAD e pode trocar essa fonte: **PROCEDENTE condicional**. (`P bin/tests/test_lei_akita.sh:70–78,507–513,998`)

**4. QUEM ESCREVE / QUEM LÊ: PROCEDENTE no contrato das mensagens fabricadas.**

| Elemento | Escritor → leitor |
|---|---|
| `.git/hooks/commit-msg` | `hooks.sh install` escreve o wrapper no caminho fixo da principal; o conteúdo executa `commit_msg.sh` com os argumentos recebidos. (`P bin/hooks.sh:9,14–15,70–76`) |
| `bin/commit_msg.sh` | Novo arquivo do patch, chamado pelo wrapper; monta o objeto e chama o selo existente. (`P bin/hooks.sh:74`; `P bin/commit_msg.sh:19–28`) |
| `LEI_AKITA_R` | Escrito pelo hook com `TOPO`; lido para definir `R` no selo. (`P bin/commit_msg.sh:18,28`; `B bin/tests/test_lei_akita.sh:17`) |
| `LEI_AKITA_COMMIT` | Escrito pelo hook com o SHA sintético; lido no seletor Bash, ramo Python e execução do juiz. (`P bin/commit_msg.sh:25,28`; `P bin/tests/test_lei_akita.sh:23,507–510,996–998`) |
| `LEI_AKITA_HEAD` / `ALVO` | Pre-push escreve HEAD do push; modo commit substitui pela revisão sintética. `_ler` e `_show` usam ALVO como revisão-fonte. (`B bin/pre-push.sh:38`; `P bin/tests/test_lei_akita.sh:21–24,76–78,998`) |

Quanto aos chamadores:

- **`pos_push.sh`: passa pela ausência de linha**, pois sua mensagem automática não contém LEI-AKITA. O mesmo ocorre nas mensagens de `relato`, `codex_estado` e `reverter_o122`. (`B bin/pos_push.sh:30–38`; `B bin/relato.sh:136`; `B bin/codex_estado.sh:83`; `B bin/reverter_o122.sh:30`; `P bin/commit_msg.sh:17`)
- **Fabricante: PROCEDENTE.** O prompt exige formato “exato” sem `padrao=`, e o LEIAME repete esse formato. Se seguido literalmente, o juiz recusa por campo ausente. (`B bin/fabricante_prompt.md:49–56`; `B bin/molde_fatia/LEIAME:44–46`; `B bin/tests/test_lei_akita.sh:319–321`)
- **Esteira/integrador:** copia a mensagem, substitui RED/Suite e commita com `-F ... -- caminhos`; não acrescenta `padrao=`. As cadeias antigas também usam diretamente o arquivo de mensagem. (`B bin/molde_fatia/esteira.sh:14,22,26`; `B app/core/integrador_lote.py:481`; `B bin/molde_fatia/cadeia.sh:68`; `B bin/molde_autocura/cadeia.sh:65`)
- **`codex_pergunta --construtor`:** proíbe commit; **disjuntor:** os comandos Git encontrados consultam worktrees, histórico e status. Não há mensagem automática desses caminhos demonstrada como vítima do hook. (`B bin/codex_pergunta.sh:343–346`; `B bin/disjuntor.py:1234–1247`)

Recusa não demonstra travamento: no integrador, commit malsucedido não entra em `subiram`, mas o fluxo segue ao push. Incidência em pacotes reais: **[nao verificado]**. (`B app/core/integrador_lote.py:719–720,739`)

**5. Selo: PROCEDENTE — há verdes vazios.**

**Reconhecedor divergente:** para `titulo\rLEI-AKITA: padrao=Inventado`, o `grep` inicial não encontra a linha e o hook retorna 0; `tem_linha` Python reconhece a linha após o CR. Conferido em memória, sem arquivos. Logo existe mensagem que o hook dispensa e o juiz do push examinaria. (`P bin/commit_msg.sh:17`; `B bin/tests/test_lei_akita.sh:339–355`)

**Delegação apenas textual:** `_delega` exige executável e substring. Um hook com `exit 0` e a referência somente em comentário satisfaz essa condição. A bateria executa outro wrapper, plantado pelo teste, sem provar a execução do instalado ou do alvo efetivo dele. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:24,40–42,55,74`)

**Censo vazio por erro:** falha de `worktree list` não é conferida pelo chamador; falha de configuração vira `continue`; diretório ausente é pulado. Só a saída textual vazia determina aprovação. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:28–33,44–45`)

Os MORDEs provam menos que a promessa:

- **(a)** detecta o script mutante que aceita tudo na fixture; não testa o wrapper instalado. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:76–81`)
- **(b)** detecta o desvio plantado por worktree e sua retirada; não prova que o censo real enumerou alguma worktree. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:83–88`)

**Global/sistema e relativo: nada quanto à omissão explícita desses escopos.** A consulta não restringe a configuração a `--local`; caminhos relativos são ancorados em cada worktree. A cobertura continua sujeita aos erros silenciosos e ao teste textual de delegação. Configurações efetivas do host: **[nao verificado]**. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:30–32`)

**Selo órfão: nada.** Seu nome entra no glob executado pela régua. (`B bin/regua.sh:138–145`)

**6. Classes de reprovação.**

| Classe | Achado |
|---|---|
| **Segundo juiz duplicando o registrado** | **nada no predicado principal:** `julgar_commit` chama o `julgar` existente. (`P bin/tests/test_lei_akita.sh:408–430`; `B bin/tests/test_lei_akita.sh:336–344`) |
| **Derivação paralela de fonte única** | **PROCEDENTE:** “tem linha” é recalculado pelo `grep`, com semântica divergente do reconhecedor Python. (`P bin/commit_msg.sh:17`; `B bin/tests/test_lei_akita.sh:354–355`) |
| **Selo sem mordida** | **PROCEDENTE na ligação com o instalado:** a mordida funcional está na fixture; o wrapper real passa por inspeção textual. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:24,40,55,74–81`) |
| **ALVO com dois significados** | **nada no caminho normal:** significa revisão-fonte, seja ponta do push ou objeto sintético. O defeito demonstrado é o seletor ambiental que pode trocar indevidamente o caminho de julgamento. (`P bin/tests/test_lei_akita.sh:21–24,76–78,507–512,998`) |

## 14/11 14:50 · sessao b39e0829dc2b · - · - · origem=Ronald (tmux)

Referências **B** = arquivo da cópia/base; **P** = linhas resultantes do diff fornecido.

- **1 — nada na extração do PUSH, isoladamente.** `julgar_commit` conserva os comandos de merge, raiz, `--no-renames`, `-z` e a chamada de `medida_do_commit`; o laço conserva os filtros de duplicatas antes e exceção nominal depois. Apenas relê mensagem/pais do SHA. [B `bin/tests/test_lei_akita.sh:458`; B `bin/tests/test_lei_akita.sh:468`; P `bin/tests/test_lei_akita.sh:408`; P `bin/tests/test_lei_akita.sh:490`; P `bin/tests/test_lei_akita.sh:500`]

- **3 — itens 1–3 executam, mas não decidem o modo commit.** O script não tem `errexit`; essas verificações só incrementam `falhas`. O modo commit redireciona stdout e retorna antes do tratamento final desse contador. Vocabulário executa efetivamente antes do ramo commit e rejeita GLOSSARIO/NORTE ilegíveis; `producao`, catraca e MORDE não são chamados nesse caminho. [B `bin/tests/test_lei_akita.sh:16`; B `bin/tests/test_lei_akita.sh:24`; B `bin/tests/test_lei_akita.sh:35`; B `bin/tests/test_lei_akita.sh:100`; B `bin/tests/test_lei_akita.sh:129`; P `bin/tests/test_lei_akita.sh:23`; P `bin/tests/test_lei_akita.sh:507`; P `bin/tests/test_lei_akita.sh:996`]

- **ALVO: nada quanto a campo com dois significados, no caminho normal.** É a revisão que fornece o vocabulário: ponta do push ou objeto sintético do commit. O shell força `LEI_AKITA_HEAD="$LEI_AKITA_COMMIT"` para o Python. `medida_do_commit` continua consultando `h`, independentemente do ALVO global. [B `bin/tests/test_lei_akita.sh:74`; B `bin/tests/test_lei_akita.sh:76`; B `bin/tests/test_lei_akita.sh:296`; P `bin/tests/test_lei_akita.sh:998`]

- **PROCEDENTE — nasce seletor ambiental que pode desviar o juiz do PUSH.** O pre-push força apenas `LEI_AKITA_HEAD`; um `LEI_AKITA_COMMIT=<SHA válido aprovado>` herdado ativa o novo modo, troca o alvo e evita universo, catraca e itens 1–3. Não há evidência de uso em produção. [B `bin/pre-push.sh:38`; P `bin/tests/test_lei_akita.sh:23`; P `bin/tests/test_lei_akita.sh:507`; P `bin/tests/test_lei_akita.sh:998`; produção: **[nao verificado]**]

- **PROCEDENTE — segundo reconhecedor divergente produz verde vazio.** O gate `grep` retorna “sem linha”, mas `tem_linha` do juiz retorna verdadeiro para `titulo\rLEI-AKITA: padrao=Inventado\n`, `titulo\vLEI-AKITA: ...` e prefixo NBSP. Verifiquei as expressões em memória, sem arquivos; `git stripspace -s` preserva esses bytes. O hook sai antes de chamar o juiz; o juiz do PUSH reconhece tais linhas. [P `bin/commit_msg.sh:17`; B `bin/tests/test_lei_akita.sh:354`; B `bin/tests/test_lei_akita.sh:339`]

- **PROCEDENTE condicional — `LEI_AKITA_MORDE=1` herdado troca o vocabulário.** O Python fixa ALVO em HEAD quando MORDE está ativo; o ramo commit roda antes do ramo MORDE. Assim pode julgar o SHA sintético com vocabulário do HEAD. A ocorrência desse ambiente em produção não foi demonstrada. [B `bin/tests/test_lei_akita.sh:68`; B `bin/tests/test_lei_akita.sh:74`; P `bin/tests/test_lei_akita.sh:507`; P `bin/tests/test_lei_akita.sh:513`; produção: **[nao verificado]**]

Coordenadas P conferidas por contagem das alterações: início `+2`, extração `+30`, substituição de 14 linhas por uma `−13`, ramo commit Python `+6`, total `+25`; `julgar_commit:408–430`, ramo Python `507–512`, ramo shell `996–1002`. [diff fornecido: **[nao verificado por arquivo físico]**] [nao verificado]

## 14/11 14:50 · sessao ff798a325404 · - · - · origem=Ronald (tmux)

Q4: **PROCEDENTE condicional nos geradores de mensagens; nenhum travamento demonstrado.**

- `bin/hooks.sh install` escreve o wrapper no caminho fixo da principal, delegando para `bin/commit_msg.sh`; os destinos novos são `FONTE_MSG`/`ALVO_MSG` e o `exec` está no bloco adicionado do diff. Não há instalação realizada nem hooks reais no dado fornecido. (`diff_v1.patch`, hunk de `bin/hooks.sh:9` e hunk `+65`; instalação atual [nao verificado].)
- Escritor de `LEI_AKITA_COMMIT` e `LEI_AKITA_R`: a nova chamada de `bin/commit_msg.sh:28`; leitores: modo Bash e ramo Python acrescentados em `bin/tests/test_lei_akita.sh`. `LEI_AKITA_HEAD` já é escrito pelo pre-push e lido pelo selo; `ALVO` Bash alimenta `_ler`, e o Python possui sua própria variável `ALVO`. (`bin/pre-push.sh:38`; `bin/tests/test_lei_akita.sh:17`; `bin/tests/test_lei_akita.sh:21`; `bin/tests/test_lei_akita.sh:22`; `bin/tests/test_lei_akita.sh:74`; `diff_v1.patch`, hunks `+19`, `+497`, `+993`.)
- **Possível bypass novo:** `LEI_AKITA_COMMIT=<SHA sem linha>` herdado no push ativa o modo que julga apenas aquele SHA e sai antes do universo/catraca; o pre-push atribui somente `LEI_AKITA_HEAD`, sem limpar a variável nova. Outras guardas do push continuam aplicáveis. Isso contradiz a promessa “nenhum env de bypass nasce”. (`bin/pre-push.sh:38`; `diff_v1.patch`, ramo `if os.environ.get('LEI_AKITA_COMMIT')` e bloco Bash do hunk `+993`; `contrato.txt:1`.)

Chamadores:

| Sítio | Resultado pelo código |
|---|---|
| `pos_push.sh` | Mensagem automática não contém LEI-AKITA; passa pelo retorno inicial do hook. (`bin/pos_push.sh:30`; `bin/pos_push.sh:31`; `bin/pos_push.sh:37`; proposta `bin/commit_msg.sh:17`.) |
| `codex_pergunta --construtor` | Proíbe commit; inventário verifica HEAD e descarta a saída quando alterado. Não monta mensagem de commit. (`bin/codex_pergunta.sh:345`; `bin/codex_pergunta.sh:148`; `bin/codex_pergunta.sh:181`; `bin/codex_pergunta.sh:437`; `bin/codex_pergunta.sh:441`.) |
| `disjuntor.py` | Os subprocessos Git encontrados consultam worktrees, log e status, sem commit; ele não depende de mensagem automática LEI-AKITA. (`bin/disjuntor.py:1234`; `bin/disjuntor.py:1237`; `bin/disjuntor.py:1244`; `bin/disjuntor.py:1247`.) |
| Fabricante | Prompt exige formato “exato” de LEI-AKITA **sem `padrao=`** e proíbe commit direto; se seguido literalmente, o novo hook recusará no integrador. (`bin/fabricante_prompt.md:49`; `bin/fabricante_prompt.md:52`; `bin/fabricante_prompt.md:70`; `bin/tests/test_lei_akita.sh:319`; `bin/tests/test_lei_akita.sh:321`.) |
| Esteira/integrador | Copia `msg_commit.molde` para `.txt`, substitui somente RED/Suite e entrega ao integrador; este executa `git commit -F msg_commit.txt -- caminhos`, sem completar o campo ausente. (`bin/molde_fatia/esteira.sh:14`; `bin/molde_fatia/esteira.sh:22`; `bin/molde_fatia/esteira.sh:26`; `app/core/integrador_lote.py:481`.) |
| Cadeias antigas | Também commitam o arquivo de mensagem diretamente; mensagem sem `padrao=` será recusada. (`bin/molde_fatia/cadeia.sh:68`; `bin/molde_autocura/cadeia.sh:65`; `bin/tests/test_lei_akita.sh:321`.) |
| Outros automáticos | `relato`, `codex_estado` e `reverter_o122` geram mensagens sem LEI-AKITA, portanto passam pelo retorno inicial. (`bin/relato.sh:136`; `bin/codex_estado.sh:83`; `bin/reverter_o122.sh:30`; proposta `bin/commit_msg.sh:17`.) |

A recusa de `_commitar` apenas impede adicionar a fatia em `subiram`; o integrador segue ao push. Incidência em pacotes reais e produção: [nao verificado]. (`app/core/integrador_lote.py:719`; `app/core/integrador_lote.py:720`; `app/core/integrador_lote.py:739`.)

Q6, juiz registrado: **nada quanto a duplicação de juiz de domínio**. O mapa descreve juízes de domínio gerados de `core/juizes.py`; a proposta reutiliza `julgar` existente por meio de `julgar_commit`. Há, porém, reconhecimento paralelo de “tem linha” no grep do hook e em `tem_linha` Python, sujeito a divergência de whitespace Unicode. (`app/docs/MAPA-JUIZES.md:3`; `app/docs/MAPA-JUIZES.md:7`; `bin/tests/test_lei_akita.sh:336`; `bin/tests/test_lei_akita.sh:354`; proposta `bin/commit_msg.sh:17`; `diff_v1.patch`, definição nova `julgar_commit`.)

## 14/11 14:49 · sessao 1b363f83bb2d · - · - · origem=Ronald (tmux)

**Item 5 — PROCEDENTE: selo pode ficar verde com hook instalado inoperante.** `_delega` verifica somente executável e ocorrência textual de `bin/commit_msg.sh`; um wrapper executável contendo `exit 0` e `# /caminho/existente/bin/commit_msg.sh` satisfaz a inspeção, inclusive existência do alvo. A bateria executa outro wrapper, criado no repositório plantado, e não o instalado. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:24`, `:40`, `:41`, `:55`, `:74`.

**Item 5 — PROCEDENTE: censo vazio por erro pode passar.** Falha de `worktree list` não é checada pelo chamador; erro de `config --get` vira `continue`; caminhos inexistentes também são pulados. `DESV` vazio é aprovado independentemente do status da função. O MORDE usa outro repositório e não exige uma worktree sequer enumerada no repositório real. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:28`, `:29`, `:30`, `:44`, `:45`, `:83`. Ocorrência disso em produção: **[nao verificado]**.

**MORDE (a): prova estreita, válida por inspeção.** O mutante ganha `exit 0`; sua bateria precisa denunciar aceitação do padrão inventado. Isso testa o script mutante na fixture, não a execução efetiva do hook instalado. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:76`, `:78`, `:81`.

**MORDE (b): prova estreita, válida por inspeção.** Planta configuração por worktree apontando para pasta vazia, exige denúncia e depois ausência de denúncia ao remover a configuração. Não cobre configuração global/sistema plantada nem enumeração real vazia. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:82`, `:83`, `:85`, `:86`.

**hooksPath global/sistema/relativo: nada quanto à omissão explícita de escopo.** A consulta usa `git config --get`, sem restringir a `--local` ou `--worktree`; valores relativos são ancorados na raiz de cada worktree. São usadas as mesmas condições textuais fracas de delegação. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:30`, `:31`, `:32`. Configurações efetivamente presentes no host: **[nao verificado]**.

**Selo órfão: nada.** A régua descobre `bin/tests/test_*.sh` e executa cada arquivo por Bash; o novo nome entra no glob. `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`.

**Item 6 — selo sem mordida: PROCEDENTE na ligação com o instalado.** A mordida funcional existe na fixture; a ligação com o wrapper real permanece um grep que aceita comentário ou código inalcançável. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:24`, `:40`, `:55`, `:74`.

## 14/11 13:55 · sessao 0911a0f79119 · HJ-091 · sonda · origem=Code

| Campo | Valor |
|---|---|
| universo_admin_encerrado_colab_com_ferias | `845` [nao verificado] |
| com_dia_do_fato | `419` [nao verificado] |
| dia_coberto_por_ferias | `27` [nao verificado] |
| veto_admin_sem_fato_novo_ativo_hoje | `16` [nao verificado] |
| erros | `0` [nao verificado] |
| pks_veto | `[3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801]` [nao verificado] |
| DEPOIS | `{'pk': 3977, 'tipo': 'ferias', 'status': 'rejeitada', 'agendamento_ferias_id': 103, 'data': datetime.date(2026, 11, 7)}` [nao verificado] |
| DEPOIS | `{'pk': 3985, 'tipo': 'ferias', 'status': 'rejeitada', 'agendamento_ferias_id': 103, 'data': datetime.date(2026, 11, 5)}` [nao verificado] |
| DEPOIS | `{'pk': 3995, 'tipo': 'ferias', 'status': 'rejeitada', 'agendamento_ferias_id': 103, 'data': datetime.date(2026, 11, 6)}` [nao verificado] |
| DEPOIS | `{'pk': 5592, 'tipo': 'abono', 'status': 'aprovada', 'agendamento_ferias_id': None, 'data': datetime.date(2026, 11, 5)}` [nao verificado] |
| col344_0507 | `[(3977, 'ferias', 'rejeitada', 103), (3985, 'ferias', 'rejeitada', 103), (3995, 'ferias', 'rejeitada', 103), (5592, 'abono', 'aprovada', None)]` [nao verificado] |
| rejeitadas_sem_origem_em_agendamento_vivo | `0 []` [nao verificado] |
| ferias_sem_origem_total | `60` [nao verificado] |
| Ocorrências literais de ERRO no dado raspado | `0` [nao verificado] |
| Ocorrências literais de Traceback no dado raspado | `0` [nao verificado] |

## 14/11 13:53 · sessao 3423c9c36e81 · HJ-091 · sonda · origem=Code

| Campo | Valor |
|---|---|
| universo_admin_encerrado_colab_com_ferias [nao verificado] | 845 |
| com_dia_do_fato [nao verificado] | 419 |
| dia_coberto_por_ferias [nao verificado] | 27 |
| veto_admin_sem_fato_novo_ativo_hoje [nao verificado] | 16 |
| erros [nao verificado] | 0 |
| pks_veto [nao verificado] | [3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801] |
| TRES [nao verificado] | 3977 col 344 data 2026-11-07 tipo ferias status rejeitada marcas [103] janelas [103] |
| TRES [nao verificado] | 3985 col 344 data 2026-11-05 tipo ferias status rejeitada marcas [103] janelas [103] |
| TRES [nao verificado] | 3995 col 344 data 2026-11-06 tipo ferias status rejeitada marcas [103] janelas [103] |
| coluna_fk_existe [nao verificado] | False |
| Ocorrências literais de ERRO no bloco fornecido [nao verificado] | 0 |
| Ocorrências literais de Traceback no bloco fornecido [nao verificado] | 0 |

O bloco transcrito informa `erros 0`; a afirmação “erro NAO e zero” não encontra suporte nesse bloco. O total real de erros da execução permanece [nao verificado].

## 14/11 13:50 · sessao af94840d4a0c · FRAC5 · verificador · origem=Code

**Os dois bypasses centrais da volta 2 estão fechados na remarcação regular: ausência de fração ≥14 e divergência entre `dias` e intervalo. Não encontrei novo bypass de gravação introduzido pelo delta.** Permanecem limitações nos testes e caminhos anteriores que não passam por `clean()`. (`app/ferias/services.py:680`; `app/ferias/services.py:686`; `app/ferias/services.py:718`; `app/ferias/models.py:380`; `app/ferias/models.py:396`)

Execução de RED contra `63ff6e88b`, suíte, navegador, pouso e impacto em produção: **[nao verificado]**. Os resultados contra o pai abaixo são contrafactuais pelo delta fornecido.

**1. NÃO PROCEDE [bloqueia] — julgamento sobre foto velha ou antes da atribuição.**

`vivo` vem de `select_for_update()`. O período também é relido sob trava e atribuído à relação de `vivo`. Depois são atribuídos os novos `data_inicio`, `data_fim` e `dias`; somente então roda `vivo.clean()`. O juiz exclui o próprio PK e usa o intervalo novo para julgar a fração substituta. (`app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/services.py:603`; `app/ferias/services.py:680`; `app/ferias/services.py:686`; `app/ferias/models.py:371`; `app/ferias/models.py:377`)

**NÃO PROCEDE [bloqueia] — suposta aplicação de saldo, abono, período e sobreposição pelo `clean()`.** O corpo desse método contém somente fracionamento, ordem das datas e igualdade entre quantidade e intervalo. A chamada acrescentada é `clean()`, não `full_clean()`. (`app/ferias/models.py:363`; `app/ferias/models.py:399`; `app/ferias/services.py:686`)

| Validação | Onde ocorre; pertinência à remarcação |
|---|---|
| Máximo de três frações | `clean()` conta os irmãos ativos e a fração substituta. Pode recusar a remarcação de um período que já tenha quatro frações. É o mesmo julgamento registrado, agora aplicado à substituição. (`app/ferias/models.py:368`; `app/ferias/models.py:372`; `app/core/juizes.py:271`) |
| Alguma fração ≥14 | Usa dias gravados dos irmãos e duração nova do próprio objeto. Fecha o caso único **14→5**. (`app/ferias/models.py:377`; `app/ferias/models.py:380`) |
| Fração escrita ≥5 | Julga o intervalo novo; frações curtas históricas dos irmãos não bloqueiam essa cláusula. (`app/ferias/models.py:384`; `app/ferias/models.py:387`) |
| Ordem e equivalência das datas | Recusa fim anterior ao início e `dias` diferente do intervalo inclusivo. Ambas fazem sentido numa troca de janela. Fracionamento tem precedência sobre essas mensagens. (`app/ferias/models.py:380`; `app/ferias/models.py:387`; `app/ferias/models.py:392`; `app/ferias/models.py:396`) |
| Sobreposição | Continua no `save()`, pelo juiz de precedência. A limpeza anterior e a gravação estão no mesmo `atomic`; o veto desfaz ambas. (`app/ferias/services.py:669`; `app/ferias/services.py:694`; `app/ferias/services.py:699`; `app/ferias/models.py:450`; `app/ferias/models.py:454`; `app/ferias/models.py:463`) |
| Período aquisitivo/concessivo | `clean()` usa o período para selecionar irmãos; não exige status `adquirido` nem enquadramento das novas datas no ciclo ou limite concessivo. A troca salva somente concedidos/status do PA. (`app/ferias/models.py:363`; `app/ferias/models.py:368`; `app/ferias/services.py:701`) |
| Saldo | É julgado **antes** da atribuição: saldo atual mais os dias já reservados pelo agendamento. Isso permite substituir a reserva sem cobrá-la duas vezes. O novo `clean()` não refaz essa conta. (`app/ferias/services.py:675`; `app/ferias/services.py:679`; `app/ferias/models.py:220`; `app/ferias/models.py:225`) |
| Abono | A troca não vende dias; vendidos já são descontados pelo saldo. O teto de venda é aplicado nas portas que incluem abono, separadamente. (`app/ferias/models.py:220`; `app/ferias/services.py:312`; `app/ferias/services.py:277`; `app/ferias/services.py:699`; `app/ferias/services.py:701`) |

**NÃO PROCEDE [bloqueia] — ausência de `clean()` significar gravação regular liberada.** Ele não roda quando:

- A linha está `cancelado`: a trava recusa antes, impedindo ressuscitar a reserva. (`app/ferias/models.py:313`; `app/ferias/services.py:600`; `app/ferias/services.py:670`)
- Está `em_gozo`, `concluido`, outro status diferente de `agendado`, ou `agendado` com início já alcançado: `pode_editar_em(hoje)` recusa antes. (`app/ferias/models.py:340`; `app/ferias/models.py:344`; `app/ferias/services.py:671`)
- Falta saldo: a recusa também antecede o julgamento. (`app/ferias/services.py:675`; `app/ferias/services.py:677`)
- A entrada é a correção administrativa: o wrapper mantém `so_nao_iniciado=False`, independentemente do status não cancelado. É a exceção A3 expressa, com aviso pela view. (`app/ferias/services.py:657`; `app/ferias/services.py:681`; `app/ferias/services.py:708`; `app/ferias/views.py:508`; `app/ferias/views.py:552`)

O status novo só é atribuído depois do `clean()`. Isso não altera seu resultado atual: ele filtra o status dos irmãos, mas não consulta `self.status`. (`app/ferias/services.py:691`; `app/ferias/models.py:363`; `app/ferias/models.py:370`)

**2. NÃO PROCEDE [bloqueia] — perda de cobertura ao remover o recorte do wrapper.**

Toda remarcação regular que chega à gravação passa `so_nao_iniciado=True` e atravessa `clean()`. Para `novos_dias=1–4`, um intervalo curto é recusado pelo fracionamento; um intervalo maior é recusado pela desigualdade com `dias`. Chamadas inválidas por status/início/saldo agora podem receber essas recusas primeiro, sem gravar. (`app/ferias/services.py:718`; `app/ferias/services.py:671`; `app/ferias/services.py:676`; `app/ferias/services.py:686`; `app/ferias/services.py:699`; `app/ferias/models.py:387`; `app/ferias/models.py:396`)

| Caminho encontrado | Tratamento de existente e de fração `<5` |
|---|---|
| `remarcar_agendamento` | Converge no escritor com julgamento obrigatório. (`app/ferias/services.py:718`; `app/ferias/services.py:686`) |
| `corrigir_datas_agendamento` | Converge no mesmo escritor sem `clean()`; admite curto sujeito a status, saldo e colisão. A view exige quantidade positiva, calcula o fim e acrescenta avisos. (`app/ferias/services.py:708`; `app/ferias/services.py:670`; `app/ferias/services.py:675`; `app/ferias/views.py:503`; `app/ferias/views.py:507`; `app/ferias/views.py:517`; `app/ferias/views.py:552`) |
| `_trocar_janela` diretamente | O padrão é administrativo, sem `clean()`. Os dois wrappers encontrados escolhem explicitamente suas modalidades. (`app/ferias/services.py:657`; `app/ferias/services.py:708`; `app/ferias/services.py:718`) |
| Django admin | O registro local contém somente `Afastamento`; não oferece edição de `AgendamentoFerias`. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`) |
| `sincronizar_status_ferias` | Usa a porta que altera somente status; não julga nem corrige duração curta. (`app/ferias/management/commands/sincronizar_status_ferias.py:48`; `app/ferias/services.py:928`; `app/ferias/services.py:929`) |
| `listar_ferias_dp` | Simula cancelamento e validação, depois força rollback; não remarca datas/dias. (`app/ferias/management/commands/listar_ferias_dp.py:100`; `app/ferias/management/commands/listar_ferias_dp.py:107`; `app/ferias/management/commands/listar_ferias_dp.py:109`) |
| Realinhamento de concedidos | Altera o contador do PA, não dias/datas do agendamento. (`app/ferias/services.py:156`; `app/ferias/services.py:160`; `app/ferias/services.py:162`) |
| API do colaborador | POST retorna 405; GET serializa datas, dias e status existentes. (`app/api/views.py:2223`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| API de mensageria | Consulta situação e pedido hipotético; entrega avisos. (`app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`; `app/api/views_mensageria.py:847`) |

**PROCEDE [fila] — pontos anteriores não fechados por este delta:** histórico cria fração curta sem fracionamento; cancelar a fração de 14 pode deixar somente a de 5; `save()` não chama `clean()` universalmente; chamada direta à correção ainda aceita `dias` e intervalo independentes. Nenhum desses mecanismos foi acrescentado pelo delta. (`app/ferias/services.py:492`; `app/ferias/services.py:510`; `app/ferias/services.py:616`; `app/ferias/services.py:620`; `app/ferias/models.py:461`; `app/ferias/services.py:680`; `app/ferias/services.py:681`; `app/ferias/services.py:708`)

O helper ainda ignora zero por `if f`; isso permanece uma limitação dele, mas não permite gravar zero pela remarcação regular com intervalo coerente: igualdade e ordem das datas também são verificadas. (`app/ferias/models.py:271`; `app/ferias/models.py:392`; `app/ferias/models.py:396`)

**3. Quem escreve e quem lê os campos/valores envolvidos no delta.**

| Campo/valor | Quem escreve ou monta | Quem lê |
|---|---|---|
| `so_nao_iniciado` | Padrão `False`; remarcação passa `True`; correção conserva o padrão. (`app/ferias/services.py:657`; `app/ferias/services.py:708`; `app/ferias/services.py:718`) | Guarda de edição e condição do `clean()`. (`app/ferias/services.py:671`; `app/ferias/services.py:681`) |
| `hoje` | Borda lê uma vez por request; testes fornecem datas explícitas. (`app/ferias/views.py:15`; `app/ferias/views.py:695`; `app/ponto/tests/test_ferias_fracao_demais_5.py:145`) | Guarda de início, aviso, status do agendamento e do PA. (`app/ferias/services.py:671`; `app/ferias/services.py:687`; `app/ferias/services.py:691`; `app/ferias/services.py:700`) |
| `nova_ini`, `novos_dias`, `nova_fim` | Views convertem POST e derivam fim; testes também chamam o serviço diretamente. (`app/ferias/views.py:499`; `app/ferias/views.py:507`; `app/ferias/views.py:708`; `app/ferias/views.py:718`; `app/ponto/tests/test_ferias_fracao_demais_5.py:154`) | Saldo, atribuição da janela e, agora, `clean()`. (`app/ferias/services.py:676`; `app/ferias/services.py:680`; `app/ferias/services.py:686`) |
| PK e vínculo ao PA | Criações associam o agendamento ao período; o próprio banco atribui a identidade na inserção. (`app/ferias/services.py:544`; `app/ferias/services.py:545`) | Releitura/travas; seleção dos irmãos; exclusão do próprio PK. (`app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/models.py:369`; `app/ferias/models.py:371`) |
| `AgendamentoFerias.data_inicio/data_fim/dias` | Escritor da troca atribui e persiste; criação e histórico inicializam. (`app/ferias/services.py:680`; `app/ferias/services.py:699`; `app/ferias/services.py:484`; `app/ferias/services.py:544`) | `clean()`, colisão, espelho, situação e API. (`app/ferias/models.py:377`; `app/ferias/models.py:396`; `app/ferias/models.py:450`; `app/ponto/services/ausencia.py:1185`; `app/ferias/situacao.py:122`; `app/api/views.py:2244`) |
| `AgendamentoFerias.status` | Criação/status por data, troca, cancelamentos e sincronização. (`app/ferias/services.py:546`; `app/ferias/services.py:691`; `app/ferias/services.py:619`; `app/ferias/services.py:651`; `app/ferias/services.py:928`) | Guardas, seleção de irmãos, espelho e API. (`app/ferias/services.py:600`; `app/ferias/models.py:344`; `app/ferias/models.py:370`; `app/ponto/services/ausencia.py:1181`; `app/api/views.py:2247`) |
| `PeriodoAquisitivo.dias_concedidos` | Troca substitui a reserva; criação/histórico somam; cancelamentos subtraem; realinhamento grava a soma. (`app/ferias/services.py:679`; `app/ferias/services.py:548`; `app/ferias/services.py:511`; `app/ferias/services.py:616`; `app/ferias/services.py:643`; `app/ferias/services.py:160`) | Saldo, saldo para remarcar, status do PA e contador de divergência. (`app/ferias/models.py:220`; `app/ferias/models.py:225`; `app/ferias/models.py:98`; `app/ferias/services.py:125`) |
| `dias_direito` e `dias_vendidos` do PA | Art.130/edição escrevem direito; portas de abono/histórico escrevem vendidos. (`app/ferias/services.py:373`; `app/ferias/services.py:824`; `app/ferias/services.py:277`; `app/ferias/services.py:512`; `app/ferias/services.py:906`) | Saldo utilizado antes do novo `clean()`; teto de venda. (`app/ferias/models.py:220`; `app/ferias/models.py:238`; `app/ferias/services.py:675`) |
| `PeriodoAquisitivo.status` | `aplicar_status()` recebe o veredito e a troca o persiste. (`app/ferias/models.py:215`; `app/ferias/services.py:700`; `app/ferias/services.py:701`) | Elegibilidade para agendar e apresentação da API. (`app/ferias/models.py:234`; `app/api/views_mensageria.py:841`) |
| `aviso_emitido`, antecedência 30 | A troca deriva o flag usando a constante; criação também o inicializa. (`app/ferias/situacao.py:56`; `app/ferias/services.py:687`; `app/ferias/services.py:547`) | View, situação e API. (`app/ferias/views.py:733`; `app/ferias/situacao.py:265`; `app/api/views_mensageria.py:845`) |
| Irmãos: `dias`, status e PA | Portas acima; fixtures novas inserem irmã `concluido`, 20 dias, de 05–24/01/2026, com PA e usuário existentes. (`app/ferias/test_cancelar_2x.py:247`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`) | Consultas de irmãos do `clean()` e da situação; o novo `clean()` não lê `agendado_por`. (`app/ferias/models.py:363`; `app/ferias/models.py:368`; `app/ferias/situacao.py:117`) |
| Duração calculada, frações e `caput/par1/par1_demais` | `clean()` monta duração/listas; helper compara com constantes 3/14/5. (`app/ferias/models.py:251`; `app/ferias/models.py:273`; `app/ferias/models.py:275`; `app/ferias/models.py:277`; `app/ferias/models.py:377`; `app/ferias/models.py:379`) | Ramos de recusa do `clean()` e ramos de aviso; wrapper regular deixou de escolher somente `par1_demais`. (`app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/models.py:387`; `app/ferias/situacao.py:227`; `app/ferias/services.py:718`) |
| Mensagens de `ValidationError` | Ramos do `clean()`; colisão traduzida no `save()`. (`app/ferias/models.py:373`; `app/ferias/models.py:381`; `app/ferias/models.py:388`; `app/ferias/models.py:397`; `app/ferias/models.py:454`) | View apresenta `e.messages`; testes conferem trechos. (`app/ferias/views.py:730`; `app/ponto/tests/test_ferias_fracao_demais_5.py:137`; `app/ponto/tests/test_ferias_fracao_demais_5.py:146`; `app/ponto/tests/test_ferias_fracao_demais_5.py:155`) |
| Avisos `{codigo,texto,nivel,ancora}` | `_aviso()` e situação montam. (`app/ferias/situacao.py:64`; `app/ferias/situacao.py:229`; `app/ferias/situacao.py:238`) | Índice preserva listas, API entrega, correção mostra textos e novo teste inspeciona a saída. (`app/ferias/situacao.py:302`; `app/api/views_mensageria.py:847`; `app/ferias/views.py:552`; `app/ponto/tests/test_ferias_fracao_demais_5.py:183`) |

**4. PROCEDE [fila] — as fixtures perderam coerência agregada; NÃO PROCEDE [bloqueia] — terem eliminado a mordida da limpeza e do veto.**

Na F346, o contador continua **10**, mas as linhas passam a somar **30**. Após reduzir para cinco, a asserção exige contador **5**, enquanto os agendamentos somam **25**. No teste da foto velha, contador **30** e agendamento **30** recebem uma irmã **20**; após remarcar para dez, espera-se contador **10**, mas as linhas somam **30**. Essas fixtures deixam de provar saldo agregado íntegro; a divergência é precisamente a condição medida pelo contador existente. (`app/ferias/test_remarcar_limpa_janela_f346.py:28`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:83`; `app/ferias/test_cancelar_2x.py:51`; `app/ferias/test_cancelar_2x.py:55`; `app/ferias/test_cancelar_2x.py:247`; `app/ferias/test_cancelar_2x.py:253`; `app/ferias/services.py:117`)

A irmã elimina o impedimento de falta de fração ≥14 para manter os cenários de limpeza. O R06 continua com três frações, alcança a colisão no `save()` e confere rollback de ausências, contador e início. A adulteração da foto para 999 também continua exercitada. (`app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:99`; `app/ferias/test_remarcar_limpa_janela_f346.py:105`; `app/ferias/test_remarcar_limpa_janela_f346.py:108`; `app/ferias/test_remarcar_limpa_janela_f346.py:110`; `app/ferias/test_cancelar_2x.py:251`)

Cada asserção nova/alterada, contra `63ff6e88b`, pelo trecho anterior fornecido no delta:

| Asserção | Falharia no pai? |
|---|---|
| Quatro dias: `assertRaises` | **Não:** o recorte anterior já recusava quatro. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`) |
| Quatro dias: contém `art. 134` | **Não:** a mensagem anterior também continha isso. A troca de `5 dias` por esse trecho reduz a especificidade: agora a fração única de quatro cai primeiro na exigência de 14. (`app/ponto/tests/test_ferias_fracao_demais_5.py:137`; `app/ferias/models.py:380`; `app/ferias/models.py:387`) |
| Quatro dias: permanece 14 | **Não:** já havia recusa antes de escrever. (`app/ponto/tests/test_ferias_fracao_demais_5.py:139`) |
| Única 14→5: `assertRaises` | **Sim:** cinco passava no recorte anterior. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`) |
| Única 14→5: mensagem `14 dias` | Não seria alcançada, pois faltaria a exceção. (`app/ponto/tests/test_ferias_fracao_demais_5.py:146`) |
| Única 14→5: permanece 14 | Não seria alcançada; isoladamente falharia, pois seria gravado cinco. (`app/ponto/tests/test_ferias_fracao_demais_5.py:148`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Dias 14/intervalo 15: `assertRaises` | **Sim:** o parâmetro 14 passava e não havia conferência do intervalo. (`app/ponto/tests/test_ferias_fracao_demais_5.py:153`) |
| Divergência: mensagem `intervalo real` | Não seria alcançada, pois faltaria a exceção. (`app/ponto/tests/test_ferias_fracao_demais_5.py:155`) |
| Divergência: `(14, fim14)` preservado | Não seria alcançada; isoladamente falharia pelo fim alterado para dia 15. (`app/ponto/tests/test_ferias_fracao_demais_5.py:157`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Sucesso: dias 15 | **Não:** controle positivo que passa antes e depois. (`app/ponto/tests/test_ferias_fracao_demais_5.py:161`; `app/ponto/tests/test_ferias_fracao_demais_5.py:163`) |
| Correção: ausência de substring | **Não:** permanece inspeção textual do wrapper; não executa correção nem verifica aviso. (`app/ponto/tests/test_ferias_fracao_demais_5.py:169`) |
| Aviso contém §1 | **Não:** esse ramo já existia. (`app/ponto/tests/test_ferias_fracao_demais_5.py:184`; `app/ferias/situacao.py:228`) |
| Aviso contém caput | **Não:** o `if caput` já estava no pai deste delta. O teste melhora a prova comportamental da cura anterior. (`app/ponto/tests/test_ferias_fracao_demais_5.py:185`; `app/ferias/situacao.py:237`) |

**NÃO PROCEDE [bloqueia] — conjunto sem mordida:** há dois casos que distinguem este delta do pai. **PROCEDE [fila] — cobertura incompleta:** o selo administrativo continua textual, e os novos casos de remarcação não exercitam quatro frações nem quatro dias ao lado de uma irmã ≥14; este último existe apenas diretamente sobre `clean()`. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`; `app/ponto/tests/test_ferias_fracao_demais_5.py:153`; `app/ponto/tests/test_ferias_fracao_demais_5.py:169`; `app/ponto/tests/test_ferias_fracao_demais_5.py:128`; `app/ponto/tests/test_ferias_fracao_demais_5.py:67`)

**5. NÃO PROCEDE [bloqueia] — `ValidationError` engolida ou rebatizada pela view.**

As exceções anteriores não são superclasses de `ValidationError`: não remarcável deriva da exceção de domínio; saldo deriva de `Exception`. A captura final entrega `'; '.join(e.messages)`, preservando inclusive mensagens de coerência e colisão. O comentário “juiz do fracionamento” não vira texto de tela. (`app/ferias/services.py:555`; `app/ferias/services.py:576`; `app/ferias/services.py:586`; `app/ferias/views.py:725`; `app/ferias/views.py:727`; `app/ferias/views.py:729`; `app/ferias/views.py:730`)

A precedência efetiva passou a ser **status → início → saldo → `clean()` → colisão no `save()`**. Dentro do `clean()`, uma única fração de quatro dias recebe primeiro a mensagem da exigência de 14; com irmã ≥14, recebe a de cinco. Ambas vêm dos respectivos ramos do juiz. (`app/ferias/services.py:670`; `app/ferias/services.py:671`; `app/ferias/services.py:675`; `app

[truncado em 20000 de 22824 caracteres]

## 14/11 13:47 · sessao 5e3828cd3955 · - · - · origem=Ronald (tmux)

**Censo e recorte removido**

- **NÃO PROCEDE [bloqueia] — perda de julgamento de chamada regular antes coberta.** `remarcar_agendamento` sempre encaminha `so_nao_iniciado=True`; `_trocar_janela` atribui datas/dias e chama `vivo.clean()` antes de qualquer gravação. O `clean` julga a duração pelas datas e exige igualdade com `dias`; uma declaração 1–4 com intervalo ≥5 também termina recusada pela divergência. (`app/ferias/services.py:680`; `app/ferias/services.py:681`; `app/ferias/services.py:686`; `app/ferias/services.py:718`; `app/ferias/models.py:377`; `app/ferias/models.py:387`; `app/ferias/models.py:395`)
- **NÃO PROCEDE [bloqueia] — segundo escritor produtivo encontrado.** O registro permite `ferias/services.py`; o escritor material identificado é `_trocar_janela`, convergência das duas portas. As atribuições em `services.py:821` pertencem a `PeriodoAquisitivo`, não a `AgendamentoFerias`. (`app/core/portas.py:489`; `app/ferias/services.py:680`; `app/ferias/services.py:699`; `app/ferias/services.py:708`; `app/ferias/services.py:718`; `app/ferias/services.py:816`)
- **PROCEDE [fila] — correção administrativa continua permitindo fração <5.** O wrapper usa o padrão `so_nao_iniciado=False`; não chama o `clean`. Isso tem mandato explícito A3: corrigir o lançamento com aviso. A view exige dias positivos, deriva o fim, coleta os avisos e os acrescenta ao sucesso. Não é furo novo. (`app/ferias/services.py:657`; `app/ferias/services.py:681`; `app/ferias/services.py:708`; `app/ferias/views.py:503`; `app/ferias/views.py:507`; `app/ferias/views.py:508`; `app/ferias/views.py:517`; `app/ferias/views.py:552`)

| Caminho | Alteração de existente / tratamento de `<5` |
|---|---|
| `remarcar_agendamento` | Usa o escritor e `clean`; recusa nova fração curta. (`app/ferias/services.py:718`; `app/ferias/services.py:686`; `app/ferias/models.py:387`) |
| `corrigir_datas_agendamento` | Usa o mesmo escritor sem `clean`; aceita curto sujeito a saldo, status e colisão. Aviso é providenciado pela view. (`app/ferias/services.py:708`; `app/ferias/services.py:670`; `app/ferias/services.py:675`; `app/ferias/services.py:699`; `app/ferias/views.py:517`; `app/ferias/views.py:552`) |
| Django admin | Registro local somente de `Afastamento`; nenhuma edição de `AgendamentoFerias`. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`) |
| `sincronizar_status_ferias` | Encaminha para `alinhar_status_por_data`, que salva somente `status`; não reavalia duração curta. (`app/ferias/management/commands/sincronizar_status_ferias.py:48`; `app/ferias/services.py:928`; `app/ferias/services.py:929`) |
| `listar_ferias_dp` | Simula cancelamento e validação; rollback obrigatório. Não remarca. (`app/ferias/management/commands/listar_ferias_dp.py:100`; `app/ferias/management/commands/listar_ferias_dp.py:107`; `app/ferias/management/commands/listar_ferias_dp.py:109`; `app/ferias/management/commands/listar_ferias_dp.py:110`) |
| `alertar_retornos` | Consulta por fim/status e envia alerta; não muda agendamento. (`app/ferias/management/commands/alertar_retornos.py:20`; `app/ferias/management/commands/alertar_retornos.py:24`; `app/ferias/management/commands/alertar_retornos.py:45`) |
| API colaborador | POST 405; GET serializa o que está gravado. (`app/api/views.py:2223`; `app/api/views.py:2224`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| API mensageria | Consulta situação e avisos de pedido hipotético, sem escrita. (`app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`; `app/api/views_mensageria.py:847`; `app/api/views_mensageria.py:849`) |

**Quem escreve e quem lê, para consolidar a tabela da raiz**

| Campo/valor | Escritor/montador | Leitores |
|---|---|---|
| `nova_ini`, `novos_dias`, `nova_fim` | Views leem POST e derivam fim. (`app/ferias/views.py:499`; `app/ferias/views.py:500`; `app/ferias/views.py:507`; `app/ferias/views.py:708`; `app/ferias/views.py:709`; `app/ferias/views.py:718`) | Escritor verifica saldo, atribui e passa a `clean`. (`app/ferias/services.py:676`; `app/ferias/services.py:679`; `app/ferias/services.py:680`; `app/ferias/services.py:686`) |
| `vivo.data_inicio`, `data_fim`, `dias` | `_trocar_janela`. (`app/ferias/services.py:680`; `app/ferias/services.py:699`) | `clean` calcula duração/confere dias; guarda de colisão; espelho; situação; APIs. (`app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/models.py:450`; `app/ponto/services/ausencia.py:1185`; `app/ferias/situacao.py:122`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| `periodo.dias_concedidos` | Troca subtrai dias antigos e soma novos, depois salva. Criação e histórico também somam; cancelamentos subtraem. (`app/ferias/services.py:679`; `app/ferias/services.py:701`; `app/ferias/services.py:548`; `app/ferias/services.py:511`; `app/ferias/services.py:616`; `app/ferias/services.py:643`) | Juiz de saldo e saldo para remarcar; aplicação de status. (`app/ferias/models.py:220`; `app/ferias/models.py:225`; `app/ferias/services.py:700`) |
| `vivo.aviso_emitido` | Derivado da antecedência pelo escritor. (`app/ferias/services.py:687`; `app/ferias/services.py:699`) | View acrescenta aviso de antecedência; API mensageria entrega o flag. (`app/ferias/views.py:733`; `app/api/views_mensageria.py:845`) |
| `vivo.status` | `status_por_data` e escritor; cancelamentos; sincronização. (`app/ferias/services.py:691`; `app/ferias/services.py:699`; `app/ferias/services.py:619`; `app/ferias/services.py:651`; `app/ferias/services.py:928`) | Guardas sob trava, edição, seleção das frações, espelho e APIs. (`app/ferias/services.py:600`; `app/ferias/models.py:344`; `app/ferias/models.py:370`; `app/ponto/services/ausencia.py:1181`; `app/api/views.py:2247`) |
| Frações irmãs (`dias`, `status`, vínculo ao PA, `pk`) | Criações e portas acima; fixtures novas usam `bulk_create`. (`app/ferias/services.py:544`; `app/ferias/services.py:484`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_cancelar_2x.py:247`) | `clean` filtra por PA/status e exclui a própria pk; situação repete a coleta. (`app/ferias/models.py:368`; `app/ferias/models.py:371`; `app/ferias/models.py:378`; `app/ferias/situacao.py:115`; `app/ferias/situacao.py:122`) |
| `caput`, `par1`, `par1_demais` | Helper e constantes 3/14/5. (`app/ferias/models.py:251`; `app/ferias/models.py:252`; `app/ferias/models.py:253`; `app/ferias/models.py:273`; `app/ferias/models.py:275`; `app/ferias/models.py:277`) | `clean` e avisos; não mais wrapper de remarcação. (`app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/models.py:387`; `app/ferias/situacao.py:227`; `app/ferias/situacao.py:348`; `app/ferias/services.py:718`) |

**Classes solicitadas**

- **Relógio fora da borda: PROCEDE [fila], preexistente.** Situação e proposta mantêm fallback `date.today`; cancelamento administrativo lê a data para o carimbo. A alteração usa `hoje` explícito e não acrescenta leitura de relógio. (`app/ferias/situacao.py:156`; `app/ferias/situacao.py:326`; `app/ferias/services.py:649`; `app/ferias/services.py:671`; `app/ferias/services.py:687`; `app/ferias/services.py:691`; `app/ferias/services.py:700`)
- **Derivação paralela: NÃO PROCEDE [bloqueia] quanto ao recorte novo/removido; PROCEDE [fila] quanto à coleta duplicada antiga.** A duração da remarcação agora vem das datas no juiz e é conferida contra `dias`. Permanecem duas consultas montando as frações do período, no modelo e na situação. (`app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/models.py:368`; `app/ferias/situacao.py:117`)
- **Campo com dois significados: NÃO PROCEDE [bloqueia] entre gozo e abono.** Dias vendidos são armazenados separadamente e entram separadamente no saldo. **PROCEDE [fila] divergência dias/datas ainda possível em chamada direta de correção**, que recebe ambos sem conferir equivalência; a view administrativa deriva o fim e evita esse caso. (`app/ferias/services.py:277`; `app/ferias/models.py:220`; `app/ferias/services.py:680`; `app/ferias/services.py:681`; `app/ferias/services.py:708`; `app/ferias/views.py:507`)
- **Save universal sem juiz: PROCEDE [fila], preexistente.** `save()` chama colisão e espelho, não `clean`; histórico explicitamente permite criação curta sem fracionamento. Não é alteração de linha existente nem abertura do delta. (`app/ferias/models.py:461`; `app/ferias/models.py:466`; `app/ferias/services.py:492`; `app/ferias/services.py:510`)

Outros escritores externos ou dinâmicos e efeitos em produção: **[nao verificado]**.

## 14/11 13:47 · sessao 76905d2e0dc4 · - · - · origem=Ronald (tmux)

**PROCEDE [fila] — fixtures passam a representar contadores diferentes da soma dos agendamentos.** F346 mantém `dias_concedidos=10` e acrescenta irmã concluída de 20 à linha de 10; a redução para 5 continua esperando contador 5, embora as linhas somem 25. O teste continua verificando limpeza e atualização incremental, mas não saldo agregado coerente. (`app/ferias/test_remarcar_limpa_janela_f346.py:28`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:80`; `app/ferias/test_remarcar_limpa_janela_f346.py:83`)

O teste da foto velha mantém contador 30 e linha de 30, acrescenta irmã de 20, remarca para 10 e espera contador 10: soma final das linhas 30. A adulteração da foto para 999 continua sendo exercitada; um cenário com irmã registrada e contador reconciliado não está coberto por esse teste. (`app/ferias/test_cancelar_2x.py:51`; `app/ferias/test_cancelar_2x.py:55`; `app/ferias/test_cancelar_2x.py:247`; `app/ferias/test_cancelar_2x.py:251`; `app/ferias/test_cancelar_2x.py:253`)

**NÃO PROCEDE [fila] — irmã esconde o veto do `save` no R06.** Com irmã20 e outro10, permanecem três frações; `clean` não verifica colisão, e o teste ainda pode alcançar o veto no `save`, após a limpeza. As asserções conferem restauração das ausências, contador e início. (`app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:99`; `app/ferias/test_remarcar_limpa_janela_f346.py:105`; `app/ferias/test_remarcar_limpa_janela_f346.py:108`; `app/ferias/test_remarcar_limpa_janela_f346.py:110`; `app/ferias/models.py:363`; `app/ferias/models.py:399`; `app/ferias/services.py:694`; `app/ferias/services.py:699`)

Contrafactual contra 63ff, pelo delta fornecido:

| Asserção | Resultado no pai |
|---|---|
| Quatro dias: `assertRaises` | Passa: a guarda anterior já recusava quatro. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`; delta fornecido, remoção em `remarcar_agendamento`) |
| Quatro dias: contém `art. 134` | Passa: a mensagem anterior também continha esse trecho; a asserção foi ampliada de `5 dias` para `art. 134`. (`app/ponto/tests/test_ferias_fracao_demais_5.py:137`; delta fornecido) |
| Quatro dias: permanece14 | Passa: recusa anterior precedia escrita. (`app/ponto/tests/test_ferias_fracao_demais_5.py:139`; delta fornecido) |
| Única14→5: `assertRaises` | **Falha**: cinco não acionava o recorte anterior. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`; delta fornecido) |
| Única14→5: mensagem14 | Não alcançada após falha do contexto; falta a exceção que conteria a mensagem. (`app/ponto/tests/test_ferias_fracao_demais_5.py:146`) |
| Única14→5: permanece14 | Não alcançada; isoladamente falharia, pois o escritor gravaria5. (`app/ponto/tests/test_ferias_fracao_demais_5.py:148`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Dias14/intervalo15: `assertRaises` | **Falha**: parâmetro14 passa na guarda anterior; sem `clean` o intervalo divergente é gravado. (`app/ponto/tests/test_ferias_fracao_demais_5.py:153`; delta fornecido; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Divergência: mensagem `intervalo real` | Não alcançada após falha do contexto. (`app/ponto/tests/test_ferias_fracao_demais_5.py:155`) |
| Divergência: `(14,fim14)` | Não alcançada; isoladamente falharia por fim15. (`app/ponto/tests/test_ferias_fracao_demais_5.py:157`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Sucesso15: dias15 | Passa antes e depois; controle positivo, sem RED contra pai. (`app/ponto/tests/test_ferias_fracao_demais_5.py:161`; `app/ponto/tests/test_ferias_fracao_demais_5.py:163`) |
| Correção: ausência de substring | Passa antes e depois; não executa correção nem comprova aviso. (`app/ponto/tests/test_ferias_fracao_demais_5.py:165`; `app/ponto/tests/test_ferias_fracao_demais_5.py:169`; `app/ferias/services.py:706`) |
| Aviso: contém §1 | Passa antes e depois: o ramo `par1` já existia e não mudou neste delta. (`app/ponto/tests/test_ferias_fracao_demais_5.py:184`; `app/ferias/situacao.py:228`) |
| Aviso: contém caput | Passa antes e depois: o `if caput` também já existia no pai; o delta melhora o selo, substituindo inspeção textual por comportamento. (`app/ponto/tests/test_ferias_fracao_demais_5.py:185`; `app/ferias/situacao.py:237`; delta fornecido) |

**NÃO PROCEDE [fila] — conjunto novo sem mordida.** Existem dois casos comportamentais que distinguem o delta do pai: única fração reduzida a5 e divergência dias/intervalo. **PROCEDE [fila] — selo administrativo permanece fraco:** verifica apenas substring do wrapper. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`; `app/ponto/tests/test_ferias_fracao_demais_5.py:153`; `app/ponto/tests/test_ferias_fracao_demais_5.py:169`)

**PROCEDE [fila] — não há caso novo de remarcação exercitando `caput` com quatro frações nem fração4 com irmã≥14 para exigir especificamente a segunda metade.** A fixture de remarcação tem uma única14; os casos acrescentados são5, intervalo divergente e15. O caso específico quatro ao lado de14 existe no teste direto de `clean`, sem atravessar o serviço. (`app/ponto/tests/test_ferias_fracao_demais_5.py:67`; `app/ponto/tests/test_ferias_fracao_demais_5.py:128`; `app/ponto/tests/test_ferias_fracao_demais_5.py:141`; `app/ponto/tests/test_ferias_fracao_demais_5.py:150`; `app/ponto/tests/test_ferias_fracao_demais_5.py:159`)

Execução de suíte, RED real contra o pai e efeitos de produção: **[nao verificado]**. [nao verificado]

_5166 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
